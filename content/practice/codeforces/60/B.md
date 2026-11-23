---
title: "CF 60B - Thời gian nối tiếp!"
description: "Chúng ta có một lưới ba chiều biểu thị phần bên trong của một cái đĩa. Lưới có k lớp, mỗi lớp có n hàng và m cột. Mọi ô đều trống rỗng . hoặc bị chặn. Nước bắt đầu xâm nhập từ một ô cụ thể ở lớp trên cùng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu"]
categories: ["algorithms"]
codeforces_contest: 60
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 56"
rating: 1400
weight: 60
solve_time_s: 128
verified: false
draft: false
---

[CF 60B - Thời gian nối tiếp!](https://codeforces.com/problemset/problem/60/B) 

**Đánh giá:** 1400 
**Thẻ:** dfs và tương tự, dsu 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới ba chiều biểu thị phần bên trong của một cái đĩa. Lưới có`k`lớp, mỗi lớp có`n`hàng và`m`cột. Mọi ô đều trống`.`hoặc bị chặn`#`. 

Nước bắt đầu xâm nhập từ một ô cụ thể ở lớp trên cùng. Từ đó, nó lan truyền qua các ô trống theo cả sáu hướng 3D có thể có: lên, xuống, trái, phải, tiến và lùi. Nước không thể đi qua các tế bào bị chặn. 

Mỗi phút có đúng một đơn vị khối nước được thêm vào. Vì nước ngay lập tức lan truyền khắp tất cả các không gian có thể tiếp cận được kết nối, nên tổng lượng nước mà tấm có thể chứa chính xác là số ô trống có thể tiếp cận được kết nối với vị trí bắt đầu. 

Nhiệm vụ là tính toán có bao nhiêu ô trống có thể truy cập được. 

Các kích thước tối đa là`10`, do đó toàn bộ lưới chứa tối đa:```
10 × 10 × 10 = 1000
```tế bào. 

Điều đó làm thay đổi hoàn toàn bản chất của vấn đề. Ngay cả các thuật toán có hệ số hằng số khá lớn cũng được chấp nhận ở đây. Việc duyệt đồ thị đầy đủ trên tất cả các ô là không đáng kể trong giới hạn. 

Khó khăn chính không phải là hiệu suất mà là mô hình hóa kết nối một cách chính xác theo ba chiều. 

Một số trường hợp đặc biệt có thể âm thầm phá vỡ việc triển khai không chính xác. 

Một lỗi phổ biến là quên chuyển động theo chiều dọc giữa các lớp. 

Coi như:```
2 1 1

.

.

1 1
```Ô trên cùng kết nối trực tiếp xuống lớp thứ hai. Câu trả lời đúng là`2`. Một giải pháp chỉ khám phá bên trong từng lớp riêng biệt sẽ trả về sai`1`. 

Một sai lầm khác là đếm tất cả các ô trống thay vì chỉ những ô có thể truy cập được. 

Ví dụ:```
1 2 3

.#.
###
...

1 1
```Hàng dưới cùng trống rỗng nhưng hoàn toàn bị cô lập bởi chướng ngại vật. Nước chỉ có thể ở lại ô trên cùng bên trái nên đáp án là`1`. 

Vấn đề tế nhị thứ ba là xử lý chính xác các dòng trống giữa các lớp. Định dạng đầu vào chèn các dòng trống giữa các mô tả lớp. Nếu quá trình phân tích cú pháp bỏ qua điều này một cách bất cẩn, các hàng có thể chuyển sang sai lớp. 

Cuối cùng, ô bắt đầu được đảm bảo trống, nhưng nó có thể đã bị cô lập:```
1 3 3

###
#.#
###

2 2
```Câu trả lời là`1`, vì nước không thể di chuyển đi đâu khác. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề là mô phỏng theo nghĩa đen. 

Chúng ta có thể tưởng tượng nước lan ra từng phút. Mỗi khi thêm một đơn vị nước, chúng tôi sẽ mở rộng sang các ô lân cận nếu có thể. Cuối cùng, mọi ô trống có thể truy cập sẽ được lấp đầy và câu trả lời là tổng số ô như vậy. 

Ý tưởng này đúng vì quy trình vật lý được mô tả trong câu lệnh chính xác là kết nối đồ thị. Mọi ô trống có thể tiếp cận được thông qua các ô trống liền kề cuối cùng đều chứa đầy nước. 

Vấn đề với mô phỏng thời gian rõ ràng là nó làm phức tạp logic một cách không cần thiết. Chúng tôi thực sự không quan tâm đến thứ tự các ô được lấp đầy. Chúng tôi chỉ quan tâm đến năng lực cuối cùng. 

Sự quan sát đó thay đổi hoàn toàn vấn đề. 

Dung lượng tấm bằng kích thước của thành phần được kết nối chứa ô khởi đầu. 

Khi chúng tôi nhận ra điều đó, nhiệm vụ sẽ trở thành một bài toán truyền tải đồ thị tiêu chuẩn. Mỗi ô trống là một nút. Hai ô được kết nối nếu chúng có chung một khuôn mặt. Chúng tôi bắt đầu từ vị trí nhấn và đếm xem có thể truy cập được bao nhiêu ô. 

Cách tiếp cận bạo lực có thể quét liên tục toàn bộ lưới để tìm kiếm các ô mới có thể truy cập cho đến khi không có thay đổi nào xảy ra. Vì lưới có nhiều nhất`1000`các ô, thậm chí điều này có thể sẽ vượt qua, nhưng nó thực hiện công việc lặp đi lặp lại không cần thiết. Trong trường hợp xấu nhất, mỗi lần lặp sẽ quét tất cả các ô và có thể có`1000`lặp đi lặp lại, dẫn đến khoảng`10^6`hoạt động. 

Một cách tiếp cận rõ ràng và tối ưu hơn là DFS hoặc BFS. Mỗi ô được truy cập một lần và mỗi ô lân cận được kiểm tra một lần. 

Bởi vì lưới rất nhỏ nên việc truyền tải đều hoạt động thoải mái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng lũ lụt tàn bạo | O((knm)^2) | O(knm) | Được chấp nhận nhưng không hiệu quả | 
| Truyền tải DFS/BFS | O(knm) | O(knm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước`k`,`n`, Và`m`. 
2. Lưu trữ lưới 3D theo từng lớp. 

Đầu vào chứa các dòng trống giữa các lớp, vì vậy chúng ta bỏ qua các dòng trống trong khi đọc. 
3. Chuyển đổi tọa độ bắt đầu từ chỉ mục dựa trên 1 sang chỉ mục dựa trên 0. 

Danh sách Python sử dụng chỉ mục dựa trên 0, do đó, việc không chuyển đổi sẽ truy cập sai ô. 
4. Bắt đầu DFS hoặc BFS từ vị trí bắt đầu ở lớp trên cùng. 

Chúng ta chỉ di chuyển vào những ô: 

- ở trong lưới, 
- chứa`.`, 
- chưa từng đến thăm trước đây. 
5. Từ mỗi ô được truy cập, hãy khám phá tất cả sáu hướng: 

- lên xuống giữa các lớp, 
- trái và phải trong một hàng, 
- tiến và lùi giữa các hàng. 
6. Đếm từng ô được truy cập. 

Mỗi ô trống được truy cập đại diện cho một khối đơn vị mà cuối cùng có thể chứa nước. 
7. In số đếm cuối cùng. 

### Tại sao nó hoạt động 

Quá trình duyệt khám phá chính xác thành phần được kết nối của ô bắt đầu. 

Mọi ô trống có thể tiếp cận cuối cùng đều phải đổ đầy nước vì nước có thể chảy qua các mặt liền kề vô thời hạn. Mọi ô trống không thể truy cập đều khô ráo vì chướng ngại vật chặn tất cả các đường dẫn có thể có từ nguồn. 

DFS/BFS truy cập mọi ô trống có thể truy cập chính xác một lần và không bao giờ đi vào các ô bị chặn. Vì câu trả lời chính xác là kích thước của thành phần được kết nối này nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    k, n, m = map(int, input().split())

    grid = []

    for _ in range(k):
        layer = []

        while len(layer) < n:
            line = input().strip()

            if line == "":
                continue

            layer.append(line)

        grid.append(layer)

    x, y = map(int, input().split())

    x -= 1
    y -= 1

    visited = [[[False] * m for _ in range(n)] for _ in range(k)]

    directions = [
        (1, 0, 0),
        (-1, 0, 0),
        (0, 1, 0),
        (0, -1, 0),
        (0, 0, 1),
        (0, 0, -1),
    ]

    q = deque()
    q.append((0, x, y))
    visited[0][x][y] = True

    answer = 0

    while q:
        z, r, c = q.popleft()
        answer += 1

        for dz, dr, dc in directions:
            nz = z + dz
            nr = r + dr
            nc = c + dc

            if not (0 <= nz < k):
                continue

            if not (0 <= nr < n):
                continue

            if not (0 <= nc < m):
                continue

            if visited[nz][nr][nc]:
                continue

            if grid[nz][nr][nc] == '#':
                continue

            visited[nz][nr][nc] = True
            q.append((nz, nr, nc))

    print(answer)

solve()
```Lưới được lưu trữ dưới dạng cấu trúc ba chiều được lập chỉ mục bởi:```
grid[layer][row][column]
```Bước phân tích cú pháp hơi phức tạp vì đầu vào sẽ chèn các dòng trống giữa các lớp. Vòng lặp tiếp tục đọc cho đến khi chính xác`n`các hàng hợp lệ được thu thập cho lớp hiện tại. 

Hàng đợi BFS lưu trữ gấp ba lần`(layer, row, column)`. Vì chuyển động xảy ra trong ba chiều nên tất cả các tọa độ phải chuyển động cùng nhau. 

Sáu vectơ chỉ phương tương ứng với sáu mặt khối lập phương. Việc quên hướng dọc sẽ ngắt kết nối các lớp một cách không chính xác. 

các`visited`mảng ngăn chặn việc xem lại các ô vô tận. Không có nó, các chu kỳ trong không gian mở sẽ gây ra sự truyền tải vô hạn. 

Câu trả lời sẽ tăng lên khi một ô được đưa ra khỏi hàng đợi. Vì mỗi ô có thể truy cập vào hàng đợi chính xác một lần nên số đếm cuối cùng bằng kích thước thành phần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 1

.

1 1
```Chỉ có một ô trống và đó là vị trí bắt đầu. 

| Bước | Xếp hàng | Ô hiện tại | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu |`[(0,0,0)]`| Không có | 0 | 
| 1 |`[]`|`(0,0,0)`| 1 | 

Quá trình truyền tải kết thúc ngay lập tức vì không có hàng xóm. 

Ví dụ này xác nhận trường hợp cơ sở trong đó toàn bộ vùng có thể truy cập bao gồm một ô duy nhất. 

### Ví dụ 2 

đầu vào:```
2 2 2

..
#.

..
..

1 1
```Các ô có thể truy cập trải rộng trên nhiều lớp. 

| Bước | Xếp Hàng Trước Nhạc Pop | Ô hiện tại | Đã thêm ô mới | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 |`[(0,0,0)]`|`(0,0,0)`|`(0,0,1), (1,0,0)`| 1 | 
| 2 |`[(0,0,1),(1,0,0)]`|`(0,0,1)`|`(0,1,1), (1,0,1)`| 2 | 
| 3 |`[(1,0,0),(0,1,1),(1,0,1)]`|`(1,0,0)`|`(1,1,0)`| 3 | 
| 4 |`[(0,1,1),(1,0,1),(1,1,0)]`|`(0,1,1)`|`(1,1,1)`| 4 | 
| 5 |`[(1,0,1),(1,1,0),(1,1,1)]`|`(1,0,1)`| Không có | 5 | 
| 6 |`[(1,1,0),(1,1,1)]`|`(1,1,0)`| Không có | 6 | 
| 7 |`[(1,1,1)]`|`(1,1,1)`| Không có | 7 | 

Câu trả lời cuối cùng:`7`. 

Dấu vết này chứng tỏ tại sao chuyển động thẳng đứng lại quan trọng. Một số ô chỉ có thể truy cập được bằng cách di chuyển giữa các lớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(knm) | Mỗi ô được truy cập tối đa một lần | 
| Không gian | O(knm) | Hàng đợi và mảng đã truy cập lưu trữ tối đa tất cả các ô | 

Kích thước lưới tối đa chỉ`1000`các ô, do đó quá trình truyền tải kết thúc gần như ngay lập tức. Cả việc sử dụng bộ nhớ và thời gian chạy đều nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        k, n, m = map(int, input().split())

        grid = []

        for _ in range(k):
            layer = []

            while len(layer) < n:
                line = input().strip()

                if line == "":
                    continue

                layer.append(line)

            grid.append(layer)

        x, y = map(int, input().split())

        x -= 1
        y -= 1

        visited = [[[False] * m for _ in range(n)] for _ in range(k)]

        directions = [
            (1, 0, 0),
            (-1, 0, 0),
            (0, 1, 0),
            (0, -1, 0),
            (0, 0, 1),
            (0, 0, -1),
        ]

        q = deque([(0, x, y)])
        visited[0][x][y] = True

        ans = 0

        while q:
            z, r, c = q.popleft()
            ans += 1

            for dz, dr, dc in directions:
                nz = z + dz
                nr = r + dr
                nc = c + dc

                if not (0 <= nz < k):
                    continue

                if not (0 <= nr < n):
                    continue

                if not (0 <= nc < m):
                    continue

                if visited[nz][nr][nc]:
                    continue

                if grid[nz][nr][nc] == '#':
                    continue

                visited[nz][nr][nc] = True
                q.append((nz, nr, nc))

        return str(ans)

    return solve()

# provided sample
assert run(
"""1 1 1

.

1 1
"""
) == "1", "sample 1"

# isolated start
assert run(
"""1 3 3

###
#.#
###

2 2
"""
) == "1", "isolated cell"

# vertical connectivity
assert run(
"""2 1 1

.

.

1 1
"""
) == "2", "movement between layers"

# disconnected empty region
assert run(
"""1 2 3

.#.
###
...

1 1
"""
) == "1", "unreachable empty cells"

# fully open cube
assert run(
"""2 2 2

..
..

..
..

1 1
"""
) == "8", "all cells reachable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ô mở đơn | 1 | Lưới kích thước tối thiểu | 
| Tế bào trung tâm biệt lập | 1 | Chướng ngại vật cản trở mọi chuyển động | 
| Hai ô dọc | 2 | Truyền tải lớp chính xác | 
| Khu vực trống bị ngắt kết nối | 1 | Chỉ tính các ô có thể truy cập | 
| Khối lập phương 2×2×2 mở hoàn toàn | 8 | Kết nối đầy đủ ở chế độ 3D | 

## Vỏ cạnh 

Hãy xem xét trường hợp cần phải di chuyển giữa các lớp:```
2 1 1

.

.

1 1
```BFS bắt đầu lúc`(0,0,0)`. Một trong sáu hướng di chuyển xuống lớp`1`, đạt`(1,0,0)`. 

Quá trình duyệt truy cập chính xác vào hai ô, vì vậy kết quả đầu ra là:```
2
```Một giải pháp chỉ khám phá bên trong mỗi lớp sẽ bỏ lỡ quá trình chuyển đổi này. 

Bây giờ hãy xem xét các vùng trống bị ngắt kết nối:```
1 2 3

.#.
###
...

1 1
```Ô bắt đầu là góc trên bên trái. Tất cả các vị trí lân cận đều bị chặn hoặc nằm ngoài lưới. 

Hàng dưới cùng chứa các ô trống nhưng không có đường dẫn đến chúng vì hàng giữa bị chặn hoàn toàn. 

BFS chỉ truy cập một ô và xuất ra:```
1
```Điều này xác nhận rằng thuật toán tính dung lượng có thể truy cập chứ không phải tổng dung lượng trống. 

Cuối cùng, hãy xem xét vị trí bắt đầu được bao bọc hoàn toàn:```
1 3 3

###
#.#
###

2 2
```Hàng đợi ban đầu chỉ chứa ô trung tâm. Mọi hướng đều chạm vào tường ngay lập tức. 

Quá trình truyền tải kết thúc sau một lần lặp và trả về:```
1
```Điều này xác nhận rằng thuật toán xử lý chính xác các thành phần được kết nối riêng biệt.
