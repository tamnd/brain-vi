---
title: "CF 102428A - Giảng dạy thuật toán"
description: "Mỗi giáo viên biết một tập hợp nhỏ các thuật toán. Một học sinh được giáo viên đó đào tạo có thể học bất kỳ tập con nào không trống của các thuật toán đó. Hai học sinh có thể cùng tồn tại trong đội cuối cùng khi tập hợp đã học của học sinh đó không chứa tập hợp đã học của học sinh kia."
date: "2026-08-14T15:30:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "A"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 156
verified: true
draft: false
---

[CF 102428A - Giảng dạy thuật toán](https://codeforces.com/problemset/problem/102428/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi giáo viên biết một tập hợp nhỏ các thuật toán. Một học sinh được giáo viên đó đào tạo có thể học bất kỳ tập con nào không trống của các thuật toán đó. Hai học sinh có thể cùng tồn tại trong đội cuối cùng khi tập hợp đã học của học sinh đó không chứa tập hợp đã học của học sinh kia. Nói cách khác, các tập hợp đã học phải tạo thành một phản chuỗi khi đưa vào tập hợp. 

Cùng một kiến ​​thức đã học có thể được cung cấp bởi nhiều giáo viên, nhưng điều đó không tạo ra nhiều học sinh hữu ích. Hai học sinh có cùng một bộ học tập có thể so sánh được vì các bộ bằng nhau nên cả hai không thể cùng thuộc một nhóm. Chúng tôi chỉ quan tâm đến các tập hợp con khả thi riêng biệt. 

Quan sát cấu trúc quan trọng là mọi tập hợp khả thi đều mang theo tất cả các tập hợp con của nó. Nếu giáo viên biết`{A, B, C}`, sau đó`{A}`,`{B}`,`{A,B}`, và mọi tập con khác đều khả thi. Do đó, các tập hợp khả thi tạo thành một họ đóng hướng xuống bên trong mạng Boolean của tất cả các tập hợp con. 

Các hạn chế là nhỏ ở khía cạnh quan trọng. Có tối đa 100 giáo viên và mỗi giáo viên biết tối đa 10 thuật toán. Do đó, một giáo viên đóng góp tối đa (2^{10}-1=1023) các tập huấn luyện không trống khác nhau. Đối với tất cả các giáo viên, có thể có tối đa khoảng 102.300 bộ khả thi riêng biệt. Con số này là quá nhiều đối với việc tìm kiếm theo cấp số nhân trên tất cả các thuật toán trên toàn cầu, vì các giáo viên có thể đề cập chung tới 1000 tên thuật toán khác nhau. Việc liệt kê mọi tập hợp con trong số 1000 thuật toán đó sẽ yêu cầu (2^{1000}-1) ứng viên. 

Bản thân câu trả lời tối đa cũng nằm trong phạm vi số nguyên thông thường. Nhiều nhất khoảng 102.300 học sinh riêng biệt có thể được biểu diễn bằng các tập huấn luyện riêng biệt, vì vậy số nguyên Python không có khó khăn số học đặc biệt nào ở đây. 

Một số trường hợp đặc biệt quan trọng vì chúng bộc lộ những đơn giản hóa không chính xác phổ biến. Với một giáo viên biết một thuật toán, tập huấn luyện khả thi duy nhất là singleton đó, vì vậy câu trả lời là 1.```
1
1 HAVEFUN
```Đầu ra là`1`. Một giải pháp vô tình cho phép tập hợp con trống sẽ có thêm một khả năng. 

Trường hợp thứ hai là hai giáo viên có thuật toán khác nhau.```
2
1 A
1 B
```Câu trả lời là`2`, bởi vì`{A}`Và`{B}`là không thể so sánh được. Một giải pháp chỉ xem xét các học sinh được đào tạo bởi cùng một giáo viên sẽ trả về sai 1. 

Kiến thức trùng lặp của giáo viên cũng phải được xử lý mà không cần nhân lên các tập huấn luyện giống hệt nhau.```
2
2 A B
2 A B
```Câu trả lời là`2`, không`4`. Các tập hợp khả thi là`{A}`,`{B}`, Và`{A,B}`và antichain lớn nhất bao gồm`{A}`Và`{B}`. 

Cuối cùng, đội giỏi nhất không nhất thiết phải bao gồm các bộ có cùng kích thước. Hãy xem xét mẫu thứ hai. Giáo viên đầu tiên đóng góp sáu cặp riêng biệt, trong khi giáo viên thứ hai đóng góp hai bộ đơn lẻ trên các thuật toán hoàn toàn khác nhau. Sáu cặp đó cùng với hai singleton tạo thành một phản chuỗi có kích thước 8. Chỉ tìm cấp độ đơn lớn nhất sẽ tìm thấy 7 và sẽ sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ xây dựng mọi tập hợp con của mọi thuật toán xuất hiện ở bất kỳ đâu trong đầu vào và sau đó tìm kiếm tập hợp lớn nhất gồm các tập hợp con không thể so sánh được theo cặp. Không gian tìm kiếm được xác định bởi tổng số tên thuật toán riêng biệt, không phải bởi số lượng thuật toán mà một giáo viên biết. Trong trường hợp xấu nhất có thể có 1000 tên riêng biệt, do đó, chỉ cần liệt kê tất cả các tập hợp con không trống có thể thực hiện các thao tác (2^{1000}-1). Việc kiểm tra tất cả các kết hợp của các tập hợp con đó sẽ còn tệ hơn nữa. Lực lượng vũ phu là chính xác vì nó xem xét rõ ràng mọi học sinh có thể, nhưng vũ trụ thuật toán toàn cầu khiến nó không thể sử dụng được. 

Quan sát hữu ích là chúng ta không cần mạng Boolean toàn cục. Một học sinh chỉ có thể được đào tạo về một tập hợp con gồm tối đa 10 thuật toán của một giáo viên. Chúng ta có thể liệt kê trực tiếp các tập hợp con cục bộ đó. Liên minh của họ cung cấp một tập hợp đầy đủ các sinh viên khả thi, với tối đa (100\cdot(2^{10}-1)) ứng cử viên trước khi loại bỏ các sinh viên trùng lặp. 

Bây giờ hãy coi mọi tập huấn luyện khả thi là một đỉnh của đồ thị chu kỳ có hướng. Đặt quan hệ thứ tự từ tập hợp (X) đến tập hợp (Y) bất cứ khi nào (X\tập hợp con Y). Một chuỗi trong biểu đồ này là một tập hợp các sinh viên có tập huấn luyện có thể so sánh được lẫn nhau, do đó, một antichain chính xác là một nhóm thỏa mãn điều kiện hợp tác. 

Định lý Dilworth đưa ra sự rút gọn chính: trong bất kỳ tập hữu hạn có thứ tự từng phần nào, kích thước của một phản chuỗi tối đa bằng số lượng chuỗi tối thiểu cần thiết để bao phủ tất cả các đỉnh. Độ che phủ chuỗi tối thiểu có thể đạt được từ sự khớp tối đa trong biểu đồ lưỡng cực. Chúng ta tạo hai bản sao của mỗi tập hợp khả thi, đặt bản sao bên trái ở bên thứ nhất và bản sao bên phải ở bên thứ hai, đồng thời nối trái (X) với phải (Y) bất cứ khi nào (X) là tập con thực sự của (Y). Nếu kết quả khớp tối đa có kích thước (M) và có các tập khả thi (V), thì bìa chuỗi tối thiểu có chuỗi (V-M), đây là câu trả lời mong muốn. 

Có một sàng lọc triển khai cụ thể cho vấn đề này. Chỉ cần sử dụng các cạnh bao gồm khác nhau bởi đúng một thuật toán là đủ. Giả sử (X\tập hợp con Y) và cả hai bộ đều khả thi. Vì họ khả thi là đóng hướng xuống nên mọi tập thu được bằng cách cộng các phần tử của (Y\setminus X) lần lượt cũng khả thi. Do đó, mối quan hệ tập hợp con có thể được biểu diễn bằng các đường dẫn thông qua các phần mở rộng một phần tử này. Đối với họ này, sự phân rã chuỗi tối thiểu có thể được bão hòa theo cách này, do đó đồ thị phù hợp chỉ cần các cạnh phủ. 

Đối với một giáo viên có thuật toán (k), có (2^k) tập hợp con bao gồm cả tập trống. Mỗi tập hợp con có nhiều nhất (k) phần mở rộng một phần tử, do đó số cạnh có hướng được tạo ra nhiều nhất là (k2^{k-1}). Với (k\le10) và (N\le100), đây là tối đa 512.000 cạnh được tạo trước khi loại bỏ các cạnh trùng lặp. 

Kết quả khớp tối đa được tính toán bằng Hopcroft-Karp. Giai đoạn BFS phân lớp của nó tìm ra các đường tăng cường ngắn nhất, trong khi DFS sau đó tăng dọc theo tất cả các đường dẫn có thể có trong biểu đồ lớp đó. Điều này phù hợp hơn nhiều so với việc cố gắng so sánh từng cặp tập hợp con khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^K)) chỉ để liệt kê các ứng cử viên, (K\le1000) | (O(2^K)) | Quá chậm | 
| Tối ưu | (O(NK2^K + E\sqrt V)), với (K\le10), (V\le N2^K), (E\le NK2^{K-1}) | (O(V+E)) | Đã chấp nhận | 

