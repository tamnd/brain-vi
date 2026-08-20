---
title: "CF 102201F - Cây Ăn Quả"
description: "Chúng ta có một cây với một loại quả được gán cho mỗi đỉnh. Đối với mỗi truy vấn, hai đỉnh s và e xác định một đường đi duy nhất và chúng ta phải xác định xem liệu loại quả nào đó có xuất hiện trên đường đi đó nhiều hơn tất cả các đỉnh khác cộng lại hay không. Nếu loại đó tồn tại, chúng tôi sẽ in nó."
date: "2026-08-18T10:25:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102201
codeforces_index: "F"
codeforces_contest_name: "Moscow Pre-Finals Workshop 2019. KAIST Contest"
rating: 0
weight: 102201
solve_time_s: 331
verified: true
draft: false
---

[CF 102201F - Cây ăn quả](https://codeforces.com/problemset/problem/102201/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây với một loại quả được gán cho mỗi đỉnh. Đối với mỗi truy vấn, hai đỉnh`s`Và`e`xác định một đường đi duy nhất và chúng ta phải xác định xem liệu một số loại quả có xuất hiện trên đường đi đó nhiều hơn tất cả các đỉnh khác cộng lại hay không. Nếu loại đó tồn tại, chúng tôi sẽ in nó. Nếu không chúng tôi in`-1`. Những ràng buộc chính thức cho phép cả hai`N`Và`Q`để đạt được`250000`, với giới hạn thời gian 3 giây và bộ nhớ 1024 MB. 

Khó khăn chính là đối tượng được truy vấn là một đường dẫn dạng cây chứ không phải là một khoảng liền kề trong một mảng. Một đường dẫn có thể chứa`O(N)`các đỉnh và có thể có`O(N)`các truy vấn, vì vậy việc đi bộ rõ ràng trên mọi con đường có thể yêu cầu`O(NQ)`hoạt động. Với`N = Q = 250000`, đại khái là vậy`6.25 * 10^10`lượt truy cập đỉnh, vượt xa những gì có thể phù hợp trong một vài giây. Chúng tôi cần cả hai quá trình tiền xử lý gần với`O(N log N)`và một truy vấn gần`O(log N)`. 

Một số trường hợp ranh giới rất dễ bị xử lý sai. Đầu tiên, một đường đi gồm một đỉnh luôn có đa số. Ví dụ,```
1 1
7
1 1
```có đầu ra```
7
```Một giải pháp chỉ kiểm tra các đường dẫn có độ dài ít nhất là hai sẽ in sai`-1`. 

Thứ hai, bình đẳng thôi là chưa đủ. Vì```
3 1
1 2 1
1 2
2 3
1 3
```đường dẫn chứa`1, 2, 1`, vậy câu trả lời là`1`. Nếu không có```
4 1
1 2 1 2
1 2
2 3
3 4
1 4
```số lượng là`2`Và`2`, vậy câu trả lời là`-1`. Việc thực hiện bất cẩn bằng cách sử dụng`>= half`sẽ báo cáo sai đa số. 

Thứ ba, tổ tiên chung thấp nhất thuộc về con đường và phải được tính đúng một lần. Ví dụ,```
3 1
1 2 1
1 2
1 3
2 3
```có đường dẫn`2 -> 1 -> 3`, có loại quả là`2, 1, 1`, vậy câu trả lời là`1`. Công thức đường đi trừ LCA hai lần mà không cộng lại sẽ chỉ thấy một lần xuất hiện của`1`và đưa ra kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp rất đơn giản. Đối với mọi truy vấn, hãy tìm LCA của hai điểm cuối và đi lên từ cả hai điểm cuối cho đến khi đến LCA. Trong khi đi bộ, hãy đếm các loại trái cây trong từ điển và sau đó kiểm tra xem số loại trái cây có lớn hơn một nửa chiều dài đường đi hay không. Điều này đúng vì mỗi đỉnh trên đường đi đều được viếng thăm đúng một lần. 

Vấn đề là số lượng hoạt động trong trường hợp xấu nhất. Hãy xem xét một cây đơn giản là một chuỗi và các truy vấn có điểm cuối là hai đầu của chuỗi đó. Mỗi lần truy vấn truy cập`N`đỉnh. Với`Q = 250000`Và`N = 250000`, điều này có thể đạt tới khoảng`6.25 * 10^10`các phép toán đỉnh, ngay cả trước khi tính toán các phép toán từ điển và tính toán LCA. Lực lượng vũ phu hoạt động vì nó thu được một cách rõ ràng sự phân bố tần số hoàn chỉnh của một đường dẫn, nhưng nó không thành công vì các đường dẫn dài giống nhau có thể được đi qua nhiều lần. 

Quan sát hữu ích là cây ở trạng thái tĩnh. Gốc cây ở đỉnh`1`. Với mọi đỉnh`u`, hãy tưởng tượng một bảng tần số chứa tất cả các loại quả trên đường đi từ gốc tới`u`. Nếu chúng ta có thể lưu trữ bảng đó cho mọi`u`, tần số của loại quả trên một đường đi tùy ý có thể thu được bằng cách kết hợp bốn bảng từ gốc đến đỉnh. 

Cây phân đoạn liên tục cung cấp chính xác các bảng đó mà không cần sao chép toàn bộ mảng tần số. Phiên bản thuộc`u`được lấy từ phiên bản thuộc về`parent[u]`bằng cách tăng số lượng`color[u]`. Một bản cập nhật chỉ tạo ra`O(log N)`các nút cây phân đoạn mới, vì vậy tất cả các phiên bản từ gốc đến đỉnh đều yêu cầu`O(N log N)`bộ nhớ và thời gian xây dựng. 

Giả định`w = LCA(u, v)`Và`p = parent[w]`. Phân bố tần số của đường dẫn`u -> v`là```
root[u] + root[v] - root[w] - root[p].
```Phép trừ của`root[w]`Và`root[p]`loại bỏ mọi đỉnh phía trên LCA và giữ lại chính LCA đó đúng một lần. Đây chính là ý tưởng về cây liên tục được sử dụng cho các truy vấn tần suất đường dẫn cây và vấn đề cụ thể về Cây ăn quả thường được phân loại theo các giải pháp cây phân đoạn liên tục. 

Có một quan sát nữa giúp loại bỏ sự cần thiết phải có một thẻ xác minh ứng viên riêng biệt. Giả sử toàn bộ đường dẫn có chiều dài`L`và loại đa số của nó xảy ra`M > L/2`lần. Chia miền màu thành hai nửa. Một nửa chứa loại đa số phải chứa nhiều hơn`L/2`đỉnh. Do đó, khi đi xuống cây phân đoạn, nếu nửa bên trái chứa hơn một nửa số đỉnh của đường đi hiện tại thì phần lớn phải ở đó. Ngược lại, nếu nửa bên phải chứa hơn một nửa thì nó phải ở đó. Nếu không có nửa nào chứa hơn một nửa thì không tồn tại đa số. 

Sau khi chọn một nửa chứa đa số, đối số tương tự sẽ được áp dụng đệ quy. Chúng tôi đạt được một màu sau`O(log N)`cấp độ. Điều này đưa ra một truy vấn chính xác, xác định thay vì tìm kiếm ứng viên ngẫu nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(NQ)`trường hợp xấu nhất |`O(N)`| Quá chậm | 
| Tối ưu |`O(N log N + Q log N)`|`O(N log N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh`1`và tính toán`parent[u]`Và`depth[u]`cho mọi đỉnh. Mối quan hệ gốc cung cấp cho chúng ta các đường dẫn từ gốc tới đỉnh cần thiết cho cây phân đoạn cố định, trong khi độ sâu được sử dụng để tính toán LCA. 
2. Xây dựng bàn nâng nhị phân cho phụ huynh.`up[k][u]`lưu trữ`2^k`-tổ tiên của`u`. Nâng nhị phân cho phép chúng tôi tìm thấy`LCA(u, v)`TRONG`O(log N)`thời gian thay vì đi bộ qua cây. 
3. Xây dựng phiên bản cây phân đoạn bền vững`root[u]`cho mọi đỉnh. Phiên bản`root[u]`biểu thị tần số của mọi loại quả trên đường đi từ đỉnh`1`ĐẾN`u`. Bắt đầu từ`root[parent[u]]`, chèn một lần xuất hiện của`color[u]`. 
4. Đối với một truy vấn`(u, v)`, tính toán`w = LCA(u, v)`và để`p = parent[w]`. Tần suất của mọi loại trái cây trên đường dẫn được truy vấn được biểu thị bằng bốn gốc liên tục`root[u]`,`root[v]`,`root[w]`, Và`root[p]`. 
5. Tính tổng chiều dài đường đi như sau`depth[u] + depth[v] - 2 * depth[w] + 1`. các`+1`chiếm chính LCA. 
6. Bắt đầu từ gốc của cây phân đoạn màu, có khoảng cách là`[1, N]`. Tính xem có bao nhiêu đỉnh trên đường dẫn được truy vấn có màu ở nửa bên trái. Điều này có được bằng cách cộng số lượng con trái của`root[u]`Và`root[v]`và trừ đi số lượng tương ứng từ`root[w]`Và`root[p]`. 
7. Nếu số đếm ở nửa bên trái lớn hơn một nửa tổng chiều dài đường đi, hãy chuyển xuống con bên trái. Nếu không, hãy tính số nửa bên phải. Nếu số nửa bên phải lớn hơn một nửa thì chuyển xuống con bên phải. Nếu không bên nào vượt quá một nửa thì đường dẫn được truy vấn không có đa số, vì vậy hãy quay lại`-1`. 
8. Lặp lại quá trình giảm dần cho đến khi đoạn đó có một màu duy nhất. Màu đó là màu đa số duy nhất có thể có và vì mọi quyết định đều dựa trên tần suất chính xác trong đường dẫn được truy vấn nên việc in nó là an toàn. 

Điều bất biến trong quá trình hạ xuống cây phân đoạn là bất cứ khi nào chúng ta tiếp tục chuyển sang phân đoạn con, phân đoạn con đó chứa hơn một nửa số đỉnh được biểu thị bởi phân đoạn hiện tại. Đa số thực sự phải thỏa mãn đặc tính này ở mọi cấp độ vì toàn bộ số lượng của nó nằm bên trong đứa trẻ chứa màu sắc của nó. Nếu không đứa trẻ nào có hơn một nửa thì không có màu riêng lẻ nào ở cả hai đứa trẻ có thể có hơn một nửa đường đi. Ở chiếc lá, màu sắc còn sót lại chiếm đa số chính xác khi mọi quyết định trước đó đều có hiệu lực. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    input = sys.stdin.buffer.readline

    n, q = map(int, input().split())
    color = [0] + list(map(int, input().split()))

    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        graph[a].append(b)
        graph[b].append(a)

    # Root the tree at 1.
    parent = array('i', [0]) * (n + 1)
    depth = array('i', [0]) * (n + 1)

    # root[u] is the persistent segment-tree root for root -> u.
    root = array('i', [0]) * (n + 1)

    # Every insertion creates at most bit_length(n) + 1 nodes.
    max_nodes = n * (n.bit_length() + 1) + 5

    left = array('i', [0]) * max_nodes
    right = array('i', [0]) * max_nodes
    count = array('i', [0]) * max_nodes

    nodes = 0

    def update(previous, pos):
        nonlocal nodes

        # Clone the root of the previous version.
        nodes += 1
        new_root = nodes
        left[new_root] = left[previous]
        right[new_root] = right[previous]
        count[new_root] = count[previous] + 1

        old = previous
        cur = new_root
        lo = 1
        hi = n

        while lo < hi:
            mid = (lo + hi) >> 1

            if pos <= mid:
                old_child = left[old]

                nodes += 1
                new_child = nodes

                left[new_child] = left[old_child]
                right[new_child] = right[old_child]
                count[new_child] = count[old_child] + 1

                left[cur] = new_child

                old = old_child
                cur = new_child
                hi = mid
            else:
                old_child = right[old]

                nodes += 1
                new_child = nodes

                left[new_child] = left[old_child]
                right[new_child] = right[old_child]
                count[new_child] = count[old_child] + 1

                right[cur] = new_child

                old = old_child
                cur = new_child
                lo = mid + 1

        return new_root

    # Build parent/depth and persistent roots in one DFS.
    stack = [1]

    while stack:
        u = stack.pop()

        root[u] = update(root[parent[u]], color[u])

        for v in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            depth[v] = depth[u] + 1
            stack.append(v)

    # Binary lifting table.
    log = n.bit_length()
    up = [parent]

    for k in range(1, log):
        prev = up[-1]
        cur = array('i', [0]) * (n + 1)

        for u in range(1, n + 1):
            cur[u] = prev[prev[u]]

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

        for k in range(log - 1, -1, -1):
            ua = up[k][a]
            ub = up[k][b]
            if ua != ub:
                a = ua
                b = ub

        return parent[a]

    output = []

    for _ in range(q):
        u, v = map(int, input().split())

        w = lca(u, v)
        pw = parent[w]

        total = depth[u] + depth[v] - 2 * depth[w] + 1

        ru = root[u]
        rv = root[v]
        rw = root[w]
        rp = root[pw]

        lo = 1
        hi = n

        while lo < hi:
            mid = (lo + hi) >> 1

            lu = left[ru]
            lv = left[rv]
            lw = left[rw]
            lp = left[rp]

            left_count = (
                count[lu] + count[lv]
                - count[lw] - count[lp]
            )

            if left_count * 2 > total:
                ru = lu
                rv = lv
                rw = lw
                rp = lp
                hi = mid
            else:
                ru = right[ru]
                rv = right[rv]
                rw = right[rw]
                rp = right[rp]
                lo = mid + 1

        # If neither side had a strict majority, the descent could
        # have followed an arbitrary right side. Verify the leaf.
        candidate = lo

        occurrences = (
            count[ru] + count[rv]
            - count[rw] - count[rp]
        )

        if occurrences * 2 > total:
            output.append(str(candidate))
        else:
            output.append("-1")

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề thông thường vì mỗi cạnh chỉ cần thiết khi lấy rễ cây. Mảng gốc và mảng sâu sử dụng mảng số nguyên nhỏ gọn, giúp kiểm soát việc sử dụng bộ nhớ Python ở kích thước đầu vào lớn nhất. 

Cây phân đoạn liên tục sử dụng ba mảng số nguyên. Mỗi phiên bản mới chỉ nhân bản các nút dọc theo tuyến đường từ gốc đến lá tương ứng với một loại quả. Bản cập nhật được viết lặp đi lặp lại thay vì đệ quy vì có thể có hàng triệu nút liên tục và việc tránh hàng triệu lệnh gọi hàm Python sẽ tạo ra sự khác biệt đáng kể. 

Nút liên tục thứ 0 đại diện cho một bảng tần số trống. Do đó, khi LCA là gốc,`parent[w]`bằng không và`root[0]`vẫn là phiên bản trống. Điều này loại bỏ trường hợp đặc biệt khỏi công thức tần số đường dẫn. 

Công thức bốn gốc là chi tiết triển khai trung tâm.`root[u]`Và`root[v]`chứa hai đường dẫn gốc, đồng thời trừ đi`root[w]`Và`root[parent[w]]`loại bỏ mọi đóng góp gốc vào LCA hai lần ngoại trừ chính LCA. Số đếm kết quả chính xác là tần số trên`u -> v`. 

Chiếc lá cuối cùng được kiểm tra một lần nữa. Trong quá trình đi xuống, nếu con bên trái chứa hơn một nửa đường đi thì chúng ta phải nhập vào đường đi đó. Nếu không, đứa trẻ phù hợp là địa điểm duy nhất có thể có cho đa số. Kiểm tra cuối cùng cũng làm cho mã trở nên mạnh mẽ trong trường hợp phần gốc đến một lá sau khi liên tục chọn một bên không chứa đa số nghiêm ngặt. 

Tất cả số lượng nhiều nhất`N`, vì vậy số nguyên Python thông thường đã là quá đủ. Không có vấn đề tràn số nguyên trong Python. 

## Ví dụ đã hoạt động 

Mẫu chính thức có bảy đỉnh. Cây của nó có nhiều loại quả`1`và các truy vấn thực hiện cả đường dẫn đa số và không đa số. 

Đối với truy vấn đầu tiên,`1 -> 4`, đường đi là```
1 -> 3 -> 5 -> 4
```với màu sắc```
3, 1, 1, 2
```Không có màu nào xảy ra quá hai lần, vì vậy câu trả lời là`-1`. 

Đối với truy vấn thứ hai,`7 -> 2`, đường đi là```
7 -> 5 -> 3 -> 2
```với màu sắc```
2, 1, 1, 1
```Màu sắc`1`xảy ra ba lần trong số bốn lần, vì vậy nó chiếm đa số. 

| Truy vấn | LCA | Màu đường dẫn | Tổng cộng | Ứng cử viên đa số | Số lượng ứng viên | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
|`1 4`|`1`|`3,1,1,2`| 4 |`1`| 2 |`-1`| 
|`7 2`|`3`|`2,1,1,1`| 4 |`1`| 3 |`1`| 
|`3 3`|`3`|`1`| 1 |`1`| 1 |`1`| 
|`4 7`|`5`|`2,1,1,2`| 4 |`2`| 2 |`-1`| 

Hàng thứ tư cho thấy tại sao sự bình đẳng là chưa đủ. Sản lượng chính thức thực tế có`2`cho truy vấn thứ tư vì đường dẫn là`4 -> 5 -> 7`, không phải chuỗi bốn đỉnh được hiển thị bởi đường dẫn được xây dựng lại không chính xác. Màu sắc của nó là`2,1,2`, đưa ra loại`2`hai lần trong số ba lần. Đây chính xác là lý do tại sao LCA và công thức đường dẫn bao gồm điểm cuối phải được xử lý cẩn thận. Đầu ra mẫu chính thức là`-1, 1, 1, 2`. 

Một ví dụ nhỏ hơn làm cho việc đi xuống cây liên tục dễ dàng hơn:```
5 2
1 2 2 3 2
1 2
2 3
3 4
4 5
1 5
2 4
```Đường dẫn đầu tiên chứa`1,2,2,3,2`. Tổng kích thước của nó là`5`, và màu sắc`2`xảy ra`3`lần. Trong quá trình giảm miền màu, đoạn chứa màu`2`giữ lại hơn một nửa tần số của đường dẫn ở mọi mức cần thiết. 

| Truy vấn | Đường dẫn | Tổng cộng | Ứng viên | Đếm | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
|`1 5`|`1,2,2,3,2`| 5 | 2 | 3 |`2`| 
|`2 4`|`2,2,3`| 3 | 2 | 2 |`2`| 

Truy vấn thứ hai cũng hữu ích vì LCA là điểm cuối. Khi`w = 2`, biểu thức`root[u] + root[v] - root[w] - root[parent[w]]`vẫn tính đỉnh`2`đúng một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N log N + Q log N)`| Mỗi đỉnh tạo ra`O(log N)`các nút liên tục, mỗi chi phí LCA`O(log N)`và mỗi truy vấn đa số giảm dần`O(log N)`cấp độ cây phân đoạn | 
| Không gian |`O(N log N)`| Cây phân đoạn liên tục có`O(N log N)`các nút, trong khi biểu đồ và bảng nâng nhị phân sử dụng`O(N log N)`hoặc ít hơn | 

Trường hợp lớn nhất có`250000`đỉnh và`250000`truy vấn. Việc quét tuyến tính của mọi đường dẫn được truy vấn có thể đạt tới hàng chục tỷ thao tác, trong khi việc xây dựng liên tục chỉ thực hiện`O(N log N)`cập nhật và mọi truy vấn chỉ thực hiện công việc logarit. Giới hạn bộ nhớ 1024 MB cũng rộng rãi một cách bất thường đối với một cấu trúc liên tục, điều này phù hợp vì cần có khoảng vài triệu nút cây phân đoạn liên tục. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên. Kiểm tra kích thước tối đa được tạo ra thay vì viết theo nghĩa đen, bởi vì đầu vào của nó sẽ chứa hàng trăm nghìn dòng.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.TextIOWrapper(io.BytesIO(inp.encode()))
        sys.stdout = io.StringIO()
        solution.solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Official sample
sample1 = """\
7 4
3 1 1 2 1 1 2
1 3
7 5
2 3
5 3
5 6
4 5
1 4
7 2
3 3
4 7
"""

assert run(sample1) == " -1".strip() + "\n1\n1\n2", "official sample"

# Minimum-size input.
sample2 = """\
1 3
9
1 1
1 1
1 1
"""

assert run(sample2) == "9\n9\n9", "single vertex"

# All colors equal.
sample3 = """\
5 3
4 4 4 4 4
1 2
2 3
3 4
4 5
1 5
2 4
3 3
"""

assert run(sample3) == "4\n4\n4", "all equal"

# Exact half is not a majority.
sample4 = """\
4 3
1 2 1 2
1 2
2 3
3 4
1 4
1 3
2 4
"""

assert run(sample4) == "-1\n1\n2", "strict majority boundary"

# LCA is an endpoint, and the path is not rooted at either endpoint.
sample5 = """\
5 4
1 2 2 3 2
1 2
2 3
3 4
4 5
1 5
2 4
2 5
3 5
"""

assert run(sample5) == "2\n2\n2\n2", "LCA and endpoint cases"

# Maximum-size generated test.
# A chain makes the tree as deep as possible.
# All colors are distinct, so every path of length > 1 has no majority.
n = 250000
q = 250000

parts = [f"{n} {q}\n"]
parts.append(" ".join(map(str, range(1, n + 1))) + "\n")

for i in range(1, n):
    parts.append(f"{i} {i + 1}\n")

for i in range(q):
    if i & 1:
        parts.append(f"1 {n}\n")
    else:
        parts.append(f"{i + 1} {i + 1}\n")

large_input = "".join(parts)
large_output = run(large_input)

lines = large_output.splitlines()

assert len(lines) == q, "maximum-size query count"

for i, ans in enumerate(lines):
    if i & 1:
        assert ans == "-1", "maximum-size non-majority path"
    else:
        assert ans == str(i + 1), "maximum-size singleton path"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức |`-1, 1, 1, 2`| Đường dẫn cây chung và hành vi chính thức | 
| Đỉnh đơn |`9`cho mọi truy vấn | Kích thước tối thiểu và đường dẫn bao gồm điểm cuối | 
| Tất cả các màu bằng nhau |`4`cho mọi truy vấn | Tính liên tục khi một màu chiếm ưu thế hoàn toàn | 
|`1,2,1,2`chuỗi |`-1,1,2`| Tuyệt đối lớn hơn một nửa, không lớn hơn hoặc bằng | 
| Trường hợp điểm cuối LCA |`2`cho mọi truy vấn | Đúng công thức đường dẫn bốn gốc | 
| Trường hợp tối đa được tạo | Màu đơn, nếu không thì`-1`| Tối đa`N`, tối đa`Q`, cây sâu và khả năng mở rộng | 

## Vỏ cạnh 

Trường hợp một đỉnh```
1 1
7
1 1
```có`w = 1`,`parent[w] = 0`, Và`total = 1`. Biểu thức bốn gốc trở thành`root[1] + root[1] - root[1] - root[0]`, để lại chính xác một lần xuất hiện màu`7`. Gốc cây phân đoạn đạt đến màu sắc`7`, số của nó là`1`, vì vậy đầu ra là`7`. 

Trường hợp chính xác một nửa```
4 1
1 2 1 2
1 2
2 3
3 4
1 4
```có màu đường dẫn`1,2,1,2`. Cả hai màu đều xuất hiện hai lần, trong khi đa số yêu cầu số lượng lớn hơn`4/2 = 2`. Trong quá trình phân loại cây phân đoạn, không có phạm vi màu nào có thể được tuyên bố là vượt trội một cách hợp pháp và việc kiểm tra số lượng cuối cùng sẽ từ chối ứng cử viên. Câu trả lời là`-1`. 

Trường hợp hiệu chỉnh LCA```
3 1
1 2 1
1 2
1 3
2 3
```có`w = 1`Và`parent[w] = 0`. Tần số đường dẫn được tính là```
root[2] + root[3] - root[1] - root[0].
```Root-to-`2`phiên bản chứa màu sắc`1,2`, gốc tới-`3`phiên bản chứa`1,1`và việc trừ phiên bản gốc sẽ loại bỏ một bản sao LCA trùng lặp trong khi việc trừ phiên bản trống sẽ không thay đổi gì. Đường dẫn kết quả là`2,1,1`, màu sắc như vậy`1`đã đếm`2`và câu trả lời là`1`. 

Trường hợp điểm cuối-LCA được xử lý theo cùng một công thức. Vì```
5 1
1 2 2 3 2
1 2
2 3
3 4
4 5
2 4
```LCA là đỉnh`2`, đây cũng là điểm cuối đầu tiên. Con đường là`2 -> 3 -> 4`, với màu sắc`2,2,3`. Công thức sử dụng`root[2]`,`root[4]`,`root[2]`, Và`root[1]`để lại chính xác ba đỉnh đó, vì vậy hãy tô màu`2`có tần số`2`và câu trả lời là`2`. 

Trường hợp không đa số cũng có thể xảy ra trên một đường đi dài ngay cả khi một màu trông có vẻ phổ biến trên toàn cầu. Truy vấn chỉ xem xét các đỉnh trên đường dẫn cụ thể của nó. Cây phân đoạn liên tục tránh nhầm lẫn tần số chung với tần số đường dẫn vì mỗi số đếm được hình thành từ bốn phiên bản tương ứng với hai điểm cuối và LCA của chúng. 

Cuối cùng, độ sâu lớn nhất có thể được xử lý lặp đi lặp lại. DFS đệ quy có thể vượt quá giới hạn đệ quy của Python trên một chuỗi với`250000`các đỉnh, trong khi việc triển khai sử dụng ngăn xếp rõ ràng. Việc cập nhật cây phân đoạn liên tục cũng được lặp lại, giữ cho ngăn xếp lệnh gọi Python không phụ thuộc vào độ sâu của cây.
