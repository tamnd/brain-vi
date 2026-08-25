---
title: "CF 102191I - Trình bày dự án"
description: "Hệ thống phân cấp của công ty là một cây có gốc. Nhân viên u báo cáo trực tiếp cho p[u] và liên tục làm theo chỉ dẫn của phụ huynh cuối cùng mới đến được với Giám đốc điều hành. Mỗi nhân viên thuộc về duy nhất một dự án."
date: "2026-08-24T10:57:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "I"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 2145
verified: true
draft: false
---

[CF 102191I - Trình bày dự án](https://codeforces.com/problemset/problem/102191/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35 phút 45 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống phân cấp của công ty là một cây có gốc. Người lao động`u`báo cáo trực tiếp tới`p[u]`và liên tục làm theo các gợi ý của phụ huynh cuối cùng cũng đến được với Giám đốc điều hành. Mỗi nhân viên thuộc về duy nhất một dự án. 

Đối với một dự án cụ thể, phần trình bày của nó có sự tham dự của mọi nhân viên được giao cho dự án đó, cùng với mọi người quản lý trên con đường từ mỗi nhân viên đó trở thành Giám đốc điều hành. Nếu hai thành viên dự án có chung một người quản lý thì người quản lý đó chỉ được tính một lần. Do đó, câu trả lời bắt buộc cho một dự án là số đỉnh riêng biệt có trong sự kết hợp của tất cả các đường dẫn từ gốc đến thành viên dự án. 

Dữ liệu đầu vào chứa tối đa (10^6) nhân viên, do đó, một thuật toán chuyển đến CEO riêng cho từng thành viên dự án có thể thực hiện (O(n^2)) công việc. Với một triệu đỉnh, ngay cả một đường truyền tuyến tính cũng đã có giá trị đáng kể trong giới hạn ba giây, do đó mục tiêu phải gần với tuyến tính. Việc lưu trữ bảng tổ tiên (O(\log n)) cho mọi đỉnh cũng là điều không mong muốn trong Python vì số nguyên (10^6 \log n) vượt quá giới hạn bộ nhớ. Giải pháp nên sử dụng mảng có kích thước tuyến tính và tránh DFS đệ quy. 

Một số trường hợp rất dễ xử lý sai. Nếu hai nhân viên của cùng một dự án nằm trên cùng một đường gốc thì tổ tiên chung của họ không được tính hai lần. Ví dụ,```
3 1
1 1 1
0 1 2
```Cả ba nhân viên đều thuộc dự án 1 và câu trả lời là`3`. Giải pháp cộng ba độ dài đường dẫn gốc một cách độc lập sẽ tính nhân viên 1 và 2 nhiều lần. 

Bản thân một nhân viên có thể là tổ tiên của một nhân viên khác trong cùng một dự án. Ví dụ,```
4 2
1 2 1 2
0 1 2 3
```Dự án 1 bao gồm các nhân viên 1 và 3. Đường đi của họ là`1`Và`1 -> 2 -> 3`, vậy đáp án cho dự án 1 là`3`, không`4`. Đầu ra là`3 4`. 

CEO cũng có thể là thành viên duy nhất của một dự án. Vì```
1 1
1
0
```câu trả lời là`1`. Bất kỳ công thức nào bắt đầu bằng số cạnh và quên gốc sẽ cho kết quả bằng 0. 

Cuối cùng, một dự án có thể có các thành viên ở các nhánh hoàn toàn khác nhau. TRONG```
5 2
1 1 2 1 2
0 1 1 2 2
```dự án 1 bao gồm các nhân viên 1, 2 và 4. Những người tham dự dự án là`{1, 2, 4}`, vậy câu trả lời là`3`. Dự án 2 chứa các nhân viên 3 và 5, có đường đi cho`{1, 2, 3, 5}`, vậy đáp án của nó là`4`. Đầu ra đúng là`3 4`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp theo dõi mọi thành viên dự án từ nhân viên đó cho đến tất cả những người quản lý của dự án và chèn từng nhân viên đã ghé thăm vào một tập hợp cho dự án. Điều này đúng vì chính xác những tổ tiên đó phải tham dự. Vấn đề là số lần đi bộ lặp đi lặp lại. Hãy xem xét một chuỗi gồm (n) nhân viên trong đó mọi nhân viên thuộc cùng một dự án. Nhân viên đầu tiên có thể yêu cầu một bước cha mẹ, hai bước tiếp theo, v.v., đưa ra 

[ 
1 + 2 + \cdots + (n-1) = \frac{n(n-1)}2 = O(n^2) 
] 

sự duyệt qua cha mẹ. Đối với (n=10^6), đây là khoảng (5\cdot10^{11}) thao tác, vượt xa giới hạn. 

Quan sát hữu ích là đối với một dự án, chúng ta không cần phải xây dựng rõ ràng toàn bộ tập hợp các đường dẫn. Sắp xếp nhân viên của dự án theo thứ tự DFS. Giả sử thứ tự của chúng là (v_1,v_2,\ldots,v_k). Nhân viên đầu tiên đóng góp toàn bộ đường dẫn của nó từ gốc, trong đó có chứa`depth[v1] + 1`đỉnh. Sau đó, khi thêm (v_i), phần đường dẫn gốc đã được thành viên dự án trước đó bao phủ sẽ kết thúc tại 

[ 
LCA(v_{i-1},v_i). 
] 

Do đó, số đỉnh mới được đóng góp bởi (v_i) là 

[ 
độ sâu[v_i]-độ sâu[LCA(v_{i-1},v_i)]. 
] 

Như vậy câu trả lời là 

[ 
1+độ sâu[v_1] 
+\sum_{i=2}^{k} 
\left(độ sâu[v_i]-độ sâu[LCA(v_{i-1},v_i)]\right). 
] 

Lý do các lần xuất hiện thứ tự trước liên tiếp là đủ là vì các cây con hình thành các khoảng liền kề trong thứ tự trước. Khi hai nút được đánh dấu được tách ra theo thứ tự trước, mỗi nhánh giữa chúng được thể hiện bằng một trong các chuyển đổi liên tiếp. LCA của họ tính toán chính xác các phần của đường dẫn gốc trùng nhau. 

Do đó, chúng tôi chỉ cần một truy vấn LCA giữa các lần xuất hiện liên tiếp của mỗi dự án. Tổng cộng có tối đa (n-1) truy vấn như vậy. 

LCA nâng cấp nhị phân thông thường sẽ trả lời các truy vấn này trong mỗi truy vấn (O(\log n)) và yêu cầu bộ nhớ (O(n\log n)). Với (10^6) đỉnh, việc sử dụng bộ nhớ đó đặc biệt kém hấp dẫn trong Python. Vì tất cả các truy vấn LCA của chúng tôi đều được biết sau khi truyền tải theo thứ tự trước nên thay vào đó, chúng tôi có thể sử dụng thuật toán LCA ngoại tuyến của Tarjan. Nó trả lời tất cả các truy vấn này cùng nhau bằng cách sử dụng cấu trúc tập hợp rời rạc trong thời gian gần như tuyến tính và bộ nhớ tuyến tính. 

Do đó, chiến lược hoàn chỉnh là duyệt qua hệ thống phân cấp một lần trong đơn đặt hàng trước, tạo một truy vấn LCA giữa mỗi cặp nhân viên liên tiếp có cùng một dự án, sau đó xử lý tất cả các truy vấn bằng phiên bản lặp lại của thuật toán LCA ngoại tuyến của Tarjan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\alpha(n))) | (O(n)) | Đã chấp nhận | 

