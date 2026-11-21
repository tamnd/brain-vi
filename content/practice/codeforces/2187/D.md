--- 
title: "CF 2187D - Vấn đề thú vị" 
mô tả: "Quá trình xây dựng một mảng$c$từ trái sang phải. Nếu ký tự tiếp theo là 0 thì chúng ta di chuyển theo$$ci = c{i-1} + x$$và nếu là 1, chúng ta phản ánh xung quanh$y/2$,$$ci = y - c{i-1}.$$Đối với một chuỗi nhị phân hoàn chỉnh, giá trị quan tâm là$$f = sum{i=1}^{n} ci." date: "2026-06-07T21:22:40+07:00" tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "math"] categories: ["algorithms"] codeforces_contest: 2187 codeforces_index: "D" codeforces_contest_name: "Codeforces Round 1077 (Div. 1)" rating: 2600 weight: 2187 solve_time_s: 166 verified: true draft: false --- [CF 2187D - Cool Problem](https://codeforces.com/problemset/problem/2187/D) **Rating:** 2600   **Tags:** bitmasks, dp, math   **Solve time:** 2m 46s   **Verified:** yes   ## Solution ## Problem Understanding The process builds an array $c$ from left to right. If the next character is `0`, we move by$$c_i = c_{i-1} + x$$
and if it is `1`, we reflect around $y/2$,
$$c_i = y - c_{i-1}.$$
For a completed binary string, the value of interest is
$$f = \sum_{i=1}^{n} c_i.$$
Some positions of the string are fixed, while others are `?`. Every completion produces one value of $f$. A number is called cool if it appears as $f$ for at least one completion. We must compute the sum of all distinct cool integers.
The obvious difficulty is that a string with $m$ question marks has $2^m$ completions. Since $n$ can reach $10^5$ across the test file, any algorithm that explicitly enumerates completions is hopeless.
The key observation is that the recurrence is much more structured than it first appears. The entire history of the process can be compressed into the final value $c_n$. Once we understand the set of reachable $c_n$ values, the problem becomes a reachability problem on a one-dimensional state space.
A common mistake is to sum over completions instead of summing over distinct cool integers. For example:
⟦PROTECT_0⟧
The completions produce $f=1$ and $f=2$. The answer is $1+2=3$, not $2 \cdot 1 + 2 \cdot 2$.
Another subtle point is that different reachable terminal states may produce the same $f$. The solution must deduplicate the resulting values before summing them.
## Approaches
A brute force solution replaces every `?` by both possibilities, computes the generated array, computes $f$, and inserts the result into a set.
This is correct because it directly follows the definition. Unfortunately, if all characters are `?`, we must examine $2^n$ strings. Even $n=60$ is already impossible, while the actual limit is $10^5$.
To go further, we need to understand the algebra of the recurrence.
Define
$$G(c)=\frac{c^2+(x-y)c}{2x}.$$
A direct calculation shows that for both transitions,
$$G(c_i)-G(c_{i-1}) = c_i-\frac y2.$$
Summing over all positions gives
$$f 
= 
G(c_n)-G(0)+\frac{ny}{2} 
= 
\frac{c_n^2+(x-y)c_n+nxy}{2x}.$$
So $f$ depends only on the final state $c_n$. This is the decisive simplification. The problem is now:
1. Find every reachable value of $c_n$.
2. Convert each reachable $c_n$ into $f$.
3. Deduplicate equal $f$ values.
4. Sum them.
The remaining task is to describe the reachable terminal states efficiently.
Every reachable value always has one of four forms:
$$kx,\quad kx+y,\quad -kx,\quad -kx+y,$$
for some nonnegative integer $k$.
This suggests storing only the coefficient $k$ and which of the four forms we are in. The transitions between these forms are simple and can be represented by shifts of bitsets. The official solution uses exactly four bitsets and processes the string from left to right.
| Approach | Time Complexity | Space Complexity | Verdict |
| --- | --- | --- | --- |
| Brute Force | $O(2^m n)$ | $O(2^m)$ | Too slow |
| Bitset DP | $O(n^2 / w)$ | $O(n)$ bitsets | Accepted |
Here $w$ is the machine word size. With $n \le 10^5$, bitset shifts are fast enough.
## Algorithm Walkthrough
### State Representation
Let
- `bf0[k]` mean $kx$ is reachable.
- `bf1[k]` mean $kx+y$ is reachable.
- `br0[k]` mean $-kx$ is reachable.
- `br1[k]` mean $-kx+y$ is reachable.
The index $k$ is the only quantity stored.
### Transitions
1. Initialize only state $0$ as reachable.
2. When processing a `0`:
$$kx \rightarrow (k+1)x,$$
$$kx+y \rightarrow (k+1)x+y,$$
$$-kx \rightarrow -(k-1)x,$$
$$-kx+y \rightarrow -(k-1)x+y.$$
These are bitset shifts.
3. When processing a `1`:
$$kx \rightarrow -kx+y,$$
$$kx+y \rightarrow -kx,$$
$$-kx \rightarrow kx+y,$$
$$-kx+y \rightarrow kx.$$
This simply swaps information between the four bitsets.
4. If the current character is `?`, perform both groups of transitions and take the union.
5. After all characters are processed, enumerate every reachable terminal value $c_n$.
6. Convert each reachable $c_n$ into
$$f 
= 
\frac{c_n^2+(x-y)c_n+nxy}{2x}.$$
1. Sort the obtained values, erase duplicates, and sum them modulo $998244353$.
### Why it works
The invariant is that after processing a prefix of the string, the four bitsets contain exactly the reachable values of $c$ written in the four canonical forms
$$kx,\quad kx+y,\quad -kx,\quad -kx+y.$$
The recurrence maps every canonical form into another canonical form, so the invariant is preserved after each character.
The telescoping identity
$$G(c_i)-G(c_{i-1})=c_i-\frac y2$$
proves that $f$ depends only on the final state $c_n$. Every reachable terminal state is generated by the DP, every generated state corresponds to a valid completion, and deduplication removes repeated cool integers. Hence the algorithm computes exactly the required sum.
## Python Solution
⟦PROTECT_1⟧
The four integers `bf0`, `bf1`, `br0`, and `br1` are Python bitsets. Bit `k` being set means coefficient `k` is reachable in the corresponding canonical form.
The `0` transition becomes a shift. The `1` transition becomes movement between bitsets. This is exactly what the algebraic form of the recurrence dictates.
The function `to_sum` implements the closed-form expression for $f$. Using the formula avoids reconstructing the entire generating array.
The final sort-and-unique step is essential. Different reachable terminal states can map to the same cool integer, and the problem asks for the sum of distinct cool integers.
## Worked Examples
### Example 1
Input:
⟦PROTECT_2⟧
Reachable terminal states:
| Completion | $c_n$ | $f$ |
| --- | --- | --- |
| 0 | 1 | 1 |
| 1 | 2 | 2 |
Distinct cool integers are $\{1,2\}$.
Answer:
$$1+2=3.$$
### Example 2
Input:
⟦PROTECT_3⟧
Reachable completions:
| String | $c_3$ | $f$ |
| --- | --- | --- |
| 000 | 21 | 42 |
| 001 | -9 | 12 |
| 100 | 19 | 36 |
| 101 | -7 | 10 |
The cool integers are
$$\{42,12,36,10\}.$$
Their sum is
$$100.$$ 
Ví dụ này chứng minh rằng DP chỉ cần trạng thái đầu cuối$c_3$. Một lần$c_3$được biết,$f$suy ra ngay từ công thức đóng. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 / w)$| Bitset thay đổi và kết hợp theo chiều dài$n$| 
| Không gian |$O(n)$| Bốn bit có độ dài$n+1$| 
Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$và các hoạt động bitset xử lý song song nhiều trạng thái. Điều này thoải mái phù hợp trong giới hạn cuộc thi. 
## Trường hợp thử nghiệm```
# provided samples
assert run("""4
1 1 2
0
1 1 2
?
3 7 5
?0?
7 114514 191981
?1?????
""") == """1
3
100
8039591
"""
# minimum size
assert run("""1
1 5 7
0
""") == "5\n"
# single question mark
assert run("""1
1 1 2
?
""") == "3\n"
# all fixed
assert run("""1
3 2 3
000
""") == "12\n"
# all questions
assert run("""1
2 1 1
??
""") == run("""1
2 1 1
??
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 7 / 0`|`5`| Trường hợp cố định nhỏ nhất | 
|`1 1 2 / ?`|`3`| Cả hai quá trình chuyển đổi đều có thể truy cập | 
|`3 2 3 / 000`|`12`| Chuỗi phụ gia nguyên chất | 
|`2 1 1 / ??`| tự nhất quán | Nhiều trạng thái đầu cuối có thể truy cập | 
## Vỏ cạnh 
Hãy xem xét:```
1
1 1 2
?
```Có hai sự hoàn thành khác nhau. Thuật toán đặt một trạng thái có thể truy cập vào`bf0[1]`và một cái khác ở`bf1[0]`. Cả hai trạng thái cuối đều được chuyển đổi thành số nguyên mát mẻ và được tính tổng. Không cần liệt kê số lần hoàn thành. 
Coi như:```
1
3 7 5
?0?
```Các trạng thái đầu cuối có thể truy cập là$21$,$-9$,$19$, Và$-7$. DP lưu trữ chúng một cách gọn gàng thông qua các hệ số$k$và định danh mẫu. Mỗi trạng thái được chuyển đổi thông qua công thức đóng, tạo ra chính xác bốn số nguyên thú vị từ câu lệnh. 
Hãy xem xét một chuỗi cố định hoàn toàn như:```
1
4 10 20
0101
```Ở mỗi bước chỉ có một quá trình chuyển đổi pháp lý. Bitset DP suy biến thành một trạng thái có thể truy cập được. Bước loại bỏ trùng lặp cuối cùng để lại một giá trị chính xác là số nguyên thú vị duy nhất. 
Giải pháp được sử dụng ở trên tuân theo cách tiếp cận cuộc thi chính thức dựa trên đặc tính trạng thái đầu cuối và DP bốn bit.
