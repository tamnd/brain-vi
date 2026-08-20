---
title: "CF 102203C - \u0424\u0430\u0431\u0440\u0438\u043a\u0430"
description: "Nhà máy là một cái cây. Mỗi phòng là một đỉnh, mỗi hành lang là một cạnh, và vì có chính xác (n-1) hành lang và chính xác một đường đi giữa mỗi cặp phòng nên đường đi giữa hai phòng là duy nhất."
date: "2026-08-18T11:23:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "C"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 153
verified: true
draft: false
---

[CF 102203C - \u0424\u0430\u0431\u0440\u0438\u043a\u0430](https://codeforces.com/problemset/problem/102203/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhà máy là một cái cây. Mỗi phòng là một đỉnh, mỗi hành lang là một cạnh, và vì có chính xác (n-1) hành lang và chính xác một đường đi giữa mỗi cặp phòng nên đường đi giữa hai phòng là duy nhất. 

Đối với mọi yêu cầu ((s_i,f_i)), mọi người phải có thể đi bộ từ (s_i) đến (f_i) sau khi mọi hành lang đã được làm một chiều. Vì đồ thị cơ bản là một cây nên chỉ có một đường đi duy nhất từ ​​(s_i) đến (f_i). Do đó, mọi cạnh trên tuyến đường đó đều có hướng cưỡng bức. Nhiệm vụ là xác định xem tất cả các hướng bắt buộc này có nhất quán lẫn nhau hay không và nếu có thì xuất ra một hướng của mỗi hành lang đáp ứng tất cả các yêu cầu. 

Các giới hạn đạt tới (2\cdot10^5) cho cả số lượng phòng và số lượng yêu cầu. Một giải pháp kiểm tra mọi hướng có thể ngay lập tức là không thể thực hiện được vì một cây có (n-1) cạnh có (2^{n-1}) hướng. Ngay cả một cách tiếp cận đi rõ ràng dọc theo mọi đường dẫn được yêu cầu cũng có thể đạt tới (O(nm)), tức là khoảng (4\cdot10^{10}) lượt truy cập cạnh ở kích thước tối đa. Giải pháp dự định phải xử lý cây và tất cả các yêu cầu gần như tuyến tính. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ quá trình triển khai. Đầu tiên là một yêu cầu có điểm cuối bằng nhau. Ví dụ,```
1 1
1 1
```không có hành lang nào cả và rõ ràng là thỏa mãn nên câu trả lời là`YES`. Việc triển khai đường dẫn khác biệt giả sử hai điểm cuối là khác nhau có thể vô tình tạo ra ràng buộc giả. 

Thứ hai là hai yêu cầu yêu cầu hướng ngược nhau trên cùng một cạnh:```
2 2
1 2
1 2
2 1
```Câu trả lời đúng là`NO`. Cả hai yêu cầu đều sử dụng cạnh duy nhất, nhưng một yêu cầu yêu cầu (1\to2) trong khi yêu cầu kia yêu cầu (2\to1). Kiểm tra các yêu cầu một cách độc lập và định hướng một cạnh khi nó gặp lần đầu có thể âm thầm chấp nhận trường hợp này. 

Thứ ba là một yêu cầu được truyền qua nhiều tổ tiên. Coi như```
3 1
1 2
2 3
3 1
```Hướng hợp lệ duy nhất là (3\to2\to1). Một cách tiếp cận đơn giản là định hướng mọi cạnh từ gốc được chọn tới các cạnh con của nó sẽ tạo ra (1\to2\to3), đáp ứng cấu trúc cây nhưng vi phạm yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê mọi hướng của các cạnh (n-1). Đối với mỗi hướng, hãy kiểm tra mọi yêu cầu và kiểm tra xem tất cả các cạnh trên đường dẫn duy nhất của nó có điểm từ đầu đến cuối hay không. Điều này đúng vì mọi câu trả lời có thể đều được xem xét rõ ràng. Với việc truyền tải đường dẫn đơn giản, một hướng có thể yêu cầu (O(mn)) hoạt động trong trường hợp xấu nhất, do đó tổng số là (O(2^{n-1}mn)). Ngay cả trước khi đạt đến giá trị lớn của (n), điều này trở nên vô ích. 

Một hướng tốt hơn là ngừng suy nghĩ về các định hướng hoàn chỉnh và thay vào đó hãy hỏi từng cạnh riêng lẻ phải làm gì. Gốc cây tại phòng (1). Mọi đỉnh không phải gốc (v) đều có một cạnh giữa (v) và đỉnh gốc của nó (p(v)). Một yêu cầu có thể vượt qua ranh giới này theo đúng một trong hai cách. Nếu nó đi từ cây con của (v) về phía cây cha thì cạnh phải là (v\to p(v)). Nếu nó đi từ phía cha vào cây con thì cạnh phải là (p(v)\to v). 

Đối với một yêu cầu (s\to f), hãy để (l) là tổ tiên chung thấp nhất của (s) và (f). Đường đi chia thành phần đi lên từ (s) đến (l), theo sau là phần đi xuống từ (l) đến (f). Đây là quan sát cấu trúc quan trọng. Chúng ta có thể ghi lại tất cả các yêu cầu hướng lên bằng một mảng cây khác biệt và tất cả các yêu cầu hướng xuống bằng một mảng khác. 

Đối với phần đi lên (s\to l), cộng (1) tại (s) và trừ (1) tại (l). Sau khi tính tổng các giá trị từ con tới cha mẹ, một cạnh ((p(v),v)) nhận được số đếm dương tăng lên chính xác khi một số yêu cầu yêu cầu (v\to p(v)). 

Đối với phần hướng xuống (l\to f), cộng (1) tại (f) và trừ (1) tại (l). Việc tích lũy từ dưới lên tương tự sẽ đưa ra số đếm dương đi xuống chính xác khi một số yêu cầu yêu cầu (p(v)\to v). 

Do đó, một cạnh là không thể chính xác khi cả hai số đếm đều dương. Nếu chỉ có số đếm hướng lên là dương, hãy hướng cạnh lên trên. Nếu chỉ số đếm đi xuống là dương, hãy hướng nó xuống dưới. Nếu cả hai số đều không dương thì cạnh không bị hạn chế và có thể được định hướng tùy ý. 

Vấn đề còn lại là tìm kiếm tất cả LCA một cách hiệu quả. Vì tất cả các yêu cầu đều được biết trước khi bắt đầu xử lý nên chúng tôi có thể sử dụng thuật toán LCA ngoại tuyến của Tarjan. Nó xử lý cây theo thứ tự sau trong khi DSU đại diện cho các cây con đã hoàn thành. Mọi yêu cầu LCA đều được trả lời khi một điểm cuối được xử lý và điểm cuối kia đã được xử lý. Với việc nén đường dẫn và kết hợp theo thứ hạng, việc này gần như mất thời gian tuyến tính. 

Brute-force hoạt động vì nó kiểm tra rõ ràng mọi hướng có thể, nhưng không thành công vì số lượng hướng là theo cấp số nhân. Quan sát cho thấy mọi yêu cầu chỉ áp đặt các hướng độc lập trên từng cạnh của cây cho phép chúng tôi tổng hợp tất cả các yêu cầu có sự khác biệt về cây, trong khi LCA ngoại tuyến cung cấp thông tin cấu trúc duy nhất cần thiết để phân chia từng đường dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^{n-1}mn)) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O((n+m)\alpha(n))) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Căn cây tại đỉnh (1) và tính cha của mỗi đỉnh cùng với thứ tự DFS. Thứ tự ngược lại của thứ tự này là thứ tự hậu hợp lệ, vì vậy nó cho phép chúng ta xử lý mọi phần tử con trước phần tử cha của nó mà không cần đệ quy. 
2. Lưu trữ mọi yêu cầu hai lần trong cấu trúc kề truy vấn. Đối với yêu cầu (i=(s_i,f_i)), đính kèm (f_i) vào (s_i) và (s_i) vào (f_i). Chúng tôi cần cấu trúc này vì thuật toán LCA ngoại tuyến của Tarjan sẽ trả lời yêu cầu khi một trong hai điểm cuối được xử lý. 
3. Chạy thuật toán LCA ngoại tuyến của Tarjan. Mỗi đỉnh ban đầu tạo thành tập DSU của riêng nó. Khi một đỉnh kết thúc, tập hợp của nó sẽ được sáp nhập vào đỉnh cha của nó và đại diện DSU lưu trữ cây tổ tiên hiện tại của tập hợp đó. Khi cả hai điểm cuối của yêu cầu đã được xử lý,`ancestor[find(other)]`là LCA của họ. 
4. Với mọi yêu cầu (s\to f), hãy để (l=\operatorname{LCA}(s,f)). Tăng`up[s]`và giảm dần`up[l]`. Điều này đại diện cho đoạn (s\to l), trong đó mọi cạnh chéo phải hướng về gốc. 
5. Đối với cùng một yêu cầu, hãy tăng`down[f]`và giảm dần`down[l]`. Điều này thể hiện (l\to f), trong đó mọi cạnh chéo phải hướng ra xa gốc. 
6. Duyệt cây theo thứ tự DFS ngược và thêm mọi đỉnh`up`Và`down`giá trị cho cha mẹ của nó. Sau sự tích lũy này, với mọi đỉnh không phải gốc (v),`up[v]`đếm các yêu cầu yêu cầu (v\to parent[v]), trong khi`down[v]`đếm các yêu cầu yêu cầu (parent[v]\to v). 
7. Nếu cả hai`up[v]`Và`down[v]`là dương, đầu ra`NO`. Cần có cùng một hành lang ở cả hai hướng nên không có hướng nào có thể đáp ứng được tất cả các yêu cầu. 
8. Nếu không thì định hướng cạnh giữa (v) và`parent[v]`theo yêu cầu sẵn có. Yêu cầu hướng lên cho (v\to parent[v]), yêu cầu hướng xuống cho (parent[v]\to v) và cạnh không bị ràng buộc có thể sử dụng (parent[v]\to v). 

