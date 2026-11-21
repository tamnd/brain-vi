---
title: "CF 239A - Hai Túi Khoai Tây"
description: "Chúng ta biết túi thứ hai chứa y khoai tây. Túi đầu tiên ban đầu chứa một số dương x, nhưng giá trị đó đã bị mất. Thông tin duy nhất còn lại là tổng số khoai tây x + y chia hết cho k và không vượt quá n."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 239
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 148 (Div. 2)"
rating: 1200
weight: 239
solve_time_s: 83
verified: true
draft: false
---

[CF 239A - Hai túi khoai tây](https://codeforces.com/problemset/problem/239/A) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Biết túi thứ hai chứa`y`khoai tây. Túi đầu tiên ban đầu chứa một số dương`x`, nhưng giá trị đó đã bị mất. Thông tin duy nhất còn lại là tổng số khoai tây,`x + y`, đã chia hết cho`k`và không vượt quá`n`. 

Nhiệm vụ là in mọi giá trị có thể có của`x`thỏa mãn điều kiện đó theo thứ tự tăng dần. 

Các ràng buộc ngay lập tức hướng tới một giải pháp số học đơn giản. Bản thân các giá trị có thể lớn bằng`10^9`, vì vậy lặp đi lặp lại mọi thứ có thể`x`từ`1`ĐẾN`n`sẽ quá đắt trong bối cảnh chung. Đồng thời, điều kiện`n / k ≤ 10^5`rất lộ liễu. Nó có nghĩa là số bội của`k`lên đến`n`nhiều nhất là`100000`, vì vậy việc lặp trực tiếp qua các tổng hợp lệ là hoàn toàn an toàn. 

Quan sát quan trọng là chúng ta không thực sự tìm kiếm những con số tùy ý. Chúng tôi chỉ quan tâm đến tổng số chia hết cho`k`. 

Có một số trường hợp khó bỏ sót. 

Giả sử tổng số không thể lớn hơn túi đã biết. 

đầu vào:```
10 1 10
```Tổng phải chia hết cho`1`, nhưng cũng phải thỏa mãn`x + 10 ≤ 10`, lực nào`x ≤ 0`. Từ`x`phải là tích cực, câu trả lời đúng là:```
-1
```Việc triển khai bất cẩn có thể vô tình bao gồm`0`. 

Một trường hợp phức tạp khác xuất hiện khi bội số hợp lệ đầu tiên tạo ra`x = 0`. 

đầu vào:```
5 5 15
```bội số của`5`lên đến`15`là`5, 10, 15`. 

Đối với tổng số`5`Và`10`, chúng tôi nhận được`x = 0`Và`x = 5`. Chỉ cho phép các giá trị dương, vì vậy câu trả lời là:```
5 10
```Nếu chúng ta quên loại trừ số 0, kết quả đầu ra sẽ không chính xác. 

Một tình huống tinh tế hơn là khi không có bội số nào lớn hơn`y`. 

đầu vào:```
8 10 15
```bội số duy nhất của`10`không vượt quá`15`là`10`, mang lại`x = 2`. Điều đó là hợp lệ, vì vậy câu trả lời là:```
2
```Điều kiện là về tổng số chứ không phải về`x`chính nó được chia cho`k`. Một lỗi phổ biến là kiểm tra`x % k == 0`, điều này sẽ từ chối trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi thử mọi giá trị có thể của`x`từ`1`ĐẾN`n - y`. Đối với mỗi ứng cử viên, chúng tôi kiểm tra xem`(x + y) % k == 0`. Mọi giá trị hợp lệ sẽ được thêm vào câu trả lời. 

Điều này có hiệu quả vì các điều kiện dễ dàng được xác minh một cách độc lập. Vấn đề là quy mô. Trong trường hợp xấu nhất,`n`có thể đạt được`10^9`, do đó, việc lặp qua tất cả các ứng cử viên sẽ yêu cầu tới hàng tỷ lần kiểm tra, vượt xa những gì thực tế. 

Cấu trúc của điều kiện chia hết cho hướng tốt hơn nhiều. Thay vì đoán`x`, chúng ta có thể nghĩ về tổng số củ khoai tây. 

Tổng cộng`x + y`phải là bội số của`k`. Điều đó có nghĩa là mọi tổng hợp lệ đều có dạng:$x+y=m\cdot k$đối với một số nguyên`m`. 

Vì vậy, thay vì lặp đi lặp lại tất cả những gì có thể`x`, chúng tôi lặp lại tất cả các bội số của`k`lên đến`n`. Đối với mỗi bội số`t`, chúng tôi tính toán:$x=t-y$Nếu như`x > 0`, thì đó là một câu trả lời hợp lệ. 

Sự khác biệt quan trọng là số lượng ứng cử viên. chỉ có`n / k`bội số của`k`lên đến`n`, và bài toán đảm bảo đại lượng này lớn nhất là`10^5`. Điều đó làm cho giải pháp tối ưu hóa dễ dàng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(n/k) | O(1) không bao gồm đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`y`,`k`, Và`n`. 
2. Lặp lại từng bội số của`k`từ`k`lên đến`n`. 
3. Đối với mỗi bội số`t`, tính toán`x = t - y`. 
4. Kiểm tra xem`x`là tích cực. 

Bài toán yêu cầu túi đầu tiên phải chứa ít nhất một củ khoai tây, vì vậy`x = 0`không hợp lệ. 
5. Nếu`x > 0`, hãy thêm nó vào danh sách câu trả lời. 
6. Sau khi xử lý tất cả các bội số: 

Nếu danh sách câu trả lời trống, hãy in`-1`. Nếu không thì in tất cả các giá trị được thu thập, phân tách bằng dấu cách. 

### Tại sao nó hoạt động 

Mọi cấu hình hợp lệ phải thỏa mãn hai điều kiện:$x+y\le n$Và$(x+y)\bmod k=0$Thuật toán liệt kê mọi bội số của`k`điều đó không vượt quá`n`. Mỗi bội số như vậy đại diện cho tổng số khoai tây có thể có. Trừ`y`cho giá trị tương ứng duy nhất của`x`. 

Không thể bỏ sót câu trả lời hợp lệ nào vì mọi tổng hợp lệ đều phải xuất hiện trong số các bội số đó. Không thể đưa vào câu trả lời không hợp lệ vì chúng tôi chỉ chấp nhận câu trả lời tích cực`x`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

y, k, n = map(int, input().split())

ans = []

for total in range(k, n + 1, k):
    x = total - y
    if x > 0:
        ans.append(str(x))

if ans:
    print(" ".join(ans))
else:
    print(-1)
```Vòng lặp lặp trực tiếp trên bội số của`k`. Điều này phù hợp với cấu trúc toán học của bài toán và tránh việc kiểm tra các giá trị không liên quan. 

biểu hiện`range(k, n + 1, k)`tạo ra:```
k, 2k, 3k, ...
```lên đến`n`. 

Đối với mỗi tổng, chúng tôi xây dựng lại số tiền còn thiếu bằng cách sử dụng:```
x = total - y
```Việc kiểm tra tính tích cực là rất quan trọng. Giá trị ở đâu`x == 0`không hợp lệ vì túi đầu tiên ban đầu chứa ít nhất một củ khoai tây. 

Giải pháp lưu trữ câu trả lời dưới dạng chuỗi ngay lập tức, giúp bước in cuối cùng trở nên đơn giản và hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 1 10
```| tổng cộng | x = tổng - y | Có hiệu lực? | trả lời | 
| --- | --- | --- | --- | 
| 1 | -9 | Không | [] | 
| 2 | -8 | Không | [] | 
| 3 | -7 | Không | [] | 
| ... | ... | ... | [] | 
| 10 | 0 | Không | [] | 

Đầu ra cuối cùng:```
-1
```Ví dụ này thể hiện yêu cầu tích cực nghiêm ngặt. Mặc dù mọi tổng đều chia hết cho`1`, không có giá trị dương của`x`tồn tại. 

### Ví dụ 2 

đầu vào:```
5 3 20
```| tổng cộng | x = tổng - y | Có hiệu lực? | trả lời | 
| --- | --- | --- | --- | 
| 3 | -2 | Không | [] | 
| 6 | 1 | Có | [1] | 
| 9 | 4 | Có | [1, 4] | 
| 12 | 7 | Có | [1, 4, 7] | 
| 15 | 10 | Có | [1, 4, 7, 10] | 
| 18 | 13 | Có | [1, 4, 7, 10, 13] | 

Đầu ra cuối cùng:```
1 4 7 10 13
```Dấu vết này cho thấy mỗi bội số của`k`tương ứng với chính xác một giá trị có thể có của`x`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n/k) | Chúng tôi lặp qua từng bội số của`k`lên đến`n`| 
| Không gian | O(1) không bao gồm đầu ra | Chỉ có một vài biến được sử dụng | 

