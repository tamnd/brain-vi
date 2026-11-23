---
title: "CF 475E - Thành phố kết nối mạnh mẽ 2"
description: "Chúng tôi bắt đầu với một đồ thị vô hướng được kết nối. Mỗi cạnh phải được gán một hướng. Sau khi tất cả các hướng được chọn, một số cặp đỉnh có thứ tự $(u,v)$ sẽ thỏa mãn rằng có một đường đi có hướng từ $u$ đến $v$. Mục tiêu không phải là làm cho đồ thị được kết nối chặt chẽ."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 475
codeforces_index: "E"
codeforces_contest_name: "Bayan 2015 Contest Warm Up"
rating: 2700
weight: 475
solve_time_s: 146
verified: true
draft: false
---

[CF 475E - Thành phố được kết nối mạnh mẽ 2](https://codeforces.com/problemset/problem/475/E) 

**Đánh giá:** 2700 
**Thẻ:** dfs và tương tự 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một đồ thị vô hướng được kết nối. Mỗi cạnh phải được gán một hướng. Sau khi tất cả các hướng được chọn, một số cặp đỉnh có thứ tự$(u,v)$sẽ thỏa mãn rằng có một đường dẫn có hướng từ$u$ĐẾN$v$. 

Mục tiêu không phải là làm cho đồ thị được kết nối chặt chẽ. Mục tiêu là chọn các hướng tối đa hóa tổng số cặp thứ tự có thể tiếp cận được. Các cặp có dạng$(u,u)$đếm vì mọi đỉnh đều có thể chạm tới chính nó. 

Đồ thị được kết nối trước khi định hướng. Sau khi định hướng, nó trở thành một biểu đồ có hướng có các thành phần được kết nối chặt chẽ và cấu trúc khả năng tiếp cận phụ thuộc hoàn toàn vào các hướng đã chọn. 

Phần thú vị của vấn đề là đồ thị ban đầu có thể chứa các chu trình. Trong một chu trình, chúng ta thường có thể định hướng các cạnh sao cho mỗi đỉnh đều chạm tới mọi đỉnh khác. Các cây cầu hoạt động rất khác nhau, bởi vì bất kỳ hướng nào được chọn trên cây cầu sẽ hạn chế vĩnh viễn chuyển động qua mép đó. 

Các ràng buộc đủ nhỏ để$O(n^2 / 64)$kiểu tổng con DP trên cấu trúc nén cuối cùng, nhưng quá lớn đối với bất kỳ thứ gì thử tất cả các hướng. Một đồ thị với$m$các cạnh có$2^m$những định hướng khả thi, điều này gần như trở thành không thể ngay lập tức. 

Một sai lầm phổ biến là nghĩ rằng mọi cây cầu đều phải hướng về cùng một hướng toàn cầu. Coi như```
1 - 2 - 3
```Nếu chúng ta định hướng$1 \to 2 \to 3$, chúng ta thu được 6 cặp có thể truy cập được. 

Nếu thay vào đó chúng ta định hướng$1 \to 2 \leftarrow 3$, chúng ta chỉ thu được 5. 

Định hướng của những cây cầu có ý nghĩa toàn cầu chứ không phải cục bộ. 

Một cái bẫy dễ dàng khác là bỏ qua các chu trình và coi biểu đồ như một cái cây. Coi như```
1 - 2
| \ |
4 - 3
```Biểu đồ này không có cầu nối. Chúng ta có thể định hướng nó như một chu trình có hướng và mọi đỉnh đều chạm tới mọi đỉnh khác, cho$4^2 = 16$các cặp có thể truy cập được. Bất kỳ lý do dựa trên cây nào cũng sẽ đánh giá thấp câu trả lời. 

Trường hợp tinh tế thứ ba là một biểu đồ bao gồm một số khối nối 2 cạnh được nối với nhau bằng cầu. Ví dụ:```
(triangle) -- bridge -- (triangle)
```Mỗi tam giác có thể được thực hiện kết nối mạnh mẽ bên trong. Quyết định thực sự duy nhất là hướng đi của cây cầu. Bất kỳ giải pháp nào đếm các đỉnh riêng lẻ thay vì nén các khối sẽ trở nên phức tạp không cần thiết. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử mọi hướng của mọi cạnh, xây dựng biểu đồ có hướng thu được, tính toán khả năng tiếp cận và giữ lại câu trả lời tốt nhất. 

Điều đó đúng vì nó thực sự kiểm tra tất cả các khả năng. Thật không may, với$m$các cạnh có$2^m$định hướng. Ngay cả đối với$m=50$điều này thật vô vọng. 

Quan sát đầu tiên là các cây cầu là các cạnh duy nhất có thể phân tách đồ thị. Bất kỳ cạnh nào không phải là cầu đều thuộc về một thành phần nào đó được kết nối 2 cạnh. Một kết quả cổ điển của Robbins nói rằng mọi đồ thị liên thông không cầu nối đều thừa nhận một hướng liên thông mạnh. Sau khi tìm thấy tất cả các cầu, mọi thành phần không có cầu tối đa có thể được định hướng sao cho tất cả các đỉnh của nó chạm tới nhau. 

Giả sử một thành phần chứa$s$đỉnh. Sau khi định hướng nó kết nối mạnh mẽ, nó đóng góp chính xác$s^2$các cặp đặt hàng có thể truy cập được trong nội bộ. 

Điều này gợi ý việc nén mọi thành phần được kết nối 2 cạnh vào một nút có trọng số duy nhất có trọng số bằng kích thước của nó. Các cầu nối trở thành các cạnh giữa các nút có trọng số đó. Đồ thị nén là một cây, thường được gọi là cây cầu. 

Bây giờ vấn đề trở thành: 

Cho một cây có trọng số, hãy định hướng các cạnh của nó để tối đa hóa số lượng các cặp có thứ tự có thể tiếp cận được giữa các nút có trọng số. 

Điều này đơn giản hơn nhiều. Một thuộc tính quan trọng, được đề cập trong bài xã luận chính thức, là tồn tại một hướng tối ưu với một thành phần gốc đặc biệt sao cho mọi thành phần khác đều có thể đến được gốc hoặc có thể truy cập được từ gốc. 

Gốc đó phải là trọng tâm của cây có trọng số. Nếu chúng ta loại bỏ một trọng tâm thì mỗi phần còn lại có trọng lượng không quá một nửa tổng trọng lượng. Xung quanh tâm, mỗi cạnh lân cận có một trọng lượng nào đó$a_i$. 

Chọn một số cạnh hướng về tâm và các cạnh còn lại hướng ra xa tâm. 

Cho phép

-$r$là trọng lượng trung tâm. 
-$T = n-r$. 
-$X$là tổng trọng lượng của các cạnh hướng vào tâm. 
-$T-X$là tổng trọng lượng của các cạnh hướng ra xa tâm. 

Sau đó, các cặp có thể truy cập bổ sung được đóng góp bởi cấu trúc cây là$$Xr + r(T-X) + X(T-X)$$đơn giản hóa để$$rT + X(T-X).$$Để cố định$r$, tối đa hóa câu trả lời có nghĩa là tối đa hóa$X(T-X)$. Đây là một vấn đề phân vùng cổ điển. chúng tôi muốn$X$càng gần càng tốt$T/2$, Nhưng$X$phải là tổng trọng số của toàn bộ các cạnh lân cận. 

Điều đó trở thành DP tổng tập hợp con trên các kích thước cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^m \cdot (n+m))$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O(n+m+n^2/64)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy thuật toán cầu của Tarjan trên biểu đồ gốc và đánh dấu mọi cây cầu. 
2. Loại bỏ tất cả các cầu nối và DFS qua các cạnh còn lại. Mỗi vùng được kết nối sẽ trở thành một thành phần được kết nối 2 cạnh. 
3. Ghi lại kích thước của từng bộ phận. Nếu một thành phần chứa$s$đỉnh, thêm$s^2$để trả lời. Đây chính là sự đóng góp có được nhờ định hướng thành phần đó được kết nối mạnh mẽ. 
4. Xây dựng cây cầu. Mỗi thành phần trở thành một nút có trọng số có trọng số bằng kích thước của nó. Mỗi cây cầu ban đầu trở thành một cạnh giữa hai nút thành phần. 
5. Tính trọng số của cây con trong cây cầu và tìm tất cả các trọng tâm có trọng số. Trọng tâm là một nút mà việc loại bỏ nó không để lại phần nào có trọng số lớn hơn$n/2$. 
6. Đối với mỗi trọng tâm: 

