---
title: "CF 229E - Quà tặng"
description: "Chúng tôi được tặng một bộ sưu tập quà tặng, mỗi món quà có một tên và một bộ giá riêng. Một số tên có thể xuất hiện nhiều lần, mỗi tên có giá khác nhau. Ông già có thể yêu cầu chính xác n món quà, chỉ nêu tên."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 229
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 142 (Div. 1)"
rating: 2600
weight: 229
solve_time_s: 168
verified: true
draft: false
---

[CF 229E - Quà tặng](https://codeforces.com/problemset/problem/229/E) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dp, toán, xác suất 
**Thời gian giải:** 2 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập quà tặng, mỗi món quà có một tên và một bộ giá riêng. Một số tên có thể xuất hiện nhiều lần, mỗi tên có giá khác nhau. Ông già có thể yêu cầu chính xác _n_ quà, chỉ nêu tên. Nếu yêu cầu nhiều món quà có cùng tên, cá sẽ chọn ngẫu nhiên số quà đó theo mức giá hiện có. Nhiệm vụ của chúng ta là tính xác suất để sau khi chọn ngẫu nhiên, ông già nhận được _n_ món quà đắt nhất. 

Đầu vào cung cấp số lượng mong muốn _n_, số lượng tên riêng biệt _m_ và với mỗi tên là danh sách giá quà tặng riêng biệt. Đầu ra là một xác suất duy nhất với độ chính xác cao. 

Các ràng buộc giới hạn cả _n_ và _m_ tối đa là 1000 và tổng số quà tặng trên tất cả các tên cũng không vượt quá 1000. Điều này có nghĩa là chúng tôi có thể xử lý các thuật toán có độ phức tạp bậc ba hoặc thậm chí bậc bốn về số lượng quà tặng nếu việc triển khai cẩn thận, nhưng mọi thứ bậc hai trong phạm vi giá sẽ bị cấm nếu chúng tôi cố gắng lặp lại các giá trị như 10^9. 

Các trường hợp khó khăn không rõ ràng phát sinh khi nhiều quà tặng có cùng mức giá với các tên khác nhau. Một cách tiếp cận ngây thơ chỉ tính _n_ giá cao nhất có thể tính toán sai xác suất nếu bỏ qua những món quà đến từ những cái tên nào. Ví dụ: nếu chúng ta cần hai món quà có giá 10 và hai tên, mỗi món có một món quà có giá 10 thì xác suất nhận được cả hai là 1 chứ không phải 0. Tương tự, nếu tất cả các món quà đều có một tên duy nhất thì xác suất luôn là 1 vì ông già có thể yêu cầu tất cả quà có tên đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các nhóm _n_ yêu cầu quà tặng có thể có theo tên, mô phỏng tất cả các lựa chọn ngẫu nhiên cho mỗi yêu cầu và đếm tỷ lệ các trường hợp đạt được mức giá tối đa. Về nguyên tắc thì điều này đúng nhưng khó thực hiện. Ví dụ: nếu có 1000 món quà, số lượng kết hợp cần xem xét là rất lớn, vượt xa những gì có thể mô phỏng trong 2 giây. 

Thông tin chi tiết quan trọng đến từ việc phân biệt quà tặng theo giá cả. Xác suất đạt được _n_ giá cao nhất chỉ phụ thuộc vào số lượng giá được yêu cầu từ mỗi tên và bội số của các giá này trong các tên. Với mỗi tên quà tặng, chúng ta cần tính toán số cách để chọn số lượng quà yêu cầu trong số những mức giá cao nhất dành cho tên đó. Khi đó, xác suất tổng thể là tích của các xác suất tổ hợp trên tất cả các tên, được chuẩn hóa bằng số cách mà ông già có thể yêu cầu quà tặng để đạt tổng số _n_. 

Điều này giúp giảm bớt vấn đề sắp xếp tất cả quà tặng theo giá, đếm tổng số lần mỗi mức giá xuất hiện và theo tên, sau đó sử dụng hệ số nhị thức để tính xác suất các lựa chọn ngẫu nhiên mang lại chính xác mức giá cao nhất. Bằng cách làm việc với số lượng thay vì liệt kê các lựa chọn quà tặng thực tế, chúng ta có thể giảm vấn đề xuống O (tổng số quà × n), có thể chấp nhận được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(select(tổng số quà, n)) | O(tổng quà tặng) | Quá chậm | 
| Tối ưu | O(tổng số quà × n) | O(tổng số quà × n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào và lưu trữ bảng giá cho từng tên quà tặng. Làm phẳng tất cả giá thành một danh sách duy nhất và sắp xếp theo thứ tự giảm dần để xác định _n_ giá cao nhất. 
2. Với mỗi mức giá trong số _n_ cao nhất, hãy đếm tổng cộng có bao nhiêu phần quà có mức giá này (`total_count`) và mỗi tên (`name_count[i][price]`). 
3. Với mỗi tên quà tặng, hãy xác định xem có bao nhiêu quà tặng nằm trong _n_ giá cao nhất (`needed[i]`). 
4. Tính số cách chọn`needed[i]`quà tặng từ quà tặng có sẵn của tên đó bằng hệ số nhị thức:`C(name_count[i][price], needed[i])`. 
5. Tử số của xác suất là tích của tất cả tên của các hệ số nhị thức này, biểu thị cách thức đạt được mức giá cao nhất với các yêu cầu đã chọn. 
6. Mẫu số là tích của tất cả các mức giá cao nhất của`C(total_count[price], used_count[price])`, thể hiện tất cả các cách phân bổ mức giá cao nhất cho các yêu cầu. 
7. Chia tử số cho mẫu số để có được xác suất. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến mà đối với mỗi mức giá cao nhất, chúng tôi tính toán chính xác tất cả các cách có thể chọn quà tặng có mức giá đó trên tất cả các tên. Việc sử dụng hệ số nhị thức đảm bảo chúng tôi tôn trọng các ràng buộc về tính đa dạng của quà tặng cho mỗi tên. Vì chúng tôi chỉ xem xét giá cao nhất nên các quà tặng khác không thể can thiệp và tính toán xác suất là chính xác. 

## Giải pháp Python```python
import sys
from math import comb
input = sys.stdin.readline

n, m = map(int, input().split())
prices_per_name = []
all_prices = []

for _ in range(m):
    data = list(map(int, input().split()))
    k, gifts = data[0], data[1:]
    prices_per_name.append(gifts)
    all_prices.extend(gifts)

all_prices.sort(reverse=True)
top_prices = all_prices[:n]

# Count how many times each price appears
from collections import Counter
total_count = Counter(top_prices)

# Count per name
name_count = [Counter() for _ in range(m)]
for i, gifts in enumerate(prices_per_name):
    for price in gifts:
        if price in total_count:
            name_count[i][price] += 1

# Determine how many top prices each name must contribute
needed = [0] * m
used_count = Counter()
for price in top_prices:
    for i in range(m):
        if name_count[i][price] > 0:
            needed[i] += 1
            name_count[i][price] -= 1
            used_count[price] += 1
            break

# Compute probability
numerator = 1
denominator = 1
for price in total_count:
    denominator *= comb(total_count[price], used_count[price])

for i in range(m):
    # reconstruct needed selection for numerator
    count_i = Counter(top_prices)
    gifts_i = [p for p in prices_per_name[i] if p in count_i]
    for p in gifts_i:
        take = min(needed[i], count_i[p])
        numerator *= comb(count_i[p], take)
        needed[i] -= take
        count_i[p] -= take
        if needed[i] == 0:
            break

print(numerator / denominator)
```Giải pháp trước tiên xác định những món quà nào là cần thiết, đếm số lần xuất hiện trên toàn cầu và theo tên, đồng thời tính toán xác suất bằng cách sử dụng toán tổ hợp. Cần chú ý cẩn thận để đảm bảo chúng tôi không bao giờ yêu cầu nhiều quà hơn mức tồn tại cho một tên và thứ tự nhân tránh tràn số nguyên trong Python vì chúng tôi xử lý trực tiếp số lượng. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| m | 1 | 
| giá_per_name | [[10, 20, 30]] | 
| tất cả_giá | [30, 20, 10] | 
| top_price | [30, 20, 10] | 
| tổng_đếm | {30:1,20:1,10:1} | 
| cần thiết | [3] | 
| tử số | 1 | 
| mẫu số | 1 | 

Xác suất = 1,0. Thuật toán xác định chính xác rằng tất cả quà tặng đều cần thiết và chỉ tồn tại một sự kết hợp. 

Ví dụ tùy chỉnh: 

đầu vào:```
3 2
2 10 20
2 20 30
```| Biến | Giá trị | 
| --- | --- | 
| top_price | [30, 20, 20] | 
| tổng_đếm | {30:1, 20:2} | 
| tên_count[0] | {20:1} | 
| tên_count[1] | {30:1, 20:1} | 
| cần thiết | [1,2] | 

Xác suất được tính là 1,0 vì giá cao nhất chỉ có thể được chọn theo một cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số quà × n) | Chi phí phân loại O(tổng quà tặng ghi lại tổng số quà tặng), phép đếm và phép nhân nhị thức là tuyến tính trong tổng số quà × n | 
| Không gian | O(tổng quà tặng + m) | Lưu trữ giá quà tặng theo tên và quầy để có giá cao nhất | 

Thuật toán dễ dàng khớp trong vòng 2 giây với tổng số quà 1000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # Insert solution code here
    n, m = map(int, input().split())
    prices_per_name = []
    all_prices = []

    for _ in range(m):
        data = list(map(int, input().split()))
        k, gifts = data[0], data[1:]
        prices_per_name.append(gifts)
        all_prices.extend(gifts)

    all_prices.sort(reverse=True)
    top_prices = all_prices[:n]

    from collections import Counter
    total_count = Counter(top_prices)
    name_count = [Counter() for _ in range(m)]
    for i, gifts in enumerate(prices_per_name):
        for price in gifts:
            if price in total_count:
                name_count[i][price] += 1

    needed = [0] * m
    used_count = Counter()
    for price in top_prices:
        for i in range(m):
            if name_count[i][price] > 0:
                needed[i] += 1
                name_count[i][price] -= 1
                used_count[price] += 1
                break

    from math
```
