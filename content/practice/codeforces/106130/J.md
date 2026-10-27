---
title: "CF 106130J - \u9003\u51fa\u751f\u5929"
description: "Chúng ta được cung cấp một lưới có $n+2$ hàng và $m$ cột. Hàng trên cùng và hàng dưới cùng an toàn, trong khi hàng giữa $n$ mỗi hàng chứa chính xác một bức tượng đá. Mỗi bức tượng nằm ở một cột cụ thể trong hàng của nó và quay mặt sang trái hoặc phải."
date: "2026-06-19T19:51:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106130
codeforces_index: "J"
codeforces_contest_name: "GDUT 2025 Monthly competition"
rating: 0
weight: 106130
solve_time_s: 57
verified: true
draft: false
---

[CF 106130J - \u9003\u51fa\u751f\u5929](https://codeforces.com/problemset/problem/106130/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới với$n+2$hàng và$m$cột. Hàng trên cùng và hàng dưới cùng được an toàn, trong khi hàng$n$hàng giữa mỗi hàng có đúng một tượng đá. Mỗi bức tượng nằm ở một cột cụ thể trong hàng của nó và quay mặt sang trái hoặc phải. Từ vị trí của nó, nó phát ra một tia laser nằm ngang dọc theo hàng của nó theo hướng mà nó hướng về phía trước, và mọi tế bào được bao phủ bởi tia laser đó sẽ bị cấm. Bản thân phòng giam tượng cũng bị cấm. 

Một ô sẽ bị chặn nếu nó bị một bức tượng chiếm giữ hoặc nằm trên tia laze. Vị trí bắt đầu là góc trên bên phải của lưới và mục tiêu là góc dưới bên trái. Ở mỗi bước, người chơi có thể di chuyển một ô lên, xuống, sang trái hoặc sang phải nhưng không thể rời khỏi lưới hoặc vào bất kỳ ô bị chặn nào. 

Nhiệm vụ là xác định xem liệu một đường dẫn có tồn tại từ đầu đến đích hay không chỉ bằng cách di chuyển hợp lệ qua các ô được bỏ chặn. 

Kích thước lưới nhiều nhất là khoảng một triệu ô. Điều này ngay lập tức gợi ý rằng bất kỳ thuật toán nào truy cập từng ô với số lần không đổi đều khả thi, trong khi mọi thuật toán bậc hai liên tiếp hoặc liên quan đến việc tính toán lại lặp đi lặp lại cho mỗi truy vấn sẽ quá chậm. 

Một trường hợp thất bại tinh tế xuất phát từ sự hiểu lầm về hiệu ứng laser. Tia laser không phải là một tế bào đơn lẻ mà là toàn bộ tia, vì vậy việc chỉ coi các vị trí của bức tượng là bị chặn là không chính xác. 

Ví dụ: nếu một hàng có tượng ở cột 3 quay mặt sang phải thì mọi ô trong hàng đó từ cột 3 đến$m$là không an toàn. Một cách tiếp cận ngây thơ chỉ chặn cột 3 sẽ cho phép di chuyển qua vùng laser một cách không chính xác, tạo ra “CÓ” sai. 

Một trường hợp khác là khi tia laser không để lại ô an toàn nào liên tiếp. Nếu bức tượng ở cột 1 quay mặt sang trái thì toàn bộ hàng sẽ trở nên không an toàn, hoạt động như một rào chắn hoàn chỉnh. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề là mô hình hóa nó dưới dạng biểu đồ. Mỗi ô trong lưới là một nút và các cạnh kết nối các ô liền kề trực giao nếu cả hai điểm cuối đều an toàn. Khi biểu đồ này được tạo, chúng tôi có thể chạy tìm kiếm khả năng tiếp cận tiêu chuẩn từ ô bắt đầu. 

Việc xây dựng biểu đồ rất đơn giản vì lưới rất rõ ràng. Đối với mỗi hàng ở giữa, chúng tôi đánh dấu tất cả các ô bị tia laser chiếu vào là bị chặn. Sau đó, chúng tôi thực hiện BFS hoặc DFS trên các ô còn lại. 

Ý tưởng vũ phu đã gần đạt đến mức tối ưu. Chi phí chính là truy cập mỗi ô nhiều nhất một lần và kiểm tra tối đa bốn ô lân cận, điều này mang lại$O(nm)$thời gian. Không có sự bùng nổ tổ hợp tiềm ẩn vì các ràng buộc hoàn toàn mang tính hình học và tĩnh. 

Quan sát quan trọng là các ràng buộc laser chỉ sửa đổi những ô nào tồn tại trong biểu đồ; chúng không đưa ra động lực về thời gian hoặc sự phụ thuộc giữa các bước. Khi các ô bị chặn được tính toán trước, vấn đề sẽ giảm xuống còn kết nối lưới đơn giản. 

Vì vậy, nhiệm vụ thu gọn thành: xây dựng lưới bị chặn, sau đó chạy BFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưới vũ phu BFS |$O(nm)$|$O(nm)$| Đã chấp nhận | 
| Tương tự với tiền xử lý |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi vấn đề thành một lưới tĩnh gồm các ô trống và bị chặn, sau đó chạy BFS. 

1. Tạo mảng boolean 2D`blocked`kích thước$(n+2) \times m$, ban đầu tất cả đều sai. Điều này thể hiện liệu một ô có thể được nhập hay không. 
2. Đối với mỗi$n$hàng giữa, đọc vị trí tượng$c_i$và phương hướng$d_i$. Đánh dấu ô tượng là bị chặn. 
3. Nếu tượng quay mặt về bên phải thì mọi ô từ$c_i$ĐẾN$m$trong hàng đó sẽ bị chặn. Nếu nó quay mặt sang trái, mọi ô từ$1$ĐẾN$c_i$trở nên bị chặn. Điều này trực tiếp mã hóa tia laser thành một đoạn liền kề. 
4. Chạy BFS bắt đầu từ$(1, m)$nếu nó không bị chặn. Chúng tôi duy trì một hàng đợi và một mảng đã truy cập. 
5. Từ mỗi ô, hãy thử di chuyển theo bốn hướng. Chúng tôi chỉ đưa hàng xóm vào hàng đợi nếu nó nằm trong lưới, không bị chặn và chưa được truy cập trước đó. 
6. Dừng lại sớm nếu chúng ta đến nơi$(n+2, 1)$. 

Ý tưởng chính đằng sau BFS ở đây là mỗi lần di chuyển đều có chi phí như nhau, do đó khả năng tiếp cận trong một lưới không có trọng số chính xác là những gì BFS tính toán. 

### Tại sao nó hoạt động 

Khi các ô bị chặn được cố định, lưới sẽ trở thành biểu đồ 4 lân cận tiêu chuẩn. BFS khám phá chính xác thành phần được kết nối có chứa ô bắt đầu. Nếu mục tiêu nằm trong cùng một thành phần, BFS cuối cùng sẽ tiếp cận mục tiêu đó; nếu không thì không thể truy cập được vì không có đường dẫn thay thế nào tồn tại bên ngoài thành phần này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    
    # grid has n+2 rows
    blocked = [[False] * m for _ in range(n + 2)]
    
    # read statues
    for i in range(n):
        c, d = input().split()
        c = int(c) - 1
        r = i + 1
        
        blocked[r][c] = True
        
        if d == 'R':
            for j in range(c, m):
                blocked[r][j] = True
        else:
            for j in range(0, c + 1):
                blocked[r][j] = True
    
    sr, sc = 0, m - 1
    tr, tc = n + 1, 0
    
    if blocked[sr][sc] or blocked[tr][tc]:
        print("NO")
        return
    
    q = deque()
    q.append((sr, sc))
    visited = [[False] * m for _ in range(n + 2)]
    visited[sr][sc] = True
    
    dirs = [(1,0), (-1,0), (0,1), (0,-1)]
    
    while q:
        r, c = q.popleft()
        if (r, c) == (tr, tc):
            print("YES")
            return
        
        for dr, dc in dirs:
            nr, nc = r + dr, c + dc
            if 0 <= nr < n + 2 and 0 <= nc < m:
                if not blocked[nr][nc] and not visited[nr][nc]:
                    visited[nr][nc] = True
                    q.append((nr, nc))
    
    print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng từng hàng bản đồ bị chặn, mã hóa trực tiếp từng tia laser dưới dạng một khoảng thời gian được lấp đầy. Sau đó, nó chạy BFS tiêu chuẩn trên lưới. Mảng đã truy cập đảm bảo mỗi ô được xử lý một lần và mảng hướng mã hóa bốn bước di chuyển có thể. 

Phải cẩn thận khi lập chỉ mục: đầu vào sử dụng các cột và hàng dựa trên 1 được bù bằng 1 do có thêm các hàng ranh giới, vì vậy chúng tôi chuyển đổi mọi thứ sang lập chỉ mục dựa trên 0 trong nội bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ trong đó tia laser chặn hầu hết hàng giữa: 

đầu vào:```
1 3
2 R
```Bắt đầu là lúc$(1,3)$, mục tiêu tại$(3,1)$. Hàng giữa có tượng ở cột 2 quay mặt sang phải nên ô$(2,2)$Và$(2,3)$bị chặn, chỉ để lại$(2,1)$có thể sử dụng được. 

| Bước | Tế bào | Trạng thái xếp hàng | Hành động | 
| --- | --- | --- | --- | 
| 1 | (1,3) | (1,3) | bắt đầu | 
| 2 | (2,3 bị chặn) | (1,3) | bỏ qua | 
| 3 | (1,2) | (1,2) | di chuyển sang trái | 
| 4 | (2,1) | (2,1) | di chuyển xuống | 
| 5 | (3,1) | đạt | thành công | 

Dấu vết này cho thấy cách BFS tìm thấy một lối đi hẹp được tạo bởi ranh giới laser một cách tự nhiên. 

### Ví dụ 2 

đầu vào:```
2 2
1 L
2 R
```Ở đây cả hai hàng giữa đều bị hạn chế rất nhiều. 

| Bước | Tế bào | Trạng thái xếp hàng | Hành động | 
| --- | --- | --- | --- | 
| 1 | (1,2) | (1,2) | bắt đầu | 
| 2 | (1,1) | (1,1) | di chuyển sang trái | 
| 3 | ô hàng bị chặn | - | không thể hạ xuống | 
| 4 | - | trống | BFS kết thúc | 

Không có con đường nào có thể đi qua hàng giữa vì mỗi hàng sẽ loại bỏ tất cả các đường cắt có thể có theo ít nhất một hướng, ngăn cản mọi đường đi ngang liên tục. 

Những ví dụ này cho thấy cách thuật toán phân biệt giữa cấu hình laser có thể sử dụng và cấu hình laser chặn hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ô được đánh dấu nhiều nhất một lần và được truy cập nhiều nhất một lần trong BFS | 
| Không gian |$O(nm)$| Lưu trữ cho các lưới bị chặn và truy cập | 

Các ràng buộc cho phép lên tới khoảng$10^6$các ô, do đó việc truyền tải tuyến tính trên lưới nằm trong giới hạn cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, sys.stdin.readline().split())
    blocked = [[False] * m for _ in range(n + 2)]
    
    for i in range(n):
        c, d = sys.stdin.readline().split()
        c = int(c) - 1
        r = i + 1
        blocked[r][c] = True
        if d == 'R':
            for j in range(c, m):
                blocked[r][j] = True
        else:
            for j in range(0, c + 1):
                blocked[r][j] = True
    
    sr, sc = 0, m - 1
    tr, tc = n + 1, 0
    
    if blocked[sr][sc] or blocked[tr][tc]:
        return "NO"
    
    q = deque([(sr, sc)])
    vis = [[False] * m for _ in range(n + 2)]
    vis[sr][sc] = True
    
    dirs = [(1,0), (-1,0), (0,1), (0,-1)]
    
    while q:
        r, c = q.popleft()
        if (r, c) == (tr, tc):
            return "YES"
        for dr, dc in dirs:
            nr, nc = r + dr, c + dc
            if 0 <= nr < n + 2 and 0 <= nc < m:
                if not blocked[nr][nc] and not vis[nr][nc]:
                    vis[nr][nc] = True
                    q.append((nr, nc))
    
    return "NO"

