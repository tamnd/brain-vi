---
title: "CF 106160C - Tính mạch lạc"
description: "Mỗi mô hình được thể hiện bằng một vòng tròn trên một tấm bảng rất lớn. Đầu vào cung cấp tọa độ tâm của hình tròn và đường kính của nó. Hai mô hình được coi là kết nối trực tiếp khi khoảng cách giữa các cạnh của đế của chúng tối đa là hai inch."
date: "2026-06-25T11:11:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106160
codeforces_index: "C"
codeforces_contest_name: "2025 Benelux Algorithm Programming Contest (BAPC 25)"
rating: 0
weight: 106160
solve_time_s: 56
verified: true
draft: false
---

[CF 106160C - Tính mạch lạc](https://codeforces.com/problemset/problem/106160/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi mô hình được thể hiện bằng một vòng tròn trên một tấm bảng rất lớn. Đầu vào cung cấp tọa độ tâm của hình tròn và đường kính của nó. 

Hai mô hình được coi là kết nối trực tiếp khi khoảng cách giữa các cạnh của đế của chúng tối đa là hai inch. Vì tất cả tọa độ và đường kính được tính bằng milimét nên hai inch bằng 50,8 mm. 

Nếu chúng ta xây dựng một đồ thị trong đó mọi mô hình là một đỉnh và mọi cặp kết nối trực tiếp là một cạnh, thì một tập hợp sẽ nhất quán khi có hai điều kiện. 

Đầu tiên, đồ thị phải được kết nối. Mọi mô hình phải có thể truy cập được từ mọi mô hình khác thông qua một chuỗi kết nối trực tiếp. 

Thứ hai, khi đơn vị chứa ít nhất bảy mô hình thì mỗi mô hình phải có ít nhất hai bậc. Một mô hình chỉ được kết nối với một người hàng xóm là không đủ. 

Kích thước đầu vào là thách thức chính. Có thể có tới 200.000 mô hình. Việc kiểm tra sơ bộ từng cặp sẽ yêu cầu khoảng$$\frac{200000 \cdot 199999}{2} \approx 2 \cdot 10^{10}$$tính toán khoảng cách, điều này hoàn toàn không thể. 

Quan sát hình học quan trọng là các căn cứ đều nhỏ. Đường kính lớn nhất chỉ 160 mm nên khoảng cách kết nối lớn nhất có thể giữa các tâm là$$80 + 80 + 50.8 = 210.8 \text{ mm}.$$Không có cạnh nào có thể kết nối các mô hình có tâm cách xa nhau hơn 210,8 mm. 

Bán kính tương tác giới hạn này là yếu tố giúp giải pháp phân vùng không gian trở nên khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi hai mô hình ở chính xác khoảng cách cho phép. Ví dụ:```
2
13 13 25
88 13 25
```Khoảng cách trung tâm là 75 mm. Tổng bán kính là 25 mm. Khoảng cách giữa các đế chính xác là 50 mm, nằm trong khoảng 50,8 mm, vì vậy câu trả lời là`yes`. 

Một sự nghiêm khắc`<`so sánh sẽ bác bỏ những trường hợp như vậy một cách không chính xác. 

Một trường hợp quan trọng khác là đồ thị liên thông có bảy mô hình trở lên trong đó một đỉnh có bậc một.```
7
...
```Mặc dù đồ thị được kết nối, câu trả lời phải là`no`vì yêu cầu về độ sẽ có hiệu lực khi kích thước đơn vị đạt đến bảy. 

Nguồn sai lầm thứ ba là độ chính xác của dấu phẩy động. Ngưỡng liên quan đến 50,8 mm. So sánh trực tiếp khoảng cách dấu phẩy động có thể dẫn đến lỗi đường biên. Cách tiếp cận an toàn nhất là chuyển đổi điều kiện thành so sánh số nguyên bằng cách sử dụng khoảng cách bình phương. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi cặp mô hình, hãy tính xem chúng có được kết nối trực tiếp hay không. Bất cứ khi nào một cạnh tồn tại, hãy thêm nó vào biểu đồ, cập nhật độ và hợp nhất các điểm cuối trong DSU. Sau khi tất cả các cặp được xử lý, hãy xác minh kết nối và điều kiện mức độ. 

Điều này có hiệu quả vì định nghĩa về tính kết hợp theo nghĩa đen là một thuộc tính đồ thị. Vấn đề là số lượng cặp. Với 200.000 mẫu, số lượng cặp lên tới khoảng 20 tỷ, vượt xa thời hạn. 

Lý do chúng ta có thể làm tốt hơn là vì các cạnh có tính cục bộ cực kỳ cao. Bất kể mẫu xe ở vị trí nào trên bảng 100 km, nó chỉ có thể kết nối với những mẫu xe có tâm nằm trong khoảng 210,8 mm. 

Điều đó gợi ý việc chia mặt phẳng thành các ô vuông. Nếu chiều dài cạnh ô lớn hơn bán kính kết nối tối đa có thể thì bất kỳ mô hình nào cũng chỉ có thể kết nối với các mô hình trong ô riêng của nó hoặc một trong tám ô lân cận. 

Sử dụng kích thước ô 211 mm là đủ vì mọi cạnh có thể có chiều dài tối đa là 210,8 mm. 

Bây giờ mỗi mô hình chỉ cần được so sánh với một số ít mô hình lân cận. Bởi vì các căn cứ không bao giờ trùng nhau nên có giới hạn dưới về mức độ gần nhau của các tâm. Bên trong một vùng lân cận có kích thước không đổi, chỉ có thể tồn tại một số lượng mô hình không đổi. Tổng số so sánh trở nên tuyến tính trong thực tế và trong đối số đóng gói hình học. 

Bất cứ khi nào tìm thấy cạnh hợp lệ, chúng tôi sẽ cập nhật số lượng độ và hợp nhất các điểm cuối trong DSU. Sau khi xử lý tất cả các cặp lân cận, chúng tôi kiểm tra xem tất cả các mô hình có thuộc về một thành phần hay không và liệu mọi độ có ít nhất là hai khi$n \ge 7$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n α(n)) dự kiến/tuyến tính hình học | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các mô hình và lưu trữ tọa độ và đường kính của chúng. 
2. Tạo DSU với một nút cho mỗi mô hình. 
3. Chọn kích thước ô lưới là 211 mm. Khoảng cách này lớn hơn một chút so với khoảng cách kết nối giữa các trung tâm tối đa có thể. 
4. Xử lý từng mô hình một. Đối với một người mẫu tại`(x, y)`, tính tọa độ ô của nó:$$(x // 211,\; y // 211)$$5. Trước khi chèn mô hình vào lưới, hãy kiểm tra chín ô bao gồm ô riêng của nó và tám ô lân cận. 

Bất kỳ mô hình nào ở xa hơn mức đó đều không thể kết nối được vì kích thước ô đã vượt quá độ dài cạnh tối đa. 
6. Đối với mọi mô hình được lưu trữ trước đó được tìm thấy trong các ô đó, hãy kiểm tra xem cạnh có tồn tại hay không. 

Hãy để đường kính là`d1`Và`d2`. 

Điều kiện kết nối là$$\text{center distance}
\le
\frac{d_1+d_2}{2}+50.8$$Để tránh số học dấu phẩy động, hãy viết lại dưới dạng$$100 \cdot \text{dist}^2
\le
(5(d_1+d_2)+508)^2$$7. Bất cứ khi nào điều kiện được thỏa mãn, hãy tăng cả hai độ và hợp nhất hai đỉnh trong DSU. 
8. Chèn mô hình hiện tại vào ô lưới của nó. 
9. Sau khi tất cả các mô hình được xử lý, hãy xác minh rằng mọi mô hình đều thuộc cùng một thành phần DSU. 
10. Nếu`n >= 7`, hãy xác minh rằng mọi độ đều có ít nhất là hai. 
11. Đầu ra`"yes"`nếu cả hai lần kiểm tra đều đạt, nếu không thì xuất ra`"no"`. 

### Tại sao nó hoạt động 

Lưới không bao giờ bỏ lỡ một cạnh. Mỗi cạnh hợp lệ có chiều dài tối đa là 210,8 mm, trong khi mỗi ô có chiều dài cạnh là 211 mm. Hai điểm có các ô khác nhau nhiều hơn một ở tọa độ phải cách xa nhau hơn 211 mm ở ít nhất một trục, khiến cho cạnh không thể tồn tại. 

Mỗi cạnh thực tế được kiểm tra chính xác một lần khi điểm cuối sau đó được xử lý. Bất cứ khi nào một cạnh tồn tại theo định nghĩa bài toán, thuật toán sẽ thêm nó vào DSU và cập nhật số lượng độ. 

Do đó, DSU biểu diễn chính xác các thành phần được kết nối của biểu đồ kết hợp và mảng độ chứa chính xác các độ của biểu đồ. Việc kiểm tra cuối cùng chính xác là hai điều kiện từ định nghĩa của một đơn vị mạch lạc. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

CELL = 211

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.sz = [1] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)

        if a == b:
            return

        if self.sz[a] < self.sz[b]:
            a, b = b, a

        self.p[b] = a
        self.sz[a] += self.sz[b]

