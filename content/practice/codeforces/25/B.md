---
title: "CF 25B - Số điện thoại"
description: "Chúng ta được cấp một chuỗi các chữ số đại diện cho một số điện thoại. Nhiệm vụ là chia chuỗi này thành các phần trong đó mỗi phần có độ dài 2 hoặc 3. Các nhóm phải xuất hiện theo thứ tự ban đầu và chúng được nối với các ký tự - ở đầu ra."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 25
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 25 (Div. 2 Only)"
rating: 1100
weight: 25
solve_time_s: 84
verified: true
draft: false
---
[CF 25B - Số điện thoại](https://codeforces.com/problemset/problem/25/B) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ số đại diện cho một số điện thoại. Nhiệm vụ là chia chuỗi này thành các phần trong đó mỗi phần có độ dài bằng 2 hoặc 3. Các nhóm phải xuất hiện theo thứ tự ban đầu và chúng được nối với nhau bằng`-`các ký tự ở đầu ra. 

Sự cố không yêu cầu mọi phân vùng có thể. Bất kỳ nhóm hợp lệ đều được chấp nhận. 

Những hạn chế là rất nhỏ. Độ dài số điện thoại tối đa là 100, do đó, ngay cả những giải pháp khá kém hiệu quả cũng sẽ hoạt động thoải mái trong giới hạn. Điều này làm thay đổi bản chất của vấn đề. Thách thức không phải là tối ưu hóa mà là xây dựng một nhóm luôn hoạt động và xử lý chính xác các trường hợp góc nhỏ. 

Điều khó khăn là những lựa chọn tham lam có thể vô tình để lại một chữ số ở cuối. Nhóm có độ dài 1 là bất hợp pháp nên thuật toán phải tránh tạo ra tình huống đó. 

Hãy xem xét đầu vào:```
5
12345
```Nếu chúng ta tham lam chọn nhóm 3 người trước, chúng ta sẽ nhận được:```
123-45
```Điều này là hợp lệ. 

Nhưng nếu chúng tôi xử lý khác và lấy nhóm 2 người trước:```
12-34
```chúng tôi còn lại với`5`, không thể tạo thành một nhóm hợp lệ. 

Một trường hợp tinh vi khác là khi độ dài còn lại trở thành 4. Ví dụ:```
7
1234567
```Nếu chúng ta lấy một nhóm 3 người ngay lập tức:```
123-456
```chúng tôi còn lại với`7`, không hợp lệ. 

Chiến lược đúng là:```
12-34-567
```hoặc```
123-45-67
```Cả hai đều hoạt động vì độ dài còn lại cuối cùng luôn có thể phân tách thành 2 giây và 3 giây. 

Đầu vào hợp lệ nhỏ nhất cũng cần được chăm sóc:```
2
98
```Bản thân toàn bộ chuỗi đã là một nhóm hợp lệ. Việc triển khai bất cẩn luôn cố gắng trích xuất 3 chữ số trước sẽ thất bại ngay lập tức. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi cách có thể để phân tách chuỗi. Tại mỗi vị trí, chúng ta có thể lấy 2 chữ số hoặc 3 chữ số, sau đó lặp lại phần còn lại. Vì mỗi vị trí chia thành hai lựa chọn nên tổng số khả năng sẽ tăng theo cấp số nhân. 

Với độ dài khoảng 100, số lượng trạng thái đệ quy trở nên rất lớn. Mặc dù nhiều nhánh chết sớm, nhưng việc tìm kiếm toàn diện một cách đơn giản là công việc không cần thiết đối với một bài toán có cấu trúc như vậy. 

Lý do vũ lực hoạt động về mặt khái niệm rất đơn giản. Mọi phân vùng hợp lệ chỉ là một chuỗi các lựa chọn có độ dài từ 2 đến 3. Nếu chúng ta khám phá tất cả các chuỗi, cuối cùng chúng ta sẽ tìm thấy một phân vùng hợp lệ. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm. Mọi số có độ dài ít nhất là 2 luôn có thể được biểu diễn chỉ bằng 2 và 3, ngoại trừ độ dài 1. Vì vậy, mục tiêu thực sự chỉ là tránh để chính xác một chữ số bị tách nhóm. 

Điều đó dẫn đến một chiến lược mang tính xây dựng trực tiếp. Trong khi vẫn còn hơn 4 chữ số, chúng tôi chọn nhóm 2 một cách an toàn. Tại sao lại là 2? Bởi vì trừ 2 nhiều lần sẽ bảo toàn khả năng kết thúc hậu tố còn lại với nhóm 2 hoặc 3. 

Khi độ dài còn lại trở thành 2, 3 hoặc 4, chúng tôi dừng lại và xử lý trực tiếp: 

Nếu còn 2 chữ số thì lấy một nhóm 2 chữ số. 

Nếu còn 3 chữ số thì lấy một nhóm 3 chữ số. 

Nếu còn 4 chữ số thì chia thành 2 nhóm 2 chữ số. 

Điều này đảm bảo rằng không có nhóm nào có kích thước không hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Về mặt khái niệm quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi số điện thoại. 
2. Tạo một danh sách trống có tên`groups`để lưu trữ các khối kết quả. 
3. Duy trì chỉ mục trỏ đến vị trí chưa đọc hiện tại. 
4. Khi số chữ số còn lại lớn hơn 4 thì lấy 2 chữ số tiếp theo nối vào`groups`. 

Lấy 2 chữ số lặp đi lặp lại là an toàn vì nó ngăn phần còn lại trở thành 1. 
5. Sau khi vòng lặp kết thúc, độ dài còn lại phải là 2, 3 hoặc 4. 
6. Nếu còn lại 2 chữ số, hãy nối chúng thành một nhóm cuối cùng. 
7. Nếu còn lại 3 chữ số, hãy nối chúng thành một nhóm cuối cùng. 
8. Nếu còn 4 chữ số, chia chúng thành hai nhóm 2 chữ số và nối cả hai. 

Chia 4 thành`2 + 2`tránh tạo ra một nhóm bị cấm có kích thước 1. 
9. In tất cả các nhóm được tham gia bởi`-`. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến đơn giản: sau mỗi bước, số chữ số còn lại vẫn được biểu diễn chỉ bằng các nhóm có kích thước 2 và 3. 

Trừ 2 từ bất kỳ độ dài còn lại hợp lệ nào lớn hơn 4 sẽ giữ phần còn lại hợp lệ. Quá trình cuối cùng đạt đến một trong các độ dài cơ sở 2, 3 hoặc 4, tất cả đều có các phân tách hợp lệ trực tiếp. 

Số dư duy nhất không thể có là 1 và thuật toán được thiết kế đặc biệt để tránh tạo ra nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()

groups = []
i = 0

while n - i > 4:
    groups.append(s[i:i + 2])
    i += 2

remaining = n - i

if remaining == 2:
    groups.append(s[i:i + 2])
elif remaining == 3:
    groups.append(s[i:i + 3])
else:
    groups.append(s[i:i + 2])
    groups.append(s[i + 2:i + 4])

print("-".join(groups))
```Giải pháp tuân theo chiến lược mang tính xây dựng trực tiếp. 

Biến`i`theo dõi có bao nhiêu chữ số đã được xử lý. Vòng lặp tiếp tục trong khi vẫn còn hơn 4 chữ số vì độ dài 2, 3 và 4 có thể được hoàn thành một cách an toàn mà không cần quyết định thêm. 

Việc xử lý độ dài 4 là điều kiện biên quan trọng nhất. Một sai lầm phổ biến là lấy nhóm 3 người trước, bỏ lại một chữ số. Chia thành`2 + 2`tránh hoàn toàn vấn đề đó. 

Việc triển khai sử dụng tính năng cắt trên chuỗi gốc, cách này rõ ràng và hiệu quả đối với những ràng buộc nhỏ này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
549871
```| Bước | tôi | Còn lại | Hành động | Nhóm | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 6 | Lấy 2 chữ số |`["54"]`| 
| Vòng lặp | 2 | 4 | Dừng vòng lặp |`["54"]`| 
| Kết thúc | 2 | 4 | Chia thành 2 + 2 |`["54", "98", "71"]`| 

Đầu ra:```
54-98-71
```Ví dụ này cho thấy tại sao thuật toán dừng khi còn lại 4 chữ số. Hậu tố`9871`được xử lý như`98-71`, tránh chữ số còn sót lại không hợp lệ. 

### Ví dụ 2 

đầu vào:```
7
1234567
```| Bước | tôi | Còn lại | Hành động | Nhóm | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 7 | Lấy 2 chữ số |`["12"]`| 
| Vòng lặp | 2 | 5 | Lấy 2 chữ số |`["12", "34"]`| 
| Kết thúc vòng lặp | 4 | 3 | Dừng vòng lặp |`["12", "34"]`| 
| Kết thúc | 4 | 3 | Lấy 3 chữ số cuối cùng |`["12", "34", "567"]`| 

Đầu ra:```
12-34-567
```Dấu vết này thể hiện sự bất biến một cách rõ ràng. Mỗi bước bảo toàn phần còn lại vẫn có thể được phân tách thành các nhóm hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chữ số được xử lý một lần | 
| Không gian | O(n) | Các nhóm đầu ra lưu trữ tất cả các chữ số | 

Kích thước đầu vào tối đa là 100, do đó quá trình xử lý tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng rất nhỏ vì các nhóm được lưu trữ cùng nhau chứa chính xác các chữ số gốc. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    groups = []
    i = 0

    while n - i > 4:
        groups.append(s[i:i + 2])
        i += 2

    remaining = n - i

    if remaining == 2:
        groups.append(s[i:i + 2])
    elif remaining == 3:
        groups.append(s[i:i + 3])
    else:
        groups.append(s[i:i + 2])
        groups.append(s[i + 2:i + 4])

    print("-".join(groups))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout
    return out.getvalue().strip()

# provided sample
assert run("6\n549871\n") == "54-98-71", "sample 1"

# minimum size
assert run("2\n98\n") == "98", "minimum length"

# exactly 3 digits
assert run("3\n123\n") == "123", "single group of 3"

# exactly 4 digits
assert run("4\n1111\n") == "11-11", "split 4 into 2+2"

# odd length
assert run("7\n1234567\n") == "12-34-567", "avoid leftover 1"

# maximum-style repetitive case
assert run("10\n9999999999\n") == "99-99-99-99-99", "many groups of 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 98`|`98`| Đầu vào hợp lệ tối thiểu | 
|`3 / 123`|`123`| Nhóm đơn 3 người | 
|`4 / 1111`|`11-11`| Xử lý đúng phần còn lại 4 | 
|`7 / 1234567`|`12-34-567`| Chống dư size 1 | 
|`10 / 9999999999`|`99-99-99-99-99`| Thực hiện vòng lặp lặp đi lặp lại | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
4
1111
```Thuật toán ngay lập tức bỏ qua vòng lặp vì chỉ còn lại 4 chữ số. Nó nhập trường hợp cuối cùng và chia chuỗi thành:```
11-11
```Một cách tiếp cận tham lam bất cẩn lấy 3 chữ số đầu tiên sẽ tạo ra:```
111-1
```không hợp lệ vì nhóm có kích thước 1 bị cấm. 

Bây giờ hãy xem xét:```
7
1234567
```Các quá trình thuật toán:```
12
```để lại 5 chữ số. Sau đó nó xử lý:```
34
```để lại 3 chữ số, có thể trở thành một nhóm cuối cùng một cách an toàn:```
567
```Câu trả lời cuối cùng là:```
12-34-567
```Điều này cho thấy việc loại bỏ 2 chữ số liên tục sẽ ngăn phần còn lại trở thành 1. 

Cuối cùng, kiểm tra ranh giới nhỏ nhất:```
2
98
```Vòng lặp không bao giờ chạy vì độ dài còn lại đã hợp lệ. Thuật toán trực tiếp xuất ra:```
98
```Không cần phải hack đặc biệt vì các trường hợp cơ sở xử lý một cách tự nhiên các đầu vào được phép ngắn nhất.
