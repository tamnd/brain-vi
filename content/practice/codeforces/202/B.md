---
title: "CF 202B - Bài toán dễ dàng hoàn toàn mới"
description: "Chúng ta được cung cấp các tiêu đề về bài toán mới của Lesha và một số bài toán lưu trữ từ Torcode. Các từ trong tiêu đề của Lesha đều khác biệt. Tiêu đề kho lưu trữ có thể lặp lại các từ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 202
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 127 (Div. 2)"
rating: 1700
weight: 202
solve_time_s: 209
verified: true
draft: false
---

[CF 202B - Bài toán dễ, hoàn toàn mới](https://codeforces.com/problemset/problem/202/B) 

**Đánh giá:** 1700 
**Tags:** vũ lực 
**Thời gian giải:** 3m 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp các tiêu đề về bài toán mới của Lesha và một số bài toán lưu trữ từ Torcode. Các từ trong tiêu đề của Lesha đều khác biệt. Tiêu đề kho lưu trữ có thể lặp lại các từ. 

Đối với mọi vấn đề về lưu trữ, chúng tôi muốn biết liệu chúng tôi có thể tìm thấy tất cả các từ trong tiêu đề của Lesha bên trong nó dưới dạng một dãy con hay không, có thể theo một thứ tự khác. Trong số tất cả các đơn hàng hợp lệ, chúng tôi chọn đơn hàng có số lần đảo ngược tối thiểu so với thứ tự ban đầu của danh hiệu Lesha. 

Đảo ngược có nghĩa là hai từ được hoán đổi so với thứ tự ban đầu. Nếu tiêu đề của Lesha là:```
find the next palindrome
```thì hoán vị:```
find the palindrome next
```có một sự đảo ngược vì`palindrome`đã chuyển đi trước đó`next`. 

Nếu hoán vị tốt nhất cho một số vấn đề lưu trữ có`x`đảo ngược và tiêu đề của Lesha có`n`từ, điểm tương đồng là:```
p = n * (n - 1) / 2 - x
```Giá trị tối đa có thể xuất hiện khi không có sự đảo ngược nào, nghĩa là kho lưu trữ đã chứa các từ theo thứ tự ban đầu. 

Chúng ta phải tìm ra vấn đề lưu trữ có độ tương đồng lớn nhất. Nếu không có vấn đề lưu trữ nào chứa bất kỳ hoán vị nào của tất cả các từ, chúng tôi sẽ in`"Brand new problem!"`. 

Các ràng buộc hoàn toàn định hình giải pháp. Tiêu đề của Lesha chứa tối đa 4 từ. Đó là nhỏ bé. Số hoán vị nhiều nhất là:```
4! = 24
```Mỗi mô tả kho lưu trữ chứa tối đa 20 từ và có tối đa 10 vấn đề về lưu trữ. Ở đây ngay cả việc kiểm tra mạnh mẽ tất cả các hoán vị cũng không đáng kể. 

Phần nguy hiểm không phải là hiệu suất mà là tính chính xác. 

Một sai lầm dễ mắc phải là quên rằng kho lưu trữ có thể chứa các từ lặp lại. Giả sử tiêu đề của Lesha là:```
a b
```và tiêu đề lưu trữ là:```
a a a
```từ`b`không bao giờ xuất hiện, vì vậy không có hoán vị nào hoạt động. Một trình so khớp chuỗi con bất cẩn chỉ kiểm tra độ dài hoặc kết quả khớp một phần sẽ chấp nhận nó một cách không chính xác. 

Một trường hợp tinh vi khác là chọn sai hoán vị khi có nhiều hoán vị xuất hiện dưới dạng dãy con. Chúng ta phải giảm thiểu sự đảo ngược chứ không phải tối đa hóa tính linh hoạt của chuỗi con. 

Ví dụ:```
Lesha: a b c
Archive: b a c
```Cả hai hoán vị`b a c`Và`a c b`là những dãy số. Số lần đảo ngược của chúng là khác nhau: 

| Hoán vị | Đảo ngược | 
| --- | --- | 
| b a c | 1 | 
| a c b | 1 | 

Ở đây chúng bằng nhau, nhưng trong những trường hợp lớn hơn thì mức tối thiểu là quan trọng vì sự giống nhau phụ thuộc trực tiếp vào nghịch đảo. 

Một lỗi phổ biến khác là xử lý sai mối quan hệ giữa các vấn đề lưu trữ. Nếu hai bài toán đạt được sự tương đồng giống nhau thì chúng ta phải đưa ra chỉ số nhỏ nhất. 

Ví dụ:```
Lesha: a b
Archive 1: b a
Archive 2: b a
```Cả hai đều có số điểm bằng nhau nên đáp án phải được lưu trữ`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tạo ra mọi hoán vị trong lời nói của Lesha. Từ`n ≤ 4`, có nhiều nhất 24 hoán vị. Đối với mọi vấn đề về lưu trữ, chúng tôi kiểm tra hoán vị nào xuất hiện dưới dạng chuỗi con. Trong số đó, chúng tôi tính toán số lần đảo ngược và giữ mức tối thiểu. 

Việc kiểm tra xem một chuỗi có phải là một chuỗi con hay không có tuyến tính theo độ dài lưu trữ hay không. Kho lưu trữ chứa tối đa 20 từ, do đó, một lần kiểm tra sẽ tốn tối đa 20 từ so sánh. 

Tổng công việc là rất nhỏ:```
10 archive problems × 24 permutations × 20 checks
```Đó chỉ là vài ngàn hoạt động. 

Một lực lượng vũ phu ngây thơ hơn sẽ thử mọi tập hợp con các vị trí lưu trữ và so sánh các chuỗi kết quả. Đối với một kho lưu trữ có độ dài 20 và`n = 4`, điều đó có nghĩa là kiểm tra:```
C(20,4) = 4845
```kết hợp vị trí cho mỗi vấn đề lưu trữ. Ngay cả điều đó vẫn dễ dàng phù hợp, nhưng nó làm phức tạp việc thực hiện vì chúng ta sẽ cần phải xây dựng lại các hoán vị từ các vị trí đã chọn. 

Quan sát quan trọng là`n`là cực kỳ nhỏ. Vì số thứ tự có thể bị giới hạn bởi 24, nên chúng ta có thể trực tiếp liệt kê tất cả các hoán vị ứng cử viên và kiểm tra chúng một cách độc lập. Vấn đề trở thành sự kết hợp rõ ràng giữa việc tạo hoán vị, so khớp chuỗi con và đếm nghịch đảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tập hợp con vị trí lưu trữ | O(m × C(k,n) × n) | O(n) | Đã chấp nhận | 
| Liệt kê tất cả các hoán vị | O(m × n! × k) | Ồ (n!) | Đã chấp nhận | 

Đây`k ≤ 20`Và`n ≤ 4`. 

## Hướng dẫn thuật toán 

1. Đọc các từ của Lesha thành một mảng`base`. 
2. Tạo tất cả các hoán vị của`base`. 

Từ`n ≤ 4`, điều này tạo ra nhiều nhất là 24 chuỗi. 
3. Với mỗi hoán vị, hãy tính số lần đảo ngược của nó so với thứ tự ban đầu. 

Chúng tôi ánh xạ mọi từ gốc vào chỉ mục của nó. Sau đó với mỗi cặp`(i,j)`với`i < j`, chúng tôi tính một sự đảo ngược nếu vị trí ban đầu của`perm[i]`lớn hơn vị trí ban đầu của`perm[j]`. 
4. Xử lý mọi vấn đề lưu trữ một cách độc lập. 
5. Đối với mỗi hoán vị, hãy kiểm tra xem nó có xuất hiện dưới dạng dãy con bên trong tiêu đề kho lưu trữ hay không. 

Sử dụng hai con trỏ: 

- một con trỏ quét hoán vị 
- một con trỏ quét các từ lưu trữ 

Bất cứ khi nào các từ khớp nhau, hãy tiến con trỏ hoán vị. 

Nếu con trỏ hoán vị đến cuối thì hoán vị là một dãy con. 
6. Trong số tất cả các chuỗi con được tìm thấy trong bài toán lưu trữ hiện tại, hãy giữ số lượng đảo ngược tối thiểu. 

Đảo ngược tối thiểu có nghĩa là độ tương tự tối đa. 
7. Chuyển đổi số lượng đảo ngược tối thiểu thành độ tương tự:```
p = n * (n - 1) / 2 - inversions
```1. Theo dõi vấn đề lưu trữ có độ tương đồng cao nhất. 

Nếu có sự tương đồng, hãy giữ chỉ mục lưu trữ nhỏ hơn. 
2. Nếu không có vấn đề lưu trữ nào khớp với bất kỳ hoán vị nào, hãy in:```
Brand new problem!
```1. Mặt khác, in chỉ mục lưu trữ tốt nhất và xây dựng thanh tương tự:```
[:||||...||:]
```với chính xác`p`thanh dọc. 

### Tại sao nó hoạt động 

Mọi giải pháp hợp lệ đều tương ứng với một số hoán vị trong các từ của Lesha. Vì chúng ta liệt kê tất cả các hoán vị nên chúng ta không bao giờ bỏ sót một ứng cử viên nào. 

Việc kiểm tra trình tự sau đó là chính xác. Một hoán vị được chấp nhận khi và chỉ khi các từ của nó có thể khớp theo thứ tự bên trong tiêu đề kho lưu trữ. 

Trong số tất cả các hoán vị được chấp nhận, định nghĩa bài toán yêu cầu rõ ràng hoán vị có độ nghịch đảo tối thiểu. Chúng tôi tính toán số lần đảo ngược cho mọi hoán vị hợp lệ và giữ mức tối thiểu để hoán vị được chọn là chính xác. 

Cuối cùng, sự tương đồng là một phép biến đổi đơn điệu của các phép nghịch đảo. Giảm thiểu sự đảo ngược tương đương với tối đa hóa sự tương đồng. 

## Giải pháp Python```python
import sys
from itertools import permutations

input = sys.stdin.readline

def is_subsequence(perm, archive):
    j = 0

    for word in archive:
        if j < len(perm) and word == perm[j]:
            j += 1

    return j == len(perm)

def solve():
    n = int(input())
    base = input().split()

    pos = {word: i for i, word in enumerate(base)}

    perms = []

    for perm in permutations(base):
        inv = 0

        for i in range(n):
            for j in range(i + 1, n):
                if pos[perm[i]] > pos[perm[j]]:
                    inv += 1

        perms.append((perm, inv))

    m = int(input())

    best_similarity = -1
    best_index = -1

    max_pairs = n * (n - 1) // 2

    for idx in range(1, m + 1):
        parts = input().split()

        k = int(parts[0])
        archive = parts[1:]

        best_inv = None

        for perm, inv in perms:
            if is_subsequence(perm, archive):
                if best_inv is None or inv < best_inv:
                    best_inv = inv

        if best_inv is not None:
            similarity = max_pairs - best_inv

            if similarity > best_similarity:
                best_similarity = similarity
                best_index = idx

    if best_index == -1:
        print("Brand new problem!")
    else:
        print(best_index)
        print("[:{}:]".format('|' * best_similarity))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tạo ra mỗi hoán vị một lần. Vì số lượng từ rất nhỏ nên việc tính toán trước tất cả các hoán vị và số lần đảo ngược của chúng sẽ đơn giản hóa logic sau này. 

Tính toán đảo ngược sử dụng ánh xạ thứ tự ban đầu được lưu trữ trong`pos`. Điều này tránh việc tìm kiếm nhiều lần các chỉ mục bên trong mảng. 

Trình so khớp chuỗi con sử dụng phương pháp quét tham lam tiêu chuẩn. Tính tham lam có tác dụng vì việc so sánh các lần xuất hiện trước đó không bao giờ có thể làm cho một chuỗi con không thể xảy ra sau đó. 

Một chi tiết triển khai thường gây ra lỗi là tính toán tương tự một cách chính xác. Công thức sử dụng:```
n * (n - 1) / 2
```đó là số lượng cặp có thứ tự tối đa có thể. Vì Python sử dụng phép chia số nguyên với`//`, chúng tôi tính toán:```
max_pairs = n * (n - 1) // 2
```Một chi tiết tinh tế khác là xử lý cà vạt. Chúng tôi chỉ cập nhật kho lưu trữ tốt nhất khi mức độ tương đồng mới lớn hơn. Vì chúng tôi lặp lại các vấn đề lưu trữ theo thứ tự tăng dần nên các mối liên kết sẽ tự động giữ chỉ mục nhỏ hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
find the next palindrome
1
10 find the previous palindrome or print better luck next time
```Hoán vị chính đã được kiểm tra: 

| Hoán vị | Đảo ngược | Tiếp theo? | 
| --- | --- | --- | 
| tìm bảng màu tiếp theo | 0 | Không | 
| tìm bảng màu tiếp theo | 1 | Có | 
| tìm palindrome tiếp theo | 2 | Không | 

Hoán vị hợp lệ tốt nhất là:```
find the palindrome next
```với một lần đảo ngược. 

Số cặp tối đa có thể:```
4 × 3 / 2 = 6
```Sự tương đồng:```
6 - 1 = 5
```Đầu ra:```
1
[:|||||:]
```Dấu vết này cho thấy tại sao sự đảo ngược tối thiểu lại quan trọng. Một số hoán vị có thể xuất hiện, nhưng chỉ có hoán vị gần nhất với thứ tự ban đầu mới xác định được điểm. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
a b c
2
3 c b a
3 a b c
```Dấu vết: 

| Lưu trữ | Hoán vị tốt nhất | Đảo ngược | Sự tương đồng | 
| --- | --- | --- | --- | 
| cb a | cb a | 3 | 0 | 
| a b c | a b c | 0 | 3 | 

Kho lưu trữ thứ hai thắng. 

Đầu ra:```
2
[:|||:]
```Ví dụ này chứng minh rằng sự tương đồng trực tiếp mang lại lợi ích cho việc duy trì trật tự ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m × n! × k) | Đối với mỗi kho lưu trữ, hãy kiểm tra mọi hoán vị dưới dạng một chuỗi con | 
| Không gian | Ồ (n!) | Lưu trữ tất cả các hoán vị và số lần đảo ngược | 

