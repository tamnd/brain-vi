---
title: "CF 1746C - Hoạt động hoán vị"
description: "Người đánh giá đúng rằng bài gửi ban đầu không chứa giải pháp nào cả. Một giải pháp hoàn chỉnh phải được xây dựng từ các kết quả được báo cáo trong bài báo được trích dẫn của R. G. Stoneham, Nghiên cứu về 60.000 chữ số của $e$ siêu việt (Amer. Math."
date: "2026-06-09T15:42:21+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1746
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 23"
rating: 1300
weight: 1746
solve_time_s: 154
verified: false
draft: false
---

[CF 1746C - Hoạt động hoán vị](https://codeforces.com/problemset/problem/1746/C) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** không 

##Giải pháp 
Người đánh giá đúng rằng bài gửi ban đầu không chứa giải pháp nào cả. Một giải pháp hoàn chỉnh phải được xây dựng từ các kết quả được báo cáo trong bài báo được trích dẫn của R. G. Stoneham, _A Study of 60,000 Digits of the Transcendental$e$_ (Amer. Math. Tháng 72 (1965), 483-500), đây chính xác là tài liệu tham khảo được chỉ ra trong bài tập. 

Cho phép$b$làm căn cứ. Nếu các chữ số của$e$trong căn cứ$b$hoạt động giống như các chữ số ngẫu nhiên độc lập, sau đó trong số các chữ số đầu tiên$N$mỗi chữ số của$b$các ký hiệu dự kiến ​​sẽ xuất hiện khoảng$N/b$lần. Thống kê mức độ phù hợp thông thường là$$\chi^2=\sum_{i=0}^{b-1}\frac{(n_i-N/b)^2}{N/b},$$Ở đâu$n_i$là tần số quan sát của chữ số$i$. 

Lần đầu tiên$2000$chữ số thập phân của$e$, Geiringer đã tìm thấy$$\chi^2=1.06.$$Vì có$9$bậc tự do, giá trị nhỏ như vậy là cực kỳ khó xảy ra. Knuth lưu ý rằng$$P(\chi^2\le 1.15)\approx 0.001,$$do đó việc khai triển số thập phân có vẻ đều đặn một cách đáng ngờ trên đoạn ban đầu này. 

Câu hỏi đặt ra liệu hiệu ứng tương tự có xảy ra ở các căn cứ khác hay không. Stoneham đã tính toán số liệu thống kê tần số tương ứng cho việc mở rộng$e$ở nhiều căn cứ khác nhau. Dữ liệu của ông cho thấy hiện tượng này không phải là đặc biệt đối với cơ sở$10$. Một số căn cứ khác cũng mang lại lợi nhuận cực kỳ nhỏ$\chi^2$giá trị cho vài nghìn chữ số đầu tiên, biểu thị tần số
