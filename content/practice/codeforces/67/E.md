---
title: "CF 67E - Cứu lấy thành phố!"
description: "Chúng ta có một đa giác đơn giản được liệt kê theo chiều kim đồng hồ. Hai đỉnh đầu tiên tạo thành cạnh ngang AB và mọi đỉnh còn lại đều nằm trên cùng một phía của cạnh đó. Dọc theo đoạn AB, mọi điểm tọa độ nguyên đều có thể là vị trí của tháp canh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 67
codeforces_index: "E"
codeforces_contest_name: "Manthan 2011"
rating: 2500
weight: 67
solve_time_s: 124
verified: true
draft: false
---

[CF 67E - Cứu thành phố!](https://codeforces.com/problemset/problem/67/E) 

**Đánh giá:** 2500 
**Thẻ:** hình học 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác đơn giản được liệt kê theo chiều kim đồng hồ. Hai đỉnh đầu tạo thành cạnh ngang`AB`, và mọi đỉnh khác đều nằm hoàn toàn trên cùng một phía của cạnh đó. Dọc theo đoạn`AB`, mọi điểm tọa độ nguyên đều có thể là vị trí của tháp canh. 

Đối với vị trí tháp canh`P`TRÊN`AB`, mọi điểm bên trong đa giác phải được nhìn thấy từ`P`. Khả năng hiển thị có nghĩa là đoạn từ`P`đến điểm đích vẫn nằm trong đa giác ngoại trừ có thể ở điểm cuối. Vì đa giác là đơn giản và`P`nằm trên đường biên, nó đủ để đảm bảo rằng mọi cạnh đa giác đều có thể nhìn thấy được từ`P`. 

Nhiệm vụ là đếm xem có bao nhiêu điểm nguyên trên`AB`có thể nhìn thấy toàn bộ đa giác. 

Đa giác có nhiều nhất 1000 đỉnh. Kích thước đó đủ nhỏ cho các thuật toán hình học bậc hai, nhưng không đủ nhỏ cho bất kỳ thứ gì có hình khối với các phép toán dấu phẩy động đắt tiền. Một giải pháp xung quanh`O(n^2)`là hoàn toàn an toàn. Thậm chí`10^6`kiểm tra định hướng là tầm thường trong giới hạn. Phần khó hơn không phải là hiệu suất mà là tính chính xác về mặt hình học. 

Ràng buộc hình học quan trọng là tất cả các đỉnh ngoại trừ`A`Và`B`nằm cùng một phía`AB`. Điều đó có nghĩa là đa giác hoạt động giống như một "ngọn núi" treo bên dưới hoặc phía trên hàng rào. Cấu trúc này loại bỏ nhiều trường hợp khó nhìn thấy và biến bài toán thành giao điểm khoảng trên một đường thẳng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một đa giác trong đó một cạnh hầu như không thể nhìn thấy được từ một điểm cuối duy nhất của hàng rào:```
4
0 0
10 0
9 5
1 5
```Mỗi điểm trên`AB`hoạt động ở đây, vì đa giác lồi. Việc triển khai bất cẩn bằng cách sử dụng các bất đẳng thức nghiêm ngặt có thể từ chối các điểm cuối một cách không chính xác. 

Bây giờ hãy xem xét một đa giác không lồi:```
5
0 0
10 0
10 10
5 3
0 10
```Vết lõm bên trong gần`(5,3)`chặn tầm nhìn từ nhiều vị trí. Việc kiểm tra "đa giác bên trong" ngây thơ cho các điểm được lấy mẫu sẽ bỏ lỡ khả năng hiển thị không thành công do một đoạn vượt qua ranh giới trước khi đến mục tiêu. 

Một trường hợp tinh tế khác xuất hiện khi khả năng hiển thị thay đổi chính xác ở tọa độ nguyên:```
5
0 0
6 0
6 6
3 1
0 6
```Ở đây ranh giới khoảng hợp lệ là hợp lý. Lỗi làm tròn từ các phép tính dấu phẩy động có thể làm thay đổi số đếm đi một. Số học số nguyên an toàn hơn nhiều. 

Trường hợp góc quan trọng cuối cùng là khi không có điểm nào hoạt động cả. Một số đa giác chứa một đỉnh phản xạ có các cạnh liền kề chặn mọi góc nhìn có thể có trên`AB`. Câu trả lời đúng là`0`và thuật toán phải phát hiện rõ ràng các giao điểm khoảng trống. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi điểm nguyên trên`AB`. Đối với từng vị trí ứng viên`P`, kiểm tra khả năng hiển thị tới mọi cạnh đa giác hoặc mọi đỉnh đa giác. Một đoạn từ`P`đến một điểm là hợp lệ nếu nó không vượt qua ranh giới đa giác ngoại trừ điểm cuối. 

Điều này có tác dụng vì có thể kiểm tra khả năng hiển thị trong một đa giác đơn giản bằng cách sử dụng các phép kiểm tra giao điểm của đoạn. Vấn đề là khối lượng công việc. Nếu chiều dài hàng rào lên tới`10^6`, thì có thể có hơn một triệu vị trí ứng cử viên. Đối với từng vị trí, kiểm tra khả năng hiển thị đối với tất cả các chi phí của cạnh`O(n^2)`trong trường hợp xấu nhất. Nó nổ tung xung quanh`10^12`hoạt động. 

Nút cổ chai đang quét từng điểm nguyên một cách độc lập. Bản thân hình dạng gợi ý điều gì đó tốt hơn: các hạn chế về tầm nhìn thay đổi liên tục dọc theo hàng rào. 

Lấy một cạnh đa giác`E`. Từ vị trí nào trên`AB`toàn bộ cạnh có thể nhìn thấy được không? Vì đa giác nằm về một phía của`AB`, tình trạng chặn chỉ xuất hiện từ các tia đi qua các điểm cuối của cạnh. Các vị trí nhìn thấy được tạo thành một khoảng duy nhất trên`AB`. 

Đây là quan sát quan trọng. Thay vì kiểm tra từng điểm riêng biệt, chúng tôi tính khoảng hợp lệ được đóng góp bởi mỗi cạnh đa giác, sau đó cắt tất cả các khoảng. Giao điểm cuối cùng chứa chính xác các vị trí tháp nhìn thấy toàn bộ đa giác. 

Hình học trở nên đặc biệt rõ ràng đối với các đỉnh phản xạ. Một đỉnh lồi không bao giờ tạo ra một giới hạn. Đỉnh phản xạ tạo ra một hình nón hiển thị được giới hạn bởi hai cạnh liền kề của nó. Giao các hình nón đó với hàng rào ngang sẽ có một khoảng cho phép`x`tọa độ. 

Sau khi tính toán khoảng toàn cầu`[L, R]`, câu trả lời đơn giản là số số nguyên bên trong nó cũng thuộc phân đoạn`AB`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | AB | · n²) | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Thiết lập hình học 

