---
title: "CF 1328A - Bài toán chia hết"
description: "Mỗi trường hợp thử nghiệm chứa hai số nguyên dương. Chúng ta chỉ có thể thực hiện một loại thao tác: tăng số đầu tiên lên một."
date: "2026-06-11T16:26:34+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1328
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 629 (Div. 3)"
rating: 800
weight: 1328
solve_time_s: 325
verified: false
draft: false
---

[CF 1328A - Vấn đề chia hết](https://codeforces.com/problemset/problem/1328/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 5 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm chứa hai số nguyên dương. Chúng ta chỉ có thể thực hiện một loại thao tác: tăng số đầu tiên lên một. Mục tiêu là đạt được giá trị gần nhất lớn hơn hoặc bằng giá trị hiện tại chia hết cho số thứ hai, trong khi sử dụng càng ít số gia càng tốt. 

Đầu vào bắt đầu bằng số lượng ca kiểm thử, sau đó mỗi cặp số được xử lý độc lập. Đối với mỗi cặp, chúng ta phải xuất ra số lượng cần thiết trước khi số đầu tiên trở thành bội số của số thứ hai. 

Số lượng ca kiểm thử đạt$10^4$, trong khi cả hai số có thể lớn bằng$10^9$. Các giới hạn này đủ nhỏ để thực hiện công việc trong thời gian không đổi cho mỗi trường hợp thử nghiệm, nhưng chúng loại trừ bất kỳ phương pháp nào liên tục thêm một giá trị cho đến khi đạt được khả năng chia hết. Trong trường hợp xấu nhất, bản thân câu trả lời có thể rất lớn, vì vậy một mô phỏng có thể cần hàng trăm triệu hoặc thậm chí hàng tỷ lần lặp. 

Một số trường hợp đáng chú ý. 

Khi số đầu tiên đã chia hết cho số thứ hai thì câu trả lời là 0. 

đầu vào:```
1
92 46
```Đầu ra:```
0
```Một công thức bất cẩn như`b - (a % b)`sản xuất`46`, điều này không chính xác vì không cần di chuyển. 

Một tình huống phức tạp khác xuất hiện khi số chia lớn hơn giá trị hiện tại. 

đầu vào:```
1
123 456
```Đầu ra:```
333
```Bội số tiếp theo của 456 chính là 456, vì vậy chúng ta cần$456-123=333$số gia tăng. 

Một ví dụ rất nhỏ cũng có thể bộc lộ từng lỗi một. 

đầu vào:```
1
10 4
```Đầu ra:```
2
```Từ$10 \bmod 4 = 2$, cần thêm hai số tăng nữa để đạt đến 12. Sử dụng biểu thức không chính xác như`b - a % b - 1`sẽ cho 1 thay vì 2. 

## Phương pháp tiếp cận 

Chiến lược đơn giản nhất là liên tục tăng số đó lên một và dừng lại ngay khi nó chia hết cho số chia. Phương pháp này đúng vì mọi thao tác đều bị ép buộc và việc dừng ở giá trị chia hết đầu tiên sẽ cho số lần di chuyển tối thiểu. 

Vấn đề với phương pháp này là thời gian chạy của nó. Giả định$a=1$Và$b=10^9$. Chúng tôi sẽ biểu diễn$999\,999\,999$tăng trước khi đạt bội số đầu tiên của$10^9$. Mô phỏng như vậy là quá chậm. 

Quan sát quan trọng là khả năng chia hết chỉ phụ thuộc vào số dư khi chia cho$b$. Nếu phần còn lại là$r$, thì chính xác$b-r$cần nhiều đơn vị hơn để đạt được bội số tiếp theo. Ngoại lệ duy nhất xảy ra khi$r=0$, vì số này đã chia hết và không cần thực hiện thao tác nào. 

Lực lượng vũ phu hoạt động vì cuối cùng nó đạt đến bội số tiếp theo, nhưng không thành công khi khoảng cách quá lớn. Nhận xét rằng số dư đã cho chúng ta biết chúng ta còn cách bội số đó bao xa sẽ rút gọn bài toán thành một vài phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử và xử lý từng cặp một cách độc lập vì các ca kiểm thử không tương tác với nhau. 
2. Tính số dư`a % b`. Điều này cho chúng ta biết giá trị hiện tại cách bội số dưới gần nhất của bao xa`b`. 
3. Nếu số dư bằng 0, xuất ra 0 vì giá trị hiện tại đã chia hết. 
4. Ngược lại, xuất ra`b - remainder`. Đại lượng này chính xác là khoảng cách từ giá trị hiện tại đến bội số tiếp theo của`b`. 

### Tại sao nó hoạt động 

Với mọi số nguyên$a$, chúng ta có thể viết$$a = kb + r,$$Ở đâu$0 \le r < b$. Nếu như$r=0$, sau đó$a$chính nó là bội số của$b$. Ngược lại bội số tiếp theo là$$(k+1)b = kb + b.$$Sự khác biệt giữa hai giá trị này là$$(k+1)b-a = b-r.$$Không thể có số gia tăng nhỏ hơn vì mọi giá trị trung gian vẫn có số dư khác 0. Do đó thuật toán luôn tạo ra số lần di chuyển tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    a, b = map(int, input().split())
    rem = a % b

    if rem == 0:
        print(0)
    else:
        print(b - rem)
```Đầu tiên, chương trình sẽ đọc số lượng ca kiểm thử và xử lý từng cặp ca kiểm thử riêng biệt. 

Với mỗi cặp, nó tính số dư sau khi chia`a`qua`b`. Số dư này xác định số đó cách bội số trước đó bao xa. Nếu phần còn lại bằng 0 thì câu trả lời ngay lập tức là 0. 

Ngược lại, khoảng cách tới bội số tiếp theo bằng`b - rem`. 

Điểm tinh tế duy nhất là trường hợp chia hết. Chỉ viết`b - (a % b)`đưa ra câu trả lời sai khi`a % b`bằng 0, vì nó sẽ xuất ra`b`thay vì`0`. Xử lý trường hợp đó một cách riêng biệt sẽ tránh được lỗi từng cái một. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 4
```| một | b | một % b | Trả lời | 
| --- | --- | --- | --- | 
| 10 | 4 | 2 | 2 | 

Phần còn lại là 2, do đó, hai số tăng sẽ đưa số này thành 12, chia hết cho 4. Ví dụ này cho thấy trường hợp bình thường trong đó giá trị hiện tại nằm giữa hai bội số. 

### Ví dụ 2 

đầu vào:```
123 456
```| một | b | một % b | Trả lời | 
| --- | --- | --- | --- | 
| 123 | 456 | 123 | 333 | 

Vì số chia vượt quá giá trị hiện tại nên bội số có thể truy cập đầu tiên chính là 456. Thuật toán tính toán chính xác$456-123=333$. 

### Ví dụ 3 

đầu vào:```
92 46
```| một | b | một % b | Trả lời | 
| --- | --- | --- | --- | 
| 92 | 46 | 0 | 0 | 

Ví dụ này thực hiện trường hợp đặc biệt khi số đã chia hết. Câu trả lời vẫn là con số không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Vì có nhiều nhất$10^4$trường hợp thử nghiệm, tổng thời gian chạy là tuyến tính theo số lượng trường hợp. Công việc liên tục trong mỗi trường hợp dễ dàng nằm gọn trong giới hạn một giây và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        a, b = map(int, input().split())
        rem = a % b

        if rem == 0:
            print(0)
        else:
            print(b - rem)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""5
10 4
13 9
100 13
123 456
92 46
"""
) == """2
5
4
333
0
""", "sample 1"

# minimum values
assert run(
"""1
1 1
"""
) == """0
""", "minimum values"

# maximum values
assert run(
"""1
1000000000 1000000000
"""
) == """0
""", "maximum equal values"

# divisor larger than number
assert run(
"""1
1 1000000000
"""
) == """999999999
""", "large gap"

# off-by-one check
assert run(
"""1
10 4
"""
) == """2
""", "remainder case"

# already divisible
assert run(
"""1
81 9
"""
) == """0
""", "exact multiple"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Giá trị nhỏ nhất có thể | 
|`1000000000 1000000000`|`0`| Giá trị bằng nhau tối đa | 
|`1 1000000000`|`999999999`| Câu trả lời rất lớn | 
|`10 4`|`2`| Trường hợp còn lại điển hình | 
|`81 9`|`0`| Đã chia hết số | 

## Vỏ cạnh 

Hãy xem xét trường hợp số đã chia hết. 

đầu vào:```
1
92 46
```Thuật toán tính toán`92 % 46 = 0`. Vì phần dư bằng 0 nên ngay lập tức nó xuất ra 0. Không cần tăng số và trường hợp đặc biệt sẽ ngăn việc trả về 46 do nhầm lẫn. 

Bây giờ hãy xem xét một số chia lớn hơn giá trị hiện tại. 

đầu vào:```
1
123 456
```Số dư bằng 123. Thuật toán tính`456 - 123 = 333`, nghĩa là tăng 123 đúng 333 lần sẽ đạt 456. Bất kỳ số gia nhỏ hơn nào cũng tạo ra giá trị dưới 456 và không thể chia hết cho 456. 

Cuối cùng, hãy xem xét một trường hợp có thể bộc lộ những sai lầm riêng lẻ. 

đầu vào:```
1
10 4
```Số dư là 2. Thuật toán trả về`4 - 2 = 2`, nghĩa là dãy sẽ trở thành 11 và sau đó là 12. Sau hai phép tính, tính chia hết sẽ đạt được. Một thao tác là không đủ vì 11 không chia hết cho 4.
