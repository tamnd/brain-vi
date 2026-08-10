---
title: "CF 104020J - Đường chân trời lởm chởm"
description: "Một ô domino của bảng $8 nhân 8$ gán cho mỗi hình vuông đơn vị một hình vuông đối tác sao cho mỗi hình vuông thuộc về chính xác một đômino $1 nhân 2$ hoặc $2 nhân 1$ domino. Điều kiện trải chiếu bổ sung cấm bất kỳ đỉnh lưới nào nơi bốn quân domino khác nhau gặp nhau."
date: "2026-07-02T04:43:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104020
codeforces_index: "J"
codeforces_contest_name: "2022 Benelux Algorithm Programming Contest (BAPC 22)"
rating: 0
weight: 104020
solve_time_s: 122
verified: false
draft: false
---

[CF 104020J - Đường chân trời lởm chởm](https://codeforces.com/problemset/problem/104020/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Một lát gạch domino của$8\times 8$bảng gán cho mỗi hình vuông đơn vị một hình vuông đối tác sao cho mỗi hình vuông thuộc về chính xác một hình vuông$1\times 2$hoặc$2\times 1$domino. Điều kiện trải chiếu bổ sung cấm bất kỳ đỉnh lưới nào nơi bốn quân domino khác nhau gặp nhau. Tương tự, không có điểm mạng bên trong nào có tất cả bốn ô vuông đơn vị phụ được bao phủ bởi bốn quân domino riêng biệt được sắp xếp theo các hướng xen kẽ nhau. Đây là cấu hình cục bộ duy nhất vi phạm quy tắc, do đó vấn đề giảm xuống còn việc mô tả tất cả các ô domino trong đó cấu hình này không bao giờ xuất hiện. 

Một cách hữu ích để đọc ràng buộc là xem xét sự thay đổi hướng lan truyền như thế nào. Nếu một đỉnh không phải là điểm giao nhau bị cấm thì xung quanh đỉnh đó tất cả các quân domino ngẫu nhiên được sắp xếp theo một cách mạch lạc hoặc sự thay đổi hướng buộc phải “tuyến tính”, nghĩa là nó tiếp tục theo một hướng duy nhất thay vì phân nhánh. Điều này loại bỏ khả năng đan xen giống như lưới của cấu trúc ngang và dọc. Kết quả là, lát gạch không thể chứa một$2\times 2$vùng mà cả hai hướng trộn lẫn trong một mô hình giao nhau và mọi thay đổi từ cấu trúc ngang sang cấu trúc dọc phải lan truyền dọc theo một giao diện đơn điệu duy nhất. 

Sự cứng nhắc này buộc mỗi tấm tatami domino của hình chữ nhật phân hủy thành một đường cong đơn điệu phân cách giữa vùng chiếm ưu thế theo chiều ngang và vùng chiếm ưu thế theo chiều dọc. Khi một hướng được chọn, chẳng hạn như hướng ngang, bảng sẽ được lấp đầy bởi các hàng domino ngang ngoại trừ dọc theo một đường khuyết giống như cầu thang nơi hướng chuyển sang dọc và sau đó tiếp tục nhất quán. Đường khuyết tật bắt đầu ở một phía ranh giới, kết thúc ở một phía ranh giới khác và ở mỗi bước di chuyển đều đều, không bao giờ đảo ngược hướng, bởi vì bất kỳ sự đảo ngược nào cũng sẽ buộc bốn quân domino bị cấm gặp nhau ở đỉnh quay. 

Đối với một$n\times n$hội đồng quản trị, cấu trúc này hoàn toàn được xác định bởi ba lựa chọn độc lập. Đầu tiên, có thể chọn hướng tổng thể của lát nền theo$2$cách, theo chiều ngang hoặc theo chiều dọc. Thứ hai, vị trí bắt đầu của đường khuyết tật dọc theo đường biên có thể được chọn theo$n$cách. Thứ ba, sau khi điểm bắt đầu được cố định, đường dẫn lỗi sẽ di chuyển qua lưới theo$n-1$các bước và ở mỗi bước, nó có chính xác hai lựa chọn đơn điệu được chấp nhận, tương ứng với việc chuyển giao diện một đơn vị theo một trong hai hướng trong khi vẫn duy trì tính hợp pháp của chiếu. Những lựa chọn này độc lập vì mỗi quyết định cục bộ chỉ ảnh hưởng đến phân đoạn tiếp theo của giao diện và không bao giờ tạo ra cấu hình phân nhánh. 

Do đó số lượng đường dẫn lỗi có thể chấp nhận được là$2^{n-1}$. Nhân với$n$vị trí bắt đầu có thể và$2$định hướng toàn cầu mang lại tổng số gạch tatami domino của$n\times n$Cái bảng:$$2 \cdot n \cdot 2^{n-1} = n\cdot 2^n.$$Vì$n=8$điều này mang lại$$8\cdot 2^8 = 8\cdot 256 = 2048.$$Vì vậy, số lượng gạch tatami domino của$8\times 8$bàn cờ là$$\boxed{2048}.$$## Ghi chú 

Thực tế cấu trúc cơ bản là việc cấm bốn quân domino ở một đỉnh sẽ loại bỏ mọi khả năng tương tác hai chiều của các hướng. Những gì còn lại là một giao diện một chiều mà tổ hợp của nó giảm xuống thành một bước đi nhị phân trên bảng và việc đếm trở thành phép liệt kê vị trí bắt đầu và sự tiến hóa nhị phân của nó.
