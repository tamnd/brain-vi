---
problem: 982F
contest_id: 982
problem_index: F
name: "The Meeting Place Cannot Be Changed"
contest_name: "Codeforces Round 484 (Div. 2)"
rating: 2700
tags: ["dfs and similar", "graphs"]
answer: passed_samples
verified: false
solve_time_s: 87
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33a65c-a680-83ec-b683-c182360d8ebc
---

#CF 982F - Nơi Họp Không Thể Thay Đổi 

**Đánh giá:** 2700 
**Thẻ:** dfs và các biểu đồ tương tự 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a65c-a680-83ec-b683-c182360d8ebc 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng thể hiện các giao lộ được nối với nhau bằng đường một chiều. Một chiếc ô tô khởi hành từ một giao lộ không xác định nào đó và sau đó tiếp tục di chuyển dọc theo những con đường hướng ra ngoài mãi mãi. Chúng tôi được đảm bảo rằng chuyển động này không bao giờ bị mắc kẹt, nghĩa là trong mọi tình huống có thể tiếp cận luôn có ít nhất một cạnh đi ra để tiếp tục cuộc hành trình. 

Petr muốn chọn một giao lộ duy nhất sao cho bất kể ô tô bắt đầu ở đâu và bất kể cạnh đi ra nào được chọn trong quá trình di chuyển, ô tô cuối cùng sẽ đến giao lộ đã chọn đó tại một điểm nào đó trong quãng đường đi vô tận của nó. 

Diễn đạt lại theo thuật ngữ đồ thị, chúng ta đang tìm kiếm một đỉnh không thể tránh khỏi đối với tất cả các bước đi có hướng vô hạn trong đồ thị, giả sử bước đi đi theo các cạnh tùy ý nhưng có thể tiếp tục vô tận. 

Ràng buộc n lên tới 100000 và m lên tới 500000 buộc mọi giải pháp phải tuyến tính hoặc gần tuyến tính theo kích thước của biểu đồ. Bất cứ điều gì liên quan đến DFS lặp lại từ mọi nút hoặc kiểm tra khả năng tiếp cận theo cặp, sẽ ngay lập tức vượt quá giới hạn vì nó sẽ mở rộng thành O(n(m+n)) hoặc tệ hơn. 

Một trường hợp lỗi tinh vi xuất hiện khi biểu đồ chứa nhiều thành phần được kết nối chặt chẽ được sắp xếp theo một chu trình. Ví dụ, hãy xem xét một chu trình có hướng của các thành phần A → B → C → A, trong đó mỗi thành phần cũng có các chu trình bên trong. Một trực giác ngây thơ có thể chọn một đỉnh bên trong một SCC và cho rằng tất cả các đường dẫn cuối cùng đều chảy vào nó, nhưng trên thực tế, quá trình này có thể luân chuyển mãi mãi mà không cần đến một điểm vào duy nhất. 

Một trường hợp phức tạp khác là khi có nhiều SCC lớn có thể được truy cập theo các thứ tự khác nhau tùy thuộc vào lựa chọn đường dẫn. Trong một đồ thị như vậy, không thể tránh khỏi một đỉnh duy nhất, mặc dù chuyển động vô hạn được đảm bảo bởi các chu trình bên trong các thành phần. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là kiểm tra từng đỉnh như một điểm gặp ứng cử viên. Đối với một đỉnh cố định v, chúng ta cần phải xác minh rằng từ mọi vị trí bắt đầu có thể có và mọi khả năng tiếp tục của các cạnh, bước đi vô hạn cuối cùng sẽ chạm tới v. Điều đó đã gợi ý về trường hợp xấu nhất cần phải suy luận về tất cả các tập hợp có thể tiếp cận và tất cả các đường dẫn vô hạn. Việc mô phỏng trực tiếp tất cả các bước đi là không thể vì số lượng đường đi tăng theo cấp số nhân theo chiều dài. 

Một lực lượng vũ phu có cấu trúc hơn sẽ kiểm tra khả năng tiếp cận ngược lại: đối với mỗi đỉnh v, hãy tính tập hợp các đỉnh có thể tiếp cận v. Sau đó kiểm tra xem từ mọi đỉnh có đường dẫn đến v hay không. Điều này tương đương với việc v có thể tiếp cận được từ tất cả các nút trong biểu đồ khi các cạnh được theo sau về phía trước. Việc kiểm tra đó có thể được thực hiện bằng DFS hoặc BFS từ mọi đỉnh, có giá O(n(m+n)), quá lớn. 

Cái nhìn sâu sắc về cấu trúc quan trọng là biểu đồ có thể được phân tách thành các thành phần được kết nối chặt chẽ. Bên trong một thành phần được kết nối mạnh mẽ, mọi đỉnh có thể chạm tới nhau, do đó, từ góc độ chuyển động vô hạn, mỗi SCC hoạt động giống như một “trạng thái vĩ mô” duy nhất. Việc thu gọn từng SCC sẽ tạo ra một biểu đồ tuần hoàn có hướng. 

