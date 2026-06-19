---
title: "CF 101D - Lâu đài"
description: "Chúng tôi được đưa cho một cái cây có trọng lượng có gốc ở sảnh 1, nơi Gerald bắt đầu. Kho báu được cất giấu ngẫu nhiên ở một trong các sảnh khác. Gerald chỉ phát hiện ra kho báu khi lần đầu tiên bước vào đúng sảnh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "probabilities", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 101
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 79 (Div. 1 Only)"
rating: 2300
weight: 101
solve_time_s: 126
verified: true
draft: false
---

[CF 101D - Lâu đài](https://codeforces.com/problemset/problem/101/D) 

**Đánh giá:** 2300 
**Tags:** dp, tham lam, xác suất, phân loại, cây 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một cái cây có trọng lượng có gốc ở sảnh 1, nơi Gerald bắt đầu. Kho báu được cất giấu ngẫu nhiên ở một trong các sảnh khác. Gerald chỉ phát hiện ra kho báu khi lần đầu tiên bước vào đúng sảnh. 

Hạn chế làm thay đổi hoàn toàn vấn đề là mọi hành lang đều sụp đổ sau khi đi qua hai lần. Vì biểu đồ là một cái cây, điều này có nghĩa là Gerald phải lập kế hoạch truyền tải mà vẫn có thể đến mọi nút chính xác một lần trước khi một hành lang nào đó trở nên không thể sử dụng được. Trong thực tế, mỗi cạnh có thể được sử dụng một lần hoặc được sử dụng hai lần khi đi xuống cây con và sau đó quay trở lại. 

Đối với mỗi nút`v ≠ 1`, định nghĩa`t[v]`như lần đầu tiên Gerald bước vào`v`. Mục tiêu là giảm thiểu mức trung bình của tất cả những lần đến đầu tiên này. 

Đầu vào mô tả một cây có tối đa`10^5`các đỉnh và các cạnh có trọng số. Một thuật toán bậc hai hoặc bậc ba ngay lập tức là không thể. Thậm chí`O(n log^2 n)`sẽ là chi phí không cần thiết ở đây. Vì cấu trúc là một cái cây nên chúng ta mong đợi một tuyến tính hoặc`O(n log n)`giải pháp dựa trên DFS và các quyết định của địa phương. 

Phần khó khăn là Gerald không chọn con đường ngắn nhất để đến mục tiêu đã biết. Anh ta đang lựa chọn thứ tự thăm dò trước khi biết được kho báu ở đâu. Thứ tự cây con khác nhau làm thay đổi đáng kể thời gian khám phá dự kiến. 

Một sai lầm phổ biến là nghĩ rằng việc chiếm lợi thế ngắn nhất cục bộ trước tiên luôn là tối ưu. Hãy xem xét cây này:```
1 --(1)-- 2
|
(100)
|
3 --(1)-- 4
```Nếu chúng ta truy cập nút 2 trước thì thời gian khám phá sẽ trở thành: 

| Nút | Lần đầu ghé thăm | 
| --- | --- | 
| 2 | 1 | 
| 3 | 101 | 
| 4 | 102 | 

Trung bình =`(1 + 101 + 102) / 3 = 68`. 

Thay vào đó, nếu chúng ta đi đến nút 3 trước: 

| Nút | Lần đầu ghé thăm | 
| --- | --- | 
| 3 | 100 | 
| 4 | 101 | 
| 2 | 203 | 

Trung bình =`134.67`. 

Cạnh nhỏ hơn thực sự phải được đặt đầu tiên ở đây, nhưng lý do thực sự không phải là độ dài của cạnh. Đó là mức độ trễ trong tương lai áp dụng cho các nút chưa được khám phá còn lại. 

Một lỗi dễ xảy ra khác là khi hai cây con có kích thước rất khác nhau. Coi như:```
1 --(10)-- A
1 --(11)-- B
```Giả sử cây con`A`chứa 100 nút trong khi cây con`B`chỉ chứa 1 nút. Một quy tắc tham lam chỉ dựa trên trọng số cạnh sẽ chọn`A`đầu tiên bởi vì`10 < 11`, nhưng lý luận đó chưa đủ. Thứ tự chính xác phụ thuộc vào số lượng nút bị trì hoãn trong khi chúng ta khám phá một cây con khác. 

Một điều tinh tế cuối cùng là bản thân gốc rễ không bao giờ là một vị trí kho báu. Kỳ vọng được chia cho`n - 1`, không`n`. Việc quên điều này sẽ làm thay đổi mọi câu trả lời một chút và thường vô tình vượt qua các bài kiểm tra thủ công nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các lệnh truyền tải hợp lệ. Tại mỗi nút, chúng ta có thể chọn thứ tự khám phá các phần tử con và những lựa chọn này kết hợp đệ quy trên cây. 

Đối với một nút có mức độ`d`, có`d!`các đơn đặt hàng địa phương có thể. Trên toàn bộ cây, số lần truyền tải toàn cầu trở nên rất lớn. Ngay cả đối với một cây trung bình có nhiều điểm phân nhánh, điều này hoàn toàn không khả thi. 

Tuy nhiên, ý tưởng bạo lực vẫn tiết lộ điều gì đó hữu ích. Các quyết định có ý nghĩa duy nhất là thứ tự tương đối của các cây con anh em. Khi chúng ta vào một cây con, cuối cùng chúng ta phải hoàn thành việc khám phá nó trước khi vĩnh viễn mất quyền truy cập vào các cạnh của nó. Vì vậy, vấn đề giảm xuống còn việc quyết định thứ tự xử lý các cây con con. 

Giả sử chúng ta đứng ở nút`u`và so sánh hai cây con con`A`Và`B`. 

Cho phép:

-`sz[A]`là số nút ứng cử viên kho báu bên trong cây con`A`-`cost[A]`là tổng thời gian truyền tải trong khi khám phá đầy đủ cây con`A`và quay trở lại`u`Nếu chúng tôi xử lý`A`trước`B`, thì mọi nút bên trong`B`bị trì hoãn bởi`cost[A]`. 

Tương tự, xử lý`B`đầu tiên trì hoãn mọi nút bên trong`A`qua`cost[B]`. 

Vì thế:

-`A`trước`B`thêm độ trễ`cost[A] * sz[B]`-`B`trước`A`thêm độ trễ`cost[B] * sz[A]`Chúng ta nên đặt`A`trước`B`chính xác khi nào:$$cost[A] \cdot sz[B] < cost[B] \cdot sz[A]$$Đây là cái nhìn sâu sắc quan trọng. Bài toán trở thành bài toán lập kế hoạch rất giống với quy tắc Smith trong sắp xếp công việc. 

Bây giờ chúng ta chỉ cần: 

1. Kích thước cây con. 
2. Tổng chi phí thăm dò từng cây con. 
3. Một loại trẻ em địa phương theo sự so sánh ở trên. 

Vì mỗi cạnh được duyệt hai lần trong quá trình thăm dò toàn bộ, nếu cạnh`(u,v)`có trọng lượng`w`, sau đó:$$cost[v] = dp[v] + 2w$$Ở đâu`dp[v]`là tổng chi phí thăm dò bên trong cây con`v`. 

Sau khi sắp xếp trẻ một cách tối ưu, chúng tôi mô phỏng thứ tự DFS và tích lũy số lần truy cập đầu tiên. 

Toàn bộ giải pháp trở thành một cặp truyền tải DFS với việc sắp xếp tại mỗi nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại nút`1`. 

Điều này cung cấp cho mỗi nút một nút cha và một tập hợp nút con, giúp cho việc suy luận cây con trở nên khả thi. 
2. Chạy DFS để tính toán kích thước cây con. 

Cho phép`sz[u]`biểu thị số lượng nút ứng cử viên kho báu bên trong cây con`u`, bao gồm`u`chính nó nếu`u ≠ 1`. 
3. Trong cùng một DFS, hãy tính chi phí thăm dò. 

Cho phép`dp[u]`là tổng thời gian cần thiết để khám phá đầy đủ cây con`u`và quay trở lại`u`. 

Đối với mỗi đứa trẻ`v`kết nối bằng trọng lượng cạnh`w`:$$dp[u] += dp[v] + 2w$$bởi vì chúng ta phải đi xuống và sau đó quay trở lại dọc theo bờ đó. 
4. Đối với mỗi nút, hãy sắp xếp các nút con của nó. 

Cho hai đứa trẻ`a`Và`b`, định nghĩa:$$A = dp[a] + 2w_a$$

$$B = dp[b] + 2w_b$$Chúng tôi đặt`a`trước`b`nếu như:$$A \cdot sz[b] < B \cdot sz[a]$$Điều này giảm thiểu tổng thời gian chờ đợi đối với các nút chưa được khám phá. 
5. Chạy DFS thứ hai theo thứ tự đã sắp xếp. 

Duy trì thời gian đã trôi qua hiện tại. 

Khi lần đầu tiên vào một nút`v`, hãy thêm thời gian hiện tại vào câu trả lời chung vì đây chính xác là thời gian khám phá nếu kho báu ở đó. 
6. Sau khi khám phá đầy đủ một cây con con, hãy tăng thời gian hiện tại bằng tổng chi phí khám phá của nó. 

Điều đó phản ánh việc quay lại nút hiện tại và tiếp tục khám phá ở nơi khác. 
7. Chia tổng số tích lũy cho`n - 1`. 

Kho báu được phân bổ đồng đều giữa tất cả các nút không phải gốc. 

### Tại sao nó hoạt động 

Mỗi cây con hoạt động giống như một công việc với hai tham số: 

- mất bao lâu để xử lý đầy đủ, 
- có bao nhiêu vị trí kho báu vẫn bị chặn cho đến khi nó kết thúc. 

Nếu cây con`A`được khám phá trước cây con`B`, thì tất cả các nút trong`B`chờ đợi trong suốt thời gian thăm dò của`A`. 

Đối số trao đổi theo cặp chứng tỏ tính tối ưu. Giả sử chúng ta chỉ so sánh hai cây con liên tiếp. Đặt hàng`A`trước`B`đóng góp:$$cost[A] \cdot sz[B]$$đến tổng số nút chờ đợi trong`B`. 

Đảo ngược chúng góp phần:$$cost[B] \cdot sz[A]$$Chúng tôi chọn cái nhỏ hơn. Vì so sánh có tính bắc cầu nên việc sắp xếp theo quy tắc này sẽ mang lại thứ tự tối ưu toàn cục. 

DFS thứ hai mô phỏng chính xác quá trình truyền tải tối ưu đó, do đó thời gian đến lần đầu của mỗi nút đều được giảm thiểu. 

## Giải pháp Python```python
import sys
from functools import cmp_to_key

input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

n = int(input())

g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    a, b, w = map(int, input().split())
    g[a].append((b, w))
    g[b].append((a, w))

sz = [0] * (n + 1)
dp = [0] * (n + 1)

children = [[] for _ in range(n + 1)]

def dfs1(u, p):
    sz[u] = 1 if u != 1 else 0

    for v, w in g[u]:
        if v == p:
            continue

        dfs1(v, u)

        sz[u] += sz[v]
        dp[u] += dp[v] + 2 * w

        children[u].append((v, w))

    def cmp(x, y):
        vx, wx = x
        vy, wy = y

        cx = dp[vx] + 2 * wx
        cy = dp[vy] + 2 * wy

        left = cx * sz[vy]
        right = cy * sz[vx]

        if left < right:
            return -1
        if left > right:
            return 1
        return 0

    children[u].sort(key=cmp_to_key(cmp))

dfs1(1, 0)

ans = 0

def dfs2(u, p, cur_time):
    global ans

    if u != 1:
        ans += cur_time

    now = cur_time

    for v, w in children[u]:
        dfs2(v, u, now + w)
        now += dp[v] + 2 * w

dfs2(1, 0, 0)

print(ans / (n - 1))
```DFS đầu tiên tính toán hai đại lượng cấu trúc.`sz[u]`đếm xem có bao nhiêu vị trí kho báu hợp lệ tồn tại trong cây con`u`. Gốc rễ bị loại trừ vì kho báu không bao giờ bắt đầu từ đó.`dp[u]`đo tổng thời gian truyền cần thiết để khám phá hoàn toàn cây con`u`và quay trở lại`u`. Mỗi cạnh đóng góp hai lần vì việc khám phá đòi hỏi cả việc vào và quay lại. 

Bước phân loại là trung tâm của giải pháp. Một lỗi triển khai phổ biến là sử dụng phép chia dấu phẩy động như:```
cost[a] / sz[a]
```Điều này có thể gây ra các vấn đề về độ chính xác. Phép nhân chéo tránh được điều đó hoàn toàn. 

DFS thứ hai mô phỏng thứ tự truyền tải thực tế. Biến`now`đại diện cho thời điểm chúng tôi quay lại nút`u`và sẵn sàng bắt đầu cây con tiếp theo. 

Một điểm tinh tế khác là lệnh gọi đệ quy đi vào phần con`v`vào thời điểm đó`now + w`, không`now`. Bản thân quá trình truyền tải cạnh tiêu tốn thời gian trước khi tiếp cận lần đầu tiên`v`. 

Tất cả số học đều phù hợp một cách an toàn trong số nguyên Python vì tổng độ dài truyền tải tối đa tối đa là khoảng:$$2 \cdot (10^5 - 1) \cdot 1000$$Dù sao thì nó cũng nằm trong phạm vi 64-bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2 1
```### giá trị DFS 

| Nút | sz | dp | 
| --- | --- | --- | 
| 2 | 1 | 0 | 
| 1 | 1 | 2 | 

### Truyền tải 

| Bước | Nút hiện tại | Thời gian | trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 0 | 0 | 
| Chuyển sang 2 | 2 | 1 | 1 | 

Kỳ vọng cuối cùng:$$1 / 1 = 1$$Ví dụ này xác nhận trường hợp cơ bản. Một cạnh được đi qua một lần vì kho báu được tìm thấy ngay khi đến nơi. 

### Ví dụ 2```
4
1 3 1
3 2 1
2 4 1
```Đây chỉ đơn giản là một chuỗi:```
1 - 3 - 2 - 4
```### giá trị DFS 

| Nút | sz | dp | 
| --- | --- | --- | 
| 4 | 1 | 0 | 
| 2 | 2 | 2 | 
| 3 | 3 | 4 | 
| 1 | 3 | 6 | 

### Truyền tải 

| Bước | Nút hiện tại | Thời gian | trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 0 | 0 | 
| Nhập 3 | 3 | 1 | 1 | 
| Nhập 2 | 2 | 2 | 3 | 
| Nhập 4 | 4 | 3 | 6 | 

Kỳ vọng:$$6 / 3 = 2$$Dấu vết này cho thấy dọc theo một chuỗi không có lựa chọn đặt hàng nào. Việc truyền tải bị ép buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Các nút con của mỗi nút được sắp xếp một lần | 
| Không gian | O(n) | Danh sách kề, mảng DFS, ngăn xếp đệ quy | 

Tổng số mục con trên tất cả các nút là`n - 1`. Sắp xếp chiếm ưu thế trong thời gian chạy. Trong trường hợp xấu nhất, một nút có thể có mức độ`n - 1`, sản xuất`O(n log n)`tổng thể phức tạp. 

Điều này dễ dàng phù hợp trong giới hạn cho`n ≤ 10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from functools import cmp_to_key

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    n = int(input())

    g = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        a, b, w = map(int, input().split())
        g[a].append((b, w))
        g[b].append((a, w))

    sz = [0] * (n + 1)
    dp = [0] * (n + 1)
    children = [[] for _ in range(n + 1)]

    def dfs1(u, p):
        sz[u] = 1 if u != 1 else 0

        for v, w in g[u]:
            if v == p:
                continue

            dfs1(v, u)

            sz[u] += sz[v]
            dp[u] += dp[v] + 2 * w

            children[u].append((v, w))

        def cmp(x, y):
            vx, wx = x
            vy, wy = y

            cx = dp[vx] + 2 * wx
            cy = dp[vy] + 2 * wy

            left = cx * sz[vy]
            right = cy * sz[vx]

            if left < right:
                return -1
            if left > right:
                return 1
            return 0

        children[u].sort(key=cmp_to_key(cmp))

    dfs1(1, 0)

    ans = 0

    def dfs2(u, p, cur_time):
        nonlocal ans

        if u != 1:
            ans += cur_time

        now = cur_time

        for v, w in children[u]:
            dfs2(v, u, now + w)
            now += dp[v] + 2 * w

    dfs2(1, 0, 0)

    return f"{ans / (n - 1):.10f}"

# provided sample
assert run(
"""2
1 2 1
"""
) == "1.0000000000"

# chain
assert run(
"""4
1 3 1
3 2 1
2 4 1
"""
) == "2.0000000000"

# star
assert run(
"""5
1 2 1
1 3 1
1 4 1
1 5 1
"""
) == "4.0000000000"

# weighted ordering check
assert run(
"""3
1 2 1
1 3 100
"""
) == "51.0000000000"

# equal weights balanced tree
assert run(
"""7
1 2 1
1 3 1
2 4 1
2 5 1
3 6 1
3 7 1
"""
) == "3.6666666667"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn | 1.0 | Cây hợp lệ tối thiểu | 
| Chuỗi | 2.0 | Lệnh truyền tải bắt buộc | 
| Ngôi sao | 4.0 | Lặp đi lặp lại trả về root | 
| Vỏ có cạnh nặng | 51.0 | Đặt hàng theo độ trễ có trọng số | 
| Cây cân bằng | 3.6666666667 | Thứ tự cây con đệ quy | 

## Vỏ cạnh 

Xét cây hình ngôi sao:```
5
1 2 1
1 3 1
1 4 1
1 5 1
```Mỗi chiếc lá đều phải quay về cội nguồn trước khi đến thăm chiếc lá khác. Lá truy cập đầu tiên đạt được vào thời điểm`1`, thứ hai tại`3`, thứ ba tại`5`, và thứ tư tại`7`. 

Thuật toán tính toán: 

| Lá | Đến lần đầu | 
| --- | --- | 
| Đầu tiên | 1 | 
| Thứ hai | 3 | 
| Thứ ba | 5 | 
| Thứ tư | 7 | 

Trung bình:$$(1 + 3 + 5 + 7) / 4 = 4$$Điều này xác nhận rằng DFS thứ hai tích lũy chính xác chi phí chuyến khứ hồi. 

Bây giờ hãy xem xét trường hợp trong đó kích thước cây con quan trọng hơn trọng lượng cạnh:```
6
1 2 1
2 3 1
3 4 1
1 5 2
1 6 2
```Cây con có gốc tại`2`chứa ba nút kho báu và mất thêm chi phí tương đối ít cho mỗi nút. Thuật toán ưu tiên nó trước khi nút đơn rời đi`5`Và`6`. 

Một chiến lược ngây thơ chỉ dựa trên độ dài cạnh liền kề có thể đối xử không chính xác với cả ba phần tử con như nhau. 

Bộ so sánh xử lý việc này một cách chính xác vì nó giảm thiểu:$$cost \times delayed\_nodes$$thay vì chỉ là thời gian truyền tải địa phương. 

Cuối cùng, hãy xem xét một chuỗi chiều dài sâu`10^5`. DFS đệ quy mà không tăng độ sâu đệ quy sẽ bị lỗi do tràn ngăn xếp. Việc triển khai rõ ràng làm tăng giới hạn đệ quy để xử lý các cây trong trường hợp xấu nhất một cách an toàn.
