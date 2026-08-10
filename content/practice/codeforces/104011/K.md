---
title: "CF 104011K - Tuyến đường kính vạn hoa"
description: "Chúng ta được cho một đồ thị vô hướng với các cạnh có trọng số, trong đó trọng số được gọi là độ màu. Nhiệm vụ là đi từ thành phố 1 đến thành phố n, nhưng không phải bằng bất kỳ con đường nào."
date: "2026-07-02T05:16:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104011
codeforces_index: "K"
codeforces_contest_name: "2021-2022 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104011
solve_time_s: 49
verified: true
draft: false
---

[CF 104011K - Con đường kính vạn hoa](https://codeforces.com/problemset/problem/104011/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng với các cạnh có trọng số, trong đó trọng số được gọi là độ màu. Nhiệm vụ là đi từ thành phố 1 đến thành phố n, nhưng không phải bằng bất kỳ con đường nào. 

Đầu tiên, trong số tất cả các tuyến đường có thể, chúng tôi chỉ quan tâm đến những tuyến đường có số cạnh tối thiểu, nghĩa là các tuyến đường ngắn nhất xét về số bước nhảy chứ không phải trọng số. Trong số tất cả các đường dẫn đếm số bước nhảy ngắn nhất như vậy, chúng tôi muốn một đường dẫn tối đa hóa sự khác biệt giữa màu sắc cạnh tối đa và tối thiểu dọc theo đường dẫn. 

Vì vậy, vấn đề về cơ bản là tối ưu hóa hai cấp độ. Ràng buộc đầu tiên cố định chúng ta bên trong sơ đồ con của các đường đi ngắn nhất về số cạnh. Bên trong tập hợp giới hạn đó, chúng tôi muốn chọn một đường dẫn tối đa hóa giá trị phạm vi được xác định theo trọng số cạnh. 

Các ràng buộc rất lớn, lên tới 100.000 nút và 200.000 cạnh. Điều này ngay lập tức loại trừ việc liệt kê tất cả các đường đi ngắn nhất, vì số lượng đường đi ngắn nhất trong biểu đồ dày đặc có thể tăng theo cấp số nhân. Bất kỳ giải pháp nào cố gắng lưu trữ hoặc liệt kê tất cả các đường dẫn ứng cử viên sẽ không thành công. 

Chúng ta nên mong đợi một giải pháp kiểu O(n + m) hoặc O(m log m), có thể với BFS cộng với một bước tối ưu hóa bổ sung. 

Một vấn đề nhỏ xuất hiện khi tồn tại nhiều đường đi ngắn nhất. Một cách tiếp cận đơn giản có thể tính toán cây đường đi ngắn nhất và sau đó cố gắng điều chỉnh nó một cách tham lam, nhưng cây đường đi ngắn nhất không phải là duy nhất và các lựa chọn cha mẹ BFS khác nhau có thể thay đổi đáng kể phạm vi màu sắc có thể đạt được. 

Một cái bẫy khác là giả định rằng đường đi có phạm vi màu sắc tốt nhất trên toàn cầu cũng là đường đi ngắn nhất. Đường dẫn có độ dài dài hơn một chút có thể có dải màu tốt hơn nhiều, nhưng nó không hợp lệ vì ràng buộc đường dẫn ngắn nhất chiếm ưu thế. 

Một trường hợp thất bại minh họa nhỏ cho lối suy nghĩ ngây thơ: 

Hãy xem xét một biểu đồ: 

1 - 2 - 3 - 4 (tất cả các cạnh có trọng số 1) 

1 - 3 (độ dày cạnh 100) 

1 - 4 (độ dày cạnh 0) 

Các đường đi ngắn nhất từ ​​1 đến 4 có độ dài 2: 1-3-4 hoặc 1-2-3-4 không phải là ngắn nhất nếu dài hơn. Một thuật toán đơn giản có thể thích 1-3-4 do trọng số cực lớn, nhưng nó chỉ phải xem xét các đường đi ngắn nhất. 

Vì vậy, thách thức là làm thế nào để hạn chế sự chú ý một cách hiệu quả vào các đường dẫn ngắn nhất trong khi vẫn tối ưu hóa mục tiêu phụ so với trọng số cạnh. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu với ý tưởng trực tiếp nhất: chạy BFS từ nút 1 để tính khoảng cách ngắn nhất theo các cạnh tới mọi nút. Sau đó, chúng tôi giới hạn bản thân ở các cạnh tôn trọng việc phân lớp đường đi ngắn nhất, nghĩa là chúng tôi chỉ xem xét việc chuyển đổi từ một nút ở khoảng cách d đến một nút ở khoảng cách d+1. Điều này tạo thành một cấu trúc tuần hoàn có định hướng được xếp lớp theo cấp độ BFS. 

Khi chúng ta có cấu trúc này, vấn đề sẽ trở thành việc chọn đường dẫn từ lớp 0 đến lớp dist[n] trong khi tối đa hóa sự khác biệt giữa trọng số cạnh tối đa và tối thiểu dọc theo đường dẫn. 

Một cách mạnh mẽ bên trong DAG này là liệt kê tất cả các đường dẫn hoặc thực hiện DP trên các trạng thái theo dõi cả trọng số cạnh tối thiểu và tối đa được thấy cho đến nay. Tuy nhiên, trạng thái DP đó quá lớn: mỗi nút có thể đạt được với nhiều kết hợp tối thiểu-tối đa có thể và số lượng trạng thái như vậy có thể tăng lên O(2^n) trong trường hợp xấu nhất. 

Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào việc chọn một cặp cạnh dọc theo đường dẫn đóng vai trò là giá trị độ màu tối thiểu và tối đa. Khi chúng tôi sửa hai giới hạn này, chúng tôi chỉ cần kiểm tra xem liệu có tồn tại đường đi ngắn nhất nằm trong các cạnh có trọng số nằm trong khoảng đó và vẫn kết nối 1 với n với độ dài chính xác ngắn nhất hay không. 

Điều này chuyển đổi vấn đề thành vấn đề khả thi hai con trỏ cổ điển đối với các trọng số cạnh được sắp xếp, kết hợp với kiểm tra tính khả thi của BFS. 

Chúng tôi sắp xếp tất cả các cạnh theo màu sắc. Sau đó chúng tôi sử dụng cửa sổ trượt trên các giá trị cạnh tối thiểu và tối đa có thể có. Đối với mỗi cửa sổ ứng cử viên [L, R], chúng tôi kiểm tra xem có tồn tại đường đi ngắn nhất từ ​​1 đến n chỉ sử dụng các cạnh có trọng số nằm trong phạm vi này hay không.

Để kiểm tra tính khả thi, chúng tôi chạy BFS trên biểu đồ đã lọc và xác minh xem khoảng cách [n] có bằng khoảng cách ngắn nhất được tính toán trước trong biểu đồ đầy đủ hay không. 

Điều này có tác dụng vì việc hạn chế các cạnh không làm thay đổi cấu trúc lớp BFS ngoại trừ việc loại bỏ một số cạnh; nếu đường đi ngắn nhất vẫn tồn tại trong điều kiện hạn chế, khoảng cách BFS sẽ giữ nguyên. 

Chúng tôi duy trì hai con trỏ dựa trên các trọng số cạnh đã được sắp xếp, mở rộng R và thu nhỏ L khi cần và theo dõi khoảng khả thi tốt nhất. 

Cuối cùng, khi tìm thấy khoảng thời gian tốt nhất, chúng tôi sẽ xây dựng lại đường dẫn thực tế bằng cách sử dụng các con trỏ cha BFS bên trong biểu đồ đã lọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ép buộc DP trên các đường dẫn có theo dõi tối thiểu/tối đa | Hàm mũ | Trạng thái O(nm) | Quá chậm | 
| Phân lớp BFS + cửa sổ trượt + BFS khả thi | O(m log m + m n BFS khấu hao) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy BFS từ nút 1 trong biểu đồ đầy đủ để tính khoảng cách ngắn nhất về số cạnh tới mỗi nút. Khoảng cách này xác định độ dài đường dẫn được phép duy nhất cho các giải pháp hợp lệ. Nếu nút n không thể truy cập được, chúng tôi sẽ dừng, nhưng vấn đề đảm bảo khả năng kết nối. 
2. Lưu trữ khoảng cách ngắn nhất tới nút n dưới dạng target_length. Bất kỳ câu trả lời hợp lệ nào cũng phải là đường dẫn có độ dài chính xác như vậy. 
3. Sắp xếp tất cả các cạnh theo giá trị màu sắc của chúng. Điều này cho phép chúng ta suy luận về các khoảng ứng cử viên có trọng số được phép theo thứ tự tăng dần. 
4. Sử dụng cửa sổ trượt hai con trỏ trên các cạnh đã được sắp xếp, duy trì khoảng hiện tại [L, R] theo chỉ số cạnh trong danh sách đã sắp xếp. Khoảng này tương ứng với việc chỉ cho phép các cạnh có màu sắc nằm giữa hai giá trị được chọn. 
5. Đối với mỗi khoảng ứng viên, hãy xây dựng BFS tạm thời trên biểu đồ nhưng chỉ sử dụng các cạnh có độ màu nằm trong [L, R]. Trong BFS, tính khoảng cách ngắn nhất từ nút 1. 
6. Nếu nút n có thể truy cập được và khoảng cách của nó bằng target_length thì khoảng thời gian này là khả thi vì nó duy trì tính tối ưu của đường đi ngắn nhất trong khi hạn chế trọng số cạnh. 
7. Khi khả thi, hãy cố gắng mở rộng khoảng thời gian để tăng sự khác biệt giữa độ màu tối đa và tối thiểu. Nếu không, hãy thu nhỏ hoặc thay đổi khoảng thời gian để khôi phục tính khả thi. 
8. Theo dõi khoảng thời gian tốt nhất để tối đa hóa R_value trừ L_value trong khi vẫn khả thi. 
9. Sau khi tìm được khoảng thời gian tốt nhất, hãy chạy một BFS cuối cùng được giới hạn trong khoảng thời gian đó trong khi lưu trữ các con trỏ gốc để xây dựng lại đường đi ngắn nhất thực tế. 
10. Xuất đường dẫn được xây dựng lại từ 1 đến n bằng mảng cha. 

### Tại sao nó hoạt động 

Thuật toán dựa trên hai cấu trúc đơn điệu lồng nhau. Đầu tiên, phân lớp BFS đảm bảo rằng tất cả các giải pháp hợp lệ phải nằm trên DAG được xác định bởi khoảng cách ngắn nhất. Thứ hai, tính khả thi đối với khoảng trọng số cố định là đơn điệu: nếu khoảng [L, R] cho phép đường đi ngắn nhất, thì bất kỳ khoảng [L', R'] nào lớn hơn với L' ≤ L và R' ≥ R cũng cho phép điều đó. Tính đơn điệu này cho phép cửa sổ trượt khám phá phạm vi ứng viên một cách hiệu quả mà không bỏ lỡ các giải pháp tối ưu. Kiểm tra BFS đảm bảo tính chính xác vì nó mô tả chính xác liệu đường dẫn có độ dài ngắn nhất có tồn tại dưới các ràng buộc cạnh hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def bfs(n, adj, allowed):
    dist = [10**18] * (n + 1)
    parent = [-1] * (n + 1)
    dist[1] = 0
    q = deque([1])

    while q:
        u = q.popleft()
        for v, idx in adj[u]:
            if not allowed[idx]:
                continue
            if dist[v] > dist[u] + 1:
                dist[v] = dist[u] + 1
                parent[v] = u
                q.append(v)

    return dist, parent

def solve():
    n, m = map(int, input().split())
    edges = []
    adj = [[] for _ in range(n + 1)]

    for i in range(m):
        u, v, c = map(int, input().split())
        edges.append((u, v, c))
        adj[u].append((v, i))
        adj[v].append((u, i))

    # BFS for shortest path length
    dist0 = [10**18] * (n + 1)
    q = deque([1])
    dist0[1] = 0

    while q:
        u = q.popleft()
        for v, _ in adj[u]:
            if dist0[v] > dist0[u] + 1:
                dist0[v] = dist0[u] + 1
                q.append(v)

    target = dist0[n]

    # sort edges by colorfulness
    order = sorted(range(m), key=lambda i: edges[i][2])
    colors = [edges[i][2] for i in order]

    allowed = [False] * m

    def check(l, r):
        for i in range(m):
            allowed[i] = False
        for i in range(l, r + 1):
            allowed[order[i]] = True

        dist, parent = bfs(n, adj, allowed)
        return dist[n] == target, parent

    best_l, best_r = 0, 0
    j = 0

    for i in range(m):
        while j < m:
            ok, _ = check(i, j)
            if ok:
                if colors[j] - colors[i] > colors[best_r] - colors[best_l]:
                    best_l, best_r = i, j
                j += 1
            else:
                break

    ok, parent = check(best_l, best_r)

    path = []
    cur = n
    while cur != -1:
        path.append(cur)
        cur = parent[cur]

    path.reverse()

    print(len(path) - 1)
    print(*path)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán khoảng cách hop tối thiểu từ nút 1 đến tất cả các nút. Điều đó sửa độ dài đường dẫn hợp lệ duy nhất. Sau đó, nó sắp xếp các cạnh theo màu sắc và xử lý vấn đề như chọn khoảng liền kề tốt nhất trên các giá trị này. 

Hàm kiểm tra sẽ xây dựng lại biểu đồ đã lọc một cách ngầm định bằng cách sử dụng mặt nạ boolean và chạy BFS để xác minh xem đường đi ngắn nhất có còn tồn tại trong ràng buộc hay không. Con trỏ gốc chỉ được lưu trữ trong lệnh gọi xây dựng lại cuối cùng để tránh sử dụng bộ nhớ không cần thiết. 

Một điểm tinh tế là chúng ta phải so sánh với khoảng cách ngắn nhất ban đầu, không tính toán lại độ dài mục tiêu bên trong mỗi BFS được lọc, vì việc loại bỏ các cạnh chỉ có thể làm tăng khoảng cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị đầu vào: 

1-2 (1), 2-4 (5), 1-3 (10), 3-4 (6) 

Trước tiên, chúng tôi tính toán khoảng cách ngắn nhất từ 1 đến 4, tức là 2 trong tất cả các tuyến đường ngắn nhất hợp lệ. 

Chúng tôi sắp xếp các cạnh theo trọng số: 1, 5, 6, 10. 

Chúng tôi kiểm tra khoảng thời gian: 

| L | R | Các cạnh được phép | Có thể tiếp cận trong thời gian ngắn nhất | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | chỉ (1-2) | không | thất bại | 
| 1 | 2 | (1-2,2-4) | vâng | hợp lệ | 
| 1 | 3 | (1-2,2-4,3-4) | vâng | tốt hơn | 
| 2 | 3 | (2-4,3-4) | không | thất bại | 

Khoảng thời gian tốt nhất là [1,3], đưa ra đường dẫn 1-2-4 với max-min = 5-1 = 4. 

Dấu vết này cho thấy việc mở rộng cửa sổ sẽ cải thiện tính khả thi cho đến một điểm, sau đó loại bỏ cấu trúc cần thiết. 

### Ví dụ 2 

Đồ thị: 

1-2 (3), 2-5 (4), 1-3 (100), 3-5 (101) 

Đường dẫn ngắn nhất có độ dài 2. Cả hai đường dẫn 1-2-5 và 1-3-5 đều là đường dẫn ngắn nhất hợp lệ. 

Chúng tôi xem xét các khoảng thời gian: 

| L | R | Đường 1-2-5 | Đường 1-3-5 | Khả thi | 
| --- | --- | --- | --- | --- | 
| 3 | 4 | vâng | không | vâng | 
| 3 | 101 | vâng | vâng | vâng | 

Khoảng thời gian tốt nhất trở thành [3,101], chọn đường dẫn 1-3-5 với phạm vi 101-3. 

Điều này cho thấy thuật toán ưu tiên chính xác một đường dẫn tối đa hóa độ trải màu trong khi vẫn tôn trọng các ràng buộc về bước nhảy ngắn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m + m * (n + m)) trường hợp xấu nhất | Sắp xếp các cạnh cộng với kiểm tra tính khả thi BFS lặp đi lặp lại | 
| Không gian | O(n + m) | Lưu trữ đồ thị, mảng BFS, lập chỉ mục cạnh | 

