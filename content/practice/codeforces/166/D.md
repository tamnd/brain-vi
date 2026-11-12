---
title: "CF 166D - Cửa Hàng Giày"
description: "Chúng tôi có một bộ sưu tập giày, trong đó mỗi chiếc giày có một kích cỡ và giá riêng. Chúng tôi cũng có khách hàng, trong đó mỗi khách hàng có ngân sách và kích cỡ bàn chân. Khách hàng chỉ có thể mua giày nếu có đủ hai điều kiện."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graph-matchings", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 166
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 113 (Div. 2)"
rating: 2500
weight: 166
solve_time_s: 154
verified: true
draft: false
---

[CF 166D - Cửa hàng giày](https://codeforces.com/problemset/problem/166/D) 

**Đánh giá:** 2500 
**Thẻ:** dp, so khớp biểu đồ, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập giày, trong đó mỗi chiếc giày có một kích cỡ và giá riêng. Chúng tôi cũng có khách hàng, trong đó mỗi khách hàng có ngân sách và kích cỡ bàn chân. Khách hàng chỉ có thể mua giày nếu có đủ hai điều kiện. 

Điều kiện đầu tiên là tài chính: giá giày không được vượt quá ngân sách của khách hàng. 

Điều kiện thứ hai là khả năng tương thích về kích cỡ: nếu cỡ giày phù hợp`s`, thì kích thước chân của khách hàng phải là`s`hoặc`s - 1`. Nói cách khác, khách hàng có thể mang giày đúng kích cỡ của họ hoặc lớn hơn một cỡ. 

Mỗi chiếc giày chỉ được bán tối đa một lần và mỗi khách hàng được mua tối đa một đôi. Mục tiêu không phải là tối đa hóa số lượng bán hàng mà là tổng doanh thu từ giày bán được. 

Các ràng buộc hoàn toàn định hình giải pháp. Cả hai`n`Và`m`có thể đạt được`10^5`nên giải pháp thử tất cả các đôi giày của khách hàng ngay lập tức trở nên bất khả thi. Một thuật toán bậc hai sẽ yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất, vượt xa thời hạn. Chúng ta cần một cái gì đó gần gũi`O(n log n)`hoặc`O((n + m) log n)`. 

Kích cỡ giày độc đáo là một đặc tính cấu trúc rất chắc chắn. Vì mỗi cỡ giày xuất hiện nhiều nhất một lần trong số các loại giày nên mỗi khách hàng chỉ tương thích với tối đa hai cỡ giày. Điều đó giúp đơn giản hóa đáng kể cấu trúc đối sánh và biến vấn đề thành một vấn đề dễ quản lý hơn nhiều so với đối sánh hai bên thông thường. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét đầu vào này:```
2
100 5
1 6
1
100 5
```Đáp án đúng là bán chiếc giày đầu tiên để kiếm lời`100`. 

Một chiến lược tham lam bất cẩn xử lý giày theo thứ tự tùy ý có thể bán giày giá rẻ trước vì khách hàng cũng có thể mang size`6`. Điều đó ngăn chặn việc bán hàng đắt tiền và mang lại lợi nhuận`1`thay vì`100`. 

Một trường hợp tinh vi khác xuất hiện khi hai khách hàng có thể mua cùng một chiếc giày nhưng chỉ một trong số họ có lựa chọn thay thế.```
2
50 10
40 11
2
50 10
50 11
```Giải pháp tối ưu là:```
90
2
1 1
2 2
```Nếu chúng ta tham lam gán kích thước`10`giày cho khách hàng`2`, thì khách hàng`1`trở nên không thể so sánh được và tổng lợi nhuận giảm xuống`50`. 

Ngoài ra còn có một trường hợp nguy hiểm là việc chọn một mặt hàng bán đắt ở địa phương sẽ cản trở hai giao dịch bán hàng trong tương lai.```
3
100 5
99 6
98 7
2
100 5
99 6
```Câu trả lời đúng là`199`, bằng cách bán giày`5`Và`6`. 

Một thuật toán tham lam luôn ưu tiên giày có giá cao nhất có thể lấy giày`7`cho khách hàng`2`, vì kích thước`6`khách hàng cũng có thể mặc size`7`. Sau đó giày`6`trở nên không thể bán được và kết quả trở thành`198`. 

Vấn đề cơ bản là duy trì tính linh hoạt một cách cẩn thận trong khi tối đa hóa tổng giá trị. 

## Phương pháp tiếp cận 

Công thức trực tiếp nhất là sự kết hợp lưỡng cực. Một bên chứa khách hàng, một bên chứa giày dép. Chúng tôi tạo thêm lợi thế khi khách hàng có thể mua một đôi giày. Vì mỗi khách hàng chỉ có thể kết nối với tối đa hai cỡ giày nên biểu đồ rất thưa thớt. 

Một giải pháp brute-force có thể xây dựng toàn bộ biểu đồ và chạy thuật toán so khớp có trọng số. Việc kết hợp phải tối đa hóa tổng giá giày. Việc kết hợp lưỡng đảng có trọng số chung ở đây là quá đắt. Ngay cả việc xây dựng tất cả các cặp có thể một cách ngây thơ cũng sẽ mất`O(nm)`hoạt động. 

Quan sát quan trọng là kích cỡ giày là duy nhất. Một chiếc giày có kích thước`s`chỉ có thể được mua bởi khách hàng với kích thước bàn chân`s`hoặc`s - 1`. 

Điều đó có nghĩa là mỗi chiếc giày chỉ tương tác với hai nhóm khách hàng. 

Điều này biến biểu đồ thành cấu trúc dạng chuỗi khi kích thước được xử lý theo thứ tự được sắp xếp. Sự phụ thuộc trở thành cục bộ thay vì toàn cầu. 

Giả sử chúng ta sắp xếp giày theo kích cỡ. Khi chúng ta kiểm tra một chiếc giày có kích cỡ`s`, chỉ những khách hàng có quy mô`s`Và`s - 1`vấn đề. Giày trong tương lai có kích thước lớn hơn, vì vậy khách hàng có kích thước`s - 1`không có cơ hội trong tương lai sau kích thước`s`. Nếu bây giờ chúng ta bỏ qua việc chỉ định một khách hàng như vậy, chúng ta sẽ mất họ mãi mãi. 

Điều này tự nhiên gợi ý lập trình động trên các kích thước được sắp xếp. 

Đối với mọi vị trí kích thước, chúng ta chỉ cần biết liệu chiếc giày trước đó đã tiêu thụ một khách hàng có kích thước hiện tại hay chưa. Trạng thái nhỏ bé đó là đủ vì sự tương tác chỉ xảy ra giữa các kích thước lân cận. 

DP quyết định xem có nên: 

1. Bỏ qua giày hiện tại. 
2. Bán nó cho khách hàng có cùng quy mô. 
3. Bán nó cho khách hàng có quy mô nhỏ hơn một. 

Để làm cho những quá trình chuyển đổi này trở nên hiệu quả, chúng tôi xử lý trước mọi kích cỡ giày cho những khách hàng có kích cỡ phù hợp nhất. 

Thuật toán kết quả chạy trong`O((n + m) log n)`do sắp xếp và xử lý ưu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu | O((n + m)^3) hoặc tệ hơn | O(nm) | Quá chậm | 
| DP về kích cỡ được sắp xếp | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả giày và cất giữ`(size, price, index)`. 
2. Đọc tất cả khách hàng và nhóm họ theo cỡ chân. 

Đối với mọi kích thước foot, hãy sắp xếp khách hàng theo ngân sách theo thứ tự giảm dần. Trước tiên, chúng tôi chỉ cần những khách hàng giàu có nhất còn lại. 

1. Phân loại giày theo kích cỡ. 

Vì khả năng tương thích chỉ liên quan đến kích thước bằng nhau hoặc một kích thước nhỏ hơn nên việc xử lý các kích thước theo thứ tự sẽ làm cho tất cả các phần phụ thuộc trở nên cục bộ. 

1. Đối với mỗi chiếc giày, hãy tính toán trước hai khách hàng ứng viên. 

Ứng cử viên đầu tiên là khách hàng giàu có nhất chưa từng sử dụng, có cỡ chân bằng cỡ giày. 

Ứng cử viên thứ hai là khách hàng giàu nhất chưa sử dụng có kích thước bàn chân nhỏ hơn một. 

Nếu khách hàng không đủ tiền mua giày, chúng sẽ bị loại bỏ. 

1. Xác định trạng thái DP. 

Cho phép`dp[i][t]`là lợi nhuận tối đa sau khi xử lý lần đầu tiên`i`đôi giày. 

Lá cờ`t`cho biết liệu khách hàng tốt nhất của cỡ giày hiện tại đã sử dụng chiếc giày trước đó hay chưa. 

Trạng thái này là đủ vì các cỡ giày lân cận là những cỡ tương tác duy nhất. 

1. Chuyển đổi bằng cách bỏ qua giày. 

Chúng ta có thể bỏ qua vấn đề hiện tại và tiếp tục đưa ra câu trả lời trước đó. 

1. Chuyển đổi bằng cách bán cho khách hàng có cùng quy mô. 

Nếu có sẵn khách hàng có cùng kích cỡ, hãy thêm giá giày và chuyển sang trạng thái tiếp theo. 

1. Chuyển đổi bằng cách bán cho khách hàng có quy mô nhỏ hơn. 

Nếu một khách hàng có quy mô`s - 1`tồn tại và chưa được sử dụng bởi chiếc giày trước đó, chúng tôi có thể sử dụng chúng. 

1. Lưu trữ con trỏ gốc. 

Để xây dựng lại kết quả khớp, hãy giữ trạng thái trước đó và hành động được thực hiện. 

1. Xây dựng lại các nhiệm vụ đã chọn bằng cách quay lui qua các trạng thái DP. 
2. In ra tổng lợi nhuận và các đôi giày của khách hàng đã chọn. 

### Tại sao nó hoạt động 

Bất biến chính là khi xử lý giày theo thứ tự kích thước tăng dần, chỉ những khách hàng có kích thước hiện tại và kích thước nhỏ hơn liền kề mới có thể tương tác với giày hiện tại. 

Một khách hàng có quy mô`s - 1`không có cơ hội trong tương lai sau kích thước`s`, vì sau này giày còn to hơn nữa. Điều đó có nghĩa là mọi quyết định liên quan đến những khách hàng đó sẽ trở thành quyết định cuối cùng ngay lập tức. 

Trạng thái DP nắm bắt chính xác sự mơ hồ duy nhất có thể lan truyền về phía trước: liệu khách hàng ở quy mô hiện tại đã được sử dụng bởi chiếc giày trước đó hay chưa. Không có sự phụ thuộc lớn hơn tồn tại. 

Bởi vì mọi quá trình chuyển đổi đều xem xét tất cả các khả năng pháp lý cho chiếc giày hiện tại trong khi vẫn duy trì cấu trúc phụ tối ưu, nên DP sẽ ngầm khám phá tất cả các kết hợp tối ưu. Bước xây dựng lại chỉ đơn giản là thực hiện theo các lựa chọn tạo ra giá trị tối ưu. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

INF = 10**30

def solve():
    n = int(input())

    shoes = []
    for i in range(n):
        c, s = map(int, input().split())
        shoes.append((s, c, i + 1))

    m = int(input())

    customers_by_size = defaultdict(list)

    for i in range(m):
        d, l = map(int, input().split())
        customers_by_size[l].append((d, i + 1))

    for l in customers_by_size:
        customers_by_size[l].sort(reverse=True)

    shoes.sort()

    ptr = defaultdict(int)

    same_customer = [None] * n
    prev_customer = [None] * n

    for i, (s, c, idx) in enumerate(shoes):
        arr = customers_by_size[s]

        while ptr[s] < len(arr) and arr[ptr[s]][0] < c:
            ptr[s] += 1

        if ptr[s] < len(arr):
            same_customer[i] = arr[ptr[s]]

        arr2 = customers_by_size[s - 1]

        while ptr[s - 1] < len(arr2) and arr2[ptr[s - 1]][0] < c:
            ptr[s - 1] += 1

        if ptr[s - 1] < len(arr2):
            prev_customer[i] = arr2[ptr[s - 1]]

    dp = [[-INF] * 2 for _ in range(n + 1)]
    parent = [[None] * 2 for _ in range(n + 1)]

    dp[0][0] = 0

    for i in range(n):
        s, c, shoe_idx = shoes[i]

        for used in range(2):
            if dp[i][used] < 0:
                continue

            # skip
            if dp[i][used] > dp[i + 1][0]:
                dp[i + 1][0] = dp[i][used]
                parent[i + 1][0] = (i, used, 0, None)

            # use same size customer
            if same_customer[i] is not None:
                val = dp[i][used] + c

                nxt = 1

                if val > dp[i + 1][nxt]:
                    dp[i + 1][nxt] = val
                    parent[i + 1][nxt] = (
                        i,
                        used,
                        1,
                        same_customer[i][1],
                    )

            # use smaller size customer
            if used == 0 and prev_customer[i] is not None:
                val = dp[i][used] + c

                if val > dp[i + 1][0]:
                    dp[i + 1][0] = val
                    parent[i + 1][0] = (
                        i,
                        used,
                        2,
                        prev_customer[i][1],
                    )

    if dp[n][0] >= dp[n][1]:
        state = 0
    else:
        state = 1

    best = dp[n][state]

    ans = []

    cur_i = n
    cur_state = state

    while cur_i > 0:
        p_i, p_state, action, customer_id = parent[cur_i][cur_state]

        if action == 1 or action == 2:
            shoe_idx = shoes[cur_i - 1][2]
            ans.append((customer_id, shoe_idx))

        cur_i = p_i
        cur_state = p_state

    ans.reverse()

    print(best)
    print(len(ans))

    for x, y in ans:
        print(x, y)

solve()
```Giải pháp bắt đầu bằng việc phân nhóm khách hàng theo cỡ chân. Đối với mọi quy mô, khách hàng được sắp xếp theo ngân sách giảm dần. Điều này đảm bảo rằng khi chúng ta tìm kiếm người mua một đôi giày nào đó, khách hàng hợp lệ đầu tiên luôn là người hữu ích nhất. 

Giai đoạn tiền xử lý sẽ tính toán các ứng cử viên có cùng kích thước và kích thước nhỏ hơn tốt nhất cho mỗi chiếc giày. Vì kích cỡ giày là duy nhất nên các mối quan hệ ứng cử viên này chỉ mang tính địa phương. 

Bảng DP chỉ có hai trạng thái cho mỗi vị trí. Đó là khả năng nén cốt lõi làm cho giải pháp trở nên hiệu quả. Trạng thái boolean ghi lại xem nhóm kích thước hiện tại đã được sử dụng bởi chiếc giày trước đó hay chưa. 

Logic chuyển tiếp bảo toàn tính độc quyền một cách cẩn thận. các`used == 0`điều kiện rất quan trọng khi phân công một khách hàng có quy mô nhỏ hơn. Nếu không có nó, cùng một nhóm khách hàng có thể vô tình được sử dụng lại cho những đôi giày lân cận. 

Giai đoạn xây dựng lại theo sau các con trỏ gốc được lưu trữ ngược từ trạng thái cuối tối ưu. Mỗi hành động ghi lại chính xác khách hàng nào đã mua loại giày nào. 

Tất cả các tính toán lợi nhuận đều sử dụng số nguyên Python bình thường, xử lý tổng giá trị tối đa có thể một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
10 1
30 2
20 3
2
20 1
20 2
```Phân loại giày: 

| Vị trí | Cỡ giày | Giá | 
| --- | --- | --- | 
| 1 | 1 | 10 | 
| 2 | 2 | 30 | 
| 3 | 3 | 20 | 

Khách hàng: 

| Khách hàng | Ngân sách | Kích thước bàn chân | 
| --- | --- | --- | 
| 1 | 20 | 1 | 
| 2 | 20 | 2 | 

Dấu vết DP: 

| Giày | Hành động | Lợi nhuận | 
| --- | --- | --- | 
| Kích thước 1 | Bán cho khách hàng 1 | 10 | 
| Kích thước 2 | Không thể bán với giá 30 | 10 | 
| Cỡ 3 | Bán cho khách hàng 2 | 30 | 

Nhiệm vụ cuối cùng:```
Customer 1 -> Shoe 1
Customer 2 -> Shoe 3
```Tổng lợi nhuận trở thành`30`. 

Ví dụ này cho thấy tại sao khả năng tương thích với kích thước`s - 1`vấn đề. Khách hàng`2`không đủ tiền mua giày`2`, nhưng vẫn có thể mua giày`3`. 

### Ví dụ 2 

đầu vào:```
2
50 10
40 11
2
50 10
50 11
```Phân loại giày: 

| Vị trí | Cỡ giày | Giá | 
| --- | --- | --- | 
| 1 | 10 | 50 | 
| 2 | 11 | 40 | 

Dấu vết DP: 

| Giày | Khách hàng được chọn | Chạy lợi nhuận | 
| --- | --- | --- | 
| Kích thước 10 | Khách hàng 1 | 50 | 
| Cỡ 11 | Khách hàng 2 | 90 | 

Nếu giày`10`đã được bán cho khách hàng`2`, rồi giày`11`sẽ trở nên không thể bán được. DP đã tránh được sai lầm đó một cách chính xác. 

Dấu vết này chứng tỏ sự bất biến rằng khách hàng có quy mô nhỏ hơn nên được tiêu thụ càng muộn càng tốt trừ khi không có lựa chọn nào tốt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n + m) | Lưu trữ cho khách hàng, giày dép, DP và tái thiết | 

