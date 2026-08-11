---
title: "CF 102399I - \u0416\u0443\u043b\u0438\u043a, \u043d\u0435 \u0432\u043e\u0440\u0443\u0439"
description: "Chúng ta có một đồ thị vô hướng đơn giản được kết nối. Swiper chọn một tập hợp các đỉnh thích hợp khác rỗng và loại bỏ các đỉnh đó cùng với tất cả các cạnh liên quan. Các đỉnh còn lại phải giữ nguyên độ modulo (3) mà chúng có trước khi loại bỏ."
date: "2026-08-10T17:23:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102399
codeforces_index: "I"
codeforces_contest_name: "2019 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u043b\u0438\u0433\u0430 A"
rating: 0
weight: 102399
solve_time_s: 671
verified: true
draft: false
---

[CF 102399I - \u0416\u0443\u043b\u0438\u043a, \u043d\u0435 \u0432\u043e\u0440\u0443\u0439](https://codeforces.com/problemset/problem/102399/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng đơn giản được kết nối. Swiper chọn một tập hợp các đỉnh thích hợp khác rỗng và loại bỏ các đỉnh đó cùng với tất cả các cạnh liên quan. Các đỉnh còn lại phải giữ nguyên độ modulo (3) mà chúng có trước khi loại bỏ. 

Gọi (S) là các đỉnh bị đánh cắp và gọi (R=V\setminus S) là các đỉnh còn lại. Với mọi (v\in R), chính xác các cạnh từ (v) đến (S) biến mất. Do đó điều kiện là 

[ 
\deg_S(v)\equiv 0\pmod 3. 
] 

Vì (\deg_G(v)=\deg_R(v)+\deg_S(v)), điều kiện tương tự có thể được viết là 

[ 
\deg_R(v)\equiv\deg_G(v)\pmod 3. 
] 

Hình thức thứ hai này dễ xây dựng hơn nhiều. Chúng ta sẽ xây dựng một tập hợp (R) các đỉnh còn lại phù hợp, sau đó đánh cắp mọi đỉnh bên ngoài (R). 

Phiên bản Codeforces ban đầu có (n,m\le 500000), với tổng (n) và (m) trên tất cả các trường hợp thử nghiệm cũng được giới hạn bởi (500000). Giới hạn cuộc thi ban đầu là 2 giây và 512 MB. Điều này ngay lập tức loại trừ mọi thứ bậc hai trong kích thước biểu đồ và ngay cả việc truyền tải lặp đi lặp lại một số lần tuyến tính cũng sẽ quá tốn kém. Mục tiêu là (O(n+m)) trên mỗi biểu đồ, với số lần duyệt biểu đồ không đổi. 

Có một số trường hợp nhỏ trong đó việc triển khai có thể âm thầm tạo ra câu trả lời không hợp lệ. Với một đỉnh,```
1
1 0
```biểu đồ không có tập hợp con khác trống thích hợp để lấy trộm, vì vậy câu trả lời là`No`. Một chương trình mù quáng nhìn thấy một mức chia hết cho (3) và giữ đỉnh đó sẽ vô tình cố gắng lấy trộm các đỉnh bằng 0. 

Đối với hai đỉnh được kết nối,```
1
2 1
1 2
```cả hai độ đều là (1). Tập bị đánh cắp đúng khác rỗng duy nhất có một đỉnh, nhưng đầu ra yêu cầu ít nhất hai đỉnh bị đánh cắp. Câu trả lời đúng là`No`. 

Một chu kỳ cũng cần được điều trị đặc biệt. Vì```
1
4 4
1 2
2 3
3 4
4 1
```mọi đỉnh đều có bậc (2). Toàn bộ biểu đồ là một chu trình được giữ lại hợp lệ, nhưng việc giữ lại toàn bộ biểu đồ có nghĩa là không lấy đi thứ gì. Bất kỳ tập hợp con thực sự nào của một chu trình đều chứa một đường đi có điểm cuối có bậc bên trong (1), không khớp với phần dư ban đầu (2). Như vậy câu trả lời là`No`. 

Trường hợp tinh tế thứ tư là đồ thị có đỉnh có bậc chia hết cho (3). Ví dụ,```
1
4 3
1 2
1 3
1 4
```đỉnh (1) có bậc (3). Chỉ giữ lại đỉnh (1) là hợp lệ vì cả ba cạnh liên quan đều biến mất và (3\equiv0\pmod3). Do đó, ba chiếc lá có thể bị đánh cắp. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp là khái niệm đơn giản. Liệt kê mọi tập con đúng (S) của các đỉnh, đếm cho mỗi đỉnh bên ngoài (S) có bao nhiêu đỉnh lân cận của nó thuộc về (S) và kiểm tra xem mỗi số đó có chia hết cho (3) hay không. Điều này đúng vì nó kiểm tra chính xác định nghĩa về hành vi trộm cắp hợp pháp. Có (2^n-2) tập hợp con có thể có và việc kiểm tra một tập hợp con mất (O(n+m)) thời gian nếu biểu đồ được biểu thị bằng danh sách kề. Do đó, độ phức tạp trong trường hợp xấu nhất là 

[ 
O(2^n(n+m)). 
] 

Với (n=500000), điều này không chỉ là quá chậm mà còn hoàn toàn không khả thi. 

Quan sát hữu ích là ngừng suy nghĩ về các tập hợp con tùy ý. Đối với mọi đỉnh còn lại (v), bậc bên trong của nó phải có cùng modulo dư (3) như bậc ban đầu của nó. Điều đó có nghĩa là các đỉnh có thặng dư bậc gốc (0), (1) và (2) gợi ý một cách tự nhiên các cấu trúc nhỏ khác nhau có thể được giữ lại. 

Gọi ba loại này (Z,A,B), theo độ modulo (3). 

A(Z)-đỉnh có thể được giữ riêng. Mức độ bên trong của nó là (0), chính xác là dư lượng cần thiết. 

Hai đỉnh (A) được nối với nhau bằng một đường thích hợp có thể được giữ lại. Hai điểm cuối có một cạnh trong và mỗi đỉnh (B) trong có hai cạnh trong. Đường đi ngắn nhất giữa hai đỉnh (A) cho ta chính xác cấu trúc này. 

Một chu trình gồm các đỉnh (B) có thể được giữ nguyên. Mỗi đỉnh chu kỳ có hai cạnh bên trong, phần dư tương ứng (2). Việc chọn chu trình ngắn nhất sẽ làm cho nó không có dây nối, vì vậy mỗi đỉnh chu trình thực sự có chính xác hai đỉnh lân cận trong tập đã chọn. 

Nếu không có cấu trúc nào trong số đó tồn tại thì có chính xác một đỉnh (A) và các đỉnh (B) tạo thành một khu rừng. Mỗi thành phần của cây phải chạm vào đỉnh (A) duy nhất ít nhất hai lần. Hai thành phần như vậy tạo ra hai đường đi từ đỉnh (A) trở lại chính nó, tạo thành hai chu trình chỉ có chung đỉnh (A). Giữ hai đường đi đó và đỉnh (A) sẽ có bậc bên trong (4) tại đỉnh (A), lại là (1\bmod3), trong khi mọi đỉnh (B) trên các đường đi đều có bậc bên trong (2). 

Những trường hợp này là đầy đủ. Vấn đề chính thức là vấn đề Codeforces 1239F và sự phân loại này là ý tưởng mang tính xây dựng trọng tâm đằng sau các giải pháp được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n(n+m))) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(n+m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính bậc của mỗi đỉnh và phân loại theo`degree % 3`. Ba lớp là (Z), (A) và (B). Việc phân loại cho chúng ta biết mức độ bên trong mà một đỉnh được giữ lại phải có modulo (3). 
2. Nếu có đỉnh (Z)-(v) thì chỉ giữ lại (v). Bậc bên trong của nó bằng 0, khớp với modulo bậc ban đầu của nó (3). Nếu (n>2), việc đánh cắp mọi đỉnh khác sẽ cho câu trả lời hợp lệ. Vì (n=1) không có gì để đánh cắp và (n=2) không thể chứa đỉnh (Z) trong đồ thị liên thông. 
3. Tìm chu trình hoàn toàn bên trong đỉnh (B). Một chu trình cung cấp cho mỗi đỉnh được chọn chính xác hai lân cận được chọn, đó là phần dư cần thiết cho một -đỉnh (B). Chúng ta cần chu trình không có dây âm, vì vậy chúng ta tìm chu trình ngắn nhất theo nghĩa cây DFS. Một cạnh không phải là cây giữa tổ tiên và hậu duệ sẽ tạo ra một chu trình cơ bản và việc chọn một cạnh có khoảng cách cây tối thiểu sẽ tạo ra một chu trình không có dây cung. 
4. Nếu một chu trình như vậy tồn tại và không sử dụng mọi đỉnh, hãy lấy phần bù của nó. Nếu chu trình là toàn bộ đồ thị thì đồ thị chính xác là một chu trình và không có nghiệm thích hợp nào tồn tại thông qua cách xây dựng này. Chúng tôi chỉ tiếp tục các trường hợp tiếp theo nếu phần bù không đáp ứng được kích thước đầu ra được yêu cầu. 
5. Nếu có ít nhất hai đỉnh (A), hãy chạy BFS từ một đỉnh (A) thông qua biểu đồ và dừng khi đạt đến đỉnh (A) khác. Đường đi kết quả là ngắn nhất nên nó không thể chứa đỉnh (A) khác bên trong. Hai điểm cuối của nó có một lân cận bên trong, trong khi các đỉnh (B) bên trong của nó có hai lân cận bên trong. Giữ con đường này và đánh cắp mọi thứ khác. 
6. Nếu việc xây dựng trước đó không thành công thì không có đỉnh (Z), không có chu trình (B) và có nhiều nhất một đỉnh (A). Bởi vì đồ thị được kết nối và có nhiều hơn một đỉnh nên thực tế phải có chính xác một đỉnh (A). Mọi đỉnh khác đều là (B) và đồ thị con được tạo ra bởi các đỉnh (B) là một rừng. 
7. Hãy xem xét một thành phần được kết nối (T) của rừng (B) này. Giả sử nó có các cạnh (r) đi từ đỉnh của nó đến đỉnh (A) duy nhất. Vì mọi đỉnh (B) đều có bậc (2\bmod3), 

[ 
2|V(T)|\equiv 2|E(T)|+r\pmod3. 
] 

Vì (T) là một cây nên (|E(T)|=|V(T)|-1). Thay thế mang lại 

[ 
r\equiv2\pmod3. 
] 

Do đó, mỗi cây (B) có ít nhất hai cạnh so với đỉnh (A). Cũng phải có ít nhất hai thành phần như vậy, vì bậc của đỉnh (A) duy nhất là (1\bmod3), trong khi một thành phần sẽ đóng góp (2\bmod3). 

1. Trong hai thành phần (B) khác nhau, chọn hai đỉnh liền kề với đỉnh (A). Trong mỗi thành phần, bắt đầu từ một đỉnh như vậy và tìm đỉnh khác gần nhất liền kề với (A). Đường đi giữa chúng không chứa lân cận (A) nào khác bên trong, vì vậy việc thêm đỉnh (A) sẽ biến đường dẫn đó thành một chu trình sạch. Giữ cả hai đường đi và đỉnh (A). 
2. Nếu tập được giữ lại là đúng và để lại ít nhất hai đỉnh bị đánh cắp, hãy xuất phần bù của nó. Nếu đó là toàn bộ biểu đồ, thì biểu đồ bao gồm chính xác hai đường dẫn (A)-đến-(A) nối tại (A) và không có tập hợp pháp lý nào được giữ lại tồn tại trong trường hợp cuối cùng này. Các công trình xây dựng trước đó chắc chắn đã xử lý được mọi khả năng khác. 

Tại sao nó hoạt động: đối với mỗi công trình, cấp độ bên trong bộ được giữ lại có phần dư chính xác giống như cấp độ ban đầu. Đỉnh A(Z) có bậc trong (0). Đường dẫn (A)-đến-(A) được giữ lại cho mức độ bên trong (1) tại các điểm cuối của nó và (2) tại các đỉnh (B)-bên trong của nó. Chu kỳ không dây (B) cho mức độ bên trong (2) ở mọi nơi. Trong cấu trúc cuối cùng, mỗi đường dẫn (B) được chọn có bậc bên trong (2) tại các đỉnh (B) của nó, trong khi đỉnh (A) duy nhất có bốn lân cận được chọn. Vì (4\equiv1\pmod3), phần dư của nó cũng được bảo toàn.

Đối số cạn kiệt tuân theo cấu trúc tương tự. Nếu một đỉnh (Z) tồn tại thì công trình đầu tiên sẽ hoạt động. Ngược lại, chu trình (B) sẽ đưa ra cách xây dựng thứ hai. Ngược lại, hai đỉnh (A) sẽ cho kết cấu thứ ba. Nếu không có điều nào trong số này xảy ra thì có chính xác một đỉnh (A) và đồ thị con (B) là một khu rừng, tạo nên cấu trúc cuối cùng. Nếu cấu trúc cuối cùng đó chiếm toàn bộ biểu đồ, thì bất kỳ tập hợp được giữ lại hợp lệ nào cũng sẽ phải chứa đỉnh (A) duy nhất và ít nhất hai thành phần (B), buộc cả hai đường dẫn hoàn chỉnh phải được giữ nguyên, do đó không có giải pháp thích hợp nào tồn tại. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def make_answer(n, keep):
    mark = bytearray(n)
    for v in keep:
        mark[v] = 1

    stolen = [v + 1 for v in range(n) if not mark[v]]

    if 1 < len(stolen) < n:
        return stolen
    return None

def find_b_cycle(g, typ):
    n = len(g)

    color = bytearray(n)
    parent = [-1] * n
    depth = [0] * n
    tin = [-1] * n
    tout = [-1] * n

    timer = 0

    for s in range(n):
        if typ[s] != 2 or color[s]:
            continue

        color[s] = 1
        tin[s] = timer
        timer += 1

        stack = [(s, 0)]

        while stack:
            u, idx = stack[-1]

            if idx == len(g[u]):
                color[u] = 2
                tout[u] = timer
                stack.pop()
                continue

            v = g[u][idx]
            stack[-1] = (u, idx + 1)

            if typ[v] != 2:
                continue

            if color[v] == 0:
                parent[v] = u
                depth[v] = depth[u] + 1
                color[v] = 1
                tin[v] = timer
                timer += 1
                stack.append((v, 0))

    best_anc = -1
    best_desc = -1
    best_diff = 10**18

    for u in range(n):
        if typ[u] != 2:
            continue

        for v in g[u]:
            if v <= u or typ[v] != 2:
                continue

            if parent[v] == u or parent[u] == v:
                continue

            if tin[u] <= tin[v] < tout[u]:
                anc, desc = u, v
            elif tin[v] <= tin[u] < tout[v]:
                anc, desc = v, u
            else:
                continue

            diff = depth[desc] - depth[anc]

            if diff < best_diff:
                best_diff = diff
                best_anc = anc
                best_desc = desc

    if best_anc == -1:
        return None

    cycle = []
    x = best_desc

    while x != best_anc:
        cycle.append(x)
        x = parent[x]

    cycle.append(best_anc)
    return cycle

def find_a_path(g, typ):
    n = len(g)
    start = -1

    for v in range(n):
        if typ[v] == 1:
            start = v
            break

    if start == -1:
        return None

    parent = [-2] * n
    parent[start] = -1
    q = deque([start])

    target = -1

    while q:
        u = q.popleft()

        for v in g[u]:
            if typ[v] == 0 or parent[v] != -2:
                continue

            parent[v] = u

            if typ[v] == 1:
                target = v
                q.clear()
                break

            q.append(v)

        if target != -1:
            break

    if target == -1:
        return None

    path = []
    x = target

    while x != -1:
        path.append(x)
        x = parent[x]

    path.reverse()
    return path

def find_tree_path_to_attachment(g, start, attach):
    n = len(g)

    parent = [-2] * n
    parent[start] = -1
    q = deque([start])
    target = -1

    while q:
        u = q.popleft()

        for v in g[u]:
            if parent[v] != -2:
                continue

            if attach[v] == 0:
                parent[v] = u
                q.append(v)
            elif v != start:
                parent[v] = u
                target = v
                q.clear()
                break

        if target != -1:
            break

    if target == -1:
        return None

    path = []
    x = target

    while x != -1:
        path.append(x)
        x = parent[x]

    path.reverse()
    return path

def solve_case(n, m):
    g = [[] for _ in range(n)]
    deg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)
        deg[u] += 1
        deg[v] += 1

    typ = [d % 3 for d in deg]

    # Case 1: a degree-0-mod-3 vertex.
    for v in range(n):
        if typ[v] == 0:
            keep = [v]
            ans = make_answer(n, keep)
            if ans is not None:
                return ans

    # Case 2: a cycle consisting only of degree-2-mod-3 vertices.
    cycle = find_b_cycle(g, typ)

    if cycle is not None:
        ans = make_answer(n, cycle)
        if ans is not None:
            return ans

    # Case 3: a path between two degree-1-mod-3 vertices.
    a_count = sum(1 for x in typ if x == 1)

    if a_count >= 2:
        path = find_a_path(g, typ)

        if path is not None:
            ans = make_answer(n, path)
            if ans is not None:
                return ans

    # Case 4: exactly one A vertex and the B-subgraph is a forest.
    if a_count != 1:
        return None

    a = typ.index(1)

    attach = bytearray(n)
    for v in g[a]:
        if typ[v] == 2:
            attach[v] = 1

    visited = bytearray(n)
    chosen_components = []

    for s in range(n):
        if typ[s] != 2 or visited[s]:
            continue

        stack = [s]
        visited[s] = 1
        attachments = []

        while stack:
            u = stack.pop()

            if attach[u]:
                attachments.append(u)

            for v in g[u]:
                if typ[v] == 2 and not visited[v]:
                    visited[v] = 1
                    stack.append(v)

        if len(attachments) >= 2:
            chosen_components.append(attachments)

            if len(chosen_components) == 2:
                break

    if len(chosen_components) < 2:
        return None

    keep = {a}

    for attachments in chosen_components:
        start = attachments[0]
        path = find_tree_path_to_attachment(g, start, attach)

        if path is None:
            return None

        keep.update(path)

    ans = make_answer(n, keep)
    return ans

