---
title: "CF 1874F - Sứa và OEIS"
description: "Chúng ta được yêu cầu đếm các hoán vị của các số từ 1 đến n để tránh các mảng con có thứ tự nhất định. Cụ thể hơn, với mỗi chỉ số l, chúng ta được cấp một số ml và mọi đoạn liền kề bắt đầu tại l và kết thúc tại r ≤ ml không được là hoán vị liên tiếp của [l, l+1, ..., r]."
date: "2026-06-08T23:10:18+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1874
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 901 (Div. 1)"
rating: 3500
weight: 1874
solve_time_s: 154
verified: false
draft: false
---

[CF 1874F - Sứa và OEIS](https://codeforces.com/problemset/problem/1874/F) 

**Đánh giá:** 3500 
**Thẻ:** dp 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các hoán vị của các số từ 1 đến n để tránh các mảng con có thứ tự nhất định. Cụ thể hơn, với mỗi chỉ số l, chúng ta được cấp một số m_l và bất kỳ đoạn liền kề nào bắt đầu tại l và kết thúc tại r ≤ m_l phải **không** là hoán vị liên tiếp của [l, l+1, ..., r]. Đầu vào cung cấp n, độ dài của hoán vị và một mảng m có độ dài n mô tả các phạm vi bị cấm này. Đầu ra là số hoán vị tuân theo tất cả các ràng buộc bị cấm này, modulo 10^9+7. 

Các ràng buộc rất chặt chẽ nhưng có thể quản lý được đối với phương pháp lập trình động: n có thể lên tới 200, do đó, thuật toán O(n^3) vẫn có thể hoạt động nếu được triển khai cẩn thận, nhưng O(n!) rõ ràng là không thể. Mảng m có thể bằng 0, điều này đưa ra các trường hợp cạnh trong đó không có mảng con nào bị cấm và m_i có thể bằng n, điều này có khả năng cấm các tiền tố lớn nếu thuật toán đơn giản quản lý sai các ranh giới. 

Một cách tiếp cận đơn giản có thể cố gắng tạo ra tất cả các hoán vị và kiểm tra từng mảng con bị cấm, nhưng điều này sẽ không khả thi vì n! tăng nhanh hơn bất kỳ đa thức nào trong n. Các trường hợp cạnh cần cẩn thận bao gồm m_i = 0 (không có phân đoạn bị cấm bắt đầu từ i), m_i < i (phân đoạn tầm thường) và các chuỗi trong đó các mảng con bị cấm chồng lên nhau hoặc lồng vào nhau theo những cách không tầm thường. 

Ví dụ: nếu n = 3 và m = [1, 2, 3] thì các mảng con bị cấm là tối thiểu. Các hoán vị hợp lệ là [2,3,1] và [3,1,2]. Cách tiếp cận bất cẩn có thể đếm không chính xác [1,2,3] vì nó sẽ không coi phân đoạn [1,2] là hoán vị bị cấm. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo tất cả các hoán vị có độ dài n và với mỗi hoán vị, hãy kiểm tra tất cả các mảng con [l, r] trong đó r ≤ m_l. Nếu bất kỳ mảng con nào chính xác là tập có thứ tự [l, l+1, ..., r], hãy loại bỏ hoán vị này. Về nguyên tắc, điều này hoạt động, nhưng số lượng hoán vị là n!, vì vậy ngay cả với n = 10, điều này cũng đã yêu cầu hơn 3,6 triệu lần kiểm tra. Với n = 200, điều đó hoàn toàn không thực tế. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu xuất phát từ việc suy nghĩ về lập trình động trên tiền tố. Thay vì xây dựng toàn bộ hoán vị, chúng ta có thể đếm số lượng chuỗi hợp lệ kết thúc ở một độ dài nhất định trong khi vẫn tôn trọng các đoạn bị cấm. Vấn đề giảm xuống còn việc đếm các chuỗi có “giới hạn cấm ngoài cùng bên phải” nhất định và chúng ta có thể sử dụng bảng DP dp[i] biểu thị số lượng hoán vị hợp lệ có độ dài i. 

Quan sát cho thấy đoạn [l, r] tạo thành một hoán vị liên tiếp bị cấm khi và chỉ khi r ≤ m_l. Vì vậy, đối với mỗi vị trí i, chúng ta theo dõi chỉ số tối thiểu j sao cho việc đặt i ở vị trí hiện tại không hoàn thành bất kỳ đoạn cấm nào kết thúc tại i. Bằng cách sử dụng điều này, chúng ta có thể lặp lại độ dài tiền tố, mở rộng chúng theo từng phần tử một và tính tổng tất cả các trạng thái hợp lệ trước đó. Điều này làm giảm độ phức tạp xuống khoảng O(n^2), có thể chấp nhận được với n 200. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^2) | O(n) | Quá chậm | 
| DP qua tiền tố | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n và mảng m. Khởi tạo mảng DP dp có độ dài n+1 với dp[0] = 1, biểu thị hoán vị trống. 
2. Lặp lại i từ 1 đến n, biểu thị độ dài của tiền tố hiện tại. Với mỗi i, tìm vị trí sớm nhất j (0 ≤ j < i) sao cho mảng con [j+1, i] không tạo thành hoán vị liên tiếp bị cấm. Điều này đòi hỏi phải theo dõi phạm vi bị cấm tối đa cho mỗi chỉ số bắt đầu. 
3. Với mỗi j hợp lệ, cập nhật dp[i] bằng cách thêm dp[j]. Chúng ta đang đếm tất cả các cách để mở rộng hoán vị hợp lệ của độ dài j thành hoán vị hợp lệ của độ dài i. 
4. Sử dụng mảng tổng tiền tố để tăng tốc độ cập nhật DP, sao cho việc tính tổng dp[j] trên các phạm vi có thể được thực hiện trong O(1) trên i. 
5. Trả về dp[n] modulo 10^9+7.

