---
title: "CF 38A - Quân Đội"
description: "Vasya hiện đang giữ hạng A trong quân đội và mong muốn cuối cùng đạt được hạng B. Việc chuyển từ hạng i lên hạng i+1 cần một số năm cố định, được lưu trong mảng d."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 38
codeforces_index: "A"
codeforces_contest_name: "School Personal Contest #1 (Winter Computer School 2010/11) - Codeforces Beta Round 38 (ACM-ICPC Rules)"
rating: 800
weight: 38
solve_time_s: 78
verified: true
draft: false
---
[CF 38A - Quân đội](https://codeforces.com/problemset/problem/38/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vasya hiện đang giữ thứ hạng`a`trong quân đội và muốn cuối cùng đạt được cấp bậc`b`. Chuyển từ cấp bậc`i`xếp hạng`i + 1`yêu cầu một số năm cố định, được lưu trữ trong mảng`d`. 

Mảng có độ dài`n - 1`bởi vì có chính xác`n - 1`chuyển đổi giữa các cấp bậc liên tiếp. Ví dụ,`d[0]`đại diện cho số năm cần thiết để chuyển từ thứ hạng`1`xếp hạng`2`, Và`d[1]`đại diện cho số năm cần thiết để chuyển từ thứ hạng`2`xếp hạng`3`. 

Để đạt được thứ hạng`b`, Vasya phải vượt qua mọi cấp bậc trung cấp theo thứ tự. Điều đó có nghĩa là tổng thời gian chỉ đơn giản là tổng của tất cả các chi phí chuyển tiếp giữa`a`Và`b`. 

Những hạn chế là rất nhỏ.`n`nhiều nhất là`100`, do đó, ngay cả một vòng lặp đơn giản trên mảng cũng đủ nhanh. Một giải pháp bậc hai cũng có thể vượt qua một cách dễ dàng, vì trong trường hợp xấu nhất, nó sẽ hoạt động xung quanh`10,000`hoạt động. Vấn đề này tập trung vào việc lập chỉ mục cẩn thận hơn là tối ưu hóa. 

Nguồn gốc của lỗi chính là việc lập chỉ mục sai lệch vì thứ hạng được đánh số bắt đầu từ`1`, trong khi các mảng trong Python được lập chỉ mục từ`0`. 

Hãy xem xét ví dụ này:```
4
1 2 3
2 4
```Câu trả lời đúng là:```
5
```Chúng tôi cần thời gian từ thứ hạng`2`ĐẾN`3`, đó là`2`, và từ`3`ĐẾN`4`, đó là`3`. Tổng của họ là`5`. 

Việc triển khai bất cẩn có thể tính tổng không chính xác từ chỉ mục`2`bởi vì`4`, hoặc bao gồm các ranh giới sai. 

Một sai lầm dễ mắc phải khác là thêm vào một đoạn bổ sung. 

Ví dụ:```
5
10 20 30 40
1 3
```Đầu ra đúng:```
30
```Các chuyển đổi cần thiết là`1 -> 2`Và`2 -> 3`, vậy câu trả lời là`10 + 20 = 30`. 

Nếu ai đó vô tình tính tổng thông qua chỉ số`3`, họ sẽ bao gồm không chính xác quá trình chuyển đổi`3 -> 4`và nhận được`60`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng lộ trình thăng tiến theo cấp bậc. Bắt đầu từ cấp bậc`a`, chúng tôi liên tục cộng thêm số năm cần thiết để chuyển sang cấp bậc tiếp theo cho đến khi chúng tôi đạt được`b`. 

Phương pháp bạo lực này đã đủ hiệu quả vì có nhiều nhất`99`tổng số chuyển tiếp. Trong trường hợp xấu nhất, chúng ta tính tổng toàn bộ mảng một lần, chỉ`O(n)`hoạt động. 

Lý do điều này có hiệu quả là vì cơ cấu thăng tiến hoàn toàn tuyến tính. Chỉ có một con đường có thể từ cấp bậc`a`xếp hạng`b`. Không cần phân nhánh, logic đường dẫn ngắn nhất hoặc lập trình động. Mọi hành trình hợp lệ đều phải trải qua những lần chuyển tiếp liên tiếp giống nhau. 

Một giải pháp phức tạp hơn sẽ bổ sung thêm chi phí không cần thiết. Quan sát quan trọng là câu trả lời chính xác là tổng của một mảng con liền kề của`d`. 

Cụ thể là chuyển từ cấp bậc`a`xếp hạng`b`, chúng ta cần:```
d[a-1] + d[a] + ... + d[b-2]
```Khi chúng tôi nhận ra rằng mỗi phần tử mảng đại diện cho một cạnh giữa các cấp liên tiếp, việc triển khai sẽ trở thành một tổng phạm vi đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Trong bài toán này, các phương pháp tiếp cận tối ưu và phương pháp cưỡng bức thực sự giống nhau vì các ràng buộc rất nhỏ và cấu trúc là tuyến tính. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số bậc. 
2. Đọc mảng`d`chiều dài`n - 1`. 

Mỗi phần tử đại diện cho số năm cần thiết để chuyển từ cấp bậc này sang cấp bậc liên tiếp tiếp theo. 
3. Đọc số nguyên`a`Và`b`. 

Vasya hiện có thứ hạng`a`và muốn đạt thứ hạng`b`. 
4. Khởi tạo`answer = 0`. 

Biến này tích lũy tổng số năm cần thiết. 
5. Lặp lại các chỉ số từ`a - 1`lên đến`b - 2`. 

Đây chính xác là những quá trình chuyển đổi mà Vasya phải hoàn thành. 
6. Cộng từng giá trị tương ứng từ`d`vào trong`answer`. 
7. In`answer`. 

### Tại sao nó hoạt động 

Mỗi phần tử`d[i]`tương ứng với sự chuyển đổi từ cấp bậc`i + 1`xếp hạng`i + 2`. Đi từ cấp bậc`a`xếp hạng`b`, Vasya phải hoàn thành mọi quá trình chuyển đổi liên tiếp ở giữa. 

Thuật toán tính tổng chính xác những chuyển đổi cần thiết đó và không có gì khác. Vì chỉ có một đường đi duy nhất có thể đi qua các cấp nên tổng này được xác định duy nhất và luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
d = list(map(int, input().split()))
a, b = map(int, input().split())

answer = 0

for i in range(a - 1, b - 1):
    answer += d[i]

print(answer)
```Hai dòng đầu tiên ghi số cấp bậc và chi phí chuyển đổi giữa các cấp bậc liên tiếp. 

Chi tiết quan trọng là phạm vi vòng lặp:```
range(a - 1, b - 1)
```Giả định`a = 2`Và`b = 4`. Chúng ta cần sự chuyển tiếp:```
2 -> 3
3 -> 4
```Chúng tương ứng với các chỉ số mảng:```
1 and 2
```Trong Python,`range(1, 3)`tạo ra chính xác các chỉ số đó. 

Vòng lặp dừng trước`b - 1`, điều này ngăn chặn việc vô tình bao gồm quá trình chuyển đổi tiếp theo sau khi đạt thứ hạng`b`. 

Giải pháp sử dụng bộ nhớ bổ sung không đổi vì nó chỉ lưu trữ mảng đầu vào và một biến tích lũy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
5 6
1 2
```| Bước | Chỉ mục hiện tại`i`| Giá trị gia tăng |`answer`| 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | 0 | 
| 1 | 0 | 5 | 5 | 

Đầu ra:```
5
```Dấu vết này cho thấy trường hợp không tầm thường đơn giản nhất. Chuyển từ cấp bậc`1`xếp hạng`2`chỉ cần một lần chuyển đổi, vì vậy câu trả lời là chính xác`d[0]`. 

### Ví dụ 2 

đầu vào:```
5
10 20 30 40
2 5
```| Bước | Chỉ mục hiện tại`i`| Giá trị gia tăng |`answer`| 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | 0 | 
| 1 | 1 | 20 | 20 | 
| 2 | 2 | 30 | 50 | 
| 3 | 3 | 40 | 90 | 

Đầu ra:```
90
```Ví dụ này chứng minh rằng thuật toán tính tổng mọi chuyển đổi liên tiếp được yêu cầu. Các chỉ số khớp với đường dẫn:```
2 -> 3 -> 4 -> 5
```Dấu vết cũng xác nhận rằng ranh giới vòng lặp là chính xác vì chỉ bao gồm các phân đoạn cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Nhiều nhất`n - 1`quá trình chuyển đổi được tóm tắt | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng ngoài mảng đầu vào | 

Với`n <= 100`, giải pháp này dễ dàng nằm trong giới hạn. Ngay cả cách tiếp cận chậm hơn nhiều cũng có thể vượt qua, nhưng quá trình quét tuyến tính vừa đơn giản vừa rõ ràng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    d = list(map(int, input().split()))
    a, b = map(int, input().split())

    ans = 0

    for i in range(a - 1, b - 1):
        ans += d[i]

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("3\n5 6\n1 2\n") == "5\n", "sample 1"

# minimum size input
assert run("2\n7\n1 2\n") == "7\n", "minimum case"

# all equal values
assert run("5\n3 3 3 3\n1 5\n") == "12\n", "all equal"

# off-by-one check
assert run("5\n10 20 30 40\n1 3\n") == "30\n", "boundary correctness"

# larger path
assert run("6\n1 2 3 4 5\n2 6\n") == "14\n", "multiple transitions"

print("All tests passed!")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 7 / 1 2`|`7`| Kích thước đầu vào hợp lệ tối thiểu | 
|`5 / 3 3 3 3 / 1 5`|`12`| Tổng hợp nhiều giá trị bằng nhau | 
|`5 / 10 20 30 40 / 1 3`|`30`| Phát hiện từng lỗi một | 
|`6 / 1 2 3 4 5 / 2 6`|`14`| Xử lý đúng phạm vi dài hơn | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
2
7
1 2
```Chỉ có một quá trình chuyển đổi trong toàn bộ hệ thống. Thuật toán chạy vòng lặp một lần với chỉ số`0`và thêm vào`7`. Đầu ra trở thành:```
7
```Điều này xác nhận vòng lặp xử lý các ranh giới tối thiểu một cách chính xác. 

Bây giờ hãy xem xét trường hợp đích đến cách xa vài bậc:```
5
10 20 30 40
1 5
```Vòng lặp lặp qua các chỉ số`0`,`1`,`2`, Và`3`. 

Tổng số chạy trở thành:```
10
30
60
100
```Câu trả lời cuối cùng là:```
100
```Điều này xác minh rằng mọi chuyển đổi bắt buộc đều được bao gồm chính xác một lần. 

Cuối cùng, hãy xem xét bẫy từng cái một cổ điển:```
5
10 20 30 40
1 3
```Thuật toán chỉ lặp qua các chỉ số`0`Và`1`. 

Tổng trở thành:```
10 + 20 = 30
```Sự chuyển tiếp`3 -> 4`không được bao gồm vì vòng lặp dừng trước chỉ mục`2`. Điều này xác nhận ranh giới bên phải được xử lý chính xác.
