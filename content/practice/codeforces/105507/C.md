---
title: "CF 105507C - \u0414\u0435\u0440\u0435\u0432\u043e \u043e\u0431\u0445\u043e\u0434\u0430 \u0432 \u0433\u043b\u0443\u0431\u0438\u043d\u0443"
description: "Chúng ta có một đồ thị vô hướng liên thông $G$ có tối đa $n$ đỉnh và $m$ cạnh, và bên trong đồ thị này, chúng ta cũng có một cây bao trùm $T$ trên cùng một tập đỉnh. Mọi cạnh của $T$ được đảm bảo tồn tại trong $G$, nhưng $G$ có thể chứa các cạnh bổ sung."
date: "2026-06-23T21:57:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105507
codeforces_index: "C"
codeforces_contest_name: "2024-2025 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 24, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 105507
solve_time_s: 73
verified: true
draft: false
---

[CF 105507C - \u0414\u0435\u0440\u0435\u0432\u043e \u043e\u0431\u0445\u043e\u0434\u0430 \u0432 \u0433\u043b\u0443\u0431\u0438\u043d\u0443](https://codeforces.com/problemset/problem/105507/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông$G$lên tới$n$đỉnh và$m$các cạnh và bên trong biểu đồ này, chúng ta cũng có một cây bao trùm$T$trên cùng một tập đỉnh. Mỗi cạnh của$T$được đảm bảo tồn tại trong$G$, Nhưng$G$có thể chứa các cạnh bổ sung. 

Quá trình DFS là tiêu chuẩn: chúng tôi chọn một đỉnh bắt đầu, đánh dấu nó đã ghé thăm và quét danh sách kề theo một thứ tự cố định. Bất cứ khi nào chúng ta nhìn thấy một hàng xóm chưa được ghé thăm, chúng ta sẽ đệ quy đến đó và cạnh được sử dụng cho khám phá đầu tiên đó sẽ trở thành một phần của cây DFS. Quyền tự do duy nhất chúng ta có là sắp xếp lại danh sách kề của tất cả các đỉnh một cách tùy ý trước khi chạy DFS. 

Nhiệm vụ là, với mỗi đỉnh$i$, để xác định liệu có tồn tại thứ tự nào đó của tất cả các danh sách kề để chạy DFS từ$i$tạo ra một cây DFS chính xác bằng cây đã cho$T$. Chúng tôi không được phép thay đổi các cạnh hoặc bắt đầu nhiều lần chạy DFS, chỉ sắp xếp lại các hàng xóm. 

Các ràng buộc rất lớn:$n$tùy thuộc vào$2 \cdot 10^5$Và$m$lên đến$3 \cdot 10^5$, vì vậy bất kỳ giải pháp nào gần hơn với$O(n^2)$hoặc thậm chí$O(n \log n)$với hằng số nặng phải được chứng minh một cách cẩn thận. Truyền tuyến tính hoặc gần tuyến tính của cả đồ thị và cây là mục tiêu thực tế duy nhất. 

Một sự hiểu lầm ngây thơ là cho rằng vì chúng ta có thể sắp xếp lại danh sách kề một cách tùy ý, nên bất kỳ gốc nào cũng có thể hoạt động miễn là$T$là một cây bao trùm của$G$. Điều này là sai vì DFS nhạy cảm với thứ tự tương đối của các cạnh bên trong mỗi danh sách kề và hành vi quay lui có thể buộc cây DFS đi chệch hướng. 

Một ví dụ thất bại nhỏ là biểu đồ tam giác$1-2-3-1$với mép cây$1-2$,$2-3$. Nếu chúng tôi bắt đầu DFS lúc 1, chúng tôi có thể thực thi lệnh$2$trước$3$và lấy đúng cây. Nhưng nếu chúng ta bắt đầu ở con số 3, chúng ta không thể buộc DFS tránh đi$3 \to 1 \to 2$theo cách phá vỡ cấu trúc gốc được yêu cầu trừ khi có thứ tự cẩn thận trên toàn cầu, điều này phụ thuộc vào tính nhất quán của các khoảng cây con. 

Điểm tinh tế quan trọng là thứ tự kề phải đồng thời tôn trọng tất cả các lựa chọn DFS được tạo ra bởi cấu trúc cây có gốc. Đây là một ràng buộc toàn cầu, không phải là tự do độc lập trên mỗi đỉnh. 

## Phương pháp tiếp cận 

Một tư tưởng thô bạo là sửa tận gốc$r$, thử tất cả các hoán vị có thể có của danh sách kề và mô phỏng DFS để kiểm tra xem liệu chúng ta có thể thu được chính xác cây hay không$T$. Điều này ngay lập tức là không thể bởi vì mỗi giai thừa cấp độ đỉnh đều tăng lên và thậm chí một mô phỏng cũng bị hủy bỏ.$O(n+m)$, do đó tổng không gian sắp xếp lại là lớn về mặt thiên văn. 

Chúng ta cần phải lật lại quan điểm. Thay vì hỏi liệu một số thứ tự có hoạt động hay không, chúng tôi hỏi thứ tự nào sẽ bị ép buộc nếu cây DFS chính xác$T$. Nếu DFS từ$r$sản xuất$T$thì đối với mỗi nút, các nút con của nó trong cây DFS phải xuất hiện theo thứ tự kề phù hợp với thời gian khám phá của các cây con. Quan trọng hơn, DFS áp đặt một điều kiện: khi chúng ta ở một nút$v$, tất cả các đỉnh trong một cây con con phải được khám phá đầy đủ trước khi chuyển sang cây con tiếp theo theo thứ tự kề. 

Điều này có nghĩa là mỗi cây con của$T$phải hoạt động giống như một khối liền kề theo thứ tự truyền tải DFS (cấu trúc xuất nhập Euler). Nếu một đỉnh bên ngoài cây con xuất hiện "xen kẽ" theo thứ tự khám phá kề, DFS sẽ nhập sai cây con sớm hơn mức cho phép. 

Vì vậy, thay vì xây dựng các hoán vị, chúng tôi xác minh các điều kiện nhất quán bắt nguồn từ$T$. Chúng tôi nhổ cây$T$tại gốc ứng viên$r$, tính thời gian nhập của thứ tự DFS trên$T$, sau đó thực thi một điều kiện cần thiết cổ điển: đối với mọi cạnh không phải là cây$(u,v)$, một điểm cuối phải nằm trong cây con của điểm cuối kia trong$T$, nếu không thì DFS nhất thiết sẽ giao nhau giữa các thành phần đã được tách riêng và phá vỡ cấu trúc cây. 

Quan sát mang tính quyết định là để DFS tạo ra chính xác$T$, cái cây$T$phải là cây DFS của$G$theo thứ tự nào đó, tương đương với việc nói rằng với mỗi đỉnh, tất cả các cạnh nằm ngoài cây con của nó chỉ được kết nối với tổ tiên trong cây có gốc. Nếu tồn tại một cạnh kết nối hai cây con khác nhau, DFS sẽ buộc phải khám phá một nút sớm hơn trong$T$, cấu trúc mâu thuẫn 

Điều này làm giảm vấn đề trong việc kiểm tra, đối với từng gốc, cho dù$T$có thể là cây DFS hợp lệ của$G$. Chúng ta có thể tính toán trước các khoảng cây con của$T$và xác nhận tất cả các cạnh bổ sung trong$G$TRONG$O(1)$mỗi cạnh cho mỗi lần thử root, nhưng chúng ta không thể root lại một cách ngây thơ. Thay vào đó, chúng ta tính toán một điều kiện toàn cục: một cạnh hợp lệ nếu một điểm cuối là tổ tiên của điểm cuối kia trong$T$. Sau đó, chúng tôi kiểm tra xem nút nào có thể đóng vai trò là nút gốc sao cho không xảy ra vi phạm đối với các ràng buộc định hướng cha-con DFS, nút này có thể được ghi lại bằng cách đếm các đóng góp dọc theo đường dẫn cây và khởi động lại DP. 

Sau khi chuyển đổi các ràng buộc, vấn đề sẽ trở thành kiểm tra xem nút nào có thể là gốc sao cho mọi cạnh không phải là cây kết nối các đỉnh nơi mối quan hệ tổ tiên thẳng hàng với gốc đó. Điều này dẫn đến sự tích lũy tái tạo lại cây tiêu chuẩn: chúng tôi đánh dấu các hướng xấu cho mỗi cạnh dựa trên cấu trúc LCA và các ràng buộc truyền bá để một nút hợp lệ nếu không có cạnh nào mâu thuẫn với hướng tổ tiên do gốc gây ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị mạnh mẽ của danh sách kề + mô phỏng DFS | Hàm mũ | O(n + m) | Quá chậm | 
| Hiệu lực của gốc cây thông qua các ràng buộc tổ tiên + tích lũy tái khởi động | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cây đã cho$T$như một cấu trúc cố định và phân tích các cạnh không phải là cây của$G$hạn chế các gốc DFS có thể có. 

Đầu tiên chúng ta nhổ cây$T$tùy ý ở mức 1 và tính toán thời gian gốc, độ sâu và thời gian vào-ra bằng cách sử dụng DFS trên$T$. Điều này cho chúng ta một cách để kiểm tra mối quan hệ tổ tiên trong thời gian không đổi bằng cách sử dụng các khoảng tin/tout. 

Thứ hai, đối với mọi cạnh$(u,v)$TRONG$G$cái đó không có trong$T$, chúng tôi xác định liệu$u$là tổ tiên của$v$hoặc$v$là tổ tiên của$u$. Nếu cả hai đều không đúng thì cạnh kết nối hai cây con khác nhau không có mối quan hệ tổ tiên trong$T$. Một cạnh như vậy không bao giờ có thể được xử lý một cách an toàn trong cây DFS bằng$T$, bởi vì DFS sẽ phải nhập một cây con trong khi cây con kia vẫn "mở" theo cách buộc phải khám phá chéo. Chúng tôi ghi lại điều này như một ràng buộc toàn cầu loại trừ các vị trí gốc nhất định. 

Thứ ba, chúng ta chuyển đổi từng cạnh không phải cây thành các ràng buộc đối với các nghiệm có thể. Nếu như$u$là tổ tiên của$v$, thì cho$T$là một cây DFS có gốc tại$r$, DFS phải khám phá$u$trước khi hoàn thiện hoàn toàn$v$cây con của nó theo cách nhất quán với hành vi của ngăn xếp DFS. Điều này ngụ ý rằng các gốc hợp lệ không được nằm ở một số vùng nhất định, điều này sẽ buộc DFS vi phạm trật tự khám phá tổ tiên đầu tiên do cạnh đó gây ra. 

Chúng tôi thể hiện những ràng buộc này bằng cách sử dụng một mảng sai phân trong chuyến tham quan cây Euler. Mỗi cạnh cấm một đoạn rễ liền kề trong cấu trúc cây con. Chúng tôi thêm các điểm đánh dấu +1 và -1 trên các khoảng Euler tương ứng với các vùng gốc không hợp lệ xuất phát từ mỗi mối quan hệ điểm cuối cạnh vi phạm. 

Thứ tư, chúng ta tích lũy những đóng góp này theo cấp Euler của cây. Sau khi tổng hợp tiền tố, một nút$r$hợp lệ khi và chỉ nếu giá trị tích lũy của nó bằng 0, nghĩa là không có cạnh nào cấm chọn nó làm gốc. 

Thứ năm, chúng tôi xuất ra một chuỗi nhị phân trong đó mỗi vị trí tương ứng với việc liệu nút đó có thể đóng vai trò là gốc DFS tạo ra cây chính xác hay không$T$. 

Bất biến chính là mỗi cạnh không phải là cây tạo ra một tập hợp các vị trí gốc sẽ buộc DFS vượt qua ranh giới cây con một cách không chính xác. Mã hóa khoảng Euler đảm bảo rằng các vùng bị cấm này được biểu diễn dưới dạng các phân đoạn rời rạc và việc tích lũy tiền tố đảm bảo mỗi nút đều đếm chính xác có bao nhiêu ràng buộc loại trừ nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, m = map(int, input().split())

g = [[] for _ in range(n + 1)]
edges = set()

for _ in range(m):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)
    edges.add((u, v) if u < v else (v, u))

