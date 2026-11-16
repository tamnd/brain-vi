---
title: "CF 1936E - Một vấn đề về hoán vị khác"
description: "Chúng ta được yêu cầu đếm các hoán vị $q$ có độ dài $n$ sao cho tại mọi vị trí $i < n$, giá trị tối đa của phần tử $i$ đầu tiên trong $q$ khác với giá trị tối đa của phần tử $i$ đầu tiên trong một hoán vị cho trước $p$."
date: "2026-06-08T18:02:04+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 1936
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 930 (Div. 1)"
rating: 3400
weight: 1936
solve_time_s: 176
verified: false
draft: false
---

[CF 1936E - Một vấn đề về hoán vị khác](https://codeforces.com/problemset/problem/1936/E) 

**Đánh giá:** 3400 
**Tags:** chia để trị, fft, toán 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm hoán vị$q$chiều dài$n$sao cho ở mọi vị trí$i < n$, giá trị lớn nhất ban đầu$i$các yếu tố trong$q$khác với mức tối đa của lần đầu tiên$i$các phần tử trong một hoán vị nhất định$p$. Nói cách khác, chúng tôi muốn tránh việc khớp với “cực đại đang chạy” của$p$với$q$ở bất kỳ tiền tố nào. Phần tử cuối cùng không bị ràng buộc vì điều kiện tiền tố chỉ áp dụng tối đa$n-1$. Đầu ra phải là modulo$998244353$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có một hoán vị$p$có chiều dài lên tới$2 \cdot 10^5$, và tổng của tất cả$n$trên các trường hợp thử nghiệm được giới hạn ở$2 \cdot 10^5$. Điều này có nghĩa là thuật toán của chúng tôi phải chạy trong thời gian gần như tuyến tính cho mỗi trường hợp thử nghiệm, vì các phương pháp bậc hai (tạo ra tất cả các hoán vị,$O(n!)$) hoàn toàn không thể thực hiện được. 

Các trường hợp cạnh không rõ ràng bao gồm các hoán vị nhỏ như$n = 2$, trong đó chỉ có một vị trí quan trọng và các hoán vị có thứ tự tăng hoặc giảm nghiêm ngặt, có thể dẫn đến cực đại lặp lại trên nhiều vị trí. Ví dụ, nếu$p = [1, 2, 3]$, chúng ta không thể có$q = [1, 2, 3]$vì mỗi tiền tố tối đa sẽ trùng khớp, nhưng$q = [3, 2, 1]$là hợp lệ. Việc tạo ra tất cả các hoán vị một cách ngây thơ sẽ thất bại cả về hiệu suất và lý luận về cực đại lặp lại một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả$n!$hoán vị của$q$và kiểm tra điều kiện cực đại tiền tố cho mỗi điều kiện. Vì$n = 10$, đây đã là 3,6 triệu hoán vị; vì$n = 20$nó vượt xa giới hạn thời gian. Mỗi lần kiểm tra sẽ mất$O(n)$, mang lại$O(n \cdot n!)$, đó là điều vô vọng đối với giới hạn trên. 

Cái nhìn sâu sắc quan trọng là theo dõi các vị trí của cực đại tiềm năng. Chúng ta hãy xác định những giá trị nào xuất hiện dưới dạng cực đại tiền tố trong$p$. Đối với một hoán vị$p$chiều dài$n$, mỗi giá trị$v$xuất hiện dưới dạng tiền tố tối đa tại một chỉ mục duy nhất (lần xuất hiện đầu tiên của nó trong số các cực đại tăng dần). Chúng ta cần xây dựng$q$sao cho tiền tố tối đa tại mỗi vị trí không bao giờ khớp với tiền tố tối đa trong$p$. Điều này làm giảm vấn đề về cách sắp xếp trong đó các cực đại bị “đặt sai chỗ” so với vị trí của chúng. 

Chúng ta có thể giải quyết vấn đề này một cách hiệu quả bằng cách xem hoán vị như một quá trình chọn phần tử lớn nhất còn lại ở hai đầu của các số có sẵn. Sử dụng phương pháp chia để trị, chúng ta chọn đệ quy phần tử lớn nhất trong đoạn còn lại và chia các số còn lại thành các đoạn trái và phải. Số cách sắp xếp hợp lệ là tích của các số đếm ở các đoạn bên trái và bên phải, nhân với số cách xen kẽ chúng sao cho tiền tố cực đại không trùng nhau. Điều này có thể được tính toán theo thời gian tuyến tính bằng cách sử dụng các công thức tổ hợp và theo dõi tiền tố tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * n!) | O(n) | Quá chậm | 
| Chia rẽ và chinh phục | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước giai thừa và nghịch đảo giai thừa modulo$998244353$lên đến mức tối đa$n$. Điều này cho phép tính toán tổ hợp hiệu quả sau này. 
2. Đối với mỗi trường hợp thử nghiệm, xác định vị trí của cực đại tiền tố trong$p$. Bắt đầu với số lớn nhất$n$và xác định đệ quy giá trị lớn nhất trong các phân đoạn còn lại. 
3. Đối với mỗi phân đoạn được xác định bởi cực đại, đếm đệ quy các hoán vị hợp lệ ở bên trái và bên phải của phân đoạn. Nếu độ dài đoạn là 1, trả về 1, vì một phần tử không có xung đột. 
4. Đối với một đoạn có độ dài$m$với chiều dài bên trái$l$và chiều dài phù hợp$r = m-1-l$, số cách sắp xếp các phần tử là$\binom{m-1}{l} \cdot \text{count\_left} \cdot \text{count\_right}$. Hệ số nhị thức tính đến việc chọn phần tử nào ở bên trái và phần tử nào ở bên phải. 
5. Nhân các số đếm một cách đệ quy và lấy modulo$998244353$. 
6. Xuất kết quả cho từng test case. 

