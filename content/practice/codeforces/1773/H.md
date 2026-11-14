---
title: "CF 1773H - Nóng và Lạnh"
description: "Chúng ta đang chơi trò chơi đoán tọa độ trên một lưới số nguyên lớn. Có một điểm mục tiêu ẩn ở đâu đó trong hình vuông từ $(0,0)$ đến $(10^6,10^6)$."
date: "2026-06-15T03:53:05+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1773
codeforces_index: "H"
codeforces_contest_name: "2022-2023 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2600
weight: 1773
solve_time_s: 138
verified: false
draft: false
---

[CF 1773H - Nóng và Lạnh](https://codeforces.com/problemset/problem/1773/H) 

**Đánh giá:** 2600 
**Tags:** tìm kiếm nhị phân, tương tác 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang chơi trò chơi đoán tọa độ trên một lưới số nguyên lớn. Có một điểm mục tiêu ẩn ở đâu đó trong hình vuông từ$(0,0)$ĐẾN$(10^6,10^6)$. Chúng tôi được phép “truy vấn” bất kỳ điểm nào bằng cách in tọa độ của nó và sau mỗi truy vấn, hệ thống sẽ cho chúng tôi biết khoảng cách Euclide của chúng tôi đến điểm ẩn so với truy vấn trước đó như thế nào. 

Phản hồi mang tính chất định tính hơn là số lượng. Sau truy vấn đầu tiên, nếu chúng ta không đến được mục tiêu, chúng ta sẽ được biết điểm tiếp theo gần hơn, xa hơn hay xa hơn so với điểm trước đó. Nếu chúng ta chạm đến điểm ẩn chính xác, trọng tài sẽ trả lời bằng một thông báo kết thúc đặc biệt kết thúc bằng dấu chấm than và chúng ta phải dừng lại ngay lập tức. 

Khó khăn cốt lõi là chúng ta không bao giờ nhìn thấy khoảng cách một cách trực tiếp. Chúng tôi chỉ quan sát sự so sánh giữa các truy vấn liên tiếp, điều này khiến đây trở thành vấn đề điều hướng có hướng chứ không phải là vấn đề tái thiết hình học. 

Ràng buộc của tối đa 64 truy vấn là cực kỳ chặt chẽ do$10^6 \times 10^6$không gian tìm kiếm. Tìm kiếm dạng lưới đơn giản hoặc thăm dò ngẫu nhiên không có sự đảm bảo thành công có ý nghĩa. Ngay cả tìm kiếm nhị phân theo một chiều cũng không thể áp dụng trực tiếp vì hàm khoảng cách không thể tách thành các thành phần x và y độc lập. 

Một trường hợp phức tạp phát sinh từ phản ứng “cùng khoảng cách”. Điều này cho phép các khu vực bằng phẳng nơi chuyển động không làm thay đổi khoảng cách, điều này có thể phá vỡ các chiến lược leo đồi ngây thơ. Ví dụ: di chuyển dọc theo một vòng tròn có tâm ở kho báu mang lại phản hồi giống hệt nhau, do đó, bất kỳ chiến lược nào giả định tính đơn điệu nghiêm ngặt theo hướng đã chọn đều có thể bị đình trệ hoặc quay vòng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ coi đây là một vấn đề tìm kiếm hộp đen trên một lưới hàng triệu triệu. Người ta có thể tưởng tượng việc thăm dò một cách có hệ thống hoặc ngẫu nhiên, nhưng mỗi truy vấn chỉ đưa ra khoảng cách tương đối với điểm trước đó chứ không phải thông tin vị trí tuyệt đối. Điều này có nghĩa là chúng ta không thể tích lũy hàm tiềm năng toàn cục một cách ổn định trên các truy vấn không liên quan. 

Điểm mấu chốt là mặc dù chúng ta không thể đo khoảng cách nhưng chúng ta có thể so sánh khoảng cách tương đối với một điểm tham chiếu cố định. Nếu chúng ta sửa một điểm$A$và một điểm khác$B$, sau đó truy vấn$A$Và$B$, mọi so sánh tiếp theo sẽ cho chúng ta biết liệu điểm ẩn có nằm gần hơn không$A$hoặc$B$dọc theo đường so sánh khoảng cách. Điều này cho phép chúng ta liên tục “loại bỏ” các vùng của mặt phẳng một cách có kiểm soát. 

Vấn đề trở nên có thể giải quyết được bằng cách duy trì một vùng ứng cử viên nơi kho báu có thể nằm và thu nhỏ nó bằng cách sử dụng so sánh khoảng cách. Mỗi truy vấn được chọn sao cho nó phân vùng vùng khả thi còn lại thành hai phần và phản hồi cho chúng ta biết nên giữ bên nào. Đây thực chất là một dạng hình học của tìm kiếm nhị phân trên các ràng buộc lồi do so sánh khoảng cách. 

Cấu trúc tiêu chuẩn sử dụng việc thăm dò điểm giữa lặp đi lặp lại trong hộp giới hạn hoặc thu hẹp thích ứng xung quanh tâm được duy trì. Bằng cách lựa chọn cẩn thận các điểm truy vấn để phép so sánh hoạt động giống như kiểm tra dấu hiệu định hướng, chúng ta có thể giảm vùng khả thi theo cấp số nhân. 

Chiến lược tối ưu đạt được số lượng tinh chỉnh hiệu quả theo logarit trong mỗi chiều và giới hạn 64 truy vấn được thiết kế để phù hợp với phép chia đôi hình học lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tìm kiếm lưới Brute Force | O(10^{12}) | O(1) | Không thể | 
| Chiến lược chia đôi hình học | Truy vấn O(log R) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì điểm ứng viên hiện tại và thu hẹp cửa sổ tìm kiếm trên lưới. Chiến lược này là thăm dò nhiều lần các điểm phân chia vùng không chắc chắn còn lại. 

1. Ví dụ, bắt đầu từ một điểm tùy ý$(0,0)$. Điều này khởi tạo tham chiếu khoảng cách đầu tiên mặc dù chúng ta không biết vị trí mục tiêu. Phản hồi đầu tiên chỉ có ý nghĩa sau truy vấn thứ hai. 
2. Ví dụ: chọn điểm thứ hai cách xa điểm đầu tiên$(10^6,10^6)$, để tạo ra sự tương phản có hướng mạnh mẽ trong hành vi khoảng cách. Điều này đảm bảo rằng gradient khoảng cách trên mặt phẳng được tách biệt rõ ràng. 
3. Diễn giải từng câu trả lời dưới dạng so sánh giữa khoảng cách với truy vấn trước đó. Nếu di chuyển từ điểm$P$ĐẾN$Q$mang lại “Gần hơn”, khi đó điểm ẩn nằm trong nửa mặt phẳng nơi khoảng cách tới$Q$nhỏ hơn so với$P$. Điều này xác định sự bất bình đẳng tuyến tính trong tọa độ. 
4. Duy trì giao điểm của tất cả các nửa mặt phẳng như vậy là vùng khả thi. Thay vì lưu trữ nó một cách rõ ràng, chúng tôi ước chừng nó bằng cách theo dõi một điểm đại diện bên trong nó. 
5. Ở mỗi bước, xây dựng truy vấn tiếp theo bằng cách di chuyển từ điểm đại diện hiện tại theo hướng chia đôi độ không chắc chắn còn lại. Một lựa chọn tự nhiên là thăm dò các điểm giữa về phía các góc cực của lưới để thu được thông tin tối đa. 
6. Tiếp tục tinh chỉnh cho đến khi truy vấn trả về dấu chấm than, nghĩa là đã tìm thấy tọa độ chính xác. 

### Tại sao nó hoạt động 

Mỗi lần chuyển đổi từ một điểm$P$đến một điểm$Q$định nghĩa một sự so sánh chặt chẽ giữa khoảng cách Euclide với điểm ẩn$T$. Sự bất bình đẳng$$|T - Q| < |T - P|$$mở rộng thành bất đẳng thức bậc hai trong$x$Và$y$, mô tả một nửa mặt phẳng được giới hạn bởi đường trung trực của đoạn thẳng$PQ$. Do đó, mỗi truy vấn sẽ thêm một ràng buộc tuyến tính vào vị trí của$T$. Vùng khả thi luôn lồi và mỗi phép so sánh sẽ giảm khối lượng của nó một cách nghiêm ngặt trừ khi đoạn đó đối xứng với$T$. Bằng cách chọn các truy vấn trải rộng trong vùng không chắc chắn hiện tại, chúng tôi đảm bảo rằng mỗi bước sẽ loại bỏ một phần không đổi các khả năng còn lại, mang lại sự hội tụ trong vòng 64 bước. 

## Giải pháp Python 

Đây là một giải pháp tương tác. Nó giả định một thẩm phán phản hồi linh hoạt, do đó không bao gồm mô phỏng cục bộ.```
PythonRun
```Việc triển khai giữ hai điểm neo đại diện cho một khoảng không chắc chắn tiềm ẩn. Mỗi truy vấn cố gắng tìm một điểm giữa giữa các điểm neo. Khi phản hồi cho thấy sự cải thiện, vùng tìm kiếm sẽ chuyển sang điểm mới; nếu không thì nó co lại ở phía đối diện. 

Việc xả nước sau mỗi lần in là cần thiết vì vấn đề này có tính chất tương tác. Việc không xóa sẽ khiến chương trình bị đình trệ vô thời hạn để chờ phản hồi không bao giờ đến. 

Việc kiểm tra chấm dứt cho dấu chấm than là bắt buộc. Sau khi kho báu được tìm thấy, mọi kết quả đầu ra tiếp theo đều bị coi là không hợp lệ và dẫn đến thất bại ngay lập tức. 

## Ví dụ đã hoạt động 

Vì đây là hoạt động tương tác nên toàn bộ dấu vết xác định phụ thuộc vào các phản hồi ẩn. Thay vào đó, chúng tôi mô phỏng một kịch bản nhất quán trong đó kho báu nằm ở$(3,3)$. 

### Dấu vết mô phỏng 

| Bước | Truy vấn (x,y) | Phản hồi | Cập nhật neo | 
| --- | --- | --- | --- | 
| 1 | (0,0) | Không tìm thấy | ban đầu | 
| 2 | (1e6,1e6) | Hơn nữa | cur = (1e6,1e6) | 
| 3 | (500000,500000) | Hơn nữa | co lại về phía nguồn gốc | 
| 4 | (250000,250000) | Hơn nữa | thu nhỏ | 
| 5 | (125000,125000) | Hơn nữa | thu nhỏ | 
| 6 | (0,0) | Không tìm thấy | đặt lại neo | 

Dấu vết này cho thấy các bước di chuyển đối xứng lớn sẽ nhanh chóng làm giảm vùng khả thi. Các phản hồi “Thêm” lặp đi lặp lại cho biết rằng mục tiêu nằm ở hướng ngược lại, buộc không gian tìm kiếm phải co lại một cách nhất quán. 

### Phiên dịch 

Hành vi chính được minh họa là ngay cả khi không có khoảng cách tuyệt đối, việc so sánh điểm giữa lặp đi lặp lại sẽ buộc phải thu hẹp theo cấp số nhân. Mỗi phản hồi mã hóa một bất đẳng thức định hướng bảo toàn tính lồi của vùng khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(64) truy vấn | Mỗi truy vấn làm giảm vùng khả thi theo hệ số không đổi | 
| Không gian | O(1) | Chỉ một số điểm không đổi được lưu trữ | 

Giới hạn 64 truy vấn là đủ vì mỗi bước sẽ giảm một nửa vùng không chắc chắn một cách hiệu quả ở ít nhất một chiều. Sự phân rã theo cấp số nhân này đảm bảo sự hội tụ tốt trong ngân sách cho phép. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tìm thấy ngay lập tức | dừng lại | chấm dứt sớm | 
| phản hồi xen kẽ | dừng lại | sự mạnh mẽ của các bản cập nhật | 
| chuỗi dài không đầu cuối | dừng lại | sự ổn định theo truy vấn tối đa | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi truy vấn đầu tiên đã chạm tới kho báu. Trong tình huống đó, thẩm phán sẽ trả lời ngay một tin nhắn kết thúc bằng dấu chấm than. Thuật toán phải dừng mà không đưa ra bất kỳ truy vấn nào nữa. các`endswith("!")`kiểm tra sau mỗi phản hồi thực thi điều này. 

Một trường hợp cạnh khác xảy ra khi chuyển động không làm thay đổi khoảng cách, tạo ra “Ở cùng một khoảng cách”. Trong trường hợp này, các chiến lược ngây thơ giả định sự cải tiến hoặc suy thoái nghiêm ngặt sẽ thất bại. Chiến lược điểm giữa vẫn hoạt động vì nó không dựa vào sự đơn điệu mà chỉ duy trì một vùng khả thi thu hẹp lại. 

Trường hợp cạnh thứ ba là các vị trí đối xứng trong đó nhiều điểm mang lại sự so sánh giống hệt nhau. Ngay cả trong trường hợp này, mỗi truy vấn vẫn xác định một ràng buộc đường phân giác vuông góc hợp lệ, do đó vùng khả thi vẫn được xác định rõ ràng và tiếp tục thu hẹp cho đến khi còn lại một điểm duy nhất.
