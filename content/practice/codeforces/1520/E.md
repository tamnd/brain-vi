---
title: "CF 1520E - Xếp Cừu"
description: "Chúng ta có một bảng một chiều được biểu diễn bằng một chuỗi. Mỗi '' là một con cừu và mỗi '.' là một ô trống. Trong một lần di chuyển, một con cừu có thể di chuyển chính xác một vị trí sang trái hoặc phải vào ô trống liền kề."
date: "2026-06-10T18:07:12+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1520
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 719 (Div. 3)"
rating: 1400
weight: 1520
solve_time_s: 139
verified: true
draft: false
---

[CF 1520E - Sắp xếp cừu](https://codeforces.com/problemset/problem/1520/E) 

**Đánh giá:** 1400 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng một chiều được biểu diễn bằng một chuỗi. Mỗi`'*'`là một con cừu và mỗi con`'.'`là một ô trống. Trong một lần di chuyển, một con cừu có thể di chuyển chính xác một vị trí sang trái hoặc phải vào ô trống liền kề. Mục tiêu là làm cho tất cả cừu chiếm các vị trí liên tiếp, không có khoảng cách giữa chúng, đồng thời giảm thiểu tổng số lần di chuyển của một ô. 

Chi tiết quan trọng là cừu không thể phân biệt được. Chúng tôi không quan tâm con cừu nào sẽ ở vị trí cuối cùng nào, chỉ quan tâm rằng tất cả những con cừu đều trở nên liền kề nhau. Vì mỗi lần di chuyển sẽ thay đổi vị trí của một con cừu nên câu trả lời là tổng quãng đường mà tất cả các con cừu đã đi được. 

Độ dài của một ca kiểm thử có thể lên tới một triệu trên tất cả các ca kiểm thử cộng lại. Bất kỳ thuật toán nào liên tục mô phỏng các chuyển động hoặc thử mọi phân đoạn cuối cùng có thể đều sẽ quá chậm. Chúng tôi cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một số trường hợp đặc biệt đáng được quan tâm đặc biệt. 

Nếu không có con cừu nào cả:```
...
```Câu trả lời là`0`. Những con cừu đã được "nhóm" lại vì không có con nào cả. Việc thực hiện bất cẩn có thể cố gắng truy cập vào con cừu ở giữa và gây ra sự cố. 

Nếu có đúng một con cừu:```
.*.
```Câu trả lời là`0`. Một con cừu đã là một nhóm liền kề. 

Nếu cừu bị ngăn cách bởi những khoảng trống lớn:```
*...*...*
```Giải pháp tối ưu là không di chuyển mọi thứ về một điểm cuối. Một chiến lược ngây thơ tập hợp những con cừu ở ngoài cùng bên trái sẽ thực hiện những công việc không cần thiết. Điểm tụ tập tốt nhất được xác định bởi con cừu ở giữa. 

Một trường hợp tế nhị khác là khi số lượng cừu là số chẵn:```
*.*.*
```Có hai ứng cử viên cừu trung bình. Vị trí trung vị cho tổng khoảng cách tối thiểu như nhau. Việc thực hiện chỉ cần chọn một cách nhất quán. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là chọn một khối liền kề cuối cùng và tính xem cần bao nhiêu bước di chuyển để đặt từng con cừu vào khối đó. Giả sử có`k`con cừu. Nếu khối bắt đầu ở vị trí`L`, thì con cừu phải chiếm giữ các vị trí:```
L, L+1, L+2, ..., L+k-1
```Chúng ta có thể ghép những con cừu từ trái sang phải và tính tổng chi phí di chuyển. Lặp lại điều này cho mọi vị trí bắt đầu có thể sẽ tạo ra câu trả lời đúng. 

Vấn đề là có tới`n`các vị trí bắt đầu có thể và đánh giá từng chi phí`O(k)`. Trong trường hợp xấu nhất điều này trở thành`O(n²)`, điều đó là không thể đối với`n = 10^6`. 

Để cải thiện điều này, chúng ta cần hiểu điều gì thực sự quan trọng. 

Giả sử vị trí của cừu là:```
p0, p1, p2, ..., p(k-1)
```Nếu chúng chiếm giữ các ô liên tiếp thì cừu`i`phải đi đến:```
x + i
```cho một số vị trí bắt đầu`x`. 

Tổng chi phí trở thành:```
|p0 - (x+0)| +
|p1 - (x+1)| +
...
|p(k-1) - (x+k-1)|
```Sắp xếp lại biểu thức bằng cách xác định:```
qi = pi - i
```Khi đó chi phí sẽ trở thành:```
|q0 - x| + |q1 - x| + ... + |q(k-1) - x|
```Bây giờ vấn đề đã chuyển thành một nhiệm vụ cổ điển: chọn một giá trị`x`giảm thiểu tổng độ lệch tuyệt đối. 

Tổng độ lệch tuyệt đối tối thiểu đạt được ở mức trung vị. 

Điều đó có nghĩa là chúng ta không cần phải tìm kiếm trên tất cả các khối cuối cùng có thể có. Chúng tôi chỉ cần: 

1. Ghi lại vị trí của tất cả các con cừu. 
2. Xây dựng các giá trị được chuyển đổi`qi = pi - i`. 
3. Chọn giá trị chuyển đổi trung bình. 
4. Tổng khoảng cách đến trung vị đó. 

Vì vị trí của cừu được thu thập theo thứ tự tăng dần nên chuỗi biến đổi cũng không giảm, do đó, số trung vị sẽ có sẵn ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) trường hợp xấu nhất | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi và lưu trữ chỉ số của tất cả các con cừu trong một mảng`pos`. 
2. Nếu không có hoặc có một con cừu, xuất ra`0`. 
3. Hãy để`k`là số lượng cừu. 
4. Xây dựng dãy biến đổi:```
q[i] = pos[i] - i
```Điều này loại bỏ tác dụng của việc buộc cừu vào các vị trí liên tiếp. 
5. Chọn giá trị trung vị:```
median = q[k // 2]
```Từ`q`được sắp xếp thì đây là giá trị trung vị hợp lệ. 
6. Tính toán:```
answer = Σ |q[i] - median|
```7. Xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Giả sử khối liền kề cuối cùng bắt đầu ở vị trí`x`. Sau đó cừu`i`phải chiếm vị trí`x+i`, bởi vì việc thay đổi thứ tự tương đối của cừu chỉ có thể làm tăng sự di chuyển và không bao giờ có lợi. 

