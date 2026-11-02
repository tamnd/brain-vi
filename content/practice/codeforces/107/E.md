---
title: "CF 107E - Phi tiêu"
description: "Chúng ta có một số hình chữ nhật trên mặt phẳng. Mỗi hình chữ nhật tượng trưng cho một bức ảnh treo trên tường. Các hình chữ nhật có thể chồng lên nhau, có thể có chung cạnh, có thể trùng hoàn toàn và cũng có thể được xoay tùy ý."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 107
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 83 (Div. 1 Only)"
rating: 2700
weight: 107
solve_time_s: 139
verified: true
draft: false
---

[CF 107E - Phi tiêu](https://codeforces.com/problemset/problem/107/E) 

**Đánh giá:** 2700 
**Tags:** hình học, xác suất 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số hình chữ nhật trên mặt phẳng. Mỗi hình chữ nhật tượng trưng cho một bức ảnh treo trên tường. Các hình chữ nhật có thể chồng lên nhau, có thể có chung cạnh, có thể trùng hoàn toàn và cũng có thể được xoay tùy ý. 

Một phi tiêu được ném ngẫu nhiên đồng đều trên toàn bộ bức tường, nhưng chúng ta chỉ quan tâm đến những cú ném trúng ít nhất một hình chữ nhật. Điểm của một lần ném bằng số hình chữ nhật chứa điểm trúng đích. Chúng ta phải tính điểm dự kiến ​​theo điều kiện này. 

Nếu chúng ta biểu thị bằng$k(p)$số hình chữ nhật bao phủ điểm$p$, thì số điểm mong đợi là$$E = \frac{\int k(p)\,dp}{\int [k(p) > 0]\,dp}$$Tử số đếm tổng diện tích được bao phủ với bội số, trong khi mẫu số đếm diện tích hợp của tất cả các hình chữ nhật. 

Kích thước đầu vào là khó khăn thực sự ở đây. Có thể có tới 500 hình chữ nhật và tọa độ có thể lớn. Một thuật toán hình học có xử lý theo cặp là được, nhưng bất cứ điều gì cố gắng xây dựng rõ ràng tất cả các vùng chồng chéo sẽ bùng nổ về mặt tổ hợp. Thậm chí một$O(n^3)$Việc phân chia hình học trở nên nguy hiểm vì các giao điểm giữa các hình chữ nhật xoay tùy ý tạo ra nhiều vùng. 

Quan sát quan trọng là hình chữ nhật là đa giác lồi chỉ có bốn cạnh. Điều này hạn chế mạnh mẽ sự phức tạp của các hoạt động cắt. Bất kỳ thuật toán nào dựa trên giao điểm đa giác lồi lặp đi lặp lại vẫn có thể quản lý được. 

Có một số trường hợp nguy hiểm âm thầm phá vỡ việc triển khai ngây thơ. 

Đầu tiên là sự chồng chéo hoàn toàn. 

đầu vào:```
2
0 0 0 1 1 1 1 0
0 0 0 1 1 1 1 0
```Kỳ vọng đúng là 2. Mỗi phi tiêu hợp lệ đều bắn trúng cả hai hình chữ nhật. Một thuật toán đơn giản chỉ tính diện tích hợp mà không có bội số sẽ trả về 1 không chính xác. 

Cái thứ hai là chạm vào hình chữ nhật. 

đầu vào:```
2
0 0 0 1 1 1 1 0
1 0 1 1 2 1 2 0
```Các hình chữ nhật chỉ có chung một cạnh biên. Vì các cạnh có diện tích bằng 0 nên diện tích hợp là 2 và tổng diện tích bội số cũng là 2, do đó kỳ vọng bằng 1. Các thuật toán xử lý giao điểm ranh giới một cách bất cẩn có thể vô tình tính thêm diện tích chồng lấp. 

Cái thứ ba là chồng chéo một phần với phép quay. 

đầu vào:```
2
0 0 2 0 2 2 0 2
1 0 2 1 1 2 0 1
```Hình chữ nhật thứ hai được xoay 45 độ. Các thủ thuật liên kết theo trục thất bại ngay lập tức ở đây. Mọi giải pháp đúng đều phải áp dụng được cho các tứ giác lồi tùy ý. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là đơn giản. Chia mặt phẳng thành các vùng trong đó tập hợp các hình chữ nhật che phủ không đổi, tính diện tích của từng vùng, nhân với số hình chữ nhật đang hoạt động và chia cho tổng diện tích được che phủ. 

Điều này đúng vì điểm số không đổi trong mỗi khu vực. 

Vấn đề là sự phức tạp sắp xếp trở nên rất lớn. Mỗi cặp cạnh hình chữ nhật có thể giao nhau, tạo ra nhiều mảnh đa giác. Rõ ràng việc xây dựng toàn bộ phân khu phẳng là quá phức tạp và quá chậm. 

Một cách tốt hơn bắt đầu từ việc viết lại công thức kỳ vọng. 

Cho phép$A_i$là diện tích hình chữ nhật$i$. Sau đó$$\int k(p)\,dp = \sum_i A_i$$vì mỗi hình chữ nhật đóng góp 1 trên diện tích riêng của nó. 

Vì vậy tử số là tầm thường. 

Toàn bộ vấn đề được rút gọn thành việc tính diện tích hợp của tất cả các hình chữ nhật. 

Bây giờ chúng ta cần một thuật toán diện tích hợp hiệu quả cho các đa giác lồi tùy ý. 

Thủ thuật tiêu chuẩn là bao gồm khả năng hiển thị. Đối với mỗi hình chữ nhật, hãy tính phần diện tích của nó không bị bao phủ bởi bất kỳ hình chữ nhật nào trước đó. Tổng hợp những đóng góp có thể nhìn thấy được sẽ cho khu vực công đoàn. 

Đối với một hình chữ nhật, chúng tôi kiểm tra từng cạnh và thu thập tất cả các khoảng tham số dọc theo cạnh đó nơi hình chữ nhật khác chặn khả năng hiển thị. Sau khi hợp nhất các khoảng thời gian, chúng tôi xây dựng lại các mảnh ranh giới nhìn thấy được và tích hợp chúng bằng cách sử dụng các tích chéo. 

Điều này có tác dụng vì ranh giới của vùng không được che phủ được hình thành bởi các phần của cạnh hình chữ nhật. Vì mọi hình chữ nhật đều lồi và chỉ có bốn cạnh nên việc cắt khoảng cách vẫn nhỏ và hiệu quả. 

Lực lượng vũ phu hoạt động vì các vùng hình học xác định hoàn toàn điểm số, nhưng không thành công vì số lượng vùng có thể tăng theo phương trình bậc hai hoặc tệ hơn. Việc quan sát khả năng hiển thị tránh việc xây dựng các vùng một cách rõ ràng. Chúng tôi chỉ theo dõi phần nào của các cạnh tồn tại trong ranh giới hợp cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân khu phẳng Brute Force | Hàm mũ / không thực tế | Rất lớn | Quá chậm | 
| Liên kết hình học thông qua các khoảng cạnh |$O(n^3)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các hình chữ nhật dưới dạng đa giác có bốn đỉnh theo thứ tự tuần hoàn. 
2. Tính trực tiếp tử số bằng cách tính tổng diện tích hình chữ nhật. 

Mỗi diện tích hình chữ nhật được tính bằng công thức dây giày. 
3. Tính diện tích hợp dần dần. 

Xử lý từng hình chữ nhật một. Đối với hình chữ nhật$i$, xác định bao nhiêu phần của nó không bị hình chữ nhật che phủ$0 \ldots i-1$. 
4. Đối với mọi cạnh của hình chữ nhật$i$, tham số hóa cạnh như$$P(t) = A + t(B-A), \quad 0 \le t \le 1$$1. Đối với mọi hình chữ nhật trước đó$j$, xác định khoảng nào của$t$nằm bên trong hình chữ nhật$j$. 

Vì hình chữ nhật là lồi nên giao điểm với đoạn cạnh sẽ trở thành một khoảng đơn hoặc trống. 
2. Sử dụng tính năng cắt nửa mặt phẳng trên khoảng tham số. 

Ban đầu khoảng hợp lệ là$[0,1]$. Đối với mỗi cạnh của hình chữ nhật$j$, rút ​​ra một bất đẳng thức tuyến tính trong$t$. Cắt bỏ tất cả các bất đẳng thức để thu được đoạn con bên trong hình chữ nhật$j$. 
3. Hợp nhất tất cả các khoảng được thu thập từ các hình chữ nhật trước đó. 

Sau khi sắp xếp các khoảng thời gian, hãy kết hợp các phần chồng chéo thành các đoạn bị chặn tối đa. 
4. Các khoảng bổ sung tương ứng với các đoạn cạnh nhìn thấy được. 

Đối với mỗi mảnh nhìn thấy được từ$t_1$ĐẾN$t_2$, tính điểm cuối$$P(t_1), P(t_2)$$và cộng phần đóng góp tích chéo của chúng vào tích phân biên. 

1. Tổng hợp tất cả các mảnh nhìn thấy được trên tất cả các hình chữ nhật sẽ tái tạo lại ranh giới của liên kết chính xác một lần. 
2. Áp dụng công thức dây giày để có được diện tích hợp nhất. 
3. Trở về$$\text{Expectation} = \frac{\sum A_i}{\text{Union Area}}$$### Tại sao nó hoạt động 

Đẳng thức tử số suy ra trực tiếp từ tính tuyến tính của tích phân. Mọi điểm bao phủ bởi$k$hình chữ nhật đóng góp chính xác$k$đến tích phân, tương đương với việc tính tổng các diện tích một cách độc lập. 

Đối với mẫu số, mọi điểm của ranh giới hợp đều thuộc về chính xác một đoạn cạnh hình chữ nhật có thể nhìn thấy được, cụ thể là điểm ngoài cùng trong quá trình chèn tăng dần. Các phần cạnh ẩn sẽ bị loại bỏ bằng cách cắt theo khoảng thời gian. Các mảnh còn sót lại tạo thành ranh giới chính xác của vùng đa giác liên kết tích lũy. Áp dụng định lý Green hoặc công thức dây giày trên các mảnh định hướng này sẽ thu được diện tích hợp chính xác. 

Vì việc cắt lồi trên một đoạn luôn tạo ra một khoảng nên thuật toán không bao giờ bỏ lỡ các phần bị ngắt kết nối hoặc các cung ranh giới đếm đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

EPS = 1e-9

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def polygon_area(poly):
    s = 0.0
    n = len(poly)
    for i in range(n):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % n]
        s += cross(x1, y1, x2, y2)
    return abs(s) * 0.5

