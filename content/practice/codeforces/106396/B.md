---
title: "CF 106396B - \u732b"
description: "Chúng ta được cho một đồ thị vô hướng có trọng số. Mỗi cạnh nối hai đỉnh và mang một chi phí. Nhiệm vụ là chọn một tập hợp các cạnh kết nối tất cả các đỉnh lại với nhau, tạo thành một cấu trúc kết nối duy nhất, đồng thời tối đa hóa tổng trọng số của các cạnh đã chọn."
date: "2026-06-19T18:05:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106396
codeforces_index: "B"
codeforces_contest_name: "Tiangong University 2025 ICPC Team Selection Contest II (Online Mirror)"
rating: 0
weight: 106396
solve_time_s: 57
verified: true
draft: false
---

[CF 106396B - \u732b](https://codeforces.com/problemset/problem/106396/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số. Mỗi cạnh nối hai đỉnh và mang một chi phí. Nhiệm vụ là chọn một tập hợp các cạnh kết nối tất cả các đỉnh lại với nhau, tạo thành một cấu trúc kết nối duy nhất, đồng thời tối đa hóa tổng trọng số của các cạnh đã chọn. Nếu không thể kết nối tất cả các đỉnh thì câu trả lời phải được báo cáo là không thể. 

Một cách khác để xem vấn đề là chúng ta muốn một cây bao trùm có tổng trọng lượng cạnh tối đa. Cây bao trùm là tập hợp con các cạnh nối mọi đỉnh và không chứa chu trình. Trong số tất cả các cấu trúc hợp lệ như vậy, chúng ta muốn cấu trúc có tổng trọng số lớn nhất. 

Kích thước đầu vào đủ lớn để bất kỳ giải pháp nào cố gắng liệt kê các tập hợp con của các cạnh hoặc tính toán lại kết nối nhiều lần sẽ quá chậm. Với giới hạn Codeforces điển hình cho đồ thị, cách tiếp cận bậc hai hoặc hàm mũ sẽ bị loại trừ ngay lập tức. Thậm chí một$O(n^2)$phương pháp sẽ quá chậm khi số đỉnh lớn và đồ thị dày đặc, vì số cạnh có thể tăng lên$O(n^2)$trong những trường hợp xấu nhất. 

Ý nghĩa chính của ràng buộc là chúng ta cần một phương pháp gần tuyến tính hoặc logarit cho mỗi thao tác để kiểm tra kết nối. Điều này trực tiếp gợi ý một cấu trúc kết hợp tập hợp rời rạc, vì nó hỗ trợ các hoạt động kết hợp và tìm kiếm nhanh chóng. 

Trường hợp cạnh tinh tế phát sinh khi đồ thị bị ngắt kết nối. Ví dụ: nếu chúng ta có 4 nút và chỉ có các cạnh (1,2) và (3,4) thì không có cách nào để tạo thành một cây khung duy nhất. Một lựa chọn tham lam ngây thơ vẫn có thể chọn cả hai cạnh và đưa ra tổng, nhưng kết quả đầu ra đúng là -1 vì cấu trúc không được kết nối đầy đủ. 

Một trường hợp góc khác là khi có nhiều cạnh tồn tại giữa cùng một cặp đỉnh. Việc triển khai bất cẩn có thể xử lý chúng giống hệt nhau, nhưng thuật toán chính xác phải xem xét chúng một cách độc lập vì chỉ có sự kết hợp tốt nhất mới góp phần tạo ra cây bao trùm tối ưu. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là thử mọi tập con của các cạnh, kiểm tra xem nó có tạo thành cây bao trùm hay không và tính tổng trọng số của nó. Việc kiểm tra kết nối có thể được thực hiện bằng BFS hoặc DFS và xác minh rằng có chính xác$n-1$các cạnh đảm bảo tính chu kỳ. Cách tiếp cận này đúng vì nó trực tiếp kiểm tra mọi cấu trúc ứng viên hợp lệ. Tuy nhiên, số tập con cạnh là$2^m$và mỗi chi phí kiểm tra tính hợp lệ$O(n + m)$, làm cho độ phức tạp tổng thể$O(2^m \cdot (n + m))$, điều này không thể thực hiện được ngay cả đối với các đồ thị nhỏ vừa phải. 

Quan sát cấu trúc quan trọng là chúng ta đang giải bài toán cây bao trùm cực đại. Vấn đề giảm xuống việc lựa chọn chính xác$n-1$các cạnh kết nối tất cả các đỉnh trong khi tối đa hóa tổng trọng lượng. Điều này giống như bài toán cây bao trùm tối thiểu cổ điển, ngoại trừ việc chúng ta đảo ngược thứ tự để tối đa hóa thay vì giảm thiểu. 

Thuật toán Kruskal cung cấp cấu trúc cần thiết. Thay vì sắp xếp các cạnh theo thứ tự tăng dần về trọng lượng, chúng ta sắp xếp chúng theo thứ tự giảm dần. Sau đó, chúng tôi lặp qua các cạnh và sử dụng tập hợp rời rạc để chỉ chấp nhận các cạnh kết nối các thành phần đã bị ngắt kết nối trước đó. Mỗi cạnh được chấp nhận hợp nhất hai thành phần và đóng góp vào tổng trọng lượng. Sự lựa chọn tham lam này có hiệu quả vì bất kỳ cạnh nào kết nối hai thành phần đã được kết nối sẽ tạo ra một chu trình và không thể cải thiện cấu trúc cây bao trùm, trong khi các cạnh nặng nhất hiện có phải được ưu tiên để tối đa hóa tổng. 

Cuối cùng, nếu chúng ta chưa sử dụng chính xác$n-1$các cạnh, đồ thị bị ngắt kết nối và không tồn tại cây bao trùm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^m \cdot (n + m))$|$O(n + m)$| Quá chậm | 
| Kruskal (DSU) |$O(m \log m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đỉnh và cạnh, sau đó lưu trữ tất cả các cạnh dưới dạng bộ ba chứa điểm cuối và trọng số. Điều này chuẩn bị dữ liệu để sắp xếp theo trọng số, vốn là cốt lõi của chiến lược tham lam. 
2. Sắp xếp tất cả các cạnh theo thứ tự trọng lượng giảm dần. Lý do là vì chúng ta muốn xem xét các cạnh nặng hơn trước để chúng có cơ hội được đưa vào cây bao trùm cao nhất. 
3. Khởi tạo cấu trúc hợp tập hợp rời rạc trong đó mỗi đỉnh bắt đầu trong thành phần riêng của nó. Cấu trúc này sẽ cho phép chúng ta nhanh chóng xác định xem việc thêm một cạnh sẽ kết nối hai thành phần riêng biệt hay tạo ra một chu trình. 
4. Lặp lại các cạnh theo thứ tự được sắp xếp. Đối với mỗi cạnh, hãy kiểm tra xem điểm cuối của nó có thuộc về các thành phần khác nhau hay không bằng cách sử dụng thao tác tìm DSU. Nếu chúng khác nhau, chúng tôi hợp nhất chúng và thêm trọng số cạnh vào câu trả lời. Nếu chúng đã có trong cùng một thành phần, chúng ta bỏ qua vì nó sẽ tạo thành một chu trình. 
5. Duy trì bộ đếm xem còn lại bao nhiêu thành phần. Mỗi lần hợp nhất thành công sẽ giảm số lượng thành phần đi một. 
6. Sau khi xử lý tất cả các cạnh, hãy kiểm tra xem chỉ còn lại một thành phần được kết nối hay không. Nếu không, điều đó có nghĩa là biểu đồ không thể kết nối hoàn toàn, do đó không thể hình thành cây bao trùm. 
7. Nếu biểu đồ được kết nối, hãy xuất tổng trọng lượng tích lũy. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán sẽ duy trì một rừng các thành phần được kết nối. Mỗi cạnh được chọn là cạnh tối đa có thể kết nối hai thành phần riêng biệt tại thời điểm đó. Điều này đảm bảo rằng không có cạnh nào được chọn có thể được thay thế bằng một cạnh thay thế hợp lệ nặng hơn mà không làm đứt kết nối. Vì các chu trình không bao giờ được đưa vào nên cấu trúc luôn là một khu rừng và khi tất cả các đỉnh được kết nối, các cạnh được chọn sẽ tạo thành một cây bao trùm với tổng trọng số tối đa có thể. 

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
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]
        return True

