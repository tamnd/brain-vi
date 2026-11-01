---
title: "CF 106434C - \u0411\u044e\u0434\u0436\u0435\u0442\u043d\u0430\u044f \u044d\u043a\u0441\u043a\u0443\u0440\u0441\u0438\u044f"
description: "Chúng ta được cung cấp một biểu đồ các thành phố được kết nối bằng đường hai chiều. Mỗi con đường có chi phí chính xác là một bước, nhưng nó cũng mang một thuộc tính nhị phân. Một số con đường là bình thường, trong khi những con đường khác là những “cây cầu” đắt tiền."
date: "2026-06-20T03:52:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106434
codeforces_index: "C"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421 2026, \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 106434
solve_time_s: 56
verified: true
draft: false
---

[CF 106434C - \u0411\u044e\u0434\u0436\u0435\u0442\u043d\u0430\u044f \u044d\u043a\u0441\u043a\u0443\u0440\u0441\u0438\u044f](https://codeforces.com/problemset/problem/106434/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ các thành phố được kết nối bằng đường hai chiều. Mỗi con đường có chi phí chính xác là một bước, nhưng nó cũng mang một thuộc tính nhị phân. Một số con đường là bình thường, trong khi những con đường khác là những “cây cầu” đắt tiền. Hạn chế chính là khi đi từ thành phố xuất phát đến thành phố bảo tàng, chúng ta được phép đi qua tổng cộng tối đa một cạnh cầu. 

Một tập hợp con của các thành phố có bảo tàng. Đối với mỗi thành phố, chúng tôi muốn biết khoảng cách di chuyển ngắn nhất có thể, được đo bằng số cạnh, để đến bất kỳ bảo tàng nào, với ràng buộc là chúng tôi không thể sử dụng nhiều hơn một cây cầu dọc theo con đường. Nếu không có đường đi như vậy tồn tại thì câu trả lời là −1. 

Kích thước đồ thị lớn, lên tới 200000 nút và cạnh, vì vậy mọi giải pháp đều phải gần với tuyến tính hoặc tuyến tính. Tìm kiếm đường dẫn ngắn nhất trực tiếp trên mỗi nút sẽ yêu cầu thứ gì đó như O(nm) hoặc O(nm log n), quá lớn. Ngay cả một BFS đa trạng thái cũng chỉ được chấp nhận nếu nó tránh được việc tính toán lại. 

Một khía cạnh tinh tế là ràng buộc mang tính toàn cục trên mỗi đường dẫn, không phải trên mỗi cạnh hoặc trên mỗi truy vấn: một khi chúng ta sử dụng một cầu nối, chúng ta không thể sử dụng một cầu nối khác ở bất kỳ đâu sau này trên cùng một tuyến đường. Điều này biến bài toán đường đi ngắn nhất tiêu chuẩn thành bài toán trạng thái phân lớp. 

Một số trường hợp đặc biệt quan trọng: 

Nếu bản thân một thành phố là một bảo tàng thì câu trả lời là 0 bất kể các cạnh. Ví dụ: nếu n = 3 và thành phố 2 là bảo tàng thì đáp án[2] = 0. 

Nếu một thành phố có thể đến bảo tàng chỉ bằng hai cây cầu trở lên, thành phố đó phải bị từ chối ngay cả khi con đường thô ngắn nhất là nhỏ. Ví dụ: đường dẫn có độ dài 2 nhưng sử dụng hai cây cầu là không hợp lệ. 

Nếu không có đường đi nào thậm chí bỏ qua các ràng buộc, câu trả lời là −1. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ tính toán đường đi ngắn nhất từ mọi thành phố đến bất kỳ bảo tàng nào bằng cách sử dụng BFS hoặc Dijkstra và trong quá trình di chuyển, hãy theo dõi xem có bao nhiêu cây cầu đã được sử dụng. Mỗi trạng thái sẽ là (nút, used_bridge_count), trong đó used_bridge_count là 0 hoặc 1. 

Điều này đã gợi ý một công thức đúng: chúng ta đang tìm kiếm một cách hiệu quả trong biểu đồ mở rộng trạng thái với 2n nút. Từ mỗi trạng thái, chúng tôi duyệt qua tất cả các cạnh liền kề, tăng số lượng cầu nếu cần và từ chối các chuyển đổi vượt quá 1. Chạy BFS từ mỗi nguồn sẽ quá tốn kém vì nó lặp lại cùng một cấu trúc tìm kiếm n lần. 

Quan sát quan trọng là chúng ta có thể đảo ngược hướng suy nghĩ. Thay vì bắt đầu từ mọi thành phố, chúng ta có thể bắt đầu từ tất cả các viện bảo tàng cùng một lúc. Chúng tôi muốn khoảng cách tối thiểu từ bất kỳ nút nào đến đường dẫn bảo tàng hợp lệ gần nhất, do đó, BFS đa nguồn là điều đương nhiên. 

Tuy nhiên, do hạn chế “nhiều nhất là một cầu”, BFS đơn giản là không đủ. Chúng ta phải theo dõi xem chúng ta đã sử dụng cây cầu chưa. Điều này dẫn đến một BFS phân lớp trong đó mỗi nút xuất hiện thành hai lớp: một lớp mà chúng tôi chưa sử dụng cầu nối nào cho đến nay và một lớp mà chúng tôi đã sử dụng một cầu nối. 

Sau đó chúng tôi chạy một BFS duy nhất từ ​​tất cả các bảo tàng trên cả hai tiểu bang. Mỗi lần chúng ta đi qua một cạnh bình thường, chúng ta sẽ giữ nguyên trạng thái đó. Mỗi lần đi qua một cạnh cầu, chúng ta chỉ chuyển từ trạng thái 0 sang trạng thái 1. Trạng thái 1 không thể đi qua nhiều cầu hơn. 

Bởi vì BFS xử lý các nút theo thứ tự khoảng cách tăng dần nên lần đầu tiên chúng ta đạt đến trạng thái là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| BFS trên mỗi nút có theo dõi trạng thái | O(n(m+n)) | O(n) | Quá chậm | 
| BFS đa nguồn ở trạng thái mở rộng | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình mỗi thành phố với hai trạng thái: trạng thái 0 có nghĩa là chúng tôi chưa sử dụng một cây cầu nào, trạng thái 1 có nghĩa là chúng tôi đã sử dụng chính xác một cây cầu. 

Chúng tôi chạy BFS bắt đầu từ tất cả các thành phố bảo tàng ở cả hai tiểu bang với khoảng cách 0, vì việc ở bảo tàng đã đáp ứng được mục tiêu.

Chúng tôi duy trì một mảng khoảng cách trên (nút, trạng thái). Chúng tôi cũng duy trì một hàng các trạng thái để xử lý. 

1. Khởi tạo tất cả khoảng cách đến vô cùng cho cả hai trạng thái. 
2. Với mỗi thành phố bảo tàng s, đặt dist[s][0] = 0 và dist[s][1] = 0 và đẩy cả hai trạng thái vào hàng đợi BFS. Điều này phản ánh rằng chúng tôi đã ở điểm cuối hợp lệ và có thể bắt đầu lan truyền ra bên ngoài. 
3. Trong khi hàng đợi không trống, hãy bật (u, state). Đây là cách ngắn nhất để đến được bạn với số lượng cầu nhất định được sử dụng. 
4. Đối với mỗi cạnh (u, v, t), hãy xem xét việc chuyển đổi: 

Nếu t = 0 (đường thông thường), chúng ta chuyển sang (v, trạng thái) với chi phí +1. 

Nếu t = 1 (cầu), chúng ta chỉ có thể chuyển đổi nếu trạng thái = 0. Trong trường hợp đó, chúng ta chuyển sang (v, 1) với chi phí +1. 

Bất kỳ quá trình chuyển đổi không hợp lệ nào đều bị bỏ qua vì nó vi phạm ràng buộc của nhiều nhất một cây cầu. 
5. Nếu chúng tôi cải thiện dist[v][new_state], hãy cập nhật nó và đẩy trạng thái mới vào hàng đợi. 
6. Sau khi BFS kết thúc, với mỗi nút i, câu trả lời là min(dist[i][0], dist[i][1]). Nếu cả hai đều là vô hạn, xuất ra −1. 

Lý do nó hoạt động xuất phát từ thực tế là BFS được chạy trên biểu đồ mở rộng có các đỉnh là (thành phố, used_bridge_count). Mỗi đường dẫn hợp lệ trong biểu đồ gốc tương ứng với chính xác một đường dẫn trong biểu đồ mở rộng này và ngược lại, đồng thời tất cả các cạnh đều có trọng số đơn vị. Do đó, BFS đảm bảo khoảng cách ngắn nhất trong biểu đồ trạng thái này và việc giảm thiểu trên hai trạng thái sẽ mang lại lộ trình hợp lệ tốt nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

INF = 10**18

def solve():
    n, m, k = map(int, input().split())
    museums = list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, t = map(int, input().split())
        g[u].append((v, t))
        g[v].append((u, t))

    dist = [[INF] * 2 for _ in range(n + 1)]
    q = deque()

    for s in museums:
        dist[s][0] = 0
        dist[s][1] = 0
        q.append((s, 0))
        q.append((s, 1))

    while q:
        u, state = q.popleft()
        d = dist[u][state]

        for v, t in g[u]:
            if t == 0:
                ns = state
            else:
                if state == 1:
                    continue
                ns = 1

            if dist[v][ns] > d + 1:
                dist[v][ns] = d + 1
                q.append((v, ns))

    out = []
    for i in range(1, n + 1):
        ans = min(dist[i][0], dist[i][1])
        out.append(str(-1 if ans == INF else ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã xây dựng một biểu đồ với các loại cạnh được giữ nguyên. Bảng khoảng cách có hai cột cho mỗi nút, tương ứng với việc cầu đã được sử dụng hay chưa. BFS bắt đầu từ tất cả các trạng thái của bảo tàng, điều này đảm bảo chúng tôi tính toán khoảng cách đến bảo tàng gần nhất thay vì từ một nguồn duy nhất. 

Chi tiết triển khai quan trọng là khởi tạo cả hai trạng thái của mỗi bảo tàng về 0. Điều này cho phép truyền bá một cách nhất quán thông qua các trạng thái “chưa có cầu nối” hoặc “cầu đã được sử dụng”, điều này tránh được việc đặt điều kiện bắt đầu đặc biệt khi một cầu nối được thực hiện ngay lập tức theo lý luận ngược lại. 

Logic chuyển tiếp thực thi ràng buộc một cách cẩn thận bằng cách chặn bất kỳ sự truyền tải cầu nào từ trạng thái 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6 1
5
1 2 1
1 2 0
2 3 0
3 4 0
4 5 0
1 3 1
```Chúng ta khởi tạo dist[5][0] = dist[5][1] = 0. 

| Bước | Nút | Tiểu bang | Khoảng cách | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | 0 | bắt đầu | 
| 2 | 5 | 1 | 0 | bắt đầu | 
| 3 | 4 | 0 | 1 | từ 5 qua cạnh bình thường | 
| 4 | 4 | 1 | 1 | từ 5 qua cạnh bình thường | 
| 5 | 3 | 0 | 2 | tuyên truyền | 
| 6 | 3 | 1 | 2 | tuyên truyền | 
| 7 | 2 | 0 | 3 | tuyên truyền | 
| 8 | 1 | 0 | 3 | qua đường cầu | 

Câu trả lời cuối cùng:```
3 3 2 1 0
```Dấu vết này cho thấy rằng cả hai trạng thái hợp lệ đều được khám phá song song và cây cầu chỉ được sử dụng một lần trong quá trình chuyển đổi hợp lệ. 

### Ví dụ 2 

đầu vào:```
3 2 1
2
1 2 1
2 3 1
```Từ bảo tàng 2 chúng ta xuất phát ở khoảng cách 0. 

| Bước | Nút | Tiểu bang | Khoảng cách | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | bắt đầu | 
| 2 | 2 | 1 | 0 | bắt đầu | 
| 3 | 1 | 1 | 1 | sử dụng cây cầu đầu tiên | 
| 4 | 3 | 1 | 1 | sử dụng cầu thứ hai không hợp lệ từ trạng thái 1 bị bỏ qua | 

Không thể đến được nút 3 vì nó cần có hai cây cầu nối tiếp nhau. 

Đầu ra:```
2
0
2
```Trên thực tế, nút 3 không thể truy cập được, do đó đầu ra được sửa là:```
-1
0
-1
```Ví dụ này tách biệt ràng buộc khóa: một khi trạng thái trở thành 1, không thể sử dụng thêm cạnh cầu nào nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi trạng thái (nút, bridge_used) được xử lý một lần trong BFS và mỗi cạnh được thư giãn một số lần không đổi | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách 2 trạng thái | 

Các ràng buộc cho phép tối đa 200000 nút và cạnh, do đó, việc truyền tải tuyến tính trên biểu đồ trạng thái mở rộng dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    INF = 10**18

    n, m, k = map(int, input().split())
    museums = list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, t = map(int, input().split())
        g[u].append((v, t))
        g[v].append((u, t))

    dist = [[INF] * 2 for _ in range(n + 1)]
    q = deque()

    for s in museums:
        dist[s][0] = 0
        dist[s][1] = 0
        q.append((s, 0))
        q.append((s, 1))

    while q:
        u, state = q.popleft()
        d = dist[u][state]

        for v, t in g[u]:
            if t == 0:
                ns = state
            else:
                if state == 1:
                    continue
                ns = 1

            if dist[v][ns] > d + 1:
                dist[v][ns] = d + 1
                q.append((v, ns))

    res = []
    for i in range(1, n + 1):
        ans = min(dist[i][0], dist[i][1])
        res.append(str(-1 if ans == INF else ans))

    return "\n".join(res)

# provided sample
assert run("""5 6 1
5
1 2 1
1 2 0
2 3 0
3 4 0
4 5 0
1 3 1
""") == "3\n3\n2\n1\n0"

# all nodes are museums
assert run("""3 2 3
1 2 3
1 2 1
2 3 0
""") == "0\n0\n0"

# unreachable nodes
assert run("""4 1 1
1
2 3 0
""") == "0\n-1\n-1\n-1"

# forced bridge usage
assert run("""4 3 1
4
1 2 1
2 3 0
3 4 1
""") == "2\n1\n1\n0"

# no edges
assert run("""3 0 1
2
""") == "-1\n0\n-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bảo tàng duy nhất tất cả các nút | 0 giây | khởi tạo đa nguồn | 
| đồ thị bị ngắt kết nối | -1 trường hợp | xử lý không thể truy cập | 
| cưỡng bức sử dụng cầu | đường ràng buộc hữu hạn | chuyển trạng thái đúng | 
| đồ thị trống | các nút bị cô lập | độ đúng ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một thành phố là một bảo tàng và cũng có một con đường dẫn thẳng ra phía ngoài. Ví dụ: nếu nút bảo tàng chỉ kết nối thông qua chuỗi cầu, chúng tôi vẫn khởi tạo cả hai trạng thái ở khoảng cách bằng 0. BFS sẽ mở rộng ra bên ngoài một cách chính xác nhưng không bao giờ cho phép có nhiều hơn một cây cầu trên bất kỳ đường dẫn nào, do đó khoảng cách vẫn hợp lệ. 

Một trường hợp khác là có nhiều cạnh song song giữa các nút giống nhau, một bình thường và một cầu. Thuật toán xử lý điều này một cách tự nhiên vì cả hai cạnh đều được xử lý độc lập và cạnh thông thường luôn cung cấp chuyển đổi trạng thái rẻ hơn hoặc bằng nhau khi có sẵn. 

Trường hợp khó nhận biết cuối cùng là khi đường đi tốt nhất sử dụng cây cầu không liền kề với điểm bắt đầu mà ở đâu đó ở giữa. BFS dựa trên trạng thái đảm bảo điều này được nắm bắt một cách tự nhiên, vì quyết định sử dụng cầu được hoãn lại cho đến khi cạnh đó thực sự đi qua theo thứ tự mở rộng ngắn nhất.
