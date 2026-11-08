---
title: "CF 1406C - Trung tâm cắt liên kết"
description: "Vấn đề thực sự không nằm ở thuật toán. Kết quả hiển thị là: có nghĩa là chương trình không tạo ra kết quả nào cả. Điều đó cho thấy vấn đề về triển khai chứ không phải vấn đề toán học."
date: "2026-06-11T08:02:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1406
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 670 (Div. 2)"
rating: 1700
weight: 1406
solve_time_s: 492
verified: false
draft: false
---

[CF 1406C - Centroid cắt liên kết](https://codeforces.com/problemset/problem/1406/C) 

**Đánh giá:** 1700 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, đồ thị, cây 
**Thời gian giải:** 8 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
Vấn đề thực sự không nằm ở thuật toán. Đầu ra hiển thị như sau:```

```có nghĩa là chương trình không tạo ra kết quả nào cả. Điều đó cho thấy vấn đề về triển khai chứ không phải vấn đề toán học. 

Nhìn vào mã trước đó, khai thác thử nghiệm mẫu có chứa:```python
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    ...
```nhưng bản thân giải pháp được viết dưới dạng mã cấp cao nhất:```python
t = int(input())
for _ in range(t):
    ...
```Nếu mã đó được sao chép vào một môi trường mong đợi`solve()`chức năng hoặc trộn lẫn với khai thác thử nghiệm, nó có thể dễ dàng dẫn đến không có gì được thực thi. 

Quan trọng hơn, cách tiếp cận toán học là chính xác: 

hãy để```
x = a & b
```Sau đó```
(a xor x) + (b xor x)
```được giảm thiểu và nó đơn giản hóa để```
a xor b
```bởi vì mỗi bit trong đó cả hai số đều có`1`có thể được loại bỏ cùng một lúc. 

Ví dụ:```
a = 6  = 0110
b = 12 = 1100

a xor b = 1010 = 10
```phù hợp với mẫu. 

Do đó, một giải pháp đơn giản và trực tiếp hơn chỉ là:```
answer = a xor b
```## Giải pháp Python 3 đúng```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        print(a ^ b)

solve()
```### Xác minh trên mẫu 

đầu vào:```
6
6 12
4 9
59 832
28 14
4925 2912
1 1
```Đầu ra:```
10
13
891
18
6237
0
```phù hợp chính xác với đầu ra dự kiến. 

### Tại sao`a ^ b`là câu trả lời 

Đối với mỗi bit độc lập: 

- Nếu cả hai bit đều`0`, đóng góp là`0`. 
- Nếu cả hai bit đều`1`, chọn bit tương ứng của`x`BẰNG`1`, đóng góp cả hai`0`. 
- Nếu các bit khác nhau, một trong`(a xor x)`Và`(b xor x)`phải chứa bit đó bất kể lựa chọn`x`, vì vậy phần đóng góp chính xác là giá trị bit đó. 

Vì vậy chỉ những vị trí mà`a`Và`b`khác nhau vẫn ở mức tối thiểu. Những vị trí đó chính xác là các bit được thiết lập của`a ^ b`. 

Vì thế:```
min_x ((a xor x) + (b xor x)) = a xor b
```và giải pháp là một thao tác XOR duy nhất cho mỗi trường hợp thử nghiệm.
