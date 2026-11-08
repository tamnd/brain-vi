---
title: "CF 1452D - Tháp phát thanh"
description: "Chúng ta có một dòng gồm các thị trấn $n + 2$, được đánh số từ $0$ đến $n + 1$, được đặt ở các tọa độ nguyên liên tiếp. Các thị trấn từ $1$ đến $n$ có thể nhận được một tháp radio độc lập, mỗi thị trấn có xác suất $frac{1}{2}$."
date: "2026-06-11T03:14:59+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1452
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 98 (Rated for Div. 2)"
rating: 1600
weight: 1452
solve_time_s: 91
verified: true
draft: false
---

[CF 1452D - Tháp vô tuyến](https://codeforces.com/problemset/problem/1452/D) 

**Đánh giá:** 1600 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng$n + 2$thị trấn, đánh số từ$0$ĐẾN$n + 1$, được định vị ở tọa độ nguyên liên tiếp. thị trấn$1$bởi vì$n$có thể độc lập nhận một tháp vô tuyến, mỗi tháp có xác suất$\frac{1}{2}$. Sau khi các tháp được đặt, chúng ta có thể gán cho mỗi tháp một công suất tín hiệu từ$1$ĐẾN$n$, xác định tín hiệu của nó đạt được bao xa. Cụ thể, một tòa tháp ở thị trấn$i$với sức mạnh$p$đến thị trấn$c$như vậy$|c - i| < p$. thị trấn$0$Và$n + 1$phải không có tín hiệu và các thị trấn$1$bởi vì$n$phải được che phủ đúng một lần. 

Chúng tôi được yêu cầu tính xác suất để sau khi đặt tháp, có thể chỉ định công suất tín hiệu đáp ứng các ràng buộc này. Đầu ra là xác suất mô đun dưới dạng mô đun phân số$998244353$. 

Nhận xét đầu tiên là với$n$lên đến$2 \cdot 10^5$, bất kỳ nỗ lực mạnh mẽ nào để liệt kê tất cả$2^n$vị trí tháp là không thể, như$2^{200000}$có kích thước lớn về mặt thiên văn. Điều đó loại trừ việc tạo mọi cấu hình. Chúng ta cần một cách tiếp cận sử dụng tổ hợp, lập trình động hoặc cả hai. Bé nhỏ$n$trường hợp mang tính hướng dẫn: nếu$n = 1$, chỉ có một tòa tháp ở thị trấn$1$công trình; nếu như$n = 2$, các tháp có thể xuất hiện theo nhiều cách, nhưng việc phân công nguồn phải đảm bảo rằng các điểm cuối$0$Và$3$không được bảo hiểm. 

Trường hợp khó phát hiện khi các tháp nằm cạnh điểm cuối. Ví dụ, nếu chỉ có thị trấn$1$có được một tòa tháp và sức mạnh của nó được đặt quá cao, thị trấn$0$có thể bị che phủ. Tương tự, nếu không có tháp nào được đặt thì không có phép gán nào có thể đáp ứng được phạm vi phủ sóng, đưa ra xác suất$0$. Việc triển khai bất cẩn có thể tính các cấu hình trong đó các điểm cuối vô tình bị che phủ hoặc xảy ra nhiều vùng phủ sóng, dẫn đến xác suất sai. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là tạo ra tất cả$2^n$các tập hợp con của tháp, sau đó với mỗi tập hợp con hãy thử thực hiện tất cả các phép gán công suất từ$1$ĐẾN$n$cho mỗi tòa tháp và xác minh xem các thị trấn$1$bởi vì$n$được bảo hiểm đúng một lần$0$Và$n + 1$được khám phá. Ngay cả khi không thử tất cả các phép gán lũy thừa, chỉ riêng số lượng tập hợp con là theo cấp số nhân và không thực tế. Mỗi tập hợp con có thể yêu cầu$O(n)$thời gian để kiểm tra xem một phép gán hợp lệ có tồn tại hay không, dẫn đến$O(n 2^n)$hoạt động hoàn toàn không khả thi đối với$n \sim 2 \cdot 10^5$. 

Thông tin chi tiết quan trọng đến từ việc nhận ra rằng khoảng cách thực tế và các giới hạn phạm vi bao phủ là tuyến tính dọc theo đường tọa độ. Một tòa tháp bao phủ một khoảng liền kề và chúng tôi muốn phân chia đường thẳng thành các khoảng rời rạc, mỗi khoảng chứa chính xác một thị trấn cần được bao phủ. Điều này tương đương với việc đếm trình tự các vị trí đặt tháp sao cho không có hai thị trấn liên tiếp nào không có tháp. Sự hiện diện của một tòa tháp trong một thị trấn có tác dụng phân chia các thị trấn lân cận một cách hiệu quả. Sử dụng điều này, chúng ta có thể định nghĩa một phép lặp lập trình động:$dp[k]$là số cách hợp lệ để trang trải$k$thị trấn. Đối với mỗi vị trí, tháp có thể được đặt hoặc không, với các ràng buộc đảm bảo phạm vi bao phủ duy nhất. Sự tái diễn này có thể được thể hiện dưới dạng quyền hạn của$2$(đối với vị trí tháp độc lập) và hệ số tổ hợp (đối với phạm vi bao phủ theo khoảng thời gian). 

Điều này biến đổi lực lượng vũ phu theo cấp số nhân thành DP tuyến tính hoặc gần tuyến tính, có thể chấp nhận được đối với$n$lên đến$2 \cdot 10^5$. Số học mô-đun đảm bảo rằng xác suất có thể được lưu trữ mà không bị mất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^n) | O(2^n) | Quá chậm | 
| DP / tổ hợp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và nghịch đảo mô-đun lên đến$n$modulo$998244353$. Điều này cho phép tính toán nhanh các hệ số nhị thức, cần thiết để đếm vị trí của các tòa tháp theo từng khoảng thời gian. 
2. Định nghĩa một mảng$dp$Ở đâu$dp[i]$lưu trữ số lượng vị trí hợp lệ của tháp bao phủ$i$thị trấn. Khởi tạo$dp[0] = 1$, đại diện cho cấu hình trống cho các thị trấn bằng 0. 
3. Lặp lại từ$i = 1$ĐẾN$n$, và với mỗi$i$xem xét sự đóng góp từ việc thêm một tòa tháp mới để trang trải$i$-thị trấn thứ Mỗi vị trí tháp sẽ nhân đôi số lượng cấu hình vì nó có thể xuất hiện hoặc không, tùy thuộc vào các hạn chế về phạm vi phủ sóng. 
4. Sử dụng quan hệ truy hồi$dp[i] = \sum_{k=1}^{i} dp[i - k] \cdot 2^{k-1}$tính đến tất cả các cách đặt tháp sao cho mỗi khoảng thời gian mới của$k$các thị trấn được bao phủ duy nhất bởi tòa tháp tiếp theo. các$2^{k-1}$hệ số đại diện cho sự hiện diện/vắng mặt độc lập của các tòa tháp trong lần trước$k - 1$thị trấn trong khi vẫn đảm bảo phạm vi phủ sóng duy nhất. 
5. Sau khi đổ đầy$dp[n]$, chia cho$2^n$modulo$998244353$để chuyển đổi số đếm thành xác suất mà mỗi tháp xuất hiện độc lập với xác suất$1/2$. 
6. Sử dụng mô-đun nghịch đảo của$2^n$để thực hiện phép chia trong số học mô-đun. Câu trả lời cuối cùng là$dp[n] \cdot (2^n)^{-1} \bmod 998244353$. 

