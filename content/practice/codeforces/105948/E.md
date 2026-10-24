---
title: "CF 105948E - Đánh giá thuộc địa cho địa hóa"
description: "Chúng ta được cung cấp một biểu đồ trong đó mỗi đỉnh đại diện cho một hành tinh và mỗi đỉnh có một giá trị số gọi là khả năng sinh sống của nó. Biểu đồ là vô hướng và các cạnh biểu thị các tuyến đường di chuyển hai chiều giữa các hành tinh."
date: "2026-06-22T16:06:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105948
codeforces_index: "E"
codeforces_contest_name: "CCF CAT NAEC 2025 (Provincial)"
rating: 0
weight: 105948
solve_time_s: 55
verified: true
draft: false
---

[CF 105948E - Đánh giá quá trình thuộc địa hóa cho địa khai hóa](https://codeforces.com/problemset/problem/105948/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ trong đó mỗi đỉnh đại diện cho một hành tinh và mỗi đỉnh có một giá trị số gọi là khả năng sinh sống của nó. Biểu đồ là vô hướng và các cạnh biểu thị các tuyến đường di chuyển hai chiều giữa các hành tinh. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải chọn chính xác k hành tinh sao cho mỗi hành tinh được chọn có thể tiếp cận mọi hành tinh được chọn khác bằng cách nằm hoàn toàn trong tập hợp đã chọn. Nói cách khác, các hành tinh được chọn phải tạo thành một đồ thị con cảm ứng được kết nối, bởi vì không được phép truyền tải qua các đỉnh không được chọn. 

Trong số tất cả các lựa chọn được kết nối hợp lệ có kích thước k như vậy, chúng tôi xem xét giá trị khả năng cư trú nhỏ nhất bên trong tập hợp đã chọn. Mục tiêu là tối đa hóa giá trị tối thiểu này. 

Điều này ngay lập tức gợi ý rằng câu trả lời phụ thuộc vào việc chọn giá trị ngưỡng x và kiểm tra xem có tồn tại thành phần liên thông chứa ít nhất k đỉnh có trọng số ít nhất là x hay không. Nếu một tập hợp như vậy tồn tại thì chúng ta có thể chọn k trong số chúng bên trong vùng được kết nối đó. 

Các ràng buộc ngụ ý rằng n và m lên tới 100000 cho mỗi trường hợp thử nghiệm, với tổng số tiền trên các thử nghiệm được giới hạn bởi 200000. Điều này loại trừ bất kỳ phương pháp nào tính toán lại khả năng kết nối từ đầu cho nhiều ngưỡng ứng viên. Một giải pháp phải tuyến tính hoặc gần tuyến tính một cách hiệu quả trên tất cả các trường hợp thử nghiệm. 

Trường hợp cạnh tinh vi phát sinh khi đồ thị bị ngắt kết nối theo cách không có thành phần nào được kết nối có kích thước ít nhất là k. Ví dụ: nếu n = 5, k = 3 và tất cả các cạnh đều bị cô lập thì không có lựa chọn hợp lệ nào tồn tại và câu trả lời phải là -1 ngay cả khi tất cả các trọng số đều lớn. 

Một trường hợp lỗi khác xảy ra khi người ta có thể xem xét không chính xác các thành phần của biểu đồ đầy đủ mà không tôn trọng ràng buộc rằng việc truyền tải không thể đi qua các nút bị loại trừ. Điều này có nghĩa là chúng tôi không thể lấy một thành phần được kết nối toàn cầu trừ khi tất cả các nút bên trong nó hợp lệ dưới ngưỡng đã chọn. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là xem xét mọi tập hợp con có thể có của k nút, kiểm tra xem nó có tạo thành một đồ thị con cảm ứng được kết nối hay không và tính trọng số tối thiểu trong đó. Điều này là không thể về mặt tổ hợp vì số lượng tập hợp con là hàm mũ theo n và thậm chí việc kiểm tra kết nối trên mỗi tập hợp con là tuyến tính theo n cộng với m. Điều này dẫn đến các phép toán gần như O(n choose k · (n + m)), hoàn toàn không thể thực hiện được ngay cả đối với n rất nhỏ. 

Một cách mạnh mẽ có cấu trúc hơn là sửa câu trả lời x và sau đó kiểm tra xem đồ thị con được tạo ra bởi các nút có trọng số ít nhất x có chứa thành phần được kết nối có kích thước ít nhất là k hay không. Điều này làm giảm vấn đề xuống mức kiểm tra tính khả thi trên mỗi x. Nếu chúng ta có thể kiểm tra điều này một cách hiệu quả, chúng ta có thể tìm kiếm nhị phân trên x. 

Thông tin chi tiết quan trọng là khả năng kết nối chỉ phụ thuộc vào các cạnh giữa các nút "đang hoạt động" và chúng tôi chỉ quan tâm đến kích thước của các thành phần được kết nối trong biểu đồ được lọc. Nếu chúng ta sắp xếp các nút bằng cách giảm trọng lượng và kích hoạt chúng dần dần, chúng ta có thể duy trì các thành phần được kết nối bằng cấu trúc tìm liên kết. Ngay khi một thành phần đạt đến kích thước k, ngưỡng trọng lượng hiện tại là khả thi. 

Điều này biến vấn đề thành sắp xếp cộng với bảo trì DSU, tuyến tính cho mỗi trường hợp thử nghiệm nhưng tuyến tính tổng thể trong thực tế với các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · (n + m)) | O(n + m) | Quá chậm | 
| Ngưỡng + DSU | O(n log n + m α(n)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các nút theo thứ tự giảm dần khả năng sinh sống để khi một nút được kích hoạt, trọng số của nó đại diện cho mức tối thiểu ứng viên hiện tại. 

Chúng tôi chỉ duy trì DSU trên các nút đang hoạt động.

1. Sắp xếp tất cả các nút theo trọng số giảm dần. Điều này đảm bảo trước tiên chúng tôi thử các giá trị tối thiểu lớn hơn, tương ứng trực tiếp với việc tối đa hóa câu trả lời cuối cùng. 
2. Ban đầu, không có nút nào hoạt động trong DSU. 
3. Lặp lại qua các nút theo thứ tự được sắp xếp và kích hoạt từng nút một. 
4. Khi kích hoạt một nút, hãy đánh dấu nút đó là đang hoạt động và kết hợp nó với tất cả các nút lân cận đã hoạt động. Điều này dần dần xây dựng các thành phần được kết nối của sơ đồ con cảm ứng của các nút hiện được kích hoạt. 
5. Sau mỗi lần kích hoạt, hãy tính kích thước của thành phần DSU chứa nút đó. Nếu nó ít nhất là k, chúng ta có thể ngay lập tức trả về trọng số của nút hiện tại làm câu trả lời. 
6. Nếu chúng tôi xử lý xong tất cả các nút mà không tìm thấy thành phần có kích thước k, hãy trả về -1. 

Lý do bước 4 đúng là vì các cạnh của các nút không hoạt động không được góp phần vào kết nối. Bằng cách chỉ kết hợp các hàng xóm đang hoạt động, chúng tôi đảm bảo rằng DSU thể hiện chính xác khả năng kết nối trong biểu đồ con được tạo ra. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quy trình, tập hoạt động tương ứng chính xác với các nút có trọng số ít nhất là ngưỡng hiện tại. Các thành phần DSU đại diện cho các thành phần được kết nối trong sơ đồ con do các nút hoạt động này tạo ra. Khi một thành phần lần đầu tiên đạt đến kích thước k, điều đó có nghĩa là tồn tại một sơ đồ con được kết nối gồm k nút, tất cả đều có trọng số ít nhất là ngưỡng hiện tại. Vì chúng tôi xử lý trọng số theo thứ tự giảm dần nên ngưỡng này là giá trị tối thiểu tối đa có thể có đối với mọi tập hợp kích thước k được kết nối hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

    def comp_size(self, x):
        return self.size[self.find(x)]

t = int(input())
for _ in range(t):
    n, m, k = map(int, input().split())
    w = list(map(int, input().split()))

    edges = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges[u].append(v)
        edges[v].append(u)

    order = sorted(range(n), key=lambda i: -w[i])

    active = [False] * n
    dsu = DSU(n)

    answer = -1

    for node in order:
        active[node] = True
        for nei in edges[node]:
            if active[nei]:
                dsu.union(node, nei)

        if dsu.comp_size(node) >= k:
            answer = w[node]
            break

    print(answer)
```DSU được khởi tạo với mỗi nút trong thành phần riêng của nó, nhưng chi tiết quan trọng là các thành phần chỉ trở nên có ý nghĩa khi các nút được kích hoạt. Mảng hoạt động đảm bảo chúng tôi không bao giờ hợp nhất thông qua các nút không hoạt động, duy trì ràng buộc đồ thị con được tạo ra. 

Bước sắp xếp rất quan trọng vì nó đảm bảo rằng lần đầu tiên chúng ta đạt đến một thành phần có kích thước k, chúng ta đang sử dụng trọng số tối thiểu cao nhất có thể. 

Kiểm tra comp_size sử dụng đại diện của nút mới được kích hoạt, điều này là đủ vì tất cả các hoạt động hợp nhất liên quan đến nút đó diễn ra ngay lập tức khi kích hoạt. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ có 5 nút và k = 3: 

đầu vào:```
n = 5, m = 4, k = 3
w = [5, 1, 4, 3, 2]
edges: (1-2), (2-3), (3-4), (4-5)
```Các nút được sắp xếp theo trọng số: 1(5), 3(4), 4(3), 5(2), 2(1) 

Chúng tôi theo dõi kích hoạt: 

| Bước | Nút | Bộ hoạt động | Sáp nhập DSU | Kích thước thành phần | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | {1} | không | 1 | -1 | 
| 2 | 3 | {1,3} | không | 1,1 | -1 | 
| 3 | 4 | {1,3,4} | 3 kết nối với 4 | 2 | -1 | 
| 4 | 5 | {1,3,4,5} | 4-5 | 3 | 3 | 

Khi nút 5 (trọng số 2) được thêm vào, chuỗi 3-4-5 hoạt động và tạo thành thành phần được kết nối có kích thước 3. Tuy nhiên, chúng ta phải đảm bảo trọng số tối thiểu ở đây là 3 chứ không phải 2. Bảng này cho thấy một vấn đề tế nhị: trên thực tế, câu trả lời sẽ kích hoạt khi nút 4 được thêm vào vì thành phần hoạt động được kết nối {3,4,5} chưa được kết nối hoàn toàn cho đến khi bao gồm 5, nhưng trọng số tối thiểu của bộ đó là 2, do đó, thành phần k hợp lệ đầu tiên thực sự là tập hợp {3,4,5} có tối thiểu 2. Nếu thành phần có trọng số cao hơn có kích thước 3 tồn tại trước đó, nó sẽ được chọn đầu tiên do sắp xếp thứ tự. 

Điều này chứng tỏ rằng độ chính xác phụ thuộc vào thứ tự kích hoạt giảm dần, đảm bảo chúng tôi luôn xem xét ngưỡng tốt nhất có thể trước tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) α(n)) mỗi lần kiểm tra | Mỗi nút được kích hoạt một lần và mỗi cạnh được xử lý nhiều nhất hai lần thông qua các liên kết DSU | 
| Không gian | O(n + m) | danh sách kề cộng với mảng DSU | 

Trên tất cả các trường hợp thử nghiệm, tổng n và m được giới hạn bởi 2 × 10^5, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1] * n

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b):
            ra, rb = self.find(a), self.find(b)
            if ra == rb:
                return
            if self.size[ra] < self.size[rb]:
                ra, rb = rb, ra
            self.parent[rb] = ra
            self.size[ra] += self.size[rb]

        def comp_size(self, x):
            return self.size[self.find(x)]

    t = int(input())
    out = []

    for _ in range(t):
        n, m, k = map(int, input().split())
        w = list(map(int, input().split()))

        edges = [[] for _ in range(n)]
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            edges[u].append(v)
            edges[v].append(u)

        order = sorted(range(n), key=lambda i: -w[i])

        active = [False] * n
        dsu = DSU(n)

        ans = -1
        for node in order:
            active[node] = True
            for nei in edges[node]:
                if active[nei]:
                    dsu.union(node, nei)
            if dsu.comp_size(node) >= k:
                ans = w[node]
                break

        out.append(str(ans))

    return "\n".join(out)

# simple constructed tests
assert run("""1
1 0 1
5
""") == "5"

assert run("""1
3 0 2
5 4 3
""") == "-1"

assert run("""1
3 2 2
1 2 3
1 2
2 3
""") == "2"

assert run("""1
5 4 3
5 1 4 3 2
1 2
2 3
3 4
4 5
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 5 | trường hợp tối thiểu | 
| đồ thị bị ngắt kết nối | -1 | không thể | 
| xích nhỏ k=2 | 2 | ngưỡng đúng đắn | 
| chuỗi đầy đủ k=3 | 2 | tích lũy kết nối | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi biểu đồ có đủ nút nhưng không thành phần nào được kết nối có thể đạt kích thước k ngay cả trước khi xem xét trọng số. Trong tình huống đó, thuật toán sẽ kích hoạt tất cả các nút và không bao giờ kích hoạt điều kiện kích thước, do đó, thuật toán sẽ xuất ra đúng -1. 

Một trường hợp khác là khi câu trả lời đến từ một nút bị cô lập có trọng số cao không bao giờ kết nối. Vì cần có kết nối nên một nút không thể đáp ứng k > 1 và kích thước DSU ngăn chặn kết quả dương tính giả vì sự kết hợp không bao giờ xảy ra nếu không có các cạnh. 

Trường hợp thứ ba liên quan đến nhiều thành phần phát triển song song. Thuật toán vẫn hoạt động vì mỗi lần kích hoạt chỉ hợp nhất các vùng lân cận cục bộ và lần đầu tiên bất kỳ thành phần nào đạt kích thước k, nó sẽ tương ứng với ngưỡng tốt nhất có thể do quá trình xử lý trọng số giảm dần.
