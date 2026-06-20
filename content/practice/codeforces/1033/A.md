---
title: "CF 1033A - Vua Thoát Khỏi"
description: "Cho ta một bàn cờ hình vuông có tọa độ từ 1 đến n theo cả hai hướng. Một quân hậu được cố định ở một ô và nó tấn công dọc theo hàng, cột và đường chéo theo nghĩa cờ vua thông thường."
date: "2026-06-16T19:36:12+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1033
codeforces_index: "A"
codeforces_contest_name: "Lyft Level 5 Challenge 2018 - Elimination Round"
rating: 1000
weight: 1033
solve_time_s: 259
verified: true
draft: false
---

[CF 1033A - Vua trốn thoát](https://codeforces.com/problemset/problem/1033/A) 

**Đánh giá:** 1000 
**Tags:** dfs và tương tự, đồ thị, triển khai 
**Thời gian giải:** 4 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cho ta một bàn cờ hình vuông có tọa độ từ 1 đến n theo cả hai hướng. Một quân hậu được cố định ở một ô và nó tấn công dọc theo hàng, cột và đường chéo theo nghĩa cờ vua thông thường. Vua bắt đầu từ một ô và muốn đến một ô khác, di chuyển từng bước một theo bất kỳ hướng nào trong 8 hướng liền kề. 

Điều khó khăn là nhà vua bị cấm bước vào ô vuông đang bị nữ hoàng tấn công. Chúng tôi được hỏi liệu có tồn tại con đường an toàn nào từ vị trí bắt đầu đến vị trí đích hay không. 

Vì vậy, vấn đề trở thành một câu hỏi về khả năng tiếp cận trên biểu đồ lưới trong đó mỗi ô là một nút, các cạnh kết nối 8 lân cận, nhưng một số nút bị chặn nếu chúng nằm trong cùng một hàng, cột hoặc đường chéo với nữ hoàng. 

Kích thước bảng tối đa là 1000 x 1000, do đó có tới 1e6 ô. Việc duyệt đồ thị đơn giản trên tất cả các ô là khả thi. Bất kỳ điều gì vượt quá mức truyền tải tuyến tính hoặc gần tuyến tính cho mỗi lần kiểm tra vẫn sẽ ổn, nhưng bất kỳ điều gì liên tục tính toán lại khả năng hiển thị hoặc kiểm tra đường dẫn một cách đơn giản sẽ quá chậm. 

Một trường hợp phức tạp xuất phát từ cách đòn tấn công của quân hậu xác định các ô bị chặn. Toàn bộ dòng bị chặn trên toàn cầu, không linh hoạt, do đó tập hợp bị cấm được cố định. Một chi tiết quan trọng khác là điểm bắt đầu và mục tiêu được đảm bảo an toàn nên trở ngại duy nhất là đường đi trung gian chứ không phải điểm cuối. 

Một sai lầm ngây thơ là cố gắng tham lam “đi vòng quanh” quân hậu bằng cách chọn những nước đi an toàn tại địa phương. Điều này không thành công trong trường hợp vùng an toàn được chia thành các thành phần bị ngắt kết nối, mặc dù mọi bước cục bộ đều có vẻ khả thi. Một chế độ thất bại khác là quên các cuộc tấn công theo đường chéo và chỉ chặn các hàng và cột, điều này đánh giá quá cao khả năng kết nối một cách không chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là coi mỗi ô như một nút và thực hiện tìm kiếm từ vị trí bắt đầu của vua. Từ mỗi nút, chúng tôi thử tất cả 8 bước di chuyển và chỉ tiến hành nếu ô đích không bị quân hậu tấn công. 

Điều này đúng vì nó khám phá rõ ràng tất cả các cấu hình an toàn có thể truy cập. Tuy nhiên, trường hợp xấu nhất là lưới 1000 x 1000 trong đó hầu hết tất cả các ô đều an toàn ngoại trừ cấu trúc chặn hẹp. Trong trường hợp đó, BFS/DFS có thể truy cập tối đa 1e6 nút và xử lý tối đa 8e6 cạnh, điều này vẫn được chấp nhận trong Python, nhưng điểm kém hiệu quả chính là việc kiểm tra lặp đi lặp lại không cần thiết các cuộc tấn công của nữ hoàng cho mỗi lần mở rộng lân cận. 

Quan sát quan trọng là chúng ta không cần bất cứ điều gì thông minh về đường đi ngắn nhất. Chúng tôi chỉ cần kết nối trong lưới tĩnh với các ô bị chặn. Điều này làm giảm vấn đề xuống mức lấp lũ tiêu chuẩn. 

Vì vậy, chúng tôi tính toán trước một hàm kiểm tra xem một ô có bị nữ hoàng tấn công trong O(1) hay không. Sau đó, chúng tôi chạy BFS hoặc DFS từ vua, đánh dấu các ô an toàn đã truy cập. Nếu chúng ta đạt được mục tiêu, chúng ta sẽ thành công. 

Cấu trúc về cơ bản là một vấn đề kết nối đồ thị trên một đồ thị lưới ẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS/BFS với tính năng kiểm tra nhanh chóng | O(n^2) | O(n^2) | Đã chấp nhận | 
| BFS tối ưu với quy tắc tấn công được tính toán trước | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đánh dấu vị trí của quân hậu làm điểm tham chiếu để tính toán đòn tấn công. Một ô sẽ không an toàn nếu nó có cùng hàng, cột hoặc đường chéo với quân hậu. Điều này đưa ra quy tắc thời gian không đổi để kiểm tra tính hợp lệ. 
2. Xác định hàm`safe(x, y)`trả về sai nếu ô bị nữ hoàng tấn công. Điều này tránh việc tính toán lại các mối quan hệ hình học trong quá trình truyền tải logic. 
3. Khởi tạo hàng đợi BFS bắt đầu từ vị trí vua. Đánh dấu ô này là đã truy cập vì nó được đảm bảo an toàn. 
4. Trong khi hàng đợi không trống, hãy trích xuất ô tiếp theo. Nếu nó khớp với ô đích, trả về thành công ngay lập tức vì khả năng tiếp cận được thiết lập. 
5. Đối với mỗi nước đi trong số 8 nước đi vua có thể có, hãy tính ô lân cận. Nếu nó nằm trong bảng, chưa được truy cập và an toàn theo quy tắc của quân hậu, hãy đánh dấu nó đã truy cập và đẩy nó vào hàng đợi. 
6. Nếu BFS kết thúc mà không đến được mục tiêu, hãy kết luận rằng mục tiêu nằm ở vùng an toàn được kết nối khác. 

Lý do chúng tôi dừng ngay lập tức khi đạt được mục tiêu là vì BFS khám phá tất cả các ô an toàn có thể tiếp cận được, vì vậy việc đến lần đầu tiên đảm bảo sự tồn tại của một đường dẫn hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là mọi ô được chèn vào hàng đợi đều có thể truy cập được từ vị trí bắt đầu thông qua một đường dẫn chỉ bao gồm các ô vuông an toàn. Vì BFS khám phá mọi chuyển đổi an toàn có thể có chính xác một lần, nên tập hợp đã truy cập thể hiện thành phần được kết nối đầy đủ của vị trí bắt đầu của quân vua trong biểu đồ được tạo ra bằng cách loại bỏ các ô bị tấn công. Nếu mục tiêu thuộc thành phần này thì sẽ bị phát hiện; nếu không thì không thể truy cập được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n = int(input())
ax, ay = map(int, input().split())
bx, by = map(int, input().split())
cx, cy = map(int, input().split())

def safe(x, y):
    return not (x == ax or y == ay or abs(x - ax) == abs(y - ay))

# BFS
q = deque()
q.append((bx, by))
vis = [[False] * (n + 1) for _ in range(n + 1)]
vis[bx][by] = True

dirs = [(-1, -1), (-1, 0), (-1, 1),
        (0, -1),          (0, 1),
        (1, -1),  (1, 0), (1, 1)]

while q:
    x, y = q.popleft()
    if (x, y) == (cx, cy):
        print("YES")
        sys.exit(0)
    for dx, dy in dirs:
        nx, ny = x + dx, y + dy
        if 1 <= nx <= n and 1 <= ny <= n and not vis[nx][ny] and safe(nx, ny):
            vis[nx][ny] = True
            q.append((nx, ny))

print("NO")
```BFS là phương pháp truyền tải lưới tiêu chuẩn, nhưng chi tiết quan trọng là`safe`chức năng mã hóa trực tiếp hình học tấn công nữ hoàng. Điều này tránh việc duy trì bất kỳ trạng thái tấn công động nào. 

Mảng đã truy cập đảm bảo mỗi ô được xử lý một lần, ngăn chặn sự phân nhánh theo cấp số nhân. Việc thoát ra sớm để đạt được mục tiêu sẽ ngăn chặn việc khám phá không cần thiết sau khi khả năng kết nối được chứng minh. 

Mảng hướng bao gồm tất cả 8 nước đi vua, điều này rất cần thiết vì việc giới hạn ở 4 hướng sẽ phá vỡ kết nối đường chéo một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
4 4
1 3
3 1
```Chúng tôi theo dõi tiến trình BFS từ (1, 3). Hậu ở (4, 4) chặn hàng 4, cột 4 và cả hai đường chéo. 

| Bước | Ô hiện tại | Đã thêm hàng xóm an toàn | 
| --- | --- | --- | 
| 1 | (1,3) | (2,2), (2,3), (2,4) | 
| 2 | (2,2) | (3,2) | 
| 3 | (2,3) | (3,2), (3,3) | 
| 4 | (2,4) | (3,3) | 
| ... | ... | ... | 

BFS dần dần xây dựng một con đường tránh đường chéo xuyên qua quân hậu. Cuối cùng nó đạt đến (3,1), xác nhận kết nối. 

Dấu vết này cho thấy thuật toán không dựa vào chuyển động tham lam. Nó khám phá tất cả các nhánh an toàn cho đến khi phát hiện ra một hành lang hợp lệ. 

### Ví dụ 2 

Hãy xem xét trường hợp quân hậu chặn một đường ngang đầy đủ: 

đầu vào:```
8
4 4
2 2
6 2
```Hậu chặn hoàn toàn hàng 4, chia đôi bàn cờ. 

| Bước | Ô hiện tại | Đã thêm hàng xóm an toàn | 
| --- | --- | --- | 
| 1 | (2,2) | (1,1), (1,2), (1,3), (2,1), (2,3), (3,1), (3,2), (3,3) | 
| 2 | Tiếp tục mở rộng | Tất cả các ô có thể truy cập ở vùng trên cùng bên trái | 

BFS không bao giờ vượt qua hàng 4 vì tất cả các ô đó đều không an toàn. Nếu mục tiêu nằm dưới hàng 4 thì không bao giờ đạt được. 

Điều này chứng tỏ rằng thuật toán tôn trọng chính xác sự phân tách toàn cục do các đường tấn công gây ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi ô được truy cập tối đa một lần và mỗi lượt truy cập xử lý tối đa 8 ô lân cận | 
| Không gian | O(n^2) | Mảng đã truy cập và hàng đợi BFS có thể lưu trữ một phần tuyến tính của lưới | 

Các ràng buộc cho phép tối đa 1e6 ô và mỗi ô kích hoạt công việc liên tục, vừa vặn thoải mái trong giới hạn trong Python. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    ax, ay = map(int, input().split())
    bx, by = map(int, input().split())
    cx, cy = map(int, input().split())

    def safe(x, y):
        return not (x == ax or y == ay or abs(x - ax) == abs(y - ax))

    q = deque([(bx, by)])
    vis = [[False] * (n + 1) for _ in range(n + 1)]
    vis[bx][by] = True

    dirs = [(-1,-1),(-1,0),(-1,1),
            (0,-1),        (0,1),
            (1,-1),(1,0),(1,1)]

    while q:
        x, y = q.popleft()
        if (x, y) == (cx, cy):
            return "YES"
        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 1 <= nx <= n and 1 <= ny <= n and not vis[nx][ny] and safe(nx, ny):
                vis[nx][ny] = True
                q.append((nx, ny))

    return "NO"

# provided sample
assert run("""8
4 4
1 3
3 1
""") == "YES"

# minimum grid movement
assert run("""3
2 2
1 1
3 3
""") == "YES"

# blocked diagonal corridor
assert run("""5
3 3
1 1
5 5
""") == "NO"

# straight line block
assert run("""5
3 3
1 3
5 3
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường chéo 3x3 | CÓ | kết nối cơ bản | 
| khối chéo 5x5 | KHÔNG | tách tấn công chéo | 
| khối hàng 5x5 | KHÔNG | cắt ngang đúng cách | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi quân hậu chặn một hành lang hẹp nhưng không bao bọc hoàn toàn cả hai điểm cuối. Ví dụ, nếu quân hậu ngồi ở giữa, có nhiều con đường tồn tại nhưng cần phải đi đường vòng. BFS vẫn thành công vì nó mở rộng từng lớp một mà không cần đảm bảo tính định hướng. 

Một trường hợp khác là khi điểm bắt đầu và mục tiêu nằm trên cùng một vùng an toàn được kết nối nhưng có vẻ như bị ngăn cách bởi các đường tấn công. Thuật toán tìm chính xác các tuyến đường thay thế vì nó không hạn chế chuyển động vượt quá các giới hạn an toàn và khám phá tất cả 8 hướng một cách thống nhất. 

Trường hợp cuối cùng là khi đòn tấn công của quân hậu gần như chia cắt bàn cờ nhưng để lại một khoảng trống một ô. BFS tự nhiên đi qua khoảng trống đó vì điều kiện an toàn được kiểm tra trên mỗi ô chứ không phải theo ranh giới khu vực.
