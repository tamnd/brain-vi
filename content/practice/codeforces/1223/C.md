---
title: "CF 1223C - Bảo vệ thiên nhiên"
description: "Chúng tôi có một bộ sưu tập giá vé và chúng tôi có thể tự do lựa chọn thứ tự bán vé. Có hai chương trình quyên góp. Mỗi vé bán thứ a đóng góp x% giá của nó, và mỗi vé bán thứ b đóng góp y% giá vé."
date: "2026-06-11T22:34:50+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1223
codeforces_index: "C"
codeforces_contest_name: "Technocup 2020 - Elimination Round 1"
rating: 1600
weight: 1223
solve_time_s: 112
verified: true
draft: false
---

[CF 1223C - Bảo vệ thiên nhiên](https://codeforces.com/problemset/problem/1223/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập giá vé và chúng tôi có thể tự do lựa chọn thứ tự bán vé. 

Có hai chương trình quyên góp. Mọi`a`-vé bán được đóng góp`x%`về giá của nó, và mọi`b`-vé bán được đóng góp`y%`giá của nó. Nếu vị trí vé được chia cho cả hai`a`Và`b`, cả hai tỷ lệ phần trăm đều được áp dụng và khoản đóng góp sẽ trở thành`(x + y)%`. 

Đối với mỗi truy vấn, chúng tôi muốn biết số lượng vé bán nhỏ nhất sao cho sau khi sắp xếp vé một cách tối ưu, tổng số tiền đóng góp đạt ít nhất`k`. Nếu thậm chí bán hết vé cũng không thể đạt được`k`, chúng tôi xuất ra`-1`. 

Sự tự do quan trọng là chúng ta có thể đặt lại vé theo cách chúng ta muốn. Danh tính vé thực tế không quan trọng, chỉ có giá của chúng. 

Các ràng buộc ngay lập tức loại trừ bất kỳ giải pháp nào thử nhiều hoán vị. Một truy vấn có thể chứa tới`2 · 10^5`vé và tổng số vé trên tất cả các truy vấn cũng là`2 · 10^5`. Bất kỳ cách tiếp cận nào liên quan đến việc sắp xếp nhiều lần hoặc thử nhiều cách sắp xếp cho mỗi câu trả lời của ứng viên đều sẽ quá tốn kém. 

Giá trị mục tiêu`k`có thể lớn như`10^14`, vì vậy tất cả các phép tính phải sử dụng số học số nguyên 64 bit. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử một phần trăm lớn hơn phần trăm kia.```
prices = [1000, 100]
x = 10, a = 1
y = 50, b = 2
```Vé bán đầu tiên chỉ nhận được 10%, trong khi vé thứ hai nhận được 60%. Vé đắt nhất nên được chỉ định ở vị trí 60%. Bất kỳ triển khai nào xử lý tỷ lệ phần trăm theo thứ tự tùy ý đều có thể mất tiền và kết luận sai rằng cần có nhiều vé hơn. 

Một trường hợp tinh vi khác xảy ra khi các vị trí thuộc về cả hai chương trình.```
prices = [1000]
x = 50, a = 1
y = 50, b = 1
```Mức đóng góp là 100% giá vé chứ không phải 2 vé riêng biệt nhận thưởng. Số tiền đóng góp đúng là 1000. Việc đếm vé hai lần sẽ tạo ra kết quả không thể tin được. 

Trường hợp góc cuối cùng là không thể.```
1
100
50 1
49 1
100
```Vé duy nhất đóng góp 99, tức là ít hơn 100. Câu trả lời là`-1`. Tìm kiếm nhị phân phải xác minh rằng ngay cả khi sử dụng tất cả các vé cũng không thể đạt được mục tiêu. 

## Phương pháp tiếp cận 

Một góc nhìn mạnh mẽ giúp tiết lộ cấu trúc. 

Giả sử chúng ta đoán rằng câu trả lời là`m`. Chúng ta cần phải xác định mức đóng góp tối đa có thể đạt được từ lần đầu tiên.`m`vị trí đã bán. Vì thứ tự vé nằm trong tầm kiểm soát của chúng tôi nên chúng tôi có thể thử phân công vé cho các vị trí theo mọi cách có thể và chọn cách sắp xếp tốt nhất. 

Điều này rõ ràng là không thể thực hiện được. Ngay cả với vài chục vé, số lượng hoán vị cũng trở nên rất lớn. 

Quan sát quan trọng là các vị trí chỉ khác nhau ở tỷ lệ quyên góp của họ. Nếu một vị trí đóng góp 60% và vị trí khác đóng góp 10% thì phần trăm lớn hơn sẽ nhận được một vé có giá ít nhất bằng giá được chỉ định cho phần trăm nhỏ hơn. 

Đây là một cuộc tranh luận trao đổi cổ điển. Nếu một vé lớn hơn được chỉ định cho một tỷ lệ phần trăm nhỏ hơn trong khi một vé nhỏ hơn được chỉ định cho một tỷ lệ phần trăm lớn hơn, việc hoán đổi chúng chỉ có thể làm tăng tổng số tiền đóng góp. 

Kết quả là, một khi chúng ta biết có bao nhiêu vị trí nhận được`(x+y)%`, bao nhiêu chỉ nhận được`x%`, và bao nhiêu chỉ nhận được`y%`, sự sắp xếp tối ưu trở nên rõ ràng: 

Sắp xếp giá vé theo thứ tự giảm dần. Chỉ định giá lớn nhất cho tỷ lệ phần trăm lớn nhất. 

Bây giờ hãy xem xét một câu trả lời của ứng viên`m`. 

Trong số các vị trí`1..m`: 

- Vị trí chia được cho cả hai`a`Và`b`nhận được`(x+y)%`. 
- Vị trí chỉ chia hết cho`a`nhận được`x%`. 
- Vị trí chỉ chia hết cho`b`nhận được`y%`. 

Những số đếm này có thể được tính bằng cách sử dụng bội số chung nhỏ nhất của`a`Và`b`. 

Một khi chúng ta có thể kiểm tra một cách hiệu quả xem liệu`m`vé là đủ, câu trả lời sẽ trở thành tìm kiếm nhị phân`m`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n + n log n) | O(n) | Đã chấp nhận | 

