---
title: "CF 102297B - Xếp hạng huy chương"
description: "Mỗi trường hợp thử nghiệm mô tả số huy chương của hai quốc gia Mỹ và Nga. Sáu số nguyên được sắp xếp theo thứ tự vàng, bạc, đồng của Mỹ, tiếp theo là vàng, bạc, đồng của Nga. Có hai cách độc lập để quyết định xem Hoa Kỳ có thắng hay không."
date: "2026-08-13T22:42:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102297
codeforces_index: "B"
codeforces_contest_name: "UCF Locals 2015"
rating: 0
weight: 102297
solve_time_s: 58
verified: true
draft: false
---

[CF 102297B - Xếp hạng huy chương](https://codeforces.com/problemset/problem/102297/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả số huy chương của hai quốc gia Mỹ và Nga. Sáu số nguyên được sắp xếp theo thứ tự vàng, bạc, đồng của Mỹ, tiếp theo là vàng, bạc, đồng của Nga. 

Có hai cách độc lập để quyết định xem Hoa Kỳ có thắng hay không. Dưới`count`xếp hạng, chỉ có tổng số huy chương là quan trọng, vì vậy chúng tôi so sánh tổng của ba lần giành huy chương. Dưới`color`Trong bảng xếp hạng, các loại huy chương có mức độ ưu tiên rất khắt khe: vàng được so sánh trước, bạc chỉ được xét khi vàng và bạc hòa nhau. Đây chính xác là sự so sánh từ điển của bộ ba`(gold, silver, bronze)`. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi in sáu số ban đầu, sau đó in`count`nếu Hoa Kỳ chỉ thắng bằng tổng số huy chương,`color`nếu Hoa Kỳ chỉ thắng theo màu huy chương,`both`nếu Hoa Kỳ thắng theo cả hai hệ thống, và`none`nếu Nga không bị đánh bại theo một trong hai hệ thống. Một dòng trống theo sau mỗi kết quả. 

Câu lệnh chỉ định một số dương`n`của các trường hợp thử nghiệm và cho biết mỗi số huy chương nằm trong khoảng`0`Và`500`. Những giới hạn đó làm cho mọi phép tính số học trở nên nhỏ bé. Kể cả nếu`n`lớn như`10^5`trở lên, không cần sắp xếp, lập trình động, duyệt đồ thị hoặc bất kỳ thao tác nào tùy thuộc vào giá trị huy chương. Chúng tôi thực hiện một lượng công việc không đổi cho mỗi trường hợp thử nghiệm, do đó tổng công việc tăng tuyến tính theo số lượng trường hợp. 

Mẫu được cung cấp trong câu lệnh chứa năm trường hợp thử nghiệm sáu số nguyên nhưng không hiển thị phần đầu`n`, mặc dù mô tả đầu vào bằng văn bản nói rằng`n`nên có mặt. Giải pháp dưới đây chấp nhận cả hai hình thức: chính thức`n`-định dạng tiền tố và định dạng sáu số nguyên cho mỗi trường hợp của mẫu. Điều này không làm thay đổi thuật toán hoặc độ phức tạp của nó. 

Một trường hợp phổ biến là hòa về tổng số huy chương trong khi Hoa Kỳ thắng theo màu. Ví dụ,```
1
10 5 5 8 6 6
```Hoa Kỳ có`20`huy chương và Nga có`20`, vì vậy Hoa Kỳ không thắng theo số đếm. Tuy nhiên, Mỹ có nhiều huy chương vàng hơn,`10`so với`8`, vậy kết quả đúng là`color`. Một giải pháp bất cẩn chỉ quyết định câu trả lời từ tổng số huy chương sẽ từ chối chiến thắng màu sắc của Hoa Kỳ một cách không chính xác. 

Tình huống ngược lại cũng có thể xảy ra. Ví dụ,```
1
8 5 5 10 4 3
```Hoa Kỳ có`18`huy chương trong khi Nga có`17`, do đó Hoa Kỳ thắng theo số đếm. Nga có nhiều huy chương vàng hơn, tuy nhiên,`10`so với`8`, nên Mỹ mất thứ hạng về màu sắc. Kết quả đúng là`count`. Một giải pháp coi hai hệ thống xếp hạng là tương đương nhau sẽ giải quyết sai trường hợp này. 

Một trường hợp tế nhị khác là chiếc cà vạt bằng vàng phải được giải quyết bằng bạc. Ví dụ,```
1
5 8 1 5 7 10
```Cả hai nước đều có 5 huy chương vàng nên vàng không thể quyết định thứ hạng màu sắc. Hoa Kỳ có tám huy chương bạc so với bảy huy chương của Nga, vì vậy Hoa Kỳ thắng theo màu sắc. Đồng không còn phù hợp nữa một khi bạc đã phá vỡ mối ràng buộc. 

Logic tương tự áp dụng sâu hơn một cấp khi cả vàng và bạc đều bằng nhau. Ví dụ,```
1
5 7 9 5 7 8
```Số lượng vàng bằng nhau và số lượng bạc bằng nhau nên đồng quyết định thứ hạng màu. Hoa Kỳ có chín huy chương đồng so với tám huy chương của Nga, mang lại cho Hoa Kỳ chiến thắng màu sắc. 

Cuối cùng, sự ràng buộc chính xác trong cả hai hệ thống phải tạo ra`none`, không phải là một chiến thắng. Ví dụ,```
1
5 7 9 5 7 9
```Cả tổng số và cả ba màu huy chương đều giống nhau nên không có sự so sánh nào lớn hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là tính tổng số huy chương của Hoa Kỳ và của Nga, sau đó so sánh ba huy chương lần lượt để xếp hạng màu. Điều này đã đủ để giải quyết vấn đề vì chỉ có sáu giá trị đầu vào cho mỗi trường hợp thử nghiệm. So sánh tổng thể cần hai lần bổ sung và so sánh màu sắc cần nhiều nhất ba so sánh. 

Người ta có thể mô tả một chiến lược bạo lực theo nghĩa đen hơn là kiểm tra mọi thứ tự có thể có của ba màu huy chương và cố gắng xác định thứ tự nào phù hợp với quy tắc xếp hạng. chỉ có`3! = 6`những thứ tự như vậy, do đó, ngay cả cách tiếp cận đó cũng thực hiện tối đa một số lượng thao tác không đổi cho mỗi trường hợp. Với`n`trường hợp thử nghiệm, trường hợp xấu nhất là đại khái`6n`yêu cầu kiểm tra, vẫn còn`O(n)`. Không có kích thước đầu vào thực tế nào khiến kích thước đầu vào này trở nên quá chậm theo các ràng buộc đã nêu. Vấn đề có kích thước cố định đủ để việc tối ưu hóa hữu ích không phải là một cải tiến tiệm cận mà nhận ra rằng ngay từ đầu không cần tìm kiếm hoặc sắp xếp. 

Quan sát quan trọng là thứ hạng màu sắc đã được mô tả đầy đủ theo thứ tự`(gold, silver, bronze)`. Chúng ta không cần phải xây dựng bảng xếp hạng, phân loại huy chương hay ấn định trọng số cho các loại huy chương. Chúng ta chỉ đơn giản so sánh hai bộ ba này theo từ điển. So sánh bộ dữ liệu của Python thể hiện chính xác quy tắc này, mặc dù giải pháp cũng có thể viết ba so sánh một cách rõ ràng để làm cho lý luận trở nên minh bạch. 

Ý tưởng brute-force hoạt động vì lượng dữ liệu trong một trường hợp là không đổi, nhưng nó đưa ra những công việc không cần thiết bằng cách xem xét các khả năng mà vấn đề đã sắp xếp cho chúng ta. Quan sát rằng xếp hạng màu là so sánh từ điển trực tiếp làm giảm toàn bộ nhiệm vụ thành hai vị từ có thời gian không đổi, một cho tổng số và một cho màu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận nhưng công việc không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc sáu số huy chương cho một trường hợp kiểm tra và giữ chúng theo thứ tự ban đầu. Việc in các giá trị này không thay đổi là một phần của kết quả đầu ra được yêu cầu, do đó không có lý do gì để xây dựng lại đầu vào sau này. 
2. Tính tổng số huy chương của Hoa Kỳ như sau`ug + us + ub`và tổng số của Nga là`rg + rs + rb`. Bộ`wins_count`tùy theo tổng số tiền của Hoa Kỳ có lớn hơn hay không. Bình đẳng không phải là chiến thắng vì tuyên bố hỏi liệu Hoa Kỳ có thắng hay không, chứ không phải Hoa Kỳ có hòa hay không. 
3. So sánh bộ ba huy chương`(ug, us, ub)`Và`(rg, rs, rb)`theo thứ tự vàng, bạc, đồng. Hoa Kỳ giành chiến thắng trong bảng xếp hạng màu nếu số vàng của nước đó lớn hơn hoặc nếu số vàng hòa và số bạc của nước đó lớn hơn hoặc nếu cả vàng và bạc đều hòa và số đồng của nước đó lớn hơn. 
4. Kết hợp hai kết quả boolean. Nếu cả hai đều đúng, hãy in`both`. Nếu chỉ có kết quả đếm là đúng, hãy in`count`. Nếu chỉ có kết quả màu là đúng, hãy in`color`. Nếu cả hai đều không đúng, hãy in`none`. 
5. In một dòng trống sau kết quả của test case. Điều này phù hợp với cấu trúc đầu ra được yêu cầu và cũng giữ cho các trường hợp thử nghiệm liên tiếp được tách biệt một cách trực quan. 

Bất biến chính là sau khi so sánh tổng thể,`wins_count`điều này đúng chính xác khi Hoa Kỳ có nhiều huy chương hơn về tổng thể. Sau khi so sánh màu sắc,`wins_color`đúng chính xác khi loại huy chương đầu tiên mà các quốc gia khác nhau thuộc về Hoa Kỳ và có số lượng lớn hơn. Vì mọi kết quả có thể xảy ra đều được xác định bởi hai vị từ độc lập này nên việc phân loại theo bốn chiều cuối cùng không thể sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))

    if not data:
        return

    # The written statement includes n, while the supplied sample
    # omits it. Support both formats.
    if len(data) >= 1 and (len(data) - 1) % 6 == 0 and data[0] == (len(data) - 1) // 6:
        n = data[0]
        start = 1
    else:
        n = len(data) // 6
        start = 0

    out = []

    for i in range(n):
        a = data[start + 6 * i:start + 6 * i + 6]
        ug, us, ub, rg, rs, rb = a

        usa_total = ug + us + ub
        russia_total = rg + rs + rb
        wins_count = usa_total > russia_total

        if ug != rg:
            wins_color = ug > rg
        elif us != rs:
            wins_color = us > rs
        else:
            wins_color = ub > rb

        if wins_count and wins_color:
            result = "both"
        elif wins_count:
            result = "count"
        elif wins_color:
            result = "color"
        else:
            result = "none"

        out.append(" ".join(map(str, a)))
        out.append(result)
        out.append("")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu tiên chương trình sẽ đọc tất cả các số nguyên cùng một lúc. Điều này tránh việc phụ thuộc vào việc các trường hợp thử nghiệm có được sắp xếp vật lý trên mỗi dòng hay không, bởi vì khoảng trắng là dấu phân cách duy nhất có liên quan cho đầu vào số nguyên. 

