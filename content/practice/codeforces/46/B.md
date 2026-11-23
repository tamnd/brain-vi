---
title: "CF 46B - Áo thun từ nhà tài trợ"
description: "Chúng tôi được cung cấp số lượng áo phông có hạn với 5 kích cỡ: S, M, L, XL và XXL. Mỗi người tham gia cuộc thi có một kích thước ưa thích. Người tham gia đến theo một thứ tự cố định và cố gắng chọn chiếc áo phông gần nhất với kích cỡ ưa thích của họ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 46
codeforces_index: "B"
codeforces_contest_name: "School Personal Contest #2 (Winter Computer School 2010/11) - Codeforces Beta Round 43 (ACM-ICPC Rules)"
rating: 1100
weight: 46
solve_time_s: 73
verified: true
draft: false
---

[CF 46B - Áo thun từ nhà tài trợ](https://codeforces.com/problemset/problem/46/B) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp số lượng áo phông có hạn với 5 kích cỡ: S, M, L, XL và XXL. Mỗi người tham gia cuộc thi có một kích thước ưa thích. Người tham gia đến theo một thứ tự cố định và cố gắng chọn chiếc áo phông gần nhất với kích cỡ ưa thích của họ. Nếu có kích thước ưa thích của họ, họ sẽ lấy nó. Nếu không, họ sẽ chọn kích cỡ gần nhất có sẵn. Nếu hai kích thước gần nhau thì chọn kích thước lớn hơn. Mục tiêu là xác định chính xác kích thước mà mỗi người tham gia nhận được. 

Dữ liệu đầu vào bao gồm năm số nguyên biểu thị số lượng áo phông ở mỗi kích cỡ, theo sau là số lượng người tham gia và sau đó là danh sách các kích cỡ ưa thích theo thứ tự mà người tham gia đến. Đầu ra là một chuỗi các kích cỡ áo phông thực tế mà mỗi người tham gia nhận được. 

Các ràng buộc rất chặt chẽ nhưng có thể quản lý được. Với tối đa 1000 người tham gia và tối đa 1000 áo phông ở mỗi kích cỡ, chúng tôi có thể thực hiện tối đa khoảng 5000 lượt kiểm tra cho mỗi người tham gia mà không vượt quá giới hạn thời gian 2 giây thông thường. Điều này ngụ ý một thuật toán gần như$O(K \cdot 5)$, trong đó 5 là số lượng kích cỡ, có thể chấp nhận được. Các trường hợp khó khăn bao gồm việc không có áo phông ở một số kích cỡ nhất định, nhiều người tham gia thích một kích cỡ phổ biến duy nhất hoặc các tình huống chỉ còn lại kích thước nhỏ hơn hoặc lớn hơn. Ví dụ: nếu tất cả áo sơ mi S và M đã hết và ai đó thích M hơn, họ sẽ nhận được L. Một cách triển khai đơn giản có thể chỉ cần chọn chiếc tiếp theo theo thứ tự mảng, điều này có thể vi phạm quy tắc "gần nhất và lớn hơn nếu hòa". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại thông qua các tùy chọn kích thước cho từng người tham gia và kiểm tra tuần tự các áo sơ mi có sẵn theo thứ tự ưu tiên. Bạn có thể xây dựng thứ tự ưu tiên một cách linh hoạt cho từng người tham gia. Ví dụ: ai đó thích L sẽ chọn L, XL, M, XXL, S theo thứ tự đó. Phương pháp này hiệu quả vì bạn được đảm bảo có áo phông cho mọi người tham gia, nhưng nó yêu cầu liên tục tạo ra chuỗi ưu tiên cho mọi người tham gia và tìm kiếm qua số lượng hàng trong kho, dẫn đến$O(K \cdot 5)$hoạt động. Với K ≤ 1000, điều này có thể chấp nhận được nhưng có thể làm rõ hơn một chút bằng cách xác định trước thứ tự ưu tiên dựa trên khoảng cách từ mỗi kích thước. 

Thông tin chi tiết quan trọng là chỉ có năm kích cỡ cố định, vì vậy, đối với mỗi người tham gia, chúng tôi có thể tính toán trước thứ tự họ sẽ lấy áo sơ mi. Điều này làm giảm bất kỳ tính toán động nào và tránh những sai sót trong quá trình hòa giải. Sau khi thứ tự ưu tiên được lưu trong từ điển, việc thực hiện yêu cầu của người tham gia là một vòng lặp đơn giản qua danh sách đó để tìm kích thước đầu tiên còn hàng trong kho. Cấu trúc của bài toán, với một chiếc áo đảm bảo cho mỗi người tham gia và một bộ kích thước cố định, làm cho phương pháp này trở nên tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(K * 5) | O(1) | Đã chấp nhận | 
| Tối ưu | O(K * 5) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ánh xạ từng kích thước theo chỉ mục từ 0 đến 4: S=0, M=1, L=2, XL=3, XXL=4. Điều này làm cho việc so sánh khoảng cách dễ dàng hơn. 
2. Lưu trữ số lượng hàng tồn trong danh sách được căn chỉnh theo ánh xạ chỉ mục. Ví dụ,`stock[2]`đại diện cho số lượng áo sơ mi chữ L có sẵn. 
3. Tính toán trước thứ tự ưu tiên cho từng kích thước dưới dạng danh sách các chỉ số. Ví dụ: đối với L (chỉ mục 2), ưu tiên là`[2, 3, 1, 4, 0]`. Điều này có được bằng cách tăng khoảng cách từ chỉ số ưu tiên và trong trường hợp hòa, hãy chọn chỉ số lớn hơn trước. 
4. Lặp lại thông qua những người tham gia theo thứ tự. Chuyển đổi từng kích thước ưa thích thành chỉ mục của nó. Sử dụng thứ tự ưu tiên được tính toán trước để tìm kích thước đầu tiên còn hàng trong kho. 
5. Sau khi chọn được kích thước, hãy giảm số lượng tồn kho của nó và ghi lại kết quả. 
6. Chuyển đổi chỉ mục cuối cùng về chuỗi kích thước và xuất ra cho mỗi người tham gia. 

Điều này hiệu quả vì ở mỗi bước, mỗi người tham gia luôn chọn chiếc áo phông gần nhất có sẵn theo quy tắc. Điều bất biến là số lượng hàng tồn kho luôn phản ánh số áo sơ mi còn lại sau mỗi người tham gia, đảm bảo rằng không có hai người tham gia nào được giao chiếc áo giống nhau và mọi ưu tiên đều tuân theo quy tắc "gần nhất và lớn hơn nếu hòa". 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

size_to_index = {'S': 0, 'M': 1, 'L': 2, 'XL': 3, 'XXL': 4}
index_to_size = ['S', 'M', 'L', 'XL', 'XXL']

# Precompute preference orders
preference_order = []
for i in range(5):
    order = [i]
    for d in range(1, 5):
        if i + d <= 4:
            order.append(i + d)
        if i - d >= 0:
            order.append(i - d)
    preference_order.append(order)

# Read stock
stock = list(map(int, input().split()))
k = int(input())

# Process participants
result = []
for _ in range(k):
    pref = input().strip()
    idx = size_to_index[pref]
    for choice in preference_order[idx]:
        if stock[choice] > 0:
            stock[choice] -= 1
            result.append(index_to_size[choice])
            break

# Output
print('\n'.join(result))
```Giải pháp xác định ánh xạ từ kích thước đến chỉ số để dễ dàng tính toán. các`preference_order`list đảm bảo tuân theo quy tắc "gần nhất và lớn hơn nếu hòa". Việc lặp lại từng người tham gia rất đơn giản vì thứ tự ưu tiên đã được tính toán trước. Cổ phiếu được giảm ngay sau khi phân công để duy trì tính chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
1 0 2 0 1
3
XL
XXL
M
```| Người tham gia | Ưu tiên | Thứ tự ưu tiên | Còn Hàng Trước | Được giao | Kho Sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | XL | [3,4,2,1,0] | [1,0,2,0,1] | XXL | [1,0,2,0,0] | 
| 2 | XXL | [4,3,2,1,0] | [1,0,2,0,0] | L | [1,0,1,0,0] | 
| 3 | M | [1,2,0,3,4] | [1,0,1,0,0] | L | [1,0,0,0,0] | 

Điều này thể hiện sự ràng buộc: khi không có XL, người tham gia sẽ chọn kích thước gần nhất tiếp theo, ưu tiên kích thước lớn hơn nhỏ hơn khi khoảng cách bằng nhau. 

Đầu vào mẫu 2:```
0 1 0 1 1
4
S
M
XL
XXL
```| Người tham gia | Ưu tiên | Được giao | 
| --- | --- | --- | 
| 1 | S | M | 
| 2 | M | M | 
| 3 | XL | XL | 
| 4 | XXL | XXL | 

Điều này cho thấy việc xử lý kích thước không có hàng tồn kho một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K * 5) | Đối với mỗi người tham gia, chúng tôi kiểm tra tối đa 5 kích cỡ theo thứ tự ưu tiên. | 
| Không gian | O(1) | Chỉ có danh sách 5 thành phần cho kho và ưu đãi, cộng với đầu ra có kích thước K. | 

