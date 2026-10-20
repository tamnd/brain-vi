---
title: "CF 105712I - Hoán đổi Domino"
description: "Chúng ta có một lưới có kích thước $n lần m$ chứa hai màu đen và trắng. Mục tiêu là chuyển đổi cấu hình bắt đầu thành cấu hình đích bằng cách sử dụng một thao tác cụ thể được áp dụng cho các ô liền kề."
date: "2026-06-26T07:57:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105712
codeforces_index: "I"
codeforces_contest_name: "Rutgers University Programming Contest Fall 2024"
rating: 0
weight: 105712
solve_time_s: 30
verified: false
draft: false
---

[CF 105712I - Hoán đổi Domino](https://codeforces.com/problemset/problem/105712/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới có kích thước$n \times m$gồm hai màu đen và trắng. Mục tiêu là chuyển đổi cấu hình bắt đầu thành cấu hình đích bằng cách sử dụng một thao tác cụ thể được áp dụng cho các ô liền kề. 

Một lần di chuyển sẽ chọn hai ô lân cận có chung một cạnh và có cùng màu tại thời điểm đó, sau đó hoán đổi màu của chúng. Vì cả hai ô hiện có các giá trị giống nhau nên bản thân việc hoán đổi không thay đổi lưới cục bộ theo nghĩa rõ ràng là trao đổi các giá trị khác nhau, mà nó lật cả hai ô, chuyển đen thành trắng hoặc ngược lại cùng một lúc. Do đó, mỗi thao tác là một chuyển đổi chẵn lẻ cục bộ được áp dụng cho một cặp ô có màu bằng nhau liền kề và thách thức là sử dụng các thao tác này để định hình lại sự phân bố màu chung. 

Nhiệm vụ không phải là giảm thiểu các thao tác mà chỉ là xác định xem có thể chuyển đổi được hay không và nếu có thì hãy xây dựng bất kỳ chuỗi hợp lệ nào tối đa là$200nm$hoạt động. 

Các ràng buộc nhỏ cho mỗi trường hợp thử nghiệm, với$n, m \le 50$và tổng số ô trên tất cả các thử nghiệm nhiều nhất là 2500. Điều này ngay lập tức loại trừ mọi thứ siêu tuyến tính cho mỗi trường hợp thử nghiệm là cần thiết, nhưng cũng cho phép các chiến lược xây dựng khá tích cực như lan truyền mô phỏng, BFS trên cấu hình hoặc hiệu chỉnh hệ thống từng ô một. Sự ràng buộc$200nm$là một gợi ý mạnh mẽ rằng tồn tại một quy trình tham lam mang tính xây dựng trong đó mỗi ô được xử lý một số lần không đổi. 

Một điểm tinh tế quan trọng là các phép toán thực hiện trên các cặp ô liền kề có màu bằng nhau. Điều này có nghĩa là chúng tôi không được tự do hoán đổi màu sắc tùy ý giữa các vị trí; thay vào đó, chúng tôi chỉ có thể lật các cặp địa phương phù hợp. Điều này tạo ra các ràng buộc phụ thuộc vào khả năng kết nối và tính chẵn lẻ thay vì việc đếm các ô đen và trắng đơn giản. 

Một số trường hợp khó nhận thấy xuất hiện ngay lập tức. Nếu lưới ban đầu và lưới mục tiêu khác nhau về tổng số chẵn lẻ của các ô đen thì không có chuỗi nào có thể hoạt động, vì mỗi thao tác sẽ lật hai ô và duy trì tính chẵn lẻ của số lượng màu đen. Ví dụ: nếu lưới ban đầu toàn màu trắng và mục tiêu có chính xác một ô đen thì không có chuỗi hợp lệ vì mọi thao tác đều thay đổi số lượng ô đen bằng 0 hoặc 2. 

Một dạng lỗi khác phát sinh khi cố gắng hiệu chỉnh cục bộ một cách tham lam từ trên cùng bên trái: việc lật một ô không khớp bằng cách ghép nó với bất kỳ ô có màu bằng nhau liền kề nào có thể vô tình làm xáo trộn cấu trúc cố định trước đó, vì các hoạt động không độc lập và có thể truyền các thay đổi ngược qua lưới. 

Trường hợp cạnh thứ ba xuất hiện trong các lưới mỏng như$1 \times m$, trong đó kề là tuyến tính. Nếu lưới là một hàng, các hoạt động sẽ trở nên bị hạn chế nhiều và nhiều chiến lược sửa chữa cục bộ dựa vào tính linh hoạt 2D sẽ bị hỏng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ coi lưới là một trạng thái trong một biểu đồ lớn trong đó mỗi trạng thái kết nối với trạng thái khác thông qua việc áp dụng bất kỳ hoán đổi màu bằng nhau hợp lệ nào liền kề. Từ bất kỳ cấu hình nào, chúng tôi sẽ khám phá tất cả các chuỗi hoạt động có thể có bằng cách sử dụng BFS hoặc DFS cho đến khi đạt được lưới mục tiêu. Về nguyên tắc, điều này đúng vì nó trực tiếp mô hình hóa các chuyển đổi được phép. Tuy nhiên, hệ số phân nhánh tỷ lệ thuận với số cặp màu bằng nhau liền kề, tức là$O(nm)$và độ sâu của trình tự cũng có thể là$O(nm)$. Điều này dẫn đến sự bùng nổ theo cấp số nhân về cấu hình có thể tiếp cận, vượt xa những gì có thể sử dụng được ngay cả đối với$n,m=50$. 

Cấu trúc của hoạt động quan trọng hơn kích thước không gian trạng thái. Mỗi lần di chuyển sẽ lật hai ô giống hệt nhau liền kề, giúp duy trì tính chẵn lẻ của các ô màu đen và cũng bảo toàn một bất biến tinh tế hơn: tính chẵn lẻ của các ô màu đen trong bất kỳ thành phần được kết nối nào có màu giống hệt nhau theo cách diễn giải động của sự hợp nhất. Thay vì suy nghĩ về các chuyển đổi cấu hình toàn cầu, quan điểm đúng đắn là chúng ta được phép “định tuyến” sự mất cân bằng màu sắc qua lưới bằng cách sử dụng các lần lật cặp bằng nhau cục bộ, dần dần đẩy sự khác biệt về một khu vực nơi chúng có thể được giải quyết mà không vi phạm các ràng buộc kề cận. 

Ý tưởng chính giúp mở ra một giải pháp hiệu quả là xử lý từng ô lưới theo một thứ tự cố định và đảm bảo rằng khi chúng ta hoàn thành một ô, ô đó đã khớp với mục tiêu. Bất kỳ sự không khớp nào sẽ được đẩy về phía trước bằng cách sử dụng một chuỗi các thao tác cục bộ để di chuyển hiệu ứng sang ô lân cận chưa được hoàn thiện. Vì mỗi ô chỉ được hoàn thành một lần nên tổng số thao tác vẫn tuyến tính theo$nm$, và công trình không bao giờ cần phải xem lại vị trí cũ. 

Lực lượng vũ phu hoạt động vì nó khám phá tất cả các chuỗi hợp lệ, nhưng không thành công vì biểu đồ trạng thái rất lớn. Cách tiếp cận mang tính xây dựng tham lam có hiệu quả vì hoạt động mang tính cục bộ và có thể đảo ngược theo hướng được kiểm soát, cho phép chúng ta coi lưới như một thứ mà chúng ta có thể “nén” từ trái sang phải hoặc từ trên xuống dưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | hàm mũ | hàm mũ | Quá chậm | 
| Chỉ đạo tham lam tuyên truyền |$O(nm)$|$O(1)$hoặc$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lưới theo thứ tự hàng chính, duy trì tính bất biến rằng mọi thứ trước ô hiện tại đều khớp với lưới mục tiêu. 

Tại mỗi ô$(i, j)$, chúng tôi kiểm tra xem màu hiện tại có khớp với màu mục tiêu hay không. Nếu nó đã khớp, chúng ta tiếp tục mà không làm gì cả. 

Nếu nó không khớp, chúng ta phải sửa nó bằng một thao tác hợp lệ. Vì các phép toán yêu cầu hai ô liền kề có màu bằng nhau nên trước tiên chúng ta xác định một ô lân cận của$(i, j)$có thể tham gia vào một hoạt động và cho phép chúng ta chuyển trạng thái của$(i, j)$trong khi đẩy sự khác biệt về phía trước. 

Việc xây dựng sử dụng ý tưởng tạo hoặc sử dụng ô “đệm” phía trước theo thứ tự truyền tải. Chúng tôi chọn một ô liền kề chưa được cố định, áp dụng thao tác được phép trên cặp và từ đó lật cả hai ô. Điều này khắc phục sự không khớp hiện tại trong khi có khả năng đưa ra sự không khớp trong ô sau này sẽ được xử lý khi chúng tôi tiếp cận nó. 

Chúng tôi luôn chọn một hướng đi
