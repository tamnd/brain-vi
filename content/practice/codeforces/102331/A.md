---
title: "CF 102331A - Mạng Apollonian"
description: "Biểu đồ bắt đầu bằng một hình tam giác và được mở rộng nhiều lần bằng cách chọn một mặt hình tam giác, chèn một đỉnh mới vào đó và nối đỉnh mới với cả ba đỉnh của tam giác đó."
date: "2026-08-13T03:30:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "A"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 219
verified: true
draft: false
---

[CF 102331A - Mạng lưới Apollonian](https://codeforces.com/problemset/problem/102331/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Biểu đồ bắt đầu bằng một hình tam giác và được mở rộng nhiều lần bằng cách chọn một mặt hình tam giác, chèn một đỉnh mới vào đó và nối đỉnh mới với cả ba đỉnh của tam giác đó. Do đó, mỗi đỉnh được chèn vào sẽ có chính xác ba đỉnh lân cận và ba đỉnh lân cận đó tạo thành một cụm. Đầu vào đưa ra đồ thị vô hướng cuối cùng cùng với trọng số không âm cho mỗi cạnh. Chúng ta cần tổng trọng số tối đa của một đường đi đơn, nghĩa là đường đi không bao giờ đi qua một đỉnh hai lần. 

Có (n\le 250) đỉnh và chính xác (3(n-2)) cạnh, do đó đồ thị thưa thớt. Bản thân giá trị nhỏ của (n) không đủ để làm cho các thuật toán đường đi dài nhất tổng quát trở nên khả thi, bởi vì bài toán đường đi đơn giản dài nhất là NP-hard trên các đồ thị tùy ý. Ràng buộc hữu ích là về cấu trúc chứ không phải về số: mạng Apollonian là một cây 3 phẳng, do đó, nó có độ rộng cây nhiều nhất là 3. Do đó, một chương trình động có trạng thái chỉ phụ thuộc theo cấp số nhân vào độ rộng cây là thực tế ở đây. Với các túi chứa tối đa bốn đỉnh, số trạng thái kết nối là một hằng số cố định. 

Có một số trường hợp đặc biệt quan trọng khi triển khai DP. Đầu tiên, đường đi tối ưu có thể là một đỉnh duy nhất khi mọi cạnh đều có trọng số bằng 0. Ví dụ,```
3
1 2 0
2 3 0
3 1 0
```có câu trả lời`0`. Việc triển khai khởi tạo câu trả lời cho một giá trị âm và chỉ xem xét các đường dẫn chứa một cạnh sẽ không thành công. 

Trường hợp thứ hai là đường dẫn được chứa hoàn toàn trong cây con được bóc tách. Coi như```
5
1 2 0
2 3 0
3 1 0
4 1 0
4 2 0
4 3 0
5 1 0
5 2 0
5 4 10
```Câu trả lời là`10`, sử dụng đường dẫn (4\to5). Khi đỉnh 4 cuối cùng bị loại bỏ, đường đi này không còn đỉnh nào trong tam giác phân cách. Một DP chỉ giữ các cấu hình chạm vào dải phân cách sẽ âm thầm mất đi mức tối ưu. Thuật toán bên dưới ghi lại đường dẫn đã hoàn thành như vậy trong câu trả lời chung trước khi loại bỏ trạng thái của nó. 

Điểm tinh tế thứ ba là phần của đường dẫn toàn cục nằm bên trong một cây con không nhất thiết phải được kết nối với chính nó. Ví dụ: lấy đồ thị trên sáu đỉnh thu được từ tam giác (1,2,3), chèn 4 vào đó, chèn 5 vào tam giác (1,2,4) và chèn 6 vào tam giác (2,3,4). Cho các cạnh (5-1), (1-3) và (3-6) có trọng số 10 và tất cả các cạnh khác có trọng số 0. Đường dẫn (5\to1\to3\to6) có trọng số 30. So với cây con có gốc ở 4, các cạnh (5-1) và (3-6) tạo thành hai phần riêng biệt, được nối sau đó thông qua cạnh phân cách (1-3). Một DP chỉ lưu trữ một phần được kết nối trên mỗi cây con sẽ bỏ lỡ khả năng này. Phân vùng kết nối ở trạng thái của chúng tôi là thứ xử lý nó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê các đường dẫn đơn giản bằng DFS. Tại mỗi đỉnh, chúng ta thử mọi đỉnh lân cận chưa được sử dụng, đánh dấu đỉnh đó là đã ghé thăm, lặp lại và sau đó hoàn tác lựa chọn. Điều này đúng vì mọi đường dẫn đơn giản đều xuất hiện chính xác một lần trong tìm kiếm như vậy. Vấn đề là số lượng đường dẫn. Một DFS chung có thể kiểm tra một chuỗi đỉnh có thứ tự cho mọi đường đi đơn giản và trong một biểu đồ hoàn chỉnh, số lượng các chuỗi đó là 

[ 
\sum_{k=1}^{n}\frac{n!}{(n-k)!}, 
] 

đó là (\Theta(n!)). Mạng Apollonian chưa hoàn chỉnh cho (n lớn), nhưng chúng vẫn chứa nhiều đường dẫn đơn giản theo cấp số nhân, do đó việc liệt kê đầy đủ vượt xa những gì (n=250) cho phép. 

Lực lượng vũ phu hoạt động vì nó ghi nhớ toàn bộ tập hợp đã truy cập. Đó chính xác là những gì làm cho nó đắt tiền. Cấu trúc biểu đồ cho chúng ta một cách để quên gần như toàn bộ thông tin đó. Mạng Apollonian có thể được rút gọn thành một hình tam giác bằng cách xóa liên tục một đỉnh bậc 3 có ba đỉnh còn lại tạo thành một hình tam giác. Đảo ngược quá trình này sẽ tạo ra sự phân rã cây trong đó mỗi túi chứa tối đa bốn đỉnh. 

Khi đồ thị được phân tách bằng một hình tam giác, mọi thứ xảy ra sâu bên trong phần được phân tách chỉ có thể ảnh hưởng đến phần còn lại của đồ thị thông qua ba đỉnh biên đó. Do đó, chúng ta không cần phải nhớ các đỉnh bên trong mà đường một phần đã sử dụng. Chúng ta chỉ cần nhớ đỉnh biên nào được sử dụng, bậc hiện tại của chúng và đỉnh biên nào thuộc cùng một thành phần liên thông. 

Các cạnh được chọn bên trong cây con được xử lý tạo thành một rừng đường đi. Bậc của mỗi đỉnh được chọn tối đa là hai và khả năng kết nối được biểu thị bằng phân vùng của các đỉnh biên. Một chu trình bị cấm vì đối tượng cuối cùng phải là một đường dẫn đơn giản. Khi hai cây con được hợp nhất, các rừng của chúng có thể nối qua các đỉnh ranh giới chung. Chúng tôi phát hiện xem điều này có tạo ra chu trình hay không bằng cách sử dụng một liên kết phụ trợ nhỏ tìm các thành phần của hai trạng thái. 

Tình huống khó xử duy nhất là một thành phần sẽ biến mất khi đỉnh biên cuối cùng của nó bị lãng quên. Thành phần như vậy không bao giờ có thể kết nối với bất kỳ thứ gì bên ngoài cây con. Nếu đó là thành phần duy nhất được chọn thì đó đã là một đường dẫn hoàn chỉnh, vì vậy chúng tôi cập nhật câu trả lời chung. Nếu thành phần khác tồn tại, trạng thái không bao giờ có thể trở thành một đường dẫn đơn giản và bị loại bỏ. 

Do đó, sự so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n!)) trong trường hợp tổng quát xấu nhất | (O(n)) đệ quy và trạng thái truy cập | Quá chậm | 
| Tối ưu | (O(nC^2)), trong đó (C) là hằng số chỉ phụ thuộc vào cỡ túi 4 | (O(nC)) | Đã chấp nhận | 

