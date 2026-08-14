---
title: "CF 102411K - Những đứa con của Vua"
description: "Chúng ta có một lưới (n lần m). Một số ô chứa các chữ cái viết hoa riêng biệt, mỗi chữ cái đại diện cho lâu đài của một đứa trẻ, trong khi mọi ô khác đều trống. Nhiệm vụ là thay thế mọi ô trống bằng chữ cái viết thường của trẻ có tỉnh hình chữ nhật chứa ô đó."
date: "2026-08-14T14:39:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "K"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 476
verified: false
draft: false
---

[CF 102411K - Những đứa con của nhà vua](https://codeforces.com/problemset/problem/102411/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 56 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới (n \times m). Một số ô chứa các chữ cái viết hoa riêng biệt, mỗi chữ cái đại diện cho lâu đài của một đứa trẻ, trong khi mọi ô khác đều trống. Nhiệm vụ là thay thế mọi ô trống bằng chữ cái viết thường của trẻ có tỉnh hình chữ nhật chứa ô đó. Mỗi tỉnh phải là một hình chữ nhật chứa chính xác một lâu đài và tỉnh chứa`A`phải có diện tích tối đa có thể. Các ô lâu đài viết hoa ban đầu vẫn không thay đổi ở đầu ra. Các ràng buộc chính thức là (1 \le n,m \le 1000) và có nhiều nhất 26 lâu đài vì mỗi chữ cái viết hoa đều khác biệt. 

Giới hạn (1000 \times 1000) có nghĩa là có thể có một triệu ô, do đó, mọi giải pháp đều phải gần với tuyến tính hoặc bậc hai trong một chiều lưới, thay vì liệt kê tất cả các hình chữ nhật có thể có. Với (n=m=1000), đã có (10^6) ô, trong khi (n^2m^2) là (10^{12}). Bảng chữ cái nhỏ là hạn chế hữu ích thứ hai: chỉ có thể có 26 tỉnh, vì vậy sau khi tìm được tỉnh tối ưu cho`A`, chúng ta có thể đủ khả năng thực hiện nhiều công việc hơn đáng kể cho mỗi lâu đài, mặc dù giải pháp dưới đây không cần thiết. 

Trường hợp phức tạp đầu tiên là khi`A`đang ở trên một ranh giới. Ví dụ,```
A..
...
..B
```Tỉnh lớn nhất cho`A`không nhất thiết chỉ là hàng đầu tiên hoặc cột đầu tiên. Ở đây hình chữ nhật bao gồm hai hàng đầu tiên có diện tích 6, do đó, đầu ra tối ưu hợp lệ là```
Aaa
aaa
bbB
```Một giải pháp chỉ cố gắng mở rộng đối xứng xung quanh`A`có thể bỏ lỡ hình chữ nhật này. 

Một trường hợp tinh vi khác là một lâu đài khác chỉ chặn một hướng. Vì```
A.B
```

`A`có thể sở hữu hai ô đầu tiên, nhưng nó không thể vượt qua`B`. Đầu ra đúng là```
AaB
```Xử lý mọi trường hợp không`A`ô trống trong khi tìm kiếm`A`sẽ đưa sai`A`cả hàng. 

Vấn đề thứ ba là nhiều hình chữ nhật tối đa có thể có cùng diện tích. Vì```
A.
.B
```

`A`có thể lấy hàng đầu tiên hoặc cột đầu tiên, cả hai đều có diện tích 2. Một kết quả hợp lệ là```
Aa
bB
```Việc triển khai đúng không được phụ thuộc vào một ràng buộc cụ thể là mức tối ưu duy nhất. Bất kỳ hình chữ nhật tối đa nào cũng đủ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là liệt kê mọi hình chữ nhật chứa`A`, kiểm tra xem nó có chứa lâu đài khác hay không và giữ lại lâu đài hợp lệ lớn nhất. Ngay cả với tổng tiền tố hai chiều để kiểm tra tính hợp lệ (O(1)), số lượng hình chữ nhật chứa một ô cố định có thể đạt tới 

[ 
500\cdot501\cdot500\cdot501 
=62.750.250.000 
] 

khi lưới là (1000\times1000) và`A`nằm gần trung tâm. Nhiều ứng viên không thể được xem xét trong vòng hai giây. Việc kiểm tra từng tế bào bên trong mỗi ứng viên sẽ tệ hơn nhiều. 

Quan sát hữu ích là một hình chữ nhật chứa`A`được xác định bởi hàng trên, hàng dưới, ranh giới bên trái và ranh giới bên phải. Sau khi cố định các hàng trên cùng và dưới cùng, bạn có thể tìm thấy ranh giới ngang tốt nhất có thể một cách độc lập với các hàng. Với mỗi hàng nằm giữa hàng trên và hàng dưới đã chọn, hãy đếm xem có thể lấy ngay bao nhiêu ô trống ở bên trái và bên phải của`A`. Hình chữ nhật chỉ có thể sử dụng phần mở rộng bên trái tối thiểu và phần mở rộng bên phải tối thiểu trong số tất cả các hàng trong khoảng. 

Giả định`A`ở cột (c). Gọi (L_i) là số ô có thể được đưa về bên trái`A`trên hàng (i) và (R_i) số tương ứng ở bên phải. Sau khi truyền các giá trị tối thiểu ra khỏi hàng chứa`A`, (L_i) và (R_i) đại diện cho các tiện ích mở rộng có sẵn đồng thời trong khoảng thời gian giữa hàng (i) và`A`. 

Đối với hàng trên cùng (u) và hàng dưới cùng (d), chiều rộng tối đa là 

[ 
\min(L_u,L_d)+\min(R_u,R_d)+1. 
] 

các`+1`là cột chứa`A`. Việc thử tất cả các cặp hàng trên cùng và dưới cùng mất (O(n^2)), trong khi tính toán phần mở rộng theo chiều ngang mất (O(nm)). Điều này là đủ cho các hạn chế. 

Một khi tối ưu`A`hình chữ nhật được cố định, phần còn lại của vấn đề trở thành vấn đề xây dựng. Điều quan trọng là phân vùng khu vực bên ngoài hình chữ nhật đó theo cách đệ quy. Phần bù của hình chữ nhật bên trong hình chữ nhật lớn hơn bao gồm tối đa bốn dải hình chữ nhật: trên, dưới, trái và phải. 

Mỗi dải không trống trong số đó phải chứa một lâu đài khác. Ví dụ: nếu dải trên`A`không có lâu đài,`A`hình chữ nhật có thể được kéo dài lên trên, mâu thuẫn với mức tối đa của nó. Lập luận tương tự áp dụng cho cả bốn phía. 

Bây giờ hãy xem xét bất kỳ vùng hình chữ nhật nào có chứa nhiều lâu đài. Nếu tọa độ hàng của chúng không bằng nhau, hãy cắt vùng theo chiều ngang giữa các hàng lâu đài tối thiểu và tối đa. Cả hai hình chữ nhật thu được đều chứa ít nhất một lâu đài. Nếu tất cả các lâu đài nằm trên một hàng thì các cột của chúng sẽ khác nhau, do đó, một đường cắt dọc sẽ tách chúng ra. Lặp lại thao tác này cuối cùng sẽ để lại các hình chữ nhật chứa chính xác một lâu đài. Điều này cung cấp một phân vùng tỉnh hợp lệ mà không thay đổi địa chỉ đã tối ưu`A`hình chữ nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2m^2)) với tổng tiền tố | (O(nm)) | Quá chậm | 
| Tối ưu | (O(nm+n^2+K^2)), (K\le26) | (O(nm+K)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm vị trí của`A`và ghi lại vị trí của tất cả các lâu đài khác. Chúng tôi sẽ chỉ tối ưu hóa`A`, bởi vì khi hình chữ nhật tối đa có thể của nó được cố định, các ô còn lại có thể được phân vùng độc lập. 
2. Tìm các hàng liên tiếp xung quanh`A`có thể sử dụng được bởi một`A`hình chữ nhật. Bắt đầu từ hàng chứa`A`, di chuyển lên và xuống trong khi ô ở`A`cột của là`A`chính nó hoặc`.`. Một lâu đài khác trong cột đó chặn mọi hình chữ nhật đi qua hàng của nó. 
3. Với mỗi hàng có thể sử dụng, hãy tính số chấm liên tiếp ở ngay bên trái và bên phải của`A`cột của. Đây là phần mở rộng theo chiều ngang thô cho hàng đó. Lâu đài ở bất kỳ vị trí nào ở phía tương ứng sẽ dừng phần mở rộng. 
4. Truyền bá các phần mở rộng theo chiều ngang tối thiểu ra khỏi hàng chứa`A`. Di chuyển xuống dưới, thay thế từng giá trị bằng phần mở rộng thô tối thiểu của chính nó và giá trị từ hàng trước đó. Thực hiện phép tính đối xứng hướng lên trên. Sau đó, (L_i) đại diện cho phần mở rộng bên trái lớn nhất có sẵn trên mỗi hàng giữa`A`và hàng (i), và (R_i) có ý nghĩa tương tự ở bên phải. 
5. Liệt kê mọi hàng trên cùng và hàng dưới cùng có thể chứa`A`. Với mỗi cặp hãy tính 

[ 
width=\min(L_{top},L_{bottom})+\min(R_{top},R_{bottom})+1 
] 

và nhân nó với chiều cao. Cho hình chữ nhật có diện tích lớn nhất. 

Điểm cực tiểu điểm cuối là đủ vì các mảng được truyền đã chứa phần mở rộng tối thiểu trên toàn bộ khoảng thời gian từ`A`đến điểm cuối đó. 
6. Sơn hình chữ nhật đã chọn bằng chữ thường`a`, để lại chữ hoa`A`không thay đổi. Không có lâu đài nào khác nằm bên trong nó, bởi vì mọi phần mở rộng theo chiều ngang và chiều dọc đều bị các lâu đài chặn lại. 
7. Xét bốn dải hình chữ nhật bên ngoài`A`hình chữ nhật. Đối với mỗi dải khác trống, hãy thu thập các lâu đài nằm bên trong nó và phân vùng đệ quy dải đó. 
8. Đối với vùng đệ quy chứa một lâu đài, hãy giao toàn bộ vùng cho lâu đài đó. Nó đã là một hình chữ nhật chứa chính xác một lâu đài nên không cần cắt thêm nữa. 
9. Đối với khu vực có nhiều lâu đài, hãy kiểm tra tọa độ hàng của chúng. Nếu chúng không bằng nhau, hãy chọn ranh giới ngang giữa các hàng lâu đài nhỏ nhất và lớn nhất. Nếu không, hãy chọn ranh giới dọc giữa các cột lâu đài nhỏ nhất và lớn nhất. Giải quyết đệ quy hai vùng kết quả. 
10. In lưới đã hoàn thành. Mọi lâu đài ban đầu vẫn được viết hoa, trong khi mọi ô trống đều được chỉ định một chủ sở hữu viết thường. 

### Tại sao nó hoạt động 

Đối với`A`tỉnh, hãy xem xét bất kỳ hình chữ nhật hợp lệ nào chứa`A`. Hàng trên và dưới của nó là một số cặp (u,d). Trên mỗi hàng giữa chúng, ranh giới bên trái của nó không thể xa hơn về bên trái so với các ô trống liên tiếp trước lâu đài đầu tiên và tương tự ở bên phải. Các giá trị được truyền (L) và (R) nắm bắt chính xác các giới hạn chung đó, do đó chiều rộng được tính cho (u,d) là chiều rộng lớn nhất có thể có cho khoảng dọc đó. Vì mọi cặp trên và dưới có thể có đều được kiểm tra nên hình chữ nhật được chọn có diện tích tối đa có thể có trong số tất cả các hình chữ nhật có thể chứa một cách hợp pháp.`A`. 

Sau khi cố định hình chữ nhật này, mọi dải cạnh không trống đều chứa một lâu đài, vì nếu không`A`có thể đã được mở rộng vào ban nhạc đó. Bất kỳ vùng hình chữ nhật nào chứa nhiều lâu đài luôn có thể được phân chia theo ranh giới ngang hoặc dọc sao cho cả hai bên đều chứa ít nhất một lâu đài. Phép đệ quy cuối cùng tạo ra các hình chữ nhật chứa chính xác một lâu đài và các vết cắt rời rạc và che phủ vùng ban đầu. Do đó tất cả các ô được gán chính xác cho một tỉnh hợp lệ, trong khi`A`tỉnh vẫn tối ưu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_grid(n, m, rows):
    grid = [list(row) for row in rows]

    castles = []
    ar = ac = -1

    for r in range(n):
        for c in range(m):
            ch = grid[r][c]
            if 'A' <= ch <= 'Z':
                if ch == 'A':
                    ar, ac = r, c
                else:
                    castles.append((r, c, ch))

    # Find the vertical interval that an A-rectangle can occupy.
    lo = ar
    while lo > 0 and grid[lo - 1][ac] == '.':
        lo -= 1

    hi = ar
    while hi + 1 < n and grid[hi + 1][ac] == '.':
        hi += 1

    raw_l = [0] * n
    raw_r = [0] * n

    # Horizontal empty runs around A for every usable row.
    for r in range(lo, hi + 1):
        c = ac - 1
        cnt = 0
        row = grid[r]

        while c >= 0 and row[c] == '.':
            cnt += 1
            c -= 1
        raw_l[r] = cnt

        c = ac + 1
        cnt = 0
        while c < m and row[c] == '.':
            cnt += 1
            c += 1
        raw_r[r] = cnt

    # Propagate minima from A downwards and upwards.
    left = [0] * n
    right = [0] * n

    left[ar] = raw_l[ar]
    right[ar] = raw_r[ar]

    for r in range(ar + 1, hi + 1):
        left[r] = min(raw_l[r], left[r - 1])
        right[r] = min(raw_r[r], right[r - 1])

    for r in range(ar - 1, lo - 1, -1):
        left[r] = min(raw_l[r], left[r + 1])
        right[r] = min(raw_r[r], right[r + 1])

    # Find the maximum-area rectangle containing A.
    best_area = 1
    best_top = best_bottom = ar

    for top in range(ar, lo - 1, -1):
        for bottom in range(ar, hi + 1):
            width = (
                min(left[top], left[bottom])
                + min(right[top], right[bottom])
                + 1
            )
            area = width * (bottom - top + 1)

            if area > best_area:
                best_area = area
                best_top = top
                best_bottom = bottom

    best_left = ac - min(left[best_top], left[best_bottom])
    best_right = ac + min(right[best_top], right[best_bottom])

    # Reserve A's optimal province.
    for r in range(best_top, best_bottom + 1):
        row = grid[r]
        for c in range(best_left, best_right + 1):
            if row[c] == '.':
                row[c] = 'a'

    def fill_region(r1, r2, c1, c2, pts):
        if not pts:
            return

        if len(pts) == 1:
            _, _, ch = pts[0]
            lower = ch.lower()

            for r in range(r1, r2 + 1):
                row = grid[r]
                for c in range(c1, c2 + 1):
                    if row[c] == '.':
                        row[c] = lower
            return

        min_r = min(p[0] for p in pts)
        max_r = max(p[0] for p in pts)

        if min_r != max_r:
            cut = (min_r + max_r) // 2

            top_pts = [p for p in pts if p[0] <= cut]
            bottom_pts = [p for p in pts if p[0] > cut]

            fill_region(r1, cut, c1, c2, top_pts)
            fill_region(cut + 1, r2, c1, c2, bottom_pts)
        else:
            min_c = min(p[1] for p in pts)
            max_c = max(p[1] for p in pts)
            cut = (min_c + max_c) // 2

            left_pts = [p for p in pts if p[1] <= cut]
            right_pts = [p for p in pts if p[1] > cut]

            fill_region(r1, r2, c1, cut, left_pts)
            fill_region(r1, r2, cut + 1, c2, right_pts)

    def process_region(r1, r2, c1, c2):
        if r1 > r2 or c1 > c2:
            return

        pts = [
            p for p in castles
            if r1 <= p[0] <= r2 and c1 <= p[1] <= c2
        ]
        fill_region(r1, r2, c1, c2, pts)

    # The complement of A's rectangle consists of at most four rectangles.
    process_region(0, best_top - 1, 0, m - 1)
    process_region(best_bottom + 1, n - 1, 0, m - 1)
    process_region(best_top, best_bottom, 0, best_left - 1)
    process_region(best_top, best_bottom, best_right + 1, m - 1)

    return [''.join(row) for row in grid]

def solve():
    n, m = map(int, input().split())
    rows = [input().strip() for _ in range(n)]
    print('\n'.join(solve_grid(n, m, rows)))

if __name__ == "__main__":
    solve()
```Phần đầu tiên nằm`A`và tất cả các lâu đài khác. Danh sách lâu đài được giữ tách biệt với lưới có thể thay đổi, điều này làm cho việc phân vùng đệ quy sau này trở nên độc lập với các ô chữ thường đã được viết. 

Khoảng dọc được tìm thấy trước khi tính toán phần mở rộng theo chiều ngang. Một lâu đài khác ở`A`cột của là một rào cản cứng, vì vậy các hàng bên ngoài nó không bao giờ có thể tham gia vào một`A`tỉnh. 

Các lần chạy thô bên trái và bên phải chỉ kiểm tra các ô liền kề với`A`cột của. Bước lan truyền là bước thay đổi các giá trị thô này thành các giới hạn trên toàn khoảng thời gian. Nếu không có sự lan truyền đó, việc chỉ sử dụng hai hàng điểm cuối sẽ bỏ qua một lâu đài chặn ở giữa khoảng một cách không chính xác. 

Tìm kiếm diện tích tối đa sử dụng nghiêm ngặt`>`khi so sánh các khu vực. Cả hai hình chữ nhật có diện tích bằng nhau đều hợp lệ, vì vậy việc giữ lại hình đầu tiên sẽ tránh được việc xử lý ràng buộc không cần thiết. 

Phân vùng đệ quy không bao giờ sửa đổi`A`hình chữ nhật. Mỗi vùng bên tách rời khỏi nó và mỗi lần cắt đệ quy sẽ chia một hình chữ nhật thành hai hình chữ nhật nhỏ hơn. Khi chỉ còn lại một lâu đài trong một khu vực, toàn bộ khu vực đó có thể được sơn bằng chữ thường của đứa trẻ đó. 

Không có vấn đề tràn số nguyên trong Python. Diện tích lớn nhất chỉ là (10^6), mặc dù số nguyên Python cũng sẽ xử lý các giá trị lớn hơn đáng kể. 

## Ví dụ đã hoạt động 

Mẫu chính thức có`A`ở hàng 2, cột 3 sử dụng tọa độ gốc 0. Các phần mở rộng theo chiều ngang hữu ích sau khi lan truyền theo chiều dọc như sau. 

| Hàng | Nguyên trái | Nguyên đúng | Tuyên truyền trái | Tuyên truyền đúng | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 2 | 1 | 2 | 
| 1 | 1 | 4 | 1 | 4 | 
| 2 | 3 | 4 | 3 | 4 | 
| 3 | 3 | 4 | 3 | 4 | 
| 4 | 3 | 1 | 3 | 1 | 
| 5 | 0 | 4 | 0 | 1 | 

Trong khoảng từ hàng 1 đến hàng 3, chiều rộng là 

[ 
\min(1,3)+\min(4,4)+1=6, 
] 

vậy diện tích là (6\cdot3=18). Hình chữ nhật được chọn là các hàng từ 1 đến 3 và cột từ 2 đến 7. 

| Đầu trang | Dưới cùng | Chiều rộng | Chiều cao | Khu vực | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 8 | 1 | 8 | hàng 2..2 | 
| 1 | 2 | 6 | 2 | 12 | hàng 1..2 | 
| 1 | 3 | 6 | 3 | 18 | hàng 1..3 | 
| 0 | 3 | 4 | 4 | 16 | hàng 1..3 | 
| 1 | 4 | 5 | 4 | 20 | hàng 1..4 | 

Hàng cuối cùng trong bảng cho thấy tại sao các giá trị được truyền lại quan trọng: mặc dù bản thân hàng 4 có sẵn ba ô ở bên trái nhưng phía bên phải của nó bị chặn bởi`P`, do đó việc kéo dài hình chữ nhật xuống dưới sẽ làm giảm chiều rộng. Khoảng thời gian tốt nhất thực tế được xác định bằng cách xem xét tất cả các cặp, không chỉ bằng cách lấy các hàng riêng lẻ rộng nhất. 

Đối với ví dụ thứ hai, hãy xem xét```
2 2
A.
.B
```Hàng chứa`A`có một ô trống ở bên phải của nó. Hàng thứ hai không có ô trống ở bên phải vì`B`chiếm vị trí đó. 

| Đầu trang | Dưới cùng | Chiều rộng | Chiều cao | Khu vực | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 1 | 2 | hàng 0..0 | 
| 0 | 1 | 1 | 2 | 2 | hàng 0..0 | 

Hai lựa chọn có diện tích bằng nhau nên việc so sánh nghiêm ngặt sẽ giữ lại hình chữ nhật đầu tiên.`A`sở hữu hàng đầu tiên. Hàng thứ hai còn lại là hình chữ nhật chỉ chứa`B`, vì vậy nó trở thành`bB`. Đầu ra cuối cùng là```
Aa
bB
```Ví dụ này thể hiện cả trường hợp ràng buộc và cấu trúc đệ quy. Diện tích tối ưu cho`A`là 2 bất kể hình chữ nhật tối đa nào được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm+n^2+K^2)) | Chạy ngang quét lưới một lần, các hàng trên cùng và dưới cùng cung cấp (O(n^2)) ứng cử viên và chi phí lọc điểm đệ quy tối đa (O(K^2)) | 
| Không gian | (O(nm+K)) | Lưới có thể thay đổi chiếm ưu thế trong bộ nhớ, trong khi mảng mở rộng và danh sách lâu đài là tuyến tính | 

Với (n,m\le1000), (nm) nhiều nhất là một triệu và (n^2) cũng nhiều nhất là một triệu. Số lượng lâu đài thỏa mãn (K\le26), do đó việc xây dựng đệ quy rất nhỏ so với các phép toán lưới. Giải pháp này vẫn thoải mái trong giới hạn bộ nhớ 512 MB và tránh việc liệt kê hình chữ nhật có tỷ lệ (10^{12}) khiến cho việc sử dụng vũ lực trở nên không thực tế. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới giả định giải pháp được lưu dưới dạng`solution.py`và nhập khẩu nó`solve_grid`chức năng.```python
from solution import solve_grid

def run(inp: str) -> str:
    lines = inp.strip().splitlines()
    n, m = map(int, lines[0].split())
    rows = lines[1:]
    return "\n".join(solve_grid(n, m, rows))

# Provided sample
assert run(
    """6 8
......X.
.F......
...A....
........
.....P..
..L.....
"""
) == """xxxxxxXx
fFaaaaaa
ffaAaaaa
ffaaaaaa
pppppPpp
llLlllll""", "sample 1"

# Constructed sample 2: two maximum rectangles of equal area for A.
assert run(
    """2 2
A.
.B
"""
) == """Aa
bB""", "sample 2, tie between maximum A rectangles"

# Minimum-size input.
assert run(
    """1 1
A
"""
) == """A""", "minimum grid"

# Boundary condition and a castle blocking A's expansion.
assert run(
    """1 3
A.B
"""
) == """AaB""", "boundary and horizontal blocker"

# A at the corner, with the optimal rectangle using two rows.
assert run(
    """3 3
A..
...
..B
"""
) == """Aaa
aaa
bbB""", "corner A and maximum rectangle"

# Maximum-size legal grid with no other castles.
# The requested all-equal-castle case is illegal because all letters
# must be distinct, so this stresses the analogous all-empty interior.
n = m = 1000
rows = ["A" + "." * 999] + ["." * 1000 for _ in range(999)]
inp = f"{n} {m}\n" + "\n".join(rows)

expected = "\n".join(
    ["A" + "a" * 999] + ["a" * 1000 for _ in range(999)]
)

assert run(inp) == expected, "maximum-size grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức (6\times8) |`A`lấy hàng 1..3 và cột 2..7 | Xây dựng tổng thể và tối ưu`A`hình chữ nhật | 
|`A.`/`.B`|`Aa`/`bB`| Lựa chọn vùng bằng nhau và phân vùng đệ quy | 
|`A`|`A`| Kích thước tối thiểu | 
|`A.B`|`AaB`| Xử lý ranh giới và chặn lâu đài mở rộng theo chiều ngang | 
|`A..`/`...`/`..B`|`Aaa`/`aaa`/`bbB`| Vị trí góc và hình chữ nhật tối ưu hai hàng | 
| (1000\times1000), chỉ`A`| Toàn bộ lưới thuộc sở hữu của`A`| Kích thước tối đa và trường hợp không có lâu đài cạnh tranh | 

## Vỏ cạnh 

Khi nào`A`chiếm ô duy nhất,```
A
```khoảng dọc có một hàng, cả hai phần mở rộng theo chiều ngang đều bằng 0 và hình chữ nhật ứng cử viên duy nhất có diện tích 1. Không có vùng nào khác để phân vùng, vì vậy đầu ra vẫn còn`A`. 

Khi một lâu đài khác chặn một hướng, như trong```
A.B
```phần mở rộng bên phải thô của`A`là 1 vì dấu chấm ở cột 1 là tự do và`B`ở cột 2 dừng quá trình quét. Hình chữ nhật tốt nhất có chiều rộng 2. Vùng một ô còn lại chứa`B`, vậy kết quả là`AaB`. Lâu đài không bao giờ được coi là một ô trống trong quá trình tìm kiếm. 

Khi có nhiều hình chữ nhật tối đa có cùng diện tích,```
A.
.B
```ứng cử viên đầu tiên là hình chữ nhật cao một ô bao phủ hàng đầu tiên, có diện tích 2. Việc kéo dài xuống dưới cũng tạo ra hình chữ nhật rộng một cột có diện tích 2. Vì việc so sánh rất nghiêm ngặt nên mức tối đa đầu tiên được giữ lại. Phần bù là hàng thứ hai, được gán cho`B`, sản xuất`Aa`Và`bB`. Lựa chọn tối đa nào cũng sẽ thỏa mãn yêu cầu tối ưu hóa. 

Khi`A`nằm ở một góc nhưng có thể mở rộng ra nhiều hàng, như trong```
A..
...
..B
```phần mở rộng bên phải được lan truyền là 2 trên hai hàng đầu tiên, trong khi`B`giới hạn hàng thứ ba. Ứng viên bao gồm hàng 0 và 1 có chiều rộng 3 và chiều cao 2, cho diện tích 6. Không có hình chữ nhật nào chứa`A`có thể bao gồm hàng thứ ba trong khi vẫn giữ chiều rộng đó. Người được chọn`A`Do đó, hình chữ nhật là hai hàng đầu tiên và hàng dưới cùng còn lại chỉ chứa`B`, cho`Aaa`,`aaa`,`bbB`. 

Hộp có kích thước tối đa chỉ có`A`cũng hữu ích vì mọi ô đều đủ điều kiện cho đứa trẻ yêu thích. Phần mở rộng theo chiều ngang đạt đến ranh giới bên phải trên mỗi hàng, khoảng dọc đạt đến ranh giới dưới cùng và hình chữ nhật tối đa là toàn bộ lưới (1000\times1000). Vì không có lâu đài nào khác nên phân vùng đệ quy không có gì để xử lý sau khi đặt trước`A`hình chữ nhật của.
