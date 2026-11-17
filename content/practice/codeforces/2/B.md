---
title: "CF 2B – Đường vòng ít nhất"
description: "Chúng ta có một lưới n × n gồm các số nguyên không âm. Bắt đầu từ góc trên bên trái, chúng ta chỉ có thể di chuyển sang phải hoặc xuống cho đến khi đến góc dưới cùng bên phải. Dọc theo con đường đã chọn, chúng tôi nhân mọi giá trị đã truy cập với nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 2
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 2"
rating: 2000
weight: 2
solve_time_s: 187
verified: false
draft: false
---
## Hiểu vấn đề 

Chúng tôi có một`n × n`lưới các số nguyên không âm. Bắt đầu từ góc trên bên trái, chúng ta chỉ có thể di chuyển sang phải hoặc xuống cho đến khi đến góc dưới cùng bên phải. Dọc theo con đường đã chọn, chúng tôi nhân mọi giá trị đã truy cập với nhau. Mục tiêu là làm cho sản phẩm thu được có càng ít số 0 ở cuối càng tốt. 

Số 0 ở cuối xuất hiện bất cứ khi nào sản phẩm chứa một yếu tố`10`, và mọi`10`được hình thành từ một yếu tố`2`kết hợp với một yếu tố`5`. Điều đó có nghĩa là số số 0 ở cuối bằng:$\min(\text{count of factor }2,\ \text{count of factor }5)$Vì vậy, bản thân các giá trị thực tế trong lưới không quan trọng. Điều quan trọng là mỗi con số đóng góp bao nhiêu lần vào các yếu tố`2`Và`5`. 

Kích thước lưới có thể đạt tới`1000 × 1000`, có nghĩa là có một triệu tế bào. Bất kỳ thuật toán nào khám phá các đường dẫn một cách rõ ràng đều vô vọng vì số lượng đường dẫn hợp lệ là rất lớn. Một con đường từ`(0,0)`ĐẾN`(n-1,n-1)`chứa chính xác`2n-2`di chuyển, và chúng tôi chọn cái nào`n-1`trong số đó đang đi xuống. Tổng số đường đi là:$\binom{2n-2}{n-1}$Vì`n = 1000`, con số này lớn về mặt thiên văn. Chúng ta cần một giải pháp thời gian đa thức, lý tưởng nhất là xung quanh`O(n²)`. 

Trường hợp cạnh tinh vi nhất liên quan đến số 0 bên trong lưới. Đường đi qua số 0 tạo ra tổng sản phẩm`0`, Và`0`thường được coi là có ít nhất một số 0 ở cuối. Điều này tạo ra một tình huống bất thường trong đó một đường dẫn chứa số 0 thực sự có thể tốt hơn tất cả các đường dẫn khác 0. 

Coi như:```
2
1 10
1 0
```Bất kỳ đường đi nào tránh số 0 đều cho kết quả`10`, có một số 0 ở cuối. Đường đi qua số 0 sẽ cho ra sản phẩm`0`, cũng được coi là có một số 0 ở cuối. Câu trả lời vẫn phải là`1`. 

Bây giờ hãy xem xét:```
2
10 10
10 0
```Mọi đường đi khác 0 đều tạo ra`100`, có hai số 0 ở cuối. Đi qua số 0 chỉ có một số 0 ở cuối, vì vậy câu trả lời tối ưu sẽ trở thành`1`. 

Việc thực hiện bất cẩn thường coi số 0 là những yếu tố đóng góp vô hạn của`2`Và`5`, điều này sẽ cấm các đường dẫn đi qua số 0 một cách không chính xác. Cách xử lý đúng là phải nhớ liệu số 0 có tồn tại hay không và có khả năng xây dựng một đường dẫn đặc biệt đi qua nó. 

Một lỗi dễ mắc phải khác là việc xây dựng lại đường dẫn không chính xác khi cả hai hướng đều cho cùng một giá trị DP. Nếu quá trình chuyển đổi gốc không được lưu trữ cẩn thận, tuyến đường được in có thể không tương ứng với mức tối ưu đã tính toán. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi đường dẫn hợp lệ từ góc trên bên trái đến góc dưới bên phải, tính tích dọc theo đường dẫn đó, đếm các số 0 ở cuối và giữ lại câu trả lời đúng nhất. 

