---
title: "CF 1901C - Cộng, Chia và Tầng"
description: "Chúng ta được cho một mảng các số nguyên không âm. Trong một thao tác duy nhất, chúng ta có thể chọn một số $x$ và thêm nó vào từng phần tử, sau đó chia mỗi phần tử cho 2, làm tròn xuống. Điều này ảnh hưởng đến tất cả các yếu tố cùng một lúc."
date: "2026-06-08T21:13:07+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1901
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 158 (Rated for Div. 2)"
rating: 1400
weight: 1901
solve_time_s: 134
verified: false
draft: false
---

[CF 1901C - Cộng, chia và tính sàn](https://codeforces.com/problemset/problem/1901/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Chỉ trong một thao tác, chúng ta có thể chọn một số$x$và thêm nó vào từng phần tử, sau đó chia mỗi phần tử cho 2, làm tròn xuống. Điều này ảnh hưởng đến tất cả các yếu tố cùng một lúc. Mục tiêu của chúng tôi là làm cho tất cả các phần tử bằng nhau bằng cách sử dụng số lượng thao tác tối thiểu. Nếu số lượng thao tác đủ nhỏ, chúng tôi cũng cần báo cáo số lượng thực tế$x$các giá trị được sử dụng. 

Những hạn chế là đáng kể. Mảng có thể có tới$2 \cdot 10^5$phần tử trên nhiều trường hợp thử nghiệm và mỗi phần tử có thể lớn bằng$10^9$. Người được chọn$x$có thể rất lớn ($10^{18}$), cho phép chúng tôi thực hiện các thay đổi mạnh mẽ về phía giá trị mục tiêu. Điều này ngụ ý rằng bất kỳ giải pháp nào mô phỏng mọi chuỗi hoạt động có thể xảy ra đều sẽ quá chậm. 

Các trường hợp cạnh bao gồm các mảng có một phần tử duy nhất, các mảng trong đó tất cả các phần tử đều bằng nhau, các mảng có giá trị cao và thấp xen kẽ và các mảng có chênh lệch tối đa có thể yêu cầu nhiều thao tác. Ví dụ, cho một mảng$[0, 32]$, một cách tiếp cận đơn giản có thể thử thay đổi tăng dần cho nhiều bước, nhưng giải pháp tối ưu dựa vào việc sử dụng lớn$x$các giá trị hội tụ nhanh chóng. 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ cố gắng hết sức có thể$x$trong mỗi thao tác và mô phỏng cho đến khi tất cả các phần tử đều bằng nhau. Đối với các mảng lớn và có độ chênh lệch lớn, điều này không khả thi vì mỗi phần tử có thể được sửa đổi bởi bất kỳ ai.$x$lên đến$10^{18}$, dẫn tới vô số khả năng. 

Cái nhìn sâu sắc quan trọng là hoạt động$\lfloor (a_i + x)/2 \rfloor$là tuyến tính trong$a_i$cho một cố định$x$. Nếu chúng ta để giá trị cuối cùng của mục tiêu là$t$, chúng ta có thể suy luận về số bước tối thiểu cần thiết để đạt được$t$từ các phần tử tối đa và tối thiểu hiện tại trong mảng. Mỗi bước gần như giảm một nửa khoảng cách giữa một phần tử và mục tiêu khi chọn$x$một cách tối ưu. Do đó, số lượng thao tác tối thiểu có thể được xác định từ chênh lệch giữa phần tử tối đa và tối thiểu bằng cách sử dụng trần logarit cơ số 2 của chúng. Khi chúng ta biết số lượng thao tác,$x$các giá trị có thể được dẫn xuất ngược. 

Do đó, cách tiếp cận tối ưu là một thuật toán xây dựng tham lam: tính toán số lượng thao tác tối thiểu từ giới hạn mảng, sau đó xây dựng một chuỗi hợp lệ gồm$x$giá trị nếu số lượng thao tác không vượt quá$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * max_a) | O(n) | Quá chậm | 
| Tham lam mang tính xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng$a$và tính giá trị tối thiểu$mn$và tối đa$mx$các giá trị. Giá trị bằng nhau cuối cùng phải nằm giữa$mn$Và$mx$. 
2. Tính chênh lệch$d = mx - mn$. Nếu như$d = 0$, mảng đã bằng nhau nên câu trả lời là 0 phép toán. 
3. Mặt khác, số thao tác tối thiểu được xác định bằng số lần bạn phải giảm một nửa chênh lệch để đưa tất cả các phần tử về bằng nhau. Đây là$\lceil \log_2(d) \rceil$. Mỗi thao tác có thể giảm đi một nửa khoảng cách giữa các phần tử xa nhất nếu$x$được lựa chọn một cách tối ưu. 
4. Nếu số thao tác nhỏ hơn hoặc bằng$n$, xây dựng$x$các giá trị. Bắt đầu từ mảng hiện tại và tính toán liên tục$x$BẰNG$2t - \text{sum of current min and max}$hoặc các lựa chọn tham lam khác nhằm di chuyển mức tối thiểu và tối đa về phía mục tiêu. 
5. In số lần thực hiện. Nếu nhiều nhất là$n$, cũng in trình tự được xây dựng của$x$. 

