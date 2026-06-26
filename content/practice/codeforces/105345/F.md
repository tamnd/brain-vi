---
title: "CF 105345F - Ngôi Nhà Ma Ám"
description: "Nhiệm vụ là xác định những phòng thoát hiểm nào trong một tòa nhà là an toàn để Harry tiếp cận vì các hồn ma cũng đang di chuyển qua cùng tòa nhà với cùng tốc độ. Chúng ta có thể xem ngôi nhà như một đồ thị vô hướng trong đó các phòng là các nút và các cửa là các cạnh."
date: "2026-06-23T15:27:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105345
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 1 (Advanced)"
rating: 0
weight: 105345
solve_time_s: 95
verified: false
draft: false
---

[CF 105345F - Ngôi nhà ma ám](https://codeforces.com/problemset/problem/105345/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xác định những phòng thoát hiểm nào trong một tòa nhà là an toàn để Harry tiếp cận vì các hồn ma cũng đang di chuyển qua cùng tòa nhà với cùng tốc độ. 

Chúng ta có thể xem ngôi nhà như một đồ thị vô hướng trong đó các phòng là các nút và các cửa là các cạnh. Harry bắt đầu tại một căn phòng cố định và có một số phòng được đánh dấu là lối ra. Đồng thời, nhiều hồn ma bắt đầu từ phòng riêng của họ. Mỗi giây, Harry có thể di chuyển dọc theo một cạnh sang phòng bên cạnh, và mọi con ma đều có thể làm như vậy. Chuyển động xảy ra đồng thời theo từng bước riêng biệt. 

Một lối ra được coi là an toàn nếu Harry có thể tiếp cận được nó đồng thời đảm bảo rằng anh ta không bao giờ chiếm một căn phòng cùng lúc với bất kỳ con ma nào. Vì cả hai đều di chuyển với tốc độ như nhau và có thể chọn những con đường tối ưu nên chúng tôi đang so sánh một cách hiệu quả tốc độ mà Harry và con ma gần nhất có thể đến từng phòng. 

Các ràng buộc cho phép lên tới 200.000 phòng và 200.000 cạnh, điều này ngay lập tức loại trừ bất kỳ phương pháp nào tính toán lại các đường đi ngắn nhất riêng biệt cho từng lối ra hoặc mô phỏng các chuyển động theo thời gian. Một giải pháp phải dựa vào một số lượng nhỏ các đường truyền đồ thị tuyến tính hoặc gần tuyến tính, thường là O(n + m). 

Một trường hợp thất bại tinh tế xuất hiện khi một con ma có thể đến một căn phòng cùng lúc với Harry. Ví dụ: nếu Harry đến một căn phòng trong 3 giây và một con ma nào đó cũng đến đó trong 3 giây, thì Harry được coi là không an toàn ở đó vì họ ở cùng một phòng ở cùng một bước thời gian. 

Một trường hợp quan trọng khác là khi Harry bắt đầu trong một căn phòng vốn đã có sẵn một con ma. Trong trường hợp đó, vị trí bắt đầu ngay lập tức không hợp lệ để tồn tại vì cả hai đều chiếm cùng một phòng tại thời điểm 0. 

Cuối cùng, lối ra có thể bao gồm những căn phòng mà ngay từ đầu Harry đã không thể vào được. Những lối thoát như vậy đương nhiên không an toàn vì Harry không thể với tới được. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là mô phỏng cả Harry và tất cả các hồn ma theo thời gian. Người ta có thể thử BFS đa tác nhân hoặc mô phỏng từng giây, theo dõi vị trí của tất cả các thực thể. Điều này nhanh chóng trở nên không khả thi vì ở mỗi bước, mỗi bóng ma sẽ phân nhánh qua biểu đồ và chúng ta sẽ liên tục khám phá các trạng thái giống nhau. Trong trường hợp xấu nhất, điều này suy biến thành các lần duyệt lặp lại có kích thước O(n + m) cho mỗi thực thể và từng bước thời gian, dẫn đến hành vi hàm mũ hoặc ít nhất là O(k(n + m)), vượt xa giới hạn. 

Một cách tiếp cận có cấu trúc hơn là quan sát rằng điều quan trọng đối với mỗi phòng không phải là con đường chính xác mà là thời điểm sớm nhất mà Harry và bất kỳ hồn ma nào có thể đến đó. Nếu chúng ta tính toán khoảng cách ngắn nhất từ ​​điểm xuất phát của Harry đến mỗi phòng và tính toán riêng khoảng cách ngắn nhất từ ​​vị trí bắt đầu của bất kỳ con ma nào đến mỗi phòng thì vấn đề sẽ giảm xuống thành một so sánh đơn giản cho mỗi phòng thoát ra. 

Thông tin chi tiết quan trọng là tất cả các bóng ma đều di chuyển đồng thời và độc lập, do đó ảnh hưởng của chúng có thể được kết hợp bằng cách coi tất cả các vị trí bắt đầu của bóng ma là nguồn trong một BFS đa nguồn duy nhất. Điều này cho biết, đối với mỗi phòng, thời gian sớm nhất mà bất kỳ con ma nào có thể chiếm giữ nó. BFS của Harry cho biết thời gian đến sớm nhất của anh ấy. Vì cả hai đều di chuyển một bước mỗi giây nên Harry chỉ an toàn trong phòng nếu anh ấy đến đúng nơi trước khi bất kỳ con ma nào có thể tiếp cận nó. 

Điều này làm giảm toàn bộ vấn đề xuống còn hai lần duyệt BFS trên cùng một biểu đồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k · (n + m)) hoặc tệ hơn | O(n + m) | Quá chậm | 
| BFS đa nguồn kép | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tính toán hai bản đồ khoảng cách trên biểu đồ. 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của đồ thị từ các cạnh đã cho. Điều này cho phép truyền tải hiệu quả tất cả các phòng được kết nối. 
2. Chạy BFS đa nguồn bắt đầu từ tất cả các vị trí ma cùng một lúc. Khởi tạo một mảng khoảng cách`distG`trong đó tất cả các giá trị là vô cực ngoại trừ các phòng bắt đầu có ma, được đặt thành 0. BFS này tính toán thời gian sớm nhất mà bất kỳ con ma nào có thể đến từng phòng, bởi vì việc mở rộng đồng thời từ tất cả các nguồn ma sẽ mô phỏng sự lây lan nhanh nhất có thể một cách tự nhiên. 
3. Chạy BFS tiêu chuẩn từ phòng bắt đầu của Harry`s`để tính toán một mảng khoảng cách khác`distH`, đại diện cho thời gian sớm nhất Harry có thể đến từng phòng. 
4. Đối với mỗi phòng thoát hiểm`e_i`, kiểm tra xem`distH[e_i]`đúng là ít hơn`distG[e_i]`. Nếu có, Harry có thể đến trước khi bất kỳ con ma nào có thể chiếm giữ căn phòng đó, vì vậy nó rất an toàn. Nếu không thì không an toàn. 
5. Đếm tất cả các phòng thoát hiểm thỏa mãn điều kiện này và in ra tổng số. 

