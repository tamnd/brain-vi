---
title: "CF 919A - Siêu thị"
description: "Nhiệm vụ là quyết định làm thế nào để mua một lượng táo cố định trong khi giảm thiểu tổng chi phí trong điều kiện có nhiều siêu thị với các mức giá khác nhau. Mỗi siêu thị không trực tiếp đưa ra giá mỗi kg."
date: "2026-06-15T12:24:50+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 919
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 460 (Div. 2)"
rating: 800
weight: 919
solve_time_s: 271
verified: true
draft: false
---

[CF 919A - Siêu thị](https://codeforces.com/problemset/problem/919/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 4 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là quyết định làm thế nào để mua một lượng táo cố định trong khi giảm thiểu tổng chi phí trong điều kiện có nhiều siêu thị với các mức giá khác nhau. Mỗi siêu thị không trực tiếp đưa ra giá mỗi kg. Thay vào đó, nó đưa ra một mức giá trọn gói: trả một đơn vị tiền sẽ mua được b kg táo. Điều này ngầm xác định đơn giá của a/b, nhưng việc mua thực tế phải được thực hiện trong toàn bộ siêu thị mà không trộn lẫn các kích cỡ gói. 

Chúng ta được yêu cầu mua tổng cộng chính xác m kg và chúng ta có thể chọn bất kỳ siêu thị nào, có khả năng mua tất cả m kg từ một siêu thị duy nhất. Mục tiêu là tìm ra cách rẻ nhất để thu được m kg theo các quy tắc định giá này. 

Các ràng buộc quan trọng một cách đơn giản. Số lượng siêu thị n có thể lên tới 5000, trong khi m nhiều nhất là 100. Mỗi cặp giá a và b đều nhỏ, giới hạn bởi 100. Điều này ngay lập tức cho thấy rằng bất kỳ thuật toán tuyến tính hoặc bậc hai nào trong n và m đều có thể chấp nhận được, nhưng mọi thuật toán hàm mũ trong m sẽ không cần thiết vì m rất nhỏ. Cấu trúc cũng gợi ý rằng mỗi siêu thị hoạt động độc lập, do đó vấn đề giảm xuống còn việc đánh giá một biểu thức đơn giản cho mỗi siêu thị. 

Một điểm tinh tế có thể đánh lừa cách tiếp cận ngây thơ là diễn giải giá không chính xác. Một sai lầm phổ biến là cho rằng chúng ta chỉ có thể mua theo bội số của b kg và việc chia tỷ lệ một phần là không hợp lệ theo cách ngăn cản việc suy luận theo tỷ lệ trực tiếp. Trên thực tế, chi phí tăng theo tuyến tính: nếu một siêu thị tính giá a cho b kg, thì m kg từ siêu thị đó có giá chính xác là a * (m / b). Ví dụ: nếu a = 3 và b = 4 thì 5 kg có giá 3 * 5/4 = 3,75. Một lỗi tiềm ẩn khác là sử dụng phép chia số nguyên, việc này sẽ cắt ngắn một cách âm thầm và tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng mô hình hóa các quyết định mua hàng một cách rõ ràng, có thể nghĩ đến việc kết hợp nhiều siêu thị hoặc chia việc mua hàng thành nhiều phần. Vì m nhỏ nên người ta thậm chí có thể xem xét quy hoạch động trên tổng số kilôgam, trong đó dp[x] là chi phí tối thiểu để mua x kilôgam. Đối với mỗi siêu thị, chúng tôi sẽ cố gắng chuyển đổi liên tục từ mọi trạng thái x sang x + b hoặc xem xét chia tỷ lệ theo tỷ lệ một cách gián tiếp. Điều này dẫn đến một sự phức tạp không cần thiết: đối với mỗi siêu thị, chúng tôi sẽ mô phỏng các khoản đóng góp cho tất cả các trạng thái lên đến m, tạo ra công việc khoảng O(n * m^2) nếu được thực hiện cẩn thận hoặc tệ hơn nếu được lập mô hình kém rõ ràng hơn. 

Quan sát chính là mỗi siêu thị đều độc lập và không tương tác với những siêu thị khác. Không có lợi ích gì khi kết hợp các siêu thị trong một chiến lược mua hàng duy nhất vì chúng tôi không bị hạn chế bởi các gói riêng biệt ngoài quy mô tuyến tính. Mỗi siêu thị chỉ đơn giản đưa ra một mức giá cố định cho mỗi kg, do đó vấn đề giảm xuống còn việc tính m * (a / b) cho mỗi siêu thị và lấy mức tối thiểu. 

Điều này loại bỏ tất cả các cấu trúc tổ hợp. Chúng ta không cần phải xem xét sự kết hợp của các siêu thị nữa mà chỉ đánh giá một biểu thức đơn giản trên mỗi dòng đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP trên kg | O(n * m^2) | O(m) | Quá chậm/không cần thiết | 
| Đánh giá trực tiếp từng siêu thị | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc n và m, xác định có bao nhiêu siêu thị tồn tại và phải mua bao nhiêu kg. 
2. Khởi tạo biến best_cost thành một số rất lớn. Điều này sẽ theo dõi tổng chi phí nhỏ nhất được tìm thấy trên tất cả các siêu thị. 
3. Đối với mỗi siêu thị, hãy đọc a và b, thể hiện giá cả và số lượng của một gói hàng. 
4. Tính chi phí hiệu quả cho m kg từ siêu thị này bằng công thức chia tỷ lệ m * a / b. Điều này hiệu quả vì giá mỗi kg là không đổi trong mỗi siêu thị. 
5. So sánh chi phí được tính toán này với best_cost và cập nhật best_cost nếu giá trị mới nhỏ hơn. 
6. Sau khi xử lý tất cả các siêu thị, xuất best_cost dưới dạng số dấu phẩy động. 

### Tại sao nó hoạt động 

Mỗi siêu thị xác định một hàm chi phí tuyến tính theo số kg được mua. Vì không có ràng buộc nào về việc phân chia giữa các siêu thị hoặc giảm giá cho các tổ hợp, nên chi phí tối thiểu cho số lượng cần thiết chỉ đơn giản là mức tối thiểu trên tất cả các hàm tuyến tính riêng lẻ được đánh giá tại m. Thuật toán bảo toàn tính bất biến rằng sau khi xử lý i siêu thị, best_cost bằng chi phí tối thiểu có thể đạt được trong số i lựa chọn đó. Bởi vì mỗi siêu thị được xem xét chính xác một lần và độc lập nên không có ứng viên hợp lệ nào bị bỏ sót. 

## Giải pháp Python```
PythonRun
```Giải pháp đọc từng dòng đầu vào và duy trì một giá trị tối thiểu duy nhất. Mỗi siêu thị đóng góp một chi phí ứng viên được tính toán và chúng tôi không bao giờ lưu trữ dữ liệu không cần thiết. 

Chi tiết triển khai chính là sử dụng phép chia dấu phẩy động. Sử dụng phép chia số nguyên sẽ phá hủy độ chính xác và tạo ra câu trả lời sai. Nhân m trước tiên đảm bảo chúng ta tránh được việc cắt ngắn sớm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```

```Chúng tôi đánh giá từng siêu thị: 

| Siêu thị | một | b | Giá cho 5 kg (5 * a/b) | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2,5 | 2,5 | 
| 2 | 3 | 4 | 3,75 | 2,5 | 
| 3 | 1 | 3 | 1.6666667 | 1.6666667 | 

Siêu thị thứ ba rẻ nhất nên chúng tôi chọn nó. 

Điều này xác nhận rằng thuật toán đánh giá chính xác tất cả các tùy chọn một cách độc lập và chọn mức tối thiểu. 

### Mẫu 2 (đã thi công) 

đầu vào:```

```| Siêu thị | một | b | Giá cho 1 kg (1 * a/b) | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 98 | 99 | 0.989898... | 0.989898... | 
| 2 | 1 | 2 | 0,5 | 0,5 | 
| 3 | 5 | 10 | 0,5 | 0,5 | 
| 4 | 7 | 7 | 1.0 | 0,5 | 

Siêu thị thứ hai và thứ ba hòa nhau, và thuật toán giữ mức tối thiểu một cách chính xác. 

Điều này cho thấy thuật toán xử lý các mối quan hệ và so sánh phân số một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi siêu thị được xử lý một lần và làm việc liên tục | 
| Không gian | O(1) | Chỉ có một biến duy nhất được duy trì | 

Các ràng buộc cho phép lên tới 5000 siêu thị, vì vậy chỉ cần một lượt đi là đủ nhanh. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / 1 1 | 1.0 | trường hợp cạnh tối thiểu | 
| tỷ lệ bằng nhau | cùng giá trị | xử lý cà vạt | 
| chi phí sai lệch | lựa chọn tối thiểu đúng | so sánh đúng đắn | 
| trường hợp phân số | phép chia chính xác | độ chính xác nổi | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi m = 1. Trong trường hợp này, câu trả lời sẽ giảm trực tiếp xuống tỷ lệ a/b nhỏ nhất trong số tất cả các siêu thị. Thuật toán xử lý việc này một cách tự nhiên vì nó luôn tính m * a / b, trở thành a / b khi m = 1. 

Một trường hợp khác là khi nhiều siêu thị có đơn giá giống nhau nhưng kích cỡ gói hàng khác nhau. Ví dụ: 1/2 và 2/4 đều biểu thị cùng một tỷ lệ. Thuật toán xử lý chúng như nhau vì cả hai đều đánh giá cùng một giá trị dấu phẩy động và giá trị tối thiểu vẫn ổn định. 

Trường hợp tinh tế cuối cùng là độ nhạy chính xác. Khi các giá trị rất gần nhau, các phép so sánh dấu phẩy động vẫn hoạt động chính xác với dung sai 1e-6 bắt buộc vì phép tính chỉ bao gồm một phép nhân và phép chia cho mỗi mục nhập, ổn định về mặt số đối với các ràng buộc nhất định.
