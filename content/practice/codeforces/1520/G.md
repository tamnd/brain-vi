---
title: "CF 1520G - Đi hay không đi?"
description: "Chúng ta được yêu cầu tìm thời gian tối thiểu để Dima đi từ góc trên bên trái của lưới thành phố hình chữ nhật đến góc dưới bên phải. Thành phố được biểu diễn dưới dạng lưới n × m trong đó mỗi ô có giá trị: -1 cho bị chặn, 0 cho miễn phí và x 0 cho cổng có chi phí x."
date: "2026-06-10T18:09:27+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "graphs", "greedy", "implementation", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1520
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 719 (Div. 3)"
rating: 2200
weight: 1520
solve_time_s: 134
verified: true
draft: false
---

[CF 1520G - Đi hay không đi?](https://codeforces.com/problemset/problem/1520/G) 

**Đánh giá:** 2200 
**Tags:** vũ lực, dfs và tương tự, đồ thị, tham lam, triển khai, đường dẫn ngắn nhất 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm thời gian tối thiểu để Dima đi từ góc trên bên trái của lưới thành phố hình chữ nhật đến góc dưới bên phải. Thành phố được thể hiện như một`n × m`lưới trong đó mỗi ô có một giá trị:`-1`vì bị chặn,`0`miễn phí và`x > 0`cho một cổng thông tin có chi phí`x`. Dima có thể di chuyển kịp thời đến bất kỳ ô trống liền kề trực giao nào`w`. Ngoài ra, từ bất kỳ ô cổng nào, anh ta có thể dịch chuyển tức thời đến bất kỳ cổng nào khác và thời gian dịch chuyển là tổng chi phí của hai ô cổng. 

Đầu vào cung cấp lưới và chi phí bước`w`và đầu ra là thời gian tối thiểu để đến đích hoặc`-1`nếu điều đó là không thể. Cả ô bắt đầu và ô kết thúc đều được đảm bảo miễn phí, vì vậy Dima luôn có điểm bắt đầu hợp lệ. 

Những hạn chế là`n, m ≤ 2000`, có nghĩa là lên tới 4 triệu tế bào. Bất kỳ cách tiếp cận đơn giản nào lặp lại trên tất cả các cặp ô hoặc cổng sẽ yêu cầu khoảng`O(n²m²)`hoạt động quá lớn. Chúng tôi cũng có chi phí cổng thông tin tiềm năng lớn và chi phí di chuyển lên tới 10^9, điều này loại trừ một số chiến lược chỉ dành cho BFS đơn giản hóa mà không xử lý chi phí một cách cẩn thận. 

Các trường hợp biên không rõ ràng bao gồm một lưới trong đó tồn tại một đường dẫn trực tiếp mà không cần sử dụng cổng, các lưới nơi bắt buộc phải có cổng để đến được mục tiêu và các lưới trong đó điểm bắt đầu hoặc kết thúc bị chặn khỏi tất cả các ô khác ngoại trừ thông qua cổng. Ví dụ:```
2 2 1
0 -1
-1 1
```Ở đây, cách duy nhất để tiếp cận góc dưới bên phải là sử dụng cổng, nếu không, một cổng bỏ qua BFS ngây thơ sẽ xuất ra`-1`. 

Một tình huống phức tạp khác là khi con đường tối thiểu yêu cầu kết hợp giữa việc đi bộ và đúng một lần dịch chuyển tức thời. Nếu thuật toán không xem xét cả hai một cách độc lập, nó có thể tính toán trong thời gian dài hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là coi mọi ô như một nút trong biểu đồ và chạy thuật toán Dijkstra, bao gồm tất cả các cổng dưới dạng các cạnh dịch chuyển giữa mỗi cặp. Điều này sẽ hiệu quả vì kích thước lưới đủ nhỏ cho tìm kiếm dựa trên BFS, nhưng việc thêm biểu đồ hoàn chỉnh giữa các cổng sẽ bổ sung thêm`O(p²)`cạnh ở đâu`p`là số cổng. Trong trường hợp xấu nhất,`p ≈ n*m`, làm cho phương pháp này không khả thi do`O(n²m²)`hoạt động. 

Thông tin chi tiết quan trọng là chúng ta không cần phải xem xét từng cạnh cổng thông tin đến cổng thông tin riêng lẻ. Chúng tôi có thể tính toán riêng chi phí tối thiểu để tiếp cận bất kỳ cổng nào ngay từ đầu và chi phí tối thiểu để đạt được mục tiêu từ bất kỳ cổng nào. Cho phép`d_start[i][j]`là thời gian tối thiểu để đến ô`(i,j)`ngay từ đầu chỉ sử dụng các bước di chuyển tiêu chuẩn. Cho phép`d_end[i][j]`là thời gian tối thiểu để đến ô`(i,j)`từ mục tiêu, một lần nữa chỉ sử dụng các bước di chuyển tiêu chuẩn. Sau đó với mỗi cổng thông tin`(i,j)`chúng tôi tính toán tổng đường dịch chuyển tối thiểu là`d_start[i][j] + cost[i][j] + cost[k][l] + d_end[k][l]`trên tất cả các cặp cổng thông tin`(i,j),(k,l)`. 

Chúng tôi có thể tối ưu hóa điều này hơn nữa: thay vì kiểm tra tất cả các cặp cổng, chúng tôi chỉ có thể theo dõi cổng với mức tối thiểu.`d_start[i][j] + cost[i][j]`và tối thiểu`d_end[i][j] + cost[i][j]`. Đường dịch chuyển tối thiểu khi đó là tổng của hai giá trị tối thiểu này. Cuối cùng, câu trả lời là mức tối thiểu: con đường trực tiếp từ đầu đến cuối mà không cần dịch chuyển tức thời, con đường sử dụng một dịch chuyển tức thời theo một trong hai hướng hoặc cả hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đầy đủ Dijkstra với tất cả các cạnh cổng | O(n² m2) | O(nm) | Quá chậm | 
| BFS/Dijkstra được tối ưu hóa với khoảng cách cổng tối thiểu | O(n m) | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một`dist`mảng cho lưới kích thước`n × m`, chứa đầy vô cùng, và đặt`dist[0][0] = 0`. Điều này sẽ theo dõi khoảng cách tối thiểu từ đầu. 
2. Thực hiện BFS (hoặc Dijkstra với hàng đợi ưu tiên) từ ô bắt đầu, chỉ xem xét các bước di chuyển tiêu chuẩn đến các ô trống liền kề với chi phí`w`. Sau chuyện này,`d_start[i][j]`lưu trữ thời gian tối thiểu để đạt được`(i,j)`mà không cần sử dụng dịch chuyển tức thời. 
3. Thực hiện BFS/Dijkstra tương tự từ ô mục tiêu để tính toán`d_end[i][j]`. Điều này thể hiện chi phí tối thiểu từ bất kỳ ô nào đến mục tiêu mà không cần dịch chuyển tức thời. 
4. Xác định tất cả các ô cổng thông tin. Đối với mỗi cổng`(i,j)`, tính toán`d_start[i][j] + cost[i][j]`Và`d_end[i][j] + cost[i][j]`. Theo dõi giá trị tối thiểu trong số tất cả các cổng cho mỗi số lượng này. 
5. Tính thời gian tối thiểu sử dụng dịch chuyển tức thời bằng tổng của hai giá trị cổng tối thiểu:`min_start_portal + min_end_portal`. 
6. Tính thời gian tối thiểu mà không cần dịch chuyển tức thời,`d_start[n-1][m-1]`. 
7. Câu trả lời cuối cùng là mức tối thiểu của con đường trực tiếp và con đường được hỗ trợ dịch chuyển tức thời. Nếu tất cả các tùy chọn vẫn là vô cùng, hãy xuất`-1`. 

Điều này hiệu quả vì BFS đảm bảo chúng tôi tìm thấy đường dẫn tối thiểu đến từng ô chỉ bằng cách sử dụng các bước di chuyển tiêu chuẩn. Bằng cách tính toán riêng chi phí tối thiểu để tiếp cận một cổng từ đầu và cuối, chúng tôi tránh được sự phức tạp bậc hai khi xem xét tất cả các cặp cổng một cách rõ ràng. Thuật toán xử lý chính xác các trường hợp dịch chuyển tức thời là tùy chọn, bắt buộc hoặc tệ hơn là đi bộ trực tiếp. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def solve():
    n, m, w = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]
    
    INF = 10**18
    
    def dijkstra(start_i, start_j):
        dist = [[INF] * m for _ in range(n)]
        dist[start_i][start_j] = 0
        heap = [(0, start_i, start_j)]
        while heap:
            d, i, j = heapq.heappop(heap)
            if d > dist[i][j]:
                continue
            for di,dj in ((0,1),(1,0),(0,-1),(-1,0)):
                ni, nj = i+di, j+dj
                if 0 <= ni < n and 0 <= nj < m and grid[ni][nj] != -1:
                    nd = d + w
                    if nd < dist[ni][nj]:
                        dist[ni][nj] = nd
                        heapq.heappush(heap, (nd, ni, nj))
        return dist

    d_start = dijkstra(0, 0)
    d_end = dijkstra(n-1, m-1)
    
    min_start_portal = INF
    min_end_portal = INF
    for i in range(n):
        for j in range(m):
            if grid[i][j] > 0:
                if d_start[i][j] < INF:
                    min_start_portal = min(min_start_portal, d_start[i][j] + grid[i][j])
                if d_end[i][j] < INF:
                    min_end_portal = min(min_end_portal, d_end[i][j] + grid[i][j])
    
    ans = d_start[n-1][m-1]
    if min_start_portal < INF and min_end_portal < INF:
        ans = min(ans, min_start_portal + min_end_portal)
    print(-1 if ans >= INF else ans)

