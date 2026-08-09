---
title: "CF 102443J - Nhà máy"
description: "Chúng ta có một bản đồ hình chữ nhật (m nhân n). Một ô là một phân xưởng, được viết là , hoặc trống, được viết là .. Tất cả các ô phân xưởng tạo thành một vùng được kết nối một bên và không có vùng trống kèm theo bên trong nó."
date: "2026-08-09T13:47:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "J"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 468
verified: true
draft: false
---

[CF 102443J - Nhà máy](https://codeforces.com/problemset/problem/102443/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bản đồ hình chữ nhật (m \times n). Một ô hoặc là một xưởng, được viết là`*`, hoặc trống, được viết là`.`. Tất cả các ô phân xưởng tạo thành một vùng được kết nối bên và không có vùng trống kèm theo bên trong nó. Điều kiện thứ hai có nghĩa là mọi ô trống đều thuộc khu vực bên ngoài của nhà máy. 

Nhìn vào các đường lưới giữa các ô. Một nút lưới được gọi là quan trọng nếu nó là một góc của ít nhất một ô phân xưởng. Mỗi phía của ô phân xưởng là một hành lang nối hai nút lưới điểm cuối của nó. 

Chúng ta cần xây dựng một tuyến đường khép kín thông qua các nút lưới quan trọng. Mỗi lần di chuyển phải đi theo một hành lang, không được sử dụng hành lang nào hai lần và mọi nút quan trọng phải xảy ra ở đâu đó trên tuyến đường. Con đường không nhất thiết phải ngắn. Nếu tuyến đường như vậy không thể tồn tại, chúng tôi sẽ in`No`. Nếu không chúng tôi in`Yes`, số lượng hành lang và trình tự các nút lưới trong tuyến đường. Các ràng buộc ban đầu là (1 \le m,n \le 20), với giới hạn thời gian là hai giây và bộ nhớ 256 MB. 

Giải thích biểu đồ quan trọng là các hành lang tạo thành một biểu đồ lưới phẳng. Chúng ta đang tìm kiếm một đồ thị con liên thông chứa mọi đỉnh quan trọng trong đó mọi đỉnh được sử dụng đều có bậc chẵn. Đồ thị như vậy có một chu trình Euler và mạch đó chính xác là tuyến đường mà bài toán yêu cầu. 

Có một số trường hợp dễ xảy ra sai sót khi cách giải thích ngây thơ. Coi như```
1 1
*
```Câu trả lời là`Yes`. Bốn góc tạo thành một vòng tròn nên lộ trình có thể chỉ cần đi vòng quanh một xưởng duy nhất. 

Vì```
1 1
.
```không có nút quan trọng. Tuyến đường trống là hợp lệ, vì vậy câu trả lời là`Yes`không có hành lang. Giải pháp giả định tuyến đường phải chứa ít nhất một xưởng sẽ từ chối trường hợp này một cách không chính xác. 

Coi như```
2 2
**
**
```Câu trả lời là`No`. Biểu đồ lưới tương ứng là lưới (3 \times 3). Bốn đỉnh góc của nó có bậc hai, do đó, đồ thị con chẵn kéo dài buộc phải sử dụng toàn bộ ranh giới bên ngoài. Điều đó làm cho đỉnh trung tâm bị ngắt kết nối, trong khi việc sử dụng một cạnh từ tâm sẽ làm cho một trong các đỉnh bậc ba liền kề trở thành số lẻ. Đây là một ví dụ hữu ích vì chỉ kiểm tra xem nhà máy đã được kết nối hay chưa là chưa đủ. 

Cuối cùng,```
1 2
**
```là`Yes`. Hai ô tạo thành một biểu đồ lưới (2 \times 3) và chu vi cung cấp một tuyến đường khép kín hợp lệ qua mọi nút quan trọng. Một công trình chỉ xem xét các chu trình phân xưởng-tế bào riêng lẻ có thể bỏ sót thực tế là các chu trình liền kề có thể được kết hợp thành một tuyến đường lớn hơn. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu trực tiếp có thể được mô tả dưới dạng các mặt của đồ thị phẳng. Mỗi ô xưởng là một mặt bao quanh, trong khi tất cả các ô trống và phần bên ngoài đều thuộc cùng một mặt ngoài vì nhà xưởng không có lỗ. Cung cấp cho mỗi mặt xưởng một trong hai màu. Bất cứ khi nào hai mặt liền kề có màu khác nhau, hãy sử dụng cạnh lưới ngăn cách chúng. 

Đối với bất kỳ màu nào, các cạnh được chọn sẽ tự động có mức độ chẵn ở mọi nút lưới. Đi vòng quanh một nút, màu sắc của các mặt cuối cùng sẽ trở về màu ban đầu, do đó số lần thay đổi màu sắc là chẵn. Nếu mọi nút quan trọng có ít nhất một thay đổi màu thì mọi nút quan trọng đều có mức chẵn dương. 

Do đó, thuật toán brute-force có thể thử mọi màu nhị phân của các ô phân xưởng, xây dựng các cạnh chuyển tiếp của nó, kiểm tra xem mọi nút quan trọng có được che phủ hay không và cuối cùng kiểm tra xem các cạnh đã chọn có được kết nối hay không. Nếu có (K) ô phân xưởng, điều này sẽ xem xét (2^K) chất tạo màu. Trong trường hợp xấu nhất (K=mn=400), đưa ra công việc khoảng (400 \cdot 2^{400}). Điều đó hoàn toàn không thể thực hiện được. 

Quan sát hữu ích là điều kiện tại một nút lưới chỉ phụ thuộc vào các ô phân xưởng ngay xung quanh nút đó. Tại một nút bên trong, có tối đa bốn ô như vậy. Tại một nút ranh giới thậm chí còn có ít hơn, với phần bên ngoài được coi là mặt có màu 0. 

Cấu trúc cục bộ đó có nghĩa là chúng ta không cần phải nhớ toàn bộ màu sắc khi xây dựng nó. Xử lý từng ô theo hàng và chỉ nhớ màu của ô (w) cuối cùng, trong đó (w=\min(m,n)). Khi ô tiếp theo được chỉ định, mọi nút lưới bên trong mới hoàn thành đều có tất cả các ô sự cố của nó ở biên giới này. Chúng ta có thể kiểm tra ràng buộc ngay lập tức và loại bỏ trạng thái nếu nó thất bại. 

Nhà máy có thể được hoán vị sao cho (w) là kích thước nhỏ hơn. Trạng thái hồ sơ khi đó chỉ là mặt nạ bit (w). Có nhiều nhất (2^w) trạng thái ở bất kỳ vị trí nào và mỗi trạng thái có tối đa hai lựa chọn cho ô xưởng tiếp theo. Các ô trống chỉ có một màu có thể. 

Điều kiện không có lỗ là điều làm cho việc diễn giải khuôn mặt trở nên đặc biệt hữu ích. Các cạnh được chọn là ranh giới giữa hai màu mặt. Nếu mọi nút quan trọng đều liên quan đến một cạnh đã chọn thì các thành phần ranh giới này không thể tách rời: hai thành phần ranh giới khác nhau sẽ để lại một vùng có các mặt có màu bằng nhau giữa chúng và một nút lưới nằm hoàn toàn bên trong vùng đó sẽ không có cạnh liên quan được chọn. Điều đó sẽ mâu thuẫn với yêu cầu mọi nút quan trọng đều được bảo vệ. Do đó đồ thị chuyển tiếp đã chọn được kết nối. 

Khi đã biết các cạnh đó, việc tìm đường đi thực tế chỉ là thuật toán Hierholzer cho đồ thị Euler. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(mn \cdot 2^{mn})) | (O(mn)) | Quá chậm | 
| Hồ sơ DP | (O(mn \cdot 2^w)), (w=\min(m,n)) | (O(mn \cdot 2^w)) với tính năng ghi nhớ | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển vị trí bản đồ khi cần thiết sao cho số cột (w) không lớn hơn số hàng. Phần mũ của thuật toán phụ thuộc vào độ rộng biên giới, do đó, việc sử dụng kích thước nhỏ hơn sẽ tạo ra sự khác biệt giữa (2^{20}) và có thể là không gian trạng thái lớn hơn nhiều. 
2. Đánh dấu mọi nút lưới là một góc của ít nhất một ô xưởng. Đây chính xác là các nút mà tuyến đường cuối cùng phải ghé thăm. 
3. Giải thích mỗi ô phân xưởng là một mặt có màu bằng 0 hoặc bằng một. Các ô trống và bên ngoài luôn được coi là màu 0. Một cạnh được chọn chính xác khi màu sắc của các mặt trên hai cạnh của nó khác nhau. 
4. Quét các ô theo thứ tự hàng lớn. Trạng thái DP lưu trữ màu của các ô (w) gần đây nhất trong mặt nạ bit. Bit 0 đại diện cho ô được gán gần đây nhất, trong khi bit được lưu trữ cao nhất đại diện cho ô ở hàng trước ở cột hiện tại. 
5. Đối với ô xưởng, hãy thử cả hai màu có thể. Đối với ô trống, chỉ cho phép màu 0 vì nó là một phần của mặt ngoài. 
6. Bất cứ khi nào một ô mới được chỉ định hoàn thành một nút lưới bên trong, hãy kiểm tra bốn màu mặt tới. Nếu nút quan trọng và tất cả bốn màu đều bằng nhau thì không có cạnh nào được chọn liên quan đến nút đó, vì vậy trạng thái này không bao giờ có thể tạo ra một tuyến đường hợp lệ. 
7. Xử lý các nút ranh giới trên, trái, phải và dưới khi các ô sự cố của chúng có sẵn. Các ô bị thiếu bên ngoài bản đồ có màu 0, do đó, nút ranh giới với một ô phân xưởng sẽ được che phủ chính xác khi ô đó nhận được màu một. 
8. Ghi nhớ cặp bao gồm vị trí ô hiện tại và mặt nạ biên. Nếu cả hai lựa chọn màu đều không thể dẫn đến một màu hợp lệ hoàn chỉnh, hãy nhớ rằng trạng thái đó là không thể. Bước chuyển vị giữ chiều rộng mặt nạ tối đa là 20. 
9. Tạo lại một màu thành công bằng cách bắt đầu từ mặt nạ số 0 ban đầu và liên tục lấy màu có trạng thái đệ quy thành công. DP được ghi nhớ cho chúng ta biết lựa chọn nào có thể đi đến cuối cùng. 
10. Chuyển màu thành hành lang. Đối với mọi cạnh lưới ngang và dọc, hãy xem xét các ô hội thảo ở hai bên của nó, sử dụng màu 0 bên ngoài bản đồ và cho các ô trống. Giữ cạnh chính xác khi hai màu khác nhau. 
11. Đồ thị được chọn có bậc chẵn ở mọi nút quan trọng vì các cạnh được chọn tương ứng với sự thay đổi màu sắc xung quanh nút. Mọi nút quan trọng đều có mức độ dương vì DP từ chối các vùng lân cận đơn sắc. Thuộc tính không có lỗ trống mang lại khả năng kết nối nên đồ thị được chọn là Euler. 
12. Chạy thuật toán Hierholzer trên đồ thị đã chọn. Nó tạo ra một đường đi khép kín sử dụng mọi hành lang đã chọn đúng một lần. Vì mọi nút quan trọng đều thuộc biểu đồ này nên tuyến kết quả đáp ứng mọi yêu cầu. 

