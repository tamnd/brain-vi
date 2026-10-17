---
title: "CF 105556I - \u68cb\u76d8"
description: "Chúng ta có một bàn cờ $n nhân m$ và một mảnh ghép luôn bao phủ chính xác ba ô. Quan sát hình học quan trọng là nếu chúng ta tô màu bàn cờ giống như một bàn cờ tiêu chuẩn thì mọi vị trí quân cờ hợp lệ sẽ bao gồm ba ô cùng màu."
date: "2026-06-25T06:08:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105556
codeforces_index: "I"
codeforces_contest_name: "The 6th FanRuan Cup Southeast University Programming Contest (Winter)"
rating: 0
weight: 105556
solve_time_s: 50
verified: true
draft: false
---

[CF 105556I - \u68cb\u76d8](https://codeforces.com/problemset/problem/105556/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một$n \times m$bàn cờ và một mảnh ghép luôn bao phủ đúng ba ô. 

Quan sát hình học quan trọng là nếu chúng ta tô màu bàn cờ giống như một bàn cờ tiêu chuẩn thì mọi vị trí quân cờ hợp lệ sẽ bao gồm ba ô cùng màu. Một khi đặc tính đó được phát hiện, vấn đề đóng gói ban đầu sẽ trở thành vấn đề đếm trên các ô đen và trắng. 

Cho phép$B$là số lượng tế bào màu đen và$W$là số ô màu trắng trên bảng. 

Mỗi mảnh tiêu thụ chính xác ba ô cùng một màu, vì vậy không có giải pháp nào có thể sử dụng nhiều hơn$$\left\lfloor \frac{B}{3} \right\rfloor +
\left\lfloor \frac{W}{3} \right\rfloor$$miếng. 

Câu hỏi còn lại là liệu giới hạn trên này có luôn đạt được hay không. Gợi ý chính thức hướng tới một đối số mang tính xây dựng: sau khi tách các ô theo màu, mỗi lớp màu sẽ tạo thành một biểu đồ lưới được kết nối. Đồ thị liên thông với$k$các đỉnh có thể được chia thành nhiều thành phần có kích thước rời rạc được kết nối$3$càng tốt, để lại ít hơn ba đỉnh không được sử dụng. Điều đó có nghĩa là mọi lớp màu đều có thể nhận ra chính xác$\lfloor k/3 \rfloor$miếng. Kết hợp hai màu đạt được giới hạn trên. 

Kích thước bảng rất nhỏ từ góc độ tính toán. Khi đã biết công thức, chỉ cần thực hiện một số phép tính số học. 

Một sai lầm phổ biến là cho rằng câu trả lời chỉ đơn giản là$\lfloor nm/3 \rfloor$. Màu sắc quan trọng. 

Ví dụ, trên một$2 \times 2$Cái bảng:```
B W
W B
```Có hai ô đen và hai ô trắng.$$\left\lfloor \frac{2}{3} \right\rfloor +
\left\lfloor \frac{2}{3} \right\rfloor = 0$$trong khi$$\left\lfloor \frac{4}{3} \right\rfloor = 1$$vì vậy việc xử lý tất cả các tế bào cùng nhau sẽ cho kết quả sai. 

Một cạm bẫy dễ dàng khác xuất hiện khi số lượng ô đen và trắng khác nhau. 

Đối với một$3 \times 3$Cái bảng:$$B = 5,\quad W = 4$$Và$$\left\lfloor \frac{5}{3} \right\rfloor +
\left\lfloor \frac{4}{3} \right\rfloor
= 1 + 1 = 2.$$Chỉ sử dụng$\lfloor 9/3 \rfloor = 3$sẽ đánh giá quá cao câu trả lời. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê các vị trí của các mảnh và tìm kiếm cách đóng gói tối đa. Ngay cả đối với kích thước bo mạch vừa phải, điều này cũng không khả thi vì số lượng cấu hình có thể tăng theo cấp số nhân. 

Quan sát quan trọng đã thay đổi hoàn toàn vấn đề. 

Sau khi tô màu bảng theo kiểu bàn cờ, mỗi quân cờ chiếm ba ô cùng màu. Điều đó ngay lập tức tách bảng thành hai tài nguyên độc lập: ô đen và ô trắng. 

Nếu một lớp màu chứa$k$các ô, mỗi mảnh tiêu thụ chính xác ba ô, vì vậy nhiều nhất$\lfloor k/3 \rfloor$mảnh có thể được hình thành từ màu đó. 

Bước còn lại là chứng minh rằng giới hạn trên này có thể đạt được. Đối số về tính kết nối từ gợi ý đảm bảo rằng một tập hợp được kết nối$k$các ô luôn có thể được phân chia thành càng nhiều bộ ba kết nối càng tốt, chỉ để lại tối đa hai ô không sử dụng. Do đó mỗi màu đóng góp chính xác$\lfloor k/3 \rfloor$miếng. 

Tất cả những gì còn lại là tính toán số lượng ô đen và trắng:$$B = \left\lceil \frac{nm}{2} \right\rceil,
\qquad
W = \left\lfloor \frac{nm}{2} \right\rfloor.$$Câu trả lời trở thành$$\left\lfloor \frac{B}{3} \right\rfloor +
\left\lfloor \frac{W}{3} \right\rfloor.$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$m$. 
2. Tính tổng số ô.$$S = n \times m$$1. Tính số ô đen.$$B = \frac{S + 1}{2}$$dùng phép chia số nguyên. 

1. Tính số lượng tế bào màu trắng.$$W = \frac{S}{2}$$dùng phép chia số nguyên. 

1. Tính toán$$\left\lfloor \frac{B}{3} \right\rfloor +
\left\lfloor \frac{W}{3} \right\rfloor.$$1. Xuất kết quả. 

### Tại sao nó hoạt động 

Mỗi quân cờ chiếm đúng ba ô của một màu bàn cờ. Một lớp màu với$k$tế bào không thể đóng góp nhiều hơn$\lfloor k/3 \rfloor$mảnh vì mỗi mảnh tiêu thụ ba ô. 

Các lớp màu là các biểu đồ lưới được kết nối. Đồ thị liên thông với$k$các đỉnh có thể được phân tách nhiều lần thành các thành phần liên thông có kích thước ba, chỉ để lại ít hơn ba đỉnh không được sử dụng. Do đó mỗi lớp màu có thể nhận ra chính xác$\lfloor k/3 \rfloor$miếng. 

Vì các ô đen và trắng độc lập nên số lượng mảnh tối đa là$$\left\lfloor \frac{B}{3} \right\rfloor +
\left\lfloor \frac{W}{3} \right\rfloor.$$## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

s = n * m
black = (s + 1) // 2
white = s // 2

print(black // 3 + white // 3)
```Việc thực hiện theo công thức trực tiếp. 

biểu hiện`(s + 1) // 2`tính toán lớp màu lớn hơn trên bàn cờ, là số màu đen khi tổng số ô là số lẻ. 

biểu hiện`s // 2`tính toán lớp màu nhỏ hơn. 

Phép chia số nguyên cho ba sẽ cho ra số bộ ba hoàn chỉnh có sẵn từ mỗi lớp màu. Cộng hai giá trị đó sẽ tạo ra câu trả lời. 

Không cần vòng lặp, đệ quy hoặc cấu trúc dữ liệu lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
```Tổng số ô:$$S = 4$$| Biến | Giá trị | 
| --- | --- | 
| S | 4 | 
| B | 2 | 
| W | 2 | 
| B // 3 | 0 | 
| W // 3 | 0 | 
| Trả lời | 0 | 

Bàn cờ chỉ có hai ô mỗi màu nên không có màu nào đủ ô để tạo thành một quân cờ. 

### Ví dụ 2 

đầu vào:```
3 3
```| Biến | Giá trị | 
| --- | --- | 
| S | 9 | 
| B | 5 | 
| W | 4 | 
| B // 3 | 1 | 
| W // 3 | 1 | 
| Trả lời | 2 | 

Các tế bào đen có thể tạo thành một bộ ba và các tế bào trắng có thể tạo thành một bộ ba. Một ô đen và một ô trắng vẫn chưa được sử dụng. 

Ví dụ này chứng tỏ tại sao câu trả lời không đơn giản là$\lfloor 9/3 \rfloor = 3$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học | 
| Không gian | O(1) | Sử dụng bộ nhớ bổ sung liên tục | 

Giải pháp thực hiện một lượng công việc cố định bất kể kích thước bảng, do đó, giải pháp này dễ dàng đáp ứng mọi giới hạn cuộc thi hợp lý. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, m = map(int, sys.stdin.readline().split())
    s = n * m
    black = (s + 1) // 2
    white = s // 2

    return str(black // 3 + white // 3)

# custom cases

assert run("1 1\n") == "0", "minimum board"

assert run("2 2\n") == "0", "not enough cells of either color"

assert run("3 3\n") == "2", "odd-sized board"

assert run("3 4\n") == "4", "equal color counts"

assert run("100000 100000\n") == str(((100000 * 100000 + 1) // 2) // 3 + ((100000 * 100000) // 2) // 3), "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Bảng nhỏ nhất có thể | 
|`2 2`|`0`| Không thể tạo thành bộ ba từ một trong hai màu | 
|`3 3`|`2`| Số ô lẻ | 
|`3 4`|`4`| Số lượng đen trắng bằng nhau | 
|`100000 100000`| Giá trị công thức | Số học số lớn | 

## Vỏ cạnh 

Xem xét đầu vào```
2 2
```Bảng có hai ô đen và hai ô trắng. 

Thuật toán tính toán$$B=2,\quad W=2$$và trả về$$0+0=0.$$Một giải pháp ngây thơ sử dụng$\lfloor nm/3 \rfloor$sẽ trở lại$1$, điều này là không thể vì không có lớp màu nào chứa ba ô. 

Bây giờ hãy xem xét```
3 3
```Thuật toán tính toán$$B=5,\quad W=4.$$Câu trả lời trở thành$$\left\lfloor \frac{5}{3} \right\rfloor +
\left\lfloor \frac{4}{3} \right\rfloor
=2.$$Các ô còn lại là một ô đen và một ô trắng, cả hai ô đều không thể tham gia vào quân cờ khác. Công thức xử lý tình trạng còn sót lại này một cách tự nhiên. 

Cuối cùng, đối với một bảng có số ô chẵn như```
3 4
```chúng tôi nhận được$$B=W=6.$$Câu trả lời là$$6/3 + 6/3 = 4.$$Cả hai lớp màu đều được sử dụng hoàn toàn, cho thấy công thức cũng xử lý được trường hợp không còn sót lại.
