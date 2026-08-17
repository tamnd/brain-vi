---
title: "CF 102215D - Bộ phận Quốc gia"
description: "Mạng lưới đường bộ là một cái cây, vì có (n) thành phố, chính xác là (n-1) đường và mọi thành phố đều có thể đến được từ mọi thành phố khác. Trong mỗi dự đoán, một số thành phố có màu đỏ, một số có màu xanh lam và tất cả các thành phố còn lại đều không liên quan. Chúng tôi có thể đóng bất kỳ con đường nào."
date: "2026-08-17T23:34:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "D"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 222
verified: false
draft: false
---

[CF 102215D - Bộ phận quốc gia](https://codeforces.com/problemset/problem/102215/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mạng lưới đường bộ là một cái cây, vì có (n) thành phố, chính xác là (n-1) đường và mọi thành phố đều có thể đến được từ mọi thành phố khác. Trong mỗi dự đoán, một số thành phố có màu đỏ, một số có màu xanh lam và tất cả các thành phố còn lại đều không liên quan. 

Chúng tôi có thể đóng bất kỳ con đường nào. Sau khi làm như vậy, mọi thành phố đỏ vẫn phải được kết nối với mọi thành phố đỏ khác, mọi thành phố xanh vẫn phải được kết nối với mọi thành phố xanh khác và không thành phố đỏ nào được kết nối với bất kỳ thành phố xanh nào. Nhiệm vụ là quyết định xem có tồn tại một tập hợp các con đường khép kín như vậy cho mọi dự đoán hay không. Bài toán chính thức có (n,q\le 200000), với tổng của tất cả các thành phố màu đỏ và xanh được truy vấn nhiều nhất là (200000). 

Đối tượng chính là cây con tối thiểu kết nối một tập hợp các đỉnh. Đối với các thành phố màu đỏ, gọi đây là cây con Steiner đỏ. Bất kỳ giải pháp hợp lệ nào cũng phải để mở mọi cạnh của cây con này, vì nếu không thì hai thành phố màu đỏ sẽ bị ngắt kết nối. Điều tương tự cũng đúng với cây con Steiner màu xanh. Vì vậy, câu hỏi thực sự là liệu hai cây con cần thiết có thể tách rời nhau hay không. 

Giới hạn kích thước loại trừ việc xây dựng lại thông tin trên tất cả (n) thành phố cho mỗi truy vấn. Với (q=200000) và (n=200000), phương thức (O(nq)) có thể thực hiện khoảng (4\cdot10^{10}) thao tác, vượt xa giới hạn 2 giây. Phần hữu ích của các ràng buộc là tổng số thành phố được tô màu trên tất cả các truy vấn chỉ là (200000), do đó công việc truy vấn phải tỷ lệ thuận với số thành phố được đề cập, nhân với phép toán cây logarit. 

Có một số trường hợp khó xử lý. Nếu mỗi màu chỉ có một thành phố thì câu trả lời luôn là CÓ. Ví dụ,```
2
1 2
1
1 1 2
```có câu trả lời`YES`. Chúng tôi có thể giữ con đường duy nhất mở cho kết nối bên trong của mỗi màu và không bắt buộc phải mở con đường đỏ-xanh. 

Hai cây con màu cũng có thể có cùng một gốc mặc dù không có thành phố đỏ và xanh trùng nhau. Ví dụ,```
5
1 2
1 3
1 4
1 5
1
2 2 3 4 5
```có thành phố màu đỏ (2,3) và thành phố màu xanh (4,5). Cả hai cây con màu đều chứa thành phố (1), vì vậy câu trả lời là`NO`. Một giải pháp bất cẩn chỉ kiểm tra xem bản thân các thành phố có màu sắc có khác nhau hay không sẽ chấp nhận nó một cách sai lầm. 

Một trường hợp tinh tế khác xảy ra khi gốc Steiner của một màu là tổ tiên của gốc Steiner của màu kia. Coi như```
4
1 2
2 3
3 4
1
2 1 4 3
```Các thành phố màu đỏ là (1,4), vì vậy cây con Steiner của chúng là toàn bộ đường đi từ (1) đến (4). Thành phố xanh là (3). Cây con màu xanh nằm bên trong cây con màu đỏ nên đáp án là`NO`. Chỉ quan sát rằng hai nghiệm Steiner, (1) và (3), khác nhau là chưa đủ. 

Tình huống ngược lại cũng có thể xảy ra:```
4
1 2
2 3
3 4
1
2 1 2 3 4
```Ở đây, các thành phố màu đỏ là (1,2), các thành phố màu xanh là (3,4) và các cây con yêu cầu của chúng được phân tách bằng cạnh (2-3). Câu trả lời là`YES`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp có thể root cây và xử lý mọi cạnh cho mọi truy vấn. Đối với mỗi truy vấn, chúng tôi có thể xác định cạnh nào của mỗi cạnh chứa các thành phố màu đỏ và xanh lam, sau đó quyết định xem cạnh đó có phải tiếp tục mở cho một trong hai màu hay không. Điều này đúng vì việc loại bỏ một cạnh sẽ chia cây thành chính xác hai thành phần, do đó tất cả các yêu cầu kết nối có thể được thể hiện dưới dạng các vết cắt này. 

Vấn đề là khối lượng công việc. Xử lý tất cả (n-1) cạnh cho mỗi truy vấn có chi phí (O(nq)). Ở giới hạn tối đa, đây là khoảng (200000\cdot200000=4\cdot10^{10}) hoạt động cạnh, điều này gần như không khả thi. 

Quan sát giúp mở ra phương pháp nhanh hơn là chúng ta không bao giờ cần kiểm tra toàn bộ cây. Đối với một tập hợp các đỉnh, cây con kết nối tối thiểu của nó có đỉnh cao nhất duy nhất khi cây có gốc. Đỉnh đó đơn giản là LCA của tất cả các đỉnh trong tập hợp. 

Gọi (R) là LCA của tất cả các thành phố màu đỏ và (B) LCA của tất cả các thành phố màu xanh. Cây con Steiner đỏ chính xác là hợp các đường đi từ (R) tới mọi thành phố đỏ. Tương tự như vậy, cây con Steiner xanh là hợp của các đường đi từ (B) tới mọi thành phố xanh. 

Nếu cả (R) và (B) đều không phải là tổ tiên của cây kia thì các cây con gốc của chúng sẽ rời nhau, do đó hai cây con Steiner sẽ tự động tách nhau và câu trả lời là`YES`. 

Thay vào đó, giả sử rằng (R) là tổ tiên của (B). Toàn bộ cây con Steiner màu xanh được chứa trong cây con có gốc tại (B). Cây con Steiner đỏ đạt đến cây con đó chính xác khi có thành phố đỏ nào đó nằm bên trong cây con có gốc tại (B). Nếu một thành phố màu đỏ như vậy tồn tại, đường đi của nó từ (R) đến thành phố đó đi qua (B), trong khi cây con màu xanh cũng chứa (B), do đó hai cây con bắt buộc giao nhau. Câu trả lời là`NO`. Nếu không có thành phố màu đỏ nào nằm ở đó thì hai cây con sẽ rời nhau và câu trả lời là`YES`. 

Trường hợp (B) là tổ tiên của (R) là đối xứng. 

Do đó, mỗi truy vấn chỉ cần tính toán LCA lặp đi lặp lại, sau đó là kiểm tra tổ tiên. Chúng tôi xử lý trước cây cho các truy vấn LCA (O(\log n)) bằng cách sử dụng phân tách nặng-nhẹ. Vì tổng số thành phố được đề cập nhiều nhất là (200000), nên tổng công việc truy vấn vẫn nằm trong giới hạn dự kiến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nq)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n + S\log n)), trong đó (S\le200000) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Rễ cây ở thành phố (1). Trong một DFS lặp lại, hãy tính`parent`,`depth`và thời gian vào và ra Euler`tin`Và`tout`. Khoảng ([tin[v],tout[v])) biểu thị chính xác cây con của (v), do đó việc kiểm tra tổ tiên sau đó có thể được trả lời trong (O(1)). 
2. Tính kích thước cây con và chọn một con nặng cho mỗi đỉnh. Đứa trẻ nặng nhất là đứa trẻ có cây con lớn nhất. Theo sau các phần tử con nặng tạo ra các chuỗi trong đó số lần truy vấn LCA thay đổi chuỗi là (O(\log n)). 
3. Gán mỗi đỉnh vào đầu chuỗi nặng-nhẹ của nó. Phân tách kết quả cho phép chúng ta tính toán LCA của hai đỉnh bằng cách di chuyển liên tục đỉnh có đầu chuỗi sâu hơn so với đỉnh đầu chuỗi của nó. 
4. Đối với mỗi dự đoán, hãy lưu trữ các đỉnh màu đỏ và tính LCA chung của chúng bằng cách gấp các phép toán LCA từ trái sang phải. Bắt đầu với đỉnh màu đỏ đầu tiên là LCA hiện tại và thay thế nó bằng`lca(current, next)`cho mỗi đỉnh đỏ bổ sung. Đỉnh kết quả (R) là đỉnh cao nhất thuộc cây con Steiner đỏ. 
5. Thực hiện tương tự với các đỉnh màu xanh lam và thu được (B). Vì mỗi truy vấn chứa ít nhất một đỉnh của mỗi màu nên cả hai LCA luôn được xác định. 
6. Kiểm tra xem (R) và (B) có thể so sánh được trong cây có gốc hay không. Nếu không phải là tổ tiên của cây kia thì cây con của chúng sẽ rời rạc, do đó kết quả đầu ra`YES`. 
7. Nếu (R) là tổ tiên của (B), hãy quét các đỉnh màu đỏ và kiểm tra xem có bất kỳ đỉnh nào trong số chúng nằm trong cây con của (B) hay không. Nếu đúng như vậy, cây con Steiner màu đỏ phải đi qua (B), trong đó cây con Steiner màu xanh cũng tồn tại, do đó đầu ra`NO`. Nếu không thì xuất ra`YES`. 
8. Nếu (B) là tổ tiên của (R), hãy thực hiện kiểm tra đối xứng. Hãy tìm một đỉnh màu xanh bên trong cây con của (R). Một đỉnh như vậy buộc cây con Steiner màu xanh đi qua (R), gây ra giao điểm. Nếu không có đỉnh như vậy tồn tại, xuất ra`YES`. 

