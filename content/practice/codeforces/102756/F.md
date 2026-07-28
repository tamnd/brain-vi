---
title: "CF 102756F - Thiết kế mê cung"
description: "Mê cung là một hành lang có đúng ba hàng và n cột. Mỗi ký tự mô tả một ô. Ô 0 là nơi Alice có thể đứng, trong khi ô 1 bị chặn."
date: "2026-07-29T00:37:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102756
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 1"
rating: 0
weight: 102756
solve_time_s: 46
verified: true
draft: false
---

[CF 102756F - Thiết kế mê cung](https://codeforces.com/problemset/problem/102756/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mê cung là một hành lang có đúng ba hàng và`n`cột. Mỗi ký tự mô tả một ô. MỘT`0`ngói là nơi Alice có thể đứng, trong khi`1`gạch bị chặn. Alice có thể bắt đầu trên bất kỳ ô mở nào ở cột đầu tiên và muốn đến bất kỳ ô mở nào ở cột cuối cùng bằng cách chỉ di chuyển lên, xuống, sang trái hoặc phải. Nhiệm vụ là quyết định liệu một con đường như vậy có tồn tại hay không. 

Kích thước đầu vào là đầu mối chính. Chiều rộng có thể đạt tới`10^4`, nhưng chiều cao chỉ cố định ở ba hàng. Một thuật toán lưới chung khám phá mọi ô có thể truy cập là đủ vì chỉ có`3 * n`các vị trí. Bất kỳ cách tiếp cận nào cố gắng liệt kê các đường dẫn có thể là không thể, vì số lượng đường dẫn tăng theo cấp số nhân với số lượng cột. 

Chiều cao nhỏ là cấu trúc ẩn. Một giải pháp bất cẩn có thể coi đây như một mạng lưới lớn bình thường và tăng thêm độ phức tạp không cần thiết, nhưng mê cung chỉ có ba lớp. Một lỗi phổ biến khác là chỉ kiểm tra xem mỗi cột có ô mở hay không. Điều đó là chưa đủ vì khoảng cách giữa các hàng có thể ngắt kết nối đường dẫn. 

Ví dụ:```
2
00
10
00
```Câu trả lời là:```
Solvable!
```Đường dẫn có thể bắt đầu ở trên cùng bên trái, di chuyển sang phải, di chuyển xuống và đến cột cuối cùng. Kiểm tra từng cột hoạt động ở đây nhưng không chứng minh được khả năng kết nối. 

Một trường hợp khác là:```
2
01
10
10
```Câu trả lời là:```
Unsolvable!
```Cột cuối cùng chứa ô mở và cột đầu tiên chứa ô mở nhưng chúng được phân tách bằng ô bị chặn. Giải pháp chỉ kiểm tra sự tồn tại của các ô mở ở cả hai đầu sẽ chấp nhận nó một cách không chính xác. 

Cách an toàn nhất để giải quyết vấn đề là xem các ô mở dưới dạng các đỉnh của đồ thị. Vì đồ thị chỉ có`3n`các đỉnh, việc tìm kiếm thành phần có thể truy cập dễ dàng và đủ nhanh. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chạy biểu đồ duyệt qua mọi ô bắt đầu có thể có trong cột đầu tiên. Mỗi ô mở là một nút và các cạnh kết nối các ô mở lân cận. Tìm kiếm đầu tiên theo chiều rộng hoặc tìm kiếm đầu tiên theo chiều sâu sẽ truy cập vào mọi ô có thể truy cập và cho chúng tôi biết liệu có thể truy cập được cột cuối cùng hay không. Điều này đúng vì việc truyền tải đồ thị khám phá chính xác thành phần được kết nối có chứa nút bắt đầu. 

Tuy nhiên, việc bắt đầu một quá trình duyệt riêng biệt từ mỗi ô cột đầu tiên có thể lặp lại sẽ hoạt động. Trong trường hợp xấu nhất có ba vị trí bắt đầu, vì vậy việc lặp lại ở đây thực sự không có hại nhưng không cần thiết. Quan trọng hơn, vấn đề trở nên rõ ràng hơn nhiều khi chúng ta nhận ra rằng tất cả các lần khởi động hợp lệ đều được kết nối thông qua cùng một biểu đồ. Chúng tôi có thể thêm tất cả các ô mở cột đầu tiên vào một tìm kiếm ban đầu và khám phá toàn bộ khu vực có thể truy cập cùng một lúc. 

Nhận xét rằng chiều cao mê cung là cố định cho phép chúng ta mô hình hóa toàn bộ đoạn văn dưới dạng một biểu đồ nhỏ với`O(n)`nút. Một BFS hoặc DFS duy nhất xử lý mỗi ô nhiều nhất một lần, đưa ra giải pháp tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Được chấp nhận nhưng tìm kiếm lặp đi lặp lại không cần thiết | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba hàng mê cung và tạo một mảng đã thăm cho ba hàng đó bằng cách`n`lưới. Kích thước lưới là cố định, vì vậy biểu diễn này lưu trữ trực tiếp mọi vị trí có thể. 
2. Đặt mọi ô đang mở ở cột đầu tiên vào hàng đợi BFS và đánh dấu ô đó đã truy cập. Alice được phép bắt đầu từ bất kỳ vị trí nào trong số này, vì vậy việc tìm kiếm sẽ bắt đầu từ tất cả các vị trí đó cùng một lúc. 
3. Liên tục xóa một vị trí khỏi hàng đợi và kiểm tra bốn vị trí lân cận của nó. Nếu một người hàng xóm ở trong mê cung, đang mở và chưa được ghé thăm, hãy đánh dấu người đó đã ghé thăm và thêm nó vào hàng đợi. 
4. Trong quá trình tìm kiếm, hãy kiểm tra xem vị trí đã truy cập có nằm ở cột cuối cùng hay không. Nếu tồn tại thì mê cung có lộ trình hợp lệ từ đầu đến cuối. 
5. Nếu hàng đợi trống mà không đến cột cuối cùng, mọi vị trí có thể tiếp cận đều đã được khám phá và không có giải pháp nào tồn tại. 

Tại sao nó hoạt động: tính bất biến được BFS duy trì là mọi ô đã truy cập đều có thể truy cập được từ một số ô bắt đầu hợp lệ trong cột đầu tiên. Khi BFS kết thúc, mọi ô kết nối với vùng bắt đầu đều đã được truy cập. Nếu cột cuối cùng chứa ngăn xếp đã truy cập thì có đường dẫn hợp lệ. Nếu không, không có con đường nào có thể tồn tại vì mọi chuyển động có thể có từ vùng xuất phát đều đã được khám phá. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n = int(input())
    grid = [input().strip() for _ in range(3)]

    visited = [[False] * n for _ in range(3)]
    q = deque()

    for r in range(3):
        if grid[r][0] == '0':
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
                if not visited[nr][nc] and grid[nr][nc] == '0':
                    visited[nr][nc] = True
                    q.append((nr, nc))

    print("Unsolvable!")

if __name__ == "__main__":
    solve()
```Mã khởi động BFS từ tất cả các ô nhập có thể thay vì chọn một điểm bắt đầu tùy ý. Điều này phù hợp với định nghĩa của bài toán vì Alice có thể bắt đầu ở bất kỳ đâu trong cột đầu tiên. 

Việc kiểm tra ranh giới ngăn chặn việc truy cập các hàng bên ngoài`0`ĐẾN`2`hoặc cột bên ngoài`0`ĐẾN`n - 1`. Việc đánh dấu một ô đã ghé thăm trước khi thêm nó vào hàng đợi sẽ ngăn các mục nhập hàng đợi trùng lặp và đảm bảo mỗi ô được xử lý một lần. 

Về sớm khi tới cột`n - 1`là an toàn vì đạt được bất kỳ ô nào ở cột cuối cùng là đủ để giải mê cung. Không cần phải tiếp tục khám phá sau khi thành công đã được chứng minh. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
2
00
10
00
```Trạng thái BFS phát triển như sau. 

| Bước | Xếp hàng trước khi xử lý | Ngói hiện tại | Gạch mới ghé thăm | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0), (2,0) | (0,0) | (0,1) | Tiếp tục | 
| 2 | (2,0), (0,1) | (2,0) | (2,1) | Tiếp tục | 
| 3 | (0,1), (2,1) | (0,1) | Không có | Tiếp tục | 
| 4 | (2,1) | (2,1) | Đã đạt đến cột cuối cùng | Tan! | 

