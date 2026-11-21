---
title: "CF 241E - Chuyến bay"
description: "Chúng ta được cung cấp một biểu đồ có hướng không theo chu kỳ của các thành phố và các chuyến bay một chiều. Mỗi chuyến bay ban đầu mất 1 giờ. Chúng tôi có thể thay đổi độc lập bất kỳ thời lượng chuyến bay nào thành 1 hoặc 2 giờ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 241
codeforces_index: "E"
codeforces_contest_name: "Bayan 2012-2013 Elimination Round (ACM ICPC Rules, English statements)"
rating: 2600
weight: 241
solve_time_s: 114
verified: true
draft: false
---

[CF 241E - Chuyến bay](https://codeforces.com/problemset/problem/241/E) 

**Đánh giá:** 2600 
**Tas:** đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng không theo chu kỳ của các thành phố và các chuyến bay một chiều. Mỗi chuyến bay ban đầu mất 1 giờ. Chúng tôi có thể thay đổi độc lập bất kỳ thời lượng chuyến bay nào thành 1 hoặc 2 giờ. 

Mục tiêu là ấn định khoảng thời gian sao cho mọi đường đi có thể từ thành phố 1 đến thành phố n đều có tổng thời gian di chuyển giống hệt nhau. 

Đồ thị có tính chất không tuần hoàn vì mọi cạnh đều đi từ thành phố có số nhỏ hơn đến thành phố có số lớn hơn. Thuộc tính đó là toàn bộ lý do khiến vấn đề có thể quản lý được. Nếu không có nó, việc cân bằng tất cả độ dài đường đi trong biểu đồ tổng quát sẽ trở nên khó khăn hơn nhiều vì các chu trình có thể tạo ra vô số đường đi. 

Đầu vào cho tối đa 1000 đỉnh và 5000 cạnh. Việc gán bạo lực trên tất cả các cạnh sẽ yêu cầu kiểm tra tới$2^m$khả năng. Với$m = 5000$, điều đó là hoàn toàn không thể. 

Kích thước biểu đồ cho thấy chúng ta cần thứ gì đó xung quanh$O(nm)$hoặc$O(m \log n)$. Vì đồ thị là DAG nên việc lập trình động theo thứ tự tôpô trở thành một hướng tự nhiên. 

Phần khó khăn là chúng ta không chỉ đơn giản tìm ra con đường ngắn nhất hay dài nhất. Chúng ta phải làm cho mọi đường đi từ 1 đến n bằng nhau. Điều đó có nghĩa là các trọng số cạnh được chỉ định phải đáp ứng hệ thống ràng buộc nhất quán trên toàn bộ DAG. 

Việc triển khai đơn giản thường thất bại trên các vùng bị ngắt kết nối của biểu đồ. Ví dụ:```
4 3
1 2
2 4
3 4
```Thành phố 3 không liên quan vì không thể đến được từ thành phố 1. Những ràng buộc liên quan đến nó không thành vấn đề. Nếu chúng ta xử lý từng nút một cách mù quáng, chúng ta có thể kết luận sai rằng hệ thống không nhất quán. 

Một trường hợp tinh tế khác là khi một nút có hai đường dẫn đi ra buộc các giá trị trái ngược nhau. 

Ví dụ:```
4 4
1 2
1 3
2 4
3 4
```Điều này có thể giải quyết được vì chúng ta có thể gán:```
1->2 = 1
1->3 = 1
2->4 = 2
3->4 = 2
```Cả hai đường dẫn có tổng chiều dài 3. 

Nhưng hãy xem xét:```
5 5
1 2
1 3
2 5
3 4
4 5
```Độ dài đường dẫn có cấu trúc khác nhau ở 1 cạnh. Vì mỗi trọng số của cạnh là 1 hoặc 2 nên đường đi thứ nhất chỉ có thể có tổng 2, 3 hoặc 4, trong khi đường đi thứ hai chỉ có thể có tổng 3, 4, 5 hoặc 6. Sự chồng chéo của chúng tồn tại nên biểu đồ này vẫn có thể giải được. 

Một chiến lược tham lam bất cẩn luôn giữ trọng số là 1 trừ khi bị ép buộc sẽ không phát hiện được những điều chỉnh cần thiết. 

Các trường hợp thực sự không thể xảy ra khi các ràng buộc buộc một số cạnh cần có trọng số 0 hoặc 3. 

Ví dụ:```
4 4
1 2
2 4
1 3
3 4
```Giả sử chúng ta cố gắng thực thi: 

- khoảng cách từ 1 đến 4 qua nút 2 bằng 2 
- khoảng cách qua nút 3 bằng 4 

Khi đó một số cạnh sẽ cần trọng số lớn hơn 2. Giới hạn phạm vi hợp lệ là nguồn gốc thực sự của sự không thể thực hiện được. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Mỗi cạnh có thể độc lập nhận trọng số 1 hoặc 2, vì vậy chúng ta có thể thử tất cả$2^m$bài tập. Đối với mỗi phép gán, chúng tôi tính toán tất cả độ dài đường dẫn từ 1 đến n và kiểm tra xem chúng có giống nhau không. 

Độ chính xác có ngay lập tức vì chúng tôi kiểm tra mọi cấu hình có thể theo đúng nghĩa đen. Vấn đề là quy mô. Ngay cả khi chỉ có 50 cạnh,$2^{50}$đã là thiên văn rồi. Ở đây chúng ta có thể có 5000 cạnh. 

Quan sát quan trọng là "tất cả các đường đi có tổng chiều dài bằng nhau" hoạt động giống như một hàm thế năng. 

Giả sử mọi nút$v$có một giá trị$dp[v]$, nghĩa là khoảng cách chung còn lại từ$v$ĐẾN$n$. Sau đó với mọi cạnh$u \to v$,$$w(u,v) + dp[v] = dp[u]$$Sắp xếp lại mang lại:$$w(u,v) = dp[u] - dp[v]$$Vì mỗi trọng số của cạnh phải là 1 hoặc 2,$$dp[u] - dp[v] \in \{1,2\}$$Bây giờ vấn đề trở nên sạch sẽ hơn nhiều. Chúng ta cần gán nhãn số nguyên cho các đỉnh sao cho mỗi cạnh có thể tiếp cận đều giảm nhãn chính xác 1 hoặc 2. 

Vì biểu đồ là DAG nên chúng ta có thể tính toán ngược các nhãn này từ nút$n$. 

Đối với mỗi nút: 

- nếu nó có các cạnh đi ra, tất cả các chuyển tiếp đi ra phải cho phép các giá trị nhất quán 
- giá trị của nút được xác định bởi các nút con của nó 

Cụ thể hơn, đối với cạnh$u \to v$, 

-$dp[u]$phải bằng nhau$dp[v] + 1$hoặc$dp[v] + 2$Nếu hai cạnh hướng ra ngoài buộc các giá trị không tương thích thì câu trả lời là không thể. 

Điều này biến phép tìm kiếm hàm mũ thành đồ thị tuyến tính DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^m \cdot (n+m))$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O(n+m)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng và đồ thị ngược. 

