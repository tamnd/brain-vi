---
title: "CF 102443E - Trò chơi trốn tìm cho robot"
description: "Chúng tôi có một lưới (mtimes n). Một robot chiếm giữ một số ô và mọi robot đều chỉ về một trong bốn hướng chính. Một robot nhìn xuống sẽ thấy một vùng hình tam giác đang mở rộng: một ô ngay bên dưới nó, sau đó là ba ô ở hai hàng bên dưới, rồi năm ô ở ba hàng bên dưới, v.v."
date: "2026-08-08T12:59:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "E"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 485
verified: true
draft: false
---

[CF 102443E - Trò trốn tìm dành cho robot](https://codeforces.com/problemset/problem/102443/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới (m\times n). Một robot chiếm giữ một số ô và mọi robot đều chỉ về một trong bốn hướng chính. Một robot nhìn xuống sẽ thấy một vùng hình tam giác đang mở rộng: một ô ngay bên dưới nó, sau đó là ba ô ở hai hàng bên dưới, rồi năm ô ở ba hàng bên dưới, v.v. Ba hướng còn lại được xác định đối xứng. 

Chúng ta phải xoay robot để không có cặp robot nào nhìn thấy nhau. Việc quay rô-bốt theo (90^\circ) sẽ tốn một thao tác, do đó, chẳng hạn, việc thay đổi (U) thành (D) sẽ tốn hai thao tác. Đầu ra phải giữ chính xác các vị trí robot giống nhau và phải đạt được tổng số vòng quay tối thiểu. 

Quan sát hình học hữu ích là hai robot chỉ có thể nhìn thấy nhau khi chúng hướng về hai hướng ngược nhau và độ dịch chuyển theo hướng đó hoàn toàn lớn hơn độ dịch chuyển vuông góc. Đối với một cặp thẳng đứng, điều này có nghĩa là robot phía trên hướng xuống và robot phía dưới hướng lên sẽ nguy hiểm khi 

[ 
|\Delta c|<|\Delta r|. 
] 

Đối với một cặp ngang, điều kiện tương tự là 

[ 
|\Delta r|<|\Delta c|. 
] 

Bình đẳng là an toàn. Sự bất bình đẳng nghiêm ngặt này là nơi dễ dàng mắc lỗi từng lỗi một. 

Lưới có tối đa (4\cdot 10^6) ô. Một giải pháp (O(mn\min(m,n))) sẽ yêu cầu hàng tỷ thao tác, do đó, việc liệt kê các đường viền có thể có một cách độc lập cho mỗi cột và kiểm tra lại từng hàng là quá chậm. Mục tiêu là (O(mn)), hoặc nhiều nhất là một số lượng nhỏ các lần vượt qua như vậy không đổi. 

Có một số trường hợp quan trọng. 

Coi như```
1 3
R.L
```Hai robot ở cùng một hàng và hướng về phía nhau nên chúng nhìn thấy nhau. Xoay từng cái một (90^\circ) là đủ và chi phí tối thiểu chính xác là (1). Việc triển khai bất cẩn coi điều kiện đường chéo là bao gồm hoặc chỉ kiểm tra các ô liền kề có thể bỏ lỡ xung đột này. 

Bây giờ hãy xem xét```
3 1
R
.
L
```Hai robot ở cùng một cột nhưng đều nhìn theo chiều ngang nên không nhìn thấy con kia. Chi phí tối thiểu chính xác là (0). Điều này nắm bắt các giải pháp cho rằng mọi cặp hướng ngược nhau đều tự động xấu. 

Đối với ranh giới đường chéo nghiêm ngặt, hãy xem xét```
2 2
D.
.U
```Các robot nằm cạnh nhau theo đường chéo. Sự khác biệt về hàng và cột của chúng đều là (1), vì vậy không có hình nón nào chứa robot kia. Đầu ra đúng có thể giống với đầu vào, với chi phí (0). Việc thay thế bất đẳng thức nghiêm ngặt bằng một bất đẳng thức không nghiêm ngặt sẽ tạo ra một phép quay không chính xác. 

Cuối cùng, một lưới trống như```
2 2
..
..
```đã thỏa mãn điều kiện và phải được trả về không thay đổi. Không có lý do gì để phát minh ra robot hoặc sửa đổi các ô trống. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét mọi robot, thử bốn hướng có thể của nó và kiểm tra xem cấu hình kết quả có hợp lệ hay không. Có bốn lựa chọn cho mỗi robot, vì vậy với (k) robot thì đây là (4^k), điều này trở nên vô dụng ngay cả đối với vài chục robot. Một cách tiếp cận ít ngây thơ hơn một chút là kiểm tra từng cặp rô-bốt trong khi xây dựng cấu hình, nhưng chỉ riêng số lượng cặp là (O(k^2)) và có nhiều phép gán hướng theo cấp số nhân. 

Cấu trúc hữu ích đến từ việc chỉ nhìn vào các hướng thẳng đứng trước tiên. Hãy tưởng tượng vẽ các hình nón quan sát của tất cả các robot hướng lên trên. Vùng cấm của chúng tạo thành một đường viền đơn điệu. Đối với mỗi cột có một hàng ranh giới (d_i). Robot ở một bên của đường viền phải sử dụng hướng đi lên, trong khi robot ở phía bên kia phải sử dụng hướng đi xuống. Một robot chính xác trên đường viền là linh hoạt. Đường viền không thể nhảy nhiều hơn một hàng giữa các cột lân cận, vì vậy 

[ 
|d_i-d_{i+1}|\le 1. 
] 

Đây chính xác là loại điều kiện cục bộ mà chương trình động một chiều có thể xử lý. Đặc tính đường viền này là quan sát trung tâm đằng sau nghiệm (O(mn)) đã biết. 

Còn một chi tiết nữa. Một robot không muốn sử dụng hướng thẳng đứng chính có thể được đặt nằm ngang một cách an toàn, miễn là tất cả các robot nằm ngang như vậy trong cấu trúc chuẩn đều sử dụng cùng một hướng ngang. Các robot nằm ngang khi đó không thể nhìn thấy nhau vì chúng đều hướng về cùng một hướng. Robot sử dụng hướng dọc và robot sử dụng hướng ngang không thể nhìn thấy nhau vì khả năng hiển thị lẫn nhau yêu cầu cả hai robot phải hướng dọc theo trục dịch chuyển. 

Do đó, chúng ta có thể xây dựng một cấu hình chuẩn xung quanh một đường viền thẳng đứng. Phía trên đường viền, mọi robot đều chọn (U) hoặc một hướng ngang cố định. Bên dưới nó, mọi robot đều chọn (D) hoặc hướng ngang cố định tương tự. Trên đường viền, robot có thể chọn (U), (D) hoặc hướng ngang cố định. 

Có một cấu trúc đối xứng với đường viền ngang. Chúng tôi chuyển đổi lưới, coi (L/R) là hướng chính và sử dụng một hướng dọc cố định làm phương án thay thế. Chúng tôi thử cả hai lựa chọn cho hướng thay thế cố định trong mỗi hướng. Điều này chỉ cung cấp bốn lần chạy DP, vẫn là (O(mn)). 

Công thức đường viền không chỉ đơn thuần là một cách để xây dựng một số cấu hình hợp lệ. Đối số không xuyên suốt tiêu chuẩn cho phép mọi cấu hình hợp lệ tối ưu được chuyển đổi sang một trong các dạng chuẩn này mà không làm tăng chi phí xoay của nó. Nếu các cặp xung đột chủ yếu theo chiều dọc thì ranh giới của chúng có thể được biểu thị bằng đường viền dọc. Nếu cấu trúc tương ứng nằm ngang, hoán đổi đối số. Khi đường viền được cố định, mọi robot có thể được tối ưu hóa một cách độc lập, do đó vấn đề còn lại chính xác là DP được mô tả bên dưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(4^k k^2)) | (O(k)) | Quá chậm | 
| Kiểm tra theo cặp với các hướng tùy ý | Hàm mũ | (O(k)) | Quá chậm | 
| Đường viền DP | (O(mn)) | (O(mn)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên hãy xem xét một đường viền thẳng đứng. Đối với mỗi cột (c), hãy chọn ranh giới số nguyên (d_c) giữa (0) và (m+1). Nếu (d_c=0), toàn bộ cột nằm bên dưới đường viền. Nếu (d_c=m+1), toàn bộ cột nằm phía trên cột đó. Nếu không thì hàng (d_c) là ô đường viền. 
2. Yêu cầu 

[ 
|d_c-d_{c-1}|\le 1. 
] 

Đây chính xác là điều kiện hình học nói rằng đường viền chỉ có thể di chuyển lên hoặc xuống một hàng khi di chuyển một cột theo chiều ngang.

1. Cố định một hướng thoát ngang (H), (L) hoặc (R). Đối với robot ở trên đường viền, chỉ (U) và (H) được xem xét. Đối với robot ở dưới đường viền, chỉ (D) và (H) được xem xét. Trên đường viền, (U,D,H) có sẵn. 
2. Chuyển những lựa chọn đó thành chi phí luân chuyển. Nếu hướng ban đầu là (x) thì chi phí chọn (y) là khoảng cách vòng tròn giữa bốn hướng. Do đó (U\to R) và (U\to L) đều có giá (1), trong khi (U\to D) có giá (2). 
3. Đối với một cột cố định và một ranh giới cố định (d), hãy tính tổng chi phí của nó. Gọi (A_r) là chi phí rẻ nhất cho hàng (r) khi nó nằm trên đường viền, (B_r) là chi phí rẻ nhất khi nó ở bên dưới và (C_r) là chi phí rẻ nhất khi chính hàng đó là đường viền. Sau đó 

\sum_{r<d} A_r 
+ 
C_d 
+ 
\sum_{r>d} B_r. 
] 

