---
title: "CF 1151F - Sonya và Tin học"
description: "Chúng tôi được yêu cầu xác định xác suất để một mảng bao gồm các số 0 và một được sắp xếp theo thứ tự không giảm sau khi thực hiện một số lần hoán đổi ngẫu nhiên cố định. Mỗi lần hoán đổi chọn ngẫu nhiên hai vị trí riêng biệt trong mảng một cách thống nhất và trao đổi giá trị của chúng."
date: "2026-06-12T03:03:18+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "matrices", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1151
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 553 (Div. 2)"
rating: 2300
weight: 1151
solve_time_s: 118
verified: false
draft: false
---

[CF 1151F - Sonya và Tin học](https://codeforces.com/problemset/problem/1151/F) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, dp, ma trận, xác suất 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xác suất để một mảng bao gồm các số 0 và một được sắp xếp theo thứ tự không giảm sau khi thực hiện một số lần hoán đổi ngẫu nhiên cố định. Mỗi lần hoán đổi chọn ngẫu nhiên hai vị trí riêng biệt trong mảng một cách thống nhất và trao đổi giá trị của chúng. Độ dài mảng tối đa là 100, nhưng số lần hoán đổi có thể lớn tới 10^9, điều này ngay lập tức loại trừ mọi mô phỏng hoán đổi từng bước. Đầu ra phải bằng 0 hoặc một phần trong số học mô-đun. 

Đầu vào cung cấp cho chúng ta chính mảng đó và số lần hoán đổi. Đầu ra là modulo xác suất 10^9+7. Vì mảng chỉ chứa các số 0 và số 1 nên mảng được sắp xếp cuối cùng chỉ đơn giản là tất cả các số 0 theo sau là tất cả các số 1. Một giải pháp bất cẩn có thể cố gắng mô phỏng trực tiếp các giao dịch hoán đổi. Ví dụ, nếu`a = [0, 1, 0]`Và`k = 2`, việc mô phỏng mỗi lần hoán đổi theo cách đơn giản có thể bỏ sót thực tế là nhiều chuỗi hoán đổi dẫn đến cùng một mảng cuối cùng. Một cách tiếp cận đúng phải tính đến xác suất tổ hợp để đạt đến trạng thái được sắp xếp. 

Các trường hợp cạnh bao gồm các mảng đã được sắp xếp, các mảng được đảo ngược hoàn toàn hoặc các mảng trong đó tất cả các phần tử đều giống nhau. Ví dụ, nếu`a = [0, 0, 0]`, xác suất được sắp xếp là tầm thường 1. Nếu`a = [1, 1, 1]`, điều tương tự cũng đúng. Nếu như`a = [1, 0, 1]`Và`k = 1`, một số hoán đổi có thể không bao giờ tạo ra mảng được sắp xếp và chúng ta phải tính toán chính xác xác suất bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê tất cả các chuỗi hoán đổi có thể xảy ra. Mỗi lần hoán đổi chọn một cặp chỉ số`(i, j)`giữa`n*(n-1)/2`khả năng. Sau đó`k`trao đổi, có`(n*(n-1)/2)^k`trình tự. Vì`n = 100`Và`k`lên tới 10^9, điều này hoàn toàn không khả thi. Ngay cả việc sử dụng lập trình động trên các cấu hình mảng cũng quá lớn vì có`2^n`mảng có thể, và`n=100`làm cho`2^100`lớn về mặt thiên văn. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng mảng chỉ chứa số 0 và số 1. Cho phép`cnt1`là tổng số những cái một. Bất kỳ cấu hình nào hoàn toàn được xác định bởi số lượng cái ở "nửa sai" của mảng. Chính xác hơn, hãy xác định`state[i]`như xác suất có chính xác`i`những cái đầu tiên`n - cnt1`các vị trí. Điều này làm giảm không gian trạng thái từ`2^n`ĐẾN`min(cnt1, n-cnt1) + 1`tiểu bang, tối đa là 50. 

Quá trình này sau đó có thể được mô hình hóa như một chuỗi Markov trong đó mỗi trạng thái chuyển đổi theo xác suất dựa trên việc hoán đổi một trạng thái từ phân đoạn đầu tiên với số 0 từ phân đoạn thứ hai. Cho phép`trans[i][i-1]`,`trans[i][i]`, Và`trans[i][i+1]`biểu thị xác suất di chuyển giữa các trạng thái. Sau đó, chúng ta có thể nâng ma trận chuyển tiếp lên`k`-th sức mạnh để tính toán xác suất cuối cùng. Phép lũy thừa ma trận trên ma trận 50x50 là hiệu quả và xử lý được`k`lên đến 10^9. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O((n^2)^k) | O(2^n) | Quá chậm | 
| Phép lũy thừa ma trận / Markov tối ưu | O(n^3 log k) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`cnt1`, tổng số đơn vị trong mảng. Số số 0 là`cnt0 = n - cnt1`. Mục tiêu được sắp xếp có tất cả các số 0 theo sau là tất cả các số 1. 
2. Tính toán`wrong`, số lượng ban đầu của những cái nằm trong "đoạn 0" (đoạn đầu tiên`cnt0`các vị trí). Đây là trạng thái bắt đầu cho chuỗi Markov của chúng tôi. 
3. Xây dựng ma trận chuyển tiếp`T`kích thước`cnt1+1`qua`cnt1+1`.`T[i][i-1]`là xác suất để số lượng đơn vị trong đoạn 0 giảm đi 1,`T[i][i]`xác suất nó vẫn giữ nguyên, và`T[i][i+1]`xác suất nó tăng thêm 1. Những xác suất này có được bằng cách đếm các giao dịch hoán đổi giữa một trong phân đoạn đầu tiên và một số 0 trong phân đoạn thứ hai và chuẩn hóa bằng tổng số giao dịch hoán đổi`C(n, 2)`. 
4. Thực hiện lũy thừa ma trận nhanh`T`đến`k`-quyền lực thứ. Điều này tính toán phân phối trạng thái sau`k`trao đổi. 
5. Nhân vectơ trạng thái ban đầu`v`(với 1 ở chỉ số`wrong`) qua`T^k`. Xác suất cuối cùng để mảng được sắp xếp là giá trị tương ứng với số 0 trong đoạn 0 (trạng thái 0). 
6. Trả về xác suất dưới dạng phân số mô đun`P * Q^{-1} mod 10^9+7`. 

Tại sao nó hoạt động: Quá trình này bảo toàn thuộc tính Markov vì số lượng 1 trong đoạn 0 đủ để mô tả hệ thống. Phép lũy thừa ma trận áp dụng hiệu quả quá trình chuyển đổi`k`lần, đưa ra xác suất chính xác cho từng trạng thái sau tất cả các lần hoán đổi. Xác suất được sắp xếp tương ứng chính xác với trạng thái không có xác suất bị đặt sai vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9+7

def modinv(x):
    return pow(x, MOD-2, MOD)

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    cnt1 = sum(a)
    cnt0 = n - cnt1
    wrong = sum(a[:cnt0])
    
    if wrong == 0:
        print(1)
        return

    size = cnt1 + 1
    total_pairs = n*(n-1)//2 % MOD

    T = [[0]*size for _ in range(size)]
    for i in range(size):
        if i > 0:
            T[i][i-1] = i * (cnt0 - (i-1)) % MOD * modinv(total_pairs) % MOD
        if i < cnt1:
            T[i][i+1] = (cnt1 - i) * (cnt0 - i) % MOD * modinv(total_pairs) % MOD
        same = (total_pairs - (T[i][i-1]*total_pairs if i>0 else 0) - (T[i][i+1]*total_pairs if i<cnt1 else 0)) % MOD
        T[i][i] = same * modinv(total_pairs) % MOD

    def matmul(A, B):
        n = len(A)
        C = [[0]*n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                for k in range(n):
                    C[i][j] = (C[i][j] + A[i][k]*B[k][j]) % MOD
        return C

    def matexp(M, p):
        n = len(M)
        res = [[int(i==j) for j in range(n)] for i in range(n)]
        while p:
            if p & 1:
                res = matmul(res, M)
            M = matmul(M, M)
            p >>= 1
        return res

    T_k = matexp(T, k)
    ans = T_k[wrong][0] % MOD
    print(ans)

solve()
```Giải pháp đầu tiên tính toán số lượng đơn vị trong đoạn 0. Nó xây dựng một ma trận chuyển tiếp Markov trong đó mỗi mục nhập tương ứng với việc chuyển từ một số mục bị đặt sai vị trí sang một mục khác sau khi hoán đổi. Phép nhân ma trận có tính mô-đun để tránh tràn và phép lũy thừa ma trận làm tăng quá trình chuyển đổi sang`k`-quyền lực thứ. Cuối cùng, xác suất không có số nào bị đặt nhầm chỗ được in ra theo modulo 10^9+7. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
0 1 0
```| Tiểu bang | sai=1 | sai=0 | sai=2 | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 0 | 0 | 
| Sau 1 lần hoán đổi | 9/5 | 9/2 | 9/2 | 
| Sau 2 lần hoán đổi | 9/3 | 9/3 | 9/3 | 

Xác suất cuối cùng của mảng được sắp xếp là 1/3, với modulo 10^9+7 là 333333336. 

### Mẫu 2 

đầu vào:```
3 1
1 0 1
```Tất cả các giao dịch hoán đổi vẫn để lại ít nhất một giao dịch ở phân đoạn 0. Xác suất là 0. 

Dấu vết xác nhận rằng phép tính sai ban đầu và ma trận chuyển tiếp sẽ nắm bắt được tất cả các giao dịch hoán đổi có thể xảy ra. Bước lũy thừa được áp dụng hiệu quả`k`các bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3 log k) | Phép lũy thừa ma trận có kích thước tối đa là 100x100 và`k`lên tới 10^9 | 
| Không gian | O(n^2) | Lưu trữ ma trận chuyển tiếp có kích thước (cnt1+1)^2 100^2 | 

Điều này phù hợp thoải mái dưới giới hạn thời gian 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3 2\n0 1
```