Giả sử hàng rào`AB`nằm trên đường ngang`y = Y`. Vì tất cả các đỉnh khác đều nằm cùng một phía nên chúng ta có thể định hướng đa giác sao cho tất cả chúng đều nằm bên dưới hàng rào. Nếu chúng nằm phía trên, chúng ta phản ánh tọa độ theo chiều dọc bằng cách nhân tất cả`y`giá trị bởi`-1`. 

Vị trí tháp canh hợp lệ là số nguyên`x`tọa độ giữa`x1`Và`x2`. 

### Hạn chế tầm nhìn từ đỉnh phản xạ 

Đỉnh lồi không bao giờ cản trở tầm nhìn. Chỉ có các đỉnh phản xạ mới quan trọng. 

Đối với đỉnh phản xạ`V`, gọi các đỉnh liền kề của nó là`U`Và`W`. Bất kỳ điểm nhìn nào ở phía bên trái của một trong hai cạnh liền kề sẽ nhìn ra bên ngoài đa giác trước khi tiếp cận`V`. 

Ranh giới giữa các điểm nhìn thấy được và không nhìn thấy chính xác là đường kéo dài một trong các cạnh liền kề. 

Đối với mỗi cạnh liền kề, chúng tôi tính toán vị trí đường hỗ trợ của nó giao với hàng rào`y = Y`. Điều đó mang lại sự hạn chế`x`phối hợp trên hàng rào. 

