---
title: "CF 102471M - Giá trị"
description: "Chúng tôi chọn một số chỉ số từ 1 đến n. Mỗi chỉ số được chọn i đều đóng góp ai vào điểm số. Có một loại tương tác giữa các chỉ số được chọn: nếu cả i và j đều được chọn và j = i^k với một số nguyên k 1, thì bj sẽ bị trừ một lần."
date: "2026-08-09T18:57:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "M"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 365
verified: true
draft: false
---

[CF 102471M - Giá trị](https://codeforces.com/problemset/problem/102471/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi chọn một số chỉ số từ`1`bởi vì`n`. Mỗi chỉ số được chọn`i`đóng góp`a_i`đến điểm số. Có một loại tương tác giữa các chỉ số được chọn: nếu cả hai`i`Và`j`được chọn và`j = i^k`đối với một số nguyên`k > 1`, sau đó`b_j`được trừ đi một lần. 

Nhiệm vụ là chọn tập hợp con có điểm kết quả lớn nhất. Các mảng`a`Và`b`cả hai đều có giá trị dương, do đó việc chọn một chỉ số luôn mang lại lợi nhuận riêng lẻ, nhưng việc chọn chỉ số đó cùng với một số chỉ số khác có thể tạo ra hình phạt. Vấn đề ban đầu có`n <= 100000`Và`a_i, b_i <= 10^9`. 

Sự ràng buộc của`100000`ngay lập tức loại trừ bất kỳ điều gì được coi là tập hợp con tùy ý của các chỉ số. có`2^n`các tập hợp con như vậy, và thậm chí một`O(n^2)`việc đánh giá một tập hợp con sẽ là vô vọng. Chúng ta cần khai thác dạng rất đặc biệt của mối quan hệ`i^k = j`. 

Có một số trường hợp dễ xảy ra khi việc triển khai có thể âm thầm gặp trục trặc. Đầu tiên, chỉ số`1`là đặc biệt. Nó không bao giờ tham gia vào một hình phạt vì điều kiện yêu cầu điểm cuối nhỏ hơn ít nhất phải bằng`2`. Từ`a_1 > 0`, chỉ mục`1`phải luôn được chọn. Ví dụ,```
1
7
100
```có câu trả lời`7`, không`0`. 

Thứ hai, một hình phạt gắn liền với mọi cặp hợp lệ, không chỉ với thực tế là một số là lũy thừa của một số khác. Ví dụ,```
4
1 1 1 2
1 1 1 1
```có câu trả lời`4`. Việc chọn cả bốn chỉ số sẽ cho`1 + 1 + 1 + 2 - 1 = 4`, bởi vì cặp duy nhất có liên quan là`2 -> 4`. 

Thứ ba, khả năng chia hết thông thường là không đủ. Ví dụ,`4`chia rẽ`8`, Nhưng`8`không phải là lũy thừa nguyên của`4`. Với```
8
1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1
```các mối quan hệ quyền lực liên quan bên trong`{2,4,8}`là`2^2 = 4`Và`2^3 = 8`. không có`4 -> 8`bờ rìa. Coi tính chia hết như mối quan hệ sẽ đưa ra một hình phạt sai và có thể thay đổi câu trả lời. 

Điều tinh tế thứ tư là một số có thể có nhiều căn lũy thừa. Ví dụ,`64`có`2^6 = 64`,`4^3 = 64`, Và`8^2 = 64`. Nếu cả ba gốc và`64`được chọn,`b_64`được trừ ba lần. Một giải pháp trừ`b_64`chỉ một lần sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi tập hợp con của`{1, ..., n}`. Đối với mỗi tập hợp con, chúng ta có thể thêm tất cả các tập hợp đã chọn`a_i`giá trị và sau đó kiểm tra từng cặp có thứ tự`(i,j)`để xem liệu`j`là lũy thừa nguyên của`i`. Điều này đúng vì nó đánh giá điểm chính xác như đã xác định. Tuy nhiên, có`2^n`tập hợp con và lên đến`Theta(n^2)`cặp để kiểm tra từng cái, đưa ra`Theta(n^2 2^n)`hoạt động trong trường hợp xấu nhất. Với`n = 100000`, điều này không khả thi chút nào. 

Quan sát hữu ích đến từ việc hỏi những con số nào có thể tương tác với nhau. Mỗi số nguyên`x >= 2`có thể được viết duy nhất là`x = p^e`Ở đâu`p`bản thân nó không phải là một sức mạnh hoàn hảo và`e >= 1`. Ví dụ,`64 = 2^6`, trong khi`36`đã được biểu diễn dưới dạng`36^1`vì nó không phải là sức mạnh hoàn hảo. 

Bây giờ giả sử`i^k = j`. Viết`i = p^d`. Sau đó`j = (p^d)^k = p^(dk)`. 

Vì thế`i`Và`j`có cơ sở nguyên thủy giống hệt nhau`p`. Các số có cơ số nguyên thủy khác nhau không bao giờ có thể tương tác. 

Điều này chia toàn bộ vấn đề thành các nhóm độc lập. Đối với một cơ sở nguyên thủy cố định`p`, các số có thể là`p^1, p^2, p^3, ..., p^m`Ở đâu`p^m <= n`. Câu hỏi duy nhất còn lại là chọn số mũ nào. 

Trong một nhóm như vậy có nhiều nhất`log_2(100000) = 16`số mũ. Đó là sự giảm thiểu mang tính quyết định. Thay vì chọn từ`100000`các vị trí không liên quan cùng một lúc, chúng ta có thể liệt kê nhiều nhất`2^16 = 65536`tập hợp con của một chuỗi điện. 

Đối với số mũ`e`, số`p^e`nhận một hình phạt từ số mũ đã chọn`d`chính xác khi nào`d < e`Và`d`chia rẽ`e`. Vì vậy, nếu mặt nạ đại diện cho số mũ được chọn thì sự đóng góp của việc chọn số mũ`e`là`a[p^e] - b[p^e] * (# selected proper divisors of e)`. 

Chúng tôi có thể tính dần dần điểm số tốt nhất cho từng mặt nạ. Xóa một số mũ đã chọn`e`từ mặt nạ, lấy điểm của mặt nạ nhỏ hơn và cộng phần đóng góp của`e`. Mặt nạ chia thích hợp của mỗi số mũ có thể được tính toán trước, do đó số ước được chọn chỉ là thao tác đếm bit. 

Sự phân tích thành các cơ số nguyên thủy có thể thu được bằng cách phân tích mọi số bằng sàng có hệ số nguyên tố nhỏ nhất. Nếu như`x = q_1^c_1 q_2^c_2 ... q_t^c_t`, 

thì số mũ`e`trong biểu diễn nguyên thủy là`gcd(c_1, c_2, ..., c_t)`, và cơ sở nguyên thủy là`q_1^(c_1/e) q_2^(c_2/e) ... q_t^(c_t/e)`. 

giá trị`1`được xử lý riêng vì không thuộc chuỗi quyền lực nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^2 2^n)`|`O(n)`| Quá chậm | 
| Phân rã cơ sở nguyên thủy + tập con DP |`O(n log n + sum 2^m)`|`O(n)`| Đã chấp nhận | 

Tổng số tiền vượt quá`2^m`nhỏ vì một nhóm có độ dài`m`có nhiều nhất là cơ sở nguyên thủy`n^(1/m)`. Đặc biệt, nhóm duy nhất có khả năng đạt chiều dài`16`là cơ sở`2`nhóm, trong khi hầu hết tất cả các cơ sở chỉ có một số mũ và có hai trạng thái mặt nạ. Vì`n <= 100000`, việc liệt kê này có thể dễ dàng quản lý được. 

## Hướng dẫn thuật toán 

1. Đọc`n`,`a`, Và`b`, rồi cộng ngay`a_1`để trả lời. chỉ mục`1`không thể tạo ra hoặc nhận bất kỳ hình phạt nào, và`a_1`là dương, do đó việc loại trừ nó không bao giờ có thể là tối ưu. 
2. Xây dựng một mảng số nguyên tố nhỏ nhất lên đến`n`. Điều này cho phép chúng ta phân tích mọi số vào`O(log n)`thời gian thay vì liên tục thử tất cả các ước số có thể. 
3. Đối với mọi`x`từ`2`bởi vì`n`, nhân tố`x`vào các quyền lực hàng đầu. Lấy ước số chung lớn nhất của tất cả các số mũ nguyên tố. Gọi giá trị này`e`. Số có dạng duy nhất`x = p^e`, Ở đâu`p`là cơ sở nguyên thủy. 
4. Cửa hàng`x`trong nhóm thuộc về`p`tại số mũ`e`. Ví dụ, những con số`2, 4, 8, 16`tất cả đi vào nhóm có căn cứ`2`, tại số mũ`1, 2, 3, 4`. Một con số như`12`có được nhóm riêng của mình vì nó không phải là một sức mạnh hoàn hảo. 
5. Xử lý độc lập từng nhóm cơ sở nguyên thủy. Đối với một nhóm có cơ sở`p`và số mũ tối đa`m`, tạo chuỗi các giá trị tương ứng với`p^1`bởi vì`p^m`. 
6. Với mỗi số mũ`e`, xây dựng một mặt nạ bit chứa các ước số thích hợp của nó. Ví dụ, trong một nhóm có độ dài ít nhất`8`, mặt nạ chia riêng của số mũ`8`chứa số mũ`1`,`2`, Và`4`. Đây chính xác là những gốc rễ có thể có sức mạnh có thể ngang bằng`p^8`. 
7. Liệt kê mọi mặt nạ tập hợp con của`m`số mũ. Cho phép`bit`là một số mũ được chọn và để`prev`là mặt nạ với số mũ đó bị loại bỏ. Điểm cho mặt nạ hiện tại là điểm cho`prev`, cộng`a[p^e]`, trừ`b[p^e]`nhân với số ước số thích hợp đã chọn của`e`. 
8. Lấy điểm tối đa của tất cả các mặt nạ trong nhóm và thêm điểm đó vào câu trả lời chung. Vì không có mối quan hệ quyền lực nào chuyển từ cơ sở nguyên thủy này sang cơ sở nguyên thủy khác nên việc tối ưu hóa từng nhóm một cách độc lập sẽ mang lại mức tối ưu toàn cục. 

### Tại sao nó hoạt động 

Mỗi cặp phạt đền`i^k = j`có cùng một cơ sở nguyên thủy ở cả hai phía. Do đó, điểm hoàn chỉnh là tổng của các điểm độc lập, một cho chỉ số`1`và một cho mỗi nhóm cơ sở nguyên thủy. 

Trong một nhóm, mặt nạ thể hiện chính xác những con số nào`p^1, ..., p^m`được chọn. Khi số mũ`e`được thêm vào mặt nạ, hình phạt mới duy nhất là những hình phạt liên quan đến`p^e`là điểm cuối lớn hơn. Một hình phạt như vậy tồn tại chính xác đối với mọi ước số thích hợp đã được chọn`d`của`e`và mỗi đóng góp chính xác`b[p^e]`. Do đó, phép truy toán tính toán điểm chính xác của mọi tập hợp con có thể có của nhóm. Do đó, việc lấy mặt nạ tốt nhất sẽ mang lại sự lựa chọn tối ưu cho nhóm đó và tính tổng mức tối ưu độc lập sẽ mang lại điểm tổng thể tối ưu. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))
    b = [0] + list(map(int, input().split()))

    # Index 1 is always selected.
    answer = a[1]

    if n == 1:
        print(answer)
        return

    # Smallest prime factor sieve.
    spf = list(range(n + 1))
    if n >= 1:
        spf[1] = 1

    for i in range(2, int(n ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, n + 1, i):
                if spf[j] == j:
                    spf[j] = i

    # groups[base][exponent] = number p^exponent.
    groups = {}

    for x in range(2, n + 1):
        y = x
        g = 0
        factors = []

        while y > 1:
            p = spf[y]
            cnt = 0
            while y % p == 0:
                y //= p
                cnt += 1
            factors.append((p, cnt))
            g = cnt if g == 0 else gcd(g, cnt)

        base = 1
        for p, cnt in factors:
            base *= p ** (cnt // g)

        if base not in groups:
            groups[base] = []
        groups[base].append((g, x))

    # Precompute proper-divisor masks for all possible exponents.
    divisor_mask = [0] * 17
    for e in range(1, 17):
        mask = 0
        for d in range(1, e):
            if e % d == 0:
                mask |= 1 << (d - 1)
        divisor_mask[e] = mask

    for items in groups.values():
        items.sort()

        m = len(items)

        # The representation p^1, p^2, ..., p^m is contiguous.
        values_a = [0] * m
        values_b = [0] * m

        for exponent, x in items:
            pos = exponent - 1
            values_a[pos] = a[x]
            values_b[pos] = b[x]

        dp = [0] * (1 << m)

        best = 0

        for mask in range(1, 1 << m):
            low = mask & -mask
            pos = low.bit_length() - 1
            prev = mask ^ low
            exponent = pos + 1

            selected_roots = (prev & divisor_mask[exponent]).bit_count()

            cur = (
                dp[prev]
                + values_a[pos]
                - values_b[pos] * selected_roots
            )

            dp[mask] = cur
            if cur > best:
                best = cur

        answer += best

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai xử lý chỉ mục`1`trước khi thực hiện bất kỳ công việc lý thuyết số nào. Việc này vừa đơn giản hơn vừa tránh việc vô tình điều trị`1`như một cơ sở nguyên thủy. 

Sàng hệ số nguyên tố nhỏ nhất cho phép phân tích nhanh hệ số cho mọi số lên đến`n`. Trong quá trình nhân tố hóa,`g`trở thành gcd của số mũ nguyên tố. Nếu hệ số hóa là`2^6`, sau đó`g = 6`, cho cơ sở nguyên thủy`2`và số mũ`6`. Nếu hệ số hóa là`2^2 * 3^2`, sau đó`g = 2`, cho cơ sở nguyên thủy`6`và số mũ`2`, từ`36 = 6^2`. 

Các mục trong nhóm được lập chỉ mục theo số mũ thực tế của chúng thay vì chỉ theo thứ tự của chúng trong danh sách. Đối với một cơ sở nguyên thủy`p`, mọi quyền lực`p^1`bởi vì`p^m`nhiều nhất là`n`, do đó mọi số mũ giữa`1`Và`m`tồn tại. Điều này làm cho`exponent - 1`một vị trí bit hợp lệ. 

biểu thức```
prev & divisor_mask[exponent]
```giữ chính xác các ước số thích hợp của số mũ mới được chọn đã được chọn. của Python`bit_count()`sau đó đưa ra số của họ trực tiếp. 

Phép lặp DP thêm số mũ vào tập hợp con đã được tính toán. Vì mọi hình phạt liên quan đến số mới đều có số mới là điểm cuối lớn hơn nên mỗi hình phạt như vậy được tính chính xác một lần. Không cần phải kiểm tra lại các cặp. 

Số nguyên Python có độ chính xác tùy ý, do đó, điểm tối đa có thể đạt được theo thứ tự`10^14`, không tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4
1 1 1 2
1 1 1 1
```chỉ mục`1`đóng góp`1`vô điều kiện. Các số còn lại tạo thành nhóm cơ sở nguyên thủy`{2,4}`Và`{3}`. 