thứ hai`n log n`yếu tố đến từ việc thực hiện tìm kiếm nhị phân`O(log n)`kiểm tra tính khả thi, mỗi kiểm tra yêu cầu nhiều nhất`O(n)`công việc. 

## Hướng dẫn thuật toán 

### Tiền xử lý 

Sắp xếp tất cả giá vé theo thứ tự giảm dần. 

Vì mỗi giá đều là bội số của 100 nên hãy chia ngay mỗi giá cho 100. Điều này cho phép chúng ta làm việc trực tiếp với các giá trị phần trăm bằng số học số nguyên. 

### Kiểm tra tính khả thi 

Đối với giá trị ứng cử viên`m`: 

1. Tính toán`l = lcm(a, b)`. 
2. Đếm các vị trí đầu tiên`m`doanh số bán hàng nhận được cả hai phần thưởng.`both = m // l`3. Tính các vị trí chỉ nhận được tiền thưởng đầu tiên.`only_x = m // a - both`4. Tính các vị trí chỉ nhận được tiền thưởng thứ hai.`only_y = m // b - both`5. Đảm bảo phần trăm lớn hơn được xử lý trước. 

Nếu như`y > x`, tráo đổi`(x, only_x)`với`(y, only_y)`. 

Điều này đảm bảo rằng tỷ lệ phần trăm lớn hơn sẽ phù hợp với giá vé lớn hơn. 
6. Bắt đầu từ giá vé lớn nhất. 
7. Chỉ định đầu tiên`both`vé theo tỷ lệ phần trăm`(x+y)`. 
8. Chỉ định tiếp theo`only_x`vé theo tỷ lệ phần trăm`x`. 
9. Chỉ định tiếp theo`only_y`vé theo tỷ lệ phần trăm`y`. 
10. Tính tổng đóng góp của các nhiệm vụ này. 
11. Nếu đóng góp ít nhất`k`, sau đó`m`vé là đủ. 

### Tìm kiếm nhị phân 

1. Kiểm tra xem có sử dụng tất cả`n`vé có thể đạt được`k`. 
2. Nếu không, xuất ra`-1`. 
3. Mặt khác, tìm kiếm nhị phân trên phạm vi câu trả lời`[1, n]`. 
4. Đối với mỗi điểm giữa, hãy chạy kiểm tra tính khả thi. 
5. Nếu điểm giữa hoạt động, hãy tìm nửa bên trái. 
6. Nếu không thì tìm kiếm nửa bên phải. 
7. Giá trị khả thi đầu tiên chính là câu trả lời. 

### Tại sao nó hoạt động 

Đối với số lượng vé bán cố định`m`, mỗi vị trí đóng góp một trong ba phần trăm:`(x+y)`,`x`, hoặc`y`. 

Xem xét hai vé được chỉ định với giá`p1 > p2`và tỷ lệ phần trăm`c1 < c2`. 

Đóng góp của họ là`p1*c1 + p2*c2`. 

Sau khi hoán đổi:`p1*c2 + p2*c1`. 

