---
title: "CF 105454C - \u041f\u043e\u0447\u0442\u0430\u043b\u044c\u043e\u043d \u041f\u0435\u0447\u043a\u0438\u043d \u0432 \u0431\u043e\u043b\u044c\u0448\u043e\u043c \u0433\u043e\u0440\u043e\u0434\u0435"
description: "Thành phố là một đồ thị kết nối vô hướng trong đó mỗi ngôi nhà là một đỉnh và mỗi con đường là một cạnh không có trọng số. Một trung tâm bưu chính phải được đặt ở đúng một đỉnh."
date: "2026-06-23T02:53:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105454
codeforces_index: "C"
codeforces_contest_name: "\u041f\u0435\u0440\u043c\u0441\u043a\u0430\u044f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105454
solve_time_s: 115
verified: false
draft: false
---

[CF 105454C - \u041f\u043e\u0447\u0442\u0430\u043b\u044c\u043e\u043d \u041f\u0435\u0447\u043a\u0438\u043d \u0432 \u0431\u043e\u043b\u044c\u0448\u043e\u043c \u0433\u043e\u0440\u043e\u0434\u0435](https://codeforces.com/problemset/problem/105454/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Thành phố là một đồ thị kết nối vô hướng trong đó mỗi ngôi nhà là một đỉnh và mỗi con đường là một cạnh không có trọng số. Một trung tâm bưu chính phải được đặt ở đúng một đỉnh. Sau đó, có một số chuyến giao hàng theo kế hoạch và mỗi lần giao hàng bao gồm việc chuyển một gói hàng từ nhà nguồn đến nhà đích. Đối với mỗi lần giao hàng, trước tiên người chuyển phát nhanh sẽ đi bộ từ nguồn đến trung tâm, sau đó từ trung tâm đến đích, trả một đơn vị chi phí cho mỗi con đường đi qua. 

Tổng chi phí cho một trung tâm cố định là tổng của các chi phí này trên tất cả các lần giao hàng. Nhiệm vụ là chọn vị trí trung tâm để giảm thiểu tổng chi phí này. 

Kích thước đầu vào cho thấy rằng biểu đồ không có số lượng nút lớn nhưng có thể có số lượng cạnh dày đặc trong trường hợp xấu nhất. Ràng buộc sản phẩm trên n và m ngăn các giá trị lớn đồng thời, thường báo hiệu rằng giải pháp O(nm) hoặc O(n(n+m)) có thể chấp nhận được trong không gian giải pháp dự định, trong khi mọi thứ bậc hai tính bằng k hoặc bậc ba tính bằng n sẽ nằm ngoài phạm vi. 

Một ý tưởng ngây thơ là thử mọi trung tâm có thể và tính toán tổng chi phí của nó bằng cách thực hiện tìm kiếm đường đi ngắn nhất từ ​​trung tâm đó hoặc tính toán lại khoảng cách nhiều lần. Điều đó ngay lập tức trở nên tốn kém vì nó sẽ yêu cầu tính toán lại các đường đi ngắn nhất cho mọi trung tâm ứng viên, dẫn đến việc duyệt biểu đồ lặp đi lặp lại. 

Một trường hợp phức tạp xuất hiện khi không có đơn hàng nào được giao. Trong tình huống đó, mọi nút đều mang lại chi phí bằng 0, vì vậy mọi đỉnh đều hợp lệ. Một giải pháp đúng không được cho rằng có ít nhất một lần phân phối tồn tại. 

Một trường hợp góc khác phát sinh khi nhiều trung tâm liên kết với nhau để đạt được chi phí tối thiểu. Vấn đề cho phép trả lại bất kỳ giá trị nào trong số chúng, vì vậy giải pháp không được cố gắng thực thi việc phá vỡ ràng buộc xác định ngoài tính đúng đắn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đánh giá từng trung tâm một cách độc lập. Đối với một hub x cố định, chúng tôi tính toán khoảng cách đường đi ngắn nhất từ ​​x đến tất cả các nút và sau đó với mỗi lần phân phối (c, d), chúng tôi thêm dist(c, x) + dist(x, d). Vì biểu đồ không có trọng số nên mỗi phép tính đường đi ngắn nhất có thể được thực hiện bằng BFS. Lặp lại BFS cho mỗi trung tâm ứng cử viên có nghĩa là chạy n lần duyệt BFS, mỗi lần tốn O(n + m). Điều này dẫn đến các phép toán O(n(n + m)), trong trường hợp xấu nhất đạt đến giới hạn trên của công việc được phép ngụ ý bởi ràng buộc n · m ≤ 10^8. Về mặt khái niệm thì đúng nhưng lãng phí cấu trúc lặp đi lặp lại: mọi BFS đều tính toán lại thông tin khoảng cách giống nhau. 

Quan sát quan trọng là biểu thức chi phí được phân tách rõ ràng giữa các điểm cuối. Đối với một trung tâm cố định x, tổng chi phí sẽ trở thành tổng của tất cả các điểm cuối xuất hiện trong yêu cầu. Mỗi lần phân phối đóng góp dist(x, c) và dist(x, d), vì vậy chúng ta có thể xác định trọng số w[u] bằng số lần nút u xuất hiện dưới dạng nguồn hoặc đích. Tổng chi phí cho hub x trở thành tổng của tất cả các nút u của w[u] · dist(x, u). Điều này loại bỏ hoàn toàn sự phụ thuộc vào các cặp và biến vấn đề thành việc đánh giá tổng khoảng cách có trọng số từ mỗi nghiệm ứng viên. 

Bây giờ, nhiệm vụ trở thành tính toán khoảng cách đường đi ngắn nhất của tất cả các cặp trong biểu đồ không có trọng số hoặc ít nhất là cấu trúc đủ để đánh giá các tổng có trọng số này. Vì khoảng cách được yêu cầu giữa mỗi cặp nút trong trường hợp xấu nhất nên việc chạy BFS từ mỗi nút là phương pháp đơn giản nhất. Sau khi đã biết tất cả các khoảng cách, việc tính tổng có trọng số cho mỗi trung tâm ứng cử viên sẽ trở thành phép tổng hợp trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại BFS trên mỗi trung tâm | O(n(n + m)) | O(n) | Quá chậm trong trường hợp xấu nhất | 
| Tổng hợp BFS + tất cả các cặp | O(n(n + m)) | O(n^2) | Được chấp nhận dưới những ràng buộc | 

## Hướng dẫn thuật toán

1. Đọc biểu đồ và xây dựng danh sách kề. Biểu đồ không có trọng số, vì vậy BFS là cách tự nhiên để tính toán đường đi ngắn nhất. 
2. Xây dựng một mảng w trong đó w[u] đếm số lần nút u xuất hiện trên tất cả các điểm cuối phân phối. Mỗi lần phân phối đóng góp +1 cho nguồn và +1 cho đích. Công thức cải tiến này thay thế các đóng góp theo cặp bằng trọng số trên mỗi nút. 
3. Đối với mọi nút s từ 1 đến n, hãy chạy BFS để tính khoảng cách [s] [*], khoảng cách ngắn nhất từ ​​s đến tất cả các nút khác. Bước này là cần thiết vì chi phí cuối cùng phụ thuộc vào khoảng cách từ mọi trung tâm có thể đến tất cả các nút có trọng số. 
4. Sau khi tính toán khoảng cách từ một nguồn cố định s, hãy tính chi phí của nó bằng cách tính tổng w[u] · dist[s][u] trên tất cả các nút u. Điều này thể hiện tổng chi phí nếu trung tâm được đặt ở mức s. 
5. Theo dõi nút có chi phí tính toán nhỏ nhất. Nếu nhiều nút đạt được mức tối thiểu như nhau thì bất kỳ nút nào cũng có thể được lưu trữ. 
6. Xuất ra nút tốt nhất. 

Cấu trúc lớp BFS đảm bảo tính chính xác của khoảng cách ngắn nhất vì mỗi cạnh có trọng lượng đơn vị, do đó lần đầu tiên một nút được truy cập trong BFS tương ứng với khoảng cách tối thiểu của nó. 

### Tại sao nó hoạt động 

Việc chuyển đổi từ đóng góp cặp sang trọng số nút sẽ bảo toàn tổng chi phí vì mỗi lần phân phối đóng góp chính xác hai thuật ngữ đường đi ngắn nhất độc lập. Vì khoảng cách đường đi ngắn nhất thỏa mãn tính đối xứng trong đồ thị vô hướng và tính tuyến tính của phép tính tổng, việc tập hợp lại các số hạng theo điểm cuối không làm thay đổi tổng giá trị. Sau khi quá trình phân tách này được thực hiện, mọi trung tâm ứng cử viên sẽ được đánh giá theo cùng một hàm khoảng cách có trọng số cố định, do đó việc chọn mức tối thiểu trên tất cả các nút tương đương với việc giải quyết vấn đề tối ưu hóa ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def bfs(start, adj, n):
    dist = [-1] * (n + 1)
    q = deque([start])
    dist[start] = 0

    while q:
        v = q.popleft()
        for to in adj[v]:
            if dist[to] == -1:
                dist[to] = dist[v] + 1
                q.append(to)
    return dist

def main():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)

    k = int(input())
    w = [0] * (n + 1)

    for _ in range(k):
        c, d = map(int, input().split())
        w[c] += 1
        w[d] += 1

    # edge case: no deliveries
    if k == 0:
        print(1)
        return

    best_node = 1
    best_cost = float('inf')

    for s in range(1, n + 1):
        dist = bfs(s, adj, n)
        cost = 0
        for i in range(1, n + 1):
            cost += w[i] * dist[i]
        if cost < best_cost:
            best_cost = cost
            best_node = s

    print(best_node)

