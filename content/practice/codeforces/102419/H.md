---
title: "CF 102419H - Bằng cấp"
description: "Chúng ta có một đồ thị vô hướng có tới 2000 đỉnh và 2000 cạnh. Mỗi cạnh cuối cùng phải được định hướng chính xác về một trong hai điểm cuối của nó. Đối với đỉnh i, giá trị a[i] chỉ định chính xác số cạnh liên quan phải trỏ vào i."
date: "2026-08-14T15:12:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "H"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 1099
verified: false
draft: false
---

[CF 102419H - Bằng cấp](https://codeforces.com/problemset/problem/102419/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 18m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có tới 2000 đỉnh và 2000 cạnh. Mỗi cạnh cuối cùng phải được định hướng chính xác về một trong hai điểm cuối của nó. Đối với một đỉnh`i`, giá trị`a[i]`chỉ định chính xác số cạnh liên quan phải trỏ vào`i`. Một giá trị của`-1`loại bỏ yêu cầu đó, vì vậy mọi kết quả bằng cấp đều được chấp nhận. 

Đầu ra là một trong hai`NO`khi không có định hướng nào thỏa mãn tất cả các mức độ cố định, hoặc`YES`theo sau là một phiên bản có hướng của mọi cạnh ban đầu. Nếu cạnh ban đầu là`{u, v}`, in ấn`u v`có nghĩa là`v`là phần đầu của nó, do đó cạnh đó đóng góp một phần vào mức độ của`v`. 

Các ràng buộc đủ nhỏ để xây dựng dòng chảy chỉ với vài nghìn đỉnh và cạnh. Điều quan trọng hơn là câu trả lời là một phép gán toàn cục: việc quyết định hướng của một cạnh có thể ảnh hưởng đến việc liệu cạnh khác có thể thỏa mãn một đỉnh bị ràng buộc hay không. Một lựa chọn tham lam cục bộ có thể dễ dàng tiêu tốn cạnh duy nhất mà một đỉnh khác cần. Một thuật toán xung quanh`O(m(n+m))`dễ dàng nằm trong phạm vi dự định, trong khi sức mạnh vũ phu đối với mọi hướng là vô vọng. 

Trường hợp cạnh thứ nhất là đỉnh có bậc được yêu cầu lớn hơn bậc thực tế của nó. Ví dụ,```
2 1
2 -1
1 2
```chỉ có một cạnh tới đỉnh 1, do đó bậc bậc của nó không bao giờ có thể là 2. Kết quả đúng là`NO`. Việc thực hiện bất cẩn chỉ kiểm tra`a[i] <= m`sẽ chấp nhận nó một cách sai lầm. 

Trường hợp cạnh thứ hai là một đỉnh bị ràng buộc với yêu cầu bằng 0. Ví dụ,```
2 1
0 -1
1 2
```phải định hướng cạnh như`2 1`. Nếu việc triển khai coi số 0 là "không bị ràng buộc" hoặc khởi tạo số lượng yêu cầu của nó không chính xác, thì nó có thể tạo ra hướng ngược lại và vi phạm yêu cầu. 

Trường hợp cạnh thứ ba xảy ra khi tất cả các đỉnh đều bị ràng buộc. Vì```
3 3
1 1 1
1 2
2 3
3 1
```trình tự theo cấp độ duy nhất có thể đạt được bằng cách định hướng tam giác như một chu trình có hướng. Câu trả lời là`YES`. Không có đỉnh tự do nào có sẵn để hấp thụ một cạnh không thể được gán cho điểm cuối bị ràng buộc. 

Trường hợp cạnh thứ tư là giá trị`-1`. Coi như```
2 1
1 -1
1 2
```Cạnh duy nhất phải trỏ đến đỉnh 1, trong khi đỉnh 2 được phép nhận không có cạnh nào. điều trị`-1`như một mức độ bắt buộc theo nghĩa đen rõ ràng là sai. các`-1`các đỉnh cần dung lượng nhưng chúng không có giới hạn dưới. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là chọn hướng độc lập cho mọi cạnh. Một cạnh có hai khả năng, do đó có chính xác`2^m`định hướng kiểm tra. Đối với mỗi hướng, chúng ta có thể tính toán tất cả các mức độ theo`O(n+m)`và kiểm tra các yêu cầu. Do đó, số lượng hoạt động trong trường hợp xấu nhất là`O(2^m(n+m))`. Với`m = 2000`, số hướng là`2^2000`, đại khái`10^602`, vì vậy cách tiếp cận này không chỉ chậm mà còn hoàn toàn không thể sử dụng được. 

Quan sát hữu ích là mỗi cạnh đóng góp chính xác một đơn vị vào cấp độ chính xác của một điểm cuối. Đó chính xác là một vấn đề phân công. Giới thiệu một nút luồng cho mỗi cạnh ban đầu. Gửi một đơn vị qua nút cạnh đó có nghĩa là chọn nơi mà cạnh vô hướng tương ứng sẽ kết thúc. Nút cạnh có thể gửi đơn vị của nó tới một trong hai đỉnh điểm cuối của nó. 

Khó khăn còn lại là các đỉnh bị ràng buộc yêu cầu số lượng đơn vị đến chính xác, trong khi các đỉnh không bị ràng buộc có thể nhận được bất kỳ số lượng nào. Các yêu cầu chính xác được thể hiện một cách tự nhiên bằng các giới hạn dưới và trên của luồng. Chúng ta có thể biến toàn bộ vấn đề thành một vòng tuần hoàn khả thi. 

Tạo một đỉnh giống như nguồn`S`và một đỉnh dạng chìm`T`. Đối với mọi cạnh ban đầu`e`, tạo một nút cạnh`E_e`. Mạng lưới dòng chảy chứa`S -> E_e`với giới hạn dưới và giới hạn trên đều bằng 1, buộc mỗi cạnh ban đầu phải đóng góp chính xác một đơn vị. Từ`E_e`, thêm các cạnh dung lượng một vào hai điểm cuối của cạnh ban đầu. Cuối cùng, kết nối mọi đỉnh ban đầu`v`ĐẾN`T`. Nếu như`a[v]`được cố định, cạnh đó có giới hạn dưới và giới hạn trên`a[v]`. Nếu như`a[v] = -1`, giới hạn dưới của nó bằng 0 và giới hạn trên của nó có thể là bậc đồ thị của nó. 

Cạnh phụ`T -> S`với năng lực`m`đóng cái này thành một vòng tuần hoàn. Bảo toàn luồng bây giờ có nghĩa chính xác như những gì chúng ta muốn: mỗi nút cạnh nhận một đơn vị và gửi nó đến một điểm cuối, trong khi mỗi đỉnh bị ràng buộc sẽ gửi chính xác số lượng đơn vị được yêu cầu của nó tới`T`. 

Giới hạn dưới được loại bỏ bằng cách sử dụng mức giảm lưu thông tiêu chuẩn. Đối với một cạnh`u -> v`có giới hạn`[L, R]`, đầu tiên chúng tôi dự trữ`L`đơn vị và để lại công suất còn lại`R-L`. Giới hạn dưới dành riêng tạo ra sự mất cân bằng ở các điểm cuối của nó. Sau đó, một siêu nguồn và siêu chìm được sử dụng để sửa chữa tất cả những sự mất cân bằng đó bằng tính toán luồng cực đại thông thường. Với khả năng tích phân, một luồng khả thi tích hợp tồn tại bất cứ khi nào có bất kỳ luồng khả thi nào tồn tại, do đó, mỗi nút cạnh ban đầu cuối cùng sẽ gửi chính xác một đơn vị nguyên vẹn đến một điểm cuối. 

Đối với bước luồng tối đa, Ford-Fulkerson với DFS ở đây là đủ. Giới hạn dung lượng số nguyên thông thường của nó là`O(EF)`, Ở đâu`F`là lưu lượng tối đa. Trong cấu trúc cụ thể này, tất cả luồng bắt nguồn từ một nút cạnh bị giới hạn bởi công suất đơn vị`S -> E_e`các cạnh và chỉ có`m`những đơn vị như vậy. Dòng cân bằng còn lại có thể được xử lý thông qua một`T -> S`sự liên quan. Do đó, số lượng tăng cường hữu ích được giới hạn bởi`O(m)`, cho`O(mE) = O(m(n+m))`thời gian cho vấn đề này. 

Phương pháp brute-force hoạt động hiệu quả vì nó xem xét rõ ràng mọi nhiệm vụ có thể thực hiện được. Nó thất bại vì số lượng bài tập là theo cấp số nhân. Mô hình luồng chỉ giữ lại các lựa chọn có liên quan, cụ thể là điểm cuối nào nhận đơn vị của mỗi cạnh và cho phép thuật toán luồng cực đại giải quyết đồng thời tất cả các lựa chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^m(n+m))`|`O(n+m)`| Quá chậm | 
| Dòng chảy giới hạn dưới + Ford-Fulkerson |`O(m(n+m))`|`O(n+m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đồ thị và tính bậc của mỗi đỉnh ban đầu. Bậc đưa ra giới hạn trên tự nhiên cho một đỉnh không bị ràng buộc, bởi vì không có đỉnh nào có thể nhận được nhiều cạnh đến hơn số cạnh chạm vào nó. 
2. Xây dựng một nút luồng`E_e`với mọi cạnh vô hướng ban đầu`e = {u, v}`. Thêm một cạnh giới hạn`S -> E_e`có giới hạn`[1, 1]`. Điều này buộc mỗi cạnh ban đầu phải đóng góp chính xác một đơn vị dòng, do đó không có cạnh nào có thể biến mất khỏi hướng cuối cùng. 
3. Thêm`E_e -> u`Và`E_e -> v`, mỗi cái có giới hạn`[0, 1]`. Từ`E_e`nhận đúng một đơn vị, bảo toàn dòng chảy buộc đúng một trong hai cung này để mang đơn vị đó. Chọn cạnh đầu tiên có nghĩa là cạnh ban đầu kết thúc tại`u`; chọn thứ hai có nghĩa là nó kết thúc tại`v`. 
4. Với mọi đỉnh ban đầu`v`, thêm một cạnh`v -> T`. Nếu như`a[v]`đã được sửa, hãy đưa ra giới hạn cạnh này`[a[v], a[v]]`. Nếu như`a[v] = -1`, cho nó giới hạn`[0, degree[v]]`. Giới hạn dưới và giới hạn trên bằng nhau đối với một đỉnh bị ràng buộc, do đó nó phải nhận được chính xác số lượng đơn vị cạnh được yêu cầu. 
5. Thêm cạnh mô hình`T -> S`có giới hạn`[0, m]`. Nếu không có cạnh này,`S`sẽ có dòng chảy đi và`T`sẽ có dòng chảy vào, đó là dòng nguồn-bồn thông thường chứ không phải là dòng tuần hoàn. Việc đóng mạng làm cho mọi đỉnh đều tuân theo sự bảo toàn. 
6. Thay thế mọi cạnh giới hạn`[L, R]`bởi cạnh dư thông thường của công suất`R-L`. Đồng thời, duy trì`balance[u] -= L`Và`balance[v] += L`cho một cạnh`u -> v`. Các dấu hiệu mô tả sự mất cân bằng được tạo ra sau khi điều chỉnh luồng giới hạn dưới. 
7. Thêm siêu nguồn`SS`và siêu chìm`TT`. Đối với mọi đỉnh có cân bằng dương, hãy thêm`SS -> v`với năng lực`balance[v]`. Đối với mỗi đỉnh có số dư âm, hãy thêm`v -> TT`với năng lực`-balance[v]`. Dòng phụ trợ phải bão hòa tất cả các cạnh cân bằng này. 
8. Chạy Ford-Fulkerson từ`SS`ĐẾN`TT`. Nếu tổng dòng tiền nhỏ hơn tổng của tất cả số dư dương thì giới hạn dưới không thể tương thích được, vì vậy hãy in`NO`. 
9. Nếu tất cả các cạnh cân bằng đã bão hòa, hãy khôi phục dòng chảy trên mỗi cạnh`E_e -> u`Và`E_e -> v`vòng cung. Chính xác một trong số họ mang theo một đơn vị. Nếu như`E_e -> u`mang một đơn vị, đầu ra`v u`, bởi vì cạnh vô hướng ban đầu`{u,v}`phải kết thúc tại`u`. Nếu không thì xuất ra`u v`. 

Tại sao nó hoạt động 

Bất biến trung tâm là mọi nút cạnh ban đầu luôn biểu diễn chính xác một đơn vị cạnh. Giới hạn dưới cố định trên`S -> E_e`cung cấp đơn vị đó và hai đích đến duy nhất có thể là điểm cuối ban đầu. Do đó, một chu trình khả thi xác định một hướng hợp lệ của mọi cạnh ban đầu. 

Đối với một đỉnh bị ràng buộc`v`, cạnh`v -> T`có cả giới hạn dưới và giới hạn trên bằng`a[v]`. Lực bảo toàn chính xác`a[v]`đơn vị để đến`v`, vì vậy mức độ cuối cùng của nó là hoàn toàn chính xác. Một đỉnh không bị ràng buộc có đủ dung lượng trên để nhận bất kỳ số nào từ 0 đến bậc của nó. 

Phép biến đổi giới hạn dưới tương đương với vòng tuần hoàn ban đầu vì nó bắt đầu bằng cách sửa mọi đơn vị giới hạn dưới bắt buộc và yêu cầu luồng tối đa phụ trợ sửa chữa sự mất cân bằng đỉnh gây ra. Nếu luồng phụ bão hòa mọi cạnh cân bằng cần thiết, việc thêm các giới hạn dưới trở lại sẽ tạo ra một vòng tuần hoàn hợp lệ. Nếu nó không thể bão hòa chúng, thì việc phân công công suất còn lại không thể khôi phục sự bảo toàn, do đó không tồn tại định hướng ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

class Flow:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, cap):
        idx = len(self.g[u])
        rev = len(self.g[v])
        self.g[u].append([v, rev, cap])
        self.g[v].append([u, idx, 0])
        return idx

    def max_flow(self, s, t):
        n = self.n
        total = 0

        while True:
            used = [False] * n

            def dfs(v, pushed):
                if v == t:
                    return pushed

                used[v] = True

                for e in self.g[v]:
                    to, rev, cap = e
                    if cap <= 0 or used[to]:
                        continue

                    got = dfs(to, min(pushed, cap))
                    if got:
                        e[2] -= got
                        self.g[to][rev][2] += got
                        return got

                return 0

            pushed = dfs(s, 10**9)
            if pushed == 0:
                break

            total += pushed

        return total

