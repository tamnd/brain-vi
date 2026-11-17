---
title: "CF 1999A - Lại là A+B?"
description: "Tác vụ yêu cầu tổng các chữ số của một số có hai chữ số cho nhiều trường hợp thử nghiệm. Mỗi số đầu vào $n$ được đảm bảo nằm trong khoảng từ 10 đến 99, vì vậy chữ số đầu tiên luôn khác 0."
date: "2026-06-08T14:19:23+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1999
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 964 (Div. 4)"
rating: 800
weight: 1999
solve_time_s: 77
verified: false
draft: false
---

[CF 1999A - Lại là A+B?](https://codeforces.com/problemset/problem/1999/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Tác vụ yêu cầu tổng các chữ số của một số có hai chữ số cho nhiều trường hợp thử nghiệm. Mỗi số đầu vào$n$được đảm bảo bao gồm từ 10 đến 99, vì vậy chữ số đầu tiên luôn khác 0. Đầu vào bắt đầu bằng$t$, số lượng ca kiểm thử, theo sau là$t$mỗi dòng chứa một số nguyên có hai chữ số. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị tổng của các chữ số hàng chục và hàng đơn vị. 

Với những hạn chế, số lượng ca kiểm thử$t$nhỏ, lên tới 90, và mỗi$n$được cố định về kích thước. Không cần các thuật toán hoặc cấu trúc dữ liệu phức tạp vì ngay cả cách tiếp cận đơn giản tính tổng cho từng số riêng lẻ cũng đủ nhanh. Các trường hợp đặc biệt rất đơn giản: số có hai chữ số nhỏ nhất là 10, có tổng chữ số là 1 và số lớn nhất là 99, có tổng chữ số là 18. Cách tiếp cận bất cẩn có thể cố gắng chuyển đổi số thành chuỗi một cách không cần thiết hoặc xử lý sai số 10, nhưng cách tiếp cận số học đơn giản bằng cách sử dụng phép chia và modulo sẽ tránh được những cạm bẫy này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là chuyển đổi từng số thành một chuỗi, chia nó thành các ký tự, chuyển đổi các ký tự đó thành số nguyên và tính tổng chúng. Điều này hoạt động chính xác vì mỗi số được đảm bảo có chính xác hai chữ số. Nhược điểm là nó liên quan đến các thao tác chuỗi, không cần thiết đối với các số có kích thước này. 

Cách tiếp cận tối ưu hoàn toàn là số học. Đối với số có hai chữ số$n$, chữ số hàng chục có thể được tính là$n // 10$và chữ số hàng đơn vị là$n % 10$. Tổng hai giá trị này sẽ cho ra tổng các chữ số một cách trực tiếp, tránh mọi thao tác chuỗi và hoạt động trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Cho rằng$t$tối đa là 90, phương pháp này sẽ hoàn thành trong chưa đầy một giây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Chuyển đổi sang chuỗi và chữ số tổng | O(t) | O(1) | Đã chấp nhận | 
| Trích xuất chữ số số học | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$t$từ đầu vào, đại diện cho số lượng trường hợp thử nghiệm. 
2. Lặp lại từng test case từ 1 đến$t$. 
3. Đối với test case hiện tại, hãy đọc số nguyên $
