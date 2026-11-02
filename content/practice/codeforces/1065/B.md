---
title: "CF 1065B - Vasya và các đỉnh cô lập"
description: "Chúng ta có một đồ thị vô hướng đơn giản với số đỉnh và số cạnh cố định. Đồ thị không có vòng tự lặp và không có cạnh trùng lặp."
date: "2026-06-15T08:19:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1065
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 52 (Rated for Div. 2)"
rating: 1300
weight: 1065
solve_time_s: 351
verified: false
draft: false
---

[CF 1065B - Vasya và các đỉnh cô lập](https://codeforces.com/problemset/problem/1065/B) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, đồ thị 
**Thời gian giải:** 5 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng đơn giản với số đỉnh và số cạnh cố định. Đồ thị không có vòng tự lặp và không có cạnh trùng lặp. Cấu trúc không bị hạn chế, có nghĩa là chúng ta có thể tự do tưởng tượng bất kỳ cấu hình hợp lệ nào sử dụng chính xác$m$cạnh giữa$n$các đỉnh được dán nhãn. 

Một đỉnh được gọi là cô lập nếu nó không kề với bất kỳ cạnh nào. Nhiệm vụ không phải là xây dựng một biểu đồ mà là suy luận về các điểm cực trị: trong số tất cả các biểu đồ hợp lệ với$n$đỉnh và$m$các cạnh, chúng ta muốn số đỉnh cô lập nhỏ nhất có thể và số đỉnh cô lập lớn nhất có thể. 

Điểm căng thẳng chính là giữa cách các cạnh “tiêu thụ” các đỉnh và mức độ chúng ta có thể tập trung các cạnh bên trong các tập hợp con nhỏ của các đỉnh một cách tự do như thế nào. 

Những ràng buộc cho phép$n$lên tới$10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng việc xây dựng biểu đồ hoặc liệt kê các cạnh. Câu trả lời phải đến từ lý luận mang tính cấu trúc về cách các cạnh có thể được phân bổ. 

Một sự hiểu lầm ngây thơ thường xuất phát từ việc nghĩ rằng các cạnh luôn “sử dụng hết” hai đỉnh mới. Điều đó là sai vì các cạnh có thể chia sẻ các đỉnh và trên thực tế, việc chia sẻ là nguyên nhân làm giảm số lượng đỉnh bị cô lập tối thiểu. 

Trường hợp cạnh tinh tế xuất hiện khi$m = 0$. Trong trường hợp đó, mọi đỉnh đều bị cô lập nên cả cực tiểu và cực đại đều bằng nhau$n$. Khác là khi$m$đủ lớn để chúng ta có thể “chạm” vào hầu hết các đỉnh ngay cả khi chúng ta cố gắng tránh sự cô lập, nhưng vẫn không đủ lớn để đảm bảo khả năng kết nối. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là tưởng tượng việc xây dựng các biểu đồ và đếm các đỉnh biệt lập cho mỗi cấu hình. Đối với mỗi tập con của các cạnh, người ta có thể tính xem có bao nhiêu đỉnh xuất hiện trong ít nhất một cạnh, sau đó suy ra các đỉnh cô lập. Điều này ngay lập tức bùng nổ vì số lượng đồ thị tăng theo cấp số nhân với$m$Và$n$, làm cho việc liệt kê không thể thực hiện được ngay cả đối với các đầu vào nhỏ. 

Quan điểm đúng là đảo ngược câu hỏi. Thay vì đặt các cạnh một cách tùy ý, chúng ta hỏi có bao nhiêu đỉnh có thể được tạo thành không cô lập bằng cách sử dụng$m$các cạnh và bao nhiêu đỉnh chúng ta có thể tránh chạm vào. 

Để có số đỉnh cô lập tối đa, chúng ta muốn tập trung các cạnh càng nhiều càng tốt vào trong tập đỉnh nhỏ nhất có thể. Nếu chúng ta chọn$k$đỉnh và đặt tất cả$m$các cạnh bên trong chúng, sau đó các cạnh còn lại$n-k$đỉnh bị cô lập. Nhỏ nhất$k$có thể lưu trữ$m$cạnh là số nguyên nhỏ nhất sao cho:$$\binom{k}{2} \ge m$$Nếu như$m = 0$, sau đó$k = 0$. Nếu không thì,$k$phải có ít nhất 2. 

Vậy đỉnh cô lập lớn nhất là$n - k$, Ở đâu$k$là kích thước tối thiểu của tập đỉnh có thể hỗ trợ$m$các cạnh. 

Để có số đỉnh cô lập tối thiểu, chúng ta muốn trải rộng các cạnh sao cho chúng chạm vào càng nhiều đỉnh phân biệt càng tốt. Mỗi cạnh có thể giới thiệu tối đa hai đỉnh mới, nhưng một khi các đỉnh được sử dụng lại thì mức tăng cận biên sẽ giảm. Chiến lược tối ưu là tối đa hóa số đỉnh được bao phủ bởi các cạnh, được giới hạn bởi$2m$, nhưng không thể vượt quá$n$. Vậy số đỉnh cô lập tối thiểu là:$$\max(0, n - 2m)$$Giới hạn này có thể đạt được bằng cách xây dựng một cấu trúc giống như khớp khi có thể và sử dụng lại các đỉnh để bão hòa vùng phủ sóng. 

Hai thái cực này độc lập với nhau: một thái cực gói chặt các cạnh, thái cực kia trải chúng ra càng rộng càng tốt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê biểu đồ) | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (lý luận tổ hợp) |$O(\sqrt{n})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số đỉnh cô lập tối thiểu bằng cách ước tính có bao nhiêu đỉnh có thể được “che phủ” bởi các cạnh. Mỗi cạnh có thể đóng góp tối đa hai đỉnh mới, do đó số đỉnh được che phủ nhiều nhất là$2m$, giới hạn bởi$n$. Số bị cô lập là$n - \min(n, 2m)$, điều này đơn giản hóa thành$\max(0, n - 2m)$. 
2. Tính số đỉnh cô lập tối đa bằng cách tìm số đỉnh nhỏ nhất cần thiết để chứa tất cả các cạnh. Bắt đầu với$k = 0$, và tăng nó cho đến khi$\binom{k}{2} \ge m$. Cái này$k$đại diện cho tập đỉnh nhỏ nhất có thể lưu trữ tất cả các cạnh. 
3. Phần còn lại$n - k$các đỉnh có thể bị cô lập hoàn toàn vì không có cạnh nào buộc phải chạm vào chúng trong một cấu trúc tối ưu. 
4. Xuất ra cặp: đầu tiên là các đỉnh cô lập tối thiểu, sau đó là các đỉnh cô lập tối đa. 

