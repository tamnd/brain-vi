---
title: "CF 104720C - Lớp học nấu ăn"
description: "Chúng ta có một nhóm đối thủ cố định, mỗi người có một giá trị kỹ năng đã biết và Autumn, người cũng có một giá trị kỹ năng ban đầu. Autumn phải chọn chính xác một trong số các lớp đào tạo có sẵn, mỗi lớp sẽ bổ sung thêm một mức tăng cường tích cực cố định cho kỹ năng của cô ấy."
date: "2026-06-29T07:10:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104720
codeforces_index: "C"
codeforces_contest_name: "UTPC x WiCS Contest 10-06-23"
rating: 0
weight: 104720
solve_time_s: 29
verified: false
draft: false
---

[CF 104720C - Lớp học nấu ăn](https://codeforces.com/problemset/problem/104720/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhóm đối thủ cố định, mỗi người có một giá trị kỹ năng đã biết và Autumn, người cũng có một giá trị kỹ năng ban đầu. Autumn phải chọn chính xác một trong số các lớp đào tạo có sẵn, mỗi lớp sẽ bổ sung thêm một mức tăng cường tích cực cố định cho kỹ năng của cô ấy. Sau khi chọn một lớp, kỹ năng cuối cùng của cô ấy sẽ trở thành kỹ năng ban đầu cộng với mức tăng cường đó.

 Khi tất cả các kỹ năng đã được sửa, thứ hạng của cuộc thi được xác định hoàn toàn bằng cách sắp xếp người tham gia theo kỹ năng theo thứ tự giảm dần. Kỹ năng cao hơn có nghĩa là thứ hạng tốt hơn. Nếu nhiều người có cùng kỹ năng, họ có cùng thứ hạng và thứ hạng tiếp theo sẽ vượt lên trước quy mô của nhóm hòa.

 Nhiệm vụ là xác định mùa thu nên chọn lớp nào để đạt được thứ hạng tốt nhất có thể sau khi nâng cấp.

 Kích thước đầu vào quan trọng đáng kể. Cả số lượng thí sinh và số lớp có thể lên tới 200.000. Một giải pháp so sánh trực tiếp mọi lớp với mọi đối thủ cạnh tranh sẽ yêu cầu tới 40 tỷ phép so sánh, vượt xa giới hạn 2 giây cho phép. Điều này ngay lập tức loại trừ mọi tương tác bậc hai giữa hai mảng. 

Một điểm tinh tế là mối quan hệ ảnh hưởng như thế nào đến thứ hạng. Nếu Autumn có quan hệ tốt với nhiều đối thủ ở một cấp độ kỹ năng nhất định, thì thứ hạng của cô ấy phụ thuộc vào việc có bao nhiêu người ở trên cô ấy một cách nghiêm ngặt, chứ không chỉ là liệu có ai ngang bằng với cô ấy hay không. Điều này có nghĩa là chúng ta phải tính toán cẩn thận xem có bao nhiêu đối thủ có kỹ năng lớn hơn kỹ năng cuối cùng của Autumn.

 Một sai lầm ngây thơ là coi thứ hạng là “vị trí trong danh sách được sắp xếp sau khi chèn” mà không xử lý đúng các giá trị bằng nhau. Ví dụ: nếu đối thủ cạnh tranh có kỹ năng`[10, 10, 5]`và mùa thu trở thành`10`, cô ấy không phải hạng 2 mà là hạng 1. 

Một trường hợp thất bại khác xuất phát từ việc tính toán lại thứ hạng một cách độc lập cho mỗi lớp bằng cách sử dụng tính năng sắp xếp. Đối với đầu vào lớn, việc sắp xếp hoặc quét liên tục toàn bộ mảng trên mỗi lớp sẽ hết thời gian chờ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi lớp, hãy tính kỹ năng cuối cùng của Autumn, sau đó quét tất cả các đối thủ và đếm xem có bao nhiêu người có kỹ năng cao hơn. Số đó cộng thêm một sẽ cho biết thứ hạng của Autumn trong lớp đó. Sau đó chúng tôi lấy thứ hạng tối thiểu trong tất cả các lớp. 

Điều này hiệu quả vì thứ hạng chỉ phụ thuộc vào số lượng người ở trên mức Mùa thu chứ không phụ thuộc vào thứ tự của họ. Tuy nhiên, cách tiếp cận này thực hiện$O(N)$làm việc theo từng lớp, dẫn đến$O(NM)$tổng số hoạt động. In the worst case, this is 4×10¹⁰ comparisons, which is infeasible.

 Quan sát quan trọng là đối với giá trị kỹ năng cố định của ứng viên$x$, Thứ hạng của Autumn hoàn toàn được quyết định bởi số lượng thí sinh có kỹ năng lớn hơn$x$. Nếu chúng ta sắp xếp các kỹ năng của đối thủ một lần, chúng ta có thể trả lời số lượng này bằng cách sử dụng tìm kiếm nhị phân theo thời gian logarit. Sau đó, mỗi lớp sẽ rút gọn thành một truy vấn duy nhất: “có bao nhiêu giá trị lớn hơn$S_A + P_i$?” 

Sắp xếp chi phí một lần$O(N \log N)$và mỗi truy vấn có giá$O(\log N)$, đưa ra giải pháp tổng thể hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(1) | Quá chậm | 
| Tối ưu | O(N log N + M log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các kỹ năng của thí sinh và tách riêng chữ cái đầu của Autumn
