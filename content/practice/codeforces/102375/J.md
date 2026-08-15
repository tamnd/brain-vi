---
title: "CF 102375J - \u041f\u043e\u0440\u0442\u0430\u043b\u044b"
description: "Mê cung là một lưới (N lần M). Một ô có thể tự do, bị chiếm bởi một bức tường đặc W hoặc bị chiếm bởi một bức tường kính G. Chỉ có thể di chuyển thông thường giữa các ô tự do liền kề. Đường viền bên ngoài bao gồm các bức tường vững chắc, vì vậy mọi tia cuối cùng đều chạm tới một bức tường vững chắc."
date: "2026-08-15T18:29:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "J"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 1543
verified: false
draft: false
---

[CF 102375J - \u041f\u043e\u0440\u0442\u0430\u043b\u044b](https://codeforces.com/problemset/problem/102375/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 25 phút 43 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mê cung là một lưới (N \times M). Một ô hoặc là tự do, bị chiếm giữ bởi một bức tường vững chắc`W`, hoặc bị chiếm bởi một bức tường kính`G`. Chuyển động thông thường chỉ có thể thực hiện được giữa các ô tự do liền kề. Đường viền bên ngoài bao gồm các bức tường vững chắc, vì vậy mọi tia cuối cùng đều chạm tới một bức tường vững chắc. 

Một phát bắn của cổng di chuyển theo một trong bốn hướng lưới cho đến khi gặp bức tường vững chắc đầu tiên. Những bức tường kính không ngăn được cú đánh. Cánh cổng được đặt ở phía bức tường đối diện với người bắn. Hậu quả quan trọng là phía hữu ích của cánh cổng là ô cuối cùng trước bức tường vững chắc đó. Nếu ô đó trống, cổng có thể được nhập một cách an toàn từ ô đó. Nếu ô đó là kính, việc đi vào cổng từ phía bên kia sẽ khiến du khách ở bên trong một bức tường kính, vì vậy cổng như vậy không thể được sử dụng theo giải pháp hợp lệ. 

Có hai màu cổng. Mỗi màu có nhiều nhất một cổng và việc chụp màu đó thường thay thế cổng trước đó. Một chuyển động vào cổng được tính là một`M`hành động, nhưng sau khi vào đó, khách du lịch sẽ xuất hiện ở cổng khác ngay lập tức. 

Đầu vào cung cấp lưới và hai ô tự do, điểm bắt đầu (S) và điểm thoát (E). Đầu ra phải chứa một chuỗi các cú đánh và chuyển động hợp lệ. Mục tiêu chính là giảm thiểu số lượng (P) cú đánh. Trong số các giải pháp có mức tối thiểu (P) đó, số lượng hành động chuyển động chỉ phải nằm trong khoảng (2NM). Tuyên bố chính thức đưa ra (N,M\le 1000), giới hạn 2 giây và bộ nhớ 512 MiB. 

Với (N,M\le1000), có thể có (10^6) ô. Điều này loại trừ các thuật toán có không gian trạng thái thậm chí chứa một số ô lưới bậc hai, chưa nói đến không gian trạng thái bậc ba. Việc truyền lưới tuyến tính hoặc gần tuyến tính là phù hợp. Bản thân đầu ra có thể chứa các chuyển động (O(NM)), do đó việc sử dụng (O(NM)) thời gian và bộ nhớ là mục tiêu tự nhiên. 

### Bắt đầu và thoát đã được kết nối 

Nếu (S) và (E) thuộc cùng một thành phần được kết nối của các ô tự do thì các cổng là không cần thiết. Câu trả lời đúng là có (P=0). Ví dụ,```
3 3
WWW
W.W
WWW
2 2
2 2
```có đầu ra```
0 0
```Giải pháp luôn khởi tạo hai cổng sẽ không tối ưu. 

### Một phát súng có thể xuyên qua kính 

Hãy xem xét Mẫu 1. Từ ô bắt đầu, bắn xuống đi qua một ô thủy tinh và sau đó chạm tới một bức tường vững chắc. Điểm cuối của cổng thông tin hữu ích là ô trống ngay trước bức tường vững chắc đó. Việc triển khai ngây thơ chỉ cho phép bắn vào một bức tường vững chắc liền kề sẽ bỏ lỡ quá trình chuyển đổi này và có thể báo cáo không chính xác rằng mê cung cần nhiều cảnh quay hơn hoặc là không thể. 

### Điểm cuối bằng kính không sử dụng được 

Một cổng thông tin có điểm cuối là một`G`cell không phải là điểm cuối dịch chuyển hữu ích. điều trị`G`vì chỉ minh bạch cho cả việc bắn súng và dịch chuyển tức thời là sai, bởi vì việc rời khỏi cổng ở phía bên kia sẽ đưa người du hành vào bức tường kính. 

Ví dụ,```
5 5
WWWWW
W.GGW
WGWGW
WGG.W
WWWWW
2 2
4 4
```có hai tế bào tự do bị cô lập. Mỗi tia không cần thiết từ một trong hai đầu đều có một ô thủy tinh ngay trước bức tường vững chắc. Các phát bắn an toàn duy nhất hướng vào chính ô hiện tại, do đó không thể dịch chuyển tức thời giữa các thành phần. Đầu ra đúng là```
-1 -1
```Việc triển khai bất cẩn chấp nhận điểm cuối bằng kính sẽ tìm đường dẫn không chính xác. 

### Hai màu cổng phải được sử dụng xen kẽ 

Sau khi dịch chuyển tức thời, cổng ở điểm cuối hiện tại vẫn ở đó. Để di chuyển đến điểm cuối mới, màu còn lại sẽ được chụp ở một nơi khác, thay thế cổng cũ có màu đó. Sau đó, đi vào cổng vẫn còn tồn tại ở điểm cuối hiện tại sẽ thực hiện dịch chuyển tức thời. 

Đây là lý do tại sao một lần dịch chuyển là đủ cho mỗi lần dịch chuyển sau lần đầu tiên. Lần dịch chuyển đầu tiên tốn hai lần vì ban đầu không có cổng nào tồn tại. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ mô hình hóa trạng thái vật lý hoàn chỉnh. Trạng thái như vậy chứa ô hiện tại của người chơi cũng như vị trí và hướng của cả hai cổng, bao gồm cả khả năng cổng đó chưa tồn tại. Có thể có (O(NM)) vị trí cổng hữu ích, vì vậy số trạng thái là (O((NM)^3)). Mỗi trạng thái có tối đa bốn hành động di chuyển và tám hành động bắn súng, đưa ra số lần chuyển đổi trong trường hợp xấu nhất theo thứ tự 

[ 
12(NM)(NM+1)^2. 
] 

Đối với (NM=10^6), đây là thứ tự chuyển tiếp (10^{19}). Lực lượng vũ phu là đúng về mặt khái niệm vì nó thể hiện rõ ràng mọi cấu hình có thể có, nhưng các vị trí cổng tạo ra quá nhiều sự kết hợp. 

Quan sát quan trọng là chúng ta thực sự không cần phải nhớ cả hai vị trí cổng. Sau khi dịch chuyển tức thời đã xảy ra, cổng tại ô hiện tại có hướng đã biết và cổng khác có thể được thay thế bằng lần chụp tiếp theo. Do đó, thông tin duy nhất liên quan đến lần dịch chuyển tiếp theo là thành phần hiện tại và ô nơi đặt cổng mới. 

Gọi một ô trống là điểm cuối cổng nếu nó nằm ngay trước bức tường vững chắc theo ít nhất một hướng. Từ một ô (x), bắn theo một hướng có chính xác một điểm cuối (y): ô ngay trước bức tường đặc đầu tiên trên tia đó. Nếu (y) rảnh thì việc bắn từ (x) có thể tạo ra một cổng an toàn tại (y). 

Bây giờ hãy xem xét hai thành phần được kết nối khác nhau của các ô tự do. Nếu có điểm cuối (x) trong thành phần (A) mà cú đánh của nó tạo ra điểm cuối an toàn (y) trong thành phần (B), chúng ta có thể sử dụng điểm đó làm cạnh có hướng (A\to B). Cạnh đầu tiên như vậy tốn hai lượt bắn. Sau khi đến (B), cổng tại điểm cuối đến đã tồn tại, do đó, việc di chuyển dọc theo cạnh thành phần khác chỉ tốn thêm một lần bắn. 

Do đó, bài toán đã trở thành bài toán đường đi ngắn nhất không có trọng số thông thường trên các thành phần được kết nối của các ô tự do. Chúng tôi có thể tìm thấy các thành phần đó bằng BFS, tạo tất cả các chuyển đổi cổng thông tin có thể có trong (O(NM)) và sau đó chạy một BFS khác trên biểu đồ thành phần. 

Sự so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((NM)^3)) trạng thái | (O((NM)^3)) | Quá chậm | 
| Tối ưu | (O(NM)) | (O(NM)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và xác định vị trí các ô bắt đầu và thoát. Chỉ điều trị`.`tế bào có thể đi bộ được. Những bức tường kính và rắn đều chặn chuyển động thông thường. 
2. Đổ lũ lên tất cả các ô còn trống. Gán cho mỗi ô trống một mã định danh thành phần được kết nối. Trong quá trình di chuyển ngang này, đánh dấu mọi ô trống cạnh một bức tường vững chắc. Một ô như vậy có thể là điểm cuối cổng thông tin an toàn. 
3. Đối với mỗi điểm cuối, hãy xác định điểm cuối mà cảnh quay sẽ tạo ra theo từng hướng trong số bốn hướng. Ví dụ, theo hướng bên trái, hãy tìm điểm đầu tiên`W`ô ở bên trái và lấy ô ngay bên phải. Chúng ta có thể tính toán các giá trị này bằng bốn lần quét tuyến tính, hai lần quét trên hàng và hai lần quét trên cột. 
4. Xây dựng đồ thị thành phần ẩn. Với mọi điểm cuối (x) và mọi hướng, gọi (y) là điểm cuối đạt được khi bắn theo hướng đó. Bỏ qua quá trình chuyển đổi nếu (y) là một ô thủy tinh hoặc nếu (x) và (y) thuộc cùng một thành phần. Nếu không, hãy thêm chuyển đổi có hướng từ thành phần (x) sang thành phần (y), ghi nhớ (x), (y) và hướng bắn. 
5. Nếu phần bắt đầu và phần thoát có cùng mã định danh thành phần, hãy tìm đường dẫn BFS thông thường từ (S) đến (E) và xuất ra nó với số lần chụp bằng 0. Không có gì để tối ưu hóa hơn nữa vì số 0 là số lần chụp nhỏ nhất có thể. 
6. Ngược lại, chạy BFS trên biểu đồ thành phần, bắt đầu từ thành phần chứa (S). Lưu trữ cho mọi thành phần mới tiếp cận điểm cuối nguồn, điểm cuối đích và hướng chụp đã tiếp cận nó. BFS phù hợp vì mỗi lần chuyển đổi thành phần thể hiện một lần dịch chuyển bổ sung và do đó thêm một lần bắn sau lần dịch chuyển đầu tiên. 
7. Nếu không thể truy cập thành phần thoát trong biểu đồ này, hãy xuất`-1 -1`. Mọi chuyển đổi cổng an toàn có thể có đều được biểu diễn dưới dạng một cạnh, do đó không còn cách nào để di chuyển giữa các thành phần ô tự do. 
8. Xây dựng lại đường dẫn thành phần ngắn nhất. Giả sử sự chuyển tiếp của nó là 

[ 
x_0\to y_0,\quad x_1\to y_1,\quad \ldots,\quad x_{k-1}\to y_{k-1}. 
] 

Lần chuyển đổi đầu tiên cần hai lần chụp. Bắn màu cam từ (x_0) về phía (y_0), sau đó bắn màu xanh lam từ (x_0) về phía bất kỳ bức tường đặc liền kề nào, do đó cổng màu xanh lam được đặt ở (x_0). Bước vào cổng màu xanh đó sẽ dịch chuyển đến (y_0). 
9. Bên trong mỗi thành phần trung gian, hãy đi từ đích của quá trình chuyển đổi trước đó (y_{i-1}) đến nguồn của quá trình chuyển đổi tiếp theo (x_i). Đây là những chuyển động tế bào tự do thông thường và không cần tiêm chích. 
10. Đối với mỗi lần chuyển đổi sau này (x_i\sang y_i), hãy chụp màu đối diện với cổng hiện có tại (x_i). Sau đó nhập cổng hiện có tại (x_i). Điều này tốn đúng một lần bắn mới và dịch chuyển khách du lịch đến (y_i). Hướng được sử dụng để vào cổng chính xác là hướng bắn đã tạo (x_i) trong lần chuyển đổi trước đó. 
11. Sau lần dịch chuyển cuối cùng, đi bộ bình thường từ điểm cuối đích đến (E). 
12. Đếm từng`O`Và`B`như một phát súng và mọi thứ`M`như một phong trào. Số lần chụp là (k+1), trong đó (k) là số lần chuyển đổi thành phần. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi lần dịch chuyển, khách du lịch đứng ở điểm cuối được tạo bởi lần bắn mới nhất và cổng ở điểm cuối đó vẫn có sẵn. Màu còn lại có thể được chuyển đến điểm cuối cần thiết cho quá trình chuyển đổi thành phần tiếp theo. Do đó, mọi cạnh có hướng của đồ thị thành phần đều có thể thực hiện được, với cạnh đầu tiên có giá trị hai lần quay và mỗi cạnh sau có giá trị một lần. 

Ngược lại, mọi dịch chuyển tức thời an toàn giữa các thành phần ô tự do khác nhau đều phải sử dụng một cổng có điểm cuối vào là một ô tự do ngay trước một bức tường vững chắc nào đó. Cảnh quay tạo ra điểm cuối khác xác định chính xác một trong các chuyển tiếp được tạo ra bởi bốn lần quét hướng của chúng tôi. Do đó, bất kỳ giải pháp hợp lệ nào cũng tạo ra một đường dẫn trong biểu đồ thành phần của chúng tôi. BFS tìm số lần chuyển đổi tối thiểu như vậy, vì vậy (k+1) là số lần chụp tối thiểu có thể. 

Giới hạn chuyển động cũng xuất phát từ việc xây dựng thành phần. Đường dẫn thành phần ngắn nhất không bao giờ lặp lại một thành phần. Bên trong mỗi thành phần được truy cập, bước đi được tạo ra sử dụng tối đa (\text{size(comComponent)}-1) bước di chuyển thông thường. Có một chuyển động vào cổng cho mỗi quá trình chuyển đổi thành phần. Nếu đường đi sử dụng các thành phần (k+1) thì tổng số chuyển động tối đa là 

\sum\text{size(thành phần)}-1 
\le NM-1, 
] 

