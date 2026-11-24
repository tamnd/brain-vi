---
title: "CF 915G - Mảng Coprime"
description: "Chúng ta được yêu cầu nghiên cứu các mảng có độ dài cố định trong đó mỗi phần tử được chọn từ một phạm vi giới hạn và phân loại chúng theo thuộc tính chung: liệu toàn bộ mảng có ước số chung lớn nhất bằng 1 hay không."
date: "2026-06-13T01:58:09+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 915
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 36 (Rated for Div. 2)"
rating: 2300
weight: 915
solve_time_s: 675
verified: false
draft: false
---

[CF 915G - Mảng Coprime](https://codeforces.com/problemset/problem/915/G) 

**Đánh giá:** 2300 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 11m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu nghiên cứu các mảng có độ dài cố định trong đó mỗi phần tử được chọn từ một phạm vi giới hạn và phân loại chúng theo thuộc tính chung: liệu toàn bộ mảng có ước số chung lớn nhất bằng 1 hay không. 

Đối với một giới hạn nhất định$i$, chúng tôi xem xét tất cả các mảng có độ dài$n$nơi mọi mục nằm giữa$1$Và$i$. Trong số các mảng này, chúng tôi đếm xem có bao nhiêu mảng có tổng gcd bằng một. Số này được ký hiệu$b_i$. Nhiệm vụ không phải là xuất ra tất cả$b_i$mà thay vào đó tính toán XOR tích lũy của tất cả chúng từ$i = 1$ĐẾN$k$. 

Những hạn chế đẩy chúng tôi vượt xa việc liệt kê vũ lực. Số lượng mảng cho một mảng$i$là$i^n$, và cả hai$n$Và$k$có thể lên tới$2 \cdot 10^6$. Ngay cả việc tính toán một$b_i$ngây thơ là không thể, vì$i^n$tăng theo cấp số nhân ở cả hai tham số. Bất kỳ giải pháp nào cũng phải giảm vấn đề về số học trên các ước số và tránh lặp lại hoàn toàn trên các mảng. 

Một chế độ thất bại tinh tế xuất hiện trong lý luận gcd ngây thơ. Người ta có thể thử tạo mảng hoặc suy luận cục bộ về các ràng buộc gcd, nhưng gcd là thuộc tính toàn cục phụ thuộc vào các thừa số nguyên tố được chia sẻ trên tất cả các phần tử. Ví dụ, đối với$n = 3$Và$i = 4$, mảng như$[2,4,2]$Và$[4,4,4]$hoạt động rất khác nhau theo gcd mặc dù chúng có chung cấu trúc cục bộ. Bất kỳ cách tiếp cận nào xử lý các phần tử một cách độc lập mà không theo dõi cấu trúc phân chia sẽ bị tính sai. 

Một cạm bẫy khác là quên rằng "không phải nguyên tố cùng nhau" có nghĩa là tất cả các phần tử đều có chung ước số lớn hơn một. Đây không phải là điều kiện theo cặp mà là ràng buộc về khả năng chia hết toàn cục và việc bỏ qua điều này sẽ dẫn đến lỗi tính kép hoặc bao gồm. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu từ định nghĩa. Đối với mỗi$i$, chúng tôi sẽ liệt kê tất cả$i^n$mảng và tính gcd cho mỗi mảng. Điều này đúng về mặt khái niệm vì gcd rất dễ đánh giá trên mỗi mảng nhưng không thể tính toán được. Ngay cả đối với$i = 10$Và$n = 10$, chúng ta đã phải đối mặt rồi$10^{10}$ứng viên. 

Sự thay đổi chính là đảo ngược điều kiện gcd. Thay vì đếm các mảng có gcd chính xác bằng 1, chúng ta đếm các mảng có gcd chia hết cho một số nào đó.$d$, sau đó sử dụng loại trừ bao gồm trên các ước số. Nếu mọi phần tử của mảng đều chia hết cho$d$, thì mỗi phần tử phải đến từ tập hợp$\{ d, 2d, 3d, \dots \}$. Số lượng các phần tử như vậy lên đến$i$là$\lfloor i/d \rfloor$, do đó số lượng mảng trong đó tất cả các phần tử đều chia hết cho$d$là$\lfloor i/d \rfloor^n$. 

Cho phép$F(i) = i^n$. Định nghĩa$g(i)$là số lượng mảng có gcd chính xác$i$. Sau đó, mọi mảng trong đó tất cả các phần tử đều chia hết cho$i$góp phần vào$\sum_{j \ge 1} g(ij)$. Điều này mang lại một biến đổi chia cổ điển:$$f(i) = \sum_{j \ge 1} g(ij)$$Ở đâu$f(i) = \lfloor k/i \rfloor^n$trong phiên bản giới hạn. 

Chúng tôi phục hồi$g(i)$sử dụng đảo ngược Möbius:$$g(i) = \sum_{j \ge 1} \mu(j) \cdot f(ij)$$Điều chúng ta thực sự cần là$b_i$, số mảng có gcd bằng một và các giá trị được giới hạn bởi$i$. Đây chính xác là:$$b_i = \sum_{j=1}^{i} \mu(j)\left\lfloor \frac{i}{j} \right\rfloor^n$$Vì vậy, nhiệm vụ trở thành tính tổng trên các ước số bằng cách sử dụng các giá trị hàm Möbius và lũy thừa nhanh của$\lfloor i/j \rfloor^n$. Từ$n, k$lớn, chúng tôi tính toán trước các giá trị Möbius lên tới$k$và đánh giá tổng một cách hiệu quả bằng cách nhóm các giá trị bằng nhau của$\lfloor i/j \rfloor$. 

Cuối cùng, chúng tôi tích lũy XOR trên tất cả$b_i$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k \cdot i^n)$|$O(1)$| Quá chậm | 
| Mobius + nhóm |$O(k \log k)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào danh tính:$$b_i = \sum_{d=1}^{i} \mu(d)\left\lfloor \frac{i}{d} \right\rfloor^n$$1. Tính toán trước hàm Möbius tối đa$k$sử dụng sàng tuyến tính. Điều này là cần thiết vì mỗi ước số đóng góp một dấu tùy thuộc vào cấu trúc phân tích thừa số nguyên tố của nó và Möbius mã hóa bao gồm-loại trừ một cách gọn gàng. 
2. Quyền hạn tính toán trước$x^n$chỉ khi cần sử dụng modulo lũy thừa nhanh$10^9 + 7$. Vì cùng một cơ sở$x$lặp đi lặp lại trên nhiều$i$, chúng tôi tính toán theo yêu cầu nhưng lưu kết quả vào bộ nhớ đệm để tránh tính toán lại. 
3. Đối với mỗi$i$từ 1 đến$k$, tính toán$b_i$bằng cách tính tổng các ước$d$. Thay vì lặp lại tất cả$d$, chúng tôi nhóm các phạm vi của$d$Ở đâu$\lfloor i/d \rfloor$là không đổi. Điều này làm giảm sự chuyển đổi từ tuyến tính sang logarit trên mỗi$i$. 
4. Đối với mỗi đoạn mà$\lfloor i/d \rfloor = v$, chúng tôi thêm:$$v^n \cdot \left(\sum_{d \in segment} \mu(d)\right)$$Điều này tránh việc tính toán lại cùng một công suất nhiều lần. 