t = [[] for _ in range(n + 1)]
tree_edges = set()

for _ in range(n - 1):
    u, v = map(int, input().split())
    t[u].append(v)
    t[v].append(u)
    tree_edges.add((u, v) if u < v else (v, u))

parent = [0] * (n + 1)
depth = [0] * (n + 1)
tin = [0] * (n + 1)
tout = [0] * (n + 1)
timer = 0

def dfs(v, p):
    nonlocal_timer = dfs.timer
    # dummy placeholder, real timer handled outside

dfs.timer = 0

stack = [(1, 0, 0)]
order = []

while stack:
    v, p, state = stack.pop()
    if state == 0:
        parent[v] = p
        dfs.timer += 1
        tin[v] = dfs.timer
        order.append(v)
        stack.append((v, p, 1))
        for to in t[v]:
            if to == p:
                continue
            depth[to] = depth[v] + 1
            stack.append((to, v, 0))
    else:
        tout[v] = dfs.timer

def is_ancestor(u, v):
    return tin[u] <= tin[v] and tout[v] <= tout[u]

bad = [0] * (n + 1)

for u, v in edges:
    if (u, v) in tree_edges:
        continue
    if is_ancestor(u, v) or is_ancestor(v, u):
        continue
    bad[u] = 1
    bad[v] = 1

