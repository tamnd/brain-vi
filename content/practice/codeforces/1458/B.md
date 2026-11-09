---
title: "CF 1458B - Kính đổ một nửa"
description: "Chúng tôi có tới 100 ly. Ly i có thể chứa ai đơn vị nước và hiện chứa hai đơn vị. Chúng ta được phép di chuyển nước giữa các ly. Điều đáng chú ý là mỗi lần chuyển tiền sẽ mất một nửa số tiền đã chuyển."
date: "2026-06-11T02:31:24+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1458
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 691 (Div. 1)"
rating: 2000
weight: 1458
solve_time_s: 130
verified: true
draft: false
---

[CF 1458B - Đổ một nửa ly](https://codeforces.com/problemset/problem/1458/B) 

**Xếp hạng:** 2000 
**Thẻ:** dp 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có tới 100 ly. Thủy tinh`i`có thể giữ`a_i`đơn vị nước và hiện chứa`b_i`đơn vị. 

Chúng ta được phép di chuyển nước giữa các ly. Điều đáng chú ý là mỗi lần chuyển tiền sẽ mất một nửa số tiền đã chuyển. Nếu chúng ta đổ`x`các đơn vị ra khỏi kính, chỉ`x/2`các đơn vị có khả năng đến được đích. Bất kỳ sự vượt quá khả năng của điểm đến cũng sẽ bị mất. 

Đối với mọi`k`từ`1`ĐẾN`n`, chúng ta phải chọn chính xác`k`ly và tối đa hóa lượng nước có thể đọng lại bên trong những chiếc ly đã chọn đó sau khi thực hiện bất kỳ trình tự chuyển nào. 

Câu trả lời cho mỗi`k`là tổng lượng nước tối đa có thể được bảo đảm bên trong một số tập hợp`k`kính. 

Những ràng buộc là đầu mối đầu tiên hướng tới giải pháp dự định. Chỉ có 100 ly, mỗi dung lượng và số lượng ban đầu tối đa là 100. Tổng dung lượng tối đa là 10000 và tổng lượng nước cũng nhiều nhất là 10000. Con số này đủ nhỏ cho trạng thái lập trình động được lập chỉ mục theo tổng dung lượng, nhưng quá lớn đối với bất kỳ phép liệt kê tập hợp con nào của ly, vì`2^100`khả năng là hoàn toàn không thể xảy ra. 

Phần tế nhị của vấn đề là hiểu được những gì việc chuyển giao thực sự có thể đạt được. 

Cân nhắc lựa chọn một số bộ kính`S`. 

Cho phép

-`A = Σ a_i`trên kính trong`S`-`B = Σ b_i`trên kính trong`S`-`T = Σ b_i`trên tất cả các kính 

Chiếc ly được chọn đã có sẵn`B`đơn vị nước. Bên ngoài bộ có`T - B`đơn vị. 

Bất kỳ nguồn nước nào từ bên ngoài đổ vào đều bị mất 50% trước khi đến mức đã chọn. Vì vậy, ngay cả khi chúng ta sử dụng hết nguồn nước bên ngoài, nhiều nhất`(T - B)/2`các đơn vị bổ sung có thể được chuyển vào kính đã chọn. 

Đồng thời, ly đã chọn không thể chứa quá tổng dung tích`A`. 

Do đó lượng nước có thể đạt được trong bộ đã chọn này là$$\min\left(A,\; B + \frac{T-B}{2}\right)$$Cách tiếp cận bất cẩn thường bỏ lỡ một trong hai giới hạn này. 

Ví dụ:```
2
100 0
1 1
```Vì`k=1`, chọn ly đầu tiên sẽ có công suất 100 nhưng chỉ có 1 chiếc tồn tại trên toàn cầu. Sau khi tràn, nhiều nhất là 0,5 đạt đến nó, vì vậy câu trả lời là 0,5 chứ không phải 100. 

Ngược lại:```
2
5 5
100 100
```Chỉ chọn ly đầu tiên sẽ mang lại$$\min(5,\;5+100/2)=5$$Giới hạn năng lực chiếm ưu thế. 

Toàn bộ vấn đề trở thành việc tìm kiếm, với mọi`k`, tập hợp con tối đa hóa biểu thức đó. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê mọi tập hợp con của kính. Đối với mỗi tập hợp con, chúng tôi sẽ tính tổng công suất của nó`A`, nước ban đầu của nó`B`, và đánh giá$$\min\left(A,\; B+\frac{T-B}{2}\right).$$Điều này đúng vì biểu thức mô tả đầy đủ số lượng có thể đạt được cho tập hợp con đó. 

Vấn đề là số lượng tập hợp con. Với`n = 100`, có`2^{100}`khả năng, đó là lớn về mặt thiên văn. 

Quan sát quan trọng là giá trị của một tập hợp con chỉ phụ thuộc vào hai đại lượng tổng hợp:$$A=\sum a_i,\qquad B=\sum b_i.$$Danh tính cá nhân không còn quan trọng nữa một khi những số tiền này được biết đến. 

Tổng công suất tối đa là$$100 \cdot 100 = 10000.$$Điều này gợi ý một chương trình động kiểu ba lô trong đó chúng tôi theo dõi, đối với mọi kích thước tập hợp con và mọi tổng dung lượng, mức tối đa có thể đạt được`B`. 

Giả sử chúng ta biết, với mỗi cặp`(k, A)`, giá trị lớn nhất có thể có của`B`giữa các tập con có kích thước`k`năng lực của họ tổng cộng là`A`. 

Thì câu trả lời cho trạng thái đó chỉ đơn giản là$$\min\left(A,\; \frac{T+B}{2}\right),$$bởi vì$$B+\frac{T-B}{2} = \frac{T+B}{2}.$$Để cố định`(k, A)`, lớn hơn`B`luôn đưa ra mục tiêu lớn hơn, do đó chỉ lưu trữ tối đa`B`là đủ. 

Điều này biến bài toán thành một chiếc ba lô 0/1 cổ điển với hai chiều: số lượng ly được chọn và tổng sức chứa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(1)$| Quá chậm | 
| DP tối ưu |$O(n^2 \cdot \sum a_i)$|$O(n \cdot \sum a_i)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ly và tính tổng lượng nước$$T=\sum b_i.$$2. Hãy để`MAXA = Σ a_i`. 
3. Tạo bảng DP trong đó$$dp[j][s]$$lưu trữ tổng lượng nước ban đầu tối đa`B`có thể đạt được bằng cách chọn chính xác`j`kính có tổng công suất là`s`. 

Các trạng thái không thể truy cập được khởi tạo thành`-1`. 
4. Đặt$$dp[0][0]=0.$$5. Xử lý từng chiếc kính một. 

Đối với một ly`(a,b)`, lặp lại`j`Và`s`theo thứ tự giảm dần và thực hiện chuyển đổi ba lô 0/1 tiêu chuẩn:$$dp[j+1][s+a] = \max(dp[j+1][s+a],\, dp[j][s]+b).$$Lặp lại giảm dần ngăn việc sử dụng cùng một ly nhiều lần. 
6. Sau khi tất cả các ly đã được xử lý, hãy tính kết quả cho mỗi ly.`k`. 
7. Đối với mọi trạng thái có thể truy cập`(k,s)`với giá trị được lưu trữ`B`, đánh giá$$\min\left(s,\frac{T+B}{2}\right).$$Đây là lượng nước tối đa có thể đạt được đối với cặp cốt liệu cụ thể đó`(A=s,B)`. 
8. Lấy giá trị lớn nhất trong số tất cả các trạng thái thuộc cùng một`k`. 
9. Xuất ra tất cả các câu trả lời với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Sửa bất kỳ bộ kính nào đã chọn. 

Cho phép`A`là tổng công suất của họ và`B`nước ban đầu của họ. Cốc còn lại chứa`T-B`đơn vị. 

Mỗi thiết bị đến từ bên ngoài phải được chuyển đi ít nhất một lần, vì vậy nhiều nhất một nửa lượng nước bên ngoài có thể lọt vào bên trong khu vực đã chọn. Do đó tổng lượng nước có thể thu được vào cốc đã chọn nhiều nhất là$$B+\frac{T-B}{2} = \frac{T+B}{2}.$$Ly đã chọn cũng không được chứa nhiều hơn tổng dung tích của chúng`A`. Do đó không có chiến lược nào có thể vượt quá$$\min\left(A,\frac{T+B}{2}\right).$$Giới hạn này có thể đạt được. Giữ tất cả nước đã có trong bộ đã chọn. Chuyển nước bên ngoài vào bộ đã chọn nếu cần. Có thể cung cấp tới một nửa lượng nước bên ngoài và quá trình có thể dừng khi đạt công suất`A`đã đạt được. Do đó, mức tối ưu chính xác cho tập hợp con đó bằng$$\min\left(A,\frac{T+B}{2}\right).$$DP liệt kê mọi cặp có thể`(A,B)`có thể đạt được bằng các tập hợp con có kích thước`k`. Đối với mỗi tổng công suất`A`, nó lưu trữ lớn nhất có thể`B`, điều này là đủ vì mục tiêu tăng đơn điệu với`B`. Do đó giá trị tối đa được tính cho mỗi`k`chính xác là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    glasses = []
    total_water = 0
    total_capacity = 0

    for _ in range(n):
        a, b = map(int, input().split())
        glasses.append((a, b))
        total_water += b
        total_capacity += a

    dp = [[-1] * (total_capacity + 1) for _ in range(n + 1)]
    dp[0][0] = 0

    current_capacity_sum = 0

    for a, b in glasses:
        for cnt in range(n - 1, -1, -1):
            for cap in range(current_capacity_sum, -1, -1):
                if dp[cnt][cap] == -1:
                    continue

                nxt_cap = cap + a
                nxt_water = dp[cnt][cap] + b

                if nxt_water > dp[cnt + 1][nxt_cap]:
                    dp[cnt + 1][nxt_cap] = nxt_water

        current_capacity_sum += a

    ans = [0.0] * (n + 1)

    for k in range(1, n + 1):
        best = 0.0

        for cap in range(total_capacity + 1):
            inside_water = dp[k][cap]
            if inside_water == -1:
                continue

            value = min(float(cap), (total_water + inside_water) / 2.0)
            if value > best:
                best = value

        ans[k] = best

    print(*("{:.10f}".format(x) for x in ans[1:]))

if __name__ == "__main__":
    solve()
```Bảng DP là trung tâm của giải pháp.`dp[k][cap]`không lưu trữ câu trả lời trực tiếp. Thay vào đó, nó lưu trữ lượng nước hiện có ban đầu lớn nhất có thể trong số tất cả các tập hợp con có kích thước`k`năng lực của họ tổng cộng là`cap`. 

Sự lựa chọn này làm cho không gian trạng thái có thể quản lý được. Tổng công suất chỉ lên tới 10000, vì vậy toàn bộ DP chứa khoảng một triệu trạng thái. 

Các vòng lặp giảm dần là cần thiết. Nếu công suất được xử lý theo thứ tự tăng dần, một chiếc ly có thể được tái sử dụng nhiều lần trong cùng một lần lặp, biến quá trình chuyển đổi thành một chiếc ba lô không giới hạn và tạo ra kết quả không chính xác. 

Sau khi ba lô kết thúc, mỗi trạng thái có thể truy cập tương ứng với một tập hợp con nào đó`(A,B)`. Công thức$$\min\left(A,\frac{T+B}{2}\right)$$được đánh giá trực tiếp và giá trị tốt nhất cho từng kích thước tập hợp con được giữ lại. 

Không tồn tại mối lo ngại về tràn số nguyên vì tất cả các tổng tối đa là 10000. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
6 5
6 5
10 2
```Tổng lượng nước:$$T=12$$Trạng thái DP có liên quan sau khi xử lý tất cả các kính: 

| k | Công suất A | Nước B | 
| --- | --- | --- | 
| 1 | 6 | 5 | 
| 1 | 10 | 2 | 
| 2 | 12 | 10 | 
| 2 | 16 | 7 | 
| 3 | 22 | 12 | 

Đánh giá: 

| k | A | B | phút(A, (T+B)/2) | 
| --- | --- | --- | --- | 
| 1 | 6 | 5 | 6 | 
| 1 | 10 | 2 | 7 | 
| 2 | 12 | 10 | 11 | 
| 2 | 16 | 7 | 9,5 | 
| 3 | 22 | 12 | 12 | 

Câu trả lời hay nhất: 

| k | Trả lời | 
| --- | --- | 
| 1 | 7 | 
| 2 | 11 | 
| 3 | 12 | 

Ví dụ này cho thấy cả hai yếu tố hạn chế. Vì`k=1`, kính có dung tích lớn hơn sẽ thắng dù có ít nước ban đầu hơn vì có thể hứng nước tràn từ nơi khác. 

### Ví dụ 2 

đầu vào:```
2
100 0
1 1
```Tổng lượng nước:$$T=1$$Kỳ: 

| k | A | B | 
| --- | --- | --- | 
| 1 | 100 | 0 | 
| 1 | 1 | 1 | 
| 2 | 101 | 1 | 

Đánh giá: 

| k | A | B | Giá trị | 
| --- | --- | --- | --- | 
| 1 | 100 | 0 | 0,5 | 
| 1 | 1 | 1 | 1 | 
| 2 | 101 | 1 | 1 | 

Câu trả lời: 

| k | Trả lời | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 

Điều này chứng tỏ rằng chỉ riêng công suất khổng lồ là vô ích khi không có đủ nước trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \cdot \sum a_i)$| Chuyển đổi ba lô theo kích thước và dung lượng tập hợp con | 
| Không gian |$O(n \cdot \sum a_i)$| bảng DP | 