Ở đây (K) trong biểu thức độ phức tạp cho phép liệt kê cục bộ có nghĩa là số lượng thuật toán tối đa được biết bởi một giáo viên, do đó (K\le10), không phải là 1000 tên thuật toán khác biệt trên toàn cầu. 

## Hướng dẫn thuật toán

1. Gán cho mỗi tên thuật toán một vị trí bit toàn cục duy nhất. Kiến thức của giáo viên sau đó có thể được biểu diễn bằng bitmask. Điều này mang lại cho mỗi tập huấn luyện một biểu diễn nhỏ gọn, có thể băm được ngay cả khi các giáo viên khác nhau chia sẻ tên thuật toán. 
2. Đối với mỗi giáo viên, hãy liệt kê tất cả các mặt nạ con không trống trong mặt nạ của giáo viên đó. Chèn từng mặt nạ chung thu được vào một từ điển ánh xạ tập huấn luyện tới một ID đỉnh duy nhất. Các tập hợp con trùng lặp từ các giáo viên khác nhau sẽ nhận được cùng một ID vì chúng đại diện cho cùng một học sinh. 
3. Tạo danh sách kề cho đồ thị khớp hai bên. Đối với mọi giáo viên và mọi tập con không trống (S) trong thuật toán của giáo viên đó, hãy thử thêm từng thuật toán chưa có trong (S). Tập kết quả (T) khác với (S) đúng một thuật toán, do đó hãy thêm một cạnh từ (S) vào (T). 
4. Loại bỏ các lân cận trùng lặp khỏi mỗi danh sách kề. Một cạnh bao hàm giống nhau có thể được tạo ra bởi một số giáo viên khi họ chia sẻ các thuật toán liên quan, nhưng nó chỉ thể hiện một mối quan hệ trong tập hợp. 
5. Chạy Hopcroft-Karp trên biểu đồ lưỡng cực. Bên trái và bên phải đều chứa tất cả các tập huấn luyện khả thi. Một cạnh có nghĩa là một tập hợp khả thi có thể ngay trước một tập hợp khác trong chuỗi bao hàm bão hòa. 
6. Gọi (V) là số tập hợp khả thi riêng biệt và (M) kích thước khớp tối đa. Theo dạng phủ chuỗi của định lý Dilworth, số chuỗi tối thiểu là (V-M). Vì một nhóm chính xác là một antichain, nên in (V-M). 

