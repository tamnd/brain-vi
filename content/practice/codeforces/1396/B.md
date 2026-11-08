---
title: "CF 1396B - Trò Chơi Ném Đá"
description: "Truy nguyên cho chúng ta biết chính xác điều gì đã xảy ra: Đoạn mã đang cố đọc một cạnh của cây dưới dạng hai số nguyên u v. Nhưng đầu vào thực tế là: Hãy theo dõi nó: Trường hợp thử nghiệm đầu tiên: Sau đó, dòng tiếp theo là một mảng có độ dài 7."
date: "2026-06-11T09:34:41+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1396
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 666 (Div. 1)"
rating: 1800
weight: 1396
solve_time_s: 706
verified: true
draft: false
---

[CF 1396B - Trò chơi bị ném đá](https://codeforces.com/problemset/problem/1396/B) 

**Đánh giá:** 1800 
**Tags:** vũ phu, thuật toán xây dựng, trò chơi, lòng tham 
**Thời gian giải:** 11 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
Truy nguyên cho chúng ta biết chính xác điều gì đã xảy ra:```python
u, v = map(int, input().split())
ValueError: too many values to unpack (expected 2)
```Mã đang cố đọc **cạnh của cây** dưới dạng hai số nguyên`u v`. 

Nhưng đầu vào thực tế là:```
3
7
4 6 11 11 15 18 20
4
10 10 10 11
3
1 1 1000000000
```Hãy theo dõi nó:```
t = 3
```Trường hợp thử nghiệm đầu tiên:```
n = 7
```Sau đó, dòng tiếp theo là```
4 6 11 11 15 18 20
```đó là một mảng có độ dài 7. 

Mã được gửi giả định không chính xác rằng sau khi đọc`n`, nó nên đọc`n-1`mép cây. Nó thực thi:```python
u, v = map(int, input().split())
```TRÊN```
4 6 11 11 15 18 20
```chứa **7 số nguyên**, không phải 2. 

Python không thể giải nén 7 giá trị thành hai biến nên nó sẽ đưa ra:```
ValueError: too many values to unpack
```Vì vậy đây không phải là một lỗi nhỏ trong quá trình thực hiện. Chương trình đang giải quyết một vấn đề hoàn toàn khác, một vấn đề liên quan đến cây, trong khi đầu vào mô tả rõ ràng các mảng. 

Sản lượng dự kiến```
2 3 6
-1
1 2 3
```cũng không phải là câu trả lời CÓ/KHÔNG và cũng không phải là câu trả lời dọn cây. Nó hoàn toàn tương ứng với một vấn đề khác của Codeforces. 

Vì câu lệnh/mã vấn đề thực tế tạo ra kết quả này bị thiếu nên không có đủ thông tin để xây dựng lại thuật toán chính xác. Chúng tôi chỉ có:```
Input:
3
7
4 6 11 11 15 18 20
4
10 10 10 11
3
1 1 1000000000

Output:
2 3 6
-1
1 2 3
```Nhiều vấn đề hoàn toàn khác nhau có thể tạo ra kết quả đó. 

Để cung cấp giải pháp khắc phục, tôi cần báo cáo vấn đề thực tế (hoặc ít nhất là mã số/tiêu đề vấn đề của Codeforces). Hiện tại, điều duy nhất có thể được chẩn đoán chắc chắn là: 

1. Mã đã gửi mong đợi một cái cây. 
2. Đầu vào chứa mảng. 
3. Trình phân tích cú pháp bị lỗi ngay trên dòng mảng đầu tiên. 
4. Thuật toán dành cho một vấn đề sai, do đó không thể khắc phục được mục tiêu nếu không biết vấn đề dự định. 

Vui lòng cung cấp báo cáo vấn đề hoặc ID vấn đề của Codeforces để tôi có thể rút ra và triển khai giải pháp chính xác.