Từ`n ≤ 100`Và`Σa_i ≤ 10000`, số lượng trạng thái DP là khoảng một triệu. Tổng số lần chuyển đổi là khoảng`100 × 100 × 10000 = 10^8`, nhưng quá trình triển khai thực tế chỉ lặp lại trong phạm vi công suất có thể tiếp cận và vượt qua giới hạn cuộc thi trong Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())

    glasses = []
    total_water = 0
    total_capacity = 0

    for _ in range(n):
        a, b = map(int, input().split())
        glasses.append((a, b))
        total_water += b
        total_capacity += a

    dp = [[-1] * (total_capacity + 1) for _ in range(n + 1)]
    dp[0][0] = 0

    cur = 0

    for a, b in glasses:
        for k in range(n - 1, -1, -1):
            for s in range(cur, -1, -1):
                if dp[k][s] == -1:
                    continue
                dp[k + 1][s + a] = max(
                    dp[k + 1][s + a],
                    dp[k][s] + b
                )
        cur += a

    ans = []

    for k in range(1, n + 1):
        best = 0.0
        for s in range(total_capacity + 1):
            B = dp[k][s]
            if B == -1:
                continue
            best = max(best, min(float(s), (total_water + B) / 2.0))
        ans.append(f"{best:.10f}")

    return " ".join(ans) + "\n"