Ràng buộc`n / k ≤ 10^5`đảm bảo vòng lặp thực hiện tối đa một trăm nghìn lần lặp, điều này không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    y, k, n = map(int, input().split())

    ans = []

    for total in range(k, n + 1, k):
        x = total - y
        if x > 0:
            ans.append(str(x))

    if ans:
        print(" ".join(ans))
    else:
        print(-1)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("10 1 10\n") == "-1", "sample 1"

# custom cases
assert run("5 3 20\n") == "1 4 7 10 13", "basic valid sequence"

assert run("1 1 2\n") == "1", "minimum positive answer"

assert run("5 5 15\n") == "5 10", "exclude x = 0"

assert run("999999999 1000000000 1000000000\n") == "1", "large values"

assert run("8 10 15\n") == "2", "x itself need not be divisible by k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 3 20`|`1 4 7 10 13`| Tiến trình tiêu chuẩn của các câu trả lời hợp lệ | 
|`1 1 2`|`1`| Trường hợp hợp lệ không tầm thường nhỏ nhất | 
|`5 5 15`|`5 10`| Đảm bảo`x = 0`bị loại trừ | 
|`999999999 1000000000 1000000000`|`1`| Xử lý đúng số nguyên lớn | 
|`8 10 15`|`2`| Tổng phải chia hết chứ không được`x`| 

