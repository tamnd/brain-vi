---
title: "CF 1215D - Vé Game"
description: "Chúng ta được cấp một tấm vé được biểu thị bằng một chuỗi có độ dài chẵn. Mỗi vị trí tương ứng với một chữ số từ 0 đến 9, ngoại trừ một số vị trí không xác định và được đánh dấu bằng dấu chấm hỏi. Các vị trí chưa biết cuối cùng phải được điền bằng chữ số."
date: "2026-06-13T17:36:29+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1215
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 585 (Div. 2)"
rating: 1700
weight: 1215
solve_time_s: 480
verified: false
draft: false
---

[CF 1215D - Vé trò chơi](https://codeforces.com/problemset/problem/1215/D) 

**Đánh giá:** 1700 
**Tags:** trò chơi, tham lam, toán học 
**Thời gian giải:** 8 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tấm vé được biểu thị bằng một chuỗi có độ dài chẵn. Mỗi vị trí tương ứng với một chữ số từ 0 đến 9, ngoại trừ một số vị trí không xác định và được đánh dấu bằng dấu chấm hỏi. Các vị trí chưa biết cuối cùng phải được điền bằng chữ số. 

Hai người chơi lần lượt điền vào những vị trí chưa xác định này, bắt đầu từ Monocarp. Ở mỗi nước đi, người chơi chọn bất kỳ dấu chấm hỏi nào còn lại và thay thế nó bằng bất kỳ chữ số nào họ muốn. Sau khi tất cả các vị trí đã được lấp đầy, vé sẽ được đánh giá: nó được “cân bằng” nếu tổng của nửa chữ số đầu bằng tổng của nửa sau. Bicarp thắng chính xác khi điều kiện cân bằng này được giữ nguyên, nếu không thì Monocarp thắng. 

Khó khăn chính là người chơi không chỉ điền chữ số mà còn chủ động điều khiển sự khác biệt cuối cùng giữa hai hiệp. Monocarp muốn tạo ra sự bất bình đẳng, trong khi Bicarp muốn bảo toàn khả năng bình đẳng dù Monocarp đi trước. 

Các ràng buộc cho phép tối đa 200000 ký tự, điều này ngay lập tức loại trừ mọi hoạt động khám phá không gian trạng thái hoặc cây trò chơi đối với các bài tập. Bất kỳ giải pháp nào cũng phải nén trò chơi thành một số lượng tổng hợp không đổi, vì chỉ số lượng và tổng quan trọng chứ không phải vị trí cụ thể của các lựa chọn riêng lẻ. 

Một nỗ lực ngây thơ sẽ mô phỏng tất cả các bài tập hoặc thậm chí coi đây là một trò chơi minimax trên các chữ số còn lại. Điều đó không thành công vì mỗi dấu hỏi trong số 200000 dấu hỏi sẽ phân nhánh thành 10 khả năng, làm cho không gian trạng thái theo cấp số nhân. 

Một vấn đề phức tạp hơn là tính đối xứng của các nửa không có nghĩa là tính đối xứng của trò chơi. Một sai lầm phổ biến là chỉ theo dõi số lượng dấu chấm hỏi còn lại trong mỗi hiệp, bỏ qua rằng Monocarp có thể tạo ra sự mất cân bằng bằng cách ghép các lựa chọn giữa các hiệp theo cách mà Bicarp có thể phản ánh hoặc không thể phản ánh tùy theo thứ tự di chuyển. 

Một ví dụ nhỏ trong đó phá vỡ lý luận đối xứng ngây thơ là: 

đầu vào:```

```Nếu chúng ta giả định “cả hai bên đều đối xứng nên Bicarp luôn có thể sánh ngang với Monocarp”, chúng ta có thể dự đoán sai rằng Bicarp luôn thắng. Nhưng Monocarp chơi trước và có thể chọn một chữ số mà Bicarp không thể vô hiệu hóa hoàn toàn do thứ tự lần lượt ảnh hưởng đến việc ghép các đóng góp. 

Lý do chính xác phải giải thích rõ ràng có bao nhiêu cặp nước đi tồn tại và mức độ mất cân bằng tích lũy giữa các vị trí bắt buộc và tự do. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua cách chơi tối ưu, chúng ta có thể thử dùng vũ lực: liệt kê tất cả các cách để điền vào dấu chấm hỏi, sau đó kiểm tra xem liệu có tồn tại chiến lược chiến thắng cho Monocarp hay Bicarp hay không. Ngay cả đối với một cấu hình duy nhất, việc đánh giá tất cả các lần hoàn thành là$10^k$, Ở đâu$k$là số lượng dấu hỏi. Với$k$lên tới 200000, điều này là không thể. 

Ý tưởng ngây thơ thứ hai là mô phỏng trò chơi dưới dạng cây minimax trên các dấu hỏi còn lại. Mỗi tiểu bang phụ thuộc vào vị trí nào được lấp đầy và bởi ai. Điều này dẫn đến hệ số phân nhánh bằng với các dấu chấm hỏi còn lại, lại theo cấp số nhân. 

Thông tin chi tiết quan trọng là các giá trị thực tế được chọn bên trong dấu chấm hỏi không quan trọng riêng lẻ. Chỉ có tổng chênh lệch đóng góp giữa hiệp một và hiệp hai mới quan trọng. Mỗi chữ số được điền đóng góp tích cực hoặc tiêu cực tùy thuộc vào vị trí của nó. 

Vì vậy, mỗi bước di chuyển đều có hiệu quả là chọn một giá trị trong phạm vi từ 0 đến 9 và cộng nó vào một trong hai tổng hiện có. Trò chơi trở thành việc kiểm soát sự khác biệt cuối cùng giữa các khoản tiền này. 

Cấu trúc đơn giản hóa hơn nữa khi chúng ta quan sát việc ghép cặp: đối với mỗi vị trí i trong nửa đầu và đối tác của nó i + n/2 trong nửa sau, điều quan trọng là liệu hai vị trí đó đã biết hay chưa. Trò chơi giảm thiểu số lượng vị trí được ghép nối là ẩn số đối xứng và bao nhiêu vị trí không đối xứng. 

Các cặp không đối xứng có tính quyết định vì ai thực hiện nước đi cuối cùng trong cặp đó sẽ kiểm soát sự mất cân bằng của cặp đó. Các cặp đối xứng (cả hai đều không xác định trong cùng một nửa cấu trúc) hoạt động khác nhau và có thể được vô hiệu hóa trong cách chơi tối ưu tùy thuộc vào tính chẵn lẻ. 

Điều này dẫn đến sự phân loại rõ ràng dựa trên số lượng ẩn số tồn tại trong mỗi nửa và cách chúng tương tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^k) | O(k) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia chuỗi thành hai nửa và tính tổng chênh lệch ban đầu được đóng góp bởi các chữ số đã biết. Trừ tổng nửa sau từ tổng nửa đầu. Điều này tạo ra sự mất cân bằng ban đầu trước bất kỳ động thái nào. 
2. Đếm xem nửa đầu và nửa sau có bao nhiêu dấu chấm hỏi. Gọi chúng là q1 và q2. Những điều này xác định số lượng nước đi có thể kiểm soát được mà mỗi bên thực hiện một cách hiệu quả trên mỗi tổng. 
3. Xét tổng số dấu chấm hỏi q = q1 + q2. Vì người chơi luân phiên nhau và Monocarp di chuyển trước, Monocarp điều khiển trần (q / 2) di chuyển và Bicarp điều khiển sàn (q / 2) di chuyển. Điều này rất quan trọng vì việc kiểm soát tính chẵn lẻ của nước đi sẽ xác định ai có thể buộc thực hiện các điều chỉnh cuối cùng. 
4. Bây giờ hãy phân tích cấu trúc đóng góp mất cân bằng cho mỗi cặp nước đi. Mỗi nước đi đóng góp một chữ số từ 0 đến 9, nhưng điều quan trọng là nó được cộng vào tổng nửa đầu hay nửa sau. 
5. Nếu số dấu chấm hỏi ở cả hai hiệp bằng nhau thì mỗi nước đi có thể được ghép đối xứng qua các nửa. Trong trường hợp này, sự mất cân bằng ban đầu hoàn toàn quyết định kết quả vì cả hai người chơi đều có thể phản chiếu ảnh hưởng của nhau. 
6. Nếu số lượng khác nhau, một người chơi sẽ có thêm sự linh hoạt trong một hiệp. Người chơi có nhiều vị trí sẵn có hơn trong một hiệp có thể làm sai lệch tổng chênh lệch bằng cách tối đa hóa hoặc giảm thiểu khoản đóng góp trong hiệp đó. 
7. Trò chơi chuyển sang kiểm tra xem khoảng chênh lệch cưỡng bức cuối cùng có chứa số 0 hay không. Bicarp thắng khi và chỉ khi có thể cân bằng số tiền khi chơi tối ưu; nếu không Monocarp sẽ thắng. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi cặp nước đi, trạng thái trò chơi có thể được tóm tắt bằng một giá trị duy nhất: chênh lệch hiện tại giữa một nửa số tiền, cộng với khả năng còn lại để điều chỉnh chênh lệch đó thông qua các dấu chấm hỏi không được sử dụng. Vì mỗi nước đi chỉ làm thay đổi tổng của một bên một lượng giới hạn và người chơi có toàn quyền kiểm soát các giá trị được gán, nên khoảng chênh lệch cuối cùng có thể đạt được được xác định hoàn toàn bằng số dấu chấm hỏi mỗi hiệp và tính chẵn lẻ của các lượt. Không có thông tin vị trí nào tồn tại ngoài những tổng hợp này, do đó, quyết định sẽ giảm xuống liệu Monocarp có thể buộc chênh lệch cuối cùng khác xa 0 bất kể phản hồi của Bicarp hay không. 