Giải pháp thoải mái phù hợp với các hạn chế. Sắp xếp`2 * 10^5`đối tượng và thực hiện chuyển đổi DP tuyến tính đủ nhanh trong giới hạn 2 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    from collections import defaultdict

    input = sys.stdin.readline
    INF = 10**30

    out = io.StringIO()
    sys.stdout = out

    n = int(input())

    shoes = []
    for i in range(n):
        c, s = map(int, input().split())
        shoes.append((s, c, i + 1))

    m = int(input())

    customers_by_size = defaultdict(list)

    for i in range(m):
        d, l = map(int, input().split())
        customers_by_size[l].append((d, i + 1))

    for l in customers_by_size:
        customers_by_size[l].sort(reverse=True)

    shoes.sort()

    ptr = defaultdict(int)

    same_customer = [None] * n
    prev_customer = [None] * n

    for i, (s, c, idx) in enumerate(shoes):
        arr = customers_by_size[s]

        while ptr[s] < len(arr) and arr[ptr[s]][0] < c:
            ptr[s] += 1

        if ptr[s] < len(arr):
            same_customer[i] = arr[ptr[s]]

        arr2 = customers_by_size[s - 1]

        while ptr[s - 1] < len(arr2) and arr2[ptr[s - 1]][0] < c:
            ptr[s - 1] += 1

        if ptr[s - 1] < len(arr2):
            prev_customer[i] = arr2[ptr[s - 1]]

    dp = [[-INF] * 2 for _ in range(n + 1)]
    dp[0][0] = 0

    for i in range(n):
        s, c, idx = shoes[i]

        for used in range(2):
            if dp[i][used] < 0:
                continue

            dp[i + 1][0] = max(dp[i + 1][0], dp[i][used])

            if same_customer[i] is not None:
                dp[i + 1][1] = max(
                    dp[i + 1][1],
                    dp[i][used] + c
                )

            if used == 0 and prev_customer[i] is not None:
                dp[i + 1][0] = max(
                    dp[i + 1][0],
                    dp[i][used] + c
                )

    best = max(dp[n])

    return str(best).strip()

