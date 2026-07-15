---
title: "CF 103389C - \u8fde\u9501\u5546\u5e97"
description: "Giả sử các đỉnh đều là chuỗi nhị phân $a{2t-1}chấm a1a0$ với chính xác $t$ số 0 và $t$ số một. Một bước di chuyển bao gồm việc chọn một số chỉ mục $j trong {1,dots,2t-1}$ và hoán đổi $a0$ với $aj$, tạo ra một chuỗi mới vẫn có các số 0 $t$ và các số $t$."
date: "2026-07-03T12:12:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103389
codeforces_index: "C"
codeforces_contest_name: "2021\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 103389
solve_time_s: 138
verified: false
draft: false
---

[CF 103389C - \u8fde\u9501\u5546\u5e97](https://codeforces.com/problemset/problem/103389/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

Đặt các đỉnh đều là chuỗi nhị phân$a_{2t-1}\dots a_1a_0$với chính xác$t$số không và$t$những cái đó. Một động thái bao gồm việc chọn một số chỉ mục$j \in {1,\dots,2t-1}$và trao đổi$a_0$với$a_j$, tạo ra một chuỗi mới vẫn có$t$số không và$t$những cái đó. 

Câu hỏi hỏi liệu tất cả$(t,t)$-sự kết hợp có thể được tạo ra bằng cách sắp xếp tất cả các đỉnh trong đó các chuỗi liên tiếp khác nhau bằng cách hoán đổi như vậy. 

Tương tự, điều này hỏi liệu đồ thị$G_t$các đỉnh của nó đều là tập con của${0,1,\dots,2t-1}$kích thước$t$, với các cạnh$$S \sim T \quad \Longleftrightarrow \quad T = S \triangle \{0,j\} \text{ for some } j \neq 0,$$có chu trình Hamilton. 

## Kết quả đã biết 

Mỗi đỉnh$S$có thể được phân loại bằng cách$0 \in S$. 

Nếu như$0 \in S$, sau đó$S = {0} \cup A$Ở đâu$A \subseteq {1,\dots,2t-1}$Và$|A| = t-1$. 

Nếu như$0 \notin S$, sau đó$S = B$Ở đâu$B \subseteq {1,\dots,2t-1}$Và$|B| = t$. 

Do đó, tập đỉnh được chia thành hai lớp:$$\mathcal{L}_{t-1} = \{A \subseteq [2t-1] : |A| = t-1\}, \quad
\mathcal{L}_t = \{B \subseteq [2t-1] : |B| = t\}.$$Theo cách nhận dạng này, việc hoán đổi$a_0$với$a_j$tương ứng chính xác với việc thêm hoặc xóa$j$từ tập con của$[2t-1]$. Do đó các cạnh kết nối$A \in \mathcal{L}_{t-1}$ĐẾN$B \in \mathcal{L}_t$chính xác khi nào$B = A \cup {j}$. 

Vì thế$G_t$là đẳng cấu với đồ thị mức giữa trên mạng Boolean của$[2t-1]$, giới hạn ở cấp độ$t-1$Và$t$. 

Giả thuyết ở mức độ trung bình khẳng định rằng đồ thị này là đồ thị Hamilton cho mọi$t$. Điều này được đặt ra vào những năm 1980 và vẫn mở trong nhiều thập kỷ. Cuối cùng nó đã được chứng minh một cách tổng quát bởi Mütze (loạt kết quả năm 2014-2016), chứng minh rằng mọi đồ thị ở mức trung bình đều có chu trình Hamilton. 

## Đối số một phần 

Đẳng cấu ở trên làm giảm bài toán xây dựng chu trình Hamilton trong đồ thị hai bên có các phần là$\binom{2t-1}{t-1}$Và$\binom{2t-1}{t}$các đỉnh, với độ kề được cho bởi hiệu đối xứng với một phần tử. 

Mỗi cạnh chuyển đổi tư cách thành viên của chính xác một phần tử$j \neq 0$, vì vậy mỗi bước là một sự hoán đổi hợp lệ của$a_0$với một số$a_j$. Bất kỳ chu trình Hamilton nào trong đồ thị mức trung bình ngay lập tức mang lại một thứ tự tuần hoàn của tất cả$(t,t)$-kết hợp thỏa mãn điều kiện trao đổi yêu cầu. 

Do đó, một giải pháp tích cực cho vấn đề cấp độ trung bình hàm ý một câu trả lời tích cực cho câu hỏi hiện tại. 

## Trạng thái 

Vấn đề tương đương với phỏng đoán cấp độ trung bình. 

Giả thuyết ở mức độ trung bình bây giờ là một định lý: một chu trình Hamilton tồn tại với mọi$t \ge 1$, được chứng minh bởi Mütze và cộng tác viên vào những năm 2010. 

Vì thế tất cả$(t,t)$-sự kết hợp$a_{2t-1}\dots a_1a_0$thực sự có thể được tạo ra bằng cách hoán đổi nhiều lần$a_0$với một số phần tử khác. 

Điều này hoàn thành giải pháp. ∎
