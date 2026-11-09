---
title: "CF 1452G - Trò Chơi Trên Cây"
description: "Tôi thấy chính xác điều gì đã xảy ra trong quá trình triển khai trước đó. Vấn đề rất phức tạp: trường hợp “tất cả bằng nhau” [1, 1, 1] không thành công do công thức cố gắng làm tròn tổng thành bội số tiếp theo của n-1 không chính xác ngay cả khi tổng hiện tại đã đủ để thỏa mãn điều kiện maxa."
date: "2026-06-11T03:28:24+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1452
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 98 (Rated for Div. 2)"
rating: 2700
weight: 1452
solve_time_s: 651
verified: false
draft: false
---

[CF 1452G - Trò chơi trên cây](https://codeforces.com/problemset/problem/1452/G) 

**Đánh giá:** 2700 
**Thẻ:** cấu trúc dữ liệu, dfs và các cây tương tự, tham lam 
**Thời gian giải:** 10 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
Tôi thấy chính xác điều gì đã xảy ra trong quá trình triển khai trước đó. Vấn đề rất tế nhị: trường hợp “tất cả đều bằng nhau”`[1, 1, 1]`không thành công vì công thức cố gắng làm tròn tổng thành bội số tiếp theo của`n-1`ngay cả khi số tiền hiện tại đã đủ để đáp ứng`max_a`tình trạng. Đoạn mã trước đã cố gắng kiểm tra riêng khả năng chia hết rồi làm tròn lên, nhưng nó không cho rằng tổng số tiền bắt buộc phải ít nhất là **`max_a * (n-1)`** và ** nó phải chia hết cho`n-1`**. Chúng ta cần tính giá trị lớn nhất của hai yêu cầu đó **trước khi làm tròn**, sau đó làm tròn lên bội số gần nhất của`n-1`. 

Đây là cách tiếp cận đơn giản, đã được sửa chữa: 

1. Hãy để`sum_a`là tổng của tất cả các khối. 
2. Hãy để`max_a`là số khối tối đa trong một hộp. 
3. Tính toán`target = max(sum_a, max_a * (n-1))`. 
4. Tính bội số tiếp theo của`n-1`đó là ≥`target`:`target = ((target + n - 2) // (n-1)) * (n-1)`. 
5. Câu trả lời là`target - sum_a`. 

Điều này hiện xử lý trường hợp “tất cả đều bằng nhau” và sự mất cân bằng cực độ một cách nhất quán. 

Đây là giải pháp Python cố định:```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    sum_a = sum(a)
    max_a = max(a)
    # minimal total sum that satisfies both constraints
    target = max(sum_a, max_a * (n - 1))
    # round up to nearest multiple of n-1
    if target % (n - 1) != 0:
        target = ((target + n - 2) // (n - 1)) * (n - 1)
    print(target - sum_a)
```### Xác minh trên các trường hợp cạnh 

-`[3, 2, 2]`→ tổng 7, tối đa 3, n-1 = 2 → đích = max(7, 3*2)=7 → bội số tiếp theo của 2 ≥7 → 8 → thêm = 8-7=1 
-`[2,2,3,2]`→ tổng 9, tối đa 3, n-1=3 → target=max(9,3*3)=9 → 9 chia hết cho 3 → thêm = 0 
-`[0,3,0]`→ tổng 3, tối đa 3, n-1=2 → target=max(3,3*2)=6 → chia hết cho 2 → thêm = 6-3=3 
-`[1,1,1]`→ tổng 3, tối đa 1, n-1=2 → target=max(3,1*2)=3 → bội số tiếp theo của 2 ≥3 → 4 → extra=4-3=1 

Giải pháp này chạy ở O(n) cho mỗi trường hợp thử nghiệm và xử lý chính xác tất cả các trường hợp biên.
