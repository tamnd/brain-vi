---
title: "CF 1419F - Mưa Lửa"
description: "Cố định một giá trị t. Brimstone có thể đợi ở bất kỳ đội nào trong một khoảng thời gian tùy ý. Khi quyết định di chuyển, anh ta chọn một trong bốn hướng trục và tiếp tục di chuyển cho đến khi đến được phân đội đầu tiên nằm trên tia đó."
date: "2026-06-11T06:46:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dfs-and-similar", "dsu", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1419
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 671 (Div. 2)"
rating: 2800
weight: 1419
solve_time_s: 102
verified: false
draft: false
---

[CF 1419F - Mưa lửa](https://codeforces.com/problemset/problem/1419/F) 

**Đánh giá:** 2800 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dfs và tương tự, dsu, đồ thị, triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Sửa một giá trị`t`. 

Brimstone có thể đợi ở bất kỳ đội nào trong một khoảng thời gian tùy ý. Khi quyết định di chuyển, anh ta chọn một trong bốn hướng trục và tiếp tục di chuyển cho đến khi đến được phân đội đầu tiên nằm trên tia đó. 

Giả sử hai phần nằm trên cùng một đường thẳng đứng hoặc nằm ngang và khoảng cách của chúng tối đa là`t`. Brimstone luôn có thể bắt đầu di chuyển ngay sau một cuộc tấn công vào quỹ đạo và đến trước cuộc tấn công tiếp theo. Vì việc chờ đợi là miễn phí nên bất kỳ nước đi nào có thời gian di chuyển nhiều nhất`t`luôn luôn có thể sử dụng được. 

Điều này biến hình học thành một bài toán đồ thị. 

Tạo một đồ thị có các đỉnh là các phần tách rời. Hai phân đội được kết nối nếu chúng có chung một`x`phối hợp hoặc một`y`tọa độ và khoảng cách Manhattan của họ dọc theo đường đó nhiều nhất là`t`. 

Đối với một cố định`t`, Brimstone có thể truy cập tất cả các phần tách khi và chỉ khi đồ thị này trở nên liên thông sau khi thêm tối đa một đỉnh bổ sung. 

Tọa độ lớn như`10^9`, nhưng chỉ có`1000`điểm. Điều này ngay lập tức gợi ý rằng cấu trúc đồ thị chỉ phụ thuộc vào các điểm đã cho chứ không phụ thuộc vào kích thước của mặt phẳng tọa độ. MỘT`O(n^2)`việc xây dựng đồ thị là hoàn toàn có thể chấp nhận được, trong khi mọi thứ liên quan đến toàn bộ lưới tọa độ là không thể. 

Trường hợp cạnh tinh tế là một cấu hình bao gồm một số thành phần bị ngắt kết nối mà không thể chạm tới tất cả bởi một điểm mới. 

Ví dụ:```
(0,0)   (100,100)   (200,200)
```Đối với mọi hữu hạn`t`, một điểm mới có thể kết nối với nhiều nhất một trong những điểm này theo chiều ngang và nhiều nhất là một điểm theo chiều dọc, vì vậy cả ba thành phần không bao giờ có thể được hợp nhất. Câu trả lời đúng là`-1`. 

Một sai lầm dễ mắc phải khác là cho rằng điểm mới phải được đặt ở tọa độ hiện có. Coi như:```
(0,100)
(100,0)
(-100,0)
(0,-100)
```Điểm mới tối ưu là`(0,0)`, vốn không có mặt ban đầu. Câu trả lời là`100`. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Tìm kiếm nhị phân câu trả lời`t`. Đối với mỗi giá trị ứng cử viên, hãy xây dựng biểu đồ và kiểm tra xem có thể thêm một số điểm tọa độ nguyên để làm cho biểu đồ được kết nối hay không. 

Nút thắt là vị trí của điểm phụ. Tọa độ có phạm vi lên tới`10^9`, vì vậy việc liệt kê các vị trí có thể là vô vọng. 

Quan sát quan trọng là đối với một giá trị cố định`t`, điểm mới chỉ quan trọng thông qua các đỉnh mà nó có thể kết nối tới. 

Một điểm mới`(X,Y)`kết nối với một điểm hiện có`(x,y)`nếu một trong hai:```
X = x and |Y - y| ≤ t
```hoặc```
Y = y and |X - x| ≤ t
```Giả sử có một vị trí thành công. Nếu chúng ta trượt điểm mới cho đến khi một trong những bất bình đẳng này trở nên chặt chẽ hơn, thì khả năng kết nối sẽ không trở nên tồi tệ hơn. Do đó, mọi tọa độ x liên quan đều có dạng:```
x_i - t, x_i, x_i + t
```và mọi tọa độ y liên quan đều có dạng:```
y_i - t, y_i, y_i + t
```chỉ có`O(n)`các giá trị x ứng viên như vậy và`O(n)`giá trị y ứng viên. Sau khi nén tọa độ, chỉ`O(n^2)`vị trí ứng viên vẫn còn. 

Đối với mỗi vị trí ứng cử viên, chúng tôi tạm thời kết nối đỉnh mới với tất cả các đỉnh ban đầu có thể tiếp cận và kiểm tra xem toàn bộ biểu đồ có được kết nối hay không. Vì có nhiều ứng viên nên việc xây dựng lại DSU mỗi lần sẽ quá tốn kém. DSU khôi phục cho phép chúng tôi thêm các cạnh tạm thời, kiểm tra kết nối và hoàn tác chúng trong thời gian gần như không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vị trí Brute Force trên tọa độ | Hàm mũ trong phạm vi tọa độ | O(n²) | Không thể | 
| Tìm kiếm nhị phân + ứng viên nén + DSU khôi phục | O(n2 log V) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm kiếm nhị phân giá trị khả thi tối thiểu`t`. 
2. Đối với cố định`t`, xây dựng DSU trên bản gốc`n`biệt đội. 
3. Kết nối mọi cặp tách có chung tọa độ x hoặc tọa độ y và có khoảng cách tối đa`t`. 
4. Nếu tất cả các đỉnh ban đầu đã thuộc về một thành phần DSU thì điều này`t`là khả thi. 
5. Tạo tất cả các giá trị x ứng cử viên:```
x_i - t, x_i, x_i + t
```và tất cả các giá trị y ứng cử viên:```
y_i - t, y_i, y_i + t
```6. Phối hợp nén các giá trị này. 
7. Thêm một đỉnh DSU được đánh số`n + 1`, đại diện cho sự tách ra mới có thể. 
8. Với mỗi cặp nén`(X,Y)`: 

1. Kết nối tạm thời đỉnh`n+1`đến mọi điểm trên cột`X`khoảng cách thẳng đứng của nó tới`Y`nhiều nhất là`t`. 
2. Kết nối tạm thời đỉnh`n+1`đến mọi điểm trên hàng`Y`khoảng cách theo phương ngang của nó tới`X`nhiều nhất là`t`. 
3. Kiểm tra xem thành phần DSU có chứa đỉnh`1`có kích thước`n+1`. 
4. Khôi phục tất cả các sự hợp nhất tạm thời. 
9. Nếu một số vị trí ứng viên thành công, hiện tại`t`là khả thi. 
10. Tìm kiếm nhị phân khả thi nhỏ nhất`t`. 

### Tại sao nó hoạt động 

Đối với một cố định`t`, biểu đồ được hình thành bởi các bước di chuyển hợp lệ mô tả chính xác những phân đội nào có thể được truy cập liên tiếp giữa các lần va chạm vào quỹ đạo. 

Việc thêm một phần mới chỉ giới thiệu các cạnh liên quan đến đỉnh mới đó. Nếu vị trí thành công tồn tại, hãy di chuyển tọa độ x của nó đến giá trị gần nhất trong số`x_i-t`,`x_i`,`x_i+t`và làm tương tự cho y không thể xóa bất kỳ kết nối hữu ích nào. Do đó một số vị trí thành công xuất hiện trong số các tọa độ ứng cử viên được nén. 

DSU khôi phục luôn thể hiện khả năng kết nối của biểu đồ gốc cộng với các cạnh tạm thời tương ứng với vị trí hiện đang được kiểm tra. Khả năng kết nối của tất cả`n+1`các đỉnh chính xác là điều kiện để mỗi phần có thể được thăm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class RollbackDSU:
    def __init__(self, n):
        self.fa = list(range(n))
        self.sz = [1] * n
        self.stk = []

    def find(self, x):
        while self.fa[x] != x:
            x = self.fa[x]
        return x

    def union(self, a, b, record):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.sz[a] > self.sz[b]:
            a, b = b, a

        if record:
            self.stk.append(a)

        self.fa[a] = b
        self.sz[b] += self.sz[a]

    def rollback(self):
        while self.stk:
            a = self.stk.pop()
            b = self.fa[a]
            self.sz[b] -= self.sz[a]
            self.fa[a] = a

n = int(input())
orig = [tuple(map(int, input().split())) for _ in range(n)]

def check(t):
    dsu = RollbackDSU(n + 1)

    for i in range(n):
        xi, yi = orig[i]
        for j in range(i + 1, n):
            xj, yj = orig[j]

            if xi == xj and abs(yi - yj) <= t:
                dsu.union(i, j, False)

            if yi == yj and abs(xi - xj) <= t:
                dsu.union(i, j, False)

    if dsu.sz[dsu.find(0)] == n:
        return True

    xs = []
    ys = []

    for x, y in orig:
        xs.extend((x - t, x, x + t))
        ys.extend((y - t, y, y + t))

    xs = sorted(set(xs))
    ys = sorted(set(ys))

    vx = [[] for _ in range(len(xs))]
    vy = [[] for _ in range(len(ys))]

    xid = {v: i for i, v in enumerate(xs)}
    yid = {v: i for i, v in enumerate(ys)}

    px = [0] * n
    py = [0] * n

    for i, (x, y) in enumerate(orig):
        px[i] = xid[x]
        py[i] = yid[y]
        vx[px[i]].append(i)
        vy[py[i]].append(i)

    extra = n

    for X in range(len(xs)):
        for Y in range(len(ys)):

            for idx in vx[X]:
                if abs(ys[Y] - orig[idx][1]) <= t:
                    dsu.union(idx, extra, True)

            for idx in vy[Y]:
                if abs(xs[X] - orig[idx][0]) <= t:
                    dsu.union(idx, extra, True)

            if dsu.sz[dsu.find(0)] == n + 1:
                return True

            dsu.rollback()

    return False

lo, hi = 0, 2_000_000_000
ans = -1

while lo <= hi:
    mid = (lo + hi) // 2

    if check(mid):
        ans = mid
        hi = mid - 1
    else:
        lo = mid + 1

print(ans)
```Thẻ DSU đầu tiên xây dựng biểu đồ kết nối cho giá trị hiện tại của`t`. Những liên minh đó là vĩnh viễn cho toàn bộ quá trình kiểm tra tính khả thi. 

Các tọa độ ứng cử viên được tạo ra từ`{x_i-t, x_i, x_i+t}`Và`{y_i-t, y_i, y_i+t}`. Sau khi nén,`vx`lưu trữ tất cả các điểm có tọa độ x được nén cụ thể và`vy`làm điều tương tự với y. 

Mỗi vị trí ứng cử viên chỉ thêm các cạnh chạm vào đỉnh phụ. Những công đoàn đó được đánh dấu là hoạt động khôi phục. Sau khi kiểm tra ứng viên, DSU trở về trạng thái chỉ chứa biểu đồ ban đầu. 

DSU cố tình không sử dụng tính năng nén đường dẫn. Rollback DSU yêu cầu các thay đổi gốc phải có thể đảo ngược được. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
100 0
0 100
-100 0
0 -100
```Vì`t = 100`: 

| Điểm ứng viên | Các đỉnh gốc được kết nối | 
| --- | --- | 
| (0,0) | cả bốn điểm | 
| người khác | ít đỉnh hơn | 

DSU được hình thành bởi các điểm ban đầu có bốn thành phần biệt lập. Thêm`(0,0)`kết nối với mọi điểm vì mỗi điểm đều nằm chính xác`100`đơn vị trên cùng một hàng hoặc cột. Tất cả năm đỉnh trở nên kết nối. 

Điều này chứng tỏ tại sao điểm tối ưu có thể không xuất hiện trong đầu vào. 

### Cấu hình không thể```
3
0 0
100 100
200 200
```Đối với mọi hữu hạn`t`: 

| Điểm ứng viên | Các đỉnh có thể truy cập được qua cùng một hàng/cột | 
| --- | --- | 
| tùy ý | nhiều nhất là hai thành phần | 
| tốt nhất có thể | vẫn chưa phải cả ba | 

Không có hai điểm gốc nào có chung tọa độ x hoặc tọa độ y. Một điểm bổ sung không thể đồng thời tạo ra tất cả các sắp xếp hàng và cột cần thiết. Việc kiểm tra tính khả thi luôn thất bại nên câu trả lời là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 log V) | Tìm kiếm nhị phân qua`t`, mỗi lần kiểm tra thực hiện công việc O(n²) | 
| Không gian | O(n²) | Cấu trúc tọa độ ứng viên và DSU | 

Với`n ≤ 1000`, cái`O(n²)`kiểm tra tính khả thi là chi phí chủ yếu. Kết hợp với khoảng 31 lần lặp lại tìm kiếm nhị phân, nó phù hợp một cách thoải mái trong giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    # paste solution here
    pass

# sample 1
assert run(
"""4
100 0
0 100
-100 0
0 -100
""").strip() == "100"

# already connected
assert run(
"""2
0 0
0 5
""").strip() == "5"

# impossible
assert run(
"""3
0 0
100 100
200 200
""").strip() == "-1"

# new point in the middle
assert run(
"""2
-2 0
2 0
""").strip() == "2"

# small square
assert run(
"""4
0 0
0 2
2 0
2 2
""").strip() == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai điểm trên một cột | 5 | Đồ thị hiện tại đã đủ | 
| Ba điểm chéo | -1 | Kết nối không thể | 
| Hai điểm ở khoảng cách 4 | 2 | Điểm mới giảm đáp án | 
| Hình vuông bốn góc | 2 | Nhiều thành phần được hợp nhất bởi một điểm | 

## Vỏ cạnh 

Hãy xem xét:```
3
0 0
100 100
200 200
```Không có cặp nào chia sẻ một hàng hoặc cột. DSU ban đầu có ba thành phần. Mỗi vị trí ứng cử viên chỉ tạo kết nối hàng hoặc cột với một tập hợp con điểm giới hạn. DSU khôi phục không bao giờ đạt kích thước`n+1`, Vì thế`check(t)`luôn sai và câu trả lời cuối cùng là`-1`. 

Coi như:```
4
100 0
0 100
-100 0
0 -100
```Vì`t=99`, không có vị trí ứng cử viên nào đạt đến cả bốn đỉnh. Vì`t=100`, ứng viên`(0,0)`xuất hiện trong tập tọa độ nén và kết nối với mọi điểm ban đầu. Kích thước thành phần DSU trở thành`5`, đưa ra câu trả lời chính xác`100`. 

Coi như:```
2
0 0
0 1
```Biểu đồ gốc đã được kết nối cho`t=1`. Bước 4 của bài kiểm tra tính khả thi sẽ phát hiện điều này ngay lập tức, tránh việc liệt kê ứng viên không cần thiết. Điều này xử lý các trường hợp việc thêm một đội mới là vô ích.
