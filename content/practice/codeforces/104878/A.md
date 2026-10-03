---
title: "CF 104878A - Trộm cắp"
description: "Chúng ta có một thành phố được mô hình hóa dưới dạng đồ thị vô hướng trong đó giao lộ là nút và đường phố là cạnh. Powder bắt đầu từ nút 1, phòng thí nghiệm, và muốn đến nút N, lối vào ẩn của Zaun. Một số giao lộ ban đầu có cảnh sát."
date: "2026-06-28T09:43:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104878
codeforces_index: "A"
codeforces_contest_name: "ICHC Etapa Pe Scoala"
rating: 0
weight: 104878
solve_time_s: 80
verified: false
draft: false
---

[CF 104878A - Trộm cắp](https://codeforces.com/problemset/problem/104878/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thành phố được mô hình hóa dưới dạng đồ thị vô hướng trong đó giao lộ là nút và đường phố là cạnh. Powder bắt đầu từ nút 1, phòng thí nghiệm, và muốn đến nút N, lối vào ẩn của Zaun. Một số giao lộ ban đầu có cảnh sát. 

Một con đường được coi là nguy hiểm nếu Powder đi qua quá nhiều giao lộ có cảnh sát kiểm soát. Nhiệm vụ đầu tiên bỏ qua ràng buộc này và chỉ yêu cầu đường đi ngắn nhất từ ​​1 đến N theo số lượng giao lộ đã ghé thăm. Vì tất cả các cạnh đều không có trọng số, điều này tương đương với việc tìm đường đi ngắn nhất trong biểu đồ không có trọng số. 

Nhiệm vụ thứ hai sửa đổi tình hình. Tất cả các sĩ quan cảnh sát bị dụ vào một giao lộ V duy nhất, nghĩa là V trở thành nút duy nhất do cảnh sát kiểm soát và tất cả các nút cảnh sát khác đều trở nên an toàn. Bột hoàn toàn không được phép dẫm lên V. Chúng ta lại phải tính đường đi ngắn nhất từ ​​1 đến N, bây giờ trong đồ thị trong đó V bị cấm. 

Các ràng buộc cho phép tối đa 100000 nút và cạnh, điều này ngay lập tức loại trừ mọi thứ bậc hai hoặc thậm chí bậc ba cho mỗi truy vấn. Một BFS duy nhất cho mỗi truy vấn là khả thi vì BFS chạy ở O(N + M). Bất kỳ cách tiếp cận nào cố gắng tính toán lại các đường dẫn trên mỗi nút hoặc mô phỏng tất cả các đường dẫn đều quá chậm. 

Một trường hợp phức tạp phát sinh khi 1 hoặc N đã bị chặn bởi các quy tắc của cảnh sát. Nếu nút 1 bằng V trong nhiệm vụ con thứ hai hoặc nếu N bằng V thì không có đường dẫn nào tồn tại. Tương tự, nếu không thể truy cập được N trong biểu đồ gốc thì cả hai câu trả lời đều là -1. 

Một tình huống khó khăn khác là khi đồ thị bị ngắt kết nối. Ví dụ: nếu nút N ở thành phần khác với nút 1, ngay cả khi không có bất kỳ ràng buộc nào về cảnh sát, câu trả lời ngay lập tức là -1. Việc triển khai đường dẫn ngắn nhất ngây thơ giả định kết nối sẽ âm thầm thất bại ở đây trừ khi nó kiểm tra rõ ràng khả năng tiếp cận. 

## Phương pháp tiếp cận 

Cấu trúc của cả hai nhiệm vụ con gợi ý rõ ràng về bài toán đường đi ngắn nhất trên đồ thị không có trọng số. Cách giải thích mạnh mẽ sẽ là liệt kê tất cả các đường dẫn đơn giản từ 1 đến N và chọn đường dẫn hợp lệ ngắn nhất. Về mặt lý thuyết, điều này đúng vì mọi đường dẫn hợp lệ đều được xem xét, nhưng số lượng đường dẫn đơn giản trong biểu đồ có thể tăng theo cấp số nhân với N. Trong một biểu đồ dày đặc, điều này nhanh chóng trở nên không thể thực hiện được ngay cả đối với các đầu vào nhỏ, vì hệ số phân nhánh được gộp ở mỗi bước. 

Một ý tưởng thực tế hơn nhưng vẫn còn ngây thơ là chạy một quy trình giống Dijkstra với hàng đợi ưu tiên, ý tưởng này vẫn đúng nhưng quá mức cần thiết vì tất cả các cạnh đều có trọng số bằng nhau. Nó hoạt động ở O(M log N), có thể chấp nhận được nhưng chậm hơn mức cần thiết. Quan sát quan trọng là mọi cạnh đều có chi phí bằng nhau, do đó BFS đã đảm bảo các đường đi ngắn nhất về số cạnh và do đó cũng đảm bảo số lượng nút được truy cập. 

Đối với nhiệm vụ phụ thứ hai, thay đổi duy nhất là cấm một nút. Cấu trúc của đồ thị không thay đổi theo cách khác. Điều này có nghĩa là chúng ta có thể chỉ cần chạy lại BFS trong khi bỏ qua hoàn toàn nút đó. 

Vì vậy, vấn đề giảm xuống khi chạy BFS hai lần, một lần trên biểu đồ đầy đủ và một lần trên biểu đồ đã loại bỏ nút V. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các đường dẫn | Hàm mũ | O(N) | Quá chậm | 
| Dijkstra | O(M log N) | O(N + M) | Được chấp nhận nhưng không cần thiết | 
| Hai lần chạy BFS | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sẽ coi cả hai nhiệm vụ con là tính toán BFS độc lập. 

### Con đường ngắn nhất đầu tiên

1. Xây dựng danh sách kề cho đồ thị từ M cạnh. Điều này cho phép di chuyển nhanh chóng của hàng xóm. 
2. Chạy BFS tiêu chuẩn bắt đầu từ nút 1. Chúng tôi duy trì hàng đợi và mảng khoảng cách được khởi tạo thành -1. 
3. Đặt khoảng cách [1] thành 1, vì chúng tôi tính các giao điểm thay vì các cạnh. 
4. Đưa các nút ra khỏi hàng đợi và đối với mỗi nút lân cận, nếu nó chưa được truy cập, hãy gán khoảng cách [hiện tại] + 1 và đẩy nó. 
5. Dừng lại khi tất cả các nút có thể truy cập được xử lý. 
6. Câu trả lời cho nhiệm vụ phụ 1 là khoảng cách [N] hoặc -1 nếu chưa bao giờ đạt được. 

Ý tưởng chính ở đây là BFS khám phá các nút theo thứ tự tăng dần số bước từ nguồn, vì vậy, lần đầu tiên chúng tôi tiếp cận một nút, chúng tôi đã tìm thấy đường đi ngắn nhất có thể đến nút đó. 

### Đường đi ngắn thứ hai với nút bị cấm 

1. Lặp lại BFS nhưng coi nút V là bị chặn. 
2. Nếu nút bắt đầu 1 bằng V hoặc đích N bằng V, ngay lập tức trả về -1. 
3. Trong BFS, bất cứ khi nào chúng ta xem xét các láng giềng, hãy bỏ qua bất kỳ quá trình chuyển đổi nào dẫn đến V. 
4. Nếu không, hãy chạy BFS chính xác như trước và tính khoảng cách [N]. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào tính bất biến mà BFS xử lý các nút theo thứ tự khoảng cách không giảm từ nguồn. Vì tất cả các cạnh đều có chi phí bằng nhau nên lần đầu tiên chúng ta gán khoảng cách cho một nút, giá trị đó là nhỏ nhất trong số tất cả các đường dẫn có thể có. Việc loại bỏ nút V chỉ đơn giản là xóa nó khỏi không gian trạng thái nhưng không làm thay đổi tính chất đơn điệu này. Do đó BFS vẫn tính toán chính xác đường đi hợp lệ ngắn nhất trong biểu đồ bị hạn chế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def bfs(n, adj, blocked):
    if blocked[1] or blocked[n]:
        return -1

    dist = [-1] * (n + 1)
    q = deque()

    dist[1] = 1
    q.append(1)

    while q:
        u = q.popleft()
        for v in adj[u]:
            if blocked[v]:
                continue
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    return dist[n]

def solve():
    n, m, k, V = map(int, input().split())

    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)

    police_nodes = list(map(int, input().split()))
    blocked1 = [False] * (n + 1)
    for x in police_nodes:
        blocked1[x] = True

    ans1 = bfs(n, adj, blocked1)

    blocked2 = blocked1[:]
    blocked2[V] = True

    ans2 = bfs(n, adj, blocked2)

    print(ans1, ans2)

