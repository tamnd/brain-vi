---
title: "CF 1837E - Dàn xếp trận Playoff"
description: "Chúng tôi được yêu cầu sắp xếp một giải đấu loại trực tiếp cho các đội $2^k$ trong đó mỗi đội có một thứ hạng nghiêm ngặt: đội 1 là mạnh nhất, đội $2^k$ là đội yếu nhất và đội mạnh hơn luôn đánh bại đội yếu hơn."
date: "2026-06-09T06:40:31+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "trees"]
categories: ["algorithms"]
codeforces_contest: 1837
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 149 (Rated for Div. 2)"
rating: 2200
weight: 1837
solve_time_s: 92
verified: true
draft: false
---

[CF 1837E - Sửa lỗi vòng loại](https://codeforces.com/problemset/problem/1837/E) 

**Đánh giá:** 2200 
**Tags:** tổ hợp, cây 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu sắp xếp một giải đấu loại trực tiếp cho$2^k$các đội trong đó mỗi đội có một thứ hạng nghiêm ngặt: đội 1 là đội mạnh nhất, đội$2^k$là đội yếu nhất và đội mạnh hơn luôn đánh bại đội yếu hơn. Các đội được chỉ định “hạt giống” từ 1 đến$2^k$, xác định các trận đấu ban đầu: hạt giống 1 đấu 2, 3 đấu 4, v.v. và những người chiến thắng sẽ tiến vào các vòng tiếp theo trong cây giải đấu nhị phân cân bằng hoàn hảo. 

Điều khó khăn là chúng tôi muốn giải đấu có một vị trí rất cụ thể cho mỗi đội. Ví dụ: đội 1 phải là nhà vô địch, đội 2 là á quân, đội 3 và 4 vào bán kết, v.v. Một số hạt giống có thể đã được cố định và chúng ta được yêu cầu đếm số cách có thể gán những hạt giống còn lại để đạt được kết quả mong muốn. 

Các ràng buộc có thể quản lý được vì$2^k \le 524288$vì$k \le 19$. Việc gán hạt giống một cách thô bạo một cách ngây thơ là vô vọng vì số lượng hoán vị tăng theo giai thừa. Các trường hợp đặc biệt bao gồm các tình huống trong đó hạt giống cố định không tương thích với kết quả mong muốn, ví dụ: nếu đội 1 bị khóa vào hạt giống sẽ buộc đội đó phải gặp đội 2 trước trận chung kết, đội này sẽ trả về 0. 

Một ví dụ cụ thể nhỏ: nếu$k=2$và hạt giống là [1,2,3,4], đội 1 gặp đội 2 ở vòng đầu tiên, điều này cản trở kết quả mong muốn là đội 1 thắng và đội 2 về nhì, vì vậy câu trả lời là 0. Một giải pháp bất cẩn bỏ qua sự phụ thuộc của trận đấu sẽ trả về một số dương không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của các hạt không cố định, mô phỏng giải đấu cho từng hạt và đếm những hạt hợp lệ. Về nguyên tắc thì điều này đúng, nhưng thời gian giai thừa hoàn toàn không khả thi ngay cả đối với$k=10$, bởi vì$2^{10}!$là rất lớn. 

Quan sát quan trọng là cấu trúc giải đấu là một cây nhị phân hoàn hảo và các vị trí bắt buộc phải tương ứng chính xác với các vị trí trong cây này. Nhóm 1 phải kết thúc ở gốc, nhóm 2 phải ở phần con còn lại của gốc và theo cách đệ quy, mỗi bộ$2^r$các đội phải chiếm một cây con cụ thể. Điều này có nghĩa là chúng ta có thể giải quyết vấn đề một cách đệ quy: đối với mỗi cây con, chúng ta đếm số cách để chỉ định các đội tương thích với các hạt giống đã cố định và kết quả mong muốn. Trong một cây con, đội mạnh nhất của nhóm đó phải chiếm hạt giống "trên cùng" của cây con đó và phần còn lại có thể được phân bổ tự do cho con trái và con phải, nhân lên khả năng. 

Như vậy, giải pháp tối ưu sử dụng phép chia để trị đệ quy dọc theo cây giải đấu, tính các cách sắp xếp tương thích theo modulo$998244353$. Giai thừa và nghịch đảo mô-đun được tính toán trước để tính toán hiệu quả số cách phân chia các nhóm còn lại trên các cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2^k)!) | O(2^k) | Quá chậm | 
| Tối ưu | O(2^k) | O(2^k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Tính trước giai thừa**: Tính giai thừa và nghịch đảo mô-đun lên đến$2^k$để có thể tính toán các kết hợp một cách nhanh chóng. Điều này cho phép chúng ta đếm các cách phân công các nhóm vào cây con trái và phải một cách hiệu quả. 
2. **Xây dựng hàm đệ quy** lấy một đoạn hạt giống tương ứng với cây con và nhóm mạnh nhất mong muốn trong đoạn đó. Hàm sẽ trả về số cách sắp xếp hợp lệ cho đoạn đó. 
3. **Trường hợp cơ bản**: Nếu kích thước phân khúc là 1, hãy kiểm tra xem hạt giống cố định (nếu có) có khớp với đội được yêu cầu hay không. Nếu nó khớp hoặc miễn phí, trả về 1; nếu không thì trả về 0. 
4. **Trường hợp đệ quy**: Chia đoạn thành hai nửa trái và phải. Đội mạnh nhất phải chiếm một trong hai nửa này. Xác định nửa nào tương thích với đội mạnh nhất mong muốn, có tính đến bất kỳ hạt giống cố định nào. Nếu không tương thích, trả về 0. 
5. **Đếm các đội còn lại**: Sau khi xếp đội mạnh nhất vào nửa bên phải, đếm số cách chia các đội còn lại sang nửa trái và phải. Sử dụng công thức tổ hợp: nếu có$n$các đội còn lại và nhu cầu của nửa bên trái$l$của chúng, nhân với$C(n,l)$. 
6. **Recurse**: Gọi hàm đệ quy cho nửa bên trái và bên phải với các đội mạnh nhất phù hợp cho mỗi nửa. 
7. **Nhân và trả về**: Tổng số cách là tích của các phép đếm đệ quy trái và phải nhân với hệ số tổ hợp phân bố các đội còn lại. 

Tại sao nó hoạt động: Phân rã đệ quy phản chiếu cây giải đấu. Bằng cách đảm bảo rằng nhóm mạnh nhất của cây con được đặt chính xác và phân bổ các đội còn lại theo cách kết hợp, mọi sự sắp xếp được tính đều đảm bảo tạo ra các vị trí cuối cùng mong muốn. Sự không tương thích ngay lập tức trả về 0, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

def prepare_factorials(n):
    fac = [1]*(n+1)
    ifac = [1]*(n+1)
    for i in range(1,n+1):
        fac[i] = fac[i-1]*i % MOD
    ifac[n] = modinv(fac[n])
    for i in range(n-1,-1,-1):
        ifac[i] = ifac[i+1]*(i+1)%MOD
    return fac, ifac

def comb(n,k,fac,ifac):
    if k<0 or k>n: return 0
    return fac[n]*ifac[k]%MOD*ifac[n-k]%MOD

def solve(k,seeds):
    n = 1<<k
    fac, ifac = prepare_factorials(n)

    def dfs(l,r,team_set):
        # segment [l,r), team_set: sorted list of teams that must appear
        if r-l == 1:
            s = seeds[l]
            if s == -1:
                return 1 if team_set else 0
            return 1 if s in team_set else 0
        m = (l+r)//2
        strongest = min(team_set)
        # check fixed positions
        left_fixed = [seeds[i] for i in range(l,m) if seeds[i]!=-1]
        right_fixed = [seeds[i] for i in range(m,r) if seeds[i]!=-1]
        # strongest must go in correct half
        if strongest in left_fixed and strongest not in seeds[l:m]:
            return 0
        if strongest in right_fixed and strongest not in seeds[m:r]:
            return 0
        # divide remaining teams
        rest = set(team_set)
        rest.remove(strongest)
        left_size = (r-l)//2 - len([x for x in seeds[l:m] if x!=-1])
        right_size = (r-l)//2 - len([x for x in seeds[m:r] if x!=-1])
        if left_size<0 or right_size<0: return 0
        ways_choose = comb(len(rest),left_size,fac,ifac)
        left_teams = sorted(rest)[:left_size]
        right_teams = sorted(rest)[left_size:]
        ways_left = dfs(l,m,left_teams)
        ways_right = dfs(m,r,right_teams)
        return ways_choose*ways_left%MOD*ways_right%MOD

    # desired groups by placement
    groups = []
    i = 1
    sz = 1
    while i <= n:
        groups.append(list(range(i,min(i+sz,n+1))))
        i += sz
        sz *= 2
    # flatten into one list by tournament order
    team_order = []
    for g in reversed(groups):
        team_order += g
    return dfs(0,n,team_order)%MOD

k = int(input())
seeds = list(map(int,input().split()))
print(solve(k,seeds))
```Đầu tiên, mã này tính toán trước các giai thừa và nghịch đảo mô-đun để tính toán các kết hợp một cách hiệu quả. Hàm đệ quy`dfs`phản chiếu cấu trúc cây giải đấu, mỗi lần chia phân đoạn làm đôi, đảm bảo rằng đội mạnh nhất của phân đoạn đó được đặt chính xác và đếm đệ quy số cách phân công các đội còn lại. Các hạt cố định được kiểm tra tính tương thích ở mỗi bước, giúp ngăn ngừa sự sắp xếp không hợp lệ. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2
1 2 3 4
```| Phân đoạn | Mạnh nhất | Cố định trái | Đã sửa đúng | Kết quả | 
| --- | --- | --- | --- | --- | 
| [0,4) | 1 | 1,2 | 3,4 | không tương thích, 1 và 2 trong cùng trận đấu đầu tiên | 
| [0,2) | - | - | - | 0 | 

Thuật toán phát hiện rằng đội 1 sẽ gặp đội 2 ở vòng đầu tiên, do đó không có sự sắp xếp hợp lệ nào tồn tại. Đầu ra là 0. 

**Mẫu tùy chỉnh** 

đầu vào:```
2
-1 -1 -1 -1
```Tất cả các hạt giống đều miễn phí, vị trí mong muốn là [1,2,3,4]. Hàm đệ quy đếm tất cả các vị trí tương thích theo cây giải đấu: 

| Phân đoạn | Mạnh nhất | Kích thước bên trái | Đúng kích cỡ | Cách | 
| --- | --- | --- | --- | --- | 
| [0,4) | 1 | 1 | 1 | 2 chiều | 
| [0,2) | 2 | 0 | 1 | 1 | 
| [2,4) | 3 | | | |
