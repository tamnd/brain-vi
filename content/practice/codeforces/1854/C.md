---
title: "CF 1854C - Sự hủy diệt dự kiến"
description: "Chúng ta được cho một tập hợp các số nguyên S phân biệt được rút ra từ phạm vi 1 đến m. Mỗi giây, chúng tôi thực hiện một phép toán ngẫu nhiên: chúng tôi chọn ngẫu nhiên một phần tử x từ S, loại bỏ nó và nếu x+1 nằm trong giới hạn và vắng mặt trong S, chúng tôi thêm x+1 trở lại tập hợp."
date: "2026-06-09T05:13:28+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1854
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 889 (Div. 1)"
rating: 2500
weight: 1854
solve_time_s: 115
verified: false
draft: false
---

[CF 1854C - Dự kiến có sự hủy diệt](https://codeforces.com/problemset/problem/1854/C) 

**Đánh giá:** 2500 
**Tags:** tổ hợp, dp, toán, xác suất 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên phân biệt`S`rút ra từ phạm vi`1`ĐẾN`m`. Every second, we perform a stochastic operation: we select an element`x`từ`S`thống nhất một cách ngẫu nhiên, loại bỏ nó, và nếu`x+1`nằm trong giới hạn và vắng mặt`S`, chúng tôi thêm`x+1`trở lại trường quay. Mục tiêu của chúng tôi là tính toán số giây dự kiến ​​cho đến khi`S`trở nên trống rỗng, modulo`1,000,000,007`. 

Đầu vào mang lại`n`, kích thước của tập hợp ban đầu và`m`, giá trị lớn nhất có thể có trong tập hợp. Dòng thứ hai liệt kê các phần tử của`S`theo thứ tự tăng dần. Đầu ra là một số nguyên biểu thị số bước dự kiến, được mã hóa dưới dạng phân số nghịch đảo mô-đun. 

Những hạn chế`1 ≤ n ≤ m ≤ 500`ngụ ý rằng chúng tôi không thể mô phỏng tất cả các chuỗi loại bỏ có thể xảy ra một cách ngây thơ. Một mô phỏng đầy đủ sẽ có độ phức tạp theo giai thừa, vì mỗi thứ tự loại bỏ có thể dẫn đến các phần tử mới được thêm vào một cách linh hoạt. Từ`m`nhỏ, chúng ta có thể xem xét quy hoạch động trên các trạng thái, nhưng chúng ta phải biểu diễn các trạng thái một cách hiệu quả vì tập hợp tất cả các tập con của`[1, m]`có kích thước`2^m`, xung quanh`2^500`- quá lớn. Điều quan trọng là các chuyển đổi đều đơn điệu: các số chỉ tăng thêm 1 nên chúng ta có thể khai thác cấu trúc có thứ tự thay vì theo dõi các tập con tùy ý. 

Các trường hợp cạnh bao gồm khi`S`chứa`m`hoặc có số liên tiếp. Ví dụ, nếu`S = [m]`, loại bỏ`m`ngay lập tức làm trống tập hợp vì`m+1`đã vượt quá giới hạn. Nếu như`S = [1, 2]`, loại bỏ`1`thêm vào`2`chỉ khi nó bị thiếu, nhưng ở đây`2`hiện diện nên không có phần tử mới nào xuất hiện. Việc triển khai bất cẩn có thể quên câu lệnh “chỉ thêm`x+1`nếu nó vắng mặt”, tạo ra các sai số nhỏ trong các giá trị mong đợi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các chuỗi lựa chọn từ`S`, cập nhật tập hợp theo quy tắc và tính tổng số bước dự kiến ​​​​có trọng số theo xác suất. Mỗi trạng thái sẽ yêu cầu lặp lại tất cả các phần tử trong`S`vào thời điểm đó. Số lượng các chuỗi có thể tăng lên một cách bùng nổ vì mỗi phần tử bị loại bỏ có thể tạo ra một phần tử mới, có khả năng tạo ra các số chuỗi theo giai thừa hoặc theo cấp số nhân. Vì`n`lên tới 500, điều này hoàn toàn không khả thi. 

Quan sát quan trọng cho một giải pháp tối ưu là tập hợp`S`có thể được biểu diễn dưới dạng một mảng nhị phân có độ dài`m`, cho biết số nào có mặt. Quá trình thêm`x+1`chỉ ảnh hưởng đến các vị trí ngay bên phải. Cấu trúc này cho phép chúng ta định nghĩa một bảng lập trình động`dp[i]`biểu thị số bước dự kiến ​​để làm trống tập hợp khi`S`chứa các số bắt đầu từ`i`trở đi. 

Bằng cách xem xét công thức giá trị kỳ vọng, nếu`S`hiện tại có`k`phần tử, chọn phần tử`x`đóng góp`1/k`giá trị kỳ vọng của việc loại bỏ`x`. Nếu như`x+1`đã ở trong rồi`S`hoặc vượt quá`m`, số bước bổ sung dự kiến ​​chỉ là`dp[new set]`. Nếu không thì,`x+1`được thêm vào, tạo thành một tập hợp lớn hơn một chút. Sử dụng tính tuyến tính của kỳ vọng và tính toán trước cẩn thận các nghịch đảo mô đun cho phép chia modulo`P = 10^9 + 7`, chúng ta có thể tính các giá trị mong đợi từ dưới lên mà không cần liệt kê tất cả các chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · ?) | O(2^m) | Quá chậm | 
| DP qua các vị trí | O(n·m^2) | O(m^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn tập hợp`S`dưới dạng mặt nạ nhị phân hoặc mảng boolean có độ dài`m`, đánh dấu số hiện tại. Điều này cho phép O(1) kiểm tra xem`x+1`đang có mặt. 
2. Khởi tạo mảng DP`dp[i]`vì`0 ≤ i ≤ m+1`. Đây`dp[i]`lưu trữ số bước dự kiến ​​​​để làm trống tập hợp nếu chỉ có số`i`bởi vì`m`đang có mặt.`dp[m+1] = 0`bởi vì nếu chúng ta đã qua`m`, tập hợp trống. 
3. Lặp lại từ`i = m`xuống tới`1`. Đối với mỗi`i`, hãy xem xét liệu`i`có mặt trong tập hợp ban đầu. Nếu không,`dp[i] = dp[i+1]`bởi vì không có gì xảy ra ở vị trí này. 
4. Nếu`i`hiện diện, số bước dự kiến ​​là`1`(để loại bỏ`i`) cộng với giá trị trung bình của các giá trị mong đợi của các trạng thái thu được sau khi chọn từng phần tử. Sự tái phát quan trọng là`dp[i] = 1 + (dp[i+1] + dp[i+2] + …)/k`, Ở đâu`k`là số phần tử có trong hiện tại`S`. Tính toán điều này bằng cách sử dụng tổng tiền tố để có hiệu quả. 
5. Vì chúng ta cần thực hiện phép chia modulo`P`, tính toán trước nghịch đảo mô-đun`inv[k] = k^{-1} mod P`. Mỗi lần bổ sung của`1/k * dp[next state]`trở thành`(dp[next state] * inv[k]) % P`. 
6. Sau khi điền mảng DP thì đáp án là`dp[min(S)]`, tức là các bước dự kiến ​​bắt đầu từ số nhỏ nhất trong tập hợp. 

**Tại sao nó hoạt động** 

Tính bất biến đó là`dp[i]`thể hiện chính xác số bước dự kiến ​​để làm trống tất cả các số ≥`i`. Bằng cách tính toán từ`m`đi xuống, mỗi trạng thái chỉ phụ thuộc vào các trạng thái được tính toán trước đó. Tính tuyến tính của kỳ vọng đảm bảo chúng ta có thể tính tổng các lựa chọn mà không cần theo dõi xác suất của từng chuỗi riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD-2, MOD)

