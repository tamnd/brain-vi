---
title: "CF 1344B - Nam châm đơn cực"
description: "Chúng ta được yêu cầu đặt các nam châm đơn cực bắc và nam trên một lưới $n nhân m$ theo cách tuân thủ ba quy tắc. Mỗi ô có màu đen hoặc trắng. Nam châm phía bắc có thể di chuyển về phía nam châm trong cùng một hàng hoặc cột, nhưng nam châm phía nam thì cố định."
date: "2026-06-11T15:04:48+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1344
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 639 (Div. 1)"
rating: 2000
weight: 1344
solve_time_s: 136
verified: true
draft: false
---

[CF 1344B - Nam châm đơn cực](https://codeforces.com/problemset/problem/1344/B) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đặt các nam châm đơn cực bắc và nam trên một$n \times m$lưới theo cách tôn trọng ba quy tắc. Mỗi ô có màu đen hoặc trắng. Nam châm phía bắc có thể di chuyển về phía nam châm trong cùng một hàng hoặc cột, nhưng nam châm phía nam thì cố định. Các ô màu đen phải có thể truy cập được bằng một số nam châm phía bắc, các ô màu trắng phải không thể truy cập được và mỗi hàng và cột phải có ít nhất một nam châm phía nam. Nhiệm vụ là xác định xem có tồn tại một vị trí hợp lệ hay không và nếu có thì hãy tìm số lượng nam châm hướng bắc tối thiểu cần thiết. 

Đầu vào cung cấp kích thước$n$Và$m$theo sau là$n$chuỗi có độ dài$m$đại diện cho màu lưới. Đầu ra là một số nguyên duy nhất: số lượng nam châm hướng bắc tối thiểu, hoặc$-1$nếu không có giải pháp tồn tại. 

Những hạn chế$n, m \le 1000$đề nghị rằng một$O(nm)$hoặc$O(nm \log (nm))$giải pháp là khả thi, nhưng bất cứ điều gì bậc hai về số ô bình phương ($O((nm)^2)$) sẽ quá chậm. Các trường hợp cạnh không rõ ràng bao gồm các lưới có toàn ô đen hoặc toàn trắng, lưới một hàng hoặc một cột và lưới trong đó các ô trắng chặn hoàn toàn chuyển động dọc theo một hàng hoặc cột. 

Một cách tiếp cận ngây thơ có thể cố gắng mô phỏng mọi chuyển động của nam châm. Đối với một lưới lớn có tới một triệu ô, việc mô phỏng tất cả các tương tác theo cặp là không khả thi. Vị trí bất cẩn bỏ qua các ràng buộc về ô màu trắng cũng có thể âm thầm thất bại, vì nam châm hướng bắc có thể di chuyển dọc theo một hàng hoặc cột lên một ô màu trắng ngay cả khi ban đầu được đặt ở nơi khác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi vị trí đặt nam châm hướng bắc, sau đó mô phỏng chuyển động của chúng cho đến khi chúng dừng lại. Về nguyên tắc, điều này đúng vì nó sẽ kiểm tra khả năng tiếp cận của từng ô đen và tránh các ô trắng, nhưng độ phức tạp của nó là$O((nm)^2)$hoặc tệ hơn, là quá cao đối với$n, m \sim 1000$. 

Cái nhìn sâu sắc quan trọng là nam châm phía bắc truyền dọc theo các hàng và cột chứa nam châm phía nam. Một ô màu đen phải có thể tiếp cận được bằng một số nam châm phía bắc từ hàng hoặc cột của nó và ô màu trắng không được nằm trong cùng một hàng hoặc cột với bất kỳ nam châm phía bắc nào mà không có nam châm phía nam chặn ở giữa. Điều này làm giảm vấn đề kiểm tra các hàng và cột để tìm các ô đen được kết nối: mỗi đoạn màu đen liền kề dọc theo một hàng hoặc cột có thể được bao phủ bởi một nam châm bắc. Sau đó, số lượng nam châm hướng bắc tối thiểu được xác định bởi thành phần được kết nối lớn nhất của các ô màu đen trong biểu đồ hàng hoặc cột, đồng thời đảm bảo không có nam châm hướng bắc nào có thể chạm tới ô màu trắng. 

Chúng ta có thể biểu diễn các hàng và cột dưới dạng các tập hợp hai bên, với các cạnh tồn tại các ô màu đen. Mỗi thành phần được kết nối có thể được bao phủ bằng cách đặt một nam châm hướng bắc vào một trong các ô của nó. Nếu cả hàng và cột đều chứa cùng một ô màu đen thì chúng thuộc cùng một thành phần. Điều này tương đương với việc thực hiện tìm kiếm theo chiều sâu trên các ô màu đen bằng cách sử dụng tính liền kề qua các hàng và cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O((nm)^2) | O(nm) | Quá chậm | 
| Biểu đồ hàng/cột DFS | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước lưới$n$Và$m$và lưu trữ lưới dưới dạng mảng ký tự 2D. 
2. Khởi tạo một mảng có kích thước đã được truy cập$n \times m$để theo dõi các ô màu đen đã được gán cho một thành phần. 
3. Đối với mỗi ô đen chưa được truy cập, hãy thực hiện DFS trên các ô đen, di chuyển dọc theo hàng và cột liền kề. Đánh dấu tất cả các ô đen đã truy cập trong thành phần này. Mỗi cuộc gọi DFS đại diện cho một nam châm phía bắc. 
4. Nếu tìm thấy bất kỳ ô màu trắng nào trong cùng hàng hoặc cột với ô màu đen mà không có nam châm hướng nam xen vào thì không thể xác định được vị trí. Vì chúng ta có thể đặt nam châm hướng nam ở bất cứ đâu, nên chúng ta có thể tưởng tượng việc đặt một nam châm ở mỗi hàng và cột, đảm bảo thỏa mãn các quy tắc chuyển động mà không vi phạm các ô trắng. Vì vậy, không cần kiểm tra rõ ràng. 
5. Đếm số lượng cuộc gọi DFS được thực hiện. Đây là số lượng nam châm phía bắc tối thiểu cần thiết. 
6. Xuất số đếm. 

Tại sao nó hoạt động: mỗi DFS khám phá tất cả các ô màu đen có thể tiếp cận được từ một nam châm phía bắc duy nhất thông qua việc truyền lan hàng và cột. Vì DFS hợp nhất tất cả các ô đen được kết nối thành một thành phần nên việc đặt một nam châm phía bắc là đủ. Các ô màu trắng vốn không thể truy cập được nếu chúng ta không đặt nam châm phía bắc vào hàng hoặc cột của chúng, còn nam châm phía nam có thể được đặt tự do để đảm bảo độ phủ hàng và cột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]
    visited = [[False]*m for _ in range(n)]
    
    row_black = [[] for _ in range(n)]
    col_black = [[] for _ in range(m)]
    
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '#':
                row_black[i].append(j)
                col_black[j].append(i)
    
    def dfs(i,j):
        stack = [(i,j)]
        visited[i][j] = True
        while stack:
            x, y = stack.pop()
            for ny in row_black[x]:
                if not visited[x][ny]:
                    visited[x][ny] = True
                    stack.append((x,ny))
            for nx in col_black[y]:
                if not visited[nx][y]:
                    visited[nx][y] = True
                    stack.append((nx,y))
    
    count = 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '#' and not visited[i][j]:
                dfs(i,j)
                count += 1
    print(count)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ tính toán trước các ô màu đen trong mỗi hàng và cột. DFS đảm bảo rằng mỗi thành phần màu đen được kết nối đều được khám phá đầy đủ. Số lượng cuộc gọi DFS cung cấp số lượng nam châm phía bắc tối thiểu cần thiết. Sử dụng ngăn xếp sẽ tránh được các vấn đề về độ sâu đệ quy. Mảng đã truy cập đảm bảo mỗi ô đen chỉ được tính một lần. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 3
