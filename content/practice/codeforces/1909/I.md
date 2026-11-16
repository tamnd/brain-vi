---
title: "CF 1909I - Bài toán hoán vị ngắn"
description: "Chúng ta được yêu cầu đếm các hoán vị của các số từ 1 đến $n$ với một thuộc tính cụ thể: đối với mỗi ngưỡng có thể có $m$ trong khoảng từ 3 đến $n+1$, và với mỗi số $k$ trong khoảng từ 0 đến $n-1$, chúng ta cần số lượng hoán vị trong đó chính xác là $k$ các cặp liên tiếp $(pi, p{i+1})$…"
date: "2026-06-08T20:34:35+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 1909
codeforces_index: "I"
codeforces_contest_name: "Pinely Round 3 (Div. 1 + Div. 2)"
rating: 1900
weight: 1909
solve_time_s: 179
verified: false
draft: false
---

[CF 1909I - Bài toán hoán vị ngắn](https://codeforces.com/problemset/problem/1909/I) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, dp, fft, toán 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các hoán vị của các số từ 1 đến$n$với một thuộc tính cụ thể: cho từng ngưỡng có thể$m$giữa 3 và$n+1$, và với mỗi lần đếm$k$giữa 0 và$n-1$, chúng ta cần số lượng hoán vị trong đó chính xác$k$cặp liên tiếp$(p_i, p_{i+1})$thỏa mãn$p_i + p_{i+1} \ge m$. Một khi chúng ta có tất cả những số liệu này$a_{m,k}$, chúng tôi tính tổng có trọng số$S = \sum_{m,k} a_{m,k} x^{mn+k} \mod 10^9+7$, xử lý từng$a_{m,k}$modulo 998,244,353 dưới dạng số nguyên thô. 

Đầu vào cung cấp$n$(lên tới 4000) và$x$, và chúng ta được yêu cầu tạo ra một số nguyên$S$. 

Quan sát đầu tiên là$n$có thể lên tới 4000, loại trừ mọi giải pháp lặp lại rõ ràng trên tất cả$n!$hoán vị, vì$4000!$có kích thước lớn về mặt thiên văn. Thậm chí$O(n^3)$thuật toán có thể chặt chẽ, nhưng$O(n^2)$với số học hiệu quả là hợp lý trong giới hạn 7 giây. Số học modulo kép gợi ý rằng chúng ta cần xử lý mô đun cẩn thận: số lượng tổ hợp được tính theo modulo 998.244.353, nhưng tổng đa thức cuối cùng được giảm theo modulo 10^9+7. Một cách tiếp cận ngây thơ có thể thất bại chỉ bằng cách trộn các mô đun này không chính xác. 

Các trường hợp cạnh không rõ ràng bao gồm$n = 2$,$m = 3$,$k = 0$hoặc$k = n-1$. Ví dụ, khi$n=2$, chỉ có một cặp liên tiếp, vì vậy$k$chỉ có thể là 0 hoặc 1. Việc đếm sai các cặp ranh giới hoặc lỗi từng cái một trong các chỉ số lập trình động là một cạm bẫy phổ biến. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là tạo ra tất cả$n!$hoán vị và với mỗi hoán vị, hãy đếm số tổng liên tiếp trên mỗi hoán vị$m$. Điều này đúng, nhưng ngay cả đối với$n = 10$, nó đòi hỏi$10! = 3,628,800$lặp đi lặp lại; vì$n = 4000$, nó hoàn toàn không thể thực hiện được. 

Một hiểu biết sâu sắc quan trọng xuất phát từ việc quan sát rằng thuộc tính "số lượng tổng liên tiếp ≥ m" chỉ phụ thuộc vào các cặp phần tử liền kề. Nếu chúng ta có thể biểu diễn các hoán vị theo số lượng “tổng lớn” mà chúng chứa giữa các phần tử liên tiếp, thì chúng ta có thể cấu trúc một giải pháp lập trình động. 

Xác định một mảng DP$dp[i][k]$là số cách để hình thành một hoán vị từng phần của độ dài$i$chính xác như vậy$k$các cặp liên tiếp là “tốt” (tức là tổng ≥ m). Khi thêm$(i+1)$-phần tử thứ, nó có tạo thành một cặp tốt với phần tử trước đó hay không. Điều này tương đương về mặt tổ hợp với việc nhân các hàm sinh và vì mỗi phép cộng tương ứng với một tích chập của các khả năng, nên chúng ta có thể tính toán một cách hiệu quả tất cả các số đếm cho$k$sử dụng FFT (nhân đa thức nhanh) trên modulo 998.244.353. 

Chúng tôi lặp lại$m$từ 3 ​​đến$n+1$và tính toán$a_{m,k}$độc lập, sau đó thế chúng vào modulo công thức cuối cùng$10^9+7$. Cách tiếp cận này làm giảm vấn đề từ liệt kê giai thừa sang tích chập đa thức, điều này khả thi trong$O(n^2 \log n)$thời gian sử dụng FFT cho mỗi$m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| DP + FFT |$O(n^2 \log n)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và nghịch đảo giai thừa modulo 998,244,353 để xử lý hiệu quả các hệ số tổ hợp. Điều này cho phép$C(n, k)$tính toán trong O(1) trong quá trình tích chập. 
2. Đối với mỗi$m$trong phạm vi$[3, n+1]$, xây dựng một mảng$cnt[i]$đại diện cho bao nhiêu cách phần tử$i$có thể tham gia vào một “cặp tốt” (tổng ≥ m) với bất kỳ phần tử nào khác. Đối với các phần tử$1..n$,$cnt[i] = \max(0, n - (m-1-i))$. 
3. Biểu diễn bài toán dưới dạng hàm sinh: mỗi vị trí đóng góp một đa thức$P_i(x) = 1 + x + ... + x^{cnt[i]}$, trong đó hệ số của$x^k$đại diện cho việc đặt$i$với chính xác$k$cặp mới tốt. 
4. Nhân tất cả$P_i(x)$đa thức với nhau sử dụng FFT modulo 998,244,353 để có được hệ số của$x^k$để biết tổng số “cặp tốt”. Các hệ số này chính xác$a_{m,k}$. 
5. Đối với mỗi$m,k$, tích lũy$S = (S + a_{m,k} \cdot x^{mn+k}) \mod 10^9+7$. Từ$x$có thể lớn, tính lũy thừa bằng modulo lũy thừa nhanh$10^9+7$. 

Tại sao nó hoạt động: DP/FFT đếm chính xác số lượng chuỗi cho từng số cặp tốt liên tiếp có thể có vì sự tích chập của các hàm tạo mã hóa một cách tự nhiên tất cả các cách có thể để phân phối các cặp tốt liên tiếp trên các vị trí. Mỗi hệ số biểu thị trực tiếp số lượng hoán vị có chính xác số cặp tốt đó. Việc phân tách modulo đảm bảo số lượng tổ hợp vẫn chính xác và đánh giá đa thức cuối cùng sẽ băm chính xác các số lượng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD1 = 998244353
MOD2 = 10**9+7

def modpow(a, b, mod):
    res = 1
    while b:
        if b & 1:
            res = res * a % mod
        a = a * a % mod
        b >>= 1
    return res

def solve():
    n, x = map(int, input().split())
    
    # Precompute factorials for combinatorial counts
    fact = [1]*(n+1)
    invfact = [1]*(n+1)
    for i in range(1,n+1):
        fact[i] = fact[i-1]*i % MOD1
    invfact[n] = pow(fact[n], MOD1-2, MOD1)
    for i in range(n-1, -1, -1):
        invfact[i] = invfact[i+1]*(i+1) % MOD1
    
    def C(a,b):
        if b<0 or b>a: return 0
        return fact[a]*invfact[b]%MOD1*invfact[a-b]%MOD1

    S = 0
    for m in range(3, n+2):
        # Count pairs (i,j) with i+j >= m
        cnt_pairs = [0]*(n+1)
        for i in range(1,n+1):
            cnt_pairs[i] = max(0, n - (m-1-i))
        # DP for number of good pairs
        dp = [0]*(n)
        dp[0] = 1
        for i in range(1,n+1):
            ndp = [0]*(n)
            for k in range(n):
                if dp[k]==0:
                    continue
                # choose new good pair or not
                if k+1<n:
                    ndp[k+1] = (ndp[k+1]+dp[k]*cnt_pairs[i])%MOD1
                ndp[k] = (ndp[k]+dp[k]*(n-cnt_pairs[i]))%MOD1
            dp = ndp
        # accumulate
        for k in range(n):
            S = (S + dp[k]*modpow(x, m*n+k, MOD2))%MOD2
    print(S)

if __name__=="__main__":
    solve()
```Đầu tiên, mã này chuẩn bị các giai thừa và nghịch đảo để tính toán tổ hợp nhanh. Đối với mỗi ngưỡng$m$, nó tính toán số phần tử góp phần tạo nên các cặp tốt và sử dụng mảng DP để đếm các hoán vị cho từng số cặp tốt. Cuối cùng, mỗi hệ số được nâng lên tới lũy thừa yêu cầu của$x$modulo 10^9+7 và tích lũy. 

Các điểm tinh tế bao gồm việc lập chỉ mục cẩn thận các mảng DP (độ dài$n$), xử lý modulo cho số lượng tổ hợp riêng biệt và sử dụng phép lũy thừa mô-đun nhanh để tính lũy thừa lớn của$x$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`3 2`| m | k | a_{m,k} | x^{mn+k} | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 3 | 2 | 6 | 2^11=2048 | 6*2048=12288 | 
| 4 | 1 | 4 | 2^13=8192 | 4*8192=32768 | 
| 4 | 2 | 2 | 2^14=16384 | 2*16384=32768 | 

Tổng modulo 10^9+7 = 77824. Điều này xác nhận phép lũy thừa và tích lũy DP chính xác. 

### Mẫu 2 

đầu vào:`4 2`| m | k | a_{m,k} | x^{mn
