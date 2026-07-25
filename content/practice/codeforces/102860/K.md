---
title: "CF 102860K - Cờ đam"
description: "Chúng ta có hai loại quân cờ: quân trắng và quân đen. Chúng ta cần sắp xếp tất cả chúng thành một tháp thẳng đứng."
date: "2026-07-25T14:16:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102860
codeforces_index: "K"
codeforces_contest_name: "2020-2021 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 20)"
rating: 0
weight: 102860
solve_time_s: 44
verified: true
draft: false
---

[CF 102860K - Cờ đam](https://codeforces.com/problemset/problem/102860/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai loại quân cờ: quân trắng và quân đen. Chúng ta cần sắp xếp tất cả chúng thành một tháp thẳng đứng. Sọc đen là một khối tối đa liên tiếp của các quân đen, nghĩa là mọi quân đen trong khối chạm vào một quân đen khác ở trên hoặc dưới và khối này được ngăn cách với các khối đen khác bằng quân trắng hoặc bằng các đầu của tháp. 

Nhiệm vụ là tìm ra số lượng sọc đen riêng biệt lớn nhất có thể sau khi chọn thứ tự tốt nhất của tất cả các quân cờ. 

Đầu vào chứa số lượng quân trắng và số lượng quân đen. Đầu ra là số lượng nhóm màu đen tối đa có thể được tạo. 

Các ràng buộc cho phép số lượng cực kỳ lớn, lên tới khoảng 10^18. Điều này ngay lập tức loại trừ mọi mô phỏng, xây dựng tháp hoặc lập trình động theo số lượng mảnh. Lời giải chỉ phải phụ thuộc vào mối quan hệ toán học giữa hai đại lượng và chạy trong thời gian không đổi. 

Các trường hợp cạnh chính xuất phát từ thực tế là các mảnh màu trắng là các dải phân cách. Ví dụ, nếu không có quân đen thì không thể có sọc đen. 

Ví dụ đầu vào:```
5 0
```Đầu ra đúng là:```
0
```Giải pháp luôn trả về ít nhất một sọc sẽ sai. 

Một trường hợp quan trọng khác là có quân đen nhưng không có vạch phân cách màu trắng. 

Ví dụ đầu vào:```
0 3
```Tháp duy nhất có thể có là ba quân đen ghép lại với nhau, vì vậy câu trả lời là:```
1
```Cách tiếp cận bất cẩn trả về số lượng quân đen sẽ xuất ra không chính xác 3. 

Trường hợp biên cuối cùng là khi có đủ quân trắng để tách từng quân đen. 

Ví dụ đầu vào:```
10 3
```Ba quân đen có thể xếp thành`BWBWB`, với các mảnh màu trắng còn lại ở bất kỳ nơi nào khác. Câu trả lời là:```
3
```Giải pháp chỉ coi số quân trắng là đáp án sẽ thất bại vì số sọc không thể vượt quá số quân đen. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất là thử các thứ tự khác nhau của các quân cờ và đếm các sọc kết quả. Điều này đúng vì mọi sự sắp xếp có thể đều được xem xét, nhưng số lượng tháp có thể là rất lớn. Với 10^18 mảnh, ngay cả việc thể hiện tòa tháp cũng không thể thực hiện được, vì vậy mọi cách tiếp cận dựa trên bảng liệt kê hoặc mô phỏng đều không thể hoạt động. 

Quan sát hữu ích là quân trắng có thể tạo ra nhiều nhất một khoảng cách giữa hai nhóm quân đen. Nếu chúng ta có`a`những mảnh màu trắng, họ có thể tạo ra nhiều nhất`a + 1`Những vị trí có thể xuất hiện sọc đen: trước quân trắng đầu tiên, giữa quân trắng và sau quân trắng cuối cùng. 

Đồng thời, mỗi sọc phải có ít nhất một quân đen. Với`b`những mảnh màu đen, chúng ta không thể tạo ra nhiều hơn`b`sọc. 

Số sọc tối đa là số nhỏ hơn trong hai giới hạn sau:`min(number of black pieces, number of possible gaps created by whites)`Sự sắp xếp tối ưu chỉ đơn giản là đặt các quân đen vào càng nhiều khoảng trống càng tốt, sử dụng quân trắng làm dải phân cách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số cách sắp xếp) | O(số mảnh) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số quân trắng và quân đen. 
2. Nếu không có quân đen thì xuất ra`0`, vì một sọc phải có ít nhất một quân đen. 
3. Tính số vị trí có sẵn cho các sọc đen. Với`a`mảnh màu trắng, có`a + 1`các khu vực có thể tách biệt trong tháp. 
4. Câu trả lời là giá trị nhỏ hơn giữa số quân đen và số vùng có sẵn. 

Lý do điều này có tác dụng là vì mỗi sọc đều tiêu tốn ít nhất một quân đen và mọi khoảng cách giữa các sọc đều cần một quân trắng. Công trình luôn có thể đạt được giới hạn này bằng cách đặt một quân đen vào từng vùng đã chọn. 

