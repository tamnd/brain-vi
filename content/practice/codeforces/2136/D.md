---
title: "CF 2136D - Dành cho Nhà vô địch"
description: "Đặt $p = beta - alpha$ là xác suất để một $Uj$ nằm trong khoảng $[alpha, beta)$, như trong phương trình (4). Khoảng cách có độ dài $r$ xảy ra khi $r$ liên tiếp của $U$ nằm ngoài $[alpha, beta)$, theo sau là $U$ bên trong khoảng."
date: "2026-06-09T04:10:41+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "interactive", "math"]
categories: ["algorithms"]
codeforces_contest: 2136
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1046 (Div. 2)"
rating: 1700
weight: 2136
solve_time_s: 124
verified: false
draft: false
---

[CF 2136D - Dành cho nhà vô địch](https://codeforces.com/problemset/problem/2136/D) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tương tác, toán học 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$p = \beta - \alpha$là xác suất để một đơn$U_j$nằm trong khoảng$[\alpha, \beta)$, như trong phương trình (4). Một _gap chiều dài$r$_ xảy ra khi$r$liên tiếp$U$đang rơi ra ngoài$[\alpha, \beta)$, theo sau là một$U$bên trong khoảng. Đối với một chuỗi ngẫu nhiên, xác suất để khoảng trống có độ dài$r$là$\Pr(\text{gap length } = r) = (1-p)^r p, \quad r \ge 0.$Biểu thị bằng$X$số lượng$U$được yêu cầu phải quan sát một khoảng trống. Nếu chiều dài khoảng cách là$r$, sau đó$X = r+1$, vì khoảng cách bao gồm$r$thất bại nối tiếp thành công. Vì thế,$X$là một biến ngẫu nhiên hình học có xác suất thành công$p$. 

Giá trị kỳ vọng của$X$là$$E[X] = \sum_{r=0}^{\infty} (r+1)(1-p)^r p = \sum_{r=0}^{\infty} (1-p)^r p + \sum_{r=0}^{\infty} r (1-p)^r p.$$Tổng đầu tiên là$$\sum_{r=0}^{\infty} (1-p)^r p = 1,$$vì nó là tổng của chuỗi hình học với tổng xác suất là 1. Tổng thứ hai là$$\sum_{r=0}^{\infty} r (1-p)^r p = \frac{1-p}{p},$$theo công thức chuẩn tính giá trị trung bình của các lỗi đếm phân bố hình học. Kết hợp những điều này mang lại$$E[X] = 1 + \frac{1-p}{p} = \frac{1}{p}.$$Như vậy, số lượng dự kiến$U$để quan sát một khoảng cách duy nhất là$1/p$. 

Cho phép$X_1, X_2, \ldots, X_n$là bản sao độc lập của$X$, đại diện cho số lượng$U$cần phải quan sát từng$n$những khoảng trống. Khi đó tổng số$U$bắt buộc phải quan sát$n$khoảng trống là$$S_n = X_1 + X_2 + \cdots + X_n.$$Bằng tính tuyến tính của kỳ vọng,$$E[S_n] = n E[X] = \frac{n}{p}.$$Sự khác biệt của$X$là$$\operatorname{Var}(X) = \frac{1-p}{p^2},$$lại bằng công thức chuẩn cho một biến hình học đếm những lần thất bại trước lần thành công đầu tiên. Kể từ khi$X_j$độc lập,$$\operatorname{Var}(S_n) = n \operatorname{Var}(X) = \frac{n(1-p)}{p^2}.$$Độ lệch chuẩn là căn bậc hai của phương sai:$$\sigma = \sqrt{\operatorname{Var}(S_n)} = \frac{\sqrt{n(1-p)}}{p}.$$Điều này hoàn thành việc chứng minh. 

∎$$\boxed{\text{Expected number of $U$'s: } \frac{n}{p}, \quad \text{Standard deviation: } \frac{\sqrt{n(1-p)}}{p}}$$
