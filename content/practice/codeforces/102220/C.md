---
title: "CF 102220C - Giao lộ đường thẳng"
description: "Trong mặt phẳng có n đường thẳng vô hạn. Mỗi dòng được mô tả bởi hai điểm riêng biệt, do đó đầu vào cho bốn số nguyên cho mỗi dòng. Chúng ta cần đếm các cặp đường thẳng có ít nhất một điểm chung. Hai đường thẳng vô hạn không song song luôn cắt nhau đúng một lần."
date: "2026-08-19T00:18:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102220
codeforces_index: "C"
codeforces_contest_name: "The 13th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102220
solve_time_s: 196
verified: true
draft: false
---

[CF 102220C - Giao lộ đường thẳng](https://codeforces.com/problemset/problem/102220/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trong mặt phẳng có n đường thẳng vô hạn. Mỗi dòng được mô tả bởi hai điểm riêng biệt, do đó đầu vào cho bốn số nguyên cho mỗi dòng. Chúng ta cần đếm các cặp đường thẳng có ít nhất một điểm chung. 

Hai đường thẳng vô hạn không song song luôn cắt nhau đúng một lần. Hai đường thẳng song song thường không bao giờ cắt nhau, nhưng có một ngoại lệ: nếu chúng thực sự là cùng một đường hình học thì chúng có vô số điểm chung và cặp đường đó phải được tính. 

Bài toán chính thức có n 10 5 cho mỗi trường hợp thử nghiệm và ∑n 10 6. Giới hạn chính thức là 6 giây và 512 MB. Việc so sánh trực tiếp mỗi cặp thực hiện các phép toán O(n 2 ). Tại n=10 5, tức là 

2 100000⋅99999 ​ =4,999,950,000 

cặp, vượt xa những gì chúng tôi có thể xử lý trong giới hạn thời gian của cuộc thi thông thường. Tổng giới hạn của 106 dòng đầu vào cũng có nghĩa là giải pháp phải gần với O(nlogn), hoặc tốt nhất là O(n) được mong đợi, trong tất cả các trường hợp thử nghiệm. 

Trường hợp tinh tế đầu tiên là các dòng chồng chéo. Coi như```
1
2
0 0 1 1
0 0 2 2
```Cả hai mô tả đều đại diện cho y=x, vì vậy câu trả lời là`1`. Một giải pháp coi mọi cặp đường song song là không giao nhau sẽ xuất ra không chính xác`0`. 

Trường hợp tế nhị thứ hai là hai đường thẳng song song rõ rệt. Ví dụ,```

```Đây là x=0 và x=1, vì vậy câu trả lời là`0`. Giải pháp chỉ kiểm tra xem các vectơ chỉ phương có bằng nhau mà không phân biệt đường thực tế hay không sẽ tính cặp này không chính xác. 

Trường hợp tinh tế thứ ba là cùng một đường hình học có thể được cho với hai điểm đầu của nó bị đảo ngược. Ví dụ,```

```Cả hai dòng đều là y=x, vì vậy câu trả lời là`1`. Một biểu diễn thô chẳng hạn như (dx,dy,c) không sửa dấu của nó có thể biểu diễn hai bản sao này bằng cách sử dụng các bộ ba đối diện và không thể nhận ra rằng chúng giống hệt nhau. 

## Phương pháp tiếp cận 

Giải pháp brute-force xem xét từng cặp đường thẳng và xác định xem chúng có giao nhau hay không. Hai đường thẳng cắt nhau nếu vectơ chỉ phương của chúng không tỷ lệ hoặc nếu chúng tỷ lệ và cả hai phương trình đều mô tả cùng một đường thẳng. Điều này đúng vì cách duy nhất để hai đường thẳng vô hạn không có chung một điểm là chúng phân biệt và song song. 

Vấn đề là số lượng cặp. Với 10 5 dòng có gần năm tỷ cặp, vì vậy ngay cả việc kiểm tra hình học theo thời gian không đổi cho mỗi cặp cũng là quá chậm. Phương pháp brute-force có thời gian O(n 2 ) và không thể khai thác được thực tế là nhiều cặp có cùng mối quan hệ hình học. 

Quan sát quan trọng là mọi cặp đường thẳng không song song đều góp phần tạo ra câu trả lời. Các cặp duy nhất chúng ta cần loại trừ là các cặp đường thẳng song song khác nhau. Điều này thay đổi vấn đề từ việc kiểm tra từng cặp đường thành nhóm các đường theo hướng. 

Đối với một hướng, giả sử có k dòng. Có (2 k ​ ) cặp trong số đó. Mọi cặp như vậy sẽ bị loại trừ vì các đường thẳng song song, ngoại trừ các cặp thực sự là cùng một đường hình học. Nếu một đường hình học cụ thể xuất hiện c lần trong đầu vào, thì ( 2 c ​ ) bản sao của nó sẽ được thêm lại. 

Chúng ta có thể xử lý việc này dần dần. Khi dòng hiện tại là dòng thứ i, có i−1 dòng trước đó và ban đầu chúng ta giả vờ rằng nó cắt tất cả các dòng đó. Nếu p dòng trước có cùng hướng, trong khi q trong số đó thực sự là cùng một đường hình học, thì p−q dòng trước là khác biệt và song song, do đó, chính xác những cặp đó phải được loại bỏ. Dòng hiện tại đóng góp 

(i−1)−(p−q) 

cặp giao nhau mới. 

Nhiệm vụ còn lại là biểu diễn một dòng bằng các số nguyên. Đối với các điểm (x 1​ ,y 1​ ) và (x 2 ​ ,y 2 ​ ), chúng ta có thể viết 

Ax+By=C 

ở đâu 

A=y 1 ​ −y 2 ​ ,B=x 2 ​ −x 1 ​ ,C=Ax 1 ​ +By 1 ​ . 

Ta chia A,B,C cho gcd(∣A∣,∣B∣), sau đó chọn dấu cố định cho (A,B,C). Cặp (A,B) xác định một họ hướng, trong khi (A,B,C) xác định một đường hình học chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n 2 ) | O(1) | Quá chậm | 
| Tối ưu | Dự kiến ​​O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi hai điểm đầu vào của dòng hiện tại thành phương trình Ax+By=C. Các hệ số A và B được lấy trực tiếp từ chênh lệch tọa độ, do đó không cần độ dốc dấu phẩy động. 
2. Chia cả ba hệ số cho gcd(∣A∣,∣B∣). Điều này loại bỏ việc chia tỷ lệ tùy ý, do đó các phương trình như 2x+2y=4 và x+y=2 nhận được biểu diễn chuẩn hóa tương tự. 
3. Sửa dấu của các hệ số chuẩn hóa. Nếu A<0, nhân cả ba với −1. Nếu A=0, yêu cầu B>0, nhân lại với −1 khi cần thiết. Điều này làm cho các điểm cuối đảo ngược tạo ra biểu diễn giống hệt nhau. 
4. Sử dụng (A,B) làm khóa của bản đồ hướng-tần số. Mọi dòng có khóa này đều song song với mọi dòng khác có khóa, kể cả các bản sao của cùng một dòng. 
5. Sử dụng (A,B,C) làm khóa của bản đồ tần số chính xác. Hai đường có cùng một khóa chính xác khi chúng là cùng một đường hình học. 
6. Trước khi chèn đường hiện tại vào một trong hai bản đồ, hãy`same_direction`là số dòng trước có cùng (A, B) và đặt`same_line`là số bản sao chính xác trước đó (A,B,C). 
7. Thêm 

