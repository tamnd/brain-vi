---
title: "CF 102253I - Tôi nguyền rủa chính mình"
description: "Đồ thị được kết nối, vô hướng và có trọng số. Sự đảm bảo về cấu trúc đặc biệt là không có cạnh nào có thể tham gia vào hai chu trình đơn giản khác nhau. Đây chính xác là thuộc tính xương rồng có liên quan ở đây."
date: "2026-08-17T21:40:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102253
codeforces_index: "I"
codeforces_contest_name: "2017 Chinese Multi-University Training, BeihangU Contest"
rating: 0
weight: 102253
solve_time_s: 362
verified: false
draft: false
---

[CF 102253I - Tôi nguyền rủa chính mình](https://codeforces.com/problemset/problem/102253/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đồ thị được kết nối, vô hướng và có trọng số. Sự đảm bảo về cấu trúc đặc biệt là không có cạnh nào có thể tham gia vào hai chu trình đơn giản khác nhau. Đây chính xác là thuộc tính xương rồng có liên quan ở đây. Chúng ta cần sắp xếp tất cả các cây khung theo tổng trọng số cạnh của chúng, gọi trọng số tại vị trí (k) (V(k)) và tính toán 

[ 
\sum_{k=1}^{K} k\cdot V(k)\pmod {2^{32}}. 
] 

Nếu đồ thị có ít hơn (k) cây bao trùm thì (V(k)) được xác định bằng 0. 

Điều quan trọng là ngừng suy nghĩ về cây bao trùm như các tập hợp con tùy ý của các cạnh. Trong biểu đồ xương rồng, mọi cây cầu đều phải nằm trong mọi cây bao trùm. Trong mỗi chu kỳ, phải loại bỏ chính xác một cạnh. Khi một cạnh bị loại bỏ khỏi mỗi chu trình, tất cả các cạnh còn lại sẽ tạo thành một cây bao trùm. Do đó, mỗi cây bao trùm được xác định độc lập bằng cách chọn một cạnh bị loại bỏ khỏi mỗi chu kỳ. 

Giả sử tổng trọng số của tất cả các cạnh đồ thị là (S). Nếu các chu trình là (C_1,C_2,\ldots,C_t) và chu trình (C_i) có trọng số cạnh 

[ 
a_{i,1},a_{i,2},\ldots,a_{i,m_i}, 
] 

thì cây bao trùm thu được bằng cách loại bỏ một cạnh khỏi mỗi chu trình có trọng số 

[ 
S-(a_{1,p_1}+a_{2,p_2}+\cdots+a_{t,p_t}). 
] 

Vì vậy, các trọng số của cây bao trùm nhỏ tương ứng chính xác với các tổng lớn được hình thành bằng cách chọn một số từ mỗi chu kỳ. Chúng tôi chỉ cần số tiền loại bỏ (K) lớn nhất như vậy. 

Đồ thị có nhiều nhất (1000) đỉnh và (2n-3) cạnh, vì vậy việc tìm tất cả các chu trình bằng thuật toán đồ thị tuyến tính là điều dễ dàng. Phần khó khăn là số lượng cây bao trùm có thể rất lớn. Một cây xương rồng với hàng trăm hình tam giác có thể có nhiều cây bao trùm theo cấp số nhân. Mặt khác, (K) nhiều nhất là (10^5) và tổng (K) trên tất cả các trường hợp thử nghiệm nhiều nhất là (10^6). Do đó, thuật toán dự định phải phụ thuộc vào (K), thay vì vào tổng số cây bao trùm. 

Có một số trường hợp khó xử lý sai. Đầu tiên, một đồ thị có thể không có chu trình nào cả. Ví dụ,```
2 1
1 2 7
1
```có đúng một cây bao trùm, có trọng số (7), nên đáp án là (7). Việc triển khai giả định ít nhất một chu kỳ và khởi tạo danh sách tổng loại bỏ không chính xác có thể tạo ra số không. 

Thứ hai, các cây bao trùm khác nhau có thể có cùng trọng số và các bản sao này vẫn phải chiếm các vị trí khác nhau trong thứ tự. Vì```
3 3
1 2 5
2 3 5
3 1 5
5
```có ba cây bao trùm, tất cả đều có trọng số (10). Do đó (V(1)=V(2)=V(3)=10), trong khi (V(4)=V(5)=0), cho 

[ 
10+20+30=60. 
] 

Việc triển khai bất cẩn loại bỏ các khoản trùng lặp sẽ chỉ giữ lại một giá trị một cách không chính xác. 

Thứ ba, các chu trình được phép chia sẻ các đỉnh. Ví dụ,```
5 6
1 2 1
2 3 2
3 1 3
3 4 4
4 5 5
5 3 6
9
```chứa hai hình tam giác có chung đỉnh (3). Chúng vẫn là những lựa chọn độc lập cho một cây bao trùm, do đó có (3\cdot3=9) cây bao trùm. Phương pháp tìm chu trình giả sử tất cả các chu trình đều không khớp với đỉnh sẽ thất bại trên biểu đồ này. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liệt kê mọi cây bao trùm, tính trọng số của nó, sắp xếp tất cả các trọng số và lấy (K) đầu tiên. Điều này đúng vì mọi cây bao trùm có thể đều được xem xét. Nó hoàn toàn không thực tế vì số lượng cây bao trùm là theo cấp số nhân ngay cả khi bị hạn chế về xương rồng. Ví dụ: 499 hình tam giác có chung một đỉnh sử dụng 999 đỉnh và 1497 cạnh và đã cung cấp (3^{499}) cây bao trùm. Việc liệt kê chúng sẽ yêu cầu theo thứ tự (3^{499}) trạng thái, vượt xa mọi số lượng hoạt động khả thi. 

Cấu trúc xương rồng mang lại sự giảm thiểu lớn đầu tiên. Một cây cầu không bao giờ có thể bị loại bỏ khỏi cây bao trùm. Một chu trình cần loại bỏ chính xác một trong các cạnh của nó. Vì các chu trình khác nhau không có chung cạnh nên những lựa chọn này là độc lập. Bài toán đồ thị trở thành bài toán về dãy: với mỗi chu trình, lấy trọng số một cạnh và xét tất cả các tổng có thể có. 

Sự giảm thứ hai xuất phát từ thực tế là chúng ta chỉ cần tổng (K) tốt nhất. Sắp xếp trọng số cạnh của mỗi chu kỳ theo thứ tự giảm dần. Nếu (A) chứa số tiền tốt nhất thu được từ các chu kỳ được xử lý cho đến nay và (B) là chu kỳ tiếp theo thì tất cả các ứng cử viên mới đều được 

[ 
A_i+B_j. 
] 

Cả hai mảng đều được sắp xếp theo thứ tự giảm dần. Đối với một (B_j) cố định, chuỗi 

[ 
A_0+B_j,\ A_1+B_j,\ A_2+B_j,\ldots 
] 

cũng đang giảm dần. Do đó, tích Descartes của hai mảng có thể được xem như một số chuỗi đã sắp xếp cần được hợp nhất. Hàng đợi ưu tiên có thể giữ phần tử lớn nhất hiện tại từ mọi chuỗi. Bất cứ khi nào (A_i+B_j) được trích xuất, ứng cử viên tiếp theo từ cùng chuỗi đó là (A_{i+1}+B_j). 

Chúng tôi giữ lại tối đa (K) kết quả sau mỗi lần hợp nhất. Đây là kỹ thuật hợp nhất trình tự được mô tả bởi bài xã luận chính thức cho vấn đề này. 

Giải pháp brute-force hoạt động hiệu quả vì nó thể hiện rõ ràng mọi sự kết hợp của các lựa chọn theo chu kỳ, nhưng không thành công vì sản phẩm đó quá lớn. Quan sát cho thấy chỉ có tổng thứ tự (K) đầu tiên mới quan trọng cho phép chúng ta loại bỏ toàn bộ phần đuôi không nhìn thấy được sau mỗi lần hợp nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot T\log T)), trong đó (T=\prod_i m_i) | (O(T)) | Quá chậm | 
| Tối ưu | (O(n+m+K\sum_i\log m_i)), giới hạn bởi (O |
