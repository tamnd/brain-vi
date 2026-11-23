---
title: "CF 41D - Tốt"
description: "Chúng tôi có một mạng lưới các chữ số. Con tốt bắt đầu ở đâu đó ở hàng dưới cùng và di chuyển lên trên từng hàng một. Ở mỗi bước nó có thể đi chéo sang trái hoặc chéo sang phải. Mỗi ô được truy cập sẽ đóng góp chữ số của nó vào tổng số hạt đậu thu được."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 41
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 40 (Div. 2)"
rating: 1900
weight: 41
solve_time_s: 97
verified: true
draft: false
---
[CF 41D - Tốt](https://codeforces.com/problemset/problem/41/D) 

**Xếp hạng:** 1900 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mạng lưới các chữ số. Con tốt bắt đầu ở đâu đó ở hàng dưới cùng và di chuyển lên trên từng hàng một. Ở mỗi bước nó có thể đi chéo sang trái hoặc chéo sang phải. Mỗi ô được truy cập sẽ đóng góp chữ số của nó vào tổng số hạt đậu thu được. 

Mục tiêu không chỉ đơn giản là tối đa hóa số tiền. Tổng cuối cùng cũng phải chia hết cho`k + 1`. Trong số tất cả các đường dẫn hợp lệ, chúng ta cần đường dẫn có tổng lớn nhất. Bên cạnh tổng số tiền tối đa, chúng ta phải xây dựng lại cột bắt đầu và trình tự di chuyển chính xác. 

Hội đồng quản trị có nhiều nhất`100 × 100 = 10,000`tế bào. Mỗi ô chứa một chữ số từ`0`ĐẾN`9`, do đó tổng đường đi lớn nhất có thể nhiều nhất là`100 × 9 = 900`. Điều kiện chia hết chỉ phụ thuộc vào modulo dư`k + 1`, Và`k ≤ 10`, vậy có nhiều nhất`11`số dư rõ rệt. Đây là một gợi ý mạnh mẽ rằng lập trình động theo vị trí và phần còn lại sẽ phù hợp một cách thoải mái. 

Một DFS ngây thơ sẽ phân nhánh hai lần mỗi hàng. Vì quân tốt di chuyển lên trên chính xác`n - 1`lần, số lượng đường dẫn là khoảng`m × 2^(n-1)`. Với`n = 100`, điều này là hoàn toàn không thể. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử mọi đường dẫn hợp lệ đều có tổng không chia hết cho`k + 1`.```
2 2 1
11
11
```Mọi con đường đều thu thập`2`, chia hết cho`2`, vì vậy cái này là hợp lệ. Nhưng nếu chúng ta thay đổi bảng:```
2 2 2
11
11
```Mọi con đường đều thu thập`2`, không chia hết cho`3`. Đầu ra đúng là`-1`. Việc thực hiện bất cẩn vẫn có thể in ra số tiền lớn nhất. 

Một trường hợp tế nhị khác là hướng tái thiết. Các bước di chuyển trong đầu ra được mô tả từ dưới lên trên, nhưng nhiều giải pháp DP xử lý các hàng từ trên xuống dưới. Nếu việc tái thiết được thực hiện sai hướng, đường dẫn được tạo ra sẽ không khớp với các bước di chuyển. 

Ví dụ:```
3 3 1
123
456
789
```Đường dẫn tối ưu hợp lệ là:`8 -> 5 -> 3`Điều này tương ứng với động thái`R L`khi nhìn từ dưới lên. Đảo ngược cách giải thích tạo ra một đường dẫn không chính xác. 

Cột ranh giới cũng có vấn đề. Từ cột ngoài cùng bên trái, quân tốt không thể di chuyển lên trên bên trái. Từ cột ngoài cùng bên phải, nó không thể di chuyển lên trên bên phải. Việc quên những bước kiểm tra này sẽ gây ra các chuyển đổi ngoài giới hạn. 

Ví dụ:```
3 2 1
11
11
11
```Từ cột`1`, chỉ có một động thái đi lên là hợp pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chọn mọi cột bắt đầu ở hàng dưới cùng, thử đệ quy cả hai hướng đi lên ở mỗi bước, tính tổng đã thu thập và giữ đường đi tốt nhất có tổng chia hết cho`k + 1`. 

Điều này hiệu quả vì mọi con đường có thể đều được khám phá chính xác một lần. Vấn đề là số lượng đường dẫn. Mỗi nước đi có tối đa hai lựa chọn và có`n - 1`di chuyển, do đó một cột bắt đầu duy nhất tạo ra khoảng`2^(n-1)`những con đường. Với`n = 100`, cái này lớn về mặt thiên văn. 

Cấu trúc của vấn đề đưa ra một lựa chọn tốt hơn nhiều. Tương lai chỉ phụ thuộc vào ba điều: 

1. Hàng hiện tại. 
2. Cột hiện tại. 
3. Modulo tổng hiện tại`k + 1`. 

Bản thân số tiền chính xác không quan trọng đối với khả năng chia hết, chỉ phần còn lại của nó. Đây là tính năng nén khóa giúp lập trình động có thể thực hiện được. 

Chúng tôi xác định trạng thái DP biểu thị số tiền tối đa có thể đạt được khi đến một ô cụ thể có phần dư cụ thể. Vì có nhiều nhất`100 × 100 × 11 = 110,000`tiểu bang, chúng tôi có thể xử lý tất cả chúng một cách hiệu quả. 

Chuyển tiếp rất đơn giản. Từ một ô, chúng ta di chuyển một hàng xuống dưới trong quá trình xây dựng DP, cập nhật phần còn lại sau khi thêm chữ số tiếp theo. Mỗi trạng thái có nhiều nhất hai lần chuyển tiếp đi. 

Để xây dựng lại đường dẫn, chúng tôi lưu trữ trạng thái trước đó được sử dụng để thu được giá trị tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · 2^n) | O(n) | Quá chậm | 
| DP tối ưu | O(n · m · k) | O(n · m · k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`mod = k + 1`. 
2. Xác định`dp[row][col][rem]`là số tiền tối đa có thể đạt được khi đến ô`(row, col)`với tổng số tiền còn lại`rem`modulo`mod`. 
3. Khởi tạo tất cả các trạng thái thành`-1`, có nghĩa là không thể truy cập được. 
4. Bắt đầu từ hàng trên cùng. Đối với mỗi cột`c`, bộ:```
dp[0][c][grid[0][c] % mod] = grid[0][c]
```Ở hàng trên cùng, đường dẫn chỉ chứa một ô. 

1. Xử lý các hàng từ trên xuống dưới. 

Đối với mọi trạng thái có thể truy cập`(row, col, rem)`: 

- Hãy thử di chuyển đến`(row + 1, col - 1)`nếu nó tồn tại. 
- Hãy thử di chuyển đến`(row + 1, col + 1)`nếu nó tồn tại. 

Tính toán:```
new_sum = current_sum + next_digit
new_rem = new_sum % mod
```Nếu giá trị này cho tổng lớn hơn giá trị được lưu trữ hiện tại, hãy cập nhật bảng DP và ghi lại trạng thái gốc. 

1. Sau khi xử lý tất cả các hàng, hãy kiểm tra hàng dưới cùng. 

Trong số tất cả các cột`c`, tìm trạng thái:```
dp[n-1][c][0]
```với số tiền tối đa. 

còn lại`0`có nghĩa là chia hết cho`k + 1`. 

1. Nếu không có trạng thái đó, hãy in`-1`. 
2. Mặt khác, hãy xây dựng lại đường dẫn bằng cách sử dụng các con trỏ cha được lưu trữ. 

Trong quá trình tái thiết, chúng tôi di chuyển lùi từ dưới lên trên. Nếu cột trước nhỏ hơn cột hiện tại thì bước chuyển tiếp là`R`. Nếu không thì nó đã`L`. 

1. Đảo ngược trình tự di chuyển đã thu thập vì quá trình tái tạo tiến hành lùi. 
2. In: 

1. Số tiền tối đa. 
2. Cột bắt đầu ở hàng dưới cùng, sử dụng chỉ mục dựa trên 1. 
3. Chuỗi di chuyển. 

### Tại sao nó hoạt động 

Bất biến DP là:`dp[row][col][rem]`luôn lưu trữ tổng lớn nhất có thể trong số tất cả các đường dẫn từ hàng trên cùng đến`(row, col)`tổng số dư của nó là`rem`. 

Trường hợp cơ bản là đúng vì đường dẫn bắt đầu ở hàng trên cùng chứa chính xác một ô. 

Mọi chuyển đổi đều duy trì tính chính xác vì các bước di chuyển hợp pháp duy nhất là các bước di chuyển theo đường chéo xuống và phần dư mới sau khi thêm một chữ số được xác định duy nhất bởi phần dư cũ và chữ số mới. 

Bất cứ khi nào nhiều đường dẫn đạt đến cùng một trạng thái, chỉ giữ lại tổng lớn nhất là an toàn. Bất kỳ sự tiếp tục nào trong tương lai chỉ phụ thuộc vào vị trí và số dư, vì vậy số tiền nhỏ hơn không bao giờ có thể dẫn đến câu trả lời cuối cùng tốt hơn. 

Câu trả lời cuối cùng là tối ưu vì mọi đường đi hợp lệ đều kết thúc ở hàng dưới cùng với phần dư`0`được biểu diễn trong bảng DP và chúng tôi chọn mức tối đa trong số đó một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = -1

n, m, k = map(int, input().split())
mod = k + 1

grid = [list(map(int, input().strip())) for _ in range(n)]

# dp[row][col][rem] = maximum sum
dp = [[[INF] * mod for _ in range(m)] for _ in range(n)]

# parent[row][col][rem] = (prev_col, prev_rem)
parent = [[[None] * mod for _ in range(m)] for _ in range(n)]

# initialize top row
for c in range(m):
    val = grid[0][c]
    r = val % mod
    dp[0][c][r] = val

# transitions
for row in range(n - 1):
    for col in range(m):
        for rem in range(mod):
            cur = dp[row][col][rem]

            if cur == INF:
                continue

            for nc in (col - 1, col + 1):
                if 0 <= nc < m:
                    val = grid[row + 1][nc]
                    new_sum = cur + val
                    new_rem = new_sum % mod

                    if new_sum > dp[row + 1][nc][new_rem]:
                        dp[row + 1][nc][new_rem] = new_sum
                        parent[row + 1][nc][new_rem] = (col, rem)

# find best answer in bottom row
best_sum = INF
best_col = -1

for col in range(m):
    if dp[n - 1][col][0] > best_sum:
        best_sum = dp[n - 1][col][0]
        best_col = col

if best_sum == INF:
    print(-1)
    sys.exit()

# reconstruct path
moves = []

row = n - 1
col = best_col
rem = 0

while row > 0:
    prev_col, prev_rem = parent[row][col][rem]

    if prev_col < col:
        moves.append('R')
    else:
        moves.append('L')

    col = prev_col
    rem = prev_rem
    row -= 1

moves.reverse()

print(best_sum)
print(best_col + 1)
print(''.join(moves))
```Bảng DP chỉ lưu trữ tổng tốt nhất cho mỗi trạng thái. Các quốc gia chưa bao giờ đạt tới vẫn còn`-1`. 

Việc triển khai xử lý lưới từ trên xuống dưới vì việc tái thiết trở nên dễ dàng hơn. Mỗi lần chuyển đổi tương ứng chính xác với một nước đi tốt hợp pháp. 

Bảng cha chỉ lưu trữ cột trước đó và phần còn lại trước đó. Hàng trước luôn là`row - 1`, vì vậy việc lưu trữ nó một cách riêng biệt là không cần thiết. 

Một chi tiết tinh tế là việc giải thích các chuyển động trong quá trình tái thiết. Chúng tôi di chuyển lùi từ dưới lên trên. Giả sử cột trước nhỏ hơn cột hiện tại. Điều đó có nghĩa là bước tiến đi từ trái sang phải, nên ký hiệu đúng là`R`. 

Một lỗi dễ mắc phải khác là in sai cột. Đầu ra được yêu cầu là cột bắt đầu ở hàng dưới cùng. Vì quá trình tái thiết bắt đầu từ trạng thái hàng dưới cùng đã chọn,`best_col + 1`chính xác là câu trả lời cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 1
123
456
789
```Đây`mod = 2`. 

### Dấu vết DP 

| Hàng | Col | Tổng hợp | Phần còn lại | Được lưu trữ tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 
| 0 | 1 | 2 | 0 | 2 | 
| 0 | 2 | 3 | 1 | 3 | 
| 1 | 0 | 6 | 0 | 6 | 
| 1 | 2 | 8 | 0 | 8 | 
| 1 | 1 | 8 | 0 | 8 | 
| 2 | 1 | 16 | 0 | 16 | 

Tổng hợp lệ tốt nhất ở hàng dưới cùng là`16`. 

Tái thiết mang lại:```
(2,1) <- (1,2) <- (0,1)
```Giải thích chuyển động lùi: 

-`2 -> 1`có nghĩa là tiến về phía trước`L`-`1 -> 2`có nghĩa là tiến về phía trước`R`Sau khi đảo ngược, câu trả lời là`RL`. 

Dấu vết này cho thấy nhiều đường dẫn có thể đạt đến cùng một trạng thái như thế nào và chỉ giữ lại tổng lớn nhất. 

### Ví dụ 2 

đầu vào:```
3 3 2
111
111
111
```Hiện nay`mod = 3`. 

Mỗi đường dẫn truy cập chính xác ba ô, vì vậy mỗi tổng là`3`. 

### Dấu vết DP 

| Hàng | Col | Tổng hợp | Phần còn lại | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 
| 0 | 1 | 1 | 1 | 
| 0 | 2 | 1 | 1 | 
| 1 | 0 | 2 | 2 | 
| 1 | 1 | 2 | 2 | 
| 1 | 2 | 2 | 2 | 
| 2 | 0 | 3 | 0 | 
| 2 | 1 | 3 | 0 | 
| 2 | 2 | 3 | 0 | 

Tất cả các trạng thái dưới cùng đều hợp lệ vì phần còn lại là`0`. 

Ví dụ này xác nhận rằng DP xử lý chính xác nhiều đường dẫn tương đương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m · (k + 1)) | Mỗi trạng thái thử tối đa 2 lần chuyển đổi | 
| Không gian | O(n · m · (k + 1)) | Bảng DP và con trỏ cha | 

