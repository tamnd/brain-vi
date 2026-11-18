---
title: "CF 2014A - Robin Giúp đỡ"
description: "Đây là một bài toán xây dựng và bất khả thi (loại D/B lai). Đối với mỗi trường hợp, nhiệm vụ là xây dựng một tập hợp số rõ ràng sao cho tất cả tổng của ba phần tử riêng biệt đều là số nguyên tố hoặc chứng minh rằng không tồn tại tập hợp nào như vậy. Mỗi trong số bốn trường hợp con là độc lập."
date: "2026-06-08T12:59:32+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2014
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 974 (Div. 3)"
rating: 800
weight: 2014
solve_time_s: 71
verified: false
draft: false
---

[CF 2014A - Robin giúp đỡ](https://codeforces.com/problemset/problem/2014/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Kiểm tra loại sự cố 

Đây là **vấn đề xây dựng và bất khả thi (loại D/B lai)**. Đối với mỗi trường hợp, nhiệm vụ là xây dựng một tập hợp số rõ ràng sao cho tất cả tổng của ba phần tử riêng biệt đều là số nguyên tố hoặc chứng minh rằng không tồn tại tập hợp nào như vậy. Mỗi trong số bốn trường hợp con là độc lập. Các giải pháp đúng yêu cầu phải có công trình hợp lệ với xác minh đầy đủ hoặc bằng chứng không thể có được chứng minh đầy đủ. 

## Xác minh từng bước 

**Bước 1: Trường hợp 1 - Bốn số tự nhiên phân biệt** 

Khẳng định: Không có tập hợp nào tồn tại vì bốn số tự nhiên riêng biệt bất kỳ sẽ tạo ra tổng chia hết cho 2 hoặc 3. 

Đánh giá: Giải pháp kiểm tra một bộ ví dụ ($1,3,5,7$) và lưu ý rằng một số tổng là hợp số. Sau đó, nó tuyên bố rằng "phân tích chẵn lẻ toàn diện" loại trừ mọi khả năng. 

Nhận định: **KHÔNG CHÍNH XÁC** - Lập luận chỉ đưa ra một ví dụ thất bại; nó không bao hàm chặt chẽ tất cả các bộ bốn yếu tố tự nhiên riêng biệt. Một bằng chứng hoàn chỉnh yêu cầu phân tích chẵn lẻ và modulo 3 chính thức áp dụng cho tất cả các cấu hình. 

Phân loại: **Khoảng cách biện minh** 

**Bước 2: Trường hợp 2 - Năm số tự nhiên phân biệt** 

Yêu cầu: Không tồn tại tập hợp nào vì năm số tự nhiên riêng biệt modulo 3 bất kỳ đều phải có các thặng dư lặp lại, tạo ra tổng chia hết cho 3. 

Đánh giá: Đối số modulo 3 là chính xác. Đối với năm số, ít nhất hai số có phần dư modulo 3. Bất kỳ tổng nào bao gồm hai số này cộng với số thứ ba sẽ chia hết cho 3, do đó là hợp số. Điều này bao gồm đầy đủ tất cả các khả năng. 

Phán quyết: **HỢP LỆ** 

**Bước 3: Trường hợp 3 - Năm số nguyên phân biệt** 

Yêu cầu: Đã cố gắng xây dựng các công trình như${-1,1,3,5,7}$hoặc${-1,2,4,5,6}$có thể hoạt động. 

Đánh giá: Giải pháp tính toán rõ ràng các khoản tiền cho${-1,1,3,5,7}$và tìm thấy nhiều tổng không phải là số nguyên tố. Bộ thứ hai${-1,2,4,5,6}$không được tính toán đầy đủ; giải pháp bị cắt bỏ. **Không có công trình xây dựng hoàn chỉnh** và không có bằng chứng về tính bất khả thi. 

Nhận định: **SAU** - Lỗi nghiêm trọng; vấn đề yêu cầu một bộ hợp lệ hoặc một bằng chứng nghiêm ngặt. Không được cung cấp. 

**Bước 4: Trường hợp 4 - Sáu số nguyên phân biệt** 

Không có nỗ lực nào được thực hiện. 

Nhận định: **SAU** - Lỗi nghiêm trọng; giải pháp bỏ qua trường hợp này hoàn toàn. 

## Kiểm tra tính đầy đủ 

- Trường hợp 1: hợp lý một phần, yêu cầu đối số chẵn lẻ/mod 3 chặt chẽ. 
- Trường hợp 2: hoàn toàn hợp lý. 
- Trường hợp 3: không đầy đủ; không có bằng chứng xây dựng hợp lệ hoặc không thể thực hiện được. 
- Trường hợp 4: thiếu. 

Giải pháp đề xuất không giải quyết được tất cả các trường hợp cần thiết một cách chặt chẽ. 

## Tóm tắt 

Lời giải chứa nhiều khoảng trống: chứng minh bất khả thi cho bốn số tự nhiên không được chứng minh đầy đủ, việc xây dựng năm số nguyên không đầy đủ và sáu số nguyên không được giải quyết. 

XÁC MINH: THẤT BẠI - Giải pháp thất bại vì việc xây dựng năm số nguyên không đầy đủ và trường hợp bốn bị bỏ qua hoàn toàn.
