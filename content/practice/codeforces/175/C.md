---
title: "CF 175C - Ngựa Hình Học"
description: "Chúng tôi có một số loại hình hình học. Loại i chứa ki các hình giống hệt nhau và mọi hình thuộc loại đó đều có giá trị cơ sở ci. Khi một hình bị phá hủy, số điểm kiếm được bằng: giá trị cơ bản × hệ số hiện tại Hệ số thay đổi theo thời gian. Ban đầu nó là 1."
date: "2026-06-02T16:56:05+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 175
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 115"
rating: 1600
weight: 175
solve_time_s: 61
verified: false
draft: false
---

[CF 175C - Ngựa hình học](https://codeforces.com/problemset/problem/175/C) 

**Đánh giá:** 1600 
**Tags:** tham lam, thực hiện, sắp xếp, hai con trỏ 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số loại hình hình học. Kiểu`i`chứa`k_i`các hình giống hệt nhau và mọi hình thuộc loại đó đều có giá trị cơ bản`c_i`. 

Khi một hình bị phá hủy, số điểm kiếm được bằng:`base value × current factor`Yếu tố thay đổi theo thời gian. Ban đầu nó là`1`. Sau chính xác`p1`số liệu đã bị phá hủy, yếu tố trở thành`2`. Sau chính xác`p2`số liệu đã bị phá hủy, nó trở thành`3`, vân vân. Sau đó`pt`số liệu đã bị phá hủy, yếu tố trở thành`t + 1`. 

Tất cả các nhân vật cuối cùng đều phải bị phá hủy, nhưng chúng ta có thể tự do lựa chọn thứ tự hủy diệt của chúng. Nhiệm vụ là tối đa hóa tổng số điểm. 

Nhận xét quan trọng là hệ số này chỉ phụ thuộc vào số lượng hình đã bị phá hủy chứ không phụ thuộc vào chúng là những hình nào. Điều này có nghĩa là trò chơi tạo ra một số "khe" liên tiếp với các số nhân khác nhau. Chúng ta phải quyết định hình nào chiếm vị trí nào. 

Những hạn chế là bất thường. Có tối đa 100 loại hình, nhưng mỗi loại có thể chứa tối đa`10^9`số liệu và vị trí thay đổi của hệ số có thể lớn bằng`10^12`. Bất kỳ thuật toán nào cố gắng biểu diễn các số liệu riêng lẻ đều không thể thực hiện được. Tổng số hình có thể vượt quá`10^11`, vì vậy chúng ta phải làm việc với số lượng lớn. 

Một chi tiết quan trọng khác là chi phí có thể bằng không. Những số liệu như vậy đương nhiên nên được gán cho các số nhân tệ nhất bất cứ khi nào có thể, bởi vì việc chuyển chúng sang các số nhân tốt hơn sẽ lãng phí các vị trí có giá trị. 

Một sai lầm phổ biến là suy nghĩ về quá trình theo trình tự thời gian và tham lam chọn từng hình tiếp theo. Coi như:```
2
1000000000 1
1 1000
1
1
```Hình bị phá hủy đầu tiên sử dụng hệ số`1`, tất cả những người khác sử dụng yếu tố`2`. 

Chiến lược tối ưu là loại bỏ con số rẻ trước và giữ lại con số đắt để tính hệ số`2`. 

Điểm:```
1*1 + 1000*2 = 2001
```Việc phá hủy con số đắt tiền trước tiên sẽ mang lại:```
1000*1 + 1*2 = 1002
```Một trường hợp tinh vi khác xảy ra khi ranh giới nhân tố nằm ngoài tổng số hình.```
1
5 10
2
100 200
```Chỉ có năm hình tồn tại, vì vậy mọi hình đều bị phá hủy theo hệ số`1`. 

Câu trả lời là:```
50
```Việc thực hiện bất cẩn xử lý một cách mù quáng tất cả các khoảng thời gian từ`p`có thể giả định sai các yếu tố`2`Và`3`bao giờ đạt được. 

Một trường hợp khác nữa là khi nhiều loại hình có cùng chi phí.```
2
3 5
4 5
1
3
```Vì mọi hình đều có giá trị như nhau nên mọi thứ tự đều cho ra câu trả lời giống nhau. Thuật toán vẫn phải hoạt động chính xác mà không cần dựa vào các bất đẳng thức nghiêm ngặt. 

## Phương pháp tiếp cận 

Chế độ xem brute-force rất hữu ích để hiểu cấu trúc. 

Hãy tưởng tượng mở rộng mỗi hình thành một vật phẩm riêng lẻ. Chúng ta sẽ biết chính xác có bao nhiêu ô thuộc về hệ số`1`, có bao nhiêu thuộc về yếu tố`2`, vân vân. Mỗi khe có một số nhân gắn liền với nó. 

Ví dụ:```
Factors by destruction position:
1 1 1 2 2 3 3 3 ...
```Mỗi con số đều góp phần:```
cost × assigned multiplier
```Nếu chúng ta liệt kê rõ ràng tất cả các số liệu và tất cả các vị trí thì bài toán sẽ trở thành bài toán gán. Giải pháp tối ưu rất đơn giản: sắp xếp các số liệu theo chi phí và gán chi phí lớn nhất cho số nhân lớn nhất. 

Điều này diễn ra trực tiếp từ bất đẳng thức sắp xếp lại. Bất cứ khi nào chi phí lớn hơn được kết hợp với hệ số nhân nhỏ hơn trong khi chi phí nhỏ hơn được kết hợp với hệ số nhân lớn hơn, việc hoán đổi chúng sẽ làm tăng hoặc duy trì điểm số. 

Cách tiếp cận bạo lực không thành công vì số lượng các số liệu có thể rất lớn. Một loại duy nhất có thể chứa`10^9`số liệu, do đó việc mở rộng tất cả các số liệu là không thể. 

Quan sát quan trọng là chỉ có 100 loại hình và nhiều nhất là 101 cấp số nhân khác nhau. Chúng tôi không bao giờ cần phải theo dõi các số liệu riêng lẻ. 

Giả sử chúng ta sắp xếp các loại hình theo chi phí theo thứ tự tăng dần. Sau đó, trong phép gán tối ưu, các số liệu có chi phí thấp chiếm các vị trí sớm nhất với số nhân nhỏ, trong khi các số liệu có chi phí cao chiếm các vị trí mới nhất có số nhân lớn. 

Thay vì chỉ định từng hình một, chúng tôi xử lý các khoảng nhân theo thứ tự tăng dần và sử dụng số lượng từ các loại hình đã được sắp xếp. Mỗi khoảng chứa một số vị trí phá hủy có cùng hệ số. Chúng tôi lấp đầy các vị trí đó bằng cách sử dụng các số liệu còn lại rẻ nhất hiện nay. 

Điều này mô phỏng phép gán được sắp xếp tối ưu mà không cần mở rộng số lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(F log F) | O(F) | Quá chậm | 
| Tối ưu | O(n log n + n + t) | O(n) | Đã chấp nhận | 

Đây`F = Σ k_i`, có thể vượt quá`10^11`. 

## Hướng dẫn thuật toán 

### 1. Sắp xếp tất cả các loại hình theo giá thành tăng dần 

Mỗi loại được biểu diễn bằng`(cost, count)`. 

Sự sắp xếp tối ưu đặt các hình giá rẻ vào các ô có hệ số thấp và các hình đắt tiền vào các ô có hệ số cao. 

### 2. Chuyển đổi hệ số thành độ dài khoảng 

Hãy:```
prev = 0
```Đối với mỗi ranh giới`p_i`, tạo một khoảng:```
length = p_i - prev
factor = i
```Sau đó cập nhật:```
prev = p_i
```Các khoảng này mô tả có bao nhiêu vị trí hủy diệt sử dụng từng yếu tố. 

### 3. Khoảng thời gian xử lý từ hệ số nhỏ nhất đến hệ số lớn nhất 

Duy trì một con trỏ tới loại hình rẻ nhất hiện tại mà vẫn còn các số liệu. 

Trong một khoảng thời gian`L`và hệ số`f`, liên tục lấy số liệu từ loại hiện tại cho đến khi:```
L = 0
```hoặc loại đó đã cạn kiệt. 

Nếu chúng ta lấy`x`số liệu chi phí`c`, chúng tôi thêm:```
x * c * f
```để trả lời. 

Sau đó giảm cả độ dài khoảng và số lượng còn lại của loại hình đó. 

### 4. Xử lý tất cả các số liệu còn lại 

Sau khi xử lý mọi khoảng ranh giới, mọi số liệu còn lại sẽ bị hủy theo hệ số:```
t + 1
```Tiêu thụ tất cả số lượng còn lại và thêm:```
count * cost * (t + 1)
```để trả lời. 

### Tại sao nó hoạt động 

Hãy xem xét hai số liệu bất kỳ với chi phí`a ≤ b`và hai khe nhân`x ≤ y`. 

Nếu