thoải mái dưới mức yêu cầu (2NM). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from array import array
from collections import deque

DIRS = "UDLR"
DR = (-1, 1, 0, 0)

def solve_stream(readline):
    n, m = map(int, readline().split())

    rows = []
    grid = bytearray()
    for _ in range(n):
        s = readline().strip().encode()
        rows.append(s)
        grid.extend(s)

    sr, sc = map(int, readline().split())
    er, ec = map(int, readline().split())
    sr -= 1
    sc -= 1
    er -= 1
    ec -= 1

    vcount = n * m
    start = sr * m + sc
    finish = er * m + ec

    # 0 = not a boundary endpoint, 1 = safe portal endpoint.
    boundary = bytearray(vcount)

    # Direction of any solid wall adjacent to an endpoint.
    # Encoding: U=0, D=1, L=2, R=3.
    first_dir = bytearray(vcount)

    # Connected components of free cells.
    comp = array('i', [-1]) * vcount
    component_count = 0

    for i in range(vcount):
        if grid[i] != 46 or comp[i] != -1:
            continue

        cid = component_count
        component_count += 1

        q = deque([i])
        comp[i] = cid

        while q:
            x = q.popleft()
            r = x // m
            c = x - r * m

            is_boundary = False

            if grid[x - m] == 87:
                is_boundary = True
                first_dir[x] = 0
            elif grid[x + m] == 87:
                is_boundary = True
                first_dir[x] = 1
            elif grid[x - 1] == 87:
                is_boundary = True
                first_dir[x] = 2
            elif grid[x + 1] == 87:
                is_boundary = True
                first_dir[x] = 3

            if is_boundary:
                boundary[x] = 1

            y = x - m
            if grid[y] == 46 and comp[y] == -1:
                comp[y] = cid
                q.append(y)

            y = x + m
            if grid[y] == 46 and comp[y] == -1:
                comp[y] = cid
                q.append(y)

            y = x - 1
            if grid[y] == 46 and comp[y] == -1:
                comp[y] = cid
                q.append(y)

            y = x + 1
            if grid[y] == 46 and comp[y] == -1:
                comp[y] = cid
                q.append(y)

    start_comp = comp[start]
    finish_comp = comp[finish]

    # If ordinary movement already reaches the exit, no shot is needed.
    if start_comp == finish_comp:
        seen = bytearray(vcount)
        parent = array('i', [-1]) * vcount
        pdir = bytearray(vcount)

        q = deque([start])
        seen[start] = 1

        while q and not seen[finish]:
            x = q.popleft()

            for d, delta in enumerate((-m, m, -1, 1)):
                y = x + delta
                if grid[y] == 46 and not seen[y]:
                    seen[y] = 1
                    parent[y] = x
                    pdir[y] = d
                    q.append(y)

        path = []
        cur = finish
        while cur != start:
            path.append(pdir[cur])
            cur = parent[cur]
        path.reverse()

        out = ["0 {}".format(len(path))]
        out.extend("M" + DIRS[d] for d in path)
        return "\n".join(out) + "\n"

    # For every boundary cell, these arrays store the endpoint obtained
    # by shooting in the corresponding direction.
    left = array('i', [-1]) * vcount
    right = array('i', [-1]) * vcount
    up = array('i', [-1]) * vcount
    down = array('i', [-1]) * vcount

    # Horizontal sweeps.
    for r in range(n):
        base = r * m
        last_w = -1

        for c in range(m):
            x = base + c
            if grid[x] == 87:
                last_w = c
            elif boundary[x]:
                left[x] = base + last_w + 1

        next_w = m
        for c in range(m - 1, -1, -1):
            x = base + c
            if grid[x] == 87:
                next_w = c
            elif boundary[x]:
                right[x] = base + next_w - 1

    # Vertical sweeps.
    for c in range(m):
        last_w = -1

        for r in range(n):
            x = r * m + c
            if grid[x] == 87:
                last_w = r
            elif boundary[x]:
                up[x] = (last_w + 1) * m + c

        next_w = n
        for r in range(n - 1, -1, -1):
            x = r * m + c
            if grid[x] == 87:
                next_w = r
            elif boundary[x]:
                down[x] = (next_w - 1) * m + c

    # Linked lists of boundary cells, one list per component.
    head = array('i', [-1]) * component_count
    bnext = array('i', [-1]) * vcount

    for x in range(vcount):
        if boundary[x]:
            cid = comp[x]
            bnext[x] = head[cid]
            head[cid] = x

    # BFS over connected components.
    parent_comp = array('i', [-1]) * component_count
    edge_src = array('i', [-1]) * component_count
    edge_dst = array('i', [-1]) * component_count
    edge_dir = bytearray(component_count)

    parent_comp[start_comp] = start_comp
    q = deque([start_comp])

    target_arrays = (up, down, left, right)

    while q and parent_comp[finish_comp] == -1:
        cid = q.popleft()
        x = head[cid]

        while x != -1:
            for d, arr in enumerate(target_arrays):
                y = arr[x]

                if y == -1 or grid[y] != 46:
                    continue

                nc = comp[y]
                if nc == cid or parent_comp[nc] != -1:
                    continue

                parent_comp[nc] = cid
                edge_src[nc] = x
                edge_dst[nc] = y
                edge_dir[nc] = d
                q.append(nc)

                if nc == finish_comp:
                    break

            x = bnext[x]

            if parent_comp[finish_comp] != -1:
                break

    if parent_comp[finish_comp] == -1:
        return "-1 -1\n"

    # Recover component transitions in forward order.
    transitions = []
    cid = finish_comp

    while cid != start_comp:
        transitions.append(
            (edge_src[cid], edge_dst[cid], edge_dir[cid])
        )
        cid = parent_comp[cid]

    transitions.reverse()

    # Local BFS inside one free-cell component.
    seen = array('i', [0]) * vcount
    parent = array('i', [-1]) * vcount
    pdir = bytearray(vcount)
    stamp = 0

    def walk_path(a, b, cid):
        nonlocal stamp

        if a == b:
            return []

        stamp += 1
        q = deque([a])
        seen[a] = stamp

        while q:
            x = q.popleft()

            for d, delta in enumerate((-m, m, -1, 1)):
                y = x + delta

                if grid[y] != 46:
                    continue
                if comp[y] != cid:
                    continue
                if seen[y] == stamp:
                    continue

                seen[y] = stamp
                parent[y] = x
                pdir[y] = d

                if y == b:
                    q.clear()
                    break

                q.append(y)

        result = []
        cur = b

        while cur != a:
            result.append(pdir[cur])
            cur = parent[cur]

        result.reverse()
        return result

    actions = []

    # First component: walk from S to the source of the first transition.
    x0, y0, d0 = transitions[0]
    path = walk_path(start, x0, start_comp)
    actions.extend("M" + DIRS[d] for d in path)

    # First teleport needs two shots.
    actions.append("O" + DIRS[d0])
    actions.append("B" + DIRS[first_dir[x0]])

    # Enter the blue portal at x0.
    actions.append("M" + DIRS[first_dir[x0]])

    current = y0
    active_dir = d0

    # Every later teleport needs one new shot.
    for i in range(1, len(transitions)):
        x, y, d = transitions[i]

        cid = comp[current]
        path = walk_path(current, x, cid)
        actions.extend("M" + DIRS[move_d] for move_d in path)

        # The active portal at x was created by the previous transition.
        # Replace the other color with a portal at y.
        color = "B" if i % 2 == 1 else "O"
        actions.append(color + DIRS[d])

        # Enter the still existing portal at x.
        actions.append("M" + DIRS[active_dir])

        current = y
        active_dir = d

    # Final component: walk from the last portal endpoint to E.
    final_cid = comp[current]
    path = walk_path(current, finish, final_cid)
    actions.extend("M" + DIRS[d] for d in path)

    shots = len(transitions) + 1
    moves = len(actions) - shots

    out = [f"{shots} {moves}"]
    out.extend(actions)
    return "\n".join(out) + "\n"