Tính năng phát hiện định dạng xử lý sự khác biệt giữa báo cáo bằng văn bản và mẫu được cung cấp. Nếu số đầu tiên bằng số nhóm sáu số nguyên còn lại thì được hiểu là`n`. Nếu không, tất cả các số nguyên sẽ được coi là dữ liệu huy chương. Theo định dạng chính thức, đầu vào bắt đầu bằng`5`tiếp theo là ba mươi giá trị huy chương được công nhận là năm trường hợp. Theo định dạng mẫu được cung cấp, ba mươi giá trị huy chương được công nhận là năm trường hợp. 

Đối với mỗi trường hợp, sáu giá trị được chia thành ba số lượng của Hoa Kỳ và ba số lượng của Nga. Tổng xếp hạng sử dụng phép cộng số nguyên thông thường và một quy tắc nghiêm ngặt`>`so sánh. 

Xếp hạng màu được viết có chủ ý dưới dạng so sánh có điều kiện rõ ràng thay vì dựa vào cú pháp bộ dữ liệu. Nếu vàng khác, quyết định được đưa ra ngay lập tức vì bạc và đồng không còn quan trọng nữa. Nếu vàng bị ràng buộc, bạc cũng được đối xử như vậy. Chỉ khi cả hai đều hòa thì chúng ta mới so sánh đồng. Điều này phản ánh trực tiếp quy tắc xếp hạng và tránh việc vô tình coi các loại huy chương có giá trị như nhau. 

