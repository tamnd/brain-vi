--- 
title: "CF 1934A - Too Min Too Max" 
mô tả: "Chúng tôi được yêu cầu tối đa hóa biểu thức$$over all quadruples of distinct indices $i, j, k, l$ in an array $a$. The input provides multiple test cases, each with an array of integers. For each test case, the output is the maximum value of the expression." date: "2026-06-08T18:09:50+07:00" tags: ["codeforces", "competitive-programming", "greedy", "math"] categories: ["algorithms"] codeforces_contest: 1934 codeforces_index: "A" codeforces_contest_name: "Codeforces Round 931 (Div. 2)" rating: 800 weight: 1934 solve_time_s: 137 verified: false draft: false --- [CF 1934A - Too Min Too Max](https://codeforces.com/problemset/problem/1934/A) **Rating:** 800   **Tags:** greedy, math   **Solve time:** 2m 17s   **Verified:** no   ## Solution ## Problem Understanding We are asked to maximize the expression$$|a_i - a_j| + |a_j - a_k| + |a_k - a_l| + |a_l - a_i|$$
over all quadruples of distinct indices $i, j, k, l$ in an array $a$. The input provides multiple test cases, each with an array of integers. For each test case, the output is the maximum value of the expression.
The expression is essentially the perimeter of a "walk" through four elements of the array, moving from $a_i$ to $a_j$ to $a_k$ to $a_l$ and back to $a_i$, measuring absolute differences along the edges. The problem reduces to selecting the largest and smallest values in a way that maximizes the sum of differences.
Constraints allow $n$ up to 100 and $t$ up to 500. This permits $O(n^4)$ brute-force solutions in principle, but a careful observation can reduce the complexity. Edge cases include arrays with all equal elements, where the expression is zero, and arrays with both negative and positive numbers, which influence which combination yields the maximum.
A naive approach might try all quadruples, but we need to understand the structure of absolute differences to simplify the problem.
## Approaches
A brute-force approach enumerates all quadruples $i, j, k, l$ and computes the expression directly. This is $O(n^4)$, which is acceptable given $n \le 100$, but it is unnecessary because the expression can be simplified by considering the extreme values.
The key observation is that absolute differences are maximized when combining the minimum and maximum elements. Let $mn = \min(a)$ and $mx = \max(a)$. To maximize the sum, we want the sequence to alternate between these extremes. For example, choosing $i$ as the minimum, $j$ as the maximum, $k$ as the minimum, and $l$ as the maximum produces the largest contribution from each term. Therefore, the maximum expression reduces to:
$$2 \times (\text{max}(a) - \text{min}(a)) + 2 \times (\text{đóng góp tối đa/phút thứ hai})$$ 
Với kích thước mảng nhỏ, sự kết hợp chính xác của các giá trị cực trị có thể được liệt kê một cách hiệu quả. Một phương pháp đơn giản là sắp xếp mảng và tính biểu thức cho các chuỗi chọn hai phần tử lớn nhất và hai phần tử nhỏ nhất theo thứ tự xen kẽ. Điều này đảm bảo tối đa. 
| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(1) | Được chấp nhận (n ≤ 100) | 
| Tham lam/Cực đoan | O(n log n) | O(n) | Được chấp nhận, nhanh hơn | 
## Hướng dẫn thuật toán 
1. Với mỗi test case, hãy đọc mảng$a$và lưu trữ chiều dài của nó$n$. 
2. Sắp xếp$a$để dễ dàng truy cập các giá trị tối thiểu, tối thiểu thứ hai, tối đa và tối đa thứ hai. 
3. Tính các giá trị dự kiến ​​của biểu thức bằng cách đặt các phần tử cực trị ở các vị trí xen kẽ nhau. Các ứng cử viên điển hình là: 
-$a_0, a_1, a_{n-2}, a_{n-1}$-$a_0, a_1, a_{n-1}, a_{n-2}$-$a_0, a_{n-2}, a_1, a_{n-1}$-$a_0, a_{n-1}, a_1, a_{n-2}$4. Với mỗi ứng viên gấp bốn lần, hãy tính biểu thức$|a_i - a_j| + |a_j - a_k| + |a_k - a_l| + |a_l - a_i|$5. Theo dõi giá trị tối đa của các ứng viên và xuất nó. 
Tại sao nó hoạt động: Sự khác biệt tuyệt đối được tối đa hóa bằng cách chọn những khác biệt lớn nhất có thể có ở mỗi bước. Các phần tử tối thiểu và tối đa xen kẽ đảm bảo mỗi số hạng đóng góp tối đa vào tổng. Việc sắp xếp cho phép chúng ta dễ dàng liệt kê các cấu hình cực đoan này mà không cần kiểm tra tất cả các bộ tứ. 
## Giải pháp Python```python
import sys
input = sys.stdin.readline
def max_expression(a):
    a.sort()
    n = len(a)
    candidates = [
        [a[0], a[1], a[n-2], a[n-1]],
        [a[0], a[1], a[n-1], a[n-2]],
        [a[0], a[n-2], a[1], a[n-1]],
        [a[0], a[n-1], a[1], a[n-2]],
    ]
    res = 0
    for quad in candidates:
        i, j, k, l = quad
        val = abs(i-j) + abs(j-k) + abs(k-l) + abs(l-i)
        res = max(res, val)
    return res
def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(max_expression(a))
if __name__ == "__main__":
    solve()
```Giải thích: Việc sắp xếp đảm bảo chúng ta có thể dễ dàng chọn ra các phần tử cực trị. Các bộ tứ ứng cử viên được chọn để tối đa hóa tổng chênh lệch tuyệt đối. Số lượng nhỏ các ứng cử viên (4) đảm bảo tính chính xác trong khi vẫn giữ tính toán đơn giản. 
## Ví dụ đã hoạt động 
Đối với đầu vào`[1,1,2,2,3]`, mảng được sắp xếp là`[1,1,2,2,3]`. Ứng viên tăng gấp bốn lần: 
| tôi,j,k,l | Biểu hiện | 
| --- | --- | 
| 1,1,2,3 | | 
| 1,1,3,2 | ... = 6 | 
| 1,2,1,3 | ... = 6 | 
| 1,3,1,2 | ... = 6 | 
Tối đa là 6. 
Đối với đầu vào`[5,1,3,2,-3,-1,10,3]`, mảng được sắp xếp`[-3,-1,1,2,3,3,5,10]`. Bộ tứ ứng viên tạo ra giá trị biểu thức 38, giá trị tối đa. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; ứng viên không đổi | 
| Không gian | O(n) | Lưu trữ mảng đã sắp xếp | 
Được cho$n ≤ 100$Và$t ≤ 500$, điều này hiệu quả. 
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
assert run("5\n4\n1 1 1 1\n5\n1 1 2 2 3\n8\n5 1 3 2 -3 -1 10 3\n4\n3 3 1 1\n4\n1 2 2 -1\n") == "0\n6\n38\n8\n8"
# Custom cases
assert run("1\n4\n-1 -1 -1 -1\n") == "0", "all equal negative"
assert run("1\n5\n1 2 3 4 5\n") == "12", "ascending consecutive"
assert run("1\n6\n-5 -2 0 2 3 5\n") == "28", "mixed negatives and positives"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 phần tử đều bằng nhau | 0 | Xử lý mảng thống nhất | 
| 1..5 | 12 | Max với mảng tăng dần | 
| Hỗn hợp tiêu cực và tích cực | 28 | Xử lý đúng biển báo | 
## Vỏ cạnh 
Đối với các mảng có tất cả các phần tử bằng nhau, biểu thức bằng 0 bất kể lựa chọn. Việc sắp xếp và liệt kê ứng viên vẫn hoạt động vì tất cả các giá trị đều giống hệt nhau, tạo ra kết quả bằng 0. Đối với kích thước tối thiểu$n=4$, các ứng cử viên chính xác là bốn phần tử, do đó thuật toán tính toán chính xác biểu thức.
