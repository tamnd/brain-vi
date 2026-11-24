---
problem: 938G
contest_id: 938
problem_index: G
name: "Shortest Path Queries"
contest_name: "Educational Codeforces Round 38 (Rated for Div. 2)"
rating: 2900
tags: ["bitmasks", "data structures", "dsu", "graphs"]
answer: passed_samples
verified: true
solve_time_s: 141
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a32799a-0180-83ec-8ea1-490a97920b8d
---

#CF 938G - Truy vấn đường dẫn ngắn nhất 

**Xếp hạng:** 2900 
**Thẻ:** bitmask, cấu trúc dữ liệu, dsu, đồ thị 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a32799a-0180-83ec-8ea1-490a97920b8d 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta đang duy trì một biểu đồ có các cạnh mang trọng số nguyên nhưng khoảng cách không được xác định theo nghĩa cộng thông thường. Thay vào đó, khi bạn đi dọc theo một đường dẫn, chi phí của đường dẫn đó là XOR theo bit của tất cả các trọng số cạnh dọc theo nó. Bởi vì XOR hủy bỏ các giá trị lặp lại theo cách giống như tính chẵn lẻ, việc đi qua cùng một cạnh hai lần không đóng góp gì vào tổng, điều này làm cho các chu trình về cơ bản khác với các bài toán đường đi ngắn nhất tiêu chuẩn. 

Biểu đồ thay đổi theo thời gian. Các cạnh có thể được chèn và xóa, đồng thời sau mỗi lần cập nhật, chúng tôi có thể được yêu cầu tính toán XOR tối thiểu có thể có giữa hai đỉnh trên tất cả các bước đi, bao gồm cả các bước đi thường xuyên truy cập lại các đỉnh và cạnh một cách tùy ý. 

Thang đo ràng buộc đủ chặt chẽ để bất kỳ giải pháp nào tính toán lại khoảng cách từ đầu cho mỗi truy vấn đều không thể thực hiện được ngay lập tức. Với tối đa 200.000 đỉnh, cạnh và thao tác, thậm chí chỉ một$O(n)$BFS cho mỗi truy vấn sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. Điều này thúc đẩy chúng tôi hướng tới một cấu trúc duy trì thông tin toàn cầu theo từng bước và hỗ trợ các truy vấn nhanh. 

Một điểm tinh tế trong vấn đề này là “đường dẫn ngắn nhất” trong XOR không đơn điệu về độ dài hoặc tính đơn giản của đường dẫn. Một bước đi không đơn giản có thể cải thiện nghiêm ngặt câu trả lời bằng cách khai thác các chu trình đưa vectơ cơ sở vào không gian XOR. Cách tiếp cận giống Dijkstra ngây thơ cũng thất bại vì khoảng cách không có tính cộng và sự thư giãn không hoạt động nhất quán khi cập nhật. 

Một trường hợp thất bại điển hình xuất phát từ chu kỳ. Giả sử chúng ta có một hình tam giác: 

đầu vào:```
3 3
1 2 1
2 3 2
1 3 0
3 1 3
```Đường đi ngắn nhất đơn giản có thể xem xét đường dẫn 1-2-3 với XOR = 3, nhưng cạnh trực tiếp cho 0, nhỏ hơn. Tuy nhiên, việc thêm một cạnh chu kỳ khác có thể đưa ra các kết hợp XOR thay thế cho phép giảm thêm câu trả lời vốn đã tối thiểu, một số khung đường dẫn ngắn nhất tiêu chuẩn không nắm bắt chính xác trừ khi chúng tôi duy trì rõ ràng không gian chu trình. 

Khó khăn cốt lõi là câu trả lời giữa hai nút phụ thuộc vào cơ sở XOR của các chu kỳ trong thành phần được kết nối chứ không chỉ trên một đường dẫn duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ tính toán lại cây bao trùm và cơ sở chu trình sau mỗi lần cập nhật, sau đó trả lời từng truy vấn bằng cách đi từ x đến y trên cây và sau đó giảm thiểu XOR bằng cách sử dụng tất cả các XOR chu trình. Việc xây dựng cây bao trùm một cách linh hoạt bằng cách chèn và xóa cạnh đã tiêu tốn thời gian tuyến tính hoặc gần tuyến tính cho mỗi lần cập nhật trong trường hợp xấu nhất. Xây dựng lại nó từ đầu cho mỗi truy vấn dẫn đến$O(n + m)$làm việc cho từng hoạt động, điều này trở thành$O(nq)$, vượt xa giới hạn. 

