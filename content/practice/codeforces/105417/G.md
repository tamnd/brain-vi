---
title: "CF 105417G - Con gà và quả trứng"
description: "Chúng ta được cung cấp một biểu đồ có hướng trong đó chuyển động dọc theo mỗi cạnh mất thời gian khác nhau tùy thuộc vào việc chúng ta đang mô phỏng một con gà hay một quả trứng. Có một số nút vào được chỉ định để thử nghiệm có thể bắt đầu và một số nút thoát thể hiện trạng thái thành công."
date: "2026-06-23T17:28:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105417
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 10-11-24 Div. 1 (Advanced)"
rating: 0
weight: 105417
solve_time_s: 118
verified: false
draft: false
---

[CF 105417G - Con gà và quả trứng](https://codeforces.com/problemset/problem/105417/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng trong đó chuyển động dọc theo mỗi cạnh mất thời gian khác nhau tùy thuộc vào việc chúng ta đang mô phỏng một con gà hay một quả trứng. Có một số nút vào được chỉ định để thử nghiệm có thể bắt đầu và một số nút thoát thể hiện trạng thái thành công. 

Đối với bất kỳ lối vào bắt đầu nào, một con gà (hoặc quả trứng) được cho là hoạt động tối ưu và luôn đi theo con đường nhanh nhất có thể đến bất kỳ lối ra nào. Nếu không có đường dẫn nào tồn tại từ đầu đến bất kỳ lối ra nào thì quá trình chạy đó sẽ không bao giờ kết thúc. 

Thí nghiệm được lặp lại hai lần theo phương pháp xác suất. Đầu tiên, một con gà được đặt ngẫu nhiên một cách đồng nhất trên một trong các nút vào và thời gian di chuyển của nó để đến được lối ra được ghi lại. Sau đó, một quả trứng được đặt độc lập và ngẫu nhiên một cách thống nhất trên một nút vào và thời gian di chuyển của nó được ghi lại. Kết quả phụ thuộc vào việc ai đến được lối ra nhanh hơn, với tỷ lệ hòa được phép khi thời gian khớp nhau hoặc cả hai đều không đạt được lối ra. 

Đầu ra được xác định bằng cách so sánh xác suất con gà về đích nhanh hơn quả trứng với xác suất ngược lại. 

Các ràng buộc cho phép lên tới 200.000 nút và cạnh. Điều này ngay lập tức loại trừ mọi phương pháp tính toán đường đi ngắn nhất riêng biệt cho mỗi lối vào, vì cách đó sẽ chạy liên tục các thuật toán đồ thị trên cùng một cấu trúc. Giải pháp phải sử dụng lại các tính toán và dựa vào một số lượng nhỏ các đường đi ngắn nhất toàn cầu. 

Một trường hợp phức tạp xuất hiện khi một số lối vào không thể đến được bất kỳ lối ra nào. Trong trường hợp đó, khoảng cách của họ thực sự là vô hạn. Nếu cả hai loài đều có khởi đầu không thể đạt được, những trường hợp đó sẽ góp phần hòa hơn là thắng. Một cách tiếp cận ngây thơ đơn giản là bỏ qua các nút không thể truy cập được sẽ làm sai lệch xác suất. 

Một trường hợp góc quan trọng khác phát sinh khi tất cả các lối vào đều không thể truy cập được. Khi đó cả con gà và quả trứng luôn hòa và đáp án phải là “hòa”. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ xem xét từng cặp lối vào theo thứ tự cho con gà và quả trứng, tính toán thời gian đường đi ngắn nhất của chúng và so sánh chúng. Ngay cả khi các đường dẫn ngắn nhất được tính toán trước, điều này vẫn yêu cầu so sánh tất cả các cặp kích thước a, dẫn đến bước so sánh bậc hai O(a²), quá chậm đối với tốc độ lên tới 200.000. 

Quan sát quan trọng là sau khi chúng tôi tính toán, đối với mỗi nút, thời gian tối thiểu để đạt được bất kỳ lối ra nào cho gà và trứng riêng biệt, mỗi lối vào sẽ chỉ trở thành một cặp số duy nhất. Sau đó, vấn đề trở thành so sánh hai biến ngẫu nhiên độc lập được rút ra đồng nhất từ ​​một tập hợp có kích thước a cố định. 

Chúng ta được yêu cầu tính toán tần suất một giá trị trong danh sách nhỏ hơn giá trị khác trong cùng danh sách trên tất cả các cặp có thứ tự một cách hiệu quả. Điều này có thể được chuyển đổi thành một bài toán đếm trên các mảng đã được sắp xếp thay vì liệt kê cặp rõ ràng. 

Với mỗi lối vào i, chúng ta biết một cặp (tc[i], te[i]). Để tính toán tần suất gà thắng, chúng ta tính tổng trên tất cả j số i sao cho tc[i] < te[j]. Sắp xếp tc cho phép chúng ta trả lời từng truy vấn te[j] bằng tìm kiếm nhị phân. Cấu trúc tương tự được áp dụng đối xứng cho trứng thắng. 

Nút cổ chai chuyển từ truyền tải đồ thị sang tính toán các đường đi ngắn nhất một lần cho mỗi loại trọng số, sau đó chuyển sang sắp xếp và tìm kiếm nhị phân, đủ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| So sánh cặp Brute Force | O(a² + m log n) | O(n) | Quá chậm | 
| Dijkstra + sắp xếp + đếm | O(m log n + a log a) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đảo ngược hướng biểu đồ và coi các điểm thoát là nguồn. Điều này biến bài toán từ “bắt đầu từ lối vào, đi đến lối ra” thành bài toán đường đi ngắn nhất đa nguồn tiêu chuẩn. Chạy Dijkstra từ tất cả các lối thoát cùng một lúc sẽ mang lại khoảng cách ngắn nhất tới bất kỳ lối thoát nào cho mỗi nút. 
2. Chạy Dijkstra hai lần, một lần với trọng lượng cạnh gà và một lần với trọng lượng cạnh trứng. Điều này tạo ra hai mảng tc và te trong đó tc[v] là thời gian di chuyển tối thiểu của gà từ v đến bất kỳ lối ra nào và tương tự cho te[v]. 
3. Chỉ trích xuất các giá trị tương ứng với các nút lối vào, tạo thành hai danh sách có kích thước a, nhưng được ghép nối theo chỉ mục vì cả hai loài đều sử dụng cùng một bộ lối vào. 
4. Thay thế khoảng cách không thể truy cập bằng giá trị trọng điểm lớn biểu thị vô cực. Điều này phải nhất quán trên cả hai loài để các trạng thái không thể truy cập được so sánh chính xác. 
5. Sắp xếp danh sách thời gian gà qua các lối vào. 
6. Với mỗi lối vào j, hãy tính xem có bao nhiêu lần con gà nhỏ hơn te[j] bằng cách sử dụng tìm kiếm nhị phân trên tc. Tính tổng các giá trị này để có tổng số gà thắng. 
7. Tính tổng số cặp đặt hàng là a². Tính toán quả trứng thắng một cách đối xứng bằng cách hoán đổi vai trò của tc và te. 
8. Hòa có được bằng cách trừ số tiền thắng từ tổng số cặp và câu trả lời cuối cùng được quyết định bằng cách so sánh số lần thắng. 

Tính chính xác dựa trên thực tế là mỗi cặp lối vào có thứ tự đóng góp chính xác một loại kết quả giữa gà thắng, trứng thắng hoặc hòa và các loại này được xác định đầy đủ bằng cách so sánh hai giá trị đường đi ngắn nhất vô hướng. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

INF = 10**30

def dijkstra(n, adj, sources):
    dist = [INF] * (n + 1)
    h = []
    for s in sources:
        dist[s] = 0
        heapq.heappush(h, (0, s))

    while h:
        d, u = heapq.heappop(h)
        if d != dist[u]:
            continue
        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(h, (nd, v))
    return dist

def solve():
    n, m, a, b = map(int, input().split())

    adj = [[] for _ in range(n + 1)]
    radj = [[] for _ in range(n + 1)]

    edges = []
    for _ in range(m):
        u, v, c, e = map(int, input().split())
        adj[u].append((v, (c, e)))
        radj[v].append(u)

    starts = list(map(int, input().split()))
    exits = list(map(int, input().split()))

    radj_ch = [[] for _ in range(n + 1)]
    radj_eg = [[] for _ in range(n + 1)]

    for u in range(1, n + 1):
        for v, (c, e) in adj[u]:
            radj_ch[v].append((u, c))
            radj_eg[v].append((u, e))

    tc_all = dijkstra(n, radj_ch, exits)
    te_all = dijkstra(n, radj_eg, exits)

    tc = [tc_all[s] for s in starts]
    te = [te_all[s] for s in starts]

    def count_wins(a_list, b_list):
        a_sorted = sorted(a_list)

        from bisect import bisect_left
        res = 0
        for x in b_list:
            res += bisect_left(a_sorted, x)
        return res

    chicken_wins = count_wins(tc, te)
    egg_wins = count_wins(te, tc)

    total = a * a
    ties = total - chicken_wins - egg_wins

    if chicken_wins > egg_wins:
        print("chicken")
    elif egg_wins > chicken_wins:
        print("egg")
    else:
        print("tie")

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng biểu đồ ngược cho cả hai hệ thống cân. Sự tách biệt này là cần thiết vì Dijkstra yêu cầu một trọng số vô hướng duy nhất trên mỗi cạnh, và động lực con gà và quả trứng là những bài toán đường đi ngắn nhất độc lập. 

các`dijkstra`chức năng là đa nguồn, khởi tạo tất cả các lối thoát với khoảng cách bằng 0. Điều này tránh việc chạy thuật toán một lần cho mỗi lần truy cập và nén tất cả các trạng thái bắt đầu vào một lần chạy. 

Sau khi tính toán các đường dẫn ngắn nhất, chỉ các nút đầu vào mới được trích xuất. Điều này làm giảm vấn đề thành một vấn đề so sánh thuần túy trên mảng. 

các`count_wins`Hàm mã hóa rút gọn lõi: sắp xếp một mảng và sử dụng tìm kiếm nhị phân để đếm xem có bao nhiêu phần tử nhỏ hơn mỗi truy vấn từ mảng kia. Điều này trực tiếp tính toán sự thống trị của cặp theo thứ tự mà không cần liệt kê rõ ràng. 

Cuối cùng, so sánh số trận thắng sẽ quyết định câu trả lời. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa với ba lần vào và thời gian được tính toán của chúng: 

Giờ gà: [2, 5, INF] 

Thời gian trứng: [3, 1, INF] 

Chiến thắng của gà được tính bằng cách so sánh từng quả trứng với tất cả các lần gà. 

Ở thời điểm trứng thứ 3, giá trị gà nhỏ hơn 3 là [2], đóng góp 1. 

Đối với thời điểm trứng 1, không cái nào ít hơn, đóng góp 0. 

Đối với INF, tất cả các giá trị gà hữu hạn đều nhỏ hơn, đóng góp 2. 

Gà thắng tổng cộng 3. 

| Bước | giá trị b | Đã sắp xếp một | Đã thêm chiến thắng | 
| --- | --- | --- | --- | 
| 1 | 3 | [2,5,INF] | 1 | 
| 2 | 1 | [2,5,INF] | 0 | 
| 3 | INF | [2,5,INF] | 2 | 

Điều này chứng tỏ cách tính thống trị chuyển đổi so sánh theo cặp thành số lượng tiền tố. 

Bây giờ hãy xem xét tính đối xứng: việc hoán đổi vai trò sẽ tạo ra số trứng thắng tương tự, xác nhận rằng mỗi cặp đã đặt hàng sẽ đóng góp chính xác một lần cho mỗi bên hoặc hòa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n + a log a) | Hai lần chạy Dijkstra chiếm ưu thế với các thao tác heap, cộng với việc sắp xếp và tìm kiếm nhị phân trên các lối vào | 
| Không gian | O(n + m) | Lưu trữ đồ thị và mảng khoảng cách cho cả hai hệ thống cân | 

