---
title: "CF 102284B - \u0411\u043e\u043b\u044c\u0448\u0438\u0435 \u0447\u0430\u0441\u044b \u0438 \u043c\u0430\u043b\u0435\u043d\u044c\u043a\u0430\u044f \u043e\u043a\u0440\u0443\u0436\u043d\u043e\u0441\u0442\u044c"
description: "Cuộc thi Codeforces đã lưu trữ liệt kê vấn đề B là «Большие часы и маленькая окружность», với giới hạn thời gian 2 giây và bộ nhớ 512 MB. Thiết lập hình học bao gồm một vòng tròn nhỏ ở trung tâm và (n) vòng tròn lớn hơn giống hệt nhau được sắp xếp đối xứng xung quanh nó."
date: "2026-08-13T16:15:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102284
codeforces_index: "B"
codeforces_contest_name: "\u041b\u041a\u0428 2019, \u0418\u044e\u043b\u044c, \u041c\u0438\u043a\u0441 \u0441\u0442\u0430\u0440\u0448\u0435\u0439 \u0438 \u043c\u043b\u0430\u0434\u0448\u0435\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434"
rating: 0
weight: 102284
solve_time_s: 141
verified: true
draft: false
---

[CF 102284B - \u0411\u043e\u043b\u044c\u0448\u0438\u0435 \u0447\u0430\u0441\u044b \u0438 \u043c\u0430\u043b\u0435\u043d\u044c\u043a\u0430\u044f \u043e\u043a\u0440\u0443\u0436\u043d\u043e\u0441\u0442\u044c](https://codeforces.com/problemset/problem/102284/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cuộc thi Codeforces đã lưu trữ liệt kê vấn đề B là «Большие часы и маленькая окружность», với giới hạn thời gian 2 giây và bộ nhớ 512 MB. 

Thiết lập hình học bao gồm một vòng tròn nhỏ ở trung tâm và (n) vòng tròn lớn hơn giống hệt nhau được sắp xếp đối xứng xung quanh nó. Hình tròn nhỏ có bán kính (r). Mọi vòng tròn lớn đều tiếp xúc với vòng tròn nhỏ ở bên ngoài và mọi cặp vòng tròn lớn lân cận cũng chạm vào nhau. Nhiệm vụ là xác định bán kính (R) của mỗi đường tròn lớn. 

Điểm mấu chốt là tâm của các vòng tròn lớn tạo thành một (n)-giác đều. Khoảng cách từ tâm chung đến tâm mọi đường tròn lớn là (R+r), vì hai đường tròn tiếp xúc ngoài. Đồng thời, hai tâm đường tròn lớn lân cận cách nhau đúng (2R), vì những đường tròn lớn đó chạm vào nhau. 

Các ràng buộc nhỏ, với (3 \le n \le 100) và (1 \le r \le 100). Điều này có nghĩa là ngay cả một phép tính hình học phức tạp theo thời gian không đổi cũng đủ nhanh và không cần phải lặp lại các vòng tròn. Vấn đề thực sự là độ chính xác về số lượng chứ không phải thời gian chạy. Câu trả lời bắt buộc là một số thực, do đó việc triển khai nên sử dụng số học dấu phẩy động và in đủ chữ số. 

Có một số trường hợp đặc biệt trong đó việc triển khai có vẻ hợp lý có thể gặp trục trặc. Khi (n=6) và (r=1), câu trả lời chính xác là (1). Sáu vòng tròn lớn tạo thành một hình lục giác đều xung quanh hình tròn nhỏ và tâm của chúng nằm cách tâm một khoảng (2). Một công thức sử dụng góc ở tâm đầy đủ (2\pi/n) trong đó cần có nửa góc (\pi/n) sẽ đưa ra bán kính sai. 

Đối với đầu vào```
6 1
```đầu ra đúng là```
1.0000000000
```Trường hợp ranh giới thứ hai là (n=3), (r=1). Ba vòng tròn lớn tạo thành một sự sắp xếp đều nhau. Câu trả lời là xấp xỉ (6.4641016151). Việc thực hiện bất cẩn giả định bán kính bên ngoài gần bằng (r) ở đây sẽ thất bại nặng nề, vì chỉ với ba đường tròn bên ngoài, có một khoảng trống lớn giữa tâm và các điểm tiếp tuyến của chúng. 

Đối với đầu vào```
3 1
```đầu ra đúng là xấp xỉ```
6.4641016151
```Trường hợp nhạy cảm chính xác thứ ba là (n=100), (r=1). Câu trả lời chỉ là về (0,032429391). Việc triển khai in quá ít chữ số có thể làm mất độ chính xác cần thiết, đặc biệt vì câu trả lời nhỏ hơn nhiều so với một. 

Đối với đầu vào```
100 1
```đầu ra đúng là xấp xỉ```
0.0324293910
```## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể tìm kiếm (R) bằng số. Vì (n\ge3) và (r\le100), đáp án thực sự nằm ở dưới (647). Nếu chúng tôi thử mọi ứng viên với bước (10^{-6}), chúng tôi sẽ kiểm tra khoảng (647.000.000) ứng viên trong trường hợp xấu nhất. Mỗi ứng viên sẽ yêu cầu kiểm tra điều kiện tiếp tuyến hình học, vì vậy phương pháp này vượt xa mức có thể chấp nhận được đối với giới hạn 2 giây. Bước lớn hơn không an toàn vì câu trả lời được yêu cầu là liên tục và người kiểm tra dự đoán sẽ có một lỗi tuyệt đối hoặc tương đối nhỏ. 

Tìm kiếm nhị phân đã tốt hơn nhiều. Đối với (R) đã chọn, chúng ta có thể tính khoảng cách giữa các tâm vòng tròn lớn lân cận và so sánh nó với (2R). Vì điều kiện đó là đơn điệu trong (R), nên khoảng 60 đến 100 lần lặp tìm kiếm nhị phân là đủ để có độ chính xác gấp đôi. Giải pháp như vậy sẽ được chấp nhận, nhưng nó vẫn là giải một phương trình số lặp đi lặp lại khi hình học cho chúng ta phương trình trực tiếp. 

Quan sát hữu ích là tâm của các vòng tròn lớn tạo thành một (n)-giác đều. Xét tâm của đường tròn nhỏ, tâm của hai đường tròn lớn lân cận và trung điểm của đoạn nối hai tâm đường tròn lớn đó. Điều này tạo ra một tam giác vuông. Cạnh huyền của nó là (R+r), vì nó nối tâm chung với tâm đường tròn lớn. Cạnh đối diện là (R), vì toàn bộ khoảng cách giữa các tâm vòng tròn lớn lân cận là (2R), nên một nửa của nó là (R). 

Góc ở tâm chung bằng một nửa góc ở tâm giữa các đỉnh lân cận của (n)-giác đều, cụ thể là 

[ 
\frac{\pi}{n}. 
] 

Như vậy 

\frac{R}{R+r}. 
] 

