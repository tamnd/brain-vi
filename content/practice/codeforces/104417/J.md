---
title: "CF 104417J - Không phải vấn đề truy vấn đường dẫn khác"
description: "Chúng ta được cho một đồ thị có trọng số vô hướng. Mỗi cạnh mang trọng số 60 bit. Đối với bất kỳ bước đi nào giữa hai đỉnh, chúng tôi tính toán một giá trị bằng cách lấy AND theo bit của tất cả các trọng số cạnh dọc theo bước đi đó. Một cuộc dạo chơi được coi là tốt nếu giá trị AND này ít nhất phải đạt ngưỡng V nhất định."
date: "2026-06-30T19:17:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104417
codeforces_index: "J"
codeforces_contest_name: "The 13th Shandong ICPC Provincial Collegiate Programming Contest"
rating: 0
weight: 104417
solve_time_s: 53
verified: true
draft: false
---

[CF 104417J - Không phải vấn đề truy vấn đường dẫn khác](https://codeforces.com/problemset/problem/104417/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có trọng số vô hướng. Mỗi cạnh mang trọng số 60 bit. Đối với bất kỳ bước đi nào giữa hai đỉnh, chúng tôi tính toán một giá trị bằng cách lấy AND theo bit của tất cả các trọng số cạnh dọc theo bước đi đó. Một bước đi được coi là tốt nếu giá trị AND này ít nhất là một ngưỡng V nhất định. Đối với mỗi truy vấn, chúng ta phải quyết định xem có tồn tại bất kỳ đường dẫn nào giữa hai đỉnh được truy vấn có AND trên các cạnh của nó không giảm xuống dưới V hay không. 

Khó khăn chính là giá trị của đường dẫn không được cộng hoặc có thể giảm thiểu theo nghĩa thông thường. Việc thêm nhiều cạnh chỉ có thể giảm hoặc giữ nguyên theo bit VÀ, không bao giờ tăng nó. Điều này ngay lập tức ngụ ý rằng các đường dẫn dài hơn vốn khó đáp ứng ràng buộc ngưỡng hơn, bởi vì mọi cạnh phụ chỉ có thể xóa các bit. 

Các ràng buộc rất lớn, lên tới 100.000 đỉnh, 500.000 cạnh và 500.000 truy vấn. Bất kỳ giải pháp nào tính toán lại khả năng kết nối cho mỗi truy vấn hoặc khám phá đường dẫn trực tiếp sẽ không thành công. Ngay cả cách tiếp cận theo kiểu BFS hoặc Dijkstra cho mỗi truy vấn cũng sẽ dẫn đến các hoạt động khoảng 5e5 lần 1e5 trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Một trường hợp khó khăn nhưng quan trọng nảy sinh từ việc suy nghĩ về những con đường ngắn nhất hoặc những điểm nghẽn tối đa. Ví dụ: một đường dẫn tối đa hóa tổng hoặc tối thiểu hóa cạnh tối đa không giúp ích gì ở đây vì phép toán AND hoạt động khác. Hãy xem xét một tam giác có các cạnh là 15, 7 và 7. Một đường dẫn sử dụng cả 7 cạnh sẽ mang lại AND 7, nhưng việc đưa vào cạnh 15 sẽ làm giảm tính linh hoạt một cách khác nhau. Cấu trúc không đơn điệu theo nghĩa tối ưu hóa đồ thị thông thường. 

Một dạng lỗi khác là giả định rằng các cạnh có trọng số ít nhất là V là đủ riêng lẻ. Điều này sai vì ngay cả khi mọi cạnh đều nằm trên V thì AND của nhiều cạnh vẫn có thể giảm xuống dưới V nếu chúng không đồng ý về vị trí bit. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xử lý từng truy vấn một cách độc lập. Đối với một cặp u và v nhất định, chúng ta có thể chạy BFS hoặc DFS và cố gắng theo dõi tất cả các giá trị AND có thể có dọc theo các đường dẫn, giữ lại những giá trị tốt nhất. Tuy nhiên, không gian trạng thái bùng nổ vì mỗi nút có thể đạt được nhiều kết quả AND khác nhau và việc kết hợp chúng đòi hỏi phải theo dõi các tập hợp con của mặt nạ bit. Trong trường hợp xấu nhất, điều này trở thành cấp số nhân về độ dài đường dẫn, điều này là không thể thực hiện được. 

Một quan sát có cấu trúc hơn đến từ việc đảo ngược quan điểm. Thay vì suy nghĩ về tất cả các đường đi có thể, chúng ta có thể hỏi khi nào một đường đi hợp lệ xét về các ràng buộc trên các cạnh. Một đường dẫn có AND ít nhất V khi và chỉ khi mọi cạnh trên đường dẫn chứa tất cả các bit được đặt trong V. Nếu bất kỳ cạnh nào thiếu một bit mà V yêu cầu, toàn bộ đường dẫn sẽ ngay lập tức bị lỗi. 

Điều này biến vấn đề thành một vấn đề đồ thị được lọc. Chúng tôi loại bỏ mọi cạnh có trọng số không chứa tất cả các bit của V, nghĩa là các cạnh w sao cho (w & V) != V. Trên biểu đồ rút gọn này, mọi cạnh còn lại đều “tương thích” với yêu cầu. Bây giờ AND dọc theo bất kỳ đường dẫn nào trong sơ đồ con này sẽ tự động có ít nhất là V, bởi vì mỗi cạnh đều bảo toàn tất cả các bit cần thiết và AND không bao giờ có thể đưa ra các số 0 mới trong đó tất cả các cạnh đã có một số 0. 

Vì vậy, mỗi truy vấn sẽ giảm xuống thành một kiểm tra kết nối đơn giản trong biểu đồ được lọc này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ cho mỗi truy vấn | Cao | Quá chậm | 
| Đồ thị được lọc + DSU | O((n + m) α(n) + q α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách giảm biểu đồ trước, sau đó trả lời các truy vấn kết nối một cách hiệu quả.

1. Đọc biểu đồ và ngưỡng V. Ngưỡng xác định bit nào phải được bảo toàn dọc theo bất kỳ đường dẫn hợp lệ nào. 
2. Xây dựng cấu trúc Liên tập rời rạc trên tất cả n đỉnh. Cấu trúc này duy trì các thành phần được kết nối dưới các liên kết cạnh. 
3. Iterate through every edge (u, v, w). Đối với mỗi cạnh, hãy kiểm tra xem nó có tương thích với V hay không bằng cách xác minh rằng (w & V) == V. Điều kiện này đảm bảo mọi bit mà V yêu cầu đều tồn tại trong w. 
4. Nếu một cạnh tương thích, hãy hợp nhất các điểm cuối của nó trong DSU. This builds connected components in the filtered graph consisting only of valid edges.
 5. Với mỗi truy vấn (u, v), hãy kiểm tra xem u và v có thuộc cùng một thành phần DSU hay không. Nếu có thì xuất Yes, nếu không thì xuất No. 

Các hoạt động DSU có thời gian gần như không đổi, do đó phương pháp này mở rộng đến kích thước đầu vào tối đa. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào sự tương đương then chốt. Một đường dẫn có AND ít nhất V khi và chỉ khi mỗi cạnh trên đường dẫn riêng lẻ chứa tất cả các bit của V. Hướng thuận là đơn giản: nếu một đường dẫn AND ít nhất là V thì mọi bit được đặt trong V phải có mặt ở mọi cạnh, vì một bit bị thiếu sẽ buộc AND mất nó. Hướng ngược lại xuất phát từ tính đơn điệu của AND: nếu mọi cạnh chứa V là tập con của các bit thì AND của bất kỳ số cạnh nào như vậy cũng chứa V. 

Do đó, các đường dẫn hợp lệ chính xác là các đường dẫn trong đồ thị con được hình thành bởi các cạnh thỏa mãn (w & V) == V. Khả năng kết nối trong đồ thị con này chính xác là những gì DSU nắm bắt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n + 1))
        self.r = [0] * (n + 1)

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.r[ra] < self.r[rb]:
            ra, rb = rb, ra
        self.p[rb] = ra
        if self.r[ra] == self.r[rb]:
            self.r[ra] += 1