### Tại sao nó hoạt động 

Tập đỉnh chính xác là tập hợp các tập huấn luyện học sinh có thể có, không trống, do đó không có học sinh hợp lệ nào bị bỏ qua và không có học sinh không thể nào được đưa vào. Sự bao gồm xác định thứ tự từng phần bắt buộc vì hai học sinh không hợp tác chính xác khi tập huấn luyện của họ có thể so sánh được. 

Mọi quan hệ bao hàm đều có thể được mở rộng thành các phép cộng một phần tử vì họ khả thi là đóng xuống. Nếu (X\tập hợp con Y) khả thi thì mọi tập con trung gian giữa chúng cũng được chứa trong (Y), do đó khả thi. Do đó, cùng một thứ tự chuỗi có thể được biểu diễn chỉ bằng cách sử dụng các cạnh thêm một thuật toán tại một thời điểm. Do đó, một chuỗi trong biểu đồ được xây dựng chính xác là một chuỗi các tập huấn luyện có thể so sánh được. 

Định lý Dilworth nói rằng antichain lớn nhất có thể có cùng kích thước với vỏ chuỗi nhỏ nhất. Việc rút gọn lưỡng cực tiêu chuẩn nói rằng, đối với một poset được biểu thị bằng các quan hệ bao hàm của nó, kích thước bìa chuỗi tối thiểu là (V-M), trong đó (M) là mức khớp tối đa giữa hai bản sao của poset. Hopcroft-Karp tìm thấy sự trùng khớp tối đa đó nên giá trị được thuật toán in ra chính xác là giá trị lớn nhất có thể của nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

