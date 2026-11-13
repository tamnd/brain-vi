---
title: "CF 1690C - Khôi phục thời lượng của tác vụ"
description: "Chúng tôi được cung cấp hai mảng tăng dần. Mảng s chứa thời gian đến của mỗi tác vụ. Nhiệm vụ i sẽ khả dụng vào thời điểm s[i]. Mảng f chứa thời gian hoàn thành của mỗi nhiệm vụ. Nhiệm vụ i kết thúc đúng lúc f[i]. Polycarp xử lý các tác vụ theo thứ tự FIFO."
date: "2026-06-09T23:20:25+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1690
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 797 (Div. 3)"
rating: 800
weight: 1690
solve_time_s: 352
verified: false
draft: false
---

[CF 1690C - Khôi phục thời lượng của nhiệm vụ](https://codeforces.com/problemset/problem/1690/C) 

**Đánh giá:** 800 
**Tas:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 5 phút 52 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng tăng dần. 

Mảng`s`chứa thời gian đến của mỗi nhiệm vụ. Nhiệm vụ`i`trở nên có sẵn vào thời điểm đó`s[i]`. 

Mảng`f`chứa thời gian hoàn thành của mỗi nhiệm vụ. Nhiệm vụ`i`kết thúc đúng lúc`f[i]`. 

Polycarp xử lý các tác vụ theo thứ tự FIFO. Nếu anh ta bận khi có nhiệm vụ đến thì nhiệm vụ đó sẽ đợi trong hàng đợi. Nếu anh ta hoàn thành một nhiệm vụ và hàng đợi không trống, anh ta sẽ ngay lập tức bắt đầu nhiệm vụ chờ tiếp theo. Nếu hàng đợi trống, anh ta đợi cho đến khi có nhiệm vụ khác đến. 

Các giá trị chưa biết là thời lượng thực hiện thực tế`d[i]`của các nhiệm vụ. Chúng ta phải xây dựng lại mọi khoảng thời gian. 

Tổng số nhiệm vụ trên tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Điều này ngay lập tức gợi ý rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Bất cứ điều gì bậc hai sẽ yêu cầu đại khái`4 · 10^10`trong trường hợp xấu nhất vượt xa thời hạn. 

Phần khó khăn là một nhiệm vụ không nhất thiết phải bắt đầu khi nó đến. Nếu các tác vụ trước đó vẫn đang được xử lý thì nó phải đợi trong hàng đợi. Một triển khai ngây thơ giả định`duration = finish - arrival`thường sẽ sai. 

Coi như:```
s = [0, 3]
f = [10, 12]
```Nhiệm vụ thứ hai đến vào lúc`3`, nhưng nhiệm vụ đầu tiên chỉ kết thúc vào thời điểm`10`. Nhiệm vụ thứ hai thực sự bắt đầu vào thời điểm`10`, vậy thời gian của nó là```
12 - 10 = 2
```không```
12 - 3 = 9
```Một lỗi dễ mắc phải khác là quên rằng bộ xử lý có thể không hoạt động. 

Ví dụ:```
s = [0, 10]
f = [2, 15]
```Nhiệm vụ đầu tiên kết thúc vào thời điểm`2`. Chẳng có gì chờ đợi nên Polycarp ngồi im cho đến giờ`10`. Nhiệm vụ thứ hai bắt đầu ngay tại thời điểm nó đến và có thời hạn```
15 - 10 = 5
```Sử dụng thời gian kết thúc trước đó một cách vô điều kiện sẽ tạo ra sai số```
15 - 2 = 13
```Giải pháp phải phân biệt chính xác giữa hai tình huống này. 

## Phương pháp tiếp cận 

Có thể mô phỏng trực tiếp. 

Đối với mỗi nhiệm vụ, chúng ta có thể xác định rõ ràng thời điểm nó bắt đầu. Thời gian bắt đầu là thời điểm đến hoặc thời điểm nhiệm vụ trước đó kết thúc, tùy theo thời điểm nào đến sau. Khi đã biết thời gian bắt đầu, thời lượng chỉ đơn giản là thời gian hoàn thành trừ đi thời gian bắt đầu. 

Mô phỏng này đã hiệu quả vì các nhiệm vụ được xử lý theo thứ tự nghiêm ngặt. Chúng tôi không bao giờ cần cấu trúc hàng đợi thực tế hoặc bất kỳ hoạt động tìm kiếm nào. Thông tin duy nhất cần thiết cho nhiệm vụ`i`là khi nhiệm vụ`i - 1`hoàn thành. 

Quan sát quan trọng là thời gian bắt đầu nhiệm vụ`i`được xác định hoàn toàn bởi hai giá trị:```
start[i] = max(s[i], f[i-1])
```vì`i > 0`. 

Nếu nhiệm vụ trước đó kết thúc trước khi nhiệm vụ mới đến, Polycarp sẽ đợi và bắt đầu lúc`s[i]`. 

Nếu tác vụ trước kết thúc sau khi tác vụ mới đến thì tác vụ đó đã chờ trong hàng đợi và bắt đầu ngay lập tức lúc`f[i-1]`. 

Khi đã biết thời điểm bắt đầu,```
d[i] = f[i] - start[i]
```Điều này cho phép quét tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force với tính năng theo dõi hàng đợi rõ ràng | O(n) | O(n) | Đã chấp nhận | 
| Tái thiết trực tiếp tối ưu | O(n) | O(1) thêm | Đã chấp nhận | 

Mặc dù cả hai đều tuyến tính, cách tiếp cận thứ hai rõ ràng hơn vì nó lấy trực tiếp thời gian bắt đầu thay vì duy trì trạng thái hàng đợi. 

## Hướng dẫn thuật toán 

1. Đọc`n`, mảng đến`s`và mảng hoàn thành`f`. 
2. Đối với nhiệm vụ đầu tiên, nó sẽ bắt đầu ngay khi nó đến.```
start = s[0]
d[0] = f[0] - s[0]
```3. Đối với mọi nhiệm vụ`i > 0`, tính thời gian bắt đầu thực tế của nó.```
start = max(s[i], f[i-1])
```Nếu bộ xử lý không hoạt động trước tác vụ`i`đã đến, nhiệm vụ bắt đầu lúc`s[i]`. Nếu không thì nó đang đợi trong hàng đợi và bắt đầu vào thời điểm hoàn thành trước đó. 
4. Tính thời lượng.```
d[i] = f[i] - start
```5. Xuất tất cả thời lượng. 

### Tại sao nó hoạt động 

Thời gian hoàn thành ngày càng tăng lên và các nhiệm vụ được hoàn thành theo đúng thứ tự. Vì lẽ đó nhiệm vụ`i`không thể bắt đầu trước nhiệm vụ`i - 1`kết thúc. 

Chỉ có hai khả năng. 

Nếu nhiệm vụ`i`đến sau nhiệm vụ`i - 1`đã hoàn tất, Polycarp ở trạng thái rảnh và bắt đầu ngay lập tức lúc`s[i]`. 

Nếu nhiệm vụ`i`đến trước nhiệm vụ`i - 1`kết thúc, nó sẽ đợi trong hàng đợi và bắt đầu chính xác khi tác vụ`i - 1`hoàn thành, cụ thể là tại`f[i-1]`. 

Vì vậy thời điểm bắt đầu luôn là```
max(s[i], f[i-1])
```và thời lượng là thời gian hoàn thành trừ đi thời gian bắt đầu này. Vì thời gian bắt đầu của mọi nhiệm vụ đều được xây dựng lại một cách chính xác nên mọi khoảng thời gian đều được xây dựng lại một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    s = list(map(int, input().split()))
    f = list(map(int, input().split()))

    ans = [0] * n

    ans[0] = f[0] - s[0]

    for i in range(1, n):
        start = max(s[i], f[i - 1])
        ans[i] = f[i] - start

    print(*ans)
```Nhiệm vụ đầu tiên được xử lý riêng vì không có thời gian hoàn thành trước đó. 

Đối với mọi nhiệm vụ sau này, điểm tinh tế duy nhất là tính toán chính xác thời gian bắt đầu. sử dụng`max(s[i], f[i - 1])`nắm bắt cả hai tình huống có thể xảy ra: xếp hàng đợi hoặc bắt đầu ngay sau khi đến. 

Tất cả các giá trị vừa vặn thoải mái bên trong số nguyên Python. Dấu thời gian lớn nhất chỉ`10^9`, vì vậy tràn không phải là một mối quan tâm. 

Việc triển khai chỉ lưu trữ mảng câu trả lời và thực hiện một lần chuyển qua các tác vụ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = [0, 3, 7]
f = [2, 10, 11]
```| tôi | s[i] | f[i-1] | bắt đầu | f[i] | thời lượng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | - | 0 | 2 | 2 | 
| 1 | 3 | 2 | 3 | 10 | 7 | 
| 2 | 7 | 10 | 10 | 11 | 1 | 

Đầu ra:```
2 7 1
```Nhiệm vụ thứ ba đến vào lúc`7`nhưng không thể bắt đầu cho đến khi nhiệm vụ thứ hai kết thúc đúng lúc`10`. Điều này chứng tỏ tại sao chỉ riêng thời gian đến là không đủ. 

### Ví dụ 2 

đầu vào:```
s = [10, 15]
f = [11, 16]
```| tôi | s[i] | f[i-1] | bắt đầu | f[i] | thời lượng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 10 | - | 10 | 11 | 1 | 
| 1 | 15 | 11 | 15 | 16 | 1 | 

Đầu ra:```
1 1
```Sau khi hoàn thành nhiệm vụ đầu tiên vào thời điểm`11`, Polycarp đợi đến lúc`15`. Nhiệm vụ thứ hai bắt đầu ngay khi nó đến. 

Ví dụ này thực hiện trường hợp bộ xử lý nhàn rỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua các nhiệm vụ | 
| Không gian | O(n) | Mảng đầu ra có kích thước`n`| 

Tổng của tất cả`n`giá trị trong các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, do đó thuật toán chỉ thực hiện vài trăm nghìn thao tác. Điều này là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = list(map(int, input().split()))
        f = list(map(int, input().split()))

        ans = [0] * n
        ans[0] = f[0] - s[0]

        for i in range(1, n):
            ans[i] = f[i] - max(s[i], f[i - 1])

        out.append(" ".join(map(str, ans)))

    return "\n".join(out)

# provided sample
assert run(
"""4
3
0 3 7
2 10 11
2
10 15
11 16
9
12 16 90 195 1456 1569 3001 5237 19275
13 199 200 260 9100 10000 10914 91066 5735533
1
0
1000000000
"""
) == (
"""2 7 1
1 1
1 183 1 60 7644 900 914 80152 5644467
1000000000"""
)

# minimum size
assert run(
"""1
1
5
8
"""
) == "3"

# waiting in queue
assert run(
"""1
2
0 3
10 12
"""
) == "10 2"

# idle processor
assert run(
"""1
2
0 10
2 15
"""
) == "2 5"

# boundary transition
assert run(
"""1
3
0 5 6
5 8 20
"""
) == "5 3 12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`|`3`| Trường hợp ranh giới một nhiệm vụ | 
|`s=[0,3], f=[10,12]`|`10 2`| Nhiệm vụ đang chờ trong hàng đợi | 
|`s=[0,10], f=[2,15]`|`2 5`| Bộ xử lý không hoạt động | 
|`s=[0,5,6], f=[5,8,20]`|`5 3 12`| Chuyển đổi chính xác giữa trạng thái nhàn rỗi và hàng đợi | 

## Vỏ cạnh 

### Tác vụ đến trong khi tác vụ trước đó vẫn đang chạy 

đầu vào:```
1
2
0 3
10 12
```Đối với nhiệm vụ thứ hai:```
start = max(3, 10) = 10
duration = 12 - 10 = 2
```Đầu ra:```
10 2
```Một giải pháp sử dụng`f[i] - s[i]`sẽ quay lại không chính xác`10 9`. 

### Bộ xử lý không hoạt động 

đầu vào:```
1
2
0 10
2 15
```Đối với nhiệm vụ thứ hai:```
start = max(10, 2) = 10
duration = 15 - 10 = 5
```Đầu ra:```
2 5
```Thuật toán sẽ bỏ qua thời gian hoàn thành cũ một cách chính xác khi máy không hoạt động. 

### Nhiệm vụ đơn lẻ 

đầu vào:```
1
1
100
150
```Nhiệm vụ đầu tiên bắt đầu ngay lập tức:```
duration = 150 - 100 = 50
```Đầu ra:```
50
```Việc xử lý riêng nhiệm vụ đầu tiên sẽ tránh truy cập vào thời gian hoàn thành trước đó không tồn tại. 

### Đến chính xác sau lần hoàn thành trước đó 

đầu vào:```
1
2
0 5
5 9
```Đối với nhiệm vụ thứ hai:```
start = max(5, 5) = 5
duration = 9 - 5 = 4
```Đầu ra:```
5 4
```Công thức xử lý sự bình đẳng một cách tự nhiên mà không có trường hợp đặc biệt nào.