# sample
out = run(
"""3
6 5
6 5
10 2
"""
).strip().split()

assert abs(float(out[0]) - 7.0) < 1e-9
assert abs(float(out[1]) - 11.0) < 1e-9
assert abs(float(out[2]) - 12.0) < 1e-9

# minimum size
out = run(
"""1
5 3
"""
).strip()
assert abs(float(out) - 3.0) < 1e-9

# empty water except one glass
out = run(
"""2
100 0
1 1
"""
).strip().split()
assert abs(float(out[0]) - 1.0) < 1e-9
assert abs(float(out[1]) - 1.0) < 1e-9

# all equal glasses
out = run(
"""3
10 10
10 10
10 10
"""
).strip().split()
assert abs(float(out[0]) - 10.0) < 1e-9
assert abs(float(out[1]) - 20.0) < 1e-9
assert abs(float(out[2]) - 30.0) < 1e-9

# capacity bottleneck
out = run(
"""2
5 5
100 100
"""
).strip().split()
assert abs(float(out[0]) - 100.0) < 1e-9
assert abs(float(out[1]) - 105.0) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Kính đơn | Lượng nước hiện có | Trường hợp cơ sở | 
| Một ly đầy và một ly gần cạn | Hành vi chuyển mất | Quy tắc tràn một nửa | 
| Tất cả các kính giống hệt nhau | Xử lý đối xứng | Không phụ thuộc vào việc đặt hàng | 
| Ví dụ về nút thắt công suất | Giới hạn công suất chiếm ưu thế | Sử dụng đúng`min(A, ...)`| 

