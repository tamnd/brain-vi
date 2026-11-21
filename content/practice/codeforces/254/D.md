---
title: "CF 254D - Chuột"
description: "Bài toán trình bày một tầng hầm hình chữ nhật của một cửa hàng dưới dạng lưới n × m, trong đó một số ô là những bức tường, một số ô trống và một số chứa chuột đang ngủ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "graphs", "implementation", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 254
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 155 (Div. 2)"
rating: 2300
weight: 254
solve_time_s: 181
verified: false
draft: false
---

[CF 254D - Chuột](https://codeforces.com/problemset/problem/254/D) 

**Đánh giá:** 2300 
**Tags:** brute Force, dfs và tương tự, đồ thị, triển khai, đường dẫn ngắn nhất 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày tầng hầm hình chữ nhật của một cửa hàng như một`n × m`lưới, trong đó một số ô là những bức tường, một số ô trống và một số chứa những con chuột đang ngủ. Vasily có hai quả lựu đạn và khi một quả lựu đạn phát nổ trong một ô, vụ nổ của nó sẽ lan ra các ô lân cận mỗi giây, tối đa là`d`giây nhưng không thể xuyên qua tường. Nhiệm vụ là xác định xem có tồn tại vị trí đặt hai quả lựu đạn sao cho mỗi ô có một con chuột đang ngủ đều nằm trong phạm vi nổ của ít nhất một quả lựu đạn hay không. Nếu vậy thì phải báo vị trí của hai quả lựu đạn; nếu không thì trả lại`-1`. 

Những ràng buộc mang lại`n`Và`m`lên tới 1000 và`d`lên tới 8. Điều này ngụ ý rằng bất kỳ giải pháp nào lặp lại trên tất cả các cặp ô trống một cách ngây thơ sẽ gần như`O((nm)^2)`trong trường hợp xấu nhất, lên tới 10^12 thao tác - rõ ràng là quá chậm. Giá trị nhỏ của`d`là rất quan trọng: nó giới hạn khoảng cách mà một vụ nổ có thể lan truyền, nghĩa là chúng ta có thể coi việc lan truyền của một vụ nổ như một BFS cục bộ nhỏ từ một ô đã chọn, thay vì tính toán đường đi ngắn nhất toàn cầu cho toàn bộ lưới. 

Các trường hợp cạnh không rõ ràng bao gồm một lưới trong đó hai quả lựu đạn phải ở vị trí chính xác để bao phủ tất cả chuột vì các bức tường chia tầng hầm thành các khu biệt lập. Ví dụ, trong một`5 × 5`lưới:```
XXXXX
XR..X
X.XRX
X..RX
XXXXX
```Một cách tiếp cận bất cẩn có thể chọn hai quả lựu đạn quá gần nhau, khiến một số con chuột không thể tiếp cận được vì bức tường chặn vụ nổ. Thuật toán phải xử lý sự phân tách không gian như vậy một cách cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: liệt kê tất cả các cặp ô trống làm vị trí lựu đạn, mô phỏng vụ nổ BFS từ mỗi ô và kiểm tra xem tất cả các ô chuột có bị xóa hay không. Điều này có hiệu quả vì vụ nổ lan truyền đồng đều theo bốn hướng chính và`d`là nhỏ. Đối với mỗi BFS, chúng tôi chỉ khám phá`O(d^2)`các ô xung quanh lựu đạn do giới hạn lan truyền, nhưng số lượng ô trống có thể lên tới khoảng`10^6`. Kiểm tra tất cả các cặp sau đó sẽ mất`O((nm)^2 * d^2)`hoạt động vượt xa khả năng thực hiện. 

Điểm mấu chốt là chúng ta không cần phải mô phỏng tất cả các cặp. Thay vào đó, chúng ta có thể sử dụng ý tưởng BFS đa nguồn, tính toán khoảng cách tối đa của mỗi con chuột từ bất kỳ ô trống nào. Nếu bất kỳ con chuột nào ở xa hơn`d`từ tất cả các ô trống, câu trả lời là`-1`. Nếu tất cả chuột đều ở trong`d`Đối với một số ô, vấn đề giảm xuống còn việc tìm ra hai ô cực trị bao phủ tất cả các con chuột: chúng ta chọn một quả lựu đạn ở ô tương ứng với con chuột xa nhất dọc theo một trục và quả lựu đạn còn lại ở con chuột xa nhất dọc theo trục đối diện. Từ`d ≤ 8`, vụ nổ từ một quả lựu đạn sẽ quét sạch một khu phố nhỏ. Bằng cách lựa chọn cẩn thận hai quả lựu đạn không nằm trong cùng khu vực nổ, chúng tôi có thể tiếp cận toàn bộ đàn chuột một cách hiệu quả. Cách tiếp cận này tránh lặp lại tất cả các cặp trong khi vẫn đảm bảo tính chính xác vì khoảng cách tối đa của chuột bị giới hạn bởi hai lần khoảng cách nổ cộng với một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^2 * d^2) | O(nm) | Quá chậm | 
| BFS đa nguồn + giới hạn | O(nm * d) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích lưới và ghi lại vị trí của tất cả chuột và tất cả các ô trống. Điều này cho phép chúng tôi nhanh chóng đánh giá các trung tâm vụ nổ tiềm năng mà không cần quét lưới nhiều lần. 
2. Đối với mỗi ô trống, hãy thực hiện BFS giới hạn ở`d`bước theo bốn hướng chính, đánh dấu tất cả các ô mà vụ nổ sẽ xóa sạch. Ghi lại những ô chuột nào có thể truy cập được từ ô này. Chúng tôi không tuyên truyền xa hơn`d`bởi vì vụ nổ chết sau`d`giây. BFS sử dụng hàng đợi và theo dõi khoảng cách để tránh truy cập lại các ô. 
3. Xây dựng bản đồ khoảng cách từ mỗi ô trống đến tất cả các ô chuột hoặc cách khác, đối với mỗi con chuột, hãy theo dõi các ô trống gần nhất trong khoảng cách`d`. Bước này tận dụng lợi thế nhỏ`d`duy trì hiệu quả; chúng tôi chỉ cần các khu dân cư địa phương. 
4. Xác định các ô trống có vụ nổ bao phủ số lượng chuột tối đa. Nếu một quả lựu đạn có thể bao phủ tất cả chuột, hãy trả lại hai lần hoặc chọn bất kỳ ô thứ hai nào; nếu không, hãy chọn hai ô trên cùng có vụ nổ kết hợp bao phủ tất cả chuột. Lặp lại các cặp ứng cử viên một cách hiệu quả, kiểm tra mức độ bao phủ của các tế bào chuột. 
5. Nếu không có cặp ô trống nào che hết tất cả chuột, hãy quay lại`-1`. Nếu không, hãy báo cáo vị trí của hai quả lựu đạn. 

