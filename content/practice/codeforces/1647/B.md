---
title: "CF 1647B - Madoka Và Món Quà Thanh Lịch"
description: "Chúng tôi được cung cấp một lưới nhị phân. Hình chữ nhật con màu đen là bất kỳ hình chữ nhật thẳng hàng nào có chứa toàn bộ số 1. Trong số tất cả các hình chữ nhật màu đen, một hình chữ nhật được gọi là đẹp nếu nó không thể mở rộng thành hình chữ nhật màu đen lớn hơn chứa nó."
date: "2026-06-10T04:05:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1647
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 777 (Div. 2)"
rating: 1200
weight: 1647
solve_time_s: 118
verified: true
draft: false
---

[CF 1647B - Madoka và món quà thanh lịch](https://codeforces.com/problemset/problem/1647/B) 

**Đánh giá:** 1200 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, đồ thị, triển khai 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới nhị phân. Hình chữ nhật con màu đen là bất kỳ hình chữ nhật thẳng hàng nào bao gồm toàn bộ`1`S. 

Trong số tất cả các hình chữ nhật màu đen, một hình chữ nhật được gọi là đẹp nếu nó không thể mở rộng thành hình chữ nhật màu đen lớn hơn chứa nó. Nói cách khác, nó là tối đa đối với khả năng ngăn chặn. 

Lưới được gọi là thanh lịch khi không có hai hình chữ nhật đẹp nào chia sẻ bất kỳ ô nào. 

Thoạt nhìn, định nghĩa này nghe có vẻ phức tạp vì nó nói về tất cả những-`1`hình chữ nhật và các mối quan hệ ngăn chặn của chúng. Thách thức là xác định xem liệu các hình chữ nhật tối đa chồng chéo như vậy có thể tồn tại hay không. 

Kích thước lưới tối đa là`100 × 100`và tổng của tất cả số hàng và số cột trong các trường hợp thử nghiệm được giới hạn bởi`777`. Đây là kích thước đầu vào rất nhỏ. Thậm chí một`O(nm(n+m))`giải pháp có thể dễ dàng được chấp nhận. Điều không thể chấp nhận được là liệt kê tất cả các hình chữ nhật có thể có, bởi vì`100 × 100`lưới chứa khoảng`25 million`hình chữ nhật và việc kiểm tra từng cái sẽ quá tốn kém. 

Khó khăn chính là nhận ra định nghĩa của sự thanh lịch thực sự có nghĩa là gì về hình dạng được hình thành bởi`1`S. 

Một số trường hợp đặc biệt rất dễ bị hiểu sai. 

Coi như```
11
11
```Câu trả lời đúng là`YES`. 

Tất cả bốn ô tạo thành một hình chữ nhật màu đen tối đa duy nhất. Một cách tiếp cận ngây thơ chỉ kiểm tra xem có lân cận hay không`1`s tồn tại sẽ từ chối nó một cách không chính xác. 

Coi như```
10
01
```Câu trả lời đúng là`YES`. 

hai`1`s chỉ chạm theo đường chéo. Chúng thuộc về các hình chữ nhật tối đa riêng biệt và không giao nhau. 

Coi như```
11
10
```Câu trả lời đúng là`NO`. 

Ba`1`s tạo thành hình chữ L. Một hình chữ nhật tối đa là hàng trên cùng, một hình chữ nhật tối đa khác là cột bên trái. Chúng chồng lên nhau ở ô phía trên bên trái, do đó lưới không được trang nhã. 

Việc triển khai bất cẩn mà chỉ tính các thành phần được kết nối sẽ chấp nhận trường hợp này một cách không chính xác vì tất cả`1`s thuộc về một thành phần duy nhất. 

Một ví dụ quan trọng khác là```
111
101
111
```Câu trả lời đúng là`NO`. 

Lỗ ở giữa ngăn toàn bộ hình dạng trở thành hình chữ nhật. Nhiều hình chữ nhật màu đen tối đa chồng lên nhau xung quanh các góc, tạo ra các hình chữ nhật giao nhau đẹp mắt. 

Giải pháp đến từ việc hiểu chính xác hình dạng của`1`s được cho phép. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực tuân theo định nghĩa trực tiếp. 

Chúng ta có thể liệt kê mọi hình chữ nhật thẳng hàng với trục, kiểm tra xem nó có bao gồm toàn bộ`1`s, xác định xem nó có cực đại hay không, thu thập tất cả các hình chữ nhật đẹp và sau đó kiểm tra giao điểm của từng cặp. 

Lưới có kích thước`100 × 100`chứa khoảng$$\frac{100\cdot101}{2}\cdot\frac{100\cdot101}{2} \approx 25\,000\,000$$hình chữ nhật. 

Ngay cả trước khi kiểm tra nội dung của chúng, điều này đã vượt xa những gì có thể được xử lý trong thời gian giới hạn. 

Quan sát quan trọng là định nghĩa thực tế được rút gọn thành một thuộc tính hình học rất đơn giản. 

Lấy bất kỳ thành phần kết nối nào của`1`s sử dụng liền kề bốn hướng. 

Nếu thành phần đó tạo thành một hình chữ nhật được lấp đầy thì có chính xác một hình chữ nhật màu đen tối đa bên trong nó, đó là toàn bộ thành phần. Không có vấn đề chồng chéo tồn tại. 

Nếu thành phần không tạo thành một hình chữ nhật được tô đầy thì bên trong hộp giới hạn của nó phải có ít nhất một ô bị thiếu. Hình dạng như vậy nhất thiết phải có chỗ quay chữ L, một góc có đường chạy ngang`1`s và chạy dọc`1`sẽ gặp nhau. Hai lần chạy đó tạo ra các hình chữ nhật màu đen tối đa khác nhau chia sẻ ô góc. Do đó hai hình chữ nhật đẹp cắt nhau. 

Điều này có nghĩa là: 

Một lưới là đẹp khi và chỉ khi mọi thành phần được kết nối của`1`s tạo thành một hình chữ nhật hoàn chỉnh. 

Một khi chúng ta khám phá được đặc tính này, vấn đề sẽ trở nên rất nhỏ. Đối với mọi thành phần được kết nối, hãy tính hộp giới hạn của nó. Nếu thành phần chứa chính xác mọi ô của hộp giới hạn đó thì thành phần đó có dạng hình chữ nhật. Nếu không thì câu trả lời là ngay lập tức`NO`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hình chữ nhật cực lớn, khoảng O(n²m²) cộng với xác thực | Lớn | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại qua tất cả các ô của lưới. 
2. Bất cứ khi nào một ô chưa được thăm có chứa`1`được tìm thấy, hãy khởi động BFS hoặc DFS để khám phá toàn bộ thành phần được kết nối của nó. 
3. Trong khi duyệt thành phần, ghi lại: 

- Chỉ số hàng tối thiểu. 
- Chỉ số hàng tối đa. 
- Chỉ số cột tối thiểu. 
- Chỉ số cột tối đa. 
- Số lượng ô trong thành phần. 
4. Sau khi quá trình truyền tải kết thúc, hãy tính diện tích hình chữ nhật bao quanh của thành phần.```
area = (max_row - min_row + 1) × (max_col - min_col + 1)
```5. So sánh kích thước thành phần với diện tích hình chữ nhật bao quanh. 

Nếu chúng khác nhau thì một số ô bên trong hình chữ nhật bao quanh bị thiếu. Thành phần này không phải là một hình chữ nhật đầy màu sắc, vì vậy lưới không thể đẹp mắt. đầu ra`NO`. 
6. Tiếp tục xử lý tất cả các thành phần. 
7. Nếu mọi thành phần đều vượt qua quá trình kiểm tra, hãy xuất ra`YES`. 

Tại sao so sánh diện tích là đủ? Bởi vì mọi ô của thành phần đều nằm bên trong hộp giới hạn. Nếu số lượng ô thành phần bằng diện tích hộp thì mọi vị trí bên trong hộp phải thuộc về thành phần đó. Thành phần này chính xác là một hình chữ nhật. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là một thành phần được kết nối của`1`s an toàn chính xác khi nó là một hình chữ nhật được lấp đầy. 

Nếu một thành phần là một hình chữ nhật được tô đầy thì toàn bộ thành phần đó là một hình chữ nhật màu đen tối đa. Bất kỳ hình chữ nhật màu đen nhỏ hơn nào đều được chứa bên trong nó và không đẹp. Không có cặp hình chữ nhật đẹp nào có thể giao nhau. 

Nếu một thành phần không phải là một hình chữ nhật được tô màu thì hộp giới hạn của nó chứa ít nhất một ô bị thiếu. Hình dạng như vậy không thể được biểu diễn bằng một hình chữ nhật tối đa duy nhất. Cần có ít nhất hai hình chữ nhật màu đen tối đa và chúng chồng lên nhau ở đâu đó bên trong thành phần. Những hình chữ nhật tối đa chồng chéo đó là những hình chữ nhật đẹp, vi phạm sự sang trọng. 

Do đó, mọi thành phần được kết nối phải có hình chữ nhật và việc kiểm tra điều kiện đó là cần thiết và đủ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, m = map(int, input().split())
    g = [input().strip() for _ in range(n)]

    vis = [[False] * m for _ in range(n)]
    ok = True

    for r in range(n):
        for c in range(m):
            if g[r][c] != '1' or vis[r][c]:
                continue

            q = deque([(r, c)])
            vis[r][c] = True

            min_r = max_r = r
            min_c = max_c = c
            cnt = 0

            while q:
                x, y = q.popleft()
                cnt += 1

                min_r = min(min_r, x)
                max_r = max(max_r, x)
                min_c = min(min_c, y)
                max_c = max(max_c, y)

                for dx, dy in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                    nx = x + dx
                    ny = y + dy

                    if (
                        0 <= nx < n
                        and 0 <= ny < m
                        and not vis[nx][ny]
                        and g[nx][ny] == '1'
                    ):
                        vis[nx][ny] = True
                        q.append((nx, ny))

            area = (max_r - min_r + 1) * (max_c - min_c + 1)

            if area != cnt:
                ok = False
                break

        if not ok:
            break

    print("YES" if ok else "NO")
```Các vòng lặp bên ngoài tìm kiếm chưa được thăm`1`tế bào. Mỗi ô như vậy bắt đầu một thành phần được kết nối mới. 

BFS phục vụ hai mục đích cùng một lúc. Nó đánh dấu tất cả các ô thuộc thành phần và tính toán hộp giới hạn của thành phần. Việc theo dõi các chỉ số hàng và cột tối thiểu và tối đa sẽ tránh được bất kỳ lượt chuyển bổ sung nào qua lưới. 

Biến`cnt`lưu trữ số lượng ô trong thành phần. Sau khi BFS kết thúc, hình chữ nhật được xác định bởi hộp giới hạn đã được biết. Nếu thành phần là một hình chữ nhật hoàn hảo thì kích thước của nó phải bằng diện tích hình chữ nhật. Bất kỳ lỗ, vết lõm hoặc hình chữ L nào đều làm giảm kích thước thành phần trong khi vẫn giữ nguyên hộp giới hạn, khiến quá trình so sánh không thành công. 

Việc triển khai sử dụng tính liền kề bốn hướng vì đó là khái niệm tự nhiên về khả năng kết nối của các thành phần lưới. Hàng xóm chéo được cố tình bỏ qua. 

Không cần xử lý đặc biệt đối với các ô đơn, hàng đơn hoặc cột đơn. Công thức diện tích hoạt động chính xác trong tất cả các trường hợp đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
100
011
011
```Có hai thành phần được kết nối. 

| Thành phần | Tế bào | Hộp đóng bìa | Khu vực | Số lượng tế bào | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {(0,0)} | hàng 0..0, cột 0..0 | 1 | 1 | Có | 
| 2 | {(1,1),(1,2),(2,1),(2,2)} | hàng 1..2, cột 1..2 | 4 | 4 | Có | 

Cả hai thành phần đều là hình chữ nhật hoàn hảo, vì vậy câu trả lời là:```
YES
```Ví dụ này cho thấy nhiều hình chữ nhật bị ngắt kết nối là hoàn toàn có thể chấp nhận được. 

### Ví dụ 2 

đầu vào:```
3 3
110
111
110
```Có một thành phần được kết nối. 

| Thành phần | Hộp đóng bìa | Khu vực | Số lượng tế bào | hợp lệ | 
| --- | --- | --- | --- | --- | 
| Toàn bộ hình dạng | hàng 0..2, cột 0..1 | 6 | 5 | Không | 

Hình chữ nhật giới hạn chứa sáu ô:```
11
11
11
```nhưng một trong số chúng bị thiếu trong thành phần. Từ`area != count`, thành phần không phải là hình chữ nhật. 

Câu trả lời là:```
NO
```Ví dụ này thể hiện tình trạng lỗi chính. Hình dạng được kết nối với một ô bị thiếu bên trong hộp giới hạn của nó sẽ tạo ra các hình chữ nhật tối đa chồng lên nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập nhiều nhất một lần trong BFS | 
| Không gian | O(nm) | Mảng đã truy cập và hàng đợi BFS | 

Lưới lớn nhất có thể chỉ chứa`10,000`tế bào. MỘT`O(nm)`truyền tải là cực kỳ nhỏ đối với các giới hạn nhất định. Việc sử dụng bộ nhớ cũng rất nhỏ, vừa vặn thoải mái trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        g = [input().strip() for _ in range(n)]

        vis = [[False] * m for _ in range(n)]
        ok = True

        for r in range(n):
            for c in range(m):
                if g[r][c] != '1' or vis[r][c]:
                    continue

                q = deque([(r, c)])
                vis[r][c] = True

                min_r = max_r = r
                min_c = max_c = c
                cnt = 0

                while q:
                    x, y = q.popleft()
                    cnt += 1

                    min_r = min(min_r, x)
                    max_r = max(max_r, x)
                    min_c = min(min_c, y)
                    max_c = max(max_c, y)

                    for dx, dy in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                        nx = x + dx
                        ny = y + dy

                        if (
                            0 <= nx < n
                            and 0 <= ny < m
                            and not vis[nx][ny]
                            and g[nx][ny] == '1'
                        ):
                            vis[nx][ny] = True
                            q.append((nx, ny))

                area = (max_r - min_r + 1) * (max_c - min_c + 1)

                if area != cnt:
                    ok = False

        ans.append("YES" if ok else "NO")

    return "\n".join(ans) + "\n"

# provided sample
assert run(
"""5
3 3
100
011
011
3 3
110
111
110
1 5
01111
4 5
11111
01010
01000
01000
3 2
11
00
11
"""
) == """YES
NO
YES
NO
YES
"""

# minimum size
assert run(
"""1
1 1
1
"""
) == "YES\n"

# all zeros
assert run(
"""1
2 2
00
00
"""
) == "YES\n"

# L-shape
assert run(
"""1
2 2
11
10
"""
) == "NO\n"

# full rectangle
assert run(
"""1
3 4
1111
1111
1111
"""
) == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`chứa đựng`1`| CÓ | Hình chữ nhật không trống nhỏ nhất | 
| Tất cả số không | CÓ | Không có thành phần nào cả | 
|`11 / 10`| KHÔNG | Thành phần hình chữ L | 
| Đầy`3×4`khối cái | CÓ | Thành phần hình chữ nhật lớn | 

## Vỏ cạnh 

Xét hình chữ L:```
1
2 2
11
10
```BFS phát hiện một thành phần chứa ba ô. Hộp giới hạn của nó trải dài cả hai hàng và cả hai cột, mang lại diện tích`4`. Từ`3 != 4`, thành phần không phải là hình chữ nhật và kết quả đầu ra của thuật toán`NO`. 

Xem xét các ô chạm theo đường chéo:```
1
2 2
10
01
```Các ô không được kết nối theo bốn hướng, do đó có hai thành phần riêng biệt. Mỗi thành phần có diện tích`1`và kích thước`1`. Cả hai lần kiểm tra đều thành công và đầu ra là`YES`. 

Hãy xem xét một cấu trúc rỗng:```
1
3 3
111
101
111
```BFS tìm thấy một thành phần có tám ô. Hộp giới hạn là toàn bộ`3 × 3`lưới có diện tích là`9`. Từ`8 != 9`, thành phần bị từ chối và câu trả lời là`NO`. 

Hãy xem xét một hàng duy nhất:```
1
1 5
01111
```Thành phần chiếm các cột`1..4`. Diện tích khung giới hạn là`4`, khớp chính xác với số lượng ô. Thành phần này là một hình chữ nhật có chiều cao bằng một và thuật toán xuất ra chính xác`YES`. 

Những trường hợp này bao gồm các tình huống mà cách diễn giải đơn giản thường thất bại, nhưng thử nghiệm hộp giới hạn xử lý tất cả chúng một cách tự nhiên.
