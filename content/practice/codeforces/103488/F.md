---
title: "CF 103488F - Tầm nhìn tương lai"
description: "Chúng ta có một mê cung dạng lưới trong đó một số ô là những bức tường và những ô khác trống rỗng. Một ký tự bắt đầu từ một ô cố định được đánh dấu H tại thời điểm 0 và có thể di chuyển từng phút đến bất kỳ ô nào trong bốn ô liền kề hoặc giữ nguyên vị trí. Chuyển động bị chặn bởi các bức tường."
date: "2026-07-03T09:47:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103488
codeforces_index: "F"
codeforces_contest_name: "The 2021 Zhejiang University City College Freshman Programming Contest"
rating: 0
weight: 103488
solve_time_s: 51
verified: true
draft: false
---

[CF 103488F - Tầm nhìn tương lai](https://codeforces.com/problemset/problem/103488/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mê cung dạng lưới trong đó một số ô là những bức tường và những ô khác trống rỗng. Một ký tự bắt đầu từ một ô cố định được đánh dấu`H`tại thời điểm 0 và có thể di chuyển từng phút đến bất kỳ ô nào trong số bốn ô liền kề hoặc giữ nguyên vị trí. Chuyển động bị chặn bởi các bức tường. Đồng thời, vị trí mục tiêu được tiết lộ theo từng phút`0`theo thời gian`k-1`, và tại thời điểm`i`thanh kiếm được đặt ở ô được cung cấp`(x_i, y_i)`. 

Nhiệm vụ là xác định xem nhân vật có thể đến ô đúng lúc thanh kiếm ở đó hay không và nếu có thì báo cáo thời điểm đó sớm nhất. 

Cấu trúc chính là cả người chơi và thanh kiếm đều phát triển theo thời gian, nhưng theo những cách rất khác nhau. Người chơi di chuyển trên lưới với các chuyển tiếp Manhattan ở tốc độ đơn vị bị hạn chế bởi các bức tường, trong khi thanh kiếm chỉ cần dịch chuyển tức thời đến một chuỗi vị trí đã biết. 

Kích thước lưới tối đa là 100 x 100 và mỗi thử nghiệm có tổng số tối đa 10.000 ô. Trong tối đa 100 trường hợp thử nghiệm, việc tính toán đường đi ngắn nhất cho mỗi thử nghiệm là khả thi, nhưng mọi phép tính bậc hai trên mỗi bước thời gian trong k vẫn sẽ đạt nếu được thực hiện cẩn thận. 

Một điểm tinh tế là thanh kiếm có thể xuất hiện trên tường, nghĩa là việc đạt đến vị trí cầm kiếm không yêu cầu lúc đó ô có thể đi được mà chỉ cần người chơi có thể đứng ở đó. Tuy nhiên, vì người chơi không thể chiếm giữ các bức tường nên những vị trí đó chỉ là mục tiêu hợp lệ nếu chúng không phải là các bức tường trong lưới. 

Chi tiết quan trọng thứ hai là việc chờ đợi được cho phép. Điều này có nghĩa là các ràng buộc chẵn lẻ không cản trở khả năng tiếp cận; thời gian luôn có thể bị kéo dài. 

Trường hợp cạnh thứ ba là khi thanh kiếm xuất hiện ở vị trí bắt đầu vào thời điểm 0, phải được chấp nhận ngay lập tức. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng từng bước thời gian. Cho mỗi lần`t`, chúng ta có thể tính toán lại liệu người chơi có thể tiếp cận`(x_t, y_t)`chính xác`t`di chuyển bắt đầu từ`H`. Điều đó có nghĩa là chạy BFS hoặc tính toán đường đi ngắn nhất cho mỗi bước thời gian, mang lại độ phức tạp khoảng`k * n * m`mỗi bài kiểm tra. Trong trường hợp xấu nhất điều này trở thành về`10^4 * 10^4 = 10^8`hoạt động trên mỗi thử nghiệm, quá chậm khi nhân với tối đa 100 trường hợp thử nghiệm. 

Quan sát quan trọng là khả năng tiếp cận ngay từ đầu không phụ thuộc vào vị trí kiếm. Chúng ta chỉ cần một khoảng cách đường đi ngắn nhất từ`H`đến từng ô trong lưới. Khi đã biết những khoảng cách này, hãy kiểm tra xem người chơi có thể ở gần thanh kiếm vào thời điểm đó hay không`t`trở thành một điều kiện đơn giản: khoảng cách đường đi ngắn nhất phải lớn nhất`t`, vì có thể dành thêm thời gian chờ đợi tại chỗ. 

Điều này làm giảm vấn đề từ việc tìm kiếm biểu đồ lặp đi lặp lại thành một BFS trên lưới. Sau đó, chúng tôi quét dòng thời gian của thanh kiếm một lần và tìm ra thời điểm sớm nhất có điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại BFS mỗi lần | O(k · n · m) | O(n · m) | Quá chậm | 
| BFS đơn + quét | O(n · m + k) | O(n · m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta tính khoảng cách ngắn nhất từ`H`tới mọi ô bằng cách sử dụng BFS tiêu chuẩn trên lưới. 

1. Xác định vị trí ô bắt đầu`H`và khởi tạo ma trận khoảng cách với các giá trị vô cực, sau đó đặt khoảng cách ô bắt đầu về 0. Điều này mã hóa rằng chúng ta bắt đầu ở thời điểm 0 mà không cần di chuyển. 
2. Chạy BFS từ`H`khắp bốn phương. Bất cứ khi nào chúng tôi tiếp cận một ô không có tường hợp lệ lần đầu tiên, chúng tôi sẽ chỉ định khoảng cách của nó là độ sâu lớp BFS. Điều này đảm bảo khoảng cách là số lần di chuyển tối thiểu cần thiết để đến được ô đó. 
3. Sau khi BFS hoàn tất, chúng tôi dành cho mỗi ô thời gian tối thiểu cần thiết để đứng ở đó. 
4. Đối với mỗi lần`t`từ`0`ĐẾN`k-1`, đọc vị trí kiếm`(x_t, y_t)`và kiểm tra xem ô lưới có phải là một bức tường hay không và liệu`dist[x_t][y_t] ≤ t`. Nếu cả hai điều kiện đều đúng, chúng ta có thể đến ô đó không muộn hơn thời gian`t`và sau đó chờ đợi nếu cần thiết. 
5. Lần đầu tiên như vậy là đáp án; nếu không có thời gian thỏa mãn điều kiện, xuất ra`"NO"`. 

Lý do chúng ta không yêu cầu sự bằng nhau chính xác giữa khoảng cách và thời gian là vì việc chờ đợi luôn được cho phép. Nếu con đường ngắn nhất mất 3 bước nhưng thanh kiếm xuất hiện ở thời điểm thứ 5 thì chúng ta có thể đến được nó ở thời điểm thứ 3 và đợi hai phút. 

### Tại sao nó hoạt động 

BFS đảm bảo rằng`dist[v]`là số lần di chuyển tối thiểu cần thiết để đến bất kỳ ô nào`v`ngay từ đầu. Bất kỳ quỹ đạo hợp lệ nào tới một ô tại một thời điểm`t`phải bao gồm ít nhất`dist[v]`di chuyển, cộng với các bước chờ tùy chọn. Vì vậy, khả năng tiếp cận tại thời điểm`t`tương đương với`dist[v] ≤ t`. Vì vị trí thanh kiếm được kiểm tra theo trình tự thời gian nên lần đầu tiên điều kiện này được duy trì là thời điểm bắt giữ sớm nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]
    
    sx = sy = -1
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'H':
                sx, sy = i, j

    dist = [[-1] * m for _ in range(n)]
    q = deque()
    q.append((sx, sy))
    dist[sx][sy] = 0

    dirs = [(1,0), (-1,0), (0,1), (0,-1)]

    while q:
        x, y = q.popleft()
        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < m:
                if grid[nx][ny] != '#' and dist[nx][ny] == -1:
                    dist[nx][ny] = dist[x][y] + 1
                    q.append((nx, ny))

    k = int(input())
    for t in range(k):
        x, y = map(int, input().split())
        x -= 1
        y -= 1

        if 0 <= x < n and 0 <= y < m and grid[x][y] != '#':
            if dist[x][y] != -1 and dist[x][y] <= t:
                print("YES", t)
                return

    print("NO")

if __name__ == "__main__":
    solve()
```Phần BFS xây dựng bản đồ khoảng cách đầy đủ từ vị trí bắt đầu, coi các bức tường là không thể vượt qua. Hàng đợi xử lý các ô theo thứ tự khoảng cách tăng dần, vì vậy lần đầu tiên chúng ta gán khoảng cách cho một ô là tối ưu. 

Sau đó, vòng truy vấn sẽ trực tiếp kiểm tra điều kiện xuất phát từ logic chờ. Việc trừ 1 khỏi tọa độ đầu vào là rất quan trọng vì lưới được lập chỉ mục 0 bên trong trong khi đầu vào được lập chỉ mục 1. 

Việc quay trở lại sớm đảm bảo chúng tôi dừng lại ở thời gian hợp lệ sớm nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới đơn giản trong đó điểm bắt đầu ở gần một khu vực mở và thanh kiếm xuất hiện ngay tại cùng một vị trí. 

| Bước | Sự kiện | Vị trí | kiểm tra quận | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | BFS ban đầu | H | 0 0 0 | CÓ | 

Điều này thể hiện trường hợp chụp ngay lập tức mà không cần chuyển động. 

Bây giờ hãy xem xét trường hợp ban đầu thanh kiếm không thể chạm tới được nhưng sau đó lại có thể chạm tới được. 

| t | Kiếm (x, y) | dist[x][y] | Điều kiện dist ≤ t | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | ô bị chặn | -1 | sai | không | 
| 1 | tế bào xa | 3 | sai | không | 
| 2 | giống nhau | 3 | sai | không | 
| 3 | giống nhau | 3 | đúng | CÓ | 

Dấu vết này cho thấy việc chờ đợi khiến thất bại trung gian trở nên vô nghĩa như thế nào và chỉ có ngưỡng lần đầu mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m + k) | BFS truy cập từng ô một lần, sau đó mỗi truy vấn kiếm được kiểm tra một lần | 
| Không gian | O(n · m) | Lưới khoảng cách và hàng đợi BFS trên mê cung | 

Các giới hạn n, m ≤ 100 làm cho BFS không đáng kể và thậm chí k lên tới 10.000 cho mỗi lần kiểm tra cũng đủ nhỏ để quét tuyến tính. Qua 100 bài kiểm tra, điều này vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    def input():
        return sys.stdin.readline()
    
    def solve():
        n, m = map(int, input().split())
        grid = [list(input().strip()) for _ in range(n)]
        
        sx = sy = -1
        for i in range(n):
            for j in range(m):
                if grid[i][j] == 'H':
                    sx, sy = i, j

        dist = [[-1]*m for _ in range(n)]
        q = deque()
        q.append((sx, sy))
        dist[sx][sy] = 0

        dirs = [(1,0),(-1,0),(0,1),(0,-1)]

        while q:
            x,y = q.popleft()
            for dx,dy in dirs:
                nx,ny = x+dx,y+dy
                if 0<=nx<n and 0<=ny<m:
                    if grid[nx][ny] != '#' and dist[nx][ny] == -1:
                        dist[nx][ny] = dist[x][y] + 1
                        q.append((nx,ny))

        k = int(input())
        for t in range(k):
            x,y = map(int, input().split())
            x-=1;y-=1
            if 0<=x<n and 0<=y<m and grid[x][y] != '#':
                if dist[x][y] != -1 and dist[x][y] <= t:
                    out.append(f"YES {t}")
                    return
        out.append("NO")

    solve()
    return "\n".join(out)

# minimum grid
assert run("1 1\nH\n1\n1 1\n") == "YES 0", "min case"

# unreachable
assert run("2 2\nH#\n##\n2\n2 2\n1 2\n") == "NO", "blocked"

# reachable later
assert run("2 2\nH.\n..\n3\n2 2\n2 2\n2 1\n") == "YES 2", "delayed"

# obstacle maze
assert run("3 3\nH..\n###\n..#\n3\n1 3\n3 1\n3 3\n") == "NO", "walls block"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bắt đầu 1×1 | CÓ 0 | trận đấu ngay lập tức | 
| lưới bị chặn | KHÔNG | mục tiêu không thể tiếp cận | 
| tầm với bị trì hoãn | CÓ 2 | logic chờ đợi | 
| mê cung dày tường | KHÔNG | Tính chính xác của BFS dưới chướng ngại vật | 

## Vỏ cạnh 

Một trường hợp thất bại phổ biến là quên rằng việc chờ đợi là được phép. Nếu có thể truy cập được một ô sớm hơn thời gian kiếm, thì giải pháp đúng vẫn chấp nhận ô đó. Ví dụ, nếu`dist = 2`và thanh kiếm xuất hiện đúng lúc`5`, câu trả lời có giá trị tại`t = 5`, không bị từ chối do không khớp. 

Một trường hợp khác là khi thanh kiếm xuất hiện trên ô tường. BFS vẫn phải tính toán khoảng cách bình thường, nhưng việc kiểm tra phải loại bỏ các bức tường vì người chơi không thể chiếm được chúng. Việc triển khai đúng sẽ xác minh rõ ràng`grid[x][y] != '#'`. 

Cuối cùng, ô bắt đầu xuất hiện dưới dạng thanh kiếm ở thời điểm 0 phải được xử lý trước bất kỳ giả định BFS nào. Từ`dist[start] = 0`, điều kiện`dist <= 0`chính xác sẽ tạo nên thành công ngay lập tức.