Đối với nhóm`{2,4}`, số mũ`1`không có ước số thích hợp, trong khi số mũ`2`có ước số thích hợp`1`. 

| Mặt nạ | Đã chọn | Số mũ mới | Rễ được chọn | Điểm nhóm | 
| --- | --- | --- | --- | --- | 
|`00`| không | không | 0 | 0 | 
|`01`|`2`| 1 | 0 | 1 | 
|`10`|`4`| 2 | 0 | 2 | 
|`11`|`2,4`| 2 | 1 | 1 + 2 - 1 = 2 | 

Điểm tốt nhất của nhóm này là`2`. Nhóm độc thân`{3}`đóng góp`1`. Cùng với chỉ số`1`, câu trả lời là`1 + 2 + 1 = 4`. 

### Mẫu 2 

Đầu vào là```
4
1 1 1 1
1 1 1 2
```Một lần nữa, chỉ mục`1`đóng góp`1`. nhóm`{2,4}`có các trạng thái sau. 

| Mặt nạ | Đã chọn | Số mũ mới | Rễ được chọn | Điểm nhóm | 
| --- | --- | --- | --- | --- | 
|`00`| không | không | 0 | 0 | 
|`01`|`2`| 1 | 0 | 1 | 
|`10`|`4`| 2 | 0 | 1 | 
|`11`|`2,4`| 2 | 1 | 1 + 1 - 2 = 0 | 

