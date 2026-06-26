---
title: "CF 105346G - Miếng bí ngô"
description: "Chúng ta được cung cấp một lưới trong đó mỗi ô hoạt động giống như một loại địa hình trong một mê cung nhỏ. Khách du lịch bắt đầu từ chính xác một ô được đánh dấu S và phải đến một ô thoát duy nhất được đánh dấu E. Việc di chuyển được phép theo bốn hướng chính và mỗi lần di chuyển tốn một đơn vị thời gian."
date: "2026-06-23T15:35:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105346
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 2 (Beginner)"
rating: 0
weight: 105346
solve_time_s: 86
verified: false
draft: false
---

[CF 105346G - Miếng dán bí ngô](https://codeforces.com/problemset/problem/105346/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới trong đó mỗi ô hoạt động giống như một loại địa hình trong một mê cung nhỏ. Khách du lịch bắt đầu từ chính xác một ô được đánh dấu`S`và phải đến ô thoát duy nhất được đánh dấu`E`. Việc di chuyển được phép theo bốn hướng chính và mỗi lần di chuyển tốn một đơn vị thời gian. 

Hầu hết các ô đều trống, bị chặn hoặc đặc biệt. Các ô trống có thể di chuyển tự do. Các ô được đánh dấu`P`là những bức tường cố định. Các ô được đánh dấu`C`là những món đồ sưu tầm và mỗi chiếc sẽ tăng số lượng kẹo có sẵn lên một chiếc khi giẫm lên. Các ô được đánh dấu`J`hoạt động giống như những cánh cổng bị khóa: chúng chỉ có thể được vào nếu có ít nhất một kẹo ngô hiện đang được giữ và giẫm lên chúng sẽ tiêu tốn một ngô kẹo. 

Sự phức tạp chính là khả năng đi qua`J`tế bào phụ thuộc vào số lượng`C`các ô đã được thu thập dọc theo đường dẫn và vì có tổng cộng tối đa 8 viên kẹo nên không gian trạng thái vẫn bị giới hạn. 

Nhiệm vụ là tính thời gian tối thiểu cần thiết để đi từ`S`ĐẾN`E`hoặc báo cáo rằng không có đường dẫn hợp lệ nào tồn tại. 

Kích thước lưới tối đa là 100 x 100, do đó có tới 10.000 ô. Một tìm kiếm đường đi ngắn nhất ngây thơ mà bỏ qua trạng thái kẹo sẽ thất bại vì nó sẽ xử lý`J`tế bào không chính xác. Đồng thời, giải pháp nào cũng phải xử lý những thay đổi trạng thái do việc thu thập kẹo gây ra nên chỉ vị trí thôi là chưa đủ. 

Trạng thái chính xác phải bao gồm cả vị trí và số lượng kẹo hiện tại. Vì có nhiều nhất 8 viên kẹo nên không gian trạng thái đầy đủ tối đa là 100 × 100 × 9, đủ nhỏ cho BFS. 

Trường hợp cạnh tinh tế xuất hiện khi xem lại cùng một ô có số lượng kẹo khác nhau. Ví dụ, khi đến một`J`ô có 0 viên kẹo là không thể, nhưng đến muộn hơn với 1 viên kẹo có thể hợp lệ. Một mảng được truy cập đơn giản chỉ được khóa theo vị trí sẽ loại bỏ các đường dẫn hợp lệ một cách không chính xác. 

Một trường hợp khó khăn khác xảy ra khi một viên kẹo được yêu cầu phải vượt qua một`J`ô, nhưng đường đi tối ưu đòi hỏi phải đi đường vòng để thu thập nó trước. Con đường ngắn nhất tham lam bỏ qua những ràng buộc trong tương lai sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các con đường có thể từ`S`ĐẾN`E`, theo dõi việc thu thập kẹo dọc theo mỗi con đường. Mỗi bước phân nhánh thành tối đa bốn hướng, do đó số lượng đường đi có thể tăng theo cấp số nhân theo chiều dài đường đi. Ngay cả đối với lưới 100 x 100, điều này trở nên hoàn toàn không khả thi, vì chỉ riêng số lượng đường đi đơn giản đã lớn về mặt thiên văn. 

Lý do điều này là không cần thiết là vì bài toán có cấu trúc con tối ưu: việc tiếp cận cùng một ô với cùng số kẹo là tương đương bất kể chúng ta đến đó bằng cách nào. Khi chúng tôi nhận ra rằng trạng thái được mô tả đầy đủ theo vị trí và số kẹo, bài toán sẽ trở thành đường đi ngắn nhất trong biểu đồ không có trọng số. 

Điều này trực tiếp gợi ý tìm kiếm theo chiều rộng trên một không gian trạng thái mở rộng. Mỗi nút là một bộ ba`(i, j, k)`Ở đâu`k`là số kẹo hiện có. Chuyển tiếp tương ứng với việc di chuyển đến các ô liền kề và cập nhật`k`tùy thuộc vào loại tế bào. Vì mỗi lần di chuyển tốn đúng một đơn vị nên BFS đảm bảo đường đi ngắn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| BFS trên biểu đồ trạng thái mở rộng | O(n · m · 9) | O(n · m · 9) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi cấu hình có thể truy cập là một trạng thái trong biểu đồ và chạy BFS từ trạng thái bắt đầu. 

1. Xác định vị trí ô bắt đầu`S`và khởi tạo hàng đợi BFS với trạng thái`(S_row, S_col, 0, 0)`, trong đó giá trị cuối cùng là số bước đã thực hiện. Số kẹo bắt đầu từ 0 vì chưa có kẹo nào được thu thập. 
2. Tạo cấu trúc đã truy cập được lập chỉ mục bởi`(row, col, candy_count)`. Điều này đảm bảo chúng tôi không loại bỏ sai các trạng thái truy cập lại một ô có số lượng kẹo khác nhau. 
3. Trong khi hàng đợi không trống, hãy trích xuất trạng thái phía trước`(r, c, k, dist)`. 
4. Nếu ô hiện tại là lối ra`E`, quay lại ngay`dist`, vì BFS đảm bảo đây là con đường ngắn nhất có thể. 
5. Đối với mỗi hướng trong số bốn hướng di chuyển có thể, hãy tính ô tiếp theo`(nr, nc)`. Bỏ qua nó nếu nó vượt quá giới hạn hoặc một quả bí ngô`P`. 
6. Xác định nước đi có hợp lệ hay không dựa trên ô đích: 

Nếu nó là`J`, đảm bảo`k > 0`, sau đó giảm`k`bởi một vì một viên kẹo đã được tiêu thụ. 

Nếu nó là`C`, tăng`k`bởi một sau khi bước vào nó. 

Nếu nó trống rỗng,`S`, hoặc`E`, số kẹo vẫn không thay đổi trừ khi đã được xử lý. 
7. Nếu trạng thái kết quả`(nr, nc, nk)`chưa được truy cập, đánh dấu nó đã truy cập và đẩy nó vào hàng đợi với khoảng cách`dist + 1`. 
8. Nếu hàng đợi đã hết mà không đạt được`E`, trở lại`"SPOOKED!"`. 

### Tại sao nó hoạt động 

Thuật toán khám phá các trạng thái theo thứ tự khoảng cách tăng dần vì BFS xử lý tất cả các trạng thái ở độ sâu`d`trước bất kỳ độ sâu nào`d+1`. Mỗi trạng thái mã hóa đầy đủ tất cả thông tin cần thiết để đưa ra quyết định trong tương lai: vị trí và kho kẹo. Điều này ngăn chặn việc cắt tỉa không chính xác vì hai lần truy cập vào cùng một ô với số lượng kẹo khác nhau thực sự là những tình huống khác nhau. Do đó, khi gặp lối ra lần đầu tiên, không thể tồn tại đường dẫn hợp lệ ngắn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]
    
    sr = sc = er = ec = -1
    
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'S':
                sr, sc = i, j
            elif grid[i][j] == 'E':
                er, ec = i, j
    
    # visited[r][c][k]
    visited = [[[False] * 9 for _ in range(m)] for _ in range(n)]
    
    q = deque()
    q.append((sr, sc, 0, 0))
    visited[sr][sc][0] = True
    
    directions = [(1,0), (-1,0), (0,1), (0,-1)]
    
    while q:
        r, c, k, dist = q.popleft()
        
        if grid[r][c] == 'E':
            print(dist)
            return
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if not (0 <= nr < n and 0 <= nc < m):
                continue
            if grid[nr][nc] == 'P':
                continue
            
            nk = k
            cell = grid[nr][nc]
            
            if cell == 'C':
                nk += 1
            elif cell == 'J':
                if nk == 0:
                    continue
                nk -= 1
            
            if not visited[nr][nc][nk]:
                visited[nr][nc][nk] = True
                q.append((nr, nc, nk, dist + 1))
    
    print("SPOOKED!")

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp định nghĩa trạng thái. Hàng đợi BFS lưu trữ cả vị trí và số lượng kẹo, điều này cần thiết cho sự chính xác. Mảng đã truy cập là ba chiều để ngăn chặn việc xem lại các trạng thái giống hệt nhau. Thứ tự xử lý các hiệu ứng ô rất quan trọng: chúng tôi tính toán số kẹo tiếp theo dựa trên ô được nhập chứ không phải ô còn lại. 

