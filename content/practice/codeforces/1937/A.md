---
title: "CF 1937A - Bữa tiệc ngẫu nhiên"
description: "Chúng ta được yêu cầu theo dõi vị trí của số 1 trong một chuỗi hoán đổi đặc biệt trên một mảng có độ dài n. Ban đầu mảng là [1, 2, 3, ..., n]. Với mỗi k từ 2 đến n, chúng ta hoán đổi ak với ad, trong đó d là ước số thực sự lớn nhất của k."
date: "2026-06-08T17:56:57+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1937
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 930 (Div. 2)"
rating: 800
weight: 1937
solve_time_s: 108
verified: true
draft: false
---

[CF 1937A - Bữa tiệc ngẫu nhiên](https://codeforces.com/problemset/problem/1937/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu theo dõi vị trí của số`1`trong một chuỗi hoán đổi đặc biệt trên một mảng có độ dài`n`. Ban đầu, mảng là`[1, 2, 3, ..., n]`. Đối với mọi`k`từ`2`ĐẾN`n`, chúng ta trao đổi`a_k`với`a_d`, Ở đâu`d`là ước số thực sự lớn nhất của`k`. Sau khi thực hiện tất cả các giao dịch hoán đổi này theo thứ tự tăng dần`k`, mục tiêu là báo cáo chỉ số tại đó`1`kết thúc. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho một số nguyên duy nhất`n`. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm - vị trí cuối cùng của`1`. 

Những hạn chế là quan trọng để lý giải.`n`có thể lớn tới 10^9 và số lượng ca kiểm thử`t`có thể lên tới 10^4. Một mô phỏng đơn giản thực hiện hoán đổi O(n) cho mỗi trường hợp thử nghiệm là không khả thi vì tổng số thao tác sẽ đạt tới 10^13, vượt xa giới hạn thời gian cho phép. Vì vậy, chúng ta cần một thuật toán có thời gian chạy không phụ thuộc tuyến tính vào`n`. 

Một trường hợp cạnh tinh tế phát sinh đối với các mảng nhỏ. Vì`n = 1`, không có sự hoán đổi nào xảy ra và`1`vẫn ở vị trí 1. Một trường hợp cạnh khác là khi`n`là sức mạnh của hai. Quan sát kết quả đầu ra của mẫu cho thấy rằng vị trí cuối cùng của`1`có xu hướng tuân theo lũy thừa của hai, điều này gợi ý về một thuộc tính toán học làm cơ sở cho các giao dịch hoán đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Khởi tạo mảng`[1, 2, ..., n]`và lặp lại`k`từ`2`ĐẾN`n`, tính ước số thực lớn nhất`d`của`k`, và trao đổi`a_k`với`a_d`. Điều này đúng nhưng yêu cầu thao tác O(n) cho mỗi trường hợp thử nghiệm. Vì`n = 10^9`, không thể mô phỏng tất cả các giao dịch hoán đổi trong 1 giây. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát hành vi của`1`trong quá trình hoán đổi. Con số`1`ban đầu ở vị trí 1. Nó chỉ di chuyển khi chúng ta đổi chỗ một số`k`có ước số thực sự lớn nhất`d`bằng với vị trí hiện tại của`1`. Điều này xảy ra chính xác khi`k`gấp đôi vị trí hiện tại của`1`. Kết quả là, vị thế của`1`tăng gấp đôi mỗi lần nó di chuyển. Sự nhân đôi này tiếp tục cho đến khi`2*position > n`, tại thời điểm đó không có trao đổi nào nữa ảnh hưởng đến`1`. Vì vậy, vị trí cuối cùng của`1`là lũy thừa lớn nhất của hai không vượt quá`n`. 

Quan sát này làm giảm vấn đề thành một phép tính đơn giản về lũy thừa cao nhất của hai nhỏ hơn hoặc bằng`n`, là O(1) cho mỗi trường hợp thử nghiệm và không yêu cầu xây dựng hoặc mô phỏng mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| Tối ưu | O(log n) hoặc O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`. 
3. Tính lũy thừa lớn nhất của hai không vượt quá`n`. Điều này có thể được thực hiện bằng cách bắt đầu với`1`và nhân liên tục với`2`cho đến khi phép nhân tiếp theo vượt quá`n`. 
4. In giá trị kết quả. 

Tại sao nó hoạt động: Mỗi trao đổi ảnh hưởng đến`1`chỉ nếu`1`ở vị trí bằng ước số thực sự lớn nhất của`k`. Vì ước số thực sự lớn nhất của một số nguyên nhiều nhất là một nửa số nguyên,`1`di chuyển chính xác khi vị trí hiện tại của nó tăng gấp đôi. Điều này tạo ra một chuỗi`1, 2, 4, 8, ...`cho đến khi chúng ta vượt quá`n`. Điều bất biến là ở mỗi bước vị trí của`1`là lũy thừa của hai và lần hoán đổi tiếp theo ảnh hưởng đến`1`nhân đôi nó. Do đó, vị trí cuối cùng là lũy thừa lớn nhất của hai ≤`n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def largest_power_of_two_leq(n):
    power = 1
    while power * 2 <= n:
        power *= 2
    return power

t = int(input())
for _ in range(t):
    n = int(input())
    print(largest_power_of_two_leq(n))
```chức năng`largest_power_of_two_leq`bắt đầu bằng 1 và nhân đôi nó cho đến khi gấp đôi tiếp theo vượt quá`n`. Điều này hiệu quả vì nó lặp lại nhiều nhất là log2(n) lần. Mỗi trường hợp thử nghiệm sử dụng không gian bổ sung không đổi. I/O nhanh đảm bảo chúng tôi xử lý tối đa 10^4 trường hợp kiểm thử một cách hiệu quả. Cần chú ý đảm bảo tình trạng`power*2 <= n`ngăn cản việc vượt quá mức. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
1
4
5
120240229
```Dấu vết cho`n = 4`: 

| Bước | Vị trí hiện tại của 1 | Ứng cử viên quyền lực | 
| --- | --- | --- | 
| Ban đầu | 1 | 1 | 
| Nhân với 2 | 2 | 2 | 
| Nhân với 2 | 4 | 4 | 
| Nhân với 2 | 8 > 4 | dừng lại | 

Vị trí cuối cùng = 4 

Dấu vết cho`n = 5`: 

| Bước | Vị trí hiện tại của 1 | Ứng cử viên quyền lực | 
| --- | --- | --- | 
| Ban đầu | 1 | 1 | 
| Nhân với 2 | 2 | 2 | 
| Nhân với 2 | 4 | 4 | 
| Nhân với 2 | 8 > 5 | dừng lại | 

Vị trí cuối cùng = 4 

Những dấu vết này chứng tỏ rằng`1`di chuyển qua lũy thừa của hai cho đến khi nhân đôi tiếp theo sẽ vượt quá`n`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) cho mỗi trường hợp thử nghiệm | Mỗi lần lặp lại tăng gấp đôi`power`cho đến khi vượt quá`n`, nhiều nhất log2(n) bước | 
| Không gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một biến số nguyên duy nhất được sử dụng | 

