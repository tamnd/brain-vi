---
title: "CF 104021I - Base62"
description: "Chúng ta được cho một số được viết bằng hệ thống chữ số vị trí tùy ý với cơ số $x$, trong đó các chữ số không bị giới hạn ở 0-9 mà mở rộng qua các chữ cái viết hoa và viết thường lên đến tổng số 62 ký hiệu riêng biệt."
date: "2026-07-02T04:36:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104021
codeforces_index: "I"
codeforces_contest_name: "The 2019 ICPC Asia Yinchuan Regional Contest"
rating: 0
weight: 104021
solve_time_s: 49
verified: true
draft: false
---

[CF 104021I - Base62](https://codeforces.com/problemset/problem/104021/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số được viết bằng hệ thống chữ số vị trí tùy ý với cơ số$x$, trong đó các chữ số không bị giới hạn ở 0-9 mà mở rộng qua các chữ cái viết hoa và viết thường lên đến tổng số 62 ký hiệu riêng biệt. Nhiệm vụ của chúng ta là diễn giải chuỗi này dưới dạng một giá trị nguyên và sau đó xuất ra cùng một số nguyên được biểu thị trong một cơ sở khác$y$, sử dụng cùng bảng chữ cái 62 ký hiệu. 

Nói một cách cụ thể hơn, đầu vào cung cấp cho chúng ta cơ sở nguồn, cơ sở đích và mã hóa chuỗi của một số trong cơ sở nguồn. Trước tiên chúng ta phải giải mã chuỗi đó thành giá trị số nguyên thực của nó, sau đó mã hóa lại số nguyên đó vào cơ sở đích. 

Các ràng buộc không đủ lớn để yêu cầu các thư viện số học có độ chính xác tùy ý, nhưng sự hiện diện của các cơ số lên tới 62 và các chuỗi đầu vào có thể dài có nghĩa là chúng ta không thể dựa vào các hàm chuyển đổi cơ số tích hợp sẵn. Chúng ta phải mô phỏng việc chuyển đổi một cách cẩn thận bằng cách sử dụng số học số nguyên hoặc xử lý chữ số thủ công. 

Điểm tinh tế chính là số đầu vào được cung cấp dưới dạng một chuỗi trong cơ sở$x$, vì vậy các số 0 đứng đầu và ánh xạ ký tự rất quan trọng. Việc triển khai bất cẩn thường thất bại khi xử lý các ký tự dưới dạng giá trị ASCII thô thay vì ánh xạ chúng thành các giá trị số dự kiến. 

Một số trường hợp đặc biệt cần được làm rõ. 

Nếu số đầu vào là`"0"`, bất kể cơ sở, đầu ra đúng là`"0"`trong bất kỳ cơ sở mục tiêu nào. Vòng lặp chuyển đổi đơn giản không bao giờ xử lý số 0 riêng biệt có thể tạo ra một chuỗi trống. 

Nếu đầu vào chỉ sử dụng các chữ cái, chẳng hạn như`"FB"`ở cơ sở 16, rất dễ lập bản đồ sai`'F'`sang giá trị ASCII của nó thay vì 15. Ví dụ: thông dịch`'F' - '0'`đưa ra một giá trị vô nghĩa và phá vỡ chuyển đổi. 

Nếu số rất lớn, chẳng hạn như một chuỗi dài trong cơ sở 62, việc phân tích cú pháp số nguyên trực tiếp thông qua phép lũy thừa đơn giản có nguy cơ bị tràn hoặc tính toán quá mức, trong khi việc tích lũy từng chữ số vẫn an toàn. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng chuyển đổi chuỗi đầu vào thành một số nguyên bằng cách sử dụng phép lũy thừa lặp lại: đối với mỗi vị trí, hãy tính$digit \times x^{power}$và tổng hợp chúng. Điều này đúng về mặt toán học, nhưng khả năng tính toán liên tục dẫn đến công việc lặp đi lặp lại không cần thiết. Đối với một chuỗi có độ dài$n$, lũy thừa ngây thơ cho mỗi chữ số làm cho độ phức tạp$O(n^2)$, vì mỗi lần tính toán công suất tốn tới$O(n)$nếu được thực hiện lặp đi lặp lại. 

Một quan sát tiêu chuẩn và hiệu quả hơn là các số vị trí không cần lũy thừa rõ ràng. Thay vào đó, chúng ta có thể xây dựng giá trị tăng dần từ trái sang phải. Mỗi chữ số mới sẽ thay đổi giá trị hiện tại theo hệ số$x$, sau đó thêm chữ số. Điều này biến việc chuyển đổi thành quét tuyến tính. 

Khi chúng tôi nhận được giá trị số nguyên, hãy chuyển đổi nó thành giá trị cơ sở$y$là đối xứng. Chúng tôi liên tục chia cho$y$, thu thập số dư, tương ứng trực tiếp với các chữ số trong biểu diễn cơ số mới. Đây là sự phân rã tham lam tiêu chuẩn của một số nguyên trong hệ thống vị trí. 

Ý tưởng cấu trúc chính là cả hai hướng chuyển đổi đều giảm xuống việc áp dụng lặp đi lặp lại cùng một nhận dạng: biểu diễn cơ số chỉ là một chuỗi các phép nhân với cơ số cộng với phép cộng các chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng sức mạnh lặp đi lặp lại | O(n²) | O(1) | Quá chậm | 
| Phân tích tuyến tính + phép chia lặp lại | O(n + log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia quy trình thành hai giai đoạn: giải mã chuỗi đầu vào thành một giá trị số nguyên, sau đó mã hóa số nguyên đó vào cơ sở đích. 

### 1. Chuẩn bị ánh xạ chữ số 

Chúng tôi xác định ánh xạ từ ký tự đến giá trị:`'0'-'9'`bản đồ tới 0-9,`'A'-'Z'`bản đồ tới 10-35, và`'a'-'z'`bản đồ tới 36-61. Điều này cho phép giải thích theo thời gian liên tục của từng chữ số. 

Ánh xạ này rất cần thiết vì cách biểu diễn không phải là số học dựa trên ASCII mà là bảng chữ cái tùy chỉnh. 

### 2. Giải mã số cơ số x thành số nguyên 

Chúng tôi khởi tạo một bộ tích lũy`value = 0`. Đối với mỗi nhân vật`c`trong chuỗi đầu vào, chúng tôi chuyển đổi nó thành giá trị số của nó`d`, sau đó cập nhật:$$value = value \cdot x + d$$Mỗi bước dịch chuyển số hiện tại sang một chữ số trong cơ số$x$và chèn chữ số mới. 

Điều này hoạt động vì ký hiệu vị trí xác định các số là:$$(((d_1 \cdot x + d_2)\cdot x + d_3)\cdots)$$Vì vậy, sự truy hồi tái tạo lại chính xác giá trị dự định. 

### 3. Xử lý số 0 một cách rõ ràng 

Nếu đầu vào là`"0"`(hoặc giá trị tính toán bằng 0), chúng ta sẽ xuất ngay`"0"`. Điều này tránh tạo ra đầu ra trống trong quá trình chuyển đổi. 

### 4. Chuyển số nguyên sang biểu diễn cơ số y 

Chúng tôi liên tục lấy phần còn lại theo modulo$y$để trích xuất chữ số có ý nghĩa nhỏ nhất trong cơ sở$y$. Mỗi phần còn lại được ánh xạ trở lại một ký tự bằng cách sử dụng mặt trái của ánh xạ chữ số. 

Chúng tôi thu thập các chữ số theo thứ tự ngược lại vì trích xuất modulo tạo ra các chữ số có ý nghĩa nhỏ nhất trước tiên. 

Sau vòng lặp, chúng ta đảo ngược các ký tự đã thu thập để có được biểu diễn cuối cùng. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến. Trong quá trình giải mã, sau khi xử lý dữ liệu đầu tiên$i$nhân vật,`value`bằng số nguyên được biểu thị bằng tiền tố độ dài$i$trong căn cứ$x$. Trong quá trình mã hóa, tại mỗi bước, số còn lại`value`chính xác là thương số sau khi loại bỏ các chữ số bậc thấp hơn đã được phát ra, do đó, mỗi phép toán modulo sẽ trích xuất đúng chữ số tiếp theo trong cơ số$y$. Các thuộc tính này đảm bảo cả hai giai đoạn tái tạo lại cùng một số nguyên mà không có sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def char_to_val(c):
    if '0' <= c <= '9':
        return ord(c) - ord('0')
    if 'A' <= c <= 'Z':
        return ord(c) - ord('A') + 10
    return ord(c) - ord('a') + 36

def val_to_char(v):
    if v < 10:
        return chr(ord('0') + v)
    if v < 36:
        return chr(ord('A') + v - 10)
    return chr(ord('a') + v - 36)

x, y, z = input().split()
x = int(x)
y = int(y)

value = 0
for c in z.strip():
    value = value * x + char_to_val(c)

if value == 0:
    print("0")
    sys.exit()

res = []
while value > 0:
    res.append(val_to_char(value % y))
    value //= y

print("".join(reversed(res)))
```Vòng giải mã là cốt lõi của giải pháp. Nó tránh hoàn toàn lũy thừa và duy trì một giá trị cuộn luôn đại diện cho tiền tố được phân tích cú pháp trong cơ sở$x$. Vòng lặp mã hóa phản ánh phép chia dài: mỗi lần lặp sẽ loại bỏ cơ sở ít quan trọng nhất$y$chữ số. 

Một lỗi phổ biến là quên loại bỏ các ký tự dòng mới khỏi chuỗi đầu vào, điều này sẽ làm hỏng quá trình phân tích chữ số. Một cách khác là xử lý số 0 không chính xác, nếu không sẽ dẫn đến kết quả trống`res`danh sách và đầu ra không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
16 2 FB
```Chúng tôi giải thích`"FB"`ở căn cứ 16. 

| Bước | Nhân vật | Giá trị chữ số | Giá trị đang chạy | 
| --- | --- | --- | --- | 
| 1 | F | 15 | 15 | 
| 2 | B | 11 | 15 * 16 + 11 = 251 | 

Bây giờ chuyển đổi 251 sang cơ số 2. 

| Bước | Giá trị | Phần còn lại | Giá trị tiếp theo | 
| --- | --- | --- | --- | 
| 1 | 251 | 1 | 125 | 
| 2 | 125 | 1 | 62 | 
| 3 | 62 | 0 | 31 | 
| 4 | 31 | 1 | 15 | 
| 5 | 15 | 1 | 7 | 
| 6 | 7 | 1 | 3 | 
| 7 | 3 | 1 | 1 | 
| 8 | 1 | 1 | 0 | 

Đảo ngược số dư cho`11111011`. 

Dấu vết này xác nhận rằng việc xây dựng từng chữ số khớp chính xác với đánh giá vị trí. 

### Ví dụ 2 

đầu vào:```
62 10 z
```Đây`'z'`là giá trị chữ số lớn nhất, 61. 

| Bước | Nhân vật | Giá trị chữ số | Giá trị đang chạy | 
| --- | --- | --- | --- | 
| 1 | z | 61 | 61 | 

Bây giờ chuyển đổi 61 thành cơ số 10. 

| Bước | Giá trị | Phần còn lại | Giá trị tiếp theo | 
| --- | --- | --- | --- | 
| 1 | 61 | 1 | 6 | 
| 2 | 6 | 6 | 0 | 

Đầu ra là`61`. 

Điều này thể hiện việc xử lý các đầu vào ký tự đơn và chữ số giới hạn trên trong cơ sở 62. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + log_y N) | Quét tuyến tính để giải mã chuỗi cộng với phép chia lặp lại để mã hóa | 
| Không gian | O(1) | Chỉ lưu trữ số nguyên trung gian và chữ số đầu ra | 

Độ dài đầu vào được giới hạn sao cho đủ để xử lý tuyến tính và chuyển đổi số nguyên đủ nhanh theo các số nguyên chính xác tùy ý của Python. Vòng chia chạy theo thời gian logarit tương ứng với giá trị số, nằm trong giới hạn cho các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def char_to_val(c):
        if '0' <= c <= '9':
            return ord(c) - ord('0')
        if 'A' <= c <= 'Z':
            return ord(c) - ord('A') + 10
        return ord(c) - ord('a') + 36

    def val_to_char(v):
        if v < 10:
            return chr(ord('0') + v)
        if v < 36:
            return chr(ord('A') + v - 10)
        return chr(ord('a') + v - 36)

    x, y, z = input().split()
    x = int(x)
    y = int(y)

    value = 0
    for c in z.strip():
        value = value * x + char_to_val(c)

    if value == 0:
        return "0"

    res = []
    while value > 0:
        res.append(val_to_char(value % y))
        value //= y

    return "".join(reversed(res))

# provided sample
assert run("16 2 FB") == "11111011"

# minimum size
assert run("2 2 0") == "0"

# single digit max base
assert run("62 10 z") == "61"

# base identity
assert run("10 10 12345") == "12345"

# binary to hex
assert run("2 16 1111") == "F"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 16 2 FB | 11111011 | Phân tích cú pháp nhiều chữ số và chuyển đổi nhị phân | 
| 2 2 0 | 0 | Không xử lý | 
| 62 10z | 61 | Giá trị chữ số tối đa | 
| 10 10 12345 | 12345 | Chuyển đổi danh tính | 
| 2 16 1111 | F | Chuyển đổi cơ sở sức mạnh của hai | 

## Vỏ cạnh 

các`"0"`trường hợp đầu vào là trường hợp cạnh quan trọng nhất. Nếu chúng ta chạy vòng giải mã,`value`vẫn bằng 0 và không có bộ bảo vệ, vòng lặp mã hóa sẽ tạo ra một danh sách trống. Kiểm tra số 0 rõ ràng đảm bảo chúng tôi quay lại ngay lập tức`"0"`. 

Đối với trường hợp như`"0"`trong cơ sở 62: 

Giải mã giữ`value = 0`. Chúng tôi bỏ qua chuyển đổi và đầu ra`"0"`trực tiếp. Điều này bảo tồn tính chính xác trên tất cả các cơ sở. 

Đầu vào chữ số tối đa một ký tự như`"z"`kiểm tra xem ánh xạ chữ số có đạt đến 61 một cách chính xác hay không. Sau đó, quá trình chuyển đổi giảm xuống một chuỗi thao tác modulo đơn và thuật toán vẫn tạo ra các chữ số chính xác mà không cần xử lý đặc biệt. 

Những trường hợp này xác nhận rằng cả lớp ánh xạ và lớp chuyển đổi số học đều hoạt động nhất quán trên toàn bộ phạm vi 2-62.
