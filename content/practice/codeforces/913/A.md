---
title: "CF 913A - Hàm mũ mô đun"
description: "Bài toán yêu cầu chúng ta tính số dư khi chia một số nguyên m cho $2^n$, trong đó n và m là các số nguyên dương. Nói cách khác, chúng ta được cho biết kích thước của mô đun lũy thừa hai và số bị chia, và chúng ta phải tìm số còn lại sau khi chia số bị chia cho mô đun đó."
date: "2026-06-13T01:04:43+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 913
codeforces_index: "A"
codeforces_contest_name: "Hello 2018"
rating: 900
weight: 913
solve_time_s: 376
verified: true
draft: false
---

[CF 913A - Hàm mũ mô-đun](https://codeforces.com/problemset/problem/913/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 6 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tính số dư khi chia một số nguyên _m_ cho$2^n$, trong đó _n_ và _m_ là các số nguyên dương. Nói cách khác, chúng ta được cho biết kích thước của mô đun lũy thừa hai và số bị chia, và chúng ta phải tìm số còn lại sau khi chia số bị chia cho mô đun đó. Dữ liệu vào _n_ chỉ định số lần chúng ta nhân 2 với chính nó để thu được ước số, trong khi _m_ là số cần rút gọn theo modulo của ước số đó. Đầu ra là một số nguyên duy nhất từ ​​0 đến$2^n - 1$, đại diện cho phần còn lại. 

Những hạn chế ở mức độ vừa phải nhưng có những hàm ý tinh tế. Cả _n_ và _m_ đều có thể lớn tới 10^8. Tính toán trực tiếp$2^n$là không thực tế, bởi vì việc lưu trữ hoặc vận hành với số lượng khoảng$10^{30,000,000}$chữ số là không thể trong bộ nhớ. Điều này loại trừ bất kỳ cách tiếp cận nào xây dựng theo nghĩa đen$2^n$. Do đó, thuật toán phải dựa vào tính chất lũy thừa của hai, thay vì tính toán chúng một cách rõ ràng. 

Các trường hợp cạnh phát sinh khi _m_ nhỏ hơn$2^n$, trong trường hợp đó phần dư chỉ đơn giản là _m_ hoặc khi _m_ chia hết cho$2^n$, mang lại số dư bằng 0. Ví dụ: nếu _n_ = 5 và _m_ = 32,$2^5 = 32$và phần còn lại là 0. Việc thực hiện bất cẩn cố gắng tính toán$2^n$theo nghĩa đen sẽ thất bại hoặc tràn trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng tính toán$2^n$và sau đó thực hiện _m_ modulo$2^n$. Về nguyên tắc thì điều này đúng, nhưng với _n_ lên tới 10^8 thì con số$2^n$có kích thước lớn về mặt thiên văn và không thể biểu diễn dưới dạng số nguyên tiêu chuẩn. Cố gắng xây dựng nó một cách trực tiếp sẽ đòi hỏi thời gian và bộ nhớ theo cấp số nhân, vượt xa giới hạn bài toán. 

Cái nhìn sâu sắc quan trọng là$2^n$có cách biểu diễn nhị phân rất đơn giản: đó là số 1 theo sau là _n_ số 0 trong hệ nhị phân. Đang tính toán _m_ modulo$2^n$tương đương với việc lấy _n_ bit cuối cùng của _m_. Trong thực tế, đây chính xác là những gì thao tác bitwise AND với$2^n - 1$đạt được, bởi vì$2^n - 1$ở dạng nhị phân là _n_ đơn vị. Quan sát này làm giảm vấn đề thành một thao tác nhanh duy nhất có thể hoạt động ngay cả đối với các ràng buộc lớn nhất và nó tránh được việc xây dựng các số lượng lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Quá chậm, không thể với n lớn | 
| Modulo theo bit | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các số nguyên _n_ và _m_. Chúng chỉ định số mũ của 2 và số cần giảm tương ứng. 
2. Tính toán$mask = 2^n - 1$. Điều này tạo ra một số có _n_ số nhị phân. Lý do là bất kỳ số nào theo modulo lũy thừa hai đều tương đương với _n_ bit cuối cùng của nó mà mặt nạ này tách biệt. 
3. Tính số dư là$result = m \& mask$. Phép toán AND theo bit giữ chính xác _n_ bit cuối cùng của _m_, loại bỏ các bit cao hơn. Hoạt động này trực tiếp tạo ra$m \bmod 2^n$không có bất kỳ phép tính số lớn nào. 
4. In kết quả. Điều này hoàn thành việc tính toán một cách hiệu quả. 

Tại sao nó hoạt động: bất biến là với mọi số nguyên _m_, phần dư của phép chia cho$2^n$chỉ phụ thuộc vào _n_ bit thấp nhất của _m_. Phép toán AND với$2^n - 1$bảo toàn các bit đó trong khi loại bỏ các bit bậc cao hơn, không ảnh hưởng đến phần còn lại. Điều này đảm bảo tính chính xác bất kể _m_ hay _n_ lớn đến mức nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
m = int(input())

# 2^n - 1 is a mask with n ones in binary
mask = (1 << n) - 1

# remainder of m divided by 2^n
result = m & mask

print(result)
```Mã đọc đầu vào bằng I/O nhanh, tính toán mặt nạ bằng cách dịch chuyển trái để tránh tính toán lũy thừa lớn một cách rõ ràng, sau đó áp dụng AND theo bit để trích xuất _n_ bit cuối cùng. sử dụng`1 << n`vừa nhanh vừa an toàn trong Python vì số nguyên Python không bị giới hạn. Bản in cuối cùng xuất ra phần còn lại theo yêu cầu. 

## Ví dụ đã hoạt động 

**Mẫu 1:** n = 4, m = 42 

| Bước | Giá trị | Giải thích | 
| --- | --- | --- | 
| n | 4 | đọc từ đầu vào | 
| m | 42 | đọc từ đầu vào | 
| mặt nạ | 15 |$2^4 - 1 = 16 - 1 = 15$| 
| kết quả | 10 | 42 & 15 = 101010 & 1111 = 1010 (nhị phân) = 10 | 

Điều này cho thấy 4 bit cuối cùng của 42 là 1010, phần còn lại là 10. 

**Mẫu 2:** n = 1, m = 58 

| Bước | Giá trị | Giải thích | 
| --- | --- | --- | 
| n | 1 | đọc từ đầu vào | 
| m | 58 | đọc từ đầu vào | 
| mặt nạ | 1 |$2^1 - 1 = 1$| 
| kết quả | 0 | 58 & 1 = 111010 & 1 = 0 | 

Điều này chứng tỏ trường hợp cạnh trong đó m chia hết cho$2^n$, cho số dư bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | thao tác bit AND đơn lẻ | 
| Không gian | O(1) | chỉ có ba số nguyên được lưu trữ (n, m, mặt nạ) | 

Độ phức tạp về thời gian không phụ thuộc vào độ lớn của _n_ hoặc _m_, do đó nó dễ dàng nằm gọn trong giới hạn 1 giây. Dung lượng sử dụng ở mức tối thiểu, nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    m = int(input())
    mask = (1 << n) - 1
    return str(m & mask)

# provided samples
assert run("4\n42\n") == "10", "sample 1"
assert run("1\n58\n") == "0", "sample 2"

# custom cases
assert run("5\n32\n") == "0", "m equals 2^n"
assert run("5\n31\n") == "31", "m just below 2^n"
assert run("8\n300\n") == "44", "medium n and m"
assert run("30\n1073741825\n") == "1", "large n with overflow-like value"
assert run("10\n1023\n") == "1023", "m exactly 2^n - 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5\n32\n | 0 | số dư bằng 0 khi m = 2^n | 
| 5\n31\n | 31 | số dư bằng m khi m < 2^n | 
| 8\n300\n | 44 | trường hợp điển hình, kiểm tra mặt nạ đúng | 
| 30\n1073741825\n | 1 | n lớn, kiểm tra tính đúng đắn với số lớn | 
| 10\n1023\n | 1023 | m = 2^n - trường hợp 1 cạnh | 

## Vỏ cạnh 

Khi _m_ nhỏ hơn$2^n$, thuật toán sẽ trả về chính xác _m_. Ví dụ: n = 5, m = 3 dẫn đến mặt nạ = 31 và 3 & 31 = 3. Khi _m_ bằng$2^n$, chẳng hạn như n = 5, m = 32, mặt nạ là 31 và 32 & 31 = 0. _n_ lớn chẳng hạn như 30 và m = 1073741825 vẫn hoạt động vì Python hỗ trợ các số nguyên chính xác tùy ý và bitwise AND tách biệt 30 bit cuối cùng chính xác. Những trường hợp này xác nhận thuật toán xử lý tất cả các ranh giới một cách chính xác.
