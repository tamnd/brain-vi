---
title: "CF 102281E - \u0418\u043d\u043d\u043e\u0432\u0430\u0446\u0438\u043e\u043d\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng tôi bắt đầu với n robot sửa chữa và m hư hỏng nano độc lập. Trong một giây, mọi robot hiện có đều chọn chính xác một hành động. Nó sẽ sửa chữa một hư hỏng hoặc dành thời gian thứ hai để tạo ra một robot mới. Một robot mới được tạo sẽ có sẵn từ giây tiếp theo."
date: "2026-08-13T09:22:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "E"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 98
verified: true
draft: false
---

[CF 102281E - \u0418\u043d\u043d\u043e\u0432\u0430\u0446\u0438\u043e\u043d\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với`n`sửa chữa robot và`m`hư hỏng nano độc lập. Trong một giây, mọi robot hiện có đều chọn chính xác một hành động. Nó sẽ sửa chữa một hư hỏng hoặc dành thời gian thứ hai để tạo ra một robot mới. Một robot mới được tạo sẽ có sẵn từ giây tiếp theo. 

Đầu ra phải mô tả một lịch trình tối ưu. Cứ mỗi giây chúng tôi in ra có bao nhiêu robot hiện có sao chép. Tất cả các robot còn lại sẽ sửa chữa những hư hỏng trong giây đó. Dòng đầu tiên là số giây tối thiểu cần thiết. Nếu không có cách nào để sửa chữa số lượng hư hỏng cần thiết, chúng tôi sẽ in`IMPOSSIBLE`. 

Tuyên bố ban đầu cho phép cả hai`n`Và`m`lớn như`10^100`, vì vậy số nguyên máy thông thường trong các ngôn ngữ có số học có độ rộng cố định là không đủ. Các số nguyên có độ chính xác tùy ý của Python đặc biệt thuận tiện ở đây. Giới hạn thời gian cũng cho chúng ta biết rằng chúng ta không thể mô phỏng mọi số lượng robot có thể có hoặc mọi lịch trình có thể có. Câu trả lời hữu ích sẽ chỉ có nhiều giây theo logarit, vì số lượng robot có thể tăng gấp đôi. 

Có một số trường hợp khó xử lý. Nếu đầu vào là`0 0`, con tàu đã không bị hư hại gì nên đáp án đúng là`0`, không có dòng lệnh. Việc triển khai bất cẩn luôn in ít nhất một giây sẽ không tối ưu. 

Nếu đầu vào là`0 1`, không có robot nào có khả năng sửa chữa bất cứ thứ gì và không có cách nào tạo ra robot đầu tiên nên kết quả đầu ra đúng là`IMPOSSIBLE`. Việc triển khai tính toán logarit một cách mù quáng hoặc liên tục nhân đôi số lượng robot có thể bị kẹt hoặc chia cho 0. 

Nếu đầu vào là`10 10`, một giây là đủ. Chúng ta hoàn toàn không cần sao chép nên lệnh có thể`0`. Chiến lược luôn bắt đầu bằng cách sao chép sẽ sử dụng hai giây và sẽ mất đi tính tối ưu. 

Nếu đầu vào là`10 11`, một giây là không đủ, trong khi hai giây là đủ. Chúng ta có thể để tất cả mười robot sao chép trong giây đầu tiên, thu được 20 robot và sau đó sử dụng chính xác 11 robot trong số đó để sửa chữa trong giây thứ hai. Trình tự lệnh là`10 9`. Một lỗi phổ biến là xuất`10 0`, sửa chữa 20 thiệt hại và không phải là lịch trình hợp lệ chỉ cho 11 thiệt hại hiện có. 

Tuyên bố Codeforces được lưu trữ xác nhận`10^100`giới hạn và hai trường hợp mẫu được sử dụng dưới đây. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể thử mọi số lượng robot có thể sao chép trong mỗi giây. Nếu có`R`robot ở đầu giây, có`R + 1`những lựa chọn, từ sao chép không ai đến sao chép mọi người. Không gian trạng thái phát triển cực kỳ nhanh chóng. Mặc dù mỗi bang đều có ít nhất`n + 1`những lựa chọn có thể, một cuộc tìm kiếm theo chiều sâu`t`đã chứa ít nhất`(n + 1)^t`lịch trình. Số lượng lịch trình chính xác có thể được mô tả đệ quy bằng`S(0, R) = 1`Và`S(t, R) = sum(S(t - 1, R + x))`vì`0 <= x <= R`. 

Sự phân nhánh thực tế lớn hơn khi dân số tăng lên. Với những số có một trăm chữ số, cách tiếp cận này không khả thi chút nào. 

Lực lượng vũ phu hoạt động vì nó xem xét rõ ràng mọi sự cân bằng có thể có giữa công việc sửa chữa hiện tại và nhân rộng trong tương lai. Nó thất bại vì có quá nhiều sự đánh đổi như vậy. Quan sát quan trọng là đối với một số giây còn lại cố định, việc sao chép sẽ có giá trị hơn khi được thực hiện sớm hơn. 

Giả sử có`R`robot và chính xác`t`giây còn lại, với`t >= 2`. Nếu chúng ta làm`x`robot sẽ sao chép trong giây hiện tại, sau đó`R - x`robot sửa chữa hư hỏng ngay lập tức và`R + x`robot vẫn còn cho phần tiếp theo`t - 1`giây. Giả sử một cách đệ quy rằng số lượng tối đa có thể sửa chữa được trong`t - 1`giây từ`R + x`robot là`(R + x) * 2^(t-2)`. Tổng số trở thành`R - x + (R + x) * 2^(t-2)`có thể được sắp xếp lại thành`R * (1 + 2^(t-2)) + x * (2^(t-2) - 1)`. 

Vì`t >= 2`, hệ số của`x`không âm nên lựa chọn tốt nhất là`x = R`. Nói cách khác, khi còn ít nhất hai giây, mọi robot sẽ sao chép ngay lập tức. 

Điều này đưa ra một công thức đơn giản. Với`R`robot và`t`giây có sẵn, số lượng thiệt hại tối đa có thể được sửa chữa là`R * 2^(t-1)`. 

Có thể đạt được điều này bằng cách tích cực sử dụng bản sao và để lại giây cuối cùng để sửa chữa. Tổng quát hơn, một khi chúng ta biết rằng`m <= n * 2^(t-1)`, chúng ta có thể làm cái đầu tiên`t - 1`giây sao chép thuần túy. Tại thời điểm đó có chính xác`n * 2^(t-1)`robot. Trong giây cuối cùng, chúng ta chọn chính xác đủ số robot để sao chép sao cho số robot còn lại bằng nhau`m`. Như vậy tất cả`m`hư hỏng được sửa chữa chính xác. 

Vấn đề bây giờ đã trở thành việc tìm kiếm nhỏ nhất`t`thỏa mãn`n * 2^(t-1) >= m`. 

Chúng tôi thậm chí không cần tìm kiếm nhị phân. Bắt đầu với`capacity = n`Và`t = 1`, liên tục nhân đôi`capacity`và tăng dần`t`cho đến khi`capacity >= m`. Từ`m <= 10^100`, việc này chỉ mất vài trăm lần lặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ít nhất`Ω((n+1)^t)`lịch trình | Hàm mũ | Quá chậm | 
| Tối ưu |`O(log m)`các phép toán số nguyên lớn |`O(log m)`không gian đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`dưới dạng số nguyên có độ chính xác tùy ý. Nếu như`m = 0`, con tàu đã được sửa chữa rồi, vậy nên hãy thiết lập`t = 0`và không xuất ra lệnh nào. 
2. Nếu`n = 0`trong khi`m > 0`, đầu ra`IMPOSSIBLE`. Không có robot nào có thể sửa chữa hư hỏng hoặc tạo ra robot khác. 
3. Nếu không thì bắt đầu với`t = 1`Và`capacity = n`. Đây`capacity`đại diện cho số lượng robot tối đa có thể thực hiện sửa chữa trong giây cuối cùng của`t`- Lịch trình tối ưu thứ hai 
4. Trong khi`capacity < m`, gấp đôi`capacity`và tăng dần`t`. Mỗi giây thêm có thể tăng gấp đôi số lượng robot có sẵn cho giây sửa chữa cuối cùng, do đó công suất chính xác như sau`n * 2^(t-1)`. 
5. Đầu tiên`t - 1`giây là bản sao thuần túy. Trong lần đầu tiên thứ hai như vậy tất cả`n`robot sẽ nhân rộng trong thời gian tiếp theo`2n`robot sao chép, sau đó tất cả`4n`robot sao chép, v.v. Do đó, các lệnh sao chép là`n, 2n, 4n, ...`. 
6. Vào đầu giây cuối cùng có`capacity`robot. Chúng tôi muốn chính xác`m`robot để sửa chữa những hư hỏng còn lại, chính xác là`capacity - m`robot nên nhân rộng. Do đó, lệnh cuối cùng là`capacity - m`. 
7. Đầu ra`t`và trình tự kết quả. Mỗi lệnh nằm trong khoảng từ 0 đến số lượng robot hiện có, do đó lịch trình có giá trị về mặt vật lý. 

### Tại sao nó hoạt động 

Điều bất biến là sau`k`giây sao chép ban đầu, số lượng robot chính xác là`n * 2^k`. Sự lặp lại ở trên chứng minh rằng đối với bất kỳ số giây cố định nào, việc sao chép mọi robot có sẵn trong giây hiện tại là tối ưu bất cứ khi nào còn lại ít nhất hai giây. Do đó, không có lịch trình`t`giây có thể sửa chữa nhiều hơn`n * 2^(t-1)`thiệt hại. Thuật toán chọn giá trị nhỏ nhất`t`mà giới hạn trên này đạt tới`m`, vì vậy không thể tồn tại câu trả lời nhỏ hơn. Lịch trình xây dựng của nó đạt đến chính xác`m`sửa chữa bằng cách chỉ điều chỉnh đến giây cuối cùng, điều này chứng tỏ vừa khả thi vừa tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    if m == 0:
        print(0)
        return

    if n == 0:
        print("IMPOSSIBLE")
        return

    t = 1
    capacity = n

    while capacity < m:
        capacity *= 2
        t += 1

    commands = []
    robots = n

    for _ in range(t - 1):
        commands.append(robots)
        robots *= 2

    commands.append(robots - m)

    print(t)
    print(*commands)

if __name__ == "__main__":
    solve()
```Hai điều kiện đầu tiên xử lý hai trường hợp 0 ​​trước khi xảy ra bất kỳ sự nhân đôi nào. Nếu không có thiệt hại, 0 giây là tối ưu. Nếu có hư hỏng nhưng không có robot thì việc sao chép và sửa chữa đều không thể thực hiện được. 

Vòng lặp duy trì`capacity = n * 2^(t-1)`. Nó dừng ở giá trị đầu tiên của`t`mà lần sửa chữa cuối cùng có đủ robot để bù đắp mọi thiệt hại. Các số nguyên của Python có thể biểu diễn chính xác tất cả các giá trị trung gian, bao gồm cả các giá trị nằm ngoài`10^100`. 

Việc xây dựng lệnh phản ánh bằng chứng.`robots`là số lượng robot trước giây hiện tại. Đối với mỗi cái đầu tiên`t - 1`giây, mọi robot đều sao chép, nên lệnh là`robots`và dân số tăng gấp đôi. 

Vào giây cuối cùng,`robots`bằng`capacity`. In ấn`robots - m`có nghĩa là nhiều robot này sẽ sao chép, để lại chính xác`m`robot để sửa chữa. Phép trừ cuối cùng này cũng là lý do tại sao chúng ta không thể in số 0 cho mỗi lệnh sao chép. Kế hoạch không được sửa chữa nhiều thiệt hại hơn mức thực tế tồn tại. 

Vì`t = 1`, vòng lặp tạo tiền tố sao chép sẽ thực hiện 0 lần. Lệnh duy nhất trở thành`n - m`, điều này đúng vì tất cả`n - (n-m) = m`robot sửa chữa trong thời gian duy nhất có sẵn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`n = 10`Và`m = 30`, một giây chỉ cho phép sửa chữa mười lần, vì vậy chúng ta cần thêm một giây nữa. Hai giây cho tối đa hai mươi lần sửa chữa, vẫn chưa đủ. Ba giây cho tối đa bốn mươi lần sửa chữa. 

Thuật toán xây dựng một lịch trình sử dụng hai giây đầu tiên để sao chép và sau đó điều chỉnh giây cuối cùng để sửa chữa chính xác 30 hư hỏng. 

| Bước |`t`|`robots`|`capacity`| Lệnh | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 10 | 10 | | 
| Sau khi kiểm tra năng lực | 2 | 10 | 20 | | 
| Sau khi kiểm tra năng lực | 3 | 10 | 40 | | 
| Nhân bản thứ hai 1 | | 20 | | 10 | 
| Bản sao thứ hai 2 | | 40 | | 20 | 
| Sửa chữa cuối cùng thứ hai | | 40 | 40 | 10 | 

Đầu ra có thể là```
3
10 20 10
```Trong hai giây đầu tiên không có hư hỏng nào được sửa chữa. Vào đầu giây thứ ba, có bốn mươi robot, trong đó có mười robot nhân bản và ba mươi robot sửa chữa. Như vậy tất cả ba mươi thiệt hại sẽ biến mất chính xác vào cuối giây thứ ba. Mẫu chính thức sử dụng một lịch trình khác nhưng tối ưu không kém,`0 10 0`. 

### Mẫu 2 

cho`n = 15`Và`m = 70`, dung lượng cho một, hai, ba và bốn giây là mười lăm, ba mươi, sáu mươi và một trăm hai mươi. Do đó bốn giây là cần thiết. 

Lịch trình được xây dựng sẽ tăng gấp đôi dân số trong ba giây đầu tiên và sau đó sử dụng giây cuối cùng cho đúng bảy mươi lần sửa chữa. 

| Bước |`t`|`robots`|`capacity`| Lệnh | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 15 | 15 | | 
| Công suất tăng gấp đôi | 2 | 15 | 30 | | 
| Công suất tăng gấp đôi | 3 | 15 | 60 | | 
| Công suất tăng gấp đôi | 4 | 15 | 120 | | 
| Nhân bản thứ hai 1 | | 30 | | 15 | 
| Bản sao thứ hai 2 | | 60 | | 30 | 
| Nhân bản thứ 3 | | 120 | | 60 | 
| Sửa chữa cuối cùng thứ hai | | 120 | 120 | 50 | 

Đầu ra có thể là```
4
15 30 60 50
```Lệnh cuối cùng yêu cầu 50 robot tái tạo, để lại 70 robot sửa chữa 70 hư hỏng. Thay vào đó, mẫu chính thức sử dụng`10 20 0 0`, cũng kết thúc sau bốn giây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(log m)`lặp đi lặp lại | Dung lượng tăng gấp đôi sau mỗi lần lặp cho đến khi đạt`m`. | 
| Không gian |`O(log m)`không gian đầu ra | Nhiều nhất một lệnh được tạo ra cho mỗi giây và số giây là logarit. | 

Vì`m <= 10^100`, số giây nhiều nhất là khoảng 334 khi`n >= 1`. Mỗi lệnh chỉ dài vài trăm chữ số thập phân. Số học có độ chính xác tùy ý của Python xử lý trực tiếp các giá trị này, do đó việc tính toán rất nhỏ so với giới hạn 1,5 giây và 128 MB. 

## Trường hợp thử nghiệm 

Trình trợ giúp kiểm tra bên dưới sử dụng cấu trúc xác định chính xác từ giải pháp, do đó, các chuỗi dự kiến có thể được so sánh trực tiếp. Hai mẫu chính thức được đưa vào lịch trình do quá trình triển khai này tạo ra thay vì các lịch trình cụ thể được trình bày trong tuyên bố, vì bài toán rõ ràng cho phép bất kỳ lịch trình tối ưu nào.```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n, m = map(int, sys.stdin.readline().split())

        if m == 0:
            print(0)
            return sys.stdout.getvalue()

        if n == 0:
            print("IMPOSSIBLE")
            return sys.stdout.getvalue()

        t = 1
        capacity = n

        while capacity < m:
            capacity *= 2
            t += 1

        commands = []
        robots = n

        for _ in range(t - 1):
            commands.append(robots)
            robots *= 2

        commands.append(robots - m)

        print(t)
        print(*commands)

        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def run(inp: str) -> str:
    return solve_io(inp)

# Provided sample 1, using our valid optimal schedule.
assert run("10 30\n") == "3\n10 20 10\n", "sample 1"

# Provided sample 2, using our valid optimal schedule.
assert run("15 70\n") == "4\n15 30 60 50\n", "sample 2"

# Minimum-size feasible input.
assert run("0 0\n") == "0\n", "empty ship"

# Impossible case: there is damage but no robot.
assert run("0 1\n") == "IMPOSSIBLE\n", "impossible"

# Exactly one second is enough.
assert run("10 10\n") == "1\n0\n", "one-second boundary"

# Just one damage beyond the initial number of robots.
assert run("10 11\n") == "2\n10 9\n", "first doubling boundary"

# Maximum-size equal values, which should still take one second.
assert run("10" + "0" * 99 + " " + "10" + "0" * 99 + "\n") == (
    "1\n0\n"
), "maximum equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| Đầu vào tối thiểu có thể và câu trả lời không giây | 
|`0 1`|`IMPOSSIBLE`| Không có robot ban đầu và thiệt hại tích cực | 
|`10 10`|`1`theo sau là`0`| Ranh giới nơi dân số ban đầu đủ chính xác | 
|`10 11`|`2`theo sau là`10 9`| Trường hợp đầu tiên yêu cầu sao chép và sửa chữa cuối cùng chính xác | 
|`10^100 10^100`|`1`theo sau là`0`| Số nguyên có kích thước tối đa và số học có độ chính xác tùy ý | 

## Vỏ cạnh 

cho`0 0`, thuật toán sẽ thấy ngay`m == 0`và in`0`. Không cần thứ hai vì không có gì để sửa chữa. Chuỗi lệnh trống, phù hợp với yêu cầu phải có chính xác`t`lệnh. 

Vì`0 1`, thuật toán đạt đến`n == 0`điều kiện sau khi xác minh rằng thiệt hại tồn tại. Vì không có robot nào tồn tại nên việc sửa chữa không thể xảy ra trong bất kỳ giây nào trong tương lai, vì vậy`IMPOSSIBLE`là kết quả duy nhất có thể xảy ra. 

Vì`10 10`, dung lượng ban đầu đã là 10 nên vòng lặp không bao giờ chạy. chúng tôi có`t = 1`, và lệnh cuối cùng là`10 - 10 = 0`. Tất cả mười robot đều sửa chữa một hư hỏng trong mỗi giây đó, đưa ra chính xác mười lần sửa chữa. 

Vì`10 11`, dung lượng ban đầu của mười là quá nhỏ nên nó được nhân đôi lên hai mươi và`t`trở thành hai. Lệnh đầu tiên là`10`, sản xuất 20 robot. Lệnh cuối cùng là`20 - 11 = 9`, thế là chín robot nhân bản và mười một robot còn lại sửa chữa. Con tàu được sửa chữa đúng vào cuối giây thứ hai. 

Vì`10^100 10^100`, không cần nhân đôi. Python đọc cả hai số dưới dạng số nguyên có độ chính xác tùy ý, so sánh chúng trực tiếp và tạo ra`1`theo sau là`0`. Thuật toán không bao giờ chuyển đổi các giá trị thành dấu phẩy động, do đó không làm mất độ chính xác. 

Việc xây dựng cũng xử lý các trường hợp`m`nằm chặt chẽ giữa hai lũy thừa hai lần`n`. Ví dụ, với`n = 10`Và`m = 31`, dung lượng tối thiểu là bốn mươi, vì vậy cần ba giây. Hai lệnh đầu tiên là`10`Và`20`, để lại bốn mươi robot trong giây cuối cùng. Lệnh cuối cùng là`40 - 31 = 9`, vậy chính xác là có 31 robot sửa chữa. Đối số tương tự áp dụng cho mọi giá trị có thể có của`m`giữa hai công suất liên tiếp.
