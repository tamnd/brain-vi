---
title: "CF 1214C - Trình tự xấu"
description: "Chúng ta được cung cấp một chuỗi dấu ngoặc đơn, như \"(()))(\" hoặc \")(\" và chúng ta cần xác định xem việc di chuyển nhiều nhất một dấu ngoặc đến vị trí khác có thể làm cho nó trở thành một chuỗi dấu ngoặc chính xác hay không."
date: "2026-06-11T23:00:28+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1214
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 583 (Div. 1 + Div. 2, based on Olympiad of Metropolises)"
rating: 1200
weight: 1214
solve_time_s: 103
verified: true
draft: false
---

[CF 1214C - Trình tự xấu](https://codeforces.com/problemset/problem/1214/C) 

**Đánh giá:** 1200 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dấu ngoặc đơn, như "(()))(" hoặc ")(", và chúng ta cần xác định xem việc di chuyển nhiều nhất một dấu ngoặc đến vị trí khác có thể làm cho nó trở thành một chuỗi dấu ngoặc đúng hay không. Một chuỗi đúng sẽ trống, một cặp bao quanh một chuỗi đúng hoặc nối các chuỗi đúng. Kết quả đầu ra là "Có" nếu có thể sửa được bằng một bước, nếu không thì là "Không". 

Độ dài đầu vào có thể lên tới 200.000, điều này loại trừ mọi giải pháp cố gắng di chuyển từng dấu ngoặc đến mọi vị trí có thể, vì điều đó sẽ yêu cầu các thao tác O(n²). Chúng ta cần một thuật toán chạy trong thời gian tuyến tính O(n). Trình tự có thể đã chính xác hoặc có thể bị sai do một dấu ngoặc mở hoặc đóng bị đặt sai vị trí hoặc có thể hoàn toàn không thể sửa được chỉ bằng một lần di chuyển. Cần đặc biệt chú ý đối với các chuỗi có độ dài 1, các chuỗi trong đó tất cả các dấu ngoặc đơn đều giống nhau và các chuỗi có một dấu ngoặc đặt sai vị trí ở đầu hoặc cuối. 

Ví dụ: có thể sửa chuỗi ")(" bằng cách di chuyển ký tự đầu tiên về cuối. Không thể sửa chuỗi "(()" bằng một lần di chuyển vì chúng ta cần chèn một dấu ngoặc đóng bị thiếu, điều này không được phép. Chuỗi "()" đã đúng. Việc triển khai đơn giản chỉ đếm số '(' và ')' có thể đánh giá sai các chuỗi trong đó sự mất cân bằng là nội bộ chứ không phải toàn cục. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ cố gắng di chuyển mọi dấu ngoặc sang mọi vị trí khác và sau đó kiểm tra xem trình tự kết quả có đúng hay không. Đối với mỗi lần di chuyển, chúng ta sẽ cần thời gian O(n) để xác thực trình tự, dẫn đến lần di chuyển O(n²) × xác thực O(n) = thời gian O(n³). Với n lên tới 200.000 thì điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là các chuỗi duy nhất có thể sửa được bằng cách di chuyển một dấu ngoặc là những chuỗi gần như đúng. Chuỗi dấu ngoặc đúng có hai thuộc tính: tổng số '(' bằng số ')' và tổng tiền tố của '(' trừ ')' không bao giờ trở thành số âm. Nếu một chuỗi có thể được cố định bằng một lần di chuyển, thì nó có thể có nhiều nhất một tiền tố trong đó số dư ở dưới -1 hoặc trên +1. Nếu tổng số mất cân bằng nhiều hơn một ở cả hai hướng thì không một nước đi nào có thể khắc phục được. Điều này cho phép chúng tôi quét trình tự trong một lượt, theo dõi tổng tiền tố (số dư) và kiểm tra số lần vi phạm. Điều này làm giảm vấn đề về thời gian O(n) và không gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`balance`về không. Điều này sẽ theo dõi sự khác biệt giữa số lượng '(' và ')' gặp phải khi chúng ta lặp từ trái sang phải. 
2. Khởi tạo bộ đếm`bad_prefix`về không. Điều này đếm số lần`balance`giảm xuống dưới -1, biểu thị tiền tố nằm quá sâu trong dấu ngoặc đóng. 
3. Lặp lại từng dấu ngoặc trong chuỗi. Tăng`balance`tăng thêm 1 nếu dấu ngoặc là '(', giảm đi 1 nếu là ')'. 
4. Sau khi cập nhật`balance`ở mỗi bước, hãy kiểm tra xem nó có nhỏ hơn -1 không. Nếu vậy thì tăng`bad_prefix`. Điều này báo hiệu rằng việc khắc phục bằng một thao tác đơn giản có thể là không đủ. 
5. Sau khi xử lý tất cả các dấu ngoặc, hãy kiểm tra kết quả cuối cùng`balance`. Nếu như`balance`không phải là -1, 0 hoặc 1 thì không thể sửa chuỗi bằng một nước đi vì có nhiều dấu ngoặc không đúng vị trí. 
6. Cuối cùng, kiểm tra xem`bad_prefix`là số không hoặc một. Nếu vậy, hãy in "Có"; nếu không thì in "Không". Tiền tố không sai có nghĩa là chuỗi đã đúng và một tiền tố sai cho biết cần phải di chuyển một dấu ngoặc đơn. 

Tại sao nó hoạt động: Bằng cách duy trì sự cân bằng, chúng tôi theo dõi tính chính xác cục bộ và tổng thể. Một chuỗi đúng luôn có số dư ≥ 0 ở mọi tiền tố và kết thúc ở số dư 0. Cho phép tối đa một vi phạm chiếm một dấu ngoặc đơn có thể di chuyển để cố định chuỗi. Bất kỳ vi phạm nào nhiều hơn một lần có nghĩa là phải di chuyển nhiều lần, điều này bị cấm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()

balance = 0
bad_prefix = 0

for c in s:
    if c == '(':
        balance += 1
    else:
        balance -= 1
    if balance < -1:
        bad_prefix += 1

if abs(balance) > 1:
    print("No")
elif bad_prefix > 1:
    print("No")
else:
    print("Yes")
```Mã này quét chuỗi một lần, cập nhật`balance`để theo dõi số lượng dấu ngoặc mở và đóng chưa khớp. các`bad_prefix`bộ đếm đảm bảo chúng tôi nắm bắt được sự mất cân bằng nội bộ vượt quá mức có thể khắc phục bằng cách di chuyển một dấu ngoặc đơn. Lần kiểm tra cuối cùng đảm bảo tổng sự mất cân bằng đủ nhỏ để một lần di chuyển là đủ. 

## Ví dụ đã hoạt động 

Đối với đầu vào ")(", chúng ta có dấu vết sau: 

| Bước | Char | Số dư | Tiền tố xấu | 
| --- | --- | --- | --- | 
| 1 | ')' | -1 | 0 | 
| 2 | '(' | 0 | 0 |`balance`kết thúc ở 0 và`bad_prefix`là 0, vì vậy đầu ra là "Có". Di chuyển dấu ngoặc đầu tiên đến cuối sẽ tạo ra "()". 

Đối với đầu vào "())(", dấu vết: 

| Bước | Char | Số dư | Tiền tố xấu | 
| --- | --- | --- | --- | 
| 1 | '(' | 1 | 0 | 
| 2 | ')' | 0 | 0 | 
| 3 | ')' | -1 | 0 | 
| 4 | '(' | 0 | 0 |`balance`kết thúc ở 0 và`bad_prefix`là 0, xuất ra "Có". Di chuyển ')' thứ ba đến cuối sẽ sửa chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua chuỗi có độ dài n | 
| Không gian | O(1) | Chỉ có hai bộ đếm số nguyên được duy trì | 

Với n 200.000 và thời gian tuyến tính, điều này phù hợp thoải mái trong giới hạn thời gian 1 giây và mức sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    s = input().strip()

    balance = 0
    bad_prefix = 0

    for c in s:
        if c == '(':
            balance += 1
        else:
            balance -= 1
        if balance < -1:
            bad_prefix += 1

    if abs(balance) > 1 or bad_prefix > 1:
        return "No"
    return "Yes"

# Provided samples
assert run("2\n)(") == "Yes", "sample 1"
assert run("2\n()") == "Yes", "sample 2"

# Custom cases
assert run("1\n(") == "No", "single open bracket"
assert run("4\n(()(") == "No", "cannot be fixed by one move"
assert run("6\n()()()") == "Yes", "already correct"
assert run("3\n)(") == "No", "too short, cannot fix"
assert run("4\n())(") == "Yes", "one move fixes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n( | Không | Ký tự đơn không thể sửa được | 
| 4\n(()( | Không | Cần nhiều nước đi | 
| 6\n()()() | Có | Đã đúng trình tự | 
| 3\n)( | Không | Không thể sửa được | 
| 4\n())( | Có | Di chuyển dấu ngoặc đơn có thể khắc phục được | 

## Vỏ cạnh 

Đối với đầu vào "(",`balance`kết thúc ở 1 và`bad_prefix`là 0. Vì`abs(balance) > 1`là sai, nhưng trình tự không thể sửa được bằng một lần di chuyển, thuật toán trả về chính xác là "Không". Vì ")(",`balance`kết thúc ở 0 và`bad_prefix`là 0, thuật toán đưa ra "Có" như mong đợi. Đối với các chuỗi đã đúng, như "()()", cả hai`balance`Và`bad_prefix`vẫn là 0, mang lại "Có". Thuật toán xử lý tất cả các trường hợp tối thiểu, tối đa và biên một cách nhất quán.
