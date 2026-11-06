---
title: "CF 1238A - Phép trừ nguyên tố"
description: "Cho hai số nguyên x và y với x y. Nhiệm vụ là xác định xem có thể chọn một số nguyên tố p và liên tục trừ nó khỏi x cho đến khi đạt được y hay không. Điều quan trọng là khi một số nguyên tố được chọn, nó phải được sử dụng riêng; chúng ta không thể chuyển số nguyên tố giữa chừng."
date: "2026-06-11T22:09:09+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1238
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 74 (Rated for Div. 2)"
rating: 900
weight: 1238
solve_time_s: 83
verified: true
draft: false
---

[CF 1238A - Phép trừ nguyên tố](https://codeforces.com/problemset/problem/1238/A) 

**Xếp hạng:** 900 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`x`Và`y`, với`x > y`. Nhiệm vụ là xác định xem có thể chọn một số nguyên tố duy nhất hay không`p`và liên tục trừ nó khỏi`x`cho đến khi chúng tôi đạt được`y`. Điều quan trọng là khi một số nguyên tố được chọn, nó phải được sử dụng riêng; chúng ta không thể chuyển số nguyên tố giữa chừng. Mỗi trường hợp thử nghiệm là độc lập và các giá trị của`x`Và`y`có thể rất lớn, lên tới$10^{18}$. 

Vấn đề giảm xuống để kiểm tra xem sự khác biệt`x - y`có thể được biểu diễn dưới dạng bội số của một số nguyên tố. Nếu số nguyên tố đó tồn tại thì câu trả lời là "CÓ"; nếu không thì "KHÔNG". 

Các ràng buộc cho thấy chúng ta không thể lặp qua tất cả các số nguyên tố cho đến`x - y`bởi vì sự khác biệt có thể lớn bằng$10^{18}$và chúng tôi có tới 1000 trường hợp thử nghiệm. Một cách tiếp cận ngây thơ cố gắng phân chia thử nghiệm cho mọi số nguyên tố tiềm năng sẽ là quá chậm. 

Các trường hợp biên bao gồm các tình huống trong đó`x - y = 1`. Vì 1 không phải là số nguyên tố nên không thể trừ bất kỳ số nguyên tố nào để đạt được`y`. Một trường hợp khác là khi`x - y`chẵn và lớn hơn 2: chúng ta luôn có thể sử dụng 2 để thu hẹp khoảng cách. Tương tự, nếu`x - y`chính nó là số nguyên tố, chúng ta có thể sử dụng số nguyên tố đó một cách trực tiếp. Những hiểu biết này dẫn đến cách tiếp cận tối ưu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ cố gắng tạo ra tất cả các số nguyên tố nhỏ hơn`x - y`và kiểm tra xem nó có phân chia sự khác biệt hay không. Mặc dù đúng với các đầu vào nhỏ nhưng điều này không thành công vì tạo ra các số nguyên tố lên đến$10^{18}$là không thể thực hiện được. Số lượng hoạt động sẽ rất lớn, dễ dàng vượt quá$10^{15}$, làm cho nó không thực tế. 

Quan sát quan trọng là chúng ta chỉ cần kiểm tra số nguyên tố nhỏ nhất, 2, và liệu chính hiệu đó có phải là số nguyên tố hay không. Nếu như`x - y = 1`, không có số nguyên tố nào hoạt động và câu trả lời là "KHÔNG". Vì`x - y >= 2`, luôn có số nguyên tố`p`đó là sự phân chia sự khác biệt. Cụ thể, nếu chênh lệch là chẵn thì 2 công; nếu hiệu lớn hơn 2 và lẻ thì chính hiệu đó là số nguyên tố hoặc có thừa số nguyên tố nhỏ hơn chính nó. Do đó, vấn đề đơn giản hóa thành việc kiểm tra liên tục theo thời gian cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (phân chia thử / sàng lên đến x-y) | O(sqrt(x-y)) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm cho x-y ~ 1e18 | 
| Tối ưu (phân tích sự khác biệt) | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`x`Và`y`. 
3. Tính chênh lệch`d = x - y`. 
4. Nếu`d == 1`, in "KHÔNG" vì không có số nguyên tố nào có thể thu hẹp chênh lệch bằng 1. 
5. Ngược lại, in "CÓ". Bất kỳ sự khác biệt nào lớn hơn 1 luôn có thể được biểu thị dưới dạng bội số của một số nguyên tố. 

Lý do là mọi số nguyên lớn hơn 1 đều có ít nhất một thừa số nguyên tố. Như vậy, hoặc`d`chính nó là số nguyên tố hoặc có thể phân tích thành bội số của một số nguyên tố thỏa mãn yêu cầu của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x, y = map(int, input().split())
    if x - y == 1:
        print("NO")
    else:
        print("YES")
```Mã theo thuật toán trực tiếp. Chúng tôi sử dụng đầu vào nhanh thông qua`sys.stdin.readline`để xử lý nhiều trường hợp thử nghiệm một cách hiệu quả. Sự khác biệt`x - y`được tính như một phép trừ số nguyên duy nhất. Chúng tôi kiểm tra trường hợp cạnh bị lỗi duy nhất có chênh lệch là 1 và nếu không thì trả về "CÓ". 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
100 98
```| x | y | d = x-y | Đầu ra | 
| --- | --- | --- | --- | 
| 100 | 98 | 2 | CÓ | 

Giải thích:`d = 2`. Hiệu lớn hơn 1 nên ta có thể trừ số nguyên tố 2 đúng một lần. 

Đầu vào mẫu 2:```
41 40
```| x | y | d = x-y | Đầu ra | 
| --- | --- | --- | --- | 
| 41 | 40 | 1 | KHÔNG | 

Giải thích:`d = 1`. Không có số nguyên tố nào có thể bị trừ để đạt được`y`vì mọi số nguyên tố đều lớn hơn 1. 

Những dấu vết này chứng tỏ rằng trường hợp duy nhất cần xử lý cẩn thận là khi chênh lệch chính xác bằng 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Chúng tôi thực hiện các hoạt động liên tục theo thời gian cho từng trường hợp thử nghiệm t. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng, không phụ thuộc vào kích thước đầu vào. | 

Điều này phù hợp thoải mái trong giới hạn thời gian 2 giây ngay cả khi t = 1000 tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())
        if x - y == 1:
            print("NO")
        else:
            print("YES")
    return output.getvalue().strip()

