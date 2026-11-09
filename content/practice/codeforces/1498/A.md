---
title: "CF 1498A - Tổng GCD"
description: "Bài toán yêu cầu chúng ta tìm, với một số nguyên $n$, số nguyên nhỏ nhất $x$ lớn hơn hoặc bằng $n$ sao cho ước số chung lớn nhất của $x$ và tổng các chữ số của nó lớn hơn một."
date: "2026-06-10T21:35:39+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1498
codeforces_index: "A"
codeforces_contest_name: "CodeCraft-21 and Codeforces Round 711 (Div. 2)"
rating: 800
weight: 1498
solve_time_s: 123
verified: true
draft: false
---

[CF 1498A - Tổng GCD](https://codeforces.com/problemset/problem/1498/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tìm một số nguyên cho trước$n$, số nguyên nhỏ nhất$x$lớn hơn hoặc bằng$n$sao cho ước chung lớn nhất của$x$và tổng các chữ số của nó lớn hơn một. Nói cách khác, chúng tôi muốn$x \ge n$với$gcd(x, s(x)) > 1$, Ở đâu$s(x)$là tổng các chữ số của$x$. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra một số duy nhất$n$và đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm thỏa mãn điều kiện này. 

Những ràng buộc cho phép$n$lên đến$10^{18}$và lên đến$10^4$trường hợp thử nghiệm. Một cách tiếp cận ngây thơ lặp đi lặp lại từ$n$trở lên và tính toán$gcd(x, s(x))$đối với mỗi ứng viên có thể khả thi nếu mỗi lần kiểm tra diễn ra cực kỳ nhanh. Từ$10^{18}$rất lớn, chúng ta không thể có được giải pháp quét tuyến tính trên một dãy số lớn trong trường hợp xấu nhất. Mỗi lần kiểm tra bao gồm việc tính tổng các chữ số và sau đó là gcd, cả hai đều nhanh đối với một số. 

Các trường hợp cạnh bao gồm các số trong đó$n$bản thân nó đã thỏa mãn điều kiện, các số có tổng các chữ số là số nguyên tố hoặc các số ngay dưới ứng cử viên "tốt". Ví dụ, nếu$n = 75$, gcd của 75 và 12 là 3, nên đáp án chính là 75. Nếu như$n = 11$,$gcd(11,2) = 1$, vì vậy chúng ta cần chuyển đến 12, ở đó$gcd(12,3) = 3$. Việc thực hiện bất cẩn có thể bỏ qua việc kiểm tra$n$chính nó và luôn tăng trước, tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: bắt đầu từ$n$, tính tổng các chữ số, tính gcd với số đó và kiểm tra xem nó có lớn hơn 1 hay không. Nếu không, hãy tăng số và lặp lại. Điều này hoạt động vì mỗi số là độc lập và tính toán gcd rất nhanh. Tuy nhiên, trong trường hợp xấu nhất, chúng ta có thể cần kiểm tra nhiều số liên tiếp trước khi tìm ra số hợp lệ.$x$. Với những hạn chế, chúng ta có thể không may mắn với một vài số, nhưng trên thực tế, tổng các chữ số nhiều nhất là 162 (đối với các số có 18 chữ số), do đó, một số hầu như luôn có gcd lớn hơn 1 với tổng các chữ số của nó trong một vài bước. Điều này cho phép quét tuyến tính đơn giản trong thực tế. 

Điều quan trọng giúp giải quyết vấn đề là điều kiện gcd hiếm khi bị vi phạm đối với các số liên tiếp. Tổng các chữ số thay đổi chậm và nhiều số có chung ước số với tổng các chữ số của chúng. Vì vậy, chỉ cần tăng$n$cho đến khi điều kiện được thỏa mãn vừa đúng vừa hiệu quả với các giới hạn của bài toán. Không cần công thức toán học phức tạp hoặc tính toán trước vì quá trình quét tuyến tính chỉ yêu cầu một vài lần lặp cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu / Quét tuyến tính | O(18 * t) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Tối ưu | O(18 * t) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test, hãy đọc số nguyên$n$. 
3. Khởi tạo$x = n$như số ứng cử viên. 
4. Lặp lại các bước sau cho đến khi hợp lệ$x$được tìm thấy: 

1. Tính tổng các chữ số của$x$. Điều này được thực hiện bằng cách chuyển đổi$x$thành một chuỗi và tính tổng các giá trị nguyên của mỗi ký tự. 
2. Tính toán$gcd(x, \text{sum of digits})$. Tích hợp sẵn của Python`math.gcd`chức năng xử lý việc này một cách hiệu quả. 
3. Nếu gcd lớn hơn 1, xuất ra$x$và dừng vòng lặp cho trường hợp thử nghiệm này. 
4. Ngược lại thì tăng$x$bằng 1 và tiếp tục vòng lặp. 
5. Chuyển sang test case tiếp theo. 

Tại sao nó hoạt động: Tính bất biến của vòng lặp là với mỗi số được kiểm tra, nó thỏa mãn điều kiện gcd hoặc chúng ta tăng lên số nguyên tiếp theo. Bởi vì các số nguyên là rời rạc và tăng nghiêm ngặt nên số đầu tiên có`gcd(x, s(x)) > 1`được đảm bảo ở mức tối thiểu. Thuật toán luôn xem xét đầu vào$n$chính nó, vì vậy nó không bỏ qua các giải pháp hợp lệ. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    x = n
    while True:
        s = sum(int(d) for d in str(x))
        if math.gcd(x, s) > 1:
            print(x)
            break
        x += 1
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc$n$và bắt đầu kiểm tra từ$x = n$. Tổng các chữ số được tính bằng cách sử dụng biểu thức trình tạo lặp qua biểu diễn chuỗi của số. các`math.gcd`hàm tính toán gcd của hai số một cách hiệu quả. Nếu thỏa mãn điều kiện thì in$x$và dừng lại. Ngược lại, chúng ta tăng$x$và tiếp tục. Vòng lặp được đảm bảo kết thúc vì một giá trị hợp lệ$x$luôn tồn tại và được tìm thấy nhanh chóng. 

## Ví dụ đã hoạt động 

### Mẫu 1 Trace 

Đầu vào: 11 

| x | tổng_chữ số(x) | gcd(x, sum_digits) | Hành động | 
| --- | --- | --- | --- | 
| 11 | 2 | 1 | tăng | 
| 12 | 3 | 3 | in 12 | 

Đầu vào: 31 

| x | tổng_chữ số(x) | gcd(x, sum_digits) | Hành động | 
| --- | --- | --- | --- | 
| 31 | 4 | 1 | tăng | 
| 32 | 5 | 1 | tăng | 
| 33 | 6 | 3 | in 33 | 

Những dấu vết này cho thấy vòng lặp nhanh chóng tìm thấy giá trị tối thiểu$x$thỏa mãn điều kiện gcd, thường trong 1 đến 3 lần lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * L) | Mỗi trường hợp thử nghiệm tăng tối đa một vài lần; L là số chữ số trong n (<18), vì tính tổng các chữ số là O(L) và tính toán gcd là không đáng kể. | 
| Không gian | O(1) | Chỉ có một số biến số nguyên và bộ đếm vòng lặp được sử dụng, không phụ thuộc vào kích thước đầu vào. | 

