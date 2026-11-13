---
title: "CF 1744C - Đèn giao thông"
description: "Đèn giao thông tuân theo mô hình tuần hoàn được mô tả bằng một chuỗi. Nếu chuỗi là \"rggry\", thì sau khi đến cuối, nó sẽ bắt đầu lại từ đầu, tạo ra một chuỗi vô hạn."
date: "2026-06-09T15:50:29+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1744
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round  828 (Div. 3)"
rating: 1000
weight: 1744
solve_time_s: 102
verified: true
draft: false
---

[CF 1744C - Đèn giao thông](https://codeforces.com/problemset/problem/1744/C) 

**Đánh giá:** 1000 
**Tags:** tìm kiếm nhị phân, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đèn giao thông tuân theo mô hình tuần hoàn được mô tả bằng một chuỗi. Nếu chuỗi là`"rggry"`, rồi sau khi đến điểm cuối, nó lại bắt đầu lại từ đầu, tạo ra một chuỗi vô hạn. 

Chúng tôi biết màu hiện tại`c`, nhưng chúng tôi không biết sự xuất hiện của màu đó mà chúng tôi hiện đang xem xét trong chu kỳ. Vì chúng ta chỉ có thể vượt qua khi đèn chuyển sang màu xanh nên chúng ta phải xem xét mọi vị trí trong vòng tròn có màu`c`. 

Đối với mỗi vị trí như vậy, chúng ta có thể hỏi: phải trôi qua bao nhiêu giây cho đến vị trí tiếp theo?`'g'`xuất hiện? Vì thời điểm hiện tại thực tế có thể tương ứng với bất kỳ sự xuất hiện nào của`c`, chúng ta cần thời gian chờ đợi tồi tệ nhất trong số tất cả các vị trí đó. Thời gian chờ đợi tối đa đó chính là câu trả lời. 

Độ dài của chuỗi có thể lớn bằng`2 · 10^5`trên tất cả các trường hợp thử nghiệm kết hợp. Điều này ngay lập tức loại trừ các lần quét lồng nhau đắt tiền. MỘT`O(n²)`giải pháp sẽ yêu cầu khoảng`4 · 10^10`trong trường hợp xấu nhất vượt xa giới hạn. Cần có các giải pháp tuyến tính hoặc gần tuyến tính. 

Tính chất vòng tròn của đèn giao thông là nguyên nhân chính gây ra các lỗi nhỏ. Vị trí gần cuối dây có thể phải đợi đèn xanh xuất hiện sau khi quấn quanh đầu dây. 

Coi như:```
n = 3
c = r
s = "rrg"
```đầu tiên`'r'`đợi 2 giây cho màu xanh lá cây, trong khi giây thứ hai`'r'`đợi 1 giây. Câu trả lời là 2. Một giải pháp chỉ nhìn sang bên phải bên trong chuỗi gốc sẽ bỏ lỡ các trường hợp bao bọc trong các ví dụ khác một cách không chính xác. 

Một sai lầm dễ dàng khác xuất hiện khi`c = 'g'`.```
n = 1
c = g
s = "g"
```Chúng ta đã bật đèn xanh nên câu trả lời là 0. Bất kỳ logic tìm kiếm chung nào vẫn cố gắng tìm đèn xanh trong tương lai đều có thể trả về giá trị dương. 

Một ví dụ bao quanh thú vị hơn là:```
n = 5
c = y
s = "yrrgy"
```cuối cùng`'y'`ở vị trí 4. Màu xanh lá cây tiếp theo không nằm trong hậu tố còn lại. Chúng ta phải kết thúc ở đầu chu kỳ tiếp theo và đạt đến vùng xanh ở vị trí 3, cần 4 giây. Bỏ qua cấu trúc tuần hoàn sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp rất dễ mô tả. Đối với mọi vị trí chứa màu mục tiêu`c`, liên tục di chuyển về phía trước từng bước một qua chuỗi tuần hoàn cho đến khi tìm thấy đèn xanh. Hãy ghi lại thời gian chờ đợi đó và lấy mức tối đa. 

Điều này hiệu quả vì nó mô phỏng rõ ràng định nghĩa của vấn đề. Vấn đề là hiệu suất. Nếu có`O(n)`sự xuất hiện của`c`và mỗi người có thể quét`O(n)`vị trí trước khi bật đèn xanh, tổng độ phức tạp sẽ trở thành`O(n²)`. Với`n = 2 · 10^5`, như vậy là quá chậm. 

Quan sát quan trọng là mọi vị trí chỉ quan tâm đến đèn xanh gần nhất ở bên phải của nó trong chuỗi tuần hoàn vô hạn. Thay vì tìm kiếm riêng biệt với mọi lần xuất hiện của`c`, chúng ta có thể xử lý trước chuỗi để dễ dàng thu được các khoảng cách xanh gần nhất này. 

Một mẹo thuận tiện cho chuỗi tròn là sao chép. Nếu chúng ta nối chuỗi với chính nó, mọi đường dẫn bao quanh trong chu trình ban đầu sẽ trở thành một đường dẫn sang phải thông thường trong chuỗi nhân đôi. 

Ví dụ:```
s      = yrrgy
s + s  = yrrgyyrrgy
```Giờ đây, mọi vị trí trong bản sao đầu tiên có thể tìm kiếm an toàn ở bên phải để tìm đèn xanh tiếp theo mà không cần lo lắng về việc bị gói. 

Chúng ta quét chuỗi nhân đôi từ phải sang trái trong khi ghi nhớ vị trí gần đây nhất chứa`'g'`. Với mỗi ký tự, chúng ta biết ngay khoảng cách đến đèn xanh tiếp theo. Bất cứ khi nào ký tự bằng`c`, chúng tôi cập nhật câu trả lời với khoảng cách đó. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`c`là`'g'`, đầu ra`0`ngay lập tức vì có thể vượt qua ngay bây giờ. 
2. Tạo chuỗi nhân đôi`t = s + s`. Điều này biến chuyển động tròn thành chuyển động bình thường về bên phải. 
3. Quét`t`từ phải sang trái trong khi vẫn giữ nguyên vị trí đèn xanh gần nhất được nhìn thấy cho đến nay. 
4. Bất cứ khi nào ký tự hiện tại được`'g'`, cập nhật vị trí màu xanh lá cây được lưu trữ vào chỉ mục hiện tại. 
5. Bất cứ khi nào ký tự hiện tại bằng`c`, tính khoảng cách từ vị trí này đến vị trí màu xanh lá cây được lưu trữ. 
6. Chỉ các vị trí thuộc bản sao đầu tiên của chuỗi mới quan trọng. Câu trả lời là khoảng cách tối đa giữa tất cả các vị trí như vậy. 
7. Xuất ra khoảng cách tối đa đó. 

### Tại sao nó hoạt động 

Sau khi nhân đôi chuỗi, mọi đường dẫn từ một vị trí trong chu kỳ ban đầu đến đèn xanh tiếp theo của nó xuất hiện hoàn toàn bên trong chuỗi nhân đôi dưới dạng một đoạn đơn giản sang phải. Khi quét từ phải sang trái, vị trí màu xanh được lưu trữ luôn là đèn xanh gần nhất tại hoặc sau chỉ số hiện tại. Vì vậy, với mỗi lần xuất hiện của`c`, khoảng cách được tính toán chính xác là thời gian chờ đợi cho đến khi có đèn xanh tiếp theo. Lấy giá trị lớn nhất trong tất cả các lần xuất hiện của`c`phù hợp với yêu cầu rằng chúng ta phải chuẩn bị cho tình huống tồi tệ nhất có thể xảy ra hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, c = input().split()
    n = int(n)
    s = input().strip()

    if c == 'g':
        print(0)
        continue

    doubled = s + s

    next_green = -1
    ans = 0

    for i in range(2 * n - 1, -1, -1):
        if doubled[i] == 'g':
            next_green = i

        if i < n and doubled[i] == c:
            ans = max(ans, next_green - i)

    print(ans)
```Xử lý trường hợp đặc biệt đầu tiên`c = 'g'`. Vì đèn hiện tại đã xanh nên không cần phải chờ đợi. 

Chuỗi sau đó được nhân đôi. Bất kỳ sự chờ đợi nào thường bao quanh phần cuối của chu kỳ giờ đây được biểu thị dưới dạng một bước di chuyển bình thường sang bên phải bên trong chuỗi nhân đôi. 

Quá trình quét ngược vẫn duy trì`next_green`, vị trí màu xanh lá cây gần nhất bên phải của chỉ mục hiện tại. Vì chúng tôi xử lý các chỉ mục từ phải sang trái nên giá trị này luôn có sẵn khi cần. 

điều kiện`i < n`là quan trọng. Chúng tôi chỉ muốn xuất hiện từ chuỗi gốc. Bản sao thứ hai tồn tại hoàn toàn để làm cho khoảng cách bao quanh dễ dàng tính toán. 

Một lỗi thường gặp là cập nhật câu trả lời cho các lần xuất hiện trong cả hai bản sao. Làm như vậy sẽ tính các trạng thái không tương ứng với vị trí bắt đầu thực tế trong chu kỳ ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
c = r
s = rggry
```Chuỗi nhân đôi là:```
rggryrggry
```| tôi | char | next_green sau khi cập nhật | được xem xét? | khoảng cách | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 9 | y | -1 | Không | - | 0 | 
| 8 | r | -1 | Không | - | 0 | 
| 7 | g | 7 | Không | - | 0 | 
| 6 | g | 6 | Không | - | 0 | 
| 5 | r | 6 | Không | - | 0 | 
| 4 | y | 6 | Không | - | 0 | 
| 3 | r | 6 | Có | 3 | 3 | 
| 2 | g | 2 | Không | - | 3 | 
| 1 | g | 1 | Không | - | 3 | 
| 0 | r | 1 | Có | 1 | 3 | 

Câu trả lời là 3. 

Ví dụ này cho thấy tại sao chúng ta cần thời gian chờ đợi tối đa. Sự xuất hiện khác nhau của`'r'`tạo ra các thời gian chờ khác nhau và chúng tôi phải đảm bảo thành công trong trường hợp xấu nhất. 

### Ví dụ 2 

đầu vào:```
n = 5
c = y
s = yrrgy
```Chuỗi nhân đôi là:```
yrrgyyrrgy
```| tôi | char | next_green sau khi cập nhật | được xem xét? | khoảng cách | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 9 | y | -1 | Không | - | 0 | 
| 8 | g | 8 | Không | - | 0 | 
| 7 | r | 8 | Không | - | 0 | 
| 6 | r | 8 | Không | - | 0 | 
| 5 | y | 8 | Không | - | 0 | 
| 4 | y | 8 | Có | 4 | 4 | 
| 3 | g | 3 | Không | - | 4 | 
| 2 | r | 3 | Không | - | 4 | 
| 1 | r | 3 | Không | - | 4 | 
| 0 | y | 3 | Có | 3 | 4 | 

Câu trả lời là 4. 

Sự xuất hiện ở vị trí 4 thể hiện hành vi bao quanh. Đèn xanh tiếp theo của nó xuất hiện ở bản sao thứ hai của chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét ngược một lần trên chuỗi nhân đôi | 
| Không gian | O(n) | Chuỗi nhân đôi có độ dài 2n | 

Tổng của tất cả độ dài chuỗi trong các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Quét tuyến tính trên mỗi chuỗi nhân đôi chỉ thực hiện một lượng công việc không đổi cho mỗi ký tự, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())

    out = []

    for _ in range(t):
        n, c = input().split()
        n = int(n)
        s = input().strip()

        if c == 'g':
            out.append("0")
            continue

        doubled = s + s
        next_green = -1
        ans = 0

        for i in range(2 * n - 1, -1, -1):
            if doubled[i] == 'g':
                next_green = i

            if i < n and doubled[i] == c:
                ans = max(ans, next_green - i)

        out.append(str(ans))

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue()

