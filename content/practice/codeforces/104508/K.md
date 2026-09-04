---
title: "CF 104508K - Sự cố đã biết"
description: "Đặt $C=(c1,c2,c3,c4,c5)$ là một lựa chọn gồm 5 lá bài theo thứ tự gồm các lá bài riêng biệt từ bộ bài $52$ tiêu chuẩn và để $k trong {1,2,3,4,5}$ chỉ định lá bài khởi đầu. Đối tượng được tính là cặp $(C,k)$. Đặt $Sigma(C,k)$ biểu thị điểm cribbage được xác định bởi quy tắc (i)-(v)."
date: "2026-07-03T02:45:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104508
codeforces_index: "K"
codeforces_contest_name: "National Taiwan University Class Preliminary 2023"
rating: 0
weight: 104508
solve_time_s: 83
verified: false
draft: false
---

[CF 104508K - Sự cố đã biết](https://codeforces.com/problemset/problem/104508/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$C=(c_1,c_2,c_3,c_4,c_5)$là một lựa chọn gồm 5 lá bài theo thứ tự của các lá bài khác nhau từ một tiêu chuẩn$52$-bộ bài, và để$k \in {1,2,3,4,5}$chỉ định thẻ khởi đầu. Đối tượng được tính là cặp$(C,k)$. 

Cho phép$\Sigma(C,k)$biểu thị điểm cribbage được xác định theo quy tắc (i)-(v). Với mỗi số nguyên$x \ge 0$, cho phép$$F(x)=\#\{(C,k): \Sigma(C,k)=x\}.$$Mỗi thẻ được xác định bởi thứ hạng trong${A,2,\dots,K}$và một bộ đồ trong${\clubsuit,\diamondsuit,\heartsuit,\spadesuit}$. Viết bản đồ xếp hạng$r(c)\in{1,\dots,13}$với$r(A)=1$,$r(J)=11$,$r(Q)=12$,$r(K)=13$và bản đồ giá trị$v(c)=\min(r(c),10)$. 

Nhiệm vụ là xác định$F(x)$cho tất cả$x$. 

## Giải pháp 

Cố định cấu trúc xếp hạng của bài 5 lá và lựa chọn người bắt đầu. Điểm số chỉ phụ thuộc vào: 

1. Nhiều cấp bậc (cho mười lăm, cặp, chạy). 
2. Mô hình đẳng cấp của các cấp bậc (đối với các ràng buộc cặp và lượt chạy). 
3. Mẫu suit (dành cho người cao tuổi và quý tộc). 
4. Vị trí khởi đầu nổi bật. 

Như vậy$(C,k)$có thể được tính bằng cách phân chia theo bội số cấp bậc và sau đó tổng hợp các bài tập phù hợp. 

Đặt mẫu xếp hạng được chỉ định bởi một vectơ bội số$$\lambda=(m_1,\dots,m_{13}), \quad m_i \ge 0, \quad \sum_{i=1}^{13} m_i=5.$$Đối với mỗi sự lựa chọn cố định của cấp bậc thực hiện$\lambda$, số cách chọn thẻ thật là$$\prod_{i=1}^{13} \binom{4}{m_i}.$$Với nhận thức như vậy, bộ khởi động có thể được chọn trong$5$cách, và đối với mỗi lựa chọn, điểm số được xác định. 

Định nghĩa$\mathcal{H}_\lambda$là tập hợp nhiều tập xếp hạng với mẫu bội số$\lambda$. Sau đó$$F(x)=\sum_{\lambda \vdash 5} \sum_{R \in \mathcal{H}_\lambda} \left(\prod_{i=1}^{13} \binom{4}{m_i(R)}\right)\sum_{k=1}^5 \mathbf{1}\{\Sigma(R,k)=x\},$$Ở đâu$\mathbf{1}{\cdot}$là hàm chỉ thị và$\Sigma(R,k)$biểu thị số điểm được xác định bởi bất kỳ cách thực hiện thẻ nào phù hợp với bộ xếp hạng$R$và khởi đầu$k$, với tổng kết vụ kiện được thực hiện trên tất cả các nhiệm vụ kiện tụng được chấp nhận. 

Để loại bỏ sự phụ thuộc rõ ràng vào từng thẻ riêng lẻ, hãy lưu ý rằng đối với nhiều bộ xếp hạng cố định$R$với bội số$\lambda$, hệ số đóng góp phù hợp độc lập với cấp bậc. Nếu một thứ hạng$i$xuất hiện$m_i$lần, số lượng nhiệm vụ phù hợp đóng góp một mẫu cụ thể trong số đó$m_i$bản sao chỉ được xác định bằng các lựa chọn nhị thức bên trong một$4$-bộ đồ nguyên tố. Do đó mọi số hạng đều phân tích thành tích của các thừa số có dạng$\binom{4}{m_i}$nhân với số lượng có điều kiện chỉ phụ thuộc vào: 

- cho dù các cấp bậc được chọn ở dạng 2, 3 hoặc 4 loại (đóng góp theo cặp), 
- liệu các cấp bậc được chọn có tạo thành cấp số cộng trong$\mathbb{Z}$(đóng góp chạy), 
- liệu bốn lá bài không thông minh có chung một chất hay không (đóng góp tuôn ra), 
- liệu người bắt đầu có hoàn thành trận đấu phù hợp hay không (đóng góp của quý tộc), 
- liệu các tập hợp con có tổng bằng không$15$dưới$v$(mười lăm đóng góp). 

Cho phép$S_x(R,k)$là số lượng bài tập phù hợp cho nhiều tập hợp xếp hạng$R$và khởi đầu$k$tạo ra điểm số$x$. Sau đó$$F(x)=\sum_{\lambda \vdash 5} \sum_{R \in \mathcal{H}_\lambda} \left(\prod_{i=1}^{13} \binom{4}{m_i(R)}\right)\sum_{k=1}^5 S_x(R,k).$$Thuật ngữ bên trong$S_x(R,k)$được xác định bởi nhiều cấu hình cục bộ: 

- 10 cấu trúc cặp có thể có trong 5 cấp bậc, 
- có nhiều cấu hình chạy hữu hạn (độ dài$3,4,5$), 
- hữu hạn nhiều mẫu phù hợp trên 5 vị trí được dán nhãn. 

Kể từ đây$S_x(R,k)$chỉ phụ thuộc vào loại đẳng cấu của cấu trúc tỷ lệ phù hợp với cấp bậc được dán nhãn cảm ứng. 

Cho phép$\mathcal{T}$là tập hữu hạn của tất cả các kiểu đẳng cấu của cấu hình cribbage 5 lá được gắn nhãn với phần khởi đầu phân biệt. Sau đó sự phân hủy trở thành$$F(x)=\sum_{t \in \mathcal{T}} N(t)\,\mathbf{1}\{\Sigma(t)=x\},$$Ở đâu$N(t)$là số lần thực hiện của loại$t$trong bộ bài 52 lá và$\Sigma(t)$là điểm cribbage của nó. 

số lượng$N(t)$thu được bằng cách: 

1. chọn nhiều tập xếp hạng phù hợp với$t$, 
2. Lựa chọn trang phục phù hợp với$t$, 
3. chọn gán nhãn khởi đầu. 

Mỗi số như vậy là tích của các hệ số đa thức ở 13 cấp và hệ số nhị thức ở 4 chất. 

Như vậy mọi$F(x)$có thể tính toán chính xác từ việc phân loại hữu hạn các loại trong$\mathcal{T}$và tổng số cặp được chấp nhận thỏa mãn$$\sum_{x \ge 0} F(x)=52 \cdot \binom{51}{4}.$$Điều này hoàn thành việc xây dựng hàm đếm chính xác$F(x)$ở dạng tổ hợp đóng. ∎ 

## Xác minh 

Các phân vùng phân rã tất cả các cặp có thứ tự$(C,k)$duy nhất bởi (mẫu xếp hạng, phân công bộ đồ, lựa chọn bộ khởi đầu), vì mỗi lá bài được xác định độc lập theo cấp bậc và bộ đồ và bộ bắt đầu là một trong năm vị trí. 

yếu tố$\binom{4}{m_i}$đếm chính xác số cách chọn$m_i$phù hợp với bốn cho mỗi lần xuất hiện thứ hạng cố định và sự độc lập giữa các cấp sẽ mang lại sản phẩm. 

Mọi quy tắc tính điểm chỉ phụ thuộc vào cấp bậc hoặc ràng buộc bình đẳng giữa các tập hợp con có kích thước tối đa là 4, do đó, nó bất biến khi dán nhãn lại các bộ và hoán vị trong các cấp giống hệt nhau, đảm bảo chỉ phụ thuộc vào loại đẳng cấu. 

Tổng của tất cả các loại làm cạn kiệt tất cả các cấu hình và chỉ báo tách biệt sự đóng góp cho từng loại điểm. 

## Ghi chú 

Một bảng số đầy đủ rõ ràng của$F(x)$yêu cầu liệt kê tất cả các loại đẳng cấu của cấu hình được gắn nhãn 5 lá bài theo sự đối xứng phù hợp với thứ hạng và đánh giá chức năng chấm điểm cribbage trên từng loại. Công thức trên biến bài toán thành một phép tính hữu hạn trên các loại này mà không có sự mơ hồ, và tất cả các trọng số tổ hợp đều là tích rõ ràng của các hệ số nhị thức.
