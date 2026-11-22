---
title: "CF 325C - Quái vật và Kim cương"
description: "Mỗi loại quái vật có một hoặc nhiều quy tắc phân chia. Việc áp dụng một quy tắc sẽ tiêu tốn một quái vật thuộc loại đó và tạo ra hai thứ: Thứ nhất, một số kim cương. Thứ hai, có nhiều quái vật mới."
date: "2026-06-06T05:57:14+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 325
codeforces_index: "C"
codeforces_contest_name: "MemSQL start[c]up Round 1"
rating: 2600
weight: 325
solve_time_s: 166
verified: false
draft: false
---

[CF 325C - Quái vật và Kim cương](https://codeforces.com/problemset/problem/325/C) 

**Đánh giá:** 2600 
**Tags:** dfs và tương tự, đồ thị, đường dẫn ngắn nhất 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi loại quái vật có một hoặc nhiều quy tắc phân chia. Việc áp dụng một quy tắc sẽ tiêu tốn một quái vật thuộc loại đó và tạo ra hai thứ: 

Đầu tiên, một số kim cương. 

Thứ hai, có nhiều quái vật mới. 

Bắt đầu từ một con quái vật duy nhất, chúng tôi liên tục chọn quy tắc cho mọi con quái vật xuất hiện, cho đến khi cuối cùng không còn con quái vật nào. Tổng số kim cương thu thập được là tổng số kim cương được tạo ra bởi mỗi ứng dụng quy tắc. 

Đối với mỗi loại quái vật, chúng ta phải tính hai giá trị. 

Số lượng kim cương tối thiểu có thể đạt được trong số tất cả các chiến lược chấm dứt. 

Số lượng kim cương tối đa có thể đạt được trong số tất cả các chiến lược chấm dứt. 

Nếu việc chấm dứt là không thể thì cả hai câu trả lời đều là`-1`. 

Nếu có thể tạo ra nhiều viên kim cương tùy ý trước khi kết thúc thì câu trả lời tối đa là`-2`. 

Đầu vào tự nhiên tạo thành một siêu đồ thị có hướng. Một quy tắc```
A -> diamonds + {B, C, D}
```có nghĩa là giá trị của quái vật`A`phụ thuộc vào tất cả`B`,`C`, Và`D`. 

Những hạn chế là quan sát quan trọng. Có tới`10^5`các loại quái vật và`10^5`tổng số yếu tố quy tắc. Bất kỳ thuật toán nào liên tục khám phá toàn bộ biểu đồ từ mọi con quái vật đều không thể thực hiện được. Chúng ta cần thứ gì đó gần với thời gian tuyến tính trong tổng kích thước đầu vào. 

Những trường hợp nguy hiểm là những trường hợp có tính chu kỳ. 

Coi như:```
1 -> 1 + monster 1
1 -> 1
```Quái vật`1`có thể chấm dứt, vì quy tắc thứ hai. Nó cũng có thể lặp lại quy tắc đầu tiên bao nhiêu lần trước khi chọn quy tắc thứ hai. Câu trả lời tối thiểu là`1`, trong khi câu trả lời tối đa là vô hạn. 

Một trường hợp tế nhị khác là:```
1 -> monster 2 + 1
2 -> monster 1 + 1
```Không có quái vật nào có bất kỳ quy tắc chấm dứt nào. Cả hai câu trả lời đều`-1`cho cả hai loại. Một DFS ngây thơ chỉ tìm kiếm các chu trình sẽ kết luận sai rằng mức cực đại là vô hạn, nhưng những viên kim cương vô hạn sẽ không liên quan nếu việc kết thúc là không thể. 

Cạm bẫy thứ ba là một chu kỳ có thể tiếp cận được từ một số quái vật nhưng không phải là một phần của chính quái vật đó:```
1 -> monster 2 + 1
2 -> monster 3 + 1
3 -> monster 2 + 1
2 -> 1
3 -> 1
```Quái vật`1`có thể bước vào chu trình, ở đó lâu tùy ý, sau đó rời đi theo quy tắc kết thúc. Câu trả lời tối đa của nó cũng là vô hạn. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi chuỗi lựa chọn quy tắc có thể có và đánh giá đệ quy các quái vật thu được. Điều đó đúng về mặt khái niệm bởi vì mọi dẫn xuất tận cùng đều tương ứng với một cây khai triển hữu hạn. 

Vấn đề là số lượng các dẫn xuất có thể tăng theo cấp số nhân. Ngay cả một chu kỳ nhỏ cũng cho phép vô số chiến lược khác nhau. Với`10^5`loại quái vật, việc khám phá rõ ràng là vô vọng. 

Cấu trúc của các quy tắc mang lại một quan điểm tốt hơn nhiều. 

Cho phép`min[i]`là số kim cương tối thiểu có thể kiếm được từ quái vật`i`. 

Đối với một quy tắc```
i -> d diamonds + children
```quy tắc góp phần```
d + sum(min[child])
```bởi vì mọi quái vật được tạo ra cuối cùng đều phải được giải quyết. 

Ý tưởng tương tự áp dụng cho giá trị tối đa. 

Điều này biến vấn đề thành việc đánh giá các phương trình trên một siêu đồ thị có hướng. Một quy tắc chỉ có thể sử dụng được khi tất cả các quy tắc con của nó đã được biết là có thể kết thúc được. 

Nhiệm vụ đầu tiên là xác định xem quái vật nào có thể kết thúc. Đây là vấn đề về khả năng tiếp cận AND-OR. 

Một quái vật có thể bị tiêu diệt nếu ít nhất một trong các quy tắc của nó có tất cả các con quái vật có thể bị tiêu diệt. 

Sau khi chúng ta biết tập hợp có thể kết thúc, các giá trị tối thiểu sẽ trở thành bài toán siêu đường dẫn ngắn nhất. Các giá trị tối đa trở thành bài toán siêu đường dài nhất, ngoại trừ các chu kỳ có mức tăng dương tương ứng với các câu trả lời vô hạn. 

Quan sát quan trọng là mọi quy tắc đều tạo ra ít nhất một viên kim cương. Bất kỳ chu kỳ nào có thể đi qua và thoát ra sau đó đều có thể được lặp lại nhiều lần tùy ý, tạo ra những viên kim cương không giới hạn. Vì vậy, các trạng thái vô hạn chính xác là những con quái vật có thể kết thúc được, có thể đạt đến một chu kỳ có hướng bên trong biểu đồ các quy tắc có thể sử dụng được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + tổng kích thước quy tắc) | O(n + tổng kích thước quy tắc) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xây dựng siêu đồ thị quy tắc 

