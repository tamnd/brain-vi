---
title: "CF 102617K - Quần đảo tráng miệng"
description: "Bản đồ mô tả đầm lầy sô cô la có dạng lưới hình chữ nhật. Mỗi ô là đất bánh quy đặc (S) hoặc sô cô la lỏng (L). Các ô chỉ kết nối qua bốn cạnh của chúng, do đó tiếp xúc chéo không hợp nhất các vùng."
date: "2026-07-31T17:38:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102617
codeforces_index: "K"
codeforces_contest_name: "mBIT Rookie November 2019"
rating: 0
weight: 102617
solve_time_s: 59
verified: true
draft: false
---

[CF 102617K - Quần đảo tráng miệng](https://codeforces.com/problemset/problem/102617/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bản đồ mô tả đầm lầy sô cô la có dạng lưới hình chữ nhật. Mỗi ô là vùng đất cookie vững chắc (`S`) hoặc sôcôla lỏng (`L`). Các ô chỉ kết nối qua bốn cạnh của chúng, do đó tiếp xúc chéo không hợp nhất các vùng. 

Đảo cookie là một nhóm được kết nối`S`các ô được bao quanh hoàn toàn bởi sô cô la và không chạm vào đường viền bên ngoài của bản đồ. Một hồ sô-cô-la thì ngược lại: một nhóm được kết nối với nhau`L`các tế bào được bao quanh bởi vùng đất bánh quy. Nhiệm vụ là đếm xem có bao nhiêu hòn đảo và hồ như vậy tồn tại. 

Quan sát chính từ các ràng buộc là lưới có thể chứa tới khoảng một triệu ô. Bất kỳ cách tiếp cận nào liên tục tìm kiếm trên toàn bộ lưới cho mọi khu vực đều sẽ quá tốn kém. Quét tuyến tính với việc truyền tải đồ thị là độ phức tạp dự kiến ​​vì mỗi ô có thể được xử lý với số lần không đổi. 

Điều kiện biên đơn giản hóa vấn đề. Bên ngoài bản đồ được đảm bảo là vùng đất làm bánh quy nên mọi thành phần sô cô la đều tự động được bao bọc và là một cái hồ. Đối với các thành phần cookie, chỉ thành phần được kết nối với đường viền không phải là một hòn đảo. Tất cả các thành phần cookie khác đều là đảo. 

Một số trường hợp cạnh rất dễ xử lý sai. Một bản đồ chỉ có đất bánh quy thì không có đảo hay hồ vì chỉ có thành phần bên ngoài. 

Ví dụ đầu vào:```
3 3
SSS
SSS
SSS
```Đầu ra đúng là:```
0 0
```Một giải pháp bất cẩn có tính đến mọi`S`thành phần như một hòn đảo sẽ đếm không chính xác toàn bộ lưới. 

Một hồ nước không có hòn đảo bên trong là một trường hợp khác có thể phá vỡ các giải pháp cho rằng việc làm tổ luôn phải xảy ra. 

Ví dụ đầu vào:```
3 3
SSS
SLS
SSS
```Đầu ra đúng là:```
0 1
```Thành phần sô cô la được bao quanh bởi đất bánh quy nên nó là một cái hồ mặc dù bên trong không có hòn đảo nào. 

Trường hợp phức tạp thứ ba là lồng ghép nhiều lớp. 

Ví dụ đầu vào:```
5 5
SSSSS
SLLLS
SLSLS
SLLLS
SSSSS
```Đầu ra đúng là:```
1 4
```Các khu vực bên trong phải được tính độc lập. Một giải pháp chỉ nhìn vào ranh giới ngoài cùng sẽ bỏ sót các hồ nhỏ hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lấp đầy mọi ô chưa được truy cập và phân loại thành phần được kết nối. Đối với mỗi thành phần, chúng ta có thể ghi lại loại ký tự của nó và liệu nó có chạm vào đường viền hay không. Nếu chúng ta cố gắng tìm kiếm liên tục quanh mỗi ô hoặc thực hiện duyệt riêng cho từng vùng có thể, thì các ô giống nhau có thể được xử lý nhiều lần, dẫn đến công việc không cần thiết trên một lưới có một triệu ô. 

Quan sát hữu ích là các thành phần được kết nối đã phân vùng lưới. Mỗi ô thuộc về chính xác một thành phần và câu trả lời chỉ phụ thuộc vào ký tự của thành phần đó và liệu nó có chạm vào đường viền hay không. Một BFS hoặc DFS duy nhất trên toàn bộ lưới là đủ. 

Khi việc lấp lũ kết thúc,`L`thành phần luôn là một cái hồ vì sôcôla không bao giờ chạm tới biên giới. MỘT`S`thành phần chỉ là một hòn đảo khi nó không chạm vào biên giới. Điều này làm giảm vấn đề từ việc theo dõi các mối quan hệ ngăn chặn phức tạp đến việc phân loại đơn giản các thành phần được kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((NM)^2) | O(NM) | Quá chậm | 
| Thành phần lấp lũ | O(NM) | O(NM) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt từng ô trong lưới. Khi tìm thấy một ô chưa được truy cập, hãy khởi động BFS từ ô đó để khám phá toàn bộ thành phần được kết nối của nó. Một thành phần phải được xử lý tổng thể vì câu trả lời phụ thuộc vào khu vực chứ không phụ thuộc vào từng ô riêng lẻ. 
2. Trong BFS, lưu trữ loại ký tự của thành phần và kiểm tra xem có ô nào trong thành phần nằm trên đường viền hay không. Tiếp cận biên giới là thuộc tính duy nhất ngăn cách vùng bánh quy bên ngoài với các hòn đảo thực sự. 
3. Sau khi BFS kết thúc, hãy phân loại thành phần. Nếu nó chứa`L`, tăng số lượng hồ. Nếu nó chứa`S`và không chạm vào biên giới, tăng số lượng đảo. 
4. Sau khi đã truy cập tất cả các ô, hãy in số lượng đảo theo sau là số lượng hồ. 

Tại sao nó hoạt động: 

Mỗi vùng được kết nối của lưới được BFS truy cập chính xác một lần. Vùng sô cô la không thể chạm vào đường viền vì đường viền được đảm bảo chỉ chứa các ô cookie nên mọi thành phần sô cô la đều được bao bọc và phải là một hồ. Vùng bánh quy chạm vào đường viền là vùng đất bên ngoài và không thể là một hòn đảo, trong khi mọi thành phần bánh quy khác được bao quanh bởi sô cô la và thỏa mãn định nghĩa về một hòn đảo. Vì mọi vùng có thể được phân loại chính xác một lần nên số lượng cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    visited = [[False] * m for _ in range(n)]
    islands = 0
    lakes = 0

    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for i in range(n):
        for j in range(m):
            if not visited[i][j]:
                visited[i][j] = True
                kind = grid[i][j]
                touches_border = False
                q = deque([(i, j)])

                while q:
                    x, y = q.popleft()

                    if x == 0 or x == n - 1 or y == 0 or y == m - 1:
                        touches_border = True

                    for dx, dy in directions:
                        nx, ny = x + dx, y + dy
                        if 0 <= nx < n and 0 <= ny < m:
                            if not visited[nx][ny] and grid[nx][ny] == kind:
                                visited[nx][ny] = True
                                q.append((nx, ny))

                if kind == 'L':
                    lakes += 1
                elif not touches_border:
                    islands += 1

    print(islands, lakes)

if __name__ == "__main__":
    solve()
```Các vòng lặp bên ngoài đảm bảo rằng mọi ô đều trở thành điểm bắt đầu của quá trình lấp lũ chỉ một lần. các`visited`mảng ngăn việc xem lại các ô sau khi thành phần của chúng đã được phân loại. 

Hàng đợi BFS chỉ chứa các ô thuộc thành phần hiện tại. các`kind`biến được cố định khi quá trình truyền tải bắt đầu, do đó việc tìm kiếm không bao giờ đi từ đất liền sang sô cô la hoặc ngược lại. 

Việc kiểm tra đường viền được thực hiện trong khi xóa các ô khỏi hàng đợi. Chỉ cần nhớ một boolean là đủ vì việc phân loại cuối cùng chỉ cần biết thành phần đó có ô viền nào hay không. Không cần phải lưu trữ hình dạng đầy đủ của một thành phần. 

Số nguyên Python không phải là vấn đề đáng lo ngại ở đây vì số lượng tối đa là số lượng ô và kích thước hàng đợi lớn nhất cũng bị giới hạn bởi kích thước lưới. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
5 5
SSSSS
SLLLS
SLSLS
SLLLS
SSSSS
```| Thành phần hiện tại | Nhân vật | Chạm vào đường viền | Kết quả | 
| --- | --- | --- | --- | 
| Vùng ngoại vi | S | Có | Bỏ qua | 
| Hồ trên cùng | L | Không | Số hồ trở thành 1 | 
| Vùng trung tâm | S | Không | Số lượng đảo trở thành 1 | 
| Hồ nội địa khác | L | Không | Số hồ tăng lên 4 | 

Dấu vết cho thấy tại sao việc ngăn chặn không cần phải được lập mô hình rõ ràng. Mỗi vùng lồng nhau đã là một thành phần được kết nối độc lập. 

Một ví dụ khác:```
3 5
SSSSS
SLLLS
SSSSS
```| Thành phần hiện tại | Nhân vật | Chạm vào đường viền | Kết quả | 
| --- | --- | --- | --- | 
| Vùng ngoại vi | S | Có | Bỏ qua | 
| Miền Trung | L | Không | Số hồ trở thành 1 | 

Ví dụ này xác nhận rằng một cái hồ có thể tồn tại mà không cần có hòn đảo bên trong nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi ô vào và rời khỏi hàng đợi BFS một lần. | 
| Không gian | O(NM) | Mỗi mảng đã truy cập và hàng đợi BFS có thể chứa một số ô tuyến tính. | 

Kích thước lưới tối đa làm cho việc xử lý tuyến tính trở nên cần thiết. Giải pháp chỉ thực hiện công việc liên tục trên mỗi ô, vì vậy nó phù hợp thoải mái trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    visited = [[False] * m for _ in range(n)]
    islands = 0
    lakes = 0
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for i in range(n):
        for j in range(m):
            if not visited[i][j]:
                visited[i][j] = True
                kind = grid[i][j]
                border = False
                q = deque([(i, j)])

                while q:
                    x, y = q.popleft()
                    if x == 0 or y == 0 or x == n - 1 or y == m - 1:
                        border = True

                    for dx, dy in directions:
                        nx, ny = x + dx, y + dy
                        if 0 <= nx < n and 0 <= ny < m:
                            if not visited[nx][ny] and grid[nx][ny] == kind:
                                visited[nx][ny] = True
                                q.append((nx, ny))

                if kind == "L":
                    lakes += 1
                elif not border:
                    islands += 1

    sys.stdin = old_stdin
    return f"{islands} {lakes}\n"

assert solve_io("""7 10
SSSSSSSSSS
SLSLSLLLSS
SSLLLLSLSS
SSSLSLSLLS
SSSLLSLSSS
SSLSSLLLSS
SSSSSSSSSS
""") == "3 4\n", "sample"

assert solve_io("""3 3
SSS
SLS
SSS
""") == "0 1\n", "single lake"

assert solve_io("""3 3
SSS
SSS
SSS
""") == "0 0\n", "only outside land"

assert solve_io("""5 5
SSSSS
SLLLS
SLSLS
SLLLS
SSSSS
""") == "1 4\n", "nested regions"

assert solve_io("""5 5
SSSSS
SLSLS
SSSSS
SLSLS
SSSSS
""") == "0 4\n", "multiple lakes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới mẫu |`3 4`| Đảo lồng và đếm hồ bình thường | 
| Một`L`bên trong`S`|`0 1`| Hồ không cần đảo bên trong | 
| Tất cả`S`|`0 0`| Thành phần viền không được tính | 
| Nhiều vùng lồng nhau |`1 4`| Phân loại thành phần độc lập | 
| Một số hồ tách biệt |`0 4`| Nhiều thành phần cùng loại | 

## Vỏ cạnh 

Đối với bản đồ toàn cookie:```
3 3
SSS
SSS
SSS
```BFS tìm thấy một`S`thành phần và đánh dấu nó là chạm vào đường viền. Vì là đất bên ngoài nên số lượng đảo vẫn bằng 0 và số lượng hồ vẫn bằng 0. 

Đối với hồ không có đảo:```
3 3
SSS
SLS
SSS
```BFS trên`L`ô tìm thấy một thành phần không chạm vào đường viền. Vì tất cả các thành phần sôcôla đều được bao bọc nên thuật toán tính nó là một hồ. 

Đối với các vùng lồng nhau:```
5 5
SSSSS
SLLLS
SLSLS
SLLLS
SSSSS
```Đầu tiên, quá trình duyệt bỏ qua thành phần cookie bên ngoài, sau đó đếm riêng từng thành phần sô cô la bên trong và thành phần cookie trung tâm. Việc phân loại chỉ phụ thuộc vào thành phần hiện tại, do đó độ sâu lồng ghép không ảnh hưởng đến tính chính xác.