Với những hạn chế$t \le 10^4$Và$n \le 10^{18}$, giải pháp vừa vặn thoải mái trong giới hạn 1 giây. Python xử lý các số nguyên lớn một cách hiệu quả cho các phép toán này. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        x = n
        while True:
            s = sum(int(d) for d in str(x))
            if math.gcd(x, s) > 1:
                print(x)
                break
            x += 1
    return output.getvalue().strip()

# provided samples
assert run("3\n11\n31\n75\n") == "12\n33\n75", "sample 1"

# custom cases
assert run("2\n1\n2\n") == "2\n2", "minimum values"
assert run("1\n10\n") == "12", "edge just below solution"
assert run("1\n99\n") == "99", "sum of digits gcd already >1"
assert run("1\n1000000000000000000\n") == "1000000000000000002", "large n case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 2 | 2, 2 | Đầu vào tối thiểu | 
| 10 | 12 | Xử lý tăng dần từng bước một | 
| 99 | 99 | Bản thân đầu vào thỏa mãn điều kiện gcd | 
| 10^18 | 10^18 + 2 | Đầu vào, hiệu suất rất lớn | 

## Vỏ cạnh 

Đối với đầu vào`n = 1`, thuật toán kiểm tra`gcd(1,1) = 1`, tăng lên 2 và tìm thấy`gcd(2,2) = 2 > 1`. Đầu ra đúng 2. Đối với`n = 10^18`, tổng các chữ số là 1,`gcd(10^18,1) = 1`, tăng lên 10^18 + 1 (`gcd(10^18 + 1,1) = 1`), thì 10^18 + 2,`gcd(10^18 + 2,2) = 2`, tạo ra chính xác 10^18 + 2. Thuật toán xử lý các ranh giới và giá trị lớn mà không bị tràn vì số nguyên Python có độ chính xác tùy ý. Nó cũng xác định chính xác các trường hợp đầu vào thỏa mãn điều kiện gcd, chẳng hạn như`n = 75`, ngay lập tức in đầu vào.
