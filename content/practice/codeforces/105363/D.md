---
title: "CF 105363D - Kết nối làng nghề"
description: "Chúng tôi có một mạng lưới các ngôi làng được kết nối bằng đường, trong đó mỗi con đường ban đầu không thể sử dụng được và chỉ có thể sử dụng được sau một số giờ nhất định. Mọi con đường đều “mở khóa” song song theo lịch trình riêng. Khi một con đường được mở khóa, nó có thể được sử dụng vĩnh viễn."
date: "2026-06-23T15:54:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105363
codeforces_index: "D"
codeforces_contest_name: "XXV Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105363
solve_time_s: 69
verified: true
draft: false
---

[CF 105363D - Kết nối các ngôi làng](https://codeforces.com/problemset/problem/105363/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mạng lưới các ngôi làng được kết nối bằng đường, trong đó mỗi con đường ban đầu không thể sử dụng được và chỉ có thể sử dụng được sau một số giờ nhất định. Mọi con đường đều “mở khóa” song song theo lịch trình riêng. Khi một con đường được mở khóa, nó có thể được sử dụng vĩnh viễn. 

Câu hỏi đặt ra là xác định thời điểm sớm nhất khi các ngôi làng có thể tiếp cận hoàn toàn với nhau chỉ bằng những con đường đã được mở khóa xong. Theo thuật ngữ đồ thị, chúng tôi muốn thời gian nhỏ nhất$T$sao cho nếu chúng ta chỉ giữ các cạnh với$h_i \le T$, đồ thị kết quả được kết nối. 

Mỗi trường hợp thử nghiệm đưa ra một biểu đồ vô hướng có trọng số, trong đó các trọng số biểu thị thời gian kích hoạt. Chúng ta được yêu cầu tìm ngưỡng tối thiểu về trọng số các cạnh để làm cho biểu đồ được kết nối một cách hiệu quả. 

Các ràng buộc rất lớn: lên tới$10^5$đỉnh và$5 \cdot 10^5$các cạnh cho mỗi trường hợp thử nghiệm, với tổng số$10^6$. Điều này ngay lập tức loại trừ mọi thứ tính toán lại kết nối từ đầu cho từng thời điểm ứng cử viên, chẳng hạn như chạy liên tục BFS/DFS trên tất cả các cạnh cho mọi thời điểm riêng biệt.$h_i$. Ngay cả việc sắp xếp tất cả các cạnh và mô phỏng nhiều bản dựng lại biểu đồ đầy đủ cho mỗi ngưỡng cũng sẽ quá chậm nếu được thực hiện một cách ngây thơ. 

Chúng ta cũng cần cẩn thận với một trường hợp lỗi tinh vi: kết nối không đơn điệu theo nghĩa xử lý một phần nếu chúng ta không xử lý các cạnh theo thứ tự tăng dần một cách hợp lý. 

Ví dụ, hãy xem xét một hình tam giác:```
0 - 1 (10)
1 - 2 (10)
0 - 2 (100)
```Một cách tiếp cận ngây thơ có thể nghĩ không chính xác rằng chúng ta cần đợi cạnh (0,2), nhưng trên thực tế, các nút đã được kết nối tại thời điểm 10 bằng cách sử dụng hai cạnh đầu tiên. 

Câu trả lời đúng phụ thuộc vào cấu trúc của cách tối thiểu để kết nối tất cả các nút chứ không phụ thuộc vào bất kỳ đường dẫn cụ thể nào giữa một cặp cố định. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ là mô phỏng thời gian tăng dần từ 1 trở lên và ở mỗi bước hãy kiểm tra xem đồ thị được hình thành bởi tất cả các cạnh có$h_i \le T$được kết nối. Mỗi chi phí kiểm tra kết nối$O(n + m)$sử dụng DFS hoặc BFS. Trong trường hợp xấu nhất, chúng tôi có thể quét tới$10^6$các giá trị thời gian khác nhau nếu trọng lượng dày đặc và lớn, gây ra sự cấm đoán$O(m \cdot \max h)$hoặc thậm chí$O(m^2)$-hành vi phong cách tùy thuộc vào việc thực hiện. Điều này vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không thực sự cần phải kiểm tra mọi lúc. Chúng ta chỉ quan tâm đến thời điểm tồn tại đủ số cạnh để nối tất cả các đỉnh. Đó chính xác là thời điểm chúng ta chọn cây khung sử dụng trọng số cạnh tối đa nhỏ nhất có thể. 

Điều này biến vấn đề thành một câu hỏi về cây bao trùm có nút thắt cổ chai tối thiểu cổ điển: trong số tất cả các cây bao trùm, chúng ta muốn cây bao trùm có trọng số cạnh tối thiểu được sử dụng ở mức tối thiểu. Câu trả lời là trọng số cạnh tối đa trong cây bao trùm tối thiểu. 

Một khi chúng ta nhận ra điều này, giải pháp sẽ trở nên đơn giản. Chúng tôi sắp xếp các cạnh theo trọng số và xây dựng cây bao trùm bằng cách sử dụng Disjoint Set Union (DSU). Ngay sau khi chúng ta kết nối tất cả các đỉnh, trọng số cạnh hiện tại là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m \cdot (n + m))$|$O(n + m)$| Quá chậm | 
| Kruskal + DSU |$O(m \log m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng ý tưởng của Kruskal nhưng sẽ dừng sớm khi đạt được kết nối. 

1. Sắp xếp tất cả các con đường theo thời gian mở cửa$h_i$. Điều này đảm bảo rằng chúng tôi luôn xem xét các kết nối sớm nhất có thể trước tiên, điều này là cần thiết vì chúng tôi muốn giảm thiểu thời gian khi biểu đồ được kết nối. 
2. Khởi tạo cấu trúc DSU trong đó mỗi làng bắt đầu bằng thành phần riêng của mình. Ban đầu, không có ngôi làng nào được kết nối. 
3. Lặp lại các cạnh được sắp xếp theo thứ tự tăng dần$h_i$. Đối với mỗi cạnh$(u, v, h_i)$, cố gắng hợp nhất các thành phần có chứa$u$Và$v$. Nếu chúng đã ở trong cùng một thành phần thì cạnh sẽ không thay đổi kết nối. 
4. Sau mỗi lần hợp nhất thành công, hãy theo dõi xem còn lại bao nhiêu thành phần được kết nối. Mỗi liên minh thành công sẽ giảm số lượng này xuống một. Khi số lượng thành phần trở thành một, tất cả các làng đều được kết nối. 
5. Câu trả lời là trọng số của cạnh hiện tại$h_i$, bởi vì đây là thời điểm đầu tiên đồ thị được kết nối hoàn toàn chỉ sử dụng các cạnh cho đến thời điểm này. 

Ý tưởng chính là chúng tôi đang xây dựng một khu rừng bao trùm một cách hiệu quả theo thứ tự tăng trọng lượng cạnh và thời điểm nó trở thành một cây duy nhất chính xác là lúc đạt được kết nối. 

### Tại sao nó hoạt động 

Bất cứ lúc nào$T$, tập các cạnh có thể sử dụng chính xác là tất cả các cạnh có trọng số lớn nhất$T$. Thuật toán của Kruskal xử lý các cạnh theo thứ tự tăng dần và duy trì rừng bao trùm tối thiểu cho tiền tố đó. Thời điểm DSU hợp nhất mọi thứ thành một thành phần, chúng ta đã tìm thấy tiền tố nhỏ nhất của các cạnh kết nối tất cả các đỉnh. 

Nếu khả năng kết nối có thể sớm hơn trọng lượng cạnh hiện tại, điều đó có nghĩa là cây bao trùm tồn tại chỉ sử dụng các cạnh nhỏ hơn, mâu thuẫn với thực tế là Kruskal luôn xây dựng cây bao trùm nút cổ chai tối thiểu. Vì vậy, lần đầu tiên chúng ta tiếp cận một thành phần duy nhất phải là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.components = n

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
        self.components -= 1
        return True

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n, m = map(int, input().split())
        edges = []
        for _ in range(m):
            u, v, w = map(int, input().split())
            edges.append((w, u, v))

        edges.sort()

        dsu = DSU(n)

        ans = 0
        for w, u, v in edges:
            if dsu.union(u, v):
                ans = w
                if dsu.components == 1:
                    break

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```DSU duy trì kết nối động khi chúng tôi xử lý các cạnh theo thứ tự tăng dần. các`components`bộ đếm rất quan trọng vì nó tránh việc phải duyệt toàn bộ biểu đồ để kiểm tra kết nối. Mỗi liên kết thành công sẽ giảm số lượng thành phần được kết nối và khi đạt đến một thành phần, chúng ta biết biểu đồ đã được kết nối đầy đủ. 

Một chi tiết triển khai tinh tế là chúng tôi lưu trữ trọng số cạnh được sử dụng cuối cùng dưới dạng`ans`. Điều này hoạt động vì các cạnh được xử lý theo thứ tự không giảm, do đó, liên kết thành công cuối cùng hoàn thành kết nối sẽ đưa ra ngưỡng chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
0 1 10
0 2 20
1 2 30
```| Bước | Cạnh (w,u,v) | Linh kiện | Liên minh? | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | - | 3 | - | 0 | 
| 1 | (10,0,1) | 2 | Có | 10 | 
| 2 | (20,0,2) | 1 | Có | 20 | 

Ở trọng số 10, chỉ tồn tại cạnh (0,1) nên đồ thị chưa được kết nối đầy đủ. Ở trọng số 20, các cạnh (0,1) và (0,2) có sẵn và kết nối tất cả các nút. DSU phản ánh điều này bằng cách tiếp cận một thành phần duy nhất ở cạnh thứ hai. 

### Ví dụ 2 

đầu vào:```
4 3
0 1 10
0 2 20
0 3 30
```| Bước | Cạnh (w,u,v) | Linh kiện | Liên minh? | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | - | 4 | - | 0 | 
| 1 | (10,0,1) | 3 | Có | 10 | 
| 2 | (20,0,2) | 2 | Có | 20 | 
| 3 | (30,0,3) | 1 | Có | 30 | 

Trường hợp này cho thấy cấu trúc hình sao nơi kết nối phát triển tuyến tính. Câu trả lời là cạnh cuối cùng vì mỗi nút chỉ kết nối qua đỉnh 0 sau khi có sẵn cạnh tương ứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| Sắp xếp các cạnh chiếm ưu thế; Hoạt động DSU gần như được khấu hao cố định | 
| Không gian |$O(n + m)$| Lưu trữ cho mảng DSU và danh sách cạnh | 

Độ phức tạp này vừa vặn trong giới hạn vì tổng số cạnh trong tất cả các trường hợp thử nghiệm là nhiều nhất.$5 \cdot 10^5$, giúp cho hoạt động phân loại và DSU trở nên hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    backup_stdout = sys.stdout
    sys.stdout = StringIO()

    # assume solve() is defined above in the same module
    solve()

    out = sys.stdout.getvalue()
    sys.stdout = backup_stdout
    return out.strip()

# provided samples
assert run("""2
3 3
0 1 10
0 2 20
1 2 30
4 3
0 1 10
0 2 20
0 3 30
""") == "20\n30"

# minimum case
assert run("""1
2 1
0 1 5
""") == "5"

# chain
assert run("""1
5 4
0 1 1
1 2 2
2 3 3
3 4 4
""") == "4"

# star
assert run("""1
5 4
0 1 10
0 2 10
0 3 10
0 4 10
""") == "10"

# already connected early
assert run("""1
3 3
0 1 100
1 2 1
0 2 50
""") == "50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút cạnh đơn | 5 | kết nối tối thiểu | 
| đồ thị chuỗi | 4 | phụ thuộc đường dẫn dài nhất | 
| đồ thị sao | 10 | tham gia đồng thời | 
| hỗn hợp trọng lượng tam giác | 50 | đặt hàng đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đồ thị gần như đã được kết nối và chỉ thiếu một cạnh để hoàn thành kết nối. Ví dụ:```
3 3
0 1 100
1 2 1
0 2 50
```Đầu tiên DSU kết nối (1,2) tại thời điểm 1, sau đó kết nối (0,2) tại thời điểm 50 và dừng ở đó. Câu trả lời là 50 vì trước thời điểm đó nút 0 bị cô lập. 

Một trường hợp khác là khi tất cả các cạnh có trọng số giống nhau. Thuật toán vẫn xử lý tất cả các cạnh ở trọng số đó, nhưng khả năng kết nối sẽ đạt được ngay khi có đủ sự kết hợp. Vì:```
4 3
0 1 5
1 2 5
2 3 5
```mỗi sự kết hợp xảy ra ở trọng số 5 và lần hợp nhất cuối cùng hoàn thành kết nối ở mức 5. Thuật toán đưa ra chính xác trọng số chung vì việc sắp xếp bảo toàn tất cả các cạnh bằng nhau với nhau và DSU hợp nhất cho đến khi còn lại một thành phần.
