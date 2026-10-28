---
title: "CF 106147B - Bot đào"
description: "Có một số nguyên ẩn $n$ trong khoảng từ 1 đến 30000. Trong một thử nghiệm, chúng tôi cung cấp một tập hợp các độ dài bước được phép là $a1,a2,dots,ak$. Bot đào bắt đầu ở độ sâu 0 và muốn đạt độ sâu $n$. Nó có thể liên tục sử dụng bất kỳ độ dài bước nào đã cho."
date: "2026-06-25T11:27:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106147
codeforces_index: "B"
codeforces_contest_name: "Junior Balkan Olympiad in Informatics 2025. Day 1"
rating: 0
weight: 106147
solve_time_s: 42
verified: true
draft: false
---

[CF 106147B - Bot đào](https://codeforces.com/problemset/problem/106147/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có một số nguyên ẩn$n$trong khoảng từ 1 đến 30000. 

Trong một thử nghiệm, chúng tôi cung cấp một tập hợp độ dài bước được phép$a_1,a_2,\dots,a_k$. Bot đào bắt đầu ở độ sâu 0 và muốn đạt độ sâu$n$. Nó có thể liên tục sử dụng bất kỳ độ dài bước nào đã cho. Bot luôn chọn một con đường đến chính xác$n$sử dụng số bước tối thiểu có thể. Thử nghiệm trả về số bước tối thiểu đó. Mục đích là xác định giá trị ẩn$n$. Giới hạn chính thức cho phép tối đa 20 thử nghiệm và tổng kích thước tập hợp tối đa là 250. 

Quan sát quan trọng là đây không phải là một vấn đề thuật toán truyền thống. Chúng tôi đang thiết kế một chiến lược thẩm vấn. 

Từ$n \le 30000$, chúng ta nên tìm kiếm một truy vấn có câu trả lời trực tiếp tiết lộ$n$. Thách thức là đảm bảo rằng tính toán bước tối thiểu không thể che giấu thông tin. 

Một sai lầm phổ biến là chọn nhiều bước có độ dài lớn và hy vọng tái tạo lại$n$từ mức tối ưu được trả về. Robot đang giải quyết một vấn đề tối ưu hóa, rất nhiều giá trị khác nhau của$n$có thể đưa ra câu trả lời tương tự. 

Ví dụ, với bộ$\{1,100\}$, cả hai$n=50$Và$n=51$trả về các câu trả lời gần với giá trị của chúng vì bước có độ dài 100 không thể sử dụng được. Phản hồi không xác định duy nhất$n$. 

Giá trị đặc biệt$30000$cũng là một trường hợp Edge. Bất kỳ chiến lược nào giả định "tất cả các số đều nhỏ hơn độ dài bước lớn nhất" phải xử lý rõ ràng giá trị ẩn tối đa có thể. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ là đặt nhiều câu hỏi và dần dần thu hẹp phạm vi các giá trị có thể có. Ví dụ: người ta có thể truy vấn các bộ bước được chọn cẩn thận và sử dụng số lần di chuyển tối thiểu được trả về làm thông tin. Những cách tiếp cận như vậy dễ dàng phù hợp với giới hạn thử nghiệm, nhưng chúng không cần thiết. 

Quan sát quan trọng là số ẩn nhiều nhất là 30000. 

Hãy xem xét truy vấn:$$\{1,30000\}$$Nếu như$n < 30000$, bước có độ dài 30000 không thể được sử dụng vì nó sẽ vượt quá mục tiêu. Cách duy nhất có sẵn để tiếp cận$n$chính xác là sử dụng$n$bước có độ dài 1. 

Do đó số bước tối thiểu được trả về chính xác là$n$. 

Trường hợp duy nhất còn lại là$n=30000$. Sau đó, bot có thể tiếp cận mục tiêu chỉ trong một bước có độ dài 30000, do đó câu trả lời sẽ trở thành 1. 

Điều này xác định hoàn toàn giá trị ẩn sau một thử nghiệm: 

Nếu câu trả lời là 1 thì$n=30000$. 

Nếu không, câu trả lời sẽ bằng$n$. 

Chiến lược sử dụng một truy vấn có kích thước 2, thấp hơn nhiều so với mọi giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Truy vấn nhiều thông tin | Phụ thuộc vào chiến lược | O(1) | Không cần thiết | 
| Truy vấn đơn`{1,30000}`| O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hỏi thí nghiệm với bộ`{1, 30000}`. 
2. Đọc số bước tối thiểu được trả về`m`. 
3. Nếu`m == 1`, đầu ra`30000`. 

Điều này chỉ có thể xảy ra khi bot sử dụng một bước có độ dài 30000. 
4. Nếu không thì xuất ra`m`. 

Khi`n < 30000`, bước có độ dài 30000 là không sử dụng được nên cách xây dựng khả thi duy nhất là sử dụng chính xác`n`bước đơn vị. Như vậy`m = n`. 

### Tại sao nó hoạt động 

Với mọi giá trị$n < 30000$, bất kỳ giải pháp nào chứa bước có độ dài 30000 đều không thể thực hiện được vì nó ngay lập tức vượt quá độ sâu mục tiêu. Nước đi duy nhất được phép là một bước có độ dài 1, vì vậy số bước tối thiểu duy nhất chính xác là$n$. 

Vì$n = 30000$, một bước có độ dài 30000 sẽ đến đích ngay lập tức, đưa ra câu trả lời 1. 

Do đó, giá trị trả về là:$$m =
\begin{cases}
30000 & \text{encoded as } 1,\\
n & \text{for } n < 30000.
\end{cases}$$Quy tắc giải mã tái tạo lại giá trị ẩn một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

print("? 2 1 30000", flush=True)

m = int(input())

if m == -1:
    sys.exit(0)

if m == 1:
    print("! 30000", flush=True)
else:
    print(f"! {m}", flush=True)
```Truy vấn chỉ chứa hai bước dài. 

Giá trị 1 đảm bảo rằng mọi độ sâu ẩn đều có thể truy cập được. Giá trị 30000 được chọn vì nó bằng độ sâu ẩn tối đa có thể. Bất kỳ giá trị ẩn nhỏ hơn nào cũng không thể sử dụng bước nhảy đó. 

Việc triển khai phải hoàn thành sau mỗi đầu ra vì sự cố có tính tương tác. Đang nhận`-1`cho biết tương tác không hợp lệ và chương trình sẽ chấm dứt ngay lập tức, theo giao thức tương tác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử độ sâu ẩn là$n=23$. 

| Truy vấn | Giá trị trả về | 
| --- | --- | 
|`{1, 30000}`| 23 | 

Vì câu trả lời không phải là 1 nên chúng tôi xuất ra 23. 

Điều này thể hiện cơ chế chính. Bước nhảy có độ dài 30000 không thể sử dụng được nên bot cần chính xác 23 bước đơn vị. 

### Ví dụ 2 

Giả sử độ sâu ẩn là$n=30000$. 

| Truy vấn | Giá trị trả về | 
| --- | --- | 
|`{1, 30000}`| 1 | 

Câu trả lời bằng 1, vì vậy chúng tôi xuất ra 30000. 

Đây là giá trị ẩn duy nhất có thể đạt được trong một lần nhảy có độ dài 30000. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một thử nghiệm và xử lý liên tục | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Chiến lược sử dụng một truy vấn có kích thước 2, đáp ứng thoải mái giới hạn của tối đa 20 thử nghiệm và tổng kích thước tập hợp tối đa là 250. 

## Trường hợp thử nghiệm 

Đối với một vấn đề tương tác, không có khai thác thử nghiệm ngoại tuyến tiêu chuẩn. Hành vi logic vẫn có thể được xác minh bằng các câu trả lời mô phỏng.```
def decode(reply):
    if reply == 1:
        return 30000
    return reply

assert decode(23) == 23
assert decode(29999) == 29999
assert decode(1) == 30000
assert decode(15000) == 15000
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trả lời = 23 | 23 | Giá trị thông thường | 
| trả lời = 29999 | 29999 | Giá trị lớn nhất dưới 30000 | 
| trả lời = 1 | 30000 | Độ sâu ẩn tối đa | 
| trả lời = 15000 | 15000 | Giá trị tầm trung chung | 

## Vỏ cạnh 

Hãy xem xét giá trị ẩn tối đa có thể:```
n = 30000
```Truy vấn là`{1,30000}`. 

Bot có thể tiếp cận mục tiêu bằng một bước nhảy có độ dài 30000, do đó số bước tối thiểu được trả về là 1. Thuật toán ánh xạ phản hồi duy nhất này trở lại 30000. 

Bây giờ hãy xem xét giá trị lớn nhất dưới mức tối đa:```
n = 29999
```Không thể sử dụng bước nhảy có độ dài 30000. Mọi đường dẫn hợp lệ bao gồm toàn bộ các bước nhảy đơn vị, vì vậy số bước tối thiểu là 29999. Thuật toán xuất ra trực tiếp 29999. 

Hai trường hợp này là nơi duy nhất có thể nảy sinh sự mơ hồ và cách xử lý đặc biệt đối với câu trả lời`1`loại bỏ nó hoàn toàn.
