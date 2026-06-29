---
title: "CF 104743E - Truy vấn mô đun phạm vi"
description: "Chúng tôi được cung cấp hai mảng có cùng độ dài. Đối với mỗi khoảng truy vấn, chúng ta được yêu cầu tìm một giá trị mô đun $m$ sao cho khi chúng ta lấy mọi phần tử $ai$ trong khoảng đó và tính $ai bmod m$, chúng ta thu được chính xác $bi$ tương ứng."
date: "2026-06-29T01:21:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104743
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #25(5^2-Forces)"
rating: 0
weight: 104743
solve_time_s: 35
verified: false
draft: false
---

[CF 104743E - Truy vấn Modulo phạm vi](https://codeforces.com/problemset/problem/104743/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có cùng độ dài. Đối với mỗi khoảng truy vấn, chúng tôi được yêu cầu tìm giá trị mô đun$m$sao cho khi chúng ta lấy mọi phần tử$a_i$trong khoảng đó và tính$a_i \bmod m$, ta thu được chính xác kết quả tương ứng$b_i$. Trong số tất cả hợp lệ như vậy$m$, chúng ta cần số dương nhỏ nhất. Nếu không có số nguyên dương nào hoạt động thì câu trả lời là$-1$. 

Khó khăn chính là mỗi truy vấn đều độc lập và hỏi về ràng buộc mảng con liên quan đến số học mô-đun, không thể kết hợp trực tiếp theo cách đơn giản. Mô-đun phải hoạt động đồng thời cho tất cả các chỉ số trong khoảng, có nghĩa là một ràng buộc toàn cục duy nhất phải được trích xuất từ ​​các điều kiện cục bộ. 

Các ràng buộc rất lớn: tổng độ dài và số lượng truy vấn trên tất cả các trường hợp thử nghiệm tổng cộng bằng$10^6$. Điều này ngay lập tức loại trừ mọi hoạt động quét tuyến tính cho mỗi truy vấn trong phạm vi. Thậm chí$O(n \log n)$mỗi trường hợp thử nghiệm sẽ quá chậm nếu lặp lại cho mỗi truy vấn. Giải pháp dự định phải giảm mỗi truy vấn thành một số lượng nhỏ các truy vấn trong phạm vi được tính toán trước, sau đó trả lời từng truy vấn trong thời gian gần như không đổi hoặc logarit. 

Một vấn đề tế nhị xuất hiện trong ý nghĩa của sự bình đẳng mô-đun. điều kiện$a_i \bmod m = b_i$chỉ hợp lệ nếu$b_i < m$. Ràng buộc này rất dễ bị bỏ sót, nhưng nó trở nên quan trọng khi$b_i$lớn hoặc khi tối ưu$m$gần với các giá trị trong mảng. 

Trường hợp cạnh tinh tế thứ hai xảy ra khi tất cả$a_i = b_i$trong một phân đoạn. Trong trường hợp đó, bất kỳ mô đun nào lớn hơn tất cả$a_i$là hợp lệ, do đó câu trả lời không bị ràng buộc với các ràng buộc chia hết theo cách thông thường. 

## Phương pháp tiếp cận 

Bắt đầu từ góc độ vũ phu. Đối với một truy vấn duy nhất$[l, r]$, chúng ta có thể thử tất cả các giá trị có thể có của$m$từ$1$lên đến giá trị tối đa trong mảng và kiểm tra xem nó có thỏa mãn không$a_i \bmod m = b_i$cho tất cả$i$trong phạm vi. Việc này dễ thực hiện vì việc kiểm tra một ứng viên$m$chi phí$O(r-l+1)$. Tuy nhiên, vòng lặp bên ngoài$m$lên đến$10^6$khiến điều này hoàn toàn không thể thực hiện được. Một truy vấn có thể yêu cầu tới$10^{11}$hoạt động trong trường hợp xấu nhất. 

Để cải thiện điều này, chúng tôi đảo ngược quan điểm. Thay vì thử nghiệm$m$, chúng tôi rút ra các ràng buộc mà mọi giá trị hợp lệ$m$phải thỏa mãn. 

Từ$a_i \bmod m = b_i$, chúng tôi viết lại$a_i = k_i m + b_i$, ngụ ý rằng$a_i - b_i$phải chia hết cho$m$. Vì vậy mọi giá trị hợp lệ$m$phải chia tất cả các giá trị$a_i - b_i$trong phạm vi truy vấn. Điều này biến vấn đề thành một ràng buộc về khả năng chia hết trên một phạm vi, gợi ý sử dụng phạm vi gcd. 

Ngoài ra còn có hạn chế tiềm ẩn$b_i < m$, nghĩa$m$phải vượt mức tối đa$b_i$trong phạm vi. 

Vì vậy, mỗi truy vấn giảm xuống việc tìm ước số nhỏ nhất$m$của phạm vi gcd của$(a_i - b_i)$đó thực sự là lớn hơn