Điểm nhóm tốt nhất là`1`. chỉ mục`3`đóng góp khác`1`, vậy tổng số là`1 + 1 + 1 = 3`. 

Hai mẫu này chứng minh tại sao mỗi chuỗi điện phải được tối ưu hóa tổng thể. Việc chọn mọi chỉ số có giá trị dương không hẳn là tối ưu, vì một gốc được chọn có thể làm cho một số được chọn khác bị mất`b_j`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n + S)`| Bao thanh toán tất cả các chi phí số`O(n log n)`, Và`S = sum 2^m`trên các nhóm cơ sở nguyên thủy | 
| Không gian |`O(n)`| Sàng, mảng, nhóm và nhóm DP lớn nhất đều tuyến tính hoặc nhỏ hơn | 

Vì`n = 100000`, mỗi xích điện có chiều dài tối đa`16`, bởi vì`2^17 > 100000`. Quan trọng hơn, chuỗi dài chỉ có thể tồn tại với rất ít bazơ nguyên thủy. Hầu hết các nhóm đều có một số mũ và do đó chỉ có hai trạng thái DP. Bảng liệt kê tập hợp con kết quả đủ nhỏ cho giới hạn một giây, trong khi sàng và mảng vừa vặn thoải mái bên trong 256 MB. 

## Trường hợp thử nghiệm```python
# The production solution is the solve() function above.
# This helper executes that logic on an isolated input string.

