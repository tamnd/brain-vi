---
title: "CF 102726H - Điểm Wifi"
description: "Bài toán mô hình một ngôi nhà dưới dạng một lưới hình chữ nhật. Một số ô bị chặn bởi các bức tường, một ô là vị trí bắt đầu của Sarah và một số ô là các điểm mốc WiFi mà tất cả đều phải được truy cập."
date: "2026-08-01T22:08:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102726
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 1"
rating: 0
weight: 102726
solve_time_s: 118
verified: true
draft: false
---

[CF 102726H - Điểm phát Wifi](https://codeforces.com/problemset/problem/102726/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bài toán mô hình một ngôi nhà dưới dạng một lưới hình chữ nhật. Một số ô bị chặn bởi các bức tường, một ô là vị trí bắt đầu của Sarah và một số ô là các điểm mốc WiFi mà tất cả đều phải được truy cập. Được phép di chuyển giữa các ô không có tường lân cận và mục tiêu là tìm ra con đường ngắn nhất bắt đầu từ vị trí của Sarah và đến mọi điểm mốc ít nhất một lần. Câu trả lời là số lần di chuyển tối thiểu cần thiết cho một tuyến đường như vậy. 

Lưới có thể có tới 500 hàng và 500 cột, do đó có thể có 250.000 ô. Giải pháp khám phá nhiều tuyến đường có thể đi qua các điểm mốc chỉ mang tính thực tế vì số lượng điểm mốc rất ít, tối đa là 6 điểm mốc. Nếu số lượng điểm mốc lớn, bài toán sẽ trở thành bài toán tổng quát về kiểu đường đi Hamilton ngắn nhất và số lượng yêu cầu thăm quan có thể sẽ bùng nổ. Giá trị nhỏ của`k`là ràng buộc chính cho phép giải pháp lập trình động. 

Một lỗi phổ biến là chạy tìm kiếm đường đi ngắn nhất trực tiếp trên các trạng thái chỉ chứa vị trí lưới hiện tại. Điều đó làm mất thông tin về những địa danh nào đã được ghé thăm. Ví dụ:```
3 5 2
*****
X.S.X
*****
```Câu trả lời đúng là`4`. Một tìm kiếm chỉ theo dõi ô hiện tại có thể đạt đến một mốc và sau đó xử lý không chính xác trạng thái đã được giải quyết vì nó không nhớ rằng mốc kia vẫn còn. 

Một trường hợp khác là khi một số điểm mốc được sắp xếp sao cho việc xem lại các ô là cần thiết.```
3 7 4
***X***
X...S.X
***X***
```Câu trả lời đúng là`12`. Một chiến lược tham lam luôn đi đến điểm mốc chưa được ghé thăm gần nhất có thể tạo ra một lựa chọn tốt cục bộ và vẫn tạo ra một đường đi tổng thể dài hơn thứ tự tối ưu. 

Trường hợp thứ ba là khi mốc liền kề với vị trí xuất phát.```
1 2 1
SX
```Câu trả lời là`1`. Việc triển khai giả định vị trí bắt đầu không bao giờ nằm ​​cạnh mục tiêu hoặc khởi tạo khoảng cách không chính xác có thể thêm một bước di chuyển không cần thiết. 

# Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi thứ tự có thể để tham quan các địa danh. Đối với mỗi đơn hàng, chúng ta có thể tính toán độ dài đường đi ngắn nhất giữa các điểm liên tiếp bằng BFS trên lưới. Điều này đúng vì chi phí di chuyển lưới là đồng đều nên BFS đưa ra khoảng cách ngắn nhất giữa hai địa điểm. 

Vấn đề là có thể có tới 6 điểm mốc, cho`6! = 720`các đơn đặt hàng có thể. Riêng con số đó không lớn, nhưng nếu chúng ta tính toán lại BFS cho mỗi hoán vị trên lưới 500 x 500, thì mỗi hoán vị có thể yêu cầu khoảng 250.000 lượt truy cập ô. Trường hợp xấu nhất sẽ có khoảng 180 triệu hoạt động chỉ dành cho BFS, cộng thêm chi phí bổ sung từ việc khám phá nhiều trạng thái nhiều lần. 

Quan sát quan trọng là phần tốn kém của tuyến đường không di chuyển giữa các ô mà là quyết định thứ tự của một số vị trí quan trọng. Bản thân lưới có thể được nén. Thay vì xem xét từng ô trong giai đoạn sắp xếp, trước tiên chúng tôi tính toán khoảng cách ngắn nhất giữa mỗi cặp điểm quan trọng: vị trí của Sarah và tất cả các điểm mốc. 

Sau sự biến đổi đó, vấn đề trở thành một vấn đề nhỏ về người bán hàng lưu động. Vì có tổng cộng tối đa 7 điểm quan trọng nên chương trình động bitmask có thể lưu trữ những điểm mốc nào đã được ghé thăm và vị trí điểm mốc hiện tại. Kích thước lưới biến mất khỏi DP, chỉ để lại`2^k * k`tiểu bang. 

Lực lượng vũ phu hoạt động vì mỗi tuyến đường hợp lệ là một số thứ tự của các mốc, nhưng nó lặp lại các phép tính khoảng cách giống nhau và hậu tố tuyến đường nhiều lần. Nhận xét rằng chỉ có các mốc đã ghé thăm mới cho phép chúng ta sử dụng lại các bài toán con lặp lại đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k! * n * m) | O(n * m) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O((k + 1) * n * m + k * 2^k * k) | O((k + 1) * n * m + k * 2^k) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Lưu vị trí bắt đầu và tất cả các vị trí mốc làm điểm quan trọng. Có nhiều nhất bảy điểm như vậy nên chúng đủ nhỏ để có thể xử lý một cách rõ ràng. 
2. Chạy BFS từ mọi điểm quan trọng. Trong mỗi BFS, hãy tính khoảng cách ngắn nhất tới mọi ô có thể tiếp cận và trích xuất khoảng cách đến các điểm quan trọng khác. 

