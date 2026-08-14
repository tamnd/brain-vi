---
title: "CF 102307A - Amazon"
description: "Chúng tôi được cho một số cặp điểm. Mỗi cặp xác định một đường tàu điện ngầm thẳng vô hạn đi qua hai điểm đó. Đoạn thực tế giữa các điểm không liên quan vì tuyến tàu điện ngầm kéo dài vô tận."
date: "2026-08-13T23:33:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "A"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 66
verified: true
draft: false
---

[CF 102307A - Amazon](https://codeforces.com/problemset/problem/102307/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cho một số cặp điểm. Mỗi cặp xác định một đường tàu điện ngầm thẳng vô hạn đi qua hai điểm đó. Đoạn thực tế giữa các điểm không liên quan vì tuyến tàu điện ngầm kéo dài vô tận. 

Hai tuyến tàu điện ngầm khác nhau tạo ra một giao điểm mạnh ngay khi chúng vuông góc. Cùng một đường hình học có thể được mô tả bằng nhiều cặp đầu vào và những mô tả đó phải được tính là một đường tàu điện ngầm. Nhiệm vụ là đếm các cặp đường hình học phân biệt có hướng tạo thành một góc vuông. 

Đầu vào chứa tối đa (10^5) cặp trong một trường hợp thử nghiệm, với tối đa 100 trường hợp thử nghiệm. Các tọa độ được giới hạn bởi (2\cdot10^4), nhưng kết quả có thể lớn hơn nhiều so với phạm vi tọa độ. Với (10^5) dòng, việc kiểm tra từng cặp sẽ yêu cầu khoảng (10^{10}/2) so sánh, vượt xa giới hạn thời gian một giây. Chúng ta cần một nghiệm gần tuyến tính hoặc (O(n\log n)) về số cặp đầu vào. 

Có một số cách mà việc thực hiện bất cẩn có thể âm thầm đếm sai. Đầu tiên, các mô tả trùng lặp của cùng một dòng không được tạo thêm giao điểm. Ví dụ,```
1
3
0 0 2 0
5 0 -3 0
0 1 0 3
```chứa hai mô tả về đường ngang (y=0), cộng với đường dọc (x=0). Đầu ra đúng là`1`, vì chỉ có một đường ngang hình học và nó vuông góc với đường thẳng đứng. Việc đếm trực tiếp các cặp đầu vào sẽ tạo ra hai giao điểm không chính xác. 

Đảo ngược các điểm cuối cũng phải giữ nguyên dòng. Ví dụ,```
1
2
0 0 4 0
4 0 0 0
```mô tả cùng một dòng hai lần, vì vậy câu trả lời là`0`. Một biểu diễn dựa trên vectơ hướng thô sẽ thấy`(4, 0)`Và`(-4, 0)`khác nhau trừ khi hướng được chuẩn hóa. 

Các đường dọc và ngang cần xử lý đặc biệt nếu độ dốc được biểu thị bằng phép chia. Ví dụ,```
1
2
-20000 20000 20000 20000
20000 -20000 20000 20000
```mô tả (y=20000) và (x=20000), vuông góc với nhau, vì vậy câu trả lời là`1`. Việc sử dụng hệ số góc dấu phẩy động là không cần thiết và có thể gây ra các vấn đề về độ chính xác. Chúng ta có thể biểu diễn mọi dòng hoàn toàn bằng số nguyên. 

Cuối cùng, một số đường song song là các đường tàu điện ngầm khác nhau và phải được tính riêng khi tồn tại một đường vuông góc. Ví dụ,```
1
3
-2 0 2 0
-2 1 2 1
0 -2 0 2
```chứa hai đường ngang riêng biệt và một đường thẳng đứng. Câu trả lời đúng là`2`, vì đường thẳng đứng cắt cả hai đường ngang theo một góc vuông. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng tất cả các tuyến tàu điện ngầm và sau đó kiểm tra từng cặp trong số đó. Đối với mỗi cặp, chúng ta sẽ kiểm tra xem chúng có khác biệt hay không và liệu vectơ chỉ phương của chúng có tích vô hướng hay không. Điều này đúng vì mọi giao điểm mạnh đều tương ứng chính xác với một cặp đường vuông góc phân biệt. 

Vấn đề là số lượng so sánh. Với (n=10^5), có 

[ 
\frac{n(n-1)}2 \approx 5\cdot10^9 
] 

cặp. Ngay cả khi một phép so sánh chỉ thực hiện một vài phép tính số nguyên thì hàng tỷ phép so sánh cũng không thể đáp ứng đủ thời gian. 

Quan sát quan trọng là độ vuông góc chỉ phụ thuộc vào hướng của đường thẳng, trong khi các cặp đầu vào trùng lặp có thể được loại bỏ bằng cách xác định đường hình học hoàn chỉnh. Trước tiên, chúng ta có thể chuẩn hóa từng dòng và đặt nó thành một bộ. Sau khi loại bỏ các bản sao, chúng ta chỉ cần biết có bao nhiêu dòng riêng biệt có mỗi hướng. 

Một đường thẳng đi qua ((x_1,y_1)) và ((x_2,y_2)) có vectơ chỉ phương 

[ 
(dx,dy)=(x_2-x_1,y_2-y_1). 
] 

Để xác định cùng một hướng bất kể tỷ lệ hay hướng, hãy chia cho (\gcd(|dx|,|dy|)) và chọn một quy ước về dấu. Ví dụ: yêu cầu thành phần khác 0 đầu tiên phải dương. Như vậy`(4, 2)`,`(2, 1)`,`(-2, -1)`, Và`(-4, -2)`tất cả đều thể hiện cùng một hướng. 

Tuy nhiên, chỉ hướng là không đủ để xác định một đường thẳng, bởi vì các đường song song có thể khác nhau. Do đó chúng tôi đại diện cho dòng hoàn chỉnh như 

[ 
Ax+By+C=0, 
] 

ở đâu 

[ 
A=dy,\qquad B=-dx,\qquad C=dx,y_1-dy,x_1. 
] 

Chúng tôi chia cả ba hệ số cho gcd chung của chúng và chuẩn hóa dấu của chúng. Bộ ba kết quả`(A, B, C)`là một biểu diễn độc đáo của đường hình học. 

Khi mỗi đường phân biệt đã được biết, giả sử hướng kinh điển của nó là`(dx, dy)`. Hướng vuông góc là 

[ 
(-dy,dx). 
] 

Chúng tôi bình thường hóa hướng đó bằng cách sử dụng cùng một quy ước về dấu hiệu. Nếu như`cnt[d]`là số đường thẳng phân biệt có hướng`d`, thì tất cả các giao điểm liên quan đến hướng này và đường vuông góc của nó đều đóng góp 

[ 
cnt[d]\cdot cnt[perp]. 
] 

Chúng tôi chỉ xử lý mỗi cặp lớp hướng không có thứ tự một lần, bằng cách sử dụng so sánh thứ tự đơn giản giữa hai bộ dữ liệu hướng. 

Phương pháp brute-force hoạt động vì nó trực tiếp kiểm tra định nghĩa của giao điểm mạnh, nhưng nó thất bại vì về cơ bản nó lặp lại cùng một lý luận hình học cho hàng tỷ cặp. Nhận xét rằng câu trả lời chỉ phụ thuộc vào các đường duy nhất được nhóm theo hướng sẽ làm giảm vấn đề trong việc xây dựng một tập hợp và đếm các nhóm tương thích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log C+n)) dự kiến ​​| (O(n)) | Đã chấp nhận | 

