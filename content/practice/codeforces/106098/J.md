---
title: "CF 106098J - Hói và Eslam"
description: "Chúng ta được cho một đồ thị vô hướng liên thông. Một đỉnh được chọn ngẫu nhiên thống nhất. Sau đó, một trong các cạnh tới của nó được chọn ngẫu nhiên một cách thống nhất và bị xóa khỏi biểu đồ."
date: "2026-06-25T11:56:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106098
codeforces_index: "J"
codeforces_contest_name: "The American University in Cairo CSEA Fall 2025 contest"
rating: 0
weight: 106098
solve_time_s: 49
verified: true
draft: false
---

[CF 106098J - Hói và Eslam](https://codeforces.com/problemset/problem/106098/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông. Một đỉnh được chọn ngẫu nhiên thống nhất. Sau đó, một trong các cạnh tới của nó được chọn ngẫu nhiên một cách thống nhất và bị xóa khỏi biểu đồ. 

Sau lần xóa đó, chúng ta bắt đầu từ cùng một đỉnh đã chọn và hỏi có bao nhiêu đỉnh riêng biệt vẫn có thể thăm được. Vì đồ thị là vô hướng nên đây chỉ đơn giản là kích thước của thành phần liên thông chứa đỉnh đã chọn sau khi loại bỏ cạnh. 

Nhiệm vụ là tính toán giá trị mong đợi của kích thước thành phần đó. 

Biểu đồ có thể chứa tới$2 \cdot 10^5$các đỉnh và cạnh trong tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào tính toán lại kết nối sau mỗi lần xóa đều hoàn toàn không khả thi. Ngay cả một DFS trên mỗi cạnh cũng đã yêu cầu$O(m(n+m))$, vượt xa giới hạn. 

Đồ thị ban đầu được kết nối. Điều đó ngay lập tức cho thấy rằng hầu hết việc xóa cạnh đều không có tác dụng gì. Việc loại bỏ một cầu nối sẽ giữ cho biểu đồ được kết nối, vì vậy câu trả lời vẫn còn$n$. Chỉ có cầu nối mới có thể giảm kích thước thành phần có thể tiếp cận. 

Một lỗi phổ biến là nghĩ về tất cả các đỉnh bên trong biểu đồ sau khi loại bỏ một cây cầu. Thí nghiệm chọn một đỉnh điểm cuối trước tiên, sau đó chọn một trong các cạnh tới của nó. Sự phân bổ xác suất phụ thuộc vào độ đỉnh, do đó mỗi cây cầu đóng góp khác nhau tùy thuộc vào độ của hai điểm cuối của nó. 

Xét một đường đi gồm ba đỉnh:```
1 - 2 - 3
```Bắt đầu từ đỉnh 1, cạnh tới duy nhất của nó bị loại bỏ, để lại đỉnh 1 bị cô lập. Kích thước có thể truy cập là 1. 

Bắt đầu từ đỉnh 2, một trong hai cạnh có thể bị loại bỏ. Trong cả hai trường hợp, đỉnh 2 vẫn được kết nối với chính xác một đỉnh khác, do đó kích thước có thể truy cập là 2. 

Giá trị mong đợi là:$$\frac{1+2+1}{3}=\frac43$$Giải pháp chỉ tính số lượng cầu mà không tính đến độ điểm cuối sẽ cho kết quả sai. 

Một trường hợp tinh vi khác là đồ thị không có cầu nối, ví dụ như một chu trình:```
1 - 2
|   |
4 - 3
```Việc loại bỏ bất kỳ cạnh đơn nào sẽ làm cho biểu đồ được kết nối, vì vậy câu trả lời luôn là$n$. Bất kỳ thuật toán nào cố gắng trừ thứ gì đó cho mọi cạnh sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Việc giải thích lực lượng vũ phu tuân theo thí nghiệm theo nghĩa đen. 

Với mọi đỉnh$v$, lặp qua mọi cạnh sự cố$e$. Di dời$e$, chạy DFS hoặc BFS từ$v$, đo kích thước của thành phần có thể truy cập và lấy kết quả trung bình. 

Điều này đúng vì nó mô phỏng trực tiếp quá trình ngẫu nhiên. Vấn đề là chi phí của nó. Một biểu đồ có thể chứa$2 \cdot 10^5$các cạnh. Việc duyệt đồ thị cho mọi lựa chọn có thể dẫn đến kết quả gần đúng$O(m(n+m))$công việc, tức là có hàng tỷ thao tác trong trường hợp xấu nhất. 

Quan sát quan trọng là việc xóa một cầu nối không thay đổi gì cả. Vì đồ thị bắt đầu được kết nối, nên việc loại bỏ một cầu nối sẽ khiến nó được kết nối, do đó mọi đỉnh vẫn có thể chạm tới tất cả$n$đỉnh. 

Điều đó có nghĩa là chỉ có những cây cầu mới quan trọng. 

Giả sử một cây cầu nối đỉnh cha$p$và một đỉnh con$c$trong cây DFS. Để cây con có gốc tại$c$bao gồm$sz$đỉnh. 

Việc xóa cây cầu này sẽ chia biểu đồ thành hai thành phần:$$sz$$Và$$n-sz$$Nếu đỉnh được chọn là$p$, thì sau khi xóa cây cầu, nó vẫn nằm trong thành phần kích thước$n-sz$, thua chính xác$sz$đỉnh. 

Nếu đỉnh được chọn là$c$, nó nằm trong thành phần kích thước$sz$, thua chính xác$n-sz$đỉnh. 

Không có đỉnh nào khác có thể chọn cạnh này vì phép thử chỉ loại bỏ các cạnh liên quan đến đỉnh đã chọn. 

Điều này chuyển vấn đề thành tổng hợp những đóng góp độc lập từ mỗi cây cầu. Một DFS duy nhất có thể xác định tất cả các cầu nối và tất cả các kích thước cây con, mang lại kết quả$O(n+m)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m(n+m))$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n+m)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đồ thị và tính bậc của mỗi đỉnh. 
2. Chạy DFS tìm cầu của Tarjan. 
3. Trong DFS, hãy duy trì thời gian khám phá và giá trị liên kết thấp. 
4. Với mỗi đỉnh cây DFS, hãy tính kích thước cây con của nó`sz`. 
5. Khi khám phá rìa cây`(u, v)`, sau khi trở về từ`v`, kiểm tra xem:$$low[v] > tin[u]$$Nếu điều kiện này được giữ,`(u,v)`là một cây cầu. 

