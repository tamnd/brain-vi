---
title: "CF 1133D - Tối đa hóa số lượng bằng không"
description: "Cho hai mảng số nguyên a và b, mỗi mảng có n phần tử. Chúng ta được yêu cầu xây dựng một mảng c mới bằng cách sử dụng một số thực d sao cho mỗi phần tử ci bằng d ai + bi. Mục tiêu của chúng ta là chọn d để tối đa hóa số lượng số 0 trong c."
date: "2026-06-12T04:05:05+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1133
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 544 (Div. 3)"
rating: 1500
weight: 1133
solve_time_s: 73
verified: true
draft: false
---

[CF 1133D - Tối đa hóa số lượng bằng 0](https://codeforces.com/problemset/problem/1133/D) 

**Đánh giá:** 1500 
**Tags:** băm, toán, lý thuyết số 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên,`a`Và`b`, mỗi cái có`n`các phần tử. Chúng tôi được yêu cầu xây dựng một mảng mới`c`sử dụng một số thực duy nhất`d`sao cho mỗi phần tử`c_i`bằng`d * a_i + b_i`. Mục tiêu của chúng tôi là lựa chọn`d`để tối đa hóa số lượng số không trong`c`. Đầu ra là số phần tử 0 tối đa có thể đạt được trong`c`. 

Những hạn chế là đáng kể.`n`có thể lên tới 200.000 và`a_i`Và`b_i`mỗi giá trị có thể lớn bằng 10^9 về giá trị tuyệt đối. Điều này loại trừ bất kỳ giải pháp nào kiểm tra tất cả các khả năng có thể`d`giá trị một cách rõ ràng hoặc thử tất cả các cặp trong một vòng lặp lồng nhau. Thuật toán phải chạy trong thời gian gần như tuyến tính hoặc tuyến tính tương ứng với`n`. 

Một trường hợp khó phát hiện khi một số`a_i`các giá trị bằng không. Trong trường hợp đó,`c_i = b_i`độc lập với`d`, vì vậy chúng ta chỉ có thể nhận được số 0 cho vị trí đó nếu`b_i`bản thân nó bằng không. Một trường hợp quan trọng khác là khi có nhiều cặp`(a_i, b_i)`mang lại tỷ lệ tương tự`-b_i / a_i`. Những điều này sẽ góp phần mang lại sự tối ưu tương tự`d`. Việc triển khai bất cẩn mà bỏ qua độ chính xác hoặc giảm phân số không chính xác có thể tính sai những phân số này. 

Ví dụ nhỏ:`a = [0, 2], b = [0, 4]`. Ở đây, phần tử đầu tiên đã bằng 0 bất kể`d`. Phần tử thứ hai trở thành 0 khi`d = -2`. Tối ưu`d`là`-2`và chúng ta nhận được hai số không. Một cách tiếp cận ngây thơ bỏ qua số không trong đó`a_i = 0`sẽ chỉ đếm một số không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là xem xét mọi thực tế`d`điều đó có thể tạo ra một số`c_i`số không, sau đó đếm số không cho số đó`d`. Quan sát điều đó`c_i = 0`ngụ ý`d = -b_i / a_i`(khi`a_i ≠ 0`), một thuật toán đơn giản có thể thử kiểm tra tất cả các ứng cử viên này`d`giá trị và đếm số không trong`O(n^2)`thời gian. Điều này đúng về nguyên tắc nhưng quá chậm: vì`n = 2 * 10^5`, nó có thể yêu cầu 4 * 10^10 thao tác. 

Quan sát quan trọng là`d`được xác định duy nhất bởi tỉ số`-b_i / a_i`. Nếu nhiều chỉ số tạo ra cùng một tỷ lệ, chọn chỉ số đó`d`làm cho tất cả tương ứng`c_i`bằng không cùng một lúc. Vì vậy, chúng ta chỉ cần đếm xem mỗi tỷ lệ duy nhất xuất hiện bao nhiêu lần. Phân số phải được lưu trữ ở dạng rút gọn để tránh sự thiếu chính xác của dấu phẩy động. Không có cặp nào`(a_i = 0, b_i = 0)`có thể được tính riêng vì chúng là số 0 cho bất kỳ`d`. 

Điều này làm giảm vấn đề băm các phân số này và đếm số lần xuất hiện. Mỗi phân số có thể được biểu diễn dưới dạng`(numerator, denominator)`ở dạng rút gọn sử dụng ước số chung lớn nhất, đồng thời đảm bảo mẫu số dương để đảm bảo tính nhất quán. Khi chúng ta có số đếm, tần số tối đa cộng với số lượng`a_i = 0, b_i = 0`đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log(max( | a_i | , | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`ratio_count`để lưu trữ tần số của từng phân số rút gọn đại diện cho`-b_i / a_i`. Đồng thời khởi tạo`zero_count`để đếm các vị trí ở đó`a_i = 0`Và`b_i = 0`. 
2. Lặp lại từng cặp`(a_i, b_i)`. Nếu như`a_i`bằng không và`b_i`bằng 0, tăng`zero_count`. Nếu như`a_i`bằng không và`b_i`không phải là số 0, bỏ qua vì không`d`có thể làm cho điều này bằng không. 
3. Đối với khác không`a_i`, tính phân số`-b_i / a_i`. Hãy rút gọn phân số này về dạng đơn giản nhất bằng cách sử dụng ước số chung lớn nhất. Chuẩn hóa các dấu sao cho mẫu số dương, đảm bảo hàm băm nhất quán. 
4. Sử dụng bộ dữ liệu`(numerator, denominator)`như một chiếc chìa khóa trong quầy`ratio_count`và tăng số lượng cho khóa đó. 
5. Câu trả lời cuối cùng là số lượng tối đa trong`ratio_count`cộng thêm`zero_count`. Nếu như`ratio_count`trống (tất cả`a_i = 0`), câu trả lời là`zero_count`. 

Tại sao nó hoạt động: mỗi phân số đại diện cho chính xác một ứng cử viên`d`điều đó tạo nên sự tương ứng`c_i`không. Bằng cách đếm số lần mỗi ứng viên`d`xảy ra, chúng tôi tìm thấy`d`tối đa hóa số không. Không có phần tử độc lập với`d`được thêm vào một cách riêng biệt. Không có ứng cử viên nào bị bỏ sót và không có phân số nào được tính hai lần do chuẩn hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd
from collections import Counter

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

ratio_count = Counter()
zero_count = 0

for ai, bi in zip(a, b):
    if ai == 0:
        if bi == 0:
            zero_count += 1
        continue
    num = -bi
    den = ai
    g = gcd(num, den)
    num //= g
    den //= g
    if den < 0:
        num = -num
        den = -den
    ratio_count[(num, den)] += 1

max_ratio = max(ratio_count.values(), default=0)
print(max_ratio + zero_count)
```Giải pháp này trước tiên xử lý các số không trong đó`a_i = 0`. Sau đó, nó cẩn thận giảm phân số và chuẩn hóa các dấu hiệu để đảm bảo các khóa nhất quán. Sử dụng một`Counter`theo dõi tần số một cách hiệu quả. trận chung kết`max`cuộc gọi tìm thấy ứng cử viên tối ưu`d`. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
5
1 2 3 4 5
2 4 7 11 3
```| tôi | a_i | b_i | num=-b_i | den=a_i | giảm | tỷ lệ_đếm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | -2 | 1 | (-2,1) | 1 | 
| 2 | 2 | 4 | -4 | 2 | (-2,1) | 2 | 
| 3 | 3 | 7 | -7 | 3 | (-7,3) | 1 | 
| 4 | 4 | 11 | -11 | 4 | (-11,4) | 1 | 
| 5 | 5 | 3 | -3 | 5 | (-3,5) | 1 |`zero_count = 0`,`max_ratio = 2`, đầu ra`2`. Điều này khẳng định rằng việc lựa chọn`d = -2`số không ở hai vị trí đầu tiên. 

Mẫu 2: 

đầu vào:```
3
0 1 2
0 3 6
```| tôi | a_i | b_i | số không | tỷ lệ_đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | - | 
| 2 | 1 | 3 | 0 | (-3,1)=1 | 
| 3 | 2 | 6 | 0 | (-3,1)=2 |`zero_count=1`,`max_ratio=2`, đầu ra`3`. Việc đếm phân số cộng với xử lý số 0 hoạt động chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(max( | a_i | 
| Không gian | O(n) | Chúng tôi lưu trữ tối đa n phân số duy nhất trong bộ đếm. | 

Với`n ≤ 2 * 10^5`, độ phức tạp về thời gian có thể chấp nhận được trong vòng 2 giây. Việc sử dụng bộ nhớ là tuyến tính, nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    from math import gcd
    from collections import Counter
    ratio_count = Counter()
    zero_count = 0
    for ai, bi in zip(a, b):
        if ai == 0:
            if bi == 0:
                zero_count += 1
            continue
        num, den = -bi, ai
        g = gcd(num, den)
        num //= g
        den //= g
        if den < 0:
            num = -num
            den = -den
        ratio_count[(num, den)] += 1
    max_ratio = max(ratio_count.values(), default=0)
    return str(max_ratio + zero_count)

# provided samples
assert run("5\n1 2 3 4 5\n2 4 7 11 3\n") == "2", "sample 1"
assert run("3\n0 1 2\n0 3 6\n") == "3", "custom zero + fraction overlap"
# minimum-size input
assert run("1\n0\n0\n") == "1", "single zero"
assert run("1\n1\n2\n") == "1", "single non
```