Ở đây (\alpha(n)) là hàm Ackermann nghịch đảo, hàm này tăng chậm đến mức nó thực sự không đổi trong phạm vi ràng buộc này. 

## Hướng dẫn thuật toán 

1. Chuyển đổi mảng cha thành biểu diễn con có gốc. Vì mỗi nhân viên có chính xác một người quản lý ngoại trừ Giám đốc điều hành, nên mỗi nhân viên không phải gốc rễ có thể được chèn vào danh sách con được liên kết của người quản lý đó. Chúng tôi sử dụng mảng thay vì danh sách danh sách Python vì một triệu đối tượng Python lồng nhau sẽ tiêu tốn quá nhiều bộ nhớ. 
2. Thực hiện DFS lặp lại từ CEO. Khi một nhân viên lần đầu tiên được đưa vào, hãy ghi lại chiều sâu của nhân viên đó và kiểm tra dự án của họ. Đối với mỗi dự án, giữ`prev[project]`, nhân viên gặp phải gần đây nhất của dự án đó trong đơn đặt hàng trước. 
3. Nếu nhân viên hiện tại là người xuất hiện đầu tiên trong dự án của nó, hãy khởi tạo câu trả lời của dự án đó bằng`depth[current] + 1`. Điều này tính cả chặng đường hoàn chỉnh từ CEO đến thành viên dự án đầu tiên. 
4. Nếu dự án đã xuất hiện trước đó, hãy tạo truy vấn LCA giữa`prev[project]`và nhân viên hiện tại. Nhân viên hiện tại là điểm cuối sau trong đơn đặt hàng trước. Lưu trữ truy vấn ở cả hai điểm cuối để thuật toán của Tarjan có thể xử lý truy vấn đó khi một trong hai điểm cuối kết thúc. Sau đó thay thế`prev[project]`với nhân viên hiện tại. 
5. DFS cũng ghi lại trình tự thứ tự sau. Chúng tôi cần thứ tự thứ hai này vì thuật toán LCA ngoại tuyến của Tarjan chỉ xử lý một đỉnh sau khi tất cả các con cháu của nó đã được xử lý. 
6. Khởi tạo cấu trúc tập hợp rời rạc với một bộ cho mỗi nhân viên. Đối với mỗi bộ, cũng duy trì cây tổ tiên hiện tại của nó. Các phép toán hợp được thực hiện sau khi một phần tử con kết thúc, chính xác như trong thuật toán của Tarjan. Liên kết theo thứ hạng giữ cho DSU nông, trong khi việc nén đường dẫn lặp lại`find`hoạt động có thời gian khấu hao gần như không đổi. 
7. Xử lý nhân viên theo thứ tự sau. Đánh dấu nhân viên hiện tại là đã xử lý. Đối với mỗi truy vấn LCA được đính kèm với nó, nếu điểm cuối khác đã được xử lý thì LCA sẽ được`ancestor[find(other)]`. Tại thời điểm này, thành phần DSU chứa điểm cuối khác biểu thị chính xác phần đã hoàn thành của cây cho tới LCA. 
8. Đối với truy vấn có điểm cuối đặt hàng trước sau này là`v`, thêm 

