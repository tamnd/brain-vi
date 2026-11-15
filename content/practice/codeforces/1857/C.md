---
title: "CF 1857C - Lắp ráp thông qua mức tối thiểu"
description: "Truy nguyên đã cho chúng ta biết rằng chương trình hiện tại không bị lỗi do chính thuật toán đó. Lỗi này xảy ra do mã đọc dữ liệu đầu vào không chính xác và sau đó lập chỉ mục ở phần cuối của mảng."
date: "2026-06-09T00:46:32+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1857
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 891 (Div. 3)"
rating: 1200
weight: 1857
solve_time_s: 131
verified: false
draft: false
---

[CF 1857C - Lắp ráp thông qua mức tối thiểu](https://codeforces.com/problemset/problem/1857/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

## Giải pháp 
Truy nguyên đã cho chúng ta biết rằng chương trình hiện tại không bị lỗi do chính thuật toán đó. Lỗi này xảy ra do mã đọc dữ liệu đầu vào không chính xác và sau đó lập chỉ mục ở phần cuối của mảng. 

Hãy theo dõi trường hợp thử nghiệm đầu tiên: 

đầu vào:```
3
1 3 1
```Mã dường như làm điều gì đó như:```
n = 3
arr = [1, 3, 1]
```điều đó là tốt. 

Bây giờ hãy xem trường hợp thử nghiệm thứ hai:```
2
10
```Mã mong đợi`n = 2`phần tử, nhưng dòng tiếp theo chỉ chứa một số nguyên:```
arr = [10]
```Sau đó nó thực thi:```
for i in range(1, n):
    current = max(current, arr[i])
```Khi`i = 1`, nó cố gắng truy cập:```
arr[1]
```Nhưng`arr`có độ dài 1, gây ra:```
IndexError: list index out of range
```Điều này có nghĩa là toàn bộ định dạng đầu vào đang được diễn giải không chính xác. Đầu vào mẫu rõ ràng **không** mô tả một mảng có độ dài`n`ở dòng tiếp theo. Trên thực tế, kết quả đầu ra dự kiến:```
1 3 3
10 10
7 5 3 12
2 2 2 2 2
0 -2 0 3 5
```hoàn toàn không khớp với hành vi của thuật toán tiền tố tối đa. 

Nếu không có báo cáo vấn đề thực tế thì không thể xác định được thuật toán dự kiến ​​chỉ từ mẫu. Nhiều bài toán hoàn toàn khác nhau có thể tạo ra cùng một dạng mẫu. 

Giải pháp trước đó về cơ bản là không hợp lệ vì: 

1. Nó giả sử định dạng đầu vào là:```
n
array of length n
```nhưng mẫu mâu thuẫn với điều đó. 
2. Nó giả định đầu ra được yêu cầu là chuỗi tiền tố tối đa, nhưng đầu ra mẫu không hỗ trợ cách diễn giải đó. 
3. Sự cố xảy ra trước bất kỳ tính toán thuật toán có ý nghĩa nào vì`n`không khớp với số lượng giá trị được đọc. 

Để cung cấp giải pháp chính xác, tôi cần báo cáo vấn đề ban đầu (hoặc ít nhất là thông số đầu vào chính xác). Chỉ đầu vào/đầu ra mẫu là không đủ để xây dựng lại thuật toán dự định một cách đáng tin cậy. 

Vui lòng cung cấp báo cáo vấn đề và sau đó tôi có thể: 

1. Xác định chính xác lý do giải pháp hiện tại sai. 
2. Theo dõi lỗi trên mẫu. 
3. Tạo giải pháp Python 3 đã sửa lỗi với I/O nhanh.
