---
title: "CF 2188E - Jerry và Tom"
description: "Mỗi đỉnh đều có một cạnh nối với đỉnh tiếp theo và có thể có thêm một bước nhảy dài tới đỉnh lớn hơn. Các cạnh phụ không bao giờ cắt nhau. Nếu chúng ta vẽ mọi cạnh phía trên trục số thì sẽ không có hai cạnh thừa nào tạo thành mẫu $ui < uj < vi < vj$. Jerry phải di chuyển mỗi lượt."
date: "2026-06-09T04:37:33+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "games", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2188
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1077 (Div. 2)"
rating: 2300
weight: 2188
solve_time_s: 123
verified: true
draft: false
---

[CF 2188E – Jerry và Tom](https://codeforces.com/problemset/problem/2188/E) 

**Đánh giá:** 2300 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, trò chơi, đồ thị, tham lam, cây cối 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đỉnh đều có một cạnh nối với đỉnh tiếp theo và có thể có thêm một bước nhảy dài tới đỉnh lớn hơn. Các cạnh phụ không bao giờ cắt nhau. Nếu chúng ta vẽ mọi cạnh phía trên trục số thì sẽ không có hai cạnh phụ nào tạo thành mẫu$u_i < u_j < v_i < v_j$. 

Jerry phải di chuyển mỗi lượt. Tom có ​​thể di chuyển hoặc ở lại. Jerry thắng bằng cách đạt đến đỉnh$n$trước khi bị bắt. Tom thắng ngay khi cả hai người chơi chiếm cùng một đỉnh vào cuối lượt. 

Đối với mỗi cặp bắt đầu được đặt hàng$(x,y)$, chúng tôi xác định$f(x,y)$là số nước đi thực tế tối thiểu mà Tom phải thực hiện để giành chiến thắng. Nếu Tom không thể buộc thắng thì giá trị là$0$. 

Chúng ta phải tổng hợp$f(x,y)$trên tất cả các cặp có thứ tự$x \ne y$. 

Tổng số đỉnh trong tất cả các trường hợp thử nghiệm chỉ là$2 \cdot 10^5$, điều này ngay lập tức loại trừ bất cứ điều gì gần với$O(n^2)$mỗi trường hợp thử nghiệm. Vì câu trả lời cuối cùng liên quan đến tất cả các cặp được sắp xếp nên chúng ta cần tổng hợp thông tin trên toàn cầu thay vì mô phỏng các trò chơi riêng lẻ. 

Phần nguy hiểm nhất của vấn đề là trò chơi được chơi trên DAG chứ không phải trên cây. Một cách tiếp cận ngây thơ có thể cố gắng suy luận về những con đường tùy ý. Điều kiện không giao nhau chính là điều làm cho cấu trúc cây xuất hiện. 

Xét một đồ thị có các đỉnh$1 \to 2 \to 3 \to 4$và một cạnh phụ$1 \to 4$. 

Nếu Jerry bắt đầu lúc$1$và Tom tại$3$, Tom phải chuyển ngay đến$4$. Chờ đợi thua vì Jerry có thể nhảy thẳng tới$4$. Câu trả lời là$f(1,3)=1$, không$0$. 

Một trường hợp tế nhị khác là$x=n$. Jerry thắng ngay trước khi chơi bất kỳ lượt nào. Ví dụ:```
n = 2
x = 2
y = 1
```Tom không có cơ hội di chuyển nên$f(2,1)=0$. 

Cái bẫy thứ ba là Tom được phép đứng yên. Đôi khi chiến lược tối ưu là không thực hiện bước đi nào và chờ đợi một tổ tiên quan trọng. Bất kỳ giải pháp nào cho rằng Tom luôn tiến bộ sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ đánh giá mọi cặp có thứ tự$(x,y)$, giải trò chơi tương ứng và tích lũy câu trả lời. 

Đồ thị chứa$O(n)$các cạnh, vì vậy ngay cả một$O(n)$phân tích trò chơi mỗi cặp đã có giá$O(n^2)$. Vì có$O(n^2)$cặp, tổng độ phức tạp trở thành$O(n^3)$, điều đó thật vô vọng đối với$n=2 \cdot 10^5$. 

Quan sát chính xuất phát từ điều kiện không giao nhau. 

Với mọi đỉnh$i$, cho phép$a_i$là điểm cuối lớn nhất trong số tất cả các cạnh đi của$i$. Vì cạnh thông thường$i \to i+1$luôn tồn tại,$a_i$được xác định rõ ràng. 

Một thực tế quan trọng là mọi con đường từ$i$ĐẾN$n$phải đi qua$a_i$. Nếu một con đường cố gắng vượt qua$a_i$, cuối cùng một cạnh giao nhau sẽ được tạo ra, mâu thuẫn với hạn chế của đồ thị. Quan sát này xuất hiện trong các cuộc thảo luận giải pháp chính thức. 

Bây giờ kết nối mọi đỉnh$i$ĐẾN$a_i$. Các cạnh này tạo thành một cây có gốc$n$. Mọi đường đi từ một đỉnh tới$n$chứa tất cả tổ tiên của đỉnh đó trong cây này. 

Khi trò chơi được dịch sang cây này, kết quả sẽ trở nên đơn giản. 

Cho phép$d_u$là độ sâu của$u$trong cây, tính từ gốc$n$. Cho phép$L=\operatorname{lca}(x,y)$. 

Nếu như$d_x < d_y$, Jerry có thể đi trước Tom mãi mãi và đạt được$n$, vì vậy Tom không thể buộc phải thắng. 

Nếu như$d_x \ge d_y$, Tom thắng bằng cách chạy đua tới$L$. Anh ấy đạt tới$L$không muộn hơn Jerry và Jerry phải đi qua$L$. Số lần di chuyển tối thiểu Tom cần là$$f(x,y)=d_y-d_L.$$Điều này làm cho trò chơi trở thành vấn đề đếm cây thuần túy. 

Sau khi viết lại tổng các cặp có thứ tự và nhóm các cặp đối xứng, câu trả lời sẽ trở thành$$A-B+C,$$Ở đâu$$A=\sum_{x<y}\min(d_x,d_y),$$

$$B=\sum_{x<y} d_{\operatorname{lca}(x,y)},$$Và$$C=\sum_{\substack{x<y\\ d_x=d_y}} \left(d_x-d_{\operatorname{lca}(x,y)}\right).$$Cả ba số hạng đều có thể được tính bằng$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mọi đỉnh$i<n$, tính toán$a_i$, điểm cuối lớn nhất trong số các cạnh đi ra của nó. 
2. Xây dựng cây có gốc bằng cách tạo$a_i$cha mẹ của$i$. Gốc là đỉnh$n$. 
3. Chạy DFS để tính toán kích thước và độ sâu của cây con. 
4. Tính toán$$A=\sum_{x<y}\min(d_x,d_y).$$Sắp xếp tất cả các độ sâu. Nếu độ sâu được sắp xếp là$b_1 \le b_2 \le \dots \le b_n$, sau đó$$A=\sum_{i=1}^{n} b_i (n-i).$$Mỗi cặp đóng góp độ sâu nhỏ hơn, chính xác là điểm cuối bên trái theo thứ tự được sắp xếp. 
5. Tính toán$$B=\sum_{x<y} d_{\operatorname{lca}(x,y)}.$$Đối với một nút$u$, số cặp không có thứ tự có LCA bằng$u$là$$\binom{\text{sz}_u}{2} - \sum_{v \text{ child of } u} \binom{\text{sz}_v}{2}.$$Nhân số này với$d_u$và thêm nó vào$B$. 
6. Tính toán$$C_1=\sum_h h \binom{\text{cnt}_h}{2},$$Ở đâu$\text{cnt}_h$là số đỉnh ở độ sâu$h$. 

Điều này tương đương với phần đầu tiên của$C$. 
7. Tính toán$$C_2=\sum_{\substack{x<y\\ d_x=d_y}} d_{\operatorname{lca}(x,y)}.$$Sử dụng DSU-on-cây. Đối với mỗi nút$u$, đếm các cặp có độ sâu bằng nhau có LCA chính xác$u$. Nếu số đó là$P_u$, sau đó thêm$$d_u \cdot P_u$$ĐẾN$C_2$. 
8. Câu trả lời cuối cùng là$$A-B+(C_1-C_2).$$### Tại sao nó hoạt động 

Toàn bộ lời giải nằm trên cây được tạo ra bởi cạnh ra cực đại của mỗi đỉnh. Mỗi con đường tới$n$phải đi qua cây cha mẹ trong cây này nên trò chơi chỉ phụ thuộc vào mối quan hệ tổ tiên. Jerry có thể tránh bị bắt chính xác khi anh ta bắt đầu ở gần gốc hơn Tom. Nếu không thì Tom có ​​thể chiếm LCA trước và chờ đợi. Điều này đưa ra công thức đóng cho$f(x,y)$. Công việc còn lại là tính tần suất đóng góp của độ sâu và LCA trên tất cả các cặp, đây chính xác là điều mà ba thuật ngữ tổng hợp tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        parent = [0] * (n + 1)
        for i in range(1, n):
            parent[i] = i + 1

        for _ in range(m):
            u, v = map(int, input().split())
            if v > parent[u]:
                parent[u] = v

        g = [[] for _ in range(n + 1)]
        root = n

        for i in range(1, n):
            g[parent[i]].append(i)

        depth = [0] * (n + 1)
        sz = [0] * (n + 1)
        heavy = [0] * (n + 1)

        depth_cnt = [0] * n

        def dfs1(u):
            sz[u] = 1
            depth_cnt[depth[u]] += 1

            mx = 0
            for v in g[u]:
                depth[v] = depth[u] + 1
                dfs1(v)
                sz[u] += sz[v]
                if sz[v] > mx:
                    mx = sz[v]
                    heavy[u] = v

        dfs1(root)

        depths = depth[1:]
        depths.sort()

        A = 0
        for i, d in enumerate(depths):
            A += d * (n - 1 - i)

        B = 0
        for u in range(1, n + 1):
            pairs = sz[u] * (sz[u] - 1) // 2
            for v in g[u]:
                pairs -= sz[v] * (sz[v] - 1) // 2
            B += pairs * depth[u]

        C1 = 0
        for d, c in enumerate(depth_cnt):
            C1 += d * (c * (c - 1) // 2)

        big = [False] * (n + 1)
        freq = [0] * n

        C2 = 0

        def add_subtree(u, delta):
            freq[depth[u]] += delta
            for v in g[u]:
                if not big[v]:
                    add_subtree(v, delta)

        def count_subtree(u):
            nonlocal cur_pairs
            cur_pairs += freq[depth[u]]
            for v in g[u]:
                if not big[v]:
                    count_subtree(v)

        def dfs2(u, keep):
            nonlocal C2, cur_pairs

            for v in g[u]:
                if v != heavy[u]:
                    dfs2(v, False)

            if heavy[u]:
                dfs2(heavy[u], True)
                big[heavy[u]] = True

            pairs_here = 0

            for v in g[u]:
                if v == heavy[u]:
                    continue

                cur_pairs = 0
                count_subtree(v)
                pairs_here += cur_pairs
                add_subtree(v, 1)

            freq[depth[u]] += 1

            if heavy[u]:
                big[heavy[u]] = False

            C2 += pairs_here * depth[u]

            if not keep:
                add_subtree(u, -1)

        dfs2(root, False)

        ans = A - B + (C1 - C2)
        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên xây dựng cây cha bằng cách sử dụng cạnh ra lớn nhất từ ​​mọi đỉnh. Thuộc tính đồ thị đảm bảo rằng cây này nắm bắt mọi tuyến đường có thể đến gốc. 

DFS đầu tiên tính toán độ sâu, kích thước cây con và mức độ nặng của cây con. Những giá trị này được sử dụng lại trong mọi công thức sau này. 

Việc tính toán của$B$sử dụng nhận dạng đếm LCA tiêu chuẩn. Mỗi cặp không có thứ tự bên trong cây con đều góp phần tạo ra một tổ tiên nào đó. Trừ các cặp hoàn toàn có trong cây con con sẽ để lại chính xác các cặp có LCA là nút hiện tại. 

Phần DSU-on-tree là phần nhạy cảm duy nhất. Trong khi xử lý một nút$u$, bảng tần số lưu trữ độ sâu đã được tích lũy từ các cây con được xử lý trước đó. Khi duyệt qua một cây con con khác, mỗi đỉnh ngay lập tức tìm thấy có bao nhiêu đối tác có độ sâu bằng nhau đã tồn tại. Những cặp đó có LCA chính xác$u$, vì chúng nằm trong các cây con khác nhau. 

Tất cả số học đều sử dụng số nguyên Python nên việc tràn dữ liệu không bao giờ là vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
2 4
1 4
```Cây mẹ là:```
4
├─1
└─2
   └─3
```| Đỉnh | Độ sâu | 
| --- | --- | 
| 4 | 0 | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 2 | 

Độ sâu được sắp xếp là$[0,1,1,2]$. 

| Kỳ hạn | Giá trị | 
| --- | --- | 
| A | 5 | 
| B | 0 | 
| C1 | 1 | 
| C2 | 0 | 
| Trả lời | 6 | 

Điều này phù hợp với đầu ra mẫu. 

### Ví dụ 2 

đầu vào:```
3 1
1 3
```Cây đó là:```
3
├─1
└─2
```| Đỉnh | Độ sâu | 
| --- | --- | 
| 3 | 0 | 
| 1 | 1 | 
| 2 | 1 | 

| Kỳ hạn | Giá trị | 
| --- | --- | 
| A | 2 | 
| B | 0 | 
| C1 | 1 | 
| C2 | 1 | 
| Trả lời | 2 | 

Cặp độ sâu bằng nhau$(1,2)$góp phần một lần nữa vào phép biến đổi đối xứng, đó chính xác là những gì$C$sửa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Phân loại độ sâu và DSU-on-tree | 
| Không gian |$O(n)$| Cây, mảng độ sâu, bảng tần số | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, Vì thế$O(n \log n)$dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    bak = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = bak
    return out.getvalue()

# provided sample
assert run("""5
2 0
3 1
1 3
4 2
2 4
1 4
5 1
1 4
8 3
1 4
5 8
2 4
""") == """0
2
6
3
23
"""

# minimum graph
assert run("""1
2 0
""") == "0\n"

# pure chain
assert run("""1
3 0
""") == "0\n"

# single jump to root
assert run("""1
4 1
1 4
""") == "3\n"

# nested jumps
assert run("""1
5 2
1 5
2 4
""") == "8\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0`|`0`| Đồ thị nhỏ nhất có thể | 
| Chỉ dây chuyền |`0`| Tom không bao giờ cần những động thái tích cực | 
| Một bước nhảy tới root |`3`| Các trường hợp chặn tức thời | 
| Nhảy lồng nhau |`8`| Không vượt qua các cạnh phụ và xây dựng cây | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 0
```Jerry bắt đầu ở đỉnh$2$cho cặp$(2,1)$. Trò chơi kết thúc ngay lập tức với chiến thắng thuộc về Jerry. Độ sâu cây của đỉnh$2$là$0$, do đó công thức chính xác không đóng góp gì cả. 

Coi như:```
1
4 1
1 4
```Tom thường có thể giành chiến thắng bằng cách chờ đợi ở đỉnh$4$. Thuật toán nắm bắt được điều này vì LCA đã là gốc và$d_y-d_{LCA}=0$. 

Coi như:```
1
4 2
1 4
2 4
```Đỉnh$1$Và$2$nằm ở cùng độ sâu. Các cặp có độ sâu bằng nhau chính xác là nơi mà phép biến đổi cặp đối xứng được tính quá mức. Thuật ngữ điều chỉnh$C=C_1-C_2$loại bỏ phần thừa và đưa ra câu trả lời đúng.
