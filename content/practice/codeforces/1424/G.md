---
title: "CF 1424G - Năm"
description: "Việc truy nguyên cho thấy rằng chương trình không thực sự bị lỗi về thuật toán. Nó bị lỗi trước khi bất kỳ tính toán có ý nghĩa nào bắt đầu. Dòng khóa là: Điều này có nghĩa là mã đang hoạt động: và dòng đầu tiên của dữ liệu đầu vào là: rõ ràng không phải là một số nguyên."
date: "2026-06-11T06:04:52+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1424
codeforces_index: "G"
codeforces_contest_name: "Bubble Cup 13 - Finals [Online Mirror, unrated, Div. 2]"
rating: 1300
weight: 1424
solve_time_s: 411
verified: false
draft: false
---

[CF 1424G - Năm](https://codeforces.com/problemset/problem/1424/G) 

**Đánh giá:** 1300 
**Thẻ:** cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 6 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
Việc truy nguyên cho thấy rằng chương trình không thực sự bị lỗi về thuật toán. Nó bị lỗi trước khi bất kỳ tính toán có ý nghĩa nào bắt đầu. 

Dòng chính là:```
ValueError: invalid literal for int() with base 10: '5 6\n'
```Điều này có nghĩa là mã đang thực hiện:```python
t = int(input())
```và dòng đầu tiên của đầu vào là:```
5 6
```rõ ràng không phải là một số nguyên. 

Điều đó cho chúng ta biết giải pháp đã gửi được viết cho một định dạng bài toán hoàn toàn khác, một dạng bài dự kiến ​​sẽ có số lượng bài kiểm tra`t`trên dòng đầu tiên. Đầu vào bạn cung cấp:```
5 6
1 2
2 3
3 4
4 1
1 5
5 1
```trông giống như một vấn đề đồ thị với`n = 5`,`m = 6`rồi 6 cạnh. 

Vì vậy, có hai vấn đề riêng biệt: 

1. Mã hiện tại đang giải quyết vấn đề sai. 
2. Bởi vì nó mong đợi số lượng kiểm tra, nó ngay lập tức gặp sự cố khi đọc`"5 6"`dưới dạng số nguyên. 

Tôi chưa thể cung cấp giải pháp khắc phục vì thiếu báo cáo vấn đề thực tế. 

Chỉ đầu vào thôi là không đủ để xác định thuật toán dự định. Nhiều vấn đề về biểu đồ Codeforces khác nhau bắt đầu bằng:```
n m
u1 v1
u2 v2
...
```và câu trả lời mong đợi`8`là không đủ để xác định duy nhất vấn đề đó là gì. 

Vui lòng cung cấp một trong: 

- mã số/tên vấn đề của Codeforces, hoặc 
- báo cáo vấn đề đầy đủ, hoặc 
- ít nhất là tiêu đề của vấn đề. 

Sau khi bạn cung cấp điều đó, tôi có thể theo dõi lỗi thuật toán và đưa ra giải pháp Python được sửa chữa hoàn chỉnh.