# sample 1
assert run(
"""3
10 1
30 2
20 3
2
20 1
20 2
"""
) == "30"

# minimum case
assert run(
"""1
5 10
1
5 10
"""
) == "5"

# no customer can buy
assert run(
"""2
100 1
200 2
2
50 1
50 2
"""
) == "0"

# smaller-size compatibility
assert run(
"""1
30 5
1
30 4
"""
) == "30"

# avoid greedy mistake
assert run(
"""2
50 10
40 11
2
50 10
50 11
"""
) == "90"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giày đơn và khách hàng | 5 | Đầu vào hợp lệ tối thiểu | 
| Không ai có đủ tiền mua giày | 0 | Xử lý đúng những trường hợp bán hàng không thành công | 
| Khách hàng có kích thước`s - 1`| 30 | Ranh giới tương thích | 
| Hai chiếc giày tương tác | 90 | Ngăn chặn sự phân công tham lam không chính xác | 

## Vỏ cạnh 

Hãy xem xét ví dụ trước đó trong đó chiến lược tham lam có thể phá hủy lợi nhuận. 

đầu vào:```
2
100 5
1 6
1
100 5
```Khi xử lý size giày`5`, thuật toán nhìn thấy một khách hàng có cùng quy mô hoàn hảo và thu được lợi nhuận`100`. 