Với K 1000, điều này phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    size_to_index = {'S': 0, 'M': 1, 'L': 2, 'XL': 3, 'XXL': 4}
    index_to_size = ['S', 'M', 'L', 'XL', 'XXL']

    preference_order = []
    for i in range(5):
        order = [i]
        for d in range(1, 5):
            if i + d <= 4:
                order.append(i + d)
            if i - d >= 0:
                order.append(i - d)
        preference_order.append(order)

    stock = list(map(int, input().split()))
    k = int(input())

    result = []
    for _ in range(k):
        pref = input().strip()
        idx = size_to_index[pref]
        for choice in preference_order[idx]:
            if stock[choice] > 0:
                stock[choice] -= 1
                result.append(index_to_size[choice])
                break

    return '\n'.join(result)

# Provided samples
assert run("1 0 2 0 1\n3\nXL\nXXL\nM\n") == "XXL\nL\nL", "sample 1"

# Custom cases
assert run("0 0 1 0 1\n2\nL\nXXL\n") == "L\nXXL", "custom 1: minimal stock"
assert run("5 5 5 5 5\n5\nS\nM\nL\nXL\nXXL\n") == "S\nM\nL\nXL\nXXL", "custom 2: exact stock match"
assert run("0 1 0 1 0\n2\nS\nXXL\n") == "M\nXL", "custom 3: must pick nearest available"
assert run("0 0
```