Tại sao nó hoạt động có thể được tóm tắt bằng một bất biến: cây con Steiner của một màu là sự kết hợp của các đường dẫn từ LCA chung của màu đó đến tất cả các thiết bị đầu cuối của nó. Nếu hai LCA chung không thể so sánh được thì các hợp này nằm trong các cây con có gốc rời rạc. Nếu một LCA nằm trên LCA kia, chẳng hạn (R) phía trên (B), thì cây con màu xanh nằm hoàn toàn bên trong cây con của (B) và cây con màu đỏ giao với vùng đó một cách chính xác khi một số đầu cuối màu đỏ nằm bên trong nó. Thuật toán kiểm tra chính xác những khả năng này, do đó nó chấp nhận chính xác các dự đoán mà hai cây con Steiner yêu cầu tách rời nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)
    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    order = []

    # Iterative DFS.
    timer = 0
    stack = [(1, 0, 0)]

    while stack:
        v, p, state = stack.pop()

        if state == 0:
            parent[v] = p
            tin[v] = timer
            timer += 1
            order.append(v)

            stack.append((v, p, 1))

            for u in reversed(graph[v]):
                if u != p:
                    depth[u] = depth[v] + 1
                    stack.append((u, v, 0))
        else:
            tout[v] = timer

    # Subtree sizes and heavy child.
    size = [1] * (n + 1)
    heavy = [0] * (n + 1)

    for v in reversed(order):
        best_size = 0

        for u in graph[v]:
            if parent[u] == v:
                size[v] += size[u]
                if size[u] > best_size:
                    best_size = size[u]
                    heavy[v] = u

    # Heavy-light decomposition.
    head = [0] * (n + 1)
    chain_stack = [(1, 1)]

    while chain_stack:
        v, h = chain_stack.pop()

        while v:
            head[v] = h
            hv = heavy[v]

            for u in graph[v]:
                if parent[u] == v and u != hv:
                    chain_stack.append((u, u))

            v = hv

    def lca(a, b):
        while head[a] != head[b]:
            if depth[head[a]] > depth[head[b]]:
                a = parent[head[a]]
            else:
                b = parent[head[b]]

        return a if depth[a] < depth[b] else b

    def is_ancestor(a, b):
        return tin[a] <= tin[b] < tout[a]

    q = int(input())
    answer = []

    for _ in range(q):
        data = list(map(int, input().split()))
        r, b = data[0], data[1]

        reds = data[2:2 + r]
        blues = data[2 + r:2 + r + b]

        red_lca = reds[0]
        for v in reds[1:]:
            red_lca = lca(red_lca, v)

        blue_lca = blues[0]
        for v in blues[1:]:
            blue_lca = lca(blue_lca, v)

        if not is_ancestor(red_lca, blue_lca) and \
           not is_ancestor(blue_lca, red_lca):
            answer.append("YES")
            continue

        if is_ancestor(red_lca, blue_lca):
            # Red's Steiner tree intersects Blue's subtree
            # exactly when some red terminal is inside it.
            bad = False
            for v in reds:
                if is_ancestor(blue_lca, v):
                    bad = True
                    break
            answer.append("NO" if bad else "YES")
        else:
            # Symmetric case.
            bad = False
            for v in blues:
                if is_ancestor(red_lca, v):
                    bad = True
                    break
            answer.append("NO" if bad else "YES")

    sys.stdout.write("\n".join(answer))

