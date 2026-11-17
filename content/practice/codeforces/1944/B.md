---
title: "CF 1944B - XOR bằng nhau"
description: "Đây là bài toán Loại A: tìm tất cả các số tự nhiên $x$ thỏa mãn điều kiện cho trước. Đối với một bài toán Loại A, lời giải phải làm được hai việc: 1. Suy ra các điều kiện cần thiết và chứng minh rằng mọi lời giải đều phải thỏa mãn các điều kiện đó. 2."
date: "2026-06-09T01:53:09+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1944
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 934 (Div. 2)"
rating: 1100
weight: 1944
solve_time_s: 133
verified: false
draft: false
---

[CF 1944B - XOR bằng](https://codeforces.com/problemset/problem/1944/B) 

**Đánh giá:** 1100 
**Tas:** bitmask, thuật toán xây dựng 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

## Giải pháp 
## Kiểm tra loại sự cố 

Đây là bài toán loại A: tìm mọi số tự nhiên$x$thỏa mãn một điều kiện nhất định. 

Đối với bài toán Loại A, lời giải phải thực hiện hai việc: 

1. Rút ra các điều kiện cần thiết và chứng minh mọi giải pháp đều phải thỏa mãn các điều kiện đó. 
2. Chứng minh rằng mọi ứng viên sống sót sau những điều kiện đó đều thực sự là một giải pháp. 

Giải pháp được đề xuất sẽ xác minh$x=5$, và nó cố gắng chứng minh rằng không có nghiệm nào khác tồn tại. Vì vậy, cấu trúc phù hợp với loại vấn đề. 

## Xác minh từng bước 

### Bước 1: Nếu$x$có$n$chữ số, sau đó thêm tiền tố vào chữ số$1$cho$10^n+x$- HỢP LỆ 

Điều này đúng. Đối với một$n$-số có chữ số$x$, đặt một chữ số$1$trước khi nó mang lại kết quả$1\cdot10^n+x$. 

### Bước 2: Đạo hàm của$$\frac{x(x+1)}2=10^n+x$$và sau đó$$x^2-x-2\cdot10^n=0$$- HỢP LỆ 

Đại số là chính xác. 

### Bước 3: Chuyển đổi sang$$(2x-1)^2=1+8\cdot10^n$$và định nghĩa$y=2x-1$- HỢP LỆ 

Thật vậy,$$4x^2-4x+1=1+8\cdot10^n.$$Không có vấn đề gì. 

### Bước 4: Phân tích nhân tử$$(y-1)(y+1)=2^{n+3}5^n$$- HỢP LỆ 

Kể từ khi$$y^2-1=8\cdot10^n       =2^3\cdot2^n5^n       =2^{n+3}5^n.$$Chính xác. 

### Bước 5:$\gcd(y-1,y+1)=2$- HỢP LỆ 

Kể từ khi$y$là số lẻ, cả hai thừa số đều chẵn và$$\gcd(y-1,y+1)\mid 2.$$Vì cả hai đều chẵn nên gcd chính xác$2$. 

Đã kiểm tra các ví dụ lẻ$y=9$Và$y=31$:$\gcd(8,10)=2$,$\gcd(30,32)=2$. 

### Bước 6: Tất cả quyền hạn của$5$nằm ở một yếu tố - HỢP LỆ 

Vì gcd là$2$, các thừa số không có ước số nguyên tố lẻ. Do đó toàn bộ yếu tố$5^n$phải xảy ra ở đúng một yếu tố. 

Chính xác. 

### Bước 7: Viết WLOG$$y-1=2^a,\qquad y+1=2^b5^n$$với$a+b=n+3$- HỢP LỆ 

Vì một yếu tố không chứa yếu tố nào$5$, và số nguyên tố duy nhất còn lại trong tích là$2$, yếu tố đó phải là sức mạnh thuần túy của$2$. 

Điều này là hợp lý. 

### Bước 8: Từ$$2^b5^n-2^a=2$$suy ra$$2^{b-1}5^n-2^{a-1}=1$$và do đó$b=1$- HỢP LỆ 

Nếu$b\ge2$, thì số hạng đầu tiên là số chẵn. 

Bởi vì$a\ge1$, số hạng thứ hai cũng chẵn. 

Thậm chí trừ thậm chí không thể bằng$1$. 

Kể từ đây$b=1$. 

Lập luận này là đúng đắn. 

### Bước 9: Nhận$$5^n=2^{a-1}+1$$- HỢP LỆ 

Thay thế trực tiếp$b=1$. 

### Bước 10: Hiển thị$n$phải chẵn bằng cách giảm modulo$8$- HỢP LỆ 

Đối số là: 

Kể từ khi$a-1\ge2$,$$2^{a-1}\equiv0\pmod8,$$kể từ đây$$5^n=2^{a-1}+1\equiv1\pmod8.$$Nhưng$$5^n\equiv5\pmod8$$kỳ quặc$n$. 

Vì thế$n$là chẵn. 

Chính xác. 

Kiểm tra: 

-$n=3$:$5^3=125\equiv5\pmod8$. 
-$n=4$:$625\equiv1\pmod8$. 

Hoạt động. 

### Bước 11: Cho$n=2m$, sau đó$$(5^m-1)(5^m+1)=2^{a-1}$$- HỢP LỆ 

Hệ số hóa đơn giản. 

### Bước 12: Vì sản phẩm là sức mạnh của$2$, mỗi yếu tố phải là lũy thừa của$2$- HỢP LỆ 

Cả hai thừa số đều là số nguyên dương và tích của chúng không chứa thừa số nguyên tố lẻ. 

Do đó mỗi yếu tố tự nó là một sức mạnh của$2$. 

Chính xác. 

### Bước 13: Hai lũy thừa của$2$khác nhau bởi$2$phải là$2$Và$4$- HỢP LỆ 

Bằng chứng đưa ra là đủ. 

Thực vậy,$$2^r+2=2^s$$ngụ ý$$2^{r-1}+1=2^{s-1}.$$Vế trái chỉ lẻ khi$r=1$, mang lại$s=2$. 

Chính xác. 

### Bước 14: Suy ra$$5^m-1=2,\qquad 5^m+1=4$$và nhận được mâu thuẫn - HỢP LỆ 

Điều này mang lại$5^m=3$, không thể nào. 

Kể từ đây$n\ge2$không đưa ra giải pháp. 

Chính xác. 

### Bước 15: Kiểm tra$n=1$- CÓ HIỆU LỰC$$y^2=81$$cho$y=9$(chỉ nghiệm dương, vì$y=2x-1>0$). 

Sau đó$$x=\frac{9+1}{2}=5.$$Xác minh:$$1+2+3+4+5=15.$$Chính xác. 

## Kiểm tra tính đầy đủ 

Bằng chứng bao gồm tất cả các độ dài chữ số có thể$n$. 

Vụ án$n=1$được xử lý riêng biệt và mang lại giải pháp$x=5$. 

Vụ án$n\ge2$được giảm đến mức mâu thuẫn. 

Không có giả định ẩn nào vẫn chưa được chứng minh. 

Một vấn đề nhỏ xuất hiện trong phần "Khám phá" ban đầu: 

>$x=14$cho$105$, có được bằng cách viết$1$trước mặt$05$. 

Quan sát này không chính thức và không liên quan, nhưng nó không được sử dụng sau này và không ảnh hưởng đến chứng minh. 

Bản thân bằng chứng chính thức đã hoàn tất. 

Kết luận cuối cùng rút ra chính xác từ những gì đã được chứng minh. 

## Tóm tắt 

Đối số biến đổi chính xác điều kiện thành phương trình kiểu Pell$$y^2=1+8\cdot10^n,$$các yếu tố nó như$$(y-1)(y+1)=2^{n+3}5^n,$$sử dụng thực tế là các yếu tố khác nhau bởi$2$, buộc một thừa số phải chứa tất cả lũy thừa của$5$, dẫn xuất$b=1$, chứng minh$n$phải chẵn, và cuối cùng đạt được một hệ số không thể có được của lũy thừa$2$. 

Tất cả các trường hợp đều được giải quyết và mọi bước quan trọng đều được chứng minh. 

XÁC MINH: ĐẠT - giải pháp đã hoàn tất và tất cả các bước đều được chứng minh chính xác.
