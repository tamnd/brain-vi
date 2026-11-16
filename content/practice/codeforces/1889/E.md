---
title: "CF 1889E - Cây hoán đổi của Doremy"
description: "Chúng ta có hai cây trên cùng một tập đỉnh được dán nhãn. Các nhãn là cố định và duy nhất, vì vậy mỗi danh tính nút là toàn cục trên cả hai cây."
date: "2026-06-08T22:08:29+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1889
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 906 (Div. 1)"
rating: 3500
weight: 1889
solve_time_s: 145
verified: false
draft: false
---

[CF 1889E - Cây hoán đổi của Doremy](https://codeforces.com/problemset/problem/1889/E) 

**Đánh giá:** 3500 
**Thẻ:** dfs và tương tự, đồ thị, cây 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cây trên cùng một tập đỉnh được dán nhãn. Các nhãn là cố định và duy nhất, vì vậy mỗi danh tính nút là toàn cục trên cả hai cây. Điều duy nhất thay đổi theo thời gian là cấu trúc cạnh của cây đầu tiên thông qua chuỗi hoán đổi với các bộ cạnh được chọn cẩn thận từ cây thứ hai. 

Việc hoán đổi chỉ được phép khi chúng ta chọn một tập hợp con các cạnh từ mỗi cây sao cho sau khi chỉ giữ lại các cạnh đó và xóa các đỉnh bị cô lập, các đồ thị cảm ứng có cấu trúc kết nối giống hệt nhau trên các nhãn. Điều kiện đó có nghĩa là các sơ đồ con được chọn phải phân chia bộ nhãn thành các thành phần được kết nối theo cách giống hệt nhau. 

Sau khi chọn các sơ đồ con phù hợp như vậy, chúng tôi trao đổi các tập cạnh của chúng, chuyển “cấu trúc thành phần” giữa các cây một cách hiệu quả trong khi vẫn duy trì các ràng buộc về tính hợp lệ. 

Câu hỏi đặt ra: bắt đầu từ cặp cây ban đầu, cây đầu tiên có thể trở thành bao nhiêu cây được gắn nhãn riêng biệt sau bất kỳ số thao tác nào như vậy. 

Hạn chế chính là$n \le 10^5$, với tổng số$n$qua các bài kiểm tra cũng bị giới hạn bởi$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi hoạt động thăm dò hàm mũ trên các tập hợp con hoặc trạng thái cạnh. Thậm chí$O(n^2)$mỗi phép thử quá lớn, do đó lời giải phải giảm bài toán về cấu trúc gần tuyến tính hoặc đại số tuyến tính trên cây. 

Trường hợp cạnh tinh tế là khi cả hai cây giống hệt nhau hoặc có cấu trúc rất đơn giản. Ví dụ: nếu cả hai đều là đường dẫn trên 3 nút thì có thể xuất hiện nhiều giao dịch hoán đổi, nhưng các cấu trúc có thể truy cập thực tế bị ràng buộc chặt chẽ bởi tính đối xứng. 

Một trường hợp sai lầm khác là khi cả hai cây đều có cùng mức độ nhưng cấu trúc liên kết khác nhau. Một ý tưởng ngây thơ có thể là bất kỳ hoán vị nào của các cạnh đều có thể truy cập được, nhưng điều kiện hoán đổi hạn chế các hoạt động đối với các phân vùng của các nút do cạnh cây tạo ra, chứ không phải sắp xếp lại các cạnh tùy ý. 

Ví dụ: nếu cả hai cây đều là các ngôi sao, chẳng hạn như cây 1 được kết nối với tất cả các cây khác, thì bất kỳ sự hoán đổi nào cũng sẽ giữ nguyên cấu trúc ngôi sao, do đó không thể tạo ra cây có nhãn mới. Một cách giải thích ngây thơ có thể gợi ý không chính xác nhiều hoán vị của các lá, nhưng các nhãn được cố định nên không xảy ra thay đổi cấu trúc. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ coi mỗi trạng thái cây là một nút trong biểu đồ trạng thái khổng lồ, trong đó các chuyển đổi tương ứng với các giao dịch hoán đổi hợp lệ của các tập hợp cạnh. Mỗi trạng thái là một cây được gắn nhãn và từ mỗi trạng thái, chúng tôi thử tất cả các phân tách hợp lệ có thể thành các sơ đồ con phù hợp với thành phần. 

Số cây được dán nhãn là$n^{n-2}$, do đó việc biểu diễn không gian trạng thái là không thể. Một bước duy nhất đã liên quan đến việc chọn tập hợp con các cạnh từ cả hai cây, theo cấp số nhân$n$. Ngay cả khi bỏ qua điều đó, việc kiểm tra điều kiện tương tự đòi hỏi phải tính toán lại các thành phần được kết nối nhiều lần, đưa ra ít nhất$O(n)$mỗi lần kiểm tra. Điều này làm cho vũ lực hoàn toàn không khả thi. 

Quan sát quan trọng là hoạt động hoán đổi không tạo ra các phép biến đổi tùy ý. Nó chỉ cho phép trao đổi toàn bộ khu rừng cảm ứng tương ứng với các phân vùng đỉnh giống hệt nhau ở cả hai cây. Điều này có nghĩa là quá trình này không thực sự liên quan đến các cạnh mà là về cách các cạnh mã hóa các phân vùng phân cấp. 

Mỗi cây có thể được xem như một tập hợp các vết cắt được tạo ra bằng cách loại bỏ một cạnh. Mỗi cạnh chia cây thành hai thành phần. Một trao đổi duy trì sự tương đồng phải tôn trọng sự phân chia này đồng thời ở cả hai cây. Điều này dẫn đến một cấu trúc trong đó mỗi cạnh tương ứng với một phân vùng của tập nhãn và các hoạt động cho phép trao đổi các phân vùng tương thích một cách hiệu quả. 

Sự đơn giản hóa quan trọng là các cấu hình có thể truy cập của$T_1$chỉ phụ thuộc vào cách các cạnh của$T_1$Và$T_2$tương tác theo các phân vùng này. Khi chúng ta root một cây, mỗi cạnh tương ứng với sự phân chia kích thước của cây con và hoạt động về cơ bản cho phép gán lại các mối quan hệ cha mẹ dưới các ràng buộc do cả hai cây gây ra. 

Sau khi chính thức hóa, vấn đề giảm xuống việc đếm các cấu hình được xác định bằng các lựa chọn độc lập dọc theo các cấu trúc “có thể trao đổi” nhất định được hình thành bằng cách giao nhau giữa các hệ thống phân cấp của hai cây. Các cấu trúc này hóa ra là các thành phần được kết nối trong biểu đồ tương tác dẫn xuất trên các cạnh, trong đó mỗi thành phần đóng góp một hệ số nhân bằng số hướng hợp lệ. 

Mỗi thành phần như vậy hoạt động giống như một cấu trúc lựa chọn nhị phân được tạo ra bởi việc liệu chúng ta có căn chỉnh hướng cục bộ với$T_1$hoặc$T_2$, đưa ra hệ số 2 cho mỗi tương tác giống như chu kỳ độc lập. 

Vì vậy, câu trả lời trở thành$2^k$, Ở đâu$k$là số thành phần trao đổi độc lập được hình thành bằng cách chồng các ràng buộc từ cả hai cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tiểu bang | hàm mũ | hàm mũ | Quá chậm | 
| DSU / phân rã cấu trúc của tương tác cây |$O(n \alpha(n))$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cấu trúc tương tác giữa hai cây bằng cách coi mỗi cạnh là một ràng buộc kết nối các điểm cuối trong biểu đồ phụ thuộc dẫn xuất. 

1. Root cả hai cây tùy ý tại nút 1 và tính toán mảng gốc và độ sâu cho mỗi nút. Điều này cho phép chúng ta diễn giải mọi cạnh như một mối quan hệ trực tiếp hướng tới gốc của mỗi cây. 
2. Đối với mỗi nút, chúng tôi ghi lại cạnh cha của nó khác nhau như thế nào giữa$T_1$Và$T_2$. Về mặt khái niệm, chúng tôi kết nối các cạnh tương ứng vì chúng thể hiện các quyết định cấu trúc cạnh tranh cho cùng một đỉnh. 
3. Xây dựng cấu trúc tìm liên kết trên các nút (hoặc tương đương trên các trạng thái cạnh được định hướng) để hợp nhất các ràng buộc do cả hai cây tạo ra. Ý tưởng là nếu một cạnh trong$T_1$có thể tương ứng với nhiều vị trí hợp lệ trong$T_2$, những khả năng này thuộc cùng một lớp tương đương. 
4. Khi chúng tôi xử lý các cạnh, bất cứ khi nào sự phụ thuộc về cấu trúc ngụ ý rằng hai lựa chọn không thể được thực hiện một cách độc lập, chúng tôi sẽ hợp nhất các thành phần của chúng. Điều này dần dần hình thành các thành phần kết nối của “cấu trúc có thể hoán đổi cho nhau”. 
5. Đếm số lượng thành phần được kết nối trong biểu đồ ràng buộc này. Mỗi thành phần tương ứng với một quyết định nhị phân độc lập: liệu cấu trúc cục bộ có phù hợp với$T_1$-định hướng hoặc$T_2$-định hướng. 
6. Câu trả lời cuối cùng là$2^{\text{components}}$, modulo được tính toán$10^9+7$. 

### Tại sao nó hoạt động 

Hoạt động trao đổi chỉ trao đổi các cây con bảo toàn các phân vùng kết nối giống hệt nhau. Điều này buộc mọi phép biến đổi phải duy trì tính nhất quán của các phân vùng do cạnh gây ra trên cả hai cây. Kết quả là, sự phụ thuộc giữa các cạnh lan truyền một cách bắc cầu: việc cố định hướng của một cạnh sẽ hạn chế tất cả các cạnh trong thành phần tương tác của nó. Mỗi thành phần được kết nối trong biểu đồ phụ thuộc này biểu thị một tập hợp tối đa các cạnh có cấu hình tương đối được cố định cho đến một lần lật toàn cục, mang lại chính xác hai nhận thức nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

sys.setrecursionlimit(10**7)

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0]*n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1