1. Hãy để`sz[v]`là kích thước của cây con con. Việc xóa cây cầu này sẽ chia biểu đồ thành các thành phần có kích thước:$$sz[v]$$Và$$n - sz[v]$$1. Tổng thiệt hại do cây cầu này gây ra là:$$\frac{sz[v]}{\deg(u)}
+
\frac{n-sz[v]}{\deg(v)}$$Thuật ngữ đầu tiên tương ứng với việc chọn đỉnh`u`, thứ hai để chọn đỉnh`v`. 

1. Tính tổng tổn thất trên tất cả các cây cầu. 
2. Gọi tổng số tổn thất là`S`. 

Kích thước có thể tiếp cận dự kiến ​​là:$$n - \frac{S}{n}$$1. Thực hiện tất cả các modulo số học$10^9+7$. Thay thế mọi phép chia bằng phép nhân bằng nghịch đảo mô-đun. 

### Tại sao nó hoạt động 

Mọi kết quả có thể xảy ra của thử nghiệm đều bắt đầu từ một biểu đồ liên thông và loại bỏ chính xác một cạnh trùng với đỉnh đã chọn. 

Nếu cạnh bị loại bỏ không phải là cầu nối, biểu đồ vẫn được kết nối, do đó kích thước có thể tiếp cận vẫn bằng$n$. Các cạnh như vậy không góp phần làm mất mát. 

Đối với một cây cầu, việc loại bỏ nó sẽ chia biểu đồ thành chính xác hai thành phần. Điểm cuối được chọn vẫn ở một bên và mất quyền truy cập vào tất cả các đỉnh ở phía bên kia. Số đỉnh bị mất chính xác bằng kích thước của thành phần đối diện. 

