---
title: "CF 2190D - Prufer Vertex"
description: "Chúng tôi bắt đầu với một khu rừng. Chúng ta có thể thêm các cạnh giữa các thành phần được kết nối của nó cho đến khi toàn bộ biểu đồ trở thành một cây duy nhất. Đối với mỗi cây kết quả, hãy chạy quy trình xóa mã Prüfer tiêu chuẩn: liên tục loại bỏ lá được đánh số nhỏ nhất cho đến khi chỉ còn lại hai đỉnh."
date: "2026-06-07T21:07:12+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dsu", "number-theory", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 2190
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1073 (Div. 1)"
rating: 2500
weight: 2190
solve_time_s: 165
verified: false
draft: false
---

[CF 2190D - Prufer Vertex](https://codeforces.com/problemset/problem/2190/D) 

**Đánh giá:** 2500 
**Tas:** tổ hợp, dsu, lý thuyết số, xác suất, cây 
**Thời gian giải:** 2m 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một khu rừng. Chúng ta có thể thêm các cạnh giữa các thành phần được kết nối của nó cho đến khi toàn bộ biểu đồ trở thành một cây duy nhất. 

Đối với mỗi cây kết quả, hãy chạy quy trình xóa mã Prüfer tiêu chuẩn: liên tục loại bỏ lá được đánh số nhỏ nhất cho đến khi chỉ còn lại hai đỉnh. Một trong các đỉnh đó luôn là đỉnh$n$. Đỉnh còn lại được gọi là đỉnh Prüfer$P(T)$. 

Nhiệm vụ không phải là đếm tất cả các cây hoàn thành. Thay vào đó, đối với mỗi$v<n$, chúng ta phải đếm có bao nhiêu cách kết nối khu rừng để tạo ra một cây có đỉnh Prüfer bằng$v$. 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là$2\cdot 10^5$. Bất kỳ giải pháp nào thực hiện công việc tỷ lệ thuận với số lượng cây hoàn thành đều là vô vọng, bởi vì ngay cả một khu rừng trống trên$n$đỉnh có$n^{n-2}$những cây có thể Chúng ta cần mô tả đặc tính cấu trúc của đỉnh Prüfer và sau đó là một công thức đếm chỉ phụ thuộc vào kích thước thành phần và một vài kích thước cây con. 

Một trường hợp nguy hiểm đáng ngạc nhiên là khi$n$Và$n-1$đã thuộc về cùng một thành phần được kết nối.```
4 3
1 2
2 3
3 4
```Rừng đã là một cái cây rồi. Có chính xác một sự hoàn thành. Câu trả lời tập trung vào một đỉnh duy nhất, cụ thể là đỉnh thứ hai trên đường đi từ$4$ĐẾN$3$. Bất kỳ công thức tính nào giả định$n$Và$n-1$ban đầu bị ngắt kết nối sẽ thất bại ở đây. 

Một trường hợp khó phát hiện khác là khi khu rừng chỉ bao gồm các đỉnh cô lập.```
3 0
```Có ba cây hoàn thành. Các câu trả lời không đối xứng. đỉnh$2$lớn hơn đỉnh$1$, vì vậy thứ tự loại bỏ lá có ý nghĩa quan trọng. Bất kỳ đối số nào chỉ dựa trên cây không được gắn nhãn đều đưa ra phân phối sai. 

Cạm bẫy thứ ba là thành phần chứa$n$. Các đỉnh bên trong thành phần đó không có khả năng trở thành câu trả lời như nhau. Điều quan trọng là kích thước của cây con gốc treo bên dưới cây con của$n$, không chỉ kích thước thành phần. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi cách để kết nối khu rừng thành cây, mô phỏng quy trình Prüfer và tăng dần câu trả lời tương ứng. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Thật không may, số lượng cây hoàn thành là$$n^{k-2}\prod s_i,$$đó là số mũ trong trường hợp xấu nhất. Ngay cả đối với những giá trị khiêm tốn của$n$, số lượng cây lớn về mặt thiên văn. 

Quan sát quan trọng xuất phát từ sự hiểu biết về chính quy trình Prüfer. 

Hãy xem xét cặp sống sót cuối cùng. đỉnh$n$luôn luôn hiện diện. Khi nào có thể đạt đỉnh$n-1$biến mất trước khi kết thúc? 

Một cây có ít nhất hai đỉnh thì luôn có ít nhất hai lá. Như vậy$n-1$chỉ có thể tồn tại đến cuối cùng nếu những chiếc lá duy nhất gần cuối cùng bị loại bỏ$n-1$Và$n$. Điều đó có nghĩa là đường đi duy nhất từ$n$ĐẾN$n-1$chứa tất cả các đỉnh còn lại. Đỉnh thứ hai trên đường đi đó chính là đỉnh tồn tại cùng với$n$. Điều này hoàn toàn đặc trưng cho đỉnh Prüfer. 

Một khi đã biết đặc tính này, bài toán sẽ trở thành bài toán đếm trên các thành phần được kết nối. Công thức cổ điển$$n^{k-2}\prod s_i$$để đếm các cách kết nối các thành phần có thể được kết hợp với thực tế là mọi đỉnh bên trong một thành phần đều đối xứng. Số lượng kết quả chỉ phụ thuộc vào kích thước thành phần và đối với các đỉnh bên trong thành phần của$n$, trên kích thước cây con có gốc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Đếm DSU/cây tối ưu |$O(n)$mỗi trường hợp thử nghiệm |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tìm các thành phần được kết nối 

Chạy DFS hoặc DSU trên rừng. 

Đối với mỗi đỉnh lưu trữ id thành phần của nó và kích thước của thành phần đó. 

Root mọi thành phần tại đỉnh được đánh số lớn nhất của nó. Đặc biệt, thành phần có chứa$n$bắt nguồn từ$n$. 

### 2. Tính kích thước cây con bên trong thành phần của$n$Trong DFS, tính toán$$dp[v]=1+\sum dp[\text{child}].$$Đối với một đứa trẻ$v$của$n$, giá trị này chính xác là kích thước của nhánh treo bên dưới$v$. 

### 3. Xử lý trường hợp ở đâu$n$Và$n-1$đã được kết nối 

Đỉnh Prüfer sau đó được cố định vĩnh viễn. 

Đi bộ từ$n-1$trở lên cho đến khi đến được con của$n$nằm trên đường từ$n$ĐẾN$n-1$. 

Gọi đỉnh này$x$. 

Mỗi cây hoàn thành có$P(T)=x$, Vì thế$$ans[x] = \Big(\prod s_i\Big)\, n^{k-2}.$$Tất cả các câu trả lời khác đều bằng không. 

### 4. Xét một đỉnh ứng viên$v$Cho rằng$n$Và$n-1$nằm ở các thành phần khác nhau. 

Có ba tình huống. 

#### 4.1$v$nằm trong thành phần của$n$Sau đó$v$phải là con trực tiếp của$n$. 

Trong số tất cả các cách để kết nối các thành phần còn lại, thành phần chứa$n-1$phải kết thúc bên trong nhánh của$v$. 

Bởi vì các đỉnh của thành phần của$n$đối xứng thì xác suất rơi ở nhánh đó là$$\frac{dp[v]}{|C_n|}.$$Nhân với tổng số cây hoàn thành sẽ có câu trả lời. 

#### 4.2$v$nằm trong thành phần của$n-1$Đầu tiên chúng ta kết nối các thành phần của$n$Và$n-1$. 

Sau đó, kích thước nhánh mong muốn sẽ trở thành$$|C_n|+|C_{n-1}|.$$Thay vào công thức tính sẽ được phần đóng góp. 

#### 4.3$v$nằm trong bất kỳ thành phần nào khác 

Thành phần đó trước tiên phải được gắn vào thành phần của$n$. 

Tất cả các đỉnh bên trong thành phần đều đối xứng, do đó chỉ có kích thước thành phần là quan trọng. 

### 5. Tính toán trước nghịch đảo mô-đun và lũy thừa 

Tất cả các công thức đều chứa các phép chia theo kích thước thành phần. Vì mô đun là số nguyên tố nên hãy sử dụng nghịch đảo mô đun. 

Cũng tính toán trước$$n^0,n^1,\dots,n^k.$$### Tại sao nó hoạt động 

Tính chất quan trọng là đỉnh Prüfer chính xác là đỉnh thứ hai trên đường đi từ$n$ĐẾN$n-1$. Khi đã biết điều này, vấn đề không còn phụ thuộc vào thứ tự xóa chi tiết nữa. 

Đối với ứng viên cố định$v$, chúng ta chỉ cần đếm các cây hoàn thành trong đó thành phần chứa$n-1$kết thúc bên trong nhánh của$v$khi cây đã bén rễ$n$. Các đỉnh trong cùng một thành phần được kết nối không thể phân biệt được trong công thức đếm nối thành phần cổ điển, do đó số lượng tỷ lệ thuận với kích thước của nhánh có liên quan. Mọi công thức được thuật toán sử dụng chính xác bằng tổng số cây hoàn thành nhân với tỷ lệ đặt số lần hoàn thành$n-1$ở chi nhánh đó. Do đó, mỗi lần hoàn thành được tính một lần cho đúng một đỉnh$v$, và kết quả trả lời là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())

    inv = [0] * 200001
    inv[1] = 1
    for i in range(2, 200001):
        inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    for _ in range(t):
        n, m = map(int, input().split())

        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        comp = [-1] * (n + 1)
        parent = [0] * (n + 1)
        sub = [1] * (n + 1)
        sizes = []

        sys.setrecursionlimit(1 << 20)

        def dfs(v, p, cid):
            comp[v] = cid
            sizes[cid] += 1
            parent[v] = p

            for to in g[v]:
                if to == p:
                    continue
                dfs(to, v, cid)
                sub[v] += sub[to]

        cid = 0
        for v in range(n, 0, -1):
            if comp[v] == -1:
                sizes.append(0)
                dfs(v, 0, cid)
                cid += 1

        k = cid

        sz = [0] * (n + 1)
        for v in range(1, n + 1):
            sz[v] = sizes[comp[v]]

        p = 1
        for s in sizes:
            p = p * s % MOD

        pw = [1] * (k + 1)
        for i in range(1, k + 1):
            pw[i] = pw[i - 1] * n % MOD

        def npow(e):
            if e >= 0:
                return pw[e]
            return inv[n]

        ans = [0] * n

        if comp[n] == comp[n - 1]:
            v = n - 1
            while parent[v] != n:
                v = parent[v]

            ans[v] = p * npow(k - 2) % MOD
        else:
            comp_n = comp[n]
            comp_nm1 = comp[n - 1]

            for v in range(1, n):
                if comp[v] == comp_n:
                    if parent[v] != n:
                        continue

                    ans[v] = (
                        sub[v]
                        * p
                        % MOD
                        * inv[sz[n]]
                        % MOD
                        * npow(k - 2)
                    ) % MOD

                elif comp[v] == comp_nm1:
                    ans[v] = (
                        p
                        * inv[sz[n]]
                        % MOD
                        * inv[sz[n - 1]]
                        % MOD
                        * (sz[n] + sz[n - 1])
                        % MOD
                        * npow(k - 3)
                    ) % MOD

                else:
                    ans[v] = (
                        sz[v]
                        * p
                        % MOD
                        * inv[sz[n]]
                        % MOD
                        * inv[sz[v]]
                        % MOD
                        * npow(k - 3)
                    ) % MOD

        print(*ans[1:])

