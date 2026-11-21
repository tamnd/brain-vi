---
title: "CF 2218A - Bài toán số nguyên thứ 67"
description: "Bài toán yêu cầu chúng ta chọn một số nguyên $y$ cho một số nguyên $x$ cho trước sao cho giá trị tối thiểu của $x$ và $y$ là lớn nhất có thể. Chúng ta có nhiều trường hợp thử nghiệm, mỗi trường hợp bao gồm một số nguyên $x$ trong khoảng từ -67 đến 67."
date: "2026-06-02T08:34:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "games", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2218
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1090 (Div. 4)"
rating: 800
weight: 2218
solve_time_s: 87
verified: true
draft: false
---

[CF 2218A - Bài toán số nguyên thứ 67](https://codeforces.com/problemset/problem/2218/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, trò chơi, thực hiện, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta chọn một số nguyên$y$cho một số nguyên nhất định$x$sao cho tối thiểu$x$Và$y$là càng lớn càng tốt. Chúng ta có nhiều trường hợp thử nghiệm, mỗi trường hợp bao gồm một số nguyên duy nhất$x$trong khoảng từ -67 đến 67. Đầu ra của chúng ta phải là số nguyên$y$trong cùng giới hạn cho mỗi trường hợp thử nghiệm. 

Trình bày lại một cách đơn giản, nếu chúng ta hình dung$x$Và$y$trên trục số,$\min(x, y)$luôn nhỏ hơn trong hai số đó. Để tối đa hóa mức tối thiểu này, chúng ta cần$y$thực sự lớn hơn$x$. Sau đó$\min(x, y)$sẽ bằng$x$, đó là giá trị lớn nhất có thể. Nếu như$y \le x$, tối thiểu sẽ là$y$, nhỏ hơn hoàn toàn so với$x$, do đó sẽ không tối đa hóa mức tối thiểu. 

Những hạn chế là rất nhỏ.$x$được giới hạn từ -67 đến 67 và có nhiều nhất 6767 trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm là độc lập và việc tính toán giải pháp cho mỗi trường hợp là chuyện nhỏ. Thậm chí một$O(t)$cách tiếp cận với số học đơn giản dễ dàng trong giới hạn thời gian 1 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các giá trị tối đa và tối thiểu. Vì$x = 67$, chúng ta không thể chọn$y > 67$bởi vì giới hạn hạn chế chúng tôi. Vì thế,$y = 67$bản thân nó là hợp lệ. Tương tự, đối với$x = -67$, bất kì$y \ge -67$sẽ mang lại$\min(x, y) = -67$, vì vậy chúng ta có thể chọn$y = -66$hoặc bất kỳ số lượng lớn hơn trong giới hạn. Chìa khóa luôn là chọn một số ngay phía trên$x$nếu có thể, nhưng tôn trọng giới hạn -67 đến 67. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là tầm thường. Đối với một nhất định$x$, chúng ta có thể thử mọi cách$y$từ -67 đến 67, tính$\min(x, y)$cho mỗi kết quả và chọn kết quả lớn nhất. Điều này hoạt động vì chỉ có 135 có thể$y$giá trị, nhưng nó không cần thiết vì mẫu có thể dự đoán được. Sự phức tạp mạnh mẽ sẽ là$O(t \cdot 135)$, có thể chấp nhận được nhưng cồng kềnh và không trang nhã. 

Cái nhìn sâu sắc quan trọng là$\min(x, y)$được tối đa hóa bất cứ khi nào$y > x$, vì khi đó giá trị tối thiểu bằng$x$. Vì vậy, sự lựa chọn tối ưu cho$y$đơn giản là$x+1$, trừ khi vượt quá giới hạn trên của 67. Trong trường hợp đó,$y = 67$là sự lựa chọn hợp lệ duy nhất. Quan sát này làm giảm vấn đề thành một phép tính thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t * 135) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Điều này xác định số lần chúng tôi lặp lại tính toán. 
2. Với mỗi test case, hãy đọc$x$. Chúng tôi sẽ xác định$y$vì điều này$x$. 
3. Tính toán$y = x + 1$. Điều này đảm bảo rằng$\min(x, y) = x$, đó là cực đại. 
4. Nếu$y > 67$, điều chỉnh$y$là 67 để ở trong giới hạn. 
5. Đầu ra$y$. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Lý do điều này có tác dụng là vì ngày càng tăng$y$bên trên$x$không giảm mức tối thiểu, do đó mức tối thiểu không thể lớn hơn$x$. Lựa chọn$y = x + 1$giữ cho nó đơn giản, tránh các vấn đề bình đẳng và tôn trọng các ràng buộc của vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x = int(input())
    y = min(x + 1, 67)
    print(y)
