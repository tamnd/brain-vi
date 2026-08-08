---
title: "CF 102566A - Người ăn xin"
description: "Chúng ta được cung cấp một tập hợp các đoàn tàu, trong đó mỗi đoàn tàu có mặt tại ga trong một khoảng thời gian liên tục. Một người ăn xin phải dành toàn bộ thời gian làm việc từ lúc 0 đến lúc d trên tàu."
date: "2026-08-07T21:32:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102566
codeforces_index: "A"
codeforces_contest_name: "AGM 2020, Qualification Round"
rating: 0
weight: 102566
solve_time_s: 56
verified: true
draft: false
---

[CF 102566A - Người ăn xin](https://codeforces.com/problemset/problem/102566/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đoàn tàu, trong đó mỗi đoàn tàu có mặt tại ga trong một khoảng thời gian liên tục. Một người ăn xin phải dành toàn bộ thời gian làm việc từ lúc 0 đến lúc d trên tàu. Một người ăn xin có thể di chuyển từ chuyến tàu này sang chuyến tàu khác ngay lập tức khi chuyến tàu này khởi hành và chuyến tàu khác đến, vì vậy lịch trình của người ăn xin là một chuỗi các chuyến tàu trong đó điểm cuối của một khoảng thời gian trùng với thời điểm bắt đầu của chuyến tiếp theo. 

Mục tiêu là tối đa hóa số lượng người ăn xin có thể làm việc cùng một lúc. Hai người ăn xin không thể đi cùng một chuyến tàu và họ không thể chuyển tàu cùng một lúc vì điều đó có nghĩa là họ gặp nhau ở sân ga. Điều này có nghĩa là mỗi chuyến tàu có thể thuộc về nhiều nhất một người ăn xin và những người ăn xin khác nhau phải sử dụng các chuỗi tàu hoàn toàn riêng biệt. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra độ dài của ngày làm việc d và danh sách n khoảng thời gian chạy tàu. Đầu ra là số lượng lớn nhất các lịch trình hoàn chỉnh từ thời điểm 0 đến thời điểm d có thể được hình thành mà không có xung đột. 

Giá trị nhỏ của d là hạn chế chính định hình giải pháp. Có tối đa 200 điểm thời gian khác nhau, trong khi số lượng tàu có thể lên tới 20000. Bất kỳ phương pháp nào thử tất cả các nhóm tàu ​​có thể đều không thể thực hiện được vì số lượng kết hợp tăng theo cấp số nhân. Ngay cả việc kiểm tra riêng nhiều lịch trình có thể cũng sẽ quá chậm khi n lớn. Kích thước thời gian đủ nhỏ để việc xây dựng biểu đồ theo các điểm thời gian là thực tế. 

Một vài chi tiết có thể phá vỡ việc triển khai ngây thơ. Hãy xem xét một chuyến tàu từ 0 đến 5.```
1
5 1
0 5
```Câu trả lời là 1 vì một người ăn xin có thể sử dụng chuyến tàu đó trong suốt thời gian đó. Một cách tiếp cận chỉ tính số lần chuyển tiếp giữa các chuyến tàu có thể trả về 0 không chính xác do không có công tắc. 

Một trường hợp quan trọng khác là có nhiều đoàn tàu giống hệt nhau.```
1
3 3
0 3
0 3
0 3
```Câu trả lời là 3. Ba người ăn xin, mỗi người có thể ở trên một chuyến tàu khác nhau. Việc coi các khoảng thời gian giống hệt nhau là trùng lặp và chỉ giữ lại một khoảng thời gian sẽ làm mất lịch trình hợp lệ. 

Trường hợp thứ ba là sự chồng chéo lớn ở thời điểm trung gian.```
1
4 4
0 2
0 2
2 4
2 4
```Câu trả lời là 2. Hai người ăn xin có thể độc lập đi theo hai con đường hoàn chỉnh. Một cách tiếp cận tham lam luôn chọn con tàu có vẻ ngoài dài nhất có thể vô tình tiêu tốn một con tàu cần thiết cho một con đường hợp lệ khác. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là cố gắng xây dựng mọi lịch trình ăn xin có thể có. Lịch trình là đường đi qua các đoàn tàu, bắt đầu với một đoàn tàu xuất phát lúc 0 và kết thúc khi một đoàn tàu kết thúc tại thời điểm d. Sau khi tìm thấy một lịch trình, chúng tôi có thể xóa các chuyến tàu của nó và tìm kiếm lại. Điều này đúng nếu mọi lựa chọn có thể được khám phá, bởi vì định nghĩa của một giải pháp hợp lệ chính xác là một tập hợp các đường dẫn không chồng chéo. Vấn đề là số lượng đường dẫn có thể là rất lớn. Với 20000 đoàn tàu, số lượng kết hợp có thể có thể tăng theo cấp số nhân, khiến phương pháp này không thể sử dụng được. 

Cấu trúc của bài toán gợi ý nên nhìn vào các thời điểm thay vì nhìn vào từng người ăn xin. Mỗi khoảng thời gian của đoàn tàu có thể được xem như một cạnh được định hướng từ thời điểm bắt đầu đến thời điểm kết thúc. Toàn bộ hành trình của người ăn xin trở thành một con đường từ nút 0 đến nút d. Vì hai người ăn xin không thể đi chung một chuyến tàu nên mỗi cạnh đều có một năng lực. Vì chúng không thể gặp nhau khi chuyển mạch nên nhiều đường đi không thể chia sẻ các điểm thời gian trung gian. Đây chính xác là vấn đề về luồng tối đa trong đó cả việc sử dụng tàu và chuyển đổi vị trí đều phải bị hạn chế. 

Để mô hình hóa hạn chế chuyển đổi, mỗi nút được chia thành hai nút. Cạnh đi vào điểm thời gian và cạnh rời khỏi điểm thời gian đó phải đi qua kết nối dung lượng một. Điều này chỉ cho phép một người ăn xin có mặt tại trạm đó vào thời điểm chuyển đổi. Ngoại lệ là thời gian 0 và thời gian d, trong đó bất kỳ số lượng người ăn xin nào cũng có thể bắt đầu hoặc kết thúc, vì vậy các nút này không cần hạn chế này. 

Biểu đồ kết quả chỉ có khoảng 400 nút vì d nhiều nhất là 200. Thuật toán của Dinic đủ nhanh, ngay cả khi có nhiều cạnh tàu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng tàu | O(n) | Quá chậm | 
| Lưu lượng tối đa trên biểu đồ thời gian | O(V²E) với Dinic | O(V + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo biểu đồ biểu thị các thời điểm. Mỗi giá trị thời gian từ 0 đến d sẽ trở thành một vị trí biểu đồ. Với mỗi khoảng thời gian đào tạo [x, y], hãy thêm một cạnh có hướng từ thời điểm x đến thời điểm y với dung lượng một. Cạnh này thể hiện thực tế là một người ăn xin có thể sử dụng chuyến tàu này. 
2. Chia mọi điểm thời gian trung gian t, trong đó 0 < t < d, thành một nút vào và một nút ra. Thêm một cạnh từ phía vào đến phía ra có dung lượng một. Cạnh này đại diện cho nền tảng tại thời điểm chính xác đó, ngăn cản hai người ăn xin chuyển đổi sang đó cùng một lúc. 
3. Kết nối các cạnh của đoàn tàu một cách chính xác với các nút phân chia. Một đoàn tàu khởi hành lúc t xuất phát từ phía đi của t, và một đoàn tàu đến lúc t kết thúc ở phía đến của t. Thời điểm bắt đầu và kết thúc không bị hạn chế vì bất kỳ số lượng người ăn xin nào cũng có thể bắt đầu tại thời điểm 0 và kết thúc tại thời điểm d. 
4. Chạy thuật toán luồng cực đại từ nguồn biểu thị thời gian 0 đến đích biểu thị thời gian d. Lượng luồng là số lượng lịch trình ăn xin độc lập tối đa. 

Lý do điều này có tác dụng là vì mỗi đơn vị dòng chảy tương ứng với một đường đi hoàn chỉnh xuyên qua các đoàn tàu. Các cạnh của đoàn tàu một năng lực ngăn hai đơn vị luồng đi cùng một đoàn tàu và các cạnh thời gian trung gian một sức chứa ngăn hai đơn vị luồng gặp nhau khi đổi tàu. Mọi tập hợp người ăn xin hợp lệ đều tạo ra một luồng chính xác như vậy và mọi luồng hợp lệ mô tả một tập hợp những người ăn xin hợp lệ. 

Điều bất biến là sau mỗi lần tăng cường thuật toán luồng, mỗi đơn vị luồng đã thể hiện một tập hợp lịch trình một phần không có xung đột. Do các đường dẫn tăng cường chỉ sử dụng dung lượng dư sẵn có nên luồng tối đa cuối cùng không thể chứa sự chồng lấp không hợp lệ. 

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
        q = [s]
        self.level[s] = 0
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
                pushed = self.dfs(v, t, min(f, c))
                if pushed:
                    e[1] -= pushed
                    self.g[v][rev][1] += pushed
                    return pushed
            self.it[u] += 1
        return 0

    def flow(self, s, t):
        ans = 0
        while self.bfs(s, t):
            self.it = [0] * self.n
            while True:
                pushed = self.dfs(s, t, 10**9)
                if not pushed:
                    break
                ans += pushed
        return ans

def solve():
    out = []
    T = int(input())
    for _ in range(T):
        d, n = map(int, input().split())

        def inn(x):
            return x * 2

        def outn(x):
            return x * 2 + 1

        size = 2 * (d + 1)
        dinic = Dinic(size)

        for t in range(1, d):
            dinic.add_edge(inn(t), outn(t), 1)

        for _ in range(n):
            x, y = map(int, input().split())
            start = outn(x) if x != d else inn(x)
            end = inn(y) if y != 0 else outn(y)
            dinic.add_edge(start, end, 1)

        source = outn(0)
        sink = inn(d)
        out.append(str(dinic.flow(source, sink)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng Dinic vì biểu đồ nhỏ và thưa thớt. Mỗi cạnh đồ thị lưu trữ đích đến, dung lượng còn lại và chỉ số của cạnh ngược. Cạnh ngược được yêu cầu bởi biểu đồ dư, cho phép thuật toán xem xét lại các lựa chọn trước đó. 

Việc tách nút được xử lý bằng cách gán hai chỉ số cho mỗi giá trị thời gian. Trong thời gian giữa t, cạnh từ`inn(t)`ĐẾN`outn(t)`có năng lực một. Các mép tàu rời khỏi phía đi của thời gian bắt đầu và đi vào phía đến của thời gian kết thúc. 

Nguồn là phía đi của thời điểm 0 vì người ăn xin có thể rời khỏi thời điểm bắt đầu mà không bị hạn chế. Bồn rửa là phía đến của thời gian d vì người ăn xin có thể hoàn thành mà không bị hạn chế. Không có cách xử lý riêng biệt nào cho các khoảng thời gian vì các cạnh của đoàn tàu đã thể hiện hành vi khoảng thời gian khép kín đầy đủ mà vấn đề yêu cầu. 

Số nguyên Python không bị tràn, vì vậy dung lượng và câu trả lời vẫn an toàn mặc dù luồng tối đa có thể lớn hơn một nhiều. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu:```
1
9 7
0 2
0 2
0 3
2 5
2 9
3 9
5 9
```Các trạng thái luồng quan trọng là: 

| Bước | Xem xét cạnh tàu | Đường dẫn tối đa hiện tại | 
| --- | --- | --- | 
| Ban đầu | Không có chuyến tàu nào được chọn | 0 | 
| Thêm đường dẫn 0 -> 2 -> 5 -> 9 | Một lịch trình hoàn chỉnh | 1 | 
| Thêm đường dẫn 0 -> 3 -> 9 | Lịch trình hoàn chỉnh thứ hai | 2 | 
| Hãy thử một con đường khác | Thời gian trung gian hoặc năng lực đào tạo chặn nó | 2 | 

Kết quả là 2. Dấu vết cho thấy lý do tại sao hai lịch trình có thể cùng tồn tại khi chúng sử dụng các thời điểm chuyển đổi khác nhau, trong khi lịch trình thứ ba yêu cầu chia sẻ tài nguyên bị hạn chế. 

Trường hợp biên với các đoàn tàu đi thẳng giống hệt nhau:```
1
5 3
0 5
0 5
0 5
```có hành vi dòng chảy sau đây: 

| Bước | Hành động | Dòng chảy | 
| --- | --- | --- | 
| Xây dựng đồ thị | Ba cạnh song song từ đầu đến cuối | 0 | 
| Lần tăng cường đầu tiên | Sử dụng chuyến tàu đầu tiên | 1 | 
| Tăng cường thứ hai | Sử dụng chuyến tàu thứ hai | 2 | 
| Tăng cường thứ ba | Sử dụng chuyến tàu thứ ba | 3 | 

Kết quả là 3. Điều này xác nhận rằng các cạnh của đoàn tàu là các tài nguyên độc lập và các khoảng giống hệt nhau không được hợp nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V²E) | Dinic chạy trên biểu đồ có tối đa khoảng 400 nút và khoảng 20000 cạnh tàu | 
| Không gian | O(V + E) | Biểu đồ lưu trữ mọi cạnh tàu và cạnh dư của nó | 

Phạm vi thời gian nhỏ làm cho biểu đồ luồng nhỏ mặc dù số lượng tàu lớn. Thuật toán phù hợp thoải mái với các giới hạn vì phần đắt tiền phụ thuộc vào số lượng nút thời gian hơn là số lượng lịch trình có thể có. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    t = int(next(it))
    ans = []

    class Dinic:
        def __init__(self, n):
            self.g = [[] for _ in range(n)]

        def add(self, u, v, c):
            self.g[u].append([v, c, len(self.g[v])])
            self.g[v].append([u, 0, len(self.g[u]) - 1])

        def dfs(self, u, t, f):
            if u == t:
                return f
            while self.ptr[u] < len(self.g[u]):
                e = self.g[u][self.ptr[u]]
                if e[1] and self.level[e[0]] == self.level[u] + 1:
                    r = self.dfs(e[0], t, min(f, e[1]))
                    if r:
                        e[1] -= r
                        self.g[e[0]][e[2]][1] += r
                        return r
                self.ptr[u] += 1
            return 0

        def flow(self, s, t):
            res = 0
            while True:
                self.level = [-1] * len(self.g)
                q = [s]
                self.level[s] = 0
                for u in q:
                    for v, c, _ in self.g[u]:
                        if c and self.level[v] == -1:
                            self.level[v] = self.level[u] + 1
                            q.append(v)
                if self.level[t] == -1:
                    return res
                self.ptr = [0] * len(self.g)
                while True:
                    x = self.dfs(s, t, 10**9)
                    if not x:
                        break
                    res += x

    for _ in range(t):
        d = int(next(it))
        n = int(next(it))
        din = Dinic(2 * (d + 1))

        for x in range(1, d):
            din.add(2*x, 2*x+1, 1)

        for _ in range(n):
            x = int(next(it))
            y = int(next(it))
            din.add(2*x+1, 2*y, 1)

        ans.append(str(din.flow(1, 2*d)))

    return "\n".join(ans)

assert run("""1
9 7
0 2
0 2
0 3
2 5
2 9
3 9
5 9
""") == "2"

assert run("""1
5 1
0 5
""") == "1"

assert run("""1
3 3
0 3
0 3
0 3
""") == "3"

assert run("""1
4 4
0 2
0 2
2 4
2 4
""") == "2"

assert run("""1
1 1
0 1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu gốc | 2 | Nhiều chuỗi có thể có thời gian chia sẻ | 
| Một chuyến tàu | 1 | Lịch trình tối thiểu | 
| Ba chuyến tàu giống hệt nhau | 3 | Khoảng thời gian giống nhau song song | 
| Chia hai giai đoạn | 2 | Xử lý đúng công suất chuyển mạch | 
| Một đơn vị ngày | 1 | Phạm vi thời gian nhỏ nhất có thể | 

## Vỏ cạnh 

Một chuyến tàu duy nhất hoạt động cả ngày sẽ được xử lý vì biểu đồ luồng chứa cạnh trực tiếp từ nguồn tới đích. Luồng tối đa tính chính xác nó là một lịch trình hoàn chỉnh. 

Khi nhiều đoàn tàu có khoảng thời gian giống hệt nhau, mỗi đoàn tàu sẽ trở thành một cạnh sức chứa riêng biệt. Thuật toán không kết hợp các khoảng thời gian bằng nhau nên mỗi chuyến tàu có sẵn có thể đóng góp thêm một người ăn xin. 

Khi nhiều lịch trình cần chuyển đổi cùng lúc, dung lượng nút chia sẽ ngăn chặn các giải pháp không hợp lệ. Ví dụ: trong đầu vào có tàu hỏa`[0,2]`Và`[2,4]`, chỉ một đơn vị luồng có thể đi qua thời gian 2, phù hợp với quy luật mà người ăn xin không thể đáp ứng khi chuyển mạch. 

Thuật toán cũng xử lý các trường hợp không tồn tại tuyến đường hoàn chỉnh. Nếu không có đường dẫn nào nối thời điểm 0 với thời điểm d thì luồng tối đa bằng 0 vì không có đơn vị luồng nào có thể đến được đích. 

Điều này cũng có thể được rút ngắn thành định dạng biên tập kiểu cuộc thi nếu bạn cần độ dài xuất bản Codeforces điển hình hơn.
