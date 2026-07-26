---
title: "CF 102888A - \u4e09\u89d2\u5f62\u5207\u534a"
description: "Tam giác này luôn là tam giác vuông có góc vuông $(x0, y0)$. Hai đỉnh còn lại của nó là $(x0 + a, y0)$ và $(x0, y0 + b)$."
date: "2026-07-25T12:24:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102888
codeforces_index: "A"
codeforces_contest_name: "The 15-th Beihang University Collegiate Programming Contest (BCPC 2020) - Preliminary"
rating: 0
weight: 102888
solve_time_s: 56
verified: true
draft: false
---

[CF 102888A - \u4e09\u89d2\u5f62\u5207\u534a](https://codeforces.com/problemset/problem/102888/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tam giác luôn là tam giác vuông có góc vuông bằng$(x_0, y_0)$. Hai đỉnh còn lại của nó là$(x_0 + a, y_0)$Và$(x_0, y_0 + b)$. Chúng ta phải chọn một đường thẳng đứng$x = c$, Ở đâu$x_0 \le c \le x_0 + a$, sao cho phần tam giác bên trái và phần bên phải có diện tích bằng nhau. 

Đầu vào cho biết vị trí của hình tam giác và kích thước của nó. Đầu ra là một giá trị dấu phẩy động duy nhất$c$. Bất kỳ câu trả lời nào trong phạm vi dung sai dấu phẩy động cần thiết đều được chấp nhận. 

Giới hạn là cực kỳ nhỏ. Mọi tọa độ có độ lớn tối đa là 1000 và độ dài cả hai cạnh nhiều nhất là 1000. Vì chỉ có một hình tam giác nên ngay cả một phương pháp số lặp cũng sẽ vừa vặn một cách thoải mái trong giới hạn. Mặc dù vậy, hình học có nghiệm dạng đóng nên bài toán có thể được giải trong thời gian không đổi mà không cần lặp lại. 

Khó khăn chính là vùng bên trái không phát triển tuyến tính với vị trí cắt. Giả sử điểm ở giữa đáy cũng chia đôi diện tích là không chính xác. 

Xem xét đầu vào```
0 0 1 1
```Câu trả lời đúng là khoảng```
0.292893218...
```Cắt tại$x = 0.5$để lại một vùng lớn hơn ở bên trái vì hình tam giác trở nên ngắn hơn khi chúng ta di chuyển sang bên phải. 

Một sai lầm dễ mắc phải khác là quên rằng tam giác có thể không bắt đầu từ gốc tọa độ. 

Ví dụ,```
100 200 4 5
```Câu trả lời không được tính trực tiếp từ khoảng$[0,4]$. Sau khi giải theo tọa độ cục bộ, kết quả phải được dịch ngược lại bằng cách thêm$x_0$. 

Lỗi phổ biến thứ ba là sử dụng số học số nguyên. 

Ví dụ,```
0 0 3 2
```Câu trả lời là vô lý. Phép chia số nguyên hoặc việc cắt ngắn sớm sẽ tạo ra vị trí cắt không chính xác rõ ràng. 

## Phương pháp tiếp cận 

Phương pháp số trực tiếp là tìm kiếm nhị phân vị trí cắt. Đối với bất kỳ ứng cử viên nào$c$, chúng tôi tính diện tích bên trái, so sánh nó với một nửa tổng diện tích và điều chỉnh khoảng thời gian tìm kiếm. Vì diện tích bên trái tăng liên tục khi vết cắt di chuyển sang phải nên tìm kiếm nhị phân là chính xác. Khoảng sáu mươi lần lặp đã mang lại độ chính xác cao hơn nhiều so với yêu cầu, vì vậy phương pháp này chạy trong thời gian không đổi. 

Phần thú vị là tìm ra công thức tính diện tích. Cho phép$$t = c - x_0,$$để có thể$t$đo khoảng cách theo chiều ngang từ cạnh trái của tam giác. 

Cạnh huyền nối với nhau$(0,b)$Và$(a,0)$trong tọa độ địa phương, vì vậy phương trình của nó là$$y = b\left(1-\frac{x}{a}\right).$$Phần tam giác bên trái của$x=t$là hình thang có các cạnh song song có chiều cao$$b,\qquad b\left(1-\frac{t}{a}\right).$$Diện tích của nó là$$S_{\text{left}}
=
\frac{
b+b\left(1-\frac{t}{a}\right)
}{2}
\cdot t
=
bt-\frac{bt^2}{2a}.$$Tổng diện tích tam giác là$$\frac{ab}{2},$$vì vậy việc đặt diện tích bên trái bằng một nửa tổng diện tích sẽ mang lại$$bt-\frac{bt^2}{2a}
=
\frac{ab}{4}.$$Chia cho$b$và sắp xếp lại,$$2t-\frac{t^2}{a}
=
\frac{a}{2},$$hoặc$$t^2-2at+\frac{a^2}{2}=0.$$Gốc hợp lệ bên trong khoảng$[0,a]$là$$t=a\left(1-\frac{1}{\sqrt2}\right).$$Cuối cùng,$$c=x_0+t.$$Điều này tạo ra câu trả lời ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm nhị phân trên câu trả lời) | O(log chính xác) | O(1) | Đã chấp nhận | 
| Tối ưu (hình học dạng đóng) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$x_0$,$y_0$,$a$, Và$b$. Chỉ một$x_0$Và$a$ảnh hưởng đến câu trả lời vì mỗi mặt cắt dọc tỷ lệ thuận với$b$. 
2. Tính offset ngang$$t=a\left(1-\frac{1}{\sqrt2}\right).$$Điều này có được bằng cách giải phương trình bậc hai làm cho diện tích bên trái chính xác bằng một nửa tổng diện tích. 
3. Tính toán$$c=x_0+t.$$Điều này chuyển đổi tọa độ cục bộ trở lại hệ tọa độ ban đầu. 
4. In kết quả dưới dạng số dấu phẩy động. 

