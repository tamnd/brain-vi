---
title: "CF 102565A - Hiện vật"
description: "Chúng ta có thể xem các tạo phẩm dưới dạng các đỉnh của đồ thị có hướng. Một cặp x - y có nghĩa là tạo tác x có thể được theo sau bởi tạo tác y khi xây dựng một câu thần chú. Một câu thần chú chỉ đơn giản là một đường dẫn có hướng chứa ít nhất hai đỉnh khác nhau."
date: "2026-08-06T20:42:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102565
codeforces_index: "A"
codeforces_contest_name: "AGM 2020, Final Round, Day 2"
rating: 0
weight: 102565
solve_time_s: 58
verified: false
draft: false
---

[CF 102565A - Hiện vật](https://codeforces.com/problemset/problem/102565/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có thể xem các tạo phẩm dưới dạng các đỉnh của đồ thị có hướng. một cặp`x -> y`có nghĩa là hiện vật đó`x`có thể được theo sau bởi tạo tác`y`khi xây dựng một câu thần chú. Một câu thần chú chỉ đơn giản là một đường dẫn có hướng chứa ít nhất hai đỉnh khác nhau. 

Quan sát quan trọng là người bạn không cần phải sở hữu toàn bộ con đường. Nếu hai hiện vật xuất hiện ở bất kỳ đâu trên cùng một con đường được định hướng, việc sở hữu hai hiện vật đó là đủ để người bạn tạo lại câu thần chú hoàn chỉnh. Trong thuật ngữ đồ thị, hai đỉnh không thể cho cùng nhau nếu một trong hai đỉnh có thể chạm tới đỉnh kia. 

Nhiệm vụ là tìm tập đỉnh lớn nhất có thể sao cho không có hai đỉnh được chọn nào được kết nối bằng khả năng tiếp cận theo một trong hai hướng. Đây là antichain lớn nhất trong mối quan hệ khả năng tiếp cận. 

Đồ thị chứa tối đa 3000 đỉnh và 20000 cạnh có hướng. Một giải pháp kiểm tra mọi tập hợp con của hiện vật là không thể vì có 2 3000 lựa chọn khả thi. Ngay cả các thuật toán kiểm tra lặp đi lặp lại từng cặp đỉnh cũng phải được thiết kế cẩn thận, bởi vì mối quan hệ về khả năng tiếp cận là toàn cục chứ không chỉ dựa trên các cạnh ban đầu. Với 3000 đỉnh, quá trình tiền xử lý khối gần đến giới hạn thực tế, trong khi mọi thứ theo cấp số nhân đều bị loại trừ hoàn toàn. 

Một số chi tiết có thể phá vỡ một giải pháp đơn giản hơn. Đầu tiên, các cạnh trực tiếp là không đủ. Một cặp hiện vật có thể không có ranh giới giữa chúng nhưng vẫn xung đột trên một con đường dài hơn. 

Ví dụ:```
3 2
1 2
2 3
```Câu trả lời đúng là`1`. Một giải pháp bất cẩn chỉ kiểm tra các cạnh trực tiếp có thể chọn các tạo tác`1`Và`3`, nhưng tạo tác`1`chạm tới hiện vật`3`, nên hai hiện vật đó không thể cùng tồn tại. 

Thứ hai, chu kỳ yêu cầu xử lý đặc biệt. Coi như:```
3 3
1 2
2 3
3 1
```Câu trả lời đúng là`1`. Mỗi hiện vật có thể tiếp cận mọi hiện vật khác, vì vậy chỉ có thể chọn tối đa một hiện vật. Việc coi biểu đồ như một DAG bình thường mà không nén các thành phần được kết nối mạnh sẽ bỏ sót điều này. 

Thứ ba, các hiện vật biệt lập luôn là những lựa chọn hợp lệ. Ví dụ:```
4 1
1 2
```Câu trả lời đúng là`3`. Hiện vật`3`Và`4`không tham gia vào bất kỳ phép thuật nào và cả hai đều có thể được cho đi cùng với một điểm cuối của cạnh nếu được lựa chọn cẩn thận. Một giải pháp chỉ tính các đỉnh xuất hiện ở các cạnh sẽ đánh giá thấp câu trả lời. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi tập hợp hiện vật có thể có và xác minh xem nó có chứa cặp xung đột hay không. Điều này đúng vì câu trả lời hợp lệ chính xác là một tập hợp con không có cặp đỉnh nào liên quan đến khả năng tiếp cận. Tuy nhiên, số lượng tập hợp con tăng theo cấp số nhân nên nó không thể xử lý được dù chỉ vài chục hiện vật. 

Một lực lượng ít cực đoan hơn trước tiên sẽ tính toán khả năng tiếp cận và sau đó tham lam thêm các tạo phẩm trong khi tránh xung đột. Vấn đề với ý tưởng này là tập hợp lệ lớn nhất không nhất thiết phải đạt được bằng cách đặt hàng tham lam. Cấu trúc này là một tập hợp được sắp xếp một phần, trong đó các lựa chọn cục bộ có thể chặn một giải pháp cuối cùng lớn hơn. 

Sự chuyển đổi hữu ích đến từ việc nhìn vào cấu trúc biểu đồ. Đầu tiên, các đỉnh bên trong cùng một thành phần được kết nối mạnh có thể truy cập được lẫn nhau, vì vậy chúng ta có thể giữ lại tối đa một tạo phẩm từ mỗi thành phần. Sau khi thu gọn mọi thành phần được kết nối mạnh, biểu đồ còn lại là DAG. Khả năng tiếp cận trong DAG này xác định thứ tự một phần. 

Bây giờ vấn đề trở thành việc tìm ra antichain lớn nhất trong DAG. Định lý Dilworth đưa ra một công thức tương đương: kích thước của phản chuỗi tối đa bằng số lượng chuỗi tối thiểu cần thiết để bao phủ tất cả các đỉnh. Đối với DAG, điều này có thể được tính như sau: 

câu trả lời=C−khớp tối đa 

ở đâu`C`là số nút sau khi nén SCC. Việc so khớp được xây dựng trên biểu đồ lưỡng cực chứa hai bản sao của mỗi nút DAG. Chúng tôi thêm một cạnh từ bản sao bên trái của`u`vào bản sao bên phải của`v`bất cứ khi nào`u`có thể đạt được`v`. 

Lực lượng vũ phu không thành công vì nó cố gắng suy luận về tất cả các tập hợp độc lập có thể có. Việc quan sát thấy các xung đột tạo thành một trật tự một phần cho phép chúng ta thay thế bài toán bằng một phép tính khớp cực đại tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2 N ⋅N 2 ) | O(N 2 ) | Quá chậm | 
| SCC + Đóng cửa chuyển tiếp + Kết hợp | O(N 3 +V 2 V ​ ) | O(N 2 ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính các thành phần liên thông mạnh của đồ thị có hướng bằng thuật toán Tarjan. 

Mọi cặp đỉnh bên trong một thành phần được kết nối mạnh mẽ đều có thể chạm tới nhau, vì vậy người bạn không bao giờ có thể nhận được hai hiện vật từ cùng một thành phần. Sau khi nén, mỗi thành phần đại diện cho một lựa chọn có thể có. 
2. Xây dựng DAG cô đọng. 

Mỗi thành phần được kết nối mạnh mẽ sẽ trở thành một nút duy nhất. Nếu có một cạnh ban đầu giữa hai thành phần khác nhau, hãy thêm một cạnh giữa các nút DAG tương ứng. 

Biểu đồ cô đọng không có chu trình, giúp xử lý khả năng tiếp cận dễ dàng hơn. 
3. Tính toán khả năng tiếp cận giữa mỗi cặp thành phần. 

Vì số lượng thành phần nhiều nhất là 3000 nên việc truyền tải DAG dựa trên bitset là đủ. Xử lý các thành phần theo thứ tự tôpô đảo ngược và hợp nhất các nhóm hàng xóm đi có thể tiếp cận được. 

Sau bước này, chúng ta biết chính xác cặp thành phần nào không thể chọn được cả hai. 
4. Tạo đồ thị lưỡng cực sử dụng định lý Dilworth. 

Tạo bản sao bên trái và bên phải của mọi thành phần. Đối với mỗi cặp`(u, v)`Ở đâu`u`có thể đạt được`v`, thêm một cạnh từ bản sao bên trái của`u`vào bản sao bên phải của`v`. 

Sự kết hợp thể hiện số lượng nút có thể được hợp nhất thành chuỗi. 
5. Chạy kết hợp tối đa Hopcroft-Karp trên biểu đồ hai bên này. 

Số lượng thành phần chưa từng có sau khi áp dụng định lý là kích thước của antichain lớn nhất. 
6. Xuất giá trị: 

số lượng thành phần − kích thước phù hợp tối đa 

Tại sao nó hoạt động: 

Sau khi nén SCC, mỗi nút còn lại đại diện cho một tập hợp trong đó việc chọn nhiều hơn một đỉnh là không thể, do đó mỗi thành phần đóng góp nhiều nhất một tạo phẩm. Biểu đồ cô đọng là DAG và khả năng tiếp cận xác định thứ tự một phần. Một câu trả lời hợp lệ chính xác là một phản chuỗi theo thứ tự một phần này vì không thành phần nào được chọn có thể đạt tới thành phần được chọn khác. 

Định lý Dilworth phát biểu rằng kích thước phản chuỗi lớn nhất bằng kích thước phân hủy chuỗi tối thiểu. Đối với DAG, phân tách chuỗi tối thiểu được tính bằng cách lấy số lượng nút và trừ đi mức khớp tối đa trong biểu đồ hai bên khả năng tiếp cận. Do đó, thuật toán trả về chính xác số lượng hiện vật tối đa có thể được cho đi một cách an toàn.
