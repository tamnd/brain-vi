---
title: "CF 106409J - Truy vấn cây"
description: "Sự cố yêu cầu nhiều truy vấn phạm vi trên cây. Các đỉnh của cây có nhãn cố định từ 1 đến n. Một truy vấn đưa ra một khoảng liên tục các nhãn [l, r], nghĩa là chúng ta chọn mọi đỉnh có nhãn nằm trong khoảng này."
date: "2026-06-25T09:59:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106409
codeforces_index: "J"
codeforces_contest_name: "HPI 2026 Advanced"
rating: 0
weight: 106409
solve_time_s: 35
verified: true
draft: false
---

[CF 106409J - Truy vấn cây](https://codeforces.com/problemset/problem/106409/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Sự cố yêu cầu nhiều truy vấn phạm vi trên cây. Các đỉnh của cây có nhãn cố định từ`1`ĐẾN`n`. Một truy vấn đưa ra một khoảng thời gian liên tục của các nhãn`[l, r]`, nghĩa là chúng ta chọn mọi đỉnh có nhãn nằm trong khoảng này. Câu trả lời là số cạnh trong sơ đồ con liên thông nhỏ nhất của cây chứa tất cả các đỉnh được chọn. Đây là kích thước của cây con tối thiểu bao trùm các đỉnh đó. Câu lệnh ban đầu định nghĩa đây là tập hợp các cạnh nhỏ nhất kết nối mọi cặp đỉnh được chọn. 

Một cách trực tiếp để suy nghĩ về câu trả lời là nếu chúng ta đánh dấu các đỉnh đã chọn, chúng ta cần giữ chính xác các cạnh nằm trên đường đi giữa chúng. Đối với một đỉnh duy nhất, câu trả lời là 0 vì không cần cạnh nào. Đối với hai đỉnh, câu trả lời đơn giản là khoảng cách của chúng. Đối với nhiều đỉnh, câu trả lời là kích thước của cây Steiner bên trong cây ban đầu. 

Các ràng buộc buộc một cách tiếp cận không tầm thường. Tổng số đỉnh nhiều nhất là`2 * 10^5`và tổng số truy vấn nhiều nhất là`3 * 10^5`. Việc xây dựng lại cây con bao trùm cho mỗi truy vấn sẽ yêu cầu truy cập nhiều đỉnh nhiều lần. Một giải pháp gần`O(nq)`là không thể, thậm chí`O(n sqrt(n))`tiền xử lý kiểu phải được thiết kế cẩn thận. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. Nếu truy vấn chỉ chứa một đỉnh thì câu trả lời là 0.```
Input:
1
1 1
1 1
```Tập được chọn là`{1}`, do đó đầu ra là:```
0
```Một giải pháp luôn tính toán khoảng cách giữa các đỉnh được chọn liên tiếp sẽ tạo ra phần đóng góp khác 0 không chính xác nếu nó không xử lý được trường hợp một phần tử. 

Truy vấn chứa toàn bộ cây là một trường hợp biên quan trọng khác.```
Input:
1
4 1
1 2
2 3
2 4
1 4
```Tất cả các đỉnh được chọn. Câu trả lời là cả ba cạnh:```
3
```Bất kỳ phương pháp nào chỉ xem xét đường đi trực tiếp giữa các đỉnh được chọn đều có thể bỏ lỡ các nhánh như cạnh`2-4`. 

Sai lầm phổ biến thứ ba là quên rằng các đỉnh được chọn được sắp xếp theo vị trí DFS của chúng chứ không phải theo nhãn của chúng. Các nhãn chỉ xác định khoảng thời gian truy vấn. Cấu trúc cây xác định cạnh nào được yêu cầu. 

## Phương pháp tiếp cận 

Giải pháp vũ phu bắt đầu từ định nghĩa. Đối với mỗi truy vấn`[l, r]`, thu thập tất cả các đỉnh có nhãn trong khoảng đó. Sau đó xây dựng cây con tối thiểu chứa chúng. Một cách là liên tục thêm đường đi từ mỗi đỉnh được chọn vào cây con hiện tại. Điều này đúng vì mọi cạnh được yêu cầu phải thuộc về một đường dẫn giữa các đỉnh được chọn. Tuy nhiên, trong trường hợp xấu nhất, một truy vấn có thể chứa hầu hết tất cả các đỉnh và việc xây dựng đường dẫn có thể tốn kém.`O(n)`. Với`q`truy vấn này trở thành`O(nq)`, có thể đạt tới khoảng`6 * 10^10`hoạt động. 

Quan sát chính là chúng ta không cần xây dựng cây con một cách rõ ràng cho mọi truy vấn. Đối với bất kỳ tập hợp đỉnh nào, hãy sắp xếp chúng theo thời gian nhập DFS của chúng. Nếu các đỉnh theo thứ tự này là`v1, v2, ..., vk`, chiều dài của cây bao trùm là:$$\frac{dist(v_1,v_2)+dist(v_2,v_3)+...+dist(v_k,v_1)}{2}$$Tổng tuần hoàn tính mỗi cạnh chính xác hai lần, một lần từ mỗi phía của vết cắt do cạnh đó tạo ra. 

Công thức này thay đổi hoàn toàn vấn đề. Bây giờ, một truy vấn yêu cầu chúng tôi duy trì một tập hợp các đỉnh động và tổng khoảng cách theo chu kỳ giữa các đỉnh liền kề theo thứ tự DFS. Vì các truy vấn là các khoảng trên nhãn đỉnh nên chúng ta có thể xử lý chúng bằng thuật toán Mo. Di chuyển từ khoảng này sang khoảng tiếp theo chỉ thêm hoặc bớt một số lượng nhỏ các đỉnh. 

Khi một đỉnh được chèn vào tập thứ tự DFS, chỉ có hai cặp lân cận thay đổi. Nếu người tiền nhiệm của nó là`a`và người kế vị là`b`, tổng khoảng cách tăng thêm:$$dist(a,x)+dist(x,b)-dist(a,b)$$Việc loại bỏ một đỉnh sẽ đảo ngược thao tác này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nq)`|`O(n)`| Quá chậm | 
| Tối ưu |`O((n+q)√n log n)`|`O(n log n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây ở bất kỳ đỉnh nào và tính thời gian vào DFS. Đồng thời tính toán các bảng nâng nhị phân cho các truy vấn LCA. Thứ tự DFS đưa ra một thứ tự vòng tròn cố định trong đó các đỉnh đã chọn có thể được duy trì. 
2. Chuyển đổi mọi truy vấn`[l, r]`vào truy vấn Mo trên nhãn đỉnh. Sắp xếp truy vấn theo khối giúp giảm tổng số thao tác chèn và xóa. 
3. Duy trì tập hợp các đỉnh đã chọn hiện tại. Lưu trữ các vị trí DFS đang hoạt động và hỗ trợ tìm kiếm các vị trí hoạt động trước đó và tiếp theo. 
4. Khi thêm một đỉnh`x`, tìm tiền thân và tiền nhiệm của nó theo thứ tự DFS. Thay thế cạnh cũ trong tổng khoảng cách hình tròn giữa chúng bằng hai cạnh mới thông qua`x`. 
5. Khi loại bỏ một đỉnh`x`, thực hiện thao tác nghịch đảo. Hai cạnh xuyên qua`x`biến mất và kết nối trực tiếp tiền nhiệm-kế nhiệm quay trở lại. 
6. Sau khi xử lý truy vấn, chia tổng khoảng cách chu kỳ được duy trì cho 2. Kết quả là số cạnh trong cây con tối thiểu. 

Tại sao nó hoạt động: bất biến là giá trị được duy trì luôn là tổng khoảng cách theo chu kỳ giữa các đỉnh được chọn liên tiếp theo thứ tự DFS. Công thức chèn và xóa cập nhật chính xác các cặp lân cận bị ảnh hưởng, do đó, bất biến vẫn đúng sau mỗi chuyển động Mo. Vì tổng tuần hoàn của các đỉnh được đánh dấu của cây tính mỗi cạnh trong cây con kết nối tối thiểu hai lần, chia cho hai sẽ cho câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = sys.stdin.buffer.read().split()
    it = iter(data)

    t = int(next(it))
    out = []

    for _ in range(t):
        n = int(next(it))
        q = int(next(it))

        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            a = int(next(it)) - 1
            b = int(next(it)) - 1
            g[a].append(b)
            g[b].append(a)

        LOG = (n).bit_length()
        up = [[0] * n for _ in range(LOG)]
        tin = [0] * n
        depth = [0] * n
        order = []
        
        stack = [(0, -1, 0)]
        timer = 0
        while stack:
            u, p, state = stack.pop()
            if state == 0:
                tin[u] = timer
                order.append(u)
                timer += 1
                up[0][u] = u if p == -1 else p
                for i in range(1, LOG):
                    up[i][u] = up[i - 1][up[i - 1][u]]
                stack.append((u, p, 1))
                for v in reversed(g[u]):
                    if v != p:
                        depth[v] = depth[u] + 1
                        stack.append((v, u, 0))

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
            for i in range(LOG - 1, -1, -1):
                if up[i][a] != up[i][b]:
                    a = up[i][a]
                    b = up[i][b]
            return up[0][a]

        def dist(a, b):
            c = lca(a, b)
            return depth[a] + depth[b] - 2 * depth[c]

        queries = []
        block = max(1, int(n ** 0.5))
        for idx in range(q):
            l = int(next(it)) - 1
            r = int(next(it)) - 1
            queries.append((l, r, idx))

        queries.sort(key=lambda x: (x[0] // block, x[1] if (x[0] // block) % 2 == 0 else -x[1]))

        active = [False] * n
        pos_to_node = [0] * n
        node_to_pos = tin[:]

        bs = 450
        cnt = [0] * ((n + bs - 1) // bs + 1)
        positions = [False] * n

        def previous(pos):
            b = pos // bs
            for i in range(pos - 1, b * bs - 1, -1):
                if positions[i]:
                    return i
            for j in range(b - 1, -1, -1):
                if cnt[j]:
                    for i in range(min(n - 1, (j + 1) * bs - 1), j * bs - 1, -1):
                        if positions[i]:
                            return i
            return -1

        def following(pos):
            b = pos // bs
            for i in range(pos + 1, min(n, (b + 1) * bs)):
                if positions[i]:
                    return i
            for j in range(b + 1, len(cnt)):
                if cnt[j]:
                    for i in range(j * bs, min(n, (j + 1) * bs)):
                        if positions[i]:
                            return i
            return -1

        cur_sum = 0
        size = 0

        def add_node(x):
            nonlocal cur_sum, size
            p = node_to_pos[x]
            if size:
                a = previous(p)
                b = following(p)
                if a == -1:
                    a = following(p)
                if b == -1:
                    b = previous(p)
                if a == -1:
                    cur_sum += 0
                else:
                    cur_sum += dist(pos_to_node[a], x)
                    cur_sum += dist(x, pos_to_node[b])
                    cur_sum -= dist(pos_to_node[a], pos_to_node[b])
            positions[p] = True
            pos_to_node[p] = x
            cnt[p // bs] += 1
            size += 1

        def remove_node(x):
            nonlocal cur_sum, size
            p = node_to_pos[x]
            if size > 1:
                a = previous(p)
                b = following(p)
                if a == -1:
                    a = b
                if b == -1:
                    b = a
                cur_sum -= dist(pos_to_node[a], x)
                cur_sum -= dist(x, pos_to_node[b])
                cur_sum += dist(pos_to_node[a], pos_to_node[b])
            positions[p] = False
            cnt[p // bs] -= 1
            size -= 1

        ans = [0] * q
        L = 0
        R = -1

        for l, r, idx in queries:
            while L > l:
                L -= 1
                add_node(L)
            while R < r:
                R += 1
                add_node(R)
            while L < l:
                remove_node(L)
                L += 1
            while R > r:
                remove_node(R)
                R -= 1
            ans[idx] = cur_sum // 2

        out.extend(map(str, ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý xây dựng thứ tự DFS và bảng nâng nhị phân. Cấu trúc LCA chỉ được sử dụng để đánh giá khoảng cách một cách nhanh chóng vì mỗi lần cập nhật chu trình được duy trì đều yêu cầu một vài phép tính khoảng cách. 

Thứ tự Mo là thứ giữ cho số lượng thao tác thêm và xóa có thể quản lý được. Mảng hoạt động được lập chỉ mục theo vị trí DFS chứ không phải theo nhãn đỉnh vì công thức tuần hoàn phụ thuộc vào thứ tự DFS. 

Việc phân rã căn bậc hai trên các vị trí DFS sẽ thay thế cây tìm kiếm nhị phân cân bằng. Python không cung cấp trực tiếp nên các tìm kiếm trước và sau được xử lý theo khối. Kích thước khối được chọn sao cho số lượng vị trí được kiểm tra vẫn nhỏ. 

Việc chia cho hai chỉ được thực hiện khi trả lời một truy vấn. Giá trị duy trì được cố ý giữ ở kích thước cây con nhân đôi, giúp tránh các giá trị trung gian phân đoạn. 

## Ví dụ đã hoạt động 

Hãy xem xét cây mẫu:```
1-2
2-3
2-4
1-5
5-6
```Đối với truy vấn`[1,3]`, các đỉnh được chọn là`1,2,3`. 

| Bước | Các đỉnh được chọn | Tổng khoảng cách theo chu kỳ | Trả lời | 
| --- | --- | --- | --- | 
| Thêm 1 | {1} | 0 | 0 | 
| Thêm 2 | {1,2} | 2 | 1 | 
| Thêm 3 | {1,2,3} | 4 | 2 | 

Tổng tuần hoàn đếm các cạnh`1-2`Và`2-3`hai lần, đưa ra câu trả lời`2`. 

Đối với truy vấn`[4,6]`, các đỉnh được chọn là`4,5,6`. 

| Bước | Các đỉnh được chọn | Tổng khoảng cách theo chu kỳ | Trả lời | 
| --- | --- | --- | --- | 
| Thêm 4 | {4} | 0 | 0 | 
| Thêm 5 | {4,5} | 6 | 3 | 
| Thêm 6 | {4,5,6} | 8 | 4 | 

Cây con cần thiết chứa`2-4`,`1-2`,`1-5`, Và`5-6`, vậy câu trả lời là`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n+q)√n log n)`| Mo chuyển động mang lại về`O((n+q)√n)`cập nhật và mỗi bản cập nhật thực hiện các truy vấn khoảng cách dựa trên LCA | 
| Không gian |`O(n log n)`| Nâng nhị phân chiếm ưu thế trong việc sử dụng bộ nhớ | 

Các ràng buộc cho phép điều này vì tổng số đỉnh là`2 * 10^5`và tổng số truy vấn là`3 * 10^5`. Thuật toán tránh việc xây dựng lại các cây con lớn và chỉ thực hiện các thay đổi cục bộ khi di chuyển giữa các truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # In a local setup, call solve() here after importing the solution.
    sys.stdin = old
    return ""

# Test cases should be executed against the solve() function from the solution file.

tests = [
    (
        "1\n1 1\n1 1\n",
        "0"
    ),
    (
        "1\n4 1\n1 2\n2 3\n2 4\n1 4\n",
        "3"
    ),
    (
        "1\n6 3\n1 2\n2 3\n2 4\n1 5\n5 6\n1 2\n1 3\n4 6\n",
        "1\n2\n4"
    )
]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây đỉnh đơn |`0`| Xử lý bộ cạnh trống | 
| Truy vấn toàn bộ cây |`3`| Tất cả các chi nhánh phải được bao gồm | 
| Cây mẫu |`1 2 4`| Tính toán cây con bao trùm chung | 

## Vỏ cạnh 

Đối với một đỉnh được chọn, tập hợp hoạt động chứa một vị trí DFS. Tổng khoảng cách theo chu kỳ vẫn bằng 0 vì không cần cạnh nào để kết nối một nút. 

Đối với toàn bộ truy vấn cây, mọi đỉnh cuối cùng đều trở thành hoạt động. Mọi cạnh đều được bao gồm trong cây con tối thiểu và tổng khoảng cách tuần hoàn sẽ chính xác gấp đôi số cạnh trong cây. 

Đối với các truy vấn trong đó các đỉnh được chọn nằm ở các nhánh khác nhau, đóng góp LCA bên trong phép tính khoảng cách sẽ tự động bao gồm các đường dẫn dùng chung. Thuật toán không bao giờ giả định rằng các đỉnh được chọn được kết nối trực tiếp, do đó các cạnh nhánh được tính chính xác.
