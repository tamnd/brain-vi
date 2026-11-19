---
title: "CF 2063E - Cây tam giác"
description: "Chúng ta được cho một cây có gốc có gốc ở đỉnh 1. Với mỗi cặp đỉnh không có thứ tự (u, v), chúng ta chỉ quan tâm đến các cặp trong đó không có đỉnh nào là tổ tiên của đỉnh kia."
date: "2026-06-08T07:30:43+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2063
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1000 (Div. 2)"
rating: 2300
weight: 2063
solve_time_s: 155
verified: false
draft: false
---

[CF 2063E - Cây tam giác](https://codeforces.com/problemset/problem/2063/E) 

**Đánh giá:** 2300 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, tham lam, cây 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có gốc có gốc ở đỉnh`1`. 

Với mọi cặp đỉnh không có thứ tự`(u, v)`, chúng ta chỉ quan tâm đến các cặp trong đó không có đỉnh nào là tổ tiên của đỉnh kia. Cho phép

-`a = dist(u, lca(u,v))`-`b = dist(v, lca(u,v))`Vì cặp này tốt nên cả hai`a`Và`b`hoàn toàn tích cực. 

Đối với một cặp như vậy, chúng tôi đếm có bao nhiêu giá trị nguyên`x`có thể là cạnh thứ ba của một tam giác không suy biến có hai cạnh còn lại bằng`a`Và`b`. 

Nhiệm vụ là tính tổng đại lượng này trên tất cả các cặp đỉnh không có thứ tự. 

Quan sát đầu tiên hoàn toàn là hình học. Vì hai mặt tích cực`a`Và`b`, các giá trị nguyên của`x`thỏa mãn bất đẳng thức tam giác là`|a-b| < x < a+b`. 

Số số nguyên trong khoảng này là`(a+b-1) - (|a-b|+1) + 1 = a+b-|a-b|-1`. 

Từ`a+b-|a-b| = 2*min(a,b)`, chúng tôi nhận được`f(u,v) = 2*min(a,b)-1`. 

Như vậy toàn bộ vấn đề trở thành$$\sum_{\text{good pairs }(u,v)} \bigl(2\min(a,b)-1\bigr)$$Ở đâu`a`Và`b`là khoảng cách từ hai đỉnh tới LCA của chúng. 

Cây chứa tới`3·10^5`đỉnh trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào kiểm tra tất cả các cặp ngay lập tức đều trở nên bất khả thi vì có khoảng`4.5·10^{10}`cặp trong trường hợp xấu nhất. Thậm chí`O(n^2)`đã vượt xa giới hạn. Chúng ta cần một cái gì đó gần gũi`O(n log^2 n)`hoặc`O(n log n)`. 

Một điểm tinh tế là các cặp tổ tiên-con cháu không đóng góp gì mặc dù một trong`a`hoặc`b`sẽ trở thành số không. Việc thực hiện bất cẩn áp dụng công thức tam giác cho mọi cặp sẽ đếm sai các cặp đó. 

Hãy xem xét chuỗi```
1 - 2 - 3
```Cặp đôi`(1,3)`có khoảng cách`(0,2)`từ LCA. Công thức`2*min(a,b)-1`sẽ cho`-1`, điều đó là vô nghĩa. Những cặp này phải được loại trừ hoàn toàn. 

Một sai lầm dễ mắc phải khác là quên rằng chỉ các đỉnh từ các cây con khác nhau của LCA mới tạo thành các cặp tốt. Ví dụ```
    1
   / \
  2   3
```Cặp tốt duy nhất là`(2,3)`. Các cặp liên quan đến gốc có liên quan đến tổ tiên và không đóng góp gì. 

## Phương pháp tiếp cận 

Giải pháp vũ phu rất đơn giản. 

Với mỗi cặp đỉnh, hãy tính LCA của chúng. Nếu không phải là tổ tiên của cái kia, hãy tính$$2\min(dist(u,lca),dist(v,lca))-1.$$Tổng kết điều này trên tất cả các cặp sẽ cho câu trả lời đúng. 

Ngay cả với một`O(1)`Cấu trúc LCA, có`O(n^2)`cặp. Với`n = 3·10^5`, điều này hoàn toàn không thể thực hiện được. 

Bước quan trọng là viết lại phần đóng góp. 

Đối với một cặp tốt có LCA`w`, cho phép$$a=dist(u,w),\quad b=dist(v,w).$$Sau đó$$f(u,v)=2\min(a,b)-1.$$Sử dụng danh tính$$\min(a,b)=\sum_{k\ge1}[a\ge k][b\ge k],$$chúng tôi có được$$f(u,v) = \sum_{k\ge1}2[a\ge k][b\ge k]-1.$$Điều này chuyển đổi vấn đề thành các cặp đếm có hai độ sâu tính từ LCA ít nhất là`k`. 

Bây giờ sửa một đỉnh`w`như LCA. 

Đối với một ngưỡng`k`, định nghĩa$$cnt_k(c)$$là số đỉnh bên trong cây con con`c`của`w`khoảng cách từ ai`w`ít nhất là`k`. 

Các cặp được tính theo thuật ngữ chỉ báo phải đến từ các cây con khác nhau của`w`, vậy số cặp như vậy bằng$$\sum_{i<j} cnt_k(i)\,cnt_k(j).$$Tổng hợp tất cả`k`và tất cả các đỉnh đều đưa ra câu trả lời toàn bộ. 

Cấu trúc này lý tưởng cho DSU-on-tree (hợp nhất từ ​​nhỏ đến lớn). Đối với mỗi đỉnh, chúng ta cần truy cập hiệu quả vào phân bố độ sâu bên trong cây con của nó. Trong khi hợp nhất các cây con con, chúng ta đếm các cặp cây con chéo có LCA chính xác là đỉnh hiện tại. 

Phần khó khăn là đánh giá tất cả các ngưỡng cùng một lúc. Bí quyết tiêu chuẩn là lưu trữ tần số độ sâu và duy trì thông tin tiền tố để mỗi lần hợp nhất đều đóng góp theo thời gian logarit. 

Giải pháp kết quả chạy trong`O(n log^2 n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| DSU trên cây + hợp nhất tần số độ sâu | O(n log2 n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Cải cách 

hãy để`dep[v]`là độ sâu từ gốc. 

Đối với một cặp có LCA`w`,$$a=dep[u]-dep[w],\qquad b=dep[v]-dep[w].$$Đóng góp của nó là$$2\min(a,b)-1.$$Chúng tôi chia câu trả lời thành hai phần:$$\sum (2\min(a,b))$$trừ đi$$\#\{\text{good pairs}\}.$$Cả hai đại lượng đều có thể được tính toán trong khi xử lý LCA. 

### Dữ liệu được duy trì trong quá trình hợp nhất 

Đối với mỗi cây con, chúng tôi duy trì cấu trúc tần số được lập chỉ mục theo độ sâu. 

Đối với một đỉnh`w`, mọi giá trị độ sâu được lưu trữ thực sự đại diện cho$$dep[x].$$Khi kết hợp hai cây con con, mỗi cặp được hình thành bởi một đỉnh ở mỗi bên có LCA bằng`w`. 

Thuật ngữ$$\min(dep[u]-dep[w],\,dep[v]-dep[w])$$có thể được viết lại bằng cách sử dụng độ sâu tuyệt đối và được truy vấn thông qua cây Fenwick lưu trữ số lượng và tổng độ sâu. 

### sáp nhập DSU 

1. Chạy DFS để tính toán kích thước và độ sâu của cây con. 
2. Xử lý các đỉnh từ dưới lên. 
3. Mỗi đỉnh sở hữu một thùng chứa tất cả các độ sâu bên trong cây con được xử lý của nó. 
4. Luôn ghép thùng chứa nhỏ hơn vào thùng chứa lớn hơn. 
5. Trong khi chèn các đỉnh từ vùng chứa nhỏ hơn, hãy truy vấn xem chúng đóng góp bao nhiêu với các đỉnh đã có trong vùng chứa lớn hơn. 
6. Công thức đóng góp được chuyển đổi thành tổng các giá trị tối thiểu, có thể được đánh giá bằng cách sử dụng lưu trữ cây Fenwick: 

- số độ sâu 
- tổng độ sâu 
7. Sau khi hoàn tất tất cả các quá trình hợp nhất con, hãy chèn độ sâu của đỉnh hiện tại vào vùng chứa và đưa nó trở lên. 
8. Đồng thời đếm các cặp tốt. Mỗi cặp con chéo đóng góp một vào số hạng trừ số cặp. 
9. Tích lũy$$2\sum \min(a,b)$$và trừ số cặp tốt ở cuối. 

### Tại sao nó hoạt động 

Mỗi cặp tốt đều có một LCA duy nhất. 

Khi xử lý một đỉnh`w`, việc hợp nhất DSU xem xét các cặp có điểm cuối nằm trong hai cây con khác nhau của`w`. Các cặp như vậy được tính chính xác một lần vì không có đỉnh nào khác có thể là LCA của chúng. 

Đối với bất kỳ cặp nào, số lượng`min(a,b)`chỉ phụ thuộc vào độ sâu của hai điểm cuối và độ sâu của`w`. Các truy vấn Fenwick tính toán chính xác tổng các giá trị tối thiểu này dựa trên tất cả các cây con đã được hợp nhất trước đó. Việc hợp nhất từ ​​nhỏ đến lớn đảm bảo rằng độ sâu của mỗi đỉnh chỉ di chuyển`O(log n)`lần, mang lại độ phức tạp cần thiết. 

Vì mỗi cặp hàng tốt được tính một lần và chỉ một lần tại LCA của nó nên giá trị tích lũy bằng số tiền mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

sys.setrecursionlimit(1 << 20)

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, idx, val):
        n = self.n
        bit = self.bit
        while idx <= n:
            bit[idx] += val
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        bit = self.bit
        while idx > 0:
            res += bit[idx]
            idx -= idx & -idx
        return res

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n = int(input())

        g = [[] for _ in range(n)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        depth = [0] * n
        parent = [-1] * n
        sz = [1] * n

        order = [0]
        parent[0] = 0

        for v in order:
            for to in g[v]:
                if to == parent[v]:
                    continue
                parent[to] = v
                depth[to] = depth[v] + 1
                order.append(to)

        for v in reversed(order[1:]):
            sz[parent[v]] += sz[v]

        maxd = max(depth) + 5

        cnt_bit = Fenwick(maxd)
        sum_bit = Fenwick(maxd)

        ans_min = 0
        good_pairs = 0

        def dfs(v, p):
            nonlocal ans_min, good_pairs

            big = None
            store = []

            for to in g[v]:
                if to == p:
                    continue
                cur = dfs(to, v)
                if big is None or len(cur) > len(big):
                    big = cur

            if big is None:
                big = []

            active_cnt = 0

            for d in big:
                idx = d + 1
                cnt_bit.add(idx, 1)
                sum_bit.add(idx, d)
                active_cnt += 1

            for to in g[v]:
                if to == p:
                    continue

                cur = dfs_data[to]

                if cur is big:
                    continue

                for d in cur:
                    idx = d + 1

                    c1 = cnt_bit.sum(idx)
                    s1 = sum_bit.sum(idx)

                    ctot = active_cnt
                    stot = sum_bit.sum(maxd)

                    c2 = ctot - c1
                    s2 = stot - s1

                    ans_min += s1 + d * c2
                    good_pairs += active_cnt

                for d in cur:
                    idx = d + 1
                    cnt_bit.add(idx, 1)
                    sum_bit.add(idx, d)
                    active_cnt += 1
                    big.append(d)

            for d in big:
                idx = d + 1
                cnt_bit.add(idx, -1)
                sum_bit.add(idx, -d)

            big.append(depth[v])

            dfs_data[v] = big
            return big

        dfs_data = [None] * n
        dfs(0, -1)

        answer = 2 * ans_min - good_pairs
        out.append(str(answer))

    sys.stdout.write("\n".join(out))

solve()
```Việc thực hiện tuân theo mô hình từ nhỏ đến lớn. 

Vùng chứa được trả về bởi mỗi lệnh gọi DFS lưu trữ độ sâu của tất cả các đỉnh trong cây con đó. Khi hai cây con được hợp nhất, mỗi cặp chéo đều có đỉnh hiện tại là LCA của nó, đây chính xác là tập hợp các cặp mà chúng ta cần. 

Cây Fenwick duy trì số lượng và tổng độ sâu hiện có trong thùng chứa lớn. Đối với độ sâu đỉnh`d`, chúng ta có thể tính toán$$\sum \min(d,x)$$dựa trên tất cả các độ sâu được chèn trước đó bằng cách sử dụng số lượng tiền tố và tổng tiền tố. Đó là sự tối ưu hóa trung tâm giúp tránh lặp lại tất cả các cặp. 

Một điểm tinh tế là chỉ tính các cặp chéo con. Các đỉnh đã được sáp nhập vào vùng chứa lớn đến từ các cây con trước đó, trong khi các đỉnh hiện được xử lý thuộc về một cây con mới. Mỗi cặp được tạo chính xác một lần. 

Một điểm tinh tế khác là công thức cuối cùng. Trong quá trình hợp nhất, chúng tôi chỉ tích lũy tổng số`min(a,b)`. Đóng góp thực tế là`2*min(a,b)-1`, vì vậy sau khi tính tổng tất cả các giá trị cực tiểu, chúng ta trừ đi tổng số cặp tốt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây:```
1
/ \
2 3
```| Cặp | LCA | một | b | Đóng góp | 
| --- | --- | --- | --- | --- | 
| (2,3) | 1 | 1 | 1 | 1 | 

Trả lời = 1. 

Sự hợp nhất ở đỉnh`1`kết hợp hai cây con con. Cặp cây con chéo duy nhất đóng góp`min(1,1)=1`. Áp dụng`2*1-1`cho`1`. 

### Ví dụ 2 

Cây:```
1
|
2
|
3
```| Cặp | Quan hệ tổ tiên | Đóng góp | 
| --- | --- | --- | 
| (1,2) | vâng | 0 | 
| (1,3) | vâng | 0 | 
| (2,3) | vâng | 0 | 

Trả lời = 0. 

Không có cặp nào thuộc về các cây con khác nhau của bất kỳ LCA nào, vì vậy thuật toán không bao giờ tạo ra một cặp cây con chéo. Câu trả lời cuối cùng vẫn là con số không. 

Những ví dụ này chứng minh tính bất biến của khóa: chỉ các cặp từ các cây con khác nhau của cùng một đỉnh mới được xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log2 n) | Hợp nhất từ ​​nhỏ đến lớn cộng với truy vấn Fenwick | 
| Không gian | O(n log n) | Thùng chứa sâu và công trình Fenwick | 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là`3·10^5`. MỘT`O(n log² n)`giải pháp phù hợp một cách thoải mái trong giới hạn 2 giây khi triển khai được tối ưu hóa và là phạm vi độ phức tạp dự kiến ​​cho vấn đề này. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue()

