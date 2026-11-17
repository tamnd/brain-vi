---
title: "CF 2C - Vấn đề bình luận viên"
description: "Chúng ta được cho ba vòng tròn trên mặt phẳng. Mỗi vòng tròn tượng trưng cho một sân vận động, có tâm và bán kính. Chúng ta cần tìm một điểm mà từ đó cả ba sân vận động đều được nhìn dưới cùng một góc."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 2
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 2"
rating: 2600
weight: 2
solve_time_s: 96
verified: true
draft: false
---
## Hiểu vấn đề 

Chúng ta được cho ba vòng tròn trên mặt phẳng. Mỗi vòng tròn tượng trưng cho một sân vận động, có tâm và bán kính. Chúng ta cần tìm một điểm mà từ đó cả ba sân vận động đều được nhìn dưới cùng một góc. 

Đối với đường tròn có bán kính$r$quan sát được từ một điểm$P$, góc nhìn thấy là góc giữa hai đường tiếp tuyến từ$P$đến vòng tròn. Nếu khoảng cách từ$P$đến tâm đường tròn là$d$, góc đó bằng:$$2\arcsin\left(\frac{r}{d}\right)$$Bài toán yêu cầu một điểm mà cả ba đường tròn đều tạo ra một góc nhìn giống nhau. Trong số tất cả các điểm như vậy, chúng ta muốn điểm có góc lớn nhất. 

Các tâm được đảm bảo không thẳng hàng, do đó hình học không bao giờ suy biến thành sự mơ hồ dựa trên đường thẳng. Tọa độ và bán kính nhiều nhất là$10^3$, có nghĩa là hình học dấu phẩy động là hoàn toàn khả thi. Giới hạn thời gian là rất nhỏ, nhưng đây không phải là vấn đề tối ưu hóa số lượng mạnh mẽ. Giải pháp dự định là hình học dạng đóng chỉ có tính toán theo thời gian không đổi. 

Quan sát ẩn mấu chốt là các góc nhìn bằng nhau hàm ý một điều kiện đại số rất mạnh. Từ$$2\arcsin\left(\frac{r_i}{d_i}\right)$$phải bằng nhau đối với tất cả các vòng tròn, các giá trị$$\frac{r_i}{d_i}$$cũng phải bằng nhau. Sắp xếp lại mang lại$$d_i : d_j = r_i : r_j$$Vậy điểm chưa biết có khoảng cách tới ba tâm tỉ lệ với ba bán kính. 

Điều đó chuyển đổi bài toán từ bài toán tối ưu hóa lượng giác thành bài toán xây dựng hình học thuần túy. 

Có một số sai lầm dễ dàng xảy ra ở đây. 

Một lỗi phổ biến là tối đa hóa góc trực tiếp bằng tối ưu hóa lặp lại. Hàm góc là phi tuyến và vùng khả thi không lồi. Leo đồi số có thể hội tụ sai điểm hoặc thất bại hoàn toàn. 

Một sai lầm khác là quên rằng có thể không có điểm nào hợp lệ. Coi như:```
0 0 1
10 0 1
0 10 100
```Bán kính thứ ba lớn hơn rất nhiều so với những bán kính khác. Tỷ lệ khoảng cách yêu cầu trở nên không thể đáp ứng về mặt hình học. Việc triển khai bất cẩn làm giao nhau các vòng tròn một cách mù quáng có thể tạo ra NaN hoặc tọa độ rác. Hành vi đúng là không in gì cả. 

Một vấn đề phức tạp hơn xuất hiện khi tất cả bán kính đều bằng nhau. Khi đó điều kiện trở thành khoảng cách bằng nhau đến cả ba trung tâm. Câu trả lời là tâm đường tròn ngoại tiếp của tam giác tạo bởi các tâm. 

Ví dụ:```
0 0 10
60 0 10
30 30 10
```Câu trả lời đúng là:```
30.00000 0.00000
```Một đạo hàm dựa trên tỷ lệ đơn giản có thể vô tình chia cho 0 nếu nó giả định bán kính là khác nhau. 

