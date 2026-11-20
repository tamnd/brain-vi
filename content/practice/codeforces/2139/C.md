---
title: "CF 2139C - Phân công bánh"
description: "Giải pháp giải thích chính xác vấn đề. Một \"khoảng cách độ dài $r$\" là số $U$ liên tiếp nằm ngoài $[alpha, beta)$ trước khi $U$ tiếp theo rơi vào khoảng đó."
date: "2026-06-09T04:13:21+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2139
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1048 (Div. 2)"
rating: 1100
weight: 2139
solve_time_s: 65
verified: false
draft: false
---

[CF 2139C - Bài tập làm bánh](https://codeforces.com/problemset/problem/2139/C) 

**Đánh giá:** 1100 
**Tags:** bitmasks, thuật toán xây dựng, tham lam 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Tính đúng đắn 

Giải pháp giải thích chính xác vấn đề. Một “khoảng cách về chiều dài$r$" là số liên tiếp$U$đang rơi ra ngoài$[\alpha, \beta)$trước lần tiếp theo$U$rơi vào khoảng đó. Đây là kịch bản phân bố hình học tiêu chuẩn, trong đó thành công xảy ra với xác suất$p = \beta - \alpha$. 

Giải pháp xác định chính xác$X$như số lượng$U$được yêu cầu để quan sát một khoảng cách duy nhất. Bằng cách xác định$X = r+1$cho một khoảng cách chiều dài$r$, giải pháp làm giảm vấn đề tính toán giá trị trung bình và phương sai của các lỗi đếm biến ngẫu nhiên hình học trước lần thành công đầu tiên. Các công thức$$E[X] = \frac{1}{p}, \quad \operatorname{Var}(X) = \frac{1-p}{p^2}$$là đúng và chuẩn. 

Vì$n$khoảng cách độc lập, tổng hợp$n$các biến hình học độc lập để có được$S_n$sử dụng chính xác tính tuyến tính của kỳ vọng và tính độc lập của phương sai. Các biểu thức kết quả$$E[S_n] = \frac{n}{p}, \quad \sigma = \sqrt{\operatorname{Var}(S_n)} = \frac{\sqrt{n(1-p)}}{p}$$do đó được biện minh và trả lời trực tiếp câu hỏi. 

## Khoảng trống và lỗi 

Tất cả các bước trong lập luận đều chuẩn và được áp dụng đúng. Giải pháp nêu rõ mô hình xác suất, xác định các biến ngẫu nhiên, tính toán kỳ vọng và phương sai bằng cách sử dụng các công thức tiêu chuẩn và áp dụng tính độc lập. Không có bước nào bị thiếu, lập luận vòng tròn hoặc tuyên bố chưa được chứng minh. Công thức tính phương sai và độ lệch chuẩn của một biến số hình học không đếm được trước lần thành công đầu tiên được gọi chính xác. 

## Tóm tắt 

Giải pháp đề xuất tính toán chính xác số lượng dự kiến$U$bắt buộc phải quan sát$n$khoảng trống và độ lệch chuẩn tương ứng. Lý do đầy đủ, chặt chẽ và phù hợp với lý thuyết xác suất tiêu chuẩn. 

XÁC MINH: ĐẠT - giải pháp là chính xác và đầy đủ.
