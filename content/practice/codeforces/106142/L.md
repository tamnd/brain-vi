---
title: "CF 106142L - \u0422\u0440\u0435\u0442\u044c\u044f \u0441\u0442\u043e\u0440\u043e\u043d\u0430"
description: "Chúng ta có hai cạnh cố định của một tam giác, $a$ và $b$, và chúng ta muốn hiểu giá trị nguyên mà cạnh thứ ba $c$ có thể nhận để ba đoạn tạo thành một tam giác hợp lệ có diện tích dương."
date: "2026-06-19T19:32:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106142
codeforces_index: "L"
codeforces_contest_name: "2025-2026 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 25, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 106142
solve_time_s: 47
verified: true
draft: false
---

[CF 106142L - \u0422\u0440\u0435\u0442\u044c\u044f \u0441\u0442\u043e\u0440\u043e\u043d\u0430](https://codeforces.com/problemset/problem/106142/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cạnh cố định của một tam giác,$a$Và$b$và chúng tôi muốn hiểu giá trị nguyên của cạnh thứ ba$c$có thể thực hiện sao cho ba đoạn tạo thành một tam giác hợp lệ với diện tích dương. 

Một tam giác có diện tích dương chính xác khi nó không suy biến, nghĩa là mọi bất đẳng thức tam giác đều nghiêm ngặt. Vậy cạnh thứ ba phải đồng thời thỏa mãn hai điều kiện: phải đủ ngắn để vừa với hai cạnh còn lại, đồng thời cũng phải đủ dài để tam giác không bị thu gọn thành một đường thẳng. 

Cụ thể, đối với các bên$a$,$b$, Và$c$, tính hợp lệ yêu cầu:$a + b > c$,$a + c > b$, Và$b + c > a$. 

Nhiệm vụ là tìm số nguyên nhỏ nhất và lớn nhất$c$thỏa mãn những điều kiện này. 

Các ràng buộc đi lên đến$10^6$, có nghĩa là mọi giải pháp đều phải chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Một giải pháp lặp lại trên tất cả các giá trị có thể có của$c$lên đến tổng các cạnh sẽ là quá chậm trong trường hợp xấu nhất. Cấu trúc của các ràng buộc gợi ý một đặc tính đại số trực tiếp hơn là liệt kê. 

Một trường hợp hư hỏng khó phát hiện phổ biến là khi một bên rất lớn so với bên kia. Ví dụ, nếu$a = 1$Và$b = 10$, thì nhiều cách triển khai ngây thơ giả định không chính xác bất kỳ$c$giữa$1$Và$11$hiệu quả, nhưng họ quên mất tính nghiêm ngặt của bất đẳng thức và giới hạn dưới do điều kiện tam giác áp đặt$c > |a - b|$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi số nguyên$c$từ 1 đến$a + b - 1$, kiểm tra xem$a, b, c$tạo thành một hình tam giác hợp lệ. Mỗi lần kiểm tra là thời gian không đổi, do đó độ phức tạp trở nên$O(a + b)$, trong trường hợp xấu nhất là về$2 \cdot 10^6$. Mặc dù điều này có thể xảy ra trong một số môi trường, nhưng nó không cần thiết và dễ hỏng, đặc biệt vì logic bên trong vòng lặp chỉ xác minh những bất đẳng thức có thể được đơn giản hóa. 

Quan sát quan trọng là các ràng buộc hợp lệ của tam giác thu gọn thành một khoảng chặt chẽ cho$c$. Từ bất đẳng thức tam giác:$c < a + b$,$c > a - b$,$c > b - a$. 

Hai cái cuối cùng kết hợp thành$c > |a - b|$. Từ$c$là một số nguyên, điều này trở thành$c \ge |a - b| + 1$. Tương tự, giới hạn trên nghiêm ngặt trở thành$c \le a + b - 1$. 

Vì vậy, phạm vi hợp lệ là một phân đoạn số nguyên liên tục:$$|a - b| + 1 \le c \le a + b - 1$$Điều này biến vấn đề thành tính toán đơn giản các điểm cuối của một khoảng thay vì tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(a + b)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta tính phạm vi hợp lệ của cạnh thứ ba trực tiếp từ các bất đẳng thức tam giác. 

1. Tính giới hạn dưới của$c$BẰNG$|a - b| + 1$. Điều này đảm bảo tam giác không bị suy biến và thỏa mãn cả hai$a + c > b$Và$b + c > a$. Sự khác biệt tuyệt đối thể hiện mức độ mạnh hơn của hai ràng buộc dưới. 
2. Tính giới hạn trên của$c$BẰNG$a + b - 1$. Điều này thực thi tính tích cực nghiêm ngặt của khu vực bằng cách đảm bảo$c < a + b$. 
3. Xuất ra hai giá trị dưới dạng câu trả lời. 

### Tại sao nó hoạt động 

Hệ bất đẳng thức tam giác xác định một tập lồi các giá trị khả thi cho$c$. Bởi vì tất cả các ràng buộc là tuyến tính trong$c$, giao điểm của chúng là một khoảng liên tục. Giới hạn dưới xuất phát từ việc đảm bảo$c$lớn hơn cả hai sự khác biệt$a - b$Và$b - a$, trong khi giới hạn trên xuất phát từ ràng buộc tổng. Vì các giá trị số nguyên là bắt buộc nên khoảng thời gian liên tục sẽ chuyển thành một đoạn số nguyên với các điểm cuối được điều chỉnh bằng ±1 do sự bất bình đẳng nghiêm ngặt. Không có khoảng trống hoặc vùng rời rạc nào tồn tại, do đó, điểm cuối mô tả đầy đủ bộ giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input())
b = int(input())

