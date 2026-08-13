---
title: "CF 102282C - \u041d\u0435\u0443\u0442\u0435\u0448\u0438\u0442\u0435\u043b\u044c\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Một con ốc sên xuất phát ở độ cao 0 và leo lên núi Phú Sĩ có độ cao cố định là 3776 mét. Trong mỗi ngày nó leo lên đúng n mét. Nếu nó chưa lên đến đỉnh sau lần leo đó thì đêm hôm sau sẽ khiến nó trượt xuống m mét."
date: "2026-08-13T09:06:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102282
codeforces_index: "C"
codeforces_contest_name: "2011, \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 \u0421\u0413\u0410\u0423 \u043d\u0430 \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b ACM ICPC"
rating: 0
weight: 102282
solve_time_s: 54
verified: true
draft: false
---

[CF 102282C - \u041d\u0435\u0443\u0442\u0435\u0448\u0438\u0442\u0435\u043b\u044c\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102282/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một con ốc sên xuất phát ở độ cao 0 và leo lên núi Phú Sĩ có độ cao cố định là 3776 mét. Trong mỗi ngày nó leo lên chính xác`n`mét. Nếu sau lần leo núi đó mà chưa lên tới đỉnh thì đêm hôm sau sẽ khiến nó trượt xuống`m`mét. Câu hỏi đặt ra là cần bao nhiêu ngày để con ốc sên đạt đến độ cao 3776, hay nó sẽ không bao giờ đạt tới nó. 

Đầu vào chứa leo núi ban ngày`n`và đường trượt vào ban đêm`m`. Cả hai giá trị có thể lớn bằng (10^9), nhưng bản thân độ cao của ngọn núi chỉ là 3776. Kết quả đầu ra là ngày đầu tiên con ốc lên tới đỉnh hoặc từ`NEVER`. 

Giới hạn lớn trên`n`Và`m`không ép buộc cấu trúc dữ liệu phức tạp hoặc thuật toán lặp. Trên thực tế, độ cao của ngọn núi nhỏ và cố định đến mức ngay cả một mô phỏng trực tiếp hàng ngày cũng thực hiện tối đa 3776 lần leo núi trong trường hợp chậm nhất có thể, khi`n = 1`Và`m = 0`. Đó là rất nhỏ cho giới hạn một giây. Tuy nhiên, lời giải số học đơn giản hơn và chạy trong thời gian không đổi. 

Có một số trường hợp biên có thể làm cho một công thức đơn giản trở nên sai lầm. Nếu như`n >= 3776`, con ốc lên đến đỉnh vào ngày đầu tiên. Ví dụ, với`3776 100`, câu trả lời là`1`, bởi vì con ốc sên này đạt đến độ cao 3776 trong lần leo núi đầu tiên vào ban ngày và không bao giờ qua đêm trên núi. 

Nếu như`n <= m`Và`n < 3776`, con ốc sên không bao giờ có thể tiến bộ vĩnh viễn lên tới đỉnh. Ví dụ,`5 5`cho`NEVER`. Sau mỗi chu kỳ ngày và đêm hoàn chỉnh, ốc sên sẽ trở lại độ cao như cũ. Với`5 6`, nó thậm chí còn mất đi độ cao nhiều hơn sau mỗi chu kỳ. 

Một sai lầm phổ biến khác là áp dụng công thức tiến độ thực hàng ngày mà không xử lý riêng chặng leo cuối cùng. Vì`5 4`, quãng đường thực sự sau một ngày đêm trọn vẹn chỉ là 1 mét, nhưng con ốc sên đã lên tới đỉnh trong lần leo cuối cùng vào ban ngày và không trượt xuống sau đó. Câu trả lời đúng là 3772, không phải 3776. 

Vụ án`n = 0`cũng thuộc loại không thể. Ví dụ,`0 0`sản xuất`NEVER`, vì con ốc sên không bao giờ rời khỏi độ cao ban đầu. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản mô phỏng con ốc sên. Bắt đầu với chiều cao bằng 0 và thêm liên tục`n`. Nếu chiều cao mới ít nhất là 3776 thì ngày hiện tại là câu trả lời. Ngược lại thì trừ`m`để đại diện cho đêm và tiếp tục với ngày hôm sau. Điều này tuân theo chính xác quy trình vật lý nên tính chính xác của nó là ngay lập tức. 

Số ngày mô phỏng tối đa chỉ là 3776. Trường hợp leo núi chậm nhất có thể là`n = 1, m = 0`, đạt đến đỉnh vào ngày thứ 3776. Do đó, phương pháp vũ phu thực hiện tối đa 3776 lần lặp, thấp hơn nhiều so với những gì chương trình lập trình cạnh tranh một giây có thể xử lý. The huge upper bound of (10^9) for`n`Và`m`không làm tăng số lần lặp này, bởi vì một lượng lớn`n`chỉ làm cho con ốc kết thúc sớm hơn mà thôi. 

Giải pháp số học xuất phát từ việc tách các chu kỳ ngày đêm hoàn chỉnh khỏi ngày cuối cùng. Trước chuyến leo núi cuối cùng, con ốc sên luôn ở dưới đỉnh nên cả ngày và đêm sẽ thay đổi độ cao của nó bằng`n - m`. Nếu như`n <= m`, sự thay đổi đó là không tích cực và việc lên đến đỉnh là điều không thể trừ khi con ốc sên đạt tới đỉnh ngay trong ngày đầu tiên. 

Giả định`n > m`Và`n < 3776`. Sau đó`k`trọn ngày đêm ốc lên cao 

[ 
k(n-m). 
] 

Chúng ta cần số chu kỳ hoàn chỉnh nhỏ nhất mà sau đó lần leo núi tiếp theo vào ban ngày sẽ đạt đến đỉnh: 

[ 
k(n-m) + n \ge 3776. 
] 

Tương đương, 

[ 
k(n-m) \ge 3776-n. 
] 

Nhỏ nhất như vậy`k`là 

[ 
k = \left\lceil \frac{3776-n}{n-m} \right\rceil. 
] 

Vì chuyến leo núi cuối cùng diễn ra vào ngày hôm sau nên câu trả lời là`k + 1`. 

Phép chia trần có thể được thực hiện bằng số học số nguyên như`(a + b - 1) // b`tích cực`a`Và`b`. Ở đây mang lại 

[ 
\frac{3776-n + (n-m)-1}{n-m} 
] 

dùng phép chia số nguyên. 

Hai cách tiếp cận có độ phức tạp như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3776), thực tế là O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

