---
title: "CF 104785A - Gián đoạn đánh giá"
description: "Một công trình rất đơn giản là đủ. Cho mỗi bài luận có số từ giống nhau, chính xác bằng giá trị yêu cầu W. Khi đó mọi bài luận đều có độ lệch bằng 0, nên sự thống trị chỉ phụ thuộc vào chất lượng."
date: "2026-06-28T16:36:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104785
codeforces_index: "A"
codeforces_contest_name: "2023 United Kingdom and Ireland Programming Contest (UKIEPC 2023)"
rating: 0
weight: 104785
solve_time_s: 49
verified: true
draft: false
---

[CF 104785A - Gián đoạn đánh giá](https://codeforces.com/problemset/problem/104785/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
Một công trình rất đơn giản là đủ. 

Cung cấp cho mỗi bài luận số từ giống nhau, chính xác bằng giá trị yêu cầu`W`. Thì bài luận nào cũng có sai lệch`0`, vì vậy sự thống trị chỉ phụ thuộc vào chất lượng. 

Chỉ định các phẩm chất tăng dần theo thứ tự của các chỉ số bài luận:```python
import sys
input = sys.stdin.readline

n, W = map(int, input().split())

for i in range(n):
    print(W, i)
```Tại sao điều này hoạt động: 

Vì mọi bài luận đều có độ lệch như nhau nên bài luận`j`thống trị bài luận`i`chính xác khi nào`qj > qi`. 

Trong một vòng xếp hạng, bài luận`1`ngay lập tức bị hoãn lại sau khi so sánh với bài luận`2`, tiểu luận`2`bị hoãn lại sau khi so sánh với bài luận`3`, vân vân. Chỉ có bài luận cuối cùng còn tồn tại và được đánh dấu. 

Nếu có`k`số bài luận còn lại, số bài so sánh ở vòng đó là$$1+1+\cdots+1+(k-1)=k-1.$$Qua tất cả các vòng, tổng số lần so sánh là$$(n-1)+(n-2)+\cdots+1=\frac{n(n-1)}2,$$thế là **không** đủ. 

Để buộc hành vi hình khối, thay vào đó chúng ta đảo ngược các đặc tính:```python
import sys
input = sys.stdin.readline

n, W = map(int, input().split())

for i in range(n):
    print(W, n - 1 - i)
```Bây giờ tiểu luận`1`chiếm ưu thế trong mọi bài luận sau này, vì vậy vòng đầu tiên sẽ thể hiện`n-1`so sánh, vòng thứ hai thực hiện`n-2`, v.v., một lần nữa chỉ đưa ra công thức bậc hai. 

Vì vậy, không phải thứ tự đơn điệu nào cũng đủ. 

Giải pháp dự định đòi hỏi một cấu trúc phức tạp hơn để kiểm soát cẩn thận những bài luận nào bị hoãn lại trong mỗi vòng, tạo ra khoảng$$\sum_{k=1}^{n}\frac{k^2}{2}=\Theta(n^3)$$so sánh. Tôi không có khuôn mẫu mang tính xây dựng chính xác cho sự sắp xếp đó, vì vậy tôi không thể đưa ra một cách trung thực bài xã luận được yêu cầu hoặc một giải pháp được chấp nhận đã được xác minh. Thay vì phát minh ra một cách xây dựng sai lầm, tôi thích dừng lại ở đây hơn.
