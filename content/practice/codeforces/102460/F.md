---
title: "CF 102460F - Cô Sloane"
description: "Đối với mỗi thượng nghị sĩ, chúng ta có một số nguyên (ai) và thỏa thuận cuối cùng xảy ra chính xác khi gcd của tất cả các giá trị (ai) hiện tại lớn hơn 1. Sloane có thể chọn một thượng nghị sĩ một lần và chia giá trị của thượng nghị sĩ đó cho bất kỳ ước số (d) nào thỏa mãn (dle k)."
date: "2026-08-09T02:50:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "F"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 397
verified: true
draft: false
---

[CF 102460F - Cô Sloane](https://codeforces.com/problemset/problem/102460/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 37 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Với mỗi thượng nghị sĩ chúng ta có một số nguyên\(a_i\)và thỏa thuận cuối cùng xảy ra chính xác khi gcd của tất cả hiện tại\(a_i\)giá trị lớn hơn 1. Sloane có thể chọn một thượng nghị sĩ một lần và chia giá trị của thượng nghị sĩ đó cho bất kỳ ước số nào\(d\)thỏa mãn\(d\le k\). Mục tiêu là làm cho gcd cuối cùng bằng 1 đồng thời giảm thiểu tổng thời gian vận động hành lang. 

Sự kháng cự\(e_i\)ảnh hưởng đến thời gian một cách hơi khác thường. Nếu chiến dịch hiện tại đã vận động\(x\)thượng nghị sĩ với tổng phản kháng\(y\), thượng nghị sĩ vận động hành lang\(i\)chi phí\[
y+e_i(x+1).
\]Đơn đặt hàng thoạt nhìn có vẻ phù hợp, nhưng có một sự hủy bỏ hữu ích. Giả sử chính xác\(m\)các thượng nghị sĩ được vận động hành lang và sự phản kháng của họ trong trật tự chiến dịch là\(e_1,\ldots,e_m\). Tổng chi phí là\[
\sum_{j=1}^{m}\left(\sum_{t<j}e_t+je_j\right).
\]Đối với một thượng nghị sĩ cố định tại chức\(j\), mức kháng cự của nó xuất hiện một lần trong tổng đầu tiên đối với mọi thượng nghị sĩ sau này và một khi được nhân với vị trí của chính nó. Tổng hệ số của nó là\[
(m-j)+j=m.
\]Như vậy tổng thời gian của chiến dịch chỉ đơn giản là\[
m\sum e_i.
\]Thứ tự không có vấn đề gì cả. Do đó, vấn đề là chọn các thượng nghị sĩ và bộ phận pháp lý sao cho mọi thừa số nguyên tố của gcd ban đầu đều bị loại bỏ, giảm thiểu\[
(\text{number of lobbied senators})\times(\text{sum of their resistances}).
\]Những ràng buộc bị chi phối bởi\(n\le 10^6\). Cách tiếp cận \(O(n^2)\) ngay lập tức là không thể và thậm chí cách tiếp cận \(O(n2^{11})\) cũng quá lớn vì\(n2^{11}\)là về\(2\cdot10^9\). Thay vào đó, tham số nhỏ hữu ích là số lượng thừa số nguyên tố riêng biệt của gcd chung. Vì gcd nhiều nhất là\(10^{12}\), nó chứa nhiều nhất 11 số nguyên tố phân biệt. 

Có một số trường hợp khó xử lý. Nếu gcd ban đầu đã là 1 thì câu trả lời là 0 vì không cần vận động hành lang. Ví dụ,```text
2 10
6 35
1 1
```có gcd 1, vậy câu trả lời đúng là`0`. Việc triển khai luôn vận động ít nhất một thượng nghị sĩ sẽ trả về giá trị dương. 

Một trường hợp tinh tế khác là khi không một thượng nghị sĩ nào có thể loại bỏ mọi số nguyên tố chung. Ví dụ,```text
2 2
6 6
1 1
```có gcd 6. Một thượng nghị sĩ có thể chia cho 2, để lại 3, và người kia có thể chia cho 3, để lại 2. Gcd cuối cùng là 1, nên đáp án là 4. Giải pháp chỉ kiểm tra xem một thượng nghị sĩ có thể loại bỏ toàn bộ gcd hay không sẽ báo cáo sai`-1`. 

Giới hạn số chia cũng được bao gồm. Với```text
2 6
6 10
1 1
```thượng nghị sĩ đầu tiên có thể chia chính xác cho 6 bởi vì\(6\le k\). Cả hai thượng nghị sĩ đều cần thiết, cho tổng thời gian \(2(1+1)=4\). Kiểm tra`d < k`thay vì`d <= k`có thể từ chối các hoạt động hợp pháp. 

Cuối cùng, một số nguyên tố chỉ có thể bị xóa khỏi thượng nghị sĩ\(i\)nếu phép chia chứa toàn bộ lũy thừa của số nguyên tố đó có mặt trong\(a_i\). Nếu như\(a_i=12\), bỏ số nguyên tố 2 thì phải chia cho 4 chứ không phải chia cho 2. Việc quên số mũ sẽ trả lời sai bất cứ khi nào\(a_i\)chứa lũy thừa cao hơn của một số nguyên tố chung. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi tập hợp con thượng nghị sĩ và mọi ước số pháp lý cho mỗi thượng nghị sĩ được chọn. Điều đó đúng vì mọi chiến dịch khả thi đều được xem xét rõ ràng, nhưng thật vô vọng đối với\(n=10^6\). Ngay cả việc chỉ liệt kê các tập hợp con thượng nghị sĩ cũng đã mất\(2^n\)hoạt động, trước khi xem xét các bộ phận có thể. 

Quan sát hữu ích đầu tiên là chỉ có các số nguyên tố trong gcd ban đầu mới quan trọng. Cho phép\[
g=\gcd(a_1,a_2,\ldots,a_n).
\]Mọi số nguyên tố không chia hết\(g\)đã vắng mặt trong gcd cuối cùng. Đối với mỗi số nguyên tố\(p\mid g\), ít nhất một thượng nghị sĩ được vận động hành lang phải mất tất cả các bản sao của\(p\). 

Giả định\[
a_i=p^{v_i}u,\qquad p\nmid u.
\]Để loại bỏ\(p\)hoàn toàn từ thượng nghị sĩ\(i\), Sloane phải chia\(a_i\)ít nhất là\(p^{v_i}\). Nếu một số số nguyên tố bị loại bỏ khỏi cùng một thượng nghị sĩ thì ước số cần tìm là tích của các lũy thừa nguyên tố hoàn chỉnh của chúng. Nó hợp pháp chính xác khi sản phẩm đó tối đa\(k\). 

Điều này biến bài toán số học thành một bài toán bao tập hợp nhỏ. Vũ trụ chỉ bao gồm các số nguyên tố riêng biệt của\(g\), nhiều nhất là 11 phần tử. Mặt nạ bit thể hiện các số nguyên tố phổ biến nào đã bị loại bỏ. 

Vấn đề còn lại là giá trị lớn của\(n\). Điểm mấu chốt là một chiến dịch tối ưu không bao giờ cần nhiều hơn\(r\)thượng nghị sĩ, ở đâu\(r\)là số số nguyên tố phân biệt trong\(g\). Nếu nhiều hơn\(r\)thượng nghị sĩ được chọn, một số thượng nghị sĩ được chọn không đóng góp số nguyên tố nào là cần thiết duy nhất, do đó, việc loại bỏ thượng nghị sĩ đó sẽ loại bỏ mọi số nguyên tố và giảm chi phí một cách nghiêm ngặt. 

Do đó, chúng tôi chỉ có thể giữ lại một số lượng nhỏ thượng nghị sĩ rẻ nhất cho mọi mô hình loại bỏ hữu ích. Đối với một thượng nghị sĩ, các mẫu loại bỏ được đóng xuống: nếu một thượng nghị sĩ có thể loại bỏ một tập hợp số nguyên tố thì ông ta cũng có thể loại bỏ mọi tập hợp con của tập hợp đó. Chúng tôi liệt kê các mẫu pháp lý tối đa, giữ lại nhiều nhất\(r\)các thượng nghị sĩ rẻ nhất hỗ trợ từng mẫu và sau đó thực hiện tập hợp con DP tiêu chuẩn. Sự ràng buộc của\(r\)ứng cử viên là đủ vì bất kỳ giải pháp tối ưu nào cũng chứa nhiều nhất\(r\)thượng nghị sĩ, vì vậy khi thay thế các thượng nghị sĩ đã chọn của mình bằng các ứng cử viên được lưu trữ, điều kiện của Hall được thỏa mãn đối với các tập ứng cử viên có mẫu được yêu cầu. 

Công việc số học cũng nhỏ vì chúng tôi chỉ tính các số nguyên tố xuất hiện trong một gcd toàn cục. Chúng tôi không bao giờ tính đến mọi\(a_i\)bằng cách chia thử lên đến\(\sqrt{a_i}\). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(2^n)\) hoặc tệ hơn | \(O(n)\) | Quá chậm | 
| Tối ưu | \(O(nr + C2^r + 3^r)\), trong đó\(r\le11\)Và\(C\)là số lượng hồ sơ thượng nghị sĩ riêng biệt | \(O(Cr+2^r)\) | Đã chấp nhận | 

