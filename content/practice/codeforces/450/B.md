---
title: "CF 450B - Jzzhu và Trình tự"
description: "Bài toán xác định một chuỗi tuyến tính $fn$ với hai giá trị ban đầu $x = f1$ và $y = f2$, và một phép truy toán lặp lại mỗi sáu số hạng: $f{n} = f{n-1} - f{n-2}$."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 450
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 257 (Div. 2)"
rating: 1300
weight: 450
solve_time_s: 51
verified: true
draft: false
---

[CF 450B - Jzzhu và Trình tự](https://codeforces.com/problemset/problem/450/B) 

**Đánh giá:** 1300 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề xác định một chuỗi tuyến tính$f_n$với hai giá trị ban đầu$x = f_1$Và$y = f_2$và sự lặp lại lặp lại sau mỗi sáu thuật ngữ:$f_{n} = f_{n-1} - f_{n-2}$. Thực tế, điều này tạo ra một chuỗi có chu kỳ sau: 

-$f_1 = x$-$f_2 = y$-$f_3 = y - x$-$f_4 = -x$-$f_5 = -y$-$f_6 = x - y$-$f_7 = f_1 = x$- Và cứ thế lặp lại 6 học kỳ một lần. 

Đầu vào cung cấp số nguyên$x$,$y$, Và$n$, và chúng tôi được yêu cầu tính toán$f_n$modulo$10^9 + 7$. 

Những hạn chế chỉ ra rằng$x$Và$y$có thể rất lớn (lên tới$\pm 10^9$), Và$n$có thể đạt được$2 \times 10^9$. Điều này ngụ ý rằng việc tính toán chuỗi lặp từ 1 đến$n$sẽ không khả thi vì nó có thể đòi hỏi hàng tỷ thao tác. Vì vậy, một công thức trực tiếp tận dụng mô hình lặp lại 6 số hạng là cần thiết. 

Trường hợp cạnh tinh tế liên quan đến số âm. Vì chúng ta phải trả về kết quả theo modulo$10^9 + 7$, kết quả âm tính phải được chuyển đổi chính xác sang mô đun dương tương ứng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại từ 1 đến$n$sử dụng sự tái phát$f_n = f_{n-1} - f_{n-2}$để tính từng số hạng. Mặc dù cách tiếp cận này đúng về mặt logic nhưng nó không thực tế đối với các$n$, bởi vì$n$có thể lớn tới 2 tỷ, dẫn đến hàng tỷ phép tính số học. 

Cái nhìn sâu sắc quan trọng là nhận ra hành vi tuần hoàn 6 kỳ của chuỗi. Một khi chu trình được xác định, bất kỳ$f_n$có thể được tính trực tiếp bằng cách sử dụng phần còn lại$n \mod 6$. Điều này làm giảm vấn đề từ$O(n)$hoạt động đến một hằng số$O(1)$tra cứu và tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm đối với n lớn | 
| Công thức dựa trên chu kỳ | O(1) | O(1) | Được chấp nhận và hiệu quả | 

## Hướng dẫn thuật toán 

1. Đầu tiên, tính chu kỳ 6 kỳ dựa trên các giá trị ban đầu$x$Và$y$. Trình tự là$[x, y, y - x, -x, -y, x - y]$. 
2. Tính toán$n \mod 6$để xác định vị trí trong chu kỳ. Vì chuỗi được lập chỉ mục 1 nên hãy điều chỉnh chỉ mục bằng cách trừ đi 1:$index = (n-1) \mod 6$. 
3. Truy xuất giá trị từ chu trình tương ứng với chỉ mục này. 
4. Chuyển kết quả về dãy$[0, 10^9 + 6]$sử dụng số học modulo. Nếu số âm thì cộng$10^9 + 7$trước khi dùng modulo. 
5. Trả về giá trị cuối cùng. 

Tại sao nó hoạt động: Tính lặp lại của chuỗi đảm bảo rằng cứ 6 số hạng liên tiếp lặp lại chính xác, do đó việc tính toán$n \mod 6$cung cấp thuật ngữ chính xác bất kể lớn đến mức nào$n$là. Việc xử lý các giá trị âm bằng modulo đảm bảo đầu ra đáp ứng các ràng buộc của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

x, y = map(int, input().split())
n = int(input())

seq = [x, y, y - x, -x, -y, x - y]
result = seq[(n - 1) % 6] % MOD
print(result if result >= 0 else result + MOD)
```Mã này đầu tiên đọc các giá trị đầu vào cho$x$,$y$, Và$n$. Chu kỳ 6 kỳ`seq`được xây dựng và thuật ngữ thích hợp được truy cập thông qua`(n - 1) % 6`để tính đến việc lập chỉ mục dựa trên 1. Hoạt động modulo đảm bảo kết quả nằm trong phạm vi chính xác, xử lý mọi giá trị âm. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2 3
3
```Tính trình tự:`[2, 3, 1, -2, -3, -1]`. Thuật ngữ thứ 3 (0-chỉ mục: 2) là`1`. Hoạt động modulo ở đây không quan trọng, vì vậy đầu ra là`1`. 

Đối với đầu vào:```
2 3
8
```Chỉ số trong chu kỳ:`(8 - 1) % 6 = 1`. Số hạng thứ 2 của dãy là`3`. Mô-đun đầu ra$10^9 + 7$là`3`. 

Đối với đầu vào có kết quả âm:```
2 3
5
```Chỉ mục:`(5 - 1) % 6 = 4`. học kỳ thứ 5 là`-3`. Chuyển đổi mô-đun:`-3 % (10^9 + 7) = 1000000004`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép toán số học và lập chỉ mục theo thời gian không đổi mới được thực hiện | 
| Không gian | O(1) | Chỉ lưu trữ danh sách có độ dài cố định gồm 6 phần tử và một vài biến | 

Thuật toán cực kỳ hiệu quả và hoạt động trong các ràng buộc của bài toán ngay cả ở mức tối đa$n$là 2 tỷ. 

## Trường hợp thử nghiệm```
# sample inputs
assert run("2 3\n3\n") == "1", "sample 1"
assert run("2 3\n5\n") == "1000000004", "sample 2 negative mod"
# minimum n
assert run("1 1\n1\n") == "1", "minimum n"
# maximum n
assert run("1 2\n2000000000\n") == str((2-1) % (10**9+7)), "large n test"
# all negative inputs
assert run("-2 -3\n4\n") == str((-(-2)) % (10**9+7)), "negative x and y"
# cycle repeat
assert run("10 20\n7\n") == "10", "cycle repeat test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "2 3\n3\n" | 1 | Đầu vào mẫu, chuỗi dương bình thường | 
| "2 3\n5\n" | 1000000004 | Kết quả âm tính modulo | 
| "1 1\n1\n" | 1 | Ranh giới n tối thiểu | 
| "1 2\n2000000000\n" | 1 | Ranh giới n tối đa | 
| "-2 -3\n4\n" | 2 | Giá trị ban đầu âm | 
| "10 20\n7\n" | 10 | Trình tự lặp lại sau 6 học kỳ | 

## Vỏ cạnh 

Trường hợp một cạnh liên quan đến số âm. Ví dụ,$x = 2$,$y = 3$,$n = 5$. kỳ thứ 5 là`-3`. Sử dụng modulo ngây thơ mà không cần điều chỉnh có thể quay trở lại`-3`, nhưng đầu ra đúng là`1000000004`. Việc triển khai của chúng tôi bổ sung thêm$MOD$nếu kết quả là âm tính, hãy xử lý việc này một cách chính xác. 

Một trường hợp cạnh khác là khi$n$là rất lớn, ví dụ$n = 2 \times 10^9$. Việc tính toán lặp đi lặp lại sẽ không khả thi. Việc sử dụng tra cứu dựa trên chu kỳ với modulo sẽ làm giảm thời gian tìm kiếm không đổi. 

Trường hợp cạnh thứ ba là khi$n = 1$hoặc$n = 2$. Những giá trị này tương ứng trực tiếp với các giá trị ban đầu$x$Và$y$. Lập chỉ mục modulo của chúng tôi`(n-1) % 6`trả về chính xác phần tử thứ nhất hoặc thứ hai mà không có bất kỳ lỗi nào.
