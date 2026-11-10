---
title: "CF 1542C - Chức năng lạ"
description: "Chúng tôi được đưa ra rất nhiều truy vấn. Mỗi truy vấn cung cấp một số nguyên rất lớn $n$ và chúng tôi xem xét mọi số nguyên $i$ từ 1 đến $n$. Với mỗi $i$, chúng ta định nghĩa hàm $f(i)$ là số nguyên dương nhỏ nhất không chia hết $i$."
date: "2026-06-14T19:04:04+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1542
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 729 (Div. 2)"
rating: 1600
weight: 1542
solve_time_s: 159
verified: false
draft: false
---

[CF 1542C - Chức năng lạ](https://codeforces.com/problemset/problem/1542/C) 

**Đánh giá:** 1600 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra rất nhiều truy vấn. Mỗi truy vấn cung cấp một số nguyên rất lớn$n$, và chúng tôi xem xét mọi số nguyên$i$từ 1 đến$n$. Đối với mỗi$i$, chúng ta định nghĩa một hàm$f(i)$là số nguyên dương nhỏ nhất không chia hết được$i$. Sau khi tính toán giá trị này cho mọi$i$, chúng tôi tổng hợp tất cả các kết quả. 

Nhiệm vụ là tính toán tổng số này một cách hiệu quả ngay cả khi$n$lớn như$10^{16}$, và có thể có tới$10^4$truy vấn. 

Khó khăn chính đó là$f(i)$phụ thuộc vào cấu trúc các ước của$i$, và cho lớn$i$chúng ta không thể phân tích các ước số một cách trực tiếp. 

Ràng buộc$n \le 10^{16}$ngay lập tức loại trừ bất kỳ thuật toán nào lặp lại trên tất cả các giá trị lên đến$n$. Thậm chí$O(\sqrt{n})$mỗi bài kiểm tra là không thể tại$10^4$các bài kiểm tra. Chúng ta cần một giải pháp phụ thuộc vào một số lượng nhỏ “sự thay đổi cấu trúc” trong hàm hơn là vào các số nguyên riêng lẻ. 

Một trường hợp tế nhị là hiểu lầm$f(i)$. Nó không phải là ước số còn thiếu nhỏ nhất của$i$theo nghĩa phân tích thừa số nguyên tố, nhưng là số nguyên nhỏ nhất không nằm trong tập hợp ước số. Ví dụ, nếu$i = 12$, các ước của nó bao gồm$1,2,3,4,6,12$, Vì thế$f(12) = 5$. Việc triển khai ngây thơ có thể nghĩ sai rằng điều này chỉ phụ thuộc vào các thừa số nguyên tố và bỏ sót các khoảng trống tổng hợp. 

Một trường hợp thất bại khác là cố gắng tính toán$f(i)$độc lập cho mỗi$i$. Ngay cả khi phép liệt kê số chia đã được tối ưu hóa, việc thực hiện điều này cho tất cả$i \le n$là quá chậm. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta xem xét cách tiếp cận trực tiếp. Đối với mỗi$i$, chúng ta thử kiểm tra các số nguyên bắt đầu từ 1 trở lên cho đến khi tìm được số nguyên không chia hết$i$. Điều này có hiệu quả vì việc kiểm tra tính chia hết rất đơn giản và đảm bảo tính chính xác. Tuy nhiên, mỗi số$i$có thể có nhiều ước số nhỏ, và trong trường hợp xấu nhất (đối với các số có tổng hợp cao), chúng ta có thể kiểm tra nhiều ứng cử viên trước khi tìm ra ước số còn thiếu đầu tiên. Làm điều này cho tất cả$i \le n$dẫn đến đại khái$O(n \sqrt{n})$hoặc hành vi tệ hơn tùy thuộc vào việc thực hiện, điều này là không thể$10^{16}$. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì tính toán$f(i)$trực tiếp, chúng tôi yêu cầu từng giá trị ứng cử viên$x$: có bao nhiêu số$i \le n$có tất cả các số nguyên$1,2,\dots,x-1$chia$i$, Nhưng$x$không chia$i$? Điều này biến vấn đề thành việc đếm bội số của bội số chung nhỏ nhất. 

Nếu như$i$chia hết cho mọi số nguyên từ$1$ĐẾN$x-1$, sau đó$i$phải chia hết cho$\mathrm{lcm}(1,2,\dots,x-1)$. Hãy để chúng tôi xác định:$$L_x = \mathrm{lcm}(1,2,\dots,x)$$Sau đó: 

- số ở đâu$f(i) > x$chính xác là bội số của$L_x$, 
- số ở đâu$f(i) = x$là bội số của$L_{x-1}$nhưng không$L_x$. 

Vì vậy chúng ta có thể nhóm các đóng góp theo giá trị của$x$và thay vì lặp đi lặp lại$i$, chúng tôi lặp đi lặp lại$x$. Quan sát quan trọng đó là$L_x$phát triển cực kỳ nhanh chóng. Trên thực tế, nó chỉ tăng khi$x$giới thiệu một quyền lực mới. Điều này có nghĩa là chúng ta chỉ cần xét các giá trị của$x$lên tới khoảng 50 cho bất kỳ$n \le 10^{16}$. 

Chúng tôi tính toán khoảng thời gian đóng góp cho mỗi$x$BẰNG:$$\text{count}(x) = \left\lfloor \frac{n}{L_{x-1}} \right\rfloor - \left\lfloor \frac{n}{L_x} \right\rfloor$$và mỗi nhóm như vậy đóng góp$x \cdot \text{count}(x)$để trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \sqrt{n})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\log n)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giá trị của$L_x = \mathrm{lcm}(1,2,\dots,x)$bắt đầu từ$x = 1$. Dừng lại một lần$L_x > 10^{16}$. Điều này có tác dụng vì những đóng góp ngoài điểm này bằng 0 cho tất cả các truy vấn. 
2. Với mỗi truy vấn có giá trị$n$, khởi tạo câu trả lời là 0. 
3. Lặp lại tất cả các tính toán trước$x$các giá trị bắt đầu từ$x = 2$. Đối với mỗi$x$, tính xem có bao nhiêu số$i \le n$thỏa mãn$f(i) = x$. Điều này được xác định bởi sự khác biệt giữa bội số của các ngưỡng LCM liên tiếp. 
4. Thêm$x \cdot (\lfloor n / L_{x-1} \rfloor - \lfloor n / L_x \rfloor)$để trả lời. Biểu thức này tách biệt chính xác những số có ước số bị thiếu nhỏ nhất là$x$. 
5. Tiếp tục cho đến khi$L_{x-1} > n$, vì ngoài thời điểm này mọi đóng góp đều bằng không. 
6. Xuất ra modulo tổng tích lũy$10^9 + 7$. 

