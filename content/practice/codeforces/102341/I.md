---
title: "CF 102341I - Địa ngục"
description: "Đối với mỗi truy vấn, chúng tôi có một số Infernape được đặt trên các đỉnh của một cây cố định. Một Infernape ở đỉnh (vi) có công suất (ri) làm nóng chính xác các đỉnh có khoảng cách cây với (vi) tối đa là (ri). Một đỉnh được coi là tốt nếu có ít nhất (k-1) trong số (k) Infernape làm nóng nó."
date: "2026-08-14T01:53:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "I"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 953
verified: true
draft: false
---

[CF 102341I - Địa ngục](https://codeforces.com/problemset/problem/102341/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 15 phút 53 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi truy vấn, chúng tôi có một số Infernape được đặt trên các đỉnh của một cây cố định. Một Infernape ở đỉnh (v_i) có công suất (r_i) làm nóng chính xác các đỉnh có khoảng cách cây với (v_i) tối đa là (r_i). 

Một đỉnh được coi là tốt nếu có ít nhất (k-1) trong số (k) Infernape làm nóng nó. Tương tự, trong số (k) quả bóng được xác định bởi Infernape, đỉnh có thể nằm bên ngoài nhiều nhất một quả bóng. Nhiệm vụ là đếm tất cả các đỉnh tốt một cách độc lập cho mỗi truy vấn. 

Bản thân cây có tới (10^5) đỉnh, trong khi tổng số Infernape trên tất cả các truy vấn nhiều nhất là (3\cdot10^5). Giới hạn thời gian là 7 giây, do đó, thuật toán quét toàn bộ cây để tìm mỗi Infernape là quá đắt. Một giới hạn như các hoạt động (10^5\cdot3\cdot10^5=3\cdot10^{10}) loại trừ bất kỳ cách tiếp cận (O(nk)) nào. Chúng ta cần khoảng (O((n+\sum k)\log n)) hoặc giá trị nào đó gần với nó. 

Có một số trường hợp ranh giới mà giải pháp phải xử lý chính xác. Đầu tiên, tập hợp mong muốn không phải là giao điểm của tất cả các quả bóng. Ví dụ: trên cây (1-2), lấy hai Infernape ở (1) và (2), cả hai đều có sức mạnh (0). Câu trả lời đúng là (2), vì mỗi đỉnh được làm nóng bởi một Hỏa ngục và (k-1=1). Giao điểm của hai quả bóng trống, do đó việc triển khai chỉ tính giao điểm chung sẽ trả về không chính xác (0). 

Thứ hai, khoảng cách giới hạn là bao gồm. Trên đường đi (1-2-3), đặt một Infernape ở (1) với sức mạnh (1) và một con khác ở (3) với sức mạnh (1). Mỗi đỉnh được làm nóng bởi ít nhất một Hỏa ngục, vì vậy câu trả lời là (3). Việc xử lý khoảng cách (r_i) bị loại trừ sẽ chỉ để lại đỉnh (2). 

Thứ ba, giao điểm của nhiều quả bóng có thể có tâm ở giữa một cạnh chứ không phải ở đỉnh ban đầu. Trên cây (1-2), đặt Infernape ở cả hai điểm cuối có sức mạnh (1). Vùng nóng chung của chúng chứa cả hai đỉnh và có tâm tự nhiên ở điểm giữa của cạnh. Việc hạn chế mọi tâm bóng trung gian ở một đỉnh ban đầu làm cho thao tác giao nhau trở nên khó xử và có thể gây ra các lỗi riêng lẻ. 

Cuối cùng, giao lộ có thể trở nên trống sau khi thêm một quả bóng khác. Một giao lộ như vậy chỉ đơn giản là không đóng góp gì. Thuật toán thể hiện trạng thái này một cách rõ ràng thay vì cố gắng gán cho nó một trung tâm nhân tạo. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra từng đỉnh cây để tìm từng Infernape trong một truy vấn. Chúng ta có thể tính toán khoảng cách của nó với Hỏa ngục, đếm xem có bao nhiêu quả bóng chứa nó và tăng dần câu trả lời khi số lượng đó đạt đến (k-1). Điều này đúng vì nó tuân theo định nghĩa theo nghĩa đen. Chi phí của nó là (O(nk)) cho một truy vấn và (O(n\sum k)) cho toàn bộ đầu vào. Với tổng số tối đa (k), đây là khoảng (3\cdot10^{10}) kiểm tra đỉnh, điều này gần như không khả thi. 

Quan sát hữu ích đầu tiên là câu trả lời có thể được thể hiện bằng cách sử dụng các giao điểm thay vì tính phạm vi phủ sóng một cách trực tiếp. Đặt (S_i) là tập hợp các đỉnh được đốt nóng bởi mọi Infernape ngoại trừ số (i) và gọi (S) là tập hợp được đốt nóng bởi tất cả (k) Infernape. Một đỉnh được làm nóng bởi chính xác (k-1) Infernape thuộc về đúng một (S_i). Một đỉnh được đốt nóng bởi tất cả (k) thuộc về mọi (S_i), do đó nó được tính (k) lần trong tổng của chúng. Trừ (k-1) bản sao của (S) sẽ khắc phục chính xác tình trạng đếm quá mức đó: 

[ 
\text{answer}=\sum_{i=1}^{k}|S_i|-(k-1)|S|. 
] 

Điều này làm giảm vấn đề tính toán kích thước (k+1) giao điểm của các quả bóng cây. 

Quan sát thứ hai là cấu trúc của các giao điểm bóng trên cây. Giao điểm của hai quả bóng được kết nối trong cây lại trống hoặc là một quả bóng có tâm nằm trên đường nối giữa hai tâm ban đầu. Lặp lại thao tác có nghĩa là giao điểm của bất kỳ số lượng bóng nào vẫn có thể được biểu thị bằng một cặp 

[ 
(c,R), 
] 

nghĩa là tất cả các điểm có khoảng cách tối đa (R) tính từ tâm (c).

Tâm có thể nằm dọc theo một nửa cạnh, đó là lý do tại sao cây được chia nhỏ. Mọi cạnh ban đầu (u-v) được thay thế bằng (u-x-v), trong đó (x) là một đỉnh phụ mới. Mỗi khoảng cách cây ban đầu sau đó được nhân đôi. Chúng tôi cũng nhân đôi mọi bán kính của Infernape. Điều này làm cho trung điểm của cạnh ban đầu trở thành một đỉnh thực sự, do đó mọi giao điểm có thể được biểu diễn bằng tâm nguyên và bán kính nguyên. Chỉ các đỉnh ban đầu mới được tính trong câu trả lời cuối cùng. 

Đối với hai quả bóng (U(a,A)) và (U(b,B)), đặt (D=\operatorname{dist}(a,b)). Nếu (A+B<D), giao điểm của chúng trống. Nếu một bán kính đủ xa để chứa quả bóng kia, chúng ta trả lại quả bóng nhỏ hơn không thay đổi. Ngược lại bán kính mới là 

[ 
R=\min(A,B)-\left\lfloor\frac{D-|A-B|}{2}\right\rfloor, 
] 

và tâm mới là điểm tương ứng trên đường đi từ (a) đến (b). Nâng nhị phân cung cấp cả khoảng cách và điểm ở khoảng cách xác định dọc theo đường dẫn. Đây là cốt lõi hình học của giải pháp. Phép biểu diễn và phép giao tương tự được sử dụng trong giải pháp đã biết cho bài toán này. 

Chúng ta có thể xây dựng tất cả các giao điểm trong nhiều phép toán giao lộ tuyến tính bằng cách sử dụng các giao điểm tiền tố và hậu tố. Cho phép`pre[i]`là giao điểm của (i) quả bóng đầu tiên và`suf[i]`giao điểm của các quả bóng từ (i) trở đi. Khi đó giao điểm loại trừ bóng (i) đơn giản là 

[ 
\text{pre[i-1]\cap\text{suf[i+1]. 
] 

Do đó, mọi truy vấn chỉ tạo ra (k+1) yêu cầu đếm bóng. 

Vấn đề còn lại là đếm một cách hiệu quả có bao nhiêu đỉnh cây ban đầu nằm bên trong nhiều quả bóng tùy ý. Phân tách Centroid giải quyết vấn đề này ngoại tuyến. Đối với mỗi tâm, chúng ta biết khoảng cách từ mọi đỉnh trong thành phần hiện tại của nó đến tâm đó. Một truy vấn có tâm tại (u) với bán kính (R) có thể được kiểm tra thông qua tâm bằng cách sử dụng 

[ 
\operatorname{dist}(u,c)+\operatorname{dist}(c,x)\le R. 
] 

Tất cả các đỉnh trong thành phần đều thỏa mãn phần thứ hai thông qua khoảng cách của chúng với tâm. Số tiền tố theo khoảng cách cho biết số lượng có thể (x). Nếu (u) nằm trong một thành phần con của trọng tâm, các đỉnh của cùng thành phần con đó có thể có đường đi ngắn hơn và không đi qua trọng tâm. Trước tiên, chúng ta đếm toàn bộ thành phần thông qua trọng tâm, sau đó trừ chính xác các đỉnh cùng con. Mỗi cặp truy vấn đỉnh được gán cho trọng tâm cao nhất nơi đường đi của chúng đi qua trọng tâm đó, vì vậy nó được tính chính xác một lần. 

Quá trình phân tách trọng tâm được xử lý ngoại tuyến vì tất cả các yêu cầu đếm bóng đều được biết trước khi quá trình phân tách bắt đầu. Cách tiếp cận kết quả có thời gian (O((n+K)\log n)), trong đó (K) là tổng số Infernape và bộ nhớ (O(n\log n+K)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nK)) | (O(n)) | Quá chậm | 
| Tối ưu | (O((n+K)\log n)) | (O(n\log n+K)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia mỗi cạnh ban đầu bằng cách chèn một đỉnh phụ. Cây kết quả có (2n-1) đỉnh và mọi khoảng cách ban đầu đều tăng gấp đôi. Lưu trữ các đỉnh ban đầu làm (n) đỉnh đầu tiên, do đó giai đoạn đếm trọng tâm có thể bỏ qua các đỉnh phụ. 
2. Root cây chia nhỏ và xây dựng bảng nâng nhị phân. Các bảng hỗ trợ`lca(u,v)`Và`jump(u,d)`, Ở đâu`jump`di chuyển các cạnh (d) hướng lên trên từ (u). Những thao tác này đủ để tìm khoảng cách giữa hai trung tâm và định vị một trung tâm mới trên đường kết nối của chúng. 
3. Biểu diễn giao điểm không trống bằng`(center, radius)`trong cây phân chia. Giao lộ trống được biểu thị bằng tâm không hợp lệ. Sử dụng bán kính rất lớn làm phần tử nhận dạng, vì giao một quả bóng vô hạn với một quả bóng thông thường sẽ trả về quả bóng thông thường. 
4. Thực hiện giao điểm của hai quả bóng được biểu diễn. Gọi tâm của chúng là (a,b), bán kính (A,B) và gọi (D) là khoảng cách của chúng. Nếu (A+B<D), trả về giá trị trống. Nếu (A\ge D+B), quả bóng thứ hai được chứa trong quả bóng thứ nhất, vì vậy hãy trả lại quả bóng thứ hai. Trường hợp đối xứng trả về giá trị đầu tiên. Mặt khác tính toán bán kính mới với 
[ 
R=\min(A,B)-\left\lfloor\frac{D-|A-B|}{2}\right\rfloor. 
] 
Tâm mới là điểm trên đường đi từ (a) đến (b) cách nhau một khoảng (A-R) từ (a), hoặc tương đương (B-R) từ (b). Nâng nhị phân định vị điểm đó. 
5. Đối với mỗi truy vấn, hãy xây dựng các giao điểm tiền tố`pre`và hậu tố giao nhau`suf`. Giao lộ đầy đủ là`suf[0]`. Với mỗi Infernape (i) có thể bị bỏ qua, giao điểm của tất cả các quả bóng khác là`pre[i] ∩ suf[i+1]`. Nếu giao lộ này không trống, hãy tạo yêu cầu đếm bóng với hệ số (+1). Tạo thêm một yêu cầu cho giao lộ đầy đủ với hệ số (1-k). 
6. Phân tách cây đã chia theo trọng tâm. Việc loại bỏ trọng tâm sẽ chia thành phần hiện tại thành các thành phần nhỏ hơn, mỗi thành phần chứa tối đa một nửa số đỉnh. Do đó, sự phân rã có mức (O(\log n)). 
7. Tại một tâm (c), duyệt toàn bộ thành phần hiện tại và xây dựng`cnt[d]`, số đỉnh ban đầu ở khoảng cách chính xác (d) từ (c). Chuyển đổi nó thành một mảng tiền tố, vì vậy`cnt[d]`trở thành số đỉnh ban đầu ở khoảng cách tối đa (d). 
8. Duyệt lại thành phần và xử lý mọi yêu cầu đếm bóng có tâm là đỉnh của thành phần này. Nếu tâm (u) của nó cách (du) một khoảng (du) từ (c), thì bán kính (R) có thể đi qua (c) đến mọi đỉnh (x) với 
[ 
du+\operatorname{dist}(c,x)\le R. 
] 
Do đó, sự đóng góp là`cnt[min(max_distance, R-du)]`. 
9. Với mỗi thành phần con của (c), lặp lại quá trình đếm chỉ với các đỉnh từ thành phần con đó, nhưng cho hệ số đếm của chúng (-1). Điều này loại bỏ các đỉnh cùng con khỏi phần đóng góp được tính toán thông qua (c). Sau đó, tái diễn vào thành phần con. Phép trừ là cần thiết vì đường đi qua (c) không phải là đường đi thực tế của hai đỉnh bên trong cùng một nút con. 
10. Sau khi tất cả các mức phân rã trọng tâm được xử lý, mỗi yêu cầu bóng chứa kích thước của quả bóng được đại diện trong số các đỉnh ban đầu. Đối với truy vấn (q), các hệ số được lưu trữ của nó đã được tính toán 
[ 
\sum_i |S_i|-(k-1)|S|, 
] 
đó chính xác là câu trả lời cần thiết. 

### Tại sao nó hoạt động 

Tính đúng đắn có hai phần độc lập. Đầu tiên, công thức bao gồm là chính xác vì một đỉnh được bao phủ bởi chính xác (k-1) quả bóng thuộc về một giao điểm bóng bị bỏ sót, trong khi một đỉnh được bao phủ bởi tất cả (k) quả bóng thuộc về tất cả (k) giao điểm bóng bị bỏ qua và sau đó bị trừ (k-1) lần. 

Thứ hai, mọi giao lộ được biểu diễn chính xác bằng một bóng cây hoặc trạng thái trống. Trên một cây, các giới hạn biên của hai quả bóng giao nhau gặp nhau dọc theo đường đi duy nhất giữa tâm của chúng, do đó giao điểm có tâm trên đường đi đó và có bán kính cho trước ở trên. Phép chia làm cho mọi điểm giữa có thể trở thành một đỉnh thực sự, trong khi việc nhân đôi khoảng cách sẽ bảo toàn tất cả các phát biểu về các đỉnh ban đầu. 

Đối với trọng tâm cố định, lần duyệt đầu tiên sẽ đếm các đỉnh theo khoảng cách của chúng qua tâm. Đường đi như vậy là đúng cho các đỉnh nằm ngoài thành phần con của tâm. Đối với các đỉnh trong cùng một nút con, khoảng cách xuyên tâm có thể khác với khoảng cách thực, do đó phép duyệt thứ hai sẽ trừ chính xác các ứng cử viên cùng nút con đó. Do đó mức trọng tâm này đếm chính xác các cặp có đường đi qua tâm. Mỗi cặp đỉnh được phân tách bằng chính xác một trọng tâm cao nhất trong quá trình phân tách, do đó không có cặp nào bị bỏ sót hoặc được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    N = 2 * n - 1

    g = [[] for _ in range(N)]

    for i in range(n - 1):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        x = n + i
        g[a].append(x)
        g[x].append(a)
        g[b].append(x)
        g[x].append(b)

    # Root the subdivided tree and build binary lifting.
    parent = [0] * N
    depth = [0] * N
    order = [0]

    for u in order:
        pu = parent[u]
        for v in g[u]:
            if v == pu:
                continue
            parent[v] = u
            depth[v] = depth[u] + 1
            order.append(v)

    LOG = N.bit_length()
    up = [parent]

    for _ in range(1, LOG):
        prev = up[-1]
        cur = [0] * N
        for i in range(N):
            cur[i] = prev[prev[i]]
        up.append(cur)

    def jump(u, d):
        bit = 0
        while d:
            if d & 1:
                u = up[bit][u]
            d >>= 1
            bit += 1
        return u

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

    def distance(a, b):
        p = lca(a, b)
        return depth[a] + depth[b] - 2 * depth[p]

    INF = 10**9

    # A ball is (center, radius). (-1, -1) is empty.
    def intersect(A, B):
        a, ra = A
        b, rb = B

        if a < 0 or b < 0:
            return (-1, -1)

        p = lca(a, b)
        D = depth[a] + depth[b] - 2 * depth[p]

        if ra + rb < D:
            return (-1, -1)

        if ra >= D + rb:
            return B

        if rb >= D + ra:
            return A

        R = min(ra, rb) - (D - abs(ra - rb)) // 2

        da = depth[a] - depth[p]
        move_a = ra - R

        if da >= move_a:
            c = jump(a, move_a)
        else:
            c = jump(b, rb - R)

        return (c, R)

    q = int(input())

    answers = [0] * q

    # Offline ball-counting requests.
    qhead = [-1] * N
    qradius = []
    qid = []
    qcoef = []
    qnext = []

    def add_request(center, radius, idx, coef):
        pos = len(qradius)
        qradius.append(radius)
        qid.append(idx)
        qcoef.append(coef)
        qnext.append(qhead[center])
        qhead[center] = pos

    for qi in range(q):
        k = int(input())

        balls = [None] * k
        for i in range(k):
            v, r = map(int, input().split())
            balls[i] = (v - 1, 2 * r)

        pre = [None] * (k + 1)
        pre[0] = (0, INF)

        for i in range(k):
            pre[i + 1] = intersect(pre[i], balls[i])

        suf = [None] * (k + 1)
        suf[k] = (0, INF)

        for i in range(k - 1, -1, -1):
            suf[i] = intersect(balls[i], suf[i + 1])

        # Full intersection has coefficient 1-k.
        all_ball = suf[0]
        if all_ball[0] >= 0:
            add_request(all_ball[0], all_ball[1], qi, 1 - k)

        # Intersection of every ball except i has coefficient +1.
        for i in range(k):
            cur = intersect(pre[i], suf[i + 1])
            if cur[0] >= 0:
                add_request(cur[0], cur[1], qi, 1)

    # Centroid decomposition.
    dead = bytearray(N)
    temp_parent = [-1] * N
    subsize = [0] * N

    def find_centroid(start):
        comp = [start]
        temp_parent[start] = -1

        for u in comp:
            pu = temp_parent[u]
            for v in g[u]:
                if dead[v] or v == pu:
                    continue
                temp_parent[v] = u
                comp.append(v)

        total = len(comp)

        for u in comp:
            subsize[u] = 1

        for u in reversed(comp):
            p = temp_parent[u]
            if p != -1:
                subsize[p] += subsize[u]

        centroid = start
        best = total + 1

        for u in comp:
            largest = total - subsize[u]

            for v in g[u]:
                if dead[v]:
                    continue
                if temp_parent[v] == u and subsize[v] > largest:
                    largest = subsize[v]

            if largest < best:
                best = largest
                centroid = u

        return centroid, total

    def collect(start, parent_node, start_dist, cnt, sign):
        stack = [(start, parent_node, start_dist)]
        max_dist = start_dist

        while stack:
            u, p, d = stack.pop()

            if u < n:
                cnt[d] += sign

            if d > max_dist:
                max_dist = d

            for v in g[u]:
                if dead[v] or v == p:
                    continue
                stack.append((v, u, d + 1))

        return max_dist

    def process_requests(start, parent_node, start_dist, cnt, deg):
        stack = [(start, parent_node, start_dist)]

        while stack:
            u, p, d = stack.pop()

            e = qhead[u]
            while e != -1:
                r = qradius[e]

                if r >= d:
                    limit = r - d
                    if limit > deg:
                        limit = deg
                    answers[qid[e]] += qcoef[e] * cnt[limit]

                e = qnext[e]

            for v in g[u]:
                if dead[v] or v == p:
                    continue
                stack.append((v, u, d + 1))

    tasks = [0]

    while tasks:
        start = tasks.pop()

        centroid, total = find_centroid(start)
        dead[centroid] = 1

        cnt = [0] * (total + 1)

        # First count all vertices through the centroid.
        deg = collect(centroid, -1, 0, cnt, 1)

        for d in range(1, deg + 1):
            cnt[d] += cnt[d - 1]

        process_requests(centroid, -1, 0, cnt, deg)

        # Then subtract vertices belonging to the same child component.
        for v in g[centroid]:
            if dead[v]:
                continue

            # Only the prefix that will be used by this child needs clearing.
            child_deg = 0
            stack = [(v, centroid, 1)]
            while stack:
                u, p, d = stack.pop()
                if d > child_deg:
                    child_deg = d
                for w in g[u]:
                    if dead[w] or w == p:
                        continue
                    stack.append((w, u, d + 1))

            for d in range(child_deg + 1):
                cnt[d] = 0

            child_deg = collect(v, centroid, 1, cnt, -1)

            for d in range(1, child_deg + 1):
                cnt[d] += cnt[d - 1]

            process_requests(v, centroid, 1, cnt, child_deg)

        # The remaining neighbors are roots of independent components.
        for v in g[centroid]:
            if not dead[v]:
                tasks.append(v)

    sys.stdout.write("\n".join(map(str, answers)))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng cây chia nhỏ và root nó một lần. Các đỉnh phụ được cố tình đặt sau các đỉnh ban đầu, do đó việc kiểm tra`u < n`sau đó xác định chính xác các đỉnh phải được tính. 

Bàn nâng nhị phân được sử dụng hai lần.`lca`đưa ra khoảng cách giữa hai tâm bóng, trong khi`jump`xác định vị trí trung tâm mới của một giao lộ. Tất cả bán kính đều được nhân đôi khi truy vấn được đọc, khớp với độ dài cạnh nhân đôi trong cây chia nhỏ. 

Mảng tiền tố và hậu tố là lý do mỗi truy vấn chỉ cần giao điểm bóng (O(k)). Quả bóng nhận dạng`(0, INF)`cho phép hoạt động tiền tố hoặc hậu tố đầu tiên được viết chính xác như mọi giao lộ khác. 

Mảng yêu cầu tạo thành một danh sách liên kết cho mỗi đỉnh cây. Điều này tránh tạo ra tối đa (2n-1) đối tượng danh sách Python chứa danh sách yêu cầu và nó cũng tránh lưu trữ một số lượng lớn các bộ dữ liệu bốn phần tử. Mỗi yêu cầu lưu trữ tâm, bán kính, chỉ mục truy vấn ban đầu, hệ số và yêu cầu tiếp theo tại cùng một trung tâm. 

Sự phân rã trung tâm được viết lặp đi lặp lại. Một DFS đệ quy trên đường dẫn có (10^5) đỉnh sẽ vượt quá giới hạn đệ quy của Python, trong khi bản thân phân rã centroid có độ sâu logarit nhưng các đường truyền thành phần của nó thì không. Trình tìm kiếm thành phần xây dựng một cách rõ ràng thứ tự duyệt và tính toán ngược kích thước cây con. 

Giai đoạn đếm tâm trước tiên sẽ đếm mọi đỉnh ban đầu đi qua tâm. Đối với một yêu cầu có tâm ở khoảng cách (d) tính từ tâm, chỉ bán kính`r - d`vẫn có sẵn cho nửa sau của con đường. Các câu trả lời mảng tiền tố được tính bằng (O(1)). 

Lần duyệt thứ hai cho thành phần con có trọng số âm. Nó loại bỏ các đỉnh giống nhau khỏi số đếm đầu tiên, để lại chính xác các đỉnh có đường đi từ trung tâm yêu cầu đi qua tâm hiện tại. Sau khi mức trọng tâm này hoàn thành, trọng tâm sẽ bị loại bỏ vĩnh viễn và mọi hàng xóm còn lại sẽ bắt đầu một bài toán con độc lập. 

Mọi số học đều là số học số nguyên. Số nguyên Python cũng loại bỏ mọi lo ngại về tràn 64 bit mà lẽ ra phải được xem xét cho các câu trả lời tích lũy. 

## Ví dụ đã hoạt động 

Mẫu chính thức chứa cây sau đây và hai truy vấn, với kết quả đầu ra (5) và (7). 

### Mẫu 1 

Truy vấn đầu tiên có ba Infernape:```
(8, 1)
(3, 1)
(3, 2)
```Các bộ gia nhiệt có liên quan là 

[ 
B_1={8,9,1,2,7}, 
] 

[ 
B_2={3,1,4,10}, 
] 

và 

[ 
B_3={3,1,4,10,5,8}. 
] 

Việc tính toán giao lộ có thể được tóm tắt như sau. 

| Hoạt động | Kết quả giao lộ | Kích thước giữa các đỉnh ban đầu | Hệ số | 
| --- | --- | --- | --- | 
| (B_2\cap B_3) | ({3,1,4,10}) | 4 | (+1) | 
| (B_1\cap B_3) | ({1,8}) | 2 | (+1) | 
| (B_1\cap B_2) | ({1}) | 1 | (+1) | 
| (B_1\cap B_2\cap B_3) | ({1}) | 1 | (1-3=-2) | 

Câu trả lời tích lũy là 

[ 
4+2+1-2\cdot1=5. 
] 

Đỉnh (1) được đốt nóng bởi cả ba Infernape nên ban đầu nó xuất hiện ở cả ba giao điểm bóng bị bỏ qua. Hệ số (-2) loại bỏ chính xác hai bản sao, để lại một lần xuất hiện theo yêu cầu. 

### Mẫu 2 

Truy vấn thứ hai có hai Infernape:```
(7, 3)
(6, 0)
```Quả bóng thứ hai chỉ chứa đỉnh (6). Quả cầu thứ nhất chứa 

[ 
{7,8,1,2,9,3}. 
] 

Vì (k=2), một đỉnh cần được làm nóng bởi ít nhất một Hỏa ngục, nên câu trả lời đơn giản là sự kết hợp của hai tập hợp này. 

| Hoạt động | Kết quả | Kích thước | Hệ số | 
| --- | --- | --- | --- | 
| Chỉ bóng đầu tiên | ({7,8,1,2,9,3}) | 6 | (+1) | 
| Chỉ bóng thứ hai | ({6}) | 1 | (+1) | 
| Cả hai quả bóng | trống | 0 | (-1) | 

Kết quả là 

[ 
6+1-0=7. 
] 

Ví dụ này cũng thực hiện trạng thái giao điểm trống. Thuật toán không tạo ra yêu cầu trọng tâm cho giao lộ trống, do đó, nó đóng góp chính xác bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+K)\log n)) | Nâng nhị phân xử lý từng giao điểm bóng trong (O(\log n)) và phân tách centroid xử lý mọi đỉnh cây và mọi yêu cầu ở mức (O(\log n)) | 
| Không gian | (O(n\log n+K)) | Sử dụng nâng nhị phân (O(n\log n)), trong khi các yêu cầu ngoại tuyến và sử dụng cây (O(n+K)) | 