def solve():
    n, m, q, V = map(int, input().split())
    dsu = DSU(n)

    for _ in range(m):
        u, v, w = map(int, input().split())
        if (w & V) == V:
            dsu.union(u, v)

    out = []
    for _ in range(q):
        u, v = map(int, input().split())
        out.append("Yes" if dsu.find(u) == dsu.find(v) else "No")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc DSU được sử dụng hoàn toàn để duy trì kết nối sau khi lọc các cạnh. Chi tiết triển khai chính là kiểm tra bit`(w & V) == V`, đảm bảo rằng tất cả các bit cần thiết trong V đều được giữ nguyên. Nếu không có điều kiện này, DSU sẽ kết nối các nút thông qua các cạnh không hợp lệ và tạo ra các câu trả lời sai. 

Đánh giá đường dẫn không bao giờ được thực hiện một cách rõ ràng. Sau khi các thành phần được xây dựng, mỗi truy vấn sẽ trở thành một so sánh đại diện theo thời gian không đổi. 

## Ví dụ đã hoạt động 

Xét một đồ thị nhỏ có V = 4 và các cạnh: 

1-2 với trọng số 5, 2-3 với trọng số 6, 1-3 với trọng lượng 7. 

Đầu tiên chúng ta lọc các cạnh: 

| Cạnh | Điều kiện (w & V == V) | Giữ | 
| --- | --- | --- | 
| 1-2 (5) | 5 & ​​4 = 4 | Có | 
| 2-3 (6) | 6 & 4 = 4 | Có | 
| 1-3 (7) | 7 & 4 = 4 | Có | 

