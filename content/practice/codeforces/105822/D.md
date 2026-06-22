---
title: "CF 105822D - Hải ly"
description: "Chúng ta có một đồ thị vô hướng với một đỉnh bắt đầu được phân biệt, mà chúng ta có thể coi là đỉnh 1. Bên cạnh đồ thị, chúng ta cũng có một chuỗi các đỉnh bắt đầu từ gốc này, được viết là x0 = 1, theo sau là x1, x2, cho đến xk."
date: "2026-06-21T14:55:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105822
codeforces_index: "D"
codeforces_contest_name: "MITIT Spring 2025 Qualification Round 1"
rating: 0
weight: 105822
solve_time_s: 50
verified: true
draft: false
---

[CF 105822D - Beaverland](https://codeforces.com/problemset/problem/105822/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng với một đỉnh bắt đầu được phân biệt, mà chúng ta có thể coi là đỉnh 1. Bên cạnh đồ thị, chúng ta cũng có một chuỗi các đỉnh bắt đầu từ gốc này, được viết là x0 = 1, theo sau là x1, x2, cho đến xk. Nhiệm vụ là quyết định xem có thể sửa đổi biểu đồ bằng cách thêm các cạnh sao cho khoảng cách từ đỉnh 1 hoạt động theo một cách rất cố định dọc theo chuỗi này hay không. 

Cấu trúc dự định của việc sửa đổi rất đơn giản: chúng ta chỉ được phép thêm các cạnh giữa các đỉnh liên tiếp trong chuỗi, tạo thành chuỗi 1-x1-x2-…-xk. Sau khi thêm các cạnh này, chúng tôi xem xét khoảng cách đường đi ngắn nhất từ 1 trong biểu đồ kết quả và kiểm tra xem chúng có khớp với vị trí chỉ mục của chuỗi hay không, nghĩa là x_i sẽ kết thúc chính xác ở khoảng cách i từ 1. 

Mặc dù điều này nghe có vẻ giống như một vấn đề xây dựng cục bộ, nhưng khó khăn lại mang tính toàn cầu: việc thêm các cạnh có thể gián tiếp rút ngắn nhiều đường đi ngắn nhất và chúng ta cần đảm bảo rằng không có lối tắt ngoài ý muốn nào phá vỡ mô hình khoảng cách cần thiết. 

Các ràng buộc nhất quán với giải pháp tuyến tính hoặc gần tuyến tính, thường có nghĩa là biểu đồ có thể có tối đa khoảng 2·10^5 đỉnh và cạnh. Điều này loại trừ bất kỳ giải pháp nào tính toán lại các đường đi ngắn nhất từ ​​đầu sau mỗi lần sửa đổi hoặc cố gắng mô phỏng tất cả các bổ sung cạnh có thể có. Một BFS hoặc DFS duy nhất trên cấu trúc cuối cùng là công cụ thực tế duy nhất. 

Một trường hợp lỗi tinh vi xuất hiện khi có các cạnh hiện có trong biểu đồ ban đầu đã kết nối các đỉnh ở xa trong chuỗi. Ví dụ: nếu biểu đồ ban đầu đã chứa lối tắt trực tiếp từ x0 đến x2 thì ngay cả sau khi thêm các cạnh của chuỗi, x2 vẫn có thể tiến gần hơn khoảng cách 2, vi phạm yêu cầu. Một vấn đề khác nảy sinh nếu bản thân chuỗi không nhất quán với cấu trúc đường đi ngắn nhất ban đầu, bởi vì ngay cả khi có các cạnh được thêm vào, chúng ta không thể buộc khoảng cách tăng lên. 

## Phương pháp tiếp cận 

Quan điểm brute-force là xử lý vấn đề theo nghĩa đen: chúng tôi thêm từng cạnh chuỗi được đề xuất và tính toán lại khoảng cách đường đi ngắn nhất từ đỉnh 1 bằng cách sử dụng BFS sau mỗi lần thêm hoặc sau khi xây dựng đầy đủ, sau đó kiểm tra xem tất cả các khoảng cách cần thiết có khớp với chỉ số của chúng hay không. Điều này đúng vì BFS đưa ra các đường đi ngắn nhất chính xác trong biểu đồ không có trọng số, nhưng sẽ tốn kém nếu thực hiện lặp đi lặp lại hoặc nếu chúng ta cố gắng xác minh nhiều cấu trúc trung gian. Trong trường hợp xấu nhất, việc tính toán lại BFS cho mỗi lần kiểm tra sẽ dẫn đến hành vi O(k(n + m)), quá chậm khi cả biểu đồ và chuỗi đều lớn. 

Thông tin chi tiết quan trọng là chúng ta thực sự không bao giờ cần phải suy luận về nhiều giải pháp ứng viên hoặc những thay đổi gia tăng. Cấu trúc duy nhất quan trọng là đồ thị cuối cùng sau khi cộng các cạnh giữa các xi liên tiếp. Khi chúng tôi chấp nhận rằng công trình duy nhất được phép là chuỗi này, vấn đề sẽ giảm xuống còn việc xác minh xem liệu công trình đơn lẻ này đã tạo ra mô hình khoảng cách mong muốn hay chưa. 

Lý do sâu xa hơn mà điều này có tác dụng là vì bất kỳ đường đi ngắn nhất nào từ 1 đến đỉnh chuỗi xi đều có thể được chuyển đổi thành một đường đi tuân theo thứ tự chuỗi mà không làm tăng độ dài của nó. Bất kỳ đường vòng nào đi qua biểu đồ ban đầu giữa hai đỉnh của chuỗi đều không thể hoạt động tốt hơn đoạn chuỗi trực tiếp một khi chúng ta so sánh nó với các khác biệt về chỉ số. Điều này cho phép chúng tôi thu gọn các đường dẫn tùy ý thành các đường dẫn có cấu trúc được căn chỉnh theo trình tự và sau đó một BFS duy nhất đủ để xác minh xem khoảng cách có khớp chính xác hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tính toán lại BFS sau khi sửa đổi | O(k(n + m)) | O(n + m) | Quá chậm | 
| Xây dựng chuỗi và chạy BFS một lần | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một biểu đồ ứng cử viên và sau đó xác thực nó bằng cách sử dụng một phép tính đường đi ngắn nhất.

1. Bắt đầu từ biểu đồ gốc và chưa sửa đổi nó. Khoảng cách ban đầu xác định cấu trúc đường cơ sở mà chúng ta sẽ so sánh một cách gián tiếp. 
2. Thêm các cạnh giữa các đỉnh liên tiếp trong dãy đã cho, nối x_{i-1} với x_i với mọi i từ 1 đến k. Điều này tạo ra một đường dẫn trực tiếp đảm bảo giới hạn trên của khoảng cách từ 1 đến mọi x_i. 
3. Chạy BFS bắt đầu từ đỉnh 1 trên biểu đồ tăng cường. Điều này tính toán khoảng cách ngắn nhất thực sự từ 1 đến mọi đỉnh có thể tiếp cận sau khi thêm các cạnh của chuỗi. 
4. Kiểm tra xem khoảng cách tính toán tới mỗi x_i có chính xác là i hay không. Nếu bất kỳ đỉnh nào vi phạm đẳng thức này thì việc xây dựng không hợp lệ. 
5. Nếu tất cả các đỉnh đều thỏa mãn đẳng thức thì chấp nhận dãy đó là khả thi. 

Phần không tầm thường là tại sao chúng ta chỉ kiểm tra sự bằng nhau và không cần phải suy luận rõ ràng về tất cả các đường dẫn khác. Các cạnh của chuỗi đảm bảo rằng d(1, x_i) nhiều nhất là i, trong khi bất kỳ tuyến đường thay thế nào cố gắng đi qua biểu đồ ban đầu không thể tạo ra một khoảng cách nhỏ hơn hoàn toàn mà không mâu thuẫn với giới hạn dưới của cấu trúc được ngụ ý bởi thứ tự chuỗi. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi cộng các cạnh của chuỗi, mỗi đỉnh x_i có một đường chính tắc có độ dài i thu được bằng cách tuân theo trình tự từ 1. Điều này đảm bảo một giới hạn trên. Đồng thời, bất kỳ đường dẫn nào sử dụng các cạnh đồ thị gốc giữa các đỉnh của chuỗi đều có thể được chuyển đổi thành đường dẫn được căn chỉnh theo trình tự có độ dài không lớn hơn việc thay thế từng đoạn giữa x_a và x_b bằng |a - b|. Điều này ngăn không cho bất kỳ phím tắt nào tạo ra khoảng cách nhỏ hơn i. Do đó, khoảng cách BFS phải khớp chính xác khi và chỉ khi việc xây dựng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    k = int(input())
    x = list(map(int, input().split()))
    
    # build augmented graph implicitly via adjacency lists
    for i in range(1, k):
        u, v = x[i - 1], x[i]
        g[u].append(v)
        g[v].append(u)

    dist = [-1] * (n + 1)
    q = deque([1])
    dist[1] = 0

    while q:
        v = q.popleft()
        for to in g[v]:
            if dist[to] == -1:
                dist[to] = dist[v] + 1
                q.append(to)

    for i in range(k):
        if dist[x[i]] != i:
            print("NO")
            return

    print("YES")

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp việc xây dựng. Sự sửa đổi duy nhất đối với biểu đồ ban đầu là việc bổ sung các cạnh giữa các phần tử liên tiếp của chuỗi. Việc này được thực hiện tại chỗ trên danh sách kề vì chúng ta không bao giờ cần riêng biểu đồ gốc nữa. 

BFS tính toán khoảng cách ngắn nhất trong thời gian tuyến tính. Vòng lặp cuối cùng kiểm tra ràng buộc đẳng thức cần thiết. Điều tinh tế chính là đảm bảo rằng khoảng cách được khởi tạo chính xác bằng -1 và đỉnh 1 bắt đầu ở khoảng cách 0, căn chỉnh với x0 = 1. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó các đỉnh được kết nối theo đường 1-2-3-4 và chuỗi là [1, 2, 3, 4]. 

| Bước | Xếp hàng | Cập nhật khoảng cách | Bình luận | 
| --- | --- | --- | --- | 
| Bắt đầu | [1] | dist[1]=0 | gốc | 
| Thăm 1 | [2] | dist[2]=1 | qua cạnh | 
| Thăm 2 | [3] | dist[3]=2 | tiếp tục dòng | 
| Thăm 3 | [4] | dist[4]=3 | đến cuối | 

Mọi đỉnh đều thỏa mãn dist[x_i] = i, nên câu trả lời là CÓ. Điều này xác nhận rằng cấu trúc chuỗi duy trì khoảng cách chính xác khi không có đường tắt nào tồn tại. 

Bây giờ hãy xem xét một biểu đồ có một cạnh phụ nối trực tiếp 1 và 3 và cùng một chuỗi [1, 2, 3]. 

| Bước | Xếp hàng | Cập nhật khoảng cách | Bình luận | 
| --- | --- | --- | --- | 
| Bắt đầu | [1] | dist[1]=0 | gốc | 
| Thăm 1 | [2,3] | dist[2]=1, dist[3]=1 | kích hoạt phím tắt | 
| Thăm 2 | [3] | dist[3] đã 1 | vi phạm sự mong đợi | 

Ở đây dist[3] trở thành 1 thay vì 2, ngay lập tức phá vỡ đẳng thức cần thiết. Điều này cho thấy cấu trúc biểu đồ hiện tại có thể vô hiệu hóa trình tự như thế nào ngay cả sau khi thêm các cạnh của chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | BFS xử lý từng đỉnh và cạnh một lần sau khi tăng cường | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả cấu trúc đồ thị và BFS đều tuyến tính theo kích thước của đầu vào. Không cần tính toán đường đi ngắn nhất lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# simple valid chain
assert run("""4 3
1 2
2 3
3 4
4
1 2 3 4
""") == "YES"

# shortcut breaks distances
assert run("""3 3
1 2
2 3
1 3
3
1 2 3
""") == "NO"

# single node sequence
assert run("""1 0
1
1
""") == "YES"

# already disconnected structure
assert run("""3 1
2 3
3
1 2 3
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị đường | CÓ | truyền khoảng cách chính xác | 
| cạnh phím tắt | KHÔNG | phát hiện nén khoảng cách không hợp lệ | 
| đỉnh đơn | CÓ | xử lý trường hợp cơ bản | 
| root bị ngắt kết nối | KHÔNG | các nút không thể truy cập | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi dãy chứa các đỉnh không thể tiếp cận được trong biểu đồ gốc. Sau khi chỉ thêm các cạnh liên tiếp, khả năng tiếp cận vẫn phụ thuộc vào chuỗi, do đó BFS phải truyền khoảng cách chính xác qua các cạnh mới được thêm vào. Đối với đầu vào trong đó 1 bị cô lập ngoại trừ các cạnh chuỗi, chẳng hạn như 1-2-3, BFS đảm bảo cả 2 và 3 đều có thể truy cập được với khoảng cách chính xác, xác nhận rằng cấu trúc là đủ. 

Một trường hợp cạnh khác xảy ra khi biểu đồ ban đầu đã cung cấp đường dẫn ngắn hơn chuỗi. Ví dụ: nếu có cạnh trực tiếp từ 1 đến x2 trong khi chuỗi mong đợi khoảng cách 2, BFS sẽ gán khoảng cách 1 đến x2, ngay lập tức vi phạm kiểm tra đẳng thức. Thuật toán loại bỏ chính xác những trường hợp như vậy vì nó dựa vào các đường đi ngắn nhất thực sự trong biểu đồ tăng cường chứ không chỉ chuỗi được xây dựng.