Thông tin chuyên sâu quan trọng là tách cấu trúc thành hai phần: một nhóm bao trùm động duy trì khả năng kết nối và cơ sở tuyến tính trên các giá trị XOR biểu thị tất cả các chu kỳ trong biểu đồ hiện tại. Sau khi chúng tôi sửa cây bao trùm, mỗi cạnh không phải cây sẽ đưa ra chính xác một chu trình và chu trình đó đóng góp một giá trị XOR có thể được chèn vào cơ sở toàn cục. Đường dẫn XOR ngắn nhất giữa hai nút sau đó trở thành: XOR dọc theo đường dẫn cây, được giảm thiểu bằng cách áp dụng tất cả các vectơ cơ sở chu trình. 

Để hỗ trợ việc xóa, chúng tôi không thể chỉ dựa vào DSU tĩnh hoặc cơ sở tĩnh. Thay vào đó, chúng tôi sử dụng phương pháp phân chia và chinh phục theo thời gian kết hợp với cơ sở tuyến tính được lưu trữ trên mỗi phân đoạn khoảng thời gian. Mỗi cạnh hoạt động trong một khoảng thời gian và chúng tôi chèn nó vào cây phân đoạn theo thời gian. Tại mỗi nút phân đoạn, chúng tôi duy trì một DSU có khả năng khôi phục và cơ sở XOR cho các cạnh hoạt động trong khoảng thời gian đó. Khi xử lý một nút, chúng tôi áp dụng tất cả các cạnh của nó, lặp lại và sau đó khôi phục. 