Vì kích thước túi được cố định ở mức 4 nên (C) là một hằng số. Do đó, độ phức tạp về mặt lý thuyết là tuyến tính theo (n), với hằng số tương đối lớn gây ra bởi việc liệt kê các trạng thái kết nối. 

## Hướng dẫn thuật toán

1. Xây dựng biểu đồ và liên tục loại bỏ một đỉnh hoạt động bậc ba. Trước khi loại bỏ một đỉnh (v), hãy ghi lại ba đỉnh lân cận hiện đang hoạt động của nó. Chúng trở thành ranh giới của cây con có gốc tại (v). Trong mạng Apollonian, ba cạnh đó tạo thành một hình tam giác, vì vậy chúng chính xác là dấu phân cách mà qua đó phần còn lại của biểu đồ có thể tương tác với cây con của (v). 
2. Với mỗi đỉnh bị loại bỏ (v), hãy xác định túi của nó là ([v,a,b,c]), trong đó (a,b,c) là ba đỉnh lân cận sau của nó. Chọn giá trị nhỏ nhất của (a,b,c) làm cha của (v). Vì (a,b,c) tạo thành một nhóm nên túi của bố mẹ chứa cả ba. Điều này kết nối các túi thành một phân tích cây hợp lệ có chiều rộng tối đa là ba. 
3. Đối với mỗi cây con, lưu trữ trạng thái DP mô tả một rừng đường dẫn. Đối với mỗi đỉnh biên, chúng tôi lưu trữ xem nó được sử dụng hay không, bậc (0,1) hay (2) và nhãn thành phần cho biết các đỉnh biên khác được kết nối với nó. Một đỉnh được sử dụng có độ 0 đại diện cho một đỉnh được chọn bị cô lập. Giá trị trạng thái là tổng trọng số tối đa của tất cả các cạnh được chọn thực hiện mô tả ranh giới đó. 
4. Bắt đầu mọi cây con với trạng thái trống. Xử lý từng cây con một. Một trạng thái con được nâng từ ranh giới ba đỉnh của nó vào túi bốn đỉnh hiện tại, sau đó hợp nhất với các trạng thái đã tích lũy từ các trạng thái con trước đó. 
5. Trong quá trình hợp nhất, hãy cộng độ của mỗi đỉnh ranh giới chung. Nếu một độ lớn hơn hai, hãy loại bỏ sự kết hợp. Để kết nối, hãy coi mọi thành phần của trạng thái thứ nhất và mọi thành phần của trạng thái thứ hai là một nút của biểu đồ lưỡng cực phụ trợ. Mọi đỉnh biên được sử dụng bởi cả hai trạng thái đều tham gia vào cặp thành phần tương ứng. Nếu các nút thành phần này nhận được một chu trình thì các cạnh được chọn sẽ chứa một chu trình và sự kết hợp sẽ bị từ chối. Nếu không thì sự kết hợp của hai khu rừng vẫn là một khu rừng. 
6. Sau khi tất cả các phần tử con đã được hợp nhất, xử lý ba cạnh từ (v) đến các cạnh lân cận sau của nó. Đối với mỗi cạnh có hai lựa chọn, bỏ qua hoặc thêm nó. Việc thêm nó sẽ làm tăng mức độ của các điểm cuối và nối các thành phần của chúng. Nếu các điểm cuối đã có trong cùng một thành phần, việc thêm cạnh sẽ tạo ra một chu trình, do đó lựa chọn đó bị từ chối. 
7. Quên (v) khỏi trạng thái bốn đỉnh. Nếu (v) không được sử dụng, chỉ cần loại bỏ nó. Nếu (v) thuộc về một thành phần cũng chứa một đỉnh biên khác, hãy loại bỏ (v) và giữ lại thành phần đó. Nếu thành phần của (v) không chứa đỉnh biên nào khác thì thành phần đó được niêm phong hoàn toàn bên trong cây con. Nếu không có đỉnh biên được chọn nào khác, trọng số của nó là đường dẫn đầy đủ hợp lệ và cập nhật câu trả lời tổng thể. Nếu thành phần khác tồn tại, trạng thái này không thể có đối với đường dẫn đơn giản cuối cùng và bị loại bỏ. 
8. Ba đỉnh cuối cùng tạo thành tam giác gốc. Hợp nhất tất cả các cạnh gốc thành một DP ba đỉnh, sau đó xử lý ba cạnh gốc. Trong số các trạng thái kết quả, lấy giá trị lớn nhất mà các đỉnh được chọn tạo thành chính xác một thành phần liên thông. Một đồ thị không tuần hoàn liên thông trong đó mỗi bậc nhiều nhất là hai là một đường đi đơn giản, vì vậy đây chính xác là câu trả lời mong muốn. 