Chuỗi điều kiện cuối cùng xử lý bốn kết hợp có thể có của hai bảng xếp hạng. Không có vấn đề tràn số nguyên trong Python và ngay cả trong ngôn ngữ có chiều rộng cố định, tổng tối đa chỉ là`1500`, vì mỗi quốc gia có tối đa ba huy chương`500`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét trường hợp được cung cấp đầu tiên:```
10 5 15 10 1 0
```Những thay đổi trạng thái chính là: 

| Hoa Kỳ`(G,S,B)`| Nga`(G,S,B)`| Tổng số Hoa Kỳ | Nga Tổng cộng | Đếm Thắng | Chiến thắng màu sắc | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
|`(10,5,15)`|`(10,1,0)`| 30 | 11 | đúng | đúng | cả hai | 

Sự so sánh tổng thể mang lại cho Hoa Kỳ`30`huy chương chống lại Nga`11`, do đó Hoa Kỳ thắng theo số đếm. Vàng được buộc ở`10`, do đó sự so sánh màu chuyển sang màu bạc. Hoa Kỳ có năm huy chương bạc so với Nga, vì vậy Hoa Kỳ cũng thắng về màu sắc. Sự phân loại cuối cùng là`both`. 

### Mẫu 2 

Trường hợp được cung cấp thứ hai là:```
10 5 15 10 6 10
```Nhà nước là: 

