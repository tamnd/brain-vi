---
title: "CF 106043A - Hình vuông"
description: "Chúng ta được cấp tối đa $2 cdot 10^5$ hình vuông thẳng hàng trên mặt phẳng. Đối với mỗi hình vuông, đầu vào cung cấp các góc dưới bên trái và trên bên phải. Mọi tọa độ đều nằm trong phạm vi $[-10^9, 10^9]$."
date: "2026-06-25T12:48:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106043
codeforces_index: "A"
codeforces_contest_name: "Teamscode Summer 2025 Advanced Division"
rating: 0
weight: 106043
solve_time_s: 49
verified: true
draft: false
---

[CF 106043A - Hình vuông](https://codeforces.com/problemset/problem/106043/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được từ bỏ$2 \cdot 10^5$các hình vuông thẳng hàng với trục trên mặt phẳng. Đối với mỗi hình vuông, đầu vào cung cấp các góc dưới bên trái và trên bên phải. Mọi tọa độ đều nằm trong phạm vi$[-10^9, 10^9]$. Chúng ta phải xuất ra bất kỳ hình vuông thẳng hàng với trục nào$S$sao cho, với mỗi ô vuông cho trước$s_i$, ngã tư$S \cap s_i$bản thân nó là một hình vuông có diện tích dương. 

Phản ứng đầu tiên là nghĩ về hình học của giao điểm giữa các hình vuông. Người ta có thể cố gắng xây dựng một hình vuông được định vị cẩn thận mà phần chồng lên mọi hình vuông đầu vào vẫn là một hình vuông. Với$n$lớn như$2 \cdot 10^5$, bất kỳ giải pháp nào liên quan đến lý luận hình học theo cặp sẽ cần phải cực kỳ hiệu quả. 

Quan sát chính đơn giản hơn nhiều. 

Tất cả các ô vuông đầu vào được đảm bảo nằm hoàn toàn bên trong hộp tọa độ$$[-10^9, 10^9] \times [-10^9, 10^9].$$Nếu chúng ta xuất hình vuông$$S = [-10^9, -10^9] \to [10^9, 10^9],$$thì mọi ô vuông đầu vào được chứa hoàn toàn bên trong$S$. 

Kết quả là,$$S \cap s_i = s_i$$cho mỗi ô vuông đầu vào$s_i$. 

Vì mỗi$s_i$đã là hình vuông có diện tích dương, mọi giao lộ tự động là hình vuông có diện tích dương. Bản thân hình vuông đầu ra cũng thỏa mãn mọi giới hạn tọa độ vì các góc của nó nằm chính xác trên ranh giới cho phép. 

### Trường hợp cạnh không rõ ràng 

Một lỗi phổ biến là cố gắng xây dựng một hình vuông dựa trên đầu vào và vô tình làm cho nó quá nhỏ. 

Ví dụ:```
1
1 1 2 2
```Nếu hình vuông đã chọn không chồng lên hình vuông đầu vào thì giao điểm có diện tích bằng 0 và trở nên không hợp lệ. 

Hình vuông cố định```
-1000000000 -1000000000 1000000000 1000000000
```chứa toàn bộ hình vuông đầu vào, do đó giao điểm chính xác là giao điểm ban đầu$1 \times 1$quảng trường. 

Một sai lầm dễ mắc phải khác là quên rằng tọa độ có thể đã nằm trên đường biên:```
1
-1000000000 -1000000000 1000000000 1000000000
```Hình vuông đầu ra của chúng tôi giống hệt với hình vuông đầu vào. Giao lộ vẫn là một hình vuông có diện tích dương. 

## Phương pháp tiếp cận 

Một tư duy mạnh mẽ sẽ là kiểm tra hình học của tất cả các hình vuông nhất định và xây dựng một hình vuông có phần chồng lên nhau vẫn là hình vuông. Người ta có thể phân tích các khoảng chồng lấp trên cả hai trục và cố gắng thỏa mãn một tập hợp lớn các ràng buộc hình học. 

Vấn đề trở nên dễ dàng hơn nhiều sau khi nhận thấy rằng câu lệnh chỉ yêu cầu **bất kỳ** ô vuông hợp lệ nào. 

Vì mọi tọa độ của mọi ô vuông đầu vào đều nằm trong$[-10^9, 10^9]$, hình vuông thẳng hàng theo trục lớn nhất được phép$$[-10^9,-10^9] \to [10^9,10^9]$$đã chứa mọi ô vuông đầu vào có thể. Khi một hình chứa hoàn toàn một hình khác, giao điểm của chúng chỉ đơn giản là hình nhỏ hơn. 

Điều đó có nghĩa là mọi giao điểm đều bằng hình vuông đầu vào ban đầu, được đảm bảo là hình vuông có diện tích khác 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Phụ thuộc vào cấu trúc hình học | Phụ thuộc vào việc thực hiện | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào. Các mô tả hình vuông thực tế không ảnh hưởng đến câu trả lời. 
2. Xuất hình vuông cố định:$$(-10^9,-10^9)$$như góc dưới bên trái và$$(10^9,10^9)$$như góc trên bên phải. 
3. Kết thúc. 

### Tại sao nó hoạt động 

Mỗi ô vuông đầu vào có tất cả tọa độ của nó trong phạm vi$[-10^9,10^9]$. Hình vuông cố định trải dài chính xác toàn bộ phạm vi tọa độ. Do đó, mọi ô vuông đầu vào đều được chứa đầy bên trong nó. 

Đối với bất kỳ hình vuông chứa$s_i$,$$S \cap s_i = s_i.$$Bởi vì$s_i$được đảm bảo là hình vuông có diện tích dương thì giao điểm thỏa mãn điều kiện yêu cầu. Vì đối số này đúng cho mọi ô vuông đầu vào nên việc xây dựng luôn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

for _ in range(n):
    input()

print(-1000000000, -1000000000, 1000000000, 1000000000)
```Việc triển khai đọc và loại bỏ các ô vuông đầu vào vì vị trí chính xác của chúng không liên quan. 

Thực tế quan trọng duy nhất là giới hạn tọa độ được đưa ra trong câu lệnh. Hình vuông được in chiếm toàn bộ phạm vi tọa độ cho phép. Vì mọi hình vuông đầu vào phải nằm trong phạm vi đó nên mỗi giao điểm bằng hình vuông ban đầu. 

Không có mối lo ngại về tràn trong Python và không có phép tính hình học nào có thể gây ra lỗi biên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3 4 5
7 3 10 6
8 1 12 5
```Đầu ra:```
-1000000000 -1000000000 1000000000 1000000000
```| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Đọc n | 3 | 
| 2 | Đọc tất cả các ô vuông | Bị loại bỏ | 
| 3 | In hình vuông cố định | Xong | 

Mỗi ô trong số ba ô đầu vào nằm hoàn toàn bên trong ô vuông đầu ra. Mỗi giao điểm bằng hình vuông đầu vào tương ứng. 

### Ví dụ 2 

đầu vào:```
1
1 1 2 2
```Đầu ra:```
-1000000000 -1000000000 1000000000 1000000000
```| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Đọc n | 1 | 
| 2 | Đọc vuông | Bị loại bỏ | 
| 3 | In hình vuông cố định | Xong | 

Hình vuông đầu vào được chứa trong hình vuông đầu ra, do đó giao điểm chính xác là$1 \times 1$quảng trường. 

Ví dụ này chứng minh rằng ngay cả đầu vào hợp lệ nhỏ nhất cũng được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Ngoài việc đọc đầu vào, thuật toán không thực hiện tính toán nào phụ thuộc vào$n$| 
| Không gian | O(1) | Không sử dụng thêm dung lượng | 

Giải pháp này phù hợp một cách thoải mái trong mọi giới hạn vì nó thực hiện một khối lượng công việc không đổi và không lưu trữ cấu trúc dữ liệu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    for _ in range(n):
        sys.stdin.readline()

    return "-1000000000 -1000000000 1000000000 1000000000\n"

# sample-like cases
assert run(
"""3
2 3 4 5
7 3 10 6
8 1 12 5
"""
) == "-1000000000 -1000000000 1000000000 1000000000\n"

assert run(
"""1
1 1 2 2
"""
) == "-1000000000 -1000000000 1000000000 1000000000\n"

# minimum size
assert run(
"""1
0 0 1 1
"""
) == "-1000000000 -1000000000 1000000000 1000000000\n"

# boundary coordinates
assert run(
"""1
-1000000000 -1000000000 1000000000 1000000000
"""
) == "-1000000000 -1000000000 1000000000 1000000000\n"

# multiple squares
assert run(
"""4
1 1 2 2
3 3 5 5
-7 -7 -3 -3
10 10 20 20
"""
) == "-1000000000 -1000000000 1000000000 1000000000\n"

# off-by-one style boundary test
assert run(
"""1
999999999 999999999 1000000000 1000000000
"""
) == "-1000000000 -1000000000 1000000000 1000000000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình vuông nhỏ đơn | Hình vuông cố định | Kích thước đầu vào tối thiểu | 
| Hình vuông bao phủ toàn dải | Hình vuông cố định | Tọa độ ranh giới | 
| Một số hình vuông riêng biệt | Hình vuông cố định | Hình học đầu vào không liên quan | 
| Vuông chạm tọa độ tối đa | Hình vuông cố định | Không có vấn đề riêng lẻ nào | 
| Đầu vào kiểu mẫu | Hình vuông cố định | Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
-1000000000 -1000000000 1000000000 1000000000
```Các đầu ra thuật toán:```
-1000000000 -1000000000 1000000000 1000000000
```Giao điểm là cùng một hình vuông. Diện tích của nó là dương, nên yêu cầu được thỏa mãn. 

Bây giờ hãy xem xét:```
1
999999999 999999999 1000000000 1000000000
```Hình vuông đầu vào nằm ở góc trên bên phải của vùng được phép. Hình vuông đầu ra cố định vẫn chứa nó hoàn toàn. Giao điểm bằng hình vuông đầu vào, vẫn là hình vuông hợp lệ có diện tích khác 0. 

Cuối cùng:```
3
1 1 2 2
100 100 200 200
-500 -500 -400 -400
```Cả ba hình vuông đều nằm trong hình vuông cố định. Mỗi giao lộ chính xác là hình vuông đầu vào tương ứng. Cho dù các hình vuông cách nhau bao xa thì đối số vẫn không thay đổi.
