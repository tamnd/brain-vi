---
title: "CF 104651J - Tìm khoảng trống"
description: "Chúng ta được cho một tập hợp các điểm trong không gian ba chiều. Nhiệm vụ là đặt hai mặt phẳng song song sao cho mọi điểm nằm giữa chúng và khoảng cách giữa các mặt phẳng càng nhỏ càng tốt."
date: "2026-06-29T16:30:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104651
codeforces_index: "J"
codeforces_contest_name: "The 2023 CCPC Online Contest"
rating: 0
weight: 104651
solve_time_s: 31
verified: false
draft: false
---

[CF 104651J - Tìm khoảng trống](https://codeforces.com/problemset/problem/104651/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trong không gian ba chiều. Nhiệm vụ là đặt hai mặt phẳng song song sao cho mọi điểm nằm giữa chúng và khoảng cách giữa các mặt phẳng càng nhỏ càng tốt. Tương tự, chúng ta muốn tìm một hướng trong không gian rồi chiếu tất cả các điểm lên hướng đó và giảm thiểu sự khác biệt giữa giá trị hình chiếu tối đa và tối thiểu. 

Một khi được xem theo cách này, bài toán sẽ trở thành một bài toán tối ưu hóa hình học theo các hướng: với bất kỳ vectơ đơn vị nào$\mathbf{u}$, mỗi điểm$(x_i, y_i, z_i)$tạo ra một phép chiếu vô hướng$p_i = \mathbf{u} \cdot \mathbf{x}_i$. Khoảng cách giữa hai mặt phẳng đỡ vuông góc với$\mathbf{u}$chính xác là$\max p_i - \min p_i$. Chúng tôi muốn giá trị nhỏ nhất như vậy trên tất cả các hướng. 

Kích thước đầu vào nhỏ, tối đa là 50 điểm. Điều đó ngay lập tức loại trừ bất cứ điều gì bậc hai về số hướng ứng cử viên nếu chúng ta liệt kê các hướng một cách ngây thơ từ tất cả các vectơ thực có thể có. Tuy nhiên, nó gợi ý rõ ràng rằng việc giảm hình học liên quan đến các cấu trúc theo cặp hoặc các ứng cử viên tổ hợp là điều được mong đợi. 

Một ý tưởng ngây thơ là thử tất cả các hướng được xác định bởi các vectơ thực tùy ý hoặc thậm chí rời rạc hóa mặt cầu đơn vị. Điều đó không thành công vì câu trả lời phụ thuộc vào các hướng hỗ trợ chính xác chứ không phải hướng dẫn được lấy mẫu. Một ý tưởng ngây thơ khác là cố định hai điểm và giả sử các mặt phẳng tối ưu trực giao với đường thẳng giữa chúng, nhưng điều này không chính xác trong ba chiều, vì hướng tối ưu phụ thuộc vào cấu hình cực trị của bao lồi chứ không chỉ các cặp điểm. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các điểm đều đồng phẳng. Trong trường hợp đó, câu trả lời là 0 vì chúng ta có thể chọn các mặt phẳng trùng với mặt phẳng đó. Bất kỳ phương pháp nào dựa vào việc chuẩn hóa tích chéo đều phải xử lý suy biến diện tích bằng 0 một cách cẩn thận. 

Một trường hợp cạnh khác xảy ra khi tất cả các điểm đều nằm trên một đường thẳng. Khi đó hướng tối ưu chính xác là hướng của đường thẳng và câu trả lời chỉ đơn giản là độ dài của khoảng chiếu dọc theo đường đó. 

## Phương pháp tiếp cận 

Quan sát quan trọng là hàm chúng ta muốn cực tiểu hóa,$f(\mathbf{u}) = \max_i (\mathbf{u} \cdot \mathbf{x}_i) - \min_i (\mathbf{u} \cdot \mathbf{x}_i)$, được xác định hoàn toàn bởi bao lồi của các điểm. Đối với một hướng cố định, chỉ các điểm trên thân tàu là quan trọng, vì các điểm bên trong không bao giờ xác định điểm cực trị.
