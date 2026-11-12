---
title: "CF 1658B - Hoán vị Marin và Anti-coprime"
description: "Chúng ta được yêu cầu đếm các hoán vị có độ dài $n$ trong đó ước chung lớn nhất của dãy $1 cdot p1, 2 cdot p2, dots, n cdot pn$ hoàn toàn lớn hơn 1. Ở đây $p$ là một hoán vị của các số nguyên từ 1 đến $n$."
date: "2026-06-10T03:21:42+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1658
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 779 (Div. 2)"
rating: 800
weight: 1658
solve_time_s: 94
verified: true
draft: false
---

[CF 1658B - Hoán vị Marin và Anti-coprime](https://codeforces.com/problemset/problem/1658/B) 

**Đánh giá:** 800 
**Tags:** tổ hợp, toán, lý thuyết số 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các hoán vị của độ dài$n$trong đó ước chung lớn nhất của dãy$1 \cdot p_1, 2 \cdot p_2, \dots, n \cdot p_n$hoàn toàn lớn hơn 1. Ở đây$p$là hoán vị của các số nguyên từ 1 đến$n$. Nói một cách đơn giản hơn, mỗi phần tử được nhân với vị trí của nó và chúng tôi muốn tất cả các sản phẩm đó có chung một thừa số lớn hơn 1. 

Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định một$n$. Đầu ra là số modulo hoán vị "đẹp"$998244353$. 

Những ràng buộc cho phép$n$lên tới 1000 và lên tới 1000 trường hợp thử nghiệm. Một giải pháp đơn giản tạo ra tất cả các hoán vị sẽ có độ phức tạp$O(n!)$, điều này là không thể ngay cả đối với$n = 10$. Chúng ta cần một cách tiếp cận có hiệu quả trong khoảng$O(n^2)$hoặc$O(n \log n)$mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh dễ bị bỏ lỡ bao gồm rất nhỏ$n$. Vì$n=1$, hoán vị duy nhất là$[1]$, trong đó có$\gcd(1) = 1$và do đó không đẹp. Vì$n=2$, hoán vị$[2,1]$hoạt động vì$\gcd(2,2) = 2 > 1$. Những điều này cho thấy rằng chúng ta không thể giả định bất kỳ$n > 1$tự động có giải pháp. 

Một điều tinh tế khác là những hoán vị đẹp mắt thường xuất hiện khi chuỗi có thể được sắp xếp lại sao cho tất cả các sản phẩm đều chẵn. Điều này gợi ý một mẫu tổ hợp thay vì kiểm tra từng hoán vị riêng lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả$n!$hoán vị và tính toán$\gcd(1\cdot p_1, \dots, n \cdot p_n)$cho mỗi người. Về nguyên tắc, điều này đúng vì nó trực tiếp thực hiện phát biểu vấn đề, nhưng lại cực kỳ chậm. Ngay cả đối với$n=10$, điều này đòi hỏi 3,6 triệu hoán vị để kiểm tra, nhân với$n$hoạt động mỗi$\gcd$tính toán vượt xa thời gian chạy 1 giây cho phép. 

Cái nhìn sâu sắc quan trọng là đối với$\gcd$vượt quá 1, tất cả các số phải có chung một thừa số. Số nguyên tố nhỏ nhất cần xét là 2. Nếu chúng ta có thể đảm bảo tất cả các tích đều là số chẵn thì$\gcd$ít nhất là 2. Điều này dẫn đến một cách xây dựng đơn giản: gán các số chẵn cho các vị trí làm cho tích của chúng là số chẵn. Bằng cách phân tích, người ta có thể rút ra một phép truy toán đếm các hoán vị trong đó nửa số đầu tiên chiếm các chỉ số chẵn và nửa sau chiếm các chỉ số lẻ, điều chỉnh cho$n$là chẵn hoặc lẻ. Sau một số công việc kết hợp, điều này đơn giản hóa thành$(n//2)!^2$modulo$998244353$thậm chí$n$, và 0 cho số lẻ$n$. 

Quan sát này dựa trên thực tế là các vị trí lẻ nhân với số lẻ sẽ cho ra kết quả lẻ. Vì vậy, đối với$n$số lẻ, ít nhất một sản phẩm vẫn còn số lẻ, đưa ra một$\gcd$của 1. Chỉ chẵn$n$cho phép sắp xếp đầy đủ trong đó tất cả các sản phẩm có chung hệ số 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!·n) | O(n) | Quá chậm | 
| Tối ưu | O(n + t) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa đến mức tối đa$n$modulo$998244353$. Điều này cho phép$O(1)$tra cứu giai thừa và tránh tính toán lại cho từng trường hợp thử nghiệm. 
2. Đọc số lượng test case$t$và lặp qua từng$n$. 
3. Đối với mỗi$n$, kiểm tra xem nó có lẻ không. Nếu đúng thì ghi 0 vì ít nhất một sản phẩm sẽ là số lẻ và$\gcd$sẽ là 1. 
4. Nếu$n$là số chẵn, hãy tính số hoán vị đẹp như sau$(n//2)!^2 \mod 998244353$. Điều này tính các cách để gán nửa dưới của số cho vị trí lẻ và nửa trên cho vị trí chẵn. 
5. In kết quả cho từng test. 

Tại sao nó hoạt động: Điều bất biến là chúng ta duy trì tính chất là mọi tích chẵn khi và chỉ khi$n$là số chẵn và các số được phân phối sao cho mỗi sản phẩm có ít nhất một thừa số chẵn. Bằng cách bình phương giai thừa của$n/2$, chúng tôi tính toán riêng tất cả các sắp xếp lại hợp lệ cho hai nửa số. Không có sự sắp xếp nào khác có thể tạo ra một kết quả lớn hơn$\gcd$bởi vì việc giới thiệu một sản phẩm kỳ quặc sẽ buộc$\gcd$xuống còn 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAX_N = 1000

# Precompute factorials modulo MOD
fact = [1] * (MAX_N + 1)
for i in range(1, MAX_N + 1):
    fact[i] = fact[i-1] * i % MOD

t = int(input())
for _ in range(t):
    n = int(input())
    if n % 2 != 0:
        print(0)
    else:
        half = n // 2
        res = fact[half] * fact[half] % MOD
        print(res)
```Đầu tiên, mã sẽ tính toán trước các giai thừa để tránh tính toán lặp lại. Đối với mỗi trường hợp thử nghiệm, chúng tôi xử lý số lẻ$n$ngay lập tức bằng cách in 0. Đối với số chẵn$n$, chúng ta tính tích các giai thừa cho mỗi nửa hoán vị, bình phương giai thừa vì sự sắp xếp của hai nửa là độc lập. Modulo$998244353$đảm bảo chúng ta không bị tràn và phù hợp với yêu cầu của bài toán. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`4`: 

| n | n%2 | một nửa | sự thật[một nửa] | kết quả | 
| --- | --- | --- | --- | --- | 
| 4 | 0 | 2 | 2 | 4 | 

Giải thích:`n=4`là chẵn. Một nửa là 2.`2! = 2`. Bình phương cho 4. Có bốn hoán vị đẹp: [2,1,4,3], [2,3,4,1], [4,1,2,3], [4,3,2,1] (có thể sắp xếp lại từng nửa). 

Đầu vào mẫu`3`: 

| n | n%2 | kết quả | 
| --- | --- | --- | 
| 3 | 1 | 0 | 

Giải thích:`n=3`thật kỳ quặc. Mọi hoán vị đều có tích lẻ ở đâu đó, cho ra gcd=1. Do đó 0 hoán vị đẹp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAX_N + t) | Tính toán trước các giai thừa một lần trong O(MAX_N), mỗi trường hợp kiểm thử được xử lý trong O(1) | 
| Không gian | O(MAX_N) | Lưu trữ giai thừa lên tới 1000 | 

Với những hạn chế, điều này là hiệu quả. Tính toán trước các giai thừa đảm bảo tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm. Việc sử dụng bộ nhớ ở mức tối thiểu và dễ dàng phù hợp với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353
    MAX_N = 1000
    fact = [1] * (MAX_N + 1)
    for i in range(1, MAX_N + 1):
        fact[i] = fact[i-1] * i % MOD
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        if n % 2 != 0:
            out.append("0")
        else:
            half = n // 2
            res = fact[half] * fact[half] % MOD
            out.append(str(res))
    return "\n".join(out)

# Provided samples
assert run("7\n1\n2\n3\n4\n5\n6\n1000\n") == "0\n1\n0\n4\n0\n36\n665702330", "sample 1"

# Custom cases
assert run("2\n10\n11\n") == "14400\n0", "even vs odd n"
assert run("1\n2\n") == "1", "smallest even n"
assert run("1\n1000\n") == "665702330", "largest n"
assert run("1\n1\n") == "0", "smallest odd n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 | 14400 | Đếm đúng cho số chẵn n=10 | 
| 11 | 0 | Xuất chính xác 0 cho số lẻ n=11 | 
| 2 | 1 | Tối thiểu thậm chí n | 
| 1000 | 665702330 | Hiệu suất và độ chính xác số học mô-đun cho max n | 
| 1 | 0 | Số lẻ tối thiểu n | 

## Vỏ cạnh 

cho`n=1`, thuật toán xác định ngay`n%2==1`và đầu ra 0. Điều này xử lý chính xác đầu vào nhỏ nhất. Vì`n=2`,`n%2==0`, một nửa=1,`1!*1! = 1`đầu ra 1, khớp với hoán vị đẹp duy nhất`[2,1]`. Vì`n=1000`, các giai thừa được tính toán trước cho phép chúng ta tính toán`500!^2 % MOD`một cách hiệu quả. Thuật toán xử lý các đầu vào chẵn và lẻ một cách rõ ràng, do đó không có hoán vị của số lẻ `n