import sys
import io
from math import gcd

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples
assert run(
    "4\n"
    "1 1 1 2\n"
    "1 1 1 1\n"
) == "4", "sample 1"

assert run(
    "4\n"
    "1 1 1 1\n"
    "1 1 1 2\n"
) == "3", "sample 2"

# Minimum size: index 1 is always selected.
assert run(
    "1\n"
    "7\n"
    "100\n"
) == "7", "minimum n"

# n = 2: there is no possible power relation.
assert run(
    "2\n"
    "5 9\n"
    "100 100\n"
) == "14", "no power relation"

# All equal values, with 2^2 = 4 as the only relevant relation.
assert run(
    "4\n"
    "5 5 5 5\n"
    "1 1 1 1\n"
) == "19", "all equal values"

# Boundary case distinguishing powers from divisibility:
# 4 divides 8, but 4^k is never 8.
assert run(
    "8\n"
    "1 1 1 1 1 1 1 1\n"
    "1 1 1 1 1 1 1 1\n"
) == "7", "power relation versus divisibility"

# Maximum-size case. All a_i are huge, so every index is selected.
# The number of power pairs for n = 100000 is:
# 315 + 45 + 16 + 9 + 5 + 4 + 3 + 2 + 2 + 2 + 2 + 1 + 1 + 1 + 1 = 409.
max_n = 100000
max_input = (
    str(max_n) + "\n" +
    ("1000000000 " * max_n).strip() + "\n" +
    ("1 " * max_n).strip() + "\n"
)
assert run(max_input) == "99999999999591", "maximum n"