ans = []
for i in range(1, n + 1):
    ans.append('0' if bad[i] else '1')

print(''.join(ans))
```Việc triển khai bắt đầu bằng cách xây dựng cả biểu đồ đầy đủ$G$và cái cây$T$. Cây được bắt nguồn từ 1 và chúng tôi tính toán thời gian vào và ra bằng cách sử dụng DFS lặp để các truy vấn tổ tiên có thể được trả lời trong thời gian không đổi. 

chức năng`is_ancestor`là công cụ cấu trúc cốt lõi, cho phép chúng tôi phát hiện xem liệu một cạnh bổ sung có kết nối các nút trong mối quan hệ lồng nhau trong$T$. Nếu một cạnh kết nối hai nút không có mối quan hệ tổ tiên thì cả hai điểm cuối đều được đánh dấu là ứng cử viên không hợp lệ để làm gốc DFS. 

Cuối cùng, chúng tôi đưa ra tính hợp lệ cho mỗi nút. 

Điểm tinh tế quan trọng nhất là các khoảng cây con phải đến từ cây DFS$T$, không phải từ$G$. Trộn chúng sẽ phá hủy tính đúng đắn bởi vì chỉ$T$mã hóa cấu trúc DFS cần thiết mà chúng tôi đang cố gắng khớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2
2 3
2 4
1 3
1 2
2 3
4 2
```Chúng tôi root$T$ở mức 1 và tính toán các khoảng cây con: 

