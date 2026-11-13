---
title: "CF 169A - Công việc vặt"
description: "Chúng tôi có một danh sách những khó khăn trong công việc. Vasya phải nhận chính xác b công việc có độ khó tối đa là x, trong khi Petya phải nhận chính xác b công việc có độ khó lớn hơn x. Giá trị x phải là số nguyên. Chúng ta cần đếm xem có bao nhiêu số nguyên thỏa mãn phép chia."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 169
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2012 Round 2 (Unofficial Div. 2 Edition)"
rating: 800
weight: 169
solve_time_s: 85
verified: true
draft: false
---

[CF 169A - Việc vặt](https://codeforces.com/problemset/problem/169/A) 

**Đánh giá:** 800 
**Thẻ:** sắp xếp 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một danh sách những khó khăn trong công việc. Vasya phải nhận chính xác`b`công việc có độ khó cao nhất`x`, trong khi Petya phải nhận chính xác`a`những công việc có độ khó thực sự lớn hơn`x`. 

giá trị`x`phải là số nguyên. Chúng ta cần đếm xem có bao nhiêu số nguyên thỏa mãn phép chia. 

Sau khi phân loại các khó khăn, điều kiện trở nên dễ lý giải hơn nhiều. Vì chính xác`b`công việc phải thỏa mãn`h[i] ≤ x`, cái đầu tiên`b`những công việc nhỏ nhất phải thuộc về Vasya, những việc còn lại`a`công việc nhà phải thuộc về Petya. 

Điều đó ngay lập tức tạo ra hai giá trị biên: 

- Công việc lớn nhất mà Vasya nhận được là`b`-giá trị nhỏ nhất thứ 
- Công việc nhỏ nhất mà Petya làm là`(b + 1)`-giá trị nhỏ nhất thứ 

Nếu chúng ta ký hiệu chúng là: 

-`L = sorted_h[b - 1]`-`R = sorted_h[b]`sau đó`x`phải thỏa mãn: 

-`x ≥ L`, nếu không thì Vasya sẽ nhận được ít hơn`b`công việc vặt. 
-`x < R`, nếu không thì Petya sẽ nhận được ít hơn`a`công việc vặt. 

Vì vậy mọi số nguyên hợp lệ`x`nằm trong khoảng`[L, R - 1]`. 

Các ràng buộc ở đây rất nhỏ, chỉ tối đa 2000 phần tử, vì vậy ngay cả các giải pháp bậc hai cũng có thể vượt qua một cách thoải mái. Tuy nhiên, cấu trúc của vấn đề dẫn đến một kết cấu rất nhỏ gọn`O(n log n)`giải pháp sau khi phân loại. 

Các trường hợp phức tạp đến từ các giá trị trùng lặp xung quanh ranh giới. 

Hãy xem xét đầu vào này:```
7 3 4
1 2 3 3 3 4 5
```Sau khi sắp xếp, giá trị nhỏ thứ tư là`3`, và giá trị nhỏ thứ năm cũng là`3`. 

Chúng tôi sẽ cần:```
3 ≤ x < 3
```điều đó là không thể. Câu trả lời đúng là`0`. 

Việc triển khai bất cẩn có thể cố gắng đếm các số giữa hai vị trí biên và vô tình trả về`1`. 

Một trường hợp tế nhị khác xảy ra khi có một khoảng cách lớn:```
5 2 3
1 2 10 20 30
```Đây:```
L = 2
R = 10
```Bất kỳ số nguyên nào từ`2`bởi vì`9`hoạt động, vì vậy câu trả lời là`8`. 

Các giá trị hợp lệ không bị hạn chế đối với những khó khăn hiện có. Chúng đều là số nguyên trong khoảng trống. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng ta có thể thử mọi số nguyên có thể`x`, đếm xem có bao nhiêu công việc thỏa mãn`h[i] ≤ x`và kiểm tra xem số đó có bằng không`b`. 

Khó khăn nhỏ nhất ít nhất là`1`, và lớn nhất có thể đạt tới`10^9`, vì vậy lặp đi lặp lại tất cả những gì có thể`x`giá trị hoàn toàn không thực tế. Ngay cả khi mỗi lần kiểm tra chỉ mất`O(n)`, quét tới`10^9`số nguyên sẽ yêu cầu khoảng`2 × 10^12`hoạt động trong trường hợp xấu nhất. 

Nhận xét quan trọng là chỉ có ranh giới giữa công việc của Vasya và công việc của Petya mới quan trọng. 

Sau khi mảng được sắp xếp, đầu tiên`b`các phần tử phải thuộc về Vasya và phần còn lại`a`các phần tử phải thuộc về Petya. Các giá trị chính xác bên trong các nhóm đó không còn quan trọng nữa. 

Giả định:```
sorted_h[b - 1] = L
sorted_h[b] = R
```Để Vasya vẫn nhận được chính xác`b`công việc nhà,`x`ít nhất phải có`L`, bởi vì công việc được giao khó khăn nhất của Vasya phải thỏa mãn`h[i] ≤ x`. 

Để Petya vẫn nhận được chính xác`a`công việc nhà,`x`phải nhỏ hơn rất nhiều so với`R`, bởi vì công việc được giao dễ dàng nhất của Petya phải thỏa mãn`h[i] > x`. 

Điều đó có nghĩa là các số nguyên hợp lệ chính xác là:```
L, L + 1, ..., R - 1
```Số đếm là:```
R - L
```Nếu như`L == R`, câu trả lời tự nhiên trở thành`0`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^9 · n) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) thêm không gian ngoài việc sắp xếp | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị`n`,`a`, Và`b`, cùng với danh sách những khó khăn trong công việc. 
2. Sắp xếp mảng độ khó theo thứ tự không giảm. 

