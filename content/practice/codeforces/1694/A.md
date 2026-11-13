---
title: "CF 1694A - Bò"
description: "Giải pháp đề xuất trả lời bài tập thực tế đã được hỏi. Bài tập 3.4.2.10 yêu cầu sửa đổi Thuật toán R để loại bỏ vùng chứa khi mẫu hiện tại có kích thước $n$ có thể được lưu trữ trực tiếp trong bộ nhớ."
date: "2026-06-09T22:48:28+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1694
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 800 (Div. 2)"
rating: 800
weight: 1694
solve_time_s: 148
verified: false
draft: false
---

[CF 1694A - Creep](https://codeforces.com/problemset/problem/1694/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
## Tính đúng đắn 

Giải pháp đề xuất trả lời bài tập thực tế đã được hỏi. Bài tập 3.4.2.10 yêu cầu sửa đổi Thuật toán R để loại bỏ vùng chứa khi kích thước mẫu hiện tại$n$có thể được lưu trữ trực tiếp trong bộ nhớ. Giải pháp trình bày một sửa đổi như vậy và mô tả cách biểu diễn trạng thái mới$S[1],\ldots,S[n]$. 

Thuật toán đã sửa đổi có hoạt động tương đương với Thuật toán R. Trong Thuật toán R, kho lưu trữ chỉ tồn tại để giữ lại các bản ghi hiện được đại diện bởi các chỉ số mẫu. Nếu bản ghi mẫu vừa với bộ nhớ thì bảng chỉ mục và kho lưu trữ sẽ trở nên không cần thiết. Việc thay thế trực tiếp mục nhập mẫu đã chọn bằng bản ghi mới hoàn toàn giống với bản cập nhật mà Thuật toán R thực hiện gián tiếp thông qua$I[M]$và hồ chứa. 

Đối số chính xác về cơ bản là bằng chứng tiêu chuẩn của việc lấy mẫu hồ chứa. Lời giải cho thấy bản ghi mới vào mẫu có xác suất$n/(t+1)$, rằng mọi bản ghi được lấy mẫu hiện có sẽ bị xóa với xác suất$1/(t+1)$và bản ghi được xử lý trước đó vẫn còn trong mẫu với xác suất$$\frac{n}{t}\cdot\frac{t}{t+1} = \frac{n}{t+1}.$$Như vậy tất cả$t+1$hồ sơ có xác suất bằng nhau$n/(t+1)$thuộc về mẫu sau khi xử lý bản ghi đầu vào tiếp theo. Vì quy tắc cập nhật giống hệt với Thuật toán R ngoại trừ cách biểu diễn lưu trữ nên phân phối lấy mẫu không thay đổi. 

Do đó, thuật toán tạo ra mẫu ngẫu nhiên giống như Thuật toán R trong khi loại bỏ cả kho chứa và bảng chỉ số. 

## Khoảng trống và lỗi 

Có một khoảng cách biện minh nhỏ. 

Lời giải cho biết bất biến mạnh hơn mà mỗi một trong các$\binom{t}{n}$mẫu xảy ra với xác suất$1/\binom{t}{n}$, nhưng bằng chứng sau đây chỉ đưa ra phát biểu yếu hơn rằng mỗi bản ghi riêng lẻ đều có xác suất$n/t$. Bản thân các xác suất biên bằng nhau không hàm ý tính đồng nhất trên tất cả$n$-tập hợp con. 

Đây là **Khoảng cách biện minh**, không phải lỗi nghiêm trọng. Bài tập yêu cầu sửa đổi Thuật toán R chứ không phải để kiểm chứng hoàn toàn tính đồng nhất của nó. Do thuật toán đã sửa đổi thực hiện chính xác các quyết định thay thế ngẫu nhiên giống như Thuật toán R, nên việc phân bố đồng đều trên các mẫu được kế thừa trực tiếp từ tính chính xác của Thuật toán R. Giải pháp này rõ ràng dựa trên thực tế này khi nó nói rằng tính chính xác giống hệt với Thuật toán R. 

Không có bước nào trong thuật toán là sai. Không có tuyên bố cần thiết nào về việc sửa đổi là sai. 

## Tóm tắt 

Giải pháp đề xuất đưa ra biến thể lấy mẫu hồ chứa tiêu chuẩn chỉ lưu trữ mẫu hiện tại trong bộ nhớ. Thuật toán đúng, kho chứa thực sự bị loại bỏ và đối số đúng đắn là đủ cho mục đích của bài tập này. Điểm yếu duy nhất là bất biến mạnh hơn đã nêu không bị bác bỏ hoàn toàn, mặc dù nó xuất phát ngay từ sự tương đương với Thuật toán R. 

XÁC MINH: ĐẠT - giải pháp là chính xác và đầy đủ.
