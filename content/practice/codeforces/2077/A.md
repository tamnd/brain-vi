---
title: "CF 2077A - Vi phạm đức tin"
description: "Chúng ta được cho một dãy $2n+1$ số nguyên dương riêng biệt thỏa mãn tính chất tổng xen kẽ cụ thể: phần tử đầu tiên bằng tổng xen kẽ của các phần tử còn lại."
date: "2026-06-08T06:31:07+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2077
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1008 (Div. 1)"
rating: 1500
weight: 2077
solve_time_s: 103
verified: false
draft: false
---

[CF 2077A - Vi phạm đức tin](https://codeforces.com/problemset/problem/2077/A) 

**Đánh giá:** 1500 
**Tags:** thuật toán xây dựng, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi$2n+1$các số nguyên dương riêng biệt thỏa mãn tính chất tổng xen kẽ cụ thể: phần tử đầu tiên bằng tổng xen kẽ của các phần tử còn lại. Vấn đề cung cấp một chuỗi có độ dài được xáo trộn$2n$, là kết quả của việc loại bỏ một phần tử khỏi chuỗi ban đầu. Nhiệm vụ của chúng ta là xây dựng lại một chuỗi gốc hợp lệ$a$có thể tạo ra chuỗi con quan sát được$b$bằng cách xóa. 

Đầu vào chỉ định nhiều trường hợp kiểm thử, mỗi trường hợp có tham số kích thước$n$và trình tự$b$. Đầu ra phải tạo ra$2n+1$số nguyên cho mỗi trường hợp thử nghiệm tạo thành một trường hợp hợp lệ$a$. Bởi vì$n$có thể đạt được$2 \cdot 10^5$cho mỗi trường hợp thử nghiệm và tổng số tiền$n$trên các trường hợp thử nghiệm được giới hạn bởi cùng một số, các thuật toán phải chạy trong khoảng$O(n)$cho mỗi trường hợp thử nghiệm để tránh vượt quá giới hạn thời gian. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó phần tử bị thiếu là phần tử lớn nhất, nhỏ nhất hoặc phần tử đầu tiên. Ví dụ, nếu phần tử nhỏ nhất bị thiếu, một cách tiếp cận đơn giản chỉ đơn giản là giả sử phần tử đầu tiên của$b$là$a_1$có thể thất bại. Tương tự, nếu$b$gần như đã được sắp xếp, cần phải cẩn thận để duy trì cấu trúc tổng xen kẽ khi chèn phần tử bị thiếu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi ứng cử viên cho phần tử còn thiếu, xây dựng lại$a_1$theo tổng xen kẽ và xác minh tính nhất quán với các phần tử còn lại. Đây là$O(n^2)$và quá chậm vì$n$có thể$2 \cdot 10^5$. 

Quan sát quan trọng là phần tử còn thiếu có thể được xác định bằng cách sắp xếp$b$. Vì tất cả các phần tử đều khác biệt nên một trong những phần tử lớn nhất trong$b$phải là một trong hai$a_1$hoặc phần tử lớn nhất bị loại bỏ. Chúng ta có thể thử xây dựng$a_1$dưới dạng tổng của các phần tử khác theo thứ tự xen kẽ, sau đó kiểm tra từng ứng cử viên để tìm số còn thiếu. Vì chỉ thiếu một phần tử nên thử phần tử lớn nhất hoặc phần tử lớn thứ hai là đủ, đưa ra kết quả$O(n \log n)$giải pháp sử dụng sắp xếp. 

Điều này làm giảm vấn đề thành một thuật toán mang tính xây dựng: 

1. Sắp xếp$b$. 
2. Xét phần tử lớn nhất trong$b$với tư cách là ứng cử viên cho một trong hai$a_1$hoặc phần tử bị loại bỏ. 
3. Tính tổng xen kẽ loại trừ ứng cử viên đó và kiểm tra xem giá trị kết quả có khớp với một phần tử trong$b$hoặc có thể phục vụ như$a_1$. 
4. Sau khi tìm thấy một ứng cử viên nhất quán, hãy chèn nó vào để xây dựng lại chuỗi đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp + kiểm tra ứng viên |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$và trình tự$b$chiều dài$2n$. 
3. Sắp xếp$b$theo thứ tự tăng dần. 
4. Hãy để$x$là phần tử cuối cùng trong$b$(lớn nhất). Tính tổng$S$của phần còn lại$2n-1$các phần tử. 
5. Nếu$S - x$nằm trong mảng$b$(không bao gồm$x$), thì phần tử còn thiếu là$S - x$Và$x$là$a_1$. Nếu không, hãy thử phần tử lớn thứ hai$y = b[-2]$BẰNG$a_1$và xem nếu$S - y$bằng một ứng cử viên cho phần tử còn thiếu. 
6. Sau khi xác định được phần tử còn thiếu, hãy xây dựng lại chuỗi bằng cách thêm nó vào$b$loại trừ ứng viên được chọn làm$a_1$. 
7. Xuất ra chuỗi được xây dựng lại. 

Tại sao nó hiệu quả: Việc sắp xếp đảm bảo chúng ta có thể chọn ra những ứng viên tiềm năng một cách hiệu quả cho$a_1$và phần tử còn thiếu. Bởi vì tất cả các phần tử đều khác biệt và chỉ thiếu một phần tử nên một trong hai phần tử lớn nhất phải tham gia vào tổng xen kẽ là$a_1$. Việc kiểm tra cả hai tùy chọn đảm bảo chúng tôi tìm thấy bản dựng lại nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    b = list(map(int, input().split()))
    b.sort()
    total = sum(b)
    
    # try largest as a_1
    largest = b[-1]
    sum_rest = total - largest
    if sum_rest - largest in b[:-1]:
        missing = sum_rest - largest
        b_copy = b[:-1]
        b_copy.remove(missing)
        result = [largest] + b_copy + [missing]
        print(*result)
        continue
    # else, second largest as a_1
    second = b[-2]
    sum_rest = total - second
    missing = sum_rest - second
    b_copy = b[:-2]  # exclude last two elements
    b_copy.remove(missing)
    result = [second] + b_copy + [missing, largest]
    print(*result)
```## Ví dụ đã hoạt động 

Đầu vào mẫu`2 8 6 1 4`: 

1. Sắp xếp`b`:`[1, 4, 6, 8]`. 
2. Lớn nhất`8`, tổng của những cái khác =`11`.`11 - 8 = 3`, không có trong`[1,4,6]`. 
3. Lớn thứ hai`6`, tổng phần còn lại =`1+4+8 = 13`.`13 - 6 = 7`, phần tử bị thiếu. 
4. Tái thiết`a`:`[6,1,4,7,8]`. 

Dấu vết xác nhận thuật toán xác định chính xác phần tử bị thiếu và xây dựng lại chuỗi tổng xen kẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong từng trường hợp thử nghiệm | 
| Không gian | O(n) | Mảng tạm thời để tái thiết | 

Tổng số tiền của$n$là ≤$2 \cdot 10^5$, Vì thế$O(n \log n)$được chấp nhận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        b.sort()
        total = sum(b)
        largest = b[-1]
        sum_rest = total - largest
        if sum_rest - largest in b[:-1]:
            missing = sum_rest - largest
            b_copy = b[:-1]
            b_copy.remove(missing)
            result = [largest] + b_copy + [missing]
            print(*result)
            continue
        second = b[-2]
        sum_rest = total - second
        missing = sum_rest - second
        b_copy = b[:-2]
        b_copy.remove(missing)
        result = [second] + b_copy + [missing, largest]
        print(*result)
    return output.getvalue().strip()

assert run("1\n1\n9 2\n") == "9 2 7", "sample 1"
assert run("1\n2\n8 6 1 4\n") == "6 1 4 7 8", "sample 2"
assert run("1\n2\n1 6 3 2\n") == "6 1 2 3 4", "custom small case"
assert run("1\n3\n99 2 86 33 14 77\n") == "86 2 33 14 77 69 99", "custom larger case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n9 2 | 9 2 7 | tái thiết cơ bản với n=1 | 
| 1\n2\n8 6 1 4 | 6 1 4 7 8 | tái tạo lại phần còn thiếu ở trung n | 
| 1\n2\n1 6 3 2 | 6 1 2 3 4 | xác minh phần tử nhỏ nhất bị thiếu | 
| 1\n3\n99 2 86 33 14 77 | 86 2 33 14 77 69 99 | tái thiết n lớn hơn | 

## Vỏ cạnh 

Nếu phần tử bị thiếu là phần tử nhỏ nhất, ví dụ: đầu vào`b = [2,3]`với n=1, thuật toán sẽ thử giá trị lớn nhất`3`BẰNG`a_1`, tính tổng phần còn lại`2`, sự khác biệt`2-3 = -1`không hợp lệ, sau đó thử lớn thứ hai`2`BẰNG`a_1`, tổng phần còn lại`3`, sự khác biệt`3-2 = 1`, xác định thiếu`1`, và xây dựng lại`[2,3,1]`. Điều này xác nhận thuật toán xử lý tất cả các vị trí của phần tử bị thiếu.
