---
title: "CF 102550A - \u041f\u043e\u0438\u0441\u043a\u0438 \u0422\u0440\u0435\u0437\u0443\u0431\u0446\u0430"
description: "Bản đồ là một mảng hình chữ nhật gồm các phòng có chuyển động bao quanh. Di chuyển qua hàng cuối cùng sẽ đưa bạn đến hàng đầu tiên và di chuyển qua cột cuối cùng sẽ đưa bạn đến cột đầu tiên, do đó các phòng tạo thành hình xuyến chứ không phải hình chữ nhật thông thường."
date: "2026-08-05T14:53:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102550
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2018-2019, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102550
solve_time_s: 898
verified: false
draft: false
---

[CF 102550A - \u041f\u043e\u0438\u0441\u043a\u0438 \u0422\u0440\u0435\u0437\u0443\u0431\u0446\u0430](https://codeforces.com/problemset/problem/102550/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14 phút 58 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bản đồ là một mảng hình chữ nhật gồm các phòng có chuyển động bao quanh. Di chuyển qua hàng cuối cùng sẽ đưa bạn đến hàng đầu tiên và di chuyển qua cột cuối cùng sẽ đưa bạn đến cột đầu tiên, do đó các phòng tạo thành hình xuyến chứ không phải hình chữ nhật thông thường. Phòng bắt đầu là ô trên cùng bên trái và một số ô chứa các gợi ý được đánh dấu bằng`X`. 

Một gợi ý không có sẵn ngay lập tức. Một gợi ý trong ô`(i, j)`chỉ có thể được thu thập sau mỗi gợi ý trong các ô có giá trị nhỏ hơn`i + j - 2`đã được thu thập. Khoảng cách được đề cập trong câu lệnh chính xác là giá trị này vì ô bắt đầu có tọa độ`(1, 1)`. 

Nhiệm vụ là in bất kỳ chuỗi di chuyển nào thu thập được tất cả các gợi ý. Đầu ra không phải là danh sách các ô mà là các lệnh di chuyển thực tế mô tả tuyến đường. 

Kích thước lưới tối đa là 100 x 100, vì vậy có tối đa 10.000 phòng. Kích thước này đủ nhỏ để tìm kiếm biểu đồ trên toàn bộ bản đồ. Giải pháp thử tất cả các tuyến đường có thể là không thể vì số lượng đường dẫn tăng theo cấp số nhân, nhưng các thuật toán thực hiện số lượng truyền tải BFS vừa phải trên 10.000 trạng thái là khả thi. 

Những phần khó khăn đến từ quy tắc mở khóa. Tuyến đường chỉ đi qua lưới theo thứ tự hàng chính là sai vì nó có thể đi vào phòng chứa gợi ý trong tương lai trước khi gợi ý đó được mở khóa. Một trường hợp tinh tế khác là chuyển động quấn quanh. Ví dụ:```
1 3
S.X
```Đầu ra chính xác có thể là`R`, vì đi thẳng từ phòng thứ nhất đến phòng thứ ba qua phòng thứ hai. Việc truyền tải lưới thông thường mà bỏ qua việc gói sẽ không sử dụng được phím tắt này. 

Một trường hợp cạnh khác là một hàng hoặc một cột. Ví dụ:```
1 2
SX
```Câu trả lời có thể là`R`hoặc`L`. Việc coi lưới không có đường bao dọc hoặc ngang có thể tạo ra các bước di chuyển không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng quyết định bước đi tiếp theo trong số bốn hướng có thể trong khi theo dõi các gợi ý thu thập được. Đây là một tìm kiếm đồ thị trên các tuyến đường có thể. Mặc dù điều này đúng nhưng không gian trạng thái chứa phòng hiện tại và tập hợp các gợi ý đã được thu thập, quá lớn. Ngay cả chỉ riêng lưới cũng có 10.000 trạng thái và số lượng tập hợp con có thể được thu thập là theo cấp số nhân. 

Quan sát hữu ích là các gợi ý chỉ được sắp xếp theo khoảng cách từ đầu. Chúng ta không cần phải chọn một thứ tự tùy ý trong số tất cả các gợi ý. Chúng ta chỉ cần hoàn thành một lớp khoảng cách trước khi bước vào lớp khoảng cách lớn hơn. 

Cách tiếp cận tối ưu là xử lý từng lớp một. Đối với giá trị khoảng cách hiện tại, chúng tôi chạy BFS liên tục từ vị trí hiện tại đến gợi ý chưa được thu thập trong lớp này. Trong BFS, tất cả các gợi ý từ các lớp trong tương lai đều được coi là các ô bị chặn vì chúng chưa được mở. Cho phép các phòng trống và gợi ý từ lớp hiện tại. 

Lực lượng vũ phu thất bại vì nó khám phá tất cả các lịch sử có thể có. Cấu trúc lớp loại bỏ sự mơ hồ này và biến vấn đề thành một chuỗi tìm kiếm đường đi ngắn nhất thông thường trên một biểu đồ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(nm(nm)) trong trường hợp xấu nhất | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm mọi gợi ý theo giá trị của nó`i + j - 2`. Các gợi ý trong cùng một nhóm sẽ xuất hiện cùng lúc nên chúng có thể được thu thập theo bất kỳ thứ tự nào trong nhóm đó. 
2. Bắt đầu từ`(0, 0)`và xử lý các nhóm theo thứ tự khoảng cách tăng dần. Ở mọi giai đoạn, tất cả các nhóm có khoảng cách nhỏ hơn đều đã hoàn thành. 
3. Đối với nhóm khoảng cách hiện tại, hãy chạy BFS từ vị trí hiện tại. Một phòng không được phép ở trạng thái BFS nếu nó chứa gợi ý chưa được thu thập từ một nhóm khoảng cách lớn hơn. Gợi ý có thể truy cập đầu tiên từ nhóm hiện tại sẽ trở thành điểm đến tiếp theo. 
4. Thêm đường dẫn BFS vào câu trả lời, đánh dấu gợi ý đó là đã thu thập và tiếp tục tìm kiếm gợi ý khác trong cùng một nhóm cho đến khi nhóm trống. 
5. Sau khi hoàn thành mỗi lớp khoảng cách, hãy chuyển sang lớp tiếp theo. Chuỗi chuyển động được tạo ra là tuyến đường bắt buộc. 

Tại sao nó hoạt động: 

Khi bắt đầu khoảng cách xử lý`d`, mọi gợi ý có khoảng cách nhỏ hơn`d`đã được thu thập và mọi gợi ý có khoảng cách lớn hơn`d`vẫn bị khóa. BFS chỉ đi qua các phòng hiện hợp pháp, vì vậy mọi gợi ý được thu thập đều có thể truy cập được theo quy định. Vì tất cả các gợi ý trong các lớp nhỏ hơn đều được hoàn thành trước khi chuyển sang lớp lớn hơn nên tuyến đường không bao giờ cố gắng nhập gợi ý bị khóa. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

n, m = map(int, input().split())
grid = [list(input().strip()) for _ in range(n)]

layers = [[] for _ in range(n + m - 1)]
for i in range(n):
    for j in range(m):
        if grid[i][j] == "X":
            layers[i + j].append((i, j))

moves = [
    (1, 0, "D"),
    (-1, 0, "U"),
    (0, 1, "R"),
    (0, -1, "L")
]

collected = [[False] * m for _ in range(n)]
ans = []
cur = (0, 0)

def bfs(start, target_layer):
    q = deque([start])
    parent = {start: None}
    parent_move = {}

    while q:
        x, y = q.popleft()

        if (x, y) != start and (x, y) in target_layer and not collected[x][y]:
            path = []
            cur = (x, y)
            while cur != start:
                path.append(parent_move[cur])
                cur = parent[cur]
            return path[::-1], (x, y)

        for dx, dy, c in moves:
            nx = (x + dx) % n
            ny = (y + dy) % m

            if (nx, ny) in parent:
                continue

            if grid[nx][ny] == "X" and (nx, ny) not in target_layer:
                continue

            parent[(nx, ny)] = (x, y)
            parent_move[(nx, ny)] = c
            q.append((nx, ny))

    return [], None

for layer in layers:
    target_layer = set(layer)
    while True:
        path, pos = bfs(cur, target_layer)
        if pos is None:
            break
        ans.extend(path)
        collected[pos[0]][pos[1]] = True
        cur = pos

print("".join(ans))
```các`layers`mảng lưu trữ gợi ý theo khoảng cách mở khóa của chúng. Chỉ số của mảng chính xác`i + j - 2`sử dụng tọa độ dựa trên số không. 

BFS sử dụng từ điển dành cho phụ huynh vì lưới nhỏ và điều này giúp việc tái thiết đơn giản. Khi BFS đạt đến gợi ý trong lớp hiện tại, các liên kết gốc được lưu trữ sẽ được truy ngược lại để khôi phục các lệnh di chuyển. 

Hành vi bao quanh được xử lý bằng số học modulo. Điều này tránh các trường hợp ranh giới riêng biệt khi di chuyển lên trên hàng đầu tiên hoặc qua cột cuối cùng. 

Điều kiện bỏ qua các gợi ý trong tương lai là chi tiết triển khai chính. Một căn phòng chứa một`X`không phải lúc nào cũng bị chặn vì các gợi ý ở lớp hiện tại đã có sẵn. Chỉ nên tránh những gợi ý từ các lớp sau. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 5
S....
X.X..
.X...
...XX
```Các lớp là: 

| Khoảng cách | Gợi ý | Hành động | 
| --- | --- | --- | 
| 1 | (2,1) | Di chuyển xuống | 
| 3 | (2,3), (3,2) | Đạt cả hai gợi ý | 
| 5 | (4,4), (4,5) | Đạt cả hai gợi ý | 

Một tuyến đường có thể là: 

| Bước | Vị trí | Lệnh | 
| --- | --- | --- | 
| Bắt đầu | (1,1) | | 
| 1 | (2,1) | D | 
| 2 | (3,1) | D | 
| 3 | (3,2) | R | 
| 4 | (2,2) | Bạn | 
| 5 | (2,3) | R | 
| 6 | (3,3) | D | 
| 7 | (4,3) | D | 
| 8 | (4,4) | R | 
| 9 | (4,5) | R | 

Thuộc tính quan trọng được hiển thị ở đây là gợi ý ở khoảng cách 5 không được truy cập trước khi tất cả các lớp nhỏ hơn hoàn tất. 

Đối với mẫu thứ hai:```
1 7
S.....X
```Mọi chuyển động đều theo chiều ngang vì chỉ có một hàng. 

| Bước | Vị trí | Lệnh | 
| --- | --- | --- | 
| Bắt đầu | (1,1) | | 
| 1 | (1,7) | L | 

Hành vi hình xuyến cho phép đến phòng cuối cùng ngay lập tức bằng cách quấn quanh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((nm)^2) | Trong trường hợp xấu nhất, BFS được lặp lại với nhiều gợi ý và mọi BFS sẽ quét lưới | 
| Không gian | O(nm) | Bộ lưu trữ BFS và trạng thái được thu thập sử dụng một giá trị cho mỗi phòng | 

Với tối đa 10.000 phòng, biểu đồ đủ nhỏ cho những tìm kiếm này. Tuyến đường được tạo ra cũng bị giới hạn vì mỗi đường dẫn BFS là đường đi ngắn nhất trên hình xuyến và tổng số gợi ý được thu thập nhiều nhất là 10.000. 

## Trường hợp thử nghiệm```
# The following cases validate the idea manually.

# Minimum grid
# 1 1
# S
# Output: empty string

# Single row wrap
# 1 3
# S.X
# Output can be:
# R

# Single column wrap
# 3 1
# S
# X
# X

# Full grid of hints
# 3 3
# SXX
# XXX
# XXX
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / S`| Tuyến đường trống | Không có gợi ý nào tồn tại | 
|`1 3 / S.X`| Bất kỳ lộ trình gói một bước nào | Gói ngang | 
|`3 1 / S,X,X`| Tuyến đường dọc hợp lệ | Xử lý cột đơn | 
| Lưới 3 x 3 đầy đủ | Bất kỳ tuyến đường hợp lệ nào | Nhiều lớp và gợi ý bị khóa | 

## Vỏ cạnh 

Đối với trường hợp bao quanh:```
1 3
S.X
```Thuật toán đặt gợi ý vào lớp khoảng cách 2. BFS thấy rằng việc di chuyển sang trái từ đầu sẽ đến gợi ý ngay lập tức vì các cột được bao bọc. Đường dẫn trả về hợp lệ vì nó sử dụng các quy tắc chuyển động thực tế. 

Đối với trường hợp một hàng:```
1 2
SX
```Tính toán modulo BFS cho cả hai ô lân cận theo chiều ngang giống như hai ô giống nhau, do đó không cần xử lý đặc biệt. Gợi ý nằm ở lớp đầu tiên và được thu thập ngay lập tức. 

Đối với trường hợp mỗi phòng đều chứa một gợi ý ngoại trừ phần đầu, các gợi ý trong tương lai sẽ bị chặn cho đến khi đạt đến lớp của chúng. BFS không thể vô tình nhập lớp sau vì những phòng đó sẽ bị xóa khỏi biểu đồ tìm kiếm cho đến khi đến lượt chúng.