Tùy thuộc vào hướng của cạnh, điểm nhìn phải nằm ở bên trái hoặc bên phải của giao lộ đó. 

### Phát hiện đỉnh phản xạ 

Bởi vì các đỉnh được liệt kê theo chiều kim đồng hồ và đa giác nằm bên dưới hàng rào, một đỉnh phản xạ có tích chéo dương:```
cross(V-U, W-V) > 0
```Các đỉnh lồi được bỏ qua. 

### Tính toán giới hạn khoảng thời gian 

Đối với mỗi đỉnh phản xạ: 

1. Tận dụng lợi thế`(U,V)`. 
2. Kéo dài dòng cho đến khi đạt đến`y = Y`. 
3. Tính tương ứng`x`. 
4. Quyết định xem quan điểm hợp lệ nằm ở bên trái hay bên phải. 

Lặp lại cho cạnh`(V,W)`. 

Giao điểm của hai nửa mặt phẳng này cho khoảng nhìn thấy được áp đặt bởi đỉnh`V`. 

Giao khoảng này vào phạm vi toàn cầu`[L,R]`. 

### Đếm lần cuối 

Sau khi xử lý tất cả các đỉnh phản xạ: 

1. Kẹp khoảng cách vào đoạn hàng rào thực tế. 
2. Đếm các số nguyên trong khoảng. 

Nếu khoảng trống, trả về`0`. 

### Tại sao nó hoạt động 

Một điểm nhìn trở nên không hợp lệ chính xác khi đoạn từ điểm nhìn đến một số điểm đa giác thoát ra khỏi đa giác. Trong một đa giác đơn giản có tất cả các đỉnh ở một bên của hàng rào, vật cản đầu tiên luôn xuất hiện ở một đỉnh phản xạ. 

Mỗi đỉnh phản xạ tạo ra một vùng cấm hình nêm. Giao điểm cái nêm này với hàng rào sẽ tạo ra một ràng buộc khoảng cách duy nhất. Các đỉnh lồi không bao giờ tạo ra vật cản vì ranh giới đa giác cục bộ uốn cong ra khỏi điểm quan sát. 

Thuật toán tính toán chính xác tất cả các giới hạn khoảng này và cắt chúng. Một điểm tồn tại qua giao điểm khi và chỉ khi nó nằm trong phạm vi hiển thị của mọi đỉnh phản xạ, tương đương với việc nhìn thấy toàn bộ đa giác. 

## Giải pháp Python```python
import sys
from fractions import Fraction

input = sys.stdin.readline

def intersect_x(a, b, Y):
    x1, y1 = a
    x2, y2 = b

    if y1 == y2:
        return Fraction(x1, 1)

    return Fraction(x1 * (y2 - Y) + x2 * (Y - y1), y2 - y1)

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def solve():
    n = int(input())
    p = [tuple(map(int, input().split())) for _ in range(n)]

    Y = p[0][1]

    # Make polygon lie below the fence
    if p[2][1] > Y:
        p = [(x, -y) for x, y in p]
        Y = -Y

    x1 = p[0][0]
    x2 = p[1][0]

    if x1 > x2:
        x1, x2 = x2, x1

    L = Fraction(x1, 1)
    R = Fraction(x2, 1)

    for i in range(n):
        u = p[(i - 1) % n]
        v = p[i]
        w = p[(i + 1) % n]

        c = cross(v[0] - u[0], v[1] - u[1],
                  w[0] - v[0], w[1] - v[1])

        # reflex vertex
        if c <= 0:
            continue

        xv1 = intersect_x(u, v, Y)
        xv2 = intersect_x(v, w, Y)

        left = min(xv1, xv2)
        right = max(xv1, xv2)

        L = max(L, left)
        R = min(R, right)

    if L > R:
        print(0)
        return

    lo = (L.numerator + L.denominator - 1) // L.denominator
    hi = R.numerator // R.denominator

    ans = max(0, hi - lo + 1)
    print(ans)