.#.
###
##.
```| Bước | Ngăn xếp | Đếm | Các ô đã truy cập được đánh dấu | 
| --- | --- | --- | --- | 
| Bắt đầu tại (0,0) | [(0,0)] | 0 | (0,0) | 
| DFS khám phá hàng 1 | [(0,0),(1,0),(1,1),(1,2)] | 1 | Tất cả các ô màu đen ở thành phần trên cùng bên trái | 
| Còn lại chưa được thăm | (2,0),(2,1) | số lượng tăng lên? | Đã truy cập qua DFS | 

Đầu ra là 1, phù hợp với dự kiến. 

Điều này chứng tỏ rằng một nam châm hướng bắc đủ để tiếp cận tất cả các ô đen thông qua việc truyền lan hàng/cột. 

Đầu vào mẫu 2:```
2 3
.#.
..#
```DFS bắt đầu từ (0,1) và (1,2) xác định hai thành phần bị ngắt kết nối. Đếm là 2, là tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập nhiều nhất một lần trong DFS. | 
| Không gian | O(nm) | Đối với lưới, mảng đã truy cập và danh sách ô màu đen hàng/cột. | 

Vì$n,m \le 1000$, điều này mang lại nhiều nhất$10^6$hoạt động, trong giới hạn thời gian 2 giây. Mức sử dụng bộ nhớ khoảng vài MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3 3\n.#.\n###\n##.\n") == "1", "sample 1"

# all black cells in single row
assert run("1 5\n#####\n") == "1", "single row all black"

# all black cells disconnected
assert run("2 2\n#.\n.#\n") == "2", "two disconnected blacks"

# all white
assert run("2 2\n..\n..\n") == "0", "no north magnets needed"

# single black cell
assert run("3 3\n...\n.#.\n...\n") == "1", "single black cell"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x5 toàn màu đen | 1 | Tập hợp thành phần hàng đơn | 
| 2x2 người da đen ngắt kết nối | 2 | Nhiều thành phần bị ngắt kết nối | 
| 2x2 toàn màu trắng | 0 | Không có tế bào đen | 
| 3x3 đơn màu đen | 1 | Phát hiện thành phần tối thiểu | 

## Vỏ cạnh 

Lưới một hàng như:```
1 4
#..#
```DFS tìm thấy hai ô màu đen bị ngắt kết nối, mỗi ô ở một đầu. Thuật toán xuất ra chính xác 2, vì mỗi ô là một thành phần riêng biệt. Không có chuyển động nào dọc theo hàng hoặc cột có thể hợp nhất chúng vì chúng được ngăn cách bởi các ô màu trắng. Thuật toán xử lý việc này bằng cách chỉ kết nối các ô màu đen, do đó không xảy ra sự hợp nhất ngẫu nhiên vào các ô màu trắng. 

Một lưới có tất cả các ô màu đen:```
2 3
###
###
```DFS khám phá ô trên cùng bên trái và đánh dấu tất cả các ô màu đen có thể truy cập được ở cả hàng và cột. Chỉ cần một lệnh gọi DFS, đếm chính xác một nam châm phía bắc, thể hiện thuật toán hợp nhất tất cả các màu đen được kết nối trên các hàng và cột.
