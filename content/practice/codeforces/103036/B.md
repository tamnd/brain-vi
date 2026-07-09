---
title: "CF 103036B - Bản ghi Vinyl"
description: "Giả sử một cơ sở chính tắc $(alpha1,ldots,alphat)$ được biểu diễn dưới dạng một bộ $t$-có thứ tự gồm các phần tử riêng biệt của ${1,ldots,n}$. Điều này tương đương với một hoán vị của $t$ ký hiệu riêng biệt được chọn từ $n$, với thứ tự được giữ nguyên."
date: "2026-07-04T02:06:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103036
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 04-02-21 Div. 2 (Beginner)"
rating: 0
weight: 103036
solve_time_s: 88
verified: false
draft: false
---

[CF 103036B - Bản ghi vinyl](https://codeforces.com/problemset/problem/103036/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Hãy để một cơ sở kinh điển$(\alpha_1,\ldots,\alpha_t)$được thể hiện dưới dạng mệnh lệnh$t$-bộ các phần tử riêng biệt của${1,\ldots,n}$. Điều này tương đương với một hoán vị của$t$các ký hiệu riêng biệt được chọn từ$n$, với thứ tự được bảo toàn. Hai cơ sở liền kề nhau trong đường dẫn Gray mong muốn khi chúng khác nhau ở đúng một tọa độ, nghĩa là tồn tại chính xác một chỉ mục$j$như vậy$\alpha_j \neq \alpha'_j$, trong khi$\alpha_i = \alpha'_i$cho tất cả$i \neq j$. 

Sự kề cận này xác định đồ thị Johnson$J(n,t)$theo yêu cầu$t$-các bộ dữ liệu không lặp lại, trong đó các cạnh tương ứng với việc thay đổi một tọa độ trong khi vẫn giữ được tính khác biệt. 

Nhiệm vụ là xây dựng đường đi Hamilton trong đồ thị này. 

Ta xây dựng đường đi bằng quy nạp trên$n$, bằng cách sử dụng phép duyệt có kiểm soát các phần mở rộng của các cơ sở kinh điển từ$J(n-1,t)$ĐẾN$J(n,t)$. 

Đầu tiên giả sử rằng đường đi Hamilton tồn tại đối với$J(n-1,t)$liệt kê tất cả các căn cứ kinh điển trên${1,\ldots,n-1}$. Biểu thị đường dẫn này bằng$$(\alpha_1^{(1)},\ldots,\alpha_t^{(1)}), (\alpha_1^{(2)},\ldots,\alpha_t^{(2)}), \ldots$$trong đó các bộ dữ liệu liên tiếp khác nhau ở đúng một tọa độ. 

Chúng tôi mở rộng điều này đến$J(n,t)$bằng cách chèn biểu tượng$n$lần lượt vào từng vị trí tọa độ, theo thứ tự thuận nghịch để bảo toàn tính kề cận. 

Đối với một vị trí cố định$j$, xác định thao tác thay thế$\alpha_j$qua$n$. Từ$n$không xuất hiện trong bất kỳ bộ dữ liệu nào${1,\ldots,n-1}$, thao tác này bảo toàn tính khác biệt và thay đổi chính xác một tọa độ. Như vậy mỗi cơ sở trong$J(n-1,t)$tạo ra$t$căn cứ mới ở$J(n,t)$, một cho mỗi vị trí tọa độ. 

Chúng tôi nối các khối này theo cấu trúc sau. Vì$j = 1$ĐẾN$t$, chúng ta đi theo con đường Hamilton của$J(n-1,t)$, sửa đổi từng bộ dữ liệu bằng cách thay thế nó$j$-th phối hợp bởi$n$và duyệt khối kết quả theo thứ tự tiến hoặc lùi tùy thuộc vào tính chẵn lẻ của$j$. Hướng luân phiên này đảm bảo rằng bộ dữ liệu cuối cùng của$j$-khối thứ và bộ dữ liệu đầu tiên của$(j+1)$khối -st khác nhau ở đúng một tọa độ: tọa độ ở đó$n$được chuyển khỏi vị trí$j$để định vị$j+1$. 

Để xác minh sự liền kề giữa các khối, hãy xem xét phần tử cuối cùng của khối$j$. Nó có$n$ở vị trí$j$và một số giá trị$x \in {1,\ldots,n-1}$ở vị trí$j+1$. Phần tử đầu tiên của khối$j+1$có$n$ở vị trí$j+1$và cùng một giá trị$x$ở vị trí$j$. Tất cả các tọa độ khác đều giống hệt nhau, do đó chỉ có một tọa độ thay đổi: vị trí$j$hoặc$j+1$tùy theo hướng. Điều này mang lại một nước đi Xám hợp lệ. 

Trong mỗi khối, tính kề cận tuân theo giả thuyết quy nạp áp dụng cho$J(n-1,t)$, vì chỉ có một tọa độ thay đổi ở mỗi bước ở đó. 

Như vậy tất cả$t\binom{n}{t}$các căn cứ chính tắc được truy cập đúng một lần và các cơ sở liên tiếp khác nhau ở đúng một tọa độ. Việc xây dựng tạo ra một đường đi Hamilton trong$J(n,t)$. 

Cuối cùng, vì điểm cuối của khối đầu tiên và khối cuối cùng chỉ khác nhau bởi sự di chuyển theo chu kỳ của$n$thông qua tất cả các vị trí, cùng một đối số hướng xen kẽ xác định phần tử cuối cùng với phần tử đầu tiên di chuyển đến tọa độ đơn, đóng đường dẫn thành một chu trình nếu muốn. Tuyên bố của bài tập chỉ yêu cầu một đường đi nên không áp đặt điều kiện đóng. 

Điều này hoàn thành việc chứng minh. ∎