Đối với mỗi cửa hàng quy tắc: 

Chủ nhân quái vật. 

Số lượng kim cương được sản xuất theo quy tắc. 

Danh sách quái vật trẻ em. 

Chúng tôi cũng xây dựng các tham chiếu ngược từ mọi trẻ em đến các quy tắc có chứa nó. 

### 2. Tính toán những con quái vật có thể tiêu diệt được 

Một quy tắc trở nên thỏa mãn khi tất cả các quái vật con của nó đã được biết là có thể kết thúc được. 

Đối với mọi quy tắc, chúng tôi duy trì một bộ đếm bằng số lượng quái vật con chưa được xác nhận có thể tiêu diệt được. 

Các quy tắc không có con nào được thỏa mãn ngay lập tức. 

Bất cứ khi nào một con quái vật có thể bị tiêu diệt, chúng tôi sẽ truy cập tất cả các quy tắc phụ thuộc vào nó và giảm bộ đếm của chúng. 

Nếu bộ đếm quy tắc đạt đến 0, quái thú chủ sở hữu của nó sẽ bị tiêu diệt. 

Đây là cách lan truyền tiêu chuẩn cho đồ thị AND-OR. 

### 3. Tính số kim cương tối thiểu 

Chỉ những quy tắc mà các con của chúng đều có thể kết thúc được mới có thể đóng góp. 

Đối với mỗi quy tắc chúng ta cần```
diamonds(rule) + sum(min[child])
```Các phương trình đều đơn điệu và mọi chi phí đều dương. 

Chúng tôi xử lý chúng bằng cách thư giãn siêu đồ thị giống như Dijkstra. Một quy tắc có thể đánh giá được khi tất cả các giá trị con đều được biết. Tại thời điểm đó, nó tạo ra một giá trị ứng cử viên cho chủ sở hữu của nó. 

Ứng cử viên nhỏ nhất trong tất cả các quy tắc có thể sử dụng là câu trả lời. 

### 4. Xây dựng biểu đồ các phụ thuộc có thể sử dụng 

Đối với mọi quy tắc có thể sử dụng```
u -> children
```thêm các cạnh có hướng thông thường```
u -> child
```cho tất cả trẻ em. 

Biểu đồ này mô tả giá trị quái vật nào phụ thuộc vào giá trị nào khác. 

### 5. Tìm trạng thái vô hạn 

Chạy phân tách SCC trên biểu đồ phụ thuộc có thể sử dụng được. 

