---
title: "CF 1439B - Bài toán tập hợp con đồ thị"
description: "À, bây giờ vấn đề đã rõ ràng. Giải pháp trước đó không triển khai logic chính xác trong thời gian tối thiểu. Mã tôi cung cấp chỉ xem xét lấy một số cố định k của các lượt đón cá nhân theo thứ tự giảm dần là b[i], đây không phải là vấn đề yêu cầu."
date: "2026-06-11T04:29:53+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1439
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 684 (Div. 1)"
rating: 2600
weight: 1439
solve_time_s: 370
verified: false
draft: false
---

[CF 1439B - Vấn đề về tập hợp con đồ thị](https://codeforces.com/problemset/problem/1439/B) 

**Đánh giá:** 2600 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, đồ thị 
**Thời gian giải:** 6 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
À, bây giờ vấn đề đã rõ ràng. Giải pháp trước đó là **không triển khai logic chính xác** trong thời gian tối thiểu. Mã tôi cung cấp chỉ được coi là lấy một số cố định`k`của xe đón cá nhân theo thứ tự giảm dần`b[i]`, đó **không phải là điều vấn đề yêu cầu**. Quyết định thực tế là **đối với từng món ăn riêng**: nhận hàng hoặc nhận hàng trực tiếp và tổng thời gian là`max(total pickup time, longest courier time)`. Sắp xếp`b`và lấy số lượng lớn nhất là chưa đủ - chúng ta phải kiểm tra **mọi sự kết hợp giữa số lượng nhận hàng và giao hàng** một cách hiệu quả. 

Cách tiếp cận đúng là: 

1. Mỗi món ăn có thể được giao tận nơi (`a[i]`) hoặc nhặt được (`b[i]`). 
2. Nếu chúng ta nhấc máy`k`cá nhân các món ăn, tổng thời gian lấy hàng là tổng của`k`đã chọn`b[i]`. 
3. Đối với các món còn lại, thời gian giao hàng tối đa là`max(a[i] for delivered dishes)`. 
4. Chúng tôi muốn **giảm thiểu mức tối đa** giữa số tiền nhận hàng và thời gian giao hàng chuyển phát nhanh tối đa. 

Điều này dẫn đến một **cách tiếp cận tham lam hiệu quả**: 

- Sắp xếp món ăn theo`b[i]`theo thứ tự giảm dần. 
- Cho phép`prefix[k]`được tổng hợp hàng đầu`k` `b[i]`giá trị (tổng thời gian lấy hàng nếu Petya chọn những giá trị này`k`món ăn cá nhân). 
- Vì`k = 0..n`, câu trả lời là`max(prefix[k], max(a))`bởi vì thời gian chuyển phát nhanh lớn nhất luôn nằm trong số`a[i]`. 
- Nhưng **chúng ta cũng phải cân nhắc rằng nếu chúng ta tự mình chọn một số món ăn thì những món còn lại được giao có thể vẫn còn lớn`a[i]`**. Vì vậy, chúng ta cần`max(max(a[i] for dishes delivered), prefix[k])`. 

Một nhận xét đơn giản và chính xác hơn nhiều là: **tổng thời gian tối thiểu là`min(max(prefix[k], max(a)))`tổng thể`k`**. Nhưng chúng ta phải sắp xếp`a`Và`b`đúng cách:

-`a_sorted`giảm dần 
-`b_sorted`giảm dần 
- Sau đó, với mỗi`k = 0..n`, lớn nhất trong số`prefix[k]`Và`a_sorted[n - k - 1]`(thời gian chuyển phát nhanh nhất trong số các món còn lại) đưa ra tổng thời gian. 

Hãy viết **giải pháp đúng**:```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        # sort both arrays descending
        a_sorted = sorted(a, reverse=True)
        b_sorted = sorted(b, reverse=True)
        
        prefix = [0] * (n + 1)
        for i in range(n):
            prefix[i + 1] = prefix[i] + b_sorted[i]
        
        ans = float('inf')
        for k in range(n + 1):
            # pick k dishes personally: sum of top k b[i]
            pickup_time = prefix[k]
            # remaining dishes delivered: largest remaining a[i]
            if k < n:
                delivery_time = a_sorted[k]
            else:
                delivery_time = 0
            ans = min(ans, max(pickup_time, delivery_time))
        print(ans)

if __name__ == "__main__":
    solve()
```### Tại sao điều này lại hiệu quả: 

- Chúng tôi xem xét **tất cả các khả năng**:`k = 0..n`món ăn tự tay gắp. 
-`prefix[k]`là tổng thời gian đón nếu Petya dẫn đầu`k` `b[i]`cá nhân. 
-`a_sorted[k]`là **thời gian giao hàng tối đa trong số các món còn lại**. Sắp xếp giảm dần đảm bảo`a_sorted[k]`là lớn nhất trong số còn lại. 
-`max(pickup_time, delivery_time)`đưa ra tổng thời gian cho sự lựa chọn đó. 
- Lấy`min`tổng thể`k`đảm bảo **sự kết hợp tối ưu**. 

### Kiểm tra đầu vào mẫu của bạn: 

đầu vào:```
4
4
3 7 4 5
2 1 2 4
4
1 2 3 4
3 3 3 3
2
1 2
10 10
2
10 10
1 2
```Đầu ra:```
5
3
2
3
```Điều này khớp chính xác với đầu ra dự kiến. 

Giải pháp này là`O(n log n)`cho mỗi trường hợp thử nghiệm, có thể chấp nhận được với các ràng buộc nhất định. 

Nếu bạn muốn, tôi có thể cung cấp **bảng theo dõi chi tiết cho trường hợp thử nghiệm đầu tiên** để hiển thị chính xác cách thực hiện`k`được chọn và tại sao câu trả lời là`5`. Điều này rất hữu ích để xác minh tính chính xác. Bạn có muốn tôi làm điều đó không?
