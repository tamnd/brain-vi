---
title: "CF 1068B - LCM"
description: "Chúng ta được cho một số nguyên $b$, và về mặt khái niệm, chúng ta lặp lại trên mọi số nguyên dương $a$. Đối với mỗi $a$, chúng tôi tính toán một giá trị xuất phát từ bội số chung nhỏ nhất của $a$ và $b$, cụ thể là $frac{mathrm{lcm}(a,b)}{a}$."
date: "2026-06-15T07:53:44+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1068
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 518 (Div. 2) [Thanks, Mail.Ru!]"
rating: 1200
weight: 1068
solve_time_s: 321
verified: true
draft: false
---

[CF 1068B - LCM](https://codeforces.com/problemset/problem/1068/B) 

**Đánh giá:** 1200 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 5 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$b$và về mặt khái niệm, chúng tôi lặp lại trên mọi số nguyên dương$a$. Đối với mỗi$a$, chúng tôi tính toán một giá trị xuất phát từ bội số chung nhỏ nhất của$a$Và$b$, cụ thể$\frac{\mathrm{lcm}(a,b)}{a}$. Ivan viết giá trị này lên bảng và chúng tôi được hỏi có bao nhiêu giá trị khác biệt xuất hiện khi$a$nằm trên tất cả các số nguyên dương. 

Điểm mấu chốt là mặc dù$a$phạm vi lên đến$10^{18}$, biểu thức chỉ phụ thuộc vào cách$a$tương tác với cấu trúc cơ bản của$b$. Kết quả đầu ra không phải là tính tổng hay tối đa hóa bất cứ thứ gì, mà là đếm xem tỷ lệ này có thể tạo ra bao nhiêu kết quả nhân khác nhau. 

Ràng buộc$b \le 10^{10}$gợi ý rằng một giải pháp chỉ phụ thuộc vào việc phân tích thành thừa số nguyên tố của$b$là khả thi, vì việc phân tích nhân tử lên tới$10^{10}$có thể quản lý được một cách xác định$O(\sqrt{b})$thời gian. Bất kỳ cách tiếp cận nào lặp đi lặp lại trên tất cả$a \le 10^{18}$ngay lập tức là không thể. 

Một nỗ lực ngây thơ sẽ cố gắng đánh giá biểu thức cho nhiều$a$, nhưng điều này không thành công theo hai cách. Đầu tiên, miền của$a$là vô hạn về mặt thực tế. Thứ hai, nhiều thứ khác nhau$a$tạo ra kết quả tương tự và cấu trúc trùng lặp rất đều đặn nhưng không rõ ràng nếu không có lý thuyết số. 

Trường hợp cạnh tinh tế xuất hiện khi$b = 1$. Sau đó$\mathrm{lcm}(a,1) = a$, do đó biểu thức luôn là$1$, cho đúng một giá trị. Một trường hợp cạnh khác là khi$b$là số nguyên tố, trong đó cấu trúc chỉ có hai kết quả có thể xảy ra tùy thuộc vào khả năng chia hết cho số nguyên tố đó. 

## Phương pháp tiếp cận 

Bắt đầu từ định nghĩa của biểu thức. Sử dụng danh tính$$\mathrm{lcm}(a,b) = \frac{ab}{\gcd(a,b)},$$chúng ta viết lại biểu thức như$$\frac{\mathrm{lcm}(a,b)}{a} = \frac{b}{\gcd(a,b)}.$$Điều này biến đổi vấn đề từ một vấn đề về bội số chung nhỏ nhất thành một vấn đề thuần túy về các giá trị gcd. Bây giờ biểu thức chỉ phụ thuộc vào$\gcd(a,b)$, đó phải là ước số của$b$. Điều này ngay lập tức làm giảm miền vô hạn của$a$thành một tập hữu hạn các giá trị gcd có thể. 

Cho phép$g = \gcd(a,b)$. Sau đó$g \mid b$, và biểu thức trở thành$b/g$. Vậy mỗi giá trị ghi trên bảng tương ứng chính xác với việc chọn một ước số$g$của$b$, miễn là một số người thực sự có thể đạt được gcd như vậy$a$. Mỗi số chia$g$của$b$có thể đạt được bằng cách lấy$a = g \cdot k$Ở đâu$k$là nguyên tố cùng nhau với$b/g$, do đó không có ước số nào bị loại trừ. 

Do đó, tập hợp các giá trị phân biệt được viết chính xác là:$$\left\{ \frac{b}{d} \mid d \mid b \right\}.$$Các ước số khác nhau$d$tạo ra các giá trị khác nhau$b/d$, do đó số kết quả phân biệt chính xác là số ước của$b$. 

Vấn đề giảm xuống việc đếm các ước số của$b$, được thực hiện thông qua hệ số nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bạo lực kết thúc$a$|$O(10^{18})$|$O(1)$| Quá chậm | 
| Phân tích nhân tử + số chia |$O(\sqrt{b})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích nhân tử$b$vào quyền lực hàng đầu của nó. Chúng tôi quét các số nguyên lên tới$\sqrt{b}$, trích ra mỗi số nguyên tố chia bao nhiêu lần$b$. Điều này là đủ vì bất kỳ thừa số tổng hợp nào cũng phải có thừa số nguyên tố không vượt quá$\sqrt{b}$. 
2. Đối với mỗi số nguyên tố$p$, đếm số mũ của nó$e$trong việc nhân tử hóa$b$. Bước này quan trọng vì việc đếm số chia chỉ phụ thuộc vào các số mũ này chứ không phụ thuộc vào giá trị thực của$b$. 
3. Tính tổng số ước bằng công thức$$\tau(b) = \prod (e_i + 1),$$mỗi nơi$e_i$là số mũ của một thừa số nguyên tố phân biệt. 
4. Xuất kết quả này làm đáp án, vì mỗi ước số tương ứng với chính xác một giá trị riêng biệt của$\frac{b}{\gcd(a,b)}$. 

### Tại sao nó hoạt động 

Mọi giá trị có thể có của biểu thức được xác định duy nhất bởi ước của$b$, bởi vì biểu thức đơn giản hóa thành$b/g$Ở đâu$g \mid b$. Ngược lại, mọi ước số$g$có thể được nhận ra như một gcd bằng cách xây dựng$a$với các thừa số nguyên tố được chia sẻ thích hợp và tránh các thừa số nguyên tố bổ sung có thể làm tăng gcd. Điều này thiết lập sự tương ứng một-một giữa các ước của$b$và các giá trị riêng biệt được viết trên bảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    b = int(input().strip())
    x = b
    ans = 1

    p = 2
    while p * p <= x:
        if x % p == 0:
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            ans *= (cnt + 1)
        p += 1

    if x > 1:
        ans *= 2

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xác định yếu tố$b$bằng phép chia thử. Mỗi khi một thừa số nguyên tố được tìm thấy, nó sẽ đếm bội số của nó và cập nhật tích số chia. Nếu sau vòng lặp vẫn còn số dư lớn hơn 1 thì số dư đó là thừa số nguyên tố đóng góp số mũ 1, nhân đôi số chia. 

Điểm tinh tế là chúng ta không bao giờ lặp đi lặp lại$a$. Toàn bộ vấn đề sụp đổ thành cấu trúc bên trong$b$, và việc xây dựng lại gcd sẽ loại bỏ mọi sự phụ thuộc vào giới hạn trên rất lớn của$a$. 

## Ví dụ đã hoạt động 

Hãy xem xét$b = 2$. Các ước số là$1, 2$. Các giá trị biểu thức là$2/1 = 2$Và$2/2 = 1$. 

| Bước | Số chia$d$|$b/d$| 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 2 | 1 | 

Điều này hiển thị hai giá trị riêng biệt, khớp với số chia. 

Bây giờ hãy xem xét$b = 12 = 2^2 \cdot 3^1$. Các ước số của nó được xác định bởi các lựa chọn số mũ: đối với$2^2$, chọn số mũ từ 0 đến 2; vì$3^1$, chọn số mũ từ 0 đến 1. 

| Thủ tướng | Số mũ | Đóng góp | 
| --- | --- | --- | 
| 2 | 2 | 3 sự lựa chọn | 
| 3 | 1 | 2 sự lựa chọn | 

Tổng số ước$= 3 \cdot 2 = 6$. 

Điều này chứng tỏ các đóng góp nguyên tố độc lập kết hợp theo cấp số nhân như thế nào, phản ánh cấu trúc của các giá trị dựa trên gcd. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{b})$| thử chia nhân tử lên đến căn bậc hai | 
| Không gian |$O(1)$| chỉ các bộ đếm và một vài biến | 

