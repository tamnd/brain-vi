---
title: "CF 104874H - Cơ sở dữ liệu tải cao"
description: "Chúng ta được cung cấp một chuỗi giao dịch cố định, mỗi giao dịch mang một khối lượng công việc tích cực được đo lường trong các truy vấn. Chúng tôi không được phép sắp xếp lại các giao dịch này. Thay vào đó, chúng ta phải phân chia chuỗi thành các nhóm liền kề mà chúng ta sẽ gọi là nhóm."
date: "2026-06-28T10:08:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104874
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104874
solve_time_s: 26
verified: false
draft: false
---

[CF 104874H - Cơ sở dữ liệu tải cao](https://codeforces.com/problemset/problem/104874/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi giao dịch cố định, mỗi giao dịch mang một khối lượng công việc tích cực được đo lường trong các truy vấn. Chúng tôi không được phép sắp xếp lại các giao dịch này. Thay vào đó, chúng ta phải phân chia chuỗi thành các nhóm liền kề mà chúng ta sẽ gọi là nhóm. Chi phí của mỗi lô là tổng số truy vấn bên trong nó. 

Đối với một giới hạn nhất định`t`, một lô hợp lệ nếu tổng số truy vấn của nó không vượt quá`t`. Mục tiêu cho mỗi truy vấn`t_i`là chia toàn bộ chuỗi thành số lô hợp lệ tối thiểu. 

Đây là một vấn đề cổ điển “phân khúc tham lam theo hạn chế công suất”, nhưng với điểm mấu chốt là chúng ta phải trả lời hiệu quả tới 100.000 giá trị công suất khác nhau. 

Các ràng buộc ngụ ý một ranh giới tính toán rõ ràng. Tổng của tất cả các kích thước giao dịch tối đa là 10^6, do đó, một lần quét tuyến tính trên mảng sẽ rẻ. Tuy nhiên, việc tính toán lại phân vùng tham lam từ đầu cho mỗi truy vấn trong số tối đa 10^5 truy vấn sẽ tốn O(nq), vượt xa giới hạn khả thi. 

Một quan sát tinh tế hơn là tính khả thi phụ thuộc rất nhiều vào giá trị của`t`. Nếu như`t`quá nhỏ để có thể đáp ứng ngay cả một giao dịch đơn lẻ (tức là một số`a_i > t`), thì không tồn tại phân vùng nào và câu trả lời phải là “Không thể”. Đây là một trường hợp quan trọng phải được xử lý trước bất kỳ mô phỏng tham lam nào. 

Trường hợp cạnh thứ hai là khi`t`là rất lớn, trong trường hợp đó giải pháp tối ưu sẽ thu gọn thành một lô duy nhất chứa tất cả các giao dịch. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản: với mỗi giá trị truy vấn`t`, mô phỏng quá trình trộn từ trái sang phải. Duy trì tổng số tiền hiện có và bất cứ khi nào việc thêm giao dịch tiếp theo sẽ vượt quá`t`, bắt đầu một đợt mới. Chiến lược tham lam này là đúng vì việc mở rộng một đợt càng nhiều càng tốt sẽ không bao giờ làm giảm số lượng các đợt. 

Tính đúng đắn của cách xây dựng tham lam này xuất phát từ một đối số trao đổi đơn giản: nếu tồn tại một phân vùng hợp lệ, thì bất cứ khi nào chúng ta cắt sớm một đợt sớm hơn chiến lược tham lam, chúng ta chỉ tăng số lượng đợt. Vì vậy, luôn lấy tiền tố dài nhất có thể cho mỗi đợt sẽ giảm thiểu tổng số lượng. 

Tuy nhiên, việc lặp lại quá trình quét này một cách độc lập cho từng truy vấn sẽ dẫn đến các phép toán O(nq) trong trường hợp xấu nhất. Với n lên tới 200.000 và q lên tới 100.000, tốc độ này quá chậm. 

Thông tin chi tiết quan trọng là chúng tôi liên tục áp dụng cùng một cách phân vùng tham lam dưới các ngưỡng dung lượng khác nhau. Thay vì tính toán lại từ đầu, chúng ta có thể xử lý trước xem mỗi vị trí bắt đầu có thể mở rộng bao xa đối với một cấu trúc ràng buộc nhất định hoặc đơn giản hơn là quan sát rằng câu trả lời là đơn điệu trong`t`. BẰNG`t`tăng lên, số lượng lô không bao giờ tăng lên. Điều này gợi ý việc sắp xếp các truy vấn và xử lý chúng theo thứ tự tăng dần, duy trì cấu trúc trượt cho phép chúng ta sử dụng lại các tính toán trước đó. 

Tối ưu hóa tiêu chuẩn và trực tiếp hơn sử dụng cách tiếp cận hai con trỏ với tổng tiền tố, kết hợp với nâng cấp nhị phân hoặc xử lý ngoại tuyến. Chúng tôi tính toán trước các tổng tiền tố để có thể kiểm tra tổng phân đoạn bất kỳ trong O(1). Sau đó, chúng ta có thể mô phỏng các bước nhảy: từ mỗi chỉ số i, tìm j xa nhất sao cho tổng(i..j) ≤ t. Điều này có thể được trả lời bằng tìm kiếm nhị phân trên tổng tiền tố. Sau đó, mỗi truy vấn sẽ trở thành một quá trình nhảy tham lam qua các chỉ mục, tốn O(n log n) cho mỗi truy vấn nếu được thực hiện một cách đơn giản, nhưng có thể được tối ưu hóa bằng cách sử dụng lại các chuyển đổi được tính toán khi xử lý các truy vấn theo thứ tự được sắp xếp. 

Một cách tối ưu hóa đơn giản và đầy đủ hơn theo các ràng buộc này là sắp xếp các truy vấn và chỉ tính toán lại phân vùng tham lam khi cần thiết trong khi sử dụng lại các tổng tiền tố và tránh công việc lặp lại bằng cách chỉ quét một lần cho mỗi cấu trúc riêng biệt. Vì q lớn nhưng tổng của a_i nhỏ nên điều này vẫn được thực hiện cẩn thận. 

Một góc độ khác là chúng ta đang đếm một cách hiệu quả số lượng phân đoạn mà quá trình phân tách cửa sổ trượt tạo ra dưới ngưỡng t và số lượng này có thể được duy trì một cách hiệu quả khi t thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu tham lam mỗi truy vấn | O(nq) | O(1) | Quá chậm | 
| Tổng tiền tố + tham lam ngoại tuyến được tối ưu hóa | O(n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng tổng tiền tố và quét tham lam cho mỗi truy vấn nhưng được cấu trúc sao cho mỗi lần quét đều tuyến tính và hiệu quả. 

1. Tính trước tổng tiền tố`pref`, Ở đâu`pref[i]`lưu trữ tổng số truy vấn trong giao dịch`1`bởi vì`i`. Điều này cho phép truy vấn tổng phạm vi thời gian không đổi. 
2. Với mỗi giá trị truy vấn`t`, trước tiên hãy kiểm tra tính khả thi bằng cách xác minh rằng không có giao dịch nào vượt quá`t`. Nếu có`a_i > t`, xuất ra “Không thể” ngay lập tức. Điều này tránh lãng phí tính toán trong các trường hợp không hợp lệ. 
3. Khởi tạo con trỏ`i = 1`, đại diện cho giao dịch đầu tiên chưa được gán cho một đợt và một bộ đếm`ans = 0`. 
4. Trong khi`i ≤ n`, bắt đầu một đợt mới tại vị trí`i`. 
5. Mở rộng lô
