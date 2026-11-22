---
title: "CF 277C - Trò chơi"
description: "Mỗi đường lưới bên trong của tờ giấy được chia thành các đoạn đơn vị bằng các điểm lưới. Một vết cắt dọc theo đường lưới đánh dấu tất cả các đoạn đơn vị mà dao chạm vào là đã được cắt. Quan sát quan trọng là hình dạng của tờ giấy không bao giờ thay đổi."
date: "2026-06-05T23:17:14+07:00"
tags: ["codeforces", "competitive-programming", "games", "implementation"]
categories: ["algorithms"]
codeforces_contest: 277
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 170 (Div. 1)"
rating: 2400
weight: 277
solve_time_s: 153
verified: false
draft: false
---

[CF 277C - Trò chơi](https://codeforces.com/problemset/problem/277/C) 

**Đánh giá:** 2400 
**Tags:** trò chơi, triển khai 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đường lưới bên trong của tờ giấy được chia thành các đoạn đơn vị bằng các điểm lưới. Một vết cắt dọc theo đường lưới đánh dấu tất cả các đoạn đơn vị mà dao chạm vào là đã được cắt. 

Quan sát quan trọng là hình dạng của tờ giấy không bao giờ thay đổi. Các mảnh không được di chuyển và vết cắt trong tương lai chỉ quan tâm đến việc liệu một đoạn đơn vị của đường lưới đã được chạm vào trước đó hay chưa. Trạng thái trò chơi chỉ đơn giản là tập hợp các phân đoạn lưới đơn vị vẫn chưa bị cắt. 

Hãy xem xét một đường lưới dọc bên trong. Nó chứa`m`phân đoạn đơn vị. Các vết cắt hiện tại đánh dấu một số khoảng trên đường đó là đã bị cắt. Các phân đoạn đơn vị chưa cắt còn lại tạo thành một số đường liền kề rời rạc. Mô tả tương tự áp dụng cho mọi đường lưới ngang bên trong. 

Một nước đi sẽ chọn một đường lưới và cắt một khoảng liền kề trên đó. Bên trong một đoạn dài chưa cắt`s`, việc di chuyển như vậy sẽ loại bỏ một khối liền kề và để lại nhiều nhất là hai đường chạy nhỏ hơn không bị cắt. 

Kích thước có thể lớn như`10^9`, vì vậy chúng tôi không thể lưu trữ lưới. Số lượng cắt hiện có chỉ là`10^5`, có nghĩa là chỉ`O(k)`các đường lưới có thể khác với trạng thái nguyên vẹn. Bất kỳ giải pháp được chấp nhận nào cũng phải hoạt động chỉ từ các mô tả cắt. 

Một lỗi phổ biến là coi mỗi lần cắt đầu vào như một thành phần trò chơi độc lập. Các vết cắt chồng chéo trước tiên phải được hợp nhất. 

Ví dụ:```
3 3 2
1 0 1 2
1 1 1 3
```Cả hai vết cắt đều nằm trên cùng một đường thẳng đứng và chồng lên nhau. Liên minh của họ bao gồm toàn bộ dòng. Sau khi hợp nhất, dòng đó không có đoạn nào chưa bị cắt. 

Một sai lầm dễ mắc phải khác là đếm các phân đoạn đơn vị riêng lẻ thay vì số lần chạy tối đa không bị cắt. 

Ví dụ:```
3 3 1
1 1 1 2
```Đường thẳng đứng`x = 1`có những đoạn dài chưa cắt`1`Và`1`, không một đoạn dài nào`2`. Giá trị Sprague-Grundy phụ thuộc vào việc phân rã lần chạy. 

Trường hợp tinh tế cuối cùng đến từ những chiều không gian khổng lồ. Giả định`n = 10^9`,`m = 10^9`, Và`k = 0`. Có hàng tỷ dòng chưa được chạm tới. Giải pháp phải tổng hợp sự đóng góp của chúng về mặt toán học thay vì lặp lại chúng. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mọi lượt chạy tối đa không bị cắt là một thành phần của trò chơi. Trong một khoảng thời gian dài`s`, một bước di chuyển sẽ loại bỏ bất kỳ khối liền kề nào và để lại hai lượt chạy nhỏ hơn. Chúng tôi có thể tính toán các giá trị Sprague-Grundy cho mọi độ dài và sau đó đánh giá toàn bộ vị trí. 

Điều đó ngay lập tức thất bại vì độ dài có thể lớn bằng`10^9`. Ngay cả việc lưu trữ các giá trị Grundy với độ dài tối đa cũng không thể thực hiện được. 

Điểm quan trọng là trò chơi chạy một lần có chức năng Grundy đơn giản đến không ngờ. 

Cho phép`G(s)`là giá trị Grundy của một đoạn dài chưa cắt`s`. 

Một nước đi chọn độ dài`l`Và`r`cho các phần bên trái và bên phải còn sót lại, trong đó`l + r < s`. Kể từ đây$$G(s)=\operatorname{mex}\{G(l)\oplus G(r)\}.$$Giả sử quy nạp rằng`G(t) = t`cho tất cả các chiều dài nhỏ hơn. Sau đó$$G(s)=\operatorname{mex}\{l\oplus r \mid l+r<s\}.$$Mọi giá trị từ`0`ĐẾN`s-1`xuất hiện trong tập hợp đó và không có giá trị`≥ s`có thể xuất hiện. Như vậy$$G(s)=s.$$Vì vậy, mọi lần chạy không cắt đều hoạt động giống hệt như một đống Nim có kích thước bằng với chiều dài của nó. 

Bây giờ toàn bộ trò chơi trở thành thế Nim. Tổng giá trị Grundy chỉ đơn giản là XOR của độ dài của tất cả các lần chạy tối đa không bị cắt trên tất cả các đường lưới bên trong. 

Chỉ một`O(k)`đường lưới bị ảnh hưởng bởi các vết cắt. Mỗi đường thẳng đứng khác đóng góp một đống kích thước`m`và mọi đường ngang khác đóng góp một đống kích thước`n`. Vì XOR cùng một giá trị mà số lần chẵn bị hủy bỏ, nên các dòng chưa được chạm tới có thể được tổng hợp bằng cách sử dụng tính chẵn lẻ. 

Công việc còn lại là: 

1. Hợp nhất các khoảng trên mỗi dòng bị ảnh hưởng. 
2. Tính số bước không cắt lớn nhất trong phần bù. 
3. XOR độ dài của chúng. 
4. Áp dụng chiêu thắng chuẩn Nim. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không thể có độ dài lên tới$10^9$| Không thể | Quá chậm | 
| Tối ưu |$O(k \log k)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách các vết cắt thành nhóm dọc và nhóm ngang. 

Một vết cắt dọc thuộc về một đường bên trong`x = const`và bao gồm một khoảng trên`y`trục. Một đường cắt ngang thuộc về một đường nội thất`y = const`và bao gồm một khoảng trên`x`trục. 
2. Đối với mỗi dòng bị ảnh hưởng, hãy sắp xếp các khoảng của nó và hợp nhất
