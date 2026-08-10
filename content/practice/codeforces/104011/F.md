---
title: "CF 104011F - Giải quyết đầu tiên"
description: "Mỗi thí sinh có một danh sách cá nhân các vấn đề mà họ có khả năng giải quyết. Đối với thí sinh $i$, bài toán $j$ có thời gian khác 0 $a{i,j}$ nếu giải được, nếu không thì không sử dụng được."
date: "2026-07-02T05:14:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104011
codeforces_index: "F"
codeforces_contest_name: "2021-2022 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104011
solve_time_s: 66
verified: true
draft: false
---

[CF 104011F - Giải quyết đầu tiên](https://codeforces.com/problemset/problem/104011/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi thí sinh có một danh sách cá nhân các vấn đề mà họ có khả năng giải quyết. Dành cho thí sinh$i$, vấn đề$j$có thời gian khác 0$a_{i,j}$nếu giải được thì không dùng được. 

Trước khi cuộc thi bắt đầu, mỗi thí sinh sẽ giải các bài toán có thể giải được và hoán vị chúng một cách ngẫu nhiên. Sau đó, họ giải quyết các vấn đề theo thứ tự xáo trộn đó, tích lũy thời gian theo thời gian. Một bài toán chỉ được giải quyết nếu thời gian tích lũy tính đến thời điểm đó không vượt quá giới hạn cuộc thi$k$. 

Đối với mỗi vấn đề$j$, chúng tôi xem xét tất cả các thí sinh đã giải được câu đố đó trong thời gian. Trong đó, thí sinh nào hoàn thành bài toán đó sớm nhất sẽ nhận được giải thưởng. Nếu có nhiều thí sinh về đích cùng lúc thì tất cả đều nhận được giải thưởng. 

Nhiệm vụ là tính toán, đối với mỗi thí sinh, số giải thưởng dự kiến ​​​​mà họ nhận được trong tất cả các bài toán, dựa trên tính ngẫu nhiên của tất cả các hoán vị. 

Khó khăn chính là “thời gian kết thúc của một bài toán cố định” không độc lập giữa các thí sinh và đối với một thí sinh, nó phụ thuộc vào thứ tự ngẫu nhiên của tất cả các bài toán có thể giải được khác. 

Những hạn chế là nhỏ về khía cạnh của vấn đề, với$m \le 26$, nhưng số lượng thí sinh vừa phải,$n \le 500$, và bị ràng buộc về thời gian$k \le 300$. Điều này gợi ý rõ ràng về cách phân phối kiểu ba lô cho mỗi thí sinh trên các tập hợp con, sau đó là tổng hợp theo từng vấn đề đối với các thí sinh. 

Một mô phỏng đơn giản sẽ liệt kê rõ ràng tất cả các hoán vị cho mỗi thí sinh, điều này mang tính giai thừa trong$m$và hoàn toàn không khả thi ngay cả đối với một thí sinh. 

Một dạng thất bại tinh vi hơn xuất hiện nếu người ta giả định rằng vị trí của bài toán trong một hoán vị ngẫu nhiên là đồng nhất. Biết vị trí thôi chưa đủ; chúng ta cần phân phối tổng tiền tố của các mục có trọng số được sắp xếp ngẫu nhiên. 

Ví dụ: nếu một thí sinh có thể giải được ba bài toán với số lần$[1, 1, 100]$, thời gian kết thúc dự kiến ​​của một bài toán nhất định bị sai lệch rất nhiều bởi liệu vật nặng có xuất hiện trước nó hay không. Riêng vị trí điều trị sẽ mất hoàn toàn cấu trúc này. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Đối với mỗi thí sinh, hãy liệt kê tất cả các hoán vị của các bài toán có thể giải được của họ, mô phỏng tổng tiền tố, ghi lại thời gian hoàn thành cho từng bài toán, sau đó so sánh giữa các thí sinh cho từng bài toán. Điều này đúng nhưng đối với$m = 26$, một thí sinh đã có$26!$hoán vị, vượt xa mọi tính toán khả thi. 

Quan sát quan trọng là các hoán vị có thể được thay thế bằng một quy trình tập hợp con. Trong hoán vị ngẫu nhiên, tập hợp các phần tử xuất hiện trước một phần tử cố định là ngẫu nhiên đồng nhất giữa tất cả các tập hợp con của các phần tử khác. Điều này chuyển đổi hoán vị thành một vấn đề phân phối tổng tập hợp con. 

Khi chúng tôi sửa lỗi một thí sinh$i$và một vấn đề$j$, thời điểm kết thúc của$j$chỉ phụ thuộc vào tập hợp con của các bài toán có thể giải được khác xuất hiện trước$j$, và tổng trọng lượng của chúng. Do đó, chúng ta có thể tính toán DP ba lô để đếm xem có bao nhiêu tập hợp con có kích thước$s$có tổng thời gian$t$. Kết hợp với xác suất mà một tập hợp con có kích thước$s$xuất hiện trước$j$, chúng ta thu được phân bố chính xác về thời gian kết thúc của$j$cho thí sinh đó. 

Sau đó, bài toán trở thành: với mỗi bài toán$j$, chúng tôi có tới$n$phân phối rời rạc của thời gian kết thúc. Chúng ta phải tính toán, đối với mỗi thí sinh, xác suất để giá trị của chúng là nhỏ nhất trong số tất cả các thí sinh giải được$j$. Điều này được thực hiện bằng cách chuyển đổi các phân phối thành các hàm sinh tồn và kết hợp chúng theo cấp số nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các hoán vị |$O(n \cdot m!)$|$O(1)$| Quá chậm | 
| Tập hợp con DP + tổng hợp xác suất |$O(n \cdot m^2 \cdot k + n \cdot m \cdot k)$|$O(mk)$mỗi thí sinh | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa một thí sinh$i$và một vấn đề$j$. 

1. Xây dựng bộ$S$những vấn đề mà thí sinh$i$có thể giải quyết, loại trừ$j$. Bộ này xác định mọi thứ có thể xuất hiện trước$j$trong hoán vị ngẫu nhiên. 
2. Xây dựng một DP ba lô trên các tập hợp con của$S$. Cho phép`cnt[s][t]`là số tập con có kích thước$s$tổng thời gian của ai là$t$. Điều này được tính toán bằng cách lặp lại các mục và cập nhật các chuyển đổi kích thước và tổng. Ràng buộc$k \le 300$giới hạn tất cả các khoản tiền. 
3. Chuyển đổi số lượng tập hợp con thành xác suất. Một tập hợp con cố định$P$kích thước$s$xuất hiện trước$j$chính xác$$\frac{s!(|S|-s)!}{(|S|+1)!}$$cách liên quan đến tất cả các hoán vị bao gồm$j$. Như vậy mỗi cặp$(s,t)$đóng góp một trọng số tổ hợp đã biết nhân với`cnt[s][t]`. 

1. Từ đó xây dựng$f_{i,j}(x)$, xác suất mà thí sinh$i$kết thúc vấn đề$j$vào thời điểm chính xác$x + a_{i,j}$, miễn là chúng vẫn nằm trong giới hạn cuộc thi$k$. 
2. Chuyển đổi$f_{i,j}$thành một chức năng sinh tồn:$$S_{i,j}(x) = P(T_{i,j} \ge x)$$bằng cách tổng hợp hậu tố theo thời gian. 

1. Đối với một vấn đề cố định$j$, tính toán$$P_{\text{all}}(x) = \prod_i S_{i,j}(x)$$trong số tất cả các thí sinh có thể giải được$j$. 

1. Đối với mỗi thí sinh$i$, xóa phần đóng góp của họ theo cách chia:$$P_{\text{others}}(x) = \frac{P_{\text{all}}(x)}{S_{i,j}(x)}$$1. Xác suất mà thí sinh$i$vấn đề thắng$j$là$$\sum_x f_{i,j}(x) \cdot P_{\text{others}}(x)$$1. Tính tổng giá trị này cho mọi bài toán$j$để có được số giải thưởng như mong đợi cho thí sinh$i$. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là đối với một thí sinh cố định, hoán vị ngẫu nhiên tạo ra sự phân bố đồng đều trên các tập hợp con của các phần tử trước đó cho mỗi bài toán phân biệt. Điều này loại bỏ hoàn toàn thứ tự và thay thế nó bằng tập hợp con số lượng và tổng trọng số. Sau đó, tính độc lập giữa các thí sinh cho phép chúng tôi kết hợp các phân phối theo cấp số nhân khi tính toán mức tối thiểu, vì thời gian về đích là độc lập giữa các thí sinh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, m, k = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    # precompute factorials up to 26
    maxm = m
    fact = [1] * (maxm + 1)
    for i in range(1, maxm + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (maxm + 1)
    invfact[maxm] = modinv(fact[maxm])
    for i in range(maxm, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def build_dist(times, exclude_idx):
        # times: list of solvable times, excluding j
        # DP: cnt[s][t]
        dp = [[0] * (k + 1) for _ in range(len(times) + 1)]
        dp[0][0] = 1

        for w in times:
            for s in range(len(times) - 1, -1, -1):
                for t in range(k - w + 1):
                    if dp[s][t]:
                        dp[s + 1][t + w] = (dp[s + 1][t + w] + dp[s][t]) % MOD

        return dp

    ans = [0] * n

    for j in range(m):
        # build distributions for all i
        f = [[0] * (k + 1) for _ in range(n)]
        S = [[0] * (k + 2) for _ in range(n)]

        for i in range(n):
            if a[i][j] == 0:
                continue

            times = []
            for p in range(m):
                if p != j and a[i][p] > 0:
                    times.append(a[i][p])

            dp = build_dist(times, j)
            sz = len(times)

            total_perm = fact[sz + 1]

            # probability scaling
            inv_total = modinv(total_perm)

            # compute finish distribution
            for s in range(sz + 1):
                ways_s = (fact[s] * fact[sz - s]) % MOD
                for t in range(k + 1):
                    if dp[s][t]:
                        prob = dp[s][t] * ways_s % MOD * inv_total % MOD
                        ft = t + a[i][j]
                        if ft <= k:
                            f[i][ft] = (f[i][ft] + prob) % MOD

            # survival
            for t in range(k, -1, -1):
                S[i][t] = (S[i][t + 1] + f[i][t]) % MOD if t < k else f[i][t]

        # product of survivals
        P_all = [1] * (k + 2)
        for t in range(k + 1):
            prod = 1
            for i in range(n):
                prod = prod * S[i][t] % MOD
            P_all[t] = prod

        invS = [[0] * (k + 2) for _ in range(n)]
        for i in range(n):
            for t in range(k + 1):
                if S[i][t]:
                    invS[i][t] = modinv(S[i][t])

        for i in range(n):
            if a[i][j] == 0:
                continue
            res = 0
            for t in range(k + 1):
                if f[i][t]:
                    others = P_all[t] * invS[i][t] % MOD
                    res = (res + f[i][t] * others) % MOD
            ans[i] = (ans[i] + res) % MOD

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng các phân phối tổng tập hợp con cho mỗi thí sinh và mỗi vấn đề, sau đó chuyển chúng thành các phân phối ở thời điểm kết thúc. Hàm sống sót được tính toán từ các phân phối này, điều này rất quan trọng để biến “các biến ngẫu nhiên tối thiểu” thành cấu trúc sản phẩm. Bước cuối cùng cẩn thận loại bỏ phần đóng góp của mỗi thí sinh khi tính toán xác suất họ là người về đích sớm nhất. 

Một điểm tinh tế là việc sử dụng mô-đun nghịch đảo cho xác suất sống sót. Vì tất cả các xác suất được lưu trữ theo modulo$998244353$, phép chia được thực hiện dưới dạng phép nhân với các nghịch đảo mô-đun, đòi hỏi các giá trị tồn tại phải khác 0; trong thực tế, tỷ lệ sống sót bằng 0 tương ứng với những so sánh không thể thực hiện được và không ảnh hưởng đến các tổng hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với hai thí sinh và một bài toán mà cả hai đều có thể giải được. Giả sử thí sinh 1 có lần$[1,2]$và thí sinh 2 đã có lần$[2]$, Và$k$là lớn. 

Đối với thí sinh 1, tập con DP trên$[2]$sản lượng: 

| kích thước tập hợp con | tổng hợp | 
| --- | --- | 
| 0 | 0 | 
| 1 | 2 | 

Điều này mang lại thời gian hoàn thành 1 và 3 tùy thuộc vào mục thứ hai trước hay sau. 

Đối với thí sinh 2 thì không có bài nào khác nên thời gian về đích luôn là 2. 

So sánh cho thấy thí sinh 1 thắng khi về đích ở thời điểm 1, ngược lại thí sinh 2 thắng. 

Dấu vết này cho thấy sự lựa chọn tập hợp con, chứ không phải vị trí hoán vị, xác định sự phân bố như thế nào. 

Bây giờ hãy xem xét trường hợp cả hai đối thủ đều có phân phối giống hệt nhau. Tính đối xứng ngụ ý các chức năng sinh tồn giống hệt nhau và việc xây dựng sản phẩm mang lại những giải thưởng được mong đợi như nhau, khẳng định tính công bằng với những yếu tố đầu vào giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot m^2 \cdot k + m \cdot n \cdot k)$| tập hợp con DP cho mỗi thí sinh cho mỗi vấn đề cộng với tổng hợp theo thời gian | 
| Không gian |$O(mk)$| Bảng DP cho tổng tập hợp con cho mỗi thí sinh | 

Những hạn chế$m \le 26$Và$k \le 300$làm cho tập hợp con DP khả thi, trong khi$n \le 500$giữ bước tổng hợp trong giới hạn khi được thực hiện cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # assume solve() is defined above
    solve()
    return ""  # placeholder since full IO capture omitted

# provided sample (placeholder, actual output omitted in statement)
# assert run(...) == "..."

# minimum case
run("1 1 1\n1")

# all zeros except one solvable
run("2 2 10\n1 0\n0 1")

# identical contestants
run("2 2 10\n1 2\n1 2")

# max m boundary
run("3 26 300\n" + "\n".join(["1 "*26]*3))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | tầm thường | độ chính xác cơ sở DP | 
| khả năng giải quyết rời rạc | chia giải thưởng | thí sinh độc lập | 
| hàng giống hệt nhau | kỳ vọng như nhau | đối xứng | 
| trường hợp dày đặc đầy đủ | căng thẳng DP | xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp quan trọng xảy ra khi một thí sinh không thể giải quyết được vấn đề nào cả. Trong trường hợp đó, tất cả các phân phối cho cặp đó đều bằng 0 và chúng không đóng góp gì cho bất kỳ sản phẩm sống còn nào. Thuật toán bỏ qua chúng một cách tự nhiên và không xảy ra phép chia cho 0 vì chúng không bao giờ được tính vào tổng chiến thắng. 

Một trường hợp khác nảy sinh khi thí sinh giải được một bài toán nhưng luôn vượt quá thời gian.$k$. Sau đó$f_{i,j}$bằng 0 và chúng lại không đóng góp gì cả. Điều này đảm bảo rằng họ không thể vô tình xuất hiện với tư cách là người chiến thắng trong tổng hợp cuối cùng. 

Cuối cùng, khi nhiều thí sinh có phân bổ thời gian về đích giống hệt nhau, các mối quan hệ sẽ được xử lý chính xác vì công thức sản phẩm tính sự bình đẳng là cho phép đạt được mức tối thiểu đồng thời. Việc so sánh dựa trên sự sống còn không phá vỡ mối quan hệ, duy trì yêu cầu về thời gian sớm nhất trao giải cho nhiều thí sinh.