### Tại sao nó hoạt động 

Bất biến của hồ sơ DP là mọi nút lưới có các ô sự cố đã được chỉ định đều đã được kiểm tra. Một trạng thái chỉ được giữ lại khi mọi nút hoàn thành quan trọng có ít nhất hai cạnh sự cố được chọn và vì mức độ cạnh được chọn luôn là số chẵn, điều đó có nghĩa là mức độ của nó là dương và chẵn. 

Cấu trúc màu sắc mặt làm cho mọi mức độ cạnh được chọn đều tự động. Xung quanh bất kỳ nút lưới nào, việc nhập và rời khỏi tập hợp các mặt một màu sẽ gây ra số lượng thay đổi màu chẵn. DP ngăn không cho số đó bằng 0 tại một nút quan trọng, do đó mọi nút quan trọng đều thuộc về biểu đồ đã chọn. Vì nhà máy được kết nối và không có lỗ hổng, nên một tập hợp ranh giới thay đổi màu bị ngắt kết nối sẽ khiến một số nút lưới quan trọng bên trong một vùng không có thay đổi màu, mâu thuẫn với điều kiện DP. Do đó, đồ thị đã chọn được kết nối và tất cả các bậc của nó đều là số chẵn, do đó định lý Euler đảm bảo một đường khép kín sử dụng mọi cạnh được chọn đúng một lần. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline
sys.setrecursionlimit(1_000_000)