def main():
    t = int(input())
    out = []

    for _ in range(t):
        line = input()

        while line and not line.strip():
            line = input()

        n, m = map(int, line.split())

        ans = solve_case(n, m)

        if ans is None:
            out.append("No")
        else:
            out.append("Yes")
            out.append(str(len(ans)))
            out.append(" ".join(map(str, ans)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Mảng độ được tính toán trong khi các cạnh được đọc, do đó không có phép truyền riêng biệt nào chỉ để phân loại các đỉnh. giá trị`typ[v]`chính xác là phần dư của độ ban đầu và được sử dụng xuyên suốt quá trình thi công. 

Quy trình tuần hoàn thực hiện một DFS lặp lại vì đệ quy Python không an toàn đối với đồ thị có (500000) đỉnh.`tin`Và`tout`mô tả các khoảng DFS, cho phép mọi cạnh không phải của cây được nhận dạng là cạnh tổ tiên-con cháu. Trong số các cạnh đó chúng tôi chọn độ chênh lệch độ sâu nhỏ nhất. Bất kỳ hợp âm nào bên trong đường dẫn cây tương ứng của nó sẽ tự nó là một cạnh không phải cây với độ chênh lệch độ sâu nhỏ hơn rất nhiều, do đó chu trình đã chọn không có hợp âm. 

Đường dẫn (A) được tìm thấy bằng BFS. Vì BFS dừng ở đỉnh (A) đầu tiên khác, nên đường dẫn không thể chứa đỉnh (A) khác bên trong. Đây chính xác là những gì chúng ta cần cho điểm cuối dư (1). 

Trường hợp cuối cùng sử dụng đỉnh (A) duy nhất để đánh dấu mọi đỉnh (B) liền kề với nó. Trong mỗi cây (B), bắt đầu từ một đỉnh được đánh dấu và dừng ở đỉnh được đánh dấu đầu tiên khác sẽ tạo ra một đường đi không có đỉnh bên trong được đánh dấu. Điều đó ngăn không cho đỉnh (A) duy nhất có một dây cung không mong muốn ở giữa đường đi. 

Việc thực hiện sử dụng`bytearray`cho màu DFS, cờ đã truy cập và điểm đánh dấu tệp đính kèm. Điều này giúp tiết kiệm bộ nhớ đáng kể so với việc lưu trữ các boolean hoặc số nguyên Python cho một số mảng có độ dài (500000). Không có vấn đề tràn số nguyên trong Python và tất cả các chỉ số đỉnh được chuyển đổi sang dạng dựa trên số 0 ngay sau khi đọc chúng. Câu trả lời cuối cùng chỉ được chuyển đổi trở lại thành các chỉ số dựa trên một khi được in. 

## Ví dụ đã hoạt động 

Đối với biểu đồ đầu tiên trong mẫu, biểu đồ là một hình tam giác. Mọi đỉnh đều có bậc (2) nên cả ba đỉnh đều là đỉnh (B). Đồ thị con (B) chứa một chu trình, nhưng chu trình đó chứa mọi đỉnh. Ăn cắp phần bổ sung của nó sẽ không lấy được gì nên việc xây dựng bị từ chối. 

| Sân khấu | Tiểu bang | 
| --- | --- | 
| Bằng cấp | (2,2,2) | 
| Các loại | (B,B,B) | 
| (Z)-đỉnh được tìm thấy | Không | 
| (B)-chu kỳ | (1-2-3-1) | 
| Chu kỳ sử dụng tất cả các đỉnh | Có | 
| (A)-đỉnh | Không có | 
| Trường hợp rừng cuối cùng | Không áp dụng | 
| Trả lời |`No`| 

Điều này chứng tỏ tại sao việc phát hiện một chu trình thôi là chưa đủ. Cấu trúc giữ lại được lựa chọn phải phù hợp. Không thể sử dụng toàn bộ biểu đồ đã là một chu trình vì sẽ không có đỉnh bị đánh cắp. 

Đối với biểu đồ thứ hai, độ là (2,5,2,1,1,1). Không có đỉnh (Z) và các đỉnh (B) là (1) và (3), được nối với nhau bằng một cạnh, do đó không có chu trình (B). Các đỉnh (4,5,6) là các đỉnh (A). BFS từ đỉnh (4) đến đỉnh (5) qua đỉnh (2). 

| Sân khấu | Tiểu bang | 
| --- | --- | 
| Bằng cấp | (2,5,2,1,1,1) | 
| Các loại | (B,B,B,A,A,A) | 
| (Z)-đỉnh được tìm thấy | Không | 
| (B)-chu kỳ | Không | 
| BFS bắt đầu | (4) | 
| Đầu tiên khác (A)-đỉnh | (5) | 
| Đường dẫn được giữ lại | (4-2-5) | 
| Đỉnh bị đánh cắp | (1,3,6) | 
| Mức độ giữ lại (2) sau khi bị trộm | (2) | 
| Độ gốc của (2) modulo (3) | (5\bmod3=2) | 
| Trả lời |`Yes`| 

Đỉnh được giữ lại (2) có ba đỉnh lân cận bị đánh cắp là (1,3,6), do đó bậc của nó giảm từ (5) xuống (2). Các đỉnh được giữ lại khác (4) và (5) không mất cạnh nào. Do đó, mọi mức độ còn lại đều giữ nguyên modulo dư lượng (3). 

Biểu đồ mẫu thứ ba minh họa trường hợp cuối cùng. Đỉnh (1) có bậc (7) nên nó là đỉnh (A) duy nhất. Đồ thị con (B) bao gồm một số cây. Một cây chứa các đỉnh (2,3,6,7,8), với (6,7,8) liền kề với đỉnh (1). Cái khác chứa (4,5), với cả (4) và (5) liền kề với (1). Bắt đầu từ (6), phần đính kèm khác gần nhất là (3), trong khi cây thứ hai đưa ra đường dẫn (4-5). Giữ (1,6,3,4,5) và trộm (2,7,8) là một giải pháp hợp lý. 

| Thành phần | Bắt đầu đính kèm | Đầu tiên đính kèm khác | Đường dẫn được giữ lại | 
| --- | --- | --- | --- | 
| ({2,3,6,7,8}) | 6 | 3 | (6-3) | 
| ({4,5}) | 4 | 5 | (4-5) | 
| Đỉnh trung tâm | 1 | Cả hai con đường | 1 | 

Các đỉnh (B) được chọn có bậc nội (2). Đỉnh (1) có bốn lân cận được chọn, do đó bậc của nó thay đổi từ (7) thành (4) và cả hai giá trị đều là (1\bmod3). Các đỉnh bị đánh cắp là (2,7,8). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) | Một số lần quét DFS, BFS và quét lân cận không đổi được thực hiện | 
| Không gian | (O(n+m)) | Danh sách kề và mảng phụ có kích thước tuyến tính chiếm ưu thế trong bộ nhớ | 

Tổng (n) và tổng (m) trên tất cả các trường hợp thử nghiệm nhiều nhất là (500000), do đó tổng thời gian chạy là (O(\sum n+\sum m)). Thuật toán không bao giờ xây dựng cấu trúc (n\times n) và không bao giờ liệt kê các tập hợp con, điều này giữ cả bộ nhớ và thời gian chạy trong giới hạn cuộc thi ban đầu. 

## Trường hợp thử nghiệm 

Đầu ra của vấn đề này không phải là duy nhất, vì vậy các thử nghiệm sẽ xác nhận các yêu cầu về cấu trúc thay vì so sánh danh sách chính xác các đỉnh bị đánh cắp. Khai thác sau đây kiểm tra trạng thái, kích thước của tập hợp bị đánh cắp, tính khác biệt và điều kiện độ-modulo-(3) cho mọi đỉnh còn lại.```python
# Run this after the solution above has been defined.
import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    sys.stdout = io.StringIO()

    try:
        main()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

def validate(inp: str, out: str, expected):
    data = list(map(int, inp.split()))
    pos = 0
    t = data[pos]
    pos += 1

    tokens = out.split()
    out_pos = 0

    for case_id in range(t):
        n = data[pos]
        m = data[pos + 1]
        pos += 2

        edges = []
        deg = [0] * n

        for _ in range(m):
            u = data[pos] - 1
            v = data[pos + 1] - 1
            pos += 2
            edges.append((u, v))
            deg[u] += 1
            deg[v] += 1

        assert out_pos < len(tokens)
        status = tokens[out_pos]
        out_pos += 1

        assert status == expected[case_id], (
            f"case {case_id}: expected {expected[case_id]}, got {status}"
        )

        if status == "No":
            continue

        c = int(tokens[out_pos])
        out_pos += 1

        stolen = list(map(int, tokens[out_pos:out_pos + c]))
        out_pos += c

        assert 1 < c < n
        assert len(stolen) == c
        assert len(set(stolen)) == c

        stolen_zero = {x - 1 for x in stolen}

        assert all(0 <= x < n for x in stolen_zero)

        for v in range(n):
            if v in stolen_zero:
                continue

            lost = 0
            for u, w in edges:
                if u == v and w in stolen_zero:
                    lost += 1
                elif w == v and u in stolen_zero:
                    lost += 1

            assert lost % 3 == 0, (
                f"case {case_id}: vertex {v + 1} loses {lost} edges"
            )

    assert out_pos == len(tokens)

sample = """\
3
3 3
1 2
2 3
3 1

6 6
1 2
1 3
2 3
2 5
2 6
2 4

8 12
1 2
1 3
2 3
1 4
4 5
5 1
3 6
3 7
3 8
6 1
7 1
8 1
"""

sample_out = run(sample)
validate(sample, sample_out, ["No", "Yes", "Yes"])

minimum = """\
1
1 0
"""
assert run(minimum).strip() == "No"

two_vertices = """\
1
2 1
1 2
"""
assert run(two_vertices).strip() == "No"

star = """\
1
4 3
1 2
1 3
1 4
"""
star_out = run(star)
validate(star, star_out, ["Yes"])

cycle = """\
1
4 4
1 2
2 3
3 4
4 1
"""
assert run(cycle).strip() == "No"

two_triangles = """\
1
5 6
1 2
2 3
3 1
1 4
4 5
5 1
"""
assert run(two_triangles).strip() == "No"

five_triangles = """\
1
11 15
1 2
2 3
3 1
1 4
4 5
5 1
1 6
6 7
7 1
1 8
8 9
9 1
1 10
10 11
11 1
"""
five_triangles_out = run(five_triangles)
validate(five_triangles, five_triangles_out, ["Yes"])

# Maximum-size connected graph, a star with 500000 vertices.
# The center has degree 499999, which is 1 modulo 3.
max_n = 500000
max_edges = "\n".join(f"1 {v}" for v in range(2, max_n + 1))
max_case = f"1\n{max_n} {max_n - 1}\n{max_edges}\n"

max_out = run(max_case)
validate(max_case, max_out, ["Yes"])

print("All tests passed.")
```Thử nghiệm kích thước tối thiểu kiểm tra ranh giới (n=1) nơi không có hành vi trộm cắp hợp pháp. Kiểm tra hai đỉnh kiểm tra yêu cầu nghiêm ngặt (1<c<n). Ngôi sao kiểm tra cấu trúc (0\bmod3). Bốn chu trình kiểm tra trường hợp toàn bộ đồ thị là chu trình (B). Hai hình tam giác có chung một đỉnh là ví dụ nhỏ nhất về cấu trúc bất khả thi cuối cùng. Năm tam giác có chung một đỉnh (A) tạo ra năm thành phần (B) và thực hiện trường hợp xây dựng cuối cùng. Ngôi sao có kích thước tối đa kiểm tra cả ranh giới đầu vào lớn và hành vi thời gian tuyến tính. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=1,m=0) |`No`| Biểu đồ kích thước tối thiểu | 
| (n=2,m=1) |`No`| Ranh giới nghiêm ngặt (1<c<n) | 
| Sao bốn đỉnh |`Yes`| Xây dựng độ (0\bmod3) | 
| Bốn chu kỳ |`No`| Toàn bộ đồ thị là một chu trình (B) | 
| Hai tam giác có chung một đỉnh |`No`| Cấu trúc bất khả thi cuối cùng | 
| Năm hình tam giác có chung một đỉnh |`Yes`| Nhiều thành phần cây (B) | 
| (500000)-đỉnh sao |`Yes`| Đầu vào có kích thước tối đa và độ phức tạp tuyến tính | 

