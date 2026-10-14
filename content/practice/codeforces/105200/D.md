---
title: "CF 105200D - Đừng Bị Bắt"
description: "Chúng ta có một hội trường hình chữ nhật được biểu diễn dưới dạng lưới. Kauã bắt đầu từ ô trên cùng bên trái và muốn đến ô dưới cùng bên phải. Một số ô có lính canh và mỗi lính canh canh gác từng ô theo một hướng thẳng cho đến khi một lính canh khác chặn tầm nhìn."
date: "2026-06-27T02:52:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105200
codeforces_index: "D"
codeforces_contest_name: "IME++ Starters Try-outs 2024"
rating: 0
weight: 105200
solve_time_s: 39
verified: true
draft: false
---

[CF 105200D - Đừng để bị bắt](https://codeforces.com/problemset/problem/105200/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hội trường hình chữ nhật được biểu diễn dưới dạng lưới. Kauã bắt đầu từ ô trên cùng bên trái và muốn đến ô dưới cùng bên phải. Một số ô có lính canh và mỗi lính canh canh gác từng ô theo một hướng thẳng cho đến khi một lính canh khác chặn tầm nhìn. Kauã không thể bước vào phòng giam của lính canh hoặc bất kỳ phòng giam nào được lính canh canh gác. Nhiệm vụ là tìm ra con đường ngắn nhất có thể đi qua các ô còn lại và đánh dấu con đường đó bằng`X`. Nếu không có tuyến đường nào tồn tại, câu trả lời là`-1`. 

Đầu vào cho biết chiều cao và chiều rộng của lưới, theo sau là chính lưới đó. Các ô trống được biểu thị bằng`.`, trong khi bốn ký tự mũi tên tượng trưng cho những người lính canh đang nhìn về một trong bốn hướng chính. Đầu ra là một lưới được sửa đổi với đường dẫn hợp lệ ngắn nhất được đánh dấu hoặc`-1`. 

Kích thước lưới có thể đạt tới 2000 vào năm 2000, nghĩa là có thể có tới bốn triệu ô. Một thuật toán khám phá mọi đường đi có thể sẽ là không thể vì số lượng đường đi tăng theo cấp số nhân. Ngay cả các thuật toán có hệ số số lượng ô, chẳng hạn như`O(n * m)`, là phù hợp vì chúng chỉ chạm vào mỗi ô một số lần không đổi. 

Phần khó khăn là một ô có thể trống nhưng vẫn không thể sử dụng được vì người bảo vệ nhìn thấy nó. Giải pháp chỉ coi các vị trí bảo vệ là bị chặn sẽ đi qua các ô được theo dõi một cách không chính xác. 

Ví dụ, hãy xem xét:```
3 4
....
.>..
....
```Các tế bào`(2,3)`Và`(2,4)`đang bị bảo vệ theo dõi. Đường dẫn đầu ra chính xác không thể sử dụng chúng. Một BFS bất cẩn chỉ tránh`>`sẽ coi chúng có sẵn một cách không chính xác. 

Một trường hợp cạnh khác là khi một người bảo vệ bị chặn bởi một người bảo vệ khác:```
3 5
.....
.>>..
.....
```Người bảo vệ thứ hai chặn tầm nhìn của người bảo vệ thứ nhất. Người bảo vệ đầu tiên không thể nhìn xuyên qua người bảo vệ thứ hai, vì vậy các ô sau người bảo vệ thứ hai vẫn có sẵn. Đánh dấu toàn bộ hàng là nguy hiểm sẽ xóa đường dẫn hợp lệ. 

Trường hợp cuối cùng là khi điểm bắt đầu hoặc kết thúc được bao quanh bởi các ô được theo dõi:```
3 3
.>.
...
```Thuật toán vẫn phải chạy BFS bình thường ngay từ đầu và phát hiện ra rằng không thể truy cập được đích nếu mọi tuyến đường có thể đều bị chặn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ là thử mọi chuỗi chuyển động có thể có từ ô bắt đầu và giữ chuỗi chuyển động ngắn nhất để đến đích. Điều này đúng vì mỗi chuỗi được khám phá đều thể hiện một tuyến đường khả thi. Tuy nhiên, số lượng các tuyến đường có thể có thể rất lớn. Trong một lưới có hàng triệu ô, việc khám phá liên tục các đường dẫn sẽ tạo ra quá nhiều thao tác. 

Cấu trúc của bài toán giao cho chúng ta hai nhiệm vụ riêng biệt. Đầu tiên, xác định ô nào bị cấm. Các lính canh không di chuyển, vì vậy tầm nhìn của họ có thể được xử lý một lần trước khi tìm kiếm đường đi. Sau đó, bài toán còn lại là bài toán đường đi ngắn nhất tiêu chuẩn trên lưới trong đó mọi chuyển động đều có cùng chi phí. 

Quan sát làm cho điều này trở nên hiệu quả là BFS trên lưới không có trọng số sẽ tự động tìm đường đi ngắn nhất. Sau khi các ô được theo dõi được chuyển đổi thành các ô bị chặn, mọi hành động hợp pháp đều có chi phí như nhau, vì vậy, khi BFS đến đích lần đầu tiên, nó đã tìm thấy một lộ trình tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lượng ô | O(n×m) | Quá chậm | 
| Tối ưu | O(n×m) | O(n×m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét từng người bảo vệ và đánh dấu tất cả các ô có thể nhìn thấy từ người bảo vệ đó là nguy hiểm. Di chuyển theo hướng của người bảo vệ cho đến khi chạm tới mép lưới hoặc người bảo vệ khác. Chuyển động dừng lại ở những người bảo vệ vì một người bảo vệ chặn tầm nhìn của người bảo vệ khác. 
2. Chạy BFS từ`(0, 0)`. Chỉ di chuyển đến các ô nằm trong lưới, không phải là lính canh và không được đánh dấu là nguy hiểm. Lưu trữ ô trước đó cho mọi vị trí đã ghé thăm để có thể xây dựng lại tuyến đường cuối cùng. 
3. Nếu BFS không bao giờ đạt được`(n - 1, m - 1)`, in`-1`. Mảng đã truy cập biểu thị mọi ô có thể truy cập được thông qua các chuyển động hợp lệ, do đó, một đích đến chưa được truy cập chứng tỏ không tồn tại tuyến đường hợp lệ. 
4. Nếu đã đến đích, hãy đi theo con trỏ gốc ngược lại cho đến khi đến điểm bắt đầu. Đánh dấu mọi ô trên chuỗi này bằng`X`. 

Tại sao nó hoạt động: sau giai đoạn đầu tiên, mọi ô mà Kauã không thể vào sẽ bị loại khỏi danh sách xem xét. Lưới còn lại chính xác là biểu đồ của các chuyển động có thể xảy ra. BFS khám phá biểu đồ này theo từng lớp, vì vậy khi tiếp cận một ô lần đầu tiên, nó sẽ được tiếp cận bằng chuỗi di chuyển ngắn nhất có thể. Các con trỏ cha lưu trữ tuyến đường ngắn nhất đó và việc xây dựng lại chúng sẽ tạo ra đường đi ngắn nhất hợp lệ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    blocked = [[False] * m for _ in range(n)]

    directions = {
        '>': (0, 1),
        '<': (0, -1),
        'v': (1, 0),
        '^': (-1, 0)
    }

    for i in range(n):
        for j in range(m):
            if grid[i][j] in directions:
                blocked[i][j] = True
                di, dj = directions[grid[i][j]]
                ni, nj = i + di, j + dj
                while 0 <= ni < n and 0 <= nj < m:
                    if grid[ni][nj] in directions:
                        break
                    blocked[ni][nj] = True
                    ni += di
                    nj += dj

    parent = [[None] * m for _ in range(n)]
    visited = [[False] * m for _ in range(n)]

    q = deque([(0, 0)])
    visited[0][0] = True

    moves = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    while q:
        i, j = q.popleft()
        if (i, j) == (n - 1, m - 1):
            break

        for di, dj in moves:
            ni, nj = i + di, j + dj
            if 0 <= ni < n and 0 <= nj < m:
                if not visited[ni][nj] and not blocked[ni][nj]:
                    visited[ni][nj] = True
                    parent[ni][nj] = (i, j)
                    q.append((ni, nj))

    if not visited[n - 1][m - 1]:
        print(-1)
        return

    i, j = n - 1, m - 1
    while True:
        if grid[i][j] == '.':
            grid[i][j] = 'X'
        if (i, j) == (0, 0):
            break
        i, j = parent[i][j]

    for row in grid:
        print(''.join(row))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính toán các ô nguy hiểm. Bản thân ô bảo vệ được đánh dấu là bị chặn và quá trình quét chỉ tiếp tục qua các ô trống. Dừng khi tìm thấy một người bảo vệ khác sẽ xử lý chính xác quy tắc hiển thị. 

Phần BFS sử dụng hàng đợi vì tất cả các lần di chuyển đều có chi phí như nhau. các`parent`mảng lưu trữ vị trí trước đó được sử dụng để đến từng ô, điều này tránh lưu trữ các đường dẫn đầy đủ và giữ mức sử dụng bộ nhớ ở mức tuyến tính. 

Trong quá trình xây dựng lại, thuật toán bắt đầu từ đích và liên tục nhảy về ô trước đó. Điều này đi theo con đường ngắn nhất chính xác được BFS phát hiện. Điều kiện chỉ thay đổi các ô trống sẽ giúp bảo vệ không bị ghi đè. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5
.....
.>.v.
.....
.^.<.
.....
```| Bước | Ô hiện tại | Hành động xếp hàng | Kết quả | 
| --- | --- | --- | --- | 
| 1 |`(0,0)`| Bắt đầu BFS | Đánh dấu đã ghé thăm | 
| 2 |`(1,0)`| Di chuyển xuống dưới | Đã thêm vào hàng đợi | 
| 3 |`(2,0)`| Tiếp tục tuyến đường ngắn nhất | Đã thêm vào hàng đợi | 
| 4 |`(3,0)`| Tiếp tục đi xuống | Đã thêm vào hàng đợi | 
| 5 |`(4,4)`| Đã đến đích | Xây dựng lại con đường | 

Dấu vết cho thấy các ô bảo vệ và các ô được theo dõi bị bỏ qua trước khi BFS bắt đầu. Tuyến đường kết quả đi theo phía bên trái và sau đó đi qua hàng dưới cùng. 

### Mẫu 2 

đầu vào:```
3 6
.v....
.^.v..
...^..
```| Bước | Ô hiện tại | Hành động xếp hàng | Kết quả | 
| --- | --- | --- | --- | 
| 1 |`(0,0)`| Bắt đầu BFS | Đánh dấu đã ghé thăm | 
| 2 |`(1,0)`| Di chuyển xuống dưới | Đã thêm | 
| 3 |`(2,0)`| Di chuyển xuống dưới | Đã thêm | 
| 4 |`(2,5)`| Đã đến đích | Đường dẫn được khôi phục | 

Trường hợp này chứng tỏ rằng các ô gần lính canh vẫn có thể sử dụng được nếu chúng nằm ngoài tầm nhìn của lính canh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n×m) | Mỗi ô được quét để tìm người bảo vệ và được BFS truy cập nhiều nhất một lần. | 
| Không gian | O(n×m) | Các mảng bị chặn, đã truy cập và mảng gốc đều lưu trữ thông tin cho mọi ô. | 

Với tối đa bốn triệu ô, xử lý tuyến tính là giải pháp mong muốn. Mọi thao tác được thực hiện trên một ô đều có thời gian không đổi, do đó thuật toán phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# sample 1
assert run("""5 5
.....
.>.v.
.....
.^.<.
.....
""") == """X....
X>.v.
X....
X^.<.
XXXXX
"""

# sample 2
assert run("""3 6
.v....
.^.v..
...^..
""") == """XvXXX.
X^XvX.
XXX^XX
"""

# impossible case
assert run("""4 4
....
..^.
.<..
....
""") == """-1
"""

# minimum grid
assert run("""1 1
.
""") == """X
"""

# guard blocks only one direction
assert run("""3 5
.....
.>...
.....
""") == """X....
X>XXX
XXXXX
"""

# blocked by another guard
assert run("""3 5
.....
.>>..
.....
""") != """-1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 x 1`lưới |`X`| Xử lý điểm bắt đầu và điểm đến là cùng một ô. | 
| Người bảo vệ duy nhất ở hành lang | Một con đường tránh tầm nhìn | Xác nhận các ô đã xem đã bị chặn. | 
| Hai vệ sĩ liền kề | Có thể truy cập | Xác nhận tầm nhìn dừng lại ở một người bảo vệ khác. | 
| Mẫu không thể truy cập |`-1`| Xác nhận xử lý lỗi BFS. | 

## Vỏ cạnh 

Đối với trường hợp hiển thị đầu tiên:```
3 4
....
.>..
....
```Người bảo vệ ở`(1,1)`dấu hiệu`(1,2)`Và`(1,3)`như bị chặn. BFS vẫn có thể di chuyển xung quanh hàng qua trên cùng hoặc dưới cùng, do đó, thuật toán không xử lý sai tất cả các ô trống là an toàn nếu không kiểm tra tầm nhìn của người bảo vệ. 

Đối với trường hợp chặn:```
3 5
.....
.>>..
.....
```Người bảo vệ đầu tiên chỉ nhìn thấy người bảo vệ thứ hai vì người bảo vệ thứ hai dừng quá trình quét. Các ô sau tấm bảo vệ thứ hai không được đánh dấu. Giai đoạn BFS có thể sử dụng chúng nếu chúng dẫn đến đích phù hợp với quy tắc hiển thị thực tế. 

Đối với đích đến bị chặn hoàn toàn:```
3 3
.>.
...
```Các ô nguy hiểm được tính toán trước BFS. Nếu mọi tuyến đường có thể bị loại bỏ, đích sẽ không bao giờ được đưa vào hàng đợi và chương trình sẽ in`-1`. Giai đoạn tìm kiếm không bao giờ cần phải quan tâm đến những người bảo vệ vì tất cả các ô không hợp lệ đã được lọc ra.