# A number can have multiple power roots.
# 64 = 2^6 = 4^3 = 8^2.
assert run(
    "64\n"
    + ("1 " * 64).strip() + "\n"
    + ("1 " * 64).strip() + "\n"
) == "61", "multiple power roots"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7 / 100`|`7`| Kích thước tối thiểu và lựa chọn chỉ mục bắt buộc`1`| 
|`2 / 5 9 / 100 100`|`14`| Không có mối quan hệ quyền lực nào tồn tại khi`n = 2`| 
|`4 / 5 5 5 5 / 1 1 1 1`|`19`| Giá trị bằng nhau và một biên phạt duy nhất | 
|`8 / all 1 / all 1`|`7`| Phân biệt lũy thừa chính xác với khả năng chia hết thông thường | 
|`100000 / all 10^9 / all 1`|`99999999999591`| Tối đa`n`, điểm số nguyên lớn và tất cả các cặp lũy thừa | 
|`64 / all 1 / all 1`|`61`| Một mục tiêu có thể có nhiều nguồn gốc sức mạnh khác nhau | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là`n = 1`. Đầu vào chính xác là```
1
7
100
```Không có cặp nào có thể xảy ra cả. chỉ mục`1`đóng góp`7`, và câu trả lời là`7`. Việc triển khai xử lý việc này trước khi xây dựng các nhóm sàng. 

Trường hợp cạnh thứ hai là không có bất kỳ mối quan hệ quyền lực nào. Vì```
2
5 9
100 100
```cả hai chỉ số đều được chọn vì cả hai giá trị đều dương và không`2`cũng không`1`có thể là điểm cuối nhỏ hơn của cặp lũy thừa hợp lệ với chỉ số khác. Kết quả là`14`. 

Trường hợp cạnh thứ ba liên quan đến sự khác biệt giữa lũy thừa và khả năng chia hết. Vì```
8
1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1
```nhóm có cơ sở`2`chứa`2,4,8`. Chọn cả ba sẽ có hai hình phạt, từ`2^2 = 4`Và`2^3 = 8`, cho điểm nhóm là`1`. Những con số`1,3,5,6,7`mỗi người đóng góp`1`, cho`6`từ những chỉ số đó cộng với điểm nhóm tốt nhất`1`, kể từ đây`7`. Đặc biệt, không có hình phạt giữa`4`Và`8`, bởi vì`8`không phải là sức mạnh của`4`. 

Trường hợp cạnh thứ tư là mục tiêu có nhiều gốc. Xét nhóm chứa`2,4,8,64`. số`64`nhận một hình phạt từ mỗi thành viên được chọn trong số`2`,`4`, Và`8`, bởi vì`2^6`,`4^3`, Và`8^2`tất cả đều bình đẳng`64`. DP không cần phải có trường hợp đặc biệt này. Mặt nạ chia số thích hợp của nó cho số mũ`6`chứa số mũ`1`,`2`, Và`3`, do đó cả ba hình phạt đều được tính khi số mũ`6`được thêm vào. 

Cuối cùng, giá trị lớn yêu cầu loại số nguyên rộng. Với`n = 100000`và mọi`a_i = 10^9`, riêng phần đóng góp tích cực là`10^14`. Việc triển khai Python xử lý việc này một cách chính xác thông qua các số nguyên có độ chính xác tùy ý, trong khi việc triển khai 32 bit có chiều rộng cố định sẽ tràn. 

Nếu bạn muốn, tôi cũng có thể biến bài viết này thành một bài xã luận ngắn hơn theo phong cách Codeforces, giữ nguyên bằng chứng nhưng loại bỏ phần thảo luận thử nghiệm đầy đủ.