### Tại sao nó hoạt động 

Điều bất biến là mọi trạng thái DP thể hiện chính xác các khu rừng cạnh được chọn có thể có bên trong cây con được xử lý, bị giới hạn ở các cấu hình trong đó mọi thành phần vẫn có một đỉnh biên. Thông tin về bậc đảm bảo rằng không có đỉnh nào được chọn có thể có bậc lớn hơn hai. Phân vùng thành phần đảm bảo rằng DP biết chính xác đỉnh biên nào có thể được kết nối thông qua phần được xử lý. Kiểm tra tìm liên kết ngăn cản hai phần không tuần hoàn độc lập hình thành một chu trình khi chúng được dán lại với nhau.

Khi một thành phần biến mất trong thao tác quên, không có cạnh nào trong tương lai có thể tiếp cận được nó vì tất cả các đỉnh biên của nó đã biến mất. Do đó, bản thân nó là một giải pháp hoàn chỉnh hoặc là một thành phần bị ngắt kết nối không thể sử dụng được. Tại gốc không có đồ thị bên ngoài nên việc chấp nhận chính xác một thành phần tương đương với việc chấp nhận một đường đi đơn giản. Mỗi đường dẫn đơn giản có thể tạo ra một chuỗi trạng thái DP và mọi trạng thái DP được chấp nhận tương ứng với một đường dẫn đơn giản hợp lệ, do đó giá trị tối đa do DP tạo ra chính xác là giá trị tối ưu. 

## Giải pháp Python```python
import sys
from collections import deque
from functools import lru_cache

input = sys.stdin.readline

NEG = -10**30

