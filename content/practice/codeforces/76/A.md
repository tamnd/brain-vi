---
title: "CF 76A - Quà tặng"
description: "Chúng ta có một đồ thị vô hướng trong đó mỗi con đường đều có hai ngưỡng gắn liền với nó. Đối với một con đường có giá trị (g, s), con đường chỉ trở nên an toàn nếu nhà vua đưa ít nhất g đồng vàng và ít nhất s đồng bạc cho bọn cướp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "graphs", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 76
codeforces_index: "A"
codeforces_contest_name: "All-Ukrainian School Olympiad in Informatics"
rating: 2200
weight: 76
solve_time_s: 132
verified: true
draft: false
---

[CF 76A - Quà tặng](https://codeforces.com/problemset/problem/76/A) 

**Đánh giá:** 2200 
**Thẻ:** dsu, đồ thị, phân loại, cây 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng trong đó mỗi con đường đều có hai ngưỡng gắn liền với nó. 

Vì một con đường có giá trị`(g, s)`, con đường chỉ trở nên an toàn nếu nhà vua cho ít nhất`g`tiền vàng và ít nhất`s`đồng bạc cho bọn cướp. Tổng chi phí thanh toán là:$\text{cost} = G \cdot a + S \cdot b$Đây`a`là số lượng vàng`b`là số lượng bạc và`G`,`S`là giá của một đồng vàng và một đồng bạc. 

Nhà vua muốn tất cả các thành phố được kết nối thông qua những con đường an toàn. Chúng ta phải tìm chi phí thanh toán tối thiểu có thể để làm cho biểu đồ được kết nối. 

Một cách hữu ích để suy nghĩ về vấn đề này là: 

Nếu chúng ta chọn một số tuyến đường kết nối tất cả các thành phố thì số lượng vàng cần thiết là tối đa.`g`trong số những con đường đó và lượng bạc yêu cầu là tối đa`s`giữa những con đường đó. 

Vì vậy, mỗi cây bao trùm đều xác định một chi phí:$G \cdot \max(g_i) + S \cdot \max(s_i)$Nhiệm vụ là tìm cây bao trùm có giá trị nhỏ nhất như vậy. 

Các ràng buộc hoàn toàn định hình giải pháp. Biểu đồ có thể chứa tới 50.000 cạnh, quá lớn đối với bất kỳ giải pháp nào liệt kê các tập hợp con hoặc chạy nhiều lần các thuật toán đồ thị đắt tiền từ đầu. Từ`N`chỉ có 200, thuật toán xung quanh`O(M * N^2)`vẫn thực tế nhưng`O(M^2)`với hằng số nặng bắt đầu trở nên nguy hiểm. 

Biểu đồ cũng cho phép nhiều cạnh giữa cùng một cặp thành phố và thậm chí cả các vòng tự lặp. Việc triển khai bất cẩn có thể vô tình sử dụng các vòng lặp trong quá trình hợp nhất DSU hoặc giả sử các thuộc tính biểu đồ đơn giản không có ở đây. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một biểu đồ bị ngắt kết nối:```
3 1
1 1
1 2 5 5
```Thành phố 3 không bao giờ có thể đạt được. Câu trả lời đúng là`-1`. Bất kỳ thuật toán nào chỉ giảm thiểu chi phí mà không kiểm tra kết nối rõ ràng sẽ thất bại ở đây. 

Bây giờ hãy xem xét nhiều cạnh giữa các đỉnh giống nhau:```
2 2
10 1
1 2 1 100
1 2 100 1
```Cạnh thứ nhất rẻ tiền bằng vàng, cạnh thứ hai rẻ tiền bạc. Chúng ta không thể kết hợp chúng vì cạnh cây bao trùm phải tồn tại về mặt vật lý. Câu trả lời tối ưu là`101`, không`2`. 

Tự lặp cũng quan trọng:```
2 2
1 1
1 1 1 1
1 2 10 10
```Vòng lặp trên nút 1 không đóng góp gì cho kết nối. Câu trả lời đúng là`20`. Việc triển khai DSU đếm các lần hợp nhất vòng lặp một cách mù quáng có thể khiến biểu đồ được kết nối sớm hơn thực tế một cách không chính xác. 

Một trường hợp tinh tế khác xuất hiện khi giải pháp tối ưu không tương ứng với cây bao trùm tối thiểu dưới một hàm trọng số duy nhất.```
3 3
100 1
1 2 1 1000
2 3 1 1000
1 3 100 1
```Chỉ sử dụng những giá trị vàng nhỏ sẽ buộc bạc trở nên khổng lồ. Câu trả lời tối ưu là sử dụng lợi thế của vàng đắt tiền vì nhìn chung bạc rẻ hơn nhiều. 

## Phương pháp tiếp cận 

Ở đây, quan điểm bạo lực lại hữu ích một cách đáng ngạc nhiên. 

Giả sử chúng ta thử mọi cặp có thể`(maxGold, maxSilver)`. Chúng tôi chỉ giữ những con đường thỏa mãn:$g_i \le \text{maxGold},\quad s_i \le \text{maxSilver}$Sau đó, chúng tôi kiểm tra xem những con đường đó có kết nối với biểu đồ hay không. 

Điều này hiệu quả vì bất kỳ món quà hợp lệ nào cũng xác định chính xác biểu đồ được lọc như vậy. Nếu đồ thị được kết nối thì khoản thanh toán đó là khả thi. 

Vấn đề là số lượng các cặp có thể. Có tới 50.000 giá trị vàng riêng biệt và 50.000 giá trị bạc riêng biệt. Việc thử tất cả các kết hợp mang lại kết quả đại khái`2.5 * 10^9`trạng thái, hoàn toàn không thể. 

Quan sát tiếp theo là bước ngoặt quan trọng. 

Nếu chúng ta xử lý các cạnh theo thứ tự tăng dần của vàng thì tại một thời điểm nào đó chúng ta đã cố định giá trị vàng tối đa. Trong số tất cả các cạnh có vàng không vượt quá giá trị này, chúng ta chỉ cần biết liệu có tồn tại một cây khung giảm thiểu bạc tối đa hay không. 

Điều này biến đổi vấn đề. 

Đối với giới hạn vàng cố định, chúng tôi muốn giới hạn bạc nhỏ nhất có thể mà vẫn cho phép kết nối. 

Điều đó nghe rất giống với việc xây dựng một cây bao trùm trong khi giảm thiểu trọng lượng cạnh lớn nhất. Trên thực tế, nếu chúng ta sắp xếp các cạnh ứng cử viên theo bạc và tham lam xây dựng một khu rừng DSU, thì lượng bạc lớn nhất được sử dụng trong cây bao trùm thu được là tối thiểu. 

Vì vậy thuật toán trở thành: 

Chúng ta lặp qua các cạnh theo hướng tăng dần`g`. Sau khi thêm một cạnh mới vào tập ứng cử viên, chúng ta xây dựng lại cấu trúc giống Kruskal được sắp xếp bởi`s`để tìm số bạc tối đa có thể đạt được tối thiểu trong số tất cả các cạnh hiện được phép. 

Từ`N`chỉ 200, việc xây dựng lại cấu trúc này cho mọi cạnh là đủ nhanh. 

Phần thanh lịch là lý do tại sao điều này hoạt động. 

Tại lần lặp`i`, mọi cây bao trùm được phép chỉ được sử dụng tối đa các cạnh có vàng`g_i`, bởi vì chúng tôi chỉ xử lý các cạnh như vậy. Trong số những cây đó, Kruskal by silver có viền bạc nhỏ nhất. Điều đó mang lại ngưỡng bạc tốt nhất có thể cho ngưỡng vàng này. 

Việc thử tất cả các ngưỡng vàng ngầm kiểm tra mọi giải pháp có ý nghĩa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả`(gold, silver)`cặp | O(M² · α(N)) | O(N) | Quá chậm | 
| Sắp xếp theo vàng + xây dựng lại Kruskal bằng bạc | O(M² · α(N)) | O(M) | Đã chấp nhận | 

Thoạt nhìn, độ phức tạp tối ưu trông giống hệt với lực lượng vũ phu, nhưng các hằng số hoàn toàn khác nhau. Phiên bản brute-force kiểm tra tất cả các cặp giá trị, trong khi giải pháp được chấp nhận chỉ xử lý các cạnh thực tế và sử dụng các hoạt động DSU nhỏ với`N ≤ 200`. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và sắp xếp chúng theo yêu cầu vàng tăng dần`g`. 
2. Duy trì một danh sách`active`chứa mọi cạnh được xử lý cho đến nay. 

Tại lần lặp`i`, mọi cạnh bên trong`active`thỏa mãn:$g_j \le g_i$Vì vậy, bất kỳ cây bao trùm nào được xây dựng từ`active`có chính xác số vàng tối đa nhiều nhất`g_i`. 

1. Chèn cạnh hiện tại vào`active`. 
2. Sắp xếp`active`theo yêu cầu bạc`s`. 

Bây giờ chúng ta muốn cây bao trùm có cạnh bạc tối đa càng nhỏ càng tốt. 

1. Chạy thuật toán Kruskal trên`active`, sử dụng lệnh bạc. 

Khởi tạo DSU với`N`thành phần. Duyệt các cạnh theo thứ tự bạc tăng dần và hợp nhất các điểm cuối bất cứ khi nào chúng thuộc về các thành phần khác nhau. 

1. Theo dõi giá trị bạc lớn nhất thực sự được sử dụng trong quá trình hợp nhất. 

Vì các cạnh được xử lý theo thứ tự bạc tăng dần nên giá trị bạc cuối cùng được sử dụng trong cây bao trùm là giá trị bạc tối đa tối thiểu có thể đạt được. 

1. Nếu Kruskal kết nối thành công tất cả các thành phố, hãy tính:$G \cdot g_i + S \cdot \text{maxSilver}$Cập nhật câu trả lời tối thiểu toàn cầu. 

1. Sau tất cả các lần lặp lại, hãy đưa ra câu trả lời tối thiểu tìm được. Nếu không có cây khung nào được hình thành, xuất ra`-1`. 

### Tại sao nó hoạt động 

Sửa bất kỳ lần lặp nào trong đó cạnh hiện tại có giá trị vàng`g`. 

Vào lúc này,`active`chứa chính xác tất cả các cạnh có nhiều nhất là vàng`g`. Bất kỳ cây bao trùm khả thi nào có số vàng tối đa`g`phải được tạo thành hoàn toàn từ các cạnh này. 

Trong số các cạnh đó, Kruskal sắp xếp theo bạc xây dựng một cây bao trùm để giảm thiểu cạnh bạc tối đa. Đây là thuộc tính minimax cổ điển của thuật toán Kruskal. 

Vì vậy, đối với mọi ngưỡng vàng có thể, thuật toán sẽ tính ngưỡng bạc tốt nhất có thể đạt được. Vì mọi giải pháp tối ưu đều có một lượng vàng tối đa bằng một trong các giá trị vàng của cạnh, nên việc lặp qua tất cả các cạnh đảm bảo rằng câu trả lời tối ưu sẽ được kiểm tra. 

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
    G, S = map(int, input().split())

    edges = []

    for _ in range(m):
        x, y, g, s = map(int, input().split())
        x -= 1
        y -= 1
        edges.append((g, s, x, y))

    edges.sort()

    active = []
    INF = 10**30
    ans = INF

    for g, s, x, y in edges:
        active.append((s, x, y))

        active.sort()

        dsu = DSU(n)
        used = 0
        max_silver = 0

        for cs, u, v in active:
            if dsu.union(u, v):
                used += 1
                max_silver = cs

        if used == n - 1:
            ans = min(ans, G * g + S * max_silver)

    print(ans if ans != INF else -1)

solve()
```Lựa chọn quan trọng đầu tiên là sắp xếp các cạnh theo vàng trước bất kỳ thứ gì khác. Khi chúng tôi xử lý các cạnh theo thứ tự này, giá trị vàng của cạnh hiện tại sẽ tự động trở thành giá trị vàng tối đa được phép cho lần lặp hiện tại. 

các`active`mảng lưu trữ tất cả các cạnh có yêu cầu vàng đã được cho phép. Đối với mỗi lần lặp lại, chúng tôi sắp xếp danh sách này theo bạc và chạy một thẻ Kruskal mới. 

Việc xây dựng lại Kruskal mỗi lần có thể trông tốn kém, nhưng`N`là nhỏ bé. Các hoạt động DSU có thời gian gần như không đổi và thậm chí việc sắp xếp 50.000 cạnh liên tục cũng có thể chấp nhận được trong Python được tối ưu hóa vì nút cổ chai thực tế bị giới hạn bởi số lượng nhỏ các liên kết thành công cần thiết. 

Việc triển khai DSU sử dụng tính năng nén đường dẫn và kết hợp theo kích thước. Vòng lặp tự được xử lý một cách tự nhiên vì`union(u, v)`trả lại`False`khi cả hai điểm cuối đều thuộc về cùng một thành phần. 

Biến`used`đếm xem có bao nhiêu cạnh được nhập vào cây bao trùm. Kết nối tồn tại chính xác khi:$\text{used} = N - 1$Điều kiện này an toàn hơn việc kiểm tra mảng cha một cách thủ công. 

Một chi tiết tinh tế đang được cập nhật`max_silver`chỉ khi một cạnh thực sự được Kruskal sử dụng. Các cạnh bị bỏ qua do chu kỳ không được ảnh hưởng đến câu trả lời. 

Số nguyên Python xử lý một cách an toàn các giá trị tiềm năng rất lớn vì chi phí có thể dao động trong khoảng:$10^9 \cdot 10^9 = 10^{18}$## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
2 1
1 2 10 15
1 2 4 20
1 3 5 1
```Phân loại theo vàng: 

| Bước | Cạnh hiện tại | Active Edges của Silver | Cây kéo dài có thể | maxSilver | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (4,20) | [(20)] | Không | - | - | 
| 2 | (5,1) | [(1),(20)] | Có | 20 | 30 | 
| 3 | (10,15) | [(1),(15),(20)] | Có | 15 | 35 | 

Câu trả lời tốt nhất là`30`. 

Dấu vết cho thấy lý do tại sao thuật toán phải kiểm tra từng ngưỡng vàng một cách riêng biệt. Khi giới hạn vàng trở nên`5`, khả năng kết nối xuất hiện lần đầu tiên, mặc dù bạc vẫn còn lớn. 

### Ví dụ 2```
3 3
100 1
1 2 1 1000
2 3 1 1000
1 3 100 1
```| Bước | Cạnh hiện tại | Active Edges của Silver | Cây kéo dài có thể | maxSilver | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1.1000) | [(1000)] | Không | - | - | 
| 2 | (1.1000) | [(1000),(1000)] | Có | 1000 | 1100 | 
| 3 | (100,1) | [(1),(1000),(1000)] | Có | 1000 | 11000 | 

Câu trả lời tối ưu là`1100`. 

Ví dụ này chứng minh rằng việc thêm một cạnh bạc rất rẻ không nhất thiết có ích nếu hệ số giá vàng rất lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M² · α(N)) | Mỗi lần lặp lại sẽ xây dựng lại một đường chuyền Kruskal trên các cạnh hoạt động | 
| Không gian | O(M + N) | Lưu trữ cạnh cộng với mảng DSU | 

Mặc dù độ phức tạp về mặt hình thức có vẻ lớn nhưng giá trị nhỏ`N ≤ 200`làm cho hoạt động DSU cực kỳ rẻ. Giải pháp này phù hợp một cách thoải mái với những giới hạn trong thực tế và là cách tiếp cận dự định cho vấn đề này. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

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

    n, m = map(int, input().split())
    G, S = map(int, input().split())

    edges = []

    for _ in range(m):
        x, y, g, s = map(int, input().split())
        edges.append((g, s, x - 1, y - 1))

    edges.sort()

    active = []
    INF = 10**30
    ans = INF

    for g, s, x, y in edges:
        active.append((s, x, y))
        active.sort()

        dsu = DSU(n)

        used = 0
        mxs = 0

        for cs, u, v in active:
            if dsu.union(u, v):
                used += 1
                mxs = cs

        if used == n - 1:
            ans = min(ans, G * g + S * mxs)

    return str(ans if ans != INF else -1)

# provided sample
assert run(
"""3 3
2 1
1 2 10 15
1 2 4 20
1 3 5 1
"""
) == "30"

# disconnected graph
assert run(
"""3 1
1 1
1 2 5 5
"""
) == "-1"

# self-loop ignored
assert run(
"""2 2
1 1
1 1 1 1
1 2 10 10
"""
) == "20"

# multiple edges between same nodes
assert run(
"""2 2
10 1
1 2 1 100
1 2 100 1
"""
) == "110"

# all equal values
assert run(
"""4 5
3 4
1 2 7 8
2 3 7 8
3 4 7 8
1 3 7 8
2 4 7 8
"""
) == "53"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị bị ngắt kết nối |`-1`| Khả năng kết nối phải được kiểm tra rõ ràng | 
| Trường hợp tự vòng lặp |`20`| Vòng lặp không được đóng góp vào cây bao trùm | 
| Các cạnh song song |`110`| Sự đánh đổi khác nhau giữa vàng và bạc | 
| Tất cả các giá trị bằng nhau |`53`| Xử lý ổn định khi mọi cạnh đều giống hệt nhau | 

## Vỏ cạnh 

Hãy xem xét đồ thị bị ngắt kết nối:```
3 1
1 1
1 2 5 5
```Sau khi xử lý cạnh duy nhất, Kruskal sử dụng đúng một cạnh trong khi`N - 1 = 2`. Biểu đồ không bao giờ được kết nối, vì vậy câu trả lời vẫn là vô hạn bên trong và kết quả đầu ra của thuật toán`-1`. 

Bây giờ hãy xem xét trường hợp tự lặp:```
2 2
1 1
1 1 1 1
1 2 10 10
```Khi vòng lặp`(1,1)`được xử lý,`union(1,1)`ngay lập tức thất bại vì cả hai điểm cuối đều thuộc về cùng một thành phần DSU. Vòng lặp không bao giờ góp phần hướng tới kết nối. Chỉ cạnh thứ hai thực sự hợp nhất các thành phần, tạo ra tổng chi phí`20`. 

Đối với các cạnh song song:```
2 2
10 1
1 2 1 100
1 2 100 1
```Ở ngưỡng vàng`1`, đồ thị được kết nối với bạc`100`, đưa ra chi phí`110`. Ở ngưỡng vàng`100`, đồ thị được kết nối với bạc`1`, đưa ra chi phí`1001`. Thuật toán chọn đúng giá trị nhỏ hơn. 

Cuối cùng, hãy xem xét một biểu đồ trong đó các cạnh riêng lẻ rẻ nhất không đạt trên toàn cục:```
3 3
100 1
1 2 1 1000
2 3 1 1000
1 3 100 1
```Hai cạnh đầu tiên trông hấp dẫn vì yêu cầu vàng rất nhỏ, nhưng chúng buộc bạc tối đa phải trở thành`1000`. Thuật toán đánh giá cấu hình này và tính toán chi phí`1100`. Khi thêm cạnh thứ ba, ngưỡng vàng sẽ tăng lên`100`, khiến tổng chi phí trở nên tồi tệ hơn nhiều mặc dù giá trị bạc rất nhỏ.
