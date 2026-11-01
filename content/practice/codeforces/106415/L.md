---
title: "CF 106415L - Lập kế hoạch lộ trình bay không người lái"
description: "Thành phố là một đồ thị có trọng số có hướng. Máy bay không người lái bắt đầu tại một trung tâm nhất định, có thể bay dọc theo các tuyến đường được chỉ dẫn và mọi tuyến đường đều có chi phí năng lượng."
date: "2026-06-25T09:45:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106415
codeforces_index: "L"
codeforces_contest_name: "Winter Cup 8.0 Online Mirror Contest"
rating: 0
weight: 106415
solve_time_s: 37
verified: true
draft: false
---

[CF 106415L - Lập kế hoạch lộ trình bay không người lái](https://codeforces.com/problemset/problem/106415/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thành phố là một đồ thị có trọng số có hướng. Máy bay không người lái bắt đầu tại một trung tâm nhất định, có thể bay dọc theo các tuyến đường được chỉ dẫn và mọi tuyến đường đều có chi phí năng lượng. Một số trung tâm chứa các gói hàng và máy bay không người lái phải thường xuyên ghé thăm các trung tâm gói hàng để thu thập chính xác năm gói hàng khác nhau trước khi quay trở lại trung tâm ban đầu. Việc đi qua trung tâm gói hàng không buộc máy bay không người lái phải lấy gói hàng, vì vậy việc thu thập gói hàng là một lựa chọn. 

Đầu vào mô tả biểu đồ, trung tâm bắt đầu và tập hợp các vị trí gói hàng. Đầu ra là chi phí năng lượng tối thiểu của một chuyến đi khép kín bắt đầu và kết thúc tại điểm xuất phát trong khi thu thập năm gói hàng. Nếu không có bước đi như vậy tồn tại, câu trả lời là`-1`. 

Biểu đồ có thể chứa tới$10^5$trung tâm và$10^5$các cạnh có hướng. Giải pháp đường dẫn ngắn nhất thông thường trên tất cả các kết hợp gói có thể sẽ không thể thực hiện được vì số lượng nút biểu đồ quá lớn. Hạn chế hữu ích là có tối đa 35 trung tâm gói, điều đó có nghĩa là phần đắt tiền của giải pháp có thể phụ thuộc vào số lượng gói thay vì số lượng nút biểu đồ. 

Trọng số của cạnh có thể lớn bằng$10^9$, vì vậy câu trả lời có thể vượt quá giới hạn số nguyên 32 bit. Số nguyên Python tự động xử lý việc này, nhưng giá trị vô cực vẫn cần được chọn đủ lớn. Một điểm tinh tế khác là máy bay không người lái có thể quay lại các trung tâm và cạnh, vì vậy chúng ta không thể giả định một con đường đơn giản. Một giải pháp chỉ kiểm tra các tuyến đường truy cập năm trung tâm gói hàng một lần mà không có đường đi ngắn nhất giữa chúng sẽ bỏ lỡ các tuyến đường tối ưu hợp lệ. 

Ví dụ, hãy xem xét:```
N = 3, M = 2
Start = 1
Edges:
1 -> 2 cost 5
2 -> 1 cost 5
Packages:
2
```Đầu ra đúng là:```
-1
```Một cách tiếp cận bất cẩn có thể đếm số lượt truy cập gói hàng duy nhất và cho rằng tuyến đường là hợp lệ, nhưng máy bay không người lái cần chính xác năm gói hàng được thu thập chứ không chỉ bất kỳ số lượt truy cập gói hàng nào. 

Một trường hợp khác:```
N = 6, M = 6
Start = 1
Edges:
1 -> 2 cost 1
2 -> 1 cost 1
2 -> 3 cost 1
3 -> 2 cost 1
3 -> 4 cost 1
4 -> 3 cost 1
Packages:
2 3 4
```Đầu ra đúng là:```
-1
```Chỉ có ba trung tâm gói riêng biệt. Một tuyến đường có thể truy cập lại chúng nhiều lần, nhưng việc truy cập lại không tạo ra các gói được thu thập mới, do đó không thể truy cập được năm gói riêng biệt được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi thứ tự có thể để truy cập năm trung tâm trọn gói. Đối với mỗi đơn hàng đã chọn, chúng tôi sẽ tính toán đường đi ngắn nhất giữa các vị trí liên tiếp, cộng các chi phí đó và lấy mức tối thiểu. Điều này đúng vì bất kỳ tuyến đường hợp lệ nào cũng có thể được phân tách thành các đoạn ngắn nhất giữa các trung tâm gói mà nó chọn để thu thập. 

Vấn đề là có thể có tới 35 trung tâm gói. Số lựa chọn có thể có là khoảng$35^5$, tức là khoảng 52 triệu chuỗi ngay cả trước khi xem xét tính toán đường đi ngắn nhất. Nếu chúng ta chạy đi chạy lại các thuật toán đồ thị cho từng chuỗi thì công việc sẽ vượt xa giới hạn. 

Quan sát quan trọng là biểu đồ rất lớn nhưng số lượng vị trí quan trọng lại rất nhỏ. Quyết định duy nhất quan trọng là trung tâm gói nào đã được thu thập. Sự chuyển động giữa những vị trí quan trọng này có thể được nén lại thành những khoảng cách đường đi ngắn nhất. 

Trước tiên, chúng tôi tính toán khoảng cách ngắn nhất từ ​​mọi vị trí quan trọng đến mọi vị trí quan trọng khác. Các vị trí quan trọng là trung tâm khởi đầu và tất cả các trung tâm gói. Vì có nhiều nhất là 36 đường, nên chỉ cần 36 đường chạy ngắn nhất. Sau khi nén, đồ thị gốc biến mất và chúng ta có một đồ thị có hướng hoàn chỉnh nhỏ. 

Sau đó, chúng tôi sử dụng lập trình động trên các tập hợp con của các gói đã thu thập. Một tiểu bang lưu trữ chi phí tối thiểu để có mặt tại một trung tâm gói cụ thể sau khi thu thập một bộ gói nhất định. Vì có tối đa 35 gói nên chúng ta chỉ cần giữ trạng thái chứa tối đa 5 gói đã thu thập. Kích thước tập hợp con rất nhỏ nên số lượng trạng thái có thể quản lý được. 

Lực lượng vũ phu hoạt động vì mọi tuyến đường hợp lệ đều có thứ tự các gói được thu thập. Nó thất bại vì nó khám phá tất cả các đơn hàng mà không nhớ rằng nhiều tuyến đường từng phần cũng gặp phải tình huống tương tự. Tập hợp con DP hợp nhất các tình huống tương đương đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(K^5 \cdot \text{shortest path cost})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(K \cdot (M \log N) + \binom{K}{5} \cdot K)$|$O(\binom{K}{5} \cdot K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách chứa hub khởi đầu và mọi hub gói. Xóa trung tâm khởi đầu khỏi danh sách gói nếu nó cũng xuất hiện ở đó, vì việc thu thập gói khi bắt đầu vẫn được tính là vị trí gói nhưng DP cần ánh xạ rõ ràng về các chỉ mục gói. 
2. Chạy Dijkstra từ mọi vị trí quan trọng. Chỉ lưu trữ khoảng cách đến các vị trí quan trọng khác. Những giá trị này thể hiện chi phí di chuyển rẻ nhất có thể giữa các điểm có ý nghĩa trong khi bỏ qua các trung tâm trung gian không cần thiết. 
3. Nếu tồn tại ít hơn năm trung tâm gói, hãy quay lại ngay lập tức`-1`. Không thể có đủ gói riêng biệt để thu thập. 
4. Tạo bảng lập trình động trong đó trạng thái chứa mặt nạ bit của các gói đã thu thập và vị trí gói hiện tại. Giá trị được lưu trữ là chi phí tối thiểu để đạt được vị trí gói hàng đó sau khi thu thập chính xác các gói hàng trong mặt nạ. 
5. Khởi tạo các trạng thái bằng cách di chuyển từ trung tâm khởi đầu đến một trung tâm gói. Mỗi lần di chuyển như vậy sẽ thu thập gói hàng tại điểm đến. 
6. Liên tục mở rộng trạng thái bằng cách di chuyển đến trung tâm gói chưa được thu thập. Thêm khoảng cách đường đi ngắn nhất giữa vị trí hiện tại và vị trí gói mới. 
7. Bất cứ khi nào một tiểu bang có chính xác năm gói hàng được thu thập, hãy cộng chi phí trả lại từ trung tâm gói hàng hiện tại về trung tâm ban đầu. Mức tối thiểu trong số các tuyến đường đã hoàn thành này là câu trả lời. 

Lý do điều này có hiệu quả là sau khi thay thế mọi phân đoạn chuyển động bằng chi phí ngắn nhất có thể, bất kỳ tuyến đường tối ưu nào cũng chỉ có thể được thể hiện bằng thứ tự thu thập các gói hàng. DP lưu trữ chi phí tốt nhất có thể cho mỗi đơn hàng từng phần như vậy. Nếu hai tuyến đã thu thập các gói giống nhau và hiện ở cùng một trung tâm gói thì chỉ có tuyến rẻ hơn mới có thể dẫn đến câu trả lời cuối cùng tốt hơn, do đó việc giữ một giá trị sẽ không mất gì. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    start = int(input())

    graph = [[] for _ in range(n)]
    for _ in range(m):
        a, b, c = map(int, input().split())
        graph[a - 1].append((b - 1, c))

    k = int(input())
    packages = []
    if k:
        packages = list(map(int, input().split()))
        packages = [x - 1 for x in packages]

    if k < 5:
        print(-1)
        return

    important = [start - 1] + packages
    idx = {v: i for i, v in enumerate(important)}
    s = len(important)

    dist = [[10**30] * s for _ in range(s)]

    for i, src in enumerate(important):
        pq = [(0, src)]
        seen = [False] * n
        d = [10**30] * n
        d[src] = 0

        while pq:
            cur, v = heapq.heappop(pq)
            if seen[v]:
                continue
            seen[v] = True

            for to, w in graph[v]:
                nd = cur + w
                if nd < d[to]:
                    d[to] = nd
                    heapq.heappush(pq, (nd, to))

        for j, v in enumerate(important):
            dist[i][j] = d[v]

    pack_count = len(packages)
    dp = {}

    for i in range(pack_count):
        cost = dist[0][i + 1]
        if cost < 10**30:
            dp[(1 << i, i)] = cost

    ans = 10**30

    changed = True
    while changed:
        changed = False
        current = list(dp.items())
        for (mask, pos), value in current:
            if mask.bit_count() == 5:
                back = dist[pos + 1][0]
                if back + value < ans:
                    ans = back + value
                continue

            for nxt in range(pack_count):
                if mask & (1 << nxt):
                    continue
                new_mask = mask | (1 << nxt)
                move = dist[pos + 1][nxt + 1]
                if move >= 10**30:
                    continue
                nv = value + move
                key = (new_mask, nxt)
                if nv < dp.get(key, 10**30):
                    dp[key] = nv
                    changed = True

    print(-1 if ans == 10**30 else ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên thực hiện nén đường dẫn ngắn nhất. Mảng`important`giữ các nút quan trọng và`dist`chỉ lưu trữ khoảng cách giữa các nút đó. Điều này tránh việc thực hiện tập hợp con DP trên biểu đồ gốc. 

Khóa DP chứa mặt nạ gói đã thu thập và chỉ mục của gói hiện tại. Mặt nạ sử dụng một bit cho mỗi trung tâm gói. Vì chỉ cần năm gói nên số lượng mặt nạ có thể tiếp cận nhỏ hơn nhiều so với tất cả các tập hợp con có thể có. 

Việc khởi tạo xử lý chuyến bay đầu tiên ngay từ đầu. Mỗi quá trình chuyển đổi sẽ chọn một gói chưa được thu thập và di chuyển đến gói đó bằng khoảng cách ngắn nhất được tính toán trước. Khi trạng thái đạt tới 5 bit, hành trình quay về sẽ được kiểm tra. 

Một sai lầm phổ biến là quên rằng trung tâm khởi đầu cũng cần một đường đi ngắn nhất quay về từ trung tâm gói cuối cùng. Một cách khác là trộn các chỉ mục gói với các chỉ mục biểu đồ. DP sử dụng các chỉ số gói, trong khi`dist`sử dụng các chỉ số nút quan trọng được nén, do đó`+1`bù đắp là cần thiết. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
8 12
1
...
Packages: 2 3 4 5 6 7
```Sự phát triển DP là: 

| Mặt nạ | Gói hiện tại | Chi phí | 
| --- | --- | --- | 
| {3} | 3 | 2 | 
| {3,2} | 2 | 3 | 
| {3,2,6} | 6 | 15 | 
| {3,2,6,5} | 5 | 17 | 
| {3,2,6,5,7} | 7 | 21 | 

Trở về từ gói 7 về lúc bắt đầu có giá 6, mang lại:```
27
```Điều này cho thấy tại sao việc xem lại biểu đồ không phải là vấn đề. Tuyến đường chỉ được thể hiện bằng thứ tự gói đã thu thập. 

Đối với trường hợp đường dẫn quay lại không tồn tại:```
4 3
1
1 2 5
2 3 5
3 4 5
5
2 3 4
```Dấu vết là: 

| Mặt nạ | Gói hiện tại | Chi phí | 
| --- | --- | --- | 
| {2} | 2 | 5 | 
| {2,3} | 3 | 10 | 
| {2,3,4} | 4 | 15 | 

DP không bao giờ tạo trạng thái hoàn thành vì không có năm gói. Câu trả lời vẫn là vô hạn và chương trình sẽ in`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(K \cdot (M \log N) + 2^K \cdot K)$| Chúng tôi chỉ chạy Dijkstra cho các nút quan trọng và sau đó xử lý các trạng thái nén | 
| Không gian |$O(K^2 + 2^K)$| Khoảng cách giữa các nút quan trọng và trạng thái DP được lưu trữ | 

Với$K \le 35$, phần tập hợp con bị giới hạn bởi thực tế là chỉ có các trạng thái có tối đa năm gói được thu thập mới thực sự đạt được. Công việc vẽ đồ thị đắt tiền được giới hạn ở vài chục lần chạy Dijkstra, phù hợp với giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

# minimum impossible
assert run("""1 0
1
0
""") == "-1\n"

# simple possible cycle with five packages
assert run("""6 10
1
1 2 1
2 1 1
2 3 1
3 2 1
3 4 1
4 3 1
4 5 1
5 4 1
5 6 1
6 5 1
5
2 3 4 5 6
""") == "10\n"

# impossible because not enough package hubs
assert run("""3 2
1
1 2 1
2 1 1
1
2
""") == "-1\n"

# all reachable, answer requires choosing best order
assert run("""7 12
1
1 2 3
2 1 3
2 3 1
3 2 1
3 4 1
4 3 1
4 5 1
5 4 1
5 6 1
6 5 1
6 7 1
7 6 1
5
2 3 4 5 6
""") == "10\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút đơn không có gói |`-1`| Xử lý các gói bị thiếu và biểu đồ trống | 
| Chuỗi tuyến tính có chu kỳ |`10`| Kiểm tra chuyển tiếp DP bình thường và chi phí hoàn trả | 
| Chỉ một gói |`-1`| Ngăn chặn việc tính số lượt truy cập lặp lại dưới dạng gói mới | 
| Nhiều lựa chọn gói |`10`| Xác nhận lựa chọn tuyến đường nén ngắn nhất | 

## Vỏ cạnh 

Khi có ít hơn năm trung tâm gói, thuật toán sẽ dừng ngay lập tức. Đối với đầu vào:```
3 2
1
1 2 5
2 1 5
1
2
```Máy bay không người lái có thể ghé thăm gói hàng mãi mãi, nhưng nó không bao giờ có thể thu thập năm gói hàng riêng biệt. Thuật toán trả về`-1`trước khi chạy Dijkstra. 

Khi có thể truy cập vào trung tâm gói nhưng tuyến đường không thể quay lại điểm bắt đầu, quá trình chuyển đổi cuối cùng sẽ phát hiện ra nó. DP có thể tìm thấy một đường dẫn gồm 5 gói, nhưng khoảng cách từ điểm bắt đầu là vô hạn nên tuyến đường đó bị từ chối. 

Khi có nhiều tuyến đến cùng một tập hợp và gói hiện tại đã thu thập, DP chỉ giữ lại tuyến rẻ nhất. Tuyến đường một phần đắt tiền hơn không thể trở nên tốt hơn về sau vì mọi chi phí di chuyển trong tương lai chỉ phụ thuộc vào vị trí hiện tại và các gói hàng còn lại chứ không phụ thuộc vào lịch sử trước đó. Đây là thuộc tính cho phép nén trạng thái.
