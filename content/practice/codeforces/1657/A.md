---
title: "CF 1657A - Di chuyển số nguyên"
description: "Chúng ta bắt đầu với một con chip ở gốc (0, 0) và muốn đạt đến điểm mục tiêu (x, y). Được phép di chuyển bất cứ khi nào khoảng cách Euclide giữa vị trí hiện tại và vị trí tiếp theo là một số nguyên. Nhiệm vụ là tìm số bước di chuyển tối thiểu cần thiết để tiếp cận mục tiêu."
date: "2026-06-10T03:27:33+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1657
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 125 (Rated for Div. 2)"
rating: 800
weight: 1657
solve_time_s: 99
verified: true
draft: false
---

[CF 1657A - Di chuyển số nguyên](https://codeforces.com/problemset/problem/1657/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một con chip ở điểm gốc`(0, 0)`và muốn đạt được điểm mục tiêu`(x, y)`. 

Được phép di chuyển bất cứ khi nào khoảng cách Euclide giữa vị trí hiện tại và vị trí tiếp theo là một số nguyên. Nhiệm vụ là tìm số bước di chuyển tối thiểu cần thiết để tiếp cận mục tiêu. 

Các tọa độ rất nhỏ, nhiều nhất`50`, và có nhiều nhất`3000`trường hợp thử nghiệm. Ngay cả một giải pháp hơi kém hiệu quả cũng có thể phù hợp một cách thoải mái trong giới hạn. Tuy nhiên, vấn đề có một quan sát toán học đơn giản giúp giảm mọi thứ về thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Đại lượng quan trọng là khoảng cách từ điểm xuất phát đến điểm đến:$$d = \sqrt{x^2 + y^2}$$Nếu khoảng cách này đã là số nguyên thì chúng ta có thể di chuyển trực tiếp từ`(0,0)`ĐẾN`(x,y)`trong một thao tác. 

Một trường hợp đặc biệt xuất hiện khi đích chính là điểm gốc. Nếu như`(x,y) = (0,0)`, không cần chuyển động nên đáp án là`0`. 

Sự tinh tế chính là hiểu điều gì xảy ra khi khoảng cách không phải là số nguyên. Một giải pháp bất cẩn có thể cho rằng đôi khi có thể cần nhiều hơn hai nước đi. Ví dụ: 

đầu vào:```
1
9 15
```Khoảng cách là$$\sqrt{9^2+15^2}=\sqrt{306}$$đó không phải là số nguyên. Chuyển động trực tiếp là không thể, nhưng câu trả lời đúng vẫn là`2`, không phải một số lớn hơn. 

Một sai lầm dễ dàng khác là quên trường hợp xuất xứ: 

đầu vào:```
1
0 0
```Đầu ra:```
0
```Khoảng cách là một số nguyên (`0`), nhưng số lần di chuyển tối thiểu thì không`1`, bởi vì chúng ta đã đến đích rồi. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là tìm kiếm các điểm trung gian. Nếu không thể đến đích trực tiếp, chúng ta có thể thử mọi điểm mạng`(a,b)`và kiểm tra xem cả hai khoảng cách$$\sqrt{a^2+b^2}$$Và$$\sqrt{(x-a)^2+(y-b)^2}$$là số nguyên. 

Bởi vì tọa độ nhiều nhất là`50`, việc tìm kiếm như vậy thực sự sẽ đủ nhanh ở đây. Chúng tôi sẽ kiểm tra vài nghìn điểm ứng viên cho mỗi trường hợp thử nghiệm và dừng lại khi tìm thấy điểm trung gian hợp lệ. 

Ý tưởng vũ lực hoạt động vì tọa độ rất nhỏ nhưng nó hoàn toàn bỏ sót cấu trúc cơ bản. 

Quan sát quan trọng là mọi điểm không xuất phát luôn có thể đạt được sau nhiều nhất hai nước đi. 

Giả sử mục tiêu là`(x,y)`và khoảng cách của nó tới gốc không phải là số nguyên. Chúng ta không thể đạt được nó trong một bước. 

Tuy nhiên, trước tiên chúng ta có thể chuyển từ`(0,0)`ĐẾN`(0,0)`cộng với một số điểm ở khoảng cách nguyên từ điểm gốc, ví dụ`(x,0)`nếu như`x > 0`, hoặc bất kỳ điểm mạng phù hợp nào khác. Tổng quát hơn, một thực tế hình học mạnh mẽ hơn được khẳng định: mọi điểm trong mặt phẳng đều có thể được biểu diễn dưới dạng tổng của hai vectơ có độ dài là số nguyên. Đối với các điểm mạng, hai lần di chuyển luôn là đủ. 

Vì một nước đi có thể thực hiện được chính xác khi khoảng cách từ gốc tọa độ là một số nguyên, và nếu không thì hai nước đi là đủ, nên chỉ có thể xảy ra ba câu trả lời:`0`,`1`, hoặc`2`. 

Điều đó biến vấn đề thành một sự phân loại đơn giản dựa trên`x² + y²`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C²) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

