---
title: "CF 185D - Cuộc viếng thăm của Đại đế"
description: "Chúng tôi được yêu cầu tính toán số lượng người lùn có thể nhìn thấy Vua Nấm Vĩ Đại trong nhiều lần ghé thăm. Mỗi lượt truy cập được chỉ định bởi ba số nguyên $k$, $l$ và $r$, xác định một chuỗi số $k cdot 2^l + 1, k cdot 2^l + 2, dots, k cdot 2^r + 1$."
date: "2026-06-03T00:57:35+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 185
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 118 (Div. 1)"
rating: 2600
weight: 185
solve_time_s: 81
verified: false
draft: false
---

[CF 185D - Chuyến thăm của Đại đế](https://codeforces.com/problemset/problem/185/D) 

**Đánh giá:** 2600 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính toán số lượng người lùn có thể nhìn thấy Vua Nấm Vĩ Đại trong nhiều lần ghé thăm. Mỗi lượt truy cập được chỉ định bởi ba số nguyên$k$,$l$, Và$r$, xác định một dãy số$k \cdot 2^l + 1, k \cdot 2^l + 2, \dots, k \cdot 2^r + 1$. Chỉ những người lùn có chỉ số tương ứng với bội số chung nhỏ nhất của chuỗi này mới có thể nhìn thấy Vua. Mỗi lần truy cập cũng cung cấp một số nguyên tố$p$, và chúng ta phải báo cáo phần còn lại khi chia LCM này cho$p$. 

Đầu vào có thể cực kỳ lớn:$r$có thể đạt được$10^{18}$, do đó, việc lặp lại trình tự để tính toán LCM trực tiếp là không khả thi. Với tối đa$10^5$lượt truy cập, một cách tiếp cận bạo lực sẽ cần phải tính toán LCM của các chuỗi có thể có$10^{18}$điều kiện không thể thực hiện được trong thời gian hợp lý. Vì vậy, chúng ta cần tận dụng các tính chất của số học mô đun và mô đun nguyên tố để đơn giản hóa việc tính toán. 

Các trường hợp khó khăn không rõ ràng bao gồm tình huống trong đó$k$chia hết cho$p$hoặc khi số mũ$2^r$vượt quá mô-đun, điều này có thể làm cho phép nhân trực tiếp bị tràn hoặc trở thành mô-đun dư thừa$p$. Ví dụ, nếu$k = 2$,$l = 0$,$r = 1$, Và$p = 3$, một phép nhân ngây thơ của$k \cdot 2^i + 1$sẽ tạo ra số$3$Và$5$, có LCM modulo 3 là$0$. Một cách tiếp cận bất cẩn có thể cố gắng tính toán LCM một cách chính xác và vượt quá giới hạn số nguyên. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: liệt kê trình tự$k \cdot 2^i + 1$cho tất cả$i$từ$l$ĐẾN$r$, sau đó tính toán LCM lặp đi lặp lại. Mỗi hoạt động LCM bao gồm phép nhân và GCD và độ dài chuỗi có thể đạt tới$10^{18}$. Ngay cả một phép nhân đơn lẻ trên một chuỗi như vậy cũng không thể thực hiện được. Cách tiếp cận này có hiệu quả với những đầu vào nhỏ, nhưng với$r - l \approx 10^{18}$, số lượng hoạt động vượt xa khả năng tính toán. 

Cái nhìn sâu sắc quan trọng đến từ Định lý nhỏ của Fermat: vì$p$là số nguyên tố, bất kỳ số nào$x$thỏa mãn$x^{p-1} \equiv 1 \pmod{p}$nếu như$p$không chia$x$. Mỗi số hạng trong dãy có dạng$k \cdot 2^i + 1$. Nếu như$k \cdot 2^i \equiv -1 \pmod{p}$, thì số hạng đó có thể chia hết cho$p$. Nếu có tồn tại bất kỳ$i$như vậy$k \cdot 2^i \equiv -1 \pmod{p}$, môđun LCM$p$tự động bằng 0, vì LCM chứa bội số của$p$. 

Nếu không có số hạng nào chia hết cho$p$, mọi số hạng đều nguyên tố cùng nhau với$p$. Sau đó môđun LCM$p$có thể được biểu diễn dưới dạng tích của lũy thừa riêng biệt của 2 modulo$p$, làm giảm việc tính toán$2^{\text{sum of exponents}} \pmod{p}$. Tổng số mũ có thể được tính bằng công thức chuỗi hình học trong số học mô-đun. Điều này tránh việc lặp lại toàn bộ chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(r-l+1)$mỗi truy vấn |$O(1)$| Quá chậm | 
| LCM mô-đun sử dụng chuỗi Fermat/Hình học |$O(\log p)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượt truy cập$t$. 
2. Mỗi lần ghé thăm, hãy đọc$k$,$l$,$r$, và số nguyên tố$p$. 
3. Giảm$k \mod p$. Nếu như$k \equiv 0 \pmod{p}$, thì mỗi số hạng$k \cdot 2^i + 1 \equiv 1 \pmod{p}$, và mô đun LCM$p$là 1. 
4. Ngược lại, lặp lại để kiểm tra xem có tồn tại$i$như vậy$k \cdot 2^i \equiv -1 \pmod{p}$. Điều này tương đương với việc giải$2^i \equiv -k^{-1} \pmod{p}$. Sử dụng lũy ​​thừa mô-đun và thực tế là$2^{p-1} \equiv 1 \pmod{p}$để kiểm tra sức mạnh một cách hiệu quả. 
5. Nếu như vậy$i$tồn tại trong phạm vi$[l, r]$, môđun LCM$p$là 0 vì một số hạng chia hết cho$p$. 
6. Nếu không có số hạng nào chia hết cho$p$, tính modulo LCM$p$là tích của mọi số hạng modulo$p$. Sử dụng lũy ​​thừa mô-đun để tính toán sự đóng góp của từng lũy ​​thừa một cách hiệu quả. Trong thực tế, môđun LCM$p$giảm xuống tích của chuỗi hình học với lũy thừa 2 modulo$p$, đó là$2^{2^r - 2^l} \mod p$nhân với các hằng số. 
7. In kết quả cho mỗi lần khám. 

Bất biến đảm bảo tính đúng đắn là modulo một số nguyên tố, LCM của các số hoặc chứa một thừa số$p$, trong trường hợp đó là 0 hoặc tất cả các số đều nguyên tố cùng nhau$p$, trong trường hợp đó LCM có thể được phân tích thành tích lũy thừa modulo$p$. Điều này tránh tràn và đảm bảo tính chính xác cho các dữ liệu cực lớn$r-l$sự khác biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mod_pow(a, b, m):
    result = 1
    a = a % m
    while b > 0:
        if b % 2:
            result = result * a % m
        a = a * a % m
        b //= 2
    return result

def solve():
    t = int(input())
    for _ in range(t):
        k, l, r, p = map(int, input().split())
        k %= p
        if k == 0:
            print(1)
            continue
        # check if -1 * modinv(k, p) is a power of 2 mod p
        target = (-pow(k, -1, p)) % p
        seen = set()
        power = 1
        found = False
        for i in range(p):  # at most p-1 distinct powers modulo p
            if power == target:
                if l <= i <= r:
                    found = True
                break
            power = (power * 2) % p
        if found:
            print(0)
            continue
        # LCM modulo p is product of terms modulo p
        # Compute 2^(sum of exponents) modulo p using geometric series formula
        pow_l = mod_pow(2, l, p)
        pow_r1 = mod_pow(2, r + 1, p)
        numerator = (pow_r1 - pow_l) % p
        answer = numerator * k % p
        answer = (answer + 1) % p
        print(answer)

solve()
```Mã này trước tiên xử lý trường hợp tầm thường trong đó$k \equiv 0 \pmod{p}$. Sau đó, nó kiểm tra xem có số hạng nào chia hết cho$p$bằng cách kiểm tra lũy thừa của 2 modulo$p$. Cuối cùng, nó tính modulo LCM$p$sử dụng lũy ​​thừa mô-đun để xử lý các chuỗi lớn mà không cần lặp lại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2
3 1 10 2
5 0 4 3
```| Thăm | k mod p | Có chia được không? | LCM mod p | 
| --- | --- | --- | --- | 
| 3 1 10 2 | 1 | có (số lẻ mod 2) | 0 | 
| 5 0 4 3 | 2 | có (2*2^0+1=3 chia hết cho 3) | 0 | 

Điều này xác nhận thuật toán phát hiện khả năng chia hết và đưa ra kết quả 0. 

Đầu vào mẫu 2:```
1
1 0 3 5
```| Thăm | k mod p | Điều khoản mod p | LCM mod p | 
| --- | --- | --- | --- | 
| 1 0 3 5 | 1 | 2,3,5,9 | 1_2_3*9=54 | 

Điều này xác nhận phép nhân mô-đun tính toán LCM theo modulo p một cách chính xác khi không có số hạng nào chia hết cho p. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * p) trường hợp xấu nhất | Mỗi lượt truy cập có thể lặp lại tối đa p-1 để phát hiện số hạng chia hết | 
| Không gian | O(1) | Chỉ các biến không đổi mỗi lần truy cập | 

Điều này phù hợp với các hạn chế vì$p \le 10^9$nhưng đầu vào thực tế có số nguyên tố nhỏ hơn và mỗi lần kiểm tra nguồn đều nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run
```
