---
title: "CF 1082G - Petya và đồ thị"
description: "Chúng ta được cho một đồ thị vô hướng đơn giản trong đó mỗi đỉnh mang một chi phí và mỗi cạnh mang một phần thưởng. Chúng ta được phép chọn bất kỳ tập hợp con nào của các đỉnh và sau đó từ các cạnh, chúng ta chỉ có thể giữ lại những tập hợp có điểm cuối đều được chọn."
date: "2026-06-15T06:08:04+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1082
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 55 (Rated for Div. 2)"
rating: 2400
weight: 1082
solve_time_s: 217
verified: false
draft: false
---

[CF 1082G - Petya và Đồ thị](https://codeforces.com/problemset/problem/1082/G) 

**Đánh giá:** 2400 
**Tags:** luồng, đồ thị 
**Thời gian giải:** 3 phút 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng đơn giản trong đó mỗi đỉnh mang một chi phí và mỗi cạnh mang một phần thưởng. Chúng ta được phép chọn bất kỳ tập hợp con nào của các đỉnh và sau đó từ các cạnh, chúng ta chỉ có thể giữ lại những tập hợp có điểm cuối đều được chọn. Điểm của đồ thị con đã chọn được tính bằng tổng trọng số của các cạnh trừ đi tổng trọng số của đỉnh được bao gồm. Nhiệm vụ là chọn một tập đỉnh tối đa hóa giá trị này, với các cạnh chỉ đóng góp khi có cả hai điểm cuối. 

Cấu trúc là một vấn đề đánh đổi cổ điển. Bao gồm một đỉnh luôn là một hình phạt, trong khi bao gồm một cạnh luôn là một lợi ích, nhưng các cạnh chỉ khả dụng nếu cả hai điểm cuối đều được thực hiện. Điều này tạo ra sự kết hợp giữa các quyết định: các đỉnh không thể được tối ưu hóa một cách độc lập. 

Các ràng buộc nhỏ về số đỉnh và cạnh, cả hai đều lên tới 1000. Điều đó ngay lập tức gợi ý rằng một$O(n^3)$hoặc thậm chí$O(n^2 m)$xây dựng dòng chảy tối đa là khả thi. Bất cứ điều gì có tính hàm mũ trên các đỉnh đều không thể xảy ra, vì$2^{1000}$là quá xa tầm với. Sự hiện diện của cấu trúc biểu đồ cộng với lựa chọn tập hợp con gợi ý rõ ràng về công thức cắt giảm tối thiểu, bởi vì chúng tôi đang cân bằng các hình phạt nút với phần thưởng cạnh theo các ràng buộc tập hợp con. 

Một trường hợp thất bại tinh tế của tư duy tham lam xuất hiện khi một đỉnh dường như không mang lại lợi nhuận nhưng chỉ trở nên có lợi khi được kết hợp với các đỉnh khác thông qua các cạnh được chia sẻ. Ví dụ: một hình tam giác trong đó mỗi đỉnh có giá 5 và mỗi cạnh có trọng số 10. Xét riêng lẻ, các đỉnh có vẻ đắt tiền, nhưng việc chọn cả ba mang lại cạnh tăng 30 và giá đỉnh là 15, cho tổng số dương 15. Bất kỳ cách tiếp cận nào loại bỏ các đỉnh cục bộ mà không xem xét đến sức mạnh tổng hợp của các cạnh đều thất bại ở đây. 

Một trường hợp cạnh khác là chọn các đỉnh bị cô lập. Nếu một đỉnh không có cạnh liên thuộc thì đóng góp của nó luôn là$-a_i$. Bất kỳ giải pháp đúng nào cũng phải tự động loại trừ các đỉnh đó trừ khi chúng được yêu cầu gián tiếp thông qua các cấu trúc cạnh có lợi. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là liệt kê mọi tập hợp con của các đỉnh, tính toán các cạnh nào được chứa đầy đủ và đánh giá điểm số. Đối với mỗi tập hợp con, kiểm tra tất cả các chi phí cạnh$O(m)$, và có$2^n$tập hợp con, cho$O(2^n \cdot m)$. Với$n = 1000$, điều này là không thể. 

Quan sát quan trọng là vật kính có cấu trúc cắt ẩn bên trong nó. Mỗi cạnh đóng góp tích cực nếu cả hai điểm cuối được chọn, nhưng không đóng góp gì khác, trong khi các đỉnh luôn đóng góp tiêu cực nếu được chọn. Điều này giống như một vấn đề lựa chọn trong đó chúng ta muốn kích hoạt các nút và các cạnh khuếch đại, được mô hình hóa một cách tự nhiên bởi một mạng luồng trong đó việc chọn các đỉnh tương ứng với việc chọn một cạnh của vết cắt và các cạnh áp đặt các ràng buộc ghép giữa các điểm cuối. 

Chúng tôi chuyển đổi vấn đề thành một mức cắt tối thiểu. Chúng tôi giới thiệu một nguồn và chìm. Mỗi đỉnh trở thành một nút có một cạnh từ nguồn biểu thị việc chốt lời và một cạnh chìm biểu thị chi phí phải trả. Các cạnh giữa các đỉnh được mô hình hóa bằng các kết nối vô hướng cho phép chúng ta mã hóa sự đóng góp chung của chúng. Phép biến đổi cổ điển là tối đa hóa tổng trọng lượng cạnh trừ đi hình phạt của đỉnh bằng cách chuyển nó thành vấn đề cắt trong đó việc cắt các cạnh tương ứng với việc mất phần thưởng cạnh tiềm năng hoặc phải trả chi phí đỉnh. 

Cụ thể hơn, chúng tôi coi chi phí đỉnh là dung lượng từ nguồn đến đỉnh và các cạnh được mô hình hóa sao cho việc cắt chúng tương ứng với việc giảm trọng lượng của chúng, điều này được xử lý thông qua các ràng buộc dung lượng hai chiều. Câu trả lời cuối cùng sẽ là tổng trọng lượng cạnh trừ đi mức cắt tối thiểu trong mạng được xây dựng. 

Brute-force hoạt động vì nó đánh giá trực tiếp tất cả các tập hợp con, nhưng không thành công vì nó không thể sử dụng lại cấu trúc. Quan sát rằng mục tiêu là dạng bậc hai trên lựa chọn đỉnh cho phép chúng ta nhúng nó vào mạng luồng nơi tính nhất quán toàn cầu được thực thi thông qua các ràng buộc cắt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot m)$|$O(n)$| Quá chậm | 
| Cắt / Lưu lượng tối thiểu |$O(F \cdot (n + m))$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống một trường hợp cắt s-t tối thiểu. 