Ở đây (C) biểu thị độ lớn của các hệ số nguyên. Với các giới hạn tọa độ đã cho, phép tính gcd thực sự có thời gian không đổi, do đó độ phức tạp thực tế là (O(n)) thời gian dự kiến ​​vì các bộ và từ điển Python cung cấp tính năng chèn và tra cứu (O(1)) dự kiến. 

## Hướng dẫn thuật toán 

1. Với mỗi cặp điểm đầu vào, hãy tính`dx = x2 - x1`Và`dy = y2 - y1`. Các giá trị này mô tả hướng của tuyến tàu điện ngầm mà không sử dụng số học dấu phẩy động. 
2. Xây dựng phương trình đường thẳng (Ax+By+C=0) bằng cách sử dụng`A = dy`,`B = -dx`, Và`C = dx*y1 - dy*x1`. Chia cả ba hệ số cho gcd của chúng và chuẩn hóa dấu chung của chúng. Lưu trữ bộ ba kết quả trong một bộ. Thao tác này sẽ loại bỏ các mô tả lặp lại của cùng một đường hình học, bao gồm các mô tả có điểm cuối đảo ngược. 
3. Đối với mỗi dòng mới được chèn, hãy chuẩn hóa vectơ chỉ hướng của nó`(dx, dy)`bằng cách chia cho`gcd(abs(dx), abs(dy))`. Lật cả hai thành phần nếu cần thiết để thành phần khác 0 đầu tiên là dương. Tăng tần số của hướng này. Chúng tôi chỉ tính hướng cho các tuyến riêng biệt vì hai cặp đầu vào mô tả cùng một tuyến chỉ được đóng góp cho một tuyến tàu điện ngầm. 
4. Sau khi tất cả các dòng đã được xử lý, hãy lặp lại theo mọi hướng`d = (dx, dy)`. Xây dựng phương vuông góc của nó như`(-dy, dx)`và bình thường hóa nó với cùng một quy ước. Số cặp vuông góc được biểu diễn bởi hai lớp hướng này là`cnt[d] * cnt[perp]`. 
5. Chỉ thêm sản phẩm này khi`d < perp`về mặt từ điển. Điều này làm cho mỗi cặp lớp hướng không có thứ tự xuất hiện chính xác một lần. Hướng của một đường thẳng không bao giờ có thể bằng hướng vuông góc của chính nó, do đó không có trường hợp tự ghép đặc biệt nào. 

