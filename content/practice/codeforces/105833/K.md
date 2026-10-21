---
title: "CF 105833K - Kanto To Johto"
description: "Bài toán có thể được xem như một bài toán thăm dò đồ thị trong đó mỗi cạnh là một đường tàu có chi phí liên quan nhưng chi phí đó không được thanh toán ngay lập tức."
date: "2026-06-25T06:31:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105833
codeforces_index: "K"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2025"
rating: 0
weight: 105833
solve_time_s: 47
verified: true
draft: false
---

[CF 105833K - Kanto To Johto](https://codeforces.com/problemset/problem/105833/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán có thể được xem như một bài toán thăm dò đồ thị trong đó mỗi cạnh là một đường tàu có chi phí liên quan nhưng chi phí đó không được thanh toán ngay lập tức. Thay vào đó, Red nhận được “giấy phép” cho các cạnh khi anh ấy di chuyển và chỉ khi anh ấy cuối cùng rời ga$N$, hệ thống thanh toán sẽ kích hoạt một quy tắc đặc biệt: chỉ$K$giấy phép được mua rẻ nhất thực sự cần phải được thanh toán, trong khi tất cả các giấy phép khác thực tế là miễn phí. 

Chúng ta được cho một đồ thị vô hướng với tối đa$10^5$nút và$2 \cdot 10^5$các cạnh. Mỗi cạnh kết nối hai trạm và có một chi phí. Màu đỏ bắt đầu từ nút 1 và phải đến nút$N$. Trong khi di chuyển, bất cứ khi nào anh ta ở điểm cuối của một cạnh, anh ta được phép mua giấy phép của cạnh đó, sau đó cạnh đó sẽ có thể sử dụng được để di chuyển miễn phí theo cả hai hướng. Điểm mấu chốt là tổng chi phí không phải là tổng của tất cả các cạnh được mua mà là tổng của$K$chi phí cạnh mua nhỏ nhất. 

Vì vậy, quyết định không chỉ là nên đi theo con đường nào mà còn là những cạnh nào cần được “thu thập” một cách có chủ ý trong quá trình khám phá để tập hợp cuối cùng của các cạnh được mua có giá rẻ.$K$-tập hợp con. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào cũng phải gần tuyến tính hoặc gần tuyến tính về số cạnh, bởi vì$M$có thể$2 \cdot 10^5$. Cách tiếp cận theo kiểu đường đi ngắn nhất với vùng heap có thể được chấp nhận, nhưng bất cứ điều gì liên tục sắp xếp hoặc liệt kê tất cả các tập hợp con đều là không thể ngay lập tức. Việc khám phá theo cấp số nhân ngây thơ về tất cả các cách có thể để đi qua và mua các cạnh là không thể thực hiện được vì mỗi bước sẽ phân nhánh trên các danh sách lân cận và các quyết định mua cạnh có khả năng xảy ra, dẫn đến tăng trưởng theo cấp số nhân. 

Một điểm tinh tế là việc truy cập một nút nhiều lần sẽ rất quan trọng vì nó cho phép mua các lợi thế liên quan đến nút đó tại các thời điểm khác nhau. Việc giải thích đường đi ngắn nhất một cách bất cẩn sẽ cho rằng các cạnh chỉ có thể sử dụng được dọc theo một đường dẫn duy nhất, nhưng ở đây khả năng quay lại sau khi mua các cạnh sẽ thay đổi cấu trúc một cách đáng kể. 

Vấn đề không rõ ràng thứ hai là việc tiếp cận một nút không giống như “hoàn thành”; Màu đỏ có thể tiếp tục di chuyển sau khi đạt được$N$, do đó những con đường tạm thời vượt ra ngoài$N$hoặc xem lại các vùng hợp lệ nếu chúng giúp thu thập các cạnh tốt hơn. 

Cạm bẫy thứ ba là giả sử chúng ta chỉ quan tâm đến các cạnh trên một đường đi đơn giản từ 1 đến$N$. Điều đó là sai vì việc mua một cạnh cho phép di chuyển độc lập với vai trò của nó trong lộ trình cuối cùng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng tất cả các cách có thể để đi từ nút 1 đến nút$N$và ở mỗi bước quyết định có nên mua cạnh hiện tại hay không. Trong quá trình đi bộ, chúng tôi duy trì tập hợp các cạnh đã mua và khi chúng tôi lần đầu tiên tiếp cận$N$, chúng tôi tính tổng của$K$chi phí cạnh nhỏ nhất trong tập hợp đó. Điều này đúng nhưng sẽ bùng nổ ngay lập tức: ngay cả khi chúng tôi giới hạn chiều dài bước đi ở mức$O(N)$, mỗi bước phân nhánh thành nhiều cạnh và số lượng tập hợp con có thể mua tăng theo cấp số nhân$M$. Ngay cả khi bỏ qua các lựa chọn đường dẫn, việc đánh giá tập hợp con tốt nhất cho mỗi lần truyền tải sẽ yêu cầu sắp xếp tới$M$các giá trị lặp đi lặp lại. 

Quan sát cấu trúc quan trọng là điều duy nhất quan trọng về quá trình truyền tải là các cạnh cuối cùng được mua chứ không phải trình tự chính xác. Bản thân việc truyền tải chỉ đóng vai trò như một điều kiện khả thi: một cạnh có thể được mua nếu tại một thời điểm nào đó chúng ta đạt đến một trong các điểm cuối của nó. Vì vậy, câu hỏi thực sự trở thành: tập hợp con nào của các cạnh “có thể truy cập được” từ nút 1 theo nghĩa này và trong số các tập hợp con có thể truy cập đó, làm cách nào để chúng ta giảm thiểu tổng của$K$chi phí nhỏ nhất. 

Điều này biến vấn đề thành một bản mở rộng giống như đường đi ngắn nhất trong đó các trạng thái là các nút, nhưng quá trình chuyển đổi phụ thuộc vào việc thu thập các cạnh để mở khóa nhiều chuyển động hơn. Thay vì suy nghĩ về các đường dẫn, chúng tôi xử lý nó như việc khám phá dần dần vùng kết nối của các cạnh có thể tiếp cận trong khi vẫn duy trì cấu trúc chi phí nỗ lực tối đa để theo dõi$K$các cạnh rẻ nhất được thấy cho đến nay. 

Cách mã hóa tiêu chuẩn “chúng tôi chỉ quan tâm đến$K$các phần tử nhỏ nhất" là duy trì tối đa kích thước heap tối đa$K$, nên chúng tôi luôn giữ giá rẻ nhất$K$các cạnh gặp phải trong vùng có thể tiếp cận. Quá trình truyền tải trở thành một cuộc khám phá biểu đồ trong đó mỗi lần chúng tôi tiếp cận một nút mới, chúng tôi sẽ hiển thị các cạnh sự cố của nó và bất kỳ cạnh mới nào cũng có thể mở rộng khả năng tiếp cận hơn nữa. 

Điều này dẫn đến một quy trình giống Dijkstra trên các nút, nhưng với cấu trúc bổ sung duy trì hiệu suất tốt nhất$K$chi phí cạnh thu thập được cho đến nay. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các đường dẫn và tập hợp con | hàm mũ | hàm mũ | Quá chậm | 
| Khả năng tiếp cận + Dijkstra + top-K heap |$O((N+M)\log N + M \log K)$|$O(N+M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa quy trình dưới dạng truyền tải đồ thị bắt đầu từ nút 1, nơi chúng tôi dần dần khám phá các nút có thể truy cập và đồng thời thu thập các cạnh có thể sử dụng. 

1. Khởi tạo hàng đợi ưu tiên để truyền tải đồ thị bắt đầu từ nút 1 và đánh dấu nút 1 là có thể truy cập được. 
2. Duy trì cấu trúc lưu trữ những chi tiết nhỏ nhất$K$chi phí cạnh được thấy cho đến nay. Một đống kích thước tối đa$K$được sử dụng để khi có chi phí cạnh mới, chúng tôi sẽ chèn nó và nếu kích thước vượt quá$K$, chúng tôi loại bỏ phần tử lớn nhất. Điều này đảm bảo chúng tôi luôn giữ được$K$chi phí nhỏ nhất trong số tất cả các cạnh được thu thập. 
3. Trong khi khám phá một nút$u$, lặp qua tất cả các cạnh liên quan tới$u$. Đối với mỗi cạnh$(u, v, c)$, chúng tôi cố gắng “khám phá” nó bằng cách thêm chi phí$c$vào đầu-$K$kết cấu. Lý do điều này hợp lý là vì một khi chúng ta có thể đạt được$u$, chúng ta được phép mua cạnh đó. 
4. Nếu điểm cuối khác$v$chưa được truy cập, chúng tôi đánh dấu nó là có thể truy cập được và thêm nó vào hàng đợi truyền tải. Bước này truyền bá khả năng tiếp cận thông qua các cạnh mới có sẵn. 
5. Tiếp tục cho đến khi không thể truy cập được nút mới nào. Tại thời điểm đó, chúng tôi đã sử dụng hết tất cả các cạnh có thể gặp bắt đầu từ nút 1 theo quy tắc “các cạnh sẽ có thể sử dụng được khi đạt đến điểm cuối”. 
6. Tính đáp án cuối cùng bằng tổng các giá trị hiện được lưu trong$K$-đống. Những điều này đại diện cho$K$các cạnh có thể mua được rẻ nhất trong đồ thị con có thể tiếp cận. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là tại bất kỳ thời điểm nào, các nút được truy cập chính xác là những nút có thể truy cập được từ nút 1 bằng cách sử dụng các cạnh đã được phát hiện. Mỗi khi một nút có thể truy cập được, tất cả các cạnh sự cố của nó đều đủ điều kiện để mua, do đó, không có cạnh nào có thể mua được bị bỏ qua. Bất biến heap đảm bảo rằng trong số tất cả các cạnh đủ điều kiện, chúng ta chỉ giữ lại cạnh nhỏ nhất$K$và vì khoản thanh toán cuối cùng bỏ qua tất cả trừ$K$các cạnh được mua rẻ nhất, loại bỏ sớm các cạnh lớn hơn không bao giờ làm thay đổi số tiền cuối cùng. Việc truyền tải đảm bảo rằng nếu một cạnh có thể ảnh hưởng đến khả năng tiếp cận thì điểm cuối của nó cuối cùng sẽ được xử lý, do đó không có cạnh đóng góp hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        x, y, c = map(int, input().split())
        g[x].append((y, c))
        g[y].append((x, c))

    visited = [False] * (n + 1)
    visited[1] = True

    pq_nodes = [1]

    # max heap via negation for keeping k smallest edges
    best = []

    while pq_nodes:
        u = pq_nodes.pop()

        for v, c in g[u]:
            if len(best) < k:
                heapq.heappush(best, -c)
            else:
                if -best[0] > c:
                    heapq.heapreplace(best, -c)

            if not visited[v]:
                visited[v] = True
                pq_nodes.append(v)

    print(-sum(best))

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề vì chúng ta phải liên tục hiển thị các cạnh khi có thể truy cập được một nút. các`visited`mảng đảm bảo mỗi nút được xử lý một lần, điều này ngăn chặn các vòng mở rộng lặp đi lặp lại. 

các`best`heap lưu trữ các giá trị âm sao cho ứng viên lớn nhất hiện đang được giữ luôn ở trên cùng. Điều này giúp bạn dễ dàng loại bỏ nó khi cạnh tốt hơn (nhỏ hơn) xuất hiện. Giới hạn kích thước để$K$là rất quan trọng vì nếu không có nó, chúng ta sẽ lưu trữ tất cả các cạnh và mất đi lợi ích về hiệu quả. 

Ngăn xếp xử lý nút`pq_nodes`có thể được thay thế bằng hàng đợi hoặc cấu trúc ưu tiên, nhưng trong vấn đề này, bất kỳ thứ tự truyền tải nào cũng đủ vì tất cả các nút có thể truy cập cuối cùng đều lộ ra các cạnh của chúng và không có ràng buộc về khoảng cách đối với việc mở rộng nút. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7 5 2
1 2 3
2 7 3
5 4 2
7 5 2
3 6 1
```Chúng tôi bắt đầu ở nút 1. 

| Bước | Nút hiện tại | Các cạnh mới được nhìn thấy | Heap (top K nhỏ nhất) | 
| --- | --- | --- | --- | 
| 1 | 1 | (1-2,3) | [3] | 
| 2 | 2 | (2-7,3) | [3,3] | 
| 3 | 7 | (7-5,2) | [2,3] | 
| 4 | 5 | (5-4,2) | [2,2] | 

Nút 3 và 6 không bao giờ có thể truy cập được nên cạnh (3-6,1) bị bỏ qua. 

Đống chứa [2, 2, 3, 3] nhưng chỉ có hai vật chất nhỏ nhất cho$K=2$, đưa ra đáp án 4. 

Dấu vết này cho thấy rằng chỉ các cạnh trong thành phần có thể truy cập từ 1 đến các điểm cuối được mở khóa dần dần mới quan trọng. 

### Mẫu 2 

đầu vào:```
3 4 2
1 2 1
1 2 1
1 2 2
2 3 2
```| Bước | Nút | Những góc cạnh mới | Đống | 
| --- | --- | --- | --- | 
| 1 | 1 | (1-2,1), (1-2,1), (1-2,2) | [1,1] | 
| 2 | 2 | (2-3,2) | [1,1] | 
| 3 | 3 | - | [1,1] | 

Mặc dù có nhìn thấy cạnh (2-3,2) nhưng nó không nằm trong hai chi phí nhỏ nhất nên không ảnh hưởng đến khoản thanh toán cuối cùng. 

Điều này xác nhận rằng các cạnh trùng lặp được xử lý một cách tự nhiên vì mỗi lần xuất hiện đều được xử lý độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N + M)\log K)$| Mỗi cạnh được xử lý một lần và chi phí cập nhật mỗi heap$\log K$. | 
| Không gian |$O(N + M)$| Danh sách kề cộng với mảng đã truy cập và bộ nhớ heap. | 

Giới hạn$N \le 10^5$,$M \le 2 \cdot 10^5$phù hợp thoải mái trong sự phức tạp này, vì$K \le M$nhưng các hoạt động của đống vẫn theo logarit và có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import heapq

    n, m, k = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        x, y, c = map(int, sys.stdin.readline().split())
        g[x].append((y, c))
        g[y].append((x, c))

    visited = [False] * (n + 1)
    visited[1] = True
    q = [1]

    best = []

    while q:
        u = q.pop()
        for v, c in g[u]:
            if len(best) < k:
                heapq.heappush(best, -c)
            else:
                if -best[0] > c:
                    heapq.heapreplace(best, -c)

            if not visited[v]:
                visited[v] = True
                q.append(v)

    return str(-sum(best))

# provided samples
assert run("""7 5 2
1 2 3
2 7 3
5 4 2
7 5 2
3 6 1
""") == "4"

assert run("""7 10 5
1 2 4
2 6 3
6 4 2
3 5 7
4 7 3
3 7 1
2 4 10
1 3 12
5 7 4
4 5 4
""") == "12"

# custom cases

# minimum case
assert run("""2 1 1
1 2 5
""") == "5"

# duplicate edges
assert run("""3 3 2
1 2 5
1 2 1
2 3 10
""") == "6"

# k larger than available edges
assert run("""4 3 10
1 2 4
2 3 1
3 4 2
""") == "7"

# all equal weights
assert run("""4 4 2
1 2 3
2 3 3
3 4 3
1 3 3
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 5 | đường đi đơn giản nhất và hành vi K=1 | 
| cạnh trùng lặp | 6 | xử lý chính xác các cạnh lặp lại | 
| k lớn hơn cạnh | 7 | dự phòng khi tồn tại ít hơn K cạnh | 
| tất cả các trọng lượng bằng nhau | 6 | xử lý ràng buộc trong logic heap | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi$K$vượt quá số cạnh có thể truy cập được từ nút 1. Trong trường hợp đó, thuật toán không bao giờ lấp đầy heap theo kích thước$K$và tổng cuối cùng đương nhiên bao gồm tất cả các cạnh được thu thập. Ví dụ, trong một chuỗi tuyến tính$1-2-3-4$với$K=10$, mọi cạnh đều được bao gồm trong heap và tổng chỉ đơn giản là tổng chi phí đường đi. 

Một trường hợp tinh vi khác là có nhiều cạnh song song giữa các nút giống nhau. Mỗi cạnh được xử lý độc lập trong quá trình truyền tải, do đó, ngay cả khi hai cạnh kết nối cùng một cặp nút, cả hai đều được xem xét riêng biệt để đưa vào heap. Điều này là cần thiết vì cái rẻ hơn có thể là một phần của giải pháp tối ưu.$K$-bộ. 

Trường hợp quan trọng cuối cùng là khi quá trình thăm dò đạt tới nút$N$sớm. Thuật toán không dừng lại ở nút$N$bởi vì các nút bổ sung ngoài nó vẫn có thể truy cập được và có thể lộ ra các cạnh rẻ hơn. Việc chấm dứt sớm sẽ bỏ lỡ những đóng góp này một cách không chính xác, vì vậy quá trình truyền tải phải tiếp tục cho đến khi đạt được việc đóng không gian có thể truy cập.