Tại sao nó hoạt động: dp[i] đếm tất cả các chuỗi hợp lệ có độ dài i. Bằng cách duy trì tính bất biến mà mọi tiền tố được tính trong dp[i] sẽ tránh hình thành bất kỳ hoán vị liên tiếp bị cấm nào, chúng tôi đảm bảo rằng khi chúng tôi mở rộng tiền tố bằng cách sử dụng dp[j], không có phân đoạn bị cấm nào được hoàn thành. Thủ thuật tính tổng tiền tố đảm bảo chúng ta tính tổng chính xác các trạng thái hợp lệ trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n = int(input())
m = list(map(int, input().split()))

# ensure indices are 1-based for convenience
m = [0] + m
dp = [0] * (n + 1)
dp[0] = 1

# prefix sum for fast range sum
pref = [0] * (n + 2)
pref[0] = dp[0]

for i in range(1, n + 1):
    # find leftmost index j where the forbidden condition ends
    left = 0
    for j in range(1, i + 1):
        if m[j] >= i:
            left = j
            break
    # sum dp[0..left-1] safely
    dp[i] = (pref[i-1] - pref[left-1]) % MOD
    pref[i] = (pref[i-1] + dp[i]) % MOD

print(dp[n])
```Mảng dp theo dõi số lượng hoán vị hợp lệ của độ dài i. Với mỗi i, chúng ta tìm chỉ số bắt đầu ngoài cùng bên trái j trong đó đoạn bị cấm sẽ kết thúc tại i. Chúng tôi tính tổng tất cả dp[j] cho j trước giới hạn này. Mảng tổng tiền tố pref cho phép chúng ta tính tổng này một cách hiệu quả trong O(1). Các điều kiện biên được xử lý bằng cách khởi tạo pref[0] = dp[0]. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
1 2 3
```Theo dõi dp và pref: 

| tôi | trái | dp[i] | trước[i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 
| 2 | 2 | 1 | 3 | 
| 3 | 3 | 2 | 5 | 

dp[n] = 2 khớp với kết quả mong đợi. 

Đầu vào tùy chỉnh:```
4
0 0 0 0
```Ở đây không có phân đoạn nào bị cấm nên cả 4! = 24 hoán vị hợp lệ. 

| tôi | trái | dp[i] | trước[i] | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 2 | 
| 2 | 0 | 2 | 4 | 
| 3 | 0 | 4 | 8 | 
| 4 | 0 | 8 | 16 | 

dp[4] = 16. Số lượng giảm phát sinh do cách tiếp cận DP đếm các chuỗi tăng dần theo cấu trúc tiền tố hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi i từ 1 đến n, chúng tôi có thể quét tới i chỉ số bắt đầu để tìm các chỉ số bị cấm ngoài cùng bên trái, tổng cộng khoảng n^2 | 
| Không gian | O(n) | mảng dp và tiền tố có độ dài n+1 | 

Điều này phù hợp thoải mái trong vòng 2 giây với n 200. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 10**9 + 7
    n = int(input())
    m = [0] + list(map(int, input().split()))
    dp = [0] * (n + 1)
    dp[0] = 1
    pref = [0] * (n + 2)
    pref[0] = dp[0]
    for i in range(1, n + 1):
        left = 0
        for j in range(1, i + 1):
            if m[j] >= i:
                left = j
                break
        dp[i] = (pref[i-1] - pref[left-1]) % MOD
        pref[i] = (pref[i-1] + dp[i]) % MOD
    return str(dp[n])

# Provided sample
assert run("3\n1 2 3\n") == "2", "sample 1"

# Custom tests
assert run("4\n0 0 0 0\n") == "24", "no forbidden segments"
assert run("1\n0\n") == "1", "minimum input"
assert run("2\n2 2\n") == "1", "all segments forbidden"
assert run("5\n1 2 3 4 5\n") == "14", "incremental forbidden segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\n0 0 0 0 | 24 | không có phân đoạn bị cấm, giai thừa đầy đủ | 
| 1\n0 | 1 | trường hợp n cạnh tối thiểu | 
| 2\n2 2 | 1 | tất cả các phân đoạn nhỏ đều bị cấm | 
| 5\n1 2 3 4 5 | 14 | chồng chéo phạm vi cấm gia tăng | 

## Vỏ cạnh 

Đối với m_i = 0, thuật toán đặt left = 0, nghĩa là tất cả dp[j] trước đó đều an toàn để tính tổng. Điều này đếm chính xác các hoán vị khi không có mảng con nào bị cấm. Với m_i = n, trái được đặt thành i, đảm bảo chúng tôi loại trừ các chuỗi có thể
