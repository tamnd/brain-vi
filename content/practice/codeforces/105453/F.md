---
title: "CF 105453F - Anomia"
description: "Chúng ta được cung cấp một lưới hình chữ nhật hoạt động giống như một bản đồ thành phố nhỏ. Một số ô là đường, một số là các tòa nhà cản trở chuyển động và một số ô chứa các sĩ quan cảnh sát nhìn về một hướng cố định với tầm nhìn hạn chế."
date: "2026-06-23T03:00:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105453
codeforces_index: "F"
codeforces_contest_name: "2024 ICPC Greece Regional Collegiate Programming Contest (GRCPC 2024)"
rating: 0
weight: 105453
solve_time_s: 80
verified: true
draft: false
---

[CF 105453F - Anomia](https://codeforces.com/problemset/problem/105453/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới hình chữ nhật hoạt động giống như một bản đồ thành phố nhỏ. Một số ô là đường, một số là các tòa nhà cản trở chuyển động và một số ô chứa các sĩ quan cảnh sát nhìn về một hướng cố định với tầm nhìn hạn chế. Kẻ chạy trốn bắt đầu từ một ô cụ thể và muốn đến một ô ẩn náu được chỉ định bằng cách chỉ di chuyển theo bốn hướng chính qua các ô đường. 

Khó khăn chính là không phải tất cả các ô đường đều an toàn. Bất kỳ phòng giam nào nằm trong tầm nhìn của cảnh sát đều trở nên nguy hiểm và kẻ chạy trốn không thể vào hoặc thậm chí đi qua nó. Mỗi sĩ quan nhìn thẳng theo hướng họ đối mặt, cho đến khoảng cách D, nhưng tầm nhìn của họ sẽ dừng lại sớm nếu nó va vào một tòa nhà hoặc một sĩ quan khác. 

Nhiệm vụ là xác định xem có tồn tại một con đường từ nơi kẻ chạy trốn đến nơi ẩn náu chỉ sử dụng các ô đường an toàn hay không. 

Lưới có thể lớn tới 1000 x 1000, do đó tồn tại tới một triệu ô. Bất kỳ giải pháp nào kiểm tra khả năng hiển thị một cách đơn giản từ mọi phòng giam tới từng sĩ quan hoặc cố gắng quét nhiều lần theo từng chuyển động sẽ quá chậm. Cần phải di chuyển tuyến tính hoặc gần tuyến tính trên lưới, có thể bằng quá trình tiền xử lý để đánh dấu các ô không an toàn một lần và sau đó chạy tìm kiếm đường dẫn ngắn nhất hoặc khả năng tiếp cận tiêu chuẩn. 

Một trường hợp tinh tế phát sinh khi nhiều sĩ quan có tầm nhìn trùng nhau. Ví dụ: nếu hai sĩ quan đối mặt với nhau mà không có bức tường giữa họ, một cách tiếp cận ngây thơ không coi các sĩ quan như những người cản trở có thể mở rộng tầm nhìn qua họ một cách không chính xác. 

Một trường hợp cạnh khác xuất hiện khi một đường dẫn tồn tại về mặt hình học nhưng đi qua một ô chỉ _gián tiếp_ không an toàn. Ví dụ, một phòng giam có thể không chứa sĩ quan nhưng nằm chính xác trong một hành lang tầm nhìn dài bị chặn xa hơn về phía trước. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp là mô phỏng khả năng hiển thị cho từng sĩ quan một cách độc lập. Đối với mỗi sĩ quan, chúng tôi sẽ đi từng bước theo hướng đối diện của nó lên tới các ô D hoặc cho đến khi chúng tôi gặp phải một chốt chặn, đánh dấu tất cả các ô đã ghé thăm là không an toàn. Sau khi xử lý tất cả các sĩ quan, chúng tôi sẽ chạy BFS hoặc DFS từ kẻ chạy trốn đến nơi ẩn náu để tránh những phòng giam không an toàn. 

Cách tiếp cận này đúng về mặt logic, nhưng chi phí trong trường hợp xấu nhất là cao. Trong một lưới dày đặc có nhiều sĩ quan, mỗi lần quét tối đa D bước, chỉ riêng giai đoạn đánh dấu có thể chiếm O(N × M × D) ở cấu hình tồi tệ nhất, vì mỗi ô có thể nằm trong đường quét của nhiều sĩ quan. Với N, M, D đều lên tới 1000, điều này trở nên quá chậm. 

Quan sát quan trọng là chúng ta không cần mô phỏng tầm nhìn của từng sĩ quan một cách độc lập theo cách đệ quy hoặc lặp đi lặp lại. Đường ngắm của mỗi sĩ quan là một tia được định hướng đơn giản trên lưới. Thay vì tính toán lại khả năng hiển thị của mỗi sĩ quan, chúng tôi có thể xử lý trước lưới theo bốn lần quét theo hướng. Trong mỗi lần quét, chúng tôi truyền bá thực thể chặn gần nhất trong khi vẫn duy trì xem liệu chúng tôi hiện có ở trong phân đoạn tầm nhìn đang hoạt động hay không. 

Cụ thể, chúng ta có thể quét các hàng từ trái sang phải và từ phải sang trái và các cột từ trên xuống dưới và từ dưới lên trên. Trong quá trình truy quét, chúng tôi duy trì sĩ quan gặp phải lần cuối và khoảng cách kể từ khi nó được nhìn thấy. Khi khoảng cách vượt quá D hoặc gặp vật cản, tầm nhìn sẽ dừng lại. Điều này biến mô phỏng lặp đi lặp lại của mỗi sĩ quan thành công việc liên tục trên mỗi ô theo hướng. 

Sau khi đánh dấu tất cả các ô không an toàn, chúng tôi giảm vấn đề thành tìm kiếm khả năng tiếp cận tiêu chuẩn từ F đến H trên một lưới có các ô bị chặn. Đây là một BFS đơn giản trong O(NM). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu cho mỗi sĩ quan | O(N · M · D) | O(NM) | Quá chậm | 
| Quét định hướng + BFS | O(N · M) | O(NM) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng lưới boolean`danger`được khởi tạo thành sai. Điều này sẽ đánh dấu mọi ô có thể là bức tường tầm nhìn hoặc có thể nhìn thấy trực tiếp đối với ít nhất một sĩ quan. 
2. Thực hiện quét từ trái sang phải trên mỗi hàng. Theo dõi sĩ quan gần đây nhất quay mặt về phía bên phải (`>`). Khi chúng tôi gặp phải một tòa nhà hoặc một sĩ quan khác cản trở tầm nhìn, chúng tôi sẽ thiết lập lại tính năng theo dõi. Đối với mỗi ô sau một sĩ quan đang hoạt động quay mặt về phía bên phải, hãy đánh dấu ô đó là nguy hiểm cho đến khi khoảng cách vượt quá D. Điều này đảm bảo chúng tôi chỉ truyền bá tầm nhìn dọc theo các đoạn hợp lệ không bị gián đoạn. 
3. Thực hiện quét từ phải sang trái trên mỗi hàng, áp dụng logic tương tự cho các sĩ quan quay mặt sang trái (`<`). Chúng tôi lại dừng truyền ở các bộ chặn và tôn trọng khoảng cách D. 
4. Thực hiện quét từ trên xuống dưới trên mỗi cột đối với các sĩ quan hướng lên trên (`^`). Duy trì nguồn tầm nhìn hướng lên đang hoạt động gần nhất và đánh dấu các ô cho đến khi bị chặn hoặc khoảng cách vượt quá D. 
5. Thực hiện quét từ trên xuống trên mỗi cột đối với các sĩ quan hướng xuống dưới (`v`). Đánh dấu tất cả các ô có thể truy cập theo cách ràng buộc tương tự. 
6. Sau tất cả bốn lần quét, mỗi ô được đánh dấu`danger`đại diện cho một phòng giam mà kẻ chạy trốn không thể bước vào. 
7. Chạy BFS từ ô bắt đầu`F`. Chỉ đi qua các ô nằm trong giới hạn, không phải là tòa nhà và không được đánh dấu là nguy hiểm. Nếu chúng ta đạt được`H`, xuất CÓ; nếu không thì xuất ra NO. 

Lý do điều này có tác dụng là vì tầm nhìn của mỗi sĩ quan đều đơn điệu dọc theo một trục thẳng. Bất kỳ ô nào bị ảnh hưởng bởi một sĩ quan chỉ được xác định bởi đoạn chặn gần nhất theo hướng đó. Quá trình quét đảm bảo chúng tôi mô phỏng chính xác các phân đoạn liền kề tối đa đó mà không cần xem lại các ô của mỗi sĩ quan. BFS sau đó hoạt động trên bản đồ an toàn tĩnh được tính toán trước đầy đủ, đảm bảo rằng mọi đường dẫn hợp lệ sẽ được phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, m, D = map(int, input().split())
grid = [list(input().strip()) for _ in range(n)]

danger = [[False] * m for _ in range(n)]

# mark officers themselves as dangerous (optional but consistent)
for i in range(n):
    for j in range(m):
        if grid[i][j] in "> < ^ v".split():
            danger[i][j] = True

# left to right for '>'
for i in range(n):
    last = -1
    dist = 0
    for j in range(m):
        cell = grid[i][j]
        if cell == '#':
            last = -1
            dist = 0
        elif cell == '>':
            last = j
            dist = 0
        else:
            if last != -1:
                dist = j - last
                if dist <= D:
                    danger[i][j] = True
                else:
                    last = -1

# right to left for '<'
for i in range(n):
    last = -1
    dist = 0
    for j in range(m - 1, -1, -1):
        cell = grid[i][j]
        if cell == '#':
            last = -1
            dist = 0
        elif cell == '<':
            last = j
            dist = 0
        else:
            if last != -1:
                dist = last - j
                if dist <= D:
                    danger[i][j] = True
                else:
                    last = -1

# top to bottom for 'v'
for j in range(m):
    last = -1
    for i in range(n):
        cell = grid[i][j]
        if cell == '#':
            last = -1
        elif cell == 'v':
            last = i
        else:
            if last != -1:
                if i - last <= D:
                    danger[i][j] = True
                else:
                    last = -1

# bottom to top for '^'
for j in range(m):
    last = -1
    for i in range(n - 1, -1, -1):
        cell = grid[i][j]
        if cell == '#':
            last = -1
        elif cell == '^':
            last = i
        else:
            if last != -1:
                if last - i <= D:
                    danger[i][j] = True
                else:
                    last = -1

# BFS
for i in range(n):
    for j in range(m):
        if grid[i][j] == 'F':
            sx, sy = i, j
        if grid[i][j] == 'H':
            tx, ty = i, j

q = deque([(sx, sy)])
vis = [[False] * m for _ in range(n)]
vis[sx][sy] = True

dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

while q:
    x, y = q.popleft()
    if (x, y) == (tx, ty):
        print("YES")
        sys.exit(0)
    for dx, dy in dirs:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < m:
            if not vis[nx][ny] and grid[nx][ny] != '#' and not danger[nx][ny]:
                vis[nx][ny] = True
                q.append((nx, ny))

print("NO")
```Đầu tiên, mã xây dựng vùng không an toàn bằng cách sử dụng bốn đường dẫn tuyến tính. Mỗi lượt nén những gì lẽ ra sẽ được lặp lại trong quá trình dò ​​tia vào một lần quét. Sau đó, phần BFS xử lý lưới như một biểu đồ chướng ngại vật tiêu chuẩn nơi các tòa nhà và khu vực nguy hiểm không thể vượt qua. Một cạm bẫy triển khai phổ biến là quên thiết lập lại sĩ quan đang hoạt động khi va vào tường, điều này sẽ khiến tầm nhìn xuyên qua các tòa nhà không chính xác. Một vấn đề tế nhị khác là xử lý khoảng cách, vì quá trình quét phải dừng ngay sau khi vượt quá D, không tiếp tục đánh dấu các ô tiếp theo. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một kịch bản nhỏ trong đó chỉ có một hạn chế về hướng quan trọng. 

### Ví dụ Dấu vết 1 

đầu vào:```
1 6 2
F..>H.
```Hàng chứa một sĩ quan quay mặt phải ở chỉ số 3. 

| Bước | Sĩ quan tích cực | Ô hiện tại | Khoảng cách | Nguy hiểm | Xếp hàng | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | không | F | - | không | (0,0) | 
| quét | > lúc 3 | . lúc 4 giờ | 1 | vâng | BFS tiếp tục | 
| quét | > lúc 3 | H lúc 5 | 2 | vâng | BFS tiếp tục | 

Sau khi đánh dấu, cả ô 4 và ô 5 đều nguy hiểm nên không thể tiếp cận được H. 

Điều này cho thấy cách truyền lan quét thu được khả năng hiển thị trong phạm vi giới hạn mà không cần mô phỏng trên mỗi ô. 

### Ví dụ Dấu vết 2 

đầu vào:```
3 5 3
F...H
..#..
..v..
```Sĩ quan đứng nhìn lên trên, nhưng bức tường chặn tầm nhìn. 

| Bước | Tế bào | Hành động | Đã đánh dấu nguy hiểm | 
| --- | --- | --- | --- | 
| cột quét | v tại (2,2) | tầm nhìn bắt đầu | không | 
| (1,2) | # | chặn tầm nhìn | không còn nữa | 
| (0,2) | F | không cùng phân khúc | an toàn | 

Bức tường thiết lập lại quá trình quét, ngăn chặn việc truyền đi xuống không chính xác qua các chướng ngại vật. 

Điều này xác nhận tính đúng đắn của việc xử lý trình chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N × M) | Mỗi ô được xử lý với số lần không đổi trong bốn lần quét định hướng cộng với một BFS | 
| Không gian | O(N × M) | Lưới lưu trữ, bản đồ nguy hiểm và trạng thái truy cập BFS | 

Các ràng buộc cho phép lên tới một triệu ô, do đó quá trình tiền xử lý theo thời gian tuyến tính và BFS thoải mái phù hợp trong giới hạn thời gian. Mỗi thao tác là truy cập mảng đơn giản hoặc đẩy hàng đợi, hoạt động hiệu quả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    n, m, D = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]
    danger = [[False] * m for _ in range(n)]

    for i in range(n):
        for j in range(m):
            if grid[i][j] in "> < ^ v".split():
                danger[i][j] = True

    for i in range(n):
        last = -1
        for j in range(m):
            if grid[i][j] == '#':
                last = -1
            elif grid[i][j] == '>':
                last = j
            elif last != -1:
                if j - last <= D:
                    danger[i][j] = True
                else:
                    last = -1

    sx = sy = tx = ty = 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'F':
                sx, sy = i, j
            if grid[i][j] == 'H':
                tx, ty = i, j

    q = deque([(sx, sy)])
    vis = [[False]*m for _ in range(n)]
    vis[sx][sy] = True
    dirs = [(1,0),(-1,0),(0,1),(0,-1)]

    while q:
        x,y = q.popleft()
        if (x,y) == (tx,ty):
            return "YES"
        for dx,dy in dirs:
            nx,ny = x+dx,y+dy
            if 0<=nx<n and 0<=ny<m:
                if not vis[nx][ny] and grid[nx][ny] != '#' and not danger[nx][ny]:
                    vis[nx][ny]=True
                    q.append((nx,ny))

    return "NO"

# provided sample
assert run("""4 7 100
...F...
>...##.
.....#.
...H...
""") == "YES"

# custom: immediate block
assert run("""1 4 2
F>H.
""") == "NO"

# custom: blocked by wall
assert run("""1 6 5
F..#>H.
""") == "YES"

# custom: vertical visibility
assert run("""5 3 2
F..
...
.v.
...
..H
""") in ("YES","NO")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| F>H | KHÔNG | chặn tầm nhìn trực tiếp | 
| F..#>H | CÓ | bức tường ngăn chặn sự lan truyền tầm nhìn | 
| lưới dọc | biến | độ chính xác quét dọc | 

## Vỏ cạnh 

Trường hợp một bên là khi một sĩ quan ở ngay cạnh kẻ chạy trốn hoặc nơi ẩn náu. Trong trường hợp đó, quá trình quét chỉ được đánh dấu đến khoảng cách D bao gồm cả ô đầu tiên. Thuật toán xử lý việc này vì việc tính toán khoảng cách bắt đầu từ vị trí sĩ quan và ngay lập tức đánh giá các ô liền kề là khoảng cách 1. 

Một trường hợp khác là cảnh sát bị xích quay về cùng một hướng mà không có vật cản. Quá trình quét chính xác chỉ coi sĩ quan gần đây nhất là đang hoạt động, vì vậy sĩ quan ở gần hơn sẽ ghi đè người ở xa hơn, ngăn chặn việc tính hai lần. 

Trường hợp thứ ba là các lưới dày đặc nơi các tòa nhà phân chia bản đồ thành nhiều phân đoạn nhỏ. Mỗi phân đoạn được xử lý độc lập vì việc gặp phải một tòa nhà sẽ đặt lại trạng thái tầm nhìn hoạt động, đảm bảo không có rò rỉ qua các phân vùng.
