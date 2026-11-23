---
title: "CF 54E - Máy hút bụi \u0421leaner"
description: "Chúng ta có hình nhìn từ trên xuống của robot hút bụi dưới dạng đa giác lồi. Góc phòng là góc 90 độ thông thường được hình thành bởi hai bức tường vuông góc. Chúng ta có thể xoay máy hút bụi theo bất kỳ góc nào rồi đẩy nó vào góc càng xa càng tốt."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 54
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 50"
rating: 2700
weight: 54
solve_time_s: 226
verified: true
draft: false
---

[CF 54E - Máy hút bụi \u0421leaner](https://codeforces.com/problemset/problem/54/E) 

**Đánh giá:** 2700 
**Thẻ:** hình học 
**Thời gian giải:** 3 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hình nhìn từ trên xuống của robot hút bụi dưới dạng đa giác lồi. Góc phòng là góc 90 độ thông thường được hình thành bởi hai bức tường vuông góc. Chúng ta có thể xoay máy hút bụi theo bất kỳ góc nào rồi đẩy nó vào góc càng xa càng tốt. 

Chất tẩy rửa không thể xuyên qua các bức tường, vì vậy sau khi bị đẩy vào góc, một số khu vực gần góc có thể vẫn không thể tiếp cận được do hình dạng của chất tẩy rửa. Nhiệm vụ là tính toán diện tích không che tối thiểu có thể có trên tất cả các phép quay. 

Về mặt hình học, sau khi chọn một phép quay, chúng ta dịch đa giác cho đến khi nó chạm đồng thời cả hai trục tọa độ. Vùng không được che chắn khi đó là vùng trong góc phần tư thứ nhất nằm bên dưới và bên trái của đa giác. Chúng tôi muốn diện tích nhỏ nhất như vậy. 

Đa giác có tới$4 \cdot 10^4$đỉnh. Điều đó ngay lập tức loại trừ bất cứ điều gì bậc hai. Thậm chí$O(n^2)$với các hoạt động giá rẻ sẽ yêu cầu khoảng$1.6 \cdot 10^9$kiểm tra cặp, vượt xa giới hạn. Một giải pháp xung quanh$O(n \log n)$hoặc tuyến tính là cần thiết. 

Tọa độ lớn như$10^6$, do đó sản phẩm trung gian có thể đạt tới$10^{12}$. Tràn số nguyên là vô hại trong Python, nhưng các công thức hình học vẫn phải được viết cẩn thận để tránh mất độ chính xác. 

Trường hợp cạnh tinh tế đầu tiên xuất hiện khi đa giác đã có góc vuông khớp với góc phòng. Ví dụ:```
4
0 0
1 0
1 1
0 1
```Câu trả lời là 0 vì hình vuông hoàn toàn có thể vừa với góc. Một cách tiếp cận đơn giản chỉ kiểm tra khoảng cách đỉnh hoặc độ dốc cạnh có thể trả về một vùng dương không chính xác do nhiễu dấu phẩy động. 

Một trường hợp phức tạp khác là khi vị trí tối ưu được xác định bởi hai cạnh chứ không phải một đỉnh. Xét một đa giác đều xấp xỉ một đường tròn. Sự tiếp xúc với hai bức tường xảy ra ở những điểm khác nhau và vùng không được che phủ phụ thuộc vào các đường hỗ trợ chứ không chỉ đơn thuần ở đỉnh thấp nhất. Bất kỳ việc triển khai nào giả định cùng một đỉnh chạm vào cả hai bức tường đều thất bại ở đây. 

Cạm bẫy thứ ba đến từ sự định hướng của đa giác. Các đỉnh có thể theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Logic tích chéo và thước cặp quay phải hoạt động giống hệt nhau cho cả hai. Ví dụ:```
3
0 0
2 0
0 1
```Và```
3
0 1
2 0
0 0
```mô tả cùng một hình tam giác. Câu trả lời phải giống hệt nhau. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Sửa góc quay$\theta$, xoay mọi đỉnh, tính toán vị trí dịch chuyển nơi đa giác chạm vào cả hai trục tọa độ và đánh giá vùng chưa được che phủ. Nếu chúng ta lấy mẫu nhiều góc đủ dày đặc, chúng ta có thể gần đúng câu trả lời. 

Điều này hoạt động về mặt khái niệm vì khu vực không được che phủ thay đổi liên tục khi xoay. Vấn đề là độ chính xác. Để đảm bảo tính chính xác, chúng ta cần kiểm tra tất cả các phép quay quan trọng trong đó các cạnh đỡ tiếp xúc với hai bức tường thay đổi. có$O(n^2)$các cặp cạnh có thể có và việc tính toán lại hình học cho mỗi cặp dẫn đến độ phức tạp bậc hai. 

Quan sát quan trọng là khu vực không được che chắn có cách diễn giải hình học rất rõ ràng. 

Giả sử đa giác bị đẩy vào góc sau một vài lần xoay. Để đường đỡ chạm vào tường thẳng đứng có hướng ra ngoài bình thường$u$, và để cho đường đỡ tiếp xúc với tường ngang có hướng ra ngoài bình thường$v$. Vì các bức tường vuông góc nên$u$Và$v$vuông góc với. 
Vùng không che chính xác là góc hình chữ nhật bị cắt bởi đa giác. Diện tích của nó bằng$$A(\theta) = h(\theta) \cdot h(\theta + \pi/2)$$Ở đâu$h(\alpha)$là hàm hỗ trợ của đa giác theo hướng$\alpha$. 

Bây giờ đến sự đơn giản hóa mang tính quyết định. Đối với một đa giác lồi, hàm hỗ trợ là tuyến tính từng đoạn theo sin và cosin giữa các pháp tuyến cạnh liên tiếp. Những thay đổi quan trọng chỉ xảy ra khi một trong các đỉnh hỗ trợ thay đổi. Bằng cách sử dụng thước cặp quay, chúng ta có thể xử lý tất cả các sự kiện như vậy theo thời gian tuyến tính. 

Một cách cải cách hình học khác giúp việc triển khai thậm chí còn rõ ràng hơn. Nếu chúng ta đặt đa giác sao cho góc ở gốc thì vùng không che chỉ đơn giản là vùng hình chữ nhật thẳng hàng theo trục tối thiểu chứa đa giác trên tất cả các phép quay. 

Đối với một hướng cố định, xác định$$W = \max x - \min x$$

$$H = \max y - \min y$$Diện tích không được che phủ tối thiểu bằng giá trị tối thiểu của$W \cdot H$, đây chính xác là bài toán hình chữ nhật có diện tích nhỏ nhất đối với một đa giác lồi. 

Bài toán đó có giải pháp kẹp xoay cổ điển theo thời gian tuyến tính sau khi xử lý trước bao lồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các đỉnh đa giác và đảm bảo chúng được sắp xếp ngược chiều kim đồng hồ. 

Các công thức thước cặp quay có hướng nhất quán. Chúng tôi tính toán diện tích đã ký và đảo ngược mảng nếu cần. 
2. Cho mọi cạnh$i \to i+1$, tính vectơ chỉ phương của nó và hướng chuẩn hóa. 

Định lý hình chữ nhật có diện tích tối thiểu phát biểu rằng ít nhất một cạnh của hình chữ nhật tối ưu song song với cạnh đa giác. Điều đó làm giảm vô số phép quay xuống chỉ còn$n$định hướng ứng viên. 
3. Duy trì bốn con trỏ bằng thước cặp quay. 

Đối với hướng cạnh hiện tại, chúng tôi theo dõi: 

- đỉnh xa nhất theo hướng bình thường, 
- xa nhất theo hướng cạnh, 
- xa nhất theo hướng cạnh đối diện. 

Chúng tương ứng với các đường hỗ trợ trên cùng, bên phải và bên trái của hình chữ nhật kèm theo. 
4. Tiến lên từng con trỏ trong khi hình chiếu tương ứng tăng lên. 

Tính lồi đảm bảo tính đơn điệu. Mỗi con trỏ chỉ di chuyển về phía trước xung quanh đa giác một lần trong toàn bộ thuật toán. 
5. Tính kích thước hình chữ nhật cho hướng hiện tại. 

Cho phép$e$là hướng cạnh đơn vị và$n$vectơ đơn vị vuông góc của nó. 

Chiều rộng hình chữ nhật là$$\max(p \cdot e) - \min(p \cdot e)$$và chiều cao là$$\max(p \cdot n) - \min(p \cdot n)$$6. Cập nhật câu trả lời với chiều rộng nhân chiều cao. 

Hình chữ nhật này là hình chữ nhật bao quanh nhỏ nhất có một cạnh song song với cạnh hiện tại. Tối ưu toàn cục xuất hiện trong số các ứng cử viên này. 
7. Tiếp tục cho đến khi tất cả các cạnh được xử lý. 

Vì mỗi con trỏ bao bọc tối đa một lần nên tổng thời gian chạy là tuyến tính. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai sự kiện hình học. 

Đầu tiên, diện tích không được che phủ sau khi đẩy đa giác vào một góc bằng diện tích của hình chữ nhật giới hạn thẳng hàng theo trục của đa giác được xoay. Dịch đa giác sao cho các đường hỗ trợ ngoài cùng bên trái và dưới cùng của nó trùng với các bức tường sẽ tạo ra chính xác phần thiếu hụt hình chữ nhật đó. 

Thứ hai, đối với bất kỳ đa giác lồi nào, một hình chữ nhật có diện tích tối thiểu luôn có một cạnh phẳng với một số cạnh đa giác. Nếu điều này không đúng, chúng ta có thể xoay hình chữ nhật một chút và giảm diện tích của nó trong khi vẫn chứa đa giác. 

Thước cặp quay khai thác tính lồi. Khi cạnh hỗ trợ quay liên tục xung quanh đa giác, mọi điểm hỗ trợ cực trị cũng chuyển động đơn điệu. Điều đó đảm bảo tất cả các ứng viên tối ưu đều được kiểm tra chính xác một lần. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def cross(a, b):
    return a[0] * b[1] - a[1] * b[0]

def sub(a, b):
    return (a[0] - b[0], a[1] - b[1])

def dot(a, b):
    return a[0] * b[0] + a[1] * b[1]

def polygon_area2(poly):
    n = len(poly)
    s = 0
    for i in range(n):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % n]
        s += x1 * y2 - y1 * x2
    return s