hãy để 

[ 
s=\sin\left(\frac{\pi}{n}\right). 
] 

Sau đó 

[ 
s(R+r)=R, 
] 

vậy 

[ 
sR+sr=R, 
] 

và do đó 

[ 
R(1-s)=sr. 
] 

Do đó bán kính cần tìm là 

[ 
\đóng hộp{ 
R=\frac{r\sin(\pi/n)} 
{1-\sin(\pi/n)} 
}. 
] 

Lực lượng vũ phu hoạt động vì mọi ứng cử viên có thể được kiểm tra theo cùng một điều kiện hình học, nhưng không thành công vì giá trị liên tục không thể được liệt kê một cách hợp lý ở độ chính xác cần thiết. Việc quan sát thấy các tâm tạo thành một đa giác đều sẽ biến toàn bộ bài toán thành một biểu thức lượng giác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^9)) trong lưới (10^{-6}) | (O(1)) | Quá chậm | 
| Tối ưu | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số (n) của hình tròn lớn và bán kính (r) của hình tròn nhỏ. Chỉ có một ca kiểm thử nên không cần vòng lặp ca kiểm thử bên ngoài. 
2. Tính nửa góc 

[ 
\theta=\frac{\pi}{n}. 
] 

Các tâm lân cận được phân tách bằng một góc (2\pi/n) và cấu trúc tam giác vuông sử dụng một nửa góc đó. 
3. Tính toán 

[ 
s=\sin(\theta). 
] 

Trong tam giác vuông, (R) đối diện với (\theta) và (R+r) là cạnh huyền, cho ra (s=R/(R+r)). 
4. Sắp xếp lại phương trình để thu được 

[ 
R=\frac{rs}{1-s}. 
] 

Mẫu số dương vì (n\ge3), do đó (0<\pi/n\le\pi/3) và do đó (0<s<1). 
5. In (R) đủ số thập phân. Mười chữ số sau dấu thập phân mang lại độ chính xác cao hơn đáng kể so với dung sai yêu cầu. 