def solve_case(n, m, a, edges):
    deg = [0] * n
    for u, v in edges:
        deg[u] += 1
        deg[v] += 1

    # Node layout:
    # 0 .. n-1          original vertices
    # n .. n+m-1        one node per original edge
    # S, T              circulation source/sink
    # SS, TT            lower-bound reduction source/sink
    S = n + m
    T = S + 1
    SS = T + 1
    TT = SS + 1
    N = TT + 1

    flow = Flow(N)
    balance = [0] * N

    def add_bounded(u, v, low, high):
        idx = flow.add_edge(u, v, high - low)

        # Lower bound low is already sent on u -> v.
        # It contributes one unit of outgoing lower flow at u
        # and one unit of incoming lower flow at v.
        balance[u] -= low
        balance[v] += low

        return idx

    # For reconstruction:
    # (edge_node, arc_index_to_u, arc_index_to_v, u, v)
    original_refs = []

    for i, (u, v) in enumerate(edges):
        edge_node = n + i

        # Every original edge contributes exactly one unit.
        add_bounded(S, edge_node, 1, 1)

        idx_u = add_bounded(edge_node, u, 0, 1)
        idx_v = add_bounded(edge_node, v, 0, 1)

        original_refs.append((edge_node, idx_u, idx_v, u, v))

    for v in range(n):
        if a[v] == -1:
            add_bounded(v, T, 0, deg[v])
        else:
            add_bounded(v, T, a[v], a[v])

    # Close the S -> ... -> T flow into a circulation.
    add_bounded(T, S, 0, m)

    need = 0

    for v in range(N):
        if balance[v] > 0:
            flow.add_edge(SS, v, balance[v])
            need += balance[v]
        elif balance[v] < 0:
            flow.add_edge(v, TT, -balance[v])

    got = flow.max_flow(SS, TT)

    if got != need:
        return None

    answer = []

    for edge_node, idx_u, idx_v, u, v in original_refs:
        # The transformed capacity was 1, so residual capacity 0
        # means one unit of flow was sent through that arc.
        flow_to_u = 1 - flow.g[edge_node][idx_u][2]
        flow_to_v = 1 - flow.g[edge_node][idx_v][2]

        if flow_to_u == 1:
            # The edge ends at u.
            answer.append((v + 1, u + 1))
        elif flow_to_v == 1:
            # The edge ends at v.
            answer.append((u + 1, v + 1))
        else:
            # This cannot happen in a feasible circulation.
            return None

    return answer

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        edges.append((u - 1, v - 1))

    answer = solve_case(n, m, a, edges)

    if answer is None:
        print("NO")
        return

    out = ["YES"]
    out.extend(f"{u} {v}" for u, v in answer)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`Flow`lớp lưu trữ mọi cạnh dư dưới dạng`[destination, reverse-index, residual-capacity]`. Chỉ mục ngược là thứ cho phép đường dẫn tăng cường hoàn tác một phần của lựa chọn trước đó. Điều này quan trọng vì đường dẫn đầu có thể gán một cạnh cho một điểm cuối và đường dẫn sau có thể cần định tuyến lại nhiệm vụ đó.`add_bounded`thực hiện phép biến đổi giới hạn dưới. Công suất còn lại trở thành`high - low`, trong khi mảng cân bằng ghi lại tác dụng của lệnh bắt buộc`low`đơn vị. Bản thân giới hạn dưới ban đầu không bị mất vì nó được ngầm bao gồm khi luồng cuối cùng được xây dựng lại. 

