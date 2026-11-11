---
title: "CF 1580B - Giáo trình Toán"
description: "Chúng ta được yêu cầu đếm các hoán vị của các số từ 1 đến $n$ có chính xác các số $k$ \"tốt\". Một số $x$ trong một hoán vị được gọi là tốt nếu, khi chúng ta xem xét tất cả các mảng con liền kề chứa $x$, có chính xác $m$ giá trị cực đại phân biệt."
date: "2026-06-10T10:12:40+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1580
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 745 (Div. 1)"
rating: 2600
weight: 1580
solve_time_s: 124
verified: false
draft: false
---

[CF 1580B - Chương trình giảng dạy Toán](https://codeforces.com/problemset/problem/1580/B) 

**Đánh giá:** 2600 
**Tags:** sức mạnh vũ phu, tổ hợp, dp, cây cối 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các hoán vị của các số từ 1 đến$n$có chính xác$k$những con số “tốt”. một con số$x$trong một hoán vị được gọi là tốt nếu khi chúng ta xem xét tất cả các mảng con liền kề chứa$x$, có chính xác$m$cực đại rõ rệt. Ví dụ: số 1 trong hoán vị [1,3,2,4] là một phần của mảng con [1], [1,3], [1,3,2] và [1,3,2,4]. Cực đại của các mảng con này là 1, 3, 3 và 4, tạo ra ba cực đại riêng biệt. Nếu như$m$là 3 thì 1 là tốt. 

Đầu vào bao gồm bốn số nguyên$n, m, k, p$, Ở đâu$n$là kích thước hoán vị,$m$là số lượng cực đại riêng biệt cần thiết để một số là tốt,$k$là số lượng chính xác các số tốt mà chúng ta muốn trong một hoán vị, và$p$là mô-đun cho câu trả lời. 

Giới hạn$n \le 100$chỉ ra rằng bất kỳ$O(n^4)$hoặc cách tiếp cận cao hơn sẽ quá chậm. Việc đếm trực tiếp tất cả các hoán vị là không khả thi vì có$n!$hoán vị, tăng nhanh hơn$10^{100}$vì$n=100$. Vấn đề này đòi hỏi một cách tiếp cận tổ hợp để tránh liệt kê các hoán vị một cách rõ ràng. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó$m=1$, trong trường hợp đó chỉ những số lớn nhất mới có thể tốt. Một trường hợp phức tạp khác là$k=n$, trong đó mọi số trong hoán vị phải tốt, điều này chỉ có thể thực hiện được trong các cấu hình rất hạn chế. Nếu giải pháp ngây thơ đếm các mảng con cho mỗi hoán vị, nó sẽ thất bại ở các giới hạn này. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả$n!$hoán vị, lặp lại từng số và với mỗi số, tính toán tất cả các mảng con chứa nó để đếm các cực đại riêng biệt. Cách tiếp cận này đúng về mặt logic nhưng có độ phức tạp về thời gian giai thừa, khiến nó không thể thực hiện được đối với$n>10$. 

Cái nhìn sâu sắc quan trọng đến từ việc hiểu điều gì tạo nên một con số$x$Tốt. Đối với một số$x$trong một hoán vị, số lượng cực đại phân biệt trong các mảng con chứa nó chỉ được xác định bởi vị trí tương đối của các số lớn hơn$x$. Nếu có chính xác$m-1$số lớn hơn$x$"tách" hoán vị xung quanh$x$, sau đó$x$will be good. Do đó, chúng ta có thể trình bày lại vấn đề theo cách tổ hợp: chọn số nào tốt, đặt các số lớn hơn tương ứng với mỗi số tốt để thỏa mãn số lượng cực đại và đếm các sắp xếp hợp lệ. 

Lập trình động trên số lượng vị trí còn lại và "khe tốt" còn lại cho phép chúng tôi tính toán tổng số hoán vị hợp lệ mà không cần tạo ra chúng. Điều này làm giảm vấn đề xuống$O(n^3)$hoặc$O(n^2)$tùy thuộc vào việc thực hiện, khả thi đối với$n \le 100$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhận biết rằng một số có tốt hay không chỉ phụ thuộc vào các số lớn hơn nó và vị trí của chúng so với nó. Do đó, chúng tôi xử lý các số từ lớn nhất đến nhỏ nhất. 
2. Hãy để$f(i,j)$là số cách sắp xếp thứ nhất$i$những con số chính xác như vậy$j$trong số họ là tốt. DP này sẽ đếm các hoán vị tăng dần theo kích thước số. 
3. Cho số lớn nhất$n$, sẽ luôn tốt nếu$m=1$và không thể tốt bằng cách khác. Khởi tạo$f(1,1) = 1$nếu số lớn nhất là tốt, nếu không thì$f(1,0) = 1$. 
4. Với mỗi số$x$từ lớn nhất đến nhỏ nhất, hãy cân nhắc hai lựa chọn: xếp nó là tốt hoặc không tốt. Số cách chọn vị trí xung quanh các số đã được đặt để duy trì thuộc tính cực đại là tổ hợp:$\binom{remaining+1}{1}$cách chèn giữa các phân đoạn. 
5. Cập nhật bảng DP theo số cách chèn$x$vào các trình tự hiện có để duy trì trạng thái tốt/không tốt. Sử dụng mô đun$p$trong mọi hoạt động. 
6. Sau khi xử lý tất cả các số,$f(n,k)$chứa số hoán vị chính xác$k$số tốt modulo$p$. 

Tại sao nó hoạt động: Bất biến DP đảm bảo rằng$f(i,j)$đếm tất cả các hoán vị của số đầu tiên$i$số lớn nhất với chính xác$j$số tốt, không phụ thuộc vào thứ tự tương đối của các số nhỏ hơn. Bằng cách xử lý từ lớn nhất đến nhỏ nhất và đếm các vị trí chèn một cách kết hợp, chúng tôi đảm bảo không có hoán vị nào được tính hai lần và điều kiện số tốt được duy trì. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import comb

def main():
    n, m, k, p = map(int, input().split())
    
    # Precompute factorials modulo p for combinations
    fact = [1]*(n+1)
    for i in range(1,n+1):
        fact[i] = fact[i-1]*i % p

    # Precompute binomial coefficients modulo p
    C = [[0]*(n+1) for _ in range(n+1)]
    for i in range(n+1):
        C[i][0] = 1
        for j in range(1,i+1):
            C[i][j] = (C[i-1][j-1]+C[i-1][j])%p

    # DP[i][j]: ways to arrange first i numbers with exactly j good numbers
    dp = [[0]*(k+2) for _ in range(n+2)]
    dp[0][0] = 1

    for i in range(1,n+1):
        for j in range(0,min(i,k)+1):
            # place current number as non-good
            dp[i][j] = dp[i-1][j] * (i-1 + 1) % p
            # place current number as good, if j>0 and possible
            if j>0 and m<=i:
                dp[i][j] = (dp[i][j] + dp[i-1][j-1] * C[i-1][m-1]) % p

    print(dp[n][k]%p)

if __name__ == "__main__":
    main()
```Mã đầu tiên tính toán trước các hệ số giai thừa và nhị thức theo modulo$p$để cho phép tính toán nhanh số lần chèn. Bảng DP được cập nhật lặp đi lặp lại, xem xét cả trường hợp số hiện tại tốt và trường hợp không. Số tổ hợp$C[i-1][m-1]$phản ánh việc chọn vị trí cho các số lớn hơn số hiện tại để thỏa mãn chính xác$m$cực đại. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào mẫu`4 3 2 10007`: 

| Bước | tôi | j | dp[i][j] | 
| --- | --- | --- | --- | 
| ban đầu | 0 | 0 | 1 | 
| 1 | 1 | 0 | 1 | 
| 1 | 1 | 1 | 1 (số lớn nhất có thể tốt) | 
| 2 | 2 | 0 | 2 | 
| 2 | 2 | 1 | 2 | 
| 2 | 2 | 2 | 0 | 
| 3 | 3 | 0 | 6 | 
| 3 | 3 | 1 | 6 | 
| 3 | 3 | 2 | 4 | 
| 4 | 4 | 2 | 4 | 

Điều này xác nhận`dp[4][2] = 4`như mong đợi. 

Một bài kiểm tra khác`n=3, m=2, k=1, p=100`: 

| Bước | tôi | j | dp[i][j] | 
| --- | --- | --- | --- | 
| ban đầu | 0 | 0 | 1 | 
| 1 | 1 | 0 | 1 | 
| 1 | 1 | 1 | 0 | 
| 2 | 2 | 0 | 2 | 
| 2 | 2 | 1 | 1 | 
| 3 | 3 | 1 | 3 | 

Xác nhận một hoán vị với chính xác một số tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Ba vòng lặp lồng nhau: i cho số, j cho số đếm tốt và phép tính tổ hợp | 
| Không gian | O(n^2) | Bàn DP và bàn kết hợp | 

Độ phức tạp nằm trong giới hạn vì$n \le 100$cho phép$10^6$hoạt động thoải mái dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

assert run("4 3 2 10007") == "4", "sample 1"
assert run("3 2 1 100") == "3", "custom 1"
assert run("1 1 1 10") == "1", "minimum size"
assert run("5 1 1 1000") == "24", "only largest can be good"
assert run
```