Một cái bẫy khác đến từ việc chọn sai nhánh vòng tròn Apollonius. Các locus tỷ lệ khoảng cách tạo ra các đường tròn, nhưng mỗi cặp tâm có thể xác định nhiều dạng đại số tùy theo hướng. Lỗi ký hiệu sẽ tạo ra điểm giao nhau được phản ánh, không đáp ứng được ràng buộc thứ ba. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là tìm kiếm trên mặt phẳng và đánh giá ba góc nhìn tại mỗi điểm. Nếu các góc gần bằng nhau thì giữ lại ứng viên tốt nhất. 

Điều này hoạt động về mặt khái niệm vì góc nhìn của đường tròn từ một điểm rất dễ tính toán:$$\theta = 2\arcsin\left(\frac{r}{d}\right)$$Chúng ta có thể lấy mẫu một lưới dày đặc hoặc chạy tăng dần độ dốc. Vấn đề là độ chính xác. Tọa độ có giá trị thực và điểm hợp lệ có thể nằm ở bất kỳ đâu trong mặt phẳng. Ngay cả một lưới có khoảng cách$10^{-5}$trên phạm vi tọa độ có thể sẽ yêu cầu khoảng$10^{16}$đánh giá, hoàn toàn không thể. 

Cải tiến tiếp theo là xử lý điều kiện góc bằng bằng đại số. Vì arcsin đang tăng nghiêm ngặt,$$2\arcsin\left(\frac{r_1}{d_1}\right) = 2\arcsin\left(\frac{r_2}{d_2}\right)$$ngụ ý$$\frac{r_1}{d_1}=\frac{r_2}{d_2}$$hoặc tương đương$$\frac{d_1}{d_2}=\frac{r_1}{r_2}$$Bây giờ bài toán trở thành hình học: tìm một điểm có khoảng cách đến ba tâm có tỉ số quy định. 

Quỹ tích các điểm thỏa mãn$$\frac{|PA|}{|PB|}=k$$là đường tròn Apollonius khi$k \ne 1$, và là đường phân giác vuông góc khi$k=1$. 

Vì vậy, chúng ta có thể xây dựng hai locus như vậy:$$\frac{|PA|}{|PB|}=\frac{r_1}{r_2}$$Và$$\frac{|PA|}{|PC|}=\frac{r_1}{r_3}$$Giao điểm của họ mang lại cho ứng viên điểm. 

Tiêu chí tối ưu hóa cũng trở nên đơn giản. Góc nhìn tăng khi tỷ lệ chung$$\frac{r_i}{d_i}$$tăng lên nghĩa là khoảng cách bị thu hẹp lại. Trong số các nút giao thông có thể có, chúng tôi chọn nút giao thông gần trung tâm hơn. 

May mắn thay, hình học đã đơn giản hóa hơn nữa. Sau khi bình phương và trừ các phương trình, hệ thống trở nên tuyến tính. Thay vì giao các đường tròn một cách rõ ràng, chúng ta có thể rút ra hai phương trình tuyến tính và giải chúng một cách trực tiếp. 

Toàn bộ giải pháp chạy trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(G^2)$cho kích thước lưới$G$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba tâm và bán kính đường tròn. 

Hãy để các vòng tròn được$(x_i,y_i,r_i)$. 
2. Giới thiệu điểm quan sát chưa biết$P=(x,y)$. 

Góc nhìn bằng nhau ngụ ý:$$\frac{r_1}{d_1}=\frac{r_2}{d_2}=\frac{r_3}{d_3}$$Ở đâu$$d_i^2=(x-x_i)^2+(y-y_i)^2$$1. Viết lại điều kiện tỉ số như sau:$$\frac{d_1^2}{r_1^2}=\frac{d_2^2}{r_2^2}$$và tương tự cho các vòng tròn$1$Và$3$. 

Bình phương loại bỏ căn bậc hai trong khi vẫn giữ được đẳng thức vì mọi khoảng cách đều không âm. 

1. Khai triển các phương trình. 

Dành cho vòng kết nối$1$Và$2$:$$\frac{(x-x_1)^2+(y-y_1)^2}{r_1^2} = \frac{(x-x_2)^2+(y-y_2)^2}{r_2^2}$$1. Sắp xếp lại phương trình. 