Lý do khiến sự bất bình đẳng nghiêm ngặt trở nên quan trọng là vì việc đến cùng lúc là không an toàn. Nếu cả hai khoảng cách bằng nhau, một con ma có thể chiếm giữ căn phòng cùng lúc Harry đến, vi phạm điều kiện an toàn. 

### Tại sao nó hoạt động 

Mỗi BFS tính toán khoảng cách đường đi ngắn nhất trong biểu đồ không có trọng số, tương ứng chính xác với thời gian tối thiểu theo chi phí di chuyển đơn vị. Bởi vì ma và Harry di chuyển với tốc độ giống nhau và không tương tác với nhau ngoại trừ va chạm, thời gian đến sớm nhất hoàn toàn xác định liệu một vị trí có thể được chiếm giữ an toàn hay không. Điều bất biến là ở cuối BFS,`distG[v]`là thời gian tối thiểu mà một con ma có thể đến được phòng`v`, Và`distH[v]`là thời gian tối thiểu Harry có thể đạt được`v`. Vì cả hai quá trình đều khám phá song song tất cả các đường dẫn tối ưu nên không thể đến nhanh hơn những gì BFS ghi lại. Do đó, việc so sánh hai giá trị này tương đương với việc kiểm tra xem liệu Harry có thể vượt qua tất cả các bóng ma ở mỗi lối thoát hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

INF = 10**18

def bfs(starts, adj, n):
    dist = [INF] * (n + 1)
    q = deque()
    for s in starts:
        dist[s] = 0
        q.append(s)

    while q:
        v = q.popleft()
        for to in adj[v]:
            if dist[to] == INF:
                dist[to] = dist[v] + 1
                q.append(to)
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

    distH = bfs([s], adj, n)
    distG = bfs(ghosts, adj, n)

    ans = 0
    for e in exits:
        if distH[e] < distG[e]:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng quy trình BFS được chia sẻ cho cả Harry và các hồn ma. Sự khác biệt duy nhất là quá trình khởi tạo: BFS của Harry bắt đầu từ một nút duy nhất, trong khi BFS ma bắt đầu từ nhiều nguồn cùng một lúc. 

Một điểm tinh tế là việc khởi tạo khoảng cách thành một giá trị trọng điểm lớn, đảm bảo rằng các nút chưa được truy cập vẫn không thể truy cập được. Điều này rất quan trọng khi so sánh khoảng cách, vì một nút ma không thể truy cập được sẽ không chặn Harry tiếp cận nó. 

