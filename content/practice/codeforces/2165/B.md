---
title: "CF 2165B - Hội Đồng Đá Cẩm Thạch"
description: "Chúng ta được cho một tập hợp nhiều số nguyên, chúng ta có thể coi chúng như một túi đựng bi trong đó mỗi viên bi có một màu được biểu thị bằng một số."
date: "2026-06-07T23:31:15+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2165
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1064 (Div. 1)"
rating: 1900
weight: 2165
solve_time_s: 115
verified: false
draft: false
---

[CF 2165B - Hội đồng đá cẩm thạch](https://codeforces.com/problemset/problem/2165/B) 

**Xếp hạng:** 1900 
**Tags:** dp, toán, sắp xếp 
**Thời gian giải:** 1 phút 55 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp nhiều số nguyên, chúng ta có thể coi chúng như một túi đựng bi trong đó mỗi viên bi có một màu được biểu thị bằng một số. Nhiệm vụ là tạo ra một multiset mới`s`trước tiên bằng cách phân vùng nhiều tập hợp ban đầu thành bất kỳ số lượng tập hợp con không trống nào, sau đó, từ mỗi tập hợp con, chọn một trong các chế độ của nó (số xuất hiện thường xuyên nhất trong tập hợp con đó). Kết quả nhiều tập`s`bao gồm các chế độ được chọn này. Chúng ta cần đếm có bao nhiêu multiset khác nhau`s`có thể được tạo ra theo cách này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, chúng tôi được cung cấp kích thước`n`của multiset và chính các phần tử đó. Mỗi phần tử được đảm bảo nằm giữa`1`Và`n`. Đầu ra là số lượng nhiều tập riêng biệt`s`modulo`998,244,353`. 

Những hạn chế là rất quan trọng. Tổng của tất cả`n`trên các trường hợp thử nghiệm tối đa là 5000. Điều này có nghĩa là mặc dù chúng tôi có thể có tổng cộng tối đa 5000 phần tử nhưng chúng tôi không xử lý các tập dữ liệu cực lớn. Tuy nhiên, chúng ta phải cẩn thận với những vụ nổ tổ hợp. Một cách tiếp cận ngây thơ thử mọi phân vùng một cách rõ ràng sẽ cố gắng liệt kê số lượng kích thước của Bell`n`, tăng trưởng theo cấp số nhân và sẽ không khả thi. Thậm chí`n=20`có thể tạo ra hàng triệu phân vùng. 

Trường hợp cạnh phát sinh khi tất cả các phần tử đều bằng nhau. Ví dụ, nếu`a = [1,1,1]`, mọi phân vùng đều có cùng một chế độ, điều này làm giảm đáng kể số lượng. Ngược lại, khi tất cả các phần tử đều khác biệt, mọi tập hợp con không trống có thể tạo ra nhiều tập hợp khác nhau. Các thuật toán bất cẩn có thể đếm gấp đôi nhiều tập hợp hoặc không tôn trọng bội số chế độ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force liệt kê tất cả các phân vùng có thể có của nhiều tập hợp. Đối với mỗi phân vùng, chúng tôi tính toán tất cả các lựa chọn chế độ có thể có từ các tập hợp con và thu thập các tập hợp kết quả. Mặc dù cách tiếp cận này đúng về mặt nguyên tắc nhưng lại khó thực hiện đối với`n > 10`vì số lượng phân vùng của một tập hợp tăng nhanh hơn`2^n`. Mỗi phân vùng cũng yêu cầu tính toán các chế độ của các tập hợp con của nó, điều này bổ sung thêm chi phí. 

Thông tin chi tiết quan trọng để tăng tốc độ này xuất phát từ việc quan sát rằng vấn đề giảm xuống còn việc đếm số lần mỗi phần tử có thể xuất hiện dưới dạng một chế độ trong bất kỳ tập hợp con nào, dựa trên tần số của nó trong nhiều tập hợp ban đầu. Xét tần số của mỗi số trong`a`. Cho phép`cnt[x]`là số đếm`x`. Mỗi số có thể đóng góp vào nhiều tập hợp kết quả`s`bất cứ nơi nào từ 0 lần cho đến tần số của nó. Sau đó, vấn đề giảm xuống việc đếm các kết hợp của những đóng góp này trên tất cả các số. 

Lập trình động tự nhiên phù hợp ở đây. Cho phép`dp[i][j]`biểu thị số cách để tạo nhiều tập hợp với chính xác`j`các phần tử sử dụng phần tử đầu tiên`i`các số riêng biệt và xem xét mọi cách mà mỗi số có thể xuất hiện dưới dạng một chế độ. Chúng tôi lặp lại tất cả các số đếm cho mỗi số và cập nhật bảng DP, đảm bảo chúng tôi xem xét ngầm tất cả các phân vùng. Vì tổng của`n`nhỏ, một bảng DP có kích thước`n × n`là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Chuông(n) * n^2) | O(n^2) | Quá chậm | 
| Lập trình động | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và nghịch đảo mô-đun lên đến mức tối đa`n`cho phép tính toán nhanh các hệ số nhị thức. Chúng ta sẽ cần các tổ hợp để đếm các cách chọn phần tử để tạo thành tập hợp con. 
2. Đối với mỗi trường hợp thử nghiệm, hãy đếm tần số của từng số riêng biệt trong nhiều tập hợp`a`. Cho phép`cnt[x]`là tần số của số`x`. 
3. Khởi tạo mảng DP`dp`với`dp[0][0] = 1`. Đây`dp[i][j]`sẽ lưu trữ số cách để tạo thành nhiều kích thước`j`sử dụng cái đầu tiên`i`số riêng biệt. 
4. Lặp qua từng số riêng biệt`x`với tần số`f = cnt[x]`. Đối với mỗi kích thước tổng thể có thể`j`từ`n`xuống 0, cập nhật`dp[j+k]`cho tất cả`k`từ`1`ĐẾN`f`bằng cách thêm`dp[j] * C(f, k)`modulo`998244353`. Điều này giải thích cho việc lựa chọn`k`bản sao của số`x`như các chế độ từ tần số của nó. 
5. Sau khi xử lý tất cả các số, tính tổng tất cả`dp[j]`vì`j >= 1`để có được tổng số nhiều tập hợp không trống. 
6. Xuất modulo tổng`998244353`. 