def solve():
    n = int(input())

    models = []
    for _ in range(n):
        x, y, d = map(int, input().split())
        models.append((x, y, d))

    dsu = DSU(n)
    deg = [0] * n

    grid = defaultdict(list)

    for i, (x, y, d) in enumerate(models):
        cx = x // CELL
        cy = y // CELL

        for dx in (-1, 0, 1):
            for dy in (-1, 0, 1):
                cell = (cx + dx, cy + dy)

                for j in grid.get(cell, []):
                    x2, y2, d2 = models[j]

                    dist2 = (x - x2) * (x - x2) + (y - y2) * (y - y2)

                    rhs = 5 * (d + d2) + 508

                    if 100 * dist2 <= rhs * rhs:
                        deg[i] += 1
                        deg[j] += 1
                        dsu.union(i, j)

        grid[(cx, cy)].append(i)

    root = dsu.find(0)

    for i in range(1, n):
        if dsu.find(i) != root:
            print("no")
            return

    if n >= 7:
        for d in deg:
            if d < 2:
                print("no")
                return

    print("yes")

solve()
```DSU theo dõi kết nối mà không lưu trữ biểu đồ một cách rõ ràng. Mỗi cạnh được phát hiện sẽ ngay lập tức hợp nhất các điểm cuối của nó. 

Việc tìm kiếm hình học được xử lý bởi lưới băm. Một mô hình chỉ được so sánh với các mô hình đã được chèn vào các ô lân cận, điều này tránh việc kiểm tra trùng lặp. 

Việc so sánh khoảng cách hoàn toàn dựa trên số nguyên. biểu hiện```
rhs = 5 * (d + d2) + 508
```xuất phát từ việc nhân ngưỡng với mười:$$10 \cdot \left(\frac{d_1+d_2}{2}+50.8\right)
=
5(d_1+d_2)+508$$Bình phương cả hai vế tạo ra một phép so sánh số nguyên chính xác mà không gặp vấn đề gì về độ chính xác. 

Việc kiểm tra kết nối sử dụng một gốc đại diện duy nhất. Nếu bất kỳ đỉnh nào thuộc về thành phần DSU khác, biểu đồ sẽ bị ngắt kết nối. 

Điều kiện mức độ chỉ được kiểm tra khi`n >= 7`, khớp chính xác với phát biểu vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
13 13 25
88 13 25
```| tôi | Vị trí | Thay đổi Bằng cấp | Linh kiện | 
| --- | --- | --- | --- | 
| 0 | (13,13) | không | {0} | 
| 1 | (88,13) | độ[0]++, độ[1]++ | {0,1} | 

