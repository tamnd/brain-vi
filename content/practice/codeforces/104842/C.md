---
title: "CF 104842C - Chuỗi C và Pascal"
description: "Chúng ta được cung cấp một chuỗi byte, mỗi byte được viết dưới dạng số thập lục phân có hai chữ số, vì vậy mỗi giá trị nằm trong phạm vi từ 0 đến 255."
date: "2026-06-28T11:31:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104842
codeforces_index: "C"
codeforces_contest_name: "2020-2021 ICPC, Moscow Subregional"
rating: 0
weight: 104842
solve_time_s: 37
verified: false
draft: false
---

[CF 104842C - Chuỗi C và Pascal](https://codeforces.com/problemset/problem/104842/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi byte, mỗi byte được viết dưới dạng số thập lục phân có hai chữ số, vì vậy mỗi giá trị nằm trong phạm vi từ 0 đến 255. Chuỗi này biểu thị một kết xuất bộ nhớ thô và nhiệm vụ là quyết định xem kết xuất này có thể được hiểu là một chuỗi kiểu C hợp lệ, một chuỗi kiểu Pascal hợp lệ, cả hai hay không. 

Chuỗi kiểu C ở đây được xác định theo cách hơi đơn giản. Chúng tôi quét mảng byte từ đầu. Chúng ta có thể thấy không hoặc nhiều byte “văn bản”, trong đó mỗi byte văn bản phải nằm trong phạm vi ASCII có thể in được từ 0x20 đến 0x7f. Sau đó, phải có một byte 0 duy nhất kết thúc chuỗi. Mọi thứ sau byte 0 này đều là rác không liên quan và không ảnh hưởng đến tính hợp lệ. 

Thay vào đó, một chuỗi kiểu Pascal bắt đầu bằng byte có độ dài l. Tất cả l byte tiếp theo phải nằm trong phạm vi ASCII có thể in được từ 0x20 đến 0x7f. Sau l byte đó, phần còn lại của mảng là rác và bị bỏ qua. Độ dài l nằm trong khoảng từ 0 đến 255, nhưng nó cũng phải vừa với kích thước mảng thực tế, nghĩa là phải có ít nhất l byte sau byte độ dài. 

Điểm khác biệt chính là các chuỗi C tìm kiếm số 0 kết thúc, trong khi các chuỗi Pascal mã hóa rõ ràng độ dài của chúng khi bắt đầu. 

Kích thước đầu vào tối đa là 1000 byte, do đó, mọi giải pháp lên tới O(n^2) đều đã an toàn, nhưng cấu trúc đủ đơn giản để quét tuyến tính là đủ. Điều này gợi ý rõ ràng rằng chúng ta nên tránh mọi nỗ lực bạo lực nhằm thử tất cả các điểm phân chia có thể có mà không cần xử lý trước. 

Một vài trường hợp tế nhị quan trọng. 

Một lỗi phổ biến là cho rằng byte 0 đầu tiên tự động xác định chuỗi C hợp lệ. Điều đó không chính xác vì tất cả các byte trước đó phải in được. Ví dụ: trong đầu vào:```
01 00
```Có số 0 nhưng không thể in được tiền tố byte 01, vì vậy đây không phải là chuỗi C hợp lệ. 

Đối với chuỗi Pascal, một trường hợp tinh tế khác là khi byte có độ dài vượt quá số byte còn lại. Ví dụ:```
05 41 42
```Ở đây l = 5, nhưng chỉ có hai byte theo sau, vì vậy nó không hợp lệ ngay cả khi chúng có thể in được. 

Cuối cùng, l = 0 hợp lệ trong chuỗi Pascal, nghĩa là chuỗi có thể có byte nội dung bằng 0 và ngay lập tức trở thành rác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi vị trí có thể như một điểm kết thúc C tiềm năng và mọi vị trí có thể dưới dạng diễn giải độ dài Pascal. Đối với mỗi ứng cử viên, nó sẽ xác nhận các ràng buộc cần thiết bằng cách quét các phân đoạn của mảng. Trong trường hợp xấu nhất, mỗi lần kiểm tra tốn O(n) và có O(n) lựa chọn cho cả hai cấu trúc, dẫn đến thời gian O(n^2). 

Quan sát quan trọng là cả hai cấu trúc chỉ phụ thuộc vào các điều kiện tiền tố đơn giản. Đối với chuỗi C, chúng ta chỉ cần biết liệu tiền tố chỉ chứa byte có thể in được hay không. Đối với chuỗi Pascal, chúng ta chỉ cần xác thực một phân đoạn cố định duy nhất được xác định bởi byte đầu tiên. 

Điều này cho phép xử lý trước một mảng boolean để theo dõi xem mỗi tiền tố có thể in được đầy đủ hay không. Khi điều đó có sẵn, tính hợp lệ của C sẽ giảm xuống việc kiểm tra các vị trí có byte 0. Hiệu lực Pascal là một kiểm tra trực tiếp duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Kiểm tra tiền tố tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách quá trình xác thực thành hai kiểm tra độc lập, một dành cho diễn giải kiểu C và một dành cho diễn giải kiểu Pascal. 

### Kiểm tra chuỗi C 

1. Tính toán một mảng tiền tố trong đó mỗi vị trí i cho biết liệu tất cả các byte từ 0 đến i có nằm trong phạm vi có thể in được hay không. 
2. Quét mảng từ trái sang phải và xem xét mọi vị trí i có byte bằng 0. 
3. Đối với mỗi vị trí i như vậy, hãy kiểm tra xem tất cả byte trước nó có thể in được bằng cách sử dụng mảng tiền tố hay không. 
4. Nếu có ít nhất một vị trí như vậy tồn tại, mảng có thể được hiểu là chuỗi C hợp lệ. 

Lý do là một chuỗi C hợp lệ được xác định hoàn toàn bằng cách chọn vị trí xuất hiện số 0 đầu tiên và ràng buộc tiền tố đảm bảo không có byte không hợp lệ nào xảy ra trước nó. 

### Kiểm tra chuỗi Pascal 

1. Đọc byte đầu tiên dưới dạng l, độ dài ứng viên. 
2. Kiểm tra xem l có nhỏ hơn hoặc bằng n trừ 1 hay không, đảm bảo còn đủ byte. 
3. Xác minh rằng tất cả byte từ chỉ mục 1 đến chỉ mục l đều nằm trong phạm vi có thể in được. 
4. Nếu cả hai điều kiện đều đúng thì mảng đó là một chuỗi Pascal hợp lệ. 

Điều này hiệu quả vì mã hóa Pascal cố định cấu trúc một cách cứng nhắc ngay từ đầu, do đó không có sự mơ hồ khi l được chọn. 

###Quyết định cuối cùng 

1. Kết hợp hai kết quả boolean và xuất ra một trong bốn trường hợp: cả hai đều hợp lệ, chỉ C hợp lệ, chỉ Pascal hợp lệ hoặc không hợp lệ. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là cả hai cách giải thích đều áp đặt các ràng buộc dựa trên tiền tố và cục bộ. Điều kiện C chỉ phụ thuộc vào việc kết thúc có hợp lệ hay không
