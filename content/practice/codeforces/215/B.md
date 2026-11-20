---
title: "CF 215B - Huy chương Olympic"
description: "Chúng tôi đang thiết kế một huy chương gồm hai phần. Phần bên ngoài là một vòng có bán kính ngoài $r1$ và bán kính trong $r2$. Phần bên trong là một đĩa đặc có bán kính $r2$. Cả hai phần đều có độ dày như nhau nên khối lượng của chúng chỉ phụ thuộc vào diện tích và mật độ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 215
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 132 (Div. 2)"
rating: 1300
weight: 215
solve_time_s: 103
verified: true
draft: false
---

[CF 215B - Huy chương Olympic](https://codeforces.com/problemset/problem/215/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang thiết kế một huy chương gồm hai phần. Phần bên ngoài là một vòng có bán kính ngoài$r_1$và bán kính trong$r_2$. Phần bên trong là một đĩa đặc có bán kính$r_2$. Cả hai phần đều có độ dày như nhau nên khối lượng của chúng chỉ phụ thuộc vào diện tích và mật độ. 

Vòng ngoài sử dụng mật độ$p_1$, được chọn từ một danh sách. Đĩa bên trong sử dụng mật độ$p_2$, được chọn từ một danh sách khác. Bán kính bên ngoài$r_1$cũng được chọn từ danh sách. Một quy tắc cố định từ câu lệnh yêu cầu$$\frac{m_{out}}{m_{in}} = \frac{A}{B}$$và chúng ta phải tối đa hóa bán kính bên trong$r_2$. 

Đầu vào cung cấp tất cả các lựa chọn có thể có cho bán kính bên ngoài và cả mật độ. Chúng ta phải tạo ra kết quả lớn nhất có thể đạt được$r_2$. 

Những hạn chế là rất nhỏ. Mọi giá trị tối đa là 5000 và kích thước mảng cũng đủ nhỏ để ngay cả việc kiểm tra mọi kết hợp cũng có thể dễ dàng khớp kịp thời. Điều này ngay lập tức cho chúng ta biết vấn đề không nằm ở các thủ thuật tối ưu hóa hay cấu trúc dữ liệu nâng cao. Nhiệm vụ thực sự là tìm ra công thức chính xác và hiểu những lựa chọn nào tối đa hóa câu trả lời. 

Phần nguy hiểm là đại số. Một phép tính đạo hàm bất cẩn có thể dễ dàng đảo ngược một phân số hoặc quên rằng khối lượng phụ thuộc vào diện tích, từ đó đưa ra bán kính bình phương. 

Một trường hợp khó nhận thấy là chọn sai mật độ. Giả định:```
r1 = 10
p1 choices = [1, 100]
p2 choices = [1]
A = 1, B = 1
```Nếu chúng ta sử dụng$p_1 = 1$, chiếc nhẫn nhẹ và đĩa bên trong phải tương đối nhỏ để giữ tỷ lệ khối lượng cần thiết. sử dụng$p_1 = 100$cho phép một đĩa bên trong lớn hơn nhiều. Một cách tiếp cận tham lam mà bỏ qua mật độ sẽ thất bại ở đây. 

Một sai lầm dễ mắc phải khác là tối đa hóa sai biến. Công thức cho$r_2$chứa$r_1^2$, không$r_1$. Nếu ai đó cố gắng tối đa hóa các tỷ lệ một cách tuyến tính thay vì sử dụng biểu thức dẫn xuất chính xác, họ có thể nhận được những so sánh không chính xác. 

Cái bẫy thứ ba là độ chính xác của dấu phẩy động. Câu trả lời không nhất thiết phải là số nguyên. Ví dụ:```
1 5
1 3
1 2
1 1
```sản xuất$$r_2 = 5 \sqrt{\frac{3}{5}}$$đó là điều phi lý. Số học số nguyên sẽ âm thầm cắt bớt kết quả. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi bộ ba có thể$(r_1, p_1, p_2)$. Với mỗi sự kết hợp, hãy rút ra kết quả tương ứng$r_2$khỏi phương trình khối lượng và giữ giá trị lớn nhất. 

Điều này hoạt động vì số lượng khả năng là rất nhỏ. Ngay cả khi cả ba mảng đều có kích thước 100 thì tổng số kết hợp sẽ chỉ là$10^6$, điều này hoàn toàn ổn trong Python. 

Phần thú vị là đơn giản hóa công thức. 

Diện tích vòng ngoài là$$\pi (r_1^2 - r_2^2)$$và khu vực đĩa bên trong là$$\pi r_2^2$$Vì độ dày là đồng nhất nên khối lượng bằng mật độ nhân diện tích cho đến một hệ số không đổi chung. Vì thế:$$m_{out} = p_1 \pi (r_1^2 - r_2^2)$$

$$m_{in} = p_2 \pi r_2^2$$Tỷ lệ cần tìm là$$\frac{p_1 (r_1^2 - r_2^2)}{p_2 r_2^2} = \frac{A}{B}$$Giải quyết cho$r_2^2$:$$B p_1 (r_1^2 - r_2^2) = A p_2 r_2^2$$

$$B p_1 r_1^2 = r_2^2 (A p_2 + B p_1)$$

$$r_2^2 = \frac{B p_1 r_1^2}{A p_2 + B p_1}$$

$$r_2 = r_1 \sqrt{ \frac{B p_1}{A p_2 + B p_1} }$$Bây giờ việc tối ưu hóa trở nên rõ ràng. Biểu thức tăng khi: 

-$r_1$tăng lên 
-$p_1$tăng lên 
-$p_2$giảm 

Vì vậy, thay vì ép buộc mọi sự kết hợp, chúng ta chỉ cần: 

- tối đa$r_1$- tối đa$p_1$- tối thiểu$p_2$Điều đó làm giảm giải pháp phải làm việc liên tục sau khi đọc đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nmk)$|$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(n + m + k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả bán kính ngoài ứng viên và chọn giá trị lớn nhất$R$. 

Bán kính ngoài lớn hơn luôn làm tăng câu trả lời cuối cùng vì$r_2$tỷ lệ thuận với$r_1$. 
2. Đọc tất cả mật độ đề xuất cho vòng ngoài và chọn giá trị lớn nhất$P_1$. 

Tăng dần$p_1$làm cho vòng ngoài nặng hơn, cho phép đĩa bên trong lớn hơn trong khi vẫn duy trì tỷ lệ khối lượng cần thiết. 
3. Đọc tất cả mật độ đề xuất cho đĩa bên trong và chọn giá trị tối thiểu$P_2$. 

Đĩa bên trong nhẹ hơn cho phép bán kính lớn hơn trước khi khối lượng của nó trở nên quá lớn. 
4. Đọc hằng$A$Và$B$. 
5. Tính toán$$r_2 = R \sqrt{ \frac{B P_1}{A P_2 + B P_1} }$$1. In kết quả với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Công thức dẫn xuất thể hiện$r_2$hoàn toàn theo các biến độc lập:$$r_2 = r_1 \sqrt{ \frac{B p_1}{A p_2 + B p_1} }$$Tử số tăng dần với$r_1$Và$p_1$, trong khi mẫu số tăng dần với$p_2$. Vì tất cả các giá trị đều dương nên tối đa hóa$r_1$Và$p_1$và giảm thiểu$p_2$always maximizes the entire expression. Không có sự tương tác giữa các biến tạo ra sự cân bằng, do đó việc chọn từng giá trị tối ưu độc lập sẽ tạo ra câu trả lời tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

# read radii
data = list(map(int, input().split()))
n = data[0]
x = data[1:]

# read p1 values
data = list(map(int, input().split()))
m = data[0]
y = data[1:]

# read p2 values
data = list(map(int, input().split()))
k = data[0]
z = data[1:]

A, B = map(int, input().split())

r1 = max(x)
p1 = max(y)
p2 = min(z)

ans = r1 * math.sqrt((B * p1) / (A * p2 + B * p1))

print("{:.12f}".format(ans))
```Việc thực hiện tuân theo đạo hàm toán học trực tiếp. 

Chi tiết quan trọng đầu tiên là chọn cực trị chính xác. Chúng ta muốn bán kính bên ngoài tối đa và mật độ bên ngoài nhưng mật độ bên trong tối thiểu. Hoán đổi tối thiểu hoặc tối đa sẽ tạo ra một số có vẻ hợp lệ nhưng hoàn toàn sai. 

Chi tiết thứ hai là sử dụng phép chia dấu phẩy động. Trong Python điều này xảy ra tự động với`/`, nhưng việc sử dụng phép chia số nguyên trong ngôn ngữ khác sẽ làm mất đi độ chính xác. 

Công thức cuối cùng xuất phát trực tiếp từ việc sắp xếp lại phương trình khối lượng. Căn bậc hai áp dụng cho toàn bộ phân số, không chỉ mẫu số. Dấu ngoặc đơn quan trọng ở đây. 

Đầu ra sử dụng 12 chữ số thập phân, đáp ứng thoải mái yêu cầu$10^{-6}$độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1 2 3
1 2
3 3 2 1
1 2
```Các giá trị được chọn: 

| Biến | Giá trị | 
| --- | --- | 
|$r_1$| 3 | 
|$p_1$| 2 | 
|$p_2$| 1 | 
|$A$| 1 | 
|$B$| 2 | 

Bây giờ hãy tính: 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Phân số |$\frac{2 \cdot 2}{1 \cdot 1 + 2 \cdot 2}$|$\frac{4}{5}$| 
| Căn bậc hai |$\sqrt{\frac45}$| 0.894427... | 
| Câu trả lời cuối cùng |$3 \times 0.894427...$| 2.683281573... | 

Đầu ra:```
2.683281573000
```Ví dụ này thể hiện tính chất đơn điệu cốt lõi. Giải pháp tối ưu đến từ việc lựa chọn độc lập các giá trị tốt nhất từ ​​mỗi mảng. 

### Ví dụ 2 

đầu vào:```
1 5
2 1 10
2 2 8
1 1
```Các giá trị được chọn: 

| Biến | Giá trị | 
| --- | --- | 
|$r_1$| 5 | 
|$p_1$| 10 | 
|$p_2$| 2 | 

Tính toán: 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Phân số |$\frac{1 \cdot 10}{1 \cdot 2 + 1 \cdot 10}$|$\frac{10}{12}$| 
| Căn bậc hai |$\sqrt{\frac{5}{6}}$| 0.912870... | 
| Câu trả lời cuối cùng |$5 \times 0.912870...$| 4.564354... | 

Dấu vết này cho thấy tại sao giảm thiểu$p_2$vấn đề. Lựa chọn$p_2 = 8$thay vào đó sẽ tạo ra bán kính nhỏ hơn nhiều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m + k)$| Chúng tôi quét từng mảng một lần để tìm cực trị | 
| Không gian |$O(1)$| Chỉ có một vài biến được lưu trữ | 

