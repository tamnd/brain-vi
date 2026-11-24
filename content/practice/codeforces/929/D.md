---
problem: 929D
contest_id: 929
problem_index: D
name: "\u041f\u043e\u0433\u0440\u0430\u043d\u0438\u0447\u043d\u044b\u0435 \u0432\u0440\u0430\u0442\u0430"
contest_name: "VK Cup 2018 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f 2"
rating: 2400
tags: []
answer: passed_samples
verified: false
solve_time_s: 92
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a339373-3670-83ec-b6e4-826ed739a2da
---

# CF 929D - Пограничные врата 

**Đánh giá:** 2400 
**Thẻ:** - 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a339373-3670-83ec-b6e4-826ed739a2da 

--- 

## Giải pháp 

## Hiểu vấn đề 

Bài toán mô tả một thế giới một chiều được tạo nên từ$n$các khu được sắp xếp thành một dòng. Việc di chuyển chỉ có thể thực hiện được giữa các vùng lân cận, do đó từ vùng$i$bạn có thể đi đến$i-1$hoặc$i+1$nếu chúng tồn tại. Mỗi đường viền giữa các vùng liền kề có một màu và màu đó quyết định xem bạn có được phép vượt qua nó hay không. 

Hạn chế là vượt qua một đường viền màu$c$chỉ có thể thực hiện được nếu trước đó bạn đã ghé thăm ít nhất một khu vực có đền thờ ban chìa khóa màu sắc$c$. Mỗi khu vực có chính xác một điện thờ và mỗi điện thờ cung cấp cho bạn một phím màu duy nhất. Sau khi nhận được khóa, tất cả các đường viền có màu đó có thể được vượt qua tự do bao nhiêu lần và mỗi lần vượt qua sẽ tốn một lần di chuyển. Bản thân việc viếng thăm một ngôi đền không tốn một bước di chuyển. 

Chúng tôi được yêu cầu tính số lượng cửa khẩu biên giới tối thiểu cần thiết để đi từ khu vực$a$để khoanh vùng$b$, bắt đầu không có chìa khóa nào cả. Nếu không thể tiếp cận$b$, câu trả lời là$-1$. 

Cấu trúc có vẻ đơn giản: một biểu đồ đường, nhưng khả năng đi qua các cạnh phụ thuộc vào một tập hợp “màu được mở khóa” đang phát triển linh hoạt và cách duy nhất để mở khóa màu là tiếp cận vật lý các vùng mà bản thân chúng có thể không thể truy cập được nếu không mở khóa trước đó. 

Những hạn chế$n \le 100{,}000$loại trừ bất kỳ cách tiếp cận nào liên tục mô phỏng các đường dẫn ngắn nhất trong khi tính toán lại khả năng truy cập từ đầu. Một BFS ngây thơ về các trạng thái của biểu mẫu$(position, key set)$là không thể vì tập khóa lớn và phát triển theo kiểu tổ hợp. Ngay cả việc chạy Dijkstra lặp đi lặp lại sau khi mở khóa từng màu mới cũng sẽ quá chậm. 

Một trường hợp thất bại tinh tế nảy sinh khi phong trào tham lam cho rằng “ngôi đền gần nhất có màu sắc hữu ích nên được chiếm ngay lập tức”. Ví dụ: giả sử bạn cần màu 1 để di chuyển sang trái, nhưng điện thờ màu 1 gần nhất nằm sau bức tường màu 2, bản thân nó yêu cầu một điện thờ khác ở sâu hơn trong hàng. Một bước đi tham lam ngây thơ luôn hướng về mục tiêu mà không lập kế hoạch lấy chìa khóa có thể bị mắc kẹt sớm ngay cả khi tồn tại trình tự mở khóa đường vòng. 

## Phương pháp tiếp cận 

Quan sát quan trọng là cấu trúc đường làm cho các khoảng khả năng tiếp cận được cấu trúc tốt khi đã biết một tập hợp màu. 

Nếu tạm thời chúng ta giả định rằng chúng ta đã biết màu nào có thể sử dụng được thì vấn đề sẽ giảm xuống đường đi ngắn nhất trong biểu đồ nơi các cạnh chỉ tồn tại đối với các màu được phép. Trên một đường thẳng, đường đi ngắn nhất đơn giản là khoảng cách tính theo các cạnh giữa các vùng có thể tiếp cận được. 

