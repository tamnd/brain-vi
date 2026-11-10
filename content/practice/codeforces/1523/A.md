---
title: "CF 1523A - Trò chơi cuộc sống"
description: "Đầu ra trống biểu thị sự cố thời gian chạy trước khi bất kỳ hoạt động in nào xảy ra, không phải lỗi logic trong phép toán. Theo dõi quá trình thực thi trên đầu vào: Đầu tiên chương trình đọc t = 3, sau đó xử lý ba trường hợp kiểm thử."
date: "2026-06-10T17:44:41+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1523
codeforces_index: "A"
codeforces_contest_name: "Deltix Round, Spring 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 800
weight: 1523
solve_time_s: 720
verified: false
draft: false
---

[CF 1523A - Trò chơi cuộc sống](https://codeforces.com/problemset/problem/1523/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 12 phút 
**Đã xác minh:** không 

##Giải pháp 
Đầu ra trống biểu thị sự cố trong thời gian chạy trước khi bất kỳ hoạt động in nào xảy ra, không phải lỗi logic trong phép toán. 

Theo dõi việc thực hiện trên đầu vào:```
3
3
100
25
```Chương trình đầu tiên đọc`t = 3`, sau đó xử lý ba trường hợp thử nghiệm. Tính toán dự kiến ​​cho mỗi trường hợp thử nghiệm là: 

Đối với mỗi`k`, tính:$$\frac{100}{\gcd(k, 100)}$$Lỗi xảy ra sớm hơn bất kỳ suy luận số học nào: chương trình không bao giờ đạt được`print`. Nguyên nhân thông thường dẫn đến hiện tượng này (không có dấu vết nào được hiển thị trong các khai thác kiểu thẩm phán, chỉ có đầu ra trống) là thiếu ký hiệu nhập hoặc ký hiệu không xác định trong đường dẫn thực thi thực tế, phổ biến nhất là`math.gcd`hoặc`gcd`đang được sử dụng mà không cần nhập khẩu`math`hoặc nhập khẩu`gcd`một cách rõ ràng. 

Trong nhiều mẫu kiểu Codeforces, giải pháp sử dụng`gcd`trực tiếp, nhưng nếu nó được viết là`math.gcd`không có`import math`, hoặc như`gcd`không có`from math import gcd`, quá trình thực thi sẽ dừng ngay lập tức. 

Cách khắc phục chính xác là đảm bảo hàm gcd được nhập đúng cách và chương trình không bao giờ phụ thuộc vào tính khả dụng của vùng tên tiềm ẩn. 

Bản thân thuật toán là đúng: số lít tối thiểu đạt được bằng cách lấy cặp số nguyên nhỏ nhất tỷ lệ với$k : (100-k)$, rút ​​gọn thành phép chia cho ước chung lớn nhất của chúng. 

## Giải pháp Python đã được sửa```python
import sys
from math import gcd

input = sys.stdin.readline

t = int(input())
for _ in range(t):
    k = int(input())
    g = gcd(k, 100)
    print(100 // g)
```Phiên bản này đảm bảo: 

-`gcd`được xác định trong mọi môi trường thực thi. 
- Mỗi test case được xử lý độc lập trong O(1). 
- Đầu ra được tạo ra ngay lập tức cho mỗi trường hợp, tránh các vấn đề về bộ đệm trong các lần chạy thoát sớm bị hỏng. 

Logic vẫn không thay đổi: giảm tỷ lệ$k/100$với điều kiện thấp nhất; mẫu số đưa ra tổng số thao tác tối thiểu cần thiết để đạt được tỷ lệ phần trăm chính xác đó bằng cách sử dụng số gia đơn vị.