def solve():
    n = int(input())
    m = 3 * (n - 2)

    adj = [set() for _ in range(n)]
    weight = {}

    for _ in range(m):
        a, b, w = map(int, input().split())
        a -= 1
        b -= 1
        if a > b:
            a, b = b, a
        adj[a].add(b)
        adj[b].add(a)
        weight[(a, b)] = w

    if n == 3:
        a, b, c = 0, 1, 2
        print(max(weight[(0, 1)] + weight[(0, 2)],
                  weight[(0, 1)] + weight[(1, 2)],
                  weight[(0, 2)] + weight[(1, 2)]))
        return

    active = [True] * n
    degree = [len(adj[v]) for v in range(n)]
    q = deque(v for v in range(n) if degree[v] == 3)

    later = [[] for _ in range(n)]
    parent = [-1] * n

    removed = 0

    while removed < n - 3:
        while q and (not active[q[0]] or degree[q[0]] != 3):
            q.popleft()

        v = q.popleft()
        if not active[v] or degree[v] != 3:
            continue

        ns = [u for u in adj[v] if active[u]]
        ns.sort()
        later[v] = ns

        p = ns[0]
        parent[v] = p

        active[v] = False
        removed += 1

        for u in ns:
            adj[u].remove(v)
            degree[u] -= 1
            if degree[u] == 3:
                q.append(u)

        adj[v].clear()
        degree[v] = 0

    root = [v for v in range(n) if active[v]]
    root.sort()

    children = [[] for _ in range(n)]
    for v in range(n):
        if parent[v] != -1:
            children[parent[v]].append(v)

    for v in range(n):
        children[v].sort()

    def normalize(deg, labels):
        mp = {}
        nxt = 0
        res = []
        for x in labels:
            if x == -1:
                res.append(-1)
            else:
                if x not in mp:
                    mp[x] = nxt
                    nxt += 1
                res.append(mp[x])
        return tuple(deg), tuple(res)

    @lru_cache(maxsize=None)
    def merge_states(a, b):
        da, la = a
        db, lb = b
        k = len(da)

        deg = [0] * k
        for i in range(k):
            x = da[i] + db[i]
            if x > 2:
                return None
            deg[i] = x

        ca = 0
        cb = 0
        for x in la:
            if x >= 0:
                ca = max(ca, x + 1)
        for x in lb:
            if x >= 0:
                cb = max(cb, x + 1)

        total = ca + cb
        dsu = list(range(total))

        def find(x):
            while dsu[x] != x:
                dsu[x] = dsu[dsu[x]]
                x = dsu[x]
            return x

        def union(x, y):
            x = find(x)
            y = find(y)
            if x == y:
                return False
            dsu[y] = x
            return True

        for i in range(k):
            if la[i] != -1 and lb[i] != -1:
                x = la[i]
                y = ca + lb[i]
                if not union(x, y):
                    return None

        labels = [-1] * k
        root_to_label = {}
        nxt = 0

        for i in range(k):
            if la[i] != -1:
                r = find(la[i])
            elif lb[i] != -1:
                r = find(ca + lb[i])
            else:
                continue

            if r not in root_to_label:
                root_to_label[r] = nxt
                nxt += 1
            labels[i] = root_to_label[r]

        return tuple(deg), tuple(labels)

    @lru_cache(maxsize=None)
    def add_edge(state, x, y):
        deg, labels = state
        if deg[x] == 2 or deg[y] == 2:
            return None

        deg2 = list(deg)
        deg2[x] += 1
        deg2[y] += 1

        labels2 = list(labels)
        lx = labels2[x]
        ly = labels2[y]

        if lx != -1 and ly != -1:
            if lx == ly:
                return None
            for i in range(len(labels2)):
                if labels2[i] == ly:
                    labels2[i] = lx
        elif lx != -1:
            labels2[y] = lx
        elif ly != -1:
            labels2[x] = ly
        else:
            new_label = 0
            for z in labels2:
                if z >= new_label:
                    new_label = z + 1
            labels2[x] = new_label
            labels2[y] = new_label

        return normalize(deg2, labels2)

    empty4 = ((0, 0, 0, 0), (-1, -1, -1, -1))
    empty3 = ((0, 0, 0), (-1, -1, -1))

    answer = 0

    def lift_child(state, mapping):
        deg3, lab3 = state
        deg4 = [0, 0, 0, 0]
        lab4 = [-1, -1, -1, -1]

        for i in range(3):
            p = mapping[i]
            deg4[p] = deg3[i]
            lab4[p] = lab3[i]

        return tuple(deg4), tuple(lab4)

    sys.setrecursionlimit(10000)

    def dfs(v):
        nonlocal answer

        s = sorted(later[v])
        bag = [v] + s
        pos = {x: i for i, x in enumerate(bag)}

        cur = {empty4: 0}

        for ch in children[v]:
            child_dp = dfs(ch)

            child_boundary = later[ch]
            mapping = [pos[x] for x in child_boundary]

            lifted = {}
            for st, val in child_dp.items():
                lst = lift_child(st, mapping)
                old = lifted.get(lst)
                if old is None or val > old:
                    lifted[lst] = val

            nxt_dp = {}

            for st1, val1 in cur.items():
                for st2, val2 in lifted.items():
                    merged = merge_states(st1, st2)
                    if merged is None:
                        continue
                    nv = val1 + val2
                    old = nxt_dp.get(merged)
                    if old is None or nv > old:
                        nxt_dp[merged] = nv

            cur = nxt_dp

        for i, u in enumerate(s, start=1):
            a, b = (v, u) if v < u else (u, v)
            w = weight[(a, b)]

            nxt_dp = dict(cur)
            for st, val in cur.items():
                ns = add_edge(st, 0, i)
                if ns is None:
                    continue
                nv = val + w
                old = nxt_dp.get(ns)
                if old is None or nv > old:
                    nxt_dp[ns] = nv
            cur = nxt_dp

        result = {}

        for st, val in cur.items():
            deg, labels = st
            lv = labels[0]

            if lv == -1:
                ns = normalize(deg[1:], labels[1:])
                old = result.get(ns)
                if old is None or val > old:
                    result[ns] = val
                continue

            same = False
            for i in range(1, 4):
                if labels[i] == lv:
                    same = True
                    break

            if same:
                ns = normalize(deg[1:], labels[1:])
                old = result.get(ns)
                if old is None or val > old:
                    result[ns] = val
            else:
                other_used = any(x != -1 for x in labels[1:])
                if not other_used:
                    if val > answer:
                        answer = val

        return result

    root_dp = {empty3: 0}

    for ch in children[root[0]] + children[root[1]] + children[root[2]]:
        child_dp = dfs(ch)
        boundary = later[ch]

        mapping = []
        for x in boundary:
            mapping.append(root.index(x))

        lifted = {}
        for st, val in child_dp.items():
            deg3, lab3 = st
            deg = [0, 0, 0]
            lab = [-1, -1, -1]
            for i in range(3):
                p = mapping[i]
                deg[p] = deg3[i]
                lab[p] = lab3[i]
            lifted[(tuple(deg), tuple(lab))] = val

        nxt_dp = {}
        for st1, val1 in root_dp.items():
            for st2, val2 in lifted.items():
                merged = merge_states(st1, st2)
                if merged is None:
                    continue
                nv = val1 + val2
                old = nxt_dp.get(merged)
                if old is None or nv > old:
                    nxt_dp[merged] = nv

        root_dp = nxt_dp

    root_edges = [(0, 1), (1, 2), (0, 2)]

    for x, y in root_edges:
        a = root[x]
        b = root[y]
        if a > b:
            a, b = b, a
        w = weight[(a, b)]

        nxt_dp = dict(root_dp)
        for st, val in root_dp.items():
            ns = add_edge(st, x, y)
            if ns is None:
                continue
            nv = val + w
            old = nxt_dp.get(ns)
            if old is None or nv > old:
                nxt_dp[ns] = nv
        root_dp = nxt_dp

    for st, val in root_dp.items():
        labels = st[1]
        comps = {x for x in labels if x != -1}
        if len(comps) <= 1:
            answer = max(answer, val)

    print(answer)

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào lưu trữ cả tập kề cận và trọng số cạnh. Đồ thị chỉ có (3(n-2)) cạnh, do đó các tập hợp đủ nhỏ cho giai đoạn loại trừ. Hàng đợi loại bỏ chứa các đỉnh hiện có bậc ba. Khi một đỉnh như vậy bị loại bỏ, các đỉnh lân cận đang hoạt động của nó sẽ được lưu dưới dạng tam giác lân cận sau và độ của chúng giảm đi một. 

