---
title: "CF 70A - Bánh quy"
description: "Ta có một hình hộp vuông có kích thước 2^n × 2^n. Bên trong chiếc hộp này, chúng tôi liên tục đặt những chiếc bánh quy hình tam giác đặc biệt. Một cookie có kích thước k chiếm phần hình tam giác phía trên của hình vuông k × k, bao gồm cả đường chéo chính."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 70
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 64"
rating: 1300
weight: 70
solve_time_s: 106
verified: true
draft: false
---

[CF 70A - Cookie](https://codeforces.com/problemset/problem/70/A) 

**Đánh giá:** 1300 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hộp vuông có kích thước`2^n × 2^n`. Bên trong chiếc hộp này, chúng tôi liên tục đặt những chiếc bánh quy hình tam giác đặc biệt. 

Một chiếc bánh quy có kích thước`k`chiếm phần tam giác phía trên của`k × k`hình vuông, kể cả đường chéo chính. Diện tích của nó là:$$1 + 2 + 3 + \dots + k = \frac{k(k+1)}{2}$$Không thể xoay cookie, vì vậy mọi cookie luôn hướng về cùng một hướng. 

Quá trình đóng gói là tham lam. Ở mỗi bước, chúng tôi chọn cookie lớn nhất vẫn có thể vừa với một vị trí nào đó trong khoảng trống còn lại, đặt nó và tiếp tục cho đến khi không còn cookie nào có kích thước tối thiểu.`2`có thể đặt được nữa Cookie có kích thước`1`không tồn tại. 

Nhiệm vụ là tính toán có bao nhiêu ô đơn vị còn trống sau khi quá trình này kết thúc. Vì số tăng nhanh nên đáp án phải được in theo modulo`10^6 + 3`. 

Hạn chế là nhỏ,`n ≤ 1000`, nhưng kích thước bảng thực tế là`2^n × 2^n`, trở nên lớn về mặt thiên văn. Ví dụ, khi`n = 1000`, độ dài cạnh có hơn 300 chữ số thập phân. Bất kỳ cách tiếp cận nào cố gắng xây dựng bảng một cách rõ ràng đều không thể thực hiện được. Ngay cả việc lưu trữ một hàng cũng không thể thực hiện được. 

Điều này ngay lập tức cho chúng ta biết giải pháp phải đến từ việc xác định một mô hình toán học. 

Một trường hợp cạnh tinh tế xuất hiện tại`n = 0`. Kích thước bảng trở nên`1 × 1`, nhưng cookie có kích thước`1`không tồn tại. Chiếc hộp vẫn hoàn toàn trống rỗng. 

đầu vào:```
0
```Đầu ra đúng:```
1
```Việc triển khai bất cẩn cho rằng mọi bảng có thể vừa với ít nhất một cookie sẽ in không chính xác`0`. 

Một sai lầm dễ mắc phải khác là hiểu sai về quá trình tham lam. Việc đóng gói không phải là tùy tiện. Cookie lớn nhất có thể luôn được chọn đầu tiên. Ví dụ, đối với`n = 1`, bảng là`2 × 2`. Một cookie cỡ 2 vừa đúng một lần và bao phủ 3 ô, để lại 1 ô trống. 

đầu vào:```
1
```Đầu ra đúng:```
1
```Nếu ai đó giả định không chính xác các cookie nhỏ hơn cũng có thể được sử dụng sau đó, họ có thể cố gắng lấp đầy ô còn lại bằng cookie cỡ 1 không tồn tại. 

Cái bẫy cuối cùng là tràn số nguyên trong các ngôn ngữ có số nguyên có chiều rộng cố định. Diện tích của bảng là:$$(2^n)^2 = 4^n$$Vì`n = 1000`, con số này rất lớn. Modulo phải được áp dụng trong quá trình lũy thừa thay vì sau khi xây dựng giá trị đầy đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng quá trình đóng gói trên chính lưới. Chúng tôi có thể đại diện cho`2^n × 2^n`bảng, liên tục tìm kiếm cookie lớn nhất phù hợp, đặt nó và tiếp tục cho đến khi không còn vị trí nào. 

Điều này hoạt động với các giá trị nhỏ của`n`. Ví dụ, khi`n = 3`, bảng chỉ`8 × 8`, vì vậy sức mạnh vũ phu có thể quản lý được. Vấn đề là độ dài cạnh tăng gấp đôi với mỗi lần tăng`n`. Tại`n = 20`, bảng đã chứa hơn một nghìn tỷ ô. Ngay cả việc lặp lại trên lưới cũng trở nên không thể. 

Ý tưởng brute-force thất bại vì nó coi bảng như hình học tùy ý, trong khi quy trình thực tế có cấu trúc đệ quy rất cứng nhắc. 

Quan sát quan trọng là việc đóng gói tham lam tạo ra sự phân rã đệ quy hoàn hảo. 

Bắt đầu với một`2^n × 2^n`Cái bảng. Cookie lớn nhất phù hợp với kích thước chính xác`2^n`. Chiếc bánh quy đó chiếm toàn bộ nửa hình tam giác phía trên của bảng. 

Phần còn lại chưa được khám phá là một vùng hình tam giác khác ở nửa dưới bên phải. Sau khi dịch tọa độ, vùng còn lại đó có hình dạng giống hệt bài toán ban đầu về kích thước`2^{n-1}`. 

Điều này có nghĩa là số lượng ô trống sẽ lặp lại. 

Cho phép`f(n)`là số ô không được che phủ cho một bảng có kích thước`2^n × 2^n`. 

Các cookie lớn nhất bao gồm:$$\frac{2^n(2^n+1)}{2}$$Tổng diện tích tấm ván là:$$4^n$$Vùng không được che chắn sau khi đặt cookie lớn nhất sẽ trở thành một trường hợp khác của cùng một vấn đề với tham số`n-1`. 

Thử nghiệm với các giá trị nhỏ cho thấy:$$f(0)=1$$

$$f(1)=1$$

$$f(2)=3$$

$$f(3)=9$$Đây là một tiến trình hình học:$$f(n)=3^{n-1} \quad \text{for } n \ge 1$$Sự truy hồi cũng có thể được bắt nguồn một cách chính thức:$$f(n)=3f(n-1)$$Mỗi phân mục đệ quy tạo ra ba bản sao của mẫu chưa được khám phá trước đó. 

Sau khi xác định được mẫu, vấn đề sẽ chuyển thành lũy thừa mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Kích thước bảng theo cấp số nhân | Kích thước bảng theo cấp số nhân | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Xử lý trường hợp đặc biệt`n = 0`. 

Cái bảng là`1 × 1`và không thể đặt cookie vì kích thước cookie tối thiểu là`2`. Ô duy nhất vẫn trống, vì vậy câu trả lời là`1`. 
3. Đối với`n ≥ 1`, sử dụng công thức dẫn xuất:$$f(n)=3^{n-1}$$Điều này diễn ra từ cấu trúc đệ quy của các vùng chưa được khám phá sau mỗi lần đặt cookie lớn nhất. 

1. Tính modulo công suất`10^6 + 3`. 

Tích hợp sẵn của Python`pow(base, exp, mod)`thực hiện lũy thừa nhị phân nhanh theo thời gian logarit. 
2. In kết quả. 

### Tại sao nó hoạt động 

Quá trình tham lam luôn đặt cookie lớn nhất có thể lên đầu tiên, đó là cookie có kích thước`2^n`. Sau khi đặt nó, phần diện tích còn lại sẽ chia thành các vùng nhỏ hơn tái tạo cấu trúc hình học giống như bài toán ban đầu. 

Mỗi cấp độ đệ quy tạo ra chính xác ba bản sao độc lập của cấu hình chưa được khám phá trước đó. Bởi vì trường hợp cơ sở chứa một ô không được che chắn nên số ô không được che chắn sẽ tăng gấp ba lần ở mọi cấp độ:$$f(n)=3f(n-1)$$Giải quyết sự tái phát này mang lại:$$f(n)=3^{n-1}$$cho tất cả`n ≥ 1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**6 + 3

n = int(input())

if n == 0:
    print(1)
else:
    print(pow(3, n - 1, MOD))
```Việc triển khai được cố ý ngắn gọn vì phần khó của vấn đề là phát hiện ra sự tái diễn chứ không phải mã hóa nó. 

Nhánh đầu tiên xử lý trường hợp ngoại lệ duy nhất. Khi`n = 0`, bảng quá nhỏ để đặt bất kỳ cookie nào, vì vậy câu trả lời chính xác là một ô trống. 

Đối với tất cả các giá trị lớn hơn, giải pháp sử dụng phép truy toán dạng đóng:$$f(n)=3^{n-1}$$Tích hợp sẵn`pow`hàm có ba đối số tính toán lũy thừa mô-đun một cách hiệu quả bằng cách sử dụng phép bình phương lặp lại. Điều này tránh việc xây dựng các số trung gian khổng lồ như`3^999`. 

Một sai lầm dễ mắc phải là sử dụng`3^n`thay vì`3^{n-1}`. Kiểm tra các giá trị nhỏ ngay lập tức phát hiện ra vấn đề: 

cho`n = 1`, câu trả lời phải là`1`, không`3`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| số mũ | 2 | 
| kết quả | 9 | 

Đầu ra:```
9
```Điều này thể hiện mô hình đệ quy gấp ba lần:$$f(3)=3^2=9$$Các vùng chưa được che chắn sau mỗi cấp độ tạo thành ba bản sao của cấu hình trước đó. 

### Ví dụ 2 

đầu vào:```
1
```| Biến | Giá trị | 
| --- | --- | 
| n | 1 | 
| số mũ | 0 | 
| kết quả | 1 | 

Đầu ra:```
1
```Bảng là`2 × 2`. Một cookie cỡ 2 duy nhất bao phủ ba ô, để lại chính xác một ô không được che chắn. 

### Ví dụ 3 

đầu vào:```
0
```| Biến | Giá trị | 
| --- | --- | 
| n | 0 | 
| trường hợp đặc biệt được kích hoạt | vâng | 
| kết quả | 1 | 

Đầu ra:```
1
```Dấu vết này xác nhận điều kiện biên trong đó không thể đặt cookie nào cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | lũy thừa mô-đun nhanh | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Mặc dù kích thước bảng tăng theo cấp số nhân nhưng giải pháp không bao giờ xây dựng được bảng. Thuật toán chỉ tính toán một công suất mô-đun nên dễ dàng nằm trong giới hạn cho`n = 1000`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**6 + 3

def solve():
    input = sys.stdin.readline

    n = int(input())

    if n == 0:
        print(1)
    else:
        print(pow(3, n - 1, MOD))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("3\n") == "9\n", "sample 1"

# minimum size
assert run("0\n") == "1\n", "n = 0"

# smallest non-trivial board
assert run("1\n") == "1\n", "single size-2 cookie"

# small recursive case
assert run("2\n") == "3\n", "tripling starts"

# larger value
assert run("5\n") == "81\n", "3^(5-1)"

# maximum boundary style test
expected = str(pow(3, 999, MOD)) + "\n"
assert run("1000\n") == expected, "large exponent modulo"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| Không có cookie phù hợp | 
|`1`|`1`| Vị trí cookie hợp lệ nhỏ nhất | 
|`2`|`3`| Mở rộng đệ quy đầu tiên | 
|`5`|`81`| Công thức số mũ đúng | 
|`1000`|`pow(3,999,MOD)`| lũy thừa mô-đun lớn | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là bảng nhỏ nhất có thể. 

đầu vào:```
0
```Kích thước bảng là:$$2^0 = 1$$Vì vậy, bảng chứa chính xác một ô. Vì cookie có kích thước`1`không tồn tại, không có gì có thể được đặt. 

Thuật toán ngay lập tức kích hoạt nhánh trường hợp đặc biệt và in:```
1
```Nếu không có nhánh này, công thức`3^{n-1}`sẽ yêu cầu tính toán không chính xác`3^{-1}`. 

Trường hợp cạnh thứ hai là bảng đầu tiên có thể đặt vị trí. 

đầu vào:```
1
```Kích thước bảng là`2 × 2`. Quá trình tham lam đặt một cookie cỡ 2:$$\frac{2(2+1)}{2}=3$$Ba ô được che phủ và một ô vẫn trống. 

Thuật toán tính toán:$$3^{1-1}=3^0=1$$phù hợp với cấu hình thực tế. 

Trường hợp cạnh quan trọng cuối cùng là rất lớn`n`. 

đầu vào:```
1000
```Chiều dài cạnh bảng trở thành`2^{1000}`, vượt xa tính toán rõ ràng. Thuật toán không bao giờ xây dựng bảng hoặc bất kỳ mảng lớn nào. Nó chỉ đánh giá:$$3^{999} \bmod (10^6+3)$$sử dụng lũy ​​thừa thời gian logarit, do đó nó vẫn nhanh và tiết kiệm bộ nhớ.