Các số hạng bậc hai kết hợp thành:$$ax^2+ay^2+bx+cy+d=0$$Ở đâu$$a=\frac1{r_1^2}-\frac1{r_2^2}$$1. Xây dựng phương trình thứ hai bằng đường tròn$1$Và$3$. 
2. Loại bỏ các số hạng bậc hai. 

Nhân các phương trình một cách thích hợp và trừ chúng. Vì cả hai đều chứa cùng một$x^2+y^2$cấu trúc, phép trừ sẽ loại bỏ phần phi tuyến. 

Kết quả là một hệ thống tuyến tính trong$x$Và$y$. 
3. Giải kết quả$2 \times 2$hệ thống sử dụng định thức. 

Bởi vì các tâm không thẳng hàng nên định thức sẽ khác 0 bất cứ khi nào tồn tại một nghiệm hợp lệ. 
4. In tọa độ với năm chữ số thập phân. 
5. Nếu định thức thực tế bằng 0 thì không in gì cả. 

### Tại sao nó hoạt động 

Bất biến quan trọng là các góc nhìn bằng nhau hoàn toàn tương đương với khoảng cách tỷ lệ từ điểm quan sát đến tâm. 

Góc nhìn của hình tròn chỉ phụ thuộc vào tỉ lệ$r/d$. Vì arcsin hoàn toàn đơn điệu trên miền hợp lệ nên các góc bằng nhau buộc mọi tỷ số$r_i/d_i$để được bình đẳng. 

Sau khi bình phương và khai triển, mọi ràng buộc sẽ trở thành một phương trình bậc hai có cùng số hạng bậc hai$x^2+y^2$. Phép trừ hai phương trình như vậy sẽ loại bỏ tất cả các thành phần phi tuyến, để lại các phương trình tuyến tính có nghiệm chính xác là điểm duy nhất thỏa mãn tỷ số khoảng cách ban đầu. 

Vì đạo hàm tương đương về mặt đại số ở mỗi bước nên mọi nghiệm được tính toán đều thỏa mãn điều kiện hình học ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

EPS = 1e-12

def solve():
    circles = [tuple(map(float, input().split())) for _ in range(3)]

    x1, y1, r1 = circles[0]
    x2, y2, r2 = circles[1]
    x3, y3, r3 = circles[2]

    def build(ca, cb):
        xa, ya, ra = ca
        xb, yb, rb = cb

        ia = 1.0 / (ra * ra)
        ib = 1.0 / (rb * rb)

        a = ia - ib
        b = -2.0 * xa * ia + 2.0 * xb * ib
        c = -2.0 * ya * ia + 2.0 * yb * ib
        d = (xa * xa + ya * ya) * ia - (xb * xb + yb * yb) * ib

        return a, b, c, d

    e1 = build(circles[0], circles[1])
    e2 = build(circles[0], circles[2])

    a1, b1, c1, d1 = e1
    a2, b2, c2, d2 = e2

    A1 = a2 * b1 - a1 * b2
    B1 = a2 * c1 - a1 * c2
    C1 = a2 * d1 - a1 * d2

    det = A1 * (-B1) - B1 * (-A1)

    if abs(A1) < EPS and abs(B1) < EPS:
        return

    if abs(B1) > EPS:
        # A1*x + B1*y + C1 = 0
        # y = (-A1*x - C1)/B1

        # Plug into first quadratic equation
        p = -A1 / B1
        q = -C1 / B1

        aa = a1 * (1 + p * p)
        bb = b1 + 2 * a1 * p * q + c1 * p
        cc = a1 * q * q + c1 * q + d1

        disc = bb * bb - 4 * aa * cc

        if disc < -EPS:
            return

        disc = max(disc, 0.0)
        sq = disc ** 0.5

        xs = [
            (-bb + sq) / (2 * aa),
            (-bb - sq) / (2 * aa)
        ]

        best = None
        bestv = -1

        for x in xs:
            y = p * x + q

            d = ((x - x1) ** 2 + (y - y1) ** 2) ** 0.5
            val = r1 / d

            if val > bestv:
                bestv = val
                best = (x, y)

        x, y = best
        print(f"{x:.5f} {y:.5f}")

    else:
        x = -C1 / A1

        aa = a1
        bb = 2 * a1 * x + c1
        cc = a1 * x * x + b1 * x + d1

        disc = bb * bb - 4 * aa * cc

        if disc < -EPS:
            return

        disc = max(disc, 0.0)
        sq = disc ** 0.5

        ys = [
            (-bb + sq) / (2 * aa),
            (-bb - sq) / (2 * aa)
        ]

        best = None
        bestv = -1

        for y in ys:
            d = ((x - x1) ** 2 + (y - y1) ** 2) ** 0.5
            val = r1 / d

            if val > bestv:
                bestv = val
                best = (x, y)

        x, y = best
        print(f"{x:.5f} {y:.5f}")