(i−1)−same_direction+same_line 

cho câu trả lời, trong đó i là chỉ mục dựa trên một hiện tại. Thuật ngữ đầu tiên giả định dòng hiện tại cắt mọi dòng trước đó. Dòng thứ hai loại bỏ các dòng trước đó song song và khác biệt. Bản thứ ba khôi phục các bản sao trước đó nằm trên cùng một đường hình học. 

1. Tăng cả hai bản đồ tần số. Sau khi xử lý tất cả các dòng, giá trị tích lũy là số cặp giao nhau cần thiết. 

### Tại sao nó hoạt động 

Tại thời điểm một dòng được xử lý, mọi dòng trước đó đều cắt nó hoặc phân biệt và song song với nó. Số dòng trước đó là i−1. Trong số đó,`same_direction`các đường thẳng song song với đường thẳng hiện tại, nhưng`same_line`trong số đó thực sự là cùng một đường hình học và do đó giao nhau. Như vậy chính xác`same_direction - same_line`các dòng trước không giao nhau. Thuật toán cộng tất cả các cặp khác và không bao giờ tính cặp không giao nhau, vì vậy mỗi cặp được tính chính xác một lần. 

## Giải pháp Python```
Python
```Phần đầu tiên của vòng lặp xây dựng phương trình ẩn của đường thẳng. Sử dụng A=y 1 ​ −y 2 ​ và B=x 2 ​ −x 1 ​ cho một vectơ pháp tuyến của đường thẳng, do đó phương trình đúng cho các đường thẳng đứng và nằm ngang mà không có trường hợp đặc biệt. 

Ước chung lớn nhất loại bỏ hệ số tỷ lệ chung. Vì A và B đã là một vectơ pháp tuyến nên việc chia cho gcd(∣A∣,∣B∣) là đủ để tạo ra hướng nguyên thủy. Ước số tương tự cũng phải được áp dụng cho C. 

Việc chuẩn hóa dấu hiệu là cần thiết. Không có nó, đầu vào```

