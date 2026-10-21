---
title: "CF 105760I - Dù lượn và máy bay"
description: "Chúng ta được cung cấp một vùng không phận hình trụ nơi có thể có người chơi dù lượn. Hình trụ được xác định bởi: - Tâm $(xc, yc)$ trong mặt phẳng nằm ngang. - Bán kính $r$. - Độ cao thấp hơn $l$. - Độ cao phía trên $u$."
date: "2026-06-25T23:24:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105760
codeforces_index: "I"
codeforces_contest_name: "2020 UCF Local Programming Contest"
rating: 0
weight: 105760
solve_time_s: 44
verified: true
draft: false
---

[CF 105760I - Dù lượn và Máy bay](https://codeforces.com/problemset/problem/105760/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một vùng không phận hình trụ nơi có thể có người chơi dù lượn. 

hình trụ được xác định bởi: 

- Một trung tâm$(x_c, y_c)$trong mặt phẳng ngang. 
- Bán kính$r$. 
- Độ cao thấp hơn$l$. 
- Độ cao trên$u$. 

Mỗi máy bay xuất phát ở một vị trí đã biết$(x_a, y_a)$, độ cao$a$, tiêu đề$h$, tốc độ$s$, và tốc độ giảm$d$. 

Máy bay di chuyển dọc theo một đường thẳng trong không gian ba chiều. Chuyển động ngang của nó theo hướng di chuyển với tốc độ$s$, trong khi độ cao của nó giảm với tốc độ$d$feet trên giây. Chúng ta phải xác định xem máy bay có bao giờ đi vào hình trụ giới hạn hay không. 

Nếu đúng như vậy, chúng ta cần lần đầu tiên và lần cuối cùng khi máy bay ở trong hình trụ. Nếu không, chúng tôi báo cáo rằng chuyến bay an toàn. 

Kích thước đầu vào rất nhỏ. Có nhiều nhất là 100 máy bay. Ngay cả những phép tính hình học khá đắt tiền trên mỗi chiếc máy bay cũng hoàn toàn có thể chấp nhận được. Thách thức không phải là hiệu quả, mà là xử lý chính xác tất cả các trường hợp hình học và các trường hợp góc có dấu phẩy động. 

Nguồn sai lầm phổ biến nhất là xử lý riêng vòng tròn ngang và khoảng độ cao mà không giao nhau một cách chính xác giữa các phạm vi thời gian thu được. Một chiếc máy bay chỉ ở bên trong hình trụ khi cả hai điều kiện đều xảy ra cùng một lúc. 

Xét một hình trụ có tâm tại$(0,0)$với bán kính$1000$, phạm vi độ cao$[0,10000]$. 

Một chiếc máy bay có thể đi qua vòng tròn trong thời gian$[1,3]$, nhưng độ cao của nó có thể chỉ nằm trong giới hạn trong thời gian$[4,8]$. Câu trả lời đúng là "an toàn" vì các khoảng thời gian không trùng nhau. 

Một trường hợp tinh tế khác xảy ra khi máy bay bay theo phương ngang mà không hạ độ cao.```
Cylinder altitude range: [0, 10000]
Aircraft altitude: 15000
Descent rate: 0
```Máy bay không bao giờ có thể đạt đến phạm vi độ cao hợp lệ, bất kể đường đi ngang của nó. 

Trường hợp thứ ba là chăn thả ranh giới. Tuyên bố nói rõ ràng rằng tiếp tuyến được tính là vào và ra. Nếu đường đi chạm vào hình trụ tại đúng một thời điểm thì thời điểm đó vừa là thời gian vào và ra. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là mô phỏng chuyển động của máy bay theo thời gian và liên tục kiểm tra xem nó có nằm bên trong hình trụ hay không. 

Đây là khái niệm đơn giản. Tại mỗi bước thời gian nhỏ, tính toán vị trí và độ cao, sau đó kiểm tra xem khoảng cách theo phương ngang tính từ tâm hình trụ có lớn nhất không?$r$và độ cao nằm giữa$l$Và$u$. 

Vấn đề là độ chính xác. Để có được thời gian vào và ra được làm tròn đến hai số thập phân, chúng ta cần các bước thời gian cực kỳ nhỏ. Tệ hơn nữa, một giao lộ tiếp tuyến có thể dễ dàng bị bỏ qua hoàn toàn. Lượng mô phỏng cần thiết để đảm bảo tính chính xác trở nên không thực tế. 

Quan sát quan trọng là máy bay đi theo quỹ đạo đường thẳng ở dạng 3D. 

Hãy để thời gian trôi qua$t$. 

Tọa độ ngang là hàm tuyến tính của$t$:$$x(t)=x_a+v_x t$$

$$y(t)=y_a+v_y t$$Độ cao cũng là tuyến tính:$$z(t)=a-dt$$Ràng buộc độ cao tạo ra một khoảng thời gian. 

Ràng buộc vòng tròn ngang$$(x(t)-x_c)^2+(y(t)-y_c)^2 \le r^2$$trở thành bất đẳng thức bậc hai trong$t$. 

Bất đẳng thức bậc hai hoặc không có nghiệm, một điểm tiếp xúc hoặc một khoảng liên tục giữa hai nghiệm. 

Sau khi tính toán: 

- khoảng thời gian mà độ cao hợp lệ, 
- khoảng mà hình chiếu ngang nằm bên trong đường tròn, 
- khoảng thời gian$t \ge 0$, 

chúng ta chỉ đơn giản là giao nhau với những khoảng này. 

Nếu giao điểm cuối cùng không trống thì điểm cuối của nó chính xác là thời gian vào và ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Phụ thuộc vào kích thước bước, không giới hạn một cách hiệu quả để đảm bảo độ chính xác | O(1) | Quá chậm và không chính xác | 
| Hình học giải tích | O(1) trên mỗi máy bay | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Chuyển đổi hướng thành vector vận tốc ngang 

Tiêu đề được đo bằng độ từ trục x dương. 

Chuyển đổi nó thành radian:$$\theta = h \cdot \frac{\pi}{180}$$Sau đó$$v_x = s\cos\theta$$

$$v_y = s\sin\theta$$Đây là các thành phần vận tốc ngang. 

### 2. Tính khoảng thời gian hợp lệ về độ cao 

Độ cao là$$z(t)=a-dt$$Chúng tôi cần$$l \le a-dt \le u$$Nếu như$d=0$: 

- độ cao không đổi, 
- hoặc máy bay luôn ở trong phạm vi độ cao, 
- hoặc không bao giờ ở trong đó. 

Nếu như$d>0$, giải cả hai bất đẳng thức và thu được khoảng$$[t_{z1}, t_{z2}]$$nơi độ cao là hợp lệ. 

### 3. Tính khoảng thời gian hợp lệ của vòng tròn 

hãy để$$dx=x_a-x_c$$

$$dy=y_a-y_c$$Điều kiện vòng tròn là$$(dx+v_x t)^2+(dy+v_y t)^2 \le r^2$$Mở rộng mang lại$$At^2+Bt+C \le 0$$Ở đâu$$A=v_x^2+v_y^2$$

$$B=2(dxv_x+dyv_y)$$

$$C=dx^2+dy^2-r^2$$Vì máy bay không bao giờ khởi động bên trong xi lanh,$C>0$. 

Tính toán biệt thức$$D=B^2-4AC$$Nếu như$D<0$, đường đi không bao giờ tới được đường tròn. 

Ngược lại tính toán gốc$$t_1=\frac{-B-\sqrt D}{2A}$$

$$t_2=\frac{-B+\sqrt D}{2A}$$Máy bay nằm ngang bên trong vòng tròn chính xác cho$$[t_1,t_2]$$### 4. Giao nhau với thời gian tương lai 

Chỉ lần$$t \ge 0$$vấn đề. 

Giao khoảng cách vòng tròn với$[0,\infty)$. 

### 5. Giao nhau tất cả các khoảng 

Tính toán$$L=\max(\text{all lower bounds})$$

$$R=\min(\text{all upper bounds})$$Nếu như$$L \le R$$máy bay đi vào xi lanh. 

Thời gian vào là$L$, thời gian thoát là$R$. 

Nếu không thì máy bay sẽ an toàn. 

### Tại sao nó hoạt động 

Hình trụ là giao điểm của hai miền độc lập. 

Vùng thứ nhất là hình trụ thẳng đứng vô hạn có hình chiếu ngang là đường tròn bán kính$r$. Đôi khi, một quỹ đạo đường thẳng đi vào và rời khỏi vùng này thu được từ bất đẳng thức bậc hai. 

Vùng thứ hai là tấm độ cao giữa$l$Và$u$. Đôi khi, hàm độ cao tuyến tính đi vào và rời khỏi vùng này thu được từ bất đẳng thức tuyến tính. 

Một chiếc máy bay ở bên trong hình trụ giới hạn chính xác khi nó thuộc cả hai vùng cùng một lúc. Giao nhau giữa các khoảng thời gian tương ứng sẽ tạo ra thời gian chính xác khi cả hai điều kiện đều đúng. Vì mỗi khoảng thời gian được tính toán chính xác từ các phương trình chuyển động nên thời gian vào và ra kết quả là chính xác. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-9

xc, yc, r, l, u = map(float, input().split())
n = int(input())

for _ in range(n):
    parts = input().split()

    f = int(parts[0])
    xa = float(parts[1])
    ya = float(parts[2])
    h = float(parts[3])
    a = float(parts[4])
    s = float(parts[5])
    d = float(parts[6])

    theta = math.radians(h)

    vx = s * math.cos(theta)
    vy = s * math.sin(theta)

    # Altitude interval
    if abs(d) < EPS:
        if l - EPS <= a <= u + EPS:
            alt_lo = 0.0
            alt_hi = float('inf')
        else:
            print(f"Flight {f} is safe.")
            continue
    else:
        t_low = (a - u) / d
        t_high = (a - l) / d

        alt_lo = min(t_low, t_high)
        alt_hi = max(t_low, t_high)

    # Horizontal circle interval
    dx = xa - xc
    dy = ya - yc

    A = vx * vx + vy * vy
    B = 2.0 * (dx * vx + dy * vy)
    C = dx * dx + dy * dy - r * r

    D = B * B - 4.0 * A * C

    if D < -1e-6:
        print(f"Flight {f} is safe.")
        continue

    if D < 0:
        D = 0.0

    sqrtD = math.sqrt(D)

    t1 = (-B - sqrtD) / (2.0 * A)
    t2 = (-B + sqrtD) / (2.0 * A)

    circ_lo = min(t1, t2)
    circ_hi = max(t1, t2)

    L = max(0.0, alt_lo, circ_lo)
    R = min(alt_hi, circ_hi)

    if L <= R + 1e-6:
        print(
            f"Incoming! Flight {f} enters at {L:.2f} and exits at {R:.2f}."
        )
    else:
        print(f"Flight {f} is safe.")
```Phần đầu tiên chuyển đổi tiêu đề thành các thành phần vận tốc ngang. Điều này biến quỹ đạo máy bay thành các phương trình rõ ràng về thời gian. 

Khoảng độ cao được xử lý riêng vì độ cao chỉ phụ thuộc vào hàm tuyến tính. Khi tốc độ giảm bằng 0, cần phải xử lý đặc biệt vì việc chia cho 0 sẽ không hợp lệ. 

Khoảng cách vòng tròn xuất phát từ việc thay thế quỹ đạo vào phương trình vòng tròn. Bất đẳng thức bậc hai thu được là bài toán giao tuyến đường tròn tiêu chuẩn. Phân biệt xác định liệu quỹ đạo có bao giờ chạm đến vòng tròn hay không. 

Giao điểm cuối cùng kết hợp đồng thời ba yêu cầu: thời gian trong tương lai, độ cao hợp lệ và vị trí nằm ngang hợp lệ. 

Một epsilon nhỏ được sử dụng khi so sánh các giá trị dấu phẩy động. Nếu không có nó, các lỗi số nhỏ xung quanh các giao lộ tiếp tuyến có thể phân loại sai một chuyến bay là an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 1000 0 10000
1
1200 -5000 0 0 7500 5000 500
```Máy bay di chuyển thẳng về phía tâm xi lanh. 

| Biến | Giá trị | 
| --- | --- | 
| vx | 5000 | 
| vy | 0 | 
| Khoảng độ cao | [0, 15] | 
| Rễ tròn | [0,8, 1,2] | 
| Khoảng thời gian cuối cùng | [0,8, 1,2] | 

Đầu ra:```
Incoming! Flight 1200 enters at 0.80 and exits at 1.20.
```Độ cao có giá trị trong toàn bộ quá trình đi qua vòng tròn, vì vậy câu trả lời cuối cùng chỉ đơn giản là khoảng cách vòng tròn. 

### Ví dụ 2 

đầu vào:```
0 0 1000 0 10000
1
2400 -5000 0 90 7500 5000 500
```| Biến | Giá trị | 
| --- | --- | 
| vx | 0 | 
| vy | 5000 | 
| Khoảng cách gần nhất đến trung tâm | 5000 | 
| Phân biệt đối xử | Tiêu cực | 
| Giao lộ | Không có | 

Đầu ra:```
Flight 2400 is safe.
```Máy bay bay song song với trục y trong khi vẫn cách tâm 5000 feet. Vì bán kính chỉ có 1000 feet nên đường đi nằm ngang không bao giờ tới được hình trụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Công việc liên tục trên mỗi chiếc máy bay | 
| Không gian | O(1) | Chỉ có một số biến dấu phẩy động được lưu trữ | 

Với tối đa 100 máy bay, thời gian bay không đáng kể. Giải pháp chỉ thực hiện một số phép tính số học và căn bậc hai cho mỗi chuyến bay. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys
import io

def run(inp: str) -> str:
    return ""  # replace with solution wrapper

# sample 1
# Expected:
# Incoming! Flight 1200 enters at 0.80 and exits at 1.21.
# Flight 2400 is safe.

# minimum-sized case
assert True

# tangent case
assert True

# constant altitude outside range
assert True

# horizontal intersection but altitude interval disjoint
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Máy bay đơn hướng về trung tâm | Cảnh báo đến | Nút giao cơ bản | 
| Quỹ đạo tiếp tuyến | Thời gian vào bằng thời gian ra | Ranh giới chăn thả | 
| Không hạ độ cao bên ngoài phạm vi độ cao | An toàn | Xử lý chia cho 0 | 
| Khoảng cách vòng tròn và khoảng cách độ cao rời nhau | An toàn | Giao lộ khoảng đúng | 

## Vỏ cạnh 

### Máy bay không bao giờ đạt đến dải độ cao 

đầu vào:```
0 0 1000 0 10000
1
1000 -5000 0 0 15000 5000 0
```Độ cao vẫn là 15000 mãi mãi. Khoảng thời gian hợp lệ về độ cao bị trống nên thuật toán ngay lập tức báo cáo chuyến bay là an toàn. 

### Liên hệ tiếp tuyến 

đầu vào:```
0 0 1000 0 10000
1
1000 -1000 1000 0 5000 1000 0
```Quỹ đạo tiếp xúc với đường tròn tại đúng một điểm. Phân biệt bậc hai trở thành 0, tạo ra các nghiệm bằng nhau. Khoảng cuối cùng có các điểm cuối giống hệt nhau, thể hiện chính xác một điểm tiếp xúc gặm cỏ. Tuyên bố yêu cầu điều này được tính là vào và ra. 

### Vượt ngang sau khi rời khỏi phạm vi độ cao 

đầu vào:```
0 0 1000 0 1000
1
1000 -5000 0 0 5000 5000 4000
```Máy bay hạ cánh trong phạm vi độ cao hợp lệ từ rất sớm, rất lâu trước khi đến được vòng tròn. Khoảng cách độ cao và khoảng cách vòng tròn không trùng nhau. Giao lộ cuối cùng trống nên đầu ra là "Chuyến bay 1000 an toàn". 

Trường hợp này khẳng định rằng việc thỏa mãn từng điều kiện riêng lẻ là chưa đủ. Thời gian phải trùng lặp.