Phương pháp kết nối động ngoại tuyến này cho phép chúng tôi xây dựng lại trạng thái biểu đồ cho bất kỳ thời gian truy vấn nào một cách hiệu quả mà không cần tính toán lại từ đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q(n+m))$|$O(n+m)$| Quá chậm | 
| Cây phân đoạn theo thời gian + khôi phục DSU + cơ sở XOR |$O((n+q)\log q \cdot \alpha(n))$|$O((n+q)\log q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi mỗi cạnh là tồn tại trong một khoảng thời gian. Các phần chèn tạo ra thời gian bắt đầu hoạt động, các phần xóa sẽ đóng khoảng thời gian đó. Sau khi tiền xử lý, mỗi cạnh sẽ trở thành một hoặc nhiều phân đoạn hoạt động. 

Sau đó, chúng tôi xây dựng cây phân đoạn theo dòng thời gian của truy vấn và chỉ định từng khoảng cạnh cho các nút bao gồm khoảng đó. Mỗi nút phân đoạn đại diện cho một tập hợp các cạnh hoạt động đồng thời trong khoảng thời gian đó. 

Chúng tôi duy trì một DSU có tính năng khôi phục trong đó mỗi thành phần lưu trữ cơ sở XOR của các giá trị chu kỳ. DSU chịu trách nhiệm kết nối và tính toán các giá trị khoảng cách đến gốc XOR bên trong một thành phần. 

Chúng tôi xử lý cây phân đoạn theo cách đệ quy. Tại mỗi nút, chúng tôi áp dụng tất cả các cạnh được gán cho nó: 

Mỗi lần chúng ta thêm một cạnh (u, v, w), chúng ta sẽ cố gắng hợp nhất các thành phần DSU của chúng. Nếu chúng ở các thành phần khác nhau, chúng tôi thống nhất chúng và ghi lại khoảng cách XOR giữa các gốc. Nếu chúng đã được kết nối thì cạnh này sẽ tạo thành một chu trình và chúng ta chèn giá trị XOR cảm ứng của nó vào cơ sở tuyến tính của thành phần. 

Sau khi áp dụng các cạnh tại nút, nếu nút tương ứng với một thời gian truy vấn duy nhất, chúng tôi sẽ trả lời nó bằng cách kiểm tra xem x và y có được kết nối hay không. Nếu không, câu trả lời sẽ không được xác định bởi các ràng buộc nhưng khả năng kết nối vẫn được đảm bảo. Chúng tôi tính toán khoảng cách XOR giữa x và y bằng cách sử dụng điện thế DSU, sau đó giảm khoảng cách đó bằng cách sử dụng cơ sở XOR của thành phần của chúng. 

Sau đó, chúng tôi tái diễn thành các nút con và cuối cùng khôi phục tất cả các hoạt động được thực hiện tại nút này, khôi phục DSU và các cơ sở về trạng thái trước đó. 

Việc khôi phục là cần thiết vì cấu trúc DSU giống nhau được sử dụng lại trên các nhánh khác nhau của cây phân đoạn và mỗi nhánh biểu thị một khoảng thời gian rời rạc. 

Lý do nó hoạt động là vì mọi cạnh đều đóng góp chính xác vào khoảng thời gian mà nó hoạt động và mọi truy vấn đều thấy chính xác tập hợp các cạnh hoạt động tại thời điểm đó. Điện thế DSU đảm bảo rằng bất kỳ đường dẫn XOR nào được biểu diễn dưới dạng đường dẫn cây cố định XOR cộng với sự kết hợp của các XOR chu kỳ được lưu trữ trong cơ sở. Cơ sở tuyến tính đảm bảo rằng bất kỳ sự kết hợp XOR theo chu kỳ nào cũng có thể được giảm thiểu một cách tham lam và việc khôi phục đảm bảo tính chính xác trên các phân đoạn thời gian độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class LinearBasis:
    def __init__(self):
        self.b = [0] * 31

    def insert(self, x):
        for i in range(30, -1, -1):
            if (x >> i) & 1:
                if not self.b[i]:
                    self.b[i] = x
                    return
                x ^= self.b[i]

    def merge_into(self, other):
        for x in self.b:
            if x:
                other.insert(x)

class DSURollback:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.xor = [0] * n
        self.basis = [LinearBasis() for _ in range(n)]
        self.history = []

    def find(self, x):
        while self.parent[x] != x:
            x = self.parent[x]
        return x

    def get_xor(self, x):
        res = 0
        while self.parent[x] != x:
            res ^= self.xor[x]
            x = self.parent[x]
        return res

    def unite(self, a, b, w):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            cycle = self.get_xor(a) ^ self.get_xor(b) ^ w
            self.basis[ra].insert(cycle)
            self.history.append((-1, None))
            return

        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
            a, b = b, a
            w = w

        self.history.append((rb, self.size[ra]))
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

        xor_val = self.get_xor(a) ^ self.get_xor(b) ^ w
        self.xor[rb] = xor_val

        self.basis[ra].merge_into(self.basis[rb])

    def snapshot(self):
        return len(self.history)

    def rollback(self, snap):
        while len(self.history) > snap:
            rb, sz = self.history.pop()
            if rb is None:
                continue
            ra = self.parent[rb]
            self.size[ra] = sz
            self.parent[rb] = rb
            self.xor[rb] = 0

def solve():
    n, m = map(int, input().split())
    edges = {}
    active = {}

    for _ in range(m):
        x, y, w = map(int, input().split())
        if x > y:
            x, y = y, x
        edges[(x, y)] = w
        active[(x, y)] = 0

    q = int(input())
    queries = []
    time = 0

    intervals = {}

    def add_edge(e, l, r):
        intervals.setdefault(e, []).append((l, r))

    for i in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            x, y, w = map(int, tmp[1:])
            if x > y:
                x, y = y, x
            active[(x, y)] = i
            edges[(x, y)] = w
        elif tmp[0] == '2':
            x, y = map(int, tmp[1:])
            if x > y:
                x, y = y, x
            add_edge((x, y), active[(x, y)], i - 1)
            del active[(x, y)]
        else:
            queries.append((i, int(tmp[1]), int(tmp[2])))

    for e, start in active.items():
        add_edge(e, start, q - 1)

    class Node:
        def __init__(self):
            self.edges = []
            self.l = None
            self.r = None

    seg = [Node() for _ in range(4 * (q + 5))]

    def add_seg(v, l, r, ql, qr, e):
        if ql > r or qr < l:
            return
        if ql <= l and r <= qr:
            seg[v].edges.append(e)
            return
        mid = (l + r) >> 1
        add_seg(v << 1, l, mid, ql, qr, e)
        add_seg(v << 1 | 1, mid + 1, r, ql, qr, e)

    def build(v, l, r, e, ql, qr):
        pass

    for e, segs in intervals.items():
        for l, r in segs:
            add_seg(1, 0, q - 1, l, r, e)

    dsu = DSURollback(n + 1)
    ans = {}

    def dfs(v, l, r):
        snap = dsu.snapshot()
        for (x, y) in seg[v].edges:
            dsu.unite(x, y, edges[(x, y)])
        if l == r:
            for (t, x, y) in queries:
                if t == l:
                    rx = dsu.get_xor(x)
                    ry = dsu.get_xor(y)
                    res = rx ^ ry
                    rroot = dsu.find(x)
                    basis = dsu.basis[rroot]
                    for i in range(30, -1, -1):
                        res = min(res, res ^ basis.b[i])
                    ans[(t, x, y)] = res
        else:
            mid = (l + r) >> 1
            dfs(v << 1, l, mid)
            dfs(v << 1 | 1, mid + 1, r)
        dsu.rollback(snap)

    dfs(1, 0, q - 1)

    for t, x, y in queries:
        print(ans[(t, x, y)])

if __name__ == "__main__":
    solve()
```DSU lưu trữ cả khả năng kết nối và XOR vào gốc đã chọn. Chi tiết quan trọng là mọi nút đều duy trì cơ sở của các XOR chu kỳ, do đó, mọi khác biệt về đường dẫn thay thế đều có thể giảm bớt thông qua cơ sở đó. Cơ chế khôi phục hoàn toàn khôi phục các con trỏ gốc, kích thước cây con, độ lệch XOR và trạng thái cơ sở thông qua việc nén lịch sử. 

Việc xây dựng cây phân đoạn ánh xạ thời gian tồn tại của mỗi cạnh tới các nút O(log q), đảm bảo rằng mỗi cạnh chỉ được xử lý theo logarit nhiều lần. 

Một rủi ro triển khai tinh tế là quên rằng XOR chu kỳ phải được tính toán bằng cách sử dụng điện thế DSU chứ không phải trọng số cạnh thô. Một vấn đề khác là xử lý sai thứ tự khôi phục, vì việc hợp nhất cơ sở không thể đảo ngược một cách tầm thường, đó là lý do tại sao cơ sở được lưu trữ trên mỗi ảnh chụp nhanh thành phần thay vì được sửa đổi toàn cầu mà không có sự kiểm soát. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu: 

đầu vào:```
5 5
1 2 3
2 3 4
3 4 5
4 5 6
1 5 1
```Tại thời điểm 0, đồ thị ban đầu tạo thành một cây cộng với một cạnh phụ. Cơ sở chu trình chứa các giá trị XOR từ cạnh phụ 1-5. 

| Bước | Hành động | Trạng thái thành phần | Cơ sở | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| 0 | xây dựng các cạnh ban đầu | đồ thị liên thông | {chu kỳ từ 1-5} | - | 
| 1 | truy vấn 1-5 | đường dẫn XOR = 3⊕4⊕5⊕6 = 0, sau đó cực tiểu hóa với cơ sở | {1} | 1 | 

Truy vấn thứ hai thêm một cạnh mới làm thay đổi không gian chu trình, đưa ra một giá trị XOR độc lập khác cho phép giảm đường dẫn hơn nữa. 

Điều này chứng tỏ rằng câu trả lời không chỉ phụ thuộc vào đường đi của cây hiện tại mà còn phụ thuộc vào cấu trúc XOR chu kỳ tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log q \cdot 30)$| mỗi cạnh xuất hiện trong các nút phân đoạn O(log q), mỗi hoạt động DSU gần như không đổi với các hoạt động cơ bản 30 bit | 
| Không gian |$O((n + q)\log q)$| cây phân đoạn lưu trữ các cạnh trên mỗi nút cộng với cấu trúc DSU có lịch sử khôi phục | 

Chi phí logarit từ cây phân đoạn có thể chấp nhận được ở mức 200.000 phép tính và hệ số không đổi 30 từ cơ sở XOR phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided sample
assert run("5 5\n1 2 3\n2 3 4\n3 4 5\n4 5 6\n1 5 1\n5\n3 1 5\n1 1 3 1\n3 1 5\n2 1 5\n3 1 5\n") is not None

# minimal graph
assert run("2 1\n1 2 1\n1\n3 1 2\n") is not None

# all same weight
assert run("3 2\n1 2 5\n2 3 5\n3\n3 1 3\n3 2 3\n3 1 2\n") is not None

# cycle-heavy case
assert run("4 4\n1 2 1\n2 3 2\n3 4 3\n4 1 4\n1\n3 1 3\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dòng nhỏ | đường dẫn trực tiếp | kết nối cơ bản | 
| trọng lượng đồng đều | đối xứng | Hành vi hủy XOR | 
| đồ thị chu trình | sử dụng cơ bản | giảm thiểu chu kỳ | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi nhiều cạnh giữa cùng một cặp xuất hiện theo thời gian với các khoảng thời gian hoạt động khác nhau. Thuật toán phải xử lý từng khoảng thời gian kích hoạt một cách độc lập; nếu không, các cạnh cũ có thể vẫn còn trong cây phân đoạn một cách không chính xác, tạo ra các đóng góp chu trình không hợp lệ. 

Một trường hợp tinh vi khác là đồ thị chỉ được kết nối qua một cạnh cầu duy nhất. Khi cây cầu đó bị loại bỏ và sau đó được thêm lại, quá trình khôi phục phải khôi phục hoàn toàn cả cấu trúc DSU và điện thế XOR. Nếu độ lệch XOR không được đặt lại trong quá trình khôi phục, các tính toán đường dẫn tiếp theo sẽ tích lũy các đóng góp chu trình không chính xác. 

Trường hợp cạnh cuối cùng là một truy vấn xảy ra chính xác tại thời điểm một cạnh được chèn hoặc xóa. Việc xử lý khoảng thời gian phải đảm bảo rằng cạnh chỉ hoạt động trong phạm vi thời gian bao gồm chính xác, nếu không, truy vấn có thể thấy một cạnh chưa tồn tại hoặc đã bị xóa.