Ý tưởng chính là cả hai phép tính đều là các gói cực trị độc lập có cùng quỹ biên. 

### Tại sao nó hoạt động 

Số đỉnh được các cạnh chạm vào là yếu tố kiểm soát sự cô lập. Đối với trường hợp tối thiểu, chúng tôi tối đa hóa phạm vi bao phủ và mỗi cạnh đóng góp tối đa hai đỉnh riêng biệt, tạo ra giới hạn trên chặt chẽ. Đối với trường hợp tối đa, chúng tôi giảm thiểu phạm vi bao phủ bằng cách tập trung các cạnh vào một đồ thị con dày đặc và dung lượng của một$k$-đồ thị đơn giản có đỉnh chính xác$\binom{k}{2}$. Hai đối số đóng gói đối lập này mô tả đầy đủ tất cả các cấu hình khả thi. 

## Giải pháp Python```
PythonRun
```Mã phân tách hai phép tính cực trị. Phần đầu áp dụng trực tiếp nhận xét rằng mỗi cạnh có thể bao phủ nhiều nhất hai đỉnh mới, do đó ta trừ đi$2m$từ$n$. Phần thứ hai tìm kiếm nhỏ nhất$k$sao cho một đồ thị hoàn chỉnh trên$k$các đỉnh có đủ cạnh để chứa tất cả$m$các cạnh. Vòng lặp an toàn vì$k$không bao giờ vượt quá khoảng$\sqrt{2m}$, nằm trong giới hạn cho$n \le 10^5$. 

Một điểm tinh tế là việc xử lý$m = 0$. Không có nó, điều kiện tam giác sẽ buộc không chính xác$k = 1$, nhưng cách giải thích đúng là không cần có đỉnh nào để chứa các cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Đối với các đỉnh bị cô lập tối thiểu, chúng tôi tính toán$n - 2m = 4 - 4 = 0$. Điều này tương ứng với việc sử dụng cả hai cạnh để bao phủ cả bốn đỉnh, ví dụ như hai cạnh rời nhau. 

Đối với các đỉnh cô lập lớn nhất, chúng ta tìm giá trị nhỏ nhất$k$như vậy$\binom{k}{2} \ge 2$. Chúng tôi kiểm tra$k=2 \rightarrow 1$, không đủ;$k=3 \rightarrow 3$, đủ. Vì thế$k=3$và các đỉnh cô lập tối đa là$4 - 3 = 1$. 

| Bước | k | k(k−1)/2 | Tình trạng | 
| --- | --- | --- | --- | 
| 2 | 2 | 1 | < 2 | 
| 3 | 3 | 3 | ≥ 2 | 

Điều này xác nhận rằng hai cạnh có thể được gói thành ba đỉnh trong khi vẫn để lại một đỉnh bị cô lập. 

### Ví dụ 2 

đầu vào:```