### Tại sao nó hoạt động 

Sau khi chuẩn hóa, mỗi tuyến tàu điện ngầm hình học có chính xác một khóa tuyến, do đó tập hợp này chứa mỗi tuyến thực tế một lần. Đối với mỗi đường như vậy, hướng chuẩn hóa của nó xác định hướng của nó một cách độc lập với vị trí của nó và tọa độ được sử dụng để mô tả nó. Hai vectơ chỉ phương khác 0 vuông góc với nhau khi tích của chúng bằng 0 và`(dx,dy)`vuông góc với`(-dy,dx)`. Do đó, mỗi cặp đường tàu điện ngầm vuông góc riêng biệt thuộc về đúng một cặp lớp hướng`d`Và`perp`. Tích tần số đếm mọi sự kết hợp của một dòng từ mỗi lớp, trong khi điều kiện từ điển chỉ tính cặp lớp không có thứ tự đó một lần. Do đó tổng cuối cùng chính xác là số nút giao mạnh. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def normalize_direction(dx, dy):
    g = gcd(abs(dx), abs(dy))
    dx //= g
    dy //= g

    if dx < 0 or (dx == 0 and dy < 0):
        dx = -dx
        dy = -dy

    return dx, dy

def solve():
    t = int(input())
    answers = []

    for _ in range(t):
        n = int(input())

        lines = set()
        direction_count = {}

        for _ in range(n):
            x1, y1, x2, y2 = map(int, input().split())

            dx = x2 - x1
            dy = y2 - y1

            # The problem describes a line using two locations,
            # so the two locations are assumed to be distinct.
            A = dy
            B = -dx
            C = dx * y1 - dy * x1

            g = gcd(gcd(abs(A), abs(B)), abs(C))
            A //= g
            B //= g
            C //= g

            if A < 0 or (A == 0 and B < 0) or (
                A == 0 and B == 0 and C < 0
            ):
                A = -A
                B = -B
                C = -C

            line = (A, B, C)

            if line in lines:
                continue

            lines.add(line)

            direction = normalize_direction(dx, dy)
            direction_count[direction] = direction_count.get(direction, 0) + 1

        answer = 0

        for dx, dy in direction_count:
            perp = normalize_direction(-dy, dx)

            if (dx, dy) < perp:
                answer += direction_count.get(perp, 0) * direction_count[(dx, dy)]

        answers.append(str(answer))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```các`normalize_direction`Hàm loại bỏ hệ số chung khỏi vectơ chỉ phương, sau đó sửa hướng của nó. điều kiện`dx < 0 or (dx == 0 and dy < 0)`có nghĩa là các hướng ngang trở thành`(positive, 0)`và hướng thẳng đứng trở thành`(0, positive)`. Điều này đưa ra một biểu diễn cho mỗi hướng vô hướng. 

Phương trình đường thẳng sử dụng vectơ chỉ phương để xây dựng một vectơ pháp tuyến. Từ`A = dy`Và`B = -dx`, vectơ`(A, B)`vuông góc với đường thẳng. Hằng số`C`được chọn sao cho điểm đầu vào đầu tiên thỏa mãn phương trình. Việc chia cả ba hệ số cho gcd chung của chúng sẽ loại bỏ việc chia tỷ lệ tùy ý. 

Việc chuẩn hóa dấu hiệu của`(A, B, C)`là cần thiết bởi vì cả hai 

[ 
Ax+By+C=0 
] 

và 

[ 
-Ax-By-C=0 
] 

mô tả cùng một dòng. Nếu không có quy tắc dấu, các dòng giống hệt nhau có thể chiếm hai mục nhập khác nhau. 

Dòng được chèn vào`lines`trước khi hướng của nó được tính. Thứ tự này là cần thiết. Nếu đường này đã xuất hiện thì mô tả thứ hai của nó không được tăng tần số hướng. 

Câu trả lời sử dụng số nguyên Python, do đó không có vấn đề tràn mặc dù số lượng giao điểm tối đa có thể đạt tới 

[ 
\frac{10^5(10^5-1)}2=4.999.950.000. 
] 

Việc so sánh từ điển cuối cùng ngăn chặn việc tính hai lần. Ví dụ, nếu`(1, 0)`vuông góc với`(0, 1)`, xử lý`(1, 0)`đếm sản phẩm trong khi chế biến`(0, 1)`không làm gì cả vì`(0, 1) < (1, 0)`là sai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Ca kiểm thử đầu tiên chứa các dòng (y=2), (x=3) và (y=-3). 

| Dòng đầu vào | Hướng kinh điển | Dòng | Đếm hướng sau khi chèn | Trả lời | 
| --- | --- | --- | --- | --- | 
|`-3 2 2 2`|`(1, 0)`| (y=2) |`(1,0): 1`| 0 | 
|`3 1 3 -3`|`(0, 1)`| (x=3) |`(1,0): 1`,`(0,1): 1`| 0 | 
|`-3 -3 -1 -3`|`(1, 0)`| (y=-3) |`(1,0): 2`,`(0,1): 1`| 0 | 

Hướng ngang`(1,0)`vuông góc với phương thẳng đứng`(0,1)`. Tần số của chúng là 2 và 1, cho ra (2\cdot1=2). Do đó, đầu ra là`2`. Ví dụ này cũng giải thích tại sao các đường song song phải được tách biệt sau khi loại bỏ các đường trùng lặp. 

### Mẫu 2 

Ba đường thẳng có vectơ chỉ phương tỉ lệ với`(-6, 9)`,`(6, 4)`, Và`(-4, 2)`. 

| Dòng đầu vào | Hướng chuẩn hóa | Hướng vuông góc | Số hướng | Đã thêm | 
| --- | --- | --- | --- | --- | 
|`2 -2 -4 7`|`(2, -3)`|`(3, 2)`| 1 | 0 ban đầu | 
|`0 -2 6 2`|`(3, 2)`|`(-2, 3)`→`(2, -3)`| 1 | 1 | 
|`4 -2 0 0`|`(2, -1)`|`(1, 2)`| 1 | 0 | 

Đường thẳng thứ nhất và thứ hai vuông góc vì vectơ chỉ hướng ban đầu của chúng có tích vô hướng 

[ 
(-6)\cdot6+9\cdot4=0. 
] 

Hướng thứ ba không có đối tác vuông góc nào trong ba đường thẳng. Kết quả là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) dự kiến ​​| Mỗi dòng sử dụng một số lượng không đổi các phép toán gcd và tập hợp (O(1)) hoặc các phép toán từ điển dự kiến, theo sau là một lần chuyển qua các lớp hướng riêng biệt. | 
| Không gian | (O(n)) | Tập hợp các dòng chuẩn và từ điển hướng-tần số, mỗi dòng chứa tối đa (n) mục. | 

Đối với (n=10^5), thuật toán thực hiện khoảng một vài thao tác bảng băm trên mỗi dòng đầu vào thay vì vài tỷ lần kiểm tra theo cặp. Các giới hạn tọa độ cũng giữ cho các hệ số nguyên đủ nhỏ để số học có độ chính xác tùy ý của Python ở đây không tốn kém. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def normalize_direction(dx, dy):
    g = gcd(abs(dx), abs(dy))
    dx //= g
    dy //= g

    if dx < 0 or (dx == 0 and dy < 0):
        dx = -dx
        dy = -dy

    return dx, dy

def solve(data):
    it = iter(data.strip().split())
    t = int(next(it))
    out = []

    for _ in range(t):
        n = int(next(it))

        lines = set()
        direction_count = {}

        for _ in range(n):
            x1 = int(next(it))
            y1 = int(next(it))
            x2 = int(next(it))
            y2 = int(next(it))

            dx = x2 - x1
            dy = y2 - y1

            A = dy
            B = -dx
            C = dx * y1 - dy * x1

            g = gcd(gcd(abs(A), abs(B)), abs(C))
            A //= g
            B //= g
            C //= g

            if A < 0 or (A == 0 and B < 0) or (
                A == 0 and B == 0 and C < 0
            ):
                A = -A
                B = -B
                C = -C

            line = (A, B, C)

            if line in lines:
                continue

            lines.add(line)

            d = normalize_direction(dx, dy)
            direction_count[d] = direction_count.get(d, 0) + 1

        ans = 0

        for d in direction_count:
            dx, dy = d
            p = normalize_direction(-dy, dx)

            if d < p:
                ans += direction_count[d] * direction_count.get(p, 0)

        out.append(str(ans))

    return "\n".join(out)

# Provided samples
sample_input = """\
3
3
-3 2 2 2
3 1 3 -3
-3 -3 -1 -3
3
2 -2 -4 7
0 -2 6 2
4 -2 0 0
2
0 -1 -6 1
2 5 -3 0
"""

assert solve(sample_input) == "2\n1\n0", "provided samples"

# Minimum-size input: one line cannot have an intersection.
assert solve("""\
1
1
0 0 1 1
""") == "0", "minimum size"

# Duplicate descriptions of the same line must count once.
assert solve("""\
1
3
0 0 4 0
4 0 0 0
-2 0 2 0
""") == "0", "duplicate line descriptions"

# Horizontal and vertical boundary-coordinate lines are perpendicular.
assert solve("""\
1
2
-20000 20000 20000 20000
20000 -20000 20000 20000
""") == "1", "boundary coordinates"

# Three lines: two horizontal and one vertical, giving two intersections.
assert solve("""\
1
3
-2 0 2 0
-2 1 2 1
0 -2 0 2
""") == "2", "multiple parallel lines"

# Maximum n, but every input pair describes the same geometric line.
max_case = ["1", "100000"]
max_case.extend(["0 0 20000 0"] * 100000)
assert solve("\n".join(max_case)) == "0", "maximum n"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 0 0 1 1`|`0`| Đầu vào kích thước tối thiểu | 
| Ba bản sao của`y=0`|`0`| Loại bỏ dòng trùng lặp | 
|`y=20000`Và`x=20000`|`1`| Ranh giới dọc, ngang và tọa độ | 
| Hai đường ngang và một đường dọc |`2`| Nhiều đường thẳng song song khác biệt | 
| 100000 bản cùng dòng |`0`| Kích thước đầu vào tối đa và xử lý trùng lặp hiệu quả | 

