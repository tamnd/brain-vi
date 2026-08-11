---
title: "CF 102412B - Alexey Hiền nhân Lục đạo"
description: "Hãy coi mỗi đảng viên như một đỉnh. Có (n) đỉnh ở bên trái, được đánh số từ (1) đến (n) và (n) ở bên phải, được đánh số từ (n+1) đến (2n). Mọi bài toán đều tạo ra một cạnh giữa một đỉnh trái và một đỉnh phải."
date: "2026-08-10T13:43:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102412
codeforces_index: "B"
codeforces_contest_name: "MEX Foundation Contest (supported by AIM Tech)"
rating: 0
weight: 102412
solve_time_s: 352
verified: true
draft: false
---

[CF 102412B - Alexey Hiền nhân của Lục Đạo](https://codeforces.com/problemset/problem/102412/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 52 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi đảng viên như một đỉnh. Có (n) đỉnh ở bên trái, được đánh số từ (1) đến (n) và (n) ở bên phải, được đánh số từ (n+1) đến (2n). Mọi bài toán đều tạo ra một cạnh giữa một đỉnh trái và một đỉnh phải. Một số bài toán có thể kết nối cùng một cặp phần tử, do đó cho phép các cạnh song song. 

Sau khi làm xong tất cả các bài tập, mỗi thành viên chọn một bài toán được giao cho mình. Một vấn đề được giải quyết chính xác khi cả hai đầu cuối của nó đều chọn vấn đề đó. Vì mỗi thành viên chọn nhiều nhất một bài toán, nên số lượng bài toán được giải tối đa có thể chính xác là kích thước khớp tối đa của đồ thị hai bên này. Nhiệm vụ là chọn chính xác (m) cạnh sao cho kết quả khớp lớn nhất có kích thước nằm giữa (l) và (r), đồng thời giảm thiểu tổng mức lương được xác định theo bậc của mỗi đỉnh. 

Nếu đỉnh (i) nhận bậc (d_i) thì đóng góp của nó là (p_{i,d_i}). Do đó, cấu trúc đồ thị thực tế chỉ quan trọng thông qua hai điều: bậc của mỗi đỉnh quyết định chi phí, trong khi cách các bậc đó được kết nối sẽ xác định mức độ phù hợp tối đa. 

Các giới hạn (n,m\le30) là đầu mối chính. Hàm mũ của thuật toán tính bằng (m) hoặc (n) đã quá lớn, bởi vì (30) đủ lớn đến mức (2^{30}) là khoảng một tỷ. Mặt khác, thuật toán đa thức với một số chiều giới hạn bởi (30) là thực tế. Giải pháp dự định sử dụng một số bộ đếm từ (0) đến (n) hoặc (m), đưa ra chương trình động (O(n^3m^3)). Việc triển khai C++ được chấp nhận sử dụng chính xác cách tiếp cận tiệm cận này. 

Có một ranh giới đầu vào hơi đáng ngạc nhiên. Các ví dụ chứa (m=0), mặc dù một số bản sao của câu lệnh mô tả (m) cùng với giới hạn dưới dương. Việc triển khai sẽ hỗ trợ một cách tự nhiên (m=0). Ví dụ,```
2 0 2 2
8
9
3
4
```không có cạnh nào cả, vì vậy kết quả khớp tối đa của nó là (0), không phải (2). Đầu ra đúng là```
DEFEAT
```Một giải pháp bất cẩn giả định ít nhất một vấn đề và cố gắng xây dựng một sự so khớp một cách mù quáng có thể thất bại ở đây. 

Một trường hợp cạnh khác là (l=0). Nếu khoảng yêu cầu chứa số 0, thì đồ thị không cạnh được cho phép bất cứ khi nào (m=0). Ví dụ,```
1 0 0 0
7
9
```có câu trả lời (16), vì cả hai thành viên đều nhận được độ 0 và mức độ phù hợp tối đa là 0. Giải pháp luôn cố gắng tạo kết quả khớp có kích thước dương sẽ báo cáo lỗi không chính xác. 

Giới hạn trên của sự phù hợp cũng có vấn đề. Ví dụ,```
2 1 2 2
0 0
0 0
0 0
0 0
```chỉ chứa một vấn đề, do đó chỉ có một cạnh, do đó việc khớp kích thước (2) là không thể. Đầu ra đúng là`DEFEAT`. Một kiểm tra ngây thơ chỉ dựa trên số lượng thành viên sẵn có có thể nghĩ sai rằng hai thành viên mỗi bên là đủ. 

Cuối cùng, các cạnh song song không được coi là cơ hội kết hợp riêng biệt. Với```
2 2 2 2
0 0 0
0 0 0
0 0 0
0 0 0
```cả hai bài toán đều có thể kết nối cùng một cặp phần tử, nhưng hai bài toán đó vẫn chỉ tạo ra một bài toán giải được, bởi vì hai phần tử giống nhau không thể chọn hai bài toán khác nhau. Biểu đồ có hai cạnh song song, nhưng mức khớp tối đa của nó là (1). Bất kỳ triển khai nào xử lý các vấn đề (m) dưới dạng các cặp có thể so khớp độc lập sẽ mắc lỗi này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là quyết định hai điểm cuối của mọi vấn đề. Có (n^2) cặp có thể cho một bài toán, vì vậy việc liệt kê tất cả các bài tập sẽ cân nhắc 

[ 
(n^2)^m=n^{2m} 
] 

đồ thị. Đối với các giá trị tối đa (n=m=30), đây là khả năng (900^{30}), gần đúng (10^{88}), thậm chí trước khi tính toán mức khớp tối đa của mỗi biểu đồ. Cách tiếp cận này đúng vì mọi nhiệm vụ khả thi cuối cùng đều được xem xét, nhưng nó gần như trở nên vô dụng ngay lập tức. 

Quan sát hữu ích là mức lương chỉ phụ thuộc vào độ đỉnh. Chúng ta nên tránh quyết định điểm cuối chính xác cho đến phút cuối cùng. Thay vào đó, chúng ta có thể mô tả một biểu đồ bằng một bìa đỉnh và đối sánh được lựa chọn cẩn thận, bởi vì đồ thị hai bên có đặc tính là kích thước của một đối sánh tối đa bằng với kích thước của bìa đỉnh tối thiểu. Đây là định lý Kőnig. 

Giả sử chúng ta muốn kết quả khớp tối đa cuối cùng chính xác là (k). Chúng ta có thể chọn rõ ràng (k) các cạnh phù hợp và một bìa đỉnh chứa chính xác (k) đỉnh. Mỗi cạnh phù hợp phải chứa chính xác một đỉnh che. Mỗi cạnh khác phải chứa ít nhất một đỉnh che. Khi đó, bìa được chọn có kích thước (k), do đó không có kết quả khớp nào có thể có nhiều hơn (k) cạnh, trong khi kết quả khớp được xây dựng rõ ràng có (k) cạnh. Do đó, kết quả khớp tối đa chính xác là (k). 

Đây là mức giảm chính. Thay vì suy luận về khả năng kết nối đồ thị tùy ý, chúng ta chỉ cần quyết định, đối với mỗi thành viên, mức độ của nó, liệu nó có phải là điểm cuối của một trong các cạnh khớp (k) hay không và liệu nó có thuộc về lớp phủ hay không. 

Đối với phía bên trái, xác định (x_1) là số lượng điểm cuối phù hợp, (x_2) là số lượng các điểm cuối phù hợp thuộc về trang bìa, (x_3) là số cạnh không khớp liên quan đến phía bên trái và (x_4) là số lượng các trường hợp không khớp có điểm cuối bên trái nằm trong trang bìa. Xác định (y_1,y_2,y_3,y_4) tương tự ở bên phải. 

Cuối cùng chúng ta cần 

[ 
x_1=y_1=k, 
] 

bởi vì khớp có (k) cạnh và mỗi cạnh khớp có một điểm cuối ở mỗi bên. Chúng tôi cũng cần 

[ 
x_2+y_2=k, 
] 

bởi vì mỗi cạnh khớp phải có chính xác một điểm cuối trong trang bìa. Có (m-k) các cạnh không khớp nhau, vì vậy 

[ 
x_3=y_3=m-k. 
] 

Cuối cùng, mọi cạnh không khớp đều phải chạm vào nắp. Các đại lượng (x_4) và (y_4) tính bao gồm các trường hợp xảy ra trên các cạnh đó. Một cạnh có hai điểm cuối đều nằm trong lớp phủ đóng góp hai lần, do đó điều kiện cần và đủ là 

[ 
x_4+y_4\ge m-k. 
] 

DP độc lập tìm các phép gán bên trái và bên phải rẻ nhất thỏa mãn các bộ đếm này. Sau đó chúng tôi kết hợp các trạng thái tương thích. 

Bài xã luận ban đầu mô tả bốn bộ đếm giống nhau và (O(n^3m^3)) DP, theo sau là một quy trình mang tính xây dựng nhằm hiện thực hóa thông tin cấp độ đã chọn dưới dạng các cạnh thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^{2m}\cdot n^3)) | (O(n^2+m)) | Quá chậm | 
| DP tối ưu | (O(n^3m^3)) | (O(n^3m^2)) cho biểu diễn Python thưa thớt | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Giải thích các vấn đề dưới dạng các cạnh của đa đồ thị hai bên. Bằng cấp của một thành viên chính xác là số bài toán được giao cho thành viên đó nên việc chọn bằng cấp quyết định mức lương đóng góp. 
2. Cố định kích thước khớp tối đa mong muốn (k) bằng (l\le k\le r). Chúng ta sẽ xây dựng một cách rõ ràng sự khớp của các cạnh (k) và một bìa đỉnh của các đỉnh (k). Theo định lý Kőnig, việc làm cho cả hai cấu trúc có kích thước (k) là đủ để buộc kết quả khớp tối đa phải chính xác (k). 
3. Đối với mỗi bên, xử lý từng đỉnh (n) của nó bằng một chương trình động. Trạng thái là ((x_1,x_2,x_3,x_4)). Ở đây (x_1) tính các điểm cuối phù hợp đã chọn, (x_2) tính các điểm cuối phù hợp là các đỉnh che, (x_3) tính các trường hợp cạnh không khớp và (x_4) tính các trường hợp có điểm cuối nằm trong lớp phủ. 
4. Khi xử lý một đỉnh, gọi (c) là số cạnh liên quan không khớp của nó. Có ba vai trò có thể. Đỉnh có thể nằm ngoài khớp và bên ngoài lớp phủ, cho độ (c). Nó có thể là điểm cuối phù hợp nhưng không phải là đỉnh che, cho mức độ (c+1). Hoặc nó có thể vừa là điểm cuối phù hợp vừa là đỉnh che, một lần nữa cho mức độ (c+1), trong khi các cạnh không khớp (c) của nó cũng đóng góp vào (x_4). 
5. Thêm mức lương tương ứng (p_{i,d}) vào chi phí DP. Vì mức lương chỉ phụ thuộc vào mức độ kết quả (d), nên đích đến chính xác của các cạnh không quan trọng trong DP. 
6. Sau khi xử lý xong tất cả các đỉnh ở một bên, hãy kết hợp cả hai bên. Đối với (k) cố định, yêu cầu cả hai bên phải có (k) điểm cuối trùng khớp và các trường hợp không khớp chính xác (m-k). Các trạng thái ghép nối có số lần che phủ trên các điểm cuối trùng khớp có tổng bằng (k) và có số lần che phủ trên các cạnh không khớp có tổng ít nhất là (m-k). 
7. Giữ nguyên tổng mức lương tối thiểu trong số tất cả các bang tương thích. Nếu không có cặp tương thích nào tồn tại cho bất kỳ (k\in[l,r] nào), hãy in`DEFEAT`. 
8. Khôi phục cấp độ và vai trò đã chọn của mọi đỉnh từ con trỏ cha DP. Thông tin được khôi phục cho chúng ta biết đỉnh nào phù hợp với điểm cuối, điểm nào trong số đó là đỉnh che phủ và cấp độ cuối cùng của mỗi đỉnh. 
9. Trước tiên hãy tạo các cạnh phù hợp (k). Một đỉnh phù hợp bên trái bên ngoài bìa được ghép với một đỉnh phù hợp bên phải bên trong bìa. Một đỉnh phù hợp bên trái bên trong bìa được ghép với một đỉnh phù hợp bên phải bên ngoài bìa. Hai nhóm có kích thước chính xác bằng nhau vì (x_2+y_2=k). 
10. Một số cạnh không khớp cần có cả hai điểm cuối trên trang bìa. Số lượng các cạnh như vậy được xác định bởi độ còn lại của các đỉnh che phủ sau khi các cạnh khớp đã được đặt. Kết nối các đỉnh bìa ở hai bên cho đến khi tạo được số cạnh được phủ kép theo yêu cầu này. 
11. Bây giờ, tất cả các độ còn lại có thể được thỏa mãn bằng cách sử dụng các cạnh giữa một đỉnh che ở một bên và một đỉnh phù hợp không che ở phía bên kia. Mỗi cạnh còn lại có chính xác một điểm cuối che phủ, do đó mọi cạnh đều được che phủ. 
12. Xuất ra cặp (m) kết quả. Cho phép các cạnh song song nên việc xây dựng không cần tránh sử dụng cùng một cặp nhiều lần. 

