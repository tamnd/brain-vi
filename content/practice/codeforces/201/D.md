---
title: "CF 201D - Vấn đề hoàn toàn mới"
description: "Lesha có một mô tả vấn đề gồm n từ riêng biệt, được viết theo một thứ tự cố định. Mỗi vấn đề lưu trữ là một chuỗi từ khác, nhưng các mô tả lưu trữ có thể lặp lại các từ nhiều lần. Chúng tôi muốn so sánh mô tả của Lesha với mọi mô tả lưu trữ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp"]
categories: ["algorithms"]
codeforces_contest: 201
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 127 (Div. 1)"
rating: 2600
weight: 201
solve_time_s: 99
verified: true
draft: false
---

[CF 201D - Sự cố hoàn toàn mới](https://codeforces.com/problemset/problem/201/D) 

**Đánh giá:** 2600 
**Tags:** bitmasks, vũ phu, dp 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lesha có một mô tả vấn đề được làm bằng`n`từ riêng biệt, được viết theo một thứ tự cố định. Mỗi vấn đề lưu trữ là một chuỗi từ khác, nhưng các mô tả lưu trữ có thể lặp lại các từ nhiều lần. 

Chúng tôi muốn so sánh mô tả của Lesha với mọi mô tả lưu trữ. Đối với một vấn đề về lưu trữ, chúng tôi được phép hoán vị các từ của Lesha theo bất kỳ thứ tự nào. Nếu hoán vị đó xuất hiện dưới dạng một chuỗi con bên trong mô tả kho lưu trữ thì vấn đề lưu trữ được coi là tương tự. 

Trong số tất cả các hoán vị hợp lệ, chúng tôi chọn hoán vị có số lần đảo ngược nhỏ nhất so với thứ tự ban đầu trong các từ của Lesha. Chúng ta càng cần ít sự đảo ngược hơn thì các vấn đề càng giống nhau hơn. 

Nếu hoán vị tốt nhất có`x`nghịch đảo thì điểm tương đồng là:$$p = \frac{n(n-1)}{2} + 1 - x$$Số lần đảo ngược tối đa có thể là`n(n-1)/2`, do đó độ tương tự lớn hơn có nghĩa là ít nghịch đảo hơn. 

Nhiệm vụ là tìm ra bài toán lưu trữ có độ tương đồng lớn nhất. Nếu không có vấn đề lưu trữ nào chứa bất kỳ hoán vị nào của các từ của Lesha dưới dạng một chuỗi con, chúng tôi sẽ in`"Brand new problem!"`. 

Các ràng buộc hoàn toàn định hình giải pháp. 

Giới hạn quan trọng là`n ≤ 15`. Điều đó ngay lập tức gợi ý bitmask DP, bởi vì`2^15 = 32768`có thể quản lý được. Đồng thời, mô tả lưu trữ có thể chứa tới`500000`tổng số từ, do đó, bất kỳ giải pháp cấp số nhân nào về chiều dài lưu trữ đều không thể thực hiện được. 

Một lực lượng mạnh mẽ đối với tất cả các hoán vị sẽ yêu cầu kiểm tra tới`15!`hoán vị, có giá trị lớn về mặt thiên văn. Thậm chí`10!`đã quá lớn rồi. 

cái nhỏ`n`và kích thước kho lưu trữ khổng lồ cho thấy rằng kho lưu trữ nên được xử lý gần như tuyến tính, trong khi các tổ hợp tốn kém chỉ xảy ra trên`n`Lời của Lesha. 

Có một số trường hợp tế nhị phá vỡ các giải pháp ngây thơ. 

Giả sử những lời của Lesha là:```
a b c
```và mô tả lưu trữ là:```
c b a
```Hoán vị hợp lệ duy nhất là chính xác`c b a`, có 3 nghịch đảo. Một cách tiếp cận tham lam cố gắng duy trì trật tự ban đầu sẽ không tìm ra giải pháp nào, ngay cả khi giải pháp đó tồn tại. 

Một trường hợp nguy hiểm khác là lặp lại các từ lưu trữ:```
Lesha: a b
Archive: a a a b
```Hoán vị tốt nhất vẫn là`a b`với 0 nghịch đảo. Nếu chúng ta chỉ lưu trữ lần xuất hiện đầu tiên của mỗi từ và bỏ qua các vị trí sau đó, chúng ta có thể vô tình làm mất các chuyển tiếp hợp lệ. 

Một vấn đề tinh vi hơn xuất hiện khi có thể có nhiều hoán vị:```
Lesha: a b c
Archive: b a c
```Cả hai hoán vị`b a c`Và`a c b`là những dãy số. Số lần đảo ngược của chúng là khác nhau:```
b a c -> 1 inversion
a c b -> 1 inversion
```Một DP bất cẩn chỉ kiểm tra sự tồn tại sẽ bỏ lỡ nhu cầu giảm thiểu sự đảo ngược. 

Cuối cùng, vấn đề hòa giải. Nếu hai bài toán lưu trữ đạt được độ tương tự như nhau thì chúng ta phải xuất ra chỉ mục lưu trữ nhỏ nhất. 

## Phương pháp tiếp cận 

Giải pháp vũ phu trực tiếp nhất là tạo ra mọi hoán vị trong lời nói của Lesha. Đối với mỗi hoán vị, chúng tôi kiểm tra xem nó có xuất hiện dưới dạng một chuỗi con bên trong mô tả kho lưu trữ hay không. Nếu đúng như vậy, chúng tôi sẽ tính toán số lần đảo ngược của nó và giữ mức tối thiểu. 

Việc kiểm tra sự tồn tại của dãy con cần có thời gian tuyến tính theo chiều dài kho lưu trữ. Có tới`n!`hoán vị. 

Vì`n = 15`:$$15! \approx 1.3 \times 10^{12}$$Ngay cả trước khi xem xét việc quét kho lưu trữ, điều này là vô vọng. 

Brute-force hoạt động về mặt khái niệm vì bài toán thực sự yêu cầu chúng ta tìm kiếm các hoán vị. Vấn đề là các hoán vị là cách biểu diễn sai. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến hoán vị đầy đủ. Chúng tôi chỉ quan tâm đến việc xây dựng một chuỗi con theo thứ tự lưu trữ trong khi giảm thiểu sự đảo ngược. 

Giả sử chúng ta quét mô tả kho lưu trữ từ trái sang phải. Bất cứ khi nào chúng tôi gặp một trong những từ của Lesha, chúng tôi có thể quyết định thêm nó vào chuỗi đã chọn của mình. 

Thứ tự các từ được thêm vào sẽ trở thành hoán vị. 

Bây giờ hãy nghĩ về số lượng đảo ngược tăng dần. 

Nếu chúng ta thêm từ`i`sau khi đã chọn một số tập hợp từ, thì mọi từ được chọn trước đó có chỉ mục lớn hơn`i`tạo ra một sự đảo ngược. 

Điều đó có nghĩa là chi phí đảo ngược bổ sung chỉ phụ thuộc vào mặt nạ tập hợp con hiện tại. 

Đây chính xác là cấu trúc cần thiết cho tập hợp con DP. 

Chúng tôi xác định:`dp[mask] = minimum inversions needed to build exactly this set of chosen words as a subsequence so far`Khi xử lý một từ lưu trữ mới tương ứng với chỉ mục Lesha`i`, chúng ta có thể chuyển đổi:```
mask -> mask | (1 << i)
```Số lượng đảo ngược được thêm vào bằng với số chỉ số đã được chọn lớn hơn`i`. 

Kho lưu trữ được xử lý từ trái sang phải, do đó thứ tự tiếp theo sẽ tự động được tôn trọng. 

Điều này chuyển đổi tìm kiếm giai thừa thành DP qua`2^n`mặt nạ. 

Tổng số mặt nạ nhiều nhất là`32768`, đủ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · k · n!) | O(n) | Quá chậm | 
| Tối ưu | O(total_words · 2^n) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các từ của Lesha và gán cho mỗi từ chỉ mục gốc của nó`0`ĐẾN`n-1`. 
2. Đối với mọi vấn đề về lưu trữ, hãy khởi tạo:```
dp[mask] = minimum inversions
```Đặt tất cả các trạng thái thành vô cùng ngoại trừ:```
dp[0] = 0
```Điều này thể hiện việc không chọn từ nào có độ đảo ngược bằng 0. 