### Tại sao nó hoạt động 

Chiều cao của tam giác giảm tuyến tính từ trái sang phải nên diện tích tích lũy là tích phân của hàm tuyến tính, là biểu thức bậc hai. Giải phương trình bậc hai đó để tìm chính xác một nửa tổng diện tích sẽ cho một nghiệm duy nhất bên trong khoảng$[0,a]$. Vì mọi đạo hàm đều chính xác nên giá trị tính toán luôn chia tam giác thành hai vùng có diện tích bằng nhau. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

x0, y0, a, b = map(int, input().split())

c = x0 + a * (1.0 - 1.0 / math.sqrt(2.0))

print(c)
```Đầu tiên chương trình sẽ đọc bốn số nguyên. Mặc dù dữ liệu đầu vào bao gồm cả tọa độ và độ dài cạnh, nhưng chỉ có vị trí nằm ngang và chiều dài ngang xuất hiện trong công thức cuối cùng. 

Việc tính toán sử dụng biểu thức dạng đóng xuất phát từ phương trình diện tích. sử dụng`math.sqrt`giữ phép tính ở dạng dấu phẩy động, dễ dàng đáp ứng khả năng chịu lỗi yêu cầu. 

Cuối cùng, chương trình in trực tiếp câu trả lời. Không cần định dạng đặc biệt vì đầu ra dấu phẩy động mặc định của Python đã cung cấp đủ độ chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
0 0 1 1
```| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào |$x_0$| 0 | 
| Đọc đầu vào |$a$| 1 | 
| Tính toán bù đắp |$t$|$1-\frac1{\sqrt2}\approx0.2928932188$| 
| Tính đáp án |$c$| 0,2928932188 | 

Đường cắt nằm gần mép trái hơn đáng kể so với điểm giữa. Điều này xác nhận rằng chiều rộng bằng nhau không có nghĩa là diện tích bằng nhau vì hình tam giác trở nên ngắn hơn về phía bên phải. 

### Mẫu 2 

đầu vào```
347 -685 868 194
```| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào |$x_0$| 347 | 
| Đọc đầu vào |$a$| 868 | 
| Tính toán bù đắp |$t$| 254.23131393 | 
| Tính đáp án |$c$| 601.23131393 | 

Ví dụ này chứng tỏ rằng độ dài cạnh thẳng đứng và vị trí thẳng đứng không ảnh hưởng gì đến câu trả lời. Chỉ có chiều dài cơ sở và dịch chuyển ngang là quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học được thực hiện. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung được phân bổ. | 

Thuật toán thực hiện một lượng công việc không đổi bất kể giá trị đầu vào. Nó dễ dàng phù hợp với mọi giới hạn thời gian và bộ nhớ hợp lý. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import math
import sys

def solve():
    input = sys.stdin.readline
    x0, y0, a, b = map(int, input().split())
    print(x0 + a * (1 - 1 / math.sqrt(2)))

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

def check(inp, expected):
    ans = float(run(inp))
    assert abs(ans - expected) <= 1e-6

# provided samples
check("0 0 1 1\n", 0.2928932188134524)
check("347 -685 868 194\n", 601.2313139300767)
check("-110 -319 376 122\n", 0.12785027385811532)

# custom cases
check("0 0 1000 1000\n", 292.89321881345245)
check("100 200 4 5\n", 101.17157287525382)
check("-1000 0 1 7\n", -999.7071067811865)
check("0 500 10 1\n", 2.9289321881345245)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1000 1000`|`292.89321881345245`| Độ dài cạnh lớn nhất | 
|`100 200 4 5`|`101.17157287525382`| Dịch ngang | 
|`-1000 0 1 7`|`-999.7071067811865`| Tọa độ âm | 
|`0 500 10 1`|`2.9289321881345245`| Độc lập khỏi`y0`Và`b`| 

## Vỏ cạnh 

Xét tam giác nhỏ nhất có thể:```
0 0 1 1
```Thuật toán tính toán$$t=1-\frac1{\sqrt2},$$vậy câu trả lời là xấp xỉ`0.2928932188`. Điều này khớp với đường cắt có diện tích bằng nhau chính xác thay vì điểm giữa không chính xác tại`0.5`. 

Bây giờ hãy xem xét một tam giác dịch chuyển:```
100 200 4 5
```Giải pháp cục bộ là$$4\left(1-\frac1{\sqrt2}\right)\approx1.171572875.$$Thêm phần bù ngang mang lại```
101.171572875...
```Bản dịch chỉ ảnh hưởng đến phép cộng cuối cùng, giữ nguyên hình học. 

Cuối cùng, hãy xem xét một trường hợp có câu trả lời vô lý:```
0 0 3 2
```Thuật toán tính toán$$3\left(1-\frac1{\sqrt2}\right)\approx0.878679656.$$Việc sử dụng số học dấu phẩy động sẽ đảm bảo độ chính xác cần thiết, trong khi số học số nguyên sẽ làm tròn kết quả không chính xác và khiến người đánh giá thất bại.