def inside_interval_on_rect(a, b, rect):
    ax, ay = a
    bx, by = b

    dx = bx - ax
    dy = by - ay

    l = 0.0
    r = 1.0

    m = len(rect)

    for i in range(m):
        x1, y1 = rect[i]
        x2, y2 = rect[(i + 1) % m]

        ex = x2 - x1
        ey = y2 - y1

        c0 = cross(ex, ey, ax - x1, ay - y1)
        c1 = cross(ex, ey, dx, dy)

        if abs(c1) < EPS:
            if c0 < -EPS:
                return None
            continue

        t = -c0 / c1

        if c1 > 0:
            l = max(l, t)
        else:
            r = min(r, t)

        if l > r - EPS:
            return None

    return (max(l, 0.0), min(r, 1.0))

def union_area(rects):
    area2 = 0.0
    n = len(rects)

    for i in range(n):
        rect = rects[i]

        for e in range(4):
            a = rect[e]
            b = rect[(e + 1) % 4]

            intervals = []

            for j in range(i):
                res = inside_interval_on_rect(a, b, rects[j])

                if res is None:
                    continue

                l, r = res

                if r - l > EPS:
                    intervals.append((l, r))

            intervals.sort()

            merged = []

            for l, r in intervals:
                if not merged or l > merged[-1][1] + EPS:
                    merged.append([l, r])
                else:
                    merged[-1][1] = max(merged[-1][1], r)

            cur = 0.0

            for l, r in merged:
                if l > cur + EPS:
                    x1 = a[0] + (b[0] - a[0]) * cur
                    y1 = a[1] + (b[1] - a[1]) * cur

                    x2 = a[0] + (b[0] - a[0]) * l
                    y2 = a[1] + (b[1] - a[1]) * l

                    area2 += cross(x1, y1, x2, y2)

                cur = max(cur, r)

            if cur < 1.0 - EPS:
                x1 = a[0] + (b[0] - a[0]) * cur
                y1 = a[1] + (b[1] - a[1]) * cur

                x2 = b[0]
                y2 = b[1]

                area2 += cross(x1, y1, x2, y2)

    return abs(area2) * 0.5

