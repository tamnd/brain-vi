---
title: "CF 106260C - ba lô"
description: "Chúng ta được cung cấp một đồ thị có hướng của các hang động và đường hầm. Chúng tôi bắt đầu ở hang 1 với không có tảng đá nào trong ba lô và muốn đến hang n. Một đường hầm thêm đá khi chúng ta đi vào nó, sau đó loại bỏ b đá khi chúng ta rời khỏi nó."
date: "2026-06-25T07:24:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106260
codeforces_index: "C"
codeforces_contest_name: "2025 SiChuan University for new student"
rating: 0
weight: 106260
solve_time_s: 76
verified: true
draft: false
---

[CF 106260C - ba lô](https://codeforces.com/problemset/problem/106260/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Solve time:** 1m 16s
 **Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng của các hang động và đường hầm. We start in cavern`1`không có hòn đá nào trong ba lô và muốn đến hang động`n`. 

Một đường hầm bổ sung`a`đá khi chúng ta bước vào nó, sau đó loại bỏ`b`đá khi chúng ta rời khỏi nó. Nếu chúng ta có ít hơn`b`đá có sẵn, đường hầm loại bỏ mọi thứ. Nếu số lượng đá hiện tại là`r`, thì sau khi đi qua một đường hầm số tiền mới là```
max(0, r + a - b)
```Thật thuận tiện để xác định```
w = a - b
```vì vậy mọi đường hầm đều áp dụng```
r -> max(0, r + w)
```Nhiệm vụ là tìm số lượng đá tối thiểu có thể có khi chúng ta lần đầu tiên đến hang động.`n`. Nếu hang động`n`không thể truy cập được, câu trả lời là`-1`. 

Đồ thị có nhiều nhất là 300 đỉnh, nhưng có thể rất dày đặc, lên tới`n(n-1)`các cạnh có hướng. Các tham số đường hầm thỏa mãn`0 ≤ a, b ≤ 10`, do đó, mỗi cạnh thay đổi số lượng đá tối đa là 10 trước khi áp dụng thao tác sàn ở mức 0. 

Trọng số cạnh nhỏ là quan sát chính. Một đường dẫn đơn giản chứa nhiều nhất`n - 1`các cạnh, do đó dọc theo bất kỳ đường đi đơn giản nào tổng mức giảm tối đa là```
10 * (n - 1) ≤ 2990
```Điều này ngay lập tức gợi ý rằng chỉ cần thể hiện một cách rõ ràng một phạm vi giới hạn số lượng đá. 

Trường hợp cạnh tinh tế xuất hiện khi chu kỳ tồn tại. 

Ví dụ:```
1 -> 2 : +10
2 -> 3 : -9
3 -> 2 : 0
```Giải pháp kiểu đường đi ngắn nhất không thành công vì việc xem lại các đỉnh sẽ thay đổi số lượng đá và có thể cải thiện câu trả lời cuối cùng. 

Một trường hợp quan trọng khác là khi một con đường đòi hỏi phải tích tụ tạm thời nhiều tảng đá trước khi giảm bớt chúng sau này. 

Ví dụ:```
1 -> 2 : +10
2 -> 2 : +10
2 -> n : -10
```Việc đi bộ tối ưu có thể sử dụng chu trình tích cực nhiều lần trước khi rời đi. Bất kỳ giải pháp nào chỉ xem xét những con đường đơn giản đều bỏ lỡ những bước đi như vậy. 

Cuối cùng, biểu đồ có thể chứa các chu trình dương có thể tiếp cận được cho phép số lượng đá lớn tùy ý. Chúng ta không thể lưu trữ mọi giá trị có thể một cách rõ ràng, do đó không gian trạng thái phải được nén cẩn thận. 

## Phương pháp tiếp cận 

Tìm kiếm brute-force coi một trạng thái là`(vertex, rocks)`và khám phá tất cả các trạng thái có thể tiếp cận. 

Điều này đúng vì quá trình chuyển đổi mang tính quyết định:```
r -> max(0, r + w)
```Vấn đề là số lượng đá về mặt lý thuyết là không giới hạn. Một chu kỳ tích cực có thể đạt được có thể tăng số lượng đá mãi mãi, tạo ra vô số trạng thái. 

Quan sát quan trọng là trọng số của các cạnh được giới hạn bởi 10 và đồ thị chỉ chứa 300 đỉnh. 

Cho phép```
B = 10 * (n - 1)
```Đối với vấn đề này,```
B ≤ 2990
```Giả sử hiện tại chúng ta có nhiều hơn`B`đá. Không có con đường đơn giản nào có thể giảm số lượng nhiều hơn`B`, bởi vì mọi cạnh đều đóng góp ít nhất`-10`. 

Điều đó có nghĩa là khi số đếm trở nên đủ lớn thì giá trị chính xác không còn quan trọng nữa. Mọi giá trị trên ngưỡng cố định đều hoạt động tương tự. Chúng tôi có thể lưu trữ tất cả số lượng lên đến một giới hạn một cách rõ ràng và hợp nhất mọi thứ lớn hơn vào một trạng thái đặc biệt duy nhất. 

Một sự lựa chọn thuận tiện là```
LIMIT = B + 10 = 3000
```Nếu chúng ta có hơn 3000 tảng đá thì sau khi đi qua bất kỳ cạnh nào chúng ta vẫn có hơn 2990 tảng đá. Hoạt động ở mức 0 không bao giờ có thể trở nên phù hợp khi chúng tôi ở lại khu vực này. 

Điều này chuyển đổi không gian trạng thái vô hạn thành một không gian hữu hạn:```
(vertex, 0..3000)
```cộng thêm một trạng thái "lớn" trên mỗi đỉnh. 

Thách thức còn lại là tính hiệu quả. Thay vì lưu trữ số lượng có thể truy cập riêng lẻ, chúng tôi lưu trữ chúng dưới dạng bitset. Một tập hợp bit có độ dài 3001 vừa vặn thoải mái với số nguyên Python và mọi phép biến đổi cạnh sẽ trở thành một vài thao tác bit. 

Thuật toán kết quả là sự lan truyền khả năng tiếp cận đơn điệu trên một biểu đồ hữu hạn và hội tụ đến tập hợp đầy đủ các trạng thái có thể tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về số lượng đá chính xác | Không giới hạn | Không giới hạn | Quá chậm | 
| Khả năng tiếp cận Bitset với trạng thái nén lớn | O(m · LIMIT / word_size) trong thực tế | O(n · GIỚI HẠN) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi đường hầm tính toán```
w = a - b
```vì quá trình chuyển đổi chỉ phụ thuộc vào giá trị này. 
2. Hãy để```
LIMIT = 3000
```Đếm`0..3000`được thể hiện một cách rõ ràng. 
3. Với mỗi đỉnh duy trì:```
bits[v]
```chút ở đâu`k`được thiết lập nếu số lượng đá`k`có thể đạt được ở đỉnh`v`. 

Cũng duy trì```
big[v]
```điều đó có nghĩa là một số số lớn hơn 3000 có thể truy cập được ở đỉnh`v`. 
4. Khởi tạo```
bits[1] = {0}
```bởi vì chúng ta bắt đầu ở hang 1 với 0 viên đá. 
5. Truyền đi nhiều lần thông tin dọc theo các cạnh đi ra. 

Đối với trọng số không âm`w`:```
k -> k + w
```Số lượng vượt quá 3000 sẽ tạo ra`big`tình trạng. 
6. Đối với trọng lượng âm`-d`:```
k -> max(0, k - d)
```Điều này được thực hiện bằng cách dịch chuyển bitset sang phải, cộng với cài đặt bit 0 nếu có bất kỳ số đếm nào bên dưới`d`đã có thể truy cập được. 
7. Nếu`big[u]`là đúng và có một cạnh`u -> v`, sau đó`big[v]`cũng trở thành sự thật. 

Bất kỳ số lượng nào trên 3000 vẫn ở trên 2990 sau một lần chuyển đổi, vì vậy giá trị chính xác là không liên quan. 
8. Sử dụng hàng đợi. Bất cứ khi nào một đỉnh nhận được trạng thái có thể tiếp cận mới, hãy đẩy nó vào hàng đợi để các cạnh đi ra của nó được xử lý lại. 
9. Sau khi đạt đến điểm cố định, kiểm tra tập bit của đỉnh`n`. 

Bit được đặt nhỏ nhất là số lượng đá tối thiểu có thể tiếp cận. 
10. Nếu cả bitset và trạng thái lớn đều không thể truy cập được ở đỉnh`n`, đầu ra`-1`. 

### Tại sao nó hoạt động 

Thuật toán tính toán điểm cố định ít nhất của mối quan hệ khả năng tiếp cận. 

Mọi trạng thái có thể truy cập đều được tạo vì chúng tôi bắt đầu từ trạng thái ban đầu và áp dụng liên tục tất cả các chuyển đổi hợp lệ. Không có trạng thái có thể truy cập được bỏ qua. 

Việc nén là an toàn vì các giá trị lớn hơn 3000 không bao giờ có thể quay trở lại trực tiếp phạm vi rõ ràng chỉ trong một bước. Mức giảm tối đa của một cạnh là 10, do đó số lượng lớn hơn 3000 luôn lớn hơn 2990 sau khi chuyển đổi. Giá trị chính xác không liên quan khi chúng ta vào vùng này và giá trị duy nhất`big`nhà nước nắm bắt tất cả các khả năng như vậy. 

Vì không gian trạng thái hữu hạn được khám phá một cách thấu đáo và mọi chuyển đổi đều được mô hình hóa chính xác, nên tập hợp số lượng đá có thể tiếp cận được tại hang động`n`là đúng. Lấy số lượng nhỏ nhất có thể tiếp cận sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

LIMIT = 3000
MASK = (1 << (LIMIT + 1)) - 1

def transform(bits, w):
    if bits == 0:
        return 0, False

    if w >= 0:
        shifted = bits << w

        low = shifted & MASK
        high = shifted >> (LIMIT + 1)

        return low, high != 0

    d = -w

    low_part = bits & ((1 << d) - 1) if d <= LIMIT else bits
    res = bits >> d

    if low_part:
        res |= 1

    return res, False

def solve():
    n, m = map(int, input().split())

    g = [[] for _ in range(n)]

    for _ in range(m):
        x, y, a, b = map(int, input().split())
        g[x - 1].append((y - 1, a - b))

    bits = [0] * n
    big = [False] * n

    bits[0] = 1  # rock count 0

    q = deque([0])
    inq = [False] * n
    inq[0] = True

    while q:
        u = q.popleft()
        inq[u] = False

        cur_bits = bits[u]
        cur_big = big[u]

        for v, w in g[u]:
            add_bits, make_big = transform(cur_bits, w)

            changed = False

            new_bits = bits[v] | add_bits
            if new_bits != bits[v]:
                bits[v] = new_bits
                changed = True

            if cur_big or make_big:
                if not big[v]:
                    big[v] = True
                    changed = True

            if changed and not inq[v]:
                inq[v] = True
                q.append(v)

    target = bits[n - 1]

    if target == 0 and not big[n - 1]:
        print(-1)
        return

    answer = (target & -target).bit_length() - 1
    print(answer)

solve()
```Giải pháp lưu trữ tất cả số lượng có thể truy cập lên tới 3000 bên trong một số nguyên Python. Chút`k`tương ứng với số lượng đá`k`. 

Đối với trọng số cạnh không âm, chúng ta dịch bitset sang trái. Bất kỳ bit nào di chuyển ra ngoài vị trí 3000 đều biểu thị số lượng lớn hơn phạm vi rõ ràng, do đó đỉnh đích sẽ nhận được`big`lá cờ. 

Đối với trọng số cạnh âm, chúng ta dịch chuyển sang phải. Số đếm nhỏ hơn độ lớn của trọng số âm sẽ giảm về 0 do`max(0, ...)`hoạt động. Đó là lý do tại sao bit 0 phải được thêm vào bất cứ khi nào có thể truy cập được một trong những số đếm nhỏ đó. 

Truyền bá dựa trên hàng đợi là một quy trình luồng dữ liệu đơn điệu tiêu chuẩn. Một đỉnh chỉ được xử lý lại khi tập trạng thái có thể tiếp cận của nó tăng lên. 

biểu hiện```
(target & -target).bit_length() - 1
```trả về chỉ số của bit được đặt thấp nhất, chính xác là số lượng đá nhỏ nhất có thể tiếp cận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2 10 0
2 4 2 1
4 3 0 5
3 2 3 3
```Trọng số cạnh hiệu quả là:```
1 -> 2 : +10
2 -> 4 : +1
4 -> 3 : -5
3 -> 2 : 0
```| Bước | Đỉnh | Đá | 
| --- | --- | --- | 
| Bắt đầu | 1 | 0 | 
| 1 -> 2 | 2 | 10 | 
| 2 -> 4 | 4 | 11 | 
| 4 -> 3 | 3 | 6 | 
| 3 -> 2 | 2 | 6 | 
| 2 -> 4 | 4 | 7 | 
| 4 -> 3 | 3 | 2 | 
| 3 -> 2 | 2 | 2 | 
| 2 -> 4 | 4 | 3 | 
| 4 -> 3 | 3 | 0 | 
| 3 -> 2 | 2 | 0 | 
| 2 -> 4 | 4 | 1 | 

Giá trị tối thiểu có thể tiếp cận tại hang 4 là 1. 

### Ví dụ 2 

đầu vào:```
3 2
1 2 5 0
2 3 0 10
```Trọng lượng hiệu quả:```
1 -> 2 : +5
2 -> 3 : -10
```| Bước | Đỉnh | Đá | 
| --- | --- | --- | 
| Bắt đầu | 1 | 0 | 
| 1 -> 2 | 2 | 5 | 
| 2 -> 3 | 3 | 0 | 

Cạnh thứ hai loại bỏ tất cả các loại đá có sẵn nên câu trả lời là 0. 

Ví dụ này chứng tỏ tại sao quá trình chuyển đổi không phải là phép cộng thông thường. Giá trị âm không bao giờ được phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · LIMIT / word_size) trong thực tế | Hoạt động của Bitset xử lý nhiều số lượng đá cùng một lúc | 
| Không gian | O(n · GIỚI HẠN) | Một bit có độ dài 3001 mỗi đỉnh | 

