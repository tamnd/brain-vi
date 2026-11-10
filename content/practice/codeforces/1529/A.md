---
title: "CF 1529A - Eshag Yêu Mảng Lớn"
description: "Cho $U(x)$ và $V(x)$ là các đa thức bậc $r$ với các hệ số nguyên, và đặt $W(x) = U(x)V(x),$ sao cho $W(x)$ có bậc nhiều nhất là $2r$. Viết $U(x) = sum{i=0}^{r} Ui x^i, qquad V(x) = sum{i=0}^{r} Vi x^i, qquad W(x) = sum{k=0}^{2r} Wk x^k."
date: "2026-06-10T16:59:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1529
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 722 (Div. 2)"
rating: 800
weight: 1529
solve_time_s: 151
verified: false
draft: false
---

[CF 1529A - Eshag thích mảng lớn](https://codeforces.com/problemset/problem/1529/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$U(x)$Và$V(x)$là đa thức bậc$r$với các hệ số nguyên và đặt$W(x) = U(x)V(x),$Vì thế$W(x)$có nhiều nhất là bằng cấp$2r$. 

Viết$U(x) = \sum_{i=0}^{r} U_i x^i, \qquad V(x) = \sum_{i=0}^{r} V_i x^i, \qquad W(x) = \sum_{k=0}^{2r} W_k x^k.$Sửa chữa$2r+1$số nguyên phân biệt$x_0, x_1, \ldots, x_{2r}$. Định nghĩa$P_j = U(x_j), \qquad Q_j = V(x_j), \qquad R_j = W(x_j) = P_jQ_j.$Từ$W(x)$có nhiều nhất là bằng cấp$2r$, bản đồ$W(x) \mapsto (W(x_0), W(x_1), \ldots, W(x_{2r}))$là tiêm chích. Thật vậy, nếu một đa thức bậc nhiều nhất$2r$biến mất tại$2r+1$điểm phân biệt thì nó bằng 0. 

Do đó các giá trị$R_0, \ldots, R_{2r}$xác định các hệ số$W_0, \ldots, W_{2r}$độc đáo. 

Xác định$(2r+1)\times(2r+1)$Ma trận Vandermonde$A$qua$A_{j,k} = x_j^k, \qquad 0 \le j,k \le 2r.$Sau đó, danh tính đánh giá cho$W(x)$là$R = AW,$Ở đâu$R = (W(x_0), \ldots, W(x_{2r}))^T, \qquad W = (W_0, \ldots, W_{2r})^T.$Sự khác biệt của$x_j$ngụ ý rằng$A$là khả nghịch, vì định thức của nó bằng$\prod_{0 \le i < j \le 2r} (x_j - x_i) \ne 0.$Như vậy$W = A^{-1}R.$Cho phép$\lambda_{k,j}$biểu thị các mục của$A^{-1}$. Sau đó với mỗi$k$, 

W_k = \sum_{j=0}^{2r} \lambda_{k,j} W(x_j). \thẻ{25} 

Mỗi hệ số$W_k$do đó là sự kết hợp tuyến tính cố định của các giá trị$W(x_j)$, trong đó các hằng số$\lambda_{k,j}$chỉ phụ thuộc vào các điểm nội suy đã chọn$x_0, \ldots, x_{2r}$và không trên$U$hoặc$V$. 

Từ$W(x_j) = U(x_j)V(x_j)$và mỗi$U(x_j), V(x_j)$thu được bằng cách đánh giá mức độ$r$đa thức tại một đối số nguyên cố định, mỗi đánh giá như vậy bao gồm vô số phép cộng và phép nhân với các số nguyên cố định được xác định bởi$x_j$. Mỗi$W(x_j)$sau đó là một phép nhân của hai số nguyên có kích thước giới hạn so với tham số kích thước đầu vào$n$trong mô hình nhân đệ quy ở Mục 4.3.3. 

Công thức tái thiết (25) áp dụng một giá trị cố định$(2r+1)\times(2r+1)$phép biến đổi tuyến tính sang vectơ$(W(x_0), \ldots, W(x_{2r}))$. Vì các hệ số$\lambda_{k,j}$là các số nguyên cố định độc lập với$n$, phép biến đổi này chỉ bao gồm các phép cộng và phép nhân với các hằng số. 

Điều này chứng tỏ rằng mỗi hệ số$W_k$được lấy từ các giá trị$W(x_0), \ldots, W(x_{2r})$bởi mối quan hệ tuyến tính (25). 

Điều này hoàn thành việc chứng minh. ∎
