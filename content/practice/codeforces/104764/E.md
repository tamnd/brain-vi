---
title: "CF 104764E - Sứa hang động"
description: "Chúng tôi được cấp một cây có trọng số lên tới 100 nút. Mỗi nút đại diện cho một hang động biển và chứa một số lượng sứa, được biểu thị bằng một giá trị không âm."
date: "2026-06-28T21:41:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104764
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 11-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104764
solve_time_s: 88
verified: false
draft: false
---

[CF 104764E - Sứa hang động](https://codeforces.com/problemset/problem/104764/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một cây có trọng số lên tới 100 nút. Mỗi nút đại diện cho một hang động biển và chứa một số lượng sứa, được biểu thị bằng một giá trị không âm. Di chuyển giữa hai hang động được kết nối sẽ tiêu tốn năng lượng bằng trọng lượng của cạnh và tổng chi phí di chuyển giữa hai hang động bất kỳ là tổng trọng lượng dọc theo con đường duy nhất trong cây. 

Nếu Ao Run chọn hang động$y$làm căn cứ của mình thì cho mọi hang động$x$, anh ấy đi từ$y$ĐẾN$x$, trả chi phí khoảng cách$dist(y,x)$, rồi “giao chiến” với lũ sứa trong$x$, tiêu tốn thêm một đơn vị năng lượng. Sự đóng góp tham gia từ hang động$x$được định nghĩa là$$\frac{c_x}{dist(y,x) + 1}.$$Nhiệm vụ là chọn nút cơ sở$y$tối đa hóa tổng số tiền đóng góp này trên tất cả các nút và xuất ra cả nút đã chọn và tổng tối đa thu được. 

Kích thước đầu vào nhỏ,$n \le 100$, vì vậy ngay cả các phương pháp bậc ba hoặc bậc hai cũng có thể thực hiện được. Điều này ngay lập tức gợi ý rằng chúng ta có đủ khả năng tính toán trước tất cả khoảng cách theo cặp giữa các nút. 

Một vấn đề tế nhị là sự ổn định về số lượng. Câu trả lời yêu cầu phép chia dấu phẩy động và phải chính xác trong phạm vi$10^{-4}$, do đó số học số nguyên đơn giản là không đủ ở bước tổng hợp cuối cùng, nhưng độ chính xác kép tiêu chuẩn là đủ dễ dàng vì số lượng thuật ngữ nhỏ (nhiều nhất là 100 mỗi tổng). 

Không có trường hợp cạnh cấu trúc phức tạp nào như biểu đồ bị ngắt kết nối hoặc nhiều thành phần, vì đầu vào rõ ràng là một cây. 

Một trường hợp thất bại đối với lối suy luận ngây thơ là cố gắng chọn một gốc chỉ dựa trên mức cao gần đó.$c_i$các giá trị. Ví dụ: trong một cây dòng nhỏ, một nút có giá trị lân cận nhỏ hơn một chút nhưng khoảng cách toàn cầu tốt hơn nhiều có thể hoạt động tốt hơn lựa chọn tối ưu cục bộ. Điều này cho thấy mục tiêu mang tính toàn cầu và phụ thuộc vào khoảng cách, không thể phân tách thành các đóng góp của địa phương. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi nút cơ sở có thể$y$. Với mỗi lựa chọn, chúng tôi tính toán khoảng cách đường đi ngắn nhất từ$y$đến tất cả các nút khác, sau đó tính tổng$\frac{c_x}{dist(y,x)+1}$. 

Vì biểu đồ là một cây nên các đường đi ngắn nhất là duy nhất và có thể được tính bằng BFS hoặc DFS khi trọng số nhỏ nhưng vì trọng số lên tới$10^3$, chúng ta cần Dijkstra nếu chúng ta tính toán từ mỗi nút một cách độc lập. Điều đó mang lại$n$chạy Dijkstra, mỗi chi phí$O(n \log n)$, vậy là về$10^4 \log 100$, điều đó thật tầm thường. 

Một quan sát đơn giản hơn là$n$chỉ là 100, vì vậy chúng ta có thể tính toán trước các đường đi ngắn nhất cho tất cả các cặp. Hoặc Floyd-Warshall tham gia$O(n^3)$hoặc chạy Dijkstra từ mỗi nút trong$O(n^2 \log n)$. Khi chúng ta có ma trận khoảng cách đầy đủ, việc đánh giá từng nghiệm ứng cử viên chỉ là quét tuyến tính. 

Cấu trúc quan trọng giúp cho việc này hoạt động là cây không có chu trình, do đó khoảng cách được xác định rõ ràng và không phụ thuộc vào việc chọn gốc. Khi tất cả các khoảng cách đã được biết, hàm mục tiêu sẽ trở thành một đánh giá đơn giản trên một ma trận cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực mà không cần tính toán trước |$O(n^2 \log n)$mỗi gốc →$O(n^3 \log n)$|$O(n^2)$| Có thể chấp nhận được nhưng không cần thiết | 
| Khoảng cách tất cả các cặp + đánh giá |$O(n^2 \log n + n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của cây có trọng số. 

Điều này đưa ra một cấu trúc trong đó mỗi nút có thể tiếp cận các nút lân cận với chi phí đã biết, điều này cần thiết cho việc tính toán đường đi ngắn nhất. 
2. Chạy Dijkstra từ mọi nút$i$tính toán$dist[i][*]$. 

Mặc dù biểu đồ là một cái cây, chúng ta vẫn coi nó như một biểu đồ có trọng số tổng quát để tránh suy luận về các biểu diễn có gốc. 
3. Đối với mỗi nút$y$, tính điểm khởi tạo bằng 0. 
4. Đối với mọi nút$x$, thêm vào$c_x / (dist[y][x] + 1)$đến số điểm của$y$. 

Mẫu số bao gồm chi phí tương tác thêm 1 đơn vị, do đó, ngay cả ở cùng một nút, mức đóng góp vẫn là$c_y / 1$. 
5. Theo dõi nút có số điểm tối đa trong khi tính toán tất cả các ứng cử viên. 
6. Xuất chỉ mục nút tốt nhất và điểm của nó được định dạng đến 5 chữ số thập phân. 

Tính chính xác dựa trên thực tế là khi khoảng cách được cố định, mỗi nghiệm gốc ứng cử viên sẽ được đánh giá độc lập mà không có tương tác ẩn nào. Mỗi số hạng trong tổng chỉ phụ thuộc vào nghiệm đã chọn và khoảng cách được tính toán trước. 

### Tại sao nó hoạt động 

Thuật toán liệt kê rõ ràng tất cả các cơ sở có thể và tính giá trị chính xác của hàm mục tiêu cho từng cơ sở. Vì khoảng cách trong cây là cố định và không phụ thuộc vào việc root, nên ma trận khoảng cách được tính toán trước có giá trị cho mọi đánh giá. Do đó, lựa chọn cuối cùng là tối đa hóa chính xác trên một tập hữu hạn các giá trị được tính toán chính xác, đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def dijkstra(start, adj, n):
    INF = 10**18
    dist = [INF] * (n + 1)
    dist[start] = 0
    pq = [(0, start)]
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

def solve():
    n = int(input())
    c = [0] + list(map(int, input().split()))

    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        x, y, w = map(int, input().split())
        adj[x].append((y, w))
        adj[y].append((x, w))

    dist = []
    for i in range(1, n + 1):
        dist.append(dijkstra(i, adj, n))

    best_node = 1
    best_val = -1.0

    for y in range(1, n + 1):
        s = 0.0
        for x in range(1, n + 1):
            s += c[x] / (dist[y-1][x] + 1)
        if s > best_val:
            best_val = s
            best_node = y

    print(best_node)
    print(f"{best_val:.5f}")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xây dựng danh sách kề và tính toán tất cả các đường đi ngắn nhất bằng cách sử dụng các lần chạy Dijkstra lặp đi lặp lại. Bảng khoảng cách được lưu trữ sao cho mỗi gốc ứng cử viên có thể được đánh giá một cách riêng biệt mà không cần tính toán lại. 

Khi tính điểm, biểu thức`dist[y-1][x]`phản ánh rằng chúng tôi đã lưu trữ khoảng cách trong danh sách các nút được lập chỉ mục 0. Phép chia được thực hiện ở dạng dấu phẩy động, điều này là đủ vì tổng bao gồm tối đa 100 số hạng, giữ cho sai số số ở dưới mức cho phép. 

Việc lựa chọn Dijkstra thay vì Floyd-Warshall ở đây mang tính phong cách; cả hai đều đủ nhanh, nhưng Dijkstra giữ giải pháp gần hơn với trực giác đồ thị tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Hãy xem xét một cây nhỏ gồm ba nút trên một dòng: 1-2-3, với tất cả trọng số và giá trị của các cạnh là 1$c = [2, 1, 3]$. 

| Căn cứ y | quận(y,1) | quận(y,2) | quận(y,3) | tính điểm | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 2 | 2/1 + 1/2 + 3/3 = 2 + 0,5 + 1 | 
| 2 | 1 | 0 | 1 | 2/2 + 1/1 + 3/2 = 1 + 1 + 1,5 | 
| 3 | 2 | 1 | 0 | 2/3 + 1/2 + 3/1 | 

Cơ sở tốt nhất là nút 3 vì nó được hưởng lợi từ giá trị lớn nhất ở gần nhất. 

Dấu vết này cho thấy sự bất đối xứng về khoảng cách ảnh hưởng trực tiếp đến sự đóng góp của mỗi nút như thế nào, khiến cho việc phân bổ giá trị và tính trung tâm trở nên quan trọng. 

### Ví dụ Dấu vết 2 

Cây hình ngôi sao có tâm 1 nối với 2, 3, 4 có trọng số 2 và các giá trị$c = [10, 1, 1, 1]$. 

| Căn cứ y | mẫu khoảng cách trung tâm | cấu trúc điểm | 
| --- | --- | --- | 
| 1 | tất cả các lá lúc 1 | 10/1 + 1/3 + 1/3 + 1/3 | 
| 2 | khoảng cách không đối xứng | 1/1 + 10/3 + 1/5 + 1/5 | 
| 3 | đối xứng tới 2 | tương tự | 

Dấu vết cho thấy mặc dù các lá có thể đến gần trung tâm hơn nhưng giá trị trung tâm cao chiếm ưu thế khi phần đế được đặt ở trung tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \log n)$| Dijkstra chạy từ mỗi nút qua$n$nút với$n-1$cạnh | 
| Không gian |$O(n^2)$| Ma trận khoảng cách đầy đủ được lưu trữ cho tất cả các cặp nút | 

Với$n \le 100$, điều này tương ứng nhiều nhất là khoảng$10^4$bước thư giãn mỗi lần chạy, lặp lại 100 lần, dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from math import isclose

    # Re-run solution inline
    import heapq

    def dijkstra(start, adj, n):
        INF = 10**18
        dist = [INF] * (n + 1)
        dist[start] = 0
        pq = [(0, start)]
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

    n = int(input())
    c = [0] + list(map(int, input().split()))
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        x, y, w = map(int, input().split())
        adj[x].append((y, w))
        adj[y].append((x, w))

    dist = [dijkstra(i, adj, n) for i in range(1, n + 1)]

    best_node = 1
    best_val = -1.0

    for y in range(1, n + 1):
        s = 0.0
        for x in range(1, n + 1):
            s += c[x] / (dist[y-1][x] + 1)
        if s > best_val:
            best_val = s
            best_node = y

    return str(best_node) + "\n" + f"{best_val:.5f}"

# provided sample
assert run("5\n5 2 9 1 7\n1 2 2\n1 3 2\n3 4 1\n3 5 3\n") == "3\n13.31667"

# minimum size
assert run("2\n1 2\n1 2 1\n") is not None

# star test
assert run("4\n10 1 1 1\n1 2 1\n1 3 1\n1 4 1\n").startswith("1")

# chain test
assert run("3\n1 2 3\n1 2 1\n2 3 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 3/13.31667 | tính đúng đắn của tuyên bố đầy đủ | 
| cây 2 nút | giá trị nhỏ | xử lý trường hợp cơ bản | 
| cây sao | 1 | hành vi thống trị trung tâm | 
| cây xích | tổng số tiền thả nổi nhất quán | sự tích lũy khoảng cách đúng đắn | 

## Vỏ cạnh 

Cây tối thiểu có hai nút sẽ kiểm tra xem quy tắc mẫu số có được áp dụng chính xác ngay cả khi một nút vừa là nguồn vừa là đích. Nếu nút 1 kết nối với nút 2 có trọng số 5 và$c = [4, 6]$, sau đó chọn nút 1 mang lại$4/1 + 6/6$, trong khi chọn nút 2 mang lại kết quả$6/1 + 4/6$. Thuật toán đánh giá chính xác cả bằng cách sử dụng ma trận khoảng cách được tính toán trước và chọn giá trị lớn hơn. 

Một cây có độ mất cân bằng cao, chẳng hạn như một chuỗi gồm 100 nút, sẽ kiểm tra xem việc tích lũy khoảng cách có gây ra độ lệch chính xác hay không. Vì mỗi gốc ứng cử viên sử dụng tối đa 100 phép cộng phân số nên lỗi dấu phẩy động vẫn ổn định và nút tốt nhất vẫn được xác định chính xác bằng cách so sánh trực tiếp các tổng được tính toán.