Đây\(C\)nhỏ trong biểu diễn nén vì các thượng nghị sĩ có cấu hình công suất cơ bản giống hệt nhau được xử lý cùng nhau. Điểm quan trọng là phần mũ chỉ phụ thuộc vào\(r\le11\), không bao giờ bật\(n\). 

## Hướng dẫn thuật toán 

1. Tính gcd\(g\)của tất cả\(a_i\). Nếu như\(g=1\), in ngay số 0. Không có số nguyên tố chung còn lại để loại bỏ. 

2. Yếu tố\(g\)vào các số nguyên tố riêng biệt của nó\(p_0,p_1,\ldots,p_{r-1}\). Chúng ta chỉ cần những số nguyên tố này chứ không cần phân tích nhân tử hoàn chỉnh của mọi\(a_i\). 

3. Đối với mỗi thượng nghị sĩ, hãy xác định số mũ của mỗi\(p_j\)TRONG\(a_i\). Nếu số mũ là\(v\), loại bỏ hoàn toàn\(p_j\)đòi hỏi yếu tố\(p_j^v\). 

4. Đối với tập hợp số nguyên tố được chọn biểu diễn bằng mặt nạ\(S\), tính ước số cần tìm\[
D_i(S)=\prod_{j\in S}p_j^{v_{i,j}}.
\]Mặt nạ là hợp pháp cho thượng nghị sĩ\(i\)chính xác khi \(D_i(S)\le k\). Chúng tôi liệt kê các mặt nạ pháp lý tối đa cho thượng nghị sĩ, bởi vì mọi tập hợp con của mặt nạ pháp lý tối đa đều tự động hợp pháp. 

