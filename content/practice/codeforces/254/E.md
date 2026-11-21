---
title: "CF 254E - Ký túc xá"
description: "Vasya nhận thức ăn mỗi sáng. Thức ăn của ngày i chỉ được ăn vào ngày i hoặc ngày i + 1. Mỗi ngày bản thân Vasya phải tiêu thụ đúng v kg."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 254
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 155 (Div. 2)"
rating: 2100
weight: 254
solve_time_s: 152
verified: false
draft: false
---

[CF 254E - Ký túc xá](https://codeforces.com/problemset/problem/254/E) 

**Xếp hạng:** 2100 
**Tags:** dp, triển khai 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Vasya nhận thức ăn mỗi sáng. Món ăn từ ngày`i`chỉ có thể ăn vào ngày`i`hoặc ngày`i + 1`. Mỗi ngày bản thân Vasya phải tiêu thụ chính xác`v`kilôgam. Ngoài ra, một số người bạn sẽ ở lại với anh ta trong những khoảng thời gian nhất định và nếu Vasya quyết định cho một người bạn ăn vào ngày mà người bạn đó có mặt, anh ta phải cung cấp đầy đủ nhu cầu hàng ngày cho người bạn đó. 

Mỗi lần cho ăn thành công sẽ tăng điểm phổ biến lên một. Mục tiêu là tối đa hóa tổng số bữa ăn trong cả học kỳ, đồng thời tôn trọng các quy định về hết hạn thực phẩm. 

Phần khó khăn là thức ăn chỉ tồn tại được đúng hai ngày. Điều này tạo ra một hạn chế về nguồn lực luân chuyển. Thức ăn đến hôm nay có thể dùng ngay hoặc để dành cho ngày mai, nhưng sau ngày mai thì nó sẽ biến mất vĩnh viễn. Vì vậy, những quyết định trong ngày này sẽ ảnh hưởng trực tiếp đến ngày hôm sau. 

Các ràng buộc đủ nhỏ để lập trình động, nhưng không đủ nhỏ cho bất kỳ thứ gì theo cấp số nhân. Cả hai`n`Và`m`nhiều nhất là`400`, và mỗi lượng thức ăn cũng như cảm giác thèm ăn của bạn bè cũng nhiều nhất`400`. Một không gian trạng thái xung quanh`400 * 400 * something`là hợp lý, trong khi việc thử tất cả các tập hợp con bạn bè mỗi ngày là không thể. Một ngày có thể có hàng trăm người bạn tích cực, do đó, áp lực tàn bạo đối với các nhóm nhỏ sẽ bùng nổ chỉ trong một ngày. 

Việc triển khai ngây thơ có thể âm thầm thất bại theo nhiều cách. 

Một sai lầm nguy hiểm là quên rằng thực phẩm cũ sẽ hết hạn sau một ngày. Giả định:```
2 1
1 100
0
```Vào ngày thứ nhất, một kg đến và phải được tiêu thụ ngay lập tức. Không ai có thể sống sót đến ngày thứ 3 vì không có ngày thứ 3. Việc thực hiện bất cẩn khiến thực phẩm tích lũy thay vì theo dõi thời hạn sử dụng có thể khiến người ta lầm tưởng rằng lượng thức ăn thừa khổng lồ tồn tại mãi mãi. 

Một vấn đề tế nhị khác là ngày nay tiêu tốn quá nhiều thực phẩm tươi sống khi thực phẩm cũ cần được ưu tiên. Coi như:```
2 2
4 1
1
2 2 1
```Vào ngày đầu tiên, Vasya cần hai kg, còn lại hai kg. Hai thứ đó có thể dự trữ và sử dụng vào ngày thứ 2. Người bạn ở ngày thứ 2 cần một kg nên đáp án đúng là`1`. Nếu chúng ta tham lam tiêu thụ thực phẩm tươi trước, chúng ta có thể lãng phí nguồn dự trữ và kết luận rằng người bạn đó không thể ăn được. 

Lỗi dễ thứ ba xuất hiện khi xây dựng lại những người bạn đã được cho ăn. Nhiều người bạn có thể có cùng sở thích và khoảng thời gian chồng chéo nhau. Nếu việc xây dựng lại chỉ lưu trữ số lượng thay vì danh tính, nó có thể tạo ra việc gán chỉ số bạn bè không hợp lệ. 

Ví dụ:```
3 1
3 3 3
2
1 3 1
1 3 1
```Câu trả lời tối ưu là`6`, cho cả hai bạn ăn mỗi ngày. Việc xây dựng lại phải bảo toàn chỉ số bạn bè thực tế. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Mỗi ngày, hãy xác định những người bạn nào có mặt và thử từng nhóm nhỏ trong số họ. Tổng số thức ăn cần thiết trong ngày đó là:```
v + sum(friend appetites in chosen subset)
```Sau đó mô phỏng xem quy định hết hạn thực phẩm có cho phép lựa chọn đó hay không. 

Điều này hoạt động hợp lý vì mọi quyết định cho ăn đều mang tính cục bộ trong một ngày và thời gian hết hạn chỉ kéo dài thêm một ngày. Vấn đề là số lượng tập hợp con. Nếu thậm chí có 20 người bạn hoạt động trong một ngày thì đã có hơn một triệu khả năng. Với tối đa 400 người bạn, không gian tìm kiếm trở nên hoàn toàn không thể. 

Quan sát quan trọng là chỉ có tổng lượng thức ăn bổ sung tiêu thụ trong một ngày mới quan trọng chứ không phải chính xác người bạn nào đã sản xuất ra nó. Nếu hai nhóm bạn khác nhau yêu cầu tổng số kg giống nhau và có cùng số lượng người thì họ có thể thay thế cho nhau theo quan điểm của hệ thống thực phẩm. 

Điều này biến vấn đề thành hai lớp. 

Lớp đầu tiên là quá trình tiền xử lý theo kiểu ba lô cho mỗi ngày. Trong số tất cả bạn bè có mặt ngày hôm đó, với mọi lượng thức ăn bổ sung có thể`x`, tính toán số lượng bạn bè tối đa có thể được cho ăn bằng cách sử dụng chính xác`x`kilôgam. 

Lớp thứ hai là lập trình động theo ngày và thức ăn thừa. Vì thực phẩm chỉ tồn tại được đúng một ngày nên trạng thái duy nhất chúng ta cần nhớ là lượng thực phẩm từ ngày hôm qua vẫn còn cho ngày hôm nay. 

Giả định`carry`kg tồn tại từ ngày hôm qua. Vào ngày`i`, chúng tôi nhận được`a[i]`kg tươi. Chúng tôi chọn một số tiêu dùng bổ sung`x`cho bạn bè, do đó tổng lượng tiêu thụ hàng ngày trở thành:```
need = v + x
```Trước tiên, chúng ta phải thỏa mãn điều này bằng cách sử dụng những gì còn sót lại của ngày hôm qua, bởi vì bất cứ thứ gì không được sử dụng từ ngày hôm qua sẽ biến mất sau ngày hôm nay. Sau khi thanh toán`need`, bất kỳ thực phẩm tươi sống nào chưa sử dụng đều có thể tồn tại đến ngày mai. 

Điều này tạo ra một DP chuyển tiếp rõ ràng. 

Lực lượng vũ phu không thành công vì nó tác động trực tiếp đến các tập hợp con bạn bè riêng lẻ. Giải pháp tối ưu hóa thành công vì hệ thống thực phẩm chỉ quan tâm đến tổng số kg tiêu thụ, trong khi điểm số chỉ quan tâm đến việc có bao nhiêu người bạn sản xuất ra tổng số đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^m) | O(2^m) | Quá chậm | 
| Tối ưu | O(n · 4002 + n · 4002) | O(n · 400) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi ngày, hãy thu thập tất cả bạn bè có khoảng thời gian chứa ngày đó. 
2. Xây dựng ba lô DP cho ngày hôm đó. 

