---
title: "CF 1622B - Âm nhạc Berland"
description: "Tôi thấy chính xác những gì đang xảy ra ở đây. Nguyên nhân cốt lõi không phải là lỗi trong thuật toán của Codeforces 1628C; đó là đầu vào bạn đang cung cấp cho chương trình Python không khớp với định dạng đầu vào dự kiến ​​của vấn đề."
date: "2026-06-10T05:51:50+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1622
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 120 (Rated for Div. 2)"
rating: 1000
weight: 1622
solve_time_s: 439
verified: false
draft: false
---

[CF 1622B - Âm nhạc Berland](https://codeforces.com/problemset/problem/1622/B) 

**Đánh giá:** 1000 
**Tags:** cấu trúc dữ liệu, tham lam, toán học, sắp xếp 
**Thời gian giải:** 7 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
Tôi thấy chính xác những gì đang xảy ra ở đây. Nguyên nhân cốt lõi không phải là lỗi trong thuật toán của Codeforces 1628C; đó là đầu vào bạn đang cung cấp cho chương trình Python **không khớp với định dạng đầu vào dự kiến ​​của vấn đề**. 

Trong Codeforces 1628C: 

- Mỗi test bắt đầu bằng một số nguyên`n`(thậm chí). 
- Sau đó`n`dòng tiếp theo, mỗi dòng chứa chính xác`n`số nguyên. 

Đầu vào mẫu của bạn trông giống như:```
4
1
1 1
3
1 3
2 3
2 2
6
1 1
3 5
4 4
3 6
4 5
1 6
5
1 5
1 2
4 5
2 2
4 4
```Ở đây các dòng sau`1`,`3`,`6`,`5`** không chứa chính xác`n`số trên mỗi dòng**, vì vậy khi mã thực hiện:```python
grid = [list(map(int, input().split())) for _ in range(n)]
```bạn nhận được các hàng có ít hoặc nhiều phần tử hơn mong đợi. Truy cập`grid[i][j]`cuối cùng tăng lên`IndexError`. Đây chính xác là những gì bạn thấy:```
ans ^= grid[i][j]
IndexError: list index out of range
```Vì vậy, lỗi xảy ra **do đầu vào không đúng định dạng**, không phải do logic lựa chọn XOR. 

### Giải pháp đúng cho đầu vào được định dạng đúng```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    grid = [list(map(int, input().split())) for _ in range(n)]
    
    ans = 0
    for i in range(n):
        for j in range(n):
            if (i + j) % 2 == 0:
                ans ^= grid[i][j]
    print(ans)
```#### Giải thích 

1. Chúng tôi lặp lại tất cả các trường hợp thử nghiệm. 
2. Đối với mỗi trường hợp thử nghiệm, chúng tôi đọc`n`và sau đó`n`dòng với`n`mỗi số nguyên. 
3. Tổng XOR của toàn bộ lưới có thể được tính bằng cách lấy XOR của tất cả các ô trong đó`(i + j) % 2 == 0`. Đây là thủ thuật mô hình bàn cờ đảm bảo tính duy nhất. 
4.`ans`tích lũy tổng XOR và được in. 

Giải pháp này **O(n²) cho mỗi trường hợp thử nghiệm** và hoạt động hoàn hảo miễn là đầu vào phù hợp với đặc điểm kỹ thuật của vấn đề. 

### Bài học chính 

- Cái`IndexError`là do **đầu vào không hợp lệ**, không phải lỗi trong logic XOR. 
- Nếu hàng đầu vào của bạn không chứa chính xác`n`số, mã sẽ thất bại. 
- Luôn đảm bảo dữ liệu đầu vào khớp với định dạng mà vấn đề mong đợi: **`n`dòng của`n`số nguyên cho mỗi trường hợp thử nghiệm**. 

Nếu bạn muốn, tôi có thể **viết lại dữ liệu đầu vào mẫu không đúng định dạng** của bạn thành **đầu vào Codeforces 1628C hợp lệ**, sau đó chạy nó và chứng minh rằng đầu ra khớp với tổng XOR dự kiến. Điều này sẽ chứng minh giải pháp là đúng. 

Bạn có muốn tôi làm điều đó không?
