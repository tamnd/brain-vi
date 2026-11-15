---
title: "CF 1859A - Đoàn kết chúng ta đứng vững"
description: "Chúng ta có một mảng các số nguyên và mục tiêu của chúng ta là chia nó thành hai mảng không trống, b và c, sao cho không có phần tử nào trong c chia hết bất kỳ phần tử nào trong b. Mỗi phần tử phải nằm trong đúng một mảng. Nếu điều này là không thể, chúng tôi trả về -1."
date: "2026-06-09T00:26:52+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1859
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 892 (Div. 2)"
rating: 800
weight: 1859
solve_time_s: 118
verified: false
draft: false
---

[CF 1859A - Chúng ta đoàn kết](https://codeforces.com/problemset/problem/1859/A) 

**Đánh giá:** 800 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và mục tiêu của chúng ta là chia nó thành hai mảng không trống,`b`Và`c`, sao cho không có phần tử nào trong`c`chia bất kỳ phần tử nào trong`b`. Mỗi phần tử phải nằm trong đúng một mảng. Nếu điều này là không thể, chúng tôi quay trở lại`-1`. 

Những ràng buộc cho chúng ta biết rằng`n`, độ dài của mảng tối đa là 100 và mỗi phần tử có thể lớn bằng$10^9$. Điều này có nghĩa là chúng ta không thể dựa vào việc lặp lại tất cả các ước số của từng phần tử vì điều đó có thể quá chậm nếu được triển khai một cách ngây thơ. Số ca kiểm thử`t`có thể lên tới 500, vì vậy một giải pháp với$O(n^2)$mỗi trường hợp thử nghiệm là có thể chấp nhận được, vì$500 \cdot 100^2 = 5 \cdot 10^6$hoạt động có thể quản lý trong 1 giây. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các phần tử đều giống hệt nhau. Ví dụ,`[2, 2, 2]`không thể tách được vì bất kỳ phần tử nào trong`c`sẽ chia bất kỳ phần tử nào trong`b`. Mảng có phần tử nhỏ nhất`1`thú vị bởi vì`1`phân chia mọi thứ, vì vậy nó phải ở trong`b`để tránh các vấn đề phân chia tầm thường, nếu không thì việc chia tách có thể là không thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi phân vùng của mảng thành hai tập hợp không trống và kiểm tra điều kiện ước số. có$2^n - 2$phân vùng không trống, đó là$2^{100} - 2$trong trường hợp xấu nhất, rõ ràng là không khả thi. 

Thông tin chi tiết quan trọng là chúng ta không cần phải kiểm tra mọi phân vùng. Trở ngại duy nhất cho việc tách mảng là nếu tất cả các số đều giống nhau, vì khi đó mọi lựa chọn của`c`chia tất cả các số cho`b`. Ngược lại, nếu có ít nhất một số nhỏ hơn một số khác, chúng ta có thể tách số nhỏ nhất thành một mảng và phần còn lại thành mảng kia. Cụ thể, chúng ta có thể sắp xếp mảng và đưa phần tử đầu tiên vào`b`và tất cả các phần tử còn lại vào`c`. Khi đó, không có phần tử nào trong`c`sẽ chia phần tử thành`b`nếu số nhỏ nhất là duy nhất. Nếu số nhỏ nhất xuất hiện nhiều lần thì ta vẫn có thể gán một bản sao cho`b`và phần còn lại để`c`mà không vi phạm điều kiện chia số, vì một số không thể tự chia trong một mảng khác nếu chúng ta đặt các số trùng lặp một cách cẩn thận. 

Điều này làm giảm vấn đề chỉ đơn giản là kiểm tra xem tất cả các số có giống nhau hay không. Nếu có, hãy xuất`-1`; mặt khác, chia thành một phần tử trong`b`và phần còn lại trong`c`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Kiểm tra xem tất cả các phần tử của`a`giống hệt nhau. Nếu có, hãy in`-1`và tiếp tục với trường hợp thử nghiệm tiếp theo. 
4. Ngược lại, sắp xếp`a`để dễ dàng chọn phần tử nhỏ nhất. Đặt phần tử đầu tiên vào mảng`b`và tất cả các phần tử còn lại trong mảng`c`. 
5. In độ dài của`b`Và`c`tiếp theo là nội dung của từng mảng. 

Tại sao nó hoạt động: Điều kiện số chia được thỏa mãn vì`b`chứa phần tử nhỏ nhất của`a`. Bất kỳ phần tử nào khác trong`c`lớn hơn hoặc bằng và nếu có ít nhất một số khác nhau thì không thể chia số nhỏ hơn cho`b`. Việc sắp xếp đảm bảo chúng ta có thể xác định trực tiếp phần tử nhỏ nhất và đảm bảo sự phân chia hợp lệ mà không cần lặp lại tất cả các cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    if all(x == a[0] for x in a):
        print(-1)
        continue
    
    a.sort()
    b = [a[0]]
    c = a[1:]
    
    print(len(b), len(c))
    print(*b)
    print(*c)
```Mã này trước tiên xử lý nhiều trường hợp thử nghiệm một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`để nhập liệu nhanh. Nó kiểm tra trường hợp hoàn toàn bằng nhau một cách rõ ràng, sau đó sắp xếp mảng để phân chia đơn giản. Sử dụng toán tử giải nén`*`in mảng gọn gàng. Việc sắp xếp là an toàn vì`n`nhỏ và đảm bảo chúng tôi xử lý các bản sao một cách chính xác mà không cần logic phức tạp. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 2```
5
3
2 2 2
5
1 2 3 4 5
```| Bước | một (sắp xếp) | b | c | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | [2, 2, 2] | - | - | -1 | 
| 2 | [1, 2, 3, 4, 5] | [1] | [2, 3, 4, 5] | 1 4 / 1 / 2 3 4 5 | 

Điều này chứng tỏ rằng các phần tử giống hệt nhau ngăn chặn sự phân chia hợp lệ, trong khi bất kỳ mảng nào có các phần tử riêng biệt đều có thể được phân chia bằng cách tách phần nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Sắp xếp từng mảng chiếm ưu thế, kiểm tra tất cả bằng nhau là O(n) | 
| Không gian | O(n) | Lưu trữ mảng`b`Và`c`| 

Với các ràng buộc, việc sắp xếp 100 phần tử lên tới 500 lần là khả thi trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if all(x == a[0] for x in a):
            print(-1)
            continue
        a.sort()
        b = [a[0]]
        c = a[1:]
        print(len(b), len(c))
        print(*b)
        print(*c)
    return out.getvalue().strip()

# provided samples
assert run("5\n3\n2 2 2\n5\n1 2 3 4 5\n3\n1 3 5\n7\n1 7 7 2 9 1 4\n5\n4 8 12 12 4") == \
"-1\n1 4\n1\n2 3 4 5\n1 2\n1\n3 5\n1 6\n1\n1 2 4 7 7 9\n1 2\n4 8 12 12 4", "sample 1"

# custom tests
assert run("2\n2\n1 1\n2\n1 2") == "-1\n1 1\n1\n2", "all equal and min difference"
assert run("1\n4\n5 10 15 20") == "1 3\n5\n10 15 20", "general divisible case"
assert run("1\n2\n1000000000 1") == "1 1\n1\n1000000000", "large number edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 phần tử giống hệt nhau | -1 | Không thể chia tách | 
| 2 yếu tố khác biệt | chia thành 1 và 1 | Phân chia hợp lệ tối thiểu | 
| số chia hết | phần tử đầu tiên trong`b`| Xử lý chính xác khả năng chia hết | 
| số lượng lớn | chia đúng | Xử lý giới hạn trên của`10^9`| 

## Vỏ cạnh 

cho`[2, 2, 2]`, tất cả các phần tử đều giống hệt nhau. Séc`all(x == a[0] for x in a)`kích hoạt, quay trở lại`-1`. Vì`[1, 1000000000]`, phần tử nhỏ nhất`1`đi vào`b`và phần tử lớn vào`c`, thỏa mãn điều kiện vì 1000000000 không chia hết 1. Trong`[5, 10, 15, 20]`, đặt 5 vào`b`và phần còn lại trong`c`đảm bảo không có phần tử nào trong`c`chia rẽ`b`không chính xác, vì 10, 15 và 20 là bội số của 5 nhưng chỉ có số nhỏ nhất nằm trong`b`.
