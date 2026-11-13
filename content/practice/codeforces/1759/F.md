---
title: "CF 1759F - Tất cả các chữ số có thể"
description: "Chúng tôi có n nhiệm vụ. Nhiệm vụ tôi sẽ tặng một [i] xu bất cứ khi nào nó hoàn thành. Trong d ngày chúng tôi có thể thực hiện nhiều nhất một nhiệm vụ mỗi ngày. Sau khi thực hiện một nhiệm vụ, chúng ta phải đợi k ngày trước khi thực hiện lại nhiệm vụ đó."
date: "2026-06-09T14:34:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1759
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round  834 (Div. 3)"
rating: 1800
weight: 1759
solve_time_s: 362
verified: false
draft: false
---

[CF 1759F - Tất cả các chữ số có thể có](https://codeforces.com/problemset/problem/1759/F) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 6m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`nhiệm vụ. Nhiệm vụ`i`cho`a[i]`xu bất cứ khi nào nó được hoàn thành. 

Trong lúc`d`ngày chúng tôi có thể thực hiện tối đa một nhiệm vụ mỗi ngày. Sau khi thực hiện nhiệm vụ, chúng ta phải đợi`k`vài ngày trước khi thực hiện lại nhiệm vụ tương tự. 

Chúng tôi cần lớn nhất`k`mà có thể kiếm được ít nhất`c`đồng xu bên trong`d`ngày. 

Nếu thậm chí`k = 0`không thể với tới`c`, câu trả lời là`Impossible`. 

Nếu lớn tùy ý`k`giá trị hoạt động, câu trả lời là`Infinity`. 

Tổng của tất cả`n`và tất cả`d`nhiều nhất là`2·10^5`, vì vậy một`O(n log n + d log d)`giải pháp phong cách cho mỗi trường hợp thử nghiệm là chấp nhận được. Mô phỏng triệt để nhiều giá trị ứng viên của`k`thì không. 

## Quan sát chính 

Sắp xếp phần thưởng theo thứ tự giảm dần. 

Để có thời gian hồi chiêu cố định`k`, chiến lược tối ưu là tham lam: 

Trong bất kỳ khối nào của`k+1`ngày liên tiếp, một nhiệm vụ không thể xuất hiện hai lần. Do đó khối tốt nhất có thể bao gồm khối lớn nhất`k+1`các giá trị nhiệm vụ. 

Cho phép```
cycle = k + 1
```Giả sử chúng ta có```
full = d // cycle
rem  = d % cycle
```Mỗi chu trình hoàn chỉnh đóng góp tổng của phần lớn nhất`cycle`phần thưởng. 

Phần còn lại`rem`ngày đóng góp tổng số lớn nhất`rem`phần thưởng. 

Nếu như`cycle > n`, chúng tôi chỉ cần sử dụng tất cả các nhiệm vụ có sẵn một lần trong một chu kỳ, vì không còn nhiệm vụ riêng biệt nào nữa. 

Điều này đưa ra một cách hiệu quả để tính toán số tiền tối đa có thể đạt được cho một khoản tiền cố định.`k`. 

## Tại sao tìm kiếm nhị phân hoạt động 

Như`k`tăng lên, sự hạn chế trở nên chặt chẽ hơn. 

Số tiền tối đa có thể đạt được là đơn điệu, không tăng đối với`k`. 

Vì thế:```
can(k) = achievable_coins(k) >= c
```là một vị từ đơn điệu. 

Chúng ta có thể tìm kiếm nhị phân hợp lệ lớn nhất`k`. 

## Phát hiện vô cực 

Nếu`k`lớn tùy ý, mỗi nhiệm vụ có thể được sử dụng hiệu quả tối đa một lần trong suốt thời gian. 

Thu nhập tốt nhất có thể là```
sum of largest min(n, d) rewards
```Nếu giá trị này đã có ít nhất`c`, thì mọi thứ đủ lớn`k`hoạt động. 

Trả lời:```
Infinity
```## Phát hiện không thể 

Khi nào`k = 0`, chúng ta có thể lặp lại nhiệm vụ tốt nhất mỗi ngày. 

Thu nhập tối đa có thể trở thành```
d * max(a)
```Nếu điều này vẫn ở dưới đây`c`, đạt được mục tiêu là không thể. 

Trả lời:```
Impossible
```## Thuật toán 

1. Sắp xếp phần thưởng giảm dần. 
2. Xây dựng tổng tiền tố. 
3. Kiểm tra Vô cực. 
4. Kiểm tra Không thể. 
5. Tìm kiếm nhị phân hợp lệ lớn nhất`k`. 
6. Xuất giá trị đó. 

## Bản phác thảo chính xác 

Đối với một cố định`k`, bất kỳ nhiệm vụ nào cũng có thể xuất hiện nhiều nhất một lần trong mỗi khối có độ dài`k+1`. 

Do đó, lịch trình tối ưu liên tục nhận được phần thưởng lớn nhất hiện có trong mỗi khối như vậy. Vì phần thưởng là độc lập và không bao giờ giảm nên việc chọn phần thưởng lớn nhất sẽ tối đa hóa mọi chu kỳ một cách độc lập. 

Giá trị kết quả được tính bằng`can(k)`bằng với mức tối ưu thực sự cho thời gian hồi chiêu đó. 

Kể từ khi tăng`k`chỉ loại bỏ các lịch trình khả thi,`can(k)`là đơn điệu. Do đó tìm kiếm nhị phân tìm thấy khả năng tối đa khả thi`k`. 

Kiểm tra Vô cực đánh giá giới hạn mà nhiệm vụ không bao giờ được lặp lại. Kiểm tra Không thể đánh giá thái cực ngược lại trong đó sự lặp lại không bị hạn chế. Chúng bao gồm hai đầu ra đặc biệt. 

Do đó thuật toán luôn trả về câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, c, d = map(int, input().split())
        a = list(map(int, input().split()))

        a.sort(reverse=True)

        pref = [0]
        for x in a:
            pref.append(pref[-1] + x)

        # Infinity
        if pref[min(n, d)] >= c:
            print("Infinity")
            continue

        # Impossible
        if a[0] * d < c:
            print("Impossible")
            continue

        def can(k):
            cycle = k + 1

            take = min(cycle, n)
            cycle_sum = pref[take]

            full = d // cycle
            rem = d % cycle

            total = full * cycle_sum
            total += pref[min(rem, n)]

            return total >= c

        lo, hi = 0, d
        ans = 0

        while lo <= hi:
            mid = (lo + hi) // 2

            if can(mid):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1

        print(ans)

solve()
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | 
| --- | --- | 
| Sắp xếp |`O(n log n)`| 
| Mỗi lần kiểm tra tính khả thi |`O(1)`| 
| Tìm kiếm nhị phân |`O(log d)`| 
| Tổng số cho mỗi trường hợp thử nghiệm |`O(n log n + log d)`| 
| Không gian |`O(n)`| 

Vì tổng của`n`Và`d`trên tất cả các trường hợp thử nghiệm đều có nhiều nhất`2·10^5`, điều này dễ dàng nằm trong giới hạn.