```sẽ tạo ra các hệ số có dấu ngược lại với```

```mặc dù cả hai đều mô tả cùng một dòng. 

Hai từ điển phục vụ các mục đích khác nhau.`direction_count`cho chúng ta biết có bao nhiêu dòng trước song song với dòng hiện tại.`line_count`cho chúng ta biết có bao nhiêu đường thẳng song song thực sự giống hệt nó. 

biểu thức```
Python
```sử dụng dựa trên số không`i`. Tại lần lặp`i`, chính xác`i`dòng đã được xử lý. Chúng tôi loại bỏ tất cả`same_direction`đường song song và khôi phục lại`same_line`bản sao trùng nhau. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các sản phẩm như`A * x1`không tràn. Điều này quan trọng vì tọa độ có thể có giá trị tuyệt đối lên tới 10 9, làm cho giá trị trung gian của C lớn tới khoảng 2⋅10 18. 

Từ điển được tạo lại cho mọi trường hợp thử nghiệm. Vì mỗi trường hợp thử nghiệm có tối đa 10,5 dòng, điều này giữ cho bộ nhớ tỷ lệ thuận với trường hợp thử nghiệm riêng lẻ lớn nhất thay vì toàn bộ 10,6 dòng trong tất cả các trường hợp. 

## Ví dụ đã hoạt động 

Mẫu chính thức chứa ba trường hợp thử nghiệm: hai đường chéo chéo, hai đường thẳng đứng riêng biệt và hai đường chéo giống hệt nhau. 

Đối với trường hợp thử nghiệm đầu tiên,```

```các đường chuẩn hóa là x−y=0 và x+y=1. 

| Dòng hiện tại | Phím định hướng | Khóa dòng chính xác | Cùng hướng | Cùng dòng | Đã thêm cặp | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| y=x |`(1, -1)`|`(1, -1, 0)`| 0 | 0 | 0 | 0 | 
| x+y=1 |`(1, 1)`|`(1, 1, 1)`| 0 | 0 | 1 | 1 | 

Các phím điều hướng khác nhau nên dòng thứ hai không song song với dòng thứ nhất. Cặp đôi được đếm, tạo ra`1`. 

Đối với trường hợp thử nghiệm thứ hai,```

```các dòng là x=0 và x=1. 

| Dòng hiện tại | Phím định hướng | Khóa dòng chính xác | Cùng hướng | Cùng dòng | Đã thêm cặp | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| x=0 |`(1, 0)`|`(1, 0, 0)`| 0 | 0 | 0 | 0 | 
| x=1 |`(1, 0)`|`(1, 0, 1)`| 1 | 0 | 0 | 0 | 

Dòng thứ hai có cùng hướng nhưng phím dòng chính xác khác. Nó song song và phân biệt nên cặp thế năng bị loại bỏ. 

Đối với trường hợp thử nghiệm thứ ba,```