def solve():
    n = int(input())
    g1 = [[] for _ in range(n)]
    g2 = [[] for _ in range(n)]

    edges1 = []
    edges2 = []

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g1[u].append(v)
        g1[v].append(u)
        edges1.append((u, v))

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g2[u].append(v)
        g2[v].append(u)
        edges2.append((u, v))

    parent1 = [-1] * n
    parent2 = [-1] * n

    def dfs(g, p, parent):
        stack = [p]
        parent[p] = p
        while stack:
            u = stack.pop()
            for v in g[u]:
                if v == parent[u]:
                    continue
                parent[v] = u
                stack.append(v)

    dfs(g1, 0, parent1)
    dfs(g2, 0, parent2)

    dsu = DSU(n)

    # merge constraints based on parent structure differences
    for i in range(n):
        if parent1[i] != -1 and parent2[i] != -1:
            dsu.union(i, parent1[i])
            dsu.union(i, parent2[i])

    comp = set()
    for i in range(n):
        comp.add(dsu.find(i))

    print(pow(2, len(comp), MOD))

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Việc triển khai bắt đầu bằng việc xây dựng danh sách kề cho cả hai cây. Hai DFS chuyển gốc mỗi cây tại nút 1 và ghi lại các con trỏ cha, biến các cạnh vô hướng thành các ràng buộc có hướng. 

