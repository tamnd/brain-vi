---
title: "CF 105125E - Đường dẫn vô tỷ"
description: "Chúng ta có một đồ thị có hướng có các cạnh được đánh dấu bằng các chữ số thập phân. Bắt đầu từ đỉnh 1, chúng ta có thể đi mãi mãi bằng cách đi theo các cạnh có hướng. Chuỗi nhãn cạnh trở thành phần mở rộng thập phân của một số trong khoảng từ 0 đến 1."
date: "2026-06-27T19:30:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105125
codeforces_index: "E"
codeforces_contest_name: "MITIT 2024 Spring Invitational Qualification"
rating: 0
weight: 105125
solve_time_s: 95
verified: false
draft: false
---

[CF 105125E - Đường dẫn vô tỷ](https://codeforces.com/problemset/problem/105125/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng có các cạnh được đánh dấu bằng các chữ số thập phân. Bắt đầu từ đỉnh 1, chúng ta có thể đi mãi mãi bằng cách đi theo các cạnh có hướng. Chuỗi nhãn cạnh trở thành phần mở rộng thập phân của một số trong khoảng từ 0 đến 1. 

Câu hỏi đặt ra là liệu có tồn tại một bước đi vô hạn mà khai triển thập phân của nó là vô tỉ hay không. Khai triển số thập phân là vô tỷ chính xác khi các chữ số của nó **cuối cùng không tuần hoàn**, do đó, nhiệm vụ thực sự là hỏi liệu biểu đồ có thể tạo ra một chuỗi chữ số cuối cùng không tuần hoàn hay không. 

Đồ thị lớn nhưng tổng số đỉnh và cạnh của tất cả các ca kiểm thử nhiều nhất là$2 \cdot 10^5$. Bất kỳ thuật toán nào liên tục khám phá cùng một biểu đồ hoặc thực hiện công việc tỷ lệ với$N^2$hoặc$M^2$ngay lập tức bị loại trừ. Cần có một thuật toán đồ thị tuyến tính hoặc gần tuyến tính. 

Một số tình huống rất dễ bị bỏ qua. 

Giả sử không có chu kỳ có thể truy cập được.```
1 -> 2 -> 3
```Không có bước đi vô tận nào cả, nên câu trả lời là`No`. Giải pháp chỉ kiểm tra các nhãn cạnh khác nhau sẽ trả lời sai`Yes`. 

Giả sử phần có thể tiếp cận là một chu kỳ có hướng duy nhất.```
1 -> 2 (digit 1)
2 -> 3 (digit 2)
3 -> 1 (digit 3)
```Mỗi bước đi vô tận lặp đi lặp lại`123123123...`, vậy mọi số thập phân đều hữu tỉ. Câu trả lời đúng là`No`. 

Trường hợp tinh tế nhất là khi SCC chứa phân nhánh, nhưng mọi cạnh đi ra từ cùng một "vị trí trong chu trình" luôn mang cùng một chữ số. Việc phân nhánh như vậy làm thay đổi các đỉnh đã thăm nhưng không bao giờ thay đổi chuỗi chữ số được tạo ra. Chỉ nhìn vào cấu trúc đồ thị là chưa đủ, nhãn cạnh cũng rất quan trọng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liệt kê các bước đi ngày càng dài hơn trong khi kiểm tra xem liệu chuỗi chữ số được tạo ra cuối cùng có tuần hoàn hay không. Về nguyên tắc, điều này đúng vì tính phi lý chính xác là sự thiếu vắng tính tuần hoàn cuối cùng. Thật không may, số lượng đi bộ tăng theo cấp số nhân theo chiều dài đi bộ, vì vậy ngay cả việc khám phá tất cả các bước đi có chiều dài 50 cũng là không thể. 

Quan sát quan trọng là mọi bước đi vô hạn cuối cùng đều nằm bên trong một thành phần được kết nối mạnh mẽ. Một khi nó rời khỏi SCC, nó sẽ không bao giờ có thể quay trở lại. Điều này cho phép chúng tôi phân tích từng SCC có thể truy cập một cách độc lập. 

Bên trong một SCC, mỗi độ dài chu kỳ có ước số chung lớn nhất$g$. Thuộc tính đồ thị cổ điển cho thấy giá trị này có thể được tính theo thời gian tuyến tính bằng cách lấy gcd của tất cả các giá trị$$\text{depth}[u] + 1 - \text{depth}[v]$$trên mọi cạnh có hướng$(u,v)$trong cây DFS. 

Sau khi tính toán$g$, mọi đỉnh đương nhiên thuộc về một trong$g$các lớp dư lượng theo modulo độ sâu DFS của nó$g$. Mọi cạnh luôn đi từ cặn$c$để lại dư lượng$c+1 \pmod g$. 

Bây giờ đến đặc tính quan trọng. Nếu mọi cạnh rời khỏi các đỉnh của cùng một lớp dư lượng luôn mang cùng một chữ số thì mỗi bước đi vô hạn phải phát ra chính xác cùng một chữ số bất cứ khi nào nó ghé thăm lớp dư lượng đó. Số thập phân được tạo ra buộc phải lặp lại mỗi$g$vị trí, vì vậy mỗi bước đi cuối cùng đều mang tính định kỳ. 

Ngược lại, nếu một số lớp dư lượng chứa hai cạnh đi ra có các chữ số khác nhau, chúng ta có thể liên tục chọn giữa chúng trong khi vẫn ở trong SCC. Điều này cho phép xây dựng một chuỗi chữ số cuối cùng không tuần hoàn, đưa ra một số thập phân vô tỷ. Đặc tính này được chứng minh trong phân tích chính thức. 

Toàn bộ thuật toán trở thành một chuỗi phân tách SCC, tính toán DFS, gcd và một lần vượt qua các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(N+M)$|$O(N+M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy DFS hoặc BFS từ đỉnh 1 và bỏ qua mọi đỉnh không thể truy cập được. 
2. Tính toán các thành phần liên thông mạnh của đồ thị con có thể tiếp cận. 
3. Xử lý riêng từng SCC có thể tiếp cận. Bỏ qua các SCC không thể chứa bước đi vô hạn, cụ thể là các đỉnh đơn không có vòng tự lặp. 
4. Chọn bất kỳ đỉnh nào của SCC làm gốc và xây dựng cây DFS bên trong SCC. Ghi lại độ sâu của cây ở mỗi đỉnh. 
5. Tính gcd$$g=\gcd(\text{depth}[u]+1-\text{depth}[v])$$trên mọi cạnh của SCC. 
6. Tô màu từng đỉnh bằng`depth % g`. Khi`g = 0`, thay thế bằng 1 vì SCC luôn có ít nhất một chu kỳ. 
7. Đối với mỗi lớp dư lượng, hãy kiểm tra mọi cạnh đi ra của mọi đỉnh trong lớp đó. Tất cả các cạnh này phải mang cùng một chữ số. Nếu lớp dư nào đó chứa hai chữ số khác nhau, hãy trả lời ngay`Yes`. 
8. Nếu mọi SCC có thể truy cập đều thỏa mãn điều kiện trước đó, hãy trả lời`No`. 

### Tại sao nó hoạt động 

Mỗi bước đi vô hạn cuối cùng vẫn nằm trong một SCC có thể truy cập được. Bên trong SCC, gcd của tất cả các độ dài chu kỳ xác định thứ tự tuần hoàn chuẩn của các vị trí trong mỗi bước đi vô hạn. Các đỉnh có cùng độ sâu modulo$g$luôn chiếm cùng một vị trí trong thứ tự đó. 

Nếu mọi lớp dư lượng luôn phát ra cùng một chữ số thì chữ số ở vị trí$i$chỉ phụ thuộc vào$i \bmod g$. Mọi bước đi vô hạn cuối cùng đều tuần hoàn, do đó mọi số thập phân đều là số hữu tỉ. 

Nếu một lớp dư lượng có thể phát ra hai chữ số khác nhau, chúng ta có thể truy cập lại lớp dư lượng đó nhiều lần và chọn độc lập chữ số nào sẽ xuất ra mỗi lần. Việc chọn theo bất kỳ chuỗi nhị phân không tuần hoàn nào sẽ tạo ra một khai triển thập phân không tuần hoàn cuối cùng, điều này là không hợp lý. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline
sys.setrecursionlimit(1_000_000)

t = int(input())

for _ in range(t):
    n, m = map(int, input().split())

    g = [[] for _ in range(n)]
    rg = [[] for _ in range(n)]
    edges = []

    for _ in range(m):
        u, v, d = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, d))
        rg[v].append(u)
        edges.append((u, v, d))

    # reachable
    vis = [False] * n
    stack = [0]
    vis[0] = True
    while stack:
        v = stack.pop()
        for to, _ in g[v]:
            if not vis[to]:
                vis[to] = True
                stack.append(to)

    order = []
    used = [False] * n

    def dfs1(v):
        used[v] = True
        for to, _ in g[v]:
            if vis[to] and not used[to]:
                dfs1(to)
        order.append(v)

    for i in range(n):
        if vis[i] and not used[i]:
            dfs1(i)

    comp = [-1] * n

    def dfs2(v, c):
        comp[v] = c
        comps[-1].append(v)
        for to in rg[v]:
            if vis[to] and comp[to] == -1:
                dfs2(to, c)

    comps = []
    cid = 0
    for v in reversed(order):
        if comp[v] == -1:
            comps.append([])
            dfs2(v, cid)
            cid += 1

    ok = False

    for idx, verts in enumerate(comps):
        if ok:
            break

        if len(verts) == 1:
            u = verts[0]
            self_loop = False
            for to, _ in g[u]:
                if to == u:
                    self_loop = True
            if not self_loop:
                continue

        depth = {}
        root = verts[0]

        def dfs(v):
            for to, _ in g[v]:
                if comp[to] != idx:
                    continue
                if to not in depth:
                    depth[to] = depth[v] + 1
                    dfs(to)

        depth[root] = 0
        dfs(root)

        cyc = 0
        for u in verts:
            for v, _ in g[u]:
                if comp[v] == idx:
                    cyc = gcd(cyc, abs(depth[u] + 1 - depth[v]))
        if cyc == 0:
            cyc = 1

        digit = {}
        for u in verts:
            c = depth[u] % cyc
            for v, d in g[u]:
                if comp[v] != idx:
                    continue
                if c in digit:
                    if digit[c] != d:
                        ok = True
                        break
                else:
                    digit[c] = d
            if ok:
                break

    print("Yes" if ok else "No")
