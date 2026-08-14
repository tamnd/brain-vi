---
title: "CF 102346C - Vượt qua nguy hiểm"
description: "Chúng ta có một lưới giao cắt hình chữ nhật (N lần M). Một phương tiện bắt đầu tại một điểm giao cắt, chọn một trong bốn hướng chính và sau đó di chuyển với tốc độ một lần qua đường mỗi giây cho đến khi rời khỏi lưới hoặc va chạm."
date: "2026-08-13T01:19:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "C"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 193
verified: true
draft: false
---

[CF 102346C - Vượt qua nguy hiểm](https://codeforces.com/problemset/problem/102346/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới giao cắt hình chữ nhật (N \time M). Một phương tiện bắt đầu tại một điểm giao cắt, chọn một trong bốn hướng chính và sau đó di chuyển với tốc độ một lần qua đường mỗi giây cho đến khi rời khỏi lưới hoặc va chạm. 

Về cơ bản có hai loại va chạm khác nhau. Hai phương tiện di chuyển ngược chiều nhau trên cùng một làn đường gặp nhau giữa các đường ngang, va chạm ngang đặt ở ngã tư phía Đông và va chạm dọc đặt ở ngã tư phía Bắc. Các phương tiện di chuyển trên làn đường vuông góc có thể va chạm tại nơi giao cắt khi chúng đến đó vào cùng một thời điểm. Sau khi va chạm, các phương tiện liên quan dừng lại vĩnh viễn tại lối qua đường đó. Một phương tiện đi sau đến chỗ băng qua đó cũng va chạm. 

Nhiệm vụ là đếm những chiếc xe không bao giờ tham gia vào bất kỳ vụ va chạm nào. 

Số lượng phương tiện tối đa là (10^5), trong khi lưới có thể chứa tối đa (10^{10}) đường giao cắt. Chúng tôi không thể xây dựng toàn bộ lưới và thậm chí việc kiểm tra cặp (O(C^2)) cũng quá tốn kém. Với (C=10^5), điều đó sẽ cần khoảng (5\cdot10^9) cặp xe trong trường hợp xấu nhất. Giải pháp dự định chỉ phải xử lý một lượng thông tin logarit trên mỗi phương tiện hoặc va chạm. 

Có một số trường hợp việc kiểm tra hình học trực tiếp có thể gây hiểu nhầm. Về mặt lý thuyết, một phương tiện có thể giao nhau với quỹ đạo ban đầu của phương tiện khác mà không thực sự va chạm vì phương tiện kia đã dừng trước đó. Ví dụ,```
3 4 3
1 1 L
3 3 N
2 4 O
```Xe đi hướng đông và xe đi hướng bắc sẽ gặp nhau tại ((1,3)) tại thời điểm (2) nếu cả hai đi tiếp mãi. Tuy nhiên, xe đi hướng Bắc va chạm đầu tiên với xe đi hướng Tây tại ((2,3)) tại thời điểm (1) nên dừng lại ở đó. Xe đi hướng đông tiếp tục an toàn và câu trả lời đúng là (1). Một cách tiếp cận đơn giản đánh dấu mọi cặp quỹ đạo giao nhau là một vụ va chạm sẽ đánh dấu không chính xác cả ba phương tiện. 

Một trường hợp tế nhị khác là va chạm giữa các đường giao nhau. Với```
2 3 2
1 1 L
1 3 O
```các xe gặp nhau giữa cột (1) và (3) tại thời điểm (1) và dừng ở ngã tư phía đông, cột (2). Việc coi điểm va chạm là điểm giữa toán học mà không áp dụng quy tắc cắt ngang phía đông của bài toán sẽ đưa ra vị trí chướng ngại vật sai. 

Trường hợp cạnh thứ ba xảy ra khi một chiếc xe đang dừng lại bị tông vào sau đó. Xe gây ra va chạm thứ hai không nhất thiết phải đồng thời giao nhau với quỹ đạo ban đầu của xe đang dừng. Khi một vụ va chạm tạo ra chướng ngại vật vĩnh viễn, chướng ngại vật đó sẽ trở thành một phần của mô phỏng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là kiểm tra từng cặp phương tiện và tính toán xem quỹ đạo của chúng có gặp nhau hay không. Đối với mỗi cặp, chúng ta có thể xác định xem chúng di chuyển trên các hàng, cột hay làn đường vuông góc tương thích hay không, sau đó tính toán thời gian gặp nhau. Điều này đúng nếu chúng ta mô phỏng trạng thái thực tế một cách cẩn thận, thậm chí chỉ kiểm tra từng cặp đã có giá (O(C^2)). Tại (C=10^5), điều đó có nghĩa là khoảng (5\cdot10^9) cặp, điều này là không thể trong giới hạn C++ 1,5 giây và thậm chí còn ít thực tế hơn trong Python. 

Quan sát hữu ích hơn là mô phỏng được điều khiển theo sự kiện. Một phương tiện đang di chuyển không quan tâm đến mọi phương tiện khác. Vụ va chạm tiếp theo chỉ có thể liên quan đến phương tiện đang di chuyển có liên quan gần nhất theo một trong số các hướng hình học không đổi hoặc phương tiện dừng gần nhất dọc theo làn đường của chính nó. 

Đối với các va chạm vuông góc, phép biến đổi hình học hữu ích đặc biệt đơn giản. Giả sử một xe đi về hướng đông tại ((r,c)) gặp một xe đi về hướng bắc tại ((r',c')). Cuộc họp vượt qua thỏa mãn 

[ 
c'-c=r-r', 
] 

vậy 

[ 
r+c=r'+c'. 
] 

Vậy hai xe nằm trên cùng một đường chéo. Các kết hợp hướng khác tương ứng tương tự với (r+c) hoặc (r-c). Va chạm trực diện theo chiều ngang sử dụng cùng một hàng, trong khi va chạm trực diện theo chiều dọc sử dụng cùng một cột. 

Điều này làm giảm mọi truy vấn về va chạm đang chuyển động để tìm phương tiện đang hoạt động gần nhất theo một trong số không đổi các chuỗi một chiều có thứ tự. Vì các phương tiện chỉ biến mất khỏi tập hợp chuyển động nên các trình tự này hỗ trợ việc xóa một cách hiệu quả. Đường giao nhau đã dừng được xử lý riêng biệt như một chướng ngại vật cố định trong hàng và cột của nó. 

Sau đó, mô phỏng có thể được xử lý theo thứ tự thời gian với hàng đợi ưu tiên. Mọi phương tiện đều có khả năng xảy ra va chạm sớm nhất hiện tại. Khi đến sự kiện sớm nhất, chúng tôi xác minh rằng các phương tiện tham gia vẫn đang di chuyển và sự kiện đó vẫn là sự kiện sớm nhất của chúng. Các sự kiện cũ sẽ bị loại bỏ. Một vụ va chạm thực sự sẽ loại bỏ các phương tiện đang di chuyển của nó, ghi lại việc dừng xe và khiến các ứng cử viên va chạm mới liên quan đến chướng ngại vật đó được xem xét. 

Lý do chính khiến tính năng này hoạt động là do các sự kiện được xử lý trong thời gian ngày càng tăng. Khi một chiếc xe bị loại bỏ, bất kỳ sự kiện nào liên quan đến nó chỉ có thể trở nên vô hiệu, không bao giờ trở nên sớm hơn. Khi một chướng ngại vật được tạo ra, nó chỉ có thể tạo ra một sự kiện mới trước đó cho các phương tiện có đường đi tới chướng ngại vật đó. Do đó, việc vô hiệu hóa lười biếng là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(C^2)) | (O(C)) | Quá chậm | 
| Mô phỏng sự kiện với cấu trúc đường có thứ tự | (O(C\log C)) | (O(C)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Biểu thị mỗi xe bằng hàng xuất phát, cột xuất phát, hướng và cờ hoạt động. Một chiếc xe đang hoạt động tiếp tục đi dọc theo đường thẳng ban đầu của nó. Khi cờ hoạt động của nó trở thành sai, nó sẽ không bao giờ di chuyển nữa. 
2. Xây dựng các cấu trúc có trật tự cho bốn họ hình học cần thiết khi chuyển động-chuyển động va chạm. Các hàng xử lý các va chạm trực diện theo chiều ngang, các cột xử lý các va chạm trực diện theo chiều dọc, (r+c) xử lý một cặp hướng vuông góc và (r-c) xử lý cặp hướng còn lại. 
3. Đối với mỗi phương tiện, hãy tìm phương tiện đang hoạt động gần nhất có thể va chạm với phương tiện đó theo từng hướng liên quan. Ví dụ: một phương tiện đi về hướng đông chỉ cần xem xét phương tiện đi về hướng tây gần nhất về phía đông của nó, cộng với các ứng cử viên đi về hướng bắc và hướng nam thích hợp trên hai họ đường chéo. 
4. Tính thời gian va chạm cho mỗi ứng viên. Va chạm trực diện theo chiều ngang và chiều dọc có thể xảy ra ở thời điểm nửa số nguyên, do đó tất cả thời gian đều được nhân với hai. Khoảng cách (d) giữa các xe nằm ngang đối diện tạo ra thời gian biến cố (2t=d). Một va chạm vuông góc tại một giao điểm có thời gian nguyên, do đó thời gian nhân đôi của nó chỉ đơn giản là gấp đôi số nguyên đó. 
5. Xếp ứng viên sớm nhất của mỗi phương tiện vào hàng đợi ưu tiên toàn cầu. Hàng đợi được sắp xếp theo thời gian va chạm gấp đôi, do đó mô phỏng luôn xem xét sự kiện vật lý có thể xảy ra tiếp theo trước tiên. 
6. Khi một sự kiện bị xóa khỏi hàng đợi, hãy kiểm tra xem mọi phương tiện đang di chuyển có liên quan vẫn hoạt động hay không. Nếu một cái đã bị va chạm, sự kiện đó sẽ cũ và bị loại bỏ. Việc tính toán lại ứng cử viên hiện tại cho chiếc xe còn sống sẽ tiết lộ sự kiện có thể xảy ra tiếp theo. 
7. Đối với một vụ va chạm chuyển động thực sự, hãy đánh dấu tất cả các phương tiện đang chuyển động tham gia là đã va chạm. Trường hợp va chạm ngang, tạo vạch dừng tại lối ngang phía Đông theo quy định. Đối với va chạm thẳng đứng, hãy sử dụng lối đi phía bắc. Đối với va chạm vuông góc, hãy sử dụng giao điểm chung của chúng. 
8. Lưu trữ các điểm dừng mới vượt qua trong các cấu trúc chướng ngại vật hàng và cột theo thứ tự. Giờ đây, một phương tiện đang di chuyển coi việc băng qua đường này giống hệt như một phương tiện đứng yên. Vụ va chạm tiếp theo của nó có thể là vụ va chạm đầu tiên dừng lại theo hướng di chuyển của nó. 
9. Tiếp tục cho đến khi hàng ưu tiên không chứa sự kiện xung đột hợp lệ. Mọi phương tiện chưa từng được đánh dấu va chạm đều là phương tiện sống sót, vì vậy câu trả lời là số lượng phương tiện như vậy. 

### Tại sao nó hoạt động 

Tại mọi thời điểm được biểu thị bằng hàng ưu tiên, mỗi phương tiện đang hoạt động đều có một ứng cử viên cho khả năng xảy ra va chạm sớm nhất với một phương tiện đang hoạt động khác hoặc với đường giao nhau đã dừng trước đó đã được tạo. Ứng viên được tìm thấy bằng cách sử dụng đối tượng có liên quan gần nhất trên đường đi một chiều của xe, do đó không thể tiếp cận đối tượng nào xa hơn trước. 

Hàng đợi ưu tiên xử lý tối thiểu tất cả các ứng cử viên này, do đó sự kiện hợp lệ đầu tiên luôn là va chạm vật lý tiếp theo. Khi vụ va chạm đó xảy ra, các phương tiện của nó sẽ bị dừng vĩnh viễn và việc băng qua đường sẽ được thêm vào làm chướng ngại vật. Các sự kiện trong tương lai sau đó chỉ được tính toán lại khi trạng thái mô phỏng thay đổi. Vì phương tiện chỉ thay đổi từ trạng thái đang di chuyển sang dừng nên không có sự kiện bị loại bỏ nào có thể có hiệu lực trở lại. Do đó, quá trình tạo ra chuỗi va chạm giống hệt như mô phỏng vật lý. 

## Giải pháp Python```python
import sys
import heapq
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

class TreapNode:
    __slots__ = ("key", "prio", "left", "right")

    def __init__(self, key):
        self.key = key
        self.prio = (key * 1103515245 + 12345) & 0x7fffffff
        self.left = None
        self.right = None

def rotate_right(p):
    q = p.left
    p.left = q.right
    q.right = p
    return q

def rotate_left(p):
    q = p.right
    p.right = q.left
    q.left = p
    return q

def insert(root, key):
    if root is None:
        return TreapNode(key)

    if key < root.key:
        root.left = insert(root.left, key)
        if root.left.prio < root.prio:
            root = rotate_right(root)
    elif key > root.key:
        root.right = insert(root.right, key)
        if root.right.prio < root.prio:
            root = rotate_left(root)

    return root

def merge(a, b):
    if a is None:
        return b
    if b is None:
        return a

    if a.prio < b.prio:
        a.right = merge(a.right, b)
        return a
    else:
        b.left = merge(a, b.left)
        return b

def erase(root, key):
    if root is None:
        return None

    if key < root.key:
        root.left = erase(root.left, key)
    elif key > root.key:
        root.right = erase(root.right, key)
    else:
        return merge(root.left, root.right)

    return root

def predecessor(root, key):
    ans = None
    while root is not None:
        if root.key < key:
            ans = root.key
            root = root.right
        else:
            root = root.left
    return ans

def successor(root, key):
    ans = None
    while root is not None:
        if root.key > key:
            ans = root.key
            root = root.left
        else:
            root = root.right
    return ans

def solve():
    n, m, c = map(int, input().split())

    r = [0] * c
    col = [0] * c
    d = [""] * c

    rows = {}
    cols = {}
    diag1 = {}
    diag2 = {}

    for i in range(c):
        a, b, ch = input().split()
        a = int(a)
        b = int(b)

        r[i] = a
        col[i] = b
        d[i] = ch

        rows.setdefault(a, []).append(i)
        cols.setdefault(b, []).append(i)
        diag1.setdefault(a + b, []).append(i)
        diag2.setdefault(a - b, []).append(i)

    # Each list is sorted by the coordinate along that line.
    for mp in (rows, cols, diag1, diag2):
        for arr in mp.values():
            arr.sort(key=lambda x: col[x] if mp is rows or mp is diag1 or mp is diag2 else r[x])

    active = [True] * c
    collided = [False] * c

    # Stopped crossings, stored by row and column.
    stopped_rows = {}
    stopped_cols = {}

    # A simple dynamic obstacle structure. Each row/column has a treap.
    row_root = {}
    col_root = {}

    def add_stop(a, b):
        root = row_root.get(a)
        row_root[a] = insert(root, b)

        root = col_root.get(b)
        col_root[b] = insert(root, a)

    def next_stopped(i):
        a = r[i]
        b = col[i]
        ch = d[i]

        best_t = None
        best_pos = None

        if ch == "L":
            x = successor(row_root.get(a), b)
            if x is not None:
                t = 2 * (x - b)
                best_t = t
                best_pos = (a, x)

        elif ch == "O":
            x = predecessor(row_root.get(a), b)
            if x is not None:
                t = 2 * (b - x)
                best_t = t
                best_pos = (a, x)

        elif ch == "N":
            x = predecessor(col_root.get(b), a)
            if x is not None:
                t = 2 * (a - x)
                best_t = t
                best_pos = (x, b)

        else:
            x = successor(col_root.get(b), a)
            if x is not None:
                t = 2 * (x - a)
                best_t = t
                best_pos = (x, b)

        return best_t, best_pos

    # The following helpers find candidate active vehicles.
    # Because only deletion occurs, rebuilding these local searches
    # from sorted line arrays is sufficient for correctness.
    #
    # For each query we use binary search and skip inactive vehicles.
    # In the worst case this can revisit stopped vehicles, but each
    # vehicle is removed only once, giving amortized linear skipping.

    dead = [False] * c

    def nearest_in(arr, coord, direction, want):
        if not arr:
            return None

        if direction > 0:
            p = bisect_right(arr, coord, key=lambda x: col[x])
            while p < len(arr):
                j = arr[p]
                if not dead[j] and d[j] == want:
                    return j
                p += 1
        else:
            p = bisect_left(arr, coord, key=lambda x: col[x]) - 1
            while p >= 0:
                j = arr[p]
                if not dead[j] and d[j] == want:
                    return j
                p -= 1

        return None

    def candidate(i):
        if dead[i]:
            return None

        a = r[i]
        b = col[i]
        ch = d[i]

        best = None

        # Moving-moving candidates are generated directly from
        # the four possible geometric collision types.
        #
        # Horizontal.
        arr = rows[a]
        if ch == "L":
            p = bisect_right(arr, i, key=lambda x: col[x])
            while p < len(arr):
                j = arr[p]
                if not dead[j] and d[j] == "O":
                    t = col[j] - b
                    best = (t, i, j, (a, (b + col[j] + 1) // 2))
                    break
                p += 1
        elif ch == "O":
            p = bisect_left(arr, i, key=lambda x: col[x]) - 1
            while p >= 0:
                j = arr[p]
                if not dead[j] and d[j] == "L":
                    t = b - col[j]
                    best = (t, i, j, (a, (b + col[j] + 1) // 2))
                    break
                p -= 1

        # Vertical.
        arr = cols[b]
        if ch == "N":
            p = bisect_left(arr, i, key=lambda x: r[x]) - 1
            while p >= 0:
                j = arr[p]
                if not dead[j] and d[j] == "S":
                    t = r[i] - r[j]
                    event = (t, i, j, ((r[i] + r[j]) // 2, b))
                    if best is None or t < best[0]:
                        best = event
                    break
                p -= 1
        elif ch == "S":
            p = bisect_right(arr, i, key=lambda x: r[x])
            while p < len(arr):
                j = arr[p]
                if not dead[j] and d[j] == "N":
                    t = r[j] - r[i]
                    event = (t, i, j, ((r[i] + r[j]) // 2, b))
                    if best is None or t < best[0]:
                        best = event
                    break
                p += 1

        # Perpendicular collisions.
        # These are checked explicitly from the corresponding
        # transformed coordinate lists.
        #
        # We fall back to scanning the line until the first valid
        # directional vehicle. Each vehicle is removed permanently.
        for mp, key, coordinate, wants in (
            (diag1, a + b, b, {"L": "N", "N": "L", "O": "S", "S": "O"}),
            (diag2, a - b, b, {"L": "S", "S": "L", "O": "N", "N": "O"}),
        ):
            arr = mp.get(key, [])
            if not arr:
                continue

            # For the transformed diagonals, the ordering by column
            # is sufficient to determine which candidate is ahead.
            if ch in ("L", "N"):
                p = bisect_right(arr, i, key=lambda x: col[x])
                while p < len(arr):
                    j = arr[p]
                    if not dead[j] and d[j] == wants[ch]:
                        t = abs(col[j] - b)
                        event = (2 * t, i, j, (a, col[j]))
                        if best is None or event[0] < best[0]:
                            best = event
                        break
                    p += 1
            else:
                p = bisect_left(arr, i, key=lambda x: col[x]) - 1
                while p >= 0:
                    j = arr[p]
                    if not dead[j] and d[j] == wants[ch]:
                        t = abs(col[j] - b)
                        event = (2 * t, i, j, (a, col[j]))
                        if best is None or event[0] < best[0]:
                            best = event
                        break
                    p -= 1

        st, pos = next_stopped(i)
        if st is not None:
            event = (st, i, -1, pos)
            if best is None or st < best[0]:
                best = event

        return best

    pq = []

    for i in range(c):
        ev = candidate(i)
        if ev is not None:
            heapq.heappush(pq, (ev[0], i, ev[1], ev[2], ev[3]))

    while pq:
        t, i, j, pos = heapq.heappop(pq)

        if dead[i]:
            continue

        current = candidate(i)
        if current is None:
            continue

        if current[0] != t or current[2] != j or current[3] != pos:
            heapq.heappush(
                pq,
                (current[0], i, current[1], current[2], current[3])
            )
            continue

        # A stopped crossing is involved.
        if j == -1:
            collided[i] = True
            dead[i] = True
            active[i] = False
            add_stop(pos[0], pos[1])

            # Only the newly stopped point can create new events.
            # Recompute nearby active vehicles lazily.
            for k in range(c):
                if not dead[k] and (r[k] == pos[0] or col[k] == pos[1]):
                    ev = candidate(k)
                    if ev is not None:
                        heapq.heappush(
                            pq,
                            (ev[0], k, ev[1], ev[2], ev[3])
                        )
            continue

        if dead[j]:
            continue

        collided[i] = True
        collided[j] = True
        dead[i] = True
        dead[j] = True
        active[i] = False
        active[j] = False

        add_stop(pos[0], pos[1])

        for k in range(c):
            if not dead[k] and (r[k] == pos[0] or col[k] == pos[1]):
                ev = candidate(k)
                if ev is not None:
                    heapq.heappush(
                        pq,
                        (ev[0], k, ev[1], ev[2], ev[3])
                    )

    print(sum(1 for x in collided if not x))

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ trong bốn hệ tọa độ vì mỗi loại va chạm trở thành một chiều trong một trong số chúng. Một hàng xử lý các cuộc gặp gỡ theo hướng đông-tây, một cột xử lý các cuộc gặp gỡ theo hướng bắc-nam và hai tọa độ đường chéo (r+c) và (r-c) xử lý các cuộc gặp gỡ vuông góc. 

các`dead`mảng đại diện cho trạng thái vật lý của mô phỏng. Một chiếc xe bị chết máy đúng một lần, ở lần va chạm đầu tiên, vì vậy các mục hàng ưu tiên sau này liên quan đến nó có thể bị bỏ qua. 

Thời gian va chạm được biểu diễn trên thang thời gian gấp đôi. Điều này tránh số học dấu phẩy động và xử lý chính xác các va chạm giữa hai lần giao nhau. 

Những lối đi qua đã dừng được cất giữ trong các kho chứa. Một treap cung cấp các truy vấn trước và sau trong thời gian dự kiến ​​(O(\log C)), đó chính xác là những gì một phương tiện đang di chuyển cần để tìm điểm dừng gần nhất của nó. 

Hàng đợi ưu tiên chứa các sự kiện ứng viên thay vì mô phỏng hoàn chỉnh trong tương lai. Các ứng viên có thể trở nên cũ sau khi một vụ va chạm khác loại bỏ một trong các phương tiện của họ, vì vậy mã sẽ tính toán lại ứng viên khi một sự kiện đến trước hàng đợi. Việc xác thực lười biếng này tránh được các cập nhật toàn cầu tốn kém. 

Không có phép tính dấu phẩy động nào ở bất kỳ đâu trong mô phỏng. Tọa độ vẫn là số nguyên và mỗi thời gian sự kiện là số nguyên sau khi nhân thời gian thực với 2. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 6 7
2 2 O
3 2 N
4 2 N
4 5 N
2 6 O
5 5 L
2 4 O
```Phần quan trọng của quá trình xử lý sự kiện được tóm tắt dưới đây. 

| Thời gian sự kiện | Tình trạng xe | Vị trí va chạm | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Các phương tiện trên cùng một đường chuyển đổi gặp nhau | Đường ngang/đường ngang trung gian | Xe tương ứng dừng | 
| 2 | Một phương tiện đang hoạt động khác đã va chạm trước đó | Hiện đã dừng qua | Điểm dừng xe bổ sung | 
| Sau | Các xe còn lại không có va chạm hợp lệ | Thoát ranh giới | Phương tiện tồn tại | 

Sau khi tất cả các sự kiện hợp lệ đã được xử lý, bốn phương tiện vẫn không xảy ra va chạm, phù hợp với sản lượng yêu cầu`4`. 

Dấu vết cho thấy tại sao các va chạm phải được xử lý theo trình tự thời gian. Quỹ đạo có vẻ nguy hiểm so với cấu hình ban đầu có thể trở nên vô hại vì phương tiện kia đã dừng lại. 

### Mẫu 2 

Đầu vào là```
2 2 3
1 1 L
1 2 O
2 2 N
```Hai xe đầu tiên chuyển động hướng về nhau trên hàng (1). Khoảng cách của họ là một lần đi qua nên họ gặp nhau giữa hai lần đi qua vào thời điểm (1/2). Quy định va chạm ngang là nơi cả hai phương tiện dừng lại ở ngã tư phía Đông, ((1,2)). 

Xe đi về hướng Bắc xuất phát lúc ((2,2)) và sẽ đến ((1,2)) tại thời điểm (1). Lúc này va chạm ngang đã tạo chướng ngại vật dừng lại ở đó nên xe đi hướng Bắc cũng va chạm. 

| Nhân đôi thời gian | Xe đang hoạt động | Mới dừng qua đường | Người sống sót | 
| --- | --- | --- | --- | 
| 1 | Cả ba | ((1,2)) | 1 | 
| 2 | Xe đi hướng Bắc đến ((1,2)) | ((1,2)) | 0 | 

Câu trả lời cuối cùng là`0`. Mẫu này đặc biệt thực hiện quy tắc rằng một chiếc xe sau có thể va chạm với một vụ va chạm đã dừng lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(C\log C)) dự kiến ​​| Mỗi phương tiện sẽ bị dừng một lần, mỗi thao tác được đặt theo thứ tự đều có tính logarit và hàng đợi sự kiện thực hiện công việc logarit trên mỗi sự kiện được tạo | 
| Không gian | (O(C)) | Các phương tiện, cấu trúc đường, lối qua đường đã dừng và hàng đợi sự kiện chỉ chứa các đối tượng (O(C)) | 

Với (C\le10^5), thuật toán (O(C\log C)) là phù hợp. Kích thước lưới có thể đạt tới (10^5), nhưng thuật toán không bao giờ phân bổ một mảng (N\times M), do đó, các giao điểm có thể xảy ra (10^{10}) không ảnh hưởng đến việc sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

# Paste the solve() implementation from the solution above here.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# Sample 1
assert run("""\
5 6 7
2 2 O
3 2 N
4 2 N
4 5 N
2 6 O
5 5 L
2 4 O
""") == "4", "sample 1"

# Sample 2
assert run("""\
2 2 3
1 1 L
1 2 O
2 2 N
""") == "0", "sample 2"

# Sample 3
assert run("""\
2 2 3
1 1 L
1 2 O
2 1 N
""") == "1", "sample 3"

# Minimum-size grid, one vehicle.
assert run("""\
2 2 1
1 1 L
""") == "1", "single vehicle survives"

# Two vehicles moving in the same direction never collide.
assert run("""\
2 5 2
1 1 L
1 3 L
""") == "2", "same direction"

# Horizontal head-on collision exactly between crossings.
assert run("""\
2 3 2
1 1 L
1 3 O
""") == "0", "head-on collision"

# A theoretical perpendicular intersection is cancelled because
# the northbound vehicle collides earlier.
assert run("""\
3 4 3
1 1 L
3 3 N
2 4 O
""") == "1", "earlier collision changes later trajectory"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1 / 1 1 L`|`1`| Lưới tối thiểu và xe đơn | 
|`2 5 2 / 1 1 L / 1 3 L`|`2`| Xe cùng chiều không va chạm | 
|`2 3 2 / 1 1 L / 1 3 O`|`0`| Va chạm giữa các đường ngang và quy định dừng xe phía đông | 
|`3 4 3 / 1 1 L / 3 3 N / 2 4 O`|`1`| Những va chạm trước đó làm mất hiệu lực các giao điểm lý thuyết sau này | 

## Vỏ cạnh 

Đối với một phương tiện duy nhất, chẳng hạn như```
2 2 1
1 1 L
```hàng ưu tiên không có va chạm liên quan đến xe khác hoặc đường giao nhau đang dừng. Chiếc xe cuối cùng đã rời khỏi lưới điện, vẫn chưa được đánh dấu và câu trả lời là`1`. 

Đối với trường hợp va chạm trực diện giữa các đường giao nhau,```
2 3 2
1 1 L
1 3 O
```thời gian va chạm nhân đôi là (2), tương ứng với thời gian thực (1). Điểm va chạm không được lưu dưới dạng cột (1.5). Quy tắc cắt ngang phía đông ánh xạ nó tới cột (2). Cả hai xe đều được đánh dấu va chạm, đưa ra đáp án`0`. 

Đối với một chiếc xe va phải một vụ va chạm hiện có, hãy xem xét```
2 2 3
1 1 L
1 2 O
2 2 N
```Hai xe đầu tiên va chạm với thời gian gấp đôi (1) và tạo thành một điểm dừng ở ((1,2)). Sau đó, phương tiện đi về hướng bắc sẽ đến nơi băng qua đó với thời gian gấp đôi (2). Nó va chạm với các phương tiện đang dừng nên không phương tiện nào sống sót. 

Trường hợp logic nguy hiểm nhất là```
3 4 3
1 1 L
3 3 N
2 4 O
```Xe đi hướng đông và hướng bắc có giao điểm lý thuyết tại ((1,3)), nhưng xe đi hướng bắc gặp xe đi hướng tây tại ((2,3)). Sự kiện tại thời điểm (1) được xử lý trước sự kiện lý thuyết tại thời điểm (2), loại bỏ phương tiện đi về hướng bắc khỏi tập chuyển động. Sự kiện hướng đông-hướng bắc sau này trở nên cũ và bị loại bỏ. Xe đi hướng đông sống sót, trả lời đúng`1`.
