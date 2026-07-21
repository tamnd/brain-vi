---
title: "CF 103652B - Bộ tạo đồng dư tuyến tính"
description: "Giả sử $G$ là biểu đồ Cayley của nhóm đối xứng $Sn$ với các bộ tạo $(alpha1,dots,alphak)$ và giả sử rằng mỗi bộ tạo thỏa mãn $$alphaj(x)=y$$ cho các ký hiệu phân biệt cố định $x,y trong {1,dots,n}$."
date: "2026-07-02T21:59:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103652
codeforces_index: "B"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 8: XIX Open Cup Onsite"
rating: 0
weight: 103652
solve_time_s: 130
verified: false
draft: false
---

[CF 103652B - Trình tạo đồng dư tuyến tính](https://codeforces.com/problemset/problem/103652/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$G$là đồ thị Cayley của nhóm đối xứng$S_n$với máy phát điện$(\alpha_1,\dots,\alpha_k)$và giả sử rằng mỗi bộ tạo thỏa mãn$$\alpha_j(x)=y$$cho các ký hiệu riêng biệt cố định$x,y \in {1,\dots,n}$. 

Hãy để một con đường Hamilton vào$G$bắt đầu từ hoán vị danh tính$e=12\cdots n$được cho:$$v_0=e,\ v_1,\dots,v_{N-1}, \quad N=n!.$$Đối với mỗi bước, tồn tại một chỉ mục$j_i$như vậy$$v_{i+1}=v_i \alpha_{j_i}.$$Định nghĩa$$a_i = v_i(x), \quad b_i = v_i(y).$$Từ quy tắc chuyển tiếp và giả định$\alpha_{j}(x)=y$, chúng tôi có được$$v_{i+1}(x)=v_i(\alpha_{j_i}(x))=v_i(y),$$kể từ đây$$a_{i+1}=b_i \quad \text{for } 0 \le i \le N-2.$$Danh tính này liên kết các giá trị liên tiếp của chuỗi$(a_i)$Và$(b_i)$bằng một ca:$$b_i=a_{i+1}.$$Trình tự$(a_i)$là danh sách các giá trị được lấy bởi hàm$g \mapsto g(x)$dọc theo con đường Hamilton. Vì mỗi đỉnh của$S_n$xuất hiện đúng một lần,$(a_0,a_1,\dots,a_{N-1})$là một hoán vị của${1,\dots,n}$. 

Từ$b_i=a_{i+1}$vì$0 \le i \le N-2$, trình tự$(b_0,\dots,b_{N-2})$chính xác là dãy số$$(a_1,a_2,\dots,a_{N-1}).$$Vì thế$(b_0,\dots,b_{N-2})$chứa mọi phần tử của${1,\dots,n}$ngoại trừ$a_0$. 

Tại đỉnh ban đầu$v_0=e$, chúng tôi có$a_0=e(x)=x$. Do đó giá trị$x$không xuất hiện trong$(b_0,\dots,b_{N-2})$. Từ$(b_0,\dots,b_{N-1})$cũng là một hoán vị của${1,\dots,n}$, giá trị còn thiếu phải xuất hiện ở vị trí cuối cùng:$$b_{N-1}=x.$$Như vậy$$v_{N-1}(y)=x.$$Điều này hoàn thành việc chứng minh. ∎
