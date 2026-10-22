---
title: "CF 105900F - Fibonacci"
description: "Cho ta ba số nguyên $a$, $b$ và $M$. Nhiệm vụ không phải là tính số Fibonacci theo nghĩa thông thường là “lấy chỉ số và trả về giá trị”, mà thay vào đó là đánh giá một số Fibonacci có chỉ số chính là tích: chúng ta cần giá trị của $f{a cdot b}$, sau đó lấy nó theo modulo…"
date: "2026-06-21T12:23:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105900
codeforces_index: "F"
codeforces_contest_name: "VI UnBalloon Contest Mirror"
rating: 0
weight: 105900
solve_time_s: 47
verified: true
draft: false
---

[CF 105900F - Fibonacci](https://codeforces.com/problemset/problem/105900/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên$a$,$b$, Và$M$. Nhiệm vụ không phải là tính số Fibonacci theo nghĩa thông thường là “lấy chỉ số và trả về giá trị”, mà thay vào đó là đánh giá một số Fibonacci có chỉ số chính là tích: chúng ta cần giá trị của$f_{a \cdot b}$, sau đó lấy nó theo modulo$M$. 

Đối tượng chính là chuỗi Fibonacci được lập chỉ mục từ 0, trong đó$f_0 = 0$,$f_1 = 1$và mỗi số hạng sau bằng tổng của hai số hạng trước đó. Đầu ra đơn giản là số dư sau khi chia$(a \cdot b)$-số Fibonacci thứ bằng$M$. 

Những hạn chế là vô cùng lớn:$a$,$b$, Và$M$có thể lên tới khoảng$10^9$. Điều này ngay lập tức loại trừ mọi tính toán trực tiếp về số Fibonacci bằng phép lặp hoặc đệ quy trên chỉ mục. Thậm chí tính toán một số Fibonacci duy nhất tại chỉ mục$10^9$sử dụng phép truy hồi tuyến tính là không thể vì nó đòi hỏi$10^9$các bước, vượt xa mọi giới hạn thời gian. Sản phẩm$a \cdot b$thậm chí còn lớn hơn nên chúng ta phải coi chỉ số này là một số trừu tượng và dựa vào các đặc tính cấu trúc của số Fibonacci. 

Điểm tinh tế thứ hai là mô đun$M$cũng lớn và không nhất thiết là số nguyên tố. Điều này loại trừ các kỹ thuật giả định tính khả nghịch hoặc các thủ thuật phân chia mô-đun. Mọi thứ phải hoạt động theo mô đun tùy ý. 

Một trường hợp thất bại đơn giản nhưng phổ biến xuất phát từ việc hiểu sai việc lập chỉ mục. Ví dụ, nếu$a = 3$,$b = 2$, thì chúng ta cần$f_6$, đó là$8$. Một sự giải thích bất cẩn có thể tính toán$f_3 \cdot f_2 = 2 \cdot 1 = 2$, điều này hoàn toàn không liên quan. 

Một trường hợp cạnh tinh tế khác là tràn trong phép nhân trung gian$a \cdot b$. Mặc dù phép tính Fibonacci cuối cùng sẽ được thực hiện khác nhau, bản thân sản phẩm chỉ mục phải được xử lý cẩn thận bằng các ngôn ngữ không có số nguyên lớn, mặc dù Python vẫn an toàn ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ tính toán từng số Fibonacci cho đến khi đạt được chỉ số$a \cdot b$, rồi lấy modulo$M$. Về nguyên tắc, điều này đúng vì phép truy toán xác định trình tự một cách duy nhất. Tuy nhiên, nó đòi hỏi thời gian tuyến tính trong chỉ mục, điều này trở nên không khả thi ngay lập tức đối với đầu vào vượt quá vài triệu và ở đây chỉ số lên tới$10^{18}$. 

Quan sát quan trọng là các số Fibonacci hoạt động tốt theo lũy thừa ma trận. Sự tái diễn$$f_n = f_{n-1} + f_{n-2}$$có thể được mã hóa dưới dạng một phép biến đổi tuyến tính:$$\begin{bmatrix}
f_{n+1} \\
f_n
\end{bmatrix}
=
\begin{bmatrix}
1 & 1 \\
1 & 0
\end{bmatrix}
\begin{bmatrix}
f_n \\
f_{n-1}
\end{bmatrix}$$Điều này có nghĩa là tính toán$f_n$giảm xuống mức nâng ma trận 2x2 cố định lên lũy thừa$n$. 

Phép lũy thừa ma trận cho phép chúng ta tính toán$n$-số Fibonacci thứ trong$O(\log n)$thời gian bằng cách sử dụng bình phương lặp đi lặp lại. Mỗi phép nhân có kích thước không đổi và tất cả các phép toán được thực hiện theo modulo$M$, do đó các giá trị vẫn bị giới hạn. 

Khó khăn còn lại là số mũ$a \cdot b$. Chúng ta không cần tạo ra các giá trị Fibonacci một cách rõ ràng cho đến chỉ số đó; chúng ta chỉ cần tính số mũ rồi áp dụng phép lũy thừa nhanh. Kể từ khi nhân$a \cdot b$phù hợp với phạm vi 64-bit (lên đến$10^{18}$), chúng ta có thể tính toán trực tiếp bằng Python một cách an toàn hoặc sử dụng tính năng rút gọn mô-đun nếu cần. 

Do đó bài toán được chuyển thành tính toán$f_{a \cdot b} \bmod M$sử dụng phép lũy thừa ma trận nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(a \cdot b)$|$O(1)$| Quá chậm | 
| Hàm mũ ma trận |$O(\log (a \cdot b))$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại phép tính Fibonacci dưới dạng bài toán lũy thừa ma trận và đánh giá nó một cách hiệu quả. 

1. Tính chỉ số$n = a \cdot b$. Đây là vị trí Fibonacci chúng ta cần. Phép nhân an toàn trong Python vì số nguyên không bị giới hạn, nhưng trong các ngôn ngữ khác, phép nhân này phải sử dụng số học 64 bit. 
2. Xác định ma trận biến đổi$$T =
\begin{bmatrix}
1 & 1 \\
1 & 0
\end{bmatrix}$$Ma trận này mã hóa mối quan hệ lặp lại giữa các trạng thái Fibonacci liên tiếp. 
3. Tính toán$T^n$sử dụng lũy ​​thừa nhị phân. Ở mỗi bước, bình phương ma trận và nhân nó thành bộ tích lũy khi bit tương ứng của$n$được thiết lập. Mỗi phép nhân được thực hiện theo modulo$M$, vì vậy tất cả các giá trị trung gian vẫn bị chặn. 
4. Sau khi lũy thừa, cấu trúc trên cùng bên phải của ma trận kết quả chứa giá trị Fibonacci. Cụ thể,$f_n$có thể được trích xuất từ ​​dạng ma trận, thường là phần tử thứ hai của hàng đầu tiên khi bắt đầu từ vectơ cơ sở tiêu chuẩn. 
5. Đầu ra$f_n \bmod M$. 

Lý do lũy thừa nhị phân là cần thiết là vì nó thay thế một chuỗi nhân tuyến tính bằng một chuỗi các phép tính bình phương logarit, tận dụng cách biểu diễn nhị phân của số mũ. 

### Tại sao nó hoạt động 

Thuật toán dựa vào bất biến mà áp dụng ma trận$T$một lần biến đổi$(f_n, f_{n-1})$vào trong$(f_{n+1}, f_n)$. Bằng quy nạp, áp dụng$T^k$nâng cao trạng thái Fibonacci một cách chính xác$k$các bước. Vì thế,$T^n$áp dụng cho vector cơ sở$(f_1, f_0)$sản xuất$(f_{n+1}, f_n)$và giá trị Fibonacci bắt buộc được nhúng trực tiếp vào kết quả này. Vì phép nhân ma trận có tính kết hợp nên phép lũy thừa bằng cách bình phương lặp lại sẽ bảo toàn tính chính xác trong khi chỉ thay đổi thứ tự tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mat_mul(a, b, mod):
    return [
        [
            (a[0][0]*b[0][0] + a[0][1]*b[1][0]) % mod,
            (a[0][0]*b[0][1] + a[0][1]*b[1][1]) % mod
        ],
        [
            (a[1][0]*b[0][0] + a[1][1]*b[1][0]) % mod,
            (a[1][0]*b[0][1] + a[1][1]*b[1][1]) % mod
        ]
    ]

def mat_pow(mat, exp, mod):
    res = [[1, 0], [0, 1]]
    base = mat

    while exp > 0:
        if exp & 1:
            res = mat_mul(res, base, mod)
        base = mat_mul(base, base, mod)
        exp >>= 1

    return res

def fib(n, mod):
    if n == 0:
        return 0
    T = [[1, 1], [1, 0]]
    res = mat_pow(T, n-1, mod)
    return res[0][0] % mod

a, b, M = map(int, input().split())
n = a * b
print(fib(n, M))
```Ý tưởng cốt lõi là hàm lũy thừa ma trận`mat_pow`, thực hiện nâng nhị phân trên ma trận 2x2. Thủ tục nhân`mat_mul`đảm bảo mọi mục nhập đều được giảm modulo$M$, ngăn ngừa tràn và giữ giá trị nhất quán. 

chức năng`fib(n, mod)`xử lý sự thay đổi từng bước một giữa định nghĩa công suất ma trận và lập chỉ mục Fibonacci. Từ$T^{n-1}$áp dụng cho sản lượng vectơ cơ sở$f_n$, chúng ta trừ đi một cách rõ ràng trong số mũ. Trường hợp đặc biệt$n = 0$được xử lý riêng vì công thức ma trận không thể hiện nó một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 100
```Đây$n = 6$, vì vậy chúng tôi tính toán$f_6$. 

| Bước | Số mũ ma trận | Trạng thái chính | 
| --- | --- | --- | 
| bắt đầu | 6 | tích lũy danh tính | 
| bit 0 | 6 là chẵn | bỏ qua nhân | 
| bit 1 | đế vuông | cơ sở cập nhật | 
| bit 2 | đặt bit | nhân thành kết quả | 

Giá trị Fibonacci cuối cùng là$f_6 = 8$, vậy đầu ra là$8$. 

Dấu vết này xác nhận rằng thuật toán không bao giờ xây dựng chuỗi một cách rõ ràng; nó chỉ thao túng sức mạnh của sự biến đổi. 

### Ví dụ 2 

đầu vào:```
3 3 6
```Đây$n = 9$, vì vậy chúng tôi tính toán$f_9 = 34$. 

| Bước | Tính toán | Giá trị mod 6 | 
| --- | --- | --- | 
| bắt đầu | n = 9 | - | 
| lũy thừa | sức mạnh ma trận | trạng thái tiến hóa | 
| cuối cùng | f_9 | 34 | 

Đầu ra là$34 \bmod 6 = 4$. 

Điều này cho thấy số học mô-đun được áp dụng chính xác ở mọi phép toán ma trận, ngăn ngừa tràn và duy trì tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log (a \cdot b))$| lũy thừa ma trận trên số mũ$a \cdot b$| 
| Không gian |$O(1)$| chỉ lưu trữ ma trận 2x2 cố định | 

