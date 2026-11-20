---
title: "CF 217C - Formurosa"
description: "Công thức mô tả hàm Boolean được xây dựng từ các hằng số 0 và 1, phần giữ chỗ ?, và các toán tử AND, OR và XOR. Mỗi ? tương ứng với một chiếc lá của cây. Trong quá trình thử nghiệm, chúng tôi chọn khuẩn lạc nào được đặt trên mỗi chiếc lá."
date: "2026-06-05T00:59:41+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "dp", "expression-parsing"]
categories: ["algorithms"]
codeforces_contest: 217
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 134 (Div. 1)"
rating: 2600
weight: 217
solve_time_s: 149
verified: false
draft: false
---

[CF 217C - Formurosa](https://codeforces.com/problemset/problem/217/C) 

**Đánh giá:** 2600 
**Tags:** chia để trị, dp, phân tích biểu thức 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Công thức mô tả hàm Boolean được xây dựng từ các hằng số`0`Và`1`, phần giữ chỗ`?`và các toán tử AND, OR và XOR. 

Mỗi`?`tương ứng với một chiếc lá của cây. Trong quá trình thử nghiệm, chúng tôi chọn khuẩn lạc nào được đặt trên mỗi chiếc lá. Nếu thuộc địa`i`có giá trị loài`x_i ∈ {0,1}`, thì công thức sẽ được đánh giá dựa trên các giá trị đó và trả về`0`hoặc`1`. 

Các nhà khoa học có thể thực hiện bao nhiêu thí nghiệm tùy thích. Lời hứa duy nhất về các loại thuộc địa chưa biết là không phải tất cả các thuộc địa đều có cùng một loài. 

Câu hỏi đặt ra là liệu công thức đã cho có đủ mạnh để xác định chính xác loài của mỗi đàn cho mỗi phép gán loài hợp lệ hay không. 

Kích thước đầu vào ngay lập tức loại trừ mọi cách tiếp cận xây dựng bảng chân lý. Độ dài công thức có thể đạt tới một triệu ký tự, do đó ngay cả một`O(length × log length)`giải pháp tốn kém một cách không cần thiết. Chúng ta cần quét tuyến tính biểu thức và chỉ một lượng công việc không đổi cho mỗi ký hiệu. 

Phần tinh vi của vấn đề là công thức có thể chứa nhiều`?`lá, nhưng các nhà khoa học được phép đặt cùng một khuẩn lạc trên mỗi chiếc lá. Quan sát đó hóa ra hoàn toàn mô tả thời điểm có thể phục hồi. 

Hãy xem xét công thức`(?^?)`. 

Nếu chúng ta đặt cùng một khuẩn lạc trên cả hai lá thì kết quả luôn là`0`. Thí nghiệm không cung cấp thông tin về loài của thuộc địa đó. Trên thực tế, hai nhiệm vụ```
colony A = 0, colony B = 1
```Và```
colony A = 1, colony B = 0
```tạo ra thông tin giống hệt nhau nên không thể xác định được loài một cách duy nhất. Câu trả lời đúng là`NO`. 

Bây giờ hãy xem xét```
(?|0)
```Đặt thuộc địa`i`trên mỗi chiếc lá đánh giá chính xác giá trị của thuộc địa đó. Một thí nghiệm trên mỗi đàn sẽ tiết lộ tất cả các loài ngay lập tức, vì vậy câu trả lời đúng là`YES`. 

Một sai lầm dễ mắc phải khác là phân tích toàn bộ hàm nhiều biến. Câu hỏi thực sự không phải là liệu công thức có phân biệt được các phép gán lá khác nhau hay không. Các nhà khoa học tự mình kiểm soát mô hình phân công lá. Thí nghiệm quan trọng là đặt cùng một thuộc địa ở mọi nơi, điều này làm giảm toàn bộ công thức thành hàm của một biến duy nhất. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi công thức như một hàm Boolean của tất cả các lá của nó. Người ta có thể cố gắng mô tả đặc điểm của mọi thử nghiệm có thể xảy ra và xác định xem liệu họ hàm thu được có nhận dạng duy nhất tất cả các phép gán thuộc địa hay không. 

Điều đó nhanh chóng trở thành không thể. Công thức có thể chứa hàng trăm nghìn lá nên ngay cả việc lưu trữ bảng chân lý cũng vô vọng. Số lượng các phép gán lá có thể có là theo cấp số nhân. 

Quan sát quan trọng là các nhà khoa học có thể đặt cùng một khuẩn lạc trên mỗi chiếc lá. 

Giả sử mọi`?`trong công thức nhận được cùng một giá trị`x`. Toàn bộ biểu thức thu gọn thành hàm Boolean một biến```
g(x)
```Vì hàm Boolean của một biến chỉ có bốn khả năng nên công thức rút gọn phải là một trong```
0
1
x
!x
```Nếu như`g(x) = x`hoặc`g(x) = !x`, sau đó một thí nghiệm duy nhất với khuẩn lạc`i`đặt trên mỗi chiếc lá sẽ tiết lộ loài thuộc địa`i`. Lặp lại điều này cho tất cả các thuộc địa sẽ phục hồi toàn bộ nhiệm vụ. 

Nếu như`g(x)`là không đổi thì việc sử dụng cùng một thuộc địa ở mọi nơi luôn cho cùng một câu trả lời bất kể loài nào. Công thức không bao giờ cung cấp một tham chiếu tuyệt đối để phân biệt sp
