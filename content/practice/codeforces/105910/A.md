---
title: "CF 105910A - SCUPC"
description: "Chúng ta có ba chuỗi nhị phân có cùng độ dài. Tại mỗi vị trí, chúng ta xem xét ba bit và đếm vị trí đó nếu có ít nhất hai trong số các bit là 1. Chúng ta được phép chọn chính xác một trong ba chuỗi và xoay nó theo chu kỳ sang trái bất kỳ số lần nào."
date: "2026-06-25T14:03:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105910
codeforces_index: "A"
codeforces_contest_name: "The 23rd Sichuan University Programming Contest"
rating: 0
weight: 105910
solve_time_s: 62
verified: true
draft: false
---

[CF 105910A - SCUPC](https://codeforces.com/problemset/problem/105910/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba chuỗi nhị phân có cùng độ dài. Tại mỗi vị trí, chúng ta xem xét ba bit và đếm vị trí đó nếu có ít nhất hai bit`1`. 

Chúng ta được phép chọn chính xác một trong ba dây và xoay nó theo chu kỳ sang trái bao nhiêu lần cũng được. Hai dây còn lại vẫn cố định. Sau khi chọn chuỗi tốt nhất để xoay và số lượng xoay tốt nhất, chúng tôi muốn số lượng vị trí lớn nhất có thể có ít nhất hai trong số ba bit.`1`. Tuyên bố cuộc thi đưa ra nhiều trường hợp thử nghiệm và tổng của tất cả độ dài chuỗi nhiều nhất là$10^5$. 

Tổng chiều dài giới hạn là quan sát quan trọng. Bất kỳ giải pháp nào thử tất cả$n$quay và đánh giá tất cả$n$vị trí cho mỗi vòng quay sẽ yêu cầu$O(n^2)$hoạt động trên mỗi trường hợp thử nghiệm, nó sẽ trở nên quá lớn khi$n$đạt tới$10^5$. Chúng ta cần thứ gì đó xung quanh$O(n \log n)$. 

Một điểm tinh tế là việc xoay các chuỗi khác nhau sẽ tạo ra các vấn đề tối ưu hóa khác nhau. Một giải pháp chỉ xem xét việc xoay chuỗi thứ ba có thể bỏ lỡ sự tối ưu. 

Coi như:```
n = 3
s1 = 110
s2 = 001
s3 = 001
```Xoay`s3`không tương đương với việc quay`s1`. Câu trả lời tốt nhất phải được chọn trong cả ba lựa chọn. 

Một sai lầm dễ mắc phải khác là chỉ tối ưu hóa các phần chồng chéo theo cặp. Mục tiêu không phải là số vị trí mà cả ba chuỗi đều có`1`, nhưng số lượng vị trí có ít nhất hai chuỗi`1`. 

Ví dụ:```
s1 = 10
s2 = 10
s3 = 01
```Vị trí đầu tiên đã đóng góp vì hai chuỗi chứa`1`, mặc dù chuỗi thứ ba chứa`0`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp rất đơn giản. Chọn chuỗi để xoay. Hãy thử tất cả$n$những chuyển dịch mang tính chu kỳ. Đối với mỗi ca, quét tất cả$n$vị trí và đếm xem có bao nhiêu vị trí chứa ít nhất hai vị trí. 

Phương pháp brute-force là chính xác vì nó kiểm tra rõ ràng mọi cấu hình hợp lệ. Thời gian chạy của nó là$O(3n^2)$, điều đó có hiệu quả$O(n^2)$. Với$n = 10^5$, điều đó có nghĩa là xung quanh$10^{10}$hoạt động vượt xa giới hạn. 

Để tìm ra cách tiếp cận nhanh hơn, chúng ta cần viết lại hàm tính điểm. 

Giả sử chúng ta xoay chuỗi thứ ba. Cho phép$$a_i=s_{1,i}, \quad b_i=s_{2,i}, \quad c_i=s_{3,i}.$$Đối với các giá trị nhị phân, chỉ báo cho thấy ít nhất hai bit bằng`1`có thể được viết là$$[a_i+b_i+c_i\ge2]
=
a_ib_i+a_ic_i+b_ic_i-2a_ib_ic_i.$$Tổng hợp tất cả các vị trí cho$$\text{score}
=
\sum a_ib_i
+
\sum a_ic_i
+
\sum b_ic_i
-
2\sum a_ib_ic_i.$$Số hạng đầu tiên không phụ thuộc vào phép quay. 

Bây giờ xác định$$d_i=a_i+b_i-2a_ib_i.$$Kiểm tra tất cả bốn khả năng của$(a_i,b_i)$: 

|$a_i$|$b_i$|$d_i$| 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 0 | 1 | 1 | 
| 1 | 0 | 1 | 
| 1 | 1 | 0 | 

Vì thế$d_i = a_i \oplus b_i$. 

Điểm số trở thành$$\sum a_ib_i + \sum d_i \cdot c_i.$$Sau khi quay, chỉ có số hạng thứ hai thay đổi. Vấn đề giảm xuống: 

Tìm sự chồng chéo theo chu kỳ tối đa giữa một chuỗi nhị phân$d=a\oplus b$và chuỗi quay$c$. 

Đây là một vấn đề tương quan chéo theo chu kỳ cổ điển. Tất cả các giá trị dịch chuyển có thể được tính toán đồng thời bằng một phép chập FFT. 

Chúng tôi lặp lại phép tính tương tự cho ba lựa chọn có thể có của chuỗi được xoay và lấy câu trả lời tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| FFT tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính giá trị khi xoay chuỗi thứ ba 

1. Hãy để`a`,`b`, Và`c`là ba mảng nhị phân. 
2. Tính toán$$\text{base}=\sum a_i b_i.$$Phần này không bao giờ thay đổi khi quay`c`. 
3. Tính toán$$d_i=a_i\oplus b_i.$$Điểm cho bất kỳ ca nào sẽ trở thành$$\text{base} + \sum d_i \cdot c_{i+\text{shift}}.$$4. Xây dựng`cc = c + c`, mảng có chiều dài gấp đôi`2n`. 

Mỗi sự dịch chuyển có tính chu kỳ của`c`xuất hiện dưới dạng một chiều dài liền kề-`n`đoạn bên trong`cc`. 
5. Đảo ngược`cc`và kết hợp nó với`d`sử dụng FFT. 
6. Giải nén$n$các giá trị tương quan tương ứng với$n$những chuyển dịch mang tính chu kỳ. 
7. Hãy để`best_corr`là mối tương quan được trích xuất tối đa. 
8. Điểm xoay vòng tốt nhất`c`là$$\text{base} + \text{best\_corr}.$$### Xem xét mọi lựa chọn 

1. Xoay`s1`, giữ`s2`Và`s3`đã sửa. 
2. Xoay`s2`, giữ`s1`Và`s3`đã sửa. 
3. Xoay`s3`, giữ`s1`Và`s2`đã sửa. 
4. Xuất ra kết quả tối đa của ba kết quả. 

### Tại sao nó hoạt động 

Đối với một lựa chọn cố định của chuỗi xoay, danh tính$$[a+b+c\ge2]
=
ab+ac+bc-2abc$$chính xác với mọi bộ ba nhị phân. Sau khi tập hợp lại các thuật ngữ, toàn bộ số điểm sẽ trở thành$$\sum ab + \sum (a\oplus b)c.$$Tổng đầu tiên là không đổi. Tổng thứ hai chính xác là mối tương quan theo chu kỳ giữa hai mảng nhị phân. FFT tính toán đồng thời mối tương quan của mọi ca, do đó mối tương quan lớn nhất tương ứng với vòng quay tốt nhất. Việc đánh giá cả ba lựa chọn của chuỗi xoay đảm bảo tìm thấy mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def fft(a, invert):
    n = len(a)
    j = 0

    for i in range(1, n):
        bit = n >> 1
        while j & bit:
            j ^= bit
            bit >>= 1
        j ^= bit

        if i < j:
            a[i], a[j] = a[j], a[i]

    length = 2
    while length <= n:
        ang = 2.0 * math.pi / length
        if not invert:
            ang = -ang

        wlen = complex(math.cos(ang), math.sin(ang))

        for i in range(0, n, length):
            w = 1 + 0j
            half = length // 2

            for j in range(i, i + half):
                u = a[j]
                v = a[j + half] * w

                a[j] = u + v
                a[j + half] = u - v

                w *= wlen

        length <<= 1

    if invert:
        for i in range(n):
            a[i] /= n

def convolution(a, b):
    n = 1
    need = len(a) + len(b) - 1

    while n < need:
        n <<= 1

    fa = [complex(x, 0) for x in a] + [0j] * (n - len(a))
    fb = [complex(x, 0) for x in b] + [0j] * (n - len(b))

    fft(fa, False)
    fft(fb, False)

    for i in range(n):
        fa[i] *= fb[i]

    fft(fa, True)

    return [int(round(x.real)) for x in fa[:need]]

def solve_one(x, y, rot):
    n = len(x)

    base = 0
    d = [0] * n

    for i in range(n):
        a = x[i]
        b = y[i]

        if a and b:
            base += 1

        d[i] = a ^ b

    doubled = rot + rot

    conv = convolution(d, doubled[::-1])

    best = 0

    for shift in range(n):
        idx = n - 1 + (n - 1 - shift)
        best = max(best, conv[idx])

    return base + best

def main():
    t = int(input())

    ans = []

    for _ in range(t):
        n = int(input())

        s1 = [int(c) for c in input().strip()]
        s2 = [int(c) for c in input().strip()]
        s3 = [int(c) for c in input().strip()]

        res = 0

        res = max(res, solve_one(s2, s3, s1))
        res = max(res, solve_one(s1, s3, s2))
        res = max(res, solve_one(s1, s2, s3))

        ans.append(str(res))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    main()
```chức năng`solve_one`giả sử một chuỗi cụ thể được phép xoay. Hai chuỗi còn lại được chuyển thành phần không đổi và mảng XOR. Sau đó, vấn đề trở thành một truy vấn tương quan tuần hoàn. 

Mảng nhân đôi`rot + rot`là thủ thuật tiêu chuẩn cho sự thay đổi theo chu kỳ. Mỗi vòng quay xuất hiện dưới dạng chiều dài-`n`cửa sổ bên trong chuỗi nhân đôi này. 

Tích chập sử dụng FFT để tính toán tất cả các mối tương quan cùng một lúc. Vì FFT hoạt động với các giá trị dấu phẩy động nên hệ số kết quả được làm tròn đến số nguyên gần nhất. 

Việc trích chỉ số là phần dễ gây ra sai sót nhất. Sự tích chập của`d`Và`reverse(rot + rot)`tạo ra tất cả các sự sắp xếp. hệ số$$n-1+(n-1-\text{shift})$$tương ứng chính xác với sự dịch chuyển theo chu kỳ mà chúng ta cần. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 10
s1 = 0000001000
s2 = 0000000110
s3 = 0000000001
```Quay`s3`. 

| Vị trí | s1 | s2 | XOR(s1,s2) | 
| --- | --- | --- | --- | 
| 6 | 1 | 0 | 1 | 
| 7 | 0 | 1 | 1 | 
| 8 | 0 | 1 | 1 | 

Mảng XOR chứa ba mảng bị cô lập. Xoay`s3`có thể căn chỉnh đơn của nó`1`với chính xác một trong số họ. 

| Thay đổi | Tương quan | 
| --- | --- | 
| ca tốt nhất | 1 |`base = 0`, vì vậy câu trả lời trở thành`1`. 

Ví dụ này cho thấy rằng khi chỉ có một chuỗi đóng góp một`1`, kết quả tốt nhất có thể được xác định hoàn toàn bởi các vị trí XOR. 

### Ví dụ 2```
n = 10
s1 = 0000001000
s2 = 0000010000
s3 = 0000001100
```Để lựa chọn quay`s3`: 

| Vị trí | s1 | s2 | XOR | 
| --- | --- | --- | --- | 
| 5 | 0 | 1 | 1 | 
| 6 | 1 | 0 | 1 | 

| Thay đổi | Tương quan | 
| --- | --- | 
| ca tốt nhất | 2 |`base = 0`, vậy câu trả lời là`2`. 

Điều này chứng tỏ rằng FFT đang tìm kiếm sự chồng chéo tối đa giữa mẫu XOR và chuỗi được xoay. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Ba mối tương quan dựa trên FFT cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(n)$| Mảng FFT và bộ đệm tạm thời | 

Vì tổng của tất cả các độ dài chuỗi nhiều nhất là$10^5$, tổng khối lượng công việc FFT vẫn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from solution import main

    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    main()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided samples
assert run(
"""3
10
0000001000
0000000110
0000000001
10
0000001000
0000010000
0000001100
10
0000000111
0000000011
0001000000
"""
) == "1\n2\n3"

# minimum size
assert run(
"""1
1
0
0
0
"""
) == "0"

# all equal
assert run(
"""1
5
11111
11111
11111
"""
) == "5"

# single useful rotation
assert run(
"""1
4
1000
0000
0001
"""
) == "1"

# boundary alignment
assert run(
"""1
4
1000
0100
0010
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vị trí duy nhất, tất cả số không | 0 | Kích thước tối thiểu | 
| Ba chuỗi tất cả một | 5 | Đã tối ưu | 
| Một trận đấu luân phiên | 1 | Xử lý ca theo chu kỳ | 
| Những người gần đầu đối diện | 1 | Tính chính xác bao quanh | 

## Vỏ cạnh 

Hãy xem xét:```
n = 1
s1 = 0
s2 = 0
s3 = 0
```Không có vòng quay thay đổi bất cứ điều gì. Mảng XOR là`[0]`, mối tương quan là`0`, và câu trả lời vẫn là`0`. 

Coi như:```
n = 4
s1 = 1000
s2 = 0000
s3 = 0001
```Mảng XOR là`[1,0,0,0]`. Xoay`s3`bằng một bước di chuyển nó`1`vào vị trí đầu tiên, tạo ra mối tương quan`1`. Thuật toán tìm thấy điều này thông qua mối tương quan theo chu kỳ được trích xuất từ ​​tích chập. 

Coi như:```
n = 5
s1 = 11111
s2 = 11111
s3 = 00000
```Đây`base = 5`bởi vì mọi vị trí đều đã có hai vị trí. Mảng XOR toàn là số 0 nên mọi mối tương quan đều bằng 0. Thuật toán trả về`5`, nhận ra chính xác rằng không có vòng quay nào có thể cải thiện hoặc làm hỏng điểm số. 

Các trường hợp này bao gồm độ dài tối thiểu, căn chỉnh bao quanh và các tình huống trong đó câu trả lời đã được cố định trước khi áp dụng bất kỳ phép xoay nào.