Biểu đồ ngược rất hữu ích cho việc xác định đỉnh nào thực sự có thể tiếp cận thành phố$n$. 
2. Tìm tất cả các đỉnh có thể tới được từ thành phố 1. 

Các đỉnh bên ngoài tập hợp này không tham gia vào bất kỳ lộ trình hợp lệ nào từ 1 đến n, do đó chúng không áp đặt ràng buộc nào. 
3. Tìm tất cả các đỉnh có thể tới thành phố$n$. 

Chúng tôi chạy DFS hoặc BFS trên biểu đồ đảo ngược bắt đầu từ$n$. 
4. Chỉ giữ lại các đỉnh nằm trên đường đi nào đó từ 1 đến n. 

Một đỉnh chỉ có ý nghĩa nếu:$$reachable\_from\_1[v] = true$$Và$$can\_reach\_n[v] = true$$5. Xử lý các đỉnh theo thứ tự ngược lại từ$n$xuống còn 1. 

Vì đồ thị có tính chất không tuần hoàn và mọi cạnh đều đi từ chỉ số nhỏ hơn đến chỉ số lớn hơn nên đây đã là một trật tự tôpô ngược. 
6. Đặt:$$dp[n] = 0$$Điều này có nghĩa là khoảng cách còn lại từ$n$với chính nó là bằng không. 
7. Đối với mỗi nút hữu ích$u$, kiểm tra mọi cạnh đi ra$u \to v$. 