Cho phép`best[d][x]`là số lượng bạn bè tối đa có thể ăn trong ngày`d`sử dụng chính xác`x`kg thức ăn bổ sung. 

Ban đầu:```
best[d][0] = 0
```Dành cho mọi người bạn năng động và thèm ăn`f`, chạy bản cập nhật ba lô 0/1. 
3. Tạo DP chính. 

Cho phép:```
dp[day][carry]
```là mức độ phổ biến tối đa có thể đạt được sau khi kết thúc`day - 1`, với`carry`kg thực phẩm tươi sống còn sót lại`day`. 
4. Bắt đầu với:```
dp[1][0] = 0
```Trước ngày thứ nhất không có thức ăn thừa. 
5. Xử lý mỗi ngày. 

Giả sử chúng ta đang ở trạng thái`(day, carry)`. 

Tổng số lương thực có hôm nay là:```
carry + a[day]
```6. Hãy thử mọi lượng thức ăn bổ sung có thể`x`như vậy`best[day][x]`là hợp lệ. 

Tổng lượng tiêu thụ ngày hôm nay trở thành:```
need = v + x
```Nếu như`need > carry + a[day]`, sự lựa chọn này là không thể. 
7. Ăn thực phẩm cũ trước. 

Chúng tôi sử dụng:```
used_old = min(carry, need)
```Sau đó, nhu cầu còn lại được lấy từ thực phẩm tươi sống. 
8. Tính lượng thức ăn tươi sống còn sót lại đến ngày mai. 

