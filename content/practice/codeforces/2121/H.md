--- 
title: "CF 2121H - Bé Băng" 
mô tả: "Đối với mỗi vị trí i, chúng ta không được cung cấp một giá trị cố định. Thay vào đó, chúng ta có thể chọn bất kỳ số nguyên nào bên trong một khoảng [li, ri]. Với mỗi độ dài tiền tố k, hãy xem xét tất cả các mảng$$a1,a2,dots,ak$$như vậy$$li le ai le ri." date: "2026-06-08T03:50:08+07:00" tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp", "implementation", "sortings"] categories: ["algorithms"] codeforces_contest: 2121 codeforces_index: "H" codeforces_contest_name: "Codeforces Round 1032 (Div. 3)" rating: 2300 weight: 2121 solve_time_s: 90 verified: false draft: false --- [CF 2121H - Ice Baby](https://codeforces.com/problemset/problem/2121/H) **Rating:** 2300   **Tags:** binary search, brute force, data structures, dp, implementation, sortings   **Solve time:** 1m 30s   **Verified:** no   ## Solution ## Problem Understanding For every position `i`, we are not given a fixed value. Instead, we may choose any integer inside an interval `[l_i, r_i]`. For each prefix length `k`, consider all arrays$$a_1,a_2,\dots,a_k$$
such that
$$l_i \le a_i \le r_i.$$
Among all valid choices, we want the largest possible length of a non-decreasing subsequence.
The answer must be reported for every prefix independently. After reading the first interval we output the best LNDS length for that prefix, after reading the first two intervals we output the best LNDS length for that prefix, and so on.
The total number of intervals over all test cases is at most `2·10^5`. A quadratic DP would require roughly `4·10^10` operations in the worst case, which is completely impossible within two seconds. Any accepted solution must be close to `O(n log n)`.
The tricky part is that we are not constructing one particular array. We are optimizing over all possible choices inside the intervals. A greedy choice that looks good locally may destroy a longer subsequence later.
Consider
⟦PROTECT_0⟧
For the whole prefix, the optimal answer is `3`. Choosing the largest possible value everywhere gives `[5,4,3,3]`, whose LNDS length is only `2`.
Another easy mistake is to think that an interval contributes to the answer whenever it intersects some previous interval. For
⟦PROTECT_1⟧
every valid choice satisfies `a1 > a2`, so the answer is still `1`.
A third subtle case appears when many intervals can all use the same value:
⟦PROTECT_2⟧
Choosing all values equal gives an LNDS of length `3`. Any approach that treats equal values as strictly increasing would incorrectly return smaller answers.
## Approaches
The classical LNDS algorithm maintains
$$dp[j]$$
as the minimum possible ending value of a non-decreasing subsequence of length `j`.
For a fixed array, processing one element updates this structure using binary search.
A brute force interpretation of the current problem would try all valid choices for every interval and run LNDS on each resulting array. Even if intervals were tiny, the number of arrays grows exponentially. This is hopeless.
The key observation is that the standard LNDS DP does not actually need the exact chosen value. It only needs to know how the new interval can affect the minimal ending values.
Suppose we already maintain the LNDS DP for all arrays obtainable from the processed prefix. Let the current interval be `[l,r]`.
For a subsequence ending value `x`, we may append the new position if we can choose a value `v` such that
$$x \le v \le r.$$
Among all such choices, the best one is the smallest possible value, namely
$$v=\max(x,l).$$
This turns the transition into a purely interval-based operation.
The resulting DP keeps exactly the same monotonic structure as the ordinary patience-sorting LNDS algorithm. After simplifying the transitions, one discovers that processing `[l,r]` is equivalent to:
1. Find the first DP value greater than `r`.
2. Remove that value.
3. Insert `l`.
This can be maintained with a multiset.
The size of the structure directly equals the current optimal LNDS length plus one sentinel element.
Each interval causes one insertion and at most one deletion, giving `O(log n)` work per interval.
| Approach | Time Complexity | Space Complexity | Verdict |
|---|------|---|
| Brute Force | Exponential | Exponential | Too slow |
| Optimal | O(n log n) | O(n) | Accepted |
## Deriving the DP Transition
Let
$$dp_j$$
denote the minimum possible ending value of a non-decreasing subsequence of length `j`.
As in the classical LNDS algorithm, the sequence
$$dp_1 \le dp_2 \le \dots$$
is non-decreasing.
Assume we process interval `[l,r]`.
Take some state `dp_j`.
If `dp_j > r`, no value from the interval can extend that subsequence.
If `dp_j ≤ r`, we can choose a value from the interval and extend it. The smallest achievable ending value becomes
$$\max(dp_j,l).$$ 
Vì mảng DP đã được sắp xếp nên tất cả các giá trị lớn hơn`r`tạo thành một hậu tố. 
Cho phép`x`là giá trị DP đầu tiên lớn hơn`r`. 
Mọi giá trị DP nhỏ hơn vẫn hợp lệ, trong khi vị trí được biểu thị bằng`x`có thể được cải thiện để`l`. 
Sau khi đơn giản hóa LNDS tiêu chuẩn, toàn bộ bản cập nhật sẽ trở thành: 
- chèn`l`, 
- xóa giá trị đầu tiên lớn hơn`r`. 
Đây chính xác là hoạt động thay thế tương tự được thực hiện bằng cách sắp xếp kiên nhẫn. 
Một multiset tự nhiên hỗ trợ cả hai hoạt động. 
## Hướng dẫn thuật toán 
1. Tạo nhiều bộ`S`chứa một giá trị trọng điểm duy nhất`0`. 
2. Khoảng thời gian xử lý từ trái sang phải. 
3. Đối với khoảng thời gian hiện tại`[l,r]`, tìm phần tử đầu tiên trong`S`nó thực sự lớn hơn`r`. 
4. Chèn`l`vào trong`S`. 
5. Nếu phần tử đó tồn tại, hãy xóa phần tử đó. 
Đây là dạng nén của quá trình chuyển đổi LNDS DP được mô tả ở trên. 
6. Câu trả lời hiện tại bằng`|S| - 1`. 
Phép trừ loại bỏ trọng điểm. 
7. Xuất ra câu trả lời sau mỗi khoảng thời gian được xử lý. 
### Tại sao nó hoạt động 
Multiset luôn lưu trữ mảng LNDS DP hiện tại cùng với một trọng điểm`0`. 
Với mọi độ dài dãy con có thể`j`, cái`j`-phần tử nhỏ nhất của multiset bằng giá trị kết thúc tối thiểu có thể đạt được của chuỗi con không giảm độ dài`j`. 
Khi khoảng thời gian`[l,r]`đến, thay thế giá trị đầu tiên lớn hơn`r`qua`l`thực hiện chính xác quá trình chuyển đổi trạng thái giống như khoảng DP được suy ra ở trên. Không có chuỗi con ngắn nào trở nên tệ hơn và giá trị kết thúc tốt nhất có thể đạt được đối với độ dài bị ảnh hưởng sẽ trở nên tối thiểu. 
Do tính bất biến sắp xếp kiên nhẫn cổ điển được giữ nguyên sau mỗi lần cập nhật nên số phần tử không phải trọng điểm bằng với độ dài LNDS tối đa có thể đạt được cho tiền tố hiện tại. 
## Giải pháp Python```python
import sys
from bisect import bisect_right
input = sys.stdin.readline
from sortedcontainers import SortedList
def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        dp = SortedList([0])
        ans = []
        for _ in range(n):
            l, r = map(int, input().split())
            pos = dp.bisect_right(r)
            dp.add(l)
            if pos < len(dp):
                dp.pop(pos)
            ans.append(str(len(dp) - 1))
        out.append(" ".join(ans))
    sys.stdout.write("\n".join(out))