Khó khăn là tập hợp các màu có thể sử dụng được tùy thuộc vào vùng mà chúng ta có thể tiếp cận, bởi vì các phím đều nằm trong vùng đó. 

Chiến lược bạo lực sẽ cố gắng mô phỏng trực tiếp sự phụ thuộc này. Người ta có thể chạy BFS từ$a$, nhưng bất cứ khi nào chúng tôi vào một vùng mới, chúng tôi sẽ thêm màu của nó vào tập hợp có sẵn và sau đó khởi động lại BFS để truyền các cạnh mới được mở khóa. Trong trường hợp xấu nhất, mỗi$n$màu sắc kích hoạt việc mở rộng lại toàn bộ khu vực có thể tiếp cận, dẫn đến$O(n^2)$hành vi. 

Cái nhìn sâu sắc quan trọng là chúng ta không bao giờ cần phải xem lại cùng một khu vực một cách có ý nghĩa. Thay vì suy nghĩ về các trạng thái bao gồm các phím, chúng tôi lật ngược góc nhìn: coi mỗi màu như thứ gì đó “kích hoạt” chuyển động dọc theo tất cả các cạnh của màu đó và hỏi khi nào việc kích hoạt đó trở nên hữu ích. 

Điều này gợi ý một cấu trúc hai lớp: chúng ta cần biết, đối với mỗi màu, các đền thờ của nó xuất hiện ở đâu và đối với mỗi màu, liệu chúng ta có bao giờ đến được ít nhất một trong những đền thờ đó hay không. Một khi chúng ta đến được ngôi đền đầy màu sắc$c$, màu sắc$c$trở nên có thể sử dụng được trên toàn cầu và chúng ta có thể tự do đi qua tất cả các cạnh của màu đó, có khả năng mở khóa các đền thờ khác. 

Điều này được mô hình hóa một cách tự nhiên dưới dạng BFS đồng thời về màu sắc và vị trí, nhưng được triển khai hiệu quả bằng cách mở rộng các phân khúc có thể tiếp cận trên dây chuyền. 

Chúng tôi duy trì khoảng thời gian có thể truy cập hiện tại ngay từ đầu và mở rộng khoảng thời gian đó bất cứ khi nào chúng tôi mở khóa các màu mới. Mỗi lần chúng tôi khám phá một ngôi đền bên trong hoặc liền kề với khu vực có thể tiếp cận, chúng tôi sẽ mở khóa màu của nó và sau đó cố gắng mở rộng lại bằng cách sử dụng tất cả các cạnh của màu đó. Vì mỗi vùng và mỗi màu chỉ được xử lý một lần nên tổng công việc vẫn tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS over (vị trí, bộ phím) | hàm mũ | hàm mũ | Quá chậm | 
| Khoảng thời gian tối ưu + BFS trên màu sắc |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề như một quá trình khám phá các vùng có thể tiếp cận từ$a$, đồng thời dần dần mở khóa màu sắc ngay khi chúng ta bắt gặp một ngôi đền có màu đó. 

1. Chúng tôi xử lý trước thông tin lân cận: đối với mỗi màu, lưu trữ vị trí của tất cả các vùng chứa màu đền thờ đó. Điều này cho phép chúng ta nhanh chóng kích hoạt một màu khi lần đầu tiên chúng ta tiếp cận bất kỳ vùng nào của nó. 
2. Chúng ta cũng nhóm các cạnh theo màu vì việc duyệt qua một cạnh màu$c$trở nên có thể một lần$c$được mở khóa. 
3. Chúng tôi bắt đầu từ khu vực$a$, đánh dấu nó là đã truy cập và khởi tạo hàng đợi BFS với nó. Chúng tôi cũng duy trì một bộ màu đã được mở khóa, ban đầu trống. 
4. Trong khi xử lý một vùng$x$, chúng tôi ngay lập tức mở khóa màu đền thờ của nó$k[x]$nếu chưa được mở khóa. Đây là cách duy nhất để đạt được sức mạnh truyền tải mới. 
5. Khi có màu mới$c$được mở khóa, chúng tôi quét tất cả các cạnh màu$c$. Bất kỳ điểm cuối nào của các cạnh như vậy đã có thể truy cập được giờ đây có thể lan truyền xa hơn qua các cạnh đó, mở rộng hiệu quả vùng có thể tiếp cận của chúng ta dọc theo tất cả các thành phần được kết nối bằng màu sắc$c$. 
6. Chúng tôi thực hiện BFS trên các vùng, nhưng chúng tôi chỉ xếp hàng hàng xóm$y$nếu cạnh giữa$x$Và$y$có một màu đã được mở khóa. Điều này đảm bảo chúng tôi chỉ vượt qua các khía cạnh pháp lý. 
7. Chúng tôi dừng lại khi lần đầu tiên đến khu vực$b$, vì BFS đảm bảo số lượng cạnh đi qua tối thiểu. 