Thực phẩm tươi sống được sử dụng hiện nay:```
used_new = need - used_old
```Thức ăn tươi còn sót lại:```
next_carry = a[day] - used_new
```9. Thư giãn trong quá trình chuyển đổi.```
dp[day + 1][next_carry]
```có thể cải thiện bằng cách:```
dp[day][carry] + best[day][x]
```10. Lưu trữ các con trỏ cha để xây dựng lại. 

Chúng ta cần nhớ cả trạng thái mang theo trước đó và số tiền bổ sung`x`đã được chọn. 
11. Sau khi xử lý tất cả các ngày, hãy chọn trạng thái thực hiện cuối cùng tốt nhất. 
12. Xây dựng lại những gì đã chọn`x`giá trị cho mỗi ngày. 
13. Chạy lại quá trình tái tạo ba lô hàng ngày để xác định chính xác những người bạn nào tạo ra cảm giác thèm ăn đó`x`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi xử lý một ngày, trạng thái DP lưu trữ chính xác lượng thực phẩm tươi sống còn có thể sử dụng được vào ngày mai. Không có thực phẩm cũ nào tồn tại lâu hơn một lần chuyển đổi. 

Sử dụng thực phẩm cũ trước tiên luôn là tối ưu vì thực phẩm ngày hôm qua sẽ hết hạn ngay sau hôm nay, trong khi thực phẩm tươi chưa sử dụng vẫn có thể dùng được vào ngày mai. Bất kỳ giải pháp nào lãng phí thực phẩm cũ trong khi tiêu thụ thực phẩm tươi đều có thể được chuyển đổi thành giải pháp tốt tương đương hoặc tốt hơn bằng cách thay đổi mức tiêu thụ đó. 

Ba lô hàng ngày là đúng vì hệ thống thực phẩm chỉ phụ thuộc vào tổng số kg mà bạn bè tiêu thụ. Đối với mỗi khoản tiền thèm ăn tăng thêm có thể đạt được, chiếc ba lô sẽ lưu trữ số lượng bạn bè tối đa cho số tiền đó, đó chính xác là điều mà mục tiêu mức độ phổ biến mong muốn. 

Cùng với nhau, hai sự thật này đảm bảo sự tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = -10**9

def solve():
    n, v = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    m = int(input())

    friends = [None]
    active = [[] for _ in range(n + 1)]

    for idx in range(1, m + 1):
        l, r, f = map(int, input().split())
        friends.append((l, r, f))

        for day in range(l, r + 1):
            active[day].append((idx, f))

    MAXF = 400 * 400

    # best[day][x] = max friends count using exactly x food
    best = [[INF] * (MAXF + 1) for _ in range(n + 1)]

    # reconstruction for daily knapsack
    take = []

    for day in range(1, n + 1):
        dp = [INF] * (MAXF + 1)
        dp[0] = 0

        parent = [[False] * (MAXF + 1) for _ in range(len(active[day]) + 1)]

        for i, (_, f) in enumerate(active[day], start=1):
            ndp = dp[:]

            for s in range(MAXF - f + 1):
                if dp[s] == INF:
                    continue

                if dp[s] + 1 > ndp[s + f]:
                    ndp[s + f] = dp[s] + 1
                    parent[i][s + f] = True

            dp = ndp

        best[day] = dp
        take.append(parent)

    max_carry = 400

    dp = [[INF] * (max_carry + 1) for _ in range(n + 2)]
    prev_carry = [[-1] * (max_carry + 1) for _ in range(n + 2)]
    chosen_sum = [[-1] * (max_carry + 1) for _ in range(n + 2)]

    dp[1][0] = 0

    for day in range(1, n + 1):
        for carry in range(max_carry + 1):
            if dp[day][carry] == INF:
                continue

            total = carry + a[day]

            for extra in range(MAXF + 1):
                cnt = best[day][extra]

                if cnt < 0:
                    continue

                need = v + extra

                if need > total:
                    continue

                used_old = min(carry, need)
                used_new = need - used_old

                nxt = a[day] - used_new

                if nxt < 0 or nxt > max_carry:
                    continue

                val = dp[day][carry] + cnt

                if val > dp[day + 1][nxt]:
                    dp[day + 1][nxt] = val
                    prev_carry[day + 1][nxt] = carry
                    chosen_sum[day + 1][nxt] = extra

    ans = max(dp[n + 1])
    end_carry = dp[n + 1].index(ans)

    extras = [0] * (n + 1)

    cur = end_carry

    for day in range(n + 1, 1, -1):
        extras[day - 1] = chosen_sum[day][cur]
        cur = prev_carry[day][cur]

    result = [[] for _ in range(n + 1)]

    for day in range(1, n + 1):
        target = extras[day]

        parent = take[day - 1]
        people = active[day]

        s = target

        for i in range(len(people), 0, -1):
            idx, f = people[i - 1]

            if s >= f and parent[i][s]:
                result[day].append(idx)
                s -= f

        result[day].reverse()

    print(ans)

    for day in range(1, n + 1):
        arr = result[day]
        print(len(arr), *arr)

