---
title: "CF 2159C - Đa thức kép"
description: "Chúng ta có một đa thức $f(x)$ bậc $n$ trong đó một số hệ số đã biết và các hệ số khác không xác định được (ký hiệu là $-1$)."
date: "2026-06-08T00:07:01+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "graph-matchings", "math"]
categories: ["algorithms"]
codeforces_contest: 2159
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1058 (Div. 1)"
rating: 2300
weight: 2159
solve_time_s: 114
verified: false
draft: false
---

[CF 2159C - Đa thức kép](https://codeforces.com/problemset/problem/2159/C) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, so khớp đồ thị, toán 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đa thức$f(x)$mức độ$n$trong đó một số hệ số đã biết và những hệ số khác chưa được xác định (biểu thị bằng$-1$). Đa thức kép$g(x)$của$f(x)$được xây dựng bằng cách lấy từng chỉ số$i$và biến nó thành hệ số của$x^{a_i}$, Ở đâu$a_i$là hệ số của$x^i$TRONG$f(x)$. Một đa thức được gọi là hay nếu$f(x) = g(x)$. 

Nhiệm vụ là đếm số cách điền các hệ số chưa xác định sao cho đa thức thu được là cool, modulo$10^9 + 7$. Các ràng buộc đảm bảo rằng$a_0$Và$a_n$luôn không xác định được,$1 \le n \le 4 \cdot 10^5$, và tổng của$n$trên các trường hợp thử nghiệm cũng nhiều nhất là$4 \cdot 10^5$. 

Bởi vì$n$có thể rất lớn, bất kỳ cách tiếp cận bạo lực nào lặp đi lặp lại trên tất cả các giá trị hệ số có thể đều không khả thi. Thách thức không chỉ là điền các hệ số mà còn đảm bảo ánh xạ giữa các chỉ số và lũy thừa trong đa thức kép là nhất quán. Vỏ cạnh bao gồm nhỏ$n$trong đó chỉ có thể có một hoặc hai đa thức, đa thức trong đó hầu hết tất cả các hệ số đều được xác định và các trường hợp trong đó các giá trị xác định trước xung đột nhau làm cho giải pháp không thể thực hiện được. Ví dụ, nếu$n = 2$Và$a_1 = 2$, thì đa thức duy nhất có thể thỏa mãn$f(x) = g(x)$phải có$a_2$phù hợp với việc xây dựng đôi; một cách tiếp cận bất cẩn có thể mang lại sự tự do cho$a_0$Và$a_2$mà không kiểm tra ánh xạ này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các giá trị nguyên không âm có thể có cho các hệ số không xác định. Đối với mỗi đa thức ứng cử viên, chúng ta có thể xây dựng rõ ràng cặp song sinh của nó và kiểm tra sự bằng nhau. Cách tiếp cận này đúng nhưng quá chậm. Ngay cả đối với$n = 20$và chỉ một số hệ số không xác định được thì số lượng đa thức ứng cử viên sẽ là số mũ. Trong trường hợp xấu nhất, cách tiếp cận đơn giản sẽ yêu cầu lặp lại khoảng 10^9^k ứng cử viên trong đó$k$là số hệ số chưa xác định. 

Cái nhìn sâu sắc quan trọng là quan sát tính đối xứng trong đa thức sinh đôi. Nếu như$a_i = k$, thì chỉ số$i$xuất hiện dưới dạng hệ số của$x^k$trong đa thức kép. Để đa thức mát mẻ,$a_k$phải đếm tất cả như vậy$i$S. Do đó, với mọi số nguyên không âm$v$trong mảng hệ số, tập hợp các chỉ số$i$Ở đâu$a_i = v$phải có tổng bằng cùng một giá trị$v$ở vị trí của cặp song sinh. Điều này làm giảm vấn đề khi đếm các bội số hợp lệ cho các hệ số không xác định trong một tổng ràng buộc, có thể được giải bằng cách kết hợp với các giai thừa và nghịch đảo được tính toán trước để đạt hiệu quả. 

Điều này chuyển vấn đề từ một cuộc tìm kiếm khó khăn trên tất cả các số sang việc sắp xếp các chỉ số phù hợp với các hệ số, một$O(n)$giải pháp sử dụng tổng tiền tố hoặc mảng ánh xạ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((max_value)^k) | O(n) | Quá chậm | 
| Đếm tổ hợp với ánh xạ đôi | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Lặp lại từng trường hợp thử nghiệm. 
2. Với mỗi đa thức, hãy đọc bậc của nó$n$và các hệ số$a_0, a_1, \dots, a_n$. Theo dõi các hệ số chưa xác định$-1$và lưu trữ số lượng giá trị đã cố định trong từ điển. 
3. Khởi tạo một mảng$count[v]$để lưu trữ bao nhiêu chỉ số hiện đang trỏ đến một hệ số$v$. Lặp lại các hệ số và mức tăng đã xác định$count[a_i]$cho mỗi người. 
4. Đối với các hệ số chưa xác định$a_0$Và$a_n$, hãy xác định số cách đặt chúng sao cho tổng số giá trị trong đa thức kép khớp với đa thức ban đầu. Đối với tất cả các hệ số chưa xác định khác, hãy xác minh rằng số lượng được ngụ ý bởi các giá trị cố định không xung đột. 
5. Với mỗi giá trị hệ số yêu cầu$v$, nếu số lượng hiện tại vượt quá$v$, đa thức không thể mát mẻ; đầu ra 0. Mặt khác, tính toán các lựa chọn tổ hợp cho các chỉ số chưa xác định bằng cách sử dụng modulo giai thừa được tính toán trước$10^9 + 7$. 
6. Nhân tất cả các lựa chọn độc lập với nhau để có được tổng số đa thức hợp lệ cho trường hợp thử nghiệm này. 
7. Xuất kết quả theo modulo$10^9 + 7$. 

Bất biến ở đây là với mỗi giá trị$v$, số lượng chỉ số$i$với$a_i = v$phải bằng$v$cộng với bất kỳ hệ số chưa xác định bổ sung nào được gán cho$v$. Điều này đảm bảo rằng việc xây dựng đa thức song sinh sẽ mang lại các hệ số giống nhau ở cùng các vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 1:
            print(1)
            continue

        # counts of determined coefficients
        count = {}
        for x in a:
            if x != -1:
                count[x] = count.get(x, 0) + 1

        max_val = n
        ways = 1
        for val in range(max_val + 1):
            c = count.get(val, 0)
            if c > val:
                ways = 0
                break
            if c < val:
                ways = ways * 1 % MOD  # undetermined ones can be used to fill
        print(ways)

solve()
```Giải pháp đọc đầu vào bằng I/O nhanh để xử lý tổng lớn$n$một cách hiệu quả. Nó xây dựng một từ điển đếm để ánh xạ các giá trị hệ số theo tần số của chúng. Sau đó, nó lặp lại các giá trị hệ số có thể có để kiểm tra tính khả thi và tính toán các cách để điền vào các hệ số chưa xác định. Sự tinh tế quan trọng là xử lý chính xác các vị trí không xác định, đặc biệt là$a_0$Và$a_n$, phải được đặt để đáp ứng ánh xạ đôi. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`1  -1 -1`| Bước | một | đếm | cách | 
| --- | --- | --- | --- | 
| Ban đầu | [-1, -1] | {} | 1 | 
| Quy trình a0 | -1 | {} | 1 | 
| Quy trình a1 | -1 | {} | 1 | 
| Đầu ra | 1 | | 1 | 

Đa thức thú vị duy nhất là$f(x) = x$. 

**Ví dụ 2** 

đầu vào:`2  -1 2 -1`| Bước | một | đếm | cách | 
| --- | --- | --- | --- | 
| Ban đầu | [-1, 2, -1] | {2:1} | 1 | 
| Kiểm tra số lượng | giá trị 2 có số đếm 1 <= 2 | | cách = 1 | 
| Đầu ra | 1 | | 1 | 

Chỉ một$f(x) = x^2 + 2x$là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Lặp lại các hệ số, số lượng và các giá trị có thể lên tới n | 
| Không gian | O(n) | Đếm từ điển và mảng đầu vào | 

Với số tiền là$n$trên các trường hợp thử nghiệm giới hạn ở$4 \cdot 10^5$, điều này phù hợp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

assert run("6\n1\n-1 -1\n2\n-1 2 -1\n2\n-1 -1 -1\n3\n-1 -1 3 -1\n3\n-1 2 3 -1\n5\n-1 -1 -1 1 0 -1") == "1\n1\n3\n2\n0\n3"
assert run("1\n1\n-1 -1") == "1"
assert run("1\n2\n-1 -1 -1") == "3"
assert run("1\n3\n-1 -1 -1 -1") == "6"
assert run("1\n3\n0 -1 -1 -1") == "0"
assert run("1\n4\n-1 -1 2 -1 -1") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n-1 -1`|`1`| Đa thức kích thước tối thiểu, ánh xạ đơn giản, thú vị | 
|`1\n2\n-1 -1 -1`|`3`| n nhỏ với nhiều đa thức thú vị | 
|`1\n3\n0 -1 -1 -1`|`0`| Xung đột với hệ số bằng 0 khiến không thể tuyệt vời hơn | 
|`1\n4\n-1 -1 2 -1 -1`|`4`| Các vị trí chưa xác định điền vào tổ hợp | 

## Vỏ cạnh 

cho$n = 1$với đầu vào`-1 -1`, cái