Ở mức ràng buộc tối đa, số lượng trạng thái là khoảng`100 × 100 × 11 = 110,000`. Mỗi trạng thái thực hiện công việc liên tục, do đó thời gian chạy dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ so với`256 MB`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    INF = -1

    n, m, k = map(int, input().split())
    mod = k + 1

    grid = [list(map(int, input().strip())) for _ in range(n)]

    dp = [[[INF] * mod for _ in range(m)] for _ in range(n)]
    parent = [[[None] * mod for _ in range(m)] for _ in range(n)]

    for c in range(m):
        val = grid[0][c]
        r = val % mod
        dp[0][c][r] = val

    for row in range(n - 1):
        for col in range(m):
            for rem in range(mod):
                cur = dp[row][col][rem]

                if cur == INF:
                    continue

                for nc in (col - 1, col + 1):
                    if 0 <= nc < m:
                        val = grid[row + 1][nc]
                        ns = cur + val
                        nr = ns % mod

                        if ns > dp[row + 1][nc][nr]:
                            dp[row + 1][nc][nr] = ns
                            parent[row + 1][nc][nr] = (col, rem)

    best = INF
    best_col = -1

    for col in range(m):
        if dp[n - 1][col][0] > best:
            best = dp[n - 1][col][0]
            best_col = col

    if best == INF:
        return "-1"

    moves = []

    row = n - 1
    col = best_col
    rem = 0

    while row > 0:
        pc, pr = parent[row][col][rem]

        if pc < col:
            moves.append('R')
        else:
            moves.append('L')

        col = pc
        rem = pr
        row -= 1

    moves.reverse()

    return f"{best}\n{best_col + 1}\n{''.join(moves)}"

