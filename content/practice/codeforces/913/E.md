---
title: "CF 913E - Biểu thức logic"
description: "Chúng ta đang làm việc với các hàm Boolean có chính xác ba biến x, y và z. Hàm Boolean trên ba biến chỉ có tám phép gán đầu vào có thể. Đầu vào đưa ra giá trị của hàm trên mỗi phép gán trong số tám phép gán đó dưới dạng một chuỗi nhị phân có độ dài tám."
date: "2026-06-13T01:08:12+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 913
codeforces_index: "E"
codeforces_contest_name: "Hello 2018"
rating: 2400
weight: 913
solve_time_s: 205
verified: false
draft: false
---

[CF 913E - Biểu thức logic](https://codeforces.com/problemset/problem/913/E) 

**Đánh giá:** 2400 
**Tags:** bitmasks, dp, đường dẫn ngắn nhất 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với các hàm Boolean có chính xác ba biến,`x`,`y`, Và`z`. 

Hàm Boolean trên ba biến chỉ có tám phép gán đầu vào có thể. Đầu vào đưa ra giá trị của hàm trên mỗi phép gán trong số tám phép gán đó dưới dạng một chuỗi nhị phân có độ dài tám. Nhiệm vụ của chúng ta là xây dựng lại một biểu thức logic bằng cách sử dụng các biến, phủ định, kết hợp, phân biệt và dấu ngoặc đơn. 

Biểu thức phải ước tính chính xác bảng chân lý đã cho. Trong số tất cả các biểu thức hợp lệ, chúng ta muốn biểu thức có số lượng ký tự nhỏ nhất. Nếu một số biểu thức có cùng độ dài tối thiểu, chúng ta phải xuất ra biểu thức nhỏ nhất về mặt từ điển. 

Nhận xét quan trọng nhất là chỉ có`2^8 = 256`các hàm Boolean khác nhau của ba biến. Mặc dù số lượng biểu thức hợp lệ về mặt cú pháp là vô hạn nhưng số lượng ngữ nghĩa riêng biệt lại rất nhỏ. Điều này thay đổi hoàn toàn vấn đề. Thay vì tìm kiếm giữa các biểu thức, chúng ta có thể tìm kiếm trong số 256 bảng chân lý. 

Số lượng truy vấn lên tới 10.000, nhưng mỗi truy vấn chỉ hỏi về một trong 256 hàm có thể có. Điều này gợi ý mạnh mẽ một giải pháp tiền xử lý. Nếu chúng ta có thể tính toán biểu thức tối ưu cho tất cả 256 hàm một lần thì việc trả lời từng truy vấn sẽ trở thành một bảng tra cứu đơn giản. 

Một số trường hợp đặc biệt khiến việc triển khai đơn giản không thành công. 

Hãy xem xét chức năng được đại diện bởi:```
11110000
```Câu trả lời tối ưu là:```
!x
```Việc thực hiện bất cẩn có thể tạo ra:```
!(x)
```Cả hai đều đúng về mặt logic, nhưng biểu thức thứ hai dài hơn và do đó câu trả lời không hợp lệ. 

Một trường hợp tinh tế khác là quyền ưu tiên của toán tử. Giả sử chúng ta muốn hàm:```
x&(y|z)
```biểu thức```
x&y|z
```đại diện cho một chức năng khác bởi vì`&`liên kết mạnh hơn`|`. Dấu ngoặc đơn phải được thêm chính xác khi ngữ pháp yêu cầu và bỏ qua nếu không. 

Phá vỡ ràng buộc từ điển là một cái bẫy khác. Hai biểu thức có thể có độ dài bằng nhau và biểu diễn cùng một hàm. Ví dụ,