Tại sao nó hoạt động: đối với mọi yêu cầu (s\to f), đường dẫn cây duy nhất của nó chính xác là sự kết hợp của (s\to l) và (l\to f), trong đó (l) là LCA của chúng. Các bản cập nhật khác biệt đánh dấu mọi cạnh của đoạn đầu tiên là yêu cầu hướng đi lên và mọi cạnh của đoạn thứ hai là yêu cầu hướng đi xuống. Sau khi tích lũy, mỗi cạnh sẽ biết tất cả các hướng được yêu cầu bởi tất cả các yêu cầu. Nếu cả hai hướng xảy ra thì trường hợp này là không thể. Nếu có nhiều nhất một hướng xảy ra, việc chọn hướng đó sẽ đáp ứng mọi yêu cầu sử dụng cạnh. Vì mọi yêu cầu đều bao gồm toàn bộ các cạnh như vậy nên hướng kết quả sẽ đáp ứng tất cả các yêu cầu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    input = sys.stdin.readline
    n, m = map(int, input().split())

    # Compact forward-star representation of the tree.
    head = array('i', [-1]) * n
    to = array('i')
    nxt = array('i')

    eu = array('i')
    ev = array('i')

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        eid = len(to)
        to.append(v)
        nxt.append(head[u])
        head[u] = eid

        eid += 1
        to.append(u)
        nxt.append(head[v])
        head[v] = eid

        eu.append(u)
        ev.append(v)

    # Store requests.
    qs = array('i')
    qf = array('i')

    # Query adjacency for Tarjan's offline LCA.
    qhead = array('i', [-1]) * n
    qto = array('i')
    qnext = array('i')
    qid = array('i')

    for i in range(m):
        s, f = map(int, input().split())
        s -= 1
        f -= 1

        qs.append(s)
        qf.append(f)

        idx = len(qto)
        qto.append(f)
        qid.append(i)
        qnext.append(qhead[s])
        qhead[s] = idx

        idx = len(qto)
        qto.append(s)
        qid.append(i)
        qnext.append(qhead[f])
        qhead[f] = idx

    # Root the tree at 0 and build a DFS order.
    parent = array('i', [-1]) * n
    parent[0] = 0
    order = []

    stack = [0]
    while stack:
        v = stack.pop()
        order.append(v)

        e = head[v]
        while e != -1:
            u = to[e]
            if u != parent[v]:
                parent[u] = v
                stack.append(u)
            e = nxt[e]

    # Tarjan offline LCA.
    dsu = array('i', range(n))
    rank = array('b', [0]) * n
    ancestor = array('i', range(n))
    visited = bytearray(n)
    lca = array('i', [-1]) * m

    def find(x):
        root = x
        while dsu[root] != root:
            root = dsu[root]

        while dsu[x] != x:
            y = dsu[x]
            dsu[x] = root
            x = y

        return root

    for pos in range(n - 1, -1, -1):
        v = order[pos]

        # All child subtrees have already been merged into v.
        rv = find(v)
        ancestor[rv] = v
        visited[v] = 1

        # Answer queries whose other endpoint is already processed.
        e = qhead[v]
        while e != -1:
            other = qto[e]
            idx = qid[e]

            if visited[other] and lca[idx] == -1:
                lca[idx] = ancestor[find(other)]

            e = qnext[e]

        # Merge v into its parent after processing queries at v.
        if v != 0:
            p = parent[v]
            rv = find(v)
            rp = find(p)

            if rv != rp:
                if rank[rv] < rank[rp]:
                    dsu[rv] = rp
                    ancestor[rp] = p
                elif rank[rv] > rank[rp]:
                    dsu[rp] = rv
                    ancestor[rv] = p
                else:
                    dsu[rp] = rv
                    rank[rv] += 1
                    ancestor[rv] = p

    # We no longer need the query graph or DSU.
    del qhead, qto, qnext, qid
    del dsu, rank, ancestor, visited

    # Difference arrays for upward and downward requirements.
    up = array('i', [0]) * n
    down = array('i', [0]) * n

    for i in range(m):
        s = qs[i]
        f = qf[i]
        l = lca[i]

        up[s] += 1
        up[l] -= 1

        down[f] += 1
        down[l] -= 1

    del qs, qf, lca

    # Accumulate subtree differences from children to parents.
    possible = True

    for pos in range(n - 1, 0, -1):
        v = order[pos]

        if up[v] > 0 and down[v] > 0:
            possible = False
            break

        p = parent[v]
        up[p] += up[v]
        down[p] += down[v]

    if not possible:
        print("NO")
        return

    # Orient every original edge.
    answer = ["YES"]

    for i in range(n - 1):
        a = eu[i]
        b = ev[i]

        if parent[a] == b:
            child = a
            par = b
        else:
            child = b
            par = a

        if up[child] > 0:
            answer.append(f"{child + 1} {par + 1}")
        else:
            # This covers both down[child] > 0 and the unconstrained case.
            answer.append(f"{par + 1} {child + 1}")

    sys.stdout.write("\n".join(answer))