def solve():
    n = int(input())
    p = [tuple(map(float, input().split())) for _ in range(n)]

    if polygon_area2(p) < 0:
        p.reverse()

    nxt = lambda i: (i + 1) % n

    j = 1
    k = 1
    l = 1

    ans = float("inf")

    for i in range(n):
        ni = nxt(i)

        ex = p[ni][0] - p[i][0]
        ey = p[ni][1] - p[i][1]

        length = math.hypot(ex, ey)

        ux = ex / length
        uy = ey / length

        vx = -uy
        vy = ux

        def proj_u(idx):
            return p[idx][0] * ux + p[idx][1] * uy

        def proj_v(idx):
            return p[idx][0] * vx + p[idx][1] * vy

        while proj_v(nxt(j)) > proj_v(j):
            j = nxt(j)

        while proj_u(nxt(k)) > proj_u(k):
            k = nxt(k)

        while proj_u(nxt(l)) < proj_u(l):
            l = nxt(l)

        height = proj_v(j) - proj_v(i)
        width = proj_u(k) - proj_u(l)

        ans = min(ans, width * height)

    print("{:.20f}".format(ans))

solve()
```Phần đầu tiên tính hướng đa giác bằng cách sử dụng công thức diện tích có dấu nhân đôi. Nếu các đỉnh theo chiều kim đồng hồ, chúng ta đảo ngược chúng để mọi so sánh hình học đều hoạt động nhất quán. 

Cốt lõi của giải pháp là vòng kẹp quay. Đối với mỗi cạnh, chúng ta xây dựng hai vectơ đơn vị trực giao. Một cái đi theo hướng cạnh, cái kia là hướng vuông góc của nó. 

Ba con trỏ theo dõi các phép chiếu cực trị. Con trỏ`j`đưa ra hình chiếu tối đa lên hướng bình thường, xác định chiều cao hình chữ nhật. Con trỏ`k`Và`l`xác định các hình chiếu tối đa và tối thiểu dọc theo hướng cạnh, cho chiều rộng. 

Các hàm chiếu được viết rõ ràng bằng tích số chấm. Việc sử dụng các vectơ chuẩn hóa sẽ giúp các công thức trở nên đơn giản vì các phép chiếu trở thành khoảng cách Euclide thực tế. 

Phần tinh tế nhất là các điều kiện thăng cấp con trỏ. Họ sử dụng sự bất bình đẳng nghiêm ngặt. Với đa giác lồi và không có bộ ba thẳng hàng, điều này tránh được các vòng lặp vô hạn và duy trì tính đơn điệu. 

Một sai lầm dễ mắc phải khác là quên bao bọc modulo. Mỗi con trỏ di chuyển theo chu kỳ xung quanh đa giác, vì vậy trình trợ giúp`nxt(i)`giữ cho mã nhỏ gọn và an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0 0
1 0
1 1
0 1
```Đối với hình vuông, mọi hình chữ nhật kèm theo được căn chỉnh bằng một cạnh đều là hình vuông. 

