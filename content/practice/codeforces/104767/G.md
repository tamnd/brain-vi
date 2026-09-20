---
title: "CF 104767G - Hamster"
description: "Chúng ta được cấp một tập hợp các phân đoạn đơn vị được vẽ trên lưới số nguyên. Mỗi đoạn kết nối hai điểm giao nhau lưới cách nhau chính xác một bước theo chiều ngang hoặc chiều dọc."
date: "2026-06-29T02:28:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104767
codeforces_index: "G"
codeforces_contest_name: "2023-2024 CTU Open Contest"
rating: 0
weight: 104767
solve_time_s: 33
verified: false
draft: false
---

[CF 104767G - Hamster](https://codeforces.com/problemset/problem/104767/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các phân đoạn đơn vị được vẽ trên lưới số nguyên. Mỗi đoạn kết nối hai điểm giao nhau lưới cách nhau chính xác một bước theo chiều ngang hoặc chiều dọc. Nếu chúng ta xem mọi giao điểm lưới là một đỉnh và mỗi đoạn đã cho là một cạnh vô hướng thì đầu vào sẽ mô tả một sơ đồ con thưa thớt của biểu đồ lưới vô hạn. 

Hamster di chuyển dọc theo các cạnh lưới giữa các đỉnh liền kề miễn là không có đoạn tường nào chặn chuyển động đó. Mục đích là cài đặt các phân đoạn đơn vị bổ sung để hamster bị mắc kẹt bên trong ít nhất một khu vực khép kín, nghĩa là có ít nhất một khuôn mặt được bao bọc trong bản vẽ phẳng thu được. Về mặt biểu đồ, điều này tương ứng với việc tạo ra ít nhất một chu trình đơn giản trong biểu đồ cuối cùng, bởi vì bất kỳ chu trình nào trong biểu đồ lưới đều bao quanh một vùng không thể thoát ra nếu không vượt qua một cạnh. 

Nhiệm vụ là xác định số cạnh đơn vị bổ sung tối thiểu phải được thêm vào giữa các điểm lưới liền kề hiện chưa được sử dụng để đảm bảo rằng biểu đồ kết quả chứa ít nhất một chu trình. 

Những hạn chế
