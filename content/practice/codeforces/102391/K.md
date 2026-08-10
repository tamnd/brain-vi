---
title: "CF 102391K - Làn Gió Thay Đổi"
description: "Chúng ta có hai cây có trọng số trên cùng một tập đỉnh, có nhãn đỉnh từ (1) đến (N). Đối với hai nhãn (i) và (j), khoảng cách của chúng không được đo chỉ bằng một trong hai cây."
date: "2026-08-10T21:11:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "K"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 337
verified: true
draft: false
---

[CF 102391K - Làn gió thay đổi](https://codeforces.com/problemset/problem/102391/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 37 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cây có trọng số trên cùng một tập đỉnh, có nhãn đỉnh từ (1) đến (N). Đối với hai nhãn (i) và (j), khoảng cách của chúng không được đo chỉ bằng một trong hai cây. Thay vào đó, chúng ta cộng độ dài đường đi giữa chúng trong cây đầu tiên với độ dài đường đi giữa chúng trong cây thứ hai. Với mỗi đỉnh (i), chúng ta cần tổng nhỏ nhất như vậy trên mọi đỉnh (j) khác. 

Hai cây có thể có hình dạng và trọng lượng cạnh hoàn toàn khác nhau. Một đỉnh gần với (i) trong cây thứ nhất có thể ở rất xa trong cây thứ hai, do đó việc tìm một đỉnh gần nhất ở một trong hai cây một cách độc lập không giúp ích được gì. Câu trả lời là truy vấn lân cận gần nhất trong số liệu tích của hai cây. 

Trường hợp lớn nhất có (250.000) đỉnh và mỗi cây có (249.999) cạnh. Một thuật toán bậc hai sẽ kiểm tra khoảng (N(N-1)/2), khoảng (31) tỷ, cặp đỉnh trong trường hợp xấu nhất. Điều đó vượt xa phạm vi thực tế của giải pháp cuộc thi 12 giây. Giới hạn vấn đề chính thức là 12 giây và 1024 MiB, do đó phương pháp (O(N\log^2N)) là phù hợp. 

Ngoài ra còn có hai chi tiết số quan trọng. Khoảng cách của cây riêng lẻ có thể lớn tới khoảng (2,5\cdot10^{14}), do đó, khoảng cách kết hợp cần số học 64 bit trong C++. Số nguyên Python tự động xử lý việc này. Ngoài ra, (j=i) không bao giờ được sử dụng. Vì khoảng cách của nó bằng 0, việc vô tình cho phép nó sẽ ngay lập tức khiến mọi câu trả lời trở thành con số 0. 

Hãy xem xét cây nhỏ nhất có thể:```
2
1 2 1000000000
1 2 1000000000
```Đỉnh duy nhất có thể có khác là điểm cuối khác, vì vậy đầu ra đúng là```
2000000000
2000000000
```Việc triển khai bao gồm chính đỉnh được truy vấn sẽ in không chính xác số 0. 

Trường hợp hữu ích thứ hai là khi cả hai cây đều có hình dạng đơn giản giống nhau và tất cả các trọng số đều bằng nhau:```
3
1 2 1
2 3 1
1 2 1
2 3 1
```Mỗi đỉnh có một đỉnh khác có tổng khoảng cách (2), vì vậy câu trả lời là```
2
2
2
```Điều này nắm bắt các triển khai gây nhầm lẫn giữa khoảng cách trong một cây với tổng yêu cầu của cả hai khoảng cách. 

Cuối cùng, những cái cây không cần phải có hình dạng giống nhau chút nào. Ví dụ,```
3
1 2 1
2 3 100
1 2 100
2 3 1
```có khoảng cách kết hợp (101) giữa cặp liền kề và (202) giữa các đỉnh (1) và (3). Đầu ra đúng là```
101
101
101
```Đây là một cách kiểm tra hữu ích để tránh việc vô tình sử dụng lại hình học của cây đầu tiên cho cây thứ hai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp rất đơn giản. Tính khoảng cách giữa mỗi cặp (i,j) trong cả hai cây và giảm thiểu tổng của mỗi (i). Vì một cây có các đường dẫn duy nhất nên người ta có thể nhổ tận gốc từng cây và đạt được tất cả khoảng cách từ một nguồn trong thời gian tuyến tính, nhưng thực hiện điều đó từ mọi nguồn vẫn tốn chi phí (O(N^2)). Tương tự, xem xét rõ ràng mỗi cặp thực hiện so sánh (N(N-1)/2), đạt khoảng (31,25) tỷ cặp khi (N=250.000). 

Lực lượng vũ phu là chính xác vì nó đánh giá trực tiếp số lượng được giảm thiểu. Sự thất bại của nó hoàn toàn là do tính toán. Chúng ta cần khai thác thực tế là các khoảng cách trong cây có thể được phân tách xung quanh dấu phân cách. 

Dấu phân cách hữu ích là sự phân tách trung tâm. Việc loại bỏ trọng tâm sẽ chia cây thành các thành phần có kích thước không quá một nửa thành phần hiện tại. Lặp lại điều này sẽ tạo ra một cây trung tâm có chiều cao logarit. Thuộc tính cân bằng tiêu chuẩn này chính xác là thứ cho phép chúng ta thay thế một cặp đỉnh tùy ý bằng một cặp đỉnh trung tâm. 

Xây dựng phân tách trọng tâm độc lập cho (T_1) và (T_2). Đối với các đỉnh (i) và (j), đặt (L_1) là tổ tiên chung thấp nhất của chúng trong cây trọng tâm của (T_1) và đặt (L_2) là tổ tiên chung thấp nhất của chúng trong cây trọng tâm của (T_2). Bằng cách phân tách trọng tâm tách các thành phần, đường đi ban đầu giữa (i) và (j) trong (T_1) đi qua (L_1), và tương tự đường đi trong (T_2) đi qua (L_2). Do đó 

[d_1(L_1,i)+d_2(L_2,i)] 
+ 
[d_1(L_1,j)+d_2(L_2,j)]. 
] 

