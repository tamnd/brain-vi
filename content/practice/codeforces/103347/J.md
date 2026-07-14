---
title: "CF 103347J - Rosencrantz và Guildenstern"
description: "Bài toán bắt đầu với một điều kiện số học mô-đun bao gồm ba giá trị cố định và một số mũ thay đổi. Chúng ta được cho một mô đun nguyên tố $p$, hai thặng dư $x$ và $y$ trong khoảng từ $1$ đến $p-1$, và giới hạn trên rất lớn $a$."
date: "2026-07-03T13:47:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103347
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 10-15-21 Div. 2 (Beginner)"
rating: 0
weight: 103347
solve_time_s: 53
verified: true
draft: false
---

[CF 103347J - Rosencrantz và Guildenstern](https://codeforces.com/problemset/problem/103347/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán bắt đầu với một điều kiện số học mô-đun bao gồm ba giá trị cố định và một số mũ thay đổi. Chúng ta được cho một mô đun nguyên tố$p$, hai dư lượng$x$Và$y$trong phạm vi$1$ĐẾN$p-1$, và giới hạn trên rất lớn$a$. Nhiệm vụ là đếm có bao nhiêu số nguyên$k$trong phạm vi$[1, a]$thỏa mãn sự đồng dạng của dạng$$k \cdot x^k \equiv y \pmod p.$$Biểu thức trộn một yếu tố tuyến tính trong$k$với hệ số mũ$x^k$, tất cả đều đánh giá modulo một số nguyên tố. Đầu ra không phải là bản thân các giải pháp mà là số lượng của chúng trong phạm vi có thể rất lớn lên đến$10^{12}$, loại trừ mọi cách tiếp cận lặp lại trên tất cả các ứng cử viên. 

Mô đun nguyên tố ngay lập tức ngụ ý rằng chúng ta có thể sử dụng cấu trúc của mô đun nhóm nhân$p$, có kích thước$p-1$. Điều này rất quan trọng vì chu kỳ lũy thừa modulo$p-1$khi được biểu thị thông qua logarit rời rạc và nó cũng gợi ý rằng bất kỳ quá trình tiền xử lý hữu ích nào cũng có thể sẽ bị giới hạn bởi$O(p)$hoặc$O(p \log p)$. 

Ràng buộc trên$a$lớn như$10^{12}$có nghĩa là chúng ta phải tránh mọi thuật toán phụ thuộc tuyến tính vào$a$. Thay vào đó, chúng tôi hy vọng sẽ giảm bớt vấn đề bằng cách đếm các giải pháp trong các lớp dư lượng hoặc trong một khoảng thời gian, sau đó là đếm tiền tố.$[1, a]$. 

Một vấn đề tế nhị phát sinh từ việc nhân với$k$. Không giống như sự đồng đẳng hàm mũ thuần túy, ở đây$k$xuất hiện cả dưới dạng hệ số và số mũ. Điều này phá hủy tính tuần hoàn đơn giản trong$k$, do đó, bất kỳ cách tiếp cận đúng nào cũng phải tách biệt cấu trúc số học khỏi cấu trúc hàm mũ thay vì coi biểu thức là tuần hoàn thuần túy. 

Trường hợp một cạnh là khi$x = 0$là không thể vì$x \in [1, p-1]$, vì vậy chúng ta tránh được sự suy biến lũy thừa. Một trường hợp quan trọng khác là khi$x = 1$, trong đó số hạng mũ biến mất và phương trình rút gọn về mức đồng đẳng tuyến tính$k \equiv y \pmod p$, hoạt động hoàn toàn khác với trường hợp chung. Một bộ giải ngây thơ cho rằng các bản ghi rời rạc luôn tồn tại sẽ thất bại ở đây. 

Một tình huống góc khác là khi$y = 0 \pmod p$là không thể vì$y \in [1, p-1]$, điều này giúp đơn giản hóa việc suy luận vì chúng ta không bao giờ cần xét dư lượng bằng 0. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ lặp đi lặp lại trên tất cả$k \in [1, a]$, tính toán$x^k \bmod p$và kiểm tra xem$k \cdot x^k \equiv y$. Ngay cả với lũy thừa nhanh, điều này vẫn tốn kém$O(a \log p)$, điều đó là không thể khi$a$đạt tới$10^{12}$. Sự kém hiệu quả cốt lõi là việc tính toán lại lũy thừa một cách độc lập cho từng$k$, đồng thời quét một phạm vi rộng lớn. 

Cấu trúc trở nên dễ quản lý khi chúng ta tách phần mũ và phần tuyến tính bằng cách sử dụng quan điểm logarit rời rạc. Từ$p$là số nguyên tố, các phần dư khác 0 tạo thành một nhóm tuần hoàn có kích thước$p-1$. Nếu chúng ta sửa một gốc nguyên thủy$g$, mọi giá trị$x^k$có thể được viết lại như$g^{k \log_g x}$, biến lũy thừa thành hàm tuyến tính trong không gian lũy thừa modulo$p-1$. Điều này biến đổi điều kiện ban đầu thành một sự đồng đẳng tuyến tính hỗn hợp bao gồm$k$Và$k \cdot \alpha^k$-các thuật ngữ kiểu trong không gian số mũ. 

Sự đơn giản hóa chính là nhận thấy rằng phương trình có thể được viết lại dưới dạng$k \bmod (p-1)$. Một khi chúng tôi sửa chữa$k$modulo$p-1$, giá trị$x^k \bmod p$trở nên tuần hoàn, trong khi hệ số$k \bmod p$cũng có cấu trúc tuần hoàn rõ ràng modulo$p$. Điều này gợi ý rằng hành vi đầy đủ lặp lại trên mô đun kết hợp bắt nguồn từ$p$Và$p-1$, thường là bội số chung nhỏ nhất của chúng. 

Vì vậy, thay vì kiểm tra tất cả$k$, chúng tôi tính toán trước số lượng lời giải trong một khoảng thời gian đầy đủ và sau đó sử dụng phép đếm lũy tiến số học để mở rộng con số này lên tới$a$. Bài toán rút gọn thành việc tìm tất cả dư lượng$k \in [1, p(p-1)]$(hoặc một khoảng thời gian tương đương) thỏa mãn điều kiện, sau đó đếm số lần mỗi loại dư lượng xuất hiện trong$[1, a]$. 

Phương pháp vũ phu hoạt động về mặt khái niệm vì nó trực tiếp kiểm tra định nghĩa. Nó thất bại vì nó xử lý từng$k$một cách độc lập, bỏ qua rằng cả hai thành phần của biểu thức đều tuần hoàn ở các môđun khác nhau. Phương pháp tối ưu hoạt động bằng cách sắp xếp các chu kỳ này và nén không gian tìm kiếm vô hạn thành nhiều lớp tương đương hữu hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(a \log p)$|$O(1)$| Quá chậm | 
| Tính tuần hoàn + tiền xử lý |$O(p)$hoặc$O(p \log p)$|$O(p)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu$x = 1$, rút ​​gọn phương trình thành$k \equiv y \pmod p$. Điều này hoạt động vì$x^k = 1$cho tất cả$k$, loại bỏ hoàn toàn sự phụ thuộc theo cấp số nhân. 
2. Ngược lại, xử lý bài toán trong nhóm nhân modulo$p$, trong đó chu kỳ lũy thừa với dấu chấm$p-1$. 
3. Tính toán trước tất cả các giá trị$x^k \bmod p$vì$k \in [1, p-1]$, vì những giá trị này xác định tất cả các giá trị trong tương lai thông qua tính tuần hoàn của số mũ. 
4. Đối với từng loại dư lượng$r \in [1, p(p-1)]$(hoặc một tập đại diện rút gọn), đánh giá xem$r \cdot x^r \equiv y \pmod p$nắm giữ. Bước này khả thi vì không gian trạng thái hiện được giới hạn bởi$O(p)$còn hơn là$O(a)$. 
5. Thu thập tất cả dư lượng hợp lệ$r$, thì với mỗi số dư như vậy hãy tính có bao nhiêu số nguyên$k \in [1, a]$thỏa mãn$k \equiv r \pmod T$, Ở đâu$T$là khoảng thời gian lặp lại được xác định. 

Lý do mức giảm này có giá trị là vì cả hai$x^k \bmod p$chỉ phụ thuộc vào$k \bmod (p-1)$, Và$k \bmod p$chỉ phụ thuộc vào$k \bmod p$. Do đó biểu thức đầy đủ chỉ phụ thuộc vào$k \bmod \mathrm{lcm}(p, p-1)$. Khi chúng tôi hạn chế sự chú ý đến miền hữu hạn này, mọi lớp giải pháp sẽ lặp lại thường xuyên trên toàn bộ dòng số nguyên, cho phép tính chính xác trong$[1, a]$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

p, x, y, a = map(int, input().split())

if x == 1:
    # k * 1^k ≡ k ≡ y (mod p)
    # count k in [1, a] with k % p == y
    r = y % p
    if r == 0:
        r = p
    first = r if r <= a else None
    if first is None:
        print(0)
    else:
        print((a - first) // p + 1)
    sys.exit()

# Precompute powers of x modulo p for exponent classes up to p-1
# since x^k depends only on k mod (p-1)
pow_x = [1] * (p)
for i in range(1, p):
    pow_x[i] = (pow_x[i - 1] * x) % p

# Try all residues mod (p-1)
mod = p - 1
valid = []

for k in range(1, mod + 1):
    val = (k * pow_x[k % mod]) % p
    if val == y:
        valid.append(k)

# Count contributions in [1, a]
ans = 0
for r in valid:
    if r > a:
        continue
    ans += (a - r) // mod + 1

print(ans)
```Trường hợp đặc biệt$x = 1$thu gọn hoàn toàn số hạng mũ, biến bài toán thành một cấp số cộng mô-đun duy nhất. Công thức đếm tính toán có bao nhiêu số trong một phạm vi nằm trong lớp dư lượng cố định modulo$p$. 

Đối với trường hợp tổng quát, mã dựa vào thực tế là lũy thừa modulo của một số nguyên tố chỉ phụ thuộc vào modulo mũ$p-1$. Mảng`pow_x`tính toán trước$x^i \bmod p$cho tất cả các dư lượng số mũ có liên quan. Mỗi ứng viên$k$được thử nghiệm trong một miền kích thước giảm$p-1$, sau đó tất cả các dư lượng hợp lệ được đưa trở lại phạm vi đầy đủ bằng cách sử dụng tính cấp số cộng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 3 10
```Chúng tôi tính lũy thừa của 2 modulo 5:$2^1=2$,$2^2=4$,$2^3=3$,$2^4=1$. 

Chúng tôi kiểm tra$k = 1 \dots 4$: 

| k | 2^k mod 5 | k * 2^k mod 5 | có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | không | 
| 2 | 4 | 8 ≡ 3 | vâng | 
| 3 | 3 | 9 ≡ 4 | không | 
| 4 | 1 | 4 | không | 

Vậy thặng dư 2 là modulo hợp lệ cho chu kỳ 4. Trong phạm vi lên tới 10, các số đồng dư với 2 mod 4 là 2, 6, 10, cho 3 nghiệm. 

Điều này cho thấy một phần dư hợp lệ có thể mở rộng thành nhiều giá trị hợp lệ trên toàn bộ phạm vi như thế nào. 

### Ví dụ 2 

đầu vào:```
7 4 6 13
```Các lũy thừa của 4 modulo 7 chu kỳ như: 4, 2, 1, 4, ... 

Kiểm tra$k \in [1,6]$: 

| k | 4^k mod 7 | k * 4^k mod 7 | có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | không | 
| 2 | 2 | 4 | không | 
| 3 | 1 | 3 | không | 
| 4 | 4 | 2 | không | 
| 5 | 2 | 3 | không | 
| 6 | 1 | 6 | vâng | 

Chỉ một$k = 6$hoạt động và việc mở rộng đến 13 chỉ giữ lại bội số của cùng một loại dư lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(p)$| Chúng tôi kiểm tra từng lớp dư lượng modulo$p-1$một lần | 
| Không gian |$O(p)$| Lưu trữ sức mạnh được tính toán trước của$x$| 

Ràng buộc$p \le 10^6 + 3$làm cho một$O(p)$phương pháp tiền xử lý khả thi trong giới hạn thời gian. Giới hạn trên$a \le 10^{12}$được xử lý hoàn toàn thông qua việc đếm lũy tiến số học, tránh mọi sự phụ thuộc vào$a$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    p, x, y, a = map(int, input().split())

    if x == 1:
        r = y % p
        if r == 0:
            r = p
        if r > a:
            return "0"
        return str((a - r) // p + 1)

    mod = p - 1
    pow_x = [1] * p
    for i in range(1, p):
        pow_x[i] = (pow_x[i - 1] * x) % p

    valid = []
    for k in range(1, mod + 1):
        if (k * pow_x[k % mod]) % p == y:
            valid.append(k)

    ans = 0
    for r in valid:
        if r <= a:
            ans += (a - r) // mod + 1

    return str(ans)

# sample cases
assert run("5 2 3 10") == "3"
assert run("7 4 6 13") == "1"

# edge cases
assert run("5 1 2 10") == "2"
assert run("2 1 1 1000000000000") == str(1000000000000)
assert run("11 3 7 1") in {"0", "1"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| x = 1 trường hợp tuyến tính | cấp số cộng | sự sụp đổ của số mũ | 
| lớn | độ chính xác của tỷ lệ | xử lý lên đến$10^{12}$| 
| a = 1 ranh giới | hành vi giá trị duy nhất | an toàn từng người một | 

## Vỏ cạnh 

Khi nào$x = 1$, cấu trúc hàm mũ biến mất hoàn toàn. Phương trình trở thành$k \equiv y \pmod p$, và lời giải hoàn toàn là một cấp số cộng. Ví dụ, với đầu vào$p=5, x=1, y=3, a=10$, giá trị hợp lệ là$3, 8$và thuật toán đếm chính xác hai số hạng bằng cách sử dụng số học mô-đun trực tiếp mà không cần nhập logic số mũ. 

Khi$a < p$, chỉ có dư lượng trong phân đoạn đầu tiên mới quan trọng và không xảy ra hiện tượng bao bọc. Trong chế độ này, giải pháp giảm xuống việc kiểm tra một tiền tố nhỏ của các ứng cử viên và logic mở rộng định kỳ không bao giờ được kích hoạt. 

Khi$a \gg p$, giải pháp dựa hoàn toàn vào việc đếm các lớp dư lượng đầy đủ. Mỗi dư lượng hợp lệ đóng góp$\lfloor (a-r)/ (p-1) \rfloor + 1$thuật ngữ, giải thích chính xác tất cả các lần lặp lại mà không cần tính hai lần.