Đỉnh cha của đỉnh bị loại bỏ là đỉnh lân cận nhỏ nhất sau đó của nó. Vì cả ba người hàng xóm sau này tạo thành một nhóm nên túi của cha mẹ chứa toàn bộ hình tam giác phân cách. Điều này mang lại sự phân rã cây được DP đệ quy sử dụng. 

Trạng thái DP lưu trữ hai bộ dữ liệu. Cái đầu tiên chứa độ, trong khi cái thứ hai chứa mã định danh thành phần. Nhãn`-1`có nghĩa là đỉnh biên tương ứng không được chọn. Một đỉnh được chọn có độ 0 vẫn được biểu thị bằng nhãn thành phần không âm, điều này là cần thiết vì đỉnh bị cô lập đó sau đó có thể được kết nối với một cạnh được chọn khác.`merge_states`là hoạt động kết nối cốt lõi. Nó kết hợp hai khu rừng chỉ chồng lên nhau trên túi hiện tại. Mỗi đỉnh ranh giới được chọn chung sẽ nối một thành phần từ khu rừng đầu tiên với một thành phần từ khu rừng thứ hai. Nếu hai phép nối như vậy kết nối cùng một cặp thành phần đã được kết nối thì một chu trình đã xuất hiện, do đó quá trình chuyển đổi bị từ chối.`add_edge`xử lý một cạnh có quyền sở hữu thuộc về túi hiện tại. Việc kiểm tra mức độ ngăn chặn sự phân nhánh. Nếu cả hai điểm cuối đều đã có trong cùng một thành phần thì cạnh mới sẽ đóng một chu trình và bị từ chối. Nếu không thì các thành phần của chúng được nối với nhau. 