def main():
    sys.stdout.write(solve_stream(input))

if __name__ == "__main__":
    main()
```Giai đoạn đầu tiên gắn nhãn kết nối tế bào tự do thông thường. Các tế bào thủy tinh không bao giờ được đưa vào BFS, đó chính xác là những gì chuyển động thông thường yêu cầu. Đồng thời, một ô trống được đánh dấu là điểm cuối ranh giới nếu ít nhất một trong bốn ô lân cận của nó là`W`. Việc lưu trữ`first_dir`đưa ra hướng mà một cổng có thể được đặt trực tiếp tại ô đó. 

Quét bốn hướng là phần loại bỏ khả năng mô phỏng tia bậc hai. Khi quét hàng từ trái sang phải,`last_w`là bức tường vững chắc gần nhất bên trái. Đối với ô ranh giới (x), điểm cuối của cú đánh sang trái chỉ đơn giản là ô ngay sau`last_w`. Ba mảng còn lại được tính toán đối xứng. Vì mỗi ô tham gia vào một số lượng hoạt động quét không đổi nên giai đoạn này là tuyến tính. 

Biểu đồ thành phần được tạo một cách lười biếng trong BFS của nó thay vì lưu trữ mọi cạnh của biểu đồ. Điều này giúp tiết kiệm bộ nhớ vì một thành phần có thể chứa nhiều ô biên. Khi một thành phần bị xóa khỏi hàng đợi, danh sách các ô ranh giới được liên kết của nó sẽ được quét và mỗi mục tiêu trong số bốn mục tiêu có thể có của chúng sẽ được kiểm tra. Mỗi thành phần được xử lý một lần. 

Việc tái thiết sử dụng loại BFS thứ hai. Biểu đồ thành phần cho chúng ta biết những ô nào phải được kết nối bằng cổng, nhưng nó không cho chúng ta biết cách đi bên trong một thành phần từ điểm cuối dịch chuyển tức thời trước đó đến vị trí bắn tiếp theo.`walk_path`giải quyết chính xác vấn đề cục bộ đó. Vì đường dẫn biểu đồ thành phần không bao giờ lặp lại một thành phần nên tổng số ô được khám phá bởi tất cả các lần chạy BFS cục bộ này vẫn là (O(NM)). 

Thứ tự của hai phát bắn đầu tiên là có chủ ý. Màu cam được đặt ở đích đầu tiên, màu xanh lam ở nguồn hiện tại. Đi vào cổng màu xanh lam sẽ đến cổng màu cam. Ở mỗi lần chuyển đổi sau này, màu hiện không chiếm điểm cuối nguồn sẽ được thay thế. Cổng hiện tại tại nguồn không bao giờ bị phá hủy trước khi nó được sử dụng. 

Tất cả việc lập chỉ mục lưới đều dựa trên cơ sở nội bộ. Vì mọi ô trống đều nằm hoàn toàn bên trong đường viền ngoài đặc nên các biểu thức như`x - 1`,`x + 1`,`x - m`, Và`x + m`được an toàn bất cứ khi nào chúng được đánh giá là một ô tự do. Tràn số nguyên không phải là vấn đề trong Python và tính năng nhỏ gọn`array`các thùng chứa giữ trạng thái hàng triệu ô trong mức sử dụng bộ nhớ hợp lý. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các ô tự do được chia thành thành phần phía trên chứa phần bắt đầu và thành phần phía dưới chứa phần thoát. Bản thân ô bắt đầu là điểm cuối cổng hợp lệ vì nó nằm cạnh một bức tường vững chắc ở bên trái. 

Quá trình chuyển đổi hữu ích sẽ hướng xuống ngay từ đầu. Tia đi qua ô thủy tinh, chạm tới bức tường rắn phía dưới và tạo ra một cổng ở ô tự do ngay phía trên bức tường đó. 

| Sân khấu | Ô hiện tại | Hành động | Điểm cuối cổng thông tin | Thành phần | 
| --- | --- | --- | --- | --- | 
| 1 | ((2,3)) |`OD`| Cam tại ((4,3)) | Bắt đầu thành phần | 
| 2 | ((2,3)) |`BL`| Màu xanh tại ((2,3)) | Bắt đầu thành phần | 
| 3 | ((2,3)) |`ML`| Dịch chuyển đến ((4,3)) | Thoát thành phần | 
| 4 | ((4,3)) |`ML`| Không cần cổng thông tin | Thoát thành phần | 

Hai hành động đầu tiên thiết lập dịch chuyển tức thời đầu tiên, vì vậy (P=2). Sau khi vào cổng màu xanh với`ML`, người chơi xuất hiện tại ((4,3)) và một chuyển động bình thường sẽ đến lối ra ((4,2)). Điều này cũng chứng tỏ tại sao tia sáng phải được phép truyền qua thủy tinh. 

### Mẫu 2 

Thành phần đầu tiên chứa sự bắt đầu. Quá trình chuyển đổi cổng thông tin hữu ích đầu tiên được chuẩn bị sau khi đi qua thành phần đó tới ((6,4)). Bắn lên trên xuyên qua một ô thủy tinh và chạm tới một bức tường vững chắc, tạo ra một cổng màu cam tại ((3,4)). 

Sau đó, người chơi quay lại ((2,3)), đặt cổng màu xanh lam ngay bên phải và đi vào nó. Cổng màu cam đưa người chơi đến ((3,4)), sau đó các chuyển động cuối cùng sẽ đến lối ra. 

| Sân khấu | Ô hiện tại | Hành động | Cổng thông tin hữu ích đang hoạt động | Thành phần | 
| --- | --- | --- | --- | --- | 
| 1 | ((2,3)) | Đi bộ đến ((6,4)) | Không có | Bắt đầu thành phần | 
| 2 | ((6,4)) |`OU`| Cam tại ((3,4)) | Bắt đầu thành phần | 
| 3 | ((6,4)) | Đi bộ đến ((2,3)) | Cam tại ((3,4)) | Bắt đầu thành phần | 
| 4 | ((2,3)) |`BR`| Màu xanh tại ((2,3)) | Bắt đầu thành phần | 
| 5 | ((2,3)) |`MR`| Dịch chuyển đến ((3,4)) | Thoát thành phần | 
| 6 | ((3,4)) |`MR`| Chuyển động thông thường | Thoát thành phần | 
| 7 | ((3,5)) |`MU`| Chuyển động thông thường | Thoát thành phần | 

Một lần nữa chỉ cần hai lần bắn. Đoạn đi bộ dài không ảnh hưởng đến mục tiêu vì số lần bắn vốn đã rất ít. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(NM)) | BFS thành phần, quét bốn hướng, BFS đồ thị thành phần và tái tạo đường dẫn cục bộ cùng nhau chỉ truy cập một số lượng cấu trúc lưới không đổi trên mỗi ô | 
| Không gian | (O(NM)) | Các nhãn thành phần, bốn mảng điểm cuối, danh sách ranh giới, cha mẹ BFS và trạng thái tái thiết đều là tuyến tính | 

Đối với (N,M\le1000), (NM\le10^6). Thuật toán chỉ thực hiện một số lần chuyển không đổi qua hàng triệu ô đó và lưu trữ một lượng thông tin phụ trợ tuyến tính, do đó nó phù hợp với giới hạn đã định. Chuỗi chuyển động được tạo ra cũng có nhiều nhất là các chuyển động (NM-1) trong trường hợp cổng thông tin, nằm dưới giới hạn (2NM) bắt buộc. 

## Trường hợp thử nghiệm 

Đầu ra của một vấn đề mang tính xây dựng không phải là duy nhất, do đó, các thử nghiệm nên xác nhận chuỗi hành động được tạo ra thay vì so sánh từng byte với các mẫu. Dây nịt sau đây kiểm tra số lần bắn được báo cáo, mô phỏng mọi hành động bao gồm thay thế cổng và dịch chuyển tức thời, kiểm tra xem đã đến lối ra chưa và xác minh giới hạn chuyển động (2NM).```python
import io

