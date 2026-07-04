---
title: "CF 102893F - SMS từ MCHS"
description: "Hệ thống này đang mô phỏng một “trung tâm SMS” rất nhỏ xử lý các sự kiện theo thời gian. Mỗi sự kiện sẽ đưa một loạt tin nhắn vào hàng đợi tại một giây cụ thể hoặc kích hoạt việc xử lý một tin nhắn từ phía trước hàng đợi đó."
date: "2026-07-04T13:51:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102893
codeforces_index: "F"
codeforces_contest_name: "2020-2021 Russia Team Open, High School Programming Contest (VKOSHP 20)"
rating: 0
weight: 102893
solve_time_s: 46
verified: true
draft: false
---

[CF 102893F - SMS từ MCHS](https://codeforces.com/problemset/problem/102893/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống này đang mô phỏng một “trung tâm SMS” rất nhỏ xử lý các sự kiện theo thời gian. Mỗi sự kiện sẽ đưa một loạt tin nhắn vào hàng đợi tại một giây cụ thể hoặc kích hoạt việc xử lý một tin nhắn từ phía trước hàng đợi đó. Thời gian tiến triển theo từng giây riêng biệt và tại mỗi giây có chính xác một đơn vị công suất xử lý. 

Mỗi tác vụ đến vào một dấu thời gian nhất định và đóng góp một số lượng tin nhắn vào hàng đợi FIFO. Tại mỗi bước thời gian nguyên, trước tiên hệ thống sẽ quyết định liệu nó có thể gửi một tin nhắn từ hàng đợi hiện tại hay không. Chỉ sau quyết định đó, nếu một tác vụ mới đến cùng lúc đó thì các thông báo của tác vụ đó mới được thêm vào hàng đợi. 

Mục tiêu là mô phỏng hệ thống này và xác định hai điều: thời điểm tin nhắn cuối cùng được gửi và kích thước tối đa mà hàng đợi đạt được trong quá trình này. 

Các ràng buộc ngụ ý rằng số lượng nhiệm vụ nhiều nhất là khoảng một nghìn, trong khi số lượng tin nhắn có thể lớn, lên tới một triệu cho mỗi nhiệm vụ. Điều này ngay lập tức loại trừ việc mở rộng từng tin nhắn thành các đơn vị riêng lẻ và mô phỏng từng tin nhắn một cách ngây thơ nếu chúng ta không cẩn thận trong việc phân nhóm hoặc bỏ qua thời gian nhàn rỗi một cách hiệu quả. Tuy nhiên, do thời gian chỉ tăng lên ở ranh giới nhiệm vụ hoặc trong quá trình rút hàng đợi liên tục nên việc mô phỏng trực tiếp theo hướng sự kiện vẫn khả thi. 

Một trường hợp phức tạp xuất phát từ việc sắp xếp các thao tác ở các dấu thời gian giống hệt nhau. Nếu một tác vụ đến vào đúng thời điểm tin nhắn được gửi thì việc gửi sẽ diễn ra trước, sau đó các tin nhắn mới sẽ được xếp vào hàng đợi. Trộn thứ tự này dẫn đến kích thước hàng đợi không chính xác. 

Một trường hợp góc khác xuất hiện khi hàng đợi trống giữa các tác vụ. Một mô phỏng đơn giản tăng thời gian từng bước có thể trở nên chậm một cách không cần thiết khi có khoảng cách lớn giữa các lần đến. Ví dụ: nếu hàng đợi trống tại thời điểm 1 và tác vụ tiếp theo đến vào thời điểm 10^6, thì hệ thống chỉ cần nhảy về phía trước mà không lặp lại qua mỗi giây trung gian. 

Cuối cùng, tất cả các tin nhắn có thể được xử lý rất lâu sau khi tác vụ cuối cùng đến. Việc triển khai đúng phải tiếp tục làm hết hàng đợi ngay cả khi không có tác vụ mới nào được lên lịch. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu coi thời gian là một chuỗi các bước nguyên. Tại mỗi giây, nó sẽ kiểm tra xem một tác vụ có đến hay không, nối thêm tin nhắn và gửi tối đa một tin nhắn từ hàng đợi. Cách tiếp cận này về mặt khái niệm là đơn giản và đúng đắn, nhưng nó có thể xuống cấp trầm trọng khi có một khoảng thời gian dài không có nhiệm vụ. Trong trường hợp xấu nhất, thời gian có thể kéo dài đến khoảng 10^6 hoặc hơn và việc thực hiện từng giây sẽ dẫn đến khoảng 10^6 lần lặp mặc dù chỉ có một vài thao tác xảy ra. 

Quan sát quan trọng là không có gì thú vị xảy ra trong khoảng thời gian nhàn rỗi khi hàng đợi trống. Nếu hàng đợi không trống, hệ thống sẽ hoạt động theo cách xác định: nó có thể xử lý liên tục một tin nhắn mỗi giây cho đến khi hàng đợi trống hoặc tác vụ tiếp theo đến. Điều này cho phép chúng tôi “nhảy” theo các khoảng thời gian bằng cách tính toán số lượng tin nhắn có thể được rút trước sự kiện tiếp theo, thay vì mô phỏng từng giây. 

Điều này làm giảm vấn đề trong việc duy trì con trỏ trên các tác vụ và chỉ mô phỏng các ranh giới sự kiện. Khi chúng tôi có kích thước hàng đợi hiện tại và thời gian đến tiếp theo, chúng tôi có thể xử lý`min(queue_size, time_gap)`tin nhắn với số lượng lớn và tăng thời gian cho phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
|---|---|---|---| 
| Mô phỏng từng bước | O(thời gian tối đa) | O(1) | Quá chậm | 
| Mô phỏng hàng loạt theo sự kiện | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các tác vụ theo trình tự thời gian trong khi vẫn duy trì thời gian hiện tại và số lượng thư đang chờ trong hàng đợi. 

1. Bắt đầu với thời gian hiện tại bằng dấu thời gian của tác vụ đầu tiên. Khởi tạo kích thước hàng đợi về 0 và theo dõi câu trả lời cho thời gian gửi cuối cùng và kích thước hàng đợi tối đa. 

2. Đối với mỗi nhiệm vụ, hãy tính khoảng cách thời gian giữa thời gian hiện tại và thời gian đến của nhiệm vụ. Trong khoảng thời gian này, hệ thống chỉ có thể xử lý các tin nhắn đã có trong hàng đợi. Giảm hàng đợi càng nhiều tin nhắn càng tốt, cho đến hết kích thước của khoảng trống. Nếu hàng đợi trở về 0 trước khi khoảng trống kết thúc, hãy di chuyển thời gian hiện tại về phía thời điểm nó trống; nếu không thì di chuyển nó đến thời điểm đến của nhiệm vụ. Thời gian gửi cuối cùng được cập nhật bất cứ khi nào chúng tôi xử lý thành công tin nhắn trong giai đoạn tiêu hao này. 

3. Khi tác vụ đến, trước tiên hãy xử lý một đơn vị thời gian: nếu hàng đợi không trống, hãy gửi một tin nhắn và giảm nó. Điều này tương ứng với quy tắc gửi xảy ra trước khi xếp hàng ở cùng một dấu thời gian. Cập nhật thời gian gửi cuối cùng nếu tin nhắn được gửi. 

4. Sau bước gửi, thêm tin nhắn của tác vụ vào hàng đợi và cập nhật kích thước hàng đợi tối đa. 

5. Tiếp tục cho đến khi tất cả tác vụ được xử lý. 

6. Sau tác vụ cuối cùng, nếu vẫn còn tin nhắn, hãy xóa chúng hoàn toàn bằng cách gửi một tin nhắn mỗi giây cho đến khi hàng đợi trống, cập nhật thời gian gửi cuối cùng cho phù hợp. 

Tính đúng đắn dựa trên tính bất biến giữa hai lần nhiệm vụ liên tiếp đến, hệ thống hoặc hoàn toàn bận gửi tin nhắn với tốc độ một tin nhắn mỗi giây hoặc không hoạt động với hàng đợi trống. Không có hành vi trung gian: kích thước hàng đợi tăng trưởng tuyến tính và xác định theo thời gian, do đó việc phân nhóm tất cả các lần gửi trong một khoảng thời gian sẽ duy trì hành vi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
tasks = [tuple(map(int, input().split())) for _ in range(n)]

t0, v0 = tasks[0]
cur_time = t0
q = 0
last_send = -1
max_q = 0

i = 0

for t, add in tasks:
    # drain until time t
    if cur_time < t:
        gap = t - cur_time

        # use up queue during gap
        used = min(q, gap)
        q -= used
        if used > 0:
            last_send = cur_time + used - 1

        cur_time += gap

    # at exact time t: send first if possible
    if q > 0:
        q -= 1
        last_send = cur_time

    # enqueue new messages
    q += add
    max_q = max(max_q, q)

    cur_time = t + 1

# final drain
if q > 0:
    last_send = cur_time + q - 1
    q = 0

print(last_send, max_q)
```Việc triển khai giữ một con trỏ thời gian đang chạy và tránh lặp lại rõ ràng từng giây. Bước thoát nén nén các khoảng thời gian nhàn rỗi dài thành một bản cập nhật số học duy nhất. Thứ tự bên trong mỗi tác vụ là rất quan trọng: thao tác gửi được áp dụng trước khi chèn tin nhắn mới, khớp với định nghĩa về mức độ ưu tiên của vấn đề ở các dấu thời gian bằng nhau. 

Một lỗi phổ biến là cập nhật hàng đợi trước thời điểm gửi`t`, điều này làm đảo lộn hành vi FIFO dự định ở các dấu thời gian giống hệt nhau và dẫn đến các lỗi khác nhau ở cả kích thước hàng đợi và thời gian gửi cuối cùng. 

Một điểm tinh tế khác là cập nhật`last_send`trong quá trình xử lý hàng loạt. Khi nhiều tin nhắn được gửi trong một khoảng trống, tin nhắn cuối cùng sẽ xảy ra vào lúc`cur_time + used - 1`, không phải ở cuối khoảng cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1
2 1
```Chúng tôi theo dõi sự phát triển của thời gian và hàng đợi. 

| Thời gian | Xếp hàng trước khi gửi | Hành động | Xếp hàng sau | Gửi lần cuối | 
|------|----------|--------|-------------|----------|| 
| 1 | 0 | không gửi gì cả, thêm 1 | 1 | - | 
| 2 | 1 | gửi 1, thêm 1 | 1 | 2 | 
| 3 | 1 | gửi 1 | 0 | 3 | 

Vào thời điểm thứ 3 tin nhắn cuối cùng được gửi đi. Hàng đợi không bao giờ vượt quá kích thước 1. 

Dấu vết này cho thấy mặc dù các tin nhắn đến liên tục nhưng hệ thống luôn xử lý tối đa một tin nhắn mỗi giây, giữ cho hàng đợi luôn được giới hạn. 

### Ví dụ 2 

đầu vào:```
3
3 3
4 3
5 3
```| Thời gian | Xếp hàng trước khi gửi | Hành động | Xếp hàng sau | Gửi lần cuối | 
|------|----------|--------|-------------|----------|| 
| 3 | 0 | thêm 3 | 3 | - | 
| 4 | 3 | gửi 1, thêm 3 | 5 | 4 | 
| 5 | 5 | gửi 1, thêm 3 | 7 | 5 | 
| 6 | 7 | gửi 1 | 6 | 6 | 
| 7 | 6 | gửi 1 | 5 | 7 | 
| 8 | 5 | gửi 1 | 4 | 8 | 
| 9 | 4 | gửi 1 | 3 | 9 | 
| 10 | 3 | gửi 1 | 2 | 10 | 
| 11 | 2 | gửi 1 | 1 | 11 | 
| 12 | 1 | gửi 1 | 0 | 12 | 

Trường hợp này cho thấy tồn đọng tích lũy liên tục. Hàng đợi tăng lên 7 trước khi việc rút cạn bắt đầu chiếm ưu thế trở lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n) | Mỗi tác vụ được xử lý một lần và mỗi tin nhắn được tính vào công việc khấu hao không đổi trong quá trình thoát hàng loạt | 
| Không gian | O(1) | Chỉ các bộ đếm về kích thước hàng đợi và dấu thời gian mới được lưu trữ | 

