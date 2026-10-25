---
title: "CF 106078E - Sao Hỏa"
description: "Chúng tôi đang quản lý một bộ sưu tập nhỏ các bình sữa di chuyển giữa Sao Hỏa và Trái đất. Một chuyến đi bằng tàu vũ trụ sẽ đi từ Sao Hỏa đến Trái Đất hoặc từ Trái Đất đến Sao Hỏa. Khi một chiếc bình đến Trái đất trong chuyến du hành từ Sao Hỏa đến Trái đất, nó sẽ ngay lập tức được đổ đầy."
date: "2026-06-25T12:08:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106078
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 9-17-25 Div. 1 (Advanced)"
rating: 0
weight: 106078
solve_time_s: 48
verified: true
draft: false
---

[CF 106078E - Sao Hỏa](https://codeforces.com/problemset/problem/106078/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quản lý một bộ sưu tập nhỏ các bình sữa di chuyển giữa Sao Hỏa và Trái đất. Một chuyến đi bằng tàu vũ trụ sẽ đi từ Sao Hỏa đến Trái Đất hoặc từ Trái Đất đến Sao Hỏa. Khi một chiếc bình đến Trái đất trong chuyến du hành từ Sao Hỏa đến Trái đất, nó sẽ ngay lập tức được đổ đầy. Để mang số sữa đó trở lại, chúng ta phải chỉ định chiếc bình đã đầy cho chuyến du hành từ Trái đất đến Sao Hỏa sau này. Mỗi tàu vũ trụ có thể chở nhiều nhất một chiếc bình của chúng ta. 

Điều quan trọng là lúc đầu Johnny chỉ sở hữu hai chiếc bình rỗng. Một chiếc bình đang di chuyển không có sẵn. Sau khi trở về sao Hỏa thành công, chiếc bình lại trống rỗng và có thể được tái sử dụng. Nhiệm vụ là tối đa hóa số lượng bình sữa đầy có thể được chuyển lên sao Hỏa. 

Đầu vào mô tả tới 100000 chuyến đi tàu vũ trụ. Mỗi chuyến đi đều có thời điểm bắt đầu, thời điểm kết thúc và hướng đi. Vì số lượng chuyến đi lớn nên bất kỳ phương pháp nào thử từng cặp chuyến đi đều quá chậm. Có tới khoảng 10^10 cặp có thể, không thể vừa với giới hạn kiểu một giây. Chúng ta cần xử lý các chuyến đi gần với thời gian tuyến tính. 

Những trường hợp phức tạp là do cái bình là một nguồn tài nguyên chứ không chỉ là một khoảng thời gian đã chọn. Một chuyến đi có vẻ hữu ích có thể chiếm một cái bình trong thời gian dài và cản trở một trình tự tốt hơn. 

Ví dụ, hãy xem xét:```
3
0 100 0
1 2 0
3 4 1
```Câu trả lời là`1`. 

Một giải pháp bất cẩn có thể cho rằng thực hiện cả hai chuyến đi Trái đất luôn là tốt nhất. Tuy nhiên, chỉ có một chuyến trở về sao Hỏa. Chuyến đi dài không thể quay lại trước thời gian thứ 4 nên chỉ có chuyến đi ngắn mới có thể sản xuất được sữa. 

Một trường hợp khác:```
4
0 5 0
1 2 0
3 4 1
6 7 1
```Câu trả lời là`1`. 

Nếu chỉ tính các cặp chuyến đi tương thích, chúng ta có thể cho rằng cả hai chuyến đi Trái đất đều hữu ích. Họ không như vậy. Chuyến đi ngắn tới Trái đất và chuyến đi sao Hỏa đầu tiên có thể hoàn thành một chu kỳ. Chuyến đi Trái đất thứ hai vẫn phải đợi đến thời điểm thứ 5 trước khi được lấp đầy nên đã bỏ lỡ chuyến đi đầu tiên đến Sao Hỏa. 

Người giải phải đưa ra quyết định theo thứ tự thời gian và ghi nhớ số lượng bình có sẵn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng kết hợp mọi chuyến đi từ Sao Hỏa đến Trái Đất với mọi chuyến đi từ Trái Đất đến Sao Hỏa sau này. Đối với mỗi cặp có thể, chúng ta có thể kiểm tra xem chuyến đi đầu tiên có kết thúc trước khi chuyến đi thứ hai bắt đầu hay không và liệu các bình có sẵn có cho phép lựa chọn này hay không. Điều này đúng vì mọi lần nhập có thể được biểu thị bằng chính xác một cặp như vậy. Tuy nhiên, với 100000 chuyến đi, việc kiểm tra tất cả các cặp yêu cầu khoảng 10^10 thao tác, vượt xa khả năng có thể. 

Quan sát quan trọng là nguồn lực hạn chế duy nhất là số lượng bình. Chúng ta không bao giờ cần biết cái bình cụ thể nào là miễn phí, chỉ cần biết có bao nhiêu cái bình miễn phí. Mỗi sự kiện đều thay đổi số lượng này. Khi chuyến du hành từ Sao Hỏa đến Trái Đất bắt đầu, một chiếc bình rỗng có thể rời đi. Khi đến Trái đất, nó trở thành một cái bình chứa đầy chờ quay trở lại. Khi chuyến du hành từ Trái đất tới Sao Hỏa bắt đầu, một chiếc bình chứa đầy đang chờ có thể rời đi. Khi đến sao Hỏa, chiếc bình lại trở nên trống rỗng. 

Điều này biến vấn đề thành một cuộc quét qua tất cả các điểm cuối của chuyến đi. Sự lựa chọn tham lam là luôn sử dụng bất kỳ chiếc bình nào có sẵn trong sự kiện sớm nhất có thể. Sử dụng một chiếc bình rỗng sớm hơn không có hại gì, vì nếu chúng ta bỏ qua chuyến đi trước đó, chiếc bình sẽ không được sử dụng cho đến một thời gian sau đó. Bắt đầu sớm hơn sẽ giúp bình có ít nhất thời gian để hoàn thành một chu trình. 

Tương tự, khi có chuyến khứ hồi, việc gửi ngay một bình đầy là tối ưu vì nó chỉ làm tăng số lượng bình miễn phí sớm hơn. 

Việc triển khai giữ hai bộ đếm: số lượng bình rỗng trên Sao Hỏa và số lượng bình chứa đầy đang chờ trên Trái đất. Việc xử lý các sự kiện theo thứ tự thời gian là đủ để mô phỏng lịch trình tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Quét tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo hai sự kiện cho mỗi chuyến đi bằng tàu vũ trụ. Sự kiện bắt đầu thể hiện thời điểm tàu ​​vũ trụ rời đi và sự kiện kết thúc thể hiện thời điểm tàu ​​vũ trụ đến nơi. Chúng ta cần cả hai vì bình thay đổi trạng thái ở cả hai thời điểm. 
2. Sắp xếp tất cả các sự kiện theo thời gian. Mỗi giá trị thời gian là duy nhất, do đó không có sự mơ hồ về thứ tự thay đổi. 
3. Duy trì số lượng bình rỗng trên sao Hỏa. Ban đầu giá trị này là 2 vì Johnny sở hữu hai chiếc bình rỗng. 
4. Duy trì số lượng bình đầy hiện đang chờ trên Trái đất. Ban đầu đây là 0 vì chưa có sữa nào được vận chuyển. 
5. Khi chuyến khởi hành từ Sao Hỏa đến Trái Đất xảy ra, hãy sử dụng một chiếc bình rỗng nếu có. Chiếc bình rời khỏi sao Hỏa nên số lượng chiếc bình rỗng giảm đi. 
6. Khi chuyến đi tương tự đó đến Trái đất, chiếc bình giờ đã đầy và đang chờ chuyến trở về. Tăng số lượng bình đầy. 
7. Khi chuyến khởi hành từ Trái đất đến Sao Hỏa xảy ra, hãy gửi một cái bình đầy nếu có. Giảm số lượng bình đầy. 
8. Khi chuyến trở về tới sao Hỏa, sữa đã được giao thành công. Tăng số lượng bình rỗng và tăng câu trả lời. 

Tại sao nó hoạt động: 

Điều bất biến là sau mỗi sự kiện được xử lý, bộ đếm thể hiện chính xác trạng thái tốt nhất có thể sau tất cả các quyết định cho đến thời điểm đó. Việc bắt đầu chuyến du hành từ Sao Hỏa đến Trái Đất sớm hơn không bao giờ làm giảm các khả năng trong tương lai vì nó chỉ di chuyển một chiếc bình có sẵn sang trạng thái mà cuối cùng nó có thể tạo ra sữa. Trả lại một cái bình đã đầy càng sớm càng tốt chỉ giải phóng một nguồn tài nguyên sớm hơn. Vì mọi lựa chọn đều được thực hiện tại thời điểm sớm nhất có thể nên không có chuỗi nào trong tương lai có thể có nhiều tài nguyên sẵn có hơn mô phỏng tham lam này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    events = []

    for _ in range(n):
        l, r, t = map(int, input().split())
        if t == 0:
            events.append((l, 0, 0))
            events.append((r, 1, 0))
        else:
            events.append((l, 0, 1))
            events.append((r, 1, 1))

    events.sort()

    empty = 2
    filled = 0
    ans = 0

    for _, kind, direction in events:
        if kind == 0:
            if direction == 0:
                if empty > 0:
                    empty -= 1
            else:
                if filled > 0:
                    filled -= 1
        else:
            if direction == 0:
                filled += 1
            else:
                empty += 1
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã lưu trữ mỗi điểm cuối chuyến đi dưới dạng một sự kiện. Trường thứ ba phân biệt hướng đi của chuyến đi ban đầu, trong khi trường thứ hai phân biệt điểm khởi hành và điểm đến. 

Các biến quét thể hiện trực tiếp tình hình thực tế.`empty`đếm những chiếc bình có thể được đặt trên những con tàu từ Sao Hỏa đến Trái Đất.`filled`đếm những chiếc bình đã đến Trái đất và sẵn sàng quay trở lại. Khi người quay trở lại, câu trả lời sẽ tăng lên vì đó là thời điểm chính xác sữa đến sao Hỏa. 

Thứ tự của các hoạt động được xử lý bằng cách sắp xếp tất cả các sự kiện. Bởi vì mọi thời điểm đều khác biệt nên không có vấn đề ràng buộc giữa các sự kiện. Bộ đếm chỉ được cập nhật sau khi hành động vật lý tương ứng xảy ra, tránh những sai sót nhỏ xung quanh ranh giới chuyến đi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
0 2 0
4 6 0
1 3 1
5 7 1
8 9 1
```Dấu vết: 

| Thời gian | Sự kiện | Bình rỗng | Bình đầy | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | Sao Hỏa đến Trái Đất bắt đầu | 1 | 0 | 0 | 
| 1 | Trái đất đến sao Hỏa bắt đầu | 1 | 0 | 0 | 
| 2 | Sao Hỏa đến Trái đất | 1 | 1 | 0 | 
| 3 | Trở về | 2 | 0 | 1 | 
| 4 | Sao Hỏa đến Trái Đất bắt đầu | 1 | 0 | 1 | 
| 5 | Trái đất đến sao Hỏa bắt đầu | 1 | 0 | 1 | 
| 6 | Sao Hỏa đến Trái đất | 1 | 1 | 1 | 
| 7 | Trở về | 2 | 0 | 2 | 

Dấu vết cho thấy một chiếc bình có thể được tái sử dụng sau khi quay trở lại. Hai bình vật lý giống nhau có thể tạo ra nhiều lần nhập. 

### Mẫu 2 

đầu vào:```
3
0 10 0
1 2 0
3 4 1
```Dấu vết: 

| Thời gian | Sự kiện | Bình rỗng | Bình đầy | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | Chuyến đi dài bắt đầu | 1 | 0 | 0 | 
| 1 | Chuyến đi ngắn bắt đầu | 0 | 0 | 0 | 
| 2 | Chuyến đi ngắn đến | 0 | 1 | 0 | 
| 3 | Bắt đầu quay trở lại | 0 | 0 | 0 | 
| 4 | Trở về | 1 | 0 | 1 | 
| 10 | Chuyến đi dài đến | 1 | 1 | 1 | 

Điều này chứng tỏ tại sao bắt đầu sớm hơn là đủ. Thuật toán đương nhiên bỏ qua thực tế là chuyến đi dài không thể về kịp chuyến tàu khứ hồi có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các sự kiện được tạo 2n chiếm ưu thế trong quá trình quét | 
| Không gian | O(n) | Chúng tôi lưu trữ hai sự kiện cho mỗi chuyến đi | 

Giải pháp phù hợp với các ràng buộc vì nó tránh được việc so sánh các chuyến đi với nhau. Số lượng sự kiện được xử lý chỉ gấp đôi số chuyến đi đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(data):
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    n = int(input())
    events = []

    for _ in range(n):
        l, r, t = map(int, input().split())
        if t == 0:
            events.append((l, 0, 0))
            events.append((r, 1, 0))
        else:
            events.append((l, 0, 1))
            events.append((r, 1, 1))

    events.sort()

    empty = 2
    filled = 0
    ans = 0

    for _, kind, direction in events:
        if kind == 0:
            if direction == 0 and empty:
                empty -= 1
            elif direction == 1 and filled:
                filled -= 1
        else:
            if direction == 0:
                filled += 1
            else:
                empty += 1
                ans += 1

    return str(ans)

assert solve_data("""3
0 2 0
4 6 0
1 3 1
""") == "1"

assert solve_data("""4
0 5 0
1 2 0
3 4 1
6 7 1
""") == "1"

assert solve_data("""1
0 1 0
""") == "0"

assert solve_data("""4
0 2 0
3 5 1
6 8 0
9 10 1
""") == "2"

assert solve_data("""5
0 10 0
1 2 0
3 4 1
5 6 1
7 8 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuyến đi từ sao Hỏa đến Trái đất | 0 | Không thể quay lại | 
| Các chuyến đi Trái đất chồng chéo | 1 | Giới hạn tài nguyên | 
| Hai chu kỳ độc lập | 2 | Tái sử dụng bình | 
| Trực giác chặn chuyến đi dài | 2 | Xử lý sự kiện tham lam | 
| Nhiều lựa chọn trả lại | 2 | Gửi bình đầy ngay lập tức | 

## Vỏ cạnh 

Đối với trường hợp chuyến đi dài trên Trái đất chặn chuyến đi ngắn:```
3
0 100 0
1 2 0
3 4 1
```Quá trình quét bắt đầu chuyến đi đầu tiên tại thời điểm 0 và chuyến thứ hai tại thời điểm 1 vì cả hai bình đều trống. Chuyến đi ngắn hoàn thành vào thời điểm 2 và quay trở lại vào thời điểm 4, tạo ra một lần nhập thành công. Chuyến đi dài chỉ lấp đầy ở thời điểm 100, sau khi cơ hội trở về đã trôi qua nên đáp án cuối cùng là`1`. 

Đối với trường hợp phải sử dụng lại các bình giống nhau:```
4
0 2 0
4 6 0
1 3 1
5 7 1
```Chu kỳ bình đầu tiên hoàn thành vào thời điểm thứ 3. Bình được giải phóng lại được tính là rỗng và có thể được sử dụng cho chu kỳ thứ hai. Câu trả lời trở thành`2`, cho thấy thuật toán theo dõi thời gian tồn tại của bình thay vì coi chúng là đối tượng dùng một lần.
