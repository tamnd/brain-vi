---
title: "CF 102433G - Phát sáng, Điểm ảnh nhỏ, Phát sáng"
description: "Mỗi xung truyền dọc theo đúng một dây, theo chiều ngang hoặc chiều dọc. Xung ngang trên dây a bắt đầu ở cạnh trái tại thời điểm t, trong khi xung dọc trên dây a bắt đầu ở cạnh dưới cùng loại thời gian tham chiếu."
date: "2026-08-12T07:32:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "G"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 86
verified: true
draft: false
---

[CF 102433G - Phát sáng, Điểm ảnh nhỏ, Phát sáng](https://codeforces.com/problemset/problem/102433/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi xung truyền dọc theo đúng một dây, theo chiều ngang hoặc chiều dọc. Xung ngang trên dây`a`bắt đầu ở cạnh trái vào thời điểm`t`, trong khi một xung thẳng đứng trên dây`a`bắt đầu ở cạnh dưới cùng loại thời gian tham chiếu. Cả hai đều di chuyển với tốc độ một và mỗi xung có thời lượng`m`. 

Hãy xem xét một xung ngang và một xung dọc. Chúng giao nhau tại đúng một pixel, được xác định bởi số dây của chúng. Câu hỏi duy nhất là liệu hai xung có xuất hiện đồng thời ở pixel đó hay không. 

Đối với xung ngang trên dây`a`, cạnh đầu của nó chạm tới pixel trên dây dọc`b`vào thời điểm đó`t + b`. Cạnh cuối của nó đạt đến cùng một pixel tại một thời điểm`t + b + m`. Đối với xung dọc trên dây`b`, khoảng tương ứng là`[t_v + a, t_v + a + m_v)`. Cách giải thích nửa mở rất hữu ích vì tuyên bố nói rõ ràng rằng việc chạm vào một lúc sẽ không kích hoạt pixel. 

Có một cách rõ ràng hơn nhiều để thể hiện những khoảng thời gian đó. Đối với xung ngang, trừ số dây ngang của nó khỏi thời gian bắt đầu và xác định`start = t - a`. 

Đối với xung dọc, thực hiện tương tự với số dây dọc của nó. Sau phép biến đổi này, hai xung va chạm chính xác khi các khoảng thời gian biến đổi của chúng trùng với độ dài dương. Một xung`(t, m, a)`trở thành khoảng`[t - a, t - a + m)`. 

Do đó, chuyển động hai chiều của các xung đã trở thành bài toán chồng lấp khoảng một chiều. Phân tích chính thức của cuộc thi mô tả ý tưởng hình học tương tự như chiếu cả hai đường xung lên đường chéo`x = y`, trong đó cả hai chuỗi xung đều di chuyển với cùng tốc độ. 

Chỉ các xung ngang và dọc mới có thể kích hoạt một pixel cùng nhau. Hai xung ngang không bao giờ gặp nhau ở một pixel vì chúng truyền trên các hàng khác nhau và hai xung dọc có đặc tính tương tự. Vì vậy, câu trả lời chỉ đơn giản là số lượng các cặp chồng lên nhau bao gồm một khoảng ngang và một khoảng dọc. 

Số xung nhiều nhất là`200000`. Việc so sánh trực tiếp giữa mọi xung ngang và xung dọc có thể yêu cầu khoảng`100000 * 100000 = 10^10`so sánh, vượt xa những gì một chương trình cuộc thi hai giây có thể thực hiện. Số dây nhiều nhất là`100000`, nhưng việc xây dựng một lưới có kích thước như vậy sẽ không giải quyết được vấn đề thực sự, bởi vì chiều quan trọng là số cặp xung chứ không phải số lượng dây. Thời gian và độ dài bắt đầu cũng đủ lớn để bất kỳ giải pháp nào phụ thuộc vào đơn vị thời gian mô phỏng theo đơn vị thời gian sẽ không phù hợp. 

Một số trường hợp ranh giới rất dễ bị xử lý sai. 

Xét hai xung chỉ chạm nhau:```
2
h 1 1 1
v 2 1 1
```Khoảng cách ngang là`[0, 1)`và khoảng dọc là`[1, 2)`. Câu trả lời là`0`. Việc thực hiện bất cẩn bằng cách sử dụng`start <= other_end`hoặc xử lý các khoảng thời gian khép kín sẽ tính xung đột này mặc dù các xung không bao giờ xuất hiện đồng thời. 

Xem xét các vị trí bắt đầu bằng nhau:```
2
h 1 3 1
v 1 3 1
```Cả hai khoảng biến đổi đều là`[0, 3)`, vậy câu trả lời là`1`. Bắt đầu bằng nhau được phép chồng chéo ngay lập tức. 

Một xung cũng có thể chứa hoàn toàn một xung khác:```
2
h 1 10 1
v 3 2 1
```Các khoảng là`[0, 10)`Và`[2, 4)`, vậy câu trả lời là`1`. Việc chỉ kiểm tra xem một khoảng thời gian có bắt đầu bên trong khoảng thời gian kia hay không sẽ bỏ lỡ một số va chạm hợp lệ. 

Cuối cùng, các xung truyền cùng hướng không được tính:```
2
h 1 10 1
h 5 10 2
```Không có xung dọc nên câu trả lời là`0`, mặc dù các khoảng biến đổi của chúng trùng nhau. 

## Phương pháp tiếp cận 

Giải pháp vũ phu được rút ra trực tiếp từ cách giải thích vật lý. Lưu trữ tất cả các xung ngang và tất cả các xung dọc, sau đó kiểm tra từng cặp xung ngang. Đối với một cặp, hãy tính hai khoảng thời gian đến tại giao điểm của chúng và kiểm tra xem chúng có trùng nhau với độ dài dương hay không. Điều này đúng vì mỗi pixel được kích hoạt tương ứng với chính xác một xung ngang và một xung dọc và mỗi cặp như vậy có chính xác một pixel giao nhau. 

Vấn đề là số lượng cặp. Nếu có`100000`xung ngang và`100000`xung dọc, thuật toán thực hiện`10^10`kiểm tra cặp. Ngay cả một hằng số rất nhỏ trên mỗi lần so sánh cũng không đủ cho giới hạn hai giây. 

Quan sát hữu ích là tọa độ dây chính xác sẽ biến mất sau khi chuyển đổi đúng. Xung ngang với thời gian bắt đầu`t`, chiều dài`m`và hàng`a`đạt đến hình chiếu chéo với một khoảng bắt đầu từ`t - a`. Một xung dọc với thời gian bắt đầu`t`, chiều dài`m`và cột`a`nhận được chính xác sự đại diện tương tự. Vì vậy, mỗi xung trở thành một khoảng thông thường trên một dòng. 

Bây giờ nhiệm vụ là: cho các khoảng ngang và khoảng dọc, đếm xem có bao nhiêu cặp chồng lên nhau với độ dài dương. 

Trong một khoảng ngang`[L, R)`, một khoảng dọc`[S, E)`trùng lặp nó chính xác khi`S < R`Và`E > L`. 

Chúng ta có thể đếm điều kiện đầu tiên bằng cách sắp xếp tất cả các điểm bắt đầu theo chiều dọc và sử dụng tìm kiếm nhị phân. Chúng ta có thể đếm điều kiện thứ hai bằng cách sắp xếp tất cả các đầu dọc và sử dụng tìm kiếm nhị phân khác. Số thỏa mãn cả hai điều kiện là`number of vertical starts < R - number of vertical ends <= L`. 

Điều này tránh mọi cấu trúc dữ liệu hai chiều. Mỗi khoảng ngang được xử lý bằng hai tìm kiếm nhị phân sau khi các điểm cuối dọc đã được sắp xếp. Thay vào đó, bài xã luận chính thức mô tả một quá trình quét tương đương đối với bốn loại sự kiện theo khoảng thời gian, duy trì số lượng khoảng thời gian theo chiều ngang và chiều dọc đang hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng xung và biến nó thành khoảng một chiều. Nếu xung có thời gian bắt đầu`t`, số dây`a`, và chiều dài`m`, tính toán`L = t - a`Và`R = L + m`. Cửa hàng`[L, R)`trong bộ sưu tập ngang hoặc dọc theo hướng của nó. Phép trừ bằng`a`chiếm thêm thời gian di chuyển cần thiết để đến được điểm ảnh giao nhau. 
2. Tách các điểm cuối của tất cả các khoảng dọc. Lưu trữ mọi điểm bắt đầu theo chiều dọc trong một mảng và mọi điểm kết thúc theo chiều dọc trong một mảng khác. Sắp xếp cả hai mảng. Chúng ta chỉ cần các khoảng dọc cho các truy vấn vì mọi xung đột hợp lệ đều có chính xác một xung ngang và một xung dọc. 
3. Đối với mỗi khoảng ngang`[L, R)`, tìm xem có bao nhiêu khoảng dọc bắt đầu trước`R`. Với một mảng bắt đầu được sắp xếp,`bisect_left(starts, R)`đưa ra chính xác số lượng này. Bất đẳng thức chặt chẽ là có chủ ý, bởi vì một khoảng bắt đầu chính xác tại`R`chỉ chạm vào khoảng ngang tại điểm cuối của nó. 
4. Tìm bao nhiêu khoảng dọc kết thúc tại hoặc trước`L`. Với một mảng kết thúc được sắp xếp,`bisect_right(ends, L)`đưa ra số lượng này. Khoảng thời gian như vậy không thể chồng lên nhau`[L, R)`, bởi vì dòng điện của họ đã biến mất theo thời gian`L`. 
5. Trừ số thứ hai cho số thứ nhất. Mọi khoảng dọc còn lại đều thỏa mãn cả hai`S < R`Và`E > L`, do đó, nó chồng lên khoảng thời gian theo chiều ngang trong một khoảng thời gian dương. Thêm số này vào câu trả lời. 
6. In câu trả lời tích lũy. Số nguyên Python không bị giới hạn, điều này rất hữu ích vì càng nhiều`100000 * 100000 = 10^10`pixel có thể được kích hoạt. 

### Tại sao nó hoạt động 

Đối với mỗi cặp xung ngang và dọc, các khoảng biến đổi sẽ bảo toàn chính xác liệu dòng điện của chúng có cùng tồn tại ở pixel tương ứng hay không. Hai khoảng biến đổi`[L, R)`Và`[S, E)`trùng lặp chính xác khi`S < R`Và`E > L`. Đối với mỗi khoảng ngang, tìm kiếm nhị phân đầu tiên sẽ tính tất cả các khoảng dọc thỏa mãn`S < R`, trong khi cái thứ hai loại bỏ chính xác những cái có`E <= L`. Mọi khoảng dọc còn lại sẽ chồng lên khoảng ngang và mọi khoảng dọc chồng chéo vẫn được tính. Vì mỗi cặp ngang-dọc tương ứng với một pixel nên số lượng tích lũy chính xác là số pixel được kích hoạt. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

def solve(reader=None):
    if reader is None:
        reader = sys.stdin.readline

    n = int(reader())

    horizontal = []
    vertical_starts = []
    vertical_ends = []

    for _ in range(n):
        direction, t, m, a = reader().split()
        t = int(t)
        m = int(m)
        a = int(a)

        left = t - a
        right = left + m

        if direction == 'h':
            horizontal.append((left, right))
        else:
            vertical_starts.append(left)
            vertical_ends.append(right)

    vertical_starts.sort()
    vertical_ends.sort()

    answer = 0

    for left, right in horizontal:
        starts_before_right = bisect_left(vertical_starts, right)
        ends_at_or_before_left = bisect_right(vertical_ends, left)
        answer += starts_before_right - ends_at_or_before_left

    return answer

if __name__ == "__main__":
    print(solve())
```Vòng lặp đầu vào thực hiện phép biến đổi hình học ngay lập tức, do đó sau đó không cần số dây ban đầu. Đối với một xung`(t, m, a)`,`left = t - a`Và`right = left + m`mô tả khoảng thời gian trên đường chiếu. 

Chỉ các điểm cuối dọc cần được sắp xếp. Đối với mỗi khoảng ngang,`bisect_left(vertical_starts, right)`số đếm bắt đầu đúng trước điểm cuối bên phải. sử dụng`bisect_left`còn hơn là`bisect_right`là những gì loại trừ các khoảng thời gian chỉ chạm vào`right`. 

Đối với ranh giới khác,`bisect_right(vertical_ends, left)`số lượng kết thúc nhỏ hơn hoặc bằng điểm cuối bên trái. sử dụng`bisect_right`là thứ loại bỏ những khoảng thời gian kết thúc chính xác tại`left`, khớp với quy tắc của câu lệnh rằng liên hệ điểm cuối không kích hoạt pixel. 

Câu trả lời có thể đạt được`10^10`, do đó số nguyên 32 bit sẽ không đủ trong các ngôn ngữ có số nguyên có chiều rộng cố định. Kiểu số nguyên của Python xử lý trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bốn xung trở thành khoảng thời gian dự kiến sau đây. 

| Hướng |`t`|`m`|`a`| Khoảng thời gian | 
| --- | --- | --- | --- | --- | 
| h | 1 | 4 | 1 |`[0, 4)`| 
| v | 2 | 4 | 2 |`[0, 4)`| 
| h | 10 | 2 | 2 |`[8, 10)`| 
| v | 11 | 2 | 3 |`[8, 10)`| 

Bắt đầu theo chiều dọc là`[0, 8]`, và các đầu dọc là`[4, 10]`. 

| Khoảng ngang | Bắt đầu`< right`| Kết thúc`<= left`| Va chạm mới | Trả lời | 
| --- | --- | --- | --- | --- | 
|`[0, 4)`| 1 | 0 | 1 | 1 | 
|`[8, 10)`| 2 | 1 | 1 | 2 | 

Xung ngang đầu tiên chồng lên xung dọc đầu tiên. Xung ngang thứ hai chồng lên xung dọc thứ hai. Kết quả là`2`. 

### Mẫu 2 

Các khoảng biến đổi là 

| Hướng |`t`|`m`|`a`| Khoảng thời gian | 
| --- | --- | --- | --- | --- | 
| h | 1 | 10 | 1 |`[0, 10)`| 
| h | 5 | 10 | 2 |`[3, 13)`| 
| v | 1 | 10 | 1 |`[0, 10)`| 
| v | 5 | 10 | 3 |`[2, 12)`| 

Bắt đầu theo chiều dọc là`[0, 2]`, và các đầu dọc là`[10, 12]`. 

| Khoảng ngang | Bắt đầu`< right`| Kết thúc`<= left`| Va chạm mới | Trả lời | 
| --- | --- | --- | --- | --- | 
|`[0, 10)`| 2 | 0 | 2 | 2 | 
|`[3, 13)`| 2 | 0 | 2 | 4 | 

Mỗi khoảng ngang chồng lên cả hai khoảng dọc, do đó cả bốn cặp ngang-dọc đều kích hoạt pixel. Kết quả là`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp các điểm cuối dọc có chi phí O(n log n), theo sau là hai tìm kiếm nhị phân cho mỗi xung ngang | 
| Không gian | O(n) | Các khoảng ngang và hai mảng điểm cuối dọc, mỗi mảng chứa tối đa n phần tử | 

Với`n <= 200000`, O(n log n) chỉ yêu cầu vài triệu thao tác sắp xếp và tìm kiếm nhị phân, phù hợp với giới hạn hai giây. Việc sử dụng bộ nhớ là tuyến tính theo số xung. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm sau đây sử dụng cùng một`solve`hoạt động như một giải pháp được gửi, đồng thời chuyển một trình đọc trong bộ nhớ để mỗi trường hợp có thể được kiểm tra độc lập.```
import io

def run(inp: str) -> str:
    return str(solve(io.StringIO(inp)))

# Provided sample 1
assert run("""\
4
h 1 4 1
v 2 4 2
h 10 2 2
v 11 2 3
""") == "2", "sample 1"

# Provided sample 2
assert run("""\
4
h 1 10 1
h 5 10 2
v 1 10 1
v 5 10 3
""") == "4", "sample 2"

# Provided sample 3
assert run("""\
7
v 1 3 1
v 1 15 2
h 4 5 1
h 5 5 2
h 6 5 3
h 7 5 4
h 8 5 5
""") == "5", "sample 3"

# Minimum-size input: one pulse cannot activate any pixel.
assert run("""\
1
h 1 1 1
""") == "0", "single pulse"

# Exact endpoint touching: [0, 1) and [1, 2) do not overlap.
assert run("""\
2
h 1 1 1
v 2 1 1
""") == "0", "endpoint touching"

# Same transformed interval, so the two pulses overlap completely.
assert run("""\
2
h 1 3 1
v 1 3 1
""") == "1", "identical intervals"

# A vertical interval is completely contained inside a horizontal interval.
assert run("""\
2
h 1 10 1
v 3 2 1
""") == "1", "contained interval"

# Same direction pulses never activate a pixel together.
assert run("""\
2
h 1 10 1
h 5 10 2
""") == "0", "same direction"

# Maximum-size case: 100000 horizontal and 100000 vertical pulses.
# All transformed intervals are [100000, 300000), so every pair collides.
lines = ["200000"]
for a in range(1, 100001):
    lines.append(f"h {100000 + a} 200000 {a}")
for a in range(1, 100001):
    lines.append(f"v {100000 + a} 200000 {a}")

assert run("\n".join(lines) + "\n") == "10000000000", "maximum-size all-overlapping case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`xung ngang |`0`| Đầu vào kích thước tối thiểu | 
| Hai khoảng thời gian`[0,1)`Và`[1,2)`|`0`| Ranh giới điểm cuối chính xác | 
| Hai khoảng giống hệt nhau |`1`| Bắt đầu bằng nhau và chồng chéo hoàn toàn | 
|`[0,10)`Và`[2,4)`|`1`| Ngăn chặn | 
| Hai xung ngang |`0`| Chỉ có hướng ngược nhau mới có thể va chạm | 
|`200000`khoảng thời gian dự kiến ​​bằng nhau |`10000000000`| Kích thước đầu vào tối đa và câu trả lời có kích thước 64 bit | 

## Vỏ cạnh 

Trường hợp chạm vào điểm cuối được xử lý theo ranh giới tìm kiếm nhị phân nghiêm ngặt. Vì```
2
h 1 1 1
v 2 1 1
```xung ngang trở thành`[0, 1)`và xung dọc trở thành`[1, 2)`.`bisect_left(starts, 1)`không tính điểm bắt đầu theo chiều dọc tại`1`, vậy câu trả lời là`0`. Điều này trực tiếp mã hóa quy tắc rằng sự hiện diện đồng thời phải kéo dài trong một khoảng thời gian nhất định. 

Đối với vị trí xuất phát bằng nhau,```
2
h 1 3 1
v 1 3 1
```cả hai khoảng đều là`[0, 3)`. Bắt đầu theo chiều dọc`0`hoàn toàn nằm trước điểm cuối bên phải theo chiều ngang`3`, và đầu thẳng đứng`3`hoàn toàn nằm sau điểm cuối ngang bên trái`0`. Cặp đôi được tính, cho`1`. 

Để ngăn chặn,```
2
h 1 10 1
v 3 2 1
```các khoảng là`[0, 10)`Và`[2, 4)`. Sự khởi đầu theo chiều dọc thỏa mãn`2 < 10`, và điểm cuối của nó thỏa mãn`4 > 0`, do đó nó đóng góp một va chạm. Phương pháp này không giả định rằng một trong hai khoảng phải chứa điểm cuối của khoảng kia. 

Đối với các xung cùng hướng,```
2
h 1 10 1
h 5 10 2
```bộ sưu tập khoảng ngang chứa cả hai xung, trong khi mảng điểm cuối dọc trống. Mọi truy vấn theo chiều ngang đều đóng góp bằng 0, vì vậy câu trả lời là`0`. Điều này ngăn chặn các khoảng thời gian chiếu chồng chéo theo cùng một hướng khỏi bị nhầm lẫn với kích hoạt pixel vật lý. 

Đối với trường hợp kích thước tối đa, có`100000`ngang và`100000`xung dọc, mỗi xung được biến đổi thành`[100000, 300000)`. Mỗi khoảng ngang chồng lên mỗi khoảng dọc, tạo ra`10^10`các pixel được kích hoạt. Thuật toán không bao giờ liệt kê các cặp đó một cách riêng lẻ. Nó đếm chúng thông qua hai lần tìm kiếm nhị phân trên mỗi khoảng ngang, đó chính xác là lý do tại sao cách tiếp cận O(n log n) vẫn thực tế.