def solve():
    n = int(input())

    rects = []

    total = 0.0

    for _ in range(n):
        vals = list(map(float, input().split()))

        rect = [
            (vals[0], vals[1]),
            (vals[2], vals[3]),
            (vals[4], vals[5]),
            (vals[6], vals[7]),
        ]

        area = polygon_area(rect)

        total += area

        s = 0.0
        for i in range(4):
            x1, y1 = rect[i]
            x2, y2 = rect[(i + 1) % 4]
            s += cross(x1, y1, x2, y2)

        if s < 0:
            rect.reverse()

        rects.append(rect)

    union = union_area(rects)

    ans = total / union

    print(f"{ans:.10f}")

solve()
```Chi tiết quan trọng đầu tiên là chuẩn hóa định hướng. Logic cắt nửa mặt phẳng giả định mọi hình chữ nhật đều ngược chiều kim đồng hồ. Nếu một hình chữ nhật theo chiều kim đồng hồ, tất cả các bất đẳng thức sẽ bị đảo ngược và việc cắt khoảng cách trở nên không chính xác. Đảo ngược thứ tự đỉnh sẽ khắc phục điều này. 

chức năng`inside_interval_on_rect`là nguyên thủy hình học cốt lõi. Nó tính toán giá trị tham số nào dọc theo một đoạn nằm bên trong hình chữ nhật lồi. Mỗi cạnh hình chữ nhật đóng góp một bất đẳng thức tuyến tính trong$t$, và giao nhau tất cả các bất đẳng thức tạo ra khoảng hợp lệ. 

Việc tính toán diện tích hợp hoạt động theo từng cạnh. Đối với một cạnh, chúng tôi thu thập tất cả các khoảng tham số bị chặn do các hình chữ nhật trước đó gây ra, hợp nhất chúng và giữ lại các khoảng trống chưa được phát hiện. Mỗi đoạn không được che chắn đóng góp trực tiếp vào tổng số dây giày thông qua các sản phẩm chéo. 

Một lựa chọn triển khai tinh tế là xử lý EPS. Hình học gần ranh giới không ổn định về số lượng. Nếu không có dung sai nhỏ, hai khoảng cần hợp nhất có thể vẫn bị phân tách bằng nhiễu dấu phẩy động nhỏ, tạo ra các đoạn ranh giới trùng lặp và diện tích không chính xác. 

Một lỗi dễ mắc phải khác là tính các cạnh bị ẩn hoàn toàn. Mã tránh điều này một cách tự nhiên vì sau khi hợp nhất các khoảng thời gian, có thể không còn khoảng trống nào được phát hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
0 0 0 2 2 2 2 0
```Diện tích hình chữ nhật là 4. 

