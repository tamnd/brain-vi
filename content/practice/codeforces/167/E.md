---
title: "CF 167E - Pháp Sư và Cược"
description: "Chúng tôi được cung cấp một đồ thị theo chu kỳ có hướng. Một số đỉnh là nguồn, nghĩa là không có cạnh nào đi vào chúng. Một số đỉnh chìm, nghĩa là không có cạnh nào rời khỏi chúng. Số lượng nguồn và số lượng bồn được đảm bảo bằng nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 167
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 114 (Div. 1)"
rating: 2900
weight: 167
solve_time_s: 232
verified: true
draft: false
---

[CF 167E - Pháp sư và Cược](https://codeforces.com/problemset/problem/167/E) 

**Xếp hạng:** 2900 
**Tags:** dfs và tương tự, đồ thị, toán học, ma trận 
**Thời gian giải:** 3 phút 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một đồ thị theo chu kỳ có hướng. Một số đỉnh là nguồn, nghĩa là không có cạnh nào đi vào chúng. Một số đỉnh chìm, nghĩa là không có cạnh nào rời khỏi chúng. Số lượng nguồn và số lượng bồn được đảm bảo bằng nhau. 

Một cấu hình hợp lệ bao gồm các đường dẫn có hướng tách rời nhau sao cho mỗi nguồn được kết nối với chính xác một bồn và mỗi bồn nhận chính xác một đường dẫn. Vì các đường đi là các đỉnh rời nhau nên mỗi đỉnh có thể thuộc nhiều nhất một đường đã chọn. 

Giả sử các bồn được sắp xếp tăng dần theo số đỉnh và các nguồn được sắp xếp theo cùng một cách. Nếu chìm`i`được kết nối với nguồn`a[i]`, thì hoán vị`a`xác định dấu hiệu của cấu hình. Số lượng đảo ngược thậm chí còn đóng góp`+1`, số lượng nghịch đảo lẻ góp phần`-1`. 

Nhiệm vụ là tính tổng các dấu này trên tất cả các tập hợp hợp lệ của các đường đi rời nhau, modulo một số nguyên tố`p`. 

Đây không phải là yêu cầu chúng tôi liệt kê các kết quả phù hợp. Nó yêu cầu tổng xen kẽ trên tất cả các hệ thống đường dẫn từ nguồn tới đích hoàn hảo. 

Đồ thị có nhiều nhất là 600 đỉnh và 100000 cạnh. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận theo cấp số nhân nào đối với các tập hợp con của nguồn hoặc phần chìm. Thậm chí`2^20`sẽ là quá lớn, trong khi ở đây số lượng thiết bị đầu cuối có thể lên tới 600. 

Biểu đồ có tính chất không tuần hoàn, đây là thuộc tính cấu trúc mà toàn bộ giải pháp phụ thuộc vào. Trong DAG, việc đếm đường dẫn có thể được thực hiện bằng lập trình động theo thứ tự tôpô. Giới hạn nhỏ trên`n`mạnh mẽ đề nghị một`O(n^3)`hoặc giải pháp đại số tương tự được dự định. 

Nhiều cạnh được cho phép. Việc triển khai bất cẩn nén các cạnh song song thành một cạnh sẽ đếm thiếu các đường dẫn. Ví dụ:```
2 2
1 2
1 2
```Có hai con đường phân biệt từ`1`ĐẾN`2`, không phải một. 

Các đỉnh có thể đồng thời là nguồn và đích. Ví dụ:```
1 0
```Một đỉnh vừa là nguồn vừa là đỉnh chìm. Có chính xác một hệ thống đường đi hợp lệ, bao gồm đường đi trống từ đỉnh đến chính nó, vì vậy câu trả lời là`1`. 

Một trường hợp tế nhị khác là khi một số nguồn không thể chạm tới bồn chứa nào đó. Việc triển khai xác định ngây thơ mà quên các cặp không thể truy cập trở thành 0 có thể vô tình tính các kết quả khớp không hợp lệ. Ví dụ:```
4 2
1 3
2 3
```Nguồn là`{1,2}`, bồn rửa là`{3,4}`. Bồn rửa`4`không thể truy cập được nên không tồn tại hệ thống đường dẫn hợp lệ. Câu trả lời là`0`. 

Cạm bẫy khó khăn nhất là hiểu tại sao tổng xen kẽ lại xuất hiện. Nếu chúng ta cố gắng đếm trực tiếp các hệ thống đường dẫn không giao nhau, việc loại trừ bao gồm sẽ trở nên lộn xộn. Quan sát quan trọng là các định thức đã mã hóa các dấu hiệu hoán vị và các hệ thống đường dẫn giao nhau sẽ tự động hủy bỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Chúng ta có thể liệt kê mọi đường đi có thể từ mỗi nguồn đến mỗi bồn, sau đó thử mọi cách kết hợp các đường dẫn, kiểm tra xem chúng có tách rời đỉnh hay không, tính toán hoán vị cảm ứng và thêm`+1`hoặc`-1`. 

Điều này có tác dụng vì định nghĩa của câu trả lời theo nghĩa đen là một tổng có dấu trên các hệ thống đường dẫn hợp lệ. 

Vấn đề là số lượng đường dẫn trong DAG có thể theo cấp số nhân`n`. Ngay cả một biểu đồ lớp chỉ có hai lựa chọn gửi đi trên mỗi lớp cũng tạo ra nhiều đường dẫn từ nguồn đến đích theo cấp số nhân. Việc thử tất cả các kết hợp trở nên hoàn toàn không thể. 

Quan sát hữu ích đầu tiên là dấu chỉ phụ thuộc vào hoán vị nguồn tới đích. Đó chính xác là cấu trúc mà các yếu tố quyết định mã hóa. 

Giả sử chúng ta xây dựng một ma trận`A`Ở đâu:```
A[i][j] = number of paths from source i to sink j
```Sau đó:$\det(A)=\sum_{\pi} \operatorname{sgn}(\pi) \prod_i A_{i,\pi(i)}$Mỗi sản phẩm tương ứng với việc chọn một đường dẫn từ mọi nguồn tới bồn được chỉ định. 

Thoạt nhìn điều này có vẻ sai, bởi vì định thức tính tất cả các bộ đường đi, bao gồm cả các đường đi giao nhau. Điều kỳ diệu là bổ đề Lindström-Gessel-Viennot: trong một DAG, tất cả các bộ dữ liệu đường đi giao nhau đều triệt tiêu từng cặp trong khai triển định thức. 

Chỉ có hệ thống đường dẫn đỉnh-tách nhau tồn tại. 

Vì vậy, toàn bộ vấn đề giảm xuống: 

1. Tính toán số lượng đường dẫn giữa mọi nguồn và đích. 
2. Xây dựng ma trận. 
3. Tính định thức modulo của nó`p`. 

Từ`n ≤ 600`, việc loại bỏ Gaussian bậc ba là hoàn toàn khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n^3 + km) | O(n^2) | Đã chấp nhận | 

