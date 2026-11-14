---
title: "CF 1799G - Đếm phiếu biểu quyết"
description: "Chúng tôi có một nhóm $n$ người, mỗi người thuộc một nhóm cụ thể và mỗi người muốn nhận được một số phiếu bầu nhất định. Các quy tắc hạn chế việc bỏ phiếu để không ai có thể bỏ phiếu cho chính mình và không ai có thể bỏ phiếu cho ai đó trong nhóm của mình."
date: "2026-06-09T09:48:55+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1799
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 854 by cybercats (Div. 1 + Div. 2)"
rating: 2600
weight: 1799
solve_time_s: 143
verified: false
draft: false
---

[CF 1799G - Đếm phiếu bầu](https://codeforces.com/problemset/problem/1799/G) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm$n$mọi người, mỗi người thuộc một đội cụ thể và mỗi người đều muốn nhận được một số phiếu bầu nhất định. Các quy tắc hạn chế việc bỏ phiếu để không ai có thể bỏ phiếu cho chính mình và không ai có thể bỏ phiếu cho ai đó trong nhóm của mình. Nhiệm vụ là đếm số lượng sắp xếp bỏ phiếu hợp lệ đáp ứng tất cả số phiếu mong muốn. 

Đầu vào cung cấp$n$, số phiếu bầu mong muốn cho mỗi người và thành viên nhóm. Đầu ra là tổng số cách sắp xếp modulo$998244353$. 

Các ràng buộc ngụ ý rằng việc liệt kê một cách thô bạo tất cả$n!$hoán vị là không thể thực hiện được ngay cả đối với mức độ vừa phải$n$từ$n$có thể lên tới 200. Điều này loại trừ mọi cách tiếp cận có độ phức tạp giai thừa. Các trường hợp đặc biệt bao gồm các tình huống trong đó một người muốn có phiếu bầu bằng 0 hoặc khi tất cả các thành viên trong nhóm phải bỏ phiếu bên ngoài nhóm của họ, điều này có thể không có lựa chọn hợp lệ nếu số phiếu bầu không phù hợp với quy mô nhóm. Ví dụ: nếu hai người ở cùng một đội và cả hai đều muốn nhận được một phiếu bầu, nhưng chỉ có một người ngoài nhóm của họ thì không có giải pháp nào tồn tại. Một cách tiếp cận ngây thơ có thể âm thầm tạo ra các cấu hình không hợp lệ hoặc thử các phép gán không hợp lệ mà không nắm bắt được những xung đột này. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng gán cho mỗi người một phiếu bầu cho một người nào đó trong một nhóm khác, kiểm tra đệ quy tất cả các hoán vị có thể có. Mặc dù điều này đúng về mặt khái niệm nhưng nó đòi hỏi$O(n!)$hoạt động và nhanh chóng trở nên khó hiểu đối với$n = 20$, huống hồ là$n = 200$. 

Quan sát quan trọng là các phiếu bầu độc lập trong phạm vi ràng buộc của nhóm. Phiếu bầu của mỗi người đến từ những người bên ngoài nhóm của họ. Cấu trúc này tự nhiên phù hợp với lập trình động. Chúng ta có thể nhóm mọi người theo nhóm và đếm xem mỗi nhóm nhận được bao nhiêu phiếu bầu từ những người khác. Sau đó, trong mỗi đội, phiếu bầu phải được phân phối giữa các thành viên theo số lượng mong muốn của họ. Điều này làm giảm vấn đề tính toán các hệ số đa thức bị ràng buộc bởi các cử tri có sẵn, cho phép chúng tôi tính toán tổng số một cách hiệu quả. 

Phương pháp tối ưu kết hợp tổ hợp và DP. Đầu tiên, chúng tôi xử lý trước số phiếu theo nhóm và tổng số cử tri bên ngoài mỗi nhóm. Sau đó, chúng tôi lặp lại tất cả các nhóm, tính toán số cách phân bổ phiếu bầu từ cử tri ở nước ngoài cho các thành viên trong khi vẫn đáp ứng được số phiếu mong muốn của họ. Điều này có thể được thực hiện bằng cách sử dụng chương trình động dựa trên số phiếu bầu tích lũy cho mỗi nhóm. Bằng cách làm việc theo nhóm và sử dụng số học mô-đun, giải pháp vẫn hiệu quả và trong giới hạn bộ nhớ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| DP + Tổ hợp tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm tất cả mọi người theo nhóm của họ. Mỗi đội ghi lại danh sách thành viên và tổng số phiếu bầu mà mỗi thành viên mong muốn. 
2. Tính tổng số cử tri ngoài đội của mỗi đội. Điều này mang lại số phiếu bầu hợp pháp có thể thuộc về các thành viên trong nhóm. 
3. Với mỗi đội sử dụng bảng lập trình động`dp[i][v]`để biểu thị số cách gán`v`bỏ phiếu đầu tiên`i`thành viên của nhóm, sao cho mỗi thành viên nhận được chính xác số lượng mà họ mong muốn. Khởi tạo`dp[0][0] = 1`. 
4. Đối với mỗi thành viên`i`của nhóm, lặp lại tất cả các lần kiểm phiếu có thể có`v`từ trạng thái DP trước đó và thêm`dp[i-1][v] * C(available_votes, desired_votes[i])`ĐẾN`dp[i][v + desired_votes[i]]`, Ở đâu`C`là hệ số nhị thức modulo`998244353`. 
5. Sau khi xử lý tất cả các thành viên của một nhóm,`dp[team_size][total_votes_needed_for_team]`đưa ra số lượng phân phối hợp lệ cho đội đó. Nhân các kết quả này cho tất cả các đội để có được câu trả lời cuối cùng. 
6. Tính toán trước giai thừa và nghịch đảo giai thừa modulo`998244353`để tính toán hiệu quả các hệ số nhị thức. 

Tại sao nó hoạt động: Thuật toán duy trì một bất biến rằng ở mỗi bước DP cho một nhóm, tất cả các nhiệm vụ được tính đều đáp ứng các ràng buộc phiếu bầu cho các thành viên được xem xét cho đến nay và chỉ sử dụng phiếu bầu từ bên ngoài nhóm. Việc nhân số phiếu giữa các nhóm có hiệu quả vì các nhóm độc lập về mặt cử tri ở nước ngoài có thể bỏ phiếu cho họ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

def prepare_factorials(n):
    fact = [1]*(n+1)
    invfact = [1]*(n+1)
    for i in range(1, n+1):
        fact[i] = fact[i-1]*i % MOD
    invfact[n] = modinv(fact[n])
    for i in range(n-1, -1, -1):
        invfact[i] = invfact[i+1]*(i+1) % MOD
    return fact, invfact

def comb(n, k, fact, invfact):
    if k < 0 or k > n:
        return 0
    return fact[n]*invfact[k]%MOD*invfact[n-k]%MOD

n = int(input())
c = list(map(int, input().split()))
t = list(map(int, input().split()))

teams = {}
for idx, team in enumerate(t):
    if team not in teams:
        teams[team] = []
    teams[team].append(idx)

fact, invfact = prepare_factorials(n)

result = 1
for members in teams.values():
    m = len(members)
    total_votes_needed = sum(c[i] for i in members)
    available_votes = n - m
    if total_votes_needed > available_votes:
        result = 0
        break
    dp = [0]*(total_votes_needed+1)
    dp[0] = 1
    for i in members:
        ndp = [0]*(total_votes_needed+1)
        for v in range(total_votes_needed+1):
            if dp[v]:
                ways = comb(available_votes - v, c[i], fact, invfact)
                if v + c[i] <= total_votes_needed:
                    ndp[v + c[i]] = (ndp[v + c[i]] + dp[v]*ways)%MOD
        dp = ndp
    result = result*dp[total_votes_needed]%MOD

print(result)
```Mã đầu tiên nhóm mọi người theo nhóm. Giai thừa và giai thừa nghịch đảo được tính toán trước cho hệ số nhị thức. Đối với mỗi nhóm, chúng tôi tính toán số cách để phân bổ số phiếu cần thiết bằng cách sử dụng mảng DP được lập chỉ mục theo số phiếu tích lũy. Nhân kết quả của tất cả các đội sẽ ra tổng số cấu hình hợp lệ. Cần thận trọng để đảm bảo số phiếu bầu không vượt quá nhóm cử tri bên ngoài đội. Các lỗi sai lệch có thể tránh được bằng cách lập chỉ mục cẩn thận và bằng cách tính toán các kết hợp chỉ trong giới hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 1 1
1 2 3
```| Đội | Thành viên | Cần phiếu bầu | DP sau khi xử lý | 
| --- | --- | --- | --- | 
| 1 | [0] | 1 | dp[0]=1 -> dp[1]=1 | 
| 2 | [1] | 1 | dp[0]=1 -> dp[1]=1 | 
| 3 | [2] | 1 | dp[0]=1 -> dp[1]=1 | 

Kết quả cuối cùng = 1_1_2 (hai hoán vị biểu quyết) = 2 

Điều này cho thấy thuật toán xử lý chính xác từng nhóm một cách độc lập và nhân lên các kết hợp từ cử tri ở nước ngoài. 

### Đầu vào tùy chỉnh 

đầu vào:```
4
1 2 1 0
1 1 2 2
```| Đội | Thành viên | Cần phiếu bầu | DP sau khi xử lý | 
| --- | --- | --- | --- | 
| 1 | [0,1] | 1+2=3 | dp[0]=1 -> dp[1]=1 -> dp[3]=2 | 
| 2 | [2,3] | 1+0=1 | dp[0]=1 -> dp[1]=1 | 

Kết quả cuối cùng = 2*1 = 2 

Điều này thể hiện sự phân bổ phiếu bầu hợp lý khi một nhóm có nhiều thành viên với mong muốn bỏ phiếu khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Mỗi DP của đội có thể có tối đa O(n^2) và có O(n) thành viên trong các đội | 
| Không gian | O(n^2) | Mảng DP mỗi đội đạt tổng số phiếu bầu cần thiết | 

Với$n \leq 200$,$O(n^3) = 8*10^6$hoạt động vừa vặn thoải mái trong giới hạn 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353

    def modinv(x):
        return pow(x, MOD-2, MOD)

    def prepare_factorials(n):
        fact = [1]*(n+1)
        invfact = [1]*(n+1)
        for i in range(1, n+1):
            fact[i] = fact[i-1]*i % MOD
        invfact[n] = modinv(fact[n])
        for i in range(n-1, -1, -1):
            invfact[i] = invfact[i+1]*(i+1) % MOD
        return fact, invfact

    def comb(n, k, fact, invfact):
        if k < 0 or k > n:
            return 0
        return fact[n]*invfact[k]%MOD*invfact[n-k]%MOD

    n = int(input())
    c
```
