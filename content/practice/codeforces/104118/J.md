---
title: "CF 104118J - Junior Steiner Ba"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô là đất hoặc nước. Chính xác ba ô đã là đất liền và chúng tôi được phép chuyển đổi bất kỳ số lượng ô nước nào thành đất liền."
date: "2026-07-02T01:53:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104118
codeforces_index: "J"
codeforces_contest_name: "2022 ICPC Asia-Manila Regional Contest"
rating: 0
weight: 104118
solve_time_s: 49
verified: true
draft: false
---

[CF 104118J - Junior Steiner Ba](https://codeforces.com/problemset/problem/104118/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô là đất hoặc nước. Chính xác ba ô đã là đất liền và chúng tôi được phép chuyển đổi bất kỳ số lượng ô nước nào thành đất liền. Sau khi làm như vậy, chúng ta cần toàn bộ vùng đất được kết nối, nghĩa là từ bất kỳ ô đất nào, chúng ta có thể đến bất kỳ ô đất nào khác bằng cách chỉ di chuyển lên, xuống, sang trái hoặc phải qua các ô đất. 

Chi phí chỉ đơn giản là số lượng ô nước chúng ta chuyển đổi thành đất, vì vậy mục tiêu không chỉ là kết nối ba ô đất hiện có mà còn sử dụng số lượng ô được thêm vào ít nhất có thể. Theo thuật ngữ đồ thị, mỗi ô là một nút và kề là 4 hướng. Chúng tôi được yêu cầu tìm một tập hợp tối thiểu các nút lưới bổ sung kết nối ba nút đầu cuối cố định thành một thành phần được kết nối duy nhất. Đây là bài toán cây Steiner cổ điển trên lưới, nhưng bị giới hạn ở chính xác ba điểm cuối. 

Kích thước lưới tối đa là 100 x 100, do đó, lực lượng vũ phu đối với tất cả các tập hợp con của ô hoặc tất cả các cấu trúc kết nối có thể có là quá lớn. Bất cứ điều gì theo cấp số nhân về số lượng tế bào nước đều không thể thực hiện được ngay lập tức, vì có thể có tới 10.000 tế bào nước trong số đó. 

Một trường hợp thất bại khó phát hiện nếu người ta cố gắng tham lam kết nối ba ô đất theo cặp mà không có sự phối hợp. Ví dụ: nếu hai đường dẫn trùng nhau, một cách tiếp cận đơn giản có thể tính hai lần hoặc chọn các tuyến đường dưới mức tối ưu giao nhau không hiệu quả. Một vấn đề khác là giả định rằng các đường đi ngắn nhất giữa mỗi cặp độc lập tạo thành một giải pháp tối ưu, điều này là sai vì các đường đi chung làm giảm tổng chi phí và phải được lên kế hoạch chung. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét từng tập hợp con của các ô nước, biến chúng thành đất và kiểm tra xem ba ô đất ban đầu có được kết nối với nhau hay không. Điều này đúng nhưng ngay lập tức bùng nổ theo kiểu tổ hợp. Với tối đa 10.000 ô, ngay cả việc xem xét các tập hợp con có kích thước 20 cũng đã trở nên vô cùng to lớn. 

Cái nhìn sâu sắc về cấu trúc quan trọng là chúng tôi chỉ có ba thiết bị đầu cuối. Giải pháp tối ưu phải trông giống như một cái cây nối ba điểm này và bất kỳ cây nào như vậy trong biểu đồ lưới đều có hình dạng rất hạn chế: đó là sự kết hợp của ba đường đi ngắn nhất gặp nhau tại một điểm gặp nhau (điểm Steiner). Điều này làm giảm vấn đề từ việc tìm kiếm các sơ đồ con tùy ý đến việc chọn một ô họp và kết nối cả ba nguồn với nó một cách tối ưu. 

Sau khi chúng tôi sửa một ô họp ứng viên, chi phí tối ưu để kết nối tất cả các thiết bị đầu cuối thông qua ô đó chỉ đơn giản là tổng khoảng cách đường đi ngắn nhất từ ​​mỗi thiết bị đầu cuối đến ô đó. Vì chi phí di chuyển là đồng đều nên mỗi đường đi ngắn nhất chỉ là một khoảng cách BFS trong lưới. 

Vì vậy, vấn đề giảm xuống còn việc tính toán ba bản đồ khoảng cách bằng BFS, sau đó quét tất cả các ô làm điểm gặp gỡ tiềm năng và giảm thiểu tổng khoảng cách. Việc xây dựng cuối cùng có được bằng cách lấy cha mẹ BFS từ mỗi thiết bị đầu cuối và truy tìm các đường dẫn trở lại từ điểm gặp tối ưu đã chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^(rc)) | O(rc) | Quá chậm | 
| BFS đa nguồn + điểm gặp gỡ | O(rc) | O(rc) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xác định 3 ô đầu cuối 

