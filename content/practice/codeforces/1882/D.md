---
title: "CF 1882D - Cây XOR"
description: "Chúng ta có một cây trong đó mỗi đỉnh chứa một số. Sau khi chọn một gốc, chúng ta có thể liên tục chọn một đỉnh và XOR mọi giá trị bên trong cây con của nó bằng một số nguyên không âm c. Việc áp dụng một câu thần chú như vậy có giá (kích thước cây con) × c."
date: "2026-06-08T22:35:30+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1882
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 899 (Div. 2)"
rating: 1900
weight: 1882
solve_time_s: 47
verified: false
draft: false
---

[CF 1882D - Cây XOR](https://codeforces.com/problemset/problem/1882/D) 

**Xếp hạng:** 1900 
**Thẻ:** bitmasks, dfs và tương tự, dp, tham lam, cây cối 
**Thời gian giải:** 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây trong đó mỗi đỉnh chứa một số. Sau khi chọn gốc, chúng ta có thể liên tục chọn một đỉnh và XOR mọi giá trị bên trong cây con của nó bằng một số nguyên không âm`c`. 

Áp dụng một câu thần chú như vậy sẽ tốn kém`(subtree size) × c`. 

Với mọi lựa chọn gốc có thể, chúng ta phải tìm tổng chi phí tối thiểu cần thiết để làm cho tất cả các giá trị đỉnh bằng nhau. 

Gốc có vai trò quan trọng vì khái niệm về cây con thay đổi khi cây có gốc khác nhau. Cùng một cạnh có thể tách cây thành các mối quan hệ cha-con khác nhau tùy thuộc vào gốc được chọn, do đó chi phí tối ưu thường khác nhau đối với các gốc khác nhau. 

Những hạn chế là lớn. Trong tất cả các trường hợp thử nghiệm, tổng số đỉnh nhiều nhất là`2 · 10^5`. Bất kỳ giải pháp nào tính toán lại câu trả lời một cách độc lập cho mọi gốc sẽ yêu cầu khoảng`O(n^2)`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 3 giây. Chúng tôi cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế là giá trị chung cuối cùng không phải là f
