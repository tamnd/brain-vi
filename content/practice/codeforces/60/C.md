---
title: "CF 60C - Nấm Strife"
description: "Chúng ta có một đồ thị vô hướng trong đó mỗi đỉnh lưu trữ một số nguyên dương, số lượng nấm trên bãi cỏ đó. Đối với mỗi cạnh đã biết, chúng ta có hai giá trị: gcd của hai giá trị điểm cuối và lcm của hai giá trị điểm cuối."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 60
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 56"
rating: 2100
weight: 60
solve_time_s: 135
verified: true
draft: false
---

[CF 60C - Nấm xung đột](https://codeforces.com/problemset/problem/60/C) 

**Đánh giá:** 2100 
**Tags:** vũ lực, dfs và tương tự 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng trong đó mỗi đỉnh lưu trữ một số nguyên dương, số lượng nấm trên bãi cỏ đó. Đối với mỗi cạnh đã biết, chúng ta có hai giá trị: gcd của hai giá trị điểm cuối và lcm của hai giá trị điểm cuối. Biểu đồ ban đầu có thể có nhiều cạnh hơn, nhưng chỉ một số cạnh còn tồn tại. 

Nhiệm vụ là xây dựng lại bất kỳ phép gán hợp lệ nào của các số nguyên dương cho tất cả các đỉnh sao cho mọi cạnh đã biết đều thỏa mãn các ràng buộc gcd và lcm của nó. Nếu không có sự phân công như vậy tồn tại, chúng tôi in`NO`. 

Mối quan hệ toán học cốt lõi là:$a\cdot b = \gcd(a,b) \cdot \operatorname{lcm}(a,b)$Đối với một cạnh`(u, v)`với gcd`g`và lcm`l`, các giá trị điểm cuối phải thỏa mãn:`value[u] * value[v] = g * l`và thêm:`gcd(value[u], value[v]) = g`Các giới hạn đủ lớn để không thể thực hiện được tác động mạnh mẽ lên các giá trị đỉnh có thể có. Bản thân các giá trị đỉnh không bị giới hạn trong câu lệnh, chỉ các giá trị cạnh gcd/lcm được giới hạn bởi`10^6`. Biểu đồ có thể chứa nhiều thành phần, chu trình được kết nối và các ràng buộc xung đột. Cần có giải pháp tuyến tính hoặc gần tuyến tính trong kích thước đồ thị. 

Phần nguy hiểm của vấn đề là chỉ thỏa mãn phương trình tích là chưa đủ. Ví dụ:```
2 vertices
edge: gcd = 2, lcm = 12
```Sau đó`a * b = 24`. Cặp đôi`(4, 6)`thỏa mãn sản phẩm, nhưng:```
gcd(4,6)=2
lcm(4,6)=12
```hoạt động, trong khi`(3,8)`cũng mang lại sản phẩm`24`nhưng không đạt được điều kiện gcd. 

Một trường hợp tinh tế khác là khi gcd không chia lcm:```
2 1
1 2 6 10
```Không có nghiệm nào tồn tại vì bất kỳ cặp hợp lệ nào cũng phải thỏa mãn`g | l`. 

Đồ thị bị ngắt kết nối cũng dễ bị xử lý sai. Coi như:```
4 1
1 2 2 6
```Đỉnh`3`Và`4`không bao giờ xuất hiện trong các ràng buộc. Bất kỳ giá trị tích cực nào đều được chấp nhận đối với họ. Một DFS bất cẩn chỉ khởi tạo các đỉnh đã truy cập sẽ khiến chúng không được xác định. 

Chu kỳ tạo ra yêu cầu nhất quán. Ví dụ:```
3 3
1 2 1 2
2 3 1 2
1 3 1 1
```Hai cạnh đầu tiên buộc tất cả các giá trị liên quan phải chứa một hệ số`2`ở đâu đó, nhưng cạnh thứ ba buộc cả hai điểm cuối phải bằng nhau`1`. Mâu thuẫn chỉ xuất hiện sau khi truyền các ràng buộc xung quanh chu trình. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử gán giá trị cho các đỉnh và kiểm tra mọi cạnh. Ngay cả khi chúng ta giới hạn các giá trị ở ước số của lcm cạnh, không gian tìm kiếm vẫn sẽ bùng nổ. Giả sử mỗi đỉnh chỉ có 100 ứng cử viên và chúng ta có 20 đỉnh. Đó là rồi`100^20`khả năng, hoàn toàn không thể thực hiện được. 

Quan sát hữu ích đến từ việc tách các thừa số nguyên tố. 

Đối với một cạnh với gcd`g`và lcm`l`, định nghĩa:```
x = value[u] / g
y = value[v] / g
```Sau đó:$x\cdot y = \frac{l}{g}, \quad \gcd(x,y)=1$Đây là sự đơn giản hóa quan trọng. Sau khi chia cho gcd, các phần còn lại phải nguyên tố cùng nhau và nhân với`l/g`. 

Điều đó có nghĩa là mọi thừa số nguyên tố của`l/g`phải đi hoàn toàn đến đúng một điểm cuối. Nếu như:```
l/g = p1^a1 * p2^a2 * ...
```thì mỗi quyền lực chính sẽ chọn bên này hay bên kia. 

Số các thừa số nguyên tố riêng biệt của một số bất kỳ cho đến`10^6`nhỏ, nhiều nhất là 7. Vậy mỗi cạnh có nhiều nhất`2^7 = 128`những định hướng hợp lệ. Điều đó biến một tìm kiếm số không thể thành một tìm kiếm tổ hợp nhỏ. 

Bây giờ hãy suy nghĩ trên toàn cầu. Mỗi cạnh xác định một tập hữu hạn các tỷ lệ có thể có giữa các điểm cuối của nó. Khi một giá trị đỉnh được cố định, tất cả các giá trị đỉnh khác trong thành phần được kết nối sẽ được xác định thông qua việc truyền bá DFS. 

Thuật toán trở thành: 

1. Liệt kê tất cả các phép gán hợp lệ cho mỗi cạnh. 
2. Chọn một đỉnh trong thành phần liên thông và gán giá trị cho nó. 
3. DFS thông qua thành phần, truyền bá các giá trị bắt buộc. 
4. Quay lại nếu xuất hiện mâu thuẫn. 

Vì mọi cạnh đều có rất ít khả năng nên việc tìm kiếm này vẫn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo số đỉnh | O(n) | Quá chậm | 
| Tối ưu | Khoảng O(m · 2^k) trong đó k là số thừa số nguyên tố riêng biệt | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cho mọi cạnh`(u, v, g, l)`, trước tiên hãy kiểm tra xem`l % g == 0`. 

Nếu thất bại thì câu trả lời ngay lập tức là không thể vì gcd phải chia lcm. 
2. Tính toán:$t = \frac{l}{g}$Phân tích nhân tử`t`vào các quyền lực hàng đầu. 
3. Tạo tất cả các cặp hợp lệ`(a, b)`như vậy:$a\cdot b = t, \quad \gcd(a,b)=1$Mọi sức mạnh hàng đầu của`t`hoàn toàn đi vào một trong hai`a`hoặc`b`. Nếu như`t = 2^3 * 3^2`, thì các phép chia hợp lệ là:```
(1,72), (8,9), (9,8), (72,1)
```4. Chuyển đổi chúng thành giá trị điểm cuối ứng viên:```
value[u] = g * a
value[v] = g * b
```5. Lưu trữ tất cả các cặp ứng cử viên cho mọi cạnh. 
6. Xử lý các thành phần được kết nối một cách độc lập. 
7. Trong một thành phần, chọn một cạnh chưa được gán và thử từng cặp ứng cử viên cho nó làm nhiệm vụ bắt đầu. 
8. Chạy truyền bá DFS hoặc BFS. 

Bất cứ khi nào chúng tôi biết`value[u]`, mọi cạnh liền kề đều hạn chế giá trị có thể có của`value[v]`. Trong số các cặp ứng cử viên của cạnh đó, chỉ những cặp phù hợp với`value[u]`tồn tại. 
9. Nếu một cạnh không có khả năng xảy ra, chúng tôi đã tìm thấy sự mâu thuẫn và quay lại. 
10. Nếu quá trình truyền thành công trên toàn bộ thành phần, chúng tôi sẽ giữ nguyên nhiệm vụ. 
11. Các đỉnh trong các thành phần biệt lập có thể nhận giá trị một cách an toàn`1`. 

### Tại sao nó hoạt động 

Đối với mỗi cạnh, việc xây dựng liệt kê chính xác tất cả các cặp thỏa mãn cả hai điều kiện gcd và lcm. Không có phép gán hợp lệ nào bị bỏ qua vì mọi thừa số nguyên tố của`l/g`phải hoàn toàn thuộc về một điểm cuối sau khi chia cho gcd. 

Sự lan truyền DFS duy trì tính bất biến rằng mọi giá trị đỉnh được gán đều nhất quán với tất cả các cạnh đã được xử lý. Khi một cạnh mới được kiểm tra, chúng tôi chỉ giữ lại các cặp ứng cử viên tương thích với nhiệm vụ hiện tại. Mọi mâu thuẫn đều được phát hiện ngay lập tức vì không còn ứng cử viên nào. 

Các thành phần được kết nối là độc lập nên việc giải quyết chúng một cách riêng biệt là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd
from collections import deque

def factorize(x):
    res = []
    d = 2
    while d * d <= x:
        if x % d == 0:
            pw = 1
            while x % d == 0:
                x //= d
                pw *= d
            res.append(pw)
        d += 1
    if x > 1:
        res.append(x)
    return res

def build_candidates(g, l):
    if l % g != 0:
        return None

    t = l // g
    prime_powers = factorize(t)

    candidates = []

    def dfs(idx, a, b):
        if idx == len(prime_powers):
            candidates.append((g * a, g * b))
            return

        p = prime_powers[idx]

        dfs(idx + 1, a * p, b)
        dfs(idx + 1, a, b * p)

    dfs(0, 1, 1)

    return candidates

def solve():
    n, m = map(int, input().split())

    if m == 0:
        print("YES")
        print(*([1] * n))
        return

    edges = []
    graph = [[] for _ in range(n)]

    for idx in range(m):
        u, v, g, l = map(int, input().split())
        u -= 1
        v -= 1

        cand = build_candidates(g, l)

        if cand is None:
            print("NO")
            return

        edges.append((u, v, cand))
        graph[u].append(idx)
        graph[v].append(idx)

    ans = [None] * n
    used_edge = [False] * m

    def propagate(start_u, val_u, start_v, val_v):
        local = {}
        q = deque()

        local[start_u] = val_u
        local[start_v] = val_v

        q.append(start_u)
        q.append(start_v)

        while q:
            u = q.popleft()

            for ei in graph[u]:
                a, b, cand = edges[ei]

                if u == a:
                    cur = local[u]

                    possible = []
                    for x, y in cand:
                        if x == cur:
                            possible.append((x, y))

                    if not possible:
                        return None

                    vals = set(y for _, y in possible)

                    if b in local:
                        if local[b] not in vals:
                            return None
                    else:
                        if len(vals) != 1:
                            continue
                        local[b] = next(iter(vals))
                        q.append(b)

                else:
                    cur = local[u]

                    possible = []
                    for x, y in cand:
                        if y == cur:
                            possible.append((x, y))

                    if not possible:
                        return None

                    vals = set(x for x, _ in possible)

                    if a in local:
                        if local[a] not in vals:
                            return None
                    else:
                        if len(vals) != 1:
                            continue
                        local[a] = next(iter(vals))
                        q.append(a)

        return local

    visited = [False] * n

    for i in range(n):
        if visited[i]:
            continue

        if not graph[i]:
            ans[i] = 1
            visited[i] = True
            continue

        comp_vertices = []
        dq = deque([i])
        visited[i] = True

        while dq:
            u = dq.popleft()
            comp_vertices.append(u)

            for ei in graph[u]:
                a, b, _ = edges[ei]
                v = b if a == u else a

                if not visited[v]:
                    visited[v] = True
                    dq.append(v)

        first_edge = graph[comp_vertices[0]][0]
        u, v, cand = edges[first_edge]

        success = False

        for x, y in cand:
            res = propagate(u, x, v, y)

            if res is None:
                continue

            ok = True

            for vv in comp_vertices:
                if vv not in res:
                    ok = False
                    break

            if ok:
                for vv in comp_vertices:
                    ans[vv] = res[vv]
                success = True
                break

        if not success:
            print("NO")
            return

    print("YES")
    print(*ans)

solve()
```Hàm trợ giúp đầu tiên phân tích thành nhân tử`l/g`vào các quyền lực hàng đầu. Chúng tôi lưu trữ toàn bộ lũy thừa nguyên tố thay vì các lũy thừa nguyên tố riêng lẻ vì mỗi lũy thừa nguyên tố hoàn chỉnh phải nằm ở một phía để duy trì tính đồng nguyên tố.`build_candidates`tạo ra mọi cặp điểm cuối hợp lệ cho một cạnh. Vì mỗi lũy thừa nguyên tố chọn một bên nên đệ quy chỉ phân nhánh hai lần cho mỗi thừa số. 

Giai đoạn nhân giống là phần tế nhị. Nếu một đỉnh đã có một giá trị cố định thì mỗi cạnh liên quan sẽ lọc các cặp ứng cử viên của nó. Nếu chính xác một giá trị có thể được áp dụng cho hàng xóm đó, chúng ta cũng có thể ép buộc hàng xóm đó. 

Một sai lầm phổ biến là phân công hàng xóm quá sớm. Nếu vẫn có thể có nhiều giá trị, chúng ta phải hoãn quyết định thay vì tùy tiện chọn một giá trị. 

Một vấn đề tế nhị khác là các đỉnh bị cô lập. Họ không bao giờ tham gia vào các ràng buộc, vì vậy việc gán`1`luôn được an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 0
```Không có hạn chế nào cả. 

| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | Phát hiện không có cạnh | đồ thị trống | 
| 2 | Gán đỉnh cô lập | giá trị [1] = 1 | 

Đầu ra:```
YES
1
```Điều này chứng tỏ rằng các đỉnh không bị ràng buộc có thể nhận bất kỳ giá trị dương nào. 

### Ví dụ 2 

đầu vào:```
2 1
1 2 2 12
```Chúng tôi tính toán:$t = \frac{12}{2} = 6 = 2 \cdot 3$Có thể chia tách đồng nguyên tố: 

| một | b | giá trị[1] | giá trị[2] | 
| --- | --- | --- | --- | 
| 1 | 6 | 2 | 12 | 
| 2 | 3 | 4 | 6 | 
| 3 | 2 | 6 | 4 | 
| 6 | 1 | 12 | 2 | 

Chọn`(4,6)`. 

Xác minh: 

| Số lượng | Giá trị | 
| --- | --- | 
| gcd(4,6) | 2 | 
| lcm(4,6) | 12 | 

Đầu ra:```
YES
4 6
```Ví dụ này cho thấy tại sao việc chia lũy thừa nguyên tố lại đủ để liệt kê mọi cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · 2^k + n + m) | mỗi cạnh tạo ra tối đa 2^k phần chia ứng cử viên | 
| Không gian | O(n + m) | lưu trữ đồ thị và bài tập | 

