---
title: "CF 2047A - Alyona và câu đố ghép hình hình vuông"
description: "Alyona xây dựng một câu đố hình vuông từng lớp một xung quanh một mảnh trung tâm. Lớp đầu tiên chỉ bao gồm mảnh trung tâm nên chứa 1 ô. Mỗi lớp sau tạo thành một vòng vuông hoàn chỉnh xung quanh câu đố trước đó. Mỗi ngày cô cộng thêm một số phần được cho bởi mảng a."
date: "2026-06-09T03:34:37+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2047
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 990 (Div. 2)"
rating: 800
weight: 2047
solve_time_s: 132
verified: true
draft: false
---

[CF 2047A - Alyona và Câu đố ghép hình hình vuông](https://codeforces.com/problemset/problem/2047/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Alyona xây dựng một câu đố hình vuông từng lớp một xung quanh một mảnh trung tâm. 

Lớp đầu tiên chỉ bao gồm mảnh trung tâm nên chứa 1 ô. Mỗi lớp sau tạo thành một vòng vuông hoàn chỉnh xung quanh câu đố trước đó. Mỗi ngày cô cộng thêm một số mảnh do mảng đưa ra`a`. 

Alyona sẽ rất vui vào cuối ngày nếu tổng số mảnh ghép được lắp ráp cho đến nay hoàn thành chính xác một lớp hình vuông nào đó. Nếu cô ấy hoàn thành một lớp và ngay lập tức bắt đầu lớp tiếp theo trong cùng ngày, cô ấy sẽ không vui vì một lớp chưa hoàn thành đã tồn tại vào cuối ngày hôm đó. 

Chúng ta được cung cấp số lượng mảnh được đặt mỗi ngày và phải đếm xem có bao nhiêu ngày kết thúc chính xác ở ranh giới lớp. 

Quan sát chính xuất phát từ hình học của các lớp hình vuông. Sau khi hoàn thành nhiều lớp, khối ghép được lắp ráp luôn là một hình vuông hoàn chỉnh. Tổng số miếng sau khi hoàn thiện lớp`k`là$$1, 9, 25, 49, 81, \dots$$Đây chính xác là những ô vuông lẻ:$$1^2,\; 3^2,\; 5^2,\; 7^2,\dots$$Vì vậy, một ngày hạnh phúc chính xác khi số lượng tích lũy các mảnh được đặt là một hình vuông hoàn hảo kỳ lạ. 

Những hạn chế là rất nhỏ. Mỗi trường hợp thử nghiệm có tối đa 100 ngày và có tối đa 500 trường hợp thử nghiệm. Ngay cả một thuật toán thực hiện một lượng nhỏ công việc mỗi ngày cũng đủ nhanh. Thách thức là nhận ra mô hình toán học đằng sau các lớp hình vuông. 

Một lỗi phổ biến là kiểm tra xem số mảnh tích lũy có phải là hình vuông hoàn hảo hay không. Chỉ có các ô vuông lẻ tương ứng với các lớp đã hoàn thành. 

Coi như:```
1
2
1 3
```Số tiền tích lũy là`1`Và`4`. Trong khi`4`là một hình vuông hoàn hảo, nó không phải là một hình vuông lẻ nên ngày thứ hai không vui. Câu trả lời đúng là:```
1
```Một sai lầm dễ mắc phải khác là tính một ngày bất cứ khi nào một lớp được hoàn thành trong thời gian bổ sung của ngày đó. Hạnh phúc phụ thuộc vào trạng thái vào cuối ngày. 

Ví dụ:```
1
2
1 10
```Số tiền tích lũy là`1`Và`11`. Ngày thứ 2 trôi qua`9`, hoàn thành một lớp, nhưng kết thúc tại`11`, nơi lớp tiếp theo đã bắt đầu. Câu trả lời đúng là:```
1
```## Phương pháp tiếp cận 

Giải pháp brute-force có thể mô phỏng trực tiếp kích thước lớp. 

Trung tâm đóng góp 1 phần. Các lớp tiếp theo đóng góp:$$8,\;16,\;24,\;32,\dots$$vì chu vi được cộng thêm bởi hình vuông có độ dài cạnh$2k+1$là$8k$. 

Chúng ta có thể tiếp tục tạo các ranh giới lớp và sau mỗi ngày, kiểm tra xem số lượng mảnh tích lũy có bằng một trong số chúng hay không. Điều này đúng vì ranh giới lớp chính xác là tổng sau khi hoàn thành mỗi vòng. 

Mặc dù điều này hoạt động với các ràng buộc nhất định, nhưng nó dựa vào việc tạo ra thông tin lớp hình học một cách rõ ràng. 

Quan sát tinh tế hơn là các lớp đã hoàn thành luôn tạo thành các hình vuông có chiều dài cạnh lẻ:$$1 \times 1,\; 3 \times 3,\; 5 \times 5,\; 7 \times 7,\dots$$Do đó tổng số mảnh ở một lớp hoàn thành là:$$1^2,\;3^2,\;5^2,\;7^2,\dots$$Vấn đề ngay lập tức trở thành: 

"Đếm xem có bao nhiêu tổng tiền tố là số chính phương hoàn hảo lẻ." 

Vì tổng số mảnh tối đa có thể chỉ là$100 \times 100 = 10000$, chúng ta có thể tính toán trước tất cả các ô vuông lẻ lên tới 10000 và kiểm tra tư cách thành viên trong một tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n + số lớp) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các ô vuông hoàn hảo lẻ lên đến 10000 và lưu trữ chúng trong một bộ. 
2. Đối với mỗi trường hợp thử nghiệm, hãy khởi tạo tổng tiền tố đang chạy`s = 0`. 
3. Xử lý các ngày theo thứ tự. 
4. Cộng số quân được đặt vào ngày hiện tại vào`s`. 
5. Kiểm tra xem`s`thuộc tập hợp các ô vuông lẻ. 
6. Nếu đúng như vậy, hãy tăng câu trả lời vì câu đố kết thúc chính xác ở một lớp đã hoàn thành. 
7. Sau khi tất cả các ngày được xử lý, hãy xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Một lớp câu đố hoàn chỉnh luôn tạo thành một hình vuông có chiều dài cạnh là số lẻ. Sau khi hoàn thành lớp đầu tiên chúng ta có$1^2$miếng, sau khi hoàn thành lớp thứ hai chúng ta có$3^2$miếng, sau khi hoàn thành lớp thứ ba chúng ta có$5^2$miếng, vân vân. 