# Import solve_stream from the submitted solution.
# If this code is appended directly after the solution, simply remove
# the import and use solve_stream from the same file.

def run(inp: str) -> str:
    return solve_stream(io.StringIO(inp).readline)

def verify(inp: str, expected_p: int):
    out = run(inp)
    lines = out.strip().splitlines()

    first = list(map(int, lines[0].split()))
    p, s = first
    assert p == expected_p
    assert len(lines) == p + s + 1

    it = iter(inp.strip().splitlines())
    n, m = map(int, next(it).split())
    grid = [next(it) for _ in range(n)]
    sr, sc = map(int, next(it).split())
    er, ec = map(int, next(it).split())
    sr -= 1
    sc -= 1
    er -= 1
    ec -= 1

    pos = (sr, sc)
    portals = [None, None]  # 0 = orange, 1 = blue

    dirs = {
        'U': (-1, 0),
        'D': (1, 0),
        'L': (0, -1),
        'R': (0, 1),
    }

    shots = 0
    moves = 0

    def shoot_endpoint(r, c, d):
        dr, dc = dirs[d]
        nr, nc = r + dr, c + dc

        while grid[nr][nc] != 'W':
            nr += dr
            nc += dc

        return nr - dr, nc - dc, nr, nc

    for action in lines[1:]:
        typ = action[0]
        d = action[1]
        r, c = pos

        if typ in 'OB':
            color = 0 if typ == 'O' else 1
            erow, ecol, wrow, wcol = shoot_endpoint(r, c, d)

            # A glass endpoint is deadly and cannot be used by a valid solution.
            assert grid[erow][ecol] == '.'

            side = (wrow, wcol, d)

            occupied = False
            for portal in portals:
                if portal is not None and portal[2] == side:
                    occupied = True
                    break

            if not occupied:
                portals[color] = (erow, ecol, side)

            shots += 1

        else:
            assert typ == 'M'
            dr, dc = dirs[d]
            nr, nc = r + dr, c + dc

            if grid[nr][nc] == '.':
                pos = (nr, nc)
            else:
                assert grid[nr][nc] == 'W'

                used = None
                for color, portal in enumerate(portals):
                    if portal is None:
                        continue

                    pr, pc, side = portal
                    if (pr, pc) == (r, c) and side[2] == d:
                        used = color
                        break

                assert used is not None

                other = 1 - used
                assert portals[other] is not None

                tr, tc, _ = portals[other]
                assert grid[tr][tc] == '.'
                pos = (tr, tc)

            moves += 1

    assert shots == p
    assert moves == s
    assert pos == (er, ec)
    assert s <= 2 * n * m