Với`n ≤ 4`, ta có nhiều nhất là 24 hoán vị. Ngay cả trong trường hợp xấu nhất:```
10 × 24 × 20 = 4800
```so sánh tiếp theo được thực hiện. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def is_subsequence(perm, archive):
        j = 0

        for word in archive:
            if j < len(perm) and word == perm[j]:
                j += 1

        return j == len(perm)

    out = io.StringIO()

    n = int(input())
    base = input().split()

    pos = {word: i for i, word in enumerate(base)}

    perms = []

    for perm in permutations(base):
        inv = 0

        for i in range(n):
            for j in range(i + 1, n):
                if pos[perm[i]] > pos[perm[j]]:
                    inv += 1

        perms.append((perm, inv))

    m = int(input())

    best_similarity = -1
    best_index = -1

    max_pairs = n * (n - 1) // 2

    for idx in range(1, m + 1):
        parts = input().split()
        archive = parts[1:]

        best_inv = None

        for perm, inv in perms:
            if is_subsequence(perm, archive):
                if best_inv is None or inv < best_inv:
                    best_inv = inv

        if best_inv is not None:
            similarity = max_pairs - best_inv

            if similarity > best_similarity:
                best_similarity = similarity
                best_index = idx

    if best_index == -1:
        print("Brand new problem!", file=out)
    else:
        print(best_index, file=out)
        print("[:{}:]".format('|' * best_similarity), file=out)

    return out.getvalue()

