---
problem: 925F
contest_id: 925
problem_index: F
name: "Parametric Circulation"
contest_name: "VK Cup 2018 - Round 3"
rating: 3100
tags: ["binary search", "flows"]
answer: passed_samples
verified: false
solve_time_s: 37
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 0
---

#CF 925F - Tuần hoàn tham số 

**Đánh giá:** 3100 
**Thẻ:** tìm kiếm nhị phân, luồng 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 37s 
**Đã xác minh:** không (0/0 mẫu) 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trong đó mỗi cạnh có thể mang một luồng không âm và các luồng phải thỏa mãn sự bảo toàn tiêu chuẩn: tại mọi đỉnh, tổng luồng đi vào bằng tổng luồng đi ra. Đây là vấn đề tuần hoàn không có nguồn bên ngoài hoặc nguồn chìm. 

Điều khó khăn là mỗi cạnh không chỉ có giới hạn cố định trên luồng của nó. Thay vào đó, khoảng cho phép cho mỗi cạnh phụ thuộc tuyến tính vào tham số chung$t \in [0,1]$. Đối với mỗi cạnh$e$, dòng chảy phải thỏa mãn$$l_e(t) \le f_e \le r_e(t),$$trong đó cả hai điểm cuối của khoảng di chuyển tuyến tính như$t$những thay đổi. 

Chúng tôi chọn$t$thống nhất ngẫu nhiên từ$[0,1]$và chúng ta cần xác suất tồn tại một tuần hoàn hợp lệ cho tập hợp kết quả. 

Vì vậy, nhiệm vụ mang tính hình học được ngụy trang: trong số tất cả$t \in [0,1]$, chúng tôi muốn đo tập hợp con các giá trị mà vùng khả thi của một vòng tuần hoàn giới hạn không trống. 

Các ràng buộc gợi ý rằng một mô phỏng trực tiếp trên tất cả$t$là không thể. Ngay cả khi chúng ta rời rạc hóa$t$, kiểm tra tính khả thi cho một giải pháp cố định$t$đã yêu cầu giải quyết vấn đề về tính khả thi của lưu thông, thường thông qua luồng tối đa với giới hạn thấp hơn. Với tối đa$m = 2000$, làm nhiều lần thì chậm quá. Vì vậy, cấu trúc của tính khả thi thay đổi như thế nào với$t$phải được khai thác. 

Một trường hợp biên khó phát sinh từ thực tế là tính khả thi phụ thuộc vào các ràng buộc cân bằng toàn cầu, không chỉ ở các biên riêng lẻ. Một cách tiếp cận đơn giản có thể kiểm tra từng cạnh một cách độc lập hoặc cho rằng tính khả thi là đơn điệu trong$t$, nhưng không phải là sự thật. Một đồ thị nhỏ có thể dễ dàng có giá trị khả thi trên hai khoảng cách nhau của$t$. 

## Phương pháp tiếp cận 

Đối với một giá trị cố định của$t$, chúng ta có thể chuyển đổi bài toán tuần hoàn với giới hạn dưới và giới hạn trên thành bài toán tính khả thi của luồng tiêu chuẩn. Chúng tôi trừ các giới hạn dưới, điều chỉnh nhu cầu nút và sau đó kiểm tra xem liệu có tồn tại một luồng khả thi hay không bằng cách sử dụng cấu trúc siêu nguồn và siêu chìm. Đây là tiêu chuẩn. 

Một ý tưởng mạnh mẽ sẽ là lấy mẫu nhiều giá trị của$t$, tính toán tất cả các giới hạn và chạy kiểm tra tính khả thi của luồng tối đa cho từng mẫu. Mỗi lần kiểm tra có giá khoảng$O(F(m,n))$, tiêu biểu$O(m \sqrt n)$hoặc tệ hơn tùy thuộc vào việc thực hiện. Với thậm chí vài nghìn mẫu, việc này trở nên quá chậm. 

Quan sát quan trọng là tính khả thi không thay đổi tùy ý theo$t$. Mọi ràng buộc trong công thức chuyển đổi dòng chảy đều tuyến tính trong$t$. Sau khi chuyển đổi sang điều kiện khả thi, chúng tôi kết thúc với một hệ thống tương đương với việc kiểm tra xem mức cắt giảm tối thiểu nhất định có mất cân bằng công suất không dương hay không. Mỗi công suất cắt là một hàm tuyến tính của$t$, do đó tính khả thi giảm xuống một họ bất đẳng thức tuyến tính trong$t$. 

Điều này biến vấn đề thành việc tìm kiếm tất cả$t \in [0,1]$trong đó có một tập hợp các bất đẳng thức tuyến tính. Cấu trúc quan trọng là điều kiện khả thi giảm đến mức tối thiểu trên tất cả các lần cắt của hàm tuyến tính trong$t$. Vì vậy, chúng tôi đang nghiên cứu một cách hiệu quả một chức năng$$F(t) = \min_{\text{cuts } S} (A_S t + B_S),$$và chúng ta cần tất cả$t$như vậy$F(t) \ge 0$. 

Mỗi lần cắt xác định một đường trong$t$, và mức tối thiểu trên chúng là đường bao bên dưới của các dòng. Vùng khả thi là nơi đường bao này không âm. Đây là một cấu trúc cắt nhỏ tham số cổ điển. 

Chúng tôi tính toán cri
