---
title: "CF 2193C - Thay thế và tính tổng"
description: "Chúng ta bắt đầu với hai mảng có cùng độ dài. Đối với bất kỳ vị trí nào, chúng tôi có thể ghi đè trực tiếp a[i] bằng b[i]. Chúng ta cũng có thể sao chép các giá trị từ phải sang trái bằng cách thay thế a[i] bằng giá trị hiện tại của a[i+1]. Mỗi truy vấn yêu cầu tổng lớn nhất có thể có trên một mảng con a[l.."
date: "2026-06-07T20:50:05+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2193
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1076 (Div. 3)"
rating: 1000
weight: 2193
solve_time_s: 172
verified: true
draft: false
---

[CF 2193C - Thay thế và tính tổng](https://codeforces.com/problemset/problem/2193/C) 

**Đánh giá:** 1000 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai mảng có cùng độ dài. 

Đối với bất kỳ vị trí nào, chúng tôi có thể ghi đè trực tiếp`a[i]`với`b[i]`. Chúng ta cũng có thể sao chép các giá trị từ phải sang trái bằng cách thay thế`a[i]`với giá trị hiện tại là`a[i+1]`. 

Mỗi truy vấn yêu cầu tổng lớn nhất có thể có trên một mảng con`a[l..r]`sau khi thực hiện bất kỳ số lượng các hoạt động này. 

Chi tiết quan trọng là mọi truy vấn đều độc lập. Chúng tôi không bắt buộc phải giữ lại các sửa đổi được thực hiện cho một truy vấn khi trả lời một truy vấn khác. 

Các ràng buộc ngay lập tức loại trừ mọi cách tiếp cận mô phỏng các hoạt động cho từng truy vấn. Trên tất cả các trường hợp thử nghiệm, cả tổng kích thước mảng và tổng số truy vấn đều có thể đạt tới`2 * 10^5`. MỘT`O(nq)`giải pháp có thể yêu cầu khoảng`4 * 10^10`hoạt động, vượt xa những gì phù hợp với giới hạn 2 giây. Chúng tôi cần tiền xử lý cho phép mỗi truy vấn được trả lời theo thời gian không đổi hoặc logarit. 

Phần tinh tế nhất của vấn đề là hiểu những giá trị nào cuối cùng có thể xuất hiện ở một vị trí. 

Coi như:```
a = [3, 2, 1]
b = [1, 2, 3]
```Vị trí 1 cuối cùng có thể trở thành`3`. Chúng tôi đặt vị trí 3 thành`b[3]=3`, sao chép nó vào vị trí 2, sau đó sao chép vị trí 2 sang vị trí 1. 

Một sai lầm phổ biến là nghĩ rằng vị trí`i`chỉ có thể lựa chọn giữa`a[i]`Và`b[i]`. Ở đây vị trí 1 đạt giá trị bắt nguồn từ vị trí 3. 

Một sai lầm dễ mắc phải khác là cho rằng giá trị tốt nhất cho vị trí`i`là phần tử tối đa ở bên phải của nó. 

Coi như:```
a = [10]
b = [1]
```Không có người hàng xóm phù hợp nào cả. Giá trị tối đa chính xác ở vị trí 1 vẫn là`10`, bởi vì chúng ta có thể đơn giản để nó không thay đổi. 

Cái bẫy thứ ba là bỏ qua rằng các giá trị được sao chép có thể được tạo ra bằng cách sử dụng`b`. 

Coi như:```
a = [1, 1]
b = [1, 100]
```Vị trí 1 có thể trở thành`100`bằng cách thiết lập vị trí đầu tiên 2 để`100`rồi sao chép sang trái. Mọi lý do chỉ dựa trên mảng ban đầu`a`bỏ lỡ khả năng này. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là trả lời từng truy vấn một cách độc lập. Người ta có thể cố gắng xác định chuỗi hoạt động tốt nhất và tính tổng số tiền tối đa có thể đạt được trên phân đoạn được yêu cầu. 

Điều này hoạt động về mặt khái niệm vì các hoạt động rất đơn giản và cục bộ. Vấn đề là ngay cả việc mô tả không gian của các trạng thái có thể cũng là rất lớn. Với tối đa`2 * 10^5`vị trí và`2 * 10^5`truy vấn, mọi mô phỏng cho mỗi truy vấn đều chậm một cách vô vọng. 

Sự đột phá đến từ việc nghiên cứu một quan điểm duy nhất. 

Lấy một số chỉ mục`i`. Mọi thao tác sao chép đều di chuyển thông tin từ phải sang trái. Do đó, bất kỳ giá trị nào cuối cùng xuất hiện ở vị trí`i`phải bắt nguồn từ một vị trí nào đó`j ≥ i`. 

Tại vị trí`j`, giá trị lớn nhất chúng ta có thể cung cấp là:```
max(a[j], b[j])
```vì chúng ta có thể giữ`a[j]`hoặc ghi đè lên nó bằng`b[j]`. 

Định nghĩa:```
t[j] = max(a[j], b[j])
```Khi đó giá trị lớn nhất có thể đạt đến vị trí`i`là:```
s[i] = max(t[j]) for all j ≥ i
```mà chỉ đơn giản là một hậu tố tối đa. 

Không có giá trị cuối cùng tại vị trí`i`có thể vượt quá`s[i]`, bởi vì mọi nguồn có thể truy cập đều nằm trong hậu tố bắt đầu từ`i`. 

Thậm chí tốt hơn, toàn bộ mảng`s`thực tế là có thể đạt được. Nếu như`t[i] = s[i]`, chúng ta đặt giá trị đó trực tiếp tại vị trí`i`. Nếu không thì`s[i] = s[i+1]`, vì vậy chúng tôi sao chép từ bên phải. 

Điều này có nghĩa là đối với mọi vị trí:```
best possible value at i = s[i]
```Do đó, tổng tối đa có thể có trên bất kỳ khoảng thời gian truy vấn nào chỉ là tổng của`s[l..r]`. 

Sau khi tính toán mảng cực đại hậu tố một lần, chúng tôi xây dựng tổng tiền tố và trả lời mọi truy vấn trong`O(1)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / ít nhất O(nq) | Lớn | Quá chậm | 
| Tối ưu | O(n + q) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mọi vị trí`i`, tính toán```
t[i] = max(a[i], b[i])
```Đây là giá trị lớn nhất mà vị trí đó`i`bản thân nó có thể đóng góp như một nguồn. 
2. Xây dựng một mảng hậu tố tối đa`s`.```
s[n] = t[n]
s[i] = max(t[i], s[i+1])
```

`s[i]`đại diện cho giá trị lớn nhất có thể đạt được vị trí`i`. 
3. Xây dựng tổng tiền tố của`s`.```
pref[0] = 0
pref[i] = pref[i-1] + s[i]
```4. Đối với một truy vấn`(l, r)`, trở lại```
pref[r] - pref[l-1]
```bởi vì mảng tối ưu có thể đạt được chính xác là`s`. 

### Tại sao nó hoạt động 

Mọi giá trị kết thúc ở vị trí`i`phải bắt nguồn từ một vị trí nào đó`j ≥ i`, vì tất cả việc sao chép đều di chuyển sang trái. Giá trị nguồn lớn nhất có sẵn tại vị trí`j`là`max(a[j], b[j])`. Do đó, không có giá trị cuối cùng ở vị trí`i`có thể vượt quá mức tối đa của số lượng đó trên toàn bộ hậu tố, cụ thể là`s[i]`. 

Điều này đưa ra giới hạn trên cho mọi vị trí. 

Mảng`s`bản thân nó là có thể đạt được. Bất cứ khi nào`t[i] = s[i]`, chúng ta tạo giá trị đó trực tiếp tại vị trí`i`. Mặt khác, hậu tố tối đa xuất phát từ đâu đó bên phải, nghĩa là`s[i] = s[i+1]`và chúng tôi có được nó bằng cách sao chép từ vị trí`i+1`. 

Vì mỗi vị trí có thể đồng thời đạt đến giới hạn trên của nó nên mảng tối ưu cuối cùng chính xác là`s`và mỗi câu trả lời truy vấn là tổng của khoảng tương ứng trong`s`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        s = [0] * n

        cur = 0
        for i in range(n - 1, -1, -1):
            cur = max(cur, a[i], b[i])
            s[i] = cur

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + s[i]

        ans = []
        for _ in range(q):
            l, r = map(int, input().split())
            ans.append(str(pref[r] - pref[l - 1]))

        out.append(" ".join(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính toán trực tiếp hậu tố cực đại. Biến`cur`lưu trữ giá trị lớn nhất được thấy cho đến nay trong khi quét từ phải sang trái. Từ`cur`luôn bằng giá trị lớn nhất của tất cả`max(a[j], b[j])`trong hậu tố được xử lý, gán`s[i] = cur`tạo ra chính xác mảng được mô tả trong bằng chứng. 

Mảng tổng tiền tố sử dụng chỉ mục dựa trên một. Điều này tránh được việc xử lý đặc biệt khi`l = 1`, vì mỗi tổng khoảng đều trở thành biểu thức chuẩn:```
pref[r] - pref[l - 1]
```Tất cả các giá trị vừa vặn thoải mái trong số nguyên Python. Tổng truy vấn lớn nhất có thể là khoảng`2 * 10^5 * 10^4 = 2 * 10^9`, vẫn nằm trong phạm vi 64-bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [3, 2, 1]
b = [1, 2, 3]
query = [1, 3]
```Cực đại hậu tố tính toán đầu tiên. 

| tôi | max(a[i], b[i]) | Hậu tố hiện tại tối đa | s[i] | 
| --- | --- | --- | --- | 
| 3 | 3 | 3 | 3 | 
| 2 | 2 | 3 | 3 | 
| 1 | 3 | 3 | 3 | 

Vì thế:```
s = [3, 3, 3]
```Tổng tiền tố: 

| Vị trí | Tiền tố Tổng | 
| --- | --- | 
| 0 | 0 | 
| 1 | 3 | 
| 2 | 6 | 
| 3 | 9 | 

Trả lời:```
pref[3] - pref[0] = 9
```Điều này chứng tỏ một giá trị được tạo ở ngoài cùng bên phải có thể lan truyền sang bên trái như thế nào. 

### Ví dụ 2 

đầu vào:```
a = [4, 3, 2, 1]
b = [5, 1, 3, 1]
queries:
(1,2)
(2,4)
(3,4)
```Cấu trúc hậu tố: 

| tôi | max(a[i], b[i]) | Hậu tố hiện tại tối đa | s[i] | 
| --- | --- | --- | --- | 
| 4 | 1 | 1 | 1 | 
| 3 | 3 | 3 | 3 | 
| 2 | 3 | 3 | 3 | 
| 1 | 5 | 5 | 5 | 

Như vậy:```
s = [5, 3, 3, 1]
```Tổng tiền tố: 

| Vị trí | Tiền tố Tổng | 
| --- | --- | 
| 0 | 0 | 
| 1 | 5 | 
| 2 | 8 | 
| 3 | 11 | 
| 4 | 12 | 

Kết quả truy vấn: 

| Truy vấn | Tính toán | Trả lời | 
| --- | --- | --- | 
| (1,2) | 8 - 0 | 8 | 
| (2,4) | 12 - 5 | 7 | 
| (3,4) | 12 - 8 | 4 | 

Ví dụ này cho thấy các vị trí khác nhau có thể có các giá trị tối ưu khác nhau, được xác định chỉ bằng cực đại hậu tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) cho mỗi trường hợp thử nghiệm | Một lần truyền hậu tố, một lần truyền tiền tố, sau đó O(1) cho mỗi truy vấn | 
| Không gian | O(n) | Lưu trữ tổng hậu tố cực đại và tiền tố | 

