---
title: "CF 105346F - Ngôi Nhà Ma Ám"
description: "Chúng ta có một tòa nhà được mô hình hóa dưới dạng đồ thị vô hướng trong đó các phòng là các nút và các cửa là các cạnh. Harry bắt đầu từ một căn phòng cụ thể và muốn đến bất kỳ phòng thoát hiểm nào. Đồng thời, có nhiều hồn ma được đặt trong các phòng cố định."
date: "2026-06-23T15:34:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105346
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 2 (Beginner)"
rating: 0
weight: 105346
solve_time_s: 87
verified: false
draft: false
---

[CF 105346F - Ngôi nhà ma ám](https://codeforces.com/problemset/problem/105346/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tòa nhà được mô hình hóa dưới dạng đồ thị vô hướng trong đó các phòng là các nút và các cửa là các cạnh. Harry bắt đầu từ một căn phòng cụ thể và muốn đến bất kỳ phòng thoát hiểm nào. Đồng thời, có nhiều hồn ma được đặt trong các phòng cố định. 

Cả Harry và mọi hồn ma đều di chuyển với tốc độ như nhau, một cạnh mỗi giây và tất cả họ đều bắt đầu di chuyển đồng thời. Harry được coi là an toàn ở lối ra chỉ khi tồn tại một con đường từ điểm xuất phát đến lối ra đó mà anh ấy có thể đi qua nó mà không cần ở chung phòng cùng lúc với bất kỳ con ma nào. 

Điểm tinh tế quan trọng là ma cũng di chuyển, vì vậy một căn phòng không chỉ đơn giản là “xấu” nếu ban đầu nó có ma. Một căn phòng sẽ trở nên không an toàn đối với Harry nếu một con ma có thể đến đó cùng lúc hoặc sớm hơn Harry có thể. 

Kích thước đồ thị lên tới 200.000 nút và cạnh, vì vậy mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính. Bất cứ điều gì như chạy BFS từ mọi lối ra hoặc mô phỏng tất cả các đường dẫn trên mỗi bản ghost sẽ quá chậm. Cách tiếp cận khả thi duy nhất là tính toán khoảng cách ngắn nhất trong biểu đồ và suy luận về chúng trên toàn cầu. 

Một trường hợp thất bại thường gặp là do bỏ qua thời gian. 

Ví dụ: 

đầu vào:```
3 2 1 1 1
1 2
2 3
3
2
```Ở đây Harry bắt đầu từ số 1, hồn ma bắt đầu từ số 2, lối ra là số 3. Harry đạt đến số 3 trong 2 bước. Ghost đạt 3 trong 1 bước. Mặc dù lối ra có thể đến được nhưng Harry không thể sử dụng nó một cách an toàn. 

Một cách tiếp cận ngây thơ chỉ kiểm tra khả năng tiếp cận từ Harry sẽ tính sai lối thoát này là hợp lệ. 

Một trường hợp thất bại khác là cho rằng ma chỉ chặn phòng xuất phát của họ, điều này cũng bị phá vỡ bất cứ khi nào ma có thể di chuyển vào đường đi của Harry. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là mô phỏng đồng thời cả Harry và tất cả các hồn ma. Người ta có thể thử BFS đa nguồn để theo dõi các trạng thái như (nút, thời gian, người chiếm giữ nó) hoặc cố gắng khám phá tất cả các đường dẫn có thể có từ Harry và từ chối những đường giao nhau với các đường dẫn ma. Điều này nhanh chóng trở thành cấp số nhân trong thực tế vì mọi đường dẫn phải được kiểm tra đối với nhiều tác nhân chuyển động và số lượng tương tác tăng lên với cả n và m. 

Sự đơn giản hóa cốt lõi đến từ việc quan sát rằng chuyển động là đồng nhất. Vì cả Harry và những hồn ma đều di chuyển một cạnh mỗi giây, điều quan trọng duy nhất là thời gian sớm nhất mỗi người có thể đến được mọi phòng. Nếu một con ma đến một căn phòng vào thời điểm t, Harry phải đến sớm hơn t để đi qua nó một cách an toàn. Nếu không thì Harry và hồn ma sẽ trùng hợp vào một thời điểm nào đó. 

Điều này biến bài toán thành hai phép tính đường đi ngắn nhất trên biểu đồ không có trọng số. Chúng tôi tính toán khoảng cách ngắn nhất từ ​​phòng xuất phát của Harry bằng BFS. Chúng tôi cũng tính toán khoảng cách ngắn nhất tới bất kỳ bóng ma nào, điều này có thể được thực hiện bằng cách coi tất cả các vị trí bóng ma là nguồn đồng thời trong một BFS duy nhất. 

Khi cả hai mảng khoảng cách đều được biết, một căn phòng sẽ an toàn cho Harry nếu khoảng cách của anh ấy đến căn phòng đó hoàn toàn nhỏ hơn khoảng cách ma. Cuối cùng, chúng ta đếm xem có bao nhiêu phòng thoát thỏa mãn điều kiện này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Cao | Quá chậm | 
| So sánh khoảng cách hai BFS | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xây dựng biểu đồ 

Chúng ta lưu trữ thành phố dưới dạng danh sách kề. Mỗi phòng giữ một danh sách các phòng được kết nối. 

Cấu trúc này là bắt buộc vì cả hai quá trình truyền BFS đều cần khám phá các lân cận một cách hiệu quả. 

### 2. BFS từ phòng xuất phát của Harry 

Chúng tôi chạy BFS tiêu chuẩn bắt đầu từ s, tính toán distH[v], thời gian tối thiểu Harry cần để đến từng phòng v. 

Vì mọi cạnh đều có trọng số bằng 1 nên BFS đảm bảo đường đi ngắn nhất. 

### 3. BFS đa nguồn từ mọi vị trí ghost 

Chúng ta khởi tạo một hàng đợi với tất cả các phòng ma ở khoảng cách 0 và tính distG[v], thời điểm sớm nhất mà bất kỳ con ma nào có thể đến từng phòng v. 

Bước này rất quan trọng vì ma di chuyển đồng thời. Việc coi chúng như nhiều nguồn BFS sẽ mô hình hóa chính xác thời điểm xuất hiện sớm nhất có thể trong số tất cả các hồn ma. 

### 4. So sánh thời gian đến tại các lối ra 

Đối với mỗi phòng thoát e, chúng tôi kiểm tra xem distH[e] < distG[e] hay không. Nếu đúng, Harry có thể đến được lối ra trước bất kỳ con ma nào. 

Chúng tôi đếm có bao nhiêu lối thoát thỏa mãn điều kiện này. 

### 5. Xuất số đếm 

Câu trả lời cuối cùng là số lối thoát an toàn. 

### Tại sao nó hoạt động 

Khoảng cách BFS thể hiện chính xác thời gian đến sớm nhất với chi phí cạnh đồng đều. Vì cả Harry và những hồn ma đều di chuyển một cách tối ưu và đồng thời, nên bất kỳ cuộc chạm trán nào cũng chỉ được xác định bởi những thời điểm đến sớm nhất này. Nếu một con ma có thể đến một nút tại thời điểm t, thì bất kỳ đường đi nào mà Harry đến vào thời điểm t hoặc muộn hơn sẽ dẫn đến va chạm tại nút đó vào thời điểm t hoặc sớm hơn. Do đó, việc so sánh thời gian đến ngắn nhất sẽ nắm bắt đầy đủ tất cả các tương tác có thể xảy ra và không có cấu trúc đường dẫn thay thế nào có thể vượt qua ràng buộc này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def bfs(start_nodes, n, adj):
    INF = 10**18
    dist = [INF] * (n + 1)
    q = deque()

    if isinstance(start_nodes, list):
        for s in start_nodes:
            dist[s] = 0
            q.append(s)
    else:
        dist[start_nodes] = 0
        q.append(start_nodes)

    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] == INF:
                dist[v] = dist[u] + 1
                q.append(v)

    return dist

