--- 
title: "CF 1898D - Vẻ đẹp tuyệt đối" 
mô tả: "Với mỗi vị trí i, chúng ta có một cặp số (ai, bi). Vẻ đẹp hiện tại là tổng khoảng cách giữa các giá trị được ghép nối:$$sum We may choose at most one pair of indices and swap the corresponding values inside array b." date: "2026-06-08T21:31:48+07:00" tags: ["codeforces", "competitive-programming", "greedy", "math"] categories: ["algorithms"] codeforces_contest: 1898 codeforces_index: "D" codeforces_contest_name: "Codeforces Round 910 (Div. 2)" rating: 1900 weight: 1898 solve_time_s: 133 verified: true draft: false --- [CF 1898D - Absolute Beauty](https://codeforces.com/problemset/problem/1898/D) **Rating:** 1900   **Tags:** greedy, math   **Solve time:** 2m 13s   **Verified:** yes   ## Solution ## Problem Understanding For every position `i`, we have a pair of numbers `(a_i, b_i)`. The current beauty is the sum of the distances between the paired values:$$\sum |a_i-b_i|$$
We may choose at most one pair of indices and swap the corresponding values inside array `b`. After that swap, every position still contributes an absolute difference with its own `a_i`, and we want the largest possible total beauty.
A useful way to think about the problem is that every index defines an interval between its two values. If `a_i = 3` and `b_i = 8`, the pair corresponds to the interval `[3,8]`. The contribution of that index is exactly the interval length.
The total length of all intervals is already known. The only thing a swap can change is the contribution of the two indices involved in that swap.
The constraints are the real challenge. The sum of all `n` across test cases is at most `2·10^5`, which means an `O(n log n)` or `O(n)` solution is expected. Any algorithm that tries all pairs of indices would require `O(n²)` work, roughly `2·10^10` operations in the worst case, which is far beyond what can run in two seconds.
Several edge cases are easy to mishandle.
Consider:
⟦PROTECT_0⟧
The current beauty is `1 + 1 = 2`. Swapping the two elements of `b` produces `[1,2]`, whose beauty is `0`. The correct answer is `2`, because the operation is optional. Any solution that assumes a swap must be performed gives the wrong result.
Consider:
⟦PROTECT_1⟧
The initial beauty is `0`. After swapping, the beauty becomes `2`. The answer is `2`. A solution that only checks whether intervals overlap would miss that a beneficial swap exists even though both original intervals have length zero.
Another subtle case is when intervals are already disjoint:
⟦PROTECT_2⟧
The beauty is `4 + 4 = 8`. Any swap leaves it unchanged. The answer remains `8`. A careless derivation may incorrectly assume that every pair of non-overlapping intervals allows a gain.
## Approaches
The brute-force idea is straightforward. Compute the current beauty. For every pair `(i,j)`, swap `b_i` and `b_j`, recompute the change in contribution of those two positions, and keep the best result.
This works because a swap affects only positions `i` and `j`. Instead of recomputing the whole sum, we can evaluate a pair in constant time. Even then, there are `O(n²)` candidate pairs. With `n = 2·10^5`, that means roughly twenty billion pairs, which is completely infeasible.
The key observation is that each position can be represented by the interval
$$[l_i,r_i]$$
where
$$l_i=\min(a_i,b_i),\qquad r_i=\max(a_i,b_i).$$
Let
$$S=\sum (r_i-l_i)$$
be the original beauty.
Suppose we swap values between positions `i` and `j`. The only thing that changes is the combined contribution of those two positions.
A very useful identity is:
$$|x-v|+|u-y| 
- 
(|x-y|+|u-v|) 
= 
2\cdot \max(0,\; \text{gap})$$
where the gap is the distance between the two intervals.
After working through the geometry of absolute values, the maximum gain obtainable from swapping two positions depends only on the intervals. If two intervals overlap, no swap can improve their total contribution. If they are disjoint, the best gain equals twice the distance between them.
For intervals
$$[l_i,r_i],\quad [l_j,r_j]$$
with `r_i < l_j`, the gain is
$$2(l_j-r_i).$$
So the entire problem becomes:
Find two intervals whose separation
$$l_j-r_i$$
is as large as possible.
Equivalently, among all intervals we want the largest possible value of
$$\max l_i - \min r_i$$
taken from different intervals in the correct order.
A cleaner global interpretation emerges. Let
$$L=\max l_i,\qquad R=\min r_i.$$
If all intervals intersect, then `L ≤ R` and no improvement is possible.
If they do not all intersect, then `L > R`. The maximum achievable gain is
$$2(L-R).$$
Why? The interval achieving `L` lies completely to the right of the interval achieving `R`, and their separation is exactly `L-R`. No pair of intervals can have a larger gap than that.
Thus the answer is simply
$$S + 2\max(0,LR).$$
| Approach | Time Complexity | Space Complexity | Verdict |
| --- | --- | --- | --- |
| Brute Force | O(n²) | O(1) | Too slow |
| Optimal | O(n) | O(1) | Accepted |
## Algorithm Walkthrough
1. Initialize `base = 0`.
2. For every position `i`, compute:
$$l_i=\min(a_i,b_i)$$
and
$$r_i=\max(a_i,b_i).$$
Add `r_i - l_i` to `base`. This is exactly the original beauty contribution of that position.
3. Track:
$$L=\max l_i$$
and
$$R=\phút r_i.$$
These summarize how all intervals relate to each other.
4. If `L ≤ R`, every interval shares a common intersection point. No swap can increase the beauty, so the answer is `base`.
5. Otherwise the intervals have a positive separation of `L - R`. The best possible gain is:
$$2(L-R).$$
6. Output:
$$cơ số + 2(L-R).$$ 
### Tại sao nó hoạt động 
Mỗi chỉ số đóng góp độ dài của một khoảng`[l_i,r_i]`. Việc hoán đổi chỉ ảnh hưởng đến hai khoảng thời gian. Đối với bất kỳ cặp nào, mức tăng tối đa trong tổng đóng góp bằng hai lần khoảng cách giữa các khoảng đó và bằng 0 khi chúng trùng nhau. 
Khoảng cách lớn nhất có thể có giữa hai khoảng có được bằng cách lấy khoảng có điểm cuối bên trái lớn nhất và khoảng có điểm cuối bên phải nhỏ nhất. Sự chia ly của họ là`L-R`bất cứ khi nào`L>R`. Không có cặp nào khác có thể cách xa nhau hơn vì mọi điểm cuối bên trái nhiều nhất là`L`và mọi điểm cuối bên phải ít nhất là`R`. 
Vì vẻ đẹp ban đầu là`base`và mức tăng thêm tốt nhất chính xác là`2·max(0,L-R)`, công thức luôn cho ra đáp án tối ưu. 
## Giải pháp Python```python
import sys
input = sys.stdin.readline
def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        base = 0
        max_l = 0
        min_r = 10**18
        for x, y in zip(a, b):
            l = min(x, y)
            r = max(x, y)
            base += r - l
            max_l = max(max_l, l)
            min_r = min(min_r, r)
        gain = max(0, max_l - min_r) * 2
        ans.append(str(base + gain))
    sys.stdout.write("\n".join(ans))
solve()
```Vòng lặp đầu tiên tính khoảng tương ứng với mỗi cặp`(a_i,b_i)`. Vẻ đẹp ban đầu chỉ đơn giản là tổng của các khoảng cách, vì vậy`base`tích lũy`r - l`. 
Đồng thời, chúng tôi duy trì điểm cuối bên trái lớn nhất và điểm cuối bên phải nhỏ nhất. Đó là những giá trị toàn cầu duy nhất cần thiết để xác định mức cải thiện tối đa có thể. 
biểu hiện```
max(0, max_l - min_r)
```tính toán khoảng cách. Khi tất cả các khoảng chồng lên nhau,`max_l <= min_r`và mức tăng tự động trở thành 0. 
Tất cả số học được thực hiện bằng số nguyên Python, do đó không có rủi ro tràn ngay cả khi các giá trị có thể đạt tới`10^9`và số tiền có thể vượt quá`32-bit`giới hạn. 
## Ví dụ đã hoạt động 
### Ví dụ 1```
a = [1, 2]
b = [1, 2]
```| tôi | tôi | r | căn cứ sau tôi | max_l | phút_r | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 1 | 
| 2 | 2 | 2 | 0 | 2 | 1 | 
Sau khi xử lý tất cả các khoảng: 
| Biến | Giá trị | 
| --- | --- | 
| căn cứ | 0 | 
| max_l | 2 | 
| phút_r | 1 | 
| đạt được | 2 | 
| trả lời | 2 | 
Các khoảng`[1,1]`Và`[2,2]`rời rạc với khoảng cách`1`. Lợi ích là`2`, phù hợp với tác dụng của việc hoán đổi hai phần tử. 
### Ví dụ 2```
a = [1, 2, 3, 4]
b = [5, 6, 7, 8]
```| tôi | tôi | r | căn cứ sau tôi | max_l | phút_r | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 4 | 1 | 5 | 
| 2 | 2 | 6 | 8 | 2 | 5 | 
| 3 | 3 | 7 | 12 | 3 | 5 | 
| 4 | 4 | 8 | 16 | 4 | 5 | 
Giá trị cuối cùng: 
| Biến | Giá trị | 
| --- | --- | 
| căn cứ | 16 | 
| max_l | 4 | 
| phút_r | 5 | 
| đạt được | 0 | 
| trả lời | 16 | 
Tất cả các khoảng giao nhau vì`max_l <= min_r`. Không có sự hoán đổi nào có thể cải thiện vẻ đẹp nên giá trị ban đầu vẫn ở mức tối ưu. 
Những dấu vết này minh họa cho tính bất biến trung tâm: câu trả lời chỉ phụ thuộc vào vẻ đẹp ban đầu và cấu trúc chồng chéo của các khoảng. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần | 
| Không gian | O(1) | Chỉ một số biến đang chạy được lưu trữ | 
| Tổng số trong tất cả các bài kiểm tra | O(Σn) | Tổng của tất cả các độ dài nhiều nhất là`2·10^5`| 
Giải pháp thực hiện quét tuyến tính duy nhất cho mỗi trường hợp thử nghiệm. Vì tổng số phần tử trong tất cả các trường hợp thử nghiệm bị giới hạn bởi`2·10^5`, thời gian chạy dễ dàng nằm trong giới hạn và mức sử dụng bộ nhớ liên tục thấp hơn nhiều so với giới hạn bộ nhớ. 
## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    def solve():
        input = sys.stdin.readline
        t = int(input())
        ans = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))
            base = 0
            max_l = 0
            min_r = 10**18
            for x, y in zip(a, b):
                l = min(x, y)
                r = max(x, y)
                base += r - l
                max_l = max(max_l, l)
                min_r = min(min_r, r)
            ans.append(str(base + 2 * max(0, max_l - min_r)))
        print("\n".join(ans))
    solve()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()
# provided sample
assert run("""6
3
1 3 5
3 3 3
2
1 2
1 2
2
1 2
2 1
4
1 2 3 4
5 6 7 8
10
1 8 2 5 3 5 3 1 1 3
2 9 2 4 8 2 3 5 3 1
3
47326 6958 358653
3587 35863 59474
""") == """4
2
2
16
31
419045"""
# minimum size
assert run("""1
2
1 2
1 2
""") == "2"
# already optimal without swap
assert run("""1
2
1 2
2 1
""") == "2"
# all intervals overlap
assert run("""1
3
1 5 3
4 2 6
""") == "9"
# large separation
assert run("""1
2
1 100
1 100
""") == "198"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a=[1,2], b=[1,2]`|`2`| Kích thước tối thiểu và trao đổi có lợi | 
|`a=[1,2], b=[2,1]`|`2`| Tùy chọn trao đổi, câu trả lời có thể là vẻ đẹp nguyên bản | 
| Ví dụ về khoảng chồng chéo |`9`| Độ lợi phải bằng 0 khi các khoảng giao nhau | 
|`a=[1,100], b=[1,100]`|`198`| Tính toán tách và khuếch đại khoảng cách lớn | 
## Vỏ cạnh 
Hãy xem xét:```
1
2
1 2
2 1
```Các khoảng là`[1,2]`Và`[1,2]`. 
Chúng tôi có được:```
base = 2
max_l = 1
min_r = 2
```Từ`max_l <= min_r`, mức tăng bằng 0 và câu trả lời là`2`. Hoán đổi thực sự sẽ làm giảm vẻ đẹp, nhưng thao tác là tùy chọn, do đó thuật toán giữ chính xác giá trị ban đầu. 
Coi như:```
1
2
1 2
1 2
```Các khoảng là`[1,1]`Và`[2,2]`. 
Chúng tôi có được:```
base = 0
max_l = 2
min_r = 1
```Sự tách biệt là`1`, tạo ra mức tăng`2`. Câu trả lời trở thành`2`, hoàn toàn phù hợp với vẻ đẹp sau khi hoán đổi. 
Coi như:```
1
2
1 2
5 6
```Các khoảng là`[1,5]`Và`[2,6]`. 
Chúng tôi tính toán:```
base = 8
max_l = 2
min_r = 5
```Từ`max_l <= min_r`, mức tăng bằng 0 và câu trả lời vẫn là`8`. Các khoảng chồng lên nhau nên không có sự hoán đổi nào có thể tạo thêm khoảng cách. Công thức sẽ tự động ghi lại điều này.
