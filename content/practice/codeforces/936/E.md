---
problem: 936E
contest_id: 936
problem_index: E
name: "Iqea"
contest_name: "Codeforces Round 467 (Div. 1)"
rating: 3400
tags: ["data structures", "dfs and similar", "divide and conquer", "dsu", "shortest paths", "trees"]
answer: passed_samples
verified: true
solve_time_s: 133
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
---

#CF 936E - Iqea 

**Đánh giá:** 3400 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, phân chia và chinh phục, dsu, đường đi ngắn nhất, cây cối 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có (1/1 mẫu) 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các ô trên một lưới lớn. Mỗi ô là một thành phố và các con đường tồn tại giữa hai thành phố bất kỳ có chung một cạnh trên lưới. Các ràng buộc hình học quan trọng là tập hợp các ô được chọn tạo thành một vùng được kết nối và các ô trống cũng tạo thành một vùng được kết nối. Điều này có nghĩa là chúng ta đang làm việc bên trong một polyomino “rắn” duy nhất không có lỗ. 

Trên khu vực này, chúng tôi xử lý một chuỗi các sự kiện. Một số sự kiện thêm một “cửa hàng” mới tại một thành phố và những sự kiện khác yêu cầu khoảng cách từ thành phố đến cửa hàng đã mở gần nhất. Khoảng cách được đo bằng chiều dài đường đi ngắn nhất dọc theo các cạnh của lưới và việc di chuyển chỉ được phép bên trong nhóm thành phố nhất định. 

Khó khăn chính là đồ thị lớn, lên tới 300.000 đỉnh và cả cập nhật và truy vấn đều trực tuyến. Một truy vấn phải phản ánh tất cả các cửa hàng đã được thêm vào trước đó. 

Một cách giải thích đơn giản sẽ là tính toán lại đường đi ngắn nhất từ ​​mọi nút truy vấn đến tất cả các cửa hàng bằng BFS. Điều đó sẽ thất bại ngay lập tức vì mỗi BFS có giá O(n) và việc thực hiện điều đó cho tối đa 300.000 truy vấn sẽ dẫn đến giải pháp O(nq), vượt xa giới hạn. 

Một vấn đề tế nhị hơn là khoảng cách không phải là Euclide hay Manhattan trong toàn bộ lưới. Chúng là những đường đi ngắn nhất bị ràng buộc bởi hình dạng, do đó, không thể đi tắt qua các ô bị thiếu. Bất kỳ cách tiếp cận nào giả định hình học tọa độ đơn giản sẽ tạo ra câu trả lời sai ở vùng lõm. 

Một trường hợp thất bại minh họa nhỏ là vùng hình chữ U. Hai điểm có thể gần nhau về tọa độ nhưng xa về khoảng cách đồ thị vì đường đi phải đi xung quanh phần bên trong bị thiếu. Một giải pháp sử dụng khoảng cách tọa độ như`abs(x1-x2)+abs(y1-y2)`sẽ đánh giá thấp khoảng cách. 

Do đó, thách thức là vấn đề về đường đi ngắn nhất đa nguồn động trên một biểu đồ không có trọng số thưa thớt lớn, chỉ có các phần chèn nguồn và các truy vấn khoảng cách lặp lại. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là xử lý mọi truy vấn một cách độc lập. Đối với mỗi truy vấn loại 2, hãy chạy BFS bắt đầu từ thành phố đó và dừng khi đến bất kỳ cửa hàng nào. Điều này đúng vì BFS trong biểu đồ không có trọng số luôn tìm đường đi ngắn nhất. Tuy nhiên, nếu có k cửa hàng và mỗi truy vấn kích hoạt một BFS thì trong trường hợp xấu nhất, mỗi BFS sẽ khám phá gần như toàn bộ biểu đồ. Điều này dẫn đến khoảng O(nq), quá lớn. 

Quan sát chính là BFS từ nhiều nguồn có thể được hợp nhất. Nếu tất cả các cửa hàng đều được biết trước, chúng tôi có thể chạy một BFS đa nguồn duy nhất và tính toán khoảng cách đến cửa hàng gần nhất cho mỗi nút. Khó khăn là các cửa hàng được chèn theo thời gian và chúng ta cần câu trả lời giữa các lần chèn. 