Phiên bản số học được ưa chuộng hơn vì nó thể hiện trực tiếp lý do cơ bản và tránh duy trì chiều cao mô phỏng. 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. Đó là leo núi vào ban ngày và trượt vào ban đêm. 
2. Nếu`n >= 3776`, in`1`. Con ốc sên lên tới đỉnh trong lần leo đầu tiên nên không cần xem xét chuyển động vào ban đêm. 
3. Nếu`n <= m`, in`NEVER`. Từ`n < 3776`sau lần kiểm tra trước, con ốc sên không thể lên tới đỉnh vào ngày đầu tiên. Mỗi chu kỳ ngày đêm hoàn chỉnh thay đổi độ cao của nó bằng`n - m <= 0`, vì vậy về lâu dài nó không bao giờ có thể tiến gần đến đỉnh hơn. 
4. Mặt khác, hãy tính mức tăng ròng của một chu kỳ ngày đêm hoàn chỉnh như sau`n - m`. Giá trị này là dương nên các chu kỳ lặp đi lặp lại cuối cùng sẽ đẩy con ốc lên đủ cao. 
5. Tính số chu kỳ hoàn chỉnh cần thiết trước lần leo núi cuối cùng với`cycles = (3776 - n + (n - m) - 1) // (n - m)`. 