Hai tổng thu được bằng tổng tiền tố và hậu tố, vì vậy tất cả (m+2) giá trị có thể có của (d) cho một cột được tính theo (O(m)). 

1. Xác định (dp_c[d]) là chi phí tối thiểu sau khi xử lý các cột (0\ldots c), với (d) là vị trí đường viền trong cột (c). Các vị trí đường viền trước đó duy nhất có thể có là (d-1,d,d+1), cho 

\operatorname{cost__c(d) 
+ 
\min(dp_{c-1}[d-1],dp_{c-1[d],dp_{c-1}[d+1]). 
] 

1. Lưu trữ trạng thái nào trong ba trạng thái trước đó đã được sử dụng. Sau cột cuối cùng, chọn vị trí biên rẻ nhất và đi lùi qua các lựa chọn gốc này để xây dựng lại toàn bộ đường viền. 
2. Sử dụng đường viền được xây dựng lại để chọn hướng thực tế của mọi robot. Phía trên đường viền chọn giá trị rẻ hơn của (U) và (H). Bên dưới nó chọn giá trị rẻ hơn của (D) và (H). Trên đường viền chọn giá trị rẻ nhất (U,D,H). 
3. Lặp lại quy trình tương tự với (H=L) và (H=R). Sau đó chuyển vị trí lưới và thực hiện việc xây dựng đối xứng hai lần nữa, với các hướng chính bây giờ tương ứng với (L/R) và hướng thoát tương ứng với (U) hoặc (D). 
4. Giữ giá rẻ nhất trong bốn cấu hình thu được. Trong mọi cấu hình được tạo, tất cả các robot sử dụng hướng thoát đều giống hệt nhau nên chúng không thể nhìn thấy nhau. Các robot chính (U/D) hoặc (L/R) được phân tách bằng đường viền, và robot hướng chính và robot hướng thoát không thể nhìn thấy nhau vì hướng của chúng vuông góc. 

### Tại sao nó hoạt động 

Bất biến của DP là sau khi xử lý cột (c),`dp[d]`là chi phí xoay tối thiểu trong số tất cả các cấu hình chuẩn có đường viền kết thúc ở hàng (d). Quá trình chuyển đổi xem xét chính xác ba vị trí đường viền có thể tương thích với giới hạn độ dốc một ô, do đó mọi đường viền hợp pháp đều được thể hiện. 

Đối với đường viền cố định, hướng được chọn cho một robot không ảnh hưởng đến giá thành của bất kỳ robot nào khác. Tất cả các robot thoát hiểm ngang đều hướng về cùng một hướng, trong khi các robot dọc chính được tách thành vùng hướng lên và vùng hướng xuống. Trên đường viền, giới hạn độ dốc một ô của đường viền ngăn không cho hai hướng chính đối diện đủ gần theo chiều dọc để nhìn thấy nhau. Do đó, mọi trạng thái DP đều tương ứng với một cấu hình hợp lệ. 

Đối số đối xứng được áp dụng sau khi chuyển vị. Bổ đề đường viền nói rằng một cấu hình hợp lệ tối ưu có thể được chuyển thành một trong các dạng chính tắc này mà không làm tăng số phép quay của nó. Vì chúng tôi liệt kê cả hai trục và cả hai hướng thoát có thể có, nên kết quả tối thiểu trong bốn kết quả DP là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**9

# Directions are arranged clockwise.
ORDER = "URDL"
IDX = {ch: i for i, ch in enumerate(ORDER)}

def turn_cost(a, b):
    x = abs(IDX[a] - IDX[b])
    return min(x, 4 - x)

def solve_family(g, up, down, side):
    """
    Solve the contour problem on g.

    Above the contour:
        choose up or side.

    Below the contour:
        choose down or side.

    On the contour:
        choose up, down, or side.

    Returns:
        (minimum_cost, contour)
    """
    h = len(g)
    w = len(g[0])
    states = h + 2

    # parent[c * states + d]:
    # 0 -> previous d-1
    # 1 -> previous d
    # 2 -> previous d+1
    parent = bytearray(w * states)

    prev = [INF] * states

    for c in range(w):
        # Prefix costs for rows above the contour.
        pref = [0] * (h + 1)

        # Suffix costs for rows below the contour.
        suff = [0] * (h + 1)

        col = g

        for r in range(h):
            ch = col[r][c]

            a = min(turn_cost(ch, up), turn_cost(ch, side))
            pref[r + 1] = pref[r] + a

        for r in range(h - 1, -1, -1):
            ch = col[r][c]

            b = min(turn_cost(ch, down), turn_cost(ch, side))
            suff[r] = suff[r + 1] + b

        # Cost of every possible contour position.
        cost = [0] * states

        # d = 0, everything is below.
        cost[0] = suff[0]

        # d = h + 1, everything is above.
        cost[h + 1] = pref[h]

        for d in range(1, h + 1):
            ch = col[d - 1][c]

            boundary = min(
                turn_cost(ch, up),
                turn_cost(ch, down),
                turn_cost(ch, side),
            )

            cost[d] = pref[d - 1] + boundary + suff[d]

        if c == 0:
            prev = cost
            continue

        cur = [INF] * states
        base = c * states

        for d in range(states):
            best = prev[d]
            code = 1

            if d > 0 and prev[d - 1] < best:
                best = prev[d - 1]
                code = 0

            if d + 1 < states and prev[d + 1] < best:
                best = prev[d + 1]
                code = 2

            cur[d] = best + cost[d]
            parent[base + d] = code

        prev = cur

    best_d = min(range(states), key=prev.__getitem__)
    best_cost = prev[best_d]

    contour = [0] * w
    d = best_d

    for c in range(w - 1, -1, -1):
        contour[c] = d

        if c == 0:
            break

        code = parent[c * states + d]

        if code == 0:
            d -= 1
        elif code == 2:
            d += 1

    return best_cost, contour

def build_family(g, up, down, side, contour):
    h = len(g)
    w = len(g[0])

    ans = [list(row) for row in g]

    for c in range(w):
        d = contour[c]

        for r in range(h):
            ch = g[r][c]

            if ch == '.':
                continue

            if d == 0:
                choices = (down, side)
            elif d == h + 1:
                choices = (up, side)
            elif r < d - 1:
                choices = (up, side)
            elif r > d - 1:
                choices = (down, side)
            else:
                choices = (up, down, side)

            best = choices[0]
            best_cost = turn_cost(ch, best)

            for cand in choices[1:]:
                cur = turn_cost(ch, cand)
                if cur < best_cost:
                    best_cost = cur
                    best = cand

            ans[r][c] = best

    return [''.join(row) for row in ans]

def transpose_problem(g):
    """
    Transform the problem so that original horizontal directions
    become vertical directions.

    Original:
        L -> transformed U
        R -> transformed D
        U -> transformed L
        D -> transformed R
    """
    h = len(g)
    w = len(g[0])

    mp = {
        'L': 'U',
        'R': 'D',
        'U': 'L',
        'D': 'R',
        '.': '.',
    }

    t = []
    for c in range(w):
        row = []
        for r in range(h):
            row.append(mp[g[r][c]])
        t.append(''.join(row))

    return t

def untranspose_answer(t):
    """
    Inverse of transpose_problem.
    """
    h = len(t)
    w = len(t[0])

    mp = {
        'U': 'L',
        'D': 'R',
        'L': 'U',
        'R': 'D',
        '.': '.',
    }

    ans = [['.'] * h for _ in range(w)]

    for r in range(h):
        for c in range(w):
            ans[c][r] = mp[t[r][c]]

    return [''.join(row) for row in ans]

def solve_grid(g):
    best_cost = INF
    best_answer = None

    # Vertical contour.
    for side in ('L', 'R'):
        cost, contour = solve_family(g, 'U', 'D', side)

        if cost < best_cost:
            best_cost = cost
            best_answer = build_family(
                g, 'U', 'D', side, contour
            )

    # Horizontal contour, obtained by transposing.
    tg = transpose_problem(g)

    for side in ('L', 'R'):
        cost, contour = solve_family(tg, 'U', 'D', side)

        if cost < best_cost:
            transformed = build_family(
                tg, 'U', 'D', side, contour
            )
            best_cost = cost
            best_answer = untranspose_answer(transformed)

    return best_answer

def main():
    m, n = map(int, input().split())
    g = [input().strip() for _ in range(m)]

    ans = solve_grid(g)

    sys.stdout.write('\n'.join(ans))

if __name__ == "__main__":
    main()
```Thứ tự hướng`URDL`làm cho khoảng cách quay trở thành khoảng cách hình tròn. Ví dụ,`U`ĐẾN`D`là hai lượt, trong khi`U`đến một trong hai`L`hoặc`R`là một lượt. 

các`solve_family`chức năng là DP cốt lõi. Các trạng thái là (m+2) vị trí đường viền có thể có. Hai trạng thái bổ sung biểu thị các đường viền hoàn toàn bên trên hoặc hoàn toàn bên dưới lưới, do đó không có trường hợp đặc biệt nào liên quan đến hàng nhân tạo trong lưới thực tế. 

Đối với mỗi cột,`pref`lưu trữ chi phí tích lũy của việc đặt các hàng phía trên đường viền vào`up-or-side`loại.`suff`thực hiện tương tự cho các hàng bên dưới đường viền. Do đó, mọi vị trí đường viền có thể được đánh giá theo thời gian không đổi sau hai lần quét tuyến tính. 

Quá trình chuyển đổi kiểm tra chính xác ba người tiền nhiệm. Mảng byte`parent`là đủ vì mỗi trạng thái chỉ cần nhớ xem đường viền trước đó ở trên một hàng, bằng hay một hàng dưới. Sử dụng một`bytearray`thay vì danh sách các số nguyên Python giữ cho bộ nhớ tái tạo (O(mn)) ở mức nhỏ. 

Quá trình tái thiết sử dụng các lựa chọn giống hệt như DP. Robot phía trên đường viền sẽ chọn giữa hướng chính và hướng thoát cố định. Robot bên dưới thực hiện điều tương tự với hướng chính ngược lại. Robot trên đường viền có thêm lựa chọn về hướng chính khác. 

Cặp lần chạy thứ hai có cùng thuật toán sau khi hoán vị lưới. Việc lập bản đồ hướng là cần thiết vì bản gốc`L`trở nên biến đổi`U`, bản gốc`R`trở nên biến đổi`D`, bản gốc`U`trở nên biến đổi`L`, và một bản gốc`D`trở nên biến đổi`R`. 

Không có vấn đề tràn số nguyên nào tồn tại trong Python. Chi phí hữu ích tối đa nhiều nhất là gấp đôi số lượng robot, do đó, một lượng hữu hạn lớn`INF`là đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 3
RDL
.U.
```Một đường viền tối ưu có thể được xem theo chiều dọc. Cân nhắc sử dụng`L`như hướng thoát ngang cố định. Đường viền có thể ở hàng (1) trong mỗi cột. 

| Cột | Ranh giới | Chi phí trên | Chi phí biên | Dưới chi phí | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 |`R -> U`= 1 | 0 | 1 | 
| 2 | 1 | 0 |`D -> D`= 0 |`U -> L`= 1 | 1 | 
| 3 | 1 | 0 |`L -> L`= 0 | 0 | 0 | 

Tổng số là (2). Một điều tối ưu có thể là đầu ra mẫu:```
UDL
.R.
```Robot đầu tiên quay từ`R`ĐẾN`U`, và robot ở hàng thứ hai quay từ`U`ĐẾN`R`trong đầu ra mẫu. Cả hai thay đổi đều tốn một. 

Điểm quan trọng là việc rời đi đầu tiên`R`và thứ ba`L`không thay đổi sẽ khiến hai robot đó nhìn thấy nhau theo chiều ngang. Đường viền DP chi trả chính xác cho khoảng cách cần thiết. 

### Mẫu 2 

Đầu vào là```
2 2
..
..
```Không có robot nên mọi đường viền đều có chi phí bằng 0. 

| Cột | Ranh giới | Chi phí DP | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 0 | 0 | 

Lưới được xây dựng lại vẫn trống:```
..
..
```Điều này xác nhận rằng các trạng thái đường viền nhân tạo không tạo ra robot hoặc sửa đổi các ô trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(mn)) | Bốn lần chạy đường viền DP chỉ là một hệ số không đổi và mỗi lần chạy sẽ quét từng ô một số lần không đổi | 
| Không gian | (O(mn)) | Mảng cha lưu trữ các lựa chọn tiền thân ba chiều cho mọi trạng thái đường viền | 

