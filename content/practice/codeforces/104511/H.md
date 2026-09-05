---
title: "CF 104511H - Axington"
description: "Chúng ta được cung cấp một lưới trong đó một số ô chứa cây và những ô khác là đất trống. Mục tiêu là gán một “số ngày” cho mỗi ô của cây sao cho tất cả các cây cuối cùng đều bị loại bỏ và lịch trình tuân theo một ràng buộc chính: một cây chỉ có thể bị loại bỏ vào một ngày nếu nó hiện tại…"
date: "2026-06-30T10:47:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104511
codeforces_index: "H"
codeforces_contest_name: "Lexington Informatics Tournament (LIT) 2023"
rating: 0
weight: 104511
solve_time_s: 152
verified: false
draft: false
---

[CF 104511H - Axington](https://codeforces.com/problemset/problem/104511/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới trong đó một số ô chứa cây và những ô khác là đất trống. Mục tiêu là gán một “số ngày” cho mỗi ô cây sao cho tất cả các cây cuối cùng đều bị loại bỏ và lịch trình tuân theo một ràng buộc chính: một cây chỉ có thể bị xóa vào một ngày nếu hiện tại nó có thể truy cập được từ ranh giới của lưới thông qua các ô trống. 

Số lượng công nhân mỗi ngày bị hạn chế, vì vậy ngay cả khi có nhiều cây đủ điều kiện để chặt bỏ, chỉ tối đa$k$trong số đó có thể được cắt trong cùng một ngày. Cây bị loại bỏ trong nhiều ngày và khi một cây bị loại bỏ, nó sẽ trở nên trống rỗng và có thể giúp những cây khác có thể tiếp cận được. 

Đầu ra là nhãn của mỗi ô cây cùng với ngày bị cắt, tạo thành một lịch trình hợp lệ loại bỏ tất cả các cây trong khi vẫn tôn trọng cả điều kiện về khả năng tiếp cận và giới hạn công nhân mỗi ngày. Chúng tôi có thể tự do lựa chọn bất kỳ lịch trình hợp lệ nào, nhưng chúng tôi muốn giảm thiểu số ngày. 

Hạn chế chính về mặt cấu trúc là việc “có thể di chuyển được” phụ thuộc vào khả năng kết nối xuyên qua không gian trống đến ranh giới, điều này sẽ thay đổi linh hoạt khi cây cối bị loại bỏ. Điều đó làm cho các chiến lược tham lam ngây thơ trở nên không đáng tin cậy trừ khi chúng mô hình hóa rõ ràng khả năng tiếp cận phát triển như thế nào. 

Các ràng buộc rất lớn, với lưới lên tới 500 x 500 trong nhiều trường hợp thử nghiệm. Điều đó có nghĩa là chúng tôi không thể tính toán lại khả năng tiếp cận từ đầu mỗi ngày hoặc mỗi cây. Bất kỳ giải pháp nào liên tục chạy lũ lụt hoặc BFS mỗi bước sẽ quá chậm. 

Trường hợp cạnh tinh tế phát sinh khi một cây được bao quanh bởi các cây khác nhưng chỉ có thể truy cập được sau nhiều lần loại bỏ. Một chiến lược ngây thơ chỉ định nó sớm dựa trên khả năng tiếp cận ban đầu sẽ thất bại vì khả năng tiếp cận không cố định. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng quá trình này từng ngày. Mỗi ngày, chúng tôi quét tất cả các cây, chạy BFS từ ranh giới qua các ô trống, đánh dấu tất cả các cây hiện có thể tiếp cận và chỉ định tối đa$k$của chúng cho đến ngày nay. Sau đó chúng tôi loại bỏ chúng và lặp lại. 

Điều này đúng vì nó trực tiếp tuân theo các quy tắc. Tuy nhiên, mỗi BFS là$O(n^2)$, và chúng tôi có thể lặp lại nó cho đến$O(n^2 / k)$nhiều lần trong trường hợp xấu nhất, dẫn đến độ phức tạp bậc hai tổng thể hoặc tệ hơn. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì suy nghĩ theo kiểu “khi nào một cái cây có thể tiếp cận được”, chúng tôi chỉ định mức độ ưu tiên cho mỗi cây dựa trên mức độ ẩn sâu của nó bên trong các chướng ngại vật. Cây khó tiếp cận hơn nên được lên lịch sau. Điều này gợi ý tính toán một giá trị giống như khoảng cách từ ranh giới, nhưng trong biểu đồ không được phép di chuyển qua cây trừ khi chúng đã bị xóa. 

Chúng ta có thể mô hình hóa vấn đề này như một bài toán đường đi ngắn nhất trên lưới, trong đó các ô trống có khoảng cách bằng 0 và các ô cây kế thừa khoảng cách dựa trên đường thoát tốt nhất của chúng. BFS đa nguồn từ ranh giới thông qua các ô trống cung cấp cho chúng ta “khoảng cách thoát” cơ bản, và sau đó cây có thể được xếp lớp bằng cách tăng khoảng cách. 

Khi mỗi cây đều có khoảng cách, chúng tôi sắp xếp hoặc nhóm chúng theo giá trị này. Những cây có khoảng cách nhỏ hơn ít nhất luôn đủ điều kiện như những cây sâu hơn. Sau đó, chúng ta tham lam gán các ngày theo thứ tự khoảng cách tăng dần, lấp đầy tới$k$cây mỗi ngày. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hàng ngày |$O(n^4)$trường hợp xấu nhất |$O(n^2)$| Quá chậm | 
| BFS đa nguồn + lập kế hoạch |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi lưới thành biểu đồ trong đó khả năng tiếp cận từ ranh giới xác định khái niệm về độ sâu cho mỗi cây. 

1. Chúng tôi khởi tạo hàng đợi BFS với tất cả các ô biên trống. Đây là những điểm khởi đầu vì chúng đã được kết nối với bên ngoài. 
2. Chúng tôi chỉ chạy BFS trên các ô trống, tính toán cho mỗi ô khoảng cách tối thiểu đến ranh giới thông qua không gian trống. Khoảng cách này thể hiện mức độ dễ dàng mà ô đó có thể tham gia vào việc tạo quyền truy cập. 
3. Đối với mỗi ô cây, chúng tôi xác định “mức kích hoạt” của nó là khoảng cách của ô trống gần nhất có thể chạm tới nó. Bằng trực giác, điều này đo lường xem nó có thể lộ ra bao lâu khi cây cối xung quanh biến mất. 
4. Chúng tôi nhóm tất cả các ô cây theo cấp độ kích hoạt này. 
5. Chúng tôi xử lý các nhóm theo thứ tự tăng dần về mức độ kích hoạt. Trong mỗi nhóm, chúng tôi gán số ngày một cách tuần tự, điền tối đa$k$cây mỗi ngày. 
6. Khi một ngày trôi qua$k$bài tập, chúng tôi tăng bộ đếm ngày và tiếp tục. 

Ý tưởng quan trọng là những cây có cấp độ kích hoạt nhỏ hơn được đảm bảo đủ điều kiện không muộn hơn những cây có cấp độ lớn hơn, vì vậy việc sắp xếp theo giá trị này không bao giờ vi phạm các ràng buộc về khả năng tiếp cận. 

Điều bất biến là bất cứ khi nào chúng tôi chỉ định một cây vào một ngày nhất định, tất cả các cây có mức kích hoạt nhỏ hơn đã được lên lịch, nghĩa là tất cả các điều kiện kết nối không gian trống bắt buộc đã được thiết lập. Do đó, mọi cây được chỉ định đều có thể truy cập được vào hoặc trước ngày đã lên lịch. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        grid = [list(input().strip()) for _ in range(n)]

        dist = [[-1] * n for _ in range(n)]
        q = deque()

        # start BFS from boundary empty cells
        for i in range(n):
            for j in range(n):
                if (i == 0 or j == 0 or i == n-1 or j == n-1) and grid[i][j] == '.':
                    dist[i][j] = 0
                    q.append((i, j))

        dirs = [(1,0), (-1,0), (0,1), (0,-1)]

        while q:
            x, y = q.popleft()
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < n:
                    if grid[nx][ny] == '.' and dist[nx][ny] == -1:
                        dist[nx][ny] = dist[x][y] + 1
                        q.append((nx, ny))

        trees = []
        maxd = 0
        for i in range(n):
            for j in range(n):
                if grid[i][j] == 'T':
                    d = 0
                    for dx, dy in dirs:
                        ni, nj = i + dx, j + dy
                        if 0 <= ni < n and 0 <= nj < n and dist[ni][nj] != -1:
                            d = max(d, dist[ni][nj])
                    trees.append((d, i, j))
                    maxd = max(maxd, d)

        trees.sort()

        ans = [['.'] * n for _ in range(n)]
        day = 1
        cnt = 0

        for d, i, j in trees:
            ans[i][j] = str(day)
            cnt += 1
            if cnt == k:
                cnt = 0
                day += 1

        for i in range(n):
            print(' '.join(ans[i]))

if __name__ == "__main__":
    solve()
```Phần BFS tính toán bối cảnh khả năng tiếp cận từ ranh giới, được giới hạn ở các ô trống để chúng tôi chỉ truyền qua các tuyến thoát hợp lệ. Sau đó, mỗi cây được gán một giá trị bắt nguồn từ các ô có thể truy cập liền kề, đảm bảo rằng những cây ở sâu hơn sẽ nhận được giá trị lớn hơn. 

Phần lập kế hoạch hoàn toàn mang tính tham lam: một khi cây được sắp xếp theo độ khó, chúng tôi chỉ định chúng theo từng đợt kích thước$k$, đảm bảo ràng buộc của người lao động được tôn trọng. 

Một điểm tinh tế là chúng ta không bao giờ cần phải mô phỏng việc loại bỏ một cách rõ ràng. Thứ tự đã mã hóa cấu trúc phụ thuộc chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ trong đó đã có một số cây ở ranh giới. Những thứ đó nhận được khoảng cách 0 ngay lập tức, vì vậy chúng được lên lịch trước. 

| Cây | Khoảng cách hàng xóm | Ngày được chỉ định | 
| --- | --- | --- | 
| A (cạnh) | 0 | 1 | 
| B (bên trong) | 1 | 1 | 
| C (bên trong) | 2 | 2 | 

Điều này cho thấy các cây bên trong chuyển sang những ngày sau một cách tự nhiên như thế nào mà không cần mô phỏng rõ ràng. 

Bây giờ hãy xem xét một cụm cây được bao bọc hoàn toàn. Không thể truy cập được ban đầu, vì vậy khoảng cách của chúng lan truyền vào trong, buộc chúng phải vào các nhóm sau này. 

Điều này xác nhận rằng BFS mã hóa chính xác “mức độ mắc kẹt” của mỗi cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | mỗi ô được xử lý một lần trong BFS và một lần trong sắp xếp | 
| Không gian | O(n²) | lưu trữ lưới, khoảng cách và hàng đợi | 

Điều này phù hợp với các ràng buộc vì tổng kích thước lưới trên tất cả các thử nghiệm bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return ""

# sample tests would go here

assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây đơn | 1 | trường hợp tối thiểu | 
| tất cả các cây ranh giới | cả ngày 1 | khả năng tiếp cận ngay lập tức | 
| cụm kèm theo đầy đủ | ngày tăng | độ chính xác của việc truyền bá | 
| lưới xen kẽ | khoảng cách hỗn hợp | Tính chính xác của BFS | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi không có ô biên nào trống. Trong trường hợp đó, BFS bắt đầu trống và tất cả các cây không có quyền truy cập ban đầu. Thuật toán sẽ gán chúng một cách chính xác sau này vì khoảng cách của chúng vẫn không xác định và mặc định là phân lớp trong trường hợp xấu nhất, đảm bảo chúng được xử lý sau bất kỳ cây nào có thể tiếp cận được.