Điều bất biến đằng sau DP là mọi trạng thái được lưu trữ đều mô tả việc gán một phần có thể thực hiện được của các đỉnh được xử lý đầu tiên với các trường hợp trùng khớp và bao trùm được ghi lại chính xác cũng như với mức lương tối thiểu có thể có cho các bộ đếm đó. Khi hai trạng thái cuối cùng thỏa mãn bốn phương trình tương thích, cấu trúc sẽ tạo ra sự phù hợp về kích thước (k) và lớp phủ kích thước (k). Việc so khớp chứng tỏ rằng biểu đồ có số lượng trùng khớp ít nhất (k), trong khi bìa chứng tỏ rằng nó có số lượng trùng khớp nhiều nhất (k). Do đó, kết quả khớp tối đa của nó chính xác là (k) và bởi vì (k\in[l,r]), biểu đồ là hợp lệ. Vì DP giảm thiểu mức lương cho mọi tiểu bang và phép liệt kê cuối cùng xem xét mọi cặp tiểu bang tương thích nên biểu đồ được chọn có chi phí tối thiểu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def dp_side(cost, n, m, r):
    """
    DP over one side.

    State:
        (x1, x2, x3, x4)

    x1 = number of matching endpoints
    x2 = number of matching endpoints in the cover
    x3 = number of nonmatching edge incidences
    x4 = number of those incidences whose endpoint is in the cover

    Returns:
        dp      : final-state -> minimum cost
        parents : parent information for reconstruction
    """

    dp = {(0, 0, 0, 0): 0}
    parents = [None] * (n + 1)
    parents[0] = {}

    for i in range(1, n + 1):
        ndp = {}
        par = {}

        for state, old_cost in dp.items():
            x1, x2, x3, x4 = state

            remaining = m - x1 - x3

            for c in range(remaining + 1):
                nx3 = x3 + c

                # Vertex is neither a matching endpoint nor a cover vertex.
                ns = (x1, x2, nx3, x4)
                value = old_cost + cost[i - 1][c]

                if value < ndp.get(ns, INF):
                    ndp[ns] = value
                    par[ns] = (state, 1, c)

                # The vertex is a matching endpoint, but not in the cover.
                if x1 < r and remaining - c > 0:
                    ns = (x1 + 1, x2, nx3, x4)
                    value = old_cost + cost[i - 1][c + 1]

                    if value < ndp.get(ns, INF):
                        ndp[ns] = value
                        par[ns] = (state, 2, c + 1)

                    # The vertex is both a matching endpoint and a cover vertex.
                    ns = (x1 + 1, x2 + 1, nx3, x4 + c)
                    value = old_cost + cost[i - 1][c + 1]

                    if value < ndp.get(ns, INF):
                        ndp[ns] = value
                        par[ns] = (state, 3, c + 1)

        dp = ndp
        parents[i] = par

    return dp, parents