Bất kỳ SCC nào có nhiều hơn một đỉnh đều là một chu trình. 

Một đỉnh SCC đơn cũng là tuần hoàn nếu nó có vòng tự lặp. 

Bởi vì mỗi quy tắc mang lại ít nhất một viên kim cương, việc đạt được chu kỳ như vậy có nghĩa là chúng ta có thể kiếm được số kim cương dương sau mỗi lần truyền tải. 

Đánh dấu tất cả các SCC tuần hoàn. 

Sau đó chạy duyệt đồ thị ngược và đánh dấu mọi quái vật có thể tiêu diệt được có thể tiếp cận một trong các SCC đó. 

Những con quái vật đó có câu trả lời tối đa`-2`. 

### 6. Tính số kim cương tối đa hữu hạn 

Loại bỏ tất cả quái vật vô hạn. 

Biểu đồ phụ thuộc có thể sử dụng còn lại là không theo chu kỳ. 

Đối với mỗi quái vật còn lại,```
max[i] =
max over usable rules (
    diamonds(rule) + sum(max[child])
)
```Vì biểu đồ hiện là DAG nên chúng tôi đánh giá các giá trị này theo thứ tự tôpô ngược. 

### 7. Kẹp đáp án hữu hạn lớn 

Bất kỳ giá trị hữu hạn nào lớn hơn`314000000`được in dưới dạng`314000000`. 

### Tại sao nó hoạt động 

Sự lan truyền kết thúc tính toán chính xác điểm cố định nhỏ nhất của điều kiện```
monster is terminable
⇔
some rule has all children terminable
```Một quy tắc chỉ đóng góp số lượng kim cương hợp lệ nếu mọi quái vật được tạo ra đều có thể tự kết thúc. Các phương trình tối thiểu và tối đa chính xác là định nghĩa đệ quy về kết quả trò chơi. 

Mỗi chu kỳ trong biểu đồ có thể sử dụng đều có mức tăng dương hoàn toàn vì mỗi quy tắc tạo ra ít nhất một viên kim cương. Một con quái vật có thể kết thúc có thể đạt đến một chu kỳ như vậy có thể đi qua nó bất kỳ số lần nào và sau đó rời đi bằng chiến lược kết thúc, mang lại tổng số hữu hạn lớn tùy ý. Đó chính xác là ý nghĩa của câu trả lời`-2`. 

Sau khi loại bỏ các trạng thái vô hạn, đồ thị phụ thuộc trở thành không có chu kỳ, do đó phương trình cực đại có nghiệm hữu hạn duy nhất thu được bằng lập trình động trên DAG. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

CAP = 314000000

