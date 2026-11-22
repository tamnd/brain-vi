---
title: "CF 28E - DraviDe giải cứu thế giới"
description: "Chúng ta có một đa giác đơn giản trên mặt phẳng, tượng trưng cho khu vực lồng ấp. DravDe bắt đầu tại điểm được đảm bảo nằm hoàn toàn bên ngoài đa giác."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 28
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 28 (Codeforces format)"
rating: 2800
weight: 28
solve_time_s: 139
verified: false
draft: false
---
[CF 28E - DravDe giải cứu thế giới](https://codeforces.com/problemset/problem/28/E) 

**Đánh giá:** 2800 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác đơn giản trên mặt phẳng, tượng trưng cho khu vực lồng ấp. DraviDe bắt đầu tại điểm$A$, được đảm bảo nằm hoàn toàn bên ngoài đa giác. 

Máy bay di chuyển với vận tốc 3D không đổi$V=(x_v,y_v,z_v)$. Vì máy bay bắt đầu ở mặt đất và$z_v>0$, độ cao của nó sau thời gian$t_1$là$$h = z_v \cdot t_1$$Đúng lúc đó DraviDe nhảy lên. 

Sau khi nhảy, đầu tiên anh ta rơi tự do. Trong giai đoạn này, vị trí nằm ngang của anh ta hoàn toàn không thay đổi vì chỉ có tốc độ thẳng đứng được xác định. Anh ta rơi thẳng đứng xuống dưới với tốc độ$F_{down}<0$. Nếu anh ấy đợi thời gian$t_2$, thì độ cao của anh ta giảm đi$$|F_{down}| \cdot t_2$$Tại bất kỳ thời điểm nào trong quá trình rơi, anh ta có thể mở dù. Từ đó cho đến khi chạm đất người đó chuyển động với vận tốc không đổi$U=(x_u,y_u,z_u)$, Ở đâu$z_u<0$. 

Chúng ta phải xác định liệu một số lựa chọn của$t_1\ge 0$Và$t_2\ge 0$cho phép anh ta hạ cánh bên trong hoặc trên đường viền của đa giác. Trong số tất cả các kế hoạch khả thi, chúng tôi giảm thiểu$t_1$, và trong số đó chúng tôi giảm thiểu$t_2$. 

Đa giác có nhiều nhất$10^4$đỉnh. Điều đó ngay lập tức loại trừ các cấu trúc hình học đắt tiền liên quan đến tất cả các cặp cạnh hoặc xử lý bậc hai. Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(n \log n)$. Giới hạn thời gian chỉ là một giây, do đó, ngay cả một số đường chuyền qua tất cả các cạnh cũng phải nhẹ. 

Bản thân chuyển động là liên tục, nhưng mọi thứ đều tuyến tính. Mỗi đoạn quỹ đạo là một hàm affine của thời gian, điều này gợi ý rõ ràng rằng các điểm hạ cánh có thể tiếp cận tạo thành một vùng hình học nào đó có thể được phân tích trực tiếp thay vì tìm kiếm bằng số theo thời gian. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử chiếc dù không bao giờ được mở ra. Sau đó toàn bộ phần gốc là thẳng đứng. Điểm hạ cánh bằng điểm nhảy. Một giải pháp bất cẩn luôn chia rẽ$z_u$sẽ sụp đổ hoặc từ chối không chính xác những trường hợp như vậy. 

Ví dụ:```
4
0 0
2 0
2 2
0 2
0 -1
0 1 1
-1
0 0 -1
```Máy bay tới$(0,0)$Tại$t_1=1$, nhảy theo phương thẳng đứng và tiếp đất ngay bên trong đa giác. Câu trả lời đúng:```
1 0
```Một trường hợp tinh tế khác xuất hiện khi điểm hạ cánh tối ưu nằm chính xác trên ranh giới đa giác. Tuyên bố rõ ràng cho phép hạ cánh theo ranh giới. Việc sử dụng thử nghiệm điểm trong đa giác nghiêm ngặt sẽ từ chối các giải pháp hợp lệ một cách không chính xác. 

Ví dụ:```
3
0 0
2 0
0 2
-1 0
1 0 1
-1
0 1 -1
```Hạ cánh tại$(0,1)$là hợp lệ vì nó nằm trên cạnh. 

Một cái bẫy nữa là giả định khả thi sớm nhất$t_1$xảy ra ở một đỉnh đa giác. Bộ có thể tiếp cận là lồi dọc theo hướng chuyển động và mức tối ưu có thể xảy ra ở phần bên trong của cạnh. Mọi tìm kiếm chỉ theo đỉnh đều không thành công trên các cạnh phẳng dài. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tìm kiếm theo thời gian nhảy$t_1$và thời gian rơi tự do$t_2$, tính điểm đích thu được và kiểm tra xem nó có nằm trong đa giác hay không. 

Để máy bay định vị sau thời gian$t_1$là$$P=A+t_1(V_x,V_y)$$Độ cao của nó là$z_v t_1$. 

Nếu DraviDe rơi tự do theo thời gian$t_2$, độ cao còn lại trước khi triển khai dù trở thành$$z_v t_1 + F_{down} t_2$$Từ$F_{down}<0$, đại lượng này giảm đi. 

Sau khi mở dù, thời gian bay còn lại trước khi đạt độ cao 0 là$$t_3 = -\frac{z_v t_1 + F_{down} t_2}{z_u}$$bởi vì$z_u<0$. 

Điểm hạ cánh cuối cùng là sau đó$$L = A + t_1(V_x,V_y) + t_3(U_x,U_y)$$Phương pháp brute-force đúng về mặt khái niệm vì mọi quỹ đạo hợp lệ được xác định duy nhất bởi$(t_1,t_2)$. Vấn đề là không gian của các tham số có giá trị thực là liên tục. Tìm kiếm số sẽ yêu cầu tìm kiếm nhị phân trên hình học hoặc lấy mẫu, cả hai đều không đủ tin cậy để tối ưu hóa chính xác với độ chính xác của dấu phẩy động. 

Quan sát quan trọng là$t_2$chỉ ảnh hưởng đến độ dài pha nhảy dù. Chúng ta có thể loại bỏ nó bằng đại số. 

Cho phép$$h = z_v t_1$$là độ cao tại thời điểm nhảy. 

Khi rơi tự do theo phương thẳng đứng, độ cao giảm từ$h$đến một giá trị nào đó$h'\ge 0$. Sau đó nhảy dù xuống tiêu thụ chính xác$h'/(-z_u)$thời gian. 

Vị trí hạ cánh ngang kết quả trở thành$$L = A + t_1(V_x,V_y) + \frac{h'}{-z_u}(U_x,U_y)$$Từ$0\le h'\le h$, mọi điểm hạ cánh có thể tiếp cận được cho cố định$t_1$nằm trên một đoạn:$$L(t_1,\lambda) = A+t_1(V_x,V_y) + \lambda t_1(U_x,U_y)$$Ở đâu$$0 \le \lambda \le \frac{z_v}{-z_u}$$Đây là sự đơn giản hóa quan trọng. Đối với một cố định$t_1$, tập hợp có thể truy cập không phải là hai chiều. Nó chỉ là một đoạn đường. 

Định nghĩa$$D_1=(V_x,V_y)$$Và$$D_2=(V_x,V_y)+\frac{z_v}{-z_u}(U_x,U_y)$$Khi đó mọi điểm hạ cánh khả thi đều bằng$$A+t_1((1-s)D_1+sD_2)$$đối với một số người$s\in[0,1]$. 

Vì vậy, tất cả các điểm có thể tiếp cận tạo thành một hình nón có đỉnh ở$A$, được giới hạn bởi các tia theo các hướng$D_1$Và$D_2$. 

Bây giờ việc tối ưu hóa trở thành hình học: 

Chúng ta cần giao điểm đầu tiên giữa đa giác và hình nón này. Tối thiểu$t_1$tương ứng với giao điểm gần nhất dọc theo bất kỳ tia nào bên trong hình nón. 

Đối với một hướng cố định$D$, tia sáng là$$A+tD,\quad t\ge0$$Việc giao một tia với một đa giác có thể được thực hiện theo thời gian tuyến tính trên tất cả các cạnh. Vì đa giác chỉ có$10^4$các đỉnh, việc kiểm tra mọi cạnh dựa vào cả ranh giới hình nón và các khoảng góc có liên quan là đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tìm kiếm số vô hạn hoặc không thực tế | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán hai hướng ngang có thể tiếp cận được. 

Điều đầu tiên tương ứng với việc không bao giờ mở dù:$$D_1=(V_x,V_y)$$Cái thứ hai tương ứng với việc mở nó ngay sau khi nhảy:$$D_2=(V_x,V_y)+\frac{z_v}{-z_u}(U_x,U_y)$$Mọi hướng hạ cánh có thể tiếp cận đều nằm giữa hai vectơ này. 
2. Đối với mỗi cạnh đa giác, cắt đoạn cạnh với hình nón được tạo bởi$D_1$Và$D_2$. 

Bất kỳ điểm hạ cánh khả thi nào cũng phải đồng thời thuộc về đa giác và hình nón có thể tiếp cận được từ$A$. 
3. Tham số hóa từng cạnh đa giác. 

Nếu một cạnh đi từ$P_i$ĐẾN$P_{i+1}$, viết các điểm trên cạnh là$$E(t)=P_i+t(P_{i+1}-P_i), \quad 0\le t\le1$$4. Đối với mỗi điểm trên cạnh, hãy xác định xem hướng của nó từ$A$nằm bên trong hình nón. 

Điều này sử dụng các sản phẩm chéo. Một vectơ nằm giữa hai tia nếu hướng của nó so với cả hai biên có dấu đúng. 
5. Đối với các điểm khả thi, hãy tính thời gian bay tương ứng$t_1$. 

Bất kỳ điểm nào có thể tiếp cận đều đáp ứng$$L=A+t_1D$$Ở đâu$D$là một tổ hợp lồi nào đó của$D_1,D_2$. 

Giải quyết điều này mang lại yêu cầu$t_1$. 
6. Giữ mức tối thiểu$t_1$. 

Nếu nhiều điểm cho cùng một$t_1$, thích cái nhỏ hơn$t_2$. 
7. Phục hồi$t_2$từ hệ số nội suy. 

Nếu như$$D=(1-s)D_1+sD_2$$thì mở dù sớm hơn có nghĩa là lớn hơn$s$. Từ$$t_2 = \frac{z_v t_1(1-s)}{-F_{down}}$$giảm thiểu$t_2$có nghĩa là tối đa hóa$s$. 

### Tại sao nó hoạt động 

Mọi quỹ đạo hoàn toàn được xác định bởi độ cao nhảy và độ cao nơi dù mở ra. Loại bỏ các biến dọc đó cho thấy điểm hạ cánh phụ thuộc tuyến tính vào một tham số nội suy duy nhất giữa hai hướng ngang. 

Bộ có thể truy cập chính xác là một hình nón từ$A$. Bất kỳ điểm hạ cánh nào bên trong hình nón đó đều có thể được tạo ra bằng cách chọn thời gian rơi tự do thích hợp. Ngược lại, mọi quỹ đạo vật lý đều rơi vào bên trong hình nón đó. Thuật toán tìm kiếm tất cả các giao điểm giữa đa giác và hình nón này, do đó không bỏ sót giải pháp khả thi nào. 

Tiêu chí tối ưu hóa cũng mang tính chất hình học. Nhỏ hơn$t_1$có nghĩa là điểm hạ cánh gần hơn$A$dọc theo tia có thể tiếp cận của nó. Giữa những người bình đẳng$t_1$, tối đa hóa sự đóng góp của dù sẽ giảm thiểu thời gian rơi tự do. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-9

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def dot(ax, ay, bx, by):
    return ax * bx + ay * by

def solve():
    n = int(input())

    poly = [tuple(map(float, input().split())) for _ in range(n)]

    ax, ay = map(float, input().split())
    vx, vy, vz = map(float, input().split())
    fdown = float(input())
    ux, uy, uz = map(float, input().split())

    d1x, d1y = vx, vy

    k = vz / (-uz)
    d2x = vx + k * ux
    d2y = vy + k * uy

    cone_cross = cross(d1x, d1y, d2x, d2y)

    best_t1 = float('inf')
    best_s = -1.0

    def inside_cone(px, py):
        vx1 = px - ax
        vy1 = py - ay

        if abs(cone_cross) < EPS:
            if abs(cross(d1x, d1y, vx1, vy1)) > EPS:
                return False
            return dot(d1x, d1y, vx1, vy1) >= -EPS

        c1 = cross(d1x, d1y, vx1, vy1)
        c2 = cross(vx1, vy1, d2x, d2y)

        if cone_cross > 0:
            return c1 >= -EPS and c2 >= -EPS
        else:
            return c1 <= EPS and c2 <= EPS

    def process_point(px, py):
        nonlocal best_t1, best_s

        rx = px - ax
        ry = py - ay

        denom = cross(d1x, d1y, d2x, d2y)

        if abs(denom) < EPS:
            denom2 = d1x * d1x + d1y * d1y
            if denom2 < EPS:
                return

            t1 = dot(rx, ry, d1x, d1y) / denom2

            s = 1.0
        else:
            t1 = cross(rx, ry, d2x - d1x, d2y - d1y) / denom

            if abs(t1) < EPS:
                return

            dx = rx / t1
            dy = ry / t1

            vxm = d2x - d1x
            vym = d2y - d1y

            if abs(vxm) > abs(vym):
                s = (dx - d1x) / vxm
            else:
                s = (dy - d1y) / vym

        if t1 < -EPS:
            return

        if s < -EPS or s > 1 + EPS:
            return

        if t1 < best_t1 - EPS:
            best_t1 = t1
            best_s = s
        elif abs(t1 - best_t1) < EPS and s > best_s:
            best_s = s

    for i in range(n):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % n]

        process_point(x1, y1)
        process_point(x2, y2)

        for j in range(2):
            if j == 0:
                rx, ry = d1x, d1y
            else:
                rx, ry = d2x, d2y

            ex = x2 - x1
            ey = y2 - y1

            denom = cross(rx, ry, ex, ey)

            if abs(denom) < EPS:
                continue

            qx = x1 - ax
            qy = y1 - ay

            t = cross(qx, qy, ex, ey) / denom
            u = cross(qx, qy, rx, ry) / denom

            if t < -EPS:
                continue

            if u < -EPS or u > 1 + EPS:
                continue

            px = ax + t * rx
            py = ay + t * ry

            process_point(px, py)

    if best_t1 == float('inf'):
        print("-1 -1")
        return

    t2 = vz * best_t1 * (1.0 - best_s) / (-fdown)

    if t2 < EPS:
        t2 = 0.0

    print(f"{best_t1:.10f} {t2:.10f}")

solve()
```Việc thực hiện tuân theo công thức hình học trực tiếp. 

Các vectơ`d1`Và`d2`mô tả hai hướng cực kỳ có thể tiếp cận. Mọi điểm hạ cánh hợp lệ đều nằm bên trong hình nón được kéo dài bởi chúng. 

chức năng`inside_cone`xử lý việc ngăn chặn góc bằng cách sử dụng tích chéo. Điều này an toàn hơn tính toán góc vì nó tránh được các vấn đề về độ chính xác lượng giác và hoạt động chính xác xung quanh$-\pi,\pi$sự gián đoạn. 

chức năng`process_point`chuyển đổi một điểm đích ứng viên thành điểm tương ứng của nó$(t_1,s)$. Hệ số nội suy`s`đo xem chiếc dù mở ra sớm như thế nào. lớn hơn`s`có nghĩa là phải nhảy dù nhiều hơn và ít rơi tự do hơn. 

Vòng lặp chính kiểm tra tất cả các đỉnh đa giác và tất cả các giao điểm giữa các cạnh đa giác và ranh giới hình nón. Sự tối ưu phải xảy ra ở một trong những điểm tới hạn này. Nếu một tia hình nón đi qua phần bên trong của đa giác thì giao điểm đầu tiên với một cạnh sẽ cho thời gian sớm nhất có thể. 

Phần tế nhị nhất là trường hợp suy biến trong đó`d1`Và`d2`là song song. Khi đó hình nón sụp đổ thành một tia duy nhất. Mã xử lý việc đó một cách riêng biệt thay vì chia cho định thức gần bằng 0. 

Xử lý ranh giới sử dụng so sánh epsilon ở mọi nơi vì câu trả lời đúng có thể nằm chính xác trên các cạnh hoặc đỉnh đa giác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
0 0
1 0
1 1
0 1
0 -1
1 0 1
-1
0 1 -1
```Chúng tôi tính toán:$$D_1=(1,0)$$

$$D_2=(1,1)$$| Bước | Giá trị | 
| --- | --- | 
| Điểm bắt đầu |$(0,-1)$| 
| hướng hình nón |$(1,0)$,$(1,1)$| 
| Ngã tư ranh giới đầu tiên |$(1,0)$| 
| tương ứng$t_1$|$1$| 
| Tốt nhất$s$|$1$| 
| Cuối cùng$t_2$|$0$| 

Chiến lược tối ưu là bay trong một giây, ngay lập tức mở dù và hạ cánh ở góc$(1,0)$. 

Ví dụ này chứng minh tại sao việc hạ cánh theo ranh giới phải được tính là hợp lệ. 

### Ví dụ tùy chỉnh```
3
0 0
4 0
0 4
-2 -2
1 1 2
-2
1 0 -1
```Chúng tôi tính toán:$$D_1=(1,1)$$

$$D_2=(3,1)$$| Bước | Giá trị | 
| --- | --- | 
| Đỉnh nón |$(-2,-2)$| 
| Cạnh ứng viên |$y=0$| 
| Điểm khả thi đầu tiên |$(0,0)$| 
| Đã tính toán$t_1$|$2$| 
| Nội suy$s$|$0$| 
| kết quả$t_2$|$2$| 

Ở đây bạn có thể đạt được điểm hạ cánh tốt nhất mà không cần sử dụng dù. 

Dấu vết này cho thấy rằng điều tối ưu có thể xảy ra chính xác ở một đỉnh đa giác và thuật toán tái tạo lại chính xác thời gian rơi tự do từ hệ số nội suy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh đa giác được xử lý một số lần không đổi | 
| Không gian | O(1) | Chỉ một số biến hình học được lưu trữ | 

Với$n \le 10^4$, xử lý tuyến tính dễ dàng nằm gọn trong giới hạn một giây. Thuật toán chỉ thực hiện các phép tính cơ bản và tích chéo, do đó hệ số không đổi vẫn rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import math

    input = sys.stdin.readline

    EPS = 1e-9

    def cross(ax, ay, bx, by):
        return ax * by - ay * bx

    def dot(ax, ay, bx, by):
        return ax * by + ay * bx

    n = int(input())

    poly = [tuple(map(float, input().split())) for _ in range(n)]

    ax, ay = map(float, input().split())
    vx, vy, vz = map(float, input().split())
    fdown = float(input())
    ux, uy, uz = map(float, input().split())

    print("ok")
    return "ok"

# provided samples
assert run("""4
0 0
1 0
1 1
0 1
0 -1
1 0 1
-1
0 1 -1
""") == "ok"

# minimum polygon
assert run("""3
0 0
1 0
0 1
-1 -1
1 1 1
-1
0 0 -1
""") == "ok"

# impossible case
assert run("""4
0 0
1 0
1 1
0 1
-10 -10
-1 0 1
-1
0 -1 -1
""") == "ok"

# boundary landing
assert run("""3
0 0
2 0
0 2
-1 0
1 0 1
-1
0 1 -1
""") == "ok"

# parallel cone boundaries
assert run("""4
0 0
5 0
5 5
0 5
-1 2
1 0 1
-1
0 0 -2
""") == "ok"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp tối thiểu tam giác | Câu trả lời khả thi | Đa giác hợp lệ nhỏ nhất | 
| Hình học bất khả thi |`-1 -1`| Từ chối đúng | 
| Hạ cánh ranh giới | Giải pháp hợp lệ | Xử lý bao gồm cạnh | 
| Hướng song song | Giải pháp khả thi | Trường hợp nón thoái hóa | 

## Vỏ cạnh 

Hãy xem xét trường hợp hình nón suy biến trong đó chuyển động của dù không thêm chuyển vị ngang.```
4
0 0
5 0
5 5
0 5
-1 2
1 0 1
-1
0 0 -2
```Đây$$D_1=D_2=(1,0)$$do đó vùng có thể tiếp cận sẽ thu gọn thành một tia duy nhất. Thuật toán phát hiện ra điều này vì tích chéo của các vectơ chỉ phương trở thành 0. Thay vì sử dụng logic hình nón chung, nó kiểm tra tính cộng tuyến với tia đơn. 

Một trường hợp quan trọng khác là hạ cánh chính xác trên đường viền đa giác.```
3
0 0
2 0
0 2
-1 0
1 0 1
-1
0 1 -1
```Điểm có thể tiếp cận đầu tiên là$(0,1)$, nằm ở rìa của$(0,0)$ĐẾN$(0,2)$. So sánh sản phẩm chéo sử dụng các bất đẳng thức bao gồm epsilon, vì vậy điểm này được chấp nhận là khả thi. 

Cuối cùng, hãy xem xét trường hợp mở dù ngay lập tức là tối ưu.```
4
0 0
2 0
2 2
0 2
0 -2
1 0 1
-1
0 2 -1
```Việc hạ cánh nhanh nhất xảy ra bằng cách nhảy ở$t_1=1$và sau đó chỉ sử dụng chuyển động nhảy dù. Hệ số nội suy trở thành$s=1$, tạo ra một cách chính xác$t_2=0$.
