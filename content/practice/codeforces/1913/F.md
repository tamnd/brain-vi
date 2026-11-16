---
title: "CF 1913F - Bài toán Palindromic"
description: "Chúng ta được cung cấp một chuỗi bao gồm các chữ cái viết thường và chúng ta được phép thay đổi tối đa một ký tự thành bất kỳ chữ cái viết thường nào khác. Mục tiêu là tạo ra một chuỗi tối đa hóa số lượng chuỗi con palindromic."
date: "2026-06-08T20:09:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "hashing", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 1913
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 160 (Rated for Div. 2)"
rating: 2800
weight: 1913
solve_time_s: 37
verified: false
draft: false
---

[CF 1913F - Vấn đề Palindromic](https://codeforces.com/problemset/problem/1913/F) 

**Đánh giá:** 2800 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, băm, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bao gồm các chữ cái viết thường và chúng ta được phép thay đổi tối đa một ký tự thành bất kỳ chữ cái viết thường nào khác. Mục tiêu là tạo ra một chuỗi tối đa hóa số lượng chuỗi con palindromic. Chuỗi con palindromic là bất kỳ chuỗi ký tự liền kề nào đọc xuôi và ngược giống nhau và các palindrome lặp lại được tính mỗi khi chúng xuất hiện. Nếu nhiều chuỗi đạt được số lượng chuỗi con palindromic tối đa, chúng ta phải chọn chuỗi con nhỏ nhất về mặt từ điển. 

Kích thước đầu vào có thể lên tới 300.000 ký tự. Điều này loại trừ bất kỳ cách tiếp cận ngây thơ nào kiểm tra tất cả các chuỗi con có thể một cách riêng lẻ, bởi vì số lượng chuỗi con tăng theo phương trình bậc hai. Cụ thể, đối với một chuỗi có độ dài$n$, có$n(n+1)/2$các chuỗi con, do đó, một giải pháp bạo lực đơn giản sẽ yêu cầu khoảng$4.5 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, điều này vượt xa khả năng thực hiện trong 5 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi đã là palindrome, các chuỗi có tất cả các chữ cái giống hệt nhau và các chuỗi trong đó việc thay đổi một ký tự có thể tạo ra nhiều mở rộng palindromic có thể. Ví dụ, chuỗi`aabaa`trở thành`aaaaa`nếu chúng ta thay đổi ký tự thứ ba`b`ĐẾN`a`, tăng số lượng chuỗi con palindromic từ 9 lên 15. Một cách tiếp cận bất cẩn chỉ đếm các palindrome một chữ cái o
