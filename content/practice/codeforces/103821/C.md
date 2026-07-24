---
title: "CF 103821C - Chỗ ngồi hoàn hảo"
description: "Giả sử $f$ được biểu diễn bằng sơ đồ quyết định nhị phân có thứ tự rút gọn và đặt $F(p)$ biểu thị đa thức độ tin cậy theo chuyên môn hóa $p1=cdots=pn=p$."
date: "2026-07-02T08:21:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103821
codeforces_index: "C"
codeforces_contest_name: "(Aleppo + HAIST + SVU + Private) CPC 2022"
rating: 0
weight: 103821
solve_time_s: 128
verified: false
draft: false
---

[CF 103821C - Chỗ ngồi hoàn hảo](https://codeforces.com/problemset/problem/103821/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$f$được biểu diễn bằng sơ đồ quyết định nhị phân có thứ tự rút gọn và cho$F(p)$biểu thị đa thức độ tin cậy theo chuyên môn hóa$p_1=\cdots=p_n=p$. Đối với mỗi nút$v$của BDD, hãy$F_v(p)$biểu thị giá trị hàm con tương ứng của$F(p)$thu được bằng cách hạn chế BDD phụ bắt nguồn từ$v$. Đối với các nút chìm, các định nghĩa được cố định bởi$F_{\bot}(p)=0$Và$F_{\top}(p)=1$. 

Đối với một nút nhánh$v$được gắn nhãn bởi một số biến$x_j$với người kế vị LO$v_0$và người kế nhiệm HI$v_1$, việc đánh giá đa thức độ tin cậy với xác suất giống nhau được thực hiện trực tiếp từ điều kiện về giá trị của$x_j$, vì mỗi biến bằng$1$với xác suất$p$Và$0$với xác suất$1-p$. Điều này mang lại$$F_v(p) = (1-p)F_{v_0}(p) + pF_{v_1}(p).$$Phép lặp này khớp với cấu trúc của Thuật toán C trong Phần 7.1.4, trong đó quá trình tính toán tiến hành từ dưới lên trên BDD và mỗi giá trị nút là sự kết hợp của các giá trị kế tiếp của nó. 

Để tính đạo hàm, lấy vi phân đẳng thức trên theo$p$. Viết$F_v'(p)=\frac{d}{dp}F_v(p)$cho$$F_v'(p)
= \frac{d}{dp}\bigl((1-p)F_{v_0}(p) + pF_{v_1}(p)\bigr).$$Áp dụng quy tắc tích cho mỗi số hạng mang lại kết quả$$F_v'(p)
= -(F_{v_0}(p)) + (1-p)F_{v_0}'(p) + F_{v_1}(p) + pF_{v_1}'(p).$$Việc sắp xếp lại các thuật ngữ sẽ tạo ra một biểu mẫu được căn chỉnh với cùng cấu trúc từ dưới lên:$$F_v'(p)
= (1-p)F_{v_0}'(p) + pF_{v_1}'(p) + \bigl(F_{v_1}(p)-F_{v_0}(p)\bigr).$$Điều này thể hiện cả$F_v(p)$Và$F_v'(p)$chỉ dựa trên hai nút kế tiếp, do đó, việc tính toán có thể được thực hiện trong một lần duyệt BDD theo thứ tự tôpô ngược, chính xác như trong Thuật toán C, miễn là mỗi nút chỉ được xử lý sau khi các nút kế tiếp LO và HI của nó đã được đánh giá. 

Thuật toán sửa đổi liên kết với từng nút$v$một cặp giá trị$(F_v, D_v)$, Ở đâu$D_v$đại diện cho$F_v'(p)$. Đối với các nút chìm,$$(F_{\bot},D_{\bot})=(0,0), \qquad (F_{\top},D_{\top})=(1,0).$$Đối với mỗi nút nhánh$v$với những người kế nhiệm$v_0$Và$v_1$, tính toán là$$F_v \leftarrow (1-p)F_{v_0} + pF_{v_1},$$

$$D_v \leftarrow (1-p)D_{v_0} + pD_{v_1} + (F_{v_1}-F_{v_0}).$$Bởi vì BDD được sắp xếp theo thứ tự và không theo chu kỳ, nên mỗi nút được đánh giá chính xác một lần theo thứ tự phù hợp với chỉ số biến tăng dần, do đó, tất cả các giá trị kế tiếp đều có sẵn khi cần, phù hợp với nguyên tắc đánh giá của Thuật toán C. 

Giá trị trả về cho hàm ban đầu là cặp tại nút gốc$r$, cụ thể là$F_r(p)$Và$F_r'(p)$. 

Điều này hoàn thành việc xây dựng thuật toán đã sửa đổi và chứng minh sự truy hồi đạo hàm. ∎