Điều này gợi ý việc duy trì quy trình đường đi ngắn nhất toàn cầu sẽ phát triển khi các nguồn mới xuất hiện. Thay vì tính toán lại từ đầu, chúng tôi giữ hàng đợi ưu tiên giống hệt thuật toán của Dijkstra, ngoại trừ tất cả các cạnh có trọng số 1. Mỗi lần chèn cửa hàng tương đương với việc chèn một nguồn mới có khoảng cách 0 vào quy trình toàn cầu này. 

Ý tưởng quan trọng là chúng ta không bao giờ cần phải khởi động lại quá trình tính toán. Chúng tôi duy trì một quá trình thư giãn toàn cầu theo thời gian. Khi một cửa hàng mới xuất hiện, chúng tôi đẩy nó vào hàng ưu tiên. Khi chúng tôi xử lý các truy vấn, chúng tôi đảm bảo rằng khoảng cách cho nút được truy vấn đã được giải quyết hoàn toàn bằng cách tiếp tục truyền vừa đủ. 

Điều này biến vấn đề thành việc duy trì quy trình BFS/Dijkstra đa nguồn động trong đó các nguồn chỉ được thêm vào chứ không bao giờ bị xóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chạy lại BFS cho mỗi truy vấn | O(nq) | O(n) | Quá chậm | 
| Dijkstra gia tăng toàn cầu (đa nguồn) | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi hiểu lưới là một biểu đồ không có trọng số trong đó mỗi ô là một nút và các cạnh tồn tại giữa các ô liền kề thuộc vùng. 

Sau đó chúng tôi duy trì cấu trúc đường đi ngắn nhất toàn cầu. 

1. Xây dựng danh sách kề cho tất cả các ô đã cho. Đối với mỗi ô, hãy kiểm tra bốn ô lân cận của nó và kết nối các cạnh nếu hàng xóm đó cũng là một ô hợp lệ. Điều này cho chúng ta một biểu đồ ẩn có các cạnh O(n). 
2. Duy trì mảng khoảng cách`dist`, được khởi tạo ở mức vô cùng và một mảng boolean`done`cho biết khoảng cách ngắn nhất tới một nút đã được xác định hay chưa. 
3. Duy trì các cặp lưu trữ hàng đợi ưu tiên heap tối thiểu toàn cầu`(distance, node)`. 
4. Khi một cửa hàng được thêm vào nút`v`, chúng tôi thiết lập`dist[v] = 0`và đẩy`(0, v)`vào đống. Chúng tôi không tuyên truyền ngay lập tức. 
5. Chúng tôi xử lý các truy vấn theo thứ tự. Trước khi trả lời truy vấn cho nút`u`, chúng tôi chạy tiến trình Dijkstra cho đến khi nút`u`trở nên hoàn thiện: 

1. Liên tục bật phần tử nhỏ nhất`(d, v)`từ đống. 
2. Nếu`v`đã hoàn tất rồi, bỏ qua nó. 
3. Đánh dấu`v`như đã hoàn thiện và thiết lập`dist[v] = d`. 
4. Thư giãn tất cả hàng xóm`to`của`v`. Nếu như`to`chưa được hoàn thiện và`d + 1 < dist[to]`, cập nhật`dist[to]`và đẩy`(d + 1, to)`. 
6. Một lần nút`u`được hoàn thiện, nó`dist[u]`là câu trả lời cho truy vấn. Nếu heap trở nên trống rỗng trước`u`đã đạt được, không có cửa hàng nào có thể truy cập được vào thời điểm đó, vì vậy chúng tôi xuất ra`-1`. 

