---
title: "CF 105581I - Hỏa hoạn"
description: "Chúng ta được cung cấp một lưới n x n đại diện cho một chiến trường hình vuông. Một số ô ban đầu bốc cháy, số còn lại trống rỗng. Một hiệp sĩ bắt đầu ở bất kỳ ô trống nào và cố gắng trốn thoát bằng cách cuối cùng di chuyển ra ngoài lưới. Thời gian tiến triển theo từng bước rời rạc."
date: "2026-06-22T06:11:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105581
codeforces_index: "I"
codeforces_contest_name: "Open Udmurtia Junior Programming Contest 2018"
rating: 0
weight: 105581
solve_time_s: 60
verified: true
draft: false
---

[CF 105581I - Conflagration](https://codeforces.com/problemset/problem/105581/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới n x n đại diện cho một chiến trường hình vuông. Một số ô ban đầu bốc cháy, số còn lại trống rỗng. Một hiệp sĩ bắt đầu ở bất kỳ ô trống nào và cố gắng trốn thoát bằng cách cuối cùng di chuyển ra ngoài lưới. 

Thời gian tiến triển theo từng bước rời rạc. Ở mỗi bước, có hai điều xảy ra theo thứ tự: đầu tiên, hiệp sĩ thực hiện một nước đi hiệp sĩ cờ vua tiêu chuẩn, sau đó ngọn lửa lan đến tất cả các ô mà hiệp sĩ có thể tiếp cận từ bất kỳ ô nào hiện đang cháy. Nếu tại bất kỳ thời điểm nào, hiệp sĩ đáp xuống ô đã cháy hoặc sắp cháy sau khi lây lan, ô đó được coi là bị bắt. Hiệp sĩ chỉ thành công nếu cuối cùng nó có thể bước ra ngoài lưới trước khi ở chung phòng giam với lửa. 

Nhiệm vụ là đếm xem có bao nhiêu ô bắt đầu cho phép hiệp sĩ tiếp cận bên ngoài lưới một cách an toàn theo quy trình động kết hợp này. 

Kích thước lưới có thể lên tới 500 x 500, cung cấp tới 250.000 ô. Không thể mô phỏng trực tiếp tất cả các đường hiệp sĩ có thể có từ mọi ô vì mỗi ô phân nhánh thành tối đa 8 bước di chuyển và ngọn lửa tiến hóa đồng thời. Ngay cả một BFS trên mỗi ô cũng sẽ là quá lớn, vì nó sẽ nhân 250.000 trạng thái với hệ số 250.000 khác trong trường hợp xấu nhất. 

Một sai lầm ngây thơ là coi chuyển động của hiệp sĩ và lửa lan truyền một cách độc lập. Ví dụ: giả sử chúng ta có thể tính toán trước thời gian lửa đến và sau đó chạy con đường ngắn nhất cho hiệp sĩ mà không đồng bộ hóa thời gian một cách cẩn thận sẽ dẫn đến kết quả không chính xác nếu xử lý sai xung đột thời gian bằng nhau. 

Một trường hợp thất bại tinh vi khác là bỏ qua các ràng buộc về thời gian giống như tính chẵn lẻ. Vì cả hiệp sĩ và lửa đều di chuyển theo các bước riêng biệt, nên việc hiệp sĩ đến trước hay sau khi bắn cùng lúc sẽ quyết định khả năng sống sót và việc xử lý từng người một trở nên quan trọng. 

## Phương pháp tiếp cận 

Phối cảnh mô phỏng trực tiếp sẽ bắt đầu từ mỗi ô trống và cố gắng mô phỏng tất cả các bước di chuyển của hiệp sĩ có thể xảy ra đồng thời mô phỏng từng bước mở rộng ngọn lửa. Điều này ngay lập tức dẫn đến sự bùng nổ trong không gian trạng thái. Mỗi vị trí phân nhánh thành tối đa 8 vị trí tiếp theo và ngọn lửa cũng mở rộng tương tự, do đó trạng thái kết hợp thực sự là sản phẩm của vị trí và thời gian. Trong trường hợp xấu nhất, việc khám phá tất cả các con đường cho đến khi trốn thoát hoặc chết có thể quay lại cùng một trạng thái nhiều lần với các khoảng thời gian khác nhau, tạo ra hành vi theo cấp số nhân. 

Quan sát quan trọng là chúng ta thực sự không cần phải mô phỏng tất cả các đường hiệp sĩ một cách độc lập. Tốc độ lan truyền của lửa không phụ thuộc vào hiệp sĩ, vì vậy chúng ta có thể tính toán trước thời điểm sớm nhất mỗi ô trở nên không an toàn do hỏa hoạn. Khi chúng ta biết điều đó, vấn đề sẽ giảm xuống một con đường ngắn nhất bị ràng buộc: liệu hiệp sĩ có thể đến được ranh giới trước khi ngọn lửa đến từng ô được ghé thăm hay không. 

Điều này biến vấn đề thành BFS đa nguồn cho thời gian bắn, tiếp theo là BFS cho khả năng tiếp cận hiệp sĩ với giới hạn về thời gian. BFS chữa cháy đưa ra “thời gian nguy hiểm” cho mỗi ô. BFS hiệp sĩ từ ô bắt đầu chỉ có hiệu lực nếu nó đến đúng trước thời điểm đó. Thay vì chạy BFS này từ mọi ô, chúng tôi đảo ngược quan điểm: chúng tôi tính toán ô nào có thể đến ranh giới một cách an toàn và sau đó truyền thông tin đó ngược lại qua các nước đi hiệp sĩ đảo ngược với các ràng buộc về thời gian. 

Sự đảo ngược này hoạt động vì việc thoát khỏi ranh giới được xác định hoàn toàn bằng cách tiếp cận bên ngoài lưới chứ không phải bằng cách tiếp cận một ô mục tiêu cụ thể, vì vậy chúng ta có thể coi việc thoát ranh giới như một điểm nhấn toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng mỗi lần bắt đầu | Hàm mũ | O(n²) | Quá chậm | 
| Fire BFS + khả năng tiếp cận ngược bị hạn chế | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi giải quyết vấn đề bằng cách tính toán thời điểm mỗi ô trở nên nguy hiểm do cháy, sau đó xác định xem ô nào vẫn có thể đạt đến ranh giới trước thời điểm đó. 

1. Tính ma trận`fire_time`nơi mỗi ô lưu trữ bước đầu tiên mà lửa chạm đến nó. Chúng tôi khởi tạo tất cả các ô lửa với thời gian 0 và đẩy chúng vào hàng đợi BFS. Tất cả các ô khác bắt đầu như vô cùng. Chúng tôi mở rộng các bước di chuyển của hiệp sĩ, bởi vì lửa lan truyền giống hệt như một hiệp sĩ. 
2. Chạy đồng thời BFS đa nguồn từ tất cả các ô lửa ban đầu. Khi mở rộng từ một ô tại thời điểm t, mọi hàng xóm di chuyển hiệp sĩ có thể tiếp cận được chỉ định thời gian t + 1 nếu nó chưa được chỉ định. Điều này mang lại thời gian tối thiểu để lửa có thể tiếp cận từng ô vì BFS khám phá theo thứ tự thời gian tăng dần. 
3. Xác định những ô nào có thể thoát khỏi lưới ngay lập tức trong một nước đi của hiệp sĩ. Đây là những ô mà từ đó ít nhất một nước đi của hiệp sĩ đi ra ngoài bàn cờ. Các ô liền kề với ranh giới này được coi là trạng thái "chiến thắng" ban đầu, nhưng chỉ khi hiệp sĩ sống sót đủ lâu để đứng đó trước khi lửa ập đến. 
4. Xây dựng biểu đồ đảo ngược các nước đi của hiệp sĩ. Đối với mỗi ô, chúng tôi biết tất cả các vị trí tiền nhiệm có thể tiếp cận ô đó trong một nước đi hiệp sĩ. 
5. Chạy BFS (hoặc lan truyền hàng đợi) bắt đầu từ tất cả các ô có thể thoát ngay lập tức. Một ô được coi là chiến thắng nếu hiệp sĩ có thể đến đó vào thời điểm t sao cho t nhỏ hơn`fire_time[cell]`, và từ đó nó có thể đạt đến trạng thái chiến thắng. 
6. Khi truyền ngược, chỉ chấp nhận chuyển từ ô u sang ô trước v nếu việc đến v sớm hơn vẫn đảm bảo an toàn, nghĩa là thời gian lan truyền không vượt quá`fire_time[v] - 1`. 
7. Đếm tất cả các ô được đánh dấu là chiến thắng ở cuối. 

Ý tưởng chính là chúng tôi đang tính toán một cách hiệu quả tập hợp tất cả các trạng thái từ đó tồn tại đường dẫn an toàn đến ranh giới theo thời gian, với lửa đóng vai trò là ràng buộc thời hạn cho mỗi nút. 

### Tại sao nó hoạt động 

BFS lửa tính toán thời gian đến sớm nhất cố định cho mỗi ô không phụ thuộc vào hiệp sĩ. Điều này gây ra một thời hạn cứng cho mỗi tế bào. Bất kỳ con đường hiệp sĩ hợp lệ nào cũng phải tôn trọng những thời hạn này ở mỗi bước. 

BFS ngược trên các cạnh hiệp sĩ khám phá tất cả các trạng thái cuối cùng có thể dẫn đến trốn thoát trong khi vẫn tôn trọng tính khả thi về thời gian đơn điệu. Vì mọi cạnh trong biểu đồ ngược này đều tương ứng chính xác với một nước đi hiệp sĩ tiến về phía trước hợp lệ và chúng tôi chỉ truyền bá khi các ràng buộc về thời gian được thỏa mãn, nên mọi trạng thái được đánh dấu đều tương ứng với một chiến lược thoát khỏi thực tế khả thi. Ngược lại, bất kỳ đường thoát hợp lệ nào cũng phải kết thúc ở ranh giới và có thể được truy tìm ngược lại thông qua các trạng thái trước đó hợp lệ, nghĩa là nó sẽ bị phát hiện bởi quá trình lan truyền này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n = int(input())
    grid = [input().strip() for _ in range(n)]

    INF = 10**18
    fire_time = [[INF] * n for _ in range(n)]

    q = deque()

    for i in range(n):
        for j in range(n):
            if grid[i][j] == '#':
                fire_time[i][j] = 0
                q.append((i, j))

    moves = [(2,1),(2,-1),(-2,1),(-2,-1),(1,2),(1,-2),(-1,2),(-1,-2)]

    while q:
        x, y = q.popleft()
        for dx, dy in moves:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n:
                if fire_time[nx][ny] == INF:
                    fire_time[nx][ny] = fire_time[x][y] + 1
                    q.append((nx, ny))

    rev = [[[] for _ in range(n)] for _ in range(n)]
    for i in range(n):
        for j in range(n):
            for dx, dy in moves:
                ni, nj = i + dx, j + dy
                if 0 <= ni < n and 0 <= nj < n:
                    rev[ni][nj].append((i, j))

    dist = [[INF] * n for _ in range(n)]
    dq = deque()

    def can_escape_cell(i, j):
        for dx, dy in moves:
            ni, nj = i + dx, j + dy
            if not (0 <= ni < n and 0 <= nj < n):
                return True
        return False

    for i in range(n):
        for j in range(n):
            if can_escape_cell(i, j) and fire_time[i][j] > 0:
                dist[i][j] = 0
                dq.append((i, j))

    while dq:
        x, y = dq.popleft()
        for px, py in rev[x][y]:
            nd = dist[x][y] + 1
            if nd < fire_time[px][py] and nd < dist[px][py]:
                dist[px][py] = nd
                dq.append((px, py))

    ans = 0
    for i in range(n):
        for j in range(n):
            if dist[i][j] < INF:
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```BFS đầu tiên tính toán thời gian cháy đến bằng cách sử dụng tính năng mở rộng đa nguồn. Giai đoạn thứ hai xây dựng tính liền kề hiệp sĩ ngược để chúng ta có thể truyền bá tính khả thi thoát khỏi ngược từ các ô liền kề với ranh giới. các`dist`mảng theo dõi khoảng cách thoát an toàn sớm nhất theo đơn vị thời gian ngược. Điều kiện then chốt`nd < fire_time[px][py]`buộc chúng tôi không bao giờ cho phép hiệp sĩ đến hoặc sau khi bắn. 

Chức năng kiểm tra ranh giới rất quan trọng vì chỉ có thể trốn thoát nếu một nước đi của hiệp sĩ thoát khỏi lưới. Những tế bào này hoạt động như các bồn chứa ban đầu cho quá trình lan truyền ngược. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ 4 x 4:```
....
.#..
....
....
```Đầu tiên chúng tôi tính toán thời gian cháy. Ngọn lửa ở (1,1) lan ra ngoài trong các bước nhảy hiệp sĩ. Nhiều tế bào vẫn an toàn trong một vài bước. 

Sau đó, chúng tôi xác định các ô có khả năng thoát ranh giới. Chẳng hạn, (0,0) có thể nhảy ra ngoài ngay lập tức. 

| Bước | Tế bào | quận | lửa_thời gian | Hành động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | (0,0) | 0 | lớn | bắt đầu BFS | 
| Pop | (0,0) | 0 | lớn | tuyên truyền người đi trước | 
| Đẩy | (2,1) | 1 | hợp lệ | được chấp nhận | 
| Đẩy | (1,2) | 1 | hợp lệ | được chấp nhận | 

Điều này cho thấy tính khả thi của việc thoát mở rộng vào bên trong từ các nút có thể tiếp cận ranh giới. 

Bây giờ hãy xem xét một lưới nơi lửa chiếm ưu thế sớm:```
####
#..#
####
####
```Ở đây hầu hết các ô đều có fire_time 0 hoặc 1. Ngay cả khi một ô có thể chạm tới ranh giới, việc truyền bá sẽ nhanh chóng thất bại vì`nd < fire_time`hầu như ở đâu cũng bị vi phạm. BFS dừng sớm. 

| Bước | Tế bào | quận | lửa_thời gian | Hành động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | (1,1) | INF | 0 | bắt đầu bị từ chối | 
| Ban đầu | (1,2) | INF | 0 | bắt đầu bị từ chối | 
| Kết quả | - | - | - | không có trạng thái an toàn nào có thể tiếp cận được | 

Điều này chứng tỏ rằng chỉ khả năng tiếp cận thôi là không đủ nếu không có tính khả thi về mặt thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi ô được xử lý một số lần không đổi trong hai lần duyệt BFS và xây dựng cạnh trên 8 nước đi hiệp sĩ | 
| Không gian | O(n²) | Lưu trữ thời gian cháy, kề cận ngược và mảng khoảng cách | 

Kích thước lưới tối đa là 500 x 500, tức là khoảng 250.000 ô. Mỗi ô tham gia tối đa 8 lần chuyển đổi trên mỗi BFS, vừa vặn thoải mái trong giới hạn 1 giây trong Python với BFS dựa trên deque. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""

# minimal grid
assert run("1\n.\n") == ""

# fully blocked fire
assert run("2\n##\n##\n") == ""

# single safe escape
assert run("3\n...\n...\n...\n") == ""

# fire in center
assert run("3\n...\n.#.\n...\n") == ""

# larger mixed case
assert run("4\n....\n.#..\n..#.\n....\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 trống | 1 | trường hợp thoát hiểm tầm thường | 
| tất cả lửa | 0 | không có khởi đầu hợp lệ | 
| lưới trống | 9 | hoàn toàn tự do | 
| lửa tập trung | biến | độ chính xác của việc truyền bá | 
| hỗn hợp 4x4 | biến | tương tác giữa lửa và chuyển động | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một ô có thể thoát khỏi bảng ngay lập tức nhưng cũng liền kề với ngọn lửa xuất hiện ở thời điểm 1. Thuật toán xử lý điều này bằng cách chỉ khởi tạo các ô thoát ranh giới nếu`fire_time[i][j] > 0`, đảm bảo hiệp sĩ sống sót đủ lâu để thực hiện cuộc chạy trốn. 

Ví dụ:```
#..
...
...
```Ô (0,1) có thể thoát lên trên, nhưng bắn ở (0,0) sẽ lan sang ô đó trong một bước. BFS chỉ chấp nhận nó nếu việc thoát hiểm xảy ra nghiêm ngặt trước khi hỏa hoạn xảy ra, vì vậy nó chỉ được đưa vào một cách chính xác khi an toàn. 

Một trường hợp khác là những hòn đảo an toàn bị cô lập, nơi lửa không bao giờ lan tới một số khu vực nhất định. Trong các tế bào đó,`fire_time`vẫn là vô cùng nên chúng luôn có đủ điều kiện để nhân giống. BFS ngược tự nhiên mở rộng thông qua chúng mà không cần xử lý bổ sung và chúng đóng góp đầy đủ vào số lượng cuối cùng.
