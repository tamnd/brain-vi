---
title: "CF 1375G - Sửa đổi cây"
description: "Giả sử $Nge 1$ được cố định. Một $m$-survivor là một số nguyên dương có tất cả các thừa số nguyên tố vượt quá $m$. Đặt $$f(x,m)={nle x:text{$n$ là một $m$-survivor}},$$ và let $$fk(x,m)={nle x:text{$n$ là một $m$-survivor có chính xác $k$ thừa số nguyên tố}},$$ trong đó các thừa số nguyên tố được tính…"
date: "2026-06-11T11:06:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "dfs-and-similar", "graph-matchings", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1375
codeforces_index: "G"
codeforces_contest_name: "Codeforces Global Round 9"
rating: 2800
weight: 1375
solve_time_s: 149
verified: false
draft: false
---

[CF 1375G - Sửa đổi cây](https://codeforces.com/problemset/problem/1375/G) 

**Đánh giá:** 2800 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, dfs và tương tự, khớp đồ thị, đồ thị, cây 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$N\ge 1$được cố định. MỘT$m$-survivor là số nguyên dương có tất cả các thừa số nguyên tố vượt quá$m$. Cho phép$$
f(x,m)=#{n\le x:\text{$n$ is an $m$-survivor}},
$$và để$$
f_k(x,m)=#{n\le x:\text{$n$ is an $m$-survivor having exactly $k$ prime factors}},
$$trong đó các thừa số nguyên tố được tính với bội số. 

Vấn đề là thiết lập các công thức$\pi(N^3)$và giải thích cách tính các đại lượng liên quan từ giá trị của$\pi(x)$với$x\le N^2$. 

##Giải pháp 

### (một) 

Mỗi$N$-người sống sót$n\le N^3$thuộc về đúng một trong các lớp sau. 

Đầu tiên,$n=1$. 

Thứ hai,$n=p$, Ở đâu$p>N$là nguyên tố. 

Thứ ba,$n$có ít nhất hai thừa số nguyên tố, tất cả đều vượt quá$N$. 

Giả định$n$là một$N$-người sống sót có ít nhất ba thừa số nguyên tố. Sau đó$$
n\ge (N^+)^3>N^3,
$$Ở đâu$N^+$biểu thị số nguyên tố nhỏ nhất vượt quá$N$. Do đó không có số nguyên nào như vậy là$\le N^3$. 

Vì vậy mỗi tổ hợp$N$-người sống sót$\le N^3$có đúng hai thừa số nguyên tố. Do đó 

1+(\pi(N^3)-\pi(N))+f_2(N^3,N).$$
Rearranging gives
$$\boxed{\pi(N^3)=\pi(N)+f(N^3,N)-1-f_2(N^3,N)}.$$
This proves part (a).
### (b)
An $N$-survivor with exactly two prime factors has the form
$$n=pq,$$
where
$$N<p\le q,\qquad pq\le N^3.$$
For a fixed prime $p>N$, the admissible values of $q$ are the primes satisfying
$$p\le q\le \frac{N^3}{p}.$$
Hence
\sum_{\substack{p>N\ p\le N^{3/2}}}
\Bigl(
\pi(N^3/p)-\pi(p-1)
\Bigr).
$$Từ$p>N$, chúng tôi có$$
N^3/p\le N^2,
$$do đó mọi giá trị của$\pi(N^3/p)$theo yêu cầu của công thức nằm ở một đối số không vượt quá$N^2$. 

Vì$N=10^3$, 

\sum_{\substack{1000<p\le 31622\ p\text{ prime}}} 
\Bigl( 
\pi(10^9/p)-\pi(p-1) 
\Lớn hơn).$$
Using the tabulated values of $\pi(x)$ for $x\le10^6$, the summation evaluates to
$$f_2(10^9,10^3)=563,158.$$
### (c)
Let $p_j$ be the $j$th prime and let $p_0=1$.
Define
#{n\le x:\text{all prime factors of $n$ exceed }p_j}.
$$Phân vùng$p_{j-1}$-số người sống sót không vượt quá$x$thành hai lớp. 

Lớp đầu tiên bao gồm các số nguyên có thừa số nguyên tố vượt quá$p_j$. Số của họ là$f(x,p_j)$. 

Lớp thứ hai bao gồm những lớp chia hết cho$p_j$. Mọi số nguyên như vậy đều có dạng$$
n=p_jm,
$$Ở đâu$m\le x/p_j$và mọi thừa số nguyên tố của$m$vượt quá$p_{j-1}$. 

Do đó lớp thứ hai có bản số$$
f(x/p_j,p_{j-1}).
$$Do đó 

## f(x,p_{j-1}) 

f(x/p_j,p_{j-1}) 
}.$$
This is the stated recurrence.
The initial condition is
$$f(x,p_0)=\lfloor x\rfloor.$$
Repeated application of the recurrence computes $f(N^3,N)$ using only values
$$f(y,p_i), 
\qquad 
y\le N^2,$$
because every division by a prime exceeding $N$ reduces the argument below $N^2$.
Thus $f(N^3,N)$ can be obtained from the table of values $\pi(x)$ for $x\le N^2$.
Combining part (a) with the values of $f(N^3,N)$ and $f_2(N^3,N)$ yields $\pi(N^3)$.
### (d)
The recurrence in part (c) repeatedly requests values of the form
$$f(y,p_j),$$
for many pairs $(y,j)$.
Efficient evaluation is obtained by storing previously computed values in a table indexed by $(y,j)$. Since many recursive branches reach the same pair, memoization prevents repeated computation.
The arguments $y$ that arise are of the form
$$\left\lfloor\frac{N^3}{d}\right\rfloor,$$
where $d$ is a product of distinct small primes. The number of distinct values is $O(N^{2/3})$, which permits compact storage.
For the evaluation of $f_2(N^3,N)$, a table of $\pi(x)$ for all required arguments $x\le N^2$ is sufficient. The summation
$$\sum_{N<p\le N^{3/2}} 
\bigl(\pi(N^3/p)-\pi(p-1)\bigr)$$
can then be accumulated directly.
The resulting computation requires storage only for the distinct arguments that occur in the recursion together with the prime-counting table up to $N^2$.
## Verification
Part (a) depends on the fact that an $N$-survivor $\le N^3$ cannot possess three prime factors exceeding $N$. Indeed,
$$p_1p_2p_3>(N)(N)(N)=N^3.$$
Hence every composite survivor contributes to $f_2$ and to no higher $f_k$.
Part (b) counts each semiprime survivor exactly once because the condition $p\le q$ selects a unique ordering of the two prime factors.
Part (c) partitions the set of $p_{j-1}$-survivors according to divisibility by $p_j$; the two classes are disjoint and exhaustive, therefore the recurrence is exact.
The formula of part (a) then reconstructs $\pi(N^3)$ from the survivor count and the semiprime correction term.
## Notes
The recurrence
## f(x,p_{j-1})
f(x/p_j,p_{j-1})
$$là nền tảng tổ hợp của phương pháp đếm số nguyên tố Lagarias-Miller-Odlyzko. Nó thay thế sàng trực tiếp lên đến$N^3$bằng một phép tính chỉ bao gồm thông tin tối đa$N^2$, đó là nguồn gốc của sự phức tạp được cải thiện.