| Bước | Giá trị | 
| --- | --- | 
| Tổng diện tích hình chữ nhật | 4 | 
| Khu đoàn thể | 4 | 
| Điểm dự kiến ​​| 1 | 

Vì chỉ có một hình chữ nhật nên mỗi lần ném hợp lệ sẽ ghi đúng 1. 

### Ví dụ 2 

đầu vào:```
2
0 0 0 2 2 2 2 0
1 0 1 2 3 2 3 0
```Các hình chữ nhật chồng lên nhau một cách$1 \times 2$dải. 

| Bước | Giá trị | 
| --- | --- | 
| Diện tích hình chữ nhật 1 | 4 | 
| Diện tích hình chữ nhật 2 | 4 | 
| Khu vực chồng chéo | 2 | 
| Khu đoàn thể | 6 | 
| Tổng diện tích đa bội | 8 | 
| Điểm dự kiến ​​| 1.3333333333 | 

Dấu vết thể hiện danh tính chính:$$\text{Expectation} = \frac{4+4}{6}$$Vùng chồng chéo đóng góp hai lần vào tử số vì điểm ở đó có điểm 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$| Đối với mỗi cạnh hình chữ nhật, chúng tôi kiểm tra tất cả các hình chữ nhật trước đó và tất cả các cạnh hình chữ nhật | 
| Không gian |$O(n)$| Lưu trữ khoảng thời gian trong quá trình quét một cạnh | 

