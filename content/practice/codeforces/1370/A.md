---
title: "CF 1370A - GCD tối đa"
description: "Chúng ta được yêu cầu tìm ước chung lớn nhất có thể có trong số tất cả các cặp số nguyên phân biệt từ 1 đến n. Với một n cho trước, chúng ta cần gcd lớn nhất của hai số a và b bất kỳ trong đó 1 ≤ a < b ≤ n."
date: "2026-06-11T11:24:46+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1370
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 651 (Div. 2)"
rating: 800
weight: 1370
solve_time_s: 94
verified: true
draft: false
---

[CF 1370A - GCD tối đa](https://codeforces.com/problemset/problem/1370/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm ước chung lớn nhất có thể có trong số tất cả các cặp số nguyên phân biệt từ 1 đến n. Với một n cho trước, chúng ta cần gcd lớn nhất của hai số a và b bất kỳ trong đó 1 ≤ a < b ≤ n. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định một giá trị n và đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm. 

Các ràng buộc cho chúng ta biết rằng n có thể lên tới 10^6 và có thể có tới 100 trường hợp thử nghiệm. Một giải pháp đơn giản thử tất cả các cặp số nguyên sẽ thực hiện các phép tính khoảng n2/2 gcd trong trường hợp xấu nhất. Với n = 10^6, đây là cấp độ của các phép toán 5 × 10^11, quá lớn để có thể chạy trong một giây. Chúng ta cần một cách tiếp cận chạy trong thời gian gần tuyến tính hoặc logarit so với n. 

Các trường hợp cạnh bao gồm n = 2 nhỏ nhất, trong đó cặp duy nhất là (1, 2) và gcd là 1. Một sự tinh tế khác nảy sinh khi n lẻ so với chẵn. Ví dụ: nếu n = 3 thì các cặp là (1, 2), (1, 3), (2, 3), tất cả đều có gcd 1. Với n = 5, cặp (2, 4) có gcd 2, lớn hơn 1. Bất kỳ nghiệm nào không tính đến các mẫu này đều có thể đánh giá thấp gcd tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua tất cả các cặp (a, b), tính gcd(a, b) cho mỗi cặp và theo dõi mức tối đa. Điều này đúng vì nó trực tiếp kiểm tra mọi kết hợp có thể, nhưng quá chậm khi n đạt 10^6. Đối với t = 100 trường hợp thử nghiệm, sẽ cần nhiều hơn 10^13 thao tác trong trường hợp xấu nhất, điều này rõ ràng là không thực tế. 

Cách tiếp cận tối ưu phụ thuộc vào quan sát rằng gcd lớn nhất có thể có ít nhất là n//2. Hãy xem xét chính số n. Nếu chúng ta tìm bội số của k ≤ n tạo thành một cặp với gcd k, thì k lớn nhất cho phép hai bội số phân biệt trong [1, n] là n//2. Điều này có hiệu quả vì cặp (k, 2k) luôn nằm trong phạm vi khi k ≤ n//2. Không có giá trị lớn hơn của k có thể tạo ra hai bội số phân biệt trong phạm vi. Do đó, với mỗi n, gcd tối đa chỉ đơn giản là n//2. 

Cái nhìn sâu sắc này làm giảm vấn đề thành một phép chia số nguyên duy nhất cho mỗi trường hợp thử nghiệm, tạo ra giải pháp O(1) cho mỗi trường hợp và O(t) tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case t. Điều này cho chúng ta biết chúng ta cần xử lý bao nhiêu giá trị độc lập của n. 
2. Với mỗi test, đọc số nguyên n. 
3. Tính n chia cho 2 bằng phép chia số nguyên. Gán giá trị này cho max_gcd. Điều này có tác dụng vì số nguyên lớn nhất có ít nhất hai bội số trong [1, n] chính xác là n//2. 
4. Xuất max_gcd cho test case này. 
5. Lặp lại các bước 2-4 cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Với mọi k > n//2, bội số duy nhất của k trong [1, n] chính là k, không thể tạo thành một cặp số riêng biệt. Với k ≤ n/2 thì cặp (k, 2k) hợp lệ, đảm bảo gcd k. Do đó n//2 là gcd lớn nhất có thể có trong số tất cả các cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    print(n // 2)
```Mã đọc số lượng trường hợp kiểm thử, sau đó lặp lại từng n trường hợp. Toán tử chia số nguyên`//`đảm bảo chúng tôi luôn làm tròn xuống, điều này rất quan trọng đối với các giá trị n lẻ. In trực tiếp sau khi tính toán giúp tránh lưu trữ dữ liệu không cần thiết và giảm thiểu mức sử dụng bộ nhớ. 

## Ví dụ đã hoạt động 

Mẫu 1: 

Đầu vào n = 3 

| n | n // 2 | max_gcd | 
| --- | --- | --- | 
| 3 | 1 | 1 | 

Cặp (1, 2) hoặc (1, 3) hoặc (2, 3) đều có gcd 1. n//2 đúng cho 1. 

Mẫu 2: 

Đầu vào n = 5 

| n | n // 2 | max_gcd | 
| --- | --- | --- | 
| 5 | 2 | 2 | 

Cặp (2, 4) có gcd 2, là giá trị lớn nhất. n/2 xác định chính xác điều này. 

Những dấu vết này chứng tỏ rằng với cả n lẻ và chẵn nhỏ, thuật toán chọn gcd cực đại chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một phép chia số nguyên cho mỗi trường hợp thử nghiệm, thời gian không đổi | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài đầu vào và đầu ra | 

Cho t ≤ 100, lời giải thực hiện tối đa 100 thao tác đơn giản, thoải mái trong thời gian giới hạn. Bộ nhớ không đáng kể, thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(n // 2)
    return output.getvalue().strip()

# Provided samples
assert run("2\n3\n5\n") == "1\n2", "sample 1 & 2"

# Minimum-size input
assert run("1\n2\n") == "1", "minimum n"

# Maximum-size input
assert run("1\n1000000\n") == "500000", "maximum n"

# Odd n
assert run("1\n7\n") == "3", "odd n"

# Even n
assert run("1\n8\n") == "4", "even n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | xử lý n tối thiểu | 
| 1000000 | 500000 | xử lý n tối đa | 
| 7 | 3 | trường hợp lẻ n | 
| 8 | 4 | trường hợp chẵn | 

## Vỏ cạnh 

Với n = 2 thì cặp duy nhất là (1, 2). Thuật toán tính 2//2 = 1, khớp với gcd tối đa thực tế. Đối với n lẻ như 7, cặp lớn nhất có gcd cực đại là (3, 6), cho gcd 3. Thuật toán đưa ra 7//2 = 3, xác định chính xác điều này. Đối với n = 10^6 lớn, thuật toán tính toán 500000, là gcd của cặp (500000, 1000000), chứng tỏ rằng giải pháp sẽ mở rộng đến ràng buộc trên mà không gặp vấn đề gì.