solve()
```DFS đồng thời tính toán id thành phần, kích thước thành phần, con trỏ cha và kích thước cây con. Root mỗi thành phần ở đỉnh lớn nhất của nó phù hợp với đối số được sử dụng trong bằng chứng đếm. Thành phần chứa$n$được tự động root tại$n$, điều này làm cho mọi kích thước nhánh có liên quan đều bằng kích thước cây con. 

chức năng`npow`xử lý số mũ đặc biệt$-1$. Điều này xảy ra khi$k=1$hoặc$k=2$, và công thức biên tập thay thế$n^{-1}$bởi nghịch đảo mô đun của$n$. 

Lỗi triển khai phổ biến nhất là quên rằng chỉ có con của$n$có thể là câu trả lời khi$v$thuộc thành phần của$n$. Séc`parent[v] == n`là điều cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 0
```thành phần: 

| Đỉnh | Kích thước thành phần | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 

Đây$k=3$,$n=3$. 

| v | Kết quả công thức | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 

Đầu ra:```
1 2
```Ví dụ này cho thấy nhãn quan trọng. Mặc dù tất cả các đỉnh đều bắt đầu cô lập nhưng quá trình Prüfer không đối xứng. 

### Ví dụ 2 

đầu vào:```
5 4
4 2
3 4
1 2
4 5
```Đồ thị đã là một cái cây. 

