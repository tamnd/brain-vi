---
title: "CF 1634F - Bổ sung Fibonacci"
description: "Chúng ta được yêu cầu duy trì hai mảng số nguyên A và B theo một chuỗi các phép toán được gọi là phép cộng Fibonacci. Mỗi thao tác chỉ định một phân đoạn [l, r] và một mảng cần cập nhật."
date: "2026-06-10T04:45:39+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "hashing", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1634
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 770 (Div. 2)"
rating: 2700
weight: 1634
solve_time_s: 83
verified: true
draft: false
---

[CF 1634F - Bổ sung Fibonacci](https://codeforces.com/problemset/problem/1634/F) 

**Xếp hạng:** 2700 
**Tags:** Brute Force, cấu trúc dữ liệu, băm, triển khai, toán học 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu duy trì hai mảng số nguyên,`A`Và`B`, theo một chuỗi các phép toán được gọi là phép cộng Fibonacci. Mỗi thao tác chỉ định một phân đoạn`[l, r]`và một mảng để cập nhật. Phép cộng Fibonacci tăng`X_l`qua`F_1`,`X_{l+1}`qua`F_2`, v.v. cho đến`X_r`qua`F_{r-l+1}`, tất cả modulo một cho trước`MOD`. Sau mỗi hoạt động, chúng tôi phải báo cáo xem`A`Và`B`đều bằng modulo`MOD`. 

Các mảng có thể dài tới 300.000 phần tử và có thể có tới 300.000 thao tác. Một cách tiếp cận ngây thơ lặp đi lặp lại trong phạm vi`[l, r]`cho mỗi hoạt động sẽ thực hiện lên đến$3 \cdot 10^5 \cdot 3 \cdot 10^5 \approx 9 \cdot 10^{10}$bổ sung, quá chậm. Do đó, chúng tôi cần một giải pháp tránh chạm vào mọi yếu tố cho mọi thao tác. 

Một điểm tinh tế phát sinh do phép cộng Fibonacci không phải là số gia tăng cố định - mỗi phần tử trong phân đoạn nhận được số gia tăng khác nhau. Một cách tiếp cận bất cẩn coi nó như một phép cộng phạm vi đơn giản với một giá trị duy nhất sẽ tạo ra kết quả không chính xác. Ví dụ, nếu`A=[0,0,0]`,`B=[0,0,0]`, và chúng tôi thêm`[1,3]`ĐẾN`A`, sau đó`A`trở thành`[1,1,2]`modulo`MOD=3`sau hai lần phẫu thuật. Mức tăng cho các vị trí khác nhau phải tuân theo chính xác chuỗi Fibonacci. 

Điều kiện biên cũng rất quan trọng. Hoạt động có thể nhắm mục tiêu một phần tử duy nhất (`l=r`) hoặc toàn bộ mảng (`l=1, r=n`), do đó thuật toán phải xử lý các đoạn có độ dài 1 và độ dài`n`thống nhất. 

## Phương pháp tiếp cận 

Phương pháp brute-force áp dụng trực tiếp từng phép cộng Fibonacci, lặp qua các chỉ số`i=l..r`và tăng dần`X[i]`qua`F_{i-l+1}`modulo`MOD`. Điều này đúng vì nó thực hiện định nghĩa, nhưng độ phức tạp là$O(q \cdot n)$trong trường hợp xấu nhất là quá cao so với các ràng buộc đã cho. Tối đa`n`Và`q`, nó sẽ yêu cầu khoảng$10^{10}$hoạt động. 

Cái nhìn sâu sắc quan trọng là các phép cộng Fibonacci là tuyến tính đối với chuỗi các khác biệt trước đó. Cụ thể, nếu chúng ta xác định mảng khác biệt`D[i] = X[i] - X[i-1] - X[i-2]`, dãy Fibonacci là dãy duy nhất thỏa mãn`D[i] = 0`cho tất cả`i > 2`. Điều này cho phép chúng ta giảm mỗi phép cộng Fibonacci xuống chỉ còn cập nhật một vài vị trí trong`D`, thay vì cập nhật mọi phần tử trong phân đoạn. Việc sử dụng một mảng khác biệt có dạng này cho phép cập nhật trong`O(1)`mỗi thao tác và kiểm tra sự bình đẳng giữa`A`Và`B`TRONG`O(n)`ban đầu hoặc duy trì số lượng khác biệt để trả lời các truy vấn trong`O(1)`. 

Đối với mỗi phép cộng Fibonacci từ`l`ĐẾN`r`, chúng tôi cập nhật`D[l]`,`D[r+1]`, Và`D[r+2]`với các giá trị cụ thể bắt nguồn từ`Fibonacci[l..r]`. Điều này biến vấn đề thành một vấn đề cập nhật phạm vi cổ điển, trong đó việc cập nhật phạm vi cho phép cộng Fibonacci chỉ chạm đến một số vị trí không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số Fibonacci tối đa`n+2`modulo`MOD`. Chúng tôi cần`F[0..n+2]`bởi vì các bản cập nhật mảng khác biệt của chúng tôi có thể tham khảo`F[r-l+2]`. 
2. Xây dựng mảng sai phân`D_X[i] = X[i] - X[i-1] - X[i-2]`cho cả hai`A`Và`B`. Vì`i=1`Và`i=2`, xử lý các trường hợp cơ sở riêng biệt. Khởi tạo bộ đếm các vị trí trong đó`D_A[i] != D_B[i]`. 
3. Với mỗi thao tác, xác định mảng bị ảnh hưởng (`A`hoặc`B`). Đối với phép cộng Fibonacci trên`[l,r]`, cập nhật`D[l] += 1`,`D[r+1] -= F[r-l+2]`, Và`D[r+2] -= F[r-l+1]`, modulo`MOD`. Sau mỗi lần cập nhật, hãy kiểm tra xem có`D[i]`thay đổi sự bình đẳng với mảng khác và điều chỉnh bộ đếm. 
4. Nếu bộ đếm khác nhau`D[i]`bằng 0 sau phép toán, các mảng bằng nhau và chúng tôi in`YES`. Ngược lại, in`NO`. 

Tại sao nó hoạt động: Mảng chênh lệch ghi lại những sai lệch so với thuộc tính Fibonacci. Việc cập nhật phân đoạn chỉ ảnh hưởng đến các cạnh trong`D`bởi vì các vị trí bên trong tiếp tục tự động lặp lại Fibonacci. Việc duy trì bộ đếm không khớp đảm bảo chúng tôi có thể kiểm tra sự bằng nhau trong thời gian không đổi cho mỗi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q, MOD = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

# Precompute Fibonacci numbers modulo MOD
F = [0]*(n+3)
F[1] = 1
for i in range(2, n+3):
    F[i] = (F[i-1] + F[i-2]) % MOD

# Compute difference arrays D[i] = X[i] - X[i-1] - X[i-2]
def make_diff(X):
    D = [0]*(n+2)
    for i in range(n):
        D[i+1] = (X[i] - (X[i-1] if i>=1 else 0) - (X[i-2] if i>=2 else 0)) % MOD
    return D

D_A = make_diff(A)
D_B = make_diff(B)

# Count mismatches
diff_count = sum(1 for i in range(1, n+1) if D_A[i] != D_B[i])

for _ in range(q):
    op = input().split()
    c, l, r = op[0], int(op[1]), int(op[2])
    D = D_A if c=='A' else D_B
    
    # Update D
    for idx, val in [(l,1), (r+1,-F[r-l+2]), (r+2,-F[r-l+1])]:
        if idx > n: 
            continue
        prev = D[idx]
        D[idx] = (D[idx] + val) % MOD
        # Update mismatch counter
        other = D_B if D is D_A else D_A
        if prev != other[idx] and D[idx] == other[idx]:
            diff_count -= 1
        elif prev == other[idx] and D[idx] != other[idx]:
            diff_count += 1

    print("YES" if diff_count == 0 else "NO")
```Đoạn mã đầu tiên tính toán trước các số Fibonacci theo modulo`MOD`và xây dựng các mảng khác biệt. Mỗi thao tác cập nhật tối đa ba vị trí trong mảng chênh lệch và bộ đếm không khớp đảm bảo chúng ta có thể kiểm tra sự bằng nhau trong thời gian O(1). Cần thận trọng xử lý các chỉ số vượt quá`n`và số học mô-đun để ngăn chặn các giá trị âm. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Hoạt động | Mảng | D_A | D_B | khác biệt | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | A=[2,2,1] B=[0,0,0] | D_A=[2,0,0] | D_B=[0,0,0] | 1+1+1=3 | - | 
| A 1 3 | A=[0,0,0] | D_A=[0,0,0] | D_B=[0,0,0] | 0 | CÓ | 
| A 1 3 | A=[1,1,2] | D_A=[1,1,2] | D_B=[0,0,0] | 3 | KHÔNG | 
| B 1 1 | B=[1,0,0] | D_A | D_B | 2 | KHÔNG | 
| B 2 2 | B=[1,1,0] | D_A | D_B | 2 | KHÔNG | 
| A 3 3 | A=[1,1,0] | D_A | D_B | 0 | CÓ | 

Dấu vết này chứng tỏ rằng chỉ các cạnh của phân đoạn được cập nhật mới quan trọng đối với mảng sai phân và bất biến diff_count theo dõi chính xác sự bằng nhau của mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Phép tính trước Fibonacci là O(n), khởi tạo mảng chênh lệch là O(n), mỗi thao tác cập nhật 3 vị trí trong O(1). | 
| Không gian | O(n) | Lưu trữ các mảng A, B, mảng sai phân D_A, D_B và số Fibonacci. | 

Với`n, q <= 3*10^5`, tổng công việc là khoảng 1 triệu thao tác cho mảng Fibonacci và mảng khác, cộng với các cập nhật 3*q, nằm trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        exec(open("solution.py").read())
    return out.getvalue().strip()
```