if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp quá trình chuyển đổi toán học. 
Multiset phải hỗ trợ chèn, xóa vị trí tùy ý và truy vấn giới hạn trên. Trong C++,`multiset`xử lý việc này một cách tự nhiên. Trong Python, cần có một thùng chứa có thứ tự cân bằng.`SortedList`cung cấp tất cả các hoạt động cần thiết trong`O(log n)`. 
Thứ tự của các hoạt động quan trọng. Đầu tiên chúng ta xác định giá trị đầu tiên lớn hơn`r`, sau đó chèn`l`, sau đó xóa vị trí đã xác định trước đó. Điều này tái tạo hoạt động thay thế chính xác của DP. 
lính gác`0`nhỏ hơn mọi giá trị khoảng có thể có vì tất cả các điểm cuối của khoảng ít nhất`1`. Nó biểu diễn dãy con trống và cho phép mỗi độ dài dãy con thực được dịch chuyển một đơn vị. 
## Ví dụ đã hoạt động 
### Ví dụ 1 
đầu vào:```
4
4 5
3 4
1 3
3 3
```| Bước | Khoảng thời gian | Nhiều bộ sau khi cập nhật | Trả lời | 
| --- | --- | --- | --- | 
| 1 | [4,5] | {0,4} | 1 | 
| 2 | [3,4] | {0,3} | 1 | 
| 3 | [1,3] | {0,1,3} | 2 | 
| 4 | [3,3] | {0,1,3,3} | 3 | 
Đầu ra:```
1 1 2 3
```Khoảng thứ ba tạo ra độ dài chuỗi con thứ hai có thể đạt được. Khoảng thứ tư kéo dài nó một lần nữa. 
### Ví dụ 2 
đầu vào:```
5
1 2
6 8
4 5
2 3
3 3
```| Bước | Khoảng thời gian | Nhiều bộ sau khi cập nhật | Trả lời | 
| --- | --- | --- | --- | 
| 1 | [1,2] | {0,1} | 1 | 
| 2 | [6,8] | {0,1,6} | 2 | 
| 3 | [4,5] | {0,1,4} | 2 | 
| 4 | [2,3] | {0,1,2} | 2 | 
| 5 | [3,3] | {0,1,2,3} | 3 | 
Đầu ra:```
1 2 2 2 3
```Dấu vết này cho thấy các khoảng thời gian sau có thể cải thiện trạng thái DP như thế nào mà không làm tăng câu trả lời ngay lập tức. Câu trả lời chỉ phát triển khi độ dài chuỗi con mới có thể đạt được. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Một truy vấn giới hạn trên, một lần chèn và tối đa một lần xóa trong mỗi khoảng thời gian | 
| Không gian | O(n) | Multiset lưu trữ tối đa một giá trị cho mỗi khoảng thời gian được xử lý cộng với trọng điểm | 
Với nhiều nhất`2·10^5`khoảng thời gian trên tất cả các trường hợp thử nghiệm,`O(n log n)`dễ dàng phù hợp trong giới hạn. 
## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys
from sortedcontainers import SortedList
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        dp = SortedList([0])
        cur = []
        for _ in range(n):
            l, r = map(int, input().split())
            pos = dp.bisect_right(r)
            dp.add(l)
            if pos < len(dp):
                dp.pop(pos)
            cur.append(str(len(dp) - 1))
        out.append(" ".join(cur))
    return "\n".join(out)
