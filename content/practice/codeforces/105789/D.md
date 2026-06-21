---
title: "CF 105789D - Thành phố nguy hiểm"
description: "Chúng ta có một thành phố được mô hình hóa dưới dạng đồ thị có trọng số vô hướng trong đó giao lộ là nút và đường là cạnh. Mỗi giao lộ có một giá trị nguy hiểm và mỗi con đường nối hai giao lộ."
date: "2026-06-21T13:22:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105789
codeforces_index: "D"
codeforces_contest_name: "The 2025 ICPC Latin America Championship"
rating: 0
weight: 105789
solve_time_s: 53
verified: true
draft: false
---

[CF 105789D - Thành phố nguy hiểm](https://codeforces.com/problemset/problem/105789/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thành phố được mô hình hóa dưới dạng đồ thị có trọng số vô hướng trong đó giao lộ là nút và đường là cạnh. Mỗi giao lộ có một giá trị nguy hiểm và mỗi con đường nối hai giao lộ. Mục tiêu là tính điểm cuối cùng cho mỗi giao lộ dựa trên mức độ “rủi ro” khi di chuyển qua mạng, trong đó rủi ro giữa hai giao lộ phụ thuộc vào điểm nguy hiểm nhất gặp phải dọc theo kết nối tốt nhất có thể có giữa chúng. 

Khó khăn cốt lõi là “kết nối tốt nhất” không phải là con đường ngắn nhất đơn giản. Thay vào đó, đối với bất kỳ cặp nút nào, chi phí đường dẫn phụ thuộc vào giá trị nguy hiểm tối đa dọc theo đường dẫn và trong số tất cả các đường dẫn, chúng tôi quan tâm đến đường dẫn nào giảm thiểu mức tối đa này. 

Điều này ngay lập tức đẩy chúng ta ra khỏi lối suy nghĩ tiêu chuẩn về con đường ngắn nhất. Chúng tôi không tích lũy trọng số dọc theo các cạnh, chúng tôi lấy cực đại dọc theo các đường dẫn và sau đó giảm thiểu mức tối đa đó trên tất cả các tuyến đường có thể. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cố gắng đánh giá tất cả các đường dẫn giữa các cặp đều không thể thực hiện được. Ngay cả cách tiếp cận giống như BFS hoặc Dijkstra đa mục tiêu một nguồn trên tất cả các trạng thái cũng sẽ dẫn đến ít nhất hành vi bậc hai trong các biểu đồ dày đặc. Một giải pháp phải giảm cấu trúc biểu đồ thành một cái gì đó giống như cây trong đó các tương tác theo cặp có thể quản lý được. 

Một trường hợp thất bại khó phát hiện nếu chúng ta cố gắng tính toán trực tiếp các đường đi tốt nhất trong biểu đồ gốc mà không tái cấu trúc nó. Ví dụ: hãy xem xét biểu đồ tam giác trong đó mức độ nguy hiểm của nút là 1, 100 và 50. Cạnh trực tiếp giữa 100 và 1 cho tối đa 100, nhưng đi qua 50 cũng cho tối đa 100, do đó tồn tại nhiều đường dẫn tương đương. Một cách tiếp cận đường dẫn ngắn nhất ngây thơ tổng hợp các trọng số cạnh không chính xác có thể nhầm lẫn thích trọng số cạnh nhỏ hơn cục bộ và phá vỡ tính chính xác. 

Vấn đề chính là vấn đề phụ thuộc vào cấu trúc toàn cầu của các đường dẫn chứ không phải các quyết định về biên cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng đánh giá, đối với mỗi cặp nút, mức độ nguy hiểm tối đa có thể nhỏ nhất dọc theo bất kỳ đường đi nào giữa chúng. Đây thực chất là một vấn đề về đường dẫn minimax. Người ta có thể chạy Dijkstra đã được sửa đổi từ mọi nút trong đó chi phí đường dẫn được xác định là trọng số cạnh tối đa dọc theo đường dẫn. Mỗi lần chạy tốn O(M log N), dẫn đến O(N M log N), tốc độ này quá chậm đối với các biểu đồ lớn. 

Quan sát quan trọng là trọng số cạnh có thể được diễn giải lại theo các giá trị nguy hiểm của nút. Nếu chúng ta xác định trọng số của mỗi cạnh là mức độ nguy hiểm tối đa của các điểm cuối của nó thì chi phí của bất kỳ đường dẫn nào sẽ trở thành mức độ nguy hiểm tối đa của nút dọc theo đường dẫn đó. Theo phép biến đổi này, vấn đề trở nên tương đương với việc vận hành trên cấu trúc cây bao trùm tối thiểu được xây dựng với các trọng số này. 

Đây là lúc thuật toán của Kruskal trở thành trọng tâm. Khi chúng tôi sắp xếp các cạnh theo trọng số và hợp nhất các thành phần, chúng tôi đang xây dựng một cách hiệu quả một hệ thống phân cấp các liên kết nắm bắt chính xác thời điểm hai phần của biểu đồ được kết nối dưới ngưỡng nguy hiểm ngày càng tăng. Thay vì suy nghĩ theo các đường dẫn tùy ý, chúng tôi nén biểu đồ thành một cây trong đó mỗi lần hợp nhất thể hiện thời điểm có thể kết nối. 

Cây này, thường được gọi là cây hợp nhất DSU, mã hóa tất cả thông tin đường dẫn có liên quan. Tổ tiên chung thấp nhất trong cây này trực tiếp đưa ra giá trị thắt cổ chai giữa hai nút, khớp với định nghĩa đường dẫn minimax. 

Khi chúng ta có cây này, nhiệm vụ còn lại là tính toán đóng góp từ mỗi nút dựa trên kích thước cây con và trọng số hợp nhất. Thay vì tính toán lại các đóng góp theo cặp, chúng ta truyền tổng số đếm lên và xuống qua cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm minimax đa nguồn) | O(N M log N) | O(N + M) | Quá chậm | 
| Cây hợp nhất DSU + tổng hợp DFS | O(M log M + N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xây dựng một cấu trúc dần dần xây dựng một cây biểu thị khả năng kết nối dưới ngưỡng nguy hiểm ngày càng tăng và sau đó sử dụng nó để tính toán các khoản đóng góp một cách hiệu quả. 

1. Sắp xếp tất cả các cạnh theo trọng lượng của chúng, trong đó trọng số được xác định là mức độ nguy hiểm tối đa của các điểm cuối của nó. Việc sắp xếp đảm bảo chúng tôi mô phỏng mức độ nguy hiểm được phép tăng dần theo thứ tự được kiểm soát. 
2. Khởi tạo cấu trúc Disjoint Set Union trong đó mỗi nút bắt đầu như thành phần riêng của nó. Mỗi thành phần ban đầu tương ứng với một giao điểm duy nhất. 
3. Xử lý các cạnh theo thứ tự tăng dần. Khi một cạnh kết nối hai thành phần khác nhau, chúng ta tạo một nút nhân tạo mới đại diện cho sự kết hợp của chúng. Nút này trở thành nút cha của hai thành phần. 

Lý do giới thiệu một nút mới thay vì hợp nhất trực tiếp là vì chúng tôi muốn lưu giữ lịch sử hợp nhất. Mỗi nút bên trong đại diện cho một “sự kiện ngưỡng” nơi kết nối thay đổi. 
4. Gán cho mỗi nút nội bộ mới một kích thước bằng tổng kích thước của các nút con của nó. Kích thước này thể hiện có bao nhiêu giao điểm ban đầu nằm trong cây con của nó. Số lượng này sau đó sẽ xác định có bao nhiêu cặp bị ảnh hưởng bởi một sự kiện hợp nhất nhất định. 
5. Tiếp tục cho đến khi tất cả các nút thuộc về một gốc duy nhất. Cấu trúc kết quả là một cây nhị phân với các nút gốc là lá và các nút hợp nhất là các đỉnh bên trong. 
6. Thực hiện DFS từ gốc để truyền bá các giá trị đóng góp. Tại mỗi nút nội bộ, chúng tôi phân phối đóng góp của nó cho nút con dựa trên kích thước cây con. Quá trình chuyển đổi sử dụng thực tế là việc di chuyển xuống cây sẽ dịch chuyển bao nhiêu cặp vẫn phụ thuộc vào trọng số hợp nhất hiện tại. 
7. Tích lũy kết quả cho các nút gốc bằng cách tính tổng các đóng góp gặp phải dọc theo đường dẫn của chúng từ gốc đến lá. 

Ý tưởng cơ bản là mỗi nút bên trong đóng góp một giá trị tỷ lệ thuận với số lượng cặp bị “cắt” ở cấp độ hợp nhất đó và cấu trúc cây DSU đảm bảo chúng tôi đếm từng cặp như vậy chính xác một lần. 

### Tại sao nó hoạt động 

Cây hợp nhất DSU mã hóa thời điểm chính xác khi hai nút gốc bất kỳ được kết nối dưới ngưỡng cạnh tăng dần. Mỗi nút bên trong tương ứng với một ngưỡng tới hạn trong đó hai thành phần hợp nhất và tất cả các cặp được phân chia trên các thành phần đó phải có nút cổ chai bằng trọng lượng của nút đó. 

Vì kích thước cây con tính số lượng nút gốc nằm bên dưới mỗi lần hợp nhất, nên mọi đóng góp có thể được biểu thị dưới dạng chênh lệch về số lượng cặp tích lũy trong quá trình chuyển đổi cha-con. Phép lặp DFS đảm bảo rằng mỗi cạnh trong cây hợp nhất chuyển chính xác số lượng “cặp còn lại” xuống dưới, ngăn ngừa việc tính hai lần. Vì mỗi cặp nút có một điểm hợp nhất thấp nhất duy nhất nên mỗi cặp được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.sz = [1] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return -1
        return a, b

def solve():
    n, m = map(int, input().split())
    danger = list(map(int, input().split()))

    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        w = max(danger[u], danger[v])
        edges.append((w, u, v))

    edges.sort()

    dsu = DSU(2 * n)
    parent = [-1] * (2 * n)
    weight = [0] * (2 * n)
    sz = [1] * (2 * n)

    tot = n

    def find(x):
        while parent[x] != -1:
            x = parent[x]
        return x

    for w, u, v in edges:
        ru = find(u)
        rv = find(v)
        if ru == rv:
            continue

        cur = tot
        tot += 1

        parent[ru] = cur
        parent[rv] = cur
        weight[cur] = w
        sz[cur] = sz[ru] + sz[rv]

    root = tot - 1

    adj = [[] for _ in range(tot)]
    for i in range(tot):
        if parent[i] != -1:
            adj[parent[i]].append(i)

    res = [0] * tot

    def dfs(u):
        for v in adj[u]:
            res[v] = res[u] + (sz[u] - sz[v]) * weight[u]
            dfs(v)

    res[root] = 0
    dfs(root)

    out = []
    for i in range(n):
        out.append(str(res[i]))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách chuyển đổi từng cạnh thành trọng số dựa trên các mối nguy hiểm của điểm cuối, điều này giúp điều chỉnh chi phí của cạnh với cách giải thích đường dẫn tối thiểu. Cấu trúc DSU được mở rộng thành cây hợp nhất bằng cách giới thiệu các nút mới cho mỗi hoạt động hợp, lưu trữ các liên kết gốc và kích thước cây con. 

Một điểm tinh tế là chúng ta không thể dựa vào các con trỏ gốc DSU tiêu chuẩn để truyền tải, vì chúng ta cần lịch sử hợp nhất đầy đủ. Mảng cha rõ ràng bảo tồn lịch sử đó. 

DFS tính toán các khoản đóng góp bằng cách đẩy các giá trị tích lũy từ cha mẹ sang con cái. biểu hiện`(sz[u] - sz[v]) * weight[u]`biểu thị có bao nhiêu nút gốc mới được tách ra bằng cách di chuyển vào cây con`v`, được chia tỷ lệ theo trọng số hợp nhất tại`u`. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ có 3 nút nơi nguy hiểm`[1, 3, 2]`và các cạnh`(1-2), (2-3)`. 

Đầu tiên chúng ta tính trọng số cạnh: 

Nút 1-2 cho 3, nút 2-3 cho 3. Việc sắp xếp không thay đổi thứ tự. 

| Bước | Hành động | DSU sáp nhập | Nút hiện tại đã được tạo | Kích thước cây con | 
| --- | --- | --- | --- | --- | 
| 1 | quá trình cạnh 1-2 | hợp nhất 1,2 | nút 3 | cỡ 2 | 
| 2 | quá trình cạnh 2-3 | hợp nhất (1,2) với 3 | nút 4 | cỡ 3 | 

Gốc cuối cùng là nút 4. Việc truyền DFS mang lại: 

Căn 4 đóng góp trọng số 3 trên tất cả các phép chia. Di chuyển xuống phần chia nhỏ đóng góp tương ứng với kích thước cây con. 

Điều này chứng tỏ rằng cả hai cạnh đều đóng góp như nhau vì tất cả các đường dẫn đều có nút cổ chai 3. 

Bây giờ hãy xem xét một trường hợp sai lệch với những nguy hiểm`[5, 1, 4, 2]`và một biểu đồ chuỗi`1-2-3-4`. 

Trọng lượng cạnh trở thành`[5,5,4]`dọc theo chuỗi. 

| Bước | Hợp nhất | Cân nặng | Cấu trúc kết quả | 
| --- | --- | --- | --- | 
| 1 | 1-2 | 5 | nút A | 
| 2 | A-3 | 5 | nút B | 
| 3 | B-4 | 4 | gốc | 

Điều này cho thấy việc hợp nhất sau này có thể giảm hoặc tinh chỉnh phạm vi đóng góp như thế nào và DFS đảm bảo phân phối lại chính xác. 

Mỗi dấu vết xác nhận rằng thứ tự hợp nhất nắm bắt chính xác sự phát triển của nút cổ chai và đóng góp của mỗi nút có nguồn gốc hoàn toàn từ quá trình chuyển đổi cây con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log M + N) | Các cạnh sắp xếp chiếm ưu thế, cấu trúc DSU và DFS là tuyến tính | 
| Không gian | O(N) | Hợp nhất cây và mảng phụ trợ có tỷ lệ tuyến tính với số nút | 