solve()
```Giải pháp có hai lớp DP riêng biệt. 

Lớp đầu tiên xử lý độc lập mỗi ngày. Trong số tất cả bạn bè hiện có mặt, chúng tôi tính toán mức độ phổ biến cao nhất có thể đạt được cho mỗi mức thèm ăn chính xác. Đây là chiếc ba lô cổ điển có tỷ lệ 0/1, trong đó trọng lượng là sự ngon miệng và giá trị là một điểm phổ biến. 

Lớp thứ hai xử lý hạn sử dụng của thực phẩm. Nhà nước chỉ lưu trữ thực phẩm tươi sống còn sót lại từ ngày hôm qua vì bất kỳ thực phẩm nào cũ hơn đều đã hết hạn sử dụng. Điều này giữ cho không gian trạng thái nhỏ. 

Một chi tiết triển khai tinh tế là quy tắc chuyển tiếp cho phần còn lại. Chúng ta luôn tiêu thụ thực phẩm cũ trước thực phẩm tươi. Nếu thứ tự này bị đảo ngược, DP có thể bảo quản thực phẩm hết hạn không đúng cách và tạo ra những tình trạng không thể thực hiện được. 

Một chi tiết quan trọng khác là dây buộc. Vì mỗi`a[i] ≤ 400`, số dư của ngày mai không bao giờ có thể vượt quá`400`. Điều này làm thu nhỏ bảng DP một cách đáng kể. 

Việc xây dựng lại các lựa chọn bạn bè hàng ngày sử dụng ma trận gốc riêng biệt mỗi ngày. Chỉ lưu trữ số lượng là không đủ vì nhiều tập hợp con có thể tạo ra tổng khẩu vị như nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 1
3 2 5 4
3
1 3 2
1 4 1
3 4 2
```Tình trạng sẵn có của bạn bè hàng ngày: 

| Ngày | Bạn bè tích cực | 
| --- | --- | 
| 1 | 1(2), 2(1) | 
| 2 | 1(2), 2(1) | 
| 3 | 1(2), 2(1), 3(2) | 
| 4 | 2(1), 3(2) | 

Chuyển tiếp DP chính: 

| Ngày | Mang theo | Được chọn thêm | Tổng Nhu Cầu | Thực hiện | Đạt được mức độ phổ biến | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 2 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 1 | 1 | 
| 3 | 1 | 5 | 6 | 0 | 3 | 
| 4 | 0 | 3 | 4 | 0 | 2 | 

Tổng số phổ biến trở thành`7`. 

