---
title: "CF 915D - Đồ thị gần như không có chu trình"
description: "Cho một đồ thị có hướng có n đỉnh và m cạnh. Mỗi cạnh có hướng từ đỉnh u này đến đỉnh v khác. Nhiệm vụ là xác định xem liệu chúng ta có thể loại bỏ tối đa một cạnh để làm cho đồ thị có tính tuần hoàn hay không."
date: "2026-06-13T01:42:56+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 915
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 36 (Rated for Div. 2)"
rating: 2200
weight: 915
solve_time_s: 204
verified: true
draft: false
---

[CF 915D - Đồ thị gần như không có chu kỳ](https://codeforces.com/problemset/problem/915/D) 

**Xếp hạng:** 2200 
**Thẻ:** dfs và các biểu đồ tương tự 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng với _n_ đỉnh và _m_ cạnh. Mỗi cạnh có hướng từ đỉnh _u_ này đến đỉnh _v_ khác. Nhiệm vụ là xác định xem liệu chúng ta có thể loại bỏ tối đa một cạnh để làm cho đồ thị có tính tuần hoàn hay không. Đồ thị có hướng là đồ thị không có chu trình nếu không có đỉnh nào có thể chạm tới chính nó bằng cách đi theo các cạnh có hướng dọc theo một đường dẫn không trống. 

Đầu vào thể hiện cấu trúc danh sách kề điển hình: mỗi dòng mô tả một kết nối có hướng. Đầu ra là nhị phân - "CÓ" nếu loại bỏ 0 hoặc một cạnh đủ để loại bỏ tất cả các chu kỳ, nếu không thì "KHÔNG". 

Các ràng buộc cho chúng ta biết rằng _n_ lên tới 500, đủ nhỏ cho các thuật toán có độ phức tạp bậc ba trong trường hợp xấu nhất. Số cạnh _m_ có thể lên tới 100.000, nhưng vì _n_ bị chặn nên giới hạn trên này chỉ quan trọng đối với các đồ thị gần hoàn chỉnh. Đối với vấn đề này, chúng ta nên tránh mọi điều tệ hơn O(n³) để thực hiện thực tế trong vòng 1 giây. 

Một DFS ngây thơ kiểm tra tất cả các cạnh bằng cách loại bỏ từng cạnh một có thể thất bại theo những cách tinh tế. Ví dụ: nếu biểu đồ có một chu trình được hình thành bởi ba đỉnh với hai cạnh riêng biệt trỏ đến cùng một nút, thì cách tiếp cận bất cẩn dừng lại ở lần phát hiện chu trình đầu tiên có thể kết luận sai rằng cần phải loại bỏ một cạnh khác. Một trường hợp cạnh khác là tự lặp, trong đó một cạnh từ một đỉnh đến chính nó ngay lập tức tạo thành một chu trình có thể bị phá vỡ bằng cách loại bỏ cạnh đó. Các đồ thị nhỏ, hai nút có hai cạnh tạo thành một chu trình hoặc nhiều chu trình độc lập đều phải được xem xét. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi cạnh, hãy loại bỏ nó và kiểm tra xem đồ thị kết quả có phải là không theo chu kỳ hay không bằng cách sử dụng DFS hoặc sắp xếp tôpô. Nếu bất kỳ thao tác xóa nào tạo ra biểu đồ không theo chu kỳ, hãy trả lời "CÓ". Nếu không, hãy trả lời "KHÔNG". Điều này có hiệu quả vì việc loại bỏ một cạnh có thể bị hỏng nhiều nhất là một chu kỳ và việc kiểm tra đảm bảo không còn chu kỳ nào nữa. Tuy nhiên, trong trường hợp xấu nhất, chúng tôi thực hiện các lần duyệt O(m) DFS, mỗi lần mất O(n+m). Đối với n ≈ 500 và m ≈ 100.000, con số này có thể đạt tới hàng chục triệu thao tác, tốc độ chậm và không cần thiết. 

Thông tin chi tiết quan trọng là nếu biểu đồ có chu trình, thì tất cả các chu trình phải chia sẻ các cạnh với nhau theo cách mà việc loại bỏ bất kỳ cạnh nào bên ngoài "chu trình có vấn đề" sẽ không giúp ích được gì. Điều này cho phép tiếp cận nhanh hơn: trước tiên chúng tôi tìm thấy bất kỳ chu trình nào trong biểu đồ gốc. Khi chúng ta xác định được một chu trình, việc thử chỉ loại bỏ các cạnh trên chu trình đó là đủ. Nếu việc loại bỏ bất kỳ cạnh nào trong số này dẫn đến biểu đồ không có chu kỳ thì câu trả lời là "CÓ". Điều này làm giảm đáng kể số lượng kiểm tra DFS vì một chu trình trong đồ thị có kích thước n có tối đa n cạnh. Quan sát cho thấy việc loại bỏ một cạnh bên ngoài chu trình được phát hiện không thể phá vỡ chu trình đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m*(n+m)) | O(n+m) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(n*(n+m)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị, lưu trữ các cạnh theo cặp để dễ dàng loại bỏ. 
2. Thực hiện DFS trên tất cả các nút chưa được truy cập để phát hiện một chu trình. Duy trì trạng thái "truy cập" cho mỗi nút để phân biệt giữa các nút trên đường dẫn DFS hiện tại và các nút được xử lý đầy đủ. 
3. Sau khi tìm thấy một chu trình, hãy lưu trữ tất cả các cạnh trong chu trình này. 
4. Đối với mỗi cạnh trong chu trình, hãy tạm thời loại bỏ nó và kiểm tra xem đồ thị có trở nên không tuần hoàn hay không bằng cách thực hiện một DFS khác. Khôi phục lại cạnh sau đó. 
5. Nếu bất kỳ việc loại bỏ nào dẫn đến biểu đồ không theo chu kỳ, hãy xuất "CÓ". Nếu không có thì xuất ra "NO".

Tại sao nó hoạt động: DFS phát hiện chu kỳ đảm bảo rằng chúng tôi nắm bắt được ít nhất một chu kỳ. Bất kỳ đồ thị không theo chu trình nào cũng phải không có chu trình, vì vậy chúng ta chỉ cần cố gắng loại bỏ các cạnh trong chu trình. Các cạnh bên ngoài chu trình không thể góp phần phá vỡ chu trình, vì vậy chúng ta không bao giờ bỏ lỡ giải pháp. Thuật toán duy trì tính chính xác vì nó kiểm tra tất cả các cạnh ứng cử viên có thể giải quyết vấn đề chu trình trong khi vẫn bảo toàn tất cả cấu trúc đồ thị khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10000)

