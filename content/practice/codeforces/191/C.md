---
title: "CF 191C - Những kẻ ngốc và những con đường"
description: "Đầu vào mô tả một cây thành phố. Vì có chính xác một đường đi đơn giản giữa mỗi cặp thành phố nên mạng lưới đường bộ tạo thành một biểu đồ không theo chu kỳ được kết nối. Sau đó, chúng tôi được cung cấp một số cặp thành phố."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 191
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 121 (Div. 1)"
rating: 1900
weight: 191
solve_time_s: 115
verified: true
draft: false
---

[CF 191C - Những kẻ ngốc và những con đường](https://codeforces.com/problemset/problem/191/C) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, cây 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một cây thành phố. Vì có chính xác một đường đi đơn giản giữa mỗi cặp thành phố nên mạng lưới đường bộ tạo thành một biểu đồ không theo chu kỳ được kết nối. 

Sau đó, chúng tôi được cung cấp một số cặp thành phố. Mỗi cặp đại diện cho một chuyến đi: một kẻ ngốc đi từ thành phố`a`đến thành phố`b`dọc theo con đường đơn giản độc đáo trong cây. Với mỗi con đường, chúng ta phải đếm xem có bao nhiêu chuyến đi sử dụng con đường đó. 

Một cách giải thích trực tiếp sẽ là: cho mọi truy vấn`(a, b)`, tìm đường đi từ`a`ĐẾN`b`và tăng số lượng trên mỗi cạnh của đường dẫn đó. Điều đó đúng về mặt logic, vì cây đảm bảo tính duy nhất của tuyến đường. 

Các ràng buộc ngay lập tức loại trừ cách tiếp cận đó. Cây có tới`10^5`các đỉnh và cũng có thể có`10^5`truy vấn. Một đường dẫn có thể chứa`O(n)`các cạnh trong trường hợp xấu nhất, ví dụ như trong cây hình dây chuyền. Điều đó mang lại`O(nk)`các hoạt động có thể trở thành`10^10`, vượt xa giới hạn. 

Cấu trúc của một cái cây mang lại cho chúng ta thứ gì đó mạnh mẽ hơn nhiều. Mọi đường dẫn giữa hai nút có thể được biểu diễn bằng Tổ tiên chung thấp nhất của chúng, thường được viết tắt là LCA. Thay vì đi từng bước một cách rõ ràng trên mỗi truy vấn, chúng tôi có thể đánh dấu các điểm cuối và sau đó tổng hợp các đóng góp bằng một DFS. 

Một số trường hợp khó khăn rất dễ bị xử lý sai nếu thực hiện bất cẩn. 

Hãy xem xét một truy vấn trong đó cả hai điểm cuối đều là cùng một thành phố:```
3
1 2
2 3
1
2 2
```Không có đường nào được sử dụng nên kết quả đúng là:```
0 0
```Việc triển khai đơn giản luôn tăng dọc theo đường dẫn mà không kiểm tra các đường dẫn trống có thể vô tình đếm các cạnh. 

Một trường hợp tinh tế khác xuất hiện khi một điểm cuối là tổ tiên của điểm cuối kia:```
4
1 2
2 3
3 4
1
2 4
```Đường dẫn chỉ sử dụng các cạnh`(2,3)`Và`(3,4)`. Nếu việc xử lý LCA trừ đi hai lần từ cấp độ gốc của LCA thay vì chính LCA thì số đếm sẽ không chính xác. 

Cây hình ngôi sao cũng nhấn mạnh tính đúng đắn:```
5
1 2
1 3
1 4
1 5
2
2 3
4 5
```Số cạnh trung tâm phải là:```
2 2 2 2
```Mỗi chuyến đi đều đi qua gốc rễ. Những sai sót trong việc tích lũy cây con thường tính thiếu các cấu hình như vậy vì nhiều đường dẫn chồng chéo lên nhau tại một nút. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Đối với mỗi cặp`(a, b)`, chúng ta tìm đường đi giữa chúng và tăng tất cả các cạnh trên đường đi đó. 

Vì biểu đồ là một cái cây nên một cách để thực hiện điều này là sử dụng DFS hoặc cha mẹ leo lên. Giải pháp này đúng vì mỗi cặp có chính xác một đường đi đơn giản, do đó việc truy cập các cạnh đó trực tiếp khớp với định nghĩa bài toán. 

Vấn đề là quy mô. Một đường dẫn có thể chứa`n - 1`các cạnh. Với`10^5`các truy vấn, công việc trong trường hợp xấu nhất sẽ trở thành đại khái:```
10^5 × 10^5 = 10^10
```Đó là một số bậc độ lớn quá chậm. 

Quan sát quan trọng là chúng ta không thực sự cần cập nhật các cạnh ngay lập tức. Chúng ta chỉ cần số đường đi cuối cùng đi qua mỗi cạnh. 

Giả sử chúng ta thêm`+1`ở cả hai điểm cuối của đường dẫn truy vấn, sau đó trừ đi`2`tại LCA của họ. Nếu sau này chúng ta thực hiện DFS từ các lá trở lên thì mỗi cây con sẽ tích lũy bao nhiêu đường dẫn hoạt động đi qua nó. 

Tại sao điều này làm việc? 

Hãy tưởng tượng một cạnh kết nối một nút`u`tới cha mẹ của nó. Sau khi tổng hợp, giá trị được lưu trữ tại`u`bằng số đường đi vào cây con có gốc tại`u`trừ đi số đường dẫn cũng để nó bên trong cây con. Chính xác những đường đi còn lại phải vượt qua ranh giới giữa`u`và cha mẹ của nó. 

Điều này biến đổi các cập nhật cạnh trên mỗi đường dẫn đắt tiền thành: 

1. Đánh dấu điểm cuối theo thời gian không đổi cho mỗi truy vấn. 
2. Tích lũy một DFS trên cây. 

Thách thức duy nhất còn lại là tính toán LCA một cách hiệu quả. Nâng nhị phân giải quyết điều đó trong`O(log n)`mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Tối ưu | O((n + k) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách cây kề từ các cạnh đầu vào. 

Chúng ta cần một cấu trúc cho phép truyền tải DFS và tiền xử lý tổ tiên. 
2. Chạy DFS từ một gốc tùy ý, thường là nút`1`. 

Trong DFS này, lưu trữ: 

- nút cha của mỗi nút 
- độ sâu của mỗi nút 
- chỉ số cạnh kết nối một nút với nút cha của nó 

Các giá trị này cần thiết cho cả tính toán LCA và gán câu trả lời cho các cạnh theo thứ tự đầu vào. 
3. Xây dựng bàn nâng nhị phân. 

Cho phép`up[v][j]`là`2^j`- tổ tiên thứ của nút`v`. 

Điều này cho phép chúng ta di chuyển lên trên theo thời gian logarit thay vì leo từng cạnh một. 
4. Đối với mọi truy vấn`(a, b)`, tính toán`lca(a, b)`. 

Sau đó áp dụng: 

-`cnt[a] += 1`-`cnt[b] += 1`-`cnt[lca] -= 2`Điều này mã hóa đường dẫn một cách ngầm định thay vì chạm trực tiếp vào mọi cạnh. 
5. Chạy DFS thứ hai từ thư mục gốc. 

Đối với mỗi cây con, hãy tính toán đệ quy giá trị tích lũy của nó và thêm nó vào nút hiện tại. 

Nếu một cây con con trả về giá trị`x`, thì chính xác`x`các đường dẫn đi qua ranh giới giữa đứa trẻ đó và cha mẹ của nó. 
6. Lưu trữ giá trị cây con được trả về làm kết quả cho cạnh kết nối nút con với nút cha của nó. 

Vì mỗi cạnh thuộc về chính xác một mối quan hệ con-cha trong cây có gốc nên mỗi cạnh nhận được chính xác một câu trả lời. 
7. Xuất câu trả lời theo thứ tự đầu vào ban đầu. 

### Tại sao nó hoạt động 

Mỗi truy vấn thêm một đơn vị ở cả hai điểm cuối của đường dẫn. Khi tổng số cây con được tổng hợp trở lên, các đóng góp sẽ bị hủy bên trong đường dẫn ngoại trừ dọc theo tuyến đường thực tế giữa các điểm cuối. 

Trừ`2`tại LCA là sự điều chỉnh quan trọng. Nếu không có nó, khu vực tổ tiên chung sẽ được tính hai lần. Sau khi hủy, mỗi cạnh sẽ nhận được chính xác số đường dẫn truy vấn đi từ cạnh này sang cạnh kia. 

Đối với một cạnh giữa cha mẹ`p`và đứa trẻ`u`, tổng cây con tại`u`bằng số lượng điểm cuối truy vấn bên trong cây con đó trừ đi số lượng đường dẫn hoàn chỉnh chứa đầy trong đó. Mọi con đường chưa từng có còn lại đều phải vượt qua ranh giới`(p, u)`. Số lượng đó chính xác là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

LOG = 17 + 1

def solve():
    n = int(input())

    graph = [[] for _ in range(n + 1)]

    edges = []

    for idx in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append((v, idx))
        graph[v].append((u, idx))
        edges.append((u, v))

    parent = [[0] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)
    edge_id = [-1] * (n + 1)

    def dfs(v, p):
        parent[0][v] = p

        for to, idx in graph[v]:
            if to == p:
                continue

            depth[to] = depth[v] + 1
            edge_id[to] = idx
            dfs(to, v)

    dfs(1, 0)

    for j in range(1, LOG):
        for v in range(1, n + 1):
            parent[j][v] = parent[j - 1][parent[j - 1][v]]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a

        diff = depth[a] - depth[b]

        for j in range(LOG):
            if diff & (1 << j):
                a = parent[j][a]

        if a == b:
            return a

        for j in range(LOG - 1, -1, -1):
            if parent[j][a] != parent[j][b]:
                a = parent[j][a]
                b = parent[j][b]

        return parent[0][a]

    k = int(input())

    cnt = [0] * (n + 1)

    for _ in range(k):
        a, b = map(int, input().split())

        g = lca(a, b)

        cnt[a] += 1
        cnt[b] += 1
        cnt[g] -= 2

    ans = [0] * (n - 1)

    def dfs2(v, p):
        total = cnt[v]

        for to, idx in graph[v]:
            if to == p:
                continue

            subtotal = dfs2(to, v)

            ans[idx] = subtotal
            total += subtotal

        return total

    dfs2(1, 0)

    print(*ans)

solve()
```DFS đầu tiên root cây và ghi lại thông tin cấu trúc. các`edge_id`mảng ghi nhớ cạnh đầu vào nào kết nối một nút với nút cha của nó. Việc ánh xạ đó là cần thiết vì các câu trả lời phải được in theo thứ tự đầu vào ban đầu thay vì thứ tự DFS. 

Bàn nâng nhị phân lưu trữ tổ tiên theo lũy thừa của hai khoảng cách. Giá trị của`LOG`chỉ cần thỏa mãn`2^LOG > n`. Với`n ≤ 10^5`, mười tám cấp là đủ. 

Hàm LCA trước tiên cân bằng độ sâu, sau đó nâng cả hai nút lên cùng nhau cho đến khi nút gốc của chúng khớp nhau. Điều này mang lại độ phức tạp logarit cho mỗi truy vấn. 

Bước mã hóa đường dẫn là trọng tâm của giải pháp. Việc thêm vào cả hai điểm cuối và trừ hai lần khỏi LCA sẽ tạo ra biểu diễn kiểu mảng khác biệt trên cây. Không có cạnh nào được cập nhật trực tiếp. 

DFS thứ hai thực hiện việc tích lũy. Giá trị trả về từ cây con con bằng số đường đi qua cạnh nối cây con đó với cây mẹ của nó. Đó là lý do tại sao`ans[idx] = subtotal`là đúng. 

Một chi tiết triển khai tinh tế là chỉ định câu trả lời cạnh trước khi thêm tổng phụ vào nút hiện tại. Tổng phụ thể hiện lưu lượng truy cập qua chính xác cạnh đó. Sau khi nó hợp nhất vào tổng số gốc, sự khác biệt đó sẽ biến mất. 

Một chi tiết khác là độ sâu đệ quy. Cây hình dây chuyền có thể có chiều sâu`10^5`, do đó giới hạn đệ quy phải được tăng lên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 2
1 3
2 4
2 5
2
1 4
3 5
```Cấu trúc cây:```
    1
   / \
  2   3
 / \
4   5
```Các truy vấn được`(1,4)`Và`(3,5)`. 

Tính toán LCA: 

| Truy vấn | LCA | 
| --- | --- | 
| 1 4 | 1 | 
| 3 5 | 1 | 

Sau khi cập nhật điểm cuối: 

| Nút | giá trị cnt | 
| --- | --- | 
| 1 | -2 | 
| 2 | 0 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 

Bây giờ tổng hợp từ dưới lên. 

| Nút đã được xử lý | Số tiền được trả lại | Trả lời cạnh | 
| --- | --- | --- | 
| 4 | 1 | cạnh(2,4)=1 | 
| 5 | 1 | cạnh(2,5)=1 | 
| 2 | 2 | cạnh(1,2)=2 | 
| 3 | 1 | cạnh(1,3)=1 | 

Đầu ra cuối cùng:```
2 1 1 1
```Dấu vết này cho thấy nhiều đường dẫn kết hợp tự nhiên như thế nào thông qua tổng cây con. Cạnh`(1,2)`thuộc về cả hai tuyến đường, do đó giá trị tích lũy của nó trở thành`2`. 

### Ví dụ tùy chỉnh 

đầu vào:```
4
1 2
2 3
3 4
2
1 4
2 3
```Cây:```
1 - 2 - 3 - 4
```Tính toán LCA: 

| Truy vấn | LCA | 
| --- | --- | 
| 1 4 | 1 | 
| 2 3 | 2 | 

Cập nhật sự khác biệt: 

| Nút | cnt | 
| --- | --- | 
| 1 | -1 | 
| 2 | -1 | 
| 3 | 1 | 
| 4 | 1 | 

Tổng hợp từ dưới lên: 

| Nút đã được xử lý | Số tiền được trả lại | Trả lời cạnh | 
| --- | --- | --- | 
| 4 | 1 | cạnh(3,4)=1 | 
| 3 | 2 | cạnh(2,3)=2 | 
| 2 | 1 | cạnh(1,2)=1 | 

Đầu ra:```
1 2 1
```Ví dụ này thể hiện các đường dẫn chồng chéo trên một chuỗi. Cạnh giữa thuộc cả hai tuyến nên nó tích lũy số lượng`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + k) log n) | Tiền xử lý DFS là tuyến tính, mỗi truy vấn cần một phép tính LCA | 
| Không gian | O(n log n) | Bàn nâng nhị phân chiếm ưu thế trong việc sử dụng bộ nhớ | 

Với`n`Và`k`cả lên đến`10^5`, sự phức tạp này dễ dàng phù hợp với các giới hạn. Khoảng vài triệu thao tác được thực hiện, nằm trong giới hạn 2 giây trong Python khi được triển khai cẩn thận. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    LOG = 18

    n = int(input())

    graph = [[] for _ in range(n + 1)]

    for idx in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append((v, idx))
        graph[v].append((u, idx))

    parent = [[0] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)

    def dfs(v, p):
        parent[0][v] = p

        for to, idx in graph[v]:
            if to == p:
                continue

            depth[to] = depth[v] + 1
            dfs(to, v)

    dfs(1, 0)

    for j in range(1, LOG):
        for v in range(1, n + 1):
            parent[j][v] = parent[j - 1][parent[j - 1][v]]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a

        diff = depth[a] - depth[b]

        for j in range(LOG):
            if diff & (1 << j):
                a = parent[j][a]

        if a == b:
            return a

        for j in range(LOG - 1, -1, -1):
            if parent[j][a] != parent[j][b]:
                a = parent[j][a]
                b = parent[j][b]

        return parent[0][a]

    k = int(input())

    cnt = [0] * (n + 1)

    for _ in range(k):
        a, b = map(int, input().split())

        g = lca(a, b)

        cnt[a] += 1
        cnt[b] += 1
        cnt[g] -= 2

    ans = [0] * (n - 1)

    def dfs2(v, p):
        total = cnt[v]

        for to, idx in graph[v]:
            if to == p:
                continue

            subtotal = dfs2(to, v)

            ans[idx] = subtotal
            total += subtotal

        return total

    dfs2(1, 0)

    return " ".join(map(str, ans))

# provided sample
assert run(
"""5
1 2
1 3
2 4
2 5
2
1 4
3 5
"""
) == "2 1 1 1", "sample 1"

# minimum tree
assert run(
"""2
1 2
1
1 2
"""
) == "1", "single edge"

# no queries
assert run(
"""3
1 2
2 3
0
"""
) == "0 0", "no paths"

# chain with overlapping paths
assert run(
"""4
1 2
2 3
3 4
2
1 4
2 3
"""
) == "1 2 1", "overlapping chain"

# same-node query
assert run(
"""3
1 2
2 3
1
2 2
"""
) == "0 0", "empty path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây một cạnh |`1`| Cây không tầm thường nhỏ nhất | 
| Không có truy vấn |`0 0`| Xử lý đúng khối lượng công việc trống | 
| Chuỗi chồng chéo |`1 2 1`| Tích lũy cạnh chia sẻ | 
| Truy vấn cùng nút |`0 0`| Đường dẫn trống không được ảnh hưởng đến các cạnh | 

## Vỏ cạnh 

Hãy xem xét trường hợp truy vấn bắt đầu và kết thúc tại cùng một nút:```
3
1 2
2 3
1
2 2
```LCA của`(2,2)`là`2`. Trình tự cập nhật trở thành:```
cnt[2] += 1
cnt[2] += 1
cnt[2] -= 2
```Vì vậy, hiệu ứng ròng là bằng không. Trong quá trình tổng hợp cây con, mọi cây con đều trả về 0 và tất cả các câu trả lời cạnh vẫn bằng 0. Điều này mô hình chính xác rằng không có con đường nào được đi qua. 

Bây giờ hãy xem xét một truy vấn tổ tiên-con cháu:```
4
1 2
2 3
3 4
1
2 4
```LCA là`2`. Cập nhật trở thành:```
cnt[2] += 1
cnt[4] += 1
cnt[2] -= 2
```Giá trị ròng:```
cnt[2] = -1
cnt[4] = 1
```Tổng hợp từ dưới lên cho:```
edge(3,4) = 1
edge(2,3) = 1
edge(1,2) = 0
```Đường dẫn loại trừ chính xác cạnh phía trên tổ tiên. 

Cuối cùng, hãy xem xét một cây hình ngôi sao:```
5
1 2
1 3
1 4
1 5
2
2 3
4 5
```Mọi truy vấn đều đi qua nút`1`. Tổng cây con từ tất cả các lá trở thành:```
child 2 -> 1
child 3 -> 1
child 4 -> 1
child 5 -> 1
```Vì vậy, mọi cạnh kết nối với trung tâm đều được tính`1`và vì mỗi cặp sử dụng hai nan hoa khác nhau nên câu trả lời cuối cùng là:```
1 1 1 1
```Điều này xác nhận rằng lưu lượng truy cập chồng chéo qua thư mục gốc được tổng hợp chính xác mà không cần tính hai lần.
