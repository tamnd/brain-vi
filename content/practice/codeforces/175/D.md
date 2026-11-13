---
title: "CF 175D - Máy bay tăng: Đấu tay đôi"
description: "Hai xe tăng bắt đầu giao tranh ở thời điểm 0. Cả hai khai hỏa ngay lập tức, sau đó tiếp tục bắn mỗi dt giây. Mỗi phát bắn có thể không xuyên qua được áo giáp và nếu xuyên thủng, sát thương được chọn thống nhất trong một khoảng nguyên."
date: "2026-06-02T16:56:46+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 175
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 115"
rating: 2400
weight: 175
solve_time_s: 40
verified: false
draft: false
---

[CF 175D - Máy bay xe tăng: Đấu tay đôi](https://codeforces.com/problemset/problem/175/D) 

**Đánh giá:** 2400 
**Tags:** sức mạnh vũ phu, dp, toán học, xác suất 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hai xe tăng bắt đầu giao tranh ở thời điểm 0. Cả hai khai hỏa ngay lập tức, sau đó tiếp tục bắn liên tục.`dt`giây. Mỗi phát bắn có thể không xuyên qua được áo giáp và nếu xuyên thủng, sát thương được chọn thống nhất trong một khoảng nguyên. 

Xe tăng sẽ bị tiêu diệt ngay khi điểm máu của nó trở nên không tích cực. Nếu cả hai xe tăng bắn cùng một lúc, cả hai phát bắn sẽ xảy ra đồng thời. Chi tiết này rất quan trọng vì nếu cả hai xe tăng đều chết vì phát đạn bắn vào cùng một thời điểm thì Vasya vẫn được coi là người chiến thắng. 

Đầu vào mô tả xe tăng của Vasya và xe tăng của địch. Đối với mỗi xe tăng, chúng tôi biết điểm trúng đích, thời gian nạp đạn, khoảng thời gian sát thương và xác suất phát bắn của nó không xuyên thủng áo giáp của đối thủ. 

Nhiệm vụ là tính xác suất để cuối cùng Vasya thắng. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Điểm trúng đích không bao giờ vượt quá 200, thời gian thay đạn không bao giờ vượt quá 30 và giá trị sát thương không bao giờ vượt quá 100. Các giới hạn này gợi ý rõ ràng rằng chúng ta nên mô hình hóa các điểm trúng đích còn lại một cách rõ ràng. Không gian trạng thái liên quan đến giá trị HP của cả hai xe tăng chứa tối đa khoảng`201 × 201 = 40401`trạng thái hoàn toàn có thể quản lý được. 

Phần khó khăn là thời gian. Xe tăng
