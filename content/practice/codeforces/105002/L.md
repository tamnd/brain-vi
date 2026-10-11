---
title: "CF 105002L - \u041c\u043e\u043d\u0441\u0442\u0440\u044b"
description: "Chúng tôi được cung cấp một bộ sưu tập quái vật và chúng tôi muốn chọn một số trong số chúng để tối đa hóa số lượng chúng tôi bắt được, theo giới hạn toàn cầu về mức độ “hung hăng” tổng thể. Mỗi quái vật hành xử theo một cách hơi có điều kiện."
date: "2026-06-28T03:21:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105002
codeforces_index: "L"
codeforces_contest_name: "vkoshp.letovo 2022"
rating: 0
weight: 105002
solve_time_s: 23
verified: false
draft: false
---

[CF 105002L - \u041c\u043e\u043d\u0441\u0442\u0440\u044b](https://codeforces.com/problemset/problem/105002/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập quái vật và chúng tôi muốn chọn một số trong số chúng để tối đa hóa số lượng chúng tôi bắt được, theo giới hạn toàn cầu về mức độ “hung hăng” tổng thể. 

Mỗi quái vật hành xử theo một cách hơi có điều kiện. Nếu chúng ta quyết định lấy một bộ quái vật có kích thước$k$, thì đối với mỗi quái vật sẽ có một ngưỡng$c_i$. Nếu kích thước được chọn cuối cùng$k$không vượt quá ngưỡng này, quái vật đóng góp giá trị thấp hơn$a_i$. Nếu kích thước được chọn lớn hơn$c_i$, con quái vật trở nên nguy hiểm hơn và góp phần$b_i$, Ở đâu$b_i \ge a_i$. 

Mục tiêu là chọn một tập hợp con quái vật sao cho khi đóng góp của chúng được tính theo kích thước cuối cùng của tập hợp con, tổng số tiền không vượt quá$s$và kích thước của tập hợp con càng lớn càng tốt. 

Khó khăn chính là chi phí của một quái vật được chọn phụ thuộc vào tổng số quái vật chúng ta chọn, do đó giá trị của một tập hợp con cố định không được cố định trước. 

c
