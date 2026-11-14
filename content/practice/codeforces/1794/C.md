---
title: "CF 1794C - Chuỗi tính điểm"
description: "Chúng tôi được cung cấp một mảng không giảm. Với mỗi tiền tố a[1...k], chúng ta phải xác định một giá trị đặc biệt gọi là cost. Đối với bất kỳ dãy con nào có độ dài d, điểm của nó là tích của các phần tử chia cho d!."
date: "2026-06-09T10:10:25+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1794
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 856 (Div. 2)"
rating: 1300
weight: 1794
solve_time_s: 53
verified: false
draft: false
---

[CF 1794C - Chuỗi tính điểm](https://codeforces.com/problemset/problem/1794/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tham lam, toán học, hai con trỏ 
**Thời gian giải:** 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng không giảm. Đối với mỗi tiền tố`a[1...k]`, chúng ta phải xác định một giá trị đặc biệt gọi là chi phí. 

Đối với bất kỳ dãy con nào có độ dài`d`, điểm của nó là tích của các phần tử của nó chia cho`d!`. Trong số tất cả các chuỗi con của tiền tố, trước tiên chúng ta tìm số điểm tối đa có thể có. Một số chuỗi tiếp theo có thể đạt được số điểm tối đa đó. Chi phí là độ dài lớn nhất trong số các chuỗi con tối ưu đó. 

Nhiệm vụ là xuất chi phí này cho mọi tiền tố của mảng. 

Mảng đã được sắp xếp theo thứ tự không giảm và mọi giá trị đều thỏa mãn`1 ≤ a[i] ≤ n`. 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là`5 · 10^5`. Với kích thước đầu vào này, bất cứ thứ gì bậc hai đều quá đắt. MỘT`O(n^2)`giải pháp sẽ yêu cầu khoảng`2.5 · 10^11`hoạt động trong trường hợp xấu nhất. Thậm chí`O(n log^2 n)`sẽ nặng nề một cách không cần thiết. Chúng ta nên nhắm đến`O(n log n)`hoặc tốt hơn cho mỗi tệp thử nghiệm. 

Phần khó khăn là định nghĩa liên quan đến tích của các dãy con, điều này ban đầu gợi ý tìm kiếm theo cấp số nhân trên tất cả các dãy con. Quan sát quan trọng là định nghĩa điểm có cấu trúc rất chặt chẽ, thu gọn vấn đề thành một điều kiện đếm đơn giản. 

Một sai lầm dễ mắc phải là nghĩ rằng việc tối đa hóa điểm số đòi hỏi phải kiểm tra sản phẩm một cách trực tiếp. 

Ví dụ:```
a = [1, 2, 3]
```Điểm tối ưu đạt được bằng cả`[3]`Và`[2,3]`. 

Điểm số của`[3]`là`3`. 

Điểm số của`[2,3]`là`6/2 = 3`. 

Vì chi phí yêu cầu độ dài tối đa trong số các chuỗi con tối ưu nên câu trả lời là`2`, không`1`. 

Một trường hợp tinh vi khác là khi có nhiều giá trị bằng nhau.```
a = [5,5,5,5]
```Câu trả lời là:```
1 2 3 4
```Một người tham lam bất cẩn luôn chỉ lấy phần tử lớn nhất sẽ xuất ra tất cả các phần tử, điều này không chính xác vì việc thêm một phần tử khác`5`nhân tử số với`5`trong khi mẫu số chỉ đạt được hệ số bằng độ dài mới. 

Trường hợp cạnh thứ ba xảy ra khi nhiều giá trị nhỏ xuất hiện.```
a = [1,1,1]
```Câu trả lời:```
1 1 1
```Mặc dù độ dài tiền tố tăng lên, các chuỗi con dài hơn không cải thiện được điểm tối đa vì mọi phần tử được thêm vào chỉ đóng góp`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ kiểm tra mọi chuỗi con của mọi tiền tố. 

Đối với tiền tố có độ dài`k`, có`2^k`các chuỗi tiếp theo. Việc tính điểm và tìm ra điểm tốt nhất là hoàn toàn không thể thực hiện được ngay cả đối với`k = 50`, huống hồ là`10^5`. 

Lý do vũ lực hoạt động về mặt khái niệm rất đơn giản: nó tuân theo định nghĩa. Vấn đề là số lượng lớn các chuỗi con. 

Quan sát quan trọng xuất phát từ việc so sánh một dãy con có độ dài`d-1`với cùng một dãy con sau khi thêm một phần tử nữa`x`. 

Điểm số thay đổi theo yếu tố$$\frac{x}{d}.$$Nếu như`x > d`, điểm số tăng lên một cách nghiêm ngặt. 

Nếu như`x = d`, điểm số vẫn giữ nguyên. 

Nếu như`x < d`, điểm số giảm dần. 

Điều này thay đổi hoàn toàn cách chúng ta nghĩ về vấn đề. 

Giả sử chúng ta muốn một chuỗi có độ dài`d`để đạt được số điểm tối đa có thể. Vì mảng đã được sắp xếp nên cách tốt nhất để có được độ dài`d`là lấy các phần tử lớn nhất có thể. Để một dãy con như vậy có thể mở rộng đến độ dài`d`không làm giảm điểm, các thành phần của nó phải thỏa mãn$$b_i \ge i$$sau khi sắp xếp bên trong dãy con. 

Vì mảng ban đầu đã được sắp xếp nên chúng ta chỉ cần tìm lựa chọn giống hậu tố lớn nhất mà các phần tử của nó có thể đóng vai trò là vị trí`1,2,...,d`. 

Đối với tiền tố kết thúc ở vị trí`i`, câu trả lời sẽ có độ dài lớn nhất`len`như vậy trong số cuối cùng`len`các yếu tố,$$a_{i-len+1} \ge 1,\quad a_{i-len+2} \ge 2,\quad \dots,\quad a_i \ge len.$$Vì mảng đã được sắp xếp nên điều kiện này giảm xuống còn$$a_j \ge len-(i-j)$$cho mỗi vị trí được lựa chọn. 

Một hình thức đặc biệt thuận tiện là$$a_j \ge j-L+1,$$Ở đâu`L`là vị trí bắt đầu của đoạn được chọn. 

Khi chúng tôi xử lý các tiền tố từ trái sang phải, câu trả lời không bao giờ giảm nhiều hơn mức cần thiết. Điều này cho phép duy trì độ dài hợp lệ hiện tại và mở rộng nó một cách tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để`ans`biểu thị câu trả lời cho tiền tố hiện tại. 

1. Xử lý mảng từ trái qua phải. 
2. Khi có phần tử mới`a[i]`đến, hãy thử tăng câu trả lời hiện tại lên một. 
3. Độ dài mới của ứng cử viên là`ans + 1`. 
4. Để độ dài này khả thi, phần tử nhỏ nhất bên trong hậu tố độ dài đã chọn`ans + 1`ít nhất phải có`1`, ít nhất tiếp theo`2`, vân vân. 
5. Bởi vì mảng đã được sắp xếp nên việc kiểm tra tính khả thi sẽ giảm xuống còn việc xác minh xem phần tử mới có thể chiếm vị trí hay không`ans + 1`. 
6. Nếu`a[i] > ans`, tăng`ans`bởi một. 
7. Xuất ra giá trị hiện tại của`ans`. 

Việc triển khai cũng có thể được xem là duy trì hậu tố dài nhất thỏa mãn$$a_{i-len+1} \ge 1,\; a_{i-len+2} \ge 2,\; \dots,\; a_i \ge len.$$Bất cứ khi nào một giá trị mới xuất hiện, chỉ một vị trí bổ sung có thể trở thành hợp lệ, vì vậy chỉ cần so sánh một lần là đủ. 

### Tại sao nó hoạt động 

Cho dãy con tối ưu có các phần tử được sắp xếp`b1 ≤ b2 ≤ ... ≤ bd`. 

Thêm`i`-th đã chọn e