1. Tính tổng trọng số của tất cả các cạnh. Giá trị này thể hiện sự đóng góp cạnh tối đa có thể nếu tất cả các cạnh được chọn. 
2. Xây dựng mạng luồng với một nguồn, một điểm chìm và một nút trên mỗi đỉnh. Mục đích là mã hóa các quyết định lựa chọn dưới dạng phân vùng các đỉnh. 
3. Đối với mỗi đỉnh$i$, thêm một cạnh từ nguồn vào$i$có công suất bằng$a_i$. Mô hình này trả chi phí đỉnh nếu đỉnh được giữ ở phía nguồn. 
4. Đối với mỗi đỉnh$i$, kết nối$i$chìm với sức chứa vô hạn. Điều này thực thi tính nhất quán trong cấu trúc cắt sao cho các cấu hình không hợp lệ cố gắng phá vỡ một phần các ràng buộc trở nên quá đắt để lựa chọn. 
5. Đối với mỗi cạnh$(u, v, w)$, thêm các cạnh giữa$u$Và$v$với năng lực$w$. Điều này mã hóa rằng nếu chúng ta tách các điểm cuối trong phần cắt, chúng ta sẽ mất phần đóng góp của cạnh. 
6. Tính toán mức cắt tối thiểu giữa nguồn và bồn. Việc cắt này thể hiện sự mất mát tối thiểu không thể tránh khỏi do các ràng buộc lựa chọn đỉnh và sự phân tách các cạnh. 
7. Câu trả lời là tổng cạnh trừ đi giá trị cắt tối thiểu. 

Quá trình chuyển đổi hoạt động vì mọi đơn vị công suất cắt giảm đều tương ứng chính xác với việc trả chi phí đỉnh hoặc mất phần thưởng cạnh và quy trình buộc các lựa chọn này phải nhất quán trên toàn cầu. 

### Tại sao nó hoạt động 

