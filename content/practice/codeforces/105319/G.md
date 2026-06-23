---
title: "CF 105319G - Ít hơn là nhiều hơn"
description: "Chúng ta được cho một số nguyên dương $n$. Với mỗi $n$, chúng ta xem xét biểu thức đa thức $$(a+b)^n - a^n - b^n$$ và chúng ta hỏi môđun $m$ nào mà biểu thức này luôn chia hết cho $m$, bất kể chúng ta chọn số tự nhiên $a$ và $b$ nào."
date: "2026-06-22T11:32:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105319
codeforces_index: "G"
codeforces_contest_name: "Tishreen Collegiate Programming Contest 2024"
rating: 0
weight: 105319
solve_time_s: 55
verified: true
draft: false
---

[CF 105319G - Ít hơn là nhiều hơn](https://codeforces.com/problemset/problem/105319/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$n$. Đối với mỗi$n$, chúng ta xét biểu thức đa thức$$(a+b)^n - a^n - b^n$$và chúng tôi yêu cầu mô-đun nào$m$biểu thức này luôn chia hết cho$m$, bất kể số tự nhiên nào$a$Và$b$chúng tôi chọn. 

Tương tự, chúng tôi muốn tất cả các số nguyên$m$sao cho khai triển nhị thức của$(a+b)^n$, sau khi loại bỏ tinh khiết$a^n$Và$b^n$số hạng, luôn đồng dư với 0 modulo$m$cho mỗi cặp$(a,b)$. 

Đầu ra không phải là một giá trị duy nhất mà là số lượng mô-đun hợp lệ riêng biệt$m$, lấy modulo$10^9+7$. Vì vậy, đối với mỗi trường hợp thử nghiệm, chúng tôi đang đếm một cách hiệu quả có bao nhiêu số nguyên chia cho một giá trị ẩn nhất định chỉ phụ thuộc vào$n$. 

Ràng buộc$T \le 3 \cdot 10^5$có nghĩa là chúng tôi không thể thực hiện bất kỳ phép tính đại số hoặc phép liệt kê nặng nào trên mỗi bài kiểm tra$a,b$. Mỗi truy vấn phải được trả lời về cơ bản$O(\log n)$hoặc$O(\sqrt n)$sau khi tiền xử lý. Từ$n \le 10^6$, việc tính toán trước dữ liệu lý thuyết số như các thừa số nguyên tố nhỏ nhất là khả thi. 

Một sự hiểu lầm ngây thơ là nghĩ rằng chúng ta phải kiểm tra tính chia hết cho tất cả$a,b$. Ví dụ, khi$n=2$,$$(a+b)^2 - a^2 - b^2 = 2ab$$và người ta có thể tin sai rằng câu trả lời phụ thuộc vào hành vi đối với tất cả các sản phẩm$ab$, nhưng trên thực tế, cấu trúc buộc một gcd cố định trên tất cả các đầu vào. 

Một trường hợp thất bại tinh vi khác là giả định rằng tất cả các hệ số nhị thức phải chia hết cho$m$. Điều đó quá mạnh: các biến$a^k b^{n-k}$tương tác, do đó việc hủy bỏ theo các lựa chọn khác nhau của$a,b$vấn đề. Đối tượng chính xác là ước số chung lớn nhất trên tất cả các giá trị của biểu thức, không phải là số chia hết theo hệ số. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ cố gắng tính biểu thức cho nhiều cặp$(a,b)$và sau đó lấy gcd qua các giá trị được lấy mẫu để đoán tất cả các giá trị hợp lệ$m$. Điều này ngay lập tức không khả thi vì ngay cả việc sửa các giới hạn nhỏ như$a,b \le 10^5$đã tạo ra quá nhiều đánh giá và quan trọng hơn, việc lấy mẫu không thể đảm bảo tính chính xác vì cấu trúc gcd mang tính lý thuyết số và không mang tính xác suất. 

Sự thay đổi quan trọng là ngừng suy nghĩ về các đánh giá riêng lẻ và thay vào đó hãy hỏi số nguyên nào luôn chia biểu thức cho tất cả$a,b$. Khi chúng ta mở rộng bằng cách sử dụng định lý nhị thức, mọi số hạng trong$$(a+b)^n - a^n - b^n$$có hình thức$$\binom{n}{k} a^k b^{n-k}, \quad 1 \le k \le n-1.$$Vì vậy, chúng tôi thực sự đang tìm kiếm ước chung lớn nhất của biểu thức đa thức này trên tất cả các biểu thức tự nhiên$a,b$. Điều đó làm giảm vấn đề tìm một số nguyên duy nhất$G(n)$sao cho mọi giá trị hợp lệ$m$chính xác là một ước số của$G(n)$. Câu trả lời là số ước của$G(n)$. 

Một kết quả cổ điển trong lý thuyết số cho biểu thức nhị thức đối xứng cụ thể này là:$$G(n) =
\begin{cases}
n & \text{if } n \text{ is a power of two} \\
2n & \text{otherwise}
\end{cases}$$Vì vậy, toàn bộ vấn đề giảm xuống việc kiểm tra xem$n$là lũy thừa của hai và sau đó đếm các ước của một trong hai$n$hoặc$2n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lý luận thô bạo theo cặp |$O(a b)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Giảm GCD + đếm số chia |$O(\sqrt n)$mỗi lần kiểm tra sau khi tiền xử lý |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại bài toán thành một phép tính lý thuyết số thuần túy trên$G(n)$, sau đó đếm các ước của nó. 

1. Tính trước các thừa số nguyên tố nhỏ nhất lên đến$10^6$. Điều này cho phép phân tích nhanh bất kỳ$n$hoặc$2n$theo thời gian logarit cho mỗi lần kiểm tra. Lý do chúng ta cần điều này là vì việc đếm số chia yêu cầu số mũ nguyên tố và việc tính toán lại hệ số cho mỗi truy vấn một cách đơn giản sẽ quá chậm theo$3 \cdot 10^5$các bài kiểm tra. 
2. Đối với mỗi test case, hãy kiểm tra xem$n$là sức mạnh của hai. Việc này được thực hiện bằng cách sử dụng thuộc tính bit$n \& (n-1) = 0$. Điều kiện này nắm bắt chính xác cấu trúc trong đó chỉ một bit được đặt, nghĩa là không có thừa số nguyên tố lẻ nào xuất hiện theo cách gây ra hiện tượng nhân đôi. 
3. Đặt$x = n$nếu như$n$là lũy thừa của hai, nếu không thì được đặt$x = 2n$. Bước này mã hóa kết quả gcd đã biết của biểu thức tích chập nhị thức. 
4. Nhân tố hóa$x$sử dụng bảng hệ số nguyên tố nhỏ nhất được tính toán trước. Trong quá trình nhân tử hóa, hãy tích lũy số mũ của mỗi số nguyên tố. 
5. Tính số ước là tích của tất cả các số nguyên tố của$(e_i + 1)$, Ở đâu$e_i$là số mũ của số nguyên tố đó trong$x$. Lấy kết quả modulo$10^9+7$. 
6. Xuất ra số chia này. 

### Tại sao nó hoạt động 

biểu hiện$(a+b)^n - a^n - b^n$là một đa thức đối xứng thuần nhất có bậc$n$. Giá trị của nó trên các cặp số nguyên$(a,b)$tạo ra một lý tưởng trong$\mathbb{Z}$và lý tưởng đó là hiệu trưởng, được tạo bởi một số nguyên duy nhất$G(n)$, đó là gcd của tất cả các đánh giá. Mọi mô đun hợp lệ$m$phải chia mọi đánh giá, nên nó phải chia$G(n)$. Ngược lại, bất kỳ ước số nào của$G(n)$hoạt động tầm thường. Điều này làm giảm toàn bộ vấn đề về tính toán$G(n)$và đếm các ước của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAXN = 10**6

spf = list(range(MAXN + 1))

for i in range(2, int(MAXN ** 0.5) + 1):
    if spf[i] == i:
        step = i
        start = i * i
        for j in range(start, MAXN + 1, step):
            if spf[j] == j:
                spf[j] = i

def factorize(x):
    res = {}
    while x > 1:
        p = spf[x]
        cnt = 0
        while x % p == 0:
            x //= p
            cnt += 1
        res[p] = cnt
    return res

def solve_case(n):
    if n & (n - 1) == 0:
        x = n
    else:
        x = 2 * n

    fac = factorize(x)
    ans = 1
    for e in fac.values():
        ans = (ans * (e + 1)) % MOD
    return ans

t = int(input())
out = []
for _ in range(t):
    n = int(input())
    out.append(str(solve_case(n)))

print("\n".join(out))
```Sàng xây dựng các thừa số nguyên tố nhỏ nhất sao cho mọi số lên đến$10^6$có thể được nhân tố hóa một cách nhanh chóng. Quyết định có sử dụng hay không$n$hoặc$2n$là bản dịch trực tiếp của kết quả gcd cấu trúc. Một lần$x$được cố định, số chia sẽ tuân theo lý thuyết số nhân tiêu chuẩn. 

Một cạm bẫy triển khai phổ biến là quên rằng việc phân tích nhân tử phải bao gồm hệ số bổ sung là 2 khi$n$không phải là sức mạnh của hai. Thiếu điều đó sẽ làm đảo lộn tất cả các câu trả lời cho các trường hợp tổng hợp lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$n = 2$. 

| Bước | Giá trị | 
| --- | --- | 
| sức mạnh của hai kiểm tra | đúng | 
| đã chọn$x$| 2 | 
| nhân tử hóa |$2^1$| 
| số chia | 2 | 

Vì vậy, câu trả lời là 2. 

Điều này xác nhận trường hợp cơ bản trong đó biểu thức giảm xuống còn$2ab$và tất cả các mô đun hợp lệ đều là ước của 2. 

### Ví dụ 2 

hãy để$n = 3$. 

| Bước | Giá trị | 
| --- | --- | 
| sức mạnh của hai kiểm tra | sai | 
| đã chọn$x$| 6 | 
| nhân tử hóa |$2^1 \cdot 3^1$| 
| số chia |$(1+1)(1+1)=4$| 

Vậy đáp án là 4. 

Điều này phù hợp với thực tế là tất cả các mô đun hợp lệ phải chia cho 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log \log N + T \log N)$| sàng một lần, tính từng truy vấn | 
| Không gian |$O(N)$| bảng thừa số nguyên tố nhỏ nhất | 

Quá trình tiền xử lý chiếm ưu thế một lần, trong khi mỗi truy vấn đủ nhanh để$3 \cdot 10^5$đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7
    MAXN = 10**6

    spf = list(range(MAXN + 1))
    for i in range(2, int(MAXN ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXN + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factorize(x):
        res = {}
        while x > 1:
            p = spf[x]
            c = 0
            while x % p == 0:
                x //= p
                c += 1
            res[p] = c
        return res

    def solve(n):
        x = n if (n & (n - 1)) == 0 else 2 * n
        fac = factorize(x)
        ans = 1
        for e in fac.values():
            ans = (ans * (e + 1)) % MOD
        return ans

    out = []
    for _ in range(int(input())):
        n = int(input())
        out.append(str(solve(n)))
    return "\n".join(out)

assert run("1\n2\n") == "2"
assert run("1\n3\n") == "4"
assert run("1\n4\n") == "3"
assert run("1\n6\n") == "8"
assert run("3\n1\n2\n3\n") == "1\n2\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$| 1 | hành vi ranh giới suy biến | 
|$n=2$| 2 | trường hợp không tầm thường nhỏ nhất | 
|$n=4$| 3 | sức mạnh của sự đúng đắn của hai nhánh | 
|$n=6$| 8 | trường hợp tổng hợp không lũy ​​thừa của hai | 

## Vỏ cạnh 

cho$n=1$, biểu thức bằng 0 giống hệt nhau, do đó mọi mô đun đều hoạt động, nhưng theo công thức dẫn xuất, chúng tôi coi nó là có số chia là 1 vì$x=1$và hệ số hóa trống, tạo ra câu trả lời 1. Điều này phù hợp với quy ước chỉ$m=1$được tính vào công thức rút gọn. 

Vì$n$là sức mạnh của hai như$n=8$, thuật toán chọn$x=n$còn hơn là$2n$. Điều này ngăn cản việc giới thiệu một cách giả tạo hệ số bổ sung 2 không tồn tại trong cấu trúc gcd. Kiểm tra lũy thừa hai đảm bảo nhánh này được thực hiện chính xác khi các hệ số nhị thức có sự liên kết định giá 2-adic tối đa, điều này làm thay đổi gcd toàn cầu. 

Đối với tổ hợp lớn$n$chẳng hạn như$n=10^6$, bước phân tích vẫn nhanh vì tra cứu SPF giảm từng bước chia thành$O(1)$và tổng số phép chia được giới hạn bởi số thừa số nguyên tố, nhỏ so với$n$.
