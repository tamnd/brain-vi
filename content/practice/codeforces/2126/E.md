---
title: "CF 2126E - G-C-D, xui xẻo!"
description: "Chúng ta có hai dãy số nguyên p và s, mỗi dãy có độ dài n. Mảng p đại diện cho các GCD tiền tố của một số mảng a chưa biết và mảng s đại diện cho các GCD hậu tố của cùng một mảng. Nhiệm vụ là quyết định xem mảng a như vậy có tồn tại hay không."
date: "2026-06-08T03:23:06+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2126
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1037 (Div. 3)"
rating: 1400
weight: 2126
solve_time_s: 78
verified: true
draft: false
---

[CF 2126E - G-C-D, xui xẻo!](https://codeforces.com/problemset/problem/2126/E) 

**Đánh giá:** 1400 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên,`p`Và`s`, mỗi chiều dài`n`. Mảng`p`đại diện cho các GCD tiền tố của một số mảng không xác định`a`và mảng`s`đại diện cho hậu tố GCD của cùng một mảng. Nhiệm vụ là quyết định xem một mảng như vậy có`a`tồn tại. Về mặt hình thức, nếu`a`tồn tại thì với mọi chỉ mục`i`:```
p[i] = gcd(a[1], a[2], ..., a[i])
s[i] = gcd(a[i], a[i+1], ..., a[n])
```Đầu vào bao gồm tối đa 10^4 trường hợp thử nghiệm và tổng của tất cả`n`giá trị trên các trường hợp thử nghiệm tối đa là 10^5. Điều này có nghĩa là chúng ta phải xử lý từng trường hợp kiểm thử một cách hiệu quả trong khoảng thời gian khoảng O(n), vì mọi trường hợp O(n^2) sẽ quá chậm. 

Phần không rõ ràng của vấn đề xuất phát từ những hạn chế trong việc truyền bá GCD. Ví dụ: mảng tiền tố GCD`p`phải không tăng cho đến phần tử đầu tiên là 1. Tương tự,`s`phải không tăng từ phần tử cuối cùng về phía sau. Một cách tiếp cận ngây thơ có thể cố gắng xây dựng lại tất cả các mảng ứng cử viên`a`và kiểm tra xem họ có tạo ra cái đã cho không`p`Và`s`, nhưng điều này là không khả thi đối với quy mô lớn`n`. Các trường hợp cạnh nhỏ bao gồm các mảng trong đó tất cả`p`giá trị bằng nhau hoặc tất cả`s`các giá trị bằng nhau và các mảng có`1`trong đó ảnh hưởng đến việc truyền bá GCD. Ví dụ, nếu`p = [2, 2, 2]`Và`s = [2, 2, 1]`, câu trả lời là "Không" vì không có mảng nào thỏa mãn cả GCD tiền tố và hậu tố ở tất cả các vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thử mọi mảng có thể`a`phù hợp với`p`Và`s`. Tại mỗi vị trí`i`, chúng ta có thể thử bất kỳ số nguyên nào chia hết cho`p[i]`Và`s[i]`trong khi đáp ứng tiền tố và hậu tố GCD đang chạy. Về nguyên tắc, điều này đúng nhưng cực kỳ chậm, vì số lượng mảng có thể tăng lên theo kiểu tổ hợp với`n`. Với`n`lên tới 10^5, cách tiếp cận này không thực tế. 

Cái nhìn sâu sắc quan trọng là giá trị tại vị trí`i`trong mảng`a`phải là bội số của cả tiền tố GCD trước đó (hoặc`p[i]`chính nó) và hậu tố tiếp theo là GCD (hoặc`s[i]`). Cụ thể, chúng ta có thể định nghĩa`a[i]`là bội số chung nhỏ nhất (LCM) của`p[i]`Và`s[i]`. Từ`p[i]`chia tiền tố GCD và`s[i]`chia hậu tố GCD, chọn`a[i] = lcm(p[i], s[i])`đảm bảo rằng cả hai GCD đều được bảo toàn ở vị trí đó. Sau khi xây dựng mảng ứng cử viên này, chúng ta có thể tính toán lại các GCD tiền tố và hậu tố và xác minh rằng chúng khớp với nhau.`p`Và`s`. Điều này làm giảm vấn đề xuống còn quét tuyến tính với số học đơn giản ở mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng ứng viên`a`chiều dài`n`. Đối với mỗi chỉ số`i`, bộ`a[i] = lcm(p[i], s[i])`. Lý do là đây là số tối thiểu chia hết cho cả hai`p[i]`Và`s[i]`, bảo toàn tính phân chia cần thiết cho các GCD tiền tố và hậu tố. 
2. Tính mảng tiền tố GCD`p_check`từ`a`. Bắt đầu với`p_check[0] = a[0]`, thì với mỗi`i`từ 1 đến`n-1`, bộ`p_check[i] = gcd(p_check[i-1], a[i])`. 
3. Tính mảng hậu tố GCD`s_check`từ`a`. Bắt đầu với`s_check[n-1] = a[n-1]`, thì với mỗi`i`từ`n-2`xuống 0, đặt`s_check[i] = gcd(s_check[i+1], a[i])`. 
4. So sánh`p_check`với`p`Và`s_check`với`s`. Nếu cả hai mảng khớp chính xác, hãy in "CÓ"; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: Bất biến là vậy`lcm(p[i], s[i])`có thể chia hết cho cả tiền tố và hậu tố GCD tại chỉ mục`i`. Bởi vì GCD có tính kết hợp và giao hoán, nếu mảng ứng cử viên duy trì khả năng chia hết ở mỗi vị trí, tiền tố và hậu tố GCD được tính toán lại sẽ chỉ khớp với mảng ban đầu nếu tồn tại một mảng hợp lệ. Bất kỳ sự khác biệt nào cho thấy rằng không có mảng nào như vậy có thể đáp ứng cả hai`p`Và`s`đồng thời. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        s = list(map(int, input().split()))
        a = [0] * n
        for i in range(n):
            a[i] = (p[i] * s[i]) // math.gcd(p[i], s[i])
        p_check = [0] * n
        s_check = [0] * n
        p_check[0] = a[0]
        for i in range(1, n):
            p_check[i] = math.gcd(p_check[i-1], a[i])
        s_check[-1] = a[-1]
        for i in range(n-2, -1, -1):
            s_check[i] = math.gcd(s_check[i+1], a[i])
        if p_check == p and s_check == s:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    main()
```Giải pháp xây dựng mảng ứng cử viên bằng cách sử dụng LCM, sau đó xác minh các GCD tiền tố và hậu tố. Chúng tôi sử dụng phép chia số nguyên để tính toán LCM một cách hiệu quả và tránh các lỗi dấu phẩy động. Các điều kiện biên như mảng có độ dài 1 hoặc giá trị bằng 1 được xử lý một cách tự nhiên vì`lcm(x, x) = x`Và`gcd(x, x) = x`. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
6
72 24 3 3 3 3
3 3 3 6 12 144
```| tôi | p[i] | s[i] | a[i] = lcm(p[i], s[i]) | tiền tố GCD | hậu tố GCD | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 72 | 3 | 72 | 72 | 3 | 
| 1 | 24 | 3 | 24 | 24 | 3 | 
| 2 | 3 | 3 | 3 | 3 | 3 | 
| 3 | 3 | 6 | 6 | 3 | 6 | 
| 4 | 3 | 12 | 12 | 3 | 12 | 
| 5 | 3 | 144 | 144 | 3 | 144 | 

Sau khi tính toán tiền tố và hậu tố GCD, cả hai đều khớp với mảng đầu vào. Đầu ra: CÓ. 

**Mẫu 2** 

đầu vào:```
1 2 3
4 5 6
```Tính toán`a[i] = lcm(p[i], s[i])`cho`[4, 10, 6]`. Tiền tố GCD:`[4, 2, 2]`không khớp`[1, 2, 3]`. Đầu ra: KHÔNG. Điều này chứng tỏ rằng ứng cử viên dựa trên LCM duy trì các ràng buộc về khả năng chia hết và sự không khớp sẽ phát hiện ra điều không thể xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Việc xây dựng mảng ứng cử viên và GCD tiền tố/hậu tố tính toán là tuyến tính trong`n`. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lưu trữ mảng`a`,`p_check`, Và`s_check`. | 

Xét tổng của tất cả`n`10^5, tổng số thao tác là khoảng 3 × 10^5 cho mỗi lần chạy thử, thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# provided samples
assert run("""5
6
72 24 3 3 3 3
3 3 3 6 12 144
3
1 2 3
4 5 6
5
125 125 125 25 25
25 25 25 25 75
4
123 421 282 251
125 1981 239 223
3
124 521 125
125 121 121
""") == "YES\nNO\nYES\nNO\nNO", "provided samples"

# custom cases
assert run("""1
1
10
10
""") == "YES", "single element, equal"

assert run("""1
3
5 5
```