[ 
độ sâu[v]-độ sâu[LCA] 
] 

cho câu trả lời của dự án đó. Đây chính xác là một phần của con đường dẫn đến`v`điều đó chưa được bao gồm trong lần xuất hiện trước đó của dự án. 

1. Sau khi xử lý thắc mắc của nhân viên`u`, hợp nhất thành phần DSU của nó với thành phần mẹ của nó. Nếu như`u`là CEO, không có cha mẹ và quá trình xử lý kết thúc. Ngược lại, sau khi hợp nhất, hãy đặt tổ tiên của đại diện DSU mới thành`parent[u]`. 

### Tại sao nó hoạt động 

Đối với một dự án cố định, hãy đặt hàng trước các nhân viên của nó là (v_1,\ldots,v_k). Đường dẫn gốc tới (v_1) đóng góp chính xác`depth[v1] + 1`đỉnh. Hãy xem xét mọi việc sau (v_i). Bởi vì (v_{i-1}) là thành viên dự án ngay trước trong thứ tự trước, nên phần đã được bao phủ của đường dẫn gốc tới (v_i) kết thúc tại (LCA(v_{i-1},v_i)). Mọi thứ bên dưới LCA trên đường tới (v_i) đều mới, đóng góp chính xác`depth[v_i] - depth[LCA]`. Tính tổng các phần mới rời rạc này sẽ tính mỗi nhân viên tham dự đúng một lần. 

