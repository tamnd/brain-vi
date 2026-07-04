---
title: "CF 103076E - Ngôi sao chết"
description: "Cho $n = s + t$ như trong phương trình (1) của Mục 7.2.1.3, và cho các dây cung chấp nhận được được mô tả bằng các chỉ số tăng nghiêm ngặt $n ct cdots c1 ge 0,$ tuân theo các ràng buộc $ct - c1 < m,$ và, trong bài tập hiện tại, loại trừ kề cận bổ sung $c{j+1} cj + 1 quad…"
date: "2026-07-04T00:23:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103076
codeforces_index: "E"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2021"
rating: 0
weight: 103076
solve_time_s: 159
verified: false
draft: false
---

[CF 103076E - Ngôi sao chết](https://codeforces.com/problemset/problem/103076/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$n = s + t$như trong phương trình (1) của Mục 7.2.1.3, và để các dây cho phép được mô tả bằng các chỉ số tăng nghiêm ngặt$n > c_t > \cdots > c_1 \ge 0,$chịu sự ràng buộc$c_t - c_1 < m,$và, trong bài tập hiện tại, việc loại trừ kề cận bổ sung$c_{j+1} > c_j + 1 \quad (t > j \ge 1).$Điều kiện kề được viết lại dưới dạng điều kiện khoảng cách đều$c_{j+1} \ge c_j + 2 \quad (t > j \ge 1).$Xác định trình tự được chuyển đổi$d_j = c_j - (j-1), \quad 1 \le j \le t.$Từ$c_{j+1} \ge c_j + 2$chúng tôi có được$d_{j+1} = c_{j+1} - j \ge c_j + 2 - j = (c_j - (j-1)) + 1 = d_j + 1,$Vì thế$n - (t-1) > d_t > \cdots > d_1 \ge 0.$Như vậy$(d_t,\ldots,d_1)$là một người bình thường$t$-tổ hợp được rút ra từ tập rút gọn${0,1,\ldots,n-(t-1)-1}$. 

Ngược lại, với bất kỳ dãy tăng nghiêm ngặt nào$d_t > \cdots > d_1 \ge 0$trong phạm vi giảm này, xác định$c_j = d_j + (j-1)$tạo ra một chuỗi thỏa mãn$c_{j+1} \ge c_j + 2$đảo ngược phép tính trên. Điều này thiết lập sự phân biệt giữa các hợp âm được chấp nhận và các hợp âm thông thường.$t$- sự kết hợp của kích thước$n-(t-1)$. 

Ràng buộc về khoảng cách được chuyển đổi bằng cách thay thế thành$c_t - c_1 = (d_t + t - 1) - d_1 = (d_t - d_1) + (t - 1).$Do đó giới hạn ban đầu$c_t - c_1 < m$trở thành$d_t - d_1 < m - (t-1).$Do đó bài toán hợp âm piano sửa đổi loại trừ kề tương đương với bài toán ban đầu của Bài tập 57 áp dụng cho tham số$n' = n - (t-1), \quad m' = m - (t-1).$Đặc biệt, tất cả các thủ tục liệt kê và tạo từ Bài tập 57 đều áp dụng nguyên văn sau lần rút gọn này, thay thế$n$qua$n-(t-1)$Và$m$qua$m-(t-1)$. 

Điều này hoàn thành việc chứng minh. ∎
