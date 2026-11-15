---
title: "CF 1808D - Petya, Petya, Petr và Palindromes"
description: "Đối với mỗi mảng con có độ dài k lẻ, chúng ta muốn biết cần bao nhiêu phần tử thay thế để biến mảng con đó thành một palindrome. Câu trả lời là tổng của các giá trị đó trên tất cả các mảng con có độ dài-k."
date: "2026-06-09T08:58:14+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1808
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 861 (Div. 2)"
rating: 2100
weight: 1808
solve_time_s: 137
verified: true
draft: false
---

[CF 1808D - Petya, Petya, Petr và Palindromes](https://codeforces.com/problemset/problem/1808/D) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, brute Force, cấu trúc dữ liệu, hai con trỏ 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Với mọi mảng con có độ dài lẻ`k`, chúng tôi muốn biết cần bao nhiêu phần tử thay thế để biến mảng con đó thành một bảng màu. Câu trả lời là tổng của các giá trị đó trên toàn bộ chiều dài-`k`mảng con. 

Đối với mảng con có độ dài lẻ cố định, mỗi vị trí bên trái được ghép với một vị trí đối xứng ở bên phải. Nếu hai giá trị đã bằng nhau thì không cần thay thế cặp đó. Nếu chúng khác nhau thì chỉ cần thay thế một lần là đủ. Bởi vì các cặp đối xứng khác nhau là độc lập nên tính tương xứng của một phân mảng chỉ đơn giản là số lượng các cặp đối xứng không khớp. 

Độ dài mảng lên tới`2 * 10^5`, vì vậy việc kiểm tra mọi mảng con và mọi cặp bên trong nó là không thể. Có tới`2 * 10^5`cửa sổ và mỗi cửa sổ có thể chứa tối đa`10^5`cặp đối xứng. Một giải pháp bậc hai hoặc bậc ba vượt xa giới hạn. 

Một trường hợp tế nhị xuất hiện khi`k = 1`. Mọi mảng con có độ dài-1 đều đã là một bảng màu, vì vậy câu trả lời luôn là`0`. 

Ví dụ:```
3 1
5 7 9
```Đầu ra đúng là:```
0
```Giải pháp giả định mỗi cửa sổ đóng góp ít nhất một cặp sẽ thất bại ở đây. 

Một nơi dễ mắc lỗi khác là gần viền mảng.```
5 3
1 2 1 2 1
```Cặp bằng nhau`(0, 2)`thuộc về một cửa sổ có độ dài-3 hợp lệ và do đó`(2, 4)`. Cặp đôi`(0, 4)`không thuộc về bất kỳ cửa sổ có độ dài 3 nào mặc dù nó đối xứng quanh tâm. Việc đếm tất cả các cặp bằng nhau trong giới hạn khoảng cách mà không kiểm tra xem tâm có thể là tâm của cửa sổ hợp lệ hay không sẽ cho kết quả sai. 

Cạm bẫy thứ ba là sự ngang bằng. Các vị trí đối xứng xung quanh một tâm số nguyên luôn có cùng tính chẵn lẻ. Vị trí`1`Và`2`không bao giờ có thể tạo thành một cặp đối xứng bên trong cửa sổ có độ dài lẻ vì điểm giữa của chúng không phải là chỉ số mảng. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. Với mọi cửa sổ có độ dài`k`, so sánh mọi cặp đối xứng và đếm những điểm không khớp. Điều này đúng vì mỗi cặp không khớp yêu cầu một thay thế và mỗi cặp phù hợp không yêu cầu thay thế nào. 

Vấn đề là chi phí. có`n - k + 1`cửa sổ và`(k - 1) / 2`cặp trên mỗi cửa sổ. Trong trường hợp xấu nhất, điều này trở nên đại khái`2 * 10^10`những so sánh quá lớn. 

Quan sát quan trọng là ngừng suy nghĩ về các cửa sổ và bắt đầu nghĩ về các cặp đối xứng. 

Cho phép```
m = (k - 1) / 2
```Mọi cửa sổ hợp lệ đều đóng góp chính xác`m`cặp đối xứng. Vì có`n - k + 1`windows, tổng số lần xuất hiện của cặp đối xứng là```
(n - k + 1) * m
```Nếu chúng ta tính mọi cặp như vậy là không khớp nhau thì đó sẽ là câu trả lời. Điều chỉnh duy nhất là các cặp đối xứng bằng nhau góp phần`0`thay vì`1`. 

Vì vậy, vấn đề trở thành:```
How many valid symmetric pair appearances have equal values?
```Mỗi cặp đối xứng được xác định bởi hai vị trí`(i, j)`. 

Cặp này được sử dụng trong một thời gian dài-`k`cửa sổ khi và chỉ khi:```
1. i and j have the same parity.
2. j - i <= 2m.
3. Their midpoint lies inside the range of valid window centers.
```Bây giờ chúng ta chỉ cần đếm các cặp vị trí có giá trị bằng nhau thỏa mãn các điều kiện đó. 

Các vị trí có giá trị khác nhau không liên quan nên chúng tôi nhóm các chỉ số theo giá trị. Bên trong mỗi nhóm giá trị, các chỉ số được sắp xếp. Đối với mỗi chỉ số`i`, chúng tôi tính toán khoảng cách của các vị trí`j`thỏa mãn tất cả các ràng buộc về tính hợp lệ, sau đó đếm xem có bao nhiêu chỉ số có giá trị bằng nhau nằm trong khoảng đó bằng cách sử dụng tìm kiếm nhị phân. 

Điều này làm giảm vấn đề xuống`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`m = (k - 1) // 2`. 
2. Tính toán```
total_pairs = (n - k + 1) * m
```Đây là số lần xuất hiện của cặp đối xứng trên tất cả các cửa sổ. 
3. Nhóm các vị trí mảng theo giá trị và tính chẵn lẻ. 

Các vị trí có tính chẵn lẻ khác nhau không bao giờ có thể tạo thành một cặp đối xứng quanh tâm số nguyên, vì vậy chúng tôi lưu trữ hai danh sách cho mỗi giá trị. 
4. Đối với mọi vị trí`p`bên trong một danh sách chẵn lẻ, xác định vị trí có giá trị bằng nhau`q > p`có thể tạo thành một cặp đối xứng hợp lệ. 
5. Điều kiện trung điểm cho```
m <= (p + q) / 2 <= n - m - 1
```trở thành```
2m - p <= q <= 2(n - m - 1) - p
```6. Điều kiện khoảng cách cho```
q <= p + 2m
```7. Kết hợp mọi ràng buộc,```
low = max(p + 1, 2m - p)
up  = min(p + 2m, 2(n - m - 1) - p)
```8. Sử dụng tìm kiếm nhị phân trong danh sách chẵn lẻ đã sắp xếp để đếm xem có bao nhiêu vị trí`[low, up]`. 
9. Tổng hợp tất cả các số đó thành`equal_pairs`. 
10. Mọi sự xuất hiện của cặp đối xứng hợp lệ đều góp phần`1`nếu giá trị khác nhau hoặc`0`nếu các giá trị bằng nhau thì```
answer = total_pairs - equal_pairs
```### Tại sao nó hoạt động 

Mỗi cặp đối xứng xuất hiện trong mỗi cửa sổ tương ứng với chính xác một cặp vị trí mảng`(i, j)`trung điểm của nó là tâm cửa sổ. Ba điều kiện dẫn xuất chính xác là những điều kiện mà theo đó một cửa sổ như vậy tồn tại.`total_pairs`đếm tất cả các lần xuất hiện của cặp đối xứng.`equal_pairs`đếm chính xác những lần xuất hiện có điểm cuối đã khớp. Mỗi cặp phù hợp đóng góp`0`đến palindromicity thay vì`1`, do đó, việc trừ chúng sẽ để lại chính xác số lần xuất hiện của các cặp đối xứng không khớp, là tổng số lượng palindromicity cần thiết. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right
from collections import defaultdict

input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

m = (k - 1) // 2

total_pairs = (n - k + 1) * m

pos = defaultdict(lambda: [[], []])

for i, x in enumerate(a):
    pos[x][i & 1].append(i)

equal_pairs = 0

for parity_lists in pos.values():
    for arr in parity_lists:
        for idx, p in enumerate(arr):
            low = max(p + 1, 2 * m - p)
            up = min(p + 2 * m, 2 * (n - m - 1) - p)

            if low > up:
                continue

            l = bisect_left(arr, low, idx + 1)
            r = bisect_right(arr, up)

            equal_pairs += r - l

print(total_pairs - equal_pairs)
```Phần đầu tiên tính tổng số lần xuất hiện của cặp đối xứng trên tất cả các cửa sổ. 

Từ điển nhóm các chỉ số theo giá trị và tính chẵn lẻ. Việc chia theo số chẵn lẻ là rất quan trọng vì các vị trí đối xứng xung quanh một số nguyên trung tâm luôn có số chẵn lẻ bằng nhau. Nếu không có sự phân tách này, các cặp có điểm giữa không nguyên sẽ được tính không chính xác. 

Đối với một vị trí cố định`p`, khoảng`[low, up]`là giao điểm của tất cả các ràng buộc hợp lệ. Mỗi vị trí có giá trị bằng nhau bên trong khoảng đó tạo thành một cặp đối xứng phù hợp. 

Các tìm kiếm nhị phân đếm các vị trí đó theo thời gian logarit. Vì mỗi chỉ mục thuộc về chính xác một danh sách nên tổng công việc là`O(n log n)`. 

Tất cả số học đều vừa vặn thoải mái bên trong các số nguyên Python. Câu trả lời có thể đạt được một cách đại khái`10^10`, vì vậy việc sử dụng số nguyên 32 bit trong các ngôn ngữ khác sẽ không an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8 5
1 2 8 2 5 2 8 6
```Đây:```
m = 2
total_pairs = 4 * 2 = 8
```Các cặp đối xứng bằng nhau hợp lệ: 

| p | q | giá trị | được tính? | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | Có | 
| 2 | 6 | 8 | Có | 
| 3 | 5 | 2 | Có | 
| 1 | 5 | 2 | Không | 

Cặp cuối cùng cách nhau quá xa. 

Vì vậy: 

| Số lượng | Giá trị | 
| --- | --- | 
| tổng_cặp | 8 | 
| cặp bằng nhau | 4 | 
| trả lời | 4 | 

Đầu ra:```
4
```Ví dụ này cho thấy câu trả lời không được tính toán theo từng cửa sổ. Chúng tôi đếm tất cả các lần xuất hiện của các cặp đối xứng trên toàn cầu và trừ đi những lần xuất hiện trùng khớp. 

### Mẫu 2 

đầu vào:```
9 9
1 2 3 4 5 4 3 2 1
```Đây:```
m = 4
total_pairs = 1 * 4 = 4
```Cửa sổ duy nhất đã là một bảng màu. 

| Cặp đối xứng | Bình đẳng? | 
| --- | --- | 
| (0, 8) | Có | 
| (1, 7) | Có | 
| (2, 6) | Có | 
| (3, 5) | Có | 

Như vậy: 

| Số lượng | Giá trị | 
| --- | --- | 
| tổng_cặp | 4 | 
| cặp bằng nhau | 4 | 
| trả lời | 0 | 

Đầu ra:```
0
```Điều này chứng tỏ rằng khi mọi cặp đối xứng khớp nhau, phép trừ sẽ loại bỏ tất cả các đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi chỉ mục tham gia tìm kiếm nhị phân bên trong nhóm giá trị tương đương của nó | 
| Không gian | O(n) | Lưu trữ các vị trí được nhóm | 

Với`n ≤ 2 · 10^5`, MỘT`O(n log n)`giải pháp chỉ thực hiện vài triệu thao tác và dễ dàng phù hợp với thời gian giới hạn. Bộ nhớ phụ là tuyến tính về số lượng chỉ số. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left, bisect_right
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    m = (k - 1) // 2
    total_pairs = (n - k + 1) * m

    pos = defaultdict(lambda: [[], []])

    for i, x in enumerate(a):
        pos[x][i & 1].append(i)

    equal_pairs = 0

    for parity_lists in pos.values():
        for arr in parity_lists:
            for idx, p in enumerate(arr):
                low = max(p + 1, 2 * m - p)
                up = min(p + 2 * m, 2 * (len(a) - m - 1) - p)

                if low > up:
                    continue

                l = bisect_left(arr, low, idx + 1)
                r = bisect_right(arr, up)

                equal_pairs += r - l

    return str(total_pairs - equal_pairs)

# provided sample
assert run("8 5\n1 2 8 2 5 2 8 6\n") == "4"

# minimum size
assert run("1 1\n7\n") == "0"

# all equal
assert run("5 3\n4 4 4 4 4\n") == "0"

# k = 1
assert run("4 1\n1 2 3 4\n") == "0"

# off-by-one around borders
assert run("5 3\n1 2 1 2 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`|`0`| Kích thước đầu vào tối thiểu | 
|`5 3 / 4 4 4 4 4`|`0`| Mỗi cửa sổ đã có palindromic | 
|`4 1 / 1 2 3 4`|`0`| Trường hợp đặc biệt`k = 1`| 
|`5 3 / 1 2 1 2 1`|`0`| Ràng buộc trung tâm biên giới và xử lý tính chẵn lẻ | 

## Vỏ cạnh 

Hãy xem xét:```
3 1
5 7 9
```chúng tôi có`m = 0`, Vì thế```
total_pairs = 0
```Không có cặp đối xứng nào tồn tại trong bất kỳ cửa sổ nào. Thuật toán ngay lập tức trở lại`0`. 

Coi như:```
5 3
1 2 1 2 1
```Trung tâm hợp lệ là vị trí`1`,`2`, Và`3`. 

Cặp đôi`(0, 4)`có các giá trị bằng nhau và một điểm giữa là số nguyên, nhưng khoảng cách của nó là`4`, lớn hơn`2m = 2`. Nó không bao giờ xuất hiện dưới dạng một cặp đối xứng trong cửa sổ có độ dài 3. Việc tính toán khoảng thời gian sẽ tự động loại trừ nó. 

Coi như:```
5 5
1 1 2 1 1
```Cặp đôi`(0, 1)`có giá trị bằng nhau nhưng có tính chẵn lẻ khác nhau. Điểm giữa của nó không phải là một vị trí mảng số nguyên, vì vậy nó không thể đối xứng quanh tâm của một cửa sổ có độ dài lẻ. Bởi vì các chỉ số được phân tách bằng tính chẵn lẻ trước khi đếm nên cặp này không bao giờ được xem xét. 

Những trường hợp này chính xác là khi nhiều cách triển khai không chính xác sẽ vượt quá số lượng các cặp khớp. Các điều kiện khoảng và chẵn lẻ sẽ loại bỏ tất cả các cặp không hợp lệ.
