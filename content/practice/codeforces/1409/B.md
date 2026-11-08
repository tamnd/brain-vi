---
title: "CF 1409B - Sản phẩm tối thiểu"
description: "Chúng ta bắt đầu bằng hai số a và b. Chúng ta được phép thực hiện tối đa n phép toán giảm. Mỗi phép toán giảm a hoặc b đúng một. Điều đáng chú ý là a không bao giờ có thể xuống dưới x và b không bao giờ có thể xuống dưới y."
date: "2026-06-11T07:33:55+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1409
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 667 (Div. 3)"
rating: 1100
weight: 1409
solve_time_s: 106
verified: true
draft: false
---

[CF 1409B - Sản phẩm tối thiểu](https://codeforces.com/problemset/problem/1409/B) 

**Đánh giá:** 1100 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai số,`a`Và`b`. Chúng tôi được phép biểu diễn nhiều nhất`n`hoạt động giảm dần. Mỗi thao tác đều giảm`a`hoặc`b`bằng chính xác một. Điều hấp dẫn là`a`không bao giờ có thể đi xuống dưới`x`, Và`b`không bao giờ có thể đi xuống dưới`y`. 

Mục tiêu là sử dụng tới`n`giảm theo bất cứ cách nào chúng ta muốn để sản phẩm cuối cùng`a × b`trở nên nhỏ nhất có thể. 

Đầu vào chứa tới 20.000 trường hợp thử nghiệm độc lập. Bản thân các giá trị có thể lớn bằng`10^9`, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào thực hiện giảm từng cái một. Một trường hợp thử nghiệm duy nhất có thể yêu cầu xem xét tới một tỷ hoạt động và với 20.000 trường hợp thử nghiệm thì điều đó hoàn toàn không khả thi. 

Phần thú vị của vấn đề là quyết định nơi nào sẽ chi tiêu số tiền giảm có sẵn. Việc giảm một trong hai số sẽ làm giảm tích, nhưng vì các số có giới hạn dưới khác nhau nên thứ tự chúng ta giảm chúng rất quan trọng. 

Hãy xem xét trường hợp thử nghiệm:```
a=10, b=10, x=8, y=5, n=3
```Nếu chúng ta thực hiện cả ba thao tác trên`a`, chúng tôi nhận được:```
7 operations not allowed because x=8
10 -> 8
product = 8 × 10 = 80
```Nếu chúng ta dành cả ba vào`b`:```
10 -> 7
product = 10 × 7 = 70
```Sự lựa chọn thứ hai là tốt hơn. 

Một lỗi phổ biến là luôn giảm số lớn hơn trước. Ví dụ:```
a=12, b=8, x=8, y=7, n=2
```Chỉ giảm`a`mang lại:```
10 × 8 = 80
```Nhưng việc chia các khoản giảm sẽ mang lại:```
11 × 7 = 77
```cái nào nhỏ hơn 

Một trường hợp tinh tế khác xảy ra khi một biến đạt đến giới hạn dưới trước khi tất cả các thao tác được sử dụng.```
a=10, b=11, x=9, y=1, n=10
```Chúng ta chỉ có thể giảm`a`một lần. Chín hoạt động còn lại phải đi đến`b`:```
9 × 2 = 18
```Một giải pháp dành tất cả các hoạt động một cách mù quáng cho một biến sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Quan điểm brute-force là quyết định chính xác có bao nhiêu thao tác được áp dụng cho`a`. Giả sử chúng ta chi tiêu`k`giảm trên`a`. Sau đó, chúng tôi dành phần còn lại`n-k`giảm trên`b`càng tốt. 

Ý tưởng này đúng vì một lần`k`đã được sửa, sự lựa chọn tốt nhất cho`b`là điều hiển nhiên: giảm nó đến mức cho phép. 

Vấn đề là ở chỗ đó`k`có thể dao động lên đến`10^9`. Liệt kê mọi khả năng là không thể. 

Quan sát quan trọng là chỉ có hai thứ tự có ý nghĩa có thể áp dụng mức giảm. 

Giả sử chúng ta muốn giảm thiểu một sản phẩm. Mỗi mức giảm sẽ có giá trị hơn khi áp dụng cho một số lớn hơn vì việc giảm hệ số lớn hơn sẽ làm giảm tích nhiều hơn. 

Điều đó gợi ý hai chiến lược tự nhiên: 

Đầu tiên, giảm`a`càng nhiều càng tốt, sau đó sử dụng mọi thao tác còn lại trên`b`. 

Thứ hai, giảm`b`càng nhiều càng tốt, sau đó sử dụng mọi thao tác còn lại trên`a`. 

Hóa ra một trong hai thứ tự này luôn là tối ưu. 

Tại sao? Bất kỳ chuỗi thao tác hợp lệ nào cũng có thể được sắp xếp lại thành một trong những mệnh lệnh tham lam này mà không làm cho kết quả trở nên tồi tệ hơn. Điều quan trọng duy nhất là cuối cùng mỗi biến nhận được bao nhiêu mức giảm và đối với tổng ngân sách cố định, việc phân phối tốt nhất đạt được bằng cách làm cạn kiệt một bên trước. Vì một trong hai biến có thể là biến tốt hơn nên được sử dụng hết trước nên chúng ta chỉ cần kiểm tra cả hai khả năng và lấy tích nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) hoặc O(số lần phân phối) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cân nhắc chiến lược “giảm`a`đầu tiên". 

