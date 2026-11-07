---
title: "CF 1331B - Limericks"
description: "Bài toán yêu cầu chúng ta tính một thuộc tính số cụ thể liên quan đến một số nguyên đầu vào, ký hiệu là a. Trong khi phát biểu vấn đề được viết dưới dạng thơ, nhiệm vụ cơ bản là tìm số các số nguyên nhỏ hơn a mà nguyên tố cùng nhau với a."
date: "2026-06-11T16:12:26+07:00"
tags: ["codeforces", "competitive-programming", "*special", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1331
codeforces_index: "B"
codeforces_contest_name: "April Fools Day Contest 2020"
rating: 0
weight: 1331
solve_time_s: 216
verified: false
draft: false
---

[CF 1331B - Limericks](https://codeforces.com/problemset/problem/1331/B) 

**Đánh giá:** - 
**Tags:** *đặc biệt, toán, lý thuyết số 
**Thời gian giải:** 3 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tính một thuộc tính số cụ thể liên quan đến một số nguyên đầu vào, ký hiệu là`a`. Trong khi phát biểu vấn đề được viết dưới dạng thơ, nhiệm vụ cơ bản là tìm số nguyên nhỏ hơn`a`đó là nguyên tố cùng nhau`a`. Nói cách khác, đưa ra`a`, chúng ta được yêu cầu tính giá trị của hàm tổng Euler, thường được viết là φ(a). Hàm này đếm các số nguyên từ 1 đến`a - 1`không có yếu tố chung nào với`a`khác hơn 1. 

Đầu vào là một số nguyên duy nhất`a`trong phạm vi từ 4 đến 998, bao gồm. Bởi vì tối đa`a`nhỏ hơn 1000, chúng ta có thể đủ khả năng sử dụng các phương pháp phân tích nhân tử hoặc phép lặp đơn giản mà không phải lo lắng về việc đạt đến giới hạn thời gian. Vì chúng ta chỉ có một số nguyên cho mỗi trường hợp thử nghiệm, nên`O(sqrt(a))`Cách tiếp cận này là đủ, vì nó sẽ thực hiện tối đa khoảng 31 lần lặp trong trường hợp xấu nhất. 

Một trường hợp cạnh không rõ ràng phát sinh khi`a`là lũy thừa của số nguyên tố, chẳng hạn như 4, 8 hoặc 9. Đối với`a = 4`, các số nguyên nhỏ hơn 4 nguyên tố cùng nhau với 4 là 1 và 3, cho φ(4) = 2. Một cách tiếp cận ngây thơ chỉ loại bỏ các số chẵn sẽ thất bại đối với`a = 9`, đòi hỏi phải loại bỏ bội số của 3. Một cạm bẫy tiềm tàng khác là giả sử`a`là số nguyên tố hoặc số chẵn; đầu vào có thể là bất kỳ số nguyên nào trong phạm vi nhất định. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ để tính φ(a) sẽ là lặp qua tất cả các số nguyên từ 1 đến`a - 1`và kiểm tra xem ước số chung lớn nhất có`a`là 1. Điều này có tác dụng vì hàm GCD xác định chính xác các số nguyên tố cùng nhau. Số lượng hoạt động trong trường hợp xấu nhất là xấp xỉ`a * log(a)`nếu sử dụng thuật toán Euclid cho GCD, cho`a`lên tới 998 là khoảng 10^4 thao tác và có thể chấp nhận được đối với một trường hợp thử nghiệm. 

Cách tiếp cận tối ưu sử dụng tính chất nhân của hàm tổng Euler. Nếu như`a`có thể được nhân tử hóa như`a = p1^e1 * p2^e2 * ... * pk^ek`cho các thừa số nguyên tố riêng biệt`pi`, thì φ(a) = a * Π(1 - 1/pi). Nhận xét rằng mọi số nguyên nhỏ hơn`a`không chia hết cho bất kỳ thừa số nguyên tố nào của nó là nguyên tố cùng nhau cho phép chúng ta tính toán câu trả lời theo`O(sqrt(a))`thời gian, bằng cách chia thử để tìm thừa số nguyên tố. Điều này nhanh hơn cho phạm vi lớn hơn, nhưng với`a`< 1000, cả hai cách tiếp cận bạo lực và nhân tố hóa đều khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(một bản ghi a) | O(1) | Đã chấp nhận | 
| Nhân tố hóa | O(sqrt(a)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`từ đầu vào. Đây là số mà chúng ta sẽ tính hàm tổng. 
2. Khởi tạo một biến`result`ĐẾN`a`. Điều này sẽ giữ sản phẩm đang chạy cho tổng công thức. 
3. Lặp lại tất cả các số nguyên`i`từ 2 đến`sqrt(a)`(bao gồm). Mỗi`i`là một ứng cử viên