def solve():
    n = int(input())

    name_id = {}
    teachers = []

    for _ in range(n):
        parts = input().split()
        k = int(parts[0])

        mask = 0
        for name in parts[1:]:
            if name not in name_id:
                name_id[name] = len(name_id)
            mask |= 1 << name_id[name]

        teachers.append(mask)

    # Assign one vertex id to every distinct non-empty feasible subset.
    vertex_id = {}
    masks_by_teacher = []

    for teacher_mask in teachers:
        bits = []
        x = teacher_mask
        while x:
            b = x & -x
            bits.append(b)
            x -= b

        k = len(bits)
        local_masks = [0] * (1 << k)

        for lm in range(1, 1 << k):
            lb = lm & -lm
            j = lb.bit_length() - 1
            local_masks[lm] = local_masks[lm ^ lb] | bits[j]

        masks_by_teacher.append((bits, local_masks))

        for lm in range(1, 1 << k):
            mask = local_masks[lm]
            if mask not in vertex_id:
                vertex_id[mask] = len(vertex_id)

    v = len(vertex_id)

    # Build cover edges: S -> S union {x}.
    adj = [[] for _ in range(v)]

    for bits, local_masks in masks_by_teacher:
        k = len(bits)

        for lm in range(1, 1 << k):
            u_mask = local_masks[lm]
            u = vertex_id[u_mask]

            missing = ((1 << k) - 1) ^ lm
            while missing:
                lb = missing & -missing
                missing -= lb

                j = lb.bit_length() - 1
                v_mask = u_mask | bits[j]
                w = vertex_id[v_mask]
                adj[u].append(w)

    # The same edge may have been generated by several teachers.
    for u in range(v):
        if len(adj[u]) > 1:
            adj[u] = list(set(adj[u]))

    # Hopcroft-Karp maximum matching.
    pair_u = [-1] * v
    pair_v = [-1] * v
    dist = [-1] * v

    from collections import deque

    def bfs():
        q = deque()

        for u in range(v):
            if pair_u[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = -1

        found = False

        while q:
            u = q.popleft()

            for w in adj[u]:
                pu = pair_v[w]

                if pu == -1:
                    found = True
                elif dist[pu] == -1:
                    dist[pu] = dist[u] + 1
                    q.append(pu)

        return found

    def dfs(u):
        for w in adj[u]:
            pu = pair_v[w]

            if pu == -1 or (
                dist[pu] == dist[u] + 1 and dfs(pu)
            ):
                pair_u[u] = w
                pair_v[w] = u
                return True

        dist[u] = -1
        return False

    matching = 0

    while bfs():
        for u in range(v):
            if pair_u[u] == -1 and dfs(u):
                matching += 1

    print(v - matching)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào đầu tiên chuyển đổi mọi tên thuật toán thành một vị trí bit. Sau đó, giáo viên được lưu trữ dưới dạng một số nguyên có các bit xác định chính xác các thuật toán mà giáo viên biết. 

các`local_masks`mảng rất hữu ích vì các vị trí bit chung có thể cách xa nhau. Đối với một giáo viên có thuật toán (k), mặt nạ cục bộ của nó dao động từ`0`ĐẾN`(1 << k) - 1`và mỗi bit cục bộ tương ứng với một bit thuật toán toàn cục. Điều này cho phép chúng tôi liệt kê tất cả (2^k-1) tập hợp con không trống mà không cần lặp lại trên 1000 vị trí thuật toán toàn cầu tiềm năng. 

các`vertex_id`từ điển là cơ chế chống trùng lặp. Nếu hai giáo viên có thể đào tạo một học sinh theo cùng một tập hợp con thuật toán thì cả hai lần xuất hiện đều ánh xạ tới cùng một đỉnh. Điều này là cần thiết vì hai tập huấn luyện giống hệt nhau không thể hợp tác được. 

Việc xây dựng liền kề chỉ thêm phần mở rộng một phần tử. các`missing`mặt nạ chứa các thuật toán của giáo viên không có trong tập hợp con hiện tại. Lấy bit được đặt thấp nhất liên tục để liệt kê mọi thuật toán tiếp theo có thể có. 

Các mảng phù hợp có một mục nhập cho mỗi bộ khả thi.`pair_u[u]`lưu trữ đỉnh bên phải khớp với đỉnh bên trái`u`, trong khi`pair_v[v]`lưu trữ đỉnh trái tương ứng. BFS xây dựng biểu đồ phân lớp được Hopcroft-Karp sử dụng và DFS chỉ tìm kiếm dọc theo các cạnh tôn trọng các lớp đó. 

Không có vấn đề tràn số nguyên trong Python. Độ sâu đệ quy của DFS cũng nhỏ trong vấn đề này vì mỗi cạnh khớp sẽ tăng kích thước tập hợp con thêm một dọc theo đường bao, nhưng dù sao thì giới hạn đệ quy vẫn được nâng lên để giúp việc triển khai trở nên mạnh mẽ. 

Phép trừ cuối cùng là bước Dilworth quan trọng. Mỗi cạnh khớp nhau cho phép hai đỉnh mà nếu không sẽ yêu cầu các chuỗi riêng biệt được nối thành một chuỗi. Bắt đầu từ các chuỗi đơn (V), việc khớp kích thước (M) sẽ giảm số lượng chuỗi cần thiết xuống còn (V-M), bằng với kích thước phản chuỗi tối đa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào chứa một giáo viên biết ba thuật toán. Có bảy tập con khả thi không trống. 

| Bước | Kích thước tập hợp con | Bộ khả thi mới | Tổng số đỉnh | 
| --- | --- | --- | --- | 
| Liệt kê các đơn vị | 1 | 3 | 3 | 
| Liệt kê các cặp | 2 | 3 | 6 | 
| Liệt kê ba | 3 | 1 | 7 | 
| Xây dựng các cạnh bìa | | 12 | 7 | 
| Kết hợp tối đa | | 4 đỉnh trùng khớp | 7 | 
| Câu trả lời cuối cùng | | (7-4) | 3 | 

Ba bộ cặp tạo thành một phản chuỗi có kích thước 3. Sự kết hợp có kích thước 4, do đó Dilworth đưa ra một chuỗi chuỗi tối thiểu gồm bốn chuỗi và do đó một chuỗi phản chuỗi tối đa là ba bộ. 

### Mẫu 2 

Giáo viên đầu tiên biết 4 thuật toán nên đóng góp tất cả 15 tập con khác rỗng của 4 thuật toán đó. Giáo viên thứ hai biết hai thuật toán hoàn toàn khác nhau, đóng góp thêm ba tập hợp con. Vì các bộ thuật toán rời rạc nên có 18 đỉnh riêng biệt. 

| Bước | Nguồn | Đỉnh mới | Tổng số đỉnh | 
| --- | --- | --- | --- | 
| Liệt kê các tập hợp con | Thầy 1 | 15 | 15 | 
| Liệt kê các tập hợp con | Thầy 2 | 3 | 18 | 
| Xây dựng các cạnh bìa | Thầy 1 | 32 | 32 | 
| Xây dựng các cạnh bìa | Thầy 2 | 2 | 34 | 
| Kết hợp tối đa | Cả hai | 10 | 18 | 
| Câu trả lời cuối cùng | | (18-10) | 8 | 

Phần thú vị là cấu trúc của nhóm tối ưu. Từ giáo viên đầu tiên, lấy tất cả sáu tập hợp con hai thuật toán. Từ giáo viên thứ hai, lấy hai tập con đơn của nó. Hai nhóm sử dụng các thuật toán khác nhau nên không có tập hợp nào được chọn chứa nhóm khác. Điều này mang lại cho 8 sinh viên. 

Ví dụ này chính xác là lý do tại sao chỉ đếm mức lớn nhất là không đủ. Cấp độ lớn nhất của cả nhóm chứa 7 bộ, nhưng một antichain cấp bậc hỗn hợp đạt tới 8. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(NK2^K + E\sqrt V)) | Mỗi giáo viên tạo ra (O(K2^K)) cạnh bìa, tiếp theo là Hopcroft-Karp | 
| Không gian | (O(V+E)) | Lưu trữ các tập khả thi riêng biệt, danh sách kề và mảng khớp | 