def reconstruct(parents, final_state, n):
    degree = [0] * n
    matched = [False] * n
    cover = [False] * n

    state = final_state

    for i in range(n, 0, -1):
        prev, kind, d = parents[i][state]

        degree[i - 1] = d

        if kind == 2:
            matched[i - 1] = True
        elif kind == 3:
            matched[i - 1] = True
            cover[i - 1] = True

        state = prev

    return degree, matched, cover

def solve_case(n, m, l, r, costs):
    left = costs[:n]
    right = costs[n:]

    left_dp, left_parents = dp_side(left, n, m, r)
    right_dp, right_parents = dp_side(right, n, m, r)

    best = INF
    best_states = None

    max_k = min(r, n, m)

    for k in range(l, max_k + 1):
        nonmatching = m - k

        for x2 in range(k + 1):
            y2 = k - x2

            for x4 in range(nonmatching + 1):
                min_y4 = nonmatching - x4

                for y4 in range(min_y4, nonmatching + 1):
                    ls = (k, x2, nonmatching, x4)
                    rs = (k, y2, nonmatching, y4)

                    lc = left_dp.get(ls)
                    rc = right_dp.get(rs)

                    if lc is None or rc is None:
                        continue

                    value = lc + rc

                    if value < best:
                        best = value
                        best_states = (ls, rs)

    if best_states is None:
        return None

    left_state, right_state = best_states

    left_degree, left_matched, left_cover = reconstruct(
        left_parents, left_state, n
    )
    right_degree, right_matched, right_cover = reconstruct(
        right_parents, right_state, n
    )

    # Vectors are indexed by cover status and matching status.
    groups = [[[], []], [[], []]]

    for i in range(n):
        if left_matched[i]:
            groups[0][1 if left_cover[i] else 0].append(i)
        if right_matched[i]:
            groups[1][1 if right_cover[i] else 0].append(i)

    edges = []

    def add_edge(u, v):
        edges.append((u + 1, v + n + 1))
        left_degree[u] -= 1
        right_degree[v] -= 1

    # Construct the k matching edges.
    #
    # Left non-cover matching vertices pair with right cover
    # matching vertices, and vice versa.
    if len(groups[0][0]) != len(groups[1][1]):
        raise AssertionError("invalid matching partition")
    if len(groups[0][1]) != len(groups[1][0]):
        raise AssertionError("invalid matching partition")

    for u, v in zip(groups[0][0], groups[1][1]):
        add_edge(u, v)

    for u, v in zip(groups[0][1], groups[1][0]):
        add_edge(u, v)

    # Rebuild groups using remaining degrees.
    rem_groups = [[[], []], [[], []]]

    for side in range(2):
        for i in range(n):
            if side == 0:
                d = left_degree[i]
                is_cover = left_cover[i]
            else:
                d = right_degree[i]
                is_cover = right_cover[i]

            if d > 0:
                rem_groups[side][1 if is_cover else 0].append(i)

    # First create edges covered at both endpoints.
    #
    # The amount is exactly the excess cover incidence after all
    # edges with one cover endpoint are accounted for.
    left_cover_degree = sum(
        left_degree[i] for i in range(n) if left_cover[i]
    )
    right_noncover_degree = sum(
        right_degree[i] for i in range(n) if not right_cover[i]
    )

    double_edges = left_cover_degree - right_noncover_degree

    p = rem_groups[0][1]
    q = rem_groups[1][1]

    while double_edges > 0:
        if not p or not q:
            raise AssertionError("failed to construct double-covered edges")

        u = p[-1]
        v = q[-1]
        add_edge(u, v)
        double_edges -= 1

        if left_degree[u] == 0:
            p.pop()
        if right_degree[v] == 0:
            q.pop()

    # Finish all remaining edges. Every such edge has exactly one
    # cover endpoint.
    for side in range(2):
        p = rem_groups[side][0]
        q = rem_groups[1 - side][1]

        while p:
            if not q:
                raise AssertionError("failed to construct remaining edges")

            if side == 0:
                u = p[-1]
                v = q[-1]
                add_edge(u, v)
            else:
                u = q[-1]
                v = p[-1]
                add_edge(u, v)

            if left_degree[u] == 0:
                p.pop()
            if right_degree[v] == 0:
                q.pop()

    if len(edges) != m:
        raise AssertionError("wrong number of edges")

    if any(left_degree) or any(right_degree):
        raise AssertionError("degrees were not fully constructed")

    return best, edges

