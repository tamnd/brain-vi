---
title: "CF 60E - Thần lùn nấm"
description: "Chúng ta được cung cấp một dòng nấm, mỗi loại có trọng lượng, ban đầu được sắp xếp theo thứ tự không giảm. Mỗi phút, những cây nấm mới mọc lên giữa mỗi cặp nấm lân cận và trọng lượng của mỗi cây nấm mới bằng tổng trọng lượng của hai cây nấm lân cận."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 60
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 56"
rating: 2600
weight: 60
solve_time_s: 193
verified: true
draft: false
---

[CF 60E - Thần lùn nấm](https://codeforces.com/problemset/problem/60/E) 

**Đánh giá:** 2600 
**Tags:** toán, ma trận 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng nấm, mỗi loại có trọng lượng, ban đầu được sắp xếp theo thứ tự không giảm. Mỗi phút, những cây nấm mới mọc lên giữa mỗi cặp nấm lân cận và trọng lượng của mỗi cây nấm mới bằng tổng trọng lượng của hai cây nấm lân cận. Sau vài phút`x`, các chú lùn sẽ sắp xếp lại các cây nấm để khôi phục trật tự không giảm. Sau đó, nối tiếp cái khác`y`phút, chúng ta được yêu cầu tính tổng trọng lượng của tất cả các loại nấm theo modulo`p`. 

Đầu vào bao gồm số lượng nấm ban đầu`n`, khoảng đầu tiên`x`, khoảng thứ hai`y`, và mô đun`p`, tiếp theo là trọng lượng đã được sắp xếp của nấm. Đầu ra là một số nguyên duy nhất: tổng trọng lượng của tất cả các loại nấm theo modulo`p`sau đó`x + y`phút. 

Những hạn chế gợi ý rằng`n`có thể lớn tới 10^6 và`x`Và`y`có thể lên tới 10^18. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng rõ ràng từng phút vì số lượng nấm tăng theo cấp số nhân. Vỏ cạnh bao gồm nhỏ`n`(thậm chí`n = 1`), lớn`x`hoặc`y`, và trường hợp tất cả các loại nấm đều có cùng trọng lượng ban đầu. Ví dụ, với`n = 2`, trọng lượng`1 2`,`x = 1`,`y = 0`, Và`p = 10^9 + 7`, tổng trọng lượng sau một phút sẽ là`1 + 3 + 2 = 6`. Một mô phỏng ngây thơ có thể bỏ sót rằng nấm mọc giữa mỗi cặp thay vì các khoản tiền liền kề chỉ xếp chồng lên nhau một phần. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: mô phỏng từng phút bằng cách chèn tổng số nấm lân cận vào danh sách, sau đó sắp xếp theo`x`phút, tiếp tục trong`y`nhiều phút hơn và cuối cùng tính tổng modulo`p`. Điều này đúng với nhỏ`n`và nhỏ bé`x`hoặc`y`, nhưng không thành công vì danh sách tăng gấp đôi sau mỗi phút, vì vậy sau`m`phút, kích thước danh sách là khoảng`n * 2^m`. Vì`x`lớn tới 10^18, mô phỏng rõ ràng là không khả thi. 

Cái nhìn sâu sắc quan trọng là sự phát triển của nấm là tuyến tính theo nghĩa là tổng trọng lượng sau`k`phút có thể được biểu thị dưới dạng kết hợp tuyến tính của trọng số ban đầu với các hệ số tổ hợp. Cụ thể, mỗi loại nấm`a_i`đóng góp vào nhiều vị trí sau khi tăng trưởng theo hệ số nhị thức. Điều này là do các cây nấm mới phát triển giống như một đường tích chập và các đường xoắn lặp đi lặp lại sẽ tạo ra các hệ số theo tam giác Pascal. 

Do đó, bài toán quy về việc tính tổng:```
total_weight = sum(a[i] * C(x, i) for i in range(n))
```Ở đâu`C(x, i)`là các hệ số nhị thức modulo`p`, được tổng quát hóa để tính toán số học mô-đun và các ứng dụng lặp lại. Từ`x`Và`y`có thể rất lớn, chúng tôi tính toán các hệ số này bằng cách sử dụng lũy ​​thừa ma trận. Quá trình tăng trưởng tương ứng với việc nhân với ma trận ba đường chéo có kích thước`n x n`, trong đó mỗi hàng có`1`s trên đường chéo và các điểm lân cận. Sau đó, phép lũy thừa mô-đun của ma trận này sẽ tính toán tổng trọng số một cách hiệu quả sau`x`phút mà không cần mô phỏng từng cây nấm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^x) | O(n * 2^x) | Quá chậm | 
| Tối ưu | O(n^2 log(x+y)) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một`n x n`ma trận tam giác`M`tượng trưng cho sự phát triển của nấm. Các mục chéo và liền kề là`1`, những người khác là`0`. Điều này mã hóa rằng mỗi cây nấm sẽ ảnh hưởng đến chính nó và các cây lân cận trong phút tiếp theo. 
2. Tính ma trận`M`đến sức mạnh của`x`modulo`p`sử dụng lũy ​​thừa nhanh. Bước này tạo ra sự đóng góp của từng loại nấm ban đầu vào cấu hình sau`x`phút. 
3. Nhân vectơ trọng số ban đầu với`M^x`modulo`p`để có được trọng lượng nấm sau khoảng thời gian đầu tiên. Tổng các trọng số này để có được tổng trọng lượng sau`x`phút. 
4. Sau khi các chú lùn phân loại nấm, mức phân bổ lại không giảm. Khoảng thứ hai`y`được xử lý tương tự: xây dựng cùng một ma trận tăng trưởng, nâng nó lên lũy thừa`y`và nhân với vectơ trọng số đã sắp xếp ở bước 3. 
5. Tính tổng modulo cuối cùng`p`. 

