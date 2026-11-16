--- 
title: "CF 1890D - Kế hoạch kết nối của Doremy" 
mô tả: "Chúng tôi có$n$thành phố, nơi thành phố$i$chứa$ai$mọi người. Ban đầu mọi thành phố đều bị cô lập. Một ranh giới giữa các thành phố$i$Và$j$chỉ có thể được thêm vào nếu tổng dân số của tất cả các thành phố thuộc các thành phần được kết nối có chứa$i$hoặc$j$ít nhất là$$i cdot j cdot c." date: "2026-06-09T01:09:44+07:00" tags: ["codeforces", "competitive-programming", "graphs", "greedy", "math", "sortings"] categories: ["algorithms"] codeforces_contest: 1890 codeforces_index: "D" codeforces_contest_name: "Codeforces Round 906 (Div. 2)" rating: 1700 weight: 1890 solve_time_s: 138 verified: true draft: false --- [CF 1890D - Doremy's Connecting Plan](https://codeforces.com/problemset/problem/1890/D) **Rating:** 1700   **Tags:** graphs, greedy, math, sortings   **Solve time:** 2m 18s   **Verified:** yes   ## Solution ## Problem Understanding We have $n$ cities, where city $i$ contains $a_i$ people. Initially every city is isolated. An edge between cities $i$ and $j$ can be added only if the total population of all cities belonging to the connected components containing $i$ or $j$ is at least$$tôi \cdot j \cdot c.$$
Whenever we connect two different components, their populations merge and become available for future operations.
The task is to determine whether there exists some sequence of edge additions that eventually makes the whole graph connected.
The constraints are the first clue. The total number of cities across all test cases is at most $2 \cdot 10^5$, which strongly suggests an $O(n \log n)$ solution. Any algorithm that repeatedly scans all pairs of cities would require $O(n^2)$ or worse operations, which would be far too slow for $n=2\cdot10^5$.
The tricky part is that the validity of an edge depends on the current connected components, not only on the endpoints. After several merges, a previously impossible edge may become possible because the component population has grown.
A common mistake is to think that any pair of cities can serve as the next merge candidate. The city indices appear in the cost formula, and index $1$ is special because
$$1 \cdot i \cdot c = i c$$
is the smallest possible cost involving city $i$.
Consider:
⟦PROTECT_0⟧
City $1$ alone already has population $100$. It can connect to city $2$ because $100+1 \ge 20$, then to city $3$ because $101+1 \ge 30$. The answer is YES.
A naive strategy that tries to connect arbitrary cheapest pairs first may fail to notice that city $1$ is the natural hub.
Another subtle case is:
⟦PROTECT_1⟧
The answer is NO. Although the total population is positive, city $1$ cannot connect to city $2$ because $0+0 < 2$, and city $1$ cannot connect to city $3$ because $0+2 < 3$. No merge is possible.
The solution must correctly identify when the growth process gets stuck before reaching all cities.
## Approaches
A brute-force simulation would treat each city as its own component and repeatedly search for any pair of components that can be merged.
To test whether two components can be connected, we would need their total population and some pair of cities capable of satisfying the inequality. Even with a disjoint-set structure, searching all component pairs after every merge is expensive. There can be $O(n)$ merges, and each step may examine $O(n^2)$ candidates, leading to $O(n^3)$ behavior in the worst case.
The key observation is that city $1$ dominates the problem.
Suppose we already have a component containing city $1$, with total population $S$. To attach city $i$ directly to this component, we need
$$S + a_i \ge i \cdot c.$$
This is the cheapest possible way to bring city $i$ into the growing component, because every other city index $j\ge1$ gives
$$i \cdot j \cdot c \ge i \cdot c.$$
If city $i$ cannot be attached to the component containing city $1$, then connecting it through some other city cannot be easier.
This transforms the problem into a growth process.
Start with the component containing city $1$. Its current population is $a_1$.
For every city $i>1$, define its contribution:
$$Gain_i = a_i - i c.$$
If the current population is $S$, then city $i$ is attachable exactly when
$$S + Gain_i \ge 0.$$
After attaching it, the population becomes
$$S \leftarrow S + a_i.$$
Among all currently attachable cities, we should take the one with the largest contribution $gain_i$. This maximizes future population growth and can never hurt.
The resulting process is identical to a classic greedy problem. We sort cities by $gain_i$ in descending order and keep adding cities while the current population is large enough to satisfy the corresponding requirement.
If every city can be added, the graph can be connected. Otherwise it cannot.
| Approach | Time Complexity | Space Complexity | Verdict |
| --- | --- | --- | --- |
| Brute Force | $O(n^3)$ worst case | $O(n)$ | Too slow |
| Optimal Greedy + Sorting | $O(n \log n)$ | $O(n)$ | Accepted |
## Algorithm Walkthrough
1. Start with the component containing city $1$. Let its current population be $S=a_1$.
2. For every city $i\ge2$, compute
$$Gain_i = a_i - i c.$$
This quantity measures how much population remains after paying the minimum possible connection cost involving city $1$.
3. Sort all cities $i\ge2$ by $gain_i$ in descending order.
4. Process the sorted cities from largest gain to smallest.
5. For the current city, check whether
$$S + Gain_i \ge 0.$$
This is equivalent to
$$S + a_i \ge i c,$$
meaning the city can be connected directly to the component containing city $1$.
6. If the condition fails for some city, output `"NO"`.
The remaining cities have gain no larger than the current one, so none of them can be connected either.
7. Otherwise merge the city into the component and update
$$S \leftarrow S + a_i.$$
8. After all cities are processed successfully, output `"YES"`.
### Why it works
Let $S$ be the population of the component containing city $1$.
For any city $i$, the cheapest possible way to attach it to that component is using an edge involving city $1$, whose requirement is $i c$. Any edge using another city $j$ requires $i j c$, which is never smaller.
Hence a city is usable exactly when $S+a_i \ge i c$.
Suppose several cities are currently usable. Choosing the city with the largest gain adds the most population to $S$, producing the largest possible future component. If a solution exists after choosing some other city, it also exists after choosing the largest-gain city because the component population is at least as large afterward.
This exchange argument shows that the greedy choice is always safe. If the greedy process gets stuck at some city, no alternative ordering could have produced a larger current population, so no valid sequence exists.
## Python Solution
⟦PROTECT_2⟧
The variable `cur` stores the population of the component containing city $1$.
For every other city we store two values. The first is `gain = a[i] - i*c`, which determines whether the city can currently be attached. The second is its population, which will be added to `cur` after the merge.
Sorting by descending gain implements the greedy choice. The check
⟦PROTECT_3⟧
is exactly the condition
$$cur + a_i < i c.$$ 
Nếu thất bại, thành phố không thể được kết nối. Vì tất cả lợi ích còn lại thậm chí còn nhỏ hơn nên không thành phố nào sau này có thể được kết nối. 
Số nguyên Python tự động xử lý các giá trị lên tới$10^{12}$và hơn thế nữa, do đó không có nguy cơ tràn. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, số học 64 bit sẽ được yêu cầu. 
## Ví dụ đã hoạt động 
### Ví dụ 1 
đầu vào:```
4 10
0 20 15 10
```Lợi nhuận tính toán: 
| Thành phố | Dân số | Gain = ai - i*c | 
| --- | --- | --- | 
| 2 | 20 | 0 | 
| 3 | 15 | -15 | 
| 4 | 10 | -30 | 
Thứ tự sắp xếp vẫn còn$2,3,4$. 
| Bước | S hiện tại | Đạt được | Kiểm tra S+Gain >= 0 | S mới | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | - | - | 0 | 
| Thành phố 2 | 0 | 0 | Có | 20 | 
| Thành phố 3 | 20 | -15 | Có | 35 | 
| Thành phố 4 | 35 | -30 | Có | 45 | 
Tất cả các thành phố đều được đính kèm, vì vậy câu trả lời là CÓ. 
Ví dụ này cho thấy thành phần này phát triển như thế nào theo thời gian. Việc sáp nhập sớm làm tăng dân số sẵn có và giúp việc sáp nhập sau này có thể thực hiện được. 
### Ví dụ 2 
đầu vào:```
5 2
1 1 3 1 1
```Lợi nhuận tính toán: 
| Thành phố | Dân số | Đạt được | 
| --- | --- | --- | 
| 2 | 1 | -3 | 
| 3 | 3 | -3 | 
| 4 | 1 | -7 | 
| 5 | 1 | -9 | 
Lợi nhuận được sắp xếp:$-3,-3,-7,-9$. 
| Bước | S hiện tại | Đạt được | Kiểm tra S+Gain >= 0 | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | - | - | 
| Thành phố đầu tiên | 1 | -3 | Không | 
Quá trình dừng ngay lập tức nên câu trả lời là KHÔNG. 
Điều này chứng tỏ rằng tổng dân số dương là không đủ. Thành phần chứa thành phố$1$phải có khả năng bắt đầu phát triển. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp$n-1$lợi ích chiếm ưu thế | 
| Không gian |$O(n)$| Lưu trữ lợi nhuận và dân số | 
Tổng của tất cả$n$giá trị trên các trường hợp thử nghiệm là nhiều nhất$2\cdot10^5$. MỘT$O(n \log n)$thuật toán dễ dàng nằm gọn trong giới hạn 1 giây trong Python và mức sử dụng bộ nhớ ở mức dưới giới hạn 256 MB một cách thoải mái. 
## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []
        for _ in range(t):
            n, c = map(int, input().split())
            a = list(map(int, input().split()))
            cur = a[0]
            cities = []
            for i in range(2, n + 1):
                cities.append((a[i - 1] - i * c, a[i - 1]))
            cities.sort(reverse=True)
            ok = True
            for gain, pop in cities:
                if cur + gain < 0:
                    ok = False
                    break
                cur += pop
            out.append("YES" if ok else "NO")
        return "\n".join(out)
    return solve()
# provided sample
assert run(
"""7
4 10
0 20 15 10
2 1
1 1
5 1
0 1 0 4 199
5 2
1 1 3 1 1
5 5
5 6 1 10 2
5 1000000
1000000000000 1000000000000 1000000000000 1000000000000 1000000000000
3 1
0 0 2
"""
) == """YES
YES
YES
NO
NO
YES
NO"""
# minimum size, connectable
assert run(
"""1
2 1
10 0
"""
) == "YES"
# minimum size, not connectable
assert run(
"""1
2 5
0 0
"""
) == "NO"
# all equal values
assert run(
"""1
4 1
5 5 5 5
"""
) == "YES"
# city 1 cannot start growing
assert run(
"""1
3 1
0 0 2
"""
) == "NO"
# large value boundary
assert run(
"""1
2 1000000
1000000000000 1000000000000
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 10 0`| CÓ | Phiên bản hợp lệ tối thiểu | 
|`2 5 / 0 0`| KHÔNG | Trường hợp không hợp lệ tối thiểu | 
| Tất cả các giá trị bằng 5 | CÓ | Đặt hàng tham lam không thành vấn đề | 
|`0 0 2`| KHÔNG | Thành phần chứa thành phố 1 không thể bắt đầu phát triển | 
| Lớn$10^{12}$giá trị | CÓ | Xử lý đúng số tỷ lệ 64 bit | 
## Vỏ cạnh 
Hãy xem xét:```
1
3 1
0 0 2
```Ban đầu$S=0$. Lợi ích duy nhất là$-2$Và$-1$. Sau khi sắp xếp, ứng viên đầu tiên vẫn đáp ứng$S+gain<0$. Thuật toán ngay lập tức trả về KHÔNG. Trên thực tế cũng không có cạnh nào được thêm vào nên câu trả lời là đúng. 
Coi như:```
1
2 1
1 1
```Chúng tôi bắt đầu với$S=1$. Thành phố thứ hai đã đạt được$1-2=-1$. Từ$1+(-1)=0$, việc hợp nhất được cho phép. Thuật toán chấp nhận và xuất ra CÓ. Điều này phát hiện một lỗi thường gặp khi ai đó sử dụng`<=`thay vì`<`. 
Coi như:```
1
4 10
0 20 15 10
```Thành phần ban đầu có dân số bằng không. Một giải pháp bất cẩn có thể bị từ chối ngay lập tức. Thay vào đó, thuật toán sẽ kiểm tra các thành phố theo thứ tự khuếch đại. Thành phố$2$đã đạt được$0$, làm cho việc hợp nhất đầu tiên có thể thực hiện được. Sau đó, thành phần này sẽ phát triển và các thành phố còn lại có thể truy cập được. Câu trả lời đúng là CÓ. 
Coi như:```
1
5 5
5 6 1 10 2
```Thành phố đẹp nhất theo dân số là thành phố$4$, nhưng ban đầu nó không thể gắn được. Sắp xếp theo mức tăng sẽ ưu tiên chính xác các thành phố thực sự có thể được hợp nhất. Quá trình cuối cùng bị kẹt và thuật toán đưa ra KHÔNG, khớp với câu trả lời chính thức.