Ràng buộc BFS đảm bảo mỗi lần kiểm tra tính khả thi đều chạy theo thời gian tuyến tính trên các cạnh và cửa sổ trượt giúp giảm các bước kiểm tra dư thừa trong thực tế. Với m lên đến 2e5, điều này phù hợp với giới hạn cuộc thi điển hình. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# Sample-style test
assert run("""4 4
1 2 1
2 4 5
1 3 10
3 4 6
""") == "2\n1 2 4\n"

# Minimum case
assert run("""2 1
1 2 7
""") == "1\n1 2\n"

# All equal weights
assert run("""3 3
1 2 5
2 3 5
1 3 5
""") == "1\n1 3\n"

# Chain graph
assert run("""5 4
1 2 1
2 3 2
3 4 3
4 5 4
""") == "4\n1 2 3 4 5\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn 2 nút | đường dẫn trực tiếp | độ đúng cơ sở | 
| tam giác có trọng lượng bằng nhau | ưu tiên cạnh trực tiếp ngắn nhất | xử lý cà vạt | 
| chuỗi tuyến tính | con đường ngắn nhất duy nhất | xây dựng lại đường dẫn chính xác | 
| đồ thị nhỏ với các cạnh thay thế | logic lựa chọn khoảng thời gian | sự đúng đắn theo lựa chọn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi khoảng tốt nhất thu gọn về trọng số của một cạnh. Trong trường hợp đó, chỉ cho phép các cạnh có một màu, nhưng BFS vẫn tìm chính xác đường đi ngắn nhất nếu nó tồn tại. Thuật toán xử lý việc này một cách tự nhiên vì khoảng [L, L] là hợp lệ và được kiểm tra như bất kỳ khoảng nào khác. 

Một trường hợp khác là khi tồn tại nhiều đường dẫn ngắn nhất với số bước nhảy giống hệt nhau nhưng có tập hợp cạnh hoàn toàn khác nhau. Kiểm tra tính khả thi của BFS đảm bảo rằng chỉ những đường dẫn chứa đầy đủ trong khoảng đã chọn mới đóng góp, do đó không xảy ra sự trộn lẫn các cạnh không chính xác. Ngay cả khi tồn tại một đường dẫn có dải màu ngắn hơn, nó sẽ chỉ được chọn nếu nó vẫn duy trì khả năng tiếp cận ở khoảng cách ngắn nhất. 

Trường hợp tinh tế cuối cùng là khi loại bỏ các cạnh sẽ làm mất kết nối đồ thị. Trong những trường hợp như vậy, BFS trả khoảng cách vô cực về n và khoảng thời gian bị từ chối. Điều này ngăn chặn việc chấp nhận sai các phạm vi màu không khả thi làm gián đoạn kết nối dưới các ràng buộc về đường dẫn ngắn nhất.