Ở đây (K\le10), (V\le N(2^K-1)), và số cạnh bìa được tạo ra nhiều nhất là (NK2^{K-1}). Với (N=100) và (K=10), điều đó có nghĩa là tối đa khoảng 102.300 đỉnh và 512.000 cạnh được tạo trước khi loại bỏ trùng lặp. Các giới hạn này được cố tình dựa trên số lượng thuật toán nhỏ trên mỗi giáo viên, thay vì số lượng tên thuật toán khác biệt trên toàn cầu có thể lớn hơn nhiều. 

## Trường hợp thử nghiệm```python
# Save the competitive-programming solution as solution.py first.

import io
import sys

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("""1
3 DFS BFS DIJKSTRA
""") == "3", "sample 1"

assert run("""2
4 BFS DFS LCA RMQ
2 PRIM KRUSKAL
""") == "8", "sample 2"

assert run("""4
3 BFS DFS DIJKSTRA
4 BFS DFS LCA RMQ
3 DIJKSTRA BFS DFS
3 FLOYD DFS BFS
""") == "10", "sample 3"

assert run("""1
1 HAVEFUN
""") == "1", "sample 4"

assert run("""3
4 FFEK DANTZIG DEMOUCRON FFT
4 PRIM KRUSKAL LCA FLOYD
4 DFS BFS DIJKSTRA RMQ
""") == "18", "sample 5"

