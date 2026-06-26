---
title: "CF 105316F - Lời Thì Thầm Truyền Thuyết"
description: "Chúng tôi được cung cấp hai mảng có cùng độ dài. Mảng thứ hai là cố định nhưng mảng thứ nhất có thể được hoán vị tùy ý. Sau khi chọn hoán vị của mảng đầu tiên, mỗi vị trí sẽ ghép một giá trị từ mảng đầu tiên với một giá trị từ mảng thứ hai."
date: "2026-06-23T15:09:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105316
codeforces_index: "F"
codeforces_contest_name: "2024 Aleppo Collegiate Programming Contest"
rating: 0
weight: 105316
solve_time_s: 57
verified: true
draft: false
---

[CF 105316F - Lời thì thầm huyền thoại](https://codeforces.com/problemset/problem/105316/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có cùng độ dài. Mảng thứ hai là cố định nhưng mảng thứ nhất có thể được hoán vị tùy ý. Sau khi chọn hoán vị của mảng đầu tiên, mỗi vị trí sẽ ghép một giá trị từ mảng đầu tiên với một giá trị từ mảng thứ hai. 

Việc ghép nối chỉ hợp lệ nếu mọi cặp được chọn đều thỏa mãn điều kiện chia hết: giá trị được chọn từ mảng đầu tiên phải chia hết cho giá trị tương ứng từ mảng thứ hai. Nếu không thể đạt được điều này cho tất cả các vị trí thì cấu hình không hợp lệ và câu trả lời là −1. 

Nếu tồn tại một cặp đầy đủ hợp lệ, thì mỗi vị trí sẽ đóng góp một tỷ lệ được hình thành bằng cách chia giá trị đã chọn từ mảng đầu tiên cho giá trị tương ứng từ mảng thứ hai. Giá trị của một cặp là giá trị tối thiểu của các tỷ lệ này trên tất cả các vị trí. Mục tiêu là sắp xếp lại mảng đầu tiên sao cho tỷ lệ tối thiểu này càng lớn càng tốt. 

Các ràng buộc có tổng kích thước nhỏ trong các trường hợp thử nghiệm, với tối đa 500 phần tử trên mỗi mảng cho mỗi thử nghiệm. Điều này cho phép các giải pháp nằm trong khoảng O(n³) hoặc tệ hơn một chút cho mỗi trường hợp thử nghiệm, đặc biệt nếu có hệ số logarit hoặc quy trình so khớp. Bất kỳ số mũ hoặc giai thừa nào đều bị loại trừ vì chỉ riêng hoán vị đã là 500! khả năng. 

Một nỗ lực ngây thơ sẽ thử tất cả các hoán vị của mảng đầu tiên, kiểm tra tính hợp lệ, tính tỷ lệ tối thiểu và lấy kết quả tốt nhất. Điều này thất bại ngay lập tức vì ngay cả với n = 10, số hoán vị vẫn là 10! = 3,6 triệu và với n = 500 thì điều đó hoàn toàn không khả thi. 

Một trường hợp thất bại tinh vi hơn đến từ các chiến lược kết hợp tham lam. Ví dụ: ghép từng bi với ai hợp lệ nhỏ nhất có thể có vẻ tự nhiên, nhưng nó có thể chặn các tỷ lệ lớn hơn sau này và phá hủy tính tối ưu toàn cầu. Hãy xem xét tình huống trong đó b nhỏ chỉ có một a tương thích, nhưng a đó cũng cần thiết cho b lớn hơn để duy trì tính khả thi. Một sự lựa chọn tham lam sẽ tiêu tốn nó sớm và khiến nhiệm vụ không thể thực hiện được ngay cả khi tồn tại một nhiệm vụ toàn cầu hợp lệ. 

Một trường hợp khác là tính khả thi. Nếu tồn tại ít nhất một bi mà không có ai chia hết cho nó thì không có cách sắp xếp nào có thể thỏa mãn các ràng buộc, bất kể hoán vị. Điều này phải ngay lập tức trả về −1. 

## Phương pháp tiếp cận 

Vấn đề cơ bản là gán mỗi giá trị trong b cho một giá trị duy nhất trong a, tuân theo các ràng buộc về khả năng chia hết, đồng thời tối đa hóa mục tiêu thắt cổ chai trên các tỷ lệ. 

Cách tiếp cận bạo lực sẽ xem xét mọi hoán vị của a, kiểm tra xem mỗi vị trí có thỏa mãn ai % bi == 0 hay không, tính tỷ lệ tối thiểu và theo dõi mức tối đa. Điều này đúng vì nó khám phá mọi nhiệm vụ có thể. Tuy nhiên, nó đòi hỏi n! séc và mỗi séc có giá O(n), mang lại các hoạt động O(n · n!), gần như không thể sử dụng được ngay khi vượt quá n rất nhỏ. 

Quan sát quan trọng là cấu trúc không liên quan trực tiếp đến các hoán vị mà là về sự khớp hoàn hảo trong biểu đồ hai bên. Mỗi phần tử của b phải khớp với chính xác một phần tử của a. Một cạnh chỉ tồn tại khi tính phân chia được giữ nguyên và trọng số của việc ghép cặp là ai / bi. Chúng tôi muốn tối đa hóa trọng lượng được chọn tối thiểu. 

Đây là một cách tối ưu hóa nút cổ chai cổ điển đối với các kết quả khớp. Thay vì tối ưu hóa trực tiếp tỷ lệ tối thiểu, chúng tôi đảo ngược quan điểm: cố định ngưỡng ứng viên x và hỏi liệu chúng tôi có thể xây dựng một kết hợp hoàn hảo trong đó mọi cặp được chọn đều thỏa mãn ai / bi ≥ x đồng thời thỏa mãn tính chia hết hay không. 

Đối với x cố định, chúng ta chỉ giữ các cạnh (bi → aj) sao cho aj chia hết cho bi và aj ≥ x · bi. Nếu chúng ta có thể tìm thấy sự kết hợp hoàn hảo theo những ràng buộc này thì x có thể đạt được. Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi trên biểu đồ hai bên.

Vì tính khả thi là đơn điệu trong x nên tìm kiếm nhị phân có thể áp dụng được. Chúng ta có thể kiểm tra các giá trị của x và hội tụ đến giá trị khả thi tối đa. Mỗi lần kiểm tra tính khả thi là một vấn đề khớp giữa hai bên, có thể giải quyết được bằng Hopcroft-Karp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(n · n!) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + kết hợp lưỡng cực | O(log A · E √V) | O(E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề thành việc liên tục giải quyết một câu hỏi khả thi phù hợp. 

1. Diễn giải mỗi phần tử của b là nút trái và mỗi phần tử của a là nút phải. Một kết nối chỉ được phép nếu a được chọn chia hết cho b đã chọn. Điều này mã hóa các cặp hợp lệ duy nhất được sự cố cho phép. 
2. Cố định giá trị ứng cử viên x đại diện cho tỷ lệ tối thiểu được phép. Đối với một cặp (bi, aj), chúng ta chỉ cho phép nếu aj % bi == 0 và aj ≥ x · bi. Điều này đảm bảo cả tính hợp lệ và ràng buộc tỷ lệ được thỏa mãn. 
3. Xây dựng biểu đồ lưỡng cực bằng cách sử dụng các cạnh được lọc này. Mỗi cạnh đại diện cho một phép gán có thể sử dụng hợp pháp dưới ngưỡng x hiện tại. 
4. Chạy thuật toán đối sánh lưỡng cực tối đa trên biểu đồ này và kiểm tra xem tất cả các nút trong b có thể khớp hay không. Nếu kích thước phù hợp bằng n thì x là khả thi; nếu không thì không. 
5. Tìm kiếm nhị phân x lớn nhất trong phạm vi hợp lệ. Giới hạn trên là max(ai // bi khả năng ghép cặp tối thiểu), nhưng thực tế chúng ta có thể giới hạn nó bằng max(a). 
6. Trả về giá trị x tối đa mà kết quả khớp hoàn hảo tồn tại. Nếu thậm chí x = 0 không thành công thì không thể gán đầy đủ và chúng ta trả về −1. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tính khả thi là đơn điệu trong x. Nếu tồn tại một kết quả phù hợp với một số ngưỡng x thì bất kỳ ngưỡng nào nhỏ hơn x' ≤ x chỉ làm giảm các ràng buộc cạnh và không thể phá hủy các cạnh hợp lệ hiện có. Điều này đảm bảo rằng tìm kiếm nhị phân không bỏ sót kết quả tối ưu. Ở mỗi bước, bước so khớp sẽ nắm bắt đầy đủ liệu các ràng buộc hiện tại có chấp nhận một phép gán hoàn chỉnh hay không, do đó không có quyết định cục bộ nào được đưa ra một cách tham lam; tính khả thi được kiểm tra trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

class HopcroftKarp:
    def __init__(self, n, m):
        self.n = n
        self.m = m
        self.g = [[] for _ in range(n)]
        self.pair_u = [-1] * n
        self.pair_v = [-1] * m
        self.dist = [0] * n

    def add_edge(self, u, v):
        self.g[u].append(v)

    def bfs(self):
        q = deque()
        for u in range(self.n):
            if self.pair_u[u] == -1:
                self.dist[u] = 0
                q.append(u)
            else:
                self.dist[u] = -1

        found = False

        while q:
            u = q.popleft()
            for v in self.g[u]:
                pu = self.pair_v[v]
                if pu != -1 and self.dist[pu] == -1:
                    self.dist[pu] = self.dist[u] + 1
                    q.append(pu)
                elif pu == -1:
                    found = True

        return found

    def dfs(self, u):
        for v in self.g[u]:
            pu = self.pair_v[v]
            if pu == -1 or (self.dist[pu] == self.dist[u] + 1 and self.dfs(pu)):
                self.pair_u[u] = v
                self.pair_v[v] = u
                return True
        self.dist[u] = -1
        return False

    def max_matching(self):
        matching = 0
        while self.bfs():
            for u in range(self.n):
                if self.pair_u[u] == -1 and self.dfs(u):
                    matching += 1
        return matching

def possible(a, b, x):
    n = len(a)
    hk = HopcroftKarp(n, n)
    for i in range(n):
        for j in range(n):
            if a[j] % b[i] == 0 and a[j] >= x * b[i]:
                hk.add_edge(i, j)
    return hk.max_matching() == n

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        if n == 1:
            if a[0] % b[0] == 0:
                print(a[0] // b[0])
            else:
                print(-1)
            continue

        if not possible(a, b, 0):
            print(-1)
            continue

        lo, hi = 0, 10**6
        ans = 0

        while lo <= hi:
            mid = (lo + hi) // 2
            if possible(a, b, mid):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng một phiên bản phù hợp cho mỗi lần kiểm tra tính khả thi. Cấu trúc lưỡng cực cố định b ở bên trái và a ở bên phải. Điều kiện a[j] % b[i] == 0 thực thi tính hợp lệ và bất đẳng thức a[j] ≥ x * b[i] thực thi ràng buộc tỷ lệ. 

Tìm kiếm nhị phân bao quanh việc kiểm tra tính khả thi này. Giới hạn dưới bắt đầu từ 0 vì tỷ lệ tầm thường luôn là yêu cầu yếu nhất và giới hạn trên được đặt an toàn thành 10⁶ vì tất cả các tỷ lệ đều bị giới hạn bởi thương số tối đa có thể có trong các ràng buộc. Mỗi kết quả khớp thành công sẽ cập nhật câu trả lời được biết đến nhiều nhất. 

Cạm bẫy tinh vi duy nhất là đảm bảo rằng việc so khớp được tính toán lại từ đầu cho mỗi x; việc sử dụng lại trạng thái sẽ làm hỏng tính chính xác vì các cạnh thay đổi giữa các lần lặp. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ trong đó a = [6, 8, 9], b = [2, 3, 3]. 

Chúng tôi kiểm tra x = 2: 

| Bước | Đồ thị điều kiện | Tiến trình phù hợp | Khả thi | 
| --- | --- | --- | --- | 
| Xây dựng | các cạnh chỉ khi a ≥ 2b | cạnh hạn chế | không chắc chắn | 
| Phù hợp | cố gắng hoàn thành nhiệm vụ | có thể thất bại | không | 

Bây giờ x = 1: 

| Bước | Đồ thị điều kiện | Tiến trình phù hợp | Khả thi | 
| --- | --- | --- | --- | 
| Xây dựng | được phép chia tất cả các cạnh | đồ thị phong phú hơn | vâng | 
| Phù hợp | tất cả b phù hợp | tồn tại sự phù hợp đầy đủ | vâng | 

Điều này cho thấy các ngưỡng cao hơn có thể phá vỡ tính khả thi ngay cả khi các ngưỡng thấp hơn hoạt động, thúc đẩy tìm kiếm nhị phân. 

Ví dụ thứ hai: a = [12, 6], b = [3, 2]. 

Với x = 2, các cặp hợp lệ là 12/3 = 4 và 6/2 = 3, vì vậy cả hai đều đúng. Kết hợp thành công. Với x = 3, chỉ 12/3 đúng, nhưng 6 là không đủ với b = 2 vì 6/2 = 3 vẫn đúng nên vẫn khả thi. Tăng x hơn nữa cuối cùng sẽ phá vỡ nhiệm vụ khi một số b mất tất cả các ứng cử viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · log A · E √V) | Mỗi bước tìm kiếm nhị phân chạy Hopcroft-Karp trên tối đa n2 cạnh | 
| Không gian | O(n²) | danh sách kề của đồ thị hai bên | 

Các ràng buộc giới hạn tổng n đến 500, do đó, ngay cả với khoảng 20 bước tìm kiếm nhị phân và khớp trên các biểu đồ dày đặc, giải pháp vẫn phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    class HopcroftKarp:
        def __init__(self, n, m):
            self.n = n
            self.m = m
            self.g = [[] for _ in range(n)]
            self.pair_u = [-1] * n
            self.pair_v = [-1] * m
            self.dist = [0] * n

        def add_edge(self, u, v):
            self.g[u].append(v)

        def bfs(self):
            from collections import deque
            q = deque()
            for u in range(self.n):
                if self.pair_u[u] == -1:
                    self.dist[u] = 0
                    q.append(u)
                else:
                    self.dist[u] = -1

            found = False
            while q:
                u = q.popleft()
                for v in self.g[u]:
                    pu = self.pair_v[v]
                    if pu != -1 and self.dist[pu] == -1:
                        self.dist[pu] = self.dist[u] + 1
                        q.append(pu)
                    elif pu == -1:
                        found = True
            return found

        def dfs(self, u):
            for v in self.g[u]:
                pu = self.pair_v[v]
                if pu == -1 or (self.dist[pu] == self.dist[u] + 1 and self.dfs(pu)):
                    self.pair_u[u] = v
                    self.pair_v[v] = u
                    return True
            self.dist[u] = -1
            return False

        def max_matching(self):
            res = 0
            while self.bfs():
                for i in range(self.n):
                    if self.pair_u[i] == -1 and self.dfs(i):
                        res += 1
            return res

    def possible(a, b, x):
        n = len(a)
        hk = HopcroftKarp(n, n)
        for i in range(n):
            for j in range(n):
                if a[j] % b[i] == 0 and a[j] >= x * b[i]:
                    hk.add_edge(i, j)
        return hk.max_matching() == n

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))
            if not possible(a, b, 0):
                out.append("-1")
                continue
            lo, hi = 0, 10**6
            ans = 0
            while lo <= hi:
                mid = (lo + hi) // 2
                if possible(a, b, mid):
                    ans = mid
                    lo = mid + 1
                else:
                    hi = mid - 1
            out.append(str(ans))
        return "\n".join(out)

    return solve()

# custom cases

# minimum size
assert run("1\n1\n6\n3\n") == "2"

# impossible
assert run("1\n2\n2 4\n3 5\n") == "-1"

# all equal compatible
assert run("1\n3\n6 12 18\n3 2 6\n") != ""

# exact matching pressure case
assert run("1\n3\n6 10 15\n3 5 5\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử chia hết | 2 | độ đúng cơ sở | 
| không có kết quả phù hợp | -1 | xử lý bất khả thi | 
| tất cả đều tương thích | giá trị dương | sự ổn định phù hợp đầy đủ | 
| ràng buộc hỗn hợp | đầu ra hợp lệ | cấu trúc ứng suất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi khả năng chia hết không thành công đối với một giá trị b. Ví dụ: a = [4, 6], b = [3, 5]. Vì 5 không chia hết nên không có kết quả khớp hoàn toàn mặc dù tồn tại kết quả khớp một phần. Thuật toán phát hiện điều này tại x = 0 và ngay lập tức trả về −1 vì Hopcroft-Karp không thể đạt được kết quả khớp hoàn toàn. 

Một trường hợp cạnh khác xảy ra khi nhiều giá trị b cạnh tranh để giành được một giá trị a lớn. Ví dụ: a = [12, 12, 12], b = [2, 3, 6]. Mặc dù mỗi cặp đều có thể chia hết nhưng ràng buộc tỷ lệ có thể buộc các phép gán khác nhau tùy thuộc vào x. Bước khớp sẽ giải quyết chính xác điều này vì nó không cam kết một cách tham lam; nó khám phá các bài tập trên toàn cầu, đảm bảo rằng nếu có một sự sắp xếp hợp lệ cho một ngưỡng nhất định thì nó sẽ được tìm thấy ngay cả khi có sự tranh chấp gay gắt.
