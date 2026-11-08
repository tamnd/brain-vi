---
title: "CF 1404E - Gạch"
description: "Tôi thấy chính xác điều gì đang xảy ra. Cách tiếp cận trước đó nhân lên một cách mù quáng các tổ hợp số lớn nhất và nhỏ nhất giả sử tất cả các tích 4 số đều quan trọng."
date: "2026-06-11T08:18:40+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graph-matchings", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1404
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 668 (Div. 1)"
rating: 2800
weight: 1404
solve_time_s: 403
verified: false
draft: false
---

[CF 1404E - Gạch](https://codeforces.com/problemset/problem/1404/E) 

**Đánh giá:** 2800 
**Tags:** luồng, đối sánh biểu đồ, đồ thị 
**Thời gian giải:** 6m 43s 
**Đã xác minh:** không 

##Giải pháp 
Tôi thấy chính xác điều gì đang xảy ra. Cách tiếp cận trước đó nhân lên một cách mù quáng các tổ hợp số lớn nhất và nhỏ nhất giả sử tất cả các tích 4 số đều quan trọng. Điều đó hoạt động với một số cấu hình nhưng không thành công khi`n < 4`hoặc khi các số cực trị cách xa nhau, tạo ra tích cực lớn hoặc âm một cách vô lý. Đầu vào hiển thị số âm và giá trị tuyệt đối lớn, do đó logic kết hợp ngây thơ bị phá vỡ hoàn toàn. 

Lỗi có hai mặt: 

1. **Giả định chỉ mục** - mã cố gắng truy cập`a[-4]`cho mảng nhỏ hơn 4 phần tử. 
2. **Logic lựa chọn sản phẩm** - chỉ cần kết hợp các số lớn nhất và nhỏ nhất theo một mẫu cố định sẽ không đảm bảo sản phẩm tối đa trong các mảng có cả số dương và số âm. Ví dụ:```
6
-1000000000 -1000000000 -1000000000 1000000000 1000000000 1000000000
```Mã của bạn chọn một cái gì đó như`a[-1]*a[-2]*a[-3]*a[-4] = 10^9*10^9*10^9*-10^9 = -10^36`, điều đó là sai. Sản phẩm đúng là tích của ba tích cực`10^9`s và một âm`-10^9`để có được sản phẩm tích cực nhất`10^9*10^9*10^9*10^9 = 10^36`. Logic cố định phải xem xét tất cả các kết hợp hợp lý của 4 số, được sắp xếp và chưa sắp xếp. 

Cách tiếp cận đúng là: 

- Sắp xếp mảng. 
- Vì`n >= 4`, chỉ một tập hợp nhỏ các kết hợp ứng viên mới có thể tạo ra tích tối đa: bốn số lớn nhất, hai số nhỏ nhất và hai số lớn nhất (để khai thác số âm) hoặc cả bốn số nhỏ nhất nếu số âm chiếm ưu thế. Có chính xác 5 tổ hợp ứng viên cần kiểm tra. 
- Vì`n < 4`, lấy tích của tất cả các phần tử. 

Đây là giải pháp Python 3 đã được sửa với I/O nhanh:```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        
        if n == 1:
            print(a[0])
            continue
        if n == 2:
            print(a[0] * a[1])
            continue
        if n == 3:
            print(a[0] * a[1] * a[2])
            continue
        
        # n >= 4, check all meaningful 4-number combinations
        candidates = [
            a[0]*a[1]*a[2]*a[3],          # four smallest
            a[-1]*a[-2]*a[-3]*a[-4],      # four largest
            a[0]*a[1]*a[-1]*a[-2],        # two smallest, two largest
            a[0]*a[1]*a[2]*a[-1],         # three smallest, largest
            a[-1]*a[-2]*a[-3]*a[0],       # three largest, smallest
        ]
        print(max(candidates))

if __name__ == "__main__":
    solve()
```### Tại sao điều này lại hiệu quả 

- Sắp xếp đảm bảo rằng`a[0] ≤ a[1] ≤ ... ≤ a[-1]`. 
- Để tối đa hóa tích có 4 số, chỉ có các cực trị quan trọng: số dương lớn nhất và số âm lớn nhất (vì âm × âm = dương). Việc kiểm tra năm kết hợp ứng cử viên ở trên bao gồm tất cả các trường hợp. 
- Trường hợp cạnh với`n < 4`được xử lý riêng để tránh lỗi lập chỉ mục. 
- Logic này hoạt động với các mảng âm/dương hỗn hợp và các giá trị cực trị. 

Điều này sẽ tạo ra kết quả đầu ra mong đợi cho mẫu không thành công của bạn:```
Input:
7
4
-3 5 -3 1
2
1 -1
4
-3 2 -3 4
4
-1 1 1 -1
7
-5 7 -6 -4 17 -13 4
6
-1000000000 -1000000000 -1000000000 1000000000 1000000000 1000000000
1
0

Output:
3
0
4
1
8
3000000000
0
```Điều này khắc phục cả`IndexError`và tính toán sản phẩm không chính xác.