Tại sao nó hoạt động: 

Giả sử một sự sắp xếp có`x`sọc đen. Vì mỗi sọc có ít nhất một quân đen nên`x`không thể lớn hơn tổng số quân đen. Ngoài ra, giữa mỗi cặp sọc liên tiếp phải có ít nhất một quân trắng, vậy`x`sọc cần ít nhất`x - 1`dải phân cách màu trắng. Điều này có nghĩa là số sọc không thể vượt quá`a + 1`. Vì cả hai hạn chế đều cần thiết nên câu trả lời không thể vượt quá`min(b, a + 1)`. Chúng ta luôn có thể đạt được chính xác số sọc đó bằng cách đặt một quân đen vào từng vùng có sẵn cho đến khi hết quân đen hoặc vùng đen. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())

    if b == 0:
        print(0)
    else:
        print(min(b, a + 1))

if __name__ == "__main__":
    solve()
```Giải pháp đọc trực tiếp hai số đếm và không bao giờ cố gắng xây dựng tòa tháp. Điều này là cần thiết vì các giá trị có thể quá lớn để lặp lại. 

Việc xử lý đặc biệt của`b == 0`không được yêu cầu nghiêm ngặt trong Python vì`min(0, a + 1)`cũng sẽ tạo ra số 0, nhưng việc giữ nguyên điều kiện sẽ làm cho lý luận trở nên rõ ràng. biểu thức`a + 1`là an toàn vì số nguyên Python có độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 2
```Các biến chính phát triển như sau: 

| Bước | Miếng trắng | Miếng màu đen | Các khu vực có sẵn | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 2 | 2 | 2 | 
| Áp dụng tối thiểu | 1 | 2 | 2 | 2 | 

Có một quân trắng nên tháp có thể có hai quân đen. Vì cũng có hai quân đen nên cả hai miền đều có thể nhận được một quân đen. 

### Mẫu 2 

đầu vào:```
5 2
```| Bước | Miếng trắng | Miếng màu đen | Các khu vực có sẵn | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 5 | 2 | 6 | 2 | 
| Áp dụng tối thiểu | 5 | 2 | 6 | 2 | 

Có rất nhiều mảnh phân cách nhưng chỉ tồn tại hai mảnh màu đen. Mỗi quân đen có thể tạo thành một sọc riêng nên câu trả lời là hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Lời giải chỉ phụ thuộc vào hai số nguyên nên dễ dàng khớp với giới hạn ngay cả khi các giá trị lớn tới 10^18. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

def solve():
    a, b = map(int, sys.stdin.readline().split())
    if b == 0:
        print(0)
    else:
        print(min(b, a + 1))

# provided samples
assert run("1 2\n") == "2\n", "sample 1"
assert run("5 2\n") == "2\n", "sample 2"
assert run("0 3\n") == "1\n", "sample 3"

# custom cases
assert run("0 0\n") == "0\n", "no pieces"
assert run("1000000000000000000 1\n") == "1\n", "huge whites"
assert run("1 1000000000000000000\n") == "2\n", "huge blacks"
assert run("10 10\n") == "10\n", "enough separators"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| Không có quân đen nghĩa là không có sọc | 
|`1000000000000000000 1`|`1`| Giá trị rất lớn và một quân đen | 
|`1 1000000000000000000`|`2`| Số lượng sọc bị giới hạn bởi những khoảng trống có sẵn | 
|`10 10`|`10`| Mỗi quân đen đều có thể trở thành sọc riêng của nó | 

## Vỏ cạnh 

Trường hợp không có quân đen:```
0 0
```Thuật toán nhìn thấy`b == 0`và trả về 0 ngay lập tức. Không thể có sọc đen vì tháp không có quân đen. 

Trường hợp không có quân trắng:```
0 3
```Số vùng có sẵn là`0 + 1 = 1`. Công thức trở thành`min(3, 1)`, cho`1`. Tất cả các quân đen phải được kết nối vì không có dải phân cách màu trắng. 

Đối với trường hợp có nhiều quân trắng:```
10 3
```có`10 + 1 = 11`các vùng có thể, nhưng chỉ có ba quân đen. Câu trả lời là`min(3, 11) = 3`. Mỗi quân đen có thể chiếm một vùng khác nhau, tạo thành ba sọc riêng biệt. 

Đối với trường hợp quân đen nhiều nhưng dải phân cách bị hạn chế:```
2 10
```chỉ có`2 + 1 = 3`các khu vực có thể. Mặc dù tồn tại mười quân đen nhưng chúng chỉ có thể tạo thành tối đa ba sọc, do đó kết quả là:```
3
```Thuật toán xử lý vấn đề này bằng cách áp dụng cả hai giới hạn thay vì chỉ xem xét một bên.
