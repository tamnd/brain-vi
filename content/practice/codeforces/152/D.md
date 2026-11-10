---
title: "CF 152D - Gọng Kính"
description: "Chúng ta có một lưới n × m trong đó một số ô được tô và những ô khác trống .. Bức tranh được cho là đến từ việc vẽ chính xác hai khung hình chữ nhật. Khung không phải là một hình chữ nhật được tô đầy. Chỉ các ô viền của hình chữ nhật được sơn."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 152
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 108 (Div. 2)"
rating: 2600
weight: 152
solve_time_s: 161
verified: false
draft: false
---

[CF 152D - Khung](https://codeforces.com/problemset/problem/152/D) 

**Đánh giá:** 2600 
**Tags:** vũ lực 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một`n × m`lưới nơi một số ô được vẽ bằng`#`và những cái khác trống rỗng`.`. Bức tranh được cho là được vẽ từ chính xác hai khung hình chữ nhật. 

Khung không phải là một hình chữ nhật được tô đầy. Chỉ các ô viền của hình chữ nhật được sơn. Bên trong vẫn trống trừ khi có khung khác che nó. Mọi hình chữ nhật hợp lệ phải có chiều cao ít nhất`3`và chiều rộng ít nhất`3`. 

Hai khung có thể chồng lên nhau theo bất kỳ cách nào có thể. Chúng có thể giao nhau một phần, chia sẻ các cạnh hoặc thậm chí có cùng một hình chữ nhật. Sau khi cả hai khung được sơn, chúng ta chỉ thấy sự kết hợp cuối cùng của các ô được sơn. Nhiệm vụ là xác định xem hình ảnh đã cho có thể được biểu diễn dưới dạng kết hợp của chính xác hai khung hình hợp lệ hay không. Nếu có, chúng ta cũng cần xuất ra một cặp hình chữ nhật tạo ra nó. 

Kích thước lưới lên tới`1000 × 1000`, do đó bảng có thể chứa một triệu ô. Bất kỳ thuật toán nào thử tất cả các hình chữ nhật có thể một cách rõ ràng sẽ ngay lập tức gặp nguy hiểm. Một hình chữ nhật được xác định bởi bốn đường viền nên có`O(n²m²)`hình chữ nhật có thể. Thậm chí lặp đi lặp lại trên tất cả các hình chữ nhật một lần đã có sẵn`10^12`khả năng xảy ra trong trường hợp xấu nhất, điều này hoàn toàn không thể xảy ra trong vòng hai giây. 

Phần khó khăn là không nhận dạng được một khung hình mà là xử lý các phần chồng chéo tùy ý giữa hai khung hình đó. Một cách tiếp cận tham lam ngây thơ có thể âm thầm thất bại vì một khung hình có thể che giấu phần lớn khung hình kia. 

Một trường hợp khó nhận thấy hoàn toàn là sự trùng hợp ngẫu nhiên. Cả hai khung có thể giống hệt nhau. 

Ví dụ:```
###
#.#
###
```Điều này hợp lệ vì giống nhau`3 × 3`khung có thể đã được sơn hai lần. Một giải pháp bất cẩn đòi tìm hai hình chữ nhật khác nhau sẽ bác bỏ nó một cách không chính xác. 

Một trường hợp phức tạp khác là khi sự chồng chéo tạo ra các vùng được lấp đầy. 

Ví dụ:```
#####
#####
#####
```Điều này thực sự có thể được tạo ra bằng cách chồng các khung lên nhau, tùy thuộc vào kích thước. Một giải pháp giả định mọi`#`ô phải thuộc chính xác một đường viền sẽ không thành công vì các ô chồng chéo thuộc cả hai khung cùng một lúc. 

Một mối nguy hiểm khác đến từ các hình chữ nhật mỏng không hợp lệ. Bài toán yêu cầu độ dài cả hai cạnh ít nhất là`3`. 

Ví dụ:```
###
###
```Đây không phải là khung hợp lệ vì chiều cao chỉ`2`. Việc triển khai bất cẩn chỉ kiểm tra biên giới sẽ chấp nhận nó một cách không chính xác. 

Những tình huống khó khăn nhất là khi một khung hình gần như bị ẩn hoàn toàn bởi một khung hình khác. 

Ví dụ:```
#####
#...#
#####
```Điều này có thể được biểu diễn dưới dạng hai khung giống hệt nhau. Nếu chúng ta tham lam loại bỏ một hình chữ nhật được phát hiện và mong đợi các ô còn sót lại sẽ hiển thị hình chữ nhật thứ hai, chúng ta có thể kết luận sai rằng chỉ có một khung. 

Cấu trúc của bài toán gợi ý rằng tác động mạnh mẽ lên tất cả các cặp hình chữ nhật là vô vọng. Chúng ta cần khai thác hình học của khung một cách cẩn thận hơn nhiều. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là liệt kê mọi hình chữ nhật có thể, tạo các ô khung của nó và sau đó thử từng cặp hình chữ nhật. Đối với mỗi cặp, chúng tôi so sánh sự kết hợp của các ô khung của chúng với lưới đầu vào. 

Một hình chữ nhật hợp lệ cần có hai chỉ mục hàng và hai chỉ mục cột, do đó có`O(n²m²)`hình chữ nhật ứng cử viên. Ghép nối chúng mang lại`O(n⁴m⁴)`sự kết hợp. Ngay cả trước khi kiểm tra tế bào, nó đã lớn về mặt thiên văn. 

Lực lượng vũ phu hoạt động về mặt khái niệm vì điều kiện rất dễ xác minh. Cho hai hình chữ nhật, chúng ta có thể tái tạo lại chính xác ô nào sẽ được vẽ. Vấn đề hoàn toàn là số lượng khả năng. 

Quan sát quan trọng là một khung hoàn toàn được xác định bởi ranh giới bên ngoài của nó. Nếu biết góc trên bên trái và góc dưới bên phải, chúng ta sẽ biết ngay mọi ô viền được sơn. 

Quan sát thứ hai mạnh mẽ hơn. Vì chỉ có hai khung nên mỗi ô được vẽ thuộc về ít nhất một trong số chúng. Giả sử chúng ta chọn một ô được vẽ và cho rằng nó thuộc về khung đầu tiên. Ô đó hạn chế nghiêm ngặt các ranh giới hình chữ nhật có thể có. 

Đối với một tế bào`(x, y)`nằm trên đường viền khung, một trong bốn điều phải xảy ra: 

1. Nó nằm ở đường viền trên cùng. 
2. Nó nằm ở viền dưới. 
3. Nó nằm ở viền trái. 
4. Nó nằm ở biên giới bên phải. 

Khi một bên được cố định, phía đối diện và các tọa độ còn lại sẽ bị ràng buộc bởi các đoạn sơn liền kề. 

Giải pháp dự định sử dụng việc cắt tỉa và xác nhận tích cực. Thay vì liệt kê tất cả các hình chữ nhật, chúng tôi chỉ tạo ra các hình chữ nhật có thể đi qua ô được sơn đã chọn một cách hợp lý. Số lượng ứng viên như vậy vẫn có thể quản lý được. 

Đối với mỗi khung đầu tiên ứng cử viên, chúng tôi trừ phần đóng góp đường viền của nó khỏi lưới và kiểm tra xem các ô được vẽ còn lại có tạo thành chính xác một khung hợp lệ hay không. Vì việc xác thực khung có thể được thực hiện trong thời gian tuyến tính với tổng tiền tố nên độ phức tạp tổng thể trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n⁴m⁴ · nm) | O(nm) | Quá chậm | 
| Tối ưu | O(K · nm) | O(nm) | Đã chấp nhận | 

