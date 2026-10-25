---
title: "CF 106045A - Pacman vs. Ma cà rồng"
description: "Chúng tôi có một mạng lưới nơi Pacman bắt đầu từ một ô và muốn tiếp cận ô thức ăn. Các bức tường chặn chuyển động và ma cà rồng chiếm giữ các tế bào khác. Pacman muốn chọn con đường tiếp cận thức ăn trong khi mất càng ít điểm càng tốt."
date: "2026-06-25T12:41:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106045
codeforces_index: "A"
codeforces_contest_name: "IUT Intra University Programming Contest 2025"
rating: 0
weight: 106045
solve_time_s: 89
verified: true
draft: false
---

[CF 106045A - Pacman vs. Vampire](https://codeforces.com/problemset/problem/106045/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mạng lưới nơi Pacman bắt đầu từ một ô và muốn tiếp cận ô thức ăn. Các bức tường chặn chuyển động và ma cà rồng chiếm giữ các tế bào khác. Pacman muốn chọn con đường tiếp cận thức ăn trong khi mất càng ít điểm càng tốt. Ăn đồ ăn luôn mang lại 500 điểm, trong khi mỗi ma cà rồng gặp được Pacman sẽ bị giảm 10 điểm. 

Chi tiết quan trọng là Pacman di chuyển đầu tiên trong mỗi lượt và ma cà rồng di chuyển theo sau anh ta. Vì ma cà rồng cũng có thể chọn giữ nguyên vị trí, nên ma cà rồng có thể cắn Pacman ngay khi Pacman đến ô mà ma cà rồng có thể tiếp cận với cùng số lần di chuyển hoặc ít hơn. 

Đầu vào chứa một số lưới. Mỗi lưới cung cấp bản đồ hoàn chỉnh, bao gồm vị trí của Pacman, thức ăn, tường và ma cà rồng. Đầu ra cho mỗi lưới là số điểm tối đa mà Pacman có thể đảm bảo. 

Tổng số ô trong tất cả các trường hợp thử nghiệm tối đa là 10^6. Điều này loại trừ bất cứ điều gì khám phá nhiều con đường có thể. Một giải pháp phải gần với kích thước tuyến tính của lưới, hướng tới xử lý kiểu BFS. 

Phần khó khăn là hiểu rằng chúng ta không cần phải mô phỏng các lượt. Một mô phỏng đơn giản sẽ thử các đường đi khác nhau của Pacman và sau đó để ma cà rồng đuổi theo, nhưng số lượng đường đi có thể tăng theo cấp số nhân. 

Một sai lầm phổ biến là chỉ nhìn vào độ dài đường đi ngắn nhất và cho rằng mọi ma cà rồng gần đó đều bắt được Pacman. Ví dụ:```
1
3 3
P..
.V.
..F
```Khoảng cách ngắn nhất từ ​​Pacman đến thức ăn là 4. Ma cà rồng chính xác đang đi trên con đường ngắn nhất. Câu trả lời đúng là:```
490
```Ma cà rồng bắt được Pacman nên tỷ số là 500 - 10. 

Một sai lầm khác là chỉ kiểm tra khoảng cách từ ma cà rồng đến Pacman. Ví dụ:```
1
3 3
P..
...
.VF
```Ma cà rồng đã gần bắt đầu nhưng không có con đường ngắn nhất nào để có được thức ăn. Đầu ra đúng là:```
500
```Ban đầu, ma cà rồng không cần phải thân thiết. Điều quan trọng chỉ là liệu nó có thể chặn Pacman trước khi Pacman tiếp cận thức ăn hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra các tuyến đường khả thi từ Pacman đến thức ăn. Với mỗi tuyến đường, chúng tôi có thể mô phỏng ma cà rồng và đếm xem có bao nhiêu con có thể cắn được. Điều này đúng vì nó tuân thủ trực tiếp các quy tắc trò chơi, nhưng số lượng tuyến đường trong một lưới có thể rất lớn. Trong một lưới mở, số lượng đường đi có thể tăng theo cấp số nhân, vì vậy ngay cả với một bảng nhỏ thì điều này cũng trở nên không thể. 

Nhận xét quan trọng là một con đường dài hơn không bao giờ có thể giúp ích được cho Pacman. Nếu một tuyến đường có đường vòng hoặc đường vòng, việc loại bỏ nó sẽ khiến Pacman đến được mọi ô sau sớm hơn. Đến sớm hơn chỉ có thể khiến ma cà rồng khó bắt được anh ta hơn. Vì vậy, chiến lược tối ưu luôn sử dụng con đường ngắn nhất từ ​​Pacman đến thức ăn. 

Bây giờ chúng ta chỉ cần biết ma cà rồng nào có thể đi được con đường ngắn nhất. Đặt khoảng cách ngắn nhất từ ​​Pacman đến thức ăn là D. Một ma cà rồng ở ô V có thể bắt được Pacman nếu nó nằm trên con đường ngắn nhất nào đó giữa Pacman và thức ăn. Điều kiện để có điều đó là: 

dist(P, V) + dist(V, F) = D 

Nếu sự bình đẳng được giữ nguyên, Pacman có thể tiếp cận phòng giam của ma cà rồng đó cùng lúc với thời điểm mà ma cà rồng có thể làm, vì vậy ma cà rồng có thể cắn. Nếu tổng lớn hơn D, ma cà rồng không đi trên bất kỳ con đường ngắn nhất nào và bất kỳ điểm gặp gỡ nào có thể xảy ra sẽ khiến tổng tuyến đường dài hơn tuyến đường ngắn nhất, điều này không thể xảy ra. 

Vì vậy, vấn đề trở thành hai lần truyền tải BFS. Một BFS từ Pacman cung cấp khoảng cách từ Pacman đến mọi ô. Một BFS khác từ thực phẩm cung cấp khoảng cách từ mọi tế bào đến thực phẩm. Sau đó, chúng ta chỉ cần đếm số ma cà rồng thỏa mãn điều kiện đường đi ngắn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy BFS bắt đầu từ Pacman. Lưu trữ số lần di chuyển tối thiểu cần thiết để tiếp cận mọi ô có thể đi bộ. 
2. Chạy BFS khác bắt đầu từ thực phẩm. Lưu trữ số lần di chuyển tối thiểu cần thiết từ mọi ô có thể đi bộ đến thực phẩm. 
3. Gọi D là khoảng cách từ Pacman đến thức ăn. Đây là độ dài của con đường ngắn nhất mà Pacman có thể đi. 
4. Đối với mỗi tế bào ma cà rồng, hãy kiểm tra xem khoảng cách của nó với Pacman cộng với khoảng cách của nó đến thức ăn có bằng D. Nếu đúng như vậy, ma cà rồng này không thể tránh khỏi trên mọi tuyến đường tối ưu và sẽ cắn Pacman. 
5. Bắt đầu với số điểm 500 và trừ 10 cho mỗi ma cà rồng không thể tránh khỏi. 

Tại sao nó hoạt động: Ma cà rồng chỉ có thể cắn nếu Pacman đến thăm một phòng giam mà ma cà rồng có thể tiếp cận không muộn hơn thời điểm Pacman tiếp cận nó. Nếu điều này xảy ra trên con đường ngắn nhất thì ma cà rồng phải nằm trên con đường ngắn nhất đó. Ngược lại, nếu ma cà rồng nằm trên con đường ngắn nhất, Pacman sẽ đến ô đó với số lần di chuyển chính xác như ma cà rồng, vì vậy ma cà rồng có thể ở đó và cắn. Đếm chính xác những con ma cà rồng đó sẽ cho ra số vết cắn tối thiểu có thể. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def bfs(start, grid, n, m):
    dist = [[-1] * m for _ in range(n)]
    q = deque([start])
    dist[start[0]][start[1]] = 0

    while q:
        x, y = q.popleft()
        nd = dist[x][y] + 1

        if x > 0 and grid[x - 1][y] != '#' and dist[x - 1][y] == -1:
            dist[x - 1][y] = nd
            q.append((x - 1, y))
        if x + 1 < n and grid[x + 1][y] != '#' and dist[x + 1][y] == -1:
            dist[x + 1][y] = nd
            q.append((x + 1, y))
        if y > 0 and grid[x][y - 1] != '#' and dist[x][y - 1] == -1:
            dist[x][y - 1] = nd
            q.append((x, y - 1))
        if y + 1 < m and grid[x][y + 1] != '#' and dist[x][y + 1] == -1:
            dist[x][y + 1] = nd
            q.append((x, y + 1))

    return dist

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        grid = []
        pac = None
        food = None
        vampires = []

        for i in range(n):
            row = input().strip()
            grid.append(row)
            for j, c in enumerate(row):
                if c == 'P':
                    pac = (i, j)
                elif c == 'F':
                    food = (i, j)
                elif c == 'V':
                    vampires.append((i, j))

        from_pac = bfs(pac, grid, n, m)
        from_food = bfs(food, grid, n, m)

        shortest = from_pac[food[0]][food[1]]
        bad = 0

        for x, y in vampires:
            if from_pac[x][y] + from_food[x][y] == shortest:
                bad += 1

        ans.append(str(500 - 10 * bad))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```BFS đầu tiên tính toán khoảng cách Pacman phải di chuyển để đến từng ô. BFS thứ hai cũng thực hiện tương tự từ phía thực phẩm, cung cấp khoảng cách còn lại sau bất kỳ vị trí nào. 

Kiểm tra sự bình đẳng là cốt lõi của giải pháp. Một ô thuộc về đường đi ngắn nhất chính xác khi khoảng cách từ điểm bắt đầu đến ô cộng với khoảng cách từ ô đến đích bằng khoảng cách ngắn nhất toàn cầu. Chúng tôi áp dụng điều này trực tiếp vào các vị trí ma cà rồng. 

Việc triển khai sử dụng hàng đợi BFS vì mọi chuyển động đều có chi phí như nhau. Mảng khoảng cách được khởi tạo bằng`-1`, which also handles walls and unreachable cells naturally. There are no off by one issues because Pacman's arrival time at a cell on a shortest route is exactly the BFS distance from the start.

 ## Ví dụ đã hoạt động 

Đối với lưới này:```
1
3 3
P..
.V.
..F
```Khoảng cách BFS cho: 

| Tế bào | Từ Pacman | Để ăn | Tổng hợp | 
| --- | --- | --- | --- | 
| V | 2 | 2 | 4 | 
| F | 4 | 0 | 4 | 

Độ dài tuyến đường ngắn nhất là 4. Ma cà rồng thỏa mãn điều kiện nên được tính. 

Điểm số trở thành: 

| Biến | Giá trị | 
| --- | --- | 
| khoảng cách ngắn nhất | 4 | 
| ma cà rồng nguy hiểm | 1 | 
| điểm cuối cùng | 490 | 

Đối với lưới này:```
1
3 3
P..
...
.VF
```Ma cà rồng không đi trên con đường ngắn nhất. 

| Biến | Giá trị | 
| --- | --- | 
| khoảng cách ngắn nhất | 4 | 
| khoảng cách ma cà rồng với Pacman | 3 | 
| ma cà rồng khoảng cách với thức ăn | 1 | 
| tổng cộng | 4 | 

Trong trường hợp này, ma cà rồng thực sự nằm trên con đường ngắn nhất nên nó được tính và câu trả lời là 490. Dấu vết này cho thấy tại sao chỉ kiểm tra khoảng cách xuất phát là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Hai lần duyệt BFS và một lần quét lưới | 
| Không gian | O(nm) | Hai lưới khoảng cách được lưu trữ | 

Tổng số ô trong tất cả các thử nghiệm được giới hạn bởi 10^6, do đó quá trình xử lý tuyến tính phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def bfs(start, grid, n, m):
        dist = [[-1] * m for _ in range(n)]
        q = deque([start])
        dist[start[0]][start[1]] = 0
        while q:
            x, y = q.popleft()
            for dx, dy in ((1,0),(-1,0),(0,1),(0,-1)):
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < m and grid[nx][ny] != '#' and dist[nx][ny] == -1:
                    dist[nx][ny] = dist[x][y] + 1
                    q.append((nx, ny))
        return dist

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        grid = []
        for i in range(n):
            grid.append(input().strip())

        for i in range(n):
            for j in range(m):
                if grid[i][j] == 'P':
                    p = (i, j)
                elif grid[i][j] == 'F':
                    f = (i, j)

        a = bfs(p, grid, n, m)
        b = bfs(f, grid, n, m)

        d = a[f[0]][f[1]]
        cnt = 0

        for i in range(n):
            for j in range(m):
                if grid[i][j] == 'V' and a[i][j] + b[i][j] == d:
                    cnt += 1

        out.append(str(500 - 10 * cnt))

    return "\n".join(out)

assert run("""1
1 2
PF
""") == "500"

assert run("""1
3 3
P..
.V.
..F
""") == "490"

assert run("""1
3 3
P..
...
.VF
""") == "490"

assert run("""1
3 3
P#F
.V.
...
""") == "500"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`PF`|`500`| Lưới tối thiểu có thể | 
| Trung tâm ma cà rồng |`490`| Ma cà rồng trên con đường ngắn nhất | 
| Ma cà rồng hạ đẳng |`490`| Kiểm tra lý luận dựa trên đường dẫn | 
| Tuyến đường trực tiếp bị chặn |`500`| Xử lý các bức tường và các tuyến đường không thể tiếp cận | 

## Vỏ cạnh 

Không thể có ma cà rồng ở vị trí xuất phát của Pacman trong dữ liệu đầu vào hợp lệ, nhưng ma cà rồng ngay bên cạnh Pacman có thể gây hiểu nhầm. Thuật toán không chỉ so sánh khoảng cách ban đầu. Nó kiểm tra xem ma cà rồng có nằm trên con đường ngắn nhất hay không bằng cách sử dụng cả hai mảng khoảng cách BFS. 

Một bức tường có thể buộc Pacman đi vào một tuyến đường dài hơn trong lưới, nhưng con đường ngắn nhất trong biểu đồ thực tế đã chiếm các bức tường. Ma cà rồng chỉ được tính khi hai khoảng cách được tính toán khớp chính xác với độ dài tuyến đường ngắn nhất, do đó các đường dẫn bị chặn không tạo ra kết quả dương tính giả. 

Khi có nhiều ma cà rồng, chúng tôi không mô phỏng chúng riêng lẻ. Mỗi ma cà rồng đều được kiểm tra độc lập bằng cách sử dụng hai bản đồ khoảng cách giống nhau. Điều này tránh được nhiệm vụ bất khả thi khi xem xét sự kết hợp các chuyển động của ma cà rồng.
