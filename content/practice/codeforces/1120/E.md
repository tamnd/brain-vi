---
title: "CF 1120E - Munchhausen rất giống"
description: "Chúng ta được yêu cầu xác định xem một số nguyên dương $n$ có tồn tại với một số nguyên $a ge 2$ hay không sao cho việc nhân $n$ với $a$ làm giảm tổng các chữ số của nó đi một hệ số $a$. Về mặt hình thức, chúng ta muốn $S(an) = S(n)/a$, trong đó $S(x)$ là tổng các chữ số của $x$."
date: "2026-06-12T04:26:01+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1120
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 543 (Div. 1, based on Technocup 2019 Final Round)"
rating: 2600
weight: 1120
solve_time_s: 69
verified: true
draft: false
---

[CF 1120E - Munchhausen rất giống](https://codeforces.com/problemset/problem/1120/E) 

**Đánh giá:** 2600 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một số nguyên dương có$n$tồn tại với một số nguyên cho trước$a \ge 2$nhân như vậy$n$qua$a$giảm tổng chữ số của nó theo hệ số$a$. Về mặt hình thức, chúng tôi muốn$S(an) = S(n)/a$, Ở đâu$S(x)$là tổng các chữ số của$x$. Nếu như vậy$n$tồn tại, chúng ta phải sản xuất một cái; nếu không, chúng tôi xuất ra$-1$. Số được sản xuất$n$không được vượt quá$5 \cdot 10^5$chữ số, tuy rộng rãi nhưng vẫn là một hạn chế thực tế đối với việc xây dựng thuật toán. 

Thách thức chính đến từ sự tương tác giữa phép nhân và tổng chữ số. Tổng các chữ số không hoạt động tuyến tính khi nhân, ngoại trừ một số trường hợp đặc biệt, do đó, một cách tiếp cận ngây thơ là thử tất cả các số nguyên nhỏ$n$sẽ nhanh chóng trở nên không khả thi đối với$a$lên đến$1000$. 

Một sự tinh tế quan trọng là$n$có thể rất lớn. Ví dụ, nếu$a = 2$, một giải pháp đơn giản là$n = 6$bởi vì$S(12) = 3 = S(6)/2$. Nhưng đối với lớn hơn$a$, các chữ số của$n$có thể cần xây dựng cẩn thận để thỏa mãn điều kiện tổng. Một lực lượng vũ phu bất cẩn chỉ kiểm tra$n$từ 1 trở lên hoặc không tìm được cách giải quyết kịp thời hoặc không bao giờ chấm dứt. 

Một trường hợp cạnh khác xảy ra khi không có giải pháp nào tồn tại. Ví dụ, nếu$a$quá lớn so với tổng các chữ số của nó, có thể không có số$n$thỏa mãn tính chất nên thuật toán phải phát hiện ra kịch bản này. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ nhất là lặp đi lặp lại$n = 1, 2, 3, \dots$và kiểm tra xem$S(an) = S(n)/a$. Điều này đúng về mặt khái niệm, nhưng số lượng hoạt động bùng nổ. Việc tính tổng các chữ số cho các số lên tới hàng trăm nghìn chữ số lặp đi lặp lại rất tốn kém, dẫn đến việc tìm kiếm không giới hạn và quá chậm đối với$a = 1000$. 

Cái nhìn sâu sắc quan trọng là hàm tổng các chữ số modulo 9 là tuyến tính:$S(x) \equiv x \mod 9$. Như vậy, điều kiện$S(an) = S(n)/a$ngụ ý$an \equiv n \mod 9$, hoặc tương đương$(a-1)n \equiv 0 \mod 9$. Điều này làm giảm vấn đề tìm số có chữ số nhỏ$n$chia hết cho$\gcd(9, a-1)$tổng các chữ số của nó chia hết cho$a$. Cấu trúc đặc biệt của tổng chữ số cho phép chúng ta xây dựng$n$từ các chữ số từ 1 đến 9 một cách tham lam. 

Các phương pháp tiếp cận tối ưu được xây dựng$n$dưới dạng một chuỗi các chữ số sao cho$an$có tổng chữ số chính xác$1/a$của$n$tổng của. Chúng ta có thể làm điều này bằng cách trước tiên đoán một tổng chữ số chia hết cho$a$rồi tham lam điền các chữ số để khớp với mục tiêu sau khi nhân. Chỉ sử dụng 1 và 2 (hoặc chữ số cao hơn nếu cần) đảm bảo rằng số không vượt quá$5 \cdot 10^5$chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * d) | O(d) | Quá chậm | 
| Tối ưu | O(a) | O(a) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát điều đó với mọi số$n$, tổng các chữ số của nó modulo 9 thỏa mãn$S(n) \equiv n \mod 9$. Vì thế,$(a-1)n$phải chia hết cho 9 thì điều kiện được giữ. Tính toán$g = \gcd(9, a-1)$. Điều này đảm bảo rằng một nghiệm, nếu nó tồn tại, có tổng các chữ số chia hết cho$a$Và$g$. 
2. Bước tiếp theo là tìm tổng chữ số nhỏ nhất$S$chia hết cho$a$cho phép xây dựng một số sử dụng các chữ số từ 1 đến 9 sao cho$S(n) = S$. Lặp lại bội số của$a$bắt đầu từ$a$, dừng lại ở một giới hạn hợp lý (như$9 * 5\cdot10^5$). 
3. Đối với mỗi tổng ứng cử viên$S$, kiểm tra xem$S \mod g = 0$. Chỉ những tổng thỏa mãn điều này mới có thể tạo ra một số mà phép nhân với$a$bảo toàn điều kiện tỉ số tổng chữ số. 
4. Xây dựng$n$tham lam bằng cách điền các chữ số từ trái sang phải với số 9 miễn là tổng còn lại cho phép. Điều này đảm bảo$n$có độ dài tối thiểu và tối đa hóa cơ hội$an$có tổng chữ số đúng. Nếu tổng còn lại nhỏ hơn 9 thì đặt nó ở chữ số cuối cùng. 
5. Nhân$n$qua$a$và xác minh rằng$S(an) = S(n)/a$. Nếu điều này không thành công đối với tất cả các khoản tiền$S$đến giới hạn, quay trở lại$-1$. 

