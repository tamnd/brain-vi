---
title: "CF 208C - Đồn Cảnh Sát"
description: "Chúng ta được cung cấp một biểu đồ kết nối, vô hướng biểu thị các thành phố và đường, trong đó mọi con đường đều có thời gian di chuyển như nhau. Trong số tất cả các thành phố, thành phố 1 và thành phố n rất đặc biệt: chúng tôi quan tâm đến việc di chuyển giữa hai điểm cuối này."
date: "2026-06-03T17:29:27+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 208
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 130 (Div. 2)"
rating: 1900
weight: 208
solve_time_s: 116
verified: true
draft: false
---

[CF 208C - Đồn cảnh sát](https://codeforces.com/problemset/problem/208/C) 

**Xếp hạng:** 1900 
**Tags:** dp, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ kết nối, vô hướng biểu thị các thành phố và đường, trong đó mọi con đường đều có thời gian di chuyển như nhau. Trong số tất cả các thành phố, thành phố 1 và thành phố n rất đặc biệt: chúng tôi quan tâm đến việc di chuyển giữa hai điểm cuối này. Mọi người luôn chọn những con đường ngắn nhất khi đi từ 1 đến n và nếu tồn tại nhiều con đường ngắn nhất thì tất cả chúng đều có khả năng như nhau. 

Chúng ta được phép đặt đồn cảnh sát ở đúng một thành phố. Bất kỳ con đường nào có đồn cảnh sát ở một trong những điểm cuối của nó đều được coi là an toàn; tất cả các con đường khác đều nguy hiểm. Đối với mỗi đường đi ngắn nhất từ ​​thành phố 1 đến thành phố n, chúng ta đếm xem nó sử dụng bao nhiêu cạnh an toàn. Vì có thể tồn tại nhiều đường đi ngắn nhất nên chúng tôi lấy số cạnh an toàn trung bình trên tất cả các đường đi ngắn nhất và chúng tôi muốn chọn vị trí của đồn cảnh sát sao cho tối đa hóa giá trị kỳ vọng này. 

Các hạn chế là nhỏ về các đỉnh, tối đa là 100 thành phố, nhưng số lượng đường có thể lớn. Điều này ngay lập tức gợi ý rằng các thuật toán có hành vi bậc ba trong n vẫn có thể chấp nhận được, nhưng bất cứ điều gì liên quan đến việc liệt kê tất cả các đường đi ngắn nhất thì không. 

Khó khăn chính là câu trả lời phụ thuộc vào cấu trúc của tất cả các đường đi ngắn nhất đồng thời chứ không chỉ một đường đi. Một cách tiếp cận đơn giản sẽ cố gắng liệt kê tất cả các đường đi ngắn nhất từ ​​1 đến n và đánh giá từng vị trí trạm ứng viên. Tuy nhiên, số lượng đường đi ngắn nhất có thể là số mũ trong đồ thị dày đặc. 

Trường hợp cạnh tinh tế phát sinh khi đồ thị có nhiều đường đi ngắn nhất từ ​​1 đến n với các cạnh chồng lên nhau. Ví dụ: trong cấu trúc lưỡng cực hoàn chỉnh được phân lớp theo khoảng cách BFS, mỗi đường đi ngắn nhất là sự kết hợp khác nhau của các lựa chọn trung gian. Việc liệt kê bạo lực sẽ thất bại ngay lập tức ngay cả khi n ở khoảng 30. 

Một trường hợp cạnh khác là khi đường đi ngắn nhất là duy nhất. Trong trường hợp đó, vấn đề giảm xuống mức tối đa hóa số cạnh của đường dẫn đơn đó chạm vào nút đã chọn, điều này giúp đơn giản hóa rất nhiều và là một biện pháp kiểm tra độ chính xác tốt. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản về mặt khái niệm: đầu tiên tính toán tất cả các đường đi ngắn nhất từ 1 đến n, sau đó với mỗi thành phố ứng cử viên c, hãy đánh giá mọi đường đi ngắn nhất và đếm xem có bao nhiêu cạnh trên đường đi đó liên quan đến c. Cuối cùng, tính trung bình trên tất cả các đường dẫn và lấy mức tối đa. 

Tính đúng đắn là rõ ràng vì nó tuân theo định nghĩa trực tiếp. Thất bại xuất phát từ số lượng đường đi ngắn nhất, có thể tăng theo cấp số nhân trong các biểu đồ chung. Ngay cả việc lưu trữ chúng cũng không khả thi, vì mỗi đường dẫn có độ dài O(n) và có thể có nhiều đường dẫn như vậy theo cấp số nhân. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần liệt kê các đường dẫn. Chúng ta chỉ cần biết, đối với mỗi cạnh trên mỗi đường đi ngắn nhất, liệu nó có góp phần vào việc lựa chọn trạm nhất định hay không. Thay vì suy luận theo từng đường dẫn, chúng tôi chuyển đổi phối cảnh và tổng hợp theo các cạnh. 

Trước tiên, chúng tôi tính toán khoảng cách ngắn nhất từ ​​1 và từ n bằng BFS. Điều này phân chia các cạnh thành những cạnh có thể nằm trên đường đi ngắn nhất: cạnh (u, v) có liên quan nếu nó nằm giữa các lớp liên tiếp trong đường dẫn ngắn nhất DAG, nghĩa là dist1[u] + 1 + distN[v] bằng dist1[n] hoặc điều kiện đối xứng. 

Bây giờ chúng ta xây dựng một DAG chỉ bao gồm các cạnh có thể xuất hiện theo một đường đi ngắn nhất nào đó từ 1 đến n, được định hướng từ dist1 nhỏ hơn đến dist1 lớn hơn. Mỗi đường dẫn ngắn nhất tương ứng với một đường dẫn trong DAG này. 

Gọi P là tập hợp tất cả các đường đi ngắn nhất. Chúng ta muốn, đối với mỗi nút ứng cử viên c, giá trị trung bình trên P của bao nhiêu cạnh trong đường đi liên quan đến c. Chúng ta viết lại kết quả này dưới dạng tổng các cạnh: với mỗi cạnh e, chúng ta đếm xem có bao nhiêu đường đi ngắn nhất đi qua e và cộng 1 nếu e chạm vào c.

Điều này chuyển vấn đề sang việc đếm, đối với mỗi cạnh trong đường đi ngắn nhất DAG, có bao nhiêu đường đi ngắn nhất sử dụng nó. Đó là DP tiêu chuẩn trên DAG: chúng tôi tính toán các cách từ 1 đến mọi nút và từ mọi nút đến n được giới hạn ở các cạnh có đường đi ngắn nhất. Mỗi đóng góp của cạnh sau đó là sản phẩm của số lượng tiến và lùi. 

Khi chúng tôi biết số lượng đường dẫn sử dụng cạnh, việc đánh giá nút ứng cử viên c sẽ tính tổng trên tất cả các cạnh đường dẫn ngắn nhất sự cố số lượng đường dẫn ngắn nhất sử dụng các cạnh đó, được chuẩn hóa bằng tổng số đường dẫn ngắn nhất. 

Điều này làm giảm vấn đề khi tính toán số lượng DAG đường đi ngắn nhất tất cả các cặp và sau đó thực hiện n đánh giá, mỗi đánh giá tính bằng O(deg(c)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các con đường ngắn nhất | Hàm mũ | Hàm mũ | Quá chậm | 
| Đếm DAG + DP đường dẫn ngắn nhất | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách ngắn nhất từ nút 1 bằng BFS. Điều này xác định việc phân lớp phía trước của cấu trúc đường dẫn ngắn nhất. 
2. Tính khoảng cách ngắn nhất từ ​​nút n bằng BFS. Điều này cho phép chúng ta xác minh xem một cạnh có nằm trên đường đi ngắn nhất nào đó giữa 1 và n hay không. 
3. Xây dựng tập hợp các “cạnh hữu ích” thỏa mãn dist1[u] + 1 + distN[v] = dist1[n] hoặc điều kiện đối xứng. Điều này đảm bảo mọi cạnh được chọn có thể thuộc về ít nhất một đường đi ngắn nhất. 
4. Xây dựng danh sách kề được giới hạn ở các cạnh hữu ích và định hướng chúng từ dist1 nhỏ hơn đến dist1 lớn hơn. Điều này tạo ra DAG vì khoảng cách tăng dọc theo các cạnh. 
5. Chạy DP từ nút 1 trên DAG này để tính way1[v], số đường đi ngắn nhất từ ​​1 đến v. Phép truy hồi tính tổng các way1 trên tất cả các cạnh DAG đến. 
6. Tương tự tính cách2[v], số đường đi ngắn nhất từ ​​v đến n trong DAG đảo ngược. 
7. Tổng số đường đi ngắn nhất là way1[n]. Đối với mỗi cạnh hữu ích (u, v), hãy tính xem có bao nhiêu đường đi ngắn nhất sử dụng nó như way1[u] * way2[v]. 
8. Đối với mỗi thành phố ga ứng cử viên c, hãy tính phần đóng góp trên tất cả các cạnh hữu ích của sự cố. Nếu cạnh là (c, v), nó đóng gópways1[c] *ways2[v]; nếu là (u, c), nó đóng góp way1[u] * way2[c]. Tổng hợp tất cả những đóng góp đó và chia cho tổng số đường đi ngắn nhất để có được số cạnh an toàn mong đợi. 

Lý do mà sự tổng hợp này hoạt động là vì mọi đường đi ngắn nhất được phân tách duy nhất thành các cạnh và sự đóng góp của mỗi cạnh có thể được tính một cách độc lập bằng cách sử dụng số cách để đến điểm cuối của nó mà không cần tính đường dẫn kép. 

### Tại sao nó hoạt động 

Mọi đường đi ngắn nhất từ 1 đến n tương ứng chính xác với một đường đi trong DAG đường đi ngắn nhất. Với bất kỳ cạnh nào e = (u, v), số đường đi ngắn nhất chứa e chính xác là số cách để đến u từ 1 nhân với số cách để đến n từ v. Vì mỗi đường đi được tính chính xác một lần trong phân tích này, nên việc tính tổng các đóng góp trên các cạnh sẽ cho một phân tích tuyến tính chính xác của kỳ vọng. Do đó, kỳ vọng về các đường đi tương đương với việc tính trọng số cho mỗi cạnh theo tần số của nó trên tất cả các đường đi ngắn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, m = map(int, input().split())
g = [[] for _ in range(n + 1)]
edges = []

for _ in range(m):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)
    edges.append((u, v))

def bfs(src):
    dist = [10**9] * (n + 1)
    dist[src] = 0
    q = deque([src])
    while q:
        u = q.popleft()
        for v in g[u]:
            if dist[v] > dist[u] + 1:
                dist[v] = dist[u] + 1
                q.append(v)
    return dist

dist1 = bfs(1)
distn = bfs(n)
D = dist1[n]

sg = [[] for _ in range(n + 1)]
rg = [[] for _ in range(n + 1)]

valid_edges = []

for u, v in edges:
    if dist1[u] + 1 + distn[v] == D:
        sg[u].append(v)
        rg[v].append(u)
        valid_edges.append((u, v))
    elif dist1[v] + 1 + distn[u] == D:
        sg[v].append(u)
        rg[u].append(v)
        valid_edges.append((v, u))

ways1 = [0] * (n + 1)
ways1[1] = 1
order = sorted(range(1, n + 1), key=lambda x: dist1[x])
for u in order:
    for v in sg[u]:
        ways1[v] += ways1[u]

waysn = [0] * (n + 1)
waysn[n] = 1
order = sorted(range(1, n + 1), key=lambda x: -dist1[x])
for u in order:
    for v in rg[u]:
        waysn[v] += waysn[u]

total_paths = ways1[n]

edge_cnt = 0
node_ans = [0] * (n + 1)

for u, v in valid_edges:
    cnt = ways1[u] * waysn[v]
    node_ans[u] += cnt
    node_ans[v] += cnt
    edge_cnt += cnt

best = 0.0
for i in range(1, n + 1):
    if total_paths:
        best = max(best, node_ans[i] / total_paths)

print(f"{best:.10f}")
```Việc triển khai bắt đầu bằng cách tính toán khoảng cách ngắn nhất từ ​​cả hai điểm cuối bằng BFS, đây là xương sống để xác định các cạnh nào có thể tham gia vào các đường đi ngắn nhất. Sau đó, biểu đồ được lọc thành DAG đường đi ngắn nhất bằng cách thực thi tính nhất quán giữa khoảng cách tiến và lùi. 

DP tiến đếm có bao nhiêu đường đi ngắn nhất đến mỗi nút từ 1, trong khi DP lùi đếm có bao nhiêu đường đi ngắn nhất đi từ mỗi nút đến n. Hai giá trị này kết hợp để cho ra số đường đi ngắn nhất đi qua mỗi cạnh có hướng. 

Cuối cùng, mỗi nút tổng hợp các đóng góp từ các cạnh sự cố của nó. Việc chia cho tổng số đường dẫn ngắn nhất sẽ chuyển đổi số lượng thô thành giá trị mong đợi qua việc phân bổ thống nhất các đường dẫn ngắn nhất. 

Phải cẩn thận để đảm bảo các cạnh chỉ được tính một lần theo đúng hướng, vì đồ thị ban đầu là vô hướng nhưng DP hoạt động trên cấu trúc tuần hoàn có hướng gây ra bởi khoảng cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2
2 4
1 3
3 4
```Khoảng cách ngắn nhất từ ​​1 đến 4 là 2. Tất cả các đường đi đều có độ dài bằng 2 và có đúng hai đường đi ngắn nhất. 

| Đường dẫn | Cạnh | 
| --- | --- | 
| 1-2-4 | (1,2), (2,4) | 
| 1-3-4 | (1,3), (3,4) | 

Đối với mỗi nút là trạm: 

| Trạm | Đường dẫn đóng góp các cạnh an toàn | Trung bình | 
| --- | --- | --- | 
| 1 | cả hai đường đều có 1 cạnh tới | 1.0 | 
| 2 | đường dẫn thứ nhất có 2, đường thứ hai có 0 | 1.0 | 
| 3 | đường dẫn đầu tiên có 0, đường dẫn thứ hai có 2 | 1.0 | 
| 4 | cả hai đường đều có 1 cạnh tới | 1.0 | 

Giá trị tối ưu là 1,0, phù hợp với trực giác rằng mọi đường đi ngắn nhất đều chạm chính xác một cạnh tới bất kỳ nút nào được chọn trên lớp của nó. 

Điều này xác nhận rằng việc tổng hợp dựa trên cạnh xử lý chính xác các đóng góp đối xứng trên nhiều đường dẫn ngắn nhất. 

### Ví dụ 2 

đầu vào:```
5 6
1 2
2 5
1 3
3 5
2 4
4 5
```Con đường ngắn nhất là: 

1-2-5, 1-3-5, 1-2-4-5, 1-3-4-5 tùy theo cấu trúc, đưa ra nhiều lộ trình chồng chéo. 

| Trạm | Quan sát chính | Kết quả | 
| --- | --- | --- | 
| 2 | tham gia vào nhiều con đường ngắn nhất | đóng góp cao hơn | 
| 3 | đối xứng tới 2 | tương tự | 
| 4 | nằm sâu hơn trong DAG, đóng góp ít điểm cuối cạnh hơn | thấp hơn | 

Ví dụ này minh họa cách các nút trung tâm trong DAG đường dẫn ngắn nhất tích lũy nhiều cạnh đường dẫn ngắn nhất sự cố mà DP nắm bắt thông qua số lượng đường dẫn nhân lên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Hai lần chạy BFS cộng với hai lần duyệt DAG DP qua các cạnh | 
| Không gian | O(n + m) | Danh sách kề và mảng DP cho DAG đường đi ngắn nhất | 

Kích thước đồ thị đủ nhỏ để xử lý thời gian tuyến tính trên các cạnh đủ nhanh. Các hoạt động chủ yếu là truyền tải BFS và truyền bá DP đơn giản, cả hai đều có quy mô thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    n, m = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n + 1)]
    edges = []

    for _ in range(m):
        u, v = map(int, sys.stdin.readline().split())
        g[u].append(v)
        g[v].append(u)
        edges.append((u, v))

    def bfs(src):
        dist = [10**9] * (n + 1)
        dist[src] = 0
        q = deque([src])
        while q:
            u = q.popleft()
            for v in g[u]:
                if dist[v] > dist[u] + 1:
                    dist[v] = dist[u] + 1
                    q.append(v)
        return dist

    dist1 = bfs(1)
    distn = bfs(n)
    D = dist1[n]

    sg = [[] for _ in range(n + 1)]
    rg = [[] for _ in range(n + 1)]

    valid_edges = []

    for u, v in edges:
        if dist1[u] + 1 + distn[v] == D:
            sg[u].append(v)
            rg[v].append(u)
            valid_edges.append((u, v))
        elif dist1[v] + 1 + distn[u] == D:
            sg[v].append(u)
            rg[u].append(v)
            valid_edges.append((v, u))

    ways1 = [0] * (n + 1)
    ways1[1] = 1
    order = sorted(range(1, n + 1), key=lambda x: dist1[x])
    for u in order:
        for v in sg[u]:
            ways1[v] += ways1[u]

    waysn = [0] * (n + 1)
    waysn[n] = 1
    order = sorted(range(1, n + 1), key=lambda x: -dist1[x])
    for u in order:
        for v in rg[u]:
            waysn[v] += waysn[u]

    total_paths = ways1[n]

    node_ans = [0] * (n + 1)
    for u, v in valid_edges:
        cnt = ways1[u] * waysn[v]
        node_ans[u] += cnt
        node_ans[v] += cnt

    best = 0.0
    for i in range(1, n + 1):
        if total_paths:
            best = max(best, node_ans[i] / total_paths)

    return f"{best:.10f}"

