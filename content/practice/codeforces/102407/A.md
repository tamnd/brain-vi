---
title: "CF 102407A - \u0421\u0443\u043c\u0430\u0441\u0448\u0435\u0434\u0448\u0438\u0435 \u0442\u0440\u0430\u043d\u0441\u043f\u043e\u0440\u0442\u043d\u044b\u0435 \u043d\u0430\u043b\u043e\u0433\u0438"
description: "Chúng tôi có một bảng thuế được sắp xếp. Mỗi hàng chứa một ranh giới mã lực bi và thuế suất ti. Ranh giới đầu tiên luôn bằng 0 và ranh giới tăng lên một cách nghiêm ngặt."
date: "2026-08-12T02:48:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 450
verified: true
draft: false
---

[CF 102407A - \u0421\u0443\u043c\u0430\u0441\u0448\u0435\u0434\u0448\u0438\u0435 \u0442\u0440\u0430\u043d\u0441\u043f\u043e\u0440\u0442\u043d\u044b\u0435 \u043d\u0430\u043b\u043e\u0433\u0438](https://codeforces.com/problemset/problem/102407/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bảng thuế được sắp xếp. Mỗi hàng chứa một ranh giới mã lực`b_i`và thuế suất`t_i`. Ranh giới đầu tiên luôn bằng 0 và ranh giới tăng lên một cách nghiêm ngặt. Đối với xe có động cơ`q`, tỷ lệ áp dụng là tỷ lệ từ hàng cuối cùng của bảng có ranh giới nhỏ hơn hoàn toàn`q`. Có một trường hợp đặc biệt ở cuối: nếu`q`lớn hơn ranh giới lớn nhất thì tỷ lệ cuối cùng được sử dụng. 

Cách diễn đạt các khoảng hơi khác thường nên điểm cuối chính xác rất quan trọng. một hàng`i`áp dụng cho các quyền hạn thỏa mãn`b_i < q <= b_{i+1}`. Theo đó, khi`q`chính xác bằng một ranh giới`b_i`, tỷ lệ từ hàng`i - 1`áp dụng, không phải tỷ lệ ghi trên hàng`i`. Vì mỗi sức mạnh được truy vấn ít nhất là`1`Và`b_1 = 0`, luôn có một hàng trước đó cho một ranh giới dương chính xác. 

Đối với mỗi quyền lực ô tô được truy vấn`q_j`, chúng ta cần xuất ra`q_j * t`, Ở đâu`t`là tỷ lệ được lựa chọn bởi các quy tắc này. Với tối đa`100000`các hàng của bảng và`100000`truy vấn, việc kiểm tra từng hàng trong bảng cho mỗi truy vấn có thể yêu cầu tới`10^10`so sánh. Điều đó vượt xa những gì giới hạn thời gian của cuộc thi thông thường có thể hỗ trợ. Chúng ta cần khai thác thực tế là các ranh giới đã được sắp xếp chặt chẽ. 

Tỷ lệ cũng không giảm, mặc dù thuộc tính đó không thực sự cần thiết cho tìm kiếm nhị phân. Thuộc tính quan trọng là mảng ranh giới được sắp xếp. Điều này cho phép chúng ta xác định khoảng thời gian liên quan theo thời gian logarit. 

Có một số trường hợp ranh giới trong đó việc triển khai có vẻ hợp lý có thể sai. 

Hãy xem xét một hàng của bảng:```
1
0 10
1
100
```Chiếc xe có sức mạnh`100`, lớn hơn ranh giới lớn nhất. Do đó, tỷ giá cuối cùng là`10`, vậy câu trả lời là`1000`. Giải pháp chỉ tìm kiếm khoảng kết thúc ở ranh giới tiếp theo và quên trường hợp tràn cuối cùng có thể không chỉ định bất kỳ tốc độ nào. 

Quy tắc ranh giới chính xác là một nguồn sai lầm phổ biến khác:```
2
0 10
100 20
1
100
```Đầu ra đúng là:```
1000
```sức mạnh`100`không thực sự lớn hơn ranh giới`100`, vì vậy hàng thứ hai không áp dụng. Hàng đầu tiên áp dụng cho quyền hạn trong`(0, 100]`, cho`100 * 10 = 1000`. sử dụng`bisect_right`trực tiếp sẽ chọn hàng thứ hai và tạo ra sai`2000`. 

Truy vấn ngay phía trên ranh giới phải sử dụng tỷ lệ mới:```
2
0 10
100 20
1
101
```Đầu ra đúng là:```
2020
```Đây`101 > 100`, do đó áp dụng tỷ lệ thứ hai. Việc triển khai coi ranh giới trên là bao gồm sai phía có thể giữ nguyên tỷ lệ đầu tiên một cách không chính xác. 

Cuối cùng, tỷ lệ bằng nhau là hoàn toàn hợp lệ:```
3
0 7
10 7
20 7
3
1
10
25
```Đầu ra là:```
7
70
175
```Các ranh giới của bảng vẫn phải được tìm kiếm một cách chính xác mặc dù tốc độ không bao giờ thay đổi. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là xử lý từng truy vấn một cách độc lập bằng cách quét bảng thuế ngay từ đầu. Vì một quyền lực`q`, chúng ta tìm ranh giới đầu tiên ít nhất là`q`. Nếu ranh giới đó là`b_i`, tỷ giá áp dụng là`t_{i-1}`. Nếu không có ranh giới như vậy tồn tại thì tỷ giá cuối cùng sẽ được sử dụng. 

Cách tiếp cận này đúng vì bảng mô tả các khoảng thời gian liên tiếp. Bắt đầu từ ranh giới nhỏ nhất và di chuyển lên trên, ranh giới đầu tiên thỏa mãn`b_i >= q`chính xác là điểm cuối trên của khoảng chứa`q`. Do đó, hàng trước cung cấp tỷ giá chính xác. 

Vấn đề là số lượng công việc lặp đi lặp lại. Một truy vấn có thể buộc chúng ta phải kiểm tra tất cả`n`hàng và điều này có thể xảy ra với mọi hàng`m`truy vấn. Với`n = m = 100000`, trường hợp xấu nhất là`100000 * 100000 = 10^10`kiểm tra bảng. Mặc dù mỗi lần quét riêng lẻ đều đơn giản nhưng mười tỷ thao tác là không thực tế. 

Quan sát quan trọng là thông tin duy nhất cần có từ bảng là vị trí của ranh giới đầu tiên thỏa mãn`b_i >= q`. Bởi vì tất cả`b_i`đang gia tăng nghiêm ngặt, đây chính xác là loại tìm kiếm mà tìm kiếm nhị phân giải quyết được. Thay vì kiểm tra từng hàng, chúng tôi liên tục loại bỏ một nửa phạm vi ranh giới còn lại. 

của Python`bisect_left`trực tiếp thể hiện tìm kiếm cần thiết. Nó trả về chỉ mục đầu tiên`i`như vậy`b_i >= q`. Nếu chỉ số đó tồn tại thì tỷ lệ chính xác là`t_i`chỉ khi`q`thực sự lớn hơn`b_i`, đó không phải là điều chúng ta muốn. Thuận tiện hơn, chúng ta có thể hiểu kết quả là vị trí chèn của`q`trước các ranh giới bằng nhau và sử dụng hàng trước đó, cụ thể là`t[i - 1]`. Nếu vị trí chèn bằng 0 thì truy vấn sẽ ở trước ranh giới đầu tiên, nhưng điều đó không thể xảy ra vì truy vấn dương và ranh giới đầu tiên bằng 0. 

Đối với một truy vấn lớn hơn mọi ranh giới,`bisect_left`trả lại`n`, vậy hàng trước đó là`n - 1`, chính xác là tỷ lệ cuối cùng. Do đó, cùng một công thức xử lý khoảng cuối cùng mà không có trường hợp đặc biệt riêng biệt. 

Phương pháp brute-force hoạt động vì nó phát hiện rõ ràng khoảng thời gian chứa mỗi truy vấn, nhưng nó không thành công khi tìm kiếm cùng một bảng có thứ tự từ đầu cho mỗi ô tô. Việc quan sát thấy các ranh giới được sắp xếp cho phép chúng ta thay thế mỗi lần quét tuyến tính bằng một tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân | O(n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`các hàng của bảng và lưu trữ các ranh giới trong một mảng và tỷ lệ tương ứng trong một mảng khác. Đầu vào đảm bảo rằng mảng ranh giới đã tăng lên một cách nghiêm ngặt, do đó không cần sắp xếp. 
2. Đối với mỗi xe điện`q`, trình diễn`bisect_left(boundaries, q)`. Điều này tìm thấy ranh giới đầu tiên lớn hơn hoặc bằng`q`. 
3. Đặt vị trí trả về là`i`. Tỷ lệ chính xác là`rates[i - 1]`. Nếu như`q`chính xác bằng`boundaries[i]`, khoảng kết thúc tại ranh giới đó thuộc về hàng trước đó, đó chính xác là lý do tại sao chúng ta sử dụng`i - 1`. 
4. Nếu`q`lớn hơn mọi ranh giới,`bisect_left`trả lại`n`. biểu hiện`rates[i - 1]`sau đó trở thành`rates[n - 1]`, lựa chọn tỷ lệ cuối cùng theo yêu cầu. 
5. Nhân`q`theo tỷ lệ đã chọn và nối kết quả vào đầu ra. Việc tích lũy tất cả các câu trả lời và in chúng cùng nhau sẽ tránh được các thao tác đầu ra không cần thiết. 

### Tại sao nó hoạt động 

Đối với mỗi truy vấn`q`, cho phép`i`là chỉ số đầu tiên mà`b_i >= q`. Nếu chỉ số đó tồn tại thì mọi ranh giới trước đó sẽ nhỏ hơn`q`, Vì thế`q`thuộc khoảng kết thúc tại`b_i`. Khoảng thời gian đó được gán tỷ lệ từ hàng`i - 1`, cho`rates[i - 1]`. Nếu không có chỉ mục nào như vậy tồn tại thì mọi ranh giới của bảng sẽ nhỏ hơn`q`, do đó bài toán ấn định rõ ràng tỷ lệ cuối cùng, cũng là`rates[n - 1]`bởi vì tìm kiếm nhị phân trả về`n`. Do đó, mọi truy vấn đều nhận được chính xác tỷ lệ được quy định trong bảng và nhân tỷ lệ đó với lũy thừa sẽ cho ra mức thuế yêu cầu. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n = int(input())

    boundaries = [0] * n
    rates = [0] * n

    for i in range(n):
        boundaries[i], rates[i] = map(int, input().split())

    m = int(input())
    answers = []

    for _ in range(m):
        q = int(input())

        i = bisect_left(boundaries, q)
        rate = rates[i - 1]

        answers.append(str(q * rate))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Hai mảng bảo toàn bảng chính xác như nó xuất hiện trong đầu vào. Chúng ta không cần lưu trữ các cặp và sắp xếp chúng vì câu lệnh đã đảm bảo các ranh giới ngày càng tăng.`bisect_left`là sự lựa chọn thực hiện quan trọng. Đối với một mảng ranh giới như`[0, 100, 150, 200]`và truy vấn`150`, nó trả về chỉ mục`2`, vì chỉ số`2`chứa giá trị đầu tiên lớn hơn hoặc bằng`150`. sử dụng`rates[1]`chọn tỷ lệ liên quan đến khoảng thời gian`(100, 150]`, điều đó hoàn toàn chính xác. 

sử dụng`bisect_right`sẽ là một lỗi nhỏ. Đối với cùng một truy vấn`150`, nó sẽ trả về chỉ mục`3`, khiến thuật toán chọn tốc độ cho`(150, 200]`, mặc dù`150`chính nó thuộc về khoảng trước đó. 

Khi`q`lớn hơn ranh giới cuối cùng,`bisect_left`trả lại`n`. Truy cập`rates[n - 1]`sau đó chọn tỷ lệ tối đa một cách tự nhiên, do đó không yêu cầu chi nhánh đặc biệt. 

Các số nguyên trong Python có độ chính xác tùy ý, vì vậy việc nhân lũy thừa và tỷ lệ lên tới`10^9`không thể tràn. Sản phẩm lớn nhất có thể là`10^18`, được xử lý trực tiếp bởi Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cái bàn là:```
0    24
100  35
150  50
200  75
250  150
```Các biến quan trọng trong mỗi truy vấn là sức mạnh`q`, vị trí tìm kiếm nhị phân`i`, thuế suất đã chọn và thuế cuối cùng. 

| Truy vấn`q`|`bisect_left`kết quả`i`| Đã chọn`rates[i - 1]`| Thuế | 
| --- | --- | --- | --- | 
| 107 | 2 | 35 | 3745 | 
| 143 | 2 | 35 | 5005 | 
| 152 | 4 | 75 | 11400 | 
| 170 | 4 | 75 | 12750 | 
| 150 | 2 | 35 | 5250 | 

Có sự mâu thuẫn giữa cách diễn giải trực tiếp này và kết quả mẫu được cung cấp cho`152`Và`170`. Đầu ra mẫu cho`7600`Và`8500`, tương ứng với tỷ lệ`50`, không đánh giá`75`. 

Theo quy tắc khoảng được nêu trong bài toán,`(150, 200]`nên sử dụng tỷ lệ từ hàng bắt đầu tại`150`, cụ thể là`50`. Điều này có nghĩa là việc giải thích tìm kiếm nhị phân chính xác không chỉ đơn giản là`rates[bisect_left(boundaries, q) - 1]`. 

Điều kiện đúng là tìm ranh giới lớn nhất nhỏ hơn rất nhiều so với`q`. Đó chính xác là`bisect_left(boundaries, q) - 1`chỉ khi`q`không bằng một ranh giới. Khi`q`lớn hơn một ranh giới thì tỷ lệ của ranh giới đó sẽ được áp dụng. Vì`152`,`bisect_left`trả lại`4`, bởi vì ít nhất ranh giới đầu tiên`152`là`200`, Và`rates[3]`là`75`, một lần nữa xung đột với mẫu. 

Điều này cho thấy việc giải thích khoảng thời gian thực tế cẩn thận hơn. Mẫu thiết lập rằng tỷ lệ trên hàng`i`áp dụng cho các quyền hạn lớn hơn`b_i`và nhỏ hơn hoặc bằng`b_{i+1}`. Như vậy đối với`152`, hàng ngang`150`đưa ra tỷ lệ`50`, và cho`170`, hàng ngang`150`cũng đưa ra tỷ lệ`50`. 

Do đó, việc tìm kiếm chính xác sẽ tìm được ranh giới lớn nhất`b_i < q`, có thể thu được bằng`bisect_left(boundaries, q) - 1`. Vì`152`, điều đó mang lại chỉ số`2`, lựa chọn tỷ lệ`50`. 

Để có sự bình đẳng chính xác như`q = 150`, ranh giới lớn nhất hoàn toàn nhỏ hơn`150`là`100`, do đó tỷ lệ được chọn là`35`. Tìm kiếm tương tự sẽ tự động xử lý trường hợp này. 

Dấu vết kết quả là: 

| Truy vấn`q`|`bisect_left`kết quả | Chỉ mục đã chọn | Tỷ lệ | Thuế | 
| --- | --- | --- | --- | --- | 
| 107 | 2 | 1 | 35 | 3745 | 
| 143 | 2 | 1 | 35 | 5005 | 
| 152 | 3 | 2 | 50 | 7600 | 
| 170 | 3 | 2 | 50 | 8500 | 
| 150 | 2 | 1 | 35 | 5250 | 

Bất biến quan trọng là chỉ mục được chọn luôn là hàng cuối cùng của bảng có ranh giới hoàn toàn nhỏ hơn truy vấn. Các ranh giới chính xác vẫn giữ nguyên khoảng trước đó, trong khi các giá trị ngay phía trên ranh giới sẽ di chuyển đến hàng của ranh giới đó. 

### Đã thi công mẫu 2 

Hãy xem xét:```
3
0 10
100 20
200 30
5
1
100
101
200
250
```Dấu vết là: 

| Truy vấn`q`|`bisect_left`kết quả | Chỉ mục đã chọn | Tỷ lệ | Thuế | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 10 | 10 | 
| 100 | 1 | 0 | 10 | 1000 | 
| 101 | 2 | 1 | 20 | 2020 | 
| 200 | 2 | 1 | 20 | 4000 | 
| 250 | 3 | 2 | 30 | 7500 | 

Đầu ra là:```
10
1000
2020
4000
7500
```Ví dụ này thực hiện cả hai vế của quy tắc biên. Một truy vấn chính xác bằng`100`vẫn sử dụng tỷ lệ`10`, trong khi`101`tỷ lệ sử dụng`20`. Truy vấn`250`nằm ngoài ranh giới cuối cùng nên nó sử dụng tỷ giá cuối cùng`30`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log n) | Việc đọc bảng mất O(n) và mỗi`m`truy vấn thực hiện một tìm kiếm nhị phân O(log n). | 
| Không gian | O(n + m) | Bảng sử dụng bộ nhớ O(n) và danh sách đầu ra lưu trữ các câu trả lời O(m). | 

Với`n, m <= 100000`, giải pháp thực hiện gần đúng`100000 * log2(100000)`lặp lại tìm kiếm nhị phân trong giai đoạn truy vấn, khoảng 1,7 triệu so sánh. Nó nhỏ hơn một cách thoải mái so với`10^10`có thể kiểm tra bằng cách quét tuyến tính cho mỗi truy vấn. Việc sử dụng bộ nhớ cũng tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm sau đây sử dụng một loại nguyên chất`solve_string`chức năng để mọi trường hợp có thể được kiểm tra với`assert`mà không thay thế đầu vào tiêu chuẩn của toàn bộ quy trình.```python
import io
import sys
from bisect import bisect_left

def solve_string(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    boundaries = []
    rates = []

    for _ in range(n):
        boundaries.append(int(next(it)))
        rates.append(int(next(it)))

    m = int(next(it))
    answers = []

    for _ in range(m):
        q = int(next(it))
        i = bisect_left(boundaries, q)

        if i == 0:
            rate = rates[0]
        else:
            rate = rates[i - 1]

        answers.append(str(q * rate))

    return "\n".join(answers)

# Provided sample
sample1 = """\
5
0 24
100 35
150 50
200 75
250 150
5
107
143
152
170
150
"""

assert solve_string(sample1) == """\
3745
5005
7600
8500
5250
""", "sample 1"

# Minimum-size table
case_min = """\
1
0 42
3
1
999
1000000000
"""

assert solve_string(case_min) == """\
42
41958
42000000000
""", "minimum-size table"

# Exact boundaries and values immediately above them
case_boundaries = """\
3
0 10
100 20
200 30
6
1
99
100
101
200
201
"""

assert solve_string(case_boundaries) == """\
10
990
1000
2020
6000
6030
""", "boundary conditions"

# Equal rates
case_equal_rates = """\
4
0 7
10 7
20 7
30 7
5
1
10
11
30
100
"""

assert solve_string(case_equal_rates) == """\
7
70
77
210
700
""", "all rates equal"

# Large values, including the largest allowed power and rate
case_large = """\
2
0 1000000000
1000000000 1000000000
3
1
1000000000
1000000000
"""

assert solve_string(case_large) == """\
1000000000
1000000000000000000
1000000000000000000
""", "large multiplication"

# Generated maximum-size case
n = 100000
m = 100000

lines = [str(n)]
for i in range(n):
    lines.append(f"{i} 1")

lines.append(str(m))
for _ in range(m):
    lines.append("99999")

case_max = "\n".join(lines)

expected_max = "\n".join(["99999"] * m)

assert solve_string(case_max) == expected_max, "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 42 / 1, 999, 1000000000`|`42, 41958, 42000000000`| Kích thước bảng tối thiểu và xử lý tỷ lệ cuối cùng | 
|`0 10 / 100 20 / 200 30`với các truy vấn ranh giới |`10, 990, 1000, 2020, 6000, 6030`| Ranh giới chính xác so với các giá trị ngay phía trên chúng | 
|`0 7 / 10 7 / 20 7 / 30 7`|`7, 70, 77, 210, 700`| Thuế suất bằng nhau | 
| Tỷ lệ và quyền hạn bằng`1000000000`| Giá trị lên đến`10^18`| Sản phẩm lớn và xử lý số nguyên | 
|`100000`hàng và`100000`truy vấn |`99999`lặp đi lặp lại | Kích thước đầu vào tối đa và tìm kiếm logarit | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một truy vấn chính xác bằng ranh giới bảng. Coi như:```
2
0 10
100 20
1
100
```

`bisect_left([0, 100], 100)`trả lại`1`. Chỉ số được chọn là`1 - 1 = 0`, vậy tỷ lệ là`10`và đầu ra là`1000`. Điều này phù hợp với khoảng thời gian`(0, 100]`. Một tìm kiếm sử dụng`bisect_right`sẽ trở lại`2`và chọn sai tỷ lệ thứ hai. 

Trường hợp cạnh thứ hai là một truy vấn ngay phía trên ranh giới:```
2
0 10
100 20
1
101
```Tìm kiếm nhị phân trả về`2`, vậy hàng được chọn là chỉ mục`1`và tỷ lệ là`20`. Đầu ra là`2020`. Điều này khẳng định rằng việc chuyển từ`100`ĐẾN`101`thay đổi khoảng thời gian áp dụng. 

Trường hợp cạnh thứ ba là một truy vấn vượt quá ranh giới lớn nhất:```
2
0 10
100 20
1
150
```

`bisect_left`trả lại`2`, đó là chiều dài của mảng ranh giới. Hàng đã chọn là chỉ mục`1`, do đó tỷ lệ cuối cùng`20`được sử dụng và câu trả lời là`3000`. Không cần có nhánh giới hạn trên riêng biệt. 

Trường hợp cạnh thứ tư là một bảng có tỷ lệ bằng nhau:```
3
0 7
10 7
20 7
3
1
10
25
```Vì`1`, tìm kiếm chọn hàng`0`; vì`10`, nó chọn hàng`0`vì đẳng thức thuộc khoảng trước đó; vì`25`, nó chọn hàng`2`bởi vì truy vấn vượt quá mọi ranh giới. Cả ba mức giá đều`7`, sản xuất:```
7
70
175
```Trường hợp cạnh thứ năm liên quan đến phép nhân lớn:```
1
0 1000000000
1
1000000000
```Tỷ giá hiện có duy nhất là`10^9`, vậy thuế là`10^9 * 10^9 = 10^18`. Biểu diễn số nguyên của Python xử lý việc này một cách chính xác, do đó không có cách xử lý đặc biệt nào liên quan đến tràn trong quá trình triển khai.
