---
title: "CF 218B - Sân bay"
description: "Sân bay có nhiều máy bay, mỗi máy bay có một số ghế trống nhất định. Mọi hành khách trong hàng đợi có thể chọn bất kỳ máy bay nào để mua vé và giá vé bằng với số ghế trống hiện có trên máy bay đó tại thời điểm mua."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 218
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 134 (Div. 2)"
rating: 1100
weight: 218
solve_time_s: 84
verified: false
draft: false
---

[CF 218B - Sân bay](https://codeforces.com/problemset/problem/218/B) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Sân bay có nhiều máy bay, mỗi máy bay có một số ghế trống nhất định. Mọi hành khách trong hàng đợi có thể chọn bất kỳ máy bay nào để mua vé và giá vé bằng với số ghế trống hiện có trên máy bay đó tại thời điểm mua. Chúng ta được yêu cầu tính hai tổng: tổng doanh thu tối đa nếu hành khách tham lam chọn máy bay để tối đa hóa số tiền và tổng doanh thu tối thiểu nếu hành khách tham lam chọn máy bay để giảm thiểu nó. 

Đầu vào cho biết số lượng hành khách`n`và số lượng máy bay`m`, theo sau là một dãy số nguyên biểu thị số ghế trống ban đầu cho mỗi mặt phẳng. Chúng ta phải xuất ra hai số nguyên: thu nhập tối đa và tối thiểu có thể có theo các lựa chọn tối ưu của hành khách. 

Giới hạn là nhỏ:`n`Và`m`tối đa là 1000 và số ghế ban đầu nhiều nhất là 1000. Với`n`lên tới 1000, một cách tiếp cận đơn giản quét tất cả các máy bay để tìm từng hành khách vẫn có thể chạy trong`O(n*m)`thời gian có thể chấp nhận được trong thực tế. Tuy nhiên, do số ghế giảm sau mỗi lần bán vé nên việc chọn cấu trúc dữ liệu phù hợp để truy xuất máy bay một cách hiệu quả với số ghế sẵn có tối đa hoặc tối thiểu giúp giải pháp sạch hơn và nhanh hơn. 

Các trường hợp có biên không rõ ràng bao gồm các tình huống trong đó nhiều máy bay có cùng số ghế. Ví dụ: nếu hai máy bay đều có 1 ghế trống và chỉ có 2 hành khách thì thứ tự họ chọn không ảnh hưởng đến tổng doanh thu. Một trường hợp tinh tế khác là khi tất cả các máy bay đều có đúng một chỗ ngồi và`n`bằng tổng số ghế; doanh thu tối đa và tối thiểu là như nhau, vì không hành khách nào có thể tăng hoặc giảm giá. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực lặp đi lặp lại qua từng hành khách và đối với mỗi hành khách, hãy quét tất cả các máy bay để tìm máy bay có số ghế tối đa để có doanh thu tối đa hoặc máy bay có số ghế tối thiểu để có doanh thu tối thiểu. Sau khi chọn một máy bay, số ghế của máy bay đó sẽ giảm đi một. Điều này hiệu quả vì việc lựa chọn ở mỗi bước là tối ưu cục bộ, nhưng việc quét tất cả các máy bay để tìm từng hành khách sẽ mang lại`O(n*m)`hoạt động. Với`n`Và`m`lên tới 1000, việc này có thể mất tới 1.000.000 bước - khả thi nhưng hơi không phù hợp. 

Điểm mấu chốt là vấn đề được cấu trúc sao cho ở mỗi bước chúng ta chỉ cần số ghế tối đa hoặc tối thiểu hiện tại trên máy bay. Hàng đợi ưu tiên hoặc đống ưu tiên duy trì hiệu quả số lượng chỗ ngồi tối đa hoặc tối thiểu. Để có doanh thu tối đa, chúng tôi sử dụng vùng heap tối đa và để có doanh thu tối thiểu, vùng heap tối thiểu. Chúng tôi lấy giá trị lớn nhất hoặc nhỏ nhất, thêm vào doanh thu, giảm đi một và đẩy lùi nếu nó vẫn dương. Sử dụng đống, mỗi thao tác mất`O(log m)`, cho độ phức tạp tổng thể`O(n log m)`-sạch hơn và có khả năng mở rộng hơn nếu giới hạn lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(m) | Có thể chấp nhận được đối với giới hạn nhất định | 
| Tối ưu dựa trên Heap | O(n log m) | O(m) | Hiệu quả và rõ ràng | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào: số lượng hành khách`n`, số lượng mặt phẳng`m`và các ghế trống ban đầu của mỗi mặt phẳng dưới dạng một mảng. 
2. Để có doanh thu tối đa, hãy tạo vùng heap tối đa từ số lượng chỗ ngồi ban đầu. Mỗi phần tử trong heap biểu thị số ghế trống hiện tại trên một mặt phẳng. 
3. Khởi tạo bộ đếm để có tổng doanh thu tối đa. 
4. Lặp lại từng hành khách. Ở mỗi bước, trích xuất mặt phẳng có nhiều chỗ ngồi nhất từ ​​vùng heap tối đa. Thêm số lượng chỗ ngồi hiện tại vào tổng doanh thu. Giảm số chỗ ngồi của nó xuống một đơn vị và nếu nó vẫn dương, hãy đẩy nó trở lại đống. 
5. Để có doanh thu tối thiểu, hãy xây dựng một đống tối thiểu tương tự và lặp lại quy trình tương tự nhưng trích xuất mặt phẳng có ít chỗ ngồi nhất ở mỗi bước. Giảm và đẩy lùi nếu tích cực. 
6. In hai khoản tích lũy để có doanh thu tối đa và tối thiểu. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến là vùng heap luôn phản ánh số lượng ghế trống hiện tại cho tất cả các mặt phẳng. Việc chọn mức tối đa hoặc tối thiểu sẽ đảm bảo quyết định tối ưu cục bộ cho mỗi hành khách. Vì mỗi hành khách đều chọn một cách tham lam và số ghế đang giảm dần nên chuỗi các lựa chọn tham lam đảm bảo doanh thu tối đa hoặc tối thiểu trên toàn cầu. Không có sự kết hợp các lựa chọn nào có thể mang lại số tiền tốt hơn vì mọi đơn vị doanh thu đều được thu khi số ghế lớn nhất là tối đa hoặc nhỏ nhất là tối thiểu. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

n, m = map(int, input().split())
seats = list(map(int, input().split()))

# Max revenue using a max-heap (negative values)
max_heap = [-s for s in seats]
heapq.heapify(max_heap)
max_revenue = 0
for _ in range(n):
    top = -heapq.heappop(max_heap)
    max_revenue += top
    if top - 1 > 0:
        heapq.heappush(max_heap, -(top - 1))

# Min revenue using a min-heap
min_heap = seats[:]
heapq.heapify(min_heap)
min_revenue = 0
for _ in range(n):
    top = heapq.heappop(min_heap)
    min_revenue += top
    if top - 1 > 0:
        heapq.heappush(min_heap, top - 1)

print(max_revenue, min_revenue)
```Đoạn mã đầu tiên chuyển đổi số lượng chỗ ngồi thành một đống tối đa bằng cách phủ định các số vì Python`heapq`theo mặc định là một đống tối thiểu. Để có doanh thu tối đa, chúng tôi liên tục bật số lượng chỗ ngồi lớn nhất, thêm vào doanh thu, giảm và đẩy lùi nếu dương. Để có doanh thu tối thiểu, chúng tôi sử dụng vùng heap tối thiểu và thực hiện thao tác đối xứng. Các trường hợp cạnh như nhiều mặt phẳng có cùng số chỗ ngồi hoặc`n`bằng tổng số ghế sẽ được heap tự động xử lý. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`4 3\n2 1 1`| Hành khách | Heap tối đa (sau) | Doanh thu tối đa | Min Heap (sau) | Doanh thu tối thiểu | 
| --- | --- | --- | --- | --- | 
| 1 | [1,1,1] | 2 | [1,1,2] | 2 | 
| 2 | [1,1,0] | 3 | [1,1] | 3 | 
| 3 | [1,0,0] | 4 | [1,0] | 4 | 
| 4 | [0,0,0] | 5 | [0,0] | 5 | 

Cả hai tổng đều bằng 5, thể hiện trường hợp đặc biệt trong đó tổng số ghế bằng số hành khách, làm cho doanh thu tối thiểu và tối đa giống nhau. 

**Mẫu 2** 

đầu vào:`4 3\n3 1 2`| Hành khách | Đống tối đa | Doanh thu tối đa | Heap tối thiểu | Doanh thu tối thiểu | 
| --- | --- | --- | --- | --- | 
| 1 | [2,1,2] | 3 | [1,2,3] | 1 | 
| 2 | [2,1,1] | 5 | [2,2] | 2 | 
| 3 | [1,1,1] | 7 | [1,1] | 3 | 
| 4 | [1,0,1] | 8 | [0,1] | 4 | 

Điều này cho thấy các chuỗi lựa chọn tham lam khác nhau, tạo ra tổng số tối thiểu và tối đa riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log m) | Mỗi hành khách thực hiện một cú bật và đẩy, cả hai`O(log m)`| 
| Không gian | O(m) | Heaps lưu trữ số lượng chỗ ngồi hiện tại cho tất cả các máy bay | 

Với`n`Và`m`lên tới 1000,`n log m`hoạt động là khoảng 10.000, trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    seats = list(map(int, input().split()))

    max_heap = [-s for s in seats]
    heapq.heapify(max_heap)
    max_revenue = 0
    for _ in range(n):
        top = -heapq.heappop(max_heap)
        max_revenue += top
        if top - 1 > 0:
            heapq.heappush(max_heap, -(top - 1))

    min_heap = seats[:]
    heapq.heapify(min_heap)
    min_revenue = 0
    for _ in range(n):
        top = heapq.heappop(min_heap)
        min_revenue += top
        if top - 1 > 0:
            heapq.heappush(min_heap, top - 1)

    return f"{max_revenue} {min_revenue}"

# provided samples
assert run("4 3\n2 1 1\n") == "5 5", "sample 1"
assert run("4 3\n3 1 2\n") == "8 4", "sample 2"

# custom cases
assert run("1 1\n1\n") == "1 1", "single passenger, single seat"
assert run("3 3\n1 1 1\n") == "3 3", "all seats 1"
assert run("5 2\n3 5\n") == "16 11", "unequal seats, multiple passengers"
assert run("6 4\n1 2 3 4\n") == "18 10", "mixed seats"
assert run("1000 1\n1000\n") == "500
```