Chi phí là:```
Σ |pos[i] - (x+i)|
```Viết lại:```
Σ |(pos[i]-i) - x|
```Các giá trị`pos[i]-i`được cố định. Biến duy nhất còn lại là`x`. Thuộc tính cơ bản của các giá trị tuyệt đối là tổng khoảng cách đến một tập hợp số được giảm thiểu ở mức trung vị. Việc chọn giá trị chuyển đổi trung bình mang lại chi phí di chuyển tối thiểu có thể, đây chính xác là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        pos = []

        for i, ch in enumerate(s):
            if ch == '*':
                pos.append(i)

        k = len(pos)

        if k <= 1:
            answers.append("0")
            continue

        q = [pos[i] - i for i in range(k)]
        median = q[k // 2]

        ans = 0
        for x in q:
            ans += abs(x - median)

        answers.append(str(ans))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên trích xuất vị trí của tất cả các con cừu. Chỉ làm việc với các vị trí của con cừu đơn giản hơn nhiều so với việc suy luận về các ô trống. 

Sự biến đổi`pos[i] - i`là bước then chốt. Nếu không có nó, chúng ta vẫn phải tính đến thực tế là các vị trí cuối cùng phải liên tiếp nhau. Trừ`i`loại bỏ ràng buộc đó và biến vấn đề thành một vấn đề tối thiểu hóa trung vị thuần túy. 

Trung vị được lấy trực tiếp từ mảng được chuyển đổi. Không cần sắp xếp vì các vị trí ban đầu của cừu được sắp xếp theo thứ tự tăng dần và việc trừ các chỉ số tăng dần sẽ bảo toàn thứ tự không giảm. 

Câu trả lời có thể trở nên lớn nên chúng ta tích lũy nó thành một biến số nguyên. Số nguyên Python tự động xử lý các giá trị vượt quá giới hạn 32 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
*.*...*.**
```Vị trí của cừu:```
[0, 2, 6, 8, 9]
```| tôi | vị trí[i] | q[i] = pos[i] - i | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 2 | 1 | 
| 2 | 6 | 4 | 
| 3 | 8 | 5 | 
| 4 | 9 | 5 | 

Trung vị:```
q[2] = 4
```Tổng khoảng cách: 

| q[i] | |q[i]-4| | 

|---|---| 

| 0 | 4 | 

| 1 | 3 | 

| 4 | 0 | 

| 5 | 1 | 

| 5 | 1 | 

Tổng cộng:```
4 + 3 + 0 + 1 + 1 = 9
```Đầu ra:```
9
```Ví dụ này cho thấy tọa độ được chuyển đổi giúp giảm vấn đề tìm khoảng cách từ đường trung tuyến như thế nào. 

### Ví dụ 2 

đầu vào:```
**.*..
```Vị trí:```
[0, 1, 3]
```| tôi | vị trí[i] | q[i] | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 1 | 0 | 
| 2 | 3 | 1 | 

Trung vị:```
0
```Tổng khoảng cách: 

| q[i] | |q[i]-0| | 

|---|---| 

| 0 | 0 | 

| 0 | 0 | 

| 1 | 1 | 

Tổng cộng:```
1
```Đầu ra:```
1
```Dấu vết chứng tỏ rằng chỉ cần một lần di chuyển, di chuyển con cừu ngoài cùng bên phải còn lại của một ô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(k) | Lưu trữ vị trí của tất cả cừu | 
| | | Ở đâu`k`là số lượng cừu | 

Tổng của tất cả độ dài chuỗi trong các trường hợp thử nghiệm nhiều nhất là`10^6`. Quét tuyến tính của từng trường hợp thử nghiệm dễ dàng phù hợp với thời gian giới hạn và việc lưu trữ vị trí cừu yêu cầu nhiều nhất`O(n)`ký ức. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        pos = [i for i, c in enumerate(s) if c == '*']

        if len(pos) <= 1:
            out.append("0")
            continue

        q = [pos[i] - i for i in range(len(pos))]
        med = q[len(pos) // 2]

        ans = sum(abs(x - med) for x in q)
        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""5
6
**.*..
5
*****
3
.*.
3
...
10
*.*...*.**
"""
) == """1
0
0
0
9"""

# minimum size, no sheep
assert run(
"""1
1
.
"""
) == "0"

# minimum size, one sheep
assert run(
"""1
1
*
"""
) == "0"

# already contiguous
assert run(
"""1
5
.***.
"""
) == "0"

# symmetric gaps
assert run(
"""1
5
*.*.*
"""
) == "2"

# large gap
assert run(
"""1
9
*.......*
"""
) == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`.`|`0`| Không có cừu | 
|`*`|`0`| Cừu đơn | 
|`.***.`|`0`| Đã liền kề | 
|`*.*.*`|`2`| Xử lý trung bình với số cừu lẻ | 
|`*.......*`|`7`| Khoảng cách di chuyển lớn | 

## Vỏ cạnh 

### Không có cừu 

đầu vào:```
1
3
...
```Mảng vị trí trở thành:```
[]
```Thuật toán ngay lập tức phát hiện số lượng cừu nhiều nhất là một và trả về`0`. Không có tính toán trung bình được thử. 

### Đúng là một con cừu 

đầu vào:```
1
3
.*.
```Vị trí:```
[1]
```Một lần nữa, thuật toán trả về`0`. Một con cừu duy nhất đã tạo thành một khối liền kề. 

### Số cừu chẵn 

đầu vào:```
1
4
*..*
```Vị trí:```
[0, 3]
```Các giá trị được chuyển đổi:```
[0, 2]
```Việc thực hiện lựa chọn:```
median = 2
```Trị giá:```
|0-2| + |2-2| = 2
```Chọn giá trị trung bình khác`0`cũng sẽ tạo ra chi phí`2`. Hoặc trung vị là tối ưu, vì vậy việc chọn`q[k//2]`là đúng. 

### Cừu cách nhau những khoảng trống lớn 

đầu vào:```
1
9
*...*...*
```Vị trí:```
[0, 4, 8]
```Các giá trị được chuyển đổi:```
[0, 3, 6]
```Trung vị:```
3
```Trị giá:```
3 + 0 + 3 = 6
```Thuật toán tập hợp những con cừu xung quanh con cừu ở giữa một cách tự nhiên. Điều này tránh được sai lầm phổ biến là buộc tất cả cừu về một điểm cuối, điều này sẽ đòi hỏi nhiều bước di chuyển hơn.