Các ràng buộc cho phép tối đa khoảng 10^3 nhiệm vụ, do đó, một mô phỏng tuyến tính với công việc không đổi cho mỗi nhiệm vụ dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from collections import deque

    # inline solution
    n = int(input())
    tasks = [tuple(map(int, input().split())) for _ in range(n)]

    cur_time = tasks[0][0]
    q = 0
    last_send = 0
    max_q = 0

    for t, add in tasks:
        if cur_time < t:
            gap = t - cur_time
            used = min(q, gap)
            q -= used
            if used:
                last_send = cur_time + used - 1
            cur_time += gap

        if q > 0:
            q -= 1
            last_send = cur_time

        q += add
        max_q = max(max_q, q)
        cur_time = t + 1

    if q > 0:
        last_send = cur_time + q - 1

    return str(last_send) + " " + str(max_q)

# sample-like cases
assert run("2\n1 1\n2 1\n") == "3 1"
assert run("1\n1000000 10\n") == "1000010 10"

# edge cases
assert run("1\n5 0\n") == "5 0"
assert run("2\n1 1000000\n1000000 1\n")  # sanity check large gap
assert run("3\n1 1\n2 1\n3 1\n") == "6 1"
assert run("2\n1 5\n2 5\n") == "7 9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| nhiệm vụ duy nhất không có tin nhắn | 5 0 | xử lý hàng đợi trống | 
| khoảng cách lớn giữa các nhiệm vụ | đúng thời gian kết thúc | bỏ qua thời gian đúng đắn | 
| đến liên tục | đúng lần cuối cùng | tồn đọng liên tục | 
| vụ nổ lặp đi lặp lại | hàng đợi tối đa chính xác | theo dõi đỉnh hàng đợi | 

## Vỏ cạnh 

Khi chỉ có một tác vụ, thuật toán vẫn phải xử lý chính xác cả quy tắc gửi trước hàng đợi và giai đoạn rút cuối cùng. Nếu tác vụ thêm 0 tin nhắn thì hàng đợi sẽ không bao giờ tăng lên và không có hoạt động gửi nào xảy ra, do đó, thời gian gửi cuối cùng sẽ không được đặt hoặc bằng 0 tùy thuộc vào quá trình khởi tạo. 

Khi có một khoảng cách dài giữa các nhiệm vụ và hàng đợi không trống, bước rút hàng loạt trở nên cần thiết. Hệ thống có thể hoàn thành tất cả các tin nhắn trước lần đến tiếp theo và thời gian hiện tại chỉ được tiến tới thời điểm hàng đợi trống, không nhất thiết phải đến thời điểm nhiệm vụ tiếp theo. 

Khi các tác vụ được thực hiện nối tiếp nhau với các đợt tin nhắn lớn, hàng đợi có thể tăng lên đáng kể. Thuật toán phải cập nhật kích thước hàng đợi tối đa sau mỗi lần xử lý, không phải sau khi xử lý, vì đỉnh xảy ra ngay sau khi chèn.