```Mã đầu tiên được đọc$t$, số lượng ca kiểm thử Sau đó, với mỗi trường hợp thử nghiệm, nó sẽ đọc$x$, tính toán$y$sử dụng logic được mô tả ở trên và in nó. sử dụng`min(x + 1, 67)`đảm bảo rằng chúng ta không bao giờ vượt quá giới hạn trên. I/O nhanh với`sys.stdin.readline`được sử dụng, đó là thông lệ tiêu chuẩn cho lập trình cạnh tranh, mặc dù ở đây`input()`cũng sẽ đủ do kích thước đầu vào nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: 1 

| Bước | x | y = x + 1 | phút(x, y) | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 

Tối thiểu của 1 và 2 là 1, là số tối đa vì mọi giá trị nhỏ hơn$y$sẽ cho mức tối thiểu nhỏ hơn. Điều này xác nhận thuật toán hoạt động với số dương. 

### Ví dụ 2 

Đầu vào: 67 

| Bước | x | y = x + 1 | Điều chỉnh y | phút(x, y) | 
| --- | --- | --- | --- | --- | 
| 1 | 67 | 68 | 67 | 67 | 

Đây,$x + 1$vượt quá giới hạn nên chúng tôi giới hạn ở mức 67. Giá trị tối thiểu vẫn là 67, điều này là chính xác. Điều này xác nhận rằng các điều kiện biên được xử lý đúng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Độ phức tạp về thời gian có thể chấp nhận được vì$t \le 6767$. Dung lượng bộ nhớ ở mức tối thiểu, thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        x = int(input())
        y = min(x + 1, 67)
        print(y)
    
    return output.getvalue().strip()

# Provided sample
assert run("3\n1\n3\n5\n") == "2\n4\n6", "sample 1"

# Custom tests
assert run("2\n67\n-67\n") == "67\n-66", "boundary max and min"
assert run("1\n0\n") == "1", "zero input"
assert run("4\n-1\n0\n1\n2\n") == "0\n1\n2\n3", "consecutive small numbers"
assert run("1\n66\n") == "67", "near upper bound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 67, -67 | 67, -66 | Xử lý giới hạn trên và dưới | 
| 0 | 1 | Đầu vào bằng 0 được xử lý chính xác | 
| -1,0,1,2 | 0,1,2,3 | Số nhỏ liên tiếp, chữ thường | 
| 66 | 67 | Trường hợp cạnh gần giới hạn tối đa | 

## Vỏ cạnh 

cho$x = 67$,$x + 1 = 68$vượt quá phạm vi cho phép. Các thuật toán kẹp$y$đến 67, giữ nguyên$\min(x, y) = 67$. Vì$x = -67$, thêm 1 sẽ có -66, nằm trong giới hạn, đảm bảo mức tối thiểu được tối đa hóa ở -67. Điều này xác nhận rằng cả hai đầu của phạm vi cho phép đều được xử lý chính xác. Đối với bất kỳ trung gian nào$x$,$y = x + 1$đảm bảo$\min(x, y) = x$, đó là mức tối thiểu lớn nhất có thể.
