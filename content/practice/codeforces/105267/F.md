---
title: "CF 105267F - \u9759\u6d41\u7684\u8def\u5f84"
description: "Chúng ta có một số $N$ được mô tả đầy đủ bằng hệ số nguyên tố của nó. Mọi số nguyên tố $pi$ đều xuất hiện với cùng số mũ $m$, vì vậy $N = p1^m p2^m cdots pk^m$."
date: "2026-06-23T23:28:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105267
codeforces_index: "F"
codeforces_contest_name: "CCF CAT 2024"
rating: 0
weight: 105267
solve_time_s: 64
verified: true
draft: false
---

[CF 105267F - \u9759\u6d41\u7684\u8def\u5f84](https://codeforces.com/problemset/problem/105267/F)

 **Đánh giá:** - 
**Thẻ:** - 
**Solve time:** 1m 4s
 **Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

We are given a number$N$được mô tả đầy đủ bởi hệ số nguyên tố của nó. Every prime$p_i$xuất hiện với cùng số mũ$m$, Vì thế$N = p_1^m p_2^m \cdots p_k^m$. Any divisor of$N$có thể được biểu diễn bằng cách chọn, đối với mỗi số nguyên tố, một số mũ giữa$0$Và$m$, tạo thành một vectơ có chiều dài$k$. 

Đối với một số chia$y$, we define a function $f(y)$là tổng của tất cả các số mũ trong hệ số hóa của nó. Ở dạng vectơ, đây chỉ đơn giản là tổng tọa độ. Chúng ta quan tâm đến những ước số có tổng số mũ chính xác bằng$T$. Đây là các nút “mục tiêu”. 

Chúng ta phải xây dựng một số trình tự, mỗi trình tự bắt đầu từ$1$ and ending at $N$, trong đó mỗi bước chuyển từ ước số sang ước số lớn hơn theo khả năng chia hết. Theo thuật ngữ số mũ, mỗi bước sẽ tăng một số tọa độ trong khi không bao giờ giảm bất kỳ tọa độ nào. Do đó, một chuỗi hợp lệ là một đường đi đơn điệu trong một$k$-dimensional grid from $(0,0,\dots,0)$ĐẾN$(m,m,\dots,m)$. 

Mục tiêu là chọn càng ít đường đi càng tốt sao cho mọi ước số có tổng số mũ bằng chính xác$T$xuất hiện ở ít nhất một đường dẫn đã chọn. 

The key constraints are$k \le 10^3$,$m \le 10^3$, Và$T \le mk \le 10^6$. Điều này loại trừ bất kỳ cách tiếp cận nào liệt kê rõ ràng tất cả các ước số hoặc xây dựng đầy đủ$k$-mạng lưới chiều. Thậm chí lưu trữ một bảng DP đầy đủ kích thước$k \times T$có thể là đường biên giới và bất kỳ hình khối nào trong$k$hoặc$T$ngay lập tức là không thể. 

Một trường hợp tinh tế thường phá vỡ lý luận ngây thơ là giả định rằng một đường dẫn duy nhất có thể bao gồm nhiều ước số mục tiêu. Ví dụ, khi$k=2, m=2, T=2$, mục tiêu bao gồm$(2,0)$,$(1,1)$, Và$(0,2)$. Một đường đi có thể đi qua nhiều nhất một trong số đó vì khi tổng số mũ đạt$2$, bất kỳ động thái nào tiếp theo đều làm tăng nó. Vì vậy không có đường dẫn nào có thể chứa hai mục tiêu khác nhau. Điều này buộc chúng tôi phải đưa ra giải pháp hoàn toàn dựa trên việc đếm. 

Một trường hợp cạnh khác là khi$T=0$. Mục tiêu duy nhất là số chia gốc$1$, vì vậy câu trả lời là rõ ràng$1$. Ở thái cực ngược lại, khi$T=mk$, mục tiêu duy nhất là$N$, lại đưa ra câu trả lời$1$. 

## Phương pháp tiếp cận 

Quan điểm brute-force là xây dựng rõ ràng tất cả các đường dẫn đơn điệu từ$1$ĐẾN$N$, sau đó kiểm tra các nút mục tiêu mà chúng đi qua và cố gắng chọn tập hợp lớp phủ tối thiểu. Ngay cả việc tạo ra tất cả các đường dẫn cũng không khả thi: số lượng các đường dẫn đơn điệu trong một$k$lưới chiều tăng trưởng theo cấp số nhân trong$k$Và$m$. Điều này ngay lập tức vượt quá mọi giới hạn tính toán. 

Bước đột phá về cấu trúc là nhận ra rằng mỗi đường dẫn có thể bao phủ tối đa một nút mục tiêu. chức năng$f$tăng nghiêm ngặt dọc theo bất kỳ đường dẫn hợp lệ nào vì mỗi bước tăng ít nhất một số mũ và không bao giờ giảm bất kỳ số mũ nào. Do đó, khi một đường dẫn đến một nút có tổng$T$, nó không bao giờ có thể truy cập lại một nút khác có cùng tổng. Điều này biến vấn đề thành một phân vùng: mỗi mục tiêu hợp lệ phải được gán cho một đường dẫn riêng biệt và mọi mục tiêu có thể được mở rộng một cách độc lập thành một đường dẫn đầy đủ lên tới$N$. 

Vì vậy, câu trả lời trở thành chính xác số lượng vectơ mũ$(e_1,\dots,e_k)$như vậy$0 \le e_i \le m$Và$\sum e_i = T$. Đây là bài toán tổ hợp số nguyên bị chặn, tương đương với việc rút ra hệ số của$x^T$TRONG$(1 + x + x^2 + \cdots + x^m)^k$. 

DP trực tiếp qua$k \times T$về mặt khái niệm thì đơn giản nhưng lại quá chậm trong trường hợp xấu nhất. Thay vào đó, chúng ta chuyển đổi biểu thức theo đại số và áp dụng phép loại trừ bao gồm để có được dạng đóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đường dẫn vũ phu + lựa chọn | Hàm mũ | Hàm mũ | Quá chậm | 
| DP trên các lớp |$O(kT)$|$O(T)$| Quá chậm | 
| Công thức bao gồm-loại trừ |$O(k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại hàm sinh như sau:$$(1 + x + \cdots + x^m)^k = \left(\frac{1 - x^{m+1}}{1 - x}\right)^k$$Điều này tách phần giới hạn khỏi cấu trúc thành phần không giới hạn. 

## Bước 1 

Mở rộng$(1 - x^{m+1})^k$sử dụng định lý nhị thức. Điều này mang lại một số tiền hơn$a$nơi chúng tôi chọn số lượng số nguyên tố đóng góp vào số hạng “ngưỡng”$x^{m+1}$. 

Mỗi thuật ngữ đóng góp:$$\binom{k}{a} (-1)^a x^{a(m+1)}$$## Bước 2 

Mở rộng$(1 - x)^{-k}$, là chuỗi sao và thanh tiêu chuẩn:$$(1 - x)^{-k} = \sum_{t \ge 0} \binom{t + k - 1}{k - 1} x^t$$Điều này tính các tác phẩm không bị ràng buộc. 

## Bước 3 

Nhân cả hai khai triển. Hệ số của$x^T$trở thành tổng trên tất cả$a$:$$\sum_{a \ge 0} \binom{k}{a} (-1)^a \binom{T - a(m+1) + k - 1}{k - 1}$$trong đó các thuật ngữ có đối số phủ định bị bỏ qua. 

## Bước 4 

Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến$T + k$. Khi đó mỗi hệ số nhị thức là$O(1)$. 

## Step 5

Iterate over$a$từ$0$ĐẾN$k$, tích lũy đóng góp modulo$998244353$. 

### Tại sao nó hoạt động 

Việc chuyển đổi tách các ràng buộc giới hạn khỏi các thành phần tự do. Thuật ngữ bao gồm-loại trừ$(1 - x^{m+1})^k$loại bỏ tất cả các giải pháp trong đó bất kỳ tọa độ nào vượt quá$m$, sửa lỗi đếm thừa từ$(1-x)^{-k}$. Mỗi vectơ số mũ hợp lệ được tính chính xác một lần vì mỗi tập hợp con của “tọa độ tràn” được hiệu chỉnh bằng các dấu xen kẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    m, k, T = map(int, input().split())
    primes = list(map(int, input().split()))  # not used

    max_n = T + k + 5

    fact = [1] * max_n
    invfact = [1] * max_n

    for i in range(1, max_n):
        fact[i] = fact[i - 1] * i % MOD

    invfact[max_n - 1] = modinv(fact[max_n - 1])
    for i in range(max_n - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    def C(n, r):
        if n < 0 or r < 0 or n < r:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    ans = 0

    for a in range(k + 1):
        t = T - a * (m + 1)
        if t < 0:
            break
        ways = C(k, a) * C(t + k - 1, k - 1) % MOD
        if a % 2 == 1:
            ans = (ans - ways) % MOD
        else:
            ans = (ans + ways) % MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai tính toán trước các giai thừa một lần và sử dụng chúng cho tất cả các đánh giá nhị thức. Vòng lặp kết thúc$a$dừng sớm khi$T - a(m+1)$trở nên phủ định, vì các số hạng tiếp theo không đóng góp gì. Các số nguyên tố không còn liên quan sau khi trình bày lại bài toán trong không gian số mũ, vì vậy chúng chỉ được đọc cho đầy đủ. 

## Ví dụ đã hoạt động 

Hãy xem xét$k=2, m=1, T=1$. Các vectơ số mũ hợp lệ là$(1,0)$Và$(0,1)$, vậy câu trả lời phải là$2$. 

| một | t = T - a(m+1) | C(k,a) | C(t+k-1,k-1) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 2 | 2 | 
| 1 | -1 | 2 | - | dừng lại | 

Kết quả cuối cùng là$2$, phù hợp với kỳ vọng Điều này xác nhận rằng mỗi lựa chọn tọa độ được tính độc lập. 

Bây giờ hãy xem xét$k=3, m=2, T=3$. Chúng tôi đang tính toán các giải pháp để$e_1+e_2+e_3=3$với mỗi$e_i \le 2$. Các vectơ hợp lệ đều là thành phần của 3 ngoại trừ những vectơ có tọa độ bằng 3. 

| một | t | C(3,a) | C(t+2,2) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | 10 | 10 | 
| 1 | 0 | 3 | 1 | -3 | 
| 2 | -3 | - | - | dừng lại | 

Kết quả là$7$, phù hợp với phép liệt kê trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k + T)$| tính toán trước giai thừa chiếm ưu thế, lặp lại$k$điều khoản | 
| Không gian |$O(T + k)$| mảng giai thừa và nghịch đảo | 

Các ràng buộc cho phép lên tới khoảng một triệu$T$và giải pháp chỉ thực hiện tiền xử lý tuyến tính và tính tổng tuyến tính trên$k \le 10^3$, vừa vặn một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import prod

    # inline solution
    input = sys.stdin.readline
    m, k, T = map(int, input().split())
    primes = list(map(int, input().split()))

    max_n = T + k + 5
    fact = [1] * max_n
    invfact = [1] * max_n
    for i in range(1, max_n):
        fact[i] = fact[i - 1] * i % MOD

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    invfact[max_n - 1] = modinv(fact[max_n - 1])
    for i in range(max_n - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    def C(n, r):
        if n < 0 or r < 0 or n < r:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    ans = 0
    for a in range(k + 1):
        t = T - a * (m + 1)
        if t < 0:
            break
        ways = C(k, a) * C(t + k - 1, k - 1) % MOD
        if a % 2:
            ans = (ans - ways) % MOD
        else:
            ans = (ans + ways) % MOD

    return str(ans % MOD)

# minimum case
assert run("1 1 0\n2\n") == "1"

# single variable
assert run("3 1 2\n2\n") == "1"

# small symmetric case
assert run("1 2 1\n2 3\n") == "2"

# boundary T = mk
assert run("2 2 4\n2 3\n") == "1"
```| Test input | Expected output | What it validates |
 | --- | --- | --- | 
| minimal | 1 | T=0 base case |
 | k=1 | 1 | hành vi một chiều | 
| small k=2 | 2 | basic compositions |
 | T=mk | 1 | độ đúng ranh giới tối đa | 

## Edge Cases

 Khi nào$T = 0$, nghiệm duy nhất là vectơ 0, do đó thuật toán chỉ tạo ra$a=0$ term with $C(k-1,k-1)=1$, cho đầu ra$1$. Bất kỳ nỗ lực nào coi đây là DP trên các đường dẫn sẽ bị tính quá mức bằng cách xem xét các trạng thái trung gian không cần thiết, nhưng công thức sẽ thu gọn một cách chính xác. 

Khi$T = mk$, chỉ tồn tại một vectơ số mũ khi tất cả các tọa độ đều bằng nhau$m$. Trong công thức, mọi số hạng có$a \ge 1$làm$t < 0$, vậy chỉ$a=0$đóng góp, mang lại chính xác một cấu hình. Điều này cho thấy tính năng loại trừ bao gồm sẽ loại bỏ hoàn toàn tất cả các trường hợp tràn không hợp lệ mà không có cách viết hoa đặc biệt.