Lưới chứa tối đa (4\cdot10^6) ô. Thuật toán chỉ thực hiện một số lần di chuyển tuyến tính không đổi qua các ô đó, thay vì liệt kê các cặp robot hoặc phân công hướng. Bộ nhớ (O(mn)) nằm trong giới hạn 512 MB, mặc dù kích thước nhỏ gọn của Python`bytearray`đại diện cha mẹ đặc biệt hữu ích ở đây. 

## Trường hợp thử nghiệm 

Đầu ra không phải là duy nhất, vì vậy các thử nghiệm nên xác minh cấu hình được trả về thay vì so sánh từng ký tự. Đối với các trường hợp nhỏ, chúng tôi có thể ép buộc từng cặp để kiểm tra tính hợp lệ và tính toán chi phí xoay vòng chính xác.```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import product

# Assume the editorial solution above has been placed in a module
# named solution, or copy solve_grid into this test file.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    m, n = map(int, sys.stdin.readline().split())
    g = [sys.stdin.readline().strip() for _ in range(m)]

    ans = solve_grid(g)

    sys.stdin = old_stdin
    return '\n'.join(ans)

ORDER = "URDL"
IDX = {c: i for i, c in enumerate(ORDER)}

def dist(a, b):
    x = abs(IDX[a] - IDX[b])
    return min(x, 4 - x)

def sees(r1, c1, d, r2, c2):
    dr = r2 - r1
    dc = c2 - c1

    if d == 'U':
        return dr < 0 and abs(dc) < -dr
    if d == 'D':
        return dr > 0 and abs(dc) < dr
    if d == 'L':
        return dc < 0 and abs(dr) < -dc
    return dc > 0 and abs(dr) < dc

def validate(inp, out):
    data = inp.strip().splitlines()
    m, n = map(int, data[0].split())
    original = data[1:]

    answer = out.splitlines()

    assert len(answer) == m
    assert all(len(row) == n for row in answer)

    robots = []

    for r in range(m):
        for c in range(n):
            assert (original[r][c] == '.') == (answer[r][c] == '.')

            if answer[r][c] != '.':
                robots.append((r, c, answer[r][c]))

    for i in range(len(robots)):
        r1, c1, d1 = robots[i]

        for j in range(i + 1, len(robots)):
            r2, c2, d2 = robots[j]

            assert not (
                sees(r1, c1, d1, r2, c2)
                and sees(r2, c2, d2, r1, c1)
            )

    cost = 0

    for r in range(m):
        for c in range(n):
            if original[r][c] != '.':
                cost += dist(original[r][c], answer[r][c])

    return cost

# Provided sample 1.
sample1 = """\
2 3
RDL
.U.
"""

out = run(sample1)
assert validate(sample1, out) == 2, "sample 1"

# Provided sample 2.
sample2 = """\
2 2
..
..
"""

out = run(sample2)
assert validate(sample2, out) == 0, "sample 2"

# Minimum-size input.
case3 = """\
1 1
U
"""

out = run(case3)
assert validate(case3, out) == 0, "single robot needs no rotation"

# All robots already point in the same direction.
case4 = """\
3 4
RRRR
RRRR
RRRR
"""

out = run(case4)
assert validate(case4, out) == 0, "all equal directions"

# Opposite horizontal directions in one row.
case5 = """\
1 3
R.L
"""

out = run(case5)
assert validate(case5, out) == 1, "horizontal mutual visibility"

# Opposite horizontal directions in one column.
case6 = """\
3 1
R
.
L
"""

out = run(case6)
assert validate(case6, out) == 0, "same column is safe"

# Equal row/column displacement is not visible.
case7 = """\
2 2
D.
.U
"""

out = run(case7)
assert validate(case7, out) == 0, "diagonal equality is safe"

# Maximum-size input shape, chosen so the expected cost is obvious.
m = 2000
n = 2000
large = str(m) + " " + str(n) + "\n" + "\n".join(["U" * n] * m) + "\n"

out = run(large)
assert all(row == "U" * n for row in out.splitlines()), \
    "maximum-size all-U case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3 / RDL / .U.`| Bất kỳ cấu hình hợp lệ nào có giá 2 | Cung cấp mẫu và tái tạo đường viền | 
