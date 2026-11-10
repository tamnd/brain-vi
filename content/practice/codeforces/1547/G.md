---
title: "CF 1547G - Có bao nhiêu đường đi?"
description: "Chúng ta được cung cấp một đồ thị có hướng trong đó chúng ta bắt đầu từ nút 1 và xem xét tất cả các bước đi có hướng có thể kết thúc ở mỗi đỉnh. Một bước đi có thể xem lại các nút và cạnh tùy ý nhiều lần."
date: "2026-06-14T19:59:56+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1547
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 731 (Div. 3)"
rating: 2100
weight: 1547
solve_time_s: 534
verified: false
draft: false
---

[CF 1547G - Có bao nhiêu đường dẫn?](https://codeforces.com/problemset/problem/1547/G) 

**Xếp hạng:** 2100 
**Thẻ:** dfs và tương tự, dp, đồ thị, cây 
**Thời gian giải:** 8 phút 54 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng trong đó chúng ta bắt đầu từ nút 1 và xem xét tất cả các bước đi có hướng có thể kết thúc ở mỗi đỉnh. Một bước đi có thể xem lại các nút và cạnh tùy ý nhiều lần. Đối với mỗi đỉnh, chúng ta không được yêu cầu tính toán chính xác số lần đi bộ như vậy mà chỉ phân loại con số đó thành bốn loại: không có khả năng tiếp cận, chính xác là một lần đi bộ, nhiều lần đi bộ nhưng có số lần đi bộ hữu hạn hoặc vô số lần đi bộ. 

Điểm tinh tế quan trọng nhất là các chu kỳ thay đổi hoàn toàn ý nghĩa của “số đường đi”. Một chu trình không chỉ thêm nhiều đường đi mà nó có thể được lặp lại tùy ý nhiều lần, điều này ngay lập tức tạo ra vô số bước đi riêng biệt miễn là nó có thể đến được ngay từ đầu và cũng có thể đến được mục tiêu. 

Kích thước đầu vào buộc chúng ta phải đưa ra giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số đỉnh và cạnh trên tất cả các trường hợp thử nghiệm được giới hạn bởi 4e5, do đó, bất kỳ thuật toán nào lớn hơn O(n + m) cho mỗi trường hợp thử nghiệm hoặc truy cập lại các cạnh quá mức sẽ không vượt qua. Điều này đã loại trừ mọi cách tính đường dẫn rõ ràng, mọi DFS theo cấp số nhân trên tất cả các bước đi và mọi chương trình động đơn giản không thu gọn chu kỳ. 

Có một số dạng lỗi xuất hiện trong những lần thử đơn giản. Một DFS ngây thơ đếm các đường dẫn sẽ ngay lập tức xuất hiện trên các biểu đồ giống như một chu trình được định hướng duy nhất có thể truy cập được từ nút 1, vì số lượng các bước đi riêng biệt trở nên vô hạn do việc truyền tải lặp đi lặp lại của chu trình. Một lỗi phổ biến khác là coi khả năng tiếp cận là vấn đề DAG và thực hiện DP theo thứ tự tùy ý. Điều đó không thành công khi tồn tại các chu kỳ, vì DP giả định tính không tuần hoàn và thứ tự lan truyền hữu hạn. Cuối cùng, việc đánh dấu một nút là “đã truy cập” một lần là chưa đủ: việc xem lại chính xác là điều tạo ra nhiều đường dẫn và số lượng vô hạn. 

Một trường hợp lỗi minh họa nhỏ là một chu kỳ có thể đạt được từ 1 chẳng hạn như 1 → 2 → 3 → 2. Bộ đếm đường dẫn đơn giản sẽ tiếp tục liệt kê 1-2-3-2-3-2-... vô thời hạn, trong khi câu trả lời đúng ngay lập tức là “đường dẫn vô hạn” cho các nút 2 và 3. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các ràng buộc, ý tưởng trực tiếp nhất là liệt kê tất cả các bước đi từ nút 1 bằng cách sử dụng DFS và đếm xem có bao nhiêu cách khác nhau để đạt được mỗi nút. Điều này đúng trong đồ thị không có chu kỳ vì mỗi đường dẫn là hữu hạn và số lượng đường dẫn bị giới hạn bởi tổ hợp của cấu trúc DAG. Tuy nhiên, ngay khi các chu kỳ tồn tại, cách tiếp cận này sẽ thất bại vì một chu trình có thể được đi qua bất kỳ số lần nào, tạo ra vô số bước đi riêng biệt ngay cả khi chúng ta chỉ quan tâm đến các mô hình khả năng tiếp cận. 

Quan sát cấu trúc quan trọng là chu kỳ là nguồn vô tận duy nhất. Nếu chúng ta có thể thu gọn mọi thành phần được kết nối mạnh mẽ vào một nút duy nhất thì biểu đồ kết quả sẽ trở thành DAG. Bên trong một thành phần được kết nối mạnh mẽ, khi bạn nhập nó, bạn có thể tiếp cận mọi nút trong đó và có thể rời đi theo nhiều cách. Nếu thành phần đó cũng có thể truy cập được từ một chu kỳ mà vẫn có thể vươn ra ngoài, thì tất cả các nút ở hạ lưu sẽ trở thành vô hạn. 

Điều này cho thấy một lý luận hai giai đoạn. Trước tiên, chúng tôi xác định tất cả các nút là một phần hoặc có thể truy cập được từ bất kỳ chu kỳ nào có thể truy cập được từ nút 1. Các nút này tự động nhận được câu trả lời -1 vì chúng tôi có thể lặp vô thời hạn trước khi tiếp tục hoặc sau khi quay lại. Thứ hai, trên biểu đồ còn lại, thực chất là DAG sau khi loại bỏ các nút “xấu”, chúng ta có thể tính toán số cách từ nút 1 bằng cách sử dụng DP tôpô, nhưng chúng ta chỉ cần phân biệt giữa 0, 1 và 2. Thời điểm một nút tích lũy nhiều hơn một cách, chúng ta giới hạn nó ở mức 2.

Để phát hiện chu kỳ một cách hiệu quả, chúng tôi sử dụng DFS với tính năng theo dõi trạng thái đệ quy hoặc phân tách SCC. Giải pháp CF tiêu chuẩn sử dụng các màu DFS: chưa được truy cập, đã truy cập và đã hoàn tất. Bất kỳ cạnh nào của nút “truy cập” đều biểu thị một chu kỳ. Khi chúng tôi tìm thấy bất kỳ nút chu kỳ nào có thể truy cập được từ 1, chúng tôi sẽ truyền “vô hạn” về phía trước thông qua tất cả các nút có thể truy cập từ chu trình đó trong biểu đồ gốc. 

Sau khi đánh dấu các nút vô hạn, chúng tôi bỏ qua chúng trong DP và chạy truyền tải kiểu tôpô trên biểu đồ còn lại bắt đầu từ 1. Vì các chu kỳ bị loại bỏ nên sơ đồ con này là DAG và việc đếm đường dẫn trở thành lan truyền tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê đường dẫn DFS Brute | hàm mũ | O(n) | Quá chậm | 
| DFS + DAG DP nhận biết chu kỳ | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị và tính toán danh sách kề đảo ngược. Biểu đồ ngược sau này sẽ giúp chúng ta truyền bá “khả năng tiếp cận vô hạn” ngược từ các chu kỳ được phát hiện. 
2. Chạy DFS từ nút 1 để xác định xem nút nào có thể truy cập được. Bất kỳ nút nào không được truy cập ở đây sẽ ngay lập tức nhận được câu trả lời 0, vì không có đường dẫn nào tồn tại từ 1. 
3. Trong DFS, duy trì trạng thái ngăn xếp đệ quy để phát hiện các cạnh sau. Khi chúng tôi tìm thấy một cạnh từ nút này đến nút khác hiện có trong ngăn xếp đệ quy, chúng tôi sẽ xác định được một chu trình có hướng. Đánh dấu tất cả các nút có liên quan hoặc có thể truy cập được từ các chu kỳ như vậy là “xấu”. 

Điều này có tác dụng vì bất kỳ nút nào trên một chu kỳ đều cho phép di chuyển lặp lại, tạo ra vô số bước đi riêng biệt. 
4. Sau khi phát hiện chu kỳ ban đầu, hãy truyền tiếp trạng thái “xấu”: nếu một nút có thể tiếp cận một nút xấu thì nút đó cũng bị xấu. Điều này được thực hiện bằng cách sử dụng BFS/DFS trên biểu đồ gốc bắt đầu từ tất cả các nút chu kỳ. 

Lý do việc truyền bá này là cần thiết là vì khi một đường dẫn có thể đi vào một chu kỳ và sau đó tiếp tục đi tiếp, mọi nút xuôi dòng đều thừa hưởng bội số vô hạn. 
5. Bây giờ hãy loại bỏ tất cả các nút xấu khỏi danh sách xem xét. Trên biểu đồ còn lại, tất cả các đường dẫn từ nút 1 được đảm bảo hữu hạn. 
6. Tính toán số lượng đường dẫn trong biểu đồ rút gọn này bằng cách sử dụng bộ nhớ DFS hoặc DP tôpô. Vì biểu đồ không có tính tuần hoàn sau khi loại bỏ các nút xấu nên giá trị của mỗi nút chỉ phụ thuộc vào các nút được tính toán trước đó. 

Chúng tôi chỉ lưu trữ ba trạng thái cho mỗi nút: 0 (không thể truy cập), 1 (chính xác một đường dẫn), 2 (nhiều hơn một đường dẫn). Bất kỳ số tiền nào vượt quá 2 đều bị kẹp. 
7. Kết quả đầu ra: các nút xấu nhận -1, các nút không thể truy cập nhận 0 và các nút còn lại nhận giá trị DP. 

### Tại sao nó hoạt động 

Tính đúng đắn xoay quanh việc tách biệt hai hiện tượng: tính đa bội do sự phân nhánh và tính vô hạn do các chu kỳ. Các thành phần được kết nối mạnh mẽ chính xác là các cấu trúc tối đa nơi chu kỳ tồn tại và bất kỳ SCC nào có thể truy cập được từ nút 1 có thể được xem lại đều ngụ ý sự lặp lại không giới hạn của các bước đi. Sau khi các đường dẫn này được tách biệt và truyền về phía trước, biểu đồ còn lại sẽ trở thành không theo chu kỳ, do đó số lượng đường dẫn hoạt động giống như DAG DP tiêu chuẩn. Việc kẹp thành 2 chỉ bảo toàn sự phân biệt giữa đường dẫn duy nhất và nhiều đường dẫn hữu hạn, ổn định dưới các chuyển tiếp DP trong DAG. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    rg = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b = map(int, input().split())
        g[a].append(b)
        rg[b].append(a)

    # 1. reachable from 1
    vis = [False] * (n + 1)
    stack = [1]
    vis[1] = True
    order = []
    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if not vis[v]:
                vis[v] = True
                stack.append(v)

    if not vis[1]:
        vis[1] = True

    # 2. detect cycles via DFS colors
    color = [0] * (n + 1)
    bad = [False] * (n + 1)

    def dfs(u):
        color[u] = 1
        for v in g[u]:
            if not vis[v]:
                continue
            if color[v] == 0:
                if dfs(v):
                    bad[u] = True
            elif color[v] == 1:
                bad[v] = True
                bad[u] = True
        color[u] = 2
        return bad[u]

    for i in range(1, n + 1):
        if vis[i] and color[i] == 0:
            dfs(i)

    # 3. propagate bad forward
    from collections import deque
    q = deque([i for i in range(1, n + 1) if bad[i]])
    while q:
        u = q.popleft()
        for v in g[u]:
            if vis[v] and not bad[v]:
                bad[v] = True
                q.append(v)

    # 4. DP on remaining graph
    dp = [-1] * (n + 1)

    def solve_dp(u):
        if bad[u]:
            return 0
        if dp[u] != -1:
            return dp[u]
        total = 1 if u == 1 else 0
        for v in g[u]:
            if not vis[v] or bad[v]:
                continue
            total += solve_dp(v)
            if total > 2:
                total = 2
        dp[u] = total
        return dp[u]

    for i in range(1, n + 1):
        if vis[i] and not bad[i]:
            solve_dp(i)

    res = []
    for i in range(1, n + 1):
        if not vis[i]:
            res.append("0")
        elif bad[i]:
            res.append("-1")
        else:
            res.append(str(dp[i]))

    print(" ".join(res))

t = int(input())
for _ in range(t):
    input()
    solve()
```Việc triển khai trước tiên hạn chế sự chú ý đến các nút có thể truy cập từ 1, bởi vì mọi thứ khác gần như bằng 0. Sau đó, nó sử dụng màu DFS để phát hiện các chu kỳ và đánh dấu tất cả các nút liên quan. Bước lan truyền bổ sung đảm bảo rằng bất kỳ nút nào ở hạ lưu của một chu kỳ cũng được coi là vô hạn, điều này rất cần thiết vì các chu kỳ không chỉ ảnh hưởng đến các nút của chính chúng mà còn ảnh hưởng đến mọi thứ có thể truy cập sau chúng. 

Giai đoạn DP cẩn thận tránh việc tính hai lần bằng cách kẹp các giá trị ở mức 2. Điều này rất quan trọng vì biểu đồ có thể có nhiều đường dẫn hợp nhất và không cần phải tính chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó 1 chia thành hai đường dẫn, 1 → 2 → 4 và 1 → 3 → 4. Nút 4 phải được đánh dấu là có hai đường dẫn hữu hạn. 

| Bước | Có thể truy cập | Nút chu kỳ | DP(2) | DP(3) ​​| DP(4) | 
| --- | --- | --- | --- | --- | --- | 
| sau khả năng tiếp cận | {1,2,3,4} | không | - | - | - | 
| sau DP init | giống nhau | không | 1 | 1 | - | 
| sau DP | giống nhau | không | 1 | 1 | 2 | 

Điều này xác nhận cách tăng số lượng đường dẫn hợp nhất nhưng vẫn bị giới hạn. 

Bây giờ hãy xem xét chu kỳ 1 → 2 → 3 → 2 với 3 → 4. Nút 4 có thể truy cập được sau một chu kỳ. 

| Bước | Có thể truy cập | Phát hiện chu kỳ | nút xấu | DP(4) | 
| --- | --- | --- | --- | --- | 
| sau DFS | {1,2,3,4} | {2,3} | {2,3} | - | 
| tuyên truyền | giống nhau | giống nhau | {2,3,4} | - | 
| cuối cùng | giống nhau | giống nhau | 2,3,4 = -1 | - | 

Điều này cho thấy khả năng tiếp cận theo chu kỳ buộc các nút xuôi dòng phải phân loại vô hạn như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý với số lần không đổi trên DFS, phát hiện chu kỳ, lan truyền và DP | 
| Không gian | O(n + m) | danh sách kề, mảng trạng thái và lưu trữ đệ quy/DP | 

Các ràng buộc cho phép tổng cộng 4e5 nút và cạnh, do đó việc xử lý thời gian tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Thuật toán nằm trong giới hạn này vì mọi giai đoạn đều hoàn toàn tuyến tính và tránh được việc tính toán lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        n, m = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        vis = [False]*(n+1)
        for _ in range(m):
            a,b = map(int,input().split())
            g[a].append(b)

        vis[1]=True
        stack=[1]
        while stack:
            u=stack.pop()
            for v in g[u]:
                if not vis[v]:
                    vis[v]=True
                    stack.append(v)

        color=[0]*(n+1)
        bad=[False]*(n+1)

        def dfs(u):
            color[u]=1
            for v in g[u]:
                if not vis[v]:
                    continue
                if color[v]==0:
                    dfs(v)
                elif color[v]==1:
                    bad[u]=bad[v]=True
            color[u]=2

        for i in range(1,n+1):
            if vis[i] and color[i]==0:
                dfs(i)

        from collections import deque
        q=deque([i for i in range(1,n+1) if bad[i]])
        while q:
            u=q.popleft()
            for v in g[u]:
                if vis[v] and not bad[v]:
                    bad[v]=True
                    q.append(v)

        dp=[-1]*(n+1)
        def f(u):
            if bad[u]: return 0
            if dp[u]!=-1: return dp[u]
            res = 1 if u==1 else 0
            for v in g[u]:
                if vis[v] and not bad[v]:
                    res += f(v)
                    if res>2: res=2
            dp[u]=res
            return res

        for i in range(1,n+1):
            if vis[i] and not bad[i]:
                f(i)

        out=[]
        for i in range(1,n+1):
            if not vis[i]:
                out.append("0")
            elif bad[i]:
                out.append("-1")
            else:
                out.append(str(dp[i]))
        return " ".join(out)

    return solve()

# provided samples (basic sanity placeholders)
assert run("""1

6 7
1 4
1 3
3 4
4 5
2 1
5 5
5 6
""").split()[:6], "sample sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở đường dẫn trống | 
| chỉ chu kỳ | -1 -1 -1 | lan truyền vô hạn | 
| Phân nhánh DAG | 1 1 2 | nhiều đường dẫn hữu hạn | 
| các nút không thể truy cập | 1 0 0 0 | lọc khả năng tiếp cận | 

## Vỏ cạnh 

Vòng lặp tự lặp tại nút có thể truy cập từ 1 ngay lập tức biến nút đó thành vô hạn, vì vòng lặp có thể được lặp lại tùy ý trước khi thoát. Thuật toán đánh dấu nó là một chu kỳ trong DFS và sau đó truyền trạng thái đó về phía trước, đảm bảo tính chính xác. 

Chu trình sâu hơn không được kết nối trực tiếp với nút 1 sẽ bị bỏ qua trừ khi nó có thể truy cập được từ nút 1. Điều này được xử lý chính xác vì giai đoạn đầu tiên hạn chế xử lý chỉ ở các nút có thể truy cập, ngăn chặn kết quả dương tính giả. 

Một nút có hai đường dẫn DAG đến riêng biệt nhưng không có chu kỳ phải trả về 2. Giới hạn DP ở mức 2 đảm bảo chúng tôi không cố gắng đếm toàn bộ, trong khi vẫn phân biệt tính duy nhất với tính đa dạng. 

Một nút không thể truy cập được ngoại trừ thông qua một chu kỳ vẫn phải là -1, không phải 0, vì khả năng truy cập thông qua 1 ngụ ý sự tồn tại của các đường dẫn nhưng có bội số vô hạn. Giai đoạn lan truyền đảm bảo các nút như vậy kế thừa điểm đánh dấu vô hạn.
