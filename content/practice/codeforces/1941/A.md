---
title: "CF 1941A - Rudolf và tấm vé"
description: "Vấn đề cung cấp cho chúng ta hai mảng giá trị tiền xu. Mảng đầu tiên tượng trưng cho những đồng xu trong túi bên trái của Rudolf và mảng thứ hai tượng trưng cho những đồng xu trong túi bên phải của anh ấy."
date: "2026-05-27T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1941
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 933 (Div. 3)"
rating: 800
weight: 1941
solve_time_s: 83
verified: true
draft: false
---
[CF 1941A - Rudolf và tấm vé](https://codeforces.com/problemset/problem/1941/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề cung cấp cho chúng ta hai mảng giá trị tiền xu. Mảng đầu tiên tượng trưng cho những đồng xu trong túi bên trái của Rudolf và mảng thứ hai tượng trưng cho những đồng xu trong túi bên phải của anh ấy. Rudolf phải chọn chính xác một đồng xu từ mỗi túi, nghĩa là chúng ta cần xem xét mọi cặp có thể được hình thành bởi một phần tử từ mảng đầu tiên và một phần tử từ mảng thứ hai. 

Một cặp được coi là hợp lệ nếu tổng của hai đồng xu được chọn nhỏ hơn hoặc bằng`k`. Nhiệm vụ là đếm xem tồn tại bao nhiêu cặp hợp lệ như vậy. 

Chính thức hơn, với mọi chỉ số`i`trong mảng`b`và mọi chỉ số`j`trong mảng`c`, chúng ta phải đếm cặp nếu:$$b_i + c_j \le k$$Câu trả lời cho mỗi trường hợp thử nghiệm chỉ đơn giản là số cặp chỉ số thỏa mãn điều kiện này. 

### Phân tích ràng buộc 

Số lượng ca kiểm thử nhiều nhất là`100`và cả hai kích thước mảng đều thỏa mãn:$$1 \le n, m \le 100$$Điều này là cực kỳ nhỏ. Ngay cả việc kiểm tra mọi cặp có thể cũng yêu cầu nhiều nhất:$$100 \times 100 = 10{,}000$$hoạt động cho mỗi trường hợp thử nghiệm. 

Trên tất cả các trường hợp thử nghiệm, tổng số lần kiểm tra cặp nhiều nhất là:$$100 \times 10{,}000 = 1{,}000{,}000$$có thể dễ dàng quản lý trong giới hạn thời gian 1 giây trong Python. 

Giới hạn bộ nhớ cũng rất hào phóng. Chúng tôi chỉ lưu trữ tối đa hai mảng kích thước nhỏ`100`, do đó việc sử dụng bộ nhớ là không đáng kể. 

Bởi vì các ràng buộc quá nhỏ nên giải pháp vòng lặp kép trực tiếp đã đủ tối ưu. Không cần cấu trúc dữ liệu nâng cao hoặc kỹ thuật sắp xếp. 

### Trường hợp cạnh không rõ ràng 

#### Tất cả các cặp đều không hợp lệ 

Nếu mọi cặp có thể đều vượt quá`k`, câu trả lời phải là`0`. 

Ví dụ:```
b = [10, 20]
c = [30, 40]
k = 15
```Mỗi khoản tiền đều quá lớn. Việc triển khai bất cẩn có thể vô tình đếm thứ gì đó do toán tử so sánh không chính xác. 

#### Tất cả các cặp đều hợp lệ 

Nếu mọi cặp đều thỏa mãn điều kiện thì câu trả lời sẽ là:$$n \times m$$Ví dụ:```
b = [1, 1]
c = [1, 1, 1]
k = 10
```Câu trả lời đúng là`6`. 

#### Giá trị đồng xu trùng lặp 

Sự cố đếm các cặp chỉ số, không phải giá trị riêng biệt. 

Ví dụ:```
b = [1, 1]
c = [2]
```Cả hai`(0,0)`Và`(1,0)`là những lựa chọn hợp lệ khác nhau mặc dù các giá trị giống hệt nhau. 

Một giải pháp sai loại bỏ các bản sao sẽ tạo ra kết quả không chính xác. 

#### Kích thước tối thiểu 

Khi nào`n = 1`Và`m = 1`, chỉ có một cặp có thể có. Thuật toán vẫn phải kiểm tra chính xác số tiền đó. 

#### Bình đẳng ranh giới 

Các cặp có tổng bằng nhau`k`là hợp lệ. 

Ví dụ:```
b = [3]
c = [5]
k = 8
```Điều kiện sử dụng`<=`, không`<`. 

Một bất đẳng thức nghiêm ngặt sẽ loại bỏ các cặp hợp lệ một cách không chính xác. 

## Phương pháp tiếp cận 

### Lực lượng vũ phu 

Cách tiếp cận trực tiếp nhất là thử mọi cặp tiền có thể. Đối với mỗi đồng xu ở túi bên trái, chúng tôi lặp lại từng đồng xu ở túi bên phải và tính tổng của chúng. 

Nếu tổng nhỏ hơn hoặc bằng`k`, chúng tôi tăng câu trả lời. 

Cách tiếp cận này được đảm bảo là chính xác vì nó kiểm tra rõ ràng mọi cặp ứng cử viên hợp lệ chính xác một lần. Vì mỗi cặp được kiểm tra độc lập nên không có cặp hợp lệ nào có thể bị bỏ sót và không có cặp không hợp lệ nào có thể được tính một cách ngẫu nhiên. 

Thông thường, lực lượng vũ phu đối với tất cả các cặp có thể trở nên quá chậm nếu mảng lớn. Tuy nhiên, ở đây cả hai mảng đều có kích thước tối đa`100`, do đó số lần kiểm tra tối đa cho mỗi trường hợp thử nghiệm chỉ là`10,000`. Điều này hoàn toàn có thể chấp nhận được. 

Vì vậy, giải pháp brute-force không chỉ đơn giản mà còn hoàn toàn hiệu quả đối với các ràng buộc đã cho. 

### Phương pháp tiếp cận tối ưu 

Quan sát quan trọng là các ràng buộc là cực kỳ nhỏ. Từ`n`Và`m`mỗi cái nhiều nhất`100`, không cần sắp xếp, tìm kiếm nhị phân hoặc hai con trỏ. 

Một vòng lặp lồng nhau đơn giản đã chạy thoải mái trong giới hạn. 

Thuật toán hoạt động như sau: 

1. Lặp lại từng đồng xu ở túi bên trái. 
2. Đối với mỗi đồng xu như vậy, hãy duyệt qua từng đồng xu ở túi bên phải. 
3. Kiểm tra xem tổng của chúng có lớn nhất không`k`. 
4. Nếu có, hãy tăng bộ đếm. 

Điều này trực tiếp phù hợp với định nghĩa vấn đề và tránh sự phức tạp không cần thiết. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n×m) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n×m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`,`m`, Và`k`. 
3. Đọc mảng`b`đại diện cho đồng tiền túi bên trái. 
4. Đọc mảng`c`đại diện cho những đồng tiền bỏ túi phù hợp. 
5. Khởi tạo bộ đếm`ans = 0`. 
6. Lặp lại qua mọi giá trị`x`TRONG`b`. 
7. Đối với mỗi`x`, lặp qua mọi giá trị`y`TRONG`c`. 
8. Tính toán`x + y`. Nếu tổng nhỏ hơn hoặc bằng`k`, tăng`ans`. 
9. Sau khi kiểm tra tất cả các cặp, hãy in`ans`. 

Điều này hiệu quả vì mọi cặp có thể đều được kiểm tra chính xác một lần. 

## Giải pháp Python```
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, m, k = map(int, input().split())

    b = list(map(int, input().split()))
    c = list(map(int, input().split()))

    ans = 0

    for x in b:
        for y in c:
            if x + y <= k:
                ans += 1

    print(ans)