Các nút cạnh được tạo sau các đỉnh ban đầu, giúp việc lập chỉ mục trở nên đơn giản. Đối với cạnh gốc`i`, nút luồng của nó là`n + i`. Hai cung đi được lưu trữ theo chỉ mục của chúng trong danh sách kề của nút đó. Vì dung lượng ban đầu của chúng đều là một nên dung lượng còn lại cuối cùng của chúng sẽ trực tiếp cho chúng ta biết điểm cuối nào đã nhận được đơn vị. 

các`T -> S`cạnh có công suất`m`, bởi vì chính xác`m`đơn vị nhập`T`tổng cộng, một cho mỗi cạnh ban đầu. Công suất lớn hơn cũng có tác dụng, nhưng`m`là một ràng buộc chặt chẽ và thuận tiện. 

Việc sử dụng giảm giới hạn dưới`balance[u] -= low`Và`balance[v] += low`. Số dư dương có nghĩa là các giới hạn dưới cố định sẽ để lại luồng đến dư thừa ở đỉnh đó, do đó mạng dư phải định tuyến lượng đó đi. Số dư âm có nghĩa là đỉnh cần luồng dư thừa đến. Các cạnh siêu nguồn và siêu chìm mã hóa chính xác hai trường hợp đó. 

Không có vấn đề tràn số nguyên trong Python. Tất cả số lượng liên quan nhiều nhất là vài nghìn, nhưng số nguyên Python cũng loại bỏ mọi sự phụ thuộc vào độ rộng số nguyên của máy. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, mức độ được yêu cầu là`[1, 2, 1, -1, 0]`. Một hướng hợp lệ chính xác là hướng được hiển thị trong mẫu. 