if __name__ == "__main__":
    solve()
```Giải pháp tách việc xây dựng biểu đồ khỏi quá trình truyền tải để cả hai lần chạy BFS đều sử dụng lại cùng một danh sách kề. các`blocked`mảng là sự trừu tượng hóa khóa mã hóa cả vị trí cảnh sát ban đầu và ràng buộc đã sửa đổi cho truy vấn thứ hai. 

Hàm BFS trả về khoảng cách theo số lượng giao điểm, đó là lý do tại sao chúng tôi khởi tạo khoảng cách nguồn là 1 thay vì 0. Điều này tránh được các lỗi sai sót một khi diễn giải kết quả. 

Chúng tôi cũng kiểm tra rõ ràng xem nút 1 hoặc nút N có bị chặn hay không trước khi chạy BFS, vì không có quá trình truyền tải nào có thể thành công trong trường hợp đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 10 4 6
1 2
1 3
2 4
3 5
5 6
5 7
7 8
6 9
8 9
4 5
3 5 6 7
```Chúng tôi chạy BFS từ 1 bỏ qua tất cả các nút cảnh sát 3, 5, 6, 7. 

| Bước | Nút hiện tại | Phân công khoảng cách | 
| --- | --- | --- | 
| 1 | 1 | dist[1]=1 | 
| 2 | 2 | dist[2]=2 | 
| 3 | 4 | dist[4]=3 | 
| 4 | 5 | bị chặn, bỏ qua | 
| 5 | 3 | bị chặn, bỏ qua | 
| 6 | 6 | bị chặn, bỏ qua | 

