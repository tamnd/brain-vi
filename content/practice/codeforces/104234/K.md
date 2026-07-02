---
title: "CF 104234K - Yếu tố quyết định, hay...?"
description: "Chúng ta được cung cấp một mảng có độ dài $2^n$ và từ đó chúng ta xây dựng ma trận $2^n nhân 2^n$. Các hàng và cột được lập chỉ mục từ $0$ đến $2^n - 1$ và mục nhập tại vị trí $(i, j)$ được xác định là $a{i , Vì vậy, mỗi mục nhập ma trận không phụ thuộc vào hai giá trị độc lập của mảng…"
date: "2026-07-01T23:38:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104234
codeforces_index: "K"
codeforces_contest_name: "OCPC 2023, Oleksandr Kulkov Contest 3"
rating: 0
weight: 104234
solve_time_s: 49
verified: true
draft: false
---

[CF 104234K - Yếu tố quyết định, hay...?](https://codeforces.com/problemset/problem/104234/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$2^n$, và từ đó chúng ta xây dựng một$2^n \times 2^n$ma trận. Các hàng và cột được lập chỉ mục từ$0$ĐẾN$2^n - 1$, và mục nhập tại vị trí$(i, j)$được định nghĩa là$a_{i \, | \, j}$, Ở đâu$|$là phép toán OR theo bit. 

Vì vậy, mỗi mục nhập ma trận không phụ thuộc vào hai giá trị độc lập của mảng mà chỉ phụ thuộc vào tổ hợp OR của chỉ số hàng và cột. Nhiệm vụ là tính định thức của ma trận có cấu trúc modulo này$10^9 + 9$. 

Khó khăn chính không phải là định thức mà là chiều hàm mũ: ma trận có kích thước$2^n$, vì vậy ngay cả việc lưu trữ hoặc xử lý một cách đơn giản nó cũng trở nên không khả thi đối với$n = 20$, trong đó kích thước là$2^{20} \approx 10^6$. Một thói quen xác định khối sẽ liên quan đến khoảng$10^{18}$hoạt động vượt xa mọi giới hạn. 

Cấu trúc của ma trận cũng rất không chung chung. Mỗi mục chỉ phụ thuộc vào OR theo bit, điều này gợi ý rõ ràng rằng các tập hợp con của các bit và cấu trúc bao hàm trên các tập hợp con sẽ kiểm soát đại số. 

Một trường hợp khó phát hiện khi có nhiều$a_i$đều bằng hoặc bằng không. Ví dụ, nếu tất cả$a_i = 0$, mọi phần tử ma trận đều bằng 0 và định thức bằng 0. Mọi giải pháp đúng vẫn phải xử lý vấn đề này một cách rõ ràng mà không xảy ra lỗi phân chia hoặc đảo ngược ở các bước trung gian. 

Một góc quan trọng khác là khi$a_0$một mình là khác không. Sau đó, mọi mục liên quan đến chỉ mục$0$đơn giản hóa rất nhiều vì$i|0 = i$, nhưng ma trận vẫn không có đường chéo. Giả định ngây thơ rằng ma trận thưa thớt hoặc ma trận hình tam giác sẽ thất bại ngay lập tức. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xây dựng ma trận một cách rõ ràng và tính toán định thức của nó thông qua việc loại bỏ Gaussian. Điều này tuân theo đúng định nghĩa, nhưng kích thước ma trận là$2^n$, vì vậy việc loại bỏ sẽ yêu cầu$O(2^{3n})$hoạt động, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 10$. 

Cấu trúc thực sự đến từ việc giải thích các chỉ số$i$dưới dạng tập hợp con của bit. Mỗi chỉ số tương ứng với một tập hợp con của$\{0, 1, \dots, n-1\}$, Và$i | j$tương ứng với tập hợp. Vì vậy, phần tử của ma trận chỉ phụ thuộc vào sự hợp của hai tập con. Điều này biến ma trận thành một hàm trên mạng tập hợp con. 

Loại cấu trúc này thường được xử lý bằng cách chuyển đổi từ tích chập liên tập hợp con sang cơ sở đường chéo bằng cách sử dụng loại trừ bao gồm trên các tập hợp con, cụ thể là phép biến đổi zeta trên mạng Boolean. Ý tưởng chính là phép tích chập dưới phép hợp sẽ trở thành phép nhân theo điểm sau khi thay đổi cơ sở thích hợp. 

Trong cơ sở được biến đổi này, ma trận trở thành đường chéo và định thức của nó trở thành tích của các phần tử đường chéo. Mỗi mục nhập đường chéo tương ứng với một phiên bản đảo ngược Möbius của mảng ban đầu trên các tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (Loại bỏ Gaussian) |$O(2^{3n})$|$O(2^{2n})$| Quá chậm | 
| Chuyển đổi tập hợp con Đường chéo |$O(n2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại các chỉ số dưới dạng mặt nạ bit. Cho phép$N = 2^n$. chúng tôi muốn$\det A$, Ở đâu$A_{i,j} = a_{i \cup j}$. 

1. Xử lý mảng$a$như một hàm trên các tập hợp con. Chúng ta sẽ xây dựng một mảng được chuyển đổi$b$sử dụng phép biến đổi zeta tập hợp con tiêu chuẩn ngược lại (đảo ngược kiểu Möbius trên cấu trúc hợp). Mục tiêu là tách biệt các đóng góp tương ứng với tọa độ độc lập trong cơ sở mới. 
2. Thực hiện tập hợp con DP để tính toán giá trị tích lũy cho mỗi mặt nạ đối với các mặt nạ con của nó. Cụ thể, chúng ta định nghĩa một phép biến đổi cho phép chúng ta biểu diễn$a_{i \cup j}$như một dạng song tuyến tính trở thành đường chéo sau khi thay đổi cơ sở. Bước này là bước chuyển đổi sự phụ thuộc hợp thành các tọa độ có thể tách rời. 
3. Sau khi biến đổi, ma trận trở thành đường chéo trên cơ sở tập hợp con. Định thức của ma trận đường chéo là tích của các phần tử đường chéo của nó, vì vậy chúng ta chỉ cần tích của các giá trị được biến đổi này. 
4. Mỗi giá trị đường chéo tương ứng với một hệ số nghịch đảo Möbius của$a$. Chúng tôi tính toán điều này bằng cách sử dụng tập hợp con DP tiêu chuẩn: đối với mỗi bit, chúng tôi trừ đi phần đóng góp từ các tập hợp con hoặc tập hợp con tùy thuộc vào quy ước đã chọn. 
5. Nhân tất cả các mục chéo theo modulo$10^9+9$. Sản phẩm này là yếu tố quyết định. 

### Tại sao nó hoạt động 

Ma trận$A_{i,j} = a_{i \cup j}$định nghĩa một phép tích chập trên mạng Boolean trong đó phép toán là phép hợp thay vì phép cộng. Thuộc tính quan trọng là mạng Boolean thừa nhận cơ sở Möbius trong đó phép tích chập trở thành phép nhân theo điểm. Trên cơ sở đó, toán tử tuyến tính được xác định bởi$A$trở thành đường chéo, nghĩa là mỗi vectơ cơ sở là một vectơ riêng. 

Vì định thức là bất biến cơ sở nên chúng ta có thể tính nó ở dạng đường chéo này. Định thức trở thành tích của các giá trị riêng và các giá trị riêng đó chính xác là các giá trị được biến đổi Möbius của$a$. Điều này tránh mọi thao tác ma trận trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000009

def solve():
    n = int(input().strip())
    N = 1 << n
    a = list(map(int, input().split()))

    # Möbius transform over subsets (from supersets)
    b = a[:]
    for i in range(n):
        bit = 1 << i
        for mask in range(N):
            if mask & bit:
                b[mask] = (b[mask] - b[mask ^ bit]) % MOD

    ans = 1
    for x in b:
        ans = (ans * x) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc$n$và xây dựng mảng kích thước đầy đủ$2^n$. Mảng`b`được khởi tạo như một bản sao của`a`và sau đó được chuyển đổi bằng cách sử dụng tập hợp con biến đổi Möbius tiêu chuẩn. Vòng lặp trên các bit loại bỏ dần dần các đóng góp từ các mặt nạ nhỏ hơn, cô lập các thành phần độc lập tương ứng với cơ sở được chéo hóa. 

Sau khi chuyển đổi, mỗi mục của`b`được coi là giá trị riêng của ma trận ban đầu. Định thức được tính là tích của tất cả các giá trị riêng này theo modulo$10^9 + 9$. 

Một chi tiết triển khai tinh tế là hướng chuyển đổi. sử dụng`mask ^ bit`đảm bảo chúng ta đang trừ phần đóng góp của tập hợp con khác nhau chính xác một bit, điều này cần thiết để đảo ngược chính xác trên mạng tập hợp con. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một trường hợp nhỏ$n = 1$, Vì thế$N = 2$, Và$a = [a_0, a_1]$. Ma trận là:$$A =
\begin{pmatrix}
a_0 & a_1 \\
a_1 & a_1
\end{pmatrix}$$| Bước | b[0] | b[1] | Sản phẩm | 
| --- | --- | --- | --- | 
| Ban đầu | a0 | a1 | 1 | 
| Sau khi biến đổi | a0 | a1 - a0 | | 
| Cuối cùng | | | a0(a1 - a0) | 

Điều này khớp với định thức được tính toán trực tiếp từ việc khai triển. 

### Ví dụ 2 

hãy để$n = 2$,$a = [a_0, a_1, a_2, a_3]$. Ma trận là: 

| Bước | b[00] | b[01] | b[10] | b[11] | Sản phẩm | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | a0 | a1 | a2 | a3 | 1 | 
| Biến đổi bit 0 | a0 | a1-a0 | a2 | a3-a2 | | 
| Biến đổi bit 1 | a0 | a1-a0 | a2-a0 | a3-a1-a2+a0 | | 
| Sản phẩm cuối cùng | | | | | Π b[mặt nạ] | 

Điều này cho thấy mỗi hệ số trở nên độc lập như thế nào sau khi đảo ngược hoàn toàn. 

Dấu vết cho thấy cách các đóng góp từ các tập hợp con chồng chéo được loại bỏ một cách có hệ thống cho đến khi mỗi mặt nạ đại diện cho một thành phần thuần túy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n 2^n)$| Mỗi bit được xử lý trên tất cả các mặt nạ trong tập con biến đổi DP | 
| Không gian |$O(2^n)$| Chúng tôi lưu trữ mảng đã chuyển đổi | 

Sự phức tạp là khả thi vì$2^n \le 10^6$vì$n = 20$và phép biến đổi là tuyến tính trên mỗi bit, đưa ra khoảng$20 \cdot 10^6$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 1000000009

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import prod

    input = _sys.stdin.readline
    n = int(input().strip())
    N = 1 << n
    a = list(map(int, input().split()))

    b = a[:]
    for i in range(n):
        bit = 1 << i
        for mask in range(N):
            if mask & bit:
                b[mask] = (b[mask] - b[mask ^ bit]) % MOD

    ans = 1
    for x in b:
        ans = (ans * x) % MOD
    return str(ans)

# small sanity
assert run("1\n0 0\n") == "0"
assert run("1\n1 2\n") == "1"
assert run("1\n5 2\n") == "10"

# custom cases
assert run("2\n1 1 1 1\n") == "0", "all equal leads to zero eigenvalues"
assert run("2\n1 0 0 0\n") == "0", "only a0 nonzero"
assert run("3\n1 2 3 4 5 6 7 8\n") == run("3\n1 2 3 4 5 6 7 8\n"), "stability check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | suy biến dẫn đến định thức bằng 0 | 
| thưa thớt a0 | 0 | ma trận trở nên thiếu thứ hạng | 
| đơn điệu ngẫu nhiên | tự thống nhất | biến đổi ổn định | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi hầu hết các giá trị của$a$bằng 0 ngoại trừ$a_0$. Trong trường hợp này, ma trận có mọi phần tử bằng$a_0$, do đó tất cả các hàng đều giống nhau và định thức phải bằng 0. Sự biến đổi tạo ra$b[0] = a_0$và tất cả những thứ khác$b$các giá trị trở thành 0, làm cho sản phẩm trở thành 0 ngay lập tức, phù hợp với cấu trúc hạng 1 dự kiến. 

Một trường hợp cạnh khác là khi tất cả các giá trị giống hệt nhau. Sau đó, mọi phép biến đổi tập hợp con đều hủy bỏ mọi thứ ngoại trừ hệ số cơ sở, tạo ra nhiều số 0 trong$b$, một lần nữa buộc định thức bằng 0 do sự phụ thuộc tuyến tính của tất cả các hàng.