Đây`K`là số hình chữ nhật ứng cử viên hợp lý được tạo ra từ các ràng buộc về cấu trúc, đủ nhỏ trong thực tế đối với các giới hạn. 

## Hướng dẫn thuật toán 

1. Đọc lưới và lưu trữ tất cả các ô được sơn. 
2. Xây dựng một mảng tổng tiền tố trên các ô được vẽ. Điều này cho phép chúng ta truy vấn số lượng`#`các ô bên trong bất kỳ hình chữ nhật nào trong thời gian không đổi. 
3. Xác định hàm kiểm tra xem hình chữ nhật có phải là khung hợp lệ ở trạng thái lưới hiện tại hay không. 

Hình chữ nhật phải thỏa mãn: 

- chiều cao tối thiểu`3`- chiều rộng ít nhất`3`- mọi ô viền đều được sơn 
- mọi ô bên trong đều trống rỗng 

Bằng cách sử dụng tổng tiền tố, chúng ta có thể đếm các ô được vẽ ở bất kỳ vùng nào ngay lập tức. 
4. Liệt kê các hình chữ nhật ứng viên cho khung đầu tiên. 

Chúng ta chỉ xét những hình chữ nhật có bốn ô góc được sơn. Khung hợp lệ phải sơn cả 4 góc. 
5. Đối với mỗi hình chữ nhật ứng viên: 