1. Gọi trọng lượng của nó là$r$. 
2. Với mỗi cạnh kề, hãy tính tổng trọng lượng của nó$a_i$. 
3. Chạy tập hợp con DP trên các giá trị$a_i$. 
4. Với mỗi số tiền có thể đạt được$X$, đánh giá$$r(n-r) + X((n-r)-X).$$5. Giữ giá trị tối đa. 
7. Cộng phần đóng góp cây tối đa đó vào phần đóng góp nội bộ được tính ở bước 3. 

### Tại sao nó hoạt động 

Mọi cạnh không có cầu nối đều nằm bên trong thành phần được kết nối 2 cạnh. Một thành phần như vậy có thể được định hướng liên kết chặt chẽ, do đó tất cả các cặp đỉnh bên trong nó đều có thể tiếp cận được. Việc nén từng thành phần vào một nút có trọng số sẽ không làm mất thông tin liên quan đến mức tối ưu. 

Sau khi nén, đồ thị là một cây. Theo hướng tối ưu, tất cả các đường dẫn tạo khả năng tiếp cận giữa các nhánh khác nhau phải đi qua một thành phần trọng tâm duy nhất. Sau khi trọng tâm được cố định, mỗi cạnh lân cận sẽ nằm hoàn toàn ở phía "vào" hoặc hoàn toàn ở phía "ra". Khả năng tiếp cận giữa các bên khác nhau chỉ phụ thuộc vào tổng trọng số được gán cho hai nhóm đó. 