Bất biến LCA ngoại tuyến của Tarjan cung cấp các giá trị LCA cần thiết. Khi một đỉnh được xử lý, mọi cây con con hoàn chỉnh đã được hợp nhất vào thành phần DSU của nó, nhưng thành phần này vẫn chưa được hợp nhất thông qua đỉnh này vào cây cha của nó. Do đó, đối với một truy vấn có điểm cuối khác đã được xử lý, truy vấn tổ tiên được lưu trữ tại đại diện DSU của điểm cuối đó chính xác là tổ tiên chung thấp nhất của hai điểm cuối truy vấn. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    color = array('i', map(int, input().split()))

    # parent[v] is zero-based, -1 for the CEO.
    parent = array('i', (x - 1 for x in map(int, input().split())))

    # First-child linked lists.
    head = array('i', [-1]) * n
    nxt = array('i', [-1]) * n

    root = 0
    for v in range(n):
        p = parent[v]
        if p == -1:
            root = v
        else:
            nxt[v] = head[p]
            head[p] = v

    depth = array('i', [0]) * n

    # Last occurrence of every project in preorder.
    prev = array('i', [-1]) * (m + 1)

    # At most n-1 consecutive-occurrence queries exist.
    q_u = array('i', [0]) * n
    q_v = array('i', [0]) * n
    q1 = array('i', [-1]) * n
    q2 = array('i', [-1]) * n
    q_count = 0

    answer = array('i', [0]) * (m + 1)

    # Iterative DFS. head[u] is consumed as the current child iterator.
    stack = array('i', [root])
    postorder = array('i')

    # Enter the root.
    c = color[root]
    prev[c] = root
    answer[c] = 1

    while stack:
        u = stack[-1]
        e = head[u]

        if e == -1:
            stack.pop()
            postorder.append(u)
            continue

        # Consume this child edge.
        head[u] = nxt[e]
        v = e
        depth[v] = depth[u] + 1

        c = color[v]
        old = prev[c]

        if old == -1:
            answer[c] = depth[v] + 1
        else:
            qid = q_count
            q_count += 1

            q_u[qid] = old
            q_v[qid] = v

            if q1[old] == -1:
                q1[old] = qid
            else:
                q2[old] = qid

            if q1[v] == -1:
                q1[v] = qid
            else:
                q2[v] = qid

        prev[c] = v
        stack.append(v)

    # Tarjan offline LCA.
    dsu = array('i', range(n))
    ancestor = array('i', range(n))
    rank = bytearray(n)
    processed = bytearray(n)

    def find(x):
        r = x
        while dsu[r] != r:
            r = dsu[r]

        while dsu[x] != x:
            y = dsu[x]
            dsu[x] = r
            x = y

        return r

    for u in postorder:
        processed[u] = 1

        qid = q1[u]
        if qid != -1:
            v = q_v[qid] if q_u[qid] == u else q_u[qid]
            if processed[v]:
                r = find(v)
                lca = ancestor[r]
                cur = q_v[qid]
                answer[color[cur]] += depth[cur] - depth[lca]

        qid = q2[u]
        if qid != -1:
            v = q_v[qid] if q_u[qid] == u else q_u[qid]
            if processed[v]:
                r = find(v)
                lca = ancestor[r]
                cur = q_v[qid]
                answer[color[cur]] += depth[cur] - depth[lca]

        p = parent[u]
        if p != -1:
            ru = find(u)
            rp = find(p)

            if ru != rp:
                if rank[ru] < rank[rp]:
                    dsu[ru] = rp
                    new_root = rp
                elif rank[ru] > rank[rp]:
                    dsu[rp] = ru
                    new_root = ru
                else:
                    dsu[rp] = ru
                    rank[ru] += 1
                    new_root = ru

                ancestor[new_root] = p

    sys.stdout.write(' '.join(map(str, answer[1:])))