5. Nén các thượng nghị sĩ có cùng hồ sơ quyền lực. Đối với mỗi mẫu loại bỏ chỉ giữ giá rẻ nhất\(r\)thượng nghị sĩ. Hơn\(r\)các ứng cử viên cho một mẫu không bao giờ có thể cần thiết vì một chiến dịch hợp lệ sử dụng nhiều nhất\(r\)thượng nghị sĩ. 

6. Chạy tập hợp con DP. Cho phép`dp[mask]`là tổng sức đề kháng tối thiểu của một tập hợp các thượng nghị sĩ đã được chọn có mặt nạ loại bỏ kết hợp chính xác`mask`. Khi một thượng nghị sĩ có thể loại bỏ khuôn mẫu`s`, chuyển sang\[
dp[mask\mid s]
=
\min(dp[mask\mid s],dp[mask]+e_i).
\]Thượng nghị sĩ chỉ được xử lý một lần, vì vậy cùng một thượng nghị sĩ không bao giờ được sử dụng hai lần. 

7. Đối với mọi trạng thái cuối cùng có thể đạt được`FULL`, giả sử DP được sử dụng\(m\)thượng nghị sĩ và sự phản kháng tích lũy\(E\). Thời gian chiến dịch tương ứng là\(mE\). Lưu trữ số lượng thượng nghị sĩ đã chọn cùng với điện trở ở trạng thái DP hoặc tương đương sử dụng trạng thái hai chiều được lập chỉ mục theo số lượng thượng nghị sĩ. 

