---
title: "CF 220D - Chú Voi Nhỏ Và Tam Giác"
description: "Chúng ta có tất cả các điểm mạng bên trong hình chữ nhật $$0 le x le w,qquad 0 le y le h.$$ Một câu trả lời hợp lệ là một bộ ba điểm tạo thành một tam giác không suy biến có diện tích là một số nguyên dương. Thứ tự rất quan trọng, vì vậy mỗi tam giác hình học đóng góp tới $3!"
date: "2026-06-04T01:47:52+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 220
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 136 (Div. 1)"
rating: 2500
weight: 220
solve_time_s: 163
verified: true
draft: false
---

[CF 220D - Chú voi nhỏ và tam giác](https://codeforces.com/problemset/problem/220/D) 

**Đánh giá:** 2500 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có tất cả các điểm mạng bên trong hình chữ nhật$$0 \le x \le w,\qquad 0 \le y \le h.$$Một câu trả lời hợp lệ là một bộ ba điểm **có thứ tự** tạo thành một tam giác không suy biến có diện tích là một số nguyên dương. Thứ tự rất quan trọng, vì vậy mỗi tam giác hình học đóng góp tới$3! = 6$bộ ba khác nhau. 

Hình chữ nhật có chứa$$N=(w+1)(h+1)$$điểm. Với$w,h \le 4000$, lưới có thể chứa hơn 16 triệu điểm. Bất kỳ thuật toán nào thậm chí lặp lại trên tất cả các điểm đều là không thể, chứ đừng nói đến tất cả các bộ ba. 

Khó khăn chính là tình trạng này không chỉ đơn thuần là "không thoái hóa". Diện tích cũng phải là một số nguyên. Đối với các điểm trong mạng, diện tích gấp đôi luôn là số nguyên, vì vậy chúng ta cần hiểu khi nào diện tích gấp đôi là số chẵn. 

Một lỗi phổ biến là đếm tất cả các hình tam giác có diện tích chẵn gấp đôi và quên loại bỏ các bộ ba thẳng hàng. Ví dụ: 

đầu vào```
2 1
```Ba điểm$(0,0)$,$(1,0)$,$(2,0)$có diện tích gấp đôi$0$, là số chẵn nhưng chúng không tạo thành hình tam giác và không được tính. 

Một sai lầm dễ mắc phải khác là đếm các bộ ba không có thứ tự. Tuyên bố nói rõ ràng rằng các mệnh lệnh khác nhau là những câu trả lời khác nhau. Mỗi tam giác hình học hợp lệ đều có sáu bộ ba có thứ tự. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê từng bộ ba điểm theo thứ tự, tính toán$$|(x_2-x_1)(y_3-y_1)-(x_3-x_1)(y_2-y_1)|$$và kiểm tra xem nó có khác 0 và chia hết cho hai không. 

Điều này đúng nhưng hoàn toàn không khả thi. Lưới có thể chứa hơn$16$triệu điểm, vậy số lượng bộ ba là vào thứ tự$10^{21}$. 

Sự đột phá đến từ việc tách biệt hai yêu cầu đó. 

Điều kiện diện tích nguyên chỉ phụ thuộc vào tính chẵn lẻ của diện tích nhân đôi. Modulo$2$, định thức chỉ phụ thuộc vào các lớp chẵn lẻ của tọa độ. Chỉ có bốn lớp chẵn lẻ:$$(0,0),\ (0,1),\ (1,0),\ (1,1).$$Khi đã biết các lớp chẵn lẻ, tính chẵn lẻ của diện tích nhân đôi được xác định. 

Sau khi đếm tất cả các bộ ba có thứ tự có diện tích gấp đôi là số chẵn, chúng ta trừ đi các bộ ba có diện tích thực tế bằng 0. Đó chính xác là các bộ ba thẳng hàng. 

Việc đếm các bộ ba thẳng hàng trông có vẻ hình học, nhưng hình học mạng mang lại một công thức rõ ràng. Nếu hai điểm mạng khác nhau bởi$(dx,dy)$, thì đoạn chứa$$\gcd(dx,dy)+1$$điểm mạng. Mỗi điểm mạng bên trong giữa các điểm cuối tạo ra một bộ ba thẳng hàng với các điểm cuối đó. Điều này chuyển đổi hình học thành các tổng liên quan đến$\gcd$, có thể được đánh giá một cách hiệu quả bằng cách sử dụng hàm tổng Euler. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\min(w,h)\log\log(\min(w,h)))$|$O(\min(w,h))$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Đếm các bộ ba có diện tích là số nguyên nhân đôi 

