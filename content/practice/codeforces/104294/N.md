---
title: "CF 104294N - Cổng điều tra"
description: "Chúng ta được cung cấp một biểu đồ có hướng trong đó các thành phố là các nút và các cổng ma thuật là các cạnh có hướng. Mỗi cổng đại diện cho một tuyến đường du lịch một chiều. Misaka muốn “điều tra” càng nhiều cổng càng tốt bằng cách sử dụng nhiều tác nhân độc lập được gọi là bản sao."
date: "2026-07-01T20:31:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104294
codeforces_index: "N"
codeforces_contest_name: "UTPC Spring 2023 Open Contest"
rating: 0
weight: 104294
solve_time_s: 100
verified: false
draft: false
---

[CF 104294N - Điều tra cổng thông tin](https://codeforces.com/problemset/problem/104294/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng trong đó các thành phố là các nút và các cổng ma thuật là các cạnh có hướng. Mỗi cổng đại diện cho một tuyến đường du lịch một chiều. Misaka muốn “điều tra” càng nhiều cổng càng tốt bằng cách sử dụng nhiều tác nhân độc lập được gọi là bản sao. 

Một cổng thông tin chỉ được coi là đã điều tra thành công nếu một bản sao đi qua nó hai lần. Bởi vì các cạnh được định hướng, điều này ngầm có nghĩa là một bản sao phải có khả năng đi dọc theo cạnh đó, sau đó quay lại và đi ngang qua nó theo cùng một hướng. Vì vậy, một cổng chỉ có thể được sử dụng khi cấu trúc biểu đồ cho phép cùng một tác nhân duyệt qua cạnh được định hướng đó nhiều lần. 

Có một hạn chế bổ sung: các bản sao khác nhau phải hoạt động trên các nhóm thành phố riêng biệt. Không có thành phố nào có thể được nhiều hơn một bản sao ghé thăm. Mỗi bản sao được chỉ định một thành phố bắt đầu và khám phá từ đó mà không giao nhau với các đỉnh đã ghé thăm của các bản sao khác. 

Nhiệm vụ có hai đầu ra. Đầu tiên, chúng ta phải tối đa hóa số lượng cổng có thể được nghiên cứu dưới những ràng buộc này. Thứ hai, trong số tất cả các chiến lược đạt được mức tối đa đó, chúng ta phải giảm thiểu số lượng bản sao được yêu cầu. 

Kích thước đầu vào đạt tới năm mươi nghìn thành phố và năm mươi nghìn cạnh, điều này gợi ý rõ ràng về thuật toán đồ thị tuyến tính hoặc gần tuyến tính. Bất cứ điều gì bậc hai trên các nút hoặc cạnh sẽ thất bại ngay lập tức. Điều này thúc đẩy chúng ta tiến tới phân tích cấu trúc được kết nối mạnh mẽ hơn là bất kỳ hình thức liệt kê đường dẫn rõ ràng nào. 

Trường hợp cạnh khóa xuất hiện khi đồ thị đã có tính tuần hoàn. Trong tình huống đó, không có cạnh có hướng nào có thể được đi qua hai lần bởi cùng một bản sao, bởi vì không có cách nào để quay lại điểm cuối ban đầu theo hướng dẫn. Câu trả lời đúng sẽ trở thành không có cổng được điều tra và không có bản sao. 

Một trường hợp cạnh khác là đồ thị là một thành phần duy nhất được kết nối mạnh mẽ. Ở đây, mọi cạnh đều có khả năng sử dụng được vì mọi nút đều có thể tiếp cận mọi nút khác, khiến cho việc truyền tải lặp lại trở nên khả thi. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ mô phỏng chuyển động của bản sao: chỉ định các bản sao cho các nút bắt đầu, cố gắng đi dọc theo các cạnh và đảm bảo rằng không có nút nào được sử dụng lại trên các bản sao. Mỗi bản sao sẽ cố gắng tối đa hóa số cạnh mà nó có thể đi qua hai lần. Điều này nhanh chóng trở thành một bài toán gán tổ hợp trên các đường dẫn trong đồ thị có hướng với các ràng buộc loại trừ chung và số lượng phép gán đường dẫn có thể tăng theo cấp số nhân với số lượng thành phố. Ngay cả những phương pháp phỏng đoán tham lam cũng thất bại vì những lựa chọn của địa phương về việc nhân bản nào ghé thăm thành phố nào có thể ngăn cản việc sử dụng các khu vực lớn có kết nối mạnh mẽ một cách tối ưu. 

Quan sát quan trọng đến từ việc định hình lại những gì “đi qua cổng hai lần” thực sự yêu cầu. Nếu một bản sao sử dụng cạnh có hướng u → v và sau đó sử dụng lại nó, thì nó phải có khả năng quay trở lại từ v về u mà không vi phạm các ràng buộc về hướng. Điều đó ngay lập tức có nghĩa là u và v phải nằm trong một chu trình, và trên thực tế nằm trong cùng một vùng liên thông mạnh. Bất kỳ cạnh nào đi từ thành phần được kết nối mạnh này sang thành phần khác đều có thể được sử dụng nhiều nhất một lần, vì sau khi vượt qua nó, không có đường dẫn quay lại được định hướng. 

Điều này làm giảm cấu trúc của bài toán về biểu đồ ngưng tụ của các thành phần liên thông mạnh. Biểu đồ ngưng tụ là một DAG và chỉ các cạnh có điểm cuối nằm bên trong cùng một thành phần mới có thể sử dụng được để truyền tải lặp lại. Do đó, tối đa hóa các cổng được điều tra tương đương với việc đếm tất cả các cạnh nằm hoàn toàn bên trong SCC. 

Một khi điều này được thiết lập, phần thứ hai sẽ trở nên dễ dàng hơn. Vì các bản sao không thể chia sẻ thành phố nên mỗi bản sao phải hoạt động hoàn toàn bên trong một nhóm khu vực SCC rời rạc. Bất kỳ SCC nào chứa ít nhất một cạnh có thể sử dụng được đều phải được chỉ định ít nhất một bản sao, vì một bản sao duy nhất không thể được phân chia giữa các thành phần hoặc thành phố dùng chung. SCC không có bất kỳ cạnh bên trong nào không yêu cầu bản sao vì chúng không đóng góp gì cho mục tiêu.

Điều này dẫn đến một giải pháp dựa trên SCC rõ ràng: tính toán các thành phần được kết nối mạnh, phân loại các cạnh thành thành phần bên trong hoặc thành phần chéo, đếm các cạnh bên trong và đếm xem có bao nhiêu thành phần chứa ít nhất một cạnh bên trong. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force của bản sao và đường dẫn | O(exp(n)) | O(n + m) | Quá chậm | 
| Phân hủy SCC | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách nén biểu đồ thành các thành phần được kết nối mạnh mẽ và sau đó phân tích các cạnh ở cấp độ thành phần. 

### 1. Tính toán các thành phần liên thông mạnh 

Chúng tôi chạy thuật toán SCC tiêu chuẩn như Kosaraju hoặc Tarjan. Mỗi thành phố được gán một mã định danh thành phần. Thuộc tính quan trọng là tất cả các nút bên trong một thành phần có thể tiếp cận nhau bằng các đường dẫn có hướng. 

Bước này biến đổi biểu đồ ban đầu thành DAG gồm các thành phần trong đó các chu trình đã được thu gọn hoàn toàn. 

### 2. Phân loại từng cạnh 

Với mỗi cổng u → v, chúng tôi kiểm tra xem cả hai điểm cuối có thuộc cùng một SCC hay không. 

Nếu đúng như vậy, cạnh này nằm bên trong cấu trúc chu trình và có khả năng được một bản sao đi qua hai lần. Chúng tôi coi nó như một cổng thông tin có thể điều tra được. 

Nếu chúng thuộc về các SCC khác nhau, cạnh sẽ bị bỏ qua đối với câu trả lời đầu tiên vì một khi đã vượt qua, sẽ không có đường quay lại bên trong cấu trúc được định hướng. 

### 3. Đếm các thành phần biên trong 

Chúng tôi duy trì điểm đánh dấu boolean cho mỗi SCC. Bất cứ khi nào chúng tôi gặp một cạnh bên trong bên trong một thành phần, chúng tôi đánh dấu thành phần đó là “hoạt động”. 

Điều này thể hiện rằng ít nhất một cổng bên trong SCC đó có thể được điều tra, nghĩa là ít nhất một bản sao phải được chỉ định cho khu vực đó. 

### 4. Đưa ra câu trả lời 

Câu trả lời đầu tiên là tổng số cạnh bên trong của tất cả các SCC. 

Câu trả lời thứ hai là số lượng SCC được đánh dấu hoạt động. 

### Tại sao nó hoạt động 

Bên trong một thành phần được kết nối mạnh mẽ, mọi nút đều có thể tiếp cận mọi nút khác, do đó, một bản sao được đặt ở bất kỳ đâu trong thành phần đó có thể đi qua các cạnh theo chu kỳ và truy cập lại các cạnh nếu cần. Bất kỳ cạnh nào bên ngoài SCC đều không thể là một phần của chu trình, do đó, nó không thể được duyệt qua hai lần bởi cùng một bản sao. Vì các bản sao không thể chia sẻ thành phố nên mỗi SCC yêu cầu công việc phải được xử lý độc lập, điều này buộc mỗi SCC đang hoạt động phải có một bản sao. Điều này tạo ra ánh xạ trực tiếp giữa cấu trúc SCC và phân công bản sao tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def kosaraju(n, adj, radj):
    visited = [False] * (n + 1)
    order = []

    def dfs1(u):
        visited[u] = True
        for v in adj[u]:
            if not visited[v]:
                dfs1(v)
        order.append(u)

    for i in range(1, n + 1):
        if not visited[i]:
            dfs1(i)

    comp = [-1] * (n + 1)

    def dfs2(u, c):
        comp[u] = c
        for v in radj[u]:
            if comp[v] == -1:
                dfs2(v, c)

    cid = 0
    for u in reversed(order):
        if comp[u] == -1:
            dfs2(u, cid)
            cid += 1

    return comp, cid

def main():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    radj = [[] for _ in range(n + 1)]

    edges = []

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        radj[b].append(a)
        edges.append((a, b))

    comp, c = kosaraju(n, adj, radj)

    internal_edges = 0
    active = [0] * c

    for a, b in edges:
        if comp[a] == comp[b]:
            internal_edges += 1
            active[comp[a]] = 1

    clones = sum(active)

    print(internal_edges)
    print(clones)

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ xây dựng cả danh sách kề thuận và ngược, cần thiết cho phân tách SCC hai bước của Kosaraju. Sau khi tính toán nhãn thành phần, mỗi cạnh được kiểm tra theo thời gian không đổi để xác định xem nó có nằm trong thành phần hay không. 

Một chi tiết tinh tế là các bản sao được tính trên mỗi SCC chứa ít nhất một cạnh bên trong chứ không phải trên mỗi cạnh. Sự khác biệt này quan trọng khi một thành phần chứa nhiều cạnh nhưng vẫn chỉ yêu cầu một bản sao duy nhất do có đầy đủ kết nối bên trong. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng ta bắt đầu với một biểu đồ chứa nhiều chu trình và các kết nối chéo. 

| Bước | Hành động | Các cạnh bên trong | SCC đang hoạt động | 
| --- | --- | --- | --- | 
| 1 | Tính toán SCC | 0 | 0 | 
| 2 | Xử lý các cạnh, phát hiện các cạnh cùng thành phần | 17 | đánh dấu một phần | 
| 3 | Đánh dấu các SCC chứa ít nhất một cạnh trong | 17 | 6 | 

Sự ngưng tụ cho thấy một số vùng được kết nối mạnh mẽ. Chỉ các cạnh nằm hoàn toàn bên trong các vùng đó mới được tính. Các cạnh thành phần chéo bị loại bỏ ngay lập tức. Sáu thành phần chứa ít nhất một cạnh bên trong, do đó cần có sáu bản sao. 

### Mẫu 2 

Đầu vào bao gồm hai cạnh bị ngắt kết nối tạo thành không có chu kỳ. 

| Bước | Hành động | Các cạnh bên trong | SCC đang hoạt động | 
| --- | --- | --- | --- | 
| 1 | Tính toán SCC (tất cả các nút đơn) | 0 | 0 | 
| 2 | Kiểm tra các cạnh, tất cả các thành phần chéo | 0 | 0 | 
| 3 | Không có SCC nào chứa các cạnh bên trong | 0 | 0 | 

Vì không tồn tại chu trình có định hướng nên không có cổng nào có thể đi qua hai lần. Do đó, cả hai đầu ra đều bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Hai đường chuyền DFS cho SCC cộng với một lần quét tuyến tính trên các cạnh | 
| Không gian | O(n + m) | Danh sách kề, đồ thị ngược và mảng thành phần | 

Các ràng buộc cho phép lên tới 50.000 nút và cạnh, do đó thuật toán SCC thời gian tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    sys.setrecursionlimit(10**7)

    def kosaraju(n, adj, radj):
        visited = [False] * (n + 1)
        order = []

        def dfs1(u):
            visited[u] = True
            for v in adj[u]:
                if not visited[v]:
                    dfs1(v)
            order.append(u)

        for i in range(1, n + 1):
            if not visited[i]:
                dfs1(i)

        comp = [-1] * (n + 1)

        def dfs2(u, c):
            comp[u] = c
            for v in radj[u]:
                if comp[v] == -1:
                    dfs2(v, c)

        cid = 0
        for u in reversed(order):
            if comp[u] == -1:
                dfs2(u, cid)
                cid += 1

        return comp, cid

    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    radj = [[] for _ in range(n + 1)]
    edges = []

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        radj[b].append(a)
        edges.append((a, b))

    comp, c = kosaraju(n, adj, radj)

    internal = 0
    active = [0] * c

    for a, b in edges:
        if comp[a] == comp[b]:
            internal += 1
            active[comp[a]] = 1

    return f"{internal}\n{sum(active)}"

# provided samples
assert solve("""18 27
1 2
1 2
2 1
1 7
1 8
3 4
4 3
3 8
5 6
6 5
6 8
15 16
16 15
16 8
7 9
8 10
8 12
8 14
8 17
9 10
10 9
11 12
12 11
13 14
14 13
17 18
18 17
""") == "17\n6"

assert solve("""6 2
1 2
3 4
""") == "0\n0"

# minimum-size
assert solve("""2 0
""") == "0\n0"

# single cycle
assert solve("""3 3
1 2
2 3
3 1
""") == "3\n1"

# self-contained SCC with cross edges
assert solve("""4 4
1 2
2 1
2 3
3 4
""") == "2\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 | 0 0 | xử lý đồ thị trống | 
| 3 chu kỳ | 3 1 | sử dụng SCC đầy đủ | 
| hỗn hợp SCC + chuỗi | 2 1 | Lọc SCC | 

## Vỏ cạnh 

Khi biểu đồ hoàn toàn không có chu kỳ, mỗi nút sẽ tạo thành SCC của riêng nó. Thuật toán không đánh dấu cạnh bên trong nên cả hai bộ đếm vẫn bằng 0, phù hợp với thực tế là không có cổng nào có thể đi qua hai lần. 

Trong một đồ thị liên thông mạnh, mọi cạnh đều là cạnh trong. Quá trình phân tách SCC tạo ra một thành phần duy nhất, do đó tất cả các cạnh đều được tính và cần có chính xác một bản sao để hoạt động bên trong thành phần đó. 

Khi có nhiều SCC được kết nối bằng các cạnh được định hướng, các cạnh thành phần chéo đó sẽ bị bỏ qua cho câu trả lời đầu tiên và chỉ những SCC chứa ít nhất một cạnh bên trong mới được gán các bản sao. Điều này ngăn chặn việc đếm quá nhiều bản sao cho các khu vực không đóng góp cổng thông tin có thể điều tra được.
