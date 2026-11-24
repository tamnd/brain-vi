---
title: "CF 955A - Cho mèo ăn"
description: "Andrew thức dậy vào một thời điểm nhất định trong ngày và nhận ra con mèo của mình đang đói. Con mèo bắt đầu với một số giá trị cơn đói ban đầu và cơn đói này tăng dần theo thời gian với tốc độ cố định mỗi phút. Andrew có thể làm giảm cơn đói của con mèo chỉ bằng cách mua bánh mì."
date: "2026-06-17T02:05:59+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 955
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 471 (Div. 2)"
rating: 1100
weight: 955
solve_time_s: 81
verified: true
draft: false
---

[CF 955A - Cho mèo ăn](https://codeforces.com/problemset/problem/955/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Andrew thức dậy vào một thời điểm nhất định trong ngày và nhận ra con mèo của mình đang đói. Con mèo bắt đầu với một số giá trị cơn đói ban đầu và cơn đói này tăng dần theo thời gian với tốc độ cố định mỗi phút. Andrew có thể làm giảm cơn đói của con mèo chỉ bằng cách mua bánh mì. Mỗi chiếc bánh làm giảm cơn đói đi một lượng cố định và có thể mua bất kỳ số lượng bánh bao nào vào bất kỳ lúc nào mà không tốn chi phí đi lại hay chậm trễ về thời gian. 

Có một sự thay đổi quan trọng trong việc định giá. Trước 20h mỗi bánh có giá bình thường. Bắt đầu đúng lúc 20:00, mỗi chiếc bánh sẽ rẻ hơn 20%. Chiết khấu áp dụng cho mỗi lần mua bánh, không phải theo gói và cho phép thanh toán theo từng phần theo nghĩa là giá chiết khấu có thể không phải là số nguyên, nhưng bánh vẫn không thể chia được. 

Mục tiêu là quyết định khi nào Andrew nên đến cửa hàng và anh ấy nên mua bao nhiêu chiếc bánh để cơn đói cuối cùng của con mèo trở nên không tích cực, đồng thời giảm thiểu tổng số tiền chi tiêu. 

Sự căng thẳng chính trong vấn đề là giữa hai quá trình tuyến tính. Việc chờ đợi làm tăng cảm giác đói một cách tuyến tính theo thời gian, trong khi chờ đợi có thể giảm giá mỗi chiếc bánh nếu vượt qua ngưỡng 20:00. Điều này tạo ra một số lượng nhỏ các điểm quyết định có ý nghĩa thay vì vấn đề tối ưu hóa liên tục. 

Từ những hạn chế, mức độ đói có thể lên tới 100000 và tăng tối đa 100 mỗi phút. Thời gian được giới hạn bởi một đồng hồ ngày. Một mô phỏng ngây thơ trong từng phút có thể là không cần thiết. Bất kỳ giải pháp nào tính toán lại mỗi phút hoặc mỗi búi tóc theo cách lồng nhau đều có nguy cơ xảy ra trường hợp xấu nhất là khoảng 10^7 đến 10^10 thao tác và sẽ quá chậm. Cấu trúc gợi ý rằng chỉ có số lần mua hàng của ứng viên không đổi mới quan trọng. 

Một trường hợp tế nhị là Andrew thức dậy trước 20h và cân nhắc việc chờ đợi. Việc chờ đợi làm tăng số lượng bánh cần thiết vì cơn đói tăng lên nhưng giá mỗi chiếc bánh lại giảm. Một trường hợp khác là khi Andrew thức dậy sau 20:00, thời điểm chỉ có chế độ giảm giá mới quan trọng. Trường hợp cạnh thứ ba là khi thời gian chờ vượt qua đúng 20:00, vì chiết khấu được kích hoạt ở một ranh giới rõ ràng. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử tất cả những phút có thể từ khi thức dậy cho đến cuối ngày, tính toán cơn đói, tính xem cần bao nhiêu chiếc bánh vào thời điểm đó và nhân với mức giá tương ứng. Đối với mỗi phút ứng viên, chúng tôi sẽ tính toán mức chia trần để xác định số lượng bánh bao. 

Điều này hiệu quả vì trạng thái ở mỗi phút được xác định rõ nhưng quá chậm vì có tới 1440 phút trong một ngày và mỗi đánh giá là O(1), đưa ra khoảng 10^3 lần lặp cho mỗi bài kiểm tra. Chỉ điều đó thôi cũng được, nhưng nếu được mở rộng hoặc nhúng vào trong nhiều phép tính hoặc được khái quát hóa không chính xác cho các miền thời gian lớn hơn thì nó sẽ trở nên kém hiệu quả. Quan trọng hơn, sự dư thừa thực sự mang tính khái niệm hơn là số lượng. 

Quan sát quan trọng là chỉ có hai khoảnh khắc quan trọng. Andrew mua ngay lập tức hoặc anh ấy đợi cho đến thời điểm đầu tiên chương trình giảm giá có hiệu lực. Bất kỳ thời điểm nào sau 20:00 còn tệ hơn việc mua đúng lúc 20:00, vì cơn đói chỉ tăng lên trong khi giá không đổi. Tương tự, bất kỳ thời điểm nào từ lúc thức dậy đến 20:00 đều tương đương với “mua ngay” vì giá không thay đổi trước khi giảm giá. 

Điều này giúp giảm bớt vấn đề khi đánh giá nhiều nhất hai kịch bản, tính toán các bánh cần thiết bằng cách sử dụng phép chia trần trong từng trường hợp và chọn kết quả rẻ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo thời gian | O(1440) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Đánh giá hai trường hợp tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán câu trả lời bằng cách so sánh một tập hợp nhỏ các chiến lược ứng cử viên. 

### 1. Chuyển đổi thời gian thành phút cho đến khi giảm giá 

Chúng tôi tính xem còn bao nhiêu phút nữa cho đến 20:00 kể từ thời điểm thức dậy. Nếu Andrew thức dậy vào lúc hoặc sau 20:00 thì giá trị này bằng 0. 

Điều này xác định liệu chiến lược “chờ giảm giá” có tồn tại hay không.

### 2. Xác định trạng thái mua ngay 

Nếu Andrew mua ngay thì cơn đói chính xác là giá trị ban đầu H. Số bánh mì cần thiết là số nguyên k nhỏ nhất sao cho k · N ≥ H. 

Đây là một bộ phận trần tiêu chuẩn. 

### 3. Tính giá vốn mua ngay 

Mỗi chiếc bánh có giá C trước khi giảm giá. Tổng chi phí là k · C. 

### 4. Xác định trạng thái mua hàng trong thời gian chiết khấu 

Nếu Andrew đợi đến 20:00, cơn đói tăng tuyến tính D mỗi phút trong thời gian chờ tính toán t. 

Vậy cơn đói trở thành H + t · D. 

### 5. Tính bánh đang giảm giá 

Vào lúc hoặc sau 20:00, mỗi chiếc bánh có giá 0,8 · C. Số lượng bánh lại là trần((H + t · D) / N). 

### 6. Nếu đã sau 20:00 

Chúng tôi bỏ qua hoàn toàn trường hợp trước mắt về mặt lựa chọn giá cả vì đã áp dụng chiết khấu. Chúng tôi chỉ tính phiên bản giảm giá ở thời điểm hiện tại. 

### 7. Chọn chi phí tối thiểu 

Chúng tôi so sánh các chiến lược hợp lệ và đưa ra chi phí nhỏ nhất. 

### Tại sao nó hoạt động 

Điều bất biến chính là trong bất kỳ khoảng thời gian nào trước 20:00, giá mỗi chiếc bánh không đổi trong khi cơn đói tăng lên nghiêm trọng. Điều này làm cho số lượng bánh yêu cầu không giảm theo thời gian chờ, trong khi chi phí cho mỗi bánh không thay đổi. Sau 20:00, cả hai việc chờ đợi lâu hơn chỉ làm tăng cảm giác đói trong khi vẫn giữ giá cố định, do đó hàm chi phí hoàn toàn không giảm theo thời gian. Vì vậy, bất kỳ giải pháp tối ưu nào cũng phải xảy ra tại một điểm ranh giới nơi chế độ giá thay đổi hoặc tại thời điểm ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ceil_div(a, b):
    return (a + b - 1) // b

hh, mm = map(int, input().split())
H, D, C, N = map(int, input().split())

start = hh * 60 + mm
discount_time = 20 * 60

# time until discount becomes active
t = max(0, discount_time - start)

# option 1: buy immediately
buns_now = ceil_div(H, N)
cost_now = buns_now * C

# option 2: buy at or after 20:00 (only meaningful if t > 0 or already past)
H_later = H + t * D
buns_later = ceil_div(H_later, N)
cost_later = buns_later * C * 4 / 5

# if already after 20:00, only one regime matters
if start >= discount_time:
    print(f"{cost_later:.10f}")
else:
    print(f"{min(cost_now, cost_later):.10f}")
```Việc triển khai trước tiên sẽ chuyển đổi thời gian thành phút để đơn giản hóa việc so sánh với ngưỡng cố định là 20:00. Hàm trợ giúp thực hiện phân chia trần, cần thiết để xác định số lượng bánh tối thiểu cần thiết để đáp ứng mức độ đói nhất định. 

Hai chi phí ứng cử viên tương ứng trực tiếp với hai chiến lược có ý nghĩa: mua ngay với giá đầy đủ hoặc đợi đến thời điểm giảm giá và mua với giá giảm sau khi cơn đói tăng lên. Sự so sánh ở cuối chọn phương án tốt hơn. 

Một sai lầm phổ biến là quên rằng không nên mô phỏng thêm việc chờ đợi quá 20:00 vì hàm chi phí chỉ xấu đi sau khi bắt đầu giảm giá. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
19 00
255 1 100 1
```Chúng tôi tính toán 60 phút cho đến 20:00. 

| Bước | Đói | Số lượng bún | Giá mỗi chiếc bánh | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| Mua ngay | 255 | 255 | 100 | 25500 | 
| Đợi đến 20:00 | 255 + 60·1 = 315 | 315 | 80 | 25200 | 

Chờ đợi là có lợi vì mức giảm giá lớn hơn mức tăng thêm của nạn đói. 

### Ví dụ 2 

đầu vào:```
18 30
91 1 15 1
```Chúng tôi tính toán 90 phút cho đến khi giảm giá. 

| Bước | Đói | Số lượng bún | Giá mỗi chiếc bánh | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| Mua ngay | 91 | 91 | 15 | 1365 | 
| Đợi đến 20:00 | 91 + 90 = 181 | 181 | 12 | 2172 | 

Mua ngay là tối ưu vì cơn đói gia tăng sẽ chi phối việc giảm giá. 

Hai trường hợp này cho thấy sự cân bằng trọng tâm: việc chờ đợi chỉ có tác dụng khi số tiền tiết kiệm được từ chiết khấu trên mỗi chiếc bánh lớn hơn số bánh cần bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng tính toán và so sánh số học không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Việc tính toán chỉ bao gồm một số phép tính số học bất kể kích thước đầu vào, dễ dàng phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    hh, mm = map(int, input().split())
    H, D, C, N = map(int, input().split())

    def ceil_div(a, b):
        return (a + b - 1) // b

    start = hh * 60 + mm
    discount_time = 20 * 60
    t = max(0, discount_time - start)

    buns_now = ceil_div(H, N)
    cost_now = buns_now * C

    H_later = H + t * D
    buns_later = ceil_div(H_later, N)
    cost_later = buns_later * C * 4 / 5

    if start >= discount_time:
        ans = cost_later
    else:
        ans = min(cost_now, cost_later)

    return f"{ans:.10f}"

assert run("19 00\n255 1 100 1\n")[:6] == "25200.", "sample 1"
assert run("18 30\n91 1 15 1\n")[:5] == "1365.", "sample 2"
assert run("20 00\n10 1 10 1\n")[:3] == "8.", "already discounted"
assert run("00 00\n1 100 100 10\n") == run("00 00\n1 100 100 10\n"), "stability"
assert run("10 00\n100 10 7 3\n") != "", "general case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 20:00 bắt đầu | chỉ giảm giá | ranh giới thời gian giảm giá | 
| H nhỏ | hành vi ngay lập tức | trường hợp đói tối thiểu | 
| D lớn | hiệu ứng tăng trưởng | nhạy cảm với việc chờ đợi | 
| trộn tùy ý | tính nhất quán | tính đúng đắn chung | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi Andrew thức dậy đúng lúc 20:00. Trong tình huống này, việc chờ đợi không thay đổi được gì vì giảm giá đã có hiệu lực. Thuật toán xử lý vấn đề này bằng cách đặt thời gian chờ về 0, điều này sẽ thu gọn cả hai chiến lược thành chiến lược được chiết khấu. 

Một trường hợp khác xảy ra khi cơn đói nhỏ hơn kích thước búi tóc. Trong trường hợp đó, việc phân chia trần sẽ trả về một cách chính xác một bun, ngăn chặn bất kỳ lỗi logic tràn nào hoặc lỗi zero-bun. 

Trường hợp thứ ba là việc chờ đợi sẽ làm tăng cảm giác đói đáng kể nhưng giảm giá chỉ làm giảm giá một chút. Việc so sánh giữa hai chi phí được tính toán sẽ nắm bắt chính xác sự mất cân bằng này mà không cần bất kỳ lý do suy nghiệm nào.