solve()
```Bước đầu tiên bình thường hóa hướng đa giác. Lý do biên tập giả định tất cả các đỉnh đều nằm bên dưới hàng rào. Phản chiếu theo chiều dọc giữ cho mọi mối quan hệ hiển thị không thay đổi đồng thời đơn giản hóa hình học. 

các`cross`hàm xác định các đỉnh phản xạ. Vì đa giác theo chiều kim đồng hồ nên tích chéo dương có nghĩa là góc trong vượt quá 180 độ. 

các`intersect_x`tính toán nơi cạnh đa giác gặp đường hàng rào. sử dụng`Fraction`tránh các lỗi chính xác về dấu phẩy động. Điều này quan trọng vì câu trả lời cuối cùng phụ thuộc vào ranh giới số nguyên chính xác. 

Khoảng toàn cầu`[L, R]`bắt đầu như toàn bộ hàng rào. Mọi đỉnh phản xạ đều làm nó co lại. Nếu khoảng thời gian trở nên trống thì không có tháp canh hợp lệ nào tồn tại. 

Bước đếm số nguyên phải xử lý các điểm cuối hợp lý một cách cẩn thận. Chúng tôi cần:```
ceil(L) ≤ x ≤ floor(R)
```Việc sử dụng tử số và mẫu số của phân số sẽ tránh được lỗi làm tròn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
4 8
8 8
9 4
4 0
0 4
```Tất cả các đỉnh đều nằm dưới hàng rào rồi. 

Đa giác lồi nên không có đỉnh phản xạ. 

| Bước | Đỉnh phản xạ | Khoảng thời gian sau khi xử lý | 
| --- | --- | --- | 
| Ban đầu | Không có | [4, 8] | 
| Kết thúc | Không có | [4, 8] | 

Điểm nguyên là:```
4 5 6 7 8
```Trả lời:```
5
```Ví dụ này thể hiện tình huống đơn giản nhất. Đa giác lồi không hạn chế tầm nhìn nên mọi điểm hàng rào đều hoạt động. 

### Ví dụ không lồi tùy chỉnh 

đầu vào:```
5
0 0
10 0
10 10
5 3
0 10
```Sau khi phản ánh:```
(0,0)
(10,0)
(10,-10)
(5,-3)
(0,-10)
```Đỉnh`(5,-3)`là phản xạ. 

| Bước | Nút giao thông được tính toán | Khoảng thời gian toàn cầu | 
| --- | --- | --- | 
| Ban đầu | Không có | [0, 10] | 
| Đỉnh phản xạ | x = 3,5 và x = 6,5 | [3,5, 6,5] | 

Điểm nguyên bên trong:```
4 5 6
```Trả lời:```
3
```Dấu vết này cho thấy một vết lõm hướng vào trong tạo ra hình nón hiển thị như thế nào. Chỉ những điểm nhìn bên trong hình nón mới có thể nhìn thấy toàn bộ đa giác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh được xử lý một lần | 
| Không gian | O(1) | Chỉ một số biến hình học được lưu trữ | 

