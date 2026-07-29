---
title: "CF 102757F - Thiết kế mê cung"
description: "Mê cung là một hành lang có đúng ba hàng và n cột. Mỗi ô đều mở hoặc bị chặn. Alice có thể bắt đầu từ bất kỳ ô mở nào trong cột đầu tiên và có thể di chuyển lên, xuống, sang trái hoặc phải qua các ô đang mở."
date: "2026-07-29T00:26:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102757
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 2"
rating: 0
weight: 102757
solve_time_s: 58
verified: true
draft: false
---

[CF 102757F - Thiết kế mê cung](https://codeforces.com/problemset/problem/102757/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mê cung là một hành lang có đúng ba hàng và`n`cột. Mỗi ô đều mở hoặc bị chặn. Alice có thể bắt đầu từ bất kỳ ô mở nào trong cột đầu tiên và có thể di chuyển lên, xuống, sang trái hoặc phải qua các ô đang mở. Mục tiêu là xác định xem có ít nhất một đường dẫn có thể đến bất kỳ ô mở nào ở cột cuối cùng hay không. Đầu vào chứa chiều rộng của hành lang và ba chuỗi nhị phân mô tả các hàng, trong đó`0`đại diện cho một vị trí có thể đi bộ và`1`đại diện cho một bức tường. Đầu ra là`Solvable!`nếu cột cuối cùng có thể đạt được và`Unsolvable!`nếu không thì. 

Chiều rộng có thể đạt tới`10^4`, vậy mê cung chỉ chứa khoảng`3 * 10^4`tế bào. Một giải pháp kiểm tra từng tế bào với số lần không đổi là đủ nhanh. Việc tìm kiếm thô bạo trên các đường đi có thể là không thể vì số lần đi bộ có thể tăng theo cấp số nhân khi mê cung trở nên lớn hơn. Ràng buộc hướng mạnh vào việc truyền tải đồ thị trong đó mỗi ô được xử lý một lần. 

Những trường hợp khó khăn đều xuất phát từ việc điểm xuất phát không cố định. Một lỗi phổ biến là chỉ bắt đầu từ ô trên cùng bên trái, nhưng Alice có thể chọn bất kỳ hàng nào trong cột đầu tiên. Ví dụ:```
2
10
00
10
```Đầu ra đúng là:```
Solvable!
```Việc tìm kiếm chỉ bắt đầu ở hàng 0 sẽ thất bại ngay lập tức vì ô trên cùng bên trái bị chặn, mặc dù ô ở giữa bên trái có thể tới cột thứ hai. 

Một trường hợp cạnh khác là khi các ô gần đích đang mở nhưng bị ngắt kết nối từ phía bên trái. Ví dụ:```
3
011
010
010
```Đầu ra đúng là:```
Unsolvable!
```Cột cuối cùng chứa các ô đang mở nhưng không có đường dẫn từ cột đầu tiên đến các ô đó. Một cách tiếp cận bất cẩn chỉ kiểm tra xem cột cuối cùng có chứa`0`sẽ đưa ra câu trả lời sai. 

Trường hợp ranh giới cuối cùng là một mê cung hoàn toàn mở:```
2
00
00
00
```Đầu ra đúng là:```
Solvable!
```Đường dẫn có thể đơn giản di chuyển thẳng qua bất kỳ hàng nào. Bất kỳ quá trình triển khai nào vô tình yêu cầu di chuyển qua cả ba hàng hoặc cố gắng tìm một đường dẫn duy nhất sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là coi mọi ô mở là một đỉnh của đồ thị. Hai đỉnh được kết nối khi các ô của chúng có chung một cạnh. Giải pháp brute-force có thể liệt kê các đường dẫn có thể có từ cột đầu tiên và dừng lại khi đến cột cuối cùng. Điều này đúng vì mọi chuỗi chuyển động hợp lệ đều tương ứng với một đường dẫn trong biểu đồ. Vấn đề là số lượng đường dẫn có thể. Một mê cung có nhiều điểm phân nhánh có thể tạo ra số lượng lựa chọn theo cấp số nhân, do đó việc khám phá các con đường riêng lẻ nhanh chóng trở nên không khả thi. 

Quan sát hữu ích là câu hỏi chỉ hỏi xem đường dẫn có tồn tại hay không. Chúng ta không cần bản thân đường dẫn, độ dài của nó hoặc số lượng đường dẫn. Khi một ô có thể truy cập được thì cách chính xác để truy cập ô đó không còn quan trọng nữa. Điều này có nghĩa là chúng ta có thể đánh dấu các ô là đã truy cập và mở rộng ra ngoài từ tất cả các vị trí bắt đầu có thể cùng một lúc. 

Tìm kiếm theo chiều rộng hoặc tìm kiếm theo chiều sâu là đủ vì mỗi ô mở chỉ được xem xét tối đa một lần. Hình dạng ba hàng không yêu cầu bất kỳ thuật toán mê cung đặc biệt nào. Nó chỉ làm giảm số lượng hàng xóm có thể có mà mỗi ô có thể có. Đồ thị có số đỉnh và cạnh tuyến tính, do đó, đường truyền tiêu chuẩn kết thúc trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo số lượng nhánh | O(n) | Quá chậm | 
| Truyền tải đồ thị | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo hàng đợi chứa mọi ô đang mở trong cột đầu tiên. Đây đều là những vị trí bắt đầu hợp lệ vì Alice có thể chọn bất kỳ hàng nào. 
2. Liên tục loại bỏ một ô khỏi hàng đợi và kiểm tra bốn ô lân cận có thể có của nó. Nếu một người hàng xóm ở trong mê cung, đang mở và chưa ghé thăm, hãy đánh dấu người đó đã ghé thăm và thêm nó vào hàng đợi. Mảng đã truy cập ngăn chặn công việc lặp đi lặp lại và chu kỳ di chuyển vô hạn. 
3. Trong khi xử lý các ô, hãy kiểm tra xem có ô nào được truy cập có thuộc cột cuối cùng hay không. Đến bất kỳ hàng nào của cột đó nghĩa là mê cung có lời giải. 
4. Nếu hàng đợi trống mà chưa đến cột cuối cùng, hãy báo cáo`Unsolvable!`. Mọi vị trí có thể tiếp cận đều đã được khám phá nên không thể tồn tại đường dẫn bị thiếu. 

Tại sao nó hoạt động: Việc truyền tải duy trì tính bất biến là mọi ô được đánh dấu đã truy cập đều có thể truy cập được từ một số ô bắt đầu trong cột đầu tiên. Ban đầu điều này đúng vì chỉ những ô bắt đầu hợp lệ mới được chèn vào. Bất cứ khi nào một ô mới được thêm vào, nó sẽ đạt được thông qua một chuyển động hợp lệ từ một ô đã có thể truy cập được, do đó, bất biến vẫn đúng. Việc tìm kiếm tiếp tục cho đến khi hết tất cả các ô có thể truy cập. Nếu cột cuối cùng được truy cập, đường dẫn được ghi lại chứng tỏ mê cung có thể giải được. Nếu nó chưa bao giờ được truy cập, tất cả các vị trí có thể tiếp cận được đã được kiểm tra, chứng tỏ rằng không tồn tại đường dẫn hợp lệ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n = int(input())
    maze = [input().strip() for _ in range(3)]

    visited = [[False] * n for _ in range(3)]
    q = deque()

    for r in range(3):
        if maze[r][0] == '0':
            visited[r][0] = True
            q.append((r, 0))

    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    while q:
        r, c = q.popleft()

        if c == n - 1:
            print("Solvable!")
            return

        for dr, dc in directions:
            nr = r + dr
            nc = c + dc

            if 0 <= nr < 3 and 0 <= nc < n:
                if not visited[nr][nc] and maze[nr][nc] == '0':
                    visited[nr][nc] = True
                    q.append((nr, nc))

    print("Unsolvable!")

if __name__ == "__main__":
    solve()
```Đầu vào được đọc dưới dạng ba chuỗi vì chiều cao mê cung được cố định. Mảng đã truy cập có cùng kích thước với mê cung và ghi lại xem một vị trí đã được phát hiện chưa. 

Bước khởi tạo sẽ chèn cả ba ô có thể có vào cột đầu tiên. Đây là lỗi triển khai phổ biến nhất trong vấn đề này, vì giả sử chỉ có một vị trí bắt đầu sẽ thay đổi vấn đề. 

Hàng đợi lưu trữ các cặp hàng và cột. Mỗi khi một ô bị xóa, mã sẽ kiểm tra xem nó có ở cột cuối cùng hay không trước khi khám phá các ô lân cận. Việc kiểm tra ranh giới ngăn chặn việc lập chỉ mục bên ngoài ba hàng hoặc`n`cột. 

Mỗi hàng xóm hợp lệ được đánh dấu trước khi chèn thay vì sau khi xóa. Điều này tránh việc thêm cùng một ô nhiều lần khi có hai đường dẫn khác nhau đến nó. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
00
10
00
```Trạng thái BFS thay đổi như sau. 

| Bước | Xếp hàng sau khi xử lý | Các ô đã truy cập | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | (0,0), (1,0), (2,0) | Hàng cột đầu tiên 0,1,2 | Tìm kiếm bắt đầu | 
| Quá trình (0,0) | (1,0), (2,0), (0,1) | Đã thêm ô trên cùng bên phải | Tiếp tục | 
| Quy trình (0,1) | (2,0) | Đã đến cột cuối cùng | Tan! | 

Ví dụ này cho thấy tại sao việc bắt đầu từ mỗi hàng lại quan trọng. Chỉ riêng hàng trên cùng đã cung cấp tuyến đường hợp lệ. 

Đối với mẫu thứ hai:```
2
01
10
10
```| Bước | Xếp hàng sau khi xử lý | Các ô đã truy cập | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | (2,0) | Chỉ dưới cùng bên trái | Tìm kiếm bắt đầu | 
| Quy trình (2,0) | Trống | Không có hàng xóm mở mới | Dừng lại | 
| Kết thúc | Trống | Cột cuối cùng chưa bao giờ được truy cập | Không thể giải quyết được! | 

Trường hợp này chứng tỏ rằng một ô mở ở cột cuối cùng là không đủ. Nó phải được kết nối với phía bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | chỉ có`3n`các ô và mỗi ô được xử lý một lần. | 
| Không gian | O(n) | Mảng đã truy cập và hàng đợi lưu trữ ở hầu hết các ô trong mê cung. | 

Mê cung lớn nhất chứa khoảng ba mươi nghìn ô, do đó, việc truyền tải tuyến tính dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def solve():
    input = sys.stdin.readline

    n = int(input())
    maze = [input().strip() for _ in range(3)]

    visited = [[False] * n for _ in range(3)]
    q = deque()

    for r in range(3):
        if maze[r][0] == '0':
            visited[r][0] = True
            q.append((r, 0))

    for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
        pass

    while q:
        r, c = q.popleft()
        if c == n - 1:
            print("Solvable!")
            return

        for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < 3 and 0 <= nc < n:
                if not visited[nr][nc] and maze[nr][nc] == '0':
                    visited[nr][nc] = True
                    q.append((nr, nc))

    print("Unsolvable!")

assert run("""2
00
10
00
""") == "Solvable!\n"

assert run("""2
01
10
10
""") == "Unsolvable!\n"

assert run("""2
10
00
10
""") == "Solvable!\n"

assert run("""3
011
010
010
""") == "Unsolvable!\n"

assert run("""5
00000
00000
00000
""") == "Solvable!\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đầu tiên | Tan! | Con đường bình thường xuyên qua mê cung | 
| Mẫu thứ hai | Không thể giải quyết được! | Các ô đích bị ngắt kết nối | 
|`10/00/10`| Tan! | Có thể có nhiều hàng bắt đầu | 
|`011/010/010`| Không thể giải quyết được! | Mở các ô không thể truy cập được | 
| Mê cung 3 x 5 mở hoàn toàn | Tan! | Hộp kết nối lớn đơn giản | 

## Vỏ cạnh 

Đối với trường hợp khởi động nhiều lần:```
2
10
00
10
```Thuật toán bỏ qua các ô bị chặn ở hàng đầu tiên và hàng thứ ba, sau đó chỉ chèn ô ở giữa bên trái vào hàng đợi. Từ`(1,0)`nó di chuyển đến`(1,1)`, nằm ở cột cuối cùng, vì vậy câu trả lời là`Solvable!`. 

Đối với trường hợp đích bị ngắt kết nối:```
3
011
010
010
```Ô bắt đầu duy nhất là`(1,0)`. Việc truyền tải không thể di chuyển đi bất cứ nơi nào khác vì mọi tuyến đường có thể đều bị chặn. Các ô ở cột cuối cùng không bao giờ được đánh dấu là đã truy cập, do đó thuật toán sẽ in chính xác`Unsolvable!`. 

Đối với mê cung mở hoàn toàn:```
2
00
00
00
```Cả ba ô ở cột đầu tiên đều vào hàng đợi. Việc tìm kiếm ngay lập tức mở rộng khắp hành lang và đến cột`1`. Vì đạt tới bất kỳ hàng nào của cột cuối cùng là đủ nên thuật toán trả về`Solvable!`. 

Bài xã luận có thể được điều chỉnh thêm nếu bạn muốn một lời giải thích ngắn gọn hơn theo phong cách cuộc thi hoặc một phiên bản mang phong cách hướng dẫn hơn dành cho người mới bắt đầu.
