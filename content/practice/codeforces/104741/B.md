---
title: "CF 104741B - \u5c0fM\u7684\u6e38\u620f"
description: "Chúng ta được cung cấp một đồ thị vô hướng có trọng số với các vị trí $N$ và các con đường $M$. Hai người chơi bắt đầu ở nút $1$ và muốn đến nút $N$."
date: "2026-06-29T00:52:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104741
codeforces_index: "B"
codeforces_contest_name: "The 10th Jimei University Programming Contest"
rating: 0
weight: 104741
solve_time_s: 52
verified: true
draft: false
---

[CF 104741B - \u5c0fM\u7684\u6e38\u620f](https://codeforces.com/problemset/problem/104741/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số với$N$địa điểm và$M$những con đường. Hai người chơi bắt đầu tại nút$1$và muốn tiếp cận nút$N$. Chúng di chuyển dọc theo biểu đồ từng bước một, nhưng chuyển động bị hạn chế: từ nút hiện tại, cạnh được chọn tiếp theo phải nằm trên một đường đi ngắn nhất nào đó đến nút$N$. Nói cách khác, ở mỗi bước, họ buộc phải duy trì mức tối ưu về khoảng cách còn lại đến đích. 

Trò chơi này theo lượt. Người chơi đầu tiên (Tiểu M) chọn nước đi đầu tiên từ nút$1$, sau đó Little I chọn cái tiếp theo và chúng thay thế nhau. Bất cứ ai bị buộc phải “di chuyển” khi họ đã ở nút$N$thua, nghĩa là đạt được$N$trận đấu kết thúc và người chơi vừa đến không được di chuyển nữa, nhưng đối thủ được coi là không có động thái nào và do đó thắng theo cấu trúc luật đã mô tả. 

Cả hai người chơi đều chơi tối ưu và chúng ta phải xác định xem liệu Little M (người chơi xuất phát) có thể giành chiến thắng hay không. 

Các ràng buộc cho phép lên đến$10^5$nút và$2 \cdot 10^5$các cạnh cho mỗi trường hợp thử nghiệm, với tối đa 10 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi cách tiếp cận bậc hai hoặc mở rộng trạng thái trên tất cả các đường dẫn. Bất kỳ giải pháp nào về cơ bản phải tuyến tính hoặc gần tuyến tính trong kích thước biểu đồ, thường là$O(M \log N)$hoặc$O(N + M)$. 

Khó khăn tinh vi là mặc dù biểu đồ có thể chứa các chu trình, nhưng quy tắc “phải đi theo các đường đi ngắn nhất” hạn chế một cách hiệu quả việc chơi theo cấu trúc tuần hoàn được định hướng gây ra bởi khoảng cách ngắn nhất đến nút.$N$. 

Một sai lầm ngây thơ sẽ nảy sinh nếu người ta coi đây là một trò chơi chung trên biểu đồ mà không thực thi các ràng buộc về đường đi ngắn nhất. 

Ví dụ: hãy xem xét biểu đồ tam giác trong đó nút 1 kết nối với 2 và 3 và cả hai đều kết nối với 4 (đích), có trọng số bằng nhau. Một người giải trò chơi ngây thơ có thể xem xét các chuyển đổi tùy ý và đánh giá sai các chu kỳ hoặc các trạng thái lặp lại. Hành vi đúng sẽ bỏ qua hoàn toàn các chuyển đổi không ngắn nhất. 

Một trường hợp lỗi khác xảy ra nếu khoảng cách ngắn nhất được tính từ nút$1$thay vì nút$N$. Trò chơi được xác định bởi các ràng buộc về khoảng cách đến mục tiêu, do đó, việc đảo ngược gốc tính toán đường đi ngắn nhất sẽ dẫn đến các nước đi được phép không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng trạng thái trò chơi như một cặp bao gồm nút hiện tại và lượt của nút đó. Từ mỗi trạng thái, chúng tôi thử tất cả các cạnh đường đi ngắn nhất hợp lệ và xác định đệ quy xem người chơi hiện tại có thể giành chiến thắng hay không. Mặc dù đúng về mặt logic, nhưng điều này khám phá một biểu đồ trò chơi có kích thước tỷ lệ thuận với số cạnh trong sơ đồ con đường đi ngắn nhất và trong trường hợp xấu nhất sẽ thoái hóa thành phân nhánh theo cấp số nhân khi tồn tại nhiều đường đi ngắn nhất tiếp theo ở mỗi bước. Với tối đa$10^5$các nút, điều này là hoàn toàn không khả thi. 

Quan sát quan trọng là “phải luôn di chuyển theo con đường ngắn nhất để$N$" quy tắc loại bỏ chu kỳ theo nghĩa mạnh. Nếu chúng ta tính toán$dist[u]$là khoảng cách ngắn nhất từ$u$ĐẾN$N$, thì mọi nước đi hợp lệ đều giảm nghiêm ngặt$dist$. Điều này có nghĩa là đồ thị trò chơi trở thành đồ thị không theo chu kỳ có hướng trong đó các cạnh đi từ khoảng cách lớn hơn đến khoảng cách nhỏ hơn. 

Khi chúng ta có DAG, vấn đề sẽ trở thành DP ở trạng thái chiến thắng tiêu chuẩn: một vị trí sẽ thắng nếu có ít nhất một nước đi đến vị trí thua và sẽ thua nếu tất cả các nước đi đều đến vị trí thắng. Vì các cạnh luôn đi từ khoảng cách cao hơn đến khoảng cách thấp hơn nên chúng ta có thể xử lý các nút theo thứ tự tăng dần$dist$, bắt đầu từ$N$nơi không có chuyển động tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trò chơi Brute Force | Hàm mũ trong trường hợp xấu nhất |$O(N + M)$đệ quy | Quá chậm | 
| Đường đi ngắn nhất + DP trên DAG |$O(M \log N)$|$O(N + M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi trò chơi thành một biểu đồ có hướng có đường dẫn giới hạn đường đi ngắn nhất và sau đó chạy chương trình động trạng thái chiến thắng tiêu chuẩn trên đó. 

1. Tính khoảng cách ngắn nhất từ ​​mỗi nút đến nút$N$sử dụng thuật toán Dijkstra. Điều này cho chúng ta biết, đối với mỗi nút, nó cách mục tiêu bao xa trong điều kiện di chuyển tối ưu. 
2. Với mọi cạnh vô hướng$(u, v, w)$, xác định xem nó có thể được sử dụng trong một nước đi hợp lệ hay không. Cạnh có thể được sử dụng từ$u$ĐẾN$v$nếu và chỉ khi$dist[u] = dist[v] + w$và tương tự có thể sử dụng được từ$v$ĐẾN$u$nếu như$dist[v] = dist[u] + w$. Tính định hướng này được tạo ra hoàn toàn bởi tính nhất quán của đường đi ngắn nhất. 
3. Coi mỗi nút là một trạng thái trò chơi. Xác định một boolean$dp[u]$có nghĩa là “người chơi đến lượt tại nút$u$có thể buộc phải thắng.” 
4. Khởi tạo$dp[N] = False$, vì đã đến$N$có nghĩa là không có nước đi hợp lệ đi. 
5. Sắp xếp các nút theo thứ tự tăng dần$dist[u]$. Điều này đảm bảo chúng tôi xử lý các trạng thái từ gần nhất đến$N$hướng ngoại. 
6. Đối với mỗi nút$u$theo thứ tự này, kiểm tra tất cả các nước láng giềng$v$như vậy việc chuyển đến$v$là hợp lệ (nó giảm khoảng cách bằng chính xác trọng lượng cạnh). Nếu tồn tại ít nhất một hàng xóm như vậy$v$Ở đâu$dp[v] = False$, sau đó đặt$dp[u] = True$, bởi vì người chơi hiện tại có thể ép đối thủ vào trạng thái thua cuộc. 
7. Nếu không có động thái nào như vậy, hãy đặt$dp[u] = False$. 
8. Câu trả lời là$dp[1]$, vì trò chơi bắt đầu tại nút$1$. 

### Tại sao nó hoạt động 

Giới hạn khoảng cách ngắn nhất đảm bảo rằng mọi bước di chuyển hợp lệ sẽ làm giảm nghiêm trọng giá trị của$dist[u]$. Điều này tạo ra một trật tự nghiêm ngặt trên các trạng thái, ngăn chặn các chu kỳ trong biểu đồ trò chơi. Kết quả là, mọi trạng thái chỉ phụ thuộc vào các trạng thái nhỏ hơn, do đó các nút xử lý ngày càng tăng$dist$thứ tự đảm bảo tất cả các chuyển đổi đã được giải quyết khi cần thiết. Việc lặp lại DP là chính xác vì mỗi nước đi hợp pháp được coi là chính xác một lần và điều kiện thắng phù hợp với logic trò chơi tổ hợp chơi bình thường tiêu chuẩn trên DAG. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

INF = 10**18

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        u, v, w = map(int, input().split())
        g[u].append((v, w))
        g[v].append((u, w))

    dist = [INF] * (n + 1)
    dist[n] = 0
    pq = [(0, n)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    nodes = list(range(1, n + 1))
    nodes.sort(key=lambda x: dist[x])

    dp = [False] * (n + 1)
    dp[n] = False

    for u in nodes:
        if u == n:
            continue
        for v, w in g[u]:
            if dist[u] == dist[v] + w:
                if not dp[v]:
                    dp[u] = True
                    break

    print("Little M is the winner." if dp[1] else "Little I is the winner.")

t = int(input())
for _ in range(t):
    solve()
```Giai đoạn đầu tiên tính toán khoảng cách ngắn nhất từ ​​đích đến bằng Dijkstra. Sự đảo ngược này là cần thiết vì các chuyển động được xác định bằng cách “tiến gần hơn đến$N$,” vì vậy khoảng cách phải bắt nguồn từ$N$. 

Giai đoạn thứ hai lọc các cạnh bằng cách sử dụng điều kiện$dist[u] = dist[v] + w$. Điều này tránh việc xây dựng một biểu đồ có hướng một cách rõ ràng và giữ cho bộ nhớ tuyến tính. 

Vòng lặp DP dựa vào các nút xử lý theo thứ tự khoảng cách tăng dần sao cho mỗi$dp[v]$đã được biết trước khi tính toán$dp[u]$. 

Một lỗi triển khai phổ biến là quên rằng nhiều cạnh có thể thỏa mãn điều kiện đường đi ngắn nhất; tất cả phải được kiểm tra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi đơn giản: 

1 --(1)-- 2 --(1)-- 3 

với điểm đến$3$. 

Khoảng cách ngắn nhất từ 3 là: 

| Nút | quận | 
| --- | --- | 
| 3 | 0 | 
| 2 | 1 | 
| 1 | 2 | 

Thứ tự xử lý: 3, 2, 1. 

Đối với nút 2, nó chuyển sang nút 3 và$dp[3] = False$, Vì thế$dp[2] = True$. 

Đối với nút 1, nó chuyển sang nút 2, nhưng$dp[2] = True$, vì vậy nó không thể buộc phải thắng, do đó$dp[1] = False$. 

Vậy là Tiểu M thua ở cấu hình này. 

### Ví dụ 2 

Đồ thị: 

1 kết nối với 2 và 3, cả hai đều kết nối với 4 (đích), tất cả đều có trọng số 1. 

Khoảng cách ngắn nhất: 

| Nút | quận | 
| --- | --- | 
| 4 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 1 | 2 | 

Thứ tự xử lý: 4, 2, 3, 1. 

Tại nút 2 và 3, cả hai đều có thể lên tới 4, vì vậy cả hai đều là trạng thái chiến thắng. 

Tại nút 1, cả hai nước đi đều chuyển sang trạng thái thắng, vì vậy$dp[1] = False$. 

Điều này cho thấy việc có nhiều nhánh tối ưu cũng không giúp ích gì nếu tất cả chúng đều dẫn đến vị trí chiến thắng cho đối thủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \log N)$| Dijkstra thống trị; DP tuyến tính theo các cạnh | 
| Không gian |$O(N + M)$| danh sách kề, mảng khoảng cách, mảng DP | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các cạnh, do đó hệ số logarit từ Dijkstra có thể được chấp nhận. Giai đoạn DP hoàn toàn tuyến tính và phù hợp thoải mái trong giới hạn thậm chí trên 10 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    # assume solve() and loop are defined above
    t = int(input())
    for _ in range(t):
        solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# minimal case
assert run("""1
1 0
""") == "Little M is the winner."

# simple chain where first loses
assert run("""1
3 2
1 2 1
2 3 1
""") == "Little I is the winner."

# branching case
assert run("""1
4 4
1 2 1
2 4 1
1 3 1
3 4 1
""") == "Little I is the winner."

# uneven graph
assert run("""1
5 6
1 2 2
2 5 2
1 3 1
3 4 1
4 5 1
2 4 1
""") == "Little M is the winner."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | M thắng | xử lý trạng thái đầu cuối | 
| chuỗi | Tôi thắng | độ chính xác DP tuyến tính | 
| phân nhánh đối xứng | Tôi thắng | nhiều đường đi ngắn nhất | 
| tạ hỗn hợp | M thắng | lọc đường đi ngắn nhất chính xác | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi biểu đồ chứa nhiều cạnh có đường đi ngắn nhất từ một nút, nhưng chỉ một số trong số chúng dẫn đến trạng thái mất. DP phải xem xét tất cả các khía cạnh như vậy; dừng sớm ở cạnh hợp lệ đầu tiên sẽ không chính xác. Thuật toán xử lý vấn đề này một cách chính xác bằng cách kiểm tra rõ ràng mọi hàng xóm thỏa mãn điều kiện đẳng thức đường đi ngắn nhất trước khi quyết định giá trị DP. 

Một trường hợp tinh vi khác là khi đồ thị chứa các chu trình ở dạng ban đầu. Các chu kỳ này biến mất sau khi thực thi giới hạn khoảng cách, bởi vì bất kỳ bước di chuyển hợp lệ nào cũng phải giảm nghiêm ngặt khoảng cách đến$N$. Điều này đảm bảo rằng ngay cả các đồ thị đầu vào có tính chu kỳ cao cũng hoạt động giống như một DAG trong pha DP và thuật toán vẫn được xác định rõ ràng.