# Provided samples
assert run("4\n100 98\n42 32\n1000000000000000000 1\n41 40\n") == "YES\nYES\nYES\nNO", "sample 1"

# Custom cases
assert run("2\n2 1\n3 2\n") == "NO\nNO", "difference 1 edge case"
assert run("1\n1000000000000000000 999999999999999998\n") == "YES", "large even difference"
assert run("1\n1000000000000000000 999999999999999999\n") == "NO", "large difference of 1"
assert run("1\n17 2\n") == "YES", "odd difference greater than 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | KHÔNG | Sự khác biệt của 1 không thể được bắc cầu bằng số nguyên tố | 
| 3 2 | KHÔNG | Một sự khác biệt khác của 1 | 
| 1e18 1e18-2 | CÓ | Sự chênh lệch đồng đều rất lớn | 
| 1e18 1e18-1 | KHÔNG | Chênh lệch rất lớn 1 | 
| 17 2 | CÓ | Sự khác biệt lẻ lớn hơn 1 là hợp lệ | 

## Vỏ cạnh 

Đối với chênh lệch 1, thuật toán sẽ đưa ra chính xác "KHÔNG". đầu vào`x = 41`,`y = 40`sản lượng`d = 1`, kích hoạt điều kiện cụ thể và tránh giả định không chính xác rằng bất kỳ số nguyên tố nào cũng hoạt động. 

Đối với đầu vào kích thước tối đa, như`x = 10^18`,`y = 1`, sự khác biệt`d = 10^18 - 1`lớn hơn 1 rất nhiều, do đó thuật toán đưa ra "CÓ" một cách hiệu quả mà không cần lặp lại hoặc tạo số nguyên tố. Điều này xác nhận tính chính xác và hiệu suất trên toàn bộ không gian đầu vào.