if __name__ == "__main__":
    solve()
```Giai đoạn tiền xử lý đầu tiên thực hiện DFS lặp thay vì đệ quy. Một cây hình đường dẫn có thể chứa (200000) đỉnh, đủ sâu để vượt quá giới hạn đệ quy thông thường của Python, do đó DFS đệ quy sẽ là một nguồn lỗi không cần thiết. 

các`tin`Và`tout`mảng được lấp đầy khi một đỉnh được nhập và thoát. Bởi vì quá trình truyền tải là một DFS, tất cả các đỉnh con của một đỉnh đều nhận được thời gian vào trước thời gian thoát của nó. Do đó,`a`là tổ tiên của`b`chính xác khi nào`tin[a] <= tin[b] < tout[a]`. 

các`size`Và`heavy`mảng được tính theo thứ tự DFS ngược. Mọi cây con đều đã được tính toán kích thước cây con khi cha của nó được xử lý. Đứa lớn nhất trở thành đứa nặng. 

Phân rã nặng-ánh sáng chỉ lưu trữ đầu chuỗi cho mỗi đỉnh. Để tìm LCA, chúng ta di chuyển đầu chuỗi sâu hơn lên trên cho đến khi cả hai đỉnh nằm trên cùng một chuỗi. Một cạnh nhẹ chỉ có thể được vượt qua (O(\log n)) lần, bởi vì việc chọn một cây con nhẹ sẽ làm giảm kích thước cây con còn lại ít nhất là hai lần. 

Đối với mọi truy vấn, các đỉnh màu đỏ và màu xanh được giữ lại vì quá trình quét tổ tiên ở cuối có thể cần phải kiểm tra các thiết bị đầu cuối ban đầu. Tổng số thiết bị đầu cuối được lưu trữ trên tất cả các truy vấn tối đa là (200000), do đó, điều này không tạo ra chi phí bộ nhớ lớn. 

điều kiện`is_ancestor(red_lca, blue_lca)`cố tình bao gồm sự bình đẳng. Nếu hai LCA bằng nhau thì mọi thiết bị đầu cuối màu đỏ đều nằm trong cây con của LCA chung, do đó lần quét tiếp theo sẽ ngay lập tức tìm thấy thiết bị đầu cuối màu đỏ ở đó và trả về`NO`. Điều này xử lý trường hợp LCA chung mà không yêu cầu một nhánh riêng. 

Không có phép tính số nguyên nào liên quan đến các giá trị lớn hơn (n), do đó việc tràn số nguyên trong Python là không liên quan. Ranh giới thực hiện quan trọng là khoảng Euler nửa mở trong`is_ancestor`: sử dụng`tout[v]`như một điểm cuối bao gồm sẽ gây ra lỗi từng cái một. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với cây mẫu có gốc tại thành phố (1), các mối quan hệ tổ tiên có liên quan là (1) ở trên (2,3), (2) ở trên (4,5) và (3) ở trên (6,7). 

| Truy vấn | Đỉnh đỏ | LCA đỏ | Đỉnh xanh | LCA xanh | Mối quan hệ | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | (4,5) | 2 | (6,7) | 3 | Không thể so sánh được | CÓ | 
| 2 | (4,6) | 1 | (5,7) | 1 | Bằng | KHÔNG | 
| 3 | (1,4) | 1 | (5,2) | 2 | LCA đỏ trên LCA xanh, đỏ (4) dưới 2 | KHÔNG | 
| 4 | (4,5) | 2 | (1) | 1 | LCA màu xanh phía trên LCA màu đỏ, màu xanh lam (1) nằm ngoài cây con 2 | CÓ | 
| 5 | (1) | 1 | (2) | 2 | LCA đỏ phía trên LCA xanh, đỏ (1) nằm ngoài cây con 2 | CÓ | 
| 6 | (1,2,3,4,5,6) | 1 | (7) | 7 | LCA đỏ phía trên LCA xanh, không có đỉnh đỏ dưới 7 | CÓ | 

Truy vấn đầu tiên thể hiện trường hợp thành công đơn giản nhất trong đó hai cây con Steiner nằm ở các nhánh khác nhau của gốc. Phần thứ hai chứng minh tại sao chỉ kiểm tra các đỉnh cuối là không đủ, bởi vì cả hai cây con Steiner đều phải đi qua thành phố (1). Phần thứ ba trình bày thử nghiệm cây con lồng nhau. Mặc dù các đỉnh LCA màu đỏ và màu xanh lam khác nhau, thành phố màu đỏ (4) buộc cây con màu đỏ thông qua LCA màu xanh lam (2). 

### Ví dụ thứ hai 

Hãy xem xét một con đường:```
5
1 2
2 3
3 4
4 5
3
2 1 2 4 5
2 1 1 4 3
2 1 1 3 2
```Truy vấn đầu tiên có các thành phố màu đỏ (1,2) và các thành phố màu xanh lam (4,5). Các cây con màu chiếm các cạnh đối diện của cạnh (2-3). 

| Truy vấn | LCA đỏ | LCA xanh | Quan hệ tổ tiên | Terminal bên trong cây con lồng nhau | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 1 trên 4 | Không có đỉnh đỏ trong cây con 4 | CÓ | 
| 2 | 1 | 4 | 1 trên 4 | Không có đỉnh đỏ trong cây con 4 | CÓ | 
| 3 | 1 | 3 | 1 trên 3 | Đỉnh đỏ 1 không có trong cây con 3 | CÓ | 

Để chứng minh phiên bản từ chối của cùng một cấu trúc, hãy thay đổi truy vấn thứ hai thành thành phố màu đỏ (1,5) và thành phố màu xanh lam (3). LCA màu đỏ là (1), LCA màu xanh lam là (3) và thành phố màu đỏ (5) nằm bên dưới (3). Đường màu đỏ từ (1) đến (5) phải đi qua (3) nên đáp án trở thành`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n + S\log n)) | Tiền xử lý cây là tuyến tính. Mỗi thành phố trong số (S\le200000) được đề cập tham gia vào nhiều nhất một lần quét LCA hoặc tổ tiên và mọi chi phí LCA (O(\log n)). | 
| Không gian | (O(n+S)) | Mảng cây và mảng ánh sáng nặng sử dụng bộ nhớ (O(n)), trong khi truy vấn hiện tại lưu trữ các thiết bị đầu cuối (O(r+b)). | 

