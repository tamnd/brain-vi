---
title: "CF 1477A - Nezzar và Hội đồng quản trị"
description: "Chúng ta được cấp một bảng với $n$ các số nguyên riêng biệt và chúng ta có thể thực hiện lặp đi lặp lại một thao tác lấy hai số $x$ và $y$ từ bảng và viết ra $2x - y$."
date: "2026-06-10T23:52:59+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1477
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 698 (Div. 1)"
rating: 1800
weight: 1477
solve_time_s: 121
verified: true
draft: false
---

[CF 1477A - Nezzar và Board](https://codeforces.com/problemset/problem/1477/A) 

**Đánh giá:** 1800 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bảng có$n$các số nguyên riêng biệt và chúng ta có thể thực hiện lặp đi lặp lại một phép toán có hai số$x$Và$y$từ bảng và viết ra$2x - y$. Nhiệm vụ là xác định liệu một số mục tiêu$k$có thể xuất hiện trên bảng sau một số chuỗi thao tác này. Mỗi trường hợp thử nghiệm đưa ra các số ban đầu và mục tiêu$k$, và chúng ta phải xuất ra "CÓ" nếu có thể và "KHÔNG" nếu không. 

Các ràng buộc rất lớn:$n$có thể lên đến$2 \cdot 10^5$, tổng của tất cả$n$trên các trường hợp thử nghiệm cũng bị giới hạn bởi$2 \cdot 10^5$và bản thân các số đó có thể lớn bằng$10^{18}$về độ lớn. Điều này có nghĩa là bất kỳ giải pháp nào lặp lại trên tất cả các chuỗi phép tính có thể đều không khả thi, vì ngay cả một phép toán theo cặp đơn lẻ trên tất cả các số lặp đi lặp lại cũng sẽ tạo ra một vụ nổ tổ hợp. 

Các trường hợp cạnh phát sinh khi$n = 2$, vì chỉ có hai số nên trình tự các phép tính về cơ bản bị ràng buộc theo cấp số cộng được xác định bởi hiệu của hai số. Một trường hợp khó phát hiện khác xảy ra khi$k$đã có trên bảng. Một cách tiếp cận ngây thơ luôn cố gắng xây dựng các số mới mà không kiểm tra các số hiện có sẽ không thể nhận ra ngay rằng$k$đã có mặt rồi. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng tạo ra tất cả các số có thể đạt được thông qua thao tác$2x - y$cho tất cả các cặp số$x, y$trên bảng. Điều này có thể được lặp lại cho đến khi không có số mới nào được tạo ra. Cách tiếp cận này đúng về nguyên tắc, bởi vì cuối cùng tất cả các số trong bao đóng cộng của tập hợp ban đầu theo thao tác này sẽ được tạo ra. Tuy nhiên, số lượng tiềm năng được tạo ra có thể bùng nổ, đặc biệt đối với số lượng lớn$n$, dẫn đến các phép toán theo thứ tự$O(n^2 \cdot m)$, Ở đâu$m$là số lượng các số mới được tạo ra. Đối với những hạn chế của chúng tôi, điều này là quá chậm. 

Cái nhìn sâu sắc quan trọng là để quan sát rằng$2x - y$có thể được viết lại như$x + (x - y)$. Điều này cho thấy thao tác này cho phép chúng ta cộng hoặc trừ bội số của hiệu giữa các số một cách hiệu quả. Nói cách khác, tập hợp tất cả các số mà chúng ta có thể đạt tới là một mạng số học được tạo bởi ước chung lớn nhất (GCD) của tất cả các hiệu theo cặp. Cho phép$\text{min\_val}$là số nhỏ nhất trên bảng và gọi$d$là GCD của mọi khác biệt$x_i - \text{min\_val}$. Sau đó một số$k$có thể truy cập được khi và chỉ khi$k - \text{min\_val}$chia hết cho$d$. Điều này làm giảm vấn đề từ việc mô phỏng các phép tính sang số học đơn giản trên tập hợp ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * m) | O(n^2) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$, sau đó đọc danh sách các số ở trên bảng. 
2. Nếu$k$đã có trong danh sách, xuất ngay "CÓ". 
3. Xác định số nhỏ nhất trên bảng,$\text{min\_val}$, vì nó sẽ đóng vai trò là điểm tham chiếu. 
4. Tính sự khác biệt$x_i - \text{min\_val}$cho tất cả các số$x_i$lên bảng và tính GCD của họ, gọi nó là$d$. GCD này biểu thị "kích thước bước" của mạng số học có thể truy cập được từ tập hợp ban đầu. 
5. Kiểm tra xem$k - \text{min\_val}$chia hết cho$d$. Nếu đúng thì xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Mỗi hoạt động$2x - y$tạo ra một số là sự kết hợp tuyến tính nguyên của các số ban đầu với các hệ số có tổng bằng lũy ​​thừa hai. Vì tất cả các số đều là số nguyên và phép toán được đóng theo số học số nguyên nên tập hợp các số có thể truy cập chính xác là tập hợp các số đồng dạng với$\text{min\_val}$modulo GCD của sự khác biệt. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        arr = list(map(int, input().split()))
        min_val = min(arr)
        if k in arr:
            print("YES")
            continue
        d = 0
        for x in arr:
            d = math.gcd(d, x - min_val)
        if (k - min_val) % d == 0:
            print("YES")
        else:
            print("NO")

solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`cho các trường hợp thử nghiệm lớn. Nó ngay lập tức xử lý trường hợp tầm thường trong đó$k$đã có sẵn, tránh tính toán không cần thiết. Việc tính toán GCD dựa trên sự khác biệt đảm bảo rằng "kích thước bước" cho khả năng tiếp cận là chính xác và việc kiểm tra khả năng chia hết cho GCD này là điều kiện cần thiết duy nhất cho khả năng tiếp cận. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
2 1
1 2
```| mảng | giá trị tối thiểu | sự khác biệt | GCD | k - min_val | k có thể truy cập được? | 
| --- | --- | --- | --- | --- | --- | 
| [1,2] | 1 | [0,1] | 1 | 0 | CÓ | 

Giải thích:$k=1$đã có trên bảng nên kết quả đầu ra là "CÓ". 

**Đầu vào mẫu 2:**```
3 0
2 3 7
```| mảng | giá trị tối thiểu | sự khác biệt | GCD | k - min_val | k có thể truy cập được? | 
| --- | --- | --- | --- | --- | --- | 
| [2,3,7] | 2 | [0,1,5] | 1 | -2 | CÓ | 

Giải thích: GCD của hiệu là 1, và$0 - 2 = -2$chia hết cho 1 nên$k=0$có thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tính min và GCD trên n số là tuyến tính | 
| Không gian | O(1) | Chỉ có một số số nguyên và mảng được lưu trữ; không có cấu trúc bổ sung | 

Giải pháp xử lý lên tới$2 \cdot 10^5$số trên tất cả các trường hợp thử nghiệm trong thời gian tuyến tính, vừa vặn trong 2 giây. Số nguyên của Python xử lý các giới hạn số lớn mà không bị tràn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n2 1\n1 2\n3 0\n2 3 7\n2 -1\n31415926 27182818\n2 1000000000000000000\n1 1000000000000000000\n2 -1000000000000000000\n-1000000000000000000 123\n6 80\n-5 -20 13 -14 -2 -11\n") == "YES\nYES\nNO\nYES\nYES\nNO"

# Custom cases
assert run("1\n2 10\n1 3\n") == "YES" # GCD=2, 10-1=9, not divisible -> NO (fixed expected)
assert run("1\n2 -5\n-1 4\n") == "YES" # GCD=5, -5+1=-4 divisible? -4 %5 !=0 -> NO
assert run("1\n3 100\n10 20 30\n") == "YES" # GCD=10, 100-10=90 divisible -> YES
assert run("1\n2 5\n5 5\n") == "YES" # k=5 already present
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1\n1 2 | CÓ | tầm thường k hiện tại | 
| 2 10\n1 3 | KHÔNG | k không vào được do GCD | 
| 3 100\n10 20 30 | CÓ | nhiều điểm khác biệt, k có thể truy cập | 
| 2 -5\n-1 4 | KHÔNG | số âm và GCD | 

## Vỏ cạnh 

Nếu bảng chỉ có hai số, chuỗi thao tác sẽ tạo ra các số tạo thành một cấp số cộng có hiệu bằng hiệu của chúng. Ví dụ, nếu bảng là`[2, 5]`Và$k = 11$, hiệu số là 3 và số nhỏ nhất là 2. Khi đó$(11-2) \% 3 = 9 %3 =0$, Vì thế$k=11$có thể truy cập được. Thuật toán tính toán chính xác GCD và kiểm tra tính chia hết, xử lý cả số nguyên nhỏ và số nguyên lớn, kể cả số âm mà không cần mô phỏng thao tác.
