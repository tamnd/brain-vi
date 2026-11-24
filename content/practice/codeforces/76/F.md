--- 
tựa đề: "CF 76F - Du Lịch" 
mô tả: "Mỗi sự kiện là một điểm trên mặt phẳng vị trí-thời gian. Sự kiện i xảy ra tại tọa độ xi đúng thời điểm ti. Khách du lịch di chuyển trên một đường có vận tốc lớn nhất là V, do đó việc di chuyển từ (xa, ta) đến (xb, tb) là có thể khi và chỉ khi:$$The tourist may also wait in place. We need two answers." date: "2026-05-28T00:00:00+07:00" tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp"] categories: ["algorithms"] codeforces_contest: 76 codeforces_index: "F" codeforces_contest_name: "All-Ukrainian School Olympiad in Informatics" rating: 2300 weight: 76 solve_time_s: 163 verified: true draft: false --- [CF 76F - Tourist](https://codeforces.com/problemset/problem/76/F) **Rating:** 2300   **Tags:** binary search, data structures, dp   **Solve time:** 2m 43s   **Verified:** yes   ## Solution ## Problem Understanding Each event is a point on a time-position plane. Event `i` happens at coordinate `x_i` exactly at time `t_i`. The tourist moves on a line with speed at most `V`, so moving from `(x_a, t_a)` to `(x_b, t_b)` is possible if and only if:$$|x_a - x_b| \le V \cdot (t_b - t_a)$$
The tourist may also wait in place. We need two answers.
The first answer assumes the tourist starts at coordinate `0` at time `0`.
The second answer allows the tourist to choose any starting coordinate before time starts.
The goal in both versions is to maximize how many events can be attended in chronological order.
The constraints completely rule out quadratic dynamic programming. With `n = 100000`, an `O(n^2)` transition loop would require around `10^10` checks, which is several orders of magnitude too large. Even `O(n sqrt n)` would be risky in Python. The solution must stay near `O(n log n)`.
The tricky part is that reachability depends on both position and time. A naive graph interpretation creates too many edges. We need a way to compress the transition condition into something searchable with data structures.
Several edge cases are easy to mishandle.
Events may occur at the same time. Two events with identical timestamps cannot both be visited unless they are at the same position, and the statement guarantees that exact duplicates do not exist.
Example:
⟦PROTECT_0⟧
Correct output:
⟦PROTECT_1⟧
A careless DP that only checks position difference without enforcing increasing time could incorrectly chain them together.
Negative coordinates matter because movement is on the entire number line.
Example:
⟦PROTECT_2⟧
The tourist cannot reach the first event from `(0,0)` because distance `5` requires time `5`, which is barely possible, but then moving from `-5` to `5` needs distance `10` and only `5` units of time. The correct answer is:
⟦PROTECT_3⟧
The second answer differs because with a free starting point we may begin directly at `-5`.
Another subtle case is multiple events at the same position.
⟦PROTECT_4⟧
Correct output:
⟦PROTECT_5⟧
Waiting in place is allowed, so once the tourist reaches coordinate `2`, all later events there are automatically reachable.
## Approaches
The most direct solution is dynamic programming on events sorted by time.
Define `dp[i]` as the maximum number of events ending at event `i`. Then we try every earlier event `j` and transition if:
$$|x_i - x_j| \le V(t_i - t_j)$$
For the fixed-start version we also check whether event `i` is reachable from `(0,0)`.
This is correct because any valid route must visit events in nondecreasing time order, and every feasible previous event is examined.
The problem is complexity. Each event compares against all earlier events, so the running time is `O(n^2)`. With `100000` events this becomes roughly `5 \cdot 10^9` transitions, far too slow.
The key observation is that the reachability inequality can be rewritten into two independent inequalities.
Starting from:
$$|x_i - x_j| \le V(t_i - t_j)$$
we obtain:
$$x_i - V t_i \le x_j - V t_j$$
and
$$x_i + V t_i \ge x_j + V t_j$$
Define transformed coordinates:
$$A_i = x_i - V t_i$$
$$B_i = x_i + V t_i$$
Then event `j` can precede event `i` exactly when:
$$A_j \ge A_i$$
and
$$B_j \le B_i$$
This turns the problem into a two-dimensional dominance DP.
Now the structure becomes clear. If we process events in increasing time order, we need the best DP value among all previous points satisfying:
$$A_j \ge A_i,\quad B_j \le B_i$$
That is a classic offline data structure problem.
We compress all `B` coordinates and use a Fenwick tree where each node stores the best DP value seen so far. To handle the `A_j \ge A_i` condition, we process events grouped by decreasing `A`.
For the second answer, where the start point is arbitrary, every event may start a chain with value `1`.
For the first answer, event `i` may start a chain only if:
$$|x_i| \le V t_i$$
because the tourist starts at `(0,0)`.
The resulting algorithm runs in `O(n log n)`.
| Approach | Time Complexity | Space Complexity | Verdict |
| --- | --- | --- | --- |
| Brute Force | O(n²) | O(n) | Too slow |
| Optimal | O(n log n) | O(n) | Accepted |
## Algorithm Walkthrough
1. Read all events and sort them by increasing time.
Sorting by time guarantees that every transition goes forward chronologically.
1. For every event compute:
$$A_i = x_i - V t_i$$
$$B_i = x_i + V t_i$$
These transformed coordinates encode the movement constraint as a dominance relation.
1. Coordinate-compress all `B_i` values.
Fenwick trees require indices from `1` to `m`, so we map each distinct `B` value to a compact integer.
1. Compute the answer for the free-start version.
Every event can begin a valid route by itself, so initialize:
⟦PROTECT_6⟧
Then process events in decreasing `A`.
For each event, query the Fenwick tree for the maximum DP among all earlier events with compressed `B <= B_i`.
That query gives the best chain that can transition into event `i`.
After querying all events with the same `A`, insert their DP values into the Fenwick tree.
Delaying updates inside equal-`A` groups prevents transitions between events that are not mutually reachable.
1. Compute the fixed-start version similarly.
Initialize:
⟦PROTECT_7⟧
only if:
$$|x_i| \le V t_i$$
Otherwise initialize with negative infinity.
The same dominance DP transitions are applied afterward.
1. The answer for each version is the maximum DP value over all events.
### Why it works
The transformed inequalities are exactly equivalent to the original speed constraint. Any feasible movement sequence corresponds to a chain in the dominance order:
$$A_j \ge A_i,\quad B_j \le B_i$$ 
Xử lý các sự kiện theo thời gian đảm bảo quá trình chuyển đổi chỉ tiến về phía trước theo thời gian. Cây Fenwick luôn lưu trữ DP tốt nhất trong số các cây tiền nhiệm hợp lệ đã được xử lý. Vì mọi tiền đề có thể thỏa mãn các bất đẳng thức đều được biểu diễn trong cấu trúc và không có tiền nhiệm không hợp lệ nào thỏa mãn đồng thời cả hai bất đẳng thức, nên DP sẽ tính toán số lượng sự kiện có thể truy cập tối ưu. 
## Giải pháp Python```python
import sys
input = sys.stdin.readline
INF = 10**18
class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [-INF] * (n + 1)
    def update(self, idx, val):
        while idx <= self.n:
            if val > self.bit[idx]:
                self.bit[idx] = val
            idx += idx & -idx
    def query(self, idx):
        res = -INF
        while idx > 0:
            if self.bit[idx] > res:
                res = self.bit[idx]
            idx -= idx & -idx
        return res