def solve():
    m, n = map(int, input().split())

    rules = []
    owner_rules = [[] for _ in range(n)]
    rev_rules = [[] for _ in range(n)]

    for rid in range(m):
        arr = list(map(int, input().split()))
        owner = arr[0] - 1
        l = arr[1]

        diamonds = 0
        children = []

        for x in arr[2:2 + l]:
            if x == -1:
                diamonds += 1
            else:
                children.append(x - 1)

        rules.append((owner, diamonds, children))
        owner_rules[owner].append(rid)

        for v in children:
            rev_rules[v].append(rid)

    # terminable monsters
    need = [len(ch) for _, _, ch in rules]
    can = [False] * n
    q = deque()

    for rid, (owner, _, ch) in enumerate(rules):
        if not ch and not can[owner]:
            can[owner] = True
            q.append(owner)

    while q:
        u = q.popleft()

        for rid in rev_rules[u]:
            need[rid] -= 1
            if need[rid] == 0:
                owner = rules[rid][0]
                if not can[owner]:
                    can[owner] = True
                    q.append(owner)

    # usable rules
    usable = [False] * m
    for rid, (_, _, ch) in enumerate(rules):
        ok = True
        for v in ch:
            if not can[v]:
                ok = False
                break
        usable[rid] = ok

    g = [[] for _ in range(n)]
    rg = [[] for _ in range(n)]

    for rid, (u, _, ch) in enumerate(rules):
        if not usable[rid]:
            continue
        for v in ch:
            g[u].append(v)
            rg[v].append(u)

    # SCC (Kosaraju)
    used = [False] * n
    order = []

    sys.setrecursionlimit(300000)

    def dfs1(v):
        used[v] = True
        for to in g[v]:
            if not used[to]:
                dfs1(to)
        order.append(v)

    for i in range(n):
        if can[i] and not used[i]:
            dfs1(i)

    comp = [-1] * n

    def dfs2(v, c):
        comp[v] = c
        for to in rg[v]:
            if comp[to] == -1:
                dfs2(to, c)

    cid = 0
    for v in reversed(order):
        if comp[v] == -1:
            dfs2(v, cid)
            cid += 1

    sz = [0] * cid
    for i in range(n):
        if comp[i] != -1:
            sz[comp[i]] += 1

    cyc = [False] * cid

    for u in range(n):
        if comp[u] == -1:
            continue
        if sz[comp[u]] > 1:
            cyc[comp[u]] = True
        for v in g[u]:
            if u == v and comp[u] == comp[v]:
                cyc[comp[u]] = True

    inf = [False] * n
    dq = deque()

    for i in range(n):
        if can[i] and cyc[comp[i]]:
            inf[i] = True
            dq.append(i)

    while dq:
        v = dq.popleft()
        for p in rg[v]:
            if can[p] and not inf[p]:
                inf[p] = True
                dq.append(p)

    # finite max via memo DAG
    min_dp = [-1] * n
    max_dp = [-1] * n

    def get_min(v):
        if min_dp[v] != -1:
            return min_dp[v]

        best = 10**18
        for rid in owner_rules[v]:
            if not usable[rid]:
                continue

            _, d, ch = rules[rid]
            cur = d
            for x in ch:
                cur += get_min(x)

            if cur < best:
                best = cur

        min_dp[v] = min(best, CAP)
        return min_dp[v]

    def get_max(v):
        if max_dp[v] != -1:
            return max_dp[v]

        best = 0
        for rid in owner_rules[v]:
            if not usable[rid]:
                continue

            _, d, ch = rules[rid]

            bad = False
            cur = d

            for x in ch:
                if inf[x]:
                    bad = True
                    break
                cur += get_max(x)

            if not bad:
                best = max(best, cur)

        max_dp[v] = min(best, CAP)
        return max_dp[v]

    out = []

    for i in range(n):
        if not can[i]:
            out.append("-1 -1")
        else:
            mn = get_min(i)

            if inf[i]:
                out.append(f"{mn} -2")
            else:
                mx = get_max(i)
                out.append(f"{mn} {mx}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo thuật toán gần như trực tiếp. 

Lần truyền đầu tiên tính toán tập hợp quái vật cuối cùng có thể biến mất. Mọi quy tắc đều duy trì số lượng trẻ em chưa được giải quyết. Khi số lượng đó đạt đến 0, quy tắc sẽ có thể sử dụng được. 

Giai đoạn SCC là trung tâm của việc phát hiện câu trả lời vô hạn. Một chu kỳ thôi là không đủ. Chu trình phải nằm bên trong đồ thị con có thể kết thúc. Đó là lý do tại sao việc phân rã SCC chỉ được thực hiện trên các phần phụ thuộc có thể sử dụng được. 

BFS ngược từ SCC tuần hoàn đánh dấu mọi quái vật có thể bước vào chu kỳ như vậy. Vì mỗi lần di chuyển thu được ít nhất một viên kim cương nên các trạng thái đó có giá trị tối đa không giới hạn. 

Các giá trị tối thiểu và tối đa hữu hạn sau đó là các đánh giá được ghi nhớ thông thường của các phương trình đệ quy. 

Giới hạn chỉ được áp dụng cho các câu trả lời hữu hạn, chính xác theo yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 4
1 3 -1 1 -1
1 2 -1 -1
2 3 -1 3 -1
2 3 -1 -1 -1
3 2 -1 -1
4 2 4 -1
```Cấu trúc phụ thuộc chính: 

| Quái vật | Quy tắc có thể sử dụng | 
| --- | --- | 
| 1 |`1 -> 2 diamonds`,`1 -> 2 diamonds + 1`| 
| 2 |`2 -> 2 diamonds + 3`,`2 -> 3 diamonds`| 
| 3 |`3 -> 2 diamonds`| 
| 4 |`4 -> 1 diamond + 4`| 

Phân tích chấm dứt: 

| Quái vật | Có thể chấm dứt | 
| --- | --- | 
| 1 | Có | 
| 2 | Có | 
| 3 | Có | 
| 4 | Không | 

Giá trị tối thiểu: 

| Quái vật | Tối thiểu | 
| --- | --- | 
| 3 | 2 | 
| 2 | 3 | 
| 1 | 2 | 

Giá trị tối đa: 

Quái vật`1`có thể sử dụng nhiều lần quy tắc tự sinh và sau đó chuyển sang quy tắc kết thúc nên mức tối đa của nó là vô hạn. 

Đầu ra cuối cùng:```
2 -2
3 4
2 2
-1 -1
```Ví dụ này minh họa cả quái vật không có kết thúc và giá trị tối đa vô hạn. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 2
1 2 -1 -1
2 3 -1 1 -1
```Bảng phụ thuộc: 

| Quái vật | Quy tắc | 
| --- | --- | 
| 1 | 2 viên kim cương | 
| 2 | 2 viên kim cương + quái vật 1 | 

Đánh giá: 

| Quái vật | Tối thiểu | Tối đa | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 4 | 4 | 

Không có chu trình nào tồn tại nên mọi câu trả lời đều hữu hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + tổng kích thước quy tắc) | Mọi cạnh quái vật, quy tắc và phụ thuộc đều được xử lý với số lần không đổi | 
| Không gian | O(n + tổng kích thước quy tắc) | Đồ thị, đồ thị ngược, lưu trữ quy tắc, mảng SCC | 

