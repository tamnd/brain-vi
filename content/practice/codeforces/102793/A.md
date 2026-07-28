---
title: "CF 102793A - \u0421\u043c\u0435\u043d\u0430 \u0441\u0442\u0438\u043b\u044f"
description: "Nhiệm vụ là chuyển đổi tên biến được viết theo một trong các kiểu lập trình phổ biến, CamelCase hoặc CamelCase, thành chữ hoa. Tên biến là một chuỗi các chữ cái Latinh trong đó ranh giới các từ được đánh dấu bằng chữ in hoa."
date: "2026-07-27T17:58:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102793
codeforces_index: "A"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102793
solve_time_s: 33
verified: false
draft: false
---

[CF 102793A - \u0421\u043c\u0435\u043d\u0430 \u0441\u0442\u0438\u043b\u044f](https://codeforces.com/problemset/problem/102793/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chuyển đổi tên biến được viết theo một trong các kiểu lập trình phổ biến,`camelCase`hoặc`CamelCase`, vào trong`snake_case`. Tên biến là một chuỗi các chữ cái Latinh trong đó ranh giới các từ được đánh dấu bằng chữ in hoa. Ở định dạng đích, mọi chữ cái phải viết thường và các từ liền kề phải được phân tách bằng dấu gạch dưới. Đầu vào chứa một số tên biến và với mỗi tên chúng ta phải in phiên bản đã chuyển đổi của nó. 

Ví dụ, tên`toBeOrNotToBe`bao gồm các từ`to`,`Be`,`Or`,`Not`, Và`To`, vậy kết quả sẽ là`to_be_or_not_to_be`. Quy tắc tương tự áp dụng cho các tên bắt đầu bằng chữ in hoa, chẳng hạn như`CamelCase`, trở thành`camel_case`. 

Kích thước đầu vào được thiết kế để cho phép quét trực tiếp. Có tối đa 100 tên và mỗi tên có thể chứa tối đa 1000 ký tự. Điều đó có nghĩa là tổng số ký tự được xử lý chỉ khoảng 100000, do đó, một thuật toán chạm vào từng ký tự với số lần không đổi là đủ nhanh. Bất kỳ cách tiếp cận nào liên quan đến việc tìm kiếm liên tục trong các chuỗi, việc xây dựng lại chúng không hiệu quả hoặc thử mọi cách phân tách từ có thể đều không cần thiết. 

Các trường hợp đặc biệt chính đến từ việc xử lý chữ in hoa một cách chính xác. Một giải pháp đơn giản chỉ chèn dấu gạch dưới trước chữ viết hoa có thể thất bại khi ký tự đầu tiên là chữ hoa, bởi vì`CamelCase`nên trở thành`camel_case`, không`_camel_case`. Một lỗi phổ biến khác là quên rằng một chuỗi có thể bao gồm toàn bộ chữ in hoa. Ví dụ,`ABCDE`nên trở thành`a_b_c_d_e`, vì mỗi chữ cái viết hoa đều thể hiện sự bắt đầu của một từ mới theo quy luật của bài toán này. 

Hãy xem xét đầu vào này:```
1
CamelCase
```Đầu ra đúng là:```
camel_case
```Việc thực hiện bất cẩn làm tăng thêm`_`trước mỗi ký tự viết hoa và chỉ các chữ thường sau đó mới có thể tạo ra`_camel_case`, để lại dấu phân cách ở đầu không chính xác. 

Một ví dụ khác:```
1
ABCDE
```Đầu ra đúng là:```
a_b_c_d_e
```Việc triển khai giả sử các chữ cái viết hoa chỉ xuất hiện giữa các chữ cái viết thường có thể giữ toàn bộ chuỗi lại với nhau một cách không chính xác vì`abcde`. 

## Phương pháp tiếp cận 

Giải pháp đơn giản nhất là mô phỏng chuyển đổi từng ký tự. Một cách mạnh mẽ là thử chia chuỗi thành các từ, tạo ra các cách diễn giải có thể có và chọn một quy tắc CamelCase phù hợp. Cách tiếp cận này đúng vì mọi chuyển đổi hợp lệ đều tương ứng với một số phân vùng của chuỗi gốc, nhưng điều đó là không cần thiết. Đối với một chuỗi có độ dài 1000, số lượng phân vùng có thể tăng theo cấp số nhân, khiến điều này không thể xảy ra ngay cả đối với một đầu vào lớn. 

Cấu trúc của đầu vào cho chúng ta quan sát đơn giản hơn nhiều. Trong CamelCase, các chữ in hoa đã đánh dấu mọi ranh giới giữa các từ. Chúng ta không cần phải khám phá những từ đó ở đâu. Chúng ta chỉ cần dịch từng ký tự tùy theo nó có bắt đầu từ mới hay không. 

Trong khi quét chuỗi từ trái sang phải, mọi chữ cái viết thường đều được sao chép ở dạng chữ thường. Khi một chữ in hoa xuất hiện, nó đại diện cho một từ mới. Nếu nó không phải là ký tự đầu tiên, chúng ta chèn dấu gạch dưới trước nó, sau đó thêm phiên bản chữ thường của nó. Ký tự đầu tiên là đặc biệt vì nó không thể có dấu phân cách trước nó. 

Điều này làm giảm vấn đề từ việc cố gắng hiểu toàn bộ cấu trúc chuỗi thành một lần truyền xác định duy nhất qua các ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong độ dài chuỗi | O(n) | Quá chậm | 
| Tối ưu | O(tổng số ký tự) | O(tổng kích thước đầu ra) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng tên biến và xử lý từng tên một cách độc lập. Mỗi biến có thể được chuyển đổi mà không phụ thuộc vào bất kỳ biến nào khác. 
2. Tạo chuỗi kết quả trống cho biến hiện tại. Chúng tôi xây dựng câu trả lời dần dần vì mỗi ký tự có thể ảnh hưởng đến định dạng của ký tự đầu ra tiếp theo. 
3. Quét tên biến từ ký tự đầu tiên đến ký tự cuối cùng. 
4. Nếu ký tự hiện tại là chữ hoa, hãy kiểm tra xem đó có phải là ký tự đầu tiên hay không. Nếu đó không phải là ký tự đầu tiên, hãy thêm dấu gạch dưới trước nó vì một từ mới bắt đầu ở đây. 
5. Thêm phiên bản chữ thường của ký tự hiện tại vào kết quả. Viết thường mọi ký tự ở giai đoạn này đảm bảo rằng tên cuối cùng theo sau`snake_case`. 
6. Nếu ký tự hiện tại đã là chữ thường, hãy thêm nó trực tiếp vì nó thuộc về từ hiện tại. 
7. In kết quả đã xây dựng. 

Lý do điều này có hiệu quả là thông tin duy nhất cần thiết để khôi phục ranh giới từ là vị trí của các chữ cái viết hoa. CamelCase không chứa bất kỳ dấu phân cách nào khác, vì vậy mỗi chữ cái viết hoa sau ký tự đầu tiên xác định duy nhất một vị trí phải chèn dấu gạch dưới. 

Tại sao nó hoạt động: 

Trong quá trình quét, tiền tố được xử lý của chuỗi luôn được thể hiện chính xác như nó sẽ xuất hiện trong`snake_case`. Khi chúng ta gặp một ký tự chữ thường, nó sẽ mở rộng từ hiện tại, do đó, việc thêm nó vào sẽ duy trì tính chính xác. Khi chúng ta gặp một ký tự viết hoa sau vị trí đầu tiên, định nghĩa CamelCase đảm bảo rằng một từ mới bắt đầu ở đó, do đó, việc thêm dấu gạch dưới trước dạng chữ thường của nó sẽ duy trì sự phân tách cần thiết. Vì mỗi ký tự được xử lý một lần nên đầu ra hoàn chỉnh phải chính xác sau khi ký tự cuối cùng được xử lý. 

(Phần 2 tiếp tục với Giải pháp Python, các ví dụ, độ phức tạp, bài kiểm tra và các trường hợp đặc biệt.)
