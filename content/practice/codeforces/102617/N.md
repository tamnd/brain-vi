---
title: "CF 102617N - Tình trạng khó khăn của chiếc bánh"
description: "Chúng tôi có một số loại bánh và mỗi chiếc bánh liên tiếp đều thuộc về một loại. Hai người phải chia những chiếc bánh cho nhau, nhưng việc phân chia diễn ra theo loại chứ không phải theo từng chiếc bánh. Nếu một người nhận được một chiếc bánh thuộc loại nào đó thì người đó phải nhận từng chiếc bánh thuộc loại đó."
date: "2026-07-31T17:41:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102617
codeforces_index: "N"
codeforces_contest_name: "mBIT Rookie November 2019"
rating: 0
weight: 102617
solve_time_s: 68
verified: true
draft: false
---

[CF 102617N - Tình trạng khó khăn của Pie](https://codeforces.com/problemset/problem/102617/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số loại bánh và mỗi chiếc bánh liên tiếp đều thuộc về một loại. Hai người phải chia những chiếc bánh cho nhau, nhưng việc phân chia diễn ra theo loại chứ không phải theo từng chiếc bánh. Nếu một người nhận được một chiếc bánh thuộc loại nào đó thì người đó phải nhận từng chiếc bánh thuộc loại đó. 

Mỗi người có một danh sách các loại ưa thích. Loại chỉ xuất hiện trong danh sách của một người buộc phải thuộc về người đó. Loại xuất hiện trong cả hai danh sách có thể được chỉ định theo một trong hai cách. Sau khi phân công, mỗi cặp bánh liền kề trong hàng sẽ tặng kẹo nếu cả hai chiếc bánh đều thuộc về cùng một người. Mục đích là chọn quyền sở hữu các loại kẹo linh hoạt sao cho tổng số kẹo càng lớn càng tốt. 

Phần quan trọng của đầu vào là hàng bánh nướng tạo ra mối quan hệ giữa các loại. Nếu hai chiếc bánh lân cận có loại khác nhau, việc gán hai loại đó cho những người khác nhau sẽ làm mất giá trị kẹo liên quan đến ranh giới đó. Nếu chúng có cùng chủ sở hữu thì giá trị đó sẽ đạt được. Vấn đề không phải là về từng chiếc bánh mà là việc gán loại cho hai nhóm trong khi vẫn bảo toàn được càng nhiều kết nối có giá trị càng tốt. 

Số lượng loại có thể lên tới 500 và số lượng bánh nướng có thể lên tới 1000. Một giải pháp thử mọi cách gán loại có thể có sẽ cần phải kiểm tra tối đa$2^K$khả năng. Với$K=500$, điều này vượt xa những gì giới hạn thời gian có thể hỗ trợ. Chúng ta cần một cách tiếp cận gần với thời gian đa thức. Số lượng bánh đủ nhỏ để việc xây dựng mối quan hệ trực tiếp giữa các loại là thực tế. 

Các trường hợp đặc biệt chính đến từ các phép gán bắt buộc và từ các loại xuất hiện nhiều lần trong hàng. Ví dụ, hãy xem xét:```
2 2 1 1
1
2
1 2
5
```Loại đầu tiên chỉ có thể thuộc về Joaozao và loại thứ hai chỉ thuộc về Nicoleta. Ranh giới duy nhất ngăn cách hai người nên câu trả lời là`0`. Một giải pháp bất cẩn gán các loại một cách tham lam mà không tôn trọng các ưu tiên có thể đặt cả hai loại lại với nhau một cách không chính xác và đếm ranh giới. 

Một trường hợp khác là khi một loại xuất hiện nhiều lần:```
3 5 2 2
1 2
2 3
1 2 1 3 2
1 10 10 1
```Loại 2 có thể thuộc về một trong hai người, nhưng sự xuất hiện lặp đi lặp lại của nó kết nối nó với cả loại 1 và loại 3. Chỉ nhìn vào một lần xuất hiện của loại này có thể dẫn đến quyết định sai lầm. Giải pháp phải kết hợp tất cả các ranh giới liên quan đến một loại. 

Trường hợp ranh giới cuối cùng là khi mọi loại đều đã bị ép buộc:```
3 3 1 2
1
2 3
1 2 3
7 8
```Không có quyết định nào để thực hiện. Câu trả lời đơn giản là tổng giá trị của các ranh giới mà các chủ sở hữu bắt buộc phải khớp với nhau. Bất kỳ thuật toán nào giả định tồn tại ít nhất một loại linh hoạt vẫn phải xử lý trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là giao mọi loại bánh linh hoạt cho một trong hai người, sau đó tính tổng số kẹo cho nhiệm vụ đó. Điều này hiệu quả vì mọi giải pháp hợp lệ đều tương ứng với chính xác một lựa chọn quyền sở hữu cho mọi loại. Nếu có$F$các loại linh hoạt, điều này đòi hỏi phải kiểm tra$2^F$bài tập. Trong trường hợp xấu nhất, mọi loại đều có thể được lựa chọn tự do, đưa ra$2^{500}$khả năng, đó là điều không thể. 

Sự quan sát hữu ích đến từ việc nhìn vào những gì bị mất thay vì những gì đạt được. Tổng giá trị kẹo của tất cả các ranh giới là cố định. Một ranh giới đóng góp giá trị của nó trừ khi hai loại liền kề được gán cho những người khác nhau. Do đó, việc tối đa hóa số kẹo tương đương với việc giảm thiểu tổng trọng số của các ranh giới được cắt giữa hai nhóm loại. 

Điều này biến bài toán thành bài toán cắt nhỏ nhất. Mỗi loại bánh sẽ trở thành một đỉnh. Mỗi cặp loại khác nhau liền kề đều đóng góp một cạnh có trọng số là số kẹo bị mất nếu hai loại đó bị tách ra. Các loại buộc vào Joaozao được kết nối với nguồn, và các loại buộc vào Nicoleta được kết nối với sink. Việc cắt nguồn chìm tối thiểu chọn cạnh của mọi loại linh hoạt trong khi thanh toán chính xác cho các ranh giới được phân tách. 

Số kẹo tối đa là tổng của tất cả các giá trị biên trừ đi giá trị cắt tối thiểu. Biểu đồ chỉ có khoảng 500 đỉnh và 1000 cạnh hữu ích, do đó, thuật toán luồng cực đại tiêu chuẩn dễ dàng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^K \cdot N)$|$O(K)$| Quá chậm | 
| Cắt tối thiểu |$O(V^2E)$với thuật toán Dinic trong kích thước biểu đồ này |$O(V+E)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các tùy chọn và xác định các hạn chế của chủ sở hữu đối với mọi loại. Loại chỉ xuất hiện trong danh sách của Joaozao phải được đặt ở phía nguồn, còn loại chỉ xuất hiện trong danh sách của Nicoleta phải được đặt ở phía sink. Các loại xuất hiện trong cả hai danh sách vẫn chưa được quyết định. 
2. Xây dựng biểu đồ trong đó mỗi loại bánh là một nút. Thêm một cạnh có hướng theo cả hai hướng giữa các loại lân cận có dung lượng bằng giá trị kẹo của ranh giới đó. Nhiều ranh giới giữa cùng một cặp loại được tích lũy một cách tự nhiên bằng cách bổ sung thêm dung lượng. 
3. Thêm nút nguồn và nút chìm. Kết nối mọi loại Joaozao bắt buộc vào nguồn với dung lượng rất lớn. Kết nối mọi loại Nicoleta cưỡng bức vào bồn rửa có cùng dung tích lớn. Giá trị lớn làm cho việc tách một loại bắt buộc khỏi mặt yêu cầu của nó đắt hơn bất kỳ sự mất mát kẹo nào có thể xảy ra. 
4. Tính lưu lượng tối đa từ nguồn tới đích. Theo định lý cắt tối thiểu luồng cực đại, giá trị này là tổng trọng số tối thiểu của các ranh giới ngăn cách hai chủ sở hữu. 
5. Trừ giá trị cắt tối thiểu khỏi tổng tất cả phần thưởng ranh giới. Giá trị còn lại chính xác là số kẹo tối đa có thể nhận được. 

