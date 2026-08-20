---
title: "CF 102174G - \u795e\u5723\u7684 F2 \u8fde\u63a5\u7740\u6211\u4eec"
description: "Có hai quận, mỗi quận chứa các vị trí được đánh số từ (1) đến (n). Bản thân các vị trí trong cùng một quận không có mối liên hệ nào. Lăng kính là cách duy nhất để di chuyển giữa các quận. Một lăng kính được mô tả bởi hai khoảng vị trí và thời gian di chuyển."
date: "2026-08-19T07:06:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102174
codeforces_index: "G"
codeforces_contest_name: "The 14-th BIT Campus Programming Contest"
rating: 0
weight: 102174
solve_time_s: 182
verified: true
draft: false
---

[CF 102174G - \u795e\u5723\u7684 F2 \u8fde\u63a5\u7740\u6211\u4eec](https://codeforces.com/problemset/problem/102174/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có hai quận, mỗi quận chứa các vị trí được đánh số từ (1) đến (n). Bản thân các vị trí trong cùng một quận không có mối liên hệ nào. Lăng kính là cách duy nhất để di chuyển giữa các quận. 

Một lăng kính được mô tả bởi hai khoảng vị trí và thời gian di chuyển. Nếu một đơn vị hiện đang ở bất kỳ vị trí nào trong khoảng thời gian đầu tiên, nó có thể nhảy tới bất kỳ vị trí nào trong khoảng thời gian thứ hai trong thời gian chính xác (w). Bước nhảy là hai chiều, do đó chi phí tương tự sẽ được áp dụng theo hướng ngược lại. 

Các đơn vị chiến đấu (p) bắt đầu tại các vị trí (x_1,\ldots,x_p) ở quận một, trong khi các công trình (q) của địch chiếm giữ các vị trí (y_1,\ldots,y_q) ở quận hai. Một đơn vị chiến đấu được coi là đã hoàn thành ngay khi nó đến được bất kỳ tòa nhà nào. Vì tất cả các đơn vị có thể di chuyển đồng thời nên câu trả lời bắt buộc là thời gian đến muộn nhất giữa các đơn vị khi mỗi đơn vị độc lập chọn con đường nhanh nhất đến tòa nhà của kẻ thù. 

Nếu (d(x,y)) biểu thị khoảng cách đường đi ngắn nhất giữa vị trí chiến đấu (x) và vị trí xây dựng (y), câu trả lời là 

[ 
\max_{i=1}^{p}\min_{j=1}^{q} d(x_i,y_j). 
] 

Nếu thậm chí một đơn vị chiến đấu không thể tiếp cận bất kỳ tòa nhà nào của kẻ thù thì câu trả lời là`boring game`. 

Dữ liệu đầu vào chứa tối đa (10^5) vị trí, (10^5) lăng kính, (10^5) đơn vị chiến đấu và (10^5) tòa nhà. Với (n,m) cả hai đều xung quanh (10^5), việc mở rộng rõ ràng mọi lăng kính sang tất cả các cặp vị trí là không thể. Ngay cả một lăng kính đơn bao phủ tất cả các vị trí cũng sẽ đại diện cho các cặp (n^2=10^{10}) và với lăng kính (10^5), trường hợp xấu nhất đạt tới các cặp (10^{15}) trước cả khi xem xét hai hướng. Một giải pháp cần tính logarit đại khái trên mỗi lăng trụ thay vì tính tuyến tính hoặc bậc hai trong các kích thước khoảng. 

Có một số trường hợp ranh giới dễ bỏ sót. Đầu tiên, một đơn vị chiến đấu có thể bị ngắt kết nối hoàn toàn với mọi tòa nhà. Ví dụ,```
2 1 1 1
1 1 1 1 4
2
1
```có lăng trụ chỉ nối vị trí (1) của mỗi quận, còn đơn vị chiến đấu xuất phát ở vị trí (2). Đầu ra đúng là`boring game`. Việc triển khai bất cẩn khởi tạo các khoảng cách không thể tiếp cận về 0 hoặc đơn giản là lấy giá trị tối đa trên các đơn vị có thể tiếp cận, có thể trả về 0 không chính xác. 

Thứ hai, cả hai đầu của khoảng lăng kính đều được bao gồm. Ví dụ,```
4 1 1 1
1 1 4 4 3
1
4
```có đường đi trực tiếp từ vị trí (1) đến vị trí (4), nên đáp án là`3`. Việc coi các khoảng thời gian là nửa mở trong quá trình phân tách cây phân đoạn sẽ âm thầm mất tuyến đường này. 

Thứ ba, lăng kính là hai chiều. Ví dụ,```
3 1 1 1
1 1 3 3 5
3
1
```cũng có câu trả lời`5`, mặc dù đơn vị chiến đấu bắt đầu trong khoảng thời gian xuất hiện dưới dạng điểm cuối thứ hai của lăng kính đầu vào. Việc triển khai chỉ thêm hướng được liệt kê sẽ tạo ra`boring game`. 

Cuối cùng, câu trả lời là khoảng cách ngắn nhất tối đa trên mỗi đơn vị, không phải là con đường ngắn nhất trên toàn cầu. Nếu hai đơn vị cần thời gian (2) và (7) thì chúng di chuyển đồng thời nên đối phương đầu hàng vào thời điểm (7), không phải thời gian (9) và không phải thời gian (2). 

## Phương pháp tiếp cận 

Giải pháp brute-force bắt đầu bằng cách biến mọi lăng kính thành các cạnh đồ thị thông thường. Đối với lăng kính kết nối ([a,b]) với ([c,d]), chúng ta sẽ thêm một cạnh từ mọi (x\in[a,b]) đến mọi (y\in[c,d]) và một cạnh khác theo hướng ngược lại. Biểu đồ này thể hiện chính xác vấn đề, vì vậy việc chạy thuật toán đường dẫn ngắn nhất đa nguồn trên đó là chính xác. 

Vấn đề là số cạnh. Một lăng kính có thể tạo ra các cặp ((b-a+1)(d-c+1)) theo mỗi hướng. Với cả hai khoảng độ dài (n), đây là các cạnh có hướng (2n^2). Tại (n=10^5), đó là (2\cdot10^{10}) cạnh cho chỉ một lăng kính và trường hợp xấu nhất trên (10^5) lăng kính là (2\cdot10^{15}). Điều này vượt xa giới hạn thời gian và bộ nhớ. 

Quan sát hữu ích đầu tiên là đích đến là một tập hợp các tòa nhà, vì vậy chúng ta không cần tính toán đường đi ngắn nhất riêng biệt cho mỗi đơn vị chiến đấu. Thêm một siêu nguồn mang tính khái niệm được kết nối với chi phí bằng 0 cho mọi tòa nhà của kẻ thù trong biểu đồ đảo ngược. Sau đó, một lần chạy Dijkstra sẽ tính khoảng cách từ mọi vị trí đến tòa nhà gần nhất. 

Quan sát thứ hai xử lý các khoảng thời gian lớn. Giả sử chúng ta muốn nối mọi điểm của khoảng (A) với mọi điểm của khoảng (B). Cây phân đoạn có thể biểu thị một trong hai khoảng chỉ bằng cách sử dụng các nút chính tắc (O(\log n)). 

Chúng tôi sử dụng hai bản sao có hướng của cây phân đoạn. Trong bản sao đầu tiên, mỗi phần tử con trỏ tới phần tử cha của nó với chi phí bằng 0. Do đó, một điểm có thể leo từ lá của nó tới bất kỳ nút cây đoạn nào có khoảng chứa điểm đó. Trong bản sao thứ hai, mọi phụ huynh đều trỏ đến con của nó với chi phí bằng 0. Do đó, một nút cây phân đoạn có thể đi xuống bất kỳ điểm nào trong khoảng của nó. 

Đối với một lăng kính có hướng (A\to B), hãy tạo một đỉnh ảo (v). Mọi nút chính tắc bao phủ (A) trong cây hướng lên kết nối với (v) với chi phí (w) và (v) kết nối với chi phí bằng 0 tới mọi nút chính tắc bao phủ (B) trong cây hướng xuống. Một đường đi từ bất kỳ điểm nào của (A) có thể leo lên một nút chính tắc, đi qua (w) chính xác một lần tại (v), rồi đi xuống bất kỳ điểm nào của (B). 

Vì lăng kính có tính hai chiều nên chúng ta tạo ra cấu trúc tương tự cho (B\to A). Cây hướng lên và cây hướng xuống phải tách biệt. Nếu các cạnh cha-con của chúng được tạo thành hai chiều, một điểm có thể di chuyển tự do bên trong hạt của nó bằng cách leo lên và đi xuống cùng một cây, điều này sẽ đưa ra những đường đi không tồn tại trong bài toán ban đầu. 

Việc triển khai bên dưới chạy Dijkstra trên biểu đồ nén đảo ngược. Nó lưu trữ các kết nối lăng kính một cách gọn gàng dưới dạng các sự kiện phạm vi thay vì cụ thể hóa mọi cạnh của biểu đồ. Khoảng mục tiêu của một đỉnh ảo được gắn vào các nút cây phân đoạn chuẩn của nó. Khi Dijkstra đến một nút như vậy, đỉnh ảo sẽ có thể truy cập được mà không mất thêm chi phí. Khi đỉnh ảo được bật lên, khoảng nguồn của nó được phân tách thành các nút chính tắc của cây khác và các nút đó nhận được chi phí lăng kính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(mn^2)) cấu trúc cạnh trong trường hợp xấu nhất | (O(mn^2)) | Quá chậm | 
| Nén cây phân đoạn + Dijkstra | (O((n+m)\log^2 n)) | (O(n+m\log n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Coi mọi vị trí ở cả hai hạt là một đỉnh của đồ thị khái niệm. Đối với lăng kính ([a,b]\leftrightarrow[c,d]) có giá (w), hãy tạo hai đỉnh ảo, một đỉnh biểu thị hướng từ ([a,b]) đến ([c,d]) và đỉnh còn lại biểu thị hướng ngược lại. Hai đỉnh ảo là cần thiết vì các hướng khoảng có phạm vi nguồn và đích khác nhau. 
2. Xây dựng một cây phân đoạn trên tất cả (2n) vị trí. Vị trí (1) đến (n) đại diện cho quận một và vị trí (n+1) đến (2n) đại diện cho quận hai. Giữ hai bản sao hợp lý của cây này. Trong bản sao hướng lên trên, mỗi phần tử con trỏ đến phần tử cha của nó với chi phí bằng 0. Trong bản sao hướng xuống, mỗi phụ huynh chỉ vào con của mình với chi phí bằng 0. 
3. Kết nối các bản sao lên và xuống ở mọi vị trí thực với các cạnh có chi phí bằng 0 ở cả hai hướng. Điều này làm cho cả hai cách biểu diễn của cùng một vị trí vật lý có thể hoán đổi cho nhau, trong khi các nút cây phân đoạn bên trong vẫn có tính định hướng. 
4. Với mỗi lăng trụ có hướng (A\to B), phân tách (B) thành (O(\log n)) nút chính tắc của cây hướng xuống. Trong biểu đồ đảo ngược, mỗi nút đó có kết nối miễn phí tới đỉnh ảo của lăng kính. Chúng tôi lưu trữ các kết nối này dưới dạng các sự kiện gắn liền với các nút cây phân đoạn tương ứng. 
5. Lưu trữ khoảng nguồn (A) và chi phí (w) bên trong đỉnh ảo. Khi đạt đến đỉnh ảo trong Dijkstra đảo ngược, hãy phân tách (A) thành (O(\log n)) nút chính tắc của cây hướng lên và thư giãn tất cả chúng bằng (w). Đây chính xác là hình thức trả tiền ngược (w) khi vào lăng kính. 
6. Khởi tạo Dijkstra từ mọi tòa nhà của kẻ thù ở khoảng cách bằng 0. Tòa nhà được thể hiện bằng chiếc lá của nó trong cây hướng xuống. Bắt đầu từ cây đi xuống là thuận tiện vì cạnh lăng kính đảo ngược đi vào đỉnh ảo từ các nút chính tắc biểu thị khoảng đích. 
7. Trong Dijkstra, một nút cây hướng lên đã đảo ngược các cạnh về phía nút con của nó, bởi vì cây hướng lên ban đầu có các cạnh từ nút con đến nút cha. Nút cây hướng xuống có cạnh đảo ngược về phía nút cha của nó, vì cây hướng xuống ban đầu có các cạnh từ nút cha đến nút con. Một lá thật cũng có một cạnh không tốn kém so với lá tương ứng của cây kia. 
8. Khi Dijkstra kết thúc, khoảng cách được lưu ở lá cây hướng lên tương ứng với vị trí chiến đấu (x_i) chính xác là khoảng cách ngắn nhất từ ​​(x_i) đến bất kỳ công trình địch nào. Tận dụng tối đa tất cả các đơn vị chiến đấu. Nếu khoảng cách đó là vô hạn, hãy in`boring game`. 

### Tại sao nó hoạt động 

Xét một lăng kính ban đầu từ khoảng (A) đến khoảng (B). Mỗi điểm trong (A) có thể leo lên cây hướng lên chính xác một trong các nút chính tắc bao phủ (A). Từ đó nó đạt tới đỉnh lăng kính và trả (w). Đỉnh lăng kính sau đó chạm đến mọi nút chính tắc bao phủ (B) trong cây hướng xuống và nút đó có thể đi xuống mọi điểm trong (B). Do đó, biểu đồ nén chứa đường dẫn chi phí chính xác (w) giữa mọi cặp điểm cuối được phép. 

Ngược lại, quá trình chuyển đổi chi phí dương duy nhất được cấu trúc này đưa ra là quá trình chuyển đổi qua đỉnh lăng kính ảo. Các cạnh của cây phân đoạn có chi phí bằng 0 chỉ thay đổi cách biểu diễn của cùng một khoảng thành viên và không bao giờ cho phép di chuyển giữa các vị trí thực không liên quan. Do đó, mọi đường đi nén giữa các vị trí thực đều tương ứng với một chuỗi hợp lệ các đường đi qua lăng kính ban đầu với cùng chi phí. 

Chạy Dijkstra trên biểu đồ đảo ngược từ tất cả các tòa nhà sẽ tính toán khoảng cách từ mọi vị trí thực tế đến tòa nhà gần nhất của nó. Vì tất cả các đơn vị chiến đấu đều di chuyển đồng thời và độc lập nên thời điểm tất cả các đơn vị chiến đấu đều đến nơi là thời điểm tối đa trong khoảng cách ngắn nhất đó. Một nguồn không thể truy cập có khoảng cách vô hạn, khớp chính xác với yêu cầu`boring game`tình trạng. 

## Giải pháp Python```python
import sys
import heapq
from array import array

input = sys.stdin.readline

INF = 4_000_000_000_000_000_000

def solve():
    n, m, p, q = map(int, input().split())

    # There are 2*n real positions, the first n in county 1
    # and the next n in county 2.
    N = 2 * n

    # Iterative segment tree size.
    S = 1
    while S < N:
        S <<= 1

    # Segment-tree indices are 1 .. 2*S-1.
    # Tree 0: upward tree, child -> parent in the original graph.
    # Tree 1: downward tree, parent -> child in the original graph.
    OUT_BASE = 2 * S
    VBASE = 4 * S

    virtual_count = 2 * m
    total_nodes = VBASE + virtual_count

    # For each downward-tree canonical node, head[idx] is the first
    # virtual prism attached to it in the reversed graph.
    head = array('i', [-1]) * (2 * S)

    # Linked-list storage for prism events.
    event_v = array('i')
    event_next = array('i')

    # Information stored for every virtual vertex.
    # In the reversed graph, this is the interval reached from the
    # virtual vertex, plus the cost of the prism.
    source_l = array('i')
    source_r = array('i')
    weight = array('q')

    def add_event(seg_idx, vid):
        event_v.append(vid)
        event_next.append(head[seg_idx])
        head[seg_idx] = len(event_v) - 1

    def add_interval_events(l, r, vid):
        """Attach vid to canonical nodes covering inclusive [l, r]."""
        l += S
        r += S + 1

        while l < r:
            if l & 1:
                add_event(l, vid)
                l += 1
            if r & 1:
                r -= 1
                add_event(r, vid)
            l >>= 1
            r >>= 1

    # Create both directions of every prism.
    for i in range(m):
        a, b, c, d, w = map(int, input().split())

        # Convert to zero-based positions in the combined 2*n array.
        a -= 1
        b -= 1
        c = n + c - 1
        d = n + d - 1

        # Direction: county 1 [a,b] -> county 2 [c,d].
        vid = VBASE + 2 * i
        source_l.append(a)
        source_r.append(b)
        weight.append(w)

        # In the reversed graph, destination [c,d] reaches vid at cost 0.
        add_interval_events(c, d, vid)

        # Direction: county 2 [c,d] -> county 1 [a,b].
        vid = VBASE + 2 * i + 1
        source_l.append(c)
        source_r.append(d)
        weight.append(w)

        # In the reversed graph, destination [a,b] reaches vid at cost 0.
        add_interval_events(a, b, vid)

    sources = [x - 1 for x in map(int, input().split())]
    targets = [n + y - 1 for y in map(int, input().split())]

    dist = array('q', [INF]) * total_nodes
    heap = []

    # Start from every enemy building in the downward-tree representation.
    for pos in targets:
        node = OUT_BASE + S + pos
        if dist[node] != 0:
            dist[node] = 0
            heapq.heappush(heap, (0, node))

    while heap:
        dcur, u = heapq.heappop(heap)
        if dcur != dist[u]:
            continue

        # Virtual prism vertex.
        if u >= VBASE:
            k = u - VBASE
            l = source_l[k] + S
            r = source_r[k] + S + 1
            nd = dcur + weight[k]

            # In the reversed graph, a virtual vertex reaches
            # canonical nodes covering its source interval in the
            # upward tree.
            while l < r:
                if l & 1:
                    v = l
                    node = v
                    if nd < dist[node]:
                        dist[node] = nd
                        heapq.heappush(heap, (nd, node))
                    l += 1

                if r & 1:
                    r -= 1
                    v = r
                    node = v
                    if nd < dist[node]:
                        dist[node] = nd
                        heapq.heappush(heap, (nd, node))

                l >>= 1
                r >>= 1

            continue

        # Downward-tree node.
        if u >= OUT_BASE:
            idx = u - OUT_BASE

            # Reverse of parent -> child is child -> parent.
            if idx > 1:
                v = OUT_BASE + (idx >> 1)
                if dcur < dist[v]:
                    dist[v] = dcur
                    heapq.heappush(heap, (dcur, v))

            # A leaf representing a real position is connected to
            # the same position in the upward tree.
            if idx >= S and idx < S + N:
                v = idx
                if dcur < dist[v]:
                    dist[v] = dcur
                    heapq.heappush(heap, (dcur, v))

            # Reverse prism edges: this canonical destination node
            # can enter every prism whose destination interval contains it.
            e = head[idx]
            while e != -1:
                v = event_v[e]
                if dcur < dist[v]:
                    dist[v] = dcur
                    heapq.heappush(heap, (dcur, v))
                e = event_next[e]

        # Upward-tree node.
        else:
            idx = u

            # Reverse of child -> parent is parent -> child.
            if idx < S:
                v = idx << 1
                if dcur < dist[v]:
                    dist[v] = dcur
                    heapq.heappush(heap, (dcur, v))

                v += 1
                if dcur < dist[v]:
                    dist[v] = dcur
                    heapq.heappush(heap, (dcur, v))

            # Same physical position, other representation.
            if idx >= S and idx < S + N:
                v = OUT_BASE + idx
                if dcur < dist[v]:
                    dist[v] = dcur
                    heapq.heappush(heap, (dcur, v))

    answer = 0

    for pos in sources:
        node = S + pos
        if dist[node] >= INF // 2:
            print("boring game")
            return
        if dist[node] > answer:
            answer = dist[node]

    print(answer)

if __name__ == "__main__":
    solve()
```Phần xây dựng đầu tiên chọn kích thước cây phân đoạn lũy thừa hai (S), điều này làm cho nút cha và nút con của nút phân đoạn trở nên đơn giản`idx >> 1`Và`idx << 1`. Cây kết hợp chứa cả hai hạt, vì vậy chỉ cần một cây phân đoạn là đủ. Vị trí của quận hai được dịch chuyển bởi (n), trong khi vị trí của quận một vẫn ở nửa đầu. 

Hai mảng`source_l`Và`source_r`lưu trữ khoảng ở phía nguồn của mỗi đỉnh lăng kính ảo. Khoảng thời gian đích không được lưu trữ riêng biệt vì các nút chuẩn của nó ngay lập tức được chuyển đổi thành các sự kiện trong quá trình xử lý đầu vào. Điều này giúp tiết kiệm một lượng bộ nhớ đáng kể so với việc lưu trữ hai danh sách kề rõ ràng cho mỗi lăng kính. 

Mảng sự kiện sử dụng mảng số nguyên thay vì danh sách bộ dữ liệu Python. Một bộ dữ liệu Python có chi phí đối tượng đáng kể, điều này sẽ nguy hiểm khi (10^5) mỗi lăng kính tạo ra các sự kiện cây phân đoạn (O(\log n)).`head`,`event_v`, Và`event_next`tạo thành một biểu diễn danh sách liên kết nhỏ gọn. 

Biểu đồ không bao giờ được cụ thể hóa như một danh sách kề thông thường. Các cạnh của cây phân đoạn được tạo trực tiếp từ chỉ mục nút trong Dijkstra. Khi một nút hướng lên được xử lý, các nút con của nó sẽ được tạo ra. Khi một nút hướng xuống được xử lý, nút cha của nó sẽ được tạo ra. Các cạnh duy nhất cần lưu trữ rõ ràng là các sự kiện lăng kính. 

Việc phân rã phạm vi sử dụng khoảng thời gian nửa mở ([l,r)) bên trong. Khoảng đầu vào ([l,r]) được chuyển đổi bằng cách đặt điểm cuối của cây phân đoạn thành`l + S`Và`r + S + 1`. Cái đó`+1`là cần thiết vì các khoảng thời gian của vấn đề đều mang tính chất bao hàm. 

Loại khoảng cách là một mảng 64-bit có dấu. Một đường dẫn có thể chứa nhiều chuyển đổi lăng kính, mỗi chuyển đổi có giá lên tới (10^9), vì vậy số nguyên 32 bit là không đủ. Số nguyên Python sẽ an toàn về mặt số lượng, nhưng`array('q')`giữ cho bảng khoảng cách nhỏ gọn. 

Kiểm tra mục nhập cũ`if dcur != dist[u]`thay thế một mảng được truy cập riêng biệt. Nếu một nút được cải thiện nhiều lần, các mục nhập cũ vẫn còn trong heap và chỉ mục nhập phù hợp với khoảng cách tốt nhất hiện tại mới được xử lý. 

Hai quận không được kết nối chỉ vì vị trí của chúng có cùng số lượng. Chuyển động xuyên quận duy nhất đến từ các đỉnh lăng kính. Cạnh chi phí bằng 0 giữa hai biểu diễn cây phân đoạn chỉ nằm giữa hai biểu diễn của cùng một vị trí vật lý. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 3 2 2
2 4 1 3 1
1 1 4 5 3
1 2 3 4 2
2 3
4 5
```Đơn vị chiến đấu đầu tiên bắt đầu ở vị trí (2) và đơn vị thứ hai bắt đầu ở vị trí (3). Công trình địch ở vị trí (4) và (5). 

Con đường trực tiếp hữu ích cho đơn vị đầu tiên là thông qua lăng kính thứ ba, từ vị trí quận một ([1,2]) đến vị trí quận hai ([3,4]), tính giá (2). Đơn vị thứ hai có thể sử dụng lăng kính thứ nhất từ ​​vị trí (3) đến vị trí quận hai (3), tính giá (1), sau đó sử dụng lăng kính thứ ba quay lại quận một và một đường ngang khác để tiếp cận tòa nhà của kẻ thù. Thời gian đến tốt nhất của nó là (4). 

| Bang Dijkstra | Khoảng cách | Ý nghĩa | 
| --- | --- | --- | 
| Tòa nhà 4 | 0 | Nguồn ban đầu | 
| Tòa nhà 5 | 0 | Nguồn ban đầu | 
| Lăng kính (1) đỉnh ngược | 0 | Khoảng đích của nó chứa tòa nhà 4 | 
| Lăng kính (3) đỉnh ngược | 0 | Khoảng đích của nó chứa tòa nhà 4 | 
| Vị trí nguồn 2 | 2 | Đơn vị chiến đấu đầu tiên tiếp cận một tòa nhà | 
| Vị trí nguồn 3 | 4 | Đơn vị chiến đấu thứ hai tiếp cận một tòa nhà | 

Khoảng cách ngắn nhất tối đa là (4), do đó đầu ra là`4`. Điều này chứng tỏ tại sao thao tác cuối cùng lại đạt mức tối đa trên các đường đi ngắn nhất riêng lẻ chứ không phải là tổng. 

### Mẫu 2 

Một ví dụ nhỏ thứ hai là```
3 1 1 1
1 2 2 3 5
2
3
```Lăng kính duy nhất cho phép các vị trí của quận một (1) và (2) tiếp cận các vị trí của quận hai (2) và (3) với chi phí (5). Đơn vị chiến đấu bắt đầu ở vị trí (2) và tòa nhà ở vị trí (3). 

| Bước | Đại diện hiện tại | Khoảng cách | Hoạt động | 
| --- | --- | --- | --- | 
| 1 | Tòa nhà 3, lá hướng xuống | 0 | Khởi tạo Dijkstra | 
| 2 | Nút chuẩn hướng xuống cho ([2,3]) | 0 | Leo lên cây ngược hướng xuống | 
| 3 | Lăng kính đỉnh ảo | 0 | Sự kiện khoảng thời gian đích | 
| 4 | Nút chuẩn hướng lên cho ([1,2]) | 5 | Trả chi phí lăng kính | 
| 5 | Lá hướng lên cho vị trí 2 | 5 | Đi xuống theo cây hướng lên ngược | 

Đơn vị chiến đấu duy nhất đến được tòa nhà trong (5) đơn vị thời gian, vì vậy câu trả lời là`5`. Dấu vết này chứng tỏ rằng chi phí lăng kính được thanh toán chính xác một lần, bất kể cần bao nhiêu nút cây phân đoạn để biểu thị một trong hai khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+m)\log^2 n)) | Mỗi lăng kính tạo ra các sự kiện (O(\log n)) và Dijkstra xử lý các chuyển đổi kết quả (O((n+m)\log n)) bằng hệ số heap logarit | 
| Không gian | (O(n+m\log n)) | Hai cây phân đoạn và bảng khoảng cách là tuyến tính, trong khi các sự kiện lăng kính yêu cầu bộ nhớ nhỏ gọn (O(m\log n)) | 