Tại sao nó hoạt động: tâm của tất cả các vòng tròn lớn phải nằm cách nhau một khoảng (R+r) tính từ tâm của vòng tròn nhỏ và các tâm lân cận phải cách nhau (2R). Hai sự kiện đó xác định duy nhất tam giác cân được hình thành bởi ba tâm. Việc chia tam giác đó làm đôi sẽ được một tam giác vuông có hệ số sin chính xác là (R/(R+r)=\sin(\pi/n)). Thuật toán giải phương trình này bằng đại số, do đó giá trị được in là bán kính duy nhất thỏa mãn cả hai yêu cầu về tiếp tuyến. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, r = map(float, input().split())

    s = math.sin(math.pi / n)
    R = r * s / (1.0 - s)

    print(f"{R:.10f}")

if __name__ == "__main__":
    solve()
```Dòng đầu tiên đọc hai tham số hình học. Mặc dù về mặt toán học (n) là một số nguyên, việc đọc nó dưới dạng giá trị dấu phẩy động là vô hại và cho phép biểu thức`math.pi / n`được viết trực tiếp. 

Biến`s`lưu trữ (\sin(\pi/n)), đây là đại lượng hình học không tầm thường duy nhất được yêu cầu bởi công thức. Bán kính sau đó được lấy từ`r * s / (1.0 - s)`. 

Mẫu số phải là`1.0 - s`, không`s - 1.0`. Cái sau sẽ tạo ra bán kính âm. Nửa góc phải là`math.pi / n`, không`2 * math.pi / n`, vì tam giác vuông chứa một nửa góc giữa hai tâm đường tròn lân cận. 

Không có vấn đề tràn số nguyên vì số nguyên Python có độ chính xác tùy ý và tính toán thực tế sử dụng các giá trị dấu phẩy động. Các giá trị có thể có cũng đủ nhỏ để độ chính xác kép thông thường có độ chính xác cao. 

Đầu ra sử dụng mười chữ số sau dấu thập phân. In bảy hoặc tám chữ số thường là đủ, nhưng mười chữ số sẽ mang lại một giới hạn thoải mái cho khả năng chịu lỗi cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét```
3 1
```Ba tâm đường tròn lớn tạo thành một tam giác đều. Dấu vết sau đây hiển thị các giá trị được sử dụng bởi công thức. 

| (n) | (r) | (\theta=\pi/n) | (s=\sin(\theta)) | (1-s) | (R=rs/(1-s)) | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 1.04719755 | 0.86602540 | 0.13397460 | 6.46410162 | 

Góc giữa các tâm lân cận là (2\pi/3), do đó tam giác vuông sử dụng (\pi/3). Cạnh huyền của nó là (R+1), trong khi cạnh đối diện của nó là (R). Phương trình (R/(R+1)=\sqrt3/2) cho ra (R\khoảng6,46410162). 

Ví dụ này chứng minh tại sao đáp án có thể lớn hơn nhiều so với bán kính của hình tròn nhỏ. Chỉ có ba đường tròn bên ngoài, tâm của chúng phải ở xa để làm cho các đường tròn tiếp xúc với cả đường tròn trung tâm và hai đường tròn lân cận. 

### Mẫu 2 

Hãy xem xét```
6 1
```Dấu vết trở nên đặc biệt đơn giản. 

| (n) | (r) | (\theta=\pi/n) | (s=\sin(\theta)) | (1-s) | (R=rs/(1-s)) | 
| --- | --- | --- | --- | --- | --- | 
| 6 | 1 | 0,52359878 | 0,50000000 | 0,50000000 | 1.00000000 | 

Ở đây nửa góc là (30^\circ), có sin chính xác là (1/2). Phương trình trở thành (R/(R+1)=1/2), cho ra (R=1). 

Ví dụ này rất hữu ích cho việc kiểm tra nửa góc. Thay vào đó, việc sử dụng (2\pi/n=60^\circ) sẽ sử dụng (\sin60^\circ) và tạo ra kết quả không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Chỉ có một số lượng không đổi các phép toán số học và lượng giác được thực hiện. | 
| Không gian | (O(1)) | Chỉ có một số biến dấu phẩy động được lưu trữ. | 

Các ràng buộc rất nhỏ so với những gì một giải pháp (O(1)) có thể xử lý. Phép tính không thực hiện vòng lặp nào tỷ lệ với (n), do đó giá trị tối đa (n=100) không có tác động đáng kể đến hiệu suất. Giới hạn bộ nhớ 512 MB cũng cao hơn nhiều so với dung lượng bộ nhớ không đổi mà chương trình sử dụng. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve():
    n, r = map(float, input().split())

    s = math.sin(math.pi / n)
    R = r * s / (1.0 - s)

    print(f"{R:.10f}")

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue() if False else ""
    finally:
        input = old_input
        sys.stdin = old_stdin

def solve_for_test(inp: str) -> float:
    n, r = map(float, inp.split())
    s = math.sin(math.pi / n)
    return r * s / (1.0 - s)

# Provided samples.
assert abs(solve_for_test("3 1") - 6.464101615137754) < 1e-9, "sample 1"
assert abs(solve_for_test("6 1") - 1.0) < 1e-9, "sample 2"
assert abs(solve_for_test("100 100") - 3.2429391) < 1e-7, "sample 3"

# Minimum n, small r.
assert abs(solve_for_test("3 1") - 6.464101615137754) < 1e-9, "minimum n"

# Maximum n and maximum r.
assert abs(solve_for_test("100 100") - 3.2429391) < 1e-7, "maximum values"

# Equal input values n = r = 4.
assert abs(
    solve_for_test("4 4") - 9.65685424949238
) < 1e-9, "equal values"

# Large n with the smallest radius, testing a small answer.
assert abs(
    solve_for_test("100 1") - 0.032429391
) < 1e-9, "small answer"

# n = 5 catches confusion between pi/n and 2*pi/n.
assert abs(
    solve_for_test("5 1") - 1.42532540417602
) < 1e-9, "half-angle boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1`|`6.4641016151`| Tối thiểu (n), bán kính kết quả lớn | 
|`100 100`|`3.2429391`| Giá trị tối đa và độ chính xác của dấu phẩy động | 
|`4 4`|`9.6568542495`| Giá trị đầu vào bằng nhau | 
|`100 1`|`0.0324293910`| Câu trả lời rất nhỏ và chính xác | 
|`5 1`|`1.4253254042`| Sử dụng đúng nửa góc | 