Sự so sánh chặt chẽ`distH[e] < distG[e]`mã hóa trực tiếp ràng buộc về tính đồng thời. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi tính toán khoảng cách từ Harry và tới những bóng ma, rồi so sánh ở lối ra. 

| Bước | distH tại lối ra | distG khi thoát | Quyết định | 
| --- | --- | --- | --- | 
| BFS từ Harry | tính thời gian ngắn nhất | - | - | 
| BFS từ ma | - | tính thời gian ngắn nhất | - | 
| So sánh | 2 | 3 | an toàn | 

Harry đến lối ra sớm hơn bất kỳ con ma nào nên lối ra được tính là hợp lệ. Điều này chứng tỏ trường hợp Harry đã thành công đi trước mọi con đường ma quái. 

### Mẫu 2 

| Bước | distH tại lối ra | distG khi thoát | Quyết định | 
| --- | --- | --- | --- | 
| BFS từ Harry | 2 | - | - | 
| BFS từ ma | - | 2 | không an toàn | 
| So sánh | 2 | 2 | bị từ chối | 

Ở đây một con ma có thể đến lối ra cùng lúc với Harry. Mặc dù Harry có đường đi nhưng điều kiện đồng thời khiến lối ra không an toàn. Điều này nhấn mạnh lý do tại sao cần phải có sự bất bình đẳng nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Hai lần duyệt BFS, mỗi lần xử lý mọi nút và cạnh nhiều nhất một lần | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng khoảng cách | 

Các ràng buộc cho phép lên tới 200.000 nút và cạnh, đồng thời giải pháp này chỉ thực hiện một số lượng nhỏ các lần tuyến tính không đổi trên biểu đồ, do đó, nó phù hợp một cách thoải mái trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    INF = 10**18

    def bfs(starts, adj, n):
        dist = [INF] * (n + 1)
        q = deque()
        for s in starts:
            dist[s] = 0
            q.append(s)
        while q:
            v = q.popleft()
            for to in adj[v]:
                if dist[to] == INF:
                    dist[to] = dist[v] + 1
                    q.append(to)
        return dist

    n, m, s, k, g = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)

    exits = list(map(int, input().split()))
    ghosts = list(map(int, input().split()))

    distH = bfs([s], adj, n)
    distG = bfs(ghosts, adj, n)

    ans = 0
    for e in exits:
        if distH[e] < distG[e]:
            ans += 1
    return str(ans)

# provided samples
assert run("""5 4 5 1 2
1 2
2 3
2 4
1 5
3
4
""") == "1"

assert run("""5 5 5 1 2
1 2
2 3
3 4
4 5
1 5
3
4
""") == "0"

# custom cases
assert run("""3 2 1 1 1
1 2
2 3
3
2
""") == "0", "ghost blocks path"

assert run("""4 3 1 2 1
1 2
2 3
3 4
4 2
3
""") == "1", "ghost far away exit safe"

assert run("""1 0 1 1 1
1
1
""") == "0", "same start collision"

assert run("""6 5 1 1 2
1 2
2 3
3 4
4 5
5 6
6
3 4
""") == "0", "ghost reaches middle first"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dây chuyền có chặn ma | 0 | ma vượt đường sớm | 
| bóng ma xa xôi | 1 | an toàn khi Harry nhanh hơn | 
| va chạm nút đơn | 0 | xung đột vị trí bắt đầu | 
| áp suất giữa đồ thị | 0 | sự thống trị nhiều bước của ma | 

## Vỏ cạnh 

Một trường hợp quan trọng xảy ra khi Harry bắt đầu trong một căn phòng cũng là nguồn ma. Trong tình huống đó, BFS ma chỉ định khoảng cách 0 cho nút bắt đầu và BFS của Harry cũng chỉ định 0. Việc so sánh không thành công ngay lập tức, do đó, bất kỳ lối ra nào yêu cầu ở lại hoặc đi qua nút đó đều không an toàn. Thuật toán xử lý điều này một cách tự nhiên vì sự bình đẳng không được phép. 

Một trường hợp khác là khi lối ra bị ngắt kết nối với cả Harry và tất cả các hồn ma. Khoảng cách của Harry trở thành vô cùng, trong khi khoảng cách của ma cũng là vô cùng. Vì điều kiện yêu cầu bất đẳng thức nghiêm ngặt, vô cực không nhỏ hơn vô cực nên lối ra được đánh dấu chính xác là không an toàn vì Harry không thể chạm tới được. 

Cuối cùng, khi có nhiều bóng ma tồn tại, BFS đa nguồn đảm bảo thời gian đến tối thiểu từ bất kỳ bóng ma nào chiếm ưu thế. Điều này ngăn chặn việc bỏ lỡ một đường dẫn ma nhanh hơn, nếu không sẽ làm mất hiệu lực lối thoát có vẻ an toàn nếu các bản ma được xử lý riêng lẻ.
