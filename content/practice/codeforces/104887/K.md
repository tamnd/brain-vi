---
title: "CF 104887K - Lý thuyết Kyuuing"
description: "Chúng tôi được sắp xếp một thứ tự cố định các học sinh trong hàng đợi. Mỗi học sinh cần một khoảng thời gian cố định để hoàn thành bài kiểm tra của mình và có $k$ người hướng dẫn có thể xử lý song song các học sinh."
date: "2026-06-28T09:03:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104887
codeforces_index: "K"
codeforces_contest_name: "2023 Abakoda Long Contest"
rating: 0
weight: 104887
solve_time_s: 28
verified: false
draft: false
---

[CF 104887K - Lý thuyết Kyuuing](https://codeforces.com/problemset/problem/104887/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được sắp xếp một thứ tự cố định các học sinh trong hàng đợi. Mỗi học sinh cần một khoảng thời gian cố định để hoàn thành bài kiểm tra của mình và có$k$người hướng dẫn có thể xử lý học sinh song song. Một học sinh luôn cố gắng bắt đầu ngay lập tức: nếu người hướng dẫn rảnh khi họ đến phía trước, họ sẽ bắt đầu; nếu không họ sẽ đợi cho đến khi người hướng dẫn nào đó kết thúc. 

Điều quan trọng là thứ tự xếp hàng không bao giờ thay đổi và không học sinh nào có thể vượt qua học sinh khác. Điều này làm cho hệ thống tương đương với một luồng công việc thông qua$k$máy giống hệt nhau với một ràng buộc thứ tự đến nghiêm ngặt. Sự tự do duy nhất chúng ta có là lựa chọn$k$, số lượng máy chủ song song. 

Đối với một cố định$k$, quá trình này sẽ kết thúc trong thời gian$T$hoặc nó không. Vấn đề yêu cầu hai điều cho mỗi trường hợp kiểm thử: trước tiên hãy xác định xem liệu có thể hoàn thành trong vòng$T$, và nếu có thì tìm giá trị nhỏ nhất$k$điều đó làm cho nó có thể. 

Các ràng buộc rất lớn: lên tới$n = 150{,}000$mỗi trường hợp thử nghiệm và tổng số$N \le 450{,}000$. Mỗi$a_i$có thể lớn như$10^{16}$. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào cố gắng mô hình hóa thời gian theo từng bước. Ngay cả việc mô phỏng cho mỗi người hướng dẫn cho mỗi sự kiện cũng sẽ quá chậm vì các sự kiện có thể$O(n \log n)$hoặc tệ hơn, và chúng ta có thể cần phải kiểm tra nhiều giá trị của$k$. 

Giải pháp phải đánh giá tính khả thi cho một$k$trong thời gian tuyến tính hoặc gần tuyến tính, sau đó tìm giá trị nhỏ nhất$k$, thường sử dụng tìm kiếm nhị phân. 

Trường hợp cạnh tinh tế xuất hiện khi một trường hợp rất lớn$a_i$vượt quá$T$. Trong trường hợp đó không có cấu hình nào hoạt động bất kể$k$, bởi vì một học sinh không thể hoàn thành kịp thời gian ngay cả với vô số người hướng dẫn. Một trường hợp cạnh khác là khi$T$nhỏ nhưng$n$lớn; Những giả định tham lam ngây thơ như “cứ tiếp tục chỉ định người hướng dẫn miễn phí tiếp theo” sẽ thất bại trừ khi chúng ta lập mô hình chính xác về thời gian hoàn thành. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ cố gắng tăng$k$từ 1 trở lên và mô phỏng toàn bộ quá trình mỗi lần. Đối với một cố định$k$, chúng tôi sẽ duy trì thời gian hoàn thành của tất cả các giáo viên và chỉ định mỗi học sinh vào người có thời gian sớm nhất. Một đống tối thiểu sẽ tự nhiên mô hình hóa điều này. 

Đối với mỗi trường hợp thử nghiệm, mô phỏng một giá trị của$k$mất$O(n \log k)$, vì chúng ta đẩy và bật từ một đống cho mỗi học sinh. Nếu chúng ta thử tất cả$k$lên đến$n$, điều này trở thành$O(n^2 \log n)$, tốc độ này quá chậm$n = 150{,}000$. 

Cái nhìn sâu sắc quan trọng là tính khả thi là đơn điệu trong$k$. Nếu chúng ta có thể hoàn thành trong vòng$T$sử dụng$k$người hướng dẫn, thì chúng ta cũng có thể sử dụng xong$k+1$người hướng dẫn, vì việc bổ sung năng lực không bao giờ gây hại. Điều này biến vấn đề thành một cuộc tìm kiếm$k$. 

Chúng tôi vẫn cần kiểm tra tính khả thi hiệu quả. Việc đơn giản hóa quan trọng là tránh lập mô hình hàng đợi một cách rõ ràng mà thay vào đó chỉ mô phỏng thời gian sẵn sàng của người hướng dẫn. Mỗi người hướng dẫn duy trì thời gian nó trở nên miễn phí. Đối với mỗi học sinh theo thứ tự, chúng tôi phân công cho người hướng dẫn nào rảnh sớm nhất. Nếu thời gian rảnh sớm nhất đó lớn hơn thời gian bắt đầu hiện tại thì họ phải đợi; nếu không họ sẽ bắt đầu ngay lập tức vào thời điểm hiện tại. Thời gian kết thúc sẽ cập nhật tương ứng. 

Đây chính xác là một quy trình lập kế hoạch tham lam trên các máy giống hệt nhau với thời gian giải phóng bằng với thời điểm học sinh đến trước, theo trình tự tự nhiên. 

Do đó, chúng tôi kết hợp hai ý tưởng: kiểm tra tính khả thi của heap tối thiểu và tìm kiếm nhị phân trên$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hãy thử tất cả$k$+ mô phỏng |$O(n^2 \log n)$|$O(n)$| Quá chậm | 
| Tìm kiếm nhị phân + mô phỏng đống |$O(n \log n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Kiểm tra tính khả thi của bản sửa lỗi$k$1. Khởi tạo một vùng heap tối thiểu với$k$số không, thể hiện rằng tất cả người hướng dẫn ban đầu đều rảnh tại thời điểm 0. Mô hình này giống hệt tính khả dụng khi bắt đầu. 
2. Xử lý học sinh theo thứ tự xếp hàng. Dành cho sinh viên$i$, trích xuất người hướng dẫn với thời gian có sẵn nhỏ nhất$t$. Người hướng dẫn này rảnh rỗi sớm nhất nên họ là ứng viên duy nhất có thể bắt đầu ngay lập tức. 
3. Học sinh có thể bắt đầu vào thời gian$\max(t, 0)$, nhưng vì chúng tôi theo dõi tiến trình thời gian tuyệt đối nên chúng tôi hiểu điều này là bắt đầu tại thời điểm$t$. Thời gian kết thúc của họ trở thành$t + a_i$. 
4. Đẩy thời gian hoàn thành đã cập nhật này trở lại vùng lưu trữ vì người hướng dẫn đó hiện đang bận cho đến thời điểm đó. 
5. Theo dõi thời gian hoàn thành tối đa của tất cả học sinh. Nếu tại bất kỳ điểm nào nó vượt quá$T$, chúng ta có thể dừng lại sớm để đạt hiệu quả vì lịch trình không thể sửa chữa được bằng những nhiệm vụ trong tương lai. 
6. Sau khi tất cả học sinh được xử lý, hãy trả lại xem thời gian hoàn thành tối đa có nhiều nhất không$T$. 

### Tìm kiếm giá trị tối thiểu$k$1. Nếu có đơn$a_i > T$, ngay lập tức xuất ra NO, vì một mình học sinh đó không thể hoàn thành kịp thời gian. 
2. Nếu không thì tìm kiếm nhị phân$k$từ 1 đến$n$. Đối với mỗi điểm giữa, hãy chạy kiểm tra tính khả thi. 
3. Nhỏ nhất$k$trả về true là câu trả lời. 

### Tại sao nó hoạt động 

Tính bất biến của heap là tại bất kỳ thời điểm nào nó đều chứa thời gian hoàn thành hiện tại của mọi người hướng dẫn. Mỗi bài tập luôn sử dụng người hướng dẫn sẵn sàng
