---
title: "CF 102367D - Giao hàng"
description: "Các nhà kho và các con đường tạo thành một cây nên giữa hai nhà kho bất kỳ chỉ có duy nhất một tuyến đường. Một chiếc xe tải có dung lượng pin T và nó có thể sạc lại bất cứ khi nào Sam dừng lại."
date: "2026-08-12T23:36:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102367
codeforces_index: "D"
codeforces_contest_name: "Fall 2019 ICPC-style Waterloo Local Contest"
rating: 0
weight: 102367
solve_time_s: 546
verified: true
draft: false
---

[CF 102367D - Giao hàng](https://codeforces.com/problemset/problem/102367/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 6 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Các nhà kho và các con đường tạo thành một cây nên giữa hai nhà kho bất kỳ chỉ có duy nhất một tuyến đường. Một chiếc xe tải có dung lượng pin`T`và nó có thể sạc lại bất cứ khi nào Sam dừng lại. Mỗi kho trên tuyến là điểm dừng bắt buộc, còn điểm dừng giữa đường là điểm dừng sạc tùy chọn. 

Hãy xem xét một con đường dài`D`. Khởi động với ắc quy đầy, xe tải có thể đi được tối đa`T`km trước khi cần dừng sạc khác. Vì vậy việc đi qua con đường này đòi hỏi`ceil(D / T)`các phân khúc du lịch. Phân đoạn đầu tiên bắt đầu tại kho trước đó và mỗi phân đoạn bổ sung cần có một điểm dừng trung gian. Vì mỗi nhà kho đều đã được tính là điểm dừng nên phần đóng góp của con đường này có thể được xếp gọn vào công thức`1 + ceil(D / T)`cho toàn bộ đường dẫn, trong đó phần bổ sung`1`chiếm kho đầu tiên của tuyến đường. 

Đối với đường dẫn chứa các cạnh có độ dài`D1, D2, ..., Dk`, câu trả lời là do đó`1 + sum(ceil(Di / T))`. 

Bản thân cây có tới`100000`các đỉnh và cùng thứ tự độ lớn của các truy vấn. Độ dài cạnh và dung lượng pin đều cao nhất`20000`. Một truy vấn không thể đi qua toàn bộ đường dẫn cây của nó, bởi vì một đường dẫn có thể chứa`99999`các cạnh và làm điều đó cho`100000`truy vấn cung cấp gần như`10^10`các chuyến thăm cạnh. Kể cả bình thường`O(log N)`Bản thân các truy vấn cây là không đủ vì chi phí cạnh phụ thuộc vào truy vấn`T`. 

Giới hạn nhỏ của`20000`trên chiều dài cạnh là nửa sau của cấu trúc. chức năng`ceil(D / T)`chỉ phụ thuộc vào độ dài cạnh`D`, vì vậy để cố định`T`nó trở thành trọng lượng cạnh tĩnh. Giải pháp kết hợp quan sát này với cây phân đoạn ổn định theo chiều dài cạnh. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Nếu độ dài cạnh chia hết cho`T`, nó cần chính xác`D/T`các đoạn du lịch, không`D/T + 1`. Ví dụ,```
2 1
1 2 10
1 2 5
```có câu trả lời`3`, vì cần có hai điểm dừng kho cộng với một điểm dừng sạc. Một công thức sử dụng`floor(D/T)`vì số lần sạc sẽ giảm đi một. 

Nếu một cạnh có độ dài tối đa`T`, nó không yêu cầu dừng sạc trung gian. Ví dụ,```
2 1
1 2 5
1 2 10
```có câu trả lời`2`. Xe tới kho 2 mà không dừng giữa chừng nên tính một lần dừng sạc chỉ vì có cạnh sẽ không chính xác. 

Một tuyến đường có thể chứa nhiều điểm dừng bắt buộc tại kho ngay cả khi pin không bao giờ cần sạc lại. Ví dụ,```
3 1
1 2 1
2 3 1
1 3 10
```có câu trả lời`3`, vì Sam phải dừng ở kho 1, 2 và 3. Chỉ đếm các điểm dừng sạc sẽ tạo ra 0 hoặc 1 không chính xác. 

Cuối cùng, một con đường rất dài không phải là không thể. Sam được phép dừng lại ở một điểm tùy ý trên đường, nạp năng lượng và đi tiếp. Ví dụ,```
2 1
1 2 20
1 2 5
```có câu trả lời`5`: bốn chặng hành trình yêu cầu ba điểm dừng sạc trung gian, cộng với hai điểm dừng kho. Việc triển khai loại bỏ các cạnh dài hơn`T`sẽ giải quyết được một vấn đề khác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là tìm đường đi duy nhất giữa`S`Và`F`, kiểm tra mọi cạnh trên đường dẫn đó và thêm`ceil(D/T)`, cuối cùng thêm một cái cho kho đầu tiên. Điều này đúng vì mọi cạnh đều có thể được xem xét độc lập. Xe tải có thể sạc lại ở cuối một cạnh trước khi vào cạnh tiếp theo, do đó số điểm dừng sạc tối ưu ở một cạnh không tương tác với các cạnh khác. 

Vấn đề là độ dài đường dẫn. Một cái cây có thể là một chuỗi`100000`các đỉnh, do đó một truy vấn có thể kiểm tra`99999`các cạnh. Với`100000`những truy vấn như vậy, trường hợp xấu nhất là đại khái`10^10`hoạt động cạnh, vượt xa những gì giới hạn thời gian cho phép. 

Quan sát hữu ích đầu tiên là đối với dung lượng pin cố định`T`, mọi cạnh đều nhận được trọng số tĩnh`ceil(D/T)`. Khi các trọng số đó được cố định, tổng tiền tố gốc thông thường cộng với LCA sẽ trả lời tổng đường dẫn trong thời gian không đổi sau khi LCA được tìm thấy. 

Cái khó đó là`T`thay đổi từ truy vấn này sang truy vấn khác. chỉ có`20000`các giá trị có thể có của`T`, nhưng xây dựng một mảng tiền tố gốc hoàn chỉnh cho mọi khả năng`T`sẽ yêu cầu về`2 * 10^9`giá trị được lưu trữ, quá nhiều. 

Quan sát thứ hai là bản thân chiều dài cạnh cũng bị giới hạn bởi`20000`. Đối với lớn`T`, hàm`ceil(D/T)`chỉ thay đổi một số lần nhỏ`D`dao động từ`1`ĐẾN`20000`. Ví dụ, nếu`T = 200`, các giá trị có thể chỉ là`1, 2, ..., 100`. Chúng ta có thể biểu diễn độ dài cạnh của đường đi dưới dạng phân bố tần số và tính tổng các phạm vi không đổi này thay vì kiểm tra từng cạnh. 

Cây phân đoạn liên tục cung cấp chính xác phân bố tần số đường dẫn cần thiết. Gốc cây ban đầu ở đỉnh 1 và liên kết mọi đỉnh không phải gốc với độ dài của cạnh nối nó với đỉnh gốc của nó. Với mọi đỉnh`v`, hãy xây dựng một phiên bản cây phân đoạn ổn định chứa độ dài cạnh trên đường đi từ gốc tới`v`. Tập hợp các độ dài cạnh trên đường đi từ`u`ĐẾN`v`sau đó được lấy từ các phiên bản của`u`,`v`và LCA của họ:`root[u] + root[v] - 2 * root[lca(u,v)]`. 

Bên trong cây phân đoạn cố định, toàn bộ khoảng giá trị có thể được xử lý cùng một lúc bất cứ khi nào`ceil(D/T)`không đổi trong suốt khoảng thời gian đó. Đối với lớn`T`, chỉ tồn tại một số lượng nhỏ các khoảng như vậy. 

Đối với nhỏ`T`, chiến lược ngược lại là tốt hơn. Chúng tôi tính toán trước tổng tiền tố gốc cho mỗi số nhỏ`T`xảy ra trong các truy vấn. Một ngưỡng của`200`cho nhiều nhất`200 * 100000 = 20 million`giá trị tiền tố, phù hợp thoải mái khi được lưu trữ dưới dạng số nguyên 32 bit. Truy vấn nhỏ`T`thì chỉ cần một LCA và ba lần truy cập mảng. 

Đây là sự cân bằng theo kiểu căn bậc hai tiêu chuẩn. Dung lượng nhỏ thì rẻ khi tính toán trước, trong khi dung lượng lớn có ít phạm vi thương riêng biệt và rẻ để đánh giá từ cấu trúc ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(NQ)`|`O(N)`| Quá chậm | 
| Tối ưu |`O(NB + Q(D/B)log D + Nlog N)`|`O(NB + Nlog D + Nlog N)`| Đã chấp nhận | 

Đây`D <= 20000`là độ dài cạnh tối đa và`B`là ngưỡng công suất nhỏ được sử dụng để triển khai,`200`trong mã. 

## Hướng dẫn thuật toán 

1. Gốc cây kho tại đỉnh`1`. Đối với mỗi đỉnh, lưu trữ đỉnh gốc, độ sâu và độ dài của cạnh gốc của nó. Cạnh cha được liên kết với cạnh con, do đó đường dẫn từ gốc tới đỉnh chứa mỗi cạnh đúng một lần. 
2. Xây dựng bàn nâng nhị phân cho phụ huynh. Điều này cho phép chúng tôi tìm thấy`lca(S,F)`TRONG`O(log N)`, xác định phần chung của hai đường dẫn gốc. 
3. Đọc tất cả các truy vấn trước khi thực hiện quá trình tiền xử lý phụ thuộc vào dung lượng. Đếm những giá trị nhỏ nào của`T`thực sự xảy ra. Không có lý do gì để xây dựng một mảng tiền tố cho dung lượng mà không có truy vấn nào sử dụng. 
4. Đối với mỗi lần sử dụng`T <= B`, xây dựng một mảng tiền tố gốc. Đối với một đỉnh`v`với cha mẹ`p`, bộ`pref[v] = pref[p] + ceil(edge[v] / T)`. 

Một truy vấn có khả năng này sau đó được trả lời bởi`pref[S] + pref[F] - 2 * pref[LCA] + 1`. 
5. Xây dựng phiên bản cây phân đoạn ổn định cho mọi đỉnh. Phiên bản`v`được lấy từ phiên bản`parent[v]`bằng cách chèn chính xác một giá trị, độ dài của cạnh từ`parent[v]`ĐẾN`v`. Kiên trì có nghĩa là các phiên bản cũ hơn không thay đổi. 
6. Đối với một truy vấn có`T`lớn hơn`B`, trước tiên hãy tìm LCA của nó và số cạnh đường đi của nó. Nếu như`T`ít nhất là độ dài cạnh tối đa trong toàn bộ cây, mỗi cạnh đều có đóng góp`1`, vì vậy câu trả lời chỉ đơn giản là số cạnh đường dẫn cộng`1`. 
7. Ngược lại, tính tổng của`ceil(D/T)`trên đường dẫn bằng cách sử dụng ba phiên bản liên tục. Tại một nút cây phân đoạn đại diện`[lo, hi]`, tính giá trị của`ceil(D/T)`ở cả hai điểm cuối. Nếu chúng bằng nhau thì mọi độ dài cạnh trong toàn bộ phân đoạn này đều có cùng mức đóng góp, vì vậy hãy nhân phần đóng góp đó với số cạnh đường dẫn được biểu thị bởi nút và dừng giảm dần. 
8. Nếu phần đóng góp khác nhau bên trong phân khúc, hãy chuyển xuống hai phần tử con của nó. Số lượng từ hai phiên bản điểm cuối được thêm vào và phiên bản LCA bị trừ hai lần, đưa ra chính xác tần suất độ dài cạnh từ phân đoạn con đó trên đường dẫn được yêu cầu. 
9. Thêm`1`sau khi sự đóng góp của cạnh đường dẫn đã được tính toán. Đây là điểm dừng kho ban đầu. Mỗi điểm dừng kho khác đã được đại diện ngầm bởi`ceil(D/T)`sự đóng góp của cạnh trước. 

### Tại sao nó hoạt động 

Đối với mỗi con đường dài`D`, chính xác`ceil(D/T)`phân khúc du lịch cỡ pin là cần thiết. Giữa các đoạn liên tiếp phải có điểm dừng sạc, còn các kho hàng ở cuối tuyến và giữa các tuyến đường là điểm dừng bắt buộc. Băng qua một con đường với`k`các cạnh, tổng số chính xác là`1 + sum ceil(D/T)`. 

Đối với nhỏ`T`, mảng tiền tố gốc lưu trữ chính xác phần đóng góp của cạnh này trên mọi đường dẫn gốc, do đó phép trừ đường dẫn cây tiêu chuẩn sẽ cho tổng chính xác. 

Đối với lớn`T`, các phiên bản cây phân đoạn cố định thể hiện chính xác nhiều tập hợp độ dài cạnh trên mỗi đường dẫn gốc. Việc cộng các phiên bản của hai điểm cuối và trừ phiên bản LCA hai lần sẽ mang lại chính xác nhiều cạnh trên đường dẫn được yêu cầu. Mỗi phân đoạn trên đó`ceil(D/T)`là hằng số có thể được thay thế bằng tần số của nó nhân với hằng số đó. Do đó, chỉ giảm dần khi giá trị thay đổi sẽ tính tổng tương tự mà không kiểm tra các cạnh đường dẫn riêng lẻ. 

Bất biến trong cả hai trường hợp là mọi cạnh trên đường đi của cây được yêu cầu đều đóng góp chính xác`ceil(D/T)`một lần và không có cạnh nào bên ngoài đường dẫn đó đóng góp được gì. trận chung kết`+1`chiếm kho đầu tiên, hoàn thành số lần dừng được yêu cầu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MAX_D = 20000
B = 200

def solve():
    n, q = map(int, input().split())

    head = array('i', [-1]) * n
    to = array('i')
    nxt = array('i')
    ew = array('i')

    def add_edge(u, v, w):
        idx = len(to)
        to.append(v)
        ew.append(w)
        nxt.append(head[u])
        head[u] = idx

    max_edge = 0

    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        add_edge(u, v, w)
        add_edge(v, u, w)
        if w > max_edge:
            max_edge = w

    queries = []
    used_small = set()

    for _ in range(q):
        s, f, t = map(int, input().split())
        s -= 1
        f -= 1
        queries.append((s, f, t))
        if t <= B and t < max_edge:
            used_small.add(t)

    parent = array('i', [-1]) * n
    depth = array('i', [0]) * n
    parent_edge = array('i', [0]) * n
    order = array('i')
    order.append(0)
    parent[0] = 0

    idx = 0
    while idx < len(order):
        u = order[idx]
        idx += 1

        e = head[u]
        while e != -1:
            v = to[e]
            if v != parent[u]:
                parent[v] = u
                depth[v] = depth[u] + 1
                parent_edge[v] = ew[e]
                order.append(v)
            e = nxt[e]

    LOG = max(1, n.bit_length())
    up = [parent]

    for _ in range(1, LOG):
        prev = up[-1]
        cur = array('i', [0]) * n
        for v in range(n):
            cur[v] = prev[prev[v]]
        up.append(cur)

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

        for k in range(LOG - 1, -1, -1):
            ua = up[k][a]
            ub = up[k][b]
            if ua != ub:
                a = ua
                b = ub

        return parent[a]

    prefixes = {}

    for t in used_small:
        pref = array('i', [0]) * n
        for pos in range(1, n):
            v = order[pos]
            p = parent[v]
            pref[v] = pref[p] + (parent_edge[v] + t - 1) // t
        prefixes[t] = pref

    left = array('i', [0])
    right = array('i', [0])
    cnt = array('i', [0])
    roots = array('i', [0]) * n

    if max_edge > 0:
        for pos in range(1, n):
            v = order[pos]
            old_root = roots[parent[v]]
            value = parent_edge[v]

            new_root = len(cnt)
            left.append(left[old_root])
            right.append(right[old_root])
            cnt.append(cnt[old_root] + 1)

            cur_new = new_root
            cur_old = old_root
            lo = 1
            hi = max_edge

            while lo < hi:
                mid = (lo + hi) >> 1

                if value <= mid:
                    old_child = left[cur_old]
                    new_child = len(cnt)

                    left.append(left[old_child])
                    right.append(right[old_child])
                    cnt.append(cnt[old_child] + 1)

                    left[cur_new] = new_child
                    cur_new = new_child
                    cur_old = old_child
                    hi = mid
                else:
                    old_child = right[cur_old]
                    new_child = len(cnt)

                    left.append(left[old_child])
                    right.append(right[old_child])
                    cnt.append(cnt[old_child] + 1)

                    right[cur_new] = new_child
                    cur_new = new_child
                    cur_old = old_child
                    lo = mid + 1

            roots[v] = new_root

    sys.setrecursionlimit(1_000_000)

    def persistent_sum(ra, rb, rc, t):
        def dfs(a, b, c, lo, hi):
            total = cnt[a] + cnt[b] - 2 * cnt[c]
            if total == 0:
                return 0

            qlo = (lo + t - 1) // t
            qhi = (hi + t - 1) // t

            if qlo == qhi:
                return total * qlo

            mid = (lo + hi) >> 1
            ans = dfs(left[a], left[b], left[c], lo, mid)
            ans += dfs(right[a], right[b], right[c], mid + 1, hi)
            return ans

        return dfs(ra, rb, rc, 1, max_edge)

    out = []

    for s, f, t in queries:
        w = lca(s, f)
        edge_count = depth[s] + depth[f] - 2 * depth[w]

        if t >= max_edge:
            out.append(str(edge_count + 1))
            continue

        pref = prefixes.get(t)
        if pref is not None:
            value = pref[s] + pref[f] - 2 * pref[w] + 1
            out.append(str(value))
            continue

        value = persistent_sum(roots[s], roots[f], roots[w], t)
        out.append(str(value + 1))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc kề sử dụng các mảng số nguyên nhỏ gọn thay vì danh sách các bộ dữ liệu Python. Điều này quan trọng vì cây phân đoạn cố định và mảng tiền tố dung lượng nhỏ là những thiết bị tiêu thụ bộ nhớ chiếm ưu thế và giới hạn bộ nhớ chỉ là 256 MB. 

Việc duyệt cây ban đầu tạo ra`parent`,`depth`,`parent_edge`và thứ tự mà mọi cha mẹ đều xuất hiện trước con cái của nó. Thứ tự đó làm cho cả phiên bản cố định và tổng tiền tố theo dung lượng cụ thể trở nên dễ dàng xây dựng mà không cần đệ quy qua cây ban đầu. 

Bảng nâng nhị phân chỉ được sử dụng cho các truy vấn LCA. LCA là điểm trừ chính xác vì gốc tới`S`và root-to-`F`đường dẫn chứa phần root-to-LCA chung hai lần. 

Đối với công suất nhỏ,`ceil(D/T)`được tính như`(D + T - 1) // T`. Các giá trị tiền tố phù hợp với số nguyên 32 bit có dấu vì đường dẫn lớn nhất có thể chứa ít hơn`100000`các cạnh và mỗi đóng góp nhiều nhất là`20000`. 

Cây phân đoạn liên tục lưu trữ số lượng thay vì tổng độ dài cạnh. Số lượng là đủ vì truy vấn cần tổng theo trọng số tần số của`ceil(D/T)`. Tại một đoạn mà thương số không đổi, chỉ có số cạnh là quan trọng. 

Bản cập nhật liên tục được viết lặp đi lặp lại vì nó tạo ra khoảng`O(N log 20000)`nút. Việc tránh lệnh gọi hàm Python cho mọi cấp độ sẽ giúp quá trình tiền xử lý rẻ hơn đáng kể. 

Thủ tục truy vấn sử dụng mối quan hệ`count_path = count_S + count_F - 2 * count_LCA`. Một nút có số lượng đường dẫn bằng 0 sẽ bị loại bỏ ngay lập tức. Nếu thương số bằng nhau ở cả hai đầu của phạm vi giá trị của nó thì toàn bộ phạm vi có một đóng góp và phép đệ quy có thể dừng ở đó. 

điều kiện`t >= max_edge`được xử lý trước một trong hai phương pháp tiền xử lý. Khi đó, mọi con đường đều có thể đi qua mà không cần điểm dừng sạc trung gian, vì vậy câu trả lời chỉ đơn giản là số cạnh cộng với một điểm dừng kho. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Xem xét truy vấn đầu tiên, từ kho`3`đến kho`7`, với công suất`T = 2`. Đường đi của nó là`3 -> 2 -> 4 -> 7`với độ dài cạnh`2, 3, 6`. 

| Cạnh | Chiều dài |`ceil(D/T)`| Tổng cạnh chạy | 
| --- | --- | --- | --- | 
|`3 -> 2`| 2 | 1 | 1 | 
|`2 -> 4`| 3 | 2 | 3 | 
|`4 -> 7`| 6 | 3 | 6 | 
| Kho ban đầu | | |`6 + 1 = 7`| 

Đầu ra là`7`. 

Đối với truy vấn thứ hai, từ`2`ĐẾN`6`với`T = 1`, đường đi bao gồm các cạnh có độ dài`3`Và`5`. 

| Cạnh | Chiều dài |`ceil(D/T)`| Tổng cạnh chạy | 
| --- | --- | --- | --- | 
|`2 -> 4`| 3 | 3 | 3 | 
|`4 -> 6`| 5 | 5 | 8 | 
| Kho ban đầu | | |`8 + 1 = 9`| 

Đầu ra là`9`. 

Các truy vấn còn lại tạo ra phép tính tương tự. 

| Truy vấn | Độ dài cạnh đường dẫn |`T`| Tổng của`ceil(D/T)`| Trả lời | 
| --- | --- | --- | --- | --- | 
|`3 -> 7`|`2, 3, 6`| 2 | 6 | 7 | 
|`2 -> 6`|`3, 5`| 1 | 8 | 9 | 
|`5 -> 7`|`4, 6`| 3 | 4 | 5 | 
|`1 -> 4`|`1, 3`| 1 | 4 | 5 | 
|`3 -> 7`|`2, 3, 6`| 1 | 11 | 12 | 

Mẫu này thực hiện cả khả năng phân chia chính xác và những con đường yêu cầu nhiều điểm dừng sạc. 

### Mẫu 2 

Đường đi là một chuỗi có độ dài cạnh`5, 10, 20`. 

Vì`T = 20`, mỗi cạnh cần một đoạn hành trình. 

| Cạnh | Chiều dài |`ceil(D/T)`| Tổng cạnh chạy | 
| --- | --- | --- | --- | 
|`1 -> 2`| 5 | 1 | 1 | 
|`2 -> 3`| 10 | 1 | 2 | 
|`3 -> 4`| 20 | 1 | 3 | 
| Kho ban đầu | | |`3 + 1 = 4`| 

Vì`T = 10`, cạnh cuối cùng cần hai đoạn. 

| Cạnh | Chiều dài |`ceil(D/T)`| Tổng cạnh chạy | 
| --- | --- | --- | --- | 
|`1 -> 2`| 5 | 1 | 1 | 
|`2 -> 3`| 10 | 1 | 2 | 
|`3 -> 4`| 20 | 2 | 4 | 
| Kho ban đầu | | |`4 + 1 = 5`| 

Vì`T = 5`, ba đóng góp là`1, 2, 4`, cho`8`. 

|`T`| Tổng đóng góp cạnh | Câu trả lời cuối cùng | 
| --- | --- | --- | 
| 20 | 3 | 4 | 
| 10 | 4 | 5 | 
| 5 | 7 | 8 | 

Mẫu này thể hiện ranh giới chính xác tại`D = T`Và`D = 2T`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(NB + NlogD + NlogN + Q(D/B)logD)`| Công suất sử dụng nhỏ`O(N)`xử lý trước từng cái, trong khi dung lượng lớn chỉ truy cập vào phạm vi thay đổi thương số | 
| Không gian |`O(NB + NlogD + NlogN)`| Tiền tố dung lượng nhỏ, phiên bản cây phân đoạn cố định và bảng nâng nhị phân chiếm ưu thế trong bộ nhớ | 

Với`B = 200`Và`D <= 20000`, bộ tiền xử lý dung lượng nhỏ lưu trữ tối đa khoảng 20 triệu số nguyên 32 bit. Cây phân đoạn liên tục cần khoảng`N log D`các nút và bảng LCA cần`N log N`số nguyên. Tất cả các cấu trúc này được lưu trữ dưới dạng mảng số nguyên nhỏ gọn thay vì các đối tượng Python. 

Chìa khóa ràng buộc cho công suất lớn là`ceil(D/T)`chỉ có thể lấy`O(D/T)`những giá trị riêng biệt. Từ`T > B`, điều này nhiều nhất là về`D/B`, nhiều nhất là`100`đối với ngưỡng đã chọn. Cây liên tục xử lý các phạm vi này mà không cần quét đường dẫn ban đầu. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định`solve()`chức năng từ giải pháp trên có sẵn.```python
import sys
import io

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input
    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline
        from contextlib import redirect_stdout
        out = io.StringIO()
        with redirect_stdout(out):
            solve()
        return out.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        input = old_input

sample1 = """\
7 5
1 2 1
2 3 2
2 4 3
4 5 4
4 6 5
4 7 6
3 7 2
2 6 1
5 7 3
1 4 1
3 7 1
"""

sample2 = """\
4 3
1 2 5
2 3 10
3 4 20
1 4 20
1 4 10
1 4 5
"""

assert run(sample1) == "7\n9\n5\n5\n12", "sample 1"
assert run(sample2) == "4\n5\n8", "sample 2"

minimum_case = """\
2 1
1 2 1
1 2 1
"""
assert run(minimum_case) == "2", "minimum-size case"

exact_boundary = """\
3 4
1 2 10
2 3 20
1 3 10
1 3 20
1 3 5
1 3 10
"""
assert run(exact_boundary) == "4\n3\n6\n4", "exact divisibility boundaries"

equal_edges = """\
5 4
1 2 10
2 3 10
3 4 10
4 5 10
1 5 10
1 5 5
1 5 20
2 4 10
"""
assert run(equal_edges) == "5\n9\n5\n3", "all equal edge lengths"

large_chain = "100000 100000\n"
large_chain += "".join(f"{i} {i + 1} 1\n" for i in range(1, 100000))
large_chain += "".join(f"1 100000 {t}\n" for t in range(1, 100001))

large_output = run(large_chain).splitlines()
assert len(large_output) == 100000, "maximum-size case"
assert large_output[0] == "100000", "maximum-size T=1"
assert large_output[-1] == "100000", "maximum-size large T"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`, một cạnh có chiều dài`1`,`T = 1`|`2`| Cây tối thiểu và không dừng sạc | 
| Đường đi ba đỉnh có độ dài`10, 20`|`4`,`3`,`6`,`4`| bội số chính xác của dung lượng pin | 
| Bốn cạnh có chiều dài bằng nhau`10`|`5`,`9`,`5`,`3`| Các giá trị cạnh bằng nhau lặp đi lặp lại và một số dung lượng | 
| Chuỗi với`100000`đỉnh và`100000`truy vấn |`100000`cho mọi truy vấn | Cây có kích thước tối đa, số lượng truy vấn lớn và dung lượng cực đại | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là cạnh có chiều dài đúng bằng dung lượng pin. Coi như```
2 1
1 2 10
1 2 10
```Đường đi có một cạnh và`ceil(10/10) = 1`, vậy câu trả lời là`1 + 1 = 2`. Thuật toán đi vào`t >= max_edge`case và trả về một cạnh cộng một của đường dẫn. Không có điểm dừng sạc trung gian. 

Trường hợp cạnh thứ hai là cạnh dài hơn dung lượng một chút. Coi như```
2 1
1 2 11
1 2 10
```Các cạnh cần`ceil(11/10) = 2`các phân khúc du lịch. Cần có một điểm dừng sạc ở giữa, vì vậy câu trả lời là`3`. Tính toán liên tục hoặc tiền tố ghi lại sự đóng góp của cạnh dưới dạng`2`, và trận chung kết`+1`cho`3`. 

Trường hợp cạnh thứ ba là tuyến đường chứa một số kho bắt buộc nhưng không tính phí. Coi như```
3 1
1 2 5
2 3 5
1 3 10
```Cả hai cạnh đều có`ceil(5/10) = 1`. Đóng góp của họ là`2`, và ban đầu`+1`cho`3`. Kho ở giữa được tính vì nó là điểm cuối của cạnh thứ nhất và là điểm bắt đầu của cạnh thứ hai. 

Trường hợp cạnh thứ tư là một con đường dài hơn pin rất nhiều. Coi như```
2 1
1 2 20
1 2 5
```Cạnh đơn góp phần`ceil(20/5) = 4`, vậy câu trả lời là`5`. Thuật toán không bao giờ tuyên bố đường không thể truy cập được. Nó chỉ tính ba điểm dừng sạc trung gian ngụ ý trong bốn đoạn hành trình. 

Trường hợp cạnh thứ năm là truy vấn có dung lượng vượt quá mọi con đường trong cây. Nếu con đường lớn nhất có chiều dài`20`Và`T = 21`, mỗi con đường đóng góp đúng một. Cây bền vững là không cần thiết và kết quả trực tiếp`number_of_edges_on_path + 1`là chính xác. Phím tắt này cũng tránh lãng phí thời gian cho truy vấn có câu trả lời độc lập với độ dài cạnh thực tế.