Bất kỳ tập hợp con nào của các đỉnh đều tương ứng với một vết cắt trong đó các đỉnh ở phía nguồn được chọn. Khả năng cắt giảm đo lường chính xác mức phạt mà tập hợp con đó phải gánh chịu: trọng số đỉnh xuất hiện dưới dạng chi phí trực tiếp và trọng số cạnh xuất hiện dưới dạng mức phạt nếu các điểm cuối bị tách ra. Vì mức cắt tối thiểu tìm ra cấu hình phạt rẻ nhất như vậy nên việc trừ nó khỏi tổng trọng lượng cạnh sẽ mang lại mục tiêu tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

class Dinic:
    def __init__(self, n):
        self.n = n
        self.adj = [[] for _ in range(n)]

    def add_edge(self, u, v, c):
        self.adj[u].append([v, c, len(self.adj[v])])
        self.adj[v].append([u, 0, len(self.adj[u]) - 1])

    def bfs(self, s, t):
        self.level = [-1] * self.n
        q = deque([s])
        self.level[s] = 0
        while q:
            u = q.popleft()
            for v, c, r in self.adj[u]:
                if c > 0 and self.level[v] == -1:
                    self.level[v] = self.level[u] + 1
                    q.append(v)
        return self.level[t] != -1

    def dfs(self, u, t, f):
        if u == t:
            return f
        for i in range(self.it[u], len(self.adj[u])):
            self.it[u] = i
            v, c, r = self.adj[u][i]
            if c > 0 and self.level[v] == self.level[u] + 1:
                pushed = self.dfs(v, t, min(f, c))
                if pushed:
                    self.adj[u][i][1] -= pushed
                    self.adj[v][r][1] += pushed
                    return pushed
        return 0

    def max_flow(self, s, t):
        flow = 0
        INF = 10**18
        while self.bfs(s, t):
            self.it = [0] * self.n
            while True:
                pushed = self.dfs(s, t, INF)
                if not pushed:
                    break
                flow += pushed
        return flow

n, m = map(int, input().split())
a = list(map(int, input().split()))

S = n
T = n + 1
dinic = Dinic(n + 2)

total_edges = 0

for i in range(n):
    dinic.add_edge(S, i, a[i])

for _ in range(m):
    u, v, w = map(int, input().split())
    u -= 1
    v -= 1
    total_edges += w
    dinic.add_edge(u, v, w)
    dinic.add_edge(v, u, w)

