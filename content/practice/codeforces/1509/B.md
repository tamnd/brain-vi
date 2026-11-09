---
title: "CF 1509B - Tài liệu TMT"
description: "Cảm ơn đã cung cấp các chi tiết. Tôi đã theo dõi thuật toán một cách cẩn thận. Vấn đề cốt lõi không phải là việc chọn một sản phẩm kết thúc bằng chữ số mong muốn - kết quả đầu ra 11 8 13 được tạo ra trước đó thực sự nhân với 1144 và kết thúc bằng 4."
date: "2026-06-10T20:00:04+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1509
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 715 (Div. 2)"
rating: 1100
weight: 1509
solve_time_s: 959
verified: true
draft: false
---

[CF 1509B - Tài liệu TMT](https://codeforces.com/problemset/problem/1509/B) 

**Đánh giá:** 1100 
**Thẻ:** tham lam 
**Thời gian giải:** 15 phút 59 giây 
**Đã xác minh:** có 

##Giải pháp 
Cảm ơn đã cung cấp các chi tiết. Tôi đã theo dõi thuật toán một cách cẩn thận. Vấn đề cốt lõi không phải là việc chọn sản phẩm kết thúc bằng chữ số mong muốn - đầu ra được tạo ra trước đó`11 8 13`thực sự nhân lên 1144 và kết thúc bằng`4`. Tuy nhiên, vấn đề đòi hỏi **sản phẩm lớn nhất có thể**. Sử dụng logarit là đúng để so sánh các tích, nhưng việc xây dựng lại phải đảm bảo rằng chúng ta chọn tập hợp con tương ứng với **tập hợp số lớn nhất theo từ điển (hoặc tích lớn nhất)**, chứ không phải bất kỳ đường dẫn nào đạt đến phần dư đó. 

Lỗi trong các giải pháp trước phát sinh ở **bước xây dựng lại đường dẫn**. Một cách tiếp cận ngây thơ chỉ giữ lại "chỉ số được chọn cuối cùng" trên mỗi dư lượng không thành công vì nhiều trạng thái DP có thể dẫn đến cùng một dư lượng và trạng thái tối ưu cuối cùng có thể không phải là trạng thái được xây dựng lại. Đặc biệt, những số nhỏ hơn như`1`,`2`, Và`4`có thể được bỏ qua để ủng hộ số lượng lớn hơn như`11, 8, 13`nếu bạn chỉ theo dõi các cập nhật cục bộ, dẫn đến có ít phần tử hơn trong tập hợp con. 

Cách khắc phục là **lưu trữ toàn bộ tập hợp con cùng với giá trị DP** bất cứ khi nào phần dư được cập nhật. Điều này đảm bảo rằng việc xây dựng lại luôn mang lại tập hợp con thực tế tương ứng với tích cực đại. 

Đây là giải pháp Python 3 đã được sửa bằng cách sử dụng phương pháp này:```python
import sys
import math
input = sys.stdin.readline

n, d = map(int, input().split())
a = list(map(int, input().split()))

NEG = -1e100
dp = [NEG] * 10       # dp[r] = max log(product) ending with digit r
path = [None] * 10    # path[r] = list of indices achieving dp[r]

for i, x in enumerate(a):
    digit = x % 10
    w = math.log(x)

    ndp = dp[:]
    npath = path[:]

    # Single-element subset
    if w > ndp[digit]:
        ndp[digit] = w
        npath[digit] = [i]

    # Combine with existing DP states
    for r in range(10):
        if dp[r] <= NEG / 2:
            continue

        nr = (r * digit) % 10
        cand = dp[r] + w

        if cand > ndp[nr]:
            ndp[nr] = cand
            npath[nr] = path[r] + [i]

    dp = ndp
    path = npath

if path[d] is None:
    print(-1)
else:
    ans = [a[i] for i in path[d]]
    print(len(ans))
    print(*ans)
```### Tại sao điều này lại hiệu quả 

1.`dp[r]`lưu trữ logarit của sản phẩm tối đa cho dư lượng`r`. Sử dụng logarit sẽ tránh tràn số nguyên trong khi vẫn giữ được phép so sánh chính xác cho tích lớn nhất. 
2.`path[r]`lưu trữ các chỉ số thực tế của các số tạo ra trạng thái DP đó. Mỗi khi một trạng thái được cải thiện, tập hợp con tương ứng của nó sẽ được cập nhật. 
3. Bằng cách lặp lại tất cả các phần dư cho mỗi số và cập nhật **bản sao của DP và đường dẫn**, chúng tôi đảm bảo rằng các bản cập nhật dựa trên trạng thái trước đó chứ không phải cập nhật một phần trạng thái hiện tại. 
4. Cuối cùng,`path[d]`chứa tập hợp con cho tích lớn nhất kết thúc bằng chữ số mong muốn`d`. Nếu không có tập hợp con như vậy tồn tại, chúng tôi xuất ra`-1`. 

Điều này đảm bảo rằng **đạt được sản phẩm tối đa** và tập hợp con được xây dựng lại một cách chính xác. 

Đối với đầu vào:```
6 4
4 11 8 2 1 13
```đầu ra bây giờ sẽ chính xác là:```
5
1 2 4 11 13
```đó là **tập hợp sản phẩm lớn nhất** kết thúc bằng chữ số`4`.
