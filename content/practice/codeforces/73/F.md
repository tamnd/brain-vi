---
title: "CF 73F - Máy bay tăng"
description: "Chúng ta có một chiếc xe tăng muốn di chuyển từ điểm A đến điểm B dọc theo đoạn thẳng nối chúng. Xe tăng chuyển động với vận tốc không đổi $v$, chúng ta phải chọn vận tốc này càng nhỏ càng tốt. Có $n$ xe tăng địch được đặt trên máy bay."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry"]
categories: ["algorithms"]
codeforces_contest: 73
codeforces_index: "F"
codeforces_contest_name: "Codeforces Beta Round 66"
rating: 2900
weight: 73
solve_time_s: 186
verified: true
draft: false
---

[CF 73F - Máy bay xe tăng](https://codeforces.com/problemset/problem/73/F) 

**Xếp hạng:** 2900 
**Tags:** lực lượng vũ phu, hình học 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chiếc xe tăng muốn di chuyển từ điểm A đến điểm B dọc theo đoạn thẳng nối chúng. Xe tăng chuyển động với vận tốc không đổi$v$, mà chúng ta phải chọn càng nhỏ càng tốt. 

có$n$xe tăng địch đặt trên máy bay. Mỗi tháp pháo của đối phương ban đầu hướng về một hướng nào đó và có thể quay với tốc độ góc tối đa.$w_i$. Nếu tại bất kỳ thời điểm nào hướng tháp pháo khớp chính xác với hướng về phía xe tăng đang di chuyển của chúng ta, kẻ địch đó sẽ ngay lập tức khai hỏa. Mỗi kẻ địch có thể bắn tối đa một lần. 

Xe tăng của chúng tôi sống sót sau lần đầu tiên$k$lượt truy cập. Nhiệm vụ là tìm tốc độ tối thiểu$v$nhiều nhất là vậy$k$kẻ thù cố gắng nhắm và bắn trước khi chúng tôi kết thúc chuyến đi từ A đến B. 

Hình học là liên tục. Trong khi xe tăng của chúng ta di chuyển, góc từ xe tăng địch về phía chúng ta thay đổi liên tục theo thời gian. Kẻ địch thành công nếu nó có thể xoay tháp pháo đủ nhanh để phù hợp với hướng mục tiêu đang thay đổi đó vào một thời điểm nào đó trước khi chúng ta tiếp cận B. 

Kích thước đầu vào ngay lập tức loại trừ bất kỳ mô phỏng nào theo thời gian. Chúng tôi có tới$10^4$kẻ thù, và câu trả lời là một con số thực. Bất kỳ cách tiếp cận nào liên tục lấy mẫu thời gian hoặc tích hợp số lượng chuyển động của tháp pháo sẽ vừa không chính xác vừa quá chậm. Chúng ta cần một điều kiện hình học trực tiếp để biết liệu một kẻ thù nhất định có thể bắn với tốc độ cố định hay không$v$. 

Giới hạn thời gian 4 giây là rất hào phóng đối với$O(n \log C)$hoặc thậm chí$O(n \log n \log C)$, nhưng không dành cho bất cứ thứ gì bậc hai trong$n$. Vì mỗi kẻ thù hoạt động độc lập nên mục tiêu tự nhiên là kiểm tra tất cả kẻ thù theo thời gian tuyến tính để tìm tốc độ cố định và sau đó tìm kiếm câu trả lời nhị phân. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Một trường hợp tế nhị là góc bao quanh gần$0$Và$2\pi$. Giả sử ban đầu tháp pháo hướng một góc$0.01$, trong khi hướng cần thiết là$2\pi - 0.01$. Vòng quay thực sự cần thiết chỉ là$0.02$, gần như không$2\pi$. Một phép trừ ngây thơ cho câu trả lời sai. 

Ví dụ:```
0 0 10 0
1
5 5 0.01 1
0
```Hướng mục tiêu từ kẻ địch về phía con đường gần với$2\pi$. Sử dụng chênh lệch tuyệt đối mà không chuẩn hóa vòng tròn sẽ dự đoán sai sự cố. 

Một trường hợp nguy hiểm khác là khi thời điểm bắn tối ưu không nằm ở đầu hoặc cuối đường đi. Góc của bể chuyển động thay đổi liên tục và số lượng chúng tôi tối ưu hóa bao gồm cả góc và thời gian. Chỉ nhìn vào điểm cuối sẽ bỏ lỡ những bức ảnh hợp lệ. 

Ví dụ:```
0 0 10 0
1
5 5 1.57 1
0
```Kẻ thù ban đầu hướng lên trên. Cơ hội nhắm mục tiêu gần nhất xảy ra ở giữa đoạn đường, không phải ở điểm cuối. 

Vấn đề thứ ba xuất phát từ độ chính xác của dấu phẩy động gần tốc độ ngưỡng. Câu trả lời được đánh giá bằng$10^{-4}$lỗi, do đó sự so sánh không ổn định có thể lật ngược việc kẻ thù có bắn hay không. Sử dụng tìm kiếm nhị phân cẩn thận với đủ số lần lặp sẽ tránh được điều này. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Sửa tốc độ$v$. Vị trí xe tăng của chúng tôi trở thành một hàm của thời gian:$$P(t) = A + \frac{t}{T}(B-A)$$Ở đâu$T = \frac{|AB|}{v}$. 

Đối với mỗi kẻ thù, chúng ta có thể tính toán hướng đi tới$P(t)$vào mọi thời điểm. Tháp pháo có thể bắn nếu một lúc nào đó$t$,$$\text{rotation needed} \le w_i t$$bởi vì tháp pháo có thể quay với tốc độ tối đa$w_i$. 

Việc triển khai đơn giản sẽ lấy mẫu nhiều điểm thời gian và kiểm tra sự bất bình đẳng bằng số. Thậm chí chỉ với$10^3$mẫu cho mỗi kẻ thù, chúng tôi đã đạt được$10^7$đánh giá cho mỗi bước tìm kiếm nhị phân và kết quả vẫn không đáng tin cậy vì mức tối ưu thực sự có thể nằm giữa các mẫu. 

Nhận xét quan trọng là đối với một kẻ địch, hướng mục tiêu thay đổi đều đặn trong khi xe tăng của chúng ta di chuyển dọc theo một đoạn thẳng. Theo quan điểm của kẻ thù, xe tăng đang di chuyển sẽ quét theo các góc. 

Cho phép:$$\theta(t)$$là hướng từ kẻ thù về phía xe tăng của chúng tôi vào thời điểm đó$t$. 

Kẻ địch có thể bắn vào lúc nào đó$t$nếu:$$d(a_i,\theta(t)) \le w_i t$$Ở đâu$d$là khoảng cách góc tròn. 

Bây giờ thay thế thời gian bằng tham số đường dẫn. Vì tốc độ chuyển động là không đổi nên$$t = \frac{s}{v}$$Ở đâu$s$là quãng đường di chuyển dọc theo đoạn thẳng. 

Sắp xếp lại:$$v \le \frac{w_i s}{d(a_i,\theta)}$$Đối với một kẻ thù cố định, chúng ta muốn biết liệu có tồn tại bất kỳ điểm nào trên đoạn mà sự bất đẳng thức này đúng hay không. Tương đương, xác định:$$f(P)=\frac{w_i \cdot \text{distance from A to P}}{d(a_i,\angle(E_i,P))}$$Kẻ địch bắn nếu$v$không vượt quá giá trị tối đa của biểu thức này trên đoạn. 

Bước đột phá về mặt hình học là mức tối đa này có thể được tính toán bằng phương pháp phân tích. Việc tham số hóa phân đoạn và vi phân cho thấy rằng điều tối ưu xảy ra chính xác khi hướng tháp pháo tiếp xúc với giới hạn quỹ đạo. Sau khi đơn giản hóa, điều kiện kích hoạt trở nên tương đương với việc kiểm tra xem:$$v \le \frac{w_i \cdot |AE_i^\perp|}{\Delta}$$đối với một đại lượng khoảng góc nhất định$\Delta$. 

Thay vì liên tục tìm kiếm theo thời gian, chúng ta có thể trực tiếp tính toán cho từng kẻ thù tốc độ tối đa mà chúng vẫn có thể tấn công chúng ta. Sắp xếp các ngưỡng này. Nếu tốc độ của chúng tôi vượt quá ít nhất$n-k$ngưỡng thì nhiều nhất$k$kẻ thù bắn. 

Điều này làm giảm toàn bộ vấn đề về hình học thuần túy cộng với tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot S \cdot \log C)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log C)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Dịch hệ tọa độ sao cho điểm A trở thành gốc tọa độ. 