# Provided samples.

sample1 = """\
5 5
WWWWW
WW..W
WWGWW
W...W
WWWWW
2 3
4 2
"""

sample2 = """\
7 6
WWWWWW
W..W.W
W.W..W
W.W..W
W.WG.W
W...WW
WWWWWW
2 3
2 5
"""

sample3 = """\
5 5
WWWWW
W.G.W
WW.GW
W.G.W
WWWWW
2 2
4 2
"""

verify(sample1, 2)
verify(sample2, 2)
verify(sample3, 4)

# Custom case 1: minimum grid, start equals exit.
minimum_case = """\
3 3
WWW
W.W
WWW
2 2
2 2
"""

verify(minimum_case, 0)

# Custom case 2: glass cells form a complete barrier and every nontrivial
# portal endpoint is glass, so no safe component transition exists.
impossible_case = """\
5 5
WWWWW
W.GGW
WGWGW
WGG.W
WWWWW
2 2
4 4
"""

out = run(impossible_case).strip()
assert out == "-1 -1"

# Custom case 3: the shot must cross several glass cells before reaching
# the solid border. The endpoint is the free cell immediately before W.
multi_glass_case = """\
5 5
WWWWW
W...W
WGGGW
W...W
WWWWW
2 2
4 2
"""

verify(multi_glass_case, 2)