def solve():
    n, m, l, r = map(int, input().split())

    costs = []
    for _ in range(2 * n):
        costs.append(list(map(int, input().split())))

    result = solve_case(n, m, l, r, costs)

    if result is None:
        print("DEFEAT")
        return

    answer, edges = result

    out = [str(answer)]
    out.extend(f"{u} {v}" for u, v in edges)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của việc thực hiện,`dp_side`, xử lý một nhóm một cách độc lập. Bốn bộ đếm được lưu trữ dưới dạng một bộ dữ liệu và một từ điển chỉ giữ các trạng thái thực sự có thể truy cập được. Điều này đặc biệt hữu ích trong Python vì một mảng sáu chiều đầy đủ sẽ yêu cầu một lượng lớn chi phí đối tượng. 

Đối với một đỉnh có (c) các cạnh không khớp, phép chuyển đổi đầu tiên gán cho nó bậc (c). Hai chuyển đổi còn lại gán bậc (c+1), vì đỉnh còn nhận thêm một cạnh phù hợp. Quá trình chuyển đổi thứ ba cũng làm tăng bộ đếm tỷ lệ che phủ lên (c), vì tất cả các cạnh không khớp đó đều có điểm cuối che phủ tại đỉnh này. 

điều kiện`remaining - c > 0`trước khi quá trình chuyển đổi khớp sẽ ngăn DP tạo cạnh khớp sau khi tất cả (m) vị trí cạnh đã được sử dụng. Đây là điều kiện biên dễ mắc sai lầm nhất. Điểm cuối phù hợp luôn cần thêm một cạnh ngoài (c) các cạnh không khớp của nó. 