low = abs(a - b) + 1
high = a + b - 1

print(low, high)
```Giải pháp đọc hai số nguyên và tính trực tiếp khoảng hợp lệ. Điểm tinh tế duy nhất là tôn trọng sự bất bình đẳng nghiêm ngặt: giới hạn trên giảm đi một và giới hạn dưới tăng lên vượt quá chênh lệch tuyệt đối. Cả hai sự điều chỉnh này đều cần thiết vì sự bằng nhau sẽ tạo ra một tam giác suy biến. 

Không cần vòng lặp và không cần cấu trúc dữ liệu bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
14
```Chúng tôi tính toán: 

| Bước | một | b | thấp = |a-b|+1 | cao = a+b-1 | 

|------|---|---|-------------------|----------------| 

| Ban đầu | 6 | 14 | 9 | 19 | 

| Kết quả | - | - | 9 | 19 | 

Rất hợp lệ$c$giá trị là từ 9 đến 19. 

Điều này xác nhận rằng khi một bên lớn hơn đáng kể, giới hạn dưới sẽ chiếm ưu thế, ngăn chặn tình trạng suy thoái cấu hình mỏng. 

### Ví dụ 2 

đầu vào:```
1000000
1000000
```| Bước | một | b | thấp | cao | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1e6 | 1e6 | 1 | 1999999 | 

Vì thế$c \in [1, 1999999]$. 

Điều này cho thấy rằng khi các cạnh bằng nhau, giới hạn dưới trở nên tối thiểu và ràng buộc gần như hoàn toàn bị chi phối bởi điều kiện tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học được thực hiện bất kể kích thước đầu vào | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này là tối ưu cho các ràng buộc vì bất kỳ phép lặp nào trên các cạnh thứ ba có thể sẽ là công việc không cần thiết, trong khi câu trả lời cuối cùng chỉ phụ thuộc vào các phép biến đổi đại số đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    a = int(input())
    b = int(input())

    low = abs(a - b) + 1
    high = a + b - 1

    return f"{low} {high}"

# provided sample
assert run("6\n14\n") == "9 19"

# equal sides
assert run("5\n5\n") == "1 9"

# minimal case
assert run("1\n1\n") == "1 1"

# highly unbalanced
assert run("1\n10\n") == "10 10"

# large values
assert run("1000000\n999999\n") == "2 1999998"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 5 | 1 9 | trường hợp đối xứng | 
| 1 1 | 1 1 | giới hạn nhỏ nhất | 
| 1 10 | 10 10 | trường hợp giới hạn dưới chặt chẽ | 
| 1000000 999999 | 2 1999998 | độ đúng ranh giới lớn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi hai cạnh đã cho bằng nhau. Ví dụ,$a = b = 5$. Thuật toán tính toán$low = |5 - 5| + 1 = 1$Và$high = 9$. Điều này cho phép chính xác tất cả các cạnh thứ ba của tam giác hợp lệ từ 1 đến 9. Điểm mấu chốt là giới hạn dưới không thu gọn về 0; các lực bất đẳng thức chặt chẽ ít nhất là 1. 

Một trường hợp cạnh khác là khi một bên lớn hơn nhiều so với bên kia, chẳng hạn như$a = 1$,$b = 10$. Tính toán mang lại kết quả$low = 10$,$high = 10$, nghĩa là chỉ tồn tại một tam giác hợp lệ. Truy tìm sự bất bình đẳng xác nhận điều này:$c$phải lớn hơn$9$và ít hơn$11$, chỉ để lại$10$. 

Cuối cùng, khi cả hai giá trị đều lớn và gần nhau thì khoảng sẽ trở nên rộng. Vì$a = 10^6$,$b = 10^6$, chúng ta có được phạm vi gần như đầy đủ từ 1 đến gần như$2 \cdot 10^6$, cho thấy ràng buộc bị chi phối bởi điều kiện tổng hơn là hiệu.