def solve_case(original_grid):
    original_m = len(original_grid)
    original_n = len(original_grid[0])

    # Use the smaller dimension as the profile width.
    transposed = original_n > original_m

    if transposed:
        grid = [
            ''.join(original_grid[r][c] for r in range(original_m))
            for c in range(original_n)
        ]
    else:
        grid = original_grid[:]

    h = len(grid)
    w = len(grid[0])
    full = (1 << w) - 1

    # important[r][c] says whether grid node (r, c)
    # is a corner of at least one workshop cell.
    important = [[False] * (w + 1) for _ in range(h + 1)]

    for r in range(h):
        for c in range(w):
            if grid[r][c] == '*':
                important[r][c] = True
                important[r + 1][c] = True
                important[r][c + 1] = True
                important[r + 1][c + 1] = True

    if not any(any(row) for row in important):
        # No important nodes exist.
        if transposed:
            return "Yes\n0\n0 0\n"
        return "Yes\n0\n0 0\n"

    def all_equal_and_important(r, c, values):
        if not important[r][c]:
            return False
        first = values[0]
        return all(v == first for v in values)

    @lru_cache(maxsize=None)
    def dfs(pos, mask):
        if pos == h * w:
            return True

        r, c = divmod(pos, w)

        if grid[r][c] == '*':
            choices = (1, 0)
        else:
            choices = (0,)

        for x in choices:
            left = mask & 1

            # Check the internal node (r, c).
            if r > 0 and c > 0:
                up = (mask >> (w - 1 - c)) & 1
                up_left = (mask >> (w - c)) & 1

                if all_equal_and_important(
                    r, c, (up_left, up, left, x)
                ):
                    continue

            # Top boundary.
            if r == 0:
                if c == 0:
                    if all_equal_and_important(0, 0, (0, 0, 0, x)):
                        continue
                else:
                    if all_equal_and_important(0, c, (0, 0, left, x)):
                        continue

                if c == w - 1:
                    if all_equal_and_important(0, w, (0, 0, 0, x)):
                        continue

            # Left boundary.
            if c == 0 and r > 0:
                up = (mask >> (w - 1)) & 1
                if all_equal_and_important(r, 0, (0, 0, up, x)):
                    continue

            # Right boundary.
            if c == w - 1 and r > 0:
                up = mask & 1
                if all_equal_and_important(r, w, (0, 0, up, x)):
                    continue

            # Bottom boundary.
            if r == h - 1:
                if c == 0:
                    if all_equal_and_important(h, 0, (0, 0, 0, x)):
                        continue
                else:
                    if all_equal_and_important(h, c, (0, 0, left, x)):
                        continue

                if c == w - 1:
                    if all_equal_and_important(h, w, (0, 0, 0, x)):
                        continue

            new_mask = ((mask << 1) & full) | x

            if dfs(pos + 1, new_mask):
                return True

        return False

    if not dfs(0, 0):
        return "No\n"

    # Reconstruct one successful face coloring.
    colors = [[0] * w for _ in range(h)]
    pos = 0
    mask = 0

    while pos < h * w:
        r, c = divmod(pos, w)

        if grid[r][c] == '*':
            choices = (1, 0)
        else:
            choices = (0,)

        chosen = None

        for x in choices:
            left = mask & 1
            ok = True

            if r > 0 and c > 0:
                up = (mask >> (w - 1 - c)) & 1
                up_left = (mask >> (w - c)) & 1
                if all_equal_and_important(
                    r, c, (up_left, up, left, x)
                ):
                    ok = False

            if ok and r == 0:
                if c == 0:
                    if all_equal_and_important(0, 0, (0, 0, 0, x)):
                        ok = False
                else:
                    if all_equal_and_important(0, c, (0, 0, left, x)):
                        ok = False

                if ok and c == w - 1:
                    if all_equal_and_important(0, w, (0, 0, 0, x)):
                        ok = False

            if ok and c == 0 and r > 0:
                up = (mask >> (w - 1)) & 1
                if all_equal_and_important(r, 0, (0, 0, up, x)):
                    ok = False

            if ok and c == w - 1 and r > 0:
                up = mask & 1
                if all_equal_and_important(r, w, (0, 0, up, x)):
                    ok = False

            if ok and r == h - 1:
                if c == 0:
                    if all_equal_and_important(h, 0, (0, 0, 0, x)):
                        ok = False
                else:
                    if all_equal_and_important(h, c, (0, 0, left, x)):
                        ok = False

                if ok and c == w - 1:
                    if all_equal_and_important(h, w, (0, 0, 0, x)):
                        ok = False

            if not ok:
                continue

            new_mask = ((mask << 1) & full) | x
            if dfs(pos + 1, new_mask):
                chosen = x
                break

        if chosen is None:
            raise RuntimeError("reconstruction failed")

        colors[r][c] = chosen
        mask = ((mask << 1) & full) | chosen
        pos += 1

    # Convert back to the original orientation.
    if transposed:
        selected = [
            [0] * original_n for _ in range(original_m)
        ]
        for r in range(h):
            for c in range(w):
                selected[c][r] = colors[r][c]
    else:
        selected = colors

    m = original_m
    n = original_n

    def face(r, c):
        if 0 <= r < m and 0 <= c < n:
            return selected[r][c]
        return 0

    vertices = (m + 1) * (n + 1)
    graph = [[] for _ in range(vertices)]
    edges = []

    def vid(r, c):
        return r * (n + 1) + c

    def add_edge(r1, c1, r2, c2):
        a = vid(r1, c1)
        b = vid(r2, c2)
        eid = len(edges)
        edges.append((a, b))
        graph[a].append((eid, b))
        graph[b].append((eid, a))

    # Horizontal grid edges.
    for r in range(m + 1):
        for c in range(n):
            above = face(r - 1, c)
            below = face(r, c)
            if above != below:
                add_edge(r, c, r, c + 1)

    # Vertical grid edges.
    for r in range(m):
        for c in range(n + 1):
            left = face(r, c - 1)
            right = face(r, c)
            if left != right:
                add_edge(r, c, r + 1, c)

    important_original = [
        [False] * (n + 1) for _ in range(m + 1)
    ]

    for r in range(m):
        for c in range(n):
            if original_grid[r][c] == '*':
                important_original[r][c] = True
                important_original[r + 1][c] = True
                important_original[r][c + 1] = True
                important_original[r + 1][c + 1] = True

    important_vertices = []
    for r in range(m + 1):
        for c in range(n + 1):
            if important_original[r][c]:
                important_vertices.append(vid(r, c))

    start = important_vertices[0]

    # The face-color construction should give a connected graph.
    seen = {start}
    stack = [start]

    while stack:
        v = stack.pop()
        for _, to in graph[v]:
            if to not in seen:
                seen.add(to)
                stack.append(to)

    if len(seen) != len(important_vertices):
        return "No\n"

    # Hierholzer's algorithm.
    used = [False] * len(edges)
    ptr = [0] * vertices
    stack = [start]
    route = []

    while stack:
        v = stack[-1]

        while ptr[v] < len(graph[v]) and used[graph[v][ptr[v]][0]]:
            ptr[v] += 1

        if ptr[v] == len(graph[v]):
            route.append(stack.pop())
            continue

        eid, to = graph[v][ptr[v]]
        ptr[v] += 1

        if used[eid]:
            continue

        used[eid] = True
        stack.append(to)

    route.reverse()

    if len(route) != len(edges) + 1:
        return "No\n"

    out = ["Yes", str(len(edges))]
    for v in route:
        r, c = divmod(v, n + 1)
        out.append(f"{r} {c}")

    return "\n".join(out) + "\n"

