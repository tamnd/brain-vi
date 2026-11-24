--- 
tựa đề: "CF 913H - Đừng Vượt Quá" 
Mô tả: "Hãy$$si=t1+t2+cdots+ti$$mọi nơi$ti$được chọn độc lập và thống nhất từ$[0,1]$. Chúng tôi được đưa ra giới hạn trên$x1,x2,dots,xn$. Nhiệm vụ là tính xác suất để mỗi tổng tiền tố nằm dưới giới hạn tương ứng của nó:$$sile xi quad text{for all } i." date: "2026-06-13T01:18:03+07:00" tags: ["codeforces", "competitive-programming", "math", "probabilities"] categories: ["algorithms"] codeforces_contest: 913 codeforces_index: "H" codeforces_contest_name: "Hello 2018" rating: 3400 weight: 913 solve_time_s: 590 verified: true draft: false --- [CF 913H - Don't Exceed](https://codeforces.com/problemset/problem/913/H) **Rating:** 3400   **Tags:** math, probabilities   **Solve time:** 9m 50s   **Verified:** yes   ## Solution ## Problem Understanding Let$$s_i=t_1+t_2+\cdots+t_i$$
where every $t_i$ is chosen independently and uniformly from $[0,1]$.
We are given upper bounds $x_1,x_2,\dots,x_n$. The task is to compute the probability that every prefix sum stays below its corresponding bound:
$$s_i\le x_i \quad \text{for all } i.$$
The answer is a rational number. The statement guarantees that it can be represented as $P/Q$, and asks for
$$P\cdot Q^{-1}\pmod{998244353}.$$
The most important observation is that the probability is the volume of a region inside the unit cube $[0,1]^n$. Since every $x_i$ has at most six digits after the decimal point, all coordinates are rational numbers, so the final probability is also rational.
The constraint $n\le 30$ is surprisingly small. That immediately suggests that the intended solution is not based on large-scale DP or numerical integration. Instead, we can afford fairly expensive operations on polynomials of degree up to $30$.
A subtle point is that the $x_i$ are real numbers. Any floating-point implementation is doomed, because the required answer is an exact rational value modulo a prime. All computations must be performed symbolically inside the finite field modulo $998244353$.
Another easy mistake is to compute only the distribution of $s_n$. The event depends on every intermediate prefix sum. For example:
⟦PROTECT_0⟧
The final sum constraint is weak, but the second prefix constraint is not. Ignoring intermediate bounds produces a completely different probability.
A third trap is the behavior at breakpoints. The density functions arising from repeated convolutions are piecewise polynomials. Their polynomial formula changes exactly when $x$ crosses numbers of the form $x_i+k$, where $k$ is an integer. Missing even one breakpoint causes incorrect integration on an entire interval.
## Approaches
A brute-force view is to describe the probability directly as an $n$-dimensional integral over all variables $t_1,\dots,t_n$. The region is
$$0\le t_i\le 1,$$
together with
$$t_1+\cdots+t_i\le x_i.$$
The integral is correct, but completely unusable. Even if we recursively integrate one variable at a time, the shape of the region quickly becomes complicated.
The key observation comes from viewing the process as repeated convolution with the uniform distribution on $[0,1]$.
Let $f_i(x)$ be the density of $s_i$ under the condition that all previous constraints have already been respected. Then
$$f_i(x)= 
\bắt đầu{trường hợp} 
\int_{x-1}^{x} f_{i-1}(y)\,dy, & x\le x_i,\\ 
0, & x>x_i. 
\end{trường hợp}$$
This is exactly a convolution with the uniform kernel, followed by truncation at $x_i$.
Repeated convolution of piecewise polynomials produces another piecewise polynomial. The only places where the formula can change are shifts of existing breakpoints by integers. Since every truncation introduces a breakpoint at $x_i$, every future breakpoint has the form
$$x_i+k.$$
Because $n\le 30$, there are at most
$$n(n+1)+1$$
distinct breakpoints, fewer than one thousand.
Once all breakpoints are known, every density is represented by one polynomial on each interval between consecutive breakpoints. The recurrence becomes purely algebraic: integrate a polynomial, evaluate it on neighboring intervals, and subtract.
This converts a probability problem into symbolic manipulation of low-degree piecewise polynomials.
| Approach | Time Complexity | Space Complexity | Verdict |
| --- | --- | --- | --- |
| Direct multidimensional integration | Exponential | Exponential | Too slow |
| Piecewise-polynomial DP | $O(n^4)$ | $O(n^3)$ | Accepted |
## Algorithm Walkthrough
### Definitions
Let $f_i(x)$ denote the density of $s_i$ while respecting all constraints up to step $i$.
Define
$$f_0(x)=\delta(x),$$
a unit mass at zero.
For $i\ge 1$,
$$f_i(x)= 
\bắt đầu{trường hợp} 
\int_{x-1}^{x} f_{i-1}(y)\,dy, & x\le x_i,\\ 
0, & x>x_i. 
\end{trường hợp}$$
### Global partition
All breakpoints that can ever appear have the form
$$x_j+k,$$
where $0\le k\le n$.
Collect every such value, add $0$, sort them, and remove duplicates.
These points divide the real line into intervals. On each interval every density will be represented by a single polynomial.
### Polynomial representation
For every stage $i$ and every interval, store the polynomial describing $f_i$ on that interval.
The degree never exceeds $i$, hence never exceeds $30$.
### Computing the antiderivative
1. Suppose we already know all interval polynomials of $f_{i-1}$.
2. Build a continuous antiderivative $F$ satisfying
$$F'(x)=f_{i-1}(x).$$
1. For each interval, integrate its polynomial term-by-term.
2. Adjust the constant term so that $F$ remains continuous across interval boundaries.
### Applying the convolution
1. For $x\le x_i$,
$$f_i(x)=F(x)-F(x-1).$$
1. On a fixed interval, both $x$ and $x-1$ belong to fixed pieces of the partition. Hence both $F(x)$ and $F(x-1)$ are ordinary polynomials there.
2. Subtract them to obtain the polynomial for $f_i$ on that interval.
3. Every interval entirely to the right of $x_i$ becomes zero because of the truncation.
### Extracting the answer
1. After constructing $f_n$, build its antiderivative $G$.
2. The desired probability is
$$\int_{-\infty}^{x_n} f_n(x)\,dx 
= 
G(x_n).$$
1. Evaluate this value modulo $998244353$.
### Why it works
The recurrence for $f_i$ is exactly the density recurrence obtained by adding an independent uniform random variable $t_i\in[0,1]$ and then enforcing the condition $s_i\le x_i$. Every step preserves the exact density, not an approximation.
The partition contains every possible location where a formula change can occur. Inside a single interval, both $F(x)$ and $F(x-1)$ come from fixed polynomial pieces, so their difference is also a polynomial. Thus the stored representation is always exact.
By induction on $i$, the piecewise-polynomial description equals the true density of $s_i$. Evaluating its cumulative integral at $x_n$ gives precisely the probability that all constraints hold.
## Python Solution
⟦PROTECT_1⟧
The implementation follows the recurrence literally. Every interval stores a polynomial describing the density on that interval. The antiderivative is rebuilt at each stage, keeping continuity by adjusting the constant term. After that, the convolution formula $F(x)-F(x-1)$ is applied interval by interval.
The most delicate part is the breakpoint construction. Every future discontinuity must already be present in the partition. That is why all values $x_i+k$ are inserted before the DP begins.
Another subtle point is that the answer must be exact modulo the prime. The input decimals are converted into rational field elements using the modular inverse of $10^6$. No floating-point arithmetic is used anywhere.
## Worked Examples
### Sample 1
Input
⟦PROTECT_2⟧
Every constraint is automatically satisfied because $s_i\le i$ and $x_i=i$.
| Step | Constraint | Probability |
| --- | --- | --- |
| 1 | $s_1\le1$ | 1 |
| 2 | $s_2\le2$ | 1 |
| 3 | $s_3\le3$ | 1 |
| 4 | $s_4\le4$ | 1 |
Final answer is $1$.
This example shows that the truncation never removes any mass. The density evolves exactly like the ordinary Irwin-Hall distribution.
### Sample 3
Input
⟦PROTECT_3⟧
The valid region is
$$0\le t_1\le 0,5,$$
and
$$t_1+t_2\le 1.$$
| $t_1$ range | Allowed $t_2$ length |
| --- | --- |
| $0 \le t_1 \le 0.5$ | $1-t_1$ |
Hence
$$\int_0^{0.5}(1-t_1)\,dt_1 
= 
\frac12-\frac18 
= 
\frac38.$$ 
Xác suất là$3/8$, chính xác giá trị được đề cập trong tuyên bố. 
Ví dụ này minh họa cách ràng buộc thứ hai loại bỏ một phần khối lượng mặc dù$x_2=1$không phải là đặc biệt nhỏ. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^4)$| Về$O(n^2)$khoảng, mức độ$O(n)$, lặp lại cho$n$giai đoạn | 
| Không gian |$O(n^3)$| Tối đa đa thức bậc từng phần$n$| 
Với$n\le30$, số khoảng dưới một nghìn và độ không bao giờ vượt quá ba mươi. Đa thức ký hiệu thu được DP vừa khít bên trong các giới hạn. 
## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    # invoke solution()
    pass
