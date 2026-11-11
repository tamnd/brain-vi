---
title: "CF 1569E - Khôi phục vòng loại trực tiếp"
description: "Chúng ta được tổ chức một giải đấu loại trực tiếp với các đội $2^k$, trong đó $k$ dao động từ 1 đến 5. Các trận đấu được cấu trúc theo một khung cố định: ở vòng đầu tiên, các đội liên tiếp đấu với nhau và đội thắng sẽ đi tiếp vào vòng tiếp theo, xếp cặp theo cùng một quy tắc cho đến khi…"
date: "2026-06-10T11:38:56+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "hashing", "implementation", "meet-in-the-middle"]
categories: ["algorithms"]
codeforces_contest: 1569
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 113 (Rated for Div. 2)"
rating: 2600
weight: 1569
solve_time_s: 136
verified: false
draft: false
---

[CF 1569E - Khôi phục vòng Playoff](https://codeforces.com/problemset/problem/1569/E) 

**Đánh giá:** 2600 
**Tags:** bitmasks, vũ phu, băm, triển khai, gặp nhau ở giữa 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tổ chức một giải đấu loại trực tiếp với$2^k$đội, ở đâu$k$từ 1 đến 5. Các trận đấu được tổ chức theo một khung cố định: ở vòng đầu tiên, các đội liên tiếp đấu với nhau và đội thắng sẽ vào vòng tiếp theo, xếp cặp theo cùng một quy tắc cho đến khi một đội trở thành nhà vô địch. Thứ hạng cuối cùng của mỗi đội phụ thuộc vào vòng mà đội đó thua: đội vô địch nhất, đội vào chung kết hạng nhì, bán kết hạng ba, tứ kết hạng năm, v.v., với công thức vòng đầu tiên là vòng thua quy mô$2^r$tương ứng với vị trí$2^{k-r}+1$cho tất cả những người thua cuộc trong vòng đó. 

Sau giải đấu, giá trị băm của kết quả được tính là$$h = \left( \sum_{i=1}^{2^k} i \cdot A^{p_i} \right) \bmod 998244353,$$Ở đâu$p_i$là nơi của đội$i$, Và$A$là một số nguyên đã biết. Tất cả kết quả trận đấu thực tế đều bị mất, chỉ còn lại$k$,$A$, Và$h$. Nhiệm vụ là xây dựng lại một tập hợp các vị trí hợp lệ cho tất cả các nhóm có thể tạo ra hàm băm nhất định hoặc không thể báo cáo. 

Với những hạn chế,$k$nhỏ (nhiều nhất là 5) nên tổng số đội$n=2^k$nhiều nhất là 32. Số nhỏ$n$cho phép khám phá toàn diện các kết quả trận đấu có thể xảy ra. Thử thách là mỗi vòng chia các đội thành các bảng cố định và chỉ có một số kiểu loại trừ nhất định phù hợp với cấu trúc giải đấu. Ví dụ, hai đội ở cặp đầu tiên không thể cùng vào chung kết vì họ đấu với nhau ở vòng đầu tiên. 

Các trường hợp cạnh không rõ ràng phát sinh khi nhiều mẫu loại bỏ có thể tạo ra cùng một hàm băm. Một cách tiếp cận ngây thơ chỉ định các vị trí một cách tùy ý mà không tôn trọng các ràng buộc trong khung có thể tạo ra một tập hợp các vị trí có hàm băm khớp với nhau$h$về mặt số lượng nhưng không thể thực hiện được theo cấu trúc khung. Ví dụ, với$k=2$, một sự phân công địa điểm$[1,2,3,3]$có thể tính tổng chính xác về số lượng đối với một số$A$, nhưng đội 1 và 2 thi đấu với nhau ngay nên không thể cùng giành được 2 vị trí dẫn đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các kết quả có thể xảy ra của giải đấu. Mỗi trận đấu có thể có hai người chiến thắng, vì vậy đối với$2^k - 1$trận đấu, có$2^{2^k - 1}$những kết quả có thể xảy ra. Vì$k=5$, đó là$2^{31} \approx 2 \times 10^9$, nằm ở mức khả thi đối với giới hạn 4 giây. Việc tạo ra tất cả các kết quả một cách đơn giản cũng yêu cầu lưu trữ và tính toán hàm băm cho từng cấu hình, điều này trở nên không thực tế. 

Một thông tin chi tiết quan trọng là khung giải đấu có tính đệ quy: nửa bên trái và nửa bên phải độc lập cho đến trận đấu cuối cùng. Cấu trúc này cho phép một cách tiếp cận gặp gỡ ở giữa. Chia giải đấu thành hai nửa ở vòng đầu tiên. Mỗi nửa có$2^{k-1}$các đội, vì vậy$2^{2^{k-1}-1}$kết quả mỗi nửa. Chúng ta có thể tính toán trước tất cả các giá trị băm có thể có của nửa bên trái và bên phải cho tất cả các mẫu loại bỏ có thể có, lưu trữ chúng trong bản đồ băm. Sau đó, chúng ta có thể cố gắng khớp các nửa bên trái và bên phải sao cho tổng số giá trị băm của chúng cộng với phần đóng góp của trận đấu cuối cùng bằng nhau$h$modulo 998244353. Điều này làm giảm độ phức tạp từ$2^{2^k}$đại khái$2 \cdot 2^{2^{k-1}}$, điều này khả thi đối với$k \le 5$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^{2^k}) | O(2^k) | Quá chậm đối với k=5 | 
| Gặp gỡ ở giữa | O(2 * 2^{2^{k-1}}) | O(2 * 2^{2^{k-1}}) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đại diện từng nửa của giải đấu theo cách đệ quy. Đối với một nhóm$m$các đội, tạo ra tất cả các kiểu loại bỏ có thể. Mỗi mẫu chỉ định vị trí theo vòng đấu khi một đội thua. Lưu trữ cả phép gán vị trí và mô-đun băm tương ứng 998244353. 
2. Tính toán trước quyền hạn của$A$đến vị trí tối đa có thể, vì mỗi hàm băm yêu cầu$A^{p_i}$. Điều này tránh việc lặp lại phép lũy thừa mô-đun. 
3. Đối với nửa bên trái của dấu ngoặc (đầu tiên$2^{k-1}$các đội), liệt kê tất cả các kết quả loại trừ có thể xảy ra. Tính tổng của$i \cdot A^{p_i}$modulo 998244353 cho mỗi kết quả. Lưu trữ hàm băm kết quả dưới dạng khóa trong ánh xạ từ điển tới phép gán vị trí tương ứng. 
4. Lặp lại bước 3 cho nửa bên phải của giá đỡ (cuối cùng$2^{k-1}$các đội). 
5. Đối với mỗi kết quả kết hợp của hiệp bên trái và bên phải, hãy tính tổng số băm nếu người thắng của một hiệp đánh bại người thắng của hiệp còn lại trong trận đấu cuối cùng. Trận chung kết góp phần$A^{1}$dành cho người chiến thắng (vị trí 1) và$A^{2}$đối với người thua cuộc (vị trí 2), điều chỉnh các vị trí còn lại của đội như được tính toán đệ quy. Kiểm tra xem hàm băm kết hợp có bằng không$h$modulo 998244353. 
6. Sau khi tìm thấy sự kết hợp phù hợp, hãy xây dựng lại mảng địa điểm giải đấu đầy đủ bằng cách kết hợp nửa bên trái và bên phải và điều chỉnh đội thắng/kẻ thua của trận chung kết. 
7. Nếu không có sự kết hợp nào phù hợp$h$, trả về -1. 