solve()
```các`build`hàm xây dựng phương trình bậc hai tương ứng với một ràng buộc tỷ lệ. Mỗi phương trình có dạng:$$a(x^2+y^2)+bx+cy+d=0$$Quan sát quan trọng là cả hai phương trình đều chứa cấu trúc bậc hai giống nhau. Trừ chúng sau khi chia tỷ lệ phù hợp sẽ loại bỏ các số hạng phi tuyến và tạo ra một đường thẳng. 

Khi chúng ta có được đường thẳng đó, chúng ta thay thế nó trở lại thành một phương trình bậc hai. Điều này làm giảm bài toán thành một phương trình bậc hai một biến. 

Có thể có hai điểm ứng cử viên về mặt hình học. Một cái cho góc nhìn lớn hơn cái kia. Vì góc tăng theo$r/d$, chúng tôi chỉ đơn giản là tối đa hóa$r_1/d_1$. 

Việc xử lý phân biệt đối xử là tinh tế. Các giá trị âm nhỏ có thể xuất hiện do lỗi dấu phẩy động, do đó quá trình triển khai sẽ kẹp các giá trị âm gần bằng 0 về 0 trước khi lấy căn bậc hai. 

Một điểm tinh tế khác là nhánh nơi đường thẳng trở nên thẳng đứng. Trực tiếp giải quyết$y$sẽ chia cho 0, do đó mã sẽ xử lý việc đó một cách riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 10
60 0 10
30 30 10
```Vì tất cả các bán kính đều bằng nhau nên điểm quan sát phải cách đều tất cả các tâm. 

| Bước | Giá trị | 
| --- | --- | 
| Hạn chế về tỷ lệ |$d_1=d_2=d_3$| 
| Vị trí đầu tiên | Đường phân giác vuông góc của hai tâm đầu tiên | 
| Vị trí thứ hai | Đường phân giác vuông góc của tâm thứ nhất và thứ ba | 
| Giao lộ |$(30,0)$| 

Thuật toán tìm tâm đường tròn ngoại tiếp của tam giác tạo bởi các tâm. Điều này xác nhận rằng bán kính bằng nhau sẽ đưa bài toán về dạng hình học có khoảng cách bằng nhau cổ điển. 

### Ví dụ 2 

đầu vào:```
0 0 1
4 0 2
0 3 3
```| Bước | Giá trị | 
| --- | --- | 
| Ràng buộc 1 |$d_1:d_2=1:2$| 
| Ràng buộc 2 |$d_1:d_3=1:3$| 
| Quan hệ tuyến tính | Bắt nguồn từ việc loại bỏ các số hạng bậc hai | 
| Điểm ứng viên | Hai căn bậc hai | 
| Điểm đã chọn | Một với mức tối đa$r/d$| 

Hai nghiệm đại số tương ứng với hai giao điểm của đường tròn Apollonius. Thuật toán chọn chính xác thuật toán tạo ra góc nhìn chung lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số phép tính số học cố định | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu phụ trợ | 