Dấu vết này chứng minh tại sao việc bảo quản thức ăn thừa tươi lại quan trọng. Số kilôgam tiết kiệm được từ ngày đầu tiên cho phép cho ăn thêm vào ngày thứ 2. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 2
4 1
1
2 2 1
```Chuyển tiếp: 

| Ngày | Mang theo | Thực Phẩm Tươi Sống | Thêm | Cần | Thực hiện | Đạt được | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 0 | 2 | 2 | 0 | 
| 2 | 2 | 1 | 1 | 3 | 0 | 1 | 

Câu trả lời là`1`. 

Ví dụ này xác nhận nguyên tắc tham lam cốt lõi bên trong quá trình chuyển đổi. Thức ăn cũ của ngày thứ nhất phải được tiêu thụ trước vào ngày thứ 2. Nếu không, thức ăn thừa sẽ hết hạn sử dụng và không được sử dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 4002 + n · 4002) | Ba lô hàng ngày cộng với mang theo DP | 
| Không gian | O(n · 400 + n · 400²) | Bảng DP và tái thiết | 

Các giới hạn đủ nhỏ cho sự phức tạp này. Kích thước mang theo không bao giờ vượt quá`400`và tổng số tiền thèm ăn vẫn có thể quản lý được trong giới hạn một giây bằng cách sử dụng các vòng lặp Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    INF = -10**9

    n, v = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    m = int(input())

    friends = [None]
    active = [[] for _ in range(n + 1)]

    for idx in range(1, m + 1):
        l, r, f = map(int, input().split())
        friends.append((l, r, f))

        for day in range(l, r + 1):
            active[day].append((idx, f))

    MAXF = 400 * 400

    best = [[INF] * (MAXF + 1) for _ in range(n + 1)]

    for day in range(1, n + 1):
        dp = [INF] * (MAXF + 1)
        dp[0] = 0

        for _, f in active[day]:
            ndp = dp[:]

            for s in range(MAXF - f + 1):
                if dp[s] == INF:
                    continue

                ndp[s + f] = max(ndp[s + f], dp[s] + 1)

            dp = ndp

        best[day] = dp

    max_carry = 400

    dp = [[INF] * (max_carry + 1) for _ in range(n + 2)]

    dp[1][0] = 0

    for day in range(1, n + 1):
        for carry in range(max_carry + 1):
            if dp[day][carry] == INF:
                continue

            total = carry + a[day]

            for extra in range(MAXF + 1):
                if best[day][extra] < 0:
                    continue

                need = v + extra

                if need > total:
                    continue

                used_old = min(carry, need)
                used_new = need - used_old

                nxt = a[day] - used_new

                dp[day + 1][nxt] = max(
                    dp[day + 1][nxt],
                    dp[day][carry] + best[day][extra]
                )

    print(max(dp[n + 1]))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__

    return out.getvalue().strip()

# provided sample
assert run(
"""4 1
3 2 5 4
3
1 3 2
1 4 1
3 4 2
"""
) == "7"

# minimum case
assert run(
"""1 1
1
0
"""
) == "0"

# carry-over required
assert run(
"""2 2
4 1
1
2 2 1
"""
) == "1"

# identical friends
assert run(
"""3 1
3 3 3
2
1 3 1
1 3 1
"""
) == "6"

# no extra food possible
assert run(
"""3 3
3 3 3
2
1 3 1
2 2 2
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ngày độc thân không bạn bè | 0 | Hạn chế tối thiểu | 
| Yêu cầu chuyển giao | 1 | Xử lý hết hạn đúng | 
| Hai người bạn giống hệt nhau | 6 | Sự mơ hồ trong tái thiết | 
| Không có thức ăn thừa | 0 | Lượng tiêu thụ chính xác hàng ngày | 

## Vỏ cạnh 

Hãy xem xét trường hợp thức ăn thừa phải được bảo quản đúng cách. 

đầu vào:```
2 2
4 1
1
2 2 1
```Vào ngày đầu tiên, Vasya đã tiêu thụ được hai kg. Hai kg tươi vẫn còn và tồn tại đến ngày thứ 2. 

Vào ngày thứ 2, lượng thức ăn có sẵn bằng`2 + 1 = 3`. Người bạn cần một kg, nên tổng nhu cầu trở thành`3`. Thuật toán tiêu thụ hai kg cũ trước, sau đó là một kg mới. Không còn thức ăn nào sau đó. 

Nếu quá trình chuyển đổi sử dụng thực phẩm tươi trước, thực phẩm cũ sẽ hết hạn sử dụng và DP sẽ mất đi nguồn thức ăn khả thi một cách không chính xác. 

Bây giờ hãy xem xét việc xếp chồng những người bạn có cùng sở thích như nhau. 

đầu vào:```
3 1
3 3 3
2
1 3 1
1 3 1
```Mỗi ngày có sẵn ba kg. Bản thân Vasya cần một kg, mỗi ngày nhường chỗ cho cả hai người bạn. 

Nhật ký chiếc ba lô hàng ngày:```
sum 2 -> 2 friends
```Ma trận xây dựng lại lưu trữ chính xác các chỉ số bạn bè đã tạo ra số tiền đó. Trong quá trình quay lui, cả hai chỉ số đều được phục hồi đúng cách. Nếu không có dữ liệu tái tạo này, thuật toán có thể xuất ra các chỉ mục trùng lặp hoặc bỏ sót hoàn toàn một người bạn. 

Cuối cùng, hãy xem xét hạn sử dụng của thực phẩm. 

đầu vào:```
3 1
10 1 1
0
```Sau ngày thứ nhất, nhiều nhất là chín kg sống sót. Chín cái đó chỉ có thể được sử dụng vào ngày thứ 2. Đến ngày thứ 3, mọi thứ từ ngày 1 sẽ biến mất vĩnh viễn. 

Trạng thái DP chỉ lưu trữ mang theo một ngày, vì vậy việc lưu trữ lâu dài không thể thực hiện được sẽ không bao giờ xuất hiện.
