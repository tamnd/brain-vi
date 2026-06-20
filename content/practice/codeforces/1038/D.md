---
title: "CF 1038D - Chất Nhờn"
description: "Chúng ta được cung cấp một dòng chất nhờn, mỗi dòng mang một giá trị số nguyên và chúng ta liên tục thực hiện một thao tác trong đó một chất nhờn hấp thụ chất nhờn liền kề. Nếu một chất nhờn có giá trị $x$ ăn thịt một người hàng xóm có giá trị $y$, người hàng xóm đó sẽ biến mất và giá trị của người ăn trở thành $x - y$."
date: "2026-06-16T18:30:25+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1038
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 508 (Div. 2)"
rating: 1800
weight: 1038
solve_time_s: 107
verified: false
draft: false
---

[CF 1038D - Chất nhờn](https://codeforces.com/problemset/problem/1038/D) 

**Đánh giá:** 1800 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng chất nhờn, mỗi dòng mang một giá trị số nguyên và chúng ta liên tục thực hiện một thao tác trong đó một chất nhờn hấp thụ chất nhờn liền kề. Nếu một chất nhờn có giá trị$x$ăn thịt hàng xóm có giá trị$y$, người hàng xóm đó biến mất và giá trị của người ăn trở thành$x - y$. Thao tác này có thể được áp dụng nhiều lần cho đến khi chỉ còn lại một chất nhờn duy nhất. 

Thứ tự hấp thụ này hoàn toàn linh hoạt miễn là chúng ta luôn chọn những slime liền kề. Các thứ tự khác nhau dẫn đến các giá trị cuối cùng khác nhau, bởi vì phép trừ phụ thuộc vào giá trị nào cuối cùng được trừ khỏi bộ tích lũy nào. 

Nhiệm vụ là chọn thứ tự hợp nhất để tối đa hóa giá trị còn lại cuối cùng. 

Ràng buộc$n \le 5 \cdot 10^5$ngay lập tức loại trừ mọi cách tiếp cận mô phỏng tất cả các chuỗi hợp nhất có thể có hoặc thậm chí lập trình động trên tất cả các phân đoạn. Mọi hành vi bậc hai hoặc bậc ba đều quá lớn; thậm chí$O(n \log n)$có thể chấp nhận được, trong khi mọi thứ tuyến tính hoặc logarit tuyến tính đều được yêu cầu. 

Một khó khăn tinh tế xuất phát từ thực tế là các giá trị có thể âm. Điều này khiến cho trực giác tham lam kiểu “luôn ăn thịt người hàng xóm lớn nhất” không đáng tin cậy. Ví dụ: nếu chất nhờn ăn giá trị âm, nó sẽ tăng lên, trong khi ăn giá trị dương sẽ giảm giá trị. Sự tương tác dấu hiệu này là nguồn gốc cốt lõi của tính không tầm thường. 

Một sai lầm ngây thơ là cho rằng chiến lược tốt nhất luôn là tích lũy mọi thứ vào yếu tố đầu tiên hoặc cuối cùng. Điều đó không thành công vì các lệnh hợp nhất trung gian có thể lật ngược các dấu hiệu đóng góp. 

Một ý tưởng sai lầm phổ biến khác là nghĩ rằng câu trả lời chỉ đơn giản là tổng xen kẽ hoặc thứ gì đó dựa trên tính chẵn lẻ. Điều đó bỏ qua việc chúng ta kiểm soát thứ tự các phép trừ chứ không chỉ là một biểu thức cố định. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi chuỗi hợp nhất liền kề có thể có. Mỗi trạng thái bao gồm một mảng hiện tại và mỗi bước di chuyển sẽ giảm độ dài của nó đi một. Số lượng cây hợp nhất nhị phân có thể có trên$n$các phần tử có bản chất là Catalan, gần như theo cấp số nhân$n$. Ngay cả đối với$n = 30$, điều này trở nên không thể thực hiện được. Mỗi quá trình chuyển đổi cũng yêu cầu sao chép hoặc cập nhật danh sách, khiến thời gian chạy vượt xa giới hạn. 

Quan sát quan trọng là mỗi phần tử cuối cùng sẽ bị trừ một số lần chẵn hoặc số lẻ từ một số bộ tích lũy cuối cùng tùy thuộc vào cấu trúc hợp nhất. Thay vì suy nghĩ theo trình tự các thao tác, chúng ta có thể diễn giải lại quy trình dưới dạng xây dựng một biểu thức trên mảng ban đầu trong đó mỗi giá trị xuất hiện với hệ số là một trong hai$+1$hoặc$-1$, được xác định bằng cách sắp xếp sự hợp nhất. 

Một cách có cấu trúc hơn để thấy điều đó là mọi cấu hình cuối cùng đều tương ứng với việc chọn hướng “lan truyền” ảnh hưởng và lối chơi tối ưu luôn giảm xuống việc kết hợp các đóng góp theo cách tối đa hóa kết quả ròng. Sự đơn giản hóa quan trọng là câu trả lời tối ưu chỉ phụ thuộc vào cấu trúc cực trị và chẵn lẻ toàn cục: chúng ta có thể tránh mô phỏng hoàn toàn việc hợp nhất và tính toán kết quả tốt nhất có thể đạt được bằng cách sử dụng một quá trình quét tham lam để theo dõi xem chúng ta có thể mở rộng một phân đoạn tích lũy bao xa và cách các dấu hiệu đảo ngược khi chúng ta hấp thụ các phần tử. 

Điều này làm giảm vấn đề từ việc khám phá không gian trạng thái theo cấp số nhân sang truyền tải tuyến tính với lượng trạng thái không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là xử lý mảng trong khi vẫn duy trì giá trị tốt nhất có thể đạt được của chất nhờn cuối cùng đang phát triển, xem xét rằng mỗi lần hấp thụ sẽ cộng hoặc trừ các khoản đóng góp trong tương lai tùy thuộc vào cách chúng tôi diễn giải hướng hợp nhất. 

Chúng tôi nhận thấy rằng kết quả cuối cùng có thể được xem là liên tục mở rộng một phân đoạn và quyết định xem phần tử mới đóng góp tích cực hay tiêu cực cho bộ tích lũy hiện tại, nhưng cấu trúc tối ưu luôn giảm xuống thành lựa chọn tham lam là ghép các phân đoạn nhỏ hơn trước và bảo toàn các phần tử có tác động cao. 

Sự đơn giản hóa chính xác cho vấn đề này là câu trả lời tối ưu thu được bằng cách lấy tối đa hai cấu trúc