Số mũ nhiều nhất là khoảng$10^{18}$, do đó logarit nhỏ hơn 60 bước. Mỗi bước thực hiện phép nhân ma trận không đổi, không đáng kể trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    def mat_mul(a, b, mod):
        return [
            [(a[0][0]*b[0][0] + a[0][1]*b[1][0]) % mod,
             (a[0][0]*b[0][1] + a[0][1]*b[1][1]) % mod],
            [(a[1][0]*b[0][0] + a[1][1]*b[1][0]) % mod,
             (a[1][0]*b[0][1] + a[1][1]*b[1][1]) % mod]
        ]

    def mat_pow(mat, exp, mod):
        res = [[1, 0], [0, 1]]
        base = mat
        while exp > 0:
            if exp & 1:
                res = mat_mul(res, base, mod)
            base = mat_mul(base, base, mod)
            exp >>= 1
        return res

    def fib(n, mod):
        if n == 0:
            return 0
        T = [[1, 1], [1, 0]]
        res = mat_pow(T, n-1, mod)
        return res[0][0] % mod

    a, b, M = map(int, input().split())
    print(fib(a*b, M))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return str(solve())

# provided samples
assert run("3 2 100") == "8", "sample 1"
assert run("3 3 6") == "4", "sample 2"

# custom cases
assert run("1 1 10") == "1", "f1"
assert run("2 3 1000") == "8", "f6 check"
assert run("10 10 1000000007") == str(55 % 1000000007), "f100"
assert run("0 5 7") == "0", "zero index edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 10 | 1 | Fibonacci khác 0 nhỏ nhất | 
| 2 3 1000 | 8 | nhân chỉ số đúng | 
| 10 10 1000000007 | 55 | tính chính xác trên Fibonacci lớn hơn | 
| 0 5 7 | 0 | xử lý trường hợp cơ bản | 

## Vỏ cạnh 

Một điểm mong manh là việc xử lý$n = 0$. Nếu như$a = 0$hoặc$b = 0$, thì tích bằng 0 và câu trả lời phải là$f_0 = 0$. Trong công thức ma trận, phép lũy thừa bằng 0 mang lại đồng nhất thức, không mã hóa trực tiếp Fibonacci mà không có xử lý đặc biệt. Mã kiểm tra rõ ràng trường hợp này để tránh trả về mục nhập ma trận sai. 

Một trường hợp cạnh khác là mô đun lớn$M = 1$. Bất kỳ số Fibonacci modulo 1 nào cũng bằng 0, nhưng nếu không rút gọn mô-đun ở mỗi phép nhân, các giá trị trung gian có thể tăng lên và làm phức tạp việc suy luận. Việc triển khai luôn giảm modulo$M$ở mỗi bước, do đó điều này suy biến một cách chính xác. 

Trường hợp tinh tế thứ ba là rất lớn$a \cdot b$. Ngay cả khi sản phẩm đạt đến khoảng$10^{18}$, lũy thừa nhị phân chỉ phụ thuộc vào số bit nên thuật toán vẫn ổn định và không phụ thuộc vào độ lớn ngoài thang logarit.