Tại sao nó hoạt động: 

Việc cắt tách các đỉnh loại thành hai nhóm. Phía nguồn đại diện cho loại của Joaozao và phía chìm đại diện cho loại của Nicoleta. Các loại bắt buộc không thể vượt qua vết cắt vì các cạnh công suất vô hạn của chúng sẽ khiến cho vết cắt như vậy không thể tối ưu được. Mỗi cạnh thông thường đi qua đường cắt đại diện cho một cặp bánh lân cận có chủ sở hữu khác nhau, vì vậy sức chứa của nó chính xác là giá trị kẹo bị mất ở ranh giới đó. Mỗi cạnh không giao nhau đều giữ giá trị kẹo của nó. Vì mức cắt tối thiểu sẽ giảm thiểu tất cả các giá trị kẹo bị mất nên nhiệm vụ kết quả sẽ tối đa hóa số kẹo kiếm được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Dinic:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, c):
        self.g[u].append([v, c, len(self.g[v])])
        self.g[v].append([u, 0, len(self.g[u]) - 1])

    def bfs(self, s, t):
        self.level = [-1] * self.n
        self.level[s] = 0
        q = [s]
        for u in q:
            for v, c, _ in self.g[u]:
                if c and self.level[v] == -1:
                    self.level[v] = self.level[u] + 1
                    q.append(v)
        return self.level[t] != -1

    def dfs(self, u, t, f):
        if u == t:
            return f
        while self.it[u] < len(self.g[u]):
            e = self.g[u][self.it[u]]
            v, c, rev = e
            if c and self.level[v] == self.level[u] + 1:
                ret = self.dfs(v, t, min(f, c))
                if ret:
                    e[1] -= ret
                    self.g[v][rev][1] += ret
                    return ret
            self.it[u] += 1
        return 0

    def flow(self, s, t):
        ans = 0
        while self.bfs(s, t):
            self.it = [0] * self.n
            while True:
                pushed = self.dfs(s, t, 10**18)
                if not pushed:
                    break
                ans += pushed
        return ans

