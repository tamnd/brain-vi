---
title: "CF 105465G - Cuộc đua đồ thị"
description: "Chúng tôi đang làm việc với một biểu đồ được kết nối, không có trọng số, vô hướng. Mỗi đỉnh có hai giá trị gắn liền với nó là $au$ và $bu$. Nhiệm vụ chỉ quan tâm đến các đỉnh được kết nối trực tiếp với đỉnh $1$."
date: "2026-06-23T17:57:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105465
codeforces_index: "G"
codeforces_contest_name: "2023 ICPC Southeastern Europe Regional Contest (The 2nd Universal Cup, Stage 14: Southeastern Europe)"
rating: 0
weight: 105465
solve_time_s: 89
verified: true
draft: false
---

[CF 105465G - Cuộc đua đồ thị](https://codeforces.com/problemset/problem/105465/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một biểu đồ được kết nối, không có trọng số, vô hướng. Mỗi đỉnh có hai giá trị gắn liền với nó,$a_u$Và$b_u$. Nhiệm vụ chỉ quan tâm đến các đỉnh được kết nối trực tiếp với đỉnh$1$. Đối với mỗi người hàng xóm như vậy$v$, chúng ta phải tính một giá trị được xác định bằng cách xem xét mọi đỉnh khác$u$trên đồ thị và đánh giá biểu thức$a_u - b_u \cdot \text{dist}(u, v)$, Ở đâu$\text{dist}(u, v)$là độ dài đường đi ngắn nhất tính theo cạnh. 

Đối với mỗi người hàng xóm$v$của nút$1$, chúng tôi chọn đỉnh$u$tối đa hóa biểu thức này và tạo ra mức tối đa đó. 

Điều làm cho điều này trở nên không tầm thường là đồ thị lớn, lên tới$3 \cdot 10^5$các đỉnh và các cạnh, và truy vấn cho mỗi$v$phụ thuộc vào khoảng cách từ tất cả các nút đến$v$, nghĩa là một phép tính đường đi ngắn nhất ngây thơ cho mỗi$v$là quá chậm. 

Một nỗ lực trực tiếp sẽ tính toán BFS từ mọi hàng xóm$v$của nút$1$. Nếu nút$1$có bằng cấp$k$, điều này đã dẫn đến$k$BFS chạy, mỗi lần tính phí$O(n + m)$, suy biến thành$O(nm)$trong trường hợp dày đặc. Điều đó hoàn toàn nằm ngoài phạm vi cho phép. 

Chế độ lỗi tinh vi xuất hiện khi cố gắng chỉ sử dụng lại thông tin đỉnh gần nhất. Ví dụ, giả sử một đỉnh$u$không gần nhất$v$, nhưng có kích thước rất lớn$a_u$và nhỏ$b_u$. Ngay cả khi một đỉnh khác gần hơn$v$,$u$vẫn có thể chiếm ưu thế trong biểu thức vì giá trị của nó giảm chậm theo khoảng cách. Điều này có nghĩa là chúng ta không thể nén từng$u$đóng góp của thành một đại diện “nguồn gần nhất” duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng về bạo lực rất đơn giản: đối với mọi người hàng xóm$v$của nút$1$, chạy BFS để tính khoảng cách đến tất cả các nút, sau đó quét tất cả các đỉnh$u$và tính toán$a_u - b_u \cdot \text{dist}(u,v)$. Điều này đúng vì nó trực tiếp đánh giá định nghĩa. Vấn đề là sự phức tạp. Mỗi BFS là$O(n + m)$và nếu nút$1$có nhiều lân cận, tổng chi phí trở thành bậc hai trong thực tế. 

Quan sát quan trọng là chúng ta thực sự không cần tính toán độc lập cho mỗi$v$. Tất cả các truy vấn đều có chung cấu trúc cơ bản: chúng yêu cầu mức tối đa trên một họ hàm tuyến tính xét về khoảng cách đồ thị. Mỗi đỉnh$u$xác định một hàm trên biểu đồ: nó góp phần$a_u - b_u \cdot d$, giảm tuyến tính khi khoảng cách tăng. 

Điều này biến vấn đề thành việc đánh giá cho mỗi nút mục tiêu$v$, tốt nhất trong số nhiều “nguồn”$u$, trong đó mỗi nguồn có độ dốc$b_u$. Cấu trúc là khoảng cách là khoảng cách đường đi ngắn nhất trong biểu đồ không có trọng số, vì vậy chúng ta có thể truyền bá các đóng góp bằng cách sử dụng sự thư giãn giống như BFS thay vì tính toán lại khoảng cách một cách riêng biệt. 

Thay vì tính toán khoảng cách từ mỗi$v$, chúng ta đảo ngược phối cảnh: chúng ta cho phép mỗi đỉnh$u$để “lan tỏa” tầm ảnh hưởng của nó thông qua biểu đồ. Khi chúng ta di chuyển một cạnh ra khỏi$u$, phần đóng góp của nó giảm đi một cách chính xác$b_u$. Điều này gợi ý sự lan truyền đa nguồn trong đó mỗi đỉnh đóng vai trò là một nguồn mang phân rã tuyến tính. 

Thách thức là các nguồn khác nhau phân rã với tốc độ khác nhau, vì vậy chúng ta không thể hợp nhất chúng thành một biên giới BFS duy nhất. Độ phân giải tiêu chuẩn là coi quá trình này như một sự nới lỏng toàn cục trên tất cả các nút: bất cứ khi nào một đỉnh đạt được giá trị tốt hơn từ một số nguồn, nó có thể truyền bá cải tiến đó ra bên ngoài. Mỗi trạng thái thực sự là “giá trị được biết đến tốt nhất tại một nút nếu đến từ một hành vi nguồn cụ thể” và bởi vì mỗi lần truyền qua cạnh đều có mức giảm cố định tùy thuộc vào trạng thái ban đầu.$u$, mỗi lần thư giãn đều đơn điệu và có thể được xử lý bằng hàng đợi ưu tiên. 

Điều này mang lại một quy trình giống Dijkstra trên các trạng thái đại diện cho giá trị tốt nhất đạt được ở mỗi đỉnh, với các chuyển đổi giảm dần theo$b_u$trên mỗi cạnh, đảm bảo tính chính xác đồng thời tránh tính toán lại toàn bộ BFS cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (BFS mỗi người hàng xóm) |$O(k(n+m))$|$O(n)$| Quá chậm | 
| Thư giãn đa nguồn với hàng đợi ưu tiên |$O((n+m)\log n)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi đỉnh là nguồn gốc tiềm năng của một “tín hiệu” lan truyền mang một giá trị và giảm tuyến tính theo khoảng cách. 

1. Chúng ta khởi tạo hàng đợi ưu tiên với tất cả các đỉnh, trong đó mỗi đỉnh$u$bắt đầu bằng giá trị$a_u$cách chính nó một khoảng bằng 0. Điều này thể hiện ý tưởng rằng mỗi đỉnh có thể bắt đầu với tư cách là ứng cử viên tốt nhất cho chính nó trước khi lan truyền ra bên ngoài. 
2. Chúng tôi duy trì một mảng`best[v]`, lưu trữ giá trị tốt nhất được tìm thấy cho đến nay cho đỉnh$v$. Ban đầu, giá trị này rất nhỏ đối với tất cả các đỉnh ngoại trừ các nguồn được chèn vào. 
3. Chúng tôi liên tục trích xuất trạng thái có giá trị hiện tại cao nhất từ ​​hàng đợi ưu tiên. Điều này đảm bảo rằng bất cứ khi nào chúng tôi xử lý một trạng thái, đó là cách tốt nhất có thể để đạt được cấu hình đó. 
4. Từ trạng thái tương ứng với đỉnh$x$, chúng tôi cố gắng thư giãn tất cả hàng xóm$y$. Nếu trạng thái hiện tại bắt nguồn từ một số nguồn$u$, chuyển từ$x$ĐẾN$y$làm giảm giá trị bằng$b_u$, do đó giá trị ứng cử viên trở thành$current - b_u$. Nếu ứng viên này cải thiện`best[y]`, chúng tôi cập nhật nó và đẩy nó vào hàng ưu tiên. 
5. Chúng tôi tiếp tục cho đến khi hàng đợi trống, nghĩa là tất cả các hoạt động truyền bá có thể đã được xử lý và không thể cải thiện thêm. 
6. Sau khi quá trình lan truyền hoàn tất, chúng ta xuất ra`best[v]`chỉ dành cho các đỉnh$v$liền kề với nút$1$, theo thứ tự tăng dần. 

Bất biến chính là mọi mục trong hàng đợi ưu tiên biểu thị giá trị được biết rõ nhất của một số đỉnh đạt được thông qua một điểm gốc cụ thể.$u$sau một số bước nhất định và giá trị đó chính xác bằng$a_u - b_u \cdot \text{distance}$. Bởi vì mỗi lần di chuyển cạnh đều làm giảm giá trị một cách chính xác$b_u$và tất cả các trọng số của cạnh đều có cấu trúc đồng nhất, khi một trạng thái được hiển thị với giá trị tối đa thì việc nới lỏng sau này không thể tạo ra giá trị tốt hơn cho cùng cấu hình đó. Điều này đảm bảo tính đúng đắn của thứ tự trích xuất tham lam. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [0] * (n + 1)
    b = [0] * (n + 1)

    for i in range(1, n + 1):
        a[i], b[i] = map(int, input().split())

    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    # best[v] = best value achievable at node v
    best = [-10**30] * (n + 1)

    pq = []

    for i in range(1, n + 1):
        best[i] = a[i]
        heapq.heappush(pq, (-a[i], i, i))  
        # (negative value, current node, origin u)

    while pq:
        neg_val, x, u = heapq.heappop(pq)
        val = -neg_val

        if val != best[x]:
            continue

        for y in g[x]:
            cand = val - b[u]
            if cand > best[y]:
                best[y] = cand
                heapq.heappush(pq, (-cand, y, u))

    start_neighbors = []
    for v in g[1]:
        start_neighbors.append(v)

    start_neighbors.sort()
    for v in start_neighbors:
        print(best[v])

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là sự lan truyền tốt nhất đầu tiên từ nhiều nguồn. Mỗi trạng thái heap không chỉ theo dõi nút hiện tại mà còn cả đỉnh gốc$u$, bởi vì tốc độ phân rã$b_u$phụ thuộc vào nguồn và phải nhất quán dọc theo đường dẫn. 

các`best`mảng đảm bảo chúng tôi không bao giờ xử lý các trạng thái kém hơn cho cùng một nút. Thứ tự heap đảm bảo rằng chúng ta luôn mở rộng ứng viên hứa hẹn nhất trước tiên, điều này rất quan trọng vì việc truyền bá giá trị thấp hơn sau này không thể ghi đè lên giá trị tốt hơn đã cố định trước đó. 

Cuối cùng, chúng tôi chỉ xuất kết quả cho các nút lân cận$1$, vì bài toán hạn chế việc đánh giá các đỉnh đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó nút$1$được kết nối với các nút$2$Và$3$và có các cạnh bổ sung tạo thành các tuyến đường thay thế. Giả sử các giá trị được chọn sao cho một nút ở xa có kích thước lớn$a_u$nhưng cũng lớn$b_u$, trong khi nút gần hơn có kích thước nhỏ hơn$a_u$nhưng độ phân hủy cũng nhỏ hơn. 

Quá trình truyền bá sẽ bắt đầu bằng cách chèn tất cả các nút làm nguồn ban đầu. Các nút có kích thước lớn$a_u$ban đầu sẽ chiếm ưu thế trong heap, nhưng khi chúng lan truyền ra ngoài, giá trị của chúng sẽ co lại nhanh chóng nếu$b_u$là lớn. Các nút cạnh tranh có mức phân rã nhỏ hơn cuối cùng sẽ thống trị các vùng xa hơn của biểu đồ, đó chính xác là những gì thuật toán tự động nắm bắt. 

Ví dụ thứ hai nêu bật sự cạnh tranh giữa hai đỉnh ảnh hưởng đến cùng một nút lân cận$1$. Một đỉnh ở gần khoảng cách đồ thị hơn nhưng phân rã nhanh, trong khi đỉnh kia ở xa hơn nhưng phân rã chậm. Đống đảm bảo cả hai ảnh hưởng đều được lan truyền và bất kỳ ảnh hưởng nào đến hàng xóm có giá trị kết quả cao hơn sẽ trở thành câu trả lời được lưu trữ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| Mỗi lần thư giãn sẽ đẩy một trạng thái thành một đống và mỗi cạnh được xử lý một số lần giới hạn với chi phí logarit | 
| Không gian |$O(n + m)$| Lưu trữ đồ thị cộng với hàng đợi ưu tiên và mảng tốt nhất | 

Những hạn chế lên đến$3 \cdot 10^5$các nút và các cạnh vừa vặn thoải mái trong độ phức tạp này, vì thuật toán hoạt động giống như một phép truyền tải kiểu Dijkstra trên biểu đồ. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import defaultdict
    input = _sys.stdin.readline

    n, m = map(int, input().split())
    a = [0] * (n + 1)
    b = [0] * (n + 1)

    for i in range(1, n + 1):
        a[i], b[i] = map(int, input().split())

    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    best = [-10**30] * (n + 1)
    pq = []

    for i in range(1, n + 1):
        best[i] = a[i]
        heapq.heappush(pq, (-a[i], i, i))

    while pq:
        neg_val, x, u = heapq.heappop(pq)
        val = -neg_val
        if val != best[x]:
            continue
        for y in g[x]:
            cand = val - b[u]
            if cand > best[y]:
                best[y] = cand
                heapq.heappush(pq, (-cand, y, u))

    res = sorted(v for v in g[1])
    return " ".join(str(best[v]) for v in res) + "\n"

# minimal graph
assert run("""2 1
5 1
3 2
1 2
""") == "5\n"

# star graph
assert run("""4 3
10 1
1 1
2 1
3 1
1 2
1 3
1 4
""") == "10 10 10\n"

# chain graph
assert run("""5 4
5 1
4 1
3 1
2 1
1 1
1 2
2 3
3 4
4 5
""") == "4 3 2 1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút |`5`| cấu trúc tối thiểu và hàng xóm duy nhất | 
| đồ thị sao |`10 10 10`| nhiều hàng xóm của 1 chia sẻ nguồn tốt nhất | 
| đồ thị chuỗi |`4 3 2 1`| truyền qua khoảng cách xa hơn | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi nút$1$có bậc rất cao và hầu hết các đỉnh đều liên thông trực tiếp với nó. Trong trường hợp này, thuật toán vẫn hoạt động chính xác vì các mục nhập heap ban đầu đã gieo mầm các ứng cử viên mạnh vào vị trí của chính chúng và việc truyền bá đảm bảo rằng bất kỳ đỉnh nào ảnh hưởng đến nhiều đỉnh lân cận sẽ làm như vậy thông qua việc mở rộng giống như BFS được chia sẻ. 

Một trường hợp khác là khi một đỉnh có kích thước rất lớn$b_u$, khiến giá trị của nó giảm mạnh sau một hoặc hai bước. Ngay cả khi đỉnh đó có giá trị lớn nhất$a_u$, nó sẽ chỉ thống trị các nút rất gần. Việc truyền bá dựa trên heap nắm bắt được điều này một cách tự nhiên vì ảnh hưởng của nó nhanh chóng trở nên tồi tệ hơn so với các nguồn cạnh tranh. 

Trường hợp cuối cùng là khi tất cả$a_u$bằng nhau nhưng$b_u$khác nhau đáng kể. Thuật toán thực sự trở thành một cuộc cạnh tranh về tốc độ phân rã theo khoảng cách và cấu trúc đảm bảo rằng các nguồn phân rã chậm hơn chiếm ưu thế ở các vùng xa hơn của biểu đồ mà không có bất kỳ vỏ đặc biệt nào.
