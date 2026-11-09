---
title: "CF 1498E - Hai Ngôi Nhà"
description: "Chúng ta có một thành phố có $n$ ngôi nhà, trong đó mỗi cặp nhà có chính xác một con đường nối chúng từ nhà thứ nhất đến ngôi nhà thứ hai hoặc ngược lại. Đầu vào không chỉ định hướng của những con đường này."
date: "2026-06-10T21:38:33+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "graphs", "greedy", "interactive", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1498
codeforces_index: "E"
codeforces_contest_name: "CodeCraft-21 and Codeforces Round 711 (Div. 2)"
rating: 2200
weight: 1498
solve_time_s: 160
verified: false
draft: false
---

[CF 1498E - Hai ngôi nhà](https://codeforces.com/problemset/problem/1498/E) 

**Đánh giá:** 2200 
**Tags:** bạo lực, đồ thị, tham lam, tương tác, sắp xếp 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một thành phố với$n$những ngôi nhà, trong đó mỗi cặp ngôi nhà có chính xác một con đường nối liền chúng, từ ngôi nhà thứ nhất đến ngôi nhà thứ hai hoặc ngược lại. Đầu vào không chỉ định hướng của những con đường này. Thay vào đó, chúng ta được cung cấp một mảng$k$Ở đâu$k_i$là số đường dẫn vào nhà$i$. 

Chúng tôi được yêu cầu tìm một cặp ngôi nhà$A$Và$B$như vậy$A$Và$B$có thể tiếp cận được hai bên, nghĩa là mỗi cặp có thể tiếp cận nhau thông qua một số đường dẫn và trong số tất cả các cặp như vậy, chúng tôi muốn một cặp tối đa hóa$|k_A - k_B|$. Nếu không có cặp như vậy tồn tại, chúng tôi nên báo cáo$(0, 0)$. 

Vấn đề là tương tác. Chúng ta có thể hỏi liệu nhà này có thể đến nhà khác hay không, nhưng khi thẩm phán trả lời "Có" cho bất kỳ câu hỏi nào, chúng ta phải ngừng đặt câu hỏi và đưa ra câu trả lời của mình. Không có giới hạn trên đối với truy vấn nhưng các truy vấn phải là duy nhất và được định dạng chính xác. 

T