Đây là phép biến đổi đại số quan trọng. Sự đóng góp của (i) và (j) tách biệt hoàn toàn khi (L_1,L_2) được cố định. 

Vẫn còn một điều kiện khó xử: (L_1) và (L_2) phải là hai LCA cây trung tâm chính xác. Chúng ta có thể loại bỏ tình trạng đó. Đối với (i,L_1,L_2 cố định), hãy xem xét mọi đỉnh (j) đơn giản là hậu duệ của (L_1) trong cây trọng tâm thứ nhất và là hậu duệ của (L_2) trong cây trọng tâm thứ hai. Bộ này lớn hơn bộ có chính xác hai LCA đó. 

Đối với bất kỳ (j) như vậy, 

[ 
d_1(L_1,i)+d_1(L_1,j)\ge d_1(i,j) 
] 

và 

[ 
d_2(L_2,i)+d_2(L_2,j)\ge d_2(i,j) 
] 

bởi bất đẳng thức tam giác. Vì vậy, ứng viên thoải mái không bao giờ có thể nhỏ hơn khoảng cách thực. Mặt khác, đỉnh thực sự gần nhất (j) có một cặp chính xác nào đó (L_1,L_2) và cặp đó được bao gồm trong các trường hợp thoải mái. Do đó, việc lấy giá trị tối thiểu trong tất cả các trường hợp thoải mái vẫn cho câu trả lời chính xác. 

Đối với trọng tâm cố định (L_2) trong phân tách thứ hai, chúng ta có thể truy cập mọi đỉnh trong cây con trọng tâm của nó. Đối với mỗi đỉnh (v) như vậy, chúng ta liệt kê tất cả các tổ tiên trung tâm (L_1) của (v) trong phân tách đầu tiên. Đối với cố định (L_1), chúng tôi chèn 

[ 
d_1(L_1,v)+d_2(L_2,v) 
] 

thành cấu trúc tối thiểu hai phần tử. Chúng ta chỉ cần các giá trị nhỏ nhất và nhỏ thứ hai, vì khi truy vấn đỉnh (v), giá trị nhỏ nhất có thể đến từ chính (v) và sau đó phải được bỏ qua. 