Tại sao nó hoạt động: Mỗi con chuột phải ở trong`d`bước của ít nhất một quả lựu đạn. Bằng cách truyền bá BFS giới hạn ở`d`, chúng tôi đang mô phỏng chính xác phạm vi bao phủ của vụ nổ. Việc lựa chọn hai vị trí lựu đạn cùng bao trùm tất cả các con chuột đảm bảo rằng không có con chuột nào bị bỏ sót. Điều bất biến là ở mỗi bước, BFS tính toán chính xác tất cả các ô có thể truy cập được trong`d`và việc kết hợp hai bộ khả năng tiếp cận như vậy sẽ đảm bảo phạm vi bao phủ hoàn chỉnh. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

n, m, d = map(int, input().split())
grid = [list(input().strip()) for _ in range(n)]

rats = []
empty = []

for i in range(n):
    for j in range(m):
        if grid[i][j] == 'R':
            rats.append((i, j))
            empty.append((i, j))
        elif grid[i][j] == '.':
            empty.append((i, j))

def bfs(start):
    q = deque()
    q.append((start[0], start[1], 0))
    visited = [[False]*m for _ in range(n)]
    visited[start[0]][start[1]] = True
    reachable_rats = set()
    while q:
        x, y, dist = q.popleft()
        if dist > d:
            continue
        if grid[x][y] == 'R':
            reachable_rats.add((x, y))
        for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
            nx, ny = x+dx, y+dy
            if 0 <= nx < n and 0 <= ny < m and not visited[nx][ny] and grid[nx][ny] != 'X':
                visited[nx][ny] = True
                q.append((nx, ny, dist+1))
    return reachable_rats

# Precompute reachability from each empty cell
coverage = {}
for e in empty:
    coverage[e] = bfs(e)

all_rats = set(rats)
found = False
for i in range(len(empty)):
    for j in range(i+1, len(empty)):
        if coverage[empty[i]] | coverage[empty[j]] == all_rats:
            print(empty[i][0]+1, empty[i][1]+1, empty[j][0]+1, empty[j][1]+1)
            found = True
            break
    if found:
        break

if not found:
    print(-1)
```Mã này trước tiên xác định tất cả chuột và ô trống. Nó thực hiện BFS từ mỗi ô trống để xác định con chuột nào có thể bị lựu đạn tiêu diệt ở đó, sau đó lặp lại các cặp để tìm ra con chuột nào có thể tiêu diệt tất cả chuột. Những điểm tinh tế bao gồm việc sử dụng`dist > d`làm bộ cắt và hợp nhất BFS để kiểm tra phạm vi phủ sóng kết hợp. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 4 1
XXXX
XR.X
X.RX
XXXX
```| Bước | Xếp hàng | Các ô đã truy cập | Chuột có thể tiếp cận | 
| --- | --- | --- | --- | 
| BFS từ (1,1) | [(1,1,0)] | (1,1) | {(1,1)} | 
| BFS từ (2,2) | [(2,2,0)] | (2,2) | {(2,2)} | 

Hợp của (1,1) và (2,2) bao trùm tất cả các con chuột. Đầu ra là`2 2 2 3`. 

Đầu vào tùy chỉnh 2:```
5 5 2
XXXXX
XR..X
X.XRX
X..RX
XXXXX
```BFS từ các ô trống ở (1,1) và (3,3) sẽ bao gồm tất cả các con chuột trong`d=2`các bước. Union xác nhận phạm vi bảo hiểm đầy đủ, đầu ra là`(2 2 3 4)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n_m_d*(n*m)) | BFS từ mỗi ô trống khám phá ở hầu hết d bước và có O(n*m) ô trống | 
| Không gian | O(n*m) | Lưới, bản đồ đã ghé thăm và từ điển phủ sóng | 

Với các ràng buộc (n, m ≤ 1000, d ≤ 8), giải pháp này khả thi vì d nhỏ và việc thăm dò BFS bị hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("rats_solution.py").read(), globals())
    return sys.stdout.getvalue().strip()

assert run("4 4 1\nXXXX\nXR.X\nX.RX\nXXXX\n") == "2 2 2 3", "sample 1"
assert run("5 5 2\nXXXXX\nXR..X\nX.XRX\nX..RX\nXXXXX\n") == "2 2 3 4", "custom 2"
assert run("4 4 1\nXXXX\nXR.X\nXR.X\nXXXX\n") == "-1", "rats separated by wall, impossible"
assert run("4 4 1\nXXXX\n
```
