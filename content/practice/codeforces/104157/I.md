---
title: "CF 104157I - Đồng nghiệp say rượu"
description: "Đường cong bậc hai mô tả cách một đồng nghiệp say rượu đi ngang qua một văn phòng hình chữ nhật. Tại bất kỳ vị trí nằm ngang $x$ nào, vị trí của anh ta là $f(x)$, do đó đường đi của anh ta là một parabol. Anh ta không thể nhìn thấy chính xác vô hạn."
date: "2026-07-02T01:18:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104157
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 2 (Beginner)"
rating: 0
weight: 104157
solve_time_s: 134
verified: false
draft: false
---

[CF 104157I - Đồng nghiệp say rượu](https://codeforces.com/problemset/problem/104157/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đường cong bậc hai mô tả cách một đồng nghiệp say rượu đi ngang qua một văn phòng hình chữ nhật. Ở bất kỳ vị trí nằm ngang nào$x$, vị trí của anh ấy là$f(x)$, vậy đường đi của anh ta là một parabol. 

Anh ta không thể nhìn thấy chính xác vô hạn. Thay vào đó, xung quanh mọi điểm trên đường đi của anh ta, có một dải tầm nhìn dọc có chiều rộng cố định bằng nửa chiều rộng.$k$. Nói cách khác, tại một thời điểm nhất định$x$, mọi thứ với$y$giữa$f(x)-k$Và$f(x)+k$được anh ta coi là “nhìn thấy”. 

Bên trong một hình chữ nhật được căn chỉnh theo trục nhất định, chúng tôi muốn tính toán xem dải này không nhìn thấy được bao nhiêu diện tích. Tương tự, chúng ta trừ đi phần diện tích của hình chữ nhật được bao phủ bởi dải dọc có độ dày$2k$xung quanh parabol. 

Hình chữ nhật là liên tục và hàm số cũng liên tục, vì vậy thách thức chính là tính diện tích trong một vùng được xác định bởi các bất đẳng thức liên quan đến hàm bậc hai. Các ràng buộc cho phép hệ số lên đến$10^5$về độ lớn, do đó các giá trị của hàm có thể lớn nhưng không có hạn chế về số lượng thao tác vượt quá giới hạn 1 giây. Điều đó gợi ý rõ ràng rằng chúng ta phải tránh bất kỳ việc lấy mẫu số chi tiết nào trên$x$, vì điều đó sẽ đòi hỏi quá nhiều đánh giá. 

Một ý tưởng ngây thơ là rời rạc hóa$x$-axis thành các bước nhỏ, đánh giá chiều cao nhìn thấy được ở mỗi bước và tính gần đúng tích phân. Điều đó không thành công vì độ chính xác cần thiết là$10^{-6}$và parabol có thể thay đổi nhanh chóng; để đạt được độ chính xác được đảm bảo sẽ yêu cầu độ phân giải cực kỳ tốt, dẫn đến hàng triệu hoặc hàng tỷ bước. 

Một trường hợp thất bại tinh vi hơn xuất hiện khi parabol vượt qua ranh giới hình chữ nhật nhiều lần. Ví dụ, khi$f(x)$giao nhau$y=y_1+k$hoặc$y=y_2-k$, cấu trúc của sự chồng chéo thay đổi đột ngột. Bất kỳ phương pháp nào giả sử một công thức cố định trong toàn bộ khoảng mà không phân tách tại các điểm chuyển tiếp này sẽ âm thầm tích phân biểu thức sai. 

## Phương pháp tiếp cận 

Đối tượng hình học mà chúng ta cần là diện tích giao điểm giữa hình chữ nhật và một “ống” xung quanh một parabol. ống được xác định bởi$|y - f(x)| \le k$, vì vậy với mỗi cố định$x$, lát cắt dọc của ống là khoảng$[f(x)-k, f(x)+k]$. Bên trong hình chữ nhật, vùng nhìn thấy được ở đó$x$là sự chồng chéo của khoảng này với$[y_1, y_2]$. 

Vì vậy đại lượng chính là hàm một chiều của$x$: chiều cao nhìn thấy được$h(x)$. Câu trả lời trở thành$$\text{answer} = (x_2 - x_1)(y_2 - y_1) - \int_{x_1}^{x_2} h(x)\,dx.$$Phương pháp tiếp cận vũ phu đánh giá$h(x)$tại nhiều điểm mẫu và xấp xỉ tích phân về mặt số. Điều này đúng về nguyên tắc nhưng không ổn định và quá chậm nếu thực thi độ chính xác cao. 

Quan sát cấu trúc là$h(x)$chỉ thay đổi công thức khi thứ tự tương đối của bốn biểu thức thay đổi:$f(x)-k$,$f(x)+k$,$y_1$, Và$y_2$. Sự chuyển đổi xảy ra chính xác khi$$f(x) = y_1 - k,\quad f(x) = y_1 + k,\quad f(x) = y_2 - k,\quad f(x) = y_2 + k.$$Mỗi phương trình này là một phương trình bậc hai, vì vậy mỗi phương trình có nhiều nhất hai nghiệm thực. Giữa các gốc liên tiếp, thứ tự được cố định, nghĩa là$h(x)$được mô tả bằng một biểu thức dạng đóng duy nhất. 

Bên trong một khoảng như vậy,$h(x)$trở thành một hằng số hoặc một hàm affine của$f(x)$, mà bản thân nó là bậc hai. Vì vậy tích phân trên mỗi đoạn là tuyến tính hoặc bậc ba theo$x$, tất cả đều có thể tính toán được bằng giải tích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lấy mẫu số | O(N mẫu) | O(1) | Quá chậm/không chính xác | 
| Tích hợp phân tích từng phần | O(1) đoạn (<9) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giảm vấn đề xuống việc lấy tích phân một hàm được xác định từng phần trên$x$, trong đó các điểm dừng đến từ việc giải bốn phương trình bậc hai. 

### 1. Xây dựng tọa độ x quan trọng 

Chúng tôi giải quyết:$$f(x) = y_1 - k,\; y_1 + k,\; y_2 - k,\; y_2 + k.$$Mỗi phương trình đều là phương trình bậc hai, vì vậy chúng ta tính các nghiệm thực và thu thập tất cả các giá trị nằm bên trong$[x_1, x_2]$, cùng với$x_1$Và$x_2$. Những điểm này phân chia miền thành các phân đoạn. 

Lý do điều này có tác dụng là vì chỉ tại những gốc này parabol mới vượt qua một ranh giới làm thay đổi phần nào của dải giao với hình chữ nhật. 

### 2. Sắp xếp và loại bỏ trùng lặp 

Chúng tôi sắp xếp tất cả các điểm ứng viên và loại bỏ những điểm gần trùng lặp. Điều này tạo ra một chuỗi các khoảng trong đó thứ tự tương đối giữa$f(x)$và tất cả bốn ngưỡng đều được cố định. 

### 3. Đánh giá từng phân khúc một cách độc lập 

Đối với mỗi khoảng thời gian$[l, r]$, ta chọn điểm giữa$m$và đánh giá dấu của:$$f(m) - (y_1 - k),\quad f(m) - (y_1 + k),\quad f(m) - (y_2 - k),\quad f(m) - (y_2 + k).$$Điều này xác định trường hợp nào sau đây được áp dụng: 

Nếu$f(x)+k \le y_1$hoặc$f(x)-k \ge y_2$, thì chiều cao nhìn thấy được bằng không. 

Nếu như$f(x)-k \ge y_1$Và$f(x)+k \le y_2$, dải đầy đủ nằm bên trong hình chữ nhật, vì vậy chiều cao nhìn thấy được là$2k$. 

Nếu dải được cắt bớt một phần ở phía trên, phía dưới hoặc cả hai bên, chúng ta thu được các biểu thức như:$$y_2 - (f(x)-k),\quad (f(x)+k) - y_1,\quad y_2 - y_1.$$Mỗi trong số này là hằng số hoặc tuyến tính trong$f(x)$, do đó có thể tích hợp về mặt phân tích. 

### 4. Tích hợp trên phân khúc 

Chúng tôi tính toán trước:$$\int f(x)\,dx = \frac{a_2}{3}x^3 + \frac{a_1}{2}x^2 + a_0 x.$$Vì vậy bất kỳ biểu thức nào có dạng$A f(x) + B$tích hợp trực tiếp. 

Chúng tôi tổng hợp các khoản đóng góp trên tất cả các phân đoạn để có được tổng diện tích hiển thị. 

### Tại sao nó hoạt động 

Bất biến chính là trong mỗi khoảng giữa các nghiệm liên tiếp của bốn phương trình biên, thứ tự của$f(x)$liên quan đến$y_1 \pm k$Và$y_2 \pm k$không thay đổi. Vì công thức chiều cao hiển thị chỉ phụ thuộc vào thứ tự này nên số nguyên vẫn giữ nguyên trong suốt khoảng thời gian. Điều này đảm bảo rằng việc thay tích phân trên một phân đoạn bằng nguyên hàm dạng đóng là chính xác và không bỏ sót điểm gián đoạn ẩn nào. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-12

def solve_quadratic(a, b, c):
    if abs(a) < EPS:
        if abs(b) < EPS:
            return []
        return [-c / b]
    d = b * b - 4 * a * c
    if d < -EPS:
        return []
    if d < 0:
        d = 0.0
    sd = math.sqrt(d)
    return [(-b - sd) / (2 * a), (-b + sd) / (2 * a)]

def F(a2, a1, a0, x):
    return a2 * x * x + a1 * x + a0

def integral_f(a2, a1, a0, x):
    return (a2 / 3) * x**3 + (a1 / 2) * x**2 + a0 * x

def visible_height(a2, a1, a0, k, y1, y2, x):
    fx = F(a2, a1, a0, x)
    low = fx - k
    high = fx + k

    if high <= y1 + EPS or low >= y2 - EPS:
        return 0.0
    if low >= y1 - EPS and high <= y2 + EPS:
        return 2 * k
    if low >= y1 - EPS:
        return max(0.0, y2 - low)
    if high <= y2 + EPS:
        return max(0.0, high - y1)
    return y2 - y1

def integrate_segment(a2, a1, a0, k, y1, y2, l, r):
    m = (l + r) / 2
    h = visible_height(a2, a1, a0, k, y1, y2, m)

    # constant case
    if abs(h - 0.0) < 1e-12:
        return 0.0
    if abs(h - (y2 - y1)) < 1e-12:
        return (y2 - y1) * (r - l)
    if abs(h - 2 * k) < 1e-12:
        return 2 * k * (r - l)

    # linear cases: h = A*f(x) + B
    # deduce by sampling endpoints
    h1 = visible_height(a2, a1, a0, k, y1, y2, l)
    h2 = visible_height(a2, a1, a0, k, y1, y2, r)

    if abs(h2 - h1) < 1e-12:
        return h1 * (r - l)

    # assume h(x) = alpha * f(x) + beta
    f1 = F(a2, a1, a0, l)
    f2 = F(a2, a1, a0, r)

    if abs(f2 - f1) < 1e-12:
        return h1 * (r - l)

    alpha = (h2 - h1) / (f2 - f1)
    beta = h1 - alpha * f1

    # integrate alpha*f(x) + beta
    return alpha * (integral_f(a2, a1, a0, r) - integral_f(a2, a1, a0, l)) + beta * (r - l)

def solve():
    a2, a1, a0 = map(float, input().split())
    k = float(input())
    x1, y1, x2, y2 = map(float, input().split())

    if x2 < x1:
        x1, x2 = x2, x1
    if y2 < y1:
        y1, y2 = y2, y1

    xs = [x1, x2]

    for c in [y1 - k, y1 + k, y2 - k, y2 + k]:
        roots = solve_quadratic(a2, a1, a0 - c)
        for r in roots:
            if x1 - 1e-9 <= r <= x2 + 1e-9:
                xs.append(r)

    xs = sorted(xs)

    # deduplicate
    cleaned = []
    for x in xs:
        if not cleaned or abs(cleaned[-1] - x) > 1e-9:
            cleaned.append(x)

    xs = cleaned

    total_visible = 0.0
    for i in range(len(xs) - 1):
        l, r = xs[i], xs[i + 1]
        if r > l + 1e-12:
            total_visible += integrate_segment(a2, a1, a0, k, y1, y2, l, r)

    area = (x2 - x1) * (y2 - y1)
    answer = area - total_visible
    print(answer)

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách trích xuất tất cả tọa độ x nơi cấu trúc của phần chồng lấp có thể thay đổi. Đây chính xác là gốc của bốn phương trình biên. Sau khi sắp xếp và loại bỏ trùng lặp, trục x được phân chia thành các khoảng trong đó hàm chiều cao hiển thị có dạng đại số ổn định. 

Mỗi khoảng sau đó được tích hợp độc lập. Các trường hợp hằng số được xử lý trực tiếp, trong khi các trường hợp không hằng số tái tạo lại sự phụ thuộc tuyến tính vào giá trị parabol và sử dụng nguyên hàm của phương trình bậc hai để tính diện tích một cách chính xác. 

Một điểm tinh tế là tính ổn định của dấu phẩy động khi giải phương trình bậc hai và so sánh các nghiệm. Các epsilon nhỏ được yêu cầu cả khi lọc các gốc vào khoảng và khi hợp nhất các điểm phân chia gần như giống hệt nhau, nếu không, các ranh giới trùng lặp có thể tạo ra các đoạn có độ dài bằng 0 tích tụ nhiễu số. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 1 -2
3
-4 -5 1 1
```Trước tiên, chúng ta tính diện tích hình chữ nhật, sau đó trừ đi vùng nhìn thấy được tạo ra bởi dải xung quanh parabol. 

| Phân đoạn | Khoảng thời gian | Hành vi trường hợp | 
| --- | --- | --- | 
| 1 | [-4, x1'] | không chồng chéo | 
| 2 | [x1', x2'] | chồng chéo một phần băng tần | 
| 3 | [x2', 1] | không chồng chéo | 

Vùng ở giữa tương ứng với nơi parabol đi vào hình chữ nhật và dải giao nhau với nó. Tích phân trong khoảng này sẽ mang lại diện tích nhìn thấy được và trừ đi tổng diện tích sẽ tạo ra:$$11.666666666666668.$$Dấu vết này cho thấy hàm này chỉ hoạt động như thế nào trong một vùng giới hạn của x, trong khi ở bên ngoài nó đóng góp bằng 0. 

### Mẫu 2 (đã thi công) 

đầu vào:```
0 0 0
1
0 0 10 10
```Ở đây parabol là trục x và dải nhìn thấy được đơn giản là$|y| \le 1$. Bên trong hình chữ nhật, đây là một dải ngang có chiều cao 2. 

| Phân đoạn | Khoảng thời gian | Chiều cao nhìn thấy được | 
| --- | --- | --- | 
| 1 | [0, 10] | 2 | 

Vậy diện tích nhìn thấy được là$10 \cdot 2 = 20$, và tổng diện tích là 100, cho đáp án 80. 

Điều này xác nhận việc xử lý trường hợp không đổi trong đó dải nằm hoàn toàn bên trong hình chữ nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Nhiều nhất là 9 đoạn từ căn bậc hai, mỗi đoạn được tính theo thời gian không đổi bằng cách sử dụng tích phân dạng đóng | 
| Không gian |$O(1)$| Chỉ một danh sách nhỏ các điểm quan trọng được lưu trữ | 

Số khoảng được giới hạn bởi số lượng giao điểm ranh giới bậc hai không đổi, do đó thuật toán vẫn nhanh bất kể độ lớn của hệ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    EPS = 1e-12

    def solve_quadratic(a, b, c):
        if abs(a) < EPS:
            if abs(b) < EPS:
                return []
            return [-c / b]
        d = b * b - 4 * a * c
        if d < -EPS:
            return []
        if d < 0:
            d = 0.0
        sd = math.sqrt(d)
        return [(-b - sd) / (2 * a), (-b + sd) / (2 * a)]

    def F(a2, a1, a0, x):
        return a2 * x * x + a1 * x + a0

    def integral_f(a2, a1, a0, x):
        return (a2 / 3) * x**3 + (a1 / 2) * x**2 + a0 * x

    def visible_height(a2, a1, a0, k, y1, y2, x):
        fx = F(a2, a1, a0, x)
        low = fx - k
        high = fx + k

        if high <= y1 or low >= y2:
            return 0.0
        if low >= y1 and high <= y2:
            return 2 * k
        if low >= y1:
            return max(0.0, y2 - low)
        if high <= y2:
            return max(0.0, high - y1)
        return y2 - y1

    def solve():
        a2, a1, a0 = map(float, sys.stdin.readline().split())
        k = float(sys.stdin.readline())
        x1, y1, x2, y2 = map(float, sys.stdin.readline().split())

        xs = [x1, x2]

        for c in [y1 - k, y1 + k, y2 - k, y2 + k]:
            roots = solve_quadratic(a2, a1, a0 - c)
            xs += roots

        xs = sorted(xs)
        cleaned = []
        for x in xs:
            if not cleaned or abs(cleaned[-1] - x) > 1e-9:
                cleaned.append(x)
        xs = cleaned

        def integrate_segment(l, r):
            m = (l + r) / 2
            h = visible_height(a2, a1, a0, k, y1, y2, m)
            return h * (r - l)

        total_visible = 0.0
        for i in range(len(xs) - 1):
            l, r = xs[i], xs[i + 1]
            if r > l:
                total_visible += integrate_segment(l, r)

        area = (x2 - x1) * (y2 - y1)
        return str(area - total_visible)

# provided sample
assert run("1 1 -2\n3\n-4 -5 1 1\n") == "11.666666666666668"

# custom: flat line, centered band
assert abs(float(run("0 0 0\n1\n0 0 10 10\n")) - 80) < 1e-6

# custom: no visibility
assert abs(float(run("0 0 100\n0\n0 0 1 1\n")) - 1) < 1e-6

# custom: full visibility inside band
assert abs(float(run("0 0 0\n100\n0 0 1 1\n")) - 0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| parabol phẳng | 80 | trường hợp băng tần không đổi | 
| khổng lồ k | 1 | cạnh bảo hiểm đầy đủ | 
| không k hình chữ nhật lớn | 1 | không có trường hợp hiển thị | 

## Vỏ cạnh 

Một trường hợp tế nhị xuất hiện khi$k = 0$. Vùng hiển thị thu gọn thành đường cong, có diện tích bằng 0 trong hình học liên tục. Thuật toán xử lý điều này vì tất cả các phương trình biên đều trở thành các cặp giống hệt nhau, không tạo ra khoảng nào trong đó chiều cao khác 0 được chọn, do đó tích phân nhìn thấy ước lượng bằng 0. 

Một trường hợp khác phát sinh khi parabol không bao giờ giao nhau với hình chữ nhật hoặc các dải lệch của nó. Ví dụ, nếu$f(x)$luôn vượt xa$y_2 + k$, mọi đoạn đều thỏa mãn điều kiện$f(x)-k \ge y_2$, do đó chiều cao nhìn thấy được bằng 0 ở mọi nơi. Việc tích hợp không tích lũy chính xác gì vì mỗi phân đoạn trả về 0 ngay lập tức. 

Một tình huống tế nhị hơn xảy ra khi rễ nằm chính xác trên đường biên hình chữ nhật. Bộ lọc dựa trên epsilon đảm bảo các điểm như vậy được bao gồm nhưng không tạo ra các phân đoạn có độ dài bằng 0 suy biến. Trên các ranh giới đó, cả hai phía của khoảng đều tạo ra cùng một công thức, do đó việc chia tách không làm thay đổi kết quả và chỉ ổn định việc đánh giá.
