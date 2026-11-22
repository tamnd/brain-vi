---
title: "CF 293C - Vấn đề về khối lập phương"
description: "Ba hình lập phương ban đầu có độ dài các cạnh $a$, $b$ và $c$. Tổng số khối lập phương đơn vị của họ là $$a^3+b^3+c^3.$$ Vitaly muốn xây dựng một khối lập phương lớn hơn có chiều dài cạnh là $a+b+c$, điều này đòi hỏi $$(a+b+c)^3$$ khối lập phương đơn vị."
date: "2026-06-05T17:22:05+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 293
codeforces_index: "C"
codeforces_contest_name: "Croc Champ 2013 - Round 2"
rating: 2400
weight: 293
solve_time_s: 157
verified: false
draft: false
---

[CF 293C - Vấn đề về khối lập phương](https://codeforces.com/problemset/problem/293/C) 

**Đánh giá:** 2400 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ba hình lập phương ban đầu có độ dài cạnh$a$,$b$, Và$c$. Tổng số khối đơn vị của chúng là$$a^3+b^3+c^3.$$Vitaly muốn xây một khối lập phương lớn hơn có cạnh dài bằng$a+b+c$, điều này sẽ yêu cầu$$(a+b+c)^3$$khối đơn vị. 

Chính xác là anh ấy thấp$n$hình khối, vì vậy$$(a+b+c)^3-(a^3+b^3+c^3)=n.$$Chúng tôi chỉ được cấp$n$, và chúng ta phải đếm có bao nhiêu bộ ba số nguyên dương$(a,b,c)$có thể đã tạo ra giá trị đó. 

Sự ràng buộc$n \le 10^{14}$ngay lập tức loại trừ mọi tìm kiếm trên các giá trị có thể có của$a$,$b$, hoặc$c$. Rễ khối của$10^{14}$đang ở xung quanh$4.6 \times 10^4$, do đó, ngay cả một vòng lặp ba trên tất cả các độ dài cạnh có thể sẽ cần nhiều hơn$10^{13}$lần lặp lại. 

Quan sát quan trọng là biểu thức có cấu trúc đại số mạnh. Một khi nó được chuyển thành một phương trình nhân, bài toán sẽ trở thành một trong những ước số của một số lên đến khoảng$3.3 \times 10^{13}$. Các số có kích thước đó có tương đối ít ước số, khiến cho việc liệt kê dựa trên ước số trở nên thực tế. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Ví dụ, nếu```
n = 1
```sau đó$$(a+b+c)^3-(a^3+b^3+c^3)$$luôn chia hết cho$3$, vậy đáp án phải là$0$. Một giải pháp bắt đầu liệt kê các ước số mà không kiểm tra khả năng chia hết cho$3$lãng phí và có thể tạo ra kết quả không chính xác. 

Một trường hợp tế nhị khác là```
n = 24
```tương ứng với$$3(a+b)(b+c)(c+a)=24.$$Hệ số nhân ba$(1,1,8)$thỏa mãn điều kiện tích, nhưng nó không tương ứng với các giá trị nguyên dương của$a,b,c$. Đang phục hồi$a,b,c$đòi hỏi cả các ràng buộc chẵn lẻ và tích cực. Bỏ qua những hạn chế đó. 

Sai lầm phổ biến thứ ba là quên điều đó$(a,b,c)$được ra lệnh. Bộ ba$(1,2,3)$Và$(2,1,3)$là những giải pháp khác nhau. Khi liệt kê các bộ ba thừa số đã được sắp xếp, chúng ta phải khôi phục bội số chính xác. 

## Phương pháp tiếp cận 

Bắt đầu từ danh tính$$(a+b+c)^3-(a^3+b^3+c^3) = 3(a+b)(b+c)(c+a).$$Cho phép$$x=a+b,\quad y=b+c,\quad z=c+a.$$Sau đó vấn đề trở thành$$3xyz=n.$$Nếu như$n$không chia hết cho$3$, không có giải pháp. 

Một ý tưởng mạnh mẽ sẽ là liệt kê tất cả các bộ ba yếu tố$(x,y,z)$sản phẩm của ai là$n/3$, xây dựng lại$a,b,c$, và đếm những cái hợp lệ. Điều này đã tốt hơn nhiều so với việc liệt kê$a,b,c$, vì số ước của a$10^{13}$- số nguyên có kích thước nhỏ. 

Thử thách còn lại là tính toán hiệu quả. 

Được cho$$m=\frac n3,$$chúng ta cần tất cả các bộ ba số nguyên dương thỏa mãn$$xyz=m.$$Từ$$a=\frac{x+z-y}{2},\quad b=\frac{x+y-z}{2},\quad c=\frac{y+z-x}{2},$$bộ ba tương ứng với số nguyên dương hợp lệ chính xác khi$$x+y+z \equiv 0 \pmod 2$$và bất đẳng thức tam giác chặt chẽ có giá trị:$$x+y>z,\quad y+z>x,\quad z+x>y.$$Vì chúng tôi liệt kê các bộ ba nhân tố theo thứ tự được sắp xếp$x \le y \le z$, chỉ một$$x+y>z$$cần phải được kiểm tra. 

Việc liệt kê hệ số vũ phu là chính xác, nhưng việc liệt kê tất cả các bộ ba dương một cách độc lập vẫn sẽ quá lớn. Sự quan sát đó$xyz=m$chúng ta chỉ liệt kê các ước của$m$. Ngay cả đối với điều tồi tệ nhất có thể$m$, số lượng ước số chỉ là vài nghìn, vì vậy việc kiểm tra các cặp ước số là đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$a,b,c$|$O((\sqrt[3]{n})^3)$|$O(1)$| Quá chậm | 
| Đếm số chia |$O(d(m)^2)$|$O(d(m))$| Đã chấp nhận | 

Đây$d(m)$biểu thị số ước của$m$. 

## Hướng dẫn thuật toán 

1. Đọc$n$.