if __name__ == "__main__":
    solve()
```Dòng đầu vào đầu tiên được đọc bình thường, trong khi hai mảng lớn được xây dựng trực tiếp từ các trình vòng lặp. sử dụng`array('i')`giữ mỗi số nguyên ở bốn byte thay vì biểu diễn số nguyên Python lớn hơn nhiều. Sự khác biệt này quan trọng khi có nhiều mảng, mỗi mảng chứa một triệu phần tử. 

Mảng cha được chuyển đổi thành các chỉ số dựa trên số 0 ngay lập tức. CEO trở thành`-1`, cung cấp một trọng điểm thuận tiện cho đỉnh duy nhất không nên được hợp nhất vào thành phần DSU khác. 

Cây con được biểu diễn bằng cách sử dụng`head`Và`nxt`. Đối với mỗi nhân viên`v`,`nxt[v]`chỉ vào một đứa trẻ khác của`parent[v]`. DFS tiêu thụ`head[u]`là con trỏ con hiện tại của nó, do đó, một mảng lặp riêng biệt là không cần thiết. 

DFS đặt hàng trước thực hiện hai công việc cùng một lúc. Nó tính toán độ sâu và phát hiện các lần xuất hiện liên tiếp của dự án, trong khi quá trình duyệt lặp tương tự sẽ ghi lại thứ tự sau cần thiết sau này. Ngăn xếp chỉ chứa các chỉ mục đỉnh, tránh đệ quy Python và lỗi của nó đối với chuỗi một triệu nhân viên. 

Có thể có nhiều nhất (n-1) truy vấn LCA vì lần xuất hiện đầu tiên của mỗi dự án không tạo ra truy vấn nào. Mỗi đỉnh có thể là điểm cuối của nhiều nhất hai truy vấn như vậy, một truy vấn kết nối nó với lần xuất hiện trước và một kết nối nó với lần xuất hiện tiếp theo. Điều này cho phép`q1`Và`q2`để lưu trữ ID truy vấn mà không cần xây dựng danh sách lớn các đối tượng Python cho mọi đỉnh. 

Câu trả lời được khởi tạo bằng`depth[v] + 1`cho lần xuất hiện đầu tiên của mỗi dự án. các`+1`tính đến chính nhân viên khi độ sâu được đo với Giám đốc điều hành ở độ sâu bằng 0. 

Giai đoạn thứ hai thực hiện lặp đi lặp lại thuật toán LCA ngoại tuyến của Tarjan.`processed[v]`đóng vai màu đen của Tarjan. Một truy vấn chỉ được trả lời khi điểm cuối khác của nó đã được xử lý. Tại thời điểm đó, thành phần DSU của nó đại diện cho nhánh hoàn chỉnh chứa điểm cuối đó và`ancestor[find(v)]`cung cấp LCA chính xác. 

Liên kết theo thứ hạng được sử dụng mặc dù DSU đại diện cho việc duyệt cây. Đại diện của một tập hợp không nhất thiết phải là cây tổ tiên thực sự, bởi vì`ancestor`ghi lại riêng đỉnh cây nào đại diện cho tổ tiên có liên quan cao nhất của thành phần đó. Sự khác biệt này là điều cho phép đảm bảo DSU tiêu chuẩn gần như liên tục. 

Tất cả số học đều vừa vặn thoải mái trong số nguyên 32 bit có dấu vì mọi câu trả lời của dự án đều có nhiều nhất là (n\le10^6). Số nguyên Python cũng sẽ xử lý các giá trị một cách an toàn, nhưng mảng số nguyên nhỏ gọn rất hữu ích cho việc sử dụng bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hệ thống phân cấp là```
1
├── 3
│   ├── 4
│   └── 5
└── 2
    └── 6