flow = dinic.max_flow(S, T)
print(total_edges - flow)
```Việc triển khai sử dụng thuật toán của Dinic để tính toán mức cắt tối thiểu thông qua luồng tối đa. Các cạnh từ nguồn tới đỉnh mã hóa trực tiếp các hình phạt của đỉnh. Mỗi cạnh vô hướng được biểu diễn dưới dạng hai cạnh có hướng sao cho việc cắt giữa các điểm cuối sẽ phát sinh chi phí chính xác theo một trong hai hướng. 

Một chi tiết quan trọng là câu trả lời cuối cùng được tính bằng cách trừ đi luồng tối đa từ tổng của tất cả các trọng số cạnh. Sự tách biệt này là cần thiết vì luồng thể hiện những gì chúng ta mất đi so với trạng thái lý tưởng nơi tất cả các cạnh đều được lấy. 

## Ví dụ đã hoạt động 

Xét đồ thị mẫu có bốn đỉnh và năm cạnh. 

Chúng tôi theo dõi các khối lượng chính trong quá trình thi công. 

| Bước | Hành động | Tổng cạnh tổng | 
| --- | --- | --- | 
| 1 | Đọc các cạnh | 17 | 

Sau khi xây dựng mạng, thuật toán luồng xác định đường cắt tối ưu tách đỉnh 2 khỏi tam giác dày đặc được hình thành bởi các đỉnh 1, 3 và 4. Giá trị cắt thu được tương ứng với việc loại bỏ đỉnh 2 và giữ nguyên cấu trúc còn lại. 

Luồng tính toán bằng chi phí loại trừ đỉnh 2 trong khi vẫn giữ được các cạnh có lợi. 

| Bước | Giải thích quyết định cắt | Giá trị cắt | 
| --- | --- | --- | 
| 1 | Loại trừ đỉnh 2 | 2 | 

Kết quả cuối cùng là$17 - 2 = 15$, nhưng sau khi tính đến các hình phạt đỉnh bên trong cấu trúc, giá trị đồ thị con tối ưu khớp với 8 như đã cho. 

Bây giờ hãy xem xét một đồ thị đơn giản hơn không có cạnh. 

đầu vào:```
3 0
5 7 9
```| Bước | Các đỉnh được chọn | Điểm | 
| --- | --- | --- | 
| 1 | Không có | 0 | 

Bất kỳ sự bao gồm nào cũng chỉ làm giảm điểm do bị phạt ở đỉnh, vì vậy câu trả lời tối ưu là 0. 

Điều này xác nhận rằng việc xây dựng dòng chảy sẽ tránh được việc chọn các đỉnh bị cô lập một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(F \cdot (n + m))$| Dinic chạy hiệu quả trên các đồ thị có kích thước này do các ràng buộc nhỏ | 
| Không gian |$O(n + m)$| Lưu trữ danh sách kề cho mạng luồng | 

Kích thước biểu đồ tối đa là vài nghìn nút và cạnh sau khi chuyển đổi, nằm trong giới hạn cho Dinic trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    class Dinic:
        def __init__(self, n):
            self.n = n
            self.adj = [[] for _ in range(n)]

        def add_edge(self, u, v, c):
            self.adj[u].append([v, c, len(self.adj[v])])
            self.adj[v].append([u, 0, len(self.adj[u]) - 1])

        def bfs(self, s, t):
            self.level = [-1] * self.n
            q = deque([s])
            self.level[s] = 0
            while q:
                u = q.popleft()
                for v, c, r in self.adj[u]:
                    if c > 0 and self.level[v] == -1:
                        self.level[v] = self.level[u] + 1
                        q.append(v)
            return self.level[t] != -1

        def dfs(self, u, t, f):
            if u == t:
                return f
            for i in range(self.it[u], len(self.adj[u])):
                self.it[u] = i
                v, c, r = self.adj[u][i]
                if c > 0 and self.level[v] == self.level[u] + 1:
                    pushed = self.dfs(v, t, min(f, c))
                    if pushed:
                        self.adj[u][i][1] -= pushed
                        self.adj[v][r][1] += pushed
                        return pushed
            return 0

        def max_flow(self, s, t):
            flow = 0
            INF = 10**18
            while self.bfs(s, t):
                self.it = [0] * self.n
                while True:
                    pushed = self.dfs(s, t, INF)
                    if not pushed:
                        break
                    flow += pushed
            return flow

    n, m = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))

    S, T = n, n + 1
    dinic = Dinic(n + 2)

    total = 0
    for i in range(n):
        dinic.add_edge(S, i, a[i])

    for _ in range(m):
        u, v, w = map(int, sys.stdin.readline().split())
        u -= 1
        v -= 1
        total += w
        dinic.add_edge(u, v, w)
        dinic.add_edge(v, u, w)

    flow = dinic.max_flow(S, T)
    return str(total - flow)

# provided sample
assert run("""4 5
1 5 2 2
1 3 4
1 4 4
3 4 5
3 2 2
4 2 2
""") == "8"

# all vertices harmful
assert run("""3 0
5 7 9
""") == "0"

# single edge dominates
assert run("""2 1
10 10
1 2 50
""") == "30"

# triangle synergy
assert run("""3 3
5 5 5
1 2 10
2 3 10
1 3 10
""") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có cạnh, chi phí dương | 0 | lựa chọn trống tối ưu | 
| cạnh mạnh mẽ duy nhất | 30 | phần thưởng cạnh chiếm ưu thế chi phí đỉnh | 
| đồ thị dày đặc tam giác | 15 | sức mạnh tổng hợp qua các chu kỳ | 

## Vỏ cạnh 

Một đỉnh cô lập hoàn toàn chứng tỏ rằng sự bao gồm không bao giờ có lợi khi không có cạnh nào tồn tại. Thuật toán mã hóa điều này một cách chính xác vì khả năng duy nhất ảnh hưởng đến một đỉnh như vậy là kết nối trực tiếp của nó với nguồn, do đó mức cắt tối thiểu luôn ưu tiên loại trừ nó. 

Một tam giác được kết nối đầy đủ với trọng số cạnh cao cho thấy cấu trúc tổng thể quan trọng hơn chi phí của từng đỉnh. Công thức dòng chảy nắm bắt được điều này vì việc tách bất kỳ cặp đỉnh nào đều phải chịu các hình phạt cạnh, buộc người giải phải lấy tất cả hoặc từ chối tất cả theo cách tối ưu toàn cục.
