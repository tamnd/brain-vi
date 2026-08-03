---
title: "CF 102638G - Noogies"
description: "Nhiệm vụ là xây dựng một số n mà “bộ tạo ngẫu nhiên” của nó tạo ra chính xác m vị trí. Trình tạo chọn tất cả các số nguyên từ 1 đến n có ít nhất một ước chung lớn hơn 1 với n."
date: "2026-08-02T14:44:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102638
codeforces_index: "G"
codeforces_contest_name: "Bredor contest"
rating: 0
weight: 102638
solve_time_s: 75
verified: false
draft: false
---

[CF 102638G - Noogies](https://codeforces.com/problemset/problem/102638/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xây dựng một số`n`"trình tạo ngẫu nhiên" của nó tạo ra chính xác`m`các vị trí. Trình tạo chọn tất cả các số nguyên từ`1`ĐẾN`n`có ít nhất một ước chung lớn hơn`1`với`n`. 

Do đó, đại lượng chúng ta cần kiểm soát là số số nguyên trong`[1, n]`không cùng nguyên tố với`n`. Các số nguyên nguyên tố cùng nhau`n`được tính bằng hàm tổng Euler`phi(n)`, do đó số tiền được tạo ra bởi quá trình này là:$$n - \phi(n)$$Toàn bộ vấn đề trở thành việc tìm kiếm bất kỳ`n ≤ 10^12`như vậy:$$n - \phi(n) = m$$Đầu vào chỉ chứa một số nguyên`m`, với các giá trị lên tới`10^8`. Giá trị này quá lớn để tìm kiếm thông qua các giá trị có thể có của`n`, bởi vì ngay cả việc kiểm tra tất cả các số gần`10^8`sẽ yêu cầu quá nhiều tổng số tính toán. Giải pháp cần xây dựng trực tiếp thay vì liệt kê. 

Một sai lầm phổ biến là nghĩ rằng`n = m + 1`sẽ hoạt động vì nó chỉ yêu cầu thêm một số. Tuy nhiên, điều đó có nghĩa`phi(n)=1`, điều này gần như không bao giờ đúng. Ví dụ, đối với`m=8`, đang chọn`n=9`cho`9-phi(9)=9-6=3`, không`8`. 

Một trường hợp tế nhị khác là khi`m`thật kỳ quặc. Các số có đúng một thừa số nguyên tố là`2`hành xử khác với số lẻ, nên cố gắng một cách mù quáng`n=2m`thất bại. Ví dụ,`m=9`Và`n=18`cho`18-phi(18)=18-6=12`, trong khi câu trả lời đúng có thể là`21`, bởi vì`21-phi(21)=21-12=9`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử các giá trị của`n`và tính toán`phi(n)`cho đến khi tìm được một người với`n-phi(n)=m`. Tổng số có thể được tính bằng cách phân tích nhân tố`n`, do đó, việc kiểm tra một ứng cử viên sẽ tốn khoảng số thừa số nguyên tố của số đó. Vấn đề là câu trả lời có thể lớn hơn nhiều so với`m`và không có giới hạn trên hữu ích cho việc tìm kiếm ngoại trừ giới hạn rất lớn của`10^12`. Ngay cả việc kiểm tra hàng triệu ứng viên cũng không đáng tin cậy. 

Quan sát quan trọng là ngừng tìm kiếm`n`chính nó và thay vào đó thiết kế các thừa số nguyên tố của nó. 

Lấy một số có dạng:$$n = s \cdot p$$Ở đâu`p`là số nguyên tố và`s`là số nguyên tố nhỏ cùng nhau`p`. 

Vì hàm tổng có tính nhân đối với các số nguyên tố cùng nhau:$$\phi(n)=\phi(s)\phi(p)=\phi(s)(p-1)$$Vì thế:$$n-\phi(n)=sp-\phi(s)(p-1)$$Sắp xếp lại:$$n-\phi(n)=p(s-\phi(s))+\phi(s)$$Cho phép:$$a=s-\phi(s)$$Và:$$b=\phi(s)$$Khi đó điều kiện cần sẽ trở thành:$$m=a p+b$$Đối với một nhỏ được chọn`s`, chúng ta có thể tính toán`a`Và`b`, rút ​​ra giá trị cần tìm của`p`, và kiểm tra xem nó có phải là số nguyên tố hay không. 

Câu hỏi duy nhất còn lại là làm thế nào để chọn`s`. Các ràng buộc cho phép chúng ta thử nhiều giá trị nhỏ. Một cái nhỏ`s`đưa ra một số nhân nhỏ trong`n=s*p`, giữ câu trả lời cuối cùng ở bên dưới`10^12`. Sự đảm bảo tồn tại của vấn đề có nghĩa là một trong những công trình nhỏ này sẽ thành công. 

Cuộc tìm kiếm tàn bạo`s`không tìm kiếm câu trả lời. Nó đang tìm kiếm trên một nhóm nhỏ các công thức, mỗi công thức sẽ đưa ra ngay một ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc`n`| O(câu trả lời × chi phí bao thanh toán) | O(1) | Quá chậm | 
| Xây dựng sử dụng`n=s*p`| O(S log log S + S log p) | O(S) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước hàm tổng Euler cho mọi giá trị nhỏ của`s`mà chúng tôi sẽ thử. Phạm vi tìm kiếm nhỏ so với các ràng buộc ban đầu nên chỉ cần sàng là đủ. 
2. Đối với mọi ứng viên`s`, tính:$$a=s-\phi(s)$$Và:$$b=\phi(s)$$Công thức trả lời trở thành:$$p=\frac{m-b}{a}$$Nếu như`m-b`không chia hết cho`a`, cái này`s`không thể làm việc. 
3. Kiểm tra xem kết quả tính toán có`p`là số nguyên tố và liệu`p`là nguyên tố cùng nhau với`s`. Điều kiện nguyên tố cùng nhau là bắt buộc vì công thức nhân tổng chỉ áp dụng cho các đối số nguyên tố cùng nhau. 
4. Một khi hợp lệ`s`Và`p`được tìm thấy, xuất ra:$$n=s\cdot p$$Việc xây dựng hợp lệ vì số lượng được tạo chính xác là:$$sp-\phi(s)(p-1)=m$$### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến mà mọi ứng cử viên mà nó tạo ra đều có một công thức chính xác đã biết cho`n-phi(n)`. Đối với mỗi thử nghiệm`s`, giá trị của`p`được chọn sao cho công thức bằng mục tiêu`m`. Việc kiểm tra tính nguyên tố và đồng nguyên tố đảm bảo rằng phép nhân tổng số Euler là hợp lệ. Vì vậy, khi thuật toán chấp nhận một cặp`(s,p)`, số kết quả`n=s*p`nhất thiết phải tạo ra chính xác`m`các vị trí. 

Bài toán đảm bảo rằng tồn tại một câu trả lời hợp lệ, do đó việc tìm kiếm cuối cùng sẽ tìm ra cách xây dựng như vậy. Giá trị cuối cùng của`n`nằm trong giới hạn yêu cầu vì chỉ những giá trị nhỏ của`s`được xem xét và`m`nhiều nhất là`10^8`. 

(Phần 2 tiếp tục với giải pháp Python, chi tiết triển khai, ví dụ, bài kiểm tra và các trường hợp đặc biệt.)
