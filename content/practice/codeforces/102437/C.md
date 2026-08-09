---
title: "CF 102437C - \u0415\u0434\u0438\u043d\u0430\u044f \u0441\u0435\u0442\u044c"
description: "Chúng ta có một đồ thị vô hướng liên thông có các cạnh tạo thành một cây xương rồng: mỗi con đường thuộc về nhiều nhất một chu trình đơn. Mỗi thành phố phải nhận một trong ba loại máy phát và các thành phố lân cận phải nhận các loại khác nhau."
date: "2026-08-08T15:12:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 257
verified: true
draft: false
---

[CF 102437C - \u0415\u0434\u0438\u043d\u0430\u044f \u0441\u0435\u0442\u044c](https://codeforces.com/problemset/problem/102437/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng liên thông có các cạnh tạo thành một cây xương rồng: mỗi con đường thuộc về nhiều nhất một chu trình đơn. Mỗi thành phố phải nhận một trong ba loại máy phát và các thành phố lân cận phải nhận các loại khác nhau. Loại 3 đắt tiền nên nhiệm vụ là giảm thiểu số đỉnh được tô màu bằng loại 3. 

Đầu vào chứa các thành phố và đường của biểu đồ. Đầu ra được yêu cầu là số đỉnh tối thiểu có thể sử dụng loại 3. Theo đảm bảo xương rồng đã cho, luôn tồn tại 3 màu hợp lệ, do đó`-1`thực tế không thể xảy ra đối với đầu vào hợp lệ. Một cây xương rồng là suy biến 2, bởi vì mọi đồ thị con không tầm thường đều có một đỉnh có bậc nhiều nhất là hai, và những đồ thị như vậy luôn có thể tô được 3 màu. 

Giới hạn kích thước của (n=10^5) và (m=1.5\cdot10^5) loại trừ mọi thứ bậc hai trong kích thước biểu đồ. Thậm chí (O(n^2)) có nghĩa là khoảng (10^{10}) phép toán nguyên thủy trong trường hợp xấu nhất. Chúng ta cần một thuật toán gần tuyến tính trong (n+m). Điều kiện xương rồng chính xác là hạn chế về mặt cấu trúc cho phép chúng ta đạt được nghiệm như vậy. 

Có một số trường hợp khó khăn khi cách tiếp cận đơn giản hơn lại đưa ra câu trả lời sai. Một đỉnh không có cạnh nên nó không cần bộ truyền phát đắt tiền.```
1 0
```Câu trả lời là`0`. Giải pháp giả sử mọi đồ thị liên thông đều có ít nhất một cạnh sẽ xử lý sai trường hợp này. 

Chu kỳ chẵn là chu kỳ lưỡng cực nên chỉ cần hai loại máy phát đầu tiên.```
4 4
1 2
2 3
3 4
4 1
```Câu trả lời là`0`. Phương pháp sạc mỗi chu kỳ cho máy phát loại 3 sẽ trả về không chính xác`1`. 

Các chu trình lẻ yêu cầu loại 3, nhưng một số chu trình lẻ có thể có cùng một đỉnh đắt tiền. Ví dụ, hãy xem xét hai hình tam giác có một đỉnh chung.```
5 6
1 2
2 3
3 1
1 4
4 5
5 1
```Câu trả lời là`1`. Tô màu đỉnh 1 bằng loại 3 thì mỗi tam giác có thể tô màu loại 1 và 2 trên 2 đỉnh còn lại của nó. Chỉ cần đếm chu kỳ lẻ sẽ trả về không chính xác`2`. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp nhất sẽ gán một trong ba màu cho mỗi đỉnh, sau đó kiểm tra xem tất cả các cạnh có điểm cuối có màu khác nhau hay không. Có chính xác (3^n) bài tập và việc kiểm tra một bài tập sẽ mất (O(n+m)) thời gian. Trong trường hợp xấu nhất, điều này có nghĩa là (3^{100000}) bài tập và khoảng (150000\cdot3^{100000}) kiểm tra cạnh, điều này hoàn toàn không khả thi. 

Lực lượng vũ phu hoạt động vì nó xem xét rõ ràng mọi màu sắc có thể có. Vấn đề là hầu hết đồ thị không cần phải được xem xét đồng thời. Trong cây xương rồng, đồ thị có thể được phân tách thành các khối và mỗi khối là một cạnh hoặc một chu trình đơn giản. Các khối khác nhau tương tác qua nhiều nhất một đỉnh chung. Điều đó biến cấu trúc khối thành một cái cây. 

Điều này gợi ý lập trình động trên cây khối. Đối với mỗi đỉnh (v), chúng ta giữ ba giá trị, một giá trị cho mỗi màu có thể có của (v). Giá trị biểu thị số lượng tối thiểu của máy phát loại 3 trong toàn bộ phần biểu đồ bên dưới (v), giả sử rằng (v) có màu đã chọn. 

Một khối kết nối nhiều đỉnh sau đó có thể được xử lý độc lập sau khi màu của đỉnh cha của nó được cố định. Đối với một cạnh, chúng ta chỉ cần chọn một màu khác với màu gốc. Đối với một chu trình, chúng ta sửa màu của đỉnh cha và chạy một đường nhỏ DP quanh phần còn lại của chu trình, cuối cùng kiểm tra xem đỉnh cuối cùng có khác với đỉnh cha hay không. 

Phần không cần thiết là xây dựng các khối một cách hiệu quả. Thuật toán của Tarjan dành cho các thành phần được kết nối đôi thực hiện chính xác điều đó trong (O(n+m)). Trong một cây xương rồng, mọi thành phần liên kết đôi được tạo ra đều được đảm bảo là một cạnh hoặc một chu trình đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^n(n+m))) | (O(n+m)) | Quá chậm | 
| Cây khối DP | (O(n+m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị vô hướng và tìm tất cả các thành phần liên thông đôi bằng thuật toán Tarjan. Trong khi thực hiện DFS, hãy giữ các cạnh đi ngang trên một ngăn xếp. Bất cứ khi nào`low[child] >= tin[parent]`, bật các cạnh cho đến khi cạnh DFS của con bị loại bỏ. Những cạnh đó tạo thành một thành phần được kết nối hai chiều. 

Bởi vì đồ thị đầu vào là một cây xương rồng nên mỗi thành phần hoặc là một cây cầu hoặc một chu trình đơn giản. Chúng tôi cũng thu thập các đỉnh thuộc về mọi thành phần. 
2. Xây dựng cấu trúc tỷ lệ giữa các đỉnh và các thành phần. Đối với mọi thành phần, hãy lưu trữ các đỉnh biểu đồ mà nó chứa. Với mỗi đỉnh, lưu trữ danh sách các thành phần chứa nó. 

Biểu đồ tỷ lệ kết quả là một cây sau khi coi các đỉnh và thành phần ban đầu là hai loại nút khác nhau. Đây là cây xương rồng cắt khối. 
3. Gốc cây khối này tại bất kỳ đỉnh nào của đồ thị, chẳng hạn như đỉnh`0`. Đối với mọi thành phần, hãy nhớ đỉnh nào là cha mẹ của nó. Đối với mọi đỉnh không phải gốc, hãy nhớ thành phần nào là đỉnh của nó. 

Bây giờ chúng tôi có mối quan hệ cha-con rõ ràng. Một thành phần có chính xác một đỉnh cha, trong khi tất cả các đỉnh khác của nó đều dẫn đến các cây con độc lập. 
4. Xác định`dp[v][c]`là số lượng tối thiểu của máy phát loại 3 trong cây con có gốc ở đỉnh`v`, giả sử rằng`v`nhận được màu sắc`c`. 

Đóng góp trực tiếp của`v`là`1`khi`c`là loại 3 và`0`nếu không thì. Mỗi thành phần con đóng góp giá trị tối ưu riêng cho cùng một màu của`v`. 

Như vậy, 

[ 
dp[v][c] = [c=3] + \sum_{\text{các thành phần con } B} blockDP[B][c]. 
] 
5. Xử lý các đỉnh theo thứ tự ngược lại với việc duyệt cây khối. Trước khi tính toán`dp[v]`, tất cả các đỉnh bên trong mỗi thành phần con đều đã được tính toán. Điều này cung cấp cho chúng tôi mọi thứ cần thiết để tính DP của thành phần đó. 
6. Đối với thành phần cầu có đỉnh cha`p`và đỉnh khác`u`, tính toán 

[ 
khốiDP[B][c] = 
\min_{d\ne c} dp[u][d]. 
] 

Hạn chế duy nhất được áp đặt bởi cạnh là hai điểm cuối của nó có màu khác nhau. 
7. Đối với thành phần chu trình, trước tiên hãy sắp xếp các đỉnh của nó như sau: 

[ 
p,v_1,v_2,\ldots,v_{k-1},p, 
] 

ở đâu`p`là đỉnh cha. 

Sửa màu`c`của`p`. Bắt đầu DP ba trạng thái chỉ có màu`c`được phép tại`p`, với chi phí bằng không. Sau đó xử lý (v_1,v_2,\ldots,v_{k-1}) theo thứ tự. Khi gán màu`d`tới đỉnh tiếp theo thì đỉnh trước phải có một trong hai màu khác với`d`. 

Sau khi đỉnh cuối cùng được xử lý, chỉ các trạng thái có màu khác với màu gốc cố định mới hợp lệ, vì đỉnh cuối cùng và đỉnh gốc được kết nối bằng cạnh chu kỳ đóng. 
8. Khi tất cả các thành phần con đã được tích hợp vào mọi đỉnh, câu trả lời là 

[ 
\min_{c\in{1,2,3}} dp[root][c]. 
] 

Vì một cây xương rồng hợp lệ luôn có 3 màu nên ít nhất một trong các giá trị này là hữu hạn. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`dp[v][c]`chứa chi phí tô màu tối ưu cho chính xác phần của cây khối bên dưới`v`, với`v`cố định vào màu sắc`c`. Các thành phần con khác nhau của một đỉnh chỉ giao nhau ở đỉnh đó, vì vậy sau khi màu của nó được cố định, các lựa chọn của chúng là độc lập và chi phí tối ưu của chúng có thể được cộng thêm. 

Đối với một thành phần cạnh, việc kiểm tra hai màu khác nhau có thể có chính xác là điều kiện tô màu thích hợp. Đối với một chu trình, đường DP xem xét mọi màu có thể có của mọi đỉnh trong khi thực thi bất đẳng thức trên các đỉnh liên tiếp và quá trình chuyển đổi cuối cùng thực thi cạnh đóng. Do đó, mọi màu sắc thích hợp của thành phần đều được thể hiện và màu có chi phí tối thiểu sẽ được chọn. 

Bởi vì mỗi khối chỉ được xử lý sau khi tất cả các đỉnh con của nó được giải quyết, nên bất biến sẽ lan truyền từ lá đến gốc. Do đó, mức tối thiểu cuối cùng trong ba màu có thể có của rễ sẽ xem xét mọi màu hợp lệ của toàn bộ cây xương rồng và chọn màu rẻ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10 ** 9

def solve_case(n, edges):
    m = len(edges)

    graph = [[] for _ in range(n)]
    for eid, (u, v) in enumerate(edges):
        graph[u].append(eid)
        graph[v].append(eid)

    # Iterative Tarjan algorithm for biconnected components.
    tin = [-1] * n
    low = [0] * n
    parent = [-1] * n
    parent_edge = [-1] * n
    it = [0] * n

    edge_stack = []
    components = []

    timer = 0
    tin[0] = low[0] = timer
    timer += 1

    dfs_stack = [0]

    while dfs_stack:
        u = dfs_stack[-1]

        if it[u] < len(graph[u]):
            eid = graph[u][it[u]]
            it[u] += 1

            if eid == parent_edge[u]:
                continue

            a, b = edges[eid]
            v = b if a == u else a

            if tin[v] == -1:
                parent[v] = u
                parent_edge[v] = eid
                edge_stack.append(eid)

                tin[v] = low[v] = timer
                timer += 1

                dfs_stack.append(v)
            elif tin[v] < tin[u]:
                edge_stack.append(eid)
                if tin[v] < low[u]:
                    low[u] = tin[v]
        else:
            dfs_stack.pop()

            p = parent[u]
            if p != -1:
                if low[u] < low[p]:
                    low[p] = low[u]

                if low[u] >= tin[p]:
                    comp = []
                    while True:
                        eid = edge_stack.pop()
                        comp.append(eid)
                        if eid == parent_edge[u]:
                            break
                    components.append(comp)

    k = len(components)

    # Vertices belonging to every component.
    comp_vertices = [[] for _ in range(k)]
    incident = [[] for _ in range(n)]

    for cid, comp_edges in enumerate(components):
        vertices = set()

        for eid in comp_edges:
            u, v = edges[eid]
            vertices.add(u)
            vertices.add(v)

        vertices = list(vertices)
        comp_vertices[cid] = vertices

        for v in vertices:
            incident[v].append(cid)

    # Root the block-cut tree at vertex 0.
    # parent_comp[v] is the component through which v is reached.
    parent_comp = [-2] * n
    parent_comp[0] = -1

    # comp_parent[c] is the vertex through which component c is reached.
    comp_parent = [-1] * k

    order = [0]

    for v in order:
        for cid in incident[v]:
            if cid == parent_comp[v]:
                continue

            comp_parent[cid] = v

            for u in comp_vertices[cid]:
                if u == v:
                    continue

                if parent_comp[u] == -2:
                    parent_comp[u] = cid
                    order.append(u)

    dp = [[0, 0, 0] for _ in range(n)]
    block_dp = [[0, 0, 0] for _ in range(k)]

    # Process bottom-up.
    for v in reversed(order):
        # First calculate all components for which v is the parent.
        for cid in incident[v]:
            if comp_parent[cid] != v:
                continue

            comp_edges = components[cid]
            vertices = comp_vertices[cid]

            # A component consisting of one edge.
            if len(comp_edges) == 1:
                eid = comp_edges[0]
                a, b = edges[eid]
                u = b if a == v else a

                for c in range(3):
                    best = INF
                    for d in range(3):
                        if d != c and dp[u][d] < best:
                            best = dp[u][d]
                    block_dp[cid][c] = best

            else:
                # A cactus biconnected component with more than one edge
                # is a simple cycle.
                local = {x: [] for x in vertices}

                for eid in comp_edges:
                    a, b = edges[eid]
                    local[a].append(b)
                    local[b].append(a)

                # Order the cycle starting from its parent vertex v.
                cycle = [v]
                prev = -1
                cur = v

                while True:
                    x, y = local[cur]
                    nxt = x if x != prev else y

                    if nxt == v:
                        break

                    cycle.append(nxt)
                    prev, cur = cur, nxt

                for parent_color in range(3):
                    cur_dp = [INF, INF, INF]
                    cur_dp[parent_color] = 0

                    for u in cycle[1:]:
                        nxt_dp = [INF, INF, INF]

                        for new_color in range(3):
                            best = INF
                            for old_color in range(3):
                                if old_color == new_color:
                                    continue
                                if cur_dp[old_color] < best:
                                    best = cur_dp[old_color]

                            nxt_dp[new_color] = best + dp[u][new_color]

                        cur_dp = nxt_dp

                    best = INF
                    for last_color in range(3):
                        if last_color == parent_color:
                            continue
                        if cur_dp[last_color] < best:
                            best = cur_dp[last_color]

                    block_dp[cid][parent_color] = best

        # Now every child component of v is solved.
        for color in range(3):
            value = 1 if color == 2 else 0

            for cid in incident[v]:
                if comp_parent[cid] == v:
                    value += block_dp[cid][color]

            dp[v][color] = value

    return min(dp[0])

def main():
    n, m = map(int, input().split())
    edges = [tuple(map(lambda x: int(x) - 1, input().split()))
             for _ in range(m)]

    print(solve_case(n, edges))

if __name__ == "__main__":
    main()
```Cấu trúc đồ thị lưu trữ mỗi cạnh bằng một ID nguyên. Điều này là cần thiết cho thuật toán Tarjan vì quan hệ cha là quan hệ cạnh chứ không chỉ đơn thuần là quan hệ đỉnh. Đặc biệt, khi một DFS vô hướng nhìn thấy cạnh dẫn trở lại cạnh mẹ của nó, thì cạnh chính xác đó phải bị bỏ qua. 

Việc triển khai Tarjan mang tính lặp lại chứ không phải đệ quy. Một cây xương rồng có thể chứa một đường dẫn gồm (10^5) đỉnh, do đó, DFS đệ quy sẽ yêu cầu tăng giới hạn đệ quy của Python và cũng sẽ gây áp lực không cần thiết lên ngăn xếp lệnh gọi Python. Ngăn xếp rõ ràng đưa ra cùng một thứ tự DFS mà không có rủi ro đó. 

Ngăn xếp cạnh chứa chính xác các cạnh DFS thuộc về thành phần được kết nối hai chiều hiện tại. Khi`low[u] >= tin[parent[u]]`, không có cạnh bên dưới`u`có thể kết nối ở trên`parent[u]`, do đó các cạnh lên tới`parent_edge[u]`tạo thành một khối hoàn chỉnh. 

các`incident`danh sách là thứ biến các thành phần được kết nối hai chiều thành một cây cắt khối. Đồ thị ban đầu có thể có nhiều chu trình chạm vào cùng một đỉnh, nhưng biểu diễn cắt khối vẫn có cấu trúc cây. 

Chu trình DP chỉ sử dụng ba trạng thái. Đối với mỗi màu mới, cần có trạng thái rẻ nhất trước đó với một màu khác. Hạn chế cuối cùng đối với màu sắc của bố mẹ là điều cần thiết. Việc bỏ qua nó sẽ vô tình coi chu trình là một đường dẫn và có thể chấp nhận màu không hợp lệ. 

Tất cả chi phí tối đa là (n), vì vậy số nguyên Python thông thường là quá đủ. Không có vấn đề tràn số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị bao gồm ba hình tam giác. Tam giác đầu tiên chứa các đỉnh`1, 2, 3`, trong khi hai hình tam giác còn lại được gắn ở các đỉnh`2`Và`3`. 

Đối với mỗi tam giác lá, việc cố định đỉnh dùng chung thành loại 3 cho phép hai đỉnh còn lại của nó sử dụng loại 1 và 2, do đó thành phần đó không đóng góp thêm máy phát loại 3 nào. Nếu đỉnh dùng chung sử dụng loại 1 hoặc loại 2 thì một trong hai đỉnh còn lại phải sử dụng loại 3. 

Các giá trị DP thu được là: 

| Đỉnh hoặc thành phần | Trạng thái cho loại 1 | Trạng thái cho loại 2 | Trạng thái cho loại 3 | 
| --- | --- | --- | --- | 
| Lá tam giác lúc 2 | 1 | 1 | 0 | 
|`dp[2]`| 1 | 1 | 1 | 
| Lá tam giác lúc 3 | 1 | 1 | 0 | 
|`dp[3]`| 1 | 1 | 1 | 
| Căn tam giác tại 1 | 2 | 2 | 2 | 
|`dp[1]`| 2 | 2 | 3 | 

Câu trả lời là`2`. Một màu tối ưu làm cho đỉnh 1 thuộc loại 2, đỉnh 2 thuộc loại 3 và đỉnh 3 thuộc loại 1. Sau đó, mỗi tam giác đính kèm có thể được hoàn thành với loại 1 và 2, ngoại trừ một loại 3 bổ sung trong tam giác gắn với đỉnh 3. 

### Mẫu 2 

Đồ thị chứa một số chu trình được kết nối qua các đỉnh 2, 9 và 10. Chu trình lớn qua các đỉnh từ 3 đến 8 là chẵn và chu trình qua các đỉnh 10 đến 13 cũng là chẵn. Các hình tam giác là nơi duy nhất có thể buộc thêm loại 3. 

Các trạng thái từ dưới lên có liên quan là: 

| Cấu trúc phụ | Phụ huynh loại 1 | phụ huynh loại 2 | phụ huynh loại 3 | 
| --- | --- | --- | --- | 
| Chu kỳ chẵn qua 3 | 1 | 1 | 0 | 
| Chu kỳ chẵn qua 10 | 1 | 1 | 0 | 
| Tam giác lúc 9 với 10 và 15 | 1 | 1 | 1 | 
|`dp[9]`| 1 | 1 | 2 | 
| Tam giác ở 2 với 9 và 14 | 2 | 2 | 1 | 
|`dp[2]`| 2 | 2 | 2 | 

Mức tối thiểu cuối cùng là`2`. 

Dấu vết cho thấy tại sao DP không thể đơn giản đếm các chu kỳ lẻ. Lựa chọn loại 3 được thực hiện tại một đỉnh dùng chung có thể đồng thời đáp ứng một số ràng buộc chu trình và trạng thái của đỉnh dùng chung đó mang chính xác thông tin mà thành phần cha của nó cần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) | Tarjan xử lý mọi cạnh với số lần không đổi và mỗi quá trình chuyển đổi DP khối chỉ kiểm tra ba màu | 
| Không gian | (O(n+m)) | Biểu đồ, danh sách thành phần, thông tin cắt khối và mảng DP đều tuyến tính ở kích thước đầu vào | 

Đồ thị lớn nhất được phép chỉ có (O(n)) cạnh vì nó là cây xương rồng, với giới hạn đã nêu là (m\le150000). Thuật toán thực hiện một lượng công việc DP ba màu không đổi trên mỗi đỉnh và mỗi cạnh, do đó nó phù hợp thoải mái với yêu cầu về thời gian tuyến tính dự định. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả sử giải pháp trên được lưu dưới dạng`solution.py`và phơi bày`solve_case`.```python
from solution import solve_case

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    n, m = data[0], data[1]

    edges = []
    pos = 2

    for _ in range(m):
        u = data[pos] - 1
        v = data[pos + 1] - 1
        pos += 2
        edges.append((u, v))

    return str(solve_case(n, edges))

# Sample 1
assert run("""\
7 9
1 2
2 3
3 1
2 4
4 5
5 2
3 6
6 7
7 3
""") == "2", "sample 1"

# Sample 2
assert run("""\
15 18
1 2
2 3
3 4
4 5
5 6
6 7
7 8
8 3
2 9
9 10
10 11
11 12
12 13
13 10
2 14
14 9
9 15
15 10
""") == "2", "sample 2"

# Minimum-size graph.
assert run("""\
1 0
""") == "0", "single isolated city"

# Even cycle, completely bipartite.
assert run("""\
4 4
1 2
2 3
3 4
4 1
""") == "0", "even cycle"

# Two triangles sharing one vertex.
# The common vertex can be the only type-3 vertex.
assert run("""\
5 6
1 2
2 3
3 1
1 4
4 5
5 1
""") == "1", "shared odd cycles"

# Maximum-size cactus for n = 100000.
# 49999 triangles share vertex 1, plus one leaf.
# This has 149998 edges, essentially the maximum possible for this n.
n = 100000
edges = []

for i in range(49999):
    a = 2 + 2 * i
    b = a + 1
    edges.append((1, a))
    edges.append((a, b))
    edges.append((b, 1))

edges.append((1, 100000))

assert solve_case(n, edges) == 1, "maximum-size cactus"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`0`| Đồ thị kích thước tối thiểu và trường hợp ranh giới không cạnh | 
| Bốn chu kỳ |`0`| Chu kỳ chẵn chỉ cần hai màu | 
| Hai tam giác có chung đỉnh 1 |`1`| Một đỉnh đắt tiền có thể thỏa mãn nhiều chu trình lẻ | 
| 49999 tam giác chung cộng một lá |`1`| Đầu vào có kích thước tối đa, độ phức tạp tuyến tính, cây khối lớn | 

## Vỏ cạnh 

Đối với một thành phố duy nhất,```
1 0
```danh sách chặn trống. Root không có thành phần con nên ba trạng thái DP của nó là`[0, 0, 1]`. Chọn một trong hai màu đầu tiên sẽ trả lời`0`. 

Đối với bốn chu kỳ```
4 4
1 2
2 3
3 4
4 1
```Tarjan tạo ra một thành phần chu trình chứa tất cả bốn đỉnh. Khi màu gốc được cố định, chu trình DP có thể xen kẽ hai màu còn lại xung quanh ba đỉnh còn lại và đóng chu trình mà không sử dụng loại 3. Thành phần đóng góp bằng 0 khi bản thân thành phần gốc có loại 3 và các trạng thái khác giải thích chính xác khả năng sử dụng của loại 3. Tại gốc, mức tối thiểu là`0`. 

Cho hai tam giác có chung đỉnh 1```
5 6
1 2
2 3
3 1
1 4
4 5
5 1
```cây cắt khối có đỉnh chung là đỉnh của cả hai thành phần tam giác. Nếu đỉnh 1 là loại 3 thì hai đỉnh còn lại của mỗi tam giác có thể sử dụng loại 1 và 2. Do đó, cả hai DP thành phần đều đóng góp 0 cho màu gốc 3, trong khi gốc đóng góp một cho chính nó là loại 3. Câu trả lời là`1`. 

Thử nghiệm kích thước tối đa bao gồm 49999 hình tam giác có chung đỉnh 1 và một lá bổ sung. Cho đỉnh 1 loại 3 cho phép mọi tam giác chỉ sử dụng loại 1 và 2 trên các đỉnh khác của nó, do đó toàn bộ đồ thị có giá đúng bằng một máy phát đắt tiền. Thuật toán xử lý tất cả 149998 cạnh và 100000 đỉnh một lần mà không liệt kê các màu hoặc chu trình trên toàn cầu và trả về`1`.