```Thứ tự chèn con tạo ra thứ tự trước DFS thực tế`1, 3, 5, 4, 2, 6`. Sự xuất hiện của dự án và các truy vấn LCA kết quả được hiển thị bên dưới. 

| Vị trí đặt hàng trước | Nhân viên | Dự án | Dự án tương tự trước đó | Đóng góp ban đầu mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | không | 1 | 
| 2 | 3 | 4 | không | 2 | 
| 3 | 5 | 2 | không | 3 | 
| 4 | 4 | 3 | không | 3 | 
| 5 | 2 | 2 | 5 | 0 | 
| 6 | 6 | 4 | 3 | 0 | 

Đối với dự án 2, truy vấn là`(5, 2)`. LCA của họ là nhân viên 1. Nhân viên 5 ban đầu đóng góp ba đỉnh, đó là`{1,3,5}`. Thêm nhân viên 2 đóng góp`depth[2] - depth[1] = 1`, đưa ra bốn người tham dự. 

Đối với dự án 4, truy vấn là`(3, 6)`. LCA của họ là nhân viên 1. Lần xuất hiện đầu tiên đóng góp hai đỉnh`{1,3}`và nhân viên 6 đóng góp thêm hai cấp độ từ nhân viên 1, tạo ra bốn người tham dự. 

Các câu trả lời cuối cùng là`1 4 3 4`. 

### Ví dụ tùy chỉnh 

Hãy xem xét```
5 2
1 1 2 1 2
0 1 1 2 2
```Cái cây là```
1
├── 2
│   ├── 4
│   └── 5
└── 3
```Một đơn đặt hàng trước DFS có thể là`1, 3, 2, 5, 4`. Trạng thái liên quan là: 

| Nhân viên | Dự án | Dự án tương tự trước đó | Truy vấn LCA | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | không | 1 | 
| 3 | 2 | không | không | 2 | 
| 2 | 1 | 1 |`(1,2)`| 1 | 
| 5 | 2 | 3 |`(3,5)`| 2 | 
| 4 | 1 | 2 |`(2,4)`| 1 | 

Đối với dự án 1, nhân viên đầu tiên là CEO nên số tiền đóng góp ban đầu là một. Truy vấn`(1,2)`có LCA 1 và đóng góp thêm một đỉnh nữa. Truy vấn`(2,4)`có LCA 2 và đóng góp thêm một đỉnh nữa. Kết quả là`3`. 

Đối với dự án 2, nhân viên 3 đóng góp`{1,3}`và nhân viên 5 đóng góp đường dẫn bên dưới LCA 1, cụ thể là`{2,5}`. Kết quả là`4`. 

Đầu ra là`3 4`. Ví dụ này chứng minh tại sao các cặp tổ tiên-con cháu không được phép đếm lại toàn bộ đường dẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\alpha(n))) | DFS, cấu trúc truy vấn và truyền tải thứ tự sau là tuyến tính, trong khi các hoạt động DSU của Tarjan được khấu hao (O(\alpha(n))). | 
| Không gian | (O(n)) | Mọi cấu trúc cây, truy vấn, truyền tải và DSU đều sử dụng một mảng nhỏ gọn có kích thước tuyến tính. | 

Ràng buộc (n\le10^6) làm cho thiết kế bộ nhớ tuyến tính trở nên đặc biệt phù hợp. Việc triển khai lưu trữ dữ liệu số nguyên một cách nhỏ gọn`array`đối tượng và sử dụng truyền tải lặp lại, do đó nó tránh được cả ngăn xếp cuộc gọi đệ quy của Python và chi phí đối tượng lớn của các danh sách lồng nhau. Thuật toán chỉ thực hiện một số lần duyệt cây không đổi cộng với các hoạt động DSU được khấu hao gần như không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

def solve():
    n, m = map(int, input().split())
    color = array('i', map(int, input().split()))
    parent = array('i', (x - 1 for x in map(int, input().split())))

    head = array('i', [-1]) * n
    nxt = array('i', [-1]) * n

    root = 0
    for v in range(n):
        p = parent[v]
        if p == -1:
            root = v
        else:
            nxt[v] = head[p]
            head[p] = v

    depth = array('i', [0]) * n
    prev = array('i', [-1]) * (m + 1)
    answer = array('i', [0]) * (m + 1)

    q_u = array('i', [0]) * n
    q_v = array('i', [0]) * n
    q1 = array('i', [-1]) * n
    q2 = array('i', [-1]) * n
    q_count = 0

    stack = array('i', [root])
    postorder = array('i')

    c = color[root]
    prev[c] = root
    answer[c] = 1

    while stack:
        u = stack[-1]
        e = head[u]

        if e == -1:
            stack.pop()
            postorder.append(u)
            continue

        head[u] = nxt[e]
        v = e
        depth[v] = depth[u] + 1

        c = color[v]
        old = prev[c]

        if old == -1:
            answer[c] = depth[v] + 1
        else:
            qid = q_count
            q_count += 1

            q_u[qid] = old
            q_v[qid] = v

            if q1[old] == -1:
                q1[old] = qid
            else:
                q2[old] = qid

            if q1[v] == -1:
                q1[v] = qid
            else:
                q2[v] = qid

        prev[c] = v
        stack.append(v)

    dsu = array('i', range(n))
    ancestor = array('i', range(n))
    rank = bytearray(n)
    processed = bytearray(n)

    def find(x):
        r = x
        while dsu[r] != r:
            r = dsu[r]

        while dsu[x] != x:
            y = dsu[x]
            dsu[x] = r
            x = y

        return r

    for u in postorder:
        processed[u] = 1

        for qid in (q1[u], q2[u]):
            if qid == -1:
                continue

            v = q_v[qid] if q_u[qid] == u else q_u[qid]

            if processed[v]:
                lca = ancestor[find(v)]
                cur = q_v[qid]
                answer[color[cur]] += depth[cur] - depth[lca]

        p = parent[u]
        if p != -1:
            ru = find(u)
            rp = find(p)

            if ru != rp:
                if rank[ru] < rank[rp]:
                    dsu[ru] = rp
                    new_root = rp
                elif rank[ru] > rank[rp]:
                    dsu[rp] = ru
                    new_root = ru
                else:
                    dsu[rp] = ru
                    rank[ru] += 1
                    new_root = ru

                ancestor[new_root] = p

    sys.stdout.write(' '.join(map(str, answer[1:])))

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        solve()
        return ""
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided sample.
sample1 = """\
6 4
1 2 4 3 2 4
0 1 1 3 3 2
"""

# The helper above writes directly to stdout in solve(), so capture it.
def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

assert run(sample1) == "1 4 3 4", "sample 1"

assert run("""\
1 1
1
0
""") == "1", "single employee"

assert run("""\
4 1
1 1 1 1
0 1 2 3
""") == "4", "all employees same project"

assert run("""\
4 2
1 2 1 2
0 1 2 3
""") == "3 4", "ancestor-descendant overlap"

assert run("""\
5 2
1 1 2 1 2
0 1 1 2 2
""") == "3 4", "different branches"

# Maximum-size shape, one project, one million employees in a chain.
n = 1_000_000
colors = "1 " * (n - 1) + "1"
parents = "0 " + " ".join(map(str, range(1, n)))
max_case = f"{n} 1\n{colors}\n{parents}\n"

assert run(max_case) == "1000000", "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 / 0`|`1`| Cây có kích thước tối thiểu và dự án chỉ dành cho CEO | 
|`4 1 / 1 1 1 1 / 0 1 2 3`|`4`| Tất cả nhân viên chia sẻ một dự án và mọi con đường đều trùng lặp | 
|`4 2 / 1 2 1 2 / 0 1 2 3`|`3 4`| Sự chồng chéo của con cháu tổ tiên và độ sâu ranh giới | 
|`5 2 / 1 1 2 1 2 / 0 1 1 2 2`|`3 4`| Các thành viên dự án ở các ngành khác nhau | 
| Một triệu nhân viên trong một dự án tạo thành chuỗi |`1000000`| Tối đa`n`, độ sâu dài, DFS lặp và bộ nhớ tuyến tính | 

