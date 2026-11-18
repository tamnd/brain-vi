---
title: "CF 2038B - Làm cho nó bình đẳng"
description: "Lỗi bạn hiển thị không đủ để chẩn đoán thuật toán. Hãy theo dõi những gì đã xảy ra: Đầu vào chứa 5 trường hợp thử nghiệm: Chương trình chỉ tạo ra: thay vì: Điều này có nghĩa là mã đã xử lý thành công trường hợp thử nghiệm đầu tiên và sau đó kết thúc trước khi xử lý…"
date: "2026-06-08T10:34:38+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2038
codeforces_index: "B"
codeforces_contest_name: "2024-2025 ICPC, NERC, Southern and Volga Russian Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 2100
weight: 2038
solve_time_s: 124
verified: false
draft: false
---

[CF 2038B - Làm cho nó bình đẳng](https://codeforces.com/problemset/problem/2038/B) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, toán học 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

## Giải pháp 
Lỗi bạn hiển thị không đủ để chẩn đoán thuật toán. 

Hãy theo dõi những gì đã xảy ra: 

Đầu vào chứa **5 trường hợp thử nghiệm**:```
5
2
2 1
5
2 3 4 5 1
5
2 1 4 2 3
5
4 1 1 5 4
10
4 3 9 1 6 7 9 10 10 3
```Chương trình chỉ sản xuất:```
2
```thay vì:```
2
2
5
5
5
```Điều này có nghĩa là mã đã xử lý thành công **trường hợp thử nghiệm đầu tiên** và sau đó chấm dứt trước khi xử lý bốn trường hợp còn lại. 

Điều đó gợi ý rõ ràng một trong những lỗi triển khai sau: 

1. A`return`câu lệnh bên trong vòng lặp trên các trường hợp thử nghiệm. 
2. A`break`câu lệnh thoát sau câu trả lời đầu tiên. 
3. Đọc dữ liệu đầu vào không chính xác và đạt EOF sớm. 
4. In đáp án ngay rồi thoát`solve()`. 
5. Xây dựng danh sách câu trả lời không chính xác và chỉ thêm kết quả đầu tiên. 

Tuy nhiên, nếu không có mã thực tế thì không thể xác định được điều nào đã xảy ra. 

Không giống như lỗi cú pháp hoặc ngoại lệ thời gian chạy, kết quả đầu ra```
2
```không tiết lộ thuật toán. Nhiều vấn đề hoàn toàn khác nhau có thể có cùng mẫu và kết quả mong đợi. Tôi không thể xây dựng lại giải pháp dự định chỉ từ mẫu. 

Vui lòng cung cấp: 

- giải pháp Python hiện tại, hoặc 
- tuyên bố chính xác về Codeforce/vấn đề, 

và tôi có thể: 

1. theo dõi việc thực hiện trên mẫu, 
2. xác định lỗi chính xác, 
3. giải thích tại sao chỉ xử lý test case đầu tiên, 
4. cung cấp giải pháp Python 3 hoàn chỉnh đã được sửa chữa.
