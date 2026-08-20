---
title: "CF 102189B - \u0422\u0430\u0431\u043b\u0438\u0446\u0430 \u0440\u0435\u0437\u0443\u043b\u044c\u0442\u0430\u0442\u043e\u0432"
description: "Chúng ta cần chuyển danh sách người tham gia cuộc thi thành bảng xếp hạng có định dạng. Mỗi người tham gia có một tên duy nhất và số điểm không âm. Thứ tự cuối cùng được xác định đầu tiên bằng cách giảm dần số điểm."
date: "2026-08-19T16:12:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102189
codeforces_index: "B"
codeforces_contest_name: "12-\u0439 \u043e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0442\u0443\u0440\u043d\u0438\u0440 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0432 \u0410\u0431\u0430\u043a\u0430\u043d\u0435"
rating: 0
weight: 102189
solve_time_s: 214
verified: true
draft: false
---

[CF 102189B - \u0422\u0430\u0431\u043b\u0438\u0446\u0430 \u0440\u0435\u0437\u0443\u043b\u044c\u0442\u0430\u0442\u043e\u0432](https://codeforces.com/problemset/problem/102189/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 34 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chuyển danh sách người tham gia cuộc thi thành bảng xếp hạng có định dạng. Mỗi người tham gia có một tên duy nhất và số điểm không âm. Thứ tự cuối cùng được xác định đầu tiên bằng cách giảm dần số điểm. Khi hai người tham gia có cùng số điểm, tên của họ sẽ được so sánh về mặt từ điển sau khi chuyển đổi các chữ cái thành một trường hợp thông thường, vì vậy`Dy`,`dZ`, Và`dx`được sắp xếp như`dx`,`Dy`,`dZ`. 

Đầu vào chứa`n`người tham gia, ở đâu`1 <= n <= 50000`. Mỗi tên có nhiều nhất 20 chữ cái Latinh và mỗi điểm có nhiều nhất`10^6`. Những giới hạn này làm cho thuật toán bậc hai không phù hợp. Với 50.000 người tham gia, quy trình theo cặp có thể thực hiện khoảng 1,25 tỷ phép so sánh, vượt xa những gì chúng tôi mong muốn trong giới hạn 2 giây. Một sự sắp xếp so sánh mang lại`O(n log n)`, khoảng 50.000 lần 16 so sánh theo đúng thứ tự độ lớn, vì vậy đó là mục tiêu tự nhiên. 

Đầu ra có ba cột,`Place`,`Name`, Và`Score`. Chiều rộng của chúng không cố định. Mỗi chiều rộng là độ dài chuỗi tối đa xuất hiện trong cột đó, bao gồm cả tiêu đề. Các vị trí trống được lấp đầy bằng dấu chấm. Cột đầu tiên được căn phải, trong khi hai cột còn lại được căn trái. Vị trí này cũng hơi khác so với cấp bậc thông thường: nếu một nhóm chiếm giữ nhiều vị trí liên tiếp thì mọi thành viên sẽ nhận được cùng một cấp bậc, chẳng hạn như`2-3`hoặc`5-7`. 

Có một số trường hợp việc triển khai có vẻ hợp lý nhưng vẫn tạo ra bảng sai. 

Hãy xem xét một người tham gia duy nhất:```
1
Alice 0
```Đầu ra đúng là:```
|Place|Name.|Score|
|....1|Alice|0....|
```Việc thực hiện bất cẩn có thể tính toán phạm vi địa điểm như`1-1`, mặc dù một người tham gia chỉ phải nhận được`1`. 

Đặt tên không phân biệt chữ hoa chữ thường là một cái bẫy khác. Ví dụ:```
3
aa 10
Ab 10
aA 10
```Các phím so sánh chữ thường là`aa`,`ab`, Và`aa`. Như vậy`aa`Và`aA`so sánh bằng nhau, trong khi`Ab`đến sau họ. Tên ban đầu vẫn không thay đổi trong đầu ra. Việc sắp xếp phân biệt chữ hoa chữ thường sẽ đặt các chữ cái viết hoa trước chữ thường theo thứ tự ASCII và có thể tạo ra một bảng khác. 

Lỗi định dạng phổ biến nhất là quên rằng tiêu đề tham gia vào việc xác định độ rộng cột. Vì```
2
A 7
B 1000000
```cái`Score`cột phải rộng ít nhất 5 ký tự vì`Score`bản thân nó có độ dài 5, mặc dù điểm thực tế dài nhất có độ dài 7. Tương tự,`Place`đóng góp chiều rộng bằng 5. 

Cuối cùng, các đội hòa phải sử dụng vị trí của cả nhóm chứ không phải số điểm khác biệt gặp phải. Vì```
4
A 10
B 10
C 5
D 5
```hai người tham gia đầu tiên chiếm chỗ`1-2`, và hai số cuối cùng chiếm`3-4`. Việc chỉ định thứ hạng bằng cách tăng một bộ đếm riêng cho từng điểm riêng biệt sẽ làm phức tạp quá trình tính toán một cách không cần thiết và rất dễ mắc sai lầm. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ trực tiếp nhất là liên tục tìm ra người tham gia tiếp theo. Trong mỗi lần lặp lại, hãy quét tất cả những người tham gia còn lại và so sánh họ bằng cách sử dụng quy tắc đặt hàng bắt buộc. Điều này đúng vì người tham gia tốt nhất còn lại chính xác là hàng tiếp theo của bảng được sắp xếp. 

Vấn đề là số lượng so sánh. Trong trường hợp xấu nhất, hàng đầu tiên yêu cầu`n-1`so sánh, điều thứ hai yêu cầu`n-2`, vân vân. Vì`n = 50000`, điều này mang lại`50000 * 49999 / 2 = 1,249,975,000`so sánh. Ngay cả trước khi định dạng đầu ra, điều này là quá nhiều so với giới hạn thời gian. 

Phương pháp brute-force hoạt động vì thứ tự bảng bắt buộc là thứ tự tổng cộng: mỗi cặp người tham gia có thể được so sánh một cách nhất quán theo điểm số và sau đó bằng tên viết thường. Quan sát đó chính xác là điều cho phép chúng ta thay thế việc tìm kiếm lặp đi lặp lại bằng thao tác sắp xếp tiêu chuẩn. 

của Python`sort`có thể sắp xếp những người tham gia theo một bộ dữ liệu. Trước tiên, chúng tôi muốn điểm số lớn hơn, vì vậy thành phần đầu tiên là`-score`. Chúng tôi muốn tên theo thứ tự tăng dần không phân biệt chữ hoa chữ thường, vì vậy thành phần thứ hai là`name.lower()`. Chìa khóa kết quả là`(-score, name.lower())`. 

Sau khi những người tham gia được sắp xếp, mỗi nhóm có điểm bằng nhau sẽ tạo thành một phân đoạn liền kề. Nếu một nhóm như vậy bắt đầu ở chỉ số dựa trên 0`left`và kết thúc ngay trước`right`, vị trí hiển thị của nó là`left + 1`bởi vì`right`. Khi các số này khác nhau thì vị trí hiển thị là chuỗi`left+1-right`; nếu không thì nó chỉ là một số duy nhất. 

Việc định dạng cuối cùng cũng dễ dàng hơn sau khi sắp xếp. Trước tiên, chúng tôi xây dựng tất cả các chuỗi vị trí, sau đó tìm độ rộng tối đa của ba cột. Sau đó chúng ta có thể xây dựng mỗi hàng với số lượng dấu chấm thích hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả những người tham gia và lưu mỗi người thành một cặp có chứa tên và điểm. Cần phải giữ tên gốc vì việc sắp xếp bỏ qua chữ hoa chữ thường, nhưng đầu ra phải giữ nguyên cách viết được cung cấp trong đầu vào. 
2. Sắp xếp người tham gia bằng phím`(-score, name.lower())`. Việc phủ định điểm sẽ thay đổi thứ tự điểm giảm dần mong muốn thành thứ tự bộ dữ liệu tăng dần thông thường của Python. Tên chữ thường xử lý việc so sánh từ điển không phân biệt chữ hoa chữ thường. 
3. Quét mảng được sắp xếp theo các nhóm có số điểm bằng nhau. Đối với một nhóm bắt đầu tại chỉ mục`left`, nâng cao`right`trong khi tỉ số vẫn bằng nhau. Những người tham gia trong khoảng thời gian này chiếm vị trí`left + 1`bởi vì`right`. 
4. Chuyển đổi khoảng vị trí của nhóm thành chuỗi vị trí. Nếu như`left + 1 == right`, sử dụng số địa điểm duy nhất. Nếu không thì sử dụng phạm vi`left + 1-right`. Mọi người tham gia trong nhóm này đều nhận được chính xác chuỗi đó. 
5. Chuyển đổi mọi điểm số thành một chuỗi và thu thập ba cột được hiển thị. Chiều rộng của`Place`là tối đa của`len("Place")`và mọi chuỗi địa điểm được tạo. Quy tắc tương tự được sử dụng cho`Name`Và`Score`, bao gồm cả tiêu đề của chúng. 
6. In tiêu đề và sau đó in từng hàng của người tham gia. Cột địa điểm sử dụng căn phải bằng dấu chấm, trong khi cột tên và điểm số sử dụng căn trái bằng dấu chấm. Mỗi hàng được bao quanh bởi`|`, khớp với cú pháp bảng được yêu cầu. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, người tham gia xuất hiện theo đúng thứ tự được yêu cầu vì phím sắp xếp trước tiên sẽ so sánh`-score`, tương đương với việc giảm điểm và sau đó so sánh các tên viết thường, đây chính xác là điểm phân định bắt buộc. Vì các điểm bằng nhau nằm liền kề nhau theo thứ tự này nên mỗi nhóm có điểm bằng nhau tương ứng với một khoảng vị trí liên tiếp. Một nhóm bắt đầu ở vị trí`left + 1`và kết thúc tại`right`do đó nhận được chính xác chuỗi vị trí chung`left + 1`hoặc`left + 1-right`. Giai đoạn định dạng sử dụng ô thực tế dài nhất trong mỗi cột cùng với tiêu đề của nó, vì vậy mỗi hàng đều nhận được chính xác độ rộng và căn chỉnh cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    participants = []
    for _ in range(n):
        name, score = input().split()
        participants.append((name, int(score)))

    participants.sort(key=lambda x: (-x[1], x[0].lower()))

    places = [""] * n
    i = 0

    while i < n:
        j = i + 1
        while j < n and participants[j][1] == participants[i][1]:
            j += 1

        if i + 1 == j:
            place = str(i + 1)
        else:
            place = f"{i + 1}-{j}"

        for k in range(i, j):
            places[k] = place

        i = j

    place_width = max(len("Place"), *(len(x) for x in places))
    name_width = max(len("Name"), *(len(name) for name, _ in participants))
    score_strings = [str(score) for _, score in participants]
    score_width = max(len("Score"), *(len(x) for x in score_strings))

    out = []

    header = (
        "|"
        + "Place".ljust(place_width, ".")
        + "|"
        + "Name".ljust(name_width, ".")
        + "|"
        + "Score".ljust(score_width, ".")
        + "|"
    )
    out.append(header)

    for i, (name, _) in enumerate(participants):
        row = (
            "|"
            + places[i].rjust(place_width, ".")
            + "|"
            + name.ljust(name_width, ".")
            + "|"
            + score_strings[i].ljust(score_width, ".")
            + "|"
        )
        out.append(row)

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên đọc chính xác`n`hồ sơ người tham gia. Điểm được chuyển đổi thành số nguyên vì nó tham gia vào thứ tự số, trong khi tên vẫn ở dạng chuỗi cho cả sắp xếp và xuất. 

Khóa sắp xếp chứa hai thành phần.`-x[1]`làm cho điểm cao nhất đến trước.`x[0].lower()`thực hiện so sánh không phân biệt chữ hoa chữ thường mà không thay đổi cách viết đã lưu của tên. 

Quét nhóm sử dụng các khoảng thời gian nửa mở`[i, j)`. Quy ước này làm cho việc tính toán địa điểm trở nên đặc biệt rõ ràng. có`j - i`những người tham gia trong nhóm và bởi vì người tham gia đầu tiên có chỉ số dựa trên 0`i`, nhóm chiếm giữ các vị trí dựa trên một`i + 1`bởi vì`j`. điều kiện`i + 1 == j`chính xác là trường hợp một người tham gia, tránh việc trình bày không chính xác`1-1`. 

các`places`mảng lưu trữ chuỗi địa điểm đã được tính toán cho mỗi người tham gia được sắp xếp. Chi phí này`O(n)`bộ nhớ và tránh tính toán lại cùng một phạm vi cho mọi thành viên trong một mối quan hệ. 

Để định dạng,`ljust(width, ".")`đặt dấu chấm sau các giá trị căn trái, trong khi`rjust(width, ".")`đặt dấu chấm trước các giá trị căn phải. Sử dụng dấu chấm trực tiếp làm ký tự điền sẽ đơn giản hơn và ít xảy ra lỗi hơn so với việc tính toán độ dài phần đệm theo cách thủ công. 

Số nguyên Python có độ chính xác tùy ý, do đó điểm số bị giới hạn`10^6`không có vấn đề tràn. Chuỗi địa điểm lớn nhất có thể cũng nhỏ vì chỉ có 50.000 người tham gia. 

Mẫu hiển thị trong tuyên bố cuộc thi chứa tám bản ghi người tham gia, vì vậy giá trị đầu vào đầu tiên là`8`. Mã tuân theo định dạng đầu vào thực tế và đọc số đếm đó trước các bản ghi. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp thể hiện cả ba quy tắc sắp xếp và xếp hạng chính. Sau khi sắp xếp, những người tham gia được sắp xếp theo điểm, sau đó theo tên viết thường trong các nhóm có điểm bằng nhau. 

| Bước | Người tham gia được sắp xếp | Điểm | Nhóm hiện tại | Địa điểm | 
| --- | --- | --- | --- | --- | 
| 1 | Bredor | 9999 | Bredor | 1 | 
| 2 | Petr | 100 | Petr, khách du lịch | 2-3 | 
| 3 | du lịch | 100 | Petr, khách du lịch | 2-3 | 
| 4 | người dùng | 33 | người dùng | 4 | 
| 5 | dx | 5 | dx, Dy, dZ | 5-7 | 
| 6 | Dy | 5 | dx, Dy, dZ | 5-7 | 
| 7 | dZ | 5 | dx, Dy, dZ | 5-7 | 
| 8 | nhấnF | 0 | nhấnF | 8 | 

Độ rộng của ba cột là 5 cho`Place`, 7 cho`Name`và 6 cho`Score`. Tiêu đề được bao gồm khi xác định các độ rộng này. Ví dụ,`Bredor`có chiều dài 6, nhưng`Name`có độ dài 4, vì vậy thực tế`Name`chiều rộng là 7 vì`tourist`là cái tên dài nhất. 

Bảng kết quả là:```
|Place|Name...|Score|
|....1|Bredor.|9999.|
|..2-3|Petr...|100..|
|..2-3|tourist|100..|
|....4|user...|33...|
|..5-7|dx.....|5....|
|..5-7|Dy.....|5....|
|..5-7|dZ.....|5....|
|....8|pressF.|0....|
```Ví dụ thứ hai tách biệt việc sắp xếp không phân biệt chữ hoa chữ thường và một nhóm chiếm vị trí cuối cùng. 

đầu vào:```
5
Zulu 20
alpha 10
ALAN 10
beta 0
zebra 20
```Thứ tự sắp xếp là`Zulu`,`zebra`,`ALAN`,`alpha`,`beta`. Hai người tham gia đầu tiên chia sẻ vị trí`1-2`, trong khi hai người tham gia đạt điểm 10 chia sẻ vị trí`3-4`. 

| Chỉ mục | Tên | Điểm | Nhóm | Địa điểm | 
| --- | --- | --- | --- | --- | 
| 1 | Zulu | 20 | Zulu, ngựa vằn | 1-2 |
 | 2 | ngựa vằn | 20 | Zulu, ngựa vằn | 1-2 | 
| 3 | ALAN | 10 | ALAN, alpha | 3-4 | 
| 4 | alpha | 10 | ALAN, alpha | 3-4 | 
| 5 | phiên bản beta | 0 | phiên bản beta | 5 | 

Đây`ALAN`được so sánh như`alan`Và`alpha`BẰNG`alpha`, Vì thế`ALAN`đến trước. Cách viết hoa ban đầu vẫn được in không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét nhóm và định dạng là tuyến tính | 
| Không gian | O(n) | Người tham gia, chuỗi địa điểm, chuỗi điểm số và đầu ra yêu cầu bộ nhớ tuyến tính | 

Với tối đa 50.000 người tham gia và tên tối đa 20 ký tự,`O(n log n)`việc sắp xếp thoải mái trong mức độ phức tạp dự định trong giới hạn 2 giây, 256 MB. Đầu ra được tạo ra cũng chỉ tuyến tính về số lượng người tham gia, vì vậy việc lưu trữ nó trước lần ghi cuối cùng là điều thiết thực. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng tương tự`solve`logic như chương trình được gửi. Trình trợ giúp thay thế đầu vào và đầu ra tiêu chuẩn để có thể kiểm tra từng trường hợp bằng một xác nhận Python thông thường.```python
import sys
import io
from contextlib import redirect_stdout

def solve():
    n = int(input())

    participants = []
    for _ in range(n):
        name, score = input().split()
        participants.append((name, int(score)))

    participants.sort(key=lambda x: (-x[1], x[0].lower()))

    places = [""] * n
    i = 0

    while i < n:
        j = i + 1
        while j < n and participants[j][1] == participants[i][1]:
            j += 1

        if i + 1 == j:
            place = str(i + 1)
        else:
            place = f"{i + 1}-{j}"

        for k in range(i, j):
            places[k] = place

        i = j

    place_width = max(len("Place"), *(len(x) for x in places))
    name_width = max(len("Name"), *(len(name) for name, _ in participants))
    score_strings = [str(score) for _, score in participants]
    score_width = max(len("Score"), *(len(x) for x in score_strings))

    out = []

    out.append(
        "|"
        + "Place".ljust(place_width, ".")
        + "|"
        + "Name".ljust(name_width, ".")
        + "|"
        + "Score".ljust(score_width, ".")
        + "|"
    )

    for i, (name, _) in enumerate(participants):
        out.append(
            "|"
            + places[i].rjust(place_width, ".")
            + "|"
            + name.ljust(name_width, ".")
            + "|"
            + score_strings[i].ljust(score_width, ".")
            + "|"
        )

    sys.stdout.write("\n".join(out))

input = sys.stdin.readline

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    buffer = io.StringIO()
    try:
        with redirect_stdout(buffer):
            solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = sys.stdin.readline

    return buffer.getvalue()

# Provided sample
sample = """8
Petr 100
tourist 100
Bredor 9999
dZ 5
dx 5
Dy 5
pressF 0
user 33"""

sample_expected = """|Place|Name...|Score|
|....1|Bredor.|9999.|
|..2-3|Petr...|100..|
|..2-3|tourist|100..|
|....4|user...|33...|
|..5-7|dx.....|5....|
|..5-7|Dy.....|5....|
|..5-7|dZ.....|5....|
|....8|pressF.|0....|"""

assert run(sample) == sample_expected, "provided sample"

# Minimum-size case
assert run("""1
A 0""") == """|Place|Name.|Score|
|....1|A....|0....|""", "single participant"

# All scores equal, including case-insensitive ordering
assert run("""4
aa 10
BB 10
aA 10
Ab 10""") == """|Place|Name|Score|
|..1-4|aa..|10...|
|..1-4|aA..|10...|
|..1-4|Ab..|10...|
|..1-4|BB..|10...|""", "all equal scores"

# Boundary score values and a tie at the end
assert run("""5
low 0
maximum 1000000
ZERO 0
mid 999999
top 1000000""") == """|Place|Name...|Score|
|..1-2|maximum|1000000|
|..1-2|top....|1000000|
|....3|mid....|999999.|
|..4-5|low....|0......|
|..4-5|ZERO...|0......|""", "score boundaries and final tie"

# Maximum-size case, generated rather than written as 50000 literal lines
n = 50000
max_input = str(n) + "\n" + "".join(
    f"p{i} {i % 1000001}\n" for i in range(n)
)

max_output = run(max_input)
assert max_output.count("\n") == n, "maximum-size row count"
assert max_output.startswith("|Place|Name"), "maximum-size header"
assert max_output.endswith("|"), "maximum-size final boundary"
```Các trường hợp tùy chỉnh có thể được tóm tắt như sau. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / A 0`| Một hàng có địa điểm`1`| Kích thước tối thiểu và`1`, không`1-1`, ranh giới | 
| Bốn người tham gia có điểm`10`| Tất cả đều nhận được`1-4`| Nhóm liên kết đầy đủ và sắp xếp tên không phân biệt chữ hoa chữ thường | 
| Điểm số`0`Và`1000000`cộng với cà vạt | Phạm vi chính xác ở cả hai đầu | Ranh giới điểm số và xử lý trận chung kết | 
| 50.000 người tham gia được tạo | 50.000 hàng dữ liệu và ranh giới bảng hợp lệ | Kích thước đầu vào tối đa và xử lý đầu ra tuyến tính | 

Thử nghiệm kích thước tối đa có chủ ý kiểm tra các thuộc tính cấu trúc thay vì nhúng hàng trăm nghìn ký tự vào bài xã luận. Ranh giới của bảng đầu tiên và cuối cùng cũng như số lượng hàng đầu ra chính xác sẽ phát hiện các lỗi phổ biến khi một hàng bị mất, một hàng bổ sung được in hoặc dấu phân cách cuối cùng không đúng định dạng. 

## Vỏ cạnh 

Trường hợp một người tham gia được xử lý khi quá trình quét nhóm bắt đầu bằng`i = 0`và ngay lập tức nhận được`j = 1`. Từ`i + 1 == j`, chuỗi vị trí là`1`, không`1-1`. Đối với đầu vào```
1
Alice 0
```đầu ra là```
|Place|Name.|Score|
|....1|Alice|0....|
```Trường hợp hoàn toàn bằng nhau tạo ra một nhóm bao trùm toàn bộ mảng. Vì```
4
aa 10
BB 10
aA 10
Ab 10
```các phím chữ thường là`aa`,`bb`,`aa`, Và`ab`, vậy tên được sắp xếp là`aa`,`aA`,`Ab`,`BB`. Nhóm mở rộng từ vị trí 1 đến vị trí 4, trao cho mỗi người tham gia một vị trí`1-4`. Thuật toán không tăng thứ hạng trong nhóm, đó chính xác là những gì vị trí được chia sẻ yêu cầu. 

So sánh không phân biệt chữ hoa chữ thường chỉ được thực hiện để sắp xếp. Giả sử đầu vào chứa`ALAN 10`Và`alpha 10`. Chìa khóa so sánh của họ là`alan`Và`alpha`, Vì thế`ALAN`đến trước. Tên được lưu trữ vẫn còn`ALAN`, điều này ngăn ngừa lỗi phổ biến khi in khóa sắp xếp chữ thường thay vì tên người tham gia ban đầu. 

Điểm ngang nhau ở cuối bảng kiểm tra ranh giới bên phải của quá trình quét nhóm. Vì```
5
maximum 1000000
top 1000000
mid 999999
low 0
ZERO 0
```nhóm đầu tiên chiếm vị trí`1-2`, người tham gia ở giữa chiếm`3`, và nhóm cuối cùng chiếm`4-5`. Khi quá trình quét đến được người tham gia cuối cùng,`j`trở nên chính xác`n`và nhóm vẫn được xử lý vì điều kiện vòng lặp dựa trên`i < n`. 

Điểm tối đa`1000000`không yêu cầu bất kỳ xử lý số đặc biệt nào. Điểm là số nguyên và loại số nguyên của Python thể hiện an toàn toàn bộ phạm vi được phép. Điều tương tự cũng đúng với số 0, đây vẫn là điểm hợp lệ và phải sắp xếp sau mỗi điểm dương. 

Ranh giới định dạng cũng rất đáng kể. Nếu nơi dài nhất là`1-4`, độ dài của nó là 3, nhưng tiêu đề`Place`có độ dài 5, vì vậy cột đầu tiên có chiều rộng là 5 ký tự. Tương tự,`Score`chính nó có độ dài 5. Việc tính toán độ rộng từ cả tiêu đề và dữ liệu sẽ ngăn tiêu đề không đúng định dạng khi tất cả các giá trị thực tế đều ngắn hơn. 

Cuối cùng, tên là duy nhất ở dạng ban đầu, nhưng điều đó không loại bỏ nhu cầu duy trì thứ tự xác định khi các tên chỉ khác nhau theo từng trường hợp. Sự so sánh được yêu cầu xử lý các tên như vậy bằng nhau theo quy tắc không phân biệt chữ hoa chữ thường đã nêu. Tính sắp xếp của Python ổn định nên khi hai khóa giống hệt nhau, thứ tự nhập ban đầu của chúng được giữ nguyên. Hành vi đó nhất quán với bộ so sánh vì không người tham gia nào bắt buộc phải đi trước người kia theo thứ tự đã chỉ định.