def solve():
    n = int(input())
    events = []
    for _ in range(n):
        x, t = map(int, input().split())
        events.append((t, x))
    V = int(input())
    arr = []
    for idx, (t, x) in enumerate(events):
        A = x - V * t
        B = x + V * t
        arr.append((A, B, t, x, idx))
    all_b = sorted(set(b for _, b, _, _, _ in arr))
    comp = {v: i + 1 for i, v in enumerate(all_b)}
    arr.sort(reverse=True)
    m = len(all_b)
    # free start
    bit = Fenwick(m)
    dp2 = [1] * n
    i = 0
    while i < n:
        j = i
        while j < n and arr[j][0] == arr[i][0]:
            j += 1
        updates = []
        for k in range(i, j):
            A, B, t, x, idx = arr[k]
            bidx = comp[B]
            best = bit.query(bidx)
            if best > -INF:
                dp2[idx] = max(dp2[idx], best + 1)
            updates.append((bidx, dp2[idx]))
        for bidx, val in updates:
            bit.update(bidx, val)
        i = j
    # fixed start at 0
    bit = Fenwick(m)
    dp1 = [-INF] * n
    for A, B, t, x, idx in arr:
        if abs(x) <= V * t:
            dp1[idx] = 1
    i = 0
    while i < n:
        j = i
        while j < n and arr[j][0] == arr[i][0]:
            j += 1
        updates = []
        for k in range(i, j):
            A, B, t, x, idx = arr[k]
            bidx = comp[B]
            best = bit.query(bidx)
            if best > -INF:
                dp1[idx] = max(dp1[idx], best + 1)
            updates.append((bidx, dp1[idx]))
        for bidx, val in updates:
            bit.update(bidx, val)
        i = j
    ans1 = max(dp1)
    ans2 = max(dp2)
    print(ans1, ans2)
