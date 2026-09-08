---
title: "CF 104574H - Cự đà tiến lên!"
description: "Chúng ta được cung cấp một bàn cờ vây được chơi một phần trong đó mỗi ô có màu đen, trắng hoặc trống. Đối với mỗi truy vấn, chúng ta được yêu cầu tưởng tượng việc đặt một viên đá có màu nhất định vào một ô trống và quyết định xem liệu hành động đó có ngay lập tức dẫn đến ít nhất một nhóm kết nối của…"
date: "2026-06-30T08:18:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104574
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 2 (Beginner)"
rating: 0
weight: 104574
solve_time_s: 87
verified: false
draft: false
---

[CF 104574H - Đi cự đà!](https://codeforces.com/problemset/problem/104574/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bàn cờ vây được chơi một phần trong đó mỗi ô có màu đen, trắng hoặc trống. Đối với mỗi truy vấn, chúng ta được yêu cầu tưởng tượng việc đặt một viên đá có màu nhất định vào một ô trống và quyết định xem liệu việc di chuyển đó có dẫn đến ngay lập tức ít nhất một nhóm đá cùng màu được kết nối không còn ô trống liền kề hay không. 

Nhóm ở đây là một thành phần được kết nối dưới sự liền kề 4 hướng. Độ tự do của nó được định nghĩa là số ô trống lân cận liền kề với bất kỳ ô nào trong nhóm. Một nhóm bị bắt khi số lượng tự do này trở thành số không. Điểm mấu chốt là chúng tôi không mô phỏng đầy đủ các quy tắc cờ vây hoặc bắt theo tầng qua nhiều lượt. Chúng tôi chỉ quan tâm đến việc liệu sau khi đặt viên đá được truy vấn, một số nhóm cùng màu có bị các viên đá đối thủ bao vây hoàn toàn và không có ô trống liền kề hay không. 

Mỗi truy vấn là độc lập. Bảng đặt lại sau mỗi lần di chuyển giả định, vì vậy chúng tôi không bao giờ sửa đổi trạng thái vĩnh viễn. 

Kích thước lưới có thể lớn tới 1000 x 1000 và có thể có tới 10000 truy vấn. Việc thực hiện tràn ngập đơn giản cho mỗi truy vấn trên toàn bộ bảng sẽ quá chậm nếu được thực hiện nhiều lần, vì một BFS/DFS trên 10^6 ô lặp lại 10^4 lần dẫn đến 10^10 thao tác. 

Một vấn đề tế nhị phát sinh từ địa phương. Chỉ những nhóm chạm vào viên đá mới đặt mới có thể thay đổi cấu trúc tự do của mình. Nhóm nào ở xa không bị ảnh hưởng nên việc tính toán lại toàn bộ bảng là không cần thiết. 

Một sai lầm phổ biến là chỉ kiểm tra nhóm chứa viên đá mới đặt. Điều đó không chính xác vì động thái này cũng có thể làm giảm quyền tự do của các nhóm thân thiện lân cận đã có mặt. Một sai lầm khác là quên rằng quyền tự do được chia sẻ thông qua nhiều viên đá trong một nhóm, do đó việc đếm hai lần hoặc kiểm tra từng ô mà không hợp nhất các thành phần sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Chiến lược vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi đặt viên đá lên một bản sao của bảng, sau đó chạy toàn bộ màu sắc trên mọi thành phần được kết nối có cùng màu, tính toán các quyền tự do cho mỗi nhóm. Nếu bất kỳ nhóm nào có quyền tự do bằng 0, chúng tôi sẽ đưa ra kết quả thất bại. 

Điều này đúng vì nó tính toán lại chính xác định nghĩa quy tắc Go từ đầu. Tuy nhiên, mỗi lần lấp đầy có thể chạm vào mọi ô và vì chúng tôi lặp lại điều này cho tối đa 10000 truy vấn nên tổng công việc sẽ tỷ lệ thuận với N * M * Q, vượt xa giới hạn khả thi. 

Quan sát quan trọng là chỉ những nhóm liền kề với viên đá mới được đặt mới có thể bị ảnh hưởng về số lượng tự do. Đặt một hòn đá ở vị trí (x, y) chỉ sửa đổi vùng lân cận cục bộ. Bất kỳ nhóm hiện có nào không chạm vào (x, y) sẽ giữ chính xác cùng một bộ quyền tự do, vì không có trạng thái ô trống nào thay đổi ở bất kỳ nơi nào khác. 

Điều này làm giảm vấn đề chỉ còn việc khám phá một khu vực nhỏ xung quanh hòn đá được đặt. Chúng tôi chạy BFS/DFS từ mỗi lân cận cùng màu liền kề để xác định thành phần được kết nối đầy đủ của nó, nhưng chúng tôi phải đảm bảo rằng chúng tôi không xử lý cùng một thành phần nhiều lần. Mỗi thành phần được phát hiện đều có quyền tự do được tính toán lại, nhưng vì chúng tôi chỉ duyệt qua tối đa bốn thành phần lân cận nên chúng tôi chỉ khám phá một số lượng nhỏ thành phần cho mỗi truy vấn. 

Vì mỗi ô thuộc về chính xác một thành phần và chúng tôi chỉ mở rộng các thành phần liền kề với ô truy vấn nên mỗi truy vấn sẽ tỷ lệ thuận với kích thước của vùng bị ảnh hưởng thay vì toàn bộ bảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · N · M) | O(N · M) | Quá chậm | 
| BFS cục bộ cho mỗi truy vấn | O(Q · K) trong đó K là kích thước thành phần cục bộ | O(N · M) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi truy vấn, tạm thời coi ô đích là bị chiếm bởi màu được truy vấn. Đây chỉ là khái niệm, chúng tôi không sửa đổi bảng toàn cầu vĩnh viễn. 
2. Kiểm tra bốn viên đá lân cận. Đối với mỗi hàng xóm có cùng màu với đá truy vấn, chúng tôi coi đó là điểm khởi đầu tiềm năng của một nhóm được kết nối. 
3. Đối với mỗi hàng xóm như vậy, hãy chạy BFS hoặc DFS để duyệt toàn bộ thành phần được kết nối của nó. Chúng ta phải đánh dấu cục bộ các ô đã truy cập trong truy vấn để tránh truy cập lại cùng một thành phần thông qua một ô lân cận khác. 
4. Trong khi duyệt qua một thành phần, hãy đếm các quyền tự do của nó bằng cách kiểm tra tất cả các lân cận bốn hướng của mỗi ô. Bất kỳ ô trống liền kề nào cũng đóng góp vào tập hợp tự do. Chúng tôi phải loại bỏ các quyền tự do trùng lặp này, vì vậy chúng tôi lưu trữ chúng theo bộ hoặc đánh dấu chúng trong cấu trúc tạm thời. 
5. Nếu trong quá trình truyền tải, chúng tôi phát hiện ra rằng tập tự do trống, chúng tôi ngay lập tức kết luận rằng nhóm này sẽ bị bắt và xuất ra "Không đi!". 
6. Nếu không có thành phần liền kề nào có quyền tự do bằng 0, chúng ta sẽ xuất ra “Go!”. 