Đây`k`là số lượng nguồn và đích. 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và tính độ và độ ngoài. 

Nguồn là các đỉnh có bậc bằng 0. Chìm là các đỉnh có bậc ngoài bằng 0. Thứ tự của chúng phải tuân theo chỉ số đỉnh tăng dần chính xác theo yêu cầu của câu lệnh. 
2. Tính toán thứ tự tôpô của DAG. 

Vì đồ thị không có tính tuần hoàn nên thuật toán Kahn tạo ra một thứ tự hợp lệ. Thứ tự này cho phép chúng ta thực hiện đường dẫn DP một cách chính xác. 
3. Đối với mỗi nguồn`s`, tính số đường đi từ`s`tới mọi đỉnh. 

Khởi tạo:```
dp[s] = 1
```Sau đó xử lý các đỉnh theo thứ tự tôpô. Đối với mọi cạnh`u -> v`:```
dp[v] += dp[u]
```modulo`p`. 

Bởi vì đồ thị có tính chất không tuần hoàn nên vào thời điểm chúng tôi xử lý`u`, mọi cách để tiếp cận`u`đã được hoàn thiện. 
4. Điền vào ma trận`A`.`A[i][j]`bằng số đường dẫn từ nguồn`i`chìm`j`. 
5. Tính modulo định thức`p`. 

Sử dụng phép loại trừ Gaussian với nghịch đảo mô-đun. 

Bất cứ khi nào hàng xoay khác với hàng hiện tại, hãy hoán đổi các hàng và nhân định thức với`-1`. 

Từ`p`là số nguyên tố, nghịch đảo môđun tồn tại đối với mọi trục quay khác 0. 
6. Xuất ra modulo định thức`p`. 

### Tại sao nó hoạt động 

Khai triển định thức tính tổng trên tất cả các hoán vị của phép gán từ nguồn đến đích. Đối với một hoán vị cố định, số hạng tích sẽ tính tất cả các bộ đường dẫn thực hiện phép gán đó. 

Các bộ giao nhau xuất hiện hai lần với dấu trái ngược nhau và triệt tiêu lẫn nhau. Sự hủy bỏ này là phát biểu cốt lõi của bổ đề Lindström-Gessel-Viennot. 

Chỉ còn lại các hệ thống đường dẫn đỉnh-tách rời, mỗi hệ thống có trọng số bằng dấu hoán vị của nó. Đó chính xác là đại lượng mà bài toán yêu cầu chúng ta tính toán. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def modinv(a, p):
    return pow(a, p - 2, p)

