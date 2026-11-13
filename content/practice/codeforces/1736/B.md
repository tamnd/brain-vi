---
title: "CF 1736B - Chơi với GCD"
description: "Chúng ta được cho một mảng a. Chúng ta muốn biết liệu có tồn tại một mảng b khác có độ dài n + 1 sao cho mọi phần tử của a là GCD của hai phần tử lân cận của b: [ ai = gcd(bi, b{i+1}) ] cho mọi vị trí hay không."
date: "2026-06-09T18:02:54+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1736
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 825 (Div. 2)"
rating: 1200
weight: 1736
solve_time_s: 270
verified: true
draft: false
---

[CF 1736B - Chơi với GCD](https://codeforces.com/problemset/problem/1736/B) 

**Đánh giá:** 1200 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 4 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`. Chúng tôi muốn biết liệu có tồn tại một mảng khác không`b`chiều dài`n + 1`sao cho mọi phần tử của`a`là GCD của hai phần tử lân cận của`b`: 

[ 
a_i = \gcd(b_i, b_{i+1}) 
] 

cho mọi vị trí. 

Nhiệm vụ không phải là xây dựng một chuỗi hợp lệ tùy ý bằng cách thử và sai. Chúng ta chỉ cần xác định xem có tồn tại ít nhất một chuỗi như vậy hay không. 

Tổng độ dài của tất cả các mảng trong tất cả các trường hợp thử nghiệm tối đa là (10^5). Điều đó ngay lập tức loại trừ mọi tìm kiếm trên các giá trị có thể có của (b). Ngay cả một thuật toán bậc hai cũng sẽ có rủi ro. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế là các yêu cầu GCD lân cận tương tác với nhau. thỏa mãn 

[ 
\gcd(b_i,b_{i+1})=a_i 
] 

và 

[ 
\gcd(b_{i+1},b_{i+2})=a_{i+1} 
] 

đồng thời đặt ra các hạn chế đối với giá trị được chia sẻ (b_{i+1}). Một lựa chọn cục bộ có thể dễ dàng phá vỡ một ràng buộc lân cận. 

Hãy xem xét: 

[ 
a=[4,2,4] 
] 

Câu trả lời là`NO`. Một cách tiếp cận ngây thơ có thể nhận thấy rằng mọi cặp liền kề đều có vẻ tương thích và được chấp nhận không chính xác. GCD ở giữa buộc giá trị được chia sẻ phải chứa các yếu tố làm cho một trong các GCD lân cận lớn hơn yêu cầu. 

Một trường hợp cạnh quan trọng khác là (n=1). Bất kỳ số nguyên dương nào cũng có thể được biểu thị dưới dạng GCD của hai số bằng nhau, vì vậy câu trả lời luôn là`YES`. 

Ví dụ: 

[ 
a=[343] 
] 

Chọn (b=[343,343]). 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng xây dựng trực tiếp các giá trị của (b). Vì các phần tử của (b) không bị giới hạn theo bất kỳ cách hữu ích nào nên không gian tìm kiếm là rất lớn. Ngay cả việc hạn chế các ứng cử viên ở ước số của các giá trị gần đó cũng không dẫn đến một thuật toán thực tế. 

Quan sát quan trọng là có một chuỗi ứng cử viên chuẩn. 

Giả sử chúng ta đã biết tất cả các giá trị GCD. Một số phải tham gia cả hai 

[ 
a_i=\gcd(b_i,b_{i+1}) 
] 

và 

[ 
a_{i+1}=\gcd(b_{i+1},b_{i+2}) 
] 

nên chứa tất cả các thừa số nguyên tố được yêu cầu bởi cả hai ràng buộc liền kề. Sự lựa chọn nhỏ nhất như vậy là 

[ 
\operatorname{lcm}(a_i,a_{i+1}). 
] 

Điều này gợi ý việc xây dựng 

[ 
b_1=a_1, 
] 

[ 
b_{n+1}=a_n, 
] 

và cho các vị trí nội bộ 

[ 
b_i=\tên toán tử{lcm}(a_{i-1},a_i). 
] 

Nếu tồn tại bất kỳ chuỗi hợp lệ nào thì cấu trúc này là ứng cử viên mạnh nhất có thể. Sau đó chúng tôi chỉ cần xác minh xem liệu nó có thực sự tái tạo tất cả các GCD cần thiết hay không. 

Với mọi vị trí (i), hãy tính 

[ 
\gcd(b_i,b_{i+1}). 
] 

Nếu nó bằng (a_i) ở mọi nơi thì câu trả lời là`YES`. Nếu không thì nó là`NO`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| LCM Xây dựng + Thẩm định | (O(n \log V)) | (O(n)) | Đã chấp nhận | 

Ở đây (V) là giá trị mảng tối đa. 

## Hướng dẫn thuật toán 

1. Đọc mảng (a). 
2. Nếu (n=1), trả lời`YES`. 
3. Xây dựng mảng phụ trợ (b). 

Đặt 

[ 
b_1=a_1, 
\qquad 
b_{n+1}=a_n. 
] 
4. Với mọi vị trí bên trong (2 \le i \le n), đặt 

[ 
b_i=\tên toán tử{lcm}(a_{i-1},a_i). 
] 

Đây là giá trị nhỏ nhất có thể chia đồng thời cho cả hai yêu cầu GCD lân cận. 
5. Với mọi (i), hãy tính 

[ 
g_i=\gcd(b_i,b_{i+1}). 
] 
6. Nếu có (g_i \ne a_i), trả lời`NO`. 
7. Ngược lại hãy trả lời`YES`. 

### Tại sao nó hoạt động 

Mọi phần tử bên trong của (b) phải chia hết cho cả hai giá trị GCD lân cận. Bội số chung nhỏ nhất là số nhỏ nhất có thuộc tính đó. 

Nếu việc xây dựng LCM đã tạo ra GCD lớn hơn yêu cầu ở một số vị trí thì mọi ứng cử viên hợp lệ khác sẽ chứa ít nhất các thừa số nguyên tố giống nhau và không thể giảm GCD đó trở lại giá trị mong muốn. Do đó sự thất bại của công trình này chứng tỏ là không thể. 

Ngược lại, nếu tất cả các GCD được tính toán khớp với mảng mục tiêu thì chính chuỗi được xây dựng đó là một nhân chứng hợp lệ. Vì vậy việc kiểm tra là cần thiết và đủ. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def lcm(a, b):
    return a // gcd(a, b) * b

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    b = [0] * (n + 1)
    b[0] = a[0]
    b[n] = a[-1]

    for i in range(1, n):
        b[i] = lcm(a[i - 1], a[i])

    ok = True

    for i in range(n):
        if gcd(b[i], b[i + 1]) != a[i]:
            ok = False
            break

    print("YES" if ok else "NO")
```Giai đoạn xây dựng lấp đầy các vị trí nội bộ bằng các giá trị LCM. Giai đoạn xác minh sẽ kiểm tra mọi GCD được yêu cầu chính xác một lần. 

Công thức LCM 

[ 
\operatorname{lcm}(x,y)=\frac{x}{\gcd(x,y)}y 
] 

tránh các vấn đề tràn trong các ngôn ngữ có số nguyên kích thước cố định. Python tự động xử lý các số nguyên lớn, nhưng công thức tương tự là thông lệ tiêu chuẩn. 

Việc lập chỉ mục là nơi duy nhất thường xảy ra lỗi từng cái một. Mảng phụ có độ dài (n+1), trong khi mảng ban đầu có độ dài (n). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

[ 
a=[4,2] 
] 

Xây dựng: 

| Vị trí | Giá trị | 
| --- | --- | 
| (b_1) | 4 | 
| (b_2) | lcm(4,2)=4 | 
| (b_3) | 2 | 

Xác minh: 

| Kiểm tra | Giá trị | 
| --- | --- | 
| gcd(4,4) | 4 | 
| gcd(4,2) | 2 | 

Cả hai đều phù hợp với mảng mục tiêu. 

Trả lời:`YES`. 

### Ví dụ 2 

[ 
a=[4,2,4] 
] 

Xây dựng: 

| Vị trí | Giá trị | 
| --- | --- | 
| (b_1) | 4 | 
| (b_2) | 4 | 
| (b_3) | 4 | 
| (b_4) | 4 | 

Xác minh: 

| Kiểm tra | Giá trị | 
| --- | --- | 
| gcd(4,4) | 4 | 
| gcd(4,4) | 4 | 
| gcd(4,4) | 4 | 

Giá trị ở giữa phải là 2 nhưng trở thành 4. 

Trả lời:`NO`. 

Ví dụ này cho thấy tại sao khả năng tương thích cục bộ là không đủ. Phần tử được chia sẻ buộc GCD ở giữa phải lớn hơn yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n \log V)) | Mỗi phép tính LCM và GCD tốn thời gian logarit | 
| Không gian | (O(n)) | Mảng phụ trợ (b) | 

