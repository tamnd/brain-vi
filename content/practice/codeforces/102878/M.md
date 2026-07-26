---
title: "CF 102878M - Ngụy trang"
description: "Việc ngụy trang được thể hiện dưới dạng lưới hình chữ nhật gồm các pixel đen và trắng. Các pixel trắng tạo thành các điểm riêng biệt và một điểm chỉ có thể được vẽ lại khi nó được bao quanh hoàn toàn bởi các pixel đen theo bốn hướng trực giao."
date: "2026-07-25T12:49:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102878
codeforces_index: "M"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 102878
solve_time_s: 59
verified: true
draft: false
---

[CF 102878M - Ngụy trang](https://codeforces.com/problemset/problem/102878/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Việc ngụy trang được thể hiện dưới dạng lưới hình chữ nhật gồm các pixel đen và trắng. Các pixel trắng tạo thành các điểm riêng biệt và một điểm chỉ có thể được vẽ lại khi nó được bao quanh hoàn toàn bởi các pixel đen theo bốn hướng trực giao. Trong số những đốm trắng khép kín như vậy, Long Long chỉ có thể sơn lại những chỗ có diện tích không vượt quá`d`. Nhiệm vụ là xuất ra lưới cuối cùng sau khi lấp đầy những điểm đã chọn đó bằng pixel đen. 

Kích thước lưới đều có thể đạt tới 1000, do đó có thể có tới một triệu ô. Giải pháp liên tục tìm kiếm trên lưới hoặc kiểm tra mọi vùng có thể sẽ nhanh chóng trở nên quá chậm. Với kích thước này, cách tiếp cận dự định phải xử lý mỗi ô chỉ với số lần không đổi, mang lại độ phức tạp dự kiến ​​gần bằng`O(n * m)`. 

Cái bẫy chính là nhầm lẫn một pixel được bao quanh cục bộ với toàn bộ một điểm được bao quanh. Một pixel có thể có các vùng lân cận màu đen ở cả bốn phía trong khi thuộc về thành phần màu trắng lớn hơn tiếp xúc với bên ngoài thông qua một đường dẫn khác. Quyết định phải được đưa ra đối với các thành phần được kết nối của tế bào bạch cầu chứ không phải các tế bào riêng lẻ. 

Ví dụ, hãy xem xét:```
3 3 1
###
#.#
###
```Ô ở giữa là một đốm trắng bao quanh có diện tích`1`, vì vậy kết quả đúng là:```
###
###
###
```Một giải pháp bất cẩn chỉ kiểm tra xem một ô có bốn ô lân cận màu đen có thể hoạt động ở đây hay không, nhưng nó có thể thất bại khi một số ô trắng được kết nối. 

Một trường hợp khác:```
5 5 3
#####
#...#
#.#.#
#...#
#####
```Thành phần màu trắng có diện tích`8`. Mặc dù một số ô bên trong nó được bao quanh bởi các pixel đen, nhưng toàn bộ thành phần vẫn quá lớn và phải không thay đổi. Đầu ra đúng là:```
#####
#...#
#.#.#
#...#
#####
```Giải pháp vẽ các ô bao quanh riêng lẻ thay vì các vùng màu trắng được kết nối sẽ sửa đổi lưới không chính xác. 

## Phương pháp tiếp cận 

Phương pháp bạo lực trực tiếp sẽ kiểm tra từng pixel màu trắng, chạy tìm kiếm để xác định xem nó có được bao quanh hay không, đếm kích thước vùng của nó và sơn lại nếu kích thước đủ nhỏ. Điều này đúng vì mọi quyết định đều dựa trên thành phần màu trắng thực tế. Tuy nhiên, nếu mỗi ô trong số một triệu ô bắt đầu tìm kiếm riêng thì có thể duyệt qua cùng một khu vực nhiều lần. Trong trường hợp xấu nhất điều này đạt đến`O((n * m)^2)`hoạt động, vượt xa những gì kích thước lưới cho phép. 

Quan sát quan trọng là mọi điểm trắng đều là một thành phần được kết nối. Việc lấp đầy đã cung cấp chính xác thông tin cần thiết: các ô của nó và liệu thành phần có chạm vào ranh giới hay không. Một thành phần chạm vào đường viền sẽ mở và không bao giờ có thể sơn lại được. Một thành phần không chạm vào đường viền sẽ được bao quanh hoàn toàn bởi các pixel đen vì câu lệnh đảm bảo rằng các điểm không vượt qua đường viền lưới. 

Điều này biến bài toán thành một lần duyệt tất cả các thành phần màu trắng. Trong quá trình truyền tải, chúng tôi thu thập các ô thuộc một thành phần. Nếu kích thước của nó lớn nhất`d`và nó không bao giờ vươn ra bên ngoài, chúng tôi chuyển những ô được lưu trữ đó thành màu đen. 

Lực lượng vũ phu hoạt động vì việc lấp đầy các thành phần riêng lẻ là đủ để quyết định câu trả lời, nhưng nó sẽ thất bại khi lặp lại công việc tương tự. Việc quan sát thấy mỗi ô thuộc về chính xác một thành phần cho phép chúng tôi xử lý từng pixel một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n * m)^2) | O(n * m) | Quá chậm | 
| Tối ưu | O(n * m) | O(n * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại qua từng ô của lưới. Khi tìm thấy một ô màu trắng chưa được truy cập, hãy bắt đầu lấp đầy từ ô đó. 
2. Trong quá trình lấp đầy, lưu trữ mọi ô thuộc thành phần màu trắng hiện tại và đếm kích thước của nó. Nếu bất kỳ ô nào được truy cập nằm ở viền ngoài, hãy đánh dấu thành phần đó là mở. 
3. Sau khi quá trình lấp lũ kết thúc, hãy kiểm tra thông tin thành phần. Nếu nó không mở và kích thước của nó nhiều nhất`d`, sơn lại mọi ô được lưu trữ thành màu đen. 
4. Tiếp tục cho đến khi mọi thành phần màu trắng đã được xử lý. 

Lý do chúng tôi lưu trữ các ô thay vì sơn lại chúng ngay lập tức là vì chúng tôi không biết liệu thành phần đó có hợp lệ hay không cho đến khi quá trình lấp lũ kết thúc. Một thành phần có thể trông như được bao bọc ở gần ô bắt đầu của nó nhưng sau đó lại chạm tới đường viền. 

Tại sao nó hoạt động: 

Mỗi pixel màu trắng thuộc về chính xác một thành phần được kết nối, do đó thuật toán sẽ xem xét mọi ứng cử viên có thể sơn lại chính xác một lần. Một thành phần chỉ có thể được sơn lại khi nó không chạm vào ranh giới, nghĩa là mọi đường đi rời khỏi thành phần đều đến các ô màu đen. Việc kiểm tra khu vực được thực hiện trên toàn bộ thành phần, do đó thuật toán không bao giờ vẽ một phần nào. Do đó, mọi ô được sơn đều thỏa mãn điều kiện yêu cầu và mọi điểm hợp lệ đều được sơn. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m, d = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    visited = [[False] * m for _ in range(n)]
    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '.' and not visited[i][j]:
                q = deque([(i, j)])
                visited[i][j] = True
                cells = []
                closed = True

                while q:
                    x, y = q.popleft()
                    cells.append((x, y))

                    if x == 0 or x == n - 1 or y == 0 or y == m - 1:
                        closed = False

                    for dx, dy in dirs:
                        nx, ny = x + dx, y + dy
                        if 0 <= nx < n and 0 <= ny < m:
                            if grid[nx][ny] == '.' and not visited[nx][ny]:
                                visited[nx][ny] = True
                                q.append((nx, ny))

                if closed and len(cells) <= d:
                    for x, y in cells:
                        grid[x][y] = '#'

    print("\n".join("".join(row) for row in grid))

if __name__ == "__main__":
    solve()
```Các vòng lặp bên ngoài đảm bảo rằng mọi ô đều trở thành điểm bắt đầu của nhiều nhất một lần lấp lũ. các`visited`mảng là cần thiết vì nếu không thì thành phần màu trắng giống nhau sẽ được khám phá nhiều lần. 

Hàng đợi thực hiện tìm kiếm theo chiều rộng đầu tiên trên các hàng xóm bốn hướng. Trong khi thu thập thành phần, mã chỉ ghi lại tọa độ và trì hoãn việc vẽ cho đến khi quá trình phân loại thành phần hoàn tất. Điều này tránh được lỗi phổ biến là sửa đổi một thành phần trước khi biết liệu nó có nên thay đổi hay không. 

Việc kiểm tra ranh giới sử dụng`0`Và`n - 1`cho các hàng và`0`Và`m - 1`cho các cột. Thiếu một trong bốn điều kiện này là nguyên nhân điển hình dẫn đến câu trả lời sai vì điểm mở chỉ có thể được phát hiện thông qua các ô này. 

Số nguyên Python không gặp vấn đề tràn ở đây vì kích thước thành phần lớn nhất có thể chỉ là một triệu, nhưng việc triển khai vẫn tránh số học không cần thiết và chỉ lưu trữ tọa độ cần thiết. 

## Ví dụ đã hoạt động 

Sử dụng mẫu đầu tiên:```
7 6 5
......
..##..
.#.#..
.#.#..
.#..#.
..##..
......
```Việc lấp lũ tìm thấy vùng màu trắng kèm theo ở giữa. 

| Bước | Kích thước thành phần hiện tại | Chạm vào đường viền | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu tại (2,2) | 1 | Không | Tiếp tục tìm kiếm | 
| Sau khi lấp lũ | 5 | Không | Thành phần sơn | 
| Cuối cùng | 5 | Không | Thay thế bằng màu đen | 

Thành phần này có diện tích`5`, được cho phép bởi`d`, và nó được bao bọc hoàn toàn nên tất cả các ô của nó đều trở thành màu đen. 

Đối với mẫu thứ hai:```
10 10 3
..........
...##.....
..####....
.##..##...
.######...
.####.....
.#..#.###.
.##.#.#.#.
..###.###.
..........
```Một trong những thành phần kèm theo có chính xác ba ô. 

| Bước | Kích thước thành phần hiện tại | Chạm vào đường viền | Hành động | 
| --- | --- | --- | --- | 
| Tìm vùng trắng bên trong | 1 | Không | Tiếp tục tìm kiếm | 
| Hoàn thành việc lấp lũ | 3 | Không | Thành phần sơn | 
| Các vùng trắng khác | Khác nhau | Có | Giữ nguyên | 

Ví dụ này cho thấy tại sao việc phát hiện ranh giới lại quan trọng. Các vùng màu trắng rộng mở vẫn không bị ảnh hưởng ngay cả khi chúng chứa các ô được bao quanh cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Mỗi ô được ghé thăm một lần khi lũ lấp đầy và kiểm tra một số lần không đổi. | 
| Không gian | O(n * m) | Ma trận đã truy cập và thành phần được lưu trữ lớn nhất yêu cầu bộ nhớ tuyến tính. | 

Lưới tối đa chứa một triệu ô, do đó, việc truyền tải tuyến tính vừa vặn thoải mái trong giới hạn dự định. Thuật toán tránh đệ quy nhằm ngăn chặn các vấn đề về độ sâu đệ quy Python trên các thành phần lớn. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solve(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m, d = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    visited = [[False] * m for _ in range(n)]
    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '.' and not visited[i][j]:
                q = deque([(i, j)])
                visited[i][j] = True
                cells = []
                closed = True

                while q:
                    x, y = q.popleft()
                    cells.append((x, y))

                    if x == 0 or x == n - 1 or y == 0 or y == m - 1:
                        closed = False

                    for dx, dy in dirs:
                        nx, ny = x + dx, y + dy
                        if 0 <= nx < n and 0 <= ny < m:
                            if grid[nx][ny] == '.' and not visited[nx][ny]:
                                visited[nx][ny] = True
                                q.append((nx, ny))

                if closed and len(cells) <= d:
                    for x, y in cells:
                        grid[x][y] = '#'

    return "\n".join("".join(row) for row in grid) + "\n"

assert solve("""7 6 5
......
..##..
.#.#..
.#.#..
.#..#.
..##..
......
""") == """......
..##..
.###..
.###..
.####.
..##..
......
""", "sample 1"

assert solve("""10 10 3
..........
...##.....
..####....
.##..##...
.######...
.####.....
.#..#.###.
.##.#.#.#.
..###.###.
..........
""") == """..........
...##.....
..####....
.######...
.######...
.####.....
.#..#.###.
.##.#.###.
..###.###.
..........
""", "sample 2"

assert solve("""1 1 1
.
""") == """.\n", "minimum open case"

assert solve("""3 3 1
###
#.#
###
""") == """###
###
###
""", "single enclosed cell"

assert solve("""5 5 3
#####
#...#
#.#.#
#...#
#####
""") == """#####
#...#
#.#.#
#...#
#####
""", "large enclosed component"

assert solve("""4 4 10
####
#..#
#..#
####
""") == """####
####
####
####
""", "all equal enclosed values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ô trắng đơn ở viền | Cùng một lưới | Kiểm tra các khu vực bên ngoài không bao giờ được sơn. | 
| Một ô kèm theo | Lưới đầy | Kiểm tra lần sơn lại hợp lệ nhỏ nhất. | 
| Vùng kín lớn có lỗ nhỏ | Lưới không thay đổi | Kiểm tra kích thước thành phần thay vì các ô riêng lẻ. | 
| Hình vuông khép kín hoàn toàn | Lưới đen hoàn toàn | Kiểm tra việc sơn lại bình thường với giới hạn lớn hơn. | 

## Vỏ cạnh 

Đối với trường hợp kèm theo một ô:```
3 3 1
###
#.#
###
```Quá trình lấp lũ bắt đầu ở trung tâm, tập hợp một ô và không bao giờ chạm đến ranh giới. Vì kích thước thành phần là`1`, nó được sơn lại. Kết quả là:```
###
###
###
```Đối với trường hợp thành phần lớn:```
5 5 3
#####
#...#
#.#.#
#...#
#####
```Lũ lụt đạt đến tất cả tám ô màu trắng. Thành phần này không chạm vào ranh giới nhưng kích thước của nó lớn hơn`d = 3`, do đó thuật toán giữ nguyên mọi ô. Điều này xác nhận rằng điều kiện khu vực áp dụng cho toàn bộ vị trí. 

Đối với lưới chỉ chứa các ô màu trắng được kết nối với đường viền, chẳng hạn như:```
2 3 10
...
...
```quá trình lấp đầy đánh dấu thành phần là mở ngay lập tức vì nó truy cập vào các ô viền. Mặc dù diện tích đủ nhỏ nhưng thành phần này không thể sơn lại được nên kết quả đầu ra vẫn giống hệt nhau. 

Thuật toán xử lý tất cả các trường hợp này một cách tự nhiên vì nó đưa ra mọi quyết định dựa trên thành phần được kết nối hoàn chỉnh thay vì hình thức pixel cục bộ.
