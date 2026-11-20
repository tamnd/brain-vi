---
title: "CF 2136A - Trong Giấc Mơ"
description: "Chúng ta biết tỷ số ở giờ nghỉ giải lao và tỷ số cả trận. Nếu tỷ số giữa hiệp là a:b thì trong hiệp một đội RiOI ghi đúng một bàn và đội KDOI ghi đúng b bàn."
date: "2026-06-08T02:35:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2136
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1046 (Div. 2)"
rating: 800
weight: 2136
solve_time_s: 88
verified: true
draft: false
---

[CF 2136A - Trong giấc mơ](https://codeforces.com/problemset/problem/2136/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta biết tỷ số ở giờ nghỉ giải lao và tỷ số cả trận. 

Nếu tỷ số giữa hiệp là`a:b`, thì suốt hiệp 1 đội RiOI đã ghi bàn chính xác`a`bàn thắng và đội KDOI đã ghi bàn chính xác`b`mục tiêu. 

Nếu tỉ số cuối cùng là`c:d`, sau đó trong hiệp hai đội RiOI đã ghi bàn`c-a`bàn thắng và đội KDOI đã ghi bàn`d-b`mục tiêu. 

Hạn chế duy nhất là trong mỗi hiệp, không đội nào được ghi ba bàn liên tiếp. Hai nửa là độc lập. Kỉ lục ở cuối hiệp một không tiếp tục sang hiệp hai. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định xem có thể sắp xếp các bàn thắng trong hiệp một và hiệp hai sao cho không hiệp nào có ba bàn thắng liên tiếp của cùng một đội hay không. 

Điểm số tối đa là 100 và có tối đa 1000 trường hợp thử nghiệm. Những giới hạn này rất nhỏ. Ngay cả một giải pháp thực hiện một số tính toán cho mọi cặp điểm có thể cũng sẽ dễ dàng nằm trong giới hạn. Thách thức thực sự là xác định điều kiện toán học đặc trưng khi tồn tại một chuỗi mục tiêu hợp lệ. 

Một sai lầm phổ biến là chỉ nghĩ đến tổng điểm của trận đấu. Hạn chế áp dụng riêng cho từng hiệp. Ví dụ:```
a=1 b=4 c=2 d=5
```Hiệp 1 đã có bàn thắng`1:4`và nửa sau đóng góp`1:1`. Cả hai nửa có thể được sắp xếp hợp lệ, vì vậy câu trả lời là`YES`. 

Một sai lầm dễ mắc phải khác là quên rằng hiệp không có bàn thắng luôn có giá trị. Ví dụ:```
a=4 b=1 c=4 d=1
```Hiệp hai không có bàn thắng. Một chuỗi trống không thể chứa ba bàn thắng liên tiếp, vì vậy câu trả lời là`YES`. 

Trường hợp lợi thế quan trọng nhất xảy ra khi một đội ghi được nhiều bàn thắng hơn đội kia. Ví dụ:```
a=0 b=100
```Mọi bàn thắng đều thuộc về KDOI. Điều đó chắc chắn tạo ra 100 mục tiêu KDOI liên tiếp, vi phạm quy tắc. Chỉ riêng nửa đầu là không thể nên đáp án là`NO`. 

## Phương pháp tiếp cận 

Quan điểm bạo lực rất hữu ích cho việc khám phá tình trạng này. 

Giả sử một nửa chứa`x`Mục tiêu của RiOI và`y`Mục tiêu KDOI. Chúng ta có thể thử tất cả các chuỗi chứa chính xác số lượng đó và kiểm tra xem liệu có chuỗi nào tránh được ba bàn thắng liên tiếp giống hệt nhau hay không. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa về tính hợp lệ. 

Vấn đề là số lượng trình tự rất lớn. Một hiệp đấu với 100 bàn thắng đã có khoảng`2^100`những sắp xếp có thể. Tìm kiếm toàn diện là vô vọng. 

Quan sát quan trọng là các mục tiêu của một đội chỉ có thể xuất hiện ở các khối có độ dài tối đa là 2. 

Giả sử một đội ghi được nhiều bàn thắng hơn đội kia. Cho phép```
big = max(x, y)
small = min(x, y)
```các`small`mục tiêu có thể được sử dụng làm dấu phân cách. Họ tạo ra nhiều nhất`small + 1`khoảng trống:```
_ S _ S _ S _ ...
```Trong mỗi khoảng cách, đội ghi nhiều bàn hơn có thể đóng góp nhiều nhất 2 bàn thắng, nếu không sẽ xuất hiện một pha dài 3 bàn. 

Vì có`small + 1`khoảng trống và mỗi khoảng trống có thể chứa tối đa 2 bàn thắng, số bàn thắng tối đa cho đội lớn hơn là```
2 * (small + 1)
```Vì vậy, một chuỗi hợp lệ tồn tại chính xác khi```
big <= 2 * (small + 1)
```Điều kiện này cũng đủ. Nếu đúng, chúng tôi có thể phân bổ các mục tiêu của nhóm lớn hơn cho các khoảng trống có sẵn mà không vượt quá 2 mục tiêu cho mỗi khoảng cách. 

Chúng ta chỉ cần kiểm tra điều kiện này cho nửa đầu và nửa sau một cách độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi trường hợp thử nghiệm: 

1. Tính tỷ số hiệp 1:```
(a, b)
```2. Kiểm tra xem có tồn tại chuỗi nửa đầu hợp lệ hay không. 

Cho phép:```
big = max(a, b)
small = min(a, b)
```Một nửa có giá trị nếu:```
big <= 2 * (small + 1)
```3. Tính số bàn thắng ghi được trong hiệp 2:```
x = c - a
y = d - b
```4. Kiểm tra điều kiện tương tự cho`(x, y)`. 

Một lần nữa, một nửa có giá trị nếu:```
max(x, y) <= 2 * (min(x, y) + 1)
```5. In`"YES"`nếu cả hai nửa đều hợp lệ. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Hãy xem xét một nửa bất kỳ với số lượng`(x, y)`. Gọi số lớn hơn là`big`và số lượng nhỏ hơn là`small`. 

các`small`mục tiêu chia chuỗi thành nhiều nhất`small + 1`những vị trí có thể đặt mục tiêu của nhóm lớn hơn. Vì 3 bàn thắng bằng nhau liên tiếp bị cấm nên mỗi vị trí chỉ được phép chứa tối đa 2 bàn thắng như vậy. Do đó mọi dãy hợp lệ đều thỏa mãn```
big <= 2 * (small + 1)
```Ngược lại, nếu bất đẳng thức này đúng, chúng ta có thể phân phối`big`mục tiêu giữa các`small + 1`vị trí nào để không có vị trí nào nhận được nhiều hơn 2 bàn thắng. Việc đặt các mục tiêu phân cách giữa các vị trí đó sẽ tạo ra một chuỗi hợp lệ. 

Như vậy bất đẳng thức vừa cần vừa đủ. Vì hai nửa độc lập nên giấc mơ có thể xảy ra chính xác khi cả hai nửa đều thỏa mãn điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def valid(x, y):
    big = max(x, y)
    small = min(x, y)
    return big <= 2 * (small + 1)

t = int(input())

for _ in range(t):
    a, b, c, d = map(int, input().split())

    first_half = valid(a, b)
    second_half = valid(c - a, d - b)

    print("YES" if first_half and second_half else "NO")
```Hàm trợ giúp thực hiện đặc tính toán học của một nửa hợp lệ. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi đánh giá hai nửa độc lập. Nửa đầu sử dụng trực tiếp`(a, b)`. Hiệp 2 sử dụng số bàn thắng ghi thêm sau giờ nghỉ giải lao, cụ thể là`(c-a, d-b)`. 

Không cần phải xây dựng trình tự mục tiêu thực tế. Bằng chứng cho thấy rằng bất đẳng thức hoàn toàn xác định liệu một sự sắp xếp hợp lệ có tồn tại hay không. 

Việc triển khai sẽ tự động tránh được tất cả các vấn đề về ranh giới. Các nửa trống có tác dụng vì, ví dụ,`(0, 0)`thỏa mãn bất đẳng thức. Những trường hợp chỉ có điểm của một đội cũng được xử lý chính xác. Ví dụ`(0, 2)`là hợp lệ, trong khi`(0, 3)`thì không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 4 2 5
```Nửa đầu:`(1, 4)`Nửa sau:`(1, 1)`| Một nửa | x | y | lớn | nhỏ | Tình trạng | 
| --- | --- | --- | --- | --- | --- | 
| Đầu tiên | 1 | 4 | 4 | 1 | 4 4 4 | 
| Thứ hai | 1 | 1 | 1 | 1 | 1 4 | 

Cả hai nửa đều thỏa mãn điều kiện nên đáp án là:```
YES
```Ví dụ này cho thấy một nửa mất cân đối nhiều vẫn có thể có giá trị. Một trình tự như`KKRKK`chứa bốn bàn thắng K và một bàn thắng R mà không bao giờ tạo ra ba bàn thắng K liên tiếp. 

### Ví dụ 2 

đầu vào:```
0 100 0 100
```Nửa đầu:`(0, 100)`Nửa sau:`(0, 0)`| Một nửa | x | y | lớn | nhỏ | Tình trạng | 
| --- | --- | --- | --- | --- | --- | 
| Đầu tiên | 0 | 100 | 100 | 0 | 100 2 | 
| Thứ hai | 0 | 0 | 0 | 0 | 0 2 | 

Điều kiện nửa đầu không thành công nên câu trả lời là:```
NO
```Điều này thể hiện đối số dấu phân cách. Với việc một đội không có bàn thắng, chỉ còn một khoảng trống, đội này có thể giữ tối đa hai bàn trước khi tạo ra một chuỗi cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Công việc liên tục cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Ngay cả với 1000 trường hợp kiểm thử, chương trình chỉ thực hiện một số phép tính số học cho mỗi trường hợp. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def valid(x, y):
        big = max(x, y)
        small = min(x, y)
        return big <= 2 * (small + 1)

    t = int(input())
    out = []

    for _ in range(t):
        a, b, c, d = map(int, input().split())
        ok = valid(a, b) and valid(c - a, d - b)
        out.append("YES" if ok else "NO")

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""11
1 4 1 4
4 1 4 1
1 4 2 5
0 100 0 100
1 4 2 9
3 1 13 5
8 11 17 36
19 41 30 50
20 38 30 60
0 0 0 0
100 100 100 100
"""
) == """YES
YES
YES
NO
NO
YES
NO
NO
YES
YES
YES
"""

# minimum values
assert run(
"""1
0 0 0 0
"""
) == """YES
"""

# boundary between valid and invalid
assert run(
"""2
0 2 0 2
0 3 0 3
"""
) == """YES
NO
"""

# second half causes failure
assert run(
"""1
1 1 1 4
"""
) == """NO
"""

# maximum values, all equal
assert run(
"""1
100 100 100 100
"""
) == """YES
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 0`|`YES`| Trận đấu trống là hợp lệ | 
|`0 2 0 2`|`YES`| Số lượng lớn nhất chính xác bằng giới hạn | 
|`0 3 0 3`|`NO`| Giá trị đầu tiên vượt quá giới hạn | 
|`1 1 1 4`|`NO`| Nửa đầu hợp lệ, nửa sau không hợp lệ | 
|`100 100 100 100`|`YES`| Điểm cân bằng tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1
0 2 0 2
```Trong nửa đầu,`big = 2`Và`small = 0`. Điều kiện trở thành:```
2 <= 2 * (0 + 1)
```đó là sự thật. Trình tự`KK`được phép vì hạn chế cấm ba bàn thắng liên tiếp chứ không phải hai bàn. Thuật toán in`YES`. 

Bây giờ hãy xem xét:```
1
0 3 0 3
```Đây:```
big = 3
small = 0
```Và```
3 <= 2
```là sai. Trình tự duy nhất có thể là`KKK`, trong đó có ba bàn thắng liên tiếp của cùng một đội. Thuật toán in chính xác`NO`. 

Một trường hợp tế nhị khác là:```
1
4 1 4 1
```Số đếm ở hiệp hai là:```
(0, 0)
```Điều kiện được giữ ngay lập tức vì cả hai số đếm đều bằng 0. Vì các nửa độc lập nên nửa sau trống là hoàn toàn hợp lệ. Đầu ra của thuật toán`YES`. 

Cuối cùng, hãy xem xét:```
1
1 1 1 4
```Nửa đầu thỏa mãn điều kiện. Nửa sau đóng góp`(0, 3)`, thất bại vì`3 > 2`. Chỉ nhìn vào điểm cuối cùng sẽ bỏ sót vấn đề này, nhưng việc kiểm tra từng hiệp riêng biệt sẽ mang lại kết quả chính xác`NO`.
