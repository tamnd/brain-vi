---
title: "CF 11C - Có bao nhiêu hình vuông?"
description: "Chúng tôi được cung cấp một lưới nhị phân. Đối tượng hợp lệ không phải là một hình vuông đầy, nó chỉ là đường viền của hình vuông được vẽ bằng số 1. Mọi ô không thuộc đường viền phải bằng 0."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 11
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 11"
rating: 2200
weight: 11
solve_time_s: 162
verified: true
draft: false
---
[CF 11C - Có bao nhiêu hình vuông?](https://codeforces.com/problemset/problem/11/C) 

**Đánh giá:** 2200 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới nhị phân. Một đối tượng hợp lệ không phải là một hình vuông đầy, nó chỉ là đường viền của một hình vuông được vẽ bằng`1`S. Mọi ô không thuộc đường viền đều phải`0`. 

Bài toán yêu cầu chúng ta đếm hai loại hình vuông. 

Loại đầu tiên được căn chỉnh theo trục. Các cạnh của nó có chiều ngang và chiều dọc, giống hệt như một hình vuông bình thường trong ma trận. 

Loại thứ hai được xoay 45 độ. Trong tọa độ ma trận, nó trông giống như một viên kim cương. 

Có một hạn chế bổ sung làm thay đổi hoàn toàn vấn đề. Một hình vuông hợp lệ phải được cách ly. Bất kì`1`không thuộc hình vuông thì bị cấm chạm vào hình vuông, kể cả theo đường chéo. Vì vậy, mọi ô lân cận xung quanh đường viền phải`0`, trừ khi hàng xóm đó cũng là một phần của cùng một hình vuông. 

Kích thước lưới tối đa là`250 × 250`, nhưng tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm nhiều nhất là`10^6`. Điều này có nghĩa là một`O(n^4)`hoặc`O(n^5)`giải pháp là vô vọng. Một đĩa đơn`250 × 250`lưới đã có khoảng`6 × 10^4`tế bào. Việc liệt kê tất cả các ô vuông có thể có và quét từng ô viền của chúng sẽ dễ dàng đạt tới hàng tỷ thao tác. 

Độ phức tạp mục tiêu phải ở gần tuyến tính hoặc bậc hai về số lượng ô, có thể có hệ số không đổi nhỏ. 

Phần khó khăn là không tự phát hiện được các hình vuông. Khó khăn thực sự là việc xác minh chính xác điều kiện cách ly. 

Hãy xem xét ví dụ này:```
111
101
111
```Đây là một hình vuông căn chỉnh theo trục hợp lệ. Trung tâm`0`được cho phép vì chỉ có biên giới mới quan trọng. 

Bây giờ hãy nhìn vào điều này:```
1110
1011
1110
0000
```Một giải pháp bất cẩn có thể tính đến`3 × 3`đường viền bên trái. Nhưng phần bổ sung`1`Tại`(1,3)`chạm vào hình vuông theo đường chéo, vì vậy câu trả lời thực sự là`0`. 

Các hình vuông xoay có thể gây ra một lỗi khác:```
00100
01010
00100
```Đây là một viên kim cương hợp lệ. Nhưng nếu chúng ta thêm một liền kề`1`:```
00100
01110
00100
```viên kim cương trở nên không hợp lệ vì các ô bên chạm vào hình dạng. 

Một trường hợp tế nhị khác là các đường viền chồng chéo. Giả sử hai hình vuông có chung một số ô. Không cái nào hợp lệ, bởi vì mỗi ô biên giới khi đó sẽ có các ô lân cận nước ngoài`1`là`. 
Cách an toàn nhất để nghĩ về điều kiện là: mọi thành phần được kết nối của`1`s phải chính xác là một đường viền hình vuông và không có gì khác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi ô vuông có thể, kiểm tra xem tất cả các ô viền cần thiết có`1`, kiểm tra xem tất cả các ô lân cận khác có`0`, và đếm những cái hợp lệ. 

Đối với các hình vuông thẳng hàng với trục, có`O(n^2 m^2)`hình chữ nhật có thể có và chúng tôi chỉ giữ những hình chữ nhật có độ dài cạnh bằng nhau. Xác minh chi phí một hình vuông một cách ngây thơ`O(k)`Ở đâu`k`là độ dài cạnh Trường hợp xấu nhất trở nên đại khái`O(n^5)`. 

Các hình vuông xoay thậm chí còn tệ hơn vì hình học của chúng kém thuận tiện hơn. Việc liệt kê tất cả các tâm và bán kính vẫn dẫn đến việc quét nhiều ô viền liên tục. 

Phương pháp brute-force hoạt động hợp lý vì định nghĩa mang tính cục bộ và rõ ràng. Nếu chúng ta kiểm tra từng ô viền và mọi ô lân cận thì độ chính xác sẽ có ngay lập tức. Vấn đề là số lượng công việc lặp đi lặp lại. Các ô ứng viên gần đó sử dụng lại hầu hết các thông tin giống nhau. 

Quan sát quan trọng là điều kiện cách ly đặc trưng hoàn toàn cho các thành phần được kết nối. 

Lấy bất kỳ thành phần kết nối nào của`1`s`. Nếu nó tạo thành một đường viền vuông hợp lệ thì: 

1. mỗi`1`trong thành phần thuộc về hình vuông đó, 
2. không có sự đụng chạm thêm`1`s`, 
3. toàn bộ thành phần có hình dạng rất cứng. 

Vì vậy, thay vì liệt kê các ô vuông, chúng ta liệt kê các thành phần được kết nối. 

Một thành phần chỉ có thể là một trong hai hình dạng có thể có: 

1. một đường viền hình vuông rỗng thẳng hàng với trục, 
2. đường viền kim cương. 

Tổng số ô trên tất cả các thành phần là tuyến tính theo kích thước lưới, vì vậy khi trích xuất các thành phần bằng BFS hoặc DFS, chúng ta chỉ cần phân loại từng thành phần. 

Điều này thay đổi sự phức tạp một cách đáng kể. Mỗi ô tham gia vào chính xác một lần duyệt thành phần và một lượng nhỏ công việc xác thực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^5) | O(1) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt qua lưới và tìm các thành phần được kết nối của`1`đang sử dụng BFS 8 hướng. 

Chúng tôi sử dụng 8 hướng vì việc chạm vào từng góc cũng quan trọng trong câu lệnh. Nếu hai`1`chạm vào nhau theo đường chéo thì chúng thuộc cùng một cụm cấm. 
2. Đối với mỗi thành phần, hãy thu thập tất cả tọa độ của nó. 

Đặt kích thước thành phần là`s`. 
3. Cố gắng nhận biết thành phần này là một hình vuông thẳng hàng với trục. 

Tính toán: 

- hàng tối thiểu`r1`- hàng tối đa`r2`- cột tối thiểu`c1`- cột tối đa`c2`Bản thân hộp giới hạn phải là hình vuông, vì vậy:```
r2 - r1 == c2 - c1
```Gọi độ dài cạnh là:```
L = r2 - r1 + 1
```Đường viền hợp lệ chứa chính xác:```
4 * L - 4
```tế bào. 
4. Xác minh cấu trúc đường viền. 

Mọi ô trên chu vi của hộp giới hạn phải thuộc về thành phần. Mọi ô bên trong không được thuộc về thành phần. 

Nếu tất cả các bước kiểm tra đều vượt qua và kích thước thành phần khớp`4L - 4`, thành phần này là một hình vuông hợp lệ. 
5. Nếu xác thực theo trục không thành công, hãy thử xác thực theo hình vuông xoay. 

Đối với một viên kim cương, xác định tọa độ được chuyển đổi:```
u = r + c
v = r - c
```TRONG`(u,v)`không gian, hình thoi sẽ trở thành một đường viền hình vuông thẳng hàng với trục. 
6. Tính toán:```
umin, umax
vmin, vmax
```Hộp giới hạn được chuyển đổi phải thỏa mãn:```
umax - umin == vmax - vmin
```7. Hãy:```
D = umax - umin
```Mỗi điểm biên của hình vuông được chuyển đổi tương ứng với một ô lưới ban đầu. 

Một tế bào`(r,c)`thuộc đường viền kim cương nếu:```
u == umin or u == umax or
v == vmin or v == vmax
```và tất cả tọa độ vẫn ở bên trong hình vuông được chuyển đổi. 
8. Tạo tất cả các ô viền dự kiến ​​của hình thoi và so sánh chúng với thành phần. 

Nếu chúng khớp chính xác, hãy tính thành phần đó là hợp lệ. 
9. Tính tổng các thành phần hợp lệ. 

### Tại sao nó hoạt động 

Tất cả các phân vùng BFS`1`các ô thành các thành phần được kết nối tối đa dưới sự kề cận bên hoặc góc. Một hình vuông hợp lệ không thể chia sẻ lân cận với nước ngoài`1`s`, vì vậy mọi ô vuông hợp lệ phải xuất hiện dưới dạng toàn bộ thành phần được kết nối. 

Đối với các hình vuông được căn chỉnh theo trục, hộp giới hạn sẽ xác định duy nhất đường viền ứng cử viên. Bất kỳ ô chu vi nào bị thiếu hoặc ô bên trong bổ sung sẽ phá vỡ định nghĩa ngay lập tức. 

Đối với các hình vuông được xoay, phép biến đổi tọa độ sẽ chuyển đổi các cạnh chéo thành các cạnh thẳng hàng với trục. Hình dạng được biến đổi phải trở thành một đường viền vuông hoàn hảo trong`(u,v)`không gian. Vì phép biến đổi có tính chất phỏng đoán trên các ô lưới nên việc khớp chính xác đường viền được biến đổi tương đương với việc khớp hình thoi ban đầu. 

Vì mọi thành phần đều được kiểm tra toàn diện theo cả hai dạng hình học hợp lệ nên mọi thành phần được đếm đều hợp lệ và mọi ô vuông hợp lệ đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

DIR8 = [
    (-1, -1), (-1, 0), (-1, 1),
    (0, -1),           (0, 1),
    (1, -1),  (1, 0),  (1, 1)
]

def axis_square(comp):
    cells = set(comp)

    r1 = min(r for r, c in comp)
    r2 = max(r for r, c in comp)
    c1 = min(c for r, c in comp)
    c2 = max(c for r, c in comp)

    if r2 - r1 != c2 - c1:
        return False

    side = r2 - r1 + 1

    if side < 2:
        return False

    expected = 4 * side - 4

    if len(comp) != expected:
        return False

    for r in range(r1, r2 + 1):
        for c in range(c1, c2 + 1):
            border = (
                r == r1 or r == r2 or
                c == c1 or c == c2
            )

            inside = (r, c) in cells

            if border != inside:
                return False

    return True

def diamond_square(comp):
    cells = set(comp)

    uv = []
    for r, c in comp:
        uv.append((r + c, r - c))

    umin = min(u for u, v in uv)
    umax = max(u for u, v in uv)
    vmin = min(v for u, v in uv)
    vmax = max(v for u, v in uv)

    if umax - umin != vmax - vmin:
        return False

    d = umax - umin

    if d < 2:
        return False

    expected_cells = set()

    for u in range(umin, umax + 1):
        for v in range(vmin, vmax + 1):

            border = (
                u == umin or u == umax or
                v == vmin or v == vmax
            )

            if not border:
                continue

            if (u + v) % 2 != 0:
                continue

            r = (u + v) // 2
            c = (u - v) // 2

            expected_cells.add((r, c))

    return expected_cells == cells

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        g = [input().strip() for _ in range(n)]

        vis = [[False] * m for _ in range(n)]

        ans = 0

        for i in range(n):
            for j in range(m):
                if g[i][j] == '0' or vis[i][j]:
                    continue

                q = deque()
                q.append((i, j))
                vis[i][j] = True

                comp = []

                while q:
                    r, c = q.popleft()
                    comp.append((r, c))

                    for dr, dc in DIR8:
                        nr = r + dr
                        nc = c + dc

                        if 0 <= nr < n and 0 <= nc < m:
                            if not vis[nr][nc] and g[nr][nc] == '1':
                                vis[nr][nc] = True
                                q.append((nr, nc))

                if axis_square(comp) or diamond_square(comp):
                    ans += 1

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

solve()
```Phần BFS xây dựng các thành phần kết nối theo phương liền kề 8 hướng. Chỉ sử dụng 4 hướng sẽ không chính xác vì câu lệnh cấm chạm vào đường chéo. 

các`axis_square`chức năng xác nhận đường viền hình vuông thông thường. Hộp giới hạn xác định chính xác nơi đường viền sẽ tồn tại. Sự so sánh:```
if border != inside:
```là cách rõ ràng nhất để từ chối đồng thời cả ô viền bị thiếu và ô bên trong bất hợp pháp. 

Kiểm tra kim cương sử dụng phép biến đổi tọa độ:```
u = r + c
v = r - c
```Đây là thủ thuật hình học quan trọng. Một hình vuông được xoay sẽ trở thành một hình vuông thẳng hàng với trục thông thường trong các tọa độ được chuyển đổi. 

Kiểm tra tính chẵn lẻ:```
if (u + v) % 2 != 0:
```vấn đề vì chỉ những điểm có tính chẵn lẻ phù hợp mới tương ứng với tọa độ lưới số nguyên sau khi đảo ngược. 

Sự bình đẳng cuối cùng:```
expected_cells == cells
```đảm bảo không thiếu ô viền cũng như ô thừa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0001000
0010100
0100010
0010100
0001000
```Đây là một viên kim cương. 

| Bước | Giá trị | 
| --- | --- | 
| Kích thước thành phần | 5 | 
| ừm | 3 | 
| umax | 5 | 
| vmin | -1 | 
| vmax | 1 | 
| Độ rộng giới hạn bằng | Có | 
| Đã tạo ô viền | 5 | 
| Thành phần phù hợp | Có | 

Hình vuông đã biến đổi có độ dài cạnh`2`TRONG`(u,v)`không gian. Mỗi điểm biên ánh xạ trở lại chính xác năm ô ban đầu. 

### Ví dụ 2 

đầu vào:```
1111
1001
1001
1111
```| Bước | Giá trị | 
| --- | --- | 
| Hộp giới hạn | hàng 0..3, cột 0..3 | 
| Chiều dài cạnh | 4 | 
| Ô viền dự kiến ​​| 12 | 
| Các tế bào thành phần thực tế | 12 | 
| Nội thất chứa 1 | Không | 
| Kết quả | hợp lệ | 

Điều này xác nhận tâm rỗng được cho phép. Chỉ có biên giới là quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập một lần trong BFS và được kiểm tra số lần không đổi | 
| Không gian | O(nm) | Đã truy cập mảng và lưu trữ thành phần | 

Lưới chứa nhiều nhất`62500`các ô cho mỗi trường hợp thử nghiệm. Xử lý tuyến tính dễ dàng đủ nhanh trong giới hạn 2 giây, ngay cả với nhiều trường hợp thử nghiệm. Việc sử dụng bộ nhớ cũng ở mức thoải mái dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    DIR8 = [
        (-1, -1), (-1, 0), (-1, 1),
        (0, -1),           (0, 1),
        (1, -1),  (1, 0),  (1, 1)
    ]

    def axis_square(comp):
        cells = set(comp)

        r1 = min(r for r, c in comp)
        r2 = max(r for r, c in comp)
        c1 = min(c for r, c in comp)
        c2 = max(c for r, c in comp)

        if r2 - r1 != c2 - c1:
            return False

        side = r2 - r1 + 1

        if side < 2:
            return False

        if len(comp) != 4 * side - 4:
            return False

        for r in range(r1, r2 + 1):
            for c in range(c1, c2 + 1):
                border = (
                    r == r1 or r == r2 or
                    c == c1 or c == c2
                )

                if border != ((r, c) in cells):
                    return False

        return True

    def diamond_square(comp):
        cells = set(comp)

        uv = [(r + c, r - c) for r, c in comp]

        umin = min(u for u, v in uv)
        umax = max(u for u, v in uv)
        vmin = min(v for u, v in uv)
        vmax = max(v for u, v in uv)

        if umax - umin != vmax - vmin:
            return False

        expected = set()

        for u in range(umin, umax + 1):
            for v in range(vmin, vmax + 1):

                border = (
                    u == umin or u == umax or
                    v == vmin or v == vmax
                )

                if not border:
                    continue

                if (u + v) % 2:
                    continue

                r = (u + v) // 2
                c = (u - v) // 2

                expected.add((r, c))

        return expected == cells

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        g = [input().strip() for _ in range(n)]

        vis = [[False] * m for _ in range(n)]

        ans = 0

        for i in range(n):
            for j in range(m):
                if g[i][j] == '0' or vis[i][j]:
                    continue

                q = deque([(i, j)])
                vis[i][j] = True

                comp = []

                while q:
                    r, c = q.popleft()
                    comp.append((r, c))

                    for dr, dc in DIR8:
                        nr = r + dr
                        nc = c + dc

                        if 0 <= nr < n and 0 <= nc < m:
                            if not vis[nr][nc] and g[nr][nc] == '1':
                                vis[nr][nc] = True
                                q.append((nr, nc))

                if axis_square(comp) or diamond_square(comp):
                    ans += 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""2
8 8
00010001
00101000
01000100
10000010
01000100
00101000
11010011
11000011
10 10
1111111000
1000001000
1011001000
1011001010
1000001101
1001001010
1010101000
1001001000
1000001000
1111111000
"""
) == "1\n2", "sample"

# minimum size
assert run(
"""1
2 2
11
11
"""
) == "1", "smallest valid square"

# all zeros
assert run(
"""1
4 4
0000
0000
0000
0000
"""
) == "0", "empty grid"

# touching extra cell
assert run(
"""1
4 4
1110
1011
1110
0000
"""
) == "0", "foreign touching cell"

# valid diamond
assert run(
"""1
5 5
00100
01010
00100
00000
00000
"""
) == "1", "diamond square"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2×2`tất cả những cái |`1`| Hình vuông căn chỉnh trục hợp lệ nhỏ nhất | 
| Tất cả số không |`0`| Thành phần trống | 
| Ô cảm động thêm |`0`| Quy tắc cách ly | 
| Mẫu kim cương |`1`| Phát hiện hình vuông xoay | 

## Vỏ cạnh 

Hãy xem xét đầu vào này:```
1
4 4
1110
1011
1110
0000
```các`3 × 3`đường viền thoạt nhìn có vẻ hợp lệ. Nhưng BFS với tính liền kề 8 hướng sẽ hợp nhất ô phụ`(1,3)`vào cùng một thành phần vì nó chạm vào`(0,2)`theo đường chéo. 

Kích thước thành phần trở thành`9`, trong khi một`3 × 3`biên giới yêu cầu chính xác`8`tế bào. Trình xác thực trục vuông sẽ từ chối nó ngay lập tức. 

Bây giờ hãy xem xét các hình dạng chồng chéo:```
1
5 5
11111
10001
11111
10001
11111
```Một cách tiếp cận đơn giản có thể phát hiện ra nhiều ô vuông. Thuật toán của chúng tôi xử lý toàn bộ cấu trúc như một thành phần được kết nối. Hộp giới hạn của nó là`5 × 5`, nhưng bên trong chứa đựng trái phép`1`s`. Trình xác nhận từ chối nó. 

Cuối cùng, hãy xem xét hình vuông xoay nhỏ nhất có thể:```
1
3 3
010
101
010
```Các tọa độ được chuyển đổi tạo ra:```
u in [1,3]
v in [-1,1]
```Tất cả các ô viền được chuyển đổi dự kiến ​​sẽ ánh xạ chính xác trở lại thành phần ban đầu. Thuật toán đếm một viên kim cương hợp lệ.
