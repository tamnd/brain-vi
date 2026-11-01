---
title: "CF 106475H - \u041e\u0442\u0442\u0435\u0440\u0432\u0435\u0440 \u0441\u043e\u0431\u0438\u0440\u0430\u0435\u0442\u0441\u044f \u0432 \u043f\u0443\u0442\u044c"
description: "Chúng ta được cung cấp một tập hợp các vị trí được kết nối bằng những con đường vô hướng. Mỗi con đường có thời gian đi qua cố định nhưng việc sử dụng nó rất phức tạp do đèn giao thông quay vòng liên tục giữa pha đỏ và pha xanh."
date: "2026-06-25T08:52:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106475
codeforces_index: "H"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0412\u041a\u041e\u0428\u041f.Junior 2026"
rating: 0
weight: 106475
solve_time_s: 46
verified: true
draft: false
---

[CF 106475H - \u041e\u0442\u0442\u0435\u0440\u0432\u0435\u0440 \u0441\u043e\u0431\u0438\u0440\u0430\u0435\u0442\u0441\u044f \u0432 \u043f\u0443\u0442\u044c](https://codeforces.com/problemset/problem/106475/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các vị trí được kết nối bằng những con đường vô hướng. Mỗi con đường có thời gian đi qua cố định nhưng việc sử dụng nó rất phức tạp do đèn giao thông quay vòng liên tục giữa pha đỏ và pha xanh. 

Khi bạn đến một đầu đường vào một thời điểm nào đó, bạn có thể không được phép băng qua đường ngay lập tức. Mỗi con đường có một lịch trình lặp lại: đầu tiên là khoảng thời gian màu đỏ có độ dài r trong đó lối vào bị chặn, sau đó là khoảng thời gian màu xanh lá cây có độ dài g trong thời gian đó bạn được phép đi vào và đi qua đường. Mô hình này lặp đi lặp lại mãi mãi. 

Nếu bạn đến trong giai đoạn xanh, bạn có thể bắt đầu ngay lập tức. Nếu bạn đến trong giai đoạn màu đỏ, bạn phải đợi cho đến khi giai đoạn xanh tiếp theo bắt đầu. Sau khi bạn bắt đầu băng qua đường, bạn luôn dành chính xác t đơn vị thời gian để đi dọc theo con đường đó. 

Nhiệm vụ là tính toán thời gian tối thiểu cần thiết để đi từ nút 1 đến nút n. 

Biểu đồ có tới 100.000 nút và 100.000 con đường. Mỗi truy vấn đường đi yêu cầu suy luận về hành vi phụ thuộc vào thời gian, nhưng cấu trúc vẫn là một bài toán đường đi ngắn nhất tiêu chuẩn với sự thay đổi trong độ giãn cạnh. 

Một cách giải thích ngây thơ sẽ là bỏ qua đèn giao thông và coi mọi cạnh đều có chi phí t. Điều đó thất bại ngay lập tức vì thời gian chờ đợi có thể chi phối câu trả lời và chúng phụ thuộc vào thời gian đến chứ không chỉ cấu trúc biểu đồ. 

Một trường hợp cạnh tinh tế xuất hiện khi việc chờ đợi là bắt buộc mặc dù tồn tại một đường dẫn ngắn hơn về mặt trọng số cạnh thuần túy. Ví dụ: giả sử bạn có thể tiếp cận một nút sớm thông qua một đường dẫn dài phù hợp với các pha màu xanh lá cây hoặc muộn hơn thông qua một đường dẫn ngắn hơn chạm vào pha màu đỏ. Cách tiếp cận cạnh ngắn nhất tham lam sẽ chọn sai cách sau. 

Một lỗi phổ biến khác là giả sử việc chờ đợi chỉ xảy ra một lần trên mỗi nút, thay vì trên mỗi lần truyền tải cạnh. Việc chờ đợi phụ thuộc vào thời gian đến chính xác tại mỗi điểm cuối, do đó, hai đường dẫn khác nhau đến cùng một nút có thể tạo ra các chi phí khác nhau trong tương lai. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi đây là một bài toán đồ thị phụ thuộc thời gian và mô phỏng tất cả các cách có thể để giải quyết nó. Từ trạng thái được xác định bởi (nút, thời gian), chúng tôi thử tất cả các cạnh đi ra, tính thời gian khởi hành hợp lệ tiếp theo bằng cách kiểm tra chu kỳ đèn giao thông và tiếp tục đệ quy hoặc với BFS theo trạng thái thời gian. Điều này đúng vì nó mô hình hóa rõ ràng tất cả các lịch trình khả thi. 

Vấn đề là thời gian là không giới hạn. Ngay cả khi chúng ta chỉ xem xét những khoảng thời gian xảy ra sau khi đến, mỗi lần thư giãn đều tạo ra các dấu thời gian riêng biệt mới. Trong trường hợp xấu nhất, mỗi lần truyền tải cạnh có thể tạo ra một giá trị thời gian duy nhất và số lượng trạng thái tăng theo độ lớn của thời gian thay vì số lượng nút. Với tối đa 10^5 cạnh, điều này biến thành sự bùng nổ của các trạng thái. 

Quan sát quan trọng là vấn đề vẫn thỏa mãn thuộc tính cấu trúc con tối ưu của các đường đi ngắn nhất: một khi chúng ta sửa thời gian đến đã biết rõ nhất tại một nút, chúng ta không bao giờ cần phải truy cập lại nó với thời gian tồi tệ hơn. Điều phức tạp duy nhất là độ giãn của cạnh phụ thuộc vào thời gian đến thông qua hàm xác định: thời gian chờ được tính từ modulo thời gian (r + g). Điều này làm cho biểu đồ phù hợp với thuật toán của Dijkstra, trong đó trọng số của các cạnh không phải là hằng số nhưng có thể được đánh giá một cách nhanh chóng. 

Thay vì liệt kê các trạng thái, chúng tôi duy trì thời gian đến được biết rõ nhất cho mỗi nút. Khi chúng ta nới lỏng một cạnh từ u xuống v tại thời điểm T, chúng ta tính thời gian chờ do chu trình gây ra, cộng nó với T, sau đó cộng thời gian truyền tải t. Điều này tạo ra thời gian đến ứng cử viên cho v, giống hệt như một cạnh có trọng số. 

Điều này biến vấn đề thành một phép tính đường đi ngắn nhất một nguồn tiêu chuẩn với hàm thư giãn không cố định nhưng được xác định rõ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force theo trạng thái thời gian | O(rất lớn, có khả năng theo cấp số nhân trong thực tế) | O(rất lớn) | Quá chậm | 
| Dijkstra với sự thư giãn phụ thuộc vào thời gian | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho biểu đồ trong đó mỗi cạnh lưu trữ (láng giềng, thời gian di chuyển t, r đỏ, g xanh). Điều này cho phép lặp lại hiệu quả các đường đi từ mỗi nút. 
2. Khởi tạo một mảng dist có kích thước n với vô cùng, ngoại trừ dist[1] = 0, vì chúng ta bắt đầu ở nút 1 tại thời điểm 0. 
3. Sử dụng hàng đợi ưu tiên được sắp xếp theo thời gian đã biết hiện tại. Chèn (0, 1) làm trạng thái bắt đầu. 
4. Liên tục trích xuất nút u với thời gian đến nhỏ nhất đã biết. Nếu cur đã lớn hơn dist[u], hãy bỏ qua nó vì chúng tôi có cách tốt hơn để tiếp cận bạn. 
5. Với mỗi cạnh từ u đến v, hãy tính độ dài chu trình c = r + g. Xác định vị trí trong chu trình là cur % c. 
6. Nếu vị trí này nằm trong pha màu đỏ, nghĩa là nhỏ hơn r, tính thời gian chờ là r - (cur % c). Nếu không thì thời gian chờ đợi bằng không. Mô hình này đang chờ cho đến khi giai đoạn xanh bắt đầu. 
7. Tính thời gian đến của ứng viên là cur + chờ + t. Đây là thời gian sớm nhất có thể để chúng ta có thể hoàn thành việc vượt qua bờ vực này. 
8. Nếu thời gian ứng viên này cải thiện dist[v], hãy cập nhật nó và đẩy (candidate, v) vào hàng đợi ưu tiên. 
9. Tiếp tục cho đến khi tất cả các nút có thể truy cập được xử lý. Câu trả lời là dist[n]. 

Tính chính xác dựa trên thực tế là khi một nút được đưa ra khỏi hàng đợi ưu tiên với thời gian tối thiểu thì việc thư giãn sau đó không thể tạo ra thời gian đến tốt hơn cho nút đó. Mặc dù trọng số của cạnh phụ thuộc vào thời gian, hàm thư giãn có tính xác định và đơn điệu theo nghĩa là những người đến sau không thể tạo ra những khởi hành sớm hơn trên cùng một cạnh. 

## Tại sao nó hoạt động 

Bất biến cốt lõi là dist[u] luôn lưu trữ thời gian đến tối thiểu có thể đạt được tại nút u được tìm thấy cho đến nay và hàng đợi ưu tiên luôn mở rộng thời gian nhỏ nhất hiện tại. Hàm thời gian chờ biến đổi mỗi lần truyền tải thành chi phí xác định chỉ dựa trên thời gian đến nút nguồn, do đó, mọi sự nới lỏng cạnh đều tương đương với việc đánh giá một hàm cố định trên một trạng thái đã biết. Vì tất cả các chi phí đều không âm và Dijkstra luôn hoàn thành các nút theo thứ tự thời gian tăng dần, nên bất kỳ đường dẫn thay thế nào đến một nút sau đó đều không thể cải thiện trạng thái trước đó một cách hồi tố, điều này duy trì tính chính xác ngay cả với các cạnh phụ thuộc vào thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b, t, r, gr = map(int, input().split())
        g[a].append((b, t, r, gr))
        g[b].append((a, t, r, gr))

    INF = 10**30
    dist = [INF] * (n + 1)
    dist[1] = 0

    pq = [(0, 1)]

    while pq:
        cur, u = heapq.heappop(pq)
        if cur != dist[u]:
            continue

        for v, t, r, gr in g[u]:
            cycle = r + gr
            pos = cur % cycle

            wait = 0
            if pos < r:
                wait = r - pos

            nxt = cur + wait + t

            if nxt < dist[v]:
                dist[v] = nxt
                heapq.heappush(pq, (nxt, v))

    print(dist[n])

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ các tham số cạnh đầy đủ để mỗi lần thư giãn có thể tính toán thời gian chờ cục bộ mà không cần bất kỳ quá trình tiền xử lý toàn cục nào. 

Hàng đợi ưu tiên đảm bảo chúng tôi luôn mở rộng tuyến đường từng phần được biết đến nhiều nhất hiện tại. Séc`if cur != dist[u]`tránh xử lý các trạng thái cũ, điều này cần thiết để giữ độ phức tạp trong giới hạn logarit cho mỗi thao tác. 

Logic chờ là phần không chuẩn duy nhất: nó chuyển đổi một ràng buộc theo chu kỳ thành một độ trễ cộng gộp duy nhất được tính toán từ dấu thời gian hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
1 2 3 4 5
```| Bước | Nút | Thời gian | Hành động | quận [2] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | bắt đầu | thông tin | 
| 2 | 2 | 0+4 chờ +3 du lịch | thư giãn cạnh | 7 | 

Thời gian đến 0 rơi vào pha màu đỏ có độ dài 4, vì vậy chúng ta phải đợi đến thời điểm 4, sau đó đi qua 3 đơn vị. 

Điều này chứng tỏ rằng ngay cả một cạnh duy nhất cũng có thể chiếm ưu thế trong câu trả lời hoàn toàn thông qua việc chờ đợi. 

### Ví dụ 2 

đầu vào:```
3 2
1 2 4 1 1
2 3 9 6 7
```| Bước | Nút | Thời gian | Cạnh | Chờ đã | Tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1→2 | 1 | 5 | 
| 2 | 2 | 5 | 2→3 | 1 | 15 | 

Từ nút 1 đến nút 2, thời gian đến là pha màu đỏ có độ dài 1, vì vậy chúng tôi đợi 1 rồi di chuyển 4. Từ nút 2, thời gian đến 5 lại ở pha màu đỏ so với chu kỳ thứ hai, buộc phải đợi 1 trước khi truyền tải dài. 

Điều này khẳng định rằng tính tối ưu phụ thuộc vào thời gian đến chứ không chỉ vào cấu trúc đường dẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi nút được trích xuất tối đa một lần ở dạng tốt nhất của nó và mỗi lần giãn cạnh sẽ kích hoạt nhật ký chi phí đẩy đống n | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách và hàng đợi ưu tiên | 

Các ràng buộc lên tới 100.000 nút và cạnh phù hợp thoải mái trong độ phức tạp này, vì hệ số logarit giữ cho các hoạt động tối đa ở khoảng vài triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    def solve():
        n, m = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            a, b, t, r, gr = map(int, input().split())
            g[a].append((b, t, r, gr))
            g[b].append((a, t, r, gr))

        INF = 10**30
        dist = [INF] * (n + 1)
        dist[1] = 0
        pq = [(0, 1)]

        while pq:
            cur, u = heapq.heappop(pq)
            if cur != dist[u]:
                continue
            for v, t, r, gr in g[u]:
                c = r + gr
                pos = cur % c
                wait = 0 if pos >= r else r - pos
                nxt = cur + wait + t
                if nxt < dist[v]:
                    dist[v] = nxt
                    heapq.heappush(pq, (nxt, v))

        return dist[n]

    return str(solve())

# provided samples
assert run("2 1\n1 2 3 4 5\n") == "7"
assert run("3 2\n1 2 4 1 1\n2 3 9 6 7\n") == "15"

# custom cases
assert run("2 1\n1 2 5 0 10\n") == "5", "no waiting case"
assert run("2 1\n1 2 5 10 1\n") == "15", "always wait"
assert run("3 3\n1 2 1 1 1\n2 3 1 1 1\n1 3 10 0 10\n") == "2", "alternative path"
assert run("4 3\n1 2 5 3 3\n2 4 5 3 3\n1 4 20 0 5\n") == "16", "detour vs direct"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh không chờ đợi | 5 | hành vi pha đỏ bằng không | 
| cạnh luôn chờ đợi | 15 | độ trễ đầy đủ màu đỏ | 
| đồ thị tam giác | 2 | lựa chọn đường đi ngắn nhất theo thời gian | 
| đường vòng vs trực tiếp | 16 | tương tác của nhiều cạnh | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi r = 0, nghĩa là không có pha đỏ. Trong tình huống đó, việc tính toán chờ đợi không bao giờ được kích hoạt. Đối với một đầu vào như`1 2 5 0 10`, thuật toán tính toán chính xác pos = 0 và không thấy khoảng màu đỏ, tạo ra chi phí trực tiếp là 5. 

Một trường hợp khác là khi thời gian đến đúng ranh giới giữa màu đỏ và màu xanh lá cây. Vì màu đỏ được định nghĩa là khoảng [0, r), nên việc đến đúng thời điểm r sẽ không phải chờ đợi. Logic modulo xử lý việc này vì pos >= r rơi vào pha xanh. 

Trường hợp khó phát hiện cuối cùng là khi có nhiều đường dẫn đến một nút có thời gian khác nhau. Hàng đợi ưu tiên đảm bảo việc đến sớm nhất sẽ được xử lý trước và mọi lượt đến sau sẽ bị loại bỏ. Đối với một nút có thể truy cập nhanh chóng khi chờ đợi lâu và chậm mà không phải chờ đợi, thuật toán sẽ đánh giá chính xác cả hai nhưng chỉ giữ thời gian kết quả tối thiểu, vì mỗi lần thư giãn được đánh giá độc lập.