Các ràng buộc của vị trí (10^5) và lăng kính (10^5) loại trừ bất kỳ cách xây dựng nào tỷ lệ thuận với tích của các độ dài khoảng. Việc biểu diễn cây phân đoạn làm giảm mọi tương tác khoảng thời gian thành nhiều phép toán cấu trúc theo logarit. Việc triển khai cũng tránh các danh sách kề cận nặng về đối tượng Python, đặc biệt phù hợp với giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp được gửi có sẵn dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
# helper: run solution on input string, return output string
import sys
import io
from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """5 3 2 2
2 4 1 3 1
1 1 4 5 3
1 2 3 4 2
2 3
4 5
"""
) == "4", "provided sample"

# Custom 1: minimum-size input
assert run(
    """1 1 1 1
1 1 1 1 7
1
1
"""
) == "7", "minimum size"

# Custom 2: unreachable combat unit
assert run(
    """2 1 1 1
1 1 1 1 4
2
1
"""
) == "boring game", "unreachable source"

# Custom 3: both interval boundaries must be included
assert run(
    """4 1 1 1
1 1 4 4 3
1
4
"""
) == "3", "inclusive boundaries"

# Custom 4: duplicate positions and multiple prisms
assert run(
    """5 2 3 2
2 4 3 5 2
3 3 1 2 7
2 2 4
3 5
"""
) == "2", "duplicate positions and overlapping intervals"