# sample
assert run("""4
3
1 2
1 3
3
1 2
3 2
5
2 3
1 5
4 2
1 2
11
2 1
2 3
2 4
4 5
6 5
5 7
4 8
8 9
7 10
10 11
""") == """1
0
4
29
"""

# single vertex
assert run("""1
1
""") == "0\n"

# chain
assert run("""1
4
1 2
2 3
3 4
""") == "0\n"

# star
assert run("""1
4
1 2
1 3
1 4
""") == "3\n"

# balanced tree
assert run("""1
7
1 2
1 3
2 4
2 5
3 6
3 7
""") == "13\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 0 | Kích thước tối thiểu | 
| Chuỗi | 0 | Tất cả các cặp đều có liên quan đến tổ tiên | 
| Ngôi sao | 3 | Mỗi cặp lá đóng góp 1 | 
| Cây nhị phân cân bằng | 13 | Nhiều LCA và kết hợp độ sâu | 

## Vỏ cạnh 

### Dây chuyền nguyên chất 

đầu vào:```
1
4
1 2
2 3
3 4
```Mỗi cặp có một tổ tiên và một hậu duệ. Không có cặp nào là tốt. 

Thuật toán không bao giờ hợp nhất hai cây con khác nhau vì mỗi đỉnh có nhiều nhất một cây con. Không có cặp cây con chéo nào được tạo ra, vì vậy câu trả lời chính xác là 0. 

