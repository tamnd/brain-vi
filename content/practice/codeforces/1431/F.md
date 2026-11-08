---
title: "CF 1431F - Sự cố mạng thần kinh"
description: "Vấn đề mấu chốt là đây là một vấn đề mang tính xây dựng. Đầu ra hiển thị trong câu lệnh chỉ là một chuỗi thao tác hợp lệ, không phải là đầu ra chính xác duy nhất. Đầu ra chương trình của bạn khác với đầu ra mẫu nhưng điều đó không có nghĩa là nó sai."
date: "2026-06-11T05:09:51+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1431
codeforces_index: "F"
codeforces_contest_name: "Kotlin Heroes 5: ICPC Round"
rating: 2100
weight: 1431
solve_time_s: 180
verified: false
draft: false
---

[CF 1431F - Sự cố mạng thần kinh](https://codeforces.com/problemset/problem/1431/F) 

**Đánh giá:** 2100 
**Tags:** *đặc biệt, tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 3 phút 
**Đã xác minh:** không 

##Giải pháp 
Vấn đề mấu chốt là đây là một **vấn đề mang tính xây dựng**. Đầu ra hiển thị trong câu lệnh chỉ là một chuỗi thao tác hợp lệ, không phải là đầu ra chính xác duy nhất. 

Đầu ra chương trình của bạn khác với đầu ra mẫu, nhưng điều đó **không** có nghĩa là nó sai. 

Đối với Codeforces 1439A2, thẩm phán sẽ kiểm tra: 

1. Số lượng thao tác không vượt quá giới hạn cho phép. 
2. Mọi thao tác in đều hợp lệ. 
3. Sau khi áp dụng tất cả các thao tác, bảng sẽ trở thành toàn số 0. 

Giám khảo **không** so sánh văn bản đầu ra của bạn với đầu ra mẫu. 

Nhìn vào đầu ra của bạn: 

- Test case 1 sử dụng 1 thao tác thay vì 1 thao tác của mẫu. Không sao đâu. 
- Test case 2 sử dụng 6 thao tác thay vì 2 thao tác của mẫu. Điều đó vẫn ổn nếu bảng cuối cùng trở thành số không. 
- Test case 3 sử dụng 10 thao tác thay vì 4 thao tác của mẫu. Cũng ổn. 
- Test case 4 sử dụng 14 thao tác thay vì 4 thao tác của mẫu. Cũng ổn. 
- Test case 5 sử dụng 2 thao tác, khớp số lượng mẫu nhưng với các bước di chuyển khác nhau. 

Không có gì trong vấn đề yêu cầu phải khớp các thao tác mẫu. 

Lỗi thực sự gần như chắc chắn nằm ở **bộ khai thác thử nghiệm** chứ không phải ở thuật toán. Một lỗi phổ biến là như sau:```
assert run(sample_input) == sample_output
```không hợp lệ đối với các vấn đề mang tính xây dựng. 

Thay vào đó, người kiểm tra phải:```
1. Parse the produced operations.
2. Verify each operation is legal.
3. Apply the operations to the original table.
4. Verify the resulting table contains only zeros.
5. Verify the number of operations is <= n*m.
```Nếu không có mã giải pháp ban đầu thì không có bằng chứng nào cho thấy bản thân thuật toán không chính xác. Đầu ra bạn đã đăng trông giống hệt loại đầu ra hợp lệ thay thế mà các giải pháp được chấp nhận thường tạo ra cho vấn đề này. 

Vì vậy chẩn đoán là: 

- **Thuật toán không được chứng minh là sai.** 
- **So sánh với đầu ra mẫu là sai.** 
- **Các vấn đề mang tính xây dựng cần có trình xác nhận chứ không phải xác nhận kết quả đầu ra chính xác.** 

Nếu bạn cung cấp mã nguồn Python thực tế, tôi có thể kiểm tra nó và xác định xem nó có thực sự vi phạm các yêu cầu của vấn đề hay vấn đề duy nhất là phương pháp thử nghiệm không chính xác.