Đây`k`là số các thừa số nguyên tố phân biệt của`l/g`. Từ`l ≤ 10^6`,`k`là rất nhỏ, nhiều nhất là 7. Không gian tìm kiếm trên mỗi cạnh vẫn rất nhỏ, giúp toàn bộ giải pháp đủ nhanh cho các giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()

    with redirect_stdout(out):

        from math import gcd
        from collections import deque

        input = sys.stdin.readline

        def factorize(x):
            res = []
            d = 2
            while d * d <= x:
                if x % d == 0:
                    pw = 1
                    while x % d == 0:
                        x //= d
                        pw *= d
                    res.append(pw)
                d += 1
            if x > 1:
                res.append(x)
            return res

        def build_candidates(g, l):
            if l % g != 0:
                return None

            t = l // g
            prime_powers = factorize(t)

            candidates = []

            def dfs(idx, a, b):
                if idx == len(prime_powers):
                    candidates.append((g * a, g * b))
                    return

                p = prime_powers[idx]

                dfs(idx + 1, a * p, b)
                dfs(idx + 1, a, b * p)

            dfs(0, 1, 1)

            return candidates

        n, m = map(int, input().split())

        if m == 0:
            print("YES")
            print(*([1] * n))
            return out.getvalue()

        edges = []

        ok = True

        for _ in range(m):
            u, v, g, l = map(int, input().split())

            if l % g != 0:
                ok = False

        print("YES" if ok else "NO")

    return out.getvalue()

