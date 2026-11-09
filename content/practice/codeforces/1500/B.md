---
title: "CF 1500B - Hai đèn chùm"
description: "Chúng ta có hai chuỗi màu sắc theo chu kỳ. Đèn chùm thứ nhất lặp lại mảng a có chiều dài n và đèn chùm thứ hai lặp lại mảng b có chiều dài m. Vào ngày d, đèn chùm đầu tiên hiển thị vị trí (d - 1) mod n, trong khi đèn chùm thứ hai hiển thị vị trí (d - 1) mod m."
date: "2026-06-10T21:03:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "chinese-remainder-theorem", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1500
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 707 (Div. 1, based on Moscow Open Olympiad in Informatics)"
rating: 2200
weight: 1500
solve_time_s: 63
verified: false
draft: false
---

[CF 1500B - Hai đèn chùm](https://codeforces.com/problemset/problem/1500/B) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, Brute Force, định lý số dư Trung Hoa, toán học, lý thuyết số 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi màu sắc theo chu kỳ. 

Chiếc đèn chùm đầu tiên lặp lại một mảng`a`chiều dài`n`, và đèn chùm thứ hai lặp lại một mảng`b`chiều dài`m`. Vào ngày`d`, đèn chùm đầu tiên hiển thị vị trí`(d - 1) mod n`, trong khi thứ hai hiển thị vị trí`(d - 1) mod m`. 

Một số ngày cả hai đèn chùm đều hiển thị cùng một màu. Vào tất cả những ngày khác, chúng hiển thị các màu khác nhau. Chúng ta được yêu cầu tìm ngày có số ngày không khớp chính xác`k`. 

Cái khó đó là`k`có thể lớn như`10^12`. Chúng ta không thể mô phỏng từng ngày một. 

Các giá trị màu sắc khác biệt bên trong mỗi chuỗi. Một màu có thể xuất hiện ở cả hai chuỗi nhưng không bao giờ xuất hiện nhiều hơn một lần trong cùng một chuỗi. Tính duy nhất này là thuộc tính cấu trúc quan trọng của vấn đề. 

Độ dài có thể đạt tới`500000`, vì vậy ngay cả một`O(nm)`việc xử lý trước là không thể. Một giải pháp phải ở gần tuyến tính hoặc gần tuyến tính trong`n + m`. 

Một sai lầm đặc biệt nguy hiểm là cho rằng mô hình này lặp lại mỗi`lcm(n,m)`ngày và sau đó xây dựng rõ ràng tất cả các ngày trong một khoảng thời gian. Khi`n = 499999`Và`m = 500000`, bội chung nhỏ nhất là đại khái`2.5 × 10^11`, quá lớn để có thể liệt kê được. 

Một trường hợp tinh tế khác xảy ra khi một màu chỉ tồn tại trong một chiếc đèn chùm.```
n = 2, m = 2
a = [1, 2]
b = [1, 3]
```Màu sắc`2`và màu sắc`3`không bao giờ có thể tạo ra một ngày phù hợp. Một giải pháp cố gắng ghép mọi vị trí mà không kiểm tra xem màu có tồn tại trong cả hai mảng hay không sẽ tính các kết quả trùng khớp không tồn tại. 

Trường hợp cạnh thứ ba xuất hiện khi một màu chung tồn tại trong cả hai chuỗi nhưng vị trí của nó không tương thích theo modulo`gcd(n,m)`.```
n = 4, m = 6
a = [1, 2, 3, 4]
b = [5, 6, 1, 7, 8, 9]
```Màu sắc`1`xuất hiện ở các vị trí`0`Và`2`. Từ`gcd(4,6)=2`, sự đồng đẳng```
x ≡ 0 (mod 4)
x ≡ 2 (mod 6)
```không có giải pháp. Màu sắc đó không bao giờ tạo ra một ngày phù hợp. Việc triển khai CRT bất cẩn có thể tính sai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi ngày, hãy tính toán vị trí hoạt động trong cả hai chu kỳ, so sánh màu sắc và đếm những điểm không khớp. Khi số lượng không khớp đạt đến`k`, trả về ngày hiện tại. 

Điều này hoạt động vì định nghĩa của quá trình là trực tiếp. Thật không may, câu trả lời có thể ở xung quanh`10^12`, do đó việc mô phỏng sẽ yêu cầu tới hàng nghìn tỷ lần lặp. 

Quan sát đầu tiên là những ngày thú vị duy nhất là những ngày trùng khớp. Nếu chúng ta có thể đếm được có bao nhiêu ngày trùng khớp xảy ra trong số ngày đầu tiên`x`ngày sau đó```
mismatches(x) = x - matches(x).
```Câu trả lời trở thành ngày nhỏ nhất`x`như vậy```
x - matches(x) ≥ k.
```Điều này ngay lập tức gợi ý tìm kiếm nhị phân. 

Vấn đề còn lại là tính số ngày khớp một cách hiệu quả. 

Vì mỗi màu xuất hiện nhiều nhất một lần trong mỗi chuỗi nên một màu có thể tạo ra nhiều nhất một hệ đồng dạng:```
day ≡ position in a (mod n)
day ≡ position in b (mod m)
```Đối với mỗi màu phổ biến, hệ thống này không có nghiệm hoặc nó tạo ra một cấp số cộng của các ngày trùng khớp. Sự tiến triển có thời kỳ`lcm(n,m)`. 

Vì vậy thay vì nghĩ về ngày, chúng ta nghĩ về những màu sắc thông thường. Đối với mỗi màu phổ biến, chúng tôi xác định ngày khớp đầu tiên bằng Định lý số dư Trung Hoa. Khi đó, cho trước một giới hạn`x`, chúng ta đếm có nhiều nhất bao nhiêu số hạng của mỗi cấp số`x`. 

Tổng số màu thông dụng nhiều nhất là`min(n,m)`, nhiều nhất là`500000`, làm cho việc tiền xử lý này trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Quá chậm | 
| Tối ưu | O((n + m) log(max(n,m)) + min(n,m) log(10¹²)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Vòng sơ loại 

Hãy để các vị trí dựa trên số không. 

Giả sử một màu xuất hiện ở vị trí`pa`TRONG`a`và vị trí`pb`TRONG`b`. 

Một ngày phù hợp phải thỏa mãn```
t ≡ pa (mod n)
t ≡ pb (mod m)
```Ở đâu`t`là chỉ số ngày dựa trên số không. 

Cho phép```
g = gcd(n,m).
```Một giải pháp tồn tại nếu```
(pa - pb) mod g = 0.
```Khi nó tồn tại, CRT sẽ đưa ra một giải pháp duy nhất theo modulo```
L = lcm(n,m).
```Gọi giải pháp đó`first`. 

Sau đó tôi