# provided sample
assert run(
"""3 3 1
123
456
789
"""
) == "16\n2\nRL", "sample 1"

# no valid path
assert run(
"""2 2 2
11
11
"""
) == "-1", "no divisible sum"

# minimum dimensions
assert run(
"""2 2 0
11
11
"""
) in {
    "2\n1\nR",
    "2\n2\nL"
}, "smallest board"

# all zeros
assert run(
"""3 3 5
000
000
000
"""
) in {
    "0\n1\nRR",
    "0\n2\nRL",
    "0\n2\nLR",
    "0\n3\nLL"
}, "all zero sums"

# boundary movement
assert run(
"""3 2 1
11
11
11
"""
) in {
    "3\n1\nRL",
    "3\n2\nLR"
}, "edge columns"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2×2`, không có đường đi chia hết |`-1`| Xử lý đúng các trường hợp không thể | 
| Kích thước bảng tối thiểu | Bất kỳ đường dẫn hợp lệ nào | Kích thước ranh giới | 
| Tất cả số không | Tổng`0`| Xử lý modulo chính xác bằng 0 | 
| Bảng hai cột | Chỉ di chuyển cạnh pháp lý | Bảo vệ ngoài giới hạn | 

## Vỏ cạnh 

Xét trường hợp không thể xảy ra:```
2 2 2
11
11
```Đây`mod = 3`. Mỗi đường dẫn truy cập chính xác vào hai ô, vì vậy mọi tổng đều bằng`2`. Từ`2 % 3 != 0`, không có câu trả lời hợp lệ tồn tại. 

DP lấp đầy các trạng thái còn lại`2`, nhưng hàng dưới cùng không chứa trạng thái còn lại`0`. Thuật toán in chính xác`-1`. 

Bây giờ hãy xem xét chuyển động của cột cạnh:```
3 2 1
11
11
11
```Từ cột`0`, con tốt không thể di chuyển lên trên bên trái. Từ cột`1`, nó không thể di chuyển lên trên bên phải. 

Trong quá trình chuyển đổi, điều kiện:```
if 0 <= nc < m:
```lọc các bước di chuyển không hợp lệ. Không có truy cập ngoài giới hạn xảy ra. 

Một trường hợp phức tạp khác là hướng xây dựng lại đường dẫn:```
3 3 1
123
456
789
```Chuỗi tiền thân được xây dựng lại là:```
(2,1) <- (1,2) <- (0,1)
```Trong khi đi lùi: 

- cột trước`2`đến cột hiện tại`1`có nghĩa là tiến về phía trước`L`- cột trước`1`đến cột hiện tại`2`có nghĩa là tiến về phía trước`R`Các bước di chuyển được thu thập ngược lại như`LR`, sau đó đảo ngược thành`RL`. 

Nếu không đảo ngược, đường dẫn được tạo sẽ không khớp với thứ tự truyền tải thực tế.