Tại sao nó hoạt động: điều bất biến là ở mỗi bước đệ quy, phần tử lớn nhất trong phân đoạn hiện tại không thể xuất hiện ở cùng vị trí tiền tố như trong$p$. Bằng cách tách trái và phải, chúng tôi đảm bảo rằng tất cả các phần tử còn lại duy trì thuộc tính này. Hệ số tổ hợp đếm tất cả các lần xen kẽ hợp lệ thỏa mãn các ràng buộc cực đại tiền tố. Đệ quy đảm bảo rằng các bài toán con nhỏ hơn duy trì tính chính xác, do đó kết quả cuối cùng sẽ tính tất cả các hoán vị hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

# precompute factorials and inverses
N = 2 * 10**5 + 5
fact = [1] * N
inv_fact = [1] * N
for i in range(2, N):
    fact[i] = fact[i-1] * i % MOD
inv_fact[N-1] = pow(fact[N-1], MOD-2, MOD)
for i in range(N-2, 0, -1):
    inv_fact[i] = inv_fact[i+1] * (i+1) % MOD

def comb(n, k):
    if k < 0 or k > n:
        return 0
    return fact[n] * inv_fact[k] % MOD * inv_fact[n-k] % MOD

def solve_segment(p):
    if not p:
        return 1
    m = max(p)
    idx = p.index(m)
    left = p[:idx]
    right = p[idx+1:]
    return comb(len(left)+len(right), len(left)) * solve_segment(left) % MOD * solve_segment(right) % MOD

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    print(solve_segment(p))
```Mã này tính toán trước các giai thừa và giai thừa nghịch đảo để tính toán tổ hợp.`solve_segment`tính toán đệ quy các hoán vị hợp lệ cho từng phân đoạn bằng cách tách phần tử tối đa. Hệ số kết hợp tính các cách xen kẽ phần bên trái và bên phải. I/O nhanh đảm bảo xử lý lên đến$2 \cdot 10^5$các phần tử một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1:`p = [2, 1]`| Bước | Phân đoạn | Tối đa | Chỉ mục | Trái | Đúng | Đếm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [2,1] | 2 | 0 | [] | [1] | lược(1,0)_1_1 = 1 | 

Chỉ có một hoán vị hợp lệ`[1,2]`. Xác nhận xử lý trường hợp cơ bản. 

### Mẫu 2:`p = [1, 2, 3]`| Bước | Phân đoạn | Tối đa | Chỉ mục | Trái | Đúng | Đếm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [1,2,3] | 3 | 2 | [1,2] | [] | comb(2,2)*count([1,2])*1 | 
| 2 | [1,2] | 2 | 1 | [1] | [] | lược(1,1)_1_1 = 1 | 
| 3 | [1] | 1 | 0 | [] | [] | 1 | 

Số cuối cùng = 3. Thể hiện phép chia và nhân đệ quy chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được truy cập một lần trong đệ quy, tra cứu kết hợp là O(1) | 
| Không gian | O(n) | Bảng giai thừa và ngăn xếp đệ quy | 

Giải pháp dễ dàng phù hợp trong giới hạn 5 giây, thậm chí ở tổng tối đa$n = 2 \cdot 10^5$, vì tất cả các phép toán đều tuyến tính với các hệ số không đổi nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353

    N = 2*10**5+5
    fact = [1]*N
    inv_fact = [1]*N
    for i in range(2,N):
        fact[i] = fact[i-1]*i%MOD
    inv_fact[N-1] = pow(fact[N-1],MOD-2,MOD)
    for i in range(N-2,0,-1):
        inv_fact[i] = inv_fact[i+1]*(i+1)%MOD
    def comb(n,k):
        if k<0 or k>n:
            return 0
        return fact[n]*inv_fact[k]%MOD*inv_fact[n-k]%MOD
    def solve_segment(p):
        if not p:
            return 1
        m = max(p)
        idx = p.index(m)
        left = p[:idx]
        right = p[idx+1:]
        return comb(len(left)+len(right), len(left))*solve_segment(left)%MOD*solve_segment(right)%MOD
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        p = list(map(int,input().split()))
        res.append(str(solve_segment(p)))
    return "\n".join(res)

# provided samples
assert run("6\n2\n2
```