Ở đây (K) là tổng của tất cả các kích thước truy vấn, với (K\le300000). Cây được chia có ít hơn (200000) đỉnh nên hệ số logarit vẫn nhỏ. Quá trình xử lý trung tâm ngoại tuyến tránh được nút thắt cổ chai (O(nK)) của phương pháp vũ phu và được thiết kế xung quanh các giới hạn (10^5) và (3\cdot10^5) đã cho. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`. Mẫu là mẫu chính thức, tiếp theo là bốn trường hợp mục tiêu. Trường hợp cuối cùng xây dựng một đường dẫn có mức tối đa cho phép (n=100000).```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = solution.input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solution.input = sys.stdin.readline

    try:
        solution.solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        solution.input = old_input

# Official sample
sample = """\
10
1 3
6 4
9 8
1 8
3 4
2 8
10 3
4 5
8 7
2
3
8 1
3 1
3 2
2
7 3
6 0
"""

assert run(sample) == "5\n7", "official sample"

# Minimum-size tree, zero-radius balls.
# Each vertex is heated by exactly one Infernape.
case_min = """\
2
1 2
1
2
1 0
2 0
"""

assert run(case_min) == "2", "minimum-size tree"

# Boundary distance is inclusive.
# On 1-2-3, each endpoint reaches vertex 2.
# The union contains all three vertices.
case_boundary = """\
3
1 2
2 3
1
2
1 1
3 1
"""