if __name__ == "__main__":
    main()
```Hàm BFS tính toán các đường dẫn ngắn nhất một nguồn bằng cách sử dụng hàng đợi. Vòng lặp bên ngoài đánh giá mỗi nút là một trung tâm ứng cử viên. Đối với mỗi ứng cử viên, chúng tôi sử dụng lại mảng khoảng cách giống nhau để tính tổng có trọng số theo thời gian tuyến tính. 

Chi tiết triển khai tinh tế là khởi tạo khoảng cách bằng -1. Điều này tránh việc xem lại các nút và phân biệt rõ ràng các trạng thái không thể truy cập, mặc dù biểu đồ được đảm bảo kết nối. Một điểm quan trọng khác là tách tích lũy trọng lượng khỏi tính toán BFS; trộn chúng sẽ dẫn đến các phép tính tổng dư thừa lặp đi lặp lại bên trong BFS và tăng đáng kể thời gian chạy. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ nơi bạn có thể dễ dàng theo dõi khoảng cách. Giả sử chúng ta đánh giá mỗi nút là một trung tâm tiềm năng và tính tổng có trọng số dựa trên tần số điểm cuối. 

Đối với một dấu vết đơn giản, giả sử các nút 1, 2, 3 tạo thành một dòng 1-2-3 và có các lần phân phối (1,3) và (3,1). Khi đó w[1] = 2, w[3] = 2, w[2] = 0. 

Đối với mỗi trung tâm ứng viên: 

| Trung tâm | dist tới 1 | dist tới 2 | quận tới 3 | Tính toán chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 2 | 2·0 + 0·1 + 2·2 = 4 | 
| 2 | 1 | 0 | 1 | 2·1 + 0·0 + 2·1 = 4 | 
| 3 | 2 | 1 | 0 | 2·2 + 0·1 + 2·0 = 4 | 

Tất cả các nút đều ràng buộc, vì vậy bất kỳ nút nào cũng hợp lệ. Điều này cho thấy tính đối xứng ở các điểm cuối dẫn đến sự phân bổ chi phí một cách đồng đều. 

Bây giờ hãy xem xét trường hợp sai lệch trong đó hầu hết các lần phân phối đều có chung một điểm cuối, giả sử nhiều cặp bắt đầu từ nút 1 và đi đến các nút khác nhau. Sau đó, w[1] trở nên lớn và trung tâm tối ưu sẽ dịch chuyển về phía trung tâm của các đường đi ngắn nhất từ ​​nút 1 đến các nút khác, bởi vì việc giảm thiểu khoảng cách có trọng số sẽ kéo trung tâm về phía các nút tần số cao một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n(n + m)) | BFS được chạy từ mỗi nút, mỗi BFS xử lý tất cả các cạnh một lần | 
| Không gian | O(n^2) | Ma trận khoảng cách được lưu trữ ngầm hoặc tính toán lại trên mỗi nguồn | 

Ràng buộc n · m ≤ 10^8 đảm bảo rằng việc chạy n truyền tải BFS là khả thi vì tích trong trường hợp xấu nhất vẫn bị chặn. Bộ nhớ có thể chấp nhận được vì n tối đa là 10^4 và việc lưu trữ khoảng cách hoặc tính toán lại cho mỗi nguồn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    def bfs(start, adj, n):
        dist = [-1] * (n + 1)
        q = deque([start])
        dist[start] = 0
        while q:
            v = q.popleft()
            for to in adj[v]:
                if dist[to] == -1:
                    dist[to] = dist[v] + 1
                    q.append(to)
        return dist

    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)

    k = int(input())
    w = [0] * (n + 1)
    for _ in range(k):
        c, d = map(int, input().split())
        w[c] += 1
        w[d] += 1

    if k == 0:
        return "1\n"

    best_node = 1
    best_cost = float('inf')

    for s in range(1, n + 1):
        dist = bfs(s, adj, n)
        cost = 0
        for i in range(1, n + 1):
            cost += w[i] * dist[i]
        if cost < best_cost:
            best_cost = cost
            best_node = s

    return str(best_node) + "\n"

# provided sample
assert run("""3 2
1 2
2 3
1
1 3
""") == "1\n"

# single node graph
assert run("""1 0
0
""") == "1\n"

# star graph
assert run("""5 4
1 2
1 3
1 4
1 5
2
2 3
4 5
""") in {"1\n", "2\n", "3\n", "4\n", "5\n"}

# chain skewed weights
assert run("""4 3
1 2
2 3
3 4
3
1 4
1 3
1 2
""") == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút đơn | 1 | Xử lý cấu trúc tối thiểu | 
| Đồ thị sao | Nút bất kỳ | Nhiều trung tâm tối ưu | 
| Xích lệch | 2 | Hành vi trung tâm có trọng số | 

## Vỏ cạnh 

Khi không có lần phân phối nào, thuật toán sẽ trực tiếp trả về một nút hợp lệ mà không thực hiện BFS. Điều này ngăn chặn việc tính toán không cần thiết và tránh hành vi không xác định khi mảng chi phí vẫn chưa được sử dụng. 

Đối với các biểu đồ trong đó tất cả các điểm cuối phân phối tập trung vào một nút, chẳng hạn như nút 1, mảng trọng số sẽ thiên lệch nhiều khoảng cách từ các trung tâm ứng cử viên theo hướng giảm thiểu khoảng cách đến nút 1. Đánh giá chi phí dựa trên BFS phản ánh chính xác điều này vì tất cả các khoản đóng góp chi phí đều giảm xuống w[1] · dist(x, 1), do đó, nút tối ưu sẽ trở thành một khoảng cách tối thiểu hóa đến nút 1. 

Trong các biểu đồ đối xứng hoàn toàn như biểu đồ chu trình hoặc biểu đồ hoàn chỉnh, tất cả các nút tạo ra tổng có trọng số giống hệt nhau dưới sự phân bổ điểm cuối thống nhất. Thuật toán duy trì điều này một cách tự nhiên bằng cách đánh giá các giá trị chi phí giống hệt nhau giữa các ứng viên và chấp nhận mức tối thiểu đầu tiên gặp phải.