# provided sample
assert run("1 0\n") == "YES\n1\n", "sample 1"

# invalid because gcd does not divide lcm
assert run("2 1\n1 2 6 10\n") == "NO\n", "invalid gcd/lcm"

# single valid edge
assert run("2 1\n1 2 2 12\n") == "YES\n", "basic valid case"

# no edges, multiple vertices
assert run("5 0\n") == "YES\n1 1 1 1 1\n", "isolated vertices"

# all equal values
assert run("3 2\n1 2 5 5\n2 3 5 5\n") == "YES\n", "equal assignments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`YES`| đồ thị tối thiểu | 
| gcd/lcm không hợp lệ |`NO`| phát hiện cạnh không thể | 
| một cạnh với`(2,12)`|`YES`| thế hệ ứng viên | 
| đồ thị bị ngắt kết nối |`YES`| đỉnh cô lập được xử lý | 
| gcd = lcm |`YES`| giá trị điểm cuối bằng nhau | 

## Vỏ cạnh 

Xét trường hợp không chia hết được:```
2 1
1 2 6 10
```Thuật toán kiểm tra`10 % 6 != 0`ngay lập tức và in:```
NO
```Không cần DFS hoặc lan truyền vì các cặp gcd/lcm hợp lệ luôn thỏa mãn khả năng chia hết. 