# Custom case 4: maximum-size all-free grid. Ordinary BFS is enough,
# so the optimal number of shots is zero.
n = 1000
m = 1000
rows = ["W" + "." * (m - 2) + "W"] * (n - 2)
maximum_case = (
    f"{n} {m}\n"
    + "W" * m + "\n"
    + "\n".join(rows) + "\n"
    + "W" * m + "\n"
    + "2 2\n"
    + f"{n - 1} {m - 1}\n"
)

verify(maximum_case, 0)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | (P=2) | Dịch chuyển tức thời đầu tiên, bắn xuyên kính và vào cổng | 
| Mẫu 2 | (P=2) | Những chuyến đi dài bình thường trước và sau khi dịch chuyển | 
| Mẫu 3 | (P=4) | Một số chuyển đổi cổng liên tiếp và xen kẽ màu sắc | 
|`minimum_case`| (0) cú sút | Bắt đầu đã bằng việc thoát và xử lý không bắn | 
|`impossible_case`|`-1 -1`| Điểm cuối bằng kính không được coi là cổng an toàn | 
|`multi_glass_case`| (P=2) | Bức tường đặc gần nhất sau nhiều ô kính | 
|`maximum_case`| (0) cú sút | (1000\times1000) đầu vào và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Đối với lưới tối thiểu, chỉ có một ô trống có thể. Nếu vừa bắt đầu vừa kết thúc, việc kiểm tra thành phần sẽ thành công ngay lập tức. Thuật toán không bao giờ xây dựng biểu đồ cổng thông tin và kết quả đầu ra`0 0`, điều này là tối ưu vì không có cú đánh nào có thể cải thiện ở mức 0. 

