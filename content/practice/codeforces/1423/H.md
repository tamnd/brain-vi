---
title: "CF 1423H - Virus"
description: "Chúng tôi đang duy trì một mạng lưới liên lạc xã hội ngày càng phát triển, nơi các ranh giới được tạo ra từng ngày. Mỗi cạnh thể hiện một cuộc gặp gỡ giữa hai người và những cuộc gặp gỡ này chỉ phù hợp trong một khoảng thời gian cố định có độ dài $k$ ngày."
date: "2026-06-11T06:12:18+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1423
codeforces_index: "H"
codeforces_contest_name: "Bubble Cup 13 - Finals [Online Mirror, unrated, Div. 1]"
rating: 2500
weight: 1423
solve_time_s: 111
verified: false
draft: false
---

[CF 1423H - Virus](https://codeforces.com/problemset/problem/1423/H) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục, dsu, đồ thị 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mạng lưới liên lạc xã hội ngày càng phát triển, nơi các ranh giới được tạo ra từng ngày. Mỗi cạnh đại diện cho một cuộc gặp gỡ giữa hai người và những cuộc gặp gỡ này chỉ phù hợp với một khoảng thời gian cố định$k$ngày. Bất kỳ cuộc họp nào cũ hơn khoảng thời gian đó sẽ ngừng góp phần vào “mức độ liên quan đến lây nhiễm”. 

Tại bất kỳ thời điểm nào, “tập hợp đáng ngờ” của một người được định nghĩa là tất cả các cá nhân được kết nối với họ thông qua bất kỳ chuỗi cuộc họp nào diễn ra trong vòng gần đây nhất.$k$ngày. Đây không chỉ là liên hệ trực tiếp mà còn là kết nối trong biểu đồ do cửa sổ thời gian hoạt động tạo ra. Mỗi khi chúng ta được hỏi về một người$z$, chúng ta cần báo cáo kích thước của thành phần được kết nối của chúng, bao gồm cả chính chúng, trong biểu đồ được tạo bởi tất cả các cạnh từ cạnh cuối cùng$k$ngày. 

Hệ thống phát triển theo ba cách: chúng tôi thêm một cạnh vô hướng khi hai người gặp nhau, chúng tôi nâng cao bộ đếm ngày và chúng tôi truy vấn kích thước kết nối cho một nút. 

Những ràng buộc buộc chúng tôi vào một chế độ rất chặt chẽ. Với tối đa$5 \times 10^5$hoạt động, bất kỳ giải pháp nào tính toán lại kết nối từ đầu cho mỗi truy vấn đều không khả thi ngay lập tức. Ngay cả việc duy trì danh sách kề và chạy BFS hoặc DFS cho mỗi truy vấn cũng sẽ thoái hóa thành$O(n + q)$cho mỗi truy vấn, vượt xa giới hạn. Cấu trúc cũng động với cả việc chèn và xóa các cạnh (vì các cạnh cũ hết hạn), điều này loại trừ trực tiếp DSU tĩnh. 

Cạm bẫy nguy hiểm nhất là quên rằng các cạnh hết hạn theo độ tuổi chứ không phải theo số lượng. Một DSU đơn giản chỉ có các cạnh hợp nhất sẽ bị tính quá mức vì nó không bao giờ loại bỏ các kết nối cũ. Một vấn đề tế nhị khác là khả năng kết nối không phải là “ảnh chụp nhanh mỗi ngày” mà qua cửa sổ trượt, vì vậy chúng tôi cần một cấu trúc nhận biết thời gian. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ duy trì danh sách đầy đủ các cạnh hoạt động tại mỗi thời điểm truy vấn. Đối với mỗi truy vấn loại 2, chúng tôi sẽ xây dựng biểu đồ của tất cả các cạnh từ cạnh cuối cùng.$k$ngày và chạy BFS/DFS từ nút được truy vấn để đếm kích thước thành phần của nó. Mỗi chi phí BFS$O(n + m)$và trong trường hợp xấu nhất cả nút và cạnh đều có thể$10^5$hoặc nhiều hơn, lặp đi lặp lại đến$5 \times 10^5$lần, dẫn đến khoảng$10^{10}$hoạt động. 

Quan sát cấu trúc quan trọng là khả năng kết nối chỉ thay đổi ở hai loại sự kiện: khi một cạnh mới được thêm vào và khi một cạnh cũ hết hạn sau đó.$k$ngày. Nếu chúng tôi có thể duy trì kết nối theo cả thao tác chèn và xóa thì chúng tôi đã hoàn thành. DSU tiêu chuẩn chỉ hỗ trợ chèn hiệu quả chứ không hỗ trợ xóa. 

Cách giải quyết cổ điển là xử lý thời gian ngoại tuyến bằng cách sử dụng phương pháp chia để trị theo thời gian kết hợp với DSU hỗ trợ khôi phục. Mỗi cạnh tồn tại trong một khoảng thời gian liền kề$[t, t+k)$, do đó mỗi cạnh có thể được chèn vào và loại bỏ vào những thời điểm xác định. Nếu chúng ta xử lý cây phân đoạn theo thời gian, mỗi nút sẽ lưu trữ các cạnh hoạt động trong khoảng thời gian đó. Chúng tôi áp dụng đệ quy các kết hợp khi nhập một phân đoạn và khôi phục chúng khi thoát ra, đảm bảo tính chính xác cho tất cả các truy vấn trong phân đoạn đó. 

Điều này biến vấn đề kết nối động thành vấn đề gán khoảng tĩnh với các hoạt động hợp có thể đảo ngược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force cho mỗi truy vấn |$O(q(n+m))$|$O(n+m)$| Quá chậm | 
| Cây phân đoạn + khôi phục DSU |$O((n+q)\alpha(n)\log q)$|$O(n+q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi thời gian thành một chuỗi các sự kiện tuyến tính. Mỗi lần gặp nhau$t$tạo ra một cạnh luôn hoạt động từ$t$ĐẾN$t+k-1$. Chúng tôi coi mỗi ngày là một khối hoạt động. 

1. Gán dấu thời gian cho tất cả các thao tác, chỉ tăng thời gian khi thao tác “trước ngày” xuất hiện. Mỗi cạnh có thời gian bắt đầu và thời gian hết hạn chính xác$k$vài ngày sau. Bước này là cần thiết vì các cạnh không cố định và phải được ánh xạ tới các khoảng. 
2. Đối với mỗi cạnh họp$(x, y)$xảy ra vào thời điểm$t$, lưu trữ nó dưới dạng hoạt động trong khoảng thời gian$[t, t+k)$. Chúng tôi không chèn nó ngay vào DSU vĩnh viễn vì sau này nó sẽ hết hạn. 
3. Xây dựng cây phân đoạn theo trục thời gian của tất cả các truy vấn. Mỗi khoảng cạnh được chèn vào tất cả các nút cây phân đoạn bao phủ đầy đủ phạm vi hoạt động của nó. Điều này đảm bảo mỗi cạnh được xử lý chính xác ở nơi có liên quan. 
4. Chuẩn bị cấu trúc DSU hỗ trợ khôi phục. Cùng với mảng gốc và mảng kích thước, chúng tôi duy trì một ngăn xếp ghi lại mọi thay đổi được thực hiện trong quá trình hoạt động hợp. Điều này cho phép chúng tôi hoàn tác việc hợp nhất sau khi rời khỏi một phân đoạn. 
5. Duyệt cây đoạn theo cách đệ quy. Khi nhập một nút, áp dụng tất cả các phép hợp cho các cạnh được lưu trữ trong nút đó. Điều này xây dựng trạng thái kết nối chính xác cho phân đoạn thời gian đó. 
6. Nếu nút tương ứng với một điểm thời gian duy nhất, hãy trả lời tất cả các truy vấn loại 2 bằng cách sử dụng kích thước thành phần DSU của nút được truy vấn. 
7. Sau khi hoàn thành một nút, khôi phục tất cả các thay đổi DSU được thực hiện trong nút đó trước khi quay lại nút cha. Điều này đảm bảo các phân đoạn anh chị em không can thiệp. 

### Tại sao nó hoạt động 

Tại bất kỳ nút cây phân đoạn nào, chúng tôi tạm thời áp dụng chính xác các cạnh có toàn bộ thời gian hoạt động bao phủ phân đoạn đó. Do đó, trạng thái DSU trong quá trình đệ quy đó biểu thị chính xác đồ thị được tạo ra bởi các cạnh hợp lệ cho mọi thời điểm trong khoảng đó. Bởi vì mọi cạnh chỉ được chèn trong các phân đoạn chứa đầy đủ trong khoảng hoạt động của nó, nên không có cạnh nào được áp dụng bên ngoài cửa sổ hiệu lực của nó. Khôi phục đảm bảo tính độc lập giữa các phân đoạn, vì vậy mọi truy vấn đều thấy chính xác tập hợp các cạnh hoạt động chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class DSURollback:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)
        self.history = []

    def find(self, x):
        while self.parent[x] != x:
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            self.history.append((-1, -1, -1))
            return
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.history.append((b, self.parent[b], a))
        self.parent[b] = a
        self.size[a] += self.size[b]

    def snapshot(self):
        return len(self.history)

    def rollback(self, snap):
        while len(self.history) > snap:
            b, old_parent, a = self.history.pop()
            if b == -1:
                continue
            self.parent[b] = old_parent
            self.size[a] -= self.size[b]

def solve():
    n, q, k = map(int, input().split())

    events = []
    queries_at = {}
    edges = []

    time = 0

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])
        if t == 1:
            x, y = map(int, tmp[1:])
            edges.append((time, x, y))
        elif t == 2:
            z = int(tmp[1])
            queries_at.setdefault(time, []).append((len(events), z))
            events.append((0, 0, 0))
        else:
            time += 1

    max_time = time + k + 2

    seg = [[] for _ in range(4 * max_time)]

    def add(l, r, edge, idx=1, nl=0, nr=max_time):
        if r <= nl or nr <= l:
            return
        if l <= nl and nr <= r:
            seg[idx].append(edge)
            return
        mid = (nl + nr) // 2
        add(l, r, edge, idx * 2, nl, mid)
        add(l, r, edge, idx * 2 + 1, mid, nr)

    for t, x, y in edges:
        add(t, t + k, (x, y))

    dsu = DSURollback(n)
    ans = [0] * len(events)

    def dfs(idx, l, r):
        snap = dsu.snapshot()
        for x, y in seg[idx]:
            dsu.union(x, y)

        if r - l == 1:
            if l in queries_at:
                for qi, z in queries_at[l]:
                    ans[qi] = dsu.size[dsu.find(z)]
        else:
            mid = (l + r) // 2
            dfs(idx * 2, l, mid)
            dfs(idx * 2 + 1, mid, r)

        dsu.rollback(snap)

    dfs(1, 0, max_time)

    return "\n".join(map(str, ans))