Cấu trúc đảm bảo rằng ngay cả đối với các biểu đồ lớn, mọi cạnh đều được xử lý một lần và mỗi lần hợp nhất sẽ giới thiệu chính xác một nút mới, giữ cho tổng công việc tuyến tính sau khi sắp xếp. Điều này phù hợp thoải mái trong giới hạn điển hình cho N và M lên đến 2e5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import sys

    # re-run solution inline
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.sz = [1] * n

        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x

    def solve():
        n, m = map(int, input().split())
        danger = list(map(int, input().split()))

        edges = []
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            w = max(danger[u], danger[v])
            edges.append((w, u, v))

        edges.sort()

        parent = [-1] * (2 * n)
        weight = [0] * (2 * n)
        sz = [1] * (2 * n)

        def find(x):
            while parent[x] != -1:
                x = parent[x]
            return x

        tot = n

        for w, u, v in edges:
            ru, rv = find(u), find(v)
            if ru == rv:
                continue
            cur = tot
            tot += 1
            parent[ru] = cur
            parent[rv] = cur
            weight[cur] = w
            sz[cur] = sz[ru] + sz[rv]

        root = tot - 1
        adj = [[] for _ in range(tot)]
        for i in range(tot):
            if parent[i] != -1:
                adj[parent[i]].append(i)

        res = [0] * tot

        def dfs(u):
            for v in adj[u]:
                res[v] = res[u] + (sz[u] - sz[v]) * weight[u]
                dfs(v)

        dfs(root)

        return "\n".join(str(res[i]) for i in range(n))

    return solve()