Việc xây dựng lại đi ngược lại qua các từ điển gốc được lưu trữ. Đối với mỗi thành viên được xử lý, nó sẽ khôi phục mức độ của nó, cho dù đó có phải là điểm cuối phù hợp hay không và liệu đó có phải là đỉnh che phủ hay không. 

Việc liệt kê trạng thái cuối cùng sử dụng 

[ 
(k,x_2,m-k,x_4) 
] 

ở bên trái và 

[ 
(k,k-x_2,m-k,y_4) 
] 

ở bên phải. Giới hạn dưới 

[ 
y_4\ge m-k-x_4 
] 

chính xác là yêu cầu rằng các cạnh không khớp (m-k) phải có ít nhất một điểm cuối che mỗi cạnh. 

Việc xây dựng có chủ ý tạo ra sự phù hợp trước tiên. Sau khi các cạnh (k) đó được loại bỏ khỏi các yêu cầu về mức độ còn lại, tổng mức độ còn lại ở cả hai bên đều bằng nhau. Một số cạnh còn lại phải được phủ hai lần và chúng được đặt giữa các đỉnh của lớp phủ. Sau khi loại bỏ những cạnh đó, mọi cạnh còn lại có thể được đặt giữa một đỉnh che và một đỉnh không che. Các cạnh song song là hoàn toàn hợp pháp, do đó không cần hạn chế bổ sung. 

