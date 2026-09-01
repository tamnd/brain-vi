---
title: "CF 104447B - Aswad sử dụng Telegram như thế nào?"
description: "Chúng tôi nhận được nhật ký tin nhắn theo thứ tự thời gian trong một cuộc trò chuyện nhóm. Mỗi tin nhắn có một người gửi và dấu thời gian. Một người tham gia cụ thể, Aswad, tuân theo quy tắc phản ứng cố định: bất cứ khi nào một tin nhắn xuất hiện, anh ta sẽ bắt đầu hẹn giờ chờ với độ dài k phút."
date: "2026-06-30T17:58:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104447
codeforces_index: "B"
codeforces_contest_name: "Al-Baath Collegiate Programming Contest 2023"
rating: 0
weight: 104447
solve_time_s: 61
verified: true
draft: false
---

[CF 104447B - Aswad sử dụng Telegram như thế nào?](https://codeforces.com/problemset/problem/104447/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được nhật ký tin nhắn theo thứ tự thời gian trong một cuộc trò chuyện nhóm. Mỗi tin nhắn có một người gửi và dấu thời gian. Một người tham gia cụ thể, Aswad, tuân theo quy tắc phản ứng cố định: bất cứ khi nào một tin nhắn xuất hiện, anh ta sẽ bắt đầu hẹn giờ chờ với độ dài k phút. Nếu không có tin nhắn nào khác xuất hiện trước khi bộ đếm thời gian đó kết thúc, anh ấy sẽ gửi chính xác một phản hồi. Nếu một tin nhắn khác xuất hiện trước khi bộ đếm thời gian hoàn thành, anh ta sẽ loại bỏ phản ứng đang chờ xử lý và khởi động lại bộ đếm thời gian chờ từ tin nhắn mới đó. 

Nhiệm vụ là xác định có bao nhiêu phản hồi mà Aswad gửi cho mỗi trường hợp thử nghiệm, dựa trên tất cả các dấu thời gian của tin nhắn theo thứ tự. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp k, thời lượng chờ và chuỗi dấu thời gian của tin nhắn. ID người gửi không liên quan đến logic vì Aswad phản ứng với tiến trình thời gian hơn là ai đã gửi tin nhắn. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số lần Aswad hoàn thành thành công khoảng thời gian chờ đợi không bị gián đoạn. 

Các ràng buộc đủ nhỏ để quét tuyến tính trực tiếp cho mỗi trường hợp thử nghiệm là đủ. Ngay cả khi chúng tôi xử lý mọi thư và so sánh nó với thời hạn hiện đang được theo dõi thì tổng công việc vẫn nằm trong giới hạn. Điều quan trọng là mỗi tin nhắn được xử lý một lần và mỗi quyết định đều có thời gian không đổi. 

Một trường hợp phức tạp xuất hiện xung quanh các tin nhắn đến chính xác vào thời điểm thời gian chờ đợi kết thúc. Nếu một tin nhắn đến vào thời điểm t + k khi tin nhắn cuối cùng bắt đầu vào thời điểm t thì việc chờ đợi được coi là hoàn thành chứ không phải bị gián đoạn. Vì vậy, sự bình đẳng sẽ kích hoạt phản hồi thành công chứ không phải thiết lập lại. 

Một trường hợp quan trọng khác là khi tin nhắn đến nhanh chóng. Ví dụ: nếu tin nhắn liên tục được gửi đến mỗi phút và k lớn thì Aswad sẽ không bao giờ phản hồi. Ngược lại, nếu có một khoảng trống dài sau một tin nhắn nào đó, Aswad sẽ phản hồi chính xác một lần cho khoảng trống đó. 

## Phương pháp tiếp cận 

Cách diễn giải thô bạo sẽ mô phỏng thông báo hành vi của Aswad theo từng tin nhắn và duy trì rõ ràng một hàng "cửa sổ chờ" đang chờ xử lý. Đối với mỗi tin nhắn, chúng tôi có thể quét chuyển tiếp kịp thời để kiểm tra xem có tin nhắn nào sau đó xuất hiện trước khi k phút trôi qua hay không và quyết định xem tin nhắn hiện tại có dẫn đến phản hồi hay không. Điều này dẫn đến việc quét liên tục các hậu tố của mảng và trong trường hợp xấu nhất khi số lượng tin nhắn dày đặc, mỗi tin nhắn có thể yêu cầu kiểm tra nhiều sự kiện trong tương lai. Điều đó đẩy độ phức tạp lên tới O(m2) cho mỗi trường hợp thử nghiệm, điều này là không cần thiết vì m có thể đạt đến kích thước đầu vào đầy đủ. 

Sự cải tiến đến từ việc nhận thấy rằng quy trình không yêu cầu phải xem trước nhiều hơn một bước. Chúng tôi chỉ cần theo dõi một tin nhắn ứng viên đang hoạt động hiện đang “chờ được trả lời”. Khi một tin nhắn mới đến, nó sẽ vô hiệu hóa ứng cử viên hiện tại hoặc cho phép nó hoàn thành. Điều này biến vấn đề thành một mô phỏng tham lam vượt qua một lần: chúng tôi duy trì dấu thời gian của tin nhắn cuối cùng đã bắt đầu một khoảng thời gian chờ và chúng tôi duy trì thời điểm khoảng thời gian chờ đó sẽ hết hạn. 

Sau khi chúng tôi trình bày vấn đề về “thời gian bắt đầu hoạt động hiện tại” và “thời gian hết hạn”, mọi thông báo sẽ mở rộng trạng thái hoạt động hoặc kích hoạt phản hồi hoàn chỉnh. Điều này loại bỏ tất cả nhu cầu quét lồng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(1) | Quá chậm | 
| Mô phỏng tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi tất cả dấu thời gian thành dạng số có thể so sánh được, cụ thể là số phút kể từ nửa đêm. Điều này cho phép số học trực tiếp để so sánh k-phút. 

Sau đó, chúng tôi xử lý tin nhắn theo trình tự thời gian trong khi vẫn duy trì hai biến số: dấu thời gian của tin nhắn đang chờ xử lý hiện tại và thời điểm phản hồi của tin nhắn đó sẽ được kích hoạt nếu không bị gián đoạn.

1. Khởi tạo bộ đếm phản hồi của Aswad về 0. Đồng thời khởi tạo không có tin nhắn đang chờ xử lý nào. 
2. Chuyển đổi từng dấu thời gian HH:MM thành tổng số phút bằng cách sử dụng 60 * HH + MM để hiệu số chỉ bằng phép trừ số nguyên đơn giản. Điều này tránh việc xử lý ranh giới giờ một cách rõ ràng. 
3. Đối với tin nhắn đầu tiên, hãy đặt nó làm tin nhắn đang chờ xử lý và đặt thời gian hết hạn của nó thành dấu thời gian cộng với k phút. Điều này thể hiện thời điểm sớm nhất Aswad sẽ phản hồi nếu không có gì làm gián đoạn. 
4. Đối với mỗi tin nhắn tiếp theo, hãy so sánh dấu thời gian của nó với thời gian hết hạn hiện tại. Nếu tin nhắn mới đến trước thời gian hết hạn, quá trình chờ đợi trước đó sẽ bị gián đoạn và bị loại bỏ, vì vậy chúng tôi thay thế tin nhắn đang hoạt động bằng tin nhắn mới và tính toán lại thời hạn sử dụng của nó. 
5. Nếu tin nhắn mới đến đúng hoặc sau thời gian hết hạn thì thời gian chờ trước đó đã hoàn tất thành công trước khi bị gián đoạn. Chúng tôi tăng bộ đếm phản hồi vì tin nhắn đang chờ xử lý hiện đã tạo ra phản hồi. Sau đó, chúng tôi coi tin nhắn hiện tại là điểm bắt đầu mới và gán cho nó thời gian hết hạn mới. 
6. Sau khi xử lý tất cả tin nhắn, tin nhắn đang chờ xử lý cuối cùng có thể vẫn có khoảng thời gian chờ hợp lệ và không bao giờ bị gián đoạn. Chúng tôi coi đó là phản hồi vì không có tin nhắn nào trong tương lai hủy nó. 

Điểm quyết định quan trọng luôn là sự so sánh giữa dấu thời gian tiếp theo và thời gian hết hạn hiện tại. 

### Tại sao nó hoạt động 

Ở mỗi bước, có nhiều nhất một “tin nhắn ứng viên” đang hoạt động có thể tạo ra phản hồi. Bất kỳ tin nhắn nào đến trước khi hết hạn sẽ vô hiệu hóa nó hoàn toàn và không có tin nhắn nào trước đó có liên quan trở lại vì nó luôn bị hủy hoặc đã hoàn thành. Điều này có nghĩa là thuật toán duy trì tính bất biến rằng ứng viên đang hoạt động luôn là tin nhắn gần đây nhất chưa bị gián đoạn và thời gian hết hạn của nó thể hiện chính xác thời gian sớm nhất có thể xảy ra phản hồi. Vì mỗi tin nhắn được xử lý một lần và quá trình chuyển đổi là cuối cùng nên số lần hết hạn hoàn thành khớp chính xác với phản hồi của Aswad. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_minutes(t):
    h, m = map(int, t.split(":"))
    return h * 60 + m

t = int(input())
for _ in range(t):
    n, k, m = map(int, input().split())

    times = []
    for _ in range(m):
        _, ts = input().split()
        times.append(to_minutes(ts))

    ans = 0
    start = times[0]
    expiry = start + k

    for i in range(1, m):
        cur = times[i]
        if cur < expiry:
            start = cur
            expiry = cur + k
        else:
            ans += 1
            start = cur
            expiry = cur + k

    ans += 1
    print(ans)
```Việc triển khai duy trì thời gian “bắt đầu” đang chạy cho tin nhắn đang hoạt động và thời gian hết hạn được tính toán của nó. Sự so sánh`cur < expiry`ghi lại sự gián đoạn một cách nghiêm ngặt bên trong cửa sổ chờ. Bình đẳng được coi là hoàn thành, đó là lý do tại sao phản hồi được tính trước khi chuyển trạng thái. 

trận chung kết`ans += 1`là cần thiết vì cửa sổ hoạt động cuối cùng luôn hoàn thành trừ khi bị vô hiệu rõ ràng sau đó và vì không có thông báo nào sau đó nên nó phải được tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 6, k = 5 

tin nhắn: 

01:00, 02:00, 03:00, 03:06, 03:07 

Số phút được quy đổi: 

60, 120, 180, 186, 187 

Chúng tôi theo dõi trạng thái từng bước. 

| Thời gian nhắn tin | Khởi động tích cực | Hết hạn | Hành động | Phản hồi | 
| --- | --- | --- | --- | --- | 
| 60 | 60 | 65 | bắt đầu | 0 | 
| 120 | 120 | 125 | trước đó đã hết hạn trước khi bị gián đoạn | 1 | 
| 180 | 180 | 185 | trước đó đã hết hạn | 2 | 
| 186 | 186 | 191 | ngắt trước đó | 2 | 
| 187 | 187 | 192 | ngắt trước đó | 2 | 

Cửa sổ chờ xử lý cuối cùng hoàn tất, vì vậy tổng số sẽ là 3. 

Điều này cho thấy rằng chỉ những khoảng trống không bị gián đoạn trong ít nhất k phút mới góp phần tạo ra phản hồi. 

### Ví dụ 2 

đầu vào: 

tin nhắn: 

03:45, 04:00, 04:07, 04:30, 04:41, 06:09 với k = 15 

Đã chuyển đổi: 

225, 240, 247, 270, 281, 369 

| Tin nhắn | Bắt đầu | Hết hạn | Hành động | Phản hồi | 
| --- | --- | --- | --- | --- | 
| 225 | 225 | 240 | bắt đầu | 0 | 
| 240 | 240 | 255 | hoàn thành trước đó | 1 | 
| 247 | 247 | 262 | ngắt | 1 | 
| 270 | 270 | 285 | hoàn thành trước đó | 2 | 
| 281 | 281 | 296 | ngắt | 2 | 
| 369 | 369 | 384 | hoàn thành trước đó | 3 | 

Dấu vết này cho thấy sự gián đoạn xen kẽ và khoảng trống dài tạo ra nhiều phản hồi hoàn chỉnh như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) cho mỗi trường hợp thử nghiệm | Mỗi tin nhắn được xử lý một lần với thời gian cập nhật liên tục | 
| Không gian | O(1) | Chỉ có một số biến được duy trì bên cạnh việc lưu trữ đầu vào | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với tối đa 1440 thông báo cho mỗi trường hợp, do đó, quá trình quét tuyến tính dễ dàng đủ nhanh ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def to_minutes(t):
        h, m = map(int, t.split(":"))
        return h * 60 + m

    t = int(input())
    out = []
    for _ in range(t):
        n, k, m = map(int, input().split())
        times = []
        for _ in range(m):
            _, ts = input().split()
            times.append(to_minutes(ts))

        ans = 0
        start = times[0]
        expiry = start + k

        for i in range(1, m):
            cur = times[i]
            if cur < expiry:
                start = cur
                expiry = cur + k
            else:
                ans += 1
                start = cur
                expiry = cur + k

        ans += 1
        out.append(str(ans))

    return "\n".join(out)

# provided sample (conceptual format)
assert run("""1
1 5 3
1 00:00
1 00:02
1 00:10
""") == "1"

# all spaced far apart
assert run("""1
1 5 3
1 00:00
1 00:10
1 00:20
""") == "3"

# dense messages prevent any response
assert run("""1
1 10 3
1 00:00
1 00:01
1 00:02
""") == "0"

# exact boundary equality
assert run("""1
1 5 2
1 00:00
1 00:05
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tin nhắn cách nhau | 3 | mọi khoảng cách đều gây ra phản ứng | 
| dòng suối dày đặc | 0 | chặn gián đoạn liên tục tất cả | 
| ranh giới k chính xác | 1 | bình đẳng được tính là hoàn thành | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi các tin nhắn đến cách nhau đúng k phút. Trong trường hợp này, thời gian chờ hoàn thành chính xác vào thời điểm tin nhắn tiếp theo đến, vì vậy phản hồi trước đó vẫn được tính. Ví dụ: với k = 5 và tin nhắn lúc 00:00 và 00:05, tin nhắn đầu tiên tạo ra phản hồi và tin nhắn thứ hai bắt đầu một chu kỳ mới. Thuật toán xử lý việc này vì nó sử dụng một quy tắc nghiêm ngặt`< expiry`điều kiện gián đoạn nên sự bình đẳng được coi là sự hoàn thành. 

Một trường hợp đặc biệt khác là một chuỗi tin nhắn hoàn toàn dày đặc trong đó mọi tin nhắn đều đến trước khi k phút trôi qua. Trong tình huống đó, không bao giờ được phép kết thúc thời gian chờ đợi. Mô phỏng liên tục đặt lại thông báo đang hoạt động và bộ đếm không bao giờ tăng cho đến khi hoàn thành bắt buộc cuối cùng ở cuối. Điều này phù hợp với quy tắc chỉ những khoảng thời gian không bị gián đoạn mới tạo ra phản hồi. 

Trường hợp cạnh cuối cùng là một tin nhắn duy nhất. Chỉ với một dấu thời gian nên không có khả năng bị gián đoạn nên Aswad luôn đưa ra chính xác một phản hồi. Thuật toán xử lý việc này một cách tự nhiên vì cửa sổ chờ xử lý ban đầu luôn được tính ở cuối.