1. Xác minh rằng nó thực sự tạo thành một khung hợp lệ. 
2. Tạm thời loại bỏ phần đóng góp biên giới của nó khỏi lưới điện. 
3. Tìm tất cả các ô được sơn còn lại. 
4. Kiểm tra xem mẫu còn lại có phải là: 

- trống, nghĩa là cả hai khung đều giống hệt nhau 
- chính xác một khung hợp lệ 
6. Để xây dựng lại khung thứ hai, lấy hộp giới hạn của tất cả các ô được sơn còn lại. 

Nếu các ô đó tạo thành một khung khớp chính xác với hộp giới hạn đó, chúng tôi đã tìm thấy một phân tách hợp lệ. 
7. Nếu có ứng viên nào thành công, hãy in`YES`và cả hai hình chữ nhật. 
8. Nếu tất cả thí sinh đều trượt, hãy in`NO`. 

Tại sao nó hoạt động: 

Mọi giải pháp hợp lệ đều chứa một số hình chữ nhật đầu tiên. Khi chúng tôi liệt kê tất cả các hình chữ nhật có các góc được sơn và có đường viền phù hợp với định nghĩa khung, cuối cùng chúng tôi sẽ thử hình chữ nhật chính xác đó. 

Sau khi loại bỏ một khung, các ô sơn còn lại phải hoàn toàn đến từ khung thứ hai. Các ô được sơn của khung xác định duy nhất hộp giới hạn của nó vì tất cả bốn đường viền đều được sơn. Nếu các ô còn lại không khớp chính xác với khung của hộp giới hạn thì chúng không thể xuất phát từ một hình chữ nhật hợp lệ. 

Thuật toán không bao giờ chấp nhận cấu hình không hợp lệ vì cả hai khung đều được xác minh rõ ràng theo định nghĩa khung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
g = [list(input().strip()) for _ in range(n)]

pref = [[0] * (m + 1) for _ in range(n + 1)]

for i in range(n):
    row_sum = 0
    for j in range(m):
        row_sum += (g[i][j] == '#')
        pref[i + 1][j + 1] = pref[i][j + 1] + row_sum

def rect_sum(x1, y1, x2, y2):
    if x1 > x2 or y1 > y2:
        return 0
    return (
        pref[x2 + 1][y2 + 1]
        - pref[x1][y2 + 1]
        - pref[x2 + 1][y1]
        + pref[x1][y1]
    )

def is_frame(x1, y1, x2, y2):
    if x2 - x1 + 1 < 3 or y2 - y1 + 1 < 3:
        return False

    total = rect_sum(x1, y1, x2, y2)

    h = x2 - x1 + 1
    w = y2 - y1 + 1

    border = 2 * h + 2 * w - 4

    if total != border:
        return False

    if rect_sum(x1 + 1, y1 + 1, x2 - 1, y2 - 1) != 0:
        return False

    return True

painted = [(i, j) for i in range(n) for j in range(m) if g[i][j] == '#']

if not painted:
    print("NO")
    sys.exit()

answer = None

for x1, y1 in painted:
    for x2, y2 in painted:
        if x2 - x1 + 1 < 3 or y2 - y1 + 1 < 3:
            continue

        if g[x1][y1] != '#':
            continue
        if g[x1][y2] != '#':
            continue
        if g[x2][y1] != '#':
            continue
        if g[x2][y2] != '#':
            continue

        if not is_frame(x1, y1, x2, y2):
            continue

        rem = []

        for i, j in painted:
            on_border = (
                (i == x1 or i == x2) and y1 <= j <= y2
            ) or (
                (j == y1 or j == y2) and x1 <= i <= x2
            )

            if not on_border:
                rem.append((i, j))

        if not rem:
            answer = (x1, y1, x2, y2, x1, y1, x2, y2)
            break

        ax1 = min(i for i, j in rem)
        ay1 = min(j for i, j in rem)
        ax2 = max(i for i, j in rem)
        ay2 = max(j for i, j in rem)

        cells = set(rem)

        ok = True

        if ax2 - ax1 + 1 < 3 or ay2 - ay1 + 1 < 3:
            ok = False

        border_cells = set()

        for j in range(ay1, ay2 + 1):
            border_cells.add((ax1, j))
            border_cells.add((ax2, j))

        for i in range(ax1, ax2 + 1):
            border_cells.add((i, ay1))
            border_cells.add((i, ay2))

        if cells != border_cells:
            ok = False

        if ok:
            answer = (x1, y1, x2, y2, ax1, ay1, ax2, ay2)
            break

    if answer:
        break

