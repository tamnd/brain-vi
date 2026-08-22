---
title: "CF 104181I - Chuyến Giao Hàng Trong Mưa"
description: "Chúng ta được cho một đồ thị có hướng trong đó mỗi nút đại diện cho ngôi nhà của một người bạn và mỗi cạnh có hướng đại diện cho một con đường một chiều. Bạn được phép chọn bất kỳ ngôi nhà bắt đầu nào, sau đó liên tục di chuyển dọc theo những con đường được chỉ dẫn, có thể thăm lại những ngôi nhà và con đường nhiều lần."
date: "2026-07-02T00:39:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104181
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 1 (Advanced)"
rating: 0
weight: 104181
solve_time_s: 66
verified: true
draft: false
---

[CF 104181I - Giao hàng trong mưa](https://codeforces.com/problemset/problem/104181/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trong đó mỗi nút đại diện cho ngôi nhà của một người bạn và mỗi cạnh có hướng đại diện cho một con đường một chiều. Bạn được phép chọn bất kỳ ngôi nhà bắt đầu nào, sau đó liên tục di chuyển dọc theo những con đường được chỉ dẫn, có thể thăm lại những ngôi nhà và con đường nhiều lần. 

Mục tiêu là tối đa hóa số lượng ngôi nhà riêng biệt mà bạn có thể ghé thăm dọc theo chuyến đi bộ như vậy. Vì được phép truy cập lại nhưng chỉ tính các ngôi nhà duy nhất, nên vấn đề giảm xuống còn việc tìm nút bắt đầu và bước đi có hướng bao gồm càng nhiều nút riêng biệt có thể tiếp cận càng tốt. 

Một quan sát quan trọng là khi bạn bước vào một chu kỳ được định hướng, bạn có thể ở trong chu kỳ đó và đi qua nó vô thời hạn, điều đó có nghĩa là tất cả các nút trong chu kỳ đó đều có thể truy cập được lẫn nhau. Từ bất kỳ nút nào, tất cả các nút có thể truy cập thông qua các đường dẫn được định hướng thực tế là một phần của cùng một bao đóng có thể truy cập được, nhưng các chu kỳ sẽ nén cấu trúc này. 

Các ràng buộc nhỏ về số lượng nút, lên tới 1000 và các cạnh thưa thớt với tối đa 2N cạnh. Điều này cho thấy rõ ràng rằng cách tiếp cận O(N^2) hoặc O(NM) có thể chấp nhận được, trong khi bất cứ điều gì như liệt kê tất cả các đường dẫn hoặc tập hợp con của các nút thì không. 

Một cách giải thích ngây thơ sẽ là thử mọi nút bắt đầu và thực hiện đếm các nút có thể truy cập DFS/BFS. Điều đó đã đưa ra câu trả lời đúng trong nhiều trường hợp, nhưng nó bỏ sót một điểm tinh tế quan trọng: vì các chu kỳ cho phép xem lại và vì khả năng tiếp cận có tính bắc cầu thông qua các thành phần được kết nối mạnh mẽ nên cấu trúc thực là một DAG của SCC. Câu trả lời phụ thuộc vào khả năng tiếp cận lâu nhất trong biểu đồ ngưng tụ đó, không chỉ tính BFS cục bộ nếu cấu trúc SCC không được xem xét cẩn thận. 

Một lỗi phổ biến là coi biểu đồ như thể khả năng tiếp cận đơn giản từ mỗi nút là độc lập, nhưng trong biểu đồ tuần hoàn, khả năng tiếp cận ngây thơ sẽ được tính gấp đôi hoặc không khai thác được toàn bộ SCC đó hoạt động như các đơn vị đơn lẻ. 

Các trường hợp cạnh bao gồm: 

Một chu kỳ được định hướng đơn lẻ chẳng hạn như 1 → 2 → 3 → 1. Bất kỳ sự bắt đầu nào cũng phải đưa ra câu trả lời 3, không phải 1 hoặc 2. 

Một chuỗi như 1 → 2 → 3. Bắt đầu từ 1 mang lại 3, nhưng bắt đầu từ 3 chỉ mang lại 1. Câu trả lời là 3. 

Một biểu đồ có các đường dẫn phân nhánh và hợp nhất, trong đó nhiều đường dẫn SCC hội tụ, có thể làm cho việc đếm quá hoặc thiếu đường truyền tham lam ngây thơ nếu việc nén SCC bị bỏ qua. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi nút, hãy chạy BFS hoặc DFS và đếm xem có thể truy cập được bao nhiêu nút. Câu trả lời là mức tối đa trên tất cả các nút bắt đầu. Điều này đúng vì mọi bước đi hợp lệ đều nằm trong tập hợp có thể truy cập khi bắt đầu và việc xem lại không làm tăng tập hợp vượt quá khả năng tiếp cận. 

Tuy nhiên, cách tiếp cận này giả định rằng khả năng tiếp cận độc lập với cấu trúc tái sử dụng đường dẫn. Trong các biểu đồ dày đặc hoặc các biểu đồ có nhiều đường dẫn chồng chéo, điều này vẫn hoạt động nhưng trở nên không hiệu quả nếu chúng ta cố gắng tính toán lại nhiều lần khả năng tiếp cận theo những cách đơn giản với trạng thái bổ sung. Trường hợp xấu nhất là O(N(N + M)), tức là khoảng 10^6 thao tác ở đây, vẫn ở ranh giới nhưng có thể chấp nhận được. 

Vấn đề sâu xa hơn là hiểu rằng SCC tạo thành các đơn vị nguyên tử thực sự. Bên trong một thành phần được kết nối mạnh mẽ, tất cả các nút đều có thể truy cập lẫn nhau, do đó, bất kỳ nút nào bên trong đều có thể tiếp cận toàn bộ thành phần. Khi SCC được tạo, biểu đồ sẽ trở thành DAG. Nhiệm vụ giảm xuống còn việc tìm số nút tối đa có thể truy cập từ bất kỳ SCC nào trong DAG này, trở thành đường dẫn dài nhất qua các nút DAG được tính trọng số theo kích thước thành phần. 

Quan sát này làm giảm việc thăm dò dư thừa và mang lại cấu trúc lập trình động rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS/BFS từ mỗi nút | O(N(N+M)) | O(N+M) | Quá chậm trong trường hợp xấu nhất | 
| Ngưng tụ SCC + DP trên DAG | O(N+M) | O(N+M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## 1. Tính toán các thành phần liên thông mạnh

Trước tiên, chúng tôi phân tách biểu đồ thành SCC bằng Kosaraju hoặc Tarjan. Lý do là trong SCC, mọi nút đều có thể truy cập được lẫn nhau, vì vậy chúng hoạt động như một đơn vị duy nhất trong việc thu thập bạn bè. 

## 2. Xây dựng biểu đồ thu gọn 

Chúng tôi nén mỗi SCC vào một nút duy nhất. Với mọi cạnh u → v trong đồ thị gốc, nếu u và v thuộc các SCC khác nhau, chúng ta thêm một cạnh có hướng giữa các thành phần của chúng. Điều này tạo ra DAG vì sự ngưng tụ SCC sẽ loại bỏ các chu kỳ. 

## 3. Gán trọng số cho các thành phần 

Mỗi nút SCC có trọng số bằng số lượng nút gốc bên trong nó. Điều này phản ánh số lượng bạn bè chúng tôi tự động thu thập nếu chúng tôi nhập thành phần đó. 

## 4. Tính toán khả năng tiếp cận DP trên DAG 

Chúng tôi tính toán tổng trọng số tối đa có thể đạt được từ mỗi thành phần. Vì biểu đồ là DAG nên chúng tôi có thể xử lý các nút theo thứ tự tôpô hoặc sử dụng DFS được ghi nhớ. Đối với mỗi thành phần, giá trị của nó là trọng số của chính nó cộng với giá trị tốt nhất trong số tất cả các thành phần lân cận đi ra ngoài. 

##5. Lấy điểm xuất phát tốt nhất 

Chúng tôi thử mọi thành phần làm điểm bắt đầu và lấy giá trị DP tối đa. 

### Tại sao nó hoạt động 

Bất biến chính là việc nén SCC duy trì khả năng tiếp cận theo cách một-một: bất kỳ đường dẫn nào trong biểu đồ gốc đều tương ứng với một đường dẫn trong SCC DAG và bất kỳ bước đi nào bên trong SCC đều không làm tăng tập hợp các thành phần có thể tiếp cận ngoài SCC đó. Do đó, việc tối đa hóa các nút riêng biệt được truy cập tương đương với việc chọn SCC bắt đầu và tối đa hóa tổng trọng lượng trên các nút có thể truy cập trong DAG. Vì các đường dẫn DAG không tạo thành chu kỳ nên DP sẽ tích lũy chính xác số tiền có thể tiếp cận tối ưu mà không cần tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    gr = [[] for _ in range(n)]
    
    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        g[a].append(b)
        gr[b].append(a)

    # Kosaraju: first pass order
    vis = [False] * n
    order = []

    def dfs1(u):
        vis[u] = True
        for v in g[u]:
            if not vis[v]:
                dfs1(v)
        order.append(u)

    for i in range(n):
        if not vis[i]:
            dfs1(i)

    # second pass assign components
    comp = [-1] * n
    comps = []

    def dfs2(u, cid):
        comp[u] = cid
        comps[cid].append(u)
        for v in gr[u]:
            if comp[v] == -1:
                dfs2(v, cid)

    for u in reversed(order):
        if comp[u] == -1:
            comps.append([])
            dfs2(u, len(comps) - 1)

    k = len(comps)

    # build condensed graph
    dag = [set() for _ in range(k)]
    weight = [0] * k

    for cid in range(k):
        weight[cid] = len(comps[cid])
        for u in comps[cid]:
            for v in g[u]:
                if comp[v] != cid:
                    dag[cid].add(comp[v])

    dag = [list(s) for s in dag]

    # DP on DAG
    dp = [-1] * k
    vis_dp = [False] * k

    def dfs(u):
        if vis_dp[u]:
            return dp[u]
        vis_dp[u] = True
        best = 0
        for v in dag[u]:
            best = max(best, dfs(v))
        dp[u] = weight[u] + best
        return dp[u]

    ans = 0
    for i in range(k):
        ans = max(ans, dfs(i))

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng cả biểu đồ gốc và biểu đồ ngược của nó, điều này cần thiết cho thuật toán của Kosaraju. DFS đầu tiên tính toán thứ tự hoàn thiện, đảm bảo rằng các nút được xử lý theo cách tôn trọng cấu trúc thoát của biểu đồ. 

DFS thứ hai gán ID thành phần trên biểu đồ đảo ngược, nhóm các nút có thể truy cập lẫn nhau. Mỗi thành phần được lưu trữ rõ ràng để chúng ta có thể tính toán kích thước và các cạnh đi ra của nó. 

Bước cô đọng sử dụng một tập hợp cho mỗi thành phần để tránh các cạnh trùng lặp, điều này quan trọng vì nhiều cạnh ban đầu có thể thu gọn vào cùng một quá trình chuyển đổi SCC. Sau đó, bước DP sẽ tính toán tổng số tiền có thể tiếp cận tốt nhất từ ​​mỗi SCC bằng cách sử dụng DFS đã được ghi nhớ. 

Một chi tiết tinh tế là độ sâu đệ quy; Python yêu cầu tăng giới hạn đệ quy do độ sâu lên tới 1000 nút và có thể là chuỗi DFS sâu hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
2 3
```Ở đây tất cả các nút tạo thành một chuỗi đơn giản không có chu kỳ. 

| Bước | Nhiệm vụ SCC | Đồ thị cô đặc | Giá trị DP | 
| --- | --- | --- | --- | 
| 1 | {1}, {2}, {3} | 1→2→3 | tính từ dưới lên | 
| 2 | mỗi nút có kích thước 1 | cùng một chuỗi | dp[3]=1 | 
| 3 | | | dp[2]=2 | 
| 4 | | | dp[1]=3 | 

Điều này cho thấy khả năng tiếp cận tích lũy tuyến tính dọc theo chuỗi. 

### Ví dụ 2 

đầu vào:```
3 1
1 2
```| Bước | Nhiệm vụ SCC | Đồ thị cô đặc | Giá trị DP | 
| --- | --- | --- | --- | 
| 1 | {1}, {2}, {3} | 1→2 | dp[2]=1 | 
| 2 | | | dp[1]=2 | 
| 3 | | | nút 3 bị cô lập = 1 | 

Điểm khởi đầu tốt nhất là nút 1 cho 2. 

Những ví dụ này xác nhận rằng DP tổng hợp chính xác các thành phần có thể truy cập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Kosaraju chạy theo thời gian tuyến tính, cấu trúc SCC DAG và DP cũng tuyến tính | 
| Không gian | O(N + M) | danh sách kề, lưu trữ thành phần và mảng DP | 

Các ràng buộc cho phép tối đa khoảng 2000 cạnh, do đó việc xử lý đồ thị tuyến tính dễ dàng đủ nhanh trong vòng 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n)]
    gr = [[] for _ in range(n)]
    for _ in range(m):
        a, b = map(int, sys.stdin.readline().split())
        a -= 1
        b -= 1
        g[a].append(b)
        gr[b].append(a)

    sys.setrecursionlimit(10**7)

    vis = [False] * n
    order = []

    def dfs1(u):
        vis[u] = True
        for v in g[u]:
            if not vis[v]:
                dfs1(v)
        order.append(u)

    for i in range(n):
        if not vis[i]:
            dfs1(i)

    comp = [-1] * n
    comps = []

    def dfs2(u, cid):
        comp[u] = cid
        comps[cid].append(u)
        for v in gr[u]:
            if comp[v] == -1:
                dfs2(v, cid)

    for u in reversed(order):
        if comp[u] == -1:
            comps.append([])
            dfs2(u, len(comps) - 1)

    k = len(comps)
    dag = [set() for _ in range(k)]
    weight = [len(c) for c in comps]

    for cid in range(k):
        for u in comps[cid]:
            for v in g[u]:
                if comp[v] != cid:
                    dag[cid].add(comp[v])

    dag = [list(s) for s in dag]

    from functools import lru_cache

    @lru_cache(None)
    def dfs(u):
        best = 0
        for v in dag[u]:
            best = max(best, dfs(v))
        return weight[u] + best

    ans = 0
    for i in range(k):
        ans = max(ans, dfs(i))
    return str(ans) + "\n"

# provided samples
assert run("3 2\n1 2\n2 3\n") == "3\n", "sample 1"
assert run("3 1\n1 2\n") == "2\n", "sample 2"
assert run("5 5\n3 5\n3 2\n2 3\n4 5\n5 1\n") == "4\n", "sample 3"

# custom cases
assert run("1 0\n") == "1\n", "single node"
assert run("3 3\n1 2\n2 3\n3 1\n") == "3\n", "single cycle"
assert run("4 3\n1 2\n2 3\n3 4\n") == "4\n", "chain"
assert run("4 2\n1 2\n3 4\n") == "2\n", "disconnected components"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | xử lý đồ thị tối thiểu | 
| chu kỳ đơn | 3 | SCC sụp đổ đúng đắn | 
| chuỗi | 4 | tích lũy khả năng tiếp cận tuyến tính | 
| thành phần bị ngắt kết nối | 2 | đồ thị con độc lập được xử lý chính xác | 

## Vỏ cạnh 

Một chu trình kết nối mạnh mẽ như`1 → 2 → 3 → 1`được xử lý bằng cách nhóm tất cả các nút thành một SCC. Trong quá trình ngưng tụ, nút này trở thành một nút có trọng số 3 không có cạnh đi ra và DP ngay lập tức trả về 3 bất kể điểm bắt đầu. 

Một đồ thị tuyến tính thuần túy như`1 → 2 → 3 → 4`tạo ra bốn SCC có kích thước 1 mỗi cái. DAG trở thành một chuỗi và DP tích lũy chính xác từ đầu trở về sau, đảm bảo nút bắt đầu tối đa là nút đầu của chuỗi, mang lại 4. 

Một biểu đồ bị ngắt kết nối như`1 → 2`Và`3 → 4`dẫn đến hai thành phần DAG độc lập. Vì DP được đánh giá trên tất cả các gốc SCC, nên giá trị tối đa sẽ chọn chính xác phân khúc có thể tiếp cận lớn hơn, trong trường hợp này là 2.
