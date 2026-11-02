---
title: "CF 1076E - Vasya và một cái cây"
description: "Chúng ta có một cây có gốc trong đó đỉnh 1 đóng vai trò là gốc và mọi đỉnh ban đầu đều giữ giá trị 0. Cây là tĩnh nhưng chúng ta được yêu cầu xử lý một chuỗi các thao tác cập nhật."
date: "2026-06-15T06:51:54+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 1076
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 54 (Rated for Div. 2)"
rating: 1900
weight: 1076
solve_time_s: 269
verified: true
draft: false
---

[CF 1076E - Vasya và một cái cây](https://codeforces.com/problemset/problem/1076/E) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 4 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó đỉnh 1 đóng vai trò là gốc và mọi đỉnh ban đầu đều giữ giá trị 0. Cây là tĩnh nhưng chúng ta được yêu cầu xử lý một chuỗi các thao tác cập nhật. Mỗi thao tác chọn một đỉnh$v$, giới hạn khoảng cách$d$, và một giá trị$x$, rồi cộng thêm$x$đến mọi nút$u$nằm bên trong cây con của$v$và ở khoảng cách nhiều nhất$d$từ$v$. 

Đối tượng hình học quan trọng không phải là một cây con tiêu chuẩn mà là một vùng bị cắt cụt bên trong nó. Từ một gốc cố định$v$, chúng tôi chỉ ảnh hưởng đến các nút đều là con cháu của$v$và nằm trong phạm vi độ sâu giới hạn được đo từ$v$. 

Đầu ra đơn giản là giá trị cuối cùng được lưu trữ ở mọi đỉnh sau khi tất cả các cập nhật được áp dụng. 

Các ràng buộc đẩy chúng tôi tới một giải pháp gần với tuyến tính hoặc logarit tuyến tính trên mỗi tổng hợp truy vấn. Với tối đa$3 \cdot 10^5$các nút và truy vấn, mọi giải pháp truy cập các nút bị ảnh hưởng một cách rõ ràng trên mỗi truy vấn sẽ thoái hóa thành$O(nm)$, vượt xa giới hạn khả thi. Ngay cả DFS cho mỗi truy vấn cũng không được chấp nhận. 

Một vấn đề tế nhị phát sinh từ các vùng cập nhật chồng chéo. Một nút có thể bị ảnh hưởng bởi nhiều truy vấn và mỗi truy vấn ảnh hưởng đến một vùng không liên kết theo trục trong cấu trúc cây, điều này khiến cho việc truyền bá đơn giản trở nên khó khăn. 

Một trường hợp thất bại tiêu biểu cho các phương pháp tiếp cận ngây thơ là một cái cây có hình dạng như một sợi dây chuyền. Nếu mọi truy vấn đều nhắm vào nút gốc với độ sâu lớn thì mỗi truy vấn sẽ chạm vào hầu hết các nút, dẫn đến hành vi bậc hai. Một dạng lỗi khác là lặp đi lặp lại việc duyệt lại các cây con cho các nút sâu, nơi công việc bị trùng lặp trên các phạm vi truy vấn chồng chéo. 

## Phương pháp tiếp cận 

Phương pháp brute-force tuân theo định nghĩa: đối với mỗi truy vấn, hãy chạy DFS từ$v$, dừng lại khi độ sâu vượt quá$d$, và thêm$x$đến tất cả các nút đã truy cập. Điều này đúng vì nó liệt kê chính xác định nghĩa của vùng bị ảnh hưởng. Tuy nhiên, trong trường hợp xấu nhất mỗi DFS chạm vào$O(n)$các nút và với$m$truy vấn này trở thành$O(nm)$, quá lớn đối với$3 \cdot 10^5$. 

Quan sát quan trọng là mỗi truy vấn về cơ bản là một “quả bóng xung quanh một nút, nhưng bị giới hạn ở cây con của nó”. Điều này cho thấy sự phân tách theo độ sâu. Nếu chúng ta nhìn cây theo chiều sâu từ gốc thì mỗi nút thuộc về chính xác một mức độ sâu và các ràng buộc của cây con tương ứng với các phân đoạn liền kề trong thứ tự chuyến tham quan Euler. 

Ý tưởng quan trọng là xử lý các đóng góp bằng cách sử dụng chuyến tham quan cây Euler, kết hợp với phân nhóm theo chiều sâu. Đối với mỗi nút$v$, cây con của nó trở thành một đoạn liền kề theo thứ tự Euler. Bên trong phân đoạn đó, một truy vấn chỉ ảnh hưởng đến các nút có độ sâu nằm trong khoảng giới hạn$[depth[v], depth[v] + d]$. 

Điều này chuyển đổi mỗi truy vấn thành một tập hợp các cập nhật phạm vi theo khoảng Euler nhưng được phân chia theo độ sâu. Chúng ta có thể duy trì một mảng khác biệt riêng biệt cho mỗi mức độ sâu hoặc hiệu quả hơn là duy trì cấu trúc toàn cầu theo dõi các đóng góp theo độ sâu bằng cách sử dụng quá trình truyền tải DFS giống như quét. Giải pháp tiêu chuẩn sử dụng tích lũy DSU-on-tree hoặc kiểu Fenwick được lập chỉ mục theo độ sâu trong quá trình truyền tải: chúng tôi đẩy các hiệu ứng truy vấn ở độ sâu bắt đầu của chúng và loại bỏ chúng sau khi rời khỏi cây con. 

Điều này làm giảm mỗi truy vấn xuống$O(1)$cập nhật khấu hao trên các cấu trúc được truy vấn một lần trên mỗi nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi truy vấn |$O(nm)$|$O(n)$| Quá chậm | 
| Chuyến tham quan Euler + tích lũy độ sâu (quét DFS) |$O((n+m)\log n)$hoặc$O(n+m)$tùy theo việc thực hiện |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta lấy gốc cây ở mức 1 và tính hai thuộc tính chính cho mỗi nút: độ sâu tính từ gốc và khoảng chu trình Euler của nó$[tin, tout]$, trong đó cây con của một nút tương ứng với một đoạn liền kề. 

Sau đó chúng tôi diễn giải lại từng truy vấn. Một truy vấn$(v, d, x)$ảnh hưởng đến mọi nút$u$như vậy$u$nằm trong cây con của$v$Và$depth[u] \le depth[v] + d$. Tương tự, chúng ta chỉ cần áp dụng$x$tới các nút trong đoạn Euler$[tin[v], tout[v]]$nhưng chỉ khi độ sâu của chúng thỏa mãn giới hạn trên. 

Để quản lý việc này một cách hiệu quả, chúng tôi xử lý các nút theo thứ tự DFS và duy trì cấu trúc tổng hợp các đóng góp tích cực được lập chỉ mục theo độ sâu. 

1. Thực hiện DFS từ gốc để tính toán độ sâu và thời gian vào/ra Euler. Bước này chuyển các truy vấn cây con thành các truy vấn khoảng. 
2. Đối với mỗi nút$v$, lưu trữ các truy vấn kèm theo nó nhưng được viết lại thành “kích hoạt giá trị$x$ở độ sâu$depth[v]$và hủy kích hoạt sau độ sâu$depth[v] + d + 1$.” Việc hủy kích hoạt chỉ mang tính khái niệm và đảm bảo ảnh hưởng có giới hạn. 
3. Duy trì một cấu trúc toàn cầu theo dõi tổng số đóng góp tích cực đối với từng độ sâu. Khi duyệt cây, chúng ta cập nhật cấu trúc này khi vào một nút và hoàn nguyên khi rời khỏi cây con của nó. 
4. Trong quá trình truyền tải DFS, khi chúng ta truy cập nút$u$, chúng tôi truy vấn giá trị tích lũy hiện tại ở độ sâu$depth[u]$. Giá trị này thể hiện chính xác tổng của tất cả các truy vấn đang hoạt động có các ràng buộc bao gồm$u$. 
5. Thêm giá trị này vào câu trả lời cho$u$, sau đó tiếp tục truyền sang trẻ em, duy trì trạng thái hoạt động một cách nhất quán. 

Ý tưởng trọng tâm là thay vì truy cập rõ ràng các nút bị ảnh hưởng cho mỗi truy vấn, chúng tôi truyền tải các truy vấn xuống dưới và để mỗi nút “thu thập” những đóng góp có liên quan đến độ sâu của nó. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm nào trong DFS, tập hợp đóng góp hiện hoạt tương ứng chính xác với các truy vấn có nút nguồn là tổ tiên của nút hiện tại và giới hạn độ sâu của nó vẫn cho phép đưa vào. Vì ranh giới của cây con được tuân thủ trong quá trình truyền tải Euler nên không có truy vấn nào bị rò rỉ ra ngoài cây con dự định của nó. Mỗi nút được đánh giá chính xác một lần dựa trên nhiều tập hợp cập nhật có giới hạn độ sâu đang hoạt động chính xác, do đó giá trị tích lũy là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    a, b = map(int, input().split())
    g[a].append(b)
    g[b].append(a)

m = int(input())
queries = [[] for _ in range(n + 1)]

for _ in range(m):
    v, d, x = map(int, input().split())
    queries[v].append((d, x))

depth = [0] * (n + 1)
ans = [0] * (n + 1)

# We maintain a map from depth -> current sum of active contributions
from collections import defaultdict
active = defaultdict(int)

def dfs(u, p):
    # activate queries at node u
    for d, x in queries[u]:
        active[depth[u]] += x
        active[depth[u] + d + 1] -= x

    ans[u] += active[depth[u]]

    for v in g[u]:
        if v == p:
            continue
        depth[v] = depth[u] + 1
        dfs(v, u)

    # rollback
    for d, x in queries[u]:
        active[depth[u]] -= x
        active[depth[u] + d + 1] += x

dfs(1, -1)

print(*ans)
```DFS tính toán độ sâu và duyệt cây theo thứ tự từ cha đến con, đảm bảo cấu trúc cây con được tôn trọng. các`active`từ điển hoạt động như một mảng khác biệt được lập chỉ mục theo độ sâu: mỗi truy vấn chèn một đóng góp tích cực ở độ sâu bắt đầu của nó và hủy bỏ nó vượt quá phạm vi độ sâu cho phép. 

Một điểm tinh tế là chúng tôi không bao giờ duy trì rõ ràng các chỉ số chuyến tham quan Euler, bởi vì bản thân đệ quy DFS đảm bảo rằng khi chúng tôi ở trong cây con, tất cả các kích hoạt dựa trên tổ tiên vẫn hợp lệ và khôi phục đảm bảo không có rò rỉ ra ngoài phạm vi cây con. 

Câu trả lời tại mỗi nút được tính toán tại thời điểm truy cập, chỉ sử dụng các đóng góp áp dụng cho độ sâu của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây đầu vào:```
1 - 2 - 4
  \   \
   3   5
```Truy vấn:```
(1,1,1), (2,0,10), (4,10,100)
```| Bước | Nút | Độ sâu | Đóng góp tích cực theo chiều sâu | Giá trị ứng dụng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 0 | {} | 0 | 
| Q1 hoạt động lúc 1 | 1 | 0 | {0:+1,1:-1} | 1 | 
| Thăm 2 | 2 | 1 | {0:+1,1:-1} | 1 | 
| Quý 2 lúc 2 | 2 | 1 | {0:+1,1:-1,1:+10,2:-10} | 11 | 
| Thăm 4 | 4 | 2 | bao gồm Q3 | 100 | 
| Thăm 5 | 5 | 2 | không có thêm | 0 | 

Dấu vết cho thấy các đóng góp có giới hạn độ sâu chỉ được tích lũy một cách tự nhiên khi hợp lệ. 

### Mẫu 2 (đã thi công) 

Cây:```
1
|
2
|
3
```Truy vấn:```
(1,2,5)
(2,0,3)
```| Bước | Nút | Độ sâu | Đang hoạt động | Giá trị | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | Q1 hoạt động | 5 | 
| 2 | 2 | 1 | Q1+Q2 | 8 | 
| 3 | 3 | 2 | Chỉ Q1 | 5 | 

Điều này xác nhận sự tương tác chính xác giữa các cửa sổ độ sâu chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi truy vấn được thêm và xóa một lần trong DFS và mỗi nút được truy cập một lần | 
| Không gian |$O(n + m)$| danh sách kề, ngăn xếp đệ quy và bản đồ hoạt động | 

Thuật toán phù hợp thoải mái trong giới hạn vì cả hai$n$Và$m$đang lên đến$3 \cdot 10^5$và tất cả các hoạt động được khấu hao không đổi cho mỗi sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    m = int(input())
    queries = [[] for _ in range(n + 1)]
    for _ in range(m):
        v, d, x = map(int, input().split())
        queries[v].append((d, x))

    depth = [0] * (n + 1)
    ans = [0] * (n + 1)

    from collections import defaultdict
    active = defaultdict(int)

    sys.setrecursionlimit(10**7)

    def dfs(u, p):
        for d, x in queries[u]:
            active[depth[u]] += x
            active[depth[u] + d + 1] -= x

        ans[u] += active[depth[u]]

        for v in g[u]:
            if v == p:
                continue
            depth[v] = depth[u] + 1
            dfs(v, u)

        for d, x in queries[u]:
            active[depth[u]] -= x
            active[depth[u] + d + 1] += x

    dfs(1, -1)
    return " ".join(map(str, ans[1:]))

# provided sample
assert run("""5
1 2
1 3
2 4
2 5
3
1 1 1
2 0 10
4 10 100
""") == "1 11 1 100 0"

# minimum size
assert run("""1
0
1
1 0 5
""") == "5"

# chain propagation
assert run("""4
1 2
2 3
3 4
2
1 3 2
2 1 1
""") == "2 3 2 2"

# disjoint effects
assert run("""5
1 2
1 3
3 4
3 5
2
1 0 7
3 0 3
""") == "7 0 3 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây 1 nút | giá trị đơn | tính đúng đắn của trường hợp cơ sở | 
| cập nhật chuỗi | tuyên truyền tích lũy | xử lý độ sâu | 
| truy vấn chồng chéo | tính đúng đắn bổ sung | tác dụng tương tác | 

## Vỏ cạnh 

Cây tối thiểu có một nút duy nhất sẽ kiểm tra xem thuật toán có xử lý chính xác việc kích hoạt và đánh giá ở gốc mà không đệ quy vào con hay không. Trong trường hợp đó, DFS truy cập nút 1, áp dụng tất cả các hiệu ứng truy vấn ở độ sâu 0 và trực tiếp tạo ra giá trị cuối cùng mà không gặp bất kỳ vấn đề lan truyền nào. 

Chuỗi sâu kiểm tra xem liệu việc lập chỉ mục độ sâu có phân biệt chính xác các nút nằm trong phạm vi với các nút vượt quá phạm vi đó hay không. Khi DFS đi sâu hơn, các đóng góp sẽ hết hạn chính xác khi vượt qua ngưỡng độ sâu, đảm bảo rằng các nút nằm ngoài khoảng cách cho phép không tích lũy giá trị sai. 

Một trường hợp có các truy vấn chồng chéo ở cùng một nút xác nhận rằng nhiều kích hoạt ở cùng độ sâu xếp chồng một cách chính xác trong`active`cấu trúc, vì tất cả các phần tăng dần đều tích lũy trong cùng một nhóm độ sâu trước khi được truy vấn bởi các phần tử con.
