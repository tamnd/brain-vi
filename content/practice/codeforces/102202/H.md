---
title: "CF 102202H - Thầy Cô Ghen Tuông"
description: "Hãy nghĩ về đầu vào như một biểu đồ lưỡng cực. Bên trái là (N-1) học sinh vẫn đang đi học, bên phải là (N) giáo viên. Cạnh ((s,t)) có nghĩa là (các) học sinh được phép tặng hoa cho giáo viên (t)."
date: "2026-08-18T01:19:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "H"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 390
verified: true
draft: false
---

[CF 102202H - Giáo viên ghen tị](https://codeforces.com/problemset/problem/102202/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hãy nghĩ về đầu vào như một biểu đồ lưỡng cực. Bên trái là (N-1) học sinh vẫn đang đi học, bên phải là (N) giáo viên. Cạnh ((s,t)) có nghĩa là (các) học sinh được phép tặng hoa cho giáo viên (t). Mỗi học sinh có đúng (N) bông hoa, trong khi mỗi giáo viên phải nhận đúng (N-1) bông hoa. 

Đối với mỗi cạnh nhất định, chúng ta phải xuất ra một số nguyên không âm biểu thị số lượng bông hoa đi qua cạnh đó. Tổng các giá trị trên các cạnh liên quan của mỗi học sinh phải là (N) và tổng các giá trị trên các cạnh liên quan của mỗi giáo viên phải là (N-1). Các cạnh tồn tại trong biểu đồ nhưng không nhận được hoa nào là hoàn toàn hợp lệ. 

Tổng nguồn cung là ((N-1)N) và tổng nhu cầu là (N(N-1)), do đó tổng số toàn cầu tự động đồng ý. Khó khăn hoàn toàn là do thiếu các cạnh. Đây là bài toán vận chuyển trên đồ thị lưỡng cực thưa thớt. 

Các ràng buộc đủ lớn để loại trừ luồng cực đại chung. Có thể có (10^5) học sinh và (2\cdot10^5) cặp được phép, do đó thuật toán (O(NM)) hoặc tệ hơn đã quá lớn. Giải pháp dự định giảm phần đắt tiền thành đối sánh hai bên, có thể được thực hiện trong (O(M\sqrt N)), sau đó chỉ xử lý biểu đồ tuyến tính. Giải pháp cuộc thi ban đầu cũng sử dụng cấu trúc dựa trên so khớp này thay vì thuật toán luồng chung. 

Có một số trường hợp khó khăn khi việc xây dựng có vẻ hợp lý lại thất bại. Một giáo viên bị cô lập ngay lập tức đưa ra câu trả lời không thể. Ví dụ,```
2 1
1 1
```có giáo viên 2 không có cạnh nào có thể vào được, vì vậy đầu ra đúng là`-1`. Việc thực hiện bất cẩn chỉ kiểm tra xem mỗi học sinh có giáo viên sẵn sàng hay không có thể bỏ lỡ điều này. 

Một học sinh bị cô lập cũng nguy hiểm không kém. Ví dụ,```
3 1
1 1
```khiến học sinh 2 không có nơi nào để gửi ba bông hoa của mình, vì vậy kết quả đúng là`-1`. 

Sự phù hợp là cần thiết nhưng chưa đủ. Ví dụ: mẫu thứ hai có kích thước phù hợp là 5, nhưng biểu đồ chia thành một thành phần chứa học sinh 5 và giáo viên 5 và 6, trong khi các học sinh khác không thể tương tác với những giáo viên đó. Đầu ra đúng là`-1`. Một giải pháp dừng ngay lập tức sau khi tìm thấy kích thước phù hợp (N-1) sẽ âm thầm chấp nhận một phiên bản không hợp lệ. 

Cuối cùng, các cạnh không có luồng phải được cho phép. Trong mẫu đầu tiên, nhiều cặp đầu vào không nhận được hoa nào trong một giải pháp hợp lệ. Việc xử lý mọi cạnh đầu vào như thể nó phải mang luồng dương sẽ làm thay đổi vấn đề và có thể loại bỏ các biểu đồ hợp lệ một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô hình hóa vấn đề dưới dạng luồng cực đại. Thêm một nguồn được kết nối với mọi học sinh có dung lượng (N), kết nối mọi cặp học sinh-giáo viên được phép với dung lượng vô hạn và kết nối mọi giáo viên với bồn chứa có dung lượng (N-1). Luồng (N(N-1)) chính xác là phép gán mong muốn. 

Mô hình này đúng, nhưng việc triển khai Ford-Fulkerson có chủ ý ngây thơ nhằm tăng cường từng bông hoa tại một thời điểm có thể thực hiện các phép tăng (N(N-1)). Mỗi lần tăng cường có thể kiểm tra các cạnh của đồ thị (O(M+N)), đưa ra khoảng 

[ 
N(N-1)(M+2N) 
] 

kiểm tra cạnh. Ở giới hạn tối đa, con số này ở mức (4\cdot10^{15}), vượt xa giới hạn thời gian. Giải pháp chính thức mô tả công thức luồng tối đa này cho nhiệm vụ con nhỏ và đưa ra (O(N^4)) ở đó. 

Quan sát hữu ích xuất phát từ việc xem xét một phần cắt có chứa một số tập hợp con (S) học sinh. Những học sinh đó sở hữu chung những bông hoa (N|S|). Chỉ những giáo viên lân cận mới có thể nhận được chúng và mỗi giáo viên như vậy có thể nhận được tối đa (N-1) bông hoa. Do đó mọi tập con sinh viên khác rỗng phải có ít nhất 

[ 
N|S|\le (N-1)|N(S)| 
] 

giáo viên lân cận. Vì (1\le |S|\le N-1), bất đẳng thức này tương đương với 

[ 
|N(S)|\ge |S|+1. 
] 

Người thầy bổ sung đó chính là cấu trúc mấu chốt của vấn đề. Nó mạnh hơn tình trạng của Hall bình thường. 

Đầu tiên hãy tìm một kết quả phù hợp bao gồm tất cả (N-1) học sinh. Chính xác là một giáo viên vẫn chưa từng có. Gọi thầy đó là gốc. Bây giờ hãy định hướng biểu đồ lưỡng cực theo một cách đặc biệt. Mọi cạnh phù hợp đều được hướng từ học sinh đến giáo viên, trong khi mọi cạnh không khớp được hướng từ giáo viên đến học sinh. 

Bắt đầu từ giáo viên chưa từng có, thực hiện tìm kiếm bằng cách sử dụng các cạnh được định hướng này. Nếu đạt được mọi học sinh, các cạnh tìm kiếm sẽ tạo thành một cây kết nối tất cả (N) giáo viên. Nếu không thể liên lạc được với một số học sinh, điều kiện Hall mạnh hơn được yêu cầu sẽ không thành công, do đó không có bài tập hoa nào tồn tại. 

Việc xây dựng còn lại đơn giản đến mức đáng ngạc nhiên. Trong cây này, mỗi học sinh có chính xác một giáo viên con, vì cạnh khớp của nó hướng từ học sinh đến giáo viên đó. Mỗi giáo viên ngoại trừ gốc có chính xác một học sinh cha. Khi chúng ta biết số lượng cây con trong mỗi cây con, số lượng hoa trên mỗi cạnh cây sẽ trực tiếp được rút ra từ quá trình bảo tồn. 

Mô hình dòng chảy bạo lực hoạt động vì nó thể hiện chính xác cung và cầu cần thiết. Nó thất bại vì tổng luồng là bậc hai tính theo (N). Quan sát thấy rằng các khả năng khác nhau đúng một sẽ chuyển câu hỏi về tính khả thi thành một bài toán khớp cộng với cây xen kẽ, sau đó có thể thu được các giá trị luồng thực tế từ các kích thước cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N(N-1)(M+N))) để tăng đơn vị | (O(M+N)) | Quá chậm | 
| Tối ưu | (O(M\sqrt N)) | (O(M+N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ hai bên với học sinh ở bên trái và giáo viên ở bên phải. Lưu trữ chỉ số cạnh đầu vào ban đầu cùng với mọi mục nhập kề nhau, vì câu trả lời cuối cùng phải được in chính xác theo thứ tự đầu vào. 
2. Tìm sự phù hợp lưỡng cực tối đa. Chúng tôi cần tất cả (N-1) học sinh được ghép nối. Nếu phần khớp chứa ít hơn (N-1) cạnh, hãy in ngay`-1`. 

Việc so khớp là cần thiết vì bất kỳ phép gán hoa hợp lệ nào đều thỏa mãn điều kiện Hall mạnh hơn và điều kiện đó đặc biệt bao hàm điều kiện Hall thông thường. 

1. Tìm giáo viên duy nhất chưa từng có và sử dụng nó làm gốc của tìm kiếm xen kẽ. 
2. Hướng mọi cạnh phù hợp từ học sinh của mình đến giáo viên phù hợp. Hướng mọi cạnh không phù hợp từ giáo viên đến học sinh. 

Bắt đầu từ người thầy chưa từng có, hãy làm theo những hướng dẫn sau. Bất cứ khi nào giáo viên tiếp cận một học sinh chưa được nhìn thấy trước đó, hãy thêm học sinh đó vào cây. Ngay lập tức theo dõi cạnh phù hợp của học sinh với giáo viên phù hợp và thêm giáo viên đó. 

1. Nếu tìm kiếm tiếp cận ít hơn (N-1) sinh viên, hãy in`-1`. 

Giả sử việc tìm kiếm đạt đến một tập hợp (k) sinh viên. Nó cũng tiếp cận chính xác (k+1) giáo viên, bởi vì giáo viên gốc ban đầu có mặt và mỗi học sinh mới tiếp cận đều giới thiệu giáo viên phù hợp duy nhất của mình. Nếu biểu đồ có một phép gán hợp lệ, điều kiện Hall mạnh hơn đảm bảo rằng mọi học sinh đều có thể được tiếp cận bởi giáo viên chưa từng có trong biểu đồ xen kẽ này. Một cách để thấy điều này là so sánh kết quả phù hợp hiện tại với kết quả phù hợp để tránh giáo viên phù hợp của học sinh hiện tại. Sự khác biệt đối xứng của chúng chứa một đường đi xen kẽ từ gốc chưa khớp đến học sinh đó. 

1. Coi các cạnh tìm kiếm như một cây có gốc thông thường. Đặt cho mỗi nút sinh viên trọng số cây con ban đầu là (1) và mỗi nút giáo viên có trọng số cây con ban đầu là (0). Xử lý cây theo thứ tự ngược lại để mỗi nút nhận được tổng số sinh viên trong cây con của nó. 
2. Đối với cạnh cây có con là học sinh, hãy gán 

[ 
f = \operatorname{size}(s). 
] 

Cạnh kết nối học sinh đó với giáo viên mẹ của nó, vì vậy (f) chính xác là số bông hoa mà toàn bộ cây con học sinh phải gửi lên trên. 

1. Với cạnh cây có con là giáo viên (t), gán 

[ 
f = N-1-\operatorname{size}(t). 
] 

Tương tự, nếu học sinh cha của nó là (s), 

[ 
f=N-\operatorname{size}(s). 
] 

Tất cả các cạnh đầu vào không phải là cạnh cây sẽ không có hoa. 

1. Xuất luồng được gán cho mỗi cạnh gốc theo thứ tự đầu vào. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi cây con sinh viên chứa số lượng học sinh và giáo viên bằng nhau, trong khi mọi cây con giáo viên không gốc đều chứa đúng một giáo viên nhiều hơn học sinh. Hãy xem xét một (các) sinh viên. Cạnh cha của nó mang (\operatorname{size}(s)) hoa và cạnh giáo viên con của nó mang (N-\operatorname{size}(s)) hoa. Tổng của chúng chính xác là (N), vì vậy mỗi học sinh sẽ gửi tất cả số hoa của mình. 

Bây giờ hãy xem xét một giáo viên không phải gốc (t). Cho cây con của nó chứa (k) sinh viên. Học sinh cha của nó gửi (N-1-k) hoa vào (t), trong khi cây con của học sinh con gửi chung hoa (k). Tổng số là (N-1). Gốc không có cạnh cha và các cây con học sinh con của nó chứa tất cả (N-1) học sinh, do đó nó cũng nhận được chính xác (N-1) hoa. 

Mọi giá trị được gán đều không âm vì một cây con sinh viên chứa tối đa (N-1) sinh viên. Tất cả các cạnh không phải cây đều mang số 0, do đó mọi hạn chế của đồ thị gốc đều được tôn trọng. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def hopcroft_karp(adj, n_left, n_right):
    pair_u = [-1] * n_left
    pair_v = [-1] * n_right
    dist = [-1] * n_left

    def bfs():
        q = deque()

        for u in range(n_left):
            if pair_u[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = -1

        found = False

        while q:
            u = q.popleft()

            for v in adj[u]:
                w = pair_v[v]

                if w == -1:
                    found = True
                elif dist[w] == -1:
                    dist[w] = dist[u] + 1
                    q.append(w)

        return found

    def dfs(start):
        stack = [start]
        chosen = []
        it = {}

        while stack:
            u = stack[-1]

            if u not in it:
                it[u] = 0

            i = it[u]

            while i < len(adj[u]):
                v = adj[u][i]
                i += 1
                it[u] = i

                w = pair_v[v]

                if w == -1:
                    pair_u[u] = v
                    pair_v[v] = u

                    for j in range(len(chosen) - 1, -1, -1):
                        left = stack[j]
                        right = chosen[j]
                        pair_u[left] = right
                        pair_v[right] = left

                    return True

                if dist[w] == dist[u] + 1:
                    stack.append(w)
                    chosen.append(v)
                    break
            else:
                dist[u] = -1
                stack.pop()
                if chosen:
                    chosen.pop()

        return False

    matching = 0

    while bfs():
        for u in range(n_left):
            if pair_u[u] == -1 and dfs(u):
                matching += 1

    return matching, pair_u, pair_v

def solve():
    n, m = map(int, input().split())
    left = n - 1

    adj = [[] for _ in range(left)]
    edges = []

    for idx in range(m):
        s, t = map(int, input().split())
        s -= 1
        t -= 1
        adj[s].append((t, idx))
        edges.append((s, t))

    # The matching algorithm only needs the teacher number.
    match_adj = [[t for t, _ in adj[s]] for s in range(left)]

    matching, pair_u, pair_v = hopcroft_karp(
        match_adj, left, n
    )

    if matching != left:
        print(-1)
        return

    # Find the original edge corresponding to every matching pair.
    match_edge = [-1] * left
    for s in range(left):
        mt = pair_u[s]
        for t, idx in adj[s]:
            if t == mt:
                match_edge[s] = idx
                break

    root = -1
    for t in range(n):
        if pair_v[t] == -1:
            root = t
            break

    # Nodes 0..n-1 are teachers.
    # Nodes n..n+left-1 are students.
    total_nodes = n + left
    parent = [-1] * total_nodes
    parent_edge = [-1] * total_nodes

    root_node = root
    parent[root_node] = root_node

    visited_students = [False] * left
    teacher_stack = [root_node]
    order = [root_node]

    while teacher_stack:
        tnode = teacher_stack.pop()
        t = tnode

        for s, idx in []:
            pass

        # Every adjacency entry is stored as (teacher, edge_index),
        # so scan all students that are adjacent to this teacher.
        # The graph is stored from students to teachers, therefore
        # build this reverse adjacency lazily once below.
        # This branch is intentionally replaced after reverse_adj exists.
        break

    # Reverse adjacency: teacher -> (student, original edge).
    reverse_adj = [[] for _ in range(n)]
    for s in range(left):
        for t, idx in adj[s]:
            reverse_adj[t].append((s, idx))

    # Restart the alternating-tree search.
    parent = [-1] * total_nodes
    parent_edge = [-1] * total_nodes
    parent[root_node] = root_node

    visited_students = [False] * left
    teacher_stack = [root_node]
    order = [root_node]

    while teacher_stack:
        t = teacher_stack.pop()

        for s, idx in reverse_adj[t]:
            # Matching edges point student -> teacher, so they cannot
            # be followed from a teacher.
            if pair_u[s] == t:
                continue

            if visited_students[s]:
                continue

            visited_students[s] = True

            snode = n + s
            parent[snode] = t
            parent_edge[snode] = idx
            order.append(snode)

            mt = pair_u[s]
            tchild = mt

            # A newly reached student's matched teacher is new as well.
            if parent[tchild] == -1:
                parent[tchild] = snode
                parent_edge[tchild] = match_edge[s]
                order.append(tchild)
                teacher_stack.append(tchild)

    if sum(visited_students) != left:
        print(-1)
        return

    # Count students in every subtree.
    size = [0] * total_nodes

    for node in range(n, total_nodes):
        size[node] = 1

    for node in reversed(order[1:]):
        size[parent[node]] += size[node]

    answer = [0] * m

    # Assign the flow on every tree edge.
    for node in order[1:]:
        idx = parent_edge[node]

        if node >= n:
            # Child is a student.
            answer[idx] = size[node]
        else:
            # Child is a teacher.
            answer[idx] = n - 1 - size[node]

    sys.stdout.write("\n".join(map(str, answer)))

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ hai lần về mặt khái niệm.`adj`giữ sự liền kề giữa học sinh và giáo viên ban đầu cùng với các chỉ số cạnh, trong khi`reverse_adj`được tạo sau khi so khớp để việc tìm kiếm xen kẽ có thể di chuyển từ giáo viên sang học sinh lân cận một cách hiệu quả. 

Sự phù hợp chỉ được tính toán từ số lượng giáo viên. Sau khi xác định được kết quả khớp, cạnh khớp của mỗi học sinh sẽ được phục hồi bằng cách quét danh sách kề của học sinh đó một lần. Vì tổng số cặp đầu vào là (M) nên chi phí phục hồi này (O(M)). 

Cây xen kẽ sử dụng`parent`để đánh dấu các nút đã truy cập. Các nút giáo viên chiếm các chỉ số`0`bởi vì`N-1`, trong khi (các) nút sinh viên chiếm`N+s`. Điều này tránh việc phân bổ các đối tượng đồ thị riêng biệt cho cây và làm cho việc tính toán cây con trở nên đơn giản. 

Vòng khám phá đầu tiên trước`reverse_adj`là không cần thiết và không nên có trong quá trình triển khai bóng bẩy. Phiên bản đã được làm sạch sau đây là phiên bản cần gửi:```python
import sys
from collections import deque

input = sys.stdin.readline

def hopcroft_karp(adj, n_left, n_right):
    pair_u = [-1] * n_left
    pair_v = [-1] * n_right
    dist = [-1] * n_left

    def bfs():
        q = deque()

        for u in range(n_left):
            if pair_u[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = -1

        found = False

        while q:
            u = q.popleft()

            for v in adj[u]:
                w = pair_v[v]
                if w == -1:
                    found = True
                elif dist[w] == -1:
                    dist[w] = dist[u] + 1
                    q.append(w)

        return found

    def dfs(start):
        stack = [start]
        chosen = []
        it = [0] * n_left

        while stack:
            u = stack[-1]

            while it[u] < len(adj[u]):
                v = adj[u][it[u]]
                it[u] += 1

                w = pair_v[v]

                if w == -1:
                    pair_u[u] = v
                    pair_v[v] = u

                    for j in range(len(chosen) - 1, -1, -1):
                        left = stack[j]
                        right = chosen[j]
                        pair_u[left] = right
                        pair_v[right] = left

                    return True

                if dist[w] == dist[u] + 1:
                    stack.append(w)
                    chosen.append(v)
                    break
            else:
                dist[u] = -1
                stack.pop()
                if chosen:
                    chosen.pop()

        return False

    matching = 0

    while bfs():
        for u in range(n_left):
            if pair_u[u] == -1 and dfs(u):
                matching += 1

    return matching, pair_u, pair_v

def solve():
    n, m = map(int, input().split())
    left = n - 1

    adj = [[] for _ in range(left)]
    reverse_adj = [[] for _ in range(n)]
    edges = [None] * m

    for idx in range(m):
        s, t = map(int, input().split())
        s -= 1
        t -= 1

        adj[s].append((t, idx))
        reverse_adj[t].append((s, idx))
        edges[idx] = (s, t)

    match_adj = [[t for t, _ in adj[s]] for s in range(left)]

    matching, pair_u, pair_v = hopcroft_karp(
        match_adj, left, n
    )

    if matching != left:
        print(-1)
        return

    match_edge = [-1] * left
    for s in range(left):
        mt = pair_u[s]
        for t, idx in adj[s]:
            if t == mt:
                match_edge[s] = idx
                break

    root = -1
    for t in range(n):
        if pair_v[t] == -1:
            root = t
            break

    total_nodes = n + left
    parent = [-1] * total_nodes
    parent_edge = [-1] * total_nodes

    parent[root] = root
    visited_student = [False] * left

    stack = [root]
    order = [root]

    while stack:
        t = stack.pop()

        for s, idx in reverse_adj[t]:
            if pair_u[s] == t:
                continue

            if visited_student[s]:
                continue

            visited_student[s] = True

            snode = n + s
            parent[snode] = t
            parent_edge[snode] = idx
            order.append(snode)

            mt = pair_u[s]
            parent[mt] = snode
            parent_edge[mt] = match_edge[s]
            order.append(mt)
            stack.append(mt)

    if not all(visited_student):
        print(-1)
        return

    size = [0] * total_nodes

    for s in range(left):
        size[n + s] = 1

    for node in reversed(order[1:]):
        size[parent[node]] += size[node]

    answer = [0] * m

    for node in order[1:]:
        idx = parent_edge[node]

        if node >= n:
            answer[idx] = size[node]
        else:
            answer[idx] = n - 1 - size[node]

    sys.stdout.write("\n".join(map(str, answer)))

if __name__ == "__main__":
    solve()
```Phiên bản thứ hai loại bỏ phần khởi tạo dư thừa và là phiên bản gửi thực tế. DFS lặp bên trong Hopcroft-Karp tránh được các vấn đề về độ sâu đệ quy của Python, điều này quan trọng vì biểu đồ có thể chứa (10^5) đỉnh ở một bên. 

Không có vấn đề tràn số nguyên trong Python. Giá trị luồng lớn nhất là (N-1), trong khi số lượng cây con nhiều nhất là (N-1). Ranh giới mong manh duy nhất là công thức giáo viên-trẻ em, đó là`n - 1 - size[node]`, không`n - size[node]`. Cây con giáo viên chứa (k) học sinh cần (N-1-k) hoa từ học sinh mẹ của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Sử dụng kết quả khớp hợp lệ sau đây làm dấu vết: 

[ 
s_1\đến t_5,\quad 
s_2\đến t_2,\quad 
s_3\đến t_3,\quad 
s_4\đến t_1,\quad 
s_5\đến t_6. 
] 

Thầy (t_4) vô đối nên trở thành gốc. Cây xen kẽ có hướng là 

[ 
t_4 
\to s_1\to t_5, 
\quad 
t_4\to s_4\to t_1\to s_3\to t_3, 
\quad 
t_4\đến s_5\đến t_6, 
\quad 
t_4\đến s_2\đến t_2. 
] 

| Bước | Giáo viên hiện tại | Sinh viên mới tiếp cận | Giáo viên phù hợp | Kích thước cây con sinh viên | 
| --- | --- | --- | --- | --- | 
| 1 | (t_4) | (s_1) | (t_5) | (1) | 
| 2 | (t_4) | (s_4) | (t_1) | (2) | 
| 3 | (t_4) | (s_5) | (t_6) | (1) | 
| 4 | (t_4) | (s_2) | (t_2) | (1) | 
| 5 | (t_1) | (s_3) | (t_3) | (1) | 

Kích thước cây con cuối cùng lần lượt là (1,2,1,1,1) cho (s_1,s_4,s_5,s_2,s_3). Các luồng cây khác 0 thu được là 

| Cạnh | Kích thước cây con con | Hoa | 
| --- | --- | --- | 
| (s_1,t_4) | (1) | (1) | 
| (s_1,t_5) | (0) cây con giáo viên | (5) | 
| (s_4,t_4) | (2) | (2) | 
| (s_4,t_1) | (1) cây con giáo viên | (4) | 
| (s_3,t_1) | (1) | (1) | 
| (s_3,t_3) | (0) cây con giáo viên | (5) | 
| (s_5,t_4) | (1) | (1) | 
| (s_5,t_6) | (0) cây con giáo viên | (5) | 
| (s_2,t_4) | (1) | (1) | 
| (s_2,t_2) | (0) cây con giáo viên | (5) | 

Mỗi học sinh có tổng điểm (6) và mọi giáo viên có tổng điểm (5). Điều này thể hiện trực tiếp sự bất biến của cây con. 

### Mẫu 2 

Chọn một kết hợp phù hợp như 

[ 
s_1\đến t_2,\quad 
s_2\đến t_4,\quad 
s_3\đến t_3,\quad 
s_4\đến t_1,\quad 
s_5\đến t_6. 
] 

Thầy (t_5) vô song và trở thành gốc rễ. 

| Bước | Giáo viên hiện tại | Sinh viên mới tiếp cận | Giáo viên phù hợp | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (t_5) | (s_5) | (t_6) | (s_5) đạt | 
| 2 | (t_6) | không | không | điểm dừng tìm kiếm | 
| 3 | giáo viên không thể tiếp cận | (s_1,s_2,s_3,s_4) | khác nhau | chưa bao giờ đạt tới | 

Chỉ có một học sinh được tiếp cận, trong khi bốn học sinh vẫn ở bên ngoài cây xen kẽ. Việc xây dựng do đó in`-1`. 

Lý do mang tính cấu trúc chứ không phải là sự ngẫu nhiên của sự kết hợp đã chọn. Học sinh (s_1,s_2,s_3,s_4) và giáo viên của họ tạo thành một phần riêng biệt của biểu đồ, trong khi (s_5) chỉ giới hạn ở giáo viên (5) và (6). Thiếu giáo viên bổ sung theo yêu cầu của điều kiện Hall được tăng cường đối với tập hợp học sinh lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(M\sqrt N)) | Hopcroft-Karp tìm thấy sự phù hợp; các tìm kiếm biểu đồ và tính toán cây con còn lại là tuyến tính | 
| Không gian | (O(M+N)) | Danh sách kề, mảng khớp, mảng cây và câu trả lời đều có kích thước tuyến tính | 

Với (N\le10^5) và (M\le2\cdot10^5), phần tuyến tính dễ dàng đủ nhỏ. Sự so khớp là thành phần phi tuyến tính duy nhất và (O(M\sqrt N)) là giới hạn dự định cho vấn đề này. Cuộc thảo luận ban đầu về cuộc thi xác định rõ ràng sự kết hợp lưỡng cực (O(E\sqrt V)) là phương pháp dự kiến. 

## Trường hợp thử nghiệm 

Đầu ra không phải là duy nhất nên bộ khai thác kiểm thử sẽ xác thực đầu ra thay vì so sánh nó với một nhiệm vụ cụ thể. Trình xác thực bên dưới kiểm tra xem mọi giá trị được in có thuộc về một cạnh đầu vào hay không, mọi giá trị đều không âm, mọi học sinh gửi chính xác (N) hoa và mọi giáo viên đều nhận được chính xác (N-1).```python
# Run this block after defining solve() from the solution above.

import sys
import io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    with redirect_stdout(out):
        solve()

    sys.stdin = old_stdin
    return out.getvalue().strip()

def check_feasible(inp: str, out: str):
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    m = next(it)

    edges = []
    for _ in range(m):
        s = next(it) - 1
        t = next(it) - 1
        edges.append((s, t))

    assert out != "-1", "expected a feasible assignment"

    values = list(map(int, out.split()))
    assert len(values) == m

    student_sum = [0] * (n - 1)
    teacher_sum = [0] * n

    for value, (s, t) in zip(values, edges):
        assert value >= 0
        student_sum[s] += value
        teacher_sum[t] += value

    assert student_sum == [n] * (n - 1)
    assert teacher_sum == [n - 1] * n

sample1 = """\
6 12
1 3
1 4
1 5
2 2
2 4
3 1
3 3
4 1
4 2
4 4
5 4
5 6
"""

sample2 = """\
6 12
1 2
1 3
1 4
2 2
2 4
3 1
3 3
4 1
4 2
4 4
5 5
5 6
"""

check_feasible(sample1, run(sample1))
assert run(sample2) == "-1", "sample 2"

# Custom 1: minimum-size feasible instance.
case1 = """\
2 2
1 1
1 2
"""
check_feasible(case1, run(case1))

# Custom 2: minimum-size impossible instance because one teacher is isolated.
case2 = """\
2 1
1 1
"""
assert run(case2) == "-1", "isolated teacher"

# Custom 3: a chain-like instance that exercises nested subtree sizes.
case3 = """\
3 4
1 1
1 2
2 2
2 3
"""
check_feasible(case3, run(case3))

# Custom 4: maximum-size boundary test.
# N = 100000, M = 199998. Student i can use teachers i and i+1.
n = 100000
lines = [f"{n} {2 * (n - 1)}"]
for s in range(1, n):
    lines.append(f"{s} {s}")
    lines.append(f"{s} {s + 1}")
case4 = "\n".join(lines)

check_feasible(case4, run(case4))

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Bất kỳ phép gán hợp lệ nào có 12 giá trị | Xây dựng khả thi bình thường và các luồng cây con | 
| Mẫu 2 |`-1`| Một biểu đồ có sự phân công hoa phù hợp nhưng không hợp lệ | 
|`2 2 / 1 1 / 1 2`| Một bông hoa trên mỗi cạnh | Trường hợp khả thi tối thiểu (N=2) | 
|`2 1 / 1 1`|`-1`| Giáo viên bị cô lập và xử lý ranh giới | 
|`3 4 / 1 1 / 1 2 / 2 2 / 2 3`| Bất kỳ bài tập hợp lệ nào | Cây xen kẽ lồng nhau và từng cái một trong luồng giáo viên | 
| Chuỗi được tạo (N=100000) | Bất kỳ phép gán hợp lệ nào có giá trị 199998 | Tối đa (N), quy mô thưa thớt tối đa và hiệu suất | 

## Vỏ cạnh 

Một giáo viên bị cô lập sẽ bị xử lý trong quá trình tìm kiếm xen kẽ vì giáo viên đó chỉ có thể là gốc nếu đó là giáo viên duy nhất chưa từng có. Nếu một giáo viên khác bị cô lập, không có kết quả phù hợp nào có thể bao gồm tất cả học sinh trong khi cây xen kẽ đến được mọi giáo viên. Vì```
2 1
1 1
```việc so khớp bao gồm học sinh 1 với giáo viên 1, để giáo viên 2 làm gốc. Gốc không có cạnh không khớp đi ra, vì vậy học sinh 1 không bao giờ đạt tới. Thuật toán in`-1`. 

Một học sinh bị cô lập thậm chí còn bị từ chối sớm hơn. Với```
3 1
1 1
```kích thước phù hợp chỉ có một, trong khi có hai học sinh. Vì việc so khớp không bao gồm tất cả học sinh nên thuật toán sẽ in`-1`trước khi xây dựng cây xen kẽ. 

Sự kết hợp phù hợp với mọi học sinh vẫn có thể là chưa đủ. Trong Mẫu 2, kết quả khớp có kích thước 5, nhưng việc chọn giáo viên chưa khớp làm gốc chỉ tiếp cận thành phần chứa học sinh 5. Việc tìm kiếm tiếp cận một học sinh thay vì cả năm, do đó thuật toán sẽ loại bỏ biểu đồ. Đây chính xác là điều kiện bổ sung ngoài việc kết hợp Hall thông thường. 

Trường hợp khả thi tối thiểu```
2 2
1 1
1 2
```có một học sinh với hai giáo viên có sẵn. Nhiệm vụ duy nhất có thể thực hiện được là mỗi giáo viên được tặng một bông hoa. Sự kết hợp để lại một giáo viên không thể so sánh được, cái gốc đến với học sinh qua cạnh không phù hợp và hai cạnh cây đều nhận được một bông hoa. Công thức sử dụng`N - 1 - size[teacher_subtree] = 1`. 

Trường hợp dây chuyền```
3 4
1 1
1 2
2 2
2 3
```rất hữu ích vì cây được lồng vào nhau chứ không phải là một ngôi sao. Giáo viên gốc tiếp cận học sinh 2, giáo viên phù hợp của họ tiếp cận học sinh 1, giáo viên phù hợp của họ là một chiếc lá. Kích thước cây con học sinh trở thành (2) cho học sinh 2 và (1) cho học sinh 1. Do đó, cạnh gốc tới học sinh nhận được (2) hoa, học sinh 2 gửi (1) hoa cho giáo viên phù hợp và học sinh 1 gửi (2) hoa cho giáo viên phù hợp. Mỗi học sinh gửi đúng (3), còn mỗi giáo viên nhận đúng (2). 

Các cạnh không có dòng chảy không cần xử lý đặc biệt. Khi cây xen kẽ được chọn, mọi cạnh đầu vào bên ngoài cây đó sẽ giữ nguyên bằng 0. Công trình không bao giờ cố gắng ép dòng chảy tích cực qua một cạnh chỉ vì nó tồn tại. 

Trường hợp kích thước tối đa nhấn mạnh việc triển khai phù hợp và xây dựng tuyến tính đồng thời. Với (N=100000) và (M=199998), đồ thị```
s -> t_s
s -> t_{s+1}
```for (1\le s<N) tạo thành một chuỗi dài. Việc so khớp lặp và duyệt cây tránh được lỗi độ sâu đệ quy và tính toán cây con vẫn tuyến tính về số đỉnh và cạnh.
