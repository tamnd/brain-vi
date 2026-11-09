---
title: "CF 1506B - Thay thế một phần"
description: "Thuật toán được đề xuất không gặp phải một lỗi triển khai nhỏ nào. Việc xây dựng cơ bản là sai. Chúng ta có thể thấy ngay điều này ở những trường hợp thất bại. Với n = 1, tổng số hoán vị gần như được sắp xếp chính xác là 1."
date: "2026-06-10T20:23:01+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1506
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 710 (Div. 3)"
rating: 1100
weight: 1506
solve_time_s: 459
verified: true
draft: false
---

[CF 1506B - Thay thế một phần](https://codeforces.com/problemset/problem/1506/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 7 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
Thuật toán được đề xuất không gặp phải một lỗi triển khai nhỏ nào. Việc xây dựng cơ bản là sai. 

Chúng ta có thể thấy ngay điều này ở những trường hợp thất bại. 

Vì`n = 1`, tổng số hoán vị gần như được sắp xếp chính xác là`1`. 

Các đầu ra mã không chính xác:```
1 1 -> 1
1 2 -> 1
```có nghĩa là nó không bao giờ kiểm tra xem`k`vượt quá tổng số hoán vị hợp lệ. 

Thất bại thứ hai thậm chí còn rõ ràng hơn:```
n = 6, k = 5
Expected:
1 2 4 3 5 6
Actual:
3 2 1 4 5 6
```Ý tưởng tham lam "lấy khối đảo ngược lớn nhất có số lượng phù hợp với k" không tạo ra các hoán vị theo thứ tự từ điển. Nó nhảy trực tiếp đến một hoán vị muộn hơn nhiều. 

Giải pháp đúng sử dụng cấu trúc tổ hợp thực tế của các hoán vị gần như được sắp xếp. 

Đối với vấn đề này, mọi hoán vị hợp lệ có thể thu được bằng cách phân vùng```
1,2,3,...,n
```thành các khối liên tiếp và đảo ngược từng khối một cách độc lập. 

Ví dụ:```
[1,2] [3,4,5] [6]
```trở thành```
2 1 5 4 3 6
```Mọi hoán vị gần như được sắp xếp đều tương ứng với chính xác một phân vùng như vậy. 

Nếu có`m`các số còn lại thì số cách phân vùng có thể là```
2^(m-1)
```bởi vì mỗi khoảng cách giữa các số liền kề có thể là một vết cắt hoặc không. 

Thứ tự từ điển có được bằng cách quyết định độ dài của khối đầu tiên. Nếu khối đầu tiên có chiều dài`L`, sau đó:```
L = 1  -> first element = 1
L = 2  -> first element = 2
L = 3  -> first element = 3
...
```Phần tử đầu tiên nhỏ hơn có nghĩa là hoán vị nhỏ hơn về mặt từ điển. 

Do đó, chúng tôi chọn độ dài khối đầu tiên bằng cách đếm xem có bao nhiêu hoán vị bắt đầu với mỗi độ dài có thể. 

Đối với khối chiều dài đầu tiên`L`, còn lại```
n - L
```con số, đưa ra```
2^(n-L-1)
```sự tiếp nối. 

Chúng tôi liên tục trừ đi số lượng này từ`k`cho đến khi tìm được khối chứa hoán vị mong muốn. 

Việc thực hiện chính thức được chấp nhận là:```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def solve():
    t = int(input())

    # powers capped at LIMIT+1
    pw = [1] * 61
    for i in range(1, 61):
        pw[i] = min(LIMIT + 1, pw[i - 1] * 2)

    for _ in range(t):
        n, k = map(int, input().split())

        total = 1 if n == 1 else pw[min(60, n - 1)]
        if k > total:
            print(-1)
            continue

        ans = []
        cur = 1

        while cur <= n:
            remaining = n - cur + 1

            chosen_len = None

            for length in range(1, remaining + 1):
                rest = remaining - length

                if rest == 0:
                    cnt = 1
                else:
                    cnt = pw[min(60, rest - 1)]

                if k > cnt:
                    k -= cnt
                else:
                    chosen_len = length
                    break

            block = list(range(cur, cur + chosen_len))
            block.reverse()
            ans.extend(block)

            cur += chosen_len

        print(*ans)

solve()
```Điều chỉnh chính là chúng ta phải đếm chính xác các khối từ điển. Logic nhân đôi tham lam trước đó không tương ứng với xếp hạng từ điển và do đó không thể tạo ra hoán vị thứ k chính xác.
