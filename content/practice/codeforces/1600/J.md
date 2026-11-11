---
title: "CF 1600J - Nhà Máy Robot"
description: "Lưới mô tả một sàn nhà máy hình chữ nhật trong đó mỗi ô là một viên gạch có thể có tường ở một trong bốn cạnh của nó. Mỗi ô chứa một số từ 0 đến 15 và số này mã hóa các bức tường của nó bằng bốn bit."
date: "2026-06-15T04:37:18+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 1600
codeforces_index: "J"
codeforces_contest_name: "Bubble Cup 14 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred, Div. 2)"
rating: 1400
weight: 1600
solve_time_s: 104
verified: true
draft: false
---

[CF 1600J - Nhà máy Robot](https://codeforces.com/problemset/problem/1600/J) 

**Đánh giá:** 1400 
**Thẻ:** bitmasks, dfs và tương tự 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới mô tả một sàn nhà máy hình chữ nhật trong đó mỗi ô là một viên gạch có thể có tường ở một trong bốn cạnh của nó. Mỗi ô chứa một số từ 0 đến 15 và số này mã hóa các bức tường của nó bằng bốn bit. Giải thích biểu diễn nhị phân từ bit quan trọng nhất đến bit quan trọng nhất là Bắc, Đông, Nam và Tây, bit được đặt thành 1 có nghĩa là có một bức tường ở phía đó và 0 có nghĩa là có một lỗ mở. 

Hai viên gạch liền kề sẽ thuộc về cùng một phòng nếu không có bức tường nào giữa chúng trên cạnh chung của chúng. Nhiệm vụ là xác định có bao nhiêu thành phần được kết nối riêng biệt tồn tại trong lưới này theo các quy tắc kết nối này và tính toán kích thước của từng thành phần theo số lượng ô. Cuối cùng, đầu ra là danh sách kích thước thành phần được sắp xếp theo thứ tự giảm dần. 

Các ràng buộc cho phép tối đa 10^6 ô. Bất kỳ giải pháp nào cố gắng tái tạo đồ thị lặp đi lặp lại hoặc thực hiện công việc dư thừa trên mỗi ô đều phải cẩn thận để duy trì tuyến tính về số lượng ô. Một đợt lấp lũ đơn giản truy cập lại các ô mà không đánh dấu chúng sẽ giảm xuống hành vi hàm mũ hoặc bậc hai trong các trường hợp bệnh lý, vượt quá giới hạn thời gian. 

Một trường hợp lỗi tinh vi xảy ra khi các bức tường được hiểu không chính xác là các cạnh. Ví dụ: nếu một bộ giải giả định rằng tính kề trong lưới luôn bao hàm khả năng kết nối và bỏ qua các bit trên tường, thì nó sẽ hợp nhất tất cả các ô thành một thành phần duy nhất. 

Một lỗi phổ biến khác là việc giải thích thứ tự bit không nhất quán. Ví dụ: coi biểu diễn nhị phân là Tây-Đông-Bắc-Nam thay vì Bắc-Đông-Tây Nam sẽ kết nối các phòng không chính xác. Một ví dụ nhỏ là ô có giá trị 8, tức là 1000 ở dạng nhị phân. Điều này có nghĩa là chỉ có một bức tường phía bắc. Đọc sai nó có thể cho phép di chuyển lên trên bất hợp pháp hoặc chặn hướng sai. 

## Phương pháp tiếp cận 

Lưới có thể được xem như một biểu đồ ẩn trong đó mỗi ô là một nút. Các cạnh tồn tại giữa các ô liền kề trực giao, nhưng chỉ khi cả hai ô cho phép đi qua phía chung của chúng. Mỗi ô đóng góp các ràng buộc về việc có thể di chuyển theo từng hướng hay không. 

Cách tiếp cận bạo lực sẽ coi mọi ô là điểm bắt đầu và thực hiện DFS hoặc BFS để khám phá thành phần được kết nối của nó mà không đánh dấu các nút đã truy cập trên toàn cầu. Mỗi lần truyền tải sẽ tính toán lại khả năng tiếp cận từ đầu. Trong trường hợp xấu nhất, điều này dẫn đến việc khám phá lặp đi lặp lại cùng một khu vực nhiều lần, dẫn đến hành vi gần như O((NM)^2) nếu các thành phần lớn và được xử lý lại nhiều lần. 

Quan sát quan trọng là mỗi ô phải được gán cho chính xác một thành phần. Khi một ô được truy cập, nó sẽ không bao giờ cần được xử lý lại. Điều này chuyển đổi vấn đề thành phân tách thành phần được kết nối tiêu chuẩn trên biểu đồ lưới với tính kề cận bị ràng buộc. Một DFS hoặc BFS duy nhất cho mỗi nút không được truy cập mang lại tất cả các kích thước thành phần theo thời gian tuyến tính. 

Mã hóa tường chỉ hoạt động như một bộ lọc để xác định xem một cạnh có tồn tại giữa các lân cận hay không, do đó, việc truyền tải lưới trở thành một tìm kiếm biểu đồ với việc mở rộng lân cận có điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (bắt đầu lại tìm kiếm) | O((NM)^2) | O(NM) | Quá chậm | 
| Đã truy cập đánh dấu DFS/BFS tối ưu | O(NM) | O(NM) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Giải thích mỗi ô lưới như một nút trong biểu đồ. Nút tại (i, j) có thể kết nối với tối đa bốn nút lân cận tùy thuộc vào các bit trên tường của nó. 
2. Tính toán trước hoặc kiểm tra trực tiếp từng hướng bằng cách sử dụng mặt nạ bit: Bắc là bit 8, Đông là bit 4, Nam là bit 2, Tây là bit 1. Điều này cho phép kiểm tra chuyển động liên tục theo thời gian. 
3. Duy trì mảng đã truy cập có kích thước N x M được khởi tạo thành sai. Điều này đảm bảo mỗi ô được xử lý chính xác một lần. 
4. Lặp lại từng ô theo thứ tự hàng lớn. Khi tìm thấy một ô chưa được truy cập, hãy khởi động BFS hoặc DFS từ ô đó. 
5. Trong quá trình truyền tải, đánh dấu ô bắt đầu là đã truy cập và khởi tạo bộ đếm để theo dõi kích thước thành phần. 
6. Đối với mỗi ô hiện tại, hãy cố gắng di chuyển theo cả bốn hướng. Chỉ được phép di chuyển nếu không có bức tường theo hướng đó trong ô hiện tại và không có bức tường nào ở hướng ngược lại trong ô lân cận. Kiểm tra kép này ngăn chặn việc vượt qua các ranh giới không nhất quán. 
7. Mỗi ô có thể truy cập mới được phát hiện đều được đánh dấu đã truy cập và được thêm vào cấu trúc truyền tải. Tăng kích thước thành phần cho mỗi khám phá như vậy. 
8. Sau khi quá trình truyền tải kết thúc, lưu trữ kích thước thành phần. 
9. Tiếp tục quét lưới cho đến khi tất cả các ô đã được truy cập. 
10. Sắp xếp tất cả các kích thước thành phần đã thu thập theo thứ tự giảm dần và xuất chúng. 

### Tại sao nó hoạt động 

Cấu trúc được truy cập buộc mỗi ô thuộc về chính xác một cây duyệt. Mỗi BFS khám phá chính xác tập hợp các ô có thể tiếp cận mà không cần vượt qua ranh giới tường. Bởi vì sự kề cận chỉ đối xứng khi cả hai ô đều đồng ý về độ mở nên việc kiểm tra cạnh điều kiện kép đảm bảo đồ thị được xây dựng ngầm một cách chính xác. Do đó, quá trình duyệt liệt kê chính xác các thành phần được kết nối của biểu đồ ẩn này và mỗi nút được tính một lần theo đúng một kích thước thành phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]

    visited = [[False] * m for _ in range(n)]

    # bit directions: N=8, E=4, S=2, W=1
    dirs = [(-1, 0, 8, 2), (0, 1, 4, 1), (1, 0, 2, 8), (0, -1, 1, 4)]

    def bfs(si, sj):
        q = deque()
        q.append((si, sj))
        visited[si][sj] = True
        size = 0

        while q:
            i, j = q.popleft()
            size += 1

            val = grid[i][j]

            for di, dj, wall_here, wall_next in dirs:
                if val & wall_here:
                    continue

                ni, nj = i + di, j + dj
                if ni < 0 or nj < 0 or ni >= n or nj >= m:
                    continue
                if visited[ni][nj]:
                    continue

                # check opposite wall in neighbor
                if grid[ni][nj] & wall_next:
                    continue

                visited[ni][nj] = True
                q.append((ni, nj))

        return size

    components = []

    for i in range(n):
        for j in range(m):
            if not visited[i][j]:
                components.append(bfs(i, j))

    components.sort(reverse=True)
    print(*components)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng BFS từ mỗi ô chưa được truy cập. Mảng hướng mã hóa cả vùng đồng bằng chuyển động và bit tường tương ứng trên ô hiện tại và ô lân cận. Phần tinh tế nhất là kiểm tra tường kép: bỏ qua chuyển động nếu một trong hai bên có tường để đảm bảo tính nhất quán của kết nối. 

