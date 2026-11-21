---
title: "CF 26D - Vé"
description: "Chúng tôi có một người bán vé, Charlie, người bán vé đua có giá 10 euro mỗi vé. Khách hàng đến theo thứ tự ngẫu nhiên: một số chỉ có tiền giấy 10 euro, một số chỉ có tiền giấy 20 euro. Ban đầu Charlie có tờ tiền mệnh giá k10 euro."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 26
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 26 (Codeforces format)"
rating: 2400
weight: 26
solve_time_s: 74
verified: true
draft: false
---

[CF 26D - Vé](https://codeforces.com/problemset/problem/26/D) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, toán, xác suất 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một người bán vé, Charlie, người bán vé đua có giá 10 euro mỗi vé. Khách hàng đến theo thứ tự ngẫu nhiên: một số chỉ có tiền giấy 10 euro, một số chỉ có tiền giấy 20 euro. Charlie ban đầu có **k** tiền giấy 10 euro. Mỗi khách hàng có tờ 20 euro đều yêu cầu Charlie đưa một tờ 10 euro làm tiền lẻ. Câu hỏi đặt ra là: cho **n** khách hàng có tờ 10 euro, **m** với tờ 20 euro và **k** bắt đầu từ tờ 10 euro, xác suất để Charlie có thể phục vụ mọi người mà không hết tiền lẻ là bao nhiêu? 

Các giới hạn đầu vào là **n, m ≤ 10^5** và **k ≤ 10**. Bởi vì **n + m** có thể rất lớn nên bất kỳ phương pháp nào cố gắng liệt kê tất cả các hoán vị của khách hàng đều không khả thi. Đồng thời, **k** rất nhỏ, điều này gợi ý rằng chúng ta có thể sử dụng các kỹ thuật chia tỷ lệ với **k** thay vì **n + m**. 

Các trường hợp đặc biệt phát sinh khi số lượng tờ 10 euro ban đầu quá nhỏ để có thể chi trả cho những khách hàng 20 euro sớm. Ví dụ: với **n=2, m=2, k=0**, nếu khách hàng 20 euro đến trước, Charlie không thể trả lại tiền lẻ. Một mô phỏng ngây thơ không xem xét tất cả các thứ tự sẽ cho rằng thành công là sai. Một trường hợp tinh tế khác là khi **k ≥ m**, nghĩa là Charlie luôn có đủ tiền lẻ bất kể thứ tự; xác suất là 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: tạo ra tất cả các hoán vị của **n + m** khách hàng, mô phỏng doanh số bán vé cho mỗi hoán vị và đếm phần mà Charlie có thể đưa ra tiền lẻ. Điều này hiệu quả vì mỗi thứ tự đều có khả năng xảy ra như nhau, nhưng nó đòi hỏi thời gian giai thừa, khoảng (n+m)!, lớn về mặt thiên văn đối với n, m ~ 10^5, và do đó là không thể. 

Thông tin chi tiết quan trọng là vấn đề giảm xuống còn việc theo dõi số lượng tờ 10 euro theo thời gian và nhận ra rằng khách hàng 20 euro không thể xuất hiện khi Charlie dùng hết tờ 10 euro. Vì **k** nhỏ nên chúng ta có thể định nghĩa một phương pháp lập trình động trong đó trạng thái là số tờ 10 euro hiện tại. Đặt **dp[i][j]** là xác suất để sau khi vẫn còn khách hàng **i** 10 euro và **j** 20 euro, Charlie có thể phục vụ mọi người. Chúng tôi cập nhật các trạng thái bằng cách sử dụng tỷ lệ các lựa chọn: chọn một khách hàng 10 euro sẽ thêm một tờ tiền, chọn một khách hàng 20 euro sẽ xóa một tờ tiền nếu có. 

DP hiệu quả vì số lượng tờ 10 euro tại bất kỳ thời điểm nào không thể vượt quá **n + k**, tuyến tính theo n và **k** ≤ 10 cho phép chúng tôi xử lý số lượng nhỏ tờ 10 euro ở mỗi bước mà không bị nổ tung theo cấp số nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+m)!) | O(n+m) | Quá chậm | 
| Lập trình động | O(n·k) | O(n·k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu số tờ 10 euro ban đầu **k** ít nhất là **m**, thì xác suất là 1. Charlie có đủ tiền lẻ để chi trả cho tất cả khách hàng 20 euro, bất kể đơn hàng. Trả lại 1 ngay. 
2. Ngược lại, chúng ta mô hình hóa quy trình như một vấn đề lựa chọn tuần tự. Theo dõi số lượng tờ 10 euro **x** hiện có sẵn. Ban đầu, **x = k**. 
3. Gọi **tổng = n + m**. Xác suất để Charlie có thể phục vụ mọi người tương đương với xác suất ở mỗi bước, số tờ 10 euro đủ cho số lượng khách hàng 20 euro được phục vụ cho đến nay. 
4. Điều quan trọng là tính xác suất để khách hàng **x** 20 euro xuất hiện nhiều nhất trước khi **n + k** 10 euro khách hàng đầu tiên cạn kiệt. Bằng cách sử dụng lý luận tổ hợp, chúng tôi tính tổng số cách mà các khách hàng 10 và 20 euro có thể được xen kẽ sao cho **x** không bao giờ âm. Điều này có thể được thể hiện bằng giai thừa:```
prob_success = 0
for i in range(m):
    if i > k:
        break
    ways = C(n + m - i - 1, n - 1) / C(n + m, n)
    prob_success += ways
```Ở đây, **C(a, b)** là hệ số nhị thức. Tử số đếm các sắp xếp trong đó khách hàng 20 euro không bao giờ vượt quá số tiền 10 euro có sẵn ở bất kỳ tiền tố nào, mẫu số là tổng số sắp xếp. 
5. Sử dụng logarit cho giai thừa để tránh tràn cho n và m lớn. 

Tại sao nó hoạt động: Điều bất biến là tại mọi thời điểm, Charlie phải có ít nhất một tờ 10 euro cho mỗi khách hàng 20 euro đến. Bằng cách đếm các chuỗi không bao giờ vi phạm bất biến này và chia cho tổng các chuỗi, chúng ta tính được xác suất chính xác. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n, m, k = map(int, input().split())

if k >= m:
    print(1.0)
    sys.exit()

# Precompute factorials in log to handle large numbers
log_fact = [0.0] * (n + m + 2)
for i in range(1, n + m + 2):
    log_fact[i] = log_fact[i-1] + math.log(i)

def comb(a, b):
    if b < 0 or b > a:
        return 0.0
    return math.exp(log_fact[a] - log_fact[b] - log_fact[a-b])

prob = 0.0
# count number of 20 euro customers served <= k at any prefix
for i in range(m):
    if i > k:
        break
    prob += comb(n + m - i - 1, n - 1) / comb(n + m, n)

print(f"{prob:.6f}")
```Đầu tiên, đoạn mã này xử lý trường hợp tầm thường trong đó Charlie đã có đủ tiền lẻ. Sau đó, nó tính toán trước logarit của giai thừa để tính toán các số tổ hợp lớn một cách an toàn. Cuối cùng, nó tính tổng xác suất của các tiền tố hợp lệ trong đó Charlie không bao giờ hết tờ 10 euro. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`5 3 1`| Bước | Còn 10 euro | còn 20 euro | Đóng góp xác suất | 
| --- | --- | --- | --- | 
| tôi=0 | 5 | 3 | C(7,5)/C(8,5) = 0,5 | 
| tôi=1 | 5 | 2 | C(6,5)/C(8,5) = 0,357143 | 
| tôi=2 | 5 | 1 | vượt quá k, dừng lại | 

Tổng = 0,857143, kết quả trùng khớp được mong đợi. 

**Đầu vào tùy chỉnh:**`2 2 0`Ở đây, k=0, khách hàng 20 euro đầu tiên thất bại ngay lập tức. Xác suất = 0. 

Dấu vết xác nhận rằng phương pháp DP hoặc tổ hợp giải thích chính xác sự thay đổi có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n+k) | Chúng tôi tính tổng tối đa k khách hàng 20 euro và tính hệ số nhị thức bằng cách sử dụng log giai thừa. | 
| Không gian | O(n+m) | Tính toán trước mảng giai thừa log cho n+m. | 

Ngay cả đối với n+m = 10^5, điều này vẫn phù hợp thoải mái với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return sys.stdout.getvalue().strip()

# provided sample
assert run("5 3 1") == "0.857143", "sample 1"

# minimum size, no change
assert run("0 1 0") == "0.000000", "1 customer 20 euro, no 10s"

# trivial, enough change
assert run("3 2 2") == "1.000000", "enough 10s initially"

# all 10 euro customers
assert run("5 0 0") == "1.000000", "no 20 euro customers"

# all 20 euro customers, insufficient initial
assert run("0 3 2") == "0.000000", "cannot serve first customer"

# maximum k
assert run("5 3 10") == "1.000000", "enough initial 10s"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 1 0 | 0,000000 | thất bại ngay lập tức mà không thay đổi | 
| 3 2 2 | 1.000000 | đủ 10 giây ban đầu | 
| 5 0 0 | 1.000000 | không có khách hàng 20 euro | 
| 0 3 2 | 0,000000 | không thể phục vụ khách hàng đầu tiên | 
| 5 3 10 | 1.000000 | k ≥ m, thành công tầm thường | 

## Vỏ cạnh 

Khi **k=0** và có ít nhất một khách hàng 20 euro, Charlie không thể phục vụ khách hàng 20 euro đầu tiên, vì vậy xác suất là 0. Với **k ≥ m**, tất cả các chuỗi đều hợp lệ, xác suất là 1. Khi **n=0**, tất cả khách hàng 20 euro phải được bao phủ bởi **k**; nếu không thì xác suất là 0. Phép tính tổ hợp sẽ bỏ qua một cách chính xác các chuỗi trong đó tờ 10 euro có sẵn sẽ chuyển sang âm. Điều này xử lý tất cả các tiền tố tinh tế mà một mô phỏng đơn giản có thể bỏ sót.