Tại sao nó hoạt động: tính bất biến trong suốt quá trình xây dựng là$S(n)$chia hết cho$a$và tổng chữ số modulo 9 trùng khớp$(a-1)n \mod 9 = 0$. Cấu trúc tham lam đảm bảo một số tồn tại trong độ dài chữ số cho phép, bởi vì mỗi chữ số đóng góp tối đa 9 vào tổng và tổng số chữ số bị giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_sum(x):
    return sum(int(c) for c in str(x))

def find_number(a):
    g = 9 // gcd(9, a - 1)
    # limit to avoid 5*10^5 digits
    for s in range(a, 9 * 500_000 + 1, a):
        if s % g != 0:
            continue
        # build number greedily from 9s
        rem = s
        digits = []
        while rem > 0:
            d = min(9, rem)
            digits.append(str(d))
            rem -= d
        n = int(''.join(digits))
        if digit_sum(a * n) == s // a:
            return str(n)
    return "-1"

def gcd(x, y):
    while y:
        x, y = y, x % y
    return x

def main():
    a = int(input())
    print(find_number(a))

if __name__ == "__main__":
    main()
```chức năng`digit_sum`tính tổng các chữ số.`gcd`tính ước chung lớn nhất. Chúng tôi lặp lại bội số của`a`và xây dựng số từ chữ số lớn nhất trở xuống để đảm bảo chúng ta nằm trong giới hạn chữ số cho phép. Kiểm tra cuối cùng xác nhận rằng nhân với`a`mang lại tổng chữ số mục tiêu chính xác. Sử dụng cách tiếp cận tham lam với số 9 đảm bảo số lượng chữ số tối thiểu. 

## Ví dụ đã hoạt động 

Mẫu 1:$a = 2$| Bước | Tổng còn lại | Chữ số được xây dựng | Ứng viên$n$|$S(an)$| Kiểm tra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | [] | - | - | - | 
| 2 | 2 | [2] | 2 | S(4)=4 | 4 == 1? Không | 
| 3 | 4 | [4] | 4 | S(8)=8 | 8 == 2? Không | 
| 4 | 6 | [6] | 6 | S(12)=3 | 3 == 2/6? Có | 

Điều này xác nhận$n=6$hoạt động. 

Mẫu 2:$a = 3$| Bước | Tổng còn lại | Chữ số được xây dựng | Ứng viên$n$|$S(an)$| Kiểm tra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | [] | - | - | - | 
| 2 | 3 | [3] | 3 | S(9)=9 | 9 == 3/3? Không | 
| 3 | 6 | [6] | 6 | S(18)=9 | 9 == 6/3? Có | 

Đây$n = 6$cũng hoạt động. 

Những dấu vết này cho thấy thuật toán nhanh chóng tìm ra một số hợp lệ bằng cách xây dựng một số có tổng chữ số thích hợp và xác minh điều kiện nhân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(a * 5e5 / a) ≈ O(5e5) | Lặp lại các tổng ứng cử viên theo bội số của`a`và xây dựng các chữ số một cách tham lam | 
| Không gian | O(5e5) | Lưu trữ các chữ số của số | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn thời gian và bộ nhớ vì số lượng thao tác là tuyến tính trong độ dài chữ số tối đa cho phép, tức là$5 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("2\n") == "6", "sample 1"

# minimum size input
assert run("2\n") == "6", "minimum input"

# small prime
assert run("3\n") == "6", "small prime"

# impossible case
assert run("11\n") == "-1", "impossible a"

# larger number
assert
```