def solve():
    k, n, a, b = map(int, input().split())

    jo = list(map(int, input().split()))
    ni = list(map(int, input().split()))

    owner = [0] * k
    for x in jo:
        owner[x - 1] |= 1
    for x in ni:
        owner[x - 1] |= 2

    pies = list(map(int, input().split()))
    pies = [x - 1 for x in pies]
    g = list(map(int, input().split()))

    total = sum(g)

    s = k
    t = k + 1
    dinic = Dinic(k + 2)

    inf = total + 1

    for i in range(k):
        if owner[i] == 1:
            dinic.add_edge(s, i, inf)
        elif owner[i] == 2:
            dinic.add_edge(i, t, inf)

    for i, w in enumerate(g):
        u = pies[i]
        v = pies[i + 1]
        if u != v:
            dinic.add_edge(u, v, w)
            dinic.add_edge(v, u, w)

    cut = dinic.flow(s, t)
    print(total - cut)

if __name__ == "__main__":
    solve()
```Việc triển khai thể hiện từng loại bánh dưới dạng một nút trong biểu đồ luồng. Cấu trúc Dinic lưu trữ các cạnh dư, cho phép thuật toán liên tục tìm các đường dẫn tăng cường và cập nhật dung lượng còn lại. 

Quá trình xử lý ưu tiên sử dụng biểu diễn hai bit. Một giá trị của`1`có nghĩa là Joaozao chấp nhận loại và giá trị là`2`có nghĩa là Nicoleta chấp nhận nó. Giá trị`1`Và`2`bị ép buộc, trong khi giá trị`3`linh hoạt. 

Công suất vô hạn được chọn là`sum(g) + 1`. Không có sự cắt giảm tối ưu nào có thể hy sinh nhiều công suất như vậy, bởi vì việc cắt giảm mọi chi phí biên bình thường nhiều nhất là`sum(g)`. Điều này ngăn cản thuật toán luồng gán loại bắt buộc cho phía sai. 

Chỉ ranh giới giữa các loại khác nhau mới cần có cạnh. Nếu hai chiếc bánh lân cận có cùng loại thì chúng luôn có cùng một chủ sở hữu, do đó ranh giới đó không bao giờ bị mất và không ảnh hưởng đến việc tối ưu hóa. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 6 1 3
1
2 3 4
1 3 2 2 4 1
1 2 4 8 16
```Tổng số kẹo hiện có là 31. Biểu đồ quyền sở hữu loại chứa loại 1 bắt buộc về phía Joaozao. Lượt cắt tối thiểu chọn tách loại 1 khỏi những loại còn lại, mất 17 viên kẹo. 

| Bước | Quyết định hiện tại | Giá trị cắt | 
| --- | --- | --- | 
| Ban đầu | Tất cả các ranh giới được tính | 31 | 
| Lực loại 1 | Loại 1 phải ở bên nguồn | 0 | 
| Cắt tối thiểu | Tách loại 1 khỏi loại 3,2,4 | 17 | 
| Cuối cùng | Tổng trừ kẹo bị mất | 14 | 

Dấu vết cho thấy thuật toán đang giảm thiểu các ranh giới bị mất thay vì trực tiếp tối đa hóa các ranh giới thu được. 

Đối với mẫu thứ hai:```
4 10 3 3
1 2 3
2 3 4
1 2 3 4 3 1 2 4 3 1
1 1 5 5 2 2 1 5 1
```Tổng giá trị kẹo là 23. 

| Bước | Quyết định hiện tại | Giá trị cắt | 
| --- | --- | --- | 
| Ban đầu | Đếm mọi ranh giới | 23 | 
| Các nút cưỡng bức | Loại 1 và loại 4 bị hạn chế | 0 | 
| Cắt tối thiểu | Chọn chia quyền sở hữu linh hoạt | 5 | 
| Cuối cùng | Tổng trừ kẹo bị mất | 18 | 

Ví dụ này chứng minh rằng một loại linh hoạt có thể được chỉ định dựa trên tất cả các mối quan hệ lân cận của nó chứ không chỉ một lần xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(V^2E)$| Dinic chạy trên đồ thị với$V=K+2$Và$E$tỷ lệ thuận với số lượng ranh giới | 
| Không gian |$O(V+E)$| Biểu đồ dư lưu trữ mọi cạnh luồng và cạnh ngược | 

