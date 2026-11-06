---
title: "CF 130F - Phân tích thành thừa số nguyên tố"
description: "Nhiệm vụ là phân tách một số nguyên n cho trước thành các thừa số nguyên tố của nó và in chúng theo thứ tự không giảm, với mỗi số nguyên tố được lặp lại theo bội số của nó. Về cơ bản, nếu một số là tích của các số nguyên tố như $n = 2^2 cdot 3^1 cdot 5^2$ thì kết quả đầu ra sẽ là 2 2 3 5 5."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 130
codeforces_index: "F"
codeforces_contest_name: "Unknown Language Round 4"
rating: 1600
weight: 130
solve_time_s: 108
verified: true
draft: false
---

[CF 130F - Hệ số nguyên tố](https://codeforces.com/problemset/problem/130/F) 

**Đánh giá:** 1600 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là phân tách một số nguyên _n_ cho trước thành các thừa số nguyên tố của nó và in chúng theo thứ tự không giảm, với mỗi số nguyên tố được lặp lại theo bội số của nó. Về cơ bản, nếu một số là tích của các số nguyên tố như$n = 2^2 \cdot 3^1 \cdot 5^2$, đầu ra phải là`2 2 3 5 5`. Đầu vào là một số nguyên duy nhất từ ​​2 đến 250, đủ nhỏ để chúng ta không cần các thuật toán phân tích nhân tử nâng cao như Rho của Pollard hoặc sàng được tối ưu hóa cho các số rất lớn. 

Vì giới hạn trên chỉ là 250 nên chúng ta có thể thực hiện các phép tính tỉ lệ với$n$hoặc$\sqrt{n}$thoải mái trong giới hạn thời gian 2 giây. Các trường hợp cạnh chính phát sinh khi chính _n_ là số nguyên tố (ví dụ: 2, 3, 197), trong trường hợp đó việc phân tích nhân tử chỉ là`n`hoặc khi _n_ là bình phương hoàn hảo của số nguyên tố (ví dụ: 49 = 7*7), kiểm tra xem thuật toán có xử lý chính xác các yếu tố lặp lại hay không. Một cách tiếp cận đơn giản chia cho mọi số lên tới _n_ sẽ có tác dụng ở đây vì$n \le 250$, nhưng chúng ta có thể làm tốt hơn một chút bằng cách chỉ xét các ước số tiềm năng tối đa$\sqrt{n}$. 

Một sự tinh tế khác là sắp xếp thứ tự: các thừa số phải được in theo thứ tự không giảm, và bội số phải được khai triển đầy đủ chứ không được tóm tắt dưới dạng lũy ​​thừa. Ví dụ: 8 = 2^3 phải in dưới dạng`2 2 2`, không`2^3`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp lại trên tất cả các số nguyên từ 2 đến _n_, kiểm tra tính chia hết. Mỗi khi ước số _d_ chia _n_, nó sẽ được in và _n_ được chia cho _d_ cho đến khi nó không còn chia nữa. Điều này có hiệu quả vì bất kỳ số nguyên nào lớn hơn 1 đều là số nguyên tố hoặc có thể được phân tích thành số nguyên tố. Đối với _n_ nhỏ, số lần thử chia xấp xỉ$n$, điều này có thể chấp nhận được đối với$n \le 250$. 

Quan sát cho phép tối ưu hóa là không có hệ số nào lớn hơn$\sqrt{n}$(có thể ngoại trừ chính nó) cần được xem xét trong các lần thử lặp lại. Khi chúng ta đã loại bỏ tất cả các thừa số nhỏ hơn, mọi số còn lại đều phải là số nguyên tố. Điều này cho phép chúng tôi chỉ lặp tối đa$\sqrt{n}$và xử lý riêng số còn lại cuối cùng. Điều này làm giảm một chút số lượng kiểm tra không cần thiết trong khi vẫn giữ cho thuật toán đơn giản và dễ đọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận với n ≤ 250 | 
| Tối ưu | O(√n) | O(1) | Được chấp nhận và sạch hơn | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên _n_ từ đầu vào. Khởi tạo một biến`num`để lưu trữ số còn lại khi chúng tôi trích xuất các yếu tố. 
2. Lặp lại tất cả các số nguyên`i`bắt đầu từ thứ 2 đến$\sqrt{n}$. Đối với mỗi`i`, kiểm tra xem nó có chia không`num`. 
3. Trong khi`i`chia rẽ`num`, in`i`và chia`num`qua`i`. Tiếp tục vòng lặp này cho đến khi`i`không còn chia cắt`num`. Điều này đảm bảo các thừa số nguyên tố lặp lại được in đúng số lần. 
4. Sau khi kết thúc vòng lặp đến$\sqrt{n}$, nếu như`num`vẫn lớn hơn 1 thì bản thân nó phải là thừa số nguyên tố nên in ra`num`. 
5. Phân tách tất cả các số được in bằng dấu cách. 

Tại sao nó hoạt động: Mọi số nguyên lớn hơn 1 đều có thể được phân tích thành thừa số nguyên tố duy nhất. Bằng cách lặp lại đến$\sqrt{n}$và loại bỏ tất cả các thừa số, số còn lại phải là số nguyên tố nên thuật toán in từng thừa số nguyên tố chính xác theo đúng số lần. Điều bất biến là tại mỗi lần lặp,`num`bằng tích của tất cả các thành phần nguyên tố chưa được phân tích còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
num = n
i = 2
factors = []

while i * i <= num:
    while num % i == 0:
        factors.append(i)
        num //= i
    i += 1

if num > 1:
    factors.append(num)

print(' '.join(map(str, factors)))
```Giải pháp đầu tiên đọc số nguyên và khởi tạo`num`để theo dõi phần còn lại để tính hệ số. Vòng ngoài chạy qua các ước số tiềm năng lên đến$\sqrt{num}$và vòng lặp bên trong sẽ loại bỏ tất cả các lần xuất hiện của ước số hiện tại. Chúng tôi nối tất cả các yếu tố tìm thấy vào danh sách để duy trì trật tự. Kiểm tra cuối cùng cho`num > 1`xử lý trường hợp số còn lại là số nguyên tố và lớn hơn$\sqrt{n}$. Việc nối các yếu tố thành một chuỗi duy nhất đảm bảo định dạng đầu ra chính xác. 

## Ví dụ đã hoạt động 

Đối với đầu vào`245`: 

| tôi | số | yếu tố | 
| --- | --- | --- | 
| 2 | 245 | [] | 
| 3 | 245 | [] | 
| 4 | 245 | [] | 
| 5 | 245 | [] | 
| 5 | 49 | [5] | 
| 5 | 49 | [5] (kết thúc vòng lặp bên trong) | 
| 6 | 49 | [5] | 
| 7 | 49 | [5] | 
| 7 | 7 | [5, 7] | 
| 7 | 1 | [5, 7, 7] | 

Đầu ra là`5 7 7`, như mong đợi. Dấu vết này thể hiện việc xử lý chính xác các thừa số nguyên tố và thứ tự lặp lại. 

Đối với đầu vào`13`: 

| tôi | số | yếu tố | 
| --- | --- | --- | 
| 2 | 13 | [] | 
| 3 | 13 | [] | 
| 4 | 13 | [] | 
| 5 | 13 | [] | 
| num>1 kiểm tra | 13 | [13] | 

Đầu ra là`13`, chứng tỏ các số nguyên tố lớn hơn$\sqrt{n}$được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Vòng lặp bên ngoài lặp tới √n, vòng lặp bên trong chia mỗi thừa số một lần. | 
| Không gian | O(log n) | Lưu trữ các yếu tố; trường hợp xấu nhất là tất cả 2 giây, yêu cầu các mục nhập log₂(n). | 

Với n 250, √n nhiều nhất là 16, do đó giải pháp chạy trong thời gian không đáng kể với mức sử dụng bộ nhớ tầm thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    num = n
    i = 2
    factors = []
    while i * i <= num:
        while num % i == 0:
            factors.append(i)
            num //= i
        i += 1
    if num > 1:
        factors.append(num)
    return ' '.join(map(str, factors))

# provided samples
assert run("245\n") == "5 7 7", "sample 1"

# custom cases
assert run("2\n") == "2", "minimum prime"
assert run("4\n") == "2 2", "perfect square of smallest prime"
assert run("250\n") == "2 5 5 5", "composite with multiple factors"
assert run("13\n") == "13", "prime greater than √n"
assert run("81\n") == "3 3 3 3", "power of a prime"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 2 | số nguyên tố tối thiểu | 
| 4 | 2 2 | số nguyên tố nhỏ lặp đi lặp lại | 
| 250 | 2 5 5 5 | nhiều thừa số nguyên tố riêng biệt | 
| 13 | 13 | số nguyên tố lớn hơn √n | 
| 81 | 3 3 3 3 | sức mạnh của một số nguyên tố được xử lý đúng cách | 

## Vỏ cạnh 

Đầu vào nhỏ nhất`n = 2`, tạo ra một thừa số duy nhất`2`. Thuật toán khởi tạo`num = 2`, vào vòng lặp, thấy 2*2 > 2 nên bỏ qua vòng lặp bên trong, sau đó kiểm tra`num > 1`và in`2`. Vì`n = 49`, vòng lặp loại bỏ chính xác 7 hai lần, cho thấy các thừa số lặp lại ngoài √n được mở rộng hoàn toàn. Đối với các số nguyên tố lớn hơn √n, như`n = 13`, lần kiểm tra cuối cùng đảm bảo hệ số nguyên tố duy nhất được đưa ra. Cách tiếp cận này xử lý tất cả các tình huống không rõ ràng mà không cần phân nhánh đặc biệt.
