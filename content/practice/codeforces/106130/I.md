---
title: "CF 106130I - \u8fd0\u52a8\u4e16\u754c\u6821\u56ed"
description: "Chúng ta được cung cấp một giá trị xác suất duy nhất p, mô tả tỷ lệ thành công của một nỗ lực độc lập để đạt được kết quả cần thiết. Một học sinh có đúng ba lần thử độc lập. Mỗi lần thử thành công với xác suất p hoặc thất bại với xác suất 1 - p."
date: "2026-06-20T08:22:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106130
codeforces_index: "I"
codeforces_contest_name: "GDUT 2025 Monthly competition"
rating: 0
weight: 106130
solve_time_s: 53
verified: true
draft: false
---

[CF 106130I - \u8fd0\u52a8\u4e16\u754c\u6821\u56ed](https://codeforces.com/problemset/problem/106130/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một giá trị xác suất duy nhất`p`, mô tả tỷ lệ thành công của một nỗ lực độc lập để đạt được kết quả cần thiết. Một học sinh có đúng ba lần thử độc lập. Mỗi lần thử đều thành công với xác suất`p`hoặc thất bại với xác suất`1 - p`. Quá trình này sẽ không còn phù hợp khi có ít nhất một thành công xảy ra vì điều đó đã đảm bảo thành công chung. 

Nhiệm vụ là tính xác suất để ít nhất một trong ba lần thử thành công. 

Đầu vào là số dấu phẩy động giữa`0`Và`1`. Đầu ra là một số dấu phẩy động biểu thị xác suất thành công trong vòng ba lần thử, với dung sai là`1e-4`với sai số tương đối hoặc tuyệt đối. 

Mặc dù tuyên bố ngắn gọn nhưng sự lựa chọn mô hình quan trọng là thừa nhận rằng đây là một bài toán kinh điển “ít nhất một lần thành công trong các thử nghiệm Bernoulli độc lập lặp đi lặp lại”. 

Không có ràng buộc lớn nào liên quan, do đó hiệu suất không liên quan, nhưng độ ổn định về số hơi quan trọng vì chúng ta đang xử lý số học dấu phẩy động. Các trường hợp cạnh tinh tế duy nhất xuất hiện ở ranh giới`p = 0`Và`p = 1`. 

Nếu như`p = 0`, mọi nỗ lực đều thất bại nên câu trả lời phải là`0`. Một tính toán ngây thơ như`1 - (1 - p)^3`vẫn hoạt động ở đây, nhưng chỉ khi độ chính xác của dấu phẩy động không tạo ra số 0 âm hoặc nhiễu nhỏ. 

Nếu như`p = 1`, mọi nỗ lực đều thành công nên câu trả lời là chính xác`1`. 

Một lỗi phổ biến là cố gắng liệt kê các chuỗi theo cách thủ công và vô tình tính hai kết quả trong đó xảy ra nhiều thành công. Điều đó dẫn đến lý luận tổ hợp không chính xác trừ khi được cấu trúc cẩn thận. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ liệt kê rõ ràng tất cả các kết quả có thể xảy ra của ba thử nghiệm độc lập. Mỗi thử nghiệm có hai trạng thái, thành công hoặc thất bại, do đó có`2^3 = 8`tổng số mô hình kết quả. Chúng ta có thể tính xác suất của từng mẫu và tính tổng những mẫu có ít nhất một thành công. 

Cách tiếp cận này đúng vì nó trực tiếp tuân theo định nghĩa về xác suất của các sự kiện độc lập. Tuy nhiên, nó dài dòng một cách không cần thiết và có quy mô kém nếu số lượng thử nghiệm tăng lên, bởi vì`n`thử nghiệm sẽ yêu cầu`2^n`sự kết hợp. 

Nhận xét quan trọng là việc đếm “ít nhất một lần thành công” sẽ dễ dàng hơn nhờ phần bù của nó. Thay vì tổng hợp tất cả các trường hợp thuận lợi, chúng tôi tính xác suất mà tất cả các lần thử đều thất bại và trừ đi`1`. 

Xác suất để một lần thử thất bại là`(1 - p)`. Vì các lần thử là độc lập nên xác suất để cả ba lần thử đều thất bại là`(1 - p)^3`. Do đó, xác suất mong muốn là:`1 - (1 - p)^3`Điều này làm giảm vấn đề từ việc liệt kê các kết quả xuống một biểu thức số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(2^3) | O(1) | Hoạt động nhưng không cần thiết | 
| Công thức bổ sung | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc xác suất`p`từ đầu vào. Giá trị này thể hiện xác suất thành công trên mỗi lần thử. 
2. Tính xác suất thất bại`q = 1 - p`. Điều này biến vấn đề thành lý luận về một “sự kiện xấu” thay vì “sự kiện tốt”. 
3. Tính xác suất để cả ba lần thử đều thất bại`q^3`. Tính độc lập cho phép nhân lên qua các lần thử. 
4. Trừ giá trị này khỏi`1`để có được xác suất có ít nhất một lần thử thành công. 
5. In kết quả dưới dạng số dấu phẩy động. 

Tính toán thực tế duy nhất là số lượng phép tính số học không đổi, do đó việc thực hiện rất đơn giản. 

### Tại sao nó hoạt động 

Các sự kiện “ít nhất một thành công” và “tất cả thất bại” bổ sung và loại trừ lẫn nhau, bao trùm toàn bộ không gian xác suất. Vì tính độc lập cho phép tính toán chính xác sự kiện “tất cả đều thất bại” dưới dạng tích của các xác suất giống nhau, phần bù đưa ra mục tiêu chính xác mà không cần xấp xỉ ngoài số học dấu phẩy động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

p = float(input().strip())

q = 1.0 - p
ans = 1.0 - q * q * q

print(ans)
```Mã đọc xác suất dưới dạng số float, tính xác suất lỗi phần bù và nâng nó lên lũy thừa thứ ba bằng cách nhân lặp lại. Điều này tránh mọi chi phí tiềm ẩn khi gọi`pow`và giữ cho tính toán rõ ràng. 

Phép trừ từ`1.0`được thực hiện cuối cùng để giảm thiểu tích lũy lỗi dấu phẩy động. Vì biểu thức chỉ bao gồm một vài thao tác nên độ chính xác nằm trong phạm vi dung sai yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0.5
```Chúng tôi tính toán`q = 0.5`. 

| Bước | Giá trị | 
| --- | --- | 
| p | 0,5 | 
| q | 0,5 | 
| q^3 | 0,125 | 
| 1 - q^3 | 0,875 | 

Đầu ra:```
0.875
```Điều này phù hợp với xác suất có ít nhất một lần thành công xảy ra trong ba lần tung đồng xu độc lập công bằng. 

### Ví dụ 2 

đầu vào:```
0.2
```| Bước | Giá trị | 
| --- | --- | 
| p | 0,2 | 
| q | 0,8 | 
| q^3 | 0,512 | 
| 1 - q^3 | 0,488 | 

Đầu ra:```
0.488
```Điều này khẳng định rằng ngay cả khi tỷ lệ thành công thấp, ba lần thử vẫn tăng đáng kể cơ hội thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép toán số học theo thời gian không đổi mới được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Việc tính toán diễn ra theo thời gian không đổi bất kể kích thước đầu vào, đáp ứng một cách tầm thường mọi ràng buộc hợp lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    p = float(input().strip())
    q = 1.0 - p
    ans = 1.0 - q**3
    return str(ans)

# provided sample
assert abs(float(run("0.5")) - 0.875) < 1e-9

# edge: always fail
assert abs(float(run("0")) - 0.0) < 1e-9

# edge: always succeed
assert abs(float(run("1")) - 1.0) < 1e-9

# small probability
assert abs(float(run("0.1")) - (1 - 0.9**3)) < 1e-9

# high probability
assert abs(float(run("0.9")) - (1 - 0.1**3)) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0,5 | 0,875 | trường hợp tiêu chuẩn | 
| 0 | 0 | mọi thất bại | 
| 1 | 1 | đảm bảo thành công | 
| 0,1 | 0,271 | chế độ xác suất thấp | 
| 0,9 | 0,999 | chế độ xác suất cao | 

## Vỏ cạnh 

cho`p = 0`, chúng tôi nhận được`q = 1`, Vì thế`q^3 = 1`và kết quả là`0`. Thuật toán nắm bắt chính xác rằng mọi nỗ lực đều thất bại, do đó không thể thành công. 

Vì`p = 1`, chúng tôi nhận được`q = 0`, Vì thế`q^3 = 0`và kết quả là`1`. Điều này phản ánh sự thành công được đảm bảo ở lần thử đầu tiên và những lần thử tiếp theo là không liên quan. 

Đối với đầu vào dấu phẩy động cực gần`0`hoặc`1`, phép trừ vẫn ổn định vì biểu thức chỉ bao gồm một phép trừ và hai phép nhân, nằm trong phạm vi dung sai lỗi yêu cầu.