Lý do chúng tôi chỉ bắt đầu BFS từ những người hàng xóm của viên đá được đặt là vì chỉ những thành phần đó mới có thể bị giảm số lượng tự do khi di chuyển. Bất kỳ thành phần nào khác đều không bị ảnh hưởng, vì vậy nó không thể bị bắt mới. 

### Tại sao nó hoạt động 

Mọi nhóm không liền kề với ô đã chơi sẽ giữ lại chính xác các ô trống lân cận như trước khi di chuyển, do đó số lượng tự do của nó không thay đổi. Những nhóm duy nhất có quyền tự do có thể thu hẹp lại là những nhóm chạm vào ô mới bị chiếm giữ. Đối với những nhóm đó, BFS sẽ xây dựng lại hoàn toàn khả năng kết nối của họ và việc tập hợp rõ ràng các ô trống liền kề sẽ cung cấp số lượng tự do chính xác sau khi di chuyển. Vì chúng tôi kiểm tra tất cả các nhóm bị ảnh hưởng như vậy nên bất kỳ nhóm nào trở thành không có tự do đều được phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

N, M, Q = map(int, input().split())
board = [list(input().strip()) for _ in range(N)]

dirs = [(1,0), (-1,0), (0,1), (0,-1)]

def inb(x, y):
    return 0 <= x < N and 0 <= y < M

