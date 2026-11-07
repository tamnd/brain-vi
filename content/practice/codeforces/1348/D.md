---
title: "CF 1348D - Phượng Hoàng và Khoa Học"
description: "Cho $u(x)$ là một đa thức có hệ số nguyên tự do bình phương trên $mathbb{Z}$. Điều này có nghĩa là $u(x)$ không có nghiệm lặp lại trong $mathbb{C}$, tương đương, $gcd(u(x), u'(x)) = 1$ trong $mathbb{Z[x]$, trong đó $u'(x)$ là đạo hàm của $u(x)$."
date: "2026-06-11T14:48:27+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1348
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 638 (Div. 2)"
rating: 1900
weight: 1348
solve_time_s: 173
verified: false
draft: false
---

[CF 1348D - Phượng hoàng và Khoa học](https://codeforces.com/problemset/problem/1348/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Giải bài tập 4.6.2.23 

hãy để$u(x)$là một đa thức có hệ số nguyên tự do bình phương trên$\mathbb{Z}$. Điều này có nghĩa là$u(x)$không có nguồn gốc lặp đi lặp lại trong$\mathbb{C}$, tương đương,$\gcd(u(x), u'(x)) = 1$TRONG$\mathbb{Z}[x]$, Ở đâu$u'(x)$là đạo hàm của$u(x)$. 

Ta muốn chứng minh rằng chỉ có hữu hạn số nguyên tố$p$như vậy$u(x)$không phải là modulo vuông góc$p$, tức là, sao cho việc giảm$\overline{u}(x) \in \mathbb{F}_p[x]$có một gốc lặp đi lặp lại. 

### Bước 1: Đặc trưng của modulo tự do bình phương$p$Một đa thức$v(x)$trên một cánh đồng$F$là tự do khi và chỉ khi$\gcd(v(x), v'(x)) = 1$TRONG$F[x]$. Vì thế,$\overline{u}(x)$**không phải là modulo vuông góc$p$** nếu và chỉ nếu$$\gcd\big(\overline{u}(x), \overline{u}'(x)\big) \neq 1 \quad \text{in } \mathbb{F}_p[x].$$Tương tự, tồn tại một đa thức không hằng$d(x) \in \mathbb{F}_p[x]$chia cả hai$\overline{u}(x)$Và$\overline{u}'(x)$. 

### Bước 2: Sử dụng kết quả 

hãy để$R = \operatorname{Res}(u, u')$biểu thị **kết quả** của$u(x)$Và$u'(x)$. Kết quả thỏa mãn:$$R = 0 \quad \text{if and only if} \quad \gcd(u, u') \neq 1 \text{ in } \mathbb{C}[x].$$Từ$u(x)$là vuông vắn hơn$\mathbb{Z}$, chúng tôi có$\gcd(u, u') = 1$qua$\mathbb{Z}[x]$, Vì thế$$R = \operatorname{Res}(u, u') \neq 0.$$Kết quả là một số nguyên. Hơn nữa, với bất kỳ số nguyên tố nào$p$, sự giảm của$u$modulo$p$thỏa mãn$$\overline{u}(x) \text{ is not squarefree over } \mathbb{F}_p \quad \iff \quad p \text{ divides } R.$$Điều này xảy ra sau bởi vì$\gcd(\overline{u}, \overline{u}') \neq 1$qua$\mathbb{F}_p[x]$khi và chỉ khi kết quả biến mất theo modulo$p$. 

### Bước 3: Chỉ hữu hạn nhiều số nguyên tố chia cho một số nguyên khác 0 

Kể từ khi$R \in \mathbb{Z}$khác 0 nên nó chỉ có hữu hạn số nguyên tố. Cho phép$p_1, p_2, \dots, p_k$biểu thị các số nguyên tố này. Sau đó với bất kỳ số nguyên tố nào$p \notin \{p_1, \dots, p_k\}$, đa thức$\overline{u}(x)$là modulo vuông góc$p$. 

### Bước 4: Kết luận 

Do đó, tập số nguyên tố$p$như vậy$u(x)$**không phải là modulo vuông góc$p$** nằm trong tập hữu hạn các ước số nguyên tố của$\operatorname{Res}(u, u')$. Do đó chỉ có hữu hạn số số nguyên tố như vậy.$$\boxed{\text{Only finitely many primes } p \text{ make } u(x) \text{ not squarefree modulo } p.}$$### Bước 5: Ghi chú tùy chọn 

Lập luận này không yêu cầu tính toán rõ ràng về nghiệm của$u(x)$hoặc$u'(x)$. Nó hoàn toàn dựa vào các tính chất đại số của các kết quả và thực tế là các số nguyên khác 0 có hữu hạn nhiều ước số nguyên tố. 

**Kết thúc giải pháp.**