Tính xem bao nhiêu`a`có thể giảm:```
decrease_a = min(n, a - x)
```Áp dụng mức giảm đó cho`a`. 
2. Tính xem còn lại bao nhiêu thao tác.```
remaining = n - decrease_a
```3. Sử dụng các thao tác còn lại trên`b`, tôn trọng giới hạn dưới của nó.```
decrease_b = min(remaining, b - y)
```4. Tính kết quả. 
5. Lặp lại quá trình tương tự với vai trò đảo ngược. 

Giảm đầu tiên`b`càng nhiều càng tốt, sau đó dành mọi hoạt động còn lại cho`a`. 
6. Xuất ra sản phẩm nhỏ hơn trong hai sản phẩm. 

### Tại sao nó hoạt động 

Hãy để mức giảm cuối cùng là`ra`Và`rb`, Ở đâu`ra + rb ≤ n`. 

Sản phẩm cuối cùng là```
(a - ra)(b - rb)
```tùy thuộc vào```
0 ≤ ra ≤ a-x
0 ≤ rb ≤ b-y
```Đối với bất kỳ giải pháp khả thi nào, việc đẩy các mức giảm bổ sung vào một biến cho đến khi đạt đến giới hạn không thể làm tăng sản phẩm. Câu hỏi duy nhất là biến nào sẽ được sử dụng hết trước. Hai khả năng chính xác là:```
a first, then b
b first, then a
```Mọi phân bổ tối ưu đều tương ứng với một trong các trường hợp biên này, vì vậy việc kiểm tra cả hai và lấy mức tối thiểu sẽ đảm bảo câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        a, b, x, y, n = map(int, input().split())

        # Strategy 1: reduce a first
        da = min(n, a - x)
        a1 = a - da
        rem = n - da

        db = min(rem, b - y)
        b1 = b - db

        prod1 = a1 * b1

        # Strategy 2: reduce b first
        db = min(n, b - y)
        b2 = b - db
        rem = n - db

        da = min(rem, a - x)
        a2 = a - da

        prod2 = a2 * b2

        print(min(prod1, prod2))

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp thực hiện hai lệnh ứng cử viên. 

biểu hiện`a - x`là số tiền tối đa mà`a`có thể được giảm bớt. Lấy`min(n, a - x)`đảm bảo chúng ta không bao giờ vi phạm giới hạn dưới. 

Sau khi giảm một biến, ngân sách còn lại được tính toán và áp dụng cho biến kia theo cách tương tự. 

Các sản phẩm có thể lớn như:```
10^9 × 10^9 = 10^18
```Số nguyên Python xử lý việc này một cách an toàn mà không bị tràn. 

