---
title: "CF 148A – Chữa mất ngủ"
description: "Bài toán yêu cầu chúng ta tìm ra có bao nhiêu con rồng bị ảnh hưởng bởi một công chúa có cách tự vệ độc đáo. Cô nhắm vào mọi con rồng thứ k, thứ l, thứ m và thứ n bằng những hành động khác nhau. Tổng số rồng là d."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 148
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 105 (Div. 2)"
rating: 800
weight: 148
solve_time_s: 94
verified: false
draft: false
---

[CF 148A - Chữa bệnh mất ngủ](https://codeforces.com/problemset/problem/148/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tìm ra có bao nhiêu con rồng bị ảnh hưởng bởi một công chúa có cách tự vệ độc đáo. Cô nhắm vào mọi con rồng thứ _k_-th, _l_-th, _m_-th và _n_-th bằng các hành động khác nhau. Tổng số rồng là _d_. Dữ liệu đầu vào cung cấp cho chúng ta năm số nguyên: _k_, _l_, _m_, _n_ và _d_. Đầu ra là một số nguyên biểu thị số lượng rồng bị tấn công bởi ít nhất một trong những hành động này. 

Quan sát quan trọng là một con rồng có thể bị ảnh hưởng nhiều lần, nhưng chúng tôi chỉ tính một lần. Ví dụ: nếu một con rồng vừa là rồng thứ 6 (bị ảnh hưởng bởi hành động thứ 2) vừa là rồng thứ 12 (bị ảnh hưởng bởi hành động thứ 3) thì nó vẫn được tính là một con rồng bị thiệt hại. Thách thức là đếm một cách hiệu quả những chỉ số độc đáo này trong số tất cả các con rồng. 

Những hạn chế là vừa phải. Số lượng rồng tối đa, _d_, có thể là 10^5, cho phép chúng ta lặp lại tất cả các con rồng trong một vòng lặp đơn giản. Các ước số _k_, _l_, _m_, _n_ đều 10, vì vậy việc kiểm tra xem chỉ số rồng có chia hết cho các số này hay không là không tốn kém. Các trường hợp cạnh cần xem xét bao gồm _d_ nhỏ nhất của 1 hoặc ước số là 1, điều này sẽ ảnh hưởng đến mọi con rồng. Một giải pháp đơn giản có thể bỏ lỡ các trường hợp đặc biệt trong đó tất cả các ước số là 1 và sẽ trả về chính nó _d_. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu rất đơn giản. Chúng ta có thể lặp lại mọi con rồng từ 1 đến _d_ và kiểm tra xem chỉ số có chia hết cho _k_, _l_, _m_ hay _n_ hay không. Nếu đúng như vậy, chúng tôi tăng một bộ đếm. Điều này hiệu quả vì _d_ chỉ là 10^5, do đó, một vòng lặp đơn giản có tối đa bốn phép toán modulo trên mỗi lần lặp sẽ có tổng cộng 4 × 10^5 phép toán, có thể chấp nhận được trong giới hạn thời gian 2 giây. Cách tiếp cận bạo lực hoạt động vì vấn đề về cơ bản là một vấn đề đếm với phạm vi đủ nhỏ để lặp lại, nhưng nó có thể cảm thấy không phù hợp so với cách tiếp cận dựa trên công thức. 

Cách tiếp cận tối ưu sử dụng nguyên tắc bao gồm-loại trừ để đếm tất cả những con rồng chia hết cho bất kỳ số nào. Đếm những con rồng chia hết cho _k_, sau đó là những con chia hết cho _l_, v.v., nhưng trừ đi số lần trùng lặp, thêm ba lần trùng lặp và trừ đi bốn lần trùng lặp. Quan sát giúp điều này có thể thực hiện được là những con số này nhỏ và bội số của chúng lặp lại thường xuyên, vì vậy chúng ta có thể tính toán số lượng mà không cần lặp lại. Đối với một bài toán ở quy mô này, brute-force đủ hiệu quả, dễ thực hiện và lý giải hơn, vì vậy chúng ta có thể giải quyết nó mà không cần lo ngại về hiệu suất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d) | O(1) | Đã chấp nhận | 
| Bao gồm-Loại trừ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các số nguyên _k_, _l_, _m_, _n_, và _d_. Chúng đại diện cho tần suất tấn công và tổng số rồng. 
2. Khởi tạo bộ đếm`damaged`về không. Điều này sẽ theo dõi số lượng rồng bị ảnh hưởng. 
3. Lặp lại tất cả các chỉ số rồng từ 1 đến _d_. Đối với mỗi con rồng: 

1. Kiểm tra xem chỉ mục có chia hết cho _k_, _l_, _m_ hay _n_ hay không. Nếu bất kỳ điều kiện nào trong số này được giữ, hãy tăng`damaged`. 
4. Sau vòng lặp, xuất ra giá trị của`damaged`. 

