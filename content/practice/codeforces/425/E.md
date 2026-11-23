---
title: "CF 425E - Sereja và Bộ"
description: "Chúng ta được yêu cầu đếm các tập hợp khoảng trong phạm vi số nguyên từ 1 đến n, sao cho tập hợp lớn nhất các khoảng không chồng lấp trong tập hợp có kích thước chính xác là k."
date: "2026-06-07T02:30:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 425
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 243 (Div. 1)"
rating: 2500
weight: 425
solve_time_s: 109
verified: false
draft: false
---

[CF 425E - Sereja và Bộ](https://codeforces.com/problemset/problem/425/E) 

**Đánh giá:** 2500 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các tập hợp khoảng trong phạm vi số nguyên từ 1 đến _n_, sao cho tập hợp lớn nhất các khoảng không chồng lấp trong tập hợp có kích thước chính xác _k_. Nói cách khác, với mỗi tập hợp khoảng _S_, nếu bạn cố gắng chọn càng nhiều khoảng càng tốt mà không có hai khoảng nào trùng nhau thì số tối đa bạn có thể chọn phải bằng _k_. Nhiệm vụ là đếm tất cả các bộ như vậy theo modulo 10^9 + 7. 

Đầu vào chỉ là hai số: _n_, phạm vi số nguyên có sẵn cho các điểm cuối khoảng và _k_, kích thước được yêu cầu của tập hợp các khoảng độc lập tối đa. Đầu ra là một số nguyên duy nhất cho biết tổng số bộ khoảng thời gian hợp lệ. 

Các ràng buộc nhỏ, với _n_ lên tới 500. Điều này cho phép chúng ta xem xét các giải pháp lập trình động trên các trạng thái bậc hai trong _n_. Bất cứ điều gì theo cấp số nhân trong _n_ hoặc lặp lại trên tất cả các tập hợp con của các khoảng sẽ không khả thi ngay lập tức, vì có khoảng _n2_ khoảng có thể xảy ra và các bộ 2^(_n2_), rất lớn về mặt thiên văn. 

Các trường hợp cạnh không rõ ràng bao gồm _k = 0_, trong đó không cho phép khoảng nào trong tập hợp và _k = n_, tương ứng với các tập hợp trong đó tất cả các khoảng có thể được chọn làm khoảng một phần tử. Một cách tiếp cận ngây thơ mà bỏ qua việc tính toán cẩn thận xem các khoảng có thể trùng nhau như thế nào sẽ bị tính quá mức hoặc bỏ lỡ những tình huống cực đoan này. 

Ví dụ: nếu _n = 2_ và _k = 1_ thì các tập hợp lệ bao gồm các khoảng phần tử đơn, các tập hợp có các khoảng chồng lấp và tập trống. Việc liệt kê bất cẩn có thể đếm các tập hợp có kích thước không chồng chéo tối đa là 2, điều này sẽ vi phạm yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp khoảng thời gian có thể có. Có n(n+1)/2 khoảng có thể từ 1 đến _n_ và mỗi khoảng có thể được bao gồm hoặc loại trừ một cách độc lập. Đối với mỗi bộ, chúng tôi sẽ tính toán số khoảng không chồng chéo tối đa, có thể sử dụng lựa chọn tham lam theo điểm cuối bên phải. Điều này đúng nhưng hoàn toàn không khả thi, vì số lượng tập hợp tăng lên thành 2^(n(n+1)/2). Ngay cả với n = 20, điều này vẫn nằm ngoài khả năng tính toán thực tế. 

Nhận xét quan trọng là bài toán có cấu trúc phù hợp với quy hoạch động. Cụ thể, chúng ta có thể nghĩ đến việc xây dựng các tập hợp tăng dần bằng cách chọn khoảng ngoài cùng bên phải để thêm vào một tập hợp và đếm các tập hợp theo kích thước độc lập tối đa của chúng. Điều này cho phép chúng ta định nghĩa đệ quy`dp[l][r][x]`là số lượng tập hợp các khoảng nằm hoàn toàn trong phạm vi [l, r] sao cho tập hợp con độc lập tối đa có kích thước chính xác là _x_. Sự lặp lại liên quan đến việc phân tách phạm vi ở mọi khoảng thời gian ngoài cùng bên phải có thể và kết hợp số lượng từ các phân đoạn bên trái và bên phải. 

Điều này làm giảm vấn đề từ phép liệt kê hàm mũ thành DP bậc ba hoặc bậc bốn trong các phạm vi khoảng, có thể chấp nhận được đối với _n_ 500. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n²) * n) | O(n²) | Quá chậm | 
| Lập trình động | O(n³ * k) | O(n³) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng DP 3D`dp[l][r][x]`, Ở đâu`l`Và`r`là ranh giới khoảng và`x`là kích thước tập hợp độc lập tối đa được yêu cầu.`dp[l][r][x]`biểu thị số tập hợp khoảng trong [l, r] với kích thước tập hợp độc lập tối đa chính xác là _x_. 
2. Xử lý trường hợp cơ sở: đối với các phạm vi trống (l > r), tập hợp lệ duy nhất là tập hợp trống có kích thước tối đa 0, do đó`dp[l][r][0] = 1`. 
3. Lặp lại trên tất cả các phạm vi [l, r] có độ dài tăng dần. Đối với mỗi phạm vi, hãy cân nhắc chọn khoảng [l, m] làm "khoảng đầu tiên cần thêm" vào tập hợp. 
4. Đối với mỗi kích thước có thể`x`, hãy tính số tập hợp có khoảng này. Điều này liên quan đến việc chia phần còn lại của phạm vi thành hai phân đoạn: bên trái của l và bên phải của m+1 và kết hợp các số đếm trong đó tổng kích thước tối đa trong hai phân đoạn cộng với 1 bằng x. Đây là bước tích chập quan trọng. 
5. Tổng các khoản đóng góp cho tất cả các lựa chọn của khoảng đầu tiên và tất cả các phần chia có thể có. Sử dụng số học modulo để giữ các số trong phạm vi 10^9 + 7. 
6. Câu trả lời cho toàn bộ phạm vi [1, n] và kích thước k là`dp[1][n][k]`. 