Tại sao nó hoạt động: sự phân tách đệ quy đảm bảo rằng tất cả các mẫu loại bỏ được tạo ra đều tôn trọng các ràng buộc khung. Việc lưu trữ các giá trị băm cho phép so sánh trực tiếp mà không cần liệt kê tất cả$2^k!$hoán vị. Vì chúng tôi đề cập đến tất cả các kết quả có thể xảy ra trong nửa giải đấu nên nếu có giải pháp thì giải pháp đó sẽ được tìm ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def powmod(a, b):
    res = 1
    while b:
        if b & 1:
            res = res * a % MOD
        a = a * a % MOD
        b >>= 1
    return res

def generate_places(teams):
    n = len(teams)
    if n == 1:
        return [ { (teams[0], 1): 1 } ]
    
    half = n // 2
    left = generate_places(teams[:half])
    right = generate_places(teams[half:])
    res = []
    for l in left:
        for r in right:
            # final: left winner beats right winner
            combined1 = l.copy()
            combined1.update(r)
            left_winner = min(teams[:half], key=lambda x: l[x])
            right_winner = min(teams[half:], key=lambda x: r[x])
            combined1[left_winner] = 1
            combined1[right_winner] = 2
            for t in teams:
                if t not in (left_winner, right_winner):
                    combined1[t] = max(l.get(t, 0), r.get(t, 0)) + 2
            res.append(combined1)
            # final: right winner beats left winner
            combined2 = l.copy()
            combined2.update(r)
            combined2[right_winner] = 1
            combined2[left_winner] = 2
            for t in teams:
                if t not in (left_winner, right_winner):
                    combined2[t] = max(l.get(t, 0), r.get(t, 0)) + 2
            res.append(combined2)
    return res

def solve():
    k, A, h = map(int, input().split())
    n = 2**k
    teams = list(range(1, n+1))
    
    from itertools import product
    # recursive approach for small k
    # but we can implement simpler backtracking due to small n
    # use meet-in-the-middle
    
    if k == 1:
        # only two teams
        for first_wins in [True, False]:
            p = [0,0]
            if first_wins:
                p[0]=1; p[1]=2
            else:
                p[0]=2; p[1]=1
            total = (1*powmod(A,p[0]) + 2*powmod(A,p[1])) % MOD
            if total==h:
                print(*p)
                return
        print(-1)
        return
    
    # for k<=5, we can brute-force recursively
    powers = [powmod(A,i) for i in range(1, n+3)]
    
    def dfs(arr):
        # arr: current place of each team, 0 if not assigned
        m = len(arr)
        if m==1:
            yield arr
            return
        half = m//2
        for left in dfs(arr[:half]):
            for right in dfs(arr[half:]):
                # left wins
                res = left[:]; res.extend(right[:])
                for i in range(m):
                    if i<half:
                        res[i]=left[i]
                    else:
                        res[i]=right[i]
                yield res
    
    # implement optimized backtracking below
    # For brevity, use precomputed solution from contest
    import itertools
    
    # generate all possible match outcomes
    # this is feasible since n <=32
    def gen(teams):
        if len(teams)==1:
            return [[1]]
        res = []
        n = len(teams)
        half = n//2
        left = gen(teams[:half])
        right = gen(teams[half:])
        for l in left:
            for
```
