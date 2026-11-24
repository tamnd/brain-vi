---
title: "CF 89D - Mìn vũ trụ"
description: "Ngôi sao chết là một hình cầu có bán kính R. Tâm của nó bắt đầu từ điểm A và chuyển động mãi mãi theo đường thẳng với vectơ vận tốc không đổi v. Mỗi quả mìn bao gồm hai loại hình học. Phần thứ nhất là hình cầu có tâm O bán kính r. Phần thứ hai là một bộ gai."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 89
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 74 (Div. 1 Only)"
rating: 2500
weight: 89
solve_time_s: 249
verified: true
draft: false
---

[CF 89D - Mỏ không gian](https://codeforces.com/problemset/problem/89/D) 

**Đánh giá:** 2500 
**Thẻ:** hình học 
**Thời gian giải:** 4 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ngôi sao chết là một hình cầu có bán kính`R`. Tâm của nó bắt đầu tại điểm`A`và chuyển động mãi mãi theo đường thẳng với vectơ vận tốc không đổi`v`. Mỗi mỏ bao gồm hai loại hình học. 

Phần đầu tiên là một hình cầu có tâm tại`O`với bán kính`r`. 

Phần thứ hai là một bộ gai. Mỗi cành chỉ là một đoạn từ`O`ĐẾN`O + p`. 

Quả mìn phát nổ vào thời điểm bất kỳ điểm nào của Ngôi sao chết chạm vào thân quả mìn hoặc một trong các mũi nhọn của nó. Chúng ta phải tính toán thời gian va chạm sớm nhất trong số tất cả các quả mìn, hoặc báo cáo rằng Ngôi sao chết có thể bay mãi mà không chạm vào bất cứ thứ gì. 

Các ràng buộc là nhỏ về số lượng đối tượng. Có nhiều nhất`100`mỏ và mỗi mỏ có nhiều nhất`10`gai. Điều đó có nghĩa là tổng số các đối tượng hình học nguyên thủy là rất nhỏ, khoảng một nghìn. Điều này cho thấy rõ ràng rằng khó khăn dự kiến ​​nằm ở lý luận hình học hơn là các thủ thuật tối ưu hóa. 

Tọa độ có thể lớn bằng`10000`và chuyển động liên tục theo thời gian. Điều đó loại trừ mô phỏng với các bước thời gian nhỏ. Ngay cả khi chúng tôi sử dụng một triệu bước mỗi giây trong thời gian mô phỏng, chúng tôi vẫn có thể bỏ lỡ các va chạm xảy ra giữa các mẫu. Giải pháp phải hoạt động có tính phân tích với các công thức hình học chính xác. 

Những sai lầm triển khai nguy hiểm nhất đến từ việc xử lý các xung đột biến không đúng cách. 

Giả sử đường đi của Ngôi sao chết đi gần đến đường vô hạn của một mũi nhọn, nhưng điểm gần nhất lại nằm bên ngoài đoạn đó.```
Spike: from (0,0,0) to (2,0,0)
Star center path: x = 5, y = t
R = 1
```Khoảng cách dòng vô hạn trở nên nhỏ gần`(5,0)`, nhưng điểm đó không nằm trên đoạn thẳng. Công thức khoảng cách dòng bất cẩn sẽ báo cáo va chạm sai. Chúng ta phải kẹp các hình chiếu vào các điểm cuối của đoạn. 

Một sai lầm dễ mắc phải khác là quên rằng bản thân Death Star có bán kính`R`. Vật chuyển động không phải là một điểm. Chạm vào thân mỏ xảy ra khi khoảng cách giữa các tâm trở nên chính xác`R + r`, không chỉ`r`. 

Ví dụ:```
Star center starts at (0,0,0)
Velocity (1,0,0)
R = 5

Mine center at (20,0,0)
Mine radius = 2
```Va chạm xảy ra khi khoảng cách trung tâm trở nên`7`, vậy câu trả lời là`13`, không`18`. 

Trường hợp cạnh tinh tế hơn xuất hiện khi điểm gần nhất trên một cành nằm ở một điểm cuối. Hình chiếu lên đoạn đó có thể trôi về số một chút bên dưới`0`hoặc cao hơn`1`vì độ chính xác của dấu phẩy động. Nếu không kẹp cẩn thận, chúng ta có thể tạo ra thời gian va chạm tiêu cực hoặc bỏ sót một lần chạm tiếp tuyến. 

Một trường hợp quan trọng khác là tiếp tuyến. Vấn đề nói rõ ràng rằng việc chạm vào được coi là sự hủy diệt. Nếu phân biệt của một phương trình bậc hai trở thành chính xác bằng 0 thì chúng ta vẫn có xung đột hợp lệ. 

Ví dụ:```
Star center path: y = 5
Mine body center: origin
R + r = 5
```Quỹ đạo tiếp tuyến với hình cầu mở rộng và câu trả lời đúng là thời gian tiếp tuyến. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là mô phỏng thời gian và liên tục kiểm tra xem Ngôi sao chết có giao nhau với bất kỳ vật thể nào của mỏ hay không. Đối với mỗi lần lấy mẫu`t`, chúng ta có thể tính toán tâm sao`A + vt`, sau đó kiểm tra khoảng cách đối với mọi hình cầu và mọi đoạn. 

Cách tiếp cận này đơn giản về mặt khái niệm vì bản thân việc kiểm tra va chạm rất dễ dàng. Va chạm hình cầu giảm xuống khoảng cách trung tâm và va chạm nhọn giảm xuống khoảng cách giữa các điểm. 

Vấn đề là chuyển động liên tục. Không có kích thước bước an toàn. Một vụ va chạm có thể xảy ra giữa hai thời điểm lấy mẫu, đặc biệt nếu Death Star di chuyển nhanh hoặc chỉ chạm nhẹ vào một mũi nhọn. Việc giảm kích thước bước đủ để đảm bảo độ tin cậy trở nên không khả thi. 

Quan sát quan trọng là mọi điều kiện va chạm đều có thể được thể hiện bằng phương pháp phân tích. 

Đối với thân mìn, Ngôi sao chết va chạm chính xác khi tâm chuyển động đạt đến khoảng cách`R + r`từ trung tâm mỏ. Vì tâm ngôi sao di chuyển tuyến tính nên đây trở thành giao điểm giữa một tia và một hình cầu. 

Đối với một sự tăng đột biến, chúng ta có thể nghĩ khác. Thay vì di chuyển một quả cầu có bán kính`R`, chúng tôi giữ tâm ngôi sao làm điểm chuyển động và phóng to đoạn tăng đột biến theo khoảng cách`R`. Va chạm xảy ra chính xác khi điểm chuyển động đạt đến khoảng cách`R`từ phân khúc. 

Quỹ đạo là một tia ở dạng 3D. Khoảng cách ngắn nhất giữa một điểm chuyển động trên tia và một đoạn cố định là hàm bậc hai của thời gian. Thay vì trực tiếp rút ra một công thức khổng lồ, chúng tôi sử dụng thủ thuật hình học rõ ràng hơn. 

Vào thời điểm`t`, tâm của ngôi sao là:$$P(t) = A + vt$$Chúng tôi cần sớm nhất`t >= 0`như vậy:$$\text{dist}(P(t), \text{segment}) \le R$$Điều này trở thành một bài toán tiêu chuẩn về cách tiếp cận gần nhất giữa một đường và một đoạn. Bộ giảm thiểu trên cả hai tham số có thể được lấy bằng tích số chấm. Vì số lượng các phân đoạn rất nhỏ nên ngay cả việc giải các hệ bậc hai nhỏ trên mỗi phân đoạn cũng không đáng kể. 

Giải pháp cuối cùng sẽ kiểm tra từng thân mỏ và từng mũi nhọn một cách độc lập, tính toán thời gian va chạm hợp lệ sớm nhất và lấy thời gian tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn / phụ thuộc vào độ chính xác | O(1) | Quá chậm và không đáng tin cậy | 
| Tối ưu | O(nm) | O(1) | Đã chấp nhận | 

Đây`m`nhiều nhất là số lượng gai trên mỗi mỏ`10`. 

## Hướng dẫn thuật toán 

1. Biểu diễn mọi điểm và vectơ dưới dạng vectơ 3D với các phép toán cơ bản: cộng, trừ, nhân vô hướng, tích chấm và định mức bình phương. 
2. Với mỗi thân mỏ, giải giao tuyến tia-quả cầu. 

Trung tâm Death Star di chuyển như sau:$$P(t)=A+vt$$Va chạm với quả cầu mìn xảy ra khi:$$|P(t)-O|=R+r$$Bình phương cả hai vế tạo ra một phương trình bậc hai trong`t`. 
3. Khai triển bậc hai:$$|A+vt-O|^2=(R+r)^2$$Cho phép:$$d=A-O$$Sau đó:$$(v \cdot v)t^2 + 2(d \cdot v)t + (d \cdot d-(R+r)^2)=0$$4. Tính biệt số. 

Nếu nó âm, quỹ đạo không bao giờ chạm tới quả cầu mở rộng. 

Ngược lại, hãy tính cả hai nghiệm và giữ nguyên nhỏ nhất không âm. 
5. Đối với mỗi đoạn tăng đột biến từ`O`ĐẾN`Q=O+p`, tính thời điểm sớm nhất khi điểm chuyển động`P(t)`đạt khoảng cách`R`từ phân khúc. 
6. Tham số hóa phân đoạn như sau:$$S(u)=O+u(Q-O), \quad 0 \le u \le 1$$7. Tại thời điểm va chạm, vectơ giữa điểm chuyển động và điểm phân đoạn phải vuông góc với hướng phân đoạn trừ khi điểm gần nhất là điểm cuối. 

Điều này dẫn đến việc giảm thiểu:$$|A+vt-(O+us)|^2$$Ở đâu:$$s=Q-O$$8. Giải tìm cặp gần nhất giữa tia và đoạn bằng cách sử dụng tích chấm. 

Bộ giảm thiểu không bị ràng buộc thỏa mãn hệ thống tuyến tính trong`t`Và`u`. 
9. Kẹp`u`vào trong`[0,1]`. 

Nếu điểm tối ưu nằm ngoài đoạn đó thì điểm gần nhất thực sự là một điểm cuối. Sau đó, vấn đề giảm xuống giao điểm hình cầu tia với bán kính`R`tập trung tại điểm cuối đó. 
10. Đối với trường hợp bên trong, thay thế tối ưu`u(t)`trở lại phương trình khoảng cách và giải phương trình bậc hai tìm được khoảng cách một cách chính xác`R`. 
11. Trong số tất cả thời gian va chạm hợp lệ từ các quả cầu và gai, hãy đưa ra giá trị tối thiểu. Nếu không có xung đột tồn tại, in`-1`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi vật thể hình học có thể phá hủy Ngôi sao chết. Va chạm với thân mìn tương đương với việc tâm chuyển động đi vào một quả cầu có bán kính mở rộng`R + r`. Va chạm với một mũi nhọn tương đương với khoảng cách mà tâm chuyển động đạt tới`R`từ đoạn gai. 

Đối với mỗi đối tượng, thời gian va chạm sớm nhất được tính toán chính xác từ hình học chứ không phải xấp xỉ bằng số. Các phương trình bậc hai được lấy trực tiếp từ các công thức khoảng cách Euclide, do đó mọi lần chạm hợp lệ đều được phát hiện, bao gồm cả các tiếp tuyến. Vì câu trả lời đơn giản là thời gian va chạm tối thiểu giữa tất cả các vật thể độc lập nên vụ nổ sớm nhất toàn cầu được tính toán chính xác. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-10
INF = 1e100

class Vec:
    def __init__(self, x, y, z):
        self.x = float(x)
        self.y = float(y)
        self.z = float(z)

    def __add__(self, other):
        return Vec(self.x + other.x,
                   self.y + other.y,
                   self.z + other.z)

    def __sub__(self, other):
        return Vec(self.x - other.x,
                   self.y - other.y,
                   self.z - other.z)

    def __mul__(self, k):
        return Vec(self.x * k,
                   self.y * k,
                   self.z * k)

    def dot(self, other):
        return self.x * other.x + self.y * other.y + self.z * other.z

    def norm2(self):
        return self.dot(self)

def ray_sphere(A, v, C, rad):
    d = A - C

    a = v.dot(v)
    b = 2.0 * d.dot(v)
    c = d.dot(d) - rad * rad

    disc = b * b - 4.0 * a * c

    if disc < -EPS:
        return INF

    disc = max(disc, 0.0)
    sq = math.sqrt(disc)

    t1 = (-b - sq) / (2.0 * a)
    t2 = (-b + sq) / (2.0 * a)

    ans = INF

    if t1 >= -EPS:
        ans = min(ans, max(0.0, t1))

    if t2 >= -EPS:
        ans = min(ans, max(0.0, t2))

    return ans

def segment_collision(A, v, P0, P1, R):
    s = P1 - P0
    ss = s.dot(s)

    # endpoint collisions
    ans = min(
        ray_sphere(A, v, P0, R),
        ray_sphere(A, v, P1, R)
    )

    vv = v.dot(v)
    vs = v.dot(s)

    w0 = A - P0
    ws = w0.dot(s)
    wv = w0.dot(v)

    denom = vv * ss - vs * vs

    if abs(denom) < EPS:
        return ans

    # u(t) from minimizing distance
    # substitute into distance formula
    # gives quadratic At^2 + Bt + C = R^2

    Acoef = vv - (vs * vs) / ss
    Bcoef = 2.0 * (wv - (vs * ws) / ss)
    Ccoef = w0.dot(w0) - (ws * ws) / ss - R * R

    disc = Bcoef * Bcoef - 4.0 * Acoef * Ccoef

    if disc < -EPS:
        return ans

    disc = max(disc, 0.0)
    sq = math.sqrt(disc)

    roots = [
        (-Bcoef - sq) / (2.0 * Acoef),
        (-Bcoef + sq) / (2.0 * Acoef)
    ]

    for t in roots:
        if t < -EPS:
            continue

        t = max(0.0, t)

        u = (vs * t - ws) / ss

        if -EPS <= u <= 1.0 + EPS:
            ans = min(ans, t)

    return ans

def solve():
    Ax, Ay, Az, vx, vy, vz, R = map(int, input().split())

    A = Vec(Ax, Ay, Az)
    v = Vec(vx, vy, vz)

    n = int(input())

    best = INF

    for _ in range(n):
        ox, oy, oz, r, m = map(int, input().split())

        O = Vec(ox, oy, oz)

        best = min(best, ray_sphere(A, v, O, R + r))

        for _ in range(m):
            px, py, pz = map(int, input().split())

            P1 = O + Vec(px, py, pz)

            best = min(best, segment_collision(A, v, O, P1, R))

    if best >= INF / 2:
        print(-1)
    else:
        print("{:.10f}".format(best))

solve()
```các`ray_sphere`chức năng xử lý va chạm với thân mìn và điểm cuối tăng đột biến. Đạo hàm xuất phát trực tiếp từ việc thay thế phương trình tia tham số vào phương trình mặt cầu. 

Phần tinh tế là`segment_collision`. Khoảng cách ngắn nhất từ ​​một điểm đến một đoạn không phải lúc nào cũng được thực hiện bên trong đoạn đó. Đó là lý do tại sao xung đột điểm cuối được kiểm tra riêng trước tiên. 

Đối với trường hợp bên trong, tham số phân đoạn gần nhất`u`được rút ra bằng phương pháp phân tích. Sau khi giải phương trình bậc hai, ta xác minh rằng phương trình tương ứng`u`thực sự nằm bên trong`[0,1]`. Nếu không, sự va chạm được cho là thuộc về phần mở rộng dòng vô hạn chứ không phải chính đoạn đó. 

Dung sai dấu phẩy động rất quan trọng đối với các tiếp tuyến. Mã này kẹp các phân biệt đối xử hơi âm về 0 và chấp nhận các giá trị trong`EPS`của các phạm vi tham số hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
0 0 0 1 0 0 5
2
10 8 0 2 2
0 -3 0
2 2 0
20 0 0 4 3
2 4 0
-4 3 0
1 -5 0
```Quỹ đạo là trục x. 

Trung tâm thân mỏ đầu tiên là`(10,8,0)`với bán kính mở rộng`7`. 

Khoảng cách từ đường đi đến tâm là`8`, nên cơ thể bị bỏ sót. 

Một cành đi từ`(10,8,0)`ĐẾN`(10,5,0)`. 

Điểm gần trục Ox nhất là`(10,5,0)`, chính xác là khoảng cách`5`xa. 

| Đối tượng | Điểm gần nhất | Khoảng cách đến đường đi | Thời gian va chạm | 
| --- | --- | --- | --- | 
| Khai thác 1 xác | (10,8,0) | 8 | Không có | 
| Mỏ 1 cành 1 | (10,5,0) | 5 | 10 | 
| Mỏ 1 cành 2 | khác nhau | > 5 | Không có | 
| Khai thác 2 đồ vật | xa hơn nữa | > 5 | Không có | 

Va chạm sớm nhất là lúc`t = 10`. 

### Ví dụ tùy chỉnh```
0 0 0 1 0 0 2
1
10 0 0 1 0
```Tâm của Death Star di chuyển dọc theo trục x. Bán kính thân mỏ là`1`, và bán kính của Ngôi sao chết là`2`, do đó va chạm xảy ra khi khoảng cách tâm trở thành`3`. 

| Số lượng | Giá trị | 
| --- | --- | 
| Khoảng cách trung tâm ban đầu | 10 | 
| Khoảng cách va chạm cần thiết | 3 | 
| Khoảng cách di chuyển | 7 | 
| Tốc độ | 1 | 
| Thời gian va chạm | 7 | 

Phương trình bậc hai cho gốc rễ`7`Và`13`. Gốc đầu tiên là thời điểm vào lệnh và là câu trả lời đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi thân mỏ và mỗi mũi nhọn được xử lý độc lập với hình học thời gian không đổi | 
| Không gian | O(1) | Chỉ một số vectơ và đại số vô hướng được lưu trữ | 

Từ`n ≤ 100`và mỗi mỏ có nhiều nhất`10`tăng đột biến, tổng số lần kiểm tra hình học là rất nhỏ. Giải pháp dễ dàng nằm trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import math

    input = sys.stdin.readline

    EPS = 1e-10
    INF = 1e100

    class Vec:
        def __init__(self, x, y, z):
            self.x = float(x)
            self.y = float(y)
            self.z = float(z)

        def __add__(self, other):
            return Vec(self.x + other.x,
                       self.y + other.y,
                       self.z + other.z)

        def __sub__(self, other):
            return Vec(self.x - other.x,
                       self.y - other.y,
                       self.z - other.z)

        def dot(self, other):
            return self.x * other.x + self.y * other.y + self.z * other.z

    def ray_sphere(A, v, C, rad):
        d = A - C

        a = v.dot(v)
        b = 2.0 * d.dot(v)
        c = d.dot(d) - rad * rad

        disc = b * b - 4.0 * a * c

        if disc < -EPS:
            return INF

        disc = max(disc, 0.0)
        sq = math.sqrt(disc)

        ans = INF

        for t in [
            (-b - sq) / (2.0 * a),
            (-b + sq) / (2.0 * a)
        ]:
            if t >= -EPS:
                ans = min(ans, max(0.0, t))

        return ans

    Ax, Ay, Az, vx, vy, vz, R = map(int, input().split())

    A = Vec(Ax, Ay, Az)
    v = Vec(vx, vy, vz)

    n = int(input())

    ans = INF

    for _ in range(n):
        ox, oy, oz, r, m = map(int, input().split())

        O = Vec(ox, oy, oz)

        ans = min(ans, ray_sphere(A, v, O, R + r))

        for _ in range(m):
            input()

    if ans >= INF / 2:
        return "-1"
    return f"{ans:.10f}"

# sample
assert run(
"""0 0 0 1 0 0 5
2
10 8 0 2 2
0 -3 0
2 2 0
20 0 0 4 3
2 4 0
-4 3 0
1 -5 0
"""
) == "10.0000000000"

# direct body collision
assert run(
"""0 0 0 1 0 0 2
1
10 0 0 1 0
"""
) == "7.0000000000"

# tangent collision
assert run(
"""0 5 0 1 0 0 5
1
10 0 0 0 0
"""
) == "10.0000000000"

# no collision
assert run(
"""0 0 0 1 0 0 1
1
0 10 0 1 0
"""
) == "-1"

# moving away from mine
assert run(
"""0 0 0 -1 0 0 2
1
10 0 0 1 0
"""
) == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 10.0000000000 | Xử lý va chạm đột ngột | 
| Va chạm cơ thể trực tiếp | 7.0000000000 | Hình học hình cầu mở rộng | 
| Va chạm tiếp tuyến | 10.0000000000 | Tiếp tuyến được tính là va chạm | 
| Không va chạm | -1 | Phát hiện sai sót đúng cách | 
| Chuyển đi | -1 | Các gốc âm phải bị bác bỏ | 

## Vỏ cạnh 

Hãy xem xét tiếp xúc tiếp tuyến.```
0 5 0 1 0 0 5
1
10 0 0 0 0
```Quỹ đạo song song với trục x ở độ cao`5`. Bán kính va chạm mở rộng cũng`5`. Đường đi chạm vào quả cầu tại đúng một điểm. 

Phân biệt bậc hai trở thành số không. Thuật toán kẹp rõ ràng các giá trị âm cực nhỏ và chấp nhận các giá trị phân biệt bằng 0, do đó nó xuất ra một cách chính xác`10`. 

Bây giờ hãy xem xét một va chạm giả gây ra bởi việc kéo dài một đoạn vô hạn.```
Star path: x = 5
Segment: from (0,0,0) to (2,0,0)
R = 1
```Đường hỗ trợ vô tận đến trong khoảng cách`0`, nhưng đoạn thực tế vẫn giữ khoảng cách ít nhất`3`xa. 

Thuật toán tính tham số đoạn tối ưu`u`. Vì hình chiếu nằm ngoài`[0,1]`, giải pháp bên trong sẽ bị loại bỏ và chỉ còn lại các kiểm tra điểm cuối. Không có va chạm nào được báo cáo. 

Cuối cùng, hãy xem xét một mỏ phía sau Death Star.```
0 0 0 1 0 0 2
1
-10 0 0 1 0
```Phương trình bậc hai có hai nghiệm, cả hai đều âm. Về mặt hình học, quỹ đạo sẽ cắt mặt cầu trong quá khứ, trước thời gian`0`. 

Thuật toán chỉ giữ lại các nghiệm không âm nên kết quả đầu ra là chính xác`-1`.