## Vỏ cạnh 

Xét trường hợp không có giá trị dương của`x`tồn tại. 

đầu vào:```
10 1 10
```Thuật toán kiểm tra tổng số từ`1`bởi vì`10`. Mỗi giá trị tính toán của`x = total - 10`là không tích cực. Vì danh sách câu trả lời vẫn trống nên thuật toán sẽ in:```
-1
```Điều này xử lý chính xác tình huống túi đã biết đã sử dụng toàn bộ tổng số tiền được phép. 

Bây giờ hãy xem xét ranh giới nơi bội số nhỏ nhất cho`x = 0`. 

đầu vào:```
5 5 15
```Tổng số được kiểm tra là`5`,`10`, Và`15`. 

Vì`total = 5`:```
x = 5 - 5 = 0
```Điều này bị từ chối vì túi đầu tiên phải chứa ít nhất một củ khoai tây. 

Đối với các tổng còn lại:```
x = 10 - 5 = 5
x = 15 - 5 = 10
```Cả hai đều được chấp nhận, tạo ra:```
5 10
```Điều này khẳng định sự nghiêm ngặt`x > 0`tình trạng được xử lý chính xác. 

Cuối cùng, hãy xem xét một trường hợp trong đó`x`bản thân nó không chia hết cho`k`. 

đầu vào:```
8 10 15
```bội số duy nhất của`10`không vượt quá`15`là`10`. 

Thuật toán tính toán:```
x = 10 - 8 = 2
```Từ`2 > 0`, nó được in. 

Đầu ra là:```
2
```Điều này xác minh rằng tính chia hết áp dụng cho tổng`x + y`, không để`x`riêng lẻ.