```Các đỉnh cô lập tối thiểu là$5 - 0 = 5$. Các đỉnh cô lập tối đa cũng là$5$, từ$k = 0$. Đồ thị không có cạnh nên mỗi đỉnh đều bị cô lập bất kể cách xây dựng. 

| Bước | k | k(k−1)/2 | Tình trạng | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | ≥ 0 | 

Điều này cho thấy trường hợp không có cạnh thu gọn cả hai thái cực về cùng một giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{m})$| Việc tìm kiếm số tam giác phát triển cho đến khi đạt$m$, yêu cầu nhiều nhất$O(\sqrt{m})$lặp đi lặp lại | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Các ràng buộc cho phép lên đến$10^5$đỉnh và về$10^{10}$các cạnh có thể có, nhưng chỉ có một cặp duy nhất$(n, m)$được xử lý. Vòng lặp vẫn nhanh vì yêu cầu$k$được giới hạn bởi khoảng$\sqrt{2m}$, lớn nhất là xung quanh$10^5$trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 1 | hành vi không có cạnh của một đỉnh | 
| 5 0 | 5 5 | đồ thị không có cạnh cách ly hoàn toàn | 
| 4 6 | 0 0 | trường hợp đồ thị hoàn chỉnh | 
| 6 3 | 0 3 | hành vi đóng gói trung gian | 

## Vỏ cạnh 

cho$n = 1, m = 0$, thuật toán đặt các đỉnh cô lập tối thiểu thành$1 - 0 = 1$. Tối đa,$k = 0$, vậy kết quả là$1$. Điều này phù hợp với thực tế là một đỉnh không có cạnh nào phải được cô lập. 

Vì$m = 0$với kích thước lớn$n$, công thức mang lại$min = n$Và$max = n$, phản ánh rằng không có công trình xây dựng nào có thể làm giảm sự cô lập. 

Đối với rất lớn$m$, gần với$\binom{n}{2}$, tìm kiếm tam giác tìm thấy$k = n$, do đó các đỉnh cô lập tối đa trở thành$0$. Mức tối thiểu cũng là$0$bởi vì tất cả các đỉnh phải được sử dụng trong đồ thị dày đặc.
