---
title: "CF 103182A - Các vấn đề của doanh nghiệp"
description: "Cơ sở kinh điển $(alpha1,ldots,alphat)$ trong Bài tập 12 được biểu diễn trong phần này dưới dạng lựa chọn $t$ vị trí phân biệt giữa $n$, tương đương với $(s,t)$-kết hợp với $n=s+t$, được mã hóa bằng chuỗi nhị phân $a{n-1}ldots a1a0$ thỏa mãn $sum ai=t$."
date: "2026-07-03T16:23:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103182
codeforces_index: "A"
codeforces_contest_name: "AGM 2021, Final Round, Day 2"
rating: 0
weight: 103182
solve_time_s: 132
verified: false
draft: false
---

[CF 103182A - Các vấn đề của công ty](https://codeforces.com/problemset/problem/103182/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Một cơ sở kinh điển$(\alpha_1,\ldots,\alpha_t)$trong Bài tập 12 được trình bày trong phần này dưới dạng lựa chọn$t$vị trí nổi bật giữa$n$, tương đương như một$(s,t)$-kết hợp với$n=s+t$, được mã hóa bằng chuỗi nhị phân$a_{n-1}\ldots a_1a_0$thỏa mãn$\sum a_i=t$. Điều kiện một bước chỉ thay đổi một bit có nghĩa là các cấu hình liên tiếp khác nhau chỉ bằng một lần chuyển đổi$a_i \leftarrow 1-a_i$. 

Do đó, nhiệm vụ là xây dựng một đường dẫn Gray truy cập mọi chuỗi nhị phân có độ dài$n$với chính xác$t$những cái đó, với hạn chế là các chuỗi liên tiếp khác nhau ở đúng một tọa độ. 

Việc xây dựng sử dụng dạng tổ hợp của các tổ hợp từ (7.2.1.3). Mỗi$(s,t)$-sự kết hợp tương ứng duy nhất với một thành phần$s = q_t + \cdots + q_1 + q_0$với$q_j \ge 0$theo phương trình (11) và chuỗi nhị phân có cấu trúc$a_{n-1}\ldots a_0 = 0^{q_t}1\,0^{q_{t-1}}1\cdots 1\,0^{q_0}.$Sự thay đổi một bit trong chuỗi nhị phân tương ứng với việc truyền một đơn vị giữa hai thành phần liền kề của vectơ thành phần$(q_t,\ldots,q_0)$hoặc sửa đổi tương đương chính xác một khoảng cách giữa các số 1 liên tiếp. 

Đường dẫn Gray được xây dựng bằng phương pháp quy nạp trên$t$. Vì$t=0$có một cấu hình duy nhất, vì vậy xác nhận quyền sở hữu được giữ nguyên. Giả sử đường dẫn Gray tồn tại cho tất cả$(s',t')$với$s'+t'<n$. Để cố định$n=s+t$, phân vùng tất cả$(s,t)$-kết hợp theo vị trí đầu tiên của số 1. Nếu số 1 ngoài cùng bên trái xảy ra ở vị trí$k$, thì phần còn lại$t-1$những cái đó tạo thành một$(s-k,t-1)$-kết hợp về hậu tố của độ dài$n-k-1$. Điều này xác định tập hợp các cấu hình có cố định$k$như một bản dịch của tất cả$(s-k,t-1)$-sự kết hợp. 

Trong mỗi cố định$k$, giả thuyết quy nạp cung cấp đường dẫn Gray trên các cấu hình hậu tố, thay đổi từng bit một. Sự chuyển đổi từ khối$k$chặn$k+1$đạt được bằng cách dịch chuyển 1 vị trí ngoài cùng bên trái sang trái, thay thế mẫu cục bộ$01$qua$10$. Đây là một thay đổi một bit trong chuỗi nhị phân. 

Để đảm bảo đường dẫn Gray toàn cục, việc truyền tải các khối được thực hiện theo hướng xen kẽ: khi$k$tăng lên, đường dẫn Gray cảm ứng trên hậu tố được di chuyển về phía trước; khi$k$giảm đi thì nó đi theo hướng ngược lại. Sự phản ánh này đảm bảo rằng điểm cuối của khối$k$khác với điểm bắt đầu của khối$k+1$thực hiện chính xác cùng một bước di chuyển một bit làm dịch chuyển số 1 ngoài cùng bên trái. 

Do đó, mỗi bước sẽ sửa đổi hậu tố bằng đường dẫn Gray quy nạp hoặc di chuyển số 1 dẫn đầu qua một vị trí liền kề, cả hai đều thay đổi chính xác một bit. Mọi$(s,t)$-combination xuất hiện chính xác một lần vì mỗi khối được sử dụng hết đúng một lần và mọi thành phần được gán duy nhất cho một khối theo vị trí số 1 ngoài cùng bên trái của nó. 

Điều này hoàn thành việc chứng minh. ∎