| Hoa Kỳ`(G,S,B)`| Nga`(G,S,B)`| Tổng số Hoa Kỳ | Nga Tổng cộng | Đếm Thắng | Chiến thắng màu sắc | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
|`(10,5,15)`|`(10,6,10)`| 30 | 26 | đúng | sai | đếm | 

Mỹ vẫn có tổng số huy chương nhiều hơn`30`so với`26`, vì vậy việc so sánh số lượng là đúng. Vàng được buộc ở`10`, nhưng Nga có sáu huy chương bạc trong khi Mỹ có năm huy chương. Bạc phá vỡ sự so sánh màu sắc, nên Mỹ thua về màu sắc. Câu trả lời là do đó`count`. 

Hai ví dụ này chứng minh tại sao hai hệ thống xếp hạng phải được đánh giá độc lập. Lần tiếp cận đầu tiên`both`, trong khi thứ hai đạt tới`count`mặc dù số lượng vàng là giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trong số`n`các trường hợp thử nghiệm thực hiện một số phép tính số học và so sánh cố định. | 
| Không gian | O(n) | Việc triển khai lưu trữ các chuỗi đầu vào và đầu ra hoàn chỉnh, do đó mức sử dụng bộ nhớ thực tế của nó tăng tuyến tính với kích thước đầu vào. Không gian làm việc thuật toán cho một trường hợp là O(1). | 

Chỉ với sáu số nguyên cho mỗi trường hợp thử nghiệm và số lượng tính toán không đổi cho mỗi trường hợp, quá trình xử lý dễ dàng đủ nhanh đối với các ràng buộc đã nêu. Số huy chương nhiều nhất là`500`, vì vậy số học là tầm thường. Bộ nhớ tuyến tính duy nhất đến từ việc triển khai toàn bộ đầu vào và đầu ra được lưu vào bộ đệm một cách thuận tiện chứ không phải từ lý luận của thuật toán. 

Nếu ưu tiên triển khai phát trực tuyến nghiêm ngặt, bộ nhớ làm việc có thể giảm xuống`O(1)`vượt ra ngoài bộ đệm đầu ra bằng cách đọc từng trường hợp kiểm thử một lần. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(data: str) -> str:
    values = list(map(int, data.split()))

    if not values:
        return ""

    if len(values) >= 1 and (len(values) - 1) % 6 == 0 \
            and values[0] == (len(values) - 1) // 6:
        n = values[0]
        start = 1
    else:
        n = len(values) // 6
        start = 0

    out = []

    for i in range(n):
        a = values[start + 6 * i:start + 6 * i + 6]
        ug, us, ub, rg, rs, rb = a

        wins_count = ug + us + ub > rg + rs + rb

        if ug != rg:
            wins_color = ug > rg
        elif us != rs:
            wins_color = us > rs
        else:
            wins_color = ub > rb

        if wins_count and wins_color:
            result = "both"
        elif wins_count:
            result = "count"
        elif wins_color:
            result = "color"
        else:
            result = "none"

        out.append(" ".join(map(str, a)))
        out.append(result)
        out.append("")

    return "\n".join(out)

def run(inp: str) -> str:
    return solve_data(inp)

sample = """10 5 15 10 1 0
10 5 15 10 6 10
12 5 10 5 20 30
10 0 15 10 5 30
10 5 15 10 5 15
"""

sample_expected = """10 5 15 10 1 0
both
10 5 15 10 6 10
count
12 5 10 5 20 30
color
10 0 15 10 5 30
none
10 5 15 10 5 15
none
"""

