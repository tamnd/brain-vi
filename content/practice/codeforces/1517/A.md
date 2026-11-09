---
title: "CF 1517A - Tổng của năm 2050"
description: "Chúng ta được yêu cầu lấy một số $n$ và biểu thị nó dưới dạng tổng của các số là bội số của 2050 với lũy thừa mười. Cụ thể, những con số chúng ta có thể sử dụng là 2050, 20500, 205000, 2050000, v.v. Mỗi số trong dãy này chính xác bằng 2050 nhân với lũy thừa mười."
date: "2026-06-10T18:17:24+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1517
codeforces_index: "A"
codeforces_contest_name: "Contest 2050 and Codeforces Round 718 (Div. 1 + Div. 2)"
rating: 800
weight: 1517
solve_time_s: 126
verified: true
draft: false
---

[CF 1517A - Tổng của năm 2050](https://codeforces.com/problemset/problem/1517/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu lấy một số$n$và biểu thị nó dưới dạng tổng của các số là bội số của năm 2050 theo lũy thừa mười. Cụ thể, những con số chúng ta có thể sử dụng là 2050, 20500, 205000, 2050000, v.v. Mỗi số trong dãy này chính xác bằng 2050 nhân với lũy thừa mười. Nhiệm vụ là tìm số lượng nhỏ nhất các số có tổng bằng$n$hoặc báo cáo rằng điều đó là không thể. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một số duy nhất$n$.$n$có thể lớn như$10^{18}$, loại trừ mọi sự liệt kê mạnh mẽ của tất cả các kết hợp có thể. Giới hạn này cho chúng ta biết rằng chúng ta cần một phương pháp hoạt động hiệu quả với các số nguyên lớn, lý tưởng nhất là theo thời gian logarit hoặc tuyến tính so với số chữ số của$n$. 

Trường hợp khó phát hiện xảy ra khi$n$hoàn toàn không chia hết cho năm 2050. Ví dụ,$n = 205$không thể viết dưới dạng tổng của 2050 số. Việc triển khai bất cẩn luôn cố gắng chia hoặc trừ sẽ cho kết quả không nguyên hoặc cố gắng xây dựng một tổng không tồn tại một cách không chính xác. Một trường hợp cạnh khác là khi$n$chính xác là một số 2050 có lũy thừa lớn bằng 10, chẳng hạn như$2050 \cdot 10^{15}$. Giải pháp của chúng tôi phải xử lý những vấn đề này mà không cần lặp lại tất cả các bội số nhỏ hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là tạo ra tất cả các số 2050 lên tới$n$và thử tất cả các kết hợp. Đối với mỗi số 2050, bạn có thể trừ nó nhiều lần khỏi$n$cho đến khi bạn đạt đến số không. Đây thực chất là một cách tiếp cận tham lam vũ phu. Nó hoạt động cho nhỏ$n$nhưng không thành công đối với đầu vào lớn vì số lượng số 2050 có thể có tỷ lệ với số chữ số của$n$và các vòng trừ có thể đạt tới$10^{18}/2050 \approx 5 \cdot 10^{14}$hoạt động trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là mọi giá trị hợp lệ$n$phải chia hết cho 2050, vì mọi số được phép đều là bội số của 2050. Một khi chúng ta chia$n$đến năm 2050, vấn đề giảm xuống còn việc biểu diễn số kết quả$m = n / 2050$dưới dạng tổng lũy ​​thừa của 10. Vì lũy thừa của 10 chỉ có một chữ số khác 0 nên số số hạng tối thiểu chỉ đơn giản là tổng các chữ số của$m$. Điều này tránh lặp lại mọi kết hợp có thể và giảm vấn đề thành phép tính tổng chữ số đơn giản. Cách tiếp cận này có hiệu quả vì bất kỳ số nào$m$trong cơ sở 10 có thể được biểu diễn duy nhất dưới dạng tổng lũy thừa của 10 nhân với các hệ số có một chữ số và mỗi hệ số tương ứng với số đếm của một số 2050 có lũy thừa cụ thể là 10. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n / 2050) | O(1) | Quá chậm đối với n lớn | 
| Tối ưu | O(log10 n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$T$. 
2. Với mỗi test, hãy đọc số$n$. 
3. Kiểm tra xem$n$chia hết cho 2050. Nếu không, hãy in -1 và chuyển sang trường hợp kiểm thử tiếp theo. Bước này ngay lập tức lọc các trường hợp không thể. 
4. Chia$n$đến năm 2050 có được$m$. Điều này biến vấn đề thành đếm chữ số. 
5. Khởi tạo bộ đếm số lượng 2050 số cần thiết. 
6. Trong khi$m > 0$, trích chữ số cuối cùng của$m$sử dụng modulo 10, thêm nó vào bộ đếm và chia số nguyên$m$bằng 10. Điều này tính tổng các chữ số của$m$. 
7. In bộ đếm. Đây là số lượng tối thiểu của 2050 số được yêu cầu. 

Tính chính xác được đảm bảo vì mỗi chữ số của$m$tương ứng với số đếm của một số năm 2050 nhân với lũy thừa cụ thể của 10. Tính tổng các chữ số sẽ cho số số hạng tối thiểu vì mỗi chữ số có thể được nhận ra trực tiếp với số 2050 tương ứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = int(input())
for _ in range(T):
    n = int(input())
    if n % 2050 != 0:
        print(-1)
        continue
    m = n // 2050
    count = 0
    while m > 0:
        count += m % 10
        m //= 10
    print(count)
```Đầu tiên chúng ta đọc số lượng ca kiểm thử, sau đó lặp qua từng ca kiểm thử.$n$. Việc kiểm tra tính chia hết sẽ ngăn chặn việc tính toán không cần thiết đối với các trường hợp không thể thực hiện được. Chia cho năm 2050 chuyển bài toán thành bài toán tổng các chữ số. Sau đó chúng ta lặp qua các chữ số của$m$, tính tổng chúng để có được số đếm cuối cùng. Việc sử dụng phép chia số nguyên đảm bảo rằng không phát sinh vấn đề chính xác về dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 4100 

| Bước | n | n % 2050 | m | đếm | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 4100 | 0 | 2 | 0 | 
| Vòng 1 | 4100 | 0 | 2 | 2 | 
| Vòng 2 | 4100 | 0 | 0 | 2 | 

Tổng các chữ số của 2 cho ra 2, tương ứng với 4100 = 2050 + 2050. 

### Ví dụ 2: n = 25308639900 

| Bước | n | n % 2050 | m | đếm | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 25308639900 | 0 | 12345678 | 0 | 
| Vòng 1 | 12345678 | 8 | 1234567 | 8 | 
| Vòng 2 | 1234567 | 7 | 123456 | 15 | 
| Vòng 3 | 123456 | 6 | 12345 | 21 | 
| Vòng 4 | 12345 | 5 | 1234 | 26 | 
| Vòng 5 | 1234 | 4 | 123 | 30 | 
| Vòng 6 | 123 | 3 | 12 | 33 | 
| Vòng 7 | 12 | 2 | 1 | 35 | 
| Vòng 8 | 1 | 1 | 0 | 36 | 

Tổng các chữ số cho 36, là số tối thiểu của 2050 số cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log10 n) | Chúng ta chia cho 2050 một lần rồi tính tổng các chữ số của số thu được, việc này tốn thời gian tỷ lệ thuận với số chữ số. | 
| Không gian | O(1) | Chỉ sử dụng một vài biến số nguyên, không có cấu trúc dữ liệu bổ sung. | 

Được cho$n \le 10^{18}$, số chữ số tối đa là 18, do đó thuật toán của chúng tôi thực hiện tối đa 18 thao tác cho mỗi trường hợp thử nghiệm, dễ dàng thực hiện trong 1 giây cho tối đa 1000 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    T = int(input())
    for _ in range(T):
        n = int(input())
        if n % 2050 != 0:
            output.append("-1")
            continue
        m = n // 2050
        count = 0
        while m > 0:
            count += m % 10
            m //= 10
        output.append(str(count))
    return "\n".join(output)

# provided samples
assert run("6\n205\n2050\n4100\n20500\n22550\n25308639900\n") == "-1\n1\n2\n1\n2\n36", "sample 1"

# custom cases
assert run("3\n2050\n20500\n205000\n") == "1\n1\n1", "single terms"
assert run("1\n2049\n") == "-1", "not divisible"
assert run("1\n2050000000000000000\n") == "1", "large power of ten"
assert run("1\n6150\n") == "3", "sum of three 2050s"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2050, 20500, 205000 | 1, 1, 1 | Biểu diễn tối thiểu với một số 2050 | 
| 2049 | -1 | Kiểm tra tính chia hết | 
| 20500000000000000000 | 1 | Xử lý số lượng rất lớn | 
| 6150 | 3 | Đếm đúng nhiều số 2050 giống nhau | 

## Vỏ cạnh 

Nếu$n$nhỏ hơn 2050, việc kiểm tra tính chia hết đảm bảo rằng thuật toán đưa ra -1. Vì$n$đó chính xác là số 2050 nhân với lũy thừa mười, tổng các chữ số của$m$sẽ là 1, đưa ra số lượng tối thiểu chính xác. Đối với rất lớn$n$, sử dụng phép chia số nguyên và modulo sẽ tránh mọi lỗi dấu phẩy động và đảm bảo tính chính xác. Thuật toán xử lý chính xác các số có nhiều chữ số trong$m$, tính tổng chúng để tạo ra số hạng tối thiểu.
