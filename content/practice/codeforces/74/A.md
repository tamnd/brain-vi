---
title: "CF 74A - Trưởng Phòng"
description: "Chúng tôi được cung cấp dữ liệu bảng điểm cho mọi người tham gia trong phòng Codeforces. Mỗi thí sinh có một tay cầm, một số lần hack thành công, một số lần hack không thành công và số điểm kiếm được từ các vấn đề từ A đến E."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 74
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 68"
rating: 1000
weight: 74
solve_time_s: 95
verified: true
draft: false
---

[CF 74A - Trưởng phòng](https://codeforces.com/problemset/problem/74/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp dữ liệu bảng điểm cho mọi người tham gia trong phòng Codeforces. Mỗi thí sinh có một tay cầm, một số lần hack thành công, một số lần hack không thành công và số điểm kiếm được từ các vấn đề từ A đến E. 

Tổng số điểm của một người tham gia được tính như sau: 

- tổng của năm điểm vấn đề, 
- cộng`100 × successful_hacks`, 
- trừ`50 × unsuccessful_hacks`. 

Nhiệm vụ của chúng tôi chỉ đơn giản là xác định người tham gia nào có điểm cuối cùng cao nhất và in tay cầm của họ ra. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 50 người tham gia và mỗi người tham gia chỉ có một lượng thông tin cố định. Ngay cả một giải pháp không hiệu quả cũng sẽ phù hợp một cách thoải mái trong thời hạn. Một lần vượt qua các thí sinh là đủ. 

Rủi ro triển khai chính không phải là độ phức tạp của thuật toán, mà là tính toán điểm chính xác và xử lý tổng số âm đúng cách. 

Một sai lầm dễ mắc phải là quên rằng những lần hack không thành công sẽ bị trừ điểm. 

Ví dụ:```
2
alice 0 0 500 500 500 500 500
bob 0 10 500 500 500 500 500
```Alice có`2500`điểm. 

Bob có`2500 - 10 × 50 = 2000`. 

Đầu ra đúng là:```
alice
```Việc triển khai bất cẩn mà bỏ qua các hình phạt hack sẽ chọn nhầm Bob là bị ràng buộc hoặc ngang bằng. 

Một trường hợp tinh vi khác là khi một thí sinh không giải được gì nhưng vẫn bị điểm âm do hack không thành công. 

Ví dụ:```
2
x 0 5 0 0 0 0 0
y 0 0 0 0 0 0 0
```Thí sinh`x`có`-250`, trong khi`y`có`0`. 

Đầu ra đúng là:```
y
```Nếu chúng ta khởi tạo điểm số tốt nhất để`0`thay vì một cái gì đó nhỏ một cách an toàn thì các trường hợp âm tính hoàn toàn có thể thất bại. 

Lỗi phổ biến thứ ba là đọc dữ liệu đầu vào không chính xác vì phần điều khiển là một chuỗi trong khi các trường còn lại là số nguyên. 

Ví dụ:```
1
tourist_123 1 0 500 1000 1500 2000 2500
```Đầu ra đúng là:```
tourist_123
```Trình phân tích cú pháp phải giữ phần điều khiển tách biệt khỏi các giá trị số. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi người tham gia, hãy tính điểm cuối cùng của họ từ các trường đầu vào, sau đó so sánh với những người tham gia khác để kiểm tra xem đó có phải là điểm tối đa hay không. 

Điều này hiệu quả vì số lượng thí sinh rất ít. Nếu có`n`người tham gia, cách tiếp cận này thực hiện gần như`n²`so sánh. Với`n ≤ 50`, đó chỉ là`2500`so sánh trong trường hợp xấu nhất 

Vấn đề với phương pháp này ở đây không phải là hiệu suất mà là những công việc không cần thiết. Sau khi tính điểm của một thí sinh, chúng tôi thực sự không cần phải so sánh điểm đó với từng người khác. Chúng tôi chỉ quan tâm đến mức tối đa hiện tại. 

Quan sát đó dẫn đến cách tiếp cận tối ưu. Trong khi đọc từng người tham gia, chúng tôi tính toán điểm của họ ngay lập tức và duy trì hai biến: 

- điểm số tốt nhất được thấy cho đến nay, 
- tay cầm tương ứng. 

Bất cứ khi nào người tham gia mới có số điểm lớn hơn, chúng tôi sẽ cập nhật cả hai giá trị. 

Điều này làm giảm công việc từ việc so sánh lặp đi lặp lại thành một lần quét tuyến tính. Cấu trúc của đề khiến điều này trở nên tự nhiên vì người chiến thắng chỉ phụ thuộc vào số điểm tối đa chứ không phụ thuộc vào bất kỳ mối quan hệ cặp đôi nào giữa các thí sinh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số lượng thí sinh. 
2. Khởi tạo hai biến: 

-`best_score`là một con số rất nhỏ, 
-`best_handle`dưới dạng một chuỗi rỗng. 

Chúng ta cần những biến này để theo dõi người dẫn đầu hiện tại trong khi xử lý dữ liệu đầu vào. 
3. Đối với mỗi thí sinh: 

- đọc tay cầm, 
- đọc số lần hack thành công, 
- đọc số lần hack không thành công, 
- đọc năm điểm vấn đề. 
4. Tính tổng điểm của thí sinh bằng cách sử dụng:```
total = sum(problem_scores) + 100 × successful_hacks - 50 × unsuccessful_hacks
```5. So sánh tổng số điểm này với`best_score`. 
6. Nếu điểm mới lớn hơn: 

- cập nhật`best_score`,
- cập nhật`best_handle`. 

Điều này giúp thông tin về người đứng đầu luôn chính xác sau mỗi thí sinh được xử lý. 
7. Sau khi xử lý xong tất cả thí sinh, hãy in`best_handle`. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của thí sinh,`best_score`lưu trữ điểm tối đa trong số tất cả những người tham gia được xử lý và`best_handle`lưu trữ tay cầm tương ứng. 

Ban đầu bất biến này đúng vì chưa có đối thủ nào được xử lý và`best_score`nhỏ hơn bất kỳ số điểm thực tế nào có thể có. 

Khi xử lý một thí sinh mới, chỉ có hai khả năng: 

- điểm của họ không lớn hơn mức tối đa hiện tại nên người dẫn đầu không thay đổi, 
- điểm của họ lớn hơn, do đó việc thay thế người dẫn đầu được lưu trữ sẽ giữ cho bất biến đúng. 

Sau thí sinh cuối cùng, bất biến đảm bảo rằng tay cầm được lưu trữ sẽ thuộc về người tham gia có số điểm cao nhất trong toàn phòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

best_score = -10**18
best_handle = ""

for _ in range(n):
    parts = input().split()

    handle = parts[0]
    plus = int(parts[1])
    minus = int(parts[2])

    scores = list(map(int, parts[3:]))

    total = sum(scores) + plus * 100 - minus * 50

    if total > best_score:
        best_score = total
        best_handle = handle

print(best_handle)
```Giải pháp tuân theo thuật toán trực tiếp. 

Chi tiết quan trọng đầu tiên là cách phân tích cú pháp dòng đầu vào. Handle được lưu trữ riêng vì nó là một chuỗi, trong khi các trường còn lại là số nguyên. 

Cách tính điểm chính xác phù hợp với thể lệ cuộc thi. Thêm hack thành công`100`điểm, hack không thành công bị trừ`50`, và năm điểm của vấn đề được tính tổng bình thường. 

Biến`best_score`được khởi tạo ở một giá trị rất nhỏ thay vì`0`. Điều này tránh được lỗi khi thí sinh nào cũng bị điểm âm. 

Giải pháp cập nhật người dẫn đầu ngay sau khi tính điểm của từng người tham gia, do đó không cần thêm mảng hoặc sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
Petr 3 1 490 920 1000 1200 0
tourist 2 0 490 950 1100 1400 0
Egor 7 0 480 900 950 0 1000
c00lH4x0R 0 10 150 0 0 0 0
some_participant 2 1 450 720 900 0 0
```| Thí sinh | Tổng bài toán | Hack Tiền thưởng/Hình phạt | Tổng cộng | Lãnh đạo hiện tại | 
| --- | --- | --- | --- | --- | 
| Petr | 3610 | +250 | 3860 | Petr | 
| du lịch | 3940 | +200 | 4140 | du lịch | 
| Egor | 3330 | +700 | 4030 | du lịch | 
| c00lH4x0R | 150 | -500 | -350 | du lịch | 
| some_participant | 2070 | +150 | 2220 | du lịch | 

Đầu ra cuối cùng:```
tourist
```Dấu vết này cho thấy mức tối đa đang chạy chỉ thay đổi như thế nào khi xuất hiện điểm lớn hơn. 

### Ví dụ 2 

đầu vào:```
3
alice 0 5 0 0 0 0 0
bob 0 0 0 0 0 0 0
charlie 1 0 0 0 0 0 0
```| Thí sinh | Tổng bài toán | Hack Tiền thưởng/Hình phạt | Tổng cộng | Lãnh đạo hiện tại | 
| --- | --- | --- | --- | --- | 
| Alice | 0 | -250 | -250 | Alice | 
| bob | 0 | 0 | 0 | bob | 
| Charlie | 0 | +100 | 100 | Charlie | 

Đầu ra cuối cùng:```
charlie
```Ví dụ này chứng minh tại sao điểm âm phải được xử lý chính xác và tại sao người lãnh đạo có thể thay đổi nhiều lần trong quá trình quét. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thí sinh được xử lý đúng một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với tối đa 50 thí sinh, lời giải vượt xa giới hạn thời gian và trí nhớ. Ngay cả những cách tiếp cận chậm hơn cũng có thể vượt qua, nhưng quét tuyến tính là cách triển khai đơn giản và rõ ràng nhất. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())

    best_score = -10**18
    best_handle = ""

    for _ in range(n):
        parts = input().split()

        handle = parts[0]
        plus = int(parts[1])
        minus = int(parts[2])

        scores = list(map(int, parts[3:]))

        total = sum(scores) + plus * 100 - minus * 50

        if total > best_score:
            best_score = total
            best_handle = handle

    print(best_handle)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""5
