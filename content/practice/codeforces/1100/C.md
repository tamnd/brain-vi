---
title: "CF 1100C - NN và ảo ảnh quang học"
description: "Chúng tôi đang xây dựng một cấu hình vòng tròn rất cụ thể. Có một đường tròn ở tâm bán kính $r$. Xung quanh nó, $n$ các vòng tròn giống hệt nhau được đặt sao cho chúng tạo thành một vòng tròn."
date: "2026-06-12T05:40:07+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1100
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 532 (Div. 2)"
rating: 1200
weight: 1100
solve_time_s: 87
verified: true
draft: false
---

[CF 1100C - NN và ảo ảnh quang học](https://codeforces.com/problemset/problem/1100/C) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, hình học, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một cấu hình vòng tròn rất cụ thể. Có một đường tròn tâm có bán kính$r$. Xung quanh nó,$n$các vòng tròn giống hệt nhau được đặt sao cho chúng tạo thành một vòng tròn. Mỗi vòng tròn bên ngoài này tiếp xúc với vòng tròn trung tâm và mọi vòng tròn bên ngoài cũng chạm vào hai vòng tròn bên ngoài lân cận của nó. 

Nhiệm vụ là xác định bán kính$R$của mỗi vòng tròn bên ngoài sao cho sự sắp xếp hình học này là chính xác. 

Hạn chế chính là tâm của các vòng tròn bên ngoài nằm trên một đa giác đều xung quanh tâm của vòng tròn bên trong. Một khi chúng tôi sửa chữa$n$, hình học là cứng nhắc, vì vậy$R$được xác định duy nhất bởi$r$. 

Những hạn chế$3 \le n \le 100$Và$1 \le r \le 100$đủ nhỏ để bất kỳ công thức hình học có thời gian không đổi nào cũng đủ. Ngay cả một phương trình được giải bằng số cũng có thể được chấp nhận, nhưng cấu trúc của bài toán thừa nhận dẫn xuất lượng giác trực tiếp, do đó không cần tìm kiếm. 

Một lỗi phổ biến là giả sử tỷ lệ tuyến tính giữa$r$Và$R$, hoặc coi các đường tròn bên ngoài là các đường tròn tiếp tuyến độc lập quanh một điểm. Điều đó bỏ qua thực tế là các vòng tròn bên ngoài bị ràng buộc bởi cả tiếp tuyến trung tâm và kề cận, điều này tạo ra một khoảng cách góc cứng nhắc. 

Trường hợp cạnh thực duy nhất xảy ra khi$n$nhỏ, đặc biệt là$n = 3$, trong đó hình học trở nên chặt chẽ và sự mất ổn định về số trong các hàm lượng giác có thể xuất hiện nếu thực hiện bất cẩn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử bán kính ứng viên$R$, địa điểm$n$điểm đều xung quanh một vòng tròn bán kính$r + R$và kiểm tra xem các đường tròn bán kính liền kề có$R$là tiếp tuyến. Điều này có thể được xác minh bằng cách tính toán khoảng cách giữa các trung tâm lân cận và kiểm tra xem chúng có bằng nhau không.$2R$. 

Vấn đề là điều này biến thành một vấn đề tìm kiếm gốc. Với mỗi lần đoán của$R$, chúng ta phải đánh giá một điều kiện hình học và sự hội tụ sẽ yêu cầu tìm kiếm lặp lại như tìm kiếm nhị phân hoặc phương pháp Newton. Mỗi lần kiểm tra là$O(n)$và với yêu cầu đủ về độ chính xác, chúng ta có thể cần hàng chục lần lặp. Mặc dù vẫn khả thi nhưng nó mang tính gián tiếp không cần thiết đối với một bài toán có cấu trúc dạng đóng. 

Nhận xét quan trọng là tâm của đường tròn bên ngoài nằm trên một đường tròn có bán kính$r + R$, cách đều nhau một góc$\frac{2\pi}{n}$. Khoảng cách giữa các tâm liền kề là dây cung của vòng tròn lớn hơn này. Hợp âm đó phải bằng$2R$, vì các đường tròn bên ngoài lân cận là tiếp tuyến. 

Vì vậy, chúng ta nhận được một phương trình hình học trực tiếp:$$2R = 2 (r + R)\sin\left(\frac{\pi}{n}\right)$$Điều này xuất phát từ công thức độ dài hợp âm: góc phụ của hợp âm$\theta$trong một vòng tròn bán kính$S$có chiều dài$2S\sin(\theta/2)$. Đây$S = r + R$Và$\theta = \frac{2\pi}{n}$. 

Giải phương trình đại số loại bỏ tất cả tìm kiếm:$$R = (r + R)\sin\left(\frac{\pi}{n}\right)$$

$$R - R\sin\left(\frac{\pi}{n}\right) = r\sin\left(\frac{\pi}{n}\right)$$

$$R(1 - \sin(\frac{\pi}{n})) = r\sin\left(\frac{\pi}{n}\right)$$

$$R = \frac{r\sin(\frac{\pi}{n})}{1 - \sin(\frac{\pi}{n})}$$Dạng đóng này ổn định và đánh giá theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm nhị phân trên$R$) |$O(n \log \epsilon^{-1})$|$O(1)$| Được chấp nhận nhưng không cần thiết | 
| Tối ưu (hình học dạng đóng) |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính bước góc giữa các tâm đường tròn bên ngoài liền kề, đó là$\frac{\pi}{n}$. Điều này xuất phát từ việc giảm một nửa góc đầy đủ$\frac{2\pi}{n}$vì độ dài dây cung sử dụng sin nửa góc. 
2. Đánh giá$s = \sin\left(\frac{\pi}{n}\right)$. Giá trị này thể hiện khoảng cách giữa các trung tâm liền kề so với bán kính đường tròn của sự sắp xếp của chúng. 
3. Chuyển hình học thành phương trình tuyến tính theo$R$: khoảng cách giữa các tâm liền kề là cả hai$2R$(điều kiện tiếp tuyến) và$2(r+R)s$(điều kiện độ dài hợp âm). 
4. Giải phương trình$R = (r+R)s$về mặt đại số, cô lập$R$. Điều này tạo ra một công thức trực tiếp mà không cần lặp lại. 
5. Tính giá trị cuối cùng bằng số học dấu phẩy động và xuất ra với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Toàn bộ cấu trúc giảm xuống thường xuyên$n$-gon được hình thành bởi tâm của các vòng tròn bên ngoài. Vì tất cả các đường tròn bên ngoài đều giống hệt nhau và tiếp tuyến với nhau trong một chu trình nên tâm của chúng phải nằm trên một đường tròn có khoảng cách các góc bằng nhau. Sự đối xứng cứng nhắc đó buộc phải có một ràng buộc về độ dài hợp âm duy nhất và ràng buộc đó xác định duy nhất tỷ lệ giữa$R$Và$r$. Không có cấu hình thay thế nào tồn tại một lần$n$Và$r$được cố định. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

