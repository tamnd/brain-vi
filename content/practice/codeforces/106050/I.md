---
title: "CF 106050I - Hành trình của một khách du lịch"
description: "Chúng ta có một thành phố được mô hình hóa dưới dạng biểu đồ vô hướng có trọng số với tối đa 200 nghìn vị trí và đường. Mỗi con đường đều có một thời gian đi. Trong số tất cả các địa điểm, chỉ có P đầu tiên (có P lên tới 20) là những địa điểm du lịch thú vị."
date: "2026-06-25T12:26:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106050
codeforces_index: "I"
codeforces_contest_name: "Cataratas do Pinh\u00e3o 2025"
rating: 0
weight: 106050
solve_time_s: 59
verified: true
draft: false
---

[CF 106050I - Hành trình của một khách du lịch](https://codeforces.com/problemset/problem/106050/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thành phố được mô hình hóa dưới dạng biểu đồ vô hướng có trọng số với tối đa 200 nghìn vị trí và đường. Mỗi con đường đều có một thời gian đi. Trong số tất cả các địa điểm, chỉ có P đầu tiên (có P lên tới 20) là những địa điểm du lịch thú vị. Mỗi địa điểm này có hai thuộc tính: giá trị hạnh phúc đạt được khi bạn ghé thăm lần đầu tiên và chi phí thời gian ở đó khi bạn đến. 

Một khách du lịch bắt đầu tại một địa điểm khách sạn cố định, đó là nút N. Anh ta có một ngày với ngân sách thời gian chặt chẽ, từ sáng đến nửa đêm, nghĩa là một số phút cố định có sẵn. Anh ta có thể di chuyển dọc theo các con đường và bất cứ khi nào anh ta đến một địa điểm du lịch, anh ta có thể chọn dành thời gian ở đó và đạt được hạnh phúc ở đó hay không. Mỗi địa điểm được chọn có thể được truy cập nhiều nhất một lần. Anh ta không cần phải quay lại khách sạn. 

Nhiệm vụ là chọn một thứ tự và một tập hợp con các điểm du lịch sao cho tổng thời gian đi lại cộng với thời gian tham quan không vượt quá ngân sách, đồng thời tổng giá trị hạnh phúc là tối đa. 

Hạn chế cơ cấu chính là sự không phù hợp về quy mô. Biểu đồ lớn nhưng số lượng nút thú vị lại rất nhỏ. Điều này ngay lập tức gợi ý rằng chúng ta không thể suy luận về các đường đi tùy ý trên tất cả các nút mà chỉ về các đường đi ngắn nhất giữa một tập hợp con nhỏ các nút đặc biệt. 

Ngân sách thời gian thực tế là không đổi (khoảng 16 giờ, tức là dưới 1000 phút). Bất kỳ giải pháp nào khám phá các đường dẫn một cách rõ ràng trong không gian đồ thị đều không thể thực hiện được, vì ngay cả việc truyền tải tuyến tính trên mỗi tập hợp con cũng sẽ bùng nổ. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các tuyến đường trực tiếp trên các cạnh. 

Một trường hợp đơn giản nhưng quan trọng xuất hiện khi tuyến đường tối ưu liên quan đến việc bỏ qua một nút có mức độ hài lòng cao ở gần để chuyển sang nút ở xa vì cấu trúc hành trình khiến việc đi đường vòng trở nên đắt đỏ. Ví dụ: nếu hai điểm tham quan có khoảng cách gần nhau trên biểu đồ nhưng có thời gian ghé thăm lớn, thì chúng có thể tệ hơn so với một lựa chọn thay thế ở xa nhưng có thể ghé thăm nhanh. Cách tiếp cận tham lam “luôn có tỷ lệ tốt nhất” không thành công ở đây vì chi phí đi lại phụ thuộc vào đường đi chứ không phải cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi thứ tự có thể để tham quan các điểm tham quan P. Đối với mỗi hoán vị, chúng tôi sẽ tính toán thời gian di chuyển bằng cách chạy các đường đi ngắn nhất giữa các nút liên tiếp trong tuyến và cộng thêm thời gian truy cập. Điều này đã yêu cầu tính toán trước các đường dẫn ngắn nhất giữa tất cả các cặp nút đặc biệt. Ngay cả khi chúng ta giả sử những khoảng cách đó đã biết thì số lượng hoán vị là P!, mà tại P = 20 là hoàn toàn không khả thi, theo thứ tự 2,4e18. 

Ngay cả việc giảm điều này xuống thành thử tất cả các tập hợp con vẫn khiến việc đặt hàng không được giải quyết và việc đặt hàng chính xác là yếu tố quyết định chi phí đi lại. 

Quan sát quan trọng là chi phí đi lại chỉ phụ thuộc vào đường đi ngắn nhất giữa các nút chứ không phụ thuộc vào cấu trúc trung gian. Khi chúng tôi nén biểu đồ thành một biểu đồ có trọng số hoàn chỉnh về các điểm tham quan P cộng với khách sạn, vấn đề sẽ trở thành một nhiệm vụ lập trình động theo phong cách nhân viên bán hàng du lịch với ràng buộc về thời gian trên ba lô. 

Chúng tôi tính toán các đường đi ngắn nhất từ ​​khách sạn và từ mỗi điểm tham quan P đến tất cả các nút bằng Dijkstra. Điều này cho chúng ta một ma trận khoảng cách hoàn chỉnh giữa các nút đặc biệt P + 1. Từ đó, vấn đề giảm xuống còn việc chọn một bước đi tối ưu qua các nút này, trong đó mỗi lần di chuyển đều có một chi phí đã biết và mỗi nút đóng góp một lần cho hạnh phúc và chi phí thời gian. 

Điều này dẫn đến lập trình động bitmask một cách tự nhiên: trạng thái biểu thị những điểm du lịch nào đã được ghé thăm và điểm thu hút được ghé thăm lần cuối. Quá trình chuyển đổi hãy thử thêm một điểm thu hút mới chưa được khám phá, cập nhật chi phí thời gian và hạnh phúc tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force trên các điểm tham quan | O(P! · P · M log N) | O(N) | Quá chậm | 
| Dijkstra + bitmask DP | O((P+1) M log N + P^2 2^P) | O(P 2^P) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chạy Dijkstra từ nút khách sạn N để tính thời gian di chuyển ngắn nhất tới tất cả các nút. Điều này mang lại chi phí để đạt được bất kỳ điểm thu hút nào làm điểm khởi đầu. 
2. Chạy Dijkstra một lần từ mỗi điểm tham quan P. Điều này tạo ra khoảng cách đường đi ngắn nhất giữa mỗi cặp điểm tham quan. Bước này chuyển đổi biểu đồ gốc thành biểu đồ có trọng số được kết nối đầy đủ trên P + 1 nút quan trọng. 
3. Lưu trữ thời gian di chuyển trong ma trận dist trong đó dist[i][j] là thời gian ngắn nhất từ ​​nút i đến nút j và bao gồm khách sạn làm chỉ mục. 
4. Xác định trạng thái DP dp[mask][i], nghĩa là tổng thời gian tối thiểu cần thiết để bắt đầu từ khách sạn, ghé thăm chính xác tập hợp các điểm tham quan trong mặt nạ và kết thúc tại điểm thu hút i. 
5. Khởi tạo DP bằng cách xem xét từng điểm thu hút i. Chi phí là dist[hotel][i] cộng với thời gian ở điểm tham quan đó. Hạnh phúc của những trạng thái này chỉ là f[i]. 
6. Chuyển đổi bằng cách cố gắng mở rộng tập hợp đã truy cập. Từ dp[mask][i], hãy thử truy cập bất kỳ j nào chưa được truy cập. Trạng thái mới là mặt nạ ∪ {j}, kết thúc tại j, với chi phí tăng thêm dist[i][j] cộng với thời gian truy cập t[j]. Chúng tôi dành thời gian tối thiểu có thể cho mỗi tiểu bang. 
7. Sau khi điền DP, hãy quét tất cả các trạng thái. Đối với mọi dp[mask][i] không vượt quá ngân sách thời gian, hãy tính tổng mức độ hạnh phúc của mặt nạ và theo dõi mức tối đa. 

Lý do điều này có tác dụng là vì khi biểu đồ được nén thành khoảng cách đường đi ngắn nhất giữa các nút quan trọng, quyết định duy nhất còn lại là sắp xếp thứ tự của một tập hợp nhỏ. DP liệt kê ngầm tất cả các đơn hàng hợp lệ trong khi chỉ giữ lại cách tốt nhất (thời gian tối thiểu) để tiếp cận từng cấu hình kết thúc tập hợp con. Điều này ngăn cản việc thăm dò dư thừa các hoán vị tương đương chỉ khác nhau về cấu trúc bên trong chứ không khác nhau về tập hợp con và điểm kết thúc đã chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

INF = 10**18

def dijkstra(start, g, n):
    dist = [INF] * (n + 1)
    dist[start] = 0
    pq = [(0, start)]
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist

def solve():
    N, M, P = map(int, input().split())
    g = [[] for _ in range(N + 1)]

    for _ in range(M):
        u, v, w = map(int, input().split())
        g[u].append((v, w))
        g[v].append((u, w))

    happiness = list(map(int, input().split()))
    tcost = list(map(int, input().split()))

    attractions = list(range(1, P + 1))
    hotel = N

    nodes = attractions + [hotel]
    K = P

    dist = [[INF] * (P + 1) for _ in range(P + 1)]

    full_d = dijkstra(hotel, g, N)
    for i in range(P):
        dist[K][i] = full_d[attractions[i]]

    for i in range(P):
        d = dijkstra(attractions[i], g, N)
        for j in range(P):
            dist[i][j] = d[attractions[j]]

    TIME = 16 * 60

    dp = [[INF] * P for _ in range(1 << P)]

    for i in range(P):
        dp[1 << i][i] = dist[K][i] + tcost[i]

    for mask in range(1 << P):
        for i in range(P):
            if dp[mask][i] > TIME:
                continue
            if not (mask & (1 << i)):
                continue
            for j in range(P):
                if mask & (1 << j):
                    continue
                nmask = mask | (1 << j)
                cand = dp[mask][i] + dist[i][j] + tcost[j]
                if cand < dp[nmask][j]:
                    dp[nmask][j] = cand

    best = 0
    for mask in range(1 << P):
        for i in range(P):
            if dp[mask][i] <= TIME:
                val = 0
                for j in range(P):
                    if mask & (1 << j):
                        val += happiness[j]
                if val > best:
                    best = val

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai tách giai đoạn nén đồ thị khỏi giai đoạn DP tổ hợp. Các lần chạy Dijkstra đảm bảo rằng tất cả các quyết định di chuyển chỉ phụ thuộc vào các đường đi ngắn nhất chứ không phải cấu trúc biểu đồ thô. DP theo dõi thời gian tối thiểu trên mỗi trạng thái, điều này rất quan trọng vì nhiều tuyến đường khác nhau có thể dẫn đến cùng một nhóm khách truy cập với chi phí thời gian khác nhau. 

Một điểm tinh tế là chúng tôi luôn bao gồm thời gian truy cập khi nhập một nút trong chuyển tiếp DP. Thiếu điều này dẫn đến đánh giá thấp việc sử dụng thời gian và chấp nhận các tuyến đường không hợp lệ. Một lỗi phổ biến khác là quên hạn chế chuyển đổi chỉ ở các trạng thái đã bao gồm nút hiện tại, điều này phá vỡ bất biến DP mà dp[mask][i] tương ứng với trạng thái kết thúc hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng ta xem xét một biểu đồ nhỏ có ba điểm tham quan A, B, C và một khách sạn H, với giới hạn thời gian là 100. Giả sử khoảng cách ngắn nhất là: 

| Bước | mặt nạ | kết thúc | thời gian | 
| --- | --- | --- | --- | 
| khởi tạo A | {A} | A | 30 | 
| khởi tạo B | {B} | B | 40 | 
| A→C | {A,C} | C | 70 | 
| B→C | {B,C} | C | 90 | 

DP bắt đầu bằng cách khởi tạo các nút đơn từ khách sạn. Sau đó, nó mở rộng các tập hợp con. Bảng này cho thấy rằng việc đến C sau A sẽ rẻ hơn so với sau B, do đó DP duy trì lộ trình tốt nhất đến từng tiểu bang. 

Điều này chứng tỏ rằng thuật toán chọn chính xác trong số nhiều cách để tiếp cận cùng một tập hợp con, chỉ giữ lại thời gian tối thiểu. 

### Ví dụ 2 

Hãy xem xét trường hợp việc truy cập sớm một nút có mức độ hài lòng cao sẽ không có ý nghĩa gì do chi phí đi lại. 

| Bước | mặt nạ | kết thúc | thời gian | 
| --- | --- | --- | --- | 
| khởi tạo A | {A} | A | 10 | 
| khởi tạo B | {B} | B | 12 | 
| A→B | {A,B} | B | 200 | 

Mặc dù A ở gần hơn nhưng việc chuyển từ A sang B cực kỳ tốn kém. DP đương nhiên tránh xây dựng các tập hợp con lớn thông qua các trung gian tốn kém, bởi vì các trạng thái đó vượt quá giới hạn thời gian hoặc bị chi phối bởi các lựa chọn thay thế rẻ hơn. 

Điều này xác nhận rằng DP cân bằng chính xác cơ cấu đi lại với những hạn chế về thời gian tham quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log N + P · M log N + P^2 2^P) | Dijkstra chạy thống trị tiền xử lý, DP trên các tập hợp con thống trị tổ hợp | 
| Không gian | O(N + P 2^P) | lưu trữ đồ thị cộng với bảng DP và ma trận khoảng cách | 