Nếu trọng lượng đến là$X$và trọng lượng đi là$T-X$, mọi đỉnh trong nhóm đến có thể đến mọi đỉnh trong nhóm đi qua tâm. Sự đóng góp kết quả là chính xác$$rT + X(T-X).$$Không có hướng nào khác có thể tạo ra nhiều khả năng tiếp cận giữa các nhóm hơn và việc tối đa hóa biểu thức này sẽ giảm xuống phân vùng tổng tập hợp con được mô tả ở trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    g = [[] for _ in range(n)]
    edges = []

    for eid in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v))
        g[u].append((v, eid))
        g[v].append((u, eid))

    tin = [-1] * n
    low = [0] * n
    timer = 0
    is_bridge = [False] * m

    sys.setrecursionlimit(1 << 20)

    def dfs_bridge(v, pe):
        nonlocal timer
        tin[v] = low[v] = timer
        timer += 1

        for to, eid in g[v]:
            if eid == pe:
                continue

            if tin[to] != -1:
                low[v] = min(low[v], tin[to])
            else:
                dfs_bridge(to, eid)
                low[v] = min(low[v], low[to])

                if low[to] > tin[v]:
                    is_bridge[eid] = True

    dfs_bridge(0, -1)

    comp = [-1] * n
    comp_size = []

    def dfs_comp(start, cid):
        stack = [start]
        comp[start] = cid
        sz = 0

        while stack:
            v = stack.pop()
            sz += 1

            for to, eid in g[v]:
                if is_bridge[eid]:
                    continue
                if comp[to] == -1:
                    comp[to] = cid
                    stack.append(to)

        return sz

    cc = 0
    base = 0

    for v in range(n):
        if comp[v] == -1:
            sz = dfs_comp(v, cc)
            comp_size.append(sz)
            base += sz * sz
            cc += 1

    if cc == 1:
        print(base)
        return

    tree = [[] for _ in range(cc)]

    for eid, (u, v) in enumerate(edges):
        if is_bridge[eid]:
            a = comp[u]
            b = comp[v]
            tree[a].append(b)
            tree[b].append(a)

    total_weight = n

    parent = [-1] * cc
    sub = [0] * cc

    def dfs_sub(v, p):
        parent[v] = p
        s = comp_size[v]

        for to in tree[v]:
            if to == p:
                continue
            dfs_sub(to, v)
            s += sub[to]

        sub[v] = s

    dfs_sub(0, -1)

    centroids = []

    for v in range(cc):
        mx = total_weight - sub[v]

        for to in tree[v]:
            if to == parent[v]:
                continue
            mx = max(mx, sub[to])

        if mx * 2 <= total_weight:
            centroids.append(v)

    best_extra = 0

    for c in centroids:
        sides = []

        for to in tree[c]:
            if to == parent[c]:
                sides.append(total_weight - sub[c])
            else:
                sides.append(sub[to])

        r = comp_size[c]
        T = total_weight - r

        bits = 1
        for a in sides:
            bits |= bits << a

        best_partition = 0

        for x in range(T + 1):
            if (bits >> x) & 1:
                best_partition = max(best_partition, x * (T - x))

        best_extra = max(best_extra, r * T + best_partition)

    print(base + best_extra)

