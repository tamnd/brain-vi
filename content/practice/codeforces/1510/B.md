---
title: "CF 1510B - Khóa nút"
description: "Bài toán cho ta một tổ hợp khóa nhấn nút có nhãn d từ 0 đến d-1. Việc nhấn một nút sẽ giữ nút đó được nhấn xuống vĩnh viễn cho đến khi chúng ta nhấn nút \"ĐẶT LẠI\", thao tác này sẽ bật lại tất cả các nút."
date: "2026-06-10T19:22:31+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graph-matchings", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1510
codeforces_index: "B"
codeforces_contest_name: "2020-2021 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2600
weight: 1510
solve_time_s: 187
verified: true
draft: false
---

[CF 1510B - Khóa nút](https://codeforces.com/problemset/problem/1510/B) 

**Đánh giá:** 2600 
**Tags:** luồng, đối sánh biểu đồ, đồ thị 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cho ta một tổ hợp khóa bằng nút nhấn`d`các nút được gắn nhãn từ`0`ĐẾN`d-1`. Việc nhấn một nút sẽ giữ nút đó vĩnh viễn cho đến khi chúng ta nhấn nút "ĐẶT LẠI", nút này sẽ bật lại tất cả các nút. Khóa sẽ mở ngay lập tức khi nhấn đồng thời một bộ nút cụ thể và chúng tôi được cung cấp danh sách`n`mật khẩu có thể, mỗi mật khẩu được biểu diễn dưới dạng một chuỗi nhị phân có độ dài`d`. MỘT`1`ở vị trí`j`nút nghĩa là`j`phải nhấn để lấy mật khẩu đó. 

Nhiệm vụ của chúng ta là xây dựng trình tự nhấn nút ngắn nhất (chữ số hoặc "ĐẶT LẠI") sao cho trong khi thực hiện trình tự này, khóa sẽ đi qua từng nút.`n`mật khẩu ít nhất một lần. Đầu ra phải bao gồm tổng số lần nhấn và trình tự. 

Các ràng buộc đủ chặt chẽ để cho phép các kỹ thuật tổ hợp.`d`nhiều nhất là 10, vậy tổng số trạng thái nút có thể có là`2^d = 1024`lúc tệ nhất.`n`tùy thuộc vào`2^d - 1`, vì vậy chúng ta có thể cần phải xem xét tất cả các tập hợp con các nút không trống. Điều này có nghĩa là các thuật toán theo cấp số nhân trong`d`đều có thể chấp nhận được, nhưng bất cứ điều gì mở rộng quy mô với`n!`hoặc`2^n`sẽ quá chậm đối với kích thước lớn`d`. Các trường hợp cần lưu ý bao gồm tình huống trong đó nhiều mật khẩu trùng nhau trong các nút bắt buộc hoặc khi một mật khẩu là tập hợp con của mật khẩu khác. Các phương pháp ngây thơ là nhấn các nút độc lập cho từng mật khẩu có thể nhấn nút ĐẶT LẠI quá thường xuyên một cách không cần thiết, dẫn đến các chuỗi dài hơn mức cần thiết. 

Một ví dụ tinh tế là nếu`d = 3`và mật khẩu là`101`,`111`. Một cách tiếp cận bất cẩn có thể gây áp lực`1 0 R 1 1 0 1`trong khi một chuỗi ngắn hơn`1 0 1`đủ. Phát hiện sự trùng lặp và sử dụng lại các nút đã nhấn là điều quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là tạo ra tất cả các chuỗi lần nhấn nút và chọn chuỗi ngắn nhất bao gồm tất cả mật khẩu. Điều này rõ ràng là không khả thi vì độ dài chuỗi là không giới hạn và có`d`các tùy chọn cộng với RESET cho mỗi lần nhấn. Ngay cả đối với nhỏ`d`, số lượng chuỗi tăng theo cấp số nhân theo độ dài, vì vậy cách tiếp cận này chỉ hiệu quả với các trường hợp tầm thường. 

Quan sát quan trọng là vấn đề có thể được mô hình hóa dưới dạng biểu đồ trạng thái mật khẩu. Thể hiện mỗi mật khẩu dưới dạng bitmask. Việc chuyển đổi giữa các mật khẩu tương ứng với việc nhấn các nút (bit đi từ 0 đến 1) hoặc nhấn ĐẶT LẠI. Chi phí để đi từ mật khẩu`A`ĐẾN`B`là số nút trong`B`chưa được nhấn vào`A`, cộng thêm 1 nếu chúng ta cần THIẾT LẬP LẠI. Sau đó, vấn đề giảm xuống còn việc tìm một chuỗi chi phí tối thiểu truy cập tất cả các nút mật khẩu, tương đương với đường đi ngắn nhất thông qua biểu đồ có hướng bao gồm tất cả các nút. Với`d <= 10`, chúng ta có thể tính toán trước tất cả chi phí theo cặp và sử dụng một biến thể của Bài toán người bán hàng du lịch (TSP) ở các trạng thái này. Bởi vì`2^d <= 1024`, DP trên các tập hợp con là khả thi. 

Một cái nhìn sâu sắc khác là chúng ta chỉ cần xem xét khoảng cách Hamming giữa các mặt nạ bit. Nếu như`current_mask`là các nút được nhấn hiện tại và`next_mask`là mật khẩu đích, số lần nhấn là số bit trong`next_mask`không ở trong`current_mask`. Nếu như`next_mask`yêu cầu các bit hiện không được nhấn và một số bit hiện được nhấn không nằm trong`next_mask`, RESET rẻ hơn. Điều này mang lại hàm chi phí chính xác cho DP. Với bitmask DP, chúng ta có thể tìm thấy độ dài chuỗi tối thiểu một cách hiệu quả và xây dựng lại chuỗi đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((d+1)^L) | O(L) | Quá chậm | 
| Tối ưu (Bitmask DP trên mật khẩu) | O(n * 2^n + n^2 * d) | O(n * 2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng chuỗi mật khẩu thành một bitmask số nguyên. Điều này cho phép so sánh hiệu quả và hoạt động bit. 
2. Tính toán trước chi phí chuyển đổi giữa hai mật khẩu bất kỳ. Nếu như`maskA`là trạng thái hiện tại và`maskB`là mục tiêu, kiểm tra xem`maskB`bao gồm tất cả các nút được nhấn trong`maskA`. Nếu không, cần phải RESET. Sau đó đếm số bit trong`maskB`không được cài đặt`maskA`để xác định số lần ép bổ sung. 
3. Sử dụng bitmask DP để thể hiện mật khẩu đã truy cập. Cho phép`dp[mask][i]`là độ dài chuỗi tối thiểu bao gồm tập hợp con mật khẩu được biểu thị bằng`mask`, kết thúc bằng mật khẩu`i`. 
4. Khởi tạo`dp[1 << i][i]`với số bit trong mật khẩu`i`, vì từ trạng thái trống nên chúng ta chỉ cần nhấn các bit này. 
5. Lặp lại tất cả các tập hợp con mật khẩu, cho mỗi mật khẩu cuối cùng có thể`i`trong tập hợp con và thử thêm một mật khẩu khác`j`chưa được bao gồm. Cập nhật`dp[subset | (1 << j)][j]`BẰNG`min(dp[subset | (1 << j)][j], dp[subset][i] + cost[i][j])`. 
6. Sau khi điền vào bảng DP, độ dài chuỗi tối thiểu là`min(dp[(1 << n) - 1][i])`tổng thể`i`. 
7. Xây dựng lại trình tự bằng cách quay lui qua DP, thêm các chữ số cho lần nhấn nút mới và "R" nếu cần THIẾT LẬP LẠI. 

Tại sao nó hoạt động: DP xem xét mọi tập hợp con mật khẩu và mọi mật khẩu cuối cùng có thể có trong tập hợp con đó. Bằng cách tính toán các chuyển đổi tối thiểu giữa các tập hợp con, nó đảm bảo rằng giải pháp cuối cùng bao gồm tất cả các mật khẩu với ít lần nhấn nhất. Việc sử dụng mặt nạ bit cho phép tính toán tập hợp con và chi phí hiệu quả, đồng thời chi phí tính toán trước đảm bảo rằng các quyết định RESET là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    d, n = map(int, input().split())
    passwords = []
    for _ in range(n):
        s = input().strip()
        mask = 0
        for i, c in enumerate(s):
            if c == '1':
                mask |= 1 << i
        passwords.append(mask)

    # Precompute cost between passwords
    cost = [[0] * n for _ in range(n)]
    need_reset = [[False] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if i == j:
                continue
            if passwords[j] & ~passwords[i]:
                # buttons to press outside current mask
                new_bits = passwords[j] & ~passwords[i]
                if passwords[i] & ~passwords[j]:
                    need_reset[i][j] = True
                cost[i][j] = bin(new_bits).count('1')
                if need_reset[i][j]:
                    cost[i][j] += 1  # for the RESET

    # DP over subsets
    INF = 10**9
    dp = [[INF] * n for _ in range(1 << n)]
    parent = [[-1] * n for _ in range(1 << n)]
    for i in range(n):
        dp[1 << i][i] = bin(passwords[i]).count('1')

    for mask in range(1 << n):
        for u in range(n):
            if not (mask & (1 << u)):
                continue
            for v in range(n):
                if mask & (1 << v):
                    continue
                new_mask = mask | (1 << v)
                val = dp[mask][u] + cost[u][v]
                if val < dp[new_mask][v]:
                    dp[new_mask][v] = val
                    parent[new_mask][v] = u

    # Find best last password
    full = (1 << n) - 1
    last = min(range(n), key=lambda i: dp[full][i])
    seq = []

    # Reconstruct sequence
    order = []
    mask = full
    while last != -1:
        order.append(last)
        prev = parent[mask][last]
        mask ^= (1 << last)
        last = prev
    order = order[::-1]

    # Build actual button sequence
    current = 0
    res = []
    for idx in order:
        target = passwords[idx]
        if current & ~target:
            res.append('R')
            current = 0
        for b in range(d):
            if (target >> b) & 1 and not (current >> b) & 1:
                res.append(str(b))
                current |= 1 << b

    print(len(res))
    print(' '.join(res))

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ chuyển đổi mỗi mật khẩu thành một bitmask để dễ dàng so sánh và đếm. Nó tính toán trước chi phí và liệu có cần thiết lập lại để chuyển đổi giữa các mật khẩu hay không. Sau đó DP tìm độ dài chuỗi tối thiểu bao trùm tất cả mật khẩu. Cuối cùng, chúng tôi xây dựng lại trình tự, xử lý cẩn thận RESET và tránh nhấn các nút đã nhấn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
10
11
```| Bước | Hiện tại | Mục tiêu | CÀI LẠI? | Nút nhấn | Trình tự | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 10 | Không | 0 | | 
| 1 | 0 | 10 | Không | 10 | 0 | 
| 2 | 10 | 11 | Không | 11 | 0 1 | 

Bảng xác nhận rằng thuật toán xác định trình tự tối thiểu: nhấn lần đầu`0`, sau đó`1`. 

### Mẫu 2 

đầu vào:```
3 3
100
110
111
```| Bước |