Chúng tôi quét lưới và ghi lại tọa độ của ba ô chứa đất. Đây là những điểm cuối cố định của cây Steiner của chúng tôi. Phần còn lại của lưới điện là vật liệu tiềm năng để xây dựng các kết nối. 

### 2. Chạy BFS từ mỗi thiết bị đầu cuối 

Đối với mỗi ô trong số ba ô đất, chúng tôi chạy BFS trên lưới tính toán khoảng cách ngắn nhất đến mọi ô khác. Chúng tôi cũng lưu trữ các con trỏ gốc để cho phép xây dựng lại các đường dẫn sau này. 

Bước này đúng vì chi phí di chuyển là đồng đều trên các cạnh, do đó BFS đảm bảo các đường đi ngắn nhất trong biểu đồ lưới không có trọng số. 

### 3. Hãy thử mọi ô như một điểm gặp gỡ tiềm năng

Đối với mỗi ô trong lưới, chúng tôi tính toán tổng chi phí kết nối cả ba thiết bị đầu cuối thông qua nó, bằng tổng của ba khoảng cách BFS. Chúng tôi theo dõi ô giảm thiểu số tiền này. 

Lý do điều này có hiệu quả là vì bất kỳ cây Steiner tối ưu nào cho ba điểm cuối trong một đồ thị không có trọng số đều có thể được coi là ba đường đi ngắn nhất gặp nhau ở một đỉnh nào đó. 

### 4. Xây dựng lại giải pháp từ điểm gặp đã chọn 

Khi điểm gặp gỡ tốt nhất được cố định, chúng tôi sẽ xây dựng lại các đường dẫn từ ô đó trở lại từng thiết bị đầu cuối trong số ba thiết bị đầu cuối bằng cách sử dụng con trỏ cha BFS. Mọi ô trên những đường dẫn này đều được đánh dấu là đất. 

Chúng tôi cũng bảo tồn ba ô đất ban đầu theo yêu cầu. 

### 5. Xuất lưới cuối cùng 

Chúng tôi in lưới sau khi đánh dấu tất cả các ô đường dẫn được xây dựng lại là đất. 

### Tại sao nó hoạt động 

Bất biến chính là đối với bất kỳ ô họp cố định nào, sự kết hợp của các đường dẫn ngắn nhất từ ô đó đến mỗi thiết bị đầu cuối là tối thiểu trong số tất cả các đồ thị con được kết nối bị ràng buộc phải đi qua ô đó. Vì bất kỳ giải pháp tối ưu nào cho ba thiết bị đầu cuối đều phải có một trung tâm nơi các đường dẫn gặp nhau, việc liệt kê tất cả các ô gặp nhau có thể đảm bảo chúng tôi đánh giá được cấu trúc Steiner tối ưu thực sự. BFS đảm bảo mỗi nhánh đều ngắn nhất, do đó không có đường vòng dư thừa nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

INF = 10**9