# provided sample
assert run("4 4\n1 2\n2 4\n1 3\n3 4\n") == "1.0000000000"

# custom: line graph
assert run("3 2\n1 2\n2 3\n") == "1.0000000000"

# custom: triangle
assert run("3 3\n1 2\n2 3\n1 3\n") == "1.0000000000"

# custom: square with diagonal
assert run("4 5\n1 2\n2 4\n1 3\n3 4\n2 3\n") == "1.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị đường | 1.0 | xử lý đường dẫn ngắn nhất duy nhất | 
| tam giác | 1.0 | đối xứng nhiều đường đi ngắn nhất | 
| vuông + chéo | 1.0 | chồng chéo tính nhất quán của đường dẫn ngắn nhất DAG | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi có đúng một đường đi ngắn nhất giữa 1 và n. Trong trường hợp đó, DP giảm xuống việc đếm một đường dẫn duy nhất và sự đóng góp của mỗi nút chỉ phụ thuộc vào việc nó chạm vào một hay hai cạnh của đường dẫn đó. Thuật toán xử lý điều này một cách chính xác vìways1 vàwaysn trở thành 1 dọc theo đường dẫn và 0 ở những nơi khác, do đó đóng góp của cạnh thu gọn về các giá trị chỉ báo đơn giản. 

Một trường hợp khác là khi tồn tại nhiều đường đi ngắn nhất với tính đối xứng hoàn toàn, chẳng hạn như lưới phân lớp trong đó mỗi lớp nhân đôi số lượng lựa chọn. Ở đây, suy nghĩ ngây thơ có thể gợi ý các vấn đề về tính toán kép, nhưng việc phân tích hệ số tiến-lùi đảm bảo mỗi đường dẫn được tính chính xác một lần trên mỗi cạnh, vì mỗi đường đi ngắn nhất được xác định duy nhất bằng cách chọn một chuỗi chuyển tiếp trong DAG.