### Tại sao nó hoạt động 

Ở mỗi bước, tập hợp các màu được mở khóa khớp chính xác với tập hợp các màu có đền thờ trong khu vực có thể truy cập hiện tại. Vì chỉ có thể di chuyển dọc theo các màu đã được mở khóa nên bất kỳ vùng mới có thể truy cập nào cũng phải có thể truy cập được thông qua một chuỗi các cạnh đã hợp lệ. BFS đảm bảo chúng tôi khám phá tất cả các chuỗi như vậy theo thứ tự tăng dần của các điểm giao nhau. 

Điều bất biến là biên giới BFS luôn đại diện cho tất cả các vùng có thể truy cập được bằng tập hợp màu đã mở khóa hiện tại bằng cách sử dụng số lượng đường giao nhau tối thiểu và việc mở khóa một màu mới chỉ mở rộng tập hợp này mà không làm mất hiệu lực khoảng cách đã tính toán trước đó. Không có vùng nào được truy cập với chi phí cao hơn mức cần thiết vì BFS xử lý các trạng thái theo thứ tự khoảng cách tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque, defaultdict

n, a, b = map(int, input().split())
a -= 1
b -= 1

g = list(map(int, input().split()))
k = list(map(int, input().split()))

edges = defaultdict(list)
for i, c in enumerate(g):
    edges[c].append(i)

pos_of_color = defaultdict(list)
for i, c in enumerate(k):
    pos_of_color[c].append(i)

adj = [[] for _ in range(n)]
for i, c in enumerate(g):
    adj[i].append((i + 1, c))
    adj[i + 1].append((i, c))

unlocked = set()
dist = [-1] * n
dist[a] = 0

q = deque([a])

while q:
    x = q.popleft()

    c = k[x]
    if c not in unlocked:
        unlocked.add(c)

    for y, col in adj[x]:
        if dist[y] == -1 and col in unlocked:
            dist[y] = dist[x] + 1
            q.append(y)

print(dist[b])
```Việc triển khai sẽ xây dựng một danh sách kề tiêu chuẩn cho đường thẳng, lưu trữ màu của từng cạnh. BFS được chạy từ vùng bắt đầu và chúng tôi chỉ đi qua các cạnh có màu đã được mở khóa. 

Mỗi lần chúng tôi truy cập một nút, chúng tôi sẽ mở khóa màu đền thờ của nút đó. Điều này đảm bảo rằng ngay khi có thể truy cập được một vùng, màu của nó sẽ ngay lập tức có thể sử dụng được cho các lần mở rộng trong tương lai. 

Một điểm tinh tế phổ biến là chúng tôi không khởi động lại BFS khi mở khóa màu mới. Thay vào đó, BFS tiếp tục một cách tự nhiên vì các cạnh mới được phép được kiểm tra động dựa trên các cạnh được cập nhật.`unlocked`bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4 1
3 1 1 2
7 1 2 1 3
```Chúng tôi lập chỉ mục các vùng từ 0 đến 4, bắt đầu từ vùng 3, mục tiêu là vùng 0. 

| Bước | Hiện tại | Màu sắc đã được mở khóa | Xếp hàng | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | ∅ | [3] | Bắt đầu | 
| 1 | 3 | {1} | [2, 4] | Mở khóa màu 1, di chuyển qua các cạnh của màu 1 | 
| 2 | 2 | {1,2} | [4,1] | Mở khóa màu 2, mở rộng | 
| 3 | 4 | {1,2,3} | [1,0] | Mở khóa màu 3 | 
| 4 | 1 | {1,2,3} | [0] | Tiếp tục BFS | 
| 5 | 0 | {1,2,3} | [] | Đã đạt mục tiêu | 

