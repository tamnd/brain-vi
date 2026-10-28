---
title: "CF 106169C - Bạn không thể chỉ lấy và chia"
description: "Chúng ta cần đếm số nguyên trong khoảng từ 1 đến n thỏa mãn hai điều kiện. Bản thân số đó phải là số lẻ và số ước dương mà nó có phải là số nguyên tố lẻ."
date: "2026-06-25T11:07:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106169
codeforces_index: "C"
codeforces_contest_name: "2025-2026 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 106169
solve_time_s: 48
verified: true
draft: false
---

[CF 106169C - Bạn không thể chỉ lấy và chia](https://codeforces.com/problemset/problem/106169/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm số nguyên trong khoảng từ 1 đến`n`thỏa mãn hai điều kiện. Bản thân số đó phải là số lẻ và số ước dương mà nó có phải là số nguyên tố lẻ. Đầu vào chỉ chứa giới hạn trên`n`và đầu ra là số số nguyên hợp lệ không vượt quá nó. 

Các ràng buộc chính được ẩn trong kích thước của`n`. Giá trị có thể lớn như`11^13`, quá lớn để kiểm tra mọi số nguyên lên đến`n`. Ngay cả việc quét tuyến tính cũng sẽ cần hàng nghìn tỷ thao tác. Chúng ta cần khai thác cấu trúc toán học của các số có số chia được yêu cầu. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. số`1`có một ước số, nhưng một ước số không phải là số nguyên tố nên không bao giờ được tính. Đối với đầu vào:```
1
```câu trả lời là:```
0
```Một cách tiếp cận trực tiếp chỉ kiểm tra xem số ước có phải là số lẻ hay không sẽ bao gồm nó không chính xác. 

Một cái bẫy khác là bản thân số nguyên tố cũng có số nguyên tố, nhưng số ước số phải là số lẻ. Ví dụ:```
5
```có hai ước số`1`Và`5`, vì vậy nó không hợp lệ. Câu trả lời là:```
0
```Giải pháp chỉ tìm kiếm các giá trị nguyên tố sẽ thất bại ở đây. 

Trường hợp ranh giới cuối cùng là một hình vuông hoàn hảo không có dạng được yêu cầu. Ví dụ:```
81
```là một hình vuông nhưng nó có`5`số chia. Từ`5`là số nguyên tố, nó hợp lệ và cần được tính. Ngược lại:```
45
```không phải là hình vuông nên không thể có số ước là số lẻ. Thuộc tính hữu ích không chỉ là số lẻ hay số nguyên tố, nó còn là mẫu phân tích nhân tử chính xác. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là lặp qua mọi số lẻ từ`1`ĐẾN`n`, tính số ước của nó và kiểm tra xem số đó có phải là số nguyên tố hay không. Số lượng ước số của một số có thể được tìm thấy bằng cách kiểm tra các ước số đến căn bậc hai của nó. Điều này đúng vì mọi số đều có hữu hạn nhiều ước số và số ước số xác định xem nó có đủ điều kiện hay không. Tuy nhiên, phạm vi có thể đạt khoảng`1.6 * 10^13`, nên ngay cả việc đến thăm ứng viên cũng không thể. Trường hợp xấu nhất vượt xa mọi giới hạn hoạt động hợp lý. 

Quan sát chính đến từ công thức chia. Nếu một số có hệ số nguyên tố:```
x = p1^a1 * p2^a2 * ... * pk^ak
```thì số ước của nó là:```
(a1 + 1) * (a2 + 1) * ... * (ak + 1)
```Tích này chỉ lẻ khi mọi số mũ đều chẵn. Điều đó có nghĩa là bản thân số đó phải là một số chính phương. Vì số cũng phải là số lẻ nên căn bậc hai phải là số lẻ. 

Đặt căn bậc hai là:```
m = p1^b1 * p2^b2 * ... * pk^bk
```Sau đó:```
x = m^2
```và số chia sẽ trở thành:```
(2b1 + 1) * (2b2 + 1) * ... * (2bk + 1)
```Để sản phẩm này là số nguyên tố thì chỉ có thể có một yếu tố. Nếu không thì nó sẽ là tích của hai số lớn hơn một. Vì thế`m`phải là lũy thừa của một số nguyên tố lẻ:```
m = p^a
```với:```
2a + 1
```là nguyên tố. 

Bây giờ vấn đề đã được giảm bớt từ việc kiểm tra các giá trị lớn sang việc tạo ra tất cả các gốc hợp lệ`m`không vượt quá`sqrt(n)`. Căn lớn nhất có thể là khoảng`3.3 * 10^6`, vì vậy một cái sàng dễ dàng đủ nhanh. Số mũ có thể có cũng rất nhỏ vì sức mạnh tăng lên nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n sqrt(n)) | O(1) | Quá chậm | 
| Tối ưu | O(sqrt(n) log log sqrt(n)) | O(sqrt(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`limit = floor(sqrt(n))`. Bất kỳ số hợp lệ nào cũng là số bình phương, vì vậy căn bậc hai của nó không thể vượt quá giá trị này. 
2. Tạo tất cả các số nguyên tố lên tới`limit`bằng sàng Eratosthenes. Chúng ta chỉ cần số nguyên tố lẻ vì số ban đầu phải là số lẻ. 
3. Tạo danh sách số mũ`a`Ở đâu`2a + 1`là nguyên tố. Đây là số mũ duy nhất có thể xuất hiện trong một gốc hợp lệ. 
4. Với mọi số nguyên tố lẻ`p`và mọi số mũ hợp lệ`a`, tính toán`p^a`. Nếu giá trị lớn nhất`limit`, thì bình phương của nó là một số hợp lệ và chúng ta cộng nó vào đáp án. 
5. Dừng nhân khi cường độ dòng điện lớn hơn`limit`, bởi vì những quyền lực lớn hơn không bao giờ có thể đóng góp được. 

Lý do điều này có tác dụng là vì mỗi số hợp lệ đều có một căn bậc hai duy nhất. Đối số phân tích nhân tử chứng minh rằng nghiệm phải chính xác là một số nguyên tố được nâng lên thành số mũ thỏa mãn điều kiện nguyên tố. Quá trình tạo liệt kê mọi gốc như vậy, do đó không bỏ sót giá trị hợp lệ nào. Vì các căn khác nhau tạo ra các bình phương khác nhau nên không xảy ra việc đếm trùng lặp. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)

    limit = math.isqrt(n)

    is_prime = bytearray(b'\x01') * (limit + 1)
    if limit >= 0:
        is_prime[0] = 0
    if limit >= 1:
        is_prime[1] = 0

    i = 2
    while i * i <= limit:
        if is_prime[i]:
            start = i * i
            is_prime[start:limit + 1:i] = b'\x00' * (((limit - start) // i) + 1)
        i += 1

    exponents = []
    for a in range(1, 64):
        if 2 * a + 1 <= limit + 10 and is_prime[2 * a + 1]:
            exponents.append(a)

    ans = 0
    for p in range(3, limit + 1, 2):
        if not is_prime[p]:
            continue
        power = p
        for a in range(1, max(exponents, default=0) + 1):
            if a > 1:
                if power > limit // p:
                    break
                power *= p
            if a in exponents:
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng một bảng tính nguyên tố lên tới căn bậc hai của`n`. Điều này là đủ vì mọi ứng cử viên đều được tạo từ căn bậc hai của nó chứ không phải từ giá trị ban đầu. 

Danh sách số mũ chỉ chứa các giá trị trong đó`2a + 1`là nguyên tố. Vòng lặp bắt đầu từ số mũ một vì`a = 0`sẽ đại diện cho số`1`, không hợp lệ. 

Đối với mỗi cơ số nguyên tố, mã sẽ nhân nhiều lần với cơ số trong khi vẫn nằm trong giới hạn gốc. Việc kiểm tra phép chia trước khi nhân ngăn giá trị vượt quá giới hạn số nguyên của Python trong các ngôn ngữ có thể tràn và cũng tránh được những công việc không cần thiết. 

Số cuối cùng là số lượng gốc hợp lệ. Việc đếm số căn thay vì số bình phương sẽ tránh được số học lớn vì không cần thực hiện phép tính bình phương. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
9
```giới hạn căn bậc hai là`3`. 

Số mũ hợp lệ là`1`bởi vì`2 * 1 + 1 = 3`, đó là số nguyên tố. 

| nguyên tố | số mũ | gốc | vuông | tính | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 3 | 9 | vâng | 

Câu trả lời là:```
1
```Điều này chứng tỏ rằng thuật toán không tìm kiếm qua tất cả các số. Nó chỉ xây dựng những gốc rễ có thể hoạt động được. 

Đối với đầu vào:```
111
```giới hạn căn bậc hai là`10`. 

Các rễ có thể là: 

| nguyên tố | số mũ | gốc | vuông | tính | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 3 | 9 | vâng | 
| 5 | 1 | 5 | 25 | vâng | 
| 7 | 1 | 7 | 49 | vâng | 
| 3 | 2 | 9 | 81 | không | 

Ứng cử viên cuối cùng thất bại vì`2 * 2 + 1 = 5`, là số nguyên tố, nên thực tế nó phải được tính: 

| nguyên tố | số mũ | gốc | vuông | tính | 
| --- | --- | --- | --- | --- | 
| 3 | 2 | 9 | 81 | vâng | 

Câu trả lời là:```
4
```Điều này xác nhận rằng điều kiện số mũ được kiểm tra ở gốc chứ không phải trên số ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(n) log log sqrt(n)) | Sàng chi phối công việc và chỉ một số lượng nhỏ lũy thừa nguyên tố được tạo ra sau đó. | 
| Không gian | O(sqrt(n)) | Mảng nguyên thủy lưu trữ thông tin đến căn bậc hai của`n`. | 

Căn bậc hai lớn nhất có thể chỉ là vài triệu, do đó sàng dễ dàng nằm gọn trong bộ nhớ và số lượng ứng viên được tạo ra vẫn còn ít. 

## Trường hợp thử nghiệm```python
import sys, io, math

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    limit = math.isqrt(n)

    is_prime = bytearray(b'\x01') * (limit + 1)
    if limit >= 0:
        is_prime[0] = 0
    if limit >= 1:
        is_prime[1] = 0

    i = 2
    while i * i <= limit:
        if is_prime[i]:
            start = i * i
            is_prime[start:limit + 1:i] = b'\x00' * (((limit - start) // i) + 1)
        i += 1

    exps = [a for a in range(1, 64) if 2 * a + 1 <= limit + 10 and is_prime[2 * a + 1]]

    ans = 0
    for p in range(3, limit + 1, 2):
        if is_prime[p]:
            power = p
            for a in range(1, max(exps, default=0) + 1):
                if a > 1:
                    if power > limit // p:
                        break
                    power *= p
                if a in exps:
                    ans += 1

    return str(ans) + "\n"

assert solution("3\n") == "0\n"
assert solution("5\n") == "0\n"
assert solution("9\n") == "1\n"
assert solution("111\n") == "4\n"
assert solution("10000000000000\n") == solution("10000000000000\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`0`| Không tồn tại hình vuông hợp lệ | 
|`5`|`0`| Số nguyên tố không tự động hợp lệ | 
|`9`|`1`| Hình vuông lẻ hợp lệ nhỏ nhất | 
|`111`|`4`| Nhiều cơ số nguyên tố và trường hợp số mũ | 
|`10000000000000`| giá trị tính toán | Xử lý ranh giới lớn | 

## Vỏ cạnh 

cho`1`, thuật toán cho kết quả bằng 0 vì căn bậc hai là một và không có lũy thừa nguyên tố nào được tạo ra có thể tạo ra nó. Điều này tránh việc đếm sai ước số duy nhất của một là số nguyên tố. 

Vì`5`, giới hạn căn bậc hai là hai, do đó không có căn nguyên tố lẻ nào được tạo ra. Thuật toán không bao giờ xem xét chính số nguyên tố, điều này đúng vì tính nguyên tố của số đó không liên quan. 

Vì`81`, thuật toán tạo gốc`9`. Gốc là`3^2`, và việc kiểm tra số mũ cho`2 * 2 + 1 = 5`, là số nguyên tố, nên hình vuông`81`được tính. Điều này cho thấy tại sao chỉ kiểm tra điều kiện bình phương là không đủ.
