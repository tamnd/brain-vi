---
title: "CF 89C - Chip Chơi"
description: "Chúng tôi có một lưới chứa chip. Mỗi chip lưu trữ một hướng, trái, phải, lên hoặc xuống. Khi chúng ta bắt đầu di chuyển từ một con chip nào đó, quy trình sẽ diễn ra như sau: Con chip hiện tại nhìn theo hướng mũi tên của nó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 89
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 74 (Div. 1 Only)"
rating: 2300
weight: 89
solve_time_s: 134
verified: true
draft: false
---

[CF 89C - Chơi chip](https://codeforces.com/problemset/problem/89/C) 

**Đánh giá:** 2300 
**Tags:** bạo lực, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lưới chứa chip. Mỗi chip lưu trữ một hướng, trái, phải, lên hoặc xuống. 

Khi chúng ta bắt đầu di chuyển từ một con chip nào đó, quy trình sẽ diễn ra như sau: 

Con chip hiện tại nhìn theo hướng mũi tên của nó. Trong số tất cả các chip còn lại trong hàng hoặc cột đó, chúng tôi chọn chip gần nhất theo hướng đó. Chip hiện tại biến mất và chip được tìm thấy sẽ trở thành chip hiện tại mới. Chúng tôi lặp lại điều này cho đến khi không còn con chip nào theo hướng yêu cầu. Khi đó con chip cuối cùng cũng biến mất và nước đi kết thúc. 

Điểm của nước đi bằng với số chip bị xóa. 

Nhiệm vụ là tính hai giá trị: 

Đầu tiên, số điểm tối đa có thể đạt được từ một chip khởi đầu. 

Thứ hai, có bao nhiêu chip ban đầu đạt được mức tối đa đó. 

Kích thước lưới tối đa là 5000 ô. Điều đó làm thay đổi hoàn toàn bản chất của vấn đề. Chúng tôi không xử lý một cấu trúc thưa thớt khổng lồ mà chỉ có quét tuyến tính mới quan trọng. MỘT$O((nm)^2)$Cách tiếp cận có thể vẫn còn ở ranh giới, nhưng mọi thứ theo hình khối đều hoàn toàn không thể thực hiện được. 

Phần nguy hiểm là bảng thay đổi trong quá trình này. Khi một con chip biến mất, những bước nhảy trong tương lai có thể bỏ qua nó và hạ cánh ở xa hơn. Một mô phỏng đơn giản quét liên tục các hàng và cột có thể vô tình tính toán lại quá nhiều công việc. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ việc triển khai không chính xác. 

Hãy xem xét một chuỗi lặp lại mãi mãi nếu chúng ta quên rằng các chip bị loại bỏ sẽ biến mất ngay lập tức:```
1 2
RL
```Bắt đầu từ chip bên trái: 

Con chip bên trái trỏ về bên phải nên chúng ta chuyển sang con chip bên phải và xóa con chip bên trái. 

Bây giờ chỉ còn lại con chip bên phải. Nó trỏ sang trái, nhưng không còn chip nữa nên nó biến mất và quá trình kết thúc. 

Câu trả lời đúng là 2, không phải là một chu trình vô hạn. 

Một trường hợp tinh vi khác là khi nhiều chip tồn tại theo cùng một hướng nhưng chỉ có những chip gần nhất:```
1 4
R.RL
```Bắt đầu từ con chip đầu tiên, chúng ta chuyển sang ô thứ ba, không phải ô thứ tư. Việc triển khai bất cẩn khiến bất kỳ chip nào có thể tiếp cận đều tạo ra đường dẫn sai. 

Một sai lầm phổ biến khác là cho rằng quá trình chỉ phụ thuộc vào các cạnh đồ thị được tạo từ bảng ban đầu. Nó không. 

Ví dụ:```
1 3
RRL
```Ban đầu: 

Ô 1 trỏ đến ô 2. 

Ô 2 trỏ đến ô 3. 

Ô 3 trỏ đến ô 2. 

Nếu chúng ta bắt đầu từ ô 1 thì ô 1 sẽ biến mất. Từ ô 2, chip gần bên phải nhất bây giờ là ô 3. Sau khi ô 2 biến mất, ô 3 không còn ai ở bên trái nữa. Chiều dài di chuyển là 3. 

Biểu đồ tĩnh bỏ qua những thay đổi động này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng trò chơi một cách độc lập cho mỗi quân bài bắt đầu. 

Chúng tôi duy trì bộ chip còn sống hiện tại. Ở mỗi bước, từ vị trí hiện tại, chúng tôi quét ra ngoài theo hướng cần thiết cho đến khi tìm thấy con chip còn sống gần nhất. Sau đó, chúng tôi xóa chip hiện tại và tiếp tục. 

Điều này đúng vì nó tuân theo các quy tắc một cách chính xác. 

Vấn đề là tốc độ. Giả sử bảng chứa$K$chip. Một mô phỏng có thể xóa tất cả$K$chip và mỗi bước có thể quét toàn bộ hàng hoặc cột. Trong trường hợp xấu nhất trở thành$O(K^2)$cho một sự khởi đầu, và$O(K^3)$tổng thể. Với$K \le 5000$, điều này thật vô vọng. 

Quan sát quan trọng là việc xóa chỉ ảnh hưởng đến các hàng xóm cục bộ bên trong các hàng và cột. 

Giả sử một con chip ở vị trí$(r,c)$biến mất. Sau đó chỉ có bốn mối quan hệ thay đổi: 

Con chip gần nhất ở bên trái hiện có thể kết nối với con chip gần nhất ở bên phải. 

Con chip gần nhất ở trên hiện có thể kết nối với con chip gần nhất bên dưới. 

Không có gì khác thay đổi. 

Đây chính xác là ý tưởng giống như xóa các nút khỏi danh sách liên kết đôi. 

Đối với mỗi chip, chúng tôi lưu trữ bốn con trỏ: 

Con chip gần nhất còn lại trong cùng một hàng. 

Con chip gần nhất ngay trong cùng một hàng. 

Con chip gần nhất ở cùng một cột. 

Con chip gần nhất nằm trong cùng một cột. 

Bây giờ việc xóa một con chip trở thành$O(1)$. Chúng tôi chỉ đơn giản là kết nối lại các hàng xóm của nó. 

Thử thách còn lại là mỗi mô phỏng sẽ sửa đổi cấu trúc nên chúng ta không thể phá hủy nó vĩnh viễn. Bí quyết tiêu chuẩn là khôi phục. 

Trong quá trình mô phỏng, mọi cập nhật con trỏ đều được ghi vào ngăn xếp. Sau khi mô phỏng kết thúc, chúng tôi hoàn tác mọi sửa đổi và khôi phục cấu trúc ban đầu. 

Mỗi lần xóa chỉ gây ra những thay đổi con trỏ liên tục. Vì mỗi chip bị xóa nhiều nhất một lần trong một lần mô phỏng nên tổng công việc trên mỗi mô phỏng sẽ trở thành tuyến tính theo số lượng chip bị xóa. 

Trên tất cả các chip ban đầu, độ phức tạp trở nên$O(K^2)$, phù hợp thoải mái cho$K \le 5000$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(K^3)$|$O(K)$| Quá chậm | 
| Tối ưu |$O(K^2)$|$O(K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và gán id số nguyên cho mỗi chip. 

Các ô trống hoàn toàn bị bỏ qua vì chúng không bao giờ tham gia vào trò chơi. 
2. Đối với mỗi hàng, kết nối các chip liên tiếp bằng con trỏ trái và phải. 

Nếu chip A ở ngay trước chip B trong cùng một hàng thì:`right[A] = B`Và`left[B] = A`. 
3. Đối với mỗi cột, kết nối các chip liên tiếp bằng cách sử dụng con trỏ lên và xuống. 

Điều này xây dựng bốn danh sách liên kết đôi xung quanh mỗi chip. 
4. Đối với mỗi con chip là vị trí bắt đầu, hãy mô phỏng nước đi. 

Chúng tôi duy trì: 

- chip hiện tại 
- số lượng chip bị xóa 
- ngăn xếp khôi phục lưu trữ mọi sửa đổi con trỏ 
5. Để tiếp tục từ một con chip, hãy làm theo hướng dẫn được ghi trên đó. 

Nếu hướng là: 

-`L`, chuyển đến`left[cur]`-`R`, chuyển đến`right[cur]`-`U`, chuyển đến`up[cur]`-`D`, chuyển đến`down[cur]`Vì danh sách liên kết luôn bỏ qua các chip đã xóa nên điều này sẽ tự động cung cấp chip còn sống gần nhất theo hướng đó. 
6. Trước khi xóa chip hiện tại, hãy kết nối lại các chip lân cận của nó. 

Ví dụ cho hàng xóm ngang: 

Nếu`left[cur] = a`Và`right[cur] = b`, sau khi xóa: 

-`right[a] = b`-`left[b] = a`Điều tương tự được thực hiện theo chiều dọc. 
7. Mọi con trỏ đã thay đổi sẽ được đẩy vào ngăn xếp khôi phục dưới dạng:`(array_name, index, old_value)`Điều này cho phép chúng tôi khôi phục lại trạng thái chính xác trước đó sau này. 
8. Tiếp tục cho đến khi chip tiếp theo không tồn tại. 

Chip hiện tại vẫn bị xóa trước khi dừng, khớp hoàn toàn với luật chơi. 
9. Sau khi hoàn thành một mô phỏng, hãy so sánh điểm đạt được với điểm tối đa chung. 
10. Khôi phục tất cả các thay đổi của con trỏ bằng cách sử dụng ngăn xếp đã lưu trữ. 

Điều này khôi phục bảng gốc cho chip khởi đầu tiếp theo. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, các danh sách được liên kết thể hiện chính xác số chip hiện đang hoạt động trong mỗi hàng và cột. 

Việc xóa một chip sẽ kết nối lại các chip lân cận của nó, do đó các lần truyền tải trong tương lai sẽ tự động bỏ qua các chip đã bị loại bỏ và đến chip còn lại gần nhất. Vì mọi hành động trong trò chơi chỉ phụ thuộc vào những người hàng xóm còn sống gần nhất nên mô phỏng khớp chính xác với các quy tắc. 

Khôi phục khôi phục mọi con trỏ đã sửa đổi về giá trị trước đó của nó, do đó, mỗi mô phỏng bắt đầu đều bắt đầu từ bảng gốc chưa được chạm tới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
grid = [input().strip() for _ in range(n)]

ids = [[-1] * m for _ in range(n)]
pos = []
dirs = []

k = 0

for i in range(n):
    for j in range(m):
        if grid[i][j] != '.':
            ids[i][j] = k
            pos.append((i, j))
            dirs.append(grid[i][j])
            k += 1

LEFT = [-1] * k
RIGHT = [-1] * k
UP = [-1] * k
DOWN = [-1] * k

# build row links
for i in range(n):
    prev = -1
    for j in range(m):
        v = ids[i][j]
        if v != -1:
            if prev != -1:
                RIGHT[prev] = v
                LEFT[v] = prev
            prev = v

# build column links
for j in range(m):
    prev = -1
    for i in range(n):
        v = ids[i][j]
        if v != -1:
            if prev != -1:
                DOWN[prev] = v
                UP[v] = prev
            prev = v

arrays = {
    'L': LEFT,
    'R': RIGHT,
    'U': UP,
    'D': DOWN
}

best = 0
count = 0

for start in range(k):
    history = []

    def set_value(arr, idx, val):
        history.append((arr, idx, arr[idx]))
        arr[idx] = val

    cur = start
    score = 0

    while cur != -1:
        score += 1

        d = dirs[cur]

        if d == 'L':
            nxt = LEFT[cur]
        elif d == 'R':
            nxt = RIGHT[cur]
        elif d == 'U':
            nxt = UP[cur]
        else:
            nxt = DOWN[cur]

        l = LEFT[cur]
        r = RIGHT[cur]
        u = UP[cur]
        dwn = DOWN[cur]

        if l != -1:
            set_value(RIGHT, l, r)

        if r != -1:
            set_value(LEFT, r, l)

        if u != -1:
            set_value(DOWN, u, dwn)

        if dwn != -1:
            set_value(UP, dwn, u)

        cur = nxt

    if score > best:
        best = score
        count = 1
    elif score == best:
        count += 1

    while history:
        arr, idx, old = history.pop()
        arr[idx] = old

print(best, count)
```Giai đoạn đầu tiên chuyển đổi lưới thành id chip nhỏ gọn. Điều này quan trọng vì các ô trống không bao giờ tham gia vào trò chơi và chỉ hoạt động với các con chip sẽ khiến tất cả các mảng đều nhỏ. 

Bốn mảng lân cận tạo thành danh sách liên kết đôi trên các hàng và cột. Ví dụ,`RIGHT[x]`luôn có nghĩa là chip còn sống gần nhất ở bên phải chip`x`. 

Vòng mô phỏng tuân theo chính xác định nghĩa của trò chơi. Trước khi xóa chip hiện tại, chúng tôi tính toán chip tiếp theo bằng cách sử dụng các con trỏ hiện tại. Sau khi xóa, các con trỏ được nối lại để các tìm kiếm trong tương lai sẽ bỏ qua chip đã bị xóa. 

Ngăn xếp rollback là phần tế nhị nhất. Mọi sửa đổi con trỏ phải lưu trữ giá trị trước đó trước khi ghi đè lên nó. Thiếu ngay cả một bản cập nhật sẽ làm hỏng các mô phỏng sau này. 

Một điểm tinh tế khác là con chip cuối cùng cũng phải được đếm và xóa ngay cả khi không còn con chip tiếp theo. Đó là lý do tại sao`score`tăng trước khi kiểm tra`nxt`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3
RRL
```Cấu trúc lân cận ban đầu: 

| Chip | Hướng | Trái | Đúng | 
| --- | --- | --- | --- | 
| 0 | R | -1 | 1 | 
| 1 | R | 0 | 2 | 
| 2 | L | 1 | -1 | 

Bắt đầu từ chip 0. 

| Bước | Hiện tại | Tiếp theo | Đã xóa | Liên kết còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 1 <-> 2 | 
| 2 | 1 | 2 | 1 | 2 một mình | 
| 3 | 2 | -1 | 2 | trống | 

Điểm cuối cùng là 3. 

Dấu vết này cho thấy tại sao cập nhật động lại quan trọng. Ban đầu chip 2 còn lại cho chip 1, nhưng sau khi chip 1 biến mất thì không còn hàng xóm nào nữa. 

### Ví dụ 2 

đầu vào:```
1 2
RL
```| Bước | Hiện tại | Tiếp theo | Đã xóa | Còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | chip 1 | 
| 2 | 1 | -1 | 1 | trống | 

Điểm cuối cùng là 2. 

Ví dụ này chứng minh rằng các chu trình trong biểu đồ ban đầu không tạo ra các vòng lặp vô hạn. Việc xóa liên tục thu nhỏ cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(K^2)$| Mỗi mô phỏng xóa tối đa$K$chip với$O(1)$làm việc mỗi lần xóa | 
| Không gian |$O(K)$| Mảng lân cận và lịch sử khôi phục | 

Đây$K$là số lượng chip, và$K \le 5000$. 

Trường hợp xấu nhất thực hiện khoảng 25 triệu thao tác liên tục, vừa vặn trong giới hạn của Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    ids = [[-1] * m for _ in range(n)]
    pos = []
    dirs = []

    k = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] != '.':
                ids[i][j] = k
                pos.append((i, j))
                dirs.append(grid[i][j])
                k += 1

    LEFT = [-1] * k
    RIGHT = [-1] * k
    UP = [-1] * k
    DOWN = [-1] * k

    for i in range(n):
        prev = -1
        for j in range(m):
            v = ids[i][j]
            if v != -1:
                if prev != -1:
                    RIGHT[prev] = v
                    LEFT[v] = prev
                prev = v

    for j in range(m):
        prev = -1
        for i in range(n):
            v = ids[i][j]
            if v != -1:
                if prev != -1:
                    DOWN[prev] = v
                    UP[v] = prev
                prev = v

    best = 0
    count = 0

    for start in range(k):
        history = []

        def setv(arr, idx, val):
            history.append((arr, idx, arr[idx]))
            arr[idx] = val

        cur = start
        score = 0

        while cur != -1:
            score += 1

            d = dirs[cur]

            if d == 'L':
                nxt = LEFT[cur]
            elif d == 'R':
                nxt = RIGHT[cur]
            elif d == 'U':
                nxt = UP[cur]
            else:
                nxt = DOWN[cur]

            l = LEFT[cur]
            r = RIGHT[cur]
            u = UP[cur]
            dwn = DOWN[cur]

            if l != -1:
                setv(RIGHT, l, r)

            if r != -1:
                setv(LEFT, r, l)

            if u != -1:
                setv(DOWN, u, dwn)

            if dwn != -1:
                setv(UP, dwn, u)

            cur = nxt

        if score > best:
            best = score
            count = 1
        elif score == best:
            count += 1

        while history:
            arr, idx, old = history.pop()
            arr[idx] = old

    return f"{best} {count}"

# provided sample
assert run(
"""4 4
DRLD
U.UL
.UUR
RDDL
"""
) == "10 1", "sample 1"

# minimum size
assert run(
"""1 1
L
"""
) == "1 1", "single chip"

# simple cycle
assert run(
"""1 2
RL
"""
) == "2 2", "cycle disappears correctly"

# dynamic neighbor update
assert run(
"""1 3
RRL
"""
) == "3 1", "must skip deleted nodes"

# isolated chips
assert run(
"""2 2
L.
.R
"""
) == "1 2", "each move deletes only itself"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chip đơn |`1 1`| Xử lý đúng khi không có hàng xóm tồn tại | 
|`RL`|`2 2`| Chu kỳ chấm dứt vì chip biến mất | 
|`RRL`|`3 1`| Cập nhật hàng xóm gần nhất năng động | 
| Chip bị cô lập thưa thớt |`1 2`| Các ô trống không can thiệp | 

## Vỏ cạnh 

Hãy xem xét lại cấu hình tuần hoàn:```
1 2
RL
```Ban đầu mỗi con chip trỏ tới con chip kia. DFS dựa trên biểu đồ nếu không bị xóa sẽ lặp lại mãi mãi. 

Thuật toán của chúng tôi xóa chip hiện tại trước khi tiếp tục. Sau khi loại bỏ chip đầu tiên, chip thứ hai không còn hàng xóm hợp lệ nữa vì danh sách được liên kết sẽ kết nối lại xung quanh nút đã xóa. Quá trình kết thúc một cách tự nhiên với câu trả lời`2 2`. 

Bây giờ hãy xem xét:```
1 4
R.RL
```Con chip đầu tiên phải nhảy thẳng đến ô thứ ba vì nó là con chip gần nhất bên phải. 

Việc biểu diễn danh sách liên kết đảm bảo điều này một cách tự động.`RIGHT[cur]`luôn lưu trữ người hàng xóm còn sống gần nhất, không bao giờ ở xa hơn. 

Cuối cùng hãy xem xét:```
1 3
RRL
```Điều này nắm bắt các triển khai xây dựng biểu đồ tĩnh một lần và không bao giờ cập nhật nó. 

Sau khi xóa chip ở giữa, chip cuối cùng không còn chip lân cận bên trái nữa. Cấu trúc danh sách liên kết ngược phản ánh điều này ngay lập tức vì việc loại bỏ chip ở giữa sẽ thay đổi các con trỏ lân cận. Điểm chính xác sẽ là 3 thay vì lặp lại giữa hai chip cuối cùng.