def solve():
    m, n = map(int, input().split())
    grid = [input().strip() for _ in range(m)]
    sys.stdout.write(solve_case(grid))

if __name__ == "__main__":
    solve()
```Mặt nạ hồ sơ là chi tiết triển khai trung tâm. Trước khi xử lý một ô, bit 0 là ô đã được xử lý ngay bên trái của nó. Các bit cao hơn đại diện cho hàng trước đó theo thứ tự ngược lại. Điều này làm cho tất cả bốn ô xung quanh nút nội bộ mới hoàn thành đều có sẵn trong thời gian không đổi. 

bản cập nhật```
new_mask = ((mask << 1) & full) | x
```bỏ ô biên giới cũ nhất, dịch chuyển mọi ô còn lại cách bit 0 một vị trí và chèn màu mới ở bit 0. Mặt nạ phải được cắt ngắn bằng`full`, nếu không nó sẽ phát triển vượt quá chiều rộng cấu hình đã chọn. 

Các ô trống buộc phải tô màu bằng không. Xử lý chúng theo cách này là điều làm cho việc xử lý ranh giới trở nên thống nhất: một ô trống và mặt bên ngoài là cùng một mặt của mặt phẳng nhúng vì đầu vào đảm bảo rằng không có vùng trống kèm theo. 

Việc xây dựng lại không lưu trữ con trỏ cha cho mọi trạng thái. Thay vào đó, khi trạng thái ban đầu được biết là thành công, nó sẽ thử lại các màu tiếp theo có thể có và hỏi DP đã ghi nhớ xem trạng thái kết quả vẫn có thể hoàn thành hay không. Điều này sẽ lưu một mảng cha mẹ riêng biệt. 

Biểu đồ cuối cùng được xây dựng từ những thay đổi về màu sắc thay vì bằng cách thêm mọi cạnh của mỗi ô xưởng đã chọn. Sự khác biệt này quan trọng. Nếu hai mặt nhà liền kề đều có cùng một màu thì không được sử dụng cạnh chung của chúng. Nếu màu của chúng khác nhau thì cạnh chung chính xác là một hành lang trong biểu đồ chuyển tiếp. 

Thuật toán Hierholzer được sử dụng sau khi đồ thị đã được xây dựng vì tất cả các độ đều là số chẵn và đồ thị được kết nối. Chuỗi đỉnh được tạo ra chứa đúng một đỉnh nhiều hơn số hành lang được sử dụng, bắt đầu và kết thúc ở cùng một đỉnh và không bao giờ lặp lại một hành lang. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là```
3 3
***
***
.**
```Màu khuôn mặt hợp lệ có thể được xem là chọn một số ô phân xưởng làm màu một và coi bên ngoài là màu 0. DP quét các ô trong khi kiểm tra từng nút lưới mới hoàn thành. 

| Vị trí | Ô hiện tại | Màu được chọn | Biên giới sau bước | 
| --- | --- | --- | --- | 
| (0,0) |`*`| 1 |`...1`| 
| (0,1) |`*`| 0 |`..01`| 
| (0,2) |`*`| 1 |`.101`| 
| (1,0) |`*`| 0 |`1010`| 
| (1,1) |`*`| 1 |`0101`| 
| (1,2) |`*`| 0 |`1010`| 
| (2,0) |`.`| 0 |`0100`| 
| (2,1) |`*`| 1 |`1001`| 
| (2,2) |`*`| 0 |`0010`| 

Màu chính xác được chương trình chọn có thể khác nhau vì tồn tại một số màu hợp lệ. Điều quan trọng là mọi nút quan trọng đều nhìn thấy cả hai màu giữa các mặt tới của nó. Các cạnh thay đổi màu thu được tạo thành một đồ thị Euler được kết nối và thuật toán của Hierholzer biến các cạnh đó thành một tuyến kín hợp lệ. Mẫu chính thức sử dụng 16 hành lang, mặc dù bài toán chấp nhận mọi tuyến đường hợp lệ. 

### Mẫu 2 

Mẫu thứ hai là```
1 4
****
```Chỉ có một dãy ô xưởng. Các nút quan trọng là mười góc của lưới (2 \times 5). Các cạnh chuyển tiếp có thể tạo thành chu vi của toàn bộ dải. 

| Vị trí | Tế bào | Lựa chọn màu sắc | Ràng buộc liên quan | 
| --- | --- | --- | --- | 
| (0,0) |`*`| 1 | Nút trên cùng bên trái phải được che | 
| (0,1) |`*`| 0 | Nút trên cùng liền kề nhìn thấy màu 1 và 0 | 
| (0,2) |`*`| 1 | Nút trên cùng liền kề nhìn thấy màu 0 và 1 | 
| (0,3) |`*`| 0 | Nút trên cùng liền kề nhìn thấy màu 1 và 0 | 

Một lần nữa, màu sắc chính xác có thể khác nhau. Thực tế quan trọng là mọi nút ranh giới đều có sự thay đổi màu sắc trên cạnh tới, do đó các hành lang được chọn tạo thành một biểu đồ chẵn được kết nối. Mẫu chính thức có lộ trình gồm mười hành lang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(mn2^w)) | Có (mn) vị trí hồ sơ, tối đa (2^w) mặt nạ và nhiều nhất hai lựa chọn màu cho mỗi trạng thái | 
| Không gian | (O(mn2^w)) | Bảng ghi nhớ có thể chứa một trạng thái cho mỗi vị trí và mặt nạ biên giới | 

Ở đây (w=\min(m,n)), vì lưới được chuyển vị trước DP nếu cần. Với (w\le20), cấu hình có tối đa (2^{20}=1.048.576) mặt nạ. Sự phụ thuộc theo hàm mũ là vào kích thước lưới nhỏ hơn chứ không phải vào tất cả (mn) ô, đây là mức giảm chính từ tìm kiếm vũ phu (2^{mn}). Vấn đề ban đầu có (m,n\le20) và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Tuyến đầu ra không phải là duy nhất, vì vậy các thử nghiệm nên xác nhận cấu trúc của tuyến được trả về thay vì so sánh một tuyến.`Yes`trả lời từng byte một. Trình trợ giúp bên dưới kiểm tra xem mọi nút quan trọng đã được truy cập chưa, mọi di chuyển đều giữa các nút lưới liền kề, mỗi hành lang được sử dụng tối đa một lần và mọi cạnh được sử dụng thực sự là một hành lang.```python
# The solution above defines solve_case(grid).