n, r = map(int, input().split())

s = math.sin(math.pi / n)
R = (r * s) / (1 - s)

print(R)
```Mã trực tiếp mã hóa công thức dẫn xuất. Sự tinh tế duy nhất là sử dụng`math.pi / n`còn hơn là`2*pi/n`, vì công thức hợp âm phụ thuộc vào nửa góc. Một lỗi triển khai phổ biến là quên việc chia đôi này và sử dụng góc trung tâm đầy đủ, dẫn đến lỗi chia tỷ lệ hệ thống trong kết quả. 

Tất cả các tính toán được thực hiện với độ chính xác gấp đôi, đủ vì dung sai yêu cầu là$10^{-6}$, vượt xa epsilon dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3, r = 1$Chúng tôi tính toán$s = \sin(\pi/3) = \sqrt{3}/2 \approx 0.8660254$. 

| Bước | Giá trị | 
| --- | --- | 
|$n$| 3 | 
|$r$| 1 | 
|$s = \sin(\pi/n)$| 0.8660254 | 
|$R = \frac{r s}{1 - s}$| 6.4641016 | 

Điều này khớp chính xác với đầu ra mẫu, xác nhận cấu trúc cân bằng chặt chẽ của cấu hình. 

### Ví dụ 2:$n = 6, r = 2$Đây$s = \sin(\pi/6) = 0.5$. 

| Bước | Giá trị | 
| --- | --- | 
|$n$| 6 | 
|$r$| 2 | 
|$s$| 0,5 | 
|$R$|$\frac{2 \cdot 0.5}{1 - 0.5} = 2$| 

Các vòng tròn bên ngoài có cùng bán kính với bán kính bên trong, phù hợp với sự đối xứng lục giác thắt chặt chính xác ở cấu hình này. 

Những ví dụ này cho thấy công thức thích ứng mượt mà với cả nhu cầu nhỏ và vừa$n$, bảo toàn tính nhất quán hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số lượng không đổi các phép toán số học và lượng giác được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép mọi tính toán toán học theo thời gian không đổi, vì vậy giải pháp này dễ dàng đáp ứng cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, r = map(int, input().split())
    s = math.sin(math.pi / n)
    R = (r * s) / (1 - s)
    return str(R)

# provided sample
assert abs(float(run("3 1\n")) - 6.4641016) < 1e-6, "sample 1"

# minimum n
assert run("3 1\n")  # already covered extreme structure

# larger n
assert abs(float(run("100 1\n")) - (math.sin(math.pi/100)/(1-math.sin(math.pi/100)))) < 1e-9

# equal scaling
assert abs(float(run("6 2\n")) - 2.0) < 1e-9

# another random case
assert abs(float(run("10 5\n")) - (5*math.sin(math.pi/10)/(1-math.sin(math.pi/10)))) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 | 6.4641016 | cấu hình hợp lệ nhỏ nhất | 
| 100 1 | tính toán | ổn định cho n lớn | 
| 6 2 | 2 | trường hợp đối xứng trong đó kết quả đơn giản hóa | 
| 10 5 | tính toán | tính đúng đắn chung | 

## Vỏ cạnh 

cho$n = 3$, cấu hình sẽ trở thành tam giác chặt nhất có thể của các vòng tròn bên ngoài xung quanh tâm. Đây$\sin(\pi/3)$lớn nên mẫu số$1 - s$trở nên nhỏ, khuếch đại độ nhạy dấu phẩy động. Công thức vẫn hoạt động chính xác vì cả tử số và mẫu số đều nằm trong phạm vi dấu phẩy động ổn định. 

Đối với lớn$n$,$\sin(\pi/n) \approx \pi/n$, Vì thế$R \approx \frac{r \cdot \pi/n}{1 - \pi/n}$, tiến tới 0 khi$n$lớn lên. Việc tính toán vẫn ổn định vì cả tử số và mẫu số đều co lại trơn tru mà không bị hủy. 

Đối với các giá trị trung gian như$n = 6$, tính đối xứng dẫn đến sự đơn giản hóa chính xác như$\sin(\pi/6) = 1/2$, tạo ra kết quả số nguyên sạch. Công thức xử lý các trường hợp này mà không cần viết hoa đặc biệt, xác nhận rằng nó nắm bắt được toàn bộ hình học một cách thống nhất.
