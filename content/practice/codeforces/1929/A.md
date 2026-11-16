---
title: "CF 1929A - Sasha Và Mảng Đẹp"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta có thể sắp xếp lại các phần tử của nó theo bất kỳ thứ tự nào. Vẻ đẹp của một sự sắp xếp cụ thể được định nghĩa là tổng sự khác biệt giữa các phần tử liên tiếp: $$(a2-a1)+(a3-a2)+cdots+(an-a{n-1})$$ Nhiệm vụ của chúng ta là chọn thứ tự tạo ra…"
date: "2026-06-08T18:38:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1929
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 926 (Div. 2)"
rating: 800
weight: 1929
solve_time_s: 100
verified: true
draft: false
---

[CF 1929A - Sasha và Mảng xinh đẹp](https://codeforces.com/problemset/problem/1929/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta có thể sắp xếp lại các phần tử của nó theo bất kỳ thứ tự nào. 

Vẻ đẹp của một sự sắp xếp cụ thể được định nghĩa là tổng của sự khác biệt giữa các yếu tố liên tiếp:$$(a_2-a_1)+(a_3-a_2)+\cdots+(a_n-a_{n-1})$$Nhiệm vụ của chúng ta là chọn thứ tự tạo ra vẻ đẹp lớn nhất có thể. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được các phần tử mảng và phải xuất ra độ đẹp tối đa có thể đạt được sau khi sắp xếp lại chúng. 

Những hạn chế là rất nhỏ. Độ dài mảng tối đa là 100 và có tối đa 500 trường hợp thử nghiệm. Thậm chí một$O(n^2)$giải pháp sẽ dễ dàng đủ nhanh. Thử thách không phải là hiệu suất mà là nhận ra sự đơn giản hóa toán học ẩn giấu bên trong công thức làm đẹp. 

Một sai lầm phổ biến là nghĩ rằng bản thân sự sắp xếp đã có ý nghĩa phức tạp. Ví dụ: người ta có thể thử đặt xen kẽ các số nhỏ và số lớn:```
1 100 2 99
```và tính toán$$(100-1)+(2-100)+(99-2)=98$$Điều này có vẻ lớn, nhưng nó không tối ưu. Một sự sắp xếp đơn giản hơn,```
1 2 99 100
```cho$$(2-1)+(99-2)+(100-99)=99$$Một trường hợp cạnh khác xảy ra khi tất cả các giá trị đều bằng nhau:```
5 5 5
```Mọi sự khác biệt đều bằng 0, vì vậy câu trả lời phải là:```
0
```Việc thực hiện bất cẩn với giả định câu trả lời luôn là tích cực sẽ thất bại ở đây. 

Trường hợp quan trọng thứ ba là kích thước mảng tối thiểu:```
2
1 10
```Vẻ đẹp chỉ đơn giản là$10-1=9$, vì chỉ có một điểm khác biệt liên tiếp. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo ra mọi hoán vị của mảng, tính toán vẻ đẹp của nó và giữ mức tối đa. 

Điều này hiệu quả vì nó trực tiếp kiểm tra mọi sự sắp xếp có thể. Đối với một mảng có độ dài$n$, có$n!$hoán vị. Ngay cả đối với$n=10$, tức là đã có 3.628.800 hoán vị. Từ$n$có thể là 100, vũ phu là hoàn toàn không thể. 

Quan sát quan trọng đến từ việc mở rộng công thức làm đẹp:$$(a_2-a_1)+(a_3-a_2)+\cdots+(a_n-a_{n-1})$$Tất cả các điều khoản trung gian bị hủy bỏ:$$-a_2+a_2,\quad -a_3+a_3,\quad \ldots$$Chỉ còn lại phần tử đầu tiên và cuối cùng:$$a_n-a_1$$Điều này có nghĩa là vẻ đẹp của bất kỳ sự sắp xếp nào cũng chỉ phụ thuộc vào yếu tố đầu tiên và cuối cùng của nó. 

Một khi vấn đề được giảm xuống mức tối đa$$a_n-a_1,$$câu trả lời trở nên rõ ràng. Chúng ta nên đặt giá trị nhỏ nhất đầu tiên và giá trị lớn nhất cuối cùng. Rồi vẻ đẹp trở nên$$\max(a)-\min(a).$$Không có sự sắp xếp nào khác có thể làm tốt hơn vì không có phần tử nào nhỏ hơn mức tối thiểu và không có phần tử nào lớn hơn mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Tìm phần tử nhỏ nhất của mảng. 
3. Tìm phần tử lớn nhất của mảng. 
4. Xuất ra sự khác biệt giữa giá trị tối đa và tối thiểu. 

Lý do điều này có hiệu quả là vì công thức làm đẹp luôn tập trung vào:$$a_n-a_1.$$Để tối đa hóa số lượng đó, chúng ta đặt giá trị nhỏ nhất ở đầu và giá trị lớn nhất ở cuối. 

### Tại sao nó hoạt động 

Biểu hiện vẻ đẹp là một tổng thể kính thiên văn. Mỗi phần tử bên trong xuất hiện đúng hai lần: một lần mang dấu dương và một lần mang dấu âm. Những đóng góp này triệt tiêu lẫn nhau. 

Kết quả là vẻ đẹp của bất kỳ sự sắp xếp nào đều bằng phần tử cuối cùng trừ đi phần tử đầu tiên. Giá trị lớn nhất có thể thu được bằng cách chọn mức tối đa toàn cục làm phần tử cuối cùng và mức tối thiểu toàn cục làm phần tử đầu tiên. Sự khác biệt của họ chính xác là$\max(a)-\min(a)$, đó là vẻ đẹp lớn nhất có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(max(a) - min(a))
```Việc thực hiện tuân theo quan sát toán học trực tiếp. 

Đối với mỗi trường hợp thử nghiệm, mảng được đọc một lần. Tích hợp sẵn của Python`max()`Và`min()`các hàm quét mảng và trả về giá trị lớn nhất và nhỏ nhất. Sự khác biệt của họ là câu trả lời. 

Không có điều kiện biên phức tạp. Mảng có độ dài bằng hai hoạt động một cách tự nhiên vì câu trả lời đơn giản là sự khác biệt giữa phần tử lớn hơn và phần tử nhỏ hơn. Mảng có giá trị giống hệt nhau cũng hoạt động vì giá trị tối đa và tối thiểu bằng nhau, tạo ra số 0. 

Các giá trị có thể lớn như$10^9$, nhưng số nguyên Python dễ dàng xử lý chúng mà không lo tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:```
2 1 3
```| Bước | Giá trị | 
| --- | --- | 
| Tối thiểu | 1 | 
| Tối đa | 3 | 
| Trả lời | 3 - 1 = 2 | 

Đầu ra:```
2
```Điều này thể hiện ý tưởng cốt lõi. Mặc dù có thể sắp xếp lại một số thứ nhưng vẻ đẹp tối đa chỉ đơn giản là sự khác biệt giữa giá trị lớn nhất và nhỏ nhất. 

### Ví dụ 2 

Mảng đầu vào:```
100 54 80 43 90
```| Bước | Giá trị | 
| --- | --- | 
| Tối thiểu | 43 | 
| Tối đa | 100 | 
| Trả lời | 100 - 43 = 57 | 

Đầu ra:```
57
```Một sự sắp xếp tối ưu là:```
43 54 80 90 100
```vẻ đẹp của ai$$(54-43)+(80-54)+(90-80)+(100-90)=57.$$Dấu vết xác nhận rằng chỉ có giá trị nhỏ nhất và lớn nhất mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần quét để tìm giá trị tối thiểu và tối đa | 
| Không gian |$O(1)$| Chỉ có một vài biến ngoài mảng đầu vào | 

Với$n \le 100$và tối đa 500 trường hợp thử nghiệm, giải pháp này chạy thoải mái trong giới hạn. Tổng công việc chỉ có vài chục nghìn thao tác. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans.append(str(max(a) - min(a)))

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
3
2 1 3
3
69 69 69
5
100 54 80 43 90
4
3 4 3 3
2
2 1
"""
) == """2
0
57
1
1"""

# minimum size
assert run(
"""1
2
1 10
"""
) == "9"

# all equal
assert run(
"""1
5
7 7 7 7 7
"""
) == "0"

# already sorted
assert run(
"""1
6
1 2 3 4 5 6
"""
) == "5"

# large values
assert run(
"""1
3
1000000000 1 999999999
"""
) == "999999999"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 10`|`9`| Kích thước mảng hợp lệ tối thiểu | 
|`7 7 7 7 7`|`0`| Tất cả các giá trị bằng nhau | 
|`1 2 3 4 5 6`|`5`| Đã sắp xếp đầu vào | 
|`1000000000 1 999999999`|`999999999`| Giá trị số nguyên lớn | 

## Vỏ cạnh 

### Tất cả các phần tử đều bằng nhau 

đầu vào:```
1
3
5 5 5
```Thuật toán tính toán: 

| Số lượng | Giá trị | 
| --- | --- | 
| Tối thiểu | 5 | 
| Tối đa | 5 | 
| Trả lời | 0 | 

Đầu ra:```
0
```Mọi sự sắp xếp lại có thể đều tạo ra các giá trị mảng giống nhau, do đó mọi khác biệt liên tiếp sẽ bị loại bỏ. 

### Mảng có độ dài hai 

đầu vào:```
1
2
1 10
```Thuật toán tính toán: 

| Số lượng | Giá trị | 
| --- | --- | 
| Tối thiểu | 1 | 
| Tối đa | 10 | 
| Trả lời | 9 | 

Đầu ra:```
9
```Chỉ với hai yếu tố, vẻ đẹp bao gồm chính xác một điểm khác biệt, được tối đa hóa bằng cách đặt giá trị nhỏ hơn trước. 

### Mảng chưa được sắp xếp với các giá trị lặp lại 

đầu vào:```
1
4
3 4 3 3
```Thuật toán tính toán: 

| Số lượng | Giá trị | 
| --- | --- | 
| Tối thiểu | 3 | 
| Tối đa | 4 | 
| Trả lời | 1 | 

Đầu ra:```
1
```Các giá trị lặp lại không làm thay đổi đối số. Tổng kính thiên văn vẫn giảm xuống điểm trừ cuối cùng trước tiên, do đó chỉ có vấn đề tối thiểu và tối đa toàn cầu.