## Vỏ cạnh 

Đối với đồ thị một đỉnh```
1
1 0
```đỉnh duy nhất có bậc (0) nên thuộc lớp (Z). Công trình đầu tiên sẽ giữ nó, nhưng điều đó sẽ không để lại đỉnh bị đánh cắp.`make_answer`từ chối việc xây dựng vì số đỉnh bị đánh cắp không lớn hơn (1) và không có trường hợp nào khác để sử dụng. Đầu ra là`No`. 

Đối với đồ thị hai đỉnh```
1
2 1
1 2
```cả hai đỉnh đều có bậc (1) nên đều thuộc lớp (A). BFS ngay lập tức tìm thấy đường dẫn (1-2), nhưng đường dẫn này chứa mọi đỉnh. Phần bù của nó trống nên việc xây dựng bị loại bỏ. Không có tập hợp con thích hợp nào khác chứa ít nhất hai đỉnh bị đánh cắp và đầu ra là`No`. 

Đối với bốn chu kỳ```
1
4 4
1 2
2 3
3 4
4 1
```cả bốn đỉnh đều thuộc lớp (B). DFS tìm một cạnh không phải cây và xây dựng lại một chu trình chứa tất cả bốn đỉnh. Phần bổ sung có kích thước bằng 0 nên không thể bị đánh cắp. Tập hợp con được giữ lại thích hợp của một chu trình là một tập hợp các đường dẫn và điểm cuối của đường dẫn có mức độ bên trong (1), không khớp với phần dư (2). Thuật toán do đó trả về`No`. 

