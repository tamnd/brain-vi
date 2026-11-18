---
title: "CF 2029C - Xếp hạng mới"
description: "À, bây giờ tôi thấy sai lầm tinh tế. Logic trước đó giả định rằng việc đặt mức tối đa lên đầu tiên và trừ từng phần tử khỏi mức tối đa sẽ có hiệu quả, nhưng điều đó không đúng đối với định nghĩa điểm thực tế: điểm là $$sum{i=1}^n (ci - bi)$$ trong đó $ci$ là tiền tố tối đa lên đến…"
date: "2026-06-08T12:02:48+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2029
codeforces_index: "C"
codeforces_contest_name: "Refact.ai Match 1 (Codeforces Round 985)"
rating: 1700
weight: 2029
solve_time_s: 199
verified: false
draft: false
---

[CF 2029C - Xếp hạng mới](https://codeforces.com/problemset/problem/2029/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** không 

## Giải pháp 
À, bây giờ tôi thấy sai lầm tinh tế. Logic trước đây giả định rằng việc đặt mức tối đa lên đầu tiên và trừ từng phần tử khỏi mức tối đa sẽ có hiệu quả, nhưng điều đó **không đúng** đối với định nghĩa điểm thực tế: điểm là$$\sum_{i=1}^n (c_i - b_i)$$Ở đâu$c_i$là tiền tố tối đa cho tới i và$b_i$là tiền tố tối thiểu cho đến i. Điểm mấu chốt là **mức tối thiểu của tiền tố có thể tăng** khi bạn tiếp tục nếu bạn đặt các phần tử lớn hơn sớm, vì vậy chỉ cần`max(a) - x`tổng thể`x`đếm thiếu. 

Chiến lược đúng là: **để tối đa hóa từng khác biệt tiền tố, bạn nên đặt phần tử nhỏ nhất trước, sau đó là phần tử lớn nhất, rồi nhỏ nhất tiếp theo, lớn nhất tiếp theo, xen kẽ**. Đây là thủ thuật "tổng chênh lệch tiền tố tối đa" tiêu chuẩn. 

Hãy để tôi viết một giải pháp Python 3 đang hoạt động thực hiện điều này một cách rõ ràng.```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        res = 0
        left, right = 0, n - 1
        b = []
        # Arrange in min-max alternating to maximize c_i - b_i
        while left <= right:
            if left == right:
                b.append(a[left])
            else:
                b.append(a[left])
                b.append(a[right])
            left += 1
            right -= 1
        # Compute score
        cur_min = b[0]
        cur_max = b[0]
        for x in b:
            cur_min = min(cur_min, x)
            cur_max = max(cur_max, x)
            res += cur_max - cur_min
        print(res)

if __name__ == "__main__":
    solve()
```### Tại sao điều này lại hiệu quả 

Bằng cách sắp xếp và xen kẽ nhỏ nhất và lớn nhất, chúng tôi đảm bảo rằng mỗi phần tử mới đóng góp tối đa vào tiền tố tối đa trừ đi tiền tố tối thiểu. Phần tử đầu tiên đặt cả hai`cur_min`Và`cur_max`và mỗi bước tiếp theo sẽ tăng`cur_max`hoặc tăng nhẹ`cur_min`, tối đa hóa sự khác biệt trong mỗi tiền tố. Điều này đảm bảo số tiền tối đa theo định nghĩa của bài toán. 

### Xác minh đầu vào mẫu 

đầu vào:```
3
1
69
3
7 6 5
5
1 1 1 2 2
```Đầu ra:```
0
4
4
```Điều này bây giờ khớp chính xác với đầu ra dự kiến. Cách tiếp cận trước đó bị tính thiếu vì nó chưa bao giờ xử lý hiệu ứng xen kẽ một cách chính xác.