## Giải pháp Python```
PythonRun
```Việc triển khai sẽ chia vé thành hai nửa và tính trực tiếp các tổng chữ số đã biết. Nó cũng tính các vị trí không xác định trên mỗi nửa. Tính chẵn lẻ của tổng số ẩn số xác định liệu nước đi cân bằng cuối cùng có tồn tại hay không hay liệu Monocarp có thực hiện được hành động cuối cùng mang tính quyết định hay không. 

Điểm quyết định quan trọng là so sánh ql và qr. Khi cả hai nửa có cùng số lượng ẩn số, trò chơi đủ đối xứng để Bicarp luôn có thể phản ánh ảnh hưởng của Monocarp. Khi chúng khác nhau, sự mất cân bằng ở các vị trí có thể kiểm soát được cho phép Monocarp điều chỉnh sự khác biệt cuối cùng khỏi con số 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```| Bước | tổng_left | tổng_right | ql | qr | khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 5 | 5 | 0 | 0 | 0 | 

Không có dấu hỏi nào tồn tại, do đó không có nước đi nào xảy ra và sự khác biệt cuối cùng đã bằng không. 

Điều này xác nhận tính bất biến rằng nếu không có tính linh hoạt, kết quả được cố định khi đánh giá đầu vào và vì các nửa đã khớp nhau nên Bicarp thắng. 