if __name__ == "__main__":
    solve()
```Cây được lưu trữ dưới dạng biểu diễn ngôi sao chuyển tiếp nhỏ gọn thay vì danh sách danh sách Python. Tại các đỉnh (2\cdot10^5), điều này giúp dự đoán được dung lượng bộ nhớ. Các điểm cuối ban đầu cũng được giữ lại vì đầu ra được yêu cầu có thể liệt kê các cạnh theo bất kỳ thứ tự nào, nhưng hướng phải được xây dựng lại cho mỗi cạnh ban đầu. 

DFS đầu tiên chỉ thiết lập`parent`Và`order`. Vì đồ thị được đảm bảo là cây nên kiểm tra`u != parent[v]`là đủ để tránh đi ngược về phía cha mẹ. Không sử dụng DFS đệ quy vì cây có thể là một chuỗi gồm (2\cdot10^5) đỉnh và sẽ vượt quá giới hạn đệ quy của Python. 

Phần của Tarjan sử dụng mảng cha DSU riêng biệt. Điều này cố tình khác với cái cây`parent`mảng. Cây gốc mô tả cây có gốc thực tế, trong khi cây gốc DSU mô tả tập hợp tạm thời các cây con đã được xử lý. các`ancestor`mảng kết nối một đại diện DSU trở lại đỉnh cây hiện đóng vai trò là tổ tiên của tập hợp đó. 

Thứ tự bên trong vòng lặp Tarjan rất quan trọng. Một đỉnh được đánh dấu là đã xử lý và các truy vấn của nó được trả lời trước khi nó được hợp nhất vào đỉnh chính của nó. Nếu việc hợp nhất xảy ra trước tiên, một truy vấn có LCA là đỉnh hiện tại có thể quan sát đỉnh cao hơn và nhận được câu trả lời sai. 

Hai mảng khác nhau sử dụng số nguyên 32 bit có dấu. Mọi giá trị đều được giới hạn bởi số lượng yêu cầu, vì vậy nó phù hợp thoải mái trong phạm vi này. Bản thân Python cũng có các số nguyên có độ chính xác tùy ý, nhưng các mảng nhỏ gọn giúp giảm đáng kể mức tiêu thụ bộ nhớ. 

Hướng cuối cùng sử dụng điểm cuối con của mọi cạnh có gốc.`up[child] > 0`có nghĩa là ít nhất một yêu cầu yêu cầu cạnh phải trỏ từ con về phía cha mẹ của nó. Nếu không đúng như vậy, cạnh có thể trỏ từ cha mẹ sang con một cách an toàn vì yêu cầu đi xuống yêu cầu điều đó hoặc không ai quan tâm đến điều đó. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, lấy gốc cây ở đỉnh (1). Các cạnh gốc là (1-2), (1-4), (4-3) và (3-5). LCA và hai đoạn đường dẫn là: 

| Yêu cầu | LCA | Phân khúc đi lên | Đoạn đi xuống | 
| --- | --- | --- | --- | 
| (1\to2) | 1 | trống | (1\to2) | 
| (5\to3) | 3 | (5\to3) | trống | 
| (5\to4) | 4 | (5\to3\to4) | trống | 
| (1\to4) | 1 | trống | (1\to4) | 
| (3\to4) | 4 | trống | trống so với phần chia gốc | 

Yêu cầu cuối cùng thực sự có (4) là LCA vì (4) là nguồn gốc của (3), do đó phân đoạn đi lên của nó là (3\to4). Sau khi tích lũy, các cạnh bị ràng buộc có các hướng sau: 

| Cạnh | Đếm lên | Đếm ngược | Hướng đi đã chọn | 
| --- | --- | --- | --- | 
| (1-2) | 0 | 1 | (1\to2) | 
| (1-4) | 0 | 1 | (1\to4) | 
| (4-3) | 2 | 0 | (3\to4) | 
| (3-5) | 2 | 0 | (5\to3) | 

Không có cạnh nào có số đếm dương theo cả hai hướng, vì vậy trường hợp này là khả thi. Đầu ra được hiển thị trong câu lệnh là một hướng hợp lệ và thuật toán có thể tạo ra một hướng khác vì các cạnh không bị ràng buộc có thể được định hướng tùy ý. 

Đối với Mẫu 2, trạng thái trung gian quan trọng là tập hợp LCA: 

| Yêu cầu | LCA | Cập nhật lên | Cập nhật xuống | 
| --- | --- | --- | --- | 
| (6\to10) | 1 | (lên[6]++, lên[1]--) | (xuống[10]++, xuống[1]--) | 
| (13\to1) | 1 | (lên[13]++, lên[1]--) | không thay đổi ròng | 
| (5\to14) | 1 | (lên[5]++, lên[1]--) | (xuống[14]++, xuống[1]--) | 
| (15\to12) | 12 | (lên[15]++, lên[12]--) | không thay đổi ròng | 
| (2\to8) | 2 | không thay đổi ròng | (xuống[8]++, xuống[2]--) | 

Sau khi tích lũy từ dưới lên, mỗi cạnh có nhiều nhất một hướng dương. Ví dụ: cạnh (1-2) nhận được yêu cầu hướng lên từ (6\to10), do đó nó phải là (2\to1). Cạnh (1-3) nhận được yêu cầu giảm dần từ (6\to10), do đó nó phải là (1\to3). Cạnh (2-8) nhận được yêu cầu giảm dần từ (2\to8), do đó nó phải là (2\to8). 

Các hướng dẫn kết quả bao gồm các đường dẫn```
6 -> 2 -> 1 -> 3 -> 10
13 -> 11 -> 4 -> 1
5 -> 1 -> 3 -> 9 -> 12 -> 14
15 -> 12
2 -> 8
```điều này thể hiện tính bất biến trung tâm: mọi đường dẫn được yêu cầu được tập hợp hoàn toàn từ các cạnh có hướng được cố định độc lập bởi số lượng chênh lệch tương ứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+m)\alpha(n))) | Truyền tải cây, hoạt động DSU của Tarjan và tích lũy chênh lệch đều gần như tuyến tính | 
| Không gian | (O(n+m)) | Cây, danh sách truy vấn, trạng thái DSU, yêu cầu và hai mảng khác nhau được lưu trữ | 

Đầu vào lớn nhất có (2\cdot10^5) đỉnh và (2\cdot10^5) yêu cầu. Thuật toán chỉ thực hiện một số lần chuyển qua cây và các yêu cầu không đổi, với các hoạt động DSU có chi phí khấu hao nghịch đảo với Ackermann. Điều này phù hợp với các giới hạn tiệm cận cần thiết và tránh cả việc liệt kê theo cấp số nhân và việc truyền tải rõ ràng mọi đường dẫn được yêu cầu. 

## Trường hợp thử nghiệm 

Đầu ra của vấn đề này không phải là duy nhất, vì vậy các thử nghiệm không nên so sánh hướng thành công với một chuỗi cố định. Bộ khai thác thử nghiệm bên dưới sẽ kiểm tra xem`NO`được báo cáo khi cần thiết và, đối với`YES`, xác minh rằng mọi cạnh được tạo ra là cạnh gốc hợp lệ và mọi tuyến đường được yêu cầu thực sự được chuyển hướng từ nguồn đến đích của nó. 

Đối với thử nghiệm lớn, việc kiểm tra mọi yêu cầu bằng tìm kiếm biểu đồ đầy đủ sẽ tốn kém một cách không cần thiết, vì vậy trường hợp đó sẽ kiểm tra các thuộc tính cấu trúc của đầu ra.```python
# Save the editorial solution as solution.py before running these tests.

