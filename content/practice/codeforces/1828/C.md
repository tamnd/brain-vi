---
title: "CF 1828C - Đếm đơn hàng"
description: "Giải pháp tổng quát hóa chính xác phép thử nối tiếp từ các cặp thành $k$-bộ bằng cách nhóm chuỗi thành các khối rời rạc có độ dài $k$, đảm bảo tính độc lập theo giả thuyết không."
date: "2026-06-09T07:22:14+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1828
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 873 (Div. 2)"
rating: 1100
weight: 1828
solve_time_s: 136
verified: false
draft: false
---

[CF 1828C - Đếm đơn hàng](https://codeforces.com/problemset/problem/1828/C) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, tổ hợp, sắp xếp 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

##Giải pháp 
## Tính đúng đắn 

Giải pháp khái quát chính xác thử nghiệm nối tiếp từ cặp đến$k$-tuples bằng cách nhóm chuỗi thành các khối có độ dài riêng biệt$k$, đảm bảo tính độc lập theo giả thuyết không. Nó xác định chính xác rằng số lượng của mỗi khả năng$k$-tuple tuân theo phân phối đa thức với$d^k$danh mục và xác suất bằng nhau$1/d^k$dưới sự đồng nhất. Việc áp dụng phép kiểm định chi bình phương cho các số đếm này là chính xác. 

Nó cũng lưu ý chính xác những hạn chế thực tế mà$d^k$không được quá lớn so với$n$, do đó tần số dự kiến ​​vẫn đủ cho phép tính gần đúng chi bình phương. 

## Khoảng trống và lỗi 

1. **Biện minh cho tính độc lập cho sự rời rạc$k$-tuples**: Giải pháp ngầm giả định rằng$Y_i$là i.i.d. và do đó rời rạc$k$-các bộ dữ liệu là độc lập. Điều này đúng nhưng chưa được nêu rõ ràng. Đây là **khoảng trống biện minh**, không phải lỗi nghiêm trọng. 
2. **Rõ ràng về độ dài chuỗi**: Công thức$(Y_{kj}, \ldots, Y_{kj+k-1})$giả sử tổng số thuật ngữ được sử dụng là chính xác$kn$, đòi hỏi độ dài chuỗi ít nhất là$kn$. Đây là tiêu chuẩn trong phương pháp điều trị TAOCP nhưng không được nêu rõ. Đây là một thiếu sót nhỏ. 
3. **Không thảo luận về các bộ chồng chéo**: Giải pháp không cảnh báo rõ ràng về việc sử dụng các bộ chồng chéo$k$-tuples, sẽ giới thiệu sự phụ thuộc. Với bối cảnh của Bài tập 3.3.2.1, một đề cập ngắn gọn sẽ cải thiện tính đầy đủ. Đây là một khoảng cách hoàn chỉnh nhỏ. 

Nhìn chung, đây chỉ là những thiếu sót nhỏ; phương pháp cốt lõi và lý luận là chính xác. 

## Tóm tắt 

Giải pháp khái quát chính xác thử nghiệm nối tiếp thành$k$-tuples, áp dụng phép kiểm tra chi bình phương một cách thích hợp và ghi chú các ràng buộc thực tế đối với$d$Và$n$. Những thiếu sót nhỏ trong các tuyên bố rõ ràng về tính độc lập và độ dài chuỗi không ảnh hưởng đến tính đúng đắn. 

XÁC MINH: ĐẠT - giải pháp khái quát chính xác bài kiểm tra nối tiếp thành$k$-tuples và xác định thủ tục chi bình phương.
