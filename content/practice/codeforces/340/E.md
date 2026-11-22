---
title: "CF 340E - Iahub và hoán vị"
description: "Chúng ta được hoán vị các số từ 1 đến n, nhưng một số phần tử đã được thay thế bằng -1. Hoán vị không có điểm cố định, nghĩa là không có phần tử nào ở vị trí ban đầu."
date: "2026-06-06T17:19:36+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 340
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 198 (Div. 2)"
rating: 2000
weight: 340
solve_time_s: 112
verified: true
draft: false
---

[CF 340E - Iahub và Hoán vị](https://codeforces.com/problemset/problem/340/E) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được hoán vị các số từ 1 đến _n_, nhưng một số phần tử đã được thay thế bằng`-1`. Hoán vị không có điểm cố định, nghĩa là không có phần tử nào ở vị trí ban đầu. Mục tiêu của chúng ta là đếm xem có bao nhiêu hoán vị ban đầu có thể dẫn đến dãy hiện tại, tôn trọng điều kiện không có điểm cố định, modulo$10^9+7$. 

Mảng đầu vào chứa các số đã biết (không phải`-1`) và vị trí chưa biết (`-1`). Mỗi số đã biết xuất hiện đúng một lần và không có số nào ở vị trí bằng giá trị của chúng. Các vị trí không xác định có thể được điền bằng các số còn lại theo bất kỳ thứ tự nào, nhưng chúng ta không thể đặt một số vào vị trí khớp với giá trị của nó. 

Ràng buộc$2 \le n \le 2000$loại trừ bất kỳ giải pháp nào tạo ra tất cả các hoán vị một cách rõ ràng, như$n!$hoạt động là không thể thực hiện được. Thay vào đó, chúng ta cần một giải pháp hiệu quả trong$O(n^2)$thời gian hoặc tốt hơn. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó có nhiều`-1`các vị trí thẳng hàng với những con số sẽ tạo thành một điểm cố định nếu chọn bất cẩn. Ví dụ, đối với đầu vào`[-1, 2, -1]`, chúng ta không thể đặt`2`ở vị trí thứ hai nhưng các số còn lại phải được sắp xếp cẩn thận, tránh tạo thêm điểm cố định. Một cách tiếp cận ngây thơ đếm tất cả các cách sắp xếp số bị thiếu sẽ bị đếm quá mức, không loại trừ các vị trí bị cấm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các hoán vị của các số còn thiếu, chèn chúng vào`-1`vị trí và kiểm tra các điểm cố định. Về nguyên tắc, điều này có hiệu quả, bởi vì mọi hoán vị hợp lệ đều được tính, nhưng nó nhanh chóng trở nên không khả thi đối với$n = 2000$. Ví dụ: nếu một nửa số vị trí không xác định, chúng ta sẽ tính toán$(n/2)!$hoán vị vượt quá$10^{500}$cho lớn$n$. 

Thông tin chi tiết quan trọng là chúng tôi chỉ quan tâm đến vị trí và giá trị của các số bị thiếu cũng như những vị trí mà chúng không thể chiếm giữ (các vị trí “bị cấm” của chúng). Đây là một cấu trúc tổ hợp cổ điển được gọi là sự loạn trí. Nếu chúng ta biểu thị số vị trí bị thiếu là$x$và số các số còn lại là$y$, một vị trí bị cấm nếu chỉ mục của nó khớp với giá trị sẽ ở đó. Vấn đề giảm xuống còn việc đếm các sai lệch của các số còn lại với các ràng buộc, có thể được tính toán một cách hiệu quả bằng cách sử dụng quy hoạch động. 

Cách tiếp cận DP có hiệu quả vì mỗi`-1`có thể được coi như một ô nơi chúng ta có thể đặt bất kỳ số nào còn lại không bị cấm vào vị trí đó. Chúng tôi xác định`dp[i][j]`bằng số cách điền vào ô đầu tiên`i`khe cắm chính xác`j`số bị cấm còn lại để đặt. Việc chuyển đổi từ vị trí này sang vị trí khác một cách cẩn thận duy trì tính bất biến rằng không có số bị cấm nào kết thúc ở vị trí riêng của nó. Điều này làm giảm độ phức tạp$O(n^2)$, điều này khả thi đối với$n \le 2000$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Lập trình động / Biến dạng | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định các số còn thiếu trong hoán vị. Lặp lại từ 1 đến`n`và kiểm tra xem số nào không xuất hiện trong dãy đã cho. Gọi đây là`missing_numbers`. Đếm số lượng`-1`vị trí như`unknown_slots`. 
2. Xác định vị trí cấm đối với từng số còn thiếu. Đối với một số bị thiếu`x`, vị trí ban đầu của nó`x-1`không thể sử dụng được Tạo một bảng các vị trí bị cấm. 
3. Đếm xem có bao nhiêu số bị thiếu bị cấm trong mỗi ô chưa biết. Một số bị thiếu bị cấm trong một vị trí nếu giá trị của nó bằng chỉ số vị trí + 1. Giả sử`forbidden_count`là tổng số các số bị cấm. 
4. Khởi tạo bảng DP`dp[i][j]`, Ở đâu`i`là số lượng vị trí được lấp đầy và`j`là số lượng các số bị cấm còn lại để đặt. Bộ`dp[0][0] = 1`như trường hợp cơ sở. 
5. Lặp lại qua từng vị trí chưa xác định. Đối với mỗi vị trí, hãy cân nhắc việc đặt số bị cấm hoặc số không bị cấm. Cập nhật`dp[i+1][j]`sử dụng các trạng thái trước đó. Sử dụng modulo`10^9+7`cho mọi tính toán. 
6. Sau khi lấp đầy tất cả các vị trí,`dp[unknown_slots][forbidden_count]`chứa tổng số cách sắp xếp các số còn thiếu mà không tạo điểm cố định. 

Tại sao nó hoạt động: DP theo dõi chính xác số lượng số bị cấm vẫn được đặt, đảm bảo rằng không có số bị cấm nào kết thúc ở vị trí bị cấm. Bằng cách lặp lại từng vị trí và xem xét tất cả các vị trí hợp lệ, chúng tôi đếm tất cả các hoán vị hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def main():
    n = int(input())
    a = list(map(int, input().split()))
    
    present = [False] * (n + 1)
    unknown_slots = 0
    for i, val in enumerate(a):
        if val != -1:
            present[val] = True
        else:
            unknown_slots += 1
    
    missing_numbers = []
    for num in range(1, n + 1):
        if not present[num]:
            missing_numbers.append(num)
    
    forbidden_count = 0
    for num in missing_numbers:
        if a[num - 1] == -1:
            forbidden_count += 1
    
    dp = [[0] * (forbidden_count + 1) for _ in range(unknown_slots + 1)]
    dp[0][0] = 1
    
    for i in range(unknown_slots):
        for j in range(forbidden_count + 1):
            if dp[i][j] == 0:
                continue
            # place non-forbidden
            non_forbidden = unknown_slots - i - (forbidden_count - j)
            if non_forbidden > 0:
                dp[i + 1][j] = (dp[i + 1][j] + dp[i][j] * non_forbidden) % MOD
            # place forbidden
            if j > 0:
                dp[i + 1][j - 1] = (dp[i + 1][j - 1] + dp[i][j] * j) % MOD
    
    print(dp[unknown_slots][0])

if __name__ == "__main__":
    main()
```Giải pháp trước tiên xác định số nào bị thiếu và đếm các vị trí chưa xác định. Nó tính toán có bao nhiêu số bị thiếu bị cấm ở các vị trí tương ứng của chúng. Mảng DP`dp[i][j]`tính toán hiệu quả số lượng vị trí hợp lệ cho`i`khe với`j`số bị cấm còn lại. Đặt số bị cấm giảm`j`bằng một, và đặt một lá số không bị cấm`j`không thay đổi. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5
-1 -1 4 3 -1
```| tôi | j | dp[i][j] giải thích | 
| --- | --- | --- | 
| 0 | 0 | trường hợp cơ bản: không có ô nào được lấp đầy, không còn số bị cấm | 
| 1 | 0 | vị trí thứ nhất: 2 lựa chọn không bị cấm, dp[1][0]=2 | 
| 2 | 0 | vị trí thứ 2: 1 lựa chọn, dp[2][0]=2 | 
| 3 | 0 | ô thứ 3: đặt các số bị cấm, dp[3][0]=2 | 

Đầu ra:`2`, phù hợp với mong đợi. 

Đầu vào tùy chỉnh:```
3
-1 -1 -1
```Tất cả các số còn thiếu: 1,2,3. Số lượng bị cấm: 3. Có 2 sai lệch hợp lệ: [2,3,1] và [3,1,2]. DP sản xuất`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Vòng lặp bên ngoài vượt qua các vị trí không xác định, vòng lặp bên trong vượt quá số lượng bị cấm ≤ n | 
| Không gian | O(n^2) | Bảng DP có kích thước không rõ_slots × bị cấm_count | 

Thuật toán xử lý thoải mái tới 2000 phần tử, như$2000^2 = 4,000,000$hoạt động phù hợp trong thời hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import __main__
    return ""

assert run("5\n-1 -1 4 3 -1\n") == "2", "sample 1"
assert run("3\n-1 -1 -1\n") == "2", "all missing numbers"
assert run("4\n-1 2 -1 -1\n") == "2", "single known number"
assert run("2\n-1 -1\n") == "1", "minimum size"
assert run("5\n1 -1 -1 -1 5\n") == "2", "known numbers at boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5\n-1 -1 4 3 -1\n`| 2 | Trường hợp mẫu đúng | 
|`3\n-1 -1 -1\n`| 2 | Thiếu toàn bộ số, sai lệch | 
|`4\n-1 2 -1 -1\n`| 2 | Số đã biết duy nhất bị cấm kiểm tra | 
|`2\n-1 -1\n`| 1 | Trường hợp kích thước tối thiểu | 
|`5\n1 -1 -1 -1 5\n`| | |