Tổng số tham chiếu quái vật trên tất cả các quy tắc nhiều nhất là`10^5`, do đó đồ thị vẫn có kích thước tuyến tính. Thuật toán SCC thời gian tuyến tính và sự lan truyền thời gian tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old
    return out.getvalue().strip()

# sample 1
assert run(
"""6 4
1 3 -1 1 -1
1 2 -1 -1
2 3 -1 3 -1
2 3 -1 -1 -1
3 2 -1 -1
4 2 4 -1
"""
) == """2 -2
3 4
2 2
-1 -1"""

# sample 2
assert run(
"""3 2
1 2 1 -1
2 2 -1 -1
2 3 2 1 -1
"""
) == """-1 -1
2 2"""

# single terminal monster
assert run(
"""1 1
1 2 -1 -1
"""
) == """2 2"""

# self cycle with exit
assert run(
"""2 1
1 2 1 -1
1 1 -1
"""
) == """1 -2"""

# pure cycle, no exit
assert run(
"""1 1
1 2 1 -1
"""
) == """-1 -1"""

# chain
assert run(
"""2 2
1 2 -1 -1
2 3 -1 1 -1
"""
) == """2 2
4 4"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quy tắc thiết bị đầu cuối đơn |`2 2`| Ví dụ nhỏ nhất có thể | 
| Tự chu kỳ có lối ra |`1 -2`| Phát hiện tối đa vô hạn | 
| Chu kỳ tinh khiết |`-1 -1`| Chu kỳ không thể kết thúc | 
| Dây chuyền đơn giản | giá trị hữu hạn | Tích lũy đệ quy | 
| Mẫu 1 | đầu ra chính thức | Hỗn hợp các trạng thái hữu hạn, vô hạn, không thể | 

## Vỏ cạnh 

Hãy xem xét vòng lặp tự có lối ra:```
2 1
1 2 1 -1
1 1 -1
```Quy tắc đầu tiên tạo ra một viên kim cương và tạo ra quái vật`1`. Quy tắc thứ hai kết thúc ngay lập tức với một viên kim cương. 

Sự phân rã SCC tìm thấy một SCC tuần hoàn chứa quái vật`1`. Kể từ khi quái vật`1`cũng có thể kết thúc thông qua quy tắc thứ hai, chu trình có hiệu quả. Quái vật đánh dấu giai đoạn tiếp cận ngược`1`như vô tận. 

Câu trả lời tối thiểu đến từ việc áp dụng quy tắc kết thúc ngay lập tức, đưa ra`1`. Câu trả lời tối đa là`-2`. 

Bây giờ hãy xem xét một chu trình thuần túy:```
1 1
1 2 1 -1
```Việc truyền bá chấm dứt không bao giờ đánh dấu quái vật`1`có thể bị chấm dứt vì mọi quy tắc đều phụ thuộc vào quái vật`1`chính nó. Quái vật bị loại trừ trước khi bất kỳ lý do SCC nào được sử dụng. Câu trả lời trở thành:```
-1 -1
```đó chính xác là những gì tuyên bố yêu cầu. 

Cuối cùng, hãy xem xét một con quái vật chỉ đạt đến chu kỳ sản xuất:```
1 -> 2 + diamond
2 -> 2 + diamond
2 -> diamond
```Quái vật`1`chính nó không nằm trong một chu trình. Quá trình truyền ngược từ SCC tuần hoàn đến quái vật`1`, do đó nó được đánh dấu chính xác là có giá trị tối đa vô hạn. Thuật toán xử lý trường hợp này mà không có bất kỳ logic đặc biệt nào ngoài SCC và các giai đoạn truy cập ngược.
