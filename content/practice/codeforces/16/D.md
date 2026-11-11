---
title: "CF 16D - Ghi nhật ký"
description: "Chúng tôi được cung cấp một chuỗi các mục nhật ký theo đúng thứ tự chúng được viết. Ban đầu mọi mục đều có cả ngày và giờ, nhưng ngày đã bị mất nên chỉ còn lại dấu thời gian 12 giờ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 16
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 16 (Div. 2 Only)"
rating: 1900
weight: 16
solve_time_s: 94
verified: true
draft: false
---
[CF 16D - Ghi nhật ký](https://codeforces.com/problemset/problem/16/D) 

**Xếp hạng:** 1900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các mục nhật ký theo đúng thứ tự chúng được viết. Ban đầu mọi mục đều có cả ngày và giờ, nhưng ngày đã bị mất nên chỉ còn lại dấu thời gian 12 giờ. 

Nhiệm vụ là tái tạo lại số ngày riêng biệt nhỏ nhất có thể tạo ra chuỗi đã cho trong khi vẫn giữ các mục theo thứ tự thời gian. 

Phần khó khăn là thứ tự thời gian mang tính toàn cục chứ không phải theo chuỗi dấu thời gian. Nếu một dòng nhật ký xuất hiện sau dòng khác thì thời điểm thực của nó phải lớn hơn hoặc bằng dòng trước đó. Cho phép sử dụng dấu thời gian bằng nhau vì nhiều tin nhắn có thể xảy ra trong cùng một phút và câu lệnh đảm bảo tối đa 10 mục nhập mỗi phút. 

Dấu thời gian sử dụng định dạng 12 giờ: 

-`12:00 a.m.`là nửa đêm. 
-`12:00 p.m.`là buổi trưa. 
-`01:00 p.m.`là 13 giờ trong 24 giờ. 

Những hạn chế là rất nhỏ. Chúng tôi có tối đa 100 dòng nhật ký, vì vậy ngay cả các giải pháp bậc hai cũng dễ dàng đủ nhanh. Khó khăn không phải là hiệu suất, mà là xử lý chính xác việc chuyển đổi thời gian và chuyển đổi ngày. 

Một sai lầm phổ biến là xử lý sai vai trò đặc biệt của giờ`12`. 

Ví dụ:```
[12:00 a.m.]
[01:00 a.m.]
```Nửa đêm phải trở thành`00:00`, không`12:00`. 

Một lỗi dễ mắc khác là cho rằng cần phải tăng thời gian một cách nghiêm ngặt. Thời gian bằng nhau là hợp lệ vì một số mục có thể xảy ra trong cùng một phút. 

Ví dụ:```
[05:00 a.m.]
[05:00 a.m.]
```Câu trả lời vẫn là`1`. 

Trường hợp phức tạp hơn xuất hiện khi đồng hồ quay ngược nhưng không vượt qua nửa đêm trong ký hiệu 24 giờ. 

Ví dụ:```
[01:13 p.m.]
[01:10 p.m.]
```Trong thời gian 24 giờ, đây là:```
13:13
13:10
```Lần nhập thứ hai không thể diễn ra trong cùng một ngày vì thời gian đã giảm. Chúng ta phải bắt đầu một ngày mới ở đây. 

Một trường hợp nguy hiểm khác là chuyển giờ giữa trưa. 

Ví dụ:```
[11:59 a.m.]
[12:00 p.m.]
```Điều này ngày càng tăng vì`11:59 -> 12:00`. 

Nhưng:```
[12:00 p.m.]
[11:59 a.m.]
```phải bắt đầu một ngày mới bởi vì`12:00 -> 11:59`giảm đi. 

Toàn bộ vấn đề rút gọn thành một câu hỏi: trong khi quét nhật ký theo thứ tự, chúng ta buộc phải chuyển sang ngày hôm sau bao nhiêu lần? 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ nhất là gán số ngày cho mỗi mục nhật ký và kiểm tra xem trình tự đầy đủ có theo trình tự thời gian hay không. Vì về mặt lý thuyết, mỗi mục có thể thuộc về nhiều ngày khác nhau nên việc tìm kiếm này giống như việc tìm kiếm trên nhiều bài tập. 

Nếu chúng ta thử mọi khả năng thì không gian trạng thái sẽ bùng nổ. Ngay cả khi chỉ có hai lựa chọn cho mỗi mục nhập, chúng tôi đã có được`2^100`, đó là điều vô vọng. 

Lý do khiến cho việc sử dụng bạo lực trở nên tự nhiên là vì vấn đề nói về việc xây dựng lại những ngày tháng bị thiếu. Có vẻ như chúng ta phải đoán ngày tháng một cách rõ ràng. Nhưng điều kiện thời gian có cấu trúc đơn giản hơn nhiều. 

Giả sử chúng ta chuyển đổi mọi dấu thời gian thành phút từ nửa đêm. Khi đó mỗi mục sẽ trở thành một số nguyên nằm giữa`0`Và`1439`. 

Bây giờ hãy xem xét hai mục liên tiếp: 

- Nếu thời gian hiện tại lớn hơn hoặc bằng thời gian trước đó thì chúng có thể thuộc cùng một ngày. 
- Nếu thời gian hiện tại nhỏ hơn thì ngày ở đây phải tăng lên. 

Không có tự do trong quyết định đó. Việc giảm thời gian buộc phải có một ngày mới và việc không giảm không bao giờ có lợi khi bắt đầu một ngày mới vì chúng ta muốn có số ngày tối thiểu. 

Quan sát này thu gọn vấn đề thành một lần quét tuyến tính duy nhất. 

Chúng tôi chuyển đổi tất cả các dấu thời gian thành phút 24 giờ, sau đó đếm số lần chuỗi giảm đi. Câu trả lời là:```
1 + number_of_decreases
```bởi vì mục nhật ký đầu tiên luôn bắt đầu vào ngày đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng nhật ký. 
2. Trích xuất dấu thời gian từ mỗi dòng. 

Dấu thời gian luôn xuất hiện bên trong dấu ngoặc vuông nên chúng ta chỉ cần chuỗi con trước`]`. 
3. Chuyển đổi định dạng 12 giờ thành phút 24 giờ. 

Vì`a.m.`: 

-`12:x a.m.`trở thành giờ`0`. 
- Các giờ khác không thay đổi. 

Vì`p.m.`: 

-`12:x p.m.`ở lại`12`. 
- Bất kỳ giờ nào khác sẽ được thêm vào`12`. 

Sau đó tính toán:```
total_minutes = hour * 60 + minute
```4. Khởi tạo câu trả lời dưới dạng`1`. 

Ngay cả một mục nhật ký cũng chiếm một ngày. 
5. Quét dấu thời gian liên tiếp. 

Nếu dấu thời gian hiện tại nhỏ hơn dấu thời gian trước đó, việc sắp xếp thứ tự thời gian trong cùng một ngày là không thể, vì vậy chúng ta phải bắt đầu một ngày mới. 

Tăng câu trả lời bằng`1`. 
6. In câu trả lời. 

### Tại sao nó hoạt động 

Thuật toán duy trì việc gán ngày nhỏ nhất có thể cho mỗi tiền tố của nhật ký. 

Bất cứ khi nào thời gian giảm đi, việc giữ nguyên ngày sẽ vi phạm trật tự thời gian ngay lập tức. Bắt đầu một ngày mới là điều bắt buộc. 

Bất cứ khi nào thời gian không giảm, việc giữ nguyên ngày luôn có giá trị và không bao giờ tệ hơn việc tạo ra một ngày mới. Vì chúng ta muốn số ngày tối thiểu nên việc thêm một ngày không cần thiết không bao giờ có thể hữu ích. 

Bởi vì mọi quá trình chuyển đổi đều được xử lý một cách tối ưu và độc lập nên số lượng cuối cùng là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_minutes(s):
    parts = s.split()

    hm = parts[0]
    period = parts[1]

    h, m = map(int, hm.split(':'))

    if period == 'a.m.':
        if h == 12:
            h = 0
    else:
        if h != 12:
            h += 12

    return h * 60 + m

def solve():
    n = int(input())

    times = []

    for _ in range(n):
        line = input().strip()

        end = line.index(']')
        time_str = line[1:end]

        times.append(to_minutes(time_str))

    days = 1

    for i in range(1, n):
        if times[i] < times[i - 1]:
            days += 1

    print(days)

solve()
```Phần quan trọng đầu tiên là việc chuyển đổi từ thời gian 12 giờ thành phút 24 giờ. Việc xử lý đặc biệt về giờ`12`là toàn bộ khó khăn ở đây. 

Cho nửa đêm:```
12:xx a.m. -> 00:xx
```Cho buổi trưa:```
12:xx p.m. -> 12:xx
```Mỗi người khác`p.m.`tăng giờ`12`. 

Trình phân tích cú pháp chỉ trích xuất chuỗi con bên trong dấu ngoặc. Chúng tôi hoàn toàn bỏ qua tin nhắn vì nó không ảnh hưởng gì đến niên đại. 

Bản thân quá trình quét rất đơn giản. Chúng tôi so sánh từng dấu thời gian với dấu thời gian trước đó. Nếu thời gian lùi lại, mục nhập hiện tại không thể thuộc về cùng một ngày, vì vậy chúng tôi tăng bộ đếm ngày. 

Việc so sánh phải chặt chẽ`<`, không`<=`. Dấu thời gian bằng nhau có giá trị trong cùng một ngày. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
[05:00 a.m.]: Server is started
[05:00 a.m.]: Rescan initialized
[01:13 p.m.]: Request processed
[01:10 p.m.]: Request processed
[11:40 p.m.]: Rescan completed
```Số lần chuyển đổi: 

| Nhập cảnh | Thời gian | Phút | So sánh | Ngày | 
| --- | --- | --- | --- | --- | 
| 1 | 05:00 sáng | 300 | bắt đầu | 1 | 
| 2 | 05:00 sáng | 300 | 300 >= 300 | 1 | 
| 3 | 01:13 chiều | 793 | 793 >= 300 | 1 | 
| 4 | 01:10 chiều | 790 | 790 < 793 | 2 | 
| 5 | 11:40 tối | 1420 | 1420 >= 790 | 2 | 

Câu trả lời cuối cùng:```
2
```Ví dụ này chứng minh tại sao các dấu thời gian bằng nhau không được tạo ra một ngày mới và tại sao một bước lùi lại buộc phải tạo ra một ngày mới. 

### Ví dụ tùy chỉnh 

đầu vào:```
4
[11:59 p.m.]: A
[12:00 a.m.]: B
[12:01 a.m.]: C
[11:00 p.m.]: D
```Số lần chuyển đổi: 

| Nhập cảnh | Thời gian | Phút | So sánh | Ngày | 
| --- | --- | --- | --- | --- | 
| 1 | 11:59 tối | 1439 | bắt đầu | 1 | 
| 2 | 12:00 sáng | 0 | 0 < 1439 | 2 | 
| 3 | 12:01 sáng | 1 | 1 >= 0 | 2 | 
| 4 | 11 giờ tối | 1380 | 1380 >= 1 | 2 | 

Câu trả lời cuối cùng:```
2
```Dấu vết này kiểm tra chuyển đổi lúc nửa đêm. Nếu như`12:00 a.m.`đã được chuyển đổi không chính xác thành`12:00`, thuật toán sẽ cho kết quả sai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một đường chuyền để phân tích cú pháp và một đường chuyền để so sánh | 
| Không gian | O(n) | Lưu trữ tất cả các dấu thời gian đã chuyển đổi | 

Với tối đa 100 mục nhật ký, thuật toán này thấp hơn nhiều so với giới hạn. Ngay cả một giải pháp bậc hai cũng có thể vượt qua một cách thoải mái, nhưng việc quét tuyến tính vừa sạch hơn vừa đơn giản hơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def to_minutes(s):
        parts = s.split()

        hm = parts[0]
        period = parts[1]

        h, m = map(int, hm.split(':'))

        if period == 'a.m.':
            if h == 12:
                h = 0
        else:
            if h != 12:
                h += 12

        return h * 60 + m

    n = int(input())

    times = []

    for _ in range(n):
        line = input().strip()
        end = line.index(']')
        time_str = line[1:end]
        times.append(to_minutes(time_str))

    ans = 1

    for i in range(1, n):
        if times[i] < times[i - 1]:
            ans += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run(
"""5
[05:00 a.m.]: Server is started
[05:00 a.m.]: Rescan initialized
[01:13 p.m.]: Request processed
[01:10 p.m.]: Request processed
[11:40 p.m.]: Rescan completed
"""
) == "2", "sample 1"

# minimum size
assert run(
"""1
[12:00 a.m.]: A
"""
) == "1", "single entry"

# equal timestamps
assert run(
"""3
[05:00 a.m.]: A
[05:00 a.m.]: B
[05:00 a.m.]: C
"""
) == "1", "equal times stay same day"

# midnight rollover
assert run(
"""2
[11:59 p.m.]: A
[12:00 a.m.]: B
"""
) == "2", "crossing midnight"

# noon handling
assert run(
"""2
[11:59 a.m.]: A
[12:00 p.m.]: B
"""
) == "1", "noon conversion"

# decreasing multiple times
assert run(
"""5
[10:00 a.m.]: A
[09:00 a.m.]: B
[08:00 a.m.]: C
[07:00 a.m.]: D
[06:00 a.m.]: E
"""
) == "5", "every step starts new day"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mục nhật ký đơn | 1 | Trường hợp cơ sở | 
| Tất cả các dấu thời gian bằng nhau | 1 | Thời gian bằng nhau được cho phép | 
| 11:59 tối -> 12 giờ sáng | 2 | Chuyển tiếp lúc nửa đêm | 
| 11:59 sáng -> 12:00 trưa | 1 | Chuyển đổi buổi trưa | 
| Giảm thời gian nghiêm ngặt | 5 | Mỗi bước lùi buộc một ngày mới | 

## Vỏ cạnh 

Xem xét các dấu thời gian lặp lại:```
3
[05:00 a.m.]: A
[05:00 a.m.]: B
[05:00 a.m.]: C
```Trình tự được chuyển đổi là:```
300, 300, 300
```Thuật toán chỉ bắt đầu một ngày mới khi giá trị hiện tại nhỏ hơn giá trị trước đó. Vì sự bình đẳng được cho phép nên câu trả lời vẫn là`1`. 

Bây giờ hãy xem xét việc xử lý lúc nửa đêm:```
2
[11:59 p.m.]: A
[12:00 a.m.]: B
```Việc chuyển đổi trở thành:```
1439, 0
```Từ`0 < 1439`, thuật toán bắt đầu một ngày mới và đưa ra kết quả`2`. 

Trường hợp này không thành công nếu nửa đêm được xử lý không chính xác thành giờ`12`. 

Một trường hợp tế nhị khác là buổi trưa:```
2
[11:59 a.m.]: A
[12:00 p.m.]: B
```Việc chuyển đổi trở thành:```
719, 720
```Trình tự tăng dần nên đáp án là`1`. 

Nếu như`12:00 p.m.`đã được chuyển đổi không chính xác thành`24:00`, logic sẽ bị phá vỡ. 

Cuối cùng, hãy xem xét việc buộc phải đặt lại nhiều lần:```
4
[03:00 p.m.]: A
[02:00 p.m.]: B
[01:00 p.m.]: C
[12:00 p.m.]: D
```Trình tự được chuyển đổi:```
900, 840, 780, 720
```Mỗi bước đi đều giảm đi, nên mỗi bước đi đều cần một ngày mới. Đầu ra của thuật toán`4`, đây là mức tối thiểu vì không có hai mục liên tiếp nào có thể chia sẻ một ngày.