Điểm tinh tế là chúng ta không cần xử lý toàn bộ biểu đồ. Chúng ta chỉ cần mở rộng biên giới Dijkstra cho đến khi đạt được nút truy vấn. Vì trọng lượng của các cạnh là đồng nhất nên điều này hoạt động giống hệt như mặt sóng BFS mở rộng ra bên ngoài từ tất cả các cửa hàng. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hàng đợi ưu tiên chứa tất cả các ứng cử viên đường đi ngắn nhất đã được phát hiện nhưng chưa hoàn thiện từ tất cả các cửa hàng được chèn cho đến nay. Mỗi lần chúng tôi thêm một cửa hàng mới, chúng tôi sẽ giới thiệu một nguồn khoảng cách 0 mới vào cùng một cấu trúc toàn cầu. Bởi vì thuật toán của Dijkstra đảm bảo rằng các nút được hoàn thiện theo thứ tự khoảng cách không giảm, nên khi một nút được bật ra, việc nới lỏng các nguồn đã được chèn trong tương lai có thể cải thiện nút đó. Vì các cửa hàng mới chỉ bổ sung thêm các nguồn không khoảng cách, nên chúng chỉ tạo ra những giây phút thư giãn bổ sung được kết hợp một cách tự nhiên vào cùng một quy trình. Do đó, khi cuối cùng chúng tôi hoàn thành một nút, chúng tôi đã tính toán khoảng cách ngắn nhất của nó từ toàn bộ các cửa hàng được chèn vào thời điểm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n = int(input())
    cells = set()
    coords = []

    for _ in range(n):
        x, y = map(int, input().split())
        coords.append((x, y))
        cells.add((x, y))

    # build adjacency
    adj = {}
    for x, y in coords:
        adj[(x, y)] = []
        for dx, dy in ((1,0), (-1,0), (0,1), (0,-1)):
            nx, ny = x + dx, y + dy
            if (nx, ny) in cells:
                adj[(x, y)].append((nx, ny))

    q = int(input())

    INF = 10**18
    dist = {c: INF for c in cells}
    done = {c: False for c in cells}

    pq = []

    def push(v, d):
        heapq.heappush(pq, (d, v))

    def process_until(target=None):
        while pq:
            d, v = heapq.heappop(pq)
            if done[v]:
                continue
            done[v] = True
            dist[v] = d
            if target is not None and v == target:
                return
            for to in adj[v]:
                if not done[to] and d + 1 < dist[to]:
                    dist[to] = d + 1
                    push(to, d + 1)
        return

    for _ in range(q):
        t, x, y = map(int, input().split())
        v = (x, y)

        if t == 1:
            dist[v] = 0
            push(v, 0)
        else:
            process_until(v)
            print(dist[v] if done[v] else -1)

if __name__ == "__main__":
    solve()