Đối với một mê cung nơi điểm bắt đầu và điểm thoát được kết nối bởi các ô tự do thông thường, việc kiểm tra thành phần tương tự sẽ kết thúc thuật toán trước bất kỳ quá trình xử lý cổng nào. Điều này cũng cần thiết để đạt được sự tối ưu. Giải pháp dựa trên cổng thông tin có một hoặc nhiều lần chụp không thể đánh bại (P=0). 

Đối với rào chắn bằng kính, việc quét định hướng vẫn có thể tìm thấy ô mục tiêu, nhưng mục tiêu là`G`, do đó cạnh đồ thị tương ứng bị loại bỏ. Đây là sự khác biệt chính xác giữa một tế bào thủy tinh trong suốt đối với súng và an toàn để chiếm giữ sau khi dịch chuyển tức thời. Do đó, trường hợp tùy chỉnh không thể thực hiện được không có đường dẫn biểu đồ thành phần và tạo ra`-1 -1`. 

Đối với một tia chứa nhiều ô thủy tinh, theo sau là các ô tự do và sau đó là một bức tường đặc, quá trình quét không dừng lại ở kính. Nó ghi lại ô ngay trước bức tường vững chắc làm điểm cuối. Mẫu 1 và`multi_glass_case`cả hai đều thực hiện điều kiện này. Giải pháp chỉ tìm kiếm các bức tường liền kề sẽ bỏ lỡ các tuyến đường hai lần hợp lệ. 

