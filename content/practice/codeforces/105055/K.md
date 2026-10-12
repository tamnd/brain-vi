---
title: "CF 105055K - Mã Gõ"
description: "Mỗi chữ cái viết hoa phải được dịch sang vị trí của nó bên trong ma trận Knock Code 5 x 5 cố định. Mỗi chữ cái được biểu diễn bằng một cặp bao gồm hàng và cột của nó. Bản thân các số được mã hóa dưới dạng dấu hoa thị liên tiếp, do đó hàng 3 trở thành ."
date: "2026-06-28T00:25:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105055
codeforces_index: "K"
codeforces_contest_name: "UDESC Selection Contest 2023-2"
rating: 0
weight: 105055
solve_time_s: 31
verified: false
draft: false
---

[CF 105055K - Mã Knock](https://codeforces.com/problemset/problem/105055/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chữ cái viết hoa phải được dịch sang vị trí của nó bên trong ma trận Knock Code 5 x 5 cố định. Mỗi chữ cái được biểu diễn bằng một cặp bao gồm hàng và cột của nó. Bản thân các số được mã hóa dưới dạng dấu hoa thị liên tiếp, do đó hàng 3 trở thành`***`và cột 5 trở thành`*****`. Một khoảng trắng ngăn cách mọi chuỗi gõ, bất kể chuỗi tiếp theo thuộc về cùng một chữ cái hay chữ cái tiếp theo. 

Quy tắc đặc biệt duy nhất là ma trận chỉ có 25 ô, nên các chữ cái`C`Và`K`chia sẻ cùng một vị trí. Bất cứ khi nào đầu vào chứa một trong hai chữ cái, cả hai phải được mã hóa thành tọa độ của`C/K`tế bào. 

Độ dài đầu vào tối đa là 10000 ký tự. Mỗi ký tự tạo ra chính xác hai nhóm tối đa năm dấu hoa thị, do đó tổng lượng đầu ra được tạo tỷ lệ thuận với kích thước đầu vào. Điều này ngay lập tức loại trừ mọi nhu cầu về các thuật toán phức tạp. Một lần quét tuyến tính duy nhất trên chuỗi là quá đủ nhanh. 

Một sai lầm dễ dàng là quên điều đó`K`không có vị trí riêng của mình. 

đầu vào```
1
K
```Đầu ra đúng```
* ***
```Một chương trình đặt`K`sau đó`J`trong bảng chữ cái sẽ tạo ra tọa độ sai. 

Một lỗi phổ biến khác là chèn thêm dấu cách giữa các chữ cái. 

đầu vào```
2
AA
```Đầu ra đúng```
* * * *
```Chỉ có một khoảng trống giữa các nhóm gõ liên tiếp. Không có dấu phân cách đặc biệt giữa các chữ cái khác nhau vì nhóm hàng và nhóm cột đã được phân tách bằng dấu cách. 

Trường hợp tế nhị cuối cùng là chữ cái cuối cùng của tin nhắn. 

đầu vào```
1
Z
```Đầu ra đúng```
***** *****
```Việc thực hiện bất cẩn có thể để lại một khoảng trống sau nhóm cuối cùng. Đầu ra được yêu cầu không có khoảng trống thừa ở cuối. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là tìm kiếm ma trận 5 x 5 cho mỗi ký tự. Đối với mỗi chữ cái, chúng ta quét toàn bộ ma trận cho đến khi tìm thấy ô phù hợp, sau đó xuất ra hàng và cột tương ứng. Vì ma trận chỉ chứa 25 vị trí nên điều này yêu cầu tối đa 25 phép so sánh cho mỗi ký tự. Ngay cả đối với 10000 ký tự, đó chỉ là khoảng 250000 so sánh, dễ dàng nằm trong giới hạn. 

Mặc dù cách tiếp cận này đã đủ nhanh nhưng chúng ta hoàn toàn có thể tránh được việc tìm kiếm lặp lại. Ma trận không bao giờ thay đổi, vì vậy mọi chữ cái luôn ánh xạ tới cùng tọa độ. Thay vì tìm kiếm mọi lúc, chúng tôi xây dựng một từ điển trên