Tử số biểu thị độ cao vẫn còn thiếu sau một lần leo lên trong ngày và phép chia trần sẽ tìm ra số chu kỳ đầy đủ nhỏ nhất có thể bù được độ cao bị thiếu đó. 
6. In`cycles + 1`. Phần bổ sung là chặng leo núi cuối cùng trong ngày mà con ốc sên thực sự lên tới đỉnh. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau mỗi ngày đêm trọn vẹn, khi con ốc sên chưa lên đến đỉnh thì chiều cao của nó tăng lên chính xác`n - m`. Khi`n <= m`, mức tăng này là không dương nên không có chuỗi chu kỳ hoàn chỉnh nào có thể làm cho con ốc sên đạt đến độ cao mà nó không thể đạt tới trong ngày đầu tiên. Khi`n > m`, sau đó`cycles`hoàn thành chu kỳ chiều cao là chính xác`cycles * (n - m)`, và cách phân chia trần được chọn làm cho số chu kỳ này trở thành số chu kỳ nhỏ nhất mà việc thêm một chuyến leo núi vào ban ngày nữa đạt tới 3776. Vì ngày cuối cùng không có đêm tiếp theo nên câu trả lời chính xác là`cycles + 1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

HEIGHT = 3776

def solve():
    n, m = map(int, input().split())

    if n >= HEIGHT:
        print(1)
        return

    if n <= m:
        print("NEVER")
        return

    gain = n - m
    remaining = HEIGHT - n

    cycles = (remaining + gain - 1) // gain
    print(cycles + 1)

if __name__ == "__main__":
    solve()
```Hằng số`HEIGHT`lưu trữ chiều cao đỉnh cố định, giúp công thức dễ đọc hơn và tránh phân tán giá trị 3776 qua mã. 

Điều kiện đầu tiên phải được kiểm tra trước điều kiện không thể. Nếu như`n`ít nhất là 3776, con ốc sên thành công ngay lập tức ngay cả khi`m`lớn hơn nhiều so với`n`. Ví dụ,`3776 1000000000`vẫn có câu trả lời`1`, vì hiện tượng trượt vào ban đêm chỉ xảy ra sau khi con ốc đã lên đến đỉnh. 

Sau khi loại trừ trường hợp thành công ngay ngày đầu tiên,`n <= m`có nghĩa là mọi chu kỳ đầy đủ đều có tiến triển không tích cực. Ốc sên không thể leo lên đỉnh một cách vô định, vì vậy`NEVER`là đúng. 

Đối với trường hợp còn lại,`gain`là hoàn toàn tích cực. Biến`remaining`là khoảng cách vẫn cần thiết sau lần leo núi đầu tiên vào ban ngày. Bộ phận trần tìm ra cần bao nhiêu chu kỳ hoàn chỉnh để bao phủ khoảng cách đó. trận chung kết`+ 1`tính đến thời gian leo núi ban ngày xảy ra sau những chu kỳ hoàn chỉnh đó. 

Số nguyên Python có độ chính xác tùy ý, do đó, mặc dù giá trị đầu vào có thể đạt tới (10^9) nhưng không có vấn đề tràn. Việc tính toán cũng sử dụng số học số nguyên xuyên suốt, tránh các vấn đề làm tròn dấu phẩy động. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`n = 5`Và`m = 4`. Con ốc sên chỉ tăng được một mét trong một chu kỳ ngày đêm hoàn chỉnh, và sau đủ nhiều chu kỳ, chuyến leo núi tiếp theo vào ban ngày sẽ đạt đến đỉnh. 

| Trạng thái ngày | Chiều cao trước khi leo | Leo núi trong ngày | Chiều cao sau khi leo | Trượt đêm | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | +5 | 5 | -4 | 
| 2 | 1 | +5 | 6 | -4 | 
| 3 | 2 | +5 | 7 | -4 | 
| ... | ... | ... | ... | ... | 
| 3772 | 3771 | +5 | 3776 | không | 

Công thức cho`gain = 1`Và`remaining = 3771`. Như vậy`cycles = 3771`, và câu trả lời cuối cùng là`3772`. Ngày cuối cùng được xử lý khác với mọi ngày trước đó vì con ốc sên dừng lại ngay khi lên tới đỉnh. 

Đối với mẫu thứ hai,`n = 100`Và`m = 200`. 

| Kiểm tra | Giá trị | Kết quả | 
| --- | --- | --- | 
|`n >= 3776`| 100 >= 3776 | sai | 
|`n <= m`| 100 <= 200 | đúng | 
| Đầu ra | |`NEVER`| 

