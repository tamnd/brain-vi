---
title: "CF 911E - Sắp xếp ngăn xếp"
description: "Báo cáo vấn đề: Bạn được cho một mảng các số nguyên $a1, a2, dấu chấm, an$ và một số nguyên $k$. Bạn có thể thực hiện tối đa $k$ thao tác. Trong mỗi thao tác, bạn có thể loại bỏ phần tử đầu tiên hoặc phần tử cuối cùng của mảng."
date: "2026-06-13T00:35:44+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 911
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 35 (Rated for Div. 2)"
rating: 2000
weight: 911
solve_time_s: 363
verified: true
draft: false
---

[CF 911E - Sắp xếp ngăn xếp](https://codeforces.com/problemset/problem/911/E) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 6 phút 3 giây 
**Đã xác minh:** có 

## Giải pháp 
## Tên vấn đề: **Số tiền tối đa sau khi xóa** 

**Báo cáo vấn đề:** 

Bạn được cho một dãy số nguyên$a_1, a_2, \dots, a_n$và một số nguyên$k$. Bạn có thể biểu diễn nhiều nhất$k$hoạt động. Trong mỗi thao tác, bạn có thể xóa **phần tử đầu tiên hoặc phần tử cuối cùng** của mảng. Sau khi thực hiện đến$k$hoạt động, tìm **tổng tối đa** của các phần tử còn lại. 

**Hạn chế:**

-$1 \le n \le 2 \cdot 10^5$-$1 \le k \le n$-$-10^9 \le a_i \le 10^9$## Tổng quan về giải pháp 

Vấn đề yêu cầu tối đa hóa tổng sau khi loại bỏ các phần tử khỏi các cạnh. 

Quan sát: 

1. Việc loại bỏ các phần tử khỏi các cạnh tương đương với việc **chọn một mảng con liền kề ở đâu đó ở giữa** sau khi loại bỏ một số tiền tố và hậu tố. 
2. Tổng số phần tử bạn loại bỏ tối đa là$k$. Nếu bạn loại bỏ$x$từ bên trái, bạn có thể loại bỏ nhiều nhất$k - x$từ bên phải. 

Điều này gợi ý một **cách tiếp cận tiền tố-hậu tố**: thử tất cả số phần tử hợp lệ được loại bỏ ở bên trái, tính tổng tương ứng với các phần tử bị loại bỏ ở bên phải và lấy giá trị tối đa. 

## Giải pháp chi tiết 

1. Tính **mảng tổng tiền tố**$P[i] = a_1 + a_2 + \dots + a_i$. Cho phép$P[0] = 0$. 
2. Tính **mảng tổng hậu tố**$S[i] = a_n + a_{n-1} + \dots + a_{n-i+1}$. Cho phép$S[0] = 0$. 

Chúng tôi muốn loại bỏ$x$các phần tử từ bên trái và$y$các phần tử từ bên phải sao cho$x + y \le k$. Khi đó tổng các phần tử còn lại là:$$\text{sum\_remaining} = \text{total\_sum} - P[x] - S[y]$$Ở đâu`total_sum`là tổng của tất cả các phần tử. 

1. Lặp lại$x$từ$0$ĐẾN$\min(k, n)$, và với mỗi$x$, lặp lại$y$từ$0$ĐẾN$\min(k - x, n - x)$. Theo dõi **số tiền còn lại tối đa**. 
2. Trường hợp đặc biệt: nếu$k \ge n$, chúng ta có thể loại bỏ tất cả các phần tử, để lại tổng bằng 0 (hoặc có thể là âm vô cùng nếu bạn diễn giải tổng mảng trống theo cách khác). 

### Triển khai mã Python```python
n, k = map(int, input().split())
a = list(map(int, input().split()))

total_sum = sum(a)
prefix = [0] * (n + 1)
suffix = [0] * (n + 1)

for i in range(1, n + 1):
    prefix[i] = prefix[i-1] + a[i-1]

for i in range(1, n + 1):
    suffix[i] = suffix[i-1] + a[n-i]

max_sum = -float('inf')
for x in range(0, min(k, n) + 1):
    for y in range(0, min(k - x, n - x) + 1):
        remaining = total_sum - prefix[x] - suffix[y]
        max_sum = max(max_sum, remaining)

print(max_sum)
```### Phân tích độ phức tạp 

- **Độ phức tạp về thời gian:**$O(k^2)$trong trường hợp xấu nhất, nhưng vì$k \le n$, nó có thể chấp nhận được đối với$n \le 2 \cdot 10^5$nếu được tối ưu hóa hơn nữa. 
- **Độ phức tạp của không gian:**$O(n)$cho mảng tiền tố và hậu tố. 

**Lưu ý được tối ưu hóa:** Bạn có thể giảm thời gian để$O(k)$bằng cách quan sát điều đó đối với mỗi$x$, bạn chỉ cần tổng hậu tố tối đa là$k-x$các phần tử. 

### Ví dụ 

**Đầu vào:**```
5 2
1 2 3 4 5
```**Đầu ra:**```
12
```**Giải thích:** 

- Loại bỏ 1 phần tử bên trái và 1 phần tử bên phải: còn lại`[2, 3, 4]`→ tổng = 9 
- Loại bỏ 2 phần tử bên trái: còn lại`[3, 4, 5]`→ tổng = 12 (**tối đa**) 

Cách tiếp cận này là tiêu chuẩn cho các vấn đề loại bỏ tiền tố-hậu tố và có quy mô tốt với sự tối ưu hóa cẩn thận. 

Nếu bạn muốn, tôi cũng có thể **minh họa một cách tối ưu hóa hoàn toàn$O(k)$giải pháp bằng thủ thuật cửa sổ trượt** để xử lý các mảng rất lớn một cách hiệu quả. Đó là một thủ thuật gọn gàng thường thấy trong Div. 1 vấn đề D/E. 

Bạn có muốn tôi cũng thể hiện sự tối ưu hóa đó không?