Tối đa (n) và tổng số thành phố được tô màu đều là (200000). Quá trình xử lý trước chỉ chạm vào mỗi con đường một số lần không đổi, trong khi giai đoạn truy vấn chỉ thực hiện các hoạt động LCA logarit trên các thành phố được dự đoán đề cập rõ ràng. Do đó, giải pháp phù hợp với giới hạn 2 giây và 256 MB mà không cần dựa vào đệ quy hoặc bàn nâng lớn (O(n\log n)). 

## Trường hợp thử nghiệm```python
# The solution above defines solve() and the global input variable.
# This harness temporarily replaces stdin/stdout so solve() can be tested
# multiple times in one process.

import sys
import io

def run(inp: str) -> str:
    global input

    old_input = input
    old_stdout = sys.stdout

    input = io.StringIO(inp).readline
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        input = old_input
        sys.stdout = old_stdout

# Provided sample
sample1 = """\
7
1 2
1 3
2 4
2 5
3 6
3 7
6
2 2 4 5 6 7
2 2 4 6 5 7
2 1 4 5 2
2 1 4 5 1
1 1 1 2
6 1 1 2 3 4 5 6 7
"""

assert run(sample1) == """\
YES
NO
NO
YES
YES
YES""", "sample 1"

# Minimum-size tree.
minimum = """\
2
1 2
1
1 1 2
"""