assert run(sample) == sample_expected, "provided sample"

sample_with_n = """5
10 5 15 10 1 0
10 5 15 10 6 10
12 5 10 5 20 30
10 0 15 10 5 30
10 5 15 10 5 15
"""

assert run(sample_with_n) == sample_expected, "official n-prefixed format"

minimum = """1
0 0 0 0 0 0
"""

assert run(minimum) == """0 0 0 0 0 0
none
""", "minimum and complete tie"

maximum = """1
500 500 500 499 499 499
"""

assert run(maximum) == """500 500 500 499 499 499
both
""", "maximum medal counts"

all_equal = """1
10 20 30 10 20 30
"""

assert run(all_equal) == """10 20 30 10 20 30
none
""", "all values equal"

gold_tie_silver_win = """1
5 8 1 5 7 10
"""

assert run(gold_tie_silver_win) == """5 8 1 5 7 10
color
""", "silver breaks a gold tie"

count_only = """1
8 5 5 10 4 3
"""

assert run(count_only) == """8 5 5 10 4 3
count
""", "count wins while color loses"

bronze_breaks_tie = """1
5 7 9 5 7 8
"""

assert run(bronze_breaks_tie) == """5 7 9 5 7 8
color
""", "bronze breaks gold and silver ties"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 0 0 0`|`none`| Giá trị tối thiểu và đẳng thức chính xác | 
|`500 500 500 499 499 499`|`both`| Giá trị tối đa và cả thứ hạng | 
|`10 20 30 10 20 30`|`none`| Hoàn toàn bình đẳng trên tất cả các loại huy chương | 
|`5 8 1 5 7 10`|`color`| So sánh bạc sau hòa vàng | 
|`8 5 5 10 4 3`|`count`| Đếm thắng trong khi thua xếp hạng màu | 
|`5 7 9 5 7 8`|`color`| So sánh đồng sau khi hòa vàng và bạc | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là hòa chính xác. Vì```
1
5 7 9 5 7 9
```cả hai tổng số đều là`21`, Vì thế`wins_count`là sai. Vàng buộc, bạc buộc, đồng cũng buộc nên so sánh màu sắc cuối cùng là`9 > 9`, điều đó là sai. Cả hai vị từ đều sai, cho`none`. Một so sánh không nghiêm ngặt như`>=`sẽ phân loại không chính xác đây là chiến thắng của Hoa Kỳ. 

Hòa về tổng số huy chương không ngăn cản việc giành chiến thắng về màu sắc. Vì```
1
10 5 5 8 6 6
```cả hai nước đều có`20`huy chương, vì vậy việc so sánh số lượng không thành công. Vàng quyết định thứ hạng màu sắc ngay lập tức vì`10 > 8`, cho`color`. Thuật toán không bao giờ cần kiểm tra bạc hoặc đồng khi vàng có sự khác biệt. 

Trường hợp ngược lại cũng có thể xảy ra. Vì```
1
8 5 5 10 4 3
```Hoa Kỳ có`18`huy chương và Nga có`17`, vậy là Hoa Kỳ thắng. Vàng đi theo hướng khác,`8 < 10`, vì vậy màu sắc bị mất. Hai boolean trở thành`true`Và`false`, sản xuất`count`. 

Khi buộc vàng, phải kiểm tra bạc trước đồng. Vì```
1
5 8 1 5 7 10
```sự so sánh vàng không thuyết phục vì cả hai giá trị đều`5`. Bạc mang lại chiến thắng cho Hoa Kỳ với`8 > 7`, vậy kết quả là`color`. Việc thực hiện bất cẩn chỉ so sánh vàng và tuyên bố hòa sẽ bỏ lỡ chiến thắng này. 

Khi cả vàng và bạc đều hòa nhau thì đồng trở nên quyết định. Vì```
1
5 7 9 5 7 8
```Hai lần so sánh đầu tiên bằng nhau, nhưng Hoa Kỳ có chín huy chương đồng so với tám huy chương của Nga. Vị từ màu sắc là đúng, tạo ra`color`. 

Cuối cùng, giá trị tối đa```
1
500 500 500 499 499 499
```đưa Hoa Kỳ`1500`huy chương chống lại Nga`1497`, và Mỹ cũng có nhiều huy chương vàng hơn. Cả hai vị từ đều đúng nên kết quả là`both`. Điều này xác nhận rằng phép tính số học và phép so sánh hoạt động bình thường ở giới hạn trên của phạm vi số huy chương đã nêu.
