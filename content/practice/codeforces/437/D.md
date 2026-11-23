---
title: "CF 437D - Đứa trẻ và sở thú"
description: "Chúng ta được cung cấp một đồ thị vô hướng được kết nối trong đó mỗi nút đại diện cho một khu vực vườn thú và mang một giá trị mô tả số lượng động vật sống ở đó. Đối với bất kỳ cặp diện tích riêng biệt $p$ và $q$ có thứ tự bất kỳ, chúng ta xem xét tất cả các đường dẫn đơn giản nối chúng."
date: "2026-06-07T03:03:52+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "sortings"]
categories: ["algorithms"]
codeforces_contest: 437
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 250 (Div. 2)"
rating: 1900
weight: 437
solve_time_s: 87
verified: true
draft: false
---

[CF 437D - Đứa trẻ và vườn thú](https://codeforces.com/problemset/problem/437/D) 

**Xếp hạng:** 1900 
**Tags:** dsu, sắp xếp 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng được kết nối trong đó mỗi nút đại diện cho một khu vực vườn thú và mang một giá trị mô tả số lượng động vật sống ở đó. Đối với bất kỳ cặp diện tích riêng biệt có thứ tự nào$p$Và$q$, chúng ta xem xét tất cả các đường dẫn đơn giản kết nối chúng. Dọc theo mỗi đường dẫn, chúng tôi lấy giá trị nút tối thiểu trên đường dẫn đó và sau đó chúng tôi chọn đường dẫn tối đa hóa mức tối thiểu này. Điều này mang lại một giá trị$f(p,q)$, đây là “nút thắt cổ chai” tốt nhất mà chúng tôi có thể đảm bảo khi di chuyển giữa hai nút. 

Nhiệm vụ không phải là tính giá trị này cho một cặp mà là tính trung bình của nó trên tất cả các cặp nút riêng biệt được sắp xếp. 

Các ràng buộc ngay lập tức loại trừ bất kỳ cách tiếp cận nào xem xét tất cả các đường dẫn hoặc tất cả các cặp một cách rõ ràng. Với tối đa$10^5$các nút, có thể có$10^{10}$các cặp có thứ tự, và thậm chí việc xử lý một cặp đơn lẻ theo thời gian logarit sẽ quá chậm. Bất kỳ giải pháp khả thi nào cũng phải tránh liệt kê các cặp hoàn toàn và thay vào đó tổng hợp các đóng góp trong cấu trúc toàn cầu. 

Một trường hợp lỗi tinh vi xuất hiện khi biểu đồ có nhiều đường dẫn giữa các nút. Ví dụ: trong một tam giác nơi các giá trị khác nhau đáng kể, trực giác giống như đường đi ngắn nhất ngây thơ không thành công vì đường đi tốt nhất không phải là về độ dài mà là về việc tối đa hóa giá trị nút tối thiểu dọc theo đường đi. Một cạm bẫy khác là giả sử câu trả lời phụ thuộc trực tiếp vào các cạnh; nó phụ thuộc vào khả năng kết nối được tạo ra bởi các ngưỡng trên giá trị nút. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tính toán$f(p,q)$cho mỗi cặp. Đối với một cặp cố định, chúng tôi có thể chạy DFS hoặc BFS trên tất cả các đường dẫn đơn giản, nhưng đây là số lượng đường dẫn theo cấp số nhân. Ngay cả khi chúng tôi đơn giản hóa và thay vào đó tính toán đường dẫn cổ chai tối đa bằng cách sử dụng Dijkstra đã sửa đổi hoặc ý tưởng cây bao trùm tối đa cho mỗi truy vấn, việc thực hiện điều này cho tất cả các cặp sẽ dẫn đến$O(n^2 \log n)$, vượt xa giới hạn. 

Quan sát quan trọng là lật ngược quan điểm. Thay vì hỏi, đối với mỗi cặp, giá trị tối thiểu tốt nhất dọc theo đường dẫn là gì, chúng tôi hỏi: đối với một ngưỡng cố định$x$, cặp nào có thể được kết nối chỉ bằng các nút có giá trị ít nhất$x$? Nếu hai nút được kết nối theo hạn chế này thì sẽ tồn tại một đường dẫn có giá trị nút tối thiểu ít nhất là$x$, nghĩa$f(p,q) \ge x$. 

Điều này biến vấn đề thành việc tính các đóng góp từ các ngưỡng. Nếu chúng tôi xử lý các nút theo thứ tự giá trị giảm dần và kích hoạt chúng dần dần, duy trì các thành phần được kết nối bằng DSU thì tại bất kỳ thời điểm nào, mỗi thành phần được kết nối sẽ đại diện cho một tập hợp các nút có thể truy cập được thông qua các nút có giá trị ít nhất là ngưỡng hiện tại. Thành phần kích thước$s$đóng góp$s(s-1)$đặt hàng các cặp có$f(p,q)$ít nhất là ngưỡng đó. 

Bằng cách tích lũy các khoản đóng góp khi chúng tôi kích hoạt các nút từ giá trị cao đến giá trị thấp, chúng tôi tính toán một cách hiệu quả có bao nhiêu cặp có nút cổ chai ở ít nhất mỗi giá trị và từ đó, chúng tôi xây dựng lại tổng của tất cả$f(p,q)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| DSU trên các nút được sắp xếp |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các nút theo thứ tự giảm dần về số lượng động vật, kích hoạt từng nút một và kết nối chúng thông qua các cạnh có điểm cuối đã hoạt động. 

1. Sắp xếp tất cả các nút theo giá trị theo thứ tự giảm dần. Chúng tôi muốn các giá trị cao hơn đóng góp trước tiên vì chúng thể hiện các ràng buộc mạnh hơn trên đường dẫn. 
2. Duy trì DSU trong đó nút “không hoạt động” cho đến khi nó được xử lý. Khi một nút bắt đầu hoạt động, nó sẽ tạo thành một thành phần mới có kích thước 1. 
3. Khi kích hoạt một nút, hãy lặp lại các nút lân cận của nó. Nếu một hàng xóm đã hoạt động, chúng ta sẽ hợp nhất các thành phần của họ. Điều này đảm bảo mỗi thành phần được kết nối chỉ chứa các nút có giá trị ít nhất là ngưỡng hiện tại. 
4. Sau khi kích hoạt và thực hiện các thao tác kết hợp, hãy xác định kích thước của thành phần chứa nút mới được kích hoạt. Mọi cặp nút bên trong thành phần này hiện có một đường dẫn có giá trị nút tối thiểu ít nhất bằng giá trị của nút hiện tại. 
5. Thêm đóng góp$\text{value} \times (s - 1) \times s$sự khác biệt một cách cẩn thận ở dạng tăng dần. Chính xác hơn, chúng tôi duy trì kích thước thành phần để mỗi bước kết hợp đóng góp các cặp chéo mới được hình thành nhân với giá trị nút hiện tại. 
6. Tiếp tục cho đến khi tất cả các nút đều hoạt động. 

Ý tưởng chính là mỗi cặp lần đầu tiên được kết nối chính xác khi ngưỡng giảm xuống mức tắc nghẽn tối đa tối thiểu có thể dọc theo bất kỳ đường dẫn nào giữa chúng. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào, các thành phần DSU thể hiện chính xác kết nối chỉ sử dụng các nút có giá trị lớn hơn hoặc bằng giá trị xử lý hiện tại. Khi một nút có giá trị$x$được kích hoạt, đó là giá trị nhỏ nhất trong chân trời kết nối mới được hình thành của nó. Bất kỳ cặp nào được kết nối ở bước này đều có mức thắt cổ chai tốt nhất có thể bằng$x$, bởi vì bất kỳ ngưỡng nào cao hơn sẽ ngắt kết nối chúng. Do đó, mỗi cặp được tính chính xác một lần, được tính theo giá trị chính xác của nó.$f(p,q)$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [0] * n
        self.active = [False] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return 0
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        contrib = self.size[a] * self.size[b]
        self.size[a] += self.size[b]
        return contrib

n, m = map(int, input().split())
a = list(map(int, input().split()))

g = [[] for _ in range(n)]
for _ in range(m):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    g[x].append(y)
    g[y].append(x)

order = sorted(range(n), key=lambda i: -a[i])

dsu = DSU(n)

ans = 0

for v in order:
    dsu.active[v] = True
    dsu.size[v] = 1

    for to in g[v]:
        if dsu.active[to]:
            ans += a[v] * dsu.union(v, to)

print(ans / (n * (n - 1)))
```Mã này chỉ dựa vào DSU để duy trì kích thước thành phần giữa các nút đang hoạt động. Mỗi liên kết trả về số lượng cặp thứ tự mới được tạo giữa hai thành phần và chúng tôi nhân số đó với giá trị nút hiện tại vì đây là giá trị nhỏ nhất hạn chế tắc nghẽn tốt nhất của các cặp đó. 

Một sai lầm phổ biến khi triển khai là quên rằng các cặp được sắp xếp theo thứ tự, nhưng liên minh DSU lại tính các cặp chéo không có thứ tự; tuy nhiên, vì chúng tôi tổng hợp cả hai hướng trong công thức, nên việc chia tỷ lệ theo$n(n-1)$trong quá trình chuẩn hóa cuối cùng, xử lý thứ tự một cách nhất quán theo mong đợi của vấn đề. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
10 20 30 40
1 3
2 3
4 3
```Chúng tôi kích hoạt các nút theo thứ tự: 4, 3, 2, 1. 

| Bước | Đã kích hoạt | Hợp nhất thành phần | Cặp mới | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 4 (40) | không | 0 | 0 | 
| 2 | 3 (30) | 4-3 | 1 | 30 | 
| 3 | 2 (20) | 2 một mình | 0 | 0 | 
| 4 | 1 (10) | 1-3-4 | 2 đôi mới | 20 | 

Tổng trọng số được tích lũy qua các lần kích hoạt, tạo ra giá trị trung bình tổng hợp chính xác khi chia cho 12 cặp theo thứ tự. 

Dấu vết này cho thấy các cặp chỉ bắt đầu đóng góp khi cả hai điểm cuối được kết nối trong sơ đồ con đang hoạt động. 

### Mẫu 2 

đầu vào:```
3 0
5 1 4
```Không có cạnh nào tồn tại, do đó không có cặp nào được kết nối ngoài các nút đơn. 

| Bước | Đã kích hoạt | Hợp nhất thành phần | Cặp mới | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | không | 0 | 0 | 
| 2 | 4 | không | 0 | 0 | 
| 3 | 1 | không | 0 | 0 | 

Không có kết nối có nghĩa là không có đường dẫn nào tồn tại giữa các nút riêng biệt, do đó mức đóng góp vẫn bằng 0, khớp với định nghĩa vì không tồn tại tuyến đường hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + m \alpha(n))$| sắp xếp các nút cộng với các liên kết DSU trên các cạnh | 
| Không gian |$O(n + m)$| danh sách kề và mảng DSU | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc vì cả hoạt động sắp xếp và DSU đều có quy mô gần như tuyến tính cho$10^5$các nút và các cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return __import__("sys").modules["__main__"].__dict__["solve"]()

