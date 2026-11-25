---
title: "CF 999E - Khả năng tiếp cận từ thủ đô"
description: "Chúng ta được cung cấp một biểu đồ có hướng trong đó các thành phố là các nút và đường là các cạnh một chiều. Một thành phố được chỉ định làm thủ đô. Mục tiêu là để đảm bảo rằng mọi thành phố đều có thể đến được bằng cách đi theo những con đường được chỉ dẫn bắt đầu từ thủ đô."
date: "2026-06-16T23:52:51+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 999
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 490 (Div. 3)"
rating: 2000
weight: 999
solve_time_s: 75
verified: true
draft: false
---

[CF 999E - Khả năng tiếp cận từ thủ đô](https://codeforces.com/problemset/problem/999/E) 

**Xếp hạng:** 2000 
**Tags:** dfs và tương tự, đồ thị, tham lam 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng trong đó các thành phố là các nút và đường là các cạnh một chiều. Một thành phố được chỉ định làm thủ đô. Mục tiêu là để đảm bảo rằng mọi thành phố đều có thể đến được bằng cách đi theo những con đường được chỉ dẫn bắt đầu từ thủ đô. Nếu không thể truy cập được một thành phố, chúng tôi được phép thêm đường dẫn mới và chúng tôi muốn giảm thiểu số lượng đường chúng tôi thêm vào. 

Nhiệm vụ cốt lõi không phải là tính toán khả năng tiếp cận mà là xác định số cạnh có hướng mới tối thiểu phải được chèn để vốn cuối cùng có thể chạm tới mọi nút trong biểu đồ. 

Các ràng buộc đủ nhỏ cho các thuật toán đồ thị tuyến tính hoặc gần tuyến tính về số đỉnh và cạnh. Với cả n và m đều lên tới 5000, về nguyên tắc, giải pháp O(n²) hoặc O(nm) có thể được chấp nhận, nhưng bất kỳ khối nào hoặc liên tục tính toán lại khả năng tiếp cận từ đầu sẽ không mở rộng được. 

Một vấn đề nhỏ xuất hiện khi biểu đồ chứa các thành phần liên kết chặt chẽ. Bên trong một thành phần được kết nối mạnh mẽ, mọi nút đều có thể truy cập được lẫn nhau, do đó việc xử lý các nút riêng lẻ sẽ dẫn đến lý luận dư thừa. Sự trừu tượng chính xác là chỉ có cấu trúc giữa các thành phần được kết nối mạnh mẽ mới quan trọng. 

Một cách tiếp cận ngây thơ có thể thử mô phỏng việc thêm các cạnh một cách tham lam, liên tục chạy tìm kiếm khả năng tiếp cận và kết nối vốn với các nút mới. Điều này không thành công trong trường hợp nhiều vùng không thể truy cập phụ thuộc vào nhau một cách gián tiếp. Ví dụ: nếu biểu đồ có một số SCC được sắp xếp trong DAG, việc kết nối một nút trong một thành phần có thể mở khóa nhiều nút khác và các quyết định cục bộ tham lam có thể đếm quá hoặc thiếu mà không nhận ra cấu trúc này. 

## Phương pháp tiếp cận 

Một chiến lược trực tiếp nhưng không hiệu quả là chạy liên tục BFS hoặc DFS từ thủ đô, đánh dấu tất cả các nút có thể truy cập, sau đó chọn một nút không thể truy cập và thêm đường từ thủ đô đến nút đó, lặp lại cho đến khi mọi thứ đều có thể truy cập được. Mỗi phần bổ sung sẽ thay đổi khả năng tiếp cận nên chúng tôi tính toán lại từ đầu. Trong trường hợp xấu nhất, chúng ta có thể thêm các cạnh O(n) và mỗi lần tính toán khả năng tiếp cận sẽ tốn O(n + m), dẫn đến thời gian O(n(n + m)), quá chậm ở giới hạn trên. 

Thông tin chi tiết quan trọng là khả năng tiếp cận bên trong các thành phần được kết nối mạnh mẽ không quan trọng riêng lẻ. Khi chúng tôi thu gọn từng thành phần được kết nối mạnh vào một nút duy nhất, biểu đồ sẽ trở thành biểu đồ chu kỳ có hướng (DAG). Trong DAG, khả năng tiếp cận từ vốn chỉ phụ thuộc vào thành phần nào có thể tiếp cận được từ thành phần vốn. 

Bây giờ vấn đề trở thành: trong DAG này, thành phần nào không thể truy cập được từ thành phần vốn và chúng ta cần thêm bao nhiêu cạnh để tất cả các thành phần đều có thể truy cập được từ nó? Mỗi cạnh được thêm vào có thể bắt đầu trong bất kỳ thành phần nào có thể truy cập được và đi đến bất kỳ thành phần nào không thể truy cập được, vì vậy, chúng tôi muốn kết nối các phần không thể truy cập của DAG một cách hiệu quả. 

Một đặc tính nổi tiếng của DAG sẽ giúp ích ở đây. Nếu chúng ta xem xét biểu đồ ngưng tụ, mọi vùng được kết nối yếu không thể truy cập được từ nguồn cần có ít nhất một cạnh đến từ phía có thể truy cập được. Số cạnh tối thiểu cần thiết bằng số lượng thành phần nguồn trong sơ đồ con của các thành phần không thể truy cập, khi xem xét dưới góc độ kết nối từ vùng có thể truy cập. 

Trên thực tế, chúng tôi giải quyết vấn đề này bằng cách tính toán các thành phần được kết nối mạnh mẽ, xây dựng DAG cô đọng, đánh dấu thành phần nào có thể truy cập được từ vốn và sau đó đếm xem có bao nhiêu thành phần không thể truy cập không có cạnh đến từ một thành phần không thể truy cập khác. Mỗi thành phần như vậy phải nhận được ít nhất một cạnh mới. 

Điều này làm giảm vấn đề phân rã SCC cộng với quét tuyến tính trên biểu đồ thu gọn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| DFS/BFS lặp lại có bổ sung cạnh | O(n(n + m)) | O(n + m) | Quá chậm | 
| SCC + phân tích biểu đồ ngưng tụ | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén biểu đồ thành các thành phần được kết nối mạnh bằng thuật toán của Kosaraju hoặc thuật toán của Tarjan. Bước này nhóm các nút có thể truy cập được lẫn nhau thành các đơn vị đơn lẻ, vì trong một nhóm như vậy, khả năng tiếp cận đã được đảm bảo và không yêu cầu bất kỳ cạnh mới nào. 

Tiếp theo, chúng tôi xây dựng biểu đồ ngưng tụ trong đó mỗi SCC trở thành một nút và các cạnh biểu thị các kết nối giữa các thành phần. Biểu đồ này được đảm bảo là DAG vì các chu kỳ đã được thu gọn lại. 

Sau đó chúng tôi xác định SCC có chứa vốn. Từ thành phần này, chúng tôi thực hiện DFS hoặc BFS trên biểu đồ ngưng tụ để đánh dấu tất cả các thành phần có thể truy cập được. 

Sau đó, chúng tôi chỉ tập trung vào các thành phần không được đánh dấu là có thể truy cập được. Trong số các thành phần không thể tiếp cận này, chúng tôi tính toán thành phần nào không có cạnh đến từ bất kỳ thành phần có thể tiếp cận nào. Mỗi thành phần như vậy đại diện cho một khu vực không thể tiếp cận gián tiếp và do đó cần phải kết nối ít nhất một cạnh mới. 

Cuối cùng, chúng tôi đếm các thành phần này và đưa ra kết quả. 

### Tại sao nó hoạt động 

Sau khi thu gọn thành SCC, biểu đồ sẽ trở thành DAG và khả năng tiếp cận hoạt động giống như truyền tải theo thứ tự một phần. Bất kỳ nút nào trong SCC có thể truy cập đều có thể tiếp cận tất cả các nút trong thành phần của nó và tất cả các SCC ở hạ lưu. Nếu không thể truy cập được SCC, nó phải nằm trong vùng DAG khác và không có đường dẫn từ thành phần thủ đô. 

Nếu một SCC không thể truy cập được có cạnh đến từ một SCC không thể truy cập khác thì nó có thể đạt được sau khi SCC ngược dòng đó được kết nối. Do đó, chỉ những SCC không có cạnh đến từ bên trong sơ đồ con không thể truy cập mới là “điểm vào” độc lập yêu cầu kết nối rõ ràng. Mỗi SCC như vậy cần ít nhất một cạnh mới và một cạnh là đủ vì nó có thể kết nối từ bất kỳ SCC nào có thể truy cập được. Điều này thiết lập cả giới hạn dưới và tính tối ưu của kết cấu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def kosaraju(n, g, rg):
    visited = [False] * n
    order = []

    def dfs1(v):
        visited[v] = True
        for to in g[v]:
            if not visited[to]:
                dfs1(to)
        order.append(v)

    for i in range(n):
        if not visited[i]:
            dfs1(i)

    comp = [-1] * n
    cid = 0

    def dfs2(v, c):
        comp[v] = c
        for to in rg[v]:
            if comp[to] == -1:
                dfs2(to, c)

    for v in reversed(order):
        if comp[v] == -1:
            dfs2(v, cid)
            cid += 1

    return comp, cid

def solve():
    n, m, s = map(int, input().split())
    s -= 1

    g = [[] for _ in range(n)]
    rg = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        rg[v].append(u)

    comp, k = kosaraju(n, g, rg)

    cg = [[] for _ in range(k)]
    indeg = [0] * k

    for u in range(n):
        for v in g[u]:
            if comp[u] != comp[v]:
                cg[comp[u]].append(comp[v])

    cap = comp[s]

    reachable = [False] * k
    stack = [cap]
    reachable[cap] = True

    while stack:
        v = stack.pop()
        for to in cg[v]:
            if not reachable[to]:
                reachable[to] = True
                stack.append(to)

    indeg_unreach = [0] * k

    for u in range(n):
        cu = comp[u]
        if reachable[cu]:
            continue
        for v in g[u]:
            cv = comp[v]
            if not reachable[cv]:
                indeg_unreach[cv] += 1

    ans = 0
    for c in range(k):
        if not reachable[c] and indeg_unreach[c] == 0:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng cả biểu đồ gốc và biểu đồ đảo ngược để phân tách SCC. Sau khi tính toán các nhãn thành phần, nó sẽ ngầm xây dựng biểu đồ ngưng tụ và khám phá khả năng tiếp cận từ thành phần vốn. 

Chi tiết triển khai quan trọng là tách các SCC có thể truy cập và không thể truy cập trước khi đếm các cạnh đến. Điều này ngăn chặn việc đếm quá mức các cạnh bắt nguồn từ các vùng đã có thể tiếp cận, điều này không quan trọng đối với việc kết nối biểu đồ còn lại. 

Bước đếm cuối cùng chỉ xem xét các SCC không thể truy cập được và không có cạnh nào đến từ các SCC không thể truy cập khác, tương ứng chính xác với các thành phần độc lập phải được kết nối trực tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 9 1
1 2
1 3
2 3
1 5
5 6
6 1
1 8
9 8
7 1
```Sau khi phân tách SCC, giả sử mỗi nút là thành phần riêng của nó ngoại trừ các chu kỳ như (1,5,6). Chúng tôi theo dõi khả năng tiếp cận từ thành phần của 1. 

| Bước | Hành động | SCC có thể tiếp cận | 
| --- | --- | --- | 
| 1 | Bắt đầu từ SCC(1) | {1} | 
| 2 | Các cạnh ngưng tụ ngang | {1,2,3,5,6,8} | 
| 3 | Đánh dấu các SCC không thể truy cập | {4,7,9} | 
| 4 | Đếm số SCC không có dữ liệu đến từ không thể truy cập | 3 | 

Kết quả là 3, phù hợp với nhu cầu kết nối ba vùng độc lập không thể truy cập được. 

Điều này xác nhận rằng chỉ có các nguồn SCC độc lập không thể truy cập mới quan trọng chứ không phải tất cả các nút. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
5 2 5
5 4
4 3
```Ở đây nút 5 có thể chạm tới nút 4 và 3, nhưng nút 1 và 2 bị cô lập. 

| Bước | Hành động | SCC có thể tiếp cận | 
| --- | --- | --- | 
| 1 | Bắt đầu từ SCC(5) | {5,4,3} | 
| 2 | SCC còn lại | {1,2} | 
| 3 | Đếm các SCC độc lập không thể truy cập | 2 | 

Cần có hai cạnh, một cạnh đạt SCC(1), một cạnh đạt SCC(2). 

Điều này cho thấy rằng mỗi thành phần bị ngắt kết nối đều yêu cầu ít nhất một kết nối đến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Tính toán SCC và truyền tải đồ thị mỗi lần chạy theo thời gian tuyến tính trên các đỉnh và cạnh | 
| Không gian | O(n + m) | danh sách kề, đồ thị ngược, mảng thành phần và cấu trúc phụ trợ | 

Các ràng buộc cho phép tối đa 5000 nút và cạnh, đồng thời cách tiếp cận SCC theo thời gian tuyến tính nằm trong giới hạn. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import ModuleType

    # assumes solution is defined in same file context
    return _sys.stdout.getvalue() if False else ""

# Since full harness integration isn't possible in this static format,
# below are logical assert-style cases for reference.

# sample 1
# assert run("""9 9 1
# 1 2
# 1 3
# 2 3
# 1 5
# 5 6
# 6 1
# 1 8
# 9 8
# 7 1
# """) == "3"

# minimal case
# assert run("""1 0 1
# """) == "0"

# disconnected nodes
# assert run("""3 0 1
# """) == "2"

# already strongly connected
# assert run("""3 3 1
# 1 2
# 2 3
# 3 1
# """) == "0"

# reversed chain
# assert run("""4 3 4
# 4 3
# 3 2
# 2 1
# """) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp cơ bản tầm thường | 
| không có cạnh | n-1 | trường hợp mất kết nối tồi tệ nhất | 
| đồ thị chu trình | 0 | Hành vi sụp đổ SCC | 
| con đường đảo ngược | 0 | khả năng tiếp cận đầy đủ từ đuôi | 

## Vỏ cạnh 

Trường hợp một cạnh là khi vốn nằm trong thành phần chìm của đồ thị ngưng tụ. Trong tình huống này, không có cạnh đi nào tồn tại từ SCC của nó, do đó chỉ các nút bên trong SCC của nó mới có thể truy cập được. Thuật toán chỉ đánh dấu thành phần đó là có thể truy cập được và mọi SCC khác không có khả năng truy cập đến sẽ được tính. Điều này mang lại chính xác một kết nối cần thiết cho mỗi vùng không thể truy cập độc lập. 

Một trường hợp khác là khi đồ thị đã được liên thông mạnh. Sau khi nén SCC, chỉ còn một thành phần. Bước về khả năng tiếp cận đánh dấu nó là có thể truy cập được ngay lập tức và số lượng cuối cùng không tìm thấy thành phần nào không thể truy cập được, trả về 0 mà không thực hiện bất kỳ thao tác đếm không cần thiết nào. 

Trường hợp thứ ba là khi có nhiều SCC nhỏ bị ngắt kết nối và không có cạnh nào cả. Mỗi SCC không có cạnh nào đến từ các thành phần có thể tiếp cận được, vì vậy mỗi cạnh được tính một lần. Thuật toán diễn giải chính xác từng yêu cầu một kết nối trực tiếp từ vùng thủ đô.
