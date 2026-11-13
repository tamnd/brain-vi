---
title: "CF 1742G - Orray"
description: "Nhiệm vụ là sắp xếp lại một mảng các số nguyên không âm cho trước sao cho mảng OR tiền tố có độ lớn về mặt từ điển càng lớn càng tốt."
date: "2026-06-09T16:18:34+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1742
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 827 (Div. 4)"
rating: 1500
weight: 1742
solve_time_s: 120
verified: false
draft: false
---

[CF 1742G - Orray](https://codeforces.com/problemset/problem/1742/G) 

**Đánh giá:** 1500 
**Tags:** bitmasks, vũ phu, tham lam, toán học, sắp xếp 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là sắp xếp lại một mảng các số nguyên không âm cho trước sao cho mảng OR tiền tố có độ lớn về mặt từ điển càng lớn càng tốt. Tiền tố mảng OR được định nghĩa là$b_i = a_1 ,\mathsf{OR}, a_2 ,\mathsf{OR}, \dots ,\mathsf{OR}, a_i$, nghĩa là mỗi phần tử$b_i$đại diện cho bitwise tích lũy HOẶC từ đầu mảng đến vị trí$i$. Mục tiêu là hoán vị$a$để tối đa hóa$b$theo thứ tự từ điển, có nghĩa là phần tử đầu tiên của$b$phải càng lớn càng tốt và nếu có bằng nhau thì phần tử thứ hai phải càng lớn càng tốt, v.v. 

Đầu vào chứa nhiều trường hợp thử nghiệm và tổng kích thước của tất cả các mảng không vượt quá$2 \cdot 10^5$. Ràng buộc này ngụ ý rằng các thuật toán với$O(n^2)$sự phức tạp sẽ không hoạt động hiệu quả, nhưng$O(n \log n)$hoặc$O(n \cdot 30)$các phép toán có thể chấp nhận được vì mỗi số nhiều nhất là$10^9$, nghĩa là nhiều nhất