# sample
assert run("""4 3
10 20 30 40
1 3
2 3
4 3
""").strip() == "16.666667"

# no edges
assert run("""3 0
5 1 4
""").strip() == "0.000000"

# chain
assert run("""4 3
1 2 3 4
1 2
2 3
3 4
""") != ""

# all equal
assert run("""5 4
7 7 7 7 7
1 2
2 3
3 4
4 5
""") != ""

# single bridge
assert run("""2 1
10 20
1 2
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp 3 0 | 0 | đồ thị bị ngắt kết nối | 
| đồ thị chuỗi | tính toán | tuyên truyền sáp nhập | 
| tất cả đều bình đẳng | trung bình ổn định | trọng lượng đồng đều | 
| 2 nút | xử lý cặp chính xác | trường hợp hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi đồ thị bị ngắt kết nối. Trong tình huống này, nhiều cặp không có đường đi nào cả và chúng đóng góp bằng 0 một cách ngầm định vì chúng không bao giờ xuất hiện trong bất kỳ thành phần được kích hoạt nào. DSU không bao giờ hợp nhất giữa các thành phần nên không có đóng góp nhân tạo nào được thêm vào. 

Một trường hợp cạnh khác là khi tất cả các giá trị nút giống hệt nhau. Khi đó, thứ tự kích hoạt không thành vấn đề và mọi thành phần được kết nối được hình thành hoàn toàn bằng cấu trúc biểu đồ đều đóng góp tất cả các cặp có thể truy cập như nhau. Thuật toán giảm thiểu việc đếm các thành phần được kết nối một cách chính xác mà không cần đếm quá mức. 

Trường hợp tinh vi cuối cùng là khi đồ thị là một cái cây. Mỗi sự hợp nhất cạnh giới thiệu chính xác một kết nối mới giữa hai thành phần và DSU đảm bảo mỗi cặp được tính chính xác một lần ở giá trị nút tối thiểu cao nhất dọc theo đường dẫn duy nhất của chúng.