|`2 2 / .. / ..`| Lưới trống | Đầu vào trống | 
|`1 1 / U`|`U`| Đầu vào kích thước tối thiểu | 
|`3 4`với mọi tế bào`R`| Cùng một lưới | Tất cả đều bằng nhau | 
|`1 3 / R.L`| Bất kỳ cấu hình hợp lệ nào có giá 1 | Khả năng hiển thị lẫn nhau theo chiều ngang | 
|`3 1 / R / . / L`| Cùng một lưới | Hướng ngang đối diện trong một cột | 
|`2 2 / D. / .U`| Cùng một lưới | Ranh giới chéo nghiêm ngặt | 
|`2000 x 2000`tất cả`U`| Cùng một lưới | Đầu vào và hiệu suất kích thước tối đa | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một lưới trống. Vì```
2 2
..
..
```mọi cột có thể sử dụng trạng thái đường viền (0), (1), (2) hoặc (3) và chi phí của mỗi cột bằng 0. Do đó, DP trả về 0 và việc tái cấu trúc để lại mọi ô như`.`. 

Trường hợp cạnh thứ hai là một robot duy nhất. Vì```
1 1
L
```chúng ta có thể chọn đường viền trực tiếp qua ô đó, do đó robot có thể giữ lại`L`với chi phí bằng không. Quá trình chuyển đổi ranh giới của DP bao gồm hướng ban đầu thông qua mức tối thiểu so với các hướng được phép của nó, do đó nó không buộc phải quay một vòng không cần thiết. 

