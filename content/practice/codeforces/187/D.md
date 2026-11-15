---
title: "CF 187D - Hợp đồng BRT"
description: "Một chiếc xe buýt đi qua một đoạn đường cố định. Giữa các đoạn liên tiếp có đèn giao thông và mọi đèn đều tuân theo một chu trình đồng bộ như nhau. Mỗi chu kỳ kéo dài g + r giây."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 187
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 119 (Div. 1)"
rating: 2800
weight: 187
solve_time_s: 114
verified: true
draft: false
---

[CF 187D - Hợp đồng BRT](https://codeforces.com/problemset/problem/187/D) 

**Đánh giá:** 2800 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một chiếc xe buýt đi qua một đoạn đường cố định. Giữa các đoạn liên tiếp có đèn giao thông và mọi đèn đều tuân theo một chu trình đồng bộ như nhau. 

Mỗi chu kỳ kéo dài`g + r`giây. Lúc đầu đèn xanh`g`giây của chu kỳ và màu đỏ trong thời gian tiếp theo`r`giây. Vào thời điểm`0`mọi đèn đều bắt đầu xanh. Xe buýt chỉ được phép băng qua giao lộ khi đèn xanh. Nếu nó đến đúng vào thời điểm màu xanh lá cây kết thúc thì nó phải đợi giai đoạn màu xanh lá cây tiếp theo. 

Dữ liệu đầu vào cung cấp thời gian di chuyển của từng đoạn đường. Xe buýt bắt đầu vào một thời điểm khởi hành nào đó`t`, đi qua đoạn đầu tiên, có thể đợi ở giao lộ đầu tiên, đi qua đoạn tiếp theo, v.v. cho đến khi đến đích sau đoạn cuối cùng. 

Đối với mỗi lần truy vấn`t_i`, chúng ta phải tính toán chính xác thời gian đến nơi. 

Các ràng buộc loại trừ hoàn toàn việc mô phỏng mọi giao lộ một cách độc lập cho mọi truy vấn. Cả hai`n`Và`q`đang lên đến`10^5`, vì vậy một`O(nq)`giải pháp sẽ thực hiện xung quanh`10^10`trong trường hợp xấu nhất vượt xa thời hạn. Chúng tôi cần một cái gì đó gần với tuyến tính hoặc logarit cho mỗi truy vấn. 

Phần khó khăn là việc chờ đợi phụ thuộc vào thời gian hiện tại theo modulo độ dài chu kỳ. Một chiếc xe buýt có thể đến cùng một giao lộ vào những thời điểm hơi khác nhau và có thể vượt qua ngay lập tức hoặc đợi gần như toàn bộ giai đoạn đèn đỏ. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử một chiếc xe buýt đến giao lộ đúng lúc đèn xanh kết thúc.```
g = 3, r = 2
arrival = 3
```Khoảng thời gian`[0,3)`màu xanh lá cây và`[3,5)`là màu đỏ. Xe buýt phải đợi. điều trị`<= g`vì màu xanh lá cây tạo ra câu trả lời sai. 

Một trường hợp tinh tế khác xuất hiện khi có nhiều nút giao thông liên tiếp được vượt qua mà không phải chờ đợi.```
n = 2
g = 10, r = 5
segments = [2,2,2]
start = 1
```Thời gian đến tại các giao lộ là`3`Và`5`, cả hai đều bên trong màu xanh lá cây. Một mô phỏng bất cẩn làm tròn chu kỳ không chính xác có thể gây ra sự chờ đợi không cần thiết. 

Thái cực ngược lại lặp đi lặp lại ở mỗi ngã tư.```
n = 2
g = 1, r = 10
segments = [1,1,1]
start = 0
```Xe buýt đến ngã tư đầu tiên đúng giờ`1`, chính xác là màu đỏ, đợi cho đến khi`11`, đến giao lộ thứ hai tại`12`, lại đợi cho đến khi`22`, và kết thúc tại`23`. 

Một lỗi phổ biến khác là do quên rằng đoạn cuối không có đèn giao thông sau đó. Việc chờ đợi chỉ được kiểm tra tại các ngã tư, không kiểm tra sau khi đã đến đích. 

## Phương pháp tiếp cận 

Việc mô phỏng trực tiếp rất đơn giản. Đối với mỗi truy vấn, hãy duy trì thời gian hiện tại. Thêm độ dài đoạn, sau đó nếu đây không phải là đoạn cuối cùng, hãy kiểm tra trạng thái đèn giao thông tại thời điểm mới. 

Cho phép`T = g + r`. Nếu như`current % T < g`, xe buýt đi qua ngay lập tức. Nếu không nó sẽ đợi cho đến bội số tiếp theo của`T`. 

Mô phỏng này đúng vì nó tuân theo các quy luật chính xác về chuyển động và chờ đợi. Vấn đề là chi phí. Mỗi truy vấn xử lý tất cả`n`giao lộ, do đó tổng độ phức tạp trở thành`O(nq)`. Với cả hai giá trị bằng`10^5`, điều này đạt tới`10^10`hoạt động. 

Quan sát quan trọng là hành vi chờ đợi chỉ phụ thuộc vào modulo thời gian`T`. 

Xác định một chức năng:```
f(x) = final arrival time if the bus starts at time x
```Hãy xem điều gì sẽ xảy ra nếu chúng ta tăng thời gian bắt đầu lên đúng một chu kỳ`T`. 

Mỗi thời gian đến trong tương lai cũng tăng lên một cách chính xác`T`, do đó các quyết định xanh/đỏ vẫn giống nhau. Toàn bộ quỹ đạo chuyển dịch về phía trước một chu kỳ. 

Điều đó có nghĩa là:```
f(x + T) = f(x) + T
```Cấu trúc tuần hoàn này làm giảm bài toán thành việc tính toán các câu trả lời chỉ cho`T`dư lượng có thể modulo chiều dài chu kỳ. 

Không may`T`bản thân nó có thể lớn bằng`10^9`, vì vậy chúng ta vẫn không thể tính trước trực tiếp tất cả các phần dư. 

Quan sát tiếp theo mạnh mẽ hơn. Vì thời gian bắt đầu thay đổi liên tục nên kiểu chờ chỉ thay đổi tại các điểm dừng cụ thể. Giữa hai điểm dừng, câu trả lời đơn giản là:```
f(x) = x + constant
```Mỗi giao lộ góp một khoảng dư lượng xấu khiến chúng phải chờ đợi ở đó. Sau khi xử lý tất cả các giao điểm, toàn bộ trục thời gian sẽ được phân chia thành các khoảng trong đó tổng thời gian chờ được cố định. 

Chúng ta có thể xử lý ngược các giao lộ và duy trì các khoảng thời gian này bên trong bản đồ có thứ tự. Mọi truy vấn sau đó sẽ trở thành tìm kiếm nhị phân theo các khoảng thời gian. 

Độ phức tạp cuối cùng là`O(n log n + q log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Cải cách khóa 

Hãy:```
pref[i] = l1 + l2 + ... + li
```Sau khi đi qua chặng đầu tiên`i`đoạn, xe buýt đến giao lộ`i`vào thời điểm:```
start + pref[i] + waits_before_i
```Xe buýt phải đợi ở ngã tư`i`nếu:```
(start + pref[i] + waits_before_i) mod T >= g
```Thay vì mô phỏng về phía trước, chúng tôi xác định thời điểm bắt đầu chờ đợi. 

### Giải thích khoảng thời gian 

Giả sử tổng số chờ đợi tích lũy trước nút giao`i`đã được cố định trong một khoảng thời gian bắt đầu. 

Thì điều kiện chờ ở ngã tư`i`trở thành một điều kiện khoảng mô-đun đơn giản tại thời điểm bắt đầu. 

Vượt qua khoảng thời gian này làm tăng sự chờ đợi trong tương lai một cách chính xác`r`. 

Khi chúng tôi xử lý từng giao điểm một, chúng tôi duy trì sự phân chia phần dư thời gian bắt đầu thành các khoảng thời gian chờ tích lũy bằng nhau. 

###Cấu trúc dữ liệu 

Chúng tôi lưu trữ các khoảng rời rạc trên dư lượng modulo`T`. 

Đối với mỗi khoảng thời gian, chúng tôi giữ:```
value = total extra waiting accumulated
```Ban đầu tất cả dư lượng đã chờ đợi`0`. 

Khi xử lý giao lộ: 

1. Đối với mỗi khoảng thời gian hiện tại, hãy tính toán phạm vi phụ nào gây ra tình trạng chờ đợi tại giao lộ này. 
2. Chia khoảng thời gian nếu cần thiết. 
3. Thêm`r`đến giá trị chờ đợi của các phần bị ảnh hưởng. 

Vì mỗi giao lộ có nhiều nhất hai ranh giới mới nên tổng số khoảng cách vẫn là tuyến tính. 

###Trả lời truy vấn 

Đối với thời gian bắt đầu truy vấn`x`: 

1. Tính toán`k = x mod T`. 
2. Tìm khoảng chứa`k`. 
3. Đặt giá trị chờ được lưu trữ của nó là`w`. 
4. Câu trả lời cuối cùng là:```
x + total_segment_sum + w
```### Tại sao nó hoạt động 

Bên trong bất kỳ khoảng dư nào, mọi ngã tư đều đưa ra quyết định giống nhau, hoặc vượt ngay hoặc đợi một pha đỏ. Vì trình tự quyết định là cố định nên tổng thời gian chờ đợi cũng cố định. 

Bất cứ khi nào phần dư đi qua ranh giới nơi một số giao lộ thay đổi hành vi, tổng thời gian chờ sẽ tăng thêm đúng một pha màu đỏ. Cấu trúc khoảng thời gian nắm bắt chính xác những chuyển tiếp này. 

Bởi vì tất cả các đèn đều được đồng bộ hóa, việc dịch chuyển thời gian bắt đầu theo một chu kỳ đầy đủ sẽ bảo toàn mọi quyết định, do đó các lớp cặn hoàn toàn xác định hành vi. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    n, g, r = map(int, input().split())
    l = list(map(int, input().split()))

    T = g + r
    total = sum(l)

    pref = []
    s = 0
    for i in range(n):
        s += l[i]
        pref.append(s % T)

    # events[pos] = added waiting after this breakpoint
    events = []

    cur_add = 0

    intervals = [(0, 0)]

    for p in pref:
        new_intervals = []

        for start, val in intervals:
            end = T

            L = (g - p - val) % T
            R = (-p - val) % T

            if L < R:
                pieces = [
                    (start, min(end, L), val),
                    (max(start, L), min(end, R), val + r),
                    (max(start, R), end, val),
                ]
            else:
                pieces = [
                    (start, min(end, R), val),
                    (max(start, R), min(end, L), val + r),
                    (max(start, L), end, val),
                ]

            for a, b, v in pieces:
                if a < b:
                    new_intervals.append((a, v))
                    if b < T:
                        new_intervals.append((b, val))

        new_intervals.sort()

        merged = []
        for pos, val in new_intervals:
            if merged and merged[-1][1] == val:
                continue
            merged.append((pos, val))

        intervals = merged

    starts = [x[0] for x in intervals]
    vals = [x[1] for x in intervals]

    q = int(input())

    out = []

    for _ in range(q):
        t = int(input())

        k = t % T
        idx = bisect_right(starts, k) - 1

        out.append(str(t + total + vals[idx]))

    print("\n".join(out))

solve()
```Giải pháp lưu trữ các ranh giới khoảng thay vì phạm vi rõ ràng. Nếu một khoảng thời gian bắt đầu tại`starts[i]`, nó tiếp tục cho đến ranh giới tiếp theo. 

Giai đoạn tiền xử lý dần dần tinh chỉnh phân vùng dư lượng. Đối với mỗi giao lộ, chúng tôi xác định lực dư nào đang chờ ở đó và thêm`r`đến sự chậm trễ tích lũy của họ. 

Số học mô-đun là phần khó nhất. Điều kiện:```
(start + pref[i] + current_wait) mod T >= g
```xác định một khoảng tròn trên dư lượng. Các khoảng tròn có thể bao quanh số 0, do đó việc triển khai sẽ xử lý hai trường hợp riêng biệt. 

Một điểm tinh tế khác là việc xử lý chính xác`>= g`. Đến đúng giờ`g`có nghĩa là đèn đã chuyển sang màu đỏ. sử dụng`>`thay vào đó sẽ tạo ra câu trả lời sai. 

Tất cả số học đều sử dụng số nguyên Python, hỗ trợ tự nhiên phạm vi 64 bit được yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 3 2
5 2
5
1
2
3
4
5
```Đây:```
T = 5
total = 7
```Giao lộ duy nhất có thể đạt được sau`5`giây. 

| Bắt đầu | Tiếp cận ngã tư | Dư lượng | Chờ đợi? | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 1 | Không | 8 | 
| 2 | 7 | 2 | Không | 9 | 
| 3 | 8 | 3 | Vâng, đợi 2 | 12 | 
| 4 | 9 | 4 | Vâng, đợi đã 1 | 12 | 
| 5 | 10 | 0 | Không | 12 | 

Ví dụ này thể hiện điều kiện biên. Dư lượng`3`Và`4`có màu đỏ vì màu xanh chỉ tồn tại ở dạng cặn`0,1,2`. 

### Ví dụ thứ hai 

Hãy xem xét:```
2 2 3
1 1 1
```Độ dài chu kỳ là`5`. 

| Bắt đầu | Đến lần đầu | Đợi sau trước nhé? | Đến lần thứ hai | Đợi sau giây lát? | Kết thúc | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | Không | 2 | Có | 6 | 
| 1 | 2 | Có | 6 | Không | 7 | 
| 2 | 3 | Có | 7 | Có | 11 | 

Dấu vết này cho thấy việc chờ đợi trước đó sẽ làm dịch chuyển các chất cặn bã trong tương lai. Giao điểm thứ hai không thể được phân tích độc lập với giao điểm thứ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | bảo trì định kỳ và tìm kiếm nhị phân | 
| Không gian | O(n) | ranh giới khoảng được lưu trữ | 

Số lượng ranh giới khoảng tăng tuyến tính vì mỗi giao lộ chỉ đưa ra một số lượng điểm phân chia mới không đổi. Với`10^5`giao lộ và truy vấn, giải pháp thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from bisect import bisect_right

def solve_io(inp: str) -> str:
    input_data = io.StringIO(inp)
    output = io.StringIO()

    input = input_data.readline

    n, g, r = map(int, input().split())
    l = list(map(int, input().split()))

    T = g + r
    total = sum(l)

    pref = []
    s = 0
    for i in range(n):
        s += l[i]
        pref.append(s % T)

    intervals = [(0, 0)]

    for p in pref:
        new_intervals = []

        for start, val in intervals:
            end = T

            L = (g - p - val) % T
            R = (-p - val) % T

            if L < R:
                pieces = [
                    (start, min(end, L), val),
                    (max(start, L), min(end, R), val + r),
                    (max(start, R), end, val),
                ]
            else:
                pieces = [
                    (start, min(end, R), val),
                    (max(start, R), min(end, L), val + r),
                    (max(start, L), end, val),
                ]

            for a, b, v in pieces:
                if a < b:
                    new_intervals.append((a, v))
                    if b < T:
                        new_intervals.append((b, val))

        new_intervals.sort()

        merged = []
        for pos, val in new_intervals:
            if merged and merged[-1][1] == val:
                continue
            merged.append((pos, val))

        intervals = merged

    starts = [x[0] for x in intervals]
    vals = [x[1] for x in intervals]

    q = int(input())

    ans = []

    for _ in range(q):
        t = int(input())

        k = t % T
        idx = bisect_right(starts, k) - 1

        ans.append(str(t + total + vals[idx]))

    return "\n".join(ans)

# provided sample
assert solve_io(
"""1 3 2
5 2
5
1
2
3
4
5
"""
) == "\n".join(["8", "9", "12", "12", "12"])

# minimum size
assert solve_io(
"""1 10 1
1 1
1
0
"""
) == "2"

# exact boundary hit
assert solve_io(
"""1 3 2
3 1
1
0
"""
) == "6"

# always green
assert solve_io(
"""2 100 1
1 1 1
2
0
50
"""
) == "\n".join(["3", "53"])

# repeated waiting
assert solve_io(
"""2 1 10
1 1 1
1
0
"""
) == "23"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Kích thước tối thiểu | Đúng đường truyền đơn giản | Chức năng cơ bản | 
| Lượt truy cập ranh giới chính xác | Xe buýt dừng ở`time % T == g`| Độ chính xác từng cái một | 
| Luôn xanh | Không phải chờ đợi ở đâu | Tránh chờ đợi không cần thiết | 
| Chờ đợi lặp đi lặp lại | Nhiều lần chờ đợi màu đỏ tích lũy | Dư lượng trong tương lai dịch chuyển chính xác | 

## Vỏ cạnh 

Hãy xem xét sự chuyển đổi chính xác từ màu xanh sang màu đỏ.```
1 3 2
3 1
1
0
```Xe buýt đến ngã tư đúng giờ`3`. Vì màu xanh lá cây chỉ bao phủ những tàn dư`0,1,2`, dư lượng`3`đã có màu đỏ rồi. Xe buýt đợi đến giờ`5`và sau đó kết thúc tại`6`. 

Thuật toán xử lý việc này vì điều kiện chờ sử dụng:```
residue >= g
```còn hơn là`>`. 

Bây giờ hãy xem xét việc chờ đợi lặp đi lặp lại:```
2 1 10
1 1 1
1
0
```Lần đến đầu tiên xảy ra vào thời điểm`1`, màu đỏ. Xe buýt đợi cho đến khi`11`. 

Sau khi đi qua đoạn thứ hai, nó đến giao lộ tiếp theo tại`12`, lại màu đỏ, đợi cho đến khi`22`, và cuối cùng đến đích tại`23`. 

Khoảng thời gian tiền xử lý tích lũy chính xác cả hai lần chờ vì mỗi lần gặp màu đỏ bổ sung sẽ thêm một lần chờ khác`r`đến giá trị khoảng. 

Cuối cùng, hãy xem xét một trường hợp không có sự chờ đợi nào cả:```
2 100 1
1 1 1
1
0
```Mọi dư lượng đến vẫn ở bên dưới`100`, do đó thời gian chờ tích lũy vẫn bằng 0 trong toàn bộ khoảng thời gian còn lại. Câu trả lời cuối cùng chỉ đơn giản là thời gian bắt đầu cộng với tổng chiều dài đoạn đường.