Một lỗi triển khai phổ biến là quên khôi phục các giá trị ban đầu trước khi thử nghiệm chiến lược thứ hai. Mã tránh điều này bằng cách lưu trữ các biến riêng biệt (`a1`,`b1`,`a2`,`b2`) cho cả hai kịch bản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 10 8 5 3
```Chiến lược 1, giảm`a`đầu tiên: 

| Bước | một | b | Còn lại n | 
| --- | --- | --- | --- | 
| Ban đầu | 10 | 10 | 3 | 
| Giảm một | 8 | 10 | 1 | 
| Giảm b | 8 | 9 | 0 | 

Sản phẩm:```
8 × 9 = 72
```Chiến lược 2, giảm`b`đầu tiên: 

| Bước | một | b | Còn lại n | 
| --- | --- | --- | --- | 
| Ban đầu | 10 | 10 | 3 | 
| Giảm b | 10 | 7 | 0 | 

Sản phẩm:```
10 × 7 = 70
```Trả lời:```
70
```Ví dụ này cho thấy tại sao việc thử cả hai lệnh là cần thiết. 

### Ví dụ 2 

đầu vào:```
12 8 8 7 2
```Chiến lược 1, giảm`a`đầu tiên: 

| Bước | một | b | Còn lại n | 
| --- | --- | --- | --- | 
| Ban đầu | 12 | 8 | 2 | 
| Giảm một | 10 | 8 | 0 | 

Sản phẩm:```
80
```Chiến lược 2, giảm`b`đầu tiên: 

| Bước | một | b | Còn lại n | 
| --- | --- | --- | --- | 
| Ban đầu | 12 | 8 | 2 | 
| Giảm b | 12 | 7 | 1 | 
| Giảm một | 11 | 7 | 0 | 

Sản phẩm:```
77
```Trả lời:```
77
```Dấu vết này chứng tỏ rằng việc thực hiện một số thao tác cho từng biến có thể tốt hơn việc thực hiện tất cả các thao tác đó cho một biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Ngay cả với 20.000 trường hợp thử nghiệm, tổng công việc vẫn rất nhỏ. Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        a, b, x, y, n = map(int, input().split())

        da = min(n, a - x)
        a1 = a - da
        rem = n - da
        db = min(rem, b - y)
        b1 = b - db
        p1 = a1 * b1

        db = min(n, b - y)
        b2 = b - db
        rem = n - db
        da = min(rem, a - x)
        a2 = a - da
        p2 = a2 * b2

        ans.append(str(min(p1, p2)))

    return "\n".join(ans)

# provided samples
assert run(
"""7
10 10 8 5 3
12 8 8 7 2
12343 43 4543 39 123212
1000000000 1000000000 1 1 1
1000000000 1000000000 1 1 1000000000
10 11 2 1 5
10 11 9 1 10
"""
) == "\n".join([
    "70",
    "77",
    "177177",
    "999999999000000000",
    "999999999",
    "55",
    "10"
])

# minimum values
assert run(
"""1
1 1 1 1 1
"""
) == "1", "already at lower bounds"

# all values equal
assert run(
"""1
5 5 5 5 100
"""
) == "25", "no reductions possible"

# boundary where one variable reaches its limit
assert run(
"""1
10 11 9 1 10
"""
) == "10", "remaining operations must go to b"

# very large values
assert run(
"""1
1000000000 1000000000 1 1 1000000000
"""
) == "999999999"

print("OK")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1`|`1`| Ví dụ kích thước tối thiểu | 
|`5 5 5 5 100`|`25`| Không thể giảm giá | 
|`10 11 9 1 10`|`10`| Một biến chạm tới giới hạn dưới của nó ngay lập tức | 
|`1000000000 1000000000 1 1 1000000000`|`999999999`| Số học có giá trị lớn nhất và phạm vi 64 bit | 

## Vỏ cạnh 

### Một biến không thể giảm được chút nào 

đầu vào:```
a=5, b=10, x=5, y=1, n=4
```Thuật toán tính toán:```
a-x = 0
```Vì vậy giảm`a`đầu tiên không có gì thay đổi. Tất cả bốn hoạt động đi đến`b`:```
5 × 6 = 30
```Chiến lược thứ hai đạt được kết quả tương tự. Giới hạn dưới trên`a`được tôn trọng tự động bởi vì`min(n, a-x)`trở thành số không. 

### Nhiều hoạt động hơn tổng mức giảm hiện có 

đầu vào:```
a=10, b=10, x=8, y=7, n=100
```Chỉ một```
(10-8) + (10-7) = 5
```có thể giảm được. 

Thuật toán giảm cả hai biến về giá trị tối thiểu của chúng:```
8 × 7 = 56
```Các thao tác bổ sung đơn giản là không được sử dụng, điều này được cho phép vì vấn đề cho biết "nhiều nhất`n`hoạt động". 

### Câu trả lời hay hơn phụ thuộc vào thứ tự 

đầu vào:```
a=10, b=10, x=8, y=5, n=3
```Giảm`a`sản lượng đầu tiên:```
8 × 9 = 72
```Giảm`b`sản lượng đầu tiên:```
10 × 7 = 70
```Thuật toán đánh giá cả hai và trả về`70`. 

Đây chính xác là tình huống phá vỡ các giải pháp tham lam chỉ cam kết theo một đơn hàng. 

### Việc chia cắt giảm là cần thiết 

đầu vào:```
a=12, b=8, x=8, y=7, n=2
```Kết quả tốt nhất là:```
11 × 7 = 77
```Chiến lược thứ hai tự nhiên tìm thấy điều này bằng cách giảm`b`đến mức tối thiểu trước tiên và sau đó thực hiện thao tác còn lại cho`a`. Việc kiểm tra cả hai lệnh đảm bảo rằng việc phân bổ hỗn hợp như vậy không bị bỏ sót.