Số nguyên Python có độ chính xác tùy ý, do đó, các giá trị lương lớn tới (10^9) và tổng của chúng trên tối đa (2n) thành viên, không yêu cầu bất kỳ xử lý tràn đặc biệt nào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là```
2 0 2 2
8
9
3
4
```Không có vấn đề gì nên mọi đỉnh đều có độ 0. Biểu đồ duy nhất có thể có mức khớp tối đa (0). 

Trạng thái DP cuối cùng có liên quan là trạng thái 0 ở cả hai bên. 

| Số lượng | Trái | Đúng | 
| --- | --- | --- | 
| Điểm cuối phù hợp | 0 | 0 | 
| Bao gồm các điểm cuối phù hợp | 0 | 0 | 
| Các trường hợp không phù hợp | 0 | 0 | 
| Bao gồm các trường hợp không phù hợp | 0 | 0 | 
| Chi phí | 17 | 7 | 

Kích thước khớp mong muốn phải nằm trong khoảng (2) và (2), nhưng giá trị duy nhất có thể là (0). Không tồn tại trạng thái cuối cùng tương thích nên thuật toán sẽ in`DEFEAT`. 

Ví dụ này xác nhận rằng DP không phát minh ra các cạnh khi (m=0) và giới hạn dưới được yêu cầu được kiểm tra dựa trên kích thước khớp thực tế. 

### Mẫu 2 

Mẫu thứ hai có (n=2), (m=8) và (l=r=2). Một mô hình mức độ tối ưu là 

[ 
d_L=(4,4),\qquad d_R=(5,3), 
] 

chi phí của ai là 

[ 
p_{1,4}+p_{2,4}+p_{3,5}+p_{4,3} 
=-10+0-9-2=-21. 
] 

Trạng thái cuối cùng tương ứng có thể được mô tả như sau. 

| Số lượng | Trái | Đúng | 
| --- | --- | --- | 
| (k) | 2 | 2 | 
| Điểm cuối phù hợp (x_1,y_1) | 2 | 2 | 
| Khớp các điểm cuối trong bìa (x_2,y_2) | 2 | 0 | 
| Sự cố không khớp (x_3,y_3) | 6 | 6 | 
| Các trường hợp không khớp được đề cập (x_4,y_4) | 6 | 0 | 
| Chi phí | -10 | -11 | 

Kích thước phù hợp là (k=2). Cả hai đỉnh khớp bên trái đều nằm trong bìa, vì vậy mọi cạnh không khớp cũng có thể được che từ bên trái. Vì có (m-k=6) cạnh không khớp và phía bên trái cung cấp sáu trường hợp che phủ, nên cả sáu trường hợp đều được che phủ. 