Dấu vết cho thấy BFS không cần phải đi theo một lộ trình đã được lên kế hoạch duy nhất. Nó khám phá mọi con đường có thể cho đến khi tìm thấy một con đường thành công. 

Đối với ví dụ thứ hai:```
2
01
10
10
```| Bước | Xếp hàng trước khi xử lý | Ngói hiện tại | Gạch mới ghé thăm | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | (0,0) | Không có | Tiếp tục | 
| 2 | Trống | Không có | Không có | Không có cột cuối cùng đạt được | 

Ô bắt đầu duy nhất bị chặn tiếp cận cột thứ hai, do đó tìm kiếm sẽ loại bỏ mê cung một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | chỉ có`3n`các ô và mỗi ô được truy cập tối đa một lần | 
| Không gian | O(n) | Mảng đã truy cập và hàng đợi BFS lưu trữ ở hầu hết tất cả các ô | 

Chiều cao cố định giữ cho đồ thị nhỏ. Quét tuyến tính trên mê cung nằm trong giới hạn thoải mái cho`n = 10^4`. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solve_input(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    grid = [input().strip() for _ in range(3)]

    visited = [[False] * n for _ in range(3)]
    q = deque()

    for r in range(3):
        if grid[r][0] == '0':
            visited[r][0] = True
            q.append((r, 0))

    while q:
        r, c = q.popleft()
        if c == n - 1:
            return "Solvable!\n"

        for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < 3 and 0 <= nc < n:
                if not visited[nr][nc] and grid[nr][nc] == '0':
                    visited[nr][nc] = True
                    q.append((nr, nc))

    return "Unsolvable!\n"

assert solve_input("""2
00
10
00
""") == "Solvable!\n", "sample 1"

assert solve_input("""2
01
10
10
""") == "Unsolvable!\n", "sample 2"

assert solve_input("""2
00
00
00
""") == "Solvable!\n", "all open"

assert solve_input("""2
11
00
11
""") == "Unsolvable!\n", "blocked entry"

assert solve_input("""10000
""" + "0" * 10000 + "\n" + "1" * 10000 + "\n" + "0" * 10000 + "\n") == "Solvable!\n", "maximum width"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Tan! | Một tuyến đường thay đổi hàng | 
| Mẫu 2 | Không thể giải quyết được! | Cột cuối cùng bị ngắt kết nối | 
| Tất cả mê cung mở | Tan! | Trường hợp kết nối đầy đủ đơn giản | 
| Mục nhập bị chặn | Không thể giải quyết được! | Không có vị trí bắt đầu | 
| Chiều rộng trường hợp 10000 | Tan! | Hiệu suất tuyến tính ở kích thước tối đa | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là khi tồn tại nhiều vị trí bắt đầu. TRONG:```
2
00
00
00
```BFS bắt đầu bằng cả ba ô trong cột đầu tiên. Thuật toán không giả sử Alice bắt đầu trên một hàng cụ thể, do đó mọi lối vào có thể đều được xử lý. 

Trường hợp thứ hai là khi cột cuối cùng có các ô mở nhưng không thể truy cập được:```
2
01
10
10
```Việc tìm kiếm bắt đầu lúc`(0,0)`. Hàng xóm của nó ở ngoài lưới hoặc bị chặn, do đó hàng đợi trở nên trống. Vì không có ô cột cuối cùng nào được truy cập nên thuật toán xuất ra`Unsolvable!`. 

Trường hợp thứ ba là một mê cung dài và hẹp:```
10000
00000000000000000000...000
11111111111111111111...111
00000000000000000000...000
```với`10000`cột. BFS vẫn truy cập từng ô có thể truy cập một lần. Nó không phụ thuộc vào độ sâu đệ quy hoặc số lượng đường dẫn có thể, do đó nó xử lý độ rộng tối đa một cách an toàn.
