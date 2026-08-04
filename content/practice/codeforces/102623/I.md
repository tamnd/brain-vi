---
title: "CF 102623I - Cây Bất Tử"
description: "Chúng ta cần đếm số cây được dán nhãn trên các đỉnh 1..n thỏa mãn hai loại hạn chế. Một số cặp đỉnh buộc phải được nối bằng một cạnh. Các hạn chế khác giới hạn mức độ cuối cùng của các đỉnh cụ thể từ phía trên hoặc phía dưới."
date: "2026-08-04T17:12:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102623
codeforces_index: "I"
codeforces_contest_name: "2020 Lenovo Cup USST Campus Online Invitational Contest"
rating: 0
weight: 102623
solve_time_s: 82
verified: true
draft: false
---

[CF 102623I - Cây bất tử](https://codeforces.com/problemset/problem/102623/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm số cây được dán nhãn trên các đỉnh`1..n`thỏa mãn hai loại hạn chế. Một số cặp đỉnh buộc phải được nối bằng một cạnh. Các hạn chế khác giới hạn mức độ cuối cùng của các đỉnh cụ thể từ phía trên hoặc phía dưới. 

Câu trả lời không phải là yêu cầu chúng ta xây dựng một cây. Chúng ta cần đếm mọi ma trận kề có thể đại diện cho một cây hợp lệ, modulo`998244353`. 

Giá trị nhỏ`n <= 60`loại trừ bất kỳ cách tiếp cận nào liệt kê cây hoặc cạnh. Số cây được dán nhãn là`n^(n-2)`theo công thức Cayley, vốn đã rất lớn đối với`n = 60`. Chúng ta cần một giải pháp đa thức. 

Quan sát quan trọng là các cạnh cưỡng bức tạo thành một khu rừng trong mọi trường hợp hợp lệ. Nếu chúng chứa một chu trình thì không cây nào có thể chứa tất cả chúng. Sau khi thu gọn mọi thành phần được kết nối của khu rừng này, các lựa chọn còn lại chỉ là kết nối các thành phần này thành một cây. 

Một điểm tinh tế là các hạn chế về mức độ áp dụng cho các đỉnh ban đầu, không phải cho các thành phần thu gọn. Một đỉnh đã có một số bậc được đóng góp bởi các cạnh cưỡng bức và chỉ bậc còn lại của nó có thể được cung cấp bởi các cạnh giữa các thành phần. 

## Phương pháp tiếp cận 

Giải pháp bạo lực có thể tạo ra tất cả các cây được gắn nhãn bằng cách sử dụng trình tự Prüfer. Một cây tương ứng với một chuỗi có độ dài`n-2`, vì vậy chúng ta có thể liệt kê mọi trình tự, xây dựng lại cây và kiểm tra tất cả các ràng buộc. Điều này đúng vì mỗi cây được gắn nhãn xuất hiện đúng một lần. Tuy nhiên, có`n^(n-2)`trình tự, điều này là không thể ngay cả đối với người vừa phải`n`. 

Cấu trúc hữu ích đến từ việc thu hẹp các cạnh bắt buộc. Giả sử rừng bắt buộc có`c`các thành phần được kết nối. Bất kỳ cây cuối cùng hợp lệ nào cũng thu được bằng cách cộng chính xác`c-1`cạnh giữa các thành phần này. Sau khi co lại, mọi cặp thành phần đều có thể được kết nối và việc chọn điểm cuối bên trong một thành phần sẽ góp phần quyết định mức độ của điểm cuối đó. 

Đối với một thành phần`C`, cho phép```
S_C = sum(x_v)
```trên tất cả các đỉnh`v`bên trong nó. Công thức Cayley có trọng số nói rằng hàm sinh của cây trên các thành phần bị thu hẹp là```
(S_1 + S_2 + ... + S_c)^(c-2) * S_1 * S_2 * ... * S_c
```trong đó số mũ của`x_v`biểu thị có bao nhiêu cạnh mới rời khỏi đỉnh`v`. 

Nhiệm vụ còn lại chỉ là trích xuất các mức độ được cho phép bởi các ràng buộc. Bởi vì`n`chỉ 60, lập trình động trên tổng độ bên ngoài có thể là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force Prüfer | O(n^(n-2) * n) | O(n) | Quá chậm | 
| Rừng được khoán + chức năng phát điện DP | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem các cạnh bắt buộc có tạo thành một nhóm bằng DSU hay không. Nếu một cạnh nối hai đỉnh đã được kết nối thì câu trả lời là 0 vì mọi cây đều có chu kỳ. 
2. Với mỗi đỉnh, hãy tính bậc của nó trong rừng bắt buộc. Đây là một phần của mức độ cuối cùng đã được cố định. 
3. Chuyển đổi mọi ràng buộc độ thành một phạm vi cho số cạnh bổ sung liên quan đến đỉnh. Nếu một đỉnh đã có bậc bắt buộc`d`, thì bậc phụ của nó phải thỏa mãn:```
lower - d <= extra_degree <= upper - d
```4. Ký hợp đồng rừng bắt buộc thành từng phần. Nếu chỉ có một thành phần thì tất cả các cạnh của cây cuối cùng đã được cố định, vì vậy câu trả lời là một nếu tất cả các ràng buộc về mức độ đều khớp. 
5. Đối với mỗi thành phần, hãy tính một đa thức mô tả có bao nhiêu cách mà các đỉnh của nó có thể nhận được tổng số cạnh ngoài cho trước. Đối với một đỉnh có thể nhận được`t`các cạnh bên ngoài, sự đóng góp của nó là:```
x^t / t!
```Nhân các đa thức này bên trong một thành phần sẽ được đa thức thành phần đó. 
6. Kết hợp tất cả các thành phần. Một thành phần phải có ít nhất một cạnh đi ra trong cây được rút gọn, do đó tổng mức độ bên ngoài của nó là`g >= 1`. Công thức Cayley góp phần:```
(c-2)! * g
```để lựa chọn thành phần đóng góp. 
7. Chạy ba lô lên các bộ phận để làm cho tổng độ bên ngoài bằng`2(c-1)`, bởi vì một cái cây trên`c`các nút hợp đồng có`c-1`các cạnh và mỗi cạnh như vậy đóng góp hai độ. 

### Tại sao nó hoạt động 

Các cạnh bắt buộc xác định một khu rừng cố định. Mỗi cây hợp lệ tương ứng duy nhất với một cây giữa các thành phần được rút gọn cộng với các lựa chọn trong đó các đỉnh ban đầu là điểm cuối của các cạnh mới. 

Hàm tạo mã hóa chính xác các lựa chọn điểm cuối đó. Hệ số của đơn thức cho biết số cách phân bố độ ngoài giữa các đỉnh. Công thức Cayley đưa ra số cách để kết nối các thành phần với các bậc thành phần đó. Vì DP tính tổng tất cả các phép gán mức độ hợp lệ có thể có nên mỗi cây hợp lệ sẽ được tính một lần và những cây không hợp lệ sẽ bị loại trừ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, m, k = map(int, input().split())

    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return False
        if size[a] < size[b]:
            a, b = b, a
        parent[b] = a
        size[a] += size[b]
        return True

    forced_deg = [0] * n

    ok = True
    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        forced_deg[a] += 1
        forced_deg[b] += 1
        if not union(a, b):
            ok = False

    low = [1] * n
    high = [n - 1] * n

    for _ in range(k):
        op, x, d = map(int, input().split())
        x -= 1
        if op == 0:
            low[x] = max(low[x], d)
        else:
            high[x] = min(high[x], d)

    if not ok:
        print(0)
        return

    for i in range(n):
        low[i] -= forced_deg[i]
        high[i] -= forced_deg[i]
        if high[i] < 0:
            print(0)
            return
        low[i] = max(low[i], 0)

    roots = {}
    comp_id = []
    for i in range(n):
        r = find(i)
        if r not in roots:
            roots[r] = len(roots)
        comp_id.append(roots[r])

    c = len(roots)

    if c == 1:
        ans = 1
        for i in range(n):
            if not (low[i] <= 0 <= high[i]):
                ans = 0
        print(ans)
        return

    fact = [1] * (n + 5)
    invfact = [1] * (n + 5)
    for i in range(1, n + 5):
        fact[i] = fact[i-1] * i % MOD
    invfact[-1] = pow(fact[-1], MOD - 2, MOD)
    for i in range(n + 4, 0, -1):
        invfact[i-1] = invfact[i] * i % MOD

    comps = [[] for _ in range(c)]
    for i in range(n):
        comps[comp_id[i]].append(i)

    polys = []

    for comp in comps:
        poly = [1]
        for v in comp:
            nxt = [0] * (len(poly) + n)
            for i, a in enumerate(poly):
                if a:
                    for d in range(low[v], high[v] + 1):
                        nxt[i + d] = (nxt[i + d] + a * invfact[d]) % MOD
            poly = nxt

        val = [0] * len(poly)
        for i in range(1, len(poly)):
            val[i] = poly[i] * i % MOD
        polys.append(val)

    dp = [0] * (2 * c)
    dp[0] = 1

    for poly in polys:
        ndp = [0] * (2 * c)
        for i in range(len(dp)):
            if dp[i]:
                for j in range(1, len(poly)):
                    if i + j < len(ndp):
                        ndp[i + j] = (ndp[i + j] + dp[i] * poly[j]) % MOD
        dp = ndp

    ans = dp[2 * c - 2] * fact[c - 2] % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Phần DSU xử lý rừng bắt buộc. Một kết nối lặp lại bên trong cùng một thành phần ngay lập tức chứng tỏ là không thể thực hiện được vì một cây không thể chứa một chu trình. 

Phép biến đổi độ là phần dễ mắc sai sót nhất. Các hạn chế mô tả mức độ cuối cùng, nhưng hàm tạo chỉ tính các cạnh mới được thêm vào. Trừ đi độ rừng đã cố định sẽ chuyển bài toán thành đại lượng chính xác được biểu thị bằng đa thức. 

Nghịch đảo giai thừa xuất hiện do hệ số của khai triển đa thức chứa các phép chia theo giai thừa của số mũ đã chọn. Nhân với mức độ thành phần sau đó sẽ khôi phục phần đóng góp của Cayley cho cây đã được ký hợp đồng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Hợp nhất đa thức chiếm ưu thế, với tối đa 60 đỉnh và phạm vi độ 60 | 
| Không gian | O(n^2) | Mảng DP chỉ lưu trữ phân bố độ | 

Phạm vi mức độ tối đa là nhỏ vì mỗi cây chỉ có`n-1`các cạnh. Với`n = 60`, giới hạn bậc ba dễ dàng nằm trong giới hạn.