1. Duy trì XOR đang chạy của tất cả$b_i$. Mỗi$b_i$được tính theo modulo$10^9+7$và câu trả lời cuối cùng là XOR trên kết quả số nguyên. 

Ý tưởng cấu trúc chính là các ràng buộc gcd biến phép nhân trên các ước số và phép đảo ngược Möbius chuyển đổi “tất cả các phần tử chia hết cho d” thành “cấu trúc gcd chính xác”. 

### Tại sao nó hoạt động 

Mỗi mảng được tính một lần trong đúng một lớp gcd. chức năng$\lfloor i/d \rfloor^n$đếm các mảng trong đó tất cả các phần tử là bội số của$d$và nghịch đảo Möbius trừ đi số lượng vượt quá đến từ các mảng có gcd có nhiều thừa số nguyên tố. Điều này đảm bảo rằng mỗi mảng đóng góp chính xác một lần vào$b_i$khi và chỉ khi gcd của nó bằng một. 

## Giải pháp Python```
PythonRun
```Việc triển khai bắt đầu bằng việc xây dựng các giá trị Möbius lên tới$k$. Đây là xương sống của việc bao gồm-loại trừ khả năng chia hết. 

Việc tính toán lũy thừa sử dụng phương pháp ghi nhớ vì cùng một cơ số$\lfloor i/d \rfloor$lặp đi lặp lại rất nhiều qua các lần lặp của$i$. Nếu không có bộ nhớ đệm, lũy thừa lặp đi lặp lại sẽ chiếm ưu thế trong thời gian chạy. 

Vòng lặp chính tính toán$b_i$bằng cách quét các khối chia. Mỗi khối tương ứng với một thương số không đổi$v = \lfloor i/d \rfloor$, đây là một thủ thuật tiêu chuẩn trong các bài toán tính tổng số chia. 

Cuối cùng, mỗi$b_i$được XOR vào kết quả theo yêu cầu. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu$n = 3, k = 4$. 

Đối với mỗi$i$, chúng tôi tính toán:$$b_i = \sum_{d=1}^{i} \mu(d)\left\lfloor \frac{i}{d} \right\rfloor^3$$| tôi | ước số d | tầng(i/d) | mu(d) | đóng góp | b_i | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 
| 2 | 1,2 | 2,1 | 1,-1 | 8 - 1 | 7 | 
| 3 | 1,2,3 | 3,1,1 | 1,-1,-1 | 27 - 1 - 1 | 25 | 
| 4 | 1,2,3,4 | 4,2,1,1 | 1,-1,-1,0 | 64 - 8 - 1 | 55 | 

Dấu vết này cho thấy cách hủy của Möbius loại bỏ các mảng bị đếm quá mức có gcd lớn hơn một, để lại chính xác các mảng nguyên tố cùng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k \log k)$| Mỗi$b_i$được tính toán thông qua nhóm số chia và tổng tiền tố Möbius | 
| Không gian |$O(k)$| Lưu trữ các giá trị và bộ nhớ đệm Möbius | 

Các ràng buộc cho phép lên đến$2 \cdot 10^6$, do đó, sàng tuyến tính hoặc gần tuyến tính kết hợp với xử lý logarit theo giá trị phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp biên nhỏ nhất | 
| 2 2 | 3 | tính chính xác bao gồm-loại trừ nhỏ | 
| 3 5 | cấu trúc XOR không cần thiết | tương tác trên nhiều b_i | 
| 10 1 | 1 | suy thoái phạm vi sụp đổ | 

## Vỏ cạnh 

Khi nào$i = 1$, chỉ có các mảng chứa đầy các mảng tồn tại, vì vậy gcd luôn là một và việc hủy bỏ Möbius giảm đáng kể xuống còn một số hạng còn tồn tại. Thuật toán xử lý việc này bởi vì chỉ$d = 1$đóng góp và$\mu(1) = 1$. 

Khi$i$là số nguyên tố, tất cả các ước số không tầm thường đều vắng mặt ngoại trừ$1$Và$i$và hàm Möbius gán đóng góp âm cho$i$, đảm bảo rằng chỉ còn lại các mảng có gcd chính xác một sau khi trừ trường hợp "tất cả chia hết cho số nguyên tố". 

Khi$i$có tính tổng hợp cao, nhiều ước số tương tác với nhau, nhưng tổng tiền tố của các giá trị Möbius đảm bảo rằng các cấu trúc được tính quá mức sẽ hủy chính xác ngay cả khi nhiều thừa số nguyên tố trùng nhau, vì Möbius bằng 0 trên các bình phương và xen kẽ trên các tích không có bình phương.
