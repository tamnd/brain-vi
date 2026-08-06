---
title: "CF 102565G - Người múa rối"
description: "Chúng ta có một cây có rễ. Mỗi đỉnh có một số duy nhất từ ​​1 đến N, do đó các số này tạo thành một hoán vị của các đỉnh. Với mỗi đỉnh v, chúng ta chỉ xét các đỉnh bên trong cây con của v và thu thập số của chúng. Những con số này tạo ra một số phạm vi liên tiếp tối đa."
date: "2026-08-05T14:19:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102565
codeforces_index: "G"
codeforces_contest_name: "AGM 2020, Final Round, Day 2"
rating: 0
weight: 102565
solve_time_s: 117
verified: true
draft: false
---

[CF 102565G - Người múa rối](https://codeforces.com/problemset/problem/102565/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có rễ. Mỗi đỉnh có một số duy nhất từ ​​1 đến N, do đó các số này tạo thành một hoán vị của các đỉnh. Với mỗi đỉnh v, chúng ta chỉ xét các đỉnh bên trong cây con của v và thu thập số của chúng. Những con số này tạo ra một số phạm vi liên tiếp tối đa. Nhiệm vụ là đếm các phạm vi đó cho mọi đỉnh. 

Ví dụ: nếu một cây con chứa các giá trị`{2,3,4,8,10,11}`, các khoảng rút gọn là`[2,4]`,`[8,8]`, Và`[10,11]`, vậy đáp án cho cây con này là 3. 

Thách thức đến từ việc cần có câu trả lời này cho mọi cây con. Với N đạt 250000 và tổng kích thước của tất cả các thử nghiệm đạt 500000, việc xây dựng lại tập hợp các giá trị cho mọi đỉnh là không thể. Cách tiếp cận bậc hai có thể thực hiện xung quanh các phép toán N2 trên cây hình chuỗi, vượt xa giới hạn hai giây cho phép. Chúng ta cần một giải pháp gần tuyến tính hoặc N log N. 

Các trường hợp phức tạp là do thực tế là các khoảng nhỏ gọn phụ thuộc vào các giá trị lân cận chứ không phụ thuộc trực tiếp vào cấu trúc cây. Một giá trị được chèn có thể hợp nhất hai khoảng hiện có hoặc tạo một khoảng mới. 

Xét một cây có một đỉnh:```
1
```có giá trị`1`. Câu trả lời là:```
1
```Giải pháp đếm các cặp liền kề thay vì phạm vi tối đa sẽ không thành công vì một giá trị duy nhất vẫn là một khoảng nhỏ gọn. 

Một trường hợp khác:```
1
|
2
```với các giá trị:```
[2,3]
```Cây con của đỉnh 1 chứa`{2,3}`, vậy câu trả lời là`1`. Việc triển khai bất cẩn chỉ kiểm tra xem mọi giá trị có giá trị kế tiếp hay không sẽ bỏ lỡ điều đó`[2,3]`là một khoảng đầy đủ 

Một trường hợp quan trọng cuối cùng là sáp nhập:```
values = {1,3}
```Có hai khoảng,`[1,1]`Và`[3,3]`. Thêm giá trị`2`thay đổi câu trả lời từ 2 thành 1 vì giá trị mới kết nối cả hai bên. Bất kỳ công thức cập nhật nào chỉ tính các giá trị riêng biệt mới sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng cây con một cách độc lập. Đối với mỗi đỉnh, chúng tôi thu thập tất cả các giá trị bên dưới nó, sắp xếp chúng và đếm các khoảng cách giữa các số liên tiếp. Điều này đúng vì các khoảng rút gọn chính xác là các nhóm cực đại trong đó các giá trị liên tiếp khác nhau một. 

Tuy nhiên, trong cây chuỗi, cây con gốc chứa N đỉnh, cây con tiếp theo chứa N-1 đỉnh, v.v. Sắp xếp mọi cây con sẽ cho khoảng:```
N + (N-1) + ... + 1 = O(N²)
```các giá trị được xử lý ngay cả trước khi xem xét chi phí sắp xếp. 

Quan sát quan trọng là chúng ta không cần toàn bộ tập hợp được sắp xếp. Khi chúng ta chèn một giá trị x vào một tập hợp được duy trì, chỉ sự hiện diện của x-1 và x+1 mới quan trọng. 

Nếu không có hàng xóm nào tồn tại, x sẽ tạo một khoảng mới. 

Nếu có một hàng xóm tồn tại, x sẽ kéo dài khoảng thời gian hiện có. 

Nếu cả hai hàng xóm tồn tại, x nối ​​hai khoảng thành một. 

Do đó, số khoảng nhỏ gọn có thể được duy trì linh hoạt trong khi duyệt qua các cây con. 

Để trả lời tất cả các truy vấn cây con một cách hiệu quả, chúng tôi sử dụng kỹ thuật DSU trên cây, còn được gọi là hợp nhất từ ​​nhỏ đến lớn. Đối với mỗi đỉnh, chúng tôi giữ cấu trúc dữ liệu của con lớn nhất và sử dụng lại nó. Các cây con nhỏ hơn được tạm thời thêm vào cấu trúc này. Vì mỗi đỉnh chỉ di chuyển O(log N) lần giữa các cấu trúc nên tổng công là O(N log N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2 log N) | O(N) | Quá chậm | 
| DSU trên cây | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lấy gốc cây ở đỉnh 1 và tính kích thước của mỗi cây con. Trong DFS này, lưu trữ con nặng của mỗi đỉnh, nghĩa là con có cây con lớn nhất. 
2. Duyệt cây bằng DSU trên cây. Đầu tiên xử lý mọi light child mà không giữ lại dữ liệu của nó sau đó. Sau đó xử lý trẻ nặng trong khi vẫn giữ nguyên thiết lập đã được duy trì. 
3. Sau khi xử lý cây con nặng, cộng tất cả các đỉnh từ các cây con nhẹ và đỉnh hiện tại vào tập được duy trì. Giá trị hiện tại của bộ đếm khoảng thời gian hiện đại diện cho câu trả lời cho cây con này. 
4. Lưu bộ đếm hiện tại làm đáp án của đỉnh. Nếu cây con này không được đánh dấu là được giữ lại, hãy xóa tất cả các đỉnh của nó khỏi cấu trúc được duy trì để lệnh gọi gốc có thể tiếp tục chính xác. 

Bất biến được duy trì là tập hoạt động luôn chứa chính xác các đỉnh thuộc cây con DSU hiện đang được xử lý. Bộ đếm khoảng thời gian chỉ được cập nhật thông qua các thay đổi cục bộ do thêm hoặc xóa một giá trị, do đó, nó luôn bằng số khoảng thời gian rút gọn trong tập hợp đó. 

Khi chèn x, thay đổi là:```
+1 - (x-1 exists) - (x+1 exists)
```Khi xóa x, sự thay đổi nghịch đảo là:```
-1 + (x-1 exists) + (x+1 exists)
```Các công thức này bao gồm tất cả các trường hợp, bao gồm cả khoảng thời gian chia tách và hợp nhất. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(1 << 20)

input = sys.stdin.readline

def solve():
    n = int(input())
    val = [0] + list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    size = [1] * (n + 1)
    heavy = [0] * (n + 1)
    parent = [0] * (n + 1)

    def dfs(v, p):
        parent[v] = p
        best = 0
        for u in g[v]:
            if u == p:
                continue
            dfs(u, v)
            size[v] += size[u]
            if size[u] > best:
                best = size[u]
                heavy[v] = u

    dfs(1, 0)

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    order = []

    def euler(v, p):
        tin[v] = len(order)
        order.append(v)
        for u in g[v]:
            if u != p:
                euler(u, v)
        tout[v] = len(order)

    euler(1, 0)

    present = [False] * (n + 2)
    ans = [0] * (n + 1)
    current = 0

    def add_vertex(v):
        nonlocal current
        x = val[v]
        current += 1
        if present[x - 1]:
            current -= 1
        if present[x + 1]:
            current -= 1
        present[x] = True

    def remove_vertex(v):
        nonlocal current
        x = val[v]
        present[x] = False
        current -= 1
        if present[x - 1]:
            current += 1
        if present[x + 1]:
            current += 1

    def add_subtree(v):
        for i in range(tin[v], tout[v]):
            add_vertex(order[i])

    def remove_subtree(v):
        for i in range(tin[v], tout[v]):
            remove_vertex(order[i])

    def dfs2(v, p, keep):
        for u in g[v]:
            if u != p and u != heavy[v]:
                dfs2(u, v, False)

        if heavy[v]:
            dfs2(heavy[v], v, True)

        for u in g[v]:
            if u != p and u != heavy[v]:
                add_subtree(u)

        add_vertex(v)
        ans[v] = current

        if not keep:
            remove_subtree(v)

    dfs2(1, 0, True)

    print(*ans[1:])

t = int(input())
for _ in range(t):
    solve()
```DFS đầu tiên tính toán kích thước cây con và xác định con nặng. DFS thứ hai là truyền tải DSU trên cây. các`present`mảng lưu trữ xem một giá trị hiện có nằm trong cây con đang hoạt động hay không. 

Bộ đếm khoảng thời gian được cập nhật trước khi thay đổi`present[x]`trong quá trình chèn vì các hàng xóm cũ mô tả các khoảng hiện có. Trong quá trình xóa, giá trị sẽ bị xóa trước tiên và sau đó các giá trị lân cận sẽ được kiểm tra vì bản thân giá trị đó không còn được coi là hiện diện nữa. 

Số nguyên Python có độ chính xác tùy ý, do đó không cần xử lý tràn. Giới hạn đệ quy được tăng lên vì một cây có thể là một chuỗi có độ sâu N. Thứ tự Euler cho phép thêm hoặc xóa toàn bộ cây con bằng cách lặp qua một đoạn liền kề.