Điều này có tác dụng vì định nghĩa của vấn đề được gắn trực tiếp với một đường dẫn. Mỗi tuyến đường hợp lệ có thể được kiểm tra độc lập. Vấn đề là số lượng đường dẫn. Một đường dẫn bao gồm`n-1`chuyển động đi xuống và`n-1`các bước di chuyển sang phải được sắp xếp theo thứ tự nào đó, cho:$\binom{2n-2}{n-1}$Vì`n = 20`, con số này đã vượt quá ba mươi tỷ con đường rồi. Cách tiếp cận vũ phu trở nên không thể sử dụng được từ lâu trước giới hạn thực tế. 

Quan sát quan trọng là các số 0 ở cuối chỉ phụ thuộc vào số lượng thừa số nguyên tố`2`Và`5`. Đối với mỗi ô, chúng ta có thể tính toán trước:```
twos[i][j]  = exponent of 2 in grid[i][j]
fives[i][j] = exponent of 5 in grid[i][j]
```Nếu một con đường tích lũy`a`các yếu tố của`2`Và`b`các yếu tố của`5`, thì số chữ số 0 ở cuối bằng`min(a, b)`. 

Điều này thay đổi vấn đề hoàn toàn. Thay vì nhân những con số khổng lồ, chúng ta chỉ cần những chi phí cộng dồn dọc theo một đường đi. Lập trình động trở nên tự nhiên vì mọi di chuyển chỉ phụ thuộc vào hàng xóm trên hoặc bên trái. 

Một sự tinh tế vẫn còn. Giảm thiểu`min(a, b)`trực tiếp là khó khăn vì mức tối thiểu phụ thuộc vào hai đại lượng cùng một lúc. Bí quyết là giải quyết hai vấn đề DP riêng biệt:```
minimum total factors of 2
minimum total factors of 5
```Bất kỳ đường dẫn nào có số 0 ở cuối tối thiểu đều phải tối ưu hóa một trong các số đếm này. Nếu đường đi tối ưu có`k`số 0 ở cuối thì số lượng của nó là`2`s bằng`k`hoặc số lượng của nó`5`s bằng`k`. 

Vì vậy, chúng tôi chạy DP hai lần và nhận được kết quả tốt hơn. 

Trường hợp số 0 cần được xử lý đặc biệt. Một ô số 0 có thể cung cấp chính xác một số 0 ở cuối nếu mọi đường dẫn thông thường đều có câu trả lời lớn hơn một. Trong tình huống đó, chúng ta cố tình xây dựng một con đường đi qua số 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Ngăn xếp đệ quy theo cấp số nhân | Quá chậm | 
| Tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi ô, hãy tính xem giá trị của nó chia hết cho bao nhiêu lần`2`và bởi`5`. 

Ví dụ,`40 = 2³ × 5¹`, do đó nó đóng góp ba yếu tố`2`và một yếu tố của`5`. 
2. Nếu một ô chứa`0`, hãy nhớ riêng tọa độ của nó. 

Chúng tôi tạm thời coi số 0 là đóng góp một chi phí rất lớn trong cả hai bảng DP nên các đường dẫn DP thông thường sẽ tránh nó trừ khi cần thiết rõ ràng sau này. 
3. Xây dựng bảng DP cho các hệ số của`2`. 

Cho phép`dp2[i][j]`là tổng số yếu tố tối thiểu`2`cần thiết để tiếp cận tế bào`(i,j)`. 

Chuyển tiếp:```
dp2[i][j] = twos[i][j] + min(top, left)
```4. Lưu trữ hướng sử dụng để tiếp cận từng ô. 

Điều này cho phép xây dựng lại đường dẫn thực tế sau khi DP kết thúc. 
5. Lặp lại quy trình tương tự cho các hệ số của`5`. 

Điều này mang lại một bảng DP khác`dp5`. 
6. Hãy:```
best2 = dp2[n-1][n-1]
best5 = dp5[n-1][n-1]
```Con đường thông thường tốt nhất có câu trả lời:```
min(best2, best5)
```7. Kiểm tra xem có tồn tại ô số 0 hay không. 

Nếu câu trả lời thông thường đã có`0`, sử dụng số 0 không thể cải thiện nó. 

Nếu câu trả lời bình thường lớn hơn`1`, sau đó định tuyến qua số 0 sẽ cho câu trả lời chính xác`1`, cái nào tốt hơn. 
8. Nếu sử dụng số 0 thì tốt hơn, hãy tạo đường dẫn theo cách thủ công. 

Di chuyển xuống cho đến khi đến hàng số 0, di chuyển sang phải cho đến khi đến cột số 0, sau đó kết thúc ở góc dưới cùng bên phải. 
9. Mặt khác, hãy xây dựng lại đường dẫn DP tốt hơn bằng cách sử dụng các hướng dẫn gốc đã được lưu trữ. 