Ngược lại, mỗi ô vuông lẻ tương ứng với một tập hợp các lớp đã hoàn thiện. Như vậy, một ngày sẽ hạnh phúc khi tổng số quân được đặt bằng một hình vuông lẻ. Thuật toán đếm chính xác những ngày đó nên không thể đếm thừa hoặc đếm thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# all possible totals are at most 100 * 100 = 10000
odd_squares = set()
k = 1
while k * k <= 10000:
    odd_squares.add(k * k)
    k += 2

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    s = 0
    ans = 0

    for x in a:
        s += x
        if s in odd_squares:
            ans += 1

    print(ans)
```Phần đầu tiên tính toán trước mọi ô vuông lẻ có thể xuất hiện dưới dạng tổng tiền tố. Vì tổng số mảnh không bao giờ vượt quá 10000 nên chỉ cần các ô vuông lẻ đạt đến giới hạn đó. 

Đối với mỗi trường hợp thử nghiệm, biến`s`lưu trữ số lượng tích lũy các mảnh được lắp ráp cho đến nay. Sau khi xử lý một ngày, chúng tôi kiểm tra xem tổng tích lũy này có bằng bình phương lẻ hay không. 

Việc sử dụng một bộ giúp kiểm tra thành viên theo thời gian trung bình liên tục. Không cần tính toán căn bậc hai, điều này giúp việc triển khai đơn giản và tránh mọi lo ngại về dấu phẩy động. 

Các điều kiện biên là đơn giản. Ngày đầu tiên luôn đóng góp chính xác một phần, vì vậy tổng tiền tố đầu tiên là`1`, được nhận dạng chính xác là hình vuông lẻ đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [1, 3, 2, 1, 2]
```| Ngày | Các mảnh được thêm vào | Tiền tố Tổng | Hình vuông kỳ lạ? | Ngày Hạnh Phúc | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Có | 1 | 
| 2 | 3 | 4 | Không | 1 | 
| 3 | 2 | 6 | Không | 1 | 
| 4 | 1 | 7 | Không | 1 | 
| 5 | 2 | 9 | Có | 2 | 

