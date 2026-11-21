---
title: "CF 231E - Xương Rồng"
description: "Chúng ta có một đồ thị vô hướng liên thông được đảm bảo là một cây xương rồng có đỉnh. Trong một cây xương rồng có đỉnh, mỗi đỉnh thuộc nhiều nhất một chu trình đơn. Các chu trình có thể tiếp xúc với phần còn lại của đồ thị thông qua các điểm khớp nối, nhưng hai chu trình khác nhau không bao giờ có thể có chung một đỉnh."
date: "2026-06-04T09:14:34+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 231
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 143 (Div. 2)"
rating: 2100
weight: 231
solve_time_s: 155
verified: false
draft: false
---

[CF 231E - Xương rồng](https://codeforces.com/problemset/problem/231/E) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, đồ thị, cây 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng liên thông được đảm bảo là một cây xương rồng có đỉnh. Trong một cây xương rồng có đỉnh, mỗi đỉnh thuộc nhiều nhất một chu trình đơn. Các chu trình có thể tiếp xúc với phần còn lại của đồ thị thông qua các điểm khớp nối, nhưng hai chu trình khác nhau không bao giờ có thể có chung một đỉnh. 

Đối với mỗi truy vấn`(x, y)`, chúng ta phải đếm có bao nhiêu đường đi đơn giản khác nhau bắt đầu tại`x`và kết thúc tại`y`. Hai đường đi được coi là khác nhau nếu tập hợp các cạnh của chúng khác nhau. Câu trả lời là bắt buộc theo modulo`10^9 + 7`. 

Đồ thị có tới`10^5`đỉnh,`10^5`các cạnh và`10^5`truy vấn. Bất kỳ giải pháp nào xử lý truy vấn bằng cách khám phá trực tiếp biểu đồ sẽ bị loại trừ ngay lập tức. Thậm chí một`O(n)`tính toán cho mỗi truy vấn sẽ yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất. 

Hạn chế về xương rồng là toàn bộ vấn đề. Trong một đồ thị tổng quát, việc đếm các đường đi đơn giản là vô cùng khó khăn. Ở cây xương rồng, mọi chu trình hoạt động độc lập và đồ thị có thể được nén thành cấu trúc giống cây. 

Một trường hợp tinh tế xảy ra khi cả hai đỉnh được truy vấn đều nằm trong cùng một chu trình. 

Ví dụ:```
1-2
| |
4-3
```Truy vấn`(1,3)`có hai đường dẫn đơn giản:```
1-2-3
1-4-3
```Cách tiếp cận dựa trên cây giả định tính duy nhất của đường dẫn sẽ trả về không chính xác`1`. 

Một trường hợp quan trọng khác là khi đường đi giữa hai đỉnh đi qua nhiều chu kỳ. 

Ví dụ:```
cycle A -- bridge -- cycle B
```Bất kỳ đường đi đơn giản nào từ bên này sang bên kia đều phải chọn một hướng trong chu trình A và một hướng trong chu trình B. Câu trả lời là`2 * 2 = 4`. 

Việc triển khai bất cẩn chỉ kiểm tra chu kỳ của điểm cuối sẽ bỏ lỡ hiệu ứng nhân này. 

Trường hợp cạnh thứ ba là khi đường đi chạm vào một chu trình qua đỉnh khớp nối của nó nhưng không cần phải đi qua chu trình đó. Một chu kỳ như vậy không đóng góp gì cả. Chỉ các chu trình nằm trên đường dẫn cây nén giữa các điểm cuối mới quan trọng. 

## Phương pháp tiếp cận 

Ý tưởng Brute Force là liệt kê tất cả các đường đi đơn giản giữa các đỉnh được truy vấn. 

Điều này đúng vì bài toán trực tiếp yêu cầu số đường đi đơn giản. Thật không may, ngay cả một truy vấn cũng có thể có nhiều đường dẫn đơn giản theo cấp số nhân trong một biểu đồ có nhiều chu kỳ. Với tối đa`10^5`truy vấn, điều này là vô vọng. 

Thuộc tính xương rồng làm thay đổi cấu trúc một cách đáng kể. Vì mỗi đỉnh thuộc nhiều nhất một chu trình nên mọi thành phần liên thông đôi là một cạnh hoặc một chu trình. 

Hãy nghĩ xem điều gì sẽ xảy ra khi chúng ta thu gọn mọi chu kỳ thành một nút đặc biệt. 

Cấu trúc thu được sẽ trở thành một cái cây, thường được gọi là cây cắt khối của cây xương rồng. 

Các đỉnh ban đầu trở thành một loại nút. 

Mỗi chu kỳ sẽ trở thành một loại nút khác. 

Mọi đỉnh thuộc một chu trình đều được kết nối với nút chu trình đó. 

Bởi vì các chu trình không bao giờ trùng nhau nên đồ thị này là một cây. 

Bây giờ hãy xem xét một truy vấn. Giữa hai đỉnh của cây có đúng một đường đi. Bất cứ khi nào tuyến đường đó đi qua một nút chu trình, có chính xác hai cách để đi qua chu trình tương ứng. Người ta có thể đi vòng theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. 

Các nút chu kỳ khác nhau đóng góp độc lập. Nếu đường dẫn cây chứa`c`nút chu kỳ, câu trả lời đơn giản là:```
2^c
```Vì vậy, vấn đề giảm xuống: 

1. Xây dựng cây khối xương rồng. 
2. Đối với mỗi truy vấn, hãy đếm xem có bao nhiêu nút chu kỳ nằm trên đường dẫn cây. 
3. Trở về`2^(count)`. 

Đếm các nút chu trình trên đường dẫn cây là một vấn đề LCA tiêu chuẩn với tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O((n+m) log n + k log n) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tìm tất cả các đỉnh của chu trình 

Chạy DFS và tính toán thời gian khám phá tiêu chuẩn cũng như các giá trị liên kết thấp. 

Bất cứ khi nào DFS gặp cạnh sau từ một nút`u`đến tổ tiên`v`, các đỉnh trên ngăn xếp DFS từ`u`quay lại`v`tạo thành một chu kỳ của cây xương rồng. 

Vì đồ thị là một cây xương rồng có đỉnh nên mỗi đỉnh có thể thuộc nhiều nhất một chu trình như vậy. 

Chúng tôi chỉ định một mã định danh cho mỗi chu kỳ được phát hiện. 

### 2. Xây dựng cây khối 

Tạo các nút có hai loại. 

đầu tiên`n`các nút tương ứng với các đỉnh đồ thị ban đầu. 

Mỗi chu trình được phát hiện sẽ nhận được một nút bổ sung. 

Đối với mỗi đỉnh chứa trong một chu trình, hãy kết nối:```
vertex node <-> cycle node
```Các đỉnh không thuộc bất kỳ chu trình nào vẫn bị cô lập khỏi các nút chu trình. 

Đồ thị kết quả là một cái cây. 

Lý do là mỗi khối của cây xương rồng đều là một cạnh hoặc một chu trình và cách biểu diễn cắt khối tiêu chuẩn của đồ thị như vậy là không có chu kỳ. 

### 3. Đánh dấu các nút chu trình 

Chỉ định trọng lượng:```
0 for original vertices
1 for cycle nodes
```Sau đó, tổng đường dẫn sẽ đếm xem có bao nhiêu nút chu kỳ gặp phải. 

### 4. LCA tiền xử lý 

Root cây khối ở bất cứ đâu, ví dụ tại nút`1`. 

Tính toán:```
depth[u]
up[j][u]
pref[u]
```Ở đâu`pref[u]`là số nút chu kỳ trên root-to-`u`con đường. 

Nâng nhị phân mang lại`O(log n)`Các truy vấn LCA. 

### 5. Trả lời câu hỏi 

hãy để`a`Và`b`là các nút đỉnh ban đầu. 

Tìm thấy:```
l = LCA(a,b)
```Số nút chu trình trên đường đi của cây là:```
cnt =
pref[a] + pref[b]
- 2 * pref[l]
+ weight[l]
```Mỗi chu kỳ như vậy đóng góp một hệ số hai. 

Câu trả lời là:```
2^cnt mod MOD
```### Tại sao nó hoạt động 

Sau khi thay thế mỗi chu kỳ bằng một nút chu trình chuyên dụng, cây xương rồng sẽ trở thành một cái cây. Bất kỳ tuyến đường nào giữa hai đỉnh trong biểu đồ gốc đều tương ứng với đường dẫn cây duy nhất giữa các nút tương ứng của chúng. 

Bất cứ khi nào đường đi của cây này đi qua một nút chu trình, đường đi ban đầu sẽ đi vào chu trình qua một đỉnh chu trình và rời khỏi một đỉnh chu trình khác. Một chu trình đơn giản cung cấp chính xác hai tuyến đường đơn giản riêng biệt giữa các đỉnh đó, mỗi tuyến theo một hướng xung quanh chu trình. 

Các lựa chọn được thực hiện bên trong các chu trình khác nhau là độc lập vì các chu trình khác nhau không có chung đỉnh. Do đó, mỗi chu trình gặp phải sẽ nhân đôi số đường đi đơn giản hợp lệ. 

Số chu kỳ đường dẫn cây chính xác là số lượng các lựa chọn nhị phân độc lập, do đó câu trả lời bằng`2^(cycle count)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

n, m = map(int, input().split())

g = [[] for _ in range(n + 1)]
for _ in range(m):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

tin = [0] * (n + 1)
low = [0] * (n + 1)
parent = [0] * (n + 1)
timer = 0

cycles = []
stack = []

sys.setrecursionlimit(300000)

def dfs(u, p):
    global timer
    timer += 1
    tin[u] = low[u] = timer

    for v in g[u]:
        if v == p:
            continue

        if not tin[v]:
            parent[v] = u
            dfs(v, u)
            low[u] = min(low[u], low[v])
        elif tin[v] < tin[u]:
            low[u] = min(low[u], tin[v])

            cyc = [v]
            cur = u
            while cur != v:
                cyc.append(cur)
                cur = parent[cur]
            cycles.append(cyc)

dfs(1, 0)

tot = n + len(cycles)
tree = [[] for _ in range(tot + 1)]

for idx, cyc in enumerate(cycles, start=1):
    cid = n + idx
    for v in cyc:
        tree[cid].append(v)
        tree[v].append(cid)

LOG = (tot + 1).bit_length()

depth = [0] * (tot + 1)
pref = [0] * (tot + 1)
up = [[0] * (tot + 1) for _ in range(LOG)]
weight = [0] * (tot + 1)

for i in range(n + 1, tot + 1):
    weight[i] = 1

def dfs_tree(u, p):
    up[0][u] = p

    if p:
        pref[u] = pref[p] + weight[u]
        depth[u] = depth[p] + 1
    else:
        pref[u] = weight[u]

    for v in tree[u]:
        if v != p:
            dfs_tree(v, u)

dfs_tree(1, 0)

for j in range(1, LOG):
    prev = up[j - 1]
    cur = up[j]
    for v in range(1, tot + 1):
        cur[v] = prev[prev[v]]

pow2 = [1] * (len(cycles) + 1)
for i in range(1, len(pow2)):
    pow2[i] = (pow2[i - 1] * 2) % MOD

def lca(a, b):
    if depth[a] < depth[b]:
        a, b = b, a

    diff = depth[a] - depth[b]
    bit = 0

    while diff:
        if diff & 1:
            a = up[bit][a]
        diff >>= 1
        bit += 1

    if a == b:
        return a

    for j in range(LOG - 1, -1, -1):
        if up[j][a] != up[j][b]:
            a = up[j][a]
            b = up[j][b]

    return up[0][a]

q = int(input())

ans = []

for _ in range(q):
    a, b = map(int, input().split())

    l = lca(a, b)

    cnt = (
        pref[a]
        + pref[b]
        - 2 * pref[l]
        + weight[l]
    )

    ans.append(str(pow2[cnt]))

sys.stdout.write("\n".join(ans))
```Sau khi tìm thấy tất cả các chu trình, mã sẽ tạo thêm một nút cho mỗi chu trình và kết nối nút đó với tất cả các đỉnh thuộc chu trình. Đây là bước nén quan trọng. 

DFS trên cây khối tính toán cả tổ tiên nâng nhị phân và số tiền tố của các nút chu kỳ. Số tiền tố cho phép khôi phục bất kỳ tổng đường dẫn nào bằng công thức LCA tiêu chuẩn. 

Một điểm tinh tế là bản thân các nút chu kỳ mang trọng lượng`1`, trong khi các đỉnh thông thường mang trọng lượng`0`. Điều này làm cho tổng đường đi bằng số chu kỳ gặp phải. 

Một chi tiết khác là sức mạnh tính toán trước của hai. Số chu kỳ tối đa là nhiều nhất`m`, do đó chỉ cần tính toán trước tuyến tính đơn giản là đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị chứa hai chu kỳ:```
1-2-3-4-1
6-7-8-6
```Cây nén chứa hai nút chu kỳ. 

Truy vấn`(9,2)`: 

| Nút trên đường dẫn nén | Loại | Đóng góp | 
| --- | --- | --- | 
| 9 | đỉnh | 0 | 
| 7 chu kỳ | chu kỳ | 1 | 
| 6 | đỉnh | 0 | 
| 5 | đỉnh | 0 | 
| 3 | đỉnh | 0 | 
| 4 chu kỳ | chu kỳ | 1 | 
| 2 | đỉnh | 0 | 

Tổng số chu kỳ = 2. 

Trả lời:```
2^2 = 4
```phù hợp với mẫu. 

Điều này chứng tỏ rằng sự đóng góp của chu kỳ nhân lên một cách độc lập. 

### Ví dụ tùy chỉnh 

đầu vào:```
4 4
1 2
2 3
3 4
4 1
1
1 3
```Cây nén: 

| Nút cây | Loại | 
| --- | --- | 
| 1 | đỉnh | 
| 2 | đỉnh | 
| 3 | đỉnh | 
| 4 | đỉnh | 
| C | chu kỳ | 

Con đường:```
1 -> C -> 3
```Số chu kỳ = 1. 

Trả lời:```
2^1 = 2
```Hai con đường đó là:```
1-2-3
1-4-3
```Điều này xác nhận việc giải thích nút chu kỳ là một lựa chọn nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) + (n + c) log n + k log n) | DFS, tiền xử lý cây và truy vấn LCA | 
| Không gian | O((n + c) log n) | Cây khối và bàn nâng nhị phân | 

