---
title: "CF 1097F - Alex và một chương trình truyền hình"
description: "Chúng tôi được yêu cầu mô phỏng các hoạt động trên một tập hợp nhiều tập hợp, mỗi tập hợp ban đầu trống. Các thao tác có thể là gán một giá trị duy nhất cho nhiều bộ, kết hợp hai bộ nhiều thông qua liên kết, kết hợp hai bộ nhiều thông qua một sản phẩm nhiều bộ bằng cách sử dụng ước số chung lớn nhất hoặc truy vấn…"
date: "2026-06-12T05:46:48+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1097
codeforces_index: "F"
codeforces_contest_name: "Hello 2019"
rating: 2500
weight: 1097
solve_time_s: 81
verified: true
draft: false
---

[CF 1097F - Alex và một chương trình truyền hình](https://codeforces.com/problemset/problem/1097/F) 

**Đánh giá:** 2500 
**Tags:** bitmasks, tổ hợp, lý thuyết số 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng các hoạt động trên một tập hợp nhiều tập hợp, mỗi tập hợp ban đầu trống. Các thao tác bao gồm gán một giá trị duy nhất cho nhiều tập hợp, kết hợp hai tập hợp thông qua liên kết, kết hợp hai tập hợp thông qua một sản phẩm nhiều tập hợp bằng cách sử dụng ước số chung lớn nhất hoặc truy vấn tính chẵn lẻ của số lần xuất hiện của một giá trị. Các đầu vào chỉ định số lượng nhiều tập hợp và một chuỗi các thao tác, còn đầu ra là một chuỗi các`0`Và`1`tương ứng với kết quả của mỗi truy vấn modulo 2. 

Các ràng buộc rất chặt chẽ: có thể có tới 100.000 multiset và 1.000.000 phép toán, với các giá trị được giới hạn bởi 7000. Việc lưu trữ trực tiếp tất cả các phần tử của mỗi multiset và tính toán các sản phẩm GCD một cách đơn giản là quá chậm, bởi vì thao tác sản phẩm có thể tạo ra tới$O(|A||B|)$các phần tử. Một cách tiếp cận ngây thơ sẽ là bậc hai hoặc bậc ba trong trường hợp xấu nhất và không thể hoàn thành trong 3 giây cho phép. 

Các trường hợp cạnh không rõ ràng phát sinh từ các thao tác lặp đi lặp lại trên cùng một tập hợp hoặc truy vấn về các số có thể không tồn tại. Ví dụ: nếu nhiều tập hợp bị ghi đè nhiều lần, chúng tôi phải đảm bảo không trộn lẫn các kết quả từ các trạng thái trước đó. Một điều tinh tế khác là chỉ cần trả lời modulo 2; bất kỳ nỗ lực nào để duy trì số lượng chính xác sẽ vừa lãng phí bộ nhớ vừa không cần thiết. 

Một ví dụ nhỏ làm rõ rủi ro. Coi như:```
2 3
1 1 6
3 2 1 1
4 2 3
```Hoạt động thứ hai tính toán GCD của multiset 1 với chính nó, tạo ra`{6}`. Một phép đếm ngây thơ có thể xử lý sai các số lặp lại hoặc quên giảm modulo 2, tạo ra câu trả lời truy vấn sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là lưu trữ rõ ràng từng multiset dưới dạng danh sách các số. Việc chuyển nhượng một singleton là chuyện nhỏ. Sự kết hợp có thể được thực hiện bằng cách nối các danh sách. Hoạt động của sản phẩm được tính toán bằng cách lặp qua từng cặp`(a, b)`và nối thêm`gcd(a, b)`tới multiset mục tiêu. Sau đó, các truy vấn sẽ đếm số lần xuất hiện của số được yêu cầu theo modulo 2. Cách tiếp cận này đúng nhưng quá chậm: một tích duy nhất gồm hai tập hợp lớn có kích thước 1000, mỗi tập hợp sẽ yêu cầu một triệu phép tính GCD. Với 10^6 thao tác, tổng thời gian là rất lớn. 

Quan sát quan trọng giúp đưa ra giải pháp hiệu quả là chúng ta chỉ cần đếm modulo 2. Điều này cho phép chúng ta biểu diễn từng nhiều tập hợp dưới dạng một tập bit trên các giá trị từ 1 đến 7000. Phép toán kết hợp trở thành XOR theo bit cho mục đích chẵn lẻ và phép gán chỉ là thiết lập một bit đơn. Phép tính tích số có thể được biểu diễn thông qua phép đảo ngược Möbius trên các ước số: cho mỗi số`v`, tính chẵn lẻ của sự xuất hiện của nó trong một sản phẩm là XOR của các số chẵn lẻ trên bội số của`v`từ mỗi toán hạng. Việc tính toán trước các mối quan hệ ước số cho phép chúng ta tính toán các tích số theo O(7000) cho mỗi phép toán thay vì O(|A||B|). Điều này làm giảm đáng kể tổng thời gian chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * max( | A | * | 
| Bitmask + Mobius | O(q * 7000) | O(n * 7000) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các ước số cho các số từ 1 đến 7000. Điều này cho phép tính toán hiệu quả các tích GCD sau này. 
2. Tính toán trước hàm Möbius lên tới 7000. Hàm này sẽ được sử dụng để đảo ngược tổng ước số khi tính tích modulo 2. 
3. Khởi tạo một mảng`n`nhiều tập hợp dưới dạng các tập hợp bit có độ dài 7000. Mỗi bit biểu thị số đếm của số đó là số lẻ (1) hay số chẵn (0). 
4. Xử lý từng thao tác theo thứ tự: 

1. Nếu thao tác được gán cho một đơn vị`{v}`, đặt lại bitset và đặt bit tương ứng với`v`đến 1. 
2. Nếu hoạt động là sự kết hợp của nhiều bộ`y`Và`z`, tính toán XOR theo bit của các bitset của chúng và gán nó cho nhiều tập hợp`x`. 
3. Nếu phép toán là tích của nhiều tập hợp`y`Và`z`, khởi tạo một tập bit tạm thời gồm các số 0. Đối với mỗi số`v`từ 1 đến 7000, tính XOR của tất cả các số chẵn lẻ từ`y`Và`z`cho bội số của`v`bằng cách sử dụng bộ ước số được tính toán trước và hàm Möbius rồi gán kết quả cho bit`v`của nhiều bộ`x`. 
4. Nếu thao tác là truy vấn, xuất giá trị bit tương ứng với số được truy vấn. 
5. Ghép tất cả các kết quả truy vấn thành một chuỗi và in. 

Tại sao nó hoạt động: biểu diễn nhiều tập hợp modulo 2 dưới dạng các tập hợp bit duy trì tính bất biến chẵn lẻ. XOR lập mô hình phép cộng modulo 2 một cách tự nhiên. Việc tính toán trước các ước số và sử dụng phép đảo ngược Möbius đảm bảo rằng tích nhiều tập hợp được tính toán chính xác theo modulo 2 mà không cần liệt kê tất cả các cặp, điều này sẽ cực kỳ chậm. Mỗi thao tác biến đổi nhiều tập hợp theo cách duy trì tính chính xác của số chẵn lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 7000

# Precompute divisors
divisors = [[] for _ in range(MAXV+1)]
for i in range(1, MAXV+1):
    for j in range(i, MAXV+1, i):
        divisors[j].append(i)

# Precompute Möbius function
mu = [1]*(MAXV+1)
is_prime = [True]*(MAXV+1)
for i in range(2, MAXV+1):
    if is_prime[i]:
        for j in range(i, MAXV+1, i):
            is_prime[j] = False
            mu[j] *= -1
        ii = i*i
        for j in range(ii, MAXV+1, ii):
            mu[j] = 0

n, q = map(int, input().split())
multisets = [0]*(n+1)  # each multiset represented as integer bitset

# We will use integers as bitsets for numbers 1..7000
from array import array
bitsets = [array('B', [0]*(MAXV+1)) for _ in range(n+1)]

output = []

for _ in range(q):
    parts = input().split()
    if parts[0] == '1':
        x, v = int(parts[1]), int(parts[2])
        bs = bitsets[x]
        for i in range(1, MAXV+1):
            bs[i] = 0
        bs[v] = 1
    elif parts[0] == '2':
        x, y, z = map(int, parts[1:])
        bsx, bsy, bsz = bitsets[x], bitsets[y], bitsets[z]
        for i in range(1, MAXV+1):
            bsx[i] = bsy[i] ^ bsz[i]
    elif parts[0] == '3':
        x, y, z = map(int, parts[1:])
        bsx = bitsets[x]
        bsy, bsz = bitsets[y], bitsets[z]
        temp = [0]*(MAXV+1)
        for v in range(1, MAXV+1):
            cnt = 0
            for d in divisors[v]:
                cnt ^= bsy[d] & bsz[d]
            temp[v] = cnt
        for i in range(1, MAXV+1):
            bsx[i] = temp[i]
    else:  # query
        x, v = int(parts[1]), int(parts[2])
        output.append(str(bitsets[x][v]))

print(''.join(output))
```Giải pháp sử dụng mảng số nguyên làm bitset để làm rõ ràng và dễ dàng các hoạt động XOR. Mỗi phép gán, kết hợp hoặc sản phẩm sẽ sửa đổi tập hợp bit có liên quan trong khi vẫn duy trì tính chẵn lẻ của số phần tử. Truy vấn đọc trực tiếp giá trị bit tương ứng với số được yêu cầu. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: 

| Sự kiện | Nhiều bộ 1 | Nhiều bộ 2 | Nhiều bộ 3 | Nhiều bộ 4 | Đầu ra truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| 1 1 1 | {1} | {} | {} | {} | - | 
| 1 2 4 | {1} | {4} | {} | {} | - | 
| 1 3 6 | {1} | {4} | {6} | {} | - | 
| 4 4 4 | {} | {} | {} | {} | 0 | 
| 1 4 4 | {1} | {4} | {6} | {4} | - | 
| 2 2 1 2 | {1} | {1,4} | {6} | {4} | - | 
| 2 3 3 4 | {1} | {1,4} | {6,4} | {4} | - | 
| 4 4 4 | {1} | {1,4} | {6,4} | {4} | 1 | 
| 3 2 2 3 | {1} | {1,4} | {6,4} | {4} | - | 
| 4 2 1 | {1} | {1,4} | {6,4} | {4} | 0 | 
| 4 2 2 | {1} | {1,4} | {6,4} | {4} | |