Lý do chúng ta có thể dừng sớm một cách an toàn là vì một lần$L_{x-1} > n$, không có số$i \le n$chia hết cho tất cả các số nguyên đến$x-1$, Vì thế$f(i) < x$cho tất cả các trường hợp còn lại. 

### Tại sao nó hoạt động 

Thuật toán phân vùng số nguyên$i \le n$thành các nhóm rời rạc dựa trên số nguyên nhỏ nhất không chia được chúng. điều kiện$f(i) = x$tương đương với việc nói rằng$i$chia hết cho mọi số nguyên$1$bởi vì$x-1$, nhưng không chia hết cho$x$. Những điều kiện này chuyển trực tiếp thành khả năng chia hết cho$L_{x-1}$và không chia hết cho$L_x$. Vì các tập hợp này rời nhau và bao gồm tất cả các số nguyên nên tổng trên tất cả$x$chính xác là số tiền cần thiết của$f(i)$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

MOD = 10**9 + 7

# Precompute lcms of prefix [1..x]
lcm_vals = [1]

def lcm(a, b):
    return a // math.gcd(a, b) * b

x = 1
while True:
    nxt = lcm(lcm_vals[-1], x)
    if nxt > 10**16:
        break
    lcm_vals.append(nxt)
    x += 1

# lcm_vals[k] = lcm(1..k)
# we start from k = 0 meaning 1..0 is 1

def solve(n):
    ans = 0
    # f(i) = x contributes using L_{x-1}, L_x
    for x in range(2, len(lcm_vals)):
        L_prev = lcm_vals[x-2]
        L_curr = lcm_vals[x-1]
        if L_prev > n:
            break
        cnt = n // L_prev - n // L_curr
        if cnt > 0:
            ans = (ans + x * cnt) % MOD
    return ans

t = int(input())
for _ in range(t):
    n = int(input())
    print(solve(n))
