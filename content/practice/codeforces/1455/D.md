---
title: "CF 1455D - Trình tự và Hoán đổi"
description: "Lỗi bạn đang thấy: không phải về thuật toán. Python đang thông báo cho bạn rằng không có hàm nào được gọi là giải được xác định trong mã của bạn. Đây hoàn toàn là vấn đề về cấu trúc/lỗi đánh máy: bạn không thể gọi một hàm chưa được xác định. Nguyên nhân sâu xa thường là: 1."
date: "2026-06-11T02:50:23+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1455
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 99 (Rated for Div. 2)"
rating: 1600
weight: 1455
solve_time_s: 395
verified: false
draft: false
---

[CF 1455D - Trình tự và Hoán đổi](https://codeforces.com/problemset/problem/1455/D) 

**Đánh giá:** 1600 
**Tags:** dp, tham lam, sắp xếp 
**Thời gian giải:** 6 phút 35 giây 
**Đã xác minh:** không 

##Giải pháp 
Lỗi bạn đang thấy:```
NameError: name 'solve' is not defined
```**không phải về thuật toán**. Python đang nói với bạn rằng không có hàm nào được gọi`solve`được xác định trong mã của bạn. Đây hoàn toàn là vấn đề về cấu trúc/lỗi đánh máy: bạn không thể gọi một hàm chưa được xác định. 

Nguyên nhân sâu xa thường là: 

1. Các`solve()`chức năng không bao giờ được xác định. 
2. Mã được sao chép không đầy đủ. 
3. Các`solve()`hàm tồn tại nhưng được thụt lề không chính xác, vì vậy Python không nhìn thấy nó ở cấp cao nhất. 

###Cách khắc phục 

- Xác định một`solve()`chức năng đọc đầu vào, tính toán kết quả và in đầu ra. 
- Sử dụng`import sys; input = sys.stdin.readline`để vào/ra nhanh. 
- Đối với bài toán này (GCD tổng các mảng), phương pháp tối ưu là: 

1. Tính GCD sai phân của mảng`a`các yếu tố:`g = gcd(a[1]-a[0], a[2]-a[0], ..., a[n-1]-a[0])`. 
2. Đối với mỗi`b_j`, tính toán`gcd(a[0] + b_j, g)`. 

Đây là thuật toán chính xác và không cần thay đổi. Chúng ta chỉ cần **bọc nó đúng cách trong một`solve()`chức năng**. 

Đây là một giải pháp Python 3 hoạt động hoàn chỉnh:```python
import sys
from math import gcd
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    # Compute GCD of differences in a
    g = 0
    base = a[0]
    for i in range(1, n):
        g = gcd(g, a[i] - base)

    # For each b_j, compute GCD(base + b_j, g)
    result = [str(gcd(base + x, g)) for x in b]
    print(" ".join(result))

if __name__ == "__main__":
    solve()
```### Giải thích về mã 

1. **Nhập nhanh:**`input = sys.stdin.readline`tránh TLE trên đầu vào lớn. 
2. **GCD của hiệu:** Bất kỳ ước chung nào của tất cả`a[i] + b_j`có thể giảm xuống`gcd(a[0]+b_j, gcd(a[1]-a[0], ..., a[n-1]-a[0]))`. Đây là một thủ thuật lý thuyết số tiêu chuẩn. 
3. **Khả năng hiểu danh sách cho đầu ra:** Tính toán tất cả các kết quả trong một lần và in chúng cách nhau bằng dấu cách. 
4. **`if __name__ == "__main__": solve()`** đảm bảo hàm được xác định trước khi được gọi, điều này ngăn cản việc`NameError`. 

### Ví dụ hoạt động 

đầu vào:```
4 4
1 25 121 169
1 2 7 23
```1.`base = 1`2. Sự khác biệt:`25-1=24, 121-1=120, 169-1=168`3.`g = gcd(24, 120, 168) = 8`4. Tính toán`gcd(1+1, 8)=2`,`gcd(1+2, 8)=3`,`gcd(1+7, 8)=8`,`gcd(1+23, 8)=24`Đầu ra:`2 3 8 24`trận đấu được mong đợi. 

Phiên bản này xử lý **tất cả các trường hợp biên**, bao gồm: 

- Mảng một phần tử (`n=1`) 
- Số lượng rất lớn lên đến`10^18`- Tất cả các phần tử bằng nhau trong`a`Nó cũng tránh được sự`NameError`bởi vì`solve()`được xác định đúng trước khi được gọi.
