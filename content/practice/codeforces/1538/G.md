---
title: "CF 1538G - Bộ Quà Tặng"
description: "Chúng tôi có hai đống kẹo. Một đống chứa x viên kẹo màu đỏ và đống kia chứa y viên kẹo màu xanh. Mỗi bộ quà phải sử dụng đúng a+b kẹo. Có hai cách kết hợp có thể: - a đỏ và b xanh lam - b đỏ và xanh lam Hai loại đối xứng."
date: "2026-06-10T14:54:09+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1538
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 725 (Div. 3)"
rating: 2100
weight: 1538
solve_time_s: 54
verified: false
draft: false
---

[CF 1538G - Bộ quà tặng](https://codeforces.com/problemset/problem/1538/G) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, tham lam, toán học, tìm kiếm bậc ba 
**Thời gian giải:** 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai đống kẹo. Một đống chứa`x`kẹo màu đỏ và cái còn lại chứa`y`kẹo màu xanh. 

Mỗi bộ quà tặng phải sử dụng chính xác`a + b`kẹo. Có hai thành phần có thể: 

-`a`màu đỏ và`b`màu xanh da trời
-`b`màu đỏ và`a`màu xanh 

Hai loại là đối xứng. Đối với mỗi bộ, chúng tôi có thể tự do lựa chọn một trong hai hướng. Một viên kẹo có thể được sử dụng nhiều nhất một lần. 

Nhiệm vụ là tính toán số lượng bộ quà tặng lớn nhất có thể được tạo thành. 

Những hạn chế là đầu mối chính. Tất cả các giá trị có thể lớn bằng`10^9`, và có tới`10^4`trường hợp thử nghiệm. Bất kỳ thuật toán nào cố gắng mô phỏng từng bộ quà tặng đều không thể. Ngay cả một trường hợp thử nghiệm đơn lẻ cũng có thể có câu trả lời gần giống`10^9`. 

Một giải pháp xung quanh`O(log 10^9)`mỗi trường hợp thử nghiệm là hoàn toàn ổn vì`10^4 × 30 ≈ 3 × 10^5`hoạt động. Bất cứ điều gì tuyến tính trong câu trả lời đều bị loại trừ. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```
x = 1, y = 1, a = 2, b = 2
```Mỗi bộ cần tổng cộng bốn viên kẹo, nhưng chỉ tồn tại hai viên kẹo. Câu trả lời là:```
0
```Một kiểm tra ngây thơ chỉ dựa trên việc cân bằng màu sắc có thể cho rằng một bộ là có thể thực hiện được. 

Bây giờ hãy xem xét:```
x = 1000000000
y = 1
a = 1
b = 1000000000
```Có thể thực hiện được một bộ bằng cách lấy`1`màu đỏ và`1000000000`màu xanh hoặc ngược lại. Vì kẹo màu xanh lam rất khan hiếm nên chỉ có thể làm được một bộ. Câu trả lời đúng là:```
1
```Bất kỳ công thức nào chỉ nhìn vào tổng số kẹo sẽ dự đoán sai một câu trả lời lớn hơn nhiều. 

Một trường hợp tế nhị khác xảy ra khi`a = b`.```
x = 10
y = 12
a = b = 3
```Mỗi bộ tiêu thụ đúng ba viên kẹo màu đỏ và ba viên màu xanh. Không còn tự do trong việc định hướng nữa. Câu trả lời trở thành:```
min(10 // 3, 12 // 3) = 3
```Một giải pháp chung vẫn phải xử lý tình trạng thoái hóa này một cách chính xác. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ là đoán xem có bao nhiêu bộ sử dụng hướng thứ nhất và bao nhiêu bộ sử dụng hướng thứ hai. 

Giả sử chúng ta muốn tạo`k`tổng số bộ. Cho phép`t`trong số họ được`(a red, b blue)`Và`k - t`là`(b red, a blue)`. 

Tổng mức tiêu thụ màu đỏ trở thành:```
t·a + (k - t)·b
```và tổng mức tiêu thụ màu xanh lam trở thành:```
t·b + (k - t)·a
```Chúng ta có thể thử mọi giá trị có thể của`t`và kiểm tra tính khả thi. Điều này đúng vì mọi sự sắp xếp đều tương ứng với một số lựa chọn về`t`. 

Vấn đề là ở chỗ đó`k`bản thân nó có thể gần`10^9`. Thậm chí kiểm tra tất cả các giá trị của`t`đối với một câu trả lời của một ứng cử viên là quá đắt. 

Quan sát quan trọng là câu trả lời đều đơn điệu. 

Nếu có thể tạo`k`bộ quà tặng thì cũng có thể tạo số lượng bộ quà tặng nhỏ hơn bất kỳ. Chúng ta có thể đơn giản loại bỏ một số bộ. 

Tính đơn điệu ngay lập tức gợi ý tìm kiếm nhị phân cho câu trả lời. 

Bây giờ vấn đề trở thành: 

> Cho một giá trị ứng viên`k`, chúng ta có thể xác định liệu`k`bộ quà tặng có thể thực hiện được? 

Cho phép`t`một lần nữa biểu thị số lượng tập hợp hướng đầu tiên. 

Những hạn chế về nguồn lực là:```
t·a + (k - t)·b ≤ x
t·b + (k - t)·a ≤ y
```Sau khi sắp xếp lại:```
t(a - b) ≤ x - kb
t(a - b) ≥ ka - y
```Những bất đẳng thức này mô tả một khoảng giá trị hợp lệ cho`t`. 

Thay vì tìm kiếm`t`, chúng ta chỉ cần biết liệu khoảng này có chứa số nguyên hay không. 

Điều đó biến việc kiểm tra tính khả thi thành một phép tính số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) mỗi lần kiểm tra tính khả thi | O(1) | Quá chậm | 
| Tối ưu | O(log(min(x,y))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`a < b`, trao đổi chúng. Điều này đảm bảo`a - b ≥ 0`, làm đơn giản hóa các bất đẳng thức. 
2. Tìm kiếm câu trả lời nhị phân`k`. 
3. Đối với cố định`k`, xác định xem
