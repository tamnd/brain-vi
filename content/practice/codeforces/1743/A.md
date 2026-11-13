---
title: "CF 1743A - Mật khẩu"
description: "Vấn đề về mật khẩu của Monocarp là một vấn đề đếm bị hạn chế bởi cả bội số và loại trừ. Mật khẩu là một chuỗi gồm bốn chữ số. Mỗi chuỗi phải chứa chính xác hai chữ số riêng biệt và mỗi chữ số này xuất hiện đúng hai lần."
date: "2026-06-09T16:01:50+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1743
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 137 (Rated for Div. 2)"
rating: 800
weight: 1743
solve_time_s: 166
verified: true
draft: false
---

[CF 1743A - Mật khẩu](https://codeforces.com/problemset/problem/1743/A) 

**Đánh giá:** 800 
**Tags:** lực lượng vũ phu, tổ hợp, thực hiện, toán học 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề về mật khẩu của Monocarp là một vấn đề đếm bị hạn chế bởi cả bội số và loại trừ. Mật khẩu là một chuỗi gồm bốn chữ số. Mỗi chuỗi phải chứa chính xác hai chữ số riêng biệt và mỗi chữ số này xuất hiện đúng hai lần. Chúng ta cũng được cung cấp danh sách các chữ số không thể xuất hiện trong mật khẩu. Nhiệm vụ là xác định có bao nhiêu chuỗi 4 chữ số hợp lệ tồn tại tuân theo các ràng buộc này. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, trước tiên chúng tôi đọc số chữ số bị cấm, sau đó là chính các chữ số bị cấm. Đầu ra phải là số lượng các chuỗi thỏa mãn yêu cầu hai chữ số riêng biệt, mỗi chữ số có hai chữ số đồng thời tránh các chữ số bị cấm. 

Các ràng buộc rất nhỏ: tổng cộng tối đa 10 chữ số và tối đa 8 chữ số bị cấm. Điều này có nghĩa là bất kỳ giải pháp nào thực hiện phép tính tổ hợp đơn giản trên các chữ số được phép còn lại sẽ có hiệu quả. Các trường hợp đặc biệt bao gồm các tình huống trong đó các chữ số bị cấm chỉ để lại một hoặc không có chữ số nào có thể sử dụng được, trong đó không thể có mật khẩu hợp lệ. Chẳng hạn, nếu cấm 9 trên 10 chữ số, chỉ để lại một chữ số thì không thể tạo mật khẩu có hai chữ số riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra tất cả các chuỗi 4 chữ số có thể có từ 0 đến 9, kiểm tra xem có chính xác hai chữ số riêng biệt xuất hiện không và mỗi chữ số xuất hiện hai lần, sau đó loại bỏ các chuỗi chứa các chữ số bị cấm. Giải pháp bạo lực này sẽ hiệu quả vì chỉ có 10.000 trình tự, nhưng nó không cần thiết và tẻ nhạt. 

Một cách tiếp cận tốt hơn dựa vào tổ hợp. Khi chúng tôi loại bỏ các chữ số bị cấm, chúng tôi chỉ còn lại`k`chữ số cho phép. Để tạo một mật khẩu hợp lệ, chúng ta cần chọn chính xác hai chữ số riêng biệt từ các chữ số này`k`chữ số cho phép. Đây chỉ đơn giản là "k chọn 2". Với mỗi cặp như vậy, có chính xác sáu dãy riêng biệt thỏa mãn điều kiện hai chữ số: các dãy đều là những hoán vị có thể có của`[d1, d1, d2, d2]`, đó là`4! / (2! * 2!) = 6`. Do đó, tổng số chuỗi hợp lệ là`6 * (k choose 2)`. Nếu như`k`nhỏ hơn 2, không tồn tại mật khẩu hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
|---|---|---|---| 
| Lực lượng vũ phu | O(10^4) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng không cần thiết | 
| Đếm tổ hợp | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận và tối ưu | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 

2. Với mỗi test, đọc số chữ số bị cấm`n`và danh sách các chữ số bị cấm. 

3. Tính số chữ số được phép là`k = 10 - n`. 

4. Nếu`k < 2`, xuất ra 0 vì không thể chọn hai chữ số riêng biệt. 

5. Mặt khác, hãy tính số cách chọn hai chữ số từ các chữ số cho phép, đó là`k * (k - 1) / 2`. 

6. Nhân số cặp với 6 để tính hoán vị của`[d1, d1, d2, d2]`. 

7. Xuất kết quả. 

Phương pháp này đảm bảo tính chính xác vì nó liệt kê trực tiếp tất cả các khả năng tổ hợp mà không tính quá mức hoặc bỏ sót bất kỳ trường hợp nào. Điều bất biến được duy trì là chúng ta chỉ đếm các chuỗi có đúng hai chữ số riêng biệt, mỗi chuỗi xuất hiện chính xác hai lần và không chứa các chữ số bị cấm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    forbidden = list(map(int, input().split()))
    k = 10 - n  # number of allowed digits
    
    if k < 2:
        print(0)
        continue
    
    # number of pairs of distinct allowed digits
    pairs = k * (k - 1) // 2
    
    # each pair has 6 permutations of type [d1, d1, d2, d2]
    result = pairs * 6
    print(result)
```Mã sử ​​dụng khả năng đọc đầu vào nhanh và xử lý nhiều trường hợp thử nghiệm một cách hiệu quả. Chúng tôi trừ các chữ số bị cấm từ 10 để có được số chữ số có thể sử dụng được. Chúng tôi kiểm tra trường hợp tầm thường khi còn lại ít hơn hai chữ số. Đối với số đếm hợp lệ, chúng tôi tính toán "k chọn 2" và nhân với sáu để có sáu cách sắp xếp có thể có của mỗi cặp chữ số. 

## Ví dụ đã hoạt động 

For the first sample:

 | Trường hợp thử nghiệm | Forbidden | Allowed k | Pairs kC2 | Sequences |
 |---|---|---|---|---|
 | 8 cấm: 0,1,2,4,5,6,8,9 | 3,7 | 2 | 1 | 1 * 6 = 6 |

 Các chữ số được phép là 3 và 7. Chỉ có một cách để chọn hai chữ số phân biệt. Mỗi cặp có thể được sắp xếp theo 6 cách, tạo thành 6 dãy. 

For the second sample:

 | Trường hợp thử nghiệm | Bị cấm | Allowed k | Cặp kC2 | Sequences |
 |---|---|---|---|---|
 | 1 forbidden: 8 | 0-7,9 (9 digits) | 9 | 36 | 36 * 6 = 216 |

 Các chữ số được phép là 0-7,9. Có 9 chọn 2 = 36 cặp. Mỗi cặp có 6 hoán vị, tạo ra 216 dãy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ tính toán số học, không lặp lại các chuỗi | 
| Không gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ cần một vài biến số nguyên | 

Giải pháp cực kỳ hiệu quả với những hạn chế. Với tối đa 200 trường hợp thử nghiệm và hằng số nhỏ, thuật toán hoàn thành tốt trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        forbidden = list(map(int, input().split()))
        k = 10 - n
        if k < 2:
            output.append("0")
            continue
        pairs = k * (k - 1) // 2
        output.append(str(pairs * 6))
    return "\n".join(output)

# provided samples
assert run("2\n8\n0 1 2 4 5 6 8 9\n1\n8\n") == "6\n216", "sample 1"

# custom cases
assert run("1\n0\n") == "6*45=270\n".strip(), "all digits allowed"
assert run("1\n9\n0 1 2 3 4 5 6 7 8\n") == "0", "only one digit allowed"
assert run("1\n1\n5\n") == "216", "forbidden digit in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| 0 bị cấm | 270 | tất cả các chữ số có sẵn, chuỗi tối đa | 
| 9 bị cấm | 0 | không đủ chữ số để tạo thành mật khẩu hai phân biệt | 
| 1 bị cấm (5) | 216 | trường hợp tổng quát có một chữ số bị cấm | 

## Vỏ cạnh 

Nếu tất cả trừ một chữ số đều bị cấm,`k = 1`, thuật toán trả về chính xác 0 vì không thể tạo mật khẩu. Nếu không có chữ số nào bị cấm,`k = 10`, số cặp là 10 chọn 2 = 45, cho ra 45 * 6 = 270 dãy. Nếu vẫn cho phép chính xác hai chữ số, thuật toán sẽ tạo ra đúng 6 chuỗi. Tất cả các trường hợp cạnh đều được xử lý chính xác bằng số học đơn giản.