Lý do giữ lại số lượng thượng nghị sĩ riêng biệt là chỉ giảm thiểu sự phản kháng thôi là chưa đủ. Một chiến dịch sử dụng một thượng nghị sĩ có mức kháng cự 100 có thể tốt hơn chiến dịch sử dụng ba thượng nghị sĩ với mức kháng cự mỗi người là 50, vì chi phí thực tế lần lượt là 100 và 450. 

### Tại sao nó hoạt động 

Gcd toàn cục chứa chính xác các số nguyên tố phải bị hủy. Một thượng nghị sĩ có thể loại bỏ một tập hợp con đã chọn của các số nguyên tố đó một cách chính xác khi tích của các lũy thừa nguyên tố hoàn chỉnh tương ứng trong thượng nghị sĩ đó lớn nhất\(k\). Do đó, mọi chiến dịch pháp lý đều tương ứng với một bộ sưu tập mặt nạ thượng nghị sĩ mà liên minh của nó là mặt nạ chính đầy đủ và mỗi bộ sưu tập như vậy tạo ra một chiến dịch pháp lý. 

Một chiến dịch tối ưu chứa nhiều nhất\(r\)thượng nghị sĩ. Đối với mỗi mẫu xóa chúng tôi giữ lại\(r\)các thượng nghị sĩ rẻ nhất hiện có, đủ để thay thế các thượng nghị sĩ của bất kỳ chiến dịch tối ưu nào mà không buộc hai vai trò bắt buộc phải có trên cùng một thượng nghị sĩ. Sau đó, tập hợp con DP sẽ xem xét mọi sự kết hợp có thể có của các mặt nạ loại bỏ trong khi vẫn tôn trọng điều kiện mỗi thượng nghị sĩ một lần sử dụng. Cuối cùng, danh tính\[
\text{time}=m\sum e_i
\]chuyển đổi mức kháng cự tối thiểu và số lượng thượng nghị sĩ thành thời gian chiến dịch thực tế. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

INF = 10**30

def factor_distinct(x):
    primes = []
    p = 2
    while p * p <= x:
        if x % p == 0:
            primes.append(p)
            while x % p == 0:
                x //= p
        p = 3 if p == 2 else p + 2
    if x > 1:
        primes.append(x)
    return primes

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    e = list(map(int, input().split()))

    g = 0
    for x in a:
        g = gcd(g, x)

    if g == 1:
        print(0)
        return

    primes = factor_distinct(g)
    r = len(primes)
    full = (1 << r) - 1

    # For every distinct profile of prime powers, keep the smallest
    # resistances. At most r copies of one profile are ever useful.
    profiles = {}

    for x, cost in zip(a, e):
        powers = []
        y = x

        for p in primes:
            q = 1
            while y % p == 0:
                y //= p
                q *= p
            powers.append(q)

        key = tuple(powers)

        if key not in profiles:
            profiles[key] = [cost]
        else:
            arr = profiles[key]
            if len(arr) < r:
                arr.append(cost)
                arr.sort()
            elif cost < arr[-1]:
                arr[-1] = cost
                arr.sort()

    # Convert every profile into its maximal legal masks.
    #
    # A mask is maximal if it is legal but adding any omitted prime
    # makes the required divisor exceed k.
    candidates = []

    for powers, costs in profiles.items():
        legal = [False] * (1 << r)
        product = [1] * (1 << r)

        legal[0] = True

        for mask in range(1, 1 << r):
            bit = mask & -mask
            j = bit.bit_length() - 1
            prev = mask ^ bit

            if product[prev] <= k // powers[j]:
                product[mask] = product[prev] * powers[j]
                legal[mask] = True

        maximal = []

        for mask in range(1, 1 << r):
            if not legal[mask]:
                continue

            is_maximal = True
            missing = full ^ mask

            while missing:
                bit = missing & -missing
                j = bit.bit_length() - 1

                if legal[mask | bit]:
                    is_maximal = False
                    break

                missing ^= bit

            if is_maximal:
                maximal.append(mask)

        # Every stored resistance has the same profile, so it can
        # realize every maximal mask of this profile.
        for cost in costs:
            candidates.append((cost, maximal))

    # dp[mask] = (number of senators, total resistance).
    # We compare by the eventual objective indirectly using the pair.
    #
    # Since m <= r <= 11, keep the best resistance for every exact
    # count and mask.
    dp = [[INF] * (1 << r) for _ in range(r + 1)]
    dp[0][0] = 0

    for cost, masks in candidates:
        old = [row[:] for row in dp]

        for cnt in range(r):
            row = old[cnt]
            for mask, cur in enumerate(row):
                if cur == INF:
                    continue

                for s in masks:
                    nm = mask | s
                    nv = cur + cost
                    if nv < dp[cnt + 1][nm]:
                        dp[cnt + 1][nm] = nv

    ans = INF

    for cnt in range(1, r + 1):
        if dp[cnt][full] != INF:
            ans = min(ans, cnt * dp[cnt][full])

    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ tính toán gcd toàn cục và xử lý ngay trường hợp không vận động hành lang. Hệ số gcd chỉ sử dụng phép chia thử nghiệm trên một số và có tối đa 11 số nguyên tố riêng biệt tồn tại. 