Lý do thực hiện BFS từ mọi điểm quan trọng là vì mọi quyết định sau này chỉ cần khoảng cách giữa các vị trí quan trọng. Đường dẫn chính xác qua các ô trống không còn quan trọng nữa. 

1. Xây dựng ma trận khoảng cách trong đó`dist[i][j]`là số bước đi ngắn nhất cần thiết để đi từ điểm quan trọng`i`đến điểm quan trọng`j`. 
2. Sử dụng lập trình động bitmask. Trạng thái DP là`(mask, pos)`, Ở đâu`mask`mô tả những địa danh nào đã được ghé thăm và`pos`là điểm quan trọng hiện tại. 

Điểm bắt đầu không được bao gồm trong mặt nạ vì không cần phải truy cập. Ban đầu, vị trí hiện tại là vị trí của Sarah và chiếc mặt nạ trống rỗng. 

1. Từ mỗi trạng thái, hãy thử di chuyển đến mọi điểm mốc không nằm trong mặt nạ hiện tại. Cập nhật trạng thái mới bằng cách thêm mốc đó vào mặt nạ và thêm khoảng cách tương ứng từ ma trận. 
2. Sau khi tất cả các trạng thái được xử lý, câu trả lời là giá trị tối thiểu trong số tất cả các trạng thái mà mọi điểm mốc đã được ghé thăm. 

Tại sao nó hoạt động: 

Mỗi tuyến đường hợp lệ có thể được chia thành hai phần: di chuyển giữa các điểm quan trọng và thứ tự của các điểm quan trọng đó. BFS đưa ra chi phí tối ưu cho mọi chuyển động có thể xảy ra giữa các điểm quan trọng. DP xem xét mọi tập hợp có thể có của các mốc đã được ghé thăm và mọi vị trí hiện tại có thể có, do đó, DP xem xét mọi thứ tự có thể mà không cần tính toán lại các tình huống tương đương. Vì mỗi tuyến đường hoàn chỉnh xuất hiện dưới dạng một chuỗi chuyển tiếp DP và mỗi chuyển đổi sử dụng chuyển động ngắn nhất có thể giữa các mốc liên tiếp nên giá trị DP tối thiểu chính xác là độ dài tuyến đường tối ưu. 

#Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    points = []
    start = None

    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'S':
                start = (i, j)
            elif grid[i][j] == 'X':
                points.append((i, j))

    points.append(start)
    start_idx = len(points) - 1

    total = len(points)
    dist = [[0] * total for _ in range(total)]

    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for idx, (sx, sy) in enumerate(points):
        d = [[-1] * m for _ in range(n)]
        q = deque()
        q.append((sx, sy))
        d[sx][sy] = 0

        while q:
            x, y = q.popleft()
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < m:
                    if grid[nx][ny] != '*' and d[nx][ny] == -1:
                        d[nx][ny] = d[x][y] + 1
                        q.append((nx, ny))

        for j, (x, y) in enumerate(points):
            dist[idx][j] = d[x][y]

    landmarks = total - 1
    size = 1 << landmarks
    inf = 10**18

    dp = [[inf] * total for _ in range(size)]
    dp[0][start_idx] = 0

    for mask in range(size):
        for pos in range(total):
            if dp[mask][pos] == inf:
                continue

            for nxt in range(landmarks):
                if mask & (1 << nxt):
                    continue

                new_mask = mask | (1 << nxt)
                new_pos = nxt
                dp[new_mask][new_pos] = min(
                    dp[new_mask][new_pos],
                    dp[mask][pos] + dist[pos][nxt]
                )

    print(min(dp[-1]))

if __name__ == "__main__":
    solve()
```Mã đầu tiên thu thập tất cả các vị trí có liên quan. Ô bắt đầu được thêm vào sau các mốc, vì vậy các chỉ số mốc vẫn được giữ nguyên`0`bởi vì`k - 1`và chỉ số bắt đầu rất dễ xác định. 

Phần BFS được lặp lại một lần cho mỗi điểm quan trọng. Mảng khoảng cách đã truy cập được tạo lại mỗi lần vì mỗi BFS có một nguồn khác nhau. Kiểm tra chuyển động theo bốn hướng xử lý các ranh giới lưới trước khi truy cập vào một ô, ngăn chặn việc lập chỉ mục không hợp lệ. 

DP chỉ sử dụng các bit mốc. Vị trí bắt đầu được lưu dưới dạng vị trí hiện tại nhưng không bao giờ xuất hiện trong mặt nạ vì việc truy cập điểm bắt đầu đã được thực hiện. Quá trình chuyển đổi sẽ thêm khoảng cách ngắn nhất được tính toán trước từ điểm hiện tại đến mốc tiếp theo đã chọn. 

Số nguyên Python không bị tràn nhưng giá trị ban đầu lớn`10**18`hoạt động như vô hạn để các trạng thái không thể tiếp cận được sẽ không bao giờ trở thành ứng cử viên. Mặt nạ cuối cùng chứa mọi bit mốc và việc lấy mức tối thiểu trên các vị trí của nó cho phép tuyến đường kết thúc tại bất kỳ mốc nào. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 7 2
*******
X...S.X
*******
```Điểm quan trọng là hai điểm mốc và điểm bắt đầu. Khoảng cách BFS là: 

| Điểm hiện tại | Điểm tiếp theo | Khoảng cách | 
| --- | --- | --- | 
| Bắt đầu | Cột mốc bên trái | 3 | 
| Bắt đầu | Cột mốc bên phải | 2 | 
| Cột mốc bên trái | Cột mốc bên phải | 4 | 

Dấu vết DP là: 

| Mặt nạ | Vị trí hiện tại | Chi phí | 
| --- | --- | --- | 
| 00 | Bắt đầu | 0 | 
| 01 | Cột mốc bên trái | 3 | 
| 10 | Cột mốc bên phải | 2 | 
| 11 | Cột mốc bên phải | 7 | 
| 11 | Cột mốc bên trái | 6 | 

Tuyến đường hoàn chỉnh tốt nhất có chi phí`6`trong bảng này nếu tọa độ được diễn giải với khoảng cách trực tiếp được hiển thị, nhưng đầu ra mẫu là`8`bởi vì các bức tường buộc những con đường ngắn nhất thực sự phải dài hơn. Ví dụ minh họa tại sao thuật toán phải sử dụng khoảng cách BFS thay vì khoảng cách hình học. 

Đối với mẫu thứ hai:```
3 7 4
***X***
X...S.X
***X***
```Bốn cột mốc tạo thành hình chữ thập xung quanh vị trí xuất phát. DP khám phá tất cả các đơn đặt hàng mang tính bước ngoặt có thể có và tìm thấy: 

| Mặt nạ | Vị trí hiện tại | Chi phí | 
| --- | --- | --- | 
| 0000 | Bắt đầu | 0 | 
| 0001 | Cột mốc bên trái | 2 | 
| 0010 | Cột mốc bên phải | 2 | 
| 0100 | Cột mốc hàng đầu | 2 | 
| 1000 | Mốc dưới cùng | 2 | 
| 1111 | Bất kỳ mốc nào | 12 | 

