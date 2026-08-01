---
title: "CF 102623D - Khôi phục thảm họa"
description: "Chúng ta có một đồ thị vô hướng với các thành phố là các đỉnh và các con đường được đề xuất là các cạnh. Chi phí của một con đường giữa thành phố x và y là tổng của hai giá trị gắn liền với các điểm cuối của nó, cụ thể là giá trị Fibonacci của thành phố x cộng với giá trị Fibonacci của thành phố y."
date: "2026-08-01T08:56:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102623
codeforces_index: "D"
codeforces_contest_name: "2020 Lenovo Cup USST Campus Online Invitational Contest"
rating: 0
weight: 102623
solve_time_s: 90
verified: false
draft: false
---

[CF 102623D - Khắc phục thảm họa](https://codeforces.com/problemset/problem/102623/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng với các thành phố là các đỉnh và các con đường được đề xuất là các cạnh. Chi phí đường bộ giữa các thành phố`x`Và`y`là tổng của hai giá trị gắn liền với điểm cuối của nó, cụ thể là giá trị Fibonacci của thành phố`x`cộng với giá trị Fibonacci của thành phố`y`. 

Mục tiêu đầu tiên là chọn những con đường kết nối mọi thành phố trong khi chi tiêu số tiền tối thiểu có thể. Đây chính xác là bài toán cây bao trùm tối thiểu. Tuy nhiên, có một yêu cầu bổ sung: trong số tất cả các cây bao trùm có chi phí tối thiểu, chúng ta phải chọn cây có độ thành phố lớn nhất càng nhỏ càng tốt. Đầu ra là mức tối đa nhỏ nhất có thể. 

Cấu trúc quan trọng đến từ hình dạng đặc biệt của trọng lượng cạnh. Các giá trị Fibonacci tăng theo chỉ số thành phố và trọng số của một cạnh chỉ được xác định bởi hai điểm cuối của nó. Các ràng buộc cho phép lên đến`100000`thành phố và`200000`những con đường. Một thuật toán bậc hai đã có sẵn`10^10`hoạt động vượt xa giới hạn. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(m log n)`. 

Một sai lầm phổ biến là xây dựng bất kỳ cây khung nhỏ nhất nào rồi đo bậc lớn nhất của nó. Điều đó là chưa đủ vì các MST khác nhau có thể có mức độ phân bổ khác nhau. Một sai lầm khác là chọn bất kỳ cạnh nào kết nối một thành phố mới với một thành phần đã được kết nối. Điều đó có thể phá vỡ điều kiện chi phí tối thiểu vì điểm cuối rẻ hơn có thể tồn tại bên trong cùng một thành phần. 

Hãy xem xét ví dụ nhỏ này:```
4 3
1 2
1 3
2 4
```Câu trả lời là`2`. 

Khi thành phố 4 được thêm vào, nó chỉ có thể có một kết nối duy nhất, do đó cạnh của nó bị ép buộc. Một phương pháp cố gắng cân bằng độ mà không tôn trọng trọng số cạnh có thể chọn một cạnh khác nếu có, nhưng điều đó có thể làm tăng tổng chi phí và không còn là MST nữa. 

Một trường hợp quan trọng khác là khi một số cạnh kết nối cùng một thành phố mới với cùng một thành phần đã được kết nối.```
5 5
1 2
1 3
2 4
3 4
1 5
```Khi thành phố 4 được xử lý, cả hai`(2,4)`Và`(3,4)`kết nối cùng một thành phần. Cái rẻ hơn phải được chọn vì chi phí MST được ưu tiên. Chỉ sau khi ấn định được chi phí, chúng ta mới có thể sử dụng phần tự do còn lại để giảm độ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chạy thuật toán Kruskal, sắp xếp tất cả các cạnh theo trọng số tổng Fibonacci của chúng và xây dựng một MST. Điều này đúng khi tìm chi phí tối thiểu vì Kruskal luôn tạo ra MST. 

Tuy nhiên, nó không giải quyết được toàn bộ vấn đề. Các lựa chọn có chi phí bằng nhau hoặc các lựa chọn MST hợp lệ khác nhau có thể dẫn đến mức độ tối đa khác nhau và việc thử tất cả các MST có thể là không thể. Trong trường hợp xấu nhất có thể có nhiều cây bao trùm theo cấp số nhân. 

Quan sát quan trọng là trọng lượng cạnh có một thứ tự đặc biệt. Đối với bất kỳ cạnh nào`(u,v)`với`u < v`, trọng số của nó bị chi phối bởi điểm cuối lớn hơn. Mọi cạnh có điểm cuối lớn hơn nhỏ hơn`v`được xử lý trước mọi cạnh có điểm cuối lớn hơn lớn hơn`v`. Điều này có nghĩa là chúng ta có thể nghĩ về việc xây dựng MST theo thành phố theo thứ tự chỉ số tăng dần. 

Khi thành phố`v`được xem xét, tất cả các thành phố có chỉ số nhỏ hơn đã hình thành một số thành phần được kết nối. Mỗi cạnh liên quan`v`đi từ`v`vào một trong các thành phần đó. Để giữ tổng chi phí ở mức tối thiểu, đối với mọi thành phần trước đó phải được kết nối với`v`, chúng ta phải chọn cạnh rẻ nhất từ`v`vào thành phần đó. Từ`v`được cố định, điều này có nghĩa là chọn hàng xóm được lập chỉ mục nhỏ nhất bên trong thành phần đó. 

Nếu một số cạnh có cùng chi phí thì chúng có thể thay thế cho chi phí MST. Chỉ những mối quan hệ đó mới quan trọng đối với mục tiêu thứ hai, vì vậy trong số các ứng cử viên bằng nhau, chúng tôi chọn đỉnh có bậc nhỏ nhất hiện tại. Lựa chọn cục bộ này là đủ vì các quyết định trong tương lai chỉ phụ thuộc vào giá trị cấp độ bên trong mỗi thành phần đã được xây dựng sẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các lựa chọn MST | Hàm mũ | O(n+m) | Quá chậm | 
| Kruskal cộng với xử lý thứ cấp | O(m log m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm mọi con đường theo điểm cuối lớn hơn của nó. Khi xử lý thành phố`v`, chỉ những con đường kết thúc tại`v`là cần thiết vì tất cả các con đường có điểm cuối nhỏ hơn và lớn hơn đều đã được xử lý. 
2. Duy trì cơ cấu liên minh rời rạc cho các thành phố có chỉ số nhỏ hơn hoặc bằng thành phố hiện tại. Các thành phần đại diện cho những thành phố cũ nào đã được kết nối trong MST một phần. 
3. Xử lý các thành phố từ`1`ĐẾN`n`. Đối với thành phố hiện tại`v`, kiểm tra mọi hàng xóm có chỉ số nhỏ hơn. Tìm thành phần DSU của hàng xóm đó. 
4. Đối với mọi thành phần liền kề`v`, chỉ giữ lại cạnh ứng cử viên tốt nhất. Ứng cử viên tốt nhất là điểm có điểm cuối khác có chỉ số nhỏ nhất, vì điểm đó mang lại giá trị Fibonacci nhỏ nhất và do đó có cạnh rẻ nhất. Nếu nhiều ứng viên có cùng chỉ số, hãy sử dụng chỉ số có mức độ hiện tại nhỏ hơn. 
5. Thêm một cạnh được chọn từ`v`tới từng thành phần lân cận. Tăng mức độ của cả hai điểm cuối. Số cạnh được chọn chính xác là số thành phần cũ`v`phải hợp nhất với. 
6. Hợp nhất`v`và tất cả các thành phần được chạm vào trong DSU. Cây một phần hiện đã chính xác cho tiền tố kết thúc tại`v`. 
7. Sau khi tất cả các thành phố được xử lý, câu trả lời là mức độ lớn nhất trong số tất cả các thành phố. 

Điều bất biến là sau khi xử lý thành phố`v`, các cạnh được chọn tạo thành một khu rừng bao trùm chi phí tối thiểu của biểu đồ do các thành phố tạo ra`1`bởi vì`v`. Lựa chọn duy nhất có thể ảnh hưởng đến sự phân bổ mức độ cuối cùng là các cạnh có chi phí bằng nhau, bởi vì việc chọn cạnh đắt hơn sẽ vi phạm mục tiêu chính. Trong số các lựa chọn có chi phí bằng nhau đó, việc chọn điểm cuối hiện được tải ít nhất sẽ giữ mức tối đa ở mức nhỏ nhất có thể. 

Phần tiếp theo bao gồm cách triển khai, chi tiết chứng minh, ví dụ, kiểm tra và thảo luận về độ phức tạp.