# Custom 5: maximum n and m, while keeping every prism interval a singleton
m = 100000
lines = ["100000 100000 1 1"]
lines.extend(["1 1 1 1 1"] * m)
lines.append("1")
lines.append("1")
max_case = "\n".join(lines) + "\n"

assert run(max_case) == "1", "maximum n and m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu được cung cấp |`4`| Nhiều đơn vị và đường dẫn | 
| (n=1,m=1) |`7`| Đồ thị nhỏ nhất có thể | 
| Nguồn bị ngắt kết nối |`boring game`| Xử lý khoảng cách không thể tiếp cận | 
| Khoảng ranh giới Singleton |`3`| Điểm cuối khoảng bao gồm | 
| Vị trí trùng lặp, chồng chéo |`2`| Nguồn và mục tiêu lặp đi lặp lại | 
| (n=m=100000) với lăng kính đơn |`1`| Quy mô đầu vào tối đa và bộ nhớ nhỏ gọn | 

## Vỏ cạnh 

Trường hợp ngắt kết nối```
2 1 1 1
1 1 1 1 4
2
1
```bắt đầu đơn vị chiến đấu duy nhất ở vị trí quận một (2). Lăng kính duy nhất chỉ chấp nhận vị trí quận một (1), do đó không có con đường nào có thể rời khỏi vị trí (2). Dijkstra không bao giờ ấn định một khoảng cách hữu hạn cho lá hướng lên của nó. Lần quét cuối cùng phát hiện`INF`và in`boring game`. 