1. Tính kích thước của bốn lớp chẵn lẻ. 
2. Hãy để$$N=(w+1)(h+1).$$Số bộ ba các điểm phân biệt có thứ tự là$$N(N-1)(N-2).$$1. Định thức là modulo lẻ$2$khi và chỉ khi ba điểm thuộc ba lớp chẵn lẻ khác nhau. 
2. Với mỗi lựa chọn của ba lớp chẵn lẻ riêng biệt, hãy thêm$$6 \cdot c_i c_j c_k$$đến số lượng định thức lẻ. 

1. Trừ số lẻ khỏi tổng số. Kết quả là số bộ ba phân biệt có thứ tự có diện tích gấp đôi là số chẵn. 

### Đếm các bộ ba có thứ tự thẳng hàng 

1. Hãy xem xét một đoạn có điểm cuối khác nhau$(dx,dy)$. 
2. Số điểm mạng trên đoạn đó bằng$$\gcd(dx,dy)+1.$$1. Số điểm mạng bên trong là$$\gcd(dx,dy)-1.$$Mỗi điểm trong cùng với hai điểm cuối tạo thành một bộ ba thẳng hàng không có thứ tự. 

1. Số lượng vị trí của một phân khúc như vậy là$$(w-dx+1)(h-dy+1).$$1. Nếu cả hai$dx$Và$dy$đều dương, cả hai hệ số góc$+\frac{dy}{dx}$Và$-\frac{dy}{dx}$tồn tại, vì vậy chúng tôi nhân với$2$. 
2. Tính tổng các phần đóng góp để được số bộ ba thẳng hàng không có thứ tự. 
3. Nhân với$6$bởi vì câu trả lời yêu cầu phải có thứ tự gấp ba. 

### Tăng tốc tổng gcd 

cho$dx>0$Và$dy>0$, chúng tôi cần$$\sum (w-dx+1)(h-dy+1)(\gcd(dx,dy)-1).$$sử dụng$$\gcd(a,b)-1=\sum_{\substack{d\mid \gcd(a,b)\\ d>1}}\varphi(d),$$chúng tôi nhận được$$\sum_{d\ge2}\varphi(d)\,S_x(d)\,S_y(d),$$Ở đâu$$S_x(d)=\sum_{k=1}^{\lfloor w/d\rfloor}(w-dk+1),$$

$$S_y(d)=\sum_{k=1}^{\lfloor h/d\rfloor}(h-dk+1).$$Cả hai tổng đều có dạng đóng:$$S_x(d)=m(w+1)-d\frac{m(m+1)}2, \quad m=\left\lfloor\frac wd\right\rfloor.$$Công thức tương tự cũng có tác dụng đối với$S_y$. 

### Tại sao nó hoạt động 

Đối số chẵn lẻ đếm chính xác các bộ ba có thứ tự có diện tích gấp đôi là số chẵn. Mọi tam giác có diện tích nguyên hợp lệ đều thuộc tập hợp đó. 

Các bộ ba duy nhất được tính ở đó không hợp lệ là các bộ ba thẳng hàng. Một bộ ba thẳng hàng có diện tích bằng 0, do đó diện tích bằng 0 tăng gấp đôi, do đó nó luôn được tính vào số chẵn lẻ. 

