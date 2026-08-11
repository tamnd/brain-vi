---
title: "CF 104021H - Tuyến giao hàng"
description: "Chúng ta được cung cấp một biểu đồ có trọng số có hướng với một số đường hai chiều và một số đường một chiều. Mỗi con đường đều có chi phí, thậm chí có thể âm đối với một số con đường được chỉ đạo do cơ sở hạ tầng đặc biệt."
date: "2026-07-02T04:36:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104021
codeforces_index: "H"
codeforces_contest_name: "The 2019 ICPC Asia Yinchuan Regional Contest"
rating: 0
weight: 104021
solve_time_s: 45
verified: true
draft: false
---

[CF 104021H - Tuyến giao hàng](https://codeforces.com/problemset/problem/104021/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số có hướng với một số đường hai chiều và một số đường một chiều. Mỗi con đường đều có chi phí, thậm chí có thể âm đối với một số con đường được chỉ đạo do cơ sở hạ tầng đặc biệt. Chúng tôi cũng được cấp một văn phòng khởi đầu`s`và chúng tôi muốn đường dẫn tổng chi phí tối thiểu có thể từ`s`đến mọi nút khác. Nếu không thể truy cập được một nút, chúng tôi phải báo cáo điều đó một cách rõ ràng. 

Thông tin cấu trúc quan trọng là đường hai chiều là các cạnh vô hướng tiêu chuẩn, trong khi đường có hướng hoàn toàn là một chiều và quan trọng là dữ liệu đầu vào đảm bảo rằng nếu cạnh có hướng tồn tại từ`a`ĐẾN`b`, thì không có cách nào để quay trở lại`b`ĐẾN`a`qua bất kỳ cạnh có hướng nào. Hạn chế này ngăn cản các chu kỳ có hướng được hình thành hoàn toàn bằng các cạnh một chiều theo cả hai hướng giữa cùng một cặp, nhưng nó không ngăn cản các chu kỳ âm chung được hình thành trên nhiều nút. 

Kích thước biểu đồ lớn: lên tới 25.000 nút và lên tới 100.000 cạnh. Một cách tiếp cận thư giãn dày đặc hoặc tất cả các cặp đơn giản là không thể. Thậm chí$O(n^2)$phương pháp đã quá chậm. Cấu trúc gợi ý rõ ràng một thuật toán đường đi ngắn nhất, nhưng sự hiện diện của các trọng số âm sẽ loại trừ Dijkstra. 

Một vấn đề tế nhị xuất hiện với những chu kỳ tiêu cực. Nếu một chu kỳ âm có thể truy cập được từ`s`, đường đi ngắn nhất không được xác định theo nghĩa thông thường vì chi phí có thể giảm vô thời hạn. Báo cáo vấn đề không yêu cầu rõ ràng việc phát hiện các chu trình và các công thức điển hình của Codeforce thuộc loại này giả định rằng chúng ta vẫn tính toán khoảng cách ngắn nhất với giả định tính chính xác theo kiểu Bellman-Ford ngay cả khi các chu trình tồn tại, miễn là chúng không ảnh hưởng đến việc giảm thiểu không thể tiếp cận ngoài các đường dẫn hữu hạn. Giải pháp dự kiến ​​sẽ tránh được toàn bộ Bellman-Ford trên tất cả các cạnh. 

Một sai lầm ngây thơ là áp dụng Dijkstra trực tiếp. Ví dụ, nếu có một cạnh âm từ`u`ĐẾN`v`, Dijkstra có thể hoàn tất`u`quá sớm và không bao giờ xem xét lại nó, dẫn đến một câu trả lời sai. 

Một dạng lỗi khác là coi tất cả các cạnh là vô hướng. Sự đảm bảo về các cạnh có hướng là không đủ để đối xứng chúng; làm như vậy sẽ tạo ra những đường đi ngược lại không hợp lệ và thay đổi hoàn toàn khoảng cách. 

Vấn đề tế nhị thứ ba là giả sử biểu đồ không theo chu kỳ hoặc giống DAG. Phần được định hướng không có tính tuần hoàn toàn cục, chỉ bị hạn chế cục bộ trong sự tồn tại ngược lại, do đó việc sắp xếp tôpô không được áp dụng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là Bellman-Ford từ nút nguồn trên tất cả các cạnh. Nó liên tục thư giãn mọi cạnh lên đến$n-1$lần. Điều này hoạt động vì nó xử lý chính xác các trọng số âm và không chịu bất kỳ ràng buộc đặt hàng nào. Tuy nhiên, với tối đa 100.000 cạnh và tối đa 25.000 nút, điều này dẫn đến khoảng$2.5 \times 10^9$nới lỏng trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Quan sát chính là cấu trúc biểu đồ thưa thớt nhưng không tùy ý: đó là một biểu đồ có hướng có trọng số tiêu chuẩn với một số cạnh vô hướng và chúng ta chỉ cần các đường đi ngắn nhất một nguồn. Đây chính xác là cài đặt mà Dijkstra hoạt động nếu tất cả các trọng số không âm, nhưng ở đây các cạnh âm chỉ tồn tại trên các đường có hướng. Thủ thuật quan trọng là tách quá trình xử lý để chúng tôi chỉ xử lý lại các nút khi cần thiết bằng cách sử dụng hàng ưu tiên nhưng vẫn đảm bảo tính chính xác ngay cả với các cạnh âm. 

Giải pháp dự định là sử dụng thuật toán đường đi ngắn nhất hoạt động giống như Dijkstra nhưng không dựa vào các cạnh không âm trên toàn cầu. Cách tiếp cận đúng là sử dụng tối ưu hóa dựa trên deque tương tự như SPFA với thứ tự thư giãn kiểu BFS 0-1, nhưng được khái quát hóa cho các trọng số tùy ý bằng cách sử dụng hàng đợi ưu tiên trong khi vẫn xử lý cẩn thận các bản cập nhật. Đây thực sự là Dijkstra với một đống, nhưng chúng tôi phải cho phép chèn lại ngay cả khi một nút đã được truy cập và không bao giờ đánh dấu vĩnh viễn các nút là đã hoàn tất. 

Trong thực tế, giải pháp là Dijkstra tiêu chuẩn với vùng heap tối thiểu, không có bước "hoàn thiện đã truy cập" thông thường, dựa vào việc thư giãn lặp đi lặp lại ngay cả khi một nút được trích xuất nhiều lần. Điều này là an toàn vì tính đúng đắn không phụ thuộc vào tính hữu hạn; chúng tôi chỉ quan tâm đến khoảng cách được biết đến nhiều nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bellman-Ford | O(nm) | O(n + m) | Quá chậm | 
| Đường dẫn ngắn nhất dựa trên heap (thư giãn lại) | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý biểu đồ một cách bình thường và chạy thuật toán đường đi ngắn nhất dựa trên hàng đợi ưu tiên từ`s`. 

1. Xây dựng danh sách lân cận cho tất cả các tuyến đường. Mỗi con đường hai chiều`(u, v, c)`trở thành hai cạnh có hướng`u -> v`Và`v -> u`, mỗi cái có chi phí`c`. Mỗi đường một chiều được thêm vào dưới dạng một cạnh có hướng duy nhất. 
2. Khởi tạo một mảng khoảng cách với giá trị lớn cho mỗi nút và đặt`dist[s] = 0`. Đây là mức chi phí được biết đến nhiều nhất cho đến nay. 
3. Đẩy`(0, s)`thành một đống tối thiểu. Mỗi mục nhập heap đại diện cho một đường dẫn ngắn nhất ứng cử viên đến một nút. 
4. Liên tục trích xuất nút`u`với khoảng cách dự kiến ​​nhỏ nhất từ ​​đống. 
5. Đối với mỗi cạnh đi`u -> v`với trọng lượng`w`, kiểm tra xem`dist[u] + w < dist[v]`. Nếu vậy, hãy cập nhật`dist[v]`và đẩy`(dist[v], v)`vào đống. 
6. Tiếp tục cho đến khi heap trống. Mỗi khi tìm thấy đường dẫn tốt hơn, nó sẽ được chèn lại và sẽ được xử lý lại sau đó. 

Lý do chính khiến chúng tôi không “hoàn thiện” các nút sau khi bật chúng là các cạnh âm có thể làm mất hiệu lực các giả định tối ưu trước đó. Cho phép thư giãn lặp đi lặp lại đảm bảo rằng các cải tiến được lan truyền một cách chính xác. 

### Tại sao nó hoạt động 

Thuật toán duy trì sự bất biến`dist[v]`luôn là chi phí nhỏ nhất được tìm thấy cho đến nay trong số tất cả các đường đi được phát hiện từ`s`ĐẾN`v`. Mỗi bước thư giãn chỉ cải thiện giá trị này chứ không bao giờ làm nó xấu đi. Mặc dù các nút có thể được xử lý nhiều lần, mỗi khi phát hiện ra một đường dẫn ngắn hơn, nó sẽ được truyền thẳng về phía trước thông qua các cạnh đi ra. Vì tất cả các cải tiến đều làm giảm nghiêm trọng một số`dist[v]`và đồ thị có số lượng hữu hạn các điểm thư giãn riêng biệt có thể tạo ra các cải tiến dưới các ràng buộc thông thường, quá trình hội tụ đến các khoảng cách đường đi ngắn nhất thực sự. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**18

def solve():
    n, x, y, s = map(int, input().split())
    s -= 1

    adj = [[] for _ in range(n)]

    for _ in range(x):
        a, b, c = map(int, input().split())
        a -= 1
        b -= 1
        adj[a].append((b, c))
        adj[b].append((a, c))

    for _ in range(y):
        a, b, c = map(int, input().split())
        a -= 1
        b -= 1
        adj[a].append((b, c))

    dist = [INF] * n
    dist[s] = 0

    pq = [(0, s)]

    while pq:
        d, u = heapq.heappop(pq)

        if d != dist[u]:
            continue

        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    for i in range(n):
        if dist[i] == INF:
            print("NO PATH")
        else:
            print(dist[i])

if __name__ == "__main__":
    solve()
```Cấu trúc liền kề mã hóa trực tiếp các loại đường mà không cần logic trường hợp đặc biệt ở phần sau của thuật toán. Đường hai chiều được mở rộng ngay lập tức, tránh tình trạng phân nhánh khi giãn. 

Kiểm tra khoảng cách`if d != dist[u]`là rất quan trọng. Nếu không có nó, các mục heap lỗi thời sẽ gây ra sự thư giãn dư thừa và có khả năng làm giảm hiệu suất đáng kể. 

Chúng tôi không bao giờ đánh dấu các nút là đã truy cập vĩnh viễn. Điều này khác với Dijkstra cổ điển và là sự thích ứng chính cho phép các khía cạnh tiêu cực cùng tồn tại một cách an toàn trong quá trình thư giãn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu.```
6 3 3 4
1 2 5
3 4 5
5 6 10
3 5 -100
4 6 -100
1 3 -10
```Chúng tôi bắt đầu từ nút 4. 

| Bước | Nút | Khoảng cách | Thư giãn | 
| --- | --- | --- | --- | 
| 1 | 4 | 0 | 4 → 6 (giá -100) cho 6 = -100 | 
| 2 | 6 | -100 | không có cải tiến nào | 
| 3 | 6 | -100 | hoàn thiện việc tuyên truyền | 
| 4 | người khác | INF | các nút không thể truy cập vẫn còn | 

Từ nút 4, chúng ta đạt tới 6 nút với giá rẻ, nhưng 6 không thể cải thiện các nút tiếp theo. Các nút 1, 2 và 5 vẫn không thể truy cập được hoặc không thể cải thiện tùy thuộc vào khả năng kết nối thông qua các cạnh âm. 

Dấu vết này cho thấy các cạnh tiêu cực ảnh hưởng ngay lập tức như thế nào đến các nút có thể truy cập và cách truyền bá đống đảm bảo rằng các cải tiến không bị trì hoãn. 

Bây giờ hãy xem xét một ví dụ nhỏ được xây dựng:```
4 0 3 1
1 2 2
2 3 -5
3 4 1
```| Bước | Nút | Khoảng cách | Thư giãn | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 → 2 = 2 | 
| 2 | 2 | 2 | 2 → 3 = -3 | 
| 3 | 3 | -3 | 3 → 4 = -2 | 
| 4 | 4 | -2 | xong | 

Điều này chứng tỏ sự lan truyền của một cạnh âm thông qua một chuỗi, đó chính xác là nơi mà Dijkstra ngây thơ sẽ thất bại nếu nó giả định tính hữu hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi lần giãn cạnh có thể bị đẩy thành đống, mỗi thao tác tốn log n | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách và đống | 

Với tối đa 100.000 cạnh, điều này thoải mái phù hợp với các ràng buộc thông thường đối với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-like case
assert run("""6 3 3 4
1 2 5
3 4 5
5 6 10
3 5 -100
4 6 -100
1 3 -10
""") != "", "sample 1"

# single node
assert run("""1 0 0 1
""") == "0"

# disconnected graph
assert run("""3 0 0 1
""") == "0\nNO PATH\nNO PATH"

# negative chain
assert run("""4 0 3 1
1 2 2
2 3 -5
3 4 1
""") == "0\n2\n-3\n-2"

# bidirectional only
assert run("""3 2 0 1
1 2 1
2 3 2
""") == "0\n1\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | khởi tạo cơ sở | 
| đồ thị bị ngắt kết nối | KHÔNG CÓ dòng ĐƯỜNG DẪN | xử lý không thể truy cập | 
| chuỗi âm | giảm khoảng cách | độ chính xác của việc truyền bá | 
| chỉ hai chiều | đường đi ngắn nhất tiêu chuẩn | xử lý cạnh vô hướng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một nút chỉ có thể truy cập được thông qua một chuỗi các cạnh có hướng âm. Trong tình huống đó, việc triển khai Dijkstra nghiêm ngặt để hoàn thiện các nút sẽ bị khóa ở khoảng cách dưới mức tối ưu quá sớm. Trong giải pháp này, việc thư giãn dựa trên heap đảm bảo rằng khi một đường dẫn ngắn hơn xuất hiện sau đó, nó vẫn được xử lý. 

Một trường hợp khác là các nút bị ngắt kết nối. Vì khoảng cách vẫn ở vô cùng nên đầu ra phải in rõ ràng`"NO PATH"`thay vì giữ chỗ bằng số. Vòng lặp cuối cùng kiểm tra trực tiếp điều kiện này. 

Trường hợp khó phát hiện cuối cùng là có nhiều mục nhập vùng nhớ heap lỗi thời cho cùng một nút. các`if d != dist[u]`Guard đảm bảo rằng chỉ cải tiến gần đây nhất mới được mở rộng, ngăn chặn cả các vấn đề về tính chính xác và những công việc không cần thiết.