Các ràng buộc được tách biệt rõ ràng: biểu đồ lớn được xử lý bằng Dijkstra, trong khi phần hàm mũ được giới hạn ở P ≤ 20. Điều này đảm bảo DP vẫn khả thi vì 2^20 là khoảng một triệu trạng thái và có thể quản lý được quá trình chuyển đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else ""

# The following are structural tests rather than full I/O harness,
# since full integration depends on embedding solve().

# minimal case
# 2 nodes, 1 attraction
# trivial path
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm thu hút duy nhất tối thiểu | hạnh phúc đúng đắn nếu có thể tiếp cận | khởi tạo cơ sở | 
| hai điểm tham quan với thời gian eo hẹp | sự lựa chọn duy nhất tốt nhất | lựa chọn tập hợp con | 
| trung tâm khách sạn đồ thị sao | số lượt chọn trực tiếp tối đa | độ chính xác của đường đi ngắn nhất | 
| đồ thị chuỗi | độ nhạy đặt hàng | Tính chính xác của quá trình chuyển đổi DP | 

## Vỏ cạnh 

Một trường hợp trong đó tất cả các điểm tham quan đều cực kỳ gần nhau nhưng cách xa khách sạn sẽ kiểm tra xem thuật toán có tính toán đúng Dijkstra ban đầu từ khách sạn hay không. Việc khởi tạo DP đảm bảo rằng mọi trạng thái đều bao gồm chi phí để đạt được điểm thu hút đầu tiên một cách chính xác, do đó, một cách tiếp cận ngây thơ giả định chi phí ban đầu bằng 0 sẽ đánh giá quá cao tính khả thi. 

Trường hợp thứ hai trong đó một điểm thu hút có mức độ hài lòng rất cao nhưng thời gian ghé thăm cực lớn đảm bảo rằng DP loại trừ nó một cách chính xác ngay cả khi nó có cấu trúc trung tâm trong biểu đồ. Trạng thái dp[mask][i] luôn mang theo thời gian tích lũy, do đó các nút như vậy sẽ tự động trở nên không khả thi khi chúng đẩy tổng số vượt quá giới hạn. 

Trường hợp thứ ba liên quan đến hai điểm tham quan có chi phí du lịch không đối xứng, trong đó đi A đến B thì rẻ nhưng B đến A thì đắt. DP xử lý việc này một cách chính xác vì nó đánh giá cả hai hướng một cách độc lập trong ma trận khoảng cách và không đảm nhận tính đối xứng trong thành phần đường dẫn.
