---
title: "CF 105971A - Mã thông báo trên biểu đồ"
description: "Chúng ta có một đồ thị vô hướng được kết nối. Một số đỉnh chứa mã thông báo và một số đỉnh chứa phần thưởng. Một bước di chuyển bao gồm việc lấy một mã thông báo và di chuyển nó sang một đỉnh lân cận."
date: "2026-06-25T13:41:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105971
codeforces_index: "A"
codeforces_contest_name: "BSUIR Open XIII: Student final"
rating: 0
weight: 105971
solve_time_s: 51
verified: true
draft: false
---

[CF 105971A - Mã thông báo trên biểu đồ](https://codeforces.com/problemset/problem/105971/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng được kết nối. Một số đỉnh chứa mã thông báo và một số đỉnh chứa phần thưởng. Một bước di chuyển bao gồm việc lấy một mã thông báo và di chuyển nó sang một đỉnh lân cận. Lần di chuyển đầu tiên luôn được phép, nhưng mọi lần di chuyển sau chỉ được phép nếu mã thông báo đã di chuyển trước đó rơi vào một đỉnh thưởng. Nước đi tiếp theo phải sử dụng mã thông báo khác. Mục tiêu là quyết định liệu cuối cùng có bất kỳ mã thông báo nào có thể đạt tới đỉnh 1 hay không. 

Khó khăn là sự di chuyển của mã thông báo không phải là điều duy nhất quan trọng. Một mã thông báo gần về đích có thể vẫn vô dụng nếu không có đủ mã thông báo khác để duy trì chuỗi hạ cánh thưởng. Đồng thời, một số nhóm tiền thưởng có thể tạo ra các lượt di chuyển bổ sung không giới hạn. 

Các ràng buộc cho phép tổng số đỉnh và cạnh trong các trường hợp thử nghiệm đạt khoảng 200000. Điều này loại trừ việc khám phá mọi chuỗi di chuyển mã thông báo có thể có, bởi vì số lượng trạng thái có thể tăng theo cấp số nhân. Một giải pháp phải gần tuyến tính về kích thước của đồ thị. 

Một số trường hợp rất dễ bỏ sót. Nếu mã thông báo bắt đầu ở đỉnh 1, câu trả lời ngay lập tức là CÓ vì không cần di chuyển. Ví dụ:```
n = 3, m = 2
tokens = [1]
bonuses = [2]
edges = (1,2), (2,3)
```Đầu ra đúng là CÓ. Giải pháp chỉ kiểm tra xem mã thông báo có thể di chuyển vào đỉnh 1 hay không sẽ từ chối nó một cách không chính xác. 

Một trường hợp phức tạp khác là khi một mã thông báo chỉ có thể về đích bằng cách sử dụng một số phần thưởng nhưng không có đủ mã thông báo khác để tiếp tục chuỗi.```
n = 4, m = 3
tokens = [4]
bonuses = [3]
edges = (1,2), (2,3), (3,4)
```Đầu ra đúng là KHÔNG. Mã thông báo có thể di chuyển từ 4 đến 3, nhưng sau đó nó không thể di chuyển lại vì mã thông báo tương tự không thể được sử dụng lại. Việc kiểm tra đường dẫn ngắn nhất đơn giản sẽ cho rằng mã thông báo có thể tiếp tục một cách không chính xác. 

Trường hợp cạnh cuối cùng là nguồn di chuyển vô hạn. Nếu hai phần thưởng ở gần nhau, một mã thông báo gần khu vực đó có thể tiếp tục kích hoạt phần thưởng liên tục bằng cách di chuyển các mã thông báo khác nhau xung quanh cạnh phần thưởng.```
n = 5, m = 5
tokens = [5,4]
bonuses = [2,3]
edges = (1,2), (2,3), (3,4), (4,5), (2,5)
```Đầu ra đúng là CÓ vì cặp tiền thưởng có thể cung cấp lượt chơi không giới hạn. Cách tiếp cận chỉ đếm số lượng mã thông báo gần đó sẽ bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng tất cả các trò chơi có thể xảy ra. Chúng ta có thể chọn một mã thông báo, di chuyển nó, sau đó thử đệ quy mọi mã thông báo tiếp theo có sẵn. Điều này đúng vì nó khám phá chính xác các chuỗi di chuyển có thể xảy ra. Vấn đề là hệ số phân nhánh có thể rất lớn. Với nhiều mã thông báo, số lượng lệnh di chuyển có thể tương đương với số lượng hoán vị của mã thông báo, vượt xa giới hạn cho phép. 

Quan sát quan trọng là ngừng suy nghĩ về toàn bộ chuỗi và tập trung vào số lần di chuyển cần thiết để đưa một mã thông báo đã chọn về đích. Giả sử chúng ta chọn một mã thông báo cuối cùng sẽ đạt đến đỉnh 1. Trước bước đi cuối cùng, mã thông báo này phải di chuyển qua các đỉnh cho phép chuỗi tiếp tục. Các đỉnh hữu ích này là các đỉnh bổ sung nằm trên đường đi về đích. 

Trước tiên, đối với mỗi đỉnh, chúng tôi tìm thấy số lần hạ cánh thưởng tối thiểu cần thiết trước khi mã thông báo ở đó có thể về đích. Điều này có thể được thực hiện với BFS bắt đầu từ đỉnh 1, nhưng chúng tôi chỉ mở rộng thông qua các đỉnh thưởng. Nếu mã thông báo ở một đỉnh có khoảng cách`d`, nó cần`d`động thái hỗ trợ trước khi nó có thể được hoàn thành. 

Bây giờ chúng tôi chỉ cần biết liệu các token khác có thể cung cấp đủ hỗ trợ hay không. Một mã thông báo có thể đóng góp thêm một bước di chuyển nếu nó nằm cạnh một đỉnh thưởng. Nó có thể đóng góp những bước di chuyển không giới hạn nếu nó nằm cạnh một đỉnh thưởng là một phần của cặp phần thưởng liền kề, bởi vì những phần thưởng đó có thể được sử dụng lại mãi mãi bằng các mã thông báo xen kẽ. 

Lực lượng vũ phu hoạt động vì nó mô hình hóa mọi thứ tự có thể. Nó thất bại khi số lượng đơn đặt hàng có thể bùng nổ. Nhận xét rằng chỉ số lượng các vấn đề hỗ trợ sẵn có mới cho phép chúng ta thay thế tìm kiếm theo trình tự bằng tiền xử lý đồ thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và lưu trữ đỉnh nào chứa mã thông báo và đỉnh nào chứa phần thưởng. Nếu bất kỳ mã thông báo nào bắt đầu ở đỉnh 1, câu trả lời ngay lập tức là CÓ vì trò chơi đã kết thúc. 
2. Đánh dấu mọi đỉnh thuộc về một cặp phần thưởng liền kề. Các đỉnh này đại diện cho một nơi mà mã thông báo có thể giúp tạo ra các bước di chuyển không giới hạn trong tương lai. 
3. Chạy BFS từ đỉnh 1. Chỉ di chuyển qua các đỉnh thưởng trong khi mở rộng. Khoảng cách kết quả cho chúng ta biết số lần di chuyển tối thiểu cần thiết từ đỉnh đó để về đích trong khi vẫn giữ cho trò chơi tiếp tục. 
4. Đối với mỗi mã thông báo, hãy tính xem nó cần bao nhiêu hỗ trợ. Một mã thông báo có thể sử dụng miễn phí nước đi đầu tiên của nó, vì vậy nếu khoảng cách yêu cầu của nó là`d`, nó cần`d - 1`di chuyển bổ sung từ các mã thông báo khác. 
5. Đếm số lượng hỗ trợ có sẵn từ tất cả các token. Mã thông báo đóng góp hỗ trợ không giới hạn nếu nó nằm cạnh khu vực thưởng gấp đôi. Nếu không, nó sẽ đóng góp một nước đi nếu nó có thể đạt được phần thưởng trong một bước. 
6. Hãy thử mọi mã thông báo làm mã thông báo chiến thắng cuối cùng. Tạm thời loại bỏ sự đóng góp của chính nó khỏi số lượng hỗ trợ. Nếu có hỗ trợ không giới hạn hoặc đủ hỗ trợ cho một lượt di chuyển thì câu trả lời là CÓ. 

Tại sao nó hoạt động: mã thông báo chiến thắng đã chọn chỉ cần một chuỗi nước đi trước khi nước đi cuối cùng của nó về đích. Mọi nước đi không phải là nước đi cuối cùng chỉ tồn tại để kích hoạt một lượt khác, vì vậy thông tin liên quan duy nhất về các mã thông báo khác là số lượt mà chúng có thể cung cấp. Các mã thông báo gần cấu trúc tiền thưởng có thể tái sử dụng cung cấp lượt quay vô hạn, trong khi tất cả các mã thông báo hữu ích khác chỉ cung cấp chính xác một lượt. Khoảng cách BFS đưa ra số vòng quay chính xác cần thiết nên việc so sánh cuối cùng là đủ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    p, b = map(int, input().split())

    tokens = list(map(int, input().split()))
    bonuses = [0] * (n + 1)

    if b:
        for x in map(int, input().split()):
            bonuses[x] = 1
    else:
        input()

    graph = [[] for _ in range(n + 1)]

    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    has_pair = [0] * (n + 1)
    for i in range(1, n + 1):
        if bonuses[i]:
            for v in graph[i]:
                if bonuses[v]:
                    has_pair[i] = 1
                    break

    if 1 in tokens:
        print("YES")
        return

    support = [0] * (n + 1)

    for x in tokens:
        for v in graph[x]:
            if has_pair[v]:
                support[x] = 2
                break
            if bonuses[v]:
                support[x] = max(support[x], 1)

    dist = [-1] * (n + 1)
    q = deque([1])
    dist[1] = 0

    while q:
        u = q.popleft()
        for v in graph[u]:
            if bonuses[v] and dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    cnt_one = 0
    cnt_inf = 0

    for x in tokens:
        if support[x] == 1:
            cnt_one += 1
        elif support[x] == 2:
            cnt_inf += 1

    for x in tokens:
        if support[x] == 1:
            cnt_one -= 1
        elif support[x] == 2:
            cnt_inf -= 1

        need = 10**9

        if dist[x] != -1:
            need = dist[x]

        for v in graph[x]:
            if dist[v] != -1:
                need = min(need, dist[v] + 1)

        if need != 10**9:
            if cnt_inf > 0 or cnt_one >= need - 1:
                print("YES")
                return

        if support[x] == 1:
            cnt_one += 1
        elif support[x] == 2:
            cnt_inf += 1

    print("NO")

t = int(input())
for _ in range(t):
    solve()
```Mã đầu tiên xây dựng danh sách lân cận vì mỗi bước sau đó cần truy cập nhanh vào hàng xóm. các`has_pair`mảng xác định các đỉnh nằm bên trong một cặp phần thưởng liền kề, chính xác là những nơi mà sự hỗ trợ trở nên không giới hạn. 

BFS hơi khác so với tìm kiếm đường đi ngắn nhất thông thường. Chúng tôi bắt đầu từ điểm kết thúc và chỉ mở rộng sang các đỉnh thưởng vì mọi bước di chuyển trung gian đều phải đạt được phần thưởng. Đỉnh không thưởng không thể là một phần của đường chuẩn bị. 

Vòng lặp cuối cùng kiểm tra từng mã thông báo khi mã thông báo đạt đến đích. Đóng góp của chính nó sẽ bị loại bỏ trước khi kiểm tra vì nó không thể tự giúp được. giá trị`need - 1`xuất phát từ thực tế là nước đi đầu tiên của trò chơi luôn có sẵn mà không cần kích hoạt tiền thưởng trước đó. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
n = 4, m = 3
tokens = [4,3]
bonuses = [3]
edges = (1,2), (2,3), (3,4)
```| Bước | Mã thông báo hiện tại | Khoảng cách | Hỗ trợ một động thái | Hỗ trợ vô hạn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 4 | 3 | 1 | 0 | Tiếp tục | 
| Mã thông báo kiểm tra 4 | 3 cần thiết | 1 | 0 | Không đủ | KHÔNG | 

Mã thông báo ở vị trí thứ 4 có thể nhập phần thưởng, nhưng sau đó không còn mã thông báo nào khác để tiếp tục chuỗi. Dấu vết cho thấy tại sao chỉ khoảng cách thôi là chưa đủ. 

Một ví dụ khác:```
n = 4, m = 4
tokens = [4,3]
bonuses = [2,3]
edges = (1,2), (2,3), (3,4), (1,4)
```| Bước | Mã thông báo hiện tại | Khoảng cách | Hỗ trợ một động thái | Hỗ trợ vô hạn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| BFS | 1 | 0 | | | | 
| Tiền thưởng 2 | 1 | | | | | 
| Tiền thưởng 3 | 2 | | | | | 
| Mã thông báo kiểm tra 4 | 2 | 0 | 1 | 0 | CÓ | 

Mã thông báo ở vị trí thứ 4 có thể sử dụng chuỗi thưởng. Sự hỗ trợ từ mã thông báo khác là đủ để giữ cho trò chơi tồn tại cho đến nước đi cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được xử lý một số lần không đổi bằng BFS và tiền xử lý | 
| Không gian | O(n + m) | Danh sách kề và mảng phụ lưu trữ thông tin đồ thị | 

Tổng kích thước đầu vào trên tất cả các thử nghiệm phù hợp với giải pháp tuyến tính. Thuật toán tránh khám phá các trạng thái trò chơi, do đó nó vẫn nhanh ngay cả đối với các biểu đồ lớn nhất. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n, m = map(int, sys.stdin.readline().split())
        p, b = map(int, sys.stdin.readline().split())
        tokens = list(map(int, sys.stdin.readline().split()))
        bonuses = [0] * (n + 1)
        if b:
            for x in map(int, sys.stdin.readline().split()):
                bonuses[x] = 1
        else:
            sys.stdin.readline()

        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, sys.stdin.readline().split())
            g[u].append(v)
            g[v].append(u)

        if 1 in tokens:
            out.append("YES")
            continue

        pair = [0] * (n + 1)
        for i in range(1, n + 1):
            if bonuses[i] and any(bonuses[x] for x in g[i]):
                pair[i] = 1

        typ = []
        one = inf = 0
        for x in tokens:
            val = 0
            for y in g[x]:
                if pair[y]:
                    val = 2
                    break
                if bonuses[y]:
                    val = 1
            typ.append(val)
            if val == 1:
                one += 1
            elif val == 2:
                inf += 1

        dist = [-1] * (n + 1)
        q = deque([1])
        dist[1] = 0
        while q:
            u = q.popleft()
            for v in g[u]:
                if bonuses[v] and dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)

        ok = False
        for idx, x in enumerate(tokens):
            if typ[idx] == 1:
                one -= 1
            elif typ[idx] == 2:
                inf -= 1

            need = 10**9
            if dist[x] != -1:
                need = dist[x]
            for y in g[x]:
                if dist[y] != -1:
                    need = min(need, dist[y] + 1)

            if need != 10**9 and (inf or one >= need - 1):
                ok = True

            if typ[idx] == 1:
                one += 1
            elif typ[idx] == 2:
                inf += 1

        out.append("YES" if ok else "NO")

    sys.stdin = old
    return "\n".join(out)

