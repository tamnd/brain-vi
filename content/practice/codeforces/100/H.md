---
title: "CF 100H - Thiết Giáp Hạm"
description: "Chúng tôi được cấp một số bảng Chiến hạm 10 × 10. Mỗi ô trống hoặc bị chiếm bởi một phần của con tàu. Nhiệm vụ là xác minh xem mọi ô bị chiếm có thuộc về cấu hình nhóm hợp lệ hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dfs-and-similar", "implementation"]
categories: ["algorithms"]
codeforces_contest: 100
codeforces_index: "H"
codeforces_contest_name: "Unknown Language Round 3"
rating: 2100
weight: 100
solve_time_s: 184
verified: true
draft: false
---

[CF 100H - Thiết giáp hạm](https://codeforces.com/problemset/problem/100/H) 

**Xếp hạng:** 2100 
**Thẻ:** *đặc biệt, dfs và tương tự, triển khai 
**Thời gian giải:** 3m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số bảng Chiến hạm 10 × 10. Mỗi ô trống hoặc bị chiếm bởi một phần của con tàu. Nhiệm vụ là xác minh xem mọi ô bị chiếm có thuộc về cấu hình nhóm hợp lệ hay không. 

Một hạm đội Chiến hạm hợp lệ có chính xác một tàu có chiều dài 4, hai tàu có chiều dài 3, ba tàu có chiều dài 2 và bốn tàu có chiều dài 1. Mỗi tàu phải là một đoạn thẳng nằm ngang hoặc thẳng đứng. Các con tàu không thể chồng lên nhau, không thể uốn cong và không thể chạm vào nhau, thậm chí là theo đường chéo. 

Bảng rất nhỏ, chỉ có 100 ô. Ngay cả với tối đa 10 bảng, chúng tôi chỉ xử lý tổng cộng tối đa 1000 ô. Điều này ngay lập tức loại trừ hiệu suất là thách thức chính. Khó khăn hoàn toàn nằm ở việc xác nhận chính xác tất cả các điều kiện hình học. 

Những sai lầm nguy hiểm nhất đến từ việc chấp nhận hình dạng tàu không hợp lệ hoặc quên kiểm tra các đường chéo kề nhau. 

Hãy xem xét mảnh bảng này:```
**00000000
*000000000
0000000000
```Ba ô bị chiếm giữ tạo thành hình chữ L. Một DFS bất cẩn chỉ đếm các thành phần được kết nối sẽ thấy thành phần có kích thước 3 và có thể phân loại không chính xác thành tàu hợp lệ có chiều dài 3. Câu trả lời đúng là KHÔNG vì tàu phải thẳng. 

Một cạm bẫy phổ biến khác là chạm chéo:```
*000000000
0*00000000
0000000000
```Đây là hai tàu đơn bào chạm chéo nhau. Quy tắc của tàu chiến cấm điều này, vì vậy bảng không hợp lệ. Giải pháp chỉ kiểm tra liền kề 4 hướng sẽ bỏ sót điều này. 

Trường hợp tinh vi thứ ba là sáp nhập tàu:```
***0000000
00*0000000
0000000000
```Ô dọc tiếp xúc trực giao với tàu ngang, tạo thành bộ phận bị uốn cong. Hội đồng quản trị phải bị từ chối ngay lập tức. 

Loại lỗi cuối cùng xuất phát từ việc đếm tàu. Ngay cả khi mọi thành phần được kết nối là một đường thẳng thì thành phần nhóm phải khớp chính xác với số lượng yêu cầu. Ví dụ: có hai tàu có chiều dài 4 và không có tàu có chiều dài 3 là không hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi có thể cố gắng xác định mọi vị trí tàu có thể một cách độc lập và loại bỏ các tàu phù hợp khỏi bảng. Vì bảng chỉ có kích thước 10 × 10 nên ngay cả việc liệt kê khá vụng về vẫn sẽ chạy nhanh chóng. 

Một chiến lược ngây thơ là liên tục quét bảng để tìm các đoạn ngang hoặc dọc, đánh dấu chúng là tàu và hy vọng các ô còn lại cũng tạo thành tàu hợp lệ. Vấn đề là sự mơ hồ. Một hình dạng không đúng định dạng có thể vô tình bị phân hủy thành nhiều hình dạng hợp pháp. Ví dụ, hình chữ L có thể được hiểu là một con tàu có chiều dài 2 cộng với một con tàu một ô, mặc dù bản thân thành phần đó không hợp lệ. 

Cách đúng đắn để suy nghĩ về bảng là giải bài toán bằng đồ thị. Mỗi ô bị chiếm dụng thuộc về chính xác một thành phần được kết nối trong vùng lân cận 4 hướng. Vì các tàu không thể chạm trực giao nên mỗi thành phần được kết nối phải tương ứng với chính xác một tàu. 

Quan sát này đơn giản hóa mọi thứ: 

1. Chạy DFS hoặc BFS trên mọi ô bị chiếm dụng chưa được truy cập. 
2. Trích xuất toàn bộ thành phần được kết nối. 
3. Xác minh rằng tất cả các ô đều nằm trên một hàng hoặc một cột. 
4. Xác minh rằng các ô tạo thành một đoạn liên tục. 
5. Xác minh rằng không có ô nào chạm vào tàu khác theo đường chéo. 
6. Đếm chiều dài con tàu. 

Vì bo mạch chỉ có 100 ô nên DFS trên toàn bộ bo mạch thực tế là thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100³) | O(100) | Chấp nhận nhưng lúng túng | 
| Tối ưu | O(100) | O(100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bảng 10 × 10 thành lưới. 
2. Duy trì một mảng đã truy cập để mỗi ô bị chiếm được xử lý chính xác một lần. 
3. Lặp lại qua từng ô của bảng. 
4. Khi một người không ghé thăm`'*'`tìm thấy ô, hãy khởi động DFS hoặc BFS từ ô đó để thu thập toàn bộ thành phần được kết nối của nó chỉ bằng chuyển động 4 hướng. 

Dùng 4 hướng là đúng vì tàu chỉ kết nối theo chiều ngang hoặc chiều dọc. Các ô chéo không bao giờ được thuộc cùng một tàu. 
5. Đối với mỗi ô bên trong thành phần, hãy kiểm tra tất cả bốn ô lân cận theo đường chéo. 

Nếu bất kỳ hàng xóm đường chéo nào chứa`'*'`, bảng không hợp lệ ngay lập tức vì tàu không được phép chạm chéo. 
6. Sau khi thu thập thành phần, hãy xác nhận hình dạng của nó. 

Nếu thành phần có kích thước 1 thì nó tự động là tàu đơn bào hợp lệ. 

Nếu không thì: 

- tất cả các ô phải chia sẻ cùng một hàng, hoặc 
- tất cả các ô phải có chung một cột. 

Nếu không có điều kiện nào xảy ra, con tàu bị uốn cong và tấm ván không còn hiệu lực. 
7. Kiểm tra tính liên tục. 

Đối với tàu nằm ngang, sắp xếp chỉ số cột và xác minh chúng tạo thành các số nguyên liên tiếp. 

Đối với tàu thẳng đứng, hãy sắp xếp các chỉ số hàng và xác minh chúng tạo thành các số nguyên liên tiếp. 

Điều này ngăn chặn các hình dạng bị ngắt kết nối như:```
*0*
```8. Ghi lại chiều dài tàu. 
9. Sau khi xử lý tất cả các thành phần, hãy xác minh thành phần chính xác của đội xe: 

- một tàu cỡ 4, 
- hai tàu cỡ 3, 
- ba tàu cỡ 2, 
- 4 tàu cỡ 1. 
10. In CÓ nếu tất cả kiểm tra thành công, nếu không in ra KHÔNG. 

### Tại sao nó hoạt động 

Mỗi ô bị chiếm dụng thuộc về chính xác một thành phần được kết nối trong vùng lân cận 4 hướng. Vì các tàu không thể chồng lên nhau hoặc chạm trực giao nên mỗi thành phần phải đại diện cho một tàu duy nhất. 

Thuật toán xác nhận mọi thuộc tính cần thiết của một con tàu hợp pháp: 

- kiểm tra đường chéo ngăn chặn tàu chạm vào, 
- căn chỉnh hàng/cột ngăn ngừa uốn cong, 
- kiểm tra tính liên tục ngăn chặn những khoảng trống, 
- việc đếm đội tàu thực thi việc kiểm kê tàu chính xác theo yêu cầu. 

Vì mọi thành phần đều được kiểm tra độc lập và mỗi ô bị chiếm dụng đều được xử lý chính xác một lần nên không có cấu hình không hợp lệ nào có thể thoát khỏi sự phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

DIRS = [(-1, 0), (1, 0), (0, -1), (0, 1)]
DIAG = [(-1, -1), (-1, 1), (1, -1), (1, 1)]

def solve_board(grid):
    visited = [[False] * 10 for _ in range(10)]
    counts = [0] * 5

    def inside(r, c):
        return 0 <= r < 10 and 0 <= c < 10

    def dfs(sr, sc):
        stack = [(sr, sc)]
        visited[sr][sc] = True
        cells = []

        while stack:
            r, c = stack.pop()
            cells.append((r, c))

            for dr, dc in DIRS:
                nr, nc = r + dr, c + dc

                if inside(nr, nc) and not visited[nr][nc] and grid[nr][nc] == '*':
                    visited[nr][nc] = True
                    stack.append((nr, nc))

        return cells

    for r in range(10):
        for c in range(10):
            if grid[r][c] == '*' and not visited[r][c]:
                cells = dfs(r, c)

                # diagonal touching check
                for x, y in cells:
                    for dx, dy in DIAG:
                        nx, ny = x + dx, y + dy

                        if inside(nx, ny) and grid[nx][ny] == '*':
                            return False

                size = len(cells)

                if size > 4:
                    return False

                rows = set(x for x, _ in cells)
                cols = set(y for _, y in cells)

                if len(rows) != 1 and len(cols) != 1:
                    return False

                if len(rows) == 1:
                    vals = sorted(y for _, y in cells)
                else:
                    vals = sorted(x for x, _ in cells)

                for i in range(1, len(vals)):
                    if vals[i] != vals[i - 1] + 1:
                        return False

                counts[size] += 1

    return counts[1] == 4 and counts[2] == 3 and counts[3] == 2 and counts[4] == 1

def main():
    t = int(input())
    ans = []

    for _ in range(t):
        grid = []

        while len(grid) < 10:
            line = input().strip()

            if line:
                grid.append(line)

        ans.append("YES" if solve_board(grid) else "NO")

    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Giải pháp tách xác thực thành các kiểm tra độc lập, giúp việc gỡ lỗi dễ dàng hơn nhiều. 

DFS thu thập chính xác một thành phần được kết nối tại một thời điểm. Bởi vì chúng ta chỉ di chuyển theo bốn hướng nên các con tàu tiếp xúc theo đường chéo vẫn là những thành phần riêng biệt, đó chính xác là những gì chúng ta mong muốn. 

Việc xác thực đường chéo được thực hiện có chủ ý sau khi trích xuất thành phần. Tại thời điểm đó, chúng ta đã biết ô nào thuộc về tàu hiện tại, do đó, bất kỳ đường chéo nào`'*'`phải thuộc tàu khác và ngay lập tức vi phạm nội quy. 

Kiểm tra tập hợp hàng và cột là xác thực hình dạng cốt lõi. Nếu cả hai bộ có kích thước lớn hơn một thì bộ phận đó sẽ bị uốn cong. Điều này bắt được tất cả các hình chữ L và hình chữ T. 

Việc kiểm tra tính liên tục là tinh tế nhưng cần thiết. Một thành phần như:```
*0*
```không thể xuất hiện dưới kết nối 4 hướng hợp lệ, nhưng việc kiểm tra tính liên tục vẫn làm cho logic trở nên mạnh mẽ và khép kín. 

Việc xác minh số lượng hạm đội cuối cùng đảm bảo lượng tồn kho Chiến hạm chính xác. Ngay cả những con tàu có hình dáng hoàn hảo cũng bị từ chối nếu số lượng không phù hợp với quy định chính thức. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bảng đầu vào:```
****000000
0000000000
***00***00
0000000000
00000000**
000**00000
00000000**
000*000000
00000*00*0
0*00000000
```Dấu vết của tàu được phát hiện: 

| Tế bào thành phần | Hình dáng | Chiều dài | hợp lệ | 
| --- | --- | --- | --- | 
| (0,0)-(0,3) | Ngang | 4 | Có | 
| (2,0)-(2,2) | Ngang | 3 | Có | 
| (2,5)-(2,7) | Ngang | 3 | Có | 
| (4,8)-(4,9) | Ngang | 2 | Có | 
| (5,3)-(5,4) | Ngang | 2 | Có | 
| (6,8)-(6,9) | Ngang | 2 | Có | 
| (7,3) | Độc thân | 1 | Có | 
| (8,5) | Độc thân | 1 | Có | 
| (8,8) | Độc thân | 1 | Có | 
| (9,1) | Độc thân | 1 | Có | 

Tính toán cuối cùng: 

| Cỡ tàu | Bắt buộc | Tìm thấy | 
| --- | --- | --- | 
| 1 | 4 | 4 | 
| 2 | 3 | 3 | 
| 3 | 2 | 2 | 
| 4 | 1 | 1 | 

Bàn cờ đáp ứng tất cả các ràng buộc hình học và số lượng đội xe, vì vậy câu trả lời là CÓ. 

### Ví dụ không hợp lệ```
**00000000
*000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
```Dấu vết: 

| Tế bào thành phần | Hình dáng | Kết quả | 
| --- | --- | --- | 
| (0,0), (0,1), (1,0) | Hình chữ L | Không hợp lệ | 

Thành phần này trải dài trên nhiều hàng và nhiều cột, vì vậy nó không phải là một con tàu thẳng. Thuật toán ngay lập tức từ chối bảng. 

Ví dụ này chứng minh tại sao chỉ tính kích thước thành phần được kết nối là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100) | Mỗi ô được truy cập tối đa một lần | 
| Không gian | O(100) | Mảng đã truy cập và ngăn xếp DFS | 

Kích thước bảng được cố định ở mức 10 × 10, do đó thời gian chạy thực tế là không đổi. Ngay cả với 10 trường hợp thử nghiệm, tổng công việc vẫn rất nhỏ so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    DIRS = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    DIAG = [(-1, -1), (-1, 1), (1, -1), (1, 1)]

    def solve_board(grid):
        visited = [[False] * 10 for _ in range(10)]
        counts = [0] * 5

        def inside(r, c):
            return 0 <= r < 10 and 0 <= c < 10

        def dfs(sr, sc):
            stack = [(sr, sc)]
            visited[sr][sc] = True
            cells = []

            while stack:
                r, c = stack.pop()
                cells.append((r, c))

                for dr, dc in DIRS:
                    nr, nc = r + dr, c + dc

                    if inside(nr, nc) and not visited[nr][nc] and grid[nr][nc] == '*':
                        visited[nr][nc] = True
                        stack.append((nr, nc))

            return cells

        for r in range(10):
            for c in range(10):
                if grid[r][c] == '*' and not visited[r][c]:
                    cells = dfs(r, c)

                    for x, y in cells:
                        for dx, dy in DIAG:
                            nx, ny = x + dx, y + dy

                            if inside(nx, ny) and grid[nx][ny] == '*':
                                return False

                    size = len(cells)

                    if size > 4:
                        return False

                    rows = set(x for x, _ in cells)
                    cols = set(y for _, y in cells)

                    if len(rows) != 1 and len(cols) != 1:
                        return False

                    if len(rows) == 1:
                        vals = sorted(y for _, y in cells)
                    else:
                        vals = sorted(x for x, _ in cells)

                    for i in range(1, len(vals)):
                        if vals[i] != vals[i - 1] + 1:
                            return False

                    counts[size] += 1

        return counts[1] == 4 and counts[2] == 3 and counts[3] == 2 and counts[4] == 1

    t = int(input())
    out = []

    for _ in range(t):
        grid = []

        while len(grid) < 10:
            line = input().strip()

            if line:
                grid.append(line)

        out.append("YES" if solve_board(grid) else "NO")

    return "\n".join(out)

# provided samples
assert solve(
"""2
****000000
0000000000
***00***00
0000000000
00000000**
000**00000
00000000**
000*000000
00000*00*0
0*00000000

****000000
0000000000
***00***00
0000000000
00000000**
000**00000
00000000**
0000*00000
00000*00*0
0*00000000
"""
) == "YES\nNO"

# diagonal touching
assert solve(
"""1
*000000000
0*00000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
"""
) == "NO"

# bent ship
assert solve(
"""1
**00000000
*000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
"""
) == "NO"

# too many large ships
assert solve(
"""1
****0****0
0000000000
***00***00
0000000000
00000000**
000**00000
00000000**
000*000000
00000*00*0
0*00000000
"""
) == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | CÓ / KHÔNG | Tính đúng đắn cơ bản | 
| Chạm chéo | KHÔNG | Phát hiện sự kề cận đường chéo bị cấm | 
| Tàu uốn cong | KHÔNG | Loại bỏ các thành phần phi tuyến tính | 
| Tàu cỡ 4 | KHÔNG | Xác minh số lượng đội tàu chính xác | 

## Vỏ cạnh 

### Chạm chéo 

đầu vào:```
1
*000000000
0*00000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
```DFS tìm thấy hai thành phần ô đơn riêng biệt. Trong quá trình xác thực đường chéo, ô`(0,0)`phát hiện người khác`'*'`Tại`(1,1)`. Thuật toán ngay lập tức trả về KHÔNG. 

Trường hợp này xác nhận rằng các đường chéo lân cận được kiểm tra độc lập với việc xây dựng thành phần. 

### Con tàu bị cong 

đầu vào:```
1
**00000000
*000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
0000000000
```DFS thu thập thành phần:```
(0,0), (0,1), (1,0)
```Tập hợp các hàng là`{0,1}`và tập hợp các cột là`{0,1}`. Vì cả hai kích thước đều khác nhau nên thành phần này không phải là một đường thẳng. Thuật toán từ chối bảng. 

Điều này ngăn cản việc vô tình chấp nhận tàu hình chữ L. 

### Thành phần đội tàu không chính xác 

đầu vào:```
1
****0****0
0000000000
***00***00
0000000000
00000000**
000**00000
00000000**
000*000000
00000*00*0
0*00000000
```Mọi thành phần đều có giá trị riêng lẻ, nhưng số lượng cuối cùng sẽ trở thành: 

| Cỡ tàu | Tìm thấy | 
| --- | --- | 
| 1 | 4 | 
| 2 | 3 | 
| 3 | 2 | 
| 4 | 2 | 

Số lượng tàu có chiều dài 4 yêu cầu chính xác là một nên bảng không hợp lệ. 

Điều này khẳng định rằng hình học thôi là chưa đủ, kho hàng của đội xe cũng phải khớp chính xác.