Đây`C`biểu thị tọa độ giới hạn. 

## Hướng dẫn thuật toán 

1. Đọc tọa độ mục tiêu`(x, y)`. 
2. Nếu`x == 0`Và`y == 0`, đầu ra`0`. 

Chúng ta đã đến đích rồi nên không cần di chuyển nữa. 
3. Tính toán$$s = x^2 + y^2$$Khoảng cách từ gốc là`√s`. 
4. Kiểm tra xem`s`là một hình vuông hoàn hảo 

Nếu vậy thì`√s`là một số nguyên, do đó một bước chuyển trực tiếp từ`(0,0)`ĐẾN`(x,y)`là hợp pháp. 
5. Nếu`s`là một hình vuông hoàn hảo, đầu ra`1`. 
6. Ngược lại, xuất ra`2`. 

Một nước đi là không thể vì khoảng cách không phải là số nguyên và hai nước đi luôn là đủ. 

### Tại sao nó hoạt động 

Câu trả lời không bao giờ có thể vượt quá`2`. 

Nếu đích đến là điểm xuất phát thì mức tối thiểu rõ ràng là`0`. 

Đối với bất kỳ điểm nào khác, nếu`x² + y²`là một hình vuông hoàn hảo thì khoảng cách từ`(0,0)`ĐẾN`(x,y)`là một số nguyên. Có một động thái trực tiếp tồn tại, vì vậy câu trả lời là`1`. 

Nếu như`x² + y²`không phải là một hình vuông hoàn hảo, việc di chuyển trực tiếp là không thể. Thuộc tính hình học đã biết được sử dụng trong lời giải chính thức là mọi điểm mạng đều có thể đạt được sau hai lần di chuyển có độ dài nguyên. Vì một nước đi không thể thực hiện được và hai nước đi luôn có thể thực hiện được nên mức tối thiểu chính xác là`2`. 

Ba trường hợp này loại trừ lẫn nhau và bao gồm tất cả các đầu vào, do đó thuật toán đúng. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

t = int(input())

for _ in range(t):
    x, y = map(int, input().split())

    if x == 0 and y == 0:
        print(0)
        continue

    s = x * x + y * y
    r = isqrt(s)

    if r * r == s:
        print(1)
    else:
        print(2)
```Nhánh đầu tiên xử lý nguồn gốc. Không có nó,`(0,0)`sẽ bị phân loại không chính xác là cần một nước đi vì`0`là một hình vuông hoàn hảo 

Biến`s`lưu trữ khoảng cách bình phương. Làm việc với khoảng cách bình phương sẽ tránh hoàn toàn số học dấu phẩy động.`isqrt(s)`trả về số nguyên sàn của`√s`. Một số là số chính phương chính xác khi`isqrt(s)^2 == s`. 

Khi chúng ta biết khoảng cách có phải là số nguyên hay không, câu trả lời sẽ được rút ra trực tiếp từ cách phân loại ba trường hợp được mô tả trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 6
```| Bước | x | y | s = x2+y2 | isqrt(các) | Hình vuông hoàn hảo? | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 8 | 6 | 100 | 10 | Có | 1 | 

