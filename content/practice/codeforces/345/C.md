---
title: "CF 345C - Đếm Thứ Sáu"
description: "Chúng tôi được cung cấp một danh sách nhỏ các ngày thi. Đối với mỗi ngày, chúng ta phải xác định xem ngày dương lịch đó có vừa là ngày 13 trong tháng vừa là thứ Sáu hay không. Câu trả lời chỉ đơn giản là số ngày thi thỏa mãn cả hai điều kiện."
date: "2026-06-07T15:45:28+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 345
codeforces_index: "C"
codeforces_contest_name: "Friday the 13th, Programmers Day"
rating: 2000
weight: 345
solve_time_s: 103
verified: true
draft: false
---

[CF 345C - Đếm thứ Sáu](https://codeforces.com/problemset/problem/345/C) 

**Xếp hạng:** 2000 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách nhỏ các ngày thi. Đối với mỗi ngày, chúng ta phải xác định xem ngày dương lịch đó có vừa là ngày 13 trong tháng vừa là thứ Sáu hay không. Câu trả lời chỉ đơn giản là số ngày thi thỏa mãn cả hai điều kiện. 

Phạm vi năm là từ 1974 đến 2030 và số ngày nhiều nhất là 10. Những ràng buộc này rất nhỏ. Ngay cả một thuật toán thực hiện một lượng lớn tính toán lịch cho mỗi truy vấn cũng sẽ nằm gọn trong giới hạn. Thách thức thực sự không phải là hiệu quả mà là việc xác định chính xác ngày trong tuần của một ngày nhất định, bao gồm cả năm nhuận và độ dài tháng khác nhau. 

Một lỗi phổ biến là chỉ kiểm tra xem số ngày có bằng 13 hay không. Hãy xem xét:```
1
2013-09-13
```Đầu ra đúng là:```
1
```vì ngày 13 tháng 9 năm 2013 là thứ Sáu. Bỏ qua ngày trong tuần sẽ được tính vào ngày 13 hàng tuần bất kể đó có phải là thứ Sáu hay không. 

Một sai lầm dễ mắc phải khác là xử lý sai năm nhuận. Coi như:```
1
2012-02-13
```Đầu ra đúng là:```
0
```Bất kỳ phép tính ngày trong tuần nào mà quên rằng năm 2012 là năm nhuận đều có thể tạo ra ngày trong tuần sai và tính sai ngày này. 

Trường hợp tinh vi thứ ba là trùng lặp ngày thi:```
3
2012-01-13
2012-01-13
2012-01-13
```Đầu ra đúng là:```
3
```Mỗi cuộc thi phải được tính độc lập. Tuyên bố rõ ràng cho phép nhiều cuộc thi trong cùng một ngày. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xác định ngày trong tuần của mỗi ngày nhất định và đếm xem có bao nhiêu ngày thứ Sáu xảy ra vào ngày 13 của tháng. 

Một cách mạnh mẽ để xác định các ngày trong tuần là chọn một ngày tham chiếu đã biết và mô phỏng từng ngày cho đến khi đạt được ngày mục tiêu. Điều này hiệu quả vì các ngày trong tuần lặp lại bảy ngày một lần. Nếu ngày tham chiếu cách đây hàng thập kỷ thì mỗi truy vấn có thể phải duyệt qua hàng chục nghìn ngày. Khoảng thời gian tối đa trong bài toán này là khoảng 56 năm, hay khoảng 20.000 ngày. Chỉ với mười truy vấn, điều này vẫn hoàn toàn có thể chấp nhận được, nhưng nó cồng kềnh một cách không cần thiết. 

Quan sát quan trọng là tính toán ngày trong tuần có các công thức dạng đóng nổi tiếng. Vì phạm vi ngày cố định và nhỏ nên chúng ta có thể tính trực tiếp ngày trong tuần của một ngày nhất định bằng cách sử dụng đồng đẳng Zeller. Công thức tự động xử lý độ dài năm nhuận và tháng thông qua số học, tránh mọi mô phỏng. 

Đối với mỗi ngày đầu vào, trước tiên chúng tôi kiểm tra xem thành phần ngày có phải là 13 hay không. Nếu không, ngày đó không thể là Thứ Sáu ngày 13. Nếu nó là 13, chúng ta tính ngày trong tuần bằng cách sử dụng đồng dư Zeller. Công thức trả về một mã đại diện cho ngày trong tuần và chúng tôi đếm ngày nếu mã tương ứng với thứ Sáu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · D) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Đây,$D$là số ngày giữa ngày tham chiếu và ngày được truy vấn. 