1. Xử lý mô tả lưu trữ từ trái sang phải. 

Chỉ những từ xuất hiện trong mô tả của Lesha mới quan trọng. Tất cả các từ khác có thể được bỏ qua ngay lập tức. 

1. Giả sử từ lưu trữ hiện tại tương ứng với chỉ mục Lesha`i`. 

Chúng tôi cố gắng thêm từ này vào mọi tập hợp con hiện có chưa chứa`i`. 

1. Đối với mỗi mặt nạ hợp lệ, hãy tính chi phí đảo ngược bổ sung. 

Nếu chúng ta nối thêm`i`, thì mọi chỉ mục đã được chọn`j > i`đóng góp một sự đảo ngược. 

Vì thế:```
extra = count of bits j in mask where j > i
```1. Chuyển tiếp:```
new_mask = mask | (1 << i)

dp[new_mask] = min(
    dp[new_mask],
    dp[mask] + extra
)
```Chúng tôi lặp lại các mặt nạ theo thứ tự giảm dần để cùng một lần lưu trữ không được sử dụng lại nhiều lần. 

1. Sau khi xử lý toàn bộ mô tả kho lưu trữ, hãy kiểm tra mặt nạ đầy đủ:```
full = (1 << n) - 1
```Nếu như`dp[full]`là hữu hạn thì bài toán lưu trữ này chứa một số hoán vị hợp lệ. 