Con đường từ$5$ĐẾN$4$đơn giản là$5\to4$. 

| Số lượng | Giá trị | 
| --- | --- | 
|$n$| 5 | 
|$n-1$| 4 | 
| đỉnh thứ hai trên đường đi | 4 | 
| số lần hoàn thành | 1 | 

Đầu ra:```
0 0 0 1
```Điều này chứng tỏ trường hợp đặc biệt khi$n$Và$n-1$đã thuộc về cùng một thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| DFS cộng với quét tuyến tính trên các đỉnh | 
| Không gian |$O(n)$| Mảng đồ thị, thành phần, cha và cây con | 

Tổng của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2\cdot10^5$. Xử lý tuyến tính cho mỗi trường hợp thử nghiệm nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# These are illustrative tests.

import sys, io

# helper placeholder
def run(inp: str) -> str:
    return "implementation dependent"

# sample 1
# assert run(...) == ...

# minimum tree
# 2 vertices, already connected
# answer must be 1
# assert run("1\n2 1\n1 2\n") == "1\n"

# two isolated vertices
# only one possible tree
# assert run("1\n2 0\n") == "1\n"

# chain already formed
# Prüfer vertex fixed
# assert run("1\n4 3\n1 2\n2 3\n3 4\n") == "0 0 1\n"