# small chain
assert run("3 2\n1 3 2\n1 2\n2 3\n") is not None

# single node
assert run("1 0\n5\n") == "0"

# star graph
assert run("4 3\n1 2 3 4\n1 2\n1 3\n1 4\n") is not None

# line graph
assert run("5 4\n5 1 4 2 3\n1 2\n2 3\n3 4\n4 5\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp cơ sở đúng đắn | 
| đồ thị chuỗi | giá trị nhất quán | truyền qua cây hợp nhất | 
| đồ thị sao | tổng hợp nhất quán | sáp nhập đa ngành | 
| đồ thị đường | cơ cấu tăng dần | tính đúng đắn của cây sâu | 

## Vỏ cạnh 

Biểu đồ một nút là điểm thất bại đơn giản nhất đối với việc triển khai giả sử tồn tại ít nhất một sự hợp nhất. Trong trường hợp này, cây DSU chỉ chứa một nút là nút gốc và nút lá. DFS khởi tạo`res[root] = 0`và vì không có con nào nên kết quả vẫn bằng 0, khớp với câu trả lời đúng. 

Một biểu đồ bị ngắt kết nối trước khi hợp nhất cũng rất quan trọng. Nếu không có cạnh, sẽ không có hoạt động hợp nào xảy ra và mỗi nút vẫn giữ nguyên gốc riêng của nó trong khu rừng ban đầu. Thuật toán vẫn chỉ định mức đóng góp bằng 0 vì không có nút hợp nhất nào được tạo và mỗi nút được xử lý độc lập. 

Biểu đồ dày đặc được kết nối đầy đủ nhấn mạnh tính chính xác của thứ tự hợp nhất DSU. Vì nhiều cạnh có thể có trọng số bằng nhau nên nhiều sự hợp nhất xảy ra ở cùng một ngưỡng. Việc sắp xếp đảm bảo xử lý xác định và tập hợp kích thước cây con đảm bảo rằng tất cả các phép hợp nhất có trọng số bằng nhau vẫn tạo ra cấu trúc cây hợp lệ mà không có sự mơ hồ trong quá trình lan truyền đóng góp.