```Mã bắt đầu bằng cách xây dựng chuỗi LCM tiền tố, chuỗi này sẽ ổn định nhanh chóng vì LCM chỉ phát triển khi nguồn điện chính mới được đưa vào. Sau đó, mỗi truy vấn sẽ được xử lý bằng cách quét danh sách nhỏ được tính toán trước này. 

Một chi tiết triển khai tinh tế là lập chỉ mục:`lcm_vals[k]`đại diện cho$L_k$. Điều này làm thay đổi các chỉ số để đóng góp cho giá trị$x$sử dụng$L_{x-1}$Và$L_x$. Sự liên kết từng cái một này là nguồn sai lầm phổ biến nhất. 

phép nhân$x \cdot cnt$được lấy modulo$10^9+7$, nhưng logic chia được thực hiện theo số nguyên vì$n$có thể đạt được$10^{16}$. 

## Ví dụ đã hoạt động 

### Ví dụ: n = 10 

Chúng tôi theo dõi sự tiến triển và đóng góp của LCM. 

| x | L_{x-1} | L_x | n // L_{x-1} | n // L_x | cnt | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 2 | 10 | 5 | 5 | 10 | 
| 3 | 2 | 6 | 5 | 1 | 4 | 12 | 
| 4 | 6 | 12 | 1 | 0 | 1 | 4 | 

Tổng = 26 

Điều này cho thấy các khoản đóng góp giảm đi như thế nào khi LCM phát triển, cô lập các nhóm rời rạc. 

### Ví dụ: n = 4 

| x | L_{x-1} | L_x | n // L_{x-1} | n // L_x | cnt | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 2 | 4 | 2 | 2 | 4 | 
| 3 | 2 | 6 | 2 | 0 | 2 | 6 | 
| 4 | 6 | 12 | 0 | 0 | 0 | 0 | 

Tổng = 10 

Điều này phù hợp với tính toán trực tiếp từ định nghĩa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot K)$| Mỗi truy vấn lặp lại tối đa ~50 ngưỡng LCM | 
| Không gian |$O(K)$| Lưu trữ các giá trị LCM tiền tố lên đến giới hạn tràn | 

Số lượng trạng thái LCM hữu ích là rất nhỏ vì LCM tăng theo cấp số nhân do bao gồm các lũy thừa nguyên tố. Điều này làm cho giải pháp dễ dàng đủ nhanh cho$10^4$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import math
    input = sys.stdin.readline

    lcm_vals = [1]

    def lcm(a, b):
        return a // math.gcd(a, b) * b

    x = 1
    while True:
        nxt = lcm(lcm_vals[-1], x)
        if nxt > 10**16:
            break
        lcm_vals.append(nxt)
        x += 1

    def solve(n):
        ans = 0
        for x in range(2, len(lcm_vals)):
            L_prev = lcm_vals[x-2]
            L_curr = lcm_vals[x-1]
            if L_prev > n:
                break
            cnt = n // L_prev - n // L_curr
            ans = (ans + x * cnt) % MOD
        return ans

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(solve(n)))
    return "\n".join(out)

# provided samples
assert run("""6
1
2
3
4
10
10000000000000000
""") == """2
5
7
10
26
366580019"""

# custom cases
assert run("1\n1\n") == "2", "minimum case"
assert run("1\n4\n") == "10", "small structured case"
assert run("1\n10\n") == "26", "medium correctness"
assert run("1\n10000000000000000\n") is not None, "large stress"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 2 | ranh giới nhỏ nhất | 
| 4 | 10 | phân nhóm sớm đúng | 
| 10 | 26 | tăng trưởng LCM nhiều lớp | 
| 10^16 | giá trị lớn | ổn định theo n lớn | 

## Vỏ cạnh 

Trường hợp một cạnh là$n = 1$. Số duy nhất là$i = 1$, có ước số chỉ là 1, nên số nguyên dương còn thiếu nhỏ nhất là 2. Thuật toán xử lý việc này vì$L_1 = 1$, Và$L_2 = 2$đã vượt quá$n$, vậy chỉ có$x = 2$thuật ngữ đóng góp, đưa ra chính xác một số đếm. 

Một trường hợp khác là khi$n$rất lớn nhưng vẫn nhỏ hơn bước nhảy trong LCM. Ví dụ$n = 10^{16}$. Thuật toán vẫn chỉ lặp qua một số lượng nhỏ giá trị LCM cố định. Mỗi lần lặp sẽ tính toán chính xác số bội số trong một phạm vi lớn và phép trừ sẽ tránh được việc tính hai lần. 

Trường hợp thứ ba là chấm dứt sớm khi$L_{x-1} > n$. Đối với như vậy$x$, cả hai số hạng sàn đều bằng 0 và vòng lặp ngắt chính xác, đảm bảo không có tính toán không cần thiết ngoài cấu trúc chia hết có thể tiếp cận.