Đối với ngôi sao bốn đỉnh```
1
4 3
1 2
1 3
1 4
```trung tâm có bậc (3) nên gọi là lớp (Z). Chỉ giữ lại tâm sẽ cho độ bên trong (0), trong khi độ ban đầu của nó là (3), do đó phần dư được bảo toàn. Ba chiếc lá bị đánh cắp, đưa ra câu trả lời hợp lệ là (c=3). 

Đối với cấu trúc bất khả thi cuối cùng,```
1
5 6
1 2
2 3
3 1
1 4
4 5
5 1
```đỉnh (1) có bậc (4), do đó là đỉnh (A) duy nhất. Các đỉnh (B) tạo thành hai thành phần cây, mỗi thành phần có một cạnh và mỗi thành phần có chính xác hai cạnh tới đỉnh (1). Cách duy nhất để cung cấp cho đỉnh (A) dư độ bên trong cần thiết là sử dụng cả hai thành phần. Vì mỗi thành phần đã chính xác là đường dẫn giữa hai phần đính kèm của nó nên việc giữ lại cả hai đường dẫn có nghĩa là giữ lại tất cả năm đỉnh. Không có bộ giữ lại hợp lệ thích hợp, vì vậy câu trả lời là`No`. 

Đối với một biểu đồ chứa nhiều thành phần như vậy, tình huống sẽ thay đổi. Với năm tam giác có chung đỉnh (1), đỉnh (1) có bậc (10), tức là (1\bmod3). Năm thành phần (B) là các cạnh riêng biệt, mỗi cạnh được gắn vào (1) ở cả hai điểm cuối. Việc giữ các đường dẫn trong hai thành phần bất kỳ sẽ cho đỉnh (1) bậc nội (4), trong khi tất cả các đỉnh (B) được chọn đều có bậc trong (2). Ba thành phần còn lại có thể bị đánh cắp, vì vậy câu trả lời trở thành`Yes`.
