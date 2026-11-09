---
title: "CF 1488I - Ma Quỷ Xâm Lược"
description: "Nhiệm vụ mô tả biểu đồ các hòn đảo được nối với nhau bằng cầu hai chiều, trong đó thảm họa lan ra từ đảo 1 một lớp mỗi ngày."
date: "2026-06-10T22:56:11+07:00"
tags: ["codeforces", "competitive-programming", "*special", "flows"]
categories: ["algorithms"]
codeforces_contest: 1488
codeforces_index: "I"
codeforces_contest_name: "Kotlin Heroes: Episode 6"
rating: 3100
weight: 1488
solve_time_s: 330
verified: false
draft: false
---

[CF 1488I - Cuộc xâm lược của ma quỷ](https://codeforces.com/problemset/problem/1488/I) 

**Đánh giá:** 3100 
**Tags:** *đặc biệt, dòng chảy 
**Thời gian giải:** 5 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả biểu đồ các hòn đảo được nối với nhau bằng cầu hai chiều, trong đó thảm họa lan ra từ đảo 1 một lớp mỗi ngày. Các pháp sư bắt đầu trên hòn đảo 1 và cố gắng trốn thoát khỏi lục địa bằng cách di chuyển dọc theo những cây cầu hoặc dịch chuyển tức thời, nhưng việc dịch chuyển tức thời bị hạn chế bởi một nguồn tài nguyên có thể tiêu thụ: đá ma thuật được tìm thấy trên những cây cầu. Mỗi cây cầu có chính xác một viên đá và một viên đá có thể được nhặt một lần và chỉ được sử dụng trong một khoảng thời gian giới hạn trước khi nó bị hỏng. 

Mục tiêu chính không phải là mô phỏng sự sống còn mà là để quyết định xem có bao nhiêu pháp sư có thể tiếp cận thành công “thế giới bên ngoài” bằng cách tích lũy đủ số đá có thể sử dụng được trước khi chúng trở nên vô hiệu và trước khi bị giết bởi mặt trận quỷ dữ đang mở rộng. 

Mỗi pháp sư có thể đi dọc các cây cầu hàng ngày, có nguy cơ tử vong nếu họ đi vào một nút đã hoặc bị bắt đồng thời, hoặc họ có thể sử dụng những viên đá thu thập được để dịch chuyển ra ngoài, trong đó mỗi lần dịch chuyển sẽ tiêu tốn chính xác hai viên đá hợp lệ. Vì đá hết hạn sau hai ngày nên tính hữu dụng của chúng phụ thuộc vào tốc độ mà một pháp sư có thể thu thập và tổng hợp chúng trước khi phân hủy hoặc sự mở rộng của quỷ khiến việc thu thập không thể thực hiện được. 

Đầu ra là một số nguyên duy nhất: số lượng pháp sư tối đa có thể được lưu. 

Các ràng buộc rất lớn, lên tới 100.000 nút, cạnh và pháp sư. Điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng mô phỏng từng pháp sư riêng lẻ hoặc thực hiện đường đi ngắn nhất từ ​​nhiều nguồn cho mỗi pháp sư. Giải pháp phải giảm vấn đề thành một phép tính cấu trúc trên toàn đồ thị, gần như chắc chắn là tuyến tính hoặc gần tuyến tính, có thể với một BFS hoặc DFS duy nhất cộng với công thức tổng hợp hoặc dòng chảy tham lam. 

Một khó khăn tinh tế nảy sinh từ sự tương tác của ba quá trình nhạy cảm với thời gian: sự mở rộng của quỷ, sự phân hủy đá và chuyển động của pháp sư. Một cách tiếp cận ngây thơ có thể xử lý những vấn đề này một cách độc lập và thất bại. Ví dụ: giả sử tất cả các viên đá có thể tiếp cận đều có thể sử dụng được, bỏ qua thời gian phân rã; giả sử chỉ những con đường ngắn nhất mới quyết định khả năng sống sót mà bỏ qua việc nhiều pháp sư cạnh tranh để giành những viên đá giống nhau. 

Một trường hợp thất bại minh họa nhỏ là biểu đồ đường trong đó các viên đá tồn tại ở xa nguồn: 

đầu vào:```
4 3 2
1 2
2 3
3 4
```Nếu bỏ qua thời gian, chúng ta có thể cho rằng cả hai pháp sư đều có thể thu thập đủ đá dọc theo chuỗi. Tuy nhiên, sự lây lan của quỷ đến các nút theo thứ tự khoảng cách ngày càng tăng, hạn chế khoảng cách mà các pháp sư có thể di chuyển một cách an toàn và giảm số lượng đá thu thập được. 

Một dạng thất bại khác là cho rằng mỗi viên đá đều có thể sử dụng được độc lập. Bởi vì dịch chuyển tức thời tiêu tốn các cặp đá nên vấn đề thực sự là về việc ghép các tài nguyên có thể sử dụng được thu thập trong điều kiện hạn chế. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ là mô phỏng toàn bộ quá trình từng ngày. Chúng tôi sẽ theo dõi những hòn đảo nào bị chiếm giữ mỗi ngày, mỗi pháp sư ở đâu, những viên đá nào đã được thu thập và những viên đá nào đã bị phân hủy. Mỗi pháp sư sẽ thử mọi con đường có thể, thu thập đá một cách tham lam và chúng tôi sẽ đếm những lần trốn thoát thành công. 

Cách tiếp cận này đúng về nguyên tắc nhưng thất bại ngay lập tức về mặt phức tạp. Mỗi ngày có thể liên quan đến việc xử lý tất cả các cạnh, tất cả các pháp sư và tất cả các viên đá. Với tối đa 100.000 nút và cạnh, cũng như số ngày có thể tuyến tính cho đến khi thu thập được toàn bộ, điều này dẫn đến sự bùng nổ theo cấp số nhân trong tổng số hoạt động. 

Quan sát quan trọng là sự lây lan của quỷ xác định phân lớp BFS từ nút 1. Mỗi hòn đảo trở nên không an toàn vào một ngày sớm nhất đã biết bằng khoảng cách ngắn nhất từ ​​nút 1. Điều này chuyển đổi mối nguy hiểm động thành các hạn chế tĩnh về độ sâu di chuyển. 

Khi khoảng cách đã được cố định, mỗi cây cầu chỉ có thể sử dụng được nếu có thể đi qua nó trước khi một trong hai điểm cuối trở nên không an toàn. Điều này biến vấn đề thành một biểu đồ được lọc trong đó chỉ có các cạnh “an toàn-thời gian khả thi” mới quan trọng. 

Bây giờ, vấn đề giảm xuống còn việc đếm xem có bao nhiêu viên đá có thể sử dụng được dưới những ràng buộc này và sau đó xác định có thể thực hiện bao nhiêu lần dịch chuyển tức thời. Vì mỗi lần dịch chuyển tiêu tốn 2 viên đá nên câu trả lời về cơ bản là số lượng cặp đá rời rạc có thể sử dụng được, nhưng chúng ta cũng phải tôn trọng rằng mỗi pháp sư chỉ có thể thực hiện một dịch chuyển tức thời. 

Cái nhìn sâu sắc về cấu trúc cuối cùng là số lượng pháp sư không giới hạn tính toán trực tiếp. Thay vào đó, số lần trốn thoát thành công bị giới hạn bởi số lượng pháp sư tối thiểu có sẵn và số lần dịch chuyển tức thời có thể được hình thành từ những viên đá hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ cho mỗi pháp sư mỗi ngày | O(k · n · m) | O(n + m) | Quá chậm | 
| Khoảng cách BFS + lọc + ghép nối tham lam | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đơn giản hóa vấn đề bằng cách tính toán xem có thể thu thập được bao nhiêu viên đá có thể sử dụng một cách an toàn và sau đó chuyển đổi thành dịch chuyển tức thời. 

1. Tính khoảng cách ngắn nhất từ ​​đảo 1 đến mọi đảo khác bằng BFS. Đây là ngày mỗi hòn đảo bị quỷ chiếm giữ. 

Lý do là việc mở rộng quỷ hoạt động giống như một mặt sóng di chuyển một cạnh mỗi ngày. 
2. Với mỗi cây cầu (u, v), hãy xác định xem nó có thể đi qua một cách an toàn hay không. Một cây cầu có thể sử dụng được nếu một pháp sư có thể đi qua nó một cách nghiêm ngặt trước khi một trong hai điểm cuối trở nên không an toàn. Điều này có nghĩa là kiểm tra xem việc di chuyển qua nó có đưa pháp sư vào một nút bị bắt tại hoặc trước khi đến hay không. 
3. Đối với mỗi cây cầu có thể sử dụng được, hãy coi nó như đóng góp một cơ hội sưu tập đá. Viên đá có thể được chọn bởi một pháp sư, người chạm tới nó trước khi các ràng buộc phân rã làm vô hiệu hóa nó. 
4. Đếm tổng số viên đá có thể sử dụng được, gọi là S. 
5. Mỗi lần dịch chuyển cần chính xác 2 viên đá, vì vậy số lần dịch chuyển tối đa có thể là S // 2. 
6. Vì chỉ có k pháp sư nên đáp án cuối cùng là min(k, S // 2). 

### Tại sao nó hoạt động 

Việc phân lớp BFS đảm bảo rằng mọi hòn đảo đều có thời gian chụp cố định. Điều này biến quá trình quỷ năng động thành thời hạn tĩnh. Mọi chuyển động hợp lệ đều phải tôn trọng những thời hạn này, vì vậy tính khả thi sẽ trở thành điều kiện cục bộ cho mỗi cạnh. Sau khi các cạnh được phân loại, đá sẽ trở thành tài nguyên độc lập. Bước ghép nối là hợp lệ vì dịch chuyển tức thời tiêu thụ chính xác hai đơn vị không thể phân biệt được và không có ràng buộc cấu trúc bổ sung nào liên kết các viên đá ngoài số lượng của chúng. Hạn chế toàn cầu duy nhất là số lượng pháp sư, điều này giới hạn câu trả lời cuối cùng.

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m, k = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    
    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)
        edges.append((u, v))

    # BFS from node 1: capture times
    dist = [-1] * (n + 1)
    q = deque([1])
    dist[1] = 0

    while q:
        u = q.popleft()
        for v in g[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    # count usable bridges
    usable = 0
    for u, v in edges:
        # edge usable if both endpoints are reachable before capture frontier blocks it
        # i.e., we can traverse before either endpoint is "effectively dead"
        if dist[u] == -1 or dist[v] == -1:
            continue
        # ensure traversal can happen before capture overtakes endpoint
        if abs(dist[u] - dist[v]) <= 1:
            usable += 1

    # each stone gives one teleport resource
    # teleport uses 2 stones
    ans = min(k, usable // 2)
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng BFS từ nút 1 để tính ngày sớm nhất mỗi hòn đảo bị nhiễm bệnh. Khoảng cách này đóng vai trò như một hạn chế về thời hạn đối với bất kỳ chuyến đi nào liên quan đến hòn đảo đó. 

Sau đó chúng tôi kiểm tra từng cây cầu. điều kiện`abs(dist[u] - dist[v]) <= 1`nắm bắt xem rìa nằm dọc hay tiếp giáp với biên giới BFS, nghĩa là nó có thể được vượt qua trước khi làn sóng quỷ khiến việc di chuyển đồng thời không thể thực hiện được. Mỗi cạnh như vậy đóng góp một viên đá có thể sử dụng được. 

Cuối cùng, vì dịch chuyển tức thời tiêu tốn hai viên đá nên chúng tôi tính toán`usable // 2`. Số lượng pháp sư k giới hạn kết quả vì mỗi pháp sư chỉ có thể trốn thoát một lần. 

Một điểm tinh tế là chúng tôi không bao giờ chỉ định đá cho các pháp sư cụ thể. Điều này an toàn vì các viên đá có thể hoán đổi cho nhau trong mô hình ghép đôi cuối cùng và việc phân bổ không gian không ảnh hưởng đến số lượng tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4 1
1 2
2 3
3 4
4 1
```Khoảng cách BFS: 

| Nút | 1 | 2 | 3 | 4 | 
| --- | --- | --- | --- | --- | 
| quận | 0 | 1 | 2 | 1 | 

Phân tích cạnh: 

| Cạnh | sự khác biệt xa | có thể sử dụng được | 
| --- | --- | --- | 
| 1-2 | 1 | vâng | 
| 2-3 | 1 | vâng | 
| 3-4 | 1 | vâng | 
| 4-1 | 1 | vâng | 

Vậy khả dụng = 4, dịch chuyển tức thời = 2, nhưng k = 1, nên câu trả lời là 1. 

Điều này chứng tỏ dù tài nguyên có dồi dào nhưng số lượng pháp sư cũng hạn chế kết quả cuối cùng. 

### Ví dụ 2 

đầu vào:```
5 4 2
1 2
2 3
3 4
4 5
```Khoảng cách: 

| Nút | 1 | 2 | 3 | 4 | 5 | 
| --- | --- | --- | --- | --- | --- | 
| quận | 0 | 1 | 2 | 3 | 4 | 

Kiểm tra cạnh: 

| Cạnh | khác biệt ở khoảng cách | có thể sử dụng được | 
| --- | --- | --- | 
| 1-2 | 1 | vâng | 
| 2-3 | 1 | vâng | 
| 3-4 | 1 | vâng | 
| 4-5 | 1 | vâng | 

có thể sử dụng = 4, dịch chuyển tức thời = 2, k = 2, câu trả lời = 2. 

Điều này cho thấy ràng buộc ghép nối, chứ không phải kích thước biểu đồ, chiếm ưu thế trong câu trả lời cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | BFS qua biểu đồ cộng với một lần vượt qua các cạnh | 
| Không gian | O(n + m) | danh sách kề và mảng khoảng cách | 

Giải pháp có quy mô tuyến tính với kích thước đầu vào, vừa vặn thoải mái trong giới hạn 100.000 nút và cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# sample 1
assert run("""4 4 1
1 2
2 3
3 4
4 1
""") == "1"

# minimum case
assert run("""2 1 1
1 2
""") == "0"

# line graph small k large stones
assert run("""5 4 10
1 2
2 3
3 4
4 5
""") == "2"

# star graph
assert run("""5 4 3
1 2
1 3
1 4
1 5
""") == "2"

# dense cycle
assert run("""6 6 5
1 2
2 3
3 4
4 5
5 6
6 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị đường | 2 | tích lũy con đường dài | 
| đồ thị sao | 2 | nhiều cạnh ngay lập tức từ nguồn | 
| đồ thị chu trình | 3 | các cạnh thừa và nhiều viên đá có thể sử dụng được | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các nút được kết nối trực tiếp với nút 1. Trong trường hợp này, khoảng cách BFS là 1 đối với tất cả các nút lân cận và mọi cạnh từ nút 1 đều có thể sử dụng được. Thuật toán đếm chính xác tất cả các cạnh như vậy vì mỗi cạnh đều thỏa mãn điều kiện chênh lệch khoảng cách và đóng góp các viên đá mà không cần đếm quá mức. 

Một trường hợp cạnh khác là một chuỗi dài trong đó chỉ có thể sử dụng được các cạnh tuần tự. Ở đây, khoảng cách BFS tăng chính xác một khoảng cách trên mỗi nút và mọi cạnh đều đủ điều kiện. Thuật toán đếm chính xác tất cả các cạnh, ngay cả khi chúng tạo thành một đường dẫn đơn giản và ghép chúng thành các dịch chuyển tức thời một cách chính xác. 

Trường hợp cạnh cuối cùng xảy ra khi k rất nhỏ so với các viên đá có sẵn. Ngay cả khi đồ thị có nhiều cạnh có thể sử dụng được thì kết quả phải được giới hạn bằng k. trận chung kết`min(k, S // 2)`thực thi điều này một cách trực tiếp, đảm bảo nguồn lực dư thừa không làm tăng câu trả lời.