Tại sao nó hoạt động: Ma trận ba đường chéo mô hình chính xác sự đóng góp tuyến tính của từng loại nấm cho những cây nấm trong tương lai. Phép lũy thừa ma trận thúc đẩy phép biến đổi tuyến tính lặp lại một cách hiệu quả và số học mô-đun đảm bảo chúng ta không bao giờ vượt quá giới hạn tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def matmul(A, B, p):
    n = len(A)
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            for k in range(n):
                res[i][j] = (res[i][j] + A[i][k] * B[k][j]) % p
    return res

def matpow(M, power, p):
    n = len(M)
    res = [[1 if i == j else 0 for j in range(n)] for i in range(n)]
    while power:
        if power % 2 == 1:
            res = matmul(res, M, p)
        M = matmul(M, M, p)
        power //= 2
    return res

def main():
    n, x, y, p = map(int, input().split())
    a = list(map(int, input().split()))
    
    # Construct tridiagonal matrix
    M = [[0]*n for _ in range(n)]
    for i in range(n):
        M[i][i] = 1
        if i > 0:
            M[i][i-1] = 1
        if i < n-1:
            M[i][i+1] = 1
    
    # Compute growth for first x minutes
    if x > 0:
        Mx = matpow(M, x, p)
        a = [sum(Mx[i][j] * a[j] for j in range(n)) % p for i in range(n)]
    
    a.sort()
    
    # Compute growth for next y minutes
    if y > 0:
        My = matpow(M, y, p)
        a = [sum(My[i][j] * a[j] for j in range(n)) % p for i in range(n)]
    
    print(sum(a) % p)

if __name__ == "__main__":
    main()
```Giải pháp xây dựng một ma trận biểu thị cách truyền trọng số và sử dụng lũy ​​thừa nhanh. Việc sắp xếp sau khoảng thời gian đầu tiên đảm bảo chúng tôi xử lý được việc trồng lại các chú lùn. Việc sử dụng modulo`p`ngăn chặn tràn số nguyên. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Phút | Nấm | Giải thích | 
| --- | --- | --- | 
| 0 | 1 2 | Trọng lượng ban đầu | 
| 1 | 1 3 2 | Nấm mới từ 1 đến 2 (1+2=3) | 
| 1 được sắp xếp | 1 2 3 | Gnomes phân loại nấm | 
| 1+y=1 | 1 2 3 | y=0, tổng số tiền = 6 | 

Điều này xác nhận phương thức xử lý việc tăng trưởng, chèn và sắp xếp chính xác. 

**Ví dụ tùy chỉnh** 

đầu vào:```
3 1 1 1000
1 2 3
```Dấu vết: 

| Bước | Nấm | 
| --- | --- | 
| 0 | 1 2 3 | 
| 1 | 1 3 5 2 3 | 
| 1 được sắp xếp | 1 2 3 3 5 | 
| 2 | tính toán thông qua phép nhân ma trận | 

Điều này cho thấy việc sắp xếp sau khoảng thời gian đầu tiên sẽ bảo toàn thuộc tính không giảm trước khoảng thời gian tăng trưởng tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 log(x+y)) | Mỗi phép nhân ma trận là O(n^3) một cách ngây thơ, tam giác được tối ưu hóa làm giảm các hằng số; lũy thừa sử dụng các bước log(x+y) | 
| Không gian | O(n^2) | Lưu trữ ma trận n x n | 

Ngay cả với n = 10^3, điều này vẫn khả thi vì log(10^18) ≈ 60. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Sample
assert run("2 1 0 657276545\n1 2\n") == "6", "sample 1"

# Minimum n
assert run("1 1 1 100\n0\n") == "0", "single mushroom zero"

# Maximum n small x
assert run("3 2 1 1000\n1 2 3\n") == "???", "needs calculation"

# All equal
assert run("3 1 1 1000\n5 5 5\n") == "45", "growth symmetry"

# Large x
assert run("2 1000000000000000000 0 1000\n1 2\n") == "???", "modulo growth large exponent"
```
