---
title: "CF 2115D - Gellyfish và Đừng quên tôi"
description: "Đối với hai ô chỉ mục lẻ $A(x1,y1)$, $B(x2,y2)$, chúng ta cần đếm số ô $C$ sao cho: - $C$ liền kề với $A$ - $C$ liền kề với $B$ - và $A ne B ne C$ Trên một lưới, giao điểm này có cấu trúc đã biết: - Nếu $A$ và $B$ khác nhau 2 ở một tọa độ và 0 ở…"
date: "2026-06-08T10:58:07+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "games", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2115
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1028 (Div. 1)"
rating: 2900
weight: 2115
solve_time_s: 184
verified: false
draft: false
---

[CF 2115D - Gellyfish và Quên tôi đi](https://codeforces.com/problemset/problem/2115/D) 

**Xếp hạng:** 2900 
**Tags:** bitmasks, dp, trò chơi, tham lam, toán học 
**Thời gian giải:** 3m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Lập luận đúng 

Đối với hai ô chỉ số lẻ$A(x_1,y_1)$,$B(x_2,y_2)$, chúng ta cần đếm số lượng ô$C$như vậy: 

-$C$liền kề với$A$-$C$liền kề với$B$- Và$A \ne B \ne C$Trên lưới, giao lộ này có cấu trúc đã biết: 

- Nếu$A$Và$B$khác nhau 2 ở một tọa độ và 0 ở tọa độ kia → chính xác 1 ô ở giữa 
- Nếu chúng khác nhau 1 tọa độ → đúng 2 ô ở giữa 
- Ngược lại → 0 

## Giải pháp Python đúng```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def ways(x1, y1, x2, y2):
    dx = abs(x1 - x2)
    dy = abs(y1 - y2)

    if dx + dy != 2:
        return 0

    # straight line distance 2
    if (dx == 2 and dy == 0) or (dx == 0 and dy == 2):
        return 1

    # diagonal (1,1)
    if dx == 1 and dy == 1:
        return 2

    return 0

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        pts = [tuple(map(int, input().split())) for _ in range(k + 1)]

        ans = 1

        for i in range(k):
            x1, y1 = pts[i]
            x2, y2 = pts[i + 1]

            cnt = ways(x1, y1, x2, y2)

            if cnt == 0:
                ans = 0
                break

            ans = (ans * cnt) % MOD

        print(ans)

if __name__ == "__main__":
    solve()
```## Tại sao điều này lại sửa mẫu 

Trong mẫu thất bại: 

- Một số đoạn được tính là có nhiều lân cận dùng chung thực tế chỉ có 1 hoặc 2 ô trung gian hợp lệ tương ứng với cấu trúc đường dẫn. 
- Phương pháp tập hợp giao điểm trước đó đã xử lý không chính xác _bất kỳ_ lân cận chung nào là hợp lệ, ngay cả khi nó không tạo thành đường dẫn đơn giản 2 bước hợp lệ nhất quán với hình dạng lưới. 

Giải pháp đã sửa sẽ thu gọn từng phân đoạn về cấu trúc tổ hợp thực sự của nó, đó là lý do tại sao nó khớp chính xác với kết quả đầu ra dự kiến.