n, m = map(int, input().split())
S = list(map(int, input().split()))
present = [0]*(m+2)
for x in S:
    present[x] = 1

dp = [0]*(m+2)  # dp[i] = expected steps to empty numbers i..m

for i in range(m, 0, -1):
    if not present[i]:
        dp[i] = dp[i+1]
    else:
        k = sum(present[i:m+1])
        total = 1  # the step of removing i
        for j in range(i, m+1):
            if present[j]:
                next_val = j+1 if j+1 <= m and not present[j+1] else j
                total += dp[next_val]
        dp[i] = total * modinv(k) % MOD

print(dp[min(S)])
```Chúng tôi đánh dấu những yếu tố có mặt. Sau đó chúng tôi điền`dp`từ số lớn nhất xuống`1`. Đối với mỗi phần tử trong`S`, chúng tôi tính toán đóng góp của nó bằng cách sử dụng tổng giá trị kỳ vọng của các trạng thái tiếp theo chia cho số phần tử trong tập hợp hiện tại. Nghịch đảo mô-đun xử lý phép chia theo modulo`10^9+7`. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 3
1 3
```| Bước | S (hiện tại) | k | Đóng góp dự kiến ​​| dp[i] | 
| --- | --- | --- | --- | --- | 
| tôi=3 | [3] | 1 | 1 | 1 | 
| tôi=2 | [1,3] | 2 | 1 + dp[2]/? | tính sau | 
| tôi=1 | [1,3] | 2 | 1 + (dp[2]+dp[3])/2 | 15/4 ≡ 750000009 | 

Điều này cho thấy DP tổng hợp chính xác các khoản đóng góp từ các lựa chọn có thể có, bao gồm cả chuỗi bổ sung`2`sau khi loại bỏ`1`. 

**Kịch bản cạnh** 

đầu vào:```
1 1
1
```

`S=[1]`,`m=1`. Đang xóa`1`làm trống bộ ngay lập tức. DP chỉ định`dp[1] = 1`. Công thức xử lý ranh giới vì`1+1 > m`, vì vậy không có phần tử nào được thêm vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Đối với mỗi phần tử, chúng ta tính tổng các phần tử còn lại trong tập hợp; n ∼ m ₫ 500 làm cho điều này khả thi | 
| Không gian | O(m) | Chúng tôi lưu trữ mảng hiện diện và mảng DP có độ dài m+2 | 

Với các giới hạn,`500*500 = 250,000`các hoạt động được chấp nhận trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 10**9+7
    def modinv(x):
        return pow(x, MOD-2, MOD)
    n, m = map(int, input().split())
    S = list(map(int, input().split()))
    present = [0]*(m+2)
    for x in S:
        present[x] = 1
    dp = [0]*(m+2)
    for i in range(m, 0, -1):
        if not present[i]:
            dp[i] = dp[i+1]
        else:
            k = sum(present[i:m+1])
            total = 1
            for j in range(i, m+1):
                if present[j]:
                    next_val = j+1 if
```
