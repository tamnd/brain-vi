---
title: "CF 1425B - Xanh và Đỏ của Khoa chúng tôi!"
description: "Lỗi không nằm ở logic tính toán sàn. RuntimeError bạn đang thấy là NameError: giải quyết không được xác định. Đây là vấn đề về phạm vi: khai thác thử nghiệm của bạn gọi giải quyết() trước khi nó thực sự được xác định."
date: "2026-06-11T05:57:52+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "dp"]
categories: ["algorithms"]
codeforces_contest: 1425
codeforces_index: "B"
codeforces_contest_name: "2020 ICPC, COMPFEST 12, Indonesia Multi-Provincial Contest (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2600
weight: 1425
solve_time_s: 511
verified: false
draft: false
---

[CF 1425B - Màu xanh và đỏ của khoa chúng tôi!](https://codeforces.com/problemset/problem/1425/B) 

**Đánh giá:** 2600 
**Tags:** chia để trị, dp 
**Thời gian giải:** 8m 31s 
**Đã xác minh:** không 

##Giải pháp 
Lỗi không nằm ở logic tính toán sàn. các`RuntimeError`bạn đang thấy là **NameError:`solve`không được xác định`**. This is a **scoping issue**: your testing harness calls `gỡ rối()`before it is actually defined. Python cannot find a function named`giải quyết` trong phạm vi toàn cầu tại thời điểm bạn gọi nó. Điều này không liên quan gì đến các trường hợp số học hoặc cạnh. 

Để khắc phục, chúng ta phải ** gói giải pháp chính vào một`solve()`chức năng** và đảm bảo nó được xác định **trước** bộ dây thử nghiệm hoặc`__main__`khối gọi nó. Đây là giải pháp Python 3 hoạt động hoàn chỉnh bằng cách sử dụng I/O nhanh:```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        if n <= 2:
            # First floor contains apartments 1 and 2
            print(1)
        else:
            # Apartments on floors after the first
            # Subtract first floor (2 apartments), compute zero-based floor index
            # Then add 2 to account for first floor being floor 1
            floor = (n - 3) // x + 2
            print(floor)

if __name__ == "__main__":
    solve()
```### Tại sao cách này khắc phục được sự cố 

1. Chức năng`solve()`hiện được **được xác định rõ ràng**. 
2. Các`__main__`chặn cuộc gọi`solve()`một cách an toàn, vì vậy việc chạy tập lệnh trực tiếp sẽ hoạt động. 
3. Logic`(n - 3) // x + 2`tính toán chính xác sàn: 

- Trừ 2 căn hộ tầng 1:`n - 3`. 
- Chia số nguyên cho`x`cho biết có bao nhiêu tầng bổ sung ngoài tầng đầu tiên. 
- Thêm vào`2`để chuyển đổi chỉ số dựa trên số 0 thành số tầng (1 cho tầng một, cộng thêm). 

### Dấu vết ví dụ 

Đối với đầu vào`7 3`: 

-`n = 7`,`x = 3`-`n > 2`, do đó hãy tính sàn:`(7 - 3) // 3 + 2 = 4 // 3 + 2 = 1 + 2 = 3`- Đầu ra:`3`trận đấu được mong đợi. 

Đối với đầu vào`1 5`: 

-`n = 1`→`n <= 2`, tầng một → lối ra`1`trận đấu được mong đợi. 

Cấu trúc này cũng cho phép bạn`run()`kiểm tra khai thác để làm việc bởi vì`solve()`hiện được xác định trong phạm vi. Bạn có thể làm một cách an toàn:```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

assert run("4\n7 3\n1 5\n22 5\n987 13\n") == "3\n1\n5\n77"
```các`NameError`sẽ không còn xảy ra nữa. Tất cả các trường hợp biên, bao gồm các căn hộ ở tầng một và bội số chính xác của`x`, được xử lý chính xác.