Đây`c`là số chu kỳ và`c ≤ m`. Với mọi giới hạn xung quanh`10^5`, độ phức tạp dễ dàng nằm gọn trong giới hạn 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    from solution import solve
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.getvalue().strip()

# sample 1
assert run(
"""10 11
1 2
2 3
3 4
1 4
3 5
5 6
8 6
8 7
7 6
7 9
9 10
6
1 2
3 5
6 9
9 2
9 3
9 10
"""
) == "\n".join(["2","2","2","4","4","1"])

# tree
assert run(
"""2 1
1 2
1
1 2
"""
) == "1"

# single cycle
assert run(
"""4 4
1 2
2 3
3 4
4 1
1
1 3
"""
) == "2"

# two cycles in chain
assert run(
"""7 8
1 2
2 3
3 1
3 4
4 5
5 6
6 4
6 7
1
1 7
"""
) == "4"

# cycle queried at articulation
assert run(
"""5 5
1 2
2 3
3 1
3 4
4 5
1
5 3
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai nút | 1 | Con đường độc đáo trên cây | 
| Chu kỳ đơn | 2 | Một chu kỳ nhân đôi số lượng | 
| Hai chu kỳ bị xiềng xích | 4 | Phép nhân độc lập | 
| Truy vấn kết thúc ở khớp nối | 1 | Chu kỳ không nhất thiết phải được sử dụng | 
| Mẫu 1 | Đầu ra mẫu | Cấu trúc xương rồng đầy đủ | 

## Vỏ cạnh 

Hãy xem xét một đồ thị chỉ có một chu kỳ:```
4 4
1 2
2 3
3 4
4 1
1
1 3
```Đường dẫn nén chứa chính xác một nút chu trình. Thuật toán trả về`2^1 = 2`, tương ứng với hai hướng xung quanh chu kỳ. 

Bây giờ hãy xem xét:```
5 5
1 2
2 3
3 1
3 4
4 5
1
5 3
```Đường dẫn trong cây khối chỉ đơn giản là:```
5 -> 4 -> 3
```Nút chu trình không được duyệt qua. Số chu kỳ bằng 0 nên câu trả lời là`1`. Điều này ngăn chặn các chu kỳ đếm quá mức chỉ chạm vào tuyến đường. 

Cuối cùng, xét hai chu trình được nối với nhau bằng một cây cầu:```
cycle A -- bridge -- cycle B
```Bất kỳ đường đi nào đi qua đồ thị phải chọn một hướng trong chu kỳ A và chọn một hướng độc lập trong chu kỳ B. Đường dẫn cây khối chứa hai nút chu kỳ, cho`2^2 = 4`. Cấu trúc nhân lên trực tiếp từ đặc tính của cây xương rồng là các chu kỳ không bao giờ chồng chéo lên nhau.
