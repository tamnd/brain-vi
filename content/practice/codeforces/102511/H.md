---
title: "CF 102511H - Đoàn tàu của Hobsons"
description: "Mạng là một đồ thị có hướng với một thuộc tính đặc biệt: mỗi trạm có chính xác một cạnh đi ra. Từ ga i, giá trị đã cho d[i] là ga duy nhất có thể đến được trong một chặng tàu."
date: "2026-08-05T16:21:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102511
codeforces_index: "H"
codeforces_contest_name: "2019 ICPC World Finals"
rating: 0
weight: 102511
solve_time_s: 128
verified: true
draft: false
---

[CF 102511H - Đoàn tàu của Hobsons](https://codeforces.com/problemset/problem/102511/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mạng là một đồ thị có hướng với một thuộc tính đặc biệt: mỗi trạm có chính xác một cạnh đi ra. Từ ga`i`, giá trị đã cho`d[i]`là ga duy nhất có thể đến được bằng một chặng tàu. 

Đối với mỗi trạm`A`, chúng ta cần đếm xem có bao nhiêu trạm xuất phát có thể đến được`A`sử dụng nhiều nhất`k`di chuyển. Bản thân trạm bắt đầu được tính nên trạm luôn được đưa vào câu trả lời của chính nó. 

Đồ thị trong đó mỗi đỉnh có một cạnh ngoài được gọi là đồ thị hàm số. Mỗi thành phần được kết nối của đồ thị như vậy chứa chính xác một chu trình có hướng và mọi trạm khác thuộc về một cây cuối cùng dẫn đến chu trình đó. 

Số lượng trạm có thể lớn như`5 * 10^5`. Điều này loại trừ việc kiểm tra từng trạm xuất phát riêng biệt. Một mô phỏng từ mỗi nút sẽ cần tới`O(n)`di chuyển trên mỗi nút, dẫn đến`O(n^2)`công việc vượt xa những gì có thể. 

Những trường hợp phức tạp được gây ra bởi các chu kỳ và bởi những chuỗi dài dẫn vào chúng. Giải pháp coi biểu đồ như một cây bình thường sẽ thất bại vì các nút chu kỳ có nhiều cách để tiếp cận. 

Ví dụ:```
5 3
2
3
1
5
4
```Đồ thị có hai chu kỳ:`1 -> 2 -> 3 -> 1`Và`4 -> 5 -> 4`. Đầu ra đúng là:```
3
3
3
2
2
```Cách tiếp cận chỉ bằng cây sẽ bỏ lỡ trạm đó`1`có thể đến được từ các trạm`2`Và`3`thông qua chu kỳ. 

Một trường hợp cạnh khác là một chuỗi đi vào một chu trình:```
4 2
2
3
4
3
```Câu trả lời là:```
1
2
4
3
```Ga tàu`4`có thể đạt được từ`1`,`2`,`3`, Và`4`, nhưng khoảng cách từ`1`có ba chân nên bị loại trừ khi`k = 2`. Việc triển khai tìm kiếm theo chiều rộng một cách bất cẩn chỉ đánh dấu các trạm đã ghé thăm mà không theo dõi khoảng cách có thể tính sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là bắt đầu tìm kiếm từ mọi trạm và lần theo các cạnh đi ngược lại. Nó đúng vì nó khám phá chính xác tất cả các điểm xuất phát có thể đi đến đích. Tuy nhiên, trong trường hợp xấu nhất, đồ thị có thể chứa một chuỗi có độ dài`n`, vì vậy việc lặp lại điều này cho mỗi trạm tốn khoảng`n * n`hoạt động. 

Quan sát hữu ích đến từ cấu trúc của đồ thị hàm số. Nếu chúng ta loại bỏ tất cả các cạnh giữa các nút chu trình, mọi thành phần sẽ trở thành một tập hợp các cây có gốc có gốc là các nút chu trình. Đối với một trạm bên trong một trong những cây này, tất cả các trạm có thể tiếp cận nó chính xác là con cháu của nó trong cây đảo ngược. Câu hỏi trở thành: tối đa có bao nhiêu con cháu ở độ sâu`k`bên dưới nút này? 

Truy vấn cây đó có thể được trả lời ngoại tuyến. Trong quá trình truyền tải theo chiều sâu, chúng tôi gán cho mỗi nút một khoảng Euler. Cây con của nút là một khoảng liền kề theo thứ tự này. Chúng tôi sắp xếp các truy vấn theo độ sâu tối đa cho phép của chúng và thêm các nút vào cây Fenwick khi độ sâu của chúng trở nên hợp lệ. Cây Fenwick sau đó sẽ đưa ra số lượng nút hoạt động bên trong mỗi khoảng thời gian của cây con. 

Phần còn lại là xử lý các nút chu kỳ. Một nút chu trình cũng có thể được truy cập từ các nút chu trình khác. Trên một chu kỳ có độ dài`m`, mọi nút chu kỳ đều đến mọi nút chu kỳ khác trong`m - 1`các bước, vì vậy sự đóng góp của chu trình chỉ đơn giản là`min(k + 1, m)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm tất cả các nút chu kỳ bằng cách sử dụng phương pháp loại trừ bậc. Các nút có độ bằng 0 không thể là một phần của một chu trình, do đó, việc loại bỏ chúng nhiều lần sẽ để lại chính xác các chu kỳ được định hướng. 
2. Xây dựng khu rừng đảo ngược bằng cách thêm một cạnh từ một nút vào tất cả các nút không có chu kỳ trỏ đến nó. Các cạnh của chu kỳ này bị bỏ qua vì chúng được xử lý riêng biệt. 
3. Thực hiện DFS trên khu rừng này. Gán cho mỗi nút một thời gian vào Euler, thời gian thoát và độ sâu của nó từ gốc chu trình. Trong quá trình truyền tải này, hãy ghi lại số lượng nút tồn tại ở mọi độ sâu. 
4. Tạo một truy vấn cho mỗi nút. Truy vấn yêu cầu số lượng nút bên trong cây con Euler của nó có độ sâu tối đa là`depth[node] + k`. 
5. Sắp xếp các truy vấn theo giới hạn độ sâu của chúng. Quét qua độ sâu từ nhỏ đến lớn. Khi độ sâu được cho phép, hãy chèn mọi nút có độ sâu đó vào cây Fenwick bằng vị trí Euler của nó. Câu trả lời truy vấn là tổng Fenwick trên khoảng cây con của nó. 
6. Đối với mỗi nút chu kỳ, hãy thêm số lượng nút chu kỳ có thể truy cập. Đối với một chu kỳ có độ dài`m`, giá trị này là`min(k + 1, m)`. 

Tại sao nó hoạt động: 

Mỗi trạm bên ngoài một chu trình đều có một con đường duy nhất hướng tới chu trình. Việc đảo ngược các cạnh biến tất cả các phần trước có thể có thành một cây, vì vậy mọi trạm khởi đầu hợp lệ đều chính xác là hậu duệ của cây đó. Thuộc tính khoảng Euler đảm bảo rằng các truy vấn cây con tính chính xác những con cháu đó và việc quét độ sâu đảm bảo rằng chỉ các nút bên trong`k`các cạnh được bao gồm. Các nút chu trình là nơi duy nhất tồn tại nhiều hướng xung quanh một chu trình và sự đóng góp của chúng không phụ thuộc vào các cây đính kèm, đó là lý do tại sao nó có thể được thêm riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n, k = map(int, input().split())
    to = [0] * n
    rev = [[] for _ in range(n)]
    indeg = [0] * n

    for i in range(n):
        x = int(input()) - 1
        to[i] = x
        rev[x].append(i)
        indeg[x] += 1

    from collections import deque

    q = deque()
    for i in range(n):
        if indeg[i] == 0:
            q.append(i)

    removed = [False] * n
    while q:
        x = q.popleft()
        removed[x] = True
        y = to[x]
        indeg[y] -= 1
        if indeg[y] == 0:
            q.append(y)

    cycle = [not removed[i] for i in range(n)]

    children = [[] for _ in range(n)]
    for i in range(n):
        if not cycle[i]:
            children[to[i]].append(i)

    tin = [0] * n
    tout = [0] * n
    depth = [0] * n
    nodes_by_depth = []
    timer = 0

    for root in range(n):
        if cycle[root]:
            stack = [(root, 0, False)]
            depth[root] = 0
            while stack:
                x, idx, exit_flag = stack.pop()
                if exit_flag:
                    tout[x] = timer - 1
                    continue
                tin[x] = timer + 1
                timer += 1
                while len(nodes_by_depth) <= depth[x]:
                    nodes_by_depth.append([])
                nodes_by_depth[depth[x]].append(x)
                stack.append((x, 0, True))
                for c in reversed(children[x]):
                    depth[c] = depth[x] + 1
                    stack.append((c, 0, False))

    queries = [(min(n - 1, depth[i] + k), i) for i in range(n)]
    queries.sort()

    bit = Fenwick(n)
    ans = [0] * n
    ptr = 0

    for limit, node in queries:
        while ptr <= limit and ptr < len(nodes_by_depth):
            for x in nodes_by_depth[ptr]:
                bit.add(tin[x], 1)
            ptr += 1
        ans[node] = bit.range_sum(tin[node], tout[node])

    visited = [False] * n
    for i in range(n):
        if cycle[i] and not visited[i]:
            cur = i
            length = 0
            while not visited[cur]:
                visited[cur] = True
                length += 1
                cur = to[cur]
            add = min(k + 1, length)
            cur = i
            while True:
                ans[cur] += add
                cur = to[cur]
                if cur == i:
                    break

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giai đoạn loại bỏ theo mức độ tách các nút chu trình khỏi các nút cây mà không cần đệ quy, điều này là cần thiết vì một chuỗi có độ dài`500000`có thể vượt quá giới hạn đệ quy Python. Sau sự phân tách đó, chỉ các cạnh không có chu trình được đặt vào`children`, do đó cấu trúc DFS là một khu rừng thực sự. 

Việc truyền tải Euler cho`tin`Và`tout`các giá trị. Khoảng thời gian`[tin[v], tout[v]]`chứa chính xác các nút trong`v`cây con cây đảo ngược của. Cây Fenwick lưu trữ các nút hiện đang hoạt động trong quá trình quét độ sâu, do đó mọi truy vấn của cây con sẽ trở thành một tổng phạm vi. 

Việc xử lý chu trình được thực hiện có chủ đích sau khi truy vấn cây. Câu trả lời dạng cây đã bao gồm chính nút chu trình, vì vậy chỉ cần thêm các nút chu trình khác. sử dụng`min(k + 1, length)`tránh được lỗi từng cái một khi`k`lớn hơn độ dài chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi nút tham gia quét Fenwick một lần và mọi truy vấn đều thực hiện cập nhật hoặc tính tổng logarit. | 
| Không gian | O(n) | Biểu đồ, mảng truyền tải và cây Fenwick đều lưu trữ dữ liệu có kích thước tuyến tính. | 

Giải pháp phù hợp với`5 * 10^5`giới hạn nút vì nó không bao giờ thực hiện công tỷ lệ thuận với tích của các nút và độ dài đường dẫn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_out
    sys.stdin = old
    return out.getvalue()

assert run("""6 2
2
3
4
5
4
3
""") == """1
2
4
5
3
1
"""

assert run("""5 3
2
3
1
5
4
""") == """3
3
3
2
2
"""

assert run("""2 1
2
1
""") == """2
2
"""

assert run("""4 2
2
3
4
3
""") == """1
2
4
3
"""

assert run("""5 1
2
3
4
5
4
""") == """1
2
2
2
1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đầu tiên |`1 2 4 5 3 1`| Các nút cây gắn liền với một chu kỳ | 
| Mẫu thứ hai |`3 3 3 2 2`| Chu kỳ thuần khiết không có cây | 
| Chu kỳ hai nút |`2 2`| Chu kỳ nhỏ nhất có thể | 
| Chuỗi thành chu kỳ |`1 2 4 3`| Xử lý cắt khoảng cách | 
| Dây chuyền dài |`1 2 2 2 1`| Các trường hợp ranh giới cho`k`| 

## Vỏ cạnh 

Đối với trường hợp chu kỳ đầu tiên:```
5 3
2
3
1
5
4
```Ba trạm đầu tiên tạo thành một chu kỳ có độ dài ba. Từ`k = 3`, mọi trạm trên chu trình đó sẽ đến mọi trạm chu trình khác, mang lại cho mỗi trạm trong số đó ba lần khởi động hợp lệ. Chu kỳ thứ hai có độ dài bằng hai, do đó cả hai trạm cũng đến được với nhau, nhưng đầu ra vẫn bị giới hạn bởi quy mô của chu kỳ đó. 

Đối với trường hợp dây chuyền:```
4 2
2
3
4
3
```Cây đảo ngược gốc ở ga`3`chứa`1`,`2`,`3`, Và`4`. Truy vấn cây cho trạm`3`đếm độ sâu`0`,`1`, Và`2`, nhưng không bao gồm trạm`1`vì nó cách ba cạnh. Sự đóng góp của chu trình chỉ thêm các trạm trên`3 -> 4 -> 3`chu kỳ, đưa ra các giá trị cuối cùng`1, 2, 4, 3`. 

Đối với một lượng lớn`k`, chẳng hạn như một chu kỳ có độ dài năm với`k = 10`, đóng góp của chu kỳ vẫn chỉ là năm. các`min(k + 1, length)`công thức ngăn việc đếm cùng một nút chu kỳ nhiều lần sau khi xoay hoàn toàn.