assert run(minimum) == "YES", "minimum tree"

# Star where both color Steiner trees must use the center.
same_lca = """\
5
1 2
1 3
1 4
1 5
1
2 2 3 4 5
"""

assert run(same_lca) == "NO", "same LCA"

# Path with both a successful nested case and a failing nested case.
path_cases = """\
5
1 2
2 3
3 4
4 5
3
2 1 2 4 5
2 1 1 4 3
2 1 1 5 3
"""

assert run(path_cases) == """\
YES
YES
NO""", "nested ancestor cases"

# Maximum-size tree and maximum total number of colored cities.
# Red = 1..100000, Blue = 100001..200000.
# Their Steiner subtrees are separated by the edge 100000-100001.
n = 200000
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))

red = " ".join(str(i) for i in range(1, 100001))
blue = " ".join(str(i) for i in range(100001, 200001))

maximum = (
    f"{n}\n"
    f"{edges}\n"
    f"1\n"
    f"100000 100000 {red} {blue}\n"
)

assert run(maximum) == "YES", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`YES NO NO YES YES YES`| Mẫu chính thức đầy đủ, bao gồm các trường hợp không thể so sánh được, LCA bằng nhau và lồng nhau | 
| Cây hai nút |`YES`| Tối thiểu (n), một thành phố đỏ và một thành phố xanh | 
| Sao năm nút |`NO`| Cả hai cây Steiner gặp nhau tại cùng một LCA | 
| Đường dẫn năm nút |`YES YES NO`| Mối quan hệ tổ tiên lồng nhau và kiểm tra cây con thiết bị đầu cuối mang tính quyết định | 
| (n=200000) đường dẫn |`YES`| Kích thước tối đa, tổng số đầu vào truy vấn tối đa và an toàn truyền tải lặp đi lặp lại | 