## Vỏ cạnh 

### Mô tả trùng lặp trên một dòng 

cho```
1
3
0 0 4 0
4 0 0 0
-2 0 2 0
```cả ba cặp đều mô tả (y=0). Phương trình đường chuẩn hóa của chúng là bộ ba chính tắc giống nhau, nên chỉ có một mục đạt tới`direction_count`. Không có hướng vuông góc và đầu ra là`0`. Giải pháp đếm chỉ đường trước khi loại bỏ các đường trùng lặp sẽ cho rằng có ba đường ngang không chính xác. 

### Đảo ngược điểm cuối 

cho```
1
2
0 0 4 0
4 0 0 0
```hướng đầu tiên là`(4,0)`và thứ hai là`(-4,0)`. Cả hai bình thường hóa thành`(1,0)`. Quan trọng hơn, cả hai đều tạo ra cùng một phương trình đường chuẩn, do đó cặp đầu vào thứ hai bị loại bỏ dưới dạng trùng lặp. Câu trả lời là`0`. 

### Đường dọc và đường ngang 

cho```
1
2
-20000 20000 20000 20000
20000 -20000 20000 20000
```dòng đầu tiên nằm ngang theo hướng`(1,0)`, trong khi cái thứ hai thẳng đứng với hướng`(0,1)`. Tra cứu vuông góc của`(1,0)`sản xuất`(0,1)`và mỗi hướng có tần số bằng một. Sản phẩm là`1`, đó là câu trả lời đúng. 