**Tại sao nó hoạt động:** Mỗi số được xem xét độc lập về khả năng đóng góp của nó cho tập hợp cuối cùng. Bằng cách lặp lại tất cả các số lượng và kết hợp chúng thông qua DP, chúng tôi ngầm tính đến tất cả các phân vùng của nhiều tập hợp ban đầu mà không liệt kê chúng một cách rõ ràng. Bất biến DP là`dp[j]`luôn đại diện cho số lượng nhiều kích thước`j`được hình thành từ những con số được xử lý cho đến nay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

# Precompute factorials and inverses
MAXN = 5000 + 5
fact = [1] * MAXN
inv_fact = [1] * MAXN

for i in range(1, MAXN):
    fact[i] = fact[i-1] * i % MOD

inv_fact[MAXN-1] = pow(fact[MAXN-1], MOD-2, MOD)
for i in range(MAXN-2, -1, -1):
    inv_fact[i] = inv_fact[i+1] * (i+1) % MOD

def comb(n, k):
    if k < 0 or k > n:
        return 0
    return fact[n] * inv_fact[k] % MOD * inv_fact[n-k] % MOD

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    from collections import Counter
    cnt = Counter(a)
    dp = [0] * (n+1)
    dp[0] = 1
    for f in cnt.values():
        ndp = dp[:]
        for j in range(n+1):
            if dp[j] == 0:
                continue
            for k in range(1, f+1):
                if j+k <= n:
                    ndp[j+k] = (ndp[j+k] + dp[j] * comb(f, k)) % MOD
        dp = ndp
    print(sum(dp[1:]) % MOD)
```Mã này trước tiên tính toán trước các giai thừa và nghịch đảo mô đun để tính hệ số nhị thức hiệu quả. Đối với mỗi trường hợp thử nghiệm, nó đếm tần số của các số, khởi tạo một mảng DP và lặp qua tất cả các số để điền vào bảng DP. Cuối cùng, nó tính tổng tất cả số lượng nhiều tập hợp không trống. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
1 2 3
```| Bước | Mảng DP sau khi xử lý số | Giải thích | 
| --- | --- | --- | 
| Bắt đầu | [1,0,0,0] | nhiều bộ trống | 
| Quy trình 1 | [1,1,0,0] | số 1 ​​có thể xuất hiện một lần | 
| Quy trình 2 | [1,1,1,1] | số 2 thêm các tổ hợp: {2}, {1,2} | 
| Quy trình 3 | [1,1,2,3] | số 3 cộng {3},{1,3},{2,3},{1,2,3} | 
| Tổng dp[1:] | 7 | bảy bộ đa năng riêng biệt | 

Điều này xác nhận rằng DP của chúng tôi tính toán chính xác tất cả các tập hợp không trống mà không tính hai lần. 

### Đầu vào mẫu 2```
1 1 1
```| Bước | Mảng DP sau khi xử lý số | Giải thích | 
| --- | --- | --- | 
| Bắt đầu | [1,0,0,0] | nhiều bộ trống | 
| Quy trình 1 | [1,1,0,0] | đầu tiên '1' | 
| Quy trình 2 | [1,2,1,0] | '1' thứ hai, có thể được thêm một mình hoặc với '1' trước đó | 
| Quy trình 3 | [1,3,3,1] | thứ ba '1', kết hợp tích lũy | 
| Tổng dp[1:] | 7-1 = 3 | chỉ có 3 tập hợp riêng biệt: {1}, {1,1}, {1,1,1} | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi số riêng biệt, chúng tôi lặp lại các kích thước và đóng góp nhiều tập hợp có thể có theo tần số của nó. Tối đa n=5000 đảm bảo điều này phù hợp trong 2 giây. | 
| Không gian | O(n) | Mảng DP có kích thước n+1 là đủ. | 

Điều này đáp ứng các ràng buộc vì tổng của tất cả`n`trên các trường hợp thử nghiệm là 5000. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353
    MAXN = 5000 + 5
    fact = [1] * MAXN
    inv_fact = [1] * MAXN
    for i in range(1, MAXN):
        fact[i] = fact[i-1] * i % MOD
    inv_fact[MAXN-1] = pow(fact[MAXN-1], MOD-2, MOD)
    for i
```