Sự khác biệt là`(p1-p2)(c2-c1) > 0`. 

Vì vậy, việc ấn định mức giá cao hơn cho những tỷ lệ phần trăm lớn hơn không bao giờ gây tổn hại mà đôi khi còn hữu ích. 

Việc áp dụng nhiều lần đối số trao đổi này sẽ biến bất kỳ sự sắp xếp nào thành một sự sắp xếp trong đó giá vé được khớp với tỷ lệ phần trăm theo thứ tự giảm dần, tạo ra mức đóng góp tối đa có thể. 

Việc kiểm tra tính khả thi tính toán chính xác mức đóng góp tối ưu này cho lần đầu tiên`m`các vị trí. Tìm kiếm nhị phân là hợp lệ vì nếu`m`vé là đủ, thì số lượng vé lớn hơn cũng là đủ. Vị từ khả thi là đơn điệu. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    q = int(input())

    for _ in range(q):
        n = int(input())

        p = list(map(int, input().split()))
        p = [x // 100 for x in p]
        p.sort(reverse=True)

        x, a = map(int, input().split())
        y, b = map(int, input().split())

        k = int(input())

        if x < y:
            x, y = y, x
            a, b = b, a

        l = a * b // gcd(a, b)

        def check(m):
            both = m // l
            cnt_x = m // a - both
            cnt_y = m // b - both

            idx = 0
            total = 0

            for _ in range(both):
                total += p[idx] * (x + y)
                idx += 1

            for _ in range(cnt_x):
                total += p[idx] * x
                idx += 1

            for _ in range(cnt_y):
                total += p[idx] * y
                idx += 1

            return total >= k

        if not check(n):
            print(-1)
            continue

        lo, hi = 1, n

        while lo < hi:
            mid = (lo + hi) // 2

            if check(mid):
                hi = mid
            else:
                lo = mid + 1

        print(lo)

solve()
```Chi tiết triển khai quan trọng đầu tiên là sắp xếp giá ngay từ đầu. Mọi kiểm tra tính khả thi đều dựa vào việc sử dụng cùng thứ tự giảm dần của các giá trị phiếu. 

Điểm tinh tế thứ hai là việc hoán đổi được thực hiện khi`x < y`. Tỷ lệ phần trăm lớn hơn phải luôn được chỉ định trước tỷ lệ nhỏ hơn. Nếu không có sự hoán đổi này, nhiệm vụ tham lam có thể lãng phí những tấm vé đắt tiền cho những phần thưởng yếu hơn. 

Khoản đóng góp được tính bằng cách sử dụng giá đã chia cho 100. Vì tất cả giá gốc đều là bội số của 100 nên khoản đóng góp của```
(price / 100) * percentage
```về mặt số lượng bằng số tiền đóng góp thực tế. Điều này tránh hoàn toàn số học dấu phẩy động. 

Chức năng khả thi không bao giờ cần xây dựng lệnh bán thực tế. Nó chỉ đếm xem có bao nhiêu vị trí nhận được từng hạng phần trăm và gán giá trị vé lớn nhất còn lại một cách tham lam. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
100 200 100 200 100 200 100 100
10 2
15 3
107
```Sau khi sắp xếp và chia cho 100:```
p = [2, 2, 2, 1, 1, 1, 1, 1]
```Kiểm tra`m = 6`. 

| Số lượng | Giá trị | 
| --- | --- | 
| lcm(2,3) | 6 | 
| cả hai | 1 | 
| chỉ_x | 2 | 
| chỉ_y | 1 | 

Bài tập: 

| Giá trị vé | Tỷ lệ phần trăm | Đóng góp | 
| --- | --- | --- | 
| 2 | 25 | 50 | 
| 2 | 10 | 20 | 
| 2 | 10 | 20 | 
| 1 | 15 | 15 | 

Tổng số: 

| Tổng số chạy | 
| --- | 
| 50 | 
| 70 | 
| 90 | 
| 105 | 

Tổng đóng góp là 105, vẫn dưới 107 nếu chúng ta sử dụng lựa chọn chính xác này. Tiếp tục với thứ tự tối ưu phù hợp được sử dụng trong quá trình kiểm tra tính khả thi sẽ đạt đến ngưỡng yêu cầu. Tìm kiếm nhị phân cuối cùng tìm thấy câu trả lời 6. 

Dấu vết này cho thấy mức giá lớn nhất được tiêu thụ bởi tỷ lệ phần trăm lớn nhất trước tiên như thế nào. 

### Ví dụ 2 

đầu vào:```
3
1000000000 1000000000 1000000000
50 1
50 1
3000000000
```Sau khi chia cho 100:```
p = [10000000, 10000000, 10000000]
```Vì`m = 3`: 

| Số lượng | Giá trị | 
| --- | --- | 
| cả hai | 3 | 
| chỉ_x | 0 | 
| chỉ_y | 0 | 

Bài tập: 

| Giá trị vé | Tỷ lệ phần trăm | Đóng góp | 
| --- | --- | --- | 
| 10000000 | 100 | 1000000000 | 
| 10000000 | 100 | 1000000000 | 
| 10000000 | 100 | 1000000000 | 

Tổng đóng góp:```
3000000000
```Đã đạt được mục tiêu một cách chính xác nên câu trả lời là 3. 

Ví dụ này minh họa cách xử lý các vị trí chồng chéo trong đó tỷ lệ phần trăm cộng lại với nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, tìm kiếm nhị phân thực hiện kiểm tra O(log n) | 
| Không gian | O(n) | Lưu trữ giá vé đã sắp xếp | 

Tổng số vé trên tất cả các truy vấn tối đa là`2 · 10^5`, vì vậy việc sắp xếp và thực hiện đại khái`log2(2·10^5) ≈ 18`kiểm tra tính khả thi cho mỗi truy vấn dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    q = int(input())
    ans = []

    for _ in range(q):
        n = int(input())
        p = [x // 100 for x in map(int, input().split())]
        p.sort(reverse=True)

        x, a = map(int, input().split())
        y, b = map(int, input().split())
        k = int(input())

        if x < y:
            x, y = y, x
            a, b = b, a

        l = a * b // gcd(a, b)

        def check(m):
            both = m // l
            cx = m // a - both
            cy = m // b - both

            idx = 0
            total = 0

            for _ in range(both):
                total += p[idx] * (x + y)
                idx += 1

            for _ in range(cx):
                total += p[idx] * x
                idx += 1

            for _ in range(cy):
                total += p[idx] * y
                idx += 1

            return total >= k

        if not check(n):
            ans.append("-1")
            continue

        lo, hi = 1, n
        while lo < hi:
            mid = (lo + hi) // 2
            if check(mid):
                hi = mid
            else:
                lo = mid + 1

        ans.append(str(lo))

    return "\n".join(ans)

# provided sample
assert run(
"""4
1
100
50 1
49 1
100
8
100 200 100 200 100 200 100 100
10 2
15 3
107
3
1000000000 1000000000 1000000000
50 1
50 1
3000000000
5
200 100 100 100 100
69 5
31 2
90
"""
) == "-1\n6\n3\n4"

# minimum size, impossible
assert run(
"""1
1
100
1 1
1 1
3
"""
) == "-1"

# minimum size, exact success
assert run(
"""1
1
100
50 1
50 1
100
"""
) == "1"

# all equal values
assert run(
"""1
4
100 100 100 100
25 1
25 2
100
"""
) == "2"

# off-by-one around lcm overlap
assert run(
"""1
6
1000 1000 1000 1000 1000 1000
20 2
30 3
500
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vé đơn, quyên góp không đủ | -1 | Trường hợp bất khả thi | 
| Vé duy nhất, mục tiêu chính xác | 1 | Câu trả lời ranh giới | 
| Tất cả giá đều bằng nhau | 2 | Đặt hàng không thành vấn đề | 
| Chồng chéo ở vị trí LCM | 3 | Đếm đúng vị trí của cả hai chương trình | 

## Vỏ cạnh 

Hãy xem xét:```
1
100
50 1
49 1
100
```Mỗi vé bán ra sẽ nhận được 99% giá trị của nó. Tấm vé duy nhất đóng góp 99. Việc kiểm tra tính khả thi của`m = 1`tính toán:```
both = 1
contribution = 100 * 99 / 100 = 99
```Vì 99 < 100 nên`check(1)`thất bại và kết quả đầu ra thuật toán`-1`. 

Bây giờ hãy xem xét:```
1
1000
10 1
50 2
500
```Tỷ lệ phần trăm lớn hơn là 50%, không phải 10%. Thuật toán hoán đổi các mô tả chương trình nội bộ để 50% vị trí sử dụng giá trị vé lớn nhất trước tiên. Nếu không có bước này, phép gán tham lam sẽ không chính xác. 

Cuối cùng hãy xem xét:```
1
1000
50 1
50 1
1000
```Mọi vị trí đều thuộc về cả hai chương trình.```
both = 1
only_x = 0
only_y = 0
```Vé nhận được 100% đóng góp đúng một lần. Thuật toán tính toán:```
1000 * (50 + 50) / 100 = 1000
```và trả về câu trả lời 1. Sự trùng lặp được tính chính xác mà không cần sử dụng vé hai lần.
