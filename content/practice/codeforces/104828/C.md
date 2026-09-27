---
title: "CF 104828C - \u6570\u4e09\u5143\u56fe"
description: "Chúng ta có một đồ thị có hướng kiểu giải đấu: mỗi cặp đỉnh phân biệt có đúng một cạnh có hướng giữa chúng. Đối với hai nút bất kỳ $u$ và $v$, $u đến v$ hoặc $v đến u$, không bao giờ cả hai và không bao giờ không có nút nào."
date: "2026-06-28T12:26:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104828
codeforces_index: "C"
codeforces_contest_name: "The 11-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 104828
solve_time_s: 25
verified: false
draft: false
---

[CF 104828C - \u6570\u4e09\u5143\u56fe](https://codeforces.com/problemset/problem/104828/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng kiểu giải đấu: mỗi cặp đỉnh phân biệt có đúng một cạnh có hướng giữa chúng. Đối với hai nút bất kỳ$u$Và$v$, hoặc$u \to v$hoặc$v \to u$, không bao giờ cả hai và không bao giờ không có gì. Cấu trúc này tương đương với việc chọn hướng đặt hàng cho mỗi cặp. 

Trong số tất cả các định hướng như vậy về$n$các đỉnh được gắn nhãn, chúng ta muốn đếm có bao nhiêu chứa ít nhất một tam giác có hướng có chiều dài bằng 3, nghĩa là tồn tại các đỉnh phân biệt$a, b, c$như vậy$a \to b$,$b \to c$, Và$c \to a$. Câu trả lời phải được tính modulo$p$, và có nhiều trường hợp thử nghiệm. 

Các ràng buộc đầu vào là cực kỳ lớn:$T$lên đến$10^4$, và tổng của tất cả$n$giá trị lên đến$5 \cdot 10^6$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào phụ thuộc vào việc lặp lại các cặp hoặc bộ ba cho mỗi trường hợp thử nghiệm. Thậm chí$O(n^2)$mỗi trường hợp thử nghiệm là không thể, vì tổng công việc trong trường hợp xấu nhất sẽ vào khoảng$10^{12}$. 

Một quan sát cấu trúc quan trọng là mô hình đồ thị là một hướng hoàn chỉnh, do đó tổng số đồ thị có thể có chính xác là$2^{\binom{n}{2}}$. Khó khăn là việc thực thi điều kiện “chứa ít nhất một tam giác có hướng”. 

Một cái bẫy ngây thơ là cố gắng liệt kê trực tiếp tất cả các bộ ba$(a,b,c)$và kiểm tra xem chúng có tạo thành một chu trình hay không. Ngay cả việc đếm các hình tam giác theo một hướng cố định cũng đã tốn kém và ở đây chúng ta đang tính các hướng chứ không phải các hình tam giác bên trong một biểu đồ. 

Một vấn đề tế nhị khác là điều kiện “ít nhất một tam giác”. Việc tính phần bù thường dễ dàng hơn: các giải đấu không có tam giác. Việc quên sự đảo ngược này sẽ dẫn đến việc đếm quá mức hoặc loại trừ bao gồm phức tạp mà không mở rộng được. 

Ví dụ về các trường hợp cạnh: 

cho$n = 1$hoặc$n = 2$, không có bộ ba nào tồn tại, vì vậy câu trả lời phải là 0 vì không có tam giác có hướng nào có thể tồn tại cả. 

Vì$n = 3$, có$2^3 = 8$giải đấu. Chính xác 2 trong số đó là tam giác tuần hoàn và 6 tam giác còn lại là bắc cầu (không theo chu kỳ). Vì vậy, câu trả lời phải là 2. 

Một cách tiếp cận ngây thơ cố gắng phát hiện các hình tam giác trên mỗi cấu hình sẽ không khả thi ngay cả đối với các cấu hình nhỏ.$n$, vì không gian trạng thái tăng theo cấp số nhân. 

## Phương pháp tiếp cận 

Ý tưởng cốt lõi là tránh suy luận trực tiếp về các hình tam giác và thay vào đó, phân loại tất cả các giải đấu thành hai loại riêng biệt: những giải đấu chứa chu kỳ có hướng có độ dài ba và những giải đấu không chứa. Lớp sau có cấu trúc hơn nhiều. 

Một công thức bạo lực sẽ là: 

Tạo ra mọi định hướng của tất cả$\binom{n}{2}$cạnh và kiểm tra xem nó có chứa tam giác có hướng hay không. Đây là c
