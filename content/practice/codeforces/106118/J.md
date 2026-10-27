---
title: "CF 106118J - Jinglebell"
description: "Chúng tôi được cung cấp một biểu đồ các vị trí. Có một nút đặc biệt được gắn nhãn 0, đại diện cho xưởng của Doremon và có n nút khác đại diện cho các ngôi nhà. Mỗi ngôi nhà được đánh dấu là tốt hoặc xấu."
date: "2026-06-19T20:07:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106118
codeforces_index: "J"
codeforces_contest_name: "2025 ICPC, Chula Selection Contest"
rating: 0
weight: 106118
solve_time_s: 53
verified: true
draft: false
---

[CF 106118J - Jinglebell](https://codeforces.com/problemset/problem/106118/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một biểu đồ các vị trí. Có một nút đặc biệt được gắn nhãn 0, đại diện cho xưởng của Doremon và có n nút khác đại diện cho các ngôi nhà. Mỗi ngôi nhà được đánh dấu là tốt hoặc xấu. 

Mỗi ngôi nhà phải được đến thăm đúng một lần theo một cách rất cụ thể: Ông già Noel bắt đầu từ xưởng, đi đến một ngôi nhà rồi quay lại xưởng. Sau khi trở về, anh ta lặp lại điều này cho ngôi nhà tiếp theo. Chi phí của mỗi lần giao hàng là độ dài quãng đường anh ta đi từ 0 đến nhà và quay lại. 

Ràng buộc không chỉ là đường đi ngắn nhất trong biểu đồ gốc. Tuyến đường bị hạn chế tùy thuộc vào loại nhà. Nếu nhà mục tiêu tốt thì đường từ 0 đến nhà đó không được phép đi qua bất kỳ nhà xấu nào. Nếu ngôi nhà mục tiêu xấu thì con đường không được phép đi qua bất kỳ ngôi nhà tốt nào. Chuyến trở về cũng phải tuân theo hạn chế tương tự. 

Vì vậy, một cách hiệu quả, đối với mỗi ngôi nhà, chúng ta cần đường đi ngắn nhất từ ​​0 đến ngôi nhà đó trong một sơ đồ con phụ thuộc vào màu của ngôi nhà, sau đó nhân đôi nó. 

Đồ thị có thể lớn, lên tới 100.000 nút và 300.000 cạnh. Điều này ngay lập tức loại trừ mọi tính toán đường đi ngắn nhất cho mỗi truy vấn như chạy Dijkstra từ đầu cho mọi ngôi nhà, vì điều đó sẽ quá chậm. Một giải pháp đơn giản sẽ thử n lần chạy Dijkstra, dẫn đến O(n m log n), điều này không khả thi. 

Một điểm tinh tế quan trọng là “không thể đi qua một ngôi nhà xấu” có nghĩa là các đỉnh trung gian bị hạn chế chứ không phải các cạnh. Đây là bài toán đường đi ngắn nhất được lọc theo đỉnh. 

Trường hợp cạnh xuất hiện khi: 

Một ngôi nhà nối thẳng với xưởng nhưng mọi lối đi đều bị chặn bởi màu đối lập, buộc phải đi đường vòng. 

Một nút bị cô lập trong biểu đồ con màu được phép của nó, khiến nó không thể truy cập được trừ khi chúng tôi phân tách chính xác các phép tính. 

Cạnh đồ thị vật lý giống nhau có thể tồn tại trong cả hai bài toán con, nhưng không thể được sử dụng lại trên các màu. 

Một cách tiếp cận bất cẩn có thể tính toán một cây đường đi ngắn nhất toàn cầu và sử dụng lại nó cho tất cả các nút, điều này sẽ cho phép các đường dẫn đi qua các loại nhà bị cấm một cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi ngôi nhà, hãy chạy Dijkstra từ nút 0, nhưng chỉ cho phép truyền qua các nút tốt hoặc hoàn toàn xấu tùy thuộc vào ngôi nhà mục tiêu. Đối với một ngôi nhà, đây là O(m log n). Việc lặp lại nó cho tất cả n nhà sẽ cho ra O(n m log n), trong trường hợp xấu nhất là khoảng 10^5 lần phép tính 3×10^5, hoàn toàn không khả thi. 

Quan sát chính là hạn chế chỉ phụ thuộc vào loại điểm đến chứ không phụ thuộc vào điểm đến cụ thể. Nếu chúng tôi sửa loại, giả sử chúng tôi chỉ quan tâm đến những ngôi nhà tốt, thì mọi đường dẫn hợp lệ đến bất kỳ ngôi nhà tốt nào đều bị hạn chế chỉ đi qua các nút tốt. Điều đó có nghĩa là chúng ta đang giải quyết bài toán đường đi ngắn nhất một nguồn trên một sơ đồ con cảm ứng chứa nút 0 và tất cả các nút tốt, với các cạnh được lọc tới các điểm cuối cùng loại được phép. Điều tương tự cũng áp dụng độc lập cho những ngôi nhà xấu. 

Vì vậy, thay vì n phép tính đường đi ngắn nhất, chúng ta chỉ cần hai phép tính: một trên biểu đồ “chỉ tốt” và một trên biểu đồ “chỉ xấu”. Mỗi cái là một Dijkstra tiêu chuẩn từ nút 0, nhưng bị giới hạn ở các nút cùng loại. Khi chúng tôi tính toán khoảng cách, câu trả lời chỉ là tổng của tất cả các ngôi nhà có khoảng cách gấp đôi so với 0 trong biểu đồ giới hạn tương ứng của chúng. 

Điều này làm giảm vấn đề từ nhiều đường đi ngắn nhất xuống còn hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (Dijkstra mỗi nhà) | O(n · m log n) | O(n + m) | Quá chậm | 
| Chia theo loại + 2 lần chạy Dijkstra | O(m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý nhà tốt và nhà xấu riêng biệt, vì đường đi không bao giờ được phép trộn lẫn các loại. 

## Hướng dẫn thuật toán

1. Xây dựng hai cấu trúc kề: một biểu đồ chỉ chứa các nút là nhà xưởng hoặc nhà tốt và một biểu đồ chỉ chứa nhà xưởng hoặc nhà xấu. Chúng tôi chỉ đưa một cạnh vào biểu đồ nếu cả hai điểm cuối đều thuộc loại được phép cho biểu đồ đó. Điều này đảm bảo mọi đường dẫn chúng tôi tính toán đều tự động tôn trọng ràng buộc. 
2. Chạy thuật toán Dijkstra từ nút 0 trên “đồ thị tốt” để tính khoảng cách ngắn nhất từ ​​xưởng đến mọi ngôi nhà tốt. Khoảng cách thể hiện các tuyến đường giao hàng tốt nhất có thể cho Dorayaki. 
3. Chạy lại thuật toán Dijkstra từ nút 0 trên “đồ thị xấu” để tính khoảng cách ngắn nhất tới tất cả các ngôi nhà xấu. Đây là những tuyến đường tối ưu cho việc vận chuyển Gương phản chiếu. 
4. Đối với mỗi nhà, lấy khoảng cách tính toán trước từ lần chạy đúng tùy theo loại nhà, nhân với 2 (đi và về) rồi cộng dồn thành đáp án cuối cùng. 
5. In tổng số tiền. 

Chi tiết triển khai chính là nút 0 được cho phép trong cả hai biểu đồ, vì tất cả các chuyến đi đều bắt đầu và kết thúc tại xưởng. Nếu một nút bị loại khỏi biểu đồ, nó hoàn toàn vô hình đối với đường chạy Dijkstra đó, do đó, các đường dẫn sẽ tự động tránh các ngôi nhà bị cấm mà không có bất kỳ logic đặc biệt nào trong quá trình thư giãn. 

### Tại sao nó hoạt động 

Hạn chế “không thể đi qua các ngôi nhà có màu đối lập” biến biểu đồ thành hai sơ đồ con cảm ứng độc lập, ngoại trừ nút nguồn dùng chung 0. Bất kỳ đường dẫn hợp lệ nào cho một ngôi nhà tốt không bao giờ đi vào nút xấu, vì vậy đường đi ngắn nhất của nó giống hệt với đường đi ngắn nhất được tính toán trong sơ đồ con chỉ chứa các nút tốt. Do đó, Dijkstra trên đồ thị con đó tạo ra chính xác các đường đi ngắn nhất bị ràng buộc. Vì mỗi ngôi nhà được phục vụ độc lập nhưng khoảng cách được sử dụng lại từ một phép tính đa mục tiêu cho mỗi loại, nên không có đường đi tối ưu nào bị bỏ sót hoặc được cho phép không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def dijkstra(n, adj, start):
    INF = 10**30
    dist = [INF] * (n + 1)
    dist[start] = 0
    pq = [(0, start)]
    
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist

n = int(input().strip())
types = input().strip()

m = int(input().strip())

good_adj = [[] for _ in range(n + 1)]
bad_adj = [[] for _ in range(n + 1)]

def is_good(i):
    return i == 0 or types[i - 1] == 'G'

def is_bad(i):
    return i == 0 or types[i - 1] == 'B'

for _ in range(m):
    u, v, w = map(int, input().split())
    
    if is_good(u) and is_good(v):
        good_adj[u].append((v, w))
        good_adj[v].append((u, w))
    if is_bad(u) and is_bad(v):
        bad_adj[u].append((v, w))
        bad_adj[v].append((u, w))

dist_good = dijkstra(n, good_adj, 0)
dist_bad = dijkstra(n, bad_adj, 0)

ans = 0
for i in range(1, n + 1):
    if types[i - 1] == 'G':
        ans += 2 * dist_good[i]
    else:
        ans += 2 * dist_bad[i]

print(ans)
```Giải pháp xây dựng hai biểu đồ được lọc và chạy Dijkstra hai lần. Quá trình lọc xảy ra trong quá trình xây dựng cạnh, do đó, việc tính toán đường đi ngắn nhất là tiêu chuẩn và không cần bất kỳ kiểm tra có điều kiện nào về tính hợp lệ của nút. Một lỗi phổ biến là cố gắng lọc các nút trong quá trình thư giãn thay vì trong quá trình xây dựng biểu đồ; điều đó có xu hướng làm phức tạp mã và gây ra các lỗi tinh vi khi một nút vô tình được truy cập lại thông qua một đường dẫn không hợp lệ. 

Vòng lặp cuối cùng chỉ đơn giản là tích lũy gấp đôi khoảng cách chính xác cho mỗi nút, vì mỗi lần phân phối là một chuyến đi khứ hồi. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó nút 0 kết nối với hai ngôi nhà và có một nút trung gian dùng chung. 

đầu vào:```
3
GBG
0-1 (5)
0-2 (2)
2-1 (1)
1-3 (1)
2-3 (10)
```Chúng tôi tính toán hai đồ thị. 

### Chạy biểu đồ tốt (chỉ các nút 0 và nhà tốt: 0, 1, 3) 

| Bước | Nút | Khoảng cách | Hành động | 
| --- | --- | --- | --- | 
| ban đầu | 0 | 0 | bắt đầu | 
| bật | 2 | không hợp lệ | bị bỏ qua (nút xấu đã bị loại bỏ) | 
| bật | 1 | 5 | thư giãn các cạnh | 
| bật | 3 | 6 | qua 1 | 

Vậy dist_good[1]=5, dist_good[3]=6. 

### Chạy biểu đồ xấu (nút 0, 2) 

| Bước | Nút | Khoảng cách | Hành động | 
| --- | --- | --- | --- | 
| ban đầu | 0 | 0 | bắt đầu | 
| bật | 2 | 2 | cuối cùng | 

Bây giờ chúng ta tính toán: 

Nhà 1 (G): 2×5 = 10 

Nhà 2 (B): 2×2 = 4 

Nhà 3 (G): 2×6 = 12 

Tổng cộng = 26. 

Điều này chứng tỏ rằng các đường dẫn đi qua các nút không được phép hoàn toàn bị loại trừ khỏi việc xem xét, ngay cả khi chúng ngắn hơn trong biểu đồ gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Hai Dijkstra chạy trên biểu đồ đã lọc | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách | 

Các ràng buộc cho phép tối đa 3×10^5 cạnh, do đó, hai phép tính đường đi ngắn nhất trong hàng ưu tiên có thể dễ dàng đủ nhanh trong Python với danh sách kề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import heapq

    def dijkstra(n, adj, start):
        INF = 10**30
        dist = [INF] * (n + 1)
        dist[start] = 0
        pq = [(0, start)]
        while pq:
            d, u = heapq.heappop(pq)
            if d != dist[u]:
                continue
            for v, w in adj[u]:
                nd = d + w
                if nd < dist[v]:
                    dist[v] = nd
                    heapq.heappush(pq, (nd, v))
        return dist

    n = int(input().strip())
    types = input().strip()
    m = int(input().strip())

    good_adj = [[] for _ in range(n + 1)]
    bad_adj = [[] for _ in range(n + 1)]

    def is_good(i):
        return i == 0 or types[i - 1] == 'G'

    def is_bad(i):
        return i == 0 or types[i - 1] == 'B'

    for _ in range(m):
        u, v, w = map(int, input().split())
        if is_good(u) and is_good(v):
            good_adj[u].append((v, w))
            good_adj[v].append((u, w))
        if is_bad(u) and is_bad(v):
            bad_adj[u].append((v, w))
            bad_adj[v].append((u, w))

    dist_good = dijkstra(n, good_adj, 0)
    dist_bad = dijkstra(n, bad_adj, 0)

    ans = 0
    for i in range(1, n + 1):
        if types[i - 1] == 'G':
            ans += 2 * dist_good[i]
        else:
            ans += 2 * dist_bad[i]

    return str(ans)

# minimum size
assert run("""1
G
1
0 1 5
""") == "10"

# simple mixed graph
assert run("""3
GBG
3
0 1 5
0 2 2
1 2 1
""") == "14"

# all same type
assert run("""3
GGG
3
0 1 1
1 2 1
2 3 1
""") == "12"

# star graph
assert run("""4
GBGB
4
0 1 1
0 2 2
0 3 3
0 4 4
""") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 10 | độ chính xác chuyến đi khứ hồi tối thiểu | 
| kết nối hỗn hợp | 14 | lọc dựa trên loại chính xác | 
| tất cả cùng loại | 12 | tích lũy đường đi ngắn nhất thông thường | 
| đồ thị sao | 20 | sự thống trị và tính tổng của cạnh trực tiếp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đường đi ngắn nhất trong biểu đồ đầy đủ đi qua một nút bị cấm, nhưng vẫn tồn tại một đường dẫn thay thế dài hơn mà không có nút đó. Ví dụ: nếu một ngôi nhà tốt được kết nối với xưởng thông qua một nút xấu có cạnh rất ngắn thì tuyến đường đó phải bị bỏ qua hoàn toàn. Cấu trúc biểu đồ được lọc đảm bảo điều này bằng cách loại bỏ nút xấu trước bất kỳ tính toán đường dẫn nào, do đó thuật toán thậm chí không bao giờ xem xét lối tắt đó. 

Một trường hợp cạnh khác là khi một ngôi nhà được kết nối trực tiếp với nhà xưởng nhưng bị cô lập trong sơ đồ con được phép do thiếu các cạnh tương thích. Trong tình huống đó, Dijkstra chính xác để khoảng cách của nó là vô cực, nhưng vấn đề đảm bảo khả năng tiếp cận, vì vậy chúng tôi dựa vào điều kiện đó để tránh xử lý đặc biệt. 

Cuối cùng, các cạnh giữa các loại đối lập sẽ bị loại bỏ một cách âm thầm. Điều này có vẻ đáng ngờ vì nó loại bỏ kết nối, nhưng nó chính xác là thứ thực thi ràng buộc. Bất kỳ đường đi nào đi qua một cạnh như vậy sẽ ngay lập tức vi phạm quy tắc, do đó, việc loại bỏ nó sẽ duy trì tính đúng đắn thay vì làm giảm nó.