Sau khi sắp xếp, đầu tiên`b`công việc nhà là những công việc duy nhất có thể làm được mà Vasya có thể nhận được nếu anh ấy phải thực hiện chính xác`b`công việc nhỏ nhất có thể chấp nhận được. 
3. Hãy:```
L = h[b - 1]
R = h[b]
```

`L`là khó khăn lớn nhất được giao cho Vasya, và`R`là khó khăn nhỏ nhất được giao cho Petya. 
4. Mọi số nguyên hợp lệ`x`phải thỏa mãn:```
L ≤ x < R
```Điều kiện bên trái đảm bảo Vasya sẽ hoàn thành mọi công việc nhà của mình. Điều kiện phù hợp đảm bảo Petya hoàn thành mọi công việc nhà của mình. 
5. Số số nguyên trong khoảng đó là:```
R - L
```6. In`R - L`. 

### Tại sao nó hoạt động 

Sau khi phân loại, điểm phân chia giữa hai anh em đã được ấn định. Vasya phải nhận được chính xác cái đầu tiên`b`công việc nhà và Petya phải nhận chính xác những việc còn lại`a`công việc vặt. 

Đối với mọi công việc được giao cho Vasya, chúng tôi cần`h[i] ≤ x`. Hạn chế mạnh nhất đến từ công việc khó khăn nhất của Vasya, có giá trị`L`. 

Đối với mọi công việc được giao cho Petya, chúng tôi cần`h[i] > x`. Hạn chế mạnh nhất đến từ công việc dễ dàng nhất của Petya, có giá trị`R`. 

Vì vậy, các số nguyên hợp lệ chính xác là các số nguyên nằm giữa hai ranh giới này. Bất kỳ giá trị nhỏ hơn nào sẽ loại trừ một trong các công việc của Vasya và bất kỳ giá trị lớn hơn nào sẽ bao gồm một trong các công việc của Petya. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())
    h = list(map(int, input().split()))

    h.sort()

    print(h[b] - h[b - 1])

solve()
```Giải pháp dựa hoàn toàn vào thứ tự sắp xếp. 

Sau khi sắp xếp, lập chỉ mục`b - 1`là công việc cuối cùng được giao cho Vasya, trong khi chỉ số`b`là công việc đầu tiên được giao cho Petya. Python sử dụng tính năng lập chỉ mục dựa trên 0, vì vậy các vị trí này vẫn đúng ngay cả khi vấn đề mô tả các công việc một cách khái niệm bằng cách sử dụng thứ tự dựa trên một. 

Câu trả lời đơn giản là số số nguyên từ`h[b - 1]`bởi vì`h[b] - 1`, bằng`h[b] - h[b - 1]`. 

Một lỗi thường gặp là cố gắng sử dụng`h[a - 1]`Và`h[a]`. Ranh giới được xác định bởi số đếm của Vasya chứ không phải số lượng của Petya, bởi vì lần đầu tiên`b`các phần tử được sắp xếp thuộc về Vasya. 

Một lỗi dễ mắc khác là trừ một cách thủ công:```
h[b] - h[b - 1] - 1
```Điều đó sẽ loại trừ sai điểm cuối bên trái, mặc dù`x = h[b - 1]`là hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2 3
6 2 3 100 1
```Mảng được sắp xếp:```
[1, 2, 3, 6, 100]
```| Bước | Giá trị | 
| --- | --- | 
|`b`| 3 | 
|`h[b - 1]`| 3 | 
|`h[b]`| 6 | 
| Trả lời |`6 - 3 = 3`| 