## Vỏ cạnh 

Cây hai thành phố không có cấu trúc bên trong để giải thích. Với```
2
1 2
1
1 1 2
```LCA màu đỏ là (1), LCA màu xanh lam là (2) và (1) là tổ tiên của (2). Đỉnh màu đỏ duy nhất là (1), không nằm trong cây con của (2), do đó thuật toán trả về`YES`. 

Đối với trường hợp LCA thông thường,```
5
1 2
1 3
1 4
1 5
1
2 2 3 4 5
```LCA màu đỏ là (1) và LCA màu xanh cũng là (1). Thử nghiệm tổ tiên đầu tiên thành công với sự bằng nhau và thuật toán quét các đỉnh màu đỏ theo cây con của (1). Mọi đỉnh đỏ đều nằm trong nó nên nó sẽ trả về`NO`. Đây chính xác là tình huống trong đó hai nhóm được tách thành nhóm thiết bị đầu cuối nhưng không thể tách thành nhóm được kết nối. 

Đối với trường hợp lồng nhau nhưng giao nhau,```
4
1 2
2 3
3 4
1
2 1 4 3
```LCA màu đỏ là (1) và LCA màu xanh lam là (3). Vì (1) là tổ tiên của (3), nên thuật toán kiểm tra xem đầu cuối màu đỏ có nằm trong cây con có gốc tại (3) hay không. Thành phố đỏ (4) cũng vậy nên đường màu đỏ từ (1) đến (4) phải đi qua (3). Câu trả lời là`NO`. 

Đối với trường hợp lồng nhau nhưng có thể tách rời,```
4
1 2
2 3
3 4
1
2 1 2 3 4
```LCA màu đỏ là (1) và LCA màu xanh lam là (3). Không có thành phố màu đỏ nào nằm trong cây con có gốc tại (3), vì các thành phố màu đỏ là (1) và (2). Cây con Steiner màu đỏ kết thúc trước khi đi vào cây con màu xanh nên cạnh (2-3) có thể đóng được và đáp án là`YES`. 

Đường dẫn có kích thước tối đa cũng kiểm tra trường hợp cạnh dành riêng cho Python. Cây có thể có độ sâu (199999), do đó DFS đệ quy sẽ không an toàn. Việc triển khai sử dụng một ngăn xếp rõ ràng để xử lý trước, trong khi tất cả các hoạt động LCA đều sử dụng chuỗi nặng-nhẹ. Do đó, thuật toán xử lý đường đi của (200000) thành phố mà không gặp vấn đề về độ sâu đệ quy.