assert run(case_boundary) == "3", "inclusive radius boundary"

# Three identical zero-radius balls.
# Only vertex 2 is heated, and it is heated by all three.
case_equal = """\
3
1 2
2 3
1
3
2 0
2 0
2 0
"""

assert run(case_equal) == "1", "identical balls"

# Maximum-size path.
# Both radius-(n-1) balls cover the whole tree.
n = 100000
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
case_max = (
    f"{n}\n"
    f"{edges}\n"
    "1\n"
    "2\n"
    f"1 {n - 1}\n"
    f"{n} {n - 1}\n"
)

assert run(case_max) == str(n), "maximum-size path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 10 đỉnh chính thức |`5`Và`7`| Công thức giao đầy đủ và tính trọng tâm tổng quát | 
| Hai đỉnh, cả hai bán kính 0 |`2`| Cây tối thiểu và thực tế là (k-1=1) có nghĩa là hợp chứ không phải là giao | 
| Đường dẫn (1-2-3), bán kính điểm cuối 1 |`3`| Bao gồm ranh giới khoảng cách | 
| Ba quả bóng bán kính-0 giống hệt nhau |`1`| Tâm lặp lại, bóng bằng nhau và hệ số (1-k) | 
| Đường dẫn được tạo với (100000) đỉnh |`100000`| Tối đa (n), bán kính tối đa và phân rã cây lớn | 

