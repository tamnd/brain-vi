---
title: "CF 1162A - Lại hạn chế phân vùng"
description: "Chúng ta có một con phố với n vị trí tòa nhà. Mỗi vị trí có thể chứa một ngôi nhà có chiều cao là số nguyên từ 0 đến h. Lợi nhuận từ một ngôi nhà có chiều cao a là a2, vì vậy những ngôi nhà cao hơn bao giờ cũng tốt hơn. Thành phố áp đặt quy tắc phân vùng."
date: "2026-06-12T02:23:56+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1162
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 557 (Div. 2) [based on Forethought Future Cup - Final Round]"
rating: 800
weight: 1162
solve_time_s: 79
verified: true
draft: false
---

[CF 1162A - Lại hạn chế phân vùng](https://codeforces.com/problemset/problem/1162/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một con phố với`n`xây dựng các vị trí. Mỗi vị trí có thể chứa một ngôi nhà có chiều cao là số nguyên giữa`0`Và`h`. 

Lợi nhuận từ nhà trên cao`a`là`a²`, vì vậy nhà cao hơn luôn tốt hơn. Thành phố áp đặt`m`quy định phân vùng. Mỗi quy tắc bao gồm một phân đoạn vị trí`[l, r]`và nói rằng ngôi nhà cao nhất trong phân khúc đó không thể vượt quá một giá trị nào đó`x`. 

Nhiệm vụ là chọn độ cao cho mọi vị trí sao cho thỏa mãn mọi hạn chế và tổng lợi nhuận, tổng bình phương của tất cả các độ cao, càng lớn càng tốt. 

Những hạn chế là rất nhỏ. Cả hai`n`Và`m`nhiều nhất là`50`. Ngay cả một thuật toán thực hiện công việc tỷ lệ thuận với mọi thời gian hạn chế, mọi vị trí đều hoàn toàn an toàn. MỘT`O(nm)`hoặc`O(n²m)`giải pháp chạy ngay lập tức. 

Thách thức chính là hiểu được các hạn chế về phạm vi ảnh hưởng đến từng ngôi nhà như thế nào. Một hạn chế nói rằng chiều cao tối đa trong một phạm vi không thể vượt quá`x`. Điều đó có nghĩa là mọi ngôi nhà trong phạm vi đó cũng phải có nhiều nhất`x`. Nếu thậm chí một ngôi nhà cao hơn`x`, mức tối đa của phạm vi sẽ vượt quá`x`và hạn chế sẽ bị vi phạm. 

Một sai lầm phổ biến là coi một hạn chế chỉ áp dụng cho một vị trí. 

Coi như:```
3 10 1
1 3 5
```Mọi ngôi nhà đều nằm trong phạm vi giới hạn nên mọi chiều cao tối đa phải`5`. 

Độ cao tối ưu là:```
[5, 5, 5]
```Lợi nhuận:```
25 + 25 + 25 = 75
```Một sự giải thích bất cẩn có thể cho phép những độ cao như`[10, 10, 10]`, rõ ràng vi phạm hạn chế. 

Một sai lầm dễ mắc phải khác là quên rằng nhiều hạn chế có thể chồng chéo lên nhau. 

Ví dụ:```
3 10 2
1 2 7
2 3 4
```Chức vụ`2`thuộc cả hai dãy. Chiều cao của nó phải thỏa mãn cả hai giới hạn nên giá trị cực đại hiệu dụng của nó là:```
min(7, 4) = 4
```Độ cao tối ưu trở thành:```
[7, 4, 4]
```Bỏ qua một trong những hạn chế sẽ tạo ra một câu trả lời không hợp lệ. 

Một điểm tinh tế cuối cùng là một số vị trí có thể không bao giờ xuất hiện trong bất kỳ hạn chế nào. 

Ví dụ:```
3 10 1
2 2 3
```Chức vụ`1`và vị trí`3`không bị hạn chế ngoại trừ giới hạn toàn cầu`h = 10`, nên chúng vẫn ở độ cao`10`. 

Độ cao tối ưu là:```
[10, 3, 10]
```Lợi nhuận:```
100 + 9 + 100 = 209
```## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi cách gán độ cao có thể và giữ giá trị hợp lệ tốt nhất. Mỗi vị trí có tới`h + 1`sự lựa chọn, vì vậy có`(h + 1)^n`các cấu hình có thể. 

Với`n = 50`Và`h = 50`, đại khái là:```
51^50
```đó là điều lớn lao về mặt thiên văn và hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mọi hạn chế đều chuyển trực tiếp thành giới hạn trên của các vị thế riêng lẻ. 

Giả sử một hạn chế nói:```
l = 2, r = 5, x = 7
```Vì chiều cao tối đa trong khoảng đó không được vượt quá`7`, mọi vị trí từ`2`bởi vì`5`tối đa phải có riêng lẻ`7`. 

Điều này có nghĩa là chúng ta có thể nghĩ về từng vị trí một cách độc lập. Bắt đầu bằng cách giả sử mọi vị trí đều có thể đạt tới độ cao`h`. Sau đó xử lý từng hạn chế và giảm chiều cao cho phép của mọi vị trí được che phủ xuống:```
current_limit = min(current_limit, x)
```Sau khi tất cả các hạn chế được xử lý, mỗi vị trí có chiều cao tối đa hiệu dụng. 

Bây giờ hãy xem xét hàm mục tiêu. Lợi nhuận đóng góp của một vị thế là:```
a²
```tăng nghiêm ngặt khi`a`tăng đối với độ cao không âm. 

Vì các vị trí không còn tương tác nữa nên lựa chọn tốt nhất cho mỗi vị trí chỉ đơn giản là chiều cao lớn nhất cho phép của vị trí đó. 

Toàn bộ vấn đề giảm xuống việc tính toán chiều cao tối đa cuối cùng được phép ở mọi vị trí và tính tổng các bình phương của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((h+1)^n) | O(n) | Quá chậm | 
| Tối ưu | O(nm) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`height`chiều dài`n`và khởi tạo mọi mục nhập vào`h`. 
2. Xử lý từng hạn chế`(l, r, x)`. 
3. Đối với mọi vị trí trong khoảng`[l, r]`, thay thế giới hạn hiện tại của nó bằng:```
min(current_limit, x)
```Điều này tích lũy tất cả các hạn chế ảnh hưởng đến vị trí đó. 
4. Sau khi tất cả các hạn chế đã được xử lý, mỗi mục nhập của`height`lưu trữ chiều cao pháp lý lớn nhất cho vị trí đó. 
5. Tính toán:```
answer = Σ height[i]²
```6. Xuất kết quả tổng. 

### Tại sao nó hoạt động 

Một hạn chế về phạm vi nói rằng không có ngôi nhà nào trong phạm vi đó có thể vượt quá`x`. Áp dụng`min(current_limit, x)`đối với mọi vị trí được bảo hiểm đều ghi lại chính xác yêu cầu này. 

Sau khi tất cả các hạn chế được xử lý, giá trị được lưu trữ cho một vị trí là giới hạn trên nhỏ nhất được áp đặt lên nó bởi bất kỳ hạn chế nào bao trùm vị trí đó, hoặc`h`nếu không có che nó. 

Hàm lợi nhuận tăng theo chiều cao vì tất cả các độ cao đều không âm và lợi nhuận bằng nhau`a²`. Nâng cao một ngôi nhà không bao giờ làm giảm lợi nhuận. Do đó, đối với mỗi vị trí, sự lựa chọn tối ưu là chiều cao pháp lý tối đa của nó. Việc chọn bất cứ thứ gì nhỏ hơn chỉ làm giảm lợi nhuận và không thể giúp đáp ứng bất kỳ ràng buộc bổ sung nào vì tất cả các ràng buộc đã được mã hóa thành giới hạn cho mỗi vị trí. 

Do đó, thuật toán xây dựng cấu hình tối đa hóa lợi nhuận duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, h, m = map(int, input().split())

height = [h] * n

for _ in range(m):
    l, r, x = map(int, input().split())
    for i in range(l - 1, r):
        height[i] = min(height[i], x)

answer = sum(v * v for v in height)
print(answer)
```Mảng`height`lưu trữ chiều cao tối đa cho phép hiện tại cho mọi vị trí. 

Ban đầu mọi vị trí đều có thể đạt tới độ cao`h`, vì vậy mảng chứa đầy`h`. 

Mỗi hạn chế được xử lý ngay lập tức. Bởi vì đầu vào sử dụng lập chỉ mục dựa trên một trong khi danh sách Python sử dụng lập chỉ mục dựa trên 0, nên khoảng`[l, r]`trở thành:```
range(l - 1, r)
```Đối với mọi vị thế được bảo hiểm, chúng tôi duy trì giới hạn tối thiểu cho đến nay. 

Khi tất cả các hạn chế đã được áp dụng, mọi vị trí đều có chiều cao tối ưu. Vòng lặp cuối cùng tính tổng bình phương. 

Không cần xử lý đặc biệt đối với các khoảng thời gian chồng chéo vì lặp đi lặp lại`min`hoạt động tự nhiên kết hợp tất cả các hạn chế. Tràn số nguyên cũng không liên quan vì câu trả lời tối đa có thể là:```
50 × 50² = 125000
```dễ dàng phù hợp với số nguyên Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3 3
1 1 1
2 2 3
3 3 2
```Trạng thái ban đầu: 

| Bước | Mảng chiều cao | 
| --- | --- | 
| Bắt đầu | [3, 3, 3] | 

Sau mỗi hạn chế: 

| Hạn chế | Mảng chiều cao | 
| --- | --- | 
| (1,1,1) | [1, 3, 3] | 
| (2,2,3) | [1, 3, 3] | 
| (3,3,2) | [1, 3, 2] | 

Tính toán lợi nhuận: 

| Vị trí | Chiều cao | Đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 9 | 
| 3 | 2 | 4 | 

Tổng cộng:```
1 + 9 + 4 = 14
```Ví dụ này cho thấy mỗi hạn chế trực tiếp hạ thấp chiều cao cho phép của các vị trí mà nó bao phủ. 

### Mẫu 2 

đầu vào:```
4 10 2
2 3 8
3 4 7
```Trạng thái ban đầu: 

| Bước | Mảng chiều cao | 
| --- | --- | 
| Bắt đầu | [10, 10, 10, 10] | 

Sau mỗi hạn chế: 

| Hạn chế | Mảng chiều cao | 
| --- | --- | 
| (2,3,8) | [10, 8, 8, 10] | 
| (3,4,7) | [10, 8, 7, 7] | 

Tính toán lợi nhuận: 

| Vị trí | Chiều cao | Đóng góp | 
| --- | --- | --- | 
| 1 | 10 | 100 | 
| 2 | 8 | 64 | 
| 3 | 7 | 49 | 
| 4 | 7 | 49 | 

Tổng cộng:```
100 + 64 + 49 + 49 = 262
```Dấu vết này cho thấy các hạn chế chồng chéo kết hợp như thế nào thông qua hoạt động tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi hạn chế có thể cập nhật tối đa n vị trí | 
| Không gian | O(n) | Lưu trữ giới hạn chiều cao cho mọi vị trí | 

Với`n ≤ 50`Và`m ≤ 50`, công việc trong trường hợp xấu nhất chỉ là về`2500`cập nhật. Điều này thấp hơn nhiều so với giới hạn và về cơ bản chạy ngay lập tức. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, h, m = map(int, input().split())

    height = [h] * n

    for _ in range(m):
        l, r, x = map(int, input().split())
        for i in range(l - 1, r):
            height[i] = min(height[i], x)

    return str(sum(v * v for v in height))

# provided samples
assert run(
"""3 3 3
1 1 1
2 2 3
3 3 2
"""
) == "14", "sample 1"

assert run(
"""4 10 2
2 3 8
3 4 7
"""
) == "262", "sample 2"

# minimum size
assert run(
"""1 5 0
"""
) == "25", "single position"

# full-range restriction
assert run(
"""3 10 1
1 3 5
"""
) == "75", "all positions limited"

# overlapping restrictions
assert run(
"""3 10 2
1 2 7
2 3 4
"""
) == "81", "minimum of overlapping limits"

# unrestricted positions remain at h
assert run(
"""3 10 1
2 2 3
"""
) == "209", "positions outside intervals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 0`|`25`| Ví dụ kích thước tối thiểu | 
| Hạn chế toàn phạm vi |`75`| Giới hạn phạm vi áp dụng cho mọi vị trí | 
| Hạn chế chồng chéo |`81`| Tối thiểu tất cả các giới hạn áp dụng được sử dụng | 
| Vị trí hạn chế duy nhất |`209`| Các vị trí chưa được che chắn vẫn ở độ cao`h`| 

## Vỏ cạnh 

### Hạn chế chồng chéo 

đầu vào:```
3 10 2
1 2 7
2 3 4
```Quá trình xử lý mang lại:```
[10,10,10]
→ [7,7,10]
→ [7,4,4]
```Chức vụ`2`nhận cả hai hạn chế và giữ giá trị nhỏ hơn`4`. Lợi nhuận cuối cùng là:```
49 + 16 + 16 = 81
```Thuật toán xử lý việc này một cách tự động thông qua việc lặp lại`min`hoạt động. 

###Hạn chế bao trùm toàn bộ đường phố 

đầu vào:```
3 10 1
1 3 5
```Quá trình xử lý mang lại:```
[10,10,10]
→ [5,5,5]
```Lợi nhuận:```
25 + 25 + 25 = 75
```Mọi vị trí đều thuộc về khoảng thời gian giới hạn, vì vậy mọi vị trí đều nhận được giới hạn như nhau. 

### Vị trí không giới hạn 

đầu vào:```
3 10 1
2 2 3
```Quá trình xử lý mang lại:```
[10,10,10]
→ [10,3,10]
```Vị trí`1`Và`3`không bao giờ xuất hiện trong bất kỳ khoảng thời gian nào, vì vậy chúng giữ nguyên giới hạn ban đầu`h = 10`. 

Lợi nhuận:```
100 + 9 + 100 = 209
```Việc khởi tạo với`h`đảm bảo rằng các vị trí không bị ảnh hưởng bởi các hạn chế sẽ tự động nhận được chiều cao pháp lý lớn nhất có thể.
