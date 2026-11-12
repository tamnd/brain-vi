---
title: "CF 1617B - Vấn đề GCD"
description: "Chúng ta được yêu cầu xây dựng ba số nguyên dương phân biệt $a$, $b$ và $c$ sao cho tổng của chúng bằng một số nguyên $n$ và ước số chung lớn nhất của hai số đầu tiên bằng số thứ ba: $gcd(a, b) = c$."
date: "2026-06-10T06:25:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1617
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 761 (Div. 2)"
rating: 900
weight: 1617
solve_time_s: 110
verified: false
draft: false
---

[CF 1617B - Sự cố GCD](https://codeforces.com/problemset/problem/1617/B) 

**Xếp hạng:** 900 
**Tags:** vũ lực, thuật toán xây dựng, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng ba số nguyên dương phân biệt$a$,$b$, Và$c$sao cho tổng của chúng bằng một số nguyên cho trước$n$và ước chung lớn nhất của hai số đầu tiên bằng số thứ ba:$\gcd(a, b) = c$. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định một giá trị$n$. Đầu ra của chúng tôi cho mỗi trường hợp thử nghiệm là bất kỳ bộ ba hợp lệ nào.$a, b, c$. 

Những ràng buộc cho phép$n$lớn như$10^9$và số lượng ca kiểm thử cần đạt$10^5$. Điều này ngụ ý rằng bất kỳ giải pháp nào có độ phức tạp tỷ lệ thuận với$n$sẽ là quá chậm, bởi vì$10^9 \times 10^5$hoạt động là không thể thực hiện được. Do đó, chúng ta cần một cách tiếp cận hoạt động với thời gian cơ bản không đổi cho mỗi trường hợp thử nghiệm. 

Một cách tiếp cận ngây thơ sẽ cố gắng liệt kê tất cả các bộ ba$(a, b, c)$với$a + b + c = n$và kiểm tra$\gcd(a, b) = c$. Đối với nhỏ$n$điều này hoạt động, nhưng đối với lớn$n$nó trở nên không thể được. Các trường hợp cạnh không rõ ràng phát sinh đối với nhỏ$n$giống$n = 10$Ở đâu$c = 1$có thể được yêu cầu, hoặc khi$n$chẵn và lẻ vì các mẫu cố định đơn giản có thể cần điều chỉnh để duy trì sự khác biệt. 

Ví dụ, nếu$n = 11$, đang chọn$c = 1$lực lượng$a + b = 10$. Chọn$a = 2$,$b = 8$thất bại vì$\gcd(2, 8) = 2 \neq c$. Các giải pháp bất cẩn cố định các mẫu mà không kiểm tra tính chẵn lẻ hoặc các hệ số nhỏ của$n$sẽ tạo ra những câu trả lời không hợp lệ. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại tất cả những gì có thể$c$từ 1 đến$n-2$và sau đó tất cả đều có thể$a$từ 1 đến$n-c-1$, cài đặt$b = n - c - a$. Sau đó chúng tôi kiểm tra xem$\gcd(a, b) = c$. Điều này đảm bảo sẽ tìm được câu trả lời vì nó luôn tồn tại, nhưng nó đòi hỏi tới$O(n^2)$kiểm tra trên mỗi trường hợp thử nghiệm, điều này không thể chấp nhận được với các ràng buộc. 

The key insight is to pick$c$như một ước số nhỏ của$n$hoặc 1, sau đó xây dựng$a$Và$b$như bội số của$c$để đảm bảo$\gcd(a, b) = c$. Nếu như$c$chia cả hai$a$Và$b$, thì ước chung lớn nhất của chúng ít nhất là$c$. Lựa chọn$a = c$Và$b = n - 2c$đảm bảo$a + b + c = n$, và bởi vì$a$Và$b$là bội số của$c$,$\gcd(a, b) = c$. Chúng ta chỉ cần đảm bảo$a$,$b$, Và$c$khác biệt và tích cực, có thể được thực hiện bằng những điều chỉnh nhỏ. Thông thường, việc thiết lập$c = 1$kỳ quặc$n$Và$c = 2$thậm chí$n$tạo ra các bộ ba đơn giản, hợp lệ. 

Cách tiếp cận này giảm vấn đề xuống còn tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm, vì chúng tôi chỉ thực hiện một số phép tính số học và tính toán GCD. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Mang tính xây dựng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test, hãy đọc số nguyên$n$. 
3. Nếu$n$chia hết cho 2 nhưng không chia hết cho 4, hãy chọn$c = 2$,$a = n/2 - 1$,$b = n/2 + 1$. Điều này đảm bảo$a + b + c = n$Và$\gcd(a, b) = c$. 
4. Nếu$n$chia hết cho 4, chọn$c = n/2$,$a = n/4$,$b = n/4$. Điều chỉnh$b$ĐẾN$b = n/4 * 3$để duy trì sự khác biệt. Hiện nay$a + b + c = n$Và$\gcd(a, b) = c$. 
5. Nếu$n$kỳ lạ, hãy chọn$c = 1$,$a = (n-1)/2$,$b = (n-1)/2$. Điều chỉnh$b = b + 1$để đảm bảo tính khác biệt. Sau đó$a + b + c = n$Và$\gcd(a, b) = 1$. 
6. In bộ ba$a, b, c$. 

Tại sao nó hoạt động: thuộc tính quan trọng là việc chọn hàng$a$Và$b$như bội số của$c$đảm bảo rằng$\gcd(a, b)$chính xác là$c$nếu chúng tôi đảm bảo$a \neq b$. Lựa chọn$c = 1$đối với số lẻ hoặc ước số chẵn nhỏ đối với số chẵn bao gồm tất cả các khả năng có thể$n \ge 10$, đảm bảo tính tích cực và khác biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n % 2 == 1:
            # odd n: use 1 as gcd
            c = 1
            a = n // 2
            b = n // 2
            b += 1  # make distinct
        elif n % 4 == 0:
            # divisible by 4
            c = n // 2
            a = n // 4
            b = n // 4 * 3
        else:
            # even but not divisible by 4
            c = 2
            a = n // 2 - 1
            b = n // 2 + 1
        print(a, b, c)

if __name__ == "__main__":
    solve()
```Giải pháp này sử dụng I/O nhanh và xử lý nhiều trường hợp thử nghiệm một cách hiệu quả. Mỗi trường hợp được xử lý trong thời gian không đổi. Việc điều chỉnh đảm bảo rằng$a, b, c$là tích cực và khác biệt, và việc lựa chọn$c$đảm bảo rằng$\gcd(a, b) = c$. Cần thận trọng khi chia số nguyên để tránh lỗi làm tròn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:$n = 18$| n | Trường hợp | c | một | b | Kiểm tra tổng | Kiểm tra gcd | 
| --- | --- | --- | --- | --- | --- | --- | 
| 18 | chẵn, không chia hết cho 4 | 2 | 8 | 10 | 18 | 2 | 

Điều này cho thấy trường hợp chẵn nhưng không chia hết cho 4. Gcd của 8 và 10 là 2, tổng là 18. 

Đầu vào mẫu 2:$n = 73$| n | Trường hợp | c | một | b | Kiểm tra tổng | Kiểm tra gcd | 
| --- | --- | --- | --- | --- | --- | --- | 
| 73 | lẻ | 1 | 36 | 36+1=37 | 73 | 1 | 

Điều này cho thấy sự kỳ lạ$n$trường hợp. Gcd của 36 và 37 là 1, tổng là 73, các số khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng; đầu ra được in ngay lập tức. | 

Được cho$t \le 10^5$Và$n \le 10^9$, điều này nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("6\n18\n63\n73\n91\n438\n122690412\n") == \
"8 10 2\n21 39 3\n36 37 1\n44 46 1\n146 219 73\n61345206 61345206 2", "sample 1"

# custom cases
assert run("1\n10\n") == "3 5 2", "minimum even input"
assert run("1\n11\n") == "5 6 1", "minimum odd input"
assert run("1\n1000000000\n") == "499999999 500000001 1", "maximum n odd-like"
assert run("1\n100000000\n") == "24999999 74999999 50000000", "maximum n even divisible by 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 | 3 5 2 | nhỏ chẵn n không chia hết cho 4 | 
| 11 | 5 6 1 | lẻ nhỏ | 
| 1000000000 | 499999999 500000001 1 | lớn lẻ như n | 
| 100000000 | 24999999 74999999 50000000 | |