assert run("""1
3 2
1 1
1
2
1 2
2 3
""") == "YES"

assert run("""1
4 3
1 1
4
3
1 2
2 3
3 4
""") == "NO"

assert run("""1
4 4
2 2
3 4
2 3
1 2
2 3
3 4
1 4
""") == "YES"

assert run("""1
2 1
1 0
2

1 2
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mã thông báo đã hoàn tất | CÓ | Trường hợp thắng ngay lập tức | 
| Mã thông báo duy nhất cần chuỗi thưởng | KHÔNG | Ngăn chặn việc bỏ qua các quy tắc tái sử dụng mã thông báo | 
| Phần thưởng liền kề với nhiều mã thông báo | CÓ | Phát hiện hỗ trợ vô hạn | 
| Không có tiền thưởng và chỉ di chuyển một lần | KHÔNG | Điều kiện biên | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên, mã thông báo ở đỉnh 1 không bao giờ tham gia kiểm tra thuật toán vì câu trả lời được trả về ngay lập tức. Điều này xử lý trạng thái mà trò chơi đã kết thúc trước bất kỳ nước đi nào. 

Đối với trường hợp thưởng mã thông báo duy nhất, BFS phát hiện chính xác rằng mã thông báo có thể sắp về đích nhưng số lượng hỗ trợ bằng 0 sau khi xóa mã thông báo chiến thắng. Điều kiện không thành công vì không có đủ mã thông báo khác để cung cấp lượt chơi. 

Đối với trường hợp tiền thưởng liền kề, quá trình tiền xử lý sẽ đánh dấu cặp tiền thưởng. Bất kỳ mã thông báo nào bên cạnh khu vực đó đều được phân loại là hỗ trợ vô hạn, do đó, lần kiểm tra cuối cùng sẽ thành công mà không cần tính từng bước di chuyển. Điều này nắm bắt được tính chất lặp đi lặp lại của các phần thưởng liền kề.