Đầu ra mẫu sử dụng bốn bản sao của cạnh ((1,3)), ba cạnh liên quan đến đỉnh (2) và đỉnh (4) và một cạnh ((2,3)). Trình tự độ của nó chính xác như trình tự ở trên và hai đỉnh bên trái là các đỉnh bên trái duy nhất khác 0, do đó kết quả khớp tối đa là chính xác (2). Mẫu chính thức đưa ra tổng chi phí (-21). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3m^3)) | Có (O(n^2m^2)) trạng thái DP trên bốn bộ đếm và các lựa chọn độ lên tới (O(m)) cho mỗi lần chuyển đổi, cho mỗi (n) đỉnh và hai cạnh | 
| Không gian | (O(n^3m^2)) | Việc triển khai Python lưu trữ các trạng thái có thể truy cập và thông tin tái cấu trúc cho tất cả (n) lớp | 

Các giới hạn dự định chỉ là (n,m\le30), điều này làm cho công thức sáu bộ đếm trở nên thực tế khi triển khai ở mức độ thấp. Giới hạn thời gian chính thức là 2 giây và giới hạn bộ nhớ là 1024 MiB. 

Các triển khai được chấp nhận ban đầu sử dụng cùng một DP (O(n^3m^3)) và lưu trữ cấu trúc DP đầy đủ trong mảng. Phiên bản Python sử dụng các từ điển thưa thớt để tránh phân bổ một mảng đối tượng đa chiều khổng lồ, đánh đổi một số tốc độ hệ số không đổi để quản lý bộ nhớ đơn giản hơn đáng kể. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới giả định mã đã gửi được lưu dưới dạng`solution.py`. Nó kiểm tra giá trị câu trả lời chính xác, đồng thời cho phép mọi cách xây dựng hợp lệ vì bài toán cho phép rõ ràng các giải pháp tối ưu tùy ý.```python
# helper: run solution on input string, return output string
import subprocess
import sys

def run(inp: str) -> str:
    result = subprocess.run(
        [sys.executable, "solution.py"],
        input=inp.encode(),
        stdout=subprocess.PIPE,
        check=True,
    )
    return result.stdout.decode().strip()

sample1 = """\
2 0 2 2
8
9
3
4
"""

assert run(sample1) == "DEFEAT", "sample 1"

sample2 = """\
2 8 2 2
2 5 5 10 -10 -1 3 5 9
8 -10 9 9 0 1 -3 1 -1
0 5 -1 5 3 -9 1 10 6
5 -4 8 -2 2 -8 6 3 -3
"""

out = run(sample2).splitlines()
assert int(out[0]) == -21, "sample 2"

sample3 = """\
3 5 2 3
100 75 125 150 175 200
125 100 75 100 125 150
225 200 175 200 225 250
225 200 175 200 225 250
125 100 75 100 125 150
100 75 125 150 175 200
"""

out = run(sample3).splitlines()
assert int(out[0]) == 650, "sample 3"

# Minimum-size case: no problems, matching number must be zero.
case_min = """\
1 0 0 0
7
9
"""

assert run(case_min) == "16", "minimum-size case"

# Boundary case: one edge cannot create a matching of size two.
case_boundary = """\
2 1 2 2
0 0
0 0
0 0
0 0
"""

assert run(case_boundary) == "DEFEAT", "matching upper-bound case"

# All costs are equal, so every feasible construction has the same cost.
case_equal = """\
2 2 1 1
5 5 5
5 5 5
5 5 5
5 5 5
"""

out = run(case_equal).splitlines()
assert int(out[0]) == 20, "all-equal costs"

# Maximum-size instance. With 30 problems and a required matching
# of 30, every one of the 60 vertices must have degree exactly one.
rows = ["0 1" for _ in range(60)]
case_max = "30 30 30 30\n" + "\n".join(rows) + "\n"

out = run(case_max).splitlines()
assert int(out[0]) == 60, "maximum-size case"
assert len(out) == 31, "maximum-size edge count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`DEFEAT`| Không có vấn đề gì và giới hạn kết hợp thấp hơn không thể | 
| Mẫu 2 |`-21`| Chi phí tối ưu và kết cấu cạnh song song không cần thiết | 
| Mẫu 3 |`650`| công trình khả thi chính thức thứ hai | 
| (n=1,m=0,l=r=0) |`16`| Trường hợp có kích thước tối thiểu và không khớp | 
| (n=2,m=1,l=r=2) |`DEFEAT`| Phù hợp với giới hạn trên và xử lý từng cái một | 
| (n=2,m=2,l=r=1), mọi chi phí (5) |`20`| Chi phí hoàn toàn bằng nhau và sự kết hợp không hoàn hảo có chủ ý | 
| (n=m=30,l=r=30) |`60`| Ranh giới kích thước tối đa và kết hợp hoàn hảo | 

## Vỏ cạnh 

Đối với trường hợp không có vấn đề,```
1 0 0 0
7
9
```DP bắt đầu và kết thúc tại ((0,0,0,0)). Không có lựa chọn chuyển tiếp vì không có cạnh nào để phân phối. Chi phí bên trái là (7), chi phí bên phải là (9) và kích thước phù hợp là (0), thuộc về khoảng được yêu cầu. Câu trả lời là (16). 

Đối với một giới hạn dưới không thể,```
2 1 2 2
0 0
0 0
0 0
0 0
```DP không thể tạo ra trạng thái cuối cùng với (k=2), vì điểm cuối phù hợp sẽ sử dụng một cạnh hoàn chỉnh và chỉ có một cạnh khả dụng. Bảng liệt kê cuối cùng không có trạng thái ứng cử viên, vì vậy`DEFEAT`được sản xuất. 

Đối với các cạnh lặp lại, hãy xem xét```
2 2 1 1
5 5 5
5 5 5
5 5 5
5 5 5
```Cả hai vấn đề có thể được gán cho cùng một cặp. Cả hai độ trái và phải đều chứa một đỉnh bậc hai và một đỉnh bậc 0. Biểu đồ có hai cạnh song song nhưng chỉ có một cặp có thể khớp, do đó mức khớp tối đa của nó là (1). DP cho phép điều này vì nó ghi lại độ thay vì cấm các cặp lặp lại. Tổng chi phí là (20). 

Đối với ranh giới phù hợp tối đa,```
30 30 30 30
```với mỗi hàng lương bằng`0 1`, cần phải có kích thước phù hợp (30). Vì chỉ có (30) bài toán nên mọi bài toán đều phải thuộc dạng khớp, nên mỗi một trong (60) đỉnh có đúng một bậc. Tổng tiền lương do đó là (60). DP đạt đến (k=30) và (m-k=0), do đó các bộ đếm không khớp đều bằng 0. Điều này thực hiện ranh giới chính xác mà không có cạnh dư nào có thể được tạo ra. 

Đối với mức lương âm, DP không bao giờ được cho rằng bằng cấp lớn hơn sẽ đắt hơn. Trong mẫu chính thức thứ hai, một số mục là âm và mẫu tối ưu có chủ ý gán độ bốn cho hai đỉnh bên trái đầu tiên và độ năm cho một đỉnh bên phải. Một chiến lược tham lam dựa trên việc lựa chọn mức độ rẻ nhất một cách độc lập cho từng thành viên sẽ thất bại vì các mức độ phải có tổng bằng (m) ở mỗi bên và phải đồng thời hỗ trợ cấu trúc bao phủ và đối chiếu cần thiết. DP xem xét tất cả các lựa chọn bằng cấp tương thích và giảm thiểu tổng chi phí của chúng. Câu trả lời chính thức cho mẫu này là (-21). 

Trường hợp cạnh khái niệm chính là một biểu đồ có độ khớp tối đa nhỏ hơn số đỉnh khác 0 độ ở hai bên. Chỉ đếm các đỉnh hoạt động là không đủ để xác định số lượng phù hợp. Phần bìa của DP xử lý chính xác tình huống này: một số đỉnh hoạt động có thể bị ép vào một cấu trúc chỉ được bao phủ bởi (k) đỉnh, điều này giới hạn mức khớp tối đa là (k). Đây là lý do tại sao chỉ theo dõi độ hoặc chỉ số đỉnh khác 0 sẽ làm mất thông tin cần thiết. 

Cấu trúc cuối cùng cũng xử lý các cạnh được bao phủ ở cả hai điểm cuối. Một cạnh như vậy đóng góp hai trường hợp che phủ, do đó (x_4+y_4) có thể lớn hơn (m-k). Việc xây dựng trước tiên tạo ra chính xác số lượng các cạnh được phủ kép này theo yêu cầu, sau đó phân phối tất cả các mức còn lại bằng cách sử dụng các cạnh có một điểm cuối của lớp phủ. Đây là điều làm cho chuỗi bậc cuối cùng đồng thời thỏa mãn mức lương DP và điều kiện che đỉnh. 

Ý tưởng cốt lõi để giải quyết các vấn đề tương tự là ngừng suy nghĩ về biểu đồ chính xác trước tiên. Mức lương quan tâm đến mức độ, trong khi ràng buộc so khớp có thể được chứng nhận bằng cách ghép một kết quả khớp với một bìa đỉnh. Khi hai cấu trúc đó được mã hóa bằng các bộ đếm nhỏ, bản thân biểu đồ có thể được xây dựng lại sau đó.