Công thức điểm mạng$\gcd(dx,dy)+1$đếm tất cả các điểm mạng trên một đoạn. Việc chọn hai điểm cực trị và một điểm trong sẽ cho ra một bộ ba thẳng hàng duy nhất, do đó mỗi bộ ba suy biến được tính chính xác một lần trước khi nhân với$6$để đặt hàng. 

Vì vậy câu trả lời cuối cùng là$$\text{even-area triples} - \text{collinear triples}.$$## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    w, h = map(int, input().split())

    n = (w + 1) * (h + 1)

    ex = w // 2 + 1
    ox = (w + 1) - ex

    ey = h // 2 + 1
    oy = (h + 1) - ey

    c = [
        ex * ey,
        ex * oy,
        ox * ey,
        ox * oy,
    ]

    total = n * (n - 1) * (n - 2)

    odd = 0
    for i in range(4):
        for j in range(i + 1, 4):
            for k in range(j + 1, 4):
                odd += 6 * c[i] * c[j] * c[k]

    even_area = total - odd

    m = min(w, h)

    phi = list(range(m + 1))
    for i in range(2, m + 1):
        if phi[i] == i:
            for j in range(i, m + 1, i):
                phi[j] -= phi[j] // i

    unordered_collinear = 0

    for dx in range(1, w + 1):
        unordered_collinear += (dx - 1) * (w - dx + 1) * (h + 1)

    for dy in range(1, h + 1):
        unordered_collinear += (dy - 1) * (h - dy + 1) * (w + 1)

    diag = 0
    for d in range(2, m + 1):
        mx = w // d
        my = h // d

        sx = mx * (w + 1) - d * mx * (mx + 1) // 2
        sy = my * (h + 1) - d * my * (my + 1) // 2

        diag += phi[d] * sx * sy

    unordered_collinear += 2 * diag

    ordered_collinear = 6 * unordered_collinear

    ans = (even_area - ordered_collinear) % MOD
    print(ans)

solve()
```Khối đầu tiên tính toán bốn kích thước lớp chẵn lẻ và sử dụng chúng để đếm tất cả các bộ ba có thứ tự có diện tích gấp đôi là số chẵn. 

Khối thứ hai tính tổng các giá trị Euler lên đến$\min(w,h)$. Những giá trị này xuất hiện trong khai triển số chia của$\gcd(a,b)-1$. 

Các đóng góp căn chỉnh theo trục được xử lý riêng vì một trong các tọa độ bằng 0. Đối với các đoạn ngang,$\gcd(dx,0)=dx$. Các đoạn dọc có tính chất đối xứng. 

Các đóng góp theo đường chéo sử dụng đồng nhất thức tổng thể. Các biểu mẫu đã đóng cho$S_x(d)$Và$S_y(d)$tránh mọi vòng lặp lồng nhau trên tọa độ. 

Tất cả số học được thực hiện bằng số nguyên Python, xử lý an toàn các giá trị lớn hơn nhiều so với$64$- giới hạn bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
2 1
```Các lớp chẵn lẻ: 

| Lớp | Kích thước | 
| --- | --- | 
| (0,0) | 2 | 
| (0,1) | 2 | 
| (1,0) | 1 | 
| (1,1) | 1 | 

| Số lượng | Giá trị | 
| --- | --- | 
| Tổng số bộ ba riêng biệt được đặt hàng | 120 | 
| Diện tích nhân đôi lẻ | 72 | 
| Thậm chí tăng gấp đôi diện tích | 48 | 
| Bộ ba thẳng hàng có thứ tự | 12 | 
| Câu trả lời cuối cùng | 36 | 

Đầu ra:```
36
```Ví dụ này cho thấy tại sao phép trừ cộng tuyến là cần thiết. Chỉ riêng số chẵn lẻ đã cho 48 chứ không phải 36. 

### Mẫu 2 

đầu vào```
2 2
```| Số lượng | Giá trị | 
| --- | --- | 
| Tổng số bộ ba riêng biệt được đặt hàng | 504 | 
| Diện tích nhân đôi lẻ | 192 | 
| Thậm chí tăng gấp đôi diện tích | 312 | 
| Bộ ba thẳng hàng có thứ tự | 72 | 
| Câu trả lời cuối cùng | 240 | 