if __name__ == "__main__":
    solve()
```DFS đầu tiên tìm thấy tất cả các cây cầu sử dụng kỹ thuật liên kết thấp tiêu chuẩn. Một cạnh là một cây cầu chính xác khi cây con bên dưới nó không thể tiếp cận được cây tổ tiên của cây mẹ của nó. 

DFS thứ hai bỏ qua các cầu nối và gắn nhãn các thành phần được kết nối 2 cạnh. Kích thước của mỗi thành phần sẽ trở thành trọng số của một nút trong cây cầu. 

Biến`base`lưu trữ tổng của$s^2$over all compressed components. Phần câu trả lời đó không phụ thuộc vào hướng của cây cầu. 

The bridge tree is then built. Trọng số của cây con được tính toán một lần, cho phép thu được mọi cạnh lân cận của trọng tâm trong thời gian không đổi. 

Tổng tập hợp con DP sử dụng tập hợp bit được mã hóa dưới dạng số nguyên Python. Nếu bit`k`được thiết lập thì tổng trọng lượng của các cạnh là`k`là có thể đạt được. Chuyển bằng`a`và OR-ing thực hiện quá trình chuyển đổi ba lô tiêu chuẩn cực kỳ hiệu quả. 

Câu trả lời cuối cùng là đóng góp nội bộ cộng với đóng góp xuyên thành phần tốt nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4
1 2
1 3
1 4
1 5
```Đồ thị đã là một cái cây. 

Sau khi phân rã cầu, mỗi đỉnh tạo thành thành phần riêng của nó. 

| Số lượng | Giá trị | 
| --- | --- | 
| Trọng lượng thành phần | 1,1,1,1,1 | 
| Đóng góp cơ bản | 5 | 
| Trọng lượng trung tâm | 1 | 
| Trọng lượng bên | 1,1,1,1 | 
| T | 4 | 

Tổng tập hợp con có thể tạo ra$X=2$. 

| X | X(T-X) | 
| --- | --- | 
| 0 | 0 | 
| 1 | 3 | 
| 2 | 4 | 
| 3 | 3 | 
| 4 | 0 | 

Đóng góp cây tốt nhất:$$1 \cdot 4 + 4 = 8.$$Tổng cộng:$$5 + 8 = 13.$$### Mẫu 4 