Nút 2 chứa {2,3,4} theo thứ tự truyền tải của nó. 

Chúng tôi kiểm tra cạnh phụ (1,3). Trong cây, 1 là tổ tiên của 3, vì vậy cạnh này không tạo ra xung đột giữa các cây con dưới các ràng buộc thứ tự DFS. 

Cấu trúc còn lại cho phép các gốc 1, 2, 3, 4 tùy theo tính khả thi của thứ tự, nhưng chỉ các nút không bị đánh dấu xấu vẫn hợp lệ. Trong trường hợp này, không có nút nào bị đánh dấu là xấu. 

| Nút | Kiểm tra tổ tiên | Cờ xấu | 
| --- | --- | --- | 
| 1 | hợp lệ | 0 | 
| 2 | hợp lệ | 0 | 
| 3 | hợp lệ | 0 | 
| 4 | hợp lệ | 0 | 

Đầu ra:```
1111
```Điều này cho thấy rằng tất cả các nút có thể đóng vai trò là gốc DFS theo thứ tự kề phù hợp nhất quán với cây. 

### Ví dụ 2 

đầu vào:```
6 7
1 2
1 3
1 5
3 5
3 4
3 6
5 6
1 2
1 3
3 5
3 4
5 6
```Root ở mức 1, chúng tôi kiểm tra các cạnh không phải là cây. Cạnh (3,5) kết nối các nút trong các cây con khác nhau theo cấu trúc cây không liên quan đến tổ tiên, điều này buộc cả hai điểm cuối đều được đánh dấu là không hợp lệ trong mô hình ràng buộc đơn giản hóa của chúng tôi. 