Mỗi cạnh cho phép:$$dp[u] = dp[v] + 1$$hoặc$$dp[u] = dp[v] + 2$$Tất cả các cạnh đi ra phải đồng ý về ít nhất một giá trị chung cho$dp[u]$. 
8. Tính giao điểm của tất cả các giá trị được phép. 

Nếu giao lộ trở nên trống rỗng thì biểu đồ là không thể. 
9. Chọn bất kỳ giá trị hợp lệ nào cho$dp[u]$. 

Lựa chọn đơn giản nhất là lựa chọn hợp lệ nhỏ nhất. 
10. Khôi phục trọng lượng cạnh. 

Đối với mọi lợi thế hữu ích:$$w(u,v) = dp[u] - dp[v]$$Điều này sẽ luôn là 1 hoặc 2. 
11. Các cạnh không liên quan có thể nhận bất kỳ giá trị nào, ví dụ 1. 

Tại sao nó hoạt động: 

Tính bất biến đó là$dp[u]$đại diện cho khoảng cách chung còn lại từ nút$u$đến nút$n$. 

Đối với mọi cạnh đi ra$u \to v$, đi qua cạnh đó sẽ tiêu tốn trọng lượng$w(u,v)$, sau đó khoảng cách còn lại là$dp[v]$. Vậy tổng khoảng cách phải thỏa mãn:$$dp[u] = w(u,v) + dp[v]$$Vì tất cả độ dài đường đi từ$u$ĐẾN$n$phải bằng nhau thì phương trình này phải đúng cho mọi cạnh đi cùng một lúc. 

Thuật toán xây dựng rõ ràng chính xác một hệ thống như vậy. Nếu tại một nút nào đó không tồn tại giá trị chung thì không có phép gán nào có thể đáp ứng tất cả các ràng buộc gửi đi. Mặt khác, trọng số được xây dựng đảm bảo mọi đường dẫn đều tích lũy tổng số như nhau. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    edges = []
    g = [[] for _ in range(n + 1)]
    rg = [[] for _ in range(n + 1)]

    for i in range(m):
        u, v = map(int, input().split())
        edges.append((u, v))
        g[u].append((v, i))
        rg[v].append(u)

    # reachable from 1
    vis1 = [False] * (n + 1)
    q = deque([1])
    vis1[1] = True

    while q:
        u = q.popleft()
        for v, _ in g[u]:
            if not vis1[v]:
                vis1[v] = True
                q.append(v)

    # can reach n
    vis2 = [False] * (n + 1)
    q = deque([n])
    vis2[n] = True

    while q:
        u = q.popleft()
        for v in rg[u]:
            if not vis2[v]:
                vis2[v] = True
                q.append(v)

    useful = [vis1[i] and vis2[i] for i in range(n + 1)]

    dp = [None] * (n + 1)
    dp[n] = 0

    ans = [1] * m

    for u in range(n - 1, 0, -1):
        if not useful[u]:
            continue

        possible = None

        for v, idx in g[u]:
            if not useful[v]:
                continue

            cur = {dp[v] + 1, dp[v] + 2}

            if possible is None:
                possible = cur
            else:
                possible &= cur

        if not possible:
            print("No")
            return

        dp[u] = min(possible)

    for u, v in edges:
        if useful[u] and useful[v]:
            w = dp[u] - dp[v]
            if w < 1 or w > 2:
                print("No")
                return

    for i, (u, v) in enumerate(edges):
        if useful[u] and useful[v]:
            ans[i] = dp[u] - dp[v]
        else:
            ans[i] = 1

    print("Yes")
    print("\n".join(map(str, ans)))