### Tại sao nó hoạt động 

Đối với bất kỳ tích số nguyên nào, các số 0 ở cuối bằng số phần tử hoàn chỉnh`(2,5)`cặp trong hệ số nguyên tố của nó. Vì mỗi đường dẫn tích lũy các yếu tố một cách độc lập giữa các ô nên tổng số`2`cát`5`s trở nên phụ gia. Lập trình động tính toán chính xác số tiền tối thiểu có thể đạt được vì mọi đường dẫn đến`(i,j)`phải đến từ một trong hai`(i-1,j)`hoặc`(i,j-1)`. 

Giả sử đường đi tối ưu có`a`các yếu tố của`2`Và`b`các yếu tố của`5`. Các số 0 ở cuối nó là`min(a,b)`. Nếu như`a ≤ b`, thì đường dẫn này là tối ưu để giảm thiểu các yếu tố của`2`. Nếu một con đường khác có ít hơn`2`s, nó cũng sẽ có ít số 0 ở cuối hơn, mâu thuẫn với tính tối ưu. Đối số đối xứng giữ cho`5`S. Vậy giải quyết hai bài toán DP độc lập là đủ. 

Việc xử lý số 0 là đúng vì bất kỳ đường đi nào qua số 0 đều tạo ra sản phẩm`0`, đóng góp chính xác một số 0 ở cuối trong bối cảnh của vấn đề này. Đường đi như vậy chỉ có ý nghĩa khi mọi đường đi thông thường đều có ít nhất hai số 0 ở cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**9

n = int(input())

twos = [[0] * n for _ in range(n)]
fives = [[0] * n for _ in range(n)]

zero_pos = None

for i in range(n):
    row = list(map(int, input().split()))
    
    for j in range(n):
        x = row[j]

        if x == 0:
            zero_pos = (i, j)
            twos[i][j] = INF
            fives[i][j] = INF
            continue

        cnt2 = 0
        while x % 2 == 0:
            cnt2 += 1
            x //= 2

        cnt5 = 0
        while x % 5 == 0:
            cnt5 += 1
            x //= 5

        twos[i][j] = cnt2
        fives[i][j] = cnt5

def build_dp(cost):
    dp = [[INF] * n for _ in range(n)]
    parent = [[''] * n for _ in range(n)]

    dp[0][0] = cost[0][0]

    for i in range(n):
        for j in range(n):
            if i == 0 and j == 0:
                continue

            if i > 0 and dp[i - 1][j] + cost[i][j] < dp[i][j]:
                dp[i][j] = dp[i - 1][j] + cost[i][j]
                parent[i][j] = 'D'

            if j > 0 and dp[i][j - 1] + cost[i][j] < dp[i][j]:
                dp[i][j] = dp[i][j - 1] + cost[i][j]
                parent[i][j] = 'R'

    return dp, parent

dp2, par2 = build_dp(twos)
dp5, par5 = build_dp(fives)

best2 = dp2[n - 1][n - 1]
best5 = dp5[n - 1][n - 1]

best = min(best2, best5)

if zero_pos is not None and best > 1:
    zi, zj = zero_pos

    path = []
    path.extend('D' * zi)
    path.extend('R' * zj)
    path.extend('D' * (n - 1 - zi))
    path.extend('R' * (n - 1 - zj))

    print(1)
    print(''.join(path))

else:
    if best2 < best5:
        parent = par2
        answer = best2
    else:
        parent = par5
        answer = best5

    path = []

    i = n - 1
    j = n - 1

    while i > 0 or j > 0:
        move = parent[i][j]
        path.append(move)

        if move == 'D':
            i -= 1
        else:
            j -= 1

    path.reverse()

    print(answer)
    print(''.join(path))