## Vỏ cạnh 

Đối với cây tối thiểu```
2
1 2
1
2
1 0
2 0
```hai quả bóng là ({1}) và ({2}). Giao điểm đầy đủ của chúng trống, trong khi hai giao điểm thu được bằng cách bỏ qua một quả bóng có kích thước (1) và (1). Công thức cho (1+1-1\cdot0=2). Pha trung tâm không bao giờ nhận được yêu cầu về giao lộ trống, vì vậy nó không thể vô tình đếm một tâm không hợp lệ. 

Đối với trường hợp bán kính bao gồm```
3
1 2
2 3
1
2
1 1
3 1
```cây nhân đôi chứa đường đi có độ dài cạnh (1) và hai quả bóng ban đầu có bán kính gấp đôi (2). Ranh giới của chúng đều đạt đến đỉnh (2). Giao điểm của quả bóng bị bỏ qua chỉ đơn giản là hai quả bóng ban đầu, trong khi giao điểm chung của chúng là đỉnh (2). Công thức là (2+2-1=3), khớp với sự kết hợp của hai quả bóng. 

Đối với những quả bóng giống hệt nhau```
3
1 2
2 3
1
3
2 0
2 0
2 0
```mọi quả bóng đều là quả bóng đơn ({2}). Mỗi giao điểm bóng bị bỏ qua có kích thước (1) và giao điểm đầy đủ cũng có kích thước (1). Giá trị tích lũy là 

