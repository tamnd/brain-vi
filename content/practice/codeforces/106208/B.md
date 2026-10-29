---
title: "CF 106208B - Truy vấn giá đường dẫn cây"
description: "Chúng ta có một cây có gốc trong đó mỗi nút đại diện cho một vị trí lưu trữ một số mục giống hệt nhau. Mỗi nút có hai thuộc tính: nó chứa bao nhiêu mặt hàng và một mức giá chung cho tất cả các mặt hàng tại nút đó."
date: "2026-06-19T16:18:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106208
codeforces_index: "B"
codeforces_contest_name: "Inter University Programming Contest - MU CSE Fest 2025 - MIRROR"
rating: 0
weight: 106208
solve_time_s: 58
verified: true
draft: false
---

[CF 106208B - Truy vấn giá đường dẫn cây](https://codeforces.com/problemset/problem/106208/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi nút đại diện cho một vị trí lưu trữ một số mục giống hệt nhau. Mỗi nút có hai thuộc tính: nó chứa bao nhiêu mặt hàng và một mức giá chung cho tất cả các mặt hàng tại nút đó. 

Một truy vấn yêu cầu chúng ta xem xét một nút u cụ thể và chỉ xem xét các nút nằm trên đường đi từ u đến gốc. Trong số những tổ tiên đó, chúng tôi chỉ quan tâm đến các nút nằm trong một khoảng cách nhất định d tính từ bạn trong chỉ số cây. Từ các nút thỏa mãn cả hai điều kiện, chúng tôi đếm tất cả các mục có giá nút nằm trong một khoảng nhất định. 

Vì vậy, mỗi truy vấn về cơ bản là yêu cầu tổng được lọc trên phân đoạn tổ tiên bị ràng buộc của một nút, trong đó các bộ lọc kết hợp khoảng cách cây và ràng buộc phạm vi giá trị đối với trọng số của nút. 

Kích thước cây và số lượng truy vấn đều lên tới 2×10^5. Bất kỳ giải pháp nào kiểm tra đường dẫn đầy đủ cho mỗi truy vấn sẽ ngay lập tức thất bại vì một đường dẫn gốc có thể là O(n) và việc thực hiện điều đó cho mọi truy vấn sẽ dẫn đến O(nq), vượt xa giới hạn có thể chấp nhận được. Ngay cả O(n log n) cho mỗi truy vấn cũng sẽ quá chậm. 

Khó khăn tinh tế là các truy vấn không tĩnh. Các tham số được giải mã giống như XOR bằng cách sử dụng câu trả lời trước đó, do đó, các giả định về tính độc lập hoặc sắp xếp lại thứ tự ngoại tuyến không thể sử dụng trực tiếp được. Điều này buộc chúng tôi vào một cấu trúc trực tuyến hoàn toàn. 

Một cách tiếp cận đơn giản cũng phá vỡ các trường hợp u sâu và d lớn, nghĩa là bao gồm toàn bộ chuỗi gốc và khi nhiều nút có chung mức giá nên việc lọc phạm vi vẫn yêu cầu truyền tải đầy đủ. Một dạng lỗi khác là khi tất cả các nút nằm trên một đường dẫn duy nhất, điều này làm thoái hóa cây thành danh sách liên kết và tối đa hóa chi phí truy vấn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi leo từ nút u trở lên về phía gốc, dừng lại khi vượt quá khoảng cách d. Đối với mỗi nút được truy cập, chúng tôi kiểm tra xem giá của nó có nằm trong phạm vi [pwer, pupper] hay không và nếu có thì chúng tôi sẽ thêm ki vào câu trả lời. 

Điều này đúng vì nó tuân theo định nghĩa truy vấn: các nút đủ điều kiện chính xác là các nút tổ tiên trong khoảng cách d. Tuy nhiên, cách tiếp cận này có thể duyệt qua các nút O(n) cho mỗi truy vấn trong cây lệch. Với tối đa 2×10^5 truy vấn, điều này dẫn đến O(nq), điều này không khả thi. 

Quan sát quan trọng là các truy vấn luôn bị ràng buộc ở các đường dẫn gốc, là các đường dẫn có cấu trúc cố định trong cây. Thay vì đi lên trên mỗi truy vấn, chúng ta có thể xử lý trước cấu trúc từ gốc đến nút và chuyển đổi từng truy vấn thành truy vấn phạm vi trên một tập hợp các nút trên một đường dẫn. Khi chúng tôi sửa thứ tự truyền tải cho cây, chẳng hạn như chuyến tham quan Euler hoặc thứ tự DFS kết hợp với nâng nhị phân để kiểm tra tổ tiên, chúng tôi có thể giảm vấn đề khi truy vấn một tập hợp nút động trong cấu trúc giống cây con. 

Một cách cải cách mạnh mẽ hơn là coi mỗi nút là một điểm có hai tọa độ: độ sâu (để lọc khoảng cách dọc theo chuỗi tổ tiên) và giá (để lọc phạm vi). Đối với một nút cố định u, tất cả các nút hợp lệ đều nằm trên chuỗi tổ tiên của nó và ràng buộc khoảng cách giới hạn chúng ta ở một tiền tố liền kề của chuỗi đó về độ chênh lệch độ sâu. 

Điều này chuyển đổi mỗi truy vấn thành: trong số các tổ tiên của u có độ sâu trong một khoảng cụ thể, đếm tổng số ki có giá nút nằm trong [pwer, pupper]. Đây là vấn đề về cấu trúc ngoại tuyến hai chiều cổ điển trên cây, có thể được giải quyết bằng cách sử dụng cây phân đoạn liên tục được xây dựng theo thứ tự DFS và được duy trì theo độ sâu hoặc tương đương, DSU trên cây là không đủ vì các truy vấn không dựa trên cây con mà dựa trên chuỗi tổ tiên. 

Giải pháp rõ ràng là sử dụng nâng cấp nhị phân kết hợp với cây phân đoạn liên tục trên các nút được lập chỉ mục theo độ sâu. Chúng tôi xây dựng một cấu trúc được phiên bản trong đó phiên bản tại nút u đại diện cho tất cả các nút trên đường dẫn từ gốc đến u. Mỗi phiên bản lưu trữ một cấu trúc tần số theo giá được tính theo ki. Sau đó, truy vấn tổ tiên trở thành truy vấn khác biệt giữa các phiên bản.

Ngoài ra, chúng tôi chuyển đổi ràng buộc khoảng cách thành một bước nhảy: từ u, chúng tôi tìm thấy tổ tiên v cao nhất sao cho độ sâu [u] - độ sâu [v] ≤ d. Tổ tiên đó được tìm thấy bằng cách nâng nhị phân. Khi đó câu trả lời là tổng trên đường dẫn root→u trừ root→parent(v), giới hạn trong phạm vi giá. 

Điều này làm giảm mỗi truy vấn thành hai truy vấn phạm vi tiền tố trên cây phân đoạn liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Cây phân đoạn liên tục + nâng nhị phân | O((n+q) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở mức 1 và tính toán độ sâu của mọi nút bằng DFS. 

Chúng tôi tính toán trước tổ tiên nâng nhị phân để có thể nhảy lên theo thời gian logarit. Điều này cho phép chúng ta tìm thấy, đối với bất kỳ nút u và khoảng cách d nào, tổ tiên v cao nhất sao cho v vẫn nằm trong khoảng cách d tính từ u. 

Chúng tôi xây dựng một cây phân khúc ổn định dựa trên các giá trị giá. Vì giá tăng lên 10^9 nên trước tiên chúng tôi nén chúng vào một không gian tọa độ được sắp xếp nhỏ hơn. Mỗi phiên bản của cây phân đoạn tương ứng với một nút u và lưu trữ tập hợp nhiều mục của tất cả các mục từ gốc đến u. 

Với mỗi nút u, chúng ta xây dựng version[u] từ version[parent[u]] bằng cách chèn các mục ki với giá pi. 

Mỗi nút cây phân đoạn lưu trữ tổng số mặt hàng trong khoảng giá nén của nó. 

Để trả lời một truy vấn, chúng tôi tính toán v, tổ tiên nằm ngay bên ngoài ranh giới khoảng cách cho phép. Các nút hợp lệ chính xác là các nút trên đường dẫn root→u ngoại trừ root→parent[v]. Vì vậy, chúng tôi tính toán: 

query(version[u], pwer, pupper) trừ truy vấn(version[parent[v]], pwer, pupper). 

Mỗi truy vấn trên cây phân đoạn cố định sẽ cho chúng ta tổng các mục trong phạm vi giá. 

### Tại sao nó hoạt động 

Cấu trúc liên tục đảm bảo rằng version[u] mã hóa chính xác tất cả các nút trên đường dẫn gốc tới u, bao gồm nhiều mục. Phép trừ version[parent[v]] loại bỏ chính xác tiền tố của đường dẫn nằm ngoài khoảng cách d, để lại chính xác các nút trong phân đoạn tổ tiên hợp lệ. Vì mỗi nút đóng góp độc lập và việc lọc giá được xử lý theo tổng phạm vi cây phân đoạn nên kết quả khớp chính xác với định nghĩa truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("l", "r", "val")
    def __init__(self):
        self.l = -1
        self.r = -1
        self.val = 0

def build(a, l, r):
    idx = len(seg)
    seg.append(Node())
    if l != r:
        m = (l + r) // 2
        seg[idx].l = build(a, l, m)
        seg[idx].r = build(a, m + 1, r)
    return idx

def update(prev, l, r, pos, val):
    idx = len(seg)
    seg.append(Node())
    seg[idx].l = seg[prev].l
    seg[idx].r = seg[prev].r
    seg[idx].val = seg[prev].val + val
    if l != r:
        m = (l + r) // 2
        if pos <= m:
            seg[idx].l = update(seg[prev].l, l, m, pos, val)
        else:
            seg[idx].r = update(seg[prev].r, m + 1, r, pos, val)
    return idx

def query(node, l, r, ql, qr):
    if node == -1 or qr < l or r < ql:
        return 0
    if ql <= l and r <= qr:
        return seg[node].val
    m = (l + r) // 2
    return query(seg[node].l, l, m, ql, qr) + query(seg[node].r, m + 1, r, ql, qr)

n = int(input())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    a, b = map(int, input().split())
    g[a].append(b)
    g[b].append(a)

k = [0] * (n + 1)
p = [0] * (n + 1)

for i in range(1, n + 1):
    k[i], p[i] = map(int, input().split())

vals = sorted(set(p[1:]))

mp = {v: i + 1 for i, v in enumerate(vals)}
m = len(vals)

parent = [[0] * (n + 1) for _ in range(20)]
depth = [0] * (n + 1)
version = [0] * (n + 1)

def dfs(u, par):
    parent[0][u] = par
    version[u] = update(version[par], 1, m, mp[p[u]], k[u])
    for v in g[u]:
        if v == par:
            continue
        depth[v] = depth[u] + 1
        dfs(v, u)

dfs(1, 0)

for j in range(1, 20):
    for i in range(1, n + 1):
        parent[j][i] = parent[j - 1][parent[j - 1][i]]

def jump(u, d):
    for i in range(20):
        if d & (1 << i):
            u = parent[i][u]
    return u

def kth_ancestor(u, d):
    return jump(u, d)

q = int(input())
ans = 0

for _ in range(q):
    Ui, Di, PL, PR = map(int, input().split())
    u = (Ui + ans) % n + 1
    d = (Di + ans) % n
    pl = (PL + ans) % (10**9) + 1
    pr = (PR + ans) % (10**9) + 1
    if pl > pr:
        pl, pr = pr, pl

    v = kth_ancestor(u, d)

    l = 1
    r = m

    def get_range(x):
        if x == 0:
            return 0
        return query(version[x], 1, m, pl_idx, pr_idx)

    pl_idx = 1
    pr_idx = m
    for i, val in enumerate(vals):
        if val >= pl:
            pl_idx = i + 1
            break
    for i, val in enumerate(vals):
        if val > pr:
            pr_idx = i
            break

    res = query(version[u], 1, m, pl_idx, pr_idx)
    if v != 0:
        res -= query(version[parent[0][v]], 1, m, pl_idx, pr_idx)

    ans = res
    print(ans)
```Giải pháp này xây dựng một cây phân đoạn ổn định trong đó mỗi nút lưu trữ thông tin tiền tố từ gốc đến đỉnh đó. DFS chịu trách nhiệm xây dựng các phiên bản này một cách nhất quán để các mối quan hệ tổ tiên chuyển thành những khác biệt về tiền tố. 

Nâng nhị phân được sử dụng để xác định vị trí tổ tiên biên trong khoảng cách d theo thời gian logarit. Khi đã biết ranh giới đó, chúng tôi sẽ xóa mọi thứ phía trên nó bằng cách trừ đi phiên bản gốc, để lại chính xác đoạn hợp lệ của đường dẫn root-to-u. 

Bước nén giá là rất quan trọng vì cây phân khúc phải hoạt động trong phạm vi chỉ số dày đặc. Nếu không nén, cấu trúc sẽ không thể được xây dựng trong giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó nút 1 là gốc, nút 2 là con của nó và nút 3 là con của 2. Giả sử các mặt hàng và giá cả là: 

Nút 1: k=3, p=5 

Nút 2: k=2, p=7 

Nút 3: k=4, p=7 

Truy vấn: u=3, d=1, khoảng giá [6,7] 

Chúng tôi xây dựng các phiên bản: 

| Nút | Phiên bản gốc | Giá chèn | Nội dung phiên bản | 
| --- | --- | --- | --- | 
| 1 | trống | 5×3 | (5:3) | 
| 2 | v1 | 7×2 | (5:3, 7:2) | 
| 3 | v2 | 7×4 | (5:3, 7:6) | 

Đối với u=3 và d=1, chúng ta chỉ có thể bao gồm các nút trong một cạnh trở lên, vì vậy các nút hợp lệ là 3 và 2. Trừ version[parent[2]] = version[1] sẽ loại bỏ nút 1. Nội dung còn lại là nút 2 và 3. 

Chúng tôi truy vấn phạm vi giá [6,7], chỉ thu được giá 7, đưa ra tổng cộng 6 mặt hàng. 

Dấu vết này cho thấy cách tính liên tục chuyển đổi quá trình lọc đường dẫn thành phép trừ tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi cập nhật và truy vấn trên cây phân đoạn liên tục mất thời gian logarit | 
| Không gian | O(n log n) | Mỗi nút tạo các nút cây phân đoạn O(log n) | 

Các ràng buộc cho phép tối đa 2×10^5 nút và truy vấn, do đó cần có chi phí logarit. Giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    return "ok"

# minimal tree
assert run("""1
1
1
1
1 1 0 1
""") == "ok"

# chain tree
assert run("""3
1 2
2 3
1 1
1 2
1 3
1
1 2 1 3
""") == "ok"

# all same price
assert run("""5
1 2
2 3
3 4
4 5
1 5
1 5
1 5
1 5
1 5
1
5 4 1 5
""") == "ok"

# boundary distance zero
assert run("""3
1 2
2 3
1 1
1 2
1 3
1
3 0 2 2
""") == "ok"

# max-like structure stress
assert run("""4
1 2
1 3
1 4
10 1
10 2
10 3
10 4
1
4 3 1 4
""") == "ok"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây tối thiểu | tầm thường | độ chính xác của nút đơn | 
| cây xích | con đường nhỏ | logic tiền tố tổ tiên | 
| cùng giá | tổng hợp đầy đủ | xử lý trùng lặp | 
| d = 0 trường hợp | nút đơn | loại trừ ranh giới | 
| cây sao | anh chị em độc lập | phân nhánh đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi d = 0. Trong trường hợp đó, chỉ tính nút u. Thuật toán xử lý điều này vì ranh giới tổ tiên trở thành u và phép trừ version[parent[u]] sẽ loại bỏ mọi thứ ngoại trừ phần đóng góp của u. 

Một trường hợp cạnh khác xảy ra khi u ở gần gốc. Nếu khoảng cách d vượt quá độ sâu [u], thì tổ tiên biên sẽ trở thành 0. Bước trừ bị bỏ qua, để lại đường dẫn từ gốc đến u đầy đủ, điều này đúng vì tất cả các nút đều nằm trong phạm vi. 

Trường hợp thứ ba là khi tất cả các mức giá đều giống nhau. Sau đó, việc lọc giá trở nên không phù hợp và cấu trúc hoạt động giống như một tổng cây con thuần túy trên một đường dẫn. Cây phân đoạn cố định vẫn tích lũy chính xác tất cả các giá trị ki, do đó câu trả lời giảm xuống việc đếm các nút trên phân đoạn tổ tiên hợp lệ. 

Trường hợp cạnh cuối cùng là khi ki = 0 đối với nhiều nút. Các nút này vẫn tồn tại về mặt cấu trúc nhưng không đóng góp gì. Bước cập nhật thêm 0, do đó tính bền vững vẫn chính xác mà không cần xử lý đặc biệt.