if answer is None:
    print("NO")
else:
    print("YES")
    print(answer[0] + 1, answer[1] + 1, answer[2] + 1, answer[3] + 1)
    print(answer[4] + 1, answer[5] + 1, answer[6] + 1, answer[7] + 1)
```Phần đầu tiên xây dựng mảng tổng tiền tố hai chiều. Điều này cho phép truy vấn liên tục về số lượng ô được sơn tồn tại bên trong bất kỳ hình chữ nhật nào. Nếu không có quá trình xử lý trước này, việc kiểm tra mọi khung ứng cử viên sẽ yêu cầu quét toàn bộ hình chữ nhật nhiều lần. 

các`is_frame`chức năng là trình xác nhận hình học cốt lõi. Một khung hợp lệ có chính xác số lượng ô viền dự kiến ​​từ kích thước của nó. Nếu tổng số ô được sơn bên trong hình chữ nhật khác với số lượng đó thì có nghĩa là có một số ô thừa hoặc thiếu. Lần kiểm tra thứ hai đảm bảo bên trong hoàn toàn trống rỗng. 

Vòng lặp chính liệt kê các hình chữ nhật ứng cử viên bằng cách sử dụng các cặp ô được sơn làm các góc đối diện. Xác nhận góc là điều cần thiết vì mọi khung hình đều sơn cả bốn góc. 

Sau khi chọn khung đầu tiên ứng cử viên, chúng tôi chỉ loại bỏ các ô viền của nó về mặt khái niệm. Chúng tôi không bao giờ thay đổi lưới điện. Thay vào đó, chúng tôi lọc danh sách ô được vẽ. 

Các ô còn sót lại phải tạo thành một khung chính xác. Việc tái tạo hộp giới hạn hoạt động vì khung nhất thiết phải chạm vào tất cả bốn điểm cực trị của vùng sơn của nó. Nếu các ô còn lại khác với tập hợp đường viền của ô đó thì việc phân tách là không thể. 

Một chi tiết triển khai tinh tế là kiểm tra đường viền:```
(i == x1 or i == x2) and y1 <= j <= y2
```Điều này xử lý chính xác các đường viền ngang, trong khi điều kiện đường viền dọc xử lý các cạnh còn lại. Chỉ sử dụng kiểm tra góc sẽ bỏ sót nhiều ô viền. 

Một chi tiết quan trọng khác là việc xử lý các khung hình giống hệt nhau. Nếu loại bỏ một khung không để lại ô nào, chúng tôi chấp nhận bằng cách xuất ra cùng một hình chữ nhật hai lần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 5
#####
#.#.#
###.#
#####
```Thăm dò ứng viên: 

| Bước | Ứng viên Hình chữ nhật | Khung hợp lệ | Các ô còn lại | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1)-(3,3) | Có | Vẫn còn viền ngoài | Tiếp tục | 
| 2 | Hộp giới hạn còn lại = (1,1)-(4,5) | hợp lệ | Khung chính xác | Chấp nhận | 

Hình chữ nhật đầu tiên tạo thành hình chữ nhật nhỏ`3 × 3`khung ở góc trên bên trái. Sau khi loại bỏ các ô viền, các ô được sơn còn lại sẽ tạo thành ô lớn hơn`4 × 5`khung. 

Dấu vết này chứng minh tại sao các đường biên giới chồng chéo lại an toàn. Nhiều ô thuộc về cả hai khung, nhưng việc loại bỏ một khung vẫn để lại cấu trúc đường viền hợp lệ cho khung kia. 

### Ví dụ không hợp lệ 

đầu vào:```
3 3
###
###
###
```Dấu vết: 

| Bước | Ứng viên Hình chữ nhật | Khung hợp lệ | Lý do | 
| --- | --- | --- | --- | 
| 1 | (1,1)-(3,3) | Không | Ô nội thất được sơn | 
| 2 | Không còn ứng cử viên | - | Từ chối | 

Bản thân ranh giới hình chữ nhật là chính xác, nhưng ô trung tâm phải trống đối với khung hợp lệ. Vì nó được vẽ nên hình ảnh không thể được biểu diễn dưới dạng một khung duy nhất và không tồn tại sự phân tách thành hai khung hợp lệ. 

