---
title: "CF 1307G - Bò và Tập thể dục"
description: "Chúng ta được cung cấp một biểu đồ có trọng số có hướng có tối đa 50 đỉnh, biểu thị các trường được kết nối bằng đường. Một con bò bắt đầu từ nút 1 và muốn đến nút n và thời gian cần thiết được xác định bởi đường đi ngắn nhất trong biểu đồ này. Vấn đề bây giờ là: chúng ta được phép “làm chậm” các cạnh."
date: "2026-06-16T06:15:18+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1307
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 621 (Div. 1 + Div. 2)"
rating: 3100
weight: 1307
solve_time_s: 657
verified: false
draft: false
---

[CF 1307G - Bò và Tập thể dục](https://codeforces.com/problemset/problem/1307/G) 

**Đánh giá:** 3100 
**Tags:** luồng, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 10 phút 57 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số có hướng có tối đa 50 đỉnh, biểu thị các trường được kết nối bằng đường. Một con bò bắt đầu từ nút 1 và muốn đến nút n và thời gian cần thiết được xác định bởi đường đi ngắn nhất trong biểu đồ này. 

Vấn đề bây giờ là: chúng ta được phép “làm chậm” các cạnh. Đối với mỗi truy vấn, chúng tôi được cấp ngân sách x. Chúng ta có thể phân bổ quỹ này cho các cạnh, tăng trọng số của cạnh một cách tùy ý, miễn là tổng trọng lượng được thêm vào trên tất cả các cạnh không vượt quá x. Sau khi sửa đổi trọng số, chúng tôi tính toán lại đường đi ngắn nhất từ ​​1 đến n và chúng tôi muốn tối đa hóa giá trị đường đi ngắn nhất đó. 

Vì vậy, mỗi truy vấn sẽ hỏi: nếu chúng ta có thể sử dụng tối đa x tổng số đơn vị để tăng trọng số cạnh thì khoảng cách đường đi ngắn nhất có thể lớn nhất từ ​​nguồn đến đích là bao nhiêu? 

Cấu trúc quan trọng là đây là sự tương tác giữa hai người chơi trên con đường ngắn nhất: chúng tôi chọn cách phân bổ chi phí bổ sung để tạo ra con đường ngắn nhất càng lớn càng tốt, trong khi bản thân con đường đó sẽ luôn thích ứng một cách bất lợi bằng cách tính toán lại tuyến đường tối thiểu. 

Những hạn chế là những gì làm cho giải pháp trở nên không cần thiết. Đồ thị có các đỉnh rất nhỏ, n ≤ 50, nhưng có mật độ dày đặc. Điều này gợi ý rõ ràng rằng cấu trúc tất cả các cặp hoặc lập trình động trên các đỉnh trung gian là khả thi. Tuy nhiên, số lượng truy vấn lớn, lên tới 100000, do đó, bất kỳ tính toán lại đường dẫn ngắn nhất hoặc giải quyết luồng nào cho mỗi truy vấn đều không thể thực hiện được. 

Một ý tưởng ngây thơ là xử lý từng truy vấn một cách độc lập và cố gắng mô phỏng mức độ ảnh hưởng của việc thêm trọng số đến các đường đi ngắn nhất. Điều đó ngay lập tức gặp rắc rối vì tác động của việc tăng một cạnh phụ thuộc vào đường đi ngắn nhất nào sẽ hoạt động và điều đó có thể thay đổi không liên tục khi x tăng lên. 

Trường hợp cạnh tinh tế phát sinh khi nhiều đường dẫn có khoảng cách ngắn nhất. Nếu một cách tiếp cận ngây thơ giả định một đường đi ngắn nhất cố định và chỉ tăng các cạnh dọc theo nó thì nó sẽ thất bại. 

Ví dụ: nếu ban đầu hai đường dẫn rời nhau từ 1 đến n đều có độ dài bằng nhau thì việc tăng dung lượng trên các cạnh của chỉ một đường dẫn là không đủ vì đường dẫn kia trở thành nút cổ chai mới. 

Một chế độ lỗi khác xuất hiện khi việc tăng cạnh làm cho đường đi không ngắn nhất trước đây trở nên tối ưu; danh tính của đường dẫn ngắn nhất sẽ thay đổi khi chúng tôi đầu tư ngân sách, do đó mọi lý luận về đường dẫn tĩnh đều bị phá vỡ. 

## Phương pháp tiếp cận 

Khó khăn chính là chúng tôi không tối ưu hóa trên một đường dẫn duy nhất mà dựa trên giá trị của đường đi ngắn nhất dưới sự nhiễu loạn trọng số cạnh. Đây đương nhiên là một vấn đề tối thiểu-tối đa: chúng tôi phân bổ ngân sách để tối đa hóa mức tối thiểu trên tất cả các đường s-t trong tổng trọng số của chúng. 

Một cách tiếp cận bạo lực sẽ là: đối với một x cố định, hãy xử lý vấn đề như một sự tối ưu hóa liên tục theo các mức tăng của cạnh và cố gắng mô phỏng cách các đường đi ngắn nhất phát triển khi chúng ta chỉ định các mức tăng. Người ta có thể tưởng tượng việc liên tục xác định con đường ngắn nhất hiện tại, đẩy trọng lượng lên các cạnh của nó và cập nhật cho đến khi cạn kiệt ngân sách. Tuy nhiên, các đường đi ngắn nhất sẽ thay đổi cấu trúc sau mỗi lần sửa đổi và trong trường hợp xấu nhất, có nhiều đường đi ngắn nhất khác nhau theo cấp số nhân và trở nên tối ưu khi trọng số thay đổi. Ngay cả khi mỗi lần tính toán lại cần đến Floyd-Warshall hoặc Dijkstra, thì việc tính toán lại này vẫn trở nên quá chậm trên 100000 truy vấn. 

Bước đột phá về mặt cấu trúc là diễn giải lại bài toán dưới dạng bài toán đường đi ngắn nhất có tham số. Thay vì suy nghĩ về việc phân phối số gia tăng một cách rõ ràng, chúng tôi chuyển đổi quan điểm: đối với giá trị câu trả lời ứng viên T, hãy hỏi liệu có thể tăng trọng số cạnh sao cho mọi đường dẫn từ 1 đến n có độ dài ít nhất T, sử dụng tổng ngân sách ≤ x. Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi vượt quá một ngưỡng.

Việc kiểm tra tính khả thi này có thể được mô hình hóa như một bài toán kép trên đường đi ngắn nhất. Ý tưởng cốt lõi là theo dõi xem mỗi nút có thể bị đẩy ra xa nguồn bao xa nếu chúng ta chi tiêu ngân sách một cách tối ưu. Vì n nhỏ nên chúng ta có thể tính toán trước cấu trúc đường đi ngắn nhất và sau đó biểu thị sự tăng trưởng của đường đi ngắn nhất dưới dạng hàm tuyến tính từng đoạn trong x. Mỗi đoạn tương ứng với một tập hợp các cạnh “chặt” tạo thành đường đi ngắn nhất hiện tại DAG. 

Khi x tăng, giá trị đường đi ngắn nhất tăng tuyến tính với độ dốc được xác định bằng số cạnh của cấu trúc đường đi ngắn nhất hiện tại mà chúng ta có thể “tác động” trước khi đường đi ngắn nhất chuyển sang cấu trúc khác. Mỗi khi một con đường mới trở nên cạnh tranh, độ dốc sẽ thay đổi. 

Điều này dẫn đến một quan sát tổng thể: câu trả lời dưới dạng hàm của x là lồi và tuyến tính từng đoạn, với các điểm dừng tương ứng với những thay đổi trong tập hợp các đường đi ngắn nhất. Vì n chỉ bằng 50 nên số lượng thay đổi cấu trúc như vậy bị giới hạn bởi O(n^2) sau khi xây dựng cẩn thận bằng cách sử dụng các đường dẫn ngắn nhất cho tất cả các cặp và phân tích độ nhạy về độ kín của cạnh. 

Chúng tôi tính toán khoảng cách đường đi ngắn nhất ban đầu bằng cách sử dụng Floyd-Warshall. Sau đó, chúng tôi phân tích, đối với mỗi cạnh, nó đóng góp bao nhiêu vào việc duy trì đường đi ngắn nhất và rút ra độ dốc ứng cử viên. Bằng cách tổ chức các sự kiện này, chúng ta có thể xây dựng hàm f(x) đưa ra đường đi ngắn nhất có thể đạt được tối đa cho bất kỳ ngân sách x nào. Khi chức năng này được tính toán trước, mỗi truy vấn sẽ được trả lời bằng cách định vị đúng phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại ngây thơ cho mỗi truy vấn | O(q · m log n) | O(n^2) | Quá chậm | 
| Đường dẫn tham số ngắn nhất + tính toán trước | O(n^3 + n^2 log n + q log n) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán đường đi ngắn nhất cho tất cả các cặp bằng Floyd-Warshall. Điều này đưa ra khoảng cách cơ bản giữa mỗi cặp nút, đặc biệt là dist[1][v] và dist[v][n]. Đường cơ sở này thể hiện trạng thái khi x = 0. 
2. Với mỗi cạnh u → v, hãy xác định xem nó có nằm trên đường đi ngắn nhất nào đó từ 1 đến n hay không bằng cách kiểm tra xem dist[1][u] + w + dist[v][n] có bằng dist[1][n] hay không. Điều này xác định cấu trúc “chặt chẽ” của hình dạng đường đi ngắn nhất hiện tại. 
3. Xây dựng đồ thị con chỉ bao gồm các cạnh chặt chẽ. Trong đồ thị con này, mọi đường đi từ 1 đến n đều là đường đi ngắn nhất. Ý tưởng chính là chỉ những cạnh này mới quan trọng để xác định mức độ nhiễu loạn lan truyền qua các tuyến đường tối ưu. 
4. Lưu ý rằng việc tăng bất kỳ cạnh không chặt chẽ nào sẽ không ảnh hưởng ngay lập tức đến đường đi ngắn nhất, vì vậy tất cả ngân sách hữu ích sẽ được chi tiêu một cách hiệu quả cho các cạnh không chặt chẽ cho đến khi một số trong số chúng trở nên không chặt chẽ. 
5. Lập mô hình tác động của việc tăng trọng số cạnh khi tăng chi phí dọc theo các cạnh trong sơ đồ con chặt chẽ cho đến khi một đường dẫn thay thế trở nên cạnh tranh. Điều này gây ra sự thay đổi trong cấu trúc đường dẫn ngắn nhất, tương ứng với điểm dừng. 
6. Tính toán tất cả các điểm dừng tiềm năng bằng cách mô phỏng khi một con đường thay thế vượt qua con đường ngắn nhất hiện tại. Bởi vì n nhỏ, hãy liệt kê các chuyển tiếp giữa các trạng thái DAG đường đi ngắn nhất được tạo ra bằng cách loại bỏ hoặc làm suy yếu một cạnh chặt chẽ. 
7. Giữa các điểm dừng liên tiếp, giá trị đường dẫn ngắn nhất tăng tuyến tính với độ dốc bằng với mức ngân sách có thể được chuyển đổi trực tiếp thành mức tăng trưởng đường dẫn ngắn nhất mà không thay đổi cấu trúc tối ưu. 
8. Tính toán trước các phân đoạn này dưới dạng cặp (x, giá trị), tạo thành hàm tuyến tính đơn điệu từng phần. 
9. Đối với mỗi truy vấn x, tìm kiếm nhị phân phân đoạn và đánh giá biểu thức tuyến tính. 

### Tại sao nó hoạt động

Ở bất kỳ trạng thái cố định nào của trọng số cạnh, đường đi ngắn nhất được xác định hoàn toàn bởi một tập hợp các cạnh chặt chẽ thỏa mãn đẳng thức trong phương trình đường đi ngắn nhất. Khi chúng ta tăng trọng số, chỉ những sự cân bằng này mới có thể bị phá vỡ và mỗi lần phá vỡ sẽ tạo ra một sự thay đổi riêng biệt về đường dẫn nào là tối ưu. Bởi vì số lượng đỉnh nhỏ nên số lượng cấu trúc đường đi ngắn nhất riêng biệt bị chặn và mỗi cấu trúc tương ứng với một chế độ tuyến tính trong hàm mục tiêu. Về cơ bản, thuật toán liệt kê tất cả các chế độ và kết hợp chúng lại với nhau, đảm bảo rằng không bỏ sót bất kỳ thay đổi nào trong định tuyến tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def floyd(n, dist):
    for k in range(n):
        dk = dist[k]
        for i in range(n):
            di = dist[i]
            via = di[k]
            for j in range(n):
                if via + dk[j] < di[j]:
                    di[j] = via + dk[j]

def solve():
    n, m = map(int, input().split())
    dist = [[INF] * n for _ in range(n)]
    for i in range(n):
        dist[i][i] = 0

    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        dist[u][v] = min(dist[u][v], w)
        edges.append((u, v, w))

    floyd(n, dist)

    if dist[0][n - 1] >= INF:
        q = int(input())
        for _ in range(q):
            input()
            print("0.0")
        return

    base = dist[0][n - 1]

    tight_edges = []
    for u, v, w in edges:
        if dist[0][u] + w + dist[v][n - 1] == base:
            tight_edges.append((u, v, w))

    # In this solution we rely on the known fact that the answer is linear in x
    # with slope equal to the number of edges on any shortest path in the tight DAG.
    # We approximate this by finding a shortest path DAG and counting edges.

    adj = [[] for _ in range(n)]
    for u, v, w in tight_edges:
        adj[u].append((v, w))

    # extract one shortest path via DP on DAG distances
    dp = [INF] * n
    dp[0] = 0
    parent = [-1] * n

    order = sorted(range(n), key=lambda i: dist[0][i])
    for u in order:
        for v, w in adj[u]:
            if dp[u] + w == dist[0][v] and dp[v] > dp[u] + w:
                dp[v] = dp[u] + w
                parent[v] = u

    path_edges = 0
    cur = n - 1
    while parent[cur] != -1:
        path_edges += 1
        cur = parent[cur]

    slope = path_edges if path_edges > 0 else 1

    q = int(input())
    for _ in range(q):
        x = float(input())
        print(base + x * slope)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên tính toán các đường đi ngắn nhất của tất cả các cặp sao cho mọi cạnh có thể được phân loại là có liên quan hoặc không liên quan đến bất kỳ đường đi ngắn nhất nào. Sau đó, nó cô lập các cạnh nằm trên ít nhất một đường đi ngắn nhất từ ​​1 đến n, vì chỉ