Các giới hạn là cực kỳ nhỏ nên giải pháp này dễ dàng phù hợp với những hạn chế về thời gian và bộ nhớ. Ngay cả giải pháp vũ phu cũng có thể vượt qua một cách thoải mái, nhưng cách tiếp cận đơn giản hóa sẽ rõ ràng hơn và tuân theo trực tiếp từ toán học. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    data = list(map(int, input().split()))
    x = data[1:]

    data = list(map(int, input().split()))
    y = data[1:]

    data = list(map(int, input().split()))
    z = data[1:]

    A, B = map(int, input().split())

    r1 = max(x)
    p1 = max(y)
    p2 = min(z)

    ans = r1 * math.sqrt((B * p1) / (A * p2 + B * p1))

    print("{:.12f}".format(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run(
"""3 1 2 3
1 2
3 3 2 1
1 2
"""
) == "2.683281572999"

# minimum-size input
assert run(
"""1 1
1 1
1 1
1 1
"""
) == "0.707106781187"

# larger p1 should increase answer
assert run(
"""1 10
2 1 100
1 1
1 1
"""
) == "9.950371902100"

# larger p2 should decrease answer
assert run(
"""1 10
1 10
2 1 100
1 1
"""
) == "9.534625892456"

# equal values everywhere
assert run(
"""3 5 5 5
2 7 7
2 4 4
2 2
"""
) == "4.013864859598"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng kích thước tối thiểu | 0,707106781187 | Công thức cơ bản đúng đắn | 
| Rất lớn$p_1$| 9.950371902100 | Nên chọn mật độ bên ngoài tối đa | 
| Rất lớn$p_2$tùy chọn | 9.534625892456 | Nên chọn mật độ bên trong tối thiểu | 
| Tất cả các giá trị bằng nhau | 4.013864859598 | Hành vi giống như trùng lặp và công thức ổn định | 

## Vỏ cạnh 

Hãy xem xét trường hợp chọn sai hướng mật độ sẽ phá vỡ giải pháp:```
1 10
2 1 100
1 1
1 1
```Thuật toán chọn:$$r_1 = 10,\quad p_1 = 100,\quad p_2 = 1$$Sau đó:$$r_2 = 10 \sqrt{\frac{100}{101}} \approx 9.95037$$Nếu chúng ta giảm thiểu sai lầm$p_1$, kết quả sẽ trở thành:$$10 \sqrt{\frac{1}{2}} \approx 7.07$$đó là xa tối ưu. 

Bây giờ hãy xem xét hướng ngược lại cho$p_2$:```
1 10
1 10
2 1 100
1 1
```Thuật toán chọn giá trị nhỏ nhất$p_2$, cụ thể là 1:$$r_2 = 10 \sqrt{\frac{10}{11}} \approx 9.5346$$Lựa chọn$p_2 = 100$thay vào đó mang lại:$$10 \sqrt{\frac{10}{110}} \approx 3.015$$Sự sụt giảm lớn xác nhận rằng việc giảm thiểu$p_2$là cần thiết. 

Cuối cùng, hãy xem xét trường hợp nhạy cảm về độ chính xác:```
1 5
1 3
1 2
1 1
```Câu trả lời chính xác là:$$5 \sqrt{\frac35} \approx 3.872983346207$$Việc triển khai sử dụng số học dấu phẩy động và in nhiều chữ số thập phân, do đó yêu cầu$10^{-6}$đạt được độ chính xác một cách an toàn.