Với`n ≤ 300`Và`LIMIT = 3000`, tổng số biểu diễn trạng thái dưới một triệu bit. Việc truyền bá dựa trên bitset dễ dàng phù hợp với giới hạn bộ nhớ và đủ nhanh cho kích thước đồ thị trong bài toán này. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    LIMIT = 3000
    MASK = (1 << (LIMIT + 1)) - 1

    def transform(bits, w):
        if bits == 0:
            return 0, False

        if w >= 0:
            shifted = bits << w
            return shifted & MASK, (shifted >> (LIMIT + 1)) != 0

        d = -w
        low_part = bits & ((1 << d) - 1)
        res = bits >> d
        if low_part:
            res |= 1
        return res, False

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())

    g = [[] for _ in range(n)]

    for _ in range(m):
        x, y, a, b = map(int, input().split())
        g[x - 1].append((y - 1, a - b))

    from collections import deque

    bits = [0] * n
    big = [False] * n

    bits[0] = 1

    q = deque([0])
    inq = [False] * n
    inq[0] = True

    while q:
        u = q.popleft()
        inq[u] = False

        for v, w in g[u]:
            add_bits, make_big = transform(bits[u], w)

            changed = False

            nb = bits[v] | add_bits
            if nb != bits[v]:
                bits[v] = nb
                changed = True

            if big[u] or make_big:
                if not big[v]:
                    big[v] = True
                    changed = True

            if changed and not inq[v]:
                inq[v] = True
                q.append(v)

    target = bits[n - 1]

    if target == 0 and not big[n - 1]:
        return "-1\n"

    ans = (target & -target).bit_length() - 1
    return str(ans) + "\n"

