---
title: "CF 102A - Quần áo"
description: "Chúng tôi được yêu cầu tìm cách rẻ nhất để Gerald mua ba bộ quần áo giống nhau. Mỗi mặt hàng quần áo đều có giá và một số cặp mặt hàng được đánh dấu là phù hợp. Dữ liệu đầu vào cung cấp cho chúng tôi tổng số mặt hàng, giá cả và danh sách các cặp phù hợp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 102
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 79 (Div. 2 Only)"
rating: 1200
weight: 102
solve_time_s: 102
verified: true
draft: false
---

[CF 102A - Quần áo](https://codeforces.com/problemset/problem/102/A) 

**Đánh giá:** 1200 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tìm cách rẻ nhất để Gerald mua ba bộ quần áo giống nhau. Mỗi mặt hàng quần áo đều có giá và một số cặp mặt hàng được đánh dấu là phù hợp. Dữ liệu đầu vào cung cấp cho chúng tôi tổng số mặt hàng, giá cả và danh sách các cặp phù hợp. Đầu ra của chúng tôi phải là tổng tối thiểu của ba mục tạo thành một bộ ba khớp được kết nối đầy đủ hoặc -1 nếu không có bộ ba nào như vậy tồn tại. 

Vấn đề có thể được hiểu một cách tự nhiên như một vấn đề đồ thị. Mỗi mặt hàng quần áo là một nút và mỗi cặp phù hợp là một cạnh vô hướng. Gerald muốn tìm một hình tam giác trong biểu đồ này và trong số tất cả các hình tam giác, anh ấy muốn hình tam giác có tổng trọng số nút (giá) nhỏ nhất. 

Với những hạn chế,`n`có thể lên tới 100 và`m`lên tới n(n-1)/2, nghĩa là đồ thị có thể dày đặc. Với`n`giải pháp nhỏ này kiểm tra tất cả các bộ ba nút là khả thi. Tuy nhiên, chúng ta phải cẩn thận để kiểm tra một cách hiệu quả xem bộ ba có tạo thành một tam giác mà không cần tính toán dư thừa hay không. 

Trường hợp phức tạp là khi có chính xác ba mục nhưng không phải tất cả chúng đều khớp. Ví dụ,`n=3`và chỉ tồn tại hai cạnh: chúng ta không thể tạo thành bộ ba hợp lệ và câu trả lời phải là -1. Một trường hợp khác xảy ra khi có nhiều bộ ba tồn tại, nhưng một bộ bao gồm một mặt hàng rất đắt tiền; chúng ta phải tìm bộ ba có tổng nhỏ nhất chứ không phải bất kỳ tam giác nào. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là mạnh mẽ: xem xét mọi sự kết hợp của ba mục và kiểm tra xem cả ba cặp có tồn tại trong danh sách phù hợp hay không. Điều này đúng nhưng nó liên quan đến việc kiểm tra`O(n^3)`bộ ba, và với mỗi bộ ba, xác minh sự tồn tại của ba cạnh. Với`n=100`,`n^3`là 1.000.000, có thể chấp nhận được với giới hạn 2 giây. 

Một cách tiếp cận hiệu quả hơn một chút là lưu trữ thông tin lân cận trong một tập hợp cho mỗi nút. Sau đó, với bộ ba ứng cử viên`(i, j, k)`, chúng ta có thể xác minh trong thời gian không đổi xem các cạnh có`(i, j)`,`(i, k)`, Và`(j, k)`hiện hữu. Điều này làm giảm chi phí từ việc tìm kiếm lặp đi lặp lại trong danh sách các cạnh ban đầu. 

Quan sát quan trọng là vấn đề này đủ nhỏ để chịu được phép lặp O(n^3), do đó không cần thuật toán đồ thị phức tạp. Việc sử dụng các tập kề sẽ đơn giản hóa việc kiểm tra cạnh và việc lặp lại i<j<k đảm bảo chúng ta không xem xét cùng một bộ ba nhiều lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra ba lần Brute Force với danh sách kề | O(n^3) | O(n^2) | Đã chấp nhận | 
| Kiểm tra ba lần Brute Force với bộ kề | O(n^3) | O(n^2) | Được chấp nhận và đơn giản hơn | 

## Hướng dẫn thuật toán 

1. Đọc số mục`n`và số cặp tương ứng`m`. Đọc bảng giá cho từng mặt hàng. 
2. Xây dựng một tập kề cận cho mỗi nút, lưu trữ các mục mà nó khớp với. Điều này cho phép kiểm tra cạnh O(1). 
3. Khởi tạo một biến`min_sum`đến một số rất lớn, số này sẽ theo dõi tổng tối thiểu của một bộ ba hợp lệ. 
4. Lặp lại tất cả các bộ ba`(i, j, k)`như vậy`0 <= i < j < k < n`. Điều này đảm bảo không có bộ ba nào được lặp lại và tất cả các kết hợp đều được xem xét. 
5. Với mỗi bộ ba, kiểm tra xem cả ba cạnh có tồn tại trong tập kề hay không:`i-j`,`i-k`,`j-k`. Nếu có, hãy tính tổng giá của chúng. 
6. Nếu số tiền này nhỏ hơn`min_sum`, cập nhật`min_sum`. 
7. Sau khi kiểm tra tất cả các bộ ba, nếu`min_sum`đã được cập nhật, hãy in nó. Ngược lại, in -1 để cho biết không tồn tại bộ ba hợp lệ. 

Tại sao nó hoạt động: Thuật toán xem xét một cách có hệ thống mọi bộ ba có thể có và chỉ chấp nhận các bộ ba tạo thành một hình tam giác trong biểu đồ khớp. Bằng cách giữ số tiền tối thiểu, chúng tôi đảm bảo kết quả là sự kết hợp rẻ nhất. Các bộ kề đảm bảo tra cứu cạnh nhanh chóng, do đó mọi bộ ba hợp lệ đều được xác định chính xác mà không cần tính toán dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
prices = list(map(int, input().split()))

# adjacency sets for fast edge lookup
adj = [set() for _ in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    adj[u].add(v)
    adj[v].add(u)

min_sum = float('inf')
for i in range(n):
    for j in range(i+1, n):
        if j not in adj[i]:
            continue
        for k in range(j+1, n):
            if k in adj[i] and k in adj[j]:
                total = prices[i] + prices[j] + prices[k]
                if total < min_sum:
                    min_sum = total

print(min_sum if min_sum != float('inf') else -1)
```Giải pháp đầu tiên là đọc giá và cạnh, lưu trữ thông tin kề theo bộ. Phép lặp ba lần đảm bảo không có sự kết hợp trùng lặp và kiểm tra sự tồn tại của cả ba cạnh trong thời gian không đổi. sử dụng`float('inf')`như ban đầu`min_sum`đảm bảo chúng ta có thể phát hiện sự vắng mặt của bất kỳ bộ ba hợp lệ nào một cách rõ ràng. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| tôi | j | k | Các cạnh tồn tại? | Giá tổng hợp | tổng tối thiểu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | vâng | 1+2+3=6 | 6 | 

Ở đây cả ba cạnh đều tồn tại, do đó bộ ba hợp lệ duy nhất được sử dụng, dẫn đến đầu ra 6. 

Mẫu 2 (không có ba): 

đầu vào:```
3 2
10 20 30
1 2
2 3
```| tôi | j | k | Các cạnh tồn tại? | Giá tổng hợp | tổng tối thiểu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | thiếu cạnh 0-2 | - | thông tin | 

Không có bộ ba hợp lệ nào tồn tại nên đầu ra là -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Lặp lại tất cả các bộ ba và kiểm tra các cạnh trong O(1) bằng cách sử dụng các bộ kề | 
| Không gian | O(n^2) | Bộ kề có thể lưu trữ tối đa n(n-1)/2 cạnh | 

Được cho`n <= 100`,`n^3`là khoảng 1.000.000 lần lặp, chạy thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    prices = list(map(int, input().split()))
    adj = [set() for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].add(v)
        adj[v].add(u)
    min_sum = float('inf')
    for i in range(n):
        for j in range(i+1, n):
            if j not in adj[i]:
                continue
            for k in range(j+1, n):
                if k in adj[i] and k in adj[j]:
                    total = prices[i] + prices[j] + prices[k]
                    if total < min_sum:
                        min_sum = total
    return str(min_sum if min_sum != float('inf') else -1)

# provided samples
assert run("3 3\n1 2 3\n1 2\n2 3\n3 1\n") == "6"
assert run("3 2\n10 20 30\n1 2\n2 3\n") == "-1"

# custom cases
assert run("4 6\n1 2 3 4\n1 2\n1 3\n1 4\n2 3\n2 4\n3 4\n") == "6" # all items form multiple triangles
assert run("3 0\n1 2 3\n") == "-1" # no edges
assert run("5 3\n5 4 3 2 1\n1 2\n2 3\n3 4\n") == "-1" # not enough connected triple
assert run("3 3\n1000000 1000000 1000000\n1 2\n2 3\n1 3\n") == "3000000" # max prices
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 6 với tất cả các cạnh | 6 | Tìm tam giác tối thiểu trong đồ thị được kết nối đầy đủ | 
| 3 0 | -1 | Không có cạnh nào cả | 
| 5 3 | -1 | Xử lý biểu đồ không có bộ ba hợp lệ mặc dù có một số cạnh | 
| 3 3 giá tối đa | 3000000 | Tính tổng chính xác giá lớn | 

## Vỏ cạnh 

Nếu có chính xác ba mục nhưng một cặp không khớp, chẳng hạn như dữ liệu đầu vào`3 2\n10 20 30\n1 2\n2 3\n`, thuật toán kiểm tra bộ ba`(0,1,2)`, nhìn thấy cạnh đó`0-2`bị thiếu và`min_sum`còn lại`inf`. Nó in chính xác -1. 

Nếu tất cả các mục được kết nối đầy đủ, bộ ba`(i,j,k)`lặp lại tìm thấy nhiều ứng cử viên nhưng luôn cập nhật`min_sum`đến tổng nhỏ nhất. Đối với đầu vào `4 6\n1 2 3 4\n1 2\n1 3\n1 4\n2