import io
import sys

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        m, n = map(int, input().split())
        grid = [input().strip() for _ in range(m)]
        return solve_case(grid)
    finally:
        sys.stdin = old_stdin

def validate(inp: str, out: str, expected_possible: bool):
    lines = out.strip().splitlines()
    assert lines

    if not expected_possible:
        assert lines[0] == "No"
        return

    assert lines[0] == "Yes"

    m, n = map(int, inp.splitlines()[0].split())
    grid = inp.splitlines()[1:1 + m]

    k = int(lines[1])
    route = [tuple(map(int, line.split())) for line in lines[2:]]

    assert len(route) == k + 1
    assert route[0] == route[-1]

    important = set()

    for r in range(m):
        for c in range(n):
            if grid[r][c] == '*':
                important.add((r, c))
                important.add((r + 1, c))
                important.add((r, c + 1))
                important.add((r + 1, c + 1))

    assert important.issubset(set(route))

    used = set()

    for a, b in zip(route, route[1:]):
        ar, ac = a
        br, bc = b

        assert 0 <= ar <= m
        assert 0 <= br <= m
        assert 0 <= ac <= n
        assert 0 <= bc <= n

        assert abs(ar - br) + abs(ac - bc) == 1

        edge = tuple(sorted((a, b)))
        assert edge not in used
        used.add(edge)

        if ar == br:
            r = ar
            c = min(ac, bc)
            workshop = (
                r > 0 and grid[r - 1][c] == '*'
            ) or (
                r < m and grid[r][c] == '*'
            )
        else:
            r = min(ar, br)
            c = ac
            workshop = (
                c > 0 and grid[r][c - 1] == '*'
            ) or (
                c < n and grid[r][c] == '*'
            )

        assert workshop