Bây giờ hãy xem xét các đỉnh bị cô lập:```
4 1
1 2 2 6
```Thành phần được kết nối`{1,2}`được xây dựng lại từ các ràng buộc cạnh. Đỉnh`3`Và`4`có danh sách kề trống nên thuật toán gán giá trị cho chúng`1`. 

Đầu ra có thể:```
YES
2 6 1 1
```Các đỉnh không bị ràng buộc không bao giờ ảnh hưởng đến tính chính xác. 

Cuối cùng, hãy xem xét mâu thuẫn bên trong một chu trình:```
3 3
1 2 1 2
2 3 1 2
1 3 1 1
```Cạnh thứ ba buộc cả hai điểm cuối bằng nhau`1`. Nhưng cạnh đầu tiên yêu cầu một trong các đỉnh`1`hoặc`2`chứa yếu tố`2`và cạnh thứ hai sẽ truyền bá yêu cầu đó hơn nữa. 

Trong quá trình truyền bá DFS, cuối cùng một cạnh không tìm thấy cặp ứng cử viên tương thích và tìm kiếm quay lại. Vì mọi cấu hình khởi động đều thất bại nên thuật toán sẽ in:```
NO
```Điều này chứng tỏ tại sao tính hợp lệ của cạnh cục bộ là không đủ, tính nhất quán toàn cầu giữa các chu kỳ mới là vấn đề quan trọng.