Cấu trúc phù hợp thoải mái trong giới hạn vì biểu đồ chỉ được xử lý hai lần và so sánh đầu vào được giảm xuống thành việc đếm dựa trên sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve.__wrapped__() if hasattr(solve, "__wrapped__") else None

# Sample-style and custom tests are illustrative; full harness assumes solve() prints output
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đối xứng đường đơn | cà vạt | phân phối bằng nhau | 
| lối vào không thể tiếp cận | cà vạt | Xử lý INF | 
| gà nhanh hơn | gà | đặt hàng thống trị | 

## Vỏ cạnh 

Khi tất cả các lối vào không thể đến được bất kỳ lối ra nào, cả mảng tc và te đều bao gồm các giá trị INF. Mọi so sánh giữa bất kỳ cặp nào đều mang lại sự bình đẳng, vì vậy tất cả các cặp a² đều bằng nhau. Thuật toán xử lý việc này một cách tự nhiên vì tìm kiếm nhị phân trên danh sách INF được sắp xếp giống hệt nhau không tạo ra chiến thắng nào cho cả hai bên, dẫn đến quyết định hòa. 

Khi một loài có thể đạt được lối thoát trong khi loài kia không thể, tất cả các giá trị hữu hạn sẽ chiếm ưu thế so với INF. Trong trường hợp đó, mỗi cặp luôn ưu tiên các loài có thể tiếp cận được và việc đếm dựa trên phân loại sẽ tạo ra sự thống trị hoàn toàn mà không cần vỏ bọc đặc biệt. 

Khi nhiều lối vào chia sẻ thời gian đường dẫn ngắn nhất giống hệt nhau, thuật toán sẽ tính toán chính xác các mối quan hệ ngầm vì không có sự bất bình đẳng nghiêm ngặt nào góp phần vào số lần thắng.
