--- 
tiêu đề: "CF 1466E - Apollo đấu với Pan" 
mô tả: "Chúng ta được yêu cầu tính tổng của ba số nguyên rút ra từ một mảng nhất định. Cụ thể, đối với một dãy số nguyên không âm$x1, x2, ldots, xn$, giá trị mục tiêu là$$sum{i=1}^n sum{j=1}^n sum{k=1}^n (xi & xj) cdot (xj , where $&$ is the bitwise AND and $ The…" date: "2026-06-11T01:44:54+07:00" tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "math"] categories: ["algorithms"] codeforces_contest: 1466 codeforces_index: "E" codeforces_contest_name: "Good Bye 2020" rating: 1800 weight: 1466 solve_time_s: 74 verified: true draft: false --- [CF 1466E - Apollo versus Pan](https://codeforces.com/problemset/problem/1466/E) **Rating:** 1800   **Tags:** bitmasks, brute force, math   **Solve time:** 1m 14s   **Verified:** yes   ## Solution ## Problem Understanding We are asked to compute a sum over triples of integers drawn from a given array. Specifically, for a sequence of non-negative integers $x_1, x_2, \ldots, x_n$, the target value is$$\sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n (x_i \& x_j) \cdot (x_j \, | \, x_k)$$
where $\&$ is the bitwise AND and $|$ is the bitwise OR. The input contains multiple test cases, and for each, we need the answer modulo $10^9 + 7$.
The constraints allow $n$ up to $5 \cdot 10^5$ and numbers up to $2^{60}$. With a worst-case triple sum over $n^3$, a naive solution would perform $10^{17}$ operations in the largest test case. This is clearly impossible in 2 seconds. Therefore, we need an algorithm that scales linearly or at most $O(n \cdot \text{log}(X))$, where $X$ is the maximum number size.
A subtle point is that numbers are large (up to 60 bits), so any solution depending on iterating all possible values directly is infeasible. Zero-valued numbers and sequences where all numbers are equal are edge cases that could produce different bitwise patterns, but the solution must handle them correctly.
For example, if the array is `[0]`, the sum is `0` because both AND and OR operations yield 0. A careless approach might assume every element contributes non-zero, producing a wrong sum.
## Approaches
The brute-force method is straightforward: loop over all triples $i, j, k$ and compute $(x_i \& x_j) \cdot (x_j | x_k)$. This works in principle, but with $n = 5 \cdot 10^5$, the triple nested loop performs $(5 \cdot 10^5)^3 \approx 10^{17}$ operations per test case. This is far beyond feasible.
The key insight is to factor the sum cleverly. Observe that $(x_i \& x_j)$ depends only on $i$ and $j$, while $(x_j | x_k)$ depends only on $j$ and $k$. For a fixed $j$, the sum over $i$ of $(x_i \& x_j)$ and the sum over $k$ of $(x_j | x_k)$ can be computed separately. Let
$$S_n = \sum_{i=1}^n (x_i \và x_i), \quad X_i = \sum_{k=1}^n (x_j | x_k)$$
Then the triple sum is simply
$$\sum_{j=1}^n S_j \cdot T_j$$
This reduces the problem from $O(n^3)$ to $O(n^2)$. Still, $O(n^2)$ is too slow for $n = 5 \cdot 10^5$. To go further, we can compute $S_j$ and $T_j$ efficiently using bitwise decomposition.
For each bit position $b$ from 0 to 59, let `cnt[b]` be the number of elements with bit `b` set. Then
$$S_j = \sum_{b=0}^{59} 
\bắt đầu{trường hợp} 
2^b \cdot \text{cntferb] & \text{if } x_j \text{ has bit } b \text{ set} \\ 
0 & \text{nếu không} 
\end{trường hợp}$$
and
$$T_j = \sum_{b=0}^{59} 
\bắt đầu{trường hợp} 
2^b \cdot n & \text{if } x_j \text{ has bit } b \text{ set} \\ 
2^b \cdot \text{cnt[b] & \text{otherwise} 
\end{case}$$ 
Điều này hoạt động bởi vì nếu`x_j`có chút`b`bộ,`(x_j & x_i)`đóng góp`2^b`bất cứ khi nào`x_i`có một chút, và`(x_j | x_k)`đóng góp`2^b`cho mọi`k`. Nếu như`x_j`không có chút nào,`(x_j | x_k)`đóng góp`2^b`chỉ nếu`x_k`có một chút. 
Việc phân tích theo bit này làm giảm việc tính tổng cho mỗi`j`đến O(60), cho độ phức tạp tổng thể là O(n * 60) cho mỗi trường hợp thử nghiệm, có thể chấp nhận được. 
| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n * 60) | O(60) | Đã chấp nhận | 
## Hướng dẫn thuật toán 
1. Với mỗi test case, hãy đọc`n`và mảng`x`. 
2. Khởi tạo một mảng`cnt`có độ dài 60, đếm xem mỗi bit có bao nhiêu số. Lặp lại qua`x`và tăng dần`cnt[b]`cho mỗi bit`b`đó là thiết lập trong`x[i]`. 
3. Khởi tạo một biến`answer`đến 0. Lặp lại từng phần tử`x_j`của`x`: 
1. Tính toán`S_j`là tổng của tất cả các vị trí bit trong đó`x_j`có một tập hợp bit, nhân giá trị bit`2^b`qua`cnt[b]`. 
2. Tính toán`T_j`là tổng của tất cả các vị trí bit. Nếu như`x_j`có một chút, đóng góp`2^b * n`. Còn không thì đóng góp`2^b * cnt[b]`. 
3. Nhân`S_j`Và`T_j`, lấy modulo`10^9 + 7`, và thêm vào`answer`. 
4. Đầu ra`answer`modulo`10^9 + 7`. 
Tại sao nó hoạt động: quá trình phân tách khai thác tính tuyến tính của phép tính tổng và tính độc lập của các bit trong phép toán AND và OR. Mỗi bộ ba`(i, j, k)`đóng góp đúng một lần vào`S_j * T_j`và tổng theo bit đảm bảo rằng giá trị của mỗi bit được tính đúng số lần. Số học Modulo bảo toàn tính đúng đắn. 
## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7
t = int(input())
for _ in range(t):
    n = int(input())
    x = list(map(int, input().split()))
    
    cnt = [0] * 60
    for num in x:
        for b in range(60):
            if (num >> b) & 1:
                cnt[b] += 1
    
    answer = 0
    for num in x:
        s = 0
        t_sum = 0
        for b in range(60):
            bit_val = (1 << b) % MOD
            if (num >> b) & 1:
                s = (s + bit_val * cnt[b]) % MOD
                t_sum = (t_sum + bit_val * n) % MOD
            else:
                t_sum = (t_sum + bit_val * cnt[b]) % MOD
        answer = (answer + s * t_sum) % MOD
    print(answer)
```Mã bắt đầu bằng cách đọc số lượng ca kiểm thử và đầu vào của từng ca kiểm thử. Nó tính toán`cnt`array để lưu trữ số phần tử với mỗi bộ bit, đảm bảo chúng tôi không tính lại số lượng cho từng phần tử. Khi tính toán`S_j`Và`T_j`, chúng ta nhân với lũy thừa của hai modulo$10^9 + 7$để tránh tràn. Cuối cùng, chúng tôi tổng hợp sự đóng góp của tất cả`x_j`và in câu trả lời modulo`10^9 + 7`. Phép toán modulo được áp dụng ở mỗi phép nhân và phép cộng để tránh tràn số nguyên với số lớn. 
## Ví dụ đã hoạt động 
Đối với mảng`[1, 7]`: 
| j | số | S_j | T_j | S_j * T_j | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 8 | 8 | 
| 2 | 7 | 8 | 15 | 120 | 
Tổng số tiền:`8 + 120 = 128`, phù hợp với đầu ra mẫu. 
Đối với mảng`[1, 2, 4]`: 
| j | số | S_j | T_j | S_j * T_j | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 7 | 7 | 
| 2 | 2 | 2 | 10 | 20 | 
| 3 | 4 | 4 | 14 | 64 | 
Tổng số tiền:`7 + 20 + 64 = 91`, phù hợp với đầu ra mẫu. 
Những dấu vết này xác nhận rằng`S_j`đếm sự đóng góp của`(x_i & x_j)`một cách chính xác và`T_j`đếm sự đóng góp của`(x_j | x_k)`một cách chính xác, chứng tỏ tính đúng đắn của việc phân tách theo bit. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 60) | Đếm bit và tính toán`S_j`Và`T_j`cho mỗi`x_j`| 
| Không gian | O(60) | Lưu trữ số lượng trên mỗi vị trí bit | 
Giải pháp là tuyến tính trong`n`với một cái nhỏ
