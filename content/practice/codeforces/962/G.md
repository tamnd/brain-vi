---
title: "CF 962G - Vùng đen có thể nhìn thấy"
description: "Chúng ta có một đa giác trực giao đơn giản, nghĩa là các cạnh của nó nằm ngang hoặc dọc và nó không bao giờ tự cắt nhau. Phần bên trong của đa giác này được coi là "màu đen". Chúng tôi cũng có một cửa sổ hình chữ nhật được căn chỉnh theo trục cố định. Cửa sổ tĩnh và chúng ta chỉ nhìn qua nó."
date: "2026-06-17T01:45:02+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "geometry", "trees"]
categories: ["algorithms"]
codeforces_contest: 962
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 42 (Rated for Div. 2)"
rating: 2800
weight: 962
solve_time_s: 32
verified: false
draft: false
---

[CF 962G - Vùng đen có thể nhìn thấy](https://codeforces.com/problemset/problem/962/G) 

**Đánh giá:** 2800 
**Tags:** cấu trúc dữ liệu, dsu, hình học, cây cối 
**Thời gian giải:** 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác trực giao đơn giản, nghĩa là các cạnh của nó nằm ngang hoặc dọc và nó không bao giờ tự cắt nhau. Phần bên trong của đa giác này được coi là "màu đen". Chúng tôi cũng có một cửa sổ hình chữ nhật được căn chỉnh theo trục cố định. Cửa sổ tĩnh và chúng ta chỉ nhìn qua nó. 

Bên trong cửa sổ, các phần bên trong của đa giác có thể xuất hiện dưới dạng một số vùng màu đen bị ngắt kết nối. Nhiệm vụ là đếm xem có bao nhiêu thành phần được kết nối bên trong đa giác được hiển thị trong cửa sổ. 

Khả năng kết nối theo nghĩa hình học thông thường: hai điểm được kết nối nếu bạn có thể di chuyển giữa chúng trong khi vẫn ở trong vùng màu đen, không vượt qua ranh giới đa giác và ở bên trong cửa sổ. 

Khó khăn chính là đa giác có thể lớn và sự tương tác của nó với hình chữ nhật có thể chia một vùng được kết nối thành nhiều thành phần hiển thị. Điều này xảy ra khi cửa sổ cắt qua các hành lang của đa giác hoặc cắt đi các đoạn kết nối. 

Các ràng buộc cho phép lên tới 15.000 đỉnh và tọa độ lên tới 15.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng rasterize toàn bộ mặt phẳng ở độ phân giải cao và sau đó chạy một lớp lấp đầy đơn giản trên tất cả các ô, vì toàn bộ lưới sẽ có kích thước bậc hai và vẫn cần cắt hình học cẩn thận. Một giải pháp phải hoạt động gần tuyến tính hoặc gần tuyến tính về số cạnh đa giác, cộng với chi phí nhỏ. 

Trường hợp cạnh tinh tế xuất hiện khi cửa sổ giao với ranh giới đa giác theo cách mà các hành lang mỏng tồn tại bên ngoài cửa sổ nhưng cần thiết để kết nối các vùng bên trong nó. Một cách tiếp cận ngây thơ chỉ xem xét các cạnh đa giác có thể nhìn thấy mà không tính toán chính xác khả năng kết nối thông qua các vùng bị cắt bớt sẽ phân chia các thành phần không chính xác. 

Một trường hợp cạnh khác là khi cửa sổ chứa đầy một lối đi hẹp nối hai khu vực lớn, nhưng lối đi đó nằm một phần bên ngoài ranh giới đa giác trong giao điểm cửa sổ. Quá trình quét cục bộ có thể coi hai bên là các thành phần riêng biệt không chính xác nếu nó không tái tạo lại kết nối thực sự của phần bên trong bị cắt bớt. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là rời rạc hóa toàn bộ mặt phẳng tọa độ thành các ô đơn vị và đánh dấu những ô nào nằm bên trong đa giác. Sau đó, chúng tôi giao nhau với cửa sổ và chạy tràn để đếm các thành phần được kết nối. Điều này đơn giản về mặt khái niệm: kiểm tra điểm trong đa giác hoặc điền đường quét có thể đánh dấu các ô bên trong và BFS/DFS có thể đếm các thành phần. 

Tuy nhiên, phạm vi tọa độ lên tới 15.000 theo cả hai hướng. Một lưới đầy đủ sẽ chứa khoảng 225 triệu ô và thậm chí quét tuyến tính trên lưới này cũng quá chậm và quá nặng về bộ nhớ. Quan trọng hơn, việc xây dựng một lưới như vậy một cách chính xác cho một đa giác có tối đa 15.000 cạnh đòi hỏi phải xử lý giao điểm trên mỗi hàng một cách cẩn thận, bản thân việc này rất tốn kém. 

Quan sát quan trọng là đa giác được căn chỉnh theo trục và đơn giản, cho phép chúng ta rút gọn bài toán thành một biểu đồ được xây dựng từ các cạnh và cấu trúc giao nhau của nó với cửa sổ. Thay vì suy luận về diện tích, chúng ta suy luận về các phân đoạn ranh giới và cách chúng phân chia không gian bên trong cửa sổ. 

Chúng ta có thể coi giao điểm của đa giác với cửa sổ là bài toán chia nhỏ mặt phẳng giới hạn trong một hình chữ nhật. Khả năng kết nối bên trong được xác định bằng cách các cạnh đa giác giao nhau với ranh giới cửa sổ và cách chúng tạo thành các rào cản dọc và ngang. 

Một cách tiếp cận tiêu chuẩn cho đa giác trực giao là nén mặt phẳng thành một tập hợp các tấm dọc và ngang được xác định bởi tất cả các đỉnh đa giác và ranh giới cửa sổ. Bên trong cấu trúc nén này, mỗi ô nằm bên trong hoặc bên ngoài đa giác và vùng lân cận có thể được mô hình hóa thông qua DSU hoặc BFS trên một lưới thưa thớt được tạo ra bởi các cạnh.

Tuy nhiên, một góc nhìn hiệu quả hơn là coi các cạnh đa giác là chướng ngại vật và mô phỏng khả năng kết nối của không gian trống bên trong cửa sổ. Chúng tôi xử lý các đường quét dọc trên tọa độ x nơi các sự kiện xảy ra ở các đỉnh đa giác và ranh giới cửa sổ. Giữa các sự kiện x liên tiếp, cấu trúc dọc được cố định và có thể được xử lý thông qua liên kết khoảng trên các phân đoạn y. DSU được sử dụng để duy trì khả năng kết nối của các dải dọc trên các lát x. 

Đa giác trực giao đảm bảo rằng các cạnh thay đổi hướng, do đó các ràng buộc dọc và ngang tạo thành một biểu đồ giống như lưới. Mỗi tấm dọc đóng góp nhiều nhất là các khoảng O(n) và DSU hợp nhất các khoảng vẫn được kết nối thông qua các cạnh ngang. 

Điều này làm giảm vấn đề duy trì khả năng kết nối của các phân đoạn trong một đường quét, trong đó chúng tôi theo dõi cách các vùng bên trong đa giác giao với dải dọc hiện tại bên trong cửa sổ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưới + lấp lũ | O(W·H) | O(W·H | |