1. Chuyển số đảo ngược thành số tương tự:$$p = \frac{n(n-1)}{2} + 1 - dp[full]$$Giữ vấn đề lưu trữ ở mức tối đa`p`. Phá vỡ mối quan hệ bằng chỉ số nhỏ hơn. 

1. Nếu không có vấn đề lưu trữ nào đạt đến mặt nạ đầy đủ, hãy in:```
Brand new problem!
```Nếu không hãy in chỉ mục lưu trữ tốt nhất và:```
[:||||...||:]
```với chính xác`p`thanh dọc. 

### Tại sao nó hoạt động 

Bất biến DP là:```
dp[mask] = minimum inversions among all subsequences
formed so far whose chosen Lesha words are exactly mask
```Khi xử lý các từ lưu trữ từ trái sang phải, mỗi lần chuyển đổi sẽ thêm một từ mới vào cuối chuỗi tiếp theo. Điều đó tự động bảo toàn tính hợp lệ của dãy số sau. 

Sự đóng góp đảo ngược của việc nối thêm từ`i`chỉ phụ thuộc vào chỉ số nào lớn hơn đã được chọn. Mỗi cặp như vậy đóng góp chính xác một phép đảo ngược và mỗi lần đảo ngược được tính chính xác một lần, khi từ có chỉ số nhỏ hơn được thêm vào sau từ có chỉ mục lớn hơn. 

Vì mọi hoán vị hợp lệ tương ứng với một số chuỗi chuyển đổi DP và mỗi chuyển đổi DP tạo nên một hoán vị hợp lệ, nên DP khám phá chính xác không gian tìm kiếm cần thiết trong khi giảm thiểu các đảo ngược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**9

def solve():
    n = int(input())
    words = input().split()

    pos = {}
    for i, w in enumerate(words):
        pos[w] = i

    m = int(input())

    full = (1 << n) - 1
    max_inv = n * (n - 1) // 2

    best_similarity = -1
    best_index = -1

    greater_bits = [0] * n
    for i in range(n):
        mask = 0
        for j in range(i + 1, n):
            mask |= (1 << j)
        greater_bits[i] = mask

    for problem_index in range(1, m + 1):
        data = input().split()
        k = int(data[0])
        archive = data[1:]

        dp = [INF] * (1 << n)
        dp[0] = 0

        for w in archive:
            if w not in pos:
                continue

            i = pos[w]
            bit = 1 << i

            for mask in range(full, -1, -1):
                if dp[mask] == INF:
                    continue

                if mask & bit:
                    continue

                new_mask = mask | bit

                extra = (mask & greater_bits[i]).bit_count()

                cand = dp[mask] + extra

                if cand < dp[new_mask]:
                    dp[new_mask] = cand

        if dp[full] == INF:
            continue

        inversions = dp[full]
        similarity = max_inv + 1 - inversions

        if similarity > best_similarity:
            best_similarity = similarity
            best_index = problem_index

    if best_similarity == -1:
        print("Brand new problem!")
    else:
        print(best_index)
        print("[:{}:]".format("|" * best_similarity))