Mỗi đỉnh chỉ có (O(\log N)) tổ tiên trung tâm trong cả hai quá trình phân rã. Do đó, bảng liệt kê lồng nhau chứa các kết hợp (O(N\log^2N)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log^2N)) | (O(N\log N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng phân rã trọng tâm của cây đầu tiên. Bất cứ khi nào một trọng tâm (c) được chọn, hãy duyệt thành phần hiện tại của nó và ghi lại (d_1(c,v)) cho mọi đỉnh (v) trong thành phần đó. Vì (c) trở thành tổ tiên của mọi đỉnh như vậy trong cây trọng tâm, điều này mang lại cho mỗi đỉnh một danh sách các tổ tiên trọng tâm của nó cùng với khoảng cách từ cây ban đầu của chúng. 
2. Xây dựng phân rã trọng tâm của cây thứ hai, nhưng chúng ta không cần lưu trữ danh sách khoảng cách của nó. Trong quá trình xử lý sau này, bất cứ khi nào một centroid (c) được chọn, chúng ta có thể duyệt qua thành phần hiện tại của nó và trực tiếp thu được (d_2(c,v)) cho mọi phần tử con (v). 
3. Khởi tạo mọi câu trả lời thành vô cùng. Khi xử lý một centroid (c=L_2) của cây thứ hai, thành phần hiện tại của nó chính xác là cây con của (c) trong cây centroid thứ hai. Do đó, mọi đỉnh được truy cập trong thành phần này đều là ứng cử viên hợp lệ cho điều kiện thoải mái (L_2) là tổ tiên. 
4. Đối với mỗi đỉnh được truy cập (v), hãy lặp qua trung tâm tổ tiên được lưu trữ của nó ((L_1,d_1(L_1,v))) từ cây đầu tiên. Với mỗi (L_1), hãy tính 
[ 
x=d_1(L_1,v)+d_2(c,v). 
] 
Giữ hai giá trị nhỏ nhất của (x) cho mọi (L_1), cùng với đỉnh tạo ra mỗi giá trị. 
5. Sau khi tất cả các đỉnh trong thành phần cây thứ hai hiện tại đã được chèn, hãy lặp lại chúng. Đối với mọi đỉnh (v) và mọi cây tổ tiên đầu tiên (L_1), lấy giá trị được lưu trữ nhỏ nhất thuộc về một đỉnh khác với (v). Nếu giá trị nhỏ nhất được tạo ra bởi (v), thay vào đó hãy sử dụng giá trị nhỏ thứ hai. 
6. Cộng giá trị cho chính (v): 
[ 
d_1(L_1,v)+d_2(c,v)+ 
\min_{j\ne v} 
[d_1(L_1,j)+d_2(c,j)]. 
] 
Cập nhật câu trả lời chung của (v) với ứng viên này. Đây chính xác là biểu hiện thoải mái có nguồn gốc ở trên. 
7. Đánh dấu trọng tâm của cây thứ hai hiện tại là đã bị loại bỏ và tiếp tục độc lập với mọi thành phần còn lại. Bởi vì mọi thành phần có nhiều nhất một nửa kích thước trước đó, nên mỗi đỉnh chỉ tham gia vào các mức trọng tâm (O(\log N)). 
8. In kết quả tối thiểu cho mỗi đỉnh. Tự ghép đôi được loại trừ bằng cách chọn mức tối thiểu thứ hai bất cứ khi nào mức tối thiểu đầu tiên thuộc về đỉnh được truy vấn. 

### Tại sao nó hoạt động 

Cố định hai đỉnh (i\ne j) và đặt (L_1,L_2) là tổ tiên chung thấp nhất của chúng trong hai phân tách trọng tâm. Bởi vì các trọng tâm đó tách biệt (i) và (j), nên các đường đi của cây ban đầu sẽ đi qua các trọng tâm tương ứng. Do đó khoảng cách kết hợp thực sự của họ bằng 

[ 
d_1(L_1,i)+d_2(L_2,i)+d_1(L_1,j)+d_2(L_2,j). 
] 

Thuật toán xem xét cặp tổ tiên chính xác này vì cả hai đều là tổ tiên của (i) và (j) nằm trong cả hai cây con trung tâm tương ứng. Vì vậy, sự tối ưu thực sự nằm trong số các ứng cử viên được xem xét. 

Mọi ứng viên thoải mái đều có dạng 

[ 
d_1(L_1,i)+d_1(L_1,j)+d_2(L_2,i)+d_2(L_2,j). 
] 

Theo bất đẳng thức tam giác trong mỗi cây, giá trị này ít nhất bằng (d_1(i,j)+d_2(i,j)). Vì vậy, thuật toán không bao giờ có thể tạo ra giá trị dưới mức tối ưu thực sự. Vì cặp tối ưu thực cũng được xem xét nên mức tối thiểu do thuật toán tạo ra chính xác là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def get_component_centroid(adj, root, removed, parent, size):
    nodes = []
    stack = [root]
    parent[root] = -1

    while stack:
        u = stack.pop()
        nodes.append(u)
        pu = parent[u]

        for v, _ in adj[u]:
            if removed[v] or v == pu:
                continue
            parent[v] = u
            stack.append(v)

    for u in reversed(nodes):
        s = 1
        for v, _ in adj[u]:
            if removed[v]:
                continue
            if parent[v] == u:
                s += size[v]
        size[u] = s

    total = len(nodes)
    centroid = nodes[0]
    best_balance = total + 1

    for u in nodes:
        largest = total - size[u]

        for v, _ in adj[u]:
            if removed[v]:
                continue
            if parent[v] == u and size[v] > largest:
                largest = size[v]

        if largest < best_balance:
            best_balance = largest
            centroid = u

    return nodes, centroid

def build_ancestors(adj, n):
    removed = bytearray(n)
    parent = [-1] * n
    size = [0] * n
    ancestors = [[] for _ in range(n)]

    tasks = [0]

    while tasks:
        root = tasks.pop()

        _, centroid = get_component_centroid(
            adj, root, removed, parent, size
        )

        stack = [(centroid, -1, 0)]

        while stack:
            u, p, d = stack.pop()
            ancestors[u].append((centroid, d))

            for v, w in adj[u]:
                if removed[v] or v == p:
                    continue
                stack.append((v, u, d + w))

        removed[centroid] = 1

        for v, _ in adj[centroid]:
            if not removed[v]:
                tasks.append(v)

    return ancestors

def solve():
    input = sys.stdin.readline
    n = int(input())

    adj1 = [[] for _ in range(n)]
    adj2 = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        adj1[u].append((v, w))
        adj1[v].append((u, w))

    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        adj2[u].append((v, w))
        adj2[v].append((u, w))

    ancestors = build_ancestors(adj1, n)

    removed = bytearray(n)
    parent = [-1] * n
    size = [0] * n

    best1 = [INF] * n
    best2 = [INF] * n
    id1 = [-1] * n
    id2 = [-1] * n
    seen = [0] * n
    token = 0

    ans = [INF] * n
    tasks = [0]

    while tasks:
        root = tasks.pop()

        _, centroid = get_component_centroid(
            adj2, root, removed, parent, size
        )

        nodes_dist = []
        stack = [(centroid, -1, 0)]

        while stack:
            u, p, d = stack.pop()
            nodes_dist.append((u, d))

            for v, w in adj2[u]:
                if removed[v] or v == p:
                    continue
                stack.append((v, u, d + w))

        token += 1

        for v, d2 in nodes_dist:
            for a, d1 in ancestors[v]:
                if seen[a] != token:
                    seen[a] = token
                    best1[a] = INF
                    best2[a] = INF
                    id1[a] = -1
                    id2[a] = -1

                value = d1 + d2

                if value < best1[a]:
                    best2[a] = best1[a]
                    id2[a] = id1[a]
                    best1[a] = value
                    id1[a] = v
                elif value < best2[a]:
                    best2[a] = value
                    id2[a] = v

        for v, d2 in nodes_dist:
            for a, d1 in ancestors[v]:
                if id1[a] == v:
                    other = best2[a]
                else:
                    other = best1[a]

                if other < INF:
                    candidate = d1 + d2 + other
                    if candidate < ans[v]:
                        ans[v] = candidate

        removed[centroid] = 1

        for v, _ in adj2[centroid]:
            if not removed[v]:
                tasks.append(v)

    sys.stdout.write("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Người trợ giúp đầu tiên,`get_component_centroid`, thực hiện một quá trình duyệt hoàn toàn lặp lại. Điều này tránh được các vấn đề về độ sâu đệ quy Python trên đường dẫn chứa (250.000) đỉnh. Đầu tiên, nó ghi lại thành phần hiện tại và mối quan hệ cha mẹ, sau đó tính toán kích thước cây con theo thứ tự ngược lại và cuối cùng chọn một đỉnh có phần lớn nhất còn lại càng nhỏ càng tốt.`build_ancestors`xây dựng phân rã trung tâm đầu tiên. Các cạnh có trọng số ban đầu không bao giờ bị thay đổi. Đối với mỗi tâm, quá trình truyền tải ghi lại khoảng cách có trọng số thực tế từ tâm đó đến mọi đỉnh trong thành phần hiện tại của nó. Đây chính xác là thông tin được yêu cầu sau này, do đó không cần phải xây dựng cây trọng tâm một cách rõ ràng. 

Việc phân tách thứ hai được xử lý sau khi phân tách đầu tiên được lưu trữ. Đối với mỗi trọng tâm của cây thứ hai,`nodes_dist`chứa mọi đỉnh trong cây con trọng tâm đó và khoảng cách của nó tới trọng tâm. Mỗi đỉnh như vậy đã biết tất cả tổ tiên trọng tâm cây đầu tiên của nó, do đó, việc lặp lại hai danh sách sẽ liệt kê mọi kết hợp ((L_1,L_2,v)) cần thiết. 

Bốn mảng`best1`,`best2`,`id1`, Và`id2`đại diện cho hai giá trị nhỏ nhất cho mỗi giá trị (L_1). các`seen`mảng cho phép xóa lười biếng. Thay vì đặt lại tất cả các mục (N) mỗi khi một centroid được xử lý, chỉ các giá trị (L_1) thực sự gặp trong thành phần đó mới được khởi tạo. 

Việc loại trừ nghiêm ngặt đỉnh hiện tại được xử lý bởi ID đỉnh được lưu trữ. Nếu như`id1[a] == v`, ứng viên nhỏ nhất là người tự ghép đôi và`best2[a]`phải được sử dụng. Nếu ứng cử viên đầu tiên thuộc về một đỉnh khác, nó sẽ hợp lệ ngay lập tức. 

Tất cả số học được thực hiện bằng cách sử dụng số nguyên Python. Trong C++, nhu cầu triển khai tương ứng`long long`, vì tổng khoảng cách của hai cây có thể vượt quá (2^{31}-1). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với cây đầu tiên, đỉnh (4) là trọng tâm cấp cao nhất tự nhiên. Một trong những mối quan hệ trọng tâm con cháu của nó là (2), phân cách các đỉnh (1) và (2). Trong cây thứ hai, đỉnh (1) là tâm cấp cao nhất. 

Đối với cặp (L_1=2,L_2=1), các đỉnh liên quan là (1) và (2). Giá trị của chúng trước khi loại trừ đỉnh truy vấn là 

[ 
d_1(2,1)+d_2(1,1)=10, 
] 

và 

[ 
d_1(2,2)+d_2(1,2)=15. 
] 

Do đó, hai giá trị nhỏ nhất là (10) từ đỉnh (1) và (15) từ đỉnh (2). 

| Đỉnh truy vấn | (L_1) | (L_2) | Giá trị riêng | Tối thiểu khác | Ứng viên | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 10 | 15 | 25 | 
| 2 | 2 | 1 | 15 | 10 | 25 | 

Đối với đỉnh (1), thí sinh sử dụng đỉnh (2) cho (10+15=25). Đối với đỉnh (2), cặp tương tự được sử dụng theo hướng ngược lại, cho (15+10=25). 

Các kết hợp trọng tâm khác tạo ra các câu trả lời còn lại. 

| Đỉnh | Nhân chứng tốt nhất | Khoảng cách cây đầu tiên | Khoảng cách cây thứ hai | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 10 | 15 | 25 | 
| 2 | 1 | 10 | 15 | 25 | 
| 3 | 1 | 60 | 25 | 85 | 
| 4 | 1 | 30 | 35 | 65 | 
| 5 | 1 | 80 | 25 | 105 | 

Điều này thể hiện quy tắc tự loại trừ cũng như nhận dạng phân rã trung tâm. Đầu ra cuối cùng là```
25
25
85
65
105
```### Mẫu 2 

Mẫu thứ hai rất hữu ích vì hai cây có cấu trúc rất khác nhau. Cặp tối ưu cho mỗi đỉnh không chỉ đơn giản là một cặp liền kề trong một cây cố định. 

Các nhân chứng giảm thiểu cuối cùng có thể được kiểm tra trực tiếp từ hai cây ban đầu. 

| Đỉnh truy vấn | Nhân chứng | (d_1(i,j)) | (d_2(i,j)) | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | 8 | 8278 | 9806 | 18084 | 
| 2 | 6 | 410 | 8959 | 9369 | 
| 3 | 8 | 9078 | 504 | 9582 | 
| 4 | 7 | 15446 | 7984 | 23430 | 
| 5 | 2 | 21833 | 4861 | 26694 | 
| 6 | 2 | 410 | 8959 | 9369 | 
| 7 | 4 | 15446 | 7984 | 23430 | 
| 8 | 3 | 9078 | 504 | 9582 | 
| 9 | 3 | 22225 | 763 | 22988 | 

Ví dụ: đỉnh (3) gần nhất với đỉnh (8). Đường đi của cây đầu tiên từ (3) đến (8) có giá (4268+4810=9078), trong khi cạnh cây thứ hai có giá (504), cho ra (9582). Quá trình phân tách sẽ gặp cặp tổ tiên trung tâm tương ứng với các đường dẫn này và mức tối thiểu được lưu trữ cho cặp đó sẽ khôi phục chính xác giá trị này. 

Mẫu chính thức có đầu ra được hiển thị bên dưới.```
18084
9369
9582
23430
26694
9369
23430
9582
22988
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log^2N)) | Mỗi đỉnh có (O(\log N)) tổ tiên trong mỗi phân rã centroid và mỗi cấp độ centroid của cây thứ hai xử lý các kết hợp tổ tiên tương ứng. | 
| Không gian | (O(N\log N)) | Phân tách đầu tiên lưu trữ một khoảng cách cho mỗi đỉnh và mỗi tổ tiên trung tâm của cây đầu tiên. Hai cây ban đầu và mảng làm việc yêu cầu không gian bổ sung (O(N)). | 

Phân rã centroid có chiều cao logarit vì mỗi lần loại bỏ sẽ để lại các thành phần có kích thước tối đa bằng một nửa thành phần hiện tại. Do đó, mỗi đỉnh chỉ có (O(\log N)) tổ tiên được lưu trữ. Bảng liệt kê tổ tiên cây thứ nhất và cây thứ hai lồng nhau cung cấp thời gian chạy (O(N\log^2N)) được mô tả bởi giải pháp dự định. 

Với (N=250.000), đây là phạm vi tiệm cận được yêu cầu bởi giới hạn 12 giây và 1024 MiB của bài toán. Việc triển khai Python sử dụng các phép duyệt lặp lại vì DFS đệ quy trên một cây dài sẽ vượt quá giới hạn đệ quy của Python. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`. Trình trợ giúp thay thế đầu vào và đầu ra tiêu chuẩn, sau đó gọi giá trị thực tế`solve()`chức năng.```python
import sys
import io

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample1 = """\
5
1 2 10
2 4 20
3 4 30
4 5 50
1 2 15
1 3 25
1 4 35
1 5 25
"""

assert run(sample1) == """\
25
25
85
65
105
""", "sample 1"

sample2 = """\
9
5 7 6577
4 5 8869
5 9 9088
2 1 124
6 2 410
2 8 8154
4 8 4810
3 4 4268
3 9 763
6 2 8959
7 4 7984
3 8 504
8 6 9085
5 2 4861
1 9 8539
1 7 7834
"""

assert run(sample2) == """\
18084
9369
9582
23430
26694
9369
23430
9582
22988
""", "sample 2"

case_minimum = """\
2
1 2 1000000000
1 2 1000000000
"""

assert run(case_minimum) == """\
2000000000
2000000000
""", "minimum size and maximum edge weight"

case_equal = """\
4
1 2 1
1 3 1
1 4 1
1 2 1
1 3 1
1 4 1
"""

assert run(case_equal) == """\
2
2
2
2
""", "all equal weights"

case_different_shapes = """\
3
1 2 1
2 3 100
1 2 100
2 3 1
"""

assert run(case_different_shapes) == """\
101
101
101
""", "different tree weights"

def make_max_case():
    n = 250000
    lines = [str(n)]

    for i in range(1, n):
        lines.append(f"{i} {i + 1} 1")

    for i in range(1, n):
        lines.append(f"{i} {i + 1} 1")

    return "\n".join(lines) + "\n"

maximum_case = make_max_case()
assert run(maximum_case) == "2\n" * 250000, "maximum size stress case"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`25 25 85 65 105`| Xử lý cặp trung tâm cơ bản và tự loại trừ | 
| Mẫu 2 |`18084 9369 9582 23430 26694 9369 23430 9582 22988`| Cấu trúc cây khác nhau và đường dẫn có trọng số | 
| (N=2), trọng lượng (10^9) |`2000000000 2000000000`| Kích thước tối thiểu, số học số nguyên lớn và loại trừ bắt buộc self | 
| (N=4), các sao đơn vị giống hệt nhau |`2 2 2 2`| Cân bằng và nhiều thí sinh gần nhất bị ràng buộc | 
| (N=3), trọng lượng trái ngược nhau |`101 101 101`| Cả hai số liệu cây đều phải đóng góp | 
| (N=250000), đường dẫn đơn vị giống hệt nhau |`2`lặp đi lặp lại 250000 lần | Kích thước đầu vào tối đa và an toàn di chuyển lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp (N=2) chỉ có một cặp hợp lệ. Vì```
2
1 2 1000000000
1 2 1000000000
```phân tách trọng tâm đầu tiên lưu trữ hai đỉnh ở khoảng cách (0) và (10^9), và phân tách thứ hai cũng thực hiện tương tự. Khi xử lý một trong hai đỉnh, mức tối thiểu đầu tiên thuộc về chính đỉnh đó, do đó thuật toán chọn mức tối thiểu thứ hai. Giá trị kết quả là (10^9+10^9=2.000.000.000). 

Vấn đề tự ghép đặc biệt rõ ràng khi một cây con centroid chỉ chứa một đỉnh cho một cặp tổ tiên cụ thể. Cấu trúc hai cực tiểu khi đó chỉ có một giá trị hữu hạn. Việc thực hiện kiểm tra`INF`trước khi cập nhật câu trả lời, vì vậy đỉnh thứ hai không tồn tại không thể trở thành ứng cử viên giả. 

Khoảng cách bằng nhau cũng được xử lý chính xác. Trong ngôi sao đơn vị bốn đỉnh, một số đỉnh khác có khoảng cách kết hợp hoàn toàn giống nhau. Cấu trúc hai tối thiểu lưu trữ ID đỉnh cùng với các giá trị, do đó, các giá trị được gắn từ các đỉnh khác nhau vẫn là các ứng cử viên riêng biệt. Với mỗi đỉnh, ít nhất một đỉnh khác có giá trị (2), cho ra bốn đáp án bằng (2). 

Trọng số cạnh lớn không ảnh hưởng đến sự phân hủy. Việc lựa chọn tâm chỉ phụ thuộc vào kích thước thành phần, trong khi trọng số thực tế của cạnh được giữ không thay đổi khi tính toán khoảng cách từ tâm. Sự tách biệt này rất hữu ích vì việc phân tách cấu trúc và tính toán số liệu không ảnh hưởng lẫn nhau. 

Đường dẫn có (250.000) đỉnh là hình dạng tồi tệ nhất đối với DFS đệ quy trong Python, vì độ sâu thông thường của nó là (250.000). Việc triển khai sử dụng các ngăn xếp rõ ràng cho các tác vụ truyền tải thành phần, truyền tải khoảng cách trung tâm và phân tách, do đó, ngăn xếp cuộc gọi Python của nó vẫn có độ sâu không đổi ngay cả trên hình dạng đối nghịch này. 

Hai cây cũng có thể có cấu trúc liên kết hoàn toàn không liên quan. Thuật toán không bao giờ giả định rằng trọng tâm trên cây này tương ứng với trọng tâm có cùng nhãn ở cây kia. Nó chỉ sử dụng nhãn để xác định cùng một điểm trên hai cây, trong khi (L_1) và (L_2) được chọn độc lập. Tính độc lập này là điều làm cho phương pháp này hoạt động được với các cặp cây có trọng số tùy ý.
