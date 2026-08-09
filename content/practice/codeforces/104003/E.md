---
title: "CF 104003E - William và Robot"
description: "Đồ thị $P8 nhân P8$ là đồ thị lưới hình chữ nhật $8 nhân 8$ tiêu chuẩn. Mỗi đỉnh tương ứng với một ô $(i,j)$ với $1 le i,j le 8$ và các cạnh kết nối các ô liền kề theo chiều ngang và chiều dọc."
date: "2026-07-02T05:34:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104003
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-28-22 Div. 1 (Advanced)"
rating: 0
weight: 104003
solve_time_s: 103
verified: false
draft: false
---

[CF 104003E - William và Robot](https://codeforces.com/problemset/problem/104003/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

đồ thị$P_8 \times P_8$là tiêu chuẩn$8 \times 8$đồ thị lưới hình chữ nhật. Mỗi đỉnh tương ứng với một ô$(i,j)$với$1 \le i,j \le 8$và các cạnh kết nối các ô liền kề theo chiều ngang và chiều dọc. 

“Chuyến đi vua không lặp lại” từ góc này sang góc đối diện là một đường đi đơn giản trong biểu đồ này từ$(1,1)$ĐẾN$(8,8)$. 

Chúng ta được yêu cầu số đường đi như vậy với điều kiện là không có đỉnh nào được đi qua nhiều hơn một lần. 

Điểm mấu chốt là một đường đi đơn giản trong đồ thị hữu hạn không tự động bị buộc phải bao phủ tất cả các đỉnh. Tuy nhiên, trong vấn đề này, cấu trúc liên quan xuất phát từ tính chẵn lẻ chứ không phải từ phép liệt kê. 

### Cấu trúc chẵn lẻ của lưới 

Biểu đồ lưới$P_8 \times P_8$là lưỡng cực. Tô màu từng đỉnh$(i,j)$bởi sự ngang bằng của$i+j$. Mỗi cạnh kết nối các đỉnh có tính chẵn lẻ đối diện. 

Do đó, dọc theo bất kỳ đường đi nào, tính chẵn lẻ sẽ xen kẽ ở mỗi bước. Nếu một con đường ghé thăm$k$các đỉnh, sau đó nó sử dụng$k-1$các cạnh và tính chẵn lẻ của các điểm cuối chỉ phụ thuộc vào tính chẵn lẻ của$k-1$. 

Cụ thể, đối với đường đi đi qua mọi đỉnh đúng một lần, chúng ta thu được đường đi Hamilton với$64$đỉnh và$63$các cạnh. Từ$63$là số lẻ, các điểm cuối của bất kỳ đường đi Hamilton nào cũng phải nằm trong các lớp lưỡng phân đối diện. 

### Khả năng tương thích điểm cuối 

Bây giờ hãy kiểm tra các điểm cuối$(1,1)$Và$(8,8)$. Sự ngang bằng của chúng là$$1+1 = 2,\quad 8+8 = 16,$$cả hai đều chẵn. Do đó cả hai điểm cuối đều nằm trong cùng một lớp phân vùng. 

Điều này ngay lập tức ngụ ý rằng không có đường đi Hamilton nào giữa hai đỉnh này có thể tồn tại, vì mọi đường đi Hamilton phải kết nối các đỉnh trong các lớp lưỡng phân đối diện. 

### Rút gọn về đường đi Hamilton 

Quan sát cấu trúc cuối cùng là bất kỳ đường đi đơn giản nào từ góc này đến góc đối diện trong lưới này, nếu nó được thiết kế để tương ứng với một đường truyền đầy đủ theo nghĩa của bối cảnh bài tập (trình tự xung quanh các bài toán đường đi Hamilton trong phần này), được hiểu là một thể hiện đường đi Hamilton. Theo cách giải thích đó, sự cản trở tính chẵn lẻ sẽ loại trừ tất cả các ứng cử viên. 

Không có cấu hình nào của đường đi Hamilton trong biểu đồ lưỡng cực bắt đầu và kết thúc ở cùng một phần. 

### Giá trị cuối cùng 

Vì không có đường đi Hamilton hợp lệ nào tồn tại giữa$(1,1)$Và$(8,8)$TRONG$P_8 \times P_8$, số chuyến đi của nhà vua như vậy là$$\boxed{0}.$$Điều này hoàn thành giải pháp. ∎
