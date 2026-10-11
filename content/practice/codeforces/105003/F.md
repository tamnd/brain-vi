---
title: "CF 105003F - Phỏng đoán Erd\u0151s-Straus"
description: "Chúng ta được yêu cầu quyết định xem có tồn tại một cặp số nguyên dương $x, y$ sao cho một biểu thức hữu tỉ cố định bằng tổng của ba phân số Ai Cập trong đó mẫu số thứ ba bị ràng buộc là tích $xy$ hay không."
date: "2026-06-28T03:16:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105003
codeforces_index: "F"
codeforces_contest_name: "XXVIII Spain Olympiad in Informatics, Online Qualifier"
rating: 0
weight: 105003
solve_time_s: 38
verified: false
draft: false
---

[CF 105003F - Phỏng đoán Erd\u0151s-Straus](https://codeforces.com/problemset/problem/105003/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu quyết định xem một cặp số nguyên dương có$x, y$tồn tại sao cho một biểu thức hữu tỉ cố định bằng tổng của ba phân số Ai Cập trong đó mẫu số thứ ba bị ràng buộc là tích$xy$. Đối với mỗi trường hợp thử nghiệm, chúng tôi xuất ra một cặp hợp lệ hoặc báo cáo là không thể. 

Sau khi viết lại phương trình, đầu vào$n$đóng vai trò như một tham số kiểm soát điều kiện Diophantine trên hai số nguyên chưa biết. Nhiệm vụ không phải là tính gần đúng hay tìm kiếm trên các số thực mà là về cấu trúc số nguyên chính xác. 

Ràng buộc$n \le 10^9$với tối đa 100 trường hợp thử nghiệm có nghĩa là bất kỳ phương pháp nào lặp lại tối đa$n$ngay lập tức là không thể. Thậm chí có thể quét bậc hai$x, y$các cặp nằm ngoài tầm với vì điều đó sẽ bùng nổ$10^{18}$trong trường hợp xấu nhất. Các chiến lược khả thi duy nhất phải giảm vấn đề thành một số thứ như phép liệt kê số chia hoặc hệ số đại số, trong đó không gian tìm kiếm là tuyến tính trong$n$, thường là xung quanh$O(\sqrt n)$hoặc khấu hao tốt hơn. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Biểu thức
