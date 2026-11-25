---
title: "CF 965C - Arkady tham lam"
description: "Chúng ta được cấp một dòng tuyến tính gồm $n$ kẹo và một số lượng người cố định $k$. Một tham số $x$ xác định cách phân phối kẹo: kẹo được xử lý theo các khối có kích thước $x$ liên tiếp."
date: "2026-06-17T01:37:58+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 965
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 476 (Div. 2) [Thanks, Telegram!]"
rating: 2000
weight: 965
solve_time_s: 38
verified: false
draft: false
---

[CF 965C - Arkady tham lam](https://codeforces.com/problemset/problem/965/C) 

**Xếp hạng:** 2000 
**Thẻ:** toán 
**Thời gian giải:** 38s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng tuyến tính của$n$kẹo và một số lượng người cố định$k$. Một tham số duy nhất$x$xác định cách phân phối kẹo: kẹo được xử lý theo các khối có kích thước liên tiếp$x$. Mỗi khối được gán cho một người theo thứ tự tuần hoàn, bắt đầu từ Arkady (người 1), sau đó đến người 2, v.v. cho đến khi người đó$k$, sau đó lặp lại từ người 1 lần nữa. 

Số kẹo còn sót lại ít hơn$x$cuối cùng sẽ bị loại bỏ. Vì vậy, chỉ có khối đầy đủ mới quan trọng. 

Tổng lợi ích mà Arkady kiếm được hoàn toàn phụ thuộc vào số lượng khối đầy đủ được gán cho anh ta, vì mỗi khối như vậy mang lại cho anh ta chính xác$x$kẹo. Nếu anh ta nhận được$t$khối, tổng số của anh ấy là$t \cdot x$. 

Sự lựa chọn của$x$bị hạn chế theo hai cách. Đầu tiên,$x \le M$, nên mỗi đợt không quá lớn. Thứ hai, không ai có thể nhận được nhiều hơn$D$tổng số lô. Vì các khối được phân phối theo chu kỳ nên mỗi người sẽ nhận được một trong hai$\lfloor B/k \rfloor$hoặc$\lceil B/k \rceil$khối tùy thuộc vào vị trí, nơi$B = \lfloor n/x \rfloor$. 

Mục tiêu là chọn$x$tôn trọng cả hai ràng buộc trong khi tối đa hóa tổng số kẹo của Arkady. 

Những hạn chế là vô cùng lớn:$n$có thể đạt được$10^{18}$, vì vậy bất kỳ giải pháp nào lặp lại kẹo hoặc mô phỏng phân phối đều không thể thực hiện được. Thách thức tính toán chính là lý luận về các ước số và số khối một cách hiệu quả, theo độ phức tạp kiểu logarit hoặc căn bậc hai. 

Một trường hợp cạnh tinh tế phát sinh khi các giá trị nhỏ của$x$tạo ra quá nhiều khối, khiến một số người vượt quá$D$. Ví dụ, nếu$n$lớn và$x = 1$, sau đó$B = n$, và mỗi người nhận được khoảng$n/k$khối, có thể dễ dàng vượt quá$D$. Mặc dù$x$nhỏ và có vẻ có lợi nhưng lại không hợp lệ. 

Một trường hợp cạnh khác là khi$x$lớn, gần$M$hoặc$n$. Sau đó$B$nhỏ, có thể$0$hoặc$1$và Arkady có thể nhận được rất ít khối mặc dù các ràng buộc đã được thỏa mãn. Giải pháp tối ưu thường nằm ở khoảng giữa. 

Cuối cùng, trường hợp biên xảy ra khi$n < k\cdot x$, dẫn đến$B < k$, nơi chỉ một số người đầu tiên nhận được khối và Arkady có thể được hưởng lợi một cách không tương xứng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi cách có thể$x$từ$1$ĐẾN$M$. Đối với mỗi$x$, tính toán$B = \lfloor n/x \rfloor$, mô phỏng số lượng khối mà mỗi người nhận được, kiểm tra xem có người nào vượt quá không$D$, và nếu hợp lệ hãy tính mức tăng của Arkady. Điều này đúng vì nó phản ánh trực tiếp quá trình được mô tả trong bài toán. 

Tuy nhiên, cách tiếp cận này còn quá chậm.$M$có thể lớn như$10^{18}$, và thậm chí cả tính toán$B$cho mỗi$x$dẫn đến điều không thể thực hiện được$O(Mk)$hoặc$O(M)$quá trình. Nút thắt cổ chai không phải là số học mà là sự lặp lại trên một miền rộng lớn. 

Quan sát quan trọng là số lượng khối đầy đủ$B = \lfloor n/x \rfloor$chỉ thay đổi khi$x$vượt qua các ước của$n$. Quan trọng hơn, đối với cố định$B$, mọi giá trị của$x$trong một khoảng thời gian tạo ra cùng một$B$, và do đó có cùng điều kiện khả thi và điểm Arkady. Điều này biến vấn đề thành lý luận trên phạm vi của$x$được nhóm theo thương số không đổi$n/x$. 

Sau đó chúng ta đảo ngược quan điểm: thay vì chọn$x$, chúng tôi chọn$B$, số khối đầy đủ. Từ$B = \lfloor n/x \rfloor$, có hiệu lực$x$nằm trong khoảng$(n/(B+1), n/B]$. Đối với mỗi như vậy$B$, chúng ta có thể kiểm tra xem liệu có tồn tại một$x \le M$trong khoảng này cũng thỏa mãn ràng buộc của mỗi người. Một lần$B$được cố định, lợi ích của Arkady trở nên tối đa$x \cdot t$, Ở đâu$t$là số lần anh ấy xuất hiện trong chu kỳ trong số$B$khối. 

Điều này làm giảm vấn đề chỉ còn việc kiểm tra$O(\sqrt{n})$phạm vi ứng cử viên của$B$, bởi vì cấu trúc thương của$n/x$chỉ có bấy nhiêu giá trị riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$x$|$O(Mk)$|$O(1)$| Quá chậm | 
| Nhóm thương số trên$B = \lfloor n/x \rfloor$|$O(\sqrt{n})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát điều đó để cố định$x$, tổng số lô đầy đủ là$B = \lfloor n/x \rfloor$. Điều này xác định tất cả các phân phối. 
2. Với mỗi khả năng$B$, suy ra phạm vi của$x$cái đó tạo ra nó. Khoảng đó là$x \in [\lfloor n/(B+1) \rfloor + 1, \lfloor n/B \rfloor]$. Điều này chuyển đổi vấn đề từ việc lặp đi lặp lại$x$để lặp đi lặp lại$B$. 
3. Lặp lại tất cả các giá trị riêng biệt của$B$by jumping using the standard quotient trick. Bắt đầu từ$x = 1$, tính toán$B = n // x$, và nhảy$x$to the end of this segment.
 4. Đối với mỗi phân đoạn, hãy tính xem có tồn tại ít nhất một$x \le M$trong phân khúc. Nếu không, hãy bỏ qua nó hoàn toàn vì nó không khả thi. 
5. Đối với phân đoạn hợp lệ, hãy xác định số lượng khối mà Arkady nhận được. TRONG$B$tổng số khối được phân phối theo chu kỳ giữa$k$mọi người, Arkady cũng vậy$\lfloor B/k \rfloor$hoặc$\lceil B/k \rceil$khối tùy thuộc vào phần còn lại. Vì Arkady là người đầu tiên nên anh ấy nhận được$\lceil B/k \rceil$. 
6. Để tối đa hóa tổng số kẹo của Arkady, chúng tôi muốn số lượng lớn nhất có thể$x$trong phân khúc, vì mức tăng tỷ lệ thuận với$x$. Vì vậy hãy chọn