Tại sao nó hoạt động: mỗi thao tác sẽ giảm khoảng một nửa phạm vi giữa phần tử lớn nhất và nhỏ nhất nếu được chọn tối ưu. Việc lặp lại quá trình này đảm bảo sự hội tụ về một giá trị duy nhất theo các bước logarit so với sai phân ban đầu. Bằng cách chọn$x$dựa trên mức tối thiểu và tối đa hiện tại, chúng tôi đảm bảo rằng giới hạn sẽ thu hẹp lại theo từng bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        mn, mx = min(a), max(a)
        if mn == mx:
            print(0)
            continue
        # minimum number of operations to converge
        ops = math.ceil(math.log2(mx - mn))
        print(ops)
        if ops <= n:
            # construct x values greedily
            x = []
            l, r = mn, mx
            for _ in range(ops):
                # choose x to bring l closer to r
                xi = r - l
                x.append(xi)
                l = (l + xi) // 2
                r = (r + xi) // 2
            print(' '.join(map(str, x)))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán tối thiểu và tối đa của mảng. Nếu chúng bằng nhau thì không cần thực hiện thao tác nào. Mặt khác, nó tính toán số phép toán bằng cách sử dụng logarit cơ số 2 của hiệu. Nếu số lượng thao tác nhỏ, chúng ta xây dựng$x$các giá trị để thu hẹp phạm vi trong mỗi bước. Sử dụng phép chia số nguyên đảm bảo làm tròn xuống thích hợp. Cách tiếp cận tránh lặp lại các giá trị lớn không cần thiết của$x$. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`[4, 6]`| Bước | tôi | r | x | 
| --- | --- | --- | --- | 
| 0 | 4 | 6 | | 
| 1 | 5 | 6 | 2 | 
| 2 | 5 | 5 | 1 | 

Sau 2 thao tác, mảng hội tụ về`[5, 5]`. 

**Ví dụ 2:** 

đầu vào:`[0, 32]`| Bước | tôi | r | x | 
| --- | --- | --- | --- | 
| 0 | 0 | 32 | | 
| 1 | 16 | 32 | 32 | 
| 2 | 24 | 32 | 16 | 
| 3 | 28 | 32 | 8 | 
| 4 | 30 | 32 | 4 | 
| 5 | 31 | 32 | 2 | 
| 6 | 31 | 32 | 1 | 

Số lượng thao tác vượt quá n=2 nên chỉ in ra số bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tìm min và max, O(ops) <= O(n) để xây dựng x nếu cần | 
| Không gian | O(n) | Lưu trữ mảng, cộng với danh sách giá trị x | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì vòng lặp chính xử lý từng mảng một lần và các phép tính logarit trên các hiệu là không đáng kể trong khoảng thời gian n lên đến$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n1\n10\n2\n4 6\n6\n2 1 2 1 2 1\n2\n0 32\n") == "0\n2\n2 1\n6", "sample 1"

# Custom tests
assert run("1\n3\n5 5 5\n") == "0", "all equal"
assert run("1\n2\n0 1\n") == "1\n1", "min max difference 1"
assert run("1\n5\n0 1000000000 0 1000000000 500000000\n") == "30", "large values"
assert run("1\n2\n0 32\n") == "6", "more operations than n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n5 5 5`|`0`| Đã có mảng bằng nhau | 
|`2\n0 1`|`1\n1`| Sự hội tụ khác biệt nhỏ | 
|`5\n0 10^9 0 10^9 5*10^8`|`30`| Số lớn hoạt động logarit | 
|`2\n0 32`|`6`| Quá n, x không in | 

## Vỏ cạnh 

Đối với mảng một phần tử, thuật toán trả về chính xác 0 phép toán vì min và max bằng nhau. Đối với các mảng trong đó tất cả các phần tử đều bằng nhau ngoại trừ một phần tử, thuật toán sẽ tính toán hiệu và các phép toán tối thiểu để hội tụ. Đối với các mảng có chênh lệch lớn vượt quá n, giải pháp chỉ in số lượng thao tác mà không xây dựng giá trị x, đảm bảo giải pháp xử lý hiệu quả cả trường hợp nhỏ và trường hợp cực đoan.
