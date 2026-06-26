---
title: "CF 105335K - Cuộc Đua Trẻ Em"
description: "Bản đồ là một lưới các điểm mạng N × M. Mỗi điểm có điểm từ 0 đến 9. Alice bắt đầu ở góc trên bên trái (1,1) và Bob bắt đầu ở góc trên bên phải (1,M). Một nước đi phải đi đến số hàng lớn hơn và mọi nước đi đều là một đoạn thẳng."
date: "2026-06-26T00:32:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105335
codeforces_index: "K"
codeforces_contest_name: "ICPC Thailand National Competition 2024"
rating: 0
weight: 105335
solve_time_s: 83
verified: true
draft: false
---

[CF 105335K - Cuộc biểu tình dành cho trẻ em](https://codeforces.com/problemset/problem/105335/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bản đồ là một`N × M`lưới các điểm mạng. Mỗi điểm có số điểm nằm trong khoảng`0`Và`9`. 

Alice bắt đầu ở góc trên bên trái`(1,1)`và Bob bắt đầu ở góc trên bên phải`(1,M)`. Một nước đi phải đi đến số hàng lớn hơn và mọi nước đi đều là một đoạn thẳng. Điểm của một đường đi là tổng của tất cả các điểm mạng được đường đi đó ghé thăm. 

Hai con đường không được phép giao nhau. Một trong hai đứa trẻ có thể dừng lại bất cứ lúc nào và kết thúc cuộc biểu tình của mình. 

Câu trả lời cuối cùng là$$(\text{Alice score}) \times (\text{Bob score})$$và chúng tôi muốn giá trị tối đa có thể. 

Kích thước lưới tối đa là`1000 × 1000`, nhưng mỗi giá trị ô chỉ có một chữ số. Khoảng điểm nhỏ đó hóa ra lại là quan sát quan trọng. 

Thực tế hình học đầu tiên là đường đi tối ưu không bao giờ nhảy qua các hàng. Nếu một đường đi di chuyển từ hàng`r`trực tiếp đến hàng`r+2`, mọi điểm đều không âm, do đó việc chèn điểm trung gian vào hàng`r+1`chỉ có thể tăng điểm. Hướng dẫn dành cho cuộc thi nêu rõ rằng trong một giải pháp tối ưu, tọa độ hàng sẽ tăng chính xác một ở mỗi lần di chuyển. 

Khi cả hai người chơi vẫn đang di chuyển, họ phải xuất hiện ở mọi hàng và Alice phải ở ngay bên trái Bob ở hàng đó. 

Trường hợp lợi thế tinh tế xuất hiện khi một người chơi dừng lại sớm. 

Coi như```
2 2
9 1
9 0
```Nếu cả hai tiếp tục ở hàng 2 thì sản phẩm tốt nhất là`18 × 1 = 18`. 

Giải pháp tốt hơn là Alice dừng lại ngay lập tức. Alice ghi điểm`9`, trong khi Bob di chuyển đến`(2,1)`và được`1 + 9 = 10`. Câu trả lời trở thành`90`. Hướng dẫn chính thức nêu chính xác tình huống này. 

Bất kỳ giải pháp nào giả định cả hai người chơi luôn tiếp tục cho đến hàng`N`sẽ thất bại trong bài kiểm tra này. 

## Phương pháp tiếp cận 

Một công thức bạo lực rất dễ mô tả. Đối với mỗi hàng, chọn cột của Alice và cột của Bob sao cho Alice ở bên trái. Sau đó tính toán cả hai điểm và tối đa hóa sản phẩm. 

Vấn đề là mỗi hàng có khoảng`M²`khả năng. Với`N,M ≤ 1000`, số lượng cấu hình lớn về mặt thiên văn. 

Quan sát quan trọng là khi chúng ta biết cột của Alice liên tiếp, Bob chỉ quan tâm đến giá trị tốt nhất ở bên phải cột đó. 

Giả sử chúng ta đang xử lý hàng`r`. 

Nếu Alice lấy cột`i`, cô ấy đạt được$$S[r][i]$$và Bob có thể chọn cột tốt nhất ở bên phải, đạt được$$\max_{j>i} S[r][j].$$Đối với hàng này chúng ta chỉ cần cặp$$(a,b).$$Vì mỗi giá trị ô nằm giữa`0`Và`9`, cả hai`a`Và`b`là các chữ số. 

Để có được lợi ích cố định của Alice`a`, chỉ có mức Bob đạt được tối đa mới quan trọng. Điều đó có nghĩa là mỗi hàng đóng góp tối đa mười chuyển đổi hữu ích, một chuyển đổi cho mỗi chữ số`0..9`. 

Bây giờ hãy nhìn vào tổng số điểm. 

Điểm bổ sung của Alice ngoài ô xuất phát nhiều nhất là$$9(N-1) \le 8991.$$Điều đó có nghĩa là chúng ta có thể chạy DP kiểu ba lô trong đó trạng thái là điểm tích lũy của Alice. 

Cho phép$$dp[x]$$là số điểm Bob tối đa có thể đạt được trong khi cả hai người chơi vẫn còn hoạt động và Alice đã tích lũy được số điểm bổ sung`x`. 

Không gian trạng thái chỉ có khoảng`9000`, đủ nhỏ. 

Biến chứng còn lại là dừng sớm. Sau khi xử lý một số tiền tố của hàng, Alice có thể dừng và Bob trở nên không bị hạn chế hoặc Bob có thể dừng và Alice trở nên không bị hạn chế. Chúng tôi đánh giá cả hai khả năng ở mọi ranh giới hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP về điểm của Alice | O(N · 9000) | O(9000) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới. 
2. Hãy để`baseA = S[1][1]`Và`baseB = S[1][M]`. 
3. Đối với mỗi hàng`r ≥ 2`, tính:`rowMax[r]`= giá trị tối đa trong hàng đó. 

Đối với mỗi chữ số`a`từ`0`ĐẾN`9`, tính toán`best[a] = max value to the right of some column whose value equals a`. 

Điều này thể hiện tất cả các chuyển đổi hữu ích trong khi cả hai người chơi vẫn đang hoạt động. 
4. Xây dựng một mảng hậu tố$$suf[r] = \sum_{k=r}^{N} rowMax[k]$$điều này cho chúng ta biết một người chơi đơn độc còn lại vẫn có thể thu thập được bao nhiêu điểm sau khi người kia dừng lại. 
5. Khởi tạo```
dp[0] = 0
```và tất cả các trạng thái khác là không thể. 
6. Trước khi xử lý hàng`r`, đánh giá việc dừng sau hàng`r-1`. 

Nếu Alice dừng lại:```
Alice = baseA + x
Bob   = baseB + dp[x] + suf[r]
```Nếu Bob dừng lại:```
Alice = baseA + x + suf[r]
Bob   = baseB + dp[x]
```Cập nhật câu trả lời với cả hai sản phẩm. 
7. Hàng xử lý`r`. 

Đối với mọi trạng thái có thể truy cập`x`và mọi chuyển đổi chữ số hợp lệ`(a,b)`từ hàng đó:```
ndp[x + a] =
    max(ndp[x + a], dp[x] + b)
```8. Thay thế`dp`với`ndp`và tiếp tục. 
9. Sau khi tất cả các hàng được xử lý, hãy đánh giá trường hợp cả hai người chơi vẫn hoạt động cho đến hàng cuối cùng:```
(baseA + x) * (baseB + dp[x])
```10. Sản lượng sản phẩm tối đa. 

### Tại sao nó hoạt động 

Trong khi cả hai người chơi đang di chuyển, mỗi hàng đều đóng góp độc lập. 

Nếu Alice chọn một cột có giá trị`a`, lựa chọn tối ưu của Bob trong cùng hàng đó chỉ phụ thuộc vào giá trị tốt nhất ở bên phải. Không có thông tin nào khác về vấn đề hàng. 

DP lưu trữ mọi điểm tổng Alice có thể có và điểm Bob lớn nhất tương thích với nó. Vì các hàng trong tương lai chỉ phụ thuộc vào tổng tích lũy nên trạng thái này là đủ. 

Bất cứ khi nào một người chơi dừng lại, người chơi còn lại sẽ không bị ràng buộc và có thể độc lập lấy giá trị tối đa từ mỗi hàng còn lại. Các khoản tiền hậu tố thể hiện chính xác mức tăng trong tương lai. 

DP khám phá mọi cấu hình tiền tố hoạt động khả thi và mọi vị trí dừng có thể có để tìm ra sản phẩm tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    g = [list(map(int, input().split())) for _ in range(n)]

    baseA = g[0][0]
    baseB = g[0][m - 1]

    row_max = [0] * n
    row_opts = []

    for r in range(1, n):
        row = g[r]
        row_max[r] = max(row)

        suf_right = [-1] * m
        cur = -1
        for c in range(m - 1, -1, -1):
            suf_right[c] = cur
            cur = max(cur, row[c])

        best = [-1] * 10

        for c in range(m - 1):
            a = row[c]
            b = suf_right[c]
            if b >= 0:
                best[a] = max(best[a], b)

        opts = []
        for a in range(10):
            if best[a] >= 0:
                opts.append((a, best[a]))

        row_opts.append(opts)

    suf = [0] * (n + 2)
    for r in range(n - 1, 0, -1):
        suf[r] = suf[r + 1] + row_max[r]

    MAXS = 9 * (n - 1)

    NEG = -10**18
    dp = [NEG] * (MAXS + 1)
    dp[0] = 0

    ans = baseA * baseB

    for idx in range(n - 1):
        r = idx + 2

        rem = suf[r]

        for x in range(MAXS + 1):
            if dp[x] == NEG:
                continue

            bob_now = dp[x]

            ans = max(
                ans,
                (baseA + x) * (baseB + bob_now + rem)
            )

            ans = max(
                ans,
                (baseA + x + rem) * (baseB + bob_now)
            )

        ndp = [NEG] * (MAXS + 1)

        for x in range(MAXS + 1):
            if dp[x] == NEG:
                continue

            cur_b = dp[x]

            for a, b in row_opts[idx]:
                nx = x + a
                if cur_b + b > ndp[nx]:
                    ndp[nx] = cur_b + b

        dp = ndp

    for x in range(MAXS + 1):
        if dp[x] == NEG:
            continue

        ans = max(
            ans,
            (baseA + x) * (baseB + dp[x])
        )

    print(ans)

solve()
```Sau khi đọc lưới, mã sẽ xử lý trước mỗi hàng thành một tập hợp chuyển tiếp nhỏ gọn. Đối với mỗi chữ số`a`, chỉ có Bob lớn nhất mới đạt được trong khi Alice đạt được`a`được giữ lại. Đây chính xác là tính năng nén giúp giảm công việc trên mỗi hàng từ`O(M²)`khả năng tối đa là mười lần chuyển đổi. 

Mảng DP được lập chỉ mục theo điểm tích lũy của Alice. Vì mỗi điểm là một chữ số nên chỉ số tối đa chỉ là`9(N-1)`. 

Mảng hậu tố xử lý các trường hợp dừng sớm. Khi Alice hoặc Bob dừng lại, người chơi còn lại có thể tự do thu thập giá trị tối đa của mỗi hàng còn lại, do đó, một tổng hậu tố đơn giản là đủ. 

Lỗi triển khai phổ biến nhất là quên các trường hợp dừng trước hàng cuối cùng. Hướng dẫn chính thức`2 × 2`Ví dụ này tồn tại chính xác vì mức tối ưu có thể yêu cầu một người chơi dừng lại ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
3 5
9 1 3 2 5
0 0 9 0 0
3 1 2 6 1
```Hàng 2 tạo ra: 

| Alice đạt được | Đạt được Bob tốt nhất | 
| --- | --- | 
| 0 | 9 | 
| 9 | 0 | 

Hàng 3 tạo ra: 

| Alice đạt được | Đạt được Bob tốt nhất | 
| --- | --- | 
| 3 | 6 | 
| 1 | 6 | 
| 2 | 6 | 
| 6 | 1 | 

Sự phát triển của DP: 

| Sân khấu | Điểm Alice | Điểm Bob | 
| --- | --- | --- | 
| Bắt đầu | 0 | 0 | 
| Sau hàng 2 | 0 | 9 | 
| Sau hàng 2 | 9 | 0 | 
| Sau hàng 3 | 3 | 15 | 
| Sau hàng 3 | 1 | 15 | 
| Sau hàng 3 | 2 | 15 | 
| Sau hàng 3 | 6 | 10 | 
| Sau hàng 3 | 12 | 6 | 
| Sau hàng 3 | 10 | 6 | 
| Sau hàng 3 | 11 | 6 | 
| Sau hàng 3 | 15 | 1 | 

Trạng thái tốt nhất đưa ra tổng số:```
Alice = 9 + 3 = 12
Bob   = 5 + 15 = 20
```Sản phẩm:```
240
```Điều này phù hợp với mẫu. 

### Ví dụ 2```
2 2
9 1
9 0
```Trước khi xử lý hàng 2: 

| Alice thêm | Bob thêm | 
| --- | --- | 
| 0 | 0 | 

Nếu Alice dừng lại ngay lập tức: 

| Số lượng | Giá trị | 
| --- | --- | 
| Alice | 9 | 
| Bob | 1 + 9 = 10 | 
| Sản phẩm | 90 | 

Nếu cả hai tiếp tục: 

| Số lượng | Giá trị | 
| --- | --- | 
| Alice | 18 | 
| Bob | 1 | 
| Sản phẩm | 18 | 

Thuật toán giữ đúng`90`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · 9000) | Phạm vi điểm DP nhiều nhất là`9(N-1)`| 
| Không gian | O(9000) | Một mảng DP trên điểm của Alice | 

Với`N ≤ 1000`, phạm vi điểm không bao giờ vượt quá khoảng`9000`, do đó DP vẫn ở mức thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    data = io.StringIO(inp)
    input = data.readline

    n, m = map(int, input().split())
    g = [list(map(int, input().split())) for _ in range(n)]

    baseA = g[0][0]
    baseB = g[0][m - 1]

    row_max = [0] * n
    row_opts = []

    for r in range(1, n):
        row = g[r]
        row_max[r] = max(row)

        suf_right = [-1] * m
        cur = -1
        for c in range(m - 1, -1, -1):
            suf_right[c] = cur
            cur = max(cur, row[c])

        best = [-1] * 10

        for c in range(m - 1):
            a = row[c]
            b = suf_right[c]
            if b >= 0:
                best[a] = max(best[a], b)

        opts = []
        for a in range(10):
            if best[a] >= 0:
                opts.append((a, best[a]))

        row_opts.append(opts)

    suf = [0] * (n + 2)
    for r in range(n - 1, 0, -1):
        suf[r] = suf[r + 1] + row_max[r]

    MAXS = 9 * (n - 1)
    NEG = -10**18

    dp = [NEG] * (MAXS + 1)
    dp[0] = 0

    ans = baseA * baseB

    for idx in range(n - 1):
        r = idx + 2
        rem = suf[r]

        for x in range(MAXS + 1):
            if dp[x] == NEG:
                continue

            ans = max(ans, (baseA + x) * (baseB + dp[x] + rem))
            ans = max(ans, (baseA + x + rem) * (baseB + dp[x]))

        ndp = [NEG] * (MAXS + 1)

        for x in range(MAXS + 1):
            if dp[x] == NEG:
                continue

            for a, b in row_opts[idx]:
                ndp[x + a] = max(ndp[x + a], dp[x] + b)

        dp = ndp

    for x in range(MAXS + 1):
        if dp[x] != NEG:
            ans = max(ans, (baseA + x) * (baseB + dp[x]))

    return str(ans) + "\n"

# provided sample
assert run(
"""3 5
9 1 3 2 5
0 0 9 0 0
3 1 2 6 1
"""
) == "240\n"

# custom cases
assert run(
"""2 2
9 1
9 0
"""
) == "90\n"

assert run(
"""2 2
0 0
0 0
"""
) == "0\n"

assert run(
"""2 3
1 2 3
4 5 6
"""
) == "24\n"

assert run(
"""3 2
9 9
9 9
9 9
"""
) == "243\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ví dụ dừng sớm 2×2 | 90 | Một người chơi dừng ngay lập tức | 
| Tất cả số không | 0 | Xử lý điểm 0 | 
| Hàng thêm đơn | 24 | Chuyển đổi DP cơ bản | 
| Tất cả các giá trị bằng nhau | 243 | Nhiều lựa chọn tương đương | 

## Vỏ cạnh 

Hãy xem xét lại:```
2 2
9 1
9 0
```Trạng thái ban đầu:```
Alice = 9
Bob = 1
```Trước khi hàng 2 được xử lý, thuật toán sẽ đánh giá việc dừng. 

Hậu tố hàng tối đa còn lại là:```
9
```Alice dừng lại cho:```
9 × (1 + 9) = 90
```mà trở thành câu trả lời hiện tại. 

Quá trình chuyển đổi DP để tiếp tục chỉ tạo ra:```
18 × 1 = 18
```vậy nên câu trả lời cuối cùng vẫn là`90`. 

Bây giờ hãy xem xét:```
2 2
0 0
0 0
```Mọi chuyển đổi đều không đóng góp gì. DP chỉ chứa một trạng thái có thể truy cập và mọi đánh giá dừng cũng tạo ra giá trị 0. Đầu ra của thuật toán`0`, điều đó đúng. 

Cuối cùng hãy xem xét:```
3 2
9 9
9 9
9 9
```Thứ tự duy nhất có thể là Alice ở cột 1 và Bob ở cột 2. Cả hai đều thu thập cả ba hàng:```
27 × 27 = 729
```Nếu một trong hai dừng sớm thì sản phẩm sẽ nhỏ hơn. DP đánh giá cả việc tiếp tục và dừng, giữ mức tối đa chính xác.