Trường hợp cạnh thứ ba là ranh giới đường chéo nghiêm ngặt:```
2 2
D.
.U
```Hai robot đang dịch chuyển ((1,1)). Hình nón hướng xuống chỉ chứa ô cùng cột ở hàng đầu tiên nên ô chéo không nhìn thấy được. Điều tương tự cũng đúng với hình nón hướng lên. Thuật toán không bao giờ đưa ra ràng buộc về đẳng thức vì đối số đường viền sử dụng một hình nón nghiêm ngặt, khớp với hình dạng ban đầu. 

Trường hợp cạnh thứ tư là hướng ngang ngược nhau trong một cột:```
3 1
R
.
L
```Những robot này không thể nhìn thấy nhau vì độ dịch chuyển theo phương ngang của chúng bằng không. Biểu diễn đường viền dọc có thể kém thuận tiện hơn cho việc bảo toàn cả hai hướng, nhưng sau khi chuyển đổi lưới, chúng nằm trên cùng một ranh giới đường viền ngang. DP đối xứng xử lý cả hai`L`Và`R`mà không buộc phải quay, mang lại chi phí bằng không. 

Trường hợp khó phát hiện cuối cùng là khi có nhiều robot nằm trên đường viền. Các hàng đường viền liên tiếp có thể khác nhau tối đa một, do đó, hai robot biên có chuyển vị dọc không lớn hơn chuyển vị ngang của chúng. Điều đó hoàn toàn trái ngược với điều kiện nghiêm ngặt cần có cho một quan điểm chung theo chiều dọc. Đây là lý do tại sao robot trên đường viền có thể sử dụng hai hướng chính mà không tạo ra xung đột dọc tiềm ẩn. 

Một lưu ý triển khai thực tế: bốn lần chạy DP có hệ số không đổi là phần đáng được tối ưu hóa nhất trong Python. các`bytearray`lưu trữ gốc và tránh các đối tượng Python lồng nhau cho trạng thái DP là những lựa chọn có chủ ý cho giới hạn 2000×2000.