Vì tổng của tất cả (n) giá trị tối đa là (10^5), nên thuật toán dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```
# sample cases

assert solve_case([343]) == "YES"
assert solve_case([4, 2]) == "YES"
assert solve_case([4, 2, 4]) == "NO"
assert solve_case([1, 1, 1, 1]) == "YES"

# minimum size
assert solve_case([1]) == "YES"

# all equal
assert solve_case([6, 6, 6, 6]) == "YES"

# common failure pattern
assert solve_case([2, 6, 2]) == "YES"

# impossible middle constraint
assert solve_case([6, 2, 6]) == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [1] | CÓ | Kích thước tối thiểu | 
| [6,6,6,6] | CÓ | Giá trị lặp lại | 
| [2,6,2] | CÓ | Công trình hợp lệ không tầm thường | 
| [6,2,6] | KHÔNG | GCD giữa trở nên quá lớn | 

## Vỏ cạnh 

Đối với (n=1), cấu trúc thuật toán (b=[a_1,a_1]). Bước xác minh tính toán chính xác một GCD, bằng (a_1). Câu trả lời luôn là`YES`. 

Đối với các mảng trong đó các giá trị liền kề phân chia lẫn nhau, chẳng hạn như 

[ 
[8,4,2], 
] 

việc xây dựng LCM mang lại 

[ 
[8,8,4,2]. 
] 

Mọi GCD xác minh đều khớp với chuỗi mục tiêu nên thuật toán sẽ chấp nhận chính xác. 

Đối với các mảng như 

[ 
[4,2,4], 
] 

công trình tạo ra giá trị ở giữa buộc phải chứa hệ số (2^2) từ cả hai phía. Việc xác minh phát hiện ra rằng GCD kết quả là (4) thay vì (2), chứng tỏ rằng không tồn tại chuỗi hợp lệ.
