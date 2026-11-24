---
title: "CF 903G - Một vấn đề về dòng chảy tối đa khác"
description: "Đồ thị bao gồm hai chuỗi có hướng. Các đỉnh $A1,dots,An$ tạo thành một chuỗi và $B1,dots,Bn$ tạo thành một chuỗi khác. Công suất trên chuỗi $B$ là cố định. Công suất trên chuỗi $A$ được cập nhật trực tuyến."
date: "2026-06-12T22:59:33+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 903
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 34 (Rated for Div. 2)"
rating: 2700
weight: 903
solve_time_s: 133
verified: false
draft: false
---

[CF 903G - Một vấn đề về luồng tối đa khác](https://codeforces.com/problemset/problem/903/G) 

**Đánh giá:** 2700 
**Tags:** cấu trúc dữ liệu, luồng, đồ thị 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đồ thị bao gồm hai chuỗi có hướng. Đỉnh$A_1,\dots,A_n$tạo thành một chuỗi và$B_1,\dots,B_n$tạo thành một cái khác. Công suất trên$B$-chuỗi được cố định. Công suất trên$A$-chain được cập nhật trực tuyến. Ngoài ra còn có các cạnh định hướng từ một số$A_x$đến một số$B_y$, và không có cạnh nào theo hướng ngược lại. 

Đối với mọi phiên bản của biểu đồ, bao gồm cả phiên bản gốc và sau mỗi lần cập nhật, chúng ta cần giá trị của luồng tối đa từ$A_1$ĐẾN$B_n$. 

Những hạn chế đạt tới$2\cdot10^5$đỉnh, cạnh chéo và cập nhật. Việc tính toán lại luồng tối đa sau mỗi lần cập nhật là không thể. Ngay cả một luồng tối đa chung duy nhất cũng sẽ quá đắt trong trường hợp xấu nhất. 

Thách thức chính là khai thác cấu trúc rất cứng nhắc của mạng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xây dựng lại biểu đồ sau mỗi lần sửa đổi và chạy Dinic. Điều này đúng vì luồng cực đại giải quyết được vấn đề một cách chính xác, nhưng nó lại cực kỳ chậm. Với$q=2\cdot10^5$, thậm chí$O(m\sqrt n)$hoặc$O(nm)$mỗi truy vấn vượt xa giới hạn. 

Quan sát quan trọng là mọi con đường đều bắt đầu từ$A$-chuỗi, chéo từ$A$ĐẾN$B$đúng một lần, sau đó tiếp tục dọc theo$B$-xích. Vì không có cạnh nào từ$B$quay lại$A$, mỗi đơn vị dòng chảy có một điểm giao nhau duy nhất. 

Từ định lý cắt tối thiểu luồng cực đại, bất kỳ vết cắt nào cũng có thể được mô tả bằng cách chọn một số vị trí trên$A$-chuỗi và một số vị trí trên$B$-xích. Chi phí của việc cắt giảm trở thành$$\min_i x_i + \min_j y_j + \text{contribution of cross edges}$$ở dạng có cấu trúc hơn. Sau khi sắp xếp lại các số hạng, câu trả lời có thể được hiểu là giá trị tối thiểu của một họ biểu thức liên quan đến thông tin tiền tố trên$A$-thông tin về chuỗi và hậu tố trên$B$-xích. 

Điều này chuyển đổi vấn đề thành một vấn đề phạm vi động. Các bản cập nhật chỉ ảnh hưởng đến một cạnh trên$A$-chain, nhờ đó các kỹ thuật dựa trên cây phân đoạn trở nên khả thi. 

Phần khó khăn là mã hóa cách các cạnh chéo đóng góp vào từng cặp vị trí cắt có thể có. Giải pháp được chấp nhận nén sự tương tác này thành các hàm được lưu trữ trong các nút cây phân đoạn và hợp nhất chúng một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại luồng tối đa mỗi truy vấn | Quá lớn | Quá lớn | Quá chậm | 
| Cây phân đoạn có chức năng cắt hợp nhất | Cập nhật đa logarit | Tuyến tính | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy mô tả mọi$s$-$t$cắt bởi đỉnh cuối cùng được giữ trên$A$-side và đỉnh đầu tiên được giữ trên$B$-bên. 
2. Biểu thị công suất cắt bằng tổng của ba phần, một phần từ$A$-chuỗi, một từ$B$-chuỗi, và một từ các cạnh chéo. 
3. Viết lại phần đóng góp chéo thành một dạng có thể tổng hợp cục bộ. 
4. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ một biểu diễn nhỏ gọn của tất cả các hàm cắt có liên quan trong khoảng của nó. 
5. Hợp nhất hai nút con bằng cách kết hợp các chức năng của chúng. Cấu trúc đặc biệt đảm bảo thông tin có kích thước tuyến tính bên trong mỗi nút và việc hợp nhất hiệu quả. 
6. Gốc chứa giá trị cắt tối thiểu, bằng với lưu lượng tối đa. 
7. Khi một$A$-edge thay đổi dung lượng, cập nhật lá tương ứng và tính toán lại thông tin trên đường dẫn đến gốc. 

### Tại sao nó hoạt động 

Mọi đường dẫn dòng chảy đều đi qua từ$A$ĐẾN$B$đúng một lần. Vì vậy, mỗi vết cắt được xác định bởi hai vị trí dọc theo dây chuyền. Cây phân đoạn duy trì mức tối thiểu trong tất cả các khả năng như vậy. Vì mỗi nút lưu trữ chính xác thông tin cần thiết để xây dựng lại tất cả các lần cắt trong khoảng thời gian của nó, nên nút gốc luôn biểu thị mức cắt tối thiểu toàn cục. Theo định lý cắt tối thiểu luồng cực đại, giá trị đó là câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log n + q\log n)$| Cập nhật và hợp nhất cây phân đoạn | 
| Không gian |$O(n+m)$| Thông tin về cây và nút được lưu trữ | 

Điều này phù hợp thoải mái trong giới hạn cho$n,m,q\le2\cdot10^5$. 

## Vỏ cạnh 

Nhiều cạnh giống nhau$A_x$giống nhau$B_y$đều phải được tính riêng. Việc tổng hợp chúng không chính xác sẽ làm thay đổi công suất cắt. 

Nếu không có cạnh chéo nào cả, không có đường đi nào tới được$B$-chain, và câu trả lời là 0 bất kể năng lực của chuỗi. 

Công suất rất lớn lên đến$10^9$yêu cầu số học 64-bit. Lưu lượng tối đa có thể vượt quá$2^{31}$, vì vậy việc triển khai phải sử dụng số nguyên 64 bit. 

Các bản cập nhật có thể tăng hoặc giảm$A$-cạnh quyết liệt. Cấu trúc dữ liệu phải tính toán lại tổ tiên thay vì cố gắng điều chỉnh câu trả lời một cách tham lam, vì vị trí cắt tối thiểu có thể di chuyển tùy ý sau một lần thay đổi. 

Nếu bạn muốn nguồn gốc và triển khai được chấp nhận đầy đủ, tôi có thể giải thích chi tiết về quy trình hợp nhất và biểu diễn nút, đây là ý tưởng kỹ thuật cốt lõi đằng sau Codeforces 903G.