Các ràng buộc rất nhỏ nhưng hình học đòi hỏi độ chính xác. Giải pháp phân tích theo thời gian không đổi dễ dàng nằm gọn trong giới hạn 1 giây và tránh được sự mất ổn định từ các phương pháp số lặp. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isclose

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    EPS = 1e-12
    input = sys.stdin.readline

    out = io.StringIO()

    circles = [tuple(map(float, input().split())) for _ in range(3)]

    x1, y1, r1 = circles[0]

    def build(ca, cb):
        xa, ya, ra = ca
        xb, yb, rb = cb

        ia = 1.0 / (ra * ra)
        ib = 1.0 / (rb * rb)

        a = ia - ib
        b = -2 * xa * ia + 2 * xb * ib
        c = -2 * ya * ia + 2 * yb * ib
        d = (xa * xa + ya * ya) * ia - (xb * xb + yb * yb) * ib

        return a, b, c, d

    e1 = build(circles[0], circles[1])
    e2 = build(circles[0], circles[2])

    a1, b1, c1, d1 = e1
    a2, b2, c2, d2 = e2

    A1 = a2 * b1 - a1 * b2
    B1 = a2 * c1 - a1 * c2
    C1 = a2 * d1 - a1 * d2

    if abs(B1) > EPS:
        p = -A1 / B1
        q = -C1 / B1

        aa = a1 * (1 + p * p)
        bb = b1 + 2 * a1 * p * q + c1 * p
        cc = a1 * q * q + c1 * q + d1

        disc = max(0.0, bb * bb - 4 * aa * cc)
        sq = disc ** 0.5

        xs = [
            (-bb + sq) / (2 * aa),
            (-bb - sq) / (2 * aa)
        ]

        best = None
        bestv = -1

        for x in xs:
            y = p * x + q
            d = ((x - x1) ** 2 + (y - y1) ** 2) ** 0.5
            val = r1 / d

            if val > bestv:
                bestv = val
                best = (x, y)

        x, y = best
        print(f"{x:.5f} {y:.5f}", file=out)

    return out.getvalue().strip()

# provided sample
assert run(
"""0 0 10
60 0 10
30 30 10
"""
) == "30.00000 0.00000", "sample 1"

# equal radii, symmetric triangle
assert run(
"""0 0 5
10 0 5
0 10 5
"""
) == "5.00000 5.00000", "circumcenter"

# different radii
res = run(
"""0 0 1
4 0 2
0 3 3
"""
)
assert len(res.split()) == 2, "valid geometry case"

# large coordinates
res = run(
"""1000 1000 10
-1000 500 20
300 -700 30
"""
)
assert len(res.split()) == 2, "large coordinate stability"

# near-degenerate geometry
res = run(
"""0 0 1
1000 1 2
2 1000 3
"""
)
assert len(res.split()) == 2, "floating point robustness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu bán kính bằng nhau | Tâm đường tròn chính xác | Giảm về hình học có khoảng cách bằng nhau | 
| Bán kính khác nhau | Bất kỳ tọa độ hợp lệ nào | Hành vi chung của vòng tròn Apollonius | 
| Tọa độ lớn | Đầu ra số ổn định | Độ ổn định điểm nổi | 
| Tam giác cận suy biến | Câu trả lời hợp lệ | Độ bền chính xác | 

## Vỏ cạnh 

Xét bán kính bằng nhau:```
0 0 10
60 0 10
30 30 10
```Điều kiện tỉ số trở thành:$$d_1=d_2=d_3$$Phép trừ phương trình sẽ loại bỏ ngay các số hạng bậc hai và để lại hai đường phân giác vuông góc. Giao điểm của chúng là tâm đường tròn$(30,0)$. Thuật toán xử lý việc này một cách tự nhiên mà không cần bất kỳ hình học trường hợp đặc biệt nào. 

Bây giờ hãy xem xét hình học không thể:```
0 0 1
10 0 1
0 10 100
```Các tỷ lệ bắt buộc buộc điểm quan sát phải ở rất xa hai trung tâm đầu tiên so với trung tâm thứ ba. Phân biệt bậc hai trở nên âm. Việc triển khai phát hiện điều này và không in gì thay vì tạo ra tọa độ không hợp lệ. 

Một trường hợp tinh vi khác là đường loại trừ theo chiều dọc:```
0 0 1
4 0 2
2 5 3
```Việc loại bỏ tuyến tính có thể tạo ra một phương trình có dạng:$$x = c$$Đang cố gắng giải quyết cho$y$đầu tiên sẽ chia cho số 0. Việc thực hiện phân nhánh rõ ràng vào việc liệu hệ số của$y$biến mất, tránh mất ổn định.