Bằng cấp cuối cùng: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 

Biểu đồ được kết nối. Từ`n < 7`, quy tắc mức độ không được áp dụng. Câu trả lời là`yes`. 

### Ví dụ 2 

đầu vào:```
7
1066 910 130
1007 1032 130
875 1062 130
770 978 130
770 843 130
875 758 130
1007 788 130
```| Đỉnh | Bằng Sau Khi Xử Lý | 
| --- | --- | 
| 0 | 2 | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 2 | 
| 5 | 2 | 
| 6 | 2 | 

Tất cả các đỉnh tạo thành một cấu trúc giống như một chu trình. Đồ thị được kết nối và mỗi độ có ít nhất là hai. Câu trả lời là`yes`. 

Ví dụ này chứng minh tại sao điều kiện mức độ lại tách biệt khỏi khả năng kết nối. Chỉ một biểu đồ được kết nối sẽ không đủ khi có bảy mô hình trở lên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) dự kiến ​​| Mỗi mô hình chỉ tương tác với các mô hình lân cận trong một vùng lân cận có kích thước không đổi | 
| Không gian | O(n) | DSU, mảng độ và lưu trữ lưới | 

Bảng rất lớn nhưng bán kính tương tác rất nhỏ và bị giới hạn. Băm không gian giữ số lần kiểm tra khoảng cách tỷ lệ thuận với số lượng mô hình, giúp giải pháp dễ dàng đủ nhanh cho 200.000 đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    CELL = 211

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.sz = [1] * n

        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a == b:
                return
            if self.sz[a] < self.sz[b]:
                a, b = b, a
            self.p[b] = a
            self.sz[a] += self.sz[b]

    n = int(input())
    models = [tuple(map(int, input().split())) for _ in range(n)]

    dsu = DSU(n)
    deg = [0] * n
    grid = defaultdict(list)

    for i, (x, y, d) in enumerate(models):
        cx = x // CELL
        cy = y // CELL

        for dx in (-1, 0, 1):
            for dy in (-1, 0, 1):
                for j in grid.get((cx + dx, cy + dy), []):
                    x2, y2, d2 = models[j]
                    dist2 = (x - x2) ** 2 + (y - y2) ** 2
                    rhs = 5 * (d + d2) + 508
                    if 100 * dist2 <= rhs * rhs:
                        deg[i] += 1
                        deg[j] += 1
                        dsu.union(i, j)

        grid[(cx, cy)].append(i)

    root = dsu.find(0)

    ok = True
    for i in range(n):
        if dsu.find(i) != root:
            ok = False

    if ok and n >= 7:
        ok = all(d >= 2 for d in deg)

    return ("yes\n" if ok else "no\n")

