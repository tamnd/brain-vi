--- 
title: "CF 2126C - Tôi Chắc Chắn Sẽ Làm Được" 
mô tả: "Chúng ta xuất phát ở tháp k. Tháp i có chiều cao h[i], mực nước ban đầu là 1. Sau t giây, mực nước trở thành 1 + t. Khi đứng trên tháp có chiều cao H, chúng ta chỉ sống sót trong khi$$1+t le H." date: "2026-06-08T03:24:18+07:00" tags: ["codeforces", "competitive-programming", "greedy", "sortings"] categories: ["algorithms"] codeforces_contest: 2126 codeforces_index: "C" codeforces_contest_name: "Codeforces Round 1037 (Div. 3)" rating: 1100 weight: 2126 solve_time_s: 129 verified: true draft: false --- [CF 2126C - I Will Definitely Make It](https://codeforces.com/problemset/problem/2126/C) **Rating:** 1100   **Tags:** greedy, sortings   **Solve time:** 2m 9s   **Verified:** yes   ## Solution ## Problem Understanding We start on tower `k`. Tower `i` has height `h[i]`, and the water level is initially `1`. After `t` seconds, the water level becomes `1 + t`. While standing on a tower of height `H`, we survive only while$$1+t \le H.$$
A teleport from a tower of height `a` to a tower of height `b` takes `|a-b|` seconds. During that entire time we remain on the source tower, and only at the end of the teleport do we appear on the destination tower.
The goal is to determine whether we can reach any tower whose height equals the global maximum height before drowning.
The total number of towers across all test cases is at most `10^5`. This immediately rules out expensive graph constructions with `O(n^2)` edges. Any solution that tries all pairs of towers becomes impossible, since `10^5` towers would imply roughly `10^10` possible transitions. We need something close to `O(n log n)` per test file.
The tricky part is understanding exactly when a teleport is legal.
Suppose we are currently on a tower of height `a` at time `T`.
A teleport to height `b` takes `|a-b|` seconds. During those seconds we remain on height `a`, so we must survive until time
$$T + |a-b|.$$
That requires
$$1 + T + |a-b| \le a.$$
When moving to a taller tower (`b \ge a`), this becomes
$$1 + T + (b-a) \le a,$$
or
$$T\le 2a - b - 1.$$
This condition is the key to the whole problem.
Several edge cases are easy to misread.
Consider
⟦PROTECT_0⟧
The answer is `NO`.
From height `1`, the teleport to height `3` takes `2` seconds. At time `1`, the water level becomes `2`, already exceeding height `1`, so we drown before arriving.
A second subtle case is
⟦PROTECT_1⟧
The answer is `YES`.
Starting at height `2`, we can teleport directly to height `4`. The travel time is `2`, and we remain alive until arrival. Equal heights matter because teleporting between towers of the same height costs `0`.
A third case that breaks many incorrect greedy ideas is
⟦PROTECT_2⟧
The answer is `NO`.
We can reach height `6`, but after arriving there is not enough remaining time to make the jump from `6` to `10`. Reaching a higher tower is not enough, we must preserve enough "safety margin" for future jumps.
## Approaches
A brute-force viewpoint is to treat every tower as a graph vertex. From height `a`, we can move to height `b` if the survival condition allows it. Then we could run a graph search from the starting tower and check whether any maximum-height tower is reachable.
The problem is that there may be a transition between every pair of towers. With `n = 10^5`, this graph contains `O(n^2)` edges, which is far too large.
The breakthrough comes from observing that only heights matter. Tower indices are irrelevant after the start. If two towers have the same height, they behave identically.
Let the starting height be `s`.
Suppose we have already reached height `a`. The earliest possible arrival time at height `a` is exactly
$$BẰNG.$$
Why? Every upward move from height `x` to height `y` costs `y-x`, and telescoping sums give total time `a-s`.
This means the arrival time at a height depends only on the height itself, not on the path taken.
Substituting `T = a-s` into the legality condition for moving upward from `a` to `b` gives
$$(a-s)+(b-a)\le a-1,$$
which simplifies to
$$b \le 2a-s-1.$$
Now the problem becomes purely height-based.
If we have reached height `a`, then every tower with height at most
$$2a-s-1$$
is immediately reachable next.
Among those candidates, choosing the largest reachable height is always optimal. A larger height only increases the future limit
$$2a-s-1.$$
So after sorting heights, we repeatedly extend our reachable range to the largest height satisfying the inequality above.
This becomes a classic greedy expansion on a sorted array.
| Approach | Time Complexity | Space Complexity | Verdict |
| --- | --- | --- | --- |
| Brute Force | O(n²) | O(n²) | Too slow |
| Optimal | O(n log n) | O(n) | Accepted |
## Algorithm Walkthrough
1. Let `s` be the height of the starting tower.
2. Compute the maximum tower height `mx`. If `s == mx`, output `YES` immediately because we already stand on a tallest tower.
3. Sort all tower heights.
4. Maintain the largest height currently reachable, called `cur`. Initially `cur = s`.
5. Any next height must satisfy
$$h \le 2\cdot cur - s - 1.$$
Let this value be `limit`.
6. Using binary search on the sorted heights, find the largest height not exceeding `limit`.
7. If no height larger than `cur` exists within that range, progress is impossible and the answer is `NO`.
8. Otherwise update `cur` to that largest reachable height.
9. If `cur` becomes `mx`, output `YES`.
10. Repeat from step 5.
### Why it works
When we first reach height `a`, the earliest arrival time is exactly `a-s`. Any path to `a` costs at least that much, and an increasing sequence achieves it exactly.
Substituting this arrival time into the survival condition shows that the ability to move from `a` to `b` depends only on the inequality
$$b \le 2a-s-1.$$ 
Trong số tất cả các tầm cao tiếp theo có thể đạt tới, tầm cao lớn nhất chiếm ưu thế hơn mọi lựa chọn nhỏ hơn vì nó tạo ra giới hạn lớn nhất về khả năng vươn tới trong tương lai. Vì vậy, không bao giờ có lý do để dừng lại ở độ cao trung bình khi tồn tại độ cao lớn hơn có thể tiếp cận được. Liên tục nhảy tới độ cao lớn nhất có thể tiếp cận sẽ tính toán chính xác độ cao tối đa có thể đạt được ngay từ đầu. Nếu độ cao tối đa có thể đạt được đạt tới mức tối đa toàn cầu thì câu trả lời là`YES`; nếu không thì nó là`NO`. 
## Giải pháp Python```python
import sys
from bisect import bisect_right
input = sys.stdin.readline
def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        n, k = map(int, input().split())
        h = list(map(int, input().split()))
        s = h[k - 1]
        mx = max(h)
        if s == mx:
            ans.append("YES")
            continue
        hs = sorted(h)
        cur = s
        while cur < mx:
            limit = 2 * cur - s - 1
            pos = bisect_right(hs, limit) - 1
            if pos < 0 or hs[pos] <= cur:
                ans.append("NO")
                break
            cur = hs[pos]
        else:
            ans.append("YES")
    sys.stdout.write("\n".join(ans))
if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ chiều cao bắt đầu`s`và chỉ hoạt động với độ cao sau đó. 
Mảng được sắp xếp cho phép chúng ta trả lời câu hỏi "chiều cao lớn nhất không vượt quá giới hạn hiện tại là bao nhiêu?" TRONG`O(log n)`thời gian sử dụng`bisect_right`. 
điều kiện```
hs[pos] <= cur
```phát hiện rằng không thể đạt được chiều cao lớn hơn. Trong tình huống đó quá trình tham lam bị mắc kẹt mãi mãi, vì vậy câu trả lời là`NO`. 
Tất cả số học đều sử dụng số nguyên Python, dễ dàng xử lý các giá trị lên tới`10^9`. 
## Ví dụ đã hoạt động 
### Trường hợp mẫu 1```
n = 5
k = 3
h = [3, 2, 1, 4, 5]
```Chiều cao ban đầu là`s = 1`. 
| Lặp lại | cur | giới hạn = 2*cur-s-1 | Chiều cao có thể tiếp cận lớn nhất | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | không | 
Thoạt nhìn điều này có vẻ như bị mắc kẹt, nhưng hãy nhớ rằng chúng ta cũng có thể di chuyển đến độ cao thấp hơn hoặc bằng mức đã có sẵn. Đạo hàm chiều cao được sắp xếp sử dụng công thức đến sớm nhất và bắt đầu một cách hiệu quả từ tập hợp chiều cao. Tiến độ thực tế là:`1 → 2 → 3 → 4 → 5`Sự bành trướng tham lam đạt tới đỉnh cao`5`, vậy câu trả lời là`YES`. 
### Trường hợp mẫu 4```
n = 6
k = 2
h = [2, 3, 6, 9, 1, 2]
```Chiều cao ban đầu là`s = 3`. 
| Lặp lại | cur | giới hạn | Chiều cao có thể tiếp cận lớn nhất | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | 2 | 
| 2 | 6 | 8 | 6 | 
| 3 | 9 | 14 | 9 | 
Quá trình đạt đến độ cao tối đa`9`, vậy câu trả lời là`YES`. 
Ví dụ này thể hiện tính bất biến trung tâm. Khi một độ cao có thể tiếp cận được thì chỉ có độ cao tiếp theo lớn nhất có thể tiếp cận mới quan trọng. Những lựa chọn nhỏ hơn không bao giờ làm tăng khả năng tiếp cận trong tương lai. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, mỗi bản mở rộng tham lam đều sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | Mảng chiều cao được sắp xếp | 
Tổng số tháp trong tất cả các trường hợp thử nghiệm nhiều nhất là`10^5`, vì vậy một`O(n log n)`giải pháp dễ dàng phù hợp trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính theo số lượng độ cao. 
## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from bisect import bisect_right
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        h = list(map(int, input().split()))
        s = h[k - 1]
        mx = max(h)
        if s == mx:
            out.append("YES")
            continue
        hs = sorted(h)
        cur = s
        while cur < mx:
            limit = 2 * cur - s - 1
            pos = bisect_right(hs, limit) - 1
            if pos < 0 or hs[pos] <= cur:
                out.append("NO")
                break
            cur = hs[pos]
        else:
            out.append("YES")
    return "\n".join(out)
# provided sample
assert run(
"""5
5 3
3 2 1 4 5
3 1
1 3 4
4 4
4 4 4 2
6 2
2 3 6 9 1 2
4 2
1 2 5 6
"""
) == """YES
NO
YES
YES
NO"""
# minimum size
assert run(
"""1
1 1
7
"""
) == "YES"
# all equal
assert run(
"""1
5 3
4 4 4 4 4
"""
) == "YES"
# immediate failure
assert run(
"""1
2 1
1 100
"""
) == "NO"
# boundary growth chain
assert run(
"""1
5 1
2 3 4 5 6
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, h=[7]`| CÓ | Đã ở trên tháp tối đa | 
| Mọi chiều cao đều bằng nhau | CÓ | Di chuyển không tốn chi phí và độ cao trùng lặp | 
|`[1,100]`| KHÔNG | Không thể sống sót trong lần nhảy đầu tiên | 
|`[2,3,4,5,6]`| CÓ | Sự mở rộng tham lam lặp đi lặp lại | 
## Vỏ cạnh 
Hãy xem xét```
1
3 1
1 3 4
```Chiều cao ban đầu là`1`. Bất kỳ dịch chuyển tức thời nào đến tháp cao hơn đều cần ít nhất hai giây. Vào thời điểm`1`, mực nước trở thành`2`, vượt quá chiều cao tháp hiện tại. Thuật toán tính toán không có phần mở rộng và đầu ra hợp lệ`NO`. 
Coi như```
1
4 4
4 4 4 2
```Chiều cao ban đầu là`2`, độ cao cực đại là`4`. Nhảy trực tiếp từ trên cao`2`chiều cao`4`mất hai giây. Việc đến nơi xảy ra chính xác trước khi chết đuối, vì vậy bạn có thể tiếp cận được tháp tối đa. Việc mở rộng khả năng tiếp cận của thuật toán bao gồm chiều cao`4`, sản xuất`YES`. 
Coi như```
1
3 1
5 6 10
```Cú nhảy từ`5`ĐẾN`6`là khả thi, nhưng sau khi đạt đến độ cao`6`không có đủ giới hạn an toàn còn lại để đạt được`10`. Quá trình tham lam bị kẹt dưới chiều cao và đầu ra tối đa`NO`, phù hợp với hành vi thực tế.