# Provided sample 1.
sample1 = """\
3 3
***
***
.**
"""
validate(sample1, run(sample1), True)

# Provided sample 2.
sample2 = """\
1 4
****
"""
validate(sample2, run(sample2), True)

# Minimum-size workshop.
case3 = """\
1 1
*
"""
validate(case3, run(case3), True)

# No workshops at all.
case4 = """\
2 3
...
...
"""
validate(case4, run(case4), True)

# Full 2 x 2 factory, whose 3 x 3 grid graph has no spanning
# Eulerian subgraph.
case5 = """\
2 2
**
**
"""
validate(case5, run(case5), False)

# A maximum-width one-row factory.
case6 = "1 20\n" + "*" * 20 + "\n"
validate(case6, run(case6), True)

# A maximum-size full grid. The forced outer boundary leaves
# an interior grid graph that cannot be included in one Eulerian route.
case7 = "20 20\n" + "\n".join(["*" * 20] * 20) + "\n"
validate(case7, run(case7), False)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 3 / *** / *** / .**`|`Yes`| Cung cấp mẫu, các nút nội bộ và hội thảo không phải hình chữ nhật | 
|`1 4 / ****`|`Yes`| Cung cấp mẫu và nhà máy mỏng | 
|`1 1 / *`|`Yes`| Hội thảo tối thiểu và đồ thị chỉ có ranh giới | 
|`2 3 / ... / ...`|`Yes`| Không có nút quan trọng và tuyến đường có độ dài bằng 0 | 
|`2 2 / ** / **`|`No`| Trường hợp Euler không thể nhỏ nhất không tầm thường | 
|`1 20 / ********************`|`Yes`| Chiều rộng biên dạng tối đa và ranh giới dài | 
|`20 20 / all *`|`No`| Lưới kích thước tối đa và vật cản ranh giới bắt buộc | 

## Vỏ cạnh 

Đối với một xưởng duy nhất,```
1 1
*
```bốn nút quan trọng duy nhất là các góc của ô đó. DP màu mặt có thể tô màu mặt một và mặt số 0 bên ngoài. Mỗi cạnh biên được chọn, tạo ra một chu trình Euler bốn cạnh. Do đó, đầu ra bắt đầu bằng`Yes`, theo sau là`4`và năm nút lưới tạo thành một hình vuông khép kín. 

Đối với một bản đồ trống,```
1 1
.
```không có nút quan trọng nào cả. DP gán màu 0 cho ô duy nhất và biểu đồ chuyển tiếp không chứa cạnh. Việc triển khai xử lý việc này trước khi chạy Hierholzer và in tuyến đường không có hành lang bao gồm một nút lưới duy nhất. 

Vì```
2 2
**
**
```mỗi một trong bốn góc ngoài chỉ có một ô xưởng sự cố nên các ô đó buộc phải tô màu một ô. Điều đó buộc toàn bộ ranh giới bên ngoài vào biểu đồ chuyển tiếp. Khi đó, các đỉnh biên đã có bậc hai theo yêu cầu, ngăn không cho bốn cạnh dẫn đến tâm được chọn. Trung tâm rất quan trọng nhưng lại bị cô lập nên không có tuyến đường hợp lệ nào tồn tại. Hồ sơ DP phát hiện ra mâu thuẫn tương tự thông qua các ràng buộc màu sắc khuôn mặt cục bộ của nó. 

Đối với trường hợp ranh giới một hàng```
1 2
**
```hai ô xưởng có thể nhận được màu sắc trái ngược nhau. Cạnh được chia sẻ sẽ được chọn và các cạnh bên ngoài liền kề với ô một màu cũng được chọn. Biểu đồ được chọn kết quả được kết nối và mọi nút quan trọng đều có mức độ dương, do đó Hierholzer tạo ra một tuyến đường khép kín mà không lặp lại hành lang. 

Việc kiểm tra ranh giới trong quá trình thực hiện đáng được quan tâm đặc biệt. Một nút bên trong được điều chỉnh bởi tối đa bốn ô phân xưởng, nhưng một nút ở ranh giới bên ngoài chỉ có một hoặc hai ô thực tế và mặt bên ngoài phải được coi là màu 0. Việc quên mặt bên ngoài là nguyên nhân phổ biến dẫn đến kết quả dương tính giả, đặc biệt đối với một xưởng sản xuất đơn lẻ hoặc một nhà máy mỏng (1 \times n).