# provided samples
assert run("2\n13 13 25\n88 13 25\n") == "yes\n"
assert run("2\n13 13 25\n89 13 25\n") == "no\n"

# custom cases
assert run("2\n0 0 25\n1000 0 25\n") == "no\n"
assert run("3\n0 0 25\n50 0 25\n100 0 25\n") == "yes\n"
assert run("7\n0 0 25\n50 0 25\n100 0 25\n150 0 25\n200 0 25\n250 0 25\n300 0 25\n") == "no\n"
assert run("7\n100 0 130\n62 78 130\n-22 97 130\n-90 43 130\n-90 -43 130\n-22 -97 130\n62 -78 130\n") == "yes\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai người mẫu cách xa nhau | không | Đồ thị bị ngắt kết nối | 
| Ba mô hình trong một chuỗi | vâng | Kết nối thông qua các đỉnh trung gian | 
| Con đường bảy mẫu | không | Điều kiện độ kích hoạt | 
| Chu kỳ bảy mô hình | vâng | Đã kết nối và mỗi mức độ ít nhất là hai | 

## Vỏ cạnh 

Xét hai mô hình chính xác ở ngưỡng cho phép:```
2
13 13 25
88 13 25
```Khoảng cách trung tâm là 75. Ngưỡng là$$12.5 + 12.5 + 50.8 = 75.8.$$Phép so sánh số nguyên chấp nhận cạnh vì nó sử dụng`<=`, không`<`. Đồ thị được kết nối, vì vậy câu trả lời là`yes`. 

Bây giờ hãy xem xét một chuỗi kết nối gồm bảy mô hình:```
7
0 0 25
50 0 25
100 0 25
150 0 25
200 0 25
250 0 25
300 0 25
```Đồ thị được kết nối, nhưng điểm cuối có bậc một. Sau khi xử lý cạnh, mảng độ bắt đầu và kết thúc bằng`1`. Từ`n >= 7`, thuật toán sẽ loại bỏ đơn vị và in`no`. 

Cuối cùng, hãy xem xét trường hợp gần ranh giới dấu phẩy động. Thuật toán không bao giờ tính căn bậc hai và không bao giờ lưu trữ 50,8 dưới dạng ngưỡng dấu phẩy động. Nó kiểm tra$$100 \cdot \text{dist}^2
\le
(5(d_1+d_2)+508)^2$$chỉ sử dụng số nguyên, do đó không có lỗi làm tròn nào có thể làm thay đổi kết quả một cách sai lầm.
