---
title: "CF 1641C - Ẩn danh là quan trọng"
description: "Chúng tôi có một hàng gồm n người. Mỗi người đều có bệnh tật hoặc khỏe mạnh. Khi thông tin đến, chúng tôi nhận được hai loại tuyên bố. Câu lệnh với x = 0 nói rằng không ai trong khoảng [l, r] bị ốm. Mọi người trong phạm vi đó chắc chắn sẽ trở nên khỏe mạnh."
date: "2026-06-10T04:21:22+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dsu", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1641
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 773 (Div. 1)"
rating: 2200
weight: 1641
solve_time_s: 122
verified: true
draft: false
---

[CF 1641C - Ẩn danh là quan trọng](https://codeforces.com/problemset/problem/1641/C) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, dsu, tham lam, sắp xếp 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng`n`mọi người. Mỗi người đều có bệnh tật hoặc khỏe mạnh. 

Khi thông tin đến, chúng tôi nhận được hai loại tuyên bố. 

Một tuyên bố với`x = 0`nói rằng không có ai trong khoảng thời gian đó`[l, r]`bị ốm. Mọi người trong phạm vi đó chắc chắn sẽ trở nên khỏe mạnh. 

Một tuyên bố với`x = 1`nói rằng có ít nhất một người bệnh ở đâu đó bên trong`[l, r]`. Điều này không xác định ai bị bệnh, nó chỉ đảm bảo rằng khoảng thời gian đó có ít nhất một người bị bệnh. 

Xen kẽ với những câu nói đó là những truy vấn hỏi về một vị trí duy nhất`j`. Chúng ta phải trả lời:`NO`nếu người`j`chắc chắn là khỏe mạnh.`YES`nếu người`j`chắc chắn là bị bệnh.`N/A`nếu cả hai khả năng vẫn phù hợp với tất cả thông tin được thấy cho đến nay. 

Những hạn chế là thách thức thực sự. Cả hai`n`Và`q`lớn như`2 · 10^5`. Bất kỳ giải pháp nào quét một khoảng thời gian trong khi cập nhật hoặc quét tất cả các giới hạn được lưu trữ trong một truy vấn sẽ ngay lập tức trở nên quá chậm. Một cách tiếp cận bậc hai sẽ thực hiện theo thứ tự`4 · 10^10`hoạt động. 

The tricky cases come from the interaction between positive and negative information.

 Giả sử chúng ta biết:```
0 1 2 0
0 1 3 1
```Tuyên bố đầu tiên đưa ra quan điểm`1`Và`2`khỏe mạnh. Câu thứ hai nói rằng có một người bị bệnh ở`[1,3]`. Vì chỉ có vị trí`3`vẫn có thể, câu trả lời cho vị trí`3`là`YES`. 

Một giải pháp ngây thơ chỉ lưu trữ khoảng thời gian`[1,3]`và không bao giờ kết hợp các lần loại bỏ sau này sẽ bỏ lỡ khoản khấu trừ này. 

Một trường hợp tinh tế khác là các khoảng dương lồng nhau.```
[2,10] contains a sick person
[4,6] contains a sick person
```Tuyên bố thứ hai mạnh mẽ hơn. Một khi chúng ta biết`[4,6]`chứa một người bệnh, khoảng cách lớn hơn không đóng góp gì mới. Giữ cả hai khoảng thời gian chỉ tạo ra công việc dư thừa. 

Quan sát đó là chìa khóa cho giải pháp được chấp nhận. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. 

Duy trì địa vị của mỗi người. Với mỗi phát biểu với`x = 0`, đánh dấu toàn bộ khoảng thời gian lành mạnh. Với mỗi phát biểu với`x = 1`, lưu trữ khoảng. Khi được hỏi về vị trí`j`, kiểm tra tất cả các khoảng thời gian được lưu trữ và kiểm tra xem liệu thông tin tích lũy có bị ép buộc hay không`j`bị bệnh. 

Điều này đúng vì nó trực tiếp mô hình hóa các ràng buộc, nhưng nó trở nên chậm một cách vô vọng. Với`2 · 10^5`hoạt động và khoảng thời gian dài`2 · 10^5`, trường hợp xấu nhất lên tới hàng chục tỷ phép tính. 

Quan sát quan trọng là những người khỏe mạnh là những vị trí duy nhất được biết đến vĩnh viễn. Một khi một người được chứng minh là khỏe mạnh, họ có thể bị loại khỏi danh sách xem xét vĩnh viễn. 

Hãy tưởng tượng việc duy trì một tập hợp những người vẫn chưa rõ tình trạng. 

Đối với một tuyên bố tích cực`[l, r]`, tất cả những gì chúng tôi thực sự quan tâm là trong số những vị trí hiện chưa được biết đến trong khoảng thời gian đó, cuối cùng ít nhất một người phải bị ốm. 

Bây giờ hãy xem xét hai khoảng dương trong đó cái này chứa cái kia. 

Nếu như`[4,6]`đã đảm bảo cho một người bệnh rồi`[2,10]`không thêm thông tin mới. Bất kỳ phép gán nào thỏa mãn khoảng nhỏ hơn sẽ tự động thỏa mãn khoảng lớn hơn. 

Vì vậy, chúng ta chỉ cần giữ khoảng cách tích cực tối thiểu trong ngăn chặn. 

Khi một truy vấn hỏi về vị trí`j`, cho phép:`L`= vị trí chưa biết trước đó trước đó`j`

`R`= vị trí chưa biết tiếp theo sau`j`Sau đó`(L, R)`chứa chính xác một người chưa biết, cụ thể là`j`. 

Nếu tồn tại một khoảng dương được lưu trữ hoàn toàn bên trong`(L, R)`và chứa`j`, khoảng đó chỉ còn lại một ứng cử viên. Người bị bệnh bắt buộc phải là`j`. 

Điều đó biến vấn đề thành việc duy trì: 

Một tập hợp năng động của các vị trí vẫn chưa được biết đến. 

Một tập hợp các khoảng dương không dư thừa được sắp xếp theo điểm cuối bên trái của chúng. 

Cả hai đều có thể được xử lý theo thời gian logarit. 

Giải pháp được chấp nhận sử dụng cấu trúc "tiếp theo" giống như DSU để xóa các vị trí lành mạnh một cách hiệu quả và bản đồ có thứ tự các khoảng dương tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n + q) | Quá chậm | 
| Tối ưu | O((n + q) log n) | O(n + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì tập hợp các vị trí chưa rõ trạng thái. 

Ban đầu mọi vị trí đều thuộc về tập hợp này. 
2. Để phát biểu`[l, r]`với`x = 0`, xóa mọi vị trí trong khoảng đó khỏi tập hợp chưa biết. 

Những người đó bây giờ chắc chắn đã khỏe mạnh. 
3. Sử dụng cấu trúc "con trỏ tiếp theo" DSU để bỏ qua các vị trí đã bị xóa. 

Mỗi vị trí sẽ bị xóa nhiều nhất một lần, do đó, về tổng thể, tất cả các lần xóa gần như tuyến tính. 
4. Để phát biểu`[l, r]`với`x = 1`, chèn khoảng vào một bản đồ có thứ tự các ràng buộc dương. 
5. Nếu khoảng thời gian được lưu trữ hiện tại được chứa hoàn toàn trong`[l, r]`, khoảng mới là dư thừa và có thể bỏ qua. 

Khoảng thời gian nhỏ hơn là mạnh mẽ hơn. 
6. Nếu không thì chèn`[l, r]`và xóa mọi khoảng thời gian được lưu trữ trước đó có chứa nó. 

Sau bước này, không có khoảng thời gian được lưu trữ nào chứa khoảng thời gian khác. 
7. Đối với truy vấn về vị trí`j`, trước tiên hãy kiểm tra xem`j`vẫn thuộc tập chưa biết. 

Nếu không thì trả lời`NO`. 
8. Hãy để`L`là vị trí chưa biết trước đó và`R`vị trí chưa biết tiếp theo. 

Vì các vị trí chưa biết được lưu trữ theo thứ tự nên những vị trí lân cận này có thể được tìm thấy theo thời gian logarit. 
9. Tìm kiếm khoảng được lưu trữ đầu tiên có điểm cuối bên trái lớn hơn`L`. 
10. Nếu khoảng thời gian đó bắt đầu vào hoặc trước`j`và kết thúc trước`R`, trả lời`YES`. 

Khoảng đó nằm hoàn toàn bên trong khối có vị trí duy nhất chưa biết là`j`, vì vậy người bệnh bắt buộc phải là`j`. 
11. Ngược lại hãy trả lời`N/A`. 

### Tại sao nó hoạt động 

Tập chưa biết luôn chứa chính xác các vị trí chưa được chứng minh là lành mạnh. 

Cấu trúc khoảng chỉ lưu trữ các ràng buộc dương tối thiểu. Việc loại bỏ một khoảng lớn hơn chứa một khoảng nhỏ hơn không bao giờ làm thay đổi tập hợp các phép gán hợp lệ, bởi vì việc thỏa mãn khoảng nhỏ hơn sẽ tự động thỏa mãn khoảng lớn hơn. 

Đối với một vị trí được truy vấn`j`, hàng xóm`L`Và`R`phân định đoạn tối đa có vị trí duy nhất chưa biết là`j`. Mọi vị trí khác trong phân khúc đó đều được coi là lành mạnh. 

Nếu khoảng dương nằm hoàn toàn bên trong`(L, R)`và chứa`j`, thì khoảng đó chỉ còn lại đúng một ứng cử viên bị bệnh. Kể từ đây`j`chắc là bị bệnh. 

Nếu không tồn tại khoảng như vậy, chúng ta luôn có thể xây dựng một phép gán hợp lệ trong đó`j`khỏe mạnh nên thông tin không đủ để ép buộc bệnh tật. 

Hai sự thật đó khớp chính xác với các câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right
import random

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        n = self.n
        while idx <= n:
            self.bit[idx] += val
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        while idx:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

    def kth(self, k):
        idx = 0
        bitmask = 1 << (self.n.bit_length())
        while bitmask:
            nxt = idx + bitmask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                idx = nxt
            bitmask >>= 1
        return idx + 1

class DSU:
    def __init__(self, n):
        self.p = list(range(n + 3))

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def erase(self, x):
        self.p[x] = self.find(x + 1)

class Node:
    __slots__ = ("l", "r", "prio", "left", "right")

    def __init__(self, l, r):
        self.l = l
        self.r = r
        self.prio = random.randint(1, 1 << 30)
        self.left = None
        self.right = None

def split(root, key):
    if not root:
        return (None, None)
    if root.l < key:
        a, b = split(root.right, key)
        root.right = a
        return (root, b)
    a, b = split(root.left, key)
    root.left = b
    return (a, root)

def merge(a, b):
    if not a or not b:
        return a or b
    if a.prio > b.prio:
        a.right = merge(a.right, b)
        return a
    b.left = merge(a, b.left)
    return b

def insert(root, node):
    if not root:
        return node
    if node.prio > root.prio:
        node.left, node.right = split(root, node.l)
        return node
    if node.l < root.l:
        root.left = insert(root.left, node)
    else:
        root.right = insert(root.right, node)
    return root

def lower_bound(root, key):
    ans = None
    while root:
        if root.l >= key:
            ans = root
            root = root.left
        else:
            root = root.right
    return ans

def predecessor(root, key):
    ans = None
    while root:
        if root.l < key:
            ans = root
            root = root.right
        else:
            root = root.left
    return ans

def erase_key(root, key):
    if not root:
        return None
    if root.l == key:
        return merge(root.left, root.right)
    if key < root.l:
        root.left = erase_key(root.left, key)
    else:
        root.right = erase_key(root.right, key)
    return root

n, q = map(int, input().split())

fw = Fenwick(n + 2)
for i in range(n + 2):
    fw.add(i + 1, 1)

alive = [True] * (n + 2)

dsu = DSU(n + 2)
root = None

out = []

for _ in range(q):
    data = list(map(int, input().split()))

    if data[0] == 0:
        l, r, x = data[1], data[2], data[3]

        if x == 0:
            p = dsu.find(l)
            while p <= r:
                if alive[p]:
                    alive[p] = False
                    fw.add(p + 1, -1)
                dsu.erase(p)
                p = dsu.find(p)

        else:
            cur = lower_bound(root, l)

            if cur and cur.r <= r:
                continue

            root = insert(root, Node(l, r))

            while True:
                prv = predecessor(root, l)
                if prv is None or prv.r < r:
                    break
                root = erase_key(root, prv.l)

    else:
        j = data[1]

        if not alive[j]:
            out.append("NO")
            continue

        rank = fw.sum(j + 1)

        left_pos = fw.kth(rank - 1) - 1
        right_pos = fw.kth(rank + 1) - 1

        cur = lower_bound(root, left_pos + 1)

        if cur and cur.l <= j and cur.r < right_pos:
            out.append("YES")
        else:
            out.append("N/A")

sys.stdout.write("\n".join(out))
```DSU chỉ chịu trách nhiệm xóa khoảng thời gian. Khi một vị trí trở nên lành mạnh, nó sẽ không bao giờ quay trở lại, do đó mọi chỉ mục sẽ bị xóa nhiều nhất một lần. 

Cây Fenwick duy trì tập hợp có thứ tự các vị trí chưa biết. Nó cho phép chúng ta tìm vị trí trước và sau của vị trí được truy vấn thông qua thống kê đơn hàng. 

Tre lưu trữ các khoảng dương tối thiểu được sắp xếp theo điểm cuối bên trái của chúng. Việc chèn sẽ loại bỏ các khoảng dư thừa lớn hơn, bảo toàn bất biến mà không có khoảng nào được lưu trữ chứa khoảng khác. 

Logic truy vấn tuân theo bằng chứng trực tiếp. Nếu một khoảng bị mắc kẹt bên trong khoảng trống có vị trí duy nhất chưa biết là`j`, sau đó`j`phải là người bệnh được yêu cầu trong khoảng thời gian đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 9
0 4 5 0
1 5
1 6
0 4 6 1
1 6
0 2 5 1
0 2 2 0
1 3
1 2
```| Hoạt động | Vị trí không xác định | Khoảng thời gian tích cực được lưu trữ | Trả lời | 
| --- | --- | --- | --- | 
| 0 4 5 0 | {1,2,3,6} | {} | | 
| 1 5 | {1,2,3,6} | {} | KHÔNG | 
| 1 6 | {1,2,3,6} | {} | Không áp dụng | 
| 0 4 6 1 | {1,2,3,6} | {[4,6]} | | 
| 1 6 | {1,2,3,6} | {[4,6]} | CÓ | 
| 0 2 5 1 | {1,2,3,6} | {[2,5]} | | 
| 0 2 2 0 | {1,3,6} | {[2,5]} | | 
| 1 3 | {1,3,6} | {[2,5]} | CÓ | 
| 1 2 | {1,3,6} | {[2,5]} | KHÔNG | 

Thời điểm quan trọng là sau vị trí`2`trở nên khỏe mạnh. Khoảng thời gian`[2,5]`vẫn cần một người bệnh, và chức vụ`3`là ứng cử viên duy nhất còn lại trong khu vực đó. 

### Ví dụ tùy chỉnh```
5 4
0 1 5 1
1 3
0 1 4 0
1 5
```| Hoạt động | Vị trí không xác định | Khoảng thời gian tích cực được lưu trữ | Trả lời | 
| --- | --- | --- | --- | 
| 0 1 5 1 | {1,2,3,4,5} | {[1,5]} | | 
| 1 3 | {1,2,3,4,5} | {[1,5]} | Không áp dụng | 
| 0 1 4 0 | {5} | {[1,5]} | | 
| 1 5 | {5} | {[1,5]} | CÓ | 

Ban đầu bất kỳ vị trí nào cũng có thể thỏa mãn khoảng dương. Sau vị trí`1..4`trở nên khỏe mạnh, vị trí duy nhất`5`vẫn còn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi lần xóa xảy ra một lần, mọi phép toán trên cây đều là logarit | 
| Không gian | O(n + q) | DSU, cây Fenwick và các khoảng được lưu trữ | 

Với`n, q ≤ 2 · 10^5`, các cập nhật và truy vấn logarit dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    # call solve() here in a real test harness
    pass

# sample 1
assert run(
"""6 9
0 4 5 0
1 5
1 6
0 4 6 1
1 6
0 2 5 1
0 2 2 0
1 3
1 2
"""
) == """NO
N/A
YES
YES
NO"""

# minimum size
assert run(
"""1 2
1 1
1 1
"""
) == """N/A
N/A"""

# single forced sick person
assert run(
"""3 3
0 1 2 0
0 1 3 1
1 3
"""
) == "YES"

# everybody healthy
assert run(
"""4 3
0 1 4 0
1 1
1 4
"""
) == """NO
NO"""

# boundary positions
assert run(
"""5 4
0 2 4 0
0 1 5 1
1 1
1 5
"""
) == """N/A
N/A"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ kích thước tối thiểu | Không áp dụng | Không có thông tin | 
| Buộc sống sót | CÓ | Khoảng dương thu gọn về một ứng cử viên | 
| Tất cả đều khỏe mạnh | KHÔNG | Logic xóa phạm vi | 
| Vị trí ranh giới | Không áp dụng | Xử lý tiền nhiệm/kế nhiệm gần kết thúc | 

## Vỏ cạnh 

Hãy xem xét:```
3 3
0 1 2 0
0 1 3 1
1 3
```Sau tuyên bố đầu tiên, các vị trí`1`Và`2`được loại bỏ khỏi tập hợp chưa biết. Khoảng thời gian`[1,3]`vẫn cần một người bệnh. Ứng cử viên duy nhất còn lại là`3`, do đó thuật toán tìm thấy khoảng nằm hoàn toàn bên trong khoảng trống có vị trí duy nhất không xác định là`3`và câu trả lời`YES`. 

Bây giờ hãy xem xét các khoảng lồng nhau:```
5 3
0 1 5 1
0 2 4 1
1 3
```Khoảng thời gian`[2,4]`mạnh hơn`[1,5]`. Thuật toán chỉ giữ khoảng thời gian tối thiểu. Việc loại bỏ khoảng thời gian lớn hơn không thay đổi bất kỳ phép gán hợp lệ nào, đó chính xác là lý do tại sao việc cắt tỉa ngăn chặn là chính xác. 

Cuối cùng, hãy xem xét:```
4 2
0 1 4 0
1 2
```Chức vụ`2`được loại bỏ ngay lập tức khi đến khoảng thời gian lành mạnh. Truy vấn thậm chí không bao giờ kiểm tra các ràng buộc tích cực và trả lời trực tiếp`NO`, đó là kết quả duy nhất có thể xảy ra.
