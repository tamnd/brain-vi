---
title: "CF 1994H - Pháo đài"
description: "Bài toán đầu tiên hỏi liệu một chuỗi vô hạn các bình phương hoàn hảo $(an^2)$ có thể tồn tại sao cho mỗi số hạng bắt đầu từ số thứ ba thỏa mãn phép truy toán kiểu Fibonacci $an^2 = a{n-1}^2 + a{n-2}^2 hay không."
date: "2026-06-09T02:24:27+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "games", "greedy", "hashing", "interactive", "math", "number-theory", "strings"]
categories: ["algorithms"]
codeforces_contest: 1994
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 959 sponsored by NEAR (Div. 1 + Div. 2)"
rating: 3500
weight: 1994
solve_time_s: 124
verified: false
draft: false
---

[CF 1994H - Fortnite](https://codeforces.com/problemset/problem/1994/H) 

**Đánh giá:** 3500 
**Tags:** tổ hợp, thuật toán xây dựng, trò chơi, tham lam, băm, tương tác, toán học, lý thuyết số, chuỗi 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

## Giải pháp 
## Thăm dò 

Bài toán đầu tiên hỏi liệu một chuỗi vô hạn các hình vuông hoàn hảo có$(a_n^2)$có thể tồn tại sao cho mỗi số hạng bắt đầu từ số hạng thứ ba thỏa mãn phép truy toán kiểu Fibonacci$a_n^2 = a_{n-1}^2 + a_{n-2}^2.$Vì các bình phương tăng nhanh hơn tuyến tính và phép truy toán có tính chất cộng tính nên sự tồn tại của chuỗi như vậy rất hạn chế. Bất kỳ phương pháp tiếp cận nào được đề xuất đều phải loại trừ nghiêm ngặt tất cả các khả năng hoặc đưa ra một cách xây dựng hợp lệ. 

Bài toán thứ hai yêu cầu một chuỗi các ô vuông tăng vô hạn$(a_n^2)$sao cho mỗi tổng các số hạng liên tiếp tự nó là một số chính phương,$a_n^2 + a_{n+1}^2 = b_n^2$đối với một số nguyên$b_n$. Điều kiện này tương đương với việc yêu cầu mỗi bộ ba liên tiếp$(a_n, a_{n+1}, b_n)$tạo thành bộ ba Pythagore. Một giải pháp hợp lệ phải cung cấp một cách xây dựng rõ ràng và chứng minh tính chất cho tất cả các điều khoản. 

## Hiểu vấn đề 

Vấn đề thứ nhất là vấn đề không tồn tại. Một giải pháp nghiêm ngặt phải kiểm tra tất cả các chuỗi số nguyên thỏa mãn$a_n^2 = a_{n-1}^2 + a_{n-2}^2$và chứng tỏ rằng không thể có dãy số tự nhiên vô hạn. Việc kiểm tra các số nguyên nhỏ cho thấy rằng chuỗi chỉ có thể bắt đầu với các giá trị tầm thường, vì bất kỳ chuỗi không tầm thường nào đều nhanh chóng vi phạm các ràng buộc số nguyên. 

Vấn đề thứ hai là một vấn đề tồn tại mang tính xây dựng. Người ta phải xác định một dãy tăng dần rõ ràng$(a_n)$và xác minh rằng$a_n^2 + a_{n+1}^2$luôn là một hình vuông hoàn hảo. Bất kỳ phép lặp hoặc công thức nào đều phải tạo ra số nguyên từ bước đầu tiên, vì cặp đầu tiên phải đáp ứng chính xác điều kiện. 

## Kiến trúc chứng minh 

Đối với vấn đề đầu tiên, người ta có thể kiểm tra trình tự$(a_n)$modulo số nguyên nhỏ để hiển thị các ràng buộc hoặc tranh luận bằng cách sử dụng mức tối thiểu. Bắt đầu bằng số nguyên$a_1, a_2$, học kỳ tiếp theo$a_3$được xác định là$a_3 = \sqrt{a_1^2 + a_2^2}$. Từ$a_1^2 + a_2^2$phải là một hình vuông hoàn hảo, cặp$(a_1, a_2)$bản thân nó phải tạo thành một cặp Pythagore. Sau đó, trình tự này rút gọn thành việc tạo ra các bộ ba Pythagore theo trình tự kiểu Fibonacci. Các phân loại đã biết của nghiệm số nguyên của$x^2 + y^2 = z^2$chứng tỏ rằng không có dãy số nguyên tăng nghiêm ngặt vô hạn nào có thể thỏa mãn$a_n^2 = a_{n-1}^2 + a_{n-2}^2$, bởi vì cuối cùng đệ quy buộc các số hạng không nguyên hoặc giảm. 

Đối với bài toán thứ hai, người ta có thể khai thác tham số hóa của bộ ba Pythagore nguyên thủy: đối với các số nguyên nguyên tố cùng nhau$u>v>0$có tính chẵn lẻ đối lập, bộ ba$(u^2 - v^2, 2uv, u^2 + v^2)$