# provided sample
assert run(
"""4
find the next palindrome
1
10 find the previous palindrome or print better luck next time
"""
) == "1\n[:|||||:]\n", "sample 1"

# minimum size
assert run(
"""1
hello
1
1 hello
"""
) == "1\n[::]\n", "single word"

# no valid permutation
assert run(
"""2
a b
1
3 a a a
"""
) == "Brand new problem!\n", "missing word"

# tie between archives, smaller index wins
assert run(
"""2
a b
2
2 b a
2 b a
"""
) == "1\n[::]\n", "tie breaking"

# perfect ordering
assert run(
"""3
a b c
1
3 a b c
"""
) == "1\n[:|||:]\n", "maximum similarity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp một từ |`[::]`| Không đảo ngược và không có cặp tối đa | 
| Thiếu từ trong kho lưu trữ |`Brand new problem!`| Tính chính xác xác nhận sau đó | 
| Hai điểm lưu trữ giống hệt nhau | Chỉ mục nhỏ hơn được chọn | Xử lý cà vạt | 
| Đặt hàng hoàn hảo | Độ tương tự tối đa | Tính toán nghịch đảo đúng | 

## Vỏ cạnh 

Hãy xem xét trường hợp kho lưu trữ chứa các từ lặp lại nhưng thiếu một từ bắt buộc:```
2
a b
1
4 a a a a
```Thuật toán kiểm tra mọi hoán vị:```
a b
b a
```Bộ so khớp chuỗi con không bao giờ đạt tới điểm cuối của một trong hai hoán vị bởi vì`b`không bao giờ xuất hiện.`best_inv`còn lại`None`, do đó kho lưu trữ bị từ chối một cách chính xác. 

Bây giờ hãy xem xét việc xử lý cà vạt:```
2
a b
2
2 b a
2 b a
```Cả hai kho lưu trữ đều chứa hoán vị`b a`với một lần đảo ngược. Từ:```
max_pairs = 1
similarity = 1 - 1 = 0
```cả hai kho lưu trữ đều có độ tương tự như nhau. 

Thuật toán chỉ cập nhật câu trả lời khi độ tương tự lớn hơn. Lưu trữ`1`vẫn được chọn, phù hợp với hành vi được yêu cầu. 

Cuối cùng, hãy xem xét đầu vào nhỏ nhất có thể:```
1
hello
1
1 hello
```Chỉ có một hoán vị và không có khả năng đảo ngược nào:```
1 × 0 / 2 = 0
```Thanh tương tự trở thành:```
[::]
```Việc triển khai xử lý việc này một cách tự nhiên vì nhân`'|' * 0`tạo ra một chuỗi trống.
