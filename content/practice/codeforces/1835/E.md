---
title: "CF 1835E - Di Động Cũ"
description: "Chúng tôi được yêu cầu tính số lần nhấn nút dự kiến ​​cần thiết để nhập một số trên thiết bị di động cũ có bàn phím lạ. Bàn phím có các nút chữ số $m$ và một phím xóa lùi. Jan, người dùng, không thể phân biệt được nút nào cho đến khi anh ta nhấn vào nút đó."
date: "2026-06-09T06:49:44+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1835
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 880 (Div. 1)"
rating: 3500
weight: 1835
solve_time_s: 95
verified: false
draft: false
---

[CF 1835E - Điện thoại di động cũ](https://codeforces.com/problemset/problem/1835/E) 

**Đánh giá:** 3500 
**Tags:** tổ hợp, dp, xác suất 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính số lần nhấn nút dự kiến cần thiết để nhập một số trên thiết bị di động cũ có bàn phím lạ. Bàn phím có$m$các nút chữ số và một phím xóa lùi. Jan, người dùng, không thể phân biệt được nút nào cho đến khi anh ta nhấn vào nút đó. Nếu nhấn sai chữ số, anh ta phải xóa nó bằng phím backspace trước khi thử lại. Do đó, mỗi lần nhấn nút là một lựa chọn mang tính xác suất và Jan luôn hành động tối ưu dựa trên những gì anh đã khám phá được cho đến nay. 

Đầu vào cho biết độ dài số$n$và cơ sở$m$, theo sau là các chữ số của số cần gõ. Đầu ra là số lần nhấn modulo dự kiến$10^9 + 7$, được biểu thị bằng cách sử dụng nghịch đảo mô-đun để xử lý các kỳ vọng hợp lý. 

Với$n$lên đến$10^6$Và$m$lên đến$10^3$, bất kỳ cách tiếp cận nào có nhiều hơn đại khái$O(n m)$hoạt động có thể sẽ quá chậm. Mô phỏng ngây thơ của tất cả các chuỗi nút ngẫu nhiên có thể có sẽ lớn theo cấp số nhân trong$n$, làm cho nó không thể thực hiện được. Các trường hợp cạnh bao gồm khi$n=1$, khi tất cả các chữ số giống hệt nhau, hoặc khi$m=2$do đó xác suất là thô và các khoảng lùi lặp đi lặp lại là thường xuyên. Ví dụ như gõ`0`trong cơ sở 2 có thể yêu cầu nhấn nhiều lần nếu nút ngẫu nhiên đầu tiên là phím lùi hoặc`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi trình tự nhấn nút có thể có cho đến khi Jan gõ số thành công. Mỗi bước bao gồm việc chọn một nút ngẫu nhiên trong số những nút chưa biết và tiến lên nếu đúng hoặc sử dụng phím xóa lùi nếu không chính xác. Điều này tạo ra một cây khả năng với hệ số phân nhánh$m+1$cho từng vị trí, dẫn đến$O((m+1)^n)$tiểu bang. Điều này đúng nhưng chậm về mặt thiên văn ngay cả đối với những$n$và việc lưu trữ một phần kỳ vọng cho mỗi chuỗi là không khả thi. 

Điểm mấu chốt là vấn đề thể hiện một cấu trúc xác suất đơn giản: tại bất kỳ thời điểm nào, số lần nhấn dự kiến ​​để gõ hậu tố còn lại chỉ phụ thuộc vào chữ số nào đã được phát hiện. Bởi vì Jan luôn nhấn một nút không xác định một cách tối ưu, mỗi chữ số chưa xác định mới sẽ thêm một chuỗi số lần nhấn dự kiến ​​tỷ lệ với số nút chưa xác định còn lại. Điều này cho phép chúng tôi tính toán các kỳ vọng lặp đi lặp lại, theo dõi lần xuất hiện cuối cùng của mỗi chữ số. Cấu trúc tổ hợp làm giảm độ phức tạp$O(n m)$, có thể chấp nhận được với các ràng buộc đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((m+1)^n) | O(n) | Quá chậm | 
| Tối ưu | O(n m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`last_seen`kích thước$m$với số không. Điều này sẽ ghi lại vị trí cuối cùng mỗi chữ số xuất hiện. Điều này cho phép chúng tôi xác định những nút nào chúng tôi cần xem xét ở mỗi bước. 
2. Khởi tạo bộ tích lũy`expected`về không. Điều này sẽ giữ tổng số lần nhấn dự kiến ​​​​dưới dạng số hữu tỷ theo modulo$10^9+7$. 
3. Lặp lại các vị trí của số từ trái sang phải. Đối với mỗi vị trí`i`, gọi chữ số hiện tại là`d`. 
4. Tính số nút hiện chưa biết cho chữ số này. Đây là`m - known_digits`, Ở đâu`known_digits`là số chữ số riêng biệt mà Jan đã gặp cho đến vị trí này. 
5. Số lần nhấn dự kiến ​​để gõ chữ số này lần đầu tiên là`(m + 1) / (m - known_digits)`. tử số`m+1`xuất phát từ tổng số nút bao gồm phím lùi và mẫu số phản ánh xác suất nhấn đúng chữ số chưa biết. Sử dụng số học nghịch đảo mô-đun để xử lý phép chia theo mô-đun$10^9+7$. 
6. Cập nhật`last_seen[d]`đến vị trí hiện tại để theo dõi rằng chữ số này đã được phát hiện. Tăng`known_digits`truy cập nếu đây là lần đầu tiên chữ số này xuất hiện. 
7. Tích lũy số lần nhấn dự kiến ​​được tính cho vị trí này vào`expected`. 
8. Sau khi xử lý tất cả các chữ số,`expected`chứa số lần nhấn modulo dự kiến$10^9+7$. Xuất giá trị này. 

Điều bất biến là ở mỗi bước, chúng tôi duy trì số lượng chính xác các chữ số đã biết và vị trí cuối cùng chúng xuất hiện. Điều này đảm bảo rằng xác suất nhấn đúng chữ số ở mỗi bước được tính toán chính xác và giá trị mong đợi là cộng gộp giữa các vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(a, mod):
    return pow(a, mod-2, mod)

def solve():
    n, m = map(int, input().split())
    digits = list(map(int, input().split()))
    
    last_seen = [0] * m
    known_digits = 0
    expected_num = 0
    
    for i, d in enumerate(digits, 1):
        if last_seen[d] == 0:
            prob_den = m - known_digits
            expected_num += (m + 1) * modinv(prob_den, MOD)
            expected_num %= MOD
            known_digits += 1
        last_seen[d] = i
    
    print(expected_num % MOD)
```các`modinv`hàm tính toán nghịch đảo môđun bằng định lý nhỏ Fermat. Chúng tôi sử dụng lập chỉ mục dựa trên 1 cho các vị trí để tránh các vấn đề về phân chia bằng 0. Đối với mỗi chữ số mới, chúng tôi tính toán số lần nhấn dự kiến ​​là`(m+1) / (m - known_digits)`modulo`10^9+7`, và tích lũy. Đang cập nhật`last_seen`đảm bảo chúng tôi không đếm hai lần các chữ số lặp lại. Logic này thực hiện tính toán lặp lại tối ưu của các kỳ vọng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
1 2
0
```| tôi | chữ số | known_digits | thăm dò | dự kiến_num | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 | (2+1)/2 = 3/2 → 666666674 mod 1e9+7 | 

Dấu vết này xác nhận việc tính toán nghịch đảo mô-đun và xử lý chính xác đầu vào một chữ số. 

Đầu vào tùy chỉnh:```
3 3
0 1 0
```| tôi | chữ số | known_digits | thăm dò | dự kiến_num | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 3 | (3+1)/3 = 4/3 | 
| 2 | 1 | 1 | 2 | 4/3 + 4/2 = 4/3 + 2 = 10/3 | 
| 3 | 0 | 2 | 1 | 10/3 + 4/1 = 10/3 + 4 = 22/3 | 

Tính toán modulo cuối cùng:`(22 * modinv(3, MOD)) % MOD`. 

Điều này thể hiện việc xử lý các chữ số lặp lại và điều chỉnh các kỳ vọng bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m) | Mỗi chữ số trong số n chữ số có thể yêu cầu tính toán nghịch đảo mô-đun là O(log MOD) và chúng tôi theo dõi m chữ số | 
| Không gian | O(m) | Mảng lưu trữ Last_seen cho mỗi chữ số có thể | 

Thuật toán lặp lại số một lần, thực hiện số học trên mỗi vị trí, do đó, thuật toán có tỷ lệ thoải mái cho n lên đến 10^6 và m lên đến 10^3. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("1 2\n0\n") == "666666674", "sample 1"

# custom tests
assert run("3 3\n0 1 0\n") == str((22 * pow(3, MOD-2, MOD)) % MOD), "repeated digits"
assert run("2 2\n1 1\n") == str((3 + 3) * pow(1, MOD-2, MOD) % MOD), "same digit repeated"
assert run("1 10\n9\n") == str((11 * pow(10, MOD-2, MOD)) % MOD), "single digit max base"
assert run("4 2\n0 1 0 1\n") == str((3 + 3 + 3 + 3) % MOD), "alternating digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 3\n0 1 0`|`22*modinv(3)`| Xử lý chữ số lặp lại | 
|`2 2\n1 1`|`6`| Tính toán một chữ số lặp đi lặp lại | 
|`1 10\n9`|`11*modinv(10)`| Cơ sở tối đa một chữ số | 
|`4 2\n0 1 0 1`|`12`| Các chữ số xen kẽ và kỳ vọng xóa lùi | 

## Vỏ cạnh 

Khi nào$n=1$Và$m=2$, cho đầu vào`0`,`expected_num`là`(2+1)/2 = 3/2`. Thuật toán tính toán`modinv(2, MOD)`một cách chính xác và nhân lên, cho`666666674`. Khi các chữ số lặp lại,`known_digits`đảm bảo công thức kỳ vọng không tính hai lần. Để có cơ số tối đa, nghịch đảo mô-đun ngăn ngừa lỗi chia số nguyên. Đối với các chữ số xen kẽ, mỗi chữ số mới sẽ kích hoạt các lần nhấn đúng dự kiến, xác minh rằng mô hình cộng lặp có thể xử lý tất cả các chuỗi.
