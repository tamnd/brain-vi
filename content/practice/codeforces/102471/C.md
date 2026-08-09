---
title: "CF 102471C - Dirichlet $k$-th gốc"
description: "Chúng tôi làm việc với các mảng được lập chỉ mục bởi các số nguyên dương, nhưng phép nhân mảng không phải là phép nhân theo từng phần tử thông thường. Đối với hai hàm (f) và (g), tích chập Dirichlet của chúng tại (n) là [ (fg)(n)=sum{dmid n}f(d)g(n/d)."
date: "2026-08-09T15:42:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "C"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 220
verified: true
draft: false
---

[CF 102471C - Dirichlet$k$-gốc thứ](https://codeforces.com/problemset/problem/102471/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi làm việc với các mảng được lập chỉ mục bởi các số nguyên dương, nhưng phép nhân mảng không phải là phép nhân theo từng phần tử thông thường. Đối với hai hàm (f) và (g), tích chập Dirichlet của chúng tại (n) là 

[ 
(f*g)(n)=\sum_{d\mid n}f(d)g(n/d). 
] 

Đầu vào đưa ra các giá trị (n) đầu tiên của hàm (g), cùng với số mũ (k). Chúng ta cần khôi phục hàm (f) thỏa mãn 

[ 
f^k=\underbrace{f_f_\cdots*f__{k\text{ lần}}=g, 
] 

với (f(1)=g(1)=1) và mọi phép tính được thực hiện theo modulo (998244353). Tuyên bố chính thức có (n\le 10^5) và giới hạn thời gian một giây. 

Đầu ra là giá trị (n) đầu tiên của (f) như vậy. Dưới những ràng buộc đã cho, một giải pháp thực sự luôn tồn tại và là duy nhất. Khả năng hiển nhiên của việc in ra (-1) xuất phát từ cách diễn đạt chung của bài toán, nhưng ở đây (k) hoàn toàn nhỏ hơn mô đun nguyên tố và biểu thức truy hồi dưới đây có mẫu số khác 0 cho mọi (n>1). 

Giới hạn (n\le10^5) loại trừ bất kỳ số bậc hai nào trong (n). Thậm chí (O(n^2)) sẽ yêu cầu khoảng (10^{10}) lần lặp. Chúng ta cần khai thác thực tế là phép tích chập Dirichlet chỉ ghép các chỉ số có tích là mục tiêu, do đó tất cả các cặp có liên quan có thể được liệt kê trong khoảng (n\log n). 

Có một số trường hợp khó xử lý. Đầu tiên, (k=1) hoàn toàn hợp lệ. Ví dụ,```
2 1
1 7
```có đầu ra đúng```
1 7
```bởi vì (f^1=f=g). Việc triển khai giả định (k\ge2) sẽ thất bại một cách không cần thiết ở đây. 

Thứ hai, phải đưa vào các hệ số tổng hợp. Coi như```
4 2
1 2 2 1
```Đầu ra đúng là```
1 1 1 0
```bởi vì 

[ 
(f*f)(4)=f(1)f(4)+f(2)f(2)+f(4)f(1)=0+1+0=1. 
] 

Một phương pháp chỉ xét đến hai thừa số (1) và (4) sẽ bỏ qua đóng góp (2\cdot2). 

Thứ ba, đại lượng sử dụng trong phép truy hồi phải tính các thừa số nguyên tố có bội số. Ví dụ, 

[ 
\Omega(4)=2, 
] 

không (1). Việc sử dụng số thừa số nguyên tố riêng biệt sẽ làm cho mẫu số của (n=4) không chính xác và phá vỡ phép truy hồi. 

Cuối cùng, (k) có thể rất gần với môđun. Ví dụ,```
2 998244352
1 1
```có câu trả lời```
1 998244352
```bởi vì (k=-1\pmod {998244353}), nên (f(2)=1/k=-1\pmod {998244353}). Chúng ta phải thực hiện tất cả các phép chia dưới dạng nghịch đảo mô đun thay vì phép chia số nguyên thông thường. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xây dựng (f), liên tục tích chập nó với chính nó và so sánh kết quả với (g). Một tích chập Dirichlet có thể được tính bằng cách liệt kê tất cả các cặp (ab=n). Trên mọi (n\le N), tổng số cặp như vậy là 

[ 
\sum_{a=1}^{N}\left\lfloor\frac Na\right\rfloor 
=O(N\log N). 
] 

Do đó, một tích chập có chi phí (O(n\log n)), nhưng việc thực hiện nó (k-1) lần chi phí (O(kn\log n)). Trong trường hợp xấu nhất (k) gần như (10^9), vì vậy với (n=10^5) đây là thứ tự của (10^{15}) phép tính số học. Cách tiếp cận này đúng, nhưng số mũ làm cho nó không thể sử dụng được. 

Quan sát hữu ích là việc lấy đạo hàm thông thường biến lũy thừa thành phép nhân với (k): 

[ 
(F^k)'=kF^{k-1}F'. 
] 

Chuỗi Dirichlet đưa ra cách biểu diễn đại số tương tự cho tích chập Dirichlet. Khó khăn là việc phân biệt (n^{-s}) đưa vào (-\ln n), đây không phải là modulo hữu ích trực tiếp (998244353). 

Chúng ta thực sự không cần các tính chất số của logarit. Chúng tôi chỉ cần danh tính 

[ 
\ln(ab)=\ln a+\ln b. 
] 

Vì vậy, chúng ta có thể thay thế (\ln n) bằng bất kỳ hàm cộng hoàn toàn nào. Sự lựa chọn tự nhiên là 

[ 
\Omega(n), 
] 

số thừa số nguyên tố của (n), được tính theo cấp số nhân. Nó thỏa mãn 

[ 
\Omega(ab)=\Omega(a)+\Omega(b). 
] 

Xác định toán tử (T) trên hàm bằng cách 

[ 
(Tf)(n)=\Omega(n)f(n). 
] 

Tính chất cộng của (\Omega) đưa ra quy tắc Leibniz cho tích chập Dirichlet: 

[ 
T(f_g)=Tf_g+f*Tg. 
] 

Do đó, với (G=F^k), 

[ 
T(G)_F=kG_T(F). 
] 

Nhìn vào hệ số thuộc (n) ta có phương trình chỉ chứa (f(n)) một lần. Tất cả các thuật ngữ khác sử dụng (f(d)) cho (d<n). Điều này biến vấn đề gốc ban đầu thành một sự tái diễn đơn giản. 

Chi tiết triển khai còn lại là đánh giá hiệu quả tất cả các đóng góp của số chia thích hợp. Khi (f(d)) được biết đến, nó đóng góp vào mọi (n=d\cdot a) với (a\ge2). Chúng tôi có thể phân phối khoản đóng góp đó ngay lập tức cho người tích lũy. Số cặp như vậy ((d,a)) là (O(n\log n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(kn\log n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính (\Omega(i)) với mọi (1\le i\le n). Chúng ta có thể làm điều này với một sàng tuyến tính, bởi vì (\Omega(p)=1) và (\Omega(ip)=\Omega(i)+1). 
2. Xác định phép biến đổi 

[ 
(Tf)(n)=\Omega(n)f(n). 
] 

Bởi vì (\Omega(ab)=\Omega(a)+\Omega(b)), phép biến đổi này tuân theo 

[ 
T(f_g)=Tf_g+f*Tg. 
] 

Đây là tính chất đại số chính xác mà đạo hàm thông thường đóng góp trong quy tắc lũy thừa thông thường. 

1. Vì (g=f^k), áp dụng (T) cho cả hai vế và sử dụng quy tắc lũy thừa sẽ cho 

[ 
T(g)_f=k,g_T(f). 
] 

Viết hệ số tại (n), 

k\sum_{d\mid n}g(d)f(n/d)\Omega(n/d). 
] 

1. Cô lập số hạng chứa (f(n)). Ở vế phải nó xảy ra khi (d=1), bởi vì (g(1)=f(1)=1). Vì (\Omega(1)=0 nên không có số hạng (f(n)) tương ứng ở bên trái. Sắp xếp lại mang lại 

\sum_{\substack{d\mid n\d<n}} 
f(d)g(n/d) 
\left(\Omega(n/d)-k\Omega(d)\right). 
] 

Do đó 

[ 
\đóng hộp{ 
f(n)= 
\frac{ 
\displaystyle 
\sum_{\substack{d\mid n\d<n}} 
f(d)g(n/d) 
\left(\Omega(n/d)-k\Omega(d)\right) 
}{ 
k\Omega(n) 
} 
}. 
] 

Với mọi (n>1), (\Omega(n)\ge1). Ngoài ra (1\le k<998244353), do đó (k\not\equiv0\pmod{998244353}). Do đó, mẫu số có thể nghịch đảo. 

1. Đặt (f(1)=1). Bảo trì ắc quy`acc[m]`chứa tử số của phép truy toán cho (f(m)). 

Khi (f(d)) đã được tính, hãy xét mọi (a\ge2) với (d a\le n). Cặp đôi 

[ 
(d,a) 
] 

đóng góp vào (m=da) bởi 

[ 
f(d)g(a) 
\left(\Omega(a)-k\Omega(d)\right). 
] 

Vì vậy, chúng tôi thêm chính xác giá trị này vào`acc[d*a]`. 

1. Xử lý (d=1,2,\ldots,n) theo thứ tự tăng dần. Vì (d=1), (f(1)=1) đã được biết và các cập nhật của nó tạo ra thuật ngữ (g(n)\Omega(n)) trong mọi bộ tích lũy. Đối với (d>1), tất cả các ước số thực sự của (d) đã được xử lý, do đó`acc[d]`đã hoàn tất và chúng ta có thể tính toán 

[ 
f(d)= 
\text{acc[d],(k\Omega(d))^{-1}\pmod p. 
] 

Sau đó phân phối ngay (f(d)) mới biết cho tất cả các bội số của nó. 

1. Tính nghịch đảo môđun của (k) và các giá trị nhỏ có thể có của (\Omega(n)). Vì (n\le10^5), (\Omega(n)\le16), nên chỉ cần một bảng nghịch đảo nhỏ. 

### Tại sao nó hoạt động 

Bất biến trung tâm là trước khi tính toán (f(n)),`acc[n]`chứa chính xác 

[ 
\sum_{\substack{d\mid n\d<n}} 
f(d)g(n/d) 
\left(\Omega(n/d)-k\Omega(d)\right). 
] 

Mỗi số hạng đã được thêm vào khi hệ số nhỏ hơn (d) của nó được xử lý và không có số hạng nào liên quan đến (f(n)) được thêm vào vì chúng ta chỉ nhân với (a\ge2). 

Phép biến đổi (T(f)(n)=\Omega(n)f(n)) thỏa mãn quy tắc Leibniz vì (\Omega) hoàn toàn cộng tính. Như vậy mọi nghiệm hợp lệ đều thỏa mãn phép truy toán. Ngược lại, phép truy hồi của chúng tôi xây dựng các giá trị thỏa mãn phép truy hồi đó. Giả sử (H=f^k). Cả (H) và (g) đã cho đều thỏa mãn cùng một phương trình tích chập được biến đổi. Hiệu của chúng có giá trị bằng 0 tại (1) và tại mọi (n>1), hệ số của nó được nhân với giá trị khác 0 (k\Omega(n)), trong khi tất cả các số hạng khác bao gồm các chỉ số nhỏ hơn. Cảm ứng lên (n) lực (H(n)=g(n)) với mọi (n\le N). Do đó (f) được xây dựng là nghiệm Dirichlet thứ (k)-th đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())
    g = [0] + list(map(int, input().split()))

    # Linear sieve for Omega(n), the number of prime factors
    # counted with multiplicity.
    lp = [0] * (n + 1)
    omega = [0] * (n + 1)
    primes = []

    for i in range(2, n + 1):
        if lp[i] == 0:
            lp[i] = i
            primes.append(i)
            omega[i] = 1

        for p in primes:
            x = i * p
            if x > n or p > lp[i]:
                break
            lp[x] = p
            omega[x] = omega[i] + 1

    # Modular inverse of k.
    inv_k = pow(k, MOD - 2, MOD)

    # Omega(n) <= 16 for n <= 1e5, but 20 is a convenient safe bound.
    inv_omega = [0] * 21
    for x in range(1, 21):
        inv_omega[x] = pow(x, MOD - 2, MOD)

    f = [0] * (n + 1)
    acc = [0] * (n + 1)

    f[1] = 1

    for d in range(1, n + 1):
        if d > 1:
            f[d] = (acc[d] % MOD) * inv_k % MOD
            f[d] = f[d] * inv_omega[omega[d]] % MOD

        fd = f[d]
        od = omega[d]

        # Every a >= 2 gives m = d * a > d.
        # This contribution is part of the recurrence for f[m].
        for a in range(2, n // d + 1):
            m = d * a
            acc[m] += fd * g[a] * (omega[a] - k * od)

    print(*f[1:])

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ với số 0 giả ở chỉ số 0, do đó chỉ số toán học (i) và chỉ số Python (i) trùng khớp. Điều này tránh lặp lại`i - 1`chuyển đổi. 

Sàng tuyến tính tính toán`omega[i]`sử dụng cách biểu diễn thừa số nguyên tố nhỏ nhất. Khi nhân (i) với một số nguyên tố (p), một thừa số nguyên tố bổ sung sẽ được đưa vào, do đó`omega[i * p] = omega[i] + 1`. 

Bộ tích lũy cố tình không giảm modulo`MOD`bên trong vòng lặp trong cùng. Mỗi`acc[m]`chỉ nhận được một số hạng cho mỗi ước số thực sự của (m), do đó nó chỉ chứa các số hạng (O(\tau(m))). Các số nguyên có độ chính xác tùy ý của Python có thể xử lý các giá trị này một cách thoải mái cho (n\le10^5), đồng thời tránh được phép toán modulo tốn kém trên mỗi cặp ước số. 

Khi bắt đầu vòng lặp cho`d`, mọi ước số thực sự của`d`đã tuyên truyền sự đóng góp của mình cho`acc[d]`. Đây là lý do tại sao thứ tự ngày càng tăng của`d`là điều cần thiết. Sau khi tính toán`f[d]`, chúng tôi ngay lập tức truyền nó đến các chỉ số lớn hơn, điều này bảo toàn tính bất biến cần thiết cho các lần lặp sau. 

biểu hiện```
omega[a] - k * od
```được cố ý cho phép là tiêu cực. Việc giảm mô-đun cuối cùng của`acc[d]`xử lý các giá trị tích lũy âm một cách chính xác. 

Không có vấn đề tràn số nguyên trong Python. Trong ngôn ngữ có số nguyên có chiều rộng cố định, các sản phẩm phải được lưu trữ ở loại số nguyên đủ rộng trước khi lấy mô đun. 

Mã này cũng không kiểm tra rõ ràng`-1`. Dưới những ràng buộc này, mẫu số (k\Omega(n)) luôn khác 0 modulo mô đun nguyên tố, do đó phép truy toán luôn tạo ra nghiệm hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 2
1 8 4 26 6
```Chúng tôi có 

[ 
\Omega(2)=1,\quad 
\Omega(3)=1,\quad 
\Omega(4)=2,\quad 
\Omega(5)=1. 
] 

Bộ tích lũy bắt đầu bằng cách xử lý (d=1). Vì (f(1)=1), nên nó đóng góp (g(a)\Omega(a)) vào mọi (a). 

| (d) | (f(d)) | Chỉ mục cập nhật | Đóng góp |`acc`sau khi cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | (8\cdot1=8) |`acc[2]=8`| 
| 1 | 1 | 3 | (4\cdot1=4) |`acc[3]=4`| 
| 1 | 1 | 4 | (26\cdot2=52) |`acc[4]=52`| 
| 1 | 1 | 5 | (6\cdot1=6) |`acc[5]=6`| 
| 2 | 4 | 4 | (4\cdot8(1-2)=-32) |`acc[4]=20`| 

Bây giờ các giá trị được lấy từ 

[ 
f(n)=\frac{\text{acc}[n]}{2\Omega(n)}. 
] 

| (n) | (\Omega(n)) |`acc[n]`| Mẫu số | (f(n)) | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | | | 1 | 
| 2 | 1 | 8 | 2 | 4 | 
| 3 | 1 | 4 | 2 | 2 | 
| 4 | 2 | 20 | 4 | 5 | 
| 5 | 1 | 6 | 2 | 3 | 

Gốc kết quả là```
1 4 2 5 3
```Phần thú vị là (n=4). Đóng góp trực tiếp (1\cdot4) cho ra (52), nhưng phân tích nhân tử (2\cdot2) đóng góp (-32) vào phép truy hồi, để lại (20). Đây chính xác là những gì cần thiết để phục hồi (f(4)=5). 

### Ví dụ được xây dựng 

lấy```
5 2
1 4 6 14 14
```Root dự định là```
1 2 3 5 7
```bởi vì 

[ 
(f*f)(2)=2f(2)=4, 
] 

[ 
(f*f)(3)=2f(3)=6, 
] 

[ 
(f*f)(4)=2f(4)+f(2)^2=10+4=14, 
] 

và 

[ 
(f*f)(5)=2f(5)=14. 
] 

Quá trình tái diễn diễn ra như sau. 

| (d) | (f(d)) | Mới`acc`đóng góp | Liên quan`acc`| 
| --- | --- | --- | --- | 
| 1 | 1 | (g(a)\Omega(a)) |`acc[2]=4`,`acc[3]=6`,`acc[4]=28`,`acc[5]=14`| 
| 2 | 2 | (2\cdot4(1-2)=-8) đến chỉ mục 4 |`acc[4]=20`| 
| 3 | 3 | không bội số (3a\le5) với (a\ge2) | không thay đổi | 
| 4 | 5 | không có bội số trong phạm vi | không thay đổi | 
| 5 | 7 | không có bội số trong phạm vi | không thay đổi | 

Các phân chia cuối cùng là 

[ 
f(2)=4/2=2, 
] 

[ 
f(3)=6/2=3, 
] 

[ 
f(4)=20/4=5, 
] 

[ 
f(5)=14/2=7. 
] 

Dấu vết này chứng tỏ sự bất biến rằng`acc[n]`chứa chính xác phần ước số thích hợp của phép truy toán trước khi (f(n)) được tính toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Mỗi cặp ((d,a)) với (da\le n) và (a\ge2) được xử lý một lần, cho ra (\sum_{d=1}^n O(n/d)=O(n\log n)). Sàng tuyến tính là (O(n)). | 
| Không gian | (O(n)) | Các mảng`g`,`f`,`acc`,`omega`và các mảng sàng đều có độ dài (O(n)). | 

Đối với (n=10^5), số lần cập nhật cặp số chia chỉ ở mức (n\log n), khoảng một triệu thay vì (10^{10}) phép toán của thuật toán bậc hai. Mức tiêu thụ bộ nhớ là tuyến tính và thoải mái dưới 256 MB. Giới hạn cuộc thi ban đầu là một giây và 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng thuật toán tương tự như giải pháp được gửi. Trường hợp kích thước tối đa có chủ ý sử dụng (k=1), giúp dễ dàng xác minh câu trả lời mong đợi mà không cần nhúng một chữ 100.000 phần tử.```python
import sys
import io

MOD = 998244353

def solve_instance(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    k = next(it)
    g = [0] + [next(it) for _ in range(n)]

    lp = [0] * (n + 1)
    omega = [0] * (n + 1)
    primes = []

    for i in range(2, n + 1):
        if lp[i] == 0:
            lp[i] = i
            primes.append(i)
            omega[i] = 1

        for p in primes:
            x = i * p
            if x > n or p > lp[i]:
                break
            lp[x] = p
            omega[x] = omega[i] + 1

    inv_k = pow(k, MOD - 2, MOD)

    inv_omega = [0] * 21
    for x in range(1, 21):
        inv_omega[x] = pow(x, MOD - 2, MOD)

    f = [0] * (n + 1)
    acc = [0] * (n + 1)
    f[1] = 1

    for d in range(1, n + 1):
        if d > 1:
            f[d] = acc[d] % MOD
            f[d] = f[d] * inv_k % MOD
            f[d] = f[d] * inv_omega[omega[d]] % MOD

        fd = f[d]
        od = omega[d]

        for a in range(2, n // d + 1):
            m = d * a
            acc[m] += fd * g[a] * (omega[a] - k * od)

    return " ".join(map(str, f[1:]))

def run(inp: str) -> str:
    return solve_instance(inp)

# Provided sample
assert run(
    "5 2\n"
    "1 8 4 26 6\n"
) == "1 4 2 5 3", "sample 1"

# k = 1, so f must equal g.
assert run(
    "2 1\n"
    "1 7\n"
) == "1 7", "minimum size and k=1"

# Composite contribution 2 * 2 is required.
assert run(
    "4 2\n"
    "1 2 2 1\n"
) == "1 1 1 0", "composite factorization"

# k = MOD - 1, so 1 / k = -1 modulo MOD.
assert run(
    "2 998244352\n"
    "1 1\n"
) == "1 998244352", "large k boundary"

# All values of the root are 1. For k = 2, g(n) is the divisor count.
assert run(
    "5 2\n"
    "1 2 2 3 2\n"
) == "1 1 1 1 1", "all-equal root"

# Maximum n, with k = 1 and all values equal to 1.
n = 100000
inp = f"{n} 1\n" + " ".join(["1"] * n) + "\n"
expected = " ".join(["1"] * n)
assert run(inp) == expected, "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 2 / 1 8 4 26 6`|`1 4 2 5 3`| Cung cấp đóng góp mẫu và ước số tổng hợp | 
|`2 1 / 1 7`|`1 7`| Kích thước tối thiểu và (k=1) | 
|`4 2 / 1 2 2 1`|`1 1 1 0`| Hệ số hóa (2\cdot2) và (\Omega(4)=2) | 
|`2 998244352 / 1 1`|`1 998244352`| (k) ở giá trị tối đa cho phép | 
|`5 2 / 1 2 2 3 2`|`1 1 1 1 1`| Giá trị gốc bằng nhau và bội số chia | 
| (n=100000,\ k=1), tất cả những cái | 100001 cái | Kích thước đầu vào tối đa và hiệu suất ranh giới | 

## Vỏ cạnh 

Đối với (k=1), phép lặp vẫn hoạt động mà không cần sửa đổi. Với```
2 1
1 7
```chúng ta có (\Omega(2)=1) và bộ tích lũy của (2) là (7). Mẫu số là (1\cdot1), vì vậy (f(2)=7). Do đó đầu ra là`1 7`, đúng như yêu cầu. 

Đối với một số tổng hợp có các thừa số nguyên tố lặp lại, (\Omega) phải tính bội số. TRONG```
4 2
1 2 2 1
```gốc là`1 1 1 0`. Tại (n=4), ước số thích hợp (d=2) đóng góp 

[ 
f(2)g(2)(\Omega(2)-2\Omega(2)) 
=1\cdot2(1-2)=-2. 
] 

Phần đóng góp ban đầu (d=1) là (g(4)\Omega(4)=1\cdot2=2), vì vậy`acc[4]=0`và do đó (f(4)=0). Việc sử dụng số thừa số nguyên tố riêng biệt sẽ sử dụng sai (\Omega(4)=1) và tạo ra mẫu số sai. 

Đối với số mũ được phép lớn nhất,```
2 998244352
1 1
```chúng tôi có (k\equiv-1\pmod{998244353}). Sự tái phát mang lại 

[ 
f(2)=1/(-1)=-1\pmod{998244353}, 
] 

vì vậy đầu ra là`1 998244352`. Điều này chứng tỏ tại sao cần phải đảo ngược mô-đun ngay cả khi số mũ đầu vào được biểu diễn dưới dạng số nguyên dương thông thường. 

Đối với (n=1), sẽ không có sự lặp lại vì (\Omega(1)=0), điều này sẽ làm cho mẫu số trở nên vô nghĩa. Sự cố bắt đầu từ (n=2) và chúng tôi đặt rõ ràng (f(1)=1) trước khi xử lý bất kỳ chỉ mục nào khác. Điều này cũng cung cấp phần tử nhận dạng cần thiết cho mỗi phép chập Dirichlet sau này. 

Khả năng có giá trị bằng 0 trong (g) không gây ra trường hợp đặc biệt nào. Ví dụ, gốc trong```
4 2
1 2 2 1
```có (f(4)=0) và phép lặp xử lý nó chính xác như mọi giá trị khác. Thuật toán không bao giờ chia cho (g(n)), do đó các phần tử 0 của (g) là vô hại. 

Trường hợp đầu ra rõ ràng (-1) cũng không yêu cầu xử lý đặc biệt đối với các ràng buộc đã cho. Vì (k) khác 0 modulo số nguyên tố (998244353) và (\Omega(n)) là số nguyên dương nhỏ hơn rất nhiều so với số nguyên tố đó của (n\le10^5), nên mọi mẫu số (k\Omega(n)) đều khả nghịch. Do đó, phép truy hồi xác định gốc cho mọi đầu vào hợp lệ.