### Ví dụ 2 

đầu vào:```

```| Bước | tổng_left | tổng_right | ql | qr | khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 0 | 1 | 1 | 0 | 

Tổng cộng có hai nước đi, một nước đi cho mỗi người chơi. Vì cả hai nửa đều đối xứng nên Bicarp luôn có thể phản ánh lựa chọn của Monocarp, đảm bảo cả hai chữ số đều có tổng đóng góp bằng nhau. 

Điều này chứng tỏ rằng tính đối xứng trong phân bố không xác định sẽ ngăn Monocarp tạo ra sự mất cân bằng không thể đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | chuyển một chuỗi để tính tổng và đếm | 
| Không gian | O(1) | chỉ các bộ đếm và tổng được lưu trữ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó xử lý tối đa 200000 ký tự trong thời gian tuyến tính với chi phí bộ nhớ không đổi. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4/0523 | bicarp | trường hợp cạnh không di chuyển | 
| 2 / ?? | bicarp | tính chẵn lẻ đối xứng chưa biết | 
| trộn nhỏ | Monocarp hoặc Bicarp | ảnh hưởng bất đối xứng | 
| tất cả ? | bicarp | trường hợp đối xứng đầy đủ | 

## Vỏ cạnh 

Một vé được biết đến đầy đủ, chẳng hạn như`n = 4, s = 0523`, không có động thái. Thuật toán ngay lập tức trả về dựa trên việc các nửa đã khớp hay chưa và tránh mọi giả định về lượt của người chơi. 

Một vé hoàn toàn không xác định như`????`chia đều cho các nửa hoặc không tùy theo mức phân phối. Khi cả hai nửa có số lượng không xác định bằng nhau, thuật toán sẽ phân loại nó thành đối xứng, điều này đảm bảo khả năng phản ánh Monocarp của Bicarp và duy trì sự bình đẳng. 

Một phân phối sai lệch như`?0000`lực lượng mất cân bằng vì một bên có thêm sự linh hoạt. Thuật toán nắm bắt điều này thông qua`ql != qr`, điều này kích hoạt điều kiện chiến thắng của Monocarp bằng cách xác định quyền kiểm soát không đối xứng đối với vị trí chữ số.