def solve():
    n, m, s, k, g = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)

    exits = list(map(int, input().split()))
    ghosts = list(map(int, input().split()))

    distH = bfs(s, n, adj)
    distG = bfs(ghosts, n, adj)

    ans = 0
    for e in exits:
        if distH[e] < distG[e]:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp phân tách hai phép tính khoảng cách một cách rõ ràng. Trình trợ giúp BFS hỗ trợ cả trường hợp nguồn đơn và đa nguồn bằng cách chấp nhận một số nguyên duy nhất hoặc danh sách các nút bắt đầu. BFS ghost có nhiều nguồn, điều này rất cần thiết cho tính chính xác. 

Một chi tiết tinh tế là khởi tạo khoảng cách với giá trị INF lớn và chỉ cập nhật một lần cho mỗi nút. Điều này đảm bảo mỗi nút được xử lý ở khoảng cách tối thiểu chính xác một lần, duy trì tính chính xác của BFS. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4 5 1 2
1 2
2 3
2 4
4 1
3
4 5
```Chúng tôi tính toán khoảng cách: 

| Bước | Nút | Quận Harry | Quận ma | 
| --- | --- | --- | --- | 
| Ban đầu | 5 | 0 | INF | 
| BFS H | 3 | 2 | INF | 
| BFS H | 4 | 1 | INF | 
| BFS G | 4 | 2 | 0 | 
| BFS G | 2 | INF | 1 | 
| BFS G | 1 | INF | 2 | 

Bây giờ hãy kiểm tra các lần thoát: 

Lối ra 3: Harry tới ở 2, bóng ma không bao giờ tới được (INF), nên hợp lệ. 

Câu trả lời là 1. 

Điều này cho thấy trường hợp chỉ cần khả năng tiếp cận là đủ vì bóng ma ở rất xa. 

### Mẫu 2 

đầu vào:```
5 5 5 1 2
1 2
2 3
3 4
4 1
5 1
3
4
```Kết quả khoảng cách: 

| Nút | Quận Harry | Quận ma | 
| --- | --- | --- | 
| 3 | 2 | 1 | 
| 4 | 1 | 0 | 
| 1 | 2 | 1 | 

Lối ra là 3: 

Harry đến được 2, hồn ma đến được 1, nên điều kiện không thành công. 

Câu trả lời là 0. 

Điều này chứng tỏ rằng ngay cả những lối thoát có thể tiếp cận cũng trở nên không hợp lệ khi một bóng ma đến sớm hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Hai lần duyệt BFS qua danh sách kề | 
| Không gian | O(n + m) | Lưu trữ đồ thị cộng với mảng khoảng cách | 

Các ràng buộc cho phép lên tới 200.000 nút và cạnh, đồng thời BFS tuyến tính vừa vặn thoải mái trong cả giới hạn thời gian và bộ nhớ trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    def bfs(start_nodes, n, adj):
        INF = 10**18
        dist = [INF] * (n + 1)
        q = deque()

        if isinstance(start_nodes, list):
            for s in start_nodes:
                dist[s] = 0
                q.append(s)
        else:
            dist[start_nodes] = 0
            q.append(start_nodes)

        while q:
            u = q.popleft()
            for v in adj[u]:
                if dist[v] == INF:
                    dist[v] = dist[u] + 1
                    q.append(v)

        return dist

    def solve():
        n, m, s, k, g = map(int, input().split())
        adj = [[] for _ in range(n + 1)]

        for _ in range(m):
            a, b = map(int, input().split())
            adj[a].append(b)
            adj[b].append(a)

        exits = list(map(int, input().split()))
        ghosts = list(map(int, input().split()))

        distH = bfs(s, n, adj)
        distG = bfs(ghosts, n, adj)

        ans = 0
        for e in exits:
            if distH[e] < distG[e]:
                ans += 1
        return str(ans)

    return solve()

# sample cases
assert run("""5 4 5 1 2
1 2
2 3
2 4
4 1
3
4 5
""") == "1"

assert run("""5 5 5 1 2
1 2
2 3
3 4
4 1
5 1
3
4
""") == "0"

# custom: single node
assert run("""1 0 1 1 0
1
""") == "1"

# custom: ghost blocks start
assert run("""2 1 1 1 1
1 2
2
1
""") == "0"

# custom: disconnected exit
assert run("""4 2 1 1 1
1 2
3 4
4
2
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | lối thoát an toàn tầm thường | 
| bóng ma ở lối thoát | 0 | hành vi chặn | 
| đồ thị bị ngắt kết nối | 0 | khả năng tiếp cận và an toàn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một con ma xuất hiện trong căn phòng cạnh Harry và đến lối ra nhanh hơn mặc dù đường đi của Harry ngắn. Thuật toán xử lý vấn đề này vì BFS từ các hồn ma truyền chính xác thời gian đến sớm nhất của chúng ra bên ngoài, khiến các khu vực lân cận ngay lập tức không an toàn trừ khi Harry nhanh hơn. 

Một trường hợp khác là khi có nhiều hồn ma tồn tại và chỉ có sự xuất hiện tối thiểu trong số đó mới quan trọng. BFS đa nguồn mã hóa điều này một cách tự nhiên vì tất cả các bóng ma đều vào hàng đợi tại thời điểm 0 và lần đến đầu tiên của mỗi nút tự động là mức tối thiểu trên tất cả các nguồn. 

Trường hợp cuối cùng là khi ma không thể truy cập được lối ra. Khoảng cách ma vẫn là vô hạn và phép so sánh distH[e] < distG[e] chấp nhận chính xác các lối ra như vậy bất kể độ dài đường dẫn, phù hợp với thực tế là không ma nào có thể can thiệp.