def determinant(mat, p):
    n = len(mat)
    det = 1

    for col in range(n):
        pivot = -1

        for row in range(col, n):
            if mat[row][col] % p != 0:
                pivot = row
                break

        if pivot == -1:
            return 0

        if pivot != col:
            mat[pivot], mat[col] = mat[col], mat[pivot]
            det = (-det) % p

        pivot_val = mat[col][col] % p
        det = (det * pivot_val) % p

        inv_pivot = modinv(pivot_val, p)

        for row in range(col + 1, n):
            if mat[row][col] == 0:
                continue

            factor = mat[row][col] * inv_pivot % p

            for k in range(col, n):
                mat[row][k] = (mat[row][k] - factor * mat[col][k]) % p

    return det % p

def solve():
    n, m, p = map(int, input().split())

    g = [[] for _ in range(n)]
    indeg = [0] * n
    outdeg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        g[u].append(v)
        indeg[v] += 1
        outdeg[u] += 1

    sources = [i for i in range(n) if indeg[i] == 0]
    sinks = [i for i in range(n) if outdeg[i] == 0]

    k = len(sources)

    q = deque()

    cur_indeg = indeg[:]

    for i in range(n):
        if cur_indeg[i] == 0:
            q.append(i)

    topo = []

    while q:
        u = q.popleft()
        topo.append(u)

        for v in g[u]:
            cur_indeg[v] -= 1
            if cur_indeg[v] == 0:
                q.append(v)

    mat = [[0] * k for _ in range(k)]

    for i, s in enumerate(sources):
        dp = [0] * n
        dp[s] = 1

        for u in topo:
            if dp[u] == 0:
                continue

            val = dp[u]

            for v in g[u]:
                dp[v] += val
                if dp[v] >= p:
                    dp[v] %= p

        for j, t in enumerate(sinks):
            mat[i][j] = dp[t] % p

    ans = determinant(mat, p)
    print(ans % p)

solve()
```Phần đầu tiên xây dựng DAG và xác định nguồn và đích chính xác theo thứ tự đỉnh tăng dần. Câu lệnh xác định việc đánh số một cách ngầm định theo cách này, do đó việc sắp xếp khác nhau sẽ thay đổi quy ước về dấu hiệu và tạo ra các câu trả lời sai. 

Việc sắp xếp tôpô là xử lý Kahn tiêu chuẩn. Chúng tôi sao chép mảng độ vì sau này vẫn cần các giá trị ban đầu để xác định nguồn. 

Đối với mỗi nguồn, chúng tôi chạy DP đếm đường dẫn trên DAG. Từ`n`chỉ 600, chạy một DP cho mỗi nguồn là đủ rẻ. Nhiều cạnh được xử lý một cách tự nhiên vì mỗi cạnh đóng góp riêng biệt trong quá trình truyền. 

Việc triển khai xác định sử dụng phép loại bỏ Gaussian mô-đun. Một sai lầm phổ biến là quên rằng việc hoán đổi hàng sẽ làm thay đổi dấu định thức. Một lỗi phổ biến khác là phép chia bình thường thay vì nhân với nghịch đảo mô-đun. 

Ma trận được sửa đổi tại chỗ trong quá trình loại bỏ. Điều đó không sao cả vì sau này không bao giờ cần đến các giá trị ban đầu nữa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2 1000003
1 3
2 4
```Nguồn là`[1,2]`. Bồn rửa là`[3,4]`. 

Ma trận đường dẫn: 

| Nguồn | Chìm 3 | Chìm 4 | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 0 | 1 | 

Vì thế:$\det\begin{pmatrix}1&0\\0&1\end{pmatrix}=1$Yếu tố quyết định là`1`. 

Dấu vết này thể hiện hệ thống không giao nhau đơn giản nhất có thể. Định thức rút gọn về hoán vị nhận dạng có dấu dương. 

### Mẫu 2 

Hãy xem xét:```
4 2 1000003
1 4
2 3
```Nguồn là`[1,2]`. Bồn rửa là`[3,4]`. 

Ma trận đường dẫn: 

| Nguồn | Chìm 3 | Chìm 4 | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 1 | 0 | 

Vì thế:$\det\begin{pmatrix}0&1\\1&0\end{pmatrix}=-1$Modulo`1000003`, câu trả lời trở thành`1000002`. 

Ví dụ này cho thấy cách định thức tự động mã hóa tính chẵn lẻ hoán vị. Sự phù hợp duy nhất hoán đổi hai thiết bị đầu cuối, tạo ra một dấu hiệu đảo ngược và âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k(n + m) + k^3) | Đường dẫn DP cho mỗi nguồn cộng với tính toán định thức | 
| Không gian | O(k^2 + n + m) | Lưu trữ ma trận, đồ thị và DP | 