Khi xử lý size giày`6`, không còn khách hàng tương thích nào nữa. 

Câu trả lời cuối cùng:```
100
1
1 1
```DP không bao giờ hy sinh chiếc giày đắt tiền để lấy chiếc rẻ tiền vì nó đánh giá tổng lợi nhuận trong tương lai hơn là tính sẵn có của địa phương. 

Bây giờ hãy xem xét trường hợp tương tác:```
2
50 10
40 11
2
50 10
50 11
```Tại giày`10`, khách hàng có quy mô`11`tương thích về mặt kỹ thuật. Nhưng giao họ ngay sẽ cản trở việc đánh giày`11`. 

DP so sánh cả hai hợp đồng tương lai: 

| Quyết định ở Giày 10 | Lợi nhuận cuối cùng | 
| --- | --- | 
| Sử dụng khách hàng 10 | 90 | 
| Sử dụng khách hàng 11 | 50 | 

Nhánh tốt hơn sẽ tồn tại. 

Cuối cùng, hãy xem xét trường hợp tương thích ranh giới này:```
1
30 5
1
30 4
```Cỡ giày của khách hàng chính xác là nhỏ hơn một cỡ so với cỡ giày được phép. 

Thuật toán kiểm tra cả hai nhóm ứng cử viên: kích thước`5`và kích thước`4`. Kích thước`4`khách hàng thỏa mãn điều kiện ngân sách nên việc bán hàng thành công với tổng lợi nhuận`30`.
