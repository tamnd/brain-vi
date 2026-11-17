---
title: "CF 1950A - Cầu thang, Đỉnh hay không?"
description: "Chúng ta có ba chữ số a, b và c và phải phân loại mối quan hệ của chúng. Một dãy được gọi là bậc thang khi các giá trị tăng dần từ trái sang phải, nghĩa là a < b < c."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1950
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 937 (Div. 4)"
rating: 800
weight: 1950
solve_time_s: 64
verified: true
draft: false
---

[CF 1950A - Cầu thang, Đỉnh hay không?](https://codeforces.com/problemset/problem/1950/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba chữ số,`a`,`b`, Và`c`và phải phân loại mối quan hệ của chúng. 

Một chuỗi được gọi là **cầu thang** khi các giá trị tăng dần từ trái sang phải, nghĩa là`a < b < c`. 

Một chuỗi được gọi là **đỉnh** khi giá trị ở giữa lớn hơn cả hai dãy lân cận, nghĩa là`a < b`Và`b > c`. 

Nếu cả hai điều kiện đều không được thỏa mãn thì ta in`"NONE"`. 

Mỗi test case chỉ chứa ba số và có tối đa 1000 test case. Vì mỗi phân loại chỉ yêu cầu một vài so sánh nên khối lượng công việc cho mỗi trường hợp kiểm thử là không đổi. Ngay cả việc triển khai đơn giản cũng chỉ thực hiện một số thao tác, vì vậy hiệu quả không bao giờ là vấn đề đáng lo ngại. 

Thách thức chính là xử lý chính xác những bất bình đẳng nghiêm ngặt. Các giá trị bằng nhau không thỏa mãn cả hai định nghĩa. 

Hãy xem xét đầu vào:```
1
1 1 2
```Câu trả lời đúng là:```
NONE
```Việc triển khai bất cẩn chỉ kiểm tra xem chuỗi có phải là không giảm hay không có thể phân loại sai chuỗi thành bậc thang. 

Một sai lầm dễ mắc phải khác là coi mức cực đại cục bộ là đỉnh mà không xác minh độ tăng của cạnh trái:```
1
3 2 1
```Câu trả lời đúng là:```
NONE
```Mặc dù`2 > 1`, điều kiện`3 < 2`là sai, vì vậy đây không phải là đỉnh điểm. 

Trường hợp cạnh thứ ba xảy ra khi tất cả các giá trị đều bằng nhau:```
1
0 0 0
```Đầu ra đúng là:```
NONE
```Cả mức tăng nghiêm ngặt lẫn điều kiện cao điểm nghiêm ngặt đều không được giữ nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra cả ba số và kiểm tra xem chúng có thỏa mãn định nghĩa hay không. Vì bản thân bài toán xác định bậc thang và đỉnh bằng cách sử dụng các bất đẳng thức nên chúng ta có thể đánh giá các bất đẳng thức đó một cách đơn giản. 

Giải pháp brute-force sẽ kiểm tra mọi mẫu có thể và so sánh các giá trị tương ứng. Bởi vì mỗi trường hợp thử nghiệm chứa chính xác ba chữ số nên nó đã chạy trong thời gian không đổi. Ngay cả với 1000 trường hợp thử nghiệm, tổng công việc vẫn rất nhỏ. 

Quan sát quan trọng là không có cấu trúc ẩn nào để khám phá. Vấn đề hoàn toàn là một bài tập thực hiện. Các định nghĩa có thể được dịch trực tiếp thành mã. 

Đầu tiên hãy kiểm tra xem`a < b < c`. Nếu đúng thì câu trả lời là`"STAIR"`. 

Ngược lại hãy kiểm tra xem`a < b > c`. Nếu đúng thì câu trả lời là`"PEAK"`. 

Nếu không có điều kiện nào phù hợp thì câu trả lời phải là`"NONE"`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`a`,`b`, Và`c`. 
3. Kiểm tra xem`a < b < c`. 

Điều này hoàn toàn phù hợp với định nghĩa của một cầu thang. 
4. Nếu điều kiện trước đó là sai, hãy kiểm tra xem`a < b > c`. 

Điều này hoàn toàn phù hợp với định nghĩa của một đỉnh cao. 
5. Nếu cả hai điều kiện đều không đúng, xuất ra`"NONE"`. 

### Tại sao nó hoạt động 

Vấn đề cung cấp các định nghĩa đầy đủ cho cả hai phân loại hợp lệ. 

Dãy số là một bậc thang khi và chỉ khi`a < b < c`. Dãy số là đỉnh khi và chỉ khi`a < b > c`. Những điều kiện này loại trừ lẫn nhau vì một giá trị không thể đồng thời thỏa mãn`b < c`Và`b > c`. 

Thuật toán kiểm tra trực tiếp định nghĩa bậc thang, sau đó kiểm tra trực tiếp định nghĩa đỉnh. Bất kỳ chuỗi còn lại nào không thỏa mãn định nghĩa và phải được phân loại là`"NONE"`. Vì mọi đầu vào có thể đều rơi vào đúng một trong các trường hợp này nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    a, b, c = map(int, input().split())

    if a < b < c:
        print("STAIR")
    elif a < b > c:
        print("PEAK")
    else:
        print("NONE")
```Phần đầu tiên đọc số lượng trường hợp thử nghiệm. 

Đối với mỗi trường hợp thử nghiệm, ba chữ số được đọc và lưu trữ trong`a`,`b`, Và`c`. 

điều kiện`a < b < c`sử dụng cú pháp so sánh chuỗi của Python. Nó tương đương với việc kiểm tra cả hai`a < b`Và`b < c`. 

Nếu điều kiện đó không thành công, đoạn mã sẽ kiểm tra`a < b > c`, tương đương với việc kiểm tra cả hai`a < b`Và`b > c`. 

Thứ tự chỉ quan trọng vì chúng ta muốn in ra một câu trả lời duy nhất. Trong thực tế, hai điều kiện không thể đồng thời đúng. 

Bất kỳ trường hợp nào liên quan đến các giá trị bằng nhau sẽ tự động rơi vào`"NONE"`bởi vì cả hai định nghĩa đều đòi hỏi sự bất đẳng thức nghiêm ngặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 5 3
```| một | b | c | a < b < c | a < b > c | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 3 | Sai | Đúng | ĐỈNH | 

Trình tự tăng từ`1`ĐẾN`5`rồi rơi xuống`3`. Giá trị ở giữa lớn hơn cả hai vế, nên câu trả lời là`"PEAK"`. 

### Ví dụ 2 

đầu vào:```
4 5 7
```| một | b | c | a < b < c | a < b > c | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 5 | 7 | Đúng | Sai | CẦU THANG | 

Các giá trị tăng dần từ trái sang phải. Điều này khớp chính xác với định nghĩa cầu thang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ thực hiện một số lượng so sánh không đổi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Với tối đa 1000 ca kiểm thử, chương trình chỉ thực hiện vài nghìn phép so sánh. Điều này thấp hơn nhiều so với giới hạn có sẵn cho cả thời gian và bộ nhớ. 

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
        a, b, c = map(int, input().split())

        if a < b < c:
            ans.append("STAIR")
        elif a < b > c:
            ans.append("PEAK")
        else:
            ans.append("NONE")

    return "\n".join(ans) + "\n"

# provided sample
assert run(
"""7
1 2 3
3 2 1
1 5 3
3 4 1
0 0 0
4 1 7
4 5 7
"""
) == (
"""STAIR
NONE
PEAK
PEAK
NONE
NONE
STAIR
"""
), "sample 1"

# minimum values
assert run(
"""1
0 0 0
"""
) == (
"""NONE
"""
), "all equal"

# simple stair
assert run(
"""1
0 1 2
"""
) == (
"""STAIR
"""
), "strictly increasing"

# simple peak
assert run(
"""1
0 9 0
"""
) == (
"""PEAK
"""
), "strict peak"

# equality blocks stair and peak
assert run(
"""1
1 1 2
"""
) == (
"""NONE
"""
), "strict inequality required"

# boundary digits
assert run(
"""1
8 9 8
"""
) == (
"""PEAK
"""
), "largest digit in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`|`NONE`| Tất cả các giá trị bằng nhau | 
|`0 1 2`|`STAIR`| Trình tự tăng nghiêm ngặt | 
|`0 9 0`|`PEAK`| Cấu trúc đỉnh hợp lệ | 
|`1 1 2`|`NONE`| Bình đẳng không thỏa mãn sự so sánh chặt chẽ | 
|`8 9 8`|`PEAK`| Giá trị chữ số biên | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1 2
```Thuật toán kiểm tra`1 < 1 < 2`, điều này sai vì`1 < 1`là sai. Sau đó nó sẽ kiểm tra`1 < 1 > 2`, điều này cũng sai. Đầu ra là`"NONE"`. Điều này xử lý chính xác sự bình đẳng. 

Coi như:```
1
3 2 1
```Điều kiện cầu thang không thành công vì`3 < 2`là sai. Điều kiện đỉnh cũng thất bại vì`3 < 2`là sai. Đầu ra trở thành`"NONE"`. Điều này ngăn chặn việc dán nhãn không chính xác cho chuỗi giảm dần là đỉnh. 

Coi như:```
1
0 0 0
```Không`0 < 0 < 0`cũng không`0 < 0 > 0`là đúng. Thuật toán in`"NONE"`, phù hợp với các định nghĩa yêu cầu bất đẳng thức nghiêm ngặt. 

Coi như:```
1
2 5 5
```Điều kiện cầu thang không thành công vì`5 < 5`là sai. Điều kiện đỉnh không thành công vì`5 > 5`là sai. Kết quả là`"NONE"`. Điều này mắc phải một lỗi phổ biến khi các giá trị lân cận bằng nhau vô tình được coi là hợp lệ.