Tổng tiền tố chạm vào ô vuông lẻ`1`Và`9`. Những lớp đó tương ứng với các lớp đã hoàn thành, vì vậy câu trả lời là 2. 

### Ví dụ 2 

đầu vào:```
n = 2
a = [1, 8]
```| Ngày | Các mảnh được thêm vào | Tiền tố Tổng | Hình vuông kỳ lạ? | Ngày Hạnh Phúc | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Có | 1 | 
| 2 | 8 | 9 | Có | 2 | 

Lớp thứ hai chứa chính xác 8 miếng. Thêm chúng sau khi trung tâm đạt tổng cộng 9 mảnh, đó là$3^2$. Cả hai ngày đều vui vẻ. 

Ví dụ này minh họa cách giải thích hình học một cách trực tiếp: mỗi lớp hoàn chỉnh tương ứng với một hình vuông lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một ngày trôi qua | 
| Không gian | O(1) | Tập hợp các ô vuông lẻ có kích thước cố định | 

Tổng công việc tối đa là rất nhỏ. Ngay cả với 500 ca kiểm thử và 100 ngày cho mỗi ca kiểm thử, chỉ có 50.000 tổng tiền tố được kiểm tra. Giải pháp thoải mái phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    odd_squares = set()
    k = 1
    while k * k <= 10000:
        odd_squares.add(k * k)
        k += 2

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        s = 0
        ans = 0

        for x in a:
            s += x
            if s in odd_squares:
                ans += 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""5
1
1
2
1 8
5
1 3 2 1 2
7
1 2 1 10 2 7 2
14
1 10 10 100 1 1 10 1 10 2 10 2 10 1
"""
) == """1
2
2
2
3"""

# minimum size
assert run(
"""1
1
1
"""
) == "1"

# reaches 9 exactly
assert run(
"""1
2
1 8
"""
) == "2"

# hits even square but not odd square
assert run(
"""1
2
1 3
"""
) == "1"

# maximum style boundary, final total 81
assert run(
"""1
2
1 80
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`1`| Trường hợp nhỏ nhất có thể | 
|`1 / 2 / 1 8`|`2`| Hoàn thành lớp thứ hai | 
|`1 / 2 / 1 3`|`1`| Ngay cả những hình vuông hoàn hảo cũng không được tính | 
|`1 / 2 / 1 80`|`2`| Ranh giới hình vuông lẻ lớn hơn | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
1 3
```Tổng tiền tố là`1`Và`4`. Một giải pháp bất cẩn để kiểm tra bất kỳ hình vuông hoàn hảo nào sẽ được tính cả hai ngày. Thuật toán kiểm tra tư cách thành viên trong tập hợp các ô vuông lẻ, do đó chỉ`1`tính. Đầu ra là:```
1
```Coi như:```
1
2
1 10
```Tổng tiền tố là`1`Và`11`. Trong ngày thứ 2 tổng số hoạt động đã vượt qua`9`, nhưng ngày kết thúc lúc`11`. Hạnh phúc chỉ phụ thuộc vào trạng thái cuối cùng trong ngày. Từ`11`không phải là bình phương lẻ, thuật toán chỉ tính ngày đầu tiên:```
1
```Coi như:```
1
2
1 8
```Tổng tiền tố là`1`Và`9`. Cả hai đều là hình vuông lẻ, tương ứng với các lớp đã hoàn thành đầy đủ. Thuật toán tính cả ngày và kết quả đầu ra:```
2
```Điều này xác nhận rằng ranh giới lớp chính xác được nhận dạng chính xác, bao gồm cả việc chuyển đổi từ hình vuông hoàn chỉnh này sang hình vuông tiếp theo.