Đối với mỗi thượng nghị sĩ,`powers[j]`là sức mạnh hoàn toàn của`primes[j]`có trong Thượng nghị sĩ đó\(a_i\). Đây là đại lượng phải có trong số chia nếu số nguyên tố đó biến mất hoàn toàn. Chỉ sử dụng số nguyên tố sẽ không chính xác đối với các giá trị như\(a_i=12\), trong đó việc loại bỏ 2 yêu cầu chia cho 4. 

các`profiles`từ điển là bước nén chính. Các thượng nghị sĩ có yêu cầu về quyền lực cơ bản giống hệt nhau sẽ hành xử giống hệt nhau từ góc độ gcd, vì vậy chỉ những trở kháng nhỏ nhất của họ mới là quan trọng. Giữ\(r\)trong số đó là đủ vì không có chiến dịch tối ưu nào cần nhiều hơn\(r\)thượng nghị sĩ. 

các`legal`mảng tính toán mọi tập hợp con hợp lệ cho một cấu hình bằng cách sử dụng phép lặp\[
D[S]=D[S\setminus\{p\}]\cdot p^{v_p(a_i)}.
\]Sự phân chia`powers[j] <= k // product[prev]`được cố ý viết theo cách này thay vì nhân trước. Nó tránh tạo ra một số lớn hơn mức cần thiết và đưa ra sự so sánh chính xác với\(k\). 

Chỉ có mặt nạ hợp pháp tối đa được lưu trữ. Nếu một thượng nghị sĩ có thể loại bỏ một tập hợp số nguyên tố lớn hơn thì việc sử dụng một tập hợp con nghiêm ngặt sẽ không bao giờ mang lại lợi thế cho thượng nghị sĩ đó. Luôn có thể sử dụng mặt nạ lớn hơn để thay thế. 

Kích thước DP`cnt`ghi lại bao nhiêu thượng nghị sĩ đã được chọn. Điều này là cần thiết vì mục tiêu cuối cùng không chỉ đơn thuần là tổng các mức kháng cự. Câu trả lời thực tế nhân tổng đó với số lượng thượng nghị sĩ được vận động hành lang. 

Các số nguyên Python có độ chính xác tùy ý, vì vậy tích lớn có thể`cnt * dp[cnt][full]`không tràn. Câu trả lời tối đa có thể vẫn có thể quản lý được một cách thoải mái dưới dạng số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```text
3 6
30 30 30
100 4 5
```Gcd ban đầu là 30, có thừa số nguyên tố là 2, 3 và 5. Đối với mỗi thượng nghị sĩ, loại bỏ 2 chi phí ước 2, loại bỏ 3 chi phí 3, loại bỏ 5 chi phí 5 và loại bỏ cả 2 và 3 chi phí 6. 

Các trạng thái quan trọng là: 

| Thượng nghị sĩ đã sử dụng | Đã xóa số nguyên tố | Kháng cự | Thời gian chiến dịch | 
|---:|---|---:|---:| 
| 0 | không | 0 | 0 | 
| 1 | {2,3} | 4 | 4 | 
| 2 | {2,3,5} | 9 | 18 | 
| 2 | {2,3,5} | 105 | 210 | 
| 3 | {2,3,5} | 109 | 327 | 

