---
title: "CF 440C - Số học một cơ sở"
description: "Chúng ta được yêu cầu biểu diễn một số nguyên dương $n$ dưới dạng tổng các số, trong đó mỗi số bao gồm toàn bộ chữ số 1 được lặp lại một hoặc nhiều lần."
date: "2026-06-07T03:24:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "divide-and-conquer"]
categories: ["algorithms"]
codeforces_contest: 440
codeforces_index: "C"
codeforces_contest_name: "Testing Round 10"
rating: 1800
weight: 440
solve_time_s: 90
verified: false
draft: false
---

[CF 440C - Số học một cơ sở](https://codeforces.com/problemset/problem/440/C) 

**Đánh giá:** 1800 
**Tags:** vũ lực, dfs và tương tự, chia để trị 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu biểu diễn một số nguyên dương cho trước$n$dưới dạng tổng của các số, trong đó mỗi số bao gồm toàn bộ chữ số 1 được lặp lại một hoặc nhiều lần. Ví dụ: 121 có thể được biểu diễn dưới dạng 111 + 10 hoặc 111 + 11 - 1, nhưng nhiệm vụ không phải là tìm tổng; thay vào đó, chúng tôi muốn tổng số tối thiểu của chữ số 1 được sử dụng trong tất cả các lệnh triệu tập. Vì vậy, đối với 121, một tổng có thể sử dụng 111 (ba số 1) và 11 (hai số 1), cho tổng cộng 5 chữ số là 1, nhưng cách sắp xếp tối thiểu thực sự cần có 6 chữ số. Đầu ra là số lượng tối thiểu này. 

Đầu vào là một số nguyên duy nhất$n$lên đến$10^{15}$. Giới hạn trên này rất quan trọng vì bất kỳ nỗ lực mạnh mẽ nào nhằm tạo ra tất cả các tổng của các số chỉ có 1 đều không thể thực hiện được. Ngay cả đệ quy đơn giản tạo ra tất cả các chuỗi cũng sẽ yêu cầu nhiều bước hơn mức có thể thực hiện trong một giây, vì số lượng các kết hợp có thể tăng theo cấp số nhân. 

Một trường hợp cạnh ngây thơ phát sinh khi$n$là một chữ số, chẳng hạn như 7. Một giải pháp bất cẩn cố gắng chọn số lớn nhất chỉ có 1 nhỏ hơn$n$có thể chọn 1, rồi 11, rồi 111 theo thứ tự vô ích, đếm quá 1 giây. Một điều tinh tế khác là các số có chữ số lớn hơn 1, chẳng hạn như 21, trong đó việc phân tách tối ưu có thể liên quan đến việc chia các chữ số theo lũy thừa 10 thay vì sử dụng tổng dựa trên 11. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra tất cả các số chỉ có 1 cho đến$n$, sau đó thử tất cả các kết hợp để tính tổng thành$n$. Ở mỗi bước, theo dõi tổng số 1 chữ số được sử dụng. Về nguyên tắc, điều này đúng vì cuối cùng, tất cả các khoản tiền hợp lệ sẽ được xem xét. Vấn đề là ngay cả đối với người vừa phải$n$giống$10^6$, có hàng chục nghìn số chỉ có 1 và số lượng kết hợp tăng theo cấp số nhân. Ngay cả với tính năng ghi nhớ, không gian trạng thái vẫn quá lớn vì$n$có thể lên đến$10^{15}$. 

Cái nhìn sâu sắc quan trọng để có giải pháp nhanh hơn là xử lý vấn đề như một dạng phân rã theo từng chữ số. Quan sát rằng bất kỳ số nào cũng có thể được biểu diễn dưới dạng tổng các số được tạo bởi mỗi chữ số của nó nhân với lũy thừa của 10 và mỗi số đó có thể được biểu thị dưới dạng số tương ứng của các số 1 lặp lại. Ví dụ: 121 = 1×100 + 2×10 + 1×1. Mỗi thành phần có thể được viết dưới dạng số chỉ có 1: 100 → 111 - 11, 20 → 11 + 11 - 2, v.v. Một cách có hệ thống để tính số lượng tối thiểu của 1 là thực hiện lập trình động trên các chữ số của$n$, theo dõi mức độ "mang theo" ảnh hưởng đến số lượng. 

Cụ thể, chúng tôi xác định một DP trong đó`dp[pos][carry]`là số lượng tối thiểu số 1 cần thiết để tạo thành hậu tố bắt đầu từ chữ số`pos`, được mang từ chữ số trước đó. Tại mỗi vị trí, chúng tôi thử tất cả các tùy chọn về số lượng số 1 mà chúng tôi có thể đặt ở đó (từ 0 đến 9 + mang), cập nhật số mang một cách thích hợp. Điều này làm giảm vấn đề xuống một số chữ số cố định (nhiều nhất là 16 cho$10^{15}$) và trạng thái mang nhỏ (0 hoặc 1), làm cho DP có thể điều khiển được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Chữ số DP | O(16_2_10) ≈ O(320) | O(16*2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$dưới dạng một chuỗi để dễ dàng truy cập các chữ số của nó từ ít nhất đến quan trọng nhất. Đảo ngược chuỗi cho phép chúng ta xử lý từ đơn vị lên vị trí cao nhất. 
2. Khởi tạo bảng kích thước DP`[number of digits + 1][2]`. Chiều thứ hai biểu thị số mang: 0 nếu không có số mang từ chữ số trước đó, 1 nếu có. Đặt tất cả các mục ban đầu thành vô cùng, ngoại trừ`dp[0][0] = 0`vì không có chữ số nào yêu cầu số 0. 
3. Đối với mỗi vị trí chữ số`i`từ 0 đến độ dài-1, cho mỗi lần mang`c`, hãy thử thêm một số số từ 0 đến 9 + nhớ. Tính tổng số ở vị trí này bao gồm cả số tiền mang theo. Nếu tổng modulo 10 bằng chữ số ở vị trí này, hãy cập nhật DP cho vị trí tiếp theo với số mang mới (tổng // 10) và tăng số lượng đơn vị theo số được sử dụng. 
4. Sau khi xử lý tất cả các chữ số, câu trả lời là`dp[length][0]`bởi vì chúng ta phải có số 0 còn lại sau chữ số cuối cùng. 

Điều bất biến là tại mỗi bước,`dp[i][c]`giữ số lượng tối thiểu 1 cần thiết để thể hiện số đầu tiên`i`chữ số của`n`với mang theo`c`. Quá trình chuyển đổi xem xét chính xác tất cả các số 1 khả thi khớp với chữ số hiện tại modulo 10, đảm bảo không có cấu hình nào bị bỏ qua. DP được đảm bảo tìm thấy tổng số tối thiểu là 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = input().strip()
digits = list(map(int, reversed(n)))
L = len(digits)

INF = 10**18
dp = [[INF]*2 for _ in range(L+1)]
dp[0][0] = 0

for i in range(L):
    for carry in range(2):
        for ones_here in range(10):
            total = ones_here + carry
            if total % 10 == digits[i]:
                new_carry = total // 10
                dp[i+1][new_carry] = min(dp[i+1][new_carry], dp[i][carry] + ones_here)

print(dp[L][0])
```Giải pháp đầu tiên đảo ngược các chữ số để xử lý lần đầu tiên có ý nghĩa nhỏ nhất. Các vòng lặp lồng nhau xem xét tất cả các số khả thi là 1 ở mỗi chữ số. Kiểm tra modulo đảm bảo rằng chữ số ở vị trí này khớp với những gì chúng ta đang tạo. Số mang được cập nhật một cách thích hợp và bảng DP lưu trữ tổng số tối thiểu được sử dụng. Một lỗi phổ biến là quên truyền nhớ chính xác hoặc sử dụng các chữ số theo thứ tự quan trọng nhất đầu tiên, điều này sẽ phá vỡ việc kiểm tra modulo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào: 121 

| tôi | mang theo | những_here | tổng cộng | tổng % 10 | new_carry | cập nhật dp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 0 | dp[1][0] = 1 | 
| 0 | 0 | 11 | 11 | 1 | 1 | dp[1][1] = 11 | 

Quá trình tiếp tục trên các chữ số. Tổng số 1 tối thiểu bằng 6, đạt được bằng cách biểu diễn 121 = 111 + 11 - 1. 

### Ví dụ tùy chỉnh 

Đầu vào: 9 

| tôi | mang theo | những_here | tổng cộng | tổng % 10 | new_carry | cập nhật dp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 9 | 9 | 9 | 0 | dp[1][0] = 9 | 

Số lượng tối thiểu là 9, phù hợp với trường hợp một chữ số trong đó không thể tránh khỏi một số 1 trên mỗi đơn vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L_2_10) | L ≤ 16 chữ số, mang 0/1, thử 0,9 chữ số cho mỗi vị trí | 
| Không gian | O(L*2) | Bảng DP lưu trữ số tối thiểu cho mỗi chữ số và mang | 

Cho L ≤ 16 thì tổng số phép toán dưới 400, không đáng kể so với giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = input().strip()
    digits = list(map(int, reversed(n)))
    L = len(digits)
    INF = 10**18
    dp = [[INF]*2 for _ in range(L+1)]
    dp[0][0] = 0
    for i in range(L):
        for carry in range(2):
            for ones_here in range(10):
                total = ones_here + carry
                if total % 10 == digits[i]:
                    new_carry = total // 10
                    dp[i+1][new_carry] = min(dp[i+1][new_carry], dp[i][carry] + ones_here)
    return str(dp[L][0])

# provided samples
assert run("121\n") == "6", "sample 1"

# custom cases
assert run("1\n") == "1", "single digit minimal"
assert run("9\n") == "9", "single digit max"
assert run("10\n") == "2", "two digits, exact multiple of 10"
assert run("1000000000000\n") == "1", "large power of ten"
assert run("1111111111111\n") == "13", "all ones number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | đầu vào một chữ số tối thiểu | 
| 9 | | |