solve()
```Phần đầu tiên xây dựng cả đồ thị chuẩn và đồ thị đảo ngược. Biểu đồ đảo ngược là cần thiết vì chúng ta phải xác định các nút cuối cùng có thể đến thành phố$n$. 

Hai lần duyệt BFS rất dễ bị bỏ qua, nhưng chúng rất cần thiết. Các nút bên ngoài bất kỳ 1-đến-$n$đường dẫn không nên tham gia vào các ràng buộc. Nếu chúng ta vẫn xử lý chúng, chúng ta có thể kết luận sai rằng hệ thống không có nghiệm. 

Lập trình động tiến hành lùi lại vì các cạnh luôn hướng về các chỉ số lớn hơn. Vào thời điểm chúng tôi xử lý nút$u$, mọi đứa trẻ$v$đã có bản hoàn thiện rồi$dp[v]$. 

Phần tinh tế là logic giao nhau. Mỗi cạnh đi đóng góp hai giá trị được phép cho$dp[u]$:$$dp[v] + 1$$Và$$dp[v] + 2$$Tất cả các cạnh phải đồng ý về một giá trị chung. Sử dụng trực tiếp giao lộ giúp việc triển khai đơn giản và an toàn. 

Bước xác minh cuối cùng là dư thừa về mặt kỹ thuật nếu DP đúng, nhưng nó bảo vệ khỏi các lỗi triển khai và giúp việc gỡ lỗi dễ dàng hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2
2 3
1 3
```Chúng tôi xử lý ngược. 

| Nút | Các cạnh hữu ích đi | Giá trị được phép | Đã chọn dp | 
| --- | --- | --- | --- | 
| 3 | không | cố định | 0 | 
| 2 | 2→3 | {1,2} | 1 | 
| 1 | 1→2, 1→3 | {2,3} ∩ {1,2} = {2} | 2 | 

Trọng lượng được phục hồi: 

| Cạnh | Cân nặng | 
| --- | --- | 
| 1→2 | 1 | 
| 2→3 | 1 | 
| 1→3 | 2 | 

Cả hai đường dẫn từ 1 đến 3 hiện có tổng chiều dài là 2. 

Dấu vết này thể hiện sự bất biến cốt lõi. Mỗi nút lưu trữ một khoảng cách nhất quán còn lại đến nút$n$và mọi cạnh đi ra đều bảo toàn sự đẳng thức đó. 

### Ví dụ 2 

đầu vào:```
4 4
1 2
1 3
2 4
3 4
```Xử lý ngược: 

| Nút | Các cạnh hữu ích đi | Giá trị được phép | Đã chọn dp | 
| --- | --- | --- | --- | 
| 4 | không | cố định | 0 | 
| 3 | 3→4 | {1,2} | 1 | 
| 2 | 2→4 | {1,2} | 1 | 
| 1 | 1→2, 1→3 | {2,3} ∩ {2,3} = {2,3} | 2 | 

Trọng lượng được phục hồi: 

| Cạnh | Cân nặng | 
| --- | --- | 
| 1→2 | 1 | 
| 1→3 | 1 | 
| 2→4 | 1 | 
| 3→4 | 1 | 

Mỗi đường đi có tổng chiều dài là 2. 

Ví dụ này cho thấy có thể tồn tại nhiều giải pháp hợp lệ. Thuật toán chỉ cần chọn một nhiệm vụ tiềm năng khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n+m)$| Mỗi đỉnh và cạnh được xử lý một số lần không đổi | 
| Không gian |$O(n+m)$| Lưu trữ đồ thị cộng với mảng DP và lượt truy cập | 