```cả hai đầu vào đều mô tả chính xác cùng một dòng. 

| Dòng hiện tại | Phím định hướng | Khóa dòng chính xác | Cùng hướng | Cùng dòng | Đã thêm cặp | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| y=x |`(1, -1)`|`(1, -1, 0)`| 0 | 0 | 0 | 0 | 
| y=x |`(1, -1)`|`(1, -1, 0)`| 1 | 1 | 1 | 1 | 

Lúc đầu cặp đôi này có vẻ song song, nhưng`same_line`khôi phục nó vì các đường trùng nhau có vô số điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) dự kiến ​​cho mỗi trường hợp thử nghiệm | Mỗi dòng thực hiện các thao tác từ điển theo thời gian dự kiến ​​không đổi và một phép tính GCD | 
| Không gian | O(n) | Hai từ điển tần số chứa tối đa n phím hướng và dòng riêng biệt | 

Trên tất cả các trường hợp thử nghiệm, thời gian chạy dự kiến ​​là O(∑n), ngoài chi phí logarit của các phép toán GCD số nguyên và ∑n 10 6. Trường hợp thử nghiệm riêng lẻ lớn nhất chỉ có 10 5 dòng, do đó, hai từ điển vẫn có thể quản lý được trong giới hạn bộ nhớ chính thức là 512 MB. 

## Trường hợp thử nghiệm```
Python
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 0 0 1 1`|`0`| Đầu vào tối thiểu và không có cặp tự ghép | 
| Bốn bản sao của y=x |`6`| Mỗi cặp trùng nhau đều phải được tính | 
| Bốn đường thẳng đứng riêng biệt |`0`| Phải loại trừ các đường song song nhưng không trùng nhau | 
| Điểm cuối đảo ngược cộng với các hướng khác |`5`| Chuẩn hóa dấu chuẩn và các mối quan hệ hỗn hợp | 
| Tọa độ độ lớn 10 9 |`3`| Tọa độ biên và số học số nguyên lớn | 

## Vỏ cạnh 

Đối với các đường trùng nhau, hãy xem xét```

```Dòng đầu tiên tạo ra một khóa phương trình chuẩn hóa. Dòng thứ hai tạo ra cùng một khóa sau khi chuẩn hóa dấu hiệu, mặc dù điểm cuối của nó bị đảo ngược. Trước khi xử lý dòng thứ hai,`same_direction = 1`Và`same_line = 1`, do đó phần đóng góp là 1−1+1=1. Câu trả lời cuối cùng là`1`. 

Xét các đường thẳng song song phân biệt```

```Cả 2 dòng đều có phím định hướng`(1, 0)`, nhưng các khóa chính xác của chúng có các giá trị C khác nhau. Dòng thứ hai nhìn thấy`same_direction = 1`Và`same_line = 0`, cho kết quả là 1−1=0. Câu trả lời cuối cùng là`0`. 

Đối với một số bản sao của cùng một đường trộn lẫn với các đường song song, hãy xem xét```

```Hai đường đầu tiên là cùng một đường hình học, đường thứ ba song song với chúng nhưng khác biệt và đường thứ tư thẳng đứng. Khi dòng thứ hai đến, đóng góp của nó là 1−1+1=1, vì nó trùng với dòng đầu tiên. Khi dòng thứ ba đến, có hai dòng trước có cùng hướng nhưng chỉ có một bản sao chính xác của dòng, do đó đóng góp của nó là 2−2+0=0. Đường thứ tư có hướng khác và cắt cả ba đường trước đó, góp phần`3`. Câu trả lời cuối cùng là`4`. 

Việc chuẩn hóa dấu hiệu cũng xử lý các đường thẳng đứng một cách chính xác. Đối với một đường thẳng đứng, A khác 0 và B=0, do đó dạng chuẩn chỉ đơn giản là Ax=C với A>0. Không có phép chia cho 0 vì hai điểm đầu vào được đảm bảo khác biệt nên A và B không thể cùng bằng 0.