# Minimum-size input.
assert run("""1
1 A
""") == "1", "single possible student"

# Two disjoint singleton teachers.
assert run("""2
1 A
1 B
""") == "2", "disjoint singleton sets"

# All teachers have exactly the same knowledge.
same_teachers = "100\n" + "2 A B\n" * 100
assert run(same_teachers) == "2", "duplicate teachers"

# One teacher with 10 algorithms.
# Its Boolean lattice has maximum antichain size C(10, 5) = 252.
names = " ".join(f"A{i}" for i in range(10))
assert run(f"""1
10 {names}
""") == "252", "maximum local subset size"

# Mixed ranks are required for the optimum.
assert run("""2
4 A B C D
2 E F
""") == "8", "mixed-rank antichain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 A`| 1 | Yêu cầu đầu vào có kích thước tối thiểu và tập hợp con không trống | 
|`2 / 1 A / 1 B`| 2 | Học sinh được đào tạo bởi các giáo viên khác nhau có thể cùng tồn tại | 
| 100 giống hệt nhau`2 A B`giáo viên | 2 | Giáo viên trùng lặp không được trùng đỉnh | 
| Một giáo viên với 10 thuật toán | 252 | Kích thước tập hợp con cục bộ tối đa và phản chuỗi mạng Boolean trung tâm | 
|`4 A B C D`Và`2 E F`| 8 | Antichain tối đa có thể kết hợp các kích thước tập hợp con khác nhau | 

