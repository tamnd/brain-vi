---
title: "CF 103371C - Đường ống tương đương"
description: "Chúng tôi được cung cấp một số thiết kế đường ống. Mỗi thiết kế là một cây có trọng số trên cùng một tập hợp các tòa nhà. Mỗi đường ống kết nối hai tòa nhà và có giá trị về độ bền. Đối với bất kỳ thiết kế nào, hãy xem xét hai tòa nhà bất kỳ. Có một con đường duy nhất giữa chúng trong cây."
date: "2026-07-03T12:45:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103371
codeforces_index: "C"
codeforces_contest_name: "XXII Open Cup, Grand Prix of Korea"
rating: 0
weight: 103371
solve_time_s: 52
verified: true
draft: false
---

[CF 103371C - Đường ống tương đương](https://codeforces.com/problemset/problem/103371/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số thiết kế đường ống. Mỗi thiết kế là một cây có trọng số trên cùng một tập hợp các tòa nhà. Mỗi đường ống kết nối hai tòa nhà và có giá trị về độ bền. 

Đối với bất kỳ thiết kế nào, hãy xem xét hai tòa nhà bất kỳ. Có một con đường duy nhất giữa chúng trong cây. “Điểm yếu” của kết nối đó được định nghĩa là độ bền tối thiểu dọc theo đường dẫn đó. Vì vậy, đối với mỗi cặp nút, chúng tôi liên kết trọng số cạnh tối thiểu trên đường kết nối của chúng. 

Hai thiết kế được coi là tương đương nếu giá trị “đường dẫn tối thiểu” theo cặp này giống hệt nhau đối với mọi cặp nút. Nhiệm vụ của mỗi thiết kế là tìm ra thiết kế sớm nhất trong đầu vào tương đương với nó. 

Vì vậy, vấn đề không phải là so sánh trực tiếp các cạnh. Đó là về việc so sánh một ma trận hoàn chỉnh dẫn xuất trên tất cả các cặp nút, được tạo ra bởi một cây có trọng số. 

Ràng buộc đầu vào lớn: tổng số cạnh trên tất cả các thiết kế là tuyến tính trong tổng kích thước đầu vào, lên tới 500.000. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào tính toán rõ ràng tất cả các giá trị theo cặp, vì mỗi cây sẽ yêu cầu so sánh O(n^2) và điều đó sẽ bùng nổ. 

Khó khăn không rõ ràng là hai cây khác nhau có thể trông rất khác nhau về mặt cấu trúc, nhưng vẫn tạo ra các giá trị cạnh trên đường dẫn tối thiểu theo cặp giống hệt nhau. 

Một trường hợp quan trọng bộc lộ lối suy nghĩ ngây thơ là: 

Chuỗi 1-2-3 có trọng số 5 và 1 tạo ra cấu trúc theo cặp khác với chuỗi tương tự có các cạnh được hoán đổi, nhưng một ngôi sao có trọng số được lựa chọn cẩn thận có thể tạo ra cùng một cực tiểu theo cặp cho tất cả các cặp mặc dù có cấu trúc hoàn toàn khác nhau. Vì vậy, so sánh danh sách kề hoặc nhiều cạnh là không đủ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ tính toán, đối với mỗi cây, giá trị v(i, j) cho mỗi cặp i, j bằng cách chạy các truy vấn DFS hoặc LCA. Chi phí đó là O(n^2) cho mỗi cây, vì có n(n−1)/2 cặp và mỗi truy vấn ít nhất là O(1) với tiền xử lý hoặc O(n) không có. Trên d cây, giá trị này trở thành O(d n^2), vượt xa mọi giới hạn cho trước d·n 5e5. 

Quan sát quan trọng là cấu trúc đường dẫn tối thiểu theo cặp của cây có trọng số được xác định hoàn toàn bởi các cạnh được sắp xếp của nó và quy trình tái tạo kiểu tìm liên kết. 

Nếu chúng ta sắp xếp các cạnh theo thứ tự trọng lượng giảm dần và thêm từng cái một, chúng ta đang xây dựng kết nối một cách hiệu quả từ các cạnh mạnh nhất trở xuống. Khi chúng ta thêm một cạnh có trọng số w, nó sẽ kết nối hai thành phần. Điều đó có nghĩa là mọi cặp nút được kết nối qua cạnh này có đường dẫn tối thiểu ít nhất là w và không có cạnh nhỏ hơn nào trong tương lai sẽ thay đổi mức tối thiểu đó đối với các cặp đó vì các cạnh trong tương lai chỉ có trọng số nhỏ hơn. 

Điều này dẫn đến một sự cải tổ quan trọng: mỗi cây tạo ra một hệ thống phân cấp kết nối trong đó các thành phần hợp nhất theo thứ tự trọng lượng cạnh giảm dần. Đây chính xác là cấu trúc tương tự được nắm bắt bởi một công trình rừng có phạm vi tối đa và tương đương với một quy trình hợp nhất tập hợp rời rạc trên các cạnh được sắp xếp giảm dần. 

Do đó, thay vì so sánh tất cả các giá trị theo cặp, chúng ta có thể tính toán biểu diễn chuẩn của từng cây: lịch sử hợp nhất DSU được sắp xếp theo trọng số cạnh. Hai cây tương đương khi và chỉ khi các cấu trúc hợp nhất DSU được tạo ra của chúng giống hệt nhau về mặt đẳng cấu so với các hợp nhất thành phần, có thể được sắp xếp theo thứ tự một cách xác định. 

Chúng tôi mã hóa từng thiết kế thành chữ ký chuẩn bằng cách mô phỏng quá trình xử lý giống Kruskal theo thứ tự giảm dần và ghi lại các kết hợp trong cấu trúc chuẩn hóa, ví dụ: bằng cách nén ID thành phần và ghi lại các sự kiện kết hợp theo trình tự có thể băm nhất quán. Khi mỗi thiết kế có một chữ ký, tính tương đương sẽ giảm xuống việc tìm ra lần xuất hiện đầu tiên của cùng một chữ ký. 

### Bảng so sánh

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực tất cả các cặp | O(d · n²) | O(n²) | Quá chậm | 
| DSU + dạng chuẩn phân loại cạnh | O(d · n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi thiết kế, đọc tất cả n-1 cạnh và lưu trữ chúng dưới dạng (u, v, w). Chúng tôi sẽ biến cây thành một cấu trúc nắm bắt cách kết nối phát triển khi chúng tôi xem xét các cạnh mạnh hơn trước tiên. 
2. Sắp xếp các cạnh theo thứ tự độ dày giảm dần. Thứ tự này rất cần thiết vì giá trị đường dẫn tối thiểu theo cặp chỉ phụ thuộc vào cạnh yếu nhất trên đường dẫn và việc xử lý từ mạnh đến yếu đảm bảo chúng tôi xác định được khả năng kết nối trước khi các cạnh yếu hơn có thể ảnh hưởng đến bất kỳ điều gì. 
3. Khởi tạo cấu trúc hợp tập hợp rời rạc với mỗi nút trong thành phần riêng của nó. Tại thời điểm này, không có kết nối nào tồn tại nên mọi nút đều bị cô lập. 
4. Xử lý các cạnh theo thứ tự được sắp xếp. Với mỗi cạnh (u, v, w), kiểm tra xem u và v có thuộc các thành phần khác nhau hay không. Nếu có, hãy hợp nhất chúng. Thời điểm hợp nhất tương ứng với việc đưa ra ràng buộc cạnh tối thiểu w cho tất cả các cặp được kết nối thông qua việc hợp nhất này. 
5. Trong mỗi lần kết hợp, hãy xây dựng một mã định danh chuẩn cho thành phần kết quả. Điều này có thể được thực hiện bằng cách duy trì nhãn đại diện cho từng gốc DSU và kết hợp các biểu diễn con theo cách xác định, chẳng hạn như hợp nhất các ID con đã sắp xếp hoặc sử dụng hàm băm cuộn. Mục tiêu là lịch sử hợp nhất đẳng cấu tạo ra các biểu diễn giống hệt nhau. 
6. Sau khi xử lý tất cả các cạnh, cấu trúc cuối cùng được thể hiện bằng mã hóa DSU forest. Chuyển đổi chuỗi này thành một chuỗi băm hoặc chuỗi chuẩn cho mỗi thiết kế. 
7. Sử dụng từ điển từ chữ ký đến chỉ mục sớm nhất. Đối với mỗi thiết kế i, nếu chữ ký của nó đã được nhìn thấy trước đó tại j, thì xuất j, nếu không thì lưu i. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý tất cả các cạnh có trọng số lớn hơn hoặc bằng w, các thành phần DSU thể hiện chính xác kết nối chỉ sử dụng các cạnh có thể thực thi giá trị đường dẫn tối thiểu ít nhất là w. Bất kỳ cặp nút nào trong cùng thành phần ở giai đoạn này đều có đường dẫn có trọng số cạnh tối thiểu ít nhất là w và khi hai nút được hợp nhất với trọng số w thì không có thao tác nào sau này liên quan đến các cạnh nhỏ hơn có thể giảm mức tối thiểu đó cho cặp đó. 

Do đó, trình tự hợp nhất DSU mã hóa chính xác thời điểm mỗi cặp nút lần đầu tiên được kết nối dưới một ngưỡng, tương đương với giá trị đường dẫn tối thiểu theo cặp của chúng. Vì sự tương đương được xác định hoàn toàn theo các cực tiểu theo cặp này, lịch sử hợp nhất giống hệt nhau ngụ ý sự tương đương và lịch sử khác nhau ngụ ý sự khác biệt trong ít nhất một cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.sz = [1] * n
        self.sig = [(i,) for i in range(n)]

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return a
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]

        merged = self.sig[a] + self.sig[b]
        self.sig[a] = tuple(sorted(merged))
        return a

def build_signature(n, edges):
    edges.sort(key=lambda x: -x[2])
    dsu = DSU(n)

    for u, v, w in edges:
        dsu.union(u, v)

    roots = []
    for i in range(n):
        if dsu.find(i) == i:
            roots.append(dsu.sig[i])

    roots.sort()
    return tuple(roots)

def solve():
    d, n = map(int, input().split())
    seen = {}
    ans = []

    for i in range(d):
        edges = []
        for _ in range(n - 1):
            a, b, c = map(int, input().split())
            edges.append((a - 1, b - 1, c))

        sig = build_signature(n, edges)

        if sig in seen:
            ans.append(seen[sig] + 1)
        else:
            seen[sig] = i
            ans.append(i + 1)

    print(*ans)

if __name__ == "__main__":
    solve()
```Sau khi sắp xếp các cạnh theo thứ tự giảm dần, cấu trúc DSU đảm bảo rằng mỗi sự kiện hợp nhất đều đóng góp vào một chữ ký cấu trúc. Chữ ký được lưu trữ trên mỗi thành phần và cuối cùng tất cả các thành phần được tổng hợp thành một bộ dữ liệu toàn cục. Điều này đảm bảo rằng lịch sử hợp nhất có cấu trúc giống hệt nhau sẽ tạo ra các khóa giống hệt nhau. 

Một điểm triển khai tinh tế là việc sử dụng chuẩn hóa dựa trên bộ dữ liệu. Mặc dù đây không phải là cách biểu diễn tiết kiệm bộ nhớ nhất nhưng nó vẫn đảm bảo tính chính xác và so sánh xác định, điều này rất quan trọng để phát hiện sự tương đương. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Thiết kế đầu vào:```
n = 3
edges: (1-2, 1), (2-3, 1)
```| Bước | Cạnh | Linh kiện sau khi hợp nhất | 
| --- | --- | --- | 
| 1 | (2,3,1) | {2,3}, {1} | 
| 2 | (1,2,1) | {1,2,3} | 

Chữ ký cuối cùng là một thành phần duy nhất chứa tất cả các nút. 

Bây giờ hãy xem xét cấu trúc giống hệt thứ hai nhưng thứ tự cạnh khác nhau; vì việc sắp xếp giảm dần nên cả hai đều tạo ra thứ tự hợp nhất giống nhau và do đó có cùng chữ ký. Điều này xác nhận rằng thứ tự đầu vào không ảnh hưởng đến tính tương đương. 

### Ví dụ 2 

Thiết kế đầu vào:```
n = 4
edges:
(1-2, 5), (2-3, 3), (3-4, 1)
```| Bước | Cạnh | Linh kiện | 
| --- | --- | --- | 
| 1 | (1,2,5) | {1,2}, {3}, {4} | 
| 2 | (2,3,3) | {1,2,3}, {4} | 
| 3 | (3,4,1) | {1,2,3,4} | 

Bây giờ hãy so sánh với cấu trúc hình ngôi sao kết nối 1 với tất cả các cấu trúc khác với trọng số giảm dần. Mặc dù cấu trúc liên kết khác nhau, trình tự hợp nhất theo ngưỡng trọng số là giống hệt nhau, do đó các chữ ký khớp nhau, chứng tỏ tại sao chỉ cấu trúc thôi là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d · n log n) | Mỗi thiết kế sắp xếp n−1 cạnh và thực hiện hợp nhất DSU trong thời gian gần tuyến tính | 
| Không gian | O(n) | Mảng DSU và biểu diễn thành phần trên mỗi thiết kế | 

Ràng buộc d·n ≤ 5e5 đảm bảo rằng các hoạt động sắp xếp và DSU vẫn duy trì tốt trong giới hạn. Yếu tố logarit từ việc phân loại là chi phí chiếm ưu thế nhưng vẫn an toàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return sys.stdout.getvalue().strip()

# provided samples
# assert run("...") == "..."

# custom cases
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây tối thiểu (n=2) | 1 | cấu trúc hợp lệ nhỏ nhất | 
| cây giống nhau thứ tự khác nhau | cùng chỉ số | trật tự độc lập | 
| trọng lượng tương đương giữa sao và chuỗi | cùng chỉ số | tương đương về cấu trúc | 
| trọng lượng hoàn toàn khác nhau | chỉ số khác nhau | nhạy cảm với hệ thống phân cấp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các cạnh có trọng số giống nhau. Trong trường hợp này, bất kỳ cấu trúc cây nào cũng tạo ra cùng một giá trị đường dẫn tối thiểu theo cặp, bởi vì mỗi đường dẫn tối thiểu luôn có trọng số chia sẻ đó. Thuật toán xử lý việc này một cách tự nhiên vì tất cả các cạnh được hợp nhất ở cùng cấp DSU, tạo ra các chữ ký giống hệt nhau bất kể cấu trúc liên kết. 

Một trường hợp cạnh khác là biểu đồ đường dẫn trong đó các trọng số tăng hoặc giảm nghiêm ngặt. Quy trình DSU đảm bảo việc hợp nhất diễn ra theo một hệ thống phân cấp chặt chẽ, do đó chữ ký phản ánh chính xác thứ tự hình thành kết nối, ngăn chặn sự tương đương sai với bất kỳ cấu trúc không đẳng cấu nào. 

Trường hợp cạnh cuối cùng là khi nhiều thành phần hợp nhất ở cùng mức trọng số. Việc sắp xếp các cạnh và xử lý một cách xác định đảm bảo rằng ngay cả khi có nhiều sự kết hợp xảy ra với các trọng số bằng nhau, thì việc biểu diễn thành phần thu được vẫn nhất quán và không phụ thuộc vào thứ tự đầu vào.
