---
title: "CF 102947D - Củi"
description: "Gọi $n,m là 1$. Mục tiêu là tạo ra tất cả các phân vùng của $n$ thành nhiều nhất $m$ phần, nghĩa là các chuỗi $a1 ge a2 ge cdots ge ak ge 1,quad k le m,quad a1+cdots+ak=n."
date: "2026-07-04T07:29:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102947
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 02-05-21 Div. 1 (Advanced)"
rating: 0
weight: 102947
solve_time_s: 125
verified: false
draft: false
---

[CF 102947D - Củi](https://codeforces.com/problemset/problem/102947/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$n,m \ge 1$. Mục tiêu là tạo ra tất cả các phân vùng của$n$vào nhiều nhất$m$các bộ phận, trình tự ý nghĩa$a_1 \ge a_2 \ge \cdots \ge a_k \ge 1,\quad k \le m,\quad a_1+\cdots+a_k=n.$Quan sát quan trọng là các phân vùng như vậy tương ứng một cách khách quan với các phân vùng của$n+m$vào chính xác$m$phần tích cực thông qua một sự dịch chuyển thống nhất. 

Cho bất kỳ phân vùng nào của$n$vào nhiều nhất$m$các phần, hãy viết nó ở dạng đệm dưới dạng$m$-tuple$a_1 \ge a_2 \ge \cdots \ge a_m \ge 0,\quad a_1+\cdots+a_m=n.$Định nghĩa$b_i = a_i + 1.$Sau đó$b_1 \ge b_2 \ge \cdots \ge b_m \ge 1,$Và$b_1+\cdots+b_m = n+m.$Ngược lại, mọi phân vùng$b_1 \ge \cdots \ge b_m \ge 1$của$n+m$vào chính xác$m$parts tạo ra một phân vùng duy nhất của$n$vào nhiều nhất$m$các phần bằng cách trừ$1$từ mỗi thành phần. Số mục không có trong số$a_i$chính xác là số phần đơn vị trong số$b_i$và việc xóa các số 0 sẽ khôi phục được một phân vùng có tối đa$m$những phần tích cực. Do đó, sự tương ứng này là khách quan. 

Thuật toán H tạo tất cả các phân vùng của một số nguyên thành chính xác$m$những phần tích cực. Áp dụng nó vào$n+m$thay vì$n$sản xuất tất cả$m$-bộ dữ liệu$b_1,\dots,b_m$với$b_1+\cdots+b_m=n+m,\quad b_i\ge 1.$Do đó, việc sửa đổi cần thiết được giới hạn ở bước H1, thay thế việc khởi tạo ban đầu cho đầu vào$n$bằng cách khởi tạo cho đầu vào$n+m$. 

Ở bước H1 của Thuật toán H, phép gán$a_1 \leftarrow n - m + 1,\quad a_j \leftarrow 1 \ (1<j\le m)$được thay thế bằng phép gán có được bằng cách thay thế$n+m$vì$n$, cụ thể là$a_1 \leftarrow (n+m) - m + 1 = n+1,\quad a_j \leftarrow 1 \ (1<j\le m),$với người canh gác$a_{m+1} \leftarrow -1$không thay đổi. 

Thuật toán sau đó tạo ra tất cả các phân vùng của$n+m$vào chính xác$m$các bộ phận. Sau mỗi đầu ra, thay thế từng đầu ra$a_i$qua$a_i-1$mang lại kết quả không tăng$m$-bộ số nguyên không âm có tổng bằng$n$và việc xóa các số 0 mang lại chính xác các phân vùng của$n$vào nhiều nhất$m$các bộ phận. 

Việc sửa đổi này không ảnh hưởng đến bất kỳ bước nào sau này của Thuật toán H, vì tất cả các bất biến được sử dụng ở đó chỉ phụ thuộc vào sự khác biệt tương đối giữa các phần và việc bảo toàn tổng, cả hai đều không thay đổi khi dịch đồng nhất bởi$1$. ∎