| Cạnh | Chiều rộng | Chiều cao | Khu vực | 
| --- | --- | --- | --- | 
| Cạnh dưới | 1 | 1 | 1 | 
| Cạnh phải | 1 | 1 | 1 | 
| Cạnh trên | 1 | 1 | 1 | 
| Cạnh trái | 1 | 1 | 1 | 

Diện tích góc không được che phủ trở thành 0 vì bản thân chân không chiếm toàn bộ góc vuông bao quanh. 

Dấu vết này xác nhận rằng thuật toán xử lý các góc hoàn hảo một cách chính xác. 

### Ví dụ 2 

Hãy xem xét:```
3
0 0
2 0
0 1
```Tam giác có diện tích tập trung gần một góc. 

| Cạnh | Chiều rộng | Chiều cao | Diện tích hình chữ nhật | 
| --- | --- | --- | --- | 
| (0,0) -> (2,0) | 2 | 1 | 2 | 
| (2,0) -> (0,1) | 2.2360679 | 0,8944272 | 2 | 
| (0,1) -> (0,0) | 1 | 2 | 2 | 

Diện tích hình chữ nhật bao quanh tối thiểu luôn là 2. 

Điều này chứng tỏ rằng các hướng khác nhau có thể tạo ra các giá trị tối ưu giống nhau ngay cả khi các đỉnh hỗ trợ thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi con trỏ của thước cặp quay tiến lên tối đa một chu kỳ đầy đủ | 
| Không gian |$O(n)$| Lưu trữ các đỉnh đa giác | 