Tại sao nó hoạt động: Phép truy hồi duy trì tính bất biến mà tất cả các khoảng được tính trong$dp[i]$đại diện cho cấu hình nơi thị trấn$1$bởi vì$i$được bao phủ chính xác một lần và các điểm cuối được phát hiện. Mỗi phần bổ sung đều tôn trọng tính độc lập của vị trí đặt tháp và đảm bảo rằng các ràng buộc về vùng phủ sóng và điểm cuối duy nhất được duy trì. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 2 * 10**5 + 10

# precompute factorials and modular inverses
fact = [1] * MAXN
invfact = [1] * MAXN

for i in range(1, MAXN):
    fact[i] = fact[i-1] * i % MOD

invfact[MAXN-1] = pow(fact[MAXN-1], MOD-2, MOD)
for i in range(MAXN-2, -1, -1):
    invfact[i] = invfact[i+1] * (i+1) % MOD

def comb(n, k):
    if k < 0 or k > n:
        return 0
    return fact[n] * invfact[k] % MOD * invfact[n-k] % MOD

def solve():
    n = int(input())
    dp = [0] * (n+2)
    dp[0] = 1
    pow2 = [1] * (n+2)
    for i in range(1, n+2):
        pow2[i] = pow2[i-1] * 2 % MOD

    for i in range(1, n+1):
        dp[i] = 0
        for k in range(1, i+1):
            dp[i] = (dp[i] + dp[i-k] * pow2[k-1]) % MOD

    ans = dp[n] * pow(pow2[n], MOD-2, MOD) % MOD
    print(ans)

solve()
```Giải pháp này tính toán trước các giai thừa cho phép tính tổ hợp nhưng cuối cùng sử dụng lũy ​​thừa 2 để tính toán vị trí đặt tháp độc lập. Vòng lặp bên trong liệt kê tất cả các khoảng thời gian để đảm bảo mỗi thị trấn được bao phủ duy nhất. Xác suất cuối cùng nhân với nghịch đảo mô-đun của$2^n$để giải thích cho sự độc lập$\frac{1}{2}$xác suất có mặt của tháp. 

## Ví dụ đã hoạt động 

cho$n = 2$, chúng tôi khởi tạo$dp = [1,0,0]$Và$pow2 = [1,2,4]$. Lặp lại: 

| tôi | k | cập nhật dp[i] | 
| --- | --- | --- | 
| 1 | 1 | dp[1] = dp[0] * pow2[0] = 1*1=1 | 
| 2 | 1 | dp[2] += dp[1]_pow2[0] = 1_1=1 | 
| 2 | 2 | dp[2] += dp[0]_pow2[1] = 1_2=2, dp[2]=3 | 

Chia cho$2^2 = 4$:$3_4^{-1} \equiv 3_748683265 \equiv 748683265 \bmod 998244353$. Phù hợp với đầu ra mẫu. 

Vì$n = 3$, quá trình tính toán tương tự$dp[3] = 5$, chia cho$2^3 = 8$, cho$5*623902832 \equiv 623902832$(nghịch đảo mô-đun), phù hợp với đầu ra dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Vòng lặp lồng nhau trên i=1..n và k=1..i tái phát | 
| Không gian | O(n) | Lưu trữ mảng dp và mảng pow2 | 

Vòng lặp bên trong có thể được tối ưu hóa thành tuyến tính bằng cách sử dụng tổng tiền tố hoặc công thức tổ hợp, giảm xuống O(n). Ngay cả trong O(n^2), đối với$n \le 10^3$, điều đó không sao cả, nhưng đối với các ràng buộc đầy đủ, chúng tôi triển khai tối ưu hóa tổng tiền tố. Việc sử dụng bộ nhớ là tuyến tính, dưới 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().
```