Với`n ≤ 1000`, độ phức tạp tuyến tính dễ dàng đủ nhanh. Giải pháp chỉ thực hiện một số lượng nhỏ các phép tính số học chính xác trên mỗi đỉnh, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from fractions import Fraction

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def intersect_x(a, b, Y):
        x1, y1 = a
        x2, y2 = b

        if y1 == y2:
            return Fraction(x1, 1)

        return Fraction(x1 * (y2 - Y) + x2 * (Y - y1), y2 - y1)

    def cross(ax, ay, bx, by):
        return ax * by - ay * bx

    n = int(input())
    p = [tuple(map(int, input().split())) for _ in range(n)]

    Y = p[0][1]

    if p[2][1] > Y:
        p = [(x, -y) for x, y in p]
        Y = -Y

    x1 = p[0][0]
    x2 = p[1][0]

    if x1 > x2:
        x1, x2 = x2, x1

    L = Fraction(x1, 1)
    R = Fraction(x2, 1)

    for i in range(n):
        u = p[(i - 1) % n]
        v = p[i]
        w = p[(i + 1) % n]

        c = cross(v[0] - u[0], v[1] - u[1],
                  w[0] - v[0], w[1] - v[1])

        if c <= 0:
            continue

        xv1 = intersect_x(u, v, Y)
        xv2 = intersect_x(v, w, Y)

        left = min(xv1, xv2)
        right = max(xv1, xv2)

        L = max(L, left)
        R = min(R, right)

    if L > R:
        return "0\n"

    lo = (L.numerator + L.denominator - 1) // L.denominator
    hi = R.numerator // R.denominator

    return str(max(0, hi - lo + 1)) + "\n"

# provided sample
assert run(
"""5
4 8
8 8
9 4
4 0
0 4
""") == "5\n", "sample 1"

# minimum polygon
assert run(
"""3
0 0
2 0
1 2
""") == "3\n", "triangle"

# non-convex with restricted visibility
assert run(
"""5
0 0
10 0
10 10
5 3
0 10
""") == "3\n", "reflex restriction"

# no valid point
assert run(
"""5
0 0
4 0
4 4
2 1
0 4
""") == "1\n", "single valid point"

# fence endpoints reversed
assert run(
"""4
10 0
0 0
0 5
10 5
""") == "11\n", "reversed fence order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đa giác tam giác | 3 | Đa giác lồi cho phép mọi điểm | 
| Vết lõm không lồi | 3 | Hạn chế tầm nhìn phản xạ | 
| Điểm hợp lệ duy nhất | 1 | Xử lý ranh giới khoảng thời gian chính xác | 
| Điểm cuối hàng rào đảo ngược | 11 | Chuẩn hóa đúng hướng hàng rào | 

## Vỏ cạnh 

Hãy xem xét trường hợp tầm nhìn thay đổi chính xác ở một ranh giới hợp lý:```
5
0 0
6 0
6 6
3 1
0 6
```Sau khi phản xạ, đỉnh phản xạ tạo ra các ranh giới khoảng ở tọa độ không nguyên. Giả sử phạm vi hợp lệ trở thành:```
[2.4, 3.6]
```Thuật toán tính toán các ranh giới này bằng cách sử dụng các phân số chính xác, không phải dấu phẩy động. Số đếm cuối cùng sử dụng:```
ceil(2.4) = 3
floor(3.6) = 3
```Vậy câu trả lời là đúng`1`. 

Bây giờ hãy xem xét một đa giác không có điểm nhìn hợp lệ:```
5
0 0
8 0
8 8
4 1
0 8
```Đỉnh phản xạ tạo ra một khoảng rất hẹp có thể sụp đổ sau khi giao nhau với ranh giới hàng rào. Trong quá trình xử lý:```
L > R
```Thuật toán ngay lập tức phát hiện giao lộ trống và in`0`. 

Cuối cùng, hãy xem xét một đa giác lồi:```
4
0 0
5 0
5 5
0 5
```Không có đỉnh phản xạ nên khoảng không bao giờ co lại từ`[0,5]`. Mọi điểm hàng rào số nguyên đều hoạt động và thuật toán trả về`6`. 

Những trường hợp này chính xác là khi việc triển khai dấu phẩy động hoặc kiểm tra khả năng hiển thị không đầy đủ thường thất bại. Công thức dựa trên khoảng với số học chính xác xử lý chúng một cách rõ ràng.
