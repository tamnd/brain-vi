---
title: "CF 195A - Cùng Xem Bóng Đá"
description: "Video kéo dài trong c giây. Việc xem một giây video tiêu tốn một đơn vị dữ liệu, trong khi kết nối internet chỉ tải xuống b đơn vị mỗi giây. Vì a b nên việc bắt đầu ngay lập tức là không thể vì dữ liệu sẽ được tiêu thụ nhanh hơn khi nó đến."
date: "2026-06-05T00:48:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 195
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 123 (Div. 2)"
rating: 1000
weight: 195
solve_time_s: 99
verified: true
draft: false
---

[CF 195A - Cùng xem bóng đá](https://codeforces.com/problemset/problem/195/A) 

**Đánh giá:** 1000 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Video kéo dài trong`c`giây. Xem một giây video tiêu tốn`a`đơn vị dữ liệu, trong khi kết nối internet chỉ tải xuống`b`đơn vị trên giây. Từ`a > b`, việc bắt đầu ngay lập tức là không thể vì dữ liệu sẽ được tiêu thụ nhanh hơn khi dữ liệu đến. 

Giả sử người xem chờ đợi`t`giây trước khi nhấn play. Lúc đó họ đã tải xuống rồi`b · t`đơn vị dữ liệu. Sau khi bắt đầu phát lại, quá trình tải xuống tiếp tục với tốc độ như cũ. Mục đích là tìm số nguyên nhỏ nhất`t`sao cho tại mọi thời điểm trong khi phát lại, lượng dữ liệu được tải xuống ít nhất cũng bằng lượng dữ liệu đã được sử dụng khi xem. 

Những hạn chế là rất nhỏ. Cả ba giá trị đều có nhiều nhất là 1000. Ngay cả một thuật toán kiểm tra nhiều thời gian chờ đợi của ứng viên cũng sẽ chạy thoải mái trong giới hạn. Thử thách không phải là hiệu suất mà là thể hiện điều kiện một cách chính xác. 

Một lỗi phổ biến là chỉ kiểm tra xem toàn bộ video có thể được tải xuống khi kết thúc phát lại hay không. Coi như:```
4 1 1
```Nếu chúng tôi đợi 2 giây thì khi kết thúc phát lại, chúng tôi đã tải xuống`1·(2+1)=3`đơn vị, vẫn chưa đủ. Đợi 3 giây sẽ có chính xác 4 đơn vị. Câu trả lời là 3. 

Một trường hợp tinh tế khác là khi nút cổ chai xảy ra ở cuối chứ không phải gần đầu. Ví dụ:```
2 1 10
```Chờ 4 giây có vẻ gần vì quá trình phát lại có thể bắt đầu suôn sẻ. Tuy nhiên, đến cuối giây xem thứ hai, chỉ có 18 đơn vị được tải xuống, trong khi yêu cầu phải có 20 đơn vị. Câu trả lời đúng là 5. 

Một mô phỏng bất cẩn chỉ kiểm tra thời điểm bắt đầu phát lại sẽ chấp nhận những trường hợp như vậy một cách không chính xác. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực trực tiếp là thử thời gian chờ đợi`t = 0, 1, 2, ...`và kiểm tra xem có thể phát lại hay không. 

Đối với một cố định`t`, cho phép`x`là số giây đã xem. Tại thời điểm đó, dữ liệu được tải xuống bằng```
b · (t + x)
```và dữ liệu tiêu thụ bằng```
a · x
```Phát lại hợp lệ nếu```
b · (t + x) ≥ a · x
```cho mọi`x`từ`0`ĐẾN`c`. 

Kiểm tra tất cả`c + 1`khoảnh khắc của mỗi ứng viên`t`hoạt động vì giới hạn rất nhỏ. Ngay cả việc thử 1000 lần chờ đợi và kiểm tra 1000 khoảnh khắc mỗi lần cũng chỉ là khoảng một triệu thao tác. 

Quan sát quan trọng là sự bất bình đẳng có thể được đơn giản hóa:```
b·t + b·x ≥ a·x
b·t ≥ (a-b)·x
```Từ`a > b`, vế phải tăng khi`x`tăng lên. Thời điểm khó thỏa mãn nhất là giá trị lớn nhất có thể có của`x`, cụ thể là`x = c`. 

Vì vậy, thay vì kiểm tra từng khoảnh khắc, chỉ cần yêu cầu```
b·t ≥ (a-b)·c
```Đáp án là số nguyên nhỏ nhất`t`thỏa mãn bất đẳng thức này:```
t = ceil((a-b)·c / b)
```Điều này làm giảm toàn bộ vấn đề thành một phép tính số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(c · câu trả lời) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`a`,`b`, Và`c`. 
2. Viết lại điều kiện phát lại tại thời điểm đã xem`x`:```
b·(t+x) ≥ a·x
```3. Sắp xếp lại:```
b·t ≥ (a-b)·x
```Phía bên trái không đổi, trong khi phía bên phải tăng trưởng với`x`. 
4. Vì`a > b`, giá trị lớn nhất của vế phải xảy ra tại`x = c`. 
5. Yêu cầu:```
b·t ≥ (a-b)·c
```6. Tính số nguyên nhỏ nhất thỏa mãn bất đẳng thức:```
t = ceil((a-b)·c / b)
```7. Đầu ra`t`. 

### Tại sao nó hoạt động 

Đối với mọi thời lượng đã xem`x`, việc phát lại có thể thực hiện được chính xác khi```
b·t ≥ (a-b)·x.
```Bởi vì`a-b`là dương, vế phải tăng đơn điệu với`x`. Nếu bất đẳng thức giữ giá trị lớn nhất có thể`x = c`, nó tự động giữ cho mọi giá trị nhỏ hơn của`x`. Như vậy việc kiểm tra phần cuối video là đủ và cần thiết. Thời gian chờ số nguyên nhỏ nhất thỏa mãn bất đẳng thức cuối cùng đó chính xác là```
ceil((a-b)·c / b).
```## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c = map(int, input().split())

need = (a - b) * c
answer = (need + b - 1) // b

print(answer)
```Biến`need`lưu trữ tổng mức thâm hụt phải được bù đắp trước khi bắt đầu phát lại. Trong mỗi giây xem, mức tiêu thụ vượt quá tải xuống một cách chính xác`a - b`đơn vị. Qua`c`giây, mức thâm hụt đó tích lũy thành`(a - b) · c`. 

Chờ`t`giây tải xuống`b · t`đơn vị trước đó. Chúng tôi cần:```
b·t ≥ need
```biểu hiện```
(need + b - 1) // b
```tính toán trần của`need / b`sử dụng số học số nguyên. Điều này tránh các phép tính dấu phẩy động và loại bỏ mọi lo ngại về làm tròn. 

Các ràng buộc rất nhỏ, nhưng sử dụng phép toán số nguyên vừa sạch hơn vừa chính xác hơn về mặt toán học. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 1 1
```Tính các giá trị cần thiết. 

| Biến | Giá trị | 
| --- | --- | 
| một | 4 | 
| b | 1 | 
| c | 1 | 
| cần = (a-b)·c | 3 | 
| câu trả lời = trần(3/1) | 3 | 

Đầu ra:```
3
```Người xem phải tích lũy thêm 3 đơn vị trước khi bắt đầu. Trong một giây phát lại, một đơn vị khác đến, mang lại tổng số 4 đơn vị cần thiết. 

### Mẫu 2 

đầu vào:```
2 1 10
```| Biến | Giá trị | 
| --- | --- | 
| một | 2 | 
| b | 1 | 
| c | 10 | 
| cần = (a-b)·c | 10 | 
| câu trả lời = trần (10/1) | 10 | 

Đầu ra:```
10
```Ở đây, kết nối bị tụt lại phía sau 1 đơn vị mỗi giây phát lại. Trong 10 giây, mức thâm hụt là 10 đơn vị, do đó, chính xác 10 đơn vị phải được đệm trước khi bắt đầu phát lại. 

Dấu vết này nêu bật tính bất biến trung tâm: toàn bộ vấn đề quy về việc bù đắp thâm hụt tích lũy`(a-b)·c`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một số phép tính số học không đổi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giải pháp không thực hiện vòng lặp và không có bộ lưu trữ phụ. Nó dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    a, b, c = map(int, input().split())
    need = (a - b) * c
    return str((need + b - 1) // b)

# provided sample
assert run("4 1 1\n") == "3", "sample 1"

# minimum-size values
assert run("2 1 1\n") == "1", "minimum case"

# exact divisibility
assert run("5 2 4\n") == "6", "exact division"

# ceiling required
assert run("5 3 2\n") == "2", "ceiling division"

# maximum values
assert run("1000 1 1000\n") == "999000", "maximum bounds"

# off-by-one boundary
assert run("3 2 1\n") == "1", "single unit deficit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`|`1`| Ví dụ nhỏ nhất có ý nghĩa | 
|`5 2 4`|`6`| Trường hợp chia chính xác | 
|`5 3 2`|`2`| Phân chia trần đúng cách | 
|`1000 1 1000`|`999000`| Giá trị lớn nhất | 
|`3 2 1`|`1`| Ranh giới chung của từng người | 

## Vỏ cạnh 

Hãy xem xét:```
3 2 1
```Mức thâm hụt tích lũy trong quá trình phát lại là:```
(3-2)·1 = 1
```Thuật toán tính toán:```
ceil(1/2) = 1
```Chờ 0 giây sẽ thất bại ngay lập tức vì quá trình tiêu thụ bắt đầu nhanh hơn quá trình tải xuống. Đợi 1 giây là đủ bộ đệm. 

Bây giờ hãy xem xét:```
5 2 4
```Thâm hụt là:```
(5-2)·4 = 12
```Từ```
12 / 2 = 6
```đã là một số nguyên, câu trả lời chính xác là 6. Trường hợp này xác minh rằng công thức trần không vô tình cộng thêm một giây khi phép chia chính xác. 

Cuối cùng, hãy xem xét các giá trị lớn nhất có thể:```
1000 1 1000
```Thâm hụt trở thành:```
(1000-1)·1000 = 999000
```và câu trả lời là```
999000
```Tất cả số học đều nằm gọn trong các loại số nguyên tiêu chuẩn và công thức vẫn hoạt động mà không cần bất kỳ thao tác xử lý đặc biệt nào.