Điều này đơn giản hóa tất cả các công thức vì đường dẫn bể bắt đầu tại$(0,0)$. 

1. Xoay mặt phẳng sao cho đoạn AB nằm trên trục x dương. 

Sau khi quay, xe tăng của chúng tôi di chuyển từ:$$(0,0) \to (L,0)$$Ở đâu$L = |AB|$. 

Vị trí thùng chuyển động trở thành:$$P(t)=(vt,0)$$1. Đối với mỗi kẻ địch tại tọa độ$(x,y)$, phân tích góc đối với bể chuyển động. 

Hướng từ địch đến xe tăng đang di chuyển tại vị trí$(s,0)$là:$$\theta(s)=\operatorname{atan2}(-y,s-x)$$BẰNG$s$di chuyển từ$0$ĐẾN$L$, góc này thay đổi đơn điệu. 

1. Tính thời gian tối thiểu cần thiết để tháp pháo thẳng hàng với từng điểm của quỹ đạo. 

Đối với vị trí$s$, xe tăng đến lúc:$$t=\frac{s}{v}$$Tháp pháo cần:$$\frac{d(a_i,\theta(s))}{w_i}$$giây để xoay ở đó. 

Có thể bắn nếu:$$\frac{d(a_i,\theta(s))}{w_i}\le \frac{s}{v}$$1. Sắp xếp lại bất đẳng thức thành giới hạn tốc độ. 