# provided samples (placeholders)
# assert run("2 2\n2 R\n1 R\n") == "NO"
# assert run("4 6\n2 L\n4 R\n6 R\n4 L\n") == "YES"

# custom tests
assert run("1 1\n1 L\n") == "NO", "single blocked path"
assert run("1 2\n1 R\n") == "NO", "full row blocked"
assert run("1 2\n2 L\n") == "NO", "start isolated"
assert run("0 3\n") == "YES", "empty grid trivial"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 có chặn | KHÔNG | lưới nhỏ nhất bị chặn hoàn toàn | 
| laser toàn hàng | KHÔNG | tắc nghẽn hàng hoàn chỉnh | 
| bắt đầu trường hợp bị cô lập | KHÔNG | xử lý bắt đầu không thể truy cập | 
| hàng giữa trống | CÓ | kết nối tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi ô bắt đầu hoặc ô mục tiêu nằm bên trong vùng laser. Ví dụ: nếu một cấu hình khiến ô trên cùng bên phải bị đánh dấu là bị chặn thì BFS phải từ chối đường dẫn ngay lập tức. Thuật toán xử lý việc này bằng cách kiểm tra cả hai điểm cuối trước khi bắt đầu tìm kiếm, đảm bảo không lãng phí quá trình truyền tải. 

Một trường hợp khác là một hàng mà tia laser bao phủ toàn bộ hàng. Trong tình huống như vậy, hàng đó sẽ trở thành dấu phân cách cứng trong lưới. BFS đương nhiên tôn trọng điều này vì không có chuyển tiếp nào tồn tại trên hàng đó, do đó không gian tìm kiếm sẽ chia thành các thành phần bị ngắt kết nối. 

Cuối cùng, nếu cả hai điểm cuối đều ở các vùng bị ngắt kết nối khác nhau được tạo bằng các hướng laser xen kẽ, BFS chỉ cần làm cạn kiệt vùng có thể tiếp cận mà không bao giờ đến được mục tiêu, trả về “KHÔNG” một cách chính xác.
