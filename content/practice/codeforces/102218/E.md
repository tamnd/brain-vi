---
title: "CF 102218E - Dự phòng môi trường"
description: "Chúng tôi có một loạt dự đoán N AQI, một giá trị cho mỗi ngày liên tiếp. Chúng ta cũng biết ngày trong tuần tương ứng với phần tử mảng đầu tiên và ngưỡng X. Một ngày học bị đình chỉ chính xác khi AQI của nó ít nhất là X."
date: "2026-08-17T23:18:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "E"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 176
verified: false
draft: false
---

[CF 102218E - Dự phòng môi trường](https://codeforces.com/problemset/problem/102218/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt`N`Dự đoán AQI, một giá trị cho mỗi ngày liên tiếp. Chúng ta cũng biết ngày trong tuần tương ứng với phần tử mảng đầu tiên và một ngưỡng`X`. Một ngày học bị đình chỉ chính xác khi AQI của nó ít nhất`X`. Thứ bảy và chủ nhật không bao giờ được tính, ngay cả khi AQI của họ đạt đến ngưỡng. 

Nhiệm vụ là đếm các vị trí mảng có AQI ít nhất`X`và ngày trong tuần tương ứng của họ là từ thứ Hai đến thứ Sáu. 

Dòng đầu tiên có thể chứa tối đa`10^6`ngày. Điều đó ngay lập tức loại trừ các thuật toán quét mảng liên tục hoặc thực hiện công việc tỷ lệ thuận với khoảng cách từ ngày đầu tiên cho mọi vị trí. MỘT`O(N)`quét là mục tiêu tự nhiên vì mọi giá trị AQI phải được kiểm tra ít nhất một lần. Với`N = 10^6`, MỘT`O(N log N)`lời giải vẫn hợp lý về mặt toán học, nhưng nó không mang lại lợi ích gì ở đây, trong khi`O(N^2)`có thể yêu cầu đại khái`5 * 10^11`lặp đi lặp lại và vượt xa giới hạn một giây. 

Các giá trị và ngưỡng AQI được giới hạn giữa`0`Và`500`, do đó không có vấn đề tràn số nguyên trong câu trả lời. Nhiều nhất`10^6`ngày có thể được tính. 

Trường hợp không rõ ràng đầu tiên là khi ngày đầu tiên là ngày cuối tuần. Ví dụ:```
1 Saturday 100
100
```Câu trả lời đúng là`0`. Việc thực hiện bất cẩn chỉ kiểm tra`AQI >= X`sẽ trở lại`1`, quên mất thứ bảy không có lớp học. 

Trường hợp thứ hai là so sánh ngưỡng mang tính bao hàm. Ví dụ:```
1 Monday 100
100
```Câu trả lời đúng là`1`, bởi vì AQI chính xác bằng`X`đình chỉ các lớp học. sử dụng`>`thay vì`>=`sẽ quay lại không chính xác`0`. 

Trường hợp thứ ba là vượt qua Chủ nhật trong khi quét. Ví dụ:```
3 Friday 100
100 100 100
```Các ngày là thứ sáu, thứ bảy, chủ nhật nên đáp án là`1`. Việc triển khai chỉ kiểm tra ngày trong tuần đầu tiên và quên nâng cao ngày trong tuần cho mọi vị trí mảng có thể đếm không chính xác cả ba giá trị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể xác định ngày trong tuần của từng vị trí mảng một cách độc lập. Đối với vị trí`i`, bắt đầu từ ngày đầu tiên trong tuần nhất định và tiến dần theo lịch từng ngày một cho đến khi đạt đến vị trí`i`. Sau đó kiểm tra xem ngày trong tuần đó có phải là ngày trong tuần không và`a[i] >= X`. 

Cách tiếp cận này đúng vì nó tái tạo lại chính xác ngày trong tuần của mọi phần tử mảng. Vấn đề là nó lặp lại cùng một lịch làm việc. Đối với vị trí`0`không cần thăng tiến để có được vị trí`1`cần có một sự thăng tiến, và để có được vị trí`N - 1`,`N - 1`cần có sự tiến bộ. Tổng số tiền ứng trước vào các ngày trong tuần là`0 + 1 + 2 + ... + (N - 1) = N(N - 1)/2`. 

Tại`N = 10^6`, đó là`499,999,500,000`tiến bộ trong các ngày trong tuần, thậm chí trước cả khi tính đến việc kiểm tra AQI. Điều đó không thể phù hợp trong thời hạn. 

Quan sát quan trọng là các vị trí mảng liên tiếp biểu thị các ngày theo lịch liên tiếp. Khi chúng ta biết ngày trong tuần của vị trí`i`, ngày trong tuần của vị trí`i + 1`chỉ đơn giản là ngày trong tuần tiếp theo trong chu kỳ bảy ngày. Không có lý do gì phải tính lại lịch từ đầu. 

Chúng ta có thể biểu diễn từ Thứ Hai đến Chủ Nhật bằng số nguyên`0`bởi vì`6`. Đối với mỗi giá trị AQI, chúng tôi kiểm tra hai điều kiện: AQI ít nhất phải bằng`X`, và ngày trong tuần hiện tại phải là một trong`0, 1, 2, 3, 4`. Sau khi xử lý giá trị, chúng tôi tiến tới ngày trong tuần với`(weekday + 1) % 7`. 

Phương pháp bạo lực và phương pháp tối ưu đều kiểm tra các giá trị AQI, nhưng phương pháp tối ưu chỉ thực hiện công việc liên tục mỗi ngày thay vì tái tạo lại cùng một tiền tố của lịch. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ánh xạ tên bảy ngày trong tuần thành các số nguyên theo thứ tự lịch, với Thứ Hai là`0`và chủ nhật như`6`. Điều này cho chúng ta một cách biểu diễn ngắn gọn trong đó việc chuyển sang ngày tiếp theo chỉ là một modulo tăng dần.`7`. 
2. Đọc`N`, tên của ngày trong tuần đầu tiên và ngưỡng`X`. Chuyển đổi tên ngày bắt đầu trong tuần thành biểu diễn số nguyên của nó. 
3. Đọc`N`Giá trị AQI và xử lý chúng từ trái sang phải. Tại vị trí`i`, biến`weekday`đại diện cho ngày trong tuần thực tế của`a[i]`. 
4. Nếu`weekday < 5`Và`a[i] >= X`, tăng câu trả lời. Điều kiện đầu tiên không bao gồm thứ bảy và chủ nhật, trong khi điều kiện thứ hai áp dụng ngưỡng tạm dừng đúng như quy định. 
5. Tiến lên`weekday`sử dụng`(weekday + 1) % 7`. Điều này chuyển từ Thứ Hai sang Thứ Ba, Thứ Ba sang Thứ Tư, v.v., và Chủ Nhật sẽ chuyển sang Thứ Hai. 
6. In kết quả cuối cùng`N`ngày đã được xử lý. 

### Tại sao nó hoạt động 

Bất biến là ngay trước khi xử lý`a[i]`,`weekday`chính xác là ngày trong tuần tương ứng với`i`-phần tử mảng thứ. Ban đầu nó đúng vì`weekday`được khởi tạo từ ngày đầu tiên đã nêu. Nếu nó đúng với vị trí`i`, tiến lên một lần sẽ cho ra ngày trong tuần của ngày dương lịch tiếp theo, chính xác là ngày trong tuần tương ứng với vị trí`i + 1`. Do đó, bất biến đúng cho mọi vị trí. 

Đối với mỗi vị trí, thuật toán sẽ tăng câu trả lời chính xác khi AQI ít nhất`X`và ngày là từ thứ Hai đến thứ Sáu. Đó chính xác là điều kiện để các lớp học bị đình chỉ nên mỗi ngày được tính đều hợp lệ và mỗi ngày hợp lệ đều được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    day_to_num = {
        "Monday": 0,
        "Tuesday": 1,
        "Wednesday": 2,
        "Thursday": 3,
        "Friday": 4,
        "Saturday": 5,
        "Sunday": 6,
    }

    n, day_name, x = input().split()
    n = int(n)
    x = int(x)

    weekday = day_to_num[day_name]
    answer = 0

    remaining = n
    while remaining:
        values = map(int, input().split())
        for aqi in values:
            if weekday < 5 and aqi >= x:
                answer += 1

            weekday = (weekday + 1) % 7
            remaining -= 1

            if remaining == 0:
                break

    print(answer)

if __name__ == "__main__":
    solve()
```Từ điển chuyển đổi văn bản ngày bắt đầu trong tuần thành chu kỳ số nguyên được thuật toán sử dụng. Thứ hai là`0`và thứ Sáu là`4`, vậy điều kiện`weekday < 5`xác định chính xác năm ngày học. 

Vòng lặp đọc đầu vào đáng được chú ý. Mặc dù vấn đề đặt tất cả`N`Giá trị AQI trên dòng thứ hai, xử lý dòng bằng`split()`là đủ cho đầu vào chính thức. Vòng lặp cũng xử lý các giá trị tăng dần và tránh lưu trữ toàn bộ mảng AQI, điều này giúp giữ không gian bổ sung của thuật toán ở mức tối thiểu.`O(1)`. 

quầy`remaining`làm cho quá trình xử lý trở nên mạnh mẽ ngay cả khi đầu vào được chia thành nhiều dòng. Mỗi AQI được tiêu thụ sẽ giảm đi một và quá trình xử lý dừng lại chính xác sau đó`N`các giá trị. 

Việc so sánh sử dụng`>=`, không`>`, bởi vì chỉ số AQI bằng ngưỡng là đủ để đình chỉ các lớp học. Ngày trong tuần được nâng cao sau khi AQI hiện tại được kiểm tra, do đó AQI đầu tiên được đánh giá theo ngày bắt đầu được cung cấp thay vì ngày hôm sau. Thứ tự này ngăn ngừa lỗi xảy ra phổ biến nhất. 

Số nguyên Python không tràn cho vấn đề này và câu trả lời nhiều nhất là`10^6`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Ngày đầu tiên là thứ Tư, đại diện bởi`2`. Ngưỡng là`6`. Các ngày trong tuần diễn ra theo chu kỳ bảy ngày bình thường. 

| AQI | Số ngày trong tuần | Ngày trong tuần | AQI >= 6 | Ngày học | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | Thứ Tư | Không | Có | 0 | 
| 2 | 3 | Thứ Năm | Không | Có | 0 | 
| 3 | 4 | Thứ Sáu | Không | Có | 0 | 
| 4 | 5 | thứ bảy | Không | Không | 0 | 
| 5 | 6 | chủ nhật | Không | Không | 0 | 
| 6 | 0 | Thứ Hai | Có | Có | 1 | 
| 7 | 1 | Thứ Ba | Có | Có | 2 | 
| 8 | 2 | Thứ Tư | Có | Có | 3 | 
| 9 | 3 | Thứ Năm | Có | Có | 4 | 
| 10 | 4 | Thứ Sáu | Có | Có | 5 | 

Năm giá trị đầu tiên không đóng góp. Năm điều cuối cùng đều đạt đến ngưỡng và xảy ra từ thứ Hai đến thứ Sáu, tạo ra câu trả lời`5`. Dấu vết cũng cho thấy lý do tại sao ngày trong tuần phải được nâng cao sau mỗi giá trị, bao gồm cả giá trị cuối tuần. 

### Mẫu 2 

Ngày đầu tiên là thứ bảy, đại diện bởi`5`, và ngưỡng là`223`. 

| AQI | Số ngày trong tuần | Ngày trong tuần | Chỉ số AQI >= 223 | Ngày học | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 90 | 5 | thứ bảy | Không | Không | 0 | 
| 372 | 6 | chủ nhật | Có | Không | 0 | 
| 191 | 0 | Thứ Hai | Không | Có | 0 | 
| 282 | 1 | Thứ Ba | Có | Có | 1 | 
| 223 | 2 | Thứ Tư | Có | Có | 2 | 

Ba giá trị AQI đạt đến ngưỡng, nhưng`372`xảy ra vào Chủ nhật và được loại trừ. Câu trả lời cuối cùng là`2`. Điều này khẳng định rằng chỉ riêng tiêu chuẩn AQI là chưa đủ và điều kiện ngày trong tuần phải được kiểm tra độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi giá trị AQI được xử lý chính xác một lần, với công việc không đổi trên mỗi giá trị. | 
| Không gian | O(1) | Chỉ có ngày trong tuần, ngưỡng, số lượng còn lại và câu trả lời hiện tại được duy trì ngoài từ điển ngày trong tuần nhỏ. | 

Với`N`lớn như`10^6`, một lần quét tuyến tính duy nhất là phù hợp với giới hạn một giây. Thuật toán chỉ thực hiện một số thao tác số nguyên mỗi ngày và không phân bổ một mảng chứa tất cả các giá trị AQI, do đó mức sử dụng bộ nhớ bổ sung của nó là không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    day_to_num = {
        "Monday": 0,
        "Tuesday": 1,
        "Wednesday": 2,
        "Thursday": 3,
        "Friday": 4,
        "Saturday": 5,
        "Sunday": 6,
    }

    n, day_name, x = input().split()
    n = int(n)
    x = int(x)

    weekday = day_to_num[day_name]
    answer = 0

    remaining = n
    while remaining:
        values = map(int, input().split())
        for aqi in values:
            if weekday < 5 and aqi >= x:
                answer += 1

            weekday = (weekday + 1) % 7
            remaining -= 1

            if remaining == 0:
                break

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run(
    """10 Wednesday 6
1 2 3 4 5 6 7 8 9 10
"""
) == "5", "sample 1"

assert run(
    """5 Saturday 223
90 372 191 282 223
"""
) == "2", "sample 2"

assert run(
    """5 Sunday 269
90 372 191 282 223
"""
) == "2", "sample 3"

# Minimum-size input, weekend day
assert run(
    """1 Saturday 0
500
"""
) == "0", "single weekend day"

# Exact threshold must count
assert run(
    """1 Monday 100
100
"""
) == "1", "threshold is inclusive"

# Weekend crossing and threshold boundary
assert run(
    """7 Friday 200
200 200 200 199 201 200 200
"""
) == "2", "Friday through Thursday with weekend excluded"

# All values equal to the threshold, full week
assert run(
    """7 Monday 500
500 500 500 500 500 500 500
"""
) == "5", "all weekdays at exact threshold"

# Maximum-size input, all values equal and all days qualifying on weekdays
n = 1_000_000
values = "500 " * (n - 1) + "500"
large_input = f"{n} Monday 500\n{values}\n"
assert run(large_input) == "714286", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 Saturday 0`với AQI`500`|`0`| Kích thước tối thiểu và loại trừ cuối tuần | 
|`1 Monday 100`với AQI`100`|`1`| Bao gồm`>= X`ranh giới | 
|`7 Friday 200`với các giá trị AQI hỗn hợp |`2`| Diễn biến chính xác các ngày trong tuần vào Thứ Bảy và Chủ Nhật | 
|`7 Monday 500`với mọi AQI bằng`500`|`5`| Giá trị hoàn toàn bằng nhau và ngưỡng chính xác | 
|`1,000,000`giá trị thứ hai của`500`|`714286`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Đối với một ngày cuối tuần, hãy xem xét:```
1 Saturday 0
500
```Ngày đầu tiên trong tuần là`5`, Vì thế`weekday < 5`là sai. Mặc dù`500 >= 0`, câu trả lời vẫn còn`0`. Sau đó, thuật toán sẽ chuyển sang Chủ nhật nhưng không còn giá trị nào để xử lý. 

Đối với ranh giới ngưỡng chính xác:```
1 Monday 100
100
```Ngày trong tuần là thứ Hai và`100 >= 100`là đúng, vì vậy câu trả lời trở thành`1`. Điều này phát hiện các triển khai vô tình sử dụng nghiêm ngặt`>`so sánh. 

Để chuyển đổi lịch vào cuối tuần:```
3 Friday 100
100 100 100
```Giá trị đầu tiên là thứ Sáu và được tính. Thứ hai là thứ Bảy và bị từ chối vì trường học đóng cửa. Ngày thứ ba là Chủ nhật và cũng bị từ chối. Câu trả lời cuối cùng là`1`. Trạng thái ngày trong tuần thay đổi từ`4`ĐẾN`5`ĐẾN`6`, chứng tỏ rằng quá trình cập nhật phải diễn ra sau khi xử lý từng AQI. 

Đối với chu kỳ hàng tuần hoàn chỉnh:```
7 Monday 500
500 500 500 500 500 500 500
```Năm giá trị đầu tiên tương ứng với từ Thứ Hai đến Thứ Sáu và đều được tính. Ngày thứ sáu và thứ bảy tương ứng với thứ bảy và chủ nhật và bị bỏ qua. Kết quả là`5`. Điều này xác nhận cả ranh giới ngày trong tuần và phạm vi bao quanh modulo-7. 

Hộp có kích thước tối đa chứa`10^6`Giá trị AQI. Vì mỗi giá trị gây ra một lần so sánh và một lần cập nhật ngày trong tuần nên thời gian chạy tăng theo tuyến tính thay vì theo phương trình bậc hai. Thuật toán không bao giờ cần giữ lại mảng AQI, do đó trạng thái hoạt động của nó không đổi ngay cả ở kích thước đầu vào lớn nhất.