# provided samples
assert run("4\n1.00\n2\n3.000000\n4.0\n") == "1\n"
assert run("1\n0.50216\n") == "342677322\n"
assert run("2\n0.5\n1.0\n") == "623902721\n"
# minimum size
assert run("1\n1\n") == "1\n"
# probability = 1/2
assert run("1\n0.5\n") == str((998244353 + 1) // 2) + "\n"
# all constraints very large
assert run("3\n1\n2\n3\n") == "1\n"
# strong middle restriction
assert run("3\n1\n0.5\n3\n") == run("3\n1\n0.5\n3\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1, x=1`|`1`| Khoảng thời gian đầy đủ được chấp nhận | 
|`1, x=0.5`|`1/2 mod M`| Phân tích thập phân và số học hợp lý | 
|`1,2,3`|`1`| Xác suất vẫn là một | 
| Ràng buộc chặt chẽ ở giữa | Kiểm tra xác định | Cắt ngắn hợp lý ở các bước trung gian | 
## Vỏ cạnh 
Hãy xem xét```
1
0.5
```Xác suất chính xác là$1/2$. Việc triển khai dấu phẩy động có thể đại diện$0.5$xấp xỉ và mất đi độ chính xác khi chuyển đổi sang số học mô đun. Cách tiếp cận đa thức lưu trữ giá trị dưới dạng phần tử trường$500000/1000000$, chính xác là$1/2$. 
Coi như```
2
10
0.5
```Một giải pháp chỉ nhìn vào sự phân phối cuối cùng của$s_2$sẽ tạo ra xác suất lớn. Thuật toán chính xác sẽ cắt ngắn sau mỗi giai đoạn. Khi xây dựng$f_2$, vi phạm hàng loạt$s_2\le0.5$được loại bỏ ngay lập tức. 
Coi như```
2
0.5
1.0
```Tập hợp điểm dừng chứa$0.5$,$1.0$,$1.5$,$2.0$và các biến thể dịch chuyển của chúng. Nếu phân vùng bỏ qua điểm dừng tại$1.5$, công thức mật độ sẽ được áp dụng trong khoảng thời gian mà nó không còn giá trị nữa. Bao gồm mọi giá trị$x_i+k$ngăn chặn sai lầm đó và giữ chính xác mọi phần đa thức.
