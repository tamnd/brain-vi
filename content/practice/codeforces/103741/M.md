---
title: "CF 103741M - XOR Hầu hết mọi thứ"
description: "Giả sử $f(x1,dots,xn)$ là một hàm Boolean, và $G(z)$ là hàm sinh của nó theo nghĩa của Bài tập 25, sao cho $$G(z)=sum{xin{0,1}^n} f(x), z^{w(x)},$$ trong đó $w(x)=x1+cdots+xn$ là trọng số Hamming của $x$."
date: "2026-07-02T09:08:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103741
codeforces_index: "M"
codeforces_contest_name: "HUSTPC 2022"
rating: 0
weight: 103741
solve_time_s: 125
verified: false
draft: false
---

[CF 103741M - XOR Hầu hết mọi thứ](https://codeforces.com/problemset/problem/103741/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

hãy để$f(x_1,\dots,x_n)$là một hàm Boolean và đặt$G(z)$là hàm sinh của nó theo nghĩa của Bài tập 25, sao cho$$G(z)=\sum_{x\in\{0,1\}^n} f(x)\, z^{w(x)},$$Ở đâu$w(x)=x_1+\cdots+x_n$là trọng lượng Hamming của$x$. Như vậy$G(z)$là phép tính đa thức sinh thông thường thỏa mãn các bài tập của$f$theo trọng lượng. 

Cho rằng$G(-1)\neq 0$. Mở rộng,$$G(-1)=\sum_{x\in\{0,1\}^n} f(x)\, (-1)^{w(x)}.$$Chia bài tập theo biến đầu tiên. Mọi$x\in{0,1}^n$được viết duy nhất là$(0,y)$hoặc$(1,y)$với$y\in{0,1}^{n-1}$. Sau đó$$G(-1)=\sum_{y} f(0,y)(-1)^{w(y)} + \sum_{y} f(1,y)(-1)^{1+w(y)}.$$Điều này trở thành$$G(-1)=\sum_{y} \bigl(f(0,y)-f(1,y)\bigr)(-1)^{w(y)}.$$Từ$f(0,y),f(1,y)\in{0,1}$, mỗi hệ số$f(0,y)-f(1,y)$nằm ở${-1,0,1}$. 

Bây giờ hãy xem xét bất kỳ FBDD nào cho$f$. Dọc theo bất kỳ đường dẫn từ gốc đến đích nào, mỗi biến được kiểm tra đều được cố định một lần và không có biến nào lặp lại trên đường dẫn đó. Giả sử một đường dẫn truy vấn ít hơn$n$các biến. Khi đó tồn tại ít nhất một biến$x_i$chưa được thử nghiệm trên con đường đó. Sửa tất cả các biến được kiểm tra theo đường dẫn và thay đổi$x_i$. Điều này tạo ra hai phép gán đầy đủ phù hợp với tất cả các biến được truy vấn nhưng khác nhau về giá trị của$x_i$. 

Trong FBDD, việc chấp nhận hoặc từ chối trên đường dẫn đó độc lập với các biến không được truy vấn, vì đường dẫn không bao giờ phân nhánh trên$x_i$. Do đó, giá trị hàm được đóng góp bởi tất cả các lần hoàn thành của đường dẫn đó là không đổi đối với$x_i$. Đường đi như vậy đóng góp ít nhất một khối con có chiều$1$vào bảng chân lý, do đó đóng góp một thuật ngữ vào$G(z)$đánh giá của ai tại$z=-1$hủy theo cặp khi lật biến tự do. Chính xác hơn, đối với bất kỳ khối con nào có ít nhất một biến là tự do, sự đóng góp của các phép gán chỉ khác nhau trên biến đó sẽ bị hủy trong phép xác định tổng xen kẽ$G(-1)$. 

Do đó, bất kỳ FBDD không lảng tránh nào, một FBDD trong đó mọi đường dẫn từ gốc đến gốc đều có độ dài tối đa$n-1$, phân tách tập hợp các phép gán thỏa mãn thành các cặp khác nhau trên ít nhất một biến tự do dọc theo mỗi khối con lá, buộc phải hủy bỏ tổng số trong tổng xen kẽ. Điều này ngụ ý$G(-1)=0$. 

Lấy điều trái ngược, nếu$G(-1)\neq 0$, việc hủy bỏ như vậy không thể xảy ra. Do đó mọi đường dẫn root-to-sink trong mỗi FBDD phải truy vấn tất cả$n$các biến, vì nếu không thì việc phân tách thành các khối con do các biến bị thiếu sẽ buộc$G(-1)=0$. 

Vì vậy mỗi FBDD cho$f$chứa một đường đi xuống có độ dài$n$, Vì thế$f$là lảng tránh. 

Điều này hoàn thành việc chứng minh. ∎