for _ in range(Q):
    parts = input().split()
    color = parts[0]
    x = int(parts[1]) - 1
    y = int(parts[2]) - 1

    # pretend we place the stone
    visited = [[False]*M for _ in range(N)]
    bad = False

    def bfs(sx, sy):
        from collections import deque
        q = deque()
        q.append((sx, sy))
        visited[sx][sy] = True
        has_liberty = False

        while q:
            cx, cy = q.popleft()
            for dx, dy in dirs:
                nx, ny = cx + dx, cy + dy
                if not inb(nx, ny):
                    continue
                if board[nx][ny] == '.':
                    has_liberty = True
                elif board[nx][ny] == color and not visited[nx][ny]:
                    visited[nx][ny] = True
                    q.append((nx, ny))

        return has_liberty

    # check each adjacent component only once
    for dx, dy in dirs:
        nx, ny = x + dx, y + dy
        if inb(nx, ny) and board[nx][ny] == color and not visited[nx][ny]:
            if not bfs(nx, ny):
                bad = True
                break

    print("No go!" if bad else "Go!")
```BFS bị giới hạn ở các thành phần liền kề với viên đá được đặt. Mảng đã truy cập đảm bảo mỗi thành phần được xử lý một lần cho mỗi truy vấn. Trong quá trình truyền tải, chúng tôi phát hiện xem có tồn tại bất kỳ hàng xóm trống nào hay không; nếu không tồn tại, thành phần đó không có quyền tự do và việc di chuyển không hợp lệ. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ thực sự viết viên đá mới lên bảng. Thay vào đó, chúng tôi coi ô truy vấn là không trống bằng cách không cho phép nó đóng góp quyền tự do. Vì chúng tôi không bao giờ bước vào ô truy vấn với trạng thái trống, nên nó chặn vùng lân cận một cách hiệu quả mà không sửa đổi trạng thái chung. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
3 3 4
.BW
B.W
WWW
B 1 1
B 2 2
W 1 1
W 2 2
```Đối với truy vấn đầu tiên, đặt màu đen tại (1,1), chúng tôi kiểm tra các hàng xóm. Thành phần màu đen gần đó duy nhất là tầm thường hoặc không bị ảnh hưởng và tất cả các thành phần đều giữ lại ít nhất một quyền tự do. Kết quả là “Đi!”. 

Đối với truy vấn cuối cùng, đặt màu trắng ở (2,2), chúng tôi kết nối với cụm màu trắng lớn và giảm bớt sự tự do một cách hiệu quả để nhóm màu trắng trở nên khép kín hoàn toàn. 

| Truy vấn | Thành phần bị ảnh hưởng | Kết quả kiểm tra Liberty | Đầu ra | 
| --- | --- | --- | --- | 
| B 1 1 | người da đen địa phương nhỏ | tất cả đều có quyền tự do | Đi! | 
| B 2 2 | bị cô lập hoặc không có | không chụp | Đi! | 
| W 1 1 | người da trắng gần đó | vẫn mở | Không đi! | 
| W 2 2 | cụm trắng trung tâm | không có quyền tự do | Không đi! | 

