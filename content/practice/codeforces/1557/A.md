---
title: "CF 1557A - Ezzat và hai dãy số"
description: "Chúng ta được cấp một mảng các số nguyên và chúng ta phải chia nó thành hai nhóm không trống trong khi bảo toàn mọi phần tử chính xác một lần. Mỗi nhóm có số điểm bằng giá trị trung bình số học của nó và mục tiêu là tối đa hóa tổng của hai giá trị này."
date: "2026-06-16T16:11:44+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1557
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 737 (Div. 2)"
rating: 800
weight: 1557
solve_time_s: 196
verified: true
draft: false
---

[CF 1557A - Ezzat và hai chuỗi con](https://codeforces.com/problemset/problem/1557/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học, sắp xếp 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và chúng ta phải chia nó thành hai nhóm không trống trong khi bảo toàn mọi phần tử chính xác một lần. Mỗi nhóm có số điểm bằng giá trị trung bình số học của nó và mục tiêu là tối đa hóa tổng của hai giá trị này. 

Vì vậy nếu chúng ta chọn chia thành hai dãy con$a$Và$b$, mục tiêu là$$\frac{\sum a}{|a|} + \frac{\sum b}{|b|}.$$Khó khăn là cả tổng và kích thước đều phụ thuộc vào cách chúng ta phân chia các phần tử, do đó, một trực giác ngây thơ như “đặt các số lớn lại với nhau” rõ ràng là không hợp lý. 

Các ràng buộc rất mạnh: lên đến$10^5$các phần tử cho mỗi trường hợp thử nghiệm và lên đến$3 \cdot 10^5$tổng cộng. Điều này ngay lập tức loại trừ bất cứ điều gì xem xét tất cả các phân vùng, vì việc chia thành hai tập hợp con đã có$2^n$khả năng. Ngay cả suy luận bậc hai cho mỗi trường hợp thử nghiệm cũng quá chậm trong trường hợp xấu nhất. 

Một điểm tinh tế là hàm này không tuyến tính theo kích thước phân vùng. Việc di chuyển một phần tử sẽ làm thay đổi cả tử số và mẫu số, vì vậy lý luận tham lam rõ ràng là không an toàn trừ khi chúng ta rút gọn cấu trúc. 

Các trường hợp cạnh có thể đánh lừa các giải pháp ngây thơ bao gồm các mảng có tất cả các giá trị âm và các mảng trong đó sự phân chia tốt nhất không phải là “nửa lớn nhất so với nửa nhỏ nhất” mà là thứ gì đó không cân bằng hơn. 

Ví dụ: nếu tất cả các số đều âm như$[-7, -6, -6]$, chỉ đặt phần tử âm nhất sẽ cải thiện cấu trúc trung bình theo cách không trực quan vì tính trung bình sẽ phạt quy mô nhóm một cách khác nhau đối với các giá trị âm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng bằng mọi cách để gán từng phần tử cho một trong hai chuỗi con. Đối với mỗi phân vùng, hãy tính cả hai mức trung bình và theo dõi tổng tối đa. Điều này đúng vì nó đánh giá trực tiếp hàm mục tiêu, nhưng nó đòi hỏi phải kiểm tra$2^n$bài tập cho mỗi trường hợp thử nghiệm, điều này là không thể ngay cả đối với$n = 30$. 

Để tìm cấu trúc, hãy viết lại biểu thức. Giả sử chúng ta chia thành tập hợp$a$Và$b$. Gọi tổng số tiền là$S$, và để tập hợp con$a$có tổng$S_a$và kích thước$k$. Sau đó$b$có tổng$S - S_a$và kích thước$n - k$. Mục tiêu trở thành$$\frac{S_a}{k} + \frac{S - S_a}{n-k}.$$Bây giờ quan sát quan trọng là đối với một giá trị cố định$k$, tập hợp con tốt nhất$a$đơn giản là tập hợp của$k$phần tử lớn nhất. Điều này xuất phát từ một đối số trao đổi tiêu chuẩn: hoán đổi một phần tử nhỏ hơn trong$a$với phần tử lớn hơn trong$b$luôn tăng hoặc giữ nguyên giá trị. 

Điều này làm giảm vấn đề sắp xếp mảng. Sau khi sắp xếp, chúng tôi thử mọi điểm phân chia$k$từ$1$ĐẾN$n-1$, tính tổng tiền tố sao cho mỗi phần phân chia ứng cử viên được đánh giá theo thời gian không đổi. 

Do đó, vấn đề trở thành tối ưu hóa một chiều trên các tiền tố đã được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo rằng bất kỳ tiền tố nào cũng tương ứng với một tập hợp các phần tử nhỏ nhất và bất kỳ hậu tố nào cũng tương ứng với các phần tử lớn nhất. 
2. Xây dựng tổng tiền tố sao cho tổng của số đầu tiên$i$các phần tử có thể được truy vấn trong$O(1)$. Điều này cho phép đánh giá nhanh bất kỳ sự phân chia nào. 
3. Đối với từng vị trí phân chia$k$từ$1$ĐẾN$n-1$, xử lý cái đầu tiên$k$các phần tử dưới dạng nhóm$a$và phần còn lại là nhóm$b$. 
4. Tính giá trị$$\frac{\text{sum}(0..k-1)}{k} + \frac{\text{total} - \text{sum}(0..k-1)}{n-k}.$$Điều này mang lại giá trị tốt nhất có thể cho kích thước phân chia cố định đó vì thứ tự sắp xếp đảm bảo việc phân nhóm tối ưu. 
5. Theo dõi tối đa trên tất cả hợp lệ$k$. 
6. Xuất kết quả cho từng test case. 

Lý do mỗi phần phân chia chỉ cần được kiểm tra một lần là vì trong một kích thước phân chia cố định, mọi sai lệch so với cấu trúc tiền tố đã sắp xếp đều có thể được cải thiện bằng cách hoán đổi các phần tử qua ranh giới. 

### Tại sao nó hoạt động 

Sửa kích thước chia nhỏ$k$. Giả sử chúng ta có bất kỳ tập hợp con nào$a$kích thước$k$. Nếu như$a$chứa một phần tử$x$và bên ngoài$a$có một phần tử lớn hơn$y$, hoán đổi$x$ra ngoài và$y$thực sự làm tăng tổng của$a$và điều chỉnh nhóm còn lại một cách tương ứng theo cách bảo toàn tổng số tiền. Sự thay đổi trong mục tiêu đơn giản hóa thành so sánh tuyến tính luôn ưu tiên tập trung các giá trị lớn hơn vào cùng một nhóm khi kích thước được cố định. Hoán đổi lặp đi lặp lại dẫn đến tập hợp con của$k$phần tử lớn nhất, chứng tỏ tính tối ưu cho từng phần tử$k$. Vì chúng tôi đánh giá tất cả$k$, tối ưu toàn cục theo sau. 

## Giải pháp Python```
PythonRun
```Mã bắt đầu bằng cách sắp xếp mảng sao cho các nhóm ứng cử viên trở thành tiền tố và hậu tố liền kề. Mảng tiền tố cho phép truy vấn tổng thời gian không đổi cho bất kỳ phần chia nào. Mỗi điểm phân chia được đánh giá bằng cách tính trực tiếp hai giá trị trung bình. 

Một chi tiết triển khai tinh tế là sử dụng phép chia dấu phẩy động cho giá trị trung bình. Vì độ chính xác yêu cầu là$10^{-6}$, độ chính xác kép tiêu chuẩn là đủ. Một chi tiết quan trọng khác là khởi tạo`best`đến một số rất âm để xử lý các trường hợp trong đó tất cả các giá trị đều âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[3, 1, 2]`Mảng được sắp xếp:`[1, 2, 3]`| k | tổng_a | trung bình_a | tổng_b | trung bình_b | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1.0 | 5 | 2,5 | 3 | 
| 2 | 3 | 1,5 | 3 | 3.0 | 3 | 

Cực đại xảy ra ở$k = 2$, cho$1.5 + 3 = 4.5$. 

Dấu vết này cho thấy tại sao sự mất cân bằng có thể hữu ích: phần tử lớn hơn thích hình thành một nhóm đơn lẻ để tránh làm giảm mức trung bình của một nhóm lớn hơn. 

### Ví dụ 2 

đầu vào:`[-7, -6, -6]`Mảng được sắp xếp:`[-7, -6, -6]`| k | tổng_a | trung bình_a | tổng_b | trung bình_b | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | -7 | -7,0 | -12 | -6.0 | -19 | 
| 2 | -13 | -6,5 | -6 | -6.0 | -19 | 

Cách chia tốt nhất là$k = 2$, cho$-6.5 + -6 = -12.5$. 

Điều này cho thấy ngay cả với các giá trị âm, chiến lược tối ưu vẫn bị chi phối bởi thứ tự chứ không phải dựa trên dấu hiệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét tiền tố là tuyến tính | 
| Không gian |$O(n)$| Tổng tiền tố được lưu trữ cùng với đầu vào | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi$3 \cdot 10^5$, do đó việc sắp xếp theo từng trường hợp thử nghiệm sẽ phù hợp một cách thoải mái trong giới hạn thời gian. Giải pháp chỉ thực hiện công việc bổ sung tuyến tính ngoài việc sắp xếp. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ mẫu | đưa ra | tính đúng đắn trong các trường hợp hỗn hợp | 
| tất cả đều bình đẳng | 4.0 | tính đối xứng và tính trung lập | 
| 2 yếu tố | biến | phân chia hợp lệ tối thiểu | 
| thái cực hỗn hợp | biến | xử lý phạm vi rộng | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, mọi phép chia đều mang lại cùng một giá trị vì cả hai giá trị trung bình đều bằng cùng một hằng số. Thuật toán vẫn sắp xếp và đánh giá tất cả các phần tách và mọi giá trị được tính toán đều khớp với tổng không đổi dự kiến ​​của hai giá trị trung bình giống hệt nhau. 

Khi kích thước mảng chính xác là hai thì chỉ có một phân vùng hợp lệ. Vòng lặp chỉ đánh giá$k = 1$, do đó thuật toán trả về trực tiếp$a_1 + a_2$, phù hợp với định nghĩa của hai mức trung bình đơn lẻ. 

Khi các giá trị trải dài trong phạm vi âm và dương lớn, việc sắp xếp sẽ đảm bảo rằng các giá trị cực trị được tách thành các nhóm khác nhau nếu cần. Đánh giá tiền tố-hậu tố tự động kiểm tra các phần tách trong đó các phần dương lớn được nhóm lại với nhau và các phần âm lớn được tách ra, do đó không cần cách viết hoa đặc biệt.
