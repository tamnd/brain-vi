---
title: "CF 197B - Giới hạn"
description: "Chúng ta có hai đa thức, $P(x)$ và $Q(x)$, được viết theo lũy thừa giảm dần của $x$. Nhiệm vụ là tính toán: $$lim{x to infty} frac{P(x)}{Q(x)}$$ Dữ liệu đầu vào đưa ra bậc của hai đa thức và tất cả các hệ số từ số hạng bậc cao nhất cho đến số hạng không đổi."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 197
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 124 (Div. 2)"
rating: 1400
weight: 197
solve_time_s: 93
verified: true
draft: false
---

[CF 197B - Giới hạn](https://codeforces.com/problemset/problem/197/B) 

**Đánh giá:** 1400 
**Thẻ:** toán 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta được hai đa thức$P(x)$Và$Q(x)$, được viết dưới dạng lũy ​​thừa giảm dần của$x$. Nhiệm vụ là tính:$$\lim_{x \to \infty} \frac{P(x)}{Q(x)}$$Đầu vào đưa ra bậc của hai đa thức và tất cả các hệ số từ số hạng bậc cao nhất cho đến số hạng không đổi. 

Quan sát quan trọng là khi$x$trở nên cực kỳ lớn, các số hạng cấp cao nhất sẽ thống trị mọi thứ khác. Quyền hạn thấp hơn trở nên không đáng kể so với thuật ngữ hàng đầu. 

Ví dụ:$$\frac{3x^5 + 2x + 1}{7x^5 - 9}$$hành xử gần như chính xác như:$$\frac{3x^5}{7x^5} = \frac{3}{7}$$cho rất lớn$x$. 

Các ràng buộc rất nhỏ, độ cao nhất là 100, vì vậy hiệu suất không thực sự là thách thức ở đây. Ngay cả việc đánh giá các đa thức bằng số cũng sẽ vừa vặn thoải mái trong các giới hạn. Thách thức thực sự là tính đúng đắn về mặt toán học. Đánh giá dấu phẩy động sẽ đưa ra các vấn đề về độ chính xác và thậm chí có thể tạo ra các câu trả lời hoàn toàn sai cho các đầu vào lớn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
2 1
1 1 1
2 5
```Đây là:$$\frac{x^2 + x + 1}{2x + 5}$$Tử số có bậc lớn hơn nên tỉ số tăng không giới hạn. Vì cả hai hệ số dẫn đầu đều dương nên câu trả lời là:```
Infinity
```Việc triển khai bất cẩn đánh giá ở một giá trị hữu hạn lớn nào đó vẫn có thể tạo ra một số hữu hạn. 

Bây giờ hãy xem xét:```
2 1
-1 0 0
2 1
```Đây là:$$\frac{-x^2}{2x+1}$$Tử số vẫn chiếm ưu thế nhưng hệ số dẫn đầu lại âm. Giới hạn trở thành:```
-Infinity
```Dấu hiệu quan trọng bất cứ khi nào bậc tử số lớn hơn. 

Một trường hợp quan trọng khác xảy ra khi mẫu số có bậc lớn hơn:```
1 2
5 1
1 0 0
```Đây là:$$\frac{5x+1}{x^2}$$BẰNG$x \to \infty$, mẫu số tăng nhanh hơn nên kết quả tiến tới 0:```
0/1
```Cuối cùng, khi cả hai độ bằng nhau, chúng ta phải giảm phân số một cách chính xác:```
1 1
6 0
8 1
```Giới hạn là:$$\frac{6x}{8x} = \frac{6}{8} = \frac{3}{4}$$In ấn`6/8`sẽ sai vì bài toán yêu cầu một phân số tối giản. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ đánh giá trực tiếp cả hai đa thức cho một giá trị rất lớn, chẳng hạn như$x = 10^9$, sau đó chia kết quả và cố gắng suy ra giới hạn. 

Điều này có tác dụng trong nhiều trường hợp thông thường vì các thuật ngữ chiếm ưu thế lấn át các thuật ngữ nhỏ hơn. Đối với mức độ bằng nhau, tỷ lệ tiến gần đến tỷ lệ của các hệ số dẫn đầu. Đối với các mức độ khác nhau, cường độ trở nên cực kỳ lớn hoặc cực kỳ nhỏ. 

Vấn đề là độ tin cậy. Các lũy thừa lớn tràn nhanh chóng trong nhiều ngôn ngữ và số học dấu phẩy động mất đi độ chính xác. Ngay cả các số nguyên có kích thước tùy ý của Python cũng sẽ tạo ra các giá trị lớn về mặt thiên văn một cách không cần thiết. Quan trọng hơn, phép tính gần đúng bằng số về cơ bản là công cụ sai ở đây vì câu trả lời là chính xác và hoàn toàn được xác định bởi bậc đa thức và hệ số dẫn đầu. 

Cấu trúc tăng trưởng đa thức cho một giải pháp rõ ràng hơn nhiều. 

Giả định:$$P(x)=a_0x^n+\dots$$Và$$Q(x)=b_0x^m+\dots$$BẰNG$x \to \infty$:$$\frac{P(x)}{Q(x)} \sim \frac{a_0x^n}{b_0x^m} = \frac{a_0}{b_0}x^{n-m}$$Bây giờ mọi thứ chỉ phụ thuộc vào$n-m$. 

Nếu như$n>m$, hệ số$x^{n-m}$phát triển lớn vô hạn. Dấu hiệu đến từ$\frac{a_0}{b_0}$. 

Nếu như$n<m$, hệ số có xu hướng bằng không. 

Nếu như$n=m$, các lũy thừa bị triệt tiêu hoàn toàn và chỉ giữ lại tỉ số các hệ số dẫn đầu. 

Điều này làm giảm toàn bộ vấn đề xuống còn một vài so sánh và một phép tính gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đánh giá đa thức Brute Force | O(n + m) | O(1) | Không an toàn về số lượng | 
| Quan sát toán học tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ$n$Và$m$. 
2. Đọc mảng hệ số của cả hai đa thức. 
3. Trích xuất các hệ số dẫn đầu. 

Vì các hệ số được cho từ bậc cao nhất đến bậc thấp nhất nên các hệ số cao nhất chỉ đơn giản là:$$a_0 = A[0], \quad b_0 = B[0]$$4. So sánh độ$n$Và$m$. 
5. Nếu$n > m$, tử số tăng nhanh hơn mẫu số. 

Giới hạn trở thành vô cùng dương hoặc âm tùy thuộc vào dấu của:$$a_0 \times b_0$$Nếu sản phẩm dương tính, hãy in`"Infinity"`. 

Nếu không thì in`"-Infinity"`. 
6. Nếu$n < m$, mẫu số tăng nhanh hơn. 

Giới hạn tiến tới 0, vì vậy hãy in:```
0/1
```7. Nếu$n = m$, tính phần rút gọn:$$\frac{a_0}{b_0}$$Tính toán:$$g = \gcd(|a_0|, |b_0|)$$Chia cả hai số cho$g$. 
8. Đảm bảo mẫu số dương. 

Nếu mẫu số âm thì nhân cả tử số và mẫu số với$-1$. 
9. In phân số có dạng:```
p/q
```### Tại sao nó hoạt động 

Đối với rất lớn$x$, các số hạng đa thức bậc thấp trở nên không đáng kể so với số hạng dẫn đầu. Thương số:$$\frac{P(x)}{Q(x)}$$có hành vi tiệm cận tương tự như:$$\frac{a_0x^n}{b_0x^m} = \frac{a_0}{b_0}x^{n-m}$$Số mũ$n-m$xác định hoàn toàn liệu biểu thức có tăng vô hạn, co lại về 0 hay ổn định ở một giá trị không đổi. Vì mỗi bước của thuật toán đều diễn ra trực tiếp từ dạng tiệm cận này nên câu trả lời được đưa ra là chính xác về mặt toán học. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

n, m = map(int, input().split())

A = list(map(int, input().split()))
B = list(map(int, input().split()))

a0 = A[0]
b0 = B[0]

if n > m:
    if a0 * b0 > 0:
        print("Infinity")
    else:
        print("-Infinity")

elif n < m:
    print("0/1")

else:
    g = gcd(abs(a0), abs(b0))

    p = a0 // g
    q = b0 // g

    if q < 0:
        p *= -1
        q *= -1

    print(f"{p}/{q}")
```Phần đầu tiên đọc các mảng độ và hệ số. Vì thứ tự đầu vào đã bắt đầu với hệ số bậc cao nhất nên các hệ số dẫn đầu sẽ có ngay tại chỉ mục`0`. 

Việc so sánh mức độ thúc đẩy toàn bộ giải pháp. Khi bậc tử số lớn hơn, chỉ có dấu quan trọng. Nhân các hệ số dẫn đầu sẽ có dấu của giới hạn. 

Nhánh cấp bằng là nơi duy nhất yêu cầu số học ngoài phép so sánh. Chúng tôi giảm phân số bằng cách sử dụng`gcd`, sau đó chuẩn hóa dấu sao cho mẫu số vẫn dương, đúng theo yêu cầu của câu lệnh. 

Một điểm tinh tế là sử dụng các giá trị tuyệt đối bên trong`gcd`. của Python`math.gcd`xử lý các phủ định một cách an toàn, nhưng việc lấy các giá trị tuyệt đối sẽ giữ cho mục đích rõ ràng và tránh các vấn đề về khả năng di chuyển giữa các ngôn ngữ. 

Một chi tiết tinh tế khác là chuẩn hóa mẫu số. Một kết quả như`1/-2`đúng về mặt toán học nhưng vi phạm định dạng đầu ra bắt buộc vì mẫu số phải dương. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
1 1 1
2 5
```Điều này thể hiện:$$\frac{x^2+x+1}{2x+5}$$| Biến | Giá trị | 
| --- | --- | 
| n | 2 | 
| m | 1 | 
| a0 | 1 | 
| b0 | 2 | 
| So sánh bằng cấp | n > m | 
| Dấu a0 × b0 | Tích cực | 

Đầu ra:```
Infinity
```Tử số tăng dần như$x^2$, trong khi mẫu số tăng lên như thế nào$x$. Từ$x^2$thống trị$x$, tỷ lệ này tăng lên không giới hạn. 

### Ví dụ 2 

đầu vào:```
1 1
6 0
8 1
```Điều này thể hiện:$$\frac{6x}{8x+1}$$| Biến | Giá trị | 
| --- | --- | 
| n | 1 | 
| m | 1 | 
| a0 | 6 | 
| b0 | 8 | 
| gcd(6, 8) | 2 | 
| Tử số rút gọn | 3 | 
| Mẫu số rút gọn | 4 | 

Đầu ra:```
3/4
```Cả hai đa thức đều tăng với tốc độ như nhau vì bậc của chúng bằng nhau. Giới hạn bằng tỉ số của các hệ số dẫn đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Việc đọc các mảng đầu vào chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) | Chỉ một số biến ngoài mảng đầu vào được sử dụng | 

Các hạn chế là cực kỳ nhỏ nên giải pháp này sẽ chạy ngay lập tức. Ngay cả với mức độ tối đa là 100, công việc cũng chỉ là tầm thường so với thời hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    a0 = A[0]
    b0 = B[0]

    if n > m:
        if a0 * b0 > 0:
            print("Infinity")
        else:
            print("-Infinity")

    elif n < m:
        print("0/1")

    else:
        g = gcd(abs(a0), abs(b0))

        p = a0 // g
        q = b0 // g

        if q < 0:
            p *= -1
            q *= -1

        print(f"{p}/{q}")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run(
"""2 1
1 1 1
2 5
"""
) == "Infinity", "sample 1"

# equal degrees, reducible fraction
assert run(
"""1 1
6 0
8 1
"""
) == "3/4", "fraction reduction"

# denominator degree larger
assert run(
"""1 2
5 1
1 0 0
"""
) == "0/1", "approaches zero"

# negative infinity
assert run(
"""2 1
-1 0 0
2 1
"""
) == "-Infinity", "negative sign handling"

# minimum size polynomials
assert run(
"""0 0
5
-10
"""
) == "-1/2", "constant polynomials"

# denominator normalization
assert run(
"""1 1
1 0
-2 3
"""
) == "-1/2", "positive denominator requirement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bậc bằng nhau có hệ số rút gọn | 3/4 | Giảm gcd đúng cách | 
| Tử số độ nhỏ hơn | 0/1 | Xử lý số 0 đúng | 
| Dấu hiệu dẫn âm | -Vô cực | Tính đúng dấu vô cực | 
| Đa thức bậc 0 | -1/2 | Đầu vào hợp lệ nhỏ nhất | 
| Mẫu số âm | -1/2 | Chuẩn hóa mẫu số | 

## Vỏ cạnh 

Hãy xem xét:```
2 1
-1 0 0
2 1
```Các điều khoản hàng đầu là:$$\frac{-x^2}{2x} = -\frac{x}{2}$$Thuật toán so sánh độ và tìm thấy$2 > 1$. Sau đó nó kiểm tra dấu hiệu của:$$(-1) \times 2 = -2$$Vì dấu là âm nên nó in:```
-Infinity
```Điều này nắm bắt chính xác hướng phân kỳ. 

Bây giờ hãy xem xét:```
1 2
5 1
1 0 0
```Hành vi chủ đạo là:$$\frac{5x}{x^2} = \frac{5}{x}$$BẰNG$x$tăng lên thì giá trị giảm dần về 0. 

Thuật toán nhìn thấy$1 < 2$và ngay lập tức in:```
0/1
```Không cần giảm phân số vì giới hạn chính xác bằng 0. 

Cuối cùng, hãy xem xét chuẩn hóa ký hiệu mẫu số:```
1 1
1 0
-2 3
```Tỷ lệ hệ số dẫn đầu thô là:$$\frac{1}{-2}$$gcd là$1$, do đó phần không rút gọn vẫn còn`1/-2`. 

Thuật toán phát hiện mẫu số âm và lật cả hai dấu:$$\frac{1}{-2} = \frac{-1}{2}$$Nó in:```
-1/2
```đáp ứng định dạng đầu ra được yêu cầu.
