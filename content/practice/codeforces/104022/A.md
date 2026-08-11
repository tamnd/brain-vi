---
title: "CF 104022A - Cầu thủ xuất sắc nhất"
description: "Giả sử bàn cờ là lưới $8 nhân 8$ tiêu chuẩn, được phân tách thành các ô vuông đơn vị $64$. Lớp phủ domino là một lớp lát hoàn hảo bằng các hình chữ nhật $1 nhân 2$ hoặc $2 nhân 1$ được căn chỉnh theo lưới. Mỗi domino chiếm đúng hai ô vuông liền kề."
date: "2026-07-02T04:30:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104022
codeforces_index: "A"
codeforces_contest_name: "The 2020 ICPC Asia Yinchuan Regional Programming Contest"
rating: 0
weight: 104022
solve_time_s: 124
verified: false
draft: false
---

[CF 104022A - Cầu thủ xuất sắc nhất](https://codeforces.com/problemset/problem/104022/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Hãy để bàn cờ là tiêu chuẩn$8 \times 8$lưới, phân hủy thành$64$đơn vị hình vuông. Lớp phủ domino là một lớp lát hoàn hảo bởi$1 \times 2$hoặc$2 \times 1$hình chữ nhật thẳng hàng với lưới. Mỗi domino chiếm đúng hai ô vuông liền kề. 

Một đường thẳng được gọi là đi qua phần trong của bàn cờ nếu nó cắt hình vuông mở$(0,8)\times(0,8)$, và lớp phủ không có lỗi khi mỗi đường như vậy cắt phần bên trong của ít nhất một quân domino. Nói cách khác, không có đường thẳng nào có thể đi qua bàn cờ mà tránh được khu vực bên trong của mọi quân domino. 

Điều kiện này mang tính toàn cầu: nó cấm sự tồn tại của bất kỳ “hành lang thông thoáng” nào xuyên qua lớp lát gạch. 

Nhiệm vụ là xác định có bao nhiêu quân domino$8 \times 8$board thỏa mãn tính chất này. 

##Giải pháp 

Một lát domino của bàn cờ có thể được hiểu là sự kết hợp hoàn hảo của biểu đồ lưới. Mỗi domino là ngang hoặc dọc. Hạn chế chính về cấu trúc do tính không có lỗi gây ra là không có đường thẳng nào có thể đi qua bàn cờ mà không cắt xuyên qua phần bên trong của ít nhất một quân domino. 

Chúng ta bắt đầu bằng cách chỉ ra rằng hai ô cụ thể không có lỗi. Nếu tất cả các quân domino đều nằm ngang thì mọi đường thẳng đứng giao với phần bên trong của bàn cờ đều đi qua phần bên trong của một số quân domino ngang. Bất kỳ đường ngang hoặc đường chéo nào nhất thiết phải giao với một số nội thất hình vuông, do đó cũng giao với nội thất domino ngang. Lập luận tương tự được áp dụng một cách đối xứng khi tất cả các quân domino đều thẳng đứng. Do đó, cả ốp lát theo chiều ngang và theo chiều dọc đều không có lỗi. 

Bây giờ chúng ta chứng minh rằng không có cách xếp nào khác là không có lỗi. Giả sử một ô xếp có ít nhất một quân domino ngang và ít nhất một quân domino dọc. Hãy xem xét tập hợp các cạnh lưới ngăn cách các ô vuông được bao phủ bởi các quân domino có hướng khác nhau. Vì cả hai hướng đều xuất hiện nên phải tồn tại một cấu hình cục bộ trong đó quân domino ngang liền kề với quân domino dọc dọc theo ít nhất một đỉnh chung. Xung quanh một đỉnh như vậy, việc lát gạch tạo ra một sự thay đổi về hướng, tạo ra một “bước ngoặt” trong việc phân hủy tấm ván thành$2 \times 1$miếng. 

Từ cấu hình hỗn hợp như vậy, chúng tôi xây dựng một đường thẳng tránh tất cả nội thất domino. Bắt đầu từ một điểm lệch một chút so với tâm của hình vuông tới sự thay đổi hướng ngang-dọc. Mở rộng đường dốc$1$(hoặc độ dốc$-1$nếu cần tùy thuộc vào hướng) để nó đi qua các ô vuông đơn vị liên tiếp dọc theo các đường chéo của lưới. Bởi vì ô xếp chứa cả hai hướng, người ta luôn có thể chọn độ lệch sao cho đường chỉ đi qua ranh giới chung giữa các hình vuông hoặc đi qua các góc của quân domino, không bao giờ đi vào bên trong bất kỳ ô nào.$1 \times 2$hình chữ nhật. Quan sát hình học quan trọng là một đường dốc$1$đi ngang qua bảng bằng cách di chuyển một bước sang phải và một bước lên trên, và tại mỗi bước, nó có thể được căn chỉnh để duy trì cấu trúc ranh giới do ốp lát hỗn hợp tạo ra. 

Vì việc lát gạch là hữu hạn nên đoạn đường này có thể được kéo dài ra toàn bộ phần bên trong của bàn cờ trong khi tránh tất cả các phần bên trong domino. Điều này tạo ra một đường lỗi, mâu thuẫn với tính không có lỗi. Do đó, bất kỳ lát gạch nào chứa cả hai hướng đều không có lỗi. 

Do đó, mọi lát gạch không có lỗi phải có hướng đơn sắc, theo chiều ngang hoặc theo chiều dọc. Hai cấu hình này khác biệt và đều hợp lệ. 

Điều này làm cạn kiệt mọi khả năng. 

Điều này hoàn thành việc chứng minh. ∎
