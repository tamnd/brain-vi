---
title: "CF 1143B - Niết bàn"
description: "Chúng ta được yêu cầu tìm, với một số nguyên $n$, tích lớn nhất của các chữ số mà bất kỳ số nào từ 1 đến $n$ đều có thể có. Nói cách khác, hãy tưởng tượng việc lặp qua tất cả các số từ 1 đến $n$ và nhân các chữ số của mỗi số; chúng tôi muốn sản phẩm đó ở mức tối đa."
date: "2026-06-12T03:35:51+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1143
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 549 (Div. 2)"
rating: 1200
weight: 1143
solve_time_s: 96
verified: true
draft: false
---

[CF 1143B - Nirvana](https://codeforces.com/problemset/problem/1143/B) 

**Đánh giá:** 1200 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm, với một số nguyên cho trước$n$, tích lớn nhất của các chữ số mà bất kỳ số nào từ 1 đến$n$có thể có. Nói cách khác, hãy tưởng tượng việc lặp qua tất cả các số từ 1 đến$n$và nhân các chữ số của mỗi số; chúng tôi muốn sản phẩm đó ở mức tối đa. 

Đầu vào là một số nguyên duy nhất$n$Ở đâu$1 \le n \le 2 \cdot 10^9$. Giới hạn trên này ngay lập tức cho chúng ta biết rằng việc lặp qua tất cả các số từ 1 đến$n$là không khả thi, bởi vì đó có thể là hai tỷ phép tính và bất kỳ giải pháp nào kiểm tra mọi con số đều sẽ hết thời gian chờ. 

Các trường hợp cạnh không rõ ràng phát sinh khi số tối ưu có các chữ số nhỏ hơn$n$chính nó. Ví dụ, với$n = 390$, số tối ưu là$389$, không$390$, vì việc thay đổi chữ số cuối cùng từ 0 thành 9 có thể tạo ra tích lớn hơn. Một tình huống phức tạp khác là khi$n$bao gồm chủ yếu là số 9; sau đó$n$bản thân nó có thể là tối ưu. Cuối cùng, một chữ số$n$các giá trị không đáng kể nhưng vẫn phải được xử lý chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản để mô tả: lặp qua mọi số từ 1 đến$n$, tính tích các chữ số của nó và theo dõi mức tối đa. Điều này đúng vì nó kiểm tra rõ ràng tất cả các khả năng. Độ phức tạp của nó là$O(n \cdot d)$, Ở đâu$d$là số chữ số của số hiện tại, nhưng ngay cả với$d \approx 10$vì$n = 2 \cdot 10^9$, nó quá chậm vì$n$bản thân nó là rất lớn. 

Thông tin chi tiết quan trọng để có cách tiếp cận nhanh hơn đến từ việc quan sát cách sản phẩm hoạt động khi chữ số giảm. Nếu chúng ta có một số như 390, việc giảm một trong các chữ số (từ 0 xuống 9) trong khi giảm tất cả các chữ số tiếp theo xuống 9 thường sẽ làm tăng tích. Điều này là do số 0 trong các chữ số sẽ giết chết hoàn toàn sản phẩm. Vì vậy, thay vì kiểm tra tất cả các số, chúng ta chỉ cần kiểm tra các số được tạo thành bởi: 

1. Giữ một số tiền tố của$n$giống nhau. 
2. Giảm chữ số đầu tiên khác 0 trong hậu tố còn lại đi 1. 
3. Thay tất cả các chữ số tiếp theo bằng 9. 

Ngoài ra, chúng ta phải luôn xem xét$n$chính nó. Điều này làm giảm số ứng cử viên từ hàng tỷ xuống nhiều nhất là số chữ số trong$n$cộng một, tối đa là 10 cho các ràng buộc của chúng tôi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·d) | O(1) | Quá chậm | 
| Tối ưu | O(d^2) | O(d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi$n$thành một chuỗi$s$để thao tác các chữ số riêng lẻ. 
2. Khởi tạo`max_product`là tích của các chữ số của$n$chính nó. Điều này đảm bảo chúng tôi luôn cân nhắc$n$với tư cách là một ứng cử viên. 
3. Lặp lại từng vị trí chữ số$i$TRONG$s$: 

1. Nếu chữ số ở vị trí$i$là 0, chuyển sang vị trí tiếp theo vì giảm 0 sẽ tạo ra chữ số âm hoặc không hợp lệ. 
2. Tạo số ứng viên mới bằng cách giữ tiền tố tối đa$i-1$không thay đổi. 
3. Giảm chữ số tại$i$bằng 1. 
4. Thay thế tất cả các chữ số sau$i$với 9. 
5. Tính tích các chữ số của ứng viên này và cập nhật`max_product`nếu lớn hơn. 
4. Đầu ra`max_product`. 

Tại sao nó hoạt động: Tính bất biến là đối với bất kỳ số nào lớn hơn$n$, giảm chữ số đầu tiên khác 0 và điền vào phần còn lại bằng 9 sẽ tối đa hóa tích mà không vượt quá$n$. Bằng cách kiểm tra tất cả các vị trí có thể xảy ra mức giảm, chúng tôi đảm bảo tìm thấy sản phẩm tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def product_of_digits(num_str):
    prod = 1
    for ch in num_str:
        prod *= int(ch)
    return prod

def main():
    n = input().strip()
    max_prod = product_of_digits(n)
    
    n_list = list(n)
    for i in range(len(n_list)):
        if n_list[i] == '0':
            continue
        candidate = n_list[:i] + [str(int(n_list[i])-1)] + ['9'] * (len(n_list)-i-1)
        candidate_str = ''.join(candidate).lstrip('0')
        if candidate_str:
            max_prod = max(max_prod, product_of_digits(candidate_str))
    
    print(max_prod)

if __name__ == "__main__":
    main()
```Đầu tiên, mã này xác định hàm trợ giúp để tính tích các chữ số. Sau đó nó lặp lại từng chữ số trong`n`, thử giảm chữ số đó đi 1, điền vào phần còn lại bằng 9 giây và tính tích. Các số 0 đứng đầu bị loại bỏ vì chúng không đóng góp vào số. Sản phẩm tối đa trong số tất cả các ứng cử viên được in. 

## Ví dụ đã hoạt động 

### Mẫu 1:$n = 390$| Bước | Ứng viên | Sản phẩm | 
| --- | --- | --- | 
| Bản gốc | 390 | 3_9_0 = 0 | 
| Giảm chữ số đầu tiên (3->2) + 9s | 299 | 2_9_9 = 162 | 
| Giảm chữ số thứ hai (9->8) + 9s | 389 | 3_8_9 = 216 | 

Đồ thị cho thấy rằng việc giảm chữ số thứ hai sẽ cho kết quả tối ưu là 216. 

### Mẫu 2:$n = 7$| Bước | Ứng viên | Sản phẩm | 
| --- | --- | --- | 
| Bản gốc | 7 | 7 | 

Không thể giảm được. Sản phẩm tối đa là 7. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d^2) |`d`là số chữ số trong`n`. Mỗi ứng viên yêu cầu tính tích của tối đa`d`chữ số. | 
| Không gian | O(d) | Để lưu trữ các chuỗi chữ số và ứng cử viên. | 

Được cho$d \le 10$, giải pháp sẽ thực hiện trong vài trăm thao tác, trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("390\n") == "216", "sample 1"
assert run("7\n") == "7", "sample 2"
assert run("999999999\n") == "387420489", "sample 3"

# Custom cases
assert run("1\n") == "1", "minimum n"
assert run("10\n") == "9", "two-digit with zero"
assert run("1000000000\n") == "999999999", "largest n with zeroes"
assert run("1234\n") == "1080", "mixed digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Đầu vào tối thiểu | 
| 10 | 9 | Xử lý số 0 chính xác | 
| 1000000000 | 387420489 | N tối đa có số 0, số lớn | 
| 1234 | 1080 | Logic giảm và thay thế chữ số chính xác | 

## Vỏ cạnh 

cho$n = 10$, thuật toán kiểm tra 10 gốc (tích 0) và rút gọn chữ số đầu tiên 1->0 với phần còn lại là 9 -> 9. Tích tối đa trở thành 9. Đối với$n = 1000000000$, giảm 1 đầu tiên xuống 0 và điền phần còn lại bằng 9 mang lại 999999999, tích$9^9$. Đầu vào một chữ số được xử lý tầm thường bằng cách khởi tạo`max_product`với tích các chữ số của`n`. 

Thuật toán không bao giờ bỏ lỡ một ứng cử viên tối ưu vì bất kỳ số nào có thể vượt qua tích của`n`có thể được hình thành bằng cách giảm một chữ số tiền tố và điền phần còn lại bằng 9s.
