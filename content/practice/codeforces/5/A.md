---
title: "CF 5A - Lưu lượng truy cập đi của máy chủ trò chuyện"
description: "Chúng tôi được cung cấp một chuỗi các sự kiện máy chủ trò chuyện. Người dùng có thể tham gia trò chuyện, rời khỏi cuộc trò chuyện hoặc gửi tin nhắn. Mỗi khi ai đó gửi tin nhắn, máy chủ sẽ gửi tin nhắn đó đến mọi người dùng hiện có trong cuộc trò chuyện, bao gồm cả người gửi."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 5
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 5"
rating: 1000
weight: 5
solve_time_s: 69
verified: true
draft: false
---
[CF 5A - Lưu lượng truy cập đi của máy chủ trò chuyện](https://codeforces.com/problemset/problem/5/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các sự kiện máy chủ trò chuyện. Người dùng có thể tham gia trò chuyện, rời khỏi cuộc trò chuyện hoặc gửi tin nhắn. Mỗi khi ai đó gửi tin nhắn, máy chủ sẽ gửi tin nhắn đó đến mọi người dùng hiện có trong cuộc trò chuyện, bao gồm cả người gửi. 

Chi phí của một lần gửi tin nhắn là độ dài của nội dung tin nhắn. Nếu có`k`người dùng trong cuộc trò chuyện và độ dài tin nhắn là`l`, sau đó máy chủ tạo ra`k * l`byte lưu lượng đi cho sự kiện đó. 

Nhiệm vụ là xử lý tất cả các lệnh và tính toán tổng lưu lượng được tạo ra. 

Đầu vào nhỏ. Có tối đa 100 dòng và mỗi dòng có độ dài tối đa 100 ký tự. Ngay cả một mô phỏng đơn giản cũng dễ dàng nằm trong giới hạn. Chúng tôi không cần cấu trúc dữ liệu nâng cao hoặc thủ thuật tối ưu hóa. Quét tuyến tính trên đầu vào là đủ. 

Thách thức chính là phân tích cú pháp các lệnh một cách chính xác. Có ba định dạng:```
+name
-name
sender:message
```Ký tự đầu tiên ngay lập tức cho chúng ta biết đây là thao tác thêm, thao tác xóa hay thông báo. 

Một chi tiết tinh tế là người gửi không quan trọng trong việc tính toán lưu lượng truy cập. Máy chủ sẽ gửi tin nhắn đến tất cả mọi người hiện đang trực tuyến, bất kể ai đã viết nó. Chỉ có hai giá trị quan trọng:```
current number of users
message length
```Một lỗi dễ mắc phải khác là tính sai độ dài tin nhắn. Độ dài chỉ bao gồm văn bản sau dấu hai chấm, không bao gồm tên người gửi hoặc chính dấu hai chấm. 

Hãy xem xét ví dụ này:```
+Mike
Mike:hello
```Độ dài tin nhắn là`5`, không`10`. 

Một trường hợp cạnh khác là một tin nhắn trống:```
+Mike
Mike:
```Độ dài tin nhắn là`0`, do đó lưu lượng truy cập được thêm vào cũng`0`, mặc dù một người dùng nhận được nó. 

Việc triển khai bất cẩn có thể phân tách không chính xác hoặc cho rằng mọi thư đều chứa ít nhất một ký tự sau dấu hai chấm. 

Một tình huống nữa thường gây ra lỗi là cập nhật số lượng người dùng không đúng thứ tự.```
+Mike
-Mike
Mike:hello
```Thông tin đầu vào này không hợp lệ theo các đảm bảo vì Mike không còn tham gia cuộc trò chuyện nữa. Tuyên bố đảm bảo những trường hợp như vậy sẽ không bao giờ xảy ra, điều này cho phép chúng tôi tập trung hoàn toàn vào mô phỏng thay vì xác thực. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là theo dõi rõ ràng tất cả người dùng trực tuyến trong một nhóm. Bất cứ khi nào một thông báo xuất hiện, chúng tôi có thể lặp lại qua mọi người dùng hiện đang trực tuyến và thêm độ dài tin nhắn một lần cho mỗi người dùng. 

Cách tiếp cận đó đúng vì máy chủ thực sự gửi một bản sao cho mọi người tham gia. Nếu có`k`người dùng, lặp đi lặp lại tất cả`k`người dùng mô phỏng chính xác quá trình thực tế. 

Vấn đề là chúng ta làm những việc không cần thiết. Vấn đề chỉ hỏi tổng số byte được gửi chứ không hỏi ai đã nhận chúng. Lặp lại tất cả người dùng chỉ để đếm họ lặp lại thông tin chúng ta đã biết. 

Điều quan trọng nhất là mọi người nhận đều nhận được thông điệp giống hệt nhau. Thay vì mô phỏng từng lần phân phối riêng biệt, chúng ta có thể nhân lên:```
traffic added = online_users * message_length
```Điều này làm giảm từng thao tác thông báo từ việc lặp lại trên tất cả người dùng sang tính toán theo thời gian không đổi. 

Với tối đa 100 lệnh, ngay cả giải pháp vũ phu cũng có thể vượt qua một cách thoải mái. Tuy nhiên, phiên bản được tối ưu hóa sẽ sạch hơn và thể hiện trực tiếp cấu trúc thực sự của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Đây,`n`là số lượng lệnh 

## Hướng dẫn thuật toán 

1. Khởi tạo hai biến:`users = 0`về số lượng người hiện tại trong cuộc trò chuyện và`answer = 0`cho tổng lưu lượng đi. 
2. Đọc từng dòng đầu vào cho đến khi EOF. 
3. Nếu một dòng bắt đầu bằng`'+'`, tăng`users`qua`1`. 

Một người mới đã tham gia cuộc trò chuyện nên các tin nhắn trong tương lai sẽ được gửi tới một người nhận khác. 
4. Nếu một dòng bắt đầu bằng`'-'`, giảm bớt`users`qua`1`. 

Người dùng đó đã rời khỏi cuộc trò chuyện nên các tin nhắn trong tương lai sẽ đến được ít hơn một người. 
5. Nếu không, dòng này sẽ biểu thị một thông báo. 

Tìm vị trí của đại tràng`':'`. Mọi thứ sau đó là nội dung tin nhắn. 
6. Tính độ dài tin nhắn như sau:`len(line_after_colon)`7. Thêm:`users * message_length`ĐẾN`answer`. 

Mỗi người dùng trực tuyến nhận được một bản sao của tin nhắn, vì vậy tổng số byte được gửi chính xác là sản phẩm này. 
8. Sau khi xử lý xong tất cả các dòng, in`answer`. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến trong suốt quá trình thực hiện:```
users = number of people currently inside the chat
```Các thao tác thêm và xóa cập nhật giá trị này một cách chính xác theo các lệnh. Bất cứ khi nào một thông báo xuất hiện, mỗi người tham gia hiện tại sẽ nhận được chính xác một bản sao của thông báo. Vì độ dài tin nhắn là`l`và có`users`người tham gia, lưu lượng truy cập được thêm bởi sự kiện đó chính xác là`users * l`. 

Vì mọi lệnh đều được xử lý theo thứ tự và mỗi tin nhắn đóng góp được tính chính xác một lần nên tổng cuối cùng là tổng lưu lượng gửi đi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

users = 0
answer = 0

for line in sys.stdin:
    line = line.rstrip('\n')

    if line[0] == '+':
        users += 1

    elif line[0] == '-':
        users -= 1

    else:
        colon = line.find(':')
        message = line[colon + 1:]
        answer += users * len(message)

print(answer)
```Giải pháp trực tiếp tuân theo mô phỏng được mô tả trước đó. 

Biến`users`lưu trữ bao nhiêu người hiện đang trực tuyến. Vì các lệnh thêm và xóa luôn có thứ tự hợp lệ nên việc tăng và giảm bộ đếm này là đủ. Chúng tôi thực sự không cần lưu trữ tên người dùng. 

Đối với dòng tin nhắn, phần quan trọng là chỉ trích xuất nội dung tin nhắn. Tên người gửi không được đóng góp vào số byte. sử dụng`find(':')`đưa ra vị trí phân cách và cắt từ`colon + 1`trích xuất thông điệp thực tế. 

sử dụng`rstrip('\n')`xóa dòng mới ở cuối được thêm vào bằng cách đọc đầu vào. Nếu không có điều này, ký tự dòng mới sẽ tăng độ dài tin nhắn một cách không chính xác. 

Thuật toán xử lý mỗi dòng chính xác một lần và chỉ thực hiện công việc có thời gian không đổi cho mỗi lệnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
+Mike
Mike:hello
+Kate
+Dmitry
-Dmitry
Kate:hi
-Kate
```| Lệnh | Người dùng trước | Độ dài tin nhắn | Đã thêm lưu lượng truy cập | Người dùng sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| +Mike | 0 | - | 0 | 1 | 0 | 
| Mike: xin chào | 1 | 5 | 5 | 1 | 5 | 
| +Kate | 1 | - | 0 | 2 | 5 | 
| +Dmitry | 2 | - | 0 | 3 | 5 | 
| -Dmitry | 3 | - | 0 | 2 | 5 | 
| Kate: xin chào | 2 | 2 | 4 | 2 | 9 | 
| -Kate | 2 | - | 0 | 1 | 9 | 

Dấu vết cho thấy rằng chỉ có các lệnh thông báo mới đóng góp vào câu trả lời. Tin nhắn thứ hai có độ dài`2`và được giao đến`2`người dùng, thêm`4`. 

### Ví dụ 2 

đầu vào:```
+Ann
+Bob
Ann:
-Bob
Ann:abc
```| Lệnh | Người dùng trước | Độ dài tin nhắn | Đã thêm lưu lượng truy cập | Người dùng sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| +Ann | 0 | - | 0 | 1 | 0 | 
| +Bob | 1 | - | 0 | 2 | 0 | 
| An: | 2 | 0 | 0 | 2 | 0 | 
| -Bob | 2 | - | 0 | 1 | 0 | 
| Ann:abc | 1 | 3 | 3 | 1 | 3 | 

Ví dụ này minh họa trường hợp cạnh thông báo trống. Lệnh`Ann:`không tạo ra lưu lượng truy cập vì nội dung tin nhắn có độ dài`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lệnh được xử lý một lần | 
| Không gian | O(1) | Chỉ có bộ đếm được lưu trữ | 

Các ràng buộc rất nhỏ nên giải pháp này dễ dàng nằm gọn trong giới hạn. Ngay cả với tối đa 100 lệnh, thời gian chạy vẫn diễn ra tức thời. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    users = 0
    answer = 0

    for line in sys.stdin:
        line = line.rstrip('\n')

        if line[0] == '+':
            users += 1

        elif line[0] == '-':
            users -= 1

        else:
            colon = line.find(':')
            message = line[colon + 1:]
            answer += users * len(message)

    print(answer)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run(
"""+Mike
Mike:hello
+Kate
+Dmitry
-Dmitry
Kate:hi
-Kate
"""
) == "9", "sample 1"

# single user, single message
assert run(
"""+A
A:test
"""
) == "4", "basic case"

# empty message
assert run(
"""+Ann
+Bob
Ann:
"""
) == "0", "empty message"

# multiple users receive same message
assert run(
"""+A
+B
+C
A:abc
"""
) == "9", "3 users * length 3"

# user leaves before next message
assert run(
"""+A
+B
-B
A:hello
"""
) == "5", "correct user count after removal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một người dùng gửi một tin nhắn | 4 | Tính toán tin nhắn cơ bản | 
| Nội dung tin nhắn trống | 0 | Xử lý đúng các tin nhắn có độ dài bằng 0 | 
| Ba người dùng nhận được`"abc"`| 9 | Nhân với số lượng người dùng hiện tại | 
| Người dùng đã xóa trước tin nhắn | 5 | Thứ tự cập nhật chính xác cho việc tham gia và rời đi | 

## Vỏ cạnh 

Hãy xem xét trường hợp tin nhắn trống:```
+Ann
+Bob
Ann:
```Sau hai lệnh đầu tiên,`users = 2`. Nội dung tin nhắn sau dấu hai chấm là một chuỗi trống nên độ dài của nó là`0`. 

Thuật toán tính toán:```
2 * 0 = 0
```Câu trả lời cuối cùng vẫn còn`0`, điều đó đúng. 

Bây giờ hãy xem xét việc phân tích tin nhắn một cách cẩn thận:```
+Mike
Mike:hello
```Độ dài dòng đầy đủ lớn hơn`5`, nhưng chỉ có chuỗi con sau`:`tính. Thuật toán tìm vị trí dấu hai chấm và trích xuất`"hello"`chỉ một. 

Sự đóng góp trở thành:```
1 * 5 = 5
```Việc triển khai bất cẩn khi sử dụng toàn bộ chiều dài dòng sẽ tạo ra câu trả lời sai. 

Cuối cùng, hãy xem xét việc tham gia và rời khỏi việc thay đổi số lượng người nhận:```
+A
+B
-B
A:abc
```Sau đó`+A`, người dùng trở thành`1`. 

Sau đó`+B`, người dùng trở thành`2`. 

Sau đó`-B`, người dùng quay lại`1`. 

Khi`"abc"`được gửi đi, chỉ có một người tham gia trực tuyến, do đó lưu lượng được thêm vào là:```
1 * 3 = 3
```Thuật toán xử lý việc này một cách chính xác vì bộ đếm người dùng được cập nhật ngay sau mỗi lệnh.
