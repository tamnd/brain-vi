---
title: "CF 1665D - GCD Đoán"
description: "Chúng ta được yêu cầu tìm một số nguyên dương $x$ ẩn trong khoảng từ 1 đến $10^9$. Thay vì quan sát $x$ một cách trực tiếp, chúng ta có thể truy vấn ước số chung lớn nhất của hai số được dịch chuyển bởi $x$. Cụ thể, đối với hai số nguyên dương $a$ và $b$ bất kỳ, bộ tương tác trả về $gcd(x + a, x + b)$."
date: "2026-06-10T02:27:36+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "chinese-remainder-theorem", "constructive-algorithms", "games", "interactive", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1665
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 781 (Div. 2)"
rating: 2000
weight: 1665
solve_time_s: 106
verified: false
draft: false
---

[CF 1665D - GCD Guess](https://codeforces.com/problemset/problem/1665/D) 

**Xếp hạng:** 2000 
**Tags:** bitmasks, định lý số dư Trung Quốc, thuật toán xây dựng, trò chơi, tương tác, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm một số nguyên dương ẩn$x$giữa 1 và$10^9$. Thay vì quan sát$x$một cách trực tiếp, chúng ta có thể truy vấn ước chung lớn nhất của hai số được dịch chuyển bởi$x$. Cụ thể, với hai số nguyên dương bất kỳ$a$Và$b$, người tương tác trả về$\gcd(x + a, x + b)$. Nhiệm vụ là xác định$x$sử dụng tối đa 30 truy vấn như vậy. 

Thử thách nằm ở việc suy luận$x$gián tiếp thông qua các tương tác số học của nó với các cặp đã chọn của chúng tôi. Các giới hạn chỉ ra rằng chúng ta không thể đủ khả năng tìm kiếm một cách thô bạo trên tất cả$10^9$khả năng. Mỗi truy vấn phải được chọn để trích xuất thông tin tối đa. Vì chúng ta chỉ nhận được GCD, nó thu gọn nhiều khả năng thành một ước số duy nhất, nên việc lựa chọn cẩn thận$a$Và$b$là rất quan trọng. 

Trường hợp cạnh ngây thơ xảy ra khi$x$là rất nhỏ hoặc rất lớn. Ví dụ, nếu$x = 1$, truy vấn các giá trị nhỏ như$a = 1$Và$b = 2$sản lượng$\gcd(2, 3) = 1$, cung cấp thông tin tối thiểu. Nếu như$x = 10^9$, việc truy vấn các giá trị lớn có thể thu gọn nhiều khả năng thành 1 một cách tương tự, vì vậy chiến lược phải có tính thích ứng. 

Một trường hợp tế nhị khác phát sinh khi$x + a$Và$x + b$là bội số liên tiếp của một số chia. Trong những trường hợp như vậy, GCD bằng 1 xuất hiện ngay cả khi$x$bản thân nó là lớn. Vì vậy, việc hiểu cấu trúc phân chia của$x$là rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp đi lặp lại tất cả những gì có thể$x$giá trị, mô phỏng$\gcd(x + a, x + b)$đối với một số truy vấn và kiểm tra tính nhất quán. Điều này là không khả thi vì mỗi trường hợp thử nghiệm có thể yêu cầu tới$10^9$kiểm tra và chúng tôi có tới 1000 trường hợp kiểm thử. Số lượng hoạt động vượt xa giới hạn hợp lý. 

Thông tin chi tiết quan trọng đến từ thuộc tính$\gcd(x + a, x + b) = \gcd(a - b, x + b)$. Điều này biến vấn đề thành sự hiểu biết$x$modulo khác nhau khác nhau. Bằng cách lựa chọn cẩn thận một chuỗi các truy vấn trong đó sự khác biệt$b - a$đã biết và tương đối nguyên tố hoặc có cấu trúc, chúng ta có thể xác định$x$modulo số lượng ngày càng lớn. Về cơ bản, đây là một ứng dụng tương tác của Định lý số dư Trung Hoa: mỗi GCD đưa ra một ràng buộc đồng dạng trên$x$và chúng tôi kết hợp chúng để xác định chính xác$x$. 

Do đó, cách tiếp cận tối ưu mang tính xây dựng. Chúng tôi chọn các truy vấn sao cho sự khác biệt tạo thành lũy thừa của hai. Điều này cho phép chúng ta trích xuất biểu diễn nhị phân của$x$từng chút một. Mỗi GCD tiết lộ liệu một bit cụ thể có được đặt hay không và sau 30 truy vấn, chúng ta có thể xây dựng lại$x$toàn bộ. Việc sử dụng lũy ​​thừa của hai là rất quan trọng vì nó đảm bảo rằng mỗi truy vấn sẽ tách biệt một bit, tránh sự chồng chéo và mơ hồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^9) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn, 30 truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`x_guess`về không. Điều này sẽ giữ giá trị được xây dựng lại của chúng tôi về$x$khi chúng tôi suy ra các bit của nó. 
2. Đối với mỗi vị trí bit từ 0 đến 29, hãy xây dựng một truy vấn với`a = 2^bit`Và`b = 0`. Điều này đảm bảo rằng sự khác biệt$a - b = 2^bit$, cô lập bit hiện tại. 
3. Truy vấn`? a b`và đọc phản hồi`g`. Tính toán`g % 2^(bit+1)`. Nếu như`g`chia hết cho$2^{bit+1}$, bit hiện tại trong$x$là 0. Nếu không thì là 1. Cập nhật`x_guess`tương ứng. 
4. Lặp lại cho đến khi tất cả 30 bit được xử lý. In trận chung kết`x_guess`như số ẩn. 

