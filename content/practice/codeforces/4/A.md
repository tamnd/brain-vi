---
title: "CF 4A - Dưa Hấu"
description: "Bài toán đưa ra một số nguyên w, trọng lượng của một quả dưa hấu. Chúng ta cần quyết định xem liệu nó có thể được chia thành hai phần dương sao cho cả hai phần đều là số chẵn hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 4
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 4 (Div. 2 Only)"
rating: 800
weight: 4
solve_time_s: 46
verified: true
draft: false
---
[CF 4A - Dưa hấu](https://codeforces.com/problemset/problem/4/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một số nguyên duy nhất`w`, trọng lượng của một quả dưa hấu. Chúng ta cần quyết định xem liệu nó có thể được chia thành hai phần dương sao cho cả hai phần đều là số chẵn hay không. 

Từ "tích cực" quan trọng ở đây. Tách`2`vào trong`0 + 2`không được phép vì một phần sẽ có trọng lượng bằng không. Cả hai đứa trẻ phải nhận được một phần tích cực nghiêm ngặt. 

Những hạn chế là rất nhỏ,`w`nhiều nhất là`100`. Ngay cả một giải pháp vũ phu chậm cũng có thể dễ dàng chạy trong thời gian giới hạn. Điều đó có nghĩa là thách thức không phải là hiệu suất, mà là việc nhận ra điều kiện toán học chính xác để có thể thực hiện được việc phân chia. 

Nhận xét quan trọng đầu tiên là tổng của hai số chẵn luôn là số chẵn. Vậy nếu`w`thật kỳ lạ, câu trả lời là ngay lập tức`NO`. 

Quan sát thứ hai ít rõ ràng hơn. Kể cả nếu`w`bản thân nó là chẵn, một số giá trị vẫn thất bại. Số chẵn nhỏ nhất là`2`và không thể chia nó thành hai số chẵn dương. Sự phân chia duy nhất có thể là`1 + 1`, sử dụng số lẻ, hoặc`0 + 2`, điều này không hợp lệ vì số 0 không dương. 

Việc thực hiện bất cẩn thường chỉ kiểm tra xem liệu`w`là chẵn. Điều đó không thành công cho đầu vào:```
2
```Một điều kiện ngây thơ như`w % 2 == 0`sẽ in`YES`, nhưng câu trả lời đúng là`NO`. 

Một trường hợp cạnh khác là giá trị lẻ nhỏ nhất:```
1
```Không có cách nào để chia một số lẻ thành hai số chẵn vì số chẵn cộng với số chẵn luôn là số chẵn. Đầu ra đúng là`NO`. 

Trường hợp hợp lệ đầu tiên là:```
4
```Chúng ta có thể chia nó thành`2 + 2`, vừa dương vừa chẵn. Đầu ra đúng là`YES`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là thử mọi phần đầu tiên có thể từ`1`ĐẾN`w - 1`. Với mỗi giá trị`a`, tính phần thứ hai là`b = w - a`. Nếu cả hai`a`Và`b`chẵn và dương, chúng tôi đã tìm thấy một sự phân chia hợp lệ. 

Điều này hiệu quả vì nó kiểm tra mọi phân vùng có thể có của dưa hấu. Với`w ≤ 100`, giải pháp brute-force thực hiện tối đa`99`kiểm tra, điều đó hoàn toàn ổn. 

Tuy nhiên, cách tiếp cận vũ phu đang làm được nhiều việc hơn mức cần thiết. Cấu trúc thực sự của vấn đề xuất phát từ tính chẵn lẻ. 

Nếu hai số chẵn dương cộng lại bằng`w`, sau đó`w`bản thân nó phải chẵn. Điều đó loại bỏ mọi số lẻ ngay lập tức. 

Bây giờ hãy xem xét các số chẵn. Bất kỳ số chẵn nào lớn hơn`2`luôn có thể được viết là:```
2 + (w - 2)
```Nếu như`w > 2`và thậm chí, sau đó`w - 2`cũng đều và tích cực. Điều đó đảm bảo sự phân chia hợp lệ. 

Vì vậy, toàn bộ vấn đề giảm xuống một điều kiện:```
w is even and w > 2
```Điều đó đưa ra một giải pháp toán học O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(w) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`w`. 

Đây là tổng trọng lượng của dưa hấu. 
2. Kiểm tra xem`w`là chẵn. 

Nếu như`w`là số lẻ, nó không thể được biểu diễn dưới dạng tổng của hai số chẵn, vì vậy câu trả lời phải là`NO`. 
3. Kiểm tra xem`w`lớn hơn`2`. 

giá trị`2`là số chẵn nhưng không thể chia thành hai phần chẵn dương. Mọi số chẵn lớn hơn`2`hoạt động. 
4. Nếu cả hai điều kiện đều đúng, hãy in`YES`. Ngược lại, in`NO`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

w = int(input())

if w % 2 == 0 and w > 2:
    print("YES")
else:
    print("NO")
```Chương trình đọc trọng lượng dưa hấu và áp dụng trực tiếp điều kiện toán học. 

biểu hiện`w % 2 == 0`kiểm tra xem số đó có phải là số chẵn không. Điều kiện thứ hai,`w > 2`, loại bỏ trường hợp chẵn không hợp lệ duy nhất. 

Thứ tự kiểm tra không quan trọng ở đây vì cả hai đều là hoạt động liên tục. Không có lo ngại về tràn vì kích thước đầu vào cực kỳ nhỏ. 

Một sai lầm dễ mắc phải là quên yêu cầu tích cực cho cả hai phần. Nếu không có`w > 2`điều kiện, đầu vào`2`sẽ quay lại không chính xác`YES`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
```| Bước | Giá trị | 
| --- | --- | 
| Đọc`w`| 8 | 
|`w % 2 == 0`| Đúng | 
|`w > 2`| Đúng | 
| Đầu ra | CÓ | 

số`8`chẵn và lớn hơn`2`. Một phép chia hợp lệ là`2 + 6`. 

### Ví dụ 2 

đầu vào:```
2
```| Bước | Giá trị | 
| --- | --- | 
| Đọc`w`| 2 | 
|`w % 2 == 0`| Đúng | 
|`w > 2`| Sai | 
| Đầu ra | KHÔNG | 

Ví dụ này minh họa trường hợp khó khăn. Mặc dù`2`là số chẵn thì không có cách nào chia nó thành hai số nguyên dương chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số kiểm tra số học được thực hiện | 
| Không gian | O(1) | Không sử dụng thêm bộ nhớ | 

Giải pháp dễ dàng phù hợp trong giới hạn. Nó thực hiện công việc liên tục bất kể giá trị đầu vào và chỉ sử dụng một vài biến. 

## Trường hợp thử nghiệm 

### Trường hợp thử nghiệm 1 

đầu vào:```
1
```Sản lượng dự kiến:```
NO
```Điều này xác minh rằng số lẻ bị từ chối ngay lập tức. 

### Trường hợp thử nghiệm 2 

đầu vào:```
4
```Sản lượng dự kiến:```
YES
```Đây là trọng lượng dưa hấu hợp lệ nhỏ nhất. 

### Trường hợp thử nghiệm 3 

đầu vào:```
2
```Sản lượng dự kiến:```
NO
```Điều này nắm bắt các triển khai chỉ kiểm tra xem số đó có phải là số chẵn hay không. 

### Trường hợp thử nghiệm 4 

đầu vào:```
100
```Sản lượng dự kiến:```
YES
```Điều này kiểm tra ranh giới trên của các ràng buộc. 

## Vỏ cạnh 

Trường hợp cạnh không rõ ràng đầu tiên là:```
2
```Thuật toán kiểm tra`w % 2 == 0`, đó là sự thật. Sau đó nó kiểm tra`w > 2`, điều đó là sai. Vì cả hai điều kiện đều không được thỏa mãn nên chương trình sẽ in`NO`. 

Điều này xử lý chính xác yêu cầu tích cực. Sự phân rã chẵn duy nhất sẽ liên quan đến số 0, điều này không được phép. 

Một trường hợp cạnh quan trọng khác là:```
1
```Thuật toán kiểm tra`w % 2 == 0`. Từ`1`là số lẻ, điều kiện thất bại ngay lập tức và đầu ra trở thành`NO`. 

Điều này phù hợp với thực tế toán học rằng hai số chẵn không bao giờ có thể tổng thành một số lẻ. 

Bây giờ hãy xem xét:```
4
```Thuật toán thấy rằng`4`chẵn và lớn hơn`2`, vì vậy nó in`YES`. 

Một sự phân chia hợp lệ là`2 + 2`, trong đó cả hai phần đều dương và chẵn.
