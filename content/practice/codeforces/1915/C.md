---
title: "CF 1915C - Tôi có thể bình phương được không?"
description: "Chúng ta được cấp một số thùng và mỗi thùng chứa một số ô vuông đơn vị. Tất cả các hình vuông đều giống hệt nhau và có cạnh dài 1. Câu hỏi rất đơn giản: nếu chúng ta lấy mọi hình vuông từ mỗi nhóm, liệu chúng có thể được sắp xếp để tạo thành một hình vuông lớn hơn và không còn hình vuông nào không?"
date: "2026-06-08T19:55:54+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1915
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 918 (Div. 4)"
rating: 800
weight: 1915
solve_time_s: 121
verified: true
draft: false
---

[CF 1915C - Tôi có thể bình phương được không?](https://codeforces.com/problemset/problem/1915/C) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, triển khai 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số thùng và mỗi thùng chứa một số ô vuông đơn vị. Tất cả các hình vuông đều giống nhau và có cạnh bằng 1. 

Câu hỏi rất đơn giản: nếu chúng ta lấy mọi hình vuông từ mỗi nhóm, liệu chúng có thể được sắp xếp để tạo thành một hình vuông lớn hơn mà không còn hình vuông nào không? 

Nếu tổng số ô vuông đơn vị là$S$, thì một hình vuông lớn hơn có thể được xây dựng chính xác khi diện tích của nó bằng$S$. Vì diện tích của một hình vuông phải là một số chính phương nên nhiệm vụ giảm xuống còn việc kiểm tra xem tổng của tất cả nội dung trong nhóm có phải là một hình vuông hoàn hảo hay không. 

Các ràng buộc đủ nhỏ để chúng ta có thể xử lý mọi phần tử chính xác một lần. Trên tất cả các trường hợp thử nghiệm, tổng số giá trị nhiều nhất là$2 \cdot 10^5$, vì vậy một$O(n)$giải pháp cho mỗi trường hợp thử nghiệm là đủ nhanh. Bất kỳ cách tiếp cận nào liên tục tìm kiếm hoặc lặp lại các giá trị giống nhau sẽ không cần thiết. 

Một điểm tinh tế là các giá trị riêng lẻ có thể lớn bằng$10^9$. Vì có thể có tới$2 \cdot 10^5$nhìn chung, tổng số tiền có thể trở nên rất lớn. Trong các ngôn ngữ có loại số nguyên có kích thước cố định, việc sử dụng loại quá nhỏ sẽ bị tràn. Số nguyên Python tự động tăng lên, vì vậy đây không phải là vấn đề ở đây. 

Một sai lầm dễ mắc phải khác là chỉ kiểm tra xem căn bậc hai có phải là số nguyên hay không bằng cách sử dụng số học dấu phẩy động. Số lượng lớn có thể gặp phải các vấn đề về độ chính xác. Ví dụ:```
1
2
1000000000 1000000000
```Tổng số tiền là$2000000000$, đây không phải là một hình vuông hoàn hảo. Làm tròn dấu phẩy động đôi khi có thể gây ra so sánh không chính xác đối với các giá trị lớn. Sử dụng căn bậc hai số nguyên sẽ tránh được rủi ro đó. 

Một trường hợp cạnh khác xảy ra khi chỉ có một nhóm:```
1
1
9
```Tổng số hình vuông là 9, bằng$3^2$, vậy đáp án đúng là:```
YES
```Việc triển khai bất cẩn cố gắng xây dựng các kích thước một cách rõ ràng thay vì kiểm tra khu vực có thể làm cho trường hợp này trở nên phức tạp hơn. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là tính tổng số bình phương đơn vị và sau đó xác định xem số đó có thể được viết dưới dạng không$k^2$đối với một số nguyên$k$. 

Một phiên bản brute-force sẽ liên tục thử các giá trị của$k$cho đến khi$k^2$đạt hoặc vượt tổng số tiền. Nếu tổng là$S$, điều này đòi hỏi lên đến$\sqrt{S}$lần lặp lại. Vì số tiền có thể cực kỳ lớn nên điều này trở nên không thực tế. Ví dụ, một khoản tiền gần$2 \cdot 10^{14}$sẽ yêu cầu khoảng$1.4 \cdot 10^7$séc. 

Quan sát quan trọng là việc sắp xếp các hình vuông không thành vấn đề. Chỉ có tổng số quan trọng. Khi chúng ta biết tổng diện tích, bài toán sẽ trở thành phép thử bình phương hoàn hảo. 

Thay vì tìm kiếm căn bậc hai theo cách thủ công, chúng ta có thể tính căn bậc hai nguyên của tổng. Cho phép$$r = \lfloor \sqrt{S} \rfloor.$$Nếu như$r^2 = S$, sau đó$S$là một hình vuông hoàn hảo và câu trả lời là "CÓ". Ngược lại, câu trả lời là "KHÔNG". 

Điều này làm giảm vấn đề xuống còn một lần duyệt qua các giá trị đầu vào cộng với một phép tính căn bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n + \sqrt{S})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc$n$và mảng nội dung nhóm. 
3. Tính tổng số ô vuông đơn vị:$$S = \sum a_i$$Vì phải sử dụng mọi hình vuông nên tổng này chính xác là diện tích của hình cuối cùng. 
4. Tính căn bậc hai số nguyên$r$của$S$. 

Căn bậc hai là số nguyên lớn nhất mà bình phương của nó không vượt quá$S$. 
5. Kiểm tra xem$r^2 = S$. 

Nếu sự bình đẳng giữ vững,$S$là một hình vuông hoàn hảo và một hình vuông có thể được hình thành. 
6. Đầu ra`"YES"`khi$r^2 = S$, nếu không thì xuất ra`"NO"`. 

