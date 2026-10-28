---
title: "CF 106169A - Borg Cube"
description: "Một khối lập phương có sáu mặt: trên, dưới, trái, phải, trước, sau. Mỗi góc của khối lập phương chạm đúng ba mặt. Giá trị ẩn ở một góc được xác định là tích của ba mệnh giá gặp nhau ở góc đó. Tổng cộng có tám góc."
date: "2026-06-25T11:07:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106169
codeforces_index: "A"
codeforces_contest_name: "2025-2026 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 106169
solve_time_s: 40
verified: true
draft: false
---

[CF 106169A - Borg Cube](https://codeforces.com/problemset/problem/106169/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một khối lập phương có sáu mệnh giá:`top, bottom, left, right, front, back`. 

Mỗi góc của khối lập phương chạm đúng ba mặt. Giá trị ẩn ở một góc được xác định là tích của ba mệnh giá gặp nhau ở góc đó. 

Tổng cộng có tám góc. Mã bắt buộc là tổng giá trị của tất cả tám góc. 

Đầu vào cho sáu mệnh giá. Chúng ta phải tính tổng của tám tích góc. 

Những hạn chế là cực kỳ nhỏ. Mỗi mệnh giá nhiều nhất là 500, do đó, ngay cả việc liệt kê trực tiếp tất cả các góc cũng không đáng kể. Giá trị góc lớn nhất có thể là:`500 × 500 × 500 = 125,000,000`và tổng của tám giá trị như vậy là`1,000,000,000`, phù hợp với giới hạn đầu ra đã nêu. Số học số nguyên tiêu chuẩn là đủ. 

Phần tinh tế duy nhất là xác định chính xác tất cả tám góc. Mỗi góc phải chứa chính xác một mặt của mỗi cặp đối diện: 

Trên hoặc Dưới. 

Trái hoặc Phải. 

Mặt trước hoặc Mặt sau. 

Một lỗi phổ biến là vô tình bỏ lỡ một số quả phạt góc hoặc đếm một lần hai lần. 

Coi như:```
1 1 1 1 1 1
```Mọi giá trị góc đều bằng 1, vì vậy câu trả lời là:```
8
```Nếu chỉ tính bảy góc thì kết quả là 7. 

Một sai lầm dễ mắc phải khác là trộn lẫn các khuôn mặt đối diện. Ví dụ:```
1 2 3 4 5 6
```Một góc là`top × left × front = 1 × 3 × 5 = 15`. 

Một sản phẩm như`top × bottom × front`không hợp lệ vì trên và dưới không bao giờ gặp nhau ở cùng một góc. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là tính toán rõ ràng tất cả tám tích góc và cộng chúng lại với nhau. 

Các góc là:`(top,left,front)`

`(top,left,back)`

`(top,right,front)`

`(top,right,back)`

`(bottom,left,front)`

`(bottom,left,back)`

`(bottom,right,front)`

`(bottom,right,back)`Điều này chỉ yêu cầu tám phép nhân của ba số và một vài phép cộng, thực tế là thời gian không đổi. 

Ngoài ra còn có một quan sát đại số hữu ích. Mỗi góc chọn một mặt từ mỗi cặp đối diện. Mở rộng```
(top + bottom)
× (left + right)
× (front + back)
```sản xuất chính xác tám sản phẩm góc, mỗi sản phẩm xuất hiện một lần. 

Ví dụ như việc chọn`top`từ dấu ngoặc đơn đầu tiên,`right`từ thứ hai và`back`từ phần thứ ba đóng góp sản phẩm góc:```
top × right × back
```Vì tất cả tám kết hợp xuất hiện chính xác một lần nên tổng mong muốn chỉ đơn giản là:```
(top + bottom)
× (left + right)
× (front + back)
```Điều này biến vấn đề thành một công thức duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Sản phẩm của Tổng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc sáu giá trị mặt: trên, dưới, trái, phải, trước và sau. 
2. Tính tổng các mặt thẳng đứng đối diện:```
top + bottom
```3. Tính tổng các mặt ngang đối diện:```
left + right
```4. Tính tổng các mặt trước sau đối diện:```
front + back
```5. Nhân ba số tiền này với nhau. 

Việc mở rộng tích sẽ tạo ra mọi kết hợp góc hợp lệ đúng một lần. 
6. Xuất kết quả. 

### Tại sao nó hoạt động 

Mỗi góc được hình thành bằng cách chọn một mặt từ mỗi cặp mặt đối diện. Ba cặp đó là:```
(top, bottom)
(left, right)
(front, back)
```biểu hiện```
(top + bottom)
× (left + right)
× (front + back)
```chứa một số hạng cho mọi lựa chọn có thể có từ ba cặp này. Vì có`2 × 2 × 2 = 8`các lựa chọn, bản mở rộng chứa chính xác tám tích ở góc và không có gì khác. Tổng của chúng chính xác là mã được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

top, bottom, left, right, front, back = map(int, input().split())

answer = (top + bottom) * (left + right) * (front + back)

print(answer)
```Việc thực hiện theo công thức trực tiếp. 

Sáu giá trị được đọc theo thứ tự được chỉ định bởi câu lệnh. Biểu thức tính tổng của tất cả các tích góc mà không liệt kê rõ ràng các góc. 

Vì câu trả lời tối đa là nhiều nhất`10^9`, Kiểu số nguyên Python dễ dàng xử lý nó. Không có vấn đề lập chỉ mục, vòng lặp hoặc điều kiện biên nào phải lo lắng. 
## Ví dụ đã hoạt động 
### Ví dụ 1 
đầu vào:```
1 2 3 4 4 5
```| Biến | Giá trị | 
| --- | --- | 
| trên + dưới | 3 | 
| trái + phải | 7 | 
| trước + sau | 9 | 
| trả lời | 3×7×9 = 189 | 

Đầu ra:```
189
```Ví dụ này thể hiện ý tưởng mở rộng. Giá trị 189 bằng tổng của tất cả tám tích góc được tính toán rõ ràng. 

### Ví dụ 2 

đầu vào:```
1 1 1 1 1 1
```| Biến | Giá trị | 
| --- | --- | 
| trên + dưới | 2 | 
| trái + phải | 2 | 
| trước + sau | 2 | 
| trả lời | 2 × 2 × 2 = 8 | 

Đầu ra:```
8
```Mỗi tích góc bằng 1 và có 8 góc nên đáp án là 8. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một số phép tính số học cố định | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Thuật toán thực hiện công việc liên tục bất kể giá trị đầu vào. Nó thoải mái phù hợp với mọi giới hạn cuộc thi hợp lý. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    top, bottom, left, right, front, back = map(int, input().split())
    return str((top + bottom) * (left + right) * (front + back))

# provided sample
assert run("1 2 3 4 4 5\n") == "189", "sample 1"

# minimum values
assert run("1 1 1 1 1 1\n") == "8", "all ones"

# asymmetric values
assert run("1 2 3 4 5 6\n") == "231", "general case"

# all maximum values
assert run("500 500 500 500 500 500\n") == "1000000000", "upper bound"

# one large pair, others small
assert run("500 1 1 1 1 1\n") == "2004", "boundary arithmetic"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1 1`|`8`| Mọi góc đều đóng góp như nhau | 
|`1 2 3 4 5 6`|`231`| Trường hợp không đối xứng tổng quát | 
|`500 500 500 500 500 500`|`1000000000`| Câu trả lời tối đa có thể | 
|`500 1 1 1 1 1`|`2004`| Xử lý đúng các giá trị không đồng đều | 

## Vỏ cạnh 

Hãy xem xét:```
1 1 1 1 1 1
```Thuật toán tính toán:```
(1 + 1)(1 + 1)(1 + 1)
= 2 × 2 × 2
= 8
```Tất cả tám quả phạt góc đều đóng góp chính xác là 1. Kết quả xác nhận rằng mỗi quả phạt góc được tính một lần. 

Coi như:```
1 2 3 4 5 6
```Thuật toán tính toán:```
(1 + 2)(3 + 4)(5 + 6)
= 3 × 7 × 11
= 231
```Khai triển tích sẽ thu được tám tích góc hợp lệ:```
1·3·5
1·3·6
1·4·5
1·4·6
2·3·5
2·3·6
2·4·5
2·4·6
```và tổng của chúng là 231. Không có sự kết hợp không hợp lệ như`top × bottom × front`xuất hiện trong phần mở rộng. 

Cuối cùng, đối với các giá trị được phép lớn nhất:```
500 500 500 500 500 500
```thuật toán tính toán:```
1000 × 1000 × 1000
= 1,000,000,000
```phù hợp với câu trả lời tối đa có thể và vẫn an toàn trong giới hạn số nguyên.