Petr 3 1 490 920 1000 1200 0
tourist 2 0 490 950 1100 1400 0
Egor 7 0 480 900 950 0 1000
c00lH4x0R 0 10 150 0 0 0 0
some_participant 2 1 450 720 900 0 0
"""
) == "tourist", "sample 1"

# minimum-size input
assert run(
"""1
solo 0 0 0 0 0 0 0
"""
) == "solo", "single contestant"

# negative scores
assert run(
"""2
x 0 5 0 0 0 0 0
y 0 0 0 0 0 0 0
"""
) == "y", "negative totals"

# hack bonus changes winner
assert run(
"""2
alice 0 0 500 500 500 500 500
bob 3 0 500 500 500 500 500
"""
) == "bob", "successful hacks matter"

# boundary-style large values
assert run(
"""2
maxer 50 0 500 1000 1500 2000 2500
other 0 0 500 1000 1500 2000 2500
"""
) == "maxer", "large bonuses"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thí sinh độc thân |`solo`| Đầu vào kích thước tối thiểu | 
| Trường hợp điểm âm |`y`| Xử lý đúng các tổng số âm | 
| Điểm vấn đề bằng nhau nhưng cách hack khác nhau |`bob`| Hack thành công được thêm đúng cách | 
| Trường hợp có giá trị lớn |`maxer`| Số học kiểu giới hạn trên | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các điểm đều âm ngoại trừ một điểm 0. 

đầu vào:```
2
x 0 5 0 0 0 0 0
y 0 0 0 0 0 0 0
```Thuật toán bắt đầu với một số rất nhỏ`best_score`. 

Xử lý`x`: 

- tổng cộng =`-250`- người lãnh đạo trở thành`x`Xử lý`y`: 

- tổng cộng =`0`- từ`0 > -250`, người lãnh đạo trở thành`y`Đầu ra là:```
y
```Điều này hoạt động vì giá trị ban đầu của`best_score`nhỏ hơn mọi số điểm có thể có của thí sinh. 

Bây giờ hãy xem xét trường hợp hình phạt hack quyết định người chiến thắng. 

đầu vào:```
2
alice 0 0 500 500 500 500 500
bob 0 10 500 500 500 500 500
```Thuật toán tính toán: 

-`alice = 2500`-`bob = 2000`Người đứng đầu vẫn còn`alice`. 

Đầu ra là:```
alice
```Điều này xác nhận rằng các lần hack không thành công sẽ được trừ một cách chính xác. 

Cuối cùng, hãy xem xét các thẻ điều khiển có chứa dấu gạch dưới và chữ số. 

đầu vào:```
1
tourist_123 1 0 500 1000 1500 2000 2500
```Trình phân tích cú pháp lưu trữ: 

- tay cầm =`tourist_123`- các giá trị còn lại là số nguyên 

Tổng số điểm được tính toán bình thường và kết quả là:```
tourist_123
```Điều này xác minh rằng việc triển khai phân tách chính xác bộ điều khiển chuỗi khỏi các trường số.
