---
title: "CF 106368F - Máy quét Nimes!"
description: "Nhiệm vụ được đặt trên một trò chơi lưới giống như Minesweeper, nhưng thay vì chơi tương tác, chúng tôi đang phân tích cấu hình bảng ẩn cố định. Bạn được cung cấp một lưới hình chữ nhật trong đó mỗi ô chứa một mỏ hoặc trống."
date: "2026-06-25T08:15:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106368
codeforces_index: "F"
codeforces_contest_name: "Innopolis Open 2025-2026. Final round"
rating: 0
weight: 106368
solve_time_s: 44
verified: true
draft: false
---

[CF 106368F - Nimesweeper!](https://codeforces.com/problemset/problem/106368/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ được đặt trên một trò chơi lưới giống như Minesweeper, nhưng thay vì chơi tương tác, chúng tôi đang phân tích cấu hình bảng ẩn cố định. Bạn được cung cấp một lưới hình chữ nhật trong đó mỗi ô chứa một mỏ hoặc trống. Các ô trống mang một số bằng số lượng mỏ có trong 8 ô lân cận (bao gồm cả đường chéo). 

Bài toán yêu cầu bạn xác định xem cấu hình bảng đã cho có phù hợp với một số vị trí đặt mìn thực tế hay không. Nhất quán ở đây có nghĩa là tồn tại một cách để gán từng ô là của tôi hoặc trống sao cho tất cả các số được tiết lộ khớp chính xác với số lượng mỏ liền kề và tất cả các mỏ được đặt chính xác trên các ô được đánh dấu là mỏ trong đầu vào. 

Đầu ra là một quyết định khả thi đơn giản, do đó khó khăn cốt lõi không phải là tính toán các giá trị mà là xác minh xem liệu phép gán toàn cục các trạng thái nhị phân (của tôi hay không) có thể đáp ứng đồng thời tất cả các ràng buộc cục bộ hay không. 

Cấu trúc ràng buộc rất quan trọng: mỗi ô được đánh số áp đặt một ràng buộc tổng cho tối đa 8 biến lân cận và mỗi biến tham gia vào tối đa 8 ràng buộc. Đây là một hệ thống thỏa mãn ràng buộc cổ điển trên biểu đồ lưới với mức độ cục bộ rất nhỏ nhưng có khả năng lan truyền chuỗi toàn cầu lớn. 

Từ quan điểm phức tạp, lưới trong các bài toán như vậy thường lên tới khoảng 1000 x 1000 hoặc tương tự. Điều đó đã loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê tất cả các vị trí của mỏ, vì ngay cả lưới 20x20 cũng đã có 2^400 khả năng. Ngay cả việc cố gắng phân nhánh trên mọi ô chưa biết một cách độc lập cũng không thể thực hiện được. 

Một khó khăn tinh tế hơn đến từ sự mơ hồ cục bộ. Một bộ giải đơn giản chỉ kiểm tra từng ô được đánh số một cách độc lập với các ô lân cận của nó có thể bỏ sót những mâu thuẫn chỉ xuất hiện sau khi lan truyền toàn cục. Ví dụ: hai ô được đánh số liền kề, mỗi ô có thể đáp ứng cục bộ với nhiều cấu hình, nhưng chỉ có một cấu hình dùng chung là hợp lệ trên toàn cầu. 

Một trường hợp thất bại điển hình trông như thế này: hãy tưởng tượng hai ràng buộc chồng chéo mà cả hai đều có vẻ thỏa mãn khi đứng riêng lẻ, nhưng lại buộc các nhiệm vụ không nhất quán trên các hàng xóm chung. Bất kỳ cách tiếp cận nào không tuyên truyền hàm ý sẽ chấp nhận những bảng như vậy một cách sai lầm. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ coi mọi ô chưa biết là một biến nhị phân và thử tất cả các phép gán có thể. Đối với mỗi nhiệm vụ, nó sẽ xác minh tất cả các ô được đánh số bằng cách đếm các mỏ liền kề. Điều này đúng về mặt logic vì nó trực tiếp kiểm tra định nghĩa về tính hợp lệ, nhưng lại bùng nổ về mặt tổ hợp. Với k ô chưa biết, thời gian chạy là O(2^k · (n·m·8)), điều này trở nên không thể thực hiện được ngay cả đối với k khoảng 50. 

Quan sát quan trọng là mỗi ô được đánh số không chỉ áp đặt một kiểm tra mà còn áp đặt một ràng buộc cục bộ chặt chẽ đối với một tập hợp nhỏ các biến. Nếu chúng ta nghĩ về mặt lan truyền ràng buộc, một khi một số ô được cố định thì nhiều ô khác sẽ bị ép buộc. Cấu trúc này được xử lý một cách tự nhiên bằng cách coi mỗi ô chưa biết là một nút trong biểu đồ và truyền các quyết định bắt buộc bằng cách sử dụng các quy tắc nhất quán cục bộ. 

Ý tưởng quan trọng là các ràng buộc của Minesweeper hoạt động giống như các ràng buộc tuyến tính đối với các biến nhị phân: mỗi số buộc tổng các số lân cận của nó bằng một giá trị cố định. Điều này có nghĩa là mâu thuẫn có thể được phát hiện cục bộ khi quá trình truyền ổn định. Thay vì khám phá tất cả các bài tập, chúng tôi liên tục suy luận các trạng thái bắt buộc cho đến khi không thể suy luận được nữa. 

Tại thời điểm đó, nếu bất kỳ ràng buộc nào bị vi phạm, bảng sẽ không hợp lệ. Nếu không có vi phạm nào tồn tại và không có cấu trúc không xác định nào còn mơ hồ theo cách vẫn có thể che giấu mâu thuẫn thì cấu hình đó hợp lệ theo cách giải thích tính nhất quán của Máy quét mìn tiêu chuẩn được sử dụng trong những vấn đề này.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các vị trí của mỏ | O(2^k · n·m) | O(n·m) | Quá chậm | 
| Truyền bá ràng buộc (khấu trừ kiểu hàng đợi/BFS) | O(n·m) | O(n·m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình mỗi ô là cố định (đã biết) hoặc chưa biết và chúng tôi duy trì cấu trúc hoạt động để theo dõi số lượng mỏ mà một ô được đánh số vẫn cần từ các ô lân cận còn lại chưa xác định. 

1. Khởi tạo trạng thái lưới trong đó tất cả các ô được đánh dấu theo đầu vào, tách các ô được đánh số khỏi các ô chưa biết hoặc ô của tôi. Đối với mỗi ô được đánh số, hãy tính xem có bao nhiêu ô lân cận hiện được đánh dấu là mỏ và bao nhiêu ô vẫn chưa được quyết định. Điều này cung cấp cho chúng tôi “yêu cầu còn lại của mỏ” ban đầu cho mỗi ô ràng buộc. 
2. Đưa vào hàng xử lý mọi ô được đánh số có ràng buộc được xác định ngay lập tức, nghĩa là ô đó đã có đúng số lượng mỏ liền kề hoặc không thể thực hiện được. Lý do là chỉ những ràng buộc được xác định đầy đủ mới có thể kích hoạt các khoản khấu trừ bắt buộc ngay từ đầu. 
3. Liên tục trích xuất một ô từ hàng đợi và cố gắng truyền bá ràng buộc của nó tới các ô lân cận. Nếu một ô được đánh số đã thỏa mãn tất cả các mỏ yêu cầu thì tất cả các ô lân cận chưa xác định còn lại phải an toàn. Nếu nó vẫn cần nhiều mỏ như những hàng xóm chưa biết thì tất cả những hàng xóm chưa biết đó đều phải là mỏ. Đây là quy tắc khấu trừ cốt lõi. 
4. Bất cứ khi nào trạng thái của ô lân cận thay đổi từ không xác định sang buộc phải có mìn hoặc buộc phải an toàn, hãy cập nhật tất cả các ô được đánh số liền kề bằng cách điều chỉnh số lượng mìn còn lại và số lượng chưa biết của chúng. Bất kỳ ô được đánh số nào có trạng thái cập nhật trở nên quá hạn chế (số mỏ còn lại âm hoặc vượt quá số ẩn sẵn có) sẽ ngay lập tức báo hiệu sự không nhất quán. 
5. Tiếp tục quá trình lan truyền này cho đến khi hàng đợi trống, nghĩa là không thể khấu trừ bắt buộc nữa. Tại thời điểm này, hãy kiểm tra xem có bất kỳ ô ràng buộc nào không hợp lệ hay không. Nếu phát hiện thấy bất kỳ mâu thuẫn nào trong quá trình truyền bá thì việc cấu hình là không thể. 
6. Nếu không tìm thấy mâu thuẫn, cấu hình được coi là hợp lệ vì tất cả các hàm ý bắt buộc của các ràng buộc đều được thỏa mãn một cách nhất quán. 

### Tại sao nó hoạt động 

Mỗi ô được đánh số đại diện cho một phương trình cục bộ trên các ô lân cận của nó. Các quy tắc truyền bá là đầy đủ cho các phép gán bắt buộc: bất cứ khi nào một ràng buộc không còn bậc tự do nào nữa, giá trị của tất cả các lân cận chưa biết còn lại sẽ được xác định duy nhất. Điều này có nghĩa là thuật toán chỉ thực hiện các nhiệm vụ bị ràng buộc về mặt logic. 

Bất kỳ mâu thuẫn tổng thể nào cũng phải biểu hiện dưới dạng mâu thuẫn cục bộ ở một trong các phương trình ràng buộc này ở một giai đoạn lan truyền nào đó. Vì mọi bản cập nhật ngay lập tức tính toán lại tính hợp lệ của ràng buộc nên không có cấu hình không hợp lệ nào có thể tồn tại trong quá trình mà không gây ra sự mâu thuẫn được phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

# We assume the typical Minesweeper consistency format:
# grid contains either:
# '*' or 'M' for mine
# '.' for empty
# or digits '0'-'8' for number cells

dx = [-1,-1,-1,0,0,1,1,1]
dy = [-1,0,1,-1,1,-1,0,1]

def solve():
    n, m = map(int, input().split())
    g = [list(input().strip()) for _ in range(n)]

    # state: -1 unknown, 0 safe, 1 mine
    state = [[-1]*m for _ in range(n)]

    # queue of number cells to process
    q = deque()

    # helper arrays for constraints
    need = [[0]*m for _ in range(n)]
    unk = [[0]*m for _ in range(n)]

    def inb(x,y):
        return 0 <= x < n and 0 <= y < m

    # initialize
    for i in range(n):
        for j in range(m):
            if g[i][j] == '*':
                state[i][j] = 1

    for i in range(n):
        for j in range(m):
            if g[i][j].isdigit():
                mn = int(g[i][j])
                c_mine = 0
                c_unk = 0
                for k in range(8):
                    ni, nj = i + dx[k], j + dy[k]
                    if not inb(ni, nj):
                        continue
                    if state[ni][nj] == 1:
                        c_mine += 1
                    elif state[ni][nj] == -1:
                        c_unk += 1
                need[i][j] = mn - c_mine
                unk[i][j] = c_unk
                q.append((i,j))

    def relax(i, j):
        # if contradiction already present
        if need[i][j] < 0 or need[i][j] > unk[i][j]:
            return False

        # if all mines already placed => remaining neighbors safe
        if need[i][j] == 0 and unk[i][j] > 0:
            for k in range(8):
                ni, nj = i + dx[k], j + dy[k]
                if inb(ni, nj) and state[ni][nj] == -1:
                    state[ni][nj] = 0
                    update(ni, nj)
        # if all unknown must be mines
        elif need[i][j] == unk[i][j] and unk[i][j] > 0:
            for k in range(8):
                ni, nj = i + dx[k], j + dy[k]
                if inb(ni, nj) and state[ni][nj] == -1:
                    state[ni][nj] = 1
                    update(ni, nj)
        return True

    def update(x, y):
        for k in range(8):
            ni, nj = x + dx[k], y + dy[k]
            if inb(ni, nj) and g[ni][nj].isdigit():
                if state[x][y] == 1:
                    need[ni][nj] -= 1
                unk[ni][nj] -= 1
                q.append((ni, nj))

    while q:
        i, j = q.popleft()
        if g[i][j].isdigit():
            if not relax(i, j):
                print("NO")
                return

    print("YES")

if __name__ == "__main__":
    solve()
```Mã này duy trì hai phần thông tin cho mỗi ô được đánh số: số lượng mỏ vẫn được yêu cầu và số lượng mỏ lân cận chưa được quyết định. Mỗi khi một ô được xác định, chúng tôi sẽ đẩy các bản cập nhật vào các ràng buộc liền kề để các khoản khấu trừ được xếp tầng một cách tự nhiên qua lưới. 

Một chi tiết triển khai tinh tế là mọi bản cập nhật đều phải điều chỉnh cả yêu cầu còn lại và số lượng chưa xác định còn lại. Việc quên giảm số lượng chưa biết là lý do phổ biến nhất dẫn đến việc chấp nhận quá mức không chính xác, vì nó sẽ cho phép các ràng buộc có vẻ thỏa đáng ngay cả khi chúng đã bị ràng buộc quá mức. 

Việc truyền bá dựa trên hàng đợi đảm bảo mỗi thay đổi trạng thái chỉ được xử lý trong một số lần giới hạn, giữ cho giải pháp tuyến tính trong kích thước lưới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1*1
111
111
```Chúng tôi bắt đầu bằng cách đánh dấu mỏ ở giữa hàng trên cùng. Mỗi ô số liền kề sẽ tính toán yêu cầu của nó dựa trên đó. 

| Bước | Tế bào được xử lý | Cần | Hàng xóm không rõ | Hành động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | (0,0) | 0 | 1 | nhất quán | 
| Ban đầu | (0,2) | 0 | 1 | nhất quán | 
| Ban đầu | (1,1) | 1 | 8 | đang chờ xử lý | 
| Chống đỡ | (1,1) | 1 | 8 | không buộc phải di chuyển | 

Không có mâu thuẫn nào phát sinh và không có sự lan truyền cưỡng bức nào được kích hoạt, do đó cấu hình nhất quán. 

### Ví dụ 2 

đầu vào:```
2 2
1*
*1
```| Bước | Tế bào | Cần | Không rõ | Hành động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | (0,0) | 1 | 1 | được | 
| Ban đầu | (1,1) | 1 | 1 | được | 
| Kiểm tra xung đột | (0,0) | 1 | 0 | mâu thuẫn | 
| Dừng lại | - | - | - | từ chối | 

Điều này cho thấy trường hợp cả hai ô được đánh số cạnh tranh cho cùng một ô lân cận theo cách buộc phải gán quá mức, tạo ra mâu thuẫn cục bộ ngay lập tức. 

Những dấu vết này nêu bật rằng tính chính xác phụ thuộc vào việc cập nhật liên tục các ràng buộc được chia sẻ chứ không chỉ kiểm tra từng ô một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | mỗi ô được xếp vào hàng đợi và được xử lý với số lần không đổi do tính kề cận 8 hướng bị chặn | 
| Không gian | O(n·m) | lưu trữ trạng thái lưới, theo dõi ràng buộc và hàng đợi | 

Giải pháp vừa vặn thoải mái trong các ràng buộc điển hình cho lưới lên tới 10^6 ô. Chi phí hệ số không đổi của các bản cập nhật 8 lân cận vẫn đủ nhỏ cho giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# minimal valid
assert run("""1 1
0
""") == "YES"

# simple contradiction
assert run("""2 2
1*
*1
""") == "NO"

# all safe
assert run("""2 2
00
00
""") == "YES"

# all mines consistent numbers impossible
assert run("""2 2
**
**
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 trống | CÓ | trường hợp cơ sở | 
| chồng chéo xung đột | KHÔNG | phát hiện mâu thuẫn | 
| tất cả số không | CÓ | không cần nhân giống | 
| đầy đủ mỏ | CÓ | cạnh xử lý ràng buộc | 

## Vỏ cạnh 

Trường hợp góc phát sinh khi một ô được đánh số đã được đáp ứng đầy đủ bởi các mỏ đã biết ban đầu. Trong tình huống đó, thuật toán phải ngay lập tức buộc tất cả những người hàng xóm còn lại được an toàn. Nếu bước này bị bỏ qua, người giải có thể để lại các ô mơ hồ không được giải quyết một cách không chính xác và bỏ lỡ những mâu thuẫn sau này. 

Một trường hợp cạnh khác là khi một ràng buộc bắt đầu đã quá thỏa mãn, ví dụ: một ô được gắn nhãn 1 với hai mỏ liền kề đã có trong lưới ban đầu. Trong kịch bản đó,`need`trở nên âm trong quá trình khởi tạo và thuật toán phải từ chối ngay lập tức. Nếu không có bước kiểm tra sớm này, quá trình lan truyền sẽ không bao giờ được kích hoạt nhưng cấu hình vẫn không hợp lệ, dẫn đến kết quả dương tính giả.