Trường hợp ranh giới bao gồm```
4 1 1 1
1 1 4 4 3
1
4
```có cả hai khoảng lăng kính bao gồm chính xác các vị trí biên của chúng. Sự phân rã của ([1,1]) và ([4,4]) mỗi cái tạo ra một lá cây phân đoạn duy nhất. Dijkstra đảo ngược tới lăng kính ảo ở khoảng cách bằng 0 và sau đó tới lá nguồn với khoảng cách (3). Câu trả lời là`3`. 

Trường hợp ngược chiều```
3 1 1 1
1 1 3 3 5
3
1
```yêu cầu sử dụng lăng kính từ khoảng thời gian được liệt kê thứ hai trở lại khoảng thời gian đầu tiên. Việc xây dựng rõ ràng tạo ra một đỉnh ảo thứ hai cho hướng này. Bắt đầu từ vị trí quận hai (1), Dijkstra đảo ngược đạt đến đỉnh ảo đó và sau đó là nguồn quận một với chi phí (5). Đầu ra là`5`. 

Trường hợp vị trí trùng lặp```
5 2 3 2
2 4 3 5 2
3 3 1 2 7
2 2 4
3 5
```có các đơn vị chiến đấu ở các vị trí (2,2,4), với hai bản sao của cùng một điểm cuối khoảng thời gian mục tiêu không có tòa nhà. Cả vị trí (2) và vị trí (4) đều có thể sử dụng lăng kính thứ nhất để tiếp cận tòa nhà có giá (2). Đơn vị chiến đấu trùng lặp có cùng khoảng cách với đơn vị khác ở vị trí (2). Do đó, mức tối đa là`2`. 

Thử nghiệm đầu vào lớn chứa (10^5) lăng kính, nhưng mỗi khoảng là một đơn lẻ. Mỗi lăng kính chỉ đóng góp một sự kiện cây phân đoạn chuẩn cho mỗi hướng, do đó các mảng sự kiện vẫn tuyến tính theo (m). Trường hợp này kiểm tra xem việc triển khai không phân bổ một bộ dữ liệu Python hoặc đối tượng danh sách cho mọi cạnh đồ thị tiềm năng và biểu diễn mảng số nguyên có tỷ lệ theo kích thước đầu vào lớn nhất hay không.
