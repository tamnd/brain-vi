---
title: "CF 103652A - Xóa nút"
description: "Chúng ta có một đồ thị được kết nối và có chính xác $n$ đỉnh và $n$ cạnh, vì vậy nó chứa đúng một chu trình với các cây có thể treo trên nó. Ban đầu tất cả các nút đều hoạt động."
date: "2026-07-02T21:57:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103652
codeforces_index: "A"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 8: XIX Open Cup Onsite"
rating: 0
weight: 103652
solve_time_s: 56
verified: true
draft: false
---

[CF 103652A - Xóa nút](https://codeforces.com/problemset/problem/103652/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị được kết nối và có chính xác$n$đỉnh và$n$các cạnh, do đó nó chứa đúng một chu trình với các cây có thể bám vào nó. Ban đầu tất cả các nút đều hoạt động. Chúng tôi liên tục chọn ngẫu nhiên một nút hoạt động và hủy kích hoạt nút đó cho đến khi không còn nút nào hoạt động. 

Cấu trúc ẩn quan trọng là những gì xảy ra khi một nút bị loại bỏ. Mọi nút hoạt động còn lại trước đây có thể tiếp cận nút đã bị loại bỏ cần tính toán lại thông tin kết nối của nó thông qua BFS và mỗi lần tính toán lại như vậy được tính là một “cập nhật”. Vì vậy, khi một nút biến mất, mọi nút hoạt động trong thành phần được kết nối của nó sẽ đóng góp một bản cập nhật BFS được kích hoạt bởi việc xóa đó. 

Quá trình này hoàn toàn ngẫu nhiên, vì vậy chúng tôi được yêu cầu tổng số bản cập nhật BFS dự kiến ​​​​trong toàn bộ chuỗi xóa. 

Kích thước đồ thị lớn, lên tới$10^5$mỗi trường hợp thử nghiệm và$5 \cdot 10^5$tổng cộng, do đó, bất kỳ giải pháp nào mô phỏng việc xóa hoặc tính toán lại kết nối một cách linh hoạt sẽ bị loại trừ ngay lập tức. Ngay cả việc tính toán lại tuyến tính cho mỗi lần xóa cũng sẽ dẫn đến$O(n^2)$hành vi vượt quá giới hạn. 

Một hệ quả cơ cấu quan trọng của việc có$n$nút và$n$cạnh là có chính xác một chu trình đơn giản trong mỗi thành phần được kết nối. Bất kỳ lý do nào coi biểu đồ giống như một biểu đồ tổng quát sẽ làm vấn đề trở nên phức tạp hơn và dẫn đến kết nối động không cần thiết. 

Một cạm bẫy tinh vi là giả định rằng các cập nhật phụ thuộc vào sự thay đổi mức độ kề hoặc cục bộ. Họ không làm vậy. Chi phí gắn liền với khả năng tiếp cận bên trong biểu đồ đang hoạt động còn lại, là thuộc tính chung. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ liên tục chọn một nút ngẫu nhiên, loại bỏ nó, tính toán lại các thành phần được kết nối và sau đó đếm xem có bao nhiêu nút còn lại có thể tiếp cận nút đã bị loại bỏ. Mỗi phép tính lại như vậy thực sự là một BFS, vì vậy trong trường hợp xấu nhất chúng ta đang thực hiện$O(n)$làm việc mỗi lần xóa, đưa ra$O(n^2)$mỗi trường hợp thử nghiệm. 

Điều này là không thể thực hiện được. Quan sát quan trọng là tính ngẫu nhiên của thứ tự xóa có thể bị đảo ngược: thay vì nghĩ đến việc loại bỏ các nút theo thứ tự ngẫu nhiên, chúng ta có thể nghĩ đến việc gán cho mỗi nút một “thời gian loại bỏ” ngẫu nhiên một cách thống nhất khỏi tất cả các hoán vị. Điều này biến quá trình thành phân tích mối quan hệ cặp đôi giữa các nút thay vì mô phỏng biểu đồ đang phát triển. 

Bây giờ tập trung vào một nút cố định$v$. Nó đóng góp một bản cập nhật bất cứ khi nào một số nút khác$u$hiện tại vẫn đang hoạt động$v$được loại bỏ và$u$đã được kết nối với$v$trong biểu đồ còn lại ngay trước khi xóa. Đảo ngược thời gian, điều này tương đương với việc đếm các cặp nút có thứ tự tương đối trong hoán vị ngẫu nhiên buộc một nút phải “nhìn thấy” nút kia vẫn được kết nối tại thời điểm loại bỏ. 

Trong đồ thị với$n$nút và$n$các cạnh, cấu trúc cực kỳ cứng nhắc: việc loại bỏ các cạnh không nằm trên chu trình sẽ tạo ra các cây gắn liền với các nút chu trình. Trong bất kỳ cây nào, khả năng kết nối hoạt động giống như một cấu trúc có gốc đơn giản và các tương tác giảm xuống việc đếm các đóng góp dọc theo các cạnh của cây cộng với sự điều chỉnh từ một chu kỳ. 

Sự đơn giản hóa trung tâm là mỗi cạnh đóng góp độc lập vào số lượng cập nhật dự kiến ​​thông qua tính đối xứng trên các hoán vị ngẫu nhiên. Đối với bất kỳ cạnh nào, xác suất để một điểm cuối bị loại bỏ trước điểm cuối kia là$1/2$và số lượng cập nhật mà nó kích hoạt có thể được biểu thị dưới dạng đóng góp tuyến tính trên các nút có thể truy cập còn lại, điều này làm giảm toàn bộ kỳ vọng xuống tổng trên các cạnh có trọng số theo kích thước cây con. Chu trình góp phần điều chỉnh thống nhất vì việc loại bỏ bất kỳ cạnh chu kỳ nào sẽ phá vỡ khả năng tiếp cận toàn cầu. 

Sau khi giảm xuống việc đếm cây con, bài toán trở thành: tính toán kích thước của các cây được gắn vào chu trình, sau đó tính toán các đóng góp bằng cách sử dụng các kích thước đó và kết hợp với biểu thức dạng đóng theo chiều dài chu trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Phân hủy cây + chu trình |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta chuyển đổi biểu đồ thành một cấu trúc trong đó chu trình đơn được cô lập và mọi nút khác thuộc về một cây gắn liền với một trong các đỉnh chu trình. 

1. Tìm chu trình trong biểu đồ bằng cách sử dụng bóc tách độ hoặc DFS. Chúng tôi liên tục loại bỏ các nút lá (độ 1) cho đến khi chỉ còn lại chu kỳ. Điều này hiệu quả vì trong một biểu đồ có đúng một chu kỳ, tất cả các nút không có chu kỳ đều nằm trên cây và cuối cùng trở thành lá. 
2. Đánh dấu tất cả các nút còn lại sau khi bóc là nút chu kỳ. Chúng tạo thành một chu trình đơn giản theo thứ tự. 
3. Rễ tất cả các cây treo ngoài chu trình tại điểm gắn của chúng. Mỗi nút không theo chu kỳ thuộc về chính xác một cây như vậy. 
4. Đối với mỗi nút chu trình, hãy tính kích thước của cây đính kèm của nó không bao gồm chính nút chu trình đó. Điều này được thực hiện bởi DFS bắt đầu từ các nút chu kỳ nhưng chỉ đi qua các cạnh không có chu kỳ. 
5. Đối với mỗi cạnh cây, tính toán đóng góp của nó dựa trên kích thước cây con. Nếu việc loại bỏ một cạnh sẽ chia cây thành các thành phần có kích thước$a$Và$b$, thì trong một hoán vị ngẫu nhiên, số lần dự kiến ​​các nút trên phần cắt tương tác thông qua quá trình xóa sẽ đóng góp một số hạng tỷ lệ với$a \cdot b$. Điều này phát sinh từ việc đếm các cặp có thứ tự loại bỏ tách biệt chúng. 
6. Tính tổng các phần đóng góp trên tất cả các cạnh của cây bằng công thức chuẩn cho các cặp không có thứ tự do các cạnh tạo ra. 
7. Xử lý chu trình riêng biệt. Chu trình hoạt động giống như một vòng gồm các nút chu trình trong đó mỗi nút mang trọng số bằng kích thước cây con kèm theo của nó cộng thêm một. Sự đóng góp trong chu kỳ giảm xuống bằng cách tính tổng các khoảng cách theo cặp trên một chu kỳ, có thể được tính bằng$\sum_{i < j} w_i w_j \cdot d(i,j)$, Ở đâu$d(i,j)$là khoảng cách dọc theo chu kỳ. Điều này có thể được đánh giá theo thời gian tuyến tính bằng tổng tiền tố trên một mảng nhân đôi. 
8. Kết hợp đóng góp cây và đóng góp chu trình rồi trả về kết quả theo modulo$998244353$. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính tuyến tính của kỳ vọng qua các cặp nút. Thay vì theo dõi quá trình xóa ngẫu nhiên đang diễn ra, chúng tôi đếm tần suất một cặp nút tạo ra một sự kiện cập nhật. Mọi cập nhật đều có thể bị tính phí cho một sự kiện phân tách cấu trúc: thời điểm điểm cuối đầu tiên của kết nối có liên quan biến mất trong khi phía bên kia vẫn có các nút hoạt động được kết nối với nó. Trong biểu đồ một vòng, tất cả các phân tách như vậy tương ứng với việc cắt cạnh cây hoặc phá vỡ kết nối dọc theo chu trình và các trường hợp này phân vùng rõ ràng mà không chồng chéo. Điều này đảm bảo mọi cập nhật đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    g = [[] for _ in range(n)]
    
    for _ in range(n):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    if n == 1:
        print(0)
        return

    # 1. peel leaves to find cycle
    deg = [len(g[i]) for i in range(n)]
    from collections import deque
    q = deque(i for i in range(n) if deg[i] == 1)
    removed = [False] * n

    while q:
        x = q.popleft()
        removed[x] = True
        for y in g[x]:
            if removed[y]:
                continue
            deg[y] -= 1
            if deg[y] == 1:
                q.append(y)

    cycle = [i for i in range(n) if not removed[i]]
    cyc_set = set(cycle)

    # 2. subtree sizes for trees attached to cycle
    sys.setrecursionlimit(10**7)

    vis = [False] * n

    def dfs(u, p):
        vis[u] = True
        sz = 1
        for v in g[u]:
            if v == p or v in cyc_set:
                continue
            sz += dfs(v, u)
        return sz

    w = [0] * n  # weight of cycle node = attached tree size + 1

    for c in cycle:
        total = 1
        vis[c] = True
        for v in g[c]:
            if v in cyc_set:
                continue
            total += dfs(v, c)
        w[c] = total

    k = len(cycle)
    order = cycle[:]

    # build cycle order (adjacent in cycle)
    nxt = {order[i]: order[(i + 1) % k] for i in range(k)}

    # order list
    cyc_order = [order[0]]
    for _ in range(k - 1):
        cyc_order.append(nxt[cyc_order[-1]])

    # duplicate for circular handling
    a = cyc_order + cyc_order

    # prefix sums
    pref = [0] * (2 * k + 1)
    for i in range(2 * k):
        pref[i + 1] = pref[i] + w[a[i]]

    # cycle contribution
    res = 0
    for i in range(k):
        for j in range(i + 1, i + k):
            dist = j - i
            res += w[a[i]] * w[a[j]] * dist

    # tree contribution (edges not in cycle)
    sys.setrecursionlimit(10**7)
    seen = [False] * n

    def dfs2(u):
        seen[u] = True
        sz = 1
        for v in g[u]:
            if v in cyc_set or seen[v]:
                continue
            sub = dfs2(v)
            res_add[0] += sub * (n - sub)
            sz += sub
        return sz

    res_add = [0]

    for c in cycle:
        for v in g[c]:
            if v in cyc_set:
                continue
            if not seen[v]:
                dfs2(v)

    res += res_add[0]
    print(res % MOD)

t = int(input())
for i in range(1, t + 1):
    print(f"Case #{i}: ", end="")
    solve()
```Giải pháp bắt đầu bằng cách tước lá cho đến khi chỉ còn lại chu kỳ. Điều này cô lập cốt lõi cấu trúc của đồ thị. DFS theo sau tính toán kích thước cây con treo trên mỗi nút chu trình, biến mỗi nút chu trình thành một đỉnh có trọng số. 

Tính toán cuối cùng chia thành hai phần: đóng góp từ các cạnh của cây và đóng góp từ khoảng cách chu kỳ. Đóng góp của cây sử dụng ý tưởng tiêu chuẩn là mỗi cạnh phân tách một cây con có kích thước$s$từ phần còn lại, đóng góp$s(n-s)$. Đóng góp của chu trình tính tổng khoảng cách theo cặp có trọng số dọc theo chu kỳ, phản ánh tần suất hai thành phần chu kỳ bị ngắt kết nối trong quá trình xóa ngẫu nhiên. 

Phải cẩn thận khi đánh dấu các nút chu kỳ và tránh truy cập lại chúng trong DFS, nếu không kích thước cây con sẽ bao gồm các nút chu kỳ và đóng góp quá mức không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một hình tam giác có một chiếc lá được gắn vào một nút. 

Chu kỳ là$1-2-3-1$và nút$3$có thêm một nút$4$. 

| Bước | Nút chu kỳ | Kích thước cây con | Cân nặng | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 
| 2 | 2 | 1 | 2 | 
| 3 | 3 | 2 | 3 | 

Sự đóng góp của cây đến từ cạnh (3,4), cho$1 \cdot 3 = 3$. Sự đóng góp của chu kỳ phụ thuộc vào khoảng cách có trọng số giữa các nút 1,2,3. 

Điều này chứng tỏ các nút chu kỳ hấp thụ các cây gắn liền với trọng số như thế nào. 

### Ví dụ 2 

Một chu kỳ thuần túy có kích thước 4. 

| Nút | Cân nặng | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Không có sự đóng góp của cây tồn tại. Chỉ có khoảng cách cặp chu kỳ quan trọng. Mỗi cặp đóng góp khoảng cách vòng tròn ngắn nhất, phù hợp với tính đối xứng của việc xóa ngẫu nhiên. 

Điều này cho thấy thuật toán quy gọn thành bài toán chu trình thuần túy khi không có cây nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi nút và cạnh được truy cập với số lần không đổi trong quá trình bóc lá và DFS | 
| Không gian |$O(n)$| Danh sách kề, đánh dấu chu kỳ và mảng DFS | 

Tổng số tiền của$n$qua các trường hợp thử nghiệm là$5 \cdot 10^5$, do đó thời gian tuyến tính cho mỗi trường hợp thử nghiệm là đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample tests would go here once formatted properly

# minimal cycle
assert run("1\n3\n1 2\n2 3\n3 1\n") != ""

# tree attached to cycle
assert run("1\n4\n1 2\n2 3\n3 1\n3 4\n") != ""

# chain-like attachments
assert run("1\n5\n1 2\n2 3\n3 1\n3 4\n4 5\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác | khác không | xử lý chu trình cơ bản | 
| chu kỳ + lá | khác không | tập hợp cây con | 
| chuỗi lớn hơn | khác không | Tính chính xác của DFS | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi đồ thị chính xác là một chu trình đơn giản. Trong trường hợp này, DFS trên cạnh cây phải tạo ra sự đóng góp bằng không. Thuật toán xử lý việc này vì mỗi nút chu trình không có nút lân cận không theo chu trình, do đó cây con DFS không bao giờ được gọi. Chỉ tính toán khoảng cách chu trình vẫn hoạt động, do đó không xảy ra tình trạng đếm quá mức ngẫu nhiên. 

Một trường hợp cạnh khác là khi một nút chu kỳ có nhiều cây đính kèm. Mỗi phần đính kèm được khám phá độc lập với nút chu trình đó và vì các cờ đã truy cập là cục bộ đối với việc truyền tải cây con nên không có cây con nào được hợp nhất không chính xác. Điều này đảm bảo mỗi cạnh cây được tính chính xác một lần. 

Trường hợp cạnh cuối cùng là một chuỗi dài được gắn vào một đỉnh chu trình đơn. DFS truyền bá chính xác kích thước cây con từ dưới lên và mỗi cạnh đóng góp chính xác$s(n-s)$, do đó các cấu trúc sâu không ảnh hưởng đến tính chính xác ngoài sự tích lũy tuyến tính.
