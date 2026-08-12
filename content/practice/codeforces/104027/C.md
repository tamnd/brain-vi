---
title: "CF 104027C - \u5f02\u6216"
description: "Giả sử ZDD đại diện cho một họ $mathcal{F}$ gồm các tập con của ${x1,dots,xn}$, được sắp xếp theo các chỉ số biến và để mỗi nút $k$ được gắn nhãn bởi $V(k)in{1,dots,n}$."
date: "2026-07-02T04:09:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104027
codeforces_index: "C"
codeforces_contest_name: "The 10-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 104027
solve_time_s: 123
verified: false
draft: false
---

[CF 104027C - \u5f02\u6216](https://codeforces.com/problemset/problem/104027/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Hãy để ZDD đại diện cho một gia đình$\mathcal{F}$tập hợp con của${x_1,\dots,x_n}$, được sắp xếp theo các chỉ số thay đổi và để mỗi nút$k$được dán nhãn bởi$V(k)\in{1,\dots,n}$. Cho phép$\mathrm{LO}(k)$Và$\mathrm{HI}(k)$biểu thị các phần tử con của nó, với ngữ nghĩa ZDD là$\mathrm{LO}(k)$loại trừ$x_{V(k)}$Và$\mathrm{HI}(k)$bao gồm$x_{V(k)}$. 

Giới thiệu các nút đầu cuối$\bot$Và$\top$, với$\top$đại diện cho gia đình${\emptyset}$Và$\bot$đại diện cho gia đình trống rỗng. Khi đó số giải pháp đóng góp của$\top$là$1$và bởi$\bot$là$0$. 

Đối với mỗi nút$k$, cho phép$F(k)$biểu thị số lượng giải pháp được đại diện bởi ZDD phụ bắt nguồn từ$k$, trong đó các giải pháp được tính là phép gán đầy đủ của các biến chưa cố định dọc theo đường dẫn. Nếu một đường dẫn đến một nút có nhãn$j$sau khi nhìn thấy chỉ số biến lần cuối$i<j$, thì các biến$x_{i+1},\dots,x_{j-1}$không bị ràng buộc và mỗi yếu tố đóng góp một yếu tố$2$. 

Để thể hiện điều này một cách chính thức, hãy mở rộng chỉ số biến tới các thiết bị đầu cuối bằng cách đặt$V(\top)=V(\bot)=n+1$. Khi đó mỗi cung từ một nút$k$đến một đứa trẻ$c\in{\mathrm{LO}(k),\mathrm{HI}(k)}$bỏ qua chính xác$V(c)-V(k)-1$các biến, đóng góp một yếu tố$2^{V(c)-V(k)-1}$. 

Do đó, sự đóng góp của mỗi phần tử con là số lượng giải pháp từng phần trong cây con nhân với số lượng bài tập trống do các biến bị bỏ qua gây ra. Điều này mang lại sự tái diễn$$F(k)=\sum_{c\in\{\mathrm{LO}(k),\mathrm{HI}(k)\}} 2^{V(c)-V(k)-1}\,F(c),$$với các giá trị cơ bản$$F(\bot)=0,\qquad F(\top)=1.$$Sự lặp lại này được đánh giá trong một lần duyệt chính xác như trong Thuật toán C để tính BDD, ngoại trừ hệ số nhân phụ thuộc vào các khoảng trống cấp độ thay vì đồng nhất. 

Để triển khai điều này như một sửa đổi của Thuật toán C, hãy lưu trữ các giá trị được tính toán của$F(k)$trong một bảng để tránh phải tính toán lại. Khi xử lý một nút$k$, đầu tiên tính toán đệ quy$F(\mathrm{LO}(k))$Và$F(\mathrm{HI}(k))$, sau đó kết hợp chúng bằng cách sử dụng hệ số được xác định bởi sự khác biệt về cấp độ của chúng so với$V(k)$như thể hiện trong sự tái phát. Cấu trúc ghi nhớ giống hệt với Thuật toán C, vì mỗi nút được đánh giá một lần trong ZDD rút gọn. 

Điều này tính toán số lượng phép gán thỏa mãn của hàm Boolean được biểu thị bằng ZDD, vì mỗi đường dẫn từ gốc đến đầu cuối tương ứng với một phép gán từng phần duy nhất và mỗi biến bị bỏ qua sẽ nhân đôi số phần mở rộng đầy đủ nhất quán. 

Điều này hoàn thành việc chứng minh. ∎
