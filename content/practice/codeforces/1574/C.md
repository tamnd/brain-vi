---
title: "CF 1574C - Giết Rồng"
description: "Chúng tôi có một đội anh hùng, mỗi người có một sức mạnh cố định. Đối với mỗi con rồng, chúng ta phải chọn chính xác một anh hùng để chiến đấu với nó, trong khi tất cả những anh hùng còn lại ở lại để bảo vệ lâu đài. Một con rồng có hai yêu cầu."
date: "2026-06-10T11:07:32+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1574
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 114 (Rated for Div. 2)"
rating: 1300
weight: 1574
solve_time_s: 353
verified: false
draft: false
---

[CF 1574C - Giết rồng](https://codeforces.com/problemset/problem/1574/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp, tìm kiếm ba ngôi 
**Thời gian giải:** 5 phút 53s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một đội anh hùng, mỗi người có một sức mạnh cố định. Đối với mỗi con rồng, chúng ta phải chọn chính xác một anh hùng để chiến đấu với nó, trong khi tất cả những anh hùng còn lại ở lại để bảo vệ lâu đài. 

Một con rồng có hai yêu cầu. Kẻ tấn công được chọn ít nhất phải có sức mạnh`x`, và tổng sức mạnh của tất cả những người phòng thủ ít nhất phải là`y`. 

Chúng tôi được phép tiêu xu để tăng sức mạnh anh hùng. Mỗi điểm sức mạnh tốn một xu và việc nâng cấp chỉ là tạm thời vì mỗi con rồng được xử lý độc lập. 

Đối với mỗi con rồng, chúng ta phải xác định số xu tối thiểu cần thiết để một số anh hùng có thể đáp ứng yêu cầu tấn công trong khi những anh hùng còn lại đáp ứng yêu cầu phòng thủ. 

Các ràng buộc ngay lập tức loại trừ mọi giải pháp kiểm tra mọi anh hùng đối với mọi con rồng. Cả hai`n`Và`m`có thể đạt được`2 · 10^5`, vì vậy một`O(nm)`thuật toán sẽ thực hiện khoảng`4 · 10^10`hoạt động, vượt xa những gì phù hợp với giới hạn hai giây. 

Sức mạnh của anh hùng có thể lớn bằng`10^12`, và yêu cầu phòng thủ có thể lớn bằng`10^18`. Điều này có nghĩa là chúng ta phải sử dụng số học số nguyên 64 bit. Python tự động xử lý việc này. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
Heroes: [10]
Dragon: x = 5, y = 100
```Người tấn công đã đáp ứng được yêu cầu tấn công, nhưng một khi anh hùng đó rời đi, người phòng thủ sẽ có sức mạnh`0`. Câu trả lời là`100`, không`0`. Chỉ tập trung vào tướng tấn công sẽ mang lại kết quả sai lầm. 

Coi như:```
Heroes: [5, 100]
Dragon: x = 50, y = 5
```Việc lựa chọn anh hùng mạnh nhất có vẻ hấp dẫn vì không cần nâng cấp đòn tấn công. Nhưng rồi hậu vệ có sức mạnh`5`, thế là đủ rồi. Chi phí là`0`. 

Nếu thay vào đó chúng ta chọn anh hùng`5`, chúng tôi cần`45`tiền xu để đạt được sức mạnh tấn công`50`. Sự lựa chọn của kẻ tấn công là vấn đề. 

Một trường hợp tế nhị khác:```
Heroes: [4, 8]
Dragon: x = 6, y = 100
```Chọn anh hùng`8`chi phí:```
attack = 0
defense = 100 - 4 = 96
total = 96
```Chọn anh hùng`4`chi phí:```
attack = 6 - 4 = 2
defense = 100 - 8 = 92
total = 94
```Người anh hùng gần gũi nhất`x`không phải lúc nào cũng tối ưu. Đôi khi trả nhiều tiền hơn cho kẻ tấn công sẽ cải thiện chất lượng của người phòng thủ đủ để giảm tổng chi phí. 

Những ví dụ này gợi ý rằng chúng ta phải đánh giá cẩn thận xem anh hùng nào sẽ trở thành kẻ tấn công. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Đối với mỗi con rồng, hãy thử mọi anh hùng làm kẻ tấn công. 

Giả sử sức mạnh anh hùng là`a[i]`và tổng sức mạnh của tất cả các anh hùng là`S`. 

Nếu anh hùng này tấn công, người phòng thủ sẽ đóng góp`S - a[i]`. 

Chi phí yêu cầu tấn công:```
max(0, x - a[i])
```Chi phí yêu cầu phòng thủ:```
max(0, y - (S - a[i]))
```Tổng chi phí là tổng của hai giá trị này. 

Việc kiểm tra mọi anh hùng sẽ đưa ra câu trả lời chính xác vì nó đánh giá rõ ràng tất cả những kẻ tấn công có thể và lấy mức tối thiểu. Thật không may, mỗi truy vấn cần`O(n)`công việc. Với`2 · 10^5`anh hùng và`2 · 10^5`rồng, điều này trở nên đại khái`4 · 10^10`đánh giá. 

Quan sát quan trọng là hàm chi phí thay đổi theo một cách rất có cấu trúc. 

Hãy để một anh hùng có sức mạnh`h`. 

Đóng góp của cuộc tấn công là:```
max(0, x - h)
```BẰNG`h`tăng thì đại lượng này giảm. 

Phần đóng góp cho quốc phòng là:```
max(0, y - (S - h))
```tương đương với:```
max(0, y - S + h)
```BẰNG`h`tăng thì số lượng này tăng. 

Một phần thích những anh hùng lớn hơn, phần còn lại thích những anh hùng nhỏ hơn. 

Điều này có nghĩa là mức tối ưu xảy ra gần điểm mà yêu cầu tấn công thay đổi từ không thỏa mãn sang thỏa mãn, cụ thể là xung quanh sức mạnh`x`. 

Sau khi sắp xếp các anh hùng, chúng ta có thể tìm kiếm nhị phân anh hùng đầu tiên có sức mạnh ít nhất là`x`. 

Bất kỳ anh hùng nào nhỏ hơn nhiều so với`x`yêu cầu thêm tiền tấn công. Bất kỳ anh hùng nào lớn hơn nhiều so với`x`làm suy yếu lực lượng phòng thủ một cách không cần thiết. Điều tối thiểu chỉ có thể xảy ra giữa những anh hùng gần nhất`x`. 

Cụ thể chúng ta chỉ cần kiểm tra: 

1. Anh hùng đầu tiên có sức mạnh tối thiểu`x`. 
2. Người anh hùng ngay trước nó. 

Đó là những ứng cử viên duy nhất có thể đánh bại tất cả những người khác. 

Mỗi truy vấn trở thành một tìm kiếm nhị phân và hai đánh giá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + m log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả sức mạnh của anh hùng và tính tổng sức mạnh của họ`S`. 
2. Sắp xếp sức mạnh anh hùng. 

Sắp xếp cho phép tìm kiếm nhị phân về sức mạnh anh hùng. 
3. Đối với mỗi con rồng`(x, y)`, tìm anh hùng đầu tiên có sức mạnh ít nhất`x`. 

Sử dụng tìm kiếm nhị phân (`bisect_left`). 
4. Nếu một anh hùng như vậy tồn tại, hãy đánh giá việc sử dụng anh hùng đó làm kẻ tấn công. 

Yêu cầu tấn công đã được đáp ứng nên chi phí tấn công có thể bằng 0. Chi phí bảo vệ là:```
max(0, y - (S - hero))
```5. Nếu có một anh hùng ngay trước vị trí tìm kiếm nhị phân, hãy đánh giá anh hùng đó. 

Anh hùng này là anh hùng mạnh nhất dưới đây`x`. Nó có thể cần nâng cấp tấn công:```
max(0, x - hero)
```cộng với bất kỳ nâng cấp phòng thủ. 
6. Lấy chi phí tối thiểu trong số các ứng viên được đánh giá. 
7. Xuất ra câu trả lời cho truy vấn. 

### Tại sao nó hoạt động 

Hãy để anh hùng tấn công có sức mạnh`h`. 

Chi phí là```
f(h) =
max(0, x - h)
+
max(0, y - (S - h))
```Số hạng thứ nhất giảm dần khi`h`tăng lên. Số hạng thứ hai tăng khi`h`tăng lên. 

Dành cho các anh hùng bên dưới`x`, việc chuyển sang anh hùng lớn hơn không bao giờ tăng số hạng đầu tiên mà chỉ có thể tăng dần số hạng thứ hai. Anh hùng tốt nhất dưới đây`x`là cái lớn nhất bên dưới`x`. 

Ít nhất là đối với các anh hùng`x`, số hạng đầu tiên trở thành số không. Trong số những anh hùng như vậy, việc chọn một anh hùng lớn hơn chỉ làm suy yếu lực lượng phòng thủ và không bao giờ giúp ích được gì. Ít nhất là anh hùng tốt nhất`x`ít nhất là cái nhỏ nhất`x`. 

Sau khi phân loại, hai anh hùng này chính xác là tiền thân và người kế vị của`x`trong mảng đã sắp xếp. Mọi anh hùng khác đều bị thống trị bởi một trong những ứng cử viên này, vì vậy chỉ kiểm tra hai khả năng này luôn tìm ra phương án tối ưu. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()
total = sum(a)

m = int(input())

for _ in range(m):
    x, y = map(int, input().split())

    pos = bisect_left(a, x)
    ans = 10**30

    if pos < n:
        hero = a[pos]
        cost = max(0, y - (total - hero))
        ans = min(ans, cost)

    if pos > 0:
        hero = a[pos - 1]
        cost = max(0, x - hero) + max(0, y - (total - hero))
        ans = min(ans, cost)

    print(ans)
```Phần đầu tiên sắp xếp các anh hùng và tính toán tổng sức mạnh của họ. Tổng số được sử dụng lại cho mọi truy vấn, cho phép tính toán sức mạnh của người phòng thủ ngay lập tức như`total - hero`. 

Đối với mỗi con rồng,`bisect_left`tìm anh hùng đầu tiên có sức mạnh ít nhất bằng giá trị tấn công cần thiết. Anh hùng đó là anh hùng nhỏ nhất có khả năng chiến đấu mà không cần nâng cấp đòn tấn công. 

Người tiền nhiệm cũng được kiểm tra vì đây là anh hùng mạnh nhất dưới ngưỡng. Bất kỳ anh hùng nào yếu hơn sẽ cần nâng cấp tấn công nhiều hơn trong khi không mang lại lợi thế bù đắp. 

Câu trả lời ban đầu được đặt thành một số rất lớn. Mọi ứng viên hợp lệ đều cập nhật nó. 

Một sai lầm phổ biến là quên rằng sức mạnh của người phòng thủ đã loại trừ anh hùng tấn công. Sức mạnh phòng thủ chính xác là`total - hero`, không`total`. 

Một lỗi phổ biến khác là chỉ kiểm tra kết quả tìm kiếm nhị phân và bỏ qua kết quả tìm kiếm trước đó. Một số giải pháp tối ưu yêu cầu chọn tướng thấp hơn một chút`x`bởi vì những người phòng thủ mạnh hơn sẽ tiết kiệm được nhiều xu hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1, Truy vấn`(7, 9)`Anh hùng sau khi sắp xếp:```
[2, 3, 3, 6]
```Tổng sức mạnh:```
S = 14
```Tìm kiếm nhị phân cho`x = 7`. 

| Anh hùng ứng cử viên | Chi phí tấn công | Sức mạnh hậu vệ | Chi phí phòng thủ | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 6 | 1 | 8 | 1 | 2 | 

Ít nhất không có anh hùng nào`7`. 

Trả lời:```
2
```Ví dụ này cho thấy trường hợp chỉ tồn tại ứng cử viên tiền nhiệm. 

### Mẫu 1, Truy vấn`(4, 14)`Anh hùng được sắp xếp:```
[2, 3, 3, 6]
```Tổng cộng:```
14
```Tìm kiếm nhị phân mang lại anh hùng`6`. 

| Anh hùng ứng cử viên | Chi phí tấn công | Sức mạnh hậu vệ | Chi phí phòng thủ | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 6 | 0 | 8 | 6 | 6 | 
| 3 | 1 | 11 | 3 | 4 | 

Chi phí tối thiểu là:```
4
```Anh hùng yếu hơn thực sự tốt hơn vì việc loại bỏ anh hùng mạnh nhất khỏi quân phòng thủ là rất tốn kém. 

Điều này chứng tỏ tại sao cả hai ứng viên lân cận đều phải được kiểm tra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log n) | Sắp xếp một lần, sau đó tìm kiếm nhị phân một lần cho mỗi truy vấn | 
| Không gian | O(1) thêm | Ngoài mảng anh hùng | 

Chi phí giai đoạn phân loại`O(n log n)`. Mỗi truy vấn rồng thực hiện tìm kiếm nhị phân và đánh giá tối đa hai ứng cử viên, do đó chi phí`O(log n)`. 

Với`n, m ≤ 2 · 10^5`, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    a.sort()
    total = sum(a)

    m = int(sys.stdin.readline())

    out = []

    for _ in range(m):
        x, y = map(int, sys.stdin.readline().split())

        pos = bisect_left(a, x)
        ans = 10**30

        if pos < n:
            hero = a[pos]
            ans = min(ans, max(0, y - (total - hero)))

        if pos > 0:
            hero = a[pos - 1]
            ans = min(
                ans,
                max(0, x - hero) +
                max(0, y - (total - hero))
            )

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""4
3 6 2 3
5
3 12
7 9
4 14
1 10
8 7
"""
) == """1
2
4
0
2
"""

# minimum size
assert run(
"""2
5 10
1
5 1
"""
) == """0
"""

# all equal heroes
assert run(
"""3
5 5 5
1
5 10
"""
) == """0
"""

# only predecessor exists
assert run(
"""2
4 6
1
10 1
"""
) == """4
"""

# predecessor better than successor
assert run(
"""2
4 8
1
6 100
"""
) == """94
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 5 10 / (5,1)`|`0`| Cấu hình hợp lệ kích thước tối thiểu | 
|`5 5 5 / (5,10)`|`0`| Mọi anh hùng đều bình đẳng | 
|`4 6 / (10,1)`|`4`| Vị trí tìm kiếm nhị phân ở cuối mảng | 
|`4 8 / (6,100)`|`94`| Câu trả lời tối ưu đến từ người tiền nhiệm | 

## Vỏ cạnh 

Hãy xem xét:```
2
5 100
1
50 5
```Mảng được sắp xếp là`[5, 100]`, tổng sức mạnh là`105`. 

Tìm kiếm nhị phân tìm thấy anh hùng`100`. 

Dành cho anh hùng`100`:```
attack cost = 0
defense cost = max(0, 5 - 5) = 0
total = 0
```Dành cho anh hùng`5`:```
attack cost = 45
defense cost = 0
total = 45
```Thuật toán trả về`0`, chọn chính xác anh hùng mạnh hơn. 

Bây giờ hãy xem xét:```
2
4 8
1
6 100
```Tổng sức mạnh là`12`. 

Dành cho anh hùng`8`:```
0 + (100 - 4) = 96
```Dành cho anh hùng`4`:```
(6 - 4) + (100 - 8) = 94
```Thuật toán kiểm tra cả các ứng viên lân cận và trả về`94`. Bất kỳ giải pháp nào ít nhất chỉ chọn anh hùng nhỏ nhất`x`sẽ xuất ra không chính xác`96`. 

Cuối cùng, hãy xem xét:```
2
10 20
1
5 100
```Cả hai anh hùng đều đã đáp ứng được yêu cầu tấn công. Tìm kiếm nhị phân trỏ đến anh hùng`10`. 

Dành cho anh hùng`10`:```
attack cost = 0
defense cost = 80
```Dành cho anh hùng`20`:```
attack cost = 0
defense cost = 90
```Thuật toán chọn anh hùng đủ điều kiện nhỏ hơn vì nó để lại nhiều sức mạnh hơn để bảo vệ lâu đài. Đây chính xác là lý do tại sao ứng cử viên kế vị ít nhất phải là anh hùng nhỏ nhất`x`, không phải là một anh hùng đủ điều kiện tùy ý.