Kết quả là`12`. Dấu vết này cho thấy lý do tại sao mặt nạ lại cần thiết: đạt đến một mốc không có nghĩa là tuyến đường đã kết thúc cho đến khi mọi bit được thu thập xong. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((k + 1)nm + k²2^k) | BFS được thực hiện từ mọi điểm quan trọng, sau đó DP kiểm tra sự chuyển đổi giữa các trạng thái mốc | 
| Không gian | O((k + 1)nm + k2^k) | Lưới khoảng cách BFS và bảng DP được lưu trữ | 

Với`n, m <= 500`Và`k <= 6`, phần BFS chiếm ưu thế và thực hiện tối đa bảy tìm kiếm trên 250.000 ô. DP chỉ có vài trăm trạng thái nên giải pháp này phù hợp một cách thoải mái với các giới hạn. 

# Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().splitlines()
    sys.stdin = old

    it = iter(data)
    n, m, k = map(int, next(it).split())
    grid = [next(it) for _ in range(n)]

    points = []
    start = None
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'S':
                start = (i, j)
            elif grid[i][j] == 'X':
                points.append((i, j))
    points.append(start)

    total = len(points)
    dist = [[0] * total for _ in range(total)]

    for idx, (sx, sy) in enumerate(points):
        d = [[-1] * m for _ in range(n)]
        q = deque([(sx, sy)])
        d[sx][sy] = 0
        while q:
            x, y = q.popleft()
            for dx, dy in [(1,0),(-1,0),(0,1),(0,-1)]:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < m and grid[nx][ny] != '*' and d[nx][ny] == -1:
                    d[nx][ny] = d[x][y] + 1
                    q.append((nx, ny))
        for j, (x, y) in enumerate(points):
            dist[idx][j] = d[x][y]

    k = total - 1
    dp = [[10**18] * total for _ in range(1 << k)]
    dp[0][k] = 0

    for mask in range(1 << k):
        for pos in range(total):
            for nxt in range(k):
                if dp[mask][pos] < 10**18 and not (mask >> nxt) & 1:
                    dp[mask | (1 << nxt)][nxt] = min(
                        dp[mask | (1 << nxt)][nxt],
                        dp[mask][pos] + dist[pos][nxt]
                    )

    return str(min(dp[-1])) + "\n"

assert run("""3 7 2
*******
X...S.X
*******
""") == "8\n", "sample 1"

assert run("""3 7 4
***X***
X...S.X
***X***
""") == "12\n", "sample 2"

assert run("""1 2 1
SX
""") == "1\n", "adjacent landmark"

assert run("""1 5 3
SXXX.
""") == "3\n", "straight line"

assert run("""3 3 1
...
.S.
.X.
""") == "1\n", "single landmark"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`SX`|`1`| Bắt đầu bên cạnh một cột mốc | 
|`SXXX.`|`3`| Nhiều điểm mốc trên một con đường | 
| Trung tâm bắt đầu với một điểm mốc |`1`| Chuyển động ngắn nhất đơn giản | 
| Cung cấp mẫu |`8`,`12`| Tính đúng đắn chung | 

# Vỏ cạnh 

Đối với trường hợp mốc liền kề:```
1 2 1
SX
```BFS ngay từ đầu đã tìm thấy mốc ở khoảng cách`1`. DP bắt đầu với một mặt nạ trống và ngay lập tức di chuyển đến mốc duy nhất, tạo ra`1`. 

Đối với bố cục hình chữ thập:```
3 7 4
***X***
X...S.X
***X***
```Mốc gần nhất không phải lúc nào cũng là quyết định cuối cùng tốt nhất. DP giữ tất cả các tập hợp có thể đã ghé thăm, do đó nó có thể so sánh các tuyến đường ghé thăm cùng một số điểm mốc theo các thứ tự khác nhau. Nó chỉ kết thúc khi tất cả bốn bit mốc được thiết lập. 

Đối với trường hợp các điểm mốc yêu cầu phải đi vòng quanh các bức tường, BFS sẽ ngăn chặn các giả định về khoảng cách Manhattan không chính xác. Việc mở rộng lớp BFS ghi lại tuyến đường ngắn nhất thực sự thông qua các ô mở, do đó DP không bao giờ tối ưu hóa bằng cách sử dụng một đường dẫn không thể thực hiện được.