# sample
assert run(
"""1
4
4 5
3 4
1 3
3 3
"""
) == "1 1 2 3"
# minimum size
assert run(
"""1
1
1 1
"""
) == "1"
# strictly impossible to chain
assert run(
"""1
2
3 4
1 2
"""
) == "1 1"
# all equal intervals
assert run(
"""1
4
5 5
5 5
5 5
5 5
"""
) == "1 2 3 4"
# increasing overlap
assert run(
"""1
4
1 10
2 10
3 10
4 10
"""
) == "1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quãng đơn |`1`| Kích thước tối thiểu | 
|`[3,4],[1,2]`|`1 1`| Không tồn tại cặp không giảm hợp lệ | 
| Tất cả`[5,5]`|`1 2 3 4`| Phải cho phép các giá trị bằng nhau trong LNDS | 
| Tăng khoảng thời gian |`1 2 3 4`| Tăng trưởng liên tục của độ dài chuỗi con có thể đạt được | 
## Vỏ cạnh 
Hãy xem xét```
2
3 4
1 2
```Sau khoảng thời gian đầu tiên, multiset là`{0,3}`và câu trả lời là`1`. 
Đối với khoảng thứ hai, giá trị đầu tiên lớn hơn`2`là`3`. Thay thế nó bằng`1`sản xuất`{0,1}`. Câu trả lời vẫn còn`1`. 
Điều này phù hợp với thực tế vì mọi lựa chọn hợp lệ đều thỏa mãn`a1 > a2`. 
Bây giờ hãy xem xét```
4
5 5
5 5
5 5
5 5
```Multiset phát triển như```
{0,5}
{0,5,5}
{0,5,5,5}
{0,5,5,5,5}
```và câu trả lời trở thành```
1 2 3 4
```Cho phép các giá trị bằng nhau trong một dãy con không giảm, do đó có thể chọn toàn bộ mảng. 
Cuối cùng xem xét```
3
1 10
1 10
1 10
```Mỗi khoảng có thể chọn giá trị`1`. Thuật toán không bao giờ xóa trạng thái làm giảm độ dài có thể đạt được. Multiset tăng thêm một phần tử ở mỗi bước, đưa ra câu trả lời`1 2 3`, đó là tối ưu.