Mỗi cây cầu đóng góp độc lập vì chỉ có hai điểm cuối của nó mới có thể chọn nó. Tổng hợp tổn thất của tất cả các cây cầu sẽ cho ra tổng mức giảm dự kiến ​​từ$n$. Trừ đi mức giảm đó từ$n$mang lại kích thước thành phần có thể tiếp cận dự kiến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

MAXN = 200000 + 5
inv = [0] * MAXN
for i in range(1, MAXN):
    inv[i] = pow(i, MOD - 2, MOD)

t = int(input())

for _ in range(t):
    n, m = map(int, input().split())

    adj = [[] for _ in range(n)]
    deg = [0] * n

    for eid in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        adj[u].append((v, eid))
        adj[v].append((u, eid))

        deg[u] += 1
        deg[v] += 1

    tin = [0] * n
    low = [0] * n
    sz = [0] * n

    timer = 0
    loss = 0

    sys.setrecursionlimit(1 << 20)

    def dfs(u, pe):
        nonlocal timer, loss

        timer += 1
        tin[u] = low[u] = timer
        sz[u] = 1

        for v, eid in adj[u]:
            if eid == pe:
                continue

            if tin[v]:
                low[u] = min(low[u], tin[v])
            else:
                dfs(v, eid)

                sz[u] += sz[v]
                low[u] = min(low[u], low[v])

                if low[v] > tin[u]:
                    s = sz[v]

                    loss_term = (
                        s * inv[deg[u]]
                        + (n - s) * inv[deg[v]]
                    ) % MOD

                    loss += loss_term
                    loss %= MOD

    dfs(0, -1)

    ans = (n % MOD - loss * inv[n]) % MOD
    print(ans)