Bình phương khoảng cách là`100`, đó là`10²`. Khoảng cách là một số nguyên nên một bước di chuyển trực tiếp sẽ đến được mục tiêu. 

### Ví dụ 2 

đầu vào:```
9 15
```| Bước | x | y | s = x2+y2 | isqrt(các) | Hình vuông hoàn hảo? | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 9 | 15 | 306 | 17 | Không | 2 | 

Bình phương khoảng cách là`306`. Từ`17² = 289`Và`18² = 324`, nó không phải là một hình vuông hoàn hảo. 

Một động thái trực tiếp là không thể, vì vậy câu trả lời trở thành`2`. 

Ví dụ này thể hiện sự khác biệt chính giữa "khoảng cách tồn tại" và "khoảng cách là số nguyên". Chỉ cái sau mới cho phép di chuyển một lần. 

### Ví dụ 3 

đầu vào:```
0 0
```| Bước | x | y | Trường hợp đặc biệt? | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 0 | 0 | Có | 0 | 

Điều này khẳng định việc xử lý đặc biệt về nguồn gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Số lượng số học không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Ngay cả với`3000`trường hợp thử nghiệm, chương trình chỉ thực hiện vài nghìn phép tính số học. Thời gian chạy và mức sử dụng bộ nhớ thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isqrt

def solve():
    input = sys.stdin.readline
    t = int(input())

    ans = []
    for _ in range(t):
        x, y = map(int, input().split())

        if x == 0 and y == 0:
            ans.append("0")
            continue

        s = x * x + y * y
        r = isqrt(s)

        ans.append("1" if r * r == s else "2")

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run(
"""3
8 6
0 0
9 15
"""
) == "1\n0\n2"

# origin
assert run(
"""1
0 0
"""
) == "0"

# classic Pythagorean triple
assert run(
"""1
3 4
"""
) == "1"

# non-perfect-square distance
assert run(
"""1
1 1
"""
) == "2"

# maximum coordinates
assert run(
"""1
50 50
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| Đã tới đích | 
|`3 4`|`1`| Khoảng cách số nguyên qua bộ ba Pythagore | 
|`1 1`|`2`| Khoảng cách không vuông cần hai lần di chuyển | 
|`50 50`|`2`| Tọa độ lớn nhất trong ràng buộc | 

## Vỏ cạnh 

Hãy coi đích đến đã là điểm xuất phát. 

đầu vào:```
1
0 0
```Thuật toán đi vào nhánh trường hợp đặc biệt ngay lập tức và xuất ra`0`. Điều này tránh được sai lầm thường gặp khi xử lý khoảng cách`0`dưới dạng khoảng cách số nguyên thông thường và trả về`1`. 

Xét một điểm có khoảng cách là một số nguyên. 

đầu vào:```
1
24 7
```Bình phương khoảng cách là$$24^2+7^2=576+49=625$$Từ`625 = 25²`, đầu ra của thuật toán`1`. Một sự di chuyển trực tiếp của chiều dài`25`là hợp pháp. 

Xét một điểm có khoảng cách không phải là số nguyên. 

đầu vào:```
1
1 1
```Bình phương khoảng cách là`2`, đây không phải là một hình vuông hoàn hảo. Thử nghiệm bình phương hoàn hảo không thành công và thuật toán đưa ra`2`. 

Điều này hoàn toàn chính xác vì một nước đi là không thể trong khi hai nước đi luôn là đủ. 

Hãy xem xét một đầu vào giá trị biên. 

đầu vào:```
1
50 50
```Bình phương khoảng cách là$$50^2+50^2=5000$$

`5000`không phải là một hình vuông hoàn hảo, do đó thuật toán đưa ra`2`. Kích thước của tọa độ không thay đổi lý do, cho thấy giải pháp xử lý toàn bộ phạm vi cho phép một cách thống nhất.