Sự ràng buộc$b \le 10^{10}$làm cho$\sqrt{b} \approx 10^5$, đủ nhanh trong Python. Thuật toán tránh lặp lại$a$hoàn toàn, chỉ dựa vào cấu trúc số học của$b$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    b = int(input().strip())
    x = b
    ans = 1
    p = 2
    while p * p <= x:
        if x % p == 0:
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            ans *= (cnt + 1)
        p += 1
    if x > 1:
        ans *= 2
    print(ans)

# samples
assert run("1\n") == "1"

# custom tests
assert run("2\n") == "2"
assert run("12\n") == "6"
assert run("36\n") == "9"
assert run("49\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp cạnh nhỏ nhất | 
| 2 | 2 | đầu vào chính | 
| 12 | 6 | hợp nhiều số nguyên tố | 
| 49 | 3 | thừa số nguyên tố lặp lại | 

## Vỏ cạnh 

Khi nào$b = 1$, việc phân tích thành thừa số không mang lại số nguyên tố nào, do đó số ước số vẫn bằng 1. Thuật toán trả về chính xác 1 mà không cần nhập vào thân vòng lặp. 

Khi$b$là số nguyên tố, nói$13$, vòng lặp không tìm thấy phần tử nào và phần còn lại$x > 1$kích hoạt phép nhân với 2, tạo ra chính xác hai ước số$1$Và$13$, khớp với hai giá trị khác biệt được mong đợi của biểu thức. 

Khi$b$là một sức mạnh hoàn hảo như$2^k$, vòng lặp liên tục chia cùng một số nguyên tố và tích lũy số mũ$k$, sản xuất$k+1$các ước số, tương ứng chính xác với số lượng giá trị gcd có thể đạt được và do đó kết quả đầu ra khác biệt của biểu thức ban đầu.