# sample
assert run(
"""4 4
1 2 10 0
2 4 2 1
4 3 0 5
3 2 3 3
"""
) == "1\n"

# minimum graph
assert run(
"""1 1
1 1 0 0
"""
) == "0\n"

# unreachable target
assert run(
"""3 1
1 2 1 0
"""
) == "-1\n"

# saturation at zero
assert run(
"""3 2
1 2 5 0
2 3 0 10
"""
) == "0\n"

# positive cycle
assert run(
"""3 3
1 2 10 0
2 2 10 0
2 3 0 10
"""
) == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị một đỉnh | 0 | Bắt đầu bằng đích đến | 
| Điểm đến không thể truy cập | -1 | Xử lý khả năng tiếp cận | 
| Cạnh âm lớn | 0 | Đúng hành vi ở mức 0 | 
| Chu kỳ tích cực | 0 | Nén trạng thái vô hạn | 
| Biểu đồ mẫu | 1 | Logic vấn đề đầy đủ | 

## Vỏ cạnh 

Hãy xem xét:```
3 2
1 2 5 0
2 3 0 10
```Sự chuyển tiếp từ 5 tảng đá qua cạnh thứ hai là```
max(0, 5 - 10) = 0
```Thuật toán xử lý việc này thông qua phép biến đổi trọng số âm. Bất kỳ số đếm nào dưới mức giảm yêu cầu đều góp phần tạo ra bit 0 trong tập hợp bit đích. 

Bây giờ hãy xem xét một điểm đến không thể truy cập được:```
3 1
1 2 1 0
```Không có trạng thái nào được truyền tới đỉnh 3. Bitset của nó vẫn trống và`big`cờ vẫn sai, do đó thuật toán xuất ra chính xác`-1`. 

Cuối cùng, hãy xem xét một chu kỳ tích cực có thể đạt được:```
3 3
1 2 10 0
2 2 10 0
2 3 0 10
```Vòng tự ở đỉnh 2 có thể tăng số lượng đá mà không bị ràng buộc. Khi số lượng vượt quá giới hạn rõ ràng, thuật toán sẽ chuyển sang chế độ nén`big`đại diện. Khả năng tiếp cận vẫn chính xác mà không lưu trữ vô số giá trị.
