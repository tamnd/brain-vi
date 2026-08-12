---
title: "CF 104027B - Kẹo"
description: "Chúng tôi được tặng một bộ sưu tập các gói kẹo. Mỗi gói chứa 2 viên kẹo hoặc 3 viên kẹo. Nhiệm vụ là xác định xem có thể chia tất cả số kẹo cho ba người sao cho mỗi người nhận được tổng số kẹo bằng nhau hay không."
date: "2026-07-02T04:07:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104027
codeforces_index: "B"
codeforces_contest_name: "The 10-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 104027
solve_time_s: 30
verified: false
draft: false
---

[CF 104027B - Kẹo](https://codeforces.com/problemset/problem/104027/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập các gói kẹo. Mỗi gói chứa 2 viên kẹo hoặc 3 viên kẹo. Nhiệm vụ là xác định xem có thể chia tất cả số kẹo cho ba người sao cho mỗi người nhận được tổng số kẹo bằng nhau hay không. 

Đầu vào mô tả một cách hiệu quả một tập hợp nhiều tập hợp chỉ gồm các giá trị 2 và 3. Đầu ra là một câu trả lời khả thi đơn giản: liệu có cách nào để phân chia tất cả các gói này thành ba nhóm sao cho tổng trong mỗi nhóm giống hệt nhau hay không. 

Ràng buộc về cấu trúc đầu tiên xuất phát từ tổng số tiền. Nếu tổng số kẹo là`S`, thì điều kiện cần là`S`chia hết cho 3. Ngược lại, không tồn tại phân vùng bằng nhau bất kể sự sắp xếp như thế nào. 

Ràng buộc thứ hai mang tính tổ hợp hơn là số học. Kể cả nếu`S / 3`là một số nguyên, chúng ta phải đảm bảo rằng bội số của 2 và 3 có thể được chia thành ba tập con, mỗi tập có tổng bằng`S / 3`. Đây là một bài toán phân vùng tổng tập hợp con bị ràng buộc với bảng chữ cái rất nhỏ về trọng số mục. 

Một trường hợp phức tạp xuất hiện khi lý luận tham lam dựa trên số lượng 2 và 3 giây được sử dụng mà không xem xét các tương tác giữa các nhóm. Ví dụ, hãy xem xét các gói`[3, 3, 2, 2, 2]`. Tổng số tiền là`12`, vì vậy mỗi người phải nhận được`4`. Một nỗ lực ngây thơ có thể cố gắng nhóm`(3+1)`điều này là không thể, hoặc xử lý 2 và 3 một cách độc lập, không nhận thấy rằng các phân vùng hợp lệ yêu cầu trộn cả hai loại. 

Một trường hợp đặc biệt khác là khi tổng tổng chia hết cho 3, nhưng không tồn tại phân vùng do ràng buộc không thể chia hết. Ví dụ,`[3, 3, 3, 2]`có tổng`11`, đã không chia hết được, nhưng ngay cả các biến thể như`[3, 3, 2, 2]`yêu cầu suy luận bài tập cẩn thận. 

Khó khăn chính không phải là mục tiêu số học mà là liệu các khối 2 và 3 rời rạc có thể được sắp xếp đồng thời thành ba tổng bằng nhau hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là coi đây là vấn đề phân vùng. Vì mỗi người phải nhận chính xác`S / 3`kẹo, chúng ta có thể nghĩ đến việc gán mỗi gói vào một trong ba thùng và kiểm tra xem liệu phép gán nào có hiệu quả hay không. 

Một ý tưởng mạnh mẽ là liệt kê tất cả các cách để phân từng nhóm cho một trong ba người. Với`n`gói, điều này dẫn đến`3^n`khả năng đó là quá lớn. 

Một lực lượng vũ phu có cấu trúc hơn một chút nhận thấy rằng tổng số tiền trên mỗi người là cố định, vì vậy chúng ta chỉ cần xem xét các kết hợp tập hợp con. Người ta có thể liệt kê tất cả các tập hợp con của các gói có tổng bằng`S / 3`. Giả sử có`L`các tập hợp con như vậy. Sau đó, chúng tôi thử tất cả các bộ ba tập hợp con để xem liệu chúng có tạo thành một phân vùng hay không. Điều này dẫn đến`O(L^3)`séc. Trong trường hợp xấu nhất,`L`bản thân nó có thể là hàm mũ, vì tổng tập hợp con trên các trọng số nhỏ tạo ra nhiều kết hợp. 

Quan sát quan trọng là chúng ta thực sự không cần phải suy luận về các phép gán riêng lẻ ở cấp độ các tập hợp con tùy ý. Vì tất cả các trọng số chỉ có 2 hoặc 3 nên cấu trúc bị hạn chế nhiều. Bất kỳ giải pháp hợp lệ nào cũng tương đương với việc nhóm một số gói 3 cái lại với nhau và một số gói 2 cái lại với nhau, thỉnh thoảng trộn, nhưng việc trộn có thể được chuẩn hóa. 

Một cách rõ ràng hơn để xem nó là sửa lỗi chia sẻ của một người`T = S / 3`. Chúng ta chỉ cần quyết định mỗi người sẽ có bao nhiêu gói 3 và 2 gói. Cho phép`x`là số lượng 2 gói được chỉ định và`y`là số lượng 3 gói được chỉ định. Khi đó mỗi người phải thỏa mãn`2x + 3y = T`. 

Vì vậy, vấn đề trở thành: liệu chúng ta có thể chia tất cả 2 gói và 3 gói thành ba nhóm, mỗi nhóm có cùng một phương trình tuyến tính không? Điều này làm giảm vấn đề phân vùng toàn cục thành việc kiểm tra xem số lượng 2 và 3 có thể được phân tách một cách nhất quán trên ba biểu diễn tuyến tính giống hệt nhau hay không. 

Thay vì tìm kiếm trong các bài tập, chúng tôi liệt kê tất cả các giá trị hợp lệ`(x, y)`giải pháp cho một người, sau đó kiểm tra xem liệu ba giải pháp như vậy có thể tiêu thụ chung tất cả các mặt hàng hay không. Điều này làm giảm đáng kể không gian trạng thái vì phương trình`2x + 3y = T`chỉ có`O(T)`giải pháp, và trong thực tế rất ít kể từ khi`x`bị giới hạn và tính chẵn lẻ hạn chế tính khả thi. 

Sau đó, chúng tôi kiểm tra tất cả các bộ ba của các giải pháp ứng cử viên này để xem liệu chúng có bao gồm chính xác số 2 và 3 có sẵn hay không. 

Điều này biến một bài toán phân vùng tổ hợp thành một phép liệt kê giới hạn trên các phân rã tuyến tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Liệt kê (x, y) bộ ba | O(L³), L nhỏ do ràng buộc | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số tiền`S`của tất cả các loại kẹo. Nếu như`S % 3 != 0`, hãy dừng ngay vì không thể phân chia bằng nhau. 
2. Đặt mục tiêu cho mỗi người`T = S / 3`. Đây là số tiền cố định mà mỗi nhóm trong số ba nhóm phải đạt được. 
3. Hãy để`c2`là số lượng 2 gói và`c3`là số lượng 3 gói. Bây giờ chúng ta mô tả sự lựa chọn của một người là sự lựa chọn`x`hai và`y`ba như vậy`2x + 3y = T`. 
4. Liệt kê tất cả các cặp`(x, y)`thỏa mãn các ràng buộc về phương trình và tính khả thi`0 <= x <= c2`Và`0 <= y <= c3`. Mỗi cặp đại diện cho một cách có thể hình thành một người. 
5. Lưu trữ tất cả hợp lệ`(x, y)`cặp trong một danh sách. Mỗi ứng cử viên đại diện cho sự phân rã của một phần ba phân vùng cuối cùng. 
6. Thử tất cả bộ ba ứng viên`(i, j, k)`từ danh sách này. Đối với mỗi bộ ba, hãy kiểm tra xem:`x_i + x_j + x_k = c2`Và`y_i + y_j + y_k = c3`. 

Điều này đảm bảo rằng cả 2 gói và 3 gói đều được sử dụng chính xác một lần cho cả ba người. 
7. Nếu bất kỳ bộ ba nào thỏa mãn cả hai phương trình, trả về “CÓ”. Ngược lại trả về “KHÔNG”. 

### Tại sao nó hoạt động 

Bất kỳ phân vùng hợp lệ nào đều dẫn đến sự lựa chọn số lượng 2 gói và 3 gói mà mỗi người nhận được, do đó nó tương ứng với ba cặp`(x, y)`đã ngồi