def solve():
    n, m = map(int, input().split())
    edges = []

    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((u - 1, v - 1, w))

    edges.sort(key=lambda x: x[2], reverse=True)

    dsu = DSU(n)
    total = 0
    used = 0

    for u, v, w in edges:
        if dsu.union(u, v):
            total += w
            used += 1

    if used != n - 1:
        print(-1)
    else:
        print(total)

if __name__ == "__main__":
    solve()
```Cấu trúc DSU được sử dụng để duy trì các thành phần được kết nối một cách hiệu quả. Nén đường dẫn đảm bảo thời gian khấu hao gần như không đổi cho mỗi thao tác, giúp thuật toán luôn nhanh chóng ngay cả đối với các biểu đồ lớn. Sắp xếp theo thứ tự giảm dần đảm bảo rằng các cạnh nặng hơn sẽ được xem xét đầu tiên, điều này rất cần thiết để tối đa hóa tổng trọng lượng. 

Biến`used`theo dõi bao nhiêu cạnh đã được bao gồm thành công. Một cây bao trùm hợp lệ$n$các nút phải chứa chính xác$n-1$các cạnh, vì vậy đây là lần kiểm tra tính chính xác cuối cùng. 

## Ví dụ đã hoạt động 

Xét một biểu đồ nhỏ có 4 nút và cạnh: (1-2, 10), (2-3, 5), (3-4, 7), (1-4, 1). 

Sau khi sắp xếp theo thứ tự giảm dần, các cạnh trở thành: (1-2, 10), (3-4, 7), (2-3, 5), (1-4, 1). 

| Bước | Cạnh | Thành phần trước | Hành động | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1-2 (10) | {1}{2}{3}{4} | hợp nhất | 10 | 
| 2 | 3-4 (7) | {1,2}{3}{4} | hợp nhất | 17 | 
| 3 | 2-3 (5) | {1,2}{3,4} | hợp nhất | 22 | 
| 4 | 1-4 (1) | {1,2,3,4} | bỏ qua | 22 | 

Dấu vết này cho thấy cách thuật toán xây dựng cây bao trùm trong khi luôn chọn cạnh kết nối có sẵn cao nhất. 

Bây giờ hãy xem xét một biểu đồ bị ngắt kết nối: 3 nút chỉ có cạnh (1-2, 5). 

| Bước | Cạnh | Thành phần trước | Hành động | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1-2 (5) | {1}{2}{3} | hợp nhất | 5 | 

Sau khi xử lý, nút 3 vẫn bị cô lập nên kết quả không hợp lệ và đầu ra trở thành -1. 

Điều này chứng tỏ rằng việc kiểm tra kết nối cuối cùng là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| Các cạnh sắp xếp chiếm ưu thế, hoạt động DSU được khấu hao gần như không đổi | 
| Không gian |$O(n + m)$| Lưu trữ cho mảng DSU và danh sách cạnh | 

Độ phức tạp phù hợp một cách thoải mái trong các ràng buộc điển hình đối với các biểu đồ lớn vì việc sắp xếp có hiệu quả lên tới hàng trăm nghìn cạnh và các hoạt động DSU có quy mô gần như tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.s = [1] * n

        def f(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x

        def u(self, a, b):
            a = self.f(a)
            b = self.f(b)
            if a == b:
                return False
            if self.s[a] < self.s[b]:
                a, b = b, a
            self.p[b] = a
            self.s[a] += self.s[b]
            return True

    n, m = map(int, input().split())
    e = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        e.append((u - 1, v - 1, w))
    e.sort(key=lambda x: -x[2])

    d = DSU(n)
    ans = 0
    used = 0

    for u, v, w in e:
        if d.u(u, v):
            ans += w
            used += 1

    return str(ans if used == n - 1 else -1)

# provided sample-like tests
assert run("4 4\n1 2 10\n2 3 5\n3 4 7\n1 4 1\n") == "22"
assert run("3 1\n1 2 5\n") == "-1"

# custom tests
assert run("1 0\n") == "0"
assert run("2 1\n1 2 100\n") == "100"
assert run("3 3\n1 2 1\n2 3 1\n1 3 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút, không có cạnh | 0 | cây bao trùm một đỉnh | 
| 2 nút, một cạnh | 100 | trường hợp kết nối đơn giản nhất | 
| đồ thị tam giác | 2 | xử lý chu trình trong cây bao trùm tối đa | 

## Vỏ cạnh 

Đối với một đỉnh duy nhất, DSU bắt đầu với một thành phần và không cần có cạnh nào. Thuật toán không bao giờ đi vào vòng lặp một cách có ý nghĩa và`used`vẫn bằng không. Từ$n-1 = 0$, điều kiện được thỏa mãn và câu trả lời là 0, khớp với định nghĩa đúng về cây bao trùm trống. 

Đối với biểu đồ bị ngắt kết nối, chẳng hạn như các nút 1-2 được kết nối và nút 3 bị cô lập, thuật toán chỉ hợp nhất các thành phần có thể truy cập được. DSU kết thúc bằng hai thành phần, vì vậy`used`trở thành 1 lúc$n-1 = 2$. Kiểm tra cuối cùng loại bỏ chính xác kết quả và đưa ra -1, phù hợp với việc không thể bao trùm tất cả các đỉnh.