[ 
1+1+1-2\cdot1=1. 
] 

Điều này chứng tỏ tại sao chỉ tính tổng các giao điểm bị bỏ qua (k) là sai khi một đỉnh được bao phủ bởi tất cả (k) Infernape. 

Trường hợp trung điểm xảy ra trên```
2
1 2
1
2
1 1
2 1
```Sau khi chia nhỏ, cây là (1-x-2) và cả hai bán kính đều trở thành (2). Giao điểm của hai quả bóng có tâm (x), điểm giữa được chèn và bán kính (1) trong số liệu chia nhỏ. Cả hai đỉnh ban đầu đều ở khoảng cách (1) từ (x), do đó kích thước được tính là (2). Đây chính xác là lý do tại sao phân khu cạnh là một phần của biểu diễn chứ không chỉ đơn thuần là sự thuận tiện khi triển khai. 

Cuối cùng, đường dẫn có kích thước tối đa chứa (100000) đỉnh gốc và (99999) cạnh. Hai Infernape được đặt ở các điểm cuối có bán kính (99999), mỗi cái bao phủ toàn bộ đường dẫn. Giao điểm cũng chính là toàn bộ đường đi nên đáp án là (100000). Sự phân rã centroid liên tục cắt đường đi gần một nửa, giữ cho số cấp logarit mặc dù cây ban đầu rất mất cân bằng.
