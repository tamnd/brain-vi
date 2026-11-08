---
title: "CF 1386C - Joker"
description: "Giả sử $N = pq$ trong đó $p tương đương 3 pmod 8$ và $q tương đương 7 pmod 8$. Đầu tiên chúng tôi chứng minh sự đồng nhất được khẳng định liên quan đến ký hiệu Jacobi."
date: "2026-06-11T10:39:50+07:00"
tags: ["codeforces", "competitive-programming", "*special", "bitmasks", "data-structures", "divide-and-conquer", "dsu"]
categories: ["algorithms"]
codeforces_contest: 1386
codeforces_index: "C"
codeforces_contest_name: "Baltic Olympiad in Informatics 2020, Day 1 (IOI, Unofficial Mirror Contest, Unrated)"
rating: 2800
weight: 1386
solve_time_s: 144
verified: false
draft: false
---

[CF 1386C - Joker](https://codeforces.com/problemset/problem/1386/C) 

**Đánh giá:** 2800 
**Thẻ:** *đặc biệt, mặt nạ bit, cấu trúc dữ liệu, phân chia và chinh phục, dsu 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$N = pq$Ở đâu$p \equiv 3 \pmod 8$Và$q \equiv 7 \pmod 8$. Đầu tiên chúng tôi chứng minh sự đồng nhất được khẳng định liên quan đến ký hiệu Jacobi. Nhớ lại rằng ký hiệu Jacobi có tính nhân và thỏa mãn$$\left(\frac{a}{N}\right) = \left(\frac{a}{p}\right) \left(\frac{a}{q}\right)$$với mọi số nguyên$a$. Chúng tôi tính toán$\left(\frac{-1}{N}\right)$Và$\left(\frac{2}{N}\right)$riêng. 

Theo định luật tương hỗ bậc hai và các công thức chuẩn cho$-1$Và$2$với tư cách là tử số, chúng ta có$$\left(\frac{-1}{p}\right) = (-1)^{(p-1)/2} = (-1)^{(3-1)/2} = (-1)^1 = -1,$$

$$\left(\frac{-1}{q}\right) = (-1)^{(q-1)/2} = (-1)^{(7-1)/2} = (-1)^3 = -1.$$Kể từ đây$$\left(\frac{-1}{N}\right) = \left(\frac{-1}{p}\right)\left(\frac{-1}{q}\right) = (-1)(-1) = 1.$$Tiếp theo, đối với ký hiệu có tử số$2$, nhớ lại rằng$$\left(\frac{2}{p}\right) = (-1)^{(p^2-1)/8}, \qquad \left(\frac{2}{q}\right) = (-1)^{(q^2-1)/8}.$$Tính toán$$(p^2 - 1)/8 = (3^2 - 1)/8 = (9 - 1)/8 = 1,$$

$$(q^2 - 1)/8 = (7^2 - 1)/8 = (49 - 1)/8 = 48/8 = 6.$$Vì thế$$\left(\frac{2}{p}\right) = (-1)^1 = -1, \qquad \left(\frac{2}{q}\right) = (-1)^6 = 1.$$Nhân cho$$\left(\frac{2}{N}\right) = \left(\frac{2}{p}\right)\left(\frac{2}{q}\right) = (-1)(1) = -1.$$Chúng tôi đã thiết lập điều đó$$\left(\frac{-1}{N}\right) = 1 = -(-1) = -\left(\frac{2}{N}\right),$$điều đó chứng tỏ khẳng định đầu tiên. 

Thuộc tính này cho phép thiết kế sơ đồ mã hóa kiểu Rabin mà không có sự mơ hồ. Để lại một tin nhắn văn bản gốc$m$thỏa mãn$0 \le m < N$. Xác định bản mã$$c \equiv m^2 \pmod N.$$Giải mã yêu cầu giải quyết$x^2 \equiv c \pmod N$. Có bốn giải pháp modulo$N$bởi vì$N$là tích của hai số nguyên tố lẻ phân biệt. Chúng ta có thể phân biệt chúng bằng cách xem xét ký hiệu Jacobi modulo$N$. Tính toán$$\left(\frac{x}{N}\right) \quad \text{and} \quad \left(\frac{x - 1}{2}\right)$$hoặc mã hóa tương đương một bit dư thừa thành$m$sao cho có đúng một trong bốn căn bậc hai thỏa mãn điều kiện ký hiệu Jacobi định trước. Danh tính$\left(\frac{-1}{N}\right) = -\left(\frac{2}{N}\right)$đảm bảo rằng ánh xạ từ$m$đến gốc là không rõ ràng: bốn gốc có thể được phân chia thành các cặp có ký hiệu Jacobi khác nhau, do đó việc chọn gốc có ký hiệu thích hợp sẽ khôi phục được thông báo ban đầu. 

Một cách rõ ràng, hãy để$m$được mã hóa sao cho$$\left(\frac{m}{N}\right) = 1, \quad m \text{ even or odd according to a chosen convention}.$$Sau khi tính toán$x \equiv \sqrt{c} \pmod N$, chọn giải pháp duy nhất có ký hiệu Jacobi$1$và tính chẵn lẻ được xác định trước. Điều này mang lại một hàm giải mã xác định$$m \equiv \text{Dec}(c) \pmod N$$không có sự mơ hồ. Mã hóa vẫn còn$$c \equiv m^2 \pmod N$$và việc giải mã chỉ yêu cầu lựa chọn gốc thỏa mãn điều kiện ký hiệu Jacobi cố định. Danh tính đảm bảo rằng chính xác một trong bốn nghiệm đáp ứng tiêu chí, do đó loại bỏ sự mơ hồ gấp đôi hiện diện trong hộp Rabin SQRT ban đầu. 

Điều này hoàn thành việc chứng minh. 

∎
