---
title: "CF 105760C - Sự cố lò vi sóng"
description: "Donald nhập thời gian vào lò vi sóng ở định dạng MM:SS, mong đợi nó có nghĩa là phút và giây. Thật không may, lò vi sóng diễn giải các chữ số giống hệt như HH:MM, nghĩa là giờ và phút."
date: "2026-06-25T06:14:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105760
codeforces_index: "C"
codeforces_contest_name: "2020 UCF Local Programming Contest"
rating: 0
weight: 105760
solve_time_s: 41
verified: true
draft: false
---

[CF 105760C - Sự cố lò vi sóng](https://codeforces.com/problemset/problem/105760/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Donald nhập thời gian vào lò vi sóng ở định dạng`MM:SS`, mong đợi nó có nghĩa là phút và giây. Thật không may, lò vi sóng diễn giải các chữ số giống hệt như`HH:MM`, nghĩa là giờ và phút. 

Ví dụ: nếu Donald vào`02:15`, anh ấy mong đợi lò vi sóng sẽ chạy trong 2 phút 15 giây. Thay vào đó, lò vi sóng chạy trong 2 giờ 15 phút. 

Chúng ta phải tính toán thời gian nấu thêm do sự hiểu lầm này gây ra. Câu trả lời phải được in dưới dạng`HH:MM:SS`. 

Đầu vào bao gồm một chuỗi duy nhất`MM:SS`. Cả hai phần đều nằm trong khoảng từ 0 đến 59 và ít nhất một trong số chúng khác 0. Đầu ra là sự khác biệt giữa: 

1. Thời gian thực tế được nhập, được hiểu là giờ và phút. 
2. Thời gian Donald dự định, được hiểu là phút và giây. 

Vì đầu vào chỉ chứa một dấu thời gian và phạm vi rất nhỏ nên hiệu quả không liên quan ở đây. Bất kỳ giải pháp số học thời gian không đổi đều đủ nhanh. Thách thức thực sự là chuyển đổi chính xác giữa các đơn vị thời gian khác nhau và định dạng kết quả. 

Một lỗi phổ biến là trừ các trường một cách độc lập. Coi như:```
00:10
```Donald mong đợi 10 giây. 

Lò vi sóng diễn giải nó là 10 phút. 

Sự khác biệt là:```
600 - 10 = 590 seconds
```bằng:```
00:09:50
```Nếu chúng ta trừ phút và giây riêng biệt mà không chuyển đổi mọi thứ thành giây trước, chúng ta có thể dễ dàng xử lý sai khoản vay cần thiết. 

Một cạm bẫy dễ xảy ra khác khi số giây khác 0. 

đầu vào:```
05:00
```Thời gian dự kiến:```
5 minutes = 300 seconds
```Thời gian thực tế:```
5 hours = 18000 seconds
```Sự khác biệt:```
17700 seconds = 04:55:00
```Một giải pháp chỉ so sánh các con số được hiển thị có thể bỏ lỡ rằng giờ và phút tương ứng với các thang đơn vị hoàn toàn khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là chuyển đổi cả hai cách diễn giải thành giây. 

Giả sử đầu vào là`MM:SS`. 

Thời gian dự định của Donald là:```
MM minutes + SS seconds
```bằng:```
MM * 60 + SS
```giây. 

Thời gian thực tế của lò vi sóng là:```
MM hours + SS minutes
```bằng:```
MM * 3600 + SS * 60
```giây. 

Câu trả lời bắt buộc chỉ đơn giản là sự khác biệt giữa hai giá trị này. 

Một cách tiếp cận phức tạp hơn là thực hiện phép trừ thời gian theo cách thủ công bằng cách sử dụng giờ, phút và giây khi mượn. Cách đó có hiệu quả, nhưng nó gây ra sự phức tạp không cần thiết và tạo cơ hội cho những sai sót nhỏ xảy ra trong quá trình hoạt động vay. 

Quan sát quan trọng là số học về thời gian trở nên tầm thường một khi mọi thứ được biểu diễn bằng một đơn vị duy nhất. Sau khi tính toán sự khác biệt tính bằng giây, chúng tôi chuyển đổi nó thành giờ, phút và giây bằng cách sử dụng các phép chia và modulo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trừ thời gian thủ công bằng khoản vay | O(1) | O(1) | Đã chấp nhận | 
| Chuyển đổi sang giây và trừ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào và phân tách nó ở dấu hai chấm. 
2. Phân tích hai giá trị dưới dạng số nguyên`m`Và`s`. 
3. Tính khoảng thời gian Donald dự định:```
intended = m * 60 + s
```4. Tính thời lượng lò vi sóng thực tế sử dụng:```
actual = m * 3600 + s * 60
```5. Tính thời gian cộng thêm:```
diff = actual - intended
```6. Chuyển đổi`diff`trở lại giờ, phút và giây.```
hours = diff // 3600
diff %= 3600

minutes = diff // 60
seconds = diff % 60
```7. In kết quả bằng hai chữ số cho mỗi trường. 

### Tại sao nó hoạt động 

Cả hai cách giải thích đều mô tả cùng một cặp số nhưng gán các đơn vị khác nhau cho chúng. 

Khoảng thời gian dự định được đo bằng phút và giây, trong khi lò vi sóng coi chúng là giờ và phút. Việc chuyển đổi cả hai khoảng thời gian thành tổng số giây sẽ loại bỏ mọi sự mơ hồ và cho phép thực hiện phép trừ đơn giản. 

Vì mỗi khoảng thời gian được biểu thị chính xác bằng giây nên chênh lệch được tính toán giống hệt về mặt toán học với thời gian nấu thêm. Việc chuyển đổi sự khác biệt đó thành giờ, phút và giây chỉ đơn giản là phân tách tổng số giây thành đơn vị thời gian tiêu chuẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

m, s = map(int, input().strip().split(':'))

intended = m * 60 + s
actual = m * 3600 + s * 60

diff = actual - intended

hours = diff // 3600
diff %= 3600

minutes = diff // 60
seconds = diff % 60

print(f"{hours:02d}:{minutes:02d}:{seconds:02d}")
```Bước đầu tiên phân tích hai thành phần của đầu vào. 

Giải pháp sau đó tính toán hai khoảng thời gian độc lập. Một cái tương ứng với những gì Donald muốn, được tính bằng phút và giây. Cái còn lại tương ứng với cách lò vi sóng diễn giải các chữ số tương tự, được đo bằng giờ và phút. 

Sau khi trừ đi thời lượng, giá trị còn lại là số giây. Việc chuyển đổi trở lại`HH:MM:SS`tuân theo quy trình tiêu chuẩn: trích xuất số giờ trước, sau đó là số phút từ phần còn lại, sau đó là số giây từ phần còn lại sau đó. 

Chuỗi định dạng`:02d`rất quan trọng vì bài toán yêu cầu chính xác hai chữ số trong mọi trường. Không có nó, các giá trị như`4:5:0`sẽ được in thay vì`04:05:00`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
05:00
```| Biến | Giá trị | 
| --- | --- | 
| m | 5 | 
| s | 0 | 
| dự định | 300 | 
| thực tế | 18000 | 
| khác biệt | 17700 | 

Chuyển đổi: 

| Bước | Giá trị | 
| --- | --- | 
| giờ | 4 | 
| giây còn lại | 3300 | 
| phút | 55 | 
| giây | 0 | 

Đầu ra:```
04:55:00
```Ví dụ này cho thấy rằng mặc dù các chữ số được hiển thị trông giống nhau nhưng việc diễn giải`05`vì giờ thay vì phút tạo ra sự khác biệt rất lớn. 

### Ví dụ 2 

đầu vào:```
13:37
```| Biến | Giá trị | 
| --- | --- | 
| m | 13 | 
| s | 37 | 
| dự định | 817 | 
| thực tế | 49020 | 
| khác biệt | 48203 | 

Chuyển đổi: 

| Bước | Giá trị | 
| --- | --- | 
| giờ | 13 | 
| giây còn lại | 1403 | 
| phút | 23 | 
| giây | 23 | 

Đầu ra:```
13:23:23
```Ví dụ này chứng minh rằng phép trừ phải được thực hiện theo giây chứ không phải theo từng trường vì kết quả cuối cùng chứa các giá trị khác 0 trong cả ba thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Sử dụng một lượng bộ nhớ không đổi | 

Chương trình xử lý một dấu thời gian duy nhất và thực hiện một vài phép tính số nguyên. Thời gian chạy và mức sử dụng bộ nhớ của nó thực sự tức thời so với giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    m, s = map(int, input().strip().split(':'))

    intended = m * 60 + s
    actual = m * 3600 + s * 60

    diff = actual - intended

    h = diff // 3600
    diff %= 3600
    mm = diff // 60
    ss = diff % 60

    return f"{h:02d}:{mm:02d}:{ss:02d}"

# provided samples
assert run("05:00\n") == "04:55:00"
assert run("13:37\n") == "13:23:23"
assert run("00:10\n") == "00:09:50"

# minimum positive duration
assert run("00:01\n") == "00:00:59"

# exactly one minute intended, one hour actual
assert run("01:00\n") == "00:59:00"

# largest values
assert run("59:59\n") == "58:59:01"

# only seconds component present
assert run("00:59\n") == "00:58:01"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`00:01`|`00:00:59`| Đầu vào tích cực nhỏ nhất | 
|`01:00`|`00:59:00`| Chuyển đổi giờ-phút chính xác | 
|`59:59`|`58:59:01`| Giá trị pháp lý lớn nhất | 
|`00:59`|`00:58:01`| Vay xuyên ranh giới từng phút | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
00:10
```Thời lượng dự định là:```
10 seconds
```Thời lượng thực tế là:```
10 minutes = 600 seconds
```Thuật toán tính toán:```
600 - 10 = 590
```giây. 

Chuyển đổi ngược lại 590 giây mang lại:```
00:09:50
```Kết quả đúng vì tất cả số học được thực hiện trong vài giây trước khi định dạng. 

Hãy xem xét đầu vào:```
05:00
```Thời lượng dự định là:```
300 seconds
```Thời lượng thực tế là:```
18000 seconds
```Sự khác biệt là:```
17700 seconds
```phân hủy thành:```
4 hours
55 minutes
0 seconds
```mang lại:```
04:55:00
```Không cần xử lý đặc biệt đối với 0 giây vì việc chuyển đổi từ tổng số giây sẽ tạo ra các thành phần chính xác một cách tự nhiên. 

Hãy xem xét đầu vào pháp lý lớn nhất:```
59:59
```Thuật toán tính toán:```
actual = 59 * 3600 + 59 * 60 = 215940
intended = 59 * 60 + 59 = 3599
diff = 212341
```Phân hủy:```
212341 = 58 hours + 59 minutes + 1 second
```Đầu ra trở thành:```
58:59:01
```Điều này xác nhận rằng số học vẫn đúng ngay cả ở ranh giới trên của phạm vi đầu vào.
