---
title: "CF 105093A - 4 \u2666"
description: "Chúng tôi được cung cấp một bản ghi đầy đủ về một lần thực hiện câu đố suy luận ba bóng đèn, ba công tắc cổ điển. Ba công tắc A, B và C, mỗi công tắc điều khiển chính xác một trong các bóng đèn 1, 2 và 3, nhưng chưa xác định được ánh xạ. Tất cả các công tắc đều khởi động ở trạng thái tắt."
date: "2026-06-27T20:48:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "A"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 28
verified: false
draft: false
---

[CF 105093A - 4 \u2666](https://codeforces.com/problemset/problem/105093/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bản ghi đầy đủ về một lần thực hiện câu đố suy luận ba bóng đèn, ba công tắc cổ điển. Ba công tắc A, B và C, mỗi công tắc điều khiển chính xác một trong các bóng đèn 1, 2 và 3, nhưng chưa xác định được ánh xạ. Tất cả các công tắc đều khởi động ở trạng thái tắt. Đoạn ghi âm mô tả hai công tắc đang được bật, chờ đợi, một trong số chúng được tắt và sau đó cánh cửa được mở ra. Sau đó, chúng ta quan sát bóng đèn nào sáng và trạng thái nhiệt độ của đúng một bóng đèn còn lại. 

Điểm mấu chốt là lý luận vật lý trong câu đố quyết định hoàn toàn việc lập bản đồ. Bóng đèn hiện đang bật phải tương ứng với công tắc vẫn bật. Bóng đèn nóng mà tắt phải tương ứng với công tắc đã bật rồi tắt sau một thời gian. Bóng đèn nguội phải tương ứng với công tắc chưa từng chạm vào. 

Đầu ra bắt buộc phải là hoán vị của A, B và C theo thứ tự bóng đèn 1, 2 và 3, nghĩa là chúng ta phải xuất ra công tắc nào điều khiển bóng đèn 1, công tắc nào điều khiển bóng đèn 2 và công tắc nào điều khiển bóng đèn 3. 

Kích thước đầu vào không đổi: chính xác là bảy dòng. Điều này loại bỏ mọi lo ngại về độ phức tạp của thuật toán; tính chính xác phụ thuộc hoàn toàn vào việc phân tích cú pháp chính xác và lý luận xác định. 

Trường hợp chính không rõ ràng là bóng đèn thứ hai được quan sát ở đầu vào không nhất thiết phải là bóng đèn nóng. Thay vào đó, chỉ có một bóng đèn được xác định trực tiếp là BẬT và bóng đèn còn lại là NÓNG hoặc LẠNH. Việc triển khai đơn giản giả định thứ tự bóng đèn cố định hoặc giả sử bóng đèn nào được báo cáo là nóng có thể gán sai vai trò. 

Ví dụ: nếu đầu vào báo cáo bóng đèn 2 là BẬT và bóng đèn 3 là COOL, thì bóng đèn 1 phải ngầm là HOT, mặc dù nó không được gắn nhãn rõ ràng trong đầu vào. Một giải pháp bất cẩn chỉ sử dụng các bóng đèn được đề cập rõ ràng và bỏ qua suy luận về trạng thái bị thiếu sẽ thất bại. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử tất cả 6 hoán vị của bài tập từ công tắc sang bóng đèn. Đối với mỗi hoán vị, chúng tôi sẽ mô phỏng các hành động được mô tả: hai công tắc được bật, một công tắc tắt và sau đó xác định trạng thái bóng đèn. Sau đó chúng tôi sẽ so sánh các trạng thái kết quả với các dòng đầu ra được quan sát. Vì chỉ có 6 hoán vị nên cách tiếp cận này rất nhanh và chính xác. Ngay cả khi được mở rộng sang nhiều công tắc hơn, mô phỏng trên mỗi hoán vị sẽ tiêu tốn thời gian tuyến tính tính theo số lượng hành động, ở đây là không đổi. 

Tuy nhiên, vấn đề này hoàn toàn không yêu cầu mô phỏng. Bản ghi trực tiếp mã hóa logic phân loại cuối cùng được sử dụng trong câu đố: 

Một công tắc là công tắc “vẫn BẬT”, một công tắc là công tắc “NÓNG nhưng TẮT” và một công tắc là công tắc “không bao giờ chạm vào”. Mỗi danh mục ánh xạ duy nhất tới một loại bóng đèn: bóng đèn BẬT, bóng đèn NÓNG và bóng đèn COOL. 

Do đó, vấn đề giảm xuống còn việc xác định ba loại từ bản ghi và gán chúng cho các ô 1, 2 và 3. 

Chúng tôi chỉ đơn giản phân tích: 

công tắc BẬT ở cuối, 

công tắc đã TẮT sau khi bật, 

và công tắc còn lại. 

Sau đó chúng tôi phân tích: 

bóng đèn đang BẬT, 

bóng đèn NÓNG hoặc LẠNH và suy ra bóng đèn thứ ba còn thiếu. 

Điều này mang lại sự kết hợp trực tiếp giữa công tắc và bóng đèn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Khấu trừ trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích hai lệnh “BẬT” và ghi lại hai công tắc liên quan. Đây là những công tắc duy nhất có thể nằm trong danh mục BẬT hoặc BẬT gần đây. 
2. Phân tích lệnh “TURN OFF”. Công tắc được đề cập ở đây là công tắc được