Từ`k ≤ n ≤ 600`, bước xác định bậc ba nhiều nhất là khoảng`2.1 × 10^8`các phép toán số học nguyên thủy trong trường hợp lý thuyết tồi tệ nhất, nhưng trên thực tế, Python xử lý kích thước này một cách thoải mái vì công việc bên trong là số học mô-đun đơn giản và nhiều ma trận trở nên thưa thớt trong quá trình loại bỏ. Đường dẫn DP có kích thước tuyến tính trên mỗi nguồn và dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def modinv(a, p):
        return pow(a, p - 2, p)

    def determinant(mat, p):
        n = len(mat)
        det = 1

        for col in range(n):
            pivot = -1

            for row in range(col, n):
                if mat[row][col] % p != 0:
                    pivot = row
                    break

            if pivot == -1:
                return 0

            if pivot != col:
                mat[pivot], mat[col] = mat[col], mat[pivot]
                det = (-det) % p

            pivot_val = mat[col][col] % p
            det = (det * pivot_val) % p

            inv_pivot = modinv(pivot_val, p)

            for row in range(col + 1, n):
                if mat[row][col] == 0:
                    continue

                factor = mat[row][col] * inv_pivot % p

                for k in range(col, n):
                    mat[row][k] = (
                        mat[row][k] - factor * mat[col][k]
                    ) % p

        return det % p

    n, m, p = map(int, input().split())

    g = [[] for _ in range(n)]
    indeg = [0] * n
    outdeg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        g[u].append(v)
        indeg[v] += 1
        outdeg[u] += 1

    sources = [i for i in range(n) if indeg[i] == 0]
    sinks = [i for i in range(n) if outdeg[i] == 0]

    k = len(sources)

    q = deque()

    cur = indeg[:]

    for i in range(n):
        if cur[i] == 0:
            q.append(i)

    topo = []

    while q:
        u = q.popleft()
        topo.append(u)

        for v in g[u]:
            cur[v] -= 1
            if cur[v] == 0:
                q.append(v)

    mat = [[0] * k for _ in range(k)]

    for i, s in enumerate(sources):
        dp = [0] * n
        dp[s] = 1

        for u in topo:
            for v in g[u]:
                dp[v] = (dp[v] + dp[u]) % p

        for j, t in enumerate(sinks):
            mat[i][j] = dp[t]

    return str(determinant(mat, p)) + "\n"

# provided sample
assert run(
"""4 2 1000003
1 3
2 4
"""
) == "1\n", "sample 1"

# single isolated vertex
assert run(
"""1 0 13
"""
) == "1\n", "single source-sink vertex"

# impossible matching
assert run(
"""4 2 17
1 3
2 3
"""
) == "0\n", "unreachable sink"

# odd permutation
assert run(
"""4 2 1000003
1 4
2 3
"""
) == "1000002\n", "negative determinant"

# multiple parallel edges
assert run(
"""2 2 97
1 2
1 2
"""
) == "2\n", "parallel edges counted separately"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh cô lập đơn | 1 | Đường dẫn trống được tính chính xác | 
| Bồn rửa không thể tiếp cận | 0 | Những hệ thống bất khả thi biến mất | 
| Kết hợp chéo | p-1 | Dấu hiệu hoán vị âm | 
| Các cạnh song song | 2 | Nhiều cạnh là những đường dẫn riêng biệt | 

## Vỏ cạnh 

Xét trường hợp đỉnh bị cô lập:```
1 0 13
```Một đỉnh có bậc bằng 0 và góc ngoài bằng 0, vì vậy nó vừa là nguồn vừa là điểm chìm. 

Ma trận đường dẫn là: 

| | v1 | 
| --- | --- | 
| v1 | 1 | 

Đường dẫn trống từ đỉnh đến chính nó có giá trị trong việc đếm đường dẫn DAG. Yếu tố quyết định là`1`, do đó kết quả đầu ra của thuật toán`1`. 

Bây giờ hãy xem xét các bồn rửa không thể truy cập được:```
4 2 17
1 3
2 3
```Nguồn là`[1,2]`, bồn rửa là`[3,4]`. 

Không có con đường nào chạm tới chỗ chìm`4`, do đó ma trận trở thành: 

| | 3 | 4 | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 1 | 0 | 

Định thức của nó bằng không. Trong quá trình loại bỏ, cột trụ thứ hai không có trục xoay khác 0 nên thuật toán ngay lập tức trả về`0`. 

Cuối cùng, hãy xem xét các cạnh song song:```
2 2 97
1 2
1 2
```Có đúng hai đường dẫn có hướng phân biệt từ`1`ĐẾN`2`, một cho mỗi cạnh. 

DP xử lý cả hai cạnh một cách độc lập:```
dp[2] += dp[1]
dp[2] += dp[1]
```cho đi`dp[2] = 2`. 

Yếu tố quyết định của`1 × 1`ma trận`[2]`là`2`, đó là câu trả lời đúng.