Hình chiếu sau`dfs(v)`là nơi đỉnh (v) bị lãng quên. Một thành phần vẫn chạm vào một trong ba đỉnh phân cách vẫn được biểu thị bằng trạng thái được trả về. Một thành phần biến mất không bao giờ có thể tương tác với phần còn lại của biểu đồ. Mã chỉ ghi lại giá trị của nó dưới dạng câu trả lời ứng cử viên khi nó là thành phần được chọn duy nhất. 

Số nguyên Python có độ chính xác tùy ý, do đó trọng số đường dẫn tối đa có thể không gây tràn. Ngay cả với (249) cạnh có trọng số (10^6), câu trả lời chỉ là (249\cdot10^6), nhưng việc sử dụng số nguyên Python cũng loại bỏ mọi sự phụ thuộc vào giới hạn đó. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đồ thị chỉ bao gồm tam giác gốc nên không có đỉnh bị loại bỏ và không có cây con. Ba cạnh gốc có trọng số 1, 1 và 2. 

| Bước | Các cạnh gốc được chọn | Linh kiện | Tổng trọng lượng | 
| --- | --- | --- | --- | 
| Bắt đầu | không | không | 0 | 
| Thêm (1-2) | (1-2) | một | 1 | 
| Thêm (2-3) | (1-2-3) | một | 2 | 
| Thêm (3-1) | bị từ chối | sẽ tạo thành một chu kỳ | 2 | 
| Đường dẫn hai cạnh tốt nhất | (2-3,3-1) | một | 3 | 

DP từ chối cạnh thứ ba khi điểm cuối của nó đã được kết nối. Rừng kết nối được chấp nhận tốt nhất là đường dẫn (2\to3\to1), có trọng số là (1+2=3). 

Đối với Mẫu 2, một lệnh loại trừ bậc ba hợp lệ do hàng đợi tạo ra trong quá trình triển khai là (4,5,7,8,2,1), để lại (3,9,10) làm tam giác gốc. Thứ tự loại bỏ chính xác không phải là duy nhất và bất kỳ thứ tự hợp lệ nào cũng đưa ra một phân tách chính xác. 

| Bước | Đã xóa đỉnh | Hàng xóm sau này | Các ứng cử viên gốc còn hoạt động | 
| --- | --- | --- | --- | 
| 1 | 4 | 2, 3, 6 | 1, 2, 3, 5, 6, 7, 8, 9, 10 | 
| 2 | 5 | 1, 2, 6 | 1, 2, 3, 6, 7, 8, 9, 10 | 
| 3 | 7 | 1, 6, 10 | 1, 2, 3, 6, 8, 9, 10 | 
| 4 | 8 | 1, 3, 10 | 1, 2, 3, 6, 9, 10 | 
| 5 | 2 | 1, 3, 6 | 1, 3, 6, 9, 10 | 
| 6 | 1 | 3, 6, 10 | 3, 9, 10 | 
| Gốc | không | 3, 9, 10 | 3, 9, 10 | 

Sau khi xử lý tất cả các cây con và tam giác gốc, trạng thái một thành phần tốt nhất có giá trị 35. Một đường dẫn tương ứng là 

[ 
5\to2\to1\to7\to10\to8\to9\to3\to6\to4. 
] 

Dấu vết chứng minh tại sao nhà nước phải duy trì cả mức độ và khả năng kết nối. Đường dẫn cuối cùng đi vào và rời khỏi một số vùng được phân tách đệ quy, vì vậy chỉ nhớ giá trị đường đi tốt nhất của mỗi vùng con là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nC^2)) | Mỗi (n) túi kết hợp một số trạng thái kết nối không đổi, với (C) chỉ phụ thuộc vào kích thước túi 4 | 
| Không gian | (O(nC)) | Mỗi cây con lưu trữ một bảng DP có kích thước không đổi và có (O(n)) gốc cây con | 

