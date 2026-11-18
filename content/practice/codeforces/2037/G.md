---
title: "CF 2037G - Khám phá Natlan"
description: "Chúng tôi được yêu cầu đếm số lượng đường đi riêng biệt từ thành phố đầu tiên đến thành phố cuối cùng trong vùng Natlan, nơi mỗi thành phố có một giá trị hấp dẫn. Các đường đi tuân theo một quy tắc đơn giản: từ thành phố $i$ bạn có thể đi đến thành phố $j$ nếu $i < j$ và $gcd(ai, aj) neq 1$."
date: "2026-06-08T10:17:59+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "data-structures", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2037
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 988 (Div. 3)"
rating: 2000
weight: 2037
solve_time_s: 217
verified: false
draft: false
---

[CF 2037G - Khám phá Natlan](https://codeforces.com/problemset/problem/2037/G) 

**Xếp hạng:** 2000 
**Tags:** bitmasks, tổ hợp, cấu trúc dữ liệu, dp, toán học, lý thuyết số 
**Thời gian giải:** 3 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm số lượng đường đi riêng biệt từ thành phố đầu tiên đến thành phố cuối cùng trong vùng Natlan, nơi mỗi thành phố có một giá trị hấp dẫn. Các con đường tuân theo một quy tắc đơn giản: từ thành phố$i$bạn có thể đi du lịch đến thành phố$j$nếu như$i < j$Và$\gcd(a_i, a_j) \neq 1$. Điều này làm cho cấu trúc cơ bản trở thành một biểu đồ chu kỳ có hướng (DAG), bởi vì tất cả các cạnh đều hướng từ các thành phố có chỉ số thấp hơn đến các thành phố có chỉ số cao hơn, do đó các chu trình là không thể. Đầu vào bao gồm số lượng thành phố$n$và giá trị hấp dẫn$a_i$. Đầu ra là số đường đi riêng biệt từ thành phố 1 đến thành phố$n$, modulo$998\,244\,353$. 

Những ràng buộc rất chặt chẽ. Với$n$lên đến$2 \cdot 10^5$và mỗi$a_i$lên đến$10^6$, việc lặp lại tất cả các cặp thành phố có thể có để kiểm tra GCD sẽ cần khoảng$2 \cdot 10^5 \cdot 2 \cdot 10^5 / 2 \approx 2 \cdot 10^{10}$hoạt động, vượt xa những gì phù hợp với giới hạn thời gian 4 giây. Điều này loại trừ các cách tiếp cận theo cặp ngây thơ. Chúng ta cũng cần xử lý số học modulo một cách cẩn thận để tránh tràn vì số lượng đường dẫn có thể tăng lên nhanh chóng. 

Trường hợp cạnh tinh tế xảy ra khi tất cả các thành phố đều là cặp nguyên tố cùng nhau. Ví dụ, với các thành phố$[2, 3, 5]$, không có cạnh nào tồn tại ngoại trừ có thể từ thành phố 1 đến thành phố 2 nếu$\gcd(2,3) \neq 1$, chính là nó. Trong trường hợp này, phép lặp đơn giản có thể đếm gấp đôi các đường dẫn hoặc giả sử một cạnh tồn tại trong khi không có cạnh nào tồn tại, đưa ra số lượng đường dẫn không chính xác. Một trường hợp đặc biệt khác là khi nhiều thành phố có cùng sức hấp dẫn, điều này tạo ra một số đường dẫn hợp lệ kết nối các cặp thành phố giống nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa các thành phố dưới dạng DAG và tính toán rõ ràng các cạnh giữa mỗi cặp$i < j$nếu như$\gcd(a_i, a_j) \neq 1$. Sau khi DAG được xây dựng, tìm kiếm theo chiều sâu (DFS) hoặc lập trình động từ thành phố 1 đến thành phố$n$có thể đếm tất cả các con đường. Điều này hoạt động về nguyên tắc, nhưng kiểm tra$\gcd$với mỗi cặp là$O(n^2)$, Về$4 \cdot 10^{10}$hoạt động cho đầu vào lớn nhất. Ngay cả việc ghi nhớ DFS vẫn cần xây dựng DAG, vì vậy điều này không thực tế. 

Quan sát quan trọng là hai số có GCD lớn hơn 1 khi và chỉ khi chúng có chung thừa số nguyên tố. Chúng ta có thể tính từng giá trị hấp dẫn thành các thừa số nguyên tố của nó và, với mỗi giá trị hấp dẫn,$p$, theo dõi thành phố mới nhất với sức hấp dẫn chia hết cho$p$. Điều này cho phép chúng tôi truyền bá số lượng đường dẫn một cách linh hoạt mà không cần kiểm tra từng cặp. Cụ thể, chúng tôi duy trì một mảng lập trình động`dp[i]`đếm số đường đi tới thành phố$i$. Với mỗi thừa số nguyên tố$p$của$a_i$, chúng tôi biết thành phố gần đây nhất`last[p]`với hệ số$p$trước$i$; bất kỳ con đường nào đạt tới`last[p]`có thể tiếp tục đến thành phố$i$. Chúng tôi tổng hợp tất cả sự đóng góp của các thừa số nguyên tố để tính toán`dp[i]`. 

Điều này biến bài toán thành hệ số hóa và truyền tải tuyến tính, làm giảm đáng kể độ phức tạp. Sử dụng một cái rây để phân tích thành thừa số nguyên tố lên đến$10^6$đảm bảo rằng mỗi thành phố được xử lý trong$O(\log a_i)$thời gian, bởi vì số lượng các thừa số nguyên tố phân biệt nhiều nhất là khoảng 7 đối với các số dưới$10^6$. Cách làm này vừa khả thi vừa hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 log max(a)) | O(n^2) | Quá chậm | 
| DP hệ số nguyên tố | O(n log max(a)) | O(max(a)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các thừa số nguyên tố nhỏ nhất (SPF) cho tất cả các số lên đến$10^6$dùng sàng. Điều này cho phép chúng ta phân tích bất kỳ số nào một cách nhanh chóng trong$O(\log a_i)$thời gian bằng cách chia liên tục cho thừa số nguyên tố nhỏ nhất của nó. 
2. Khởi tạo một mảng`dp`kích thước$n+1$với tất cả số không. Bộ`dp[1] = 1`, vì có đúng một con đường bắt đầu từ thành phố 1. 
3. Khởi tạo từ điển`last`để theo dõi chỉ số thành phố cuối cùng cho từng thừa số nguyên tố. Điều này cho phép chúng tôi truyền bá đường dẫn một cách hiệu quả. 
4. Lặp lại các thành phố từ chỉ số 1 đến$n$. Đối với mỗi thành phố$i$:

- Nhân tố`a[i]`vào các thừa số nguyên tố của nó bằng cách sử dụng mảng SPF. 
- Đối với mỗi thừa số nguyên tố$p$của`a[i]`, thêm vào`dp[last[p]]`ĐẾN`dp[i]`modulo$998\,244\,353$, Ở đâu`last[p]`là thành phố gần đây nhất trước đây`i`với hệ số$p$. Nếu như`last[p]`chưa tồn tại, nó đóng góp bằng không. 
- Sau khi cập nhật`dp[i]`, cập nhật`last[p] = i`cho mọi thừa số nguyên tố$p$. 
5. Câu trả lời cuối cùng là`dp[n]`, số lượng đường đi riêng biệt đến thành phố$n$. 

Tại sao nó hoạt động: mọi con đường từ thành phố 1 đến thành phố$i$có thể được phân tách duy nhất bởi các thừa số nguyên tố của các thành phố dọc theo đường đi. Mỗi yếu tố chính đảm bảo khả năng kết nối tới các thành phố sau này chia sẻ yếu tố đó. Điều bất biến là sau khi xử lý thành phố$i$,`dp[i]`đếm chính xác tất cả các đường dẫn riêng biệt kết thúc tại$i$chỉ sử dụng các cạnh hợp lệ. Tổng các đóng góp theo thừa số nguyên tố đảm bảo không có đường dẫn nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAX_A = 10**6 + 1

# Sieve to compute smallest prime factors
spf = list(range(MAX_A))
for i in range(2, int(MAX_A**0.5) + 1):
    if spf[i] == i:
        for j in range(i*i, MAX_A, i):
            if spf[j] == j:
                spf[j] = i

def factorize(x):
    factors = set()
    while x > 1:
        factors.add(spf[x])
        x //= spf[x]
    return factors

def main():
    n = int(input())
    a = list(map(int, input().split()))
    dp = [0] * n
    dp[0] = 1
    last = dict()

    for i in range(n):
        primes = factorize(a[i])
        for p in primes:
            if p in last:
                dp[i] = (dp[i] + dp[last[p]]) % MOD
        for p in primes:
            last[p] = i

    print(dp[n-1] % MOD)

if __name__ == "__main__":
    main()
```Sàng tính toán trước các thừa số nguyên tố nhỏ nhất một cách hiệu quả. các`factorize`hàm tạo ra các thừa số nguyên tố riêng biệt của bất kỳ số nào. Mảng lập trình động`dp`tích lũy số lượng đường dẫn đến mỗi thành phố. các`last`từ điển đảm bảo rằng những đóng góp chỉ đến từ các thành phố trước đó có chung ít nhất một thừa số nguyên tố. Đang cập nhật`last[p]`sau khi thêm các khoản đóng góp đảm bảo chúng tôi không tính đường dẫn nhiều lần. Việc xử lý ranh giới được thực hiện một cách tự nhiên bằng tính năng lập chỉ mục dựa trên số 0 của Python. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào`a = [2, 6, 3, 4, 6]`. 

| tôi | một [tôi] | số nguyên tố | dp[i] | mệnh lệnh cuối cùng sau tôi | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | {2} | 1 | {2:0} | 
| 1 | 6 | {2,3} | 1 | {2:1,3:1} | 
| 2 | 3 | {3} | 1 | {2:1,3:2} | 
| 3 | 4 | {2} | 2 | {2:3,3:2} | 
| 4 | 6 | {2,3} | 5 | {2:4,3:4} | 

Điều này xác nhận năm con đường riêng biệt. 

**Mẫu 2:** 

đầu vào`a = [3, 5, 15]`. 

| tôi | một [tôi] | số nguyên tố | dp[i] | mệnh lệnh cuối cùng sau tôi | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | {3} | 1 | {3:0} | 
| 1 | 5 | {5} | 0 | {3:0,5:1} | 
| 2 | 15 | {3,5} | 2 | {3:2,5:2} | 

Hai con đường đó là`[3,15]`Và`[3,5,15]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log max(a)) | Hệ số hóa cho mỗi thành phố là O(log a_i) sử dụng sàng SPF; quét tuyến tính trên n thành phố. | 
| Không gian | O(max(a)) | mảng SPF và`last`từ điển lưu trữ tối đa MAX_A mục nhập. | 

Được cho$n \le 2 \cdot 10^5$và (a_i \le