solve()
```Từ điển`pos`ánh xạ mọi từ Lesha vào chỉ mục ban đầu của nó. Vì các từ của Lesha được đảm bảo rõ ràng nên sự ánh xạ này không rõ ràng. 

Mảng DP có kích thước`2^n`, điều này khả thi vì`n ≤ 15`. 

Mảng`greater_bits[i]`lưu trữ một bitmask chứa tất cả các chỉ số lớn hơn`i`. Điều này cho phép chúng tôi tính toán số lần đảo ngược mới được tạo trong thời gian không đổi bằng cách sử dụng:```
(mask & greater_bits[i]).bit_count()
```Biểu thức đó đếm số lượng từ đã được chọn ban đầu xuất hiện sau`i`. 

Việc lặp lại mặt nạ giảm dần là điều cần thiết. Nếu không có nó, một lần lưu trữ có thể được sử dụng lại nhiều lần trong cùng một lần lặp, điều này sẽ vi phạm các quy tắc tuần tự tiếp theo. 

Một điểm tinh tế khác là vấn đề lưu trữ có thể lặp lại các từ nhiều lần. Chúng tôi phải xử lý từng lần xuất hiện một cách riêng biệt vì những lần xuất hiện sau có thể tạo ra các chuỗi tiếp theo tốt hơn. 

Công thức tương tự cuối cùng được đưa ra trực tiếp từ câu lệnh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
find the next palindrome
1
10 find the previous palindrome or print better luck next time
```Các chỉ số là: 

| Lời | Chỉ mục | 
| --- | --- | 
| tìm | 0 | 
| | 1 | 
| tiếp theo | 2 | 
| bảng màu | 3 | 

Trình tự liên quan đến kho lưu trữ trở thành:```
find the palindrome next
```Chuyển tiếp DP: 

| Từ hiện tại | Hoán vị được xây dựng | Đảo ngược | 
| --- | --- | --- | 
| tìm | tìm | 0 | 
| | tìm | 0 | 
| bảng màu | tìm bảng màu | 0 | 
| tiếp theo | tìm bảng màu tiếp theo | 1 | 

Hoán vị cuối cùng là:```
find the palindrome next
```So với thứ tự ban đầu:```
find the next palindrome
```chỉ một`(palindrome, next)`bị đảo ngược. 

Vì`n = 4`:$$\frac{4 \cdot 3}{2} = 6$$Sự tương đồng:$$6 + 1 - 1 = 6$$Đầu ra:```
1
[:||||||:]
```Dấu vết này cho thấy cách đảo ngược được tích lũy tăng dần trong khi vẫn giữ nguyên thứ tự tiếp theo. 

### Ví dụ 2 

đầu vào:```
3
a b c
1
3 c b a
```Trình tự lưu trữ có liên quan:```
c b a
```Tiến hóa DP: 

| Từ hiện tại | Hoán vị tốt nhất cho đến nay | Đảo ngược | 
| --- | --- | --- | 
| c | c | 0 | 
| b | c b | 1 | 
| một | cb a | 3 | 

Hoán vị`c b a`có cả ba cặp đảo ngược:```
(c,b), (c,a), (b,a)
```Đảo ngược tối đa cho`n = 3`cũng là 3, vì vậy:$$3 + 1 - 3 = 1$$Đầu ra:```
1
[:|:]
```Ví dụ này cho thấy thuật toán xử lý chính xác các thứ tự đảo ngược hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(total_words · 2^n) | Mỗi từ lưu trữ có liên quan lặp lại trên tất cả các mặt nạ tập hợp con | 
| Không gian | O(2^n) | Mảng DP trên các tập hợp con | 

DP lớn nhất có thể có`32768`tiểu bang. Tổng chiều dài lưu trữ là khoảng`5 × 10^5`, giúp giữ tổng số thao tác trong giới hạn thực tế trong Python được tối ưu hóa. 

Việc sử dụng bộ nhớ rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