| Cạnh gốc | Người đứng đầu được chọn | Bằng cấp sau khi xử lý | 
| --- | --- | --- | 
|`1 2`|`2`|`[0,1,0,0,0]`| 
|`1 3`|`1`|`[1,1,0,0,0]`| 
|`2 3`|`2`|`[1,2,0,0,0]`| 
|`3 4`|`3`|`[1,2,1,0,0]`| 
|`4 5`|`5`|`[1,2,1,0,1]`| 

Bậc cuối cùng tại các đỉnh 1, 2, 3 và 5 là`1, 2, 1, 0`, đúng như yêu cầu. Vertex 4 không bị ràng buộc. Mô hình luồng đạt được cùng một nhiệm vụ bằng cách gửi một đơn vị qua mỗi nút cạnh và chọn cung điểm cuối tương ứng. 

Đối với Mẫu 2, thay đổi duy nhất là đỉnh 5 hiện yêu cầu bậc 1. 

| Cạnh gốc | Người đứng đầu được chọn | Bằng cấp sau khi xử lý | 
| --- | --- | --- | 
|`1 2`|`2`|`[0,1,0,0,0]`| 
|`1 3`|`1`|`[1,1,0,0,0]`| 
|`2 3`|`2`|`[1,2,0,0,0]`| 
|`3 4`|`3`|`[1,2,1,0,0]`| 
|`4 5`|`5`|`[1,2,1,0,1]`| 