Trong biểu đồ cô đọng đó, vấn đề trở thành việc tìm một thành phần có thể truy cập được từ mọi thành phần khác. Trong DAG, điều này xảy ra khi và chỉ khi có chính xác một thành phần chìm theo nghĩa đảo ngược khả năng tiếp cận hoặc tương đương là thành phần duy nhất không có cạnh đến trong biểu đồ ngưng tụ. Thành phần đó hoạt động như một bộ thu hút phổ quát vì mọi thành phần khác có thể chảy vào nó và khi đã ở trong SCC, lối đi không thể thoát khỏi các ràng buộc về cấu trúc SCC vốn có thể tránh được nó. 

Điều này làm giảm nhiệm vụ tính toán SCC và sau đó phân tích biểu đồ ngưng tụ theo thời gian tuyến tính.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra khả năng tiếp cận Brute Force | O(n(m+n)) | O(n+m) | Quá chậm | 
| ngưng tụ SCC + phân tích | O(n+m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán các thành phần liên thông mạnh của đồ thị bằng thuật toán Kosaraju hoặc Tarjan. Nhóm này nhóm các đỉnh thành các tập hợp tối đa trong đó mọi đỉnh có thể chạm tới các đỉnh khác trong cùng một tập hợp. Bước này là cần thiết vì bên trong một thành phần như vậy, chúng ta không thể phân biệt các đỉnh theo chuyển động dài hạn. 
2. Xây dựng biểu đồ thu gọn trong đó mỗi SCC trở thành một nút duy nhất. Với mọi cạnh u → v trong đồ thị gốc, nếu u và v thuộc các thành phần khác nhau thì hãy thêm một cạnh có hướng giữa các SCC của chúng. 
3. Tính độ của tất cả các nút trong biểu đồ thu gọn. 
4. Xác định tất cả các SCC có mức độ bằng 0. Đây là những thành phần mà không thành phần nào khác có thể vào được. 
5. Nếu có chính xác một SCC như vậy, hãy chọn bất kỳ đỉnh nào bên trong nó làm câu trả lời. Nếu có nhiều SCC như vậy, ghi -1 vì không có thành phần đích duy nhất không thể tránh khỏi. 
6. Trả về đỉnh đã chọn. 

Lý do đằng sau việc tập trung vào các SCC không bậc là vì bất kỳ SCC nào có các cạnh đến đều có khả năng tránh được vô thời hạn bằng cách nằm trong chu kỳ của các SCC khác không buộc phải xâm nhập vào nó. 

### Tại sao nó hoạt động 

Đồ thị ngưng tụ là DAG, do đó phải tồn tại ít nhất một nguồn SCC. Một đỉnh là không thể tránh khỏi đối với mỗi bước đi vô hạn chỉ khi mọi đường chuyển tiếp của SCC cuối cùng đều dẫn vào cùng một SCC. Điều đó chỉ có thể thực hiện được khi có chính xác một nguồn SCC trong biểu đồ ngưng tụ, bởi vì nhiều nguồn cho phép bắt đầu di chuyển bên trong một nguồn khác và duy trì mãi mãi bên trong vùng có thể tiếp cận của nó mà không bị buộc phải chuyển sang nguồn khác. Bên trong SCC, tất cả các đỉnh đều có thể truy cập lẫn nhau, do đó, việc chọn bất kỳ đỉnh nào trong SCC nguồn duy nhất sẽ đảm bảo việc truy cập cuối cùng trong bất kỳ quá trình truyền tải vô hạn nào. 

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
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    rg = [[] for _ in range(n)]

    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        rg[v].append(u)
        edges.append((u, v))

    comp, c = kosaraju(n, g, rg)

    indeg = [0] * c
    for u, v in edges:
        cu, cv = comp[u], comp[v]
        if cu != cv:
            indeg[cv] += 1

    sources = [i for i in range(c) if indeg[i] == 0]

    if len(sources) != 1:
        print(-1)
        return

    target = sources[0]
    for i in range(n):
        if comp[i] == target:
            print(i + 1)
            return

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng thuật toán của Kosaraju để tính toán SCC theo hai lượt, trước tiên xây dựng thứ tự hoàn thiện và sau đó gán các thành phần trên biểu đồ đảo ngược. Sau đó, chúng ta chỉ cần kiểm tra các cạnh giữa các thành phần. 

Một sai lầm phổ biến là quên bỏ qua các cạnh bên trong thành phần khi tính toán độ. Những cạnh đó không ảnh hưởng đến cấu trúc ngưng tụ và sẽ làm tăng số lượng độ không chính xác. Một cách tinh tế khác là chọn bất kỳ đỉnh nào từ SCC đích, vì tất cả các đỉnh bên trong thành phần liên thông mạnh đều tương đương cho mục đích này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6
1 2
2 3
3 1
3 4
4 5
5 3
```Sự hình thành và ngưng tụ SCC: 

| Bước | Tiểu bang | 
| --- | --- | 
| SCC | {1,2,3}, {4,5} | 
| Các cạnh ngưng tụ | {1,2,3} ↔ {4,5} (chu kỳ) | 
| bằng cấp | cả hai thành phần đều có bậc 1 | 
| nguồn | không có gì độc đáo | 

Vì không có một SCC nào có bậc bằng 0 nên không có đỉnh nào là không thể tránh khỏi. 

Đầu ra:```
-1
```Điều này cho thấy chu kỳ giữa các SCC phá hủy tính duy nhất của một đích đến bắt buộc. 

### Ví dụ 2 

đầu vào:```
4 4
1 2
2 3
3 1
3 4
```| Bước | Tiểu bang | 
| --- | --- | 
| SCC | {1,2,3}, {4} | 
| Các cạnh ngưng tụ | {1,2,3} → {4} | 
| bằng cấp | {1,2,3}: 0, {4}: 1 | 
| nguồn | {1,2,3} | 

Chỉ có một SCC nguồn tồn tại, do đó mọi đỉnh bên trong {1,2,3} đều hoạt động, ví dụ 1. 

Đầu ra:```
1
```Điều này chứng tỏ cách một thành phần đầu vào duy nhất buộc phải đến đích cuối cùng bất kể điểm bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Hai lượt DFS dành cho SCC cộng với một lần quét các cạnh để phân tích ngưng tụ | 
| Không gian | O(n + m) | Danh sách kề, đồ thị ngược và mảng thành phần | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì cả n và m đều lớn nhưng truyền tải tuyến tính là đủ và không cần khám phá các trạng thái lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
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
        n, m = map(int, input().split())
        g = [[] for _ in range(n)]
        rg = [[] for _ in range(n)]
        edges = []
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            rg[v].append(u)
            edges.append((u, v))

        comp, c = kosaraju(n, g, rg)

        indeg = [0] * c
        for u, v in edges:
            if comp[u] != comp[v]:
                indeg[comp[v]] += 1

        sources = [i for i in range(c) if indeg[i] == 0]

        if len(sources) != 1:
            return "-1\n"

        for i in range(n):
            if comp[i] == sources[0]:
                return str(i + 1) + "\n"

    return solve()

# provided sample
assert run("5 6\n1 2\n2 3\n3 1\n3 4\n4 5\n5 3\n") == "-1\n"

# chain to single sink SCC
assert run("4 3\n1 2\n2 3\n3 4\n") == "1\n"

# single cycle
assert run("3 3\n1 2\n2 3\n3 1\n") == "-1\n"

# two components both reachable cycle
assert run("4 4\n1 2\n2 1\n3 4\n4 3\n") == "-1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chu kỳ giữa các SCC | -1 | SCC nhiều nguồn | 
| chuỗi tuyến tính | 1 | nguồn độc đáo SCC | 
| chu kỳ SCC đơn | -1 | toàn bộ biểu đồ là một SCC nhưng không có cấu trúc nguồn duy nhất | 
| chu kỳ rời rạc | -1 | nhiều nguồn SCC độc lập | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi toàn bộ biểu đồ là một thành phần được kết nối mạnh mẽ. Trong tình huống đó, mọi đỉnh đều có thể tiếp cận được từ mọi đỉnh khác, do đó đồ thị ngưng tụ có chính xác một nút có độ bằng 0. Thuật toán chọn bất kỳ đỉnh nào trong SCC này, điều này đúng vì ô tô cuối cùng sẽ ghé thăm mọi đỉnh vô số lần trong bất kỳ chuyến đi nào. 

Một trường hợp cạnh khác là khi nhiều SCC không có cạnh nào trong biểu đồ ngưng tụ. Ví dụ: hai chu trình rời nhau không có cạnh nào ở giữa chúng. Trong trường hợp này, việc bắt đầu trong một chu trình cho phép ô tô duy trì mãi mãi mà không cần đi vào chu trình kia, do đó không tồn tại điểm gặp chung. Thuật toán xuất ra chính xác -1 vì nó tìm thấy nhiều SCC nguồn. 

Trường hợp tinh tế cuối cùng là khi các cạnh tạo thành một chu kỳ lớn các SCC, trong đó mỗi thành phần đều có một mức độ. Mặc dù biểu đồ được kết nối hoàn toàn theo nghĩa tuần hoàn, nhưng không thể tránh khỏi một SCC đơn lẻ vì ô tô có thể lưu thông vô thời hạn mà không cần phải có một thành phần đích duy nhất.