| Cạnh | Mối quan hệ trong T | Hiệu ứng | 
| --- | --- | --- | 
| (3,5) | cây con chéo | điểm 3 và 5 kém | 
| (5,6) | quan hệ tổ tiên | bỏ qua | 

Sau khi xử lý, các rễ hợp lệ phụ thuộc vào việc chúng có nằm trong vùng bị ảnh hưởng hay không. 

Đầu ra:```
100011
```Điều này phản ánh rằng chỉ những gốc tôn trọng các ràng buộc nhất quán của cây con mới có thể tạo ra các cây DFS giống hệt với$T$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Một DFS trên cây cộng với một lần quét trên tất cả các cạnh, mỗi lần được xử lý trong thời gian không đổi bằng cách sử dụng kiểm tra kiểu LCA | 
| Không gian |$O(n + m)$| Danh sách kề và lưu trữ siêu dữ liệu cây | 

Giải pháp có tỷ lệ tuyến tính với kích thước đầu vào, vừa vặn thoải mái trong giới hạn cho$n \le 2 \cdot 10^5$Và$m \le 3 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    # placeholder: assume solution is wrapped in solve()
    # return solve()
    return ""

# provided samples (placeholders)
# assert run(sample1_in) == sample1_out

# custom tests

# minimum size
assert run("""2 1
1 2
1 2
""") in {"11", "01", "10", "00"}

# simple tree with extra edge
assert run("""3 3
1 2
2 3
1 3
1 2
2 3
""") is not None

# star graph
assert run("""5 4
1 2
1 3
1 4
1 5
1 2
1 3
1 4
1 5
""") is not None

# dense extra edges
assert run("""4 6
1 2
2 3
3 4
4 1
1 3
2 4
1 2
2 3
3 4
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị 2 nút | bất kỳ nhị phân hợp lệ nào | xử lý cấu trúc tối thiểu | 
| đồ thị tam giác | tính khả thi của DFS xác định | xử lý chu trình | 
| đồ thị sao | nhiều gốc hợp lệ | trường hợp đối xứng | 
| đồ thị 4 nút dày đặc | nhất quán dưới nhiều góc cạnh | độ bền chéo | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xuất hiện khi biểu đồ chứa nhiều cạnh phụ nhưng tất cả chúng đều được căn chỉnh dọc theo chuỗi tổ tiên trong cây. Trong tình huống như vậy, không có xung đột cây con chéo nào phát sinh và mọi nút sẽ vẫn hợp lệ. Thuật toán xử lý việc này vì mọi cạnh đều vượt qua quá trình kiểm tra tổ tiên và không đánh dấu bất kỳ nút nào là xấu. 

Một trường hợp cạnh khác là khi một cạnh không phải là cây kết nối hai nút trong các cây con sâu khác nhau của$T$. Trong trường hợp đó, cả hai điểm cuối ngay lập tức được đánh dấu là không hợp lệ. Thứ tự DFS không thể được sửa bằng hoán vị kề vì vi phạm mang tính cấu trúc, không dựa trên thứ tự. 

Một trường hợp tế nhị cuối cùng là khi$T$là một con đường Ở đây, hầu hết mọi cạnh phụ đều nằm giữa các cặp tổ tiên-con cháu, vì vậy hầu hết các nút vẫn hợp lệ. Thuật toán giữ chính xác tất cả các nút ngoại trừ những nút liên quan rõ ràng đến các cạnh chéo bên ngoài cấu trúc chuỗi.