đầu vào:```
6 7
1 2
2 3
1 3
1 4
4 5
5 6
6 4
```Có hai thành phần không có cầu nối. 

| Thành phần | Kích thước | 
| --- | --- | 
| {1,2,3} | 3 | 
| {4,5,6} | 3 | 

Đóng góp cơ bản:$$3^2 + 3^2 = 18.$$Cây cầu chứa hai nút được kết nối bằng một cây cầu. 

| Số lượng | Giá trị | 
| --- | --- | 
| Trọng lượng trung tâm | 3 | 
| Trọng lượng bên | 3 | 
| T | 3 | 

Chỉ một$X=0$hoặc$X=3$là có thể.$$3 \cdot 3 + 0 = 9.$$Tổng cộng:$$18 + 9 = 27.$$Ví dụ này cho thấy tại sao việc nén các thành phần không có cầu nối là cần thiết. Toàn bộ vấn đề giảm xuống còn việc định hướng một cây cầu duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m + n^2/64)$| Tìm cầu, xây dựng thành phần, xử lý cây và tổng tập hợp con bitset | 
| Không gian |$O(n + m)$| Đồ thị, cây cầu, mảng DFS | 

Việc duyệt đồ thị là tuyến tính. Phần phi tuyến tính duy nhất là tập hợp bit tổng con, có chi phí hiệu quả là$O(n^2/64)$. Điều đó thoải mái phù hợp trong giới hạn cuộc thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    # paste solve() here and return captured output
    pass

# provided samples
assert run(
"""5 4
1 2
1 3
1 4
1 5
""") == "13"

assert run(
"""4 5
1 2
2 3
3 4
4 1
1 3
""") == "16"

assert run(
"""2 1
1 2
""") == "3"

assert run(
"""6 7
1 2
2 3
1 3
1 4
4 5
5 6
6 4
""") == "27"

# single vertex
assert run(
"""1 0
""") == "1"

# simple path of length 2
assert run(
"""3 2
1 2
2 3
""") == "6"

# triangle, no bridges
assert run(
"""3 3
1 2
2 3
3 1
""") == "9"

# two cycles connected by one bridge
assert run(
"""6 7
1 2
2 3
3 1
3 4
4 5
5 6
6 4
""") == "27"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 1 | Đồ thị nhỏ nhất có thể | 
| Đường đi có độ dài 2 | 6 | Hành vi cây thuần túy | 
| Tam giác | 9 | Đồ thị không có cầu | 
| Hai chu kỳ và một cây cầu | 27 | Nén thành phần chính xác | 

## Vỏ cạnh 

Hãy xem xét một biểu đồ không có cầu nối:```
4 5
1 2
2 3
3 4
4 1
1 3
```Toàn bộ biểu đồ trở thành một thành phần có kích thước 4. Cây cầu chứa một nút duy nhất. Thuật toán ngay lập tức trở lại$$4^2 = 16.$$Không cần có giai đoạn tổng hợp con. 

Bây giờ hãy xem xét một cây thuần túy:```
2 1
1 2
```Mỗi đỉnh trở thành thành phần riêng của nó. Cây cầu có hai nút có trọng số 1. Trọng tâm có trọng số 1, cạnh duy nhất có trọng số 1 và công thức cho$$1 + (1 \cdot 1) + 0 = 3.$$Điều này phù hợp với mẫu. 

Cuối cùng, hãy xem xét hai khối lớn được nối với nhau bằng một cây cầu:```
1-2-3-1   bridge   4-5-6-4
```Sự phân rã tạo ra trọng số thành phần$3$Và$3$. Khả năng tiếp cận nội bộ góp phần$18$. Định hướng cầu góp phần$9$. Câu trả lời là$27$. 

Trường hợp này thể hiện tính bất biến trung tâm của lời giải: khi các thành phần kết nối 2 cạnh được nén, chỉ có cây cầu ảnh hưởng đến khả năng tiếp cận bổ sung.
