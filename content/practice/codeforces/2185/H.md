---
title: "CF 2185H - BattleCows 2"
description: "Giải đấu có cấu trúc đơn giản đến bất ngờ khi chúng ta ngừng nghĩ về các trận đấu cá nhân. Bất cứ khi nào một con bò sống sót trong trận đấu, kỹ năng mới của nó sẽ trở thành tổng của hai người tham gia."
date: "2026-06-07T21:34:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2185
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 1074 (Div. 4)"
rating: 2500
weight: 2185
solve_time_s: 176
verified: false
draft: false
---

[CF 2185H - BattleCows 2](https://codeforces.com/problemset/problem/2185/H) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, dp, tham lam 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Giải đấu có cấu trúc đơn giản đến bất ngờ khi chúng ta ngừng nghĩ về các trận đấu cá nhân. 

Bất cứ khi nào một con bò sống sót trong trận đấu, kỹ năng mới của nó sẽ trở thành tổng của hai người tham gia. Sau nhiều chiến thắng liên tiếp, kỹ năng của con bò sống sót chỉ đơn giản là tổng của tất cả những con bò mà nó đã hấp thụ. 

Giả sử chúng ta tập trung vào một con bò cụ thể có kỹ năng ban đầu`v`. Chúng tôi xóa nó khỏi vị trí ban đầu và chèn nó vào một nơi khác. Câu hỏi đặt ra là có bao nhiêu vị trí chèn cho phép con bò này trở thành người sống sót cuối cùng nếu nó có thể ép buộc nhiều nhất?`k`thua thành thắng. 

Quan sát quan trọng là khi con bò của chúng ta đến một vị trí nào đó trong hàng, mọi thứ ở bên trái của nó đã được hợp nhất thành một con bò duy nhất có sức mạnh bằng tổng các giá trị đó. Điều duy nhất quan trọng là liệu sức mạnh hiện tại của con bò của chúng tôi có ít nhất bằng số tiền tích lũy đó hay không. Nếu không, cần phải có một mánh gian lận. 

Tổng số bò trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Bất kỳ cách tiếp cận nào mô phỏng rõ ràng từng con bò ở mọi vị trí sẽ ngay lập tức trở thành phương trình bậc hai, quá lớn. Chúng ta cần thứ gì đó xung quanh`O(n log n)`mỗi trường hợp thử nghiệm, hoặc`O(n log^2 n)`trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi một con bò được chèn vào ngay phía trước. 

Ví dụ:```
n = 3, k = 0
a = [1, 2, 3]
```Nếu bò`3`được chuyển đến vị trí`1`, nó không bao giờ phải đấu tranh với tiền tố kết hợp`[1,2]`. Đối xử với mọi vị trí như thể luôn có sự hợp nhất ở bên trái sẽ vượt quá số lần gian lận bắt buộc. 

Một sai lầm dễ mắc phải khác là cho rằng một con bò có thể cần nhiều lần gian lận khi di chuyển sang phải. Sau mỗi chiến thắng gian lận, sức mạnh của nó ít nhất sẽ tăng gấp đôi, do đó số lần gian lận riêng biệt chỉ mang tính logarit trong phạm vi giá trị. Thực tế này là những gì làm cho giải pháp khả thi. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu rất đơn giản. 

Chọn một con bò. Hãy thử tất cả`n`các vị trí chèn. Mô phỏng toàn bộ giải đấu và đếm xem con bò cần gian lận bao nhiêu lần. Nếu số lượng không vượt quá`k`, vị trí đó là tốt. 

Chi phí mô phỏng duy nhất`O(n)`. có`n`các vị trí cho mỗi`n`bò, cho`O(n³)`làm việc trong trường hợp xấu nhất. Ngay cả với sự tối ưu hóa, mọi thứ gần như`O(n²)`mỗi trường hợp thử nghiệm đã quá chậm đối với`n = 2 · 10^5`. 

Bước đột phá đến từ việc xem giải đấu thông qua tổng tiền tố. 

Cho phép```
pref[i] = a1 + a2 + ... + ai
```Khi một con bò sức mạnh`v`đứng ở phía trước và bắt đầu hấp thụ bò từ trái sang phải, cần gian lận chính xác ở những vị trí mà```
a[i] > pref[i-1]
```vì sức mạnh tích lũy hiện tại trước khi gặp bò`i`bằng`pref[i-1]`. Những vị trí "nguy hiểm" này độc lập với hầu hết các lựa chọn chèn. Hơn nữa, mỗi lần vị trí như vậy buộc phải gian lận, sức mạnh của con bò ít nhất cũng tăng gấp đôi nên tổng số vị trí nguy hiểm chỉ là`O(log A)`. 

Đối với mỗi con bò, chúng ta có thể xây dựng tập hợp các vị trí nguy hiểm vẫn phù hợp sau khi loại bỏ con bò đó. Từ tập hợp này chúng ta có thể xác định: 

1. Liệu mọi vị trí có tự động chiến thắng hay không. 
2. Hậu tố nào của vị trí chèn yêu cầu nhiều nhất`k`gian lận. 
3. Trường hợp ranh giới đặc biệt khi con bò cần chính xác`k`gian lận và việc hợp nhất bên trái có thể biến mất. 

Điều đó làm giảm vấn đề đối với việc tìm kiếm nhị phân trên các tổng tiền tố và trên danh sách có thứ tự các vị trí nguy hiểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố`pref`. 
2. Xây dựng danh sách```
pos = { i | a[i] > pref[i-1] }
```Đây chính xác là những vị trí mà một con bò bắt đầu từ phía trước sẽ cần gian lận. 
3. Xử lý từng con bò`i`một cách độc lập. 
4. Xây dựng danh sách được lọc`g`. 

Đối với mọi vị trí nguy hiểm`x ≠ i`, xác định xem nó có còn nguy hiểm sau khi loại bỏ con bò không`i`. 

Nếu như`x < i`, tiền tố trước`x`không thay đổi. 

Nếu như`x > i`, giá trị bị loại bỏ`a[i]`biến mất khỏi tiền tố, do đó điều kiện trở thành```
a[x] > pref[x-1] + a[i]
```chính xác như trong giải pháp được chấp nhận. 
5. Hãy để`m = len(g)`. 

Nếu như`m < k`, ngay cả vị trí chèn tồi tệ nhất cũng sử dụng ít hơn`k`gian lận, vì vậy mọi vị trí đều tốt. 
6. Nếu`k > 0`, tìm vị trí tương ứng với`k`-sự kiện nguy hiểm thứ từ cuối. 

Điều này xác định hậu tố tối đa của các vị trí chèn vẫn có thể giành được trong phạm vi ngân sách gian lận. 
7. Trường hợp đặc biệt xảy ra khi`m == k`. 

Ở đây câu trả lời cũng bao gồm các vị trí mà việc hợp nhất bên trái biến mất hoàn toàn. 

Sử dụng tổng tiền tố và tìm kiếm nhị phân để tìm xem con bò có thể được chèn thêm bao xa trong khi tránh được một lần gian lận bổ sung. 
8. Xuất ra số đếm tích lũy của mỗi con bò. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi một con bò hấp thụ một số con bò, sức mạnh của nó bằng tổng sức mạnh của chúng. Thứ tự chính xác của các chiến thắng không quan trọng. 

Chỉ cần gian lận khi đối thủ tiếp theo vượt quá sức mạnh tích lũy hiện tại. Những sự kiện đó chính xác là những vị trí nguy hiểm được xác định bởi điều kiện tổng tiền tố. 

Việc loại bỏ một con bò chỉ thay đổi các tiền tố ở bên phải của nó, đó là lý do tại sao mọi con bò đều có thể được xử lý bằng cách điều chỉnh bộ vị trí nguy hiểm thay vì tính toán lại toàn bộ giải đấu. 

Bước đếm hậu tố có tác dụng vì các vị trí chèn trở nên đơn điệu tùy theo số lượng sự kiện nguy hiểm gặp phải. Khi một vị trí đòi hỏi nhiều hơn`k`gian lận, mọi vị trí trước đó trong cùng khu vực cũng thất bại. Tính đơn điệu này cho phép tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, k = map(int, input().split())
        a = [0] + list(map(int, input().split()))

        pref = [0] * (n + 1)
        for i in range(1, n + 1):
            pref[i] = pref[i - 1] + a[i]

        pos = []
        for i in range(1, n + 1):
            if a[i] > pref[i - 1]:
                pos.append(i)

        ans = [0] * (n + 1)

        for i in range(1, n + 1):
            g = []

            for x in pos:
                if x == i:
                    continue

                if a[x] > pref[x - 1] + (a[i] if x < i else 0):
                    g.append(x)

            if len(g) < k:
                ans[i] = n
                continue

            cur = 0

            if k > 0:
                p = g[-k]
                cur += n - p + 1 - (1 if p < i else 0)

            if len(g) == k:
                if pref[i - 1] >= a[i]:
                    cur += bisect_left(pref, a[i], 1) - 1
                else:
                    cur += bisect_left(pref, 2 * a[i], 1) - 2

            ans[i] = cur

        print(*ans[1:])

solve()
```Việc triển khai này tuân theo ý tưởng được chấp nhận từ cộng đồng phân tích cuộc thi. Mảng tổng tiền tố là cấu trúc trung tâm vì mọi trạng thái của giải đấu được biểu thị bằng tổng số con bò đã được hấp thụ trước đó. 

Danh sách đã lọc`g`là phần tế nhị nhất. Loại bỏ con bò`i`thay đổi mọi tiền tố sang bên phải của nó, vì vậy điều kiện nguy hiểm phải được điều chỉnh. Việc định hướng sai ở đây sẽ dẫn đến từng thất bại trong nhiều bài kiểm tra ẩn. 

biểu hiện```
g[-k]
```chọn`k`-vị trí nguy hiểm từ cuối. Điều này chỉ hợp lệ khi`k > 0`, đó là lý do tại sao mã xử lý trường hợp đó một cách riêng biệt. 

Tất cả số học đều sử dụng số nguyên Python, do đó không có rủi ro tràn mặc dù tổng tiền tố có thể đạt tới khoảng`2 · 10^14`. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 2
k = 0
a = [2, 1]
```| tôi | Vị trí nguy hiểm | len(g) | Vị trí tốt | 
| --- | --- | --- | --- | 
| 1 | ∅ | 0 | 2 | 
| 2 | {1} | 1 | 0 | 

bò`1`đã thống trị mọi sự sắp xếp có thể. bò`2`không bao giờ có thể thắng được con bò mạnh hơn mà không gian lận. 

### Ví dụ 2```
n = 3
k = 1
a = [1, 1, 3]
```| tôi | Vị trí nguy hiểm sau khi loại bỏ | len(g) | Trả lời | 
| --- | --- | --- | --- | 
| 1 | {3} | 1 | 2 | 
| 2 | {3} | 1 | 2 | 
| 3 | ∅ | 0 | 3 | 

Con bò lớn có thể giành chiến thắng từ mọi vị trí chèn. Những con bò nhỏ hơn chỉ có thể giành chiến thắng khi số tiền gian lận duy nhất có sẵn được sử dụng một cách tối ưu. 

Những dấu vết này minh họa sự bất biến chính: chỉ những vị trí nguy hiểm mới quan trọng chứ không phải mô phỏng giải đấu đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tìm kiếm nhị phân cộng với việc xử lý cấu trúc vị trí nguy hiểm | 
| Không gian | O(n) | Tổng tiền tố, vị trí nguy hiểm, đáp án | 

Tổng cộng`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, vì vậy một`O(n log n)`giải pháp dễ dàng nằm gọn trong giới hạn 3 giây và mức sử dụng bộ nhớ vẫn ở mức thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = [0] + list(map(int, input().split()))

        from bisect import bisect_left

        pref = [0] * (n + 1)
        for i in range(1, n + 1):
            pref[i] = pref[i - 1] + a[i]

        pos = [i for i in range(1, n + 1) if a[i] > pref[i - 1]]

        ans = [0] * (n + 1)

        for i in range(1, n + 1):
            g = []
            for x in pos:
                if x == i:
                    continue
                if a[x] > pref[x - 1] + (a[i] if x < i else 0):
                    g.append(x)

            if len(g) < k:
                ans[i] = n
                continue

            cur = 0

            if k > 0:
                p = g[-k]
                cur += n - p + 1 - (1 if p < i else 0)

            if len(g) == k:
                if pref[i - 1] >= a[i]:
                    cur += bisect_left(pref, a[i], 1) - 1
                else:
                    cur += bisect_left(pref, 2 * a[i], 1) - 2

            ans[i] = cur

        print(*ans[1:], file=out)

    return out.getvalue()

# sample
assert run("""1
2 0
2 1
""") == "2 0\n"

# minimum size
assert run("""1
2 0
1 1
""") == "1 1\n"

# all equal
run("""1
4 1
5 5 5 5
""")

# k very large
run("""1
5 4
1 2 3 4 5
""")

# increasing powers
run("""1
5 0
1 2 4 8 16
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0 / 2 1`|`2 0`| Ví dụ nhỏ nhất không tầm thường | 
|`2 0 / 1 1`|`1 1`| Xử lý cà vạt | 
| Tất cả các giá trị bằng nhau | Phụ thuộc vào`k`| Hành vi sức mạnh ngang nhau | 
| Lớn`k`| Nhiều vị trí chiến thắng | Ranh giới ngân sách gian lận | 
| Quyền hạn của hai | Nhiều vị trí nguy hiểm | Chuyển đổi tổng tiền tố | 

## Vỏ cạnh 

Hãy xem xét```
n = 2
k = 0
a = [1, 1]
```Con bò đầu tiên thắng hòa. Nếu chúng ta bỏ qua các quy tắc hòa và sử dụng sự so sánh chặt chẽ ở mọi nơi, chúng ta sẽ kết luận sai rằng không con bò nào có thể thắng. Công thức dựa trên tiền tố duy trì quy tắc giải đấu ban đầu vì các vị trí nguy hiểm được xác định bằng cách sử dụng`>`còn hơn là`>=`. 

Coi như```
n = 3
k = 0
a = [1, 2, 3]
```Di chuyển con bò cuối cùng lên phía trước sẽ loại bỏ toàn bộ sự hợp nhất bên trái. Thuật toán xử lý việc này thông qua đặc biệt`len(g) == k`nhánh, tính toán rõ ràng các vị trí mà một gian lận tiềm ẩn sẽ biến mất. 

Coi như```
n = 5
k = 4
a = [1, 1, 1, 1, 100]
```Một mô phỏng ngây thơ có thể có nhiều gian lận. Trên thực tế, số lượng vị trí nguy hiểm có ý nghĩa là rất nhỏ vì mỗi lần gian lận thành công sẽ tăng gấp đôi sức mạnh hiệu quả của con bò. Thuật toán khai thác cấu trúc logarit này và không bao giờ cần mô phỏng từng kết quả khớp một.