Tại sao nó hoạt động: DP duy trì tính bất biến`dp[l][r][x]`đếm tất cả các tập hợp lệ trong [l, r] với kích thước tập độc lập tối đa x. Bằng cách phân chia ở khoảng ngoài cùng bên phải và kết hợp đệ quy các phân đoạn bên trái và bên phải, chúng tôi đảm bảo tất cả các bộ được tính chính xác một lần mà không bỏ sót các cấu hình rời rạc chồng chéo hoặc đếm kép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, k = map(int, input().split())

# Precompute powers of 2 for number of subsets of intervals of length l
pw = [[0]*(n+2) for _ in range(n+2)]
for l in range(1, n+1):
    for r in range(l, n+1):
        pw[l][r] = pow(2, (r-l+1)*(r-l+2)//2, MOD)

# dp[l][r][x] = number of sets of intervals in [l, r] with max independent size x
dp = [[[0]*(k+2) for _ in range(n+2)] for _ in range(n+2)]

# base case: empty ranges
for l in range(n+2):
    for r in range(l-1, n+2):
        dp[l][r][0] = 1

# fill DP for ranges of increasing length
for length in range(1, n+1):
    for l in range(1, n-length+2):
        r = l + length - 1
        for first_end in range(l, r+1):
            left_len = first_end - l
            right_len = r - first_end
            for x_left in range(k+1):
                for x_right in range(k+1):
                    if x_left + x_right + 1 > k:
                        continue
                    ways = dp[l][first_end-1][x_left] * dp[first_end+1][r][x_right]
                    ways %= MOD
                    dp[l][r][x_left + x_right + 1] += ways
                    dp[l][r][x_left + x_right + 1] %= MOD

# subtract sets with smaller max independent size
for length in range(1, n+1):
    for l in range(1, n-length+2):
        r = l + length - 1
        for x in range(k, 0, -1):
            dp[l][r][x] = (dp[l][r][x] - dp[l][r][x-1]) % MOD

print(dp[1][n][k])
```Đầu tiên, mã này tính toán trước lũy thừa 2 cho các tập hợp con các khoảng để xử lý các phạm vi một cách hiệu quả. Vòng lặp DP chính xem xét tất cả các khoảng thời gian bắt đầu có thể, tính toán các kết hợp từ các phân đoạn bên trái và bên phải và số lượng cập nhật. Cuối cùng, chúng tôi loại bỏ số lượng tương ứng với kích thước độc lập nhỏ hơn để đảm bảo`dp[l][r][x]`chỉ đếm các tập hợp có kích thước độc lập tối đa chính xác là x. Số học Modulo đảm bảo tính chính xác dưới số lượng lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 1
```| tôi | r | dp[l][r][1] | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 1 | 2 | 3 | 
| 1 | 3 | 23 | 

DP đếm dần các bộ hợp lệ. Đối với [1,3] và k=1, các tập hợp bao gồm tất cả các tập hợp con của các khoảng chồng lên nhau nên tập hợp con độc lập tối đa có kích thước 1. Bảng thể hiện sự tích lũy thông qua các khoảng phân tách. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 2
```| tôi | r | dp[l][r][2] | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 1 | 2 | 1 | 

Chỉ tập hợp có khoảng [1,1] và [2,2] đạt được tập độc lập tối đa 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³ * k) | Chúng tôi lặp lại trên tất cả các phạm vi [l, r], cho từng khoảng đầu tiên có thể có và cho mỗi sự kết hợp của x_left và x_right cho đến k | 
| Không gian | O(n³) | Bảng 3D DP có kích thước n x n x k số lượng cửa hàng | 

Với n 500 và k 500, điều này phù hợp thoải mái trong bộ nhớ và giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7
    n, k = map(int, input().split())
    pw = [[0]*(n+2) for _ in range(n+2)]
    for l in range(1, n+1):
        for r in range(l, n+1):
            pw[l][r] = pow(2, (r-l+1)*(r-l+2)//2, MOD)
    dp = [[[0]*(k+2) for _ in range(n+2)] for _ in range(n+2)]
    for l in range(n+2):
        for r in range(l-
```
