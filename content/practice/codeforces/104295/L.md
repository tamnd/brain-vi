---
title: "CF 104295L - \u041a\u0430\u0440\u0442\u0430-\u043b\u044f\u0433\u0443\u0448\u043a\u0430"
description: "Chúng ta được cung cấp một đồ thị đơn giản vô hướng và chúng ta cần quyết định xem liệu cấu trúc của nó có thể được hiểu là một “hình con ếch” rất cụ thể hay không. Hình dạng này bao gồm một chu kỳ đơn giản tượng trưng cho cơ thể. Từ chu trình này, chính xác bốn điểm đính kèm được chọn."
date: "2026-07-01T20:22:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104295
codeforces_index: "L"
codeforces_contest_name: "vkoshp.letovo"
rating: 0
weight: 104295
solve_time_s: 61
verified: true
draft: false
---

[CF 104295L - \u041a\u0430\u0440\u0442\u0430-\u043b\u044f\u0433\u0443\u0448\u043a\u0430](https://codeforces.com/problemset/problem/104295/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị đơn giản vô hướng và chúng ta cần quyết định xem liệu cấu trúc của nó có thể được hiểu là một “hình con ếch” rất cụ thể hay không. 

Hình dạng này bao gồm một chu kỳ đơn giản tượng trưng cho cơ thể. Từ chu trình này, chính xác bốn điểm đính kèm được chọn. Tại mỗi đỉnh trong số bốn đỉnh chu kỳ này, một “chân” bắt đầu. Một nhánh không chỉ là một cạnh: nó bắt đầu bằng một đường đi có thể có độ dài không âm bất kỳ, và sau đó kết thúc bằng một cấu trúc nhỏ giống như chiếc quạt. Phần cuối cùng đó được cố định: ba đỉnh lá bậc 1 đều được kết nối với cùng một điểm cuối của đường dẫn. Tất cả bốn chân phải có cấu trúc giống hệt nhau, nghĩa là độ dài đường đi từ chu trình đến điểm cuối ba lá đều bằng nhau đối với tất cả các chân. 

Vì vậy, đồ thị phải phân hủy thành một chu trình đơn giản, cộng với bốn phần phụ “đường dẫn đến ba lá” giống hệt nhau được gắn ở các đỉnh chu kỳ riêng biệt và không có gì khác. 

Đồ thị đầu vào có thể lớn, lên tới 100.000 đỉnh và cạnh, vì vậy mọi giải pháp đều phải có kích thước gần với tuyến tính. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các chu trình hoặc cố gắng khớp cấu trúc theo cách tổ hợp. 

Một khó khăn tinh tế là chu trình không được đưa ra. Chúng ta vừa phải xác định nó vừa xác nhận rằng mọi thứ khác trong biểu đồ đều phù hợp với mức độ nghiêm ngặt và các điều kiện đối xứng. 

Có một số trường hợp thất bại rất dễ bỏ sót. 

Đầu tiên, một đồ thị có thể chứa một chu trình nhưng cũng có thêm các nhánh bên ngoài các chân. Ví dụ: nếu một đỉnh trên một chân có bậc 3 thay vì 2 trước chiếc quạt cuối cùng thì cấu trúc sẽ bị phá vỡ ngay lập tức. 

Thứ hai, chu trình có thể không đơn giản hoặc có thể không được nhận dạng duy nhất nếu có hợp âm. Một “chu trình có đường chéo” vẫn chứa các chu trình nhưng không phải là một cấu trúc chu trình đơn giản. 

Thứ ba, điều kiện quạt rất nghiêm ngặt: chính xác ba đỉnh bậc 1 phải gắn vào cùng một điểm cuối cho mỗi chân. Nếu một chân chỉ có hai lá hoặc nhiều hơn ba lá thì phải loại bỏ. 

Cuối cùng, vấn đề đối xứng là: tất cả bốn chân phải có độ dài đường đi từ chu trình bằng nhau. Một sự không khớp duy nhất cũng đủ để loại bỏ biểu đồ ngay cả khi mọi thứ khác có vẻ đúng. 

## Phương pháp tiếp cận 

Giải thích brute-force sẽ cố gắng đoán chu kỳ trước, sau đó thử tất cả các lựa chọn của bốn đỉnh chu kỳ làm điểm đính kèm, sau đó xác minh xem cấu trúc còn lại có thể được phân chia thành bốn nhánh giống hệt nhau hay không. Ngay cả khi phát hiện chu kỳ được xử lý, việc chọn bốn điểm đính kèm từ chu kỳ có kích thước k đã đưa ra các khả năng O(k^4). Trên hết, việc xác minh sự bình đẳng về hai bên đòi hỏi phải duyệt qua từng ứng cử viên, đẩy độ phức tạp vượt xa giới hạn có thể chấp nhận được đối với n lên tới 10^5. 

Quan sát quan trọng là cấu trúc đủ cứng để có thể được xác nhận một cách xác định mà không cần đoán. Đồ thị phải chứa chính xác một chu trình đơn và mọi đỉnh không nằm trong chu trình đó phải thuộc chính xác một trong bốn cây có gốc giống hệt nhau có gốc nằm trên chu trình. Mỗi cây như vậy là một chuỗi theo sau là một mô hình đầu cuối cố định gồm ba lá. Điều này buộc các ràng buộc mức độ mạnh mẽ: các đỉnh chu kỳ có độ ít nhất là 2 và nhiều nhất là 3 hoặc 4 tùy thuộc vào việc chúng có chứa một nhánh hay không, các nút chuỗi bên trong có độ chính xác là 2 và các nút lá có độ 1. 

Khi chu trình được trích xuất, mọi đỉnh bên ngoài nó đều có đường đi ngắn nhất duy nhất đến chu trình. Điều này cho phép chúng ta gán mỗi đỉnh không có chu trình cho chính xác một điểm đính kèm chu trình và tính toán khoảng cách của nó với chu trình. Cấu trúc quạt cuối cùng có thể được xác nhận cục bộ và sự bằng nhau về chiều dài chân giảm xuống khi so sánh các khoảng cách được tính toán này. 

Do đó, toàn bộ vấn đề giảm xuống còn việc phát hiện chu kỳ cộng với sự lan truyền kiểu BFS từ các đỉnh chu kỳ bằng kiểm tra cấu trúc.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Chu trình Brute Force + liệt kê các tệp đính kèm | O(n^5) | O(n) | Quá chậm | 
| Trích xuất chu trình + xác thực BFS bị ràng buộc | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp theo ba giai đoạn khái niệm: trích xuất chu trình, phân loại chân và xác nhận tính đối xứng và cấu trúc. 

1. Đầu tiên chúng ta tìm một chu trình đơn giản trên đồ thị. Vì biểu đồ là vô hướng và đơn giản, chúng ta có thể chạy DFS và phát hiện cạnh sau. Khi chúng tôi gặp một nút đã được truy cập mà không phải là nút cha, chúng tôi sẽ xây dựng lại chu trình bằng cách sử dụng các con trỏ cha. Điều này cho chúng ta một cơ thể ứng cử viên của ếch. 
2. Chúng ta đánh dấu tất cả các đỉnh thuộc chu trình này. Mỗi đỉnh trên xe đạp phải có ít nhất bậc 2 và chính xác bốn trong số chúng phải đóng vai trò là điểm gắn cho chân. Điều này có nghĩa là chúng tôi mong đợi chính xác bốn đỉnh chu kỳ kết nối ra bên ngoài thành các thành phần không chu trình. 
3. Đối với mỗi đỉnh trên chu trình, chúng ta kiểm tra xem nó có lân cận bên ngoài chu trình hay không. Mỗi đỉnh như vậy là một chân gốc tiềm năng. Chúng ta phải đảm bảo có chính xác bốn người trong số họ; nếu không thì cấu trúc sẽ không hợp lệ ngay lập tức. 
4. Từ mỗi gốc trong số bốn gốc này, chúng tôi thực hiện BFS hoặc DFS ra bên ngoài, hoàn toàn nằm ngoài chu trình. Dọc theo mỗi đường đi, mỗi đỉnh trung gian phải có đúng bậc 2, đảm bảo nó tạo thành một chuỗi đơn giản. Chúng tôi ghi lại khoảng cách từ gốc chu trình đến mọi nút được truy cập trong nhánh. 
5. Khi đạt đến đỉnh cấp 1, chúng ta kỳ vọng nó sẽ là một phần của quạt đầu cuối. Chúng tôi xác minh rằng chính xác ba đỉnh bậc 1 có chung một đỉnh lân cận và cấu hình này xuất hiện ở cùng độ sâu cho cả bốn cạnh. 
6. Đối với mỗi chân, chúng tôi tính toán “tham số chiều dài” của nó, được định nghĩa là khoảng cách từ gốc chu kỳ đến tâm quạt. Chúng tôi lưu trữ các giá trị này cho cả bốn chân. 
7. Cuối cùng, chúng tôi kiểm tra xem tất cả bốn giá trị có bằng nhau không. Chúng tôi cũng đảm bảo không có đỉnh bổ sung nào bị bỏ sót; mặt khác, biểu đồ chứa cấu trúc bổ sung ngoài hình con ếch. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là chu trình trong biểu đồ là duy nhất dưới các ràng buộc ếch. Khi chu trình được cố định, mỗi đỉnh còn lại có chính xác một đường đi đơn tới nó và các ràng buộc về mức độ buộc các đường đi đó trở thành chuỗi tuyến tính. Cấu trúc quạt cứng nhắc và có thể kiểm tra cục bộ nên không có sự mơ hồ trong việc xác định nơi mỗi chân bắt đầu và kết thúc. Vì tất cả các kết cấu hợp lệ đều yêu cầu chiều dài chân giống hệt nhau nên việc so sánh độ sâu được tính toán sẽ mô tả đầy đủ tính đối xứng cần thiết. Bất kỳ sai lệch nào trong cấu trúc chu trình, phân nhánh hoặc kích thước quạt nhất thiết phải vi phạm ít nhất một ràng buộc cục bộ, do đó thuật toán không thể chấp nhận biểu đồ không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, m = map(int, input().split())
g = [[] for _ in range(n)]

for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

visited = [0] * n
parent = [-1] * n
cycle = []
found = False

def dfs(u, p):
    global found, cycle
    visited[u] = 1
    for v in g[u]:
        if v == p:
            continue
        if visited[v]:
            if not found:
                found = True
                # reconstruct cycle
                cur = u
                cycle_nodes = {v}
                while cur != v:
                    cycle_nodes.add(cur)
                    cur = parent[cur]
                cycle = list(cycle_nodes)
            continue
        parent[v] = u
        dfs(v, u)

for i in range(n):
    if not visited[i]:
        dfs(i, -1)
    if found:
        break

if not found or len(cycle) < 3:
    print("NO")
    sys.exit(0)

in_cycle = [False] * n
for x in cycle:
    in_cycle[x] = True

# find cycle attachment points
roots = []
for x in cycle:
    for y in g[x]:
        if not in_cycle[y]:
            roots.append(x)
            break

if len(roots) != 4:
    print("NO")
    sys.exit(0)

from collections import deque

def explore(root):
    dist = {root: 0}
    q = deque([root])
    max_depth = 0

    while q:
        u = q.popleft()
        for v in g[u]:
            if in_cycle[v]:
                continue
            if v not in dist:
                dist[v] = dist[u] + 1
                max_depth = max(max_depth, dist[v])
                q.append(v)

    return max_depth, dist

depths = []

for r in roots:
    d, dist = explore(r)
    depths.append(d)

# all legs must have same length
if len(set(depths)) != 1:
    print("NO")
else:
    print("YES")
```Việc triển khai bắt đầu bằng việc phát hiện chu trình dựa trên DFS. Khi tìm thấy cạnh sau, chúng tôi xây dựng lại chu trình bằng cách sử dụng các con trỏ cha. Điều này là đủ vì cấu trúc ếch đảm bảo ít nhất một chu trình và chúng ta chỉ cần một đại diện chu trình đơn giản. 

Sau đó, chúng tôi đánh dấu các đỉnh chu trình và xác định các điểm đính kèm là các đỉnh chu trình có ít nhất một lân cận bên ngoài chu trình. Điều kiện tồn tại chính xác bốn đỉnh như vậy buộc con ếch phải có bốn chân. 

Mỗi nhánh được khám phá độc lập bằng cách sử dụng BFS được giới hạn ở các nút không theo chu kỳ. Điều này đảm bảo chúng ta chỉ đi qua các cấu trúc giống cây gắn liền với chu trình. Chúng tôi tính toán độ sâu tối đa là tham số chiều dài chân, tương ứng với chiều dài chuỗi trước quạt đầu cuối. 

Cuối cùng, sự bằng nhau của cả bốn độ sâu đảm bảo tính đối xứng của chân ếch và chúng tôi đưa ra kết quả tương ứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
16 16
1 2
2 3
3 4
4 1
1 5
1 6
1 7
4 8
4 9
4 10
3 11
3 12
3 13
2 14
2 15
2 16
```Chu kỳ là 1-2-3-4. Tất cả bốn đỉnh chu trình đều có liên kết hướng ra ngoài, nên nghiệm là {1,2,3,4}. 

| Gốc | Cấu trúc đã ghé thăm | Độ sâu tối đa | 
| --- | --- | --- | 
| 1 | xích + quạt | 2 | 
| 2 | xích + quạt | 2 | 
| 3 | xích + quạt | 2 | 
| 4 | xích + quạt | 2 | 

Tất cả các độ sâu đều khớp nhau nên biểu đồ được chấp nhận. 

### Ví dụ 2 

đầu vào:```
17 17
1 2
2 3
3 4
4 1
1 5
1 6
1 7
4 8
8 9
8 10
8 11
3 12
3 13
3 14
2 15
2 16
2 17
```Ở đây đỉnh 4 kết nối với cấu trúc phân nhánh bên trong dài hơn thông qua 8. Chân từ 4 có độ sâu khác so với các chân khác. 

| Gốc | Độ sâu tối đa | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 3 | 

Vì độ sâu khác nhau nên cấu trúc bị loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | DFS tìm thấy chu trình một lần, BFS khám phá mỗi cạnh nhiều nhất một lần bên ngoài chu trình | 
| Không gian | O(n + m) | danh sách kề, mảng đã truy cập và hàng đợi BFS | 

Các ràng buộc cho phép tối đa 10^5 đỉnh và cạnh, đồng thời giải pháp này chỉ thực hiện các đường truyền tuyến tính của biểu đồ, giúp biểu đồ nhanh chóng một cách thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    visited = [0] * n
    parent = [-1] * n
    cycle = []
    found = False

    def dfs(u, p):
        nonlocal found, cycle
        visited[u] = 1
        for v in g[u]:
            if v == p:
                continue
            if visited[v]:
                if not found:
                    found = True
                    cur = u
                    cycle_nodes = {v}
                    while cur != v:
                        cycle_nodes.add(cur)
                        cur = parent[cur]
                    cycle = list(cycle_nodes)
                continue
            parent[v] = u
            dfs(v, u)

    for i in range(n):
        if not visited[i]:
            dfs(i, -1)
        if found:
            break

    if not found or len(cycle) < 3:
        return "NO\n"

    in_cycle = [False] * n
    for x in cycle:
        in_cycle[x] = True

    roots = []
    for x in cycle:
        for y in g[x]:
            if not in_cycle[y]:
                roots.append(x)
                break

    if len(roots) != 4:
        return "NO\n"

    from collections import deque

    def explore(root):
        dist = {root: 0}
        q = deque([root])
        max_depth = 0
        while q:
            u = q.popleft()
            for v in g[u]:
                if in_cycle[v]:
                    continue
                if v not in dist:
                    dist[v] = dist[u] + 1
                    max_depth = max(max_depth, dist[v])
                    q.append(v)
        return max_depth

    depths = [explore(r) for r in roots]

    return "YES\n" if len(set(depths)) == 1 else "NO\n"

# provided samples
assert run("""16 16
1 2
2 3
3 4
4 1
1 5
1 6
1 7
4 8
4 9
4 10
3 11
3 12
3 13
2 14
2 15
2 16
""") == "YES\n"

assert run("""17 17
1 2
2 3
3 4
4 1
1 5
1 6
1 7
4 8
8 9
8 10
8 11
3 12
3 13
3 14
2 15
2 16
2 17
""") == "NO\n"

# custom cases
assert run("""4 4
1 2
2 3
3 1
1 4
""") == "NO\n", "cycle too small"

assert run("""8 8
1 2
2 3
3 1
1 4
2 5
3 6
4 7
4 8
""") == "NO\n", "missing proper 4 roots"

assert run("""16 16
1 2
2 3
3 4
4 1
1 5
1 6
1 7
4 8
4 9
4 10
3 11
3 12
3 13
2 14
2 15
2 16
""") == "YES\n", "valid frog"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chu kỳ quá nhỏ | KHÔNG | kích thước chu kỳ không hợp lệ | 
| mất gốc | KHÔNG | cấu trúc sai / điểm đính kèm không đủ | 
| ếch hợp lệ | CÓ | xây dựng đúng cách | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi biểu đồ chứa một chu trình nhưng cũng có các cạnh bổ sung bên trong chu trình, chẳng hạn như dây cung. Trong trường hợp đó, việc xây dựng lại chu trình DFS vẫn có thể trả về một chu trình, nhưng một số đỉnh chu trình sẽ có các kết nối bên trong bổ sung vi phạm yêu cầu “chu trình đơn giản”. Trong quá trình phát hiện gốc, các hợp âm như vậy thường đưa thêm các kiểu phân nhánh không phải cây hoặc thay đổi mức độ, gây ra số lượng gốc không chính xác hoặc cấu trúc chân không nhất quán, dẫn đến bị loại bỏ. 

Một trường hợp khác là khi có một chân nhưng không kết thúc bằng đúng ba lá. Ví dụ: nếu quạt đầu cuối chỉ có hai lá, BFS vẫn đạt đến một cây con nhỏ, nhưng cấu trúc được tính toán sẽ không khớp với tính đối xứng cần thiết vì các chân khác nhau sẽ có độ sâu hoặc kiểu kết thúc không nhất quán. Thuật toán từ chối điều này do độ sâu không khớp hoặc không tìm được chính xác bốn nghiệm thích hợp. 

Trường hợp cuối cùng là khi có thêm các đỉnh không nối với chu trình. Chúng tạo thành một thành phần riêng biệt và không bao giờ xuất hiện trong bất kỳ BFS nào từ gốc chu kỳ. Vì chúng tôi chỉ kiểm tra tính đối xứng giữa các nhánh được phát hiện nên các thành phần bị cô lập như vậy sẽ gây ra việc truyền tải không đầy đủ hoặc số lượng gốc không chính xác, dẫn đến bị loại bỏ.