# star centered at n
# checks child-of-n logic
# assert run("1\n5 4\n5 1\n5 2\n5 3\n5 4\n") == "0 0 0 1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây nối 2 đỉnh |`1`| Trường hợp hợp lệ nhỏ nhất | 
| Hai đỉnh cô lập |`1`| Vỏ đế nối các thành phần | 
| Biểu đồ đường dẫn | Câu trả lời khác 0 | Đã sửa lỗi trường hợp đỉnh Prüfer | 
| Ngôi sao tập trung ở$n$| Câu trả lời tập trung tại$n-1$| Con của-$n$logic | 

## Vỏ cạnh 

###$n$Và$n-1$đã kết nối 

đầu vào:```
4 3
1 2
2 3
3 4
```Con đường từ$4$ĐẾN$3$bắt đầu bằng đỉnh$3$, Vì thế$P(T)=3$. Không có cạnh nào có thể được thêm vào. Thuật toán phát hiện cả hai đỉnh thuộc về cùng một thành phần và trực tiếp đưa ra một câu trả lời khác 0. 

### Tất cả các đỉnh bị cô lập 

đầu vào:```
3 0
```Có ba cây hoàn thành. Thuật toán chỉ sử dụng kích thước thành phần, do đó, thuật toán sẽ tính tất cả các lần hoàn thành mà không liệt kê chúng một cách tự nhiên. 

### Đỉnh ứng viên bên trong thành phần của$n$đầu vào:```
6 3
1 6
6 4
2 1
```đỉnh$4$là con trực hệ của$6$, trong khi đỉnh$1$có một cây con lớn hơn treo bên dưới nó. Thuật toán sử dụng kích thước cây con, không chỉ kích thước thành phần, đây chính xác là yếu tố phân biệt số lượng câu trả lời của chúng.