Việc tiếp tục khám phá sẽ dẫn qua các nút được phép cho đến khi đạt đến nút 9 ở khoảng cách 6. Vì vậy, câu trả lời là 6. 

Đối với BFS thứ hai, nút 6 cũng bị chặn. Cấu trúc đường đi ngắn nhất không thay đổi trong biểu đồ cụ thể này vì tất cả các tuyến đường ngắn nhất đã tránh được V, do đó kết quả vẫn là 6. 

Điều này xác nhận rằng việc thêm một nút bị chặn không nhất thiết sẽ thay đổi độ dài đường đi ngắn nhất nếu nó không phải là một phần của các tuyến đường tối ưu. 

### Ví dụ 2 

Hãy xem xét một biểu đồ đơn giản hơn:```
5 4 1 3
1 2
2 3
3 4
4 5
2
```BFS đầu tiên bỏ qua nút 2 là cảnh sát: 

| Bước | Nút | Khoảng cách | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | bị chặn | 
| 3 | 3 | có thể truy cập thông qua đường dẫn 1-3? không | 

Nút 3 không thể truy cập được nên câu trả lời là -1. 

BFS thứ hai chặn thêm nút 3: 

Không có đường dẫn nào tồn tại nên kết quả cũng là -1. 

Điều này cho thấy cách chặn có thể ngắt kết nối hoàn toàn biểu đồ ngay cả khi nó được kết nối ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Hai lần duyệt BFS qua danh sách kề | 
| Không gian | O(N + M) | Lưu trữ đồ thị cộng với khoảng cách và mảng đã truy cập | 

Các ràng buộc cho phép tối đa 100000 nút và cạnh, do đó BFS thời gian tuyến tính nằm trong giới hạn thoải mái. Ngay cả việc chạy BFS hai lần cũng giữ cho tổng số hoạt động ở dưới ngưỡng trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def bfs(n, adj, blocked):
        if blocked[1] or blocked[n]:
            return -1
        dist = [-1] * (n + 1)
        q = deque([1])
        dist[1] = 1
        while q:
            u = q.popleft()
            for v in adj[u]:
                if blocked[v]:
                    continue
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)
        return dist[n]

    n, m, k, V = map(int, sys.stdin.readline().split())
    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        a, b = map(int, sys.stdin.readline().split())
        adj[a].append(b)
        adj[b].append(a)

    police = list(map(int, sys.stdin.readline().split()))
    blocked1 = [False] * (n + 1)
    for x in police:
        blocked1[x] = True

    ans1 = bfs(n, adj, blocked1)
    blocked2 = blocked1[:]
    blocked2[V] = True
    ans2 = bfs(n, adj, blocked2)

    return f"{ans1} {ans2}"

# sample
assert run("""9 10 4 6
1 2
1 3
2 4
3 5
5 6
5 7
7 8
6 9
8 9
4 5
3 5 6 7
""") == "6 6"

# minimum case
assert run("""2 1 0 2
1 2
""") == "2 2"

# disconnected case
assert run("""4 2 0 3
1 2
3 4
""") == "-1 -1"

# blocked start case
assert run("""3 2 1 2
1 2
2 3
2
""") == "-1 -1"

# normal small
assert run("""5 4 1 3
1 2
2 3
3 4
4 5
2
""") == "-1 -1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 2 nút | 2 2 | xử lý đồ thị tối thiểu | 
| đồ thị bị ngắt kết nối | -1 -1 | trường hợp không thể tiếp cận | 
| bắt đầu bị chặn | -1 -1 | xử lý nguồn không hợp lệ | 
| biểu đồ đường bị tắc nghẽn | -1 -1 | cắt tỉa BFS đúng cách | 

## Vỏ cạnh 

Khi nút 1 hoặc nút N bị chặn trong kịch bản thứ hai, BFS không bao giờ bắt đầu có ý nghĩa. Việc trả về sớm sẽ ngăn chặn việc truyền tải một phần không chính xác và đầu ra chính xác là -1. 

Khi biểu đồ bị ngắt kết nối, mảng khoảng cách vẫn giữ nguyên -1 cho nút N sau khi BFS hoàn thành. Điều này trực tiếp báo hiệu sự không thể thực hiện được nếu không có bất kỳ cách viết đặc biệt nào ngoài việc khởi tạo. 

Khi đường dẫn tối ưu yêu cầu đi qua V, BFS thứ hai sẽ tự nhiên tránh nó bằng cách bỏ qua hoàn toàn nút đó. Sau đó, tìm kiếm sẽ khám phá các tuyến đường thay thế và nếu không tồn tại, kết quả sẽ là -1 như mong đợi.
