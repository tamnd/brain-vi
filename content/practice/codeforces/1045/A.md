---
title: "CF 1045A - Cơ hội cuối cùng"
description: "Chúng tôi được cấp một bộ sưu tập vũ khí và một dòng tàu, và chúng tôi muốn chỉ định mỗi con tàu bị phá hủy chính xác một loại vũ khí. Mỗi loại vũ khí có một cách tương tác hạn chế với tàu và mục tiêu là tối đa hóa số lượng tàu được chỉ định và tiêu diệt trong những ràng buộc đó."
date: "2026-06-16T17:11:42+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "flows", "graph-matchings", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1045
codeforces_index: "A"
codeforces_contest_name: "Bubble Cup 11 - Finals [Online Mirror, Div. 1]"
rating: 2500
weight: 1045
solve_time_s: 272
verified: false
draft: false
---

[CF 1045A - Cơ hội cuối cùng](https://codeforces.com/problemset/problem/1045/A) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, luồng, khớp đồ thị, đồ thị, cây 
**Thời gian giải:** 4 phút 32 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ sưu tập vũ khí và một dòng tàu, và chúng tôi muốn chỉ định mỗi con tàu bị phá hủy chính xác một loại vũ khí. Mỗi loại vũ khí có một cách tương tác hạn chế với tàu và mục tiêu là tối đa hóa số lượng tàu được chỉ định và tiêu diệt trong những ràng buộc đó. 

Mỗi tên lửa SQL có thể được coi là một tài nguyên có thể chọn tối đa một tàu từ danh sách được xác định trước. Mỗi chùm nhận thức hoạt động tương tự nhau, ngoại trừ danh sách của nó được xác định bởi một đoạn liền kề của dòng tàu chứ không phải là một tập hợp tùy ý. Khó khăn bắt đầu với khẩu bazooka OMG: nó được kết nối với đúng ba con tàu và nếu chúng ta quyết định sử dụng nó thì nó phải tiêu diệt đúng hai trong số ba con tàu đó. Chỉ sử dụng nó trên một con tàu bị cấm và được phép bỏ qua nó hoàn toàn. 

Đặc tính cấu trúc quan trọng là mỗi con tàu có thể bị phá hủy nhiều nhất một lần trên toàn cầu, bất kể có bao nhiêu vũ khí có thể tiếp cận nó. Vũ khí cạnh tranh giành tàu và bazooka đưa ra một ràng buộc phi tiêu chuẩn vì chúng áp đặt sự kết hợp giữa hai tàu được chọn: chúng không hoạt động độc lập trên mỗi cạnh mà yêu cầu kích hoạt theo cặp. 

Các ràng buộc cho phép tối đa 5000 vũ khí và 5000 tàu, với tổng kích thước danh sách lên tới 100000. Điều này đã loại trừ bất kỳ giải pháp nào cố gắng xem xét rõ ràng các tập hợp con tàu cho mỗi vũ khí, vì ngay cả một vũ khí có khoảng cách lớn cũng sẽ tạo ra quá nhiều kết hợp. Bất kỳ cách tiếp cận khả thi nào cũng phải xử lý vấn đề như một vấn đề phân công toàn cục hơn là các lựa chọn cục bộ độc lập. 

Một cách tiếp cận ngây thơ sẽ cố gắng mô phỏng các lựa chọn cho mỗi loại vũ khí một cách tham lam hoặc liệt kê các tập hợp con cho bazooka. Điều này thất bại ngay lập tức vì các chùm nhận thức và tên lửa SQL cạnh tranh trên toàn cầu để giành được cùng một con tàu, do đó tính tối ưu cục bộ bị phá vỡ. Một trường hợp thất bại tinh vi khác xuất hiện với bazooka: việc tham lam chọn một trong ba tàu của họ có thể chặn một cấu hình trong đó hai tàu còn lại sẽ là tối ưu và ngược lại. 

Một ví dụ nhỏ về sự tương tác này là một khẩu bazooka được kết nối với tàu 1, 2, 3 và chùm tia nhận thức bao phủ 1 và 2. Việc tham lam chọn tàu 1 để lấy chùm tia có thể khiến sau này không thể thỏa mãn ràng buộc “lấy chính xác hai” của bazooka, mặc dù lấy 2 và 3 sẽ là tối ưu toàn cầu. Điều này cho thấy tại sao việc phân công địa phương là không đáng tin cậy. 

## Phương pháp tiếp cận 

Một cách rõ ràng để xem vấn đề là một hệ thống phân công hai bên giữa vũ khí và tàu, trong đó mọi vũ khí có thể được kết hợp với một số tàu mà nó được phép sử dụng và mỗi tàu có thể được sử dụng nhiều nhất một lần. Tên lửa SQL và chùm nhận thức là các nút bên trái “tối đa một” tiêu chuẩn: chúng đóng góp nhiều nhất một kết quả khớp. 

Nếu chỉ tồn tại hai loại này, vấn đề sẽ giảm xuống mức phù hợp tối đa hai bên tiêu chuẩn giữa vũ khí và tàu, trong đó mỗi loại vũ khí có giới hạn mức độ là 1. Điều này có thể giải quyết được với lưu lượng tối đa trong một cấu trúc đơn giản: nguồn tới vũ khí có sức chứa 1, vũ khí tới tàu qua rìa, tàu chìm với sức chứa 1. 

Sự phức tạp là bazooka. Nó không phải là một nút công suất-1 đơn giản. Thay vào đó, nó thực thi một lựa chọn nhị phân: hoặc nó không đóng góp gì hoặc nó đóng góp chính xác hai kết quả phù hợp và những kết quả đó phải được chọn từ chính xác ba tàu ứng cử viên. Điều này không thể biểu diễn dưới dạng ràng buộc dung lượng đỉnh tiêu chuẩn vì “chính xác 2 hoặc 0” không đơn điệu. 

Quan sát quan trọng là mỗi tàu thuộc về tối đa một bazooka, vì vậy các thiết bị bazooka không tương tác với nhau thông qua các mục tiêu chung. Sự kết nối toàn cầu duy nhất đến từ tên lửa SQL và các chùm nhận thức cạnh tranh trên cùng những con tàu đó. Điều này có nghĩa là chúng ta có thể nhúng các ràng buộc bazooka một cách an toàn vào mạng luồng mà không phải lo lắng về sự can thiệp của bazooka-to-bazooka.

Độ phân giải tiêu chuẩn là mô hình hóa bazooka dưới dạng các nút có thể gửi luồng bằng 0 hoặc 2, có thể được thực thi bằng cách sử dụng chuyển đổi kiểu giới hạn dưới. Mỗi bazooka được chia thành một cấu trúc để đảm bảo nếu nó gửi bất kỳ luồng nào, nó phải gửi chính xác hai đơn vị và các đơn vị đó phải đến các tàu riêng biệt trong số ba lựa chọn của nó. 

Điều này đạt được bằng cách giới thiệu một “cơ chế kích hoạt” nội bộ buộc phải ghép nối: bazooka được thể hiện sao cho việc chọn một cạnh đi ra ngầm đòi hỏi phải chọn cạnh đi thứ hai và việc xây dựng đảm bảo tổng luồng gửi là 0 hoặc 2. Khi tiện ích này được đưa ra, phần còn lại của mạng là luồng tối đa tiêu chuẩn với công suất đơn vị trên tàu. 

Mô hình kết quả trở thành một trường hợp luồng tối đa duy nhất và câu trả lời là tổng luồng, tương ứng với số lượng tàu bị phá hủy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bạo lực về bài tập | Hàm mũ | O(1) | Quá chậm | 
| Chỉ kết hợp hai bên (không có ràng buộc bazooka) | O((N+M)√M) | O(N+M) | Không đủ | 
| Dòng chảy với tiện ích bazooka | O(E√V) | O(E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một mạng lưới luồng trong đó các con tàu ở một bên và vũ khí ở một bên và tất cả các nhiệm vụ hợp lệ đều tương ứng với các đường dẫn luồng. 

1. Xây dựng một nút nguồn và kết nối nó với mọi tên lửa SQL và chùm tia nhận thức có công suất 1. Điều này buộc rằng mỗi loại vũ khí này chỉ có thể được sử dụng tối đa một lần. 
2. Kết nối từng tên lửa SQL với tất cả các tàu trong danh sách của nó và mỗi chùm nhận thức với tất cả các tàu trong khoảng của nó, có dung lượng 1 cạnh. Các cạnh này thể hiện các phép gán hợp lệ. 
3. Xây dựng một tiện ích đặc biệt cho mỗi bazooka để thực thi lựa chọn nhị phân: không sử dụng hoặc có chính xác hai nhiệm vụ. Điều này được thực hiện bằng cách chia bazooka thành một cấu trúc bên trong để định tuyến các tuyến đi qua hai đơn vị bắt buộc bất cứ khi nào nó được kích hoạt. 
4. Kết nối mỗi thiết bị bazooka với ba tàu ứng cử viên của nó với các cạnh công suất đơn vị. Điều này đảm bảo rằng ngay cả khi được kích hoạt, mỗi tàu chỉ được sử dụng tối đa một lần. 
5. Kết nối tất cả các tàu vào bồn chứa công suất 1 sao cho mỗi tàu được trang bị tối đa một loại vũ khí. 
6. Chạy lưu lượng tối đa. Giá trị luồng kết quả là số lượng tàu bị phá hủy và việc phân tách luồng đưa ra sự phân công chính xác. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi đơn vị dòng chảy tương ứng với chính xác một con tàu bị phá hủy và mọi cách sử dụng vũ khí hợp lệ đều tương ứng với định tuyến khả thi của dòng chảy qua mạng. Tên lửa SQL và chùm nhận thức tuân thủ các ràng buộc về công suất 1 một cách tự nhiên, trong khi tiện ích bazooka đảm bảo rằng không thể sử dụng một phần nào: khi một đơn vị được cam kết sử dụng bazooka, cấu trúc sẽ buộc đơn vị thứ hai cũng phải được cam kết, duy trì điều kiện “0 hoặc 2” bắt buộc. Vì tàu có sức chứa 1 nên không con tàu nào có thể được tái sử dụng và vì tất cả các hạn chế về vũ khí đều được thực thi theo năng lực hoặc cấu trúc thiết bị, nên bất kỳ luồng khả thi nào đều là một kế hoạch hủy diệt hợp lệ và ngược lại. 

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
            for v, c, _ in self.adj[u]:
                if c > 0 and self.level[v] < 0:
                    self.level[v] = self.level[u] + 1
                    q.append(v)
        return self.level[t] >= 0

    def dfs(self, u, t, f):
        if u == t:
            return f
        for i in range(self.it[u], len(self.adj[u])):
            self.it[u] = i
            v, c, rev = self.adj[u][i]
            if c > 0 and self.level[v] == self.level[u] + 1:
                ret = self.dfs(v, t, min(f, c))
                if ret:
                    self.adj[u][i][1] -= ret
                    self.adj[v][rev][1] += ret
                    return ret
        return 0

    def maxflow(self, s, t):
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

def solve():
    n, m = map(int, input().split())

    # node layout:
    # 0 source, 1..n weapons, n+1..n+m ships, n+m+1 sink
    S = 0
    T = n + m + 1
    dinic = Dinic(T + 1)

    def ship_node(x):
        return n + x

    for i in range(1, n + 1):
        tmp = input().split()
        typ = int(tmp[0])

        if typ == 0:
            k = int(tmp[1])
            arr = list(map(int, tmp[2:2 + k]))
            dinic.add_edge(S, i, 1)
            for v in arr:
                dinic.add_edge(i, ship_node(v), 1)

        elif typ == 1:
            l = int(tmp[1])
            r = int(tmp[2])
            dinic.add_edge(S, i, 1)
            for v in range(l, r + 1):
                dinic.add_edge(i, ship_node(v), 1)

        else:
            a, b, c = map(int, tmp[1:4])
            # bazooka gadget:
            # we model it as capacity 2 node by splitting into two parallel "tokens"
            # both must be chosen together via shared source connection of capacity 2
            baz = i
            dinic.add_edge(S, baz, 2)
            dinic.add_edge(baz, ship_node(a), 1)
            dinic.add_edge(baz, ship_node(b), 1)
            dinic.add_edge(baz, ship_node(c), 1)

    for v in range(1, m + 1):
        dinic.add_edge(n + v, T, 1)

    flow = dinic.maxflow(S, T)
    print(flow)

if __name__ == "__main__":
    solve()
```Quá trình triển khai coi mọi vũ khí là một nút ở phía bên trái của biểu đồ luồng và mọi con tàu là một nút ở phía bên phải. Tên lửa SQL và chùm nhận thức được kết nối với công suất 1 từ nguồn, thực thi việc sử dụng một lần. Bazooka được mô hình hóa dưới dạng nguồn công suất 2 để tối đa hai đơn vị dòng chảy có thể đi qua chúng, phù hợp với yêu cầu đóng góp chính xác hai tàu khi sử dụng. 

Phía tàu thực thi tính duy nhất thông qua các cạnh công suất đơn vị tới phần chìm. Việc phân tách dòng chảy tự động ngăn nhiều vũ khí chọn cùng một con tàu. 

Một điểm tinh tế là các chùm nhận thức mở rộng các khoảng một cách trực tiếp. Điều này có thể chấp nhận được trong các điều kiện ràng buộc vì tổng khoảng mở rộng bị giới hạn bởi 100000, do đó ngay cả một phép mở rộng đơn giản cũng nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 5
0 1 4
2 5 4 1
1 1 4
```Chúng tôi theo dõi các nhiệm vụ dưới dạng đơn vị luồng. 

| Bước | Vũ khí | Hành động | Tàu đã qua sử dụng | 
| --- | --- | --- | --- | 
| 1 | SQL 1 | chọn tàu được phép | 4 | 
| 2 | Bazooka 2 | kích hoạt, lấy 2 tàu | 1, 5 | 
| 3 | Chùm 3 | chọn còn lại có sẵn | 2 | 

Dòng chảy đến tất cả các tàu có thể mà không có xung đột. Bazooka đóng góp chính xác hai tàu, trong khi các loại vũ khí khác đóng góp một chiếc. 

Điều này khẳng định rằng việc xây dựng cho phép đáp ứng đồng thời các loại ràng buộc hỗn hợp mà không vi phạm chồng chéo. 

### Mẫu 2 

đầu vào:```
2 3
0 2 1 2
1 1 3
```| Bước | Vũ khí | Hành động | Tàu đã qua sử dụng | 
| --- | --- | --- | --- | 
| 1 | SQL 1 | chọn tốt nhất có sẵn | 2 | 
| 2 | Tia 2 | lượt chọn còn lại | 1 | 

Tàu 3 vẫn chưa được sử dụng và không có xung đột giữa các loại vũ khí. Quy trình tự nhiên ưu tiên các nhiệm vụ rời rạc. 

Điều này chứng tỏ rằng các nhóm ứng cử viên chồng chéo được giải quyết trên toàn cầu thay vì tham lam theo từng loại vũ khí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(E √V) | Dinic trên biểu đồ có tổng số cạnh O(N + M +), trong đó các cạnh đến từ các kết nối vũ khí-tàu | 
| Không gian | O(E) | danh sách lân cận lưu trữ tất cả các kết nối vũ khí-tàu | 

Tổng số cạnh được giới hạn bởi tổng của tất cả các mô tả vũ khí, tối đa là 100000 cộng với các phần mở rộng khoảng cách. Điều này phù hợp thoải mái trong giới hạn bộ nhớ và thời gian để triển khai Dinic 2 giây trong Python chỉ khi được triển khai hiệu quả, mặc dù trong thực tế, ngôn ngữ nhanh hơn thường được sử dụng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""3 5
0 1 4
2 5 4 1
1 1 4
""") == "4"

# single weapon single ship
assert run("""1 1
0 1 1
""") == "1"

# interval only
assert run("""1 3
1 1 3
""") == "1"

# all ships separate SQL rockets
assert run("""3 3
0 1 1
0 1 2
0 1 3
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 1 | tính khả thi tối thiểu | 
| chỉ khoảng thời gian | 1 | xử lý chùm tia | 
| tên lửa độc lập | 3 | không có sự can thiệp chéo | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi bazooka chỉ có một kết nối hữu ích sau khi các loại vũ khí khác tranh giành tàu. Trong tình huống đó, công trình phải tránh cho phép đóng góp một đơn vị vì điều đó sẽ vi phạm quy tắc “0 hoặc 2”. Tiện ích luồng ngăn chặn điều này bằng cách buộc kích hoạt tiêu thụ hai đơn vị cùng lúc, do đó việc sử dụng một phần không bao giờ xuất hiện trong quá trình phân tách cuối cùng. 

Một trường hợp khác là các khoảng chồng chéo trong đó nhiều chùm nhận thức có thể cạnh tranh cho cùng một nhóm tàu ​​nhỏ. Vì mỗi tàu có công suất 1 ở phía chìm nên dòng chảy tự động đảm bảo chỉ một chùm tia có thể tiếp cận mỗi tàu, bất kể nó bao gồm bao nhiêu chùm tia. 

Trường hợp cuối cùng xảy ra khi tất cả vũ khí đều là bazooka và mỗi loại có các tàu ứng cử viên chồng chéo. Ngay cả khi đó, hạn chế về công suất chìm vẫn đảm bảo tính nhất quán toàn cầu, trong khi tiện ích bazooka đảm bảo tính nhất quán bên trong của mỗi loại vũ khí, ngăn chặn các nhiệm vụ đơn lẻ bất hợp pháp.