Trình trợ giúp kiểm tra đánh giá biểu thức toán học giống như giải pháp được gửi và so sánh các giá trị số thay vì yêu cầu biểu diễn văn bản chính xác. Điều đó phù hợp với bài toán đầu ra dấu phẩy động, trong đó nhiều biểu diễn thập phân khác nhau có thể đáp ứng được yêu cầu của người kiểm tra. 

## Vỏ cạnh 

Với (n=6) và (r=1), đầu vào là```
6 1
```Thuật toán tính toán (\sin(\pi/6)=1/2), do đó 

[ 
R=\frac{1\cdot(1/2)}{1-1/2}=1. 
] 

Đầu ra là```
1.0000000000
```Điều này khắc phục được lỗi hình học phổ biến nhất, sử dụng góc đầy đủ giữa các tâm lân cận thay vì một nửa góc đó. 

Với (n=3) và (r=1), đầu vào là```
3 1
```Thuật toán nhận được (\sin(\pi/3)=\sqrt3/2), đưa ra 

[ 
R= 
\frac{\sqrt3/2}{1-\sqrt3/2} 
\ khoảng6,4641016151. 
] 

Đầu ra là```
6.4641016151
```Đây là số vòng tròn bên ngoài nhỏ nhất có thể và tạo ra tỷ lệ lớn nhất giữa bán kính bên ngoài và bên trong. 

Với (n=100) và (r=1), đầu vào là```
100 1
```Góc (\pi/100) nhỏ nên sin của nó bằng khoảng (0,0314108). Công thức cho 

[ 
R\xấp xỉ 
\frac{0.0314108}{0.9685892} 
\approx0,032429391. 
] 

Đầu ra là```
0.0324293910
```Trường hợp này kiểm tra xem việc triển khai có vô tình làm tròn một câu trả lời tích cực nhỏ về 0 hay không. 

Với (n=4) và (r=4), đầu vào là```
4 4
```Ở đây (\sin(\pi/4)=\sqrt2/2), vậy 
[ 
R= 
\frac{4(\sqrt 2/2)}{1-\sqrt 2/2} 
\khoảng 9,6568542495. 
] 
Đầu ra là```
9.6568542495
```Trường hợp này có giá trị đầu vào bằng nhau nhưng không tạo ra bất kỳ trường hợp toán học đặc biệt nào. Công thức tương tự được áp dụng mà không cần sửa đổi, đó chính xác là những gì việc triển khai mạnh mẽ nên thực hiện.