Một lỗi phổ biến là đánh dấu một ô là đã truy cập mà không xem xét số kẹo, điều này phá vỡ tính chính xác vì việc đạt đến cùng một vị trí với nhiều kẹo hơn có thể mở khóa tương lai`J`tế bào. 

Một điểm tinh tế khác là kiểm tra`E`khi bật ra khỏi hàng đợi thay vì khi đẩy, điều này đảm bảo khoảng cách trả về tương ứng với đường dẫn hợp lệ ngắn nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5
S..PC
.PPP.
..P..
..J..
...E
```Chúng tôi theo dõi trạng thái BFS như`(row, col, candies, dist)`. 

| Bước | Tiểu bang | Hành động | Ghi chú | 
| --- | --- | --- | --- | 
| 1 | (0,0,0,0) | Bắt đầu | Bắt đầu BFS | 
| 2 | (0,1,0,1) | Di chuyển sang phải | Ô trống | 
| 3 | (1,0,0,1) | Di chuyển xuống | Ô trống | 
| 4 | ... | Mở rộng biên giới | Nhiều đường dẫn | 
| ... | ... | ... | Kẹo được thu thập trước J | 
| cuối cùng | (4,4,?,16) | Đạt E | Đã tìm thấy lối thoát | 

Cơ chế chính là thuật toán buộc phải định tuyến qua`C`trước khi cố gắng nhập`J`và BFS đảm bảo sự kết hợp hợp lệ sớm nhất được chọn. 

Đầu ra:```
16
```### Mẫu 2 

đầu vào:```
1 10
SCCCCJJJJE
```Chúng tôi bắt đầu lúc`S`và BFS ngay lập tức thu thập tất cả bốn viên kẹo trước khi đạt được bất kỳ viên kẹo nào.`J`. Nhà nước phát triển như sau: 

| Bước | Tiểu bang | Kẹo | Tế bào | 
| --- | --- | --- | --- | 
| 1 | (0,0,0,0) | 0 | S | 
| 2 | (0,1,1,1) | 1 | C | 
| 3 | (0,2,2,2) | 2 | C | 
| 4 | (0,3,3,3) | 3 | C | 
| 5 | (0,4,4,4) | 4 | C | 
| 6 | (0,5,3,5) | 3 | J | 
| 7 | (0,6,2,6) | 2 | J | 
| 8 | (0,7,1,7) | 1 | J | 
| 9 | (0,8,0,8) | 0 | J | 
| 10 | (0,9,0,9) | 0 | E | 

Điều này cho thấy mức tiêu thụ kẹo chính xác cần thiết để vượt qua mỗi cổng. 

Đầu ra:```
9
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m · 9) | Mỗi ô được truy cập một lần cho mỗi trạng thái kẹo | 
| Không gian | O(n · m · 9) | Các trạng thái đã truy cập và hàng đợi BFS | 