### Tại sao nó hoạt động 

Hình vuông có độ dài cạnh$k$có diện tích$k^2$. Tổng số ô vuông đơn vị có sẵn là$S$, do đó, bất kỳ hình vuông nào được xây dựng từ tất cả các mảnh đều phải có diện tích chính xác$S$. 

Nếu như$S$là hình vuông hoàn hảo, chọn độ dài cạnh$k = \sqrt{S}$và việc xây dựng là có thể. 

Nếu như$S$không phải là một hình vuông hoàn hảo, không có độ dài cạnh nguyên nào có thể tạo ra diện tích$S$, vì vậy việc xây dựng một hình vuông là không thể. 

Thuật toán kiểm tra chính xác điều kiện cần và đủ này, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)
    root = isqrt(total)

    print("YES" if root * root == total else "NO")
```Bước đầu tiên tính tổng diện tích bằng cách tính tổng tất cả nội dung trong thùng. 

chức năng`isqrt()`trả về căn bậc hai số nguyên mà không sử dụng số học dấu phẩy động. Điều này tránh được các vấn đề về độ chính xác có thể xảy ra khi số lượng trở nên lớn. 

Sau khi có được`root`, mã sẽ kiểm tra xem`root * root`bằng số tiền ban đầu. Bình đẳng có nghĩa là tổng là một bình phương hoàn hảo. Nếu không thì không. 

Việc triển khai chỉ sử dụng bộ nhớ bổ sung không đổi ngoài mảng đầu vào và xử lý từng phần tử chính xác một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2
14 2
```Các thùng chứa 14 và 2 hình vuông. 

| Bước | Giá trị | 
| --- | --- | 
| Tổng$S$| 16 | 
| Căn bậc hai số nguyên$r$| 4 | 
|$r^2$| 16 | 
| Quyết định | CÓ | 

Từ$16 = 4^2$, tổng diện tích tương ứng chính xác với một hình vuông. 

### Ví dụ 2 

đầu vào:```
1
7
1 2 3 4 5 6 7
```| Bước | Giá trị | 
| --- | --- | 
| Tổng$S$| 28 | 
| Căn bậc hai số nguyên$r$| 5 | 
|$r^2$| 25 | 
| Quyết định | KHÔNG | 

Hình vuông gần nhất dưới 28 là 25. Vì$25 \neq 28$, tổng diện tích không phải là một hình vuông hoàn hảo. 

Ví dụ này chứng minh rằng việc có nhiều nhóm không thành vấn đề. Chỉ có số tiền cuối cùng mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi giá trị được xử lý một lần trong khi tính tổng | 
| Không gian |$O(1)$| Chỉ có một số biến được sử dụng ngoài bộ lưu trữ đầu vào | 

Tổng số giá trị trên tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, do đó việc quét tuyến tính có thể dễ dàng thực hiện trong thời gian giới hạn. Việc sử dụng bộ nhớ không đáng kể so với giới hạn sẵn có. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isqrt

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        s = sum(a)
        r = isqrt(s)

        ans.append("YES" if r * r == s else "NO")

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run(
"""5
1
9
2
14 2
7
1 2 3 4 5 6 7
6
1 3 5 7 9 11
4
2 2 2 2
"""
) == "YES\nYES\nNO\nYES\nNO"

# minimum size
assert run(
"""1
1
1
"""
) == "YES"

# single bucket, non-square
assert run(
"""1
1
2
"""
) == "NO"

# all equal values, perfect square total
assert run(
"""1
4
4 4 4 4
"""
) == "YES"

# large boundary-style values
assert run(
"""1
2
1000000000 1000000000
"""
) == "NO"

# perfect square formed from many buckets
assert run(
"""1
9
1 1 1 1 1 1 1 1 1
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, a=[1]`| CÓ | Đầu vào hợp lệ nhỏ nhất | 
|`n=1, a=[2]`| KHÔNG | Tổng cộng một thùng không vuông | 
|`a=[4,4,4,4]`| CÓ | Tất cả các giá trị bằng nhau | 
|`a=[10^9,10^9]`| KHÔNG | Tổng lớn và số học số nguyên | 
| Chín cái | CÓ | Diện tích hình vuông chính xác từ nhiều thùng | 

## Vỏ cạnh 

Xét trường hợp bình phương hoàn hảo nhỏ nhất có thể:```
1
1
1
```Tổng số tiền là$1$. Căn bậc hai số nguyên cũng là$1$, Và$1^2 = 1$. Các đầu ra thuật toán:```
YES
```Bây giờ hãy xem xét một nhóm có số lượng không phải là hình vuông:```
1
1
2
```Tổng số tiền là$2$. Căn bậc hai số nguyên là$1$, Nhưng$1^2 = 1 \neq 2$. Thuật toán xuất ra chính xác:```
NO
```Hãy xem xét một trường hợp có giá trị lớn:```
1
2
1000000000 1000000000
```Tổng số tiền là$2000000000$. Căn bậc hai số nguyên là$44721$, Và$$44721^2 = 1999967841.$$Vì số này khác với tổng nên thuật toán đưa ra:```
NO
```Điều này xác nhận rằng việc sử dụng căn bậc hai số nguyên chính xác sẽ tránh được các vấn đề về độ chính xác của dấu phẩy động. 

Cuối cùng, hãy xem xét tổng mức bình phương hoàn hảo trên nhiều nhóm:```
1
4
2 2 2 3
```Tổng số tiền là$9$. Căn bậc hai số nguyên là$3$, Và$3^2 = 9$. Đầu ra là:```
YES
```Việc phân phối giữa các nhóm không bao giờ quan trọng. Chỉ có tổng diện tích mới quyết định liệu một hình vuông có thể được hình thành hay không.