## Vỏ cạnh 

Trường hợp thuật toán đơn được xử lý vì việc liệt kê tập hợp con bắt đầu ở mặt nạ cục bộ 1, do đó tập huấn luyện trống không bao giờ được chèn vào. Vì```
1
1 HAVEFUN
```đỉnh duy nhất là`{HAVEFUN}`, không có cạnh nào, kích thước phù hợp là 0 và câu trả lời là (1-0=1). 

Các giáo viên rời rạc được xử lý một cách tự nhiên vì các tập hợp con của họ trở thành các mặt nạ toàn cầu khác nhau. Vì```
2
1 A
1 B
```các đỉnh là`{A}`Và`{B}`. Không có ranh giới bao hàm giữa chúng, vì vậy kết quả khớp tối đa là 0 và câu trả lời là 2. Điều này cho thấy thực tế là sự hợp tác chỉ so sánh các thuật toán mà hai học sinh đã học chứ không phải những thuật toán được giáo viên đào tạo. 

Giáo viên trùng lặp bị sụp đổ bởi`vertex_id`. Vì```
2
2 A B
2 A B
```cả hai giáo viên đều tạo ra chính xác ba đỉnh giống nhau. Các cạnh bìa được`{A}->{A,B}`Và`{B}->{A,B}`. Kết quả khớp tối đa có kích thước 1, vì vậy câu trả lời là (3-1=2). Nếu không loại bỏ trùng lặp, một chương trình có thể xử lý không chính xác cùng một tập huấn luyện của nhiều học viên khác nhau. 

Ranh giới mười thuật toán được xử lý mà không có trường hợp đặc biệt nào. Một giáo viên với 10 thuật toán tạo ra chính xác 1023 tập con không trống và 512 cạnh mở rộng một phần tử. Đối với một giáo viên như vậy, mạng Boolean có một phản chuỗi trung tâm có kích thước (\binom{10}{5}=252), công thức đối sánh thu được là (1023-771=252). 

Trường hợp xếp hạng hỗn hợp là mang tính hướng dẫn nhất. Vì```
2
4 A B C D
2 E F
```giáo viên đầu tiên cung cấp sáu tập hợp con hai phần tử, trong khi giáo viên thứ hai cung cấp`{E}`Và`{F}`. Không có cặp nào sau đây chứa hoặc được chứa trong bất kỳ cặp nào trong số sáu cặp, vì vậy tất cả tám bộ có thể được chọn đồng thời. Thuật toán không cho rằng một antichain tối ưu nằm ở một cấp độ. Định lý Dilworth tự động tìm ra giá trị tối ưu xếp hạng hỗn hợp và trả về 8.