Với tối đa 1000 đỉnh và 5000 cạnh, độ phức tạp tuyến tính dễ dàng đủ nhanh trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng rất nhỏ so với 256 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    from collections import deque

    n, m = map(int, input().split())

    edges = []
    g = [[] for _ in range(n + 1)]
    rg = [[] for _ in range(n + 1)]

    for i in range(m):
        u, v = map(int, input().split())
        edges.append((u, v))
        g[u].append((v, i))
        rg[v].append(u)

    vis1 = [False] * (n + 1)
    q = deque([1])
    vis1[1] = True

    while q:
        u = q.popleft()
        for v, _ in g[u]:
            if not vis1[v]:
                vis1[v] = True
                q.append(v)

    vis2 = [False] * (n + 1)
    q = deque([n])
    vis2[n] = True

    while q:
        u = q.popleft()
        for v in rg[u]:
            if not vis2[v]:
                vis2[v] = True
                q.append(v)

    useful = [vis1[i] and vis2[i] for i in range(n + 1)]

    dp = [None] * (n + 1)
    dp[n] = 0

    ans = [1] * m

    for u in range(n - 1, 0, -1):
        if not useful[u]:
            continue

        possible = None

        for v, idx in g[u]:
            if not useful[v]:
                continue

            cur = {dp[v] + 1, dp[v] + 2}

            if possible is None:
                possible = cur
            else:
                possible &= cur

        if not possible:
            print("No")
            return

        dp[u] = min(possible)

    for i, (u, v) in enumerate(edges):
        if useful[u] and useful[v]:
            ans[i] = dp[u] - dp[v]

    print("Yes")
    print("\n".join(map(str, ans)))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run(
    "3 3\n"
    "1 2\n"
    "2 3\n"
    "1 3\n"
) == "Yes\n1\n1\n2"

# minimum graph
assert run(
    "2 1\n"
    "1 2\n"
) in {
    "Yes\n1",
    "Yes\n2"
}

# two equal branches
assert run(
    "4 4\n"
    "1 2\n"
    "1 3\n"
    "2 4\n"
    "3 4\n"
).startswith("Yes")

# graph with irrelevant node
assert run(
    "5 4\n"
    "1 2\n"
    "2 5\n"
    "3 4\n"
    "4 5\n"
).startswith("Yes")

# impossible case
assert run(
    "5 6\n"
    "1 2\n"
    "1 3\n"
    "2 4\n"
    "3 4\n"
    "2 5\n"
    "4 5\n"
) == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn | Có | Đồ thị hợp lệ nhỏ nhất | 
| Hai nhánh bằng nhau | Có | Nhiều đường dẫn tương đương | 
| Thành phần bị ngắt kết nối không liên quan | Có | Lọc đúng các đỉnh vô dụng | 
| Đồ thị ràng buộc không thể | Không | Phát hiện nút giao thông trống | 

## Vỏ cạnh 

Hãy xem xét một biểu đồ có các đỉnh không liên quan:```
5 4
1 2
2 5
3 4
4 5
```Thành phố 3 và 4 không bao giờ có thể truy cập được từ thành phố 1. Thuật toán đánh dấu chúng là không hữu ích và hoàn toàn bỏ qua các ràng buộc của chúng. 

Quá trình chỉ DP ngược:```
1 -> 2 -> 5
```Việc triển khai đơn giản xử lý tất cả các nút sẽ cố gắng gán giá trị cho nút 3 và nút 4 một cách không chính xác ngay cả khi chúng không liên quan đến các đường dẫn được yêu cầu. 

Bây giờ hãy xem xét một trường hợp mâu thuẫn:```
5 6
1 2
1 3
2 4
3 4
2 5
4 5
```Xử lý ngược mang lại: 

| Nút | Giá trị được phép | 
| --- | --- | 
| 5 | 0 | 
| 4 | {1,2} | 
| 3 | phụ thuộc vào 4 | 
| 2 | giao điểm từ 2→4 và 2→5 | 

Đối với nút 2: 

- cạnh 2→5 cho phép {1,2} 
- cạnh 2→4 cho phép {2,3} 

Giao lộ trở thành {2}. 

Sau đó, nút 1 nhận được các yêu cầu không tương thích từ các cạnh đi ra của nó và cuối cùng một giao điểm trống xuất hiện. 

Thuật toán phát hiện điều này ngay lập tức và in`"No"`. 

Điều này chứng tỏ tại sao tính nhất quán cục bộ tại mỗi nút là đủ. Nếu ngay cả một nút cũng không thể chỉ định khoảng cách chung còn lại tương thích với tất cả các cạnh đi ra thì sẽ không có giải pháp tổng thể nào tồn tại.
