---
title: "CF 105900M - Đường dẫn tối thiểu"
description: "Chúng ta được cung cấp một biểu đồ có trọng số có hướng trong đó các nút biểu thị các trạm xăng và các cạnh biểu thị đường một chiều với khoảng cách di chuyển. Chúng ta bắt đầu từ nút 1 và phải đến nút N. Mọi tuyến đường hợp lệ từ 1 đến N đều có hai tiêu chí cạnh tranh."
date: "2026-06-21T15:19:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105900
codeforces_index: "M"
codeforces_contest_name: "VI UnBalloon Contest Mirror"
rating: 0
weight: 105900
solve_time_s: 51
verified: true
draft: false
---

[CF 105900M - Đường dẫn tối thiểu](https://codeforces.com/problemset/problem/105900/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số có hướng trong đó các nút biểu thị các trạm xăng và các cạnh biểu thị đường một chiều với khoảng cách di chuyển. Chúng ta bắt đầu từ nút 1 và phải đến nút N. Mọi tuyến đường hợp lệ từ 1 đến N đều có hai tiêu chí cạnh tranh. 

Đầu tiên, tổng khoảng cách của tuyến đường phải càng nhỏ càng tốt. Trong số tất cả các tuyến đường đạt được tổng khoảng cách tối thiểu này, chúng tôi quan tâm đến mức độ “mượt mà” của tuyến đường đó về mặt tiêu thụ nhiên liệu giữa các điểm dừng. Độ mịn được đo bằng trọng lượng cạnh đơn lớn nhất được sử dụng dọc theo đường dẫn. Mục tiêu cuối cùng là chọn đường đi ngắn nhất và trong số đó, giảm thiểu trọng số cạnh tối đa. 

Vì vậy, đầu ra là hai giá trị. Đầu tiên là khoảng cách ngắn nhất có thể từ 1 đến N. Thứ hai là, trong số tất cả các đường đi ngắn nhất, giá trị tối thiểu có thể có của cạnh lớn nhất được sử dụng trên đường đi đó. 

Các ràng buộc lên tới 200.000 nút và cạnh, với trọng số lên tới 100.000. Điều này ngay lập tức loại trừ bất cứ thứ gì có dạng bậc ba hoặc thậm chí bậc hai. Chạy Dijkstra một lần là được, nhưng bất cứ điều gì liên quan đến việc tính toán lại các đường đi ngắn nhất trên mỗi cạnh hoặc liệt kê tất cả các đường dẫn thì không. 

Một vấn đề nhỏ xuất hiện khi tồn tại nhiều đường đi ngắn nhất. Một thuật toán đường đi ngắn nhất ngây thơ chỉ theo dõi khoảng cách sẽ vui vẻ trả về bất kỳ đường đi ngắn nhất nào, nhưng không nhất thiết phải là đường đi có nút thắt cổ chai tốt nhất. Một cạm bẫy khác là cố gắng tối ưu hóa nút thắt cổ chai trước tiên, vì đường dẫn tối thiểu thắt cổ chai có thể không ngắn nhất về tổng khoảng cách. 

Một xung đột minh họa nhỏ: 

đầu vào:```
3 3
1 2 1
2 3 10
1 3 5
```Khoảng cách ngắn nhất là 6 qua 1 → 2 → 3, nhưng đường đi trực tiếp 1 → 3 có khoảng cách 5 và không hợp lệ cho tiêu chí đầu tiên. Trong số các đường đi ngắn nhất, chỉ tồn tại 1 → 2 → 3 nên đáp án là (11, 10). Một cách ngây thơ “giảm thiểu cạnh tối đa trước” sẽ chọn sai 1 → 3. 

Một trường hợp khác là khoảng cách ngắn nhất bằng nhau với các nút thắt khác nhau:```
3 3
1 2 1
2 3 2
1 3 3
```Cả 1→2→3 và 1→3 đều có khoảng cách 3. Điểm thứ nhất có nút cổ chai 2, điểm thứ hai có điểm thắt cổ chai 3. Câu trả lời đúng là (3, 2). 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liệt kê tất cả các đường dẫn từ 1 đến N, tính tổng khoảng cách và trọng lượng cạnh tối đa của chúng, sau đó chọn cặp tốt nhất theo thứ tự từ điển. Điều này đúng về mặt khái niệm nhưng không khả thi ngay lập tức. Ngay cả trong biểu đồ thưa thớt, số lượng đường dẫn có thể tăng theo cấp số nhân, dễ dàng đạt tới 2^N trong các cấu trúc giống DAG. Mỗi lần đánh giá đường dẫn đều tốn O(N), do đó tổng số sẽ trở thành hàm mũ và không thể sử dụng được. 

Một nỗ lực có cấu trúc hơn là chạy Dijkstra một lần với khoảng cách ngắn nhất, sau đó hạn chế sự chú ý chỉ đến các cạnh nằm trên ít nhất một đường đi ngắn nhất. Điều này dẫn đến việc xây dựng một đồ thị con trong đó cạnh (u, v) chỉ được phép nếu dist[u] + w == dist[v]. Điều này hiệu quả vì bất kỳ con đường ngắn nhất nào cũng phải thỏa mãn sự bình đẳng này ở mỗi bước. 

Khi chúng ta có DAG đường đi ngắn nhất này, vấn đề sẽ giảm xuống việc chọn đường dẫn từ 1 đến N bên trong nó để giảm thiểu trọng số cạnh tối đa. Đó là một bài toán cổ điển “tối thiểu hóa cạnh tối đa trên một đường đi”, có thể giải được bằng một phép tính đường đi ngắn nhất khác trong đó chi phí đường đi là cạnh tối đa dọc theo tuyến đường thay vì tổng. Kể từ khi thư giãn cạnh trở thành`max(current, w)`, chúng ta có thể chạy quy trình giống Dijkstra thứ hai trên DAG. 

Cái nhìn sâu sắc quan trọng là sự tách biệt các mục tiêu. Điều kiện đường đi ngắn nhất xác định một bề mặt ràng buộc và bên trong bề mặt đó, chúng tôi tối ưu hóa mục tiêu phụ. Cấu trúc của Dijkstra đảm bảo rằng tất cả các khoảng cách ngắn nhất đều được cố định trước khi chúng ta tính đến chi phí thứ cấp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Dijkstra hai pha (DP + nút cổ chai bị ràng buộc) | O((N + M) log N) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Chiến lược tối ưu 

1. Chạy Dijkstra từ nút 1 để tính khoảng cách ngắn nhất tới mọi nút. Điều này thiết lập tổng chi phí tối thiểu có thể để đến được mỗi trạm. Nếu không có điều này, chúng ta không thể xác định “đường dẫn ngắn nhất” có nghĩa là gì cục bộ tại mỗi nút. 
2. Xây dựng cấu trúc kề được lọc chỉ giữ các cạnh (u, v, w) thỏa mãn dist[u] + w == dist[v]. Điều này đảm bảo rằng mọi cạnh được giữ lại đều tương thích với ít nhất một đường đi ngắn nhất. Bất kỳ cạnh nào vi phạm điều kiện này không bao giờ có thể xuất hiện theo đường đi ngắn nhất hợp lệ, vì nó sẽ làm tăng tổng khoảng cách một cách nghiêm ngặt. 
3. Trên biểu đồ được lọc này, giờ đây chúng tôi tính toán giá trị DP thứ cấp: trọng số cạnh tối đa tối thiểu có thể có dọc theo bất kỳ đường dẫn nào từ 1 đến mỗi nút, chỉ giới hạn ở các đường dẫn ngắn nhất. 
4. Khởi tạo quy trình giống Dijkstra thứ hai trong đó chi phí trạng thái tại nút v là giá trị nhỏ nhất có thể có của cạnh tối đa được sử dụng cho đến nay. Đặt giá trị nguồn thành 0. 
5. Sử dụng hàng đợi ưu tiên được sắp xếp theo giá trị nút cổ chai này. Từ nút u, khi đi qua một cạnh (u, v, w), chi phí mới trở thành max(current_cost, w). Mô hình này mô hình chính xác cạnh tối đa gặp dọc theo đường dẫn. 
6. Hãy thư giãn v nếu giá trị thắt cổ chai mới này cải thiện giá trị tốt nhất hiện tại của nó. 
7. Sau khi xử lý, câu trả lời cho nút N là dist[N] là khoảng cách ngắn nhất và best_bottleneck[N] là cạnh tối đa được thu nhỏ nhất trong ràng buộc đường đi ngắn nhất. 

### Tại sao nó hoạt động 

Dijkstra đầu tiên khóa ở khoảng cách chính xác ngắn nhất. Điều đó biến biểu đồ thành một cấu trúc phân lớp trong đó mọi bước di chuyển hợp lệ phải duy trì mức tăng khoảng cách tối ưu. Bên trong cấu trúc này, bất kỳ đường đi nào từ 1 đến N đều tương ứng với đường đi ngắn nhất trong biểu đồ gốc. 

Bởi vì mọi cạnh được giữ lại đều duy trì tính tối ưu của đường đi ngắn nhất cục bộ, nên việc ghép các cạnh đó sẽ duy trì khoảng cách tối ưu toàn cục. Điều này có nghĩa là chúng ta không tìm kiếm trên tất cả các đường đi nữa mà chỉ tìm kiếm trên tập hợp các đường đi ngắn nhất. 

Trong tập hợp giới hạn đó, quy trình thứ hai khám phá tất cả các đường dẫn ngắn nhất khả thi nhưng so sánh chúng bằng cách sử dụng hàm chi phí đơn điệu, cạnh tối đa được thấy cho đến nay. Chức năng này tương thích với kiểu thư giãn kiểu Dijkstra vì việc mở rộng đường dẫn không thể làm giảm cạnh tối đa của nó. Điều đó đảm bảo việc mở rộng tham lam vẫn còn hiệu lực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq
from collections import defaultdict

def dijkstra_dist(n, adj):
    INF = 10**30
    dist = [INF] * (n + 1)
    dist[1] = 0
    pq = [(0, 1)]
    
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    
    return dist

def dijkstra_bottleneck(n, adj, dist):
    INF = 10**30
    best = [INF] * (n + 1)
    best[1] = 0
    pq = [(0, 1)]
    
    while pq:
        cur, u = heapq.heappop(pq)
        if cur != best[u]:
            continue
        for v, w in adj[u]:
            if dist[u] + w != dist[v]:
                continue
            nc = max(cur, w)
            if nc < best[v]:
                best[v] = nc
                heapq.heappush(pq, (nc, v))
    
    return best

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        a, b, w = map(int, input().split())
        adj[a].append((b, w))
    
    dist = dijkstra_dist(n, adj)
    best = dijkstra_bottleneck(n, adj, dist)
    
    print(dist[n], best[n])

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng danh sách lân cận được định hướng đầy đủ. Dijkstra đầu tiên tính toán chính xác khoảng cách ngắn nhất từ ​​nút 1, sau này được sử dụng làm điều kiện lọc. 

Giai đoạn thứ hai sử dụng lại danh sách lân cận tương tự nhưng chỉ cho phép các chuyển đổi duy trì sự bằng nhau về đường đi ngắn nhất. Quá trình lọc này được thực hiện nội tuyến trong quá trình thư giãn thay vì tạo trước biểu đồ thứ hai, giúp tiết kiệm bộ nhớ và thời gian. 

Một chi tiết triển khai quan trọng là kiểm tra tính bằng nhau`dist[u] + w != dist[v]`. Điều này phải chính xác; sử dụng`>`hoặc`<`so sánh sẽ cho phép các cạnh không ngắn nhất một cách không chính xác hoặc loại bỏ các cạnh hợp lệ. 

DP thắt cổ chai sử dụng hàng đợi ưu tiên thứ hai vì không gian trạng thái vẫn được hưởng lợi từ việc mở rộng tham lam. Mặc dù chi phí không tăng thêm, hàm tối đa vẫn đơn điệu dọc theo các phần mở rộng, làm cho việc sắp xếp kiểu Dijkstra trở nên hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6
1 2 1
1 3 10
1 4 1
2 3 3
3 5 2
4 5 5
```Dijkstra đầu tiên cho khoảng cách ngắn nhất: 

1=0, 2=1, 4=1, 3=4, 5=6 

Các cạnh được lọc chỉ giữ những cạnh phù hợp với đường đi ngắn nhất: 

1→2, 2→3, 3→5, 1→4, 4→5 

Bây giờ chúng ta tính toán nút cổ chai DP. 

| Bước | Nút | Giá trị nút cổ chai | Cập nhật | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 0 | bắt đầu | 
| thư giãn | 2 | tối đa(0,1)=1 | cập nhật 2 | 
| thư giãn | 4 | tối đa(0,1)=1 | cập nhật 4 | 
| quá trình 2 | 3 | tối đa(1,3)=3 | cập nhật 3 | 
| quá trình 4 | 5 | tối đa(1,5)=5 | ứng cử viên | 
| quá trình 3 | 5 | tối đa(3,2)=3 | cải thiện | 

Đường đi tốt nhất cuối cùng là 1→2→3→5 với nút cổ chai 3, tổng khoảng cách 6. 

### Ví dụ 2 

đầu vào:```
2 2
1 2 1
1 2 2
```Khoảng cách ngắn nhất chỉ là 1 qua cạnh đầu tiên. Cạnh thứ hai không liên quan vì nó không cải thiện khoảng cách. 

Biểu đồ đã lọc chỉ giữ lại cạnh (1,2,1). Nút cổ chai là 1. 

Điều này cho thấy các cạnh không bảo toàn khoảng cách ngắn nhất sẽ bị loại bỏ hoàn toàn trước khi tối ưu hóa thứ cấp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log N) | Hai lần chạy Dijkstra, mỗi lần chạy xử lý mọi cạnh bằng các thao tác heap | 
| Không gian | O(N + M) | danh sách kề cộng với mảng khoảng cách và nút cổ chai | 

Các ràng buộc cho phép lên tới 200.000 cạnh, do đó, hai lần duyệt đồ thị dựa trên heap vẫn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return run.capture(inp)

# We redefine properly for isolation
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""5 6
1 2 1
1 3 10
1 4 1
2 3 3
3 5 2
4 5 5
""") == "6 3"

# sample 2
assert run("""2 2
1 2 1
1 2 2
""") == "1 1"

# custom 1: tie shortest paths different bottleneck
assert run("""3 3
1 2 1
2 3 2
1 3 3
""") == "3 2"

# custom 2: multiple equal shortest paths
assert run("""4 4
1 2 1
2 4 1
1 3 1
3 4 1
""") == "2 1"

# custom 3: larger bottleneck forced
assert run("""4 4
1 2 1
2 4 100
1 3 1
3 4 1
""") == "2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| buộc những con đường ngắn nhất | 3 2 | độ chính xác tối ưu hóa thứ cấp | 
| đường đi ngắn nhất đối xứng | 2 1 | phân nhánh chi phí bằng nhau | 
| đường chặn cạnh lớn | 2 1 | lọc các cạnh không ngắn nhất | 

## Vỏ cạnh 

Một lỗi phổ biến là xử lý tối ưu hóa nút cổ chai một cách độc lập với việc tính toán đường đi ngắn nhất. Trong ví dụ trong đó cạnh trực tiếp có bước nhảy ngắn hơn nhưng có trọng lượng dài hơn, cách tiếp cận minimax ngây thơ sẽ chọn nó không chính xác. 

Thuật toán xử lý việc này vì các cạnh như vậy sẽ bị loại bỏ trong lần vượt qua khoảng cách đầu tiên. Ví dụ:```
3 3
1 2 1
2 3 10
1 3 5
```Dijkstra cho dist[3]=5. Cạnh 2→3 không nằm trên bất kỳ đường đi ngắn nhất nào vì 1→2→3 tổng là 11, vượt quá 5 nên nó bị bỏ qua. Cấu trúc đường dẫn còn lại chỉ cho phép các tuyến đường ngắn nhất hợp lệ và nút cổ chai chỉ được tính toán trong số đó, đảm bảo tính chính xác.