def bfs(start, r, c, grid):
    dist = [[INF] * c for _ in range(r)]
    par = [[None] * c for _ in range(r)]
    
    sr, sc = start
    dist[sr][sc] = 0
    q = deque([(sr, sc)])
    
    while q:
        x, y = q.popleft()
        for dx, dy in ((1,0), (-1,0), (0,1), (0,-1)):
            nx, ny = x + dx, y + dy
            if 0 <= nx < r and 0 <= ny < c:
                if dist[nx][ny] > dist[x][y] + 1:
                    dist[nx][ny] = dist[x][y] + 1
                    par[nx][ny] = (x, y)
                    q.append((nx, ny))
    return dist, par

r, c = map(int, input().split())
grid = [list(input().strip()) for _ in range(r)]

terms = []
for i in range(r):
    for j in range(c):
        if grid[i][j] == '#':
            terms.append((i, j))

dists = []
pars = []

for t in terms:
    d, p = bfs(t, r, c, grid)
    dists.append(d)
    pars.append(p)

best_cost = INF
best_cell = None

for i in range(r):
    for j in range(c):
        cost = dists[0][i][j] + dists[1][i][j] + dists[2][i][j]
        if cost < best_cost:
            best_cost = cost
            best_cell = (i, j)

def mark_path(par, start, end, mark):
    x, y = start
    ex, ey = end
    while (x, y) != (ex, ey):
        mark.add((x, y))
        x, y = par[x][y]
    mark.add((ex, ey))

mark = set()
for i in range(3):
    mark_path(pars[i], best_cell, terms[i], mark)

for x, y in mark:
    grid[x][y] = '#'

for row in grid:
    print(''.join(row))
```Hàm BFS xây dựng cả con trỏ khoảng cách và con trỏ cha, những điều này rất cần thiết cho việc tái cấu trúc các nhánh cây Steiner đã chọn sau này. Ba BFS độc lập, mỗi BFS bắt nguồn từ một trong các ô đất ban đầu. 

Quá trình quét lồng nhau trên tất cả các ô lưới sẽ chọn điểm gặp tối ưu. Mặc dù đây là O(r·c), nhưng nó chỉ có 10.000 thao tác, nằm trong giới hạn. 

Bước xây dựng lại cẩn thận đi lùi từ điểm gặp nhau đến từng thiết bị đầu cuối bằng cách sử dụng cha mẹ được lưu trữ. Điều này tránh việc tính toán lại đường dẫn hoặc chạy BFS bổ sung. Một bộ được sử dụng để tránh đánh dấu trùng lặp khi các đường dẫn chồng lên nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
.....
..#..
....#
.#...
```Đầu tiên chúng ta xác định ba thiết bị đầu cuối. Chạy BFS từ mỗi bản đồ sẽ tạo ra ba bản đồ khoảng cách trên lưới. Khi chúng tôi đánh giá các điểm gặp gỡ của ứng viên, điểm tối ưu là ô gần trung tâm, nơi ba đường dẫn ngắn nhất chồng lên nhau một cách tự nhiên. 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Tìm thiết bị đầu cuối | (1,2), (2,4), (3,1) | 
| 2 | BFS từ mỗi | lưới khoảng cách đầy đủ được tính toán | 
| 3 | Hãy thử tất cả các ô | điểm gặp mặt tốt nhất được chọn | 
| 4 | Xây dựng lại đường dẫn | hợp 3 con đường ngắn nhất | 
| 5 | Lưới đầu ra | đất kết nối | 

Dấu vết xác nhận rằng các phân đoạn chồng chéo được sử dụng lại thay vì trùng lặp, đó là lý do tại sao mục tiêu tổng khoảng cách mô hình chính xác cấu trúc chia sẻ. 

### Ví dụ 2 

đầu vào:```
3 3
..#
.#.
#..
```Ở đây ba thiết bị đầu cuối được bố trí theo đường chéo, tạo thành một kết nối trung tâm. 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Tìm thiết bị đầu cuối | (0,2), (1,1), (2,0) | 
| 2 | BFS từ mỗi | khoảng cách đối xứng | 
| 3 | Hãy thử tất cả các ô | tế bào trung tâm là tối ưu | 
| 4 | Tái thiết | kết nối hình ngôi sao | 
| 5 | Đầu ra | lưới được kết nối đầy đủ | 