## Vỏ cạnh 

Đối với trường hợp nhân viên độc thân```
1 1
1
0
```CEO cũng là thành viên duy nhất của dự án. Trong quá trình duyệt theo thứ tự trước, dự án 1 không xuất hiện trước đó nên câu trả lời của nó được khởi tạo là`depth[1] + 1 = 1`. Không có truy vấn LCA và CEO không có công ty mẹ để hợp nhất. Kết quả là`1`. 

Đối với chuỗi đều bằng nhau```
4 1
1 1 1 1
0 1 2 3
```đặt hàng trước là`1,2,3,4`. Sự xuất hiện đầu tiên đóng góp một đỉnh. Các truy vấn liên tiếp được`(1,2)`,`(2,3)`, Và`(3,4)`. LCA của họ tương ứng`1`,`2`, Và`3`, do đó mỗi truy vấn đều đóng góp chính xác một đỉnh mới. Câu trả lời trở thành`1+1+1+1=4`. Không có nhân viên nào được tính hai lần mặc dù mọi thành viên dự án đều nằm trên cùng một đường dẫn gốc. 

Đối với sự chồng chéo tổ tiên-con cháu,```
4 2
1 2 1 2
0 1 2 3
```dự án 1 xảy ra ở nhân viên 1 và 3. Lần xuất hiện đầu tiên đóng góp cho nhân viên 1. LCA của nhân viên 1 và 3 là nhân viên 1, do đó lần xuất hiện thứ hai đóng góp`depth[3] - depth[1] = 2`. Câu trả lời là`3`, tương ứng với nhân viên`{1,2,3}`. Dự án 2 tương tự bao gồm cả bốn nhân viên, mang lại`4`. 

Đối với các thành viên ở các chi nhánh khác nhau,```
5 2
1 1 2 1 2
0 1 1 2 2
```dự án 2 có nhân viên 3 và 5. LCA của họ là nhân viên 1. Thành viên đầu tiên đóng góp hai đỉnh,`{1,3}`, và đóng góp thứ hai`depth[5] - depth[1] = 2`, thêm`{2,5}`. Kết quả là`4`. CEO được chia sẻ được tính một lần mặc dù cả hai thành viên dự án đều yêu cầu CEO đó. 

Đối với trường hợp có độ sâu tối đa, một chuỗi gồm một triệu nhân viên nhấn mạnh cả độ sâu truyền tải và bộ nhớ. Việc triển khai không bao giờ gọi DFS đệ quy, do đó giới hạn đệ quy Python không liên quan. Mỗi nhân viên chỉ đóng góp vào một số mảng nhỏ gọn không đổi và DSU của Tarjan xử lý các truy vấn dự án liên tiếp mà không cần xây dựng bảng tổ tiên (O(n\log n)). Đối với một dự án duy nhất chứa mọi nhân viên, sự kết hợp của các đường dẫn gốc là toàn bộ chuỗi, vì vậy câu trả lời là chính xác`1000000`.