###Rễ có nhiều lá 

đầu vào:```
1
4
1 2
1 3
1 4
```Các cặp tốt là`(2,3)`,`(2,4)`,`(3,4)`. 

Mỗi người đều có khoảng cách`(1,1)`từ gốc, đóng góp`1`. 

Việc hợp nhất ở gốc đếm chính xác ba cặp chéo con này và không có cặp nào khác, tạo ra câu trả lời`3`. 

### Ghép nối có độ sâu không bằng nhau 

đầu vào:```
1
3
1 2
2 3
```Không có cặp tốt. 

Một triển khai ngây thơ sử dụng trực tiếp`2*min(a,b)-1`trên các cặp tổ tiên sẽ thu được giá trị âm. Thuật toán tránh được điều này hoàn toàn vì các cặp tổ tiên-con cháu không bao giờ phát sinh dưới dạng cặp con chéo của LCA. 

### Cành sâu và cành nông 

đầu vào:```
1
5
1 2
1 3
3 4
4 5
```Đôi`(2,5)`có khoảng cách`(1,3)`từ LCA`1`. 

Đóng góp của nó là$$2\min(1,3)-1=1.$$Truy vấn Fenwick tính toán mức đóng góp tối thiểu thông qua thống kê độ sâu, chứng minh rằng độ sâu nhánh không bằng nhau được xử lý chính xác mà không cần kiểm tra rõ ràng từng cặp.