if __name__ == "__main__":
    print(solve())
```Cốt lõi của việc triển khai là ánh xạ từng cuộc họp thành một khoảng thời gian và chèn khoảng thời gian đó vào cây phân đoạn. DSU không bao giờ xóa vĩnh viễn các cạnh, nó chỉ tạm thời áp dụng chúng trong khung đệ quy và khôi phục lại sau đó. 

Một điểm tinh tế là chúng tôi tránh nén đường dẫn trong`find`. Điều này là cần thiết vì việc nén đường dẫn là không thể đảo ngược và sẽ phá vỡ tính chính xác của quá trình khôi phục. Thay vào đó, chúng tôi sử dụng quá trình truyền tải chuỗi gốc đơn giản, dựa vào sự kết hợp theo quy mô để đạt được hiệu quả. 

Một khía cạnh tế nhị khác là cách lưu trữ kết quả truy vấn. Mỗi truy vấn được lập chỉ mục theo thứ tự xuất hiện vì nhiều truy vấn có thể xảy ra cùng một lúc. Chúng tôi lưu trữ các câu trả lời trong một mảng được căn chỉnh theo chỉ mục truy vấn để duy trì thứ tự đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 12 1
1 1 2
1 1 3
1 3 4
2 4
2 5
3
2 1
1 1 2
1 3 2
2 1
3
2 1
```Chúng tôi theo dõi các cạnh hoạt động theo thời gian. Với$k=1$, chỉ các cạnh của ngày hiện tại mới quan trọng. 