```Giai đoạn tiền xử lý chuyển đổi mọi giá trị lưới thành số lượng các yếu tố`2`Và`5`. Điều này tránh việc xử lý các tích số khổng lồ và biến phép nhân thành phép cộng đơn giản. 

Việc xử lý bằng 0 được tách biệt một cách có chủ ý khỏi DP thông thường. Phân công`INF`ngăn chặn việc vô tình sử dụng số 0 trong quá trình tối ưu hóa. Sau đó, chúng tôi quyết định rõ ràng liệu đường dẫn 0 có mang lại lợi ích hay không. 

các`build_dp`hàm tính toán các đường đi ngắn nhất trên lưới trong đó mỗi ô đóng góp một chi phí. Hướng dẫn của cha mẹ được lưu trữ trong quá trình chuyển đổi. Một được lưu trữ`'D'`có nghĩa là ô hiện tại đã được chạm tới từ phía trên, do đó quá trình tái tạo sẽ di chuyển lên trên khi lùi lại. 

Một chi tiết triển khai tinh tế là việc xây dựng lại từ đích về phía sau. Trong quá trình tái thiết:```
'D' means we came from above
'R' means we came from the left
```Vì vậy, trong khi truy tìm,`'D'`giảm chỉ số hàng và`'R'`làm giảm chỉ số cột. 

Một điểm tinh tế khác là sự so sánh:```
if zero_pos is not None and best > 1:
```Nếu mức tối ưu thông thường đã bằng`1`, sử dụng số 0 sẽ không cải thiện được gì, vì vậy cả hai câu trả lời đều được chấp nhận. Đường dẫn 0 đặc biệt chỉ quan trọng khi nó cải thiện kết quả một cách triệt để. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
4 5 6
7 8 9
```Yếu tố được tính cho`2`: 

| Tế bào | Giá trị | Đếm 2 | 
| --- | --- | --- | 
| (0,0) | 1 | 0 | 
| (0,1) | 2 | 1 | 
| (0,2) | 3 | 0 | 
| (1,0) | 4 | 2 | 
| (1,1) | 5 | 0 | 
| (1,2) | 6 | 1 | 
| (2,0) | 7 | 0 | 
| (2,1) | 8 | 3 | 
| (2,2) | 9 | 0 | 

Yếu tố được tính cho`5`: 

| Tế bào | Giá trị | Đếm 5 | 
| --- | --- | --- | 
| (0,0) | 1 | 0 | 
| (0,1) | 2 | 0 | 
| (0,2) | 3 | 0 | 
| (1,0) | 4 | 0 | 
| (1,1) | 5 | 1 | 
| (1,2) | 6 | 0 | 
| (2,0) | 7 | 0 | 
| (2,1) | 8 | 0 | 
| (2,2) | 9 | 0 | 

DP cho các yếu tố của`5`: 

| Tế bào | Chi phí tốt nhất | 
| --- | --- | 
| (0,0) | 0 | 
| (0,1) | 0 | 
| (0,2) | 0 | 
| (1,0) | 0 | 
| (1,1) | 1 | 
| (1,2) | 0 | 
| (2,0) | 0 | 
| (2,1) | 0 | 
| (2,2) | 0 | 

Đường đi tối ưu tránh được`5`hoàn toàn, không có số 0 ở cuối. Một tuyến đường hợp lệ là`DDRR`. 

Ví dụ này thể hiện ý tưởng cốt lõi rằng việc giảm thiểu các số 0 ở cuối tương đương với việc giảm thiểu tổng số`2`s hoặc tổng cộng`5`S. 

### Ví dụ 2 

đầu vào:```
2
10 10
10 0
```Đường đi thông thường tránh số 0: 

| Đường dẫn | Sản phẩm | Số 0 ở cuối | 
| --- | --- | --- | 
| RD | 100 | 2 | 
| DR | 0 | 1 | 

Các bảng DP tránh số 0 vì nó đã được gán`INF`. Câu trả lời thông thường tốt nhất trở thành`2`. 

Vì số 0 tồn tại và`2 > 1`, thuật toán sẽ tự xây dựng một đường đi qua số 0. 

| Bước | Vị trí | Di chuyển | 
| --- | --- | --- | 
| Bắt đầu | (0,0) | D | 
| Tiếp theo | (1,0) | R | 
| Kết thúc | (1,1) | - | 

Đầu ra trở thành:```
1
DR
```Ví dụ này xác nhận việc xử lý đặc biệt cho các ô bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi bảng DP xử lý mỗi ô một lần | 
| Không gian | O(n²) | Bảng DP và bảng cha lưu trữ một giá trị trên mỗi ô | 

