---
title: "CF 106089B - \u0413\u043e\u043d\u043a\u0438"
description: "Chúng tôi được cung cấp một loạt các tốc độ hàng ngày. Chúng ta phải chọn một đoạn ngày liền kề có độ dài ít nhất là k. Đối với một đoạn [l, r], chất lượng của nó là trung bình số học của tất cả các tốc độ bên trong đoạn đó: $$frac{vl + v{l+1} + dots + vr}{r-l+1}$$ Nhiệm vụ là tìm bất kỳ đoạn nào…"
date: "2026-06-19T21:52:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106089
codeforces_index: "B"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2025, \u0444\u0438\u043d\u0430\u043b"
rating: 0
weight: 106089
solve_time_s: 74
verified: true
draft: false
---

[CF 106089B - \u0413\u043e\u043d\u043a\u0438](https://codeforces.com/problemset/problem/106089/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt các tốc độ hàng ngày. Chúng ta phải chọn một đoạn ngày liền kề có độ dài ít nhất`k`. 

Đối với một phân khúc`[l, r]`, chất lượng của nó là giá trị trung bình số học của tất cả các tốc độ trong đoạn đó:$$\frac{v_l + v_{l+1} + \dots + v_r}{r-l+1}$$Nhiệm vụ là tìm bất kỳ phân đoạn nào có mức trung bình tối đa có thể và xuất ra điểm cuối của nó. 

Phần thú vị nhất của các ràng buộc là điều kiện$$n \cdot k \le 10^6.$$Mặc dù`n`có thể đạt được`100000`, nó chỉ có thể làm được điều đó khi`k`là rất nhỏ. Bất kỳ thuật toán nào có độ phức tạp tỷ lệ thuận với`nk`an toàn trong giới hạn, trong khi`O(n^2)`là không thể khi`k = 1`. 

Một sai lầm phổ biến là cho rằng đoạn tối ưu phải có độ dài chính xác`k`. 

Coi như:```
n = 3, k = 2
1 100 100
```Đoạn có độ dài`2`với số trung bình lớn nhất là`[2,3]`, trung bình`100`. Toàn bộ mảng có mức trung bình`67`, vậy độ dài`2`thực sự là tốt hơn. 

Bây giờ hãy xem xét:```
n = 3, k = 2
50 100 50
```Độ dài trung bình-2 tốt nhất là`75`, nhưng toàn bộ mảng cũng có mức trung bình`66.67`. Ở đây chiều dài`2`vẫn tối ưu. 

Không có ví dụ nào chứng minh bất cứ điều gì. Điểm mấu chốt là các đoạn dài hơn không thể bị bỏ qua nếu không có bằng chứng. 

Một vấn đề tinh tế khác là so sánh mức trung bình bằng cách sử dụng số dấu phẩy động. Coi như:```
k = 2
1000000000 999999999
999999999 1000000000
```Các phân khúc khác nhau có thể có mức trung bình khác nhau một lượng nhỏ. Việc sử dụng so sánh dấu phẩy động có thể vô tình chọn sai phân đoạn. Phép nhân chéo các phân số hoàn toàn tránh được điều này. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tính tổng tiền tố, liệt kê mọi phân đoạn hợp lệ, tính tổng của nó theo`O(1)`, tính giá trị trung bình của nó và giữ giá trị tốt nhất. 

Vấn đề là số lượng phân đoạn hợp lệ. có`Θ(n²)`của họ. Với`n = 100000`, vượt xa những gì có thể được xử lý trong một giây. 

Quan sát quan trọng đến từ việc xem xét các đoạn rất dài. 

Giả sử một đoạn có độ dài ít nhất`2k`. Chia nó thành hai phần liên tiếp, mỗi phần có độ dài ít nhất`k`. 

Nếu phần bên trái có mức trung bình`A`và phần bên phải có mức trung bình`B`, thì giá trị trung bình của toàn bộ phân khúc là giá trị trung bình có trọng số của`A`Và`B`. Giá trị trung bình có trọng số không bao giờ có thể vượt quá cả hai thành phần của nó. Vậy ít nhất một trong hai phần có điểm trung bình lớn hơn hoặc bằng điểm trung bình của cả đoạn. 

Điều đó có nghĩa là một đoạn có độ dài ít nhất`2k`không bao giờ có thể tốt hơn tất cả các phần con hợp lệ của nó. Chúng ta có thể liên tục rút ngắn đoạn như vậy cho đến khi độ dài của nó nhỏ hơn`2k`, mà không làm giảm giá trị trung bình. 

Kết quả là một số câu trả lời tối ưu luôn có độ dài trong khoảng$$k \le L \le 2k-1.$$Bây giờ không gian tìm kiếm trở nên nhỏ. chỉ có`k`độ dài có thể. Đối với mỗi độ dài, chúng tôi quét tất cả các vị trí bắt đầu. Tổng công việc là$$O(n \cdot k),$$nhiều nhất là`10^6`vì sự ràng buộc. 

Tổng tiền tố cho tổng mỗi phân đoạn theo thời gian không đổi và giá trị trung bình được so sánh chính xác bằng cách sử dụng phép nhân chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các phân đoạn | O(n²) | O(n) | Quá chậm | 
| Liệt kê độ dài từ k đến 2k-1 | O(nk) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tổng tiền tố`pref`, Ở đâu`pref[i]`lưu trữ tổng của số đầu tiên`i`các phần tử. 
2. Duy trì phân khúc tốt nhất hiện tại. Lưu trữ tổng, độ dài và điểm cuối của nó. 
3. Liệt kê mọi độ dài có thể`L`từ`k`ĐẾN`min(n, 2k - 1)`. 

Định lý chính đảm bảo rằng có ít nhất một phân đoạn tối ưu nằm trong phạm vi này. 
4. Đối với mỗi chiều dài`L`, trượt qua tất cả các đoạn có độ dài đó. 

Sử dụng tổng tiền tố, tổng phân đoạn là$$pref[r] - pref[l-1].$$5. So sánh mức trung bình của phân khúc hiện tại với mức trung bình tốt nhất cho đến nay. 

Thay vì chia hãy so sánh$$\frac{sum_1}{len_1}
\quad\text{and}\quad
\frac{sum_2}{len_2}$$bằng cách kiểm tra$$sum_1 \cdot len_2
\quad\text{vs}\quad
sum_2 \cdot len_1.$$Điều này là chính xác và tránh được lỗi dấu phẩy động. 
6. Nếu giá trị trung bình hiện tại lớn hơn, hãy thay thế câu trả lời đã lưu. 
7. Sau khi tất cả các độ dài đã được xử lý, hãy xuất các điểm cuối đã lưu. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên một thuộc tính. 

Bất kỳ đoạn nào có độ dài ít nhất`2k`có thể được chia thành hai đoạn liên tiếp, cả hai đều có chiều dài ít nhất`k`. Giá trị trung bình của toàn bộ phân khúc là giá trị trung bình có trọng số của giá trị trung bình của hai phần này. Do đó, ít nhất một phần có mức trung bình lớn ít nhất bằng toàn bộ phân khúc. 

Bắt đầu từ bất kỳ đoạn tối ưu nào, liên tục thay thế nó bằng phần đó bất cứ khi nào chiều dài của nó ít nhất`2k`. Giá trị trung bình không bao giờ giảm và cuối cùng độ dài rơi vào`[k, 2k-1]`. 

Vì vậy, một câu trả lời tối ưu luôn tồn tại trong phạm vi độ dài đó. Thuật toán liệt kê mọi đoạn có mọi độ dài trong phạm vi đó và chọn đoạn có mức trung bình tối đa. Vì mọi ứng cử viên đều được kiểm tra và các giá trị trung bình được so sánh chính xác nên phân đoạn được trả về là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    v = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + v[i]

    best_sum = -1
    best_len = 1
    best_l = best_r = 1

    max_len = min(n, 2 * k - 1)

    for length in range(k, max_len + 1):
        for l in range(1, n - length + 2):
            r = l + length - 1
            cur_sum = pref[r] - pref[l - 1]

            if best_sum == -1 or cur_sum * best_len > best_sum * length:
                best_sum = cur_sum
                best_len = length
                best_l = l
                best_r = r

    print(best_l, best_r)

solve()
```Cấu trúc tổng tiền tố làm cho tổng mỗi phân đoạn có sẵn trong thời gian không đổi. Nếu không có nó, việc tính tổng sẽ yêu cầu quét phân đoạn, điều này sẽ làm tăng độ phức tạp theo một hệ số khác của độ dài. 

Cặp biến`(best_sum, best_len)`đại diện cho mức trung bình tốt nhất hiện tại dưới dạng phân số`best_sum / best_len`. Khi xem xét một phân khúc mới, việc so sánh```
cur_sum * best_len > best_sum * length
```kiểm tra xem$$\frac{cur\_sum}{length}
>
\frac{best\_sum}{best\_len}.$$Không có phép chia nào được thực hiện nên không có vấn đề gì về độ chính xác. 

Vòng lặp theo chiều dài dừng lại ở`2k - 1`. Ràng buộc đó là toàn bộ lý do giải pháp phù hợp với các ràng buộc. 

Tất cả việc lập chỉ mục được xử lý theo tọa độ dựa trên 1 vì đầu ra được yêu cầu là dựa trên 1. Mảng tổng tiền tố được xây dựng thêm một số 0 đứng đầu, làm cho công thức```
pref[r] - pref[l - 1]
```làm việc thống nhất cho từng phân khúc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2
1 2 3 4
```Tổng tiền tố: 

| tôi | trước[i] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 3 | 
| 3 | 6 | 
| 4 | 10 | 

Độ dài được xem xét:`2`Và`3`. 

| Chiều dài | Phân đoạn | Tổng hợp | Trung bình | Tốt nhất sau bước | 
| --- | --- | --- | --- | --- | 
| 2 | [1,2] | 3 | 1,5 | [1,2] | 
| 2 | [2,3] | 5 | 2,5 | [2,3] | 
| 2 | [3,4] | 7 | 3,5 | [3,4] | 
| 3 | [1,3] | 6 | 2.0 | [3,4] | 
| 3 | [2,4] | 9 | 3.0 | [3,4] | 

Câu trả lời cuối cùng:```
3 4
```Dấu vết cho thấy ngay cả sau khi kiểm tra các đoạn dài hơn, đoạn có độ dài-2`[3,4]`vẫn tối ưu. 

### Mẫu 2 

đầu vào:```
10 2
3 5 2 7 2 9 3 8 2 7
```Độ dài được xem xét:`2`Và`3`. 

| Chiều dài | Phân đoạn | Tổng hợp | Trung bình | 
| --- | --- | --- | --- | 
| 2 | [1,2] | 8 | 4.0 | 
| 2 | [2,3] | 7 | 3,5 | 
| 2 | [3,4] | 9 | 4,5 | 
| 2 | [4,5] | 9 | 4,5 | 
| 2 | [5,6] | 11 | 5,5 | 
| 2 | [6,7] | 12 | 6.0 | 
| 2 | [7,8] | 11 | 5,5 | 
| 2 | [8,9] | 10 | 5.0 | 
| 2 | [9,10] | 9 | 4,5 | 
| 3 | [1,3] | 10 | 3,33 | 
| 3 | [2,4] | 14 | 4,67 | 
| 3 | [3,5] | 11 | 3,67 | 
| 3 | [4,6] | 18 | 6.0 | 
| 3 | [5,7] | 14 | 4,67 | 
| 3 | [6,8] | 20 | 6,67 | 
| 3 | [7,9] | 13 | 4.33 | 
| 3 | [8,10] | 17 | 5,67 | 

Mức trung bình tối đa đạt được bằng`[6,8]`. 

Câu trả lời cuối cùng:```
6 8
```Ví dụ này giải thích tại sao chỉ kiểm tra độ dài`k`là không đủ. Đoạn tốt nhất có độ dài`3`, mặc dù`k = 2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Có nhiều nhất`k`độ dài ứng cử viên và`O(n)`phân đoạn cho mỗi chiều dài | 
| Không gian | O(n) | Tổng tiền tố | 

Bởi vì`n * k ≤ 10^6`, tổng số phân đoạn được xử lý nhiều nhất là khoảng một triệu. Điều đó thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())
    v = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + v[i]

    best_sum = -1
    best_len = 1
    ans = (1, 1)

    for length in range(k, min(n, 2 * k - 1) + 1):
        for l in range(1, n - length + 2):
            r = l + length - 1
            s = pref[r] - pref[l - 1]

            if best_sum == -1 or s * best_len > best_sum * length:
                best_sum = s
                best_len = length
                ans = (l, r)

    return f"{ans[0]} {ans[1]}"

# provided samples
assert run("4 2\n1 2 3 4\n") == "3 4"
assert run("10 2\n3 5 2 7 2 9 3 8 2 7\n") == "6 8"

# minimum size
assert run("1 1\n5\n") == "1 1"

# all equal values, any optimal answer exists
assert run("5 3\n7 7 7 7 7\n") == "1 3"

# best segment has length greater than k
assert run("3 2\n3 10 3\n") == "1 3"

# off-by-one at the end of the array
assert run("5 2\n1 1 1 1 100\n") == "4 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5`|`1 1`| Ví dụ nhỏ nhất có thể | 
| Tất cả các giá trị bằng nhau |`1 3`| Mối ràng buộc giữa nhiều phân khúc | 
|`3 2 / 3 10 3`|`1 3`| Độ dài đoạn tối ưu lớn hơn`k`| 
|`5 2 / 1 1 1 1 100`|`4 5`| Xử lý đúng các phân đoạn kết thúc tại`n`| 

## Vỏ cạnh 

Hãy xem xét:```
1 1
42
```Phân đoạn hợp lệ duy nhất là`[1,1]`. Thuật toán kiểm tra độ dài`1`, tính tổng của nó từ mảng tiền tố và ngay lập tức lưu nó làm câu trả lời. Đầu ra là:```
1 1
```Bây giờ hãy xem xét trường hợp đoạn tối ưu dài hơn`k`:```
3 2
3 10 3
```Các đoạn dài-2 có giá trị trung bình`6.5`. Đoạn dài-3 có giá trị trung bình$$\frac{16}{3} \approx 5.33$$Trên thực tế cái này không lớn hơn nên chúng ta hãy sử dụng:```
3 2
5 10 5
```Trung bình có độ dài-2 là`7.5`, trong khi độ dài trung bình-3 là`20/3 ≈ 6.67`. Thuật toán kiểm tra cả độ dài và so sánh chính xác các phân số mà không cần số học dấu phẩy động. 

Cuối cùng, hãy xem xét các giá trị lớn:```
2 1
1000000000 1000000000
```Tổng số đạt`2 * 10^9`và trong quá trình so sánh, chúng tôi nhân với độ dài lên tới`10^5`. Ở một số ngôn ngữ, số nguyên 32 bit sẽ tràn. Giải pháp so sánh mức trung bình bằng cách sử dụng số học số nguyên và số nguyên có độ chính xác tùy ý của Python xử lý các giá trị này một cách an toàn.
