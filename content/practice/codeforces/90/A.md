---
title: "CF 90A - Đường cáp treo"
description: "Tuyến cáp gửi một cáp treo mỗi phút và các màu sắc lặp lại theo một chu kỳ cố định: đỏ - lục - lam - đỏ - ... Mỗi cáp treo có thể chở tối đa hai học sinh và mỗi chuyến đi mất đúng 30 phút để lên đến đỉnh. Chúng tôi có ba nhóm học sinh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 90
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 74 (Div. 2 Only)"
rating: 1000
weight: 90
solve_time_s: 93
verified: true
draft: false
---

[CF 90A - Đường cáp treo](https://codeforces.com/problemset/problem/90/A) 

**Đánh giá:** 1000 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tuyến cáp gửi một cáp treo mỗi phút và màu sắc lặp lại theo chu kỳ cố định:`red -> green -> blue -> red -> ...`Mỗi cáp treo có thể chở tối đa hai học sinh và mỗi chuyến đi mất đúng 30 phút để lên đến đỉnh. 

Chúng tôi có ba nhóm học sinh. Một nhóm chỉ đi xe màu đỏ, nhóm khác chỉ đi xe màu xanh lá cây và nhóm cuối cùng chỉ đi xe màu xanh lam. Nhiệm vụ là tính thời điểm sớm nhất mà mỗi học sinh đều đạt đến đỉnh. 

Chi tiết mấu chốt là lịch khởi hành được cố định hoàn toàn. Chúng tôi không thể sắp xếp lại màu sắc hoặc tăng tốc bất cứ điều gì. Sự lựa chọn duy nhất là có bao nhiêu học sinh lên mỗi chuyến cáp treo đến. 

Giới hạn rất nhỏ, quy mô mỗi nhóm tối đa là 100. Ngay cả một mô phỏng trực tiếp cũng dễ dàng phù hợp với giới hạn thời gian. Điều đó có nghĩa là thách thức thực sự là nhận ra mô hình cơ bản một cách rõ ràng và tránh những sai sót riêng lẻ. 

Việc thực hiện bất cẩn có thể dễ nhầm lẫn giữa thời gian đi và thời gian đến. Ví dụ: 

đầu vào:```
1 0 0
```Cáp treo màu đỏ đến ngay lập tức`0`, nhưng học sinh chỉ đạt đến đỉnh cao vào thời điểm`30`. 

Đầu ra đúng:```
30
```Một sai lầm dễ mắc nữa là quên rằng mỗi xe chở hai học sinh. Hãy xem xét: 

đầu vào:```
3 0 0
```Xe màu đỏ thỉnh thoảng xuất hiện`0, 3, 6, ...`Lúc đó chiếc xe màu đỏ đầu tiên chở hai học sinh`0`, và chiếc xe màu đỏ thứ hai chở học sinh còn lại vào lúc đó`3`. Sự xuất hiện cuối cùng xảy ra vào lúc`3 + 30 = 33`. 

Đầu ra đúng:```
33
```Một công thức ngây thơ như`r * 3 + 30`sẽ sản xuất không chính xác`39`. 

Ngoài ra còn có một trường hợp khó nhận thấy khi một hoặc hai nhóm trống. Giả sử: 

đầu vào:```
0 0 1
```Xe màu xanh xuất hiện đầu tiên`2`, không phải lúc`0`. Học sinh đến`2 + 30 = 32`. 

Đầu ra đúng:```
32
```Nếu chúng ta quên rằng các màu khác nhau bắt đầu ở các độ lệch khác nhau thì câu trả lời sẽ sai ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng cáp treo từng phút. Vào mỗi phút, chúng tôi xác định màu đến bằng cách sử dụng`time % 3`. Nếu học sinh da màu đó vẫn đang đợi thì có tối đa hai người trong số họ lên xe. Chúng tôi tiếp tục cho đến khi mọi nhóm đều trống và câu trả lời là thời gian hiện tại cộng thêm 30. 

Mô phỏng này là chính xác vì lịch trình mang tính xác định và mọi chiếc xe hữu ích phải luôn được lấp đầy một cách tham lam. Để trống ghế không bao giờ có thể giúp ích được, vì những chiếc xe cùng màu trong tương lai sẽ chỉ đến sau. 

Mặc dù giải pháp vũ phu này hoàn toàn có thể chấp nhận được ở đây, nhưng nó vẫn thực hiện công việc không cần thiết từng phút một. Trong trường hợp xấu nhất, chúng tôi có thể mô phỏng khoảng 150 phút. Điều đó vẫn còn tầm thường, nhưng cấu trúc của bài toán cho phép giải một cách toán học trực tiếp. 

Quan sát quan trọng là mọi màu sắc đều xuất hiện định kỳ cứ sau ba phút: 

Xe màu đỏ khởi hành lúc:```
0, 3, 6, 9, ...
```Xe xanh khởi hành lúc:```
1, 4, 7, 10, ...
```Xe xanh xuất phát lúc:```
2, 5, 8, 11, ...
```Mỗi xe chở hai học sinh nên một nhóm cỡ`x`nhu cầu:```
ceil(x / 2)
```những chiếc xe có màu sắc của nó. 

Nếu một màu sắc cần`k`xe ô tô, thời gian khởi hành cuối cùng của màu đó là:```
offset + (k - 1) * 3
```trong đó phần bù là:```
red   -> 0
green -> 1
blue  -> 2
```Sau khi khởi hành, chuyến đi vẫn mất 30 phút. Câu trả lời đơn giản là thời gian đến tối đa trong số ba màu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(thời gian trả lời) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba kích cỡ nhóm`r`,`g`, Và`b`. 
2. Với mỗi màu, hãy tính xem cần bao nhiêu toa cáp treo. 

Vì mỗi toa cáp chở tối đa hai học sinh nên số toa cần thiết là:```
(x + 1) // 2
```Đây là phép chia trần số nguyên cho`x / 2`. 
3. Tính thời gian khởi hành của cáp treo cuối cùng cho mỗi màu. 

Xe màu đỏ xuất phát đúng giờ`0`, màu xanh lá cây tại`1`, và màu xanh tại`2`. 

Nếu một màu sắc cần`k`xe, chuyến cuối cùng khởi hành lúc:```
offset + (k - 1) * 3
```vì hai ô tô cùng màu liên tiếp cách nhau 3 phút. 
4. Bỏ qua màu sắc khi không có học sinh. 

Họ không đóng góp vào câu trả lời cuối cùng. 
5. Cộng thêm 30 phút cho mỗi giờ khởi hành cuối cùng để có được thời gian đến. 
6. Xuất ra thời gian đến tối đa trong số tất cả các màu. 

Cả nhóm chỉ kết thúc khi nhóm chậm nhất lên đến đỉnh. 

### Tại sao nó hoạt động 

Mỗi màu hoạt động độc lập vì học sinh chỉ lên màu ưa thích của mình. Lịch trình của mỗi màu là cố định và định kỳ, vì vậy cách sớm nhất có thể để vận chuyển tất cả học sinh là lấp đầy mọi chuyến cáp treo có sẵn ngay lập tức. 

Đối với một nhóm cần`k`những chuyến đi, những chuyến đi đó phải sử dụng chuyến đi đầu tiên`k`cáp treo có màu đó. Bất kỳ sự chậm trễ nào cũng sẽ chỉ khiến chuyến đến cuối cùng muộn hơn. Vì thời gian đi luôn là 30 phút nên việc giảm thiểu thời gian khởi hành cuối cùng cũng giảm thiểu thời gian đến cuối cùng. 

Tận dụng tối đa các màu sắc có tác dụng vì tất cả các nhóm di chuyển đồng thời và độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    r, g, b = map(int, input().split())

    ans = 0

    groups = [
        (r, 0),  # red
        (g, 1),  # green
        (b, 2)   # blue
    ]

    for cnt, offset in groups:
        if cnt == 0:
            continue

        cars = (cnt + 1) // 2
        last_departure = offset + (cars - 1) * 3
        ans = max(ans, last_departure + 30)

    print(ans)

solve()
```Giải pháp tuân theo quan sát toán học trực tiếp. 

Biến`cars`tính toán số lượng cáp treo cần thiết cho một nhóm màu. sử dụng`(cnt + 1) // 2`là thủ thuật số nguyên tiêu chuẩn để chia trần cho hai. 

Biểu thức:```
offset + (cars - 1) * 3
```tính thời gian khởi hành của cáp treo hữu ích cuối cùng có màu đó. Việc trừ đi một là quan trọng. Nếu chỉ cần một cáp treo, chuyến khởi hành cuối cùng sẽ diễn ra chính xác ở điểm xuất phát ban đầu. 

Ví dụ: với một học sinh màu xanh:```
cars = 1
last_departure = 2 + (1 - 1) * 3 = 2
```cái nào đúng. 

Cuối cùng, chúng tôi cộng thêm 30 phút vì thời gian di chuyển là cố định cho mỗi chuyến đi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3 2
```| Màu sắc | Sinh viên | Cần ô tô | Bù đắp | Chuyến khởi hành cuối cùng | Đến | 
| --- | --- | --- | --- | --- | --- | 
| Đỏ | 1 | 1 | 0 | 0 | 30 | 
| Xanh | 3 | 2 | 1 | 4 | 34 | 
| Màu xanh | 2 | 1 | 2 | 2 | 32 | 

Câu trả lời cuối cùng:```
34
```Dấu vết này cho thấy câu trả lời phụ thuộc vào nhóm đến mới nhất chứ không phải chỉ quy mô nhóm lớn nhất. Green cần hai chuyến đi nên cáp treo thứ hai sẽ quyết định kết quả. 

### Ví dụ 2 

đầu vào:```
0 0 1
```| Màu sắc | Sinh viên | Cần ô tô | Bù đắp | Chuyến khởi hành cuối cùng | Đến | 
| --- | --- | --- | --- | --- | --- | 
| Màu xanh | 1 | 1 | 2 | 2 | 32 | 

Câu trả lời cuối cùng:```
32
```Ví dụ này cho thấy tầm quan trọng của việc bù màu. Xe xanh không nổ máy đúng giờ`0`, nên ngay cả một học sinh cũng phải đợi đến phút`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Các ràng buộc là cực kỳ nhỏ nên ngay cả mô phỏng cũng có thể vượt qua một cách thoải mái. Giải pháp toán học là thời gian không đổi và sử dụng bộ nhớ không đổi, dễ dàng phù hợp với mọi giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    r, g, b = map(int, input().split())

    ans = 0

    groups = [
        (r, 0),
        (g, 1),
        (b, 2)
    ]

    for cnt, offset in groups:
        if cnt == 0:
            continue

        cars = (cnt + 1) // 2
        last_departure = offset + (cars - 1) * 3
        ans = max(ans, last_departure + 30)

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("1 3 2\n") == "34\n", "sample 1"

# minimum non-zero case
assert run("1 0 0\n") == "30\n", "single red student"

# blue starts at offset 2
assert run("0 0 1\n") == "32\n", "single blue student"

# odd number requiring extra ride
assert run("3 0 0\n") == "33\n", "two red rides needed"

# all equal
assert run("2 2 2\n") == "32\n", "one ride per color"

# maximum values
assert run("100 100 100\n") == "179\n", "stress test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0`|`30`| Khởi hành đỏ ngay lập tức | 
|`0 0 1`|`32`| Xử lý bù xanh | 
|`3 0 0`|`33`| Quy mô nhóm lẻ cần thêm xe | 
|`2 2 2`|`32`| Một chiếc xe đầy đủ cho mỗi màu | 
|`100 100 100`|`179`| Giá trị ràng buộc tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 0 0
```Sinh viên đỏ sử dụng cáp treo vào thời gian`0`và đến`30`. Thuật toán tính toán:```
cars = 1
last_departure = 0
arrival = 30
```phù hợp chính xác với quy trình thực tế. 

Bây giờ hãy xem xét:```
3 0 0
```Chiếc xe màu đỏ đầu tiên lúc bấy giờ`0`chở hai học sinh. Lúc đó chiếc xe màu đỏ thứ hai`3`chở học sinh còn lại. Người đến cuối cùng là:```
3 + 30 = 33
```Thuật toán tính toán:```
cars = (3 + 1) // 2 = 2
last_departure = 0 + (2 - 1) * 3 = 3
answer = 33
```Phép trừ một trong`(cars - 1)`ngăn chặn từng lỗi một. 

Cuối cùng, hãy xem xét:```
0 0 1
```Xe màu xanh thỉnh thoảng xuất hiện`2, 5, 8, ...`Hội sinh viên duy nhất vào thời điểm đó`2`và đến`32`. 

Thuật toán tính toán:```
cars = 1
last_departure = 2
arrival = 32
```xác nhận rằng độ lệch màu được xử lý chính xác.
