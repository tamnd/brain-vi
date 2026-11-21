---
title: "CF 2205B - ​​Simons và Bánh để thành công"
description: "Chúng tôi được yêu cầu giúp Simons chia bánh cho những người bạn của anh ấy. Chính thức hơn, với một số lượng bạn bè $n$ nhất định, chúng ta cần tìm số nguyên dương nhỏ nhất $k$ sao cho $k^n$ chia hết cho $n$. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định số lượng bạn bè."
date: "2026-06-07T19:50:09+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2205
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1083 (Div. 2)"
rating: 800
weight: 2205
solve_time_s: 141
verified: true
draft: false
---

[CF 2205B - Simons và Bánh để thành công](https://codeforces.com/problemset/problem/2205/B) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu giúp Simons chia bánh cho những người bạn của anh ấy. Chính thức hơn, đối với một số lượng bạn bè nhất định$n$, ta cần tìm số nguyên dương nhỏ nhất$k$như vậy$k^n$chia hết cho$n$. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định số lượng bạn bè. Đầu ra cho mỗi trường hợp là tối thiểu$k$đó thỏa mãn điều kiện chia hết. 

Những ràng buộc cho phép$n$lớn như$10^9$và số lượng trường hợp thử nghiệm lên tới 100. Bất kỳ thuật toán nào cố gắng tính toán$k^n$trực tiếp cho tất cả$k$lên tới$n$sẽ là quá chậm, vì việc lũy thừa với số lượng lớn rất tốn kém và lặp đi lặp lại đến$10^9$là không thực tế. Do đó, thách thức là xác định một phương pháp hoạt động mà không cần đến sức mạnh cưỡng bức. 

Một cách tiếp cận ngây thơ sẽ kiểm tra$k=1,2,3,\dots$và kiểm tra xem$k^n \mod n = 0$. Đối với nhỏ$n$, điều này có hiệu quả, nhưng đối với$n\sim 10^9$, nó sẽ cần hàng tỷ lần lặp lại. Cũng,$1^n$luôn là 1, thất bại khi$n>1$, vì vậy đây là cạm bẫy phổ biến đối với việc triển khai bất cẩn. 

Một trường hợp tế nhị khác là khi$n$là số nguyên tố hoặc lũy thừa của số nguyên tố. Ví dụ,$n=8$yêu cầu$k=2$bởi vì$2^3 = 8$chia rẽ$2^8 = 256$. Hiểu sai cấu trúc của$n$thường dẫn đến việc đánh giá quá cao$k$. 

## Phương pháp tiếp cận 

Phương pháp vũ phu lặp đi lặp lại$k=1,2,3,\dots$và kiểm tra xem$n$chia rẽ$k^n$. Mỗi kiểm tra bao gồm lũy thừa mô-đun, đó là$O(\log n)$. Trong trường hợp xấu nhất,$k$có thể lớn như$n$, cho$O(n \log n)$. Vì$n \sim 10^9$, tốc độ này quá chậm. 

Cái nhìn sâu sắc quan trọng đến từ hệ số hóa. Cho phép$n$có hệ số nguyên tố$n = p_1^{a_1} p_2^{a_2} \dots p_m^{a_m}$. Để có$k^n$chia hết cho$n$, mỗi số nguyên tố$p_i$phải xuất hiện trong$k^n$có ít nhất số mũ$a_i$. Nói cách khác, tính đa dạng của$p_i$TRONG$k$ít nhất phải có$\lceil a_i / n \rceil = 1$bởi vì$a_i/n < 1$. Vì vậy, tối thiểu$k$thu được bằng cách nhân các thừa số nguyên tố riêng biệt của$n$. 

Điều này làm giảm vấn đề về việc phân tích thành thừa số nguyên tố của$n$, hiệu quả đối với các ràng buộc đã cho bởi vì$n \le 10^9$. Chúng ta có thể phân tích nhân tử bằng cách sử dụng phép chia thử lên đến$\sqrt{n}$, yêu cầu tối đa khoảng 31.622 lần lặp cho mỗi trường hợp thử nghiệm. Nhân các số nguyên tố phân biệt sẽ thu được giá trị nhỏ nhất$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(1) | Quá chậm | 
| Phân tích nhân tử & Tích số nguyên tố | O(√n) | O(√n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$. 
3. Khởi tạo một danh sách trống chứa thừa số nguyên tố. 
4. Lặp lại$d$từ 2 đến$\sqrt{n}$. Nếu như$d$chia rẽ$n$, thêm vào$d$vào danh sách các thừa số nguyên tố. Sau đó chia$n$qua$d$nhiều lần cho đến khi không còn chia hết được nữa. Điều này khai thác sức mạnh cao nhất của$d$TRONG$n$trong khi chỉ ghi lại số nguyên tố riêng biệt. 
5. Nếu sau vòng lặp$n>1$, bản thân nó là thừa số nguyên tố, hãy thêm nó vào danh sách. 
6. Tính toán$k$là tích của tất cả các thừa số nguyên tố phân biệt. 
7. In$k$. 

Tại sao nó hoạt động: Mỗi thừa số nguyên tố của$n$phải xuất hiện trong$k$, nếu không thì$k^n$không thể cung cấp đủ bội số của số nguyên tố đó để chia$n$. Bao gồm mỗi số nguyên tố riêng biệt chính xác một lần sẽ tạo ra giá trị tối thiểu$k$, vì việc thêm quyền hạn cao hơn sẽ làm tăng một cách không cần thiết$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def minimal_k(n):
    original_n = n
    k = 1
    for d in range(2, int(n**0.5)+1):
        if n % d == 0:
            k *= d
            while n % d == 0:
                n //= d
    if n > 1:
        k *= n
    return k

t = int(input())
for _ in range(t):
    n = int(input())
    print(minimal_k(n))
```Giải pháp xác định một chức năng`minimal_k`để nhân tố hóa$n$và tính tích của các số nguyên tố phân biệt. Vòng lặp bên ngoài đọc tất cả các trường hợp kiểm thử. sử dụng`int(n**0.5)`đảm bảo phép chia thử chỉ đi đến căn bậc hai. Vòng lặp bên trong phân chia$n$bởi nhân tử nhiều lần để loại bỏ mọi lũy thừa, tránh phép nhân trùng lặp. 

## Ví dụ đã hoạt động 

### Mẫu 1: n = 8 

| Bước | n còn lại | Đã tìm thấy yếu tố | k | 
| --- | --- | --- | --- | 
| 2 | 8 | 2 | 1*2 = 2 | 
| 2 | 4 | - | 2 | 
| 2 | 2 | - | 2 | 
| 2 | 1 | - | 2 | 
| Đã hoàn thành | 1 | - | 2 | 

Chúng tôi thấy điều đó`2^8 = 256`và 8 chia 256, xác nhận tính đúng đắn. 

### Mẫu 2: n = 12 

| Bước | n còn lại | Đã tìm thấy yếu tố | k | 
| --- | --- | --- | --- | 
| 2 | 12 | 2 | 2 | 
| 2 | 6 | - | 2 | 
| 2 | 3 | - | 2 | 
| 3 | 3 | 3 | 2*3 = 6 | 
| Đã hoàn thành | 1 | - | 6 |`6^12`chia hết cho 12 và 6 là nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t√n) | Mỗi trường hợp thử nghiệm phân tích nhân tử lên tới √n, t 100 | 
| Không gian | O(√n) | Lưu trữ ở hầu hết các số nguyên tố khác nhau lên tới √n | 

Để tối đa$n=10^9$Và$t=100$, tổng số thao tác ≈ 100 * 31.622 ≈ 3,16 triệu, vừa vặn thoải mái trong giới hạn 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    input = sys.stdin.readline

    def minimal_k(n):
        k = 1
        for d in range(2, int(n**0.5)+1):
            if n % d == 0:
                k *= d
                while n % d == 0:
                    n //= d
        if n > 1:
            k *= n
        return k

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        res.append(str(minimal_k(n)))
    return "\n".join(res)

# provided samples
assert run("4\n8\n12\n369\n55635800\n") == "2\n6\n123\n2090", "sample 1"

# custom cases
assert run("3\n2\n3\n5\n") == "2\n3\n5", "primes"
assert run("2\n4\n16\n") == "2\n2", "powers of two"
assert run("2\n18\n20\n") == "6\n10", "composite numbers with multiple primes"
assert run("1\n1000000000\n") == "10", "large n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2,3,5 | 2,3,5 | k tối thiểu cho số nguyên tố n | 
| 4,16 | 2,2 | quyền hạn của 2 được xử lý chính xác | 
| 18,20 | 6,10 | nhiều số nguyên tố kết hợp chính xác | 
| 1e9 | 10 | hiệu suất n lớn | 

## Vỏ cạnh 

cho$n=2$, thuật toán tìm 2 là thừa số nguyên tố duy nhất. Vì$n$xuất sắc,$k=n$. Đối với lũy thừa của một số nguyên tố như 16, thuật toán tìm thấy 2, số nguyên tố riêng biệt, đúng vì$2^{16}$chia hết cho 16. Trong mỗi trường hợp, phép nhân các số nguyên tố riêng biệt tạo ra giá trị nhỏ nhất$k$, chứng minh rằng cách tiếp cận dựa trên hệ số hóa xử lý chính xác tất cả các trường hợp cạnh không rõ ràng.
