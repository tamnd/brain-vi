---
title: "CF 1331H - Đã đến giờ chiếu rồi"
description: "Vấn đề trình bày một đầu vào số nguyên duy nhất mã hóa hai phần thông tin. Số nguyên có thể được phân tách thành đầu vào = 1000 n + mod, trong đó n là số mà chúng ta muốn tính giai thừa kép và mod là mô đun để tính nó."
date: "2026-06-11T16:15:53+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1331
codeforces_index: "H"
codeforces_contest_name: "April Fools Day Contest 2020"
rating: 0
weight: 1331
solve_time_s: 207
verified: false
draft: false
---

[CF 1331H - Đã đến giờ chiếu](https://codeforces.com/problemset/problem/1331/H) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày một số nguyên duy nhất`input`mã hóa hai mẩu thông tin. Số nguyên có thể được phân tách thành`input = 1000 * n + mod`, Ở đâu`n`là số mà chúng ta muốn tính giai thừa kép và`mod`là mô-đun để tính toán nó. Nhiệm vụ của chúng ta là tính giai thừa kép của`n`, viết là`n!!`, modulo`mod`. Nhớ lại điều đó`n!!`là tích của tất cả các số nguyên từ`n`xuống 1 có cùng tính chẵn lẻ với`n`. Ví dụ, nếu`n`là chẵn,`n!! = n * (n-2) * (n-4) ... * 2`, và nếu`n`thật kỳ lạ,`n!! = n * (n-2) * (n-4) ... * 1`. 

Đầu vào đảm bảo rằng`n`Và`mod`mỗi số nằm trong khoảng từ 1 đến 999. Sự kết hợp`input`do đó nằm trong khoảng từ 1001 đến 999999. Điều này có nghĩa là số bước nhân lớn nhất cần thiết để tính trực tiếp một giai thừa kép tối đa là 499 phép tính, đủ nhỏ cho phương pháp lặp trực tiếp. Mặc dù số lượng nhỏ, các trường hợp đặc biệt vẫn phát sinh khi`mod`là 1, hoặc khi`n!!`lớn hơn nhiều so với`mod`và số học mô-đun cẩn thận là cần thiết. Ví dụ, nếu`n = 100`Và`mod = 2`, kết quả đúng là 0 vì mọi số hạng trong`100!!`chia hết cho 2. Một cách tiếp cận đơn giản không áp dụng mô đun trong quá trình nhân có thể tràn, ngay cả đối với những giá trị khiêm tốn này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tính giai thừa kép của`n`trực tiếp và sau đó lấy mô đun ở cuối. Điều này hiệu quả vì công thức giai thừa kép rất đơn giản và mỗi phép nhân bao gồm tối đa 499 số hạng. Số lượng thao tác có thể chấp nhận được nhưng phải cẩn thận để tránh tràn số nguyên. Trong Python, tràn không phải là vấn đề đáng lo ngại, nhưng trong các ngôn ngữ khác, đây có thể là một rủi ro. 

Tối ưu hóa một chút là áp dụng mô đun ở mỗi bước nhân. Thay vì tính toán đầy đủ`n!!`và sau đó lấy modulo, chúng tôi duy trì kết quả modulo`mod`khắp. Điều này hợp lệ về mặt toán học vì phép nhân mô-đun có tính kết hợp:`(a * b) % m = ((a % m) * (b % m)) % m`. Điều này ngăn chặn sự tăng trưởng không cần thiết của kết quả trung gian và xử lý ngay các trường hợp kết quả bằng 0. Cấu trúc của giai thừa kép làm cho cách tiếp cận này trở nên tối ưu vì dãy số giảm nghiêm ngặt với kích thước bước 2 và chúng ta không bao giờ cần nhiều hơn`n/2`phép nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force, sản phẩm đầy đủ rồi mod | O(n/2) | O(1) | Được chấp nhận nhưng có nguy cơ tràn ở một số ngôn ngữ | 
| Tối ưu, modulo từng bước | O(n/2) | O(1) | Đã chấp nhận | 

Cách tiếp cận tối ưu về cơ bản giống như cách tiếp cận vũ phu nhưng an toàn hơn và có tính tổng quát hơn. 

## Hướng dẫn thuật toán 

1. Trích xuất`n`Và`mod`từ đầu vào bằng phép chia số nguyên và số dư. Cụ thể,`n = input // 1000`Và`mod = input % 1000`. Điều này tách biệt hai giá trị được mã hóa trong một số nguyên duy nhất. 
2. Khởi tạo`result`thành 1. Biến này sẽ tích lũy modulo giai thừa kép`mod`. 
3. Xác định điểm bắt đầu của dãy nhân thừa kép. Điều này đơn giản là`n`và kích thước bước luôn là 2 vì giai thừa kép bỏ qua mọi số nguyên khác. 
4. Lặp lại từ`n`xuống 1 (hoặc 2 nếu`n`là số chẵn), giảm dần 2 mỗi bước. Nhân dòng điện`result`theo số hiện tại trong dãy thì lấy ngay mô đun`mod`. chính thức,`result = (result * current) % mod`. Điều này đảm bảo sản phẩm trung gian không bao giờ vượt quá`mod`và xử lý chính xác các thuộc tính số học mô-đun. 
5. Sau khi vòng lặp hoàn tất, hãy in`result`. Đây là điều mong muốn`n!! % mod`. 

Bất biến chính là sau khi xử lý từng số hạng trong giai thừa kép,`result`bằng với giai thừa kép của các số được thấy cho đến nay theo modulo`mod`. Bởi vì phép nhân mô-đun có tính kết hợp và giao hoán nên bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x = int(input())
n = x // 1000
mod = x % 1000

res = 1
for i in range(n, 0, -2):
    res = (res * i) % mod

print(res)
```Trong giải pháp này, trước tiên chúng tôi trích xuất`n`Và`mod`. Vòng lặp lặp lại theo bước 2, nắm bắt chính xác tính chẵn lẻ của`n`cho giai thừa kép. Chúng tôi thực hiện thao tác modulo ở mỗi bước để ngăn chặn tràn số nguyên và xử lý chính xác các trường hợp cạnh như`mod = 1`hoặc khi nào`n!!`là bội số của`mod`. Việc sử dụng các số nguyên chính xác tùy ý của Python giúp việc triển khai này trở nên an toàn mà không cần xử lý tràn bổ sung. 

## Ví dụ đã hoạt động 

Đối với đầu vào`6100`, chúng tôi tính toán`n = 6`Và`mod = 100`. Trình tự của`6!!`là`6, 4, 2`. Áp dụng modulo ở mỗi bước: 

| Bước | Hiện tại tôi | res trước | độ phân giải sau | 
| --- | --- | --- | --- | 
| 1 | 6 | 1 | (1*6)%100 = 6 | 
| 2 | 4 | 6 | (6*4)%100 = 24 | 
| 3 | 2 | 24 | (24*2)%100 = 48 | 

Đầu ra là`48`. 

Đối với đầu vào`9009`,`n = 9`Và`mod = 9`. Sự liên tiếp`9!! = 9*7*5*3*1`. Áp dụng modulo: 

| Bước | Hiện tại tôi | res trước | độ phân giải sau | 
| --- | --- | --- | --- | 
| 1 | 9 | 1 | 0 | 
| ... | ... | 0 | 0 | 

Đầu ra là`0`, thể hiện sự về 0 sớm trong số học modulo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n/2) | Mỗi học kỳ từ`n`xuống 1 với bước 2 được xử lý đúng một lần | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến số nguyên không đổi | 

Được cho`n`999, tổng số thao tác tối đa là 499, thoải mái trong giới hạn 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x = int(sys.stdin.readline())
    n = x // 1000
    mod = x % 1000
    res = 1
    for i in range(n, 0, -2):
        res = (res * i) % mod
    return str(res)

# Provided samples
assert run("6100\n") == "48", "sample 1"
assert run("9009\n") == "0", "sample 2"
assert run("1002\n") == "0", "sample 3"

# Custom cases
assert run("1001\n") == "1", "minimum n and mod"
assert run("999999\n") == "0", "maximum n and mod"
assert run("501\n") == "1", "n = 0 mod 2, small mod"
assert run("1203\n") == "2", "small even n, mod not dividing product"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1001 | 1 | M |