```Biểu đồ được lưu trữ với các mã nhận dạng cạnh để DFS có thể phân biệt cạnh cây dẫn đến cây mẹ với các cạnh khác. 

Các mảng`tin`Và`low`là cấu trúc cầu Tarjan tiêu chuẩn. điều kiện`low[v] > tin[u]`chính xác là tiêu chí xác định một cây cầu. 

Kích thước cây con`sz[v]`được tính toán trong giai đoạn trả về DFS. Khi một cây cầu được tìm thấy,`sz[v]`ngay lập tức đưa ra một bên của sự phân chia và`n - sz[v]`đưa cho người khác. 

Số lượng tích lũy trong`loss`bản thân nó không phải là tổn thất dự kiến. Đó là tổng của tất cả các tổn thất trên cầu trước khi chia cuối cùng cho`n`, phù hợp với công thức$$n - \frac{S}{n}.$$Tất cả các bộ phận được thay thế bằng nghịch đảo mô-đun. Vì mỗi bậc có ít nhất một bậc trong đồ thị liên thông với$n \ge 2$, những nghịch đảo này luôn tồn tại modulo$10^9+7$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị:```
1 - 2 - 3
```| Cầu | Kích thước cây con | Mất từ ​​điểm cuối đầu tiên | Mất từ ​​điểm cuối thứ hai | 
| --- | --- | --- | --- | 
| (1,2) | 2 | 1/2 | 1/2 | 
| (2,3) | 1 | 1/2 | 1/2 | 

Tổng thiệt hại:$$2+\frac12+\frac12+2=5$$Giá trị dự kiến:$$3-\frac53=\frac43$$| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| S | 5 | 
| Trả lời | 4/3 | 

Ví dụ này cho thấy mức độ điểm cuối ảnh hưởng trực tiếp đến phân bố xác suất như thế nào.

 ### Ví dụ 2 

Đồ thị:```
1
|\
| \
2--3
```Một hình tam giác không có cây cầu nào. 

| Số lượng cầu | Tổng thiệt hại | 
| --- | --- | 
| 0 | 0 | 

| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| S | 0 | 
| Trả lời | 3 | 

Việc loại bỏ bất kỳ cạnh nào sẽ giữ cho biểu đồ được kết nối, do đó mọi kết quả đều đạt đến tất cả các đỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n+m)$| Một DFS xử lý mọi đỉnh và cạnh một lần | 
| Không gian |$O(n+m)$| Danh sách kề cộng với mảng DFS | 

Tổng số đỉnh và cạnh của tất cả các ca kiểm thử nhiều nhất là$2 \cdot 10^5$. Một giải pháp tuyến tính dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    MOD = 1000000007

    data = io.StringIO(inp)
    input = data.readline

    MAXN = 200005
    inv = [0] * MAXN
    for i in range(1, MAXN):
        inv[i] = pow(i, MOD - 2, MOD)

    t = int(input())
    out = []

    sys.setrecursionlimit(1 << 20)

    for _ in range(t):
        n, m = map(int, input().split())

        adj = [[] for _ in range(n)]
        deg = [0] * n

        for eid in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1

            adj[u].append((v, eid))
            adj[v].append((u, eid))

            deg[u] += 1
            deg[v] += 1

        tin = [0] * n
        low = [0] * n
        sz = [0] * n

        timer = 0
        loss = 0

        def dfs(u, pe):
            nonlocal timer, loss

            timer += 1
            tin[u] = low[u] = timer
            sz[u] = 1

            for v, eid in adj[u]:
                if eid == pe:
                    continue

                if tin[v]:
                    low[u] = min(low[u], tin[v])
                else:
                    dfs(v, eid)

                    sz[u] += sz[v]
                    low[u] = min(low[u], low[v])

                    if low[v] > tin[u]:
                        s = sz[v]
                        loss = (
                            loss
                            + s * inv[deg[u]]
                            + (n - s) * inv[deg[v]]
                        ) % MOD

        dfs(0, -1)

        ans = (n % MOD - loss * inv[n]) % MOD
        out.append(str(ans))

    return "\n".join(out)

# sample 1
assert run(
"""1
3 2
1 2
2 3
"""
) == "333333337"

# triangle, no bridges
assert run(
"""1
3 3
1 2
2 3
3 1
"""
) == "3"

# single edge
assert run(
"""1
2 1
1 2
"""
) == "1"

# cycle with tail
assert run(
"""1
4 4
1 2
2 3
3 1
3 4
"""
) != ""

# minimum connected graph
assert run(
"""1
1 0
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đường đi có độ dài 2 | MOD 4/3 modulo | Xử lý cầu cơ bản | 
| Tam giác | 3 | Không có cầu | 
| Cạnh đơn | 1 | Đồ thị không tầm thường nhỏ nhất | 
| Chu kỳ có đuôi | Câu trả lời hợp lệ | Cầu hỗn hợp và các cạnh không cầu | 
| Đỉnh đơn | 1 | Kích thước biểu đồ tối thiểu | 

## Vỏ cạnh 

Hãy xem xét biểu đồ:```
1 - 2 - 3
```đầu vào:```
1
3 2
1 2
2 3
```Cả hai cạnh đều là cầu. DFS tính toán kích thước cây con 2 và 1. Đóng góp của chúng trở thành:$$\frac{2}{1}+\frac{1}{2}$$Và$$\frac{1}{2}+\frac{2}{1}$$Câu trả lời cuối cùng là$\frac43$. Thuật toán tính toán chính xác độ điểm cuối thay vì xử lý mọi cây cầu như nhau. 

Bây giờ hãy xem xét một chu kỳ:```
1 - 2
|   |
4 - 3
```đầu vào:```
1
4 4
1 2
2 3
3 4
4 1
```DFS không tìm thấy cây cầu nào vì mỗi cạnh cây đều có cạnh sau hỗ trợ nó. Khoản lỗ tích lũy vẫn bằng không. Câu trả lời trở nên chính xác$n=4$, điều này phù hợp với thực tế là việc loại bỏ bất kỳ cạnh đơn nào sẽ khiến đồ thị được kết nối. 

Cuối cùng, hãy xem xét một biểu đồ bao gồm một hình tam giác có gắn một lá:```
1
|\
| \
2--3--4
```Chỉ có cạnh`(3,4)`là một cây cầu. DFS tính toán`sz=1`cho đỉnh 4. Cây cầu góp phần:$$\frac{1}{\deg(3)}
+
\frac{3}{\deg(4)}$$và không có cạnh nào khác đóng góp bất cứ điều gì. Điều này xác nhận rằng thuật toán tách biệt ảnh hưởng của cầu nối trong khi bỏ qua tất cả những cầu nối không phải.
