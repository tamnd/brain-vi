---
title: "CF 1663F - Trong Mọi Thế Hệ..."
description: "Thực tế có hai vấn đề riêng biệt. Lỗi thời gian chạy ngay lập tức hoàn toàn không phải do thuật toán gây ra. Bộ khai thác kiểm tra gọi: và bên trong run nó gọi: nhưng đoạn mã hiển thị trong bài xã luận không bao gồm giải quyết bên trong khối kiểm tra."
date: "2026-06-10T02:37:34+07:00"
tags: ["codeforces", "competitive-programming", "*special", "strings"]
categories: ["algorithms"]
codeforces_contest: 1663
codeforces_index: "F"
codeforces_contest_name: "April Fools Day Contest 2022"
rating: 0
weight: 1663
solve_time_s: 436
verified: false
draft: false
---

[CF 1663F - Trong mọi thế hệ...](https://codeforces.com/problemset/problem/1663/F) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, dây 
**Thời gian giải:** 7 phút 16 giây 
**Đã xác minh:** không 

##Giải pháp 
Thực tế có hai vấn đề riêng biệt. 

Lỗi thời gian chạy ngay lập tức hoàn toàn không phải do thuật toán gây ra. Khai thác thử nghiệm gọi:```
assert run(...)
```và bên trong`run`nó gọi:```
solve()
```nhưng đoạn mã hiển thị trong bài xã luận không bao gồm`solve`bên trong khối kiểm tra. Điều đó tạo ra một`NameError`(bị cắt bớt trong phần truy ngược mà bạn đã dán). 

Tuy nhiên, ngay cả sau khi sửa lỗi đó, bản thân thuật toán vẫn sai đối với Codeforces 1665C. 

Đối với mẫu đầu tiên:```
7
1 1 1 2 2 4
```số trẻ em là:```
[3, 2, 1]
```Công thức của bài xã luận tính:```
max(3+1, 2+2, 1+3) = 4
```điều này xảy ra phù hợp với mẫu. 

Nhưng điều này là ngẫu nhiên. Hãy xem xét một ngôi sao:```
6
1 1 1 1 1
```Số lượng trẻ em:```
[5]
```Công thức cho:```
5 + 1 = 6
```trong khi câu trả lời đúng là:```
4
```đã xuất hiện trong mẫu chính thức. 

Nguyên nhân là quá trình lây nhiễm không chỉ đơn giản là “số con lớn nhất + vị trí”. Sau khi các nguồn lây nhiễm bắt đầu được tiêm lần đầu, tất cả các bố mẹ đang hoạt động tiếp tục lây lan đồng thời. Công việc còn lại sau giai đoạn đầu tiên phải được mô hình hóa một cách rõ ràng. 

Giải pháp được chấp nhận bởi hầu hết các thí sinh là: 

1. Đếm số lượng nút con của mỗi nút. 
2. Chỉ giữ số lượng dương. 
3. Thêm một số đếm bổ sung cho gốc. 
4. Sắp xếp giảm dần. 
5. Hãy để`m`là số lượng đếm. 
6. Sau`m`giây, mỗi nhóm đã nhận được một cơ hội tiêm và cũng đang giảm dần một cách tự nhiên. 
7. Tính số tiền còn lại chưa hoàn thành. 
8. Tìm kiếm nhị phân cần thêm bao nhiêu giây. 

Đây là giải pháp biên tập tiêu chuẩn. 

Việc triển khai đúng được chấp nhận là:```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        cnt = [0] * (n + 1)
        for x in p:
            cnt[x] += 1

        a = [c for c in cnt if c > 0]
        a.append(1)  # root

        a.sort(reverse=True)

        m = len(a)

        rem = []
        for i, x in enumerate(a):
            v = x - (m - i)
            if v > 0:
                rem.append(v)

        rem.sort(reverse=True)

        if not rem:
            print(m)
            continue

        lo, hi = 0, n

        while lo < hi:
            mid = (lo + hi) // 2

            produced = 0
            for x in rem:
                if x > mid:
                    produced += x - mid

            if produced <= mid:
                hi = mid
            else:
                lo = mid + 1

        print(m + lo)

if __name__ == "__main__":
    solve()
```Cái này chạy vào`O(n log n)`cho mỗi trường hợp thử nghiệm tổng thể và là giải pháp tiêu chuẩn được chấp nhận cho Codeforces 1665C.