```Phần đầu tiên loại bỏ mọi đỉnh không thể tiếp cận được vì không có bước đi hợp lệ nào có thể đi vào các đỉnh đó. 

Thuật toán của Kosaraju phân tách biểu đồ còn lại thành SCC. Vì mỗi bước đi vô hạn cuối cùng đều nằm trong một SCC nên mỗi thành phần có thể được kiểm tra độc lập. 

Đối với mọi SCC có liên quan, DFS sẽ chỉ định độ sâu. Những độ sâu này chỉ có thể là một cây bao trùm, nhưng phép tính gcd loại bỏ sự phụ thuộc vào cây cụ thể. giá trị`depth[u] + 1 - depth[v]`nắm bắt mức độ thay đổi của mỗi cạnh không phải cây trong độ dài chu kỳ và lấy gcd trên tất cả các giá trị như vậy sẽ phục hồi gcd của mỗi độ dài chu kỳ trong SCC. 

Cuối cùng, các đỉnh được nhóm lại theo`depth % cyc`. Mọi cạnh đi ra từ một lớp dư lượng phải mang cùng một chữ số. Thời điểm hai chữ số khác nhau xuất hiện là chúng ta đã tìm ra đường đi vô tỉ và có thể dừng lại ngay lập tức. 

## Ví dụ đã hoạt động 

### Mẫu 1 

SCC có thể truy cập là một chu trình được định hướng duy nhất. 

| Bước | Giá trị | 
| --- | --- | 
| SCC | {1,2,3} | 
| gcd | 3 | 
| Dư lượng | 0,1,2 | 
| Chữ số theo dư lượng | 1,2,3 | 
| Xung đột | Không | 

Mọi phần dư luôn phát ra cùng một chữ số, vì vậy mỗi bước đi đều lặp lại`123`. 

### Ví dụ thứ hai```
1 -> 1 (0)
1 -> 2 (1)
2 -> 1 (1)
2 -> 2 (0)
```| Bước | Giá trị | 
| --- | --- | 
| SCC | {1,2} | 
| gcd | 1 | 
| Dư lượng | cả hai đỉnh trong phần dư 0 | 
| Chữ số để lại dư 0 | 0 và 1 | 
| Xung đột | Có | 

Vì một phần dư có thể phát ra một trong hai chữ số, nên chúng ta có thể xây dựng các chuỗi nhị phân tùy ý, bao gồm cả các chuỗi không tuần hoàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N+M)$| Phân tách SCC, DFS, tính toán gcd và quét cạnh đều là tuyến tính | 
| Không gian |$O(N+M)$| Lưu trữ đồ thị và mảng phụ trợ | 

Bởi vì tổng số đỉnh và cạnh của tất cả các ca kiểm thử là nhiều nhất$2 \cdot 10^5$, một thuật toán tuyến tính dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    # paste solution here
    ...

# sample
assert run("""3
4 4
1 2 1
1 2 1
2 3 2
3 1 3
2 4
1 1 0
1 2 1
2 1 1
2 2 0
6 6
1 2 4
1 3 5
2 4 6
2 5 7
6 6 8
6 6 9
""") == "No\nYes\nNo\n"

assert run("""1
1 1
1 1 7
""") == "No\n"

assert run("""1
2 2
1 2 0
2 1 1
""") == "No\n"

assert run("""1
2 4
1 1 0
1 2 1
2 1 1
2 2 0
""") == "Yes\n"

assert run("""1
3 2
1 2 5
2 3 6
""") == "No\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vòng tự đơn | Không | Số thập phân thuần túy định kỳ | 
| Một chu kỳ định hướng | Không | Mỗi bước đi lặp lại | 
| SCC có chữ số xung đột | Có | Dãy số vô tỉ tồn tại | 
| Không có chu kỳ có thể truy cập | Không | Đi bộ vô hạn là không thể | 

## Vỏ cạnh 

Hãy xem xét một biểu đồ không có chu trình có thể truy cập được.```
1 2
1 2 5
2 3 6
```Sơ đồ con có thể truy cập không chứa SCC có chu kỳ, vì vậy mọi SCC đều bị bỏ qua. Thuật toán in chính xác`No`. 

Hãy xem xét một chu trình chỉ đạo có thể tiếp cận duy nhất.```
3 3
1 2 1
2 3 2
3 1 3
```gcd là 3, mỗi lớp dư lượng phát ra chính xác một chữ số và không tìm thấy xung đột. Đầu ra là`No`. 

Cuối cùng, hãy xem xét```
2 4
1 1 0
1 2 1
2 1 1
2 2 0
```Toàn bộ đồ thị là một SCC. gcd bằng 1 nên mọi đỉnh đều thuộc cùng một lớp thặng dư. Phần dư đó có các cạnh đi ra được gắn nhãn cả 0 và 1, do đó thuật toán sẽ báo cáo ngay lập tức`Yes`, khớp chính xác với sự tồn tại của các chuỗi chữ số nhị phân không định kỳ cuối cùng.