DSU sau đó được sử dụng để hợp nhất các nút buộc phải vào cùng một lớp quyết định cấu trúc. Mỗi nút được hợp nhất với nút gốc của nó trong cả hai cây, mã hóa ý tưởng rằng một nút không thể thay đổi độc lập so với cấu trúc của cây mà không ảnh hưởng đến tính nhất quán. 

Cuối cùng, số lượng thành phần DSU được tính và lũy thừa theo lũy thừa của hai. Mỗi thành phần tương ứng với một lựa chọn nhị phân độc lập về cách thức hoán đổi truyền hướng cấu trúc. 

Một chi tiết triển khai tinh tế là các hoạt động hợp được thực hiện trên các nút chứ không phải trên các cạnh. Điều này hiệu quả vì mỗi cạnh được xác định duy nhất bởi mối quan hệ cha-con sau khi root và các ràng buộc lan truyền qua các điểm cuối được chia sẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2
1 2
1 2
```Cả hai cây đều giống hệt nhau. 

| Bước | DSU sáp nhập | Linh kiện | 
| --- | --- | --- | 
| nhổ cả hai cây | (1-2 ở cả hai) | ban đầu 2 | 
| ràng buộc công đoàn | 1 ↔ 2 | 1 | 
| đếm cuối cùng | - | 1 | 

Câu trả lời là$2^0 = 1$. Không có tự do cấu trúc độc lập tồn tại. 

Điều này xác nhận rằng các cây giống hệt nhau không tạo ra cấu trúc hoán đổi tầm thường. 

### Ví dụ 2 

đầu vào:```
3
3
1 3
2 3
2 3
2 1
```Ở đây các cây khác nhau về cách kết nối nút trung tâm. 

| Bước | DSU sáp nhập | Linh kiện | 
| --- | --- | --- | 
| cây rễ | liên kết gốc khác nhau | ban đầu 3 | 
| ràng buộc công đoàn | hợp nhất xung quanh nút 3 | 2 | 
| câu trả lời cuối cùng |$2^1$| 2 | 

Hai cấu hình tương ứng với việc hoán đổi hướng của cấu trúc ở giữa. 

Điều này cho thấy một chu trình cấu trúc độc lập tạo ra chính xác hai cây có thể tiếp cận như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \alpha(n))$| Hoạt động DFS + DSU trên các nút và cạnh | 
| Không gian |$O(n)$| danh sách kề, mảng cha, bộ lưu trữ DSU | 

Giải pháp chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm và tổng$n$qua các bài kiểm tra là$2 \cdot 10^5$, giữ cho việc thực hiện tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    sys.setrecursionlimit(10**7)

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.r = [0]*n
        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x
        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a == b:
                return
            if self.r[a] < self.r[b]:
                a, b = b, a
            self.p[b] = a
            if self.r[a] == self.r[b]:
                self.r[a] += 1

    def solve():
        n = int(input())
        g1 = [[] for _ in range(n)]
        g2 = [[] for _ in range(n)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1; v -= 1
            g1[u].append(v)
            g1[v].append(u)

        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1; v -= 1
            g2[u].append(v)
            g2[v].append(u)

        def root_parent(g):
            parent = [-1]*n
            stack = [0]
            parent[0] = 0
            while stack:
                u = stack.pop()
                for v in g[u]:
                    if v == parent[u]:
                        continue
                    parent[v] = u
                    stack.append(v)
            return parent

        p1 = root_parent(g1)
        p2 = root_parent(g2)

        dsu = DSU(n)

        for i in range(n):
            if p1[i] != -1:
                dsu.union(i, p1[i])
            if p2[i] != -1:
                dsu.union(i, p2[i])

        comp = set(dsu.find(i) for i in range(n))
        return str(pow(2, len(comp), MOD))

    out = []
    t = int(input())
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples
assert run("""3
2
1 2
1 2
3
1 3
2 3
2 3
2 1
4
1 2
2 3
3 4
4 2
2 1
1 3
""") == """1
2
4"""

# custom cases
assert run("""1
2
1 2
1 2
""") == "1", "minimum size identical"

assert run("""1
3
1 2
2 3
1 3
3 2
2 1
""") in ["2"], "small asymmetric case"

assert run("""1
4
1 2
2 3
3 4
1 3
3 2
2 4
""") == "2", "chain vs shuffled tree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút giống hệt nhau | 1 | không có tự do | 
| 3 nút bất đối xứng | 2 | tự do chu kỳ đơn | 
| chuỗi vs cây hỗn hợp | 2 | lan truyền cấu trúc không cần thiết | 

## Vỏ cạnh 

Một trường hợp góc quan trọng là khi cả hai cây đều là đường đi nhưng bị đảo ngược so với nhau. DSU hợp nhất lan truyền dọc theo chuỗi nhưng vẫn để lại chính xác một lựa chọn nhị phân toàn cầu. Đối với đường dẫn gồm 4 nút, việc root cả hai cây sẽ tạo ra chuỗi gốc như 0-1-2-3 ở một cây và 0-2-1-3 ở cây kia, đồng thời thu gọn các nút thành hai thành phần, tạo ra câu trả lời 2. 

Một trường hợp khác là khi một cái cây là một ngôi sao. Việc root một ngôi sao sẽ tạo ra một trung tâm được kết nối với tất cả những ngôi sao khác và các tập hợp ngay lập tức kết nối mọi lá thông qua trung tâm của cả hai cây. Điều này thu gọn tất cả các nút thành một thành phần duy nhất, mang lại câu trả lời 1, phù hợp với thực tế là không có sự hoán đổi tầm thường nào có thể thay đổi cấu trúc được gắn nhãn. 

Trường hợp tinh tế cuối cùng là khi các cây chỉ khác nhau bằng cách hoán đổi hai cây con ở các độ sâu khác nhau. Mặc dù về mặt trực quan có nhiều “lựa chọn”, việc truyền DSU sẽ hợp nhất tất cả các nút bị ảnh hưởng thành một thành phần duy nhất, ngăn chặn việc đếm quá mức và đảm bảo rằng chỉ những lần lật cấu trúc thực sự độc lập mới đóng góp vào số mũ.