## Hướng dẫn thuật toán 

1. Đọc số ngày thi. 
2. Đối với mỗi chuỗi ngày, hãy trích xuất năm, tháng và ngày. 
3. Nếu ngày đó không phải là ngày 13 thì hãy bỏ qua ngày này ngay lập tức. 

Không có ngày nào có thể là Thứ Sáu ngày 13 trừ khi ngày trong tháng đó bằng 13. 
4. Tính ngày trong tuần bằng phương trình đồng dư Zeller. 

Đối với tháng 1 và tháng 2, hãy coi chúng như tháng 13 và 14 của năm trước. Đây là một phần của định nghĩa của công thức. 
5. Hãy để$K$là năm trong thế kỷ và$J$là thế kỷ dựa trên số không. 
6. Tính công thức Zeller:$$h = \left( q + \left\lfloor \frac{13(m+1)}{5} \right\rfloor + K + \left\lfloor \frac{K}{4} \right\rfloor + \left\lfloor \frac{J}{4} \right\rfloor + 5J \right) \bmod 7$$Ở đâu$q$là ngày trong tháng. 
7. Trong cách đánh số của Zeller, Thứ Sáu tương ứng với$h = 6$. 
8. Nếu$h = 6$, tăng câu trả lời. 
9. Sau khi xử lý tất cả các ngày, hãy in câu trả lời. 

### Tại sao nó hoạt động 

Sự đồng đẳng của Zeller là một công thức đã được chứng minh bằng toán học, ánh xạ mọi ngày theo lịch Gregorian hợp lệ thành ngày trong tuần của nó. Việc chuyển đổi tháng 1 và tháng 2 thành tháng 13 và 14 của năm trước được tích hợp vào đạo hàm và đảm bảo xử lý chính xác năm nhuận. 

Thuật toán chỉ tính một ngày khi có hai điều kiện đồng thời: ngày trong tháng bằng 13 và ngày trong tuần được tính toán bằng thứ Sáu. Vì tính toán ngày trong tuần là chính xác cho mọi ngày đầu vào hợp lệ nên mỗi ngày được tính đều là Thứ Sáu ngày 13 thực sự và mỗi Thứ Sáu ngày 13 trong dữ liệu đầu vào được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_friday_13(year, month, day):
    if day != 13:
        return False

    if month <= 2:
        month += 12
        year -= 1

    q = day
    K = year % 100
    J = year // 100

    h = (
        q
        + (13 * (month + 1)) // 5
        + K
        + K // 4
        + J // 4
        + 5 * J
    ) % 7

    return h == 6  # Friday in Zeller's congruence

def solve():
    n = int(input())
    ans = 0

    for _ in range(n):
        s = input().strip()
        y, m, d = map(int, s.split('-'))

        if is_friday_13(y, m, d):
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách tính toán ngày trong tuần thành một hàm trợ giúp. Kiểm tra đầu tiên là liệu ngày đó có bằng 13 hay không. Điều này tránh được việc tính toán không cần thiết cho những ngày không bao giờ có thể đóng góp vào câu trả lời. 

Việc điều chỉnh tháng cho tháng 1 và tháng 2 rất dễ bị bỏ qua. Sự đồng đẳng của Zeller coi chúng là tháng 13 và 14 của năm trước. Việc quên sự chuyển đổi này sẽ tạo ra các ngày trong tuần không chính xác vào thời điểm gần đầu năm. 

