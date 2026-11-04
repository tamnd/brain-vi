---
title: "CF 1163A – Ăn Canh"
description: "Chúng ta bắt đầu với một vòng tròn gồm những con mèo $n$ ngồi theo thứ tự, tạo thành một vòng liên tục duy nhất gồm các vị trí đã được chiếm giữ. Sau đó $m$ của những con mèo này rời đi. Sau mỗi lần khởi hành, vòng tròn không còn được kết nối đầy đủ vì các vị trí trống sẽ phá vỡ sự liền kề."
date: "2026-06-13T08:36:10+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1163
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 558 (Div. 2)"
rating: 900
weight: 1163
solve_time_s: 282
verified: true
draft: false
---

[CF 1163A - Ăn súp](https://codeforces.com/problemset/problem/1163/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 4 phút 42 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một vòng tròn$n$mèo ngồi theo thứ tự, tạo thành một vòng liên tục các vị trí đã chiếm giữ. Sau đó$m$trong số những con mèo này rời đi. Sau mỗi lần khởi hành, vòng tròn không còn được kết nối đầy đủ vì các vị trí trống sẽ phá vỡ sự liền kề. Những gì còn lại là một tập hợp các con mèo bị chiếm giữ, nhưng chúng không còn nhất thiết phải tiếp giáp nhau dọc theo vòng tròn nữa nên chúng chia thành nhiều đoạn liên tiếp tối đa. Nhiệm vụ là sắp xếp$m$mèo rời đi sao cho số lượng các phân đoạn bị chiếm giữ này càng lớn càng tốt. 

Một cách khác để xem tình huống là nghĩ về một mảng có độ dài$n$sắp xếp thành một vòng tròn. Chúng tôi loại bỏ$m$các chỉ mục và chúng tôi muốn tối đa hóa số khối liền kề của các chỉ mục còn lại xuất hiện khi được xem theo chu kỳ. 

Những hạn chế$n \le 1000$Và$m \le n$ngay lập tức loại trừ mọi nhu cầu tối ưu hóa nặng nề. Một giải pháp trong$O(n^2)$hoặc thậm chí$O(n^3)$là hoàn toàn an toàn. Điều quan trọng là cái nhìn sâu sắc về cấu trúc chứ không phải áp lực hiệu suất. 

Một số tình huống nguy hiểm cần được loại bỏ sớm. 

Nếu như$m = 0$, không có ai rời đi nên toàn bộ vòng tròn vẫn còn một khối. Giải pháp nào cũng phải trả lại$1$. 

Nếu như$m = n$, tất cả mèo đều bỏ đi nên không còn đoạn nào nữa, và đáp án là$0$. 

Một điểm tinh tế là cấu trúc vòng tròn rất quan trọng. Trong một đường thẳng, việc loại bỏ các điểm sẽ tạo ra các đoạn hoàn toàn dựa trên sự kề cận tuyến tính. Trong một vòng tròn, việc loại bỏ một điểm cũng có thể phá vỡ kết nối bao quanh giữa chỉ mục$n$Và$1$, điều này ảnh hưởng đến số lượng phân đoạn theo cách mà trực giác tuyến tính có thể bỏ sót. 

Một sai lầm ngây thơ là nghĩ rằng câu trả lời luôn là$m$, vì mỗi con mèo bị loại bỏ có thể “tạo ra” một khoảng trống mới. Điều đó là sai vì các khoảng trống chỉ trở thành các nhóm riêng biệt khi việc xóa được sắp xếp để tách các phân khúc hiện có thay vì nhóm lại với nhau. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta có thể nghĩ đến sức mạnh vũ phu. Chúng tôi chọn cái nào$m$mèo rời đi, mô phỏng vòng tròn còn lại và đếm xem còn lại bao nhiêu đoạn liền kề. Đối với mỗi lựa chọn, chúng tôi quét vòng tròn và đếm các lần chuyển đổi giữa trạng thái bị chiếm dụng và trạng thái trống, chú ý đến khả năng kết nối bao quanh. có$\binom{n}{m}$cách để chọn những con mèo đã bị loại bỏ và đối với mỗi con chúng ta cần$O(n)$làm việc để đếm các phân đoạn. Điều này trở nên không khả thi ngay cả đối với người vừa phải$n$, từ$\binom{1000}{500}$có kích thước lớn về mặt thiên văn. 

Quan sát quan trọng là chúng tôi không tối ưu hóa theo các mẫu tùy ý, chúng tôi chỉ quyết định cách phân phối$m$loại bỏ các điểm xung quanh một chu kỳ. Số lượng nhóm được hình thành phụ thuộc hoàn toàn vào khoảng cách giữa các lần xóa. Nếu việc xóa được nhóm lại với nhau, chúng sẽ phá hủy ít ranh giới hơn. Nếu chúng được đặt cách nhau thì mỗi lần xóa riêng lẻ có xu hướng chia thành một phân đoạn. 

Chiến lược tốt nhất là phân bố những con mèo bị loại càng đều càng tốt xung quanh vòng tròn. Mỗi con mèo bị loại bỏ bị cô lập sẽ tăng số lượng phân đoạn lên một, nhưng nếu hai con mèo bị loại bỏ ở cạnh nhau, chúng sẽ "hợp nhất" hiệu ứng của chúng và không tạo ra các phần tách bổ sung ngoài phân đoạn đầu tiên. 

Vì vậy, vấn đề được rút gọn thành câu hỏi: chúng ta có thể tạo ra bao nhiêu “khoảng trống” riêng biệt cho việc loại bỏ nếu chúng ta phân phối$m$dọn dẹp xung quanh$n$vị trí trên đường tròn? Mỗi khoảng trống như vậy góp phần bổ sung thêm một phân khúc cho những con mèo còn lại. 

Số lượng khoảng trống tối đa chính xác là số khối không trống của mèo bị loại bỏ khi được sắp xếp tối ưu, tối đa hóa khi mỗi con mèo bị loại bỏ được cách ly càng nhiều càng tốt. Tuy nhiên, chúng tôi bị hạn chế bởi độ dài vòng tròn, vì vậy chúng tôi không thể cô lập nhiều hơn$n - m$những con mèo còn lại, vì mỗi con mèo còn lại có thể đóng vai trò là vật phân cách giữa các nhóm loại bỏ. 

Điều này dẫn đến một kết quả có cấu trúc đơn giản: mỗi con mèo còn lại có thể tách tối đa một cặp khối bị loại bỏ, do đó số lượng nhóm mèo còn lại bị giới hạn bởi cả hai$m$(mỗi lần xóa có thể tăng nhóm lên tối đa một) và$n - m$(chúng tôi không thể có nhiều phân đoạn hơn mức cấu trúc còn lại có sẵn cho phép) và chúng tôi cũng không thể vượt quá$n - m$tầm thường vì mỗi nhóm phải có ít nhất một con mèo. 

Đặt các ràng buộc này lại với nhau mang lại:$$\text{answer} = \min(m, n - m)$$với sự điều chỉnh bổ sung cho phần bao tròn được xử lý hoàn toàn bởi cùng một giới hạn. 

Để hiểu tại sao điều này lại chặt chẽ, hãy tưởng tượng việc luân phiên loại bỏ và nuôi mèo càng nhiều càng tốt. Nếu như$m \le n/2$, chúng ta có thể cô lập mọi con mèo bị loại bỏ, tạo ra$m$những vết cắt riêng biệt. Nếu như$m > n/2$, thay vào đó, chúng tôi buộc phải cách ly những con mèo còn lại, giới hạn số lượng nhóm ở mức$n - m$. 

### Bảng độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{n}{m} \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$Và$m$. Chúng đại diện cho tổng số mèo và số lượng mèo bị loại bỏ. 
2. Nếu$m = 0$, trở lại$1$, vì vòng tròn vẫn được kết nối đầy đủ. 
3. Nếu$m = n$, trở lại$0$, vì không còn con mèo nào nữa. 
4. Tính đáp án dưới dạng$\min(m, n - m)$. Điều này phản ánh liệu những con mèo bị loại bỏ hay những con mèo còn lại có phải là yếu tố hạn chế về số lượng nhóm tách biệt có thể được hình thành hay không. 
5. Xuất giá trị tính toán. 

Ý tưởng chính đằng sau bước 4 là mọi nhóm mèo còn lại phải được bao quanh bởi những con mèo đã bị loại bỏ ở cả hai bên (theo nghĩa vòng tròn). Nếu có quá ít lượt xóa thì mỗi lần xóa có thể tạo ra một phần tách mới nhiều nhất. Nếu còn lại quá ít mèo, bản thân chúng sẽ trở thành nút thắt cổ chai cho việc có thể tồn tại bao nhiêu phân khúc riêng biệt. 

### Tại sao nó hoạt động 

Vòng tròn có thể được xem như các khối xen kẽ của những con mèo được giữ và bị loại bỏ. Mỗi nhóm mèo được nuôi tương ứng với một quá trình chuyển đổi từ bị loại sang bị giữ và quay lại bị loại bỏ. Tối đa hóa các nhóm tương đương với việc tối đa hóa các chuyển đổi như vậy. Số lượng chuyển đổi có thể được giới hạn ở trên bởi cả số lượng vị trí bị loại bỏ và số lượng vị trí được giữ lại, vì mỗi chuyển đổi tiêu thụ ít nhất một trong mỗi bên. Do đó, số lượng phân đoạn được giữ tối đa chính xác là$\min(m, n - m)$. 

## Giải pháp Python```
PythonRun
```Việc triển khai được cố ý tối thiểu vì công thức dẫn xuất đã mã hóa toàn bộ cấu trúc của vấn đề. Các trường hợp đặc biệt đối với$m = 0$Và$m = n$xử lý các cấu hình suy biến trong đó biểu thức chung sẽ không trực tiếp tạo ra số phân đoạn chính xác. 

Điểm tinh tế duy nhất là đảm bảo rằng cách diễn giải vòng tròn được hấp thụ chính xác vào công thức. Chúng tôi không mô phỏng rõ ràng tính kề cận hoặc tính bao quanh, vì đối số đối xứng đã tính đến điều đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 7, m = 4$Chúng tôi tính toán$\min(4, 3) = 3$. 

| Bước | n | m | n - m | kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 7 | 4 | 3 | - | 
| tính toán | 7 | 4 | 3 | 3 | 

Điều này cho thấy rằng mặc dù bốn con mèo rời đi, nhưng chỉ có thể tối đa hóa ba nhóm rời rạc còn lại vì chỉ còn lại ba con mèo và chúng hạn chế số lượng tách biệt có thể được hình thành. 

### Ví dụ 2:$n = 6, m = 2$Chúng tôi tính toán$\min(2, 4) = 2$. 

| Bước | n | m | n - m | kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 6 | 2 | 4 | - | 
| tính toán | 6 | 2 | 4 | 2 | 

Ở đây việc loại bỏ là yếu tố hạn chế. Chúng ta có thể cô lập cả hai con mèo đã bị loại bỏ, tạo ra hai phần tách trong cấu trúc còn lại. 

Hai trường hợp này minh họa ràng buộc kép: đôi khi việc loại bỏ sẽ hạn chế việc phân đoạn và đôi khi cấu trúc còn lại sẽ hạn chế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp thời gian không đổi phù hợp thoải mái trong các ràng buộc ngay cả ở mức tối đa$n = 1000$, vì không cần lặp lại vòng tròn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 | 1 | trường hợp loại bỏ trống | 
| 2 2 | 0 | trường hợp loại bỏ đầy đủ | 
| 5 1 | 1 | sự gián đoạn tối thiểu | 
| 5 4 | 1 | loại bỏ nặng đối xứng | 
| 10 5 | 5 | trường hợp chia tối đa cân bằng | 

## Vỏ cạnh 

Khi nào$m = 0$, công thức$\min(m, n - m)$cho$0$, nhưng câu trả lời đúng là$1$bởi vì vòng tròn đầy đủ vẫn là một khối được kết nối. Thuật toán ghi đè rõ ràng trường hợp này trước khi áp dụng công thức. 

Khi$m = n$, công thức cho$0$, phù hợp với cách giải thích chính xác về cấu hình trống. Không còn con mèo nào nên không có nhóm nào tồn tại. 

Khi$m = 1$, công thức cho$\min(1, n-1) = 1$. Điều này tương ứng với việc loại bỏ một lần tạo ra chính xác một điểm ngắt trong tính liên tục của vòng tròn, không thể mở rộng thêm vì chỉ có một vị trí bị loại bỏ để đóng vai trò là dấu phân cách. 

Khi$m = n-1$, công thức cho$1$, vì chỉ còn lại một con mèo và nó tạo thành một phân đoạn biệt lập duy nhất bất kể cách sắp xếp loại bỏ.