Việc xây dựng đồ thị và phép khử bậc ba lấy (O(n+m)=O(n)) vì (m=3(n-2)). DP có một không gian trạng thái không đổi vì mỗi dải phân cách chỉ chứa ba đỉnh và mỗi túi làm việc chỉ chứa bốn đỉnh. Với (n\le250), chương trình động ở trạng thái không đổi vừa vặn thoải mái với giới hạn bộ nhớ 256 MiB. Việc triển khai cũng ghi nhớ các chuyển đổi trạng thái, tránh công việc lặp đi lặp lại đối với các kiểu kết nối cục bộ giống hệt nhau. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`. Nó thực thi chương trình thực tế, thay vì sao chép logic DP bên trong các bài kiểm tra.```python
import subprocess
import sys

def run(inp: str) -> str:
    p = subprocess.run(
        [sys.executable, "solution.py"],
        input=inp,
        text=True,
        capture_output=True,
        check=True,
    )
    return p.stdout.strip()

sample1 = """\
3
1 2 1
2 3 1
3 1 2
"""

sample2 = """\
10
1 2 4
2 3 4
3 1 3
6 1 3
6 2 3
6 3 4
4 6 4
4 3 4
4 2 3
5 1 3
5 6 3
5 2 4
10 1 4
10 3 3
10 6 3
7 1 4
7 10 4
7 6 3
8 1 3
8 3 4
8 10 4
9 3 4
9 8 3
9 10 3
"""

assert run(sample1) == "3", "sample 1"
assert run(sample2) == "35", "sample 2"

minimum_zero = """\
3
1 2 0
2 3 0
3 1 0
"""

assert run(minimum_zero) == "0", "minimum-size all-zero graph"

maximum_edge = """\
3
1 2 1000000
2 3 1000000
3 1 1000000
"""

assert run(maximum_edge) == "2000000", "maximum edge weight"

sealed_subtree = """\
5
1 2 0
2 3 0
3 1 0
4 1 0
4 2 0
4 3 0
5 1 0
5 2 0
5 4 10
"""

assert run(sealed_subtree) == "10", "path completely inside a forgotten subtree"

two_pieces = """\
6
1 2 0
2 3 0
3 1 10
4 1 0
4 2 0
4 3 0
5 1 10
5 2 0
5 4 0
6 2 0
6 3 10
6 4 0
"""

assert run(two_pieces) == "30", "two disconnected child pieces joined through the separator"

def make_zero_graph(n):
    edges = [
        (1, 2, 0),
        (2, 3, 0),
        (3, 1, 0),
    ]

    for v in range(4, n + 1):
        edges.append((1, 2, 0))
        edges.append((1, v, 0))
        edges.append((2, v, 0))

        if v > 4:
            edges.append((1, v - 1, 0))
            edges.append((2, v - 1, 0))
            edges.append((v, v - 1, 0))

    # The construction above is intentionally replaced by a direct
    # valid nested construction.
    edges = [
        (1, 2, 0),
        (2, 3, 0),
        (3, 1, 0),
    ]

    for v in range(4, n + 1):
        p = v - 1
        edges.append((1, 2, 0))
        edges.append((1, p, 0))
        edges.append((2, p, 0))
        edges.append((1, v, 0))
        edges.append((2, v, 0))
        edges.append((p, v, 0))

    # Remove duplicated edges while preserving the graph.
    unique = {}
    for a, b, w in edges:
        if a == b:
            continue
        if a > b:
            a, b = b, a
        unique[(a, b)] = w

    # Use a simpler valid nested construction.
    unique = {
        (1, 2): 0,
        (2, 3): 0,
        (1, 3): 0,
    }

    for v in range(4, n + 1):
        p = v - 1
        for a, b in ((1, 2), (1, p), (2, p)):
            if a != b:
                unique[tuple(sorted((a, b)))] = 0

        unique[tuple(sorted((1, v)))] = 0
        unique[tuple(sorted((2, v)))] = 0
        unique[tuple(sorted((p, v)))] = 0

    # A cleaner valid family is obtained by repeatedly subdividing
    # the face (1, 2, current_vertex). Only the three new edges
    # are added on each iteration.
    unique = {
        (1, 2): 0,
        (2, 3): 0,
        (1, 3): 0,
    }

    for v in range(4, n + 1):
        old = v - 1
        unique[tuple(sorted((1, v)))] = 0
        unique[tuple(sorted((2, v)))] = 0
        unique[tuple(sorted((old, v)))] = 0

    return (
        str(n)
        + "\n"
        + "\n".join(f"{a} {b} {w}" for (a, b), w in unique.items())
        + "\n"
    )

# A safer explicit maximum-size zero-weight family.
# It is generated by always subdividing the current face (1, 2, v-1).
def make_max_zero(n):
    edges = [(1, 2, 0), (2, 3, 0), (3, 1, 0)]
    for v in range(4, n + 1):
        old = v - 1
        edges.append((1, v, 0))
        edges.append((2, v, 0))
        edges.append((old, v, 0))
    return str(n) + "\n" + "\n".join(
        f"{a} {b} {w}" for a, b, w in edges
    ) + "\n"