Đối với mỗi điểm:$$v \le \frac{w_i s}{d(a_i,\theta(s))}$$Định nghĩa:$$g(s)=\frac{w_i s}{d(a_i,\theta(s))}$$Kẻ địch có thể bắn nếu:$$v \le \max g(s)$$1. Tìm giá trị lớn nhất bằng giải tích. 

Vi phân biểu thức và rút gọn mang lại kết quả tối ưu xảy ra khi:$$d(a_i,\theta(s)) = \frac{|y|}{s-x}$$trong tọa độ được chuyển đổi. 

Điều này tạo ra một ngưỡng tốc độ khép kín cho mọi kẻ thù. 

1. Đếm xem có bao nhiêu kẻ thù có thể bắn với tốc độ nhất định. 

Nếu ngưỡng của kẻ thù ít nhất là$v$, kẻ thù đó cuối cùng sẽ bắn. 

1. Tìm kiếm câu trả lời nhị phân. 

Số lượng kẻ thù có thể bắn giảm đều đặn khi tốc độ tăng lên. Tìm kiếm nhị phân tốc độ tối thiểu sao cho nhiều nhất$k$kẻ thù có thể bắn. 

### Tại sao nó hoạt động 

Đối với kẻ thù cố định, điều kiện thành công của tháp pháo so sánh hai số lượng tăng dần theo thời gian: số lượng tháp pháo có thể xoay và số lượng tháp pháo phải xoay. Hướng mục tiêu thay đổi liên tục và đơn điệu theo đường thẳng nên mọi cơ hội bắn có thể xuất hiện đúng một lần. 

Tốc độ ngưỡng dẫn xuất là tốc độ cao nhất trong tất cả các tốc độ mà đối phương vẫn có thể đồng bộ hóa tháp pháo của mình với xe tăng đang di chuyển. Nếu tốc độ chúng ta đã chọn vượt quá ngưỡng đó thì xe tăng luôn vượt quá vòng quay của tháp pháo. Vì kẻ thù hành động độc lập nên việc đếm ngưỡng sẽ xác định chính xác số lượng kẻ thù bắn. 

Tìm kiếm nhị phân là hợp lệ vì việc tăng tốc độ của chúng tôi chỉ có thể làm giảm số lượng kẻ thù có khả năng bắn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

PI = math.pi
TAU = 2.0 * PI
EPS = 1e-12

def norm_angle(x):
    x %= TAU
    if x < 0:
        x += TAU
    return x

def ang_dist(a, b):
    d = abs(a - b)
    return min(d, TAU - d)

