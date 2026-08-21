---
title: "CF 104090F - Da Mi Lao Shi Ai Kan De"
description: "Trong ZDD, mỗi cấp độ tương ứng với một biến và nút có nhãn $k$ thể hiện quyết định trên $xk$, trong đó cạnh thấp loại trừ biến và cạnh cao bao gồm biến đó trong họ tập hợp được biểu thị."
date: "2026-07-02T02:32:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104090
codeforces_index: "F"
codeforces_contest_name: "The 2022 ICPC Asia Hangzhou Regional Programming Contest"
rating: 0
weight: 104090
solve_time_s: 133
verified: false
draft: false
---

[CF 104090F - Da Mi Lao Shi Ai Kan De](https://codeforces.com/problemset/problem/104090/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

## Giải pháp 
Trong ZDD, mỗi cấp độ tương ứng với một biến và một nút được gắn nhãn$k$đại diện cho một quyết định về$x_k$, trong đó cạnh thấp loại trừ biến và cạnh cao bao gồm biến đó trong họ tập hợp được biểu diễn. bồn rửa$\perp$đại diện cho gia đình trống rỗng, trong khi$\top$đại diện cho họ chỉ chứa tập rỗng. 

ZDD được hiển thị là một nút duy nhất được gắn nhãn$x_3$cạnh thấp của nó đi tới$\perp$và cạnh cao đi đến$\top$. Điều này có nghĩa là khi$x_3=0$, không có tập hợp con nào được chấp nhận và khi$x_3=1$, tập hợp con duy nhất được chấp nhận là phần tiếp theo trống sau khi chọn$x_3$. 

Do đó, họ đại diện bao gồm chính xác một bộ:${3}$. Tất cả các biến khác$x_1,x_2,x_4,x_5,x_6$không xuất hiện ở bất kỳ nút nào, vì vậy chúng phải bị buộc phải$0$trong mọi nhiệm vụ thỏa mãn. 

Do đó, hàm Boolean là chỉ báo của phép gán đơn lẻ trong đó$x_3=1$và tất cả các biến khác là$0$:$$f(x_1,x_2,x_3,x_4,x_5,x_6) = x_3 \cdot \overline{x_1}\,\overline{x_2}\,\overline{x_4}\,\overline{x_5}\,\overline{x_6}.$$Tương đương, đó là hàm đặc trưng của tập đơn${{3}}$trong biểu diễn tập hợp con.