### Nhiều đường thẳng song song 

cho```
1
3
-2 0 2 0
-2 1 2 1
0 -2 0 2
```hai dòng đầu tiên bình thường hóa theo cùng một hướng`(1,0)`nhưng có khác nhau`C`các giá trị trong phương trình đường của chúng, vì vậy cả hai vẫn giữ nguyên tập hợp. Đường thẳng đứng có hướng`(0,1)`. Tần số hướng là 2 và 1, tạo ra (2\cdot1=2). Thuật toán tính cả hai giao điểm vật lý mặc dù các đường ngang song song với nhau. 

### Câu trả lời lớn 

Nếu có nhiều đường ngang và nhiều đường thẳng đứng khác nhau thì mọi đường ngang đều vuông góc với mọi đường thẳng đứng. Câu trả lời có thể đạt tới (n^2/4), tức là hàng tỷ cho (n=10^5). Việc triển khai sử dụng số nguyên Python, do đó kết quả được biểu diễn chính xác mà không bị tràn. 

### Đầu vào tối đa có hình dạng giống hệt nhau 

Với 100000 bản sao của cùng một cặp, bộ dòng vẫn có kích thước bằng một mặc dù đầu vào lớn. Mỗi cặp tiếp theo bị từ chối bởi quá trình tra cứu đã thiết lập và câu trả lời cuối cùng là 0. Trường hợp này cũng là một kiểm tra thực tế hữu ích để đảm bảo rằng giải pháp không vô tình thực hiện công việc tỷ lệ thuận với số lượng cặp mô tả đầu vào.
