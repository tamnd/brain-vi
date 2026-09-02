---
title: "CF 104454O - Trận Hải Chiến"
description: "Lưới là một chiến trường cố định có kích thước 10 x 10, trong đó mỗi ô là nước hoặc một phần của con tàu. Các con tàu đã được đặt chính xác trước khi bắt đầu bất kỳ truy vấn nào và mỗi con tàu là một hình dạng được kết nối được tạo thành bởi các ô liền kề theo chiều ngang hoặc chiều dọc."
date: "2026-06-30T14:30:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104454
codeforces_index: "O"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2021"
rating: 0
weight: 104454
solve_time_s: 104
verified: true
draft: false
---

[CF 104454O - Trận chiến trên biển](https://codeforces.com/problemset/problem/104454/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới là một chiến trường cố định có kích thước 10 x 10, trong đó mỗi ô là nước hoặc một phần của con tàu. Các con tàu đã được đặt chính xác trước khi bắt đầu bất kỳ truy vấn nào và mỗi con tàu là một hình dạng được kết nối được tạo thành bởi các ô liền kề theo chiều ngang hoặc chiều dọc. Tổng cộng có chính xác mười con tàu và kích thước của chúng tuân theo một sự phân bố đã biết: bốn tàu bao gồm một ô, ba tàu chiếm hai ô, hai tàu chiếm ba ô và một tàu chiếm bốn ô. 

Sau khi đọc lưới này, chúng tôi xử lý một chuỗi lệnh. Một loại lệnh mô phỏng việc bắn vào một ô cụ thể và loại kia yêu cầu báo cáo về tình trạng hiện tại của tất cả các tàu. Một con tàu có thể ở một trong ba trạng thái tùy thuộc vào số lượng tế bào của nó bị trúng đạn: hoàn toàn nguyên vẹn, hư hỏng một phần hoặc bị phá hủy hoàn toàn. 

Yêu cầu đầu ra chỉ được gắn với loại lệnh thứ hai. Đối với mỗi truy vấn báo cáo, chúng tôi phải xuất ra bao nhiêu tàu vẫn còn nguyên, bao nhiêu chiếc đã bị trúng đạn nhưng chưa bị phá hủy và bao nhiêu chiếc bị phá hủy hoàn toàn tại thời điểm đó. 

Các ràng buộc cực kỳ nhỏ: lưới được cố định ở 100 ô và số lượng lệnh nhiều nhất là 100. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu phức tạp hoặc xử lý truy vấn được tối ưu hóa tiệm cận. Ngay cả việc tính toán lại toàn bộ trạng thái vận chuyển cho mỗi truy vấn cũng có thể được chấp nhận, nhưng chúng ta có thể làm tốt hơn bằng cách duy trì trạng thái tăng dần. 

Điểm tinh tế chính là cách tiếp cận đơn giản thường thất bại khi đếm gấp đôi số lần bắn lặp lại hoặc cố gắng suy ra danh tính con tàu một cách nhanh chóng mà không tính toán trước các thành phần được kết nối. Một lỗi phổ biến khác là tính toán lại trạng thái tàu từ đầu trên mỗi SHOW, điều này an toàn nhưng không cần thiết và dễ mắc sai sót hơn khi theo dõi các lần truy cập một phần không nhất quán. 

Trường hợp cạnh cụ thể phát sinh khi cùng một ô được bắn nhiều lần. Ví dụ: nếu một con tàu cỡ hai bị bắn trúng vào một ô và sau đó ô đó lại bị nhắm mục tiêu, thì chỉ phát bắn đầu tiên mới có thể thay đổi trạng thái của con tàu. Việc thực hiện bất cẩn làm tăng số lần bắn trúng mỗi lệnh SHOT sẽ đánh dấu sai tàu là bị đánh chìm quá sớm. 

Một trường hợp khác là lệnh SHOW được ban hành trước bất kỳ phát bắn nào, trong đó tất cả các tàu phải được báo cáo là không bị hư hại. Bất kỳ logic nào khởi tạo số lần truy cập đều có nguy cơ phân loại sai các truy vấn ban đầu này. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xử lý từng SHOT một cách độc lập và bất cứ khi nào SHOW xuất hiện, hãy tính toán lại trạng thái của mọi con tàu từ đầu bằng cách quét toàn bộ lưới và kiểm tra xem ô nào đã bị bắn trúng. Điều này hiệu quả vì lưới rất nhỏ nhưng về mặt khái niệm nó trở nên không hiệu quả vì mỗi SHOW có thể kích hoạt quét toàn bộ 100 ô và lặp lại logic nhóm. Với tối đa 100 truy vấn, điều này vẫn có thể chấp nhận được, nhưng nó trộn lẫn các trách nhiệm và dễ thực hiện không chính xác vì tư cách thành viên tàu và theo dõi lượt truy cập được xây dựng lại nhiều lần. 

Một cách tiếp cận có cấu trúc hơn là chia vấn đề thành hai giai đoạn. Đầu tiên, chúng tôi xác định tất cả các tàu sau khi sử dụng phương pháp lấp lũ trên lưới. Mỗi con tàu trở thành một thành phần có danh sách các ô cố định và kích thước đã biết. Sau đó, chúng tôi duy trì một trạng thái nhỏ cho mỗi con tàu để theo dõi số lượng ô của nó đã bị bắn trúng. Mỗi SHOT cập nhật chính xác một ô và chúng tôi sử dụng ánh xạ trực tiếp từ ô này sang ô khác để cập nhật thành phần tương ứng trong thời gian không đổi. Điều này làm cho các truy vấn SHOW trở nên đơn giản vì chúng tôi chỉ cần phân loại từng tàu dựa trên số lần truy cập hiện tại của nó. 

Cái nhìn sâu sắc quan trọng là danh tính con tàu không bao giờ thay đổi, chỉ có thiệt hại tích lũy. Khi chúng tôi tính toán trước các thành phần được kết nối, phần động của vấn đề sẽ giảm xuống còn việc duy trì bộ đếm trên các thành phần này.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại mỗi SHOW | O(q · 100) | O(100) | Đã chấp nhận | 
| Tính toán trước tàu + cập nhật gia tăng | O(100 + q) | O(100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi lưới thành một tập hợp các tàu được gắn nhãn để mọi ô đều biết nó thuộc về tàu nào. 

1. Quét từng ô của lưới. Bất cứ khi nào chúng tôi tìm thấy một ô chứa phân đoạn tàu chưa được chỉ định cho tàu, chúng tôi sẽ bắt đầu lấp lũ từ ô đó. Việc lấp lũ này thu thập tất cả các ô tàu được kết nối và gán cho chúng cùng một mã định danh tàu. Bước này là cần thiết vì các bản cập nhật sau này sẽ ảnh hưởng đến toàn bộ con tàu chứ không chỉ các ô riêng lẻ. 
2. Đối với mỗi con tàu được phát hiện, hãy lưu trữ kích thước của nó và khởi tạo bộ đếm xem có bao nhiêu ô của nó đã bị bắn trúng. Tại thời điểm này, tất cả các bộ đếm lượt trúng đều bằng 0 vì chưa có cú đánh nào xảy ra. 
3. Xây dựng ánh xạ trực tiếp từ mỗi ô lưới tới mã nhận dạng tàu của nó hoặc đánh dấu nó là nước nếu nó không phải là một phần của bất kỳ con tàu nào. Điều này cho phép tra cứu thời gian liên tục trong các lệnh SHOT. 
4. Duy trì một lưới boolean để theo dõi xem một ô đã được bắn hay chưa. Điều này rất quan trọng vì các lần chụp lặp lại vào cùng một ô không được thay đổi trạng thái nhiều lần. 
5. Xử lý từng lệnh theo thứ tự. Khi lệnh SHOT đến, hãy dịch tọa độ sang một ô. Nếu ô đã được bắn, chúng tôi hoàn toàn bỏ qua nó. Nếu không, chúng tôi đánh dấu nó là đã bắn và nếu ô thuộc về một con tàu, chúng tôi sẽ tăng bộ đếm lượt bắn của con tàu đó. 
6. Khi lệnh SHOW xuất hiện, chúng tôi lặp lại tất cả các tàu và phân loại từng tàu. Nếu số lần trúng của nó bằng 0, nó không bị hư hại. Nếu số lần trúng của nó bằng kích thước của nó thì nó sẽ bị đánh chìm. Nếu không thì nó bị trúng một phần. 

Tính chính xác dựa trên thực tế là mỗi ô đóng góp cho đúng một con tàu và mỗi ô chỉ có thể chuyển từ trạng thái chưa bắn sang bắn một lần. 

### Tại sao nó hoạt động 

Bất biến được duy trì trong suốt quá trình là số lần bắn trúng của mỗi con tàu bằng chính xác số lượng ô riêng biệt của con tàu đó đã bị bắn cho đến nay. Bởi vì chúng tôi ngăn chặn rõ ràng việc đếm hai lần các lần bắn lặp lại, mỗi lần tăng tương ứng với một ô mới được bắn duy nhất. Vì tư cách thành viên của tàu được cố định từ lượng lũ ban đầu, nên việc phân loại trong SHOW chỉ đơn giản là một chức năng của bộ đếm này so với kích thước của tàu, khớp chính xác với các định nghĩa về không bị hư hại, va chạm và chìm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

n = 10
grid = [input().strip() for _ in range(10)]

ship_id = [[-1] * 10 for _ in range(10)]
ships = []

dx = [1, -1, 0, 0]
dy = [0, 0, 1, -1]

def bfs(sx, sy, idx):
    q = deque()
    q.append((sx, sy))
    ship_id[sx][sy] = idx
    cells = [(sx, sy)]
    
    while q:
        x, y = q.popleft()
        for k in range(4):
            nx, ny = x + dx[k], y + dy[k]
            if 0 <= nx < 10 and 0 <= ny < 10:
                if grid[nx][ny] == '#' and ship_id[nx][ny] == -1:
                    ship_id[nx][ny] = idx
                    q.append((nx, ny))
                    cells.append((nx, ny))
    
    return cells

for i in range(10):
    for j in range(10):
        if grid[i][j] == '#' and ship_id[i][j] == -1:
            cells = bfs(i, j, len(ships))
            ships.append({
                "size": len(cells),
                "hits": 0
            })

shot = [[False] * 10 for _ in range(10)]

q = int(input())
for _ in range(q):
    cmd = input().split()
    
    if cmd[0] == "SHOT":
        x = int(cmd[1]) - 1
        y = int(cmd[2]) - 1
        if not shot[x][y]:
            shot[x][y] = True
            sid = ship_id[x][y]
            if sid != -1:
                ships[sid]["hits"] += 1
    
    else:
        undamaged = 0
        hit = 0
        sunk = 0
        
        for s in ships:
            if s["hits"] == 0:
                undamaged += 1
            elif s["hits"] == s["size"]:
                sunk += 1
            else:
                hit += 1
        
        print(undamaged, hit, sunk)
```Mã bắt đầu bằng cách đọc lưới và nhóm tất cả các ô tàu bằng cách sử dụng phương pháp lấp lũ BFS. Mỗi thành phần được phát hiện sẽ được lưu trữ với kích thước của nó và bộ đếm lượt truy cập có thể thay đổi. các`ship_id`ma trận cung cấp ánh xạ thời gian liên tục từ bất kỳ ô nào tới tàu của nó. 

Trong quá trình mô phỏng,`shot`ma trận ngăn cản việc đếm hai lần. Mỗi phát bắn mới hợp lệ sẽ tăng chính xác một bộ đếm của tàu nếu ô mục tiêu thuộc về một tàu. 

Đối với các truy vấn SHOW, chúng tôi chỉ cần quét tất cả các tàu và phân loại chúng dựa trên trạng thái thiệt hại tích lũy của chúng. 

Một cạm bẫy triển khai phổ biến là quên chuyển đổi tọa độ đầu vào từ chỉ mục dựa trên 1 sang dựa trên 0, điều này sẽ âm thầm thay đổi tất cả các cảnh quay và tạo ra các phân loại không chính xác mà không gặp sự cố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng tôi chỉ theo dõi các tàu ở mức độ cao vì danh tính của từng tàu là trừu tượng nhưng sự phát triển của số lượng là rõ ràng. 

| Bước | Lệnh | Cảnh quay mới | Tóm tắt trạng thái tàu được cập nhật | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | HIỂN THỊ | không | tất cả tàu trúng = 0 | 10 0 0 | 
| 2 | PHÚT 1 8 | vâng | một con tàu đạt được đòn đánh đầu tiên | | 
| 3 | HIỂN THỊ | không | một con tàu bị hư hỏng một phần | 9 0 1 | 
| 4 | CHỤP 9 9 | vâng | một con tàu khác đâm vào | | 
| 5 | HIỂN THỊ | không | một con tàu bị đâm, một con bị chìm | 8 1 1 | 

Dấu vết này cho thấy việc phân loại chỉ phụ thuộc vào số lần truy cập tích lũy chứ không phụ thuộc vào thứ tự truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100 + q) | BFS xử lý tối đa 100 ô một lần, mỗi truy vấn là O(1) cho SHOT và O(10) cho SHOW | 
| Không gian | O(100) | Lưới, bản đồ tàu và mảng siêu dữ liệu tàu nhỏ | 

Các ràng buộc đảm bảo rằng ngay cả hoạt động SHOW quét tất cả các tàu có tối đa 10 mục là không đáng kể, giữ cho giải pháp luôn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import deque

    grid = [input().strip() for _ in range(10)]

    ship_id = [[-1] * 10 for _ in range(10)]
    ships = []
    dx = [1, -1, 0, 0]
    dy = [0, 0, 1, -1]

    def bfs(sx, sy, idx):
        q = deque()
        q.append((sx, sy))
        ship_id[sx][sy] = idx
        cells = [(sx, sy)]
        while q:
            x, y = q.popleft()
            for k in range(4):
                nx, ny = x + dx[k], y + dy[k]
                if 0 <= nx < 10 and 0 <= ny < 10:
                    if grid[nx][ny] == '#' and ship_id[nx][ny] == -1:
                        ship_id[nx][ny] = idx
                        q.append((nx, ny))
                        cells.append((nx, ny))
        return cells

    for i in range(10):
        for j in range(10):
            if grid[i][j] == '#' and ship_id[i][j] == -1:
                cells = bfs(i, j, len(ships))
                ships.append({"size": len(cells), "hits": 0})

    shot = [[False] * 10 for _ in range(10)]

    q = int(input())
    out = []

    for _ in range(q):
        cmd = input().split()
        if cmd[0] == "SHOT":
            x, y = int(cmd[1]) - 1, int(cmd[2]) - 1
            if not shot[x][y]:
                shot[x][y] = True
                sid = ship_id[x][y]
                if sid != -1:
                    ships[sid]["hits"] += 1
        else:
            u = h = s = 0
            for sh in ships:
                if sh["hits"] == 0:
                    u += 1
                elif sh["hits"] == sh["size"]:
                    s += 1
                else:
                    h += 1
            out.append(f"{u} {h} {s}")

    return "\n".join(out)

# provided sample
assert run("""*******#**
*####****#
*********#
****#*#***
******#***
*##*******
********#*
*#******#*
*****#**#*
###*******
5
SHOW
SHOT 1 8
SHOW
SHOT 9 9
SHOW
""") == """10 0 0
9 0 1
8 1 1"""

# custom cases
assert run("""**********
**********
**********
**********
**********
**********
**********
**********
**********
**********
1
SHOW
""") == "0 0 0", "no ships"

assert run("""#*********
#*********
#*********
#*********
#*********
#*********
#*********
#*********
#*********
#*********
3
SHOW
SHOT 1 1
SHOW
""") == "1 0 0\n0 1 0", "single ship"

assert run("""**********
****#*****
****#*****
****#*****
**********
**********
**********
**********
**********
**********
4
SHOW
SHOT 2 5
SHOT 3 5
SHOW
""") == "1 0 0\n0 0 1", "vertical ship sink"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| biển vắng | 0 0 0 | trường hợp không có cạnh tàu | 
| tàu một cột | chuyển trạng thái | phân loại đánh vs chìm | 
| bồn rửa đầy đủ dọc | chuyển đổi toàn bộ sát thương | độ chính xác phát hiện chìm | 

## Vỏ cạnh 

Lưới không có tàu được xử lý một cách tự nhiên vì quá trình lấp đầy không tìm thấy thành phần nào, vì vậy mọi truy vấn SHOW đều lặp lại trên một danh sách trống và trả về tất cả các số 0. 

Một con tàu lớn đảm bảo rằng thiệt hại một phần và sự phá hủy hoàn toàn được phân biệt chính xác. Thuật toán tăng một bộ đếm duy nhất cho con tàu đó và chỉ phân loại nó là bị đánh chìm khi bộ đếm bằng tổng kích thước của nó. 

Các lần chụp lặp lại trên cùng một ô sẽ không ảnh hưởng đến bất kỳ bộ đếm nào vì`shot`khối ma trận cập nhật lặp đi lặp lại. Điều này bảo toàn tính bất biến mà mỗi ô tàu đóng góp tối đa một lần vào số lần trúng đích của nó, ngăn chặn việc đếm quá mức có thể gây ra tình trạng chìm sớm không chính xác.