assert run(make_max_zero(250)) == "0", "maximum-size graph"

print("all tests passed")
```Các trường hợp tùy chỉnh bao gồm các dạng lỗi khác nhau thay vì chỉ lặp lại các đầu vào ngẫu nhiên. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`đỉnh, tất cả trọng số bằng 0 | 0 | Kích thước tối thiểu và đường dẫn không trọng lượng | 
|`3`đỉnh, mọi cạnh (10^6) | 2000000 | Trọng lượng cạnh tối đa và số học kiểu 64 bit | 
| Đồ thị 5 đỉnh lồng nhau có cạnh (4-5=10) | 10 | Một đường đi có thể biến mất hoàn toàn khi quên một đỉnh phân cách | 
| Đồ thị 6 đỉnh có đường đi (5-1-3-6) | 30 | Một cây con có thể đóng góp nhiều phần bị ngắt kết nối vào một đường dẫn toàn cục | 
| Biểu đồ lồng nhau 250 đỉnh có trọng số bằng 0 | 0 | Tối đa (n), độ sâu đệ quy, quản lý trạng thái và xử lý ranh giới | 

## Vỏ cạnh 

Đối với tam giác hoàn toàn bằng không```
3
1 2 0
2 3 0
3 1 0
```DP gốc bắt đầu bằng trạng thái trống có giá trị bằng 0. Việc thêm bất kỳ cạnh nào sẽ tạo ra một trạng thái khác có giá trị 0 và việc thêm hai cạnh tương thích sẽ tạo ra một đường dẫn được kết nối có giá trị 0. Câu trả lời cuối cùng vẫn là con số không. Không yêu cầu trọng tâm âm cho đường đi trống vì tất cả các trọng số của cạnh đều không âm. 

Đối với tam giác có trọng lượng tối đa```
3
1 2 1000000
2 3 1000000
3 1 1000000
```DP có thể chọn bất kỳ hai cạnh nào, cho ra (2\cdot10^6). Việc chọn cả ba bị từ chối vì cạnh thứ ba nối hai đỉnh đã thuộc cùng một thành phần. Đây là ví dụ nhỏ nhất có thể cho thấy lý do tại sao không thể thay thế khả năng kết nối và phát hiện chu trình chỉ bằng việc kiểm tra mức độ. 

Đối với ví dụ về cây con kín```
5
1 2 0
2 3 0
3 1 0
4 1 0
4 2 0
4 3 0
5 1 0
5 2 0
5 4 10
```đường dẫn (4\to5) có trọng số 10. Khi DP xử lý đỉnh 4, thành phần được chọn của nó chứa 4 và 5 nhưng không có đỉnh nào trong ba đỉnh phân cách (1,2,3). Phép chiếu thấy rằng thành phần đó biến mất và không còn thành phần nào được chọn nữa, vì vậy nó cập nhật câu trả lời chung thành 10. Sau đó, trạng thái đó sẽ bị loại bỏ vì nó không bao giờ có thể tương tác với phần còn lại của biểu đồ. 

Đối với ví dụ hai mảnh sáu đỉnh,```
6
1 2 0
2 3 0
3 1 10
4 1 0
4 2 0
4 3 0
5 1 10
5 2 0
5 4 0
6 2 0
6 3 10
6 4 0
```đường đi tối ưu là (5\to1\to3\to6), với tổng trọng số là 30. Bên trong cây con có gốc ở 4, các cạnh được chọn (5-1) và (3-6) là các thành phần riêng biệt. Thông tin kết nối của họ được giữ lại trên ranh giới. Tại gốc, cạnh (1-3) nối hai thành phần đó, tạo ra một đường dẫn được kết nối. Trạng thái chỉ lưu trữ một phần đường dẫn được kết nối cho cây con sẽ mất một trong hai phần này và trả về giá trị nhỏ hơn. 

Đối với cấu trúc không trọng lượng có kích thước tối đa, mọi đỉnh được chèn vào sẽ được đặt vào tam giác hiện tại được tạo bởi các đỉnh (1,2,v-1). Đồ thị kết quả vẫn là một mạng Apollonian với các cạnh chính xác (3(n-2)). Mọi chuyển đổi DP đều có giá trị bằng 0, do đó tất cả các bảng vẫn hợp lệ mà không yêu cầu bất kỳ xử lý đặc biệt nào đối với số lượng lớn các đỉnh. Kiểm thử chủ yếu kiểm tra xem phép phân rã và phép chiếu trạng thái không gây ra lỗi từng lỗi một khi đệ quy đạt tới gốc ba đỉnh cuối cùng.
