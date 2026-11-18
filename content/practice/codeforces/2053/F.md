---
title: "CF 2053F - Phần bổ sung ma trận nghiêm túc"
description: "Đối với một hàng $i$, đặt: - $zi$ = số -1 ô trong hàng đó. - $cti(u)$ = số lần xuất hiện cố định của giá trị $u$. Giả sử chúng ta quyết định rằng mọi chỗ trống trong hàng $i$ đều được điền cùng một giá trị $pi$. Đây không phải là một hạn chế."
date: "2026-06-08T08:26:49+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2053
codeforces_index: "F"
codeforces_contest_name: "Good Bye 2024: 2025 is NEAR"
rating: 2600
weight: 2053
solve_time_s: 80
verified: true
draft: false
---

[CF 2053F - Phần bổ sung ma trận nghiêm túc](https://codeforces.com/problemset/problem/2053/F) 

**Đánh giá:** 2600 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, tham lam, triển khai, toán học 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Quan sát chính 

Đối với một hàng$i$, cho phép:

-$z_i$= số lượng`-1`các ô trong hàng đó. 
-$cnt_i(u)$= số lần xuất hiện cố định của giá trị$u$. 

Giả sử chúng ta quyết định rằng mỗi ô trống trong hàng$i$được điền với cùng một giá trị$p_i$. 

Đây không phải là một hạn chế. Bởi vì biểu hiện vẻ đẹp là tuyến tính theo số lượng hàng nên bất kỳ sự phân bố tối ưu nào của$z_i$các khoảng trống có thể được di chuyển hoàn toàn sang một giá trị duy nhất mang lại hệ số lớn nhất. 

Vì vậy, mỗi hàng được biểu thị bằng một giá trị được chọn duy nhất$p_i$. 

Số giá trị cuối cùng$u$liên tiếp$i$trở thành$$c_{u,i}=cnt_i(u)+ \begin{cases} z_i,&u=p_i\\ 0,&u\neq p_i \end{cases}$$## Đóng góp của hai hàng liền kề 

Đối với hàng$i-1$Và$i$,$$\sum_u c_{u,i-1}c_{u,i}$$mở rộng đến$$C_i +z_{i-1}\,cnt_i(p_{i-1}) +z_i\,cnt_{i-1}(p_i) +z_{i-1}z_i[p_{i-1}=p_i]$$Ở đâu$$C_i=\sum_u cnt_{i-1}(u)\,cnt_i(u)$$là một hằng số độc lập với các lựa chọn. 

Định nghĩa$$dp_i(v)$$như vẻ đẹp tối đa khi xem xét hàng$1\dots i$khi hàng$i$chọn giá trị$v$. 

Sau đó$$dp_i(v) = C_i + z_i\,cnt_{i-1}(v) + \max_p \Big( dp_{i-1}(p) + z_{i-1}\,cnt_i(p) + z_{i-1}z_i[p=v] \Big)$$Cho phép$$A(p)=dp_{i-1}(p)+z_{i-1}cnt_i(p).$$Sau đó$$dp_i(v) = C_i + z_i cnt_{i-1}(v) + \max \Big( \max_p A(p), A(v)+z_{i-1}z_i \Big).$$Đây là sự đơn giản hóa quan trọng. 

## Cấu trúc dữ liệu 

Đối với mỗi lần chuyển đổi hàng, chúng tôi cần:$$A(v)$$cho tất cả$v$, Và$$\max_v A(v).$$Hầu hết các giá trị không bao giờ xuất hiện liên tiếp. Chỉ các giá trị xảy ra ở hàng hiện tại hoặc hàng trước đó mới quan trọng. 

Giải pháp tiêu chuẩn được chấp nhận lưu trữ các giá trị DP trên tất cả$1\ldots k$trong cây phân đoạn lười biếng hỗ trợ: 

- thêm phạm vi, 
- thêm điểm, 
- phạm vi chmax với giá trị toàn cầu, 
- truy vấn tối đa toàn cầu. 

Sự chuyển đổi từ hàng$i-1$chèo thuyền$i$sau đó có thể được triển khai chính xác như trong các giải pháp Codeforces được chấp nhận trong$$O((\text{distinct values in row }i)+ (\text{distinct values in row }i-1)) \log k.$$Vì tổng số ô ma trận trên tất cả các trường hợp thử nghiệm nhiều nhất là$6\cdot10^5$, độ phức tạp tổng thể là$$O((nm)\log k),$$phù hợp thoải mái trong giới hạn. 

## Giải pháp Python 3 được chấp nhận```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        m = 4 * n + 5
        self.mx = [0] * m
        self.add = [0] * m
        self.tagmax = [0] * m

    def _apply(self, p, addv, maxv):
        self.mx[p] = max(self.mx[p] + addv, maxv)
        self.tagmax[p] = max(self.tagmax[p] + addv, maxv)
        self.add[p] += addv

    def _push(self, p):
        if self.add[p] != 0 or self.tagmax[p] != 0:
            a = self.add[p]
            m = self.tagmax[p]
            self._apply(p * 2, a, m)
            self._apply(p * 2 + 1, a, m)
            self.add[p] = 0
            self.tagmax[p] = 0

    def modify(self, p, l, r, ql, qr, addv, maxv):
        if ql <= l and r <= qr:
            self._apply(p, addv, maxv)
            return

        self._push(p)
        mid = (l + r) >> 1

        if ql <= mid:
            self.modify(p * 2, l, mid, ql, qr, addv, maxv)
        if qr > mid:
            self.modify(p * 2 + 1, mid + 1, r, ql, qr, addv, maxv)

        self.mx[p] = max(self.mx[p * 2], self.mx[p * 2 + 1])

    def clear(self):
        self.mx[:] = [0] * len(self.mx)
        self.add[:] = [0] * len(self.add)
        self.tagmax[:] = [0] * len(self.tagmax)

t = int(input())

for _ in range(t):
    n, m, k = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    seg = SegTree(k)

    prev = {}

    for row in a:
        cur = {}

        for x in row:
            cur[x] = cur.get(x, 0) + 1

        z_prev = prev.get(-1, 0)
        z_cur = cur.get(-1, 0)

        for v, c in cur.items():
            if v != -1:
                seg.modify(1, 1, k, v, v, z_prev * c, 0)

        best = seg.mx[1]

        seg.modify(1, 1, k, 1, k, z_prev * z_cur, 0)
        seg.modify(1, 1, k, 1, k, 0, best)

        for v, c in cur.items():
            if v != -1:
                seg.modify(1, 1, k, 1, k, prev.get(v, 0) * c, 0)

        for v, c in prev.items():
            if v != -1:
                seg.modify(1, 1, k, v, v, z_cur * c, 0)

        prev = cur

    print(seg.mx[1])
```Đây là cách triển khai cây phân đoạn lười DP + được chấp nhận tiêu chuẩn bắt nguồn từ quá trình chuyển đổi ở trên.
