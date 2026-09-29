---
title: "CF 104842G - Trò Chơi Với Đá"
description: "Chúng ta được cấp một dòng gồm các hộp $n$ và một số cấu hình ban đầu của các viên đá giống hệt $m$ được phân bổ trên chúng. Mỗi cấu hình chỉ đơn giản là một mảng gồm $n$ số nguyên không âm có tổng cố định là $m$."
date: "2026-06-28T11:32:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104842
codeforces_index: "G"
codeforces_contest_name: "2020-2021 ICPC, Moscow Subregional"
rating: 0
weight: 104842
solve_time_s: 26
verified: false
draft: false
---

[CF 104842G - Trò chơi với đá](https://codeforces.com/problemset/problem/104842/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$n$hộp và một số cấu hình ban đầu của$m$những viên đá giống hệt nhau được phân bố trên chúng. Mỗi cấu hình chỉ đơn giản là một mảng$n$số nguyên không âm có tổng cố định bằng$m$. 

Một nước đi duy nhất cho phép chúng ta lấy một viên đá từ hộp và chuyển nó sang hộp liền kề. Chi phí giữa hai cấu hình là số lần di chuyển đơn vị tối thiểu cần thiết để chuyển đổi một phân phối này sang một phân phối khác. Điều này hoàn toàn giống với việc di chuyển khối lượng dọc theo biểu đồ đường dẫn trong đó việc di chuyển một đơn vị qua một cạnh sẽ tốn một đơn vị. 

Chúng tôi được trao$k$những cấu hình như vậy. Nhiệm vụ là chọn một cấu hình mới$b$(cũng là một phân phối hợp lệ của$m$đá) giúp giảm thiểu tổng chi phí vận chuyển từ mọi cấu hình nhất định đến$b$. 

Vì vậy, chúng tôi đang tìm kiếm "phân phối trung bình" trong Khoảng cách máy di chuyển trái đất trên một đường. 

Những ràng buộc ngụ ý$n, k \le 1000$Và$m \le 10^9$. Mặc dù mỗi mảng có tổng bằng$m$,$m$quá lớn để mô phỏng từng viên đá riêng lẻ. Bất kỳ giải pháp nào mở rộng phân phối thành mã thông báo đơn vị đều không thể thực hiện được ngay lập tức. Chúng ta phải làm việc với các biểu diễn luồng tiền tố hoặc số lượng tích lũy trong$O(nk)$hoặc tốt hơn. 

Trường hợp cạnh cấu trúc quan trọng xuất hiện khi phân phối có tính tập trung cao độ. Ví dụ: một cách phân phối có thể đặt tất cả các viên đá vào hộp 1, một cách phân phối khác vào hộp$n$. Một cách tiếp cận tính trung bình ngây thơ cố gắng lấy các trung vị theo tọa độ không thành công vì chi phí không thể tách rời trên mỗi tọa độ; khối lượng chuyển động tương tác giữa các vị trí. 

Một trường hợp tế nhị khác là khi tồn tại nhiều câu trả lời tối ưu. Bài toán cho phép bất kỳ điều gì, vì vậy chúng ta phải tránh dựa vào tính duy nhất. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là liệt kê tất cả các phân phối có thể$b$. Số lượng thành phần yếu của$m$vào trong$n$các bộ phận là$\binom{m+n-1}{n-1}$, lớn về mặt thiên văn đối với$m = 10^9$. Ngay cả việc xây dựng ứng cử viên cũng là điều không thể, vì vậy chúng ta cần phải cải cách cơ cấu. 

Quan sát quan trọng là trên một đường dây, chi phí$W(a, b)$có thể được hiểu là tổng trên các cạnh của chênh lệch tuyệt đối của tổng tiền tố. Nếu chúng ta định nghĩa$$A_i(x) = \sum_{t \le x} a_i[t],
\quad
B(x) = \sum_{t \le x} b[t],$$thì chi phí vận chuyển trở thành$$W(a_i, b) = \sum_{x=1}^{n-1} |A_i(x) - B(x)|.$$Điều này chuyển vấn đề thành việc chọn một hàm$B(x)$với những hạn chế$B(x)$không giảm,$B(n) = m$, giảm thiểu tổng độ lệch tuyệt đối tại mỗi vị trí. 

Bây giờ vấn đề được giải quyết theo vị trí$x$, ngoại trừ ràng buộc đơn điệu của$B(x)$. Tại mỗi lần cắt giữa các hộp, chúng ta đang chọn một giá trị một cách hiệu quả$B(x)$giúp giảm thiểu tổng khoảng cách tới$A_1(x), \dots, A_k(x)$. Đối với một cố định$x$, đây là một thực tế kinh điển: giá trị tối thiểu của tổng độ lệch tuyệt đối là bất kỳ trung vị nào của bội số. 

Vì vậy, tại địa phương, lựa chọn tốt nhất là tổng tiền tố trung bình tại mỗi vị trí. Vấn đề còn lại là đảm bảo tính nhất quán giữa các vị trí, vì$B(x)$phải không giảm. 

Điều này dẫn đến một chuỗi các trung vị có thể vi phạm tính đơn điệu. Việc hiệu chỉnh là chiếu chuỗi này vào không gian của các chuỗi không giảm, chính xác là hồi quy đẳng trương dưới$L_1$sự mất mát. Trong cài đặt này, giải pháp tối ưu có thể đạt được bằng cách xử lý các vị trí từ trái sang phải và duy trì cấu trúc hợp nhất các phân đoạn có đường trung bình vi phạm trật tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Liệt kê các bản phân phối | hàm mũ trong$m$| lớn | Quá chậm | 
| Tiền tố trung vị + hợp nhất đẳng trương |$O(kn)$|$O(kn)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng phân phối thành mảng tổng tiền tố của nó. 

1. Tính tổng tiền tố$A_i(x)$cho mỗi lần phân phối đầu vào. Điều này chuyển đổi từng cấu hình thành khối lượng tích lũy cho từng vị trí. 
2. Đối với từng vị trí$x$, thu thập các giá trị$A_1(x), A_2(x), \dots, A_k(x)$. Sắp xếp chúng hoặc duy trì cấu trúc cho phép trích xuất trung vị. Giá trị trung bình cho giá trị tối ưu cục bộ_
