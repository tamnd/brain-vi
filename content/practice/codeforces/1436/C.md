---
title: "CF 1436C - Tìm kiếm nhị phân"
description: "Chúng ta được yêu cầu đếm các hoán vị có kích thước n trong đó một số cụ thể x được đặt tại một vị trí chỉ mục cố định và thủ tục tìm kiếm nhị phân tiêu chuẩn, như được mô tả trong bài toán, đã tìm thấy thành công x tại vị trí đó. Hoán vị ở đây là thứ tự bất kỳ của các số từ 1 đến n."
date: "2026-06-11T04:50:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics"]
categories: ["algorithms"]
codeforces_contest: 1436
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 678 (Div. 2)"
rating: 1500
weight: 1436
solve_time_s: 95
verified: true
draft: false
---

[CF 1436C - Tìm kiếm nhị phân](https://codeforces.com/problemset/problem/1436/C) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, tổ hợp 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm hoán vị của kích thước`n`nơi một số cụ thể`x`được đặt ở một chỉ số cố định`pos`và thủ tục tìm kiếm nhị phân tiêu chuẩn, như được mô tả trong bài toán, tìm thấy thành công`x`ở vị trí đó. Hoán vị ở đây là thứ tự bất kỳ của các số`1`bởi vì`n`. Đầu vào chỉ định ba số nguyên:`n`về kích thước của hoán vị,`x`cho số chúng tôi đang tìm kiếm, và`pos`cho chỉ số nơi`x`phải kết thúc. Đầu ra là số lượng hoán vị hợp lệ modulo`10^9+7`. 

Khía cạnh không rõ ràng là tìm kiếm nhị phân mô phỏng một loạt so sánh. Khi tìm kiếm`x`, tìm kiếm duy trì giới hạn trái và phải và liên tục kiểm tra phần tử ở giữa. Mỗi so sánh có hiệu quả chia mảng còn lại thành các số phải nhỏ hơn hoặc lớn hơn`x`dựa trên đường dẫn tìm kiếm. Vì vậy, để hoán vị thành công, các phần tử ở bên trái của`pos`đã được viếng thăm như`mid`và so sánh với`x`tất cả đều phải nhỏ hơn`x`và các phần tử bên phải phải lớn hơn. 

Những hạn chế ở mức vừa phải:`n`có thể lên tới 1000, điều này tạo nên một cách tiếp cận bạo lực đối với tất cả`n!`hoán vị không khả thi, nhưng cho phép các thuật toán có độ phức tạp bậc ba hoặc thậm chí bậc hai. 

Trường hợp cạnh khóa xảy ra khi`x`đang ở chỉ số cực trị (0 hoặc n-1). Tìm kiếm nhị phân sẽ chỉ truy cập các vị trí nhất định và có thể bỏ qua các vị trí khác. Một cách tiếp cận ngây thơ đếm tất cả các hoán vị trong đó`x`đang ở`pos`không mô phỏng đường dẫn tìm kiếm nhị phân sẽ cho kết quả không chính xác. Ví dụ, với`n=3`,`x=2`, Và`pos=0`, chỉ một số vị trí nhất định của các số khác mới cho phép tìm kiếm nhị phân thành công. 

## Phương pháp tiếp cận 

Về mặt khái niệm, cách tiếp cận vũ phu rất đơn giản. Chúng tôi tạo ra tất cả các hoán vị có kích thước`n`, địa điểm`x`Tại`pos`, mô phỏng tìm kiếm nhị phân trên mỗi hoán vị và đếm thành công. Cách tiếp cận này đúng nhưng không khả thi về mặt tính toán vì`n!`tăng trưởng nhanh chóng, đạt 10^256 cho`n=1000`. 

Cách tiếp cận tối ưu đến từ việc phân tích hành vi tìm kiếm nhị phân. Mỗi lần lặp tìm kiếm nhị phân tại chỉ mục`mid`xác định liệu`mid`nhỏ hơn, lớn hơn hoặc bằng`pos`. Nếu như`mid < pos`, phần tử tại`mid`phải nhỏ hơn`x`. Nếu như`mid > pos`, phần tử phải lớn hơn`x`. Vì vậy, chúng ta có thể đếm số phần tử nhỏ hơn`x`phải đi vào các vị trí đã truy cập ở bên trái của`pos`và tương tự cho các phần tử lớn hơn. Các vị trí còn lại có thể được lấp đầy miễn phí. Điều này biến bài toán thành bài toán đếm tổ hợp với giai thừa và tổ hợp. 

Thông tin chi tiết giúp giảm vấn đề từ việc tạo ra tất cả các hoán vị đến cách đếm các cách phân phối số nhỏ hơn và lớn hơn trong khi vẫn tôn trọng đường dẫn tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Đếm tổ hợp | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và giai thừa nghịch đảo modulo 10^9+7 để cho phép tính toán hiệu quả các hoán vị và tổ hợp. 
2. Khởi tạo`left = 0`Và`right = n`. Điều này thể hiện giới hạn tìm kiếm nhị phân. Theo dõi hai quầy:`less_needed`đối với số nhỏ hơn`x`điều đó phải xuất hiện, và`greater_needed`đối với số lớn hơn`x`điều đó phải xuất hiện. 
3. Mô phỏng tìm kiếm nhị phân: while`left < right`, tính toán`mid = (left + right) // 2`. Nếu như`mid <= pos`, di chuyển`left = mid + 1`và tăng dần`less_needed`nếu như`mid != pos`vì chỉ mục đó phải chứa một số nhỏ hơn`x`. Nếu như`mid > pos`, di chuyển`right = mid`và tăng dần`greater_needed`bởi vì chỉ mục đó phải chứa một số lớn hơn`x`. 
4. Sau vòng lặp, chúng ta biết chính xác có bao nhiêu số nhỏ hơn`x`cần chiếm những vị trí nhất định và có bao nhiêu số lớn hơn`x`cần đảm nhiệm các vị trí khác. 
5. Tính số cách gán các số này bằng cách sử dụng giai thừa. Chọn`less_needed`số từ`x-1`số nhỏ hơn và sắp xếp chúng. Chọn`greater_needed`số từ`n-x`số lớn hơn và sắp xếp chúng. Các số còn lại có thể tự do điền vào các vị trí còn lại. 
6. Nhân các số đếm này với nhau và lấy modulo 10^9+7 để có kết quả cuối cùng. 

Tại sao nó hoạt động: đường dẫn tìm kiếm nhị phân áp đặt một ràng buộc thứ tự nghiêm ngặt đối với các phần tử nhỏ hơn và lớn hơn`x`. Bằng cách đếm chính xác có bao nhiêu phần tử nhỏ hơn và lớn hơn phải chiếm các vị trí bên trái và bên phải của`pos`và sắp xếp các phần tử còn lại một cách tùy ý, chúng ta liệt kê tất cả các hoán vị hợp lệ mà không bị trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(a, m):
    return pow(a, m-2, m)

def precompute_factorials(n):
    fact = [1]*(n+1)
    inv_fact = [1]*(n+1)
    for i in range(1, n+1):
        fact[i] = fact[i-1]*i % MOD
    inv_fact[n] = modinv(fact[n], MOD)
    for i in range(n-1, 0, -1):
        inv_fact[i] = inv_fact[i+1]*(i+1) % MOD
    return fact, inv_fact

def solve():
    n, x, pos = map(int, input().split())
    fact, inv_fact = precompute_factorials(n)
    
    left = 0
    right = n
    less_needed = 0
    greater_needed = 0

    while left < right:
        mid = (left + right) // 2
        if mid <= pos:
            if mid != pos:
                less_needed += 1
            left = mid + 1
        else:
            greater_needed += 1
            right = mid

    if less_needed > x - 1 or greater_needed > n - x:
        print(0)
        return

    remaining = n - 1 - less_needed - greater_needed

    def comb(a, b):
        return fact[a]*inv_fact[b]%MOD*inv_fact[a-b]%MOD

    ans = comb(x-1, less_needed) * fact[less_needed] % MOD
    ans = ans * comb(n-x, greater_needed) % MOD
    ans = ans * fact[greater_needed] % MOD
    ans = ans * fact[remaining] % MOD
    print(ans % MOD)

solve()
```Mã bắt đầu bằng cách tính toán trước các giai thừa và nghịch đảo mô-đun. Sau đó nó mô phỏng tìm kiếm nhị phân để đếm xem có bao nhiêu số nhỏ hơn hoặc lớn hơn`x`phải chiếm giữ những vị trí cụ thể. Công thức tổ hợp tính toán số cách gán các số này và các vị trí còn lại được lấp đầy bằng các số còn sót lại. Số học Modulo đảm bảo các giá trị nằm trong giới hạn. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`n=4, x=1, pos=2`| Bước | trái | đúng | giữa | ít_cần thiết | lớn_cần thiết | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 2 | 0 | 0 | 
| 2 | 3 | 4 | 3 | 0 | 1 | 
| 3 | 3 | 3 | - | 0 | 1 | 

Còn lại = 2 (vị trí 0 và 1).`x-1=0`số nhỏ hơn,`n-x=3`số lớn hơn. Ta chọn 1 số lớn hơn để điền số lớn hơn, 2 số còn lại điền còn lại. Tổng số cách = 3_1_2 = 6. 

**Mẫu 2:**`n=3, x=2, pos=0`| Bước | trái | đúng | giữa | ít_cần thiết | lớn_cần thiết | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 1 | 0 | 1 | 
| 2 | 0 | 1 | 0 | 0 | 1 | 

Còn lại = 1. Số nhỏ hơn = 1, số lớn = 1. Cách = 1_1_1*1=1. 

Những dấu vết này cho thấy mô phỏng tìm kiếm nhị phân xác định chính xác có bao nhiêu số nhỏ hơn và lớn hơn phải chiếm các vị trí bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Việc tính toán trước các giai thừa và nghịch đảo lấy O(n), mỗi mô phỏng tìm kiếm nhị phân là O(log n), mỗi phép nhân tổ hợp là O(n), vì vậy tổng thể O(n^2) | 
| Không gian | O(n) | Mảng giai thừa và nghịch đảo yêu cầu O(n) | 

Lời giải phù hợp thoải mái với các ràng buộc n ≤ 1000. Tính toán giai thừa modulo 10^9+7 tránh tràn số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4 1 2\n") == "6", "sample 1"
assert run("3 2 0\n") == "1", "custom small case"

# Minimum-size input
assert run("1 1 0\n") == "1", "
```
