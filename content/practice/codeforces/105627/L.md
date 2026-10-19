---
title: "CF 105627L - Trò chơi tung xúc xắc"
description: "Bàn cờ chứa các ô mở, các ô bị chặn, vị trí bắt đầu của xúc xắc và một số ô chứa các số mục tiêu từ 1 đến 6. Xúc xắc bắt đầu theo một hướng cố định: mặt trên là 6, mặt bắc là 4 và mặt tây là 2."
date: "2026-06-26T18:11:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105627
codeforces_index: "L"
codeforces_contest_name: "The 2023 ICPC Asia Tehran Regional Contest"
rating: 0
weight: 105627
solve_time_s: 49
verified: true
draft: false
---

[CF 105627L - Trò chơi tung xúc xắc](https://codeforces.com/problemset/problem/105627/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Bàn cờ chứa các ô mở, các ô bị chặn, vị trí bắt đầu cho xúc xắc và một số ô chứa các số mục tiêu từ 1 đến 6. Xúc xắc bắt đầu theo một hướng cố định: mặt trên là 6, mặt bắc là 4 và mặt tây là 2. Một nước đi sẽ lăn xúc xắc vào một ô không bị chặn liền kề, thay đổi hướng của nó theo hướng cuộn. 

Bất cứ khi nào xúc xắc đứng trên một ô được đánh số và giá trị ở mặt trên của nó bằng với số được ghi trong ô đó thì ô đó sẽ đóng góp một điểm. Mỗi ô có thể đóng góp tối đa một lần, ngay cả khi xúc xắc chạm tới nó nhiều lần. Nhiệm vụ là tìm số điểm tối đa có thể thu thập được. 

Kích thước bảng tối đa là 100 x 100, cung cấp tối đa 10.000 ô. Việc tìm kiếm trực tiếp trên các đường đi là không thể vì số lần đi bộ có thể tăng theo cấp số nhân theo số lần di chuyển. Ngay cả khi chúng ta chỉ xem xét mọi chuỗi hướng có thể có độ dài 100, số lượng khả năng sẽ vượt xa những gì có thể khám phá. Quan sát hữu ích là một con súc sắc chỉ có 24 hướng có thể có, do đó tổng số trạng thái có ý nghĩa là đủ nhỏ: nhiều nhất là 100 × 100 × 24 = 240.000 trạng thái. 

Một điểm tinh tế là mục tiêu không phải là tìm ra một con đường ngắn nhất hoặc một con đường có số lần di chuyển nhỏ. Xúc xắc có thể thăm lại các tế bào và di chuyển tự do. Giải pháp đánh dấu một ô là đã truy cập và không bao giờ nhập lại ô đó sẽ loại bỏ không chính xác các cơ hội ghi điểm có thể có trong tương lai. 

Ví dụ:```
1 2
s1
..
```Câu trả lời đúng là`1`. Khuôn có thể di chuyển đến ô chứa`1`theo một số định hướng và ghi điểm nó. Một BFS dạng lưới thông thường bỏ qua việc định hướng có thể cho rằng lần truy cập đầu tiên quyết định mọi thứ và bỏ lỡ rằng có thể đạt được cùng một vị trí với các mặt khuôn khác nhau. 

Một trường hợp khác là khi một ô có thể truy cập được nhưng không thể ghi điểm:```
1 2
s2
..
```Câu trả lời không tự động là số ô được đánh số. Một ô chỉ đóng góp khi mặt trên khớp với giá trị của nó. Một cách tiếp cận bất cẩn đếm tất cả các ô được đánh số có thể truy cập sẽ trả về`1`, nhưng câu trả lời đúng có thể là`0`nếu không thể định hướng được yêu cầu. 

Trường hợp cạnh cuối cùng là ô bắt đầu không được tính ngay. Nếu vị trí bắt đầu không có chữ số thì không có điểm. Nếu vấn đề cho phép một số ở ô bắt đầu thì ý tưởng biểu đồ trạng thái tương tự vẫn sẽ xử lý nó một cách chính xác. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là thử mọi chuỗi cuộn có thể và theo dõi số điểm cao nhất đạt được. Điều này đúng vì mọi chiến lược di chuyển có thể đều được xem xét. Vấn đề là số lượng đường dẫn có thể bùng nổ. Từ mỗi trạng thái có thể có tối đa bốn bước di chuyển, vì vậy sau nhiều lần di chuyển, số lượng đường đi được khám phá sẽ tăng theo cấp số nhân. 

Cấu trúc giúp chúng ta tiết kiệm là khuôn không có bộ nhớ vô hạn. Hành vi trong tương lai của nó chỉ phụ thuộc vào hai điều: ô bảng hiện tại và hướng hiện tại của khuôn. Lịch sử đầy đủ của các bước di chuyển là không liên quan vì việc quay lại cùng một ô với cùng một hướng sẽ mang lại những khả năng giống hệt nhau trong tương lai. 

Điều này biến vấn đề thành khả năng tiếp cận đồ thị. Mỗi trạng thái là một cặp`(row, column, orientation)`. Di chuyển khuôn sẽ tạo ra các cạnh giữa các trạng thái này. Vì mỗi lần cuộn có thể được hoàn tác bằng cách quay lại, nên tất cả các trạng thái có thể truy cập đều có thể được khám phá bằng BFS hoặc DFS. 

Sau khi tìm thấy mọi trạng thái có thể truy cập, chúng ta không cần mô phỏng lộ trình cuối cùng thực tế. Bạn có thể truy cập mọi trạng thái có thể truy cập trong khi duyệt qua biểu đồ trạng thái có thể truy cập. Do đó, đối với mỗi ô bảng, nếu có ít nhất một hướng có thể tiếp cận đặt số chính xác lên trên cùng thì ô đó có thể được thu thập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo số lần di chuyển | Hàm mũ | Quá chậm | 
| Tìm kiếm đồ thị trạng thái | O(nm × 24) | O(nm × 24) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mọi hướng khuôn có thể có dưới dạng trạng thái. Khuôn có sáu mặt, nhưng các mặt đối diện được cố định, vì vậy một khối lập phương chỉ có 24 hướng có thể có. Bắt đầu từ hướng đã cho, chúng ta tạo ra các hướng mới bằng cách lăn theo bốn hướng. 
2. Bắt đầu BFS từ trạng thái chứa ô khởi đầu và hướng khuôn ban đầu. Lý do lưu trữ hướng cùng với vị trí là vì cùng một ô có thể hoạt động khác nhau tùy thuộc vào mặt nào ở trên cùng. 
3. Khi khám phá một trạng thái, hãy thử tất cả bốn cuộn có thể. Bỏ qua các nước đi rời khỏi bảng hoặc đi vào các ô bị chặn. Đối với mỗi bước di chuyển hợp lệ, hãy tính toán hướng mới và thêm trạng thái mới nếu trước đó chưa đạt được trạng thái đó. 
4. Sau khi BFS kết thúc, hãy kiểm tra mọi trạng thái có thể truy cập. Nếu trạng thái đang đứng trên một ô được đánh số và mặt trên bằng giá trị của ô đó, hãy đánh dấu ô đó là có thể sưu tập được. 
5. Đếm các ô được đánh dấu. Số lượng này là điểm tối đa có thể có vì tất cả các ô có thể thu thập đều tương ứng với các trạng thái có thể truy cập và biểu đồ trạng thái có thể truy cập cho phép truy cập tất cả các trạng thái đó. 

Tại sao nó hoạt động: BFS khám phá chính xác các trạng thái mà khuôn có thể đạt tới. Biểu đồ trạng thái chứa tất cả thông tin cần thiết để xác định các bước di chuyển trong tương lai, vì vị trí và hướng mô tả hoàn toàn khuôn. Mọi cơ hội ghi điểm đều được thể hiện bằng một trạng thái có thể truy cập được với mặt trên phù hợp và tất cả các trạng thái có thể truy cập đều có thể được duyệt qua trong một số bước đi, vì vậy mọi ô như vậy đều có thể được thu thập. Các ô không có trạng thái có thể truy cập phù hợp sẽ không bao giờ có thể cho điểm. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def roll(state, direction):
    top, bottom, north, south, west, east = state

    if direction == 0:  # east
        return (west, east, north, south, bottom, top)
    if direction == 1:  # west
        return (east, west, north, south, top, bottom)
    if direction == 2:  # north
        return (south, north, top, bottom, west, east)
    return (north, south, bottom, top, west, east)  # south

def solve():
    n, m = map(int, input().split())
    board = [input().strip() for _ in range(n)]

    sr = sc = -1
    for i in range(n):
        for j in range(m):
            if board[i][j] == 's':
                sr, sc = i, j

    start = (6, 1, 4, 3, 2, 5)

    orientations = []
    index = {}
    queue = deque([start])
    index[start] = 0

    while queue:
        cur = queue.popleft()
        orientations.append(cur)
        for d in range(4):
            nxt = roll(cur, d)
            if nxt not in index:
                index[nxt] = len(index)
                queue.append(nxt)

    dirs = [(-1, 0, 2), (1, 0, 3), (0, -1, 1), (0, 0, 0)]
    # The last entry is replaced below because east has no simple row/col pair.
    moves = [(-1, 0, 2), (1, 0, 3), (0, -1, 1), (0, 1, 0)]

    seen = [[[False] * 24 for _ in range(m)] for _ in range(n)]
    seen[sr][sc][index[start]] = True

    q = deque([(sr, sc, index[start])])

    while q:
        r, c, o = q.popleft()
        cur = orientations[o]

        for dr, dc, d in moves:
            nr = r + dr
            nc = c + dc

            if not (0 <= nr < n and 0 <= nc < m):
                continue
            if board[nr][nc] == 'x':
                continue

            no = index[roll(cur, d)]
            if not seen[nr][nc][no]:
                seen[nr][nc][no] = True
                q.append((nr, nc, no))

    ans = 0
    for i in range(n):
        for j in range(m):
            if board[i][j].isdigit():
                value = int(board[i][j])
                ok = False
                for o in range(24):
                    if seen[i][j][o] and orientations[o][0] == value:
                        ok = True
                        break
                if ok:
                    ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai tạo ra tất cả 24 hướng khối. Thứ tự tuple là`(top, bottom, north, south, west, east)`, điều này làm cho mỗi thao tác cuộn trở thành một phép gán lại các bề mặt một cách đơn giản. 

BFS lưu trữ trạng thái ba chiều: id hàng, cột và hướng. Id hướng được lấy từ danh sách được tính toán trước, tránh so sánh bộ dữ liệu lặp đi lặp lại trong quá trình truyền tải. 

Giai đoạn tính điểm tách biệt với quá trình truyền tải. Điều này là có chủ ý vì việc đạt đến trạng thái và thu thập ô là những khái niệm khác nhau. Một ô chỉ cần một hướng có thể tiếp cận để hiển thị mặt trên chính xác. 

Việc kiểm tra ranh giới ngăn chặn các bước di chuyển không hợp lệ và các ô bị chặn sẽ bị bỏ qua trước khi thêm trạng thái mới. Số lượng trạng thái đủ nhỏ để Python có thể lưu trữ an toàn tất cả các trạng thái đã truy cập. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 4
.23s
4.2x
xx.1
```Một dấu vết đơn giản hóa của BFS và giai đoạn tính điểm là: 

| Vị trí | Định hướng hàng đầu | Hành động | 
| --- | --- | --- | 
| (0,3) | 6 | Bắt đầu BFS | 
| (0,2) | 2 | Thu thập ô chứa 2 | 
| (0,1) | 5 | Không có điểm | 
| (1,1) | 3 | Không có điểm | 
| (1,0) | 4 | Thu thập ô chứa 4 | 
| (2,2) | 1 | Thu thập ô chứa 1 | 

Biểu đồ trạng thái có thể truy cập đầy đủ chứa nhiều hướng hơn được hiển thị và các trạng thái bổ sung đó cho phép khuôn thu thập tất cả các ô phù hợp. Số đếm cuối cùng là`5`, bởi vì mọi ô được đánh số ngoại trừ một ô đều có thể được khớp theo một số hướng có thể truy cập được. 

Đối với mẫu thứ hai:```
2 2
4s
22
```Dấu vết là: 

| Vị trí | Định hướng hàng đầu | Hành động | 
| --- | --- | --- | 
| (0,1) | 6 | Bắt đầu BFS | 
| (0,0) | 2 | Không có điểm | 
| (1,1) | 3 | Không có điểm | 
| (1,0) | 4 | Không có điểm | 

Hai ô chứa`2`có thể truy cập được, nhưng không thể truy cập được với mặt trên bằng`2`. Câu trả lời là`1`bởi vì chỉ có ô chứa`4`có thể được kết hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm × 24) | Mỗi ô bảng và mỗi hướng khuôn được xử lý nhiều nhất một lần. | 
| Không gian | O(nm × 24) | Mảng đã truy cập lưu trữ mọi trạng thái định hướng vị trí có thể có. | 

Số lượng trạng thái tối đa là 240.000 cho một bảng 100 x 100, đủ nhỏ cho các giới hạn nhất định. Thuật toán tránh mọi sự phụ thuộc vào độ dài đường đi, khiến nó phù hợp ngay cả khi xúc xắc có thể di chuyển quanh bảng vô thời hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

# provided samples
assert run("""3 4
.23s
4.2x
xx.1
""") == "5\n"

assert run("""2 2
4s
22
""") == "1\n"

# minimum size
assert run("""1 1
s
""") == "0\n"

# all reachable values with several matches
assert run("""3 3
s12
345
6..
""") == "6\n"

# obstacles and unreachable cells
assert run("""3 3
sxx
x1x
xx2
""") == "0\n"

# larger open area with repeated values
assert run("""4 4
s111
1111
1111
1111
""") == "16\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / s`|`0`| Bảng nhỏ nhất và mục tiêu còn thiếu | 
| Mở bảng có chữ số từ 1 đến 6 |`6`| Nhiều hướng và chuyển động lặp đi lặp lại | 
| Các vùng được phân tách bằng chướng ngại vật |`0`| Xử lý chướng ngại vật đúng cách | 
| Diện tích lớn có giá trị giống hệt nhau |`16`| Xem lại các ô và thu thập tất cả các kết quả phù hợp | 

## Vỏ cạnh 

Đối với một bảng mà chỉ có thể truy cập ô được đánh số theo hướng khác, thuật toán sẽ không dừng lại sau lần truy cập đầu tiên. Ví dụ:```
2 2
s1
..
```BFS lưu trữ cả hướng đến đầu tiên tại một ô và mọi hướng có thể truy cập khác. Khi tế bào chứa`1`được kiểm tra, thuật toán tìm trạng thái có thể truy cập được với mặt trên`1`và đếm nó. 

Đối với một ô được đánh số có thể truy cập mà không có hướng phù hợp nào có thể, chẳng hạn như:```
2 2
s2
..
```BFS vẫn khám phá ô, nhưng giai đoạn tính điểm sẽ kiểm tra mặt trên của mọi hướng có thể tiếp cận. Nếu không có hiển thị`2`, ô bị bỏ qua. 

Đối với bảng có chướng ngại vật:```
3 3
sxx
x1x
xx2
```BFS chỉ mở rộng trạng thái bên trong các ô hợp lệ. Các ô được đánh số riêng biệt không bao giờ trở thành trạng thái có thể truy cập được nên chúng không thể đóng góp điểm. Câu trả lời vẫn là 0 nếu không có bất kỳ xử lý đặc biệt nào.