Tổng số tiền của tất cả`n`và tất cả`q`trên toàn bộ đầu vào nhiều nhất là`2 * 10^5`, do đó, thời gian chạy tổng thể là tuyến tính theo kích thước đầu vào và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        s = [0] * n
        cur = 0
        for i in range(n - 1, -1, -1):
            cur = max(cur, a[i], b[i])
            s[i] = cur

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + s[i]

        ans = []
        for _ in range(q):
            l, r = map(int, input().split())
            ans.append(str(pref[r] - pref[l - 1]))

        out.append(" ".join(ans))

    return "\n".join(out)

# provided sample
assert run(
"""4
3 1
3 2 1
1 2 3
1 3
1 1
1
2
1 1
3 2
6 7 5
9 6 8
1 2
2 3
4 3
4 3 2 1
5 1 3 1
1 2
2 4
3 4
"""
) == """9
2
17 16
8 7 4"""

# minimum size
assert run(
"""1
1 1
5
3
1 1
"""
) == "5"

# propagation from far right
assert run(
"""1
2 1
1 1
1 100
1 2
"""
) == "200"

# all equal values
assert run(
"""1
4 2
7 7 7 7
7 7 7 7
1 4
2 3
"""
) == "28 14"

# boundary interval ending at n
assert run(
"""1
3 2
1 5 2
4 3 6
1 3
3 3
"""
) == "18 6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | 5 | Trường hợp hợp lệ nhỏ nhất | 
| Giá trị lớn chỉ có ở`b[n]`| 200 | Tuyên truyền từ vị trí ngoài cùng bên phải | 
| Tất cả các giá trị bằng nhau | 28 14 | Ổn định khi mọi lựa chọn đều giống hệt nhau | 
| Truy vấn chạm vào chỉ mục cuối cùng | 18 6 | Ranh giới tổng tiền tố và xây dựng hậu tố | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 1
1 1
1 100
1 2
```Chúng tôi có`t = [1, 100]`, do đó mảng cực đại có hậu tố là:```
s = [100, 100]
```Câu trả lời là`200`. 

Một giải pháp chỉ nhìn vào`a`sẽ sản xuất`2`, điều này không chính xác vì vị trí 2 trước tiên có thể trở thành`100`rồi truyền sang trái. 

Bây giờ hãy xem xét:```
1
1 1
10
1
1 1
```Không có người hàng xóm phù hợp. Mảng tối đa hậu tố là:```
s = [10]
```Câu trả lời là`10`. 

Bất kỳ giải pháp nào giả định mọi giá trị phải đến từ việc sao chép sẽ bỏ lỡ khả năng đơn giản là rời khỏi`a[1]`không thay đổi. 

Cuối cùng, hãy xem xét:```
1
3 1
3 2 1
1 2 3
1 3
```Mảng cực đại hậu tố trở thành:```
s = [3, 3, 3]
```Câu trả lời là`9`. 

Điều này xác nhận tính bất biến trung tâm: mọi vị trí đều được giới hạn bởi giá trị nguồn tối đa có thể đạt được trong hậu tố của nó và giới hạn đó luôn có thể đạt được.
