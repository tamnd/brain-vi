---
title: "CF 102769A - Lời chào từ Tần Hoàng Đảo"
description: "Chúng tôi có một thùng chứa quả bóng màu đỏ và màu xanh. Số bi đỏ là r, số bi xanh là b. Hai quả bóng được chọn ngẫu nhiên như nhau mà không cần thay thế, nghĩa là mỗi cặp bóng đều có cơ hội được chọn như nhau."
date: "2026-07-28T23:17:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102769
codeforces_index: "A"
codeforces_contest_name: "2020 China Collegiate Programming Contest Qinhuangdao Site"
rating: 0
weight: 102769
solve_time_s: 55
verified: false
draft: false
---

[CF 102769A - Lời chào từ Tần Hoàng Đảo](https://codeforces.com/problemset/problem/102769/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một thùng chứa quả bóng màu đỏ và màu xanh. Số quả bóng màu đỏ là`r`, số bi xanh là`b`. Hai quả bóng được chọn ngẫu nhiên như nhau mà không cần thay thế, nghĩa là mỗi cặp bóng đều có cơ hội được chọn như nhau. Nhiệm vụ là tìm xác suất để cả hai quả bóng được chọn đều có màu đỏ và in nó dưới dạng phân số tối giản. 

Phần quan trọng là chuyển thí nghiệm ngẫu nhiên thành phép đếm. Tổng số cặp có thể xuất phát từ việc chọn hai quả bóng bất kỳ trong số tất cả các quả bóng có sẵn. Vì có`r + b`quả bóng, tổng số kết quả là:$$\binom{r+b}{2}$$Kết quả thành công là các cặp chỉ chứa bi đỏ. có`r`bi đỏ nên số cặp thành công là:$$\binom{r}{2}$$Câu trả lời là tỉ số giữa hai đại lượng này:$$\frac{\binom{r}{2}}{\binom{r+b}{2}}$$Các ràng buộc rất nhỏ, tối đa 10 trường hợp thử nghiệm và số lượng mỗi màu không vượt quá 100. Điều này có nghĩa là ngay cả số học trực tiếp cũng đủ. Không cần các thuật toán nâng cao hoặc cấu trúc dữ liệu lớn. Thách thức chính là xử lý tính toán xác suất một cách chính xác và giảm phân số. 

Một giải pháp bất cẩn có thể thất bại nếu coi hai lượt rút là độc lập. Ví dụ: với đầu vào:```
2 1
```Có ba quả bóng, hai quả màu đỏ và một quả màu xanh. Xác suất để lấy được hai bi đỏ là:$$\frac{2}{3} \times \frac{2}{3}$$vì sau khi chọn được 1 bi đỏ thì số bi đỏ còn lại sẽ thay đổi. Phép tính đúng là:$$\frac{\binom{2}{2}}{\binom{3}{2}}=\frac{1}{3}$$Một lỗi phổ biến khác là quên rằng câu trả lời phải được rút gọn lại. Vì:```
8 8
```phần thô là:$$\frac{\binom{8}{2}}{\binom{16}{2}}=\frac{28}{120}$$phải trở thành:$$\frac{7}{30}$$In ấn`28/120`sẽ tương đương về mặt toán học nhưng bị từ chối. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là đếm tất cả các cặp có thể có và tất cả các cặp thành công, sau đó đơn giản hóa phân số thu được. Phiên bản brute-force theo nghĩa đen sẽ liệt kê từng cặp bóng, kiểm tra xem cả hai quả bóng được chọn có màu đỏ hay không. Điều này đúng vì mọi lựa chọn có thể được xem xét chính xác một lần. 

Tuy nhiên, cách giải thích này là không cần thiết. Nếu có tới 200 quả bóng, việc liệt kê tất cả các cặp có nghĩa là kiểm tra xung quanh:$$\binom{200}{2}=19900$$cặp cho một trường hợp duy nhất. Với những ràng buộc này nó vẫn sẽ vượt qua, nhưng nó bỏ qua cấu trúc toán học của bài toán. Nếu ý tưởng tương tự được mở rộng cho các đầu vào lớn hơn nhiều thì việc liệt kê cặp sẽ nhanh chóng trở thành nút thắt cổ chai. 

Quan sát hữu ích là các quả bóng giống hệt nhau ngoại trừ màu sắc của chúng. Chúng ta không quan tâm quả bóng đỏ cụ thể nào được chọn, chỉ có bao nhiêu cách để chọn được hai quả bóng đỏ. Sự kết hợp cho số lượng này ngay lập tức. 

Phương pháp brute-force hoạt động vì mọi cặp đều độc lập và dễ phân loại, nhưng bài toán chỉ yêu cầu số lượng cặp chứ không phải bản thân các cặp. Quan sát cho thấy các kết hợp đã thể hiện số lượng các lựa chọn có thể sẽ làm giảm toàn bộ vấn đề xuống việc tính toán hai giá trị và đơn giản hóa tỷ lệ của chúng. 

Việc rút gọn phân số là cần thiết vì đầu ra yêu cầu một phân số tối giản. Việc tính ước số chung lớn nhất của tử số và mẫu số sẽ cho ra số chính xác mà cả hai giá trị sẽ được chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r+b)^2) | O(1) | Hoạt động với các giới hạn nhất định nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lần lựa chọn thành công. Hai quả bóng màu đỏ có thể được chọn từ`r`quả bóng màu đỏ trong:$$\frac{r(r-1)}{2}$$cách. Đây là tử số của xác suất vì mọi kết quả thành công đều chứa đúng hai quả bóng màu đỏ. 

1. Tính tổng số các lựa chọn có thể có. Hai quả bóng có thể được chọn từ tất cả`r+b`bóng trong:$$\frac{(r+b)(r+b-1)}{2}$$cách. Đây là mẫu số vì mọi cặp có thể có đều có xác suất bằng nhau. 

1. Tìm ước chung lớn nhất của tử số và mẫu số. Chia cả hai giá trị cho số này sẽ tạo ra biểu diễn duy nhất tối giản của cùng một xác suất. 
2. Xuất ra tử số và mẫu số rút gọn theo dạng chữ hoa chữ thường được yêu cầu. 

Tại sao nó hoạt động: 

Xác suất của một sự kiện là số kết quả thuận lợi chia cho số tất cả các kết quả khi mọi kết quả đều có khả năng xảy ra như nhau. Mọi cặp bi có thể có đều có xác suất được chọn như nhau nên chỉ cần đếm cặp là đủ. Tử số đếm chính xác các cặp chứa hai quả bóng màu đỏ, trong khi mẫu số đếm mọi cặp có thể. Việc giảm phân số không làm thay đổi giá trị của nó, do đó phân số được tạo ra biểu thị xác suất cần thiết ở định dạng bắt buộc.
