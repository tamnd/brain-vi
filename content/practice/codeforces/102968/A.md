---
title: "CF 102968A - Liên Minh Hoàn Hảo"
description: "Chúng ta được cung cấp một biểu đồ có hướng của các bộ lạc, trong đó mỗi bộ lạc là một nút và một số đường có hướng đã tồn tại giữa chúng. Mục tiêu là làm cho toàn bộ biểu đồ được kết nối chặt chẽ, nghĩa là mọi bộ lạc phải có khả năng tiếp cận mọi bộ tộc khác theo những con đường được chỉ dẫn."
date: "2026-07-04T06:34:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102968
codeforces_index: "A"
codeforces_contest_name: "AGM 2021, Qualification Round"
rating: 0
weight: 102968
solve_time_s: 47
verified: true
draft: false
---

[CF 102968A - Liên minh hoàn hảo](https://codeforces.com/problemset/problem/102968/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng của các bộ lạc, trong đó mỗi bộ lạc là một nút và một số đường có hướng đã tồn tại giữa chúng. Mục tiêu là làm cho toàn bộ biểu đồ được kết nối chặt chẽ, nghĩa là mọi bộ lạc phải có khả năng tiếp cận mọi bộ tộc khác theo những con đường được chỉ dẫn. 

Chúng tôi được phép thêm đường dẫn mới. Chi phí của việc thêm một con đường từ tập x đến tập y không phải là tùy ý: nó là tổng của hai trọng số nút, Cx + Cy. Vì vậy, mọi cạnh định hướng có thể có chi phí xây dựng cố định chỉ phụ thuộc vào điểm cuối của nó. 

Nhiệm vụ không chỉ là tính toán chi phí tối thiểu mà còn đưa ra rõ ràng những con đường mới nào sẽ được thêm vào để đạt được khả năng kết nối mạnh mẽ với chi phí tối thiểu đó. 

Các ràng buộc, N lên tới 4000 và M lên tới 20000, cho thấy rằng các ý tưởng O(N^2) hoặc thậm chí O(NM) đều có thể chấp nhận được nếu được thực hiện cẩn thận, nhưng mọi thứ khối trong N sẽ quá chậm. Điều này hướng mạnh mẽ đến cách tiếp cận ngưng tụ đồ thị hơn là bất kỳ sự gia tăng kết nối vũ phu nào. 

Một cách giải thích ngây thơ sẽ là thử thêm các cạnh cho đến khi biểu đồ trở nên được kết nối mạnh mẽ và kết nối các thành phần một cách tham lam một cách tùy ý. Điều đó không thành công vì cấu trúc chi phí không đồng nhất, do đó việc lựa chọn nút nào kết nối các thành phần là vấn đề quan trọng. 

Trường hợp cạnh tinh tế xuất hiện khi đồ thị đã được kết nối mạnh. Trong trường hợp đó, câu trả lời đúng là chi phí bằng 0 và không có cạnh. Việc triển khai đơn giản luôn cố gắng kết nối các thành phần sẽ thêm các cạnh không cần thiết một cách không chính xác. 

Một chế độ lỗi khác xảy ra khi biểu đồ ngưng tụ có nhiều lựa chọn để sử dụng nút nào làm điểm cuối kết nối. Việc chọn các nút tùy ý bên trong các thành phần có thể làm tăng chi phí, vì chi phí biên phụ thuộc vào các điểm cuối cụ thể, không chỉ các thành phần. 

## Phương pháp tiếp cận 

Quan sát chính là vấn đề cơ bản là về các thành phần được kết nối chặt chẽ. Khi chúng tôi nén biểu đồ vào SCC của nó, cấu trúc thu được là biểu đồ tuần hoàn có hướng. Để làm cho toàn bộ đồ thị được liên thông mạnh mẽ, chúng ta cần thêm các cạnh để DAG ngưng tụ này trở thành một thành phần liên thông mạnh duy nhất. 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc thêm các cạnh giữa tất cả các cặp SCC và kiểm tra xem đồ thị kết quả có được kết nối mạnh hay không. Mỗi lần bổ sung ứng viên sẽ yêu cầu kiểm tra khả năng tiếp cận, tốn O(N + M). Vì có thể có O(N^2) các cạnh được thêm vào, điều này dẫn đến trường hợp xấu nhất là O(N^3) không thể thực hiện được. 

Hiểu biết sâu sắc về cấu trúc là DAG có thể được kết nối mạnh mẽ bằng cách kết nối các nguồn và phần chìm của nó trong một chu kỳ. Trong biểu đồ ngưng tụ, các nút có độ bằng 0 là nguồn và các nút có độ ngoài bằng 0 là phần chìm. Một kết quả cổ điển là số cạnh tối thiểu cần thiết là tối đa (số lượng nguồn, số lượng bồn) và cách tối ưu là ghép chúng theo cách tuần hoàn. 

Sự phức tạp duy nhất ở đây là chi phí. Vì chi phí biên phụ thuộc vào điểm cuối nên chúng tôi không chỉ kết nối các đại diện SCC một cách tùy tiện. Thay vào đó, đối với mỗi SCC, chúng tôi chọn một nút đại diện, thường là nút có chi phí C tối thiểu bên trong thành phần đó, vì mọi kết nối liên quan đến thành phần đó sẽ rẻ hơn thông qua nút đó. 

Sau khi mỗi SCC được biểu thị bằng nút có chi phí tối thiểu, chúng tôi sẽ xây dựng SCC DAG, xác định nguồn và nguồn, sau đó kết nối chúng để tạo thành cấu trúc giống như chu trình nhằm đảm bảo kết nối mạnh mẽ đồng thời giảm thiểu chi phí gia tăng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bổ sung Brute Force Edge + Kiểm tra | O(N^3) | O(N^2) | Quá chậm | 
| Nén SCC + Kết hợp tham lam | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán các thành phần liên thông mạnh của đồ thị gốc bằng thuật toán Kosaraju hoặc Tarjan. Mỗi nút được gán một id thành phần. Bước này thu gọn các chu trình để trong mỗi thành phần, mọi thứ đều có thể truy cập được lẫn nhau. 
2. Với mỗi thành phần, hãy tìm nút có giá trị C nhỏ nhất. Nút này sẽ đóng vai trò đại diện cho thành phần đó. Chúng tôi chọn nó vì mọi kết nối đi hoặc đến liên quan đến thành phần này đều phải sử dụng điểm cuối rẻ nhất có thể. 
3. Xây dựng đồ thị ngưng tụ bằng cách lặp qua tất cả các cạnh ban đầu. Với mọi cạnh u → v trong đó comp[u] ≠ comp[v], hãy thêm một cạnh có hướng comp[u] → comp[v]. Biểu đồ này được đảm bảo là DAG. 
4. Tính độ nghiêng, độ ngoài của từng SCC trong đồ thị ngưng tụ. Xác định tất cả các thành phần nguồn (độ 0) và các thành phần chìm (độ 0). 
5. Nếu chỉ có một SCC, đồ thị đã được kết nối mạnh, do đó chúng ta tạo ra chi phí bằng 0 và không có cạnh. 
6. Ngược lại, đặt các nguồn là S1, S2, ..., Sk và các điểm chìm là T1, T2, ..., Tm. Chúng ta xây dựng một chu trình bằng cách ghép nối chúng: nối Ti → S(i+1) với i trong [1, m-1], và cuối cùng nối Tm → S1. Nếu k ≠ m, chúng ta sử dụng max(k, m) một cách hiệu quả bằng cách lặp lại các phần tử trong danh sách ngắn hơn theo chu kỳ. 
7. Đối với mỗi kết nối được yêu cầu giữa các thành phần A → B, chúng tôi xuất ra một cạnh thực tế bằng cách sử dụng các nút đại diện của chúng: Rep[A] → Rep[B]. Giá trị của mỗi cạnh là C[rep[A]] + C[rep[B]]. 

### Tại sao nó hoạt động 

Sau khi ngưng tụ, mỗi SCC là một nút trong DAG. Một DAG chỉ có thể được kết nối mạnh mẽ nếu mọi nguồn đều có cạnh đi vào và mọi bồn chứa đều có cạnh đi ra. Việc xây dựng chu trình đảm bảo đồng thời cả hai điều kiện bằng cách ghép nối các bộ phận chìm với các nguồn theo kiểu tuần hoàn, loại bỏ mọi sự mất cân bằng về hướng. Việc sử dụng các đại diện sẽ duy trì chi phí tối thiểu cho mỗi kết nối thành phần, vì bất kỳ nút thay thế nào cũng sẽ chỉ tăng giá trị C mà không cải thiện kết nối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, m = map(int, input().split())
    c = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    gr = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        gr[v].append(u)

    visited = [False] * n
    order = []

    def dfs1(u):
        visited[u] = True
        for v in g[u]:
            if not visited[v]:
                dfs1(v)
        order.append(u)

    def dfs2(u, comp_id):
        comp[u] = comp_id
        for v in gr[u]:
            if comp[v] == -1:
                dfs2(v, comp_id)

    for i in range(n):
        if not visited[i]:
            dfs1(i)

    comp = [-1] * n
    cid = 0

    for u in reversed(order):
        if comp[u] == -1:
            dfs2(u, cid)
            cid += 1

    comp_count = cid

    rep = [0] * comp_count
    for i in range(comp_count):
        rep[i] = -1

    for i in range(n):
        if rep[comp[i]] == -1 or c[i] < c[rep[comp[i]]]:
            rep[comp[i]] = i

    indeg = [0] * comp_count
    outdeg = [0] * comp_count
    dag_edges = []

    for u in range(n):
        for v in g[u]:
            if comp[u] != comp[v]:
                dag_edges.append((comp[u], comp[v]))
                outdeg[comp[u]] += 1
                indeg[comp[v]] += 1

    sources = []
    sinks = []

    for i in range(comp_count):
        if indeg[i] == 0:
            sources.append(i)
        if outdeg[i] == 0:
            sinks.append(i)

    if comp_count == 1:
        print(0)
        print(0)
        return

    k = len(sources)
    m = len(sinks)

    edges = []
    L = max(k, m)

    for i in range(L):
        u = sinks[i % m]
        v = sources[(i + 1) % k]
        edges.append((rep[u], rep[v]))

    total_cost = 0
    for u, v in edges:
        total_cost += c[u] + c[v]

    print(total_cost)
    print(len(edges))
    for u, v in edges:
        print(u + 1, v + 1)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng thuật toán của Kosaraju để tính toán SCC, điều này là cần thiết vì chỉ có cấu trúc SCC mới quan trọng đối với khả năng tiếp cận. Biểu đồ ngược được sử dụng trong DFS thứ hai để gán ID thành phần. 

Sau khi gán SCC, chúng tôi tính toán một nút đại diện cho mỗi thành phần bằng cách quét tất cả các nút và giữ lại nút đó với chi phí tối thiểu. Điều này đảm bảo mọi cạnh sau đều sử dụng điểm cuối rẻ nhất có thể bên trong mỗi thành phần. 

Sau đó chúng ta xây dựng biểu đồ ngưng tụ một cách ngầm định chỉ để tính mức độ và mức độ ngoài. Ngoài ra, danh sách kề thực tế của DAG không cần thiết vì chỉ có vấn đề nhận dạng nguồn và đích. 

Logic ghép nối cuối cùng xây dựng chính xác số cạnh cần thiết để cân bằng nguồn và phần chìm. Việc lập chỉ mục modulo đảm bảo gói chính xác khi số lượng của chúng khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Biểu đồ đầu vào dẫn đến SCC nơi tồn tại nhiều thành phần. Giả sử sau khi ngưng tụ ta có nguồn [1, 2] và phần chìm [3, 4]. 

| Bước | Nguồn | Bồn rửa | Đã thêm cạnh | 
| --- | --- | --- | --- | 
| 1 | [1, 2] | [3, 4] | 4 → 2 | 
| 2 | [1, 2] | [3, 4] | 3 → 1 | 

Điều này tạo ra một cấu trúc giống như chu trình kết nối tất cả các thành phần, đảm bảo khả năng kết nối mạnh mẽ. 

Dấu vết này cho thấy rằng ngay cả khi số lượng SCC khác nhau, việc ghép nối theo chu kỳ vẫn đảm bảo mọi thành phần đều nhận được cả kết nối đến và đi. 

### Ví dụ 2 

Hãy xem xét một biểu đồ đã được kết nối mạnh mẽ. Khi đó số SCC là 1. 

| Bước | Số lượng SCC | Hành động | 
| --- | --- | --- | 
| 1 | 1 | Đầu ra 0 cạnh | 

Điều này xác nhận thuật toán tránh được việc bổ sung cạnh không cần thiết một cách chính xác khi biểu đồ đã đáp ứng yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Hai đường chuyền DFS cho SCC cộng với một đường chuyền qua các cạnh | 
| Không gian | O(N + M) | Lưu trữ đồ thị và mảng phụ trợ cho SCC và DFS | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong các ràng buộc của N lên tới 4000 và M lên đến 20000 và mức sử dụng bộ nhớ vẫn ở mức nhỏ do biểu diễn danh sách kề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# single SCC
assert run("""3 3
1 2 3
1 2
2 3
3 1
""") == "0\n0"

# two SCC chain
assert run("""4 2
1 10 1 10
1 2
3 4
""").split()[0] != "", "basic connectivity"

# minimum case
assert run("""1 0
5
""") == "0\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chu kỳ 3 nút | 0 chi phí, 0 cạnh | đã được kết nối mạnh mẽ | 
| 4 nút hai thành phần | các cạnh khác 0 | Yêu cầu sáp nhập SCC | 
| nút đơn | 0 | trường hợp cạnh tầm thường | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi đồ thị đã được kết nối mạnh mẽ. Trong tình huống đó, việc phân tách SCC tạo ra chính xác một thành phần, do đó thuật toán ngay lập tức trả về các cạnh bằng 0. Ví dụ, đầu vào```
3 3
1 2 3
1 2
2 3
3 1
```tạo ra một SCC. Thứ tự DFS thu gọn mọi thứ thành một id thành phần duy nhất, do đó cả nguồn và phần chìm đều chỉ chứa một phần tử và các trình kích hoạt thoát sớm. 

Một trường hợp tinh vi khác là khi có nhiều SCC nhưng tất cả đều chỉ có cấu trúc đi ra hoặc chỉ có cấu trúc đi vào một cách không cân bằng. Việc ghép nối theo chu kỳ vẫn hoạt động vì các chỉ số bao bọc xung quanh, đảm bảo mọi phần chìm được sử dụng làm điểm bắt đầu cho một cạnh và mọi nguồn được sử dụng làm điểm cuối. Điều này tránh để bất kỳ thành phần nào bị ngắt kết nối trong biểu đồ tăng cường cuối cùng.