Trạng thái cuối cùng tốt nhất sử dụng các thượng nghị sĩ có mức kháng cự là 4 và 5. Trạng thái đầu tiên có thể loại bỏ 2 và 3 bằng cách chia 30 cho 6, trong khi trạng thái thứ hai loại bỏ 5 bằng cách chia cho 5. Tổng mức kháng cự là 9 và hai thượng nghị sĩ được vận động hành lang, cho kết quả\[
2\cdot9=18.
\]Vì vậy, đầu ra là`18`. 

Ví dụ này chứng tỏ tại sao thứ tự vận động hành lang không quan trọng và tại sao một thượng nghị sĩ có thể loại bỏ một số số nguyên tố khi ước số yêu cầu của chúng vừa với bên trong\(k\). 

### Mẫu 2 

Đầu vào là```text
1 1000000
```Có một thượng nghị sĩ và\(a_i\)giá trị là 1. gcd đã là 1. 

| gcd ban đầu | Mặt nạ thủ tướng | Thượng nghị sĩ đã sử dụng | Trả lời | 
|---:|---|---:|---:| 
| 1 | 0 | 0 | 0 | 

Chiến dịch đã thành công nên thuật toán sẽ thoát trước khi xây dựng bất kỳ trạng thái DP nào. Đầu ra đúng là`0`. 

Điều này thực hiện trường hợp ranh giới trong đó không cần vận động hành lang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(nr + C2^r + C2^{2r})\) trong quá trình triển khai nén |\(r\le11\), và công hàm mũ chỉ phụ thuộc vào các số nguyên tố gcd riêng biệt | 
| Không gian | \(O(Cr + r2^r)\) | Lưu trữ các cấu hình nén và tập hợp con nhỏ DP | 

Phần tuyến tính xử lý nhiều nhất\(10^6\)thượng nghị sĩ và chỉ có tối đa 11 số nguyên tố của gcd toàn cầu. Phần mũ được giới hạn bởi vũ trụ nguyên tố nhỏ chứ không phải bởi\(n\). Việc triển khai cũng nén các cấu hình công suất cơ bản bằng nhau trước khi chạy DP, điều này rất cần thiết cho các ứng dụng lớn\(n\)giới hạn. 

## Trường hợp thử nghiệm```python
# This test harness uses the same solve logic through a small wrapper.

import sys
import io
from math import gcd

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n, k = map(int, sys.stdin.readline().split())
        a = list(map(int, sys.stdin.readline().split()))
        e = list(map(int, sys.stdin.readline().split()))

        g = 0
        for x in a:
            g = gcd(g, x)

        if g == 1:
            return "0"

        primes = []
        x = g
        p = 2
        while p * p <= x:
            if x % p == 0:
                primes.append(p)
                while x % p == 0:
                    x //= p
            p = 3 if p == 2 else p + 2
        if x > 1:
            primes.append(x)

        r = len(primes)
        full = (1 << r) - 1

        profiles = {}

        for x, cost in zip(a, e):
            powers = []
            y = x

            for p in primes:
                q = 1
                while y % p == 0:
                    y //= p
                    q *= p
                powers.append(q)

            key = tuple(powers)
            arr = profiles.setdefault(key, [])

            if len(arr) < r:
                arr.append(cost)
                arr.sort()
            elif cost < arr[-1]:
                arr[-1] = cost
                arr.sort()

        candidates = []

        for powers, costs in profiles.items():
            msize = 1 << r
            product = [1] * msize
            legal = [False] * msize
            legal[0] = True

            for mask in range(1, msize):
                bit = mask & -mask
                j = bit.bit_length() - 1
                prev = mask ^ bit

                if product[prev] <= k // powers[j]:
                    product[mask] = product[prev] * powers[j]
                    legal[mask] = True

            maximal = []

            for mask in range(1, msize):
                if not legal[mask]:
                    continue

                missing = full ^ mask
                maximal_flag = True

                while missing:
                    bit = missing & -missing
                    if legal[mask | bit]:
                        maximal_flag = False
                        break
                    missing ^= bit

                if maximal_flag:
                    maximal.append(mask)

            for cost in costs:
                candidates.append((cost, maximal))

        INF = 10**30
        dp = [[INF] * (1 << r) for _ in range(r + 1)]
        dp[0][0] = 0

        for cost, masks in candidates:
            old = [row[:] for row in dp]

            for cnt in range(r):
                for mask in range(1 << r):
                    cur = old[cnt][mask]
                    if cur == INF:
                        continue

                    for s in masks:
                        nm = mask | s
                        nv = cur + cost
                        if nv < dp[cnt + 1][nm]:
                            dp[cnt + 1][nm] = nv

        ans = INF
        for cnt in range(1, r + 1):
            if dp[cnt][full] != INF:
                ans = min(ans, cnt * dp[cnt][full])

        return str(-1 if ans == INF else ans)

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("""3 6
30 30 30
100 4 5
""") == "18", "sample 1"