Đối với các thành phần lặp lại, biểu đồ thành phần được tìm kiếm thay vì biểu đồ điểm cuối riêng lẻ. Điều này ngăn chặn những chuyển đổi vô ích nằm bên trong một thành phần miễn phí. Nó cũng cung cấp cho chuyển động bị ràng buộc miễn phí, bởi vì đường dẫn thành phần ngắn nhất sẽ truy cập từng thành phần nhiều nhất một lần. 

Đối với lần dịch chuyển đầu tiên, không có cổng dịch chuyển nào hiện có tại nguồn. Quá trình xây dựng rõ ràng dành hai cảnh quay, một cho đích và một cho nguồn. Mỗi lần chuyển đổi sau chỉ cần một lần chuyển đổi vì nguồn đã chứa cổng được tạo bởi lần chuyển đổi trước đó. Đây chính xác là lý do tại sao một đường dẫn thành phần chứa (k) cạnh yêu cầu (k+1) lần chụp. 

Đối với thành phần cuối cùng, không cần cổng thông tin sau khi đến. Thuật toán chỉ đơn giản là đi từ điểm cuối cuối cùng đến lối ra. Điều này quan trọng vì mục tiêu giảm thiểu các cảnh quay chứ không phải độ dài chuyển động và việc tạo ra một cổng cuối cùng không cần thiết chỉ có thể khiến câu trả lời trở nên tồi tệ hơn.
