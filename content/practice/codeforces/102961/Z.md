---
title: "CF 102961Z - Giá trị nhỏ hơn gần nhất"
description: "Chúng ta được cung cấp một dãy số và với mỗi vị trí, chúng ta muốn tìm một “giá trị nhỏ gần nhất” xuất hiện trước nó."
date: "2026-07-04T06:58:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "Z"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 29
verified: false
draft: false
---

[CF 102961Z - Giá trị nhỏ hơn gần nhất](https://codeforces.com/problemset/problem/102961/Z) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và với mỗi vị trí, chúng ta muốn tìm một “giá trị nhỏ gần nhất” xuất hiện trước nó. Nói cách khác, khi đứng trước một phần tử, chúng ta nhìn sang bên trái và cố gắng xác định phần tử gần nhất trước đó nhỏ hơn phần tử hiện tại. Nếu không có phần tử nào như vậy tồn tại thì câu trả lời cho vị trí đó là 0. 

Do đó, đầu ra là một chuỗi có cùng độ dài, trong đó mỗi mục nhập mã hóa vị trí (hoặc giá trị, tùy thuộc vào quy ước lập chỉ mục) của phần tử nhỏ hơn gần nhất ở bên trái hoặc bằng 0 khi phía bên trái không chứa gì nhỏ hơn. 

Các ràng buộc điển hình cho loại nhiệm vụ này ngụ ý kích thước mảng lớn, thường lên tới một trăm nghìn phần tử. Điều đó ngay lập tức loại trừ các chiến lược quét bậc hai. Một vòng lặp lồng nhau, đối với mỗi vị trí, đi lùi qua tất cả các phần tử trước đó sẽ thực hiện theo thứ tự so sánh n2 trong trường hợp xấu nhất, vượt xa mức phù hợp với giới hạn thời gian khoảng hai giây. Giải pháp khả thi duy nhất là những giải pháp duy trì cấu trúc tăng dần khi chúng ta duyệt mảng một lần. 

Việc triển khai đơn giản cũng có xu hướng thất bại khi tất cả các phần tử trước đó lớn hơn phần tử hiện tại. Trong trường hợp đó, nó có thể trả về không chính xác phần tử trước gần nhất thay vì nhận dạng chính xác rằng không tồn tại phần tử hợp lệ nhỏ hơn. Ví dụ, ở đầu vào`[5, 4, 3]`, đầu ra đúng là`[0, 0, 0]`. Bất kỳ cách tiếp cận nào chỉ theo dõi khoảng cách mà không thực thi điều kiện “nhỏ hơn hiện tại” sẽ tạo ra kết quả không chính xác.`[0, 1, 2]`hoặc dự phòng vị trí tương tự. 

Một trường hợp cạnh phổ biến khác xuất hiện theo trình tự tăng nghiêm ngặt như`[1, 2, 3, 4]`. Mọi phần tử ngoại trừ phần tử đầu tiên đều có phần tử nhỏ hơn gần nhất hợp lệ và phần tử nhỏ hơn đó luôn liền kề ngay lập tức. Một giải pháp đúng phải bảo toàn vị trí này mà không cần quét lại toàn bộ tiền tố. 

## Phương pháp tiếp cận 

Chiến lược vũ phu rất đơn giản. Với mỗi chỉ số i, chúng ta quét ngược từ i − 1 xuống 0 và dừng ở phần tử đầu tiên nhỏ hơn giá trị hiện tại. Điều này đúng vì nó kiểm tra rõ ràng tất cả các ứng cử viên theo thứ tự gần nhau và đảm bảo kết quả phù hợp hợp lệ đầu tiên thực sự là kết quả gần nhất. 

Vấn đề với cách tiếp cận này là nó liên tục kiểm tra lại các yếu tố giống nhau. Trong trường hợp xấu nhất là mảng giảm dần, mọi phần tử sẽ quét tất cả các phần tử trước đó. Điều này dẫn đến sự so sánh khoảng n(n − 1)/2, là hành vi bậc hai và trở nên quá chậm đối với đầu vào lớn. 

Quan sát quan trọng là không phải tất cả các yếu tố trước đó vẫn hữu ích khi chúng ta tiếp tục. Nếu chúng ta duy trì một cấu trúc gồm các ứng cử viên mà chúng ta chưa tìm thấy “phần tử lớn hơn hoặc bằng tiếp theo” thì nhiều phần tử có thể bị loại bỏ vĩnh viễn. Cụ thể, khi chúng ta thấy một phần tử mới, bất kỳ phần tử nào trước đó lớn hơn hoặc bằng nó không bao giờ có thể nhỏ hơn gần nhất đối với bất kỳ phần tử nào trong tương lai ở bên phải của nó. Điều này gợi ý việc duy trì một cấu trúc chỉ giữ lại chuỗi ứng viên giảm dần. 

Một ngăn xếp đơn điệu sẽ nắm bắt được ý tưởng này một cách tự nhiên. Chúng ta giữ chỉ số của các phần tử trong một ngăn xếp sao cho giá trị của chúng tăng dần từ dưới lên trên. Khi một phần tử mới xuất hiện, chúng tôi loại bỏ tất cả các phần tử không nhỏ hơn nó khỏi ngăn xếp vì chúng vô ích cho việc so sánh trong tương lai. Sau khi dọn dẹp, phần trên cùng của ngăn xếp (nếu có) là phần tử nhỏ hơn gần nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Ngăn xếp đơn điệu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì một chồng chỉ số ứng cử viên. 

1. Khởi tạo một ngăn xếp trống. Mỗi phần tử trong ngăn xếp đại diện cho một chỉ mục có giá trị có thể là phần tử nhỏ hơn gần nhất cho các vị trí trong tương lai. 
2. Lặp qua mảng