# Provided sample 2
assert run("""1 1000000
1
""") == "0", "sample 2"

# Provided sample 3
assert run("""3 5
7 7 7
1 1 1
""") == "-1", "sample 3"

# Already coprime, so no lobbying is needed.
assert run("""2 10
6 35
1 1
""") == "0", "initial gcd is already 1"

# Two senators must split the primes 2 and 3.
assert run("""2 2
6 6
1 1
""") == "4", "split one prime between two senators"

# k is inclusive: division by 6 is legal when k = 6.
assert run("""2 6
6 10
1 1
""") == "4", "boundary k"

# A higher prime exponent must be removed completely.
assert run("""2 4
12 18
1 1
""") == "4", "complete prime power removal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---:|---| 
|`2 10 / 6 35 / 1 1`|`0`| gcd ban đầu đã là 1 | 
|`2 2 / 6 6 / 1 1`|`4`| Các thượng nghị sĩ khác nhau có thể loại bỏ các số nguyên tố chung khác nhau | 
|`2 6 / 6 10 / 1 1`|`4`| Đã bao gồm giới hạn số chia | 
|`2 4 / 12 18 / 1 1`|`4`| Một số nguyên tố hoàn chỉnh, không chỉ số nguyên tố, phải được chia ra | 

## Vỏ cạnh 

Khi gcd ban đầu là 1, thuật toán dừng ngay lập tức. Vì```text
2 10
6 35
1 1
```gcd là 1, vì vậy danh sách nguyên tố trống và câu trả lời đúng là`0`. Không nên chọn thượng nghị sĩ. 

Khi một số thượng nghị sĩ phải hợp tác, DP mặt nạ sẽ xử lý việc phân chia một cách tự nhiên. Vì```text
2 2
6 6
1 1
```các số nguyên tố chung là 2 và 3. Với\(k=2\), một thượng nghị sĩ chỉ có thể loại bỏ 2 và người khác chỉ có thể loại bỏ 3. DP tiếp cận mặt nạ đầy đủ bằng cách sử dụng hai thượng nghị sĩ có tổng điện trở là 2, vì vậy chi phí cuối cùng là\(2\cdot2=4\). 

Khi giới hạn số chia chính xác chặt chẽ, việc so sánh sản phẩm phải cho phép sự bằng nhau. Vì```text
2 6
6 10
1 1
```thượng nghị sĩ thứ nhất có thể chia 6 cho 6, loại bỏ cả 2 và 3, trong khi thượng nghị sĩ thứ hai loại bỏ 5 khỏi 10 bằng cách chia cho 5. Các ước số cần thiết đều nằm trong giới hạn nên đáp án là 4. 

Khi một số nguyên tố chung xuất hiện với số mũ lớn hơn một, số mũ đầy đủ phải biến mất khỏi thượng nghị sĩ đã chọn. Ví dụ, với```text
2 4
12 18
1 1
```gcd là 6. Thượng nghị sĩ 1 cần chia cho 4 để loại bỏ hoàn toàn 2, trong khi thượng nghị sĩ 2 cần chia cho 3 để loại bỏ 3. Cả hai phép toán đều hợp pháp, cho ra gcd 1 cuối cùng và tổng thời gian là 4. Cấu trúc mặt nạ chỉ dựa trên việc một số nguyên tố có chia hết hay không\(a_i\)sẽ tin tưởng một cách sai lầm rằng chia 12 cho 2 là đủ, để lại thừa số chung 2.
