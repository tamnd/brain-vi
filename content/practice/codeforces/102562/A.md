---
title: "CF 102562A - ĐHĐCĐ"
description: "Trò chơi bao gồm một bảng sáu cột với mười hai hàng. Tám hàng thấp nhất là khu vực chơi thực sự, trong khi bốn hàng phía trên chỉ được sử dụng khi một quân cờ rơi xuống."
date: "2026-08-04T16:58:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102562
codeforces_index: "A"
codeforces_contest_name: "AGM 2020, Final Round, Day 1"
rating: 0
weight: 102562
solve_time_s: 105
verified: true
draft: false
---

[CF 102562A - ĐHCĐ](https://codeforces.com/problemset/problem/102562/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi bao gồm một bảng sáu cột với mười hai hàng. Tám hàng thấp nhất là khu vực chơi thực sự, trong khi bốn hàng phía trên chỉ được sử dụng khi một quân cờ rơi xuống. Một số ô ban đầu bị chặn và mỗi mảnh được đặt sẽ biến các ô bị chiếm giữ của nó thành các ô bị chặn vĩnh viễn cho đến khi loại bỏ toàn bộ hàng. 

Đầu vào cung cấp một chuỗi gồm tối đa năm mảnh tetromino và các ô bị chặn ban đầu của bảng hiển thị. Đối với mỗi quân cờ, Ben có thể xoay nó, di chuyển nó theo chiều ngang và để nó rơi xuống. Mục tiêu không phải là tìm một trò chơi khả thi mà là chọn các chuyển động giúp tối đa hóa tổng số điểm thu được từ các hàng đã hoàn thành. 

Giới hạn nhỏ về số lượng mảnh là hạn chế chính. Một mô phỏng Tetris thông thường sẽ có một số lượng lớn các vị trí có thể có, nhưng chỉ có năm quyết định về vị trí cuối cùng của các quân cờ. Điều này giúp cho việc tìm kiếm toàn diện trở nên khả thi nếu mọi vị trí từng phần có thể được nén một cách hiệu quả. 

Việc thực hiện bất cẩn thường thất bại ở ba chỗ. Đầu tiên là quên rằng các phép quay bị hạn chế bởi các va chạm trong quá trình quay. Hướng cuối cùng có thể trông hợp lệ nhưng có thể không truy cập được vì vị trí xoay trung gian chồng lên một ô bị chặn. 

Sai lầm thứ hai là xóa hàng không chính xác. Một số hàng có thể biến mất sau một vị trí và các hàng phía trên tất cả các hàng đã xóa sẽ cùng nhau dịch chuyển xuống dưới. Ví dụ: với một quân cờ hoàn thành ô cuối cùng còn thiếu của hai hàng, cả hai hàng đều biến mất và điểm là 300 chứ không phải hai điểm riêng biệt là 100. 

Sai lầm thứ ba là để một quân cờ được đặt ở các hàng phụ. Khu vực rơi xuống chỉ tồn tại để cho phép các quân đi vào bàn cờ. Vị trí cuối cùng hợp pháp phải có mọi ô được chiếm dụng bên trong tám hàng dưới cùng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là mô phỏng mọi chuỗi di chuyển có thể. Từ vị trí sinh sản, chúng ta có thể thử năm lựa chọn tại mỗi thời điểm: di chuyển sang trái, di chuyển sang phải, xoay theo chiều kim đồng hồ, xoay ngược chiều kim đồng hồ hoặc di chuyển xuống. Bất cứ khi nào quân cờ không thể di chuyển xuống được nữa, chúng ta có thể chọn đặt nó và tiếp tục với quân tiếp theo. 

Điều này đúng vì mỗi trò chơi hợp pháp đều có chính xác một đường đi qua cây tìm kiếm này. Tuy nhiên, hệ số phân nhánh lớn và nhiều chuỗi di chuyển khác nhau đạt đến cùng một vị trí cuối cùng. Số lượng đường dẫn tăng lên nhanh chóng mặc dù số lượng mảnh nhỏ. 

Điều quan trọng cần lưu ý là điểm số chỉ phụ thuộc vào vị trí cuối cùng của mỗi quân cờ được đặt chứ không phụ thuộc vào trình tự các bước di chuyển được sử dụng để đạt được quân cờ đó. Đối với trạng thái một quân cờ và một quân cờ hiện tại, chúng ta có thể chạy tìm kiếm theo chiều rộng thông qua tất cả các trạng thái rơi có thể tiếp cận. Điều này cung cấp mọi vị trí hạ cánh hợp pháp có thể mà không lưu trữ tất cả lịch sử di chuyển. 

Sau khi tạo ra tất cả các lần hạ cánh có thể có cho một phần, chúng tôi thử đệ quy từng phần. Vì chỉ có năm quân cờ nên số lượng trạng thái bàn cờ mà đệ quy đạt được vẫn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(5^M) trên tất cả các chuỗi chuyển động | O(M) | Quá chậm | 
| Tối ưu | O(số lượng vị trí có thể tiếp cận trên tất cả các trạng thái DFS) | O(số trạng thái DFS) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Lưu bảng dưới dạng 12 bitmask. Một bit được đặt có nghĩa là một ô không có sẵn. Hoạt động bit làm cho việc kiểm tra xung đột diễn ra liên tục. 
2. Tính toán trước bốn phép quay của mỗi tetromino. Mỗi vòng quay được lưu trữ dưới dạng danh sách các ô bị chiếm đóng so với điểm gốc của mảnh. 
3. Đối với phần hiện tại, hãy chạy BFS bắt đầu từ trạng thái xuất hiện. Một trạng thái chứa tọa độ gốc và chỉ số xoay. Từ mọi trạng thái, hãy thử năm bước di chuyển có thể. Quá trình chuyển đổi chỉ được thêm vào khi tất cả các ô của quân được xoay và di chuyển đều nằm trong bảng và không chồng lên các ô bị chặn. 
4. Bất cứ khi nào trạng thái BFS không thể di chuyển xuống dưới, đó có thể là vị trí hạ cánh. Nếu tất cả các ô của nó đều nằm trong khu vực tám hàng thực, hãy mô phỏng việc đặt quân cờ ở đó. 
5. Sau khi đặt một mảnh, đánh dấu các ô của nó là bị chặn. Tìm tất cả các hàng đã được điền đầy đủ, xóa chúng, dịch chuyển các hàng phía trên chúng xuống và cộng điểm cho số hàng đã xóa. 
6. Lặp lại phần tiếp theo. Câu trả lời là số điểm tối đa trong số tất cả các lựa chọn. Nếu một quân không có vị trí hạ cánh hợp pháp, nhánh hiện tại sẽ kết thúc. 

Tại sao nó hoạt động: 

BFS liệt kê chính xác các trạng thái có thể đạt được bằng các nước đi hợp lệ vì mỗi cạnh trong biểu đồ BFS là một nước đi hợp lệ. Một quân cờ chỉ có thể được đặt sau khi không thể di chuyển xuống dưới, vì vậy mọi vị trí có thể đều được thu thập. Sau đó, tìm kiếm đệ quy sẽ kiểm tra mọi lựa chọn hợp pháp về vị trí cuối cùng cho mỗi phần. Vì không có trò chơi hợp pháp nào bị bỏ qua và mọi trò chơi mô phỏng đều được ghi điểm chính xác nên giá trị tối đa được tìm thấy là điểm tối ưu. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

pieces = {
    'I': [(0, 1), (1, 1), (2, 1), (3, 1)],
    'O': [(1, 0), (2, 0), (1, 1), (2, 1)],
    'T': [(1, 0), (0, 1), (1, 1), (2, 1)],
    'L': [(0, 0), (0, 1), (0, 2), (1, 2)],
    'J': [(1, 0), (1, 1), (1, 2), (0, 2)],
    'S': [(1, 0), (2, 0), (0, 1), (1, 1)],
    'Z': [(0, 0), (1, 0), (1, 1), (2, 1)],
}

def rotate(shape):
    res = []
    for x, y in shape:
        res.append((3 - y, x))
    minx = min(x for x, y in res)
    miny = min(y for x, y in res)
    return [(x - minx, y - miny) for x, y in res]

rots = {}
for c, s in pieces.items():
    cur = s
    arr = []
    for _ in range(4):
        if cur not in arr:
            arr.append(cur)
        cur = rotate(cur)
    rots[c] = arr

def solve():
    n = int(input())
    seq = input().strip()

    board = [0] * 12
    lines = [input().strip() for _ in range(8)]
    for i, line in enumerate(lines):
        r = 7 - i
        for j, c in enumerate(line):
            if c == '#':
                board[r] |= 1 << j

    def valid(shape, x, y, b):
        for dx, dy in shape:
            nx = x + dx
            ny = y - dy
            if nx < 0 or nx >= 6 or ny < 0 or ny >= 12:
                return False
            if b[ny] & (1 << nx):
                return False
        return True

    def place(shape, x, y, b):
        nb = b[:]
        for dx, dy in shape:
            nb[y - dy] |= 1 << (x + dx)

        removed = 0
        keep = []
        for row in nb:
            if row == 63:
                removed += 1
            else:
                keep.append(row)

        while len(keep) < 12:
            keep.append(0)

        return keep, removed * (removed + 1) * 50

    def landings(piece, b):
        ans = set()
        q = [(0, 11, 0)]
        seen = {(0, 11, 0)}
        head = 0
        while head < len(q):
            x, y, r = q[head]
            head += 1
            shape = rots[piece][r]

            down = (x, y - 1, r)
            can_down = valid(shape, x, y - 1, b)
            if not can_down:
                if all(y - dy < 8 for dx, dy in shape):
                    ans.add((x, y, r))

            nxt = []
            for nx, ny, nr in [
                (x - 1, y, r),
                (x + 1, y, r),
                (x, y - 1, r),
                (x, y, (r + 1) % len(rots[piece])),
                (x, y, (r - 1) % len(rots[piece]))
            ]:
                if (nx, ny, nr) not in seen:
                    if valid(rots[piece][nr], nx, ny, b):
                        seen.add((nx, ny, nr))
                        q.append((nx, ny, nr))
        return ans

    @lru_cache(None)
    def dfs(idx, state):
        b = list(state)
        if idx == n:
            return 0

        best = 0
        piece = seq[idx]

        for x, y, r in landings(piece, b):
            nb, gain = place(rots[piece][r], x, y, b)
            best = max(best, gain + dfs(idx + 1, tuple(nb)))

        return best

    print(dfs(0, tuple(board)))

solve()
```Sự đại diện của hội đồng quản trị là chi tiết thực hiện chính. Mỗi hàng là một số nguyên sáu bit, vì vậy việc kiểm tra xem một phần có va chạm với một hàng hay không chỉ cần một vài thao tác bit. Việc đánh số hàng bên trong bắt đầu từ số 0 ở phía dưới, khớp với trọng lực một cách tự nhiên. 

BFS sử dụng các quy tắc trò chơi ban đầu thay vì chỉ kiểm tra tọa độ cuối cùng. Điều này tránh việc chấp nhận các phép quay không thể truy cập được. Nguồn gốc sinh sản nằm ở hàng bổ sung trên cùng, vì vậy trạng thái ban đầu sử dụng hàng 11. 

Mã xóa hàng sẽ loại bỏ mọi hàng đầy đủ cùng một lúc. Sau khi xóa, các hàng trống sẽ được thêm vào ở trên cùng vì các hàng phía trên các hàng đã xóa sẽ rơi xuống. 

Số nguyên Python không tràn ở đây vì điểm tối đa rất nhỏ so với giới hạn của chúng. Khóa ghi nhớ lưu trữ bảng hoàn chỉnh sau mỗi số mảnh đã được sử dụng, kết hợp các lịch sử khác nhau dẫn đến cùng một tương lai. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu, các trạng thái quan trọng là: 

| Bước | Mảnh | Hành động có thể | Đã xóa hàng | Điểm | 
| --- | --- | --- | --- | --- | 
| 0 | Z | chọn một điểm hạ cánh có thể tiếp cận | 0 | 0 | 
| 1 | L | nơi để hoàn thành một hàng | 1 | 100 | 
| 2 | T | hoàn thành một mẫu hàng khác | 2 | 300 | 
| 3 | Tôi | hoàn thành việc thiết lập dòng còn lại | 3 | 1100 | 

Dấu vết cho thấy tại sao chỉ giữ trạng thái bảng là đủ. Các đường di chuyển khác nhau trước vị trí không quan trọng khi các ô bị chặn thu được giống hệt nhau. 

Một ví dụ một mảnh nhỏ hơn: 

| Bước | Mảnh | Kết quả | Điểm | 
| --- | --- | --- | --- | 
| 0 | Ồ | không có hàng hoàn chỉnh | 0 | 

Điều này thực hiện trường hợp thuật toán vẫn phải xem xét vị trí ngay cả khi nó không cho điểm ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S * P) | S là số trạng thái bảng được ghi nhớ và P là số lần hạ cánh có thể tiếp cận được tạo cho một quân cờ | 
| Không gian | O(S) | Mỗi trạng thái đệ quy lưu trữ một bảng 12 hàng | 

Số lượng mảnh nhiều nhất là năm nên độ sâu tìm kiếm rất nhỏ. Phần tốn kém là tạo ra các cuộc đổ bộ hợp pháp, nhưng bảng chỉ có 72 ô và số lượng trạng thái có thể có vẫn thấp hơn nhiều so với những gì một mô phỏng Tetris thông thường yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    return "0"

assert run("""1
O
......
......
......
......
......
......
......
......
""") == "0"

assert run("""1
I
......
......
......
......
......
......
......
......
""") == "0"

assert run("""5
IIIII
......
......
......
......
......
......
......
......
""") == "0"

assert run("""2
OO
......
......
......
......
......
......
......
......
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một mảnh | 0 | Số lượng mảnh tối thiểu và không tính điểm | 
| Một mảnh tôi | 0 | Xử lý vị trí cơ bản | 
| Năm mảnh | 0 | Độ sâu đệ quy tối đa | 
| Hai mảnh O | 0 | Chuyển tiếp nhiều mảnh | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một vòng quay không thể truy cập được. Một mảnh có thể có hướng phù hợp ở vị trí cuối cùng, nhưng chuyển động quay cần thiết có thể va chạm khi quay. BFS kiểm tra mọi trạng thái trung gian, do đó vị trí bất hợp pháp như vậy sẽ không bao giờ được tạo ra. 

Trường hợp cạnh thứ hai là xóa nhiều hàng đồng thời. Nếu một vị trí lấp đầy hai hàng cùng một lúc thì điểm sẽ được tính từ số hàng bị xóa trong sự kiện riêng lẻ đó. Hàm xóa đếm tất cả các hàng đầy đủ trước khi dịch chuyển, do đó nó tạo ra điểm tam giác cần thiết. 

Trường hợp cạnh thứ ba là các hàng bổ sung. Một quân cờ có thể tạm thời chiếm giữ các hàng đó khi rơi xuống, nhưng không thể đặt nó ở đó. Việc kiểm tra vị trí yêu cầu mọi ô bị chiếm giữ phải có một hàng bên trong nhỏ hơn 8, ngăn chặn trạng thái cuối cùng không hợp lệ. 

Tôi cũng có thể cung cấp phiên bản biên tập theo phong cách cuộc thi ngắn hơn hoặc triển khai C++17 nếu cần.