# provided sample
assert run("""6
5 r
rggry
1 g
g
3 r
rrg
5 y
yrrgy
7 r
rgrgyrg
9 y
rrrgyyygy
""") == """3
0
2
4
1
4
"""

# minimum size
assert run("""1
1 g
g
""") == """0
"""

# wrap-around case
assert run("""1
5 y
yrrgy
""") == """4
"""

# all red except one green
assert run("""1
6 r
rrrrrg
""") == """5
"""

# green immediately after every red
assert run("""1
6 r
rgrgrg
""") == """1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, s="g"`|`0`| Kích thước tối thiểu và`c='g'`| 
|`yrrgy`với`c='y'`|`4`| Xử lý toàn diện | 
|`rrrrrg`với`c='r'`|`5`| Chờ đợi lâu nhất có thể trong một chu kỳ | 
|`rgrgrg`với`c='r'`|`1`| Chuyển đổi xanh tiếp theo ngay lập tức | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 g
g
```Vì màu hiện tại đã là màu xanh nên câu trả lời phải là`0`. Thuật toán xử lý việc này trước khi quá trình quét bắt đầu. Nếu không có trường hợp đặc biệt này, một giải pháp có thể tìm kiếm đèn xanh trong tương lai một cách không cần thiết và tạo ra giá trị khác 0. 

Coi như:```
1
5 y
yrrgy
```Sự xuất hiện của`'y'`ở chỉ số 4 không có đèn xanh ở bên phải trong chuỗi gốc. Sau khi nhân đôi, chuỗi trở thành`yrrgyyrrgy`và màu xanh lục tiếp theo xuất hiện ở chỉ số 8. Khoảng cách là`8 - 4 = 4`, được tính chính xác. Đây chính xác là tình huống làm gián đoạn quá trình triển khai mà quên mất chu trình bao quanh. 

Coi như:```
1
6 r
rrrrrg
```Mọi vị trí màu đỏ đều chờ một màu xanh lá cây ở cuối. Khoảng cách là`5, 4, 3, 2, 1`. Tối đa là`5`. Trong quá trình quét ngược,`next_green`vẫn cố định ở vị trí màu xanh lá cây, do đó mỗi màu đỏ ngay lập tức có được khoảng cách chính xác. 

Coi như:```
1
3 r
rrg
```Màu đỏ đầu tiên đợi 2 giây và màu đỏ thứ hai đợi 1 giây. Thuật toán tính toán cả hai khoảng cách và giữ mức tối đa. Điều này xác minh rằng chúng tôi đang giải quyết vấn đề đảm bảo trong trường hợp xấu nhất thay vì tìm thời gian chờ đợi ngắn nhất trong số các lần xảy ra.
