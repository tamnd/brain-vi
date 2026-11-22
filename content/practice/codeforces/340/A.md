---
title: "CF 340A - Bức Tường"
description: "Chúng ta được yêu cầu đếm số viên gạch được cả Iahub và Floyd sơn trong một phạm vi cụ thể. Các viên gạch được đánh số bằng các số nguyên liên tiếp bắt đầu từ 1."
date: "2026-06-06T17:15:38+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 340
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 198 (Div. 2)"
rating: 1200
weight: 340
solve_time_s: 80
verified: true
draft: false
---

[CF 340A - Bức tường](https://codeforces.com/problemset/problem/340/A) 

**Đánh giá:** 1200 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số viên gạch được cả Iahub và Floyd sơn trong một phạm vi cụ thể. Các viên gạch được đánh số bằng các số nguyên liên tiếp bắt đầu từ 1. Iahub vẽ mọi viên gạch thứ _x_, bắt đầu từ viên gạch _x_, trong khi Floyd vẽ mọi viên gạch thứ _y_, bắt đầu từ viên gạch _y_. Chúng ta chỉ quan tâm đến những viên gạch được đánh số từ _a_ đến _b_, được sơn bởi cả hai. 

Đầu vào bao gồm bốn số nguyên: _x_, _y_, _a_ và _b_. Đầu ra là một số nguyên duy nhất biểu thị số viên gạch được sơn bởi cả hai. 

Các ràng buộc cho chúng ta biết rằng _x_ và _y_ nhiều nhất là 1000, do đó, bất kỳ thuật toán nào sử dụng chúng làm kích thước bước trực tiếp đều khả thi. Phạm vi [_a_, _b_] có thể lên tới 2·10⁹, điều này khiến cho việc mô phỏng lực lượng vũ phu trên toàn bộ khoảng thời gian là không thực tế. Một vòng lặp đơn giản trên mỗi viên gạch trong [_a_, _b_] có thể yêu cầu 2·10⁹ lần lặp, vượt xa thời gian thực hiện 1 giây. 

Trường hợp cạnh phát sinh khi _a_ nhỏ hơn cả _x_ và _y_. Ví dụ: nếu _x_ = 5, _y_ = 7, _a_ = 1, _b_ = 10, chúng ta phải bao gồm chính xác bội số đầu tiên của 5 và 7 rơi vào [1, 10]. Việc phân chia hoặc làm tròn sàn bất cẩn có thể loại trừ nó. Một trường hợp cạnh khác là khi _a_ bằng _b_, chẳng hạn như _x_ = 3, _y_ = 3, _a_ = 6, _b_ = 6. Thuật toán phải đếm chính xác một viên gạch nếu nó được vẽ bởi cả hai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản để mô tả. Chúng tôi lặp lại từng viên gạch được đánh số từ _a_ đến _b_. Đối với mỗi viên gạch, chúng tôi kiểm tra xem nó có chia hết cho cả _x_ và _y_ hay không. Nếu đúng như vậy, chúng tôi tăng một bộ đếm. Điều này đúng vì một viên gạch được sơn bởi cả hai khi và chỉ khi số của nó chia hết cho cả hai kích cỡ bước. Vấn đề với cách tiếp cận này là trường hợp xấu nhất: nếu _b - a_ ở mức 2·10⁹, vòng lặp thực hiện hàng tỷ lần lặp, quá chậm. 

Thông tin chi tiết quan trọng cho giải pháp tối ưu là một viên gạch được sơn bởi cả khi và chỉ khi nó là bội số của bội số chung nhỏ nhất (LCM) của _x_ và _y_. Khi chúng ta biết LCM, chẳng hạn _z_, vấn đề sẽ giảm xuống việc đếm bội số của _z_ trong khoảng [_a_, _b_]. Điều này có thể được thực hiện về mặt toán học mà không cần lặp lại từng viên gạch. Để đếm bội số của _z_ trong một phạm vi, chúng tôi tính số bội số lên đến _b_, trừ đi số bội số hoàn toàn nhỏ hơn _a_ và hiệu sẽ cho kết quả đếm trong [_a_, _b_]. Cách tiếp cận này tránh mọi mô phỏng trên mỗi viên gạch và chạy trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(b - a + 1) | O(1) | Quá chậm đối với b-a lớn | 
| Tối ưu | O(log(max(x, y))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính ước chung lớn nhất (GCD) của _x_ và _y_. Điều này là cần thiết để tính toán LCM một cách hiệu quả. Sử dụng công thức`LCM(x, y) = x * y // GCD(x, y)`đảm bảo chúng ta tránh tràn số nguyên bằng cách chia trước khi nhân. 
2. Tính LCM của _x_ và _y_, mà chúng ta gọi là _z_. Một viên gạch được cả hai họa sĩ sơn khi và chỉ khi số của nó là bội số của _z_. 
3. Đếm xem có bao nhiêu bội số của _z_ nhỏ hơn hoặc bằng _b_. Việc này được thực hiện bởi`b // z`. 
4. Đếm xem có bao nhiêu bội số của _z_ nhỏ hơn _a_. Việc này được thực hiện bởi`(a - 1) // z`. Trừ hai số đếm này sẽ cho số bội của _z_ trong [_a_, _b_]. 
5. In kết quả. 

Điều bất biến là mọi bội số của _z_ trong khoảng [_a_, _b_] được tính chính xác một lần. Không có số nào khác trong khoảng là bội số của cả _x_ và _y_, vì vậy số đếm là chính xác. Thuật toán không bao giờ cần phải kiểm tra từng viên gạch riêng lẻ, khiến nó trở nên hiệu quả ngay cả đối với những phạm vi rất lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

x, y, a, b = map(int, input().split())

def lcm(u, v):
    return u * v // gcd(u, v)

z = lcm(x, y)
count = b // z - (a - 1) // z
print(count)
```Phần đầu tiên đọc đầu vào và nhập`gcd`hàm từ thư viện toán học của Python. các`lcm`Hàm tính bội số chung nhỏ nhất một cách hiệu quả. Việc tính toán`b // z - (a - 1) // z`đếm bội số của`z`trong phạm vi mong muốn, chú ý bao gồm _a_ nếu bản thân nó là bội số. Phép trừ`(a - 1) // z`đảm bảo chúng ta không đếm thiếu khi bản thân _a_ là bội số. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

Đầu vào: 2 3 6 18 

| Bước | z (LCM) | Bội số ≤ b | Bội số < a | Đếm | 
| --- | --- | --- | --- | --- | 
| Tính z | 6 | 3 | 0 | 3 | 

Bội số của 6 trong [6, 18] là 6, 12, 18. Thuật toán đếm đúng 3. 

**Mẫu 2** 

Đầu vào: 5 7 1 10 

| Bước | z (LCM) | Bội số ≤ b | Bội số < a | Đếm | 
| --- | --- | --- | --- | --- | 
| Tính z | 35 | 0 | 0 | 0 | 

Không có viên gạch nào được đánh số từ 1 đến 10 là bội số của cả 5 và 7. Số đếm là 0. 

Những dấu vết này cho thấy phương thức này hoạt động cả khi phạm vi chứa bội số và khi phạm vi không chứa bội số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(min(x, y))) | Tính toán GCD chiếm ưu thế, là logarit nhỏ hơn x hoặc y | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với các ràng buộc, phương pháp này thực thi trong thời gian không đổi đối với giới hạn đầu vào, dễ dàng dưới 1 giây và trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, y, a, b = map(int, input().split())
    z = x * y // gcd(x, y)
    return str(b // z - (a - 1) // z)

# Provided sample
assert run("2 3 6 18\n") == "3", "sample 1"

# Custom cases
assert run("5 7 1 10\n") == "0", "no overlap"
assert run("3 3 6 6\n") == "1", "single brick"
assert run("1 1 1 1000000000\n") == "1000000000", "every brick"
assert run("2 3 1 1\n") == "0", "range below first multiple"
assert run("2 3 1 6\n") == "1", "include first multiple only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 7 1 10 | 0 | Không có gạch sơn bởi cả hai trong phạm vi | 
| 3 3 6 6 | 1 | Gạch đơn bằng bội số | 
| 1 1 1 1000000000 | 1000000000 | Tất cả gạch được sơn bởi cả hai | 
| 2 3 1 1 | 0 | Phạm vi bên dưới bội số đầu tiên | 
| 2 3 1 6 | 1 | Bao gồm bội số đầu tiên tại ranh giới | 

## Vỏ cạnh 

Khi phạm vi bắt đầu bên dưới bội số đầu tiên, phép trừ`(a - 1) // z`đảm bảo không tính thiếu. Ví dụ: x = 2, y = 3, a = 1, b = 5, z = 6. Bội số ≤ b = 0, bội số < a = 0, count = 0. Nếu bản thân _a_ là bội số, như a = 6, b = 6, thì bội số ≤ b = 1, bội số < a = 0, count = 1, bao gồm cả viên gạch một cách chính xác. Thuật toán xử lý một cách tự nhiên các phạm vi có độ dài 1, phạm vi bắt đầu từ bội số và phạm vi lớn nhờ phương pháp đếm số học.