Với`n ≤ 1000`, lưới chứa tối đa một triệu ô. Hai DP đi qua lưới dễ dàng nằm gọn trong giới hạn thời gian trong Python. Việc sử dụng bộ nhớ cũng vẫn ở mức giới hạn vì mỗi bảng chỉ lưu trữ các số nguyên hoặc hướng một ký tự. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    INF = 10**9

    n = int(input())

    twos = [[0] * n for _ in range(n)]
    fives = [[0] * n for _ in range(n)]

    zero_pos = None

    for i in range(n):
        row = list(map(int, input().split()))

        for j in range(n):
            x = row[j]

            if x == 0:
                zero_pos = (i, j)
                twos[i][j] = INF
                fives[i][j] = INF
                continue

            cnt2 = 0
            while x % 2 == 0:
                cnt2 += 1
                x //= 2

            cnt5 = 0
            while x % 5 == 0:
                cnt5 += 1
                x //= 5

            twos[i][j] = cnt2
            fives[i][j] = cnt5

    def build(cost):
        dp = [[INF] * n for _ in range(n)]
        par = [[''] * n for _ in range(n)]

        dp[0][0] = cost[0][0]

        for i in range(n):
            for j in range(n):
                if i == 0 and j == 0:
                    continue

                if i > 0 and dp[i - 1][j] + cost[i][j] < dp[i][j]:
                    dp[i][j] = dp[i - 1][j] + cost[i][j]
                    par[i][j] = 'D'

                if j > 0 and dp[i][j - 1] + cost[i][j] < dp[i][j]:
                    dp[i][j] = dp[i][j - 1] + cost[i][j]
                    par[i][j] = 'R'

        return dp, par

    dp2, par2 = build(twos)
    dp5, par5 = build(fives)

    best2 = dp2[n - 1][n - 1]
    best5 = dp5[n - 1][n - 1]

    best = min(best2, best5)

    out = []

    if zero_pos is not None and best > 1:
        zi, zj = zero_pos

        path = []
        path.extend('D' * zi)
        path.extend('R' * zj)
        path.extend('D' * (n - 1 - zi))
        path.extend('R' * (n - 1 - zj))

        out.append("1")
        out.append(''.join(path))

    else:
        if best2 < best5:
            par = par2
            ans = best2
        else:
            par = par5
            ans = best5

        path = []

        i = n - 1
        j = n - 1

        while i > 0 or j > 0:
            move = par[i][j]
            path.append(move)

            if move == 'D':
                i -= 1
            else:
                j -= 1

        path.reverse()

        out.append(str(ans))
        out.append(''.join(path))

    print('\n'.join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""3
1 2 3
4 5 6
7 8 9
"""
).startswith("0")

# minimum size
assert run(
"""2
1 1
1 1
"""
).startswith("0")

# zero gives better answer
assert run(
"""2
10 10
10 0
"""
).startswith("1")

# all equal values
assert run(
"""3
10 10 10
10 10 10
10 10 10
"""
).startswith("5")

# path with no trailing zeros exists
assert run(
"""3
2 4 8
16 32 64
3 9 27
"""
).startswith("0")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2×2`lưới của những cái |`0`số 0 ở cuối | Ranh giới kích thước tối thiểu | 
| Lưới chứa số không có lợi |`1`dấu 0 | Xử lý số 0 đúng | 
| Tất cả các giá trị bằng`10`|`5`số 0 ở cuối | Tích lũy các yếu tố | 
| Lưới có sức mạnh thuần túy của`2`ngoại trừ một hàng |`0`số 0 ở cuối | Tránh không cần thiết`5`s | 

## Vỏ cạnh 

Hãy xem xét lưới:```
2
10 10
10 0
```Mọi con đường thông thường đều tích lũy ít nhất hai yếu tố`2`và hai yếu tố`5`, cho hai số 0 ở cuối. Trong quá trình tiền xử lý, ô số 0 nhận được chi phí`INF`, nên DP tránh nó và tính toán câu trả lời`2`. 

Sau đó, thuật toán sẽ kiểm tra:```
zero exists AND best > 1
```Cả hai điều kiện đều đúng, do đó nó xây dựng một tuyến đường đi qua số 0:```
DR
```Sản phẩm trở nên`10 × 10 × 0 = 0`, mang lại chính xác một số 0 ở cuối. Câu trả lời cuối cùng là chính xác. 

Bây giờ hãy xem xét:```
2
1 1
1 0
```Một đường dẫn khác 0 đã đạt được các số 0 ở cuối. Kết quả DP trở thành`0`. 

Mặc dù tồn tại số 0 nhưng điều kiện`best > 1`thất bại. Thuật toán bỏ qua đường dẫn số 0 một cách chính xác vì việc di chuyển qua số 0 sẽ làm xấu đi câu trả lời từ`0`ĐẾN`1`. 

Cuối cùng, hãy xem xét trường hợp hòa:```
2
2 5
5 2
```Cả hai đường dẫn đều tạo ra một số 0 ở cuối. Trong quá trình tái thiết, sự lựa chọn của cha mẹ đều được chấp nhận. Thuật toán xây dựng lại một cách nhất quán một đường dẫn tối ưu hợp lệ vì mỗi quá trình chuyển đổi DP đều lưu trữ một đường dẫn trước đó thực tế được sử dụng để đạt được chi phí tối thiểu.
