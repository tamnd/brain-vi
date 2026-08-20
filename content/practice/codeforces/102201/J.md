---
title: "CF 102201J - Thầy Cô Ghen Tuông"
description: "Hãy coi học sinh và giáo viên như hai mặt của biểu đồ lưỡng cực. Có (N-1) học sinh ở bên trái và (N) giáo viên ở bên phải. Cặp đầu vào ((s,t)) có nghĩa là (các) học sinh được phép gửi hoa cho giáo viên (t)."
date: "2026-08-18T01:52:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102201
codeforces_index: "J"
codeforces_contest_name: "Moscow Pre-Finals Workshop 2019. KAIST Contest"
rating: 0
weight: 102201
solve_time_s: 229
verified: true
draft: false
---

[CF 102201J - Giáo viên ghen tị](https://codeforces.com/problemset/problem/102201/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi học sinh và giáo viên như hai mặt của biểu đồ lưỡng cực. Có (N-1) học sinh ở bên trái và (N) giáo viên ở bên phải. Cặp đầu vào ((s,t)) có nghĩa là (các) học sinh được phép gửi hoa cho giáo viên (t). 

Mỗi học sinh phải phát chính xác (N) bông hoa, trong khi mỗi giáo viên phải nhận chính xác (N-1) bông hoa. Đối với mỗi cặp được phép, chúng ta cần chọn một số nguyên không âm. Một cạnh có thể mang nhiều hoa và một cạnh được phép có thể không mang hoa. Đầu ra cho các lượng này theo thứ tự giống như các cạnh đầu vào. Nếu không có phép gán như vậy tồn tại, chúng ta in (-1). Bài toán là bài toán vận chuyển đặc biệt trên đồ thị lưỡng cực thưa thớt. 

Tổng nguồn cung là ((N-1)N) và tổng nhu cầu là (N(N-1)), do đó tổng số toàn cầu đã đồng ý. Khó khăn hoàn toàn là do hạn chế về số lượng cặp học sinh-giáo viên. 

Các giới hạn (N\le 10^5) và (M\le 2\cdot10^5) loại trừ các thuật toán xử lý nhiều lần toàn bộ biểu đồ (N). Phương thức (O(NM)) có thể kiểm tra khoảng (2\cdot10^{10}) cạnh trong trường hợp xấu nhất. Ngay cả một công thức lưu lượng tối đa tăng đơn vị thông thường cũng có thể yêu cầu các lần tăng (N(N-1)), đưa ra (O(MN^2)), kiểm tra cạnh (2\cdot10^{15}) trong trường hợp xấu nhất. Chúng ta cần một thuật toán khớp xung quanh (O(M\sqrt N)), sau đó là xây dựng thời gian tuyến tính. 

Có một số trường hợp đặc biệt trong đó việc triển khai có vẻ hợp lý không thành công. Ví dụ: với (N=2),```
2 2
1 1
1 2
```học sinh duy nhất có thể tặng mỗi giáo viên một bông hoa, vì vậy kết quả đầu ra phải là`1 1`. Việc triển khai yêu cầu mọi giáo viên phải được so khớp với một học sinh riêng biệt sẽ từ chối điều này một cách không chính xác vì số lượng bông hoa cuối cùng không khớp. 

Một trường hợp tế nhị hơn là```
3 2
1 1
2 2
```Có một sự so sánh chứa cả hai học sinh, nhưng giáo viên 3 không có cạnh sự cố. Đầu ra đúng là`-1`. Vì vậy, việc tìm kiếm sự phù hợp cho mọi học sinh là cần thiết nhưng chưa đủ. 

Một trường hợp tinh tế khác xảy ra khi một giáo viên không thể sánh được với sự so khớp ban đầu. Thầy đó không hề mắc lỗi trong việc ghép nối. Nó chính xác là gốc mà từ đó chúng tôi xây dựng cấu trúc xen kẽ được sử dụng để xây dựng tất cả các phân phối hoa cần thiết. 

Cuối cùng, giá trị đầu ra có thể bằng 0 một cách hợp pháp. Ví dụ: một cạnh được phép có thể không nhận được hoa vì các cạnh được phép khác của cùng một học sinh có thể mang toàn bộ số lượng. Việc coi mọi cạnh đầu vào là yêu cầu giá trị dương sẽ từ chối các giải pháp hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xây dựng một mạng lưới dòng chảy. Thêm một nguồn được kết nối với mọi học sinh có dung lượng (N), kết nối mọi cặp học sinh-giáo viên được phép với dung lượng không giới hạn một cách hiệu quả và kết nối mọi giáo viên với bồn chứa có dung lượng (N-1). Một luồng (N(N-1)) sẽ chính xác là câu trả lời được yêu cầu. 

Mô hình này đúng, nhưng việc sử dụng thuật toán đường dẫn tăng cường chung thì quá chậm. Nếu chúng ta tăng thêm một bông hoa tại một thời điểm thì có thể có (N(N-1)) số lần tăng thêm. Mỗi tìm kiếm có thể kiểm tra (O(M+N)) cạnh, cho kết quả (O(MN^2)), tức là khoảng (2\cdot10^{15}) kiểm tra cạnh ở giới hạn tối đa. 

Quan sát hữu ích là năng lực có mối quan hệ rất đặc biệt. Mỗi học sinh cần (N), trong khi mỗi giáo viên cần (N-1), và có đúng một giáo viên nhiều hơn học sinh. 

Giả sử chúng ta tạm thời hạn chế mọi cạnh được phép mang tối đa một bông hoa. Một kết quả phù hợp bao gồm tất cả (N-1) học sinh sẽ tặng cho mỗi học sinh một bông hoa và tặng (N-1) giáo viên một bông hoa. Chính xác là một giáo viên vẫn chưa từng có. 

Bây giờ hãy tưởng tượng việc xây dựng một kết quả phù hợp cho mọi giáo viên có thể bị bỏ qua. Nếu giáo viên (t) bị bỏ qua, chúng tôi muốn có sự trùng khớp giữa tất cả (N-1) học sinh và (N-1) giáo viên khác. Với mỗi giáo viên bị bỏ sót (t), hãy tặng một bông hoa dọc theo mỗi cạnh của phần khớp đó. 

Có (N) sự trùng khớp như vậy. Mỗi học sinh thuộc mỗi cặp phù hợp, vì vậy mỗi học sinh nhận được tổng cộng chính xác (N) bông hoa. Một giáo viên cố định (t) thuộc về tất cả các kết quả phù hợp ngoại trừ trường hợp (t) bị bỏ qua, do đó giáo viên đó nhận được chính xác (N-1) hoa. 

Vấn đề còn lại là xây dựng tất cả các kết quả khớp này mà không chạy thuật toán so khớp (N) lần. Bắt đầu bằng một phương pháp so khớp bao gồm tất cả học sinh, không để lại giáo viên (r) nào sánh kịp. Định hướng mọi lợi thế chưa từng có từ giáo viên đến học sinh và mọi lợi thế phù hợp từ học sinh đến giáo viên. Bắt đầu từ (r), hãy đi theo biểu đồ xen kẽ có hướng này. 

Bất cứ khi nào chúng tôi di chuyển 

[ 
\text{giáo viên } a \rightarrow \text{học sinh } s \rightarrow \text{giáo viên } b, 
] 

cạnh đầu tiên không khớp và cạnh thứ hai nằm trong khớp. Lật hai cạnh đó sẽ thay đổi giáo viên không ai sánh kịp, chuyển giáo viên chưa ai sánh kịp từ (a) sang (b). 

Nếu mọi giáo viên đều có thể truy cập được từ (r), cây bao trùm của các chuyển đổi xen kẽ này sẽ đưa ra đường dẫn từ (r) đến mọi giáo viên. Lật các cạnh dọc theo đường dẫn đó sẽ tạo ra sự trùng khớp mà giáo viên đích đến không thể so sánh được. Do đó, tất cả (N) kết quả khớp có thể được biểu diễn bằng một cây thay vì được lưu trữ một cách rõ ràng. 

Điều kiện về khả năng tiếp cận cũng chính là yếu tố phân biệt các trường hợp khả thi và không khả thi. Điều kiện Hall cho bài tập hoa ban đầu giảm xuống còn yêu cầu mạnh hơn là sau khi xóa bất kỳ một giáo viên nào, đồ thị còn lại vẫn có một phần khớp bao phủ mọi học sinh. Đây chính xác là điều mà bài kiểm tra khả năng tiếp cận xen kẽ kiểm tra. Giải pháp dự định sử dụng đối sánh hai bên tối đa, theo sau là cấu trúc xen kẽ này, với thời gian đối sánh (O(M\sqrt N)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force / luồng tăng cường đơn vị | (O(MN^2)) trường hợp xấu nhất | (O(N+M)) | Quá chậm | 
| Tối ưu | (O(M\sqrt N)) | (O(N+M)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng biểu đồ hai bên sử dụng một bên là học sinh và một bên là giáo viên. Giữ chỉ số cạnh ban đầu vì câu trả lời cuối cùng phải được in theo thứ tự đầu vào. 
2. Tìm sự kết hợp lưỡng cực tối đa bằng cách sử dụng Hopcroft-Karp. Chúng tôi chỉ cần kết quả phù hợp về kích thước (N-1), nghĩa là mọi học sinh đều được kết nối. Nếu kết quả phù hợp tối đa có kích thước nhỏ hơn thì không thể phân công hoa được. 
3. Hãy để (r) là giáo viên duy nhất không thể so sánh được với sự phù hợp này. Xem xét mọi cạnh không khớp theo hướng dẫn từ giáo viên đến học sinh và mọi cạnh khớp theo hướng dẫn từ học sinh đến giáo viên. 
4. Bắt đầu truyền tải tại (r). Khi quá trình truyền tải đạt đến giáo viên (t), hãy kiểm tra mọi cạnh không khớp (t-s). (Các) học sinh có chính xác một cạnh phù hợp, ví dụ (s-u), do đó quá trình chuyển đổi xen kẽ là (t\rightarrow s\rightarrow u). Nếu (u) chưa đạt được, hãy biến nó thành con của (t) trong cây giáo viên. 
5. Nếu không gặp được giáo viên nào đó, hãy in ra (-1). Phần không thể tiếp cận sẽ gây cản trở Hội trường, do đó không có sự phân công hoa hợp lệ nào tồn tại. 
6. Việc duyệt cho một cây có đỉnh là (N) giáo viên. Mỗi giáo viên không phải gốc (u) đều có giáo viên cha (p), cạnh không khớp (p-s) và cạnh khớp (s-u). Hai cạnh ban đầu này tạo thành một bước xen kẽ trong cây. 
7. Root cây giáo viên tại (r) và tính kích thước mỗi cây con. Đặt kích thước cây con của giáo viên không phải gốc (u) là (k). Chính xác (k) của (N) đường dẫn từ gốc đến giáo viên chứa cạnh cây từ (p) đến (u). 
8. Ban đầu, tặng mỗi bông hoa có cạnh (N) phù hợp ban đầu và mọi bông hoa có cạnh không có cạnh khác. Đối với cạnh cây (p-s-u), thay đổi số lượng trên cạnh không khớp (p-s) thành (k) và thay đổi số lượng trên cạnh phù hợp (s-u) thành (N-k). 

Lý do cho hai giá trị này rất đơn giản. Đối với mỗi giáo viên mục tiêu trong cây con của (u), đường dẫn từ (r) đến mục tiêu đó sẽ lật cặp xen kẽ này. Do đó, cạnh không khớp được chọn trong chính xác (k) trong số các khớp (N), trong khi cạnh khớp ban đầu được chọn trong các khớp (N-k) khác. 

### Tại sao nó hoạt động 

Bất biến chính là đối với mỗi giáo viên (t), đường dẫn duy nhất từ gốc (r) đến (t) là một đường dẫn xen kẽ có các cạnh xen kẽ giữa các cạnh không khớp và cạnh khớp. Việc lật đường dẫn đó sẽ giữ nguyên thuộc tính phù hợp và thay đổi giáo viên chưa khớp từ (r) thành (t). Do đó, với mỗi giáo viên (t), chúng ta thu được một kết quả khớp chứa mọi học sinh và mọi giáo viên ngoại trừ (t). 

Bây giờ tính tổng chỉ số của mọi cạnh trên tất cả (N) kết quả khớp như vậy. Mỗi học sinh được ghép một lần trong mỗi lần ghép và tặng (N) bông hoa cho nó. Giáo viên (t) vắng mặt đúng một lần và tặng hoa (N-1). Mọi cạnh được sử dụng đều thuộc về biểu đồ gốc và tất cả số lượng đều không âm vì kích thước cây con nằm trong khoảng từ (1) đến (N-1). 

Nếu việc truyền tải xen kẽ không thể tiếp cận được một số giáo viên thì biểu đồ sẽ không có sự so khớp cần thiết sau khi xóa giáo viên đó. Do đó việc giao hoa là không thể. Điều này thiết lập cả hai hướng của công trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

sys.setrecursionlimit(300000)

def solve(stream=None):
    if stream is None:
        stream = sys.stdin
    input = stream.readline

    N, M = map(int, input().split())

    # Edge i joins student S[i] with teacher T[i].
    S = [0] * M
    T = [0] * M

    # Adjacency by student, storing edge indices.
    adj = [[] for _ in range(N - 1)]

    for i in range(M):
        s, t = map(int, input().split())
        s -= 1
        t -= 1
        S[i] = s
        T[i] = t
        adj[s].append(i)

    left_n = N - 1
    right_n = N

    # pair_s[s] = teacher matched to student s, or -1.
    # pair_t[t] = student matched to teacher t, or -1.
    pair_s = [-1] * left_n
    pair_t = [-1] * right_n
    dist = [0] * left_n
    ptr = [0] * left_n

    # A greedy initial matching greatly reduces the number of
    # Hopcroft-Karp phases in practice.
    matching = 0
    for s in range(left_n):
        for eid in adj[s]:
            t = T[eid]
            if pair_t[t] == -1:
                pair_s[s] = t
                pair_t[t] = s
                matching += 1
                break

    def bfs():
        q = deque()
        for s in range(left_n):
            if pair_s[s] == -1:
                dist[s] = 0
                q.append(s)
            else:
                dist[s] = -1

        found = False

        while q:
            s = q.popleft()
            d = dist[s]

            for eid in adj[s]:
                t = T[eid]
                ns = pair_t[t]

                if ns == -1:
                    found = True
                elif dist[ns] == -1:
                    dist[ns] = d + 1
                    q.append(ns)

        return found

    def dfs(s):
        while ptr[s] < len(adj[s]):
            eid = adj[s][ptr[s]]
            ptr[s] += 1

            t = T[eid]
            ns = pair_t[t]

            if ns == -1 or (
                dist[ns] == dist[s] + 1 and dfs(ns)
            ):
                pair_s[s] = t
                pair_t[t] = s
                return True

        dist[s] = -1
        return False

    while bfs():
        for s in range(left_n):
            ptr[s] = 0

        for s in range(left_n):
            if pair_s[s] == -1 and dfs(s):
                matching += 1

    if matching != left_n:
        return "-1\n"

    # For every student, remember the edge used by the matching.
    match_edge = [-1] * left_n
    for s in range(left_n):
        t = pair_s[s]
        for eid in adj[s]:
            if T[eid] == t:
                match_edge[s] = eid
                break

    # Root is the unique unmatched teacher.
    root = -1
    for t in range(right_n):
        if pair_t[t] == -1:
            root = t
            break

    # Teacher-tree construction.
    #
    # parent[t] = parent teacher of t.
    # parent_student[t] is the student used in the alternating step.
    # parent_edge[t] is the nonmatching edge parent[t] -> parent_student[t].
    parent = [-1] * right_n
    parent_student = [-1] * right_n
    parent_edge = [-1] * right_n

    visited_teacher = [False] * right_n
    visited_student = [False] * left_n

    visited_teacher[root] = True
    stack = [root]

    while stack:
        t = stack.pop()

        # Inspect nonmatching edges from teacher t to students.
        for s in range(left_n):
            # This scan would be too slow, so this branch is replaced below.
            pass
        # The actual traversal is performed using a reverse adjacency list.
        break

    # Reverse adjacency by teacher, storing edge indices.
    by_teacher = [[] for _ in range(right_n)]
    for eid in range(M):
        by_teacher[T[eid]].append(eid)

    stack = [root]
    while stack:
        t = stack.pop()

        for eid in by_teacher[t]:
            s = S[eid]

            # Matching edges are directed student -> teacher,
            # so from a teacher we may only use nonmatching edges.
            if pair_s[s] == t:
                continue

            if visited_student[s]:
                continue

            visited_student[s] = True
            nt = pair_s[s]

            if nt == -1 or visited_teacher[nt]:
                continue

            visited_teacher[nt] = True
            parent[nt] = t
            parent_student[nt] = s
            parent_edge[nt] = eid
            stack.append(nt)

    if not all(visited_teacher):
        return "-1\n"

    # Build the teacher tree as an ordinary adjacency list.
    tree = [[] for _ in range(right_n)]
    for t in range(right_n):
        if t == root:
            continue
        p = parent[t]
        tree[p].append(t)

    # Compute subtree sizes iteratively.
    order = [root]
    for t in order:
        for child in tree[t]:
            order.append(child)

    subtree = [1] * right_n
    for t in reversed(order):
        if t != root:
            subtree[parent[t]] += subtree[t]

    # Start from the interpretation of the original matching:
    # every matching edge occurs in all N matchings.
    ans = [0] * M
    for s in range(left_n):
        ans[match_edge[s]] = N

    # Each teacher-tree edge represents an alternating pair:
    # nonmatching edge gets subtree size,
    # matching edge gets N - subtree size.
    for t in range(right_n):
        if t == root:
            continue

        k = subtree[t]
        s = parent_student[t]

        ans[parent_edge[t]] = k
        ans[match_edge[s]] = N - k

    return "".join(f"{x}\n" for x in ans)

if __name__ == "__main__":
    sys.stdout.write(solve())
```Đầu tiên, mã này lưu trữ mọi mối quan hệ đầu vào của sinh viên, đây là cách biểu diễn tự nhiên cho Hopcroft-Karp. Chỉ số cạnh được giữ riêng để số tiền cuối cùng có thể được viết theo đúng thứ tự đầu vào. 

Kết hợp tham lam ban đầu chỉ là tối ưu hóa. Hopcroft-Karp vẫn chịu trách nhiệm đạt được kết quả khớp tối đa, do đó lựa chọn tham lam không thể ảnh hưởng đến tính chính xác. 

Các mảng phù hợp sử dụng`-1`như giá trị chưa từng có. Sau khi khớp, mỗi học sinh có đúng một giáo viên trùng khớp và đúng một giáo viên không có học sinh trùng khớp vì có (N-1) học sinh và (N) giáo viên. 

Việc truyền tải xen kẽ cần sự kề cận theo hướng ngược lại, từ giáo viên đến chỉ số cạnh, vì vậy`by_teacher`được xây dựng một lần trong (O(M)). Từ một giáo viên, chúng tôi cố tình bỏ qua cạnh phù hợp của nó. Một cạnh không khớp sẽ dẫn đến một học sinh và học sinh đó có chính xác một cạnh trùng khớp sẽ dẫn đến giáo viên tiếp theo trên cây. 

Kích thước cây con được tính toán mà không cần DFS cây đệ quy. Điều này tránh được các vấn đề về độ sâu đệ quy Python trên đường dẫn chứa (10^5) giáo viên. 

Công thức cuối cùng sử dụng`N - k`, không`N - 1 - k`. Cạnh khớp ban đầu xuất hiện trong mọi (N) khớp trước khi chúng ta lật đường dẫn. Chính xác (k) trong số các đường dẫn đó chứa cạnh cây này và loại bỏ cạnh phù hợp đó, để lại (N-k) lần xuất hiện. 

Số lượng luôn là số nguyên nằm trong khoảng từ 0 đến (N), vì vậy số nguyên có độ chính xác tùy ý của Python là quá đủ. 

Một chi tiết triển khai nhỏ có thể được đơn giản hóa hơn nữa trong quá trình gửi sản phẩm: chi tiết đầu tiên`while stack`vòng lặp trong mã là cố ý vô hại nhưng không cần thiết. Quá trình duyệt giáo viên thực sự diễn ra ngay sau`by_teacher`được xây dựng. Việc loại bỏ vòng lặp sơ bộ đó sẽ làm cho phiên bản được gửi sạch hơn. 

Đây là phiên bản đã được làm sạch của cùng một giải pháp.```python
import sys
input = sys.stdin.readline
from collections import deque

sys.setrecursionlimit(300000)

def solve(stream=None):
    if stream is None:
        stream = sys.stdin
    input = stream.readline

    N, M = map(int, input().split())

    S = [0] * M
    T = [0] * M
    adj = [[] for _ in range(N - 1)]
    by_teacher = [[] for _ in range(N)]

    for i in range(M):
        s, t = map(int, input().split())
        s -= 1
        t -= 1
        S[i] = s
        T[i] = t
        adj[s].append(i)
        by_teacher[t].append(i)

    L = N - 1
    pair_s = [-1] * L
    pair_t = [-1] * N
    dist = [0] * L
    ptr = [0] * L

    matching = 0

    for s in range(L):
        for eid in adj[s]:
            t = T[eid]
            if pair_t[t] == -1:
                pair_s[s] = t
                pair_t[t] = s
                matching += 1
                break

    def bfs():
        q = deque()

        for s in range(L):
            if pair_s[s] == -1:
                dist[s] = 0
                q.append(s)
            else:
                dist[s] = -1

        found = False

        while q:
            s = q.popleft()
            d = dist[s]

            for eid in adj[s]:
                t = T[eid]
                ns = pair_t[t]

                if ns == -1:
                    found = True
                elif dist[ns] == -1:
                    dist[ns] = d + 1
                    q.append(ns)

        return found

    def dfs(s):
        while ptr[s] < len(adj[s]):
            eid = adj[s][ptr[s]]
            ptr[s] += 1

            t = T[eid]
            ns = pair_t[t]

            if ns == -1 or (
                dist[ns] == dist[s] + 1 and dfs(ns)
            ):
                pair_s[s] = t
                pair_t[t] = s
                return True

        dist[s] = -1
        return False

    while bfs():
        for s in range(L):
            ptr[s] = 0

        for s in range(L):
            if pair_s[s] == -1 and dfs(s):
                matching += 1

    if matching != L:
        return "-1\n"

    match_edge = [-1] * L
    for s in range(L):
        target = pair_s[s]
        for eid in adj[s]:
            if T[eid] == target:
                match_edge[s] = eid
                break

    root = pair_t.index(-1)

    parent = [-1] * N
    parent_student = [-1] * N
    parent_edge = [-1] * N
    visited = [False] * N

    visited[root] = True
    stack = [root]

    while stack:
        t = stack.pop()

        for eid in by_teacher[t]:
            s = S[eid]

            if pair_s[s] == t:
                continue

            nt = pair_s[s]

            if visited[nt]:
                continue

            visited[nt] = True
            parent[nt] = t
            parent_student[nt] = s
            parent_edge[nt] = eid
            stack.append(nt)

    if not all(visited):
        return "-1\n"

    tree = [[] for _ in range(N)]
    for t in range(N):
        if t != root:
            tree[parent[t]].append(t)

    order = [root]
    for t in order:
        order.extend(tree[t])

    subtree = [1] * N
    for t in reversed(order):
        if t != root:
            subtree[parent[t]] += subtree[t]

    ans = [0] * M

    for s in range(L):
        ans[match_edge[s]] = N

    for t in range(N):
        if t == root:
            continue

        k = subtree[t]
        s = parent_student[t]

        ans[parent_edge[t]] = k
        ans[match_edge[s]] = N - k

    return "".join(f"{x}\n" for x in ans)

if __name__ == "__main__":
    sys.stdout.write(solve())
```## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu có (N=6) nên có 5 học sinh và 6 giáo viên. Với thứ tự đầu vào, kết hợp tham lam có thể chọn 

[ 
1\rightarrow3,\quad 
2\rightarrow2,\quad 
3\rightarrow1,\quad 
4\rightarrow4,\quad 
5\rightarrow6. 
] 

Thầy 5 vô đối nên trở thành gốc. 

Việc duyệt xen kẽ tạo ra cây giáo viên 

[ 
5\rightarrow3\rightarrow1\rightarrow4, 
] 

với giáo viên 4 cũng có con 2 và 6. Kích thước cây con thu được được hiển thị bên dưới. 

| Giáo viên | Phụ huynh | Sinh viên đang chuyển tiếp | Kích thước cây con | 
| --- | --- | --- | --- | 
| 5 | không | không | 6 | 
| 3 | 5 | 1 | 5 | 
| 1 | 3 | 3 | 4 | 
| 4 | 1 | 4 | 3 | 
| 2 | 4 | 2 | 1 | 
| 6 | 4 | 5 | 1 | 

Ví dụ: quá trình chuyển đổi cây (5\rightarrow1\rightarrow3) sử dụng cạnh không khớp ((1,5)) và cạnh khớp ((1,3)). Vì giáo viên 3 có cây con cỡ 5 nên cạnh ((1,5)) nhận được 5 bông hoa và cạnh ((1,3)) nhận được (6-5=1). 

Dấu vết đầy đủ của các giá trị cạnh thu được là: 

| Cạnh đầu vào | Trạng thái phù hợp | Cây con | Hoa | 
| --- | --- | --- | --- | 
| (1,3) | khớp | 5 | 1 | 
| (1,4) | không phải cây | 0 | 0 | 
| (1,5) | cây không khớp | 5 | 5 | 
| (2,2) | khớp | 1 | 5 | 
| (2,4) | cây không khớp | 1 | 1 | 
| (3,1) | khớp | 4 | 2 | 
| (3,3) | cây không khớp | 4 | 4 | 
| (4,1) | cây không khớp | 3 | 3 | 
| (4,2) | không phải cây | 0 | 0 | 
| (4,4) | khớp | 3 | 3 | 
| (5,4) | cây không khớp | 1 | 1 | 
| (5,6) | khớp | 1 | 5 | 

Mỗi học sinh nhận được (6) bông hoa. Mỗi giáo viên nhận được (5). Các giá trị này khác với kết quả mẫu, điều này được mong đợi vì bài toán cho phép mọi cách xây dựng hợp lệ. 

### Mẫu 2 

Mẫu thứ hai cũng tương tự (N=6), nhưng học sinh 5 chỉ có thể tiếp cận được giáo viên 5 và 6. Việc so khớp tham lam có thể bao gồm các học sinh 1, 2, 3 và 5, trong khi học sinh 4 không thể khớp được. 

| Sinh viên | Giáo viên có sẵn | Trạng thái phù hợp | 
| --- | --- | --- | 
| 1 | 2, 3, 4 | khớp | 
| 2 | 2, 4 | khớp | 
| 3 | 1, 3 | khớp | 
| 4 | 1, 2, 4 | chưa từng có | 
| 5 | 5, 6 | khớp | 

Kết quả khớp tối đa có kích thước 4 thay vì yêu cầu 5. Do đó, Hopcroft-Karp kết thúc mà không khớp với học sinh 4 và thuật toán sẽ in ngay lập tức (-1). 

Điều này chứng tỏ thử nghiệm khả thi đầu tiên. Không có lý do gì để xây dựng cây xen kẽ khi biểu đồ thậm chí không thể cung cấp một giáo viên riêng biệt cho mỗi học sinh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(M\sqrt N)) | Hopcroft-Karp chiếm ưu thế; việc duyệt xen kẽ, xây dựng cây và tính toán cây con đều là (O(N+M)) | 
| Không gian | (O(N+M)) | Danh sách kề, mảng khớp, mảng cây và lưu trữ câu trả lời | 

Sự phù hợp tối đa là phần phi tuyến tính duy nhất. Với (M\le2\cdot10^5) và (N\le10^5), giới hạn dự định (O(M\sqrt N)) là khoảng (6,3\cdot10^7) các phép toán lớp cạnh cơ bản trong ước tính tiệm cận xấu nhất, trong khi tất cả công việc xây dựng đều là tuyến tính. Việc sử dụng bộ nhớ thoải mái trong giới hạn 1024 MB. Cuộc thảo luận ban đầu về cuộc thi cũng xác định sự kết hợp lưỡng cực (O(E\sqrt V)) là mức độ phức tạp dự kiến. 

## Trường hợp thử nghiệm 

Vì đây là bài toán đánh giá đặc biệt nên kết quả đầu ra hợp lệ chính xác không phải là duy nhất. Các thử nghiệm dưới đây xác nhận các điều kiện cấu trúc thay vì so sánh với một đầu ra cụ thể.```python
# This test file assumes the solution above is available as:
# from solution import solve

import io
from solution import solve

def run(inp: str) -> str:
    return solve(io.StringIO(inp))

def validate(inp: str, out: str):
    data = list(map(int, inp.split()))
    n, m = data[0], data[1]

    edges = []
    pos = 2
    allowed = set()

    for _ in range(m):
        s = data[pos] - 1
        t = data[pos + 1] - 1
        pos += 2
        edges.append((s, t))
        allowed.add((s, t))

    out = out.strip()

    if out == "-1":
        return False

    values = list(map(int, out.split()))
    assert len(values) == m

    row = [0] * (n - 1)
    col = [0] * n

    for value, (s, t) in zip(values, edges):
        assert value >= 0
        row[s] += value
        col[t] += value

    assert row == [n] * (n - 1)
    assert col == [n - 1] * n

    return True

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

assert validate(sample1, run(sample1)), "sample 1"

assert run(sample2).strip() == "-1", "sample 2"

minimum = """\
2 2
1 1
1 2
"""
assert validate(minimum, run(minimum)), "minimum valid case"

disconnected = """\
3 2
1 1
2 2
"""
assert run(disconnected).strip() == "-1", "isolated teacher"

complete = """\
4 12
1 1
1 2
1 3
1 4
2 1
2 2
2 3
2 4
3 1
3 2
3 3
3 4
"""
assert validate(complete, run(complete)), "complete bipartite graph"

# Maximum-size edge count: N = 100000, M = 200000.
# Each student i connects to teachers i and i+1, then two
# additional legal edges make M exactly 200000.
n = 100000
lines = [f"{n} 200000"]

for i in range(1, n):
    lines.append(f"{i} {i}")
    lines.append(f"{i} {i + 1}")

lines.append("1 3")
lines.append("1 4")

maximum = "\n".join(lines) + "\n"
assert validate(maximum, run(maximum)), "maximum-size valid case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Bất kỳ phép gán dòng (M) hợp lệ nào | Xây dựng đầy đủ trên biểu đồ có tính tuần hoàn | 
| Mẫu 2 |`-1`| Không tìm được thông tin phù hợp cho mọi học sinh | 
| (N=2), hai cạnh | Bất kỳ bài tập hợp lệ nào | Trường hợp ranh giới tối thiểu | 
| (N=3), hai cạnh rời nhau |`-1`| Kết hợp đầy đủ học sinh là không đủ | 
| Hoàn thành (K_{3,4}) | Bất kỳ bài tập hợp lệ nào | Đồ thị dày đặc và nhiều giải pháp khả thi | 
| (N=100000,\ M=200000) | Bất kỳ bài tập hợp lệ nào | Đầu vào có kích thước tối đa và cây xen kẽ dài | 

## Vỏ cạnh 

Trường hợp tối thiểu```
2 2
1 1
1 2
```có một học sinh và hai giáo viên. Hopcroft-Karp tìm học sinh 1 phù hợp với giáo viên 1, lấy giáo viên 2 làm gốc. Việc truyền tải xen kẽ sử dụng cạnh cho giáo viên 2 và cạnh khớp cho giáo viên 1, tạo ra cây giáo viên hai đỉnh. Kích thước cây con của giáo viên 1 là 1, do đó cạnh phù hợp nhận được (2-1=1) hoa và cạnh còn lại nhận được 1 bông hoa. Cả giáo viên đều nhận được (N-1=1) và học sinh nhận được (N=2). 

Vụ giáo viên bị cô lập```
3 2
1 1
2 2
```tinh tế hơn. Sự kết hợp tối đa bao trùm cả hai học sinh, nhưng giáo viên 3 là không thể so sánh được và không có lợi thế dẫn đến việc truyền tải xen kẽ. Việc truyền tải chỉ đến được chính giáo viên 3 nên không phải tất cả giáo viên đều được truy cập. Thuật toán in (-1). Điều này mắc phải lỗi phổ biến là chỉ kiểm tra xem tất cả học sinh có thể khớp hay không. 

Một cạnh được phép không nhận được hoa nào sẽ được xử lý một cách tự nhiên. Trong Mẫu 1, cạnh ((1,4)) có thể nhận 0 vì học sinh 1 đã gửi cả sáu bông hoa qua ((1,3)) và ((1,5)). Thuật toán không bao giờ giả định rằng cạnh đầu vào phải mang luồng dương. 

Trường hợp có nhiều cạnh tạo thành chu trình cũng an toàn. Việc truyền tải xen kẽ có chủ ý chỉ giữ lại quá trình chuyển đổi đầu tiên đến từng giáo viên, tạo ra một cây. Các cạnh bổ sung được để ở mức 0 trừ khi chúng thuộc về kết quả khớp ban đầu. Chúng không cần thiết vì cây đã đại diện cho tất cả (N) kết quả khớp cần thiết. 

Trường hợp kích thước tối đa có (N=100000) và (M=200000). Một chuỗi dài các chuyển đổi xen kẽ có thể chứa hầu hết tất cả các giáo viên, do đó, việc duyệt cây đệ quy sẽ có nguy cơ vượt quá độ sâu đệ quy của Python. Việc triển khai tính toán lặp đi lặp lại thứ tự duyệt và kích thước cây con, giữ cho giai đoạn xây dựng tuyến tính và an toàn cho trường hợp ranh giới.
