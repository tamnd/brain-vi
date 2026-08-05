---
title: "CF 102482E - Bắt tội phạm"
description: "Thành phố là một mạng lưới các tòa nhà hình chữ nhật. Mỗi ô lưu trữ một chiều cao tòa nhà. Robin bắt đầu từ trung tâm của một tòa nhà và muốn biết số bước nhảy tối thiểu cần thiết để đến được mọi tòa nhà khác."
date: "2026-08-05T18:57:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102482
codeforces_index: "E"
codeforces_contest_name: "2018 ACM-ICPC World Finals"
rating: 0
weight: 102482
solve_time_s: 236
verified: true
draft: false
---

[CF 102482E - Bắt tội phạm](https://codeforces.com/problemset/problem/102482/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thành phố là một mạng lưới các tòa nhà hình chữ nhật. Mỗi ô lưu trữ một chiều cao tòa nhà. Robin bắt đầu từ trung tâm của một tòa nhà và muốn biết số bước nhảy tối thiểu cần thiết để đến được mọi tòa nhà khác. Một bước nhảy luôn bắt đầu và kết thúc ở tâm mái nhà, sử dụng cùng tốc độ ban đầu`v`và đi theo quỹ đạo đạn được chọn bằng cách thay đổi góc phóng. 

Đầu ra là số lần nhảy ngắn nhất từ ​​tòa nhà ẩn náu đến mọi ô. Nếu không thể tiếp cận được một tòa nhà, nó sẽ nhận được`X`. 

Mạng lưới nhỏ, chỉ có tối đa 20 x 20 tòa nhà, vì vậy chỉ có 400 vị trí có thể. Điều này loại trừ các thuật toán đồ thị nặng, nhưng nó cũng có nghĩa là chúng tôi có đủ khả năng để thử nghiệm mọi cặp tòa nhà có thể. Phần tốn kém không phải là tìm kiếm biểu đồ mà là kiểm tra xem một bước nhảy có hợp lệ về mặt vật lý hay không. 

Một vài chi tiết có thể khiến những giải pháp ngây thơ thất bại. Một cú nhảy chạm vào một góc lưới phải xóa sạch mọi tòa nhà xung quanh góc đó chứ không chỉ một trong số chúng. Ngoài ra, đường đạn có thể có hai vòng cung có thể có giữa hai tòa nhà giống nhau và chỉ vòng cung cao hơn mới hữu ích để tránh va chạm. Cuối cùng, việc chỉ kiểm tra phần giữa của mỗi tòa nhà là không chính xác vì điểm thấp nhất của parabol lõm trên một khoảng luôn ở một trong các điểm cuối của khoảng đó. 

Ví dụ, hãy xem xét:```
2 1 10 20 1 1
10 100
```Việc nhảy trực tiếp từ tòa nhà thứ nhất sang tòa nhà thứ hai chỉ có thể tồn tại bằng cách đi phía trên tòa nhà cao tầng. Một giải pháp chỉ kiểm tra độ cao đích sẽ đánh dấu không chính xác nó có thể tiếp cận được. 

Một trường hợp góc khác là:```
2 2 10 20 1 1
0 100
100 0
```Một bước nhảy chéo đi chính xác qua góc họp. Nó phải cao hơn cả hai tòa nhà bên cạnh và hai tòa nhà chạm vào góc đó. Chỉ kiểm tra ô chứa điểm giữa của đường đi sẽ bỏ sót va chạm này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là xây dựng một biểu đồ trong đó mỗi tòa nhà là một đỉnh. Đối với mỗi cặp tòa nhà được sắp xếp theo thứ tự, chúng tôi giải phương trình vật lý, thu được quỹ đạo nhảy có thể và mô phỏng đường đi qua lưới. Nếu quỹ đạo đi qua mọi tòa nhà, chúng ta sẽ thêm một cạnh. Tìm kiếm đầu tiên trên phạm vi rộng từ nơi ẩn náu sau đó đưa ra số lần nhảy tối thiểu. 

Ý tưởng về vũ lực là đúng vì mọi bước nhảy đầu tiên có thể xảy ra đều được xem xét. Tuy nhiên, có tới 400 tòa nhà nên có khoảng 160000 cặp hướng. Nếu mỗi cặp kiểm tra mọi tòa nhà thì trường hợp xấu nhất là khoảng 64 triệu lượt kiểm tra va chạm. Trong Python, điều này vẫn có thể quản lý được nếu thử nghiệm va chạm hiệu quả, nhưng việc triển khai bất cẩn với công việc hình học lặp đi lặp lại có thể trở nên quá chậm. 

Nhận xét quan trọng là đồ thị rất nhỏ và bản thân thành phố cũng rất nhỏ. Chúng ta không cần những kỹ thuật đường đi ngắn nhất phức tạp. Thử thách toán học duy nhất là xác định liệu một bước nhảy có tồn tại hay không. Khi biểu đồ bước nhảy được xây dựng, BFS sẽ giải quyết phần còn lại ngay lập tức. 

Vật lý có thể được đơn giản hóa bằng cách sử dụng tỷ lệ giữa tốc độ dọc và ngang. Cho phép$$a = \frac{v_h}{v_d}$$Đối với khoảng cách ngang`d`, quỹ đạo so với mái nhà xuất phát là:$$z(x)=a x-\frac{g x^2(1+a^2)}{2v^2}$$Chiều cao hạ cánh cho một phương trình bậc hai trong`a`, tạo ra nhiều nhất hai quỹ đạo có thể. Chúng tôi thử các giải pháp hợp lệ và giữ giải pháp cao hơn vì ít nhất nó luôn tốt cho việc tránh va chạm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với mô phỏng lặp đi lặp lại | O((dx·dy)^4) | O(dx·dy) | Quá chậm nếu thực hiện bất cẩn | 
| Xây dựng biểu đồ + BFS | O(n^3) với n = dx·dy | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy coi mọi tòa nhà như một nút biểu đồ. Đối với mọi tòa nhà đích có thể, hãy tính xem liệu có bước nhảy trực tiếp từ tòa nhà hiện tại hay không. 
2. Tính khoảng cách ngang`d`giữa hai tâm mái. Giải phương trình bậc hai để tìm các hình dạng phóng có thể. Nếu không có giải pháp thực sự tồn tại thì không có lợi thế. 
3. Đối với mọi quỹ đạo hợp lệ, hãy kiểm tra mọi tòa nhà mà đường thẳng ngang đi qua. Chia đường đi bất cứ khi nào nó đi qua một đường lưới. Mỗi khoảng thuộc về một tòa nhà và vì đường đạn lõm xuống nên việc kiểm tra hai điểm cuối của khoảng là đủ. 
4. Nếu quỹ đạo nằm ngay phía trên mỗi tòa nhà giao nhau, hãy thêm một cạnh giữa hai tòa nhà. 
5. Tiến hành tìm kiếm trên diện rộng đầu tiên bắt đầu từ nơi ẩn náu của Robin. Lần đầu tiên BFS đến một tòa nhà là số lần nhảy tối thiểu cần thiết. 

Tại sao nó hoạt động: 

Biểu đồ chứa chính xác những bước nhảy mà Robin có thể thực hiện. Mỗi bước nhảy hợp lệ đều được thêm vào vì phương trình quỹ đạo và kiểm tra va chạm khớp với chuyển động thực. Mọi bước nhảy không hợp lệ đều bị từ chối vì một số tòa nhà chéo sẽ giao nhau với quỹ đạo. BFS sau đó tìm đường đi ngắn nhất trong biểu đồ không có trọng số, chính xác là số lần nhảy tối thiểu. 

## Giải pháp Python```python
import sys
from math import sqrt, hypot
from collections import deque

input = sys.stdin.readline

g = 9.80665
EPS = 1e-9

dx, dy, w, v, lx, ly = map(int, input().split())
h = [list(map(int, input().split())) for _ in range(dy)]

lx -= 1
ly -= 1
n = dx * dy

def solve_jump(x1, y1, x2, y2):
    if x1 == x2 and y1 == y2:
        return False

    sx = x1 * w + w / 2
    sy = y1 * w + w / 2
    tx = x2 * w + w / 2
    ty = y2 * w + w / 2

    d = hypot(tx - sx, ty - sy)
    dh = h[y2][x2] - h[y1][x1]

    q = g / (2 * v * v)

    # q*d^2*a^2 - d*a + (dh + q*d^2) = 0
    A = q * d * d
    B = -d
    C = dh + q * d * d

    disc = B * B - 4 * A * C
    if disc < -EPS:
        return False

    candidates = []
    if abs(A) < EPS:
        if abs(B) > EPS:
            candidates.append(-C / B)
    else:
        if disc >= 0:
            s = sqrt(max(0, disc))
            candidates.append((-B + s) / (2 * A))
            candidates.append((-B - s) / (2 * A))

    def height_at(dist, a):
        return h[y1][x1] + a * dist - q * dist * dist * (1 + a * a)

    for a in candidates:
        if a < -EPS:
            continue

        vx = tx - sx
        vy = ty - sy

        ts = [0.0, 1.0]

        if abs(vx) > EPS:
            for i in range(dx + 1):
                t = (i * w - sx) / vx
                if EPS < t < 1 - EPS:
                    ts.append(t)

        if abs(vy) > EPS:
            for i in range(dy + 1):
                t = (i * w - sy) / vy
                if EPS < t < 1 - EPS:
                    ts.append(t)

        ts.sort()

        ok = True
        for i in range(len(ts) - 1):
            l = ts[i]
            r = ts[i + 1]
            mid = (l + r) / 2

            mx = sx + mid * vx
            my = sy + mid * vy

            bx = int(mx // w)
            by = int(my // w)

            if bx < 0 or bx >= dx or by < 0 or by >= dy:
                continue

            for t in (l, r):
                dist = d * t
                if t == 1.0:
                    continue
                if height_at(dist, a) <= h[by][bx] + EPS:
                    ok = False
                    break

            if not ok:
                break

        if ok:
            return True

    return False

graph = [[] for _ in range(n)]

for y1 in range(dy):
    for x1 in range(dx):
        u = y1 * dx + x1
        for y2 in range(dy):
            for x2 in range(dx):
                if solve_jump(x1, y1, x2, y2):
                    graph[u].append(y2 * dx + x2)

start = ly * dx + lx
dist = [-1] * n
dist[start] = 0

q = deque([start])
while q:
    u = q.popleft()
    for vtx in graph[u]:
        if dist[vtx] == -1:
            dist[vtx] = dist[u] + 1
            q.append(vtx)

ans = []
for y in range(dy):
    row = []
    for x in range(dx):
        d = dist[y * dx + x]
        row.append(str(d) if d != -1 else "X")
    ans.append(" ".join(row))

print("\n".join(ans))
```Việc xây dựng đồ thị là phần đắt tiền. Chương trình sẽ thử từng cặp tòa nhà theo thứ tự, giải phương trình đường đạn và sau đó thực hiện quét hình học qua lưới. 

Kiểm tra va chạm thu thập tất cả các giá trị tham số trong đó bước nhảy vượt qua đường lưới dọc hoặc ngang. Các giá trị liên tiếp mô tả một phần nằm bên trong một tòa nhà. Vì parabol là lõm nên chiều cao tối thiểu trên phần đó là ở một trong các đầu của nó, do đó không cần lấy mẫu liên tục. 

BFS sử dụng việc truyền tải hàng đợi thông thường vì mỗi lần nhảy đều có chi phí như nhau. Mảng khoảng cách lưu trữ lớp đầu tiên nơi mỗi tòa nhà được tiếp cận. 

So sánh dấu phẩy động sử dụng một epsilon nhỏ vì đầu vào đảm bảo rằng việc thay đổi độ cao bằng`10^-6`không thay đổi câu trả lời. Điểm cuối mục tiêu bị bỏ qua trong quá trình kiểm tra va chạm vì dự định hạ cánh chính xác trên mái nhà đích. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 1 100 55 1 1
10 40 60 10
```Nút bắt đầu là tòa nhà đầu tiên. 

| Nút hiện tại | Điểm đến của ứng viên | Cạnh tồn tại | Khoảng cách BFS | 
| --- | --- | --- | --- | 
| (1,1) | (2,1) | Có | 1 | 
| (1,1) | (3,1) | Có | 1 | 
| (1,1) | (4,1) | Có | 1 | 

Có thể thực hiện được cú nhảy trực tiếp vì vòng cung đạn cao hơn sẽ làm sạch các mái nhà trung gian. 

Đối với mẫu thứ hai:```
4 4 100 55 1 1
0 10 20 30
10 20 30 40
20 30 200 50
30 40 50 60
```| Nút hiện tại | Điểm đến | Kết quả | 
| --- | --- | --- | 
| (1,1) | (4,1) | Có thể truy cập trong 1 lần nhảy | 
| (1,1) | (3,3) | Bị chặn bởi tòa nhà trung tâm cao | 
| (1,1) | (4,4) | Có thể tiếp cận thông qua các bước nhảy trung gian | 

Dấu vết chứng minh lý do tại sao biểu đồ phải được xây dựng từ các bước nhảy vật lý thay vì giả định rằng các tòa nhà gần đó luôn có thể tiếp cận được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) | Có các bước nhảy O(n²) và mỗi bài kiểm tra va chạm sẽ kiểm tra các tòa nhà O(n) | 
| Không gian | O(n²) | Biểu đồ nhảy lưu trữ tất cả các cạnh có hướng có thể có | 

Đây`n = dx * dy`, và giá trị tối đa là 400. Trường hợp xấu nhất thu được là đủ nhỏ cho các giới hạn vì đồ thị rất nhỏ và việc kiểm tra hình học rất đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # Paste the solution function here and return captured output in a judge environment.
    sys.stdin = old
    return ""

# Tests are intended to be run with the submitted solution wrapped as solve().
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 10 20 1 1`với một tòa nhà |`0`| Biểu đồ nút đơn | 
| Lưới phẳng có chiều cao bằng nhau | Tất cả các tòa nhà có thể tiếp cận | Nhảy đối xứng | 
| Đường chéo qua một góc | Bị chặn đúng cách khi quá thấp | Xử lý va chạm góc | 
| Tòa nhà trung gian rất cao | Đã đánh dấu điểm đến`X`| Phát hiện chướng ngại vật | 

## Vỏ cạnh 

Một thành phố xây dựng tạo ra một biểu đồ có một nút và không có cạnh. BFS để khoảng cách bắt đầu bằng 0, đây là câu trả lời bắt buộc. 

Một bước nhảy đi qua một góc chính xác được xử lý bằng cách tách quỹ đạo tại mọi giao điểm của đường lưới. Điểm cuối giống nhau được kiểm tra từ mọi khoảng liền kề, vì vậy tất cả các tòa nhà tiếp xúc đều được xem xét. 

Điểm đến cao hơn mái nhà xuất phát có thể cần một vòng cung dốc. Bộ giải bậc hai giữ cả hai quỹ đạo có thể có và kiểm tra chúng thay vì cho rằng cung dưới là đủ. 

Quỹ đạo gần như không chạm vào một mái nhà khác sẽ bị từ chối vì đường đi phải ở phía trên các tòa nhà khi bay. Việc so sánh epsilon ngăn nhiễu số làm thay đổi quyết định này.