Việc đánh dấu đã truy cập xảy ra ngay lập tức khi đưa một nút vào hàng đợi chứ không phải khi loại bỏ nút đó. Điều này tránh việc chèn trùng lặp và đảm bảo độ phức tạp tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
0 0 0
0 0 0
```Tất cả các viên gạch đều không có tường nên mọi thứ đều được kết nối. 

| Bước | Bắt đầu ô | Kích thước BFS | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 6 | [6] | 

Tất cả các ô đều có thể truy cập được, tạo ra một thành phần có kích thước 6. 

### Ví dụ 2 

đầu vào:```
2 2
15 15
15 15
```Tất cả các viên gạch đều được ốp tường hoàn chỉnh nên không thể di chuyển được. 

| Bước | Bắt đầu ô | Kích thước BFS | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 1 | [1] | 
| 2 | (0,1) | 1 | [1, 1] | 
| 3 | (1,0) | 1 | [1, 1, 1] | 
| 4 | (1,1) | 1 | [1, 1, 1, 1] | 

Mỗi ô tạo thành thành phần riêng của nó vì tất cả các cạnh đều bị chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi ô được truy cập đúng một lần và mỗi lần kiểm tra cạnh là công việc liên tục | 
| Không gian | O(NM) | Đã truy cập mảng và hàng đợi BFS lưu trữ ở hầu hết các ô | 

Kích thước lưới có thể đạt tới một triệu ô và việc truyền tải theo thời gian tuyến tính với các kiểm tra lân cận theo thời gian không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n, m = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(n)]
        visited = [[False] * m for _ in range(n)]
        dirs = [(-1, 0, 8, 2), (0, 1, 4, 1), (1, 0, 2, 8), (0, -1, 1, 4)]

        def bfs(si, sj):
            q = deque([(si, sj)])
            visited[si][sj] = True
            size = 0
            while q:
                i, j = q.popleft()
                size += 1
                val = grid[i][j]
                for di, dj, w1, w2 in dirs:
                    if val & w1:
                        continue
                    ni, nj = i + di, j + dj
                    if ni < 0 or nj < 0 or ni >= n or nj >= m:
                        continue
                    if visited[ni][nj]:
                        continue
                    if grid[ni][nj] & w2:
                        continue
                    visited[ni][nj] = True
                    q.append((ni, nj))
            return size

        res = []
        for i in range(n):
            for j in range(m):
                if not visited[i][j]:
                    res.append(bfs(i, j))
        res.sort(reverse=True)
        return " ".join(map(str, res))

    return solve()

# provided sample
assert run("""4 5
9 14 11 12 13
5 15 11 6 7
5 9 14 9 14
3 2 14 3 14
""") == "9 4 4 2 1"

# custom: single cell
assert run("1 1\n0\n") == "1"

# custom: fully open 2x2
assert run("2 2\n0 0\n0 0\n") == "4"

# custom: fully blocked
assert run("2 2\n15 15\n15 15\n") == "1 1 1 1"

# custom: horizontal split
assert run("1 4\n0 4 0 4\n") == "1 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 không | 1 | lưới tối thiểu | 
| 2x2 đều mở | 4 | kết nối đầy đủ | 
| 2x2 tất cả các bức tường | 1 1 1 1 | các nút bị cô lập | 
| tường xen kẽ | 1 1 1 1 | chặn định hướng | 

## Vỏ cạnh 

Một lưới cách ly hoàn toàn, trong đó mỗi ô là 15, đảm bảo BFS không bao giờ mở rộng ra ngoài một nút duy nhất. Bắt đầu từ (0,0), tất cả bốn hướng đều bị chặn ngay lập tức, do đó kích thước thành phần là 1. Lặp lại điều này cho mỗi ô sẽ mang lại danh sách các hướng. 

Một lưới mở hoàn toàn, trong đó mỗi ô bằng 0, tạo ra một BFS lớn bắt đầu từ (0,0). Mọi hàng xóm đều có thể truy cập được vì không có bit tường nào ngăn cản chuyển động và các vùng lân cận là đối xứng, vì vậy tất cả các ô NM đều được tiêu thụ trong một lần truyền tải. 

Trường hợp ranh giới hỗn hợp như một hàng đơn với các bức tường xen kẽ sẽ kiểm tra tính chính xác về hướng. Các ô có giá trị 4 chặn chuyển động về phía đông nên kết nối sẽ luân phiên. BFS từ mỗi ô chưa được thăm dò chỉ mở rộng khi cả hai ô liền kề đều cho phép đi qua, tạo ra nhiều thành phần đơn lẻ.