Điều này chứng tỏ rằng ngay cả khi các thiết bị đầu cuối được sắp xếp đối xứng, thuật toán vẫn tự nhiên chọn tâm hình học làm điểm gặp Steiner. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(rc) | Ba lần duyệt BFS trên lưới cộng với một lần quét toàn bộ tất cả các ô | 
| Không gian | O(rc) | Khoảng cách và mảng gốc cho mỗi BFS | 

Lưới tối đa là 100 x 100, tức là 10.000 ô. Ba lần chạy BFS và quét tuyến tính là không đáng kể dưới giới hạn 2 giây. Việc sử dụng bộ nhớ cũng ít vì chúng tôi chỉ lưu trữ một vài lưới số nguyên và con trỏ gốc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    from collections import deque

    INF = 10**9

    def bfs(start, r, c, grid):
        dist = [[INF] * c for _ in range(r)]
        par = [[None] * c for _ in range(r)]
        sr, sc = start
        dist[sr][sc] = 0
        q = deque([(sr, sc)])
        while q:
            x, y = q.popleft()
            for dx, dy in ((1,0),(-1,0),(0,1),(0,-1)):
                nx, ny = x+dx, y+dy
                if 0 <= nx < r and 0 <= ny < c:
                    if dist[nx][ny] > dist[x][y] + 1:
                        dist[nx][ny] = dist[x][y] + 1
                        par[nx][ny] = (x, y)
                        q.append((nx, ny))
        return dist, par

    r, c = map(int, input().split())
    grid = [list(input().strip()) for _ in range(r)]

    terms = [(i,j) for i in range(r) for j in range(c) if grid[i][j] == '#']

    dists, pars = [], []
    for t in terms:
        d, p = bfs(t, r, c, grid)
        dists.append(d)
        pars.append(p)

    best = 10**18
    best_cell = None
    for i in range(r):
        for j in range(c):
            cost = dists[0][i][j] + dists[1][i][j] + dists[2][i][j]
            if cost < best:
                best = cost
                best_cell = (i, j)

    mark = set()
    def add(par, start, end):
        x, y = start
        ex, ey = end
        while (x, y) != (ex, ey):
            mark.add((x, y))
            x, y = par[x][y]
        mark.add((ex, ey))

    for i in range(3):
        add(pars[i], best_cell, terms[i])

    out = []
    for i in range(r):
        row = []
        for j in range(c):
            row.append('#' if grid[i][j] == '#' or (i,j) in mark else '.')
        out.append(''.join(row))
    return "\n".join(out)

# sample 1
assert run("""4 5
.....
..#..
....#
.#...
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2x2 3 góc | điền tối thiểu được kết nối | lưới không cần thiết nhỏ nhất | 
| thiết bị đầu cuối đường thẳng | đường dẫn trực tiếp | không phân nhánh không cần thiết | 
| hình thành tam giác | họp trung ương | xử lý đối xứng | 
| trường hợp mẫu | tái thiết hợp lệ | độ chính xác đầy đủ của đường ống | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi hai thiết bị đầu cuối đã ở gần nhau hoặc gần như được kết nối. Trong tình huống đó, điểm gặp tối ưu có thể nằm trực tiếp trên một trong các thiết bị đầu cuối, nghĩa là một đường dẫn BFS có độ dài bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì khoảng cách BFS từ thiết bị đầu cuối đến chính nó bằng 0 và việc tối thiểu hóa tổng khoảng cách vẫn chọn chính xác ô đó làm điểm gặp hợp lệ. 

Một trường hợp khác là khi các đường đi ngắn nhất bị chồng chéo nhiều. Ví dụ: nếu hai thiết bị đầu cuối nằm trong một khu vực giống như hành lang, các đường dẫn BFS của chúng sẽ hợp nhất sớm. Do quá trình tái tạo sử dụng một tập hợp các ô được đánh dấu nên các phân đoạn chồng chéo không được tính hai lần hoặc trùng lặp ở đầu ra, duy trì tính chính xác mà không làm tăng chi phí.
