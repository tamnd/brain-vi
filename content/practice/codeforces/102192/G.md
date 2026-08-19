---
title: "CF 102192G - Trò Chơi Bài"
description: "Mỗi thẻ có hai số, chẳng hạn như (x) ở mặt trước hiện tại và (y) ở mặt sau. Lật thẻ sẽ thay đổi số nào trong hai số này được hiển thị. Chúng ta cần mỗi số nhìn thấy được phải khác nhau, đồng thời lật càng ít thẻ càng tốt."
date: "2026-08-18T20:27:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "G"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 242
verified: true
draft: false
---

[CF 102192G - Trò chơi bài](https://codeforces.com/problemset/problem/102192/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi thẻ có hai số, chẳng hạn như (x) ở mặt trước hiện tại và (y) ở mặt sau. Lật thẻ sẽ thay đổi số nào trong hai số này được hiển thị. Chúng ta cần mỗi số nhìn thấy được phải khác nhau, đồng thời lật càng ít thẻ càng tốt. Trong số tất cả các giải pháp lật bài tối thiểu, chúng ta cũng cần đếm xem có thể lật được bao nhiêu bộ bài khác nhau. Hai giải pháp chỉ được coi là giống nhau khi chúng lật giống hệt nhau chỉ số quân bài. 

Biểu diễn hữu ích là một biểu đồ. Coi mọi số riêng biệt là một đỉnh và mọi thẻ ((x,y)) là cạnh vô hướng giữa (x) và (y). Số hiển thị hiện tại cho chúng ta biết điểm cuối của cạnh được chọn. Vì vậy, mỗi thẻ ban đầu đều hướng về số nhìn thấy được của nó. Sau khi lật một lá bài, cạnh của nó sẽ hướng về điểm cuối còn lại. 

Một số xuất hiện ở mặt trước của nhiều thẻ một cách chính xác khi một số cạnh hướng về cùng một đỉnh. Do đó, yêu cầu tất cả các số nhìn thấy được là duy nhất chính xác là yêu cầu mỗi đỉnh có nhiều nhất một bậc trong hướng cuối cùng. 

Đầu vào cho phép (n) đạt (10^5) và tổng (n) trên tất cả các trường hợp thử nghiệm đạt (10^6). Giải pháp (O(n^2)) đã quá chậm đối với các thử nghiệm lớn nhất, trong khi tìm kiếm theo cấp số nhân là hoàn toàn không khả thi. Giải pháp dự định phải xử lý mọi thẻ và mọi số chỉ với một số lần không đổi, đưa ra giải pháp (O(n)) cho mỗi trường hợp thử nghiệm. 

Một số trường hợp rất dễ xử lý sai. 

Coi như```
1
1
1 1
```Câu trả lời là```
0 1
```Lá bài là một vòng tự lặp, nhưng cả hai mặt đều chứa cùng một số nên việc lật nó không thay đổi gì. Việc triển khai biểu đồ bất cẩn coi vòng lặp là một cạnh có thể đảo ngược thông thường có thể được tính là hướng thứ hai giả. 

Coi như```
1
2
1 2
1 3
```Câu trả lời là```
1 2
```Đồ thị là một cái cây. Chọn đỉnh 2 làm gốc yêu cầu lật lá bài thứ hai, trong khi chọn đỉnh 3 làm gốc yêu cầu lật lá bài thứ nhất. Cả hai giải pháp đều sử dụng một lần lật. Chỉ nhìn vào con số hiện đang bị trùng lặp và sửa nó một cách tham lam có thể bỏ lỡ một trong hai lựa chọn tối ưu toàn cầu này. 

Coi như```
1
3
1 2
2 3
1 3
```Đồ thị là một chu trình đơn. Có chính xác hai hướng hợp lệ có thể có của chu trình. Trong ví dụ này cả hai đều yêu cầu một lần lật, vì vậy câu trả lời là```
1 2
```Một giải pháp giả định mỗi chu kỳ chỉ có một hướng tối ưu sẽ tính sai. 

Cuối cùng, hãy xem xét```
1
2
1 1
1 1
```Câu trả lời là```
-1 -1
```Có hai thẻ nhưng chỉ có một số sử dụng được. Tổng quát hơn, một thành phần liên thông chứa nhiều cạnh hơn số đỉnh không thể được định hướng sao cho mỗi đỉnh nhận được nhiều nhất một cạnh đến. Chỉ kiểm tra xem từng số trùng lặp có thể được sửa chữa cục bộ hay không là chưa đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là quyết định một cách độc lập cho mỗi quân bài xem nó có được lật hay không. Có (2^n) bộ lật có thể. Đối với mỗi bộ, chúng ta có thể kiểm tra tất cả (n) số hiển thị, kiểm tra xem chúng có khác biệt hay không và giữ số lần lật tối thiểu cũng như tần số của nó. Điều này đúng vì nó xem xét rõ ràng mọi trạng thái cuối cùng có thể có. Công việc trong trường hợp xấu nhất của nó là (O(n2^n)), đối với (n=10^5) là khoảng (10^5\cdot2^{100000}) hoạt động và vượt xa khả thi. 

Công thức đồ thị cho thấy tại sao không gian tìm kiếm có nhiều cấu trúc hơn (2^n). Mỗi lá bài sẽ trở thành một cạnh mà điểm cuối được chọn là số hiển thị của nó. Điều kiện cuối cùng chỉ đơn giản là mức độ nhiều nhất là một. 

Đối với một thành phần liên thông chứa (v) đỉnh và (e) cạnh, tổng của tất cả các bậc chính xác là (e). Vì mỗi đỉnh có thể có nhiều nhất một bậc nên chúng ta phải có (e\le v). Đồ thị vô hướng liên thông với (e<v) là một cây, trong khi đồ thị liên thông với (e=v) là một đồ thị một vòng. Nếu (e>v), thành phần đó là không thể. 

Điều này làm giảm vấn đề thành hai trường hợp rất có cấu trúc. 

Đối với một cây, mọi hướng hợp lệ đều có chính xác một đỉnh có bậc bằng 0, bởi vì có (v-1) cạnh và (v) đỉnh. Khi đỉnh đó được chọn làm gốc, mọi cạnh buộc phải hướng ra xa gốc. Chúng ta chỉ cần tìm nghiệm nào giảm thiểu số cạnh có hướng ban đầu không đồng nhất với hướng cưỡng bức này. Đây là một vấn đề lập trình động tái khởi động tiêu chuẩn. 

Đối với thành phần một vòng, mỗi đỉnh phải có đúng một bậc. Tất cả các cạnh của cây gắn liền với chu trình buộc phải hướng ra xa chu trình. Bản thân chu trình chỉ có hai hướng có thể, theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Chúng tôi tính toán chi phí lật kèo cho cả hai và giữ lại chi phí tốt hơn, tính cả hai khi chúng hòa. Vòng tự lặp là chu trình một đỉnh nhưng hai cạnh của nó giống hệt nhau nên nó chỉ đóng góp một tập hợp lật có thể có. 

Do đó, quan sát cấu trúc quan trọng là mọi thành phần có thể giải được là một cây hoặc một đồ thị một vòng. Chúng ta có thể tìm các chu trình bằng cách loại bỏ liên tục các đỉnh bậc một. Các cạnh bị loại bỏ sẽ tạo thành các cây gắn liền, trong khi các cạnh còn lại tạo thành các chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một đa giác vô hướng. Đỉnh (x) đại diện cho số (x) và thẻ (i=(x,y)) trở thành cạnh (i). Lưu trữ (x) làm điểm cuối được chọn bởi mặt trước ban đầu. Tự vòng lặp được cho phép. 
2. Coi trạng thái ban đầu là hướng của mọi cạnh về phía số trước hiện tại của nó. Trạng thái cuối cùng hợp lệ chính xác là một hướng trong đó mỗi đỉnh có nhiều nhất một bậc. Đối với mọi thành phần được kết nối, hãy đếm các đỉnh và cạnh của nó một cách khái niệm. Nếu thành phần nào đó có nhiều cạnh hơn số đỉnh, hãy xuất ra (-1\ -1), vì tổng độ của nó quá lớn để vừa với các đỉnh của nó. 
3. Liên tục loại bỏ các đỉnh bậc một. Khi một lá (v) được kết nối với lá lân cận còn lại (u) của nó bằng cạnh (e), hãy loại bỏ cạnh đó và ghi (u) làm cha của (v). Trong mọi hướng hợp lệ mà phần còn lại chứa gốc hoặc chu trình, cạnh cây này phải hướng từ (u) đến (v). Đóng góp của nó vào số lần lật là một đóng góp chính xác khi điểm cuối được chọn ban đầu là (u).

Quá trình bóc tách đồng thời xác định cấu trúc đồ thị. Trong một thành phần hợp lệ là cây, mọi cạnh cuối cùng đều bị loại bỏ. Trong thành phần một vòng hợp lệ, chính xác các cạnh của chu trình vẫn được giữ nguyên. 
4. Sau khi bong tróc, kiểm tra mức độ còn lại của từng đỉnh. Đỉnh còn lại trong thành phần hợp lệ phải có bậc chính xác là hai, tính một vòng tự lặp hai lần. Nếu một số đỉnh còn lại có bậc dương khác thì thành phần đó chứa nhiều hơn một chu trình và điều đó là không thể. 
5. Xử lý mọi thành phần được kết nối. Các đỉnh và cạnh được duyệt một lần để thu thập các đỉnh của nó và tổng chi phí đã đóng góp bởi các cạnh cây bị bóc tách của nó. Nếu không còn cạnh nào trong thành phần thì đó là một cây. Ngược lại, nó là một vòng. 
6. Đối với thành phần cây, quá trình bóc tách chỉ để lại đúng một đỉnh mà không có đỉnh gốc. Đỉnh đó là nghiệm tự nhiên. Cho phép`base`là tổng chi phí của tất cả các cạnh được bóc tách. Đây là chi phí khi đỉnh cuối cùng còn lại là gốc, bởi vì mọi cạnh cha-con được ghi lại đều có hướng từ cha mẹ tới con. 
7. Reroot cây từ gốc này. Giả sử một cạnh kết nối đỉnh gốc hiện tại (v) với đỉnh con của nó (u) và điểm cuối phía trước ban đầu là (x). Với gốc tại (v), hướng mong muốn là (v\to u), do đó cạnh này có giá ([x=v]). Sau khi di chuyển gốc tới (u), hướng mong muốn sẽ trở thành (u\to v), do đó chi phí sẽ trở thành ([x=u]). Vì thế 

[ 
chi phí[u]=chi phí[v]+[x=u]-[x=v]. 
] 

Mỗi cạnh thay đổi gốc chính xác một lần trong quá trình truyền tải này, do đó tất cả chi phí gốc có thể đạt được theo thời gian tuyến tính. Giữ mức tối thiểu và đếm xem có bao nhiêu rễ đạt được nó. 
8. Đối với thành phần một vòng, tất cả các cạnh của cây được bóc tách đều đã có hướng cưỡng bức. Tìm chu trình còn lại bằng cách lần theo các cạnh chưa bị xóa khỏi bất kỳ đỉnh còn lại nào. 
9. Đi theo chu trình theo một hướng. Nếu một cạnh chu kỳ đi từ (u) đến (v), hướng theo chiều kim đồng hồ của nó sẽ chọn (v), trong khi hướng ngược lại sẽ chọn (u). Thêm một vào chi phí tương ứng bất cứ khi nào điểm cuối phía trước ban đầu là điểm cuối đối diện. Đối với một chu trình có nhiều hơn một đỉnh, hai hướng này sẽ tạo ra các bộ thẻ lật khác nhau. Nếu chi phí của họ bằng nhau thì cả hai đều được tính. 
10. Đối với vòng lặp tự, chỉ có một hướng hiệu quả vì cả hai điểm cuối đều có cùng số lượng. Sự đóng góp của nó bằng không, và nó đóng góp theo một cách chứ không phải theo hai cách. 
11. Cộng số lần lật tối thiểu của tất cả các thành phần. Vì các lựa chọn trong các thành phần được kết nối khác nhau là độc lập, hãy nhân số lượng bộ lật tối ưu của chúng theo modulo (998244353). 

Bất biến đằng sau toàn bộ thuật toán là mỗi cạnh của cây được bóc tách chỉ có một hướng khả dĩ trong bất kỳ nghiệm hợp lệ nào sau khi cạnh còn lại được cố định. Trong một cái cây, quyền tự do duy nhất còn lại là sự lựa chọn từ gốc rễ. Trong thành phần một vòng, các cây gắn liền không có tự do và lựa chọn duy nhất còn lại là hướng của chu trình. Do đó, thuật toán liệt kê mọi hướng hợp lệ có thể có ở dạng nén mà không bao giờ liệt kê (2^n) bộ lật ban đầu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        m = 2 * n
        V = m + 1

        # Forward-star adjacency.
        head = array('i', [-1]) * V
        to = array('i', [0]) * (2 * n)
        nxt = array('i', [0]) * (2 * n)

        # For edge i, x[i] is the initially visible endpoint.
        x = array('i', [0]) * n
        y = array('i', [0]) * n

        degree = array('i', [0]) * V

        for i in range(n):
            a, b = map(int, input().split())
            x[i] = a
            y[i] = b

            p = 2 * i
            to[p] = b
            nxt[p] = head[a]
            head[a] = p

            to[p + 1] = a
            nxt[p + 1] = head[b]
            head[b] = p + 1

            if a == b:
                degree[a] += 2
            else:
                degree[a] += 1
                degree[b] += 1

        # parent[v] is the vertex that survived when v was peeled.
        parent = array('i', [-1]) * V

        # child_cost[v] is the cost of edge parent[v] -> v
        # in the orientation forced by the surviving side.
        child_cost = bytearray(V)

        removed = bytearray(n)

        # Peel all trees from the outside toward their roots/cycles.
        queue = []
        for v in range(1, V):
            if head[v] != -1 and degree[v] == 1:
                queue.append(v)

        qpos = 0
        while qpos < len(queue):
            v = queue[qpos]
            qpos += 1

            if degree[v] != 1:
                continue

            arc = head[v]
            while arc != -1:
                e = arc >> 1
                if not removed[e]:
                    break
                arc = nxt[arc]

            if arc == -1:
                continue

            removed[e] = 1

            a = x[e]
            b = y[e]
            u = b if a == v else a

            parent[v] = u
            child_cost[v] = 1 if x[e] == u else 0

            degree[v] -= 1
            degree[u] -= 1

            if degree[u] == 1:
                queue.append(u)

        # After peeling, every surviving vertex must have degree 2.
        possible = True
        for v in range(1, V):
            if degree[v] != 0 and degree[v] != 2:
                possible = False
                break

        if not possible:
            out.append("-1 -1")
            continue

        seen = bytearray(V)
        root_cost = array('i', [0]) * V

        answer_cost = 0
        answer_ways = 1

        # Process one connected component at a time.
        for start in range(1, V):
            if head[start] == -1 or seen[start]:
                continue

            stack = [start]
            seen[start] = 1
            vertices = array('i')

            base = 0
            cycle_start = -1

            while stack:
                v = stack.pop()
                vertices.append(v)

                base += child_cost[v]
                if degree[v] > 0:
                    cycle_start = v

                arc = head[v]
                while arc != -1:
                    u = to[arc]
                    if not seen[u]:
                        seen[u] = 1
                        stack.append(u)
                    arc = nxt[arc]

            if cycle_start == -1:
                # The component is a tree.
                root = -1
                for v in vertices:
                    if parent[v] == -1:
                        root = v
                        break

                root_cost[root] = base

                best = base
                ways = 1

                stack = [root]

                while stack:
                    v = stack.pop()
                    cv = root_cost[v]

                    if cv < best:
                        best = cv
                        ways = 1
                    elif cv == best and v != root:
                        ways += 1

                    arc = head[v]
                    while arc != -1:
                        u = to[arc]
                        e = arc >> 1

                        # In a peeled tree, parent[u] == v means u
                        # is a child of v.
                        if parent[u] == v:
                            delta = (1 if x[e] == u else 0) - \
                                    (1 if x[e] == v else 0)
                            root_cost[u] = cv + delta
                            stack.append(u)

                        arc = nxt[arc]

                answer_cost += best
                answer_ways = answer_ways * ways % MOD

            else:
                # The component is unicyclic.
                # Find the remaining cycle.
                cycle_vertices = [cycle_start]
                cycle_edges = []

                cur = cycle_start
                prev_edge = -1

                while True:
                    arc = head[cur]
                    chosen = -1

                    while arc != -1:
                        e = arc >> 1
                        if not removed[e] and e != prev_edge:
                            chosen = e
                            break
                        arc = nxt[arc]

                    if chosen == -1:
                        break

                    cycle_edges.append(chosen)

                    a = x[chosen]
                    b = y[chosen]
                    nxt_vertex = b if a == cur else a

                    if nxt_vertex == cycle_start:
                        break

                    cycle_vertices.append(nxt_vertex)
                    prev_edge = chosen
                    cur = nxt_vertex

                k = len(cycle_vertices)

                if k == 1:
                    # The only possible cycle is a self-loop.
                    cycle_cost = 0
                    cycle_ways = 1
                else:
                    clockwise = 0
                    counterclockwise = 0

                    for i in range(k):
                        u = cycle_vertices[i]
                        v = cycle_vertices[(i + 1) % k]
                        e = cycle_edges[i]

                        # Clockwise wants u -> v, so v is visible.
                        if x[e] == u:
                            clockwise += 1

                        # Counterclockwise wants v -> u, so u is visible.
                        if x[e] == v:
                            counterclockwise += 1

                    if clockwise < counterclockwise:
                        cycle_cost = clockwise
                        cycle_ways = 1
                    elif clockwise > counterclockwise:
                        cycle_cost = counterclockwise
                        cycle_ways = 1
                    else:
                        cycle_cost = clockwise
                        cycle_ways = 2

                answer_cost += base + cycle_cost
                answer_ways = answer_ways * cycle_ways % MOD

        out.append(f"{answer_cost} {answer_ways}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc kề sử dụng biểu diễn ngôi sao chuyển tiếp thay vì danh sách danh sách Python. Điều này quan trọng vì có thể có (2n) trường hợp xảy ra ở điểm cuối và giới hạn bộ nhớ chỉ là 128 MB. các`array('i')`các thùng chứa giữ cho các chỉ số đỉnh và cạnh được thu gọn, trong khi`bytearray`là đủ cho trạng thái Boolean như các cạnh bị loại bỏ và các đỉnh đã thăm. 

Chỉ số cạnh được phục hồi từ chỉ số kề với`arc >> 1`. Mỗi thẻ đóng góp hai bản ghi kề liên tiếp, do đó không cần mảng edge-id riêng biệt. 

Trong quá trình bong tróc,`parent[v]`ghi lại hàng xóm duy nhất còn tồn tại khi (v) bị xóa. tương ứng`child_cost[v]`ghi lại xem cạnh đó có phải được lật hay không khi định hướng từ đỉnh còn sót lại về phía (v). Tổng hợp các giá trị này sẽ cho ra chi phí của việc định hướng cây bắt buộc. 

Việc root lại cây sử dụng mối quan hệ 

[ 
chi phí[u]-cost[v]=[x_e=u]-[x_e=v]. 
] 

Việc triển khai lưu trữ chi phí gốc hiện tại trong`root_cost`, do đó ngăn xếp DFS chỉ chứa các chỉ mục đỉnh chứ không chứa các bộ dữ liệu lớn. Điều này giúp giảm mức sử dụng bộ nhớ ngay cả đối với đường dẫn có (10^5) đỉnh. 

Việc truyền tải chu kỳ có chủ ý kiểm tra`e != prev_edge`. Nếu không có điều kiện này, quá trình truyền tải sẽ ngay lập tức quay trở lại dọc theo cạnh mà nó vừa sử dụng. Một vòng lặp tự được xử lý riêng biệt vì cả hai hướng chu kỳ đều biểu thị chính xác cùng một số hiển thị và do đó có cùng một bộ lật. 

Tất cả số học liên quan đến số cách đều được rút gọn theo modulo (998244353). Số lần lật nhiều nhất là (n), vì vậy số nguyên Python thông thường là quá đủ và không có vấn đề tràn. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, biểu đồ bao gồm hai thành phần cây độc lập. 

Thành phần đầu tiên chứa các thẻ ((1,2)) và ((1,3)). Quá trình bóc tách của nó cuối cùng chọn một đỉnh làm gốc. Các trạng thái khởi động lại có liên quan là: 

| Gốc | Chi phí cạnh (1-2) | Chi phí cạnh (1-3) | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 
| 2 | 0 | 1 | 1 | 
| 3 | 1 | 0 | 1 | 

Như vậy giá trị tối thiểu của nó là 1 và có 2 nghiệm tối ưu. 

Thành phần thứ hai có hình dạng giống hệt nhau, có các đỉnh (4,5,6) nên giá trị nhỏ nhất của nó cũng là 1 với 2 nghiệm tối ưu. 

Các thành phần này độc lập với nhau, tạo ra tổng số bộ lật tối thiểu tối thiểu là (1+1=2) và (2\cdot2=4). 

| Thành phần | Giá cây gốc | Chi phí chu kỳ tối thiểu | Tối thiểu địa phương | Cách địa phương | 
| --- | --- | --- | --- | --- | 
| (1,2,3) | 1 | 0 | 1 | 2 | 
| (4,5,6) | 1 | 0 | 1 | 2 | 
| Tổng cộng | 2 | 0 | 2 | 4 | 

Vì vậy, đầu ra là`2 4`, phù hợp với mẫu 

Đối với Mẫu 2, cả hai thẻ đều tự lặp ở đỉnh 1. Mỗi vòng đóng góp bậc hai, do đó đỉnh 1 có bậc bốn. Không có lá nào có thể bị loại bỏ, và mức độ còn lại không phải là 0 hay 2. 

| Đỉnh | Bằng cấp ban đầu | Sau khi lột | Bằng cấp cốt lõi hợp lệ? | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | Không | 

Thành phần này chứa hai cạnh nhưng chỉ có một đỉnh. Tổng bậc của nó sẽ phải là hai, trong khi một đỉnh duy nhất có thể chấp nhận nhiều nhất là một. Thuật toán từ chối trường hợp thử nghiệm và in`-1 -1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi thẻ đóng góp hai bản ghi kề nhau và việc bóc tách, duyệt thành phần, khởi động lại và duyệt theo chu kỳ, mỗi thẻ chỉ kiểm tra mỗi bản ghi một số lần không đổi. | 
| Không gian | (O(n)) | Có (O(n)) đỉnh liên quan đến đồ thị và các cạnh (O(n)), được lưu trữ trong mảng nhỏ gọn. | 

Trường hợp thử nghiệm lớn nhất có (n=10^5) và tổng số trên tất cả các trường hợp thử nghiệm là (10^6). Thuật toán thực hiện một số lần duyệt đồ thị tuyến tính không đổi, do đó tổng công việc là (O(\sum n)). Biểu diễn kề cận nhỏ gọn cũng giữ cho bộ nhớ tỷ lệ với (n), phù hợp với giới hạn 128 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

MOD = 998244353
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        V = 2 * n + 1

        head = array('i', [-1]) * V
        to = array('i', [0]) * (2 * n)
        nxt = array('i', [0]) * (2 * n)
        x = array('i', [0]) * n
        y = array('i', [0]) * n
        degree = array('i', [0]) * V

        for i in range(n):
            a, b = map(int, input().split())
            x[i] = a
            y[i] = b

            p = 2 * i
            to[p] = b
            nxt[p] = head[a]
            head[a] = p

            to[p + 1] = a
            nxt[p + 1] = head[b]
            head[b] = p + 1

            if a == b:
                degree[a] += 2
            else:
                degree[a] += 1
                degree[b] += 1

        parent = array('i', [-1]) * V
        child_cost = bytearray(V)
        removed = bytearray(n)

        queue = []
        for v in range(1, V):
            if head[v] != -1 and degree[v] == 1:
                queue.append(v)

        q = 0
        while q < len(queue):
            v = queue[q]
            q += 1

            if degree[v] != 1:
                continue

            arc = head[v]
            while arc != -1:
                e = arc >> 1
                if not removed[e]:
                    break
                arc = nxt[arc]

            if arc == -1:
                continue

            removed[e] = 1

            a = x[e]
            b = y[e]
            u = b if a == v else a

            parent[v] = u
            child_cost[v] = 1 if x[e] == u else 0

            degree[v] -= 1
            degree[u] -= 1

            if degree[u] == 1:
                queue.append(u)

        if any(degree[v] not in (0, 2) for v in range(1, V)):
            out.append("-1 -1")
            continue

        seen = bytearray(V)
        root_cost = array('i', [0]) * V

        total_cost = 0
        total_ways = 1

        for start in range(1, V):
            if head[start] == -1 or seen[start]:
                continue

            stack = [start]
            seen[start] = 1
            vertices = []
            base = 0
            cycle_start = -1

            while stack:
                v = stack.pop()
                vertices.append(v)
                base += child_cost[v]

                if degree[v] > 0:
                    cycle_start = v

                arc = head[v]
                while arc != -1:
                    u = to[arc]
                    if not seen[u]:
                        seen[u] = 1
                        stack.append(u)
                    arc = nxt[arc]

            if cycle_start == -1:
                root = next(v for v in vertices if parent[v] == -1)

                root_cost[root] = base
                best = base
                ways = 0

                stack = [root]
                while stack:
                    v = stack.pop()
                    cv = root_cost[v]

                    if cv < best:
                        best = cv
                        ways = 1
                    elif cv == best:
                        ways += 1

                    arc = head[v]
                    while arc != -1:
                        u = to[arc]
                        e = arc >> 1

                        if parent[u] == v:
                            delta = (x[e] == u) - (x[e] == v)
                            root_cost[u] = cv + delta
                            stack.append(u)

                        arc = nxt[arc]

                total_cost += best
                total_ways = total_ways * ways % MOD

            else:
                cv = [cycle_start]
                ce = []

                cur = cycle_start
                prev = -1

                while True:
                    arc = head[cur]
                    e = -1

                    while arc != -1:
                        z = arc >> 1
                        if not removed[z] and z != prev:
                            e = z
                            break
                        arc = nxt[arc]

                    if e == -1:
                        break

                    ce.append(e)

                    a = x[e]
                    b = y[e]
                    nxt_v = b if a == cur else a

                    if nxt_v == cycle_start:
                        break

                    cv.append(nxt_v)
                    prev = e
                    cur = nxt_v

                k = len(cv)

                if k == 1:
                    cycle_cost = 0
                    ways = 1
                else:
                    a = 0
                    b = 0

                    for i in range(k):
                        u = cv[i]
                        v = cv[(i + 1) % k]
                        e = ce[i]

                        if x[e] == u:
                            a += 1
                        if x[e] == v:
                            b += 1

                    if a < b:
                        cycle_cost = a
                        ways = 1
                    elif b < a:
                        cycle_cost = b
                        ways = 1
                    else:
                        cycle_cost = a
                        ways = 2

                total_cost += base + cycle_cost
                total_ways = total_ways * ways % MOD

        out.append(f"{total_cost} {total_ways}")

    return "\n".join(out)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline
        sys.stdout = io.StringIO()

        ans = solve()
        if ans is None:
            ans = sys.stdout.getvalue()

        return ans.strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

sample = """\
3
4
1 2
1 3
4 5
4 6
2
1 1
1 1
3
1 2
3 4
5 6
"""

assert run(sample) == "2 4\n-1 -1\n0 1", "provided samples"

assert run("""\
1
1
1 2
""") == "0 1", "minimum-size ordinary card"

assert run("""\
1
1
1 1
""") == "0 1", "minimum-size self-loop"

assert run("""\
1
2
1 2
1 3
""") == "1 2", "tree with two optimal roots"

assert run("""\
1
2
1 1
1 1
""") == "-1 -1", "all-equal values are impossible"

assert run("""\
1
2
1 4
2 3
""") == "0 1", "maximum endpoint value 2n"

# Maximum-size linear case.
n = 100000
lines = ["1", str(n)]
for i in range(1, n + 1):
    lines.append(f"{i} {i + 1}")

assert run("\n".join(lines) + "\n") == "0 1", "maximum-size path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1 2`|`0 1`| Trường hợp thông thường tối thiểu và không lật không cần thiết | 
|`1 / 1 / 1 1`|`0 1`| Xử lý tự vòng lặp | 
|`1 / 2 / 1 2 / 1 3`|`1 2`| Rễ lại cây và đếm nhiều rễ tối ưu | 
|`1 / 2 / 1 1 / 1 1`|`-1 -1`| Thành phần không thể có quá nhiều cạnh cho các đỉnh của nó | 
|`1 / 2 / 1 4 / 2 3`|`0 1`| Giá trị biên (2n) và hướng đã hợp lệ | 
| Đường dẫn của (100000) thẻ |`0 1`| Đầu vào có kích thước tối đa và độ phức tạp tuyến tính | 

## Vỏ cạnh 

Một vòng lặp tự như```
1
1
1 1
```có bậc hai trong biểu đồ, vì một vòng lặp đóng góp hai vào bậc. Nó không bị loại bỏ trong quá trình lột lá nên được coi là chu trình một đỉnh. Trình xử lý chu trình xử lý trường hợp này một cách riêng biệt và ấn định chi phí lật bằng 0 và một hướng. Đầu ra là`0 1`. 

Đối với một thành phần không thể bằng nhau như```
1
2
1 1
1 1
```đỉnh 1 có bậc 4. Không có chiếc lá nào, và mức độ còn lại không phải là hai. Thuật toán loại bỏ thành phần trước khi thực hiện bất kỳ phép tính định hướng nào, tạo ra`-1 -1`. 

Đối với cây```
1
2
1 2
1 3
```lá bóc vỏ là 2 và 3, trong đó 1 là rễ cuối cùng còn sót lại. Hướng cơ sở bắt nguồn từ 1 yêu cầu hai lần lật. Root lại ở mức 2 sẽ thay đổi chi phí bằng (-1), cho ra giá 1. Reroot ở mức 3 cũng cho chi phí 1. Cả hai gốc đều đạt mức tối ưu, do đó thành phần đóng góp`1 2`. 

Đối với chu kỳ```
1
3
1 2
2 3
1 3
```không có mép cây để bóc. Chu kỳ còn lại có thể được định hướng theo hai hướng. Một hướng lật một lá bài và hướng kia cũng lật một lá bài. Vì hai hướng đảo ngược mọi cạnh chu kỳ theo những cách ngược nhau nên chúng tương ứng với các bộ lật khác nhau, do đó thành phần này góp phần`1 2`. 

Đối với một đầu vào đã hợp lệ như```
1
3
1 2
3 4
5 6
```mỗi thành phần là một cạnh duy nhất có hướng ban đầu có thể thỏa mãn điều kiện số duy nhất. Mỗi cây có một gốc tối ưu tại điểm cuối đối diện với hướng đã chọn ban đầu, không có lần lật nào. Có đúng một tập lật tối thiểu, tập trống, nên kết quả là`0 1`.