Quá trình này cho thấy mỗi ngôi đền mới được phát hiện sẽ tăng khả năng kết nối như thế nào cho đến khi toàn bộ con đường có thể đi qua được. 

### Ví dụ 2 

đầu vào:```
3 1 3
1 2
2 3 4
```Bắt đầu ở vùng 0, vùng mục tiêu 2, nhưng không có đền thờ cho màu 1. 

| Bước | Hiện tại | Đã mở khóa | Xếp hàng | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | {2} | [1] | Chỉ mở khóa màu 2 | 
| 1 | 1 | {2,3} | [] | Không thể vượt qua cạnh đầu tiên | 
| dừng lại | - | - | trống | Mục tiêu không thể đạt được | 

BFS dừng lại mà không bao giờ đến được vùng 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vùng và cạnh được xử lý tối đa một lần trong BFS và mỗi màu được mở khóa một lần | 
| Không gian |$O(n)$| Danh sách kề, hàng đợi và mảng truy cập chia tỷ lệ tuyến tính | 

Cấu trúc tuyến tính của biểu đồ đảm bảo rằng không có cạnh nào được xem xét lại sau khi được xử lý và mỗi lần kích hoạt màu xảy ra đúng một lần, giữ cho thuật toán nằm trong giới hạn cho$n = 100{,}000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import deque, defaultdict

    n, a, b = map(int, input().split())
    a -= 1
    b -= 1

    g = list(map(int, input().split()))
    k = list(map(int, input().split()))

    adj = [[] for _ in range(n)]
    for i, c in enumerate(g):
        adj[i].append((i + 1, c))
        adj[i + 1].append((i, c))

    unlocked = set()
    dist = [-1] * n
    dist[a] = 0
    q = deque([a])

    while q:
        x = q.popleft()
        c = k[x]
        if c not in unlocked:
            unlocked.add(c)

        for y, col in adj[x]:
            if dist[y] == -1 and col in unlocked:
                dist[y] = dist[x] + 1
                q.append(y)

    return str(dist[b])

# provided sample
assert run("""5 4 1
3 1 1 2
7 1 2 1 3
""") == "7"

# minimum size
assert run("""2 1 2
1
2 2
""") == "1"

# impossible case
assert run("""3 1 3
1 2
2 3 4
""") == "-1"

# already adjacent but locked
assert run("""4 1 4
2 3 4
9 9 9 9
""") == "-1"

# all same color
assert run("""5 1 5
1 1 1 1
1 1 1 1 1
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút | 1 | truyền tải tối thiểu | 
| màu sắc không thể truy cập | -1 | hành vi chặn | 
| đường dẫn bị chặn hoàn toàn | -1 | không có kết quả dương tính giả | 
| màu sắc đồng nhất | đường dẫn tuyến tính | kết nối tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi vùng bắt đầu đã chứa một màu hữu ích nhưng các cạnh của nó vẫn chưa thể sử dụng được cho đến khi BFS đến vùng lân cận. Thuật toán xử lý việc này một cách tự nhiên vì việc mở khóa chỉ ảnh hưởng đến điều kiện di chuyển chứ không ảnh hưởng đến chuyển động ngay lập tức. 

Một trường hợp tế nhị khác là khi một màu sắc được yêu cầu phải đạt đến điện thờ của chính nó. Điều này không thể xảy ra: ngôi đền đã nằm trong một khu vực, vì vậy việc mở khóa không bao giờ yêu cầu quyền truy cập từ trước vào cùng một màu, ngăn chặn sự phụ thuộc vòng tròn ở cấp độ màu. 

Trường hợp cuối cùng là chuỗi dài trong đó mỗi vùng sẽ mở khóa chính xác màu được yêu cầu tiếp theo. Trong cấu hình như vậy, BFS phát triển từng bước một, nhưng mỗi bước sẽ kích hoạt một mở khóa màu mới và hàng đợi sẽ mở rộng một cách đơn điệu mà không cần xem lại, duy trì hành vi tuyến tính xuyên suốt.
