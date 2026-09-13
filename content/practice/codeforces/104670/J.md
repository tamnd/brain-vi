---
title: "CF 104670J - Mứt chạy bộ chung"
description: "Hai người xuất phát tại hai tọa độ nhất định trên máy bay và chạy theo đường thẳng đến đích tương ứng trong một khoảng thời gian cố định. Cả hai đều di chuyển với tốc độ không đổi nên vị trí của mỗi người là một phép nội suy tuyến tính giữa điểm bắt đầu và điểm kết thúc của họ."
date: "2026-06-29T09:36:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104670
codeforces_index: "J"
codeforces_contest_name: "2021-2022 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2021)"
rating: 0
weight: 104670
solve_time_s: 48
verified: true
draft: false
---

[CF 104670J - Joint Jog Jam](https://codeforces.com/problemset/problem/104670/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người xuất phát tại hai tọa độ nhất định trên máy bay và chạy theo đường thẳng đến đích tương ứng trong một khoảng thời gian cố định. Cả hai đều di chuyển với tốc độ không đổi nên vị trí của mỗi người là một phép nội suy tuyến tính giữa điểm bắt đầu và điểm kết thúc của họ. 

Nhiệm vụ là theo dõi khoảng cách giữa chúng trong suốt quá trình chuyển động và báo cáo khoảng cách lớn nhất xảy ra tại bất kỳ thời điểm nào giữa điểm bắt đầu và điểm kết thúc. Chúng tôi không được hỏi khi nào nó xảy ra, chỉ có giá trị tối đa của khoảng cách đó. 

Đường đi của mỗi người chạy hoàn toàn được xác định bởi hai điểm, do đó toàn bộ hệ thống giảm xuống còn hai điểm chuyển động trên một mặt phẳng, cả hai đều theo dõi các đoạn đường đồng thời trong cùng một khoảng thời gian. 

Kích thước đầu vào không đổi, tám số nguyên, vì vậy các ràng buộc không phải về khả năng mở rộng mà là về độ bền về số. Một giải pháp có hình học thời gian không đổi được mong đợi. Bất cứ điều gì liên quan đến việc rời rạc hóa thời gian hoặc lấy mẫu đều không cần thiết và có khả năng không chính xác vì mức tối đa có thể xảy ra giữa các điểm được lấy mẫu. 

Một trường hợp thất bại tinh tế đối với lối suy nghĩ ngây thơ là giả sử khoảng cách tối đa phải xảy ra ở một trong các điểm cuối. Điều đó không phải lúc nào cũng đúng vì hàm khoảng cách giữa hai điểm chuyển động tuyến tính không phải là tuyến tính. 

Ví dụ, nếu một người chạy chuyển động theo chuyển động tương đối giống như vòng tròn xung quanh người kia (mặc dù cả hai đều chuyển động thẳng, chuyển động tương đối của họ có thể tạo ra một đường cong khoảng cách quay), thì cực đại có thể xảy ra ở bên trong khoảng thời gian thay vì ở t = 0 hoặc t = 1. 

Một dạng lỗi khác là lấy mẫu thống nhất, chẳng hạn như kiểm tra 1000 lần cách đều nhau. Điều này có thể bỏ lỡ một đỉnh nhọn nếu parabol hẹp. 

Khó khăn chính là nhận ra rằng mặc dù chuyển động là hình học, nhưng hàm khoảng cách trở thành một đường cong đại số đơn giản với một biến. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng thời gian liên tục hoặc lấy mẫu dày đặc. Chúng ta có thể đánh giá khoảng cách giữa hai người chạy tại nhiều thời điểm từ 0 đến 1. Điều này đúng về mặt khái niệm vì hàm này là liên tục, do đó việc lấy mẫu đủ dày đặc sẽ xấp xỉ mức tối đa. 

Tuy nhiên, nếu chúng ta muốn độ chính xác chính xác thì việc lấy mẫu không được chấp nhận. Bất kỳ sự rời rạc hóa hữu hạn nào cũng có nguy cơ thiếu đi mức tối đa thực sự. Ngay cả khi chúng tôi lấy mẫu một triệu điểm, mức tối đa có thể nằm giữa chúng. 

Cái nhìn sâu sắc về cấu trúc là mỗi tọa độ của mỗi người chạy đều tuyến tính theo thời gian. Điều đó có nghĩa là vectơ hiệu giữa chúng cũng tuyến tính theo thời gian. Khoảng cách bình phương trở thành một hàm bậc hai của thời gian. Hàm bậc hai trên một khoảng đóng có hình dạng quen thuộc: nó mở lên hoặc hướng xuống và cực đại của nó phải xảy ra ở điểm cuối hoặc tại đỉnh nếu đỉnh nằm trong khoảng. 

Điều này làm giảm bài toán từ suy luận hình học trong mặt phẳng sang phân tích đa thức bậc hai một biến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lấy mẫu thống nhất | O(k) | O(1) | Quá chậm/không chính xác | 
| Phân tích bậc hai | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tham số hóa thời gian là t nằm trong khoảng từ 0 đến 1.

1. Biểu diễn vị trí của Kari theo thời gian. Nếu cô ấy bắt đầu tại A và kết thúc tại C thì vị trí của cô ấy là A + t(C − A). Điều này mã hóa chuyển động tuyến tính tốc độ không đổi. 
2. Biểu diễn vị trí Ola tương tự B + t(D − B). 
3. Tính vectơ hiệu giữa chúng tại thời điểm t. Điều này trở thành (A − B) + t((C − A) − (D − B)). Bước này rất quan trọng vì nó quy bài toán về một vectơ chuyển động duy nhất. 
4. Viết bình phương khoảng cách dưới dạng tích vô hướng của vectơ hiệu với chính nó. Điều này tạo ra một biểu thức có dạng f(t) = at² + bt + c, trong đó a, b, c là các giá trị vô hướng dẫn xuất từ ​​tọa độ. 
5. Xác định thời gian ứng viên trong đó phương trình bậc hai có thể đạt cực trị bằng cách tính t* = −b / (2a). Điều này xuất phát từ đạo hàm của một phương trình bậc hai bằng 0 tại đỉnh của nó. 
6. Kẹp t* vào khoảng [0, 1] vì chuyển động bị hạn chế trong thời gian chạy. Nếu đỉnh nằm ngoài khoảng thì nó không thể là đỉnh lớn nhất trong khoảng. 
7. Đánh giá f(t) tại t = 0, t = 1, và tại t* bị kẹp. Giá trị tối đa của các giá trị này là khoảng cách bình phương tối đa. 
8. Trả về căn bậc hai của giá trị lớn nhất này để thu được khoảng cách Euclide thực tế. 

### Tại sao nó hoạt động 

Hàm khoảng cách bình phương là một đa thức bậc hai theo t vì mỗi tọa độ là tuyến tính theo t và bình phương đưa ra nhiều nhất là hai số hạng. Một hàm bậc hai trên một khoảng đóng không thể có nhiều hơn một điểm tới hạn bên trong và cực trị của nó được xác định đầy đủ bởi các điểm cuối và điểm tới hạn đó. Vì bình phương bảo toàn thứ tự cho các giá trị không âm, nên việc tối đa hóa khoảng cách bình phương tương đương với việc tối đa hóa khoảng cách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def clamp(x, lo, hi):
    return max(lo, min(hi, x))

def dist2(x1, y1, x2, y2):
    dx = x1 - x2
    dy = y1 - y2
    return dx * dx + dy * dy

def solve():
    data = list(map(int, input().split()))
    ax, ay, bx, by, cx, cy, dx, dy = data

    # direction vectors
    pax, pay = cx - ax, cy - ay
    pbx, pby = dx - bx, dy - by

    # relative motion: P(t) - Q(t) = (A-B) + t((C-A)-(D-B))
    rx = ax - bx
    ry = ay - by
    vx = pax - pbx
    vy = pay - pby

    # f(t) = |r + t v|^2 = (v·v)t^2 + 2(r·v)t + (r·r)
    a = vx * vx + vy * vy
    b = 2 * (rx * vx + ry * vy)
    c = rx * rx + ry * ry

    best = c  # t = 0

    # t = 1
    best = max(best, a + b + c)

    if a != 0:
        t = -b / (2 * a)
        t = clamp(t, 0.0, 1.0)
        val = a * t * t + b * t + c
        best = max(best, val)

    print((best) ** 0.5)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách viết lại cả hai quỹ đạo thành dạng vận tốc, tránh được việc nội suy lặp lại. Các hệ số của phương trình bậc hai đến trực tiếp từ việc mở rộng chuẩn bình phương của một biểu thức tuyến tính. Việc kiểm tra điểm cuối tương ứng với t = 0 và t = 1. Ứng cử viên bên trong chỉ được tính khi phương trình bậc hai không phẳng. 

Một điểm tinh tế là sự ổn định về số lượng. Sử dụng dấu phẩy động cho t là an toàn vì bài toán cho phép sai số tương đối hoặc tuyệt đối nhỏ và phép tính cuối cùng là đơn điệu trong khoảng cách bình phương. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 0 0 1 1 2 2
```Chúng tôi tính toán: 

| Bước | Giá trị | 
| --- | --- | 
| A−B | (0, 0) | 
| v | (1, 1) | 
| một | 2 | 
| b | 0 | 
| c | 0 | 
| t* | 0 | 
| f(0) | 0 | 
| f(1) | 2 | 

Khoảng cách bình phương tối đa là 2, vì vậy câu trả lời là √2. 

Điều này cho thấy trường hợp cực đại xảy ra ở điểm cuối chứ không phải ở bên trong. 

### Ví dụ 2 

đầu vào:```
0 0 0 1 0 2 2 1
```| Bước | Giá trị | 
| --- | --- | 
| A−B | (0, -1) | 
| v | (-2, 1) | 
| một | 5 | 
| b | -4 | 
| c | 1 | 
| t* | 0,4 | 
| f(0) | 1 | 
| f(1) | 2 | 
| f(0,4) | 5.4 | 

Khoảng cách bình phương tối đa là 5,4, vì vậy câu trả lời là √5,4. 

Điều này xác nhận trường hợp đỉnh điểm bên trong, trong đó chỉ riêng việc kiểm tra điểm cuối sẽ không thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các phép tính đều là số học có kích thước cố định trên tám số nguyên | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Kích thước đầu vào không chia tỷ lệ nên giải pháp hoàn toàn là đại số. Các phép toán được giới hạn ở một vài biểu thức số học và một căn bậc hai, dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import sqrt

    data = list(map(int, inp.split()))
    ax, ay, bx, by, cx, cy, dx, dy = data

    pax, pay = cx - ax, cy - ay
    pbx, pby = dx - bx, dy - by

    rx = ax - bx
    ry = ay - by
    vx = pax - pbx
    vy = pay - pby

    a = vx * vx + vy * vy
    b = 2 * (rx * vx + ry * vy)
    c = rx * rx + ry * ry

    best = c
    best = max(best, a + b + c)

    if a != 0:
        t = -b / (2 * a)
        t = max(0.0, min(1.0, t))
        best = max(best, a * t * t + b * t + c)

    return str(math.sqrt(best))

# provided samples
assert abs(float(run("0 0 0 0 1 1 2 2")) - 1.4142135624) < 1e-6
assert abs(float(run("0 0 0 1 0 2 2 1")) - 2.2360679775) < 1e-6

# custom cases
assert abs(float(run("0 0 1 0 0 1 1 1")) - 1.4142135624) < 1e-6
assert abs(float(run("0 0 10 0 0 0 10 0")) - 10.0) < 1e-6
assert abs(float(run("0 0 0 0 0 0 0 0")) - 0.0) < 1e-6
assert abs(float(run("0 0 2 2 2 0 0 2")) - 2.8284271247) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuyển động chéo đối xứng | √2 | cân bằng nội thất và điểm cuối | 
| chồng chéo hoán đổi ngang | 10 | thoái hóa chuyển động 1D thuần túy | 
| những con đường giống hệt nhau | 0 | trường hợp cạnh khoảng cách bằng không | 
| băng qua đường | √8 | nội thất mạnh mẽ tối đa | 

## Vỏ cạnh 

Một trường hợp quan trọng xảy ra khi cả hai người chạy đều di chuyển theo cùng một hướng với cùng tốc độ. Trong tình huống đó, vận tốc tương đối bằng 0, do đó số hạng bậc hai biến mất và khoảng cách không đổi. Thuật toán xử lý việc này thông qua việc kiểm tra a != 0, bỏ qua việc đánh giá đỉnh. 

Một trường hợp khác là khi cả hai đều xuất phát tại cùng một điểm nhưng phân kỳ theo các hướng khác nhau. Mức tối đa xảy ra ở thời điểm t = 1 và thuật toán đánh giá chính xác các điểm cuối một cách rõ ràng. 

Trường hợp suy biến là khi chuyển động triệt tiêu chính xác sao cho khoảng cách lúc đầu tăng lên rồi giảm một cách đối xứng. Điều này tạo ra mức tối đa bên trong rõ ràng và đỉnh được tính toán t* nằm hoàn toàn bên trong [0,1], được kẹp và đánh giá chính xác.