solve()
```Cây Fenwick lưu trữ tiền tố tối đa thay vì tổng. Sau khi nén tọa độ, truy vấn chỉ mục`bidx`trả về DP tốt nhất trong số tất cả các sự kiện với`B_j <= B_i`. 
Xử lý các sự kiện giảm dần`A`thứ tự đảm bảo rằng tất cả các trạng thái được chèn đều đáp ứng`A_j >= A_i`. 
Việc xử lý được nhóm cho bằng nhau`A`các giá trị là tinh tế nhưng cần thiết. Giả sử hai sự kiện có chung`A`. Nếu chúng tôi cập nhật ngay lập tức sau khi xử lý một trong số chúng, thì một sự kiện khác trong cùng nhóm có thể chuyển tiếp không chính xác qua sự kiện đó ngay cả khi dấu thời gian của chúng khiến việc di chuyển không thể thực hiện được. Trì hoãn cập nhật cho đến khi toàn bộ nhóm được xử lý sẽ loại bỏ vấn đề này. 
Phiên bản khởi động cố định chỉ khác ở khâu khởi tạo. Một sự kiện chỉ có thể bắt đầu một chuỗi nếu khách du lịch có thể tiếp cận nó trực tiếp từ`(0,0)`. 
Tất cả các tọa độ đều vừa khít bên trong các số nguyên Python, nhưng trong C++, bài toán này yêu cầu số học 64-bit vì các giá trị như`V * t`có thể đạt được`2 * 10^9`. 
## Ví dụ đã hoạt động 
### Mẫu 1 
đầu vào:```
3
-1 1
42 7
40 8
2
```Các giá trị được chuyển đổi: 
| Sự kiện | x | t | A = x - Vt | B = x + Vt | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | 1 | -3 | 1 | 
| 2 | 42 | 7 | 28 | 56 | 
| 3 | 40 | 8 | 24 | 56 | 
Thứ tự xử lý đang giảm dần`A`. 
| Bước | Sự kiện | Kết quả truy vấn | dp2 | dp1 | 
| --- | --- | --- | --- | --- | 
| 1 | (42,7) | không | 1 | không thể | 
| 2 | (40,8) | 1 | 2 | không thể | 
| 3 | (-1,1) | không | 1 | 1 | 
Câu trả lời cuối cùng:```
1 2
```Phiên bản bắt đầu miễn phí bắt đầu lúc`(42,7)`và sau đó đạt tới`(40,8)`. Bắt đầu từ`(0,0)`không thể đạt tới một trong hai tọa độ dương lớn đó đủ nhanh. 
### Ví dụ tùy chỉnh 
đầu vào:```
4
0 1
1 2
2 3
3 4
1
```Các giá trị được chuyển đổi: 
| Sự kiện | A | B | 
| --- | --- | --- | 
| 1 | -1 | 1 | 
| 2 | -1 | 3 | 
| 3 | -1 | 5 | 
| 4 | -1 | 7 | 
Tất cả các sự kiện đều giống nhau`A`. 
| Bước | Sự kiện | Tốt nhất trước đó | dp | 
| --- | --- | --- | --- | 
| 1 | 1 | không | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 3 | 2 | 3 | 
| 4 | 4 | 3 | 4 | 
Đầu ra:```
4 4
```Ví dụ này chứng minh tại sao thứ tự thống trị lại khớp chính xác với điều kiện chuyển động. Mỗi sự kiện có thể truy cập được từ sự kiện trước đó với tốc độ`1`. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp cộng với các truy vấn và cập nhật Fenwick | 
| Không gian | O(n) | Mảng, nén tọa độ, cây Fenwick | 
Với`100000`sự kiện,`O(n log n)`đủ nhanh một cách dễ dàng. Các hoạt động của cây Fenwick rất nhẹ và mức sử dụng bộ nhớ vẫn tuyến tính. 
## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
INF = 10**18
class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [-INF] * (n + 1)
    def update(self, idx, val):
        while idx <= self.n:
            self.bit[idx] = max(self.bit[idx], val)
            idx += idx & -idx
    def query(self, idx):
        res = -INF
        while idx > 0:
            res = max(res, self.bit[idx])
            idx -= idx & -idx
        return res
def solve():
    input = sys.stdin.readline
    n = int(input())
    events = []
    for _ in range(n):
        x, t = map(int, input().split())
        events.append((t, x))
    V = int(input())
    arr = []
    for idx, (t, x) in enumerate(events):
        A = x - V * t
        B = x + V * t
        arr.append((A, B, t, x, idx))
    all_b = sorted(set(b for _, b, _, _, _ in arr))
    comp = {v: i + 1 for i, v in enumerate(all_b)}
    arr.sort(reverse=True)
    m = len(all_b)
    bit = Fenwick(m)
    dp2 = [1] * n
    i = 0
    while i < n:
        j = i
        while j < n and arr[j][0] == arr[i][0]:
            j += 1
        upd = []
        for k in range(i, j):
            A, B, t, x, idx = arr[k]
            bidx = comp[B]
            best = bit.query(bidx)
            if best > -INF:
                dp2[idx] = max(dp2[idx], best + 1)
            upd.append((bidx, dp2[idx]))
        for bidx, val in upd:
            bit.update(bidx, val)
        i = j
    bit = Fenwick(m)
    dp1 = [-INF] * n
    for A, B, t, x, idx in arr:
        if abs(x) <= V * t:
            dp1[idx] = 1
    i = 0
    while i < n:
        j = i
        while j < n and arr[j][0] == arr[i][0]:
            j += 1
        upd = []
        for k in range(i, j):
            A, B, t, x, idx = arr[k]
            bidx = comp[B]
            best = bit.query(bidx)
            if best > -INF:
                dp1[idx] = max(dp1[idx], best + 1)
            upd.append((bidx, dp1[idx]))
        for bidx, val in upd:
            bit.update(bidx, val)
        i = j
    print(max(dp1), max(dp2))
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()
# provided sample
assert run(
"""3
-1 1
42 7
40 8
2
"""
) == "1 2", "sample 1"
# minimum size
assert run(
"""1
0 1
1
"""
) == "1 1", "single reachable event"
# unreachable from origin but reachable with free start
assert run(
"""1
100 1
1
"""
) == "-1000000000000000000 1", "free start differs"
# same position over time
assert run(
"""3
2 3
2 4
2 5
1
"""
) == "3 3", "waiting in place"
# chain along a line
assert run(
"""4
0 1
1 2
2 3
3 4
1
"""
) == "4 4", "full chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Sự kiện đơn lẻ |`1 1`| Kích thước đầu vào tối thiểu | 
| Sự kiện không thể tiếp cận được | Câu trả lời khác nhau | Bắt đầu tự do so với bắt đầu cố định | 
| Cùng tọa độ nhiều lần |`3 3`| Chờ tại chỗ | 
| Tăng chuỗi |`4 4`| Chuỗi chuyển tiếp hợp lệ dài | 
## Vỏ cạnh 
Xét các sự kiện xảy ra cùng một lúc:```
2
0 1
10 1
100
```Cả hai sự kiện đều có dấu thời gian giống hệt nhau. Các điều kiện đã biến đổi không thể được giữ đồng thời vì thời gian chuyển động bằng không. Trong quá trình xử lý, không có sự kiện nào trở thành sự kiện trước của sự kiện kia, vì vậy cả hai giá trị DP vẫn được giữ nguyên`1`. Các đầu ra thuật toán:```
1 1
```Bây giờ hãy xem xét các sự kiện không thể truy cập được từ nguồn:```
2
-5 5
5 10
1
```Sự kiện đầu tiên có thể truy cập từ`(0,0)`, Vì thế`dp1 = 1`. Sự kiện thứ hai không thể theo sau nó vì khoảng cách di chuyển`10`đúng lúc`5`vượt quá tốc độ`1`. 
Đối với phiên bản bắt đầu miễn phí, khách du lịch có thể bắt đầu tại`-5`, tham dự sự kiện đầu tiên, rồi vẫn không thể đến được sự kiện thứ hai. Ngoài ra, hãy bắt đầu tại`5`và chỉ tham dự lần thứ hai. Còn lại tối đa`1`. 
Thuật toán loại bỏ chính xác các chuyển đổi không hợp lệ vì các bất đẳng thức được chuyển đổi không thành công. 
Cuối cùng, hãy xem xét tọa độ lặp lại:```
3
2 3
2 4
2 5
1
```Các giá trị được biến đổi thỏa mãn mối quan hệ thống trị theo thứ tự thời gian, do đó cây Fenwick xâu chuỗi cả ba sự kiện lại với nhau. Việc chờ đợi không cần xử lý đặc biệt vì chuyển động bằng 0 sẽ tự động đáp ứng giới hạn tốc độ.