Cạnh cuối cùng bây giờ phải trỏ vào đỉnh 5. Điều này chứng tỏ tại sao đỉnh 4 không bị ràng buộc không thể đơn giản hấp thụ mọi cạnh còn sót lại. Dung lượng của nó đã có sẵn nhưng yêu cầu chính xác ở đỉnh 5 vẫn phải được thỏa mãn. 

Hai dấu vết thể hiện sự bất biến giống nhau từ các phía khác nhau. Mỗi cạnh đóng góp chính xác một đơn vị và mỗi đỉnh cố định sẽ sử dụng chính xác số lượng đơn vị đó được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(m(n+m))`| Đồ thị phụ có`O(n+m)`các cạnh và nhu cầu xây dựng dòng tăng cường tích hợp`O(m)`tăng cường có liên quan | 
| Không gian |`O(n+m)`| Đồ thị chứa`O(n+m)`đỉnh và cạnh, với số cạnh dư không đổi trên mỗi cạnh được mô hình hóa | 

Những ràng buộc ban đầu có`n,m <= 2000`, do đó đồ thị phụ chỉ chứa vài nghìn đỉnh và gần như bội số không đổi của`n+m`các cung dư. Giá trị luồng được đóng góp bởi các nút cạnh ban đầu được giới hạn bởi`m`và mọi cung phía nguồn như vậy đều có dung lượng bằng một. Kết quả`O(m(n+m))`ràng buộc là nhỏ thoải mái ở quy mô này. 

## Trường hợp thử nghiệm 

Hướng đầu ra không phải là duy nhất, vì vậy các bài kiểm tra nên xác thực câu trả lời được tạo ra thay vì so sánh từng ký tự với một hướng cụ thể. Khai thác sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`.```python
# Test harness for solution.py
import io
import sys

from solution import solve_case

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    try:
        from solution import solve
        solve()
        return out.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str, out: str, possible: bool):
    data = inp.strip().splitlines()
    n, m = map(int, data[0].split())
    a = list(map(int, data[1].split()))

    edges = []
    for line in data[2:]:
        u, v = map(int, line.split())
        edges.append((u, v))

    lines = out.strip().splitlines()

    if not possible:
        assert lines == ["NO"], f"expected NO, got:\n{out}"
        return

    assert lines[0] == "YES", f"expected YES, got:\n{out}"
    assert len(lines) == m + 1

    original = {tuple(sorted(e)) for e in edges}
    indeg = [0] * n

    for line in lines[1:]:
        u, v = map(int, line.split())
        assert 1 <= u <= n
        assert 1 <= v <= n
        assert u != v

        assert tuple(sorted((u, v))) in original
        indeg[v - 1] += 1

    for i in range(n):
        if a[i] != -1:
            assert indeg[i] == a[i], (
                f"vertex {i + 1}: expected {a[i]}, got {indeg[i]}"
            )

# Sample 1
sample1 = """\
5 5
1 2 1 -1 0
1 2
1 3
2 3
3 4
4 5
"""
validate(sample1, run(sample1), True)

# Sample 2
sample2 = """\
5 5
1 2 1 -1 1
1 2
1 3
2 3
3 4
4 5
"""
validate(sample2, run(sample2), True)

# Minimum-size valid graph.
case_min = """\
2 1
1 -1
1 2
"""
validate(case_min, run(case_min), True)

# Boundary case: requested degree equals m and is actually attainable.
case_boundary = """\
3 2
2 0 -1
1 2
1 3
"""
validate(case_boundary, run(case_boundary), True)

# All vertices constrained with equal requested values.
case_equal = """\
4 4
1 1 1 1
1 2
2 3
3 4
4 1
"""
validate(case_equal, run(case_equal), True)

# Impossible because vertex 1 has degree 1 but requests in-degree 2.
case_impossible = """\
2 1
2 -1
1 2
"""
validate(case_impossible, run(case_impossible), False)

# Maximum-size graph: a 2000-cycle, every vertex requests in-degree 1.
n = 2000
m = 2000
a = " ".join(["1"] * n)
cycle_edges = "\n".join(
    f"{i} {i % n + 1}" for i in range(1, n + 1)
)
case_max = f"{n} {m}\n{a}\n{cycle_edges}\n"

validate(case_max, run(case_max), True)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`YES`với định hướng hợp lệ | Các đỉnh bị ràng buộc và không bị ràng buộc hỗn hợp cơ bản | 
| Mẫu 2 |`YES`với định hướng hợp lệ | Điểm cuối miễn phí trước đây trở nên bị ràng buộc chính xác | 
|`2 1 / 1 -1 / 1 2`|`YES`| Biểu đồ hợp lệ tối thiểu và độ chính xác một | 
|`3 2 / 2 0 -1`|`YES`| Bằng cấp tương đương`m`, không yêu cầu và định hướng chính xác | 
| Bốn chu kỳ với tất cả`1`|`YES`| Yêu cầu hoàn toàn bằng nhau và không có đỉnh không bị ràng buộc | 
|`2 1 / 2 -1 / 1 2`|`NO`| Bằng cấp yêu cầu lớn hơn bằng cấp thực tế | 
| Chu kỳ 2000 với tất cả`1`|`YES`| Mạng đầu vào có kích thước tối đa và luồng lớn | 

## Vỏ cạnh 

Mức độ được yêu cầu lớn hơn mức độ đỉnh sẽ bị chính mạng luồng từ chối. Vì```
2 1
2 -1
1 2
```nút cạnh có sẵn một đơn vị, trong khi cạnh của đỉnh 1`T`yêu cầu hai đơn vị vì giới hạn dưới và giới hạn trên của nó đều bằng 2. Không có cách nào để gửi hai đơn vị vào đỉnh 1 từ một nút cạnh. Luồng tối đa phụ trợ không thể bão hòa tất cả các cạnh cân bằng, do đó chương trình sẽ in`NO`. 

Yêu cầu bằng 0 được xử lý dưới dạng giới hạn trên và dưới chính xác của 0. Vì```
2 1
0 -1
1 2
```nút cạnh phải gửi một đơn vị của nó tới đỉnh 2, vì đỉnh 1 không có dung lượng trên`v -> T`bờ rìa. Do đó, luồng được phục hồi sẽ in`2 1`, cho đỉnh 1 bậc 0. 

Một giá trị của`-1`trở thành một khoảng linh hoạt`[0, degree[v]]`. Vì```
2 1
1 -1
1 2
```đỉnh 1 có khoảng`[1,1]`, do đó phép gán khả thi duy nhất sẽ gửi cạnh vào đỉnh 1. Đỉnh 2 có thể nhận bất cứ thứ gì từ 0 đến 1, do đó bậc 0 cuối cùng của nó là hợp lệ. 

Khi mọi đỉnh bị ràng buộc, sẽ không có điểm cuối dự phòng nào có thể hấp thụ luồng dư thừa. TRONG```
3 3
1 1 1
1 2
2 3
3 1
```mỗi nút cạnh phải gửi một đơn vị và mỗi đỉnh tới`T`edge chấp nhận chính xác một. Sự tuần hoàn có thể định tuyến ba đơn vị xung quanh tam giác, tạo ra một chu trình có hướng. Khả năng chính xác là thứ ngăn không cho bất kỳ đỉnh nào nhận được hai cạnh trong khi đỉnh khác không nhận được. 

Trường hợp kích thước tối đa là chu kỳ 2000 với mỗi cấp độ được yêu cầu bằng một. Mỗi đỉnh có thể nhận được một trong hai cạnh tới của nó và bản thân chu trình có hướng là một hướng hợp lệ. Cấu trúc luồng có 2000 nút cạnh và 2000 nút đỉnh ban đầu, tuy nhiên cấu trúc của nó vẫn tuyến tính ở kích thước đầu vào, do đó, thuật toán tương tự được áp dụng mà không cần xử lý đặc biệt.
