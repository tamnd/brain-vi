---
title: "CF 117A - Thang máy"
description: "Thang máy tuân theo một chu trình hoàn toàn xác định. Nó bắt đầu ở tầng 1 tại thời điểm 0, leo một tầng mỗi giây cho đến khi đến tầng m, sau đó ngay lập tức đổi hướng và quay trở lại tầng 1, lại di chuyển một tầng mỗi giây."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 117
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 88"
rating: 1300
weight: 117
solve_time_s: 151
verified: true
draft: false
---

[CF 117A - Thang máy](https://codeforces.com/problemset/problem/117/A) 

**Đánh giá:** 1300 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thang máy tuân theo một chu trình hoàn toàn xác định. 

Nó bắt đầu ở tầng`1`vào thời điểm đó`0`, leo một tầng mỗi giây cho đến khi chạm tới tầng`m`, sau đó lập tức đổi hướng và đi xuống sàn`1`, lại di chuyển một tầng mỗi giây. Sau khi tới tầng`1`, mô hình tương tự lặp lại mãi mãi. 

Đối với mỗi người tham gia, chúng tôi biết ba điều. Chúng tôi biết tầng nơi họ bắt đầu, tầng họ muốn tiếp cận và thời điểm họ đến tầng xuất phát. Người tham gia sẽ vào ngay lập tức nếu thang máy hiện đang dừng ở tầng đó. Nhiệm vụ là tính toán thời gian đến tầng đích sớm nhất có thể. 

Chi tiết quan trọng là hướng thang máy rất quan trọng. Một người đi lên không thể đơn giản bước vào thang máy bất cứ khi nào nó tới tầng của họ. Họ phải lên tàu khi đi lên. Điều tương tự cũng áp dụng cho chuyển động đi xuống. 

Các ràng buộc loại trừ hoàn toàn việc mô phỏng. Có thể có tới`10^5`truy vấn và cả số tầng và thời gian có thể lớn bằng`10^8`. Mô phỏng từng giây sẽ yêu cầu tới hàng tỷ thao tác ngay cả đối với một truy vấn. Chúng tôi cần một`O(1)`công thức cho mỗi người tham gia. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét ai đó đã đứng ở tầng đích. 

đầu vào:```
1 10
5 5 100
```Đầu ra:```
100
```Thang máy không liên quan ở đây. Việc triển khai bất cẩn vẫn có thể tính toán thời gian chờ đợi và chuyển động. 

Lại một trường hợp lắt léo khác xuất hiện khi thang máy lên tầng đúng lúc`t`. 

đầu vào:```
1 4
1 2 0
```Đầu ra:```
1
```Vào thời điểm`0`, thang máy đã lên tầng rồi`1`, nên người tham gia vào ngay. Nếu chúng tôi vô tình yêu cầu đến muộn hơn một cách nghiêm ngặt, chúng tôi sẽ đợi chu kỳ tiếp theo một cách không chính xác. 

Hướng không khớp cũng gây ra lỗi. 

đầu vào:```
1 4
2 4 3
```Đầu ra:```
9
```Vào thời điểm`3`, thang máy thực sự ở tầng`2`, nhưng nó đang di chuyển xuống dưới vì dãy số là:`1 -> 2 -> 3 -> 4 -> 3 -> 2 -> 1`. 

Người tham gia muốn đi lên nên lên máy bay đúng giờ`3`là vô ích. Họ phải đợi đến lần truy cập tiếp theo vào thời điểm đó`7`. 

Tầng đầu tiên và tầng cuối cùng hoạt động khác nhau vì thang máy đảo ngược hướng ở đó. 

đầu vào:```
1 5
5 1 4
```Đầu ra:```
8
```Vào thời điểm`4`, thang máy ở đúng tầng`5`và ngay lập tức bắt đầu di chuyển xuống dưới, vì vậy người tham gia có thể lên tàu ngay lập tức. Việc xử lý các bước ngoặt không đúng cách thường dẫn đến những sai lầm riêng lẻ. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ mô phỏng chuyển động của thang máy theo thời gian theo đúng nghĩa đen. Trong mỗi giây, chúng tôi sẽ theo dõi tầng và hướng thang máy, đồng thời với mỗi người tham gia, chúng tôi sẽ đợi cho đến khi thang máy đến đúng tầng trong khi di chuyển theo hướng cần thiết. 

Điều này hoạt động hợp lý vì đường dẫn thang máy là xác định. Cuối cùng, mọi thời điểm lên máy bay hợp lệ đều xuất hiện trong quá trình mô phỏng. 

Vấn đề là quy mô. Độ dài chu trình thang máy là`2 * (m - 1)`, có thể đạt gần như`2 * 10^8`. Việc mô phỏng thậm chí một chu kỳ đầy đủ cho mọi người tham gia là quá chậm. Với`10^5`người tham gia, số lượng hoạt động trở nên hoàn toàn không khả thi. 

Nhận xét quan trọng là chuyển động của thang máy là tuần hoàn. 

Đối với một tầng`x`: 

Nếu thang máy đi lên thì tới tầng`x`đôi khi:```
x - 1 + k * cycle
```Nếu thang máy đi xuống thì tới tầng`x`đôi khi:```
(m - x) + (m - 1) + k * cycle
```Ở đâu:```
cycle = 2 * (m - 1)
```Khi chúng tôi biết hướng cần thiết, chúng tôi có thể tính trực tiếp thời gian lên máy bay hợp lệ đầu tiên lớn hơn hoặc bằng`t`. 

Giả sử người tham gia muốn đi lên. Sau đó, chúng ta cần lần đầu tiên:```
x - 1 + k * cycle >= t
```Chúng ta có thể tính toán`k`với số học đơn giản thay vì mô phỏng. 

Sau khi lên máy bay, thời gian di chuyển chỉ là:```
abs(f - s)
```Toàn bộ vấn đề trở thành số học theo thời gian không đổi cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính độ dài chu trình thang máy:```
cycle = 2 * (m - 1)
```Sau nhiều giây, thang máy quay trở lại tầng`1`trong cùng một trạng thái. 
2. Đối với mỗi người tham gia, trước tiên hãy kiểm tra xem`s == f`. 

Nếu người tham gia đã bắt đầu ở điểm đến thì câu trả lời chỉ đơn giản là`t`. 
3. Xác định hướng chuyển động cần thiết. 

Nếu như`f > s`, người tham gia phải lên máy bay khi đi lên. 

Nếu không, họ phải lên tàu khi đi xuống. 
4. Tính thời điểm thang máy lên tầng đầu tiên`s`theo hướng cần thiết. 

Đối với chuyển động đi lên, thời gian truy cập cơ sở trong một chu kỳ là:```
base = s - 1
```Đối với chuyển động đi xuống:```
base = (m - 1) + (m - s)
```5. Tìm số nguyên không âm nhỏ nhất`k`như vậy:```
base + k * cycle >= t
```Đây là sự phân chia trần:```
k = max(0, (t - base + cycle - 1) // cycle)
```6. Thời gian lên máy bay trở thành:```
board = base + k * cycle
```7. Cộng quãng đường di chuyển:```
answer = board + abs(f - s)
```### Tại sao nó hoạt động 

Quỹ đạo thang máy lặp lại hoàn hảo mỗi`2 * (m - 1)`giây. Đối với mỗi cặp tầng và hướng, có chính xác một lần truy cập trong mỗi chu kỳ. Thuật toán tính toán thời gian truy cập chuẩn đó và chuyển trực tiếp đến chu kỳ đầu tiên mà thang máy đến không sớm hơn`t`. 

Vì thang máy di chuyển một tầng trong một giây mà không bị gián đoạn nên thời gian từ lúc`s`ĐẾN`f`sau khi lên máy bay luôn chính xác`abs(f - s)`. Vì chúng tôi luôn chọn thời điểm lên máy bay hợp lệ sớm nhất nên thời gian đến nơi là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    cycle = 2 * (m - 1)

    ans = []

    for _ in range(n):
        s, f, t = map(int, input().split())

        if s == f:
            ans.append(str(t))
            continue

        if f > s:
            base = s - 1
        else:
            base = (m - 1) + (m - s)

        if t <= base:
            board = base
        else:
            k = (t - base + cycle - 1) // cycle
            board = base + k * cycle

        arrival = board + abs(f - s)
        ans.append(str(arrival))

    print("\n".join(ans))

solve()
```Phần đầu tiên tính toán độ dài chu kỳ. Đường thang máy từ`1`lên đến`m`và quay lại`1`mất chính xác`2 * (m - 1)`giây. 

Đối với mỗi truy vấn, trường hợp đặc biệt`s == f`được xử lý ngay lập tức. Việc quên trường hợp này thường tạo ra các câu trả lời sai vì logic chung khi lên máy bay cho rằng người tham gia thực sự phải đi thang máy. 

Biến`base`đại diện cho thời điểm đầu tiên trong một chu kỳ khi thang máy ghé thăm tầng`s`theo hướng cần thiết. 

Đối với hành trình đi lên:```
base = s - 1
```vì thang máy bắt đầu ở tầng`1`vào thời điểm đó`0`và leo lên một tầng mỗi giây. 

Đối với hành trình đi xuống:```
base = (m - 1) + (m - s)
```Thang máy đầu tiên dành`m - 1`vài giây lên đến tầng trên cùng, rồi một giây nữa`m - s`giây giảm dần xuống sàn`s`. 

Bước chia trần là thủ thuật số học cốt lõi:```
(t - base + cycle - 1) // cycle
```Điều này tính toán số chu kỳ hoàn chỉnh nhỏ nhất cần thiết để thời gian truy cập ít nhất là`t`. 

Việc thực hiện xử lý cẩn thận`t <= base`riêng. Nếu không có nhánh đó, các giá trị âm có thể xuất hiện trong biểu thức chia. 

Số nguyên Python dễ dàng xử lý các giá trị lớn nhất ở đây, vì câu trả lời vẫn ở mức thấp hơn nhiều`10^9`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
2 4 3
1 2 0
4 2 0
2 2 5
```Độ dài chu kỳ:```
2 * (4 - 1) = 6
```| s | f | t | Hướng | căn cứ | bảng | đến | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 4 | 3 | lên | 1 | 7 | 9 | 
| 1 | 2 | 0 | lên | 0 | 0 | 1 | 
| 4 | 2 | 0 | xuống | 3 | 3 | 5 | 
| 2 | 2 | 5 | không | - | - | 5 | 

Truy vấn đầu tiên cho thấy tại sao phương hướng lại quan trọng. Thang máy đến tầng`2`vào thời điểm đó`1`trong khi di chuyển lên trên và lặp đi lặp lại`5`trong khi di chuyển xuống dưới. Vì người tham gia đến đúng lúc`3`, cả hai lượt truy cập đều không thể sử dụng được. Chuyến thăm tiếp theo sẽ diễn ra vào thời điểm`7`. 

### Ví dụ 2 

đầu vào:```
3 5
5 1 4
3 4 20
2 1 1
```Độ dài chu kỳ:```
2 * (5 - 1) = 8
```| s | f | t | Hướng | căn cứ | bảng | đến | 
| --- | --- | --- | --- | --- | --- | --- | 
| 5 | 1 | 4 | xuống | 4 | 4 | 8 | 
| 3 | 4 | 20 | lên | 2 | 26 | 27 | 
| 2 | 1 | 1 | xuống | 7 | 7 | 8 | 

Truy vấn thứ hai cho thấy cách hoạt động của bước nhảy chu kỳ. Trở lên thăm tầng`3`đôi khi xảy ra:```
2, 10, 18, 26, ...
```Vì người tham gia xuất hiện vào thời điểm`20`, thời gian lên máy bay hợp lệ đầu tiên là`26`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi người tham gia được xử lý bằng số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ bên cạnh đầu ra | 

Với`10^5`người tham gia, thời gian tuyến tính dễ dàng đủ nhanh. Giải pháp chỉ thực hiện một số phép tính số học cho mỗi truy vấn và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    cycle = 2 * (m - 1)

    ans = []

    for _ in range(n):
        s, f, t = map(int, input().split())

        if s == f:
            ans.append(str(t))
            continue

        if f > s:
            base = s - 1
        else:
            base = (m - 1) + (m - s)

        if t <= base:
            board = base
        else:
            k = (t - base + cycle - 1) // cycle
            board = base + k * cycle

        ans.append(str(board + abs(f - s)))

    print("\n".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup

    return out.getvalue()

# provided sample
assert run(
"""7 4
2 4 3
1 2 0
2 2 0
1 2 1
4 3 5
1 2 2
4 2 0
"""
) == """9
1
0
7
10
7
5
"""

# minimum size
assert run(
"""1 2
1 2 0
"""
) == """1
"""

# already at destination
assert run(
"""1 10
5 5 100
"""
) == """100
"""

# exact turning point at top floor
assert run(
"""1 5
5 1 4
"""
) == """8
"""

# waits multiple cycles
assert run(
"""1 5
3 4 20
"""
) == """27
"""

# catches direction mistake
assert run(
"""1 4
2 4 3
"""
) == """9
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 / 1 2 0`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`5 5 100`|`100`| Tầng xuất phát bằng đích đến | 
|`5 1 4`|`8`| Xử lý đúng lúc bước ngoặt | 
|`3 4 20`|`27`| Logic chờ nhiều chu kỳ | 
|`2 4 3`|`9`| Vấn đề hướng thang máy | 

## Vỏ cạnh 

Hãy xem xét trường hợp người tham gia đã bắt đầu ở tầng mục tiêu. 

đầu vào:```
1 10
5 5 100
```Thuật toán phát hiện ngay`s == f`và trả về`100`. Không có logic lên máy bay nào chạy cả. Điều này tránh việc vô tình buộc người tham gia phải đi thang máy một cách không cần thiết. 

Bây giờ hãy xem xét chính xác thời gian thang máy đến. 

đầu vào:```
1 4
1 2 0
```Đối với chuyển động đi lên:```
base = 1 - 1 = 0
```Từ:```
t = 0
```bảng thuật toán ngay lập tức:```
board = 0
arrival = 0 + 1 = 1
```Trường hợp bình đẳng là quan trọng. Việc sử dụng so sánh nghiêm ngặt sẽ làm trì hoãn việc lên máy bay một chu kỳ một cách không chính xác. 

Hướng không khớp là một tình huống tế nhị khác. 

đầu vào:```
1 4
2 4 3
```Độ dài chu kỳ là:```
6
```Trở lên thăm tầng`2`xảy ra tại:```
1, 7, 13, ...
```Các lượt truy cập đi xuống xảy ra tại:```
5, 11, ...
```Người tham gia đến đúng lúc`3`, vì vậy cả chuyến thăm lân cận đều không hoạt động. Thuật toán chọn chính xác lượt truy cập tiếp theo vào thời điểm đó`7`, sản xuất:```
7 + 2 = 9
```Cuối cùng, hãy xem xét bước ngoặt ở tầng trên cùng. 

đầu vào:```
1 5
5 1 4
```Đối với chuyển động đi xuống:```
base = (5 - 1) + (5 - 5) = 4
```Từ`t == base`, việc lên máy bay diễn ra ngay lập tức ở tầng trên cùng trong giai đoạn đi xuống. Thời gian đi lại là`4`, do đó thời gian đến trở thành`8`. 

Điều này xác nhận hành vi đảo ngược được xử lý chính xác mà không có bất kỳ lỗi nào xảy ra.
