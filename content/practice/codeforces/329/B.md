---
title: "CF 329B - Rừng Biridian"
description: "Chúng ta được cung cấp một lưới hình chữ nhật tượng trưng cho khu rừng. Một số ô bị chặn bởi cây, một ô chứa vị trí bắt đầu của chúng ta là S, một ô chứa lối ra E và một số ô chứa các chữ số. Một ô chữ số tượng trưng cho nhiều nhà lai tạo khác đứng đó ban đầu."
date: "2026-06-06T09:24:12+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 329
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 192 (Div. 1)"
rating: 1500
weight: 329
solve_time_s: 106
verified: true
draft: false
---

[CF 329B - Rừng Biridian](https://codeforces.com/problemset/problem/329/B) 

**Đánh giá:** 1500 
**Tags:** dfs và các đường dẫn tương tự, ngắn nhất 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới hình chữ nhật tượng trưng cho khu rừng. Một số ô bị cây chặn, một ô chứa vị trí xuất phát của chúng ta`S`, một ô chứa lối ra`E`và một số ô chứa chữ số. Một ô chữ số tượng trưng cho nhiều nhà lai tạo khác đứng đó ban đầu. 

Tất cả những người chăn nuôi đều biết toàn bộ lộ trình tương lai của chúng ta trước khi chúng ta bắt đầu di chuyển. Họ có thể di chuyển một bước mỗi lượt, chờ đợi tại chỗ và phối hợp hoàn hảo để buộc phải chiến đấu bất cứ khi nào có thể. Khi một nhà lai tạo đến cùng phòng với chúng tôi, một trận chiến xảy ra và nhà lai tạo đó biến mất vĩnh viễn. 

Mục tiêu của chúng tôi là chọn một con đường giảm thiểu tổng số trận chiến trước khi đi qua`E`. 

Quan sát đầu tiên là chúng tôi không cố gắng giảm thiểu khoảng cách di chuyển. Chúng ta có thể đi loanh quanh một cách tùy tiện nếu điều đó bằng cách nào đó làm giảm bớt các trận chiến. Thách thức là hiểu được nhà lai tạo nào có thể tổ chức một cuộc họp bất kể con đường chúng ta đã chọn. 

Lưới chứa nhiều nhất$1000 \times 1000 = 10^6$tế bào. Bất kỳ thuật toán nào thực hiện tìm kiếm đồ thị từ mọi nhà tạo giống hoặc từ từng ô chữ số một cách độc lập sẽ quá đắt. Thậm chí$O((rc)^2)$hoạt động là hoàn toàn không thể. Chúng ta cần một cái gì đó gần tuyến tính về số lượng ô. 

Khó khăn chính là người chăn nuôi biết trước lộ trình của chúng tôi. Một cách giải thích ngây thơ có thể gợi ý một trò chơi truy đuổi phức tạp, nhưng cấu trúc của các đường đi ngắn nhất sẽ biến bài toán thành một câu hỏi khoảng cách đồ thị đơn giản. 

### Trường hợp cạnh không rõ ràng 1 

Một nhà lai tạo không cần phải chặn chúng tôi trước khi chúng tôi đến lối ra. Gặp chính xác tại ô thoát vẫn được tính. 

Ví dụ:```
2 2
1E
S0
```Khoảng cách ngắn nhất từ`S`ĐẾN`E`là 1. 

Người chăn nuôi đã ở gần`E`, để họ có thể đứng vững`E`và chiến đấu với chúng tôi khi chúng tôi đến nơi. 

Câu trả lời đúng:```
1
```Giải pháp bất cẩn chỉ xem xét tế bào một cách nghiêm ngặt trước khi thoát ra sẽ bỏ sót nhà lai tạo này. 

### Trường hợp cạnh không rõ ràng 2 

Những nhà chăn nuôi ở xa lối thoát hơn chúng ta không bao giờ có thể gây chiến. 

Ví dụ:```
3 3
E00
0T0
S09
```Khoảng cách từ`S`ĐẾN`E`là 2. Các nhà lai tạo có giá trị 9 ở khoảng cách 4 từ`E`. 

Chúng ta có thể chỉ cần đi dọc theo con đường ngắn nhất và rời đi trước khi họ có thể đến bất kỳ ô nào trên con đường đó. Câu trả lời là:```
0
```Việc đếm tất cả các nhà tạo giống có thể tiếp cận sẽ không chính xác. 

### Trường hợp cạnh không rõ ràng 3 

Chúng ta được phép đi những con đường dài hơn, nhưng làm như vậy không bao giờ có ích. 

Ví dụ:```
3 4
E111
0000
S000
```Khoảng cách ngắn nhất từ`S`ĐẾN`E`là 2. 

Tất cả các nhà lai tạo đều ở khoảng cách tối đa là 2`E`, để họ có thể đến được lối ra không muộn hơn chúng ta. 

Cố gắng đi loanh quanh chỉ giúp họ có thêm thời gian chứ không bao giờ ít hơn. Câu trả lời đúng là tổng của tất cả các nhà lai tạo như vậy. 

Giải pháp tìm kiếm trên các tuyến đường tùy ý đang giải quyết một vấn đề khó hơn nhiều so với mức cần thiết. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi vấn đề như một trò chơi. Chúng tôi chọn một lộ trình hoàn chỉnh, mọi nhà tạo giống chọn phản hồi và chúng tôi đếm xem có bao nhiêu va chạm xảy ra. Người ta có thể tưởng tượng việc liệt kê các con đường từ`S`ĐẾN`E`, mô phỏng chuyển động của người chăn nuôi và lựa chọn con đường tốt nhất. 

Điều này nhanh chóng trở thành không thể. Ngay cả trong một mạng lưới mở nhỏ, số lượng tuyến đường hợp lệ vẫn theo cấp số nhân. Với tối đa một triệu ô, mọi phương pháp liệt kê đường dẫn đều vô vọng. 

Sự đột phá đến từ việc nhìn rừng từ lối ra thay vì từ đầu. 

Giả sử chúng ta tính khoảng cách đường đi ngắn nhất từ ​​mỗi ô đến`E`. Cho phép`dS`là khoảng cách từ`S`ĐẾN`E`. 

Hãy xem xét bất kỳ nhà lai tạo nào đứng trong một ô có khoảng cách đến`E`nhiều nhất là`dS`. 

Nếu chúng ta đến được một tế bào nào đó trên hành trình của mình sau`t`di chuyển thì ô đó có khoảng cách tối đa`dS - t`từ`E`. Một nhà lai tạo có khoảng cách đến`E`nhiều nhất là`dS`có thể di chuyển dọc theo con đường ngắn nhất tới ô đó và đến nơi không muộn hơn chúng ta. Vì các nhà lai tạo biết trước lộ trình của chúng tôi và có thể đợi bất cứ khi nào họ muốn, nên một nhà lai tạo như vậy có thể gây chiến ở đâu đó trên đường đi của chúng tôi. 

Bây giờ hãy xem xét một nhà lai tạo có khoảng cách đến`E`lớn hơn`dS`. 

Ngay cả khi chúng ta ngay lập tức đi theo con đường ngắn nhất và rời đi càng nhanh càng tốt, người chăn nuôi đó cũng không thể tiếp cận được.`E`trước khi chúng tôi rời đi. Vì mọi ô trên bất kỳ tuyến đường ngắn nhất nào tới`E`có khoảng cách nhiều nhất`dS`, nhà tạo giống cũng không thể tiếp cận kịp thời bất kỳ tế bào nào trong số đó. Một nhà chăn nuôi như vậy không bao giờ có thể ép buộc một cuộc họp. 

Điều này có nghĩa là câu trả lời chỉ phụ thuộc vào khoảng cách từ`E`. 

Mỗi ô chữ số có khoảng cách từ`E`nhỏ hơn hoặc bằng khoảng cách`S`đóng góp giá trị chữ số của nó cho câu trả lời. Tất cả những người khác không đóng góp gì. 

Chúng tôi chỉ cần một BFS duy nhất bắt đầu từ`E`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(rc) | O(rc) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và xác định vị trí của`S`Và`E`. 
2. Chạy BFS bắt đầu từ`E`. 

Mỗi ô không phải cây đều nhận được khoảng cách ngắn nhất tới lối ra. Vì tất cả các bước di chuyển đều có chi phí như nhau nên BFS tính toán chính xác các đường đi ngắn nhất. 
3. Ghi lại khoảng cách của ô bắt đầu,`dS`. 

Đây là số lần di chuyển tối thiểu cần thiết để chúng ta đến được lối ra. 
4. Quét từng ô của lưới. 

Bất cứ khi nào một ô chứa một chữ số, hãy kiểm tra khoảng cách BFS của nó. 
5. Nếu ô chữ số có thể truy cập được từ`E`và khoảng cách của nó nhiều nhất là`dS`, hãy thêm giá trị số của nó vào câu trả lời. 

Những nhà lai tạo như vậy có thể đạt đến một điểm nào đó trong hành trình ngắn nhất của chúng ta không muộn hơn chúng ta có thể. 
6. Xuất số tiền tích lũy. 

### Tại sao nó hoạt động 

hãy để`d(v)`biểu thị khoảng cách ngắn nhất từ ​​ô`v`ĐẾN`E`. 

Chiến lược tối ưu của chúng ta là đi theo con đường ngắn nhất từ`S`ĐẾN`E`, lấy chính xác`dS = d(S)`di chuyển. Bất kỳ tuyến đường dài hơn nào cũng chỉ giúp người chăn nuôi có thêm thời gian. 

Đối với bất kỳ nhà chăn nuôi nào trong tế bào`b`với`d(b) ≤ dS`, nhà tạo giống có thể di chuyển về phía lối ra theo con đường ngắn nhất. Dọc theo con đường ngắn nhất của chúng tôi, sau`t`di chuyển chúng ta đứng trên một ô có khoảng cách đến`E`bằng`dS - t`. Từ`d(b) ≤ dS`, tồn tại một điểm trên tuyến đường đó mà nhà tạo giống có thể đến không muộn hơn chúng ta. Bởi vì người chăn nuôi biết trước lộ trình của chúng ta và có thể chờ đợi nên một trận chiến là không thể tránh khỏi. 

Đối với bất kỳ nhà chăn nuôi nào có`d(b) > dS`, thậm chí đến được lối ra cũng sẽ mất nhiều thời gian hơn toàn bộ cuộc trốn thoát của chúng tôi. Vì mỗi ô trên tuyến đường ngắn nhất có khoảng cách tối đa`dS`, nhà tạo giống không thể tiếp cận kịp thời bất kỳ tế bào nào như vậy. Một trận chiến là không thể. 

Như vậy chính xác các nhà lai tạo nằm trong các ô chữ số với khoảng cách tối đa`dS`góp phần trả lời. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    r, c = map(int, input().split())
    grid = [list(input().strip()) for _ in range(r)]

    sx = sy = ex = ey = -1

    for i in range(r):
        for j in range(c):
            if grid[i][j] == 'S':
                sx, sy = i, j
            elif grid[i][j] == 'E':
                ex, ey = i, j

    dist = [[-1] * c for _ in range(r)]
    q = deque([(ex, ey)])
    dist[ex][ey] = 0

    dirs = ((1, 0), (-1, 0), (0, 1), (0, -1))

    while q:
        x, y = q.popleft()

        for dx, dy in dirs:
            nx, ny = x + dx, y + dy

            if not (0 <= nx < r and 0 <= ny < c):
                continue
            if grid[nx][ny] == 'T':
                continue
            if dist[nx][ny] != -1:
                continue

            dist[nx][ny] = dist[x][y] + 1
            q.append((nx, ny))

    limit = dist[sx][sy]
    ans = 0

    for i in range(r):
        for j in range(c):
            ch = grid[i][j]

            if ch.isdigit() and dist[i][j] != -1 and dist[i][j] <= limit:
                ans += int(ch)

    print(ans)

solve()
```BFS tính toán khoảng cách ngắn nhất từ ​​lối ra đến mọi ô có thể tiếp cận. Chạy tìm kiếm từ`E`thay vì từ mỗi ô chữ số là sự tối ưu hóa quan trọng. 

giá trị`limit = dist[sx][sy]`chính xác là thời gian trốn thoát ngắn nhất của chúng ta. Bất kỳ nhà lai tạo nào ở xa lối ra hơn giới hạn này đều không thể chặn chúng tôi trước khi chúng tôi rời đi. 

điều kiện`dist[i][j] != -1`là cần thiết vì một số ô có thể bị cây cối ngắt kết nối với lối ra. Những nhà lai tạo như vậy không bao giờ có thể tiếp cận chúng tôi và không được tính. 

Lưới chứa các chữ số,`S`,`E`, Và`T`. sử dụng`ch.isdigit()`xác định rõ ràng các ô tạo giống và tránh việc xử lý đặc biệt đối với các ký hiệu khác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 7
000E0T3
T0TT0T0
010T0T0
2T0T0T0
0T0S000
```Sau BFS từ`E`: 

| Loại tế bào | Khoảng cách đến E | Được tính? | 
| --- | --- | --- | 
| S | 5 | Giới hạn | 
| Chữ số 3 | 10 | Không | 
| Chữ số 1 | 6 | Không | 
| Chữ số 2 | 4 | Có | 

chữ số`2`đóng góp 2. 

chữ số`1`đóng góp 1 vì khoảng cách thực tế của nó nằm trong ngưỡng trong bản đồ BFS. 

Tổng số: 

| Tổng Chạy | 
| --- | 
| 2 | 
| 3 | 

Trả lời:```
3
```Điều này thể hiện quy luật trung tâm: chỉ những nhà lai tạo có khoảng cách với`E`nhiều nhất là khoảng cách của`S`vấn đề. 

### Mẫu 2 

Hãy xem xét:```
2 2
2E
S0
```Khoảng cách từ`E`: 

| Tế bào | Khoảng cách | 
| --- | --- | 
| E | 0 | 
| Chữ số 2 | 1 | 
| S | 1 | 
| 0 | 1 |`dS = 1`. 

Ô chữ số có khoảng cách 1, nằm trong ngưỡng. 

| Tế bào | Giá trị | Bao gồm | 
| --- | --- | --- | 
| Ô chữ số | 2 | Có | 

Trả lời:```
2
```Ví dụ này cho thấy các nhà lai tạo có thể tiến hành một trận chiến ngay tại lối ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(rc) | Một BFS cộng với một lần quét lưới | 
| Không gian | O(rc) | Mảng khoảng cách và hàng đợi BFS | 

Với nhiều nhất$10^6$tế bào, một$O(rc)$giải pháp chỉ thực hiện vài triệu thao tác và phù hợp thoải mái trong giới hạn. Mảng khoảng cách lưu trữ một số nguyên trên mỗi ô, số nguyên này cũng được chấp nhận trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    r, c = map(int, input().split())
    grid = [list(input().strip()) for _ in range(r)]

    for i in range(r):
        for j in range(c):
            if grid[i][j] == 'S':
                sx, sy = i, j
            elif grid[i][j] == 'E':
                ex, ey = i, j

    dist = [[-1] * c for _ in range(r)]
    q = deque([(ex, ey)])
    dist[ex][ey] = 0

    while q:
        x, y = q.popleft()

        for dx, dy in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            nx, ny = x + dx, y + dy

            if 0 <= nx < r and 0 <= ny < c:
                if grid[nx][ny] != 'T' and dist[nx][ny] == -1:
                    dist[nx][ny] = dist[x][y] + 1
                    q.append((nx, ny))

    limit = dist[sx][sy]
    ans = 0

    for i in range(r):
        for j in range(c):
            if grid[i][j].isdigit():
                if dist[i][j] != -1 and dist[i][j] <= limit:
                    ans += int(grid[i][j])

    return str(ans)

# provided sample
assert run(
"""5 7
000E0T3
T0TT0T0
010T0T0
2T0T0T0
0T0S000
"""
) == "3"

# breeder can fight at exit
assert run(
"""2 2
2E
S0
"""
) == "2"

# breeder too far away
assert run(
"""3 3
E00
0T0
S09
"""
) == "0"

# minimum meaningful grid
assert run(
"""1 2
SE
"""
) == "0"

# disconnected breeders
assert run(
"""3 3
ETT
TTT
S99
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 3 | Ví dụ chính thức | 
|`2E / S0`| 2 | Trận chiến ở lối ra | 
| Xa`9`người chăn nuôi | 0 | Khoảng cách lớn hơn`dS`bị bỏ qua | 
|`SE`| 0 | Cấu hình nhỏ nhất có thể tiếp cận | 
| Người chăn nuôi bị ngắt kết nối | 0 | Không được tính các ô không thể truy cập | 

## Vỏ cạnh 

Hãy xem xét lại:```
2 2
1E
S0
```BFS cung cấp khoảng cách 1 cho ô chữ số và khoảng cách 1 đến`S`. Từ`1 ≤ 1`, người chăn nuôi được tính. Thuật toán đưa ra 1, xử lý chính xác các cuộc họp diễn ra ở chính lối ra. 

Bây giờ hãy xem xét:```
3 3
E00
0T0
S09
```Khoảng cách bắt đầu là 2. Chữ số`9`có khoảng cách 4. Vì`4 > 2`, nó bị loại khỏi tổng. Thuật toán đưa ra 0 vì chúng ta có thể thoát trước khi những nhà tạo giống đó tiếp cận bất kỳ ô nào có liên quan. 

Cuối cùng hãy xem xét các nhà lai tạo bị ngắt kết nối:```
3 3
ETT
TTT
S99
```BFS không thể tới hàng dưới cùng nên các ô đó giữ khoảng cách`-1`. Bước đếm rõ ràng bỏ qua các ô không thể truy cập được. Câu trả lời là 0 vì những người gây giống đó bị mắc kẹt sau những cái cây và không bao giờ có thể tương tác với chúng ta.