Tất cả số học đều phù hợp thoải mái bên trong các số nguyên Python tiêu chuẩn. Không cần xử lý đặc biệt đối với năm nhuận vì công thức đã kết hợp các quy tắc lịch Gregory. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
2012-01-13
2012-09-13
2012-11-20
2013-09-13
2013-09-20
```| Ngày | Ngày = 13? | Ngày trong tuần | Được tính? | 
| --- | --- | --- | --- | 
| 13-01-2012 | Có | Thứ sáu | Có | 
| 13-09-2012 | Có | Thứ năm | Không | 
| 20-11-2012 | Không | Chưa được kiểm tra | Không | 
| 13-09-2013 | Có | Thứ sáu | Có | 
| 20-09-2013 | Không | Chưa được kiểm tra | Không | 

Câu trả lời cuối cùng:```
2
```Dấu vết này cho thấy có ngày thứ 13 là chưa đủ. Ngày trong tuần cũng phải là thứ Sáu. 

### Ví dụ tùy chỉnh 

đầu vào:```
4
2015-02-13
2015-03-13
2015-03-20
2015-11-13
```| Ngày | Ngày = 13? | Ngày trong tuần | Được tính? | 
| --- | --- | --- | --- | 
| 13-02-2015 | Có | Thứ sáu | Có | 
| 13-03-2015 | Có | Thứ sáu | Có | 
| 20-03-2015 | Không | Chưa được kiểm tra | Không | 
| 13-11-2015 | Có | Thứ sáu | Có | 

Câu trả lời cuối cùng:```
3
```Ví dụ này chứng minh rằng nhiều ngày Thứ Sáu ngày 13 có thể xảy ra trong cùng một năm và tất cả phải được tính độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán ngày trong tuần theo thời gian không đổi cho mỗi ngày | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với tối đa mười ngày, thời gian chạy thực sự là tức thời. Việc sử dụng bộ nhớ là không đổi và thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def is_friday_13(year, month, day):
    if day != 13:
        return False

    if month <= 2:
        month += 12
        year -= 1

    q = day
    K = year % 100
    J = year // 100

    h = (
        q
        + (13 * (month + 1)) // 5
        + K
        + K // 4
        + J // 4
        + 5 * J
    ) % 7

    return h == 6

def solve():
    n = int(input())
    ans = 0

    for _ in range(n):
        y, m, d = map(int, input().strip().split('-'))
        if is_friday_13(y, m, d):
            ans += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""5
2012-01-13
2012-09-13
2012-11-20
2013-09-13
2013-09-20
"""
) == "2\n", "sample 1"

# minimum size input
assert run(
"""1
2012-01-13
"""
) == "1\n", "single Friday the 13th"

# duplicate dates
assert run(
"""3
2012-01-13
2012-01-13
2012-01-13
"""
) == "3\n", "duplicates counted separately"

# no date with day 13
assert run(
"""4
2012-01-12
2012-01-14
2012-09-20
2012-11-20
"""
) == "0\n", "day must equal 13"

# leap year related date
assert run(
"""1
2012-02-13
"""
) == "0\n", "correct leap-year weekday handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ngày độc thân 13-01-2012 | 1 | Kích thước đầu vào tối thiểu | 
| Ba bản 2012-01-13 | 3 | Các cuộc thi trùng lặp được tính độc lập | 
| Không có ngày nào có ngày 13 | 0 | Logic từ chối sớm | 
| 13-02-2012 | 0 | Xử lý năm nhuận và tính ngày trong tuần | 

## Vỏ cạnh 

### Ngày thi trùng lặp 

đầu vào:```
3
2012-01-13
2012-01-13
2012-01-13
```Thuật toán xử lý từng dòng một cách độc lập. Mỗi lần xuất hiện đều có ngày thứ 13 và tính đến thứ Sáu. Bộ đếm tăng ba lần, tạo ra:```
3
```Không có sự trùng lặp nào được thực hiện, phù hợp với yêu cầu của vấn đề. 

### Ngày nhuận trong năm nhuận 

đầu vào:```
1
2012-02-13
```Ngày bằng 13 nên thuật toán tính ngày trong tuần. Tháng 2 được chuyển đổi thành tháng 14 năm 2011 trước khi áp dụng công thức Zeller. Ngày trong tuần được tính là Thứ Hai, không phải Thứ Sáu, vì vậy câu trả lời là:```
0
```Điều này xác minh rằng các ngày nhạy cảm với năm nhuận được xử lý chính xác. 

### Những ngày là ngày 13 nhưng không phải thứ sáu 

đầu vào:```
1
2012-09-13
```Điều kiện ngày trong tháng thành công. Tính toán ngày trong tuần trở lại vào thứ Năm. Vì ngày trong tuần không phải là thứ Sáu nên bộ đếm vẫn bằng 0:```
0
```Điều này xác nhận rằng cả hai điều kiện đều được yêu cầu. 

### Những ngày là thứ Sáu nhưng không phải ngày 13 

đầu vào:```
1
2013-09-20
```Ngày trong tháng là 20 nên thuật toán ngay lập tức bỏ qua việc kiểm tra ngày trong tuần. Câu trả lời là:```
0
```Chỉ một ngày thứ sáu là không đủ. Ngày cụ thể phải là Thứ Sáu ngày 13.
