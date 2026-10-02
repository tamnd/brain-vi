---
title: "CF 104873C - Đếm Cầu Thang"
description: "Chúng ta được yêu cầu đếm một lớp hình cụ thể được xây dựng từ các khối đơn vị. Mỗi cấu hình hợp lệ là một cấu trúc “giống như cầu thang”: các cột hình khối được sắp xếp từ trái sang phải với chiều cao không bao giờ tăng khi chúng ta di chuyển sang phải."
date: "2026-06-28T10:11:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104873
codeforces_index: "C"
codeforces_contest_name: "2018-2019 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104873
solve_time_s: 31
verified: false
draft: false
---

[CF 104873C - Đếm cầu thang](https://codeforces.com/problemset/problem/104873/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm một lớp hình cụ thể được xây dựng từ các khối đơn vị. Mỗi cấu hình hợp lệ là một cấu trúc “giống như cầu thang”: các cột hình khối được sắp xếp từ trái sang phải với chiều cao không bao giờ tăng khi chúng ta di chuyển sang phải. Vì vậy, hình dạng có thể được mô tả bằng một dãy số nguyên dương$h_1 \ge h_2 \ge \dots \ge h_k$, và tổng số hình lập phương là$n = \sum h_i$. 

Ngoài cấu trúc “giống như phân vùng” đơn điệu này, còn có một ràng buộc hình học bổ sung: hình dạng phải đối xứng với đường chéo$x = y$. Sự đối xứng này buộc sơ đồ phải tự phản chiếu theo đường chéo, điều này chỉ có thể thực hiện được đối với các hình dạng cầu thang rất cụ thể. 

Đầu vào cung cấp nhiều giá trị của$n$, và với mỗi cái chúng ta phải đếm xem có bao nhiêu cấu hình cầu thang đối xứng như vậy sử dụng chính xác$n$hình khối. 

Ràng buộc$n \le 2 \cdot 10^5$lên tới$10^4$các trường hợp thử nghiệm loại trừ việc tính toán lại mọi thứ cho mỗi truy vấn. Bất kỳ giải pháp nào cũng phải xử lý trước trong khoảng$O(N)$hoặc$O(N \log N)$, sau đó trả lời từng truy vấn trong thời gian không đổi. 

Một cách tiếp cận ngây thơ sẽ cố gắng tạo ra tất cả các phân vùng của$n$và kiểm tra tính đối xứng. Điều đó đã tăng theo cấp số nhân trong$\sqrt{n}$và việc kiểm tra tính đối xứng sẽ bổ sung thêm một yếu tố khác. Ngay cả đối với mức độ vừa phải$n$, điều này trở nên không thể thực hiện được. 

Một vấn đề tinh tế hơn là ràng buộc đối xứng có tính toàn cục. Thật dễ dàng để xây dựng một phân vùng “trông có cấu trúc cục bộ” nhưng không đạt được tính bất biến đường chéo khi vẽ. Ví dụ: một phân vùng như$4 + 2 + 1$tạo ra sơ đồ Ferrers không đối xứng, mặc dù nó có giá trị như một bậc thang. 

Khó khăn chính là ràng buộc đối xứng không phải là ràng buộc cục bộ đối với các hàng hoặc cột mà là kết hợp chúng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là liệt kê tất cả các chuỗi không tăng của các số nguyên dương có tổng bằng$n$và với mỗi cái, hãy xây dựng sơ đồ Ferrers của nó và kiểm tra xem nó có bằng chuyển vị của nó hay không. Số lượng phân vùng của$n$đã là số mũ trong$\sqrt{n}$, vì vậy cách tiếp cận này trở nên không thể vượt quá các giá trị rất nhỏ của$n$. Việc kiểm tra tính đối xứng là tuyến tính trong kích thước sơ đồ, điều này thậm chí còn khiến nó tệ hơn. 

Cái nhìn sâu sắc về cấu trúc là một cầu thang đối xứng qua đường chéo tương ứng chính xác với một phân vùng tự liên hợp. Trong thuật ngữ sơ đồ Ferrers, việc hoán đổi hàng và cột sẽ được thực hiện. Yêu cầu sự bình đẳng có nghĩa là sơ đồ phải bất biến theo sự hoán đổi này. 

Một thực tế cổ điển và không tầm thường là các phân vùng tự liên hợp của$n$đang song hành với các phân vùng của$n$thành những phần lẻ khác nhau. Điều này thay đổi hoàn toàn bài toán: thay vì điều kiện đối xứng 2D, chúng tôi chuyển đổi nó thành bài toán lựa chọn 1D. 

Mỗi số nguyên lẻ$2k-1$có thể được sử dụng nhiều nhất một lần và đóng góp nhiều khối như vậy. Vì vậy, chúng ta đang đếm các tập hợp con của các số nguyên lẻ có tổng bằng$n$. Đây là chiếc ba lô tiêu chuẩn 0/1 dành cho trọng lượng lẻ. 

Chúng tôi tính toán trước một DP trong đó chúng tôi lặp lại tất cả các số lẻ và cập nhật các cách để tạo thành tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tạo phân vùng + kiểm tra tính đối xứng) | Hàm mũ | O(n) | Quá chậm | 
| DP trên các phần lẻ riêng biệt | Tiền xử lý O(n^2/2), O(1) cho mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta quy vấn đề về cách đếm để biểu diễn$n$dưới dạng tổng của các số nguyên lẻ phân biệt. 

1. Tính toán trước một mảng`dp`Ở đâu`dp[x]`là số cách tính tổng`x`sử dụng các số nguyên lẻ phân biệt. 

Chúng tôi khởi tạo`dp[0] = 1`bởi vì có đúng một cách để tạo thành số 0: không chọn gì cả. 
2. Lặp lại tất cả các giá trị lẻ$1, 3, 5, \dots \le N$. 

Mỗi số lẻ được coi là một số it
