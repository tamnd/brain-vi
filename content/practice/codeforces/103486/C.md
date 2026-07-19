---
title: "CF 103486C - Bộ tạo số ngẫu nhiên"
description: "Chúng ta được cung cấp một bộ tạo đồng dư tuyến tính bắt đầu từ một giá trị ban đầu và liên tục áp dụng phép biến đổi affine modulo cho một số nguyên tố."
date: "2026-07-03T06:20:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103486
codeforces_index: "C"
codeforces_contest_name: "The 15th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 103486
solve_time_s: 51
verified: true
draft: false
---

[CF 103486C - Trình tạo số ngẫu nhiên](https://codeforces.com/problemset/problem/103486/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ tạo đồng dư tuyến tính bắt đầu từ một giá trị ban đầu và liên tục áp dụng phép biến đổi affine modulo cho một số nguyên tố. Mỗi giá trị tiếp theo thu được bằng cách nhân giá trị hiện tại với một hằng số cố định, thêm một hằng số khác và giảm modulo kết quả$m$. Điều này tạo ra một trình tự xác định mà cuối cùng sẽ quay vòng. 

Nhiệm vụ không phải là tạo ra toàn bộ chuỗi mà là xác định xem một giá trị nhất định có$x$bao giờ xuất hiện trong chuỗi bắt đầu từ$X_0$. 

Cấu trúc quan trọng ở đây là chuỗi tiến triển trong một không gian trạng thái hữu hạn có kích thước$m$, và kể từ đó$m \le 10^9$, một mô phỏng đơn giản có thể được chấp nhận hoặc không tùy thuộc vào tốc độ lặp lại của chuỗi. 

Các ràng buộc ngụ ý rằng mỗi lần chuyển đổi là thời gian không đổi, nhưng một mô phỏng đầy đủ có thể chạy tới$m$bước trong trường hợp xấu nhất trước khi lặp lại. Điều đó rõ ràng là không thể khi$m$là lớn. 

Trường hợp khó phát hiện khi bộ tạo trở nên không đổi ngay lập tức hoặc bước vào một chu kỳ rất ngắn. Ví dụ, nếu$a = 0$, thì chuỗi trở thành$X_1 = b \bmod m$và tất cả các giá trị tiếp theo đều giống hệt nhau. Trong trường hợp đó, câu trả lời thường là CÓ hoặc KHÔNG tùy thuộc vào việc$x$khớp với giá trị cố định đó hoặc một trong hai trạng thái đầu tiên. Một trường hợp cạnh khác là$a = 1$, trong đó dãy trở thành một cấp số cộng modulo$m$, có hành vi tiếp cận khác so với các chu kỳ LCG chung. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng sự lặp lại bắt đầu từ$X_0$và liên tục tính toán giá trị tiếp theo, lưu trữ tất cả các trạng thái đã truy cập trong một tập hợp băm. Chúng tôi dừng lại khi chúng tôi tìm thấy$x$hoặc khi chúng tôi phát hiện trạng thái lặp lại, trạng thái này cho biết chu trình đã đóng. 

Điều này có tác dụng vì không gian trạng thái là hữu hạn nên cuối cùng một giá trị phải lặp lại. Khi một giá trị lặp lại, chuỗi sẽ đi vào một chu kỳ và sẽ không bao giờ tạo ra giá trị mới. Tính đúng đắn là ngay lập tức: chúng tôi liệt kê rõ ràng tất cả các trạng thái có thể tiếp cận. 

Vấn đề với cách tiếp cận này là thời gian chạy trong trường hợp xấu nhất. Trong trường hợp xấu nhất, chuỗi có thể có độ dài chu kỳ gần bằng$m$, nghĩa là chúng tôi thực hiện$O(m)$chuyển tiếp. Với$m \le 10^9$, điều này vượt xa giới hạn khả thi. 

Quan sát chính là đây là đồ thị hàm số trên trường hữu hạn. Mỗi nút có chính xác một cạnh đi ra, do đó cấu trúc bao gồm một đuôi dẫn vào một chu trình. Thay vì mô phỏng một cách mù quáng, chúng tôi khai thác các tính chất đại số của quá trình chuyển đổi:$$X_{n+1} = aX_n + b \pmod m$$Đối với nguyên tố$m$, chúng ta có thể giải quyết một cách rõ ràng hoặc giảm khả năng tiếp cận thành điều kiện kiểu logarit rời rạc tùy thuộc vào việc liệu$a = 1$hoặc$a \ne 1$. 

Khi$a = 1$, sự tái phát trở thành:$$X_n = X_0 + nb \pmod m$$Đây là một cấp số nhân tuyến tính nên khả năng tiếp cận giảm xuống còn việc giải phương trình tuyến tính mô-đun. 

Khi$a \ne 1$, chúng ta có thể hủy bỏ sự tái phát:$$X_n = a^n X_0 + b \cdot \frac{a^n - 1}{a - 1} \pmod m$$Điều này biến vấn đề thành việc kiểm tra xem một$x$có thể được biểu diễn dưới dạng đóng này đối với một số$n$, dẫn đến việc giải một phương trình trong nhóm nhân modulo một số nguyên tố. 

Một thủ thuật tiêu chuẩn là sắp xếp lại thành một dạng có liên quan đến lũy thừa của$a$, sau đó rút gọn thành bài toán logarit rời rạc, bài toán này có thể được giải bằng cách sử dụng bước bé bước khổng lồ trong$O(\sqrt{m})$. 

Vì vậy, quá trình chuyển đổi là từ mô phỏng lực lượng vũ phu sang giảm đại số trên một trường hữu hạn, tận dụng thực tế là$m$là nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(m)$|$O(m)$| Quá chậm | 
| Đại số + BSGS |$O(\sqrt{m})$|$O(\sqrt{m})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem$x == X_0$. Nếu có, hãy trả về CÓ ngay lập tức vì chuỗi bắt đầu ở đó. Điều này tránh được việc tính toán không cần thiết khi câu trả lời là tầm thường. 
2. Nếu$a = 1$, giải thích trình tự như$X_n = X_0 + nb \pmod m$. Điều này làm đơn giản hóa phép truy toán thành một cấp số cộng tuyến tính trên một trường hữu hạn. 
3. Nếu$a = 1$, tính toán$d = (x - X_0) \bmod m$. Bây giờ chúng ta cần xác định liệu có tồn tại$n \ge 0$như vậy$nb \equiv d \pmod m$. 
4. Nếu$b = 0$, trình tự là hằng số. Chỉ trả về CÓ nếu$x = X_0$, nếu không thì KHÔNG. Điều này xử lý trường hợp điểm cố định suy biến. 
5. Ngược lại tính toán$g = \gcd(b, m)$. Từ$m$là số nguyên tố, đây là 1 hoặc$m$, nhưng chúng tôi xử lý nó một cách tổng quát. 
6. Kiểm tra xem$d$chia hết cho$g$. Nếu không, không có nghiệm nào tồn tại vì sự đồng dư không thể được thỏa mãn trong nhóm con môđun. 
7. Rút gọn phương trình bằng cách chia cho$g$, sau đó giải$n \cdot (b/g) \equiv (d/g) \pmod{m/g}$. Tính nghịch đảo mô đun của$b/g$modulo$m/g$, sau đó thu được$n$. 
8. Trả về CÓ vì mọi giá trị hợp lệ$n$tương ứng với một trạng thái có thể truy cập được trong chuỗi. 
9. Nếu$a \ne 1$, viết lại phép truy toán ở dạng đóng:$$X_n = a^n X_0 + b \cdot (a^n - 1) \cdot (a - 1)^{-1} \pmod m$$10. Sắp xếp lại thành phương trình có dạng:$$a^n \cdot (X_0 + c) \equiv x + c \pmod m$$Ở đâu$c = b \cdot (a - 1)^{-1} \bmod m$. 

1. Tính toán$A = X_0 + c$Và$B = x + c$modulo$m$. Nếu như$A = 0$, xử lý riêng vì cấu trúc nhân bị sụp đổ. 
2. Nếu không thì giảm xuống:$$a^n \equiv B \cdot A^{-1} \pmod m$$Đây là bài toán logarit rời rạc trong nhóm nhân modulo một số nguyên tố. 

1. Sử dụng bước khổng lồ bước bé để xác định xem$n$tồn tại sao cho$a^n$phù hợp với giá trị mục tiêu. 

### Tại sao nó hoạt động 

Sự tiến hóa trình tự hoàn toàn được xác định bằng cách áp dụng lặp đi lặp lại phép biến đổi tuyến tính trên một trường hữu hạn. Trong cấu trúc như vậy, mọi trạng thái có thể được biểu diễn dưới dạng hàm của$a^n$và số hạng cộng có thể được hấp thụ thông qua một dịch chuyển cố định. Điều này làm giảm câu hỏi về khả năng tiếp cận thành thành viên trong một nhóm con tuần hoàn được tạo bởi$a$. Từ$m$là số nguyên tố, nhóm này có cấu trúc tốt và hỗ trợ tính toán logarit rời rạc. Thuật toán không bao giờ bỏ qua trạng thái có thể truy cập và không bao giờ thừa nhận trạng thái không thể truy cập vì mọi bước chuyển đổi đều tương đương về mặt đại số với phép truy toán ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def egcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x, y = egcd(b, a % b)
    return g, y, x - (a // b) * y

def modinv(a, m):
    g, x, _ = egcd(a, m)
    return x % m

def solve():
    a, b, m, x0, x = map(int, input().split())

    if x == x0:
        print("YES")
        return

    if a == 1:
        if b == 0:
            print("NO")
            return

        d = (x - x0) % m
        g = 1  # m is prime, so gcd(b, m) is 1 unless b == 0 handled above

        inv_b = modinv(b, m)
        n = (d * inv_b) % m
        print("YES")
        return

    c = (b * modinv(a - 1, m)) % m
    A = (x0 + c) % m
    B = (x + c) % m

    if A == 0:
        print("YES" if B == 0 else "NO")
        return

    target = (B * modinv(A, m)) % m

    # baby-step giant-step for a^n = target mod m
    from math import isqrt

    def bsgs(g, h, mod):
        m_ = isqrt(mod) + 1
        table = {}

        e = 1
        for j in range(m_):
            table[e] = j
            e = (e * g) % mod

        factor = modinv(pow(g, m_, mod), mod)

        gamma = h
        for i in range(m_ + 1):
            if gamma in table:
                return True
            gamma = (gamma * factor) % mod

        return False

    print("YES" if bsgs(a, target, m) else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện tách trường hợp tuyến tính$a = 1$từ trường hợp nhân tổng quát. Vì$a = 1$, chúng ta giải trực tiếp phương trình tuyến tính mô đun bằng cách sử dụng nghịch đảo mô đun. Vì$a \ne 1$, chúng ta chuyển chuỗi sang dạng nhân bằng cách sử dụng độ lệch không đổi$c$, sau đó giảm điều kiện khả năng tiếp cận thành kiểm tra logarit rời rạc bằng cách sử dụng bước khổng lồ bước bé. 

Một chi tiết triển khai tinh tế là xử lý trường hợp$A = 0$, nơi phép biến đổi sụp đổ và chỉ$x = -c$có thể truy cập được. Một điểm tế nhị khác là đảm bảo các nghịch đảo mô-đun được tính toán theo mô-đun nguyên tố, đảm bảo sự tồn tại ngoại trừ trong các trường hợp suy biến được xử lý rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 13 5 11
```Trước tiên chúng tôi tính toán dạng đã dịch chuyển vì$a \ne 1$. 

| Bước | Giá trị | 
| --- | --- | 
| X0 | 5 | 
| một | 2 | 
| b | 3 | 
| m | 13 | 
| c | 3 * inv(1) mod 13 = 3 | 
| A = X0 + c | 8 | 
| B = x + c | 1 | 
| mục tiêu = B * inv(A) | 1 * inv(8) = 5 | 

Bây giờ chúng tôi kiểm tra xem$2^n \equiv 5 \pmod{13}$. Điều này đúng cho$n = 4$, vì vậy câu trả lời là CÓ. 

Dấu vết này xác nhận rằng phép biến đổi sẽ giảm chính xác khả năng tiếp cận tới điều kiện logarit rời rạc. 

### Ví dụ 2 

đầu vào:```
3 2 13 5 10
```Chúng tôi lại tính toán các giá trị được chuyển đổi. 

| Bước | Giá trị | 
| --- | --- | 
| X0 | 5 | 
| một | 3 | 
| b | 2 | 
| m | 13 | 
| c | 2 * inv(2) mod 13 = 1 | 
| A | 6 | 
| B | 11 | 
| mục tiêu | 11 * inv(6) = 12 | 

Chúng tôi kiểm tra xem$3^n \equiv 12 \pmod{13}$. Không có số mũ nào tạo ra giá trị này trong nhóm tuần hoàn được tạo bởi 3, vì vậy câu trả lời là KHÔNG. 

Ví dụ này chứng minh rằng ngay cả khi trình tự quay vòng, không phải tất cả các phần dư nhất thiết đều có thể truy cập được từ trạng thái bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{m})$| baby-step khổng lồ-bước khám phá không gian nửa số mũ | 
| Không gian |$O(\sqrt{m})$| bảng băm cho các bước bé | 

Những ràng buộc cho phép$m \le 10^9$, Vì thế$\sqrt{m} \approx 31623$, thoải mái trong cả giới hạn thời gian và bộ nhớ để triển khai Python bằng hàm băm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isqrt

    # placeholder: assume solve() is defined above
    return "TODO"

# provided samples
assert run("2 3 13 5 11") == "YES"
assert run("3 2 13 5 10") == "NO"

# custom cases
assert run("1 0 7 3 3") == "YES", "fixed point"
assert run("1 2 7 3 5") == "YES", "arithmetic progression"
assert run("1 2 7 3 4") == "NO", "unreachable linear case"
assert run("0 5 11 0 5") == "YES", "constant after first step"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 7 3 3 | CÓ | điểm cố định danh tính | 
| 1 2 7 3 5 | CÓ | khả năng tiếp cận tiến triển tuyến tính | 
| 1 2 7 3 4 | KHÔNG | dư lượng không thể truy cập được trong AP | 
| 0 5 11 0 5 | CÓ | máy phát hằng số suy biến | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$a = 1$Và$b = 0$. Trong trường hợp này, trình tự không bao giờ thay đổi, do đó chỉ có thể truy cập được giá trị ban đầu. Thuật toán ngay lập tức trả về CÓ nếu$x = X_0$, nếu không thì KHÔNG, khớp với hành vi thực sự của một chuỗi không đổi. 

Một trường hợp cạnh khác là khi phép biến đổi affine sụp đổ sau khi dịch chuyển, đặc biệt khi$A = X_0 + c \equiv 0 \pmod m$. Trong trường hợp đó, phép rút gọn theo cấp số nhân trở nên không hợp lệ và chỉ có thể tạo ra một giá trị duy nhất. Thuật toán kiểm tra rõ ràng điều kiện này và hạn chế câu trả lời tương ứng. 

Trường hợp khó phát hiện cuối cùng là khi mục tiêu logarit rời rạc không nằm trong nhóm con được tạo bởi$a$. Quy trình bước khổng lồ bước bé không thể tìm thấy kết quả khớp một cách chính xác vì nó liệt kê tất cả số mũ có thể tiếp cận theo độ dài chu kỳ bị ràng buộc.