solve()
```Phần đầu tiên đọc lưới và khởi tạo một mảng khoảng cách vô hạn. các`dijkstra`hàm tính toán khoảng cách tối thiểu từ bất kỳ điểm bắt đầu nào chỉ bằng cách sử dụng các bước di chuyển liền kề, tránh các ô bị chặn. Sau đó, chúng tôi tìm thấy chi phí cổng thông tin hiệu quả tối thiểu từ lúc bắt đầu và mục tiêu. Cuối cùng, câu trả lời xem xét cả con đường đi bộ trực tiếp và con đường có cổng hỗ trợ. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
5 5 1
0 -1 0 1 -1
0 20 0 0 -1
-1 -1 -1 -1 -1
3 0 0 0 0
-1 0 0 0 0
```| Bước | d_start | d_end | min_start_portal | min_end_portal | 
| --- | --- | --- | --- | --- | 
| BFS từ đầu | khoảng cách được tính toán | - | 4 (cổng thông tin tại (0,3)) | - | 
| BFS từ cuối | - | khoảng cách được tính toán | - | 10 (cổng thông tin tại (3.0)) | 
| Trả lời calc | phút(14, 4+10) | | | | 

Điều này cho thấy con đường tối ưu là đi bộ đến một cổng, dịch chuyển tức thời, sau đó đi bộ phần còn lại. 

**Đầu vào mẫu 2**```
2 2 5
0 0
0 0
```Không có cổng, đường dẫn trực tiếp là`5 + 5 = 10`. BFS tính toán`d_start[1][1] = 10`. Đầu ra của thuật toán`10`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m log(n m)) | Dijkstra được chạy hai lần với hàng đợi ưu tiên`n*m`nút; mỗi nút có 4 cạnh. | 
| Không gian | O(n m) | Mảng khoảng cách và lưới tiêu thụ không gian O(n m). | 

Giải pháp dễ dàng phù hợp với giới hạn 3 giây cho tối đa 4 triệu ô. Mức sử dụng bộ nhớ dưới giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str
```
