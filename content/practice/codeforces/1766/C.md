---
title: "CF 1766C - Bức tường Hamilton"
description: "Chúng ta có một lưới có hai hàng và m cột tượng trưng cho một bức tường. Mỗi ô có màu đen (B) hoặc trắng (W). Monocarp muốn vẽ tất cả các ô màu đen chính xác một lần trên một đường liên tục duy nhất chỉ di chuyển giữa các ô liền kề."
date: "2026-06-09T12:57:42+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1766
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 139 (Rated for Div. 2)"
rating: 1300
weight: 1766
solve_time_s: 168
verified: false
draft: false
---

[CF 1766C - Bức tường Hamilton](https://codeforces.com/problemset/problem/1766/C) 

**Đánh giá:** 1300 
**Tags:** dp, triển khai 
**Thời gian giải:** 2 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới có hai hàng và`m`cột tượng trưng cho một bức tường. Mỗi ô có màu đen (`B`) hoặc trắng (`W`). Monocarp muốn vẽ tất cả các ô màu đen chính xác một lần trên một đường liên tục duy nhất chỉ di chuyển giữa các ô liền kề. Mỗi cột có ít nhất một ô đen nên không thể có cột nào không có ô đen. Câu hỏi đặt ra là liệu đường dẫn Hamilton như vậy có tồn tại cho mỗi trường hợp thử nghiệm hay không. 

Đầu vào đảm bảo rằng mỗi cột có ít nhất một ô đen, do đó vấn đề không hề nhỏ chỉ ở cách các ô đen được kết nối. Vì chỉ có hai hàng nên mỗi cột có thể có một hoặc hai ô màu đen. Đường đi Hamilton phải đi qua tất cả các ô đen, ghé thăm mỗi ô đúng một lần và không bao giờ đi vào ô trắng. Điều này giúp giảm thiểu hiệu quả việc kiểm tra xem các ô đen có tạo thành một "con rắn" được kết nối mà không cần phải quay lại hoặc nhảy qua các ô trắng hay không. 

Các ràng buộc chặt chẽ về số lượng trường hợp thử nghiệm và tổng cột:`t`có thể đạt tới 10^4 và tổng của tất cả`m`qua các lần kiểm tra là 2 × 10^5. Điều này loại trừ mọi giải pháp bậc hai trong`m`mỗi trường hợp thử nghiệm; chúng ta cần quét tuyến tính lưới cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh không rõ ràng phát sinh khi các ô màu đen trong các cột liền kề không được căn chỉnh theo cách cho phép đường dẫn liên tục. Ví dụ, nếu cột`j`chỉ có một ô màu đen ở hàng và cột trên cùng`j+1`chỉ có một ô màu đen ở hàng dưới cùng, đường dẫn hợp lệ phải "thả" từ trên xuống dưới giữa các cột. Nếu sự sụt giảm như vậy xảy ra nhiều lần hoặc bị chặn bởi các ô trắng, thì có thể không thể bao phủ tất cả các ô đen mà không vi phạm tính kề cận hoặc ghé thăm một ô trắng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử mọi ô đen bắt đầu và thực hiện DFS để xem liệu đường dẫn Hamilton có tồn tại hay không. Cách tiếp cận này đúng nhưng quá chậm vì số lượng đường dẫn đệ quy tăng theo cấp số nhân với`m`. Mỗi trường hợp thử nghiệm có thể yêu cầu kiểm tra O(2^m) trong trường hợp xấu nhất, điều này là không khả thi do các ràng buộc. 

Cách tiếp cận tối ưu tận dụng cấu trúc của lưới. Vì chỉ có hai hàng nên đường Hamiltonian hợp lệ phải đi theo mô hình zig-zag dọc theo các cột, chỉ di chuyển giữa các hàng khi cần thiết. Chúng ta có thể định nghĩa cột "bắt đầu" là ô màu đen ngoài cùng bên trái ở hàng trên cùng có chứa một ô màu đen. Sau đó, chúng tôi quét từng cột, cố gắng theo dõi các ô màu đen trong khi chuyển đổi hàng nếu cần. Nếu chúng ta có thể di chuyển từ cột này sang cột khác mà không gặp phải cột có các ô màu đen ngăn cản sự liền kề, thì có một đường dẫn tồn tại. Nếu không thì không. 

Thông tin chi tiết quan trọng là trong lưới 2 hàng có đảm bảo "ít nhất một màu đen trên mỗi cột", đường dẫn luôn có thể được xây dựng một cách tham lam từ ô đen ngoài cùng bên trái, chuyển đổi các hàng bất cứ khi nào cần, trừ khi có một "lỗ" trong ô đen buộc phải nhảy qua ô trắng, điều này vi phạm quy tắc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (DFS tất cả các đường dẫn) | O(2^m) | O(m) | Quá chậm | 
| Quét zig-zag tham lam | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định cột ngoài cùng bên trái chứa ô màu đen. Nếu như