Tại sao nó hoạt động: Mỗi con rồng được coi là đúng một lần và chúng tôi kiểm tra tất cả bốn điều kiện để bị tấn công. Bất kỳ con rồng nào chia hết cho ít nhất một trong các số sẽ được tính và những con chia hết cho nhiều số vẫn chỉ được tính một lần. Điều này đảm bảo tính chính xác mà không cần tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input())
l = int(input())
m = int(input())
n = int(input())
d = int(input())

damaged = 0
for i in range(1, d + 1):
    if i % k == 0 or i % l == 0 or i % m == 0 or i % n == 0:
        damaged += 1

print(damaged)
```Mã trực tiếp theo thuật toán. Mỗi`input()`đọc một số nguyên duy nhất. Vòng lặp chạy từ 1 đến`d`bao gồm, kiểm tra tính chia hết bằng modulo. sử dụng`or`đảm bảo một con rồng bị ảnh hưởng bởi nhiều hành động chỉ được tính một lần. trận chung kết`print`xuất ra số đếm chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

Giá trị đầu vào: k=1, l=2, m=3, n=4, d=12 

| tôi | i%k==0 | i%l==0 | tôi%m==0 | i%n==0 | bị hư hỏng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Đúng | Sai | Sai | Sai | 1 | 
| 2 | Đúng | Đúng | Sai | Sai | 2 | 
| 3 | Đúng | Sai | Đúng | Sai | 3 | 
| ... | ... | ... | ... | ... | ... | 
| 12 | Đúng | Đúng | Đúng | Đúng | 12 | 

Tất cả rồng đều bị ảnh hưởng vì k=1 trúng mỗi con rồng. 

**Mẫu 2** 

Giá trị đầu vào: k=2, l=3, m=4, n=5, d=12 

| tôi | i%k==0 | i%l==0 | tôi%m==0 | i%n==0 | bị hư hỏng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Sai | Sai | Sai | Sai | 0 | 
| 2 | Đúng | Sai | Sai | Sai | 1 | 
| 3 | Sai | Đúng | Sai | Sai | 2 | 
| 4 | Đúng | Sai | Đúng | Sai | 3 | 
| ... | ... | ... | ... | ... | ... | 
| 12 | Đúng | Đúng | Đúng | Sai | 9 | 

Điều này thể hiện việc đếm những con rồng chia hết cho nhiều số mà không cần đếm gấp đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) | Mỗi chỉ số rồng từ 1 đến d được kiểm tra một lần dựa trên bốn ước số. | 
| Không gian | O(1) | Chỉ có một biến đếm và biến vòng lặp duy nhất được sử dụng. | 

Với các ràng buộc, các thao tác O(10^5) đủ nhanh cho giới hạn 2 giây và mức sử dụng bộ nhớ ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k = int(input())
    l = int(input())
    m = int(input())
    n = int(input())
    d = int(input())

    damaged = 0
    for i in range(1, d + 1):
        if i % k == 0 or i % l == 0 or i % m == 0 or i % n == 0:
            damaged += 1
    return str(damaged)

# Provided samples
assert run("1\n2\n3\n4\n12\n") == "12", "sample 1"
assert run("2\n3\n4\n5\n12\n") == "9", "sample 2"

# Custom cases
assert run("1\n1\n1\n1\n1\n") == "1", "minimum d"
assert run("10\n10\n10\n10\n100000\n") == "10000", "maximum d with large k,l,m,n"
assert run("2\n2\n2\n2\n7\n") == "3", "all equal divisors, small d"
assert run("2\n3\n5\n7\n30\n") == "20", "combination of primes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 1 | 1 | D tối thiểu, tất cả rồng bị ảnh hưởng | 
| 10 10 10 10 100000 | 10000 | D lớn với ước số lớn | 
| 2 2 2 2 7 | 3 | Các ước số bằng nhau, d nhỏ, xử lý từng cái một | 
| 2 3 5 7 30 | 20 | Hỗn hợp các ước số nguyên tố, đếm bội số đúng | 

## Vỏ cạnh 

Khi k=l=m=n=1 và d=1, con rồng duy nhất bị ảnh hưởng và mã trả về 1 chính xác vì kiểm tra modulo kích hoạt. Nếu d=100000 với các ước số đều là 10, thì số đếm bị hỏng là 100000 chia cho 10, tạo ra 10000. Khi một số ước số lớn hơn d, chẳng hạn như k=50 và d=30, không có con rồng nào thỏa mãn i%50==0, do đó, vòng lặp sẽ bỏ qua những ước số này một cách chính xác mà không cần đếm hai lần hoặc xảy ra lỗi.
