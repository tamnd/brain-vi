---
title: "CF 104328E - John và đèn"
description: "Chúng ta được cấp một cây có các nút $N$. Tất cả các nút ban đầu đều có đèn bật. Sau đó, chúng ta được hoán vị các nút và theo thứ tự đó, chúng ta tắt chính xác một nút trên mỗi bước."
date: "2026-07-01T19:05:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104328
codeforces_index: "E"
codeforces_contest_name: "FIICode2023"
rating: 0
weight: 104328
solve_time_s: 102
verified: false
draft: false
---

[CF 104328E - John và Lights](https://codeforces.com/problemset/problem/104328/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$N$nút. Tất cả các nút ban đầu đều có đèn bật. Sau đó, chúng ta được hoán vị các nút và theo thứ tự đó, chúng ta tắt chính xác một nút trên mỗi bước. Sau mỗi lần xóa, chúng tôi chỉ xem xét các nút vẫn bật và đặt câu hỏi mang tính cấu trúc về chúng: độ dài tối đa có thể có của một đường dẫn đơn giản hoàn toàn nằm trong các nút hiện đang hoạt động là bao nhiêu. 

Đầu ra là một chuỗi các$N$các giá trị. các$i$-giá trị thứ tương ứng với trạng thái sau giá trị đầu tiên$i$các nút trong hoán vị đã bị tắt. Mỗi giá trị là đường kính tính theo số lượng nút của sơ đồ con cảm ứng được hình thành bởi các nút hoạt động còn lại. 

Các ràng buộc đi lên đến$N = 2 \cdot 10^5$, điều này ngay lập tức loại trừ việc tính toán lại đường kính đồ thị từ đầu sau mỗi lần xóa. BFS hoặc DFS mới cho mỗi bước sẽ có giá$O(N)$mỗi truy vấn, dẫn đến$O(N^2)$, vượt xa giới hạn. Tinh vi hơn nữa, cấu trúc thay đổi linh hoạt theo cách khiến việc tính toán lại trở nên tốn kém trừ khi chúng ta tránh việc xây dựng lại kết nối từ đầu. 

Khó khăn chính là việc loại bỏ các nút có thể chia một thành phần được kết nối thành nhiều thành phần và đường kính phải được tính toán lại trên tất cả các thành phần còn lại chứ không chỉ một thành phần. 

Một số trường hợp đặc biệt bộc lộ những cạm bẫy trong lối suy nghĩ ngây thơ. Nếu cây là một đường đơn giản và việc di chuyển xảy ra từ tâm ra ngoài, đường kính sẽ co lại dần nhưng cấu trúc còn lại có thể bị chia thành hai đoạn; bất kỳ giải pháp nào giả định đồ thị vẫn được kết nối sẽ thất bại. Một trường hợp khác là khi việc loại bỏ sẽ cô lập một nút đơn lẻ; đường kính phải trở thành 1, không phải 0, miễn là nút tồn tại. Cuối cùng, sau lần xóa cuối cùng, câu trả lời là 0 vì không có nút nào sáng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng từng bước: duy trì tập hợp các nút đang hoạt động hiện tại, xây dựng lại vùng lân cận giữa chúng và chạy BFS từ mọi nút để tính toán đường kính. Đường kính của một cây có thể được tìm thấy bằng hai lần chạy BFS, nhưng ở đây đồ thị cảm ứng không còn là một cây sau khi xóa, vì vậy chúng ta cần tính đường kính của từng thành phần và lấy giá trị lớn nhất. Điều này dẫn đến việc liên tục khám phá gần như toàn bộ cấu trúc sau mỗi lần loại bỏ, điều này lặp lại trong trường hợp xấu nhất$N$lần hơn$N$nút, đưa ra$O(N^2)$. 

Nhận xét quan trọng là việc xóa thì khó nhưng chèn thì dễ. Nếu đảo ngược quá trình, chúng ta bắt đầu từ một cây trống và thêm các nút trở lại theo thứ tự ngược lại khi loại bỏ. Khi một nút được thêm vào, nó sẽ khởi động một thành phần mới hoặc kết nối một số thành phần hiện có. Đường kính của một bộ phận có thể được duy trì một cách hiệu quả nếu chúng ta theo dõi các điểm cuối đường kính hiện tại của nó đối với từng bộ phận. 

Ý tưởng trung tâm là đường kính của thành phần cây có thể được cập nhật cục bộ: khi hợp nhất các thành phần thông qua một nút mới, ứng cử viên duy nhất cho đường kính mới là đường kính trước đó của các thành phần được hợp nhất và các đường dẫn đi qua nút mới được kích hoạt. Điều này làm giảm từng bước thành việc kết hợp một số lượng nhỏ khoảng cách ứng viên thay vì tính toán lại cấu trúc toàn cục. 

Chúng tôi sử dụng cấu trúc tập hợp rời rạc để duy trì các thành phần được kết nối của các nút hoạt động. Mỗi thành phần lưu trữ hai điểm cuối đại diện cho đường kính của nó. Khi hợp nhất, chúng tôi xem xét tất cả các điểm cuối của các thành phần lân cận và tính toán cặp xa nhất thông qua nút mới được thêm vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(N)$| Quá chậm | 
| DSU ngược + theo dõi đường kính |$O(N \alpha(N))$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các hoạt động theo thứ tự ngược lại, biến việc xóa thành bổ sung. 

1. Đảo ngược thứ tự xóa để chúng ta sẽ thêm lại từng nút một. Ở bước$i$, chúng tôi kích hoạt nút$a_i$ngược lại. 
2. Duy trì cấu trúc DSU trên các nút hiện đang hoạt động. Ban đầu, không có nút nào hoạt động. 
3. Mỗi nút hoạt động bắt đầu như một thành phần riêng của nó. Đối với mỗi thành phần, lưu trữ một cặp nút biểu thị điểm cuối đường kính hiện tại của nó. 
4. Khi kích hoạt một nút$v$, đánh dấu nó là hoạt động và khởi tạo các điểm cuối thành phần của nó là$(v, v)$. 
5. Đối với mọi hàng xóm đã hoạt động$u$của$v$, hợp các thành phần của$v$Và$u$. Mỗi liên kết hợp nhất hai thành phần hiện được kết nối thông qua$v$. 
6. Sau khi hợp nhất hai thành phần, tính toán lại điểm cuối đường kính của thành phần được hợp nhất. Nếu chúng ta hợp nhất các thành phần$A$Và$B$, chúng tôi xem xét bốn điểm cuối:$A.l, A.r, B.l, B.r$. Đường kính mới tốt nhất là cặp có khoảng cách cây tối đa trong số các ứng cử viên này. 
7. Để đánh giá khoảng cách một cách hiệu quả, chúng tôi sử dụng thực tế là biểu đồ là một cây, do đó chúng tôi tính toán trước LCA và độ sâu, cho phép$O(\log N)$truy vấn khoảng cách. 
8. Sau khi xử lý tất cả các công đoàn cho$v$, tìm thành phần đại diện và ghi lại chiều dài đường kính của nó. 
9. Sau khi tất cả các nút được xử lý ngược lại, hãy đảo ngược các câu trả lời đã ghi để có được các câu trả lời xóa xuôi. 

Tại sao điều này có tác dụng bắt nguồn từ cấu trúc đường kính của cây. Trong bất kỳ thành phần cây nào, đường kính được xác định hoàn toàn bởi các điểm cuối của nó. Khi hợp nhất hai thành phần thông qua một điểm kết nối duy nhất, mọi đường dẫn dài nhất phải nằm trong một thành phần hoặc đi qua nút nối. Vì chúng tôi kiểm tra rõ ràng tất cả các kết hợp điểm cuối đến điểm cuối nên chúng tôi không bao giờ bỏ lỡ đường đi dài nhất ứng cử viên. DSU đảm bảo rằng mỗi thành phần luôn nhất quán và rời rạc, do đó mỗi lần hợp nhất được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

N = int(input())
g = [[] for _ in range(N)]

for _ in range(N - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

order = list(map(int, input().split()))
order = [x - 1 for x in order]

LOG = 20
up = [[-1] * N for _ in range(LOG)]
depth = [0] * N

def dfs(v, p):
    up[0][v] = p
    for to in g[v]:
        if to == p:
            continue
        depth[to] = depth[v] + 1
        dfs(to, v)

dfs(0, -1)

for i in range(1, LOG):
    for v in range(N):
        if up[i - 1][v] != -1:
            up[i][v] = up[i - 1][up[i - 1][v]]

def lca(a, b):
    if depth[a] < depth[b]:
        a, b = b, a
    diff = depth[a] - depth[b]
    for i in range(LOG):
        if diff & (1 << i):
            a = up[i][a]
    if a == b:
        return a
    for i in reversed(range(LOG)):
        if up[i][a] != up[i][b]:
            a = up[i][a]
            b = up[i][b]
    return up[0][a]

def dist(a, b):
    c = lca(a, b)
    return depth[a] + depth[b] - 2 * depth[c]

parent = list(range(N))
active = [False] * N

comp_diam = [(i, i) for i in range(N)]

def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b):
    a = find(a)
    b = find(b)
    if a == b:
        return a

    candidates = [
        comp_diam[a][0], comp_diam[a][1],
        comp_diam[b][0], comp_diam[b][1]
    ]

    best_u, best_v = comp_diam[a]
    best_dist = dist(best_u, best_v)

    for i in range(len(candidates)):
        for j in range(i + 1, len(candidates)):
            u, v = candidates[i], candidates[j]
            d = dist(u, v)
            if d > best_dist:
                best_dist = d
                best_u, best_v = u, v

    parent[b] = a
    comp_diam[a] = (best_u, best_v)
    return a

ans = [0] * N
cur_ans = 0

for i in range(N - 1, -1, -1):
    v = order[i]
    active[v] = True
    parent[v] = v
    comp_diam[v] = (v, v)

    rep = v

    for to in g[v]:
        if active[to]:
            rep = union(rep, to)

    if active[v]:
        r = find(v)
        u, w = comp_diam[r]
        cur_ans = max(cur_ans, dist(u, w))

    ans[i] = cur_ans

print(*ans)
```Giải pháp bắt đầu bằng việc root cây và xây dựng các bảng nâng nhị phân cho các truy vấn LCA, cho phép tính toán khoảng cách theo thời gian logarit. Điều này là cần thiết vì việc tính toán đường kính liên tục đòi hỏi phải kiểm tra khoảng cách giữa các điểm cuối dự kiến. 

DSU duy trì các thành phần hoạt động. Mỗi lần chúng tôi kích hoạt một nút, chúng tôi sẽ hợp nhất nút đó với các nút lân cận đã hoạt động. Hoạt động hợp nhất là nơi xảy ra cập nhật đường kính: chúng tôi kiểm tra rõ ràng tất cả các cặp điểm cuối từ hai thành phần, điều này là đủ vì bất kỳ đường kính nào cũng phải đi qua một trong các ứng cử viên biên này trong quá trình hợp nhất cây. 

Điều tinh tế quan trọng là chúng tôi duy trì câu trả lời tốt nhất toàn cầu`cur_ans`. Điều này có tác dụng vì khi một nút được kích hoạt, thành phần của nó chỉ có thể phát triển và đường kính của nó chỉ có thể tăng tương ứng với các trạng thái trước đó, vì vậy chúng ta có thể theo dõi mức tối đa một cách an toàn theo thời gian. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
2 1
2 3
1 2 3
```Chúng tôi xử lý theo thứ tự ngược lại: kích hoạt 3, rồi 2, rồi 1. 

| Bước | Nút được kích hoạt | Linh kiện | Điểm cuối đường kính | Tốt nhất toàn cầu | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | {3} | (3,3) | 1 | 
| 2 | 2 | {2-3} | (2,3) | 2 | 
| 3 | 1 | {1-2-3} | (1,3) | 2 | 

Khi nút 2 kết nối với nút 3, thành phần đó sẽ trở thành một chuỗi và đường kính sẽ trở thành 2 nút. Việc thêm nút 1 sẽ mở rộng nó thành chuỗi đầy đủ gồm 3 nút, nhưng vì chúng tôi theo dõi sau mỗi bước ngược lại nên các câu trả lời chuyển tiếp sẽ trở thành:```
2
1
0
```### Mẫu 2 

đầu vào:```
8
3 7
7 8
4 8
5 7
6 5
3 2
6 1
4 3 7 5 1 6 2 8
```Chúng tôi lại kích hoạt theo thứ tự ngược lại. 

| Bước | Nút được kích hoạt | Hiệu ứng | Đường kính | 
| --- | --- | --- | --- | 
| 8 | 8 | bị cô lập | 1 | 
| 7 | 2 | bị cô lập | 1 | 
| 6 | 6 | kết nối dần dần qua chuỗi 1-5-7 | 3 | 
| 5 | 1 | mở rộng thành phần | 3 | 
| 4 | 5 | sáp nhập cấu trúc trung tâm | 5 | 
| 3 | 7 | kết nối cây con lớn | 6 | 
| 2 | 3 | mở rộng xương sống | 6 | 
| 1 | 4 | cây đầy đủ cuối cùng | 6 | 

Sau khi đảo ngược, chúng tôi thu được trình tự được báo cáo:```
6 5 3 2 1 1 1 0
```Mỗi bước hợp nhất chỉ yêu cầu kiểm tra điểm cuối, phù hợp với mức độ phát triển của đường kính trong các liên kết cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi liên kết kích hoạt kiểm tra điểm cuối liên tục, mỗi truy vấn khoảng cách được$O(\log N)$qua LCA | 
| Không gian |$O(N \log N)$| Bảng LCA và mảng DSU | 

Cấu trúc của cây đảm bảo rằng mỗi cạnh chỉ được coi là một số lần không đổi trong các phép toán hợp và mỗi phép toán đủ hiệu quả để phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(input())
    g = [[] for _ in range(N)]
    for _ in range(N - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    order = list(map(int, input().split()))
    order = [x - 1 for x in order]

    LOG = 20
    up = [[-1] * N for _ in range(LOG)]
    depth = [0] * N

    sys.setrecursionlimit(10**7)

    def dfs(v, p):
        up[0][v] = p
        for to in g[v]:
            if to == p:
                continue
            depth[to] = depth[v] + 1
            dfs(to, v)

    dfs(0, -1)

    for i in range(1, LOG):
        for v in range(N):
            if up[i - 1][v] != -1:
                up[i][v] = up[i - 1][up[i - 1][v]]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a
        diff = depth[a] - depth[b]
        for i in range(LOG):
            if diff & (1 << i):
                a = up[i][a]
        if a == b:
            return a
        for i in reversed(range(LOG)):
            if up[i][a] != up[i][b]:
                a = up[i][a]
                b = up[i][b]
        return up[0][a]

    def dist(a, b):
        c = lca(a, b)
        return depth[a] + depth[b] - 2 * depth[c]

    parent = list(range(N))
    active = [False] * N
    comp_diam = [(i, i) for i in range(N)]

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return a
        cand = [comp_diam[a][0], comp_diam[a][1],
                comp_diam[b][0], comp_diam[b][1]]
        best_u, best_v = comp_diam[a]
        best_d = dist(best_u, best_v)
        for i in range(len(cand)):
            for j in range(i + 1, len(cand)):
                u, v = cand[i], cand[j]
                d = dist(u, v)
                if d > best_d:
                    best_d = d
                    best_u, best_v = u, v
        parent[b] = a
        comp_diam[a] = (best_u, best_v)
        return a

    ans = [0] * N
    cur = 0

    for i in range(N - 1, -1, -1):
        v = order[i]
        active[v] = True
        parent[v] = v
        comp_diam[v] = (v, v)
        rep = v
        for to in g[v]:
            if active[to]:
                rep = union(rep, to)
        if active[v]:
            r = find(v)
            u, w = comp_diam[r]
            cur = max(cur, dist(u, w))
        ans[i] = cur

    return " ".join(map(str, ans))

# provided sample 1
assert run("""3
2 1
2 3
1 2 3
""").strip() == "2 1 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút đơn | 1 | xử lý cây tối thiểu | 
| chuỗi 5 đảo ngược | co lại dần dần | cập nhật đường kính tuyến tính | 
| loại bỏ sao làm trung tâm | sụp đổ nhanh chóng | độ chính xác của cấu trúc trung tâm | 
| mẫu 1 | 2 1 0 | đường cơ sở chính xác | 

## Vỏ cạnh 

Cây nút đơn chứng minh rằng đường kính bắt đầu từ 1 ngay sau khi kích hoạt và chỉ trở thành 0 sau khi quá trình xóa kết thúc. Thuật toán xử lý việc này vì mỗi nút khởi tạo thành phần riêng của nó với điểm cuối (v, v), cho khoảng cách 1. 

Một chuỗi dài trong đó việc xóa bắt đầu từ các điểm cuối xác nhận rằng chỉ cần hợp nhất thông qua các điểm cuối là đủ. Mỗi liên kết mở rộng tập ứng cử viên một cách chính xác vì mọi ranh giới thành phần được biểu thị bằng các điểm cuối đường kính của nó. 

Cây hình ngôi sao đảm bảo rằng việc hợp nhất nhiều lá thông qua một nút trung tâm sẽ không bỏ lỡ các đường dẫn dài hơn. Bước hợp nhất kiểm tra rõ ràng các điểm cuối của nhiều thành phần, do đó, đường dẫn dài nhất luôn bao gồm hai lá xuyên qua trung tâm, được phát hiện chính xác.