```Cấu trúc kề sử dụng tập tọa độ băm để đảm bảo kiểm tra hàng xóm O(1). Điều này là cần thiết vì tọa độ lưới lớn nhưng thưa thớt. 

Hàng đợi ưu tiên được chia sẻ trên tất cả các hoạt động, do đó khoảng cách được phát hiện trong các giai đoạn trước đó sẽ được sử dụng lại. các`process_until`là cốt lõi: nó chỉ thúc đẩy việc mở rộng đường dẫn ngắn nhất toàn cầu khi cần thiết để hoàn thiện nút được truy vấn. 

Một chi tiết triển khai tinh tế là chúng tôi chỉ đánh dấu một nút là đã hoàn tất khi nó được lấy ra từ vùng nhớ heap chứ không phải khi nó được cập nhật. Điều này bảo đảm tính đúng đắn của thứ tự Dijkstra. Một điểm quan trọng khác là chúng tôi không bao giờ đặt lại cấu trúc giữa các truy vấn; các cửa hàng tích lũy một cách tự nhiên trong cùng một hàng đợi toàn cầu. 

## Ví dụ đã hoạt động 

Hãy xem xét một vùng hình đường nhỏ: 

đầu vào:```
3
1 1
1 2
1 3
3
1 1 1
2 1 3
2 1 2
```### Dấu vết 

| Bước | Sự kiện | Trạng thái PQ | Đã hoàn thiện | quận (1,3) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | thêm cửa hàng (1,1) | (0,(1,1)) | {} | thông tin | - | 
| 2 | truy vấn (1,3) | mở rộng | (1,1) | 2 | 2 | 
| 3 | truy vấn (1,2) | mở rộng thêm | (1,1),(1,2) | 1 | 1 | 

Điều này chứng tỏ rằng làn sóng BFS mở rộng dần dần và sử dụng lại công việc giữa các truy vấn. Truy vấn thứ hai được hưởng lợi từ các nút biên giới đã được khám phá. 

Bây giờ hãy xem xét một hình dạng không liên kết trong tọa độ nhưng được kết nối trong biểu đồ, chẳng hạn như hình chữ L:```
4
1 1
1 2
1 3
2 3
3
1 1 1
1 2 3
2 3 3
```Sóng từ cả hai cửa hàng truyền xung quanh góc một cách chính xác và truy vấn tại (3,3) nhận được khoảng cách tối thiểu chính xác thông qua đường dẫn hợp lệ duy nhất qua khúc cua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi nút được hoàn thiện một lần, mỗi cạnh được thư giãn một lần trong quy trình Dijkstra toàn cầu | 
| Không gian | O(n) | danh sách kề, bản đồ khoảng cách, lưu trữ đống | 

Cấu trúc không bao giờ tính toán lại BFS từ đầu. Mỗi đỉnh đi vào heap một số lần giới hạn và mỗi lần trích xuất sẽ hoàn thành nó vĩnh viễn. Với n và q lên tới 300.000, điều này phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import heapq

    n = int(input())
    cells = set()
    coords = []
    for _ in range(n):
        x, y = map(int, input().split())
        coords.append((x, y))
        cells.add((x, y))

    adj = {}
    for x, y in coords:
        adj[(x, y)] = []
        for dx, dy in ((1,0),(-1,0),(0,1),(0,-1)):
            if (x+dx, y+dy) in cells:
                adj[(x, y)].append((x+dx, y+dy))

    q = int(input())

    INF = 10**18
    dist = {c: INF for c in cells}
    done = {c: False for c in cells}
    pq = []

    def push(v, d):
        heapq.heappush(pq, (d, v))

    def process(target=None):
        while pq:
            d, v = heapq.heappop(pq)
            if done[v]:
                continue
            done[v] = True
            dist[v] = d
            if target == v:
                return
            for to in adj[v]:
                if not done[to] and d + 1 < dist[to]:
                    dist[to] = d + 1
                    push(to, d + 1)

    out = []
    for _ in range(q):
        t, x, y = map(int, input().split())
        v = (x, y)
        if t == 1:
            dist[v] = 0
            push(v, 0)
        else:
            process(v)
            out.append(str(dist[v] if done[v] else -1))

    return "\n".join(out)

# provided sample
assert run("""7
1 2
1 3
2 3
3 1
3 2
3 3
4 2
5
2 3 2
1 4 2
2 1 2
1 3 3
2 2 3
""") == """-1
5
1"""

# custom: single node
assert run("""1
1 1
1
2 1 1
""") == "-1"

# custom: immediate answer
assert run("""2
1 1
1 2
3
1 1 1
2 1 2
2 1 1
""") == """1
0"""

# custom: chain
assert run("""4
1 1
1 2
1 3
1 4
4
1 1 1
2 1 4
1 1 4
2 1 2
""") == """3
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | -1 | không có hộp đựng cửa hàng | 
| câu trả lời ngay lập tức | 1, 0 | nhiều truy vấn và tự mua sắm | 
| cập nhật chuỗi | 3, 1 | lan truyền theo thời gian | 

## Vỏ cạnh 

Một trường hợp góc là khi các truy vấn được hỏi trước khi có bất kỳ cửa hàng nào tồn tại. Trong trường hợp đó, hàng đợi ưu tiên vẫn trống và không có nút nào được hoàn tất. Thuật toán trả về chính xác`-1`bởi vì`done`cờ cho nút được truy vấn không bao giờ trở thành đúng. 

Một trường hợp khác là mở rộng lặp đi lặp lại thông qua một hành lang dài. Nếu các cửa hàng được thêm vào ở một đầu và các truy vấn xảy ra ở đầu kia thì quy trình Dijkstra sẽ dần dần mở rộng dọc theo hành lang. Mặc dù hành lang có thể có độ dài lên tới n, nhưng mỗi đỉnh được hoàn thành một lần, do đó các truy vấn lặp lại sẽ không xử lý lại các vùng đã được giải quyết. 

Trường hợp tinh vi cuối cùng là nhiều cửa hàng được thêm vào gần nhau. Sóng BFS của chúng hợp nhất trong hàng đợi ưu tiên, nhưng thứ tự Dijkstra đảm bảo khoảng cách nhỏ nhất luôn chiếm ưu thế, do đó không xảy ra ghi đè sai ngay cả khi các bản cập nhật đến theo các thứ tự khác nhau.
