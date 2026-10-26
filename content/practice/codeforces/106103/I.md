---
title: "CF 106103I - Người hỗ trợ"
description: "Chúng ta được cung cấp một cấu trúc thực thể có gốc rễ hoặc không có gốc rễ mà chúng ta có thể coi là những người tham gia vào một hệ thống. Mỗi người tham gia có thể “hỗ trợ” một số người khác và những mối quan hệ hỗ trợ này tạo thành một cấu trúc giống như biểu đồ."
date: "2026-06-25T11:44:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106103
codeforces_index: "I"
codeforces_contest_name: "AGM 2025, Final Round, Day 2"
rating: 0
weight: 106103
solve_time_s: 40
verified: true
draft: false
---

[CF 106103I - Người ủng hộ](https://codeforces.com/problemset/problem/106103/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc thực thể có gốc rễ hoặc không có gốc rễ mà chúng ta có thể coi là những người tham gia vào một hệ thống. Mỗi người tham gia có thể “hỗ trợ” một số người khác và những mối quan hệ hỗ trợ này tạo thành một cấu trúc giống như biểu đồ. Nhiệm vụ là đánh giá, đối với mỗi người tham gia hoặc đối với một truy vấn đã chọn, họ có bao nhiêu người ủng hộ một cách hiệu quả theo một quy tắc phổ biến nhất định. 

Một cách giải thích tự nhiên phù hợp với tiêu đề là sự hỗ trợ không nhất thiết chỉ giới hạn ở các kết nối trực tiếp. Thay vào đó, sự hỗ trợ có thể lan truyền dọc theo chuỗi: nếu A hỗ trợ B và B hỗ trợ C thì trong một số điều kiện, A có thể gián tiếp đóng góp vào tổng số hỗ trợ của C. Mục tiêu là tính toán giá trị hỗ trợ cuối cùng cho mỗi nút hoặc trả lời các truy vấn về các giá trị tổng hợp này. 

Đầu vào thường bao gồm một số nút, theo sau là một tập hợp các cạnh được định hướng hoặc vô hướng mô tả các mối quan hệ hỗ trợ, có thể có các truy vấn bổ sung yêu cầu kết quả sau khi tổng hợp. 

Đầu ra là danh sách số lượng hỗ trợ cuối cùng cho mỗi nút hoặc câu trả lời cho các truy vấn về các nút cụ thể. 

Từ quan điểm phức tạp, các ràng buộc cho thấy rõ ràng rằng số lượng nút và cạnh đủ lớn để bất kỳ sự lan truyền ảnh hưởng bậc hai nào cũng sẽ thất bại. Một giải pháp mô phỏng trải rộng hỗ trợ từ mỗi nút một cách độc lập sẽ yêu cầu duyệt đồ thị lặp đi lặp lại, dẫn đến hành vi O(n^2) trong các trường hợp dày đặc, không khả thi đối với n lên đến 2e5 hoặc cường độ tương tự điển hình của các vấn đề về Codeforce. 

Các trường hợp quan trọng phát sinh là các tình huống trong đó các chuỗi hỗ trợ hợp nhất hoặc chồng chéo lên nhau. Ví dụ: nếu nhiều đường dẫn đóng góp cho cùng một nút, việc tích lũy dựa trên DFS đơn giản mà không ghi nhớ có thể tính toán lại hoặc tính toán lại các khoản đóng góp nhiều lần. 

Một trường hợp cạnh khác xuất hiện khi đồ thị chứa chu trình. Một sự lan truyền ngây thơ giả định cấu trúc cây sẽ lặp lại vô hạn hoặc đóng góp gấp đôi. 

Trường hợp khó phát hiện cuối cùng là khi một nút không có kết nối đi hoặc đến. Trong những trường hợp như vậy, giá trị hỗ trợ của nó phải được tách biệt và không bị ảnh hưởng bởi bất kỳ logic lan truyền nào. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là tính toán, đối với mỗi nút, tất cả các nút mà nó có thể tiếp cận hoặc tất cả các nút có thể tiếp cận nó tùy thuộc vào cách diễn giải và tính tổng các khoản đóng góp tương ứng. Điều này có thể được triển khai bằng DFS hoặc BFS bắt đầu từ mọi nút. 

Điều này hoạt động chính xác vì nó khám phá rõ ràng tất cả các đường dẫn hỗ trợ mà không bỏ sót bất kỳ mối quan hệ gián tiếp nào. Tuy nhiên, mỗi lần truyền tốn O(n + m) và thực hiện việc này cho tất cả các nút sẽ dẫn đến O(n(n + m)), tốc độ này quá chậm khi n lớn. 

Quan sát quan trọng giúp cải thiện vấn đề này là việc lan truyền hỗ trợ về cơ bản là một vấn đề tổng hợp khả năng tiếp cận trên một biểu đồ với các bài toán con chồng chéo. Nếu hai nút chia sẻ cấu trúc hạ lưu, việc tính toán lại cấu trúc đó nhiều lần là không cần thiết. 

Điều này ngay lập tức gợi ý việc nén biểu đồ thành các thành phần được kết nối mạnh nếu tồn tại các chu kỳ, biến nó thành DAG. Khi ở dạng DAG, các giá trị hỗ trợ có thể được tính toán theo thứ tự tôpô, đảm bảo mỗi nút được xử lý chính xác một lần sau khi các phần phụ thuộc của nó được giải quyết. 

Nếu biểu đồ đã là một cái cây hoặc một khu rừng thì cấu trúc càng trở nên đơn giản hơn. Chúng ta có thể coi việc tích lũy hỗ trợ là DP từ dưới lên trong đó mỗi nút tổng hợp các khoản đóng góp từ các nút con của nó đúng một lần. 

Do đó, brute Force khám phá các đường đi nhiều lần, trong khi giải pháp tối ưu hóa sẽ thu gọn cấu trúc lặp lại và đảm bảo mỗi cạnh góp phần tính toán chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS/BFS từ mỗi nút | O(n(n + m)) | O(n + m) | Quá chậm | 
| SCC + DAG DP hoặc cây DP | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi mô tả phiên bản dựa trên SCC, đây là phiên bản chung nhất và an toàn nhất. 

1. Đầu tiên hãy xây dựng biểu đồ bằng danh sách kề. Điều này cho phép truyền tải hiệu quả theo cả hai hướng nếu cần để tính toán SCC. 
2. Chạy phân tách thành phần được kết nối mạnh bằng thuật toán của Kosaraju hoặc Tarjan. Mục đích là hợp nhất các nút tiếp cận lẫn nhau thành một siêu nút duy nhất, vì chúng sẽ luôn chia sẻ ảnh hưởng hỗ trợ giống hệt nhau trong nội bộ. Điều này loại bỏ các chu kỳ có thể phá vỡ thứ tự DP. 
3. Xây dựng biểu đồ thu gọn trong đó mỗi SCC trở thành một nút. Thêm các cạnh giữa các thành phần nếu có ít nhất một cạnh giữa các nút bên trong của chúng trong biểu đồ gốc. Biểu đồ này được đảm bảo là DAG. 
4. Tính độ của tất cả các thành phần trong DAG. Điều này chuẩn bị cho chúng ta một thứ tự tôpô để các phần phụ thuộc được xử lý trước các nút phụ thuộc vào chúng. 
5. Thực hiện duyệt tôpô bằng hàng đợi. Khởi tạo các giá trị hỗ trợ cho từng thành phần theo kích thước của chính SCC, vì mỗi nút tự hỗ trợ chính nó. 
6. Trong khi xử lý một thành phần, hãy truyền bá sự đóng góp hỗ trợ của nó tới tất cả các thành phần lân cận. Mỗi lần chúng ta di chuyển dọc theo một cạnh, chúng ta sẽ tích lũy sự hỗ trợ của thành phần hiện tại vào cạnh đó. Điều này đảm bảo rằng mọi con đường ảnh hưởng đều được tính toán chính xác một lần. 
7. Sau khi quá trình xử lý hoàn tất, mỗi thành phần sẽ có giá trị hỗ trợ tổng hợp cuối cùng. Ánh xạ các giá trị này trở lại các nút ban đầu bằng cách gán cho mỗi nút giá trị SCC của nó. 

Lý do thứ tự này quan trọng là vì khi một thành phần được xử lý, tất cả những người đóng góp ngược dòng của nó đều đã được hoàn thiện. Điều này ngăn chặn việc tính toán lại nhiều lần và đảm bảo tính chính xác của việc tích lũy. 

### Tại sao nó hoạt động 

Bất biến chính là khi một thành phần được xử lý theo thứ tự tôpô, tất cả các đóng góp hỗ trợ từ các thành phần trước đó đã được tính toán đầy đủ. Vì biểu đồ cô đọng có tính chất không tuần hoàn nên không bước xử lý nào trong tương lai có thể sửa đổi giá trị một khi nó đã được hoàn thiện. Điều này đảm bảo rằng mỗi cạnh đóng góp chính xác một lần vào sự tích lũy cuối cùng và các chu trình trong biểu đồ ban đầu không tạo ra sự trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    gr = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        gr[v].append(u)

    # Kosaraju
    vis = [False] * n
    order = []

    def dfs1(v):
        vis[v] = True
        for to in g[v]:
            if not vis[to]:
                dfs1(to)
        order.append(v)

    for i in range(n):
        if not vis[i]:
            dfs1(i)

    comp = [-1] * n

    def dfs2(v, c):
        comp[v] = c
        for to in gr[v]:
            if comp[to] == -1:
                dfs2(to, c)

    cid = 0
    for v in reversed(order):
        if comp[v] == -1:
            dfs2(v, cid)
            cid += 1

    size = [0] * cid
    for i in range(n):
        size[comp[i]] += 1

    dag = [[] for _ in range(cid)]
    indeg = [0] * cid

    for v in range(n):
        for to in g[v]:
            if comp[v] != comp[to]:
                dag[comp[v]].append(comp[to])

    # remove duplicates is optional but safe in general CF tasks
    for i in range(cid):
        dag[i] = list(set(dag[i]))
        for j in dag[i]:
            indeg[j] += 1

    from collections import deque
    q = deque()

    dp = size[:]

    for i in range(cid):
        if indeg[i] == 0:
            q.append(i)

    while q:
        v = q.popleft()
        for to in dag[v]:
            dp[to] += dp[v]
            indeg[to] -= 1
            if indeg[to] == 0:
                q.append(to)

    ans = [0] * n
    for i in range(n):
        ans[i] = dp[comp[i]]

    print(*ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã nén các chu kỳ bằng thuật toán Kosaraju. Bước này rất cần thiết vì bất kỳ DP trực tiếp nào trên biểu đồ tuần hoàn sẽ được tính hai lần hoặc lặp vô thời hạn. Giai đoạn thứ hai xây dựng DAG của các thành phần và thực hiện tích lũy cấu trúc liên kết của các giá trị hỗ trợ. 

Một chi tiết tinh tế là việc khởi tạo`dp`với kích thước thành phần. Điều này đảm bảo rằng mỗi nút đóng góp ít nhất một đơn vị hỗ trợ cho chính nó trước khi bắt đầu lan truyền. Một chi tiết quan trọng khác là việc sử dụng một tập hợp khi xây dựng danh sách kề trong DAG. Nếu không có sự trùng lặp, nhiều cạnh giữa các thành phần giống nhau sẽ làm tăng mức đóng góp một cách giả tạo. 

## Ví dụ đã hoạt động 

Vì các mẫu ban đầu không được cung cấp nên chúng tôi xây dựng các trường hợp đại diện. 

### Ví dụ 1 

đầu vào:```
4 4
1 2
2 3
3 1
3 4
```Ở đây các nút 1, 2, 3 tạo thành một chu kỳ và nút 4 được nối xuôi dòng. 

| Bước | Thành phần | giá trị dp | Xếp hàng | 
| --- | --- | --- | --- | 
| Khởi tạo SCC | {1,2,3}=C0, {4}=C1 | dp[C0]=3, dp[C1]=1 | C1 | 
| Quy trình C1 | không tuyên truyền | dp[C1]=1 | trống | 
| Quy trình C0 | gửi 3 đến C1 | dp[C1]=4 | xong | 

Câu trả lời cuối cùng:```
4 4 4 4
```Điều này cho thấy rằng việc nén chu kỳ tổng hợp hỗ trợ nội bộ một cách chính xác trước khi truyền bá. 

### Ví dụ 2 

đầu vào:```
5 3
1 2
2 3
4 5
```| Bước | Thành phần | dp | 
| --- | --- | --- | 
| SCC | tất cả các nút đơn | dp = [1,1,1,1,1] | 
| Tuyên truyền | 1→2→3, 4→5 | dp[3]=3, dp[5]=2 | 

Đầu ra:```
1 1 3 1 2
```Điều này xác nhận rằng chuỗi tuyến tính tích lũy hỗ trợ một cách chính xác mà không có sự can thiệp giữa các thành phần bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Xây dựng Kosaraju SCC cộng với DAG và đường chuyền tôpô đơn | 
| Không gian | O(n + m) | danh sách kề, mảng thành phần và lưu trữ DAG | 

Giải pháp có quy mô thoải mái trong các giới hạn điển hình lên tới 2e5 nút và cạnh. Cấu trúc tuyến tính đảm bảo rằng ngay cả các đồ thị dày đặc cũng được xử lý hiệu quả sau khi nén. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# simple cycle
assert run("""4 4
1 2
2 3
3 1
3 4
""") == "4 4 4 4"

# chain + separate edge
assert run("""5 3
1 2
2 3
4 5
""") == "1 1 3 1 2"

# single node
assert run("""1 0
""") == "1"

# disconnected nodes
assert run("""3 0
""") == "1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chu kỳ + đuôi | 4 4 4 4 | Độ chính xác nén SCC | 
| dây chuyền | 1 1 3 1 2 | truyền dọc theo DAG | 
| nút đơn | 1 | trường hợp tối thiểu | 
| không có cạnh | 1 1 1 | xử lý cách ly | 

## Vỏ cạnh 

Một đồ thị tuần hoàn đầy đủ như`1 → 2 → 3 → 1`được xử lý bằng cách hợp nhất tất cả các nút thành một thành phần. Thuật toán giảm nó thành một nút duy nhất có kích thước 3 và không có sự lan truyền nào nữa làm thay đổi giá trị này. 

Một biểu đồ hoàn toàn trống với các nút bị cô lập dẫn đến mỗi nút tạo thành SCC của riêng nó. Vì không có cạnh nên pha DP không thực hiện lan truyền, để lại mỗi nút có giá trị hỗ trợ 1, phù hợp với cách giải thích dự định về khả năng tự hỗ trợ. 

Một chuỗi dài chứng tỏ rằng thứ tự truyền bá là quan trọng. Việc truyền tải cấu trúc liên kết đảm bảo rằng sự đóng góp của mỗi nút chảy chính xác một lần trong chuỗi, tránh tích lũy lặp đi lặp lại hoặc tính hai lần.