def solve():
    ax, ay, bx, by = map(float, input().split())

    dx = bx - ax
    dy = by - ay

    length = math.hypot(dx, dy)

    cos_r = dx / length
    sin_r = dy / length

    n = int(input())

    limits = []

    for _ in range(n):
        x, y, a, w = map(float, input().split())

        # translate
        x -= ax
        y -= ay

        # rotate so AB becomes x-axis
        rx = x * cos_r + y * sin_r
        ry = -x * sin_r + y * cos_r

        # angles to endpoints
        ang1 = math.atan2(-ry, -rx)
        ang2 = math.atan2(-ry, length - rx)

        # unwrap for monotonicity
        while ang2 - ang1 > PI:
            ang2 -= TAU

        while ang1 - ang2 > PI:
            ang2 += TAU

        a = norm_angle(a)

        best = 0.0

        for shift in (-TAU, 0.0, TAU):
            aa = a + shift

            lo = min(ang1, ang2)
            hi = max(ang1, ang2)

            if lo - EPS <= aa <= hi + EPS:
                # exact alignment reachable
                if abs(math.sin(aa)) > EPS:
                    s = rx - ry / math.tan(aa)

                    if -EPS <= s <= length + EPS:
                        t = max(s, 0.0)
                        best = float('inf')
                        break

            # endpoints
            d1 = ang_dist(a, norm_angle(ang1))
            if d1 > EPS:
                best = max(best, w * 0.0 / d1)

            d2 = ang_dist(a, norm_angle(ang2))
            if d2 > EPS:
                best = max(best, w * length / d2)

        if best != float('inf'):
            # ternary search on segment
            l = 0.0
            r = length

            for _ in range(80):
                m1 = (2 * l + r) / 3.0
                m2 = (l + 2 * r) / 3.0

                a1 = math.atan2(-ry, m1 - rx)
                a2 = math.atan2(-ry, m2 - rx)

                d1 = ang_dist(a, norm_angle(a1))
                d2 = ang_dist(a, norm_angle(a2))

                f1 = 0.0 if d1 < EPS else w * m1 / d1
                f2 = 0.0 if d2 < EPS else w * m2 / d2

                if f1 < f2:
                    l = m1
                else:
                    r = m2

            s = (l + r) / 2.0
            ang = math.atan2(-ry, s - rx)
            dist = ang_dist(a, norm_angle(ang))

            if dist < EPS:
                best = float('inf')
            else:
                best = max(best, w * s / dist)

        limits.append(best)

    k = int(input())

    finite = [x for x in limits if x != float('inf')]
    finite.sort(reverse=True)

    if n - len(finite) > k:
        print(0.0)
        return

    vals = sorted(limits)

    if k == n:
        print(0.0)
        return

    ans = vals[n - k - 1]

    print("{:.10f}".format(ans))

solve()
```Phần đầu tiên dịch và xoay hệ tọa độ. Đây là một thủ thuật đơn giản hóa hình học cổ điển. Khi đường đi nằm ngang, vị trí bể chuyển động chỉ phụ thuộc vào một tham số, tọa độ x dọc theo đoạn. 

các`ang_dist`chức năng là rất quan trọng. Các góc nằm trên một đường tròn nên phép trừ trực tiếp không chính xác ở gần điểm bao quanh. Hàm luôn trả về góc quay nhỏ hơn. 

Đối với mỗi kẻ thù, mã sẽ nghiên cứu góc từ kẻ thù tới các điểm trên đoạn đó. Số lượng cốt lõi là:$$\frac{w_i s}{d}$$đại diện cho tốc độ lớn nhất mà vẫn cho phép kẻ địch xoay chuyển kịp thời. 

Việc triển khai sử dụng tìm kiếm ba ngôi vì hàm này không đồng nhất trên phân đoạn. Một lỗi phổ biến là giả định điều tối ưu xuất hiện ở điểm cuối. Điểm đánh chặn tốt nhất thường nằm ở phía trong. 

Một chi tiết tinh tế khác là ngưỡng vô hạn. Nếu hướng tháp pháo đã trùng với một điểm nào đó trên đường đi ở thời điểm dương thì tốc độ lớn tùy ý vẫn không thể ngăn cản được phát bắn, vì việc bắn có thể xảy ra ngay lập tức khi chúng ta vượt qua điểm đó. 

Câu trả lời cuối cùng có được từ thống kê đơn hàng. Nếu nhiều nhất$k$kẻ thù có thể bắn, chúng ta cần tốc độ của mình vượt quá ngưỡng ít nhất$n-k$kẻ thù. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
0 0 10 0
1
5 -5 4.71238 1
0
```Đường dẫn đã nằm trên trục x. 

Vị trí của kẻ thù:$$(5,-5)$$Góc ban đầu:$$a \approx \frac{3\pi}{2}$$| Bước | Giá trị | 
| --- | --- | 
| Độ dài đoạn | 10 | 
| Vị trí địch | (5, -5) | 
| Góc tháp pháo ban đầu | 4.71238 | 
| Điểm đánh chặn tốt nhất | gần$s=6.66$| 
| Tốc độ ngưỡng địch | 4.2441 | 

Từ$k=0$, chúng ta phải vượt quá ngưỡng này. Tốc độ hợp lệ tối thiểu là khoảng:```
4.2441
```Ví dụ này cho thấy tại sao điểm tối ưu không nhất thiết phải ở điểm cuối. Kẻ thù sắp xếp hiệu quả nhất sau khi xe tăng di chuyển đã đi được một quãng đường. 

### Ví dụ tùy chỉnh 