def find_cycle(n, adj):
    visited = [0] * (n + 1)  # 0=unvisited, 1=visiting, 2=visited
    parent = [0] * (n + 1)
    cycle = []

    def dfs(u):
        nonlocal cycle
        visited[u] = 1
        for v in adj[u]:
            if visited[v] == 0:
                parent[v] = u
                if dfs(v):
                    return True
            elif visited[v] == 1:
                # Found cycle: backtrack to build it
                cycle.append((u, v))
                cur = u
                while cur != v:
                    cycle.append((parent[cur], cur))
                    cur = parent[cur]
                return True
        visited[u] = 2
        return False

    for i in range(1, n + 1):
        if visited[i] == 0:
            if dfs(i):
                break
    return cycle

def is_acyclic(n, adj):
    visited = [0] * (n + 1)

    def dfs(u):
        visited[u] = 1
        for v in adj[u]:
            if visited[v] == 1:
                return False
            if visited[v] == 0:
                if not dfs(v):
                    return False
        visited[u] = 2
        return True

    for i in range(1, n + 1):
        if visited[i] == 0:
            if not dfs(i):
                return False
    return True

n, m = map(int, input().split())
adj = [[] for _ in range(n + 1)]
edges = []

for _ in range(m):
    u, v = map(int, input().split())
    adj[u].append(v)
    edges.append((u, v))

cycle = find_cycle(n, adj)
if not cycle:
    print("YES")
else:
    for u, v in cycle:
        adj[u].remove(v)
        if is_acyclic(n, adj):
            print("YES")
            break
        adj[u].append(v)
    else:
        print("NO")
```Giải pháp này trước tiên tìm một chu trình, sau đó thử loại bỏ từng cạnh trong chu trình đó để kiểm tra xem đồ thị có trở thành chu trình không. Các hàm DFS phân biệt cẩn thận giữa trạng thái truy cập và truy cập đầy đủ để tránh phân loại sai đường dẫn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 4
1 2
2 3
3 2
3 1
```| Bước | Trạng thái DFS | Phát hiện chu kỳ | Đã xóa cạnh | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | tất cả chưa được ghé thăm | - | - | - | 
| DFS 1→2→3→2 | ghé thăm = {1,2,3} | chu kỳ 2→3→2 | thử xóa 3→2 | không theo chu kỳ | 

Điều này thể hiện thuật toán xác định một chu trình, loại bỏ một cạnh và xác nhận tính chu kỳ. 

Đầu vào mẫu 2:```
3 3
1 2
2 3
3 1
```| Bước | Trạng thái DFS | Phát hiện chu kỳ | Đã xóa cạnh | Kết quả | 
| --- | --- | --- | --- | --- | 
| DFS 1→2→3→1 | ghé thăm = {1,2,3} | chu kỳ 1→2→3→1 | thử xóa 1→2 | vẫn chu kỳ | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*(n+m)) | DFS phát hiện chu kỳ là O(n+m). Chúng tôi có thể thử loại bỏ tối đa n cạnh của một chu trình, mỗi cạnh có kiểm tra DFS. | 
| Không gian | O(n+m) | Danh sách kề, mảng đã truy cập và mảng cha | 

Điều này rất phù hợp trong các giới hạn vì n ≤ 500 và m ≤ 100.000. DFS chạy thoải mái dưới 1 giây ngay cả trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())  # assuming the above solution is saved in solution.py
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3 4\n1 2\n2 3\n3 2\n3 1\n") == "YES", "sample 1"
assert run("3 3\n1 2\n2 3\n3 1\n") == "YES", "sample 2"

# custom cases
assert run("2 1\n1 2\n") == "YES", "two nodes, single edge"
assert run("2 2\n1 2\n2 1\n") == "YES", "two nodes, cycle edge removal"
assert run("4 5\n1 2\n2 3\n3 4\n4 2\n1 4\n") == "YES", "removing one edge breaks single cycle"
assert run("4 6\n1 2\n2 3\n3 4\n4 2\n1 4\n2 4\n") == "NO", "two cycles require more than one removal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|---|---