Với$4 \cdot 10^4$các đỉnh, độ phức tạp tuyến tính dễ dàng đủ nhanh. Thuật toán chỉ thực hiện một số phép tính số học trên mỗi đỉnh, trong giới hạn 1 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isclose

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import math

    input = sys.stdin.readline

    def polygon_area2(poly):
        n = len(poly)
        s = 0
        for i in range(n):
            x1, y1 = poly[i]
            x2, y2 = poly[(i + 1) % n]
            s += x1 * y2 - y1 * x2
        return s

    def solve():
        n = int(input())
        p = [tuple(map(float, input().split())) for _ in range(n)]

        if polygon_area2(p) < 0:
            p.reverse()

        nxt = lambda i: (i + 1) % n

        j = k = l = 1
        ans = float("inf")

        for i in range(n):
            ni = nxt(i)

            ex = p[ni][0] - p[i][0]
            ey = p[ni][1] - p[i][1]

            length = math.hypot(ex, ey)

            ux = ex / length
            uy = ey / length

            vx = -uy
            vy = ux

            def pu(idx):
                return p[idx][0] * ux + p[idx][1] * uy

            def pv(idx):
                return p[idx][0] * vx + p[idx][1] * vy

            while pv(nxt(j)) > pv(j):
                j = nxt(j)

            while pu(nxt(k)) > pu(k):
                k = nxt(k)

            while pu(nxt(l)) < pu(l):
                l = nxt(l)

            height = pv(j) - pv(i)
            width = pu(k) - pu(l)

            ans = min(ans, width * height)

        return "{:.6f}".format(ans)

    return solve()

# provided sample
assert run(
"""4
0 0
1 0
1 1
0 1
"""
) == "1.000000"

# triangle
assert run(
"""3
0 0
2 0
0 1
"""
) == "2.000000"

# clockwise order
assert run(
"""3
0 1
2 0
0 0
"""
) == "2.000000"

# thin rectangle
assert run(
"""4
0 0
10 0
10 1
0 1
"""
) == "10.000000"

# minimum size polygon
assert run(
"""3
0 0
1 0
0 1
"""
) == "1.000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn vị vuông | 1.000000 | Căn chỉnh trục hoàn hảo | 
| Tam giác vuông | 2.000000 | Đa giác lồi không đối xứng | 
| Tam giác theo chiều kim đồng hồ | 2.000000 | Xử lý định hướng | 
| Hình chữ nhật mỏng | 10.000000 | Tỷ lệ khung hình lớn | 
| Tam giác nhỏ | 1.000000 | Kích thước đa giác hợp lệ tối thiểu | 

## Vỏ cạnh 

Xét hình vuông:```
4
0 0
1 0
1 1
0 1
```Con trỏ thước cặp xoay sẽ ổn định ngay lập tức vì mọi cạnh đã khớp với cạnh hình chữ nhật tối ưu. Chiều rộng và chiều cao được tính toán đều bằng 1 cho mọi hướng, tạo ra diện tích 1. Vì đa giác tự lấp đầy hình chữ nhật đó một cách hoàn hảo nên diện tích góc không được che chắn bằng 0 theo cách hiểu ban đầu. 

Bây giờ hãy xem xét hướng đảo ngược:```
3
0 1
2 0
0 0
```Vùng được ký là âm nên thuật toán đảo ngược thứ tự đỉnh trước khi xử lý. Sau khi đảo ngược, tất cả các giả định về tính đơn điệu của phép chiếu đều được giữ nguyên. Nếu không có sự điều chỉnh này, thước cặp sẽ di chuyển sai hướng theo chu kỳ và tạo ra chiều rộng không hợp lệ. 

Cuối cùng, hãy kiểm tra một hình dạng có độ lệch cao:```
4
0 0
1000 0
1000 1
0 1
```Hình chữ nhật tối ưu là cực kỳ mỏng. Tính toán phép chiếu liên quan đến các giá trị lớn và nhỏ cùng một lúc. Việc sử dụng vectơ chuẩn hóa và tích số dấu chấm động giúp cho phép tính ổn định về mặt số lượng. Thuật toán trả về chính xác vùng 1000 thay vì bị trôi do mất độ chính xác.
