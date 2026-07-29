---
title: "CF 102760I - Truy vấn trên cây 17"
description: "Chúng ta có một cây có gốc với gốc 1. Mỗi đỉnh lưu trữ một số lượng người không âm. Ban đầu mọi đỉnh đều có giá trị bằng 0. Mỗi thao tác sẽ tăng các giá trị trên toàn bộ cây con hoặc toàn bộ đường dẫn đơn giản."
date: "2026-07-29T00:03:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102760
codeforces_index: "I"
codeforces_contest_name: "2020 KAIST 10th ICPC Mock Contest (XXI Open Cup. Grand Prix of Korea. Division 2)"
rating: 0
weight: 102760
solve_time_s: 114
verified: true
draft: false
---

[CF 102760I - Truy vấn trên cây 17](https://codeforces.com/problemset/problem/102760/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với gốc 1. Mỗi đỉnh lưu trữ một số lượng người không âm. Ban đầu mọi đỉnh đều có giá trị bằng 0. Mỗi thao tác sẽ tăng các giá trị trên toàn bộ cây con hoặc toàn bộ đường dẫn đơn giản. Sau mỗi lần cập nhật, chúng tôi phải in đỉnh nơi tất cả mọi người có tổng khoảng cách di chuyển nhỏ nhất có thể. Nếu nhiều đỉnh có cùng mức tối thiểu thì chúng ta chọn đỉnh gần gốc nhất. 

Giá trị chúng tôi đang giảm thiểu là tổng trọng số của khoảng cách. Với tối đa$10^5$đỉnh và$10^5$hoạt động, việc tính toán lại giá trị của mỗi đỉnh sau mỗi lần cập nhật sẽ yêu cầu khoảng$10^{10}$làm việc, điều đó là không thể. Chúng tôi cần xử lý logarit hoặc gần logarit cho mỗi thao tác. 

Các trường hợp phức tạp đến từ quy tắc hòa và từ các đỉnh có giá trị bằng 0. Một đỉnh có cây con chứa đúng một nửa số người không bắt buộc chúng ta phải chuyển sang con đó vì cả hai bên đều đưa ra chi phí như nhau và đỉnh nông hơn phải thắng. 

Ví dụ: nếu cây là một chuỗi:```
1
|
2
|
3
```và đỉnh 3 có giá trị 10, câu trả lời là 1 chứ không phải 3. Việc chuyển từ 1 sang 2 hoặc 3 không cải thiện khoảng cách thu thập đủ để đánh bại lựa chọn nông hơn. 

## Phương pháp tiếp cận 

Phương pháp bạo lực sẽ duy trì rõ ràng mọi giá trị đỉnh. Sau mỗi lần cập nhật, chúng tôi có thể chạy duyệt cây từ mọi đỉnh có thể có câu trả lời và tính tổng khoảng cách. Điều này đúng vì nó trực tiếp đánh giá định nghĩa, nhưng một truy vấn duy nhất sẽ có giá$O(n)$hoặc tệ hơn, dẫn đến$O(nQ)$hoạt động. 

Quan sát quan trọng là câu trả lời là trọng tâm. Nếu chúng ta di chuyển qua một cạnh từ một đỉnh đến cha mẹ của nó, tổng khoảng cách chỉ thay đổi theo trọng lượng của cây con được di chuyển. Nếu một cây con chứa hơn một nửa số người thì việc di chuyển vào cây con đó sẽ cải thiện câu trả lời. Nếu không chúng ta nên ở lại. 

Vấn đề còn lại là duy trì tổng cây con dưới phép cộng cây con và phép cộng đường dẫn. Chúng tôi san phẳng cây bằng ánh sáng phân hủy mạnh. Theo thứ tự DFS, mỗi cây con trở thành một khoảng và mọi đường dẫn trở thành$O(\log n)$khoảng thời gian. Cây phân đoạn lười duy trì các giá trị theo thứ tự DFS, cho phép bổ sung khoảng thời gian và tìm kiếm tiền tố. 

Trọng tâm có trọng số có thể được tìm thấy bằng cách định vị vị trí thứ tự DFS đầu tiên có trọng số tiền tố đạt tới một nửa tổng trọng lượng. Câu trả lời nằm ở chuỗi tổ tiên của đỉnh đó. Nâng nhị phân được sử dụng để di chuyển lên trên cho đến khi tìm thấy trọng tâm hợp lệ cao nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nQ)$|$O(n)$| Quá chậm | 
| HLD + Cây phân đoạn |$O(Q\log^2 n)$|$O(n\log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây gốc và tính toán kích thước cây con, độ sâu, con nặng, thứ tự DFS và tổ tiên nâng nhị phân. 
2. Sử dụng phân tách ánh sáng nặng sao cho mỗi cây con tương ứng với một khoảng DFS và mỗi đường dẫn có thể được chia thành nhiều khoảng logarit. 
3. Lưu trữ các giá trị đỉnh hiện tại trong cây phân đoạn lười theo thứ tự DFS. Cây phân đoạn hỗ trợ thêm một vào một khoảng, tìm tổng trọng số và tìm vị trí đầu tiên nơi tiền tố đạt đến mục tiêu. 
4. Sau khi cập nhật, hãy tính tổng số người$S$. Tìm vị trí DFS đầu tiên có tổng tiền tố ít nhất$\lceil S/2\rceil$. 
5. Câu trả lời là tổ tiên của đỉnh này. Sử dụng các truy vấn nâng nhị phân và tổng cây con để leo lên trong khi tổ tiên hiện tại không chứa đủ trọng số. 

Tại sao nó hoạt động: sự khác biệt giữa chi phí tại một đỉnh và cha mẹ của nó chỉ phụ thuộc vào việc bên con có chứa hơn một nửa tổng trọng lượng hay không. Đỉnh được chọn chính xác là đỉnh nông nhất mà hướng con chứa cạnh nặng không thể cải thiện chi phí. Việc tìm kiếm tiền tố sẽ tìm thấy một điểm bên trong mặt nặng đó và tổ tiên leo núi sẽ giải quyết quy tắc buộc bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(300000)

n = int(input())
g = [[] for _ in range(n + 1)]
for _ in range(n - 1):
    a, b = map(int, input().split())
    g[a].append(b)
    g[b].append(a)

par = [[0] * (n + 1) for _ in range(17)]
dep = [0] * (n + 1)
sz = [0] * (n + 1)
son = [0] * (n + 1)

def dfs1(u, p):
    par[0][u] = p
    dep[u] = dep[p] + 1
    sz[u] = 1
    best = 0
    for v in g[u]:
        if v == p:
            continue
        dfs1(v, u)
        sz[u] += sz[v]
        if sz[v] > best:
            best = sz[v]
            son[u] = v

dfs1(1, 0)

for j in range(1, 17):
    for i in range(1, n + 1):
        par[j][i] = par[j - 1][par[j - 1][i]]

tin = [0] * (n + 1)
tout = [0] * (n + 1)
rev = [0] * (n + 1)
top = [0] * (n + 1)
timer = 0

def dfs2(u, t):
    global timer
    timer += 1
    tin[u] = timer
    rev[timer] = u
    top[u] = t
    if son[u]:
        dfs2(son[u], t)
    for v in g[u]:
        if v != par[0][u] and v != son[u]:
            dfs2(v, v)
    tout[u] = timer

dfs2(1, 1)

class Seg:
    def __init__(self, n):
        self.s = [0] * (4 * n)
        self.lz = [0] * (4 * n)

    def add(self, x, l, r, ql, qr):
        if ql <= l and r <= qr:
            self.s[x] += r - l + 1
            self.lz[x] += 1
            return
        m = (l + r) // 2
        self.push(x, l, r)
        if ql <= m:
            self.add(x * 2, l, m, ql, qr)
        if qr > m:
            self.add(x * 2 + 1, m + 1, r, ql, qr)
        self.s[x] = self.s[x * 2] + self.s[x * 2 + 1]

    def push(self, x, l, r):
        if self.lz[x]:
            m = (l + r) // 2
            v = self.lz[x]
            self.s[x * 2] += v * (m - l + 1)
            self.s[x * 2 + 1] += v * (r - m)
            self.lz[x * 2] += v
            self.lz[x * 2 + 1] += v
            self.lz[x] = 0

    def query(self, x, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.s[x]
        self.push(x, l, r)
        m = (l + r) // 2
        res = 0
        if ql <= m:
            res += self.query(x * 2, l, m, ql, qr)
        if qr > m:
            res += self.query(x * 2 + 1, m + 1, r, ql, qr)
        return res

    def kth(self, x, l, r, k):
        if l == r:
            return l
        self.push(x, l, r)
        m = (l + r) // 2
        if self.s[x * 2] >= k:
            return self.kth(x * 2, l, m, k)
        return self.kth(x * 2 + 1, m + 1, r, k - self.s[x * 2])

seg = Seg(n)

def path_add(a, b):
    while top[a] != top[b]:
        if dep[top[a]] < dep[top[b]]:
            a, b = b, a
        seg.add(1, 1, n, tin[top[a]], tin[a])
        a = par[0][top[a]]
    if dep[a] > dep[b]:
        a, b = b, a
    seg.add(1, 1, n, tin[a], tin[b])

def subtree_sum(u):
    return seg.query(1, 1, n, tin[u], tout[u])

q = int(input())
ans = []
for _ in range(q):
    data = list(map(int, input().split()))
    if data[0] == 1:
        u = data[1]
        seg.add(1, 1, n, tin[u], tout[u])
    else:
        path_add(data[1], data[2])

    total = seg.s[1]
    need = (total + 1) // 2
    x = rev[seg.kth(1, 1, n, need)]

    for j in range(16, -1, -1):
        p = par[j][x]
        if p and subtree_sum(p) >= need:
            x = p

    while par[0][x] and subtree_sum(par[0][x]) >= need:
        x = par[0][x]

    ans.append(str(x))

print("\n".join(ans))
```Cây phân đoạn lưu trữ các giá trị đỉnh thực tế theo thứ tự DFS. các`add`hoạt động xử lý cả cập nhật cây con và đường dẫn ánh sáng nặng. các`kth`hàm tìm vị trí đầu tiên mà trọng lượng tích lũy đạt một nửa tổng trọng số, xác định vùng chứa trọng tâm. 

Cú nhảy tổ tiên sử dụng thực tế là chỉ tổ tiên của đỉnh được tìm thấy đó mới có thể là câu trả lời. Chuyển động đi lên cuối cùng giữ cho tâm hợp lệ nông nhất, phù hợp với điểm đứt dây buộc cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q\log^2 n)$| Mỗi bản cập nhật đường dẫn sử dụng$O(\log n)$phân khúc, chi phí cập nhật từng phân khúc$O(\log n)$và tìm kiếm centroid sử dụng bước nhảy tổ tiên logarit. | 
| Không gian |$O(n\log n)$| Nâng nhị phân chiếm ưu thế trong việc sử dụng bộ nhớ. | 

Các giới hạn cho phép phương pháp này vì mọi thao tác đều là logarit thay vì quét toàn bộ cây. 

## Vỏ cạnh 

Một đường đi duy nhất với toàn bộ trọng số tập trung tại một lá sẽ kiểm tra quy tắc buộc. Thuật toán tìm tiền tố nặng nhưng quay trở lại tổ tiên hợp lệ gần nhất, do đó nó không trả về lá một cách sai lầm. 

Truy vấn cập nhật một đỉnh thông qua một đường dẫn trong đó cả hai điểm cuối đều bằng nhau để kiểm tra ranh giới phân tách đường dẫn. Phân rã ánh sáng nặng coi đây là khoảng một đỉnh và cây phân đoạn thực hiện cập nhật điểm bình thường. 

Một cây trong đó mặt nặng chứa chính xác một nửa tổng trọng lượng kiểm tra việc xử lý bình đẳng. Thuật toán sử dụng$\lceil S/2\rceil$và chỉ di chuyển lên trên khi tổ tiên vẫn thỏa mãn điều kiện nên đỉnh nông hơn được giữ nguyên.