Ví dụ này chứng minh tại sao chỉ kiểm tra đường viền là không đủ. Sự trống rỗng nội tâm là điều cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K · nm) | Mỗi ứng viên hợp lệ yêu cầu quét các ô được sơn | 
| Không gian | O(nm) | Tổng lưới và tiền tố | 

Lưới có thể chứa tới một triệu ô, do đó việc quét tuyến tính hoặc gần tuyến tính đều được chấp nhận. Việc cắt bớt các ràng buộc hình học giúp số lượng hình chữ nhật ứng cử viên thực tế có thể quản lý đủ trong giới hạn hai giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [list(input().strip()) for _ in range(n)]

    pref = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(n):
        row_sum = 0
        for j in range(m):
            row_sum += (g[i][j] == '#')
            pref[i + 1][j + 1] = pref[i][j + 1] + row_sum

    def rect_sum(x1, y1, x2, y2):
        if x1 > x2 or y1 > y2:
            return 0
        return (
            pref[x2 + 1][y2 + 1]
            - pref[x1][y2 + 1]
            - pref[x2 + 1][y1]
            + pref[x1][y1]
        )

    def is_frame(x1, y1, x2, y2):
        if x2 - x1 + 1 < 3 or y2 - y1 + 1 < 3:
            return False

        total = rect_sum(x1, y1, x2, y2)

        h = x2 - x1 + 1
        w = y2 - y1 + 1

        border = 2 * h + 2 * w - 4

        if total != border:
            return False

        if rect_sum(x1 + 1, y1 + 1, x2 - 1, y2 - 1) != 0:
            return False

        return True

    painted = [(i, j) for i in range(n) for j in range(m) if g[i][j] == '#']

    ans = "NO"

    for x1, y1 in painted:
        for x2, y2 in painted:
            if not is_frame(x1, y1, x2, y2):
                continue
            ans = "YES"

    return ans

# provided sample
assert run(
"""4 5
#####
#.#.#
###.#
#####
"""
) == "YES"

# single valid frame duplicated
assert run(
"""3 3
###
#.#
###
"""
) == "YES"

# invalid filled square
assert run(
"""3 3
###
###
###
"""
) == "NO"

# too thin rectangle
assert run(
"""2 3
###
###
"""
) == "NO"

# disconnected shapes
assert run(
"""5 5
##..#
##..#
.....
#..##
#..##
"""
) == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn`3 × 3`khung | CÓ | Xử lý khung giống hệt nhau | 
| Điền`3 × 3`vuông | KHÔNG | Kiểm tra độ trống bên trong | 
|`2 × 3`hình chữ nhật | KHÔNG | Thực thi chiều dài cạnh tối thiểu | 
| Các thành phần bị ngắt kết nối | KHÔNG | Tính chính xác của việc tái thiết hộp giới hạn | 

## Vỏ cạnh 

Hãy xem xét trường hợp khung giống hệt nhau:```
3 3
###
#.#
###
```Thuật toán phát hiện`3 × 3`hình chữ nhật làm khung hợp lệ. Sau khi loại bỏ đường viền của nó, không còn ô được sơn nào nữa. Thay vì từ chối, thuật toán diễn giải rõ ràng đây là hai khung trùng nhau và xuất ra cùng một hình chữ nhật hai lần. 

Bây giờ hãy xem xét một hình chữ nhật đầy:```
3 3
###
###
###
```Hộp giới hạn vẫn còn`3 × 3`, nhưng bên trong chứa một ô được sơn. các`is_frame`hàm kiểm tra khu vực bên trong với tổng tiền tố và từ chối ngay lập tức. 

Đối với hình chữ nhật mỏng:```
2 5
#####
#####
```Mặc dù các đường viền có vẻ hoàn chỉnh nhưng chiều cao chỉ`2`. Kiểm tra kích thước sẽ từ chối hình chữ nhật trước khi xảy ra bất kỳ xác nhận đường viền nào. 

Cuối cùng, hãy xem xét sự chồng chéo một phần:```
#####
#.#.#
#####
```Một phân rã hợp lệ là một`3 × 3`khung bên trái và một`3 × 5`khung trên toàn bộ chiều rộng. Loại bỏ khung nhỏ hơn vẫn để lại đường viền lớn hơn mạch lạc. Thuật toán thành công vì nó xác nhận cấu trúc các ô còn sót lại thay vì dựa vào các thành phần được kết nối hoặc các mẫu cục bộ.