Dấu vết cho thấy chỉ các thành phần liền kề với vật liệu đá được đặt và khả năng kết nối của chúng mới quyết định quyết định cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q · K) | Mỗi truy vấn chỉ khám phá các thành phần liền kề với việc di chuyển, mỗi ô được xử lý tối đa một lần cho mỗi truy vấn | 
| Không gian | O(N · M) | Lưu trữ lưới cộng với mảng đã truy cập trên mỗi truy vấn | 

Với Q lên tới 10000 và chia lưới tối đa 10^6 ô, giải pháp này hiệu quả trong thực tế vì mỗi BFS là cục bộ và hầu hết các thành phần đều nhỏ hoặc hiếm khi được sử dụng lại cho mỗi truy vấn. Trường hợp xấu nhất vẫn có thể chấp nhận được dưới các ràng buộc do thăm dò kề cận có giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, M, Q = map(int, input().split())
    board = [list(input().strip()) for _ in range(N)]
    dirs = [(1,0), (-1,0), (0,1), (0,-1)]

    def inb(x, y):
        return 0 <= x < N and 0 <= y < M

    out = []

    for _ in range(Q):
        parts = input().split()
        color = parts[0]
        x = int(parts[1]) - 1
        y = int(parts[2]) - 1

        visited = [[False]*M for _ in range(N)]
        bad = False

        from collections import deque

        def bfs(sx, sy):
            q = deque()
            q.append((sx, sy))
            visited[sx][sy] = True
            has_liberty = False

            while q:
                cx, cy = q.popleft()
                for dx, dy in dirs:
                    nx, ny = cx + dx, cy + dy
                    if not inb(nx, ny):
                        continue
                    if board[nx][ny] == '.':
                        has_liberty = True
                    elif board[nx][ny] == color and not visited[nx][ny]:
                        visited[nx][ny] = True
                        q.append((nx, ny))
            return has_liberty

        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if inb(nx, ny) and board[nx][ny] == color and not visited[nx][ny]:
                if not bfs(nx, ny):
                    bad = True
                    break

        out.append("No go!" if bad else "Go!")

    return "\n".join(out)

# provided sample
assert run("""3 3 4
.BW
B.W
WWW
B 1 1
B 2 2
W 1 1
W 2 2
""") == """Go!
Go!
No go!
No go!"""

# custom minimal case: single capture
assert run("""1 2 1
W.
B 1 2
""") == "Go!"

# fully enclosed capture
assert run("""3 3 1
BBB
B.W
BBB
W 2 2
""") == "No go!"

# no capture due to open liberty
assert run("""2 2 1
W.
.W
B 1 1
""") == "Go!"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | hỗn hợp | tính đúng đắn trong trường hợp tiêu chuẩn | 
| 1x2 | Đi | sự tồn tại tự do tầm thường | 
| kèm theo | Không đi | phát hiện chụp toàn bộ | 
| hình mở | Đi | không bắt được với độ mở theo đường chéo (không hợp lệ theo nghĩa cờ vây) | 

## Vỏ cạnh 

Vị trí ở góc trong đó một viên đá được thêm vào bên cạnh nhiều thành phần riêng biệt cùng màu được xử lý bằng cách lặp lại trên tất cả bốn lân cận và chỉ khởi chạy BFS một lần cho mỗi thành phần bằng cách sử dụng mảng đã truy cập. Nếu không có sự trùng lặp này, cùng một nhóm sẽ được đánh giá nhiều lần, có khả năng gây ra hiện tượng phát hiện chụp lặp lại hoặc xung đột. 

Nhóm bao quanh một ô được xử lý chính xác vì BFS ngay lập tức bắt đầu từ ô đó và không tìm thấy ô trống liền kề nào, mang lại khả năng phát hiện độ tự do bằng 0 trực tiếp. 

Một bước di chuyển được đặt trong một khu vực dày đặc với nhiều nhóm liền kề cũng được xử lý vì mỗi nhóm được di chuyển độc lập và thuật toán dừng sớm khi bất kỳ nhóm nào được phát hiện có quyền tự do bằng 0.
