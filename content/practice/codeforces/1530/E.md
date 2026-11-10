---
title: "CF 1530E - Tối thiểu"
description: "Giải pháp đang cố gắng chứng minh công thức (25) thông qua phép nội suy đa thức của các giá trị đa thức của tích."
date: "2026-06-10T16:55:59+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1530
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 733 (Div. 1 + Div. 2, based on VK Cup 2021 - Elimination (Engine))"
rating: 2100
weight: 1530
solve_time_s: 153
verified: false
draft: false
---

[CF 1530E - Minimax](https://codeforces.com/problemset/problem/1530/E) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, tham lam, chuỗi 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Tính đúng đắn 

Giải pháp đang cố gắng chứng minh công thức (25) thông qua phép nội suy đa thức của các giá trị đa thức của tích. Chiến lược chung, sử dụng đánh giá ở$2r+1$điểm và nghịch đảo Vandermonde, hợp lý về mặt toán học và dẫn đến sự đồng nhất tiêu chuẩn rằng các hệ số của bậc-$2r$đa thức là sự kết hợp tuyến tính của các giá trị của nó tại$2r+1$những điểm khác biệt. 

Tuy nhiên, bài tập trong TAOCP Mục 4.3.3 không chỉ đơn thuần là yêu cầu phát biểu lại nguyên tắc nội suy. Trong phần đó, công thức (25) đề cập đến một danh tính có cấu trúc cụ thể bắt nguồn từ Thuật toán T, trong đó các hệ số của$W(x)$được thể hiện rõ ràng dưới dạng các giá trị được tính toán$W(0), \ldots, W(2r)$thông qua việc xây dựng cơ sở sai phân hữu hạn / giai thừa được trình bày trước đó trong các phương trình (14)-(18). Bằng chứng được mong đợi là sự chuyển đổi rõ ràng này là chính xác. 

Thay vào đó, giải pháp được đưa ra chứng minh một tuyên bố yếu hơn và tổng quát hơn: sự tồn tại của một phép biến đổi tuyến tính thông qua$A^{-1}$. Điều này không thiết lập dạng cụ thể của (25) trong văn bản, cũng không kết nối với cơ sở giai thừa hoặc quy trình tái thiết cụ thể được sử dụng trong phần này. 

Vì vậy, ngay cả khi câu lệnh đại số tuyến tính là đúng thì nó cũng không khớp với danh tính được yêu cầu. 

## Khoảng trống và lỗi 

Vấn đề chính là khoảng cách biện minh trở thành sự không phù hợp về mặt cấu trúc với kết quả mục tiêu. 

Đối số thay thế cấu trúc sai phân hữu hạn cơ bản (25) bằng đối số đảo ngược Vandermonde. Mặc dù khả năng nghịch đảo của Vandermonde đảm bảo sự tồn tại và tính duy nhất của các hệ số, nhưng nó không tái tạo lại công thức rõ ràng (25) như được đưa ra trong TAOCP, công thức này phụ thuộc vào việc đánh giá và chuyển đổi có cấu trúc giữa cơ sở lũy thừa và cơ sở giai thừa giảm. Bước này không tương đương với nhận dạng trong văn bản nên không trả lời được bài tập như đã nêu. 

Ngoài ra, khẳng định rằng “Do đó việc tính toán tất cả các hệ số$W_k$yêu cầu chính xác$2r+1$phép nhân" không được chứng minh trong mô hình TAOCP. Mô hình chi phí trong Phần 4.3.3 không thuần túy là đại số; nó nói về các phép toán bit và độ phức tạp đánh giá có cấu trúc. Đối số bỏ qua kết nối với cấu trúc tính toán cụ thể được sử dụng trong Thuật toán T. 

Tuyên bố “Danh tính (25) theo sau trực tiếp” không được hỗ trợ. Đây là tham chiếu vòng cho khung nội suy đã được giới thiệu mà không dẫn đến phép biến đổi cụ thể được sử dụng trong (25). 

Tất cả những vấn đề này là những khoảng trống biện minh chứ không phải là lỗi đại số trong lý thuyết Vandermonde, nhưng nhìn chung chúng ngăn cản lời giải giải quyết được bài tập thực tế. 

## Tóm tắt 

Lời giải đúng về mặt toán học ở mức độ trừu tượng cao nhưng không rút ra được công thức cụ thể (25) từ Mục 4.3.3 và không kết nối với cấu trúc sai phân hữu hạn cần thiết của văn bản. 

XÁC MINH: THẤT BẠI - lời giải thay thế đẳng thức sai phân hữu hạn bắt buộc (25) bằng đối số nội suy Vandermonde tổng quát và không chứng minh được công thức đã nêu.