Các số nguyên hợp lệ là:```
3, 4, 5
```Dấu vết này cho thấy mọi số nguyên bên trong khoảng cách ranh giới đều hoạt động, không chỉ các giá trị đã có trong mảng. 

### Mẫu 2 

Giả sử đầu vào là:```
7 3 4
1 2 3 3 3 4 5
```Mảng được sắp xếp:```
[1, 2, 3, 3, 3, 4, 5]
```| Bước | Giá trị | 
| --- | --- | 
|`b`| 4 | 
|`h[b - 1]`| 3 | 
|`h[b]`| 3 | 
| Trả lời |`3 - 3 = 0`| 

Không có số nguyên`x`thỏa mãn:```
3 ≤ x < 3
```Ví dụ này minh họa trường hợp cạnh ranh giới trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp thống trị thời gian chạy | 
| Không gian | O(1) thêm không gian ngoài việc sắp xếp | Chỉ có một vài biến được sử dụng | 

Với`n ≤ 2000`, thời gian chạy rất nhỏ. Việc sắp xếp 2000 số nguyên có hiệu quả ngay lập tức trong giới hạn đã cho. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, a, b = map(int, input().split())
    h = list(map(int, input().split()))

    h.sort()

    print(h[b] - h[b - 1])

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue()

# provided sample
assert run(
    "5 2 3\n"
    "6 2 3 100 1\n"
) == "3\n", "sample 1"

# minimum size
assert run(
    "2 1 1\n"
    "1 2\n"
) == "1\n", "minimum size"

# all equal values
assert run(
    "4 2 2\n"
    "5 5 5 5\n"
) == "0\n", "all equal"

# large gap
assert run(
    "5 2 3\n"
    "1 2 10 20 30\n"
) == "8\n", "multiple valid x values"

# boundary duplicate
assert run(
    "7 3 4\n"
    "1 2 3 3 3 4 5\n"
) == "0\n", "duplicate boundary"

# off-by-one check
assert run(
    "5 4 1\n"
    "1 2 3 4 5\n"
) == "1\n", "single valid value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1 / 1 2`|`1`| Đầu vào hợp lệ tối thiểu | 
|`4 2 2 / 5 5 5 5`|`0`| Tất cả các giá trị bằng nhau | 
|`5 2 3 / 1 2 10 20 30`|`8`| Khoảng lớn các số nguyên hợp lệ | 
|`7 3 4 / 1 2 3 3 3 4 5`|`0`| Giá trị ranh giới trùng lặp | 
|`5 4 1 / 1 2 3 4 5`|`1`| Độ chính xác của từng ranh giới | 

## Vỏ cạnh 

Hãy xem xét trường hợp ranh giới trùng lặp:```
7 3 4
1 2 3 3 3 4 5
```Sau khi sắp xếp:```
[1, 2, 3, 3, 3, 4, 5]
```Chúng tôi tính toán:```
L = h[3] = 3
R = h[4] = 3
```Thuật toán trả về:```
3 - 3 = 0
```Điều này đúng vì không có số nguyên nào có thể đồng thời thỏa mãn:```
x ≥ 3
x < 3
```Bây giờ hãy xem xét trường hợp khoảng cách lớn:```
5 2 3
1 2 10 20 30
```Sau khi sắp xếp:```
[1, 2, 10, 20, 30]
```Chúng tôi nhận được:```
L = 2
R = 10
```Thuật toán trả về:```
10 - 2 = 8
```Các số nguyên hợp lệ là:```
2, 3, 4, 5, 6, 7, 8, 9
```Điều này xác nhận rằng câu trả lời đếm mọi số nguyên trong khoảng, không chỉ những khó khăn đã có trong mảng. 

Cuối cùng, kiểm tra đầu vào nhỏ nhất có thể:```
2 1 1
1 2
```Sau khi sắp xếp:```
[1, 2]
```Chúng tôi tính toán:```
L = 1
R = 2
```Thuật toán trả về`1`, nghĩa là chỉ`x = 1`hoạt động. 

Nếu như`x = 0`, Vasya không nhận được việc nhà. Nếu như`x = 2`, Petya không nhận được việc nhà. Logic ranh giới vẫn hoạt động hoàn hảo ở kích thước tối thiểu.
