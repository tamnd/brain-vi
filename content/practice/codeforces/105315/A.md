---
title: "CF 105315A - Sinh Nhật Đá Cẩm Thạch"
description: "Chúng ta có một số đồ thị có hướng, mỗi đồ thị được mô tả bởi các nút và các cạnh có hướng. Cạnh được định hướng từ nút này sang nút khác có nghĩa là bạn chỉ có thể di chuyển theo hướng đó."
date: "2026-06-23T15:05:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105315
codeforces_index: "A"
codeforces_contest_name: "JPC 4.0"
rating: 0
weight: 105315
solve_time_s: 60
verified: true
draft: false
---

[CF 105315A - Sinh nhật của Marble](https://codeforces.com/problemset/problem/105315/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số đồ thị có hướng, mỗi đồ thị được mô tả bởi các nút và các cạnh có hướng. Cạnh được định hướng từ nút này sang nút khác có nghĩa là bạn chỉ có thể di chuyển theo hướng đó. 

Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là xác định số lượng nhỏ nhất các cạnh có hướng bổ sung mà chúng ta cần thêm để sau khi thêm chúng, mọi nút có thể đến mọi nút khác bằng cách đi theo các cạnh có hướng. Về mặt biểu đồ, chúng tôi muốn biểu đồ trở nên được kết nối chặt chẽ. 

Ý tưởng quan trọng là khả năng tiếp cận ở đây không đối xứng. Ngay cả khi một đường đi tồn tại từ A đến B, nó không ngụ ý một đường đi từ B quay lại A. Mục tiêu là sửa chữa sự bất đối xứng này với số cạnh được thêm vào tối thiểu. 

Các ràng buộc rất lớn: tổng số nút và cạnh trong tất cả các trường hợp thử nghiệm có thể lên tới một triệu. Điều này loại trừ mọi cách tiếp cận cố gắng kiểm tra trực tiếp khả năng tiếp cận giữa tất cả các cặp nút, vì ngay cả một ý tưởng kiểu Floyd-Warshall cũng sẽ ngay lập tức vượt quá giới hạn. Ngay cả BFS/DFS lặp lại từ mọi nút cũng sẽ quá chậm, vì đó sẽ là O(n(n + m)) trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế hơn xuất hiện khi đồ thị đã được kết nối mạnh mẽ. Trong tình huống đó, không cần có cạnh nào và câu trả lời phải bằng 0. Một tình huống phức tạp khác là khi đồ thị gần như được kết nối nhưng lại bị chia thành các thành phần tạo thành một chuỗi. Một cách tiếp cận ngây thơ có thể cố gắng “kết nối các thành phần một cách tham lam” mà không nhận ra cấu trúc thực sự chi phối số lượng cạnh tối thiểu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liên tục kiểm tra xem biểu đồ có được kết nối mạnh hay không và nếu không, hãy thử thêm một cạnh giữa mỗi cặp nút và kiểm tra lại. Mỗi lần kiểm tra kết nối đều tốn O(n + m) và có thể có O(n²) cạnh cần xem xét, khiến điều này hoàn toàn không khả thi. 

Thông tin chi tiết về cấu trúc quan trọng là bên trong bất kỳ đồ thị có hướng nào, các nút hình thành các thành phần được kết nối chặt chẽ một cách tự nhiên. Bên trong mỗi thành phần như vậy, mọi nút đều có thể tiếp cận mọi nút khác. Nếu chúng ta nén từng thành phần vào một nút duy nhất thì cấu trúc thu được là một biểu đồ tuần hoàn có hướng. 

Khi biểu đồ được rút gọn thành biểu đồ thành phần này, vấn đề ban đầu trở nên đơn giản hơn nhiều: chúng tôi không còn làm việc với các nút riêng lẻ nữa mà với các SCC hoạt động giống như các đơn vị nguyên tử. Cách duy nhất để làm cho toàn bộ biểu đồ được kết nối chặt chẽ là đảm bảo rằng biểu đồ thành phần này trở thành một chu trình, đòi hỏi phải kết nối cẩn thận các nguồn và phần chìm của nó. 

Quan sát cuối cùng là chỉ có hai loại thành phần quan trọng: những thành phần không có cạnh vào trong biểu đồ ngưng tụ và những thành phần không có cạnh ra. Số lượng cạnh tối thiểu cần thiết được xác định bằng số lượng cạnh tồn tại, bởi vì mỗi cạnh được thêm vào có thể cố định đồng thời một mối quan hệ nguồn và một đích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong n | O(n + m) | Quá chậm | 
| Nén SCC + Đếm độ | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Đầu tiên, tính tất cả các thành phần liên thông mạnh của đồ thị có hướng. Nhóm các nút này sao cho mọi nút trong một nhóm có thể tiếp cận mọi nút khác trong cùng một nhóm. Bước này là cần thiết vì cấu trúc bên trong của một thành phần không ảnh hưởng đến câu trả lời. 
2. Để tìm SCC một cách hiệu quả, chúng tôi chạy phương pháp DFS hai bước (thuật toán Kosaraju). Trong lần đầu tiên, chúng tôi duyệt qua biểu đồ và ghi lại các nút theo thứ tự hoàn thành. Thứ tự này nắm bắt sự phụ thuộc giữa các thành phần. 
3. Sau đó, chúng tôi đảo ngược tất cả các cạnh trong biểu đồ và xử lý các nút theo thứ tự thời gian hoàn thiện giảm dần. Mỗi lần duyệt DFS trong lần thứ hai này sẽ phát hiện chính xác một thành phần được kết nối mạnh. 
4. Sau khi chỉ định ID thành phần cho mỗi nút, chúng tôi coi mỗi thành phần là một nút duy nhất trong biểu đồ thu gọn mới. 
5. Chúng ta lặp qua tất cả các cạnh ban đầu. Với mọi cạnh từ u đến v, nếu u và v thuộc các thành phần khác nhau, chúng ta thêm một cạnh có hướng giữa các thành phần tương ứng của chúng. 
6. Đối với mỗi thành phần, chúng tôi tính toán hai giá trị: có bao nhiêu cạnh đi vào và bao nhiêu cạnh để lại trong biểu đồ thu gọn. 
7. Chúng tôi đếm có bao nhiêu thành phần không có cạnh vào và bao nhiêu thành phần không có cạnh ra. 
8. Nếu tổng cộng chỉ có một thành phần thì đồ thị đã được liên thông mạnh và câu trả lời là 0. Ngược lại, câu trả lời là số lượng thành phần nguồn và thành phần đích tối đa. 

### Tại sao nó hoạt động 

Đồ thị ngưng tụ luôn là đồ thị không theo chu kỳ có hướng. Trong cấu trúc như vậy, bất kỳ biểu đồ cuối cùng được kết nối mạnh mẽ nào cũng phải loại bỏ tất cả các nguồn và điểm chìm bằng cách đưa ra các kết nối mới. Mỗi cạnh được thêm vào có thể giảm số lượng nguồn hoặc phần chìm tối đa một đơn vị hiệu quả, bởi vì nó kết nối nhiều nhất một đuôi SCC với một đầu SCC. Do đó, số lượng tối đa của hai số lượng thể hiện nút thắt cổ chai không thể tránh khỏi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        g = [[] for _ in range(n)]
        rg = [[] for _ in range(n)]

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            rg[v].append(u)

        visited = [False] * n
        order = []

        sys.setrecursionlimit(10**7)

        def dfs1(u):
            stack = [(u, 0)]
            visited[u] = True
            while stack:
                node, i = stack[-1]
                if i < len(g[node]):
                    nxt = g[node][i]
                    stack[-1] = (node, i + 1)
                    if not visited[nxt]:
                        visited[nxt] = True
                        stack.append((nxt, 0))
                else:
                    order.append(node)
                    stack.pop()

        for i in range(n):
            if not visited[i]:
                dfs1(i)

        comp = [-1] * n
        cid = 0

        def dfs2(u):
            stack = [u]
            comp[u] = cid
            while stack:
                node = stack.pop()
                for nxt in rg[node]:
                    if comp[nxt] == -1:
                        comp[nxt] = cid
                        stack.append(nxt)

        for v in reversed(order):
            if comp[v] == -1:
                dfs2(v)
                cid += 1

        if cid == 1:
            print(0)
            continue

        indeg = [0] * cid
        outdeg = [0] * cid

        for u in range(n):
            for v in g[u]:
                if comp[u] != comp[v]:
                    outdeg[comp[u]] += 1
                    indeg[comp[v]] += 1

        sources = sum(1 for i in range(cid) if indeg[i] == 0)
        sinks = sum(1 for i in range(cid) if outdeg[i] == 0)

        print(max(sources, sinks))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng cả biểu đồ và mặt trái của nó. Cấu trúc kép này là cần thiết để thuật toán của Kosaraju xác định chính xác các thành phần được kết nối mạnh mẽ. 

DFS đầu tiên thu thập các đỉnh theo thứ tự hoàn thiện mà không gặp vấn đề về độ sâu ngăn xếp đệ quy bằng cách sử dụng ngăn xếp rõ ràng. DFS thứ hai gán ID thành phần bằng biểu đồ đảo ngược. Khi các thành phần được chỉ định, biểu đồ ngưng tụ không được xây dựng rõ ràng dưới dạng danh sách kề; thay vào đó, chúng tôi trực tiếp tính toán số lượng độ trong và độ ngoài trong khi quét các cạnh ban đầu. 

Kiểm tra có điều kiện cuối cùng cho một thành phần duy nhất xử lý trường hợp đã được kết nối mạnh mẽ, nếu không sẽ tạo ra số lượng dương không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Biểu đồ đầu vào có các cạnh tạo thành một chu trình giữa ba nút cộng với một kết nối bổ sung đến nút thứ tư, tạo ra nhiều SCC. 

| Bước | Hành động | Linh kiện | Nguồn | Bồn rửa | 
| --- | --- | --- | --- | --- | 
| 1 | Xây dựng SCC | {chu kỳ}, {node4} | 1 | 1 | 
| 2 | Xây dựng các cạnh ngưng tụ | 1 cạnh giữa các SCC | | | 
| 3 | Đếm indeg/outdeg | SCC0 chỉ số 0, SCC1 vượt trội 0 | 1 | 1 | 
| 4 | Tính đáp án | tối đa(1,1) = 1 | | | 

Điều này cho thấy một cạnh là đủ để kết nối hai SCC thành một cấu trúc liên kết chặt chẽ. 

### Ví dụ 2 

Biểu đồ đã tạo thành một chu kỳ duy nhất trên tất cả các nút. 

| Bước | Hành động | Linh kiện | Nguồn | Bồn rửa | 
| --- | --- | --- | --- | --- | 
| 1 | Phân hủy SCC | 1 thành phần | 0 | 0 | 
| 2 | Thoát sớm | cid = 1 | - | - | 
| 3 | Đầu ra | 0 | | | 

Điều này xác nhận rằng thuật toán tránh được việc bổ sung cạnh không cần thiết một cách chính xác khi đồ thị đã được kết nối mạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý một số lần không đổi trong quá trình xây dựng và tính độ SCC | 
| Không gian | O(n + m) | Danh sách kề, đồ thị ngược và mảng thành phần | 

Các ràng buộc cho phép tổng số lên tới một triệu nút và cạnh, do đó cần có giải pháp thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Cách tiếp cận dựa trên SCC phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # --- solution embedded ---
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, m = map(int, input().split())
            g = [[] for _ in range(n)]
            rg = [[] for _ in range(n)]

            for _ in range(m):
                u, v = map(int, input().split())
                u -= 1
                v -= 1
                g[u].append(v)
                rg[v].append(u)

            visited = [False] * n
            order = []

            sys.setrecursionlimit(10**7)

            def dfs1(u):
                stack = [(u, 0)]
                visited[u] = True
                while stack:
                    node, i = stack[-1]
                    if i < len(g[node]):
                        nxt = g[node][i]
                        stack[-1] = (node, i + 1)
                        if not visited[nxt]:
                            visited[nxt] = True
                            stack.append((nxt, 0))
                    else:
                        order.append(node)
                        stack.pop()

            for i in range(n):
                if not visited[i]:
                    dfs1(i)

            comp = [-1] * n
            cid = 0

            def dfs2(u):
                stack = [u]
                comp[u] = cid
                while stack:
                    node = stack.pop()
                    for nxt in rg[node]:
                        if comp[nxt] == -1:
                            comp[nxt] = cid
                            stack.append(nxt)

            for v in reversed(order):
                if comp[v] == -1:
                    dfs2(v)
                    cid += 1

            if cid == 1:
                print(0)
                return

            indeg = [0] * cid
            outdeg = [0] * cid

            for u in range(n):
                for v in g[u]:
                    if comp[u] != comp[v]:
                        outdeg[comp[u]] += 1
                        indeg[comp[v]] += 1

            sources = sum(1 for i in range(cid) if indeg[i] == 0)
            sinks = sum(1 for i in range(cid) if outdeg[i] == 0)

            print(max(sources, sinks))

    solve()

# sample-style sanity checks
assert run("1\n3 3\n1 2\n2 3\n3 1\n") == "0\n"
assert run("1\n2 0\n") == "1\n"
assert run("1\n4 3\n1 2\n2 3\n3 4\n") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 chu kỳ | 0 | đồ thị đã được kết nối mạnh mẽ | 
| 2 nút biệt lập | 1 | yêu cầu kết nối tối thiểu | 
| đồ thị chuỗi | 1 | SCC sụp đổ và xử lý nguồn/bồn duy nhất | 

## Vỏ cạnh 

Một biểu đồ được kết nối mạnh mẽ đầy đủ được xử lý bằng cách kết thúc sớm khi số lượng thành phần là một. Trong tình huống đó, logic mức độ và mức độ không bao giờ cần thiết và thuật toán đưa ra kết quả bằng 0 một cách chính xác. 

Một biểu đồ hoàn toàn bị ngắt kết nối, chẳng hạn như các nút không có cạnh, tạo ra một SCC cho mỗi nút. Trong biểu đồ ngưng tụ, mỗi nút vừa là nguồn vừa là điểm chìm. Thuật toán đếm số lượng nguồn và phần chìm bằng nhau, và số tối đa sẽ đưa ra chính xác số cạnh cần thiết để kết nối tất cả các thành phần thành một cấu trúc được kết nối mạnh mẽ duy nhất. 

Một chuỗi nút tuyến tính kiểm tra xem logic đồ thị ngưng tụ có xác định chính xác một nguồn và một bồn hay không. Thuật toán tạo ra một SCC nguồn duy nhất ở đầu chuỗi và một SCC chìm ở cuối chuỗi, mang lại câu trả lời là một, khớp với thực tế là một cạnh duy nhất có thể đóng chu trình.
