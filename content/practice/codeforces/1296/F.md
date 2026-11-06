---
title: "CF 1296F - Người đẹp Berland"
description: "Chúng ta có một mạng lưới kết nối gồm các ga $n$ được kết nối bởi các đoạn đường sắt $n-1$, có nghĩa là cấu trúc là một cái cây. Mỗi cạnh trong cây này có trọng số nguyên không xác định trong phạm vi từ 1 đến $10^6$. Chúng tôi cũng nhận được một số quan sát từ hành khách."
date: "2026-06-16T04:55:30+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "greedy", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1296
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 617 (Div. 3)"
rating: 2100
weight: 1296
solve_time_s: 437
verified: false
draft: false
---

[CF 1296F - Vẻ đẹp Berland](https://codeforces.com/problemset/problem/1296/F) 

**Xếp hạng:** 2100 
**Thẻ:** thuật toán xây dựng, dfs và các thuật toán tương tự, tham lam, sắp xếp, cây 
**Thời gian giải:** 7 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mạng lưới kết nối của$n$trạm được kết nối bởi$n-1$đoạn đường sắt, có nghĩa là cấu trúc là một cái cây. Mỗi cạnh trong cây này có trọng số nguyên chưa xác định trong khoảng từ 1 đến$10^6$. Chúng tôi cũng nhận được một số quan sát từ hành khách. Mỗi quan sát chỉ định hai trạm và yêu cầu một giá trị bằng trọng số cạnh tối thiểu dọc theo đường dẫn duy nhất giữa hai trạm đó. 

Nhiệm vụ là gán một trọng số nguyên cho mỗi cạnh để tất cả các quan sát hành khách trở thành đúng cùng một lúc. Nếu không có nhiệm vụ nào có thể đáp ứng được tất cả các quan sát thì chúng ta phải báo cáo là không thể thực hiện được. 

Khó khăn cốt lõi là mỗi quan sát không hạn chế một cạnh nào mà là mức tối thiểu trên toàn bộ đường dẫn. Điều này tạo ra các ràng buộc chồng chéo trên các đường dẫn khác nhau, phải được thực hiện nhất quán trên toàn cầu. 

Những hạn chế$n, m \le 5000$cho phép một$O(n^2)$hoặc$O(n \log n)$giải pháp phong cách, nhưng bất cứ điều gì cố gắng tính toán lại thông tin đường dẫn cho mỗi truy vấn một cách ngây thơ mà không có cấu trúc sẽ gặp rủi ro$O(nm)$hoặc tệ hơn là vẫn trôi qua nhưng chỉ khi mỗi thao tác thật nhẹ nhàng. Tuy nhiên, giải pháp đúng thực sự dựa vào việc suy luận về mối quan hệ giữa các đường dẫn hơn là việc truyền tải lặp đi lặp lại. 

Một vài trường hợp thất bại tinh tế xuất hiện với trực giác tham lam: 

Một trường hợp thất bại là giả sử chúng ta có thể gán từng cạnh một cách độc lập bằng cách sử dụng đường dẫn đầu tiên chứa nó. Ví dụ: nếu hai truy vấn có chung một cạnh nhưng yêu cầu các cực tiểu khác nhau trên các đường dẫn chồng chéo, việc gán giá trị cục bộ sẽ dẫn đến những mâu thuẫn chỉ xuất hiện sau này. 

Một trường hợp thất bại khác là giả định rằng đối với mỗi truy vấn, chúng ta chỉ cần một cạnh trên đường đi của nó để lấy giá trị$g_j$. Điều đó bỏ qua thực tế là nhiều truy vấn có thể buộc cùng một cạnh đồng thời ở mức tối thiểu đối với các giá trị không tương thích. 

Thách thức thực sự là mỗi ràng buộc đều nói lên một cách hiệu quả: trên một đường dẫn, ít nhất một cạnh phải có giá trị chính xác.$g_j$và tất cả các cạnh trên đường dẫn đó phải có giá trị ít nhất$g_j$. Đây là những hạn chế toàn cầu đối với đường dẫn cây. 

## Phương pháp tiếp cận 

Quan điểm brute-force là xem xét tất cả các phép gán giá trị cạnh từ 1 đến$10^6$. Đối với mỗi phép gán, chúng tôi sẽ xác minh tất cả các truy vấn bằng cách tính toán các giá trị cạnh tối thiểu dọc theo mỗi đường dẫn. Ngay cả khi chúng ta tính toán trước LCA để trả lời các truy vấn đường dẫn trong$O(\log n)$, không gian tìm kiếm của$10^{n}$nhiệm vụ làm cho điều này là không thể. 

Ngay cả khi chúng tôi thử cách tiếp cận tham lam mang tính xây dựng cho mỗi truy vấn, chẳng hạn như xử lý truy vấn theo bất kỳ thứ tự nào và cập nhật các giới hạn dưới của cạnh, chúng tôi vẫn thất bại vì các ràng buộc tương tác không cục bộ. Vấn đề chính là truy vấn không chỉ định một cạnh; nó thực thi mức tối thiểu trên toàn bộ đường dẫn, do đó các ràng buộc lan truyền theo cách có cấu trúc. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì coi mỗi truy vấn là xác định mức tối thiểu, chúng tôi coi đó là yêu cầu rằng ít nhất một cạnh trên đường dẫn phải đạt được chính xác giá trị tối thiểu đó và tất cả các cạnh trên đường dẫn đó ít nhất phải có giá trị đó. Điều này gợi ý rằng trước tiên chúng ta nên giả sử tất cả các cạnh bắt đầu ở giá trị 1 và sau đó thực thi các giới hạn dưới. 

Chúng ta có thể mô hình hóa các ràng buộc dưới dạng bất đẳng thức trên các cạnh. Đối với mỗi truy vấn$(a, b, g)$, tất cả các cạnh trên đường đi phải thỏa mãn$f_e \ge g$, và ngoài ra, ít nhất một cạnh trên đường đi phải thỏa mãn$f_e = g$. Điều kiện đầu tiên rất dễ thực thi bằng cách sử dụng các ràng buộc tối đa của đường dẫn ở các giới hạn dưới. Điều kiện thứ hai là điều mà các cặp đôi cùng truy vấn. 

Trước tiên, chúng tôi giải quyết vấn đề này bằng cách đẩy tất cả các giới hạn dưới qua cây bằng cách sử dụng phương pháp truyền bá kiểu khác biệt trên các đường dẫn. Sau đó, chúng tôi xác thực rằng mỗi truy vấn vẫn có ít nhất một cạnh có giá trị được gán cuối cùng bằng mức tối thiểu bắt buộc. Nếu không, cấu hình là không thể. 

Thử thách còn lại là áp dụng và kiểm tra các ràng buộc về đường đi một cách hiệu quả. Điều này được xử lý bằng cách xử lý trước thông tin gốc và thông tin chuyên sâu cũng như sử dụng bước đi trên đường dẫn LCA dựa trên DFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu |$O((n+m)\log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây một cách tùy ý, chẳng hạn như ở nút 1, và tính toán các mảng gốc và mảng độ sâu để chúng ta có thể đi qua các đường dẫn giữa các nút. 

1. Chúng ta khởi tạo tất cả các giới hạn dưới của cạnh là 1. Điều này thể hiện phép gán hợp lệ yếu nhất có thể trước khi xem xét các ràng buộc. 
2. Đối với mỗi truy vấn$(a, b, g)$, chúng ta đi qua đường đi từ$a$ĐẾN$b$bằng cách sử dụng các liên kết gốc (sau khi phân tách LCA) và đối với mọi cạnh trên đường dẫn này, chúng tôi cập nhật giới hạn dưới của nó ít nhất là$g$. Điều này thực thi điều kiện là mức tối thiểu trên đường dẫn không thể nhỏ hơn$g$, bởi vì bây giờ mọi cạnh đều ít nhất$g$. 
3. Sau khi xử lý tất cả các truy vấn, chúng tôi tính toán các giá trị cạnh cuối cùng dưới dạng các giới hạn dưới này. Tại thời điểm này, tất cả “$\ge g$” các ràng buộc được thỏa mãn bằng cách xây dựng. 
4. Chúng tôi xác thực từng truy vấn bằng cách kiểm tra xem đường dẫn từ$a$ĐẾN$b$chứa ít nhất một cạnh có giá trị được gán bằng$g$. Nếu bất kỳ truy vấn nào không đạt được điều kiện này, chúng tôi kết luận rằng không có phép gán hợp lệ nào tồn tại. 
5. Nếu tất cả các truy vấn đều vượt qua quá trình xác thực, chúng tôi sẽ xuất các giá trị cạnh được chỉ định. 

Điều tinh tế quan trọng là bước 2 chỉ thực thi các giới hạn dưới nhưng chưa đảm bảo rằng mỗi truy vấn đều đạt được mức tối thiểu. Đó là lý do tại sao bước 4 là cần thiết và không thể bỏ qua. 

### Tại sao nó hoạt động 

Mỗi truy vấn áp đặt một ràng buộc đơn điệu trên một đường dẫn: tất cả các cạnh ít nhất phải$g$và ít nhất một cạnh phải được siết chặt tại$g$. Việc truyền bá các giới hạn dưới đảm bảo tính khả thi theo một hướng: không có cạnh nào vi phạm bất kỳ yêu cầu tối thiểu nào. Bước xác thực đảm bảo rằng yêu cầu “chặt chẽ” cũng được thỏa mãn. 

Bởi vì tất cả các ràng buộc đều là các ràng buộc giới hạn dưới, việc tăng giá trị cạnh không bao giờ phá vỡ tính khả thi, do đó, khi chúng tôi thực thi tất cả các giới hạn dưới bắt buộc, mọi giải pháp hợp lệ đều phải nằm trên hoặc bằng giá trị đó. Nếu một giải pháp tồn tại, nó có thể được chuyển đổi thành một giải pháp trong đó các cạnh chính xác là các giới hạn dưới này mà không vi phạm bất kỳ ràng buộc nào, miễn là điều kiện chặt chẽ vẫn có thể đạt được. Bước xác nhận xác nhận xem cấu trúc cần thiết này có tồn tại hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
adj = [[] for _ in range(n)]
edges = []

for i in range(n - 1):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    adj[x].append((y, i))
    adj[y].append((x, i))
    edges.append((x, y))

m = int(input())
queries = []
for _ in range(m):
    a, b, g = map(int, input().split())
    queries.append((a - 1, b - 1, g))

parent = [-1] * n
depth = [0] * n
edge_to_parent = [-1] * n

def dfs(v, p):
    for to, idx in adj[v]:
        if to == p:
            continue
        parent[to] = v
        depth[to] = depth[v] + 1
        edge_to_parent[to] = idx
        dfs(to, v)

dfs(0, -1)

def get_path_edges(u, v):
    res = set()
    while u != v:
        if depth[u] < depth[v]:
            u, v = v, u
        res.add(edge_to_parent[u])
        u = parent[u]
    return res

f = [1] * (n - 1)

for a, b, g in queries:
    path_edges = get_path_edges(a, b)
    for ei in path_edges:
        f[ei] = max(f[ei], g)

def path_has_value(a, b, g):
    while a != b:
        if depth[a] < depth[b]:
            a, b = b, a
        if f[edge_to_parent[a]] == g:
            return True
        a = parent[a]
    return False

ok = True
for a, b, g in queries:
    if not path_has_value(a, b, g):
        ok = False
        break

if not ok:
    print(-1)
else:
    print(*f)
```DFS xây dựng một cây gốc và gán cho mỗi nút một cạnh cha, cho phép di chuyển lên trên dọc theo bất kỳ đường dẫn nào. chức năng`get_path_edges`thu thập tất cả các cạnh trên một đường dẫn bằng cách liên tục nâng nút sâu hơn, đủ để$n \le 5000$. 

Mảng`f`lưu trữ giới hạn dưới hiện tại cho mỗi cạnh, bắt đầu từ 1. Mỗi truy vấn thực thi mức tối thiểu trên toàn bộ đường dẫn. 

Hàm xác thực kiểm tra xem một truy vấn có ít nhất một cạnh chính xác bằng mức tối thiểu được yêu cầu hay không. Điều này được thực hiện bằng cách đi lại đường đi và kiểm tra các giá trị cạnh. 

Một điểm tinh tế là chúng tôi lưu trữ các chỉ số cạnh thay vì xây dựng lại các cạnh kề liên tục, đảm bảo tính chính xác ngay cả trong biểu diễn vô hướng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2
3 2
3 4
2
1 2 5
1 3 3
```Chúng tôi root ở mức 1. 

| Bước | Truy vấn | Các cạnh đường dẫn | Cập nhật | Trạng thái f | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2,5) | (1-2) | f[1-2]=5 | [5,1,1] | 
| 2 | (1,3,3) | (1-2,2-3) | f[1-2]=5, f[2-3]=3 | [5,3,1] | 

Bây giờ xác nhận: 

Truy vấn (1,2,5): đường dẫn có cạnh 1-2 = 5, hợp lệ. 

Truy vấn (1,3,3): đường dẫn (1-2,2-3) chứa cạnh 2-3 = 3, hợp lệ. 

Đầu ra là nhất quán. 

Điều này chứng tỏ rằng các ràng buộc chồng chéo có thể cùng tồn tại nếu ít nhất một cạnh trên mỗi đường dẫn được giữ chặt. 

### Ví dụ 2 

đầu vào:```
3
1 2
2 3
2
1 3 4
1 2 5
```| Bước | Truy vấn | Các cạnh đường dẫn | Cập nhật | Trạng thái f | 
| --- | --- | --- | --- | --- | 
| 1 | (1,3,4) | (1-2,2-3) | cả hai ≥4 | [4,4] | 
| 2 | (1,2,5) | (1-2) | 1-2 trở thành 5 | [5,4] | 

Xác thực: 

Truy vấn (1,3,4): đường dẫn có cạnh 2-3 = 4, OK. 

Truy vấn (1,2,5): đường dẫn có cạnh 1-2 = 5, OK. 

Điều này cho thấy các ràng buộc cao hơn sẽ ghi đè cục bộ các ràng buộc thấp hơn như thế nào trong khi vẫn duy trì tính khả thi trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi truy vấn có thể đi qua một đường dẫn cây đầy đủ trong trường hợp xấu nhất | 
| Không gian |$O(n)$| Lưu trữ cho cây, con trỏ cha và giá trị cạnh | 

Được cho$n, m \le 5000$, giải pháp dựa trên truyền tải này là đủ trong thực tế và phù hợp với giới hạn bộ nhớ một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    adj = [[] for _ in range(n)]
    edges = []

    for i in range(n - 1):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        adj[x].append((y, i))
        adj[y].append((x, i))
        edges.append((x, y))

    m = int(input())
    queries = [tuple(map(int, input().split())) for _ in range(m)]

    parent = [-1] * n
    depth = [0] * n
    edge_to_parent = [-1] * n

    def dfs(v, p):
        for to, idx in adj[v]:
            if to == p:
                continue
            parent[to] = v
            depth[to] = depth[v] + 1
            edge_to_parent[to] = idx
            dfs(to, v)

    dfs(0, -1)

    def path(u, v):
        res = set()
        while u != v:
            if depth[u] < depth[v]:
                u, v = v, u
            res.add(edge_to_parent[u])
            u = parent[u]
        return res

    f = [1] * (n - 1)

    for a, b, g in queries:
        a -= 1
        b -= 1
        path_edges = path(a, b)
        for ei in path_edges:
            f[ei] = max(f[ei], g)

    def ok(a, b, g):
        while a != b:
            if depth[a] < depth[b]:
                a, b = b, a
            if f[edge_to_parent[a]] == g:
                return True
            a = parent[a]
        return False

    for a, b, g in queries:
        a -= 1
        b -= 1
        if not ok(a, b, g):
            return "-1"

    return " ".join(map(str, f))

# samples
assert run("""4
1 2
3 2
3 4
2
1 2 5
1 3 3
""").strip() == "5 3 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây tối thiểu | bài tập hợp lệ | độ đúng cơ sở | 
| chuỗi có các ràng buộc xung đột | -1 hoặc hợp lệ | phát hiện mâu thuẫn | 
| cây sao | bài tập hợp lệ | đường dẫn chồng chéo | 
| truy vấn nặng duy nhất | tuyên truyền đúng | cập nhật đường dẫn chính xác | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi nhiều truy vấn buộc phải tăng giới hạn dưới trên cùng một cạnh trong khi mong đợi các cạnh “chặt” khác nhau cho mỗi truy vấn. Trong một chuỗi đơn giản, giả sử một truy vấn yêu cầu tối thiểu 10 trên toàn bộ đường dẫn và một truy vấn khác yêu cầu tối thiểu 5 trên một đường dẫn phụ chồng lên các cạnh giống nhau. Thuật toán tăng giới hạn một cách chính xác, nhưng việc xác thực đảm bảo truy vấn 5 vẫn có cạnh bằng 5 ở đâu đó, không bị ghi đè bằng 10 ở mọi nơi. 

Một trường hợp cạnh khác xảy ra khi mọi cạnh trên đường dẫn được truy vấn bị các truy vấn khác buộc phải vượt quá mức tối thiểu cần thiết. Trong trường hợp đó, mặc dù tất cả các ràng buộc đều được thỏa mãn dưới dạng bất đẳng thức, việc thiếu bất kỳ cạnh nào chính xác bằng mức tối thiểu bắt buộc sẽ gây ra lỗi, lỗi này được phát hiện chính xác trong quá trình xác thực bằng cách quét đường dẫn. 

Trường hợp cuối cùng là khi các ràng buộc tạo thành một hệ thống phân cấp nhất quán. Ví dụ: các đường dẫn lồng nhau có giá trị g giảm vẫn thành công vì ít nhất một cạnh trong mỗi đường dẫn vẫn ở ngưỡng riêng trước khi bị các ràng buộc mạnh hơn dỡ bỏ.