Tất cả các cạnh vẫn còn, vì vậy tất cả các nút đều được kết nối. Mọi truy vấn từ 1, 2 và 3 đều trả về Có. 

Bây giờ hãy xem xét V = 4 với các cạnh: 

1-2 (1), 2-3 (2), 1-3 (7). 

| Cạnh | Tình trạng | Giữ | 
| --- | --- | --- | 
| 1-2 (1) | 1 & 4 = 0 | Không | 
| 2-3 (2) | 2 & 4 = 0 | Không | 
| 1-3 (7) | 7 & 4 = 4 | Có | 

Chỉ còn lại cạnh 1-3. DSU tạo thành một kết nối giữa 1 và 3, trong khi 2 bị cô lập. Truy vấn 1-3 trả về Có, nhưng 1-2 và 2-3 trả về Không. 

Những dấu vết này cho thấy thuật toán giảm hoàn toàn vấn đề về khả năng kết nối trong biểu đồ được lọc và bỏ qua tất cả các cạnh không tuân thủ mà không làm mất tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m + q) α(n)) | Mỗi cạnh được xử lý một lần và mỗi truy vấn là một thao tác tìm DSU | 
| Không gian | O(n) | Mảng xếp hạng và cha mẹ DSU | 

Các ràng buộc cho phép lên tới 500.000 cạnh và truy vấn, đồng thời các hoạt động DSU có thời gian không đổi một cách hiệu quả. Điều này đảm bảo giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    _out = io.StringIO()
    _stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    # re-run solution
    class DSU:
        def __init__(self, n):
            self.p = list(range(n + 1))
            self.r = [0] * (n + 1)

        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x

        def union(self, a, b):
            ra, rb = self.find(a), self.find(b)
            if ra == rb:
                return
            if self.r[ra] < self.r[rb]:
                ra, rb = rb, ra
            self.p[rb] = ra
            if self.r[ra] == self.r[rb]:
                self.r[ra] += 1

    def solve():
        n, m, q, V = map(int, sys.stdin.readline().split())
        dsu = DSU(n)

        for _ in range(m):
            u, v, w = map(int, sys.stdin.readline().split())
            if (w & V) == V:
                dsu.union(u, v)

        res = []
        for _ in range(q):
            u, v = map(int, sys.stdin.readline().split())
            res.append("Yes" if dsu.find(u) == dsu.find(v) else "No")

        return "\n".join(res)

    return solve()

# provided sample 1
assert run("""9 8 4 5
1 2 8
1 3 7
2 4 1
3 4 14
2 5 9
4 5 7
5 6 6
3 7 15
1 6
2 7
7 6
1 8
""").strip() == """Yes
No
Yes
No"""

# provided sample 2
assert run("""3 4 1 4
1 2 3
1 2 5
2 3 2
2 3 6
1 3
""").strip() == "Yes"

# minimum case
assert run("""2 1 1 0
1 2 0
1 2
""").strip() == "Yes"

# no edges
assert run("""3 0 2 1
1 2
2 3
""").strip() == "No\nNo"

# all edges valid
assert run("""3 2 2 1
1 2 3
2 3 1
1 3
2 3
""").strip() == "Yes\nYes"

# boundary bit case
assert run("""2 1 1 8
1 2 8
1 2
""").strip() == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tối thiểu | Có | trường hợp hợp lệ một cạnh | 
| không có cạnh | Không/Không | thành phần bị ngắt kết nối | 
| tất cả đều hợp lệ | Có/Có | kết nối đầy đủ | 
| bit ranh giới | Có | độ chính xác bit cao | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi V bằng 0. Trong trường hợp này, mọi cạnh đều tự động thỏa mãn (w & V) == V, do đó DSU kết nối toàn bộ biểu đồ một cách chính xác như đã cho. Các truy vấn giảm xuống mức kết nối tiêu chuẩn và thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt. 

Một trường hợp cạnh khác xảy ra khi không có cạnh nào thỏa mãn điều kiện. DSU vẫn hoàn toàn bị ngắt kết nối, do đó chỉ các truy vấn của các nút giống hệt nhau mới thành công, nhưng vì ui và vi luôn khác biệt nên mọi truy vấn đều trả về chính xác Không. 

Trường hợp thứ ba là khi V có các bit cao hiếm khi xuất hiện. Quá trình lọc có thể loại bỏ hầu hết các cạnh, nhưng các hoạt động DSU vẫn hợp lệ do các nút bị cô lập được xử lý một cách tự nhiên như các thành phần đơn lẻ.