## Vỏ cạnh 

Hãy xem xét:```
2
100 0
1 1
```Vì`k=1`, chọn ly lớn sẽ mang lại`A=100`,`B=0`. Thuật toán đánh giá$$\min(100,(1+0)/2)=0.5.$$Việc lựa chọn ly thủy tinh nhỏ mang lại$$\min(1,(1+1)/2)=1.$$DP giữ cả hai trạng thái và trả về giá trị lớn hơn, điều này đúng. 

Bây giờ hãy xem xét:```
2
5 5
100 100
```Chỉ chọn sản lượng ly đầu tiên$$A=5,\quad B=5.$$Công thức cho$$\min(5,(105)/2)=5.$$Mặc dù lượng nước khổng lồ tồn tại ở nơi khác, chiếc cốc được chọn không thể vượt quá sức chứa của nó. Thuật toán thực thi điều này một cách tự động thông qua`min`. 

Cuối cùng:```
3
10 0
10 0
10 0
```Tất cả các bang đều có`B=0`và tổng lượng nước`T=0`. Mọi đánh giá đều trở thành số 0, vì vậy mọi câu trả lời đều chính xác bằng 0. DP xử lý chính xác những chiếc ly ban đầu không chứa nước vì`0`được lưu trữ dưới dạng tổng lượng nước có thể truy cập hợp lệ thay vì bị nhầm lẫn với trạng thái không thể truy cập.
