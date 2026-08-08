---
title: "CF 103993G - Chấm điểm"
description: "Đặt $G = (V,E)$ biểu thị đồ thị Hoa Kỳ liền kề của (18) và đặt $U tập hợp con V$. Đồ thị con cảm ứng $G mid U$ là lưỡng cực khi và chỉ nếu nó không chứa chu trình có độ dài lẻ, tương đương khi và chỉ khi mọi thành phần liên thông của $G mid U$ đều chấp nhận một 2 màu."
date: "2026-07-02T06:03:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103993
codeforces_index: "G"
codeforces_contest_name: "ICPC 2022-2023 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 103993
solve_time_s: 123
verified: false
draft: false
---

[CF 103993G - Tính điểm](https://codeforces.com/problemset/problem/103993/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$G = (V,E)$biểu thị đồ thị Hoa Kỳ tiếp giáp của (18) và đặt$U \subseteq V$. Đồ thị con cảm ứng$G \mid U$là lưỡng cực khi và chỉ nếu nó không chứa chu trình có độ dài lẻ, tương đương khi và chỉ khi mọi thành phần liên thông của$G \mid U$thừa nhận 2 màu. 

một bộ$U$là đồ thị con lưỡng cực cảm ứng cực đại khi và chỉ khi$G \mid U$là lưỡng cực và cho mọi$v \in V \setminus U$, đồ thị con cảm ứng$G \mid (U \cup {v})$chứa một chu trình lẻ. Tương tự, mọi đỉnh bị loại trừ đều cần thiết để duy trì tính chất lưỡng cực. 

Giới thiệu gia đình$$\mathcal{B} = \{U \subseteq V \mid G \mid U \text{ is bipartite}\}.$$Các đối tượng mong muốn là các phần tử tối đa$\mathcal{B}^\uparrow$theo nghĩa đại số họ ZDD từ Bài tập 236. 

Ràng buộc lưỡng cực có thể được biểu thị dưới dạng loại trừ tất cả các chu kỳ lẻ$C \subseteq V$:$$U \in \mathcal{B} \quad \Longleftrightarrow \quad \forall C \in \mathcal{C}_{\mathrm{odd}},\; C \nsubseteq U,$$Ở đâu$\mathcal{C}_{\mathrm{odd}}$là họ các tập đỉnh của tất cả các chu trình lẻ của$G$. 

Kể từ đây$$\mathcal{B} = \mathcal{C}_{\mathrm{odd}}^{\nearrow},$$phiên dịch$\mathcal{C}_{\mathrm{odd}}^{\nearrow}$là họ của tất cả các tập tránh các tập siêu chu kỳ lẻ, theo nghĩa của phép toán ZDD$f \nearrow g$từ Bài tập 236. Đồ thị con lưỡng cực cảm ứng cực đại khi đó là phần tử cực đại của họ này:$$\mathcal{M} = \mathcal{B}^\uparrow.$$Do đó việc tính toán giảm xuống còn đánh giá ZDD của$$\mathcal{M} = (\mathcal{C}_{\mathrm{odd}}^{\nearrow})^\uparrow.$$Cấu trúc này xác định họ duy nhất và việc triển khai ZDD áp dụng các quy tắc rút gọn đệ quy của Bài tập 237, truyền bá các ràng buộc bao hàm dọc theo thứ tự biến cố định của các đỉnh của$G$. Mỗi chu kỳ lẻ đóng góp một ràng buộc cấm bao gồm đồng thời tất cả các đỉnh của nó và cực đại sẽ loại bỏ bất kỳ tập hợp nào có thể được mở rộng trong khi vẫn bảo toàn tất cả các ràng buộc đó. 

Do đó số tập được chấp nhận là$$|\mathcal{M}| = \text{number of maximal elements of } \mathcal{B}.$$Một giá trị số rõ ràng phụ thuộc vào cấu trúc kề đầy đủ của đồ thị (18), vì cả tập hợp các chu kỳ lẻ$\mathcal{C}_{\mathrm{odd}}$và kết quả giảm ZDD phụ thuộc vào mối quan hệ tỷ lệ chính xác giữa các đỉnh. Biểu đồ đó không được chỉ định trong đoạn trích được cung cấp, do đó, số lượng đóng không thể được lấy từ thông tin có sẵn. 

Đặc tính cấu trúc của các trường hợp cực đoan không phụ thuộc vào dữ liệu bị thiếu. 

Đồ thị con lưỡng cực cảm ứng cực đại nhỏ nhất là bất kỳ tập bao gồm-tối thiểu nào$U \in \mathcal{B}^\uparrow$. Một tập hợp như vậy có đặc tính loại bỏ bất kỳ đỉnh nào khỏi$U$sẽ cho phép mở rộng và việc thêm bất kỳ đỉnh nào sẽ tạo ra một chu trình lẻ trong đồ thị con cảm ứng; số lượng chính xác của nó phụ thuộc vào cấu trúc chu kỳ lẻ cục bộ của$G$. 

Đồ thị con lưỡng cực cảm ứng lớn nhất là bất kỳ$U \in \mathcal{B}^\uparrow$số lượng tối đa. Mỗi tập hợp như vậy tương ứng với việc loại bỏ một đường ngang chu kỳ lẻ tối thiểu$V \setminus U$, nhưng kích thước của một đường ngang như vậy phụ thuộc vào cấu trúc chu trình chi tiết của$G$. 

Khung tương tự mở rộng đến các đồ thị con ba bên tối đa được tạo ra bằng cách thay thế$\mathcal{B}$với họ các tập đỉnh có đồ thị con cảm ứng không có chu kỳ cản trở khả năng 3 màu, tương đương không có đồ thị con nào yêu cầu bốn màu, điều này một lần nữa giảm xuống hệ thống ràng buộc ZDD đối với các cấu hình bị cấm và các phần tử tối đa của nó. 

Điều này hoàn thành việc rút gọn bài toán về đánh giá ZDD theo đại số họ của Bài tập 236. ∎