```Đầu tiên chương trình sẽ đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, nó lưu trữ hai mảng giá trị đồng xu. 

Biến`ans`theo dõi xem có bao nhiêu cặp hợp lệ đã được tìm thấy. 

Các vòng lặp lồng nhau tạo ra mọi sự kết hợp có thể có của một đồng xu ở túi bên trái và một đồng xu ở túi bên phải. Khi tổng thỏa mãn điều kiện`x + y <= k`, câu trả lời được tăng lên một. 

Cuối cùng, câu trả lời cho test case hiện tại được in ra. 

Một chi tiết triển khai quan trọng là vấn đề tính các cặp chỉ mục chứ không phải các giá trị riêng biệt. Bởi vì chúng tôi lặp lại trực tiếp từng phần tử mảng nên các giá trị trùng lặp sẽ tự động được xử lý chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4 8
1 5 10 14
2 1 8 1
```Chúng tôi kiểm tra từng cặp. 

| Đồng xu còn lại | Đúng Xu | Tổng hợp | Có hiệu lực? | Chạy Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | Có | 1 | 
| 1 | 1 | 2 | Có | 2 | 
| 1 | 8 | 9 | Không | 2 | 
| 1 | 1 | 2 | Có | 3 | 
| 5 | 2 | 7 | Có | 4 | 
| 5 | 1 | 6 | Có | 5 | 
| 5 | 8 | 13 | Không | 5 | 
| 5 | 1 | 6 | Có | 6 | 
| 10 | 2 | 12 | Không | 6 | 
| 10 | 1 | 11 | Không | 6 | 
| 10 | 8 | 18 | Không | 6 | 
| 10 | 1 | 11 | Không | 6 | 
| 14 | 2 | 16 | Không | 6 | 
| 14 | 1 | 15 | Không | 6 | 
| 14 | 8 | 22 | Không | 6 | 
| 14 | 1 | 15 | Không | 6 | 