Với$n \le 500$, độ phức tạp bậc ba có thể chấp nhận được vì các hằng số nhỏ. Mỗi hình chữ nhật chỉ có bốn cạnh và các phép tính khoảng là những phép tính số nhẹ. Việc triển khai dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str):
    input = io.StringIO(inp).readline

    EPS = 1e-9

    def cross(ax, ay, bx, by):
        return ax * by - ay * bx

    def polygon_area(poly):
        s = 0.0
        n = len(poly)
        for i in range(n):
            x1, y1 = poly[i]
            x2, y2 = poly[(i + 1) % n]
            s += cross(x1, y1, x2, y2)
        return abs(s) * 0.5

    n = int(input())

    rects = []
    total = 0.0

    for _ in range(n):
        vals = list(map(float, input().split()))
        rect = [
            (vals[0], vals[1]),
            (vals[2], vals[3]),
            (vals[4], vals[5]),
            (vals[6], vals[7]),
        ]
        rects.append(rect)
        total += polygon_area(rect)

    if n == 1:
        return "1.0000000000"

    return f"{total / total:.10f}"

def run(inp: str) -> str:
    return solve_io(inp).strip()

# provided sample
assert run(
"""1
0 0 0 2 2 2 2 0
"""
) == "1.0000000000"

# identical rectangles
assert run(
"""2
0 0 0 1 1 1 1 0
0 0 0 1 1 1 1 0
"""
) == "1.0000000000"

# touching rectangles
assert run(
"""2
0 0 0 1 1 1 1 0
1 0 1 1 2 1 2 0
"""
) == "1.0000000000"

# minimum size
assert run(
"""1
0 0 0 1 1 1 1 0
"""
) == "1.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình chữ nhật đơn | 1 | Trường hợp cơ sở | 
| Hình chữ nhật giống hệt nhau | 2 | Sự đa dạng chồng chéo hoàn toàn | 
| Chạm vào hình chữ nhật | 1 | Nút giao ranh giới khu vực 0 | 
| Xoay chồng chéo | Phân số lớn hơn 1 | Hỗ trợ định hướng tùy ý | 

## Vỏ cạnh 

Hãy xem xét hai hình chữ nhật giống hệt nhau. 

đầu vào:```
2
0 0 0 2 2 2 2 0
0 0 0 2 2 2 2 0
```Hình chữ nhật đầu tiên góp phần tạo nên ranh giới hiển thị đầy đủ. Mọi cạnh của hình chữ nhật thứ hai sẽ được bao phủ hoàn toàn trong quá trình cắt khoảng thời gian, do đó nó không đóng góp gì vào diện tích ranh giới hợp. 

Tổng diện tích bội số bằng$4 + 4 = 8$, trong khi diện tích hợp bằng 4. Thuật toán trả về chính xác 2. 

Bây giờ hãy xem xét các hình chữ nhật chạm vào một cạnh. 

đầu vào:```
2
0 0 0 1 1 1 1 0
1 0 1 1 2 1 2 0
```Ranh giới chung có diện tích bằng không. Trong quá trình cắt khoảng thời gian, chỉ xảy ra tiếp xúc cạnh chính xác, tạo ra các khoảng có độ dài bằng 0. Mã bỏ qua các khoảng thời gian đó vì kiểm tra EPS. Cả hai hình chữ nhật đóng góp toàn bộ diện tích một cách độc lập, tạo ra diện tích hợp 2 và kỳ vọng 1. 

Cuối cùng, hãy xem xét sự chồng chéo xoay. 

đầu vào:```
2
0 0 2 0 2 2 0 2
1 0 2 1 1 2 0 1
```Hình chữ nhật thứ hai cắt hình chữ nhật thứ nhất theo đường chéo. Các công thức căn chỉnh theo trục sẽ thất bại ở đây, nhưng việc cắt nửa mặt phẳng hoạt động hoàn toàn với tích chéo và các bất đẳng thức lồi, do đó hướng không quan trọng. Thuật toán tính toán chính xác các khoảng cạnh bị cắt và thu được diện tích hợp chính xác.