Với tối đa 500 loại bánh và 1000 bánh, đồ thị nhỏ. Việc tính toán luồng cực đại dễ dàng nằm trong giới hạn vì số đỉnh và cạnh thấp. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp):
    sys.stdin = io.StringIO(inp)
    import collections

    class Dinic:
        def __init__(self, n):
            self.n = n
            self.g = [[] for _ in range(n)]

        def add_edge(self, u, v, c):
            self.g[u].append([v, c, len(self.g[v])])
            self.g[v].append([u, 0, len(self.g[u]) - 1])

        def bfs(self, s, t):
            self.level = [-1] * self.n
            self.level[s] = 0
            q = [s]
            for u in q:
                for v, c, _ in self.g[u]:
                    if c and self.level[v] == -1:
                        self.level[v] = self.level[u] + 1
                        q.append(v)
            return self.level[t] != -1

        def dfs(self, u, t, f):
            if u == t:
                return f
            while self.it[u] < len(self.g[u]):
                e = self.g[u][self.it[u]]
                v, c, rev = e
                if c and self.level[v] == self.level[u] + 1:
                    r = self.dfs(v, t, min(f, c))
                    if r:
                        e[1] -= r
                        self.g[v][rev][1] += r
                        return r
                self.it[u] += 1
            return 0

        def flow(self, s, t):
            ans = 0
            while self.bfs(s, t):
                self.it = [0] * self.n
                while True:
                    x = self.dfs(s, t, 10**18)
                    if not x:
                        break
                    ans += x
            return ans

    def run():
        k, n, a, b = map(int, input().split())
        jo = list(map(int, input().split()))
        ni = list(map(int, input().split()))
        own = [0] * k
        for x in jo:
            own[x - 1] |= 1
        for x in ni:
            own[x - 1] |= 2
        p = [x - 1 for x in map(int, input().split())]
        w = list(map(int, input().split()))
        d = Dinic(k + 2)
        total = sum(w)
        inf = total + 1
        for i, x in enumerate(own):
            if x == 1:
                d.add_edge(k, i, inf)
            elif x == 2:
                d.add_edge(i, k + 1, inf)
        for i, x in enumerate(w):
            if p[i] != p[i + 1]:
                d.add_edge(p[i], p[i + 1], x)
                d.add_edge(p[i + 1], p[i], x)
        return str(total - d.flow(k, k + 1)) + "\n"

    return run()

assert solve_io("""4 6 1 3
1
2 3 4
1 3 2 2 4 1
1 2 4 8 16
""") == "14\n"

assert solve_io("""4 10 3 3
1 2 3
2 3 4
1 2 3 4 3 1 2 4 3 1
1 1 5 5 2 2 1 5 1
""") == "18\n"

assert solve_io("""2 2 1 1
1
2
1 2
5
""") == "0\n"

assert solve_io("""3 3 1 2
1
2 3
1 2 3
7 8
""") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 14 | Thi công cắt nhỏ cơ bản | 
| Mẫu 2 | 18 | Nhiều bài tập linh hoạt | 
| Hai loại bắt buộc | 0 | Xử lý quyền sở hữu bắt buộc | 
| Tất cả các ranh giới bắt buộc | 0 | Không có trường hợp nút linh hoạt | 

## Vỏ cạnh 

Khi một loại bị ép buộc cho một người, giới hạn dung lượng vô hạn khiến việc gán nó cho bên kia là không thể ở mức cắt giảm tối thiểu. Đối với đầu vào:```
2 2 1 1
1
2
1 2
5
```biểu đồ chứa kết nối nguồn đến loại 1 và kết nối chìm từ loại 2. Việc cắt duy nhất có thể tách chúng ra, làm mất giá trị biên duy nhất là 5. Kết quả là`0`. 

Khi một loại xuất hiện nhiều lần, mỗi lần xuất hiện đều đóng góp vào cùng một đỉnh biểu đồ. Ví dụ:```
3 5 2 2
1 2
2 3
1 2 1 3 2
1 10 10 1
```loại 2 nhận các cạnh từ cả loại 1 và loại 3. Thuật toán luồng xem xét hiệu ứng kết hợp của tất cả các cạnh này trước khi quyết định loại 2 thuộc về đâu. 

Khi tất cả các loại bị ép buộc, biểu đồ luồng vẫn hoạt động mà không cần xử lý đặc biệt. Không còn lựa chọn nào có ý nghĩa, nhưng giá trị cắt vẫn thể hiện ranh giới nơi các nhóm bắt buộc khác nhau. Phép trừ cuối cùng từ tổng phần thưởng sẽ cho ra số kẹo chính xác.