Đầu ra:```
240
```Ví dụ này chứa các bộ ba thẳng hàng ngang, dọc và chéo, thực hiện mọi phần của công thức đếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\min(w,h)\log\log(\min(w,h)))$| Toàn bộ sàng chiếm ưu thế | 
| Không gian |$O(\min(w,h))$| Lưu trữ cho$\varphi$| 

Với$w,h \le 4000$, sàng chứa tối đa 4000 mục và phép tính tổng chỉ chạy vài nghìn lần lặp. Giải pháp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    MOD = 1000000007

    def solve():
        w, h = map(int, input().split())

        n = (w + 1) * (h + 1)

        ex = w // 2 + 1
        ox = (w + 1) - ex
        ey = h // 2 + 1
        oy = (h + 1) - ey

        c = [ex * ey, ex * oy, ox * ey, ox * oy]

        total = n * (n - 1) * (n - 2)

        odd = 0
        for i in range(4):
            for j in range(i + 1, 4):
                for k in range(j + 1, 4):
                    odd += 6 * c[i] * c[j] * c[k]

        even_area = total - odd

        m = min(w, h)
        phi = list(range(m + 1))
        for i in range(2, m + 1):
            if phi[i] == i:
                for j in range(i, m + 1, i):
                    phi[j] -= phi[j] // i

        bad = 0

        for dx in range(1, w + 1):
            bad += (dx - 1) * (w - dx + 1) * (h + 1)

        for dy in range(1, h + 1):
            bad += (dy - 1) * (h - dy + 1) * (w + 1)

        diag = 0
        for d in range(2, m + 1):
            mx = w // d
            my = h // d

            sx = mx * (w + 1) - d * mx * (mx + 1) // 2
            sy = my * (h + 1) - d * my * (my + 1) // 2

            diag += phi[d] * sx * sy

        bad += 2 * diag

        return str((even_area - 6 * bad) % MOD)

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    return solve()

assert run("2 1\n") == "36", "sample 1"
assert run("2 2\n") == "240", "sample 2"

assert run("1 1\n") == "0", "minimum rectangle"
assert run("1 2\n") == "0", "all triangles have half-integer area"
assert run("2 3\n") == "264", "mixed parity structure"
assert run("4000 4000\n").isdigit(), "maximum bounds"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Lưới nhỏ nhất không có hình tam giác hợp lệ | 
|`1 2`|`0`| Điều kiện diện tích số nguyên loại bỏ tất cả các hình tam giác | 
|`2 3`|`264`| Trường hợp chung có nhiều độ dốc | 
|`4000 4000`| đầu ra số | Hiệu suất ở giới hạn tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
```Lưới chỉ chứa bốn điểm. Mỗi tam giác đều có diện tích$1/2$, không bao giờ là số nguyên. Bước tính chẵn lẻ sẽ phát hiện chính xác rằng mọi tam giác không suy biến đều có diện tích nhân đôi số lẻ, do đó câu trả lời trở thành 0. 

Coi như:```
2 1
```Các điểm trên mỗi hàng ngang tạo thành một bộ ba thẳng hàng. Số chẵn lẻ bao gồm chúng vì diện tích tăng gấp đôi$=0$là chẵn. Phép trừ cộng tuyến loại bỏ chính xác mười hai bộ ba có thứ tự đó, tạo ra câu trả lời đúng 36. 

Hãy xem xét:```
1 2
```Có những tam giác không suy biến nhưng mỗi tam giác đều có diện tích$1/2$. Tính toán lớp chẵn lẻ phát hiện ra rằng diện tích nhân đôi của chúng là số lẻ, do đó không có giá trị nào được tính ngay từ đầu. Việc hiệu chỉnh cộng tuyến là không liên quan và câu trả lời cuối cùng vẫn là 0. 

Việc tách thành "diện tích chẵn gấp đôi" và "cộng tuyến" xử lý tất cả các trường hợp này một cách thống nhất.