Tổng thời gian chạy cho t 10^4 và n 10^9 nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    def largest_power_of_two_leq(n):
        power = 1
        while power * 2 <= n:
            power *= 2
        return power
    for _ in range(t):
        n = int(input())
        print(largest_power_of_two_leq(n))
    return output.getvalue().strip()

# Provided samples
assert run("4\n1\n4\n5\n120240229\n") == "1\n4\n4\n67108864", "Sample 1"

# Custom tests
assert run("3\n2\n3\n8\n") == "2\n2\n8", "small arrays and exact power of two"
assert run("2\n10\n1023\n") == "8\n512", "non-powers of two"
assert run("1\n1\n") == "1", "minimum n"
assert run("1\n1000000000\n") == "536870912", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n3\n8 | 2\n8 | n nhỏ, lũy thừa chính xác của hai | 
| 10 | 8 | lũy thừa lớn nhất của hai ≤ n | 
| 1023 | 512 | lũy thừa lớn nhất của hai < n | 
| 1 | 1 | mảng kích thước tối thiểu | 
| 1000000000 | 536870912 | n tối đa, hiệu suất | 

## Vỏ cạnh 

cho`n = 1`, vòng lặp trong`largest_power_of_two_leq`không thực thi vì`1*2 > 1`. Hàm trả về đúng 1. Đối với lũy thừa của hai, chẳng hạn như`n = 8`, vòng lặp tiếp tục cho đến khi`power = 8`và dừng lại, trả về chính xác 8. Đối với`n`ngay dưới sức mạnh của hai, chẳng hạn như`n = 7`, vòng lặp dừng ở 4, là lũy thừa lớn nhất của hai ≤ 7. Tất cả các trường hợp này đều được xử lý chính xác bằng cùng một cơ chế nhân đôi.