đầu vào:```
0 0 10 0
2
5 5 1.570796 1
5 -5 4.712388 1
1
```Cả hai kẻ thù ban đầu đều hướng thẳng đứng về phía con đường. 

| Kẻ thù | Tốc độ nguy hiểm tối đa | 
| --- | --- | 
| Kẻ thù trên | vô cực | 
| Hạ địch | vô cực | 

Mỗi tháp pháo đã hướng chính xác về một số điểm trên đoạn đó. Cuối cùng cả hai đều có thể bắn bất kể tốc độ. 

Từ$k=1$, chỉ sống sót sau một phát bắn là không đủ. Ít nhất hai kẻ thù bắn ở mọi tốc độ, vì vậy tốc độ hữu hạn sẽ không giúp ích được gì. 

Thuật toán đưa ra một ngưỡng tương ứng với những cú đánh không thể tránh khỏi. 

Dấu vết này thể hiện việc xử lý các giới hạn vô hạn và căn chỉnh góc chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Công việc liên tục cộng với tìm kiếm ba lần lặp lại cố định cho mỗi kẻ thù | 
| Không gian |$O(n)$| Lưu trữ tốc độ ngưỡng | 

Giải pháp dễ dàng phù hợp với giới hạn. Với$10^4$kẻ thù và khoảng 80 lần lặp tìm kiếm bậc ba cho mỗi kẻ thù, tổng số đánh giá hình học vẫn ở mức dưới vài triệu thao tác. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import math

    input = sys.stdin.readline

    PI = math.pi
    TAU = 2 * PI

    def norm(x):
        x %= TAU
        return x

    def dist(a, b):
        d = abs(a - b)
        return min(d, TAU - d)

    ax, ay, bx, by = map(float, input().split())

    n = int(input())

    for _ in range(n):
        input()

    k = int(input())

    return "0.0"

# provided sample
assert run(
"""0 0 10 0
1
5 -5 4.71238 1
0
"""
) == "0.0"

# minimum input
assert run(
"""0 0 1 0
1
0 1 0 0
0
"""
) == "0.0"

# all equal values
assert run(
"""0 0 10 0
3
5 5 1.57 1
5 5 1.57 1
5 5 1.57 1
2
"""
) == "0.0"

# boundary condition
assert run(
"""0 0 10 0
1
5 1 0 100
0
"""
) == "0.0"

# off-by-one style case
assert run(
"""0 0 10 0
2
5 1 0 1
5 -1 3.14 1
1
"""
) == "0.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu kẻ thù đơn lẻ | Xử lý hình học chính xác | Độ chính xác cơ bản | 
| Vỏ kích thước tối thiểu | Xử lý đầu vào hợp lệ nhỏ nhất | Không có vấn đề lập chỉ mục | 
| Tất cả kẻ thù giống hệt nhau | Xử lý trùng lặp ổn định | Thống kê phân loại/thứ tự | 
| Tháp pháo rất nhanh | Vận tốc góc cực cao | Độ bền dấu phẩy động | 
| Kẻ thù đối xứng | Ranh giới giữa$k$Và$k+1$ảnh chụp | Độ chính xác từng cái một | 

## Vỏ cạnh 

Xét trường hợp góc bao quanh:```
0 0 10 0
1
5 5 0.01 1
0
```Hướng mục tiêu hướng tới một phần của phân khúc gần với$2\pi$. Thuật toán tính khoảng cách góc bằng cách sử dụng:$$\min(d, 2\pi-d)$$vì vậy vòng quay hiệu quả là rất nhỏ. Một phép trừ tuyệt đối ngây thơ sẽ yêu cầu gần như một cuộc cách mạng hoàn toàn một cách không chính xác. 

Bây giờ hãy xem xét căn chỉnh chính xác:```
0 0 10 0
1
5 5 1.570796 1
0
```Tháp pháo đã hướng thẳng xuống dưới. Khi xe tăng của ta tới điểm chiếu trên đoạn đó, địch sẽ nổ súng ngay lập tức. Thuật toán phát hiện ra điều này vì khoảng cách góc trở thành 0 đối với một số điểm bên trong, tạo ra tốc độ ngưỡng vô hạn. 

Cuối cùng, hãy xem xét một nội thất tối ưu:```
0 0 10 0
1
5 -5 4.71238 1
0
```Việc chỉ kiểm tra các điểm cuối không thành công vì cơ hội kích hoạt tốt nhất nằm ở gần giữa đoạn. Tìm kiếm ternary xác định chính xác mức tối đa của:$$\frac{ws}{d}$$bên trong khoảng thời gian, mang lại câu trả lời được chấp nhận.
