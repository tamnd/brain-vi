---
title: "CF 1521A - Nastia và những con số gần tốt"
description: "Thủ tục chuyển đổi cơ số biến đổi một số nguyên không âm được viết trong một cơ số thành biểu diễn của nó trong một cơ số khác. Chương trình đang được thảo luận là Phương pháp 1a từ Phần 4."
date: "2026-06-10T17:46:57+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1521
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 720 (Div. 2)"
rating: 1000
weight: 1521
solve_time_s: 131
verified: false
draft: false
---

[CF 1521A - Nastia và những con số gần tốt](https://codeforces.com/problemset/problem/1521/A) 

**Đánh giá:** 1000 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

Thủ tục chuyển đổi cơ số biến đổi một số nguyên không âm được viết trong một cơ số thành biểu diễn của nó trong một cơ số khác. Chương trình đang được thảo luận là Phương pháp 1a từ Phần 4.4, tạo ra các chữ số của một số nguyên$u$trong cơ số$B$bằng cách chia lặp đi lặp lại cho$B$trong cơ số ban đầu$b$. Cụ thể, nó tạo ra các chữ số$U_0, U_1, U_2, \ldots$như vậy$$u = (\ldots U_2 U_1 U_0)_B,$$trong đó mỗi chữ số có được bằng các phép tính thương và số dư liên tiếp$$U_0 = u \bmod B,\quad U_1 = \lfloor u/B \rfloor \bmod B,\quad \ldots$$cho đến khi thương số bằng không. 

Bài tập yêu cầu chứng minh rằng việc triển khai MIX của phương pháp này vẫn hợp lệ khi hằng số$10$trong lệnh chia được thay thế bằng một hằng số tổng quát hơn$c$, miễn là việc diễn giải số học của chương trình được giữ nguyên. 

Khẳng định cơ bản là thuật toán không phụ thuộc vào cơ số thập phân cụ thể mà chỉ phụ thuộc vào tính chính xác của phân tách thương số dư$$u = qB + r,\quad 0 \le r < B.$$##Giải pháp 

Chương trình trong phương trình (1) thực hiện phép chia lặp lại cho$B = 10$, trích xuất phần còn lại mỗi lần làm cơ số tiếp theo-$10$chữ số. Bất biến chính được duy trì qua các lần lặp là ở đầu mỗi vòng lặp, cặp thanh ghi$(rA, rX)$lưu trữ giá trị hiện tại$u^{(j)}$và sau khi thực hiện bước chia, giá trị này được phân tách thành$$u^{(j)} = 10 q^{(j)} + r^{(j)}, \quad 0 \le r^{(j)} < 10,$$để có thể$r^{(j)}$được lưu dưới dạng chữ số đầu ra tiếp theo và$q^{(j)}$trở thành giá trị tiếp theo để xử lý. 

Đối số đúng chỉ sử dụng thuộc tính mà phép chia trong MIX tạo ra thương và số dư thỏa mãn$$rAX = Bq + r,\quad 0 \le r < B.$$Nếu hướng dẫn`DIV =10=`được thay thế bởi`DIV =c=`, thay vào đó máy sẽ tính toán$$rAX = cq + r,\quad 0 \le r < c,$$vì vậy vòng lặp bây giờ trích xuất các chữ số trong cơ số$c$thay vì căn cứ$10$. Mỗi bước của thuật toán tiếp tục duy trì cùng một bất biến cấu trúc: sau$j$lặp đi lặp lại, giá trị được lưu trữ trong các thanh ghi làm việc bằng$$\left\lfloor \frac{u}{c^j} \right\rfloor,$$và phần dư được phát ra chính xác$$U_j = \left\lfloor \frac{u}{c^j} \right\rfloor \bmod c.$$Vì biểu diễn cơ số được xác định duy nhất bằng cách trích xuất lặp đi lặp lại các số dư ở dạng này nên chương trình sẽ tính toán cơ số$c$mở rộng của$u$. 

Không có phần nào khác của chương trình phụ thuộc vào hằng số$10$ngoại trừ số chia được sử dụng trong phân tách thương số dư này. Do đó thay thế bất kỳ số nguyên cố định$c \ge 2$duy trì tính chính xác và chỉ thay đổi cơ số của biểu diễn đầu ra. 

Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Tính chính xác phụ thuộc vào hai điều kiện độc lập mà cả hai đều phải tuân theo trong số học MIX. Đầu tiên, lệnh DIV phải thực hiện phép chia Euclide chính xác, tạo ra thương số và số dư thỏa mãn$0 \le r < c$. Điều này đảm bảo rằng mỗi chữ số được trích xuất nằm trong phạm vi chữ số chính xác cho cơ số$c$. 

Thứ hai, mối quan hệ truy hồi gây ra bởi phép chia lặp lại phải làm giảm độ lớn của giá trị làm việc ở mỗi bước. Vì mỗi lần lặp lại thay thế$u$qua$\lfloor u/c \rfloor$, chúng tôi có$$\left\lfloor \frac{u}{c^{j+1}} \right\rfloor < \left\lfloor \frac{u}{c^j} \right\rfloor$$bất cứ khi nào giá trị khác 0, đảm bảo chấm dứt sau hữu hạn nhiều bước. 

Nhận dạng tái tạo chữ số$$u = \sum_{j \ge 0} U_j c^j$$theo sau trực tiếp bằng cách giải phóng phép truy toán để trích xuất thương số lặp lại, giống hệt với cơ sở tiêu chuẩn-$10$bằng chứng với hằng số được thay thế ở mọi nơi bằng$c$. 

## Ghi chú 

Hằng số$10$trong Phương pháp 1a không có vai trò cấu trúc nào ngoài việc xác định cơ số. Do đó, mẫu chương trình tương tự thực hiện chuyển đổi sang bất kỳ cơ số cố định nào$c$có sẵn dưới dạng hằng số máy trong lệnh chia. Hạn chế duy nhất là phép chia MIX phải hoạt động theo ngữ nghĩa phép chia Euclide, đảm bảo$0 \le r < c$, đảm bảo trích xuất chữ số hợp lệ.