INF = 10**9

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    n = int(input())
    words = input().split()

    pos = {}
    for i, w in enumerate(words):
        pos[w] = i

    m = int(input())

    full = (1 << n) - 1
    max_inv = n * (n - 1) // 2

    best_similarity = -1
    best_index = -1

    greater_bits = [0] * n
    for i in range(n):
        mask = 0
        for j in range(i + 1, n):
            mask |= (1 << j)
        greater_bits[i] = mask

    for problem_index in range(1, m + 1):
        data = input().split()
        archive = data[1:]

        dp = [INF] * (1 << n)
        dp[0] = 0

        for w in archive:
            if w not in pos:
                continue

            i = pos[w]
            bit = 1 << i

            for mask in range(full, -1, -1):
                if dp[mask] == INF:
                    continue

                if mask & bit:
                    continue

                new_mask = mask | bit

                extra = (mask & greater_bits[i]).bit_count()

                dp[new_mask] = min(
                    dp[new_mask],
                    dp[mask] + extra
                )

        if dp[full] == INF:
            continue

        similarity = max_inv + 1 - dp[full]

        if similarity > best_similarity:
            best_similarity = similarity
            best_index = problem_index

    if best_similarity == -1:
        print("Brand new problem!")
    else:
        print(best_index)
        print("[:{}:]".format("|" * best_similarity))

    return out.getvalue()

# provided sample
assert run(
"""4
find the next palindrome
1
10 find the previous palindrome or print better luck next time
"""
) == "1\n[:||||||:]\n", "sample 1"

# minimum size
assert run(
"""1
hello
1
1 hello
"""
) == "1\n[:|:]\n", "single word"

# no valid permutation
assert run(
"""2
a b
1
3 a x x
"""
) == "Brand new problem!\n", "missing word"

# reversed order
assert run(
"""3
a b c
1
3 c b a
"""
) == "1\n[:|:]\n", "maximum inversions"

# tie-breaking by smaller index
assert run(
"""2
a b
2
2 a b
2 a b
"""
) == "1\n[:||:]\n", "smallest index tie"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp một từ | Điểm tương đồng 1 | Hạn chế tối thiểu | 
| Thiếu từ | Vấn đề hoàn toàn mới | Mặt nạ đầy đủ không thể truy cập | 
| Thứ tự đảo ngược | Độ tương tự thấp nhất | Xử lý đảo ngược tối đa | 
| Lưu trữ giống hệt nhau tốt nhất | Chỉ số nhỏ nhất được chọn | Sự đúng đắn của sự ràng buộc | 

## Vỏ cạnh 

Hãy xem xét:```
2
a b
1
3 a a b
```Kho lưu trữ lặp lại`a`nhiều lần. 

DP xử lý từng lần xuất hiện một cách độc lập. Sau khi đọc phần đầu tiên`a`, chúng ta có thể tạo thành tập hợp con`{a}`. Càng về sau`a`các lần xuất hiện bị bỏ qua đối với tập hợp con đó vì bit đã được đặt. 

Khi`b`đến, DP đạt đến mặt nạ đầy đủ mà không có sự đảo ngược nào. 

Đầu ra:```
1
[:||:]
```Điều này xác nhận các từ lưu trữ lặp đi lặp lại được xử lý an toàn. 

Bây giờ hãy xem xét trường hợp đảo ngược hoàn toàn:```
3
a b c
1
3 c b a
```Chuyển tiếp:```
{} -> {c} -> {c,b} -> {c,b,a}
```Khi nối thêm`b`, một chỉ số lớn hơn (`c`) đã tồn tại, thêm một phép đảo ngược. 

Khi nối thêm`a`, hai chỉ số lớn hơn đã tồn tại, thêm hai chỉ số đảo ngược nữa. 

Tổng số nghịch đảo:```
1 + 2 = 3
```cái nào đúng. 

Cuối cùng, hãy xem xét một trường hợp không có dãy con hợp lệ:```
3
a b c
1
4 a x y b
```DP không bao giờ chạm tới mặt nạ đầy đủ vì`c`không bao giờ xuất hiện. 

Trạng thái cuối cùng vẫn là vô hạn:```
dp[111] = INF
```vì vậy thuật toán in chính xác:```
Brand new problem!
```