Câu trả lời cuối cùng:```
6
```Dấu vết này cho thấy mọi cặp có thể đều được kiểm tra chính xác một lần. Chỉ có tổng nhỏ hơn hoặc bằng`8`được tính. 

### Ví dụ 2 

đầu vào:```
2 3 4
4 8
1 2 3
```| Đồng xu còn lại | Đúng Xu | Tổng hợp | Có hiệu lực? | Chạy Trả lời | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | 5 | Không | 0 | 
| 4 | 2 | 6 | Không | 0 | 
| 4 | 3 | 7 | Không | 0 | 
| 8 | 1 | 9 | Không | 0 | 
| 8 | 2 | 10 | Không | 0 | 
| 8 | 3 | 11 | Không | 0 | 

Câu trả lời cuối cùng:```
0
```Ví dụ này minh họa trường hợp không tồn tại cặp hợp lệ. Thuật toán trả về chính xác số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n×m) | Mỗi cặp xu được kiểm tra một lần | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng ngoài mảng đầu vào | 

Các vòng lặp lồng nhau chiếm ưu thế trong thời gian chạy. Từ`n`Và`m`nhiều nhất là`100`, số lượng thao tác tối đa cho mỗi trường hợp thử nghiệm chỉ là`10,000`. 

Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ các mảng đầu vào rất nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn nhất định. 

## Vỏ cạnh 

### Tất cả các cặp đều không hợp lệ 

Ví dụ:```
n = 2, m = 2, k = 3
b = [5, 6]
c = [7, 8]
```Mỗi số tiền vượt quá`3`, do đó bộ đếm không bao giờ tăng. Thuật toán xuất ra chính xác:```
0
```### Tất cả các cặp đều hợp lệ 

Ví dụ:```
n = 2, m = 3, k = 100
b = [1, 2]
c = [3, 4, 5]
```Mỗi cặp đều thỏa mãn điều kiện. Thuật toán tính tất cả:$$2 \times 3 = 6$$cặp một cách chính xác. 

### Giá trị trùng lặp 

Ví dụ:```
b = [1, 1]
c = [2]
k = 3
```Cả hai cặp đều hợp lệ:```
(0,0), (1,0)
```Mặc dù các giá trị giống hệt nhau nhưng các chỉ số lại khác nhau. Các vòng lặp lồng nhau tự nhiên tính cả hai. 

### Kích thước đầu vào tối thiểu 

Ví dụ:```
n = 1, m = 1
b = [4]
c = [5]
k = 9
```Chỉ có một cặp tồn tại. Từ`4 + 5 = 9`, câu trả lời là`1`. 

Thuật toán xử lý việc này mà không cần bất kỳ logic đặc biệt nào. 

### Tổng chính xác bằng k 

Ví dụ:```
b = [3]
c = [5]
k = 8
```Tổng bằng`k`, vậy cặp này hợp lệ vì điều kiện sử dụng`<=`. 

Thuật toán đếm chính xác cặp này.