import sys
import io
from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate_orientation(inp: str, out: str) -> bool:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    m = next(it)

    edges = set()
    for _ in range(n - 1):
        u = next(it)
        v = next(it)
        edges.add((u, v))
        edges.add((v, u))

    queries = []
    for _ in range(m):
        s = next(it)
        f = next(it)
        queries.append((s, f))

    lines = out.splitlines()
    if not lines:
        return False

    if lines[0] == "NO":
        return False

    if lines[0] != "YES":
        return False

    if len(lines) != n:
        return False

    directed = [[] for _ in range(n + 1)]

    for line in lines[1:]:
        a, b = map(int, line.split())
        if (a, b) not in edges:
            return False
        directed[a].append(b)

    # This validator is intended for small tests.
    for s, f in queries:
        seen = [False] * (n + 1)
        stack = [s]
        seen[s] = True

        while stack:
            v = stack.pop()
            if v == f:
                break

            for u in directed[v]:
                if not seen[u]:
                    seen[u] = True
                    stack.append(u)

        if not seen[f]:
            return False

    return True

# Sample 1
sample1 = """\
5 5
2 1
4 1
5 3
3 4
1 2
5 3
5 4
1 4
3 4
"""

out = run(sample1)
assert validate_orientation(sample1, out), "sample 1"