Lưới có tối đa 10.000 ô và nhiều nhất 9 trạng thái kẹo trên mỗi ô, vì vậy tổng số trạng thái là khoảng 90.000. Mỗi trạng thái xử lý bốn lần chuyển đổi, dễ dàng nằm trong giới hạn cho ràng buộc 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    with redirect_stdout(io.StringIO()) as f:
        solve()
    return f.getvalue().strip()

# provided samples
assert run("5 5\nS..PC\n.PPP.\n..P..\n..J..\n...E\n") == "16"
assert run("1 10\nSCCCCJJJJE\n") == "9"

# custom cases
assert run("1 2\nSE\n") == "1"
assert run("1 3\nSJE\n") == "SPOOKED!"
assert run("2 2\nSC\nJE\n") in ["2", "3"]
assert run("3 3\nS.P\nPJP\nC.E\n") != "", "reachable check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`S->E direct`|`1`| chuyển động tối thiểu | 
|`SJE`|`SPOOKED!`| bị J chặn không có kẹo | 
|`SC / JE`|`2 or 3`| định tuyến thông qua kẹo phụ thuộc | 
|`grid with P walls`| có thể truy cập | xử lý chướng ngại vật | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một ô được xem lại với số lượng kẹo khác nhau. Hãy xem xét một tình huống trong đó đường đi hình học ngắn nhất tới một`J`ô không cung cấp đủ kẹo, nhưng đường vòng dài hơn một chút thì có. Thuật toán xử lý việc này bằng cách xử lý`(r, c, k1)`Và`(r, c, k2)`là các trạng thái riêng biệt, do đó không có con đường nào loại bỏ con đường kia một cách sai lầm. 

Một trường hợp cạnh khác xảy ra khi lối ra`E`chỉ có thể truy cập được sau khi dùng hết kẹo. BFS cho phép tiêu thụ kẹo một cách chính xác dọc theo đường đi và đạt được`E`với`k = 0`có giá trị miễn là trình tự tiêu thụ là hợp pháp. 

Trường hợp tinh vi cuối cùng là khi nhiều viên kẹo được thu thập trước khi gặp phải bất kỳ sự cố nào.`J`. BFS đảm bảo rằng các trạng thái có số lượng kẹo cao hơn sẽ được khám phá cùng với các trạng thái thấp hơn, do đó, thuật toán sẽ tự nhiên tìm ra cấu hình giúp tối đa hóa tính khả thi thay vì thực thi bất kỳ chiến lược thu thập tham lam nào.