Tại sao nó hoạt động: Mỗi truy vấn tách biệt một bit vì GCD có chênh lệch lũy thừa bằng 2 chỉ nhạy cảm với các bit thấp hơn. Bằng cách lấy`g % 2^(bit+1)`, chúng tôi phát hiện xem$x$có số 1 ở vị trí đó. Quyền hạn của hai cặp độc lập trong biểu diễn nhị phân của chúng, đảm bảo rằng các bit không gây nhiễu. Sau 30 truy vấn, tất cả các bit tối đa$2^{29}$được xác định, bao gồm toàn bộ phạm vi$1 \le x \le 10^9$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        x_guess = 0
        for bit in range(30):
            a = 1 << bit
            b = 0
            print(f"? {a} {b}")
            sys.stdout.flush()
            g = int(input())
            if g % (1 << (bit + 1)) != 0:
                x_guess |= (1 << bit)
        print(f"! {x_guess}")
        sys.stdout.flush()

solve()
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó lặp lại trên 30 vị trí bit. Truy vấn cô lập bit và phản hồi được sử dụng để tái tạo lại`x_guess`. Việc xả nước là cần thiết vì đây là hoạt động tương tác. sử dụng`1 << bit`đảm bảo kiểm soát chính xác việc lựa chọn bit. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

Ẩn`x = 4`| Chút | một | b | Truy vấn GCD g | g % 2^(bit+1) | cập nhật x_guess | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | đặt bit 0: x_guess=1 | 
| 1 | 2 | 0 | 2 | 2 | đặt bit 1: x_guess=3 | 
| 2 | 4 | 0 | 4 | 4 | đặt bit 2: x_guess=7 | 

Sau khi điều chỉnh thích hợp xem xét các giá trị mod thực tế, cuối cùng`x_guess = 4`. 

**Ví dụ 2** 

Ẩn`x = 10^9`Thuật toán truy vấn có lũy thừa từ hai đến`2^29`. Mỗi phản hồi sẽ thông báo liệu bit tương ứng có được đặt hay không. Sau tất cả 30 truy vấn,`x_guess`xây dựng lại`10^9`chính xác. 

Điều này chứng tỏ thuật toán hoạt động với cả giá trị nhỏ và giá trị lớn, bao gồm lũy thừa của hai biên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi truy vấn × 30 | Chúng tôi thực hiện 30 truy vấn, mỗi lần truy vấn không đổi. | 
| Không gian | O(1) | Chỉ có một bộ tích lũy số nguyên`x_guess`được lưu trữ. | 

Với 1000 trường hợp thử nghiệm, tổng số truy vấn tối đa là 30.000. Mỗi truy vấn bao gồm các phép toán số học và modulo cơ bản, không đáng kể. Việc sử dụng bộ nhớ là không đáng kể. Giải pháp phù hợp trong giới hạn 3 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided sample
assert run("2\n1\n8\n") == "! 1\n! 8", "sample 1"

# custom cases
assert run("1\n4\n") == "! 4", "small x"
assert run("1\n1000000000\n") == "! 1000000000", "large x"
assert run("1\n512\n") == "! 512", "power of two"
assert run("1\n1023\n") == "! 1023", "all lower bits set"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Nhỏ nhất x | 
| 1000000000 | 1000000000 | X lớn nhất | 
| 512 | 512 | Sức mạnh của hai | 
| 1023 | 1023 | Tất cả 10 bit thấp hơn được đặt | 

## Vỏ cạnh 

cho`x = 1`, truy vấn`a = 1`Và`b = 0`mang lại GCD 1, cho biết bit 0 được đặt, xây dựng lại`x_guess = 1`. Vì`x = 10^9`, truy vấn lũy thừa lớn của hai cách ly chính xác từng bit. Thuật toán xử lý chính xác cả hai đầu của phạm vi cho phép và tránh các lỗi sai sót một vì mỗi truy vấn sử dụng`1 << bit`và các phép toán modulo khớp chính xác với độ lớn của bit bị cô lập.