# Sample 2
sample2 = """\
15 5
1 2
1 3
1 4
1 5
2 6
2 7
2 8
3 9
3 10
4 11
9 12
11 13
12 14
12 15
6 10
13 1
5 14
15 12
2 8
"""

out = run(sample2)
assert validate_orientation(sample2, out), "sample 2"

# Sample 3
sample3 = """\
5 5
1 3
5 1
4 2
3 4
4 3
4 3
3 2
1 2
5 4
"""

assert run(sample3) == "NO", "sample 3"

# Minimum-size tree, equal endpoints, no edges to orient.
case_min = """\
1 1
1 1
"""

out = run(case_min)
assert out == "YES", "minimum-size case"

# Two opposite requirements on the only edge.
case_conflict = """\
2 2
1 2
1 2
2 1
"""

assert run(case_conflict) == "NO", "opposite directions"

# A request from a deep leaf to the root.
case_reverse_chain = """\
3 1
1 2
2 3
3 1
"""

out = run(case_reverse_chain)
assert validate_orientation(case_reverse_chain, out), "reverse chain"

# All requests have equal endpoints, so every edge is unconstrained.
case_equal = """\
4 4
1 2
2 3
3 4
2 2
2 2
2 2
2 2
"""

out = run(case_equal)
assert validate_orientation(case_equal, out), "equal endpoints"