| Thời gian | Đã thêm cạnh | Các cạnh hoạt động | Truy vấn | Kết quả thành phần | 
| --- | --- | --- | --- | --- | 
| 0 | (1,2) | (1,2) | - | - | 
| 0 | (1,3) | (1,2),(1,3) | - | - | 
| 0 | (3,4) | (1,2),(1,3),(3,4) | truy vấn 4 | 4 | 
| 0 | - | giống nhau | truy vấn 5 | 1 | 

Điều này cho thấy ở truy vấn đầu tiên, nút 4 được kết nối qua 3 và 1 đến 2, tạo thành thành phần có kích thước 4. Truy vấn thứ hai được tách biệt. 

Các quá trình chuyển đổi ngày sau đó sẽ thiết lập lại các cạnh hoạt động, do đó các thành phần co lại, điều này xác nhận rằng thời gian hết hạn đang được tôn trọng. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
4 8 2
1 1 2
1 2 3
3
2 1
3
2 3
1 3 4
2 4
```Chúng tôi quan sát xem các cạnh tồn tại như thế nào trong hai ngày, nghĩa là kết nối bắc cầu có thể tồn tại qua các ranh giới ngày. 

| Bước | Các cạnh hoạt động | Truy vấn | Kết quả | 
| --- | --- | --- | --- | 
| sau ngày 0 | (1,2),(2,3) | truy vấn 1 | 3 | 
| sau ngày 1 | (1,2),(2,3) | truy vấn 3 | 3 | 
| sau khi cộng (3,4) | (1,2),(2,3),(3,4) | truy vấn 4 | 4 | 

Hành vi chính là tính bền bỉ: mặc dù các cạnh không phải là vĩnh viễn, sự chồng chéo qua các ngày vẫn duy trì chuỗi kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log q \cdot \alpha(n))$| Mỗi cạnh được chèn vào$O(\log q)$các nút cây phân đoạn, mỗi liên kết được khấu hao gần như không đổi | 
| Không gian |$O(n + q \log q)$| Mảng DSU cộng với lưu trữ cây phân đoạn của các khoảng cạnh | 

Hệ số logarit xuất phát từ việc phân tách cây phân đoạn của từng khoảng hoạt động. Với tối đa$5 \times 10^5$hoạt động, điều này vẫn nằm trong giới hạn thoải mái và việc khôi phục DSU đảm bảo mỗi liên kết có thể đảo ngược mà không cần tính toán lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("""5 12 1
1 1 2
1 1 3
1 3 4
2 4
2 5
3
2 1
1 1 2
1 3 2
2 1
3
2 1
""") == "4\n1\n1\n3\n1"

# minimum case
assert run("""1 2 1
2 1
2 1
""") == "1\n1"

# chain connectivity
assert run("""4 6 2
1 1 2
1 2 3
2 1
3
2 3
""") == "3\n3"

# all isolated
assert run("""3 4 1
2 1
2 2
2 3
2 1
""") == "1\n1\n1\n1"

# full merge then expiry
assert run("""3 5 1
1 1 2
1 2 3
3
2 1
2 3
""") == "3\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn nút đơn | 1,1 | kết nối tầm thường | 
| chuỗi | 3,3 | đóng cửa chuyển tiếp | 
| các nút bị cô lập | tất cả 1 | trường hợp không có cạnh | 
| hành vi hết hạn | 3 rồi 1 | cửa sổ trượt đúng cách | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi tất cả các cạnh xảy ra trong một ngày nhưng trải rộng trên nhiều truy vấn. Một DSU ngây thơ sẽ hợp nhất vĩnh viễn các nút, nhưng ở đây các thành phần phải co lại sau khi hết thời gian. Cách tiếp cận cây phân đoạn xử lý vấn đề này bằng cách chỉ gắn các cạnh vào khoảng thời gian hợp lệ của chúng. 

Một trường hợp tinh tế khác là nhiều truy vấn tại cùng một dấu thời gian. Vì các truy vấn được lập chỉ mục thay vì được xử lý ngay lập tức nên chúng tôi đảm bảo tất cả các trạng thái DSU trong khoảng thời gian đó đều giống hệt nhau trước khi trả lời chúng, ngăn chặn sự phụ thuộc vào thứ tự. 

Trường hợp góc cuối cùng là khi$k = 0$, điều này làm cho tất cả các cạnh ngay lập tức hết hạn. Việc xây dựng khoảng cách vẫn hoạt động vì mỗi cạnh được chèn vào$[t, t)$, không đóng góp hiệu quả gì và mọi truy vấn đều trả về chính xác các nút bị cô lập.