Con ốc sên ban ngày đi được 100m nhưng ban đêm lại đi được 200m. Tiến trình thực sự của nó là âm nên nó không thể tiếp cận đỉnh thông qua các chu kỳ lặp đi lặp lại. Thuật toán phát hiện điều này ngay lập tức mà không cần thực hiện bất kỳ mô phỏng nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép tính số học và phép so sánh được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên. | 

Các giá trị đầu vào có thể lớn bằng (10^9), nhưng thuật toán không bao giờ lặp theo độ lớn của chúng. Nó thực hiện một số thao tác cố định, do đó, nó vừa vặn thoải mái trong giới hạn thời gian một giây và giới hạn bộ nhớ 128 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

HEIGHT = 3776

def solve():
    n, m = map(int, input().split())

    if n >= HEIGHT:
        print(1)
        return

    if n <= m:
        print("NEVER")
        return

    gain = n - m
    remaining = HEIGHT - n
    cycles = (remaining + gain - 1) // gain
    print(cycles + 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples
assert run("5 4\n") == "3772", "sample 1"
assert run("100 200\n") == "NEVER", "sample 2"

# Minimum-size input
assert run("0 0\n") == "NEVER", "the snail never moves"

# Immediate success
assert run("3776 1000000000\n") == "1", "reaches the summit on day one"

# Exactly one meter of net progress
assert run("1 0\n") == "3776", "slowest possible successful climb"

# Equal climb and slide
assert run("10 10\n") == "NEVER", "zero net progress"

# Maximum input values
assert run("1000000000 0\n") == "1", "very large daytime climb"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`NEVER`| Con ốc sên hoàn toàn không thể di chuyển được. | 
|`3776 1000000000`|`1`| Việc lên tới đỉnh vào ngày đầu tiên phải được kiểm tra trước khi trượt vào ban đêm. | 
|`1 0`|`3776`| Số ngày mô phỏng tối đa có thể và ranh giới của ngày cuối cùng. | 
|`10 10`|`NEVER`| Leo lên và trượt bằng nhau không mang lại tiến bộ thực sự nào. | 
|`1000000000 0`|`1`| Giá trị đầu vào lớn và thành công ngay lập tức. | 

## Vỏ cạnh 

cho`0 0`, điều kiện ngày đầu tiên`n >= 3776`là sai, và`n <= m`là đúng. Thuật toán in`NEVER`. Mô phỏng cũng sẽ duy trì ở độ cao 0 mãi mãi, vì vậy kết quả là nhất quán. 

Vì`3776 1000000000`, con ốc leo chính xác đến đỉnh trong chuyển động ban ngày đầu tiên. Thuật toán in`1`trước khi kiểm tra xem`n <= m`. Thứ tự này rất quan trọng vì đường trượt lớn vào ban đêm sẽ không còn phù hợp khi bạn đã đạt đến đỉnh. 

Vì`1 0`, mỗi ngày đóng góp một mét và không có đường trượt vào ban đêm. Con ốc đạt tới độ cao 3776 mét vào ngày thứ 3776. Công thức có`gain = 1`,`remaining = 3775`, Vì thế`cycles = 3775`và kết quả là`3776`. Đây là câu trả lời lớn nhất có thể cho chiều cao cố định của ngọn núi. 

Vì`10 10`, mọi chu kỳ ngày đêm hoàn thành đều có thay đổi ròng bằng 0. Vì con ốc sên chỉ dài được 10 mét vào ngày đầu tiên nên nó không bao giờ đạt tới 3776.`n <= m`điều kiện bắt được trường hợp này trước khi chia, điều này cũng ngăn cản mẫu số bằng 0. 

Vì`5 4`, con ốc sên lên đến đỉnh vào ngày thứ 3772. Công thức chỉ sử dụng mức tăng ròng là một mét cho các chu kỳ hoàn chỉnh, sau đó cộng thêm lần leo cuối cùng vào ban ngày một cách riêng biệt. Việc coi mỗi ngày như một chu kỳ hoàn chỉnh sẽ tính sai một lần trượt vào ban đêm sau đỉnh núi và đưa ra câu trả lời sai.