# Maximum-size stress shape: a chain and many identical requests.
n = 200000
m = 200000

parts = [f"{n} {m}"]
for v in range(1, n):
    parts.append(f"{v} {v + 1}")
for _ in range(m):
    parts.append(f"1 {n}")

large_case = "\n".join(parts) + "\n"
out = run(large_case)

large_lines = out.splitlines()
assert large_lines[0] == "YES", "maximum-size case must be feasible"
assert len(large_lines) == n, "wrong number of output edges"

print("all tests passed")
```Trường hợp tùy chỉnh đầu tiên xác thực ranh giới (n=1), trong đó câu trả lời chỉ chứa`YES`và không có mô tả cạnh. Cách thứ hai tạo ra mâu thuẫn nhỏ nhất có thể và nắm bắt các triển khai chỉ nhớ một hướng trên mỗi cạnh. 

Trường hợp chuỗi đảo ngược nắm bắt các giả định không chính xác về hướng gốc. Trường hợp điểm cuối bằng nhau xác nhận rằng một yêu cầu (s\to s) không áp đặt bất kỳ ràng buộc biên nào cả. Chuỗi kích thước tối đa nhấn mạnh đến việc truyền tải lặp đi lặp lại, lưu trữ nhỏ gọn, xử lý LCA ngoại tuyến và xây dựng đầu ra ở giới hạn trên thực tế. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`YES`| Cây có kích thước tối thiểu và đường dẫn trống | 
|`2 2 / 1 2 / 1 2 / 2 1`|`NO`| Mâu thuẫn trực tiếp ở một bên | 
|`3 1 / 1 2 / 2 3 / 3 1`|`YES`với (3\to2\to1) | Hướng từ lá đến gốc và ranh giới đường dẫn | 
|`4 4 / 1 2 / 2 3 / 3 4 / 2 2 ...`|`YES`| Tất cả các yêu cầu đều có đường dẫn có độ dài bằng 0 | 
| (n=m=200000), chuỗi có yêu cầu (1\to n) |`YES`với tất cả các cạnh (v\to v+1) | Kích thước đầu vào tối đa và triển khai lặp lại | 

## Vỏ cạnh 

Đối với trường hợp điểm cuối bằng nhau```
1 1
1 1
```LCA cũng là đỉnh (1). Cả hai bản cập nhật khác biệt đều bị hủy ngay lập tức:`up[1] += 1`theo sau là`up[1] -= 1`, và điều tương tự cũng xảy ra với`down`. Không có cạnh nào để kiểm tra nên thuật toán sẽ in`YES`. 

Vì mâu thuẫn trực tiếp```
2 2
1 2
1 2
2 1
```đỉnh gốc (1). Yêu cầu đầu tiên tạo ra yêu cầu hướng xuống trên cạnh (1-2), trong khi yêu cầu thứ hai tạo ra yêu cầu hướng lên trên cùng một cạnh. Sau khi tích lũy,`down[2] = 1`Và`up[2] = 1`. Điều kiện xung đột xảy ra và thuật toán in`NO`. 

Đối với chuỗi đảo ngược```
3 1
1 2
2 3
3 1
```LCA của (3) và (1) là (1). Các cập nhật khác biệt trở lên là`up[3] += 1`Và`up[1] -= 1`. Tích lũy từ dưới lên chuyển giá trị từ đỉnh (3) sang đỉnh (2), rồi đến đỉnh (1). Do đó, cả hai cạnh đều có số đếm dương đi lên và không có số đếm đi xuống. Chúng được định hướng (3\to2) và (2\to1), khớp chính xác với đường dẫn được yêu cầu. 

Đối với tình huống không bị ràng buộc```
4 4
1 2
2 3
3 4
2 2
2 2
2 2
2 2
```mọi yêu cầu đều có điểm cuối giống hệt nhau. Mọi LCA đều bằng điểm cuối đó, vì vậy mọi cập nhật sai phân đều bị hủy ở cùng một đỉnh. Tất cả các cạnh đều có số lượng bằng 0 theo cả hai hướng. Thuật toán chọn hướng mặc định từ cha đến con, tạo ra (1\to2), (2\to3), (3\to4), hợp lệ vì không có yêu cầu nào cần đi qua một cạnh.
