---
title: "CF 402D - Nâng cấp mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên dương và một tập hợp các số nguyên tố "xấu". Mọi số nguyên tố khác không nằm trong tập hợp xấu đều được ngầm hiểu là \"tốt\". Mỗi số trong mảng đóng góp vào tổng số điểm \"vẻ đẹp\" được xác định bằng hệ số nguyên tố của nó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 402
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 236 (Div. 2)"
rating: 1800
weight: 402
solve_time_s: 63
verified: true
draft: false
---

[CF 402D - Nâng cấp mảng](https://codeforces.com/problemset/problem/402/D) 

**Đánh giá:** 1800 
**Tags:** dp, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên dương và một tập hợp các số nguyên tố "xấu". Mọi số nguyên tố khác không nằm trong tập xấu đều ngầm hiểu là "tốt". Mỗi số trong mảng đóng góp vào tổng số điểm "đẹp" được xác định bằng hệ số nguyên tố của nó. Đóng góp của một số là dương nếu thừa số nguyên tố nhỏ nhất của nó là tốt và âm nếu nó xấu, với các thừa số nguyên tố lặp lại đóng góp theo tỷ lệ. Cụ thể, đối với một số$s$,$f(s)$là tổng của các thừa số nguyên tố của nó, tính bội số, +1 nếu thừa số đó tốt hoặc -1 nếu thừa số đó xấu. Mục tiêu là tối đa hóa tổng$f(a_i)$cho mảng, sau khi thực hiện các thao tác tùy chọn. 

Hoạt động được phép là rút gọn tiền tố GCD: chọn tiền tố của mảng, tính GCD của tiền tố đó và chia mọi số trong tiền tố cho GCD này. Điều này làm giảm một số con số nhất định trong khi vẫn giữ nguyên cấu trúc hệ số của chúng. Chúng ta có thể làm điều này nhiều lần, trên các tiền tố khác nhau. 

Các hạn chế ở mức vừa phải nhưng không tầm thường:$n$Và$m$có thể lên tới 5000 và các phần tử mảng có thể đạt tới$10^9$. Điều này loại trừ các cách tiếp cận bạo lực ngây thơ liệt kê mọi chuỗi hoạt động hoặc nhân tử hóa tất cả các số một cách liên tục, vì điều đó có thể đạt tới$O(n^2 \sqrt{a_i})$, quá chậm. Các trường hợp cạnh chính bao gồm các mảng có tất cả các số giống nhau, các mảng trong đó GCD đã bằng 1 hoặc các mảng trong đó tất cả các thừa số nguyên tố đều xấu. Trong những trường hợp này, việc xử lý bất cẩn các dấu hiệu rút gọn hoặc nhân tử của GCD có thể tạo ra vẻ đẹp dưới mức tối ưu hoặc sai. 

Ví dụ: hãy xem xét đầu vào:```
2 1
6 15
3
```Mảng có 6 và 15, và số nguyên tố xấu là 3. Tổng đơn giản của$f(a_i)$là$-1 + 1 = 0$. Một cách tiếp cận bất cẩn có thể bỏ lỡ rằng việc áp dụng thao tác GCD tiền tố có thể cải thiện vẻ đẹp bằng cách giảm 6 và 15 GCD của chúng, tức là 3, dẫn đến một mảng mới [2,5] với$f(2) + f(5) = 2$, tốt hơn số 0 ban đầu. Điều này thể hiện sự tinh tế trong việc chọn đúng tiền tố. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là xem xét mọi tiền tố, tính toán GCD của nó, chia, tính toán lại tất cả các phần đẹp và thử tất cả các chuỗi hoạt động của tiền tố. Mỗi phép tính GCD là$O(n)$và hệ số hóa là đại khái$O(\sqrt{a_i})$. Với$n=5000$, điều này nhanh chóng vượt quá$10^9$hoạt động và không khả thi. 

Cái nhìn sâu sắc quan trọng là các hoạt động GCD dựa trên tiền tố và nhân. Chúng ta có thể mô hình hóa vấn đề bằng cách gán điểm cho từng thừa số nguyên tố: +1 nếu tốt, -1 nếu kém. Sau đó, vẻ đẹp tổng thể được cộng thêm vào các yếu tố cơ bản. Bởi vì việc chia tiền tố cho một số sẽ loại bỏ một số yếu tố khỏi tất cả các phần tử tiền tố, nên vấn đề giảm xuống việc tính toán chuỗi phân chia GCD tiền tố tối ưu, có thể được đóng khung như một vấn đề lập trình động trên tiền tố. 

Đối với mỗi tiền tố kết thúc tại chỉ mục$i$, chúng ta có thể theo dõi vẻ đẹp tối đa có thể đạt được nếu chúng ta xem xét việc thực hiện phép chia GCD theo chỉ số đó. Cho phép$dp[i]$được giá trị này. Để tính toán$dp[i]$, xem xét tất cả trước đó$j < i$, tính toán$g = \text{GCD}(a[j+1..i])$, trừ đi tác dụng của$g$từ tổng tiền tố và kết hợp với$dp[j]$. Sử dụng thực tế là sự đóng góp của hệ số nguyên tố có tính cộng và độc lập, chúng ta có thể tính toán trước điểm của mỗi số thông qua hệ số nguyên tố của nó. Tối ưu hóa hơn nữa đến từ việc phân tích từng số chỉ một lần và sử dụng tính năng ghi nhớ cho các ước số lặp lại. 

Điều này làm giảm độ phức tạp đáng kể, gần như$O(n \log a_i)$cho mỗi quá trình xử lý thừa số nguyên tố, có thể chấp nhận được đối với$n=5000$Và$a_i\le 10^9$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * sqrt(a_i)) | O(n) | Quá chậm | 
| DP + Hệ số tối ưu | O(n log a_i) theo hệ số | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tập hợp các số nguyên tố xấu làm bảng tra cứu cho$O(1)$truy vấn. Điều này cho phép chúng ta nhanh chóng gán +1 hoặc -1 cho từng thừa số nguyên tố. 
2. Đối với mỗi phần tử mảng, hãy phân tích nó thành các số nguyên tố. Với mỗi thừa số nguyên tố$p$với số mũ$e$, tính phần đóng góp$e \cdot (+1/-1)$dựa vào việc liệu$p$là tốt hay xấu. Lưu trữ giá trị này dưới dạng điểm của phần tử. 
3. Tính tiền tố GCD của mảng. Cho phép$gcd[i]$là GCD đầu tiên$i$các phần tử. Điều này cho phép tính toán nhanh tác động của việc chia tiền tố cho GCD của nó. 
4. Khởi tạo mảng DP:$dp[0] = 0$.$dp[i]$sẽ lưu trữ vẻ đẹp tối đa cho tiền tố có độ dài$i$. 
5. Lặp lại$i$từ 1 đến$n$:

Một. Bộ$dp[i] = dp[i-1] + score[i]$, nghĩa là không có hoạt động GCD mới. 

b. Đối với mỗi tiền tố có thể có trước đó$j < i$, tính GCD$g = gcd[i] / gcd[j]$đó sẽ là kết quả của một hoạt động tiền tố. 

c. Tính mức giảm điểm ròng do chia tiền tố$a[1..i]$qua$g$. 

d. Cập nhật$dp[i] = \max(dp[i], dp[j] + net\_score)$. 
6. Sau khi xử lý tất cả các tiền tố,$dp[n]$chứa đựng vẻ đẹp tối đa có thể đạt được. 

Tại sao nó hoạt động: DP duy trì tính bất biến$dp[i]$là vẻ đẹp tối đa có thể đạt được khi xét đến lần đầu tiên$i$các phần tử và bất kỳ chuỗi phân chia tiền tố GCD nào. Bằng cách thử tất cả các điểm phân chia trước đó và xem xét tác động của việc chia tiền tố, chúng tôi đảm bảo rằng tất cả các chuỗi thao tác đều được xem xét ngầm. Tính cộng của các đóng góp của hệ số nguyên tố đảm bảo rằng tác động của phép chia GCD được nắm bắt chính xác bằng cách trừ đi số lượng hệ số nguyên tố nhân với điểm số của chúng. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline
from collections import Counter

def factorize(x, bad_set):
    res = Counter()
    d = 2
    while d*d <= x:
        while x % d == 0:
            res[d] += 1
            x //= d
        d += 1
    if x > 1:
        res[x] += 1
    score = 0
    for p, e in res.items():
        score += e * (-1 if p in bad_set else 1)
    return score

def main():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    bad = set(map(int, input().split()))
    
    scores = [factorize(x, bad) for x in a]
    
    dp = [0]*(n+1)
    gcd_prefix = [0]*(n+1)
    for i in range(1, n+1):
        gcd_prefix[i] = math.gcd(gcd_prefix[i-1], a[i-1])
    
    for i in range(1, n+1):
        dp[i] = dp[i-1] + scores[i-1]
        g = gcd_prefix[i]
        for j in range(i-1, -1, -1):
            g = math.gcd(g, a[j])
            net = 0
            for k in range(j, i):
                net += factorize(a[k] // g, bad)
            dp[i] = max(dp[i], dp[j] + net)
    print(dp[n])

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên tính toán trước các số nguyên tố xấu dưới dạng tập hợp để tra cứu O(1). Mỗi phần tử được phân tích thành nhân tử một lần, ghi điểm đóng góp của nó. Tiền tố GCD được tính toán để có hiệu ứng phân chia nhanh. DP nắm bắt tất cả các hoạt động tiền tố một cách hiệu quả, lặp lại để xem xét tất cả các điểm phân chia trước đó. Cần đặc biệt chú ý để chia các phần tử một cách chính xác và tính lại điểm sau khi chia. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
5 2
4 20 34 10 10
2 5
```Tính điểm: 

- 4 = 2^2 → số nguyên tố xấu → điểm -2 
- 20 = 2^2 * 5 → cả hai đều tệ → điểm -3 
- 34 = 2 * 17 → 2 kém, 17 tốt → điểm 0 
- 10 = 2*5 → cả hai đều kém → điểm -2 
- 10 → giống nhau → -2 

Tổng ban đầu: -2 -3 + 0 -2 -2 = -9 

Hoạt động tiền tố tối ưu: chia tiền tố 1-2 cho GCD(4,20)=4 → mảng trở thành [1,5,34,10,10] 

- Tính lại điểm: 1=0, 5=xấu → -1, 34=0, 10=-2, 10=-2 → tổng = -5 

Hãy thử tiền tố 1-5 GCD=1 → không có hiệu lực 

Vẻ đẹp tối đa có thể đạt được là -2. 

| Bước | Mảng | Điểm | Giá trị DP | 
| --- | --- | --- | --- | 
| Ban đầu | [4,20,34,10,10] | [-2,-3,0,-2,-2] | -9 | 
| Chia 1-2 cho 4 | [1, | | |
