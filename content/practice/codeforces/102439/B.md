---
title: "CF 102439B - Varvara và ma trận"
description: "Chúng ta có một ma trận (n lần m) có các phần tử là số nguyên từ (0) đến (k). Số 0 đánh dấu một ô không xác định và điều kiện đặc biệt là mỗi hàng và mỗi cột chứa nhiều nhất một số 0. Chúng ta phải thay thế mọi số 0 một cách độc lập bằng (A) hoặc (B)."
date: "2026-08-10T06:41:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "B"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 364
verified: true
draft: false
---

[CF 102439B - Varvara và ma trận](https://codeforces.com/problemset/problem/102439/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 4 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ma trận (n \times m) có các phần tử là số nguyên từ (0) đến (k). Số 0 đánh dấu một ô không xác định và điều kiện đặc biệt là mỗi hàng và mỗi cột chứa nhiều nhất một số 0. Chúng ta phải thay thế mọi số 0 một cách độc lập bằng (A) hoặc (B). 

Vẻ đẹp của ma trận đếm các cặp hàng và cặp cột có bốn ô giao nhau đều chứa cùng một giá trị. Chúng ta không được yêu cầu giữ lại tập hợp các hình chữ nhật đẹp thực tế mà chỉ yêu cầu giữ lại tổng số của nó. Hậu quả chính là mọi hình chữ nhật chứa số 0 đều không đẹp trước khi thay thế, bởi vì không thể có bốn góc 0 trong điều kiện một không mỗi hàng và một không mỗi cột. Do đó, thay đổi duy nhất có thể xảy ra là một số hình chữ nhật không đẹp trước đây sẽ trở nên đẹp sau khi các số 0 được lấp đầy. Nhiệm vụ chính xác là ngăn chặn mọi hình chữ nhật mới như vậy. 

Kích thước tối đa là (1000), vì vậy việc quét toàn bộ ma trận hoặc thực hiện một lượng công việc không đổi trên mỗi ô là thực tế. Ngược lại, việc liệt kê tất cả các cặp hàng và tất cả các cặp cột đã cho kết quả 

249.500.250.000 
] 

hình chữ nhật trong trường hợp hình vuông lớn nhất. Điều đó ngay lập tức loại trừ các thuật toán kiểm tra mọi hình chữ nhật. Số lượng số 0 cũng nhiều nhất là (1000), vì các số 0 tạo thành sự khớp giữa các hàng và cột. Số lượng nhỏ các ô chưa biết này là điều khiến cho việc xây dựng ràng buộc Boolean trở nên khả thi. 

Giá trị (k) có thể lớn bằng (nm), nhưng độ lớn của nó không ảnh hưởng đến thuật toán. Chúng tôi chỉ quan tâm liệu một ô cố định có bằng (A), bằng (B) hay không bằng. 

Có một số trường hợp khó khăn mà cách tiếp cận trực tiếp có thể xử lý sai. Đầu tiên, một số 0 có thể bị cấm lấy một trong hai giá trị. Ví dụ,```
2 2
2
1 2
0 1
1 1
```Số 0 không thể trở thành (1), vì điều đó sẽ tạo ra một hình chữ nhật toàn (1). Nó có thể trở thành (2), nên kết quả đúng là`Yes`, ví dụ với```
2 1
1 1
```Một thuật toán bất cẩn coi mọi số 0 là có thể gán tự do có thể vô tình tạo ra hình chữ nhật. 

Một trường hợp tinh vi hơn là khi số 0 bị cấm lấy cả hai giá trị. Coi như```
3 3
2
1 2
1 1 1
1 0 2
1 2 2
```Nếu tâm trở thành (1), hình chữ nhật phía trên bên trái (2\times2) sẽ trở thành tất cả (1). Nếu nó trở thành (2), hình chữ nhật phía dưới bên phải (2\times2) sẽ trở thành tất cả (2). Không có sự thay thế tồn tại, vì vậy câu trả lời là`No`. Hai hạn chế đến từ các hình chữ nhật khác nhau và phải được kết hợp. 

Một trường hợp cạnh khác liên quan đến hai số không. Coi như```
2 2
2
1 2
0 1
1 0
```Hai số 0 là hai góc đối diện và hai góc còn lại là (1). Cả hai không thể được thay thế bằng (1), vì điều đó sẽ tạo ra một hình chữ nhật toàn (1). Tuy nhiên, chúng có thể nhận được các giá trị khác nhau, vì vậy câu trả lời là`Yes`. Chỉ xử lý các ràng buộc liên quan đến một số 0 sẽ bỏ lỡ điều kiện này. 

Đây chính xác là những loại hạn chế được công thức 2-SAT sử dụng bên dưới nắm bắt. Tuyên bố ban đầu và các ràng buộc có sẵn trong kho lưu trữ chính thức của Codeforces Gym. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử tất cả các thay thế (2^z), trong đó (z) là số số 0 và tính toán độ đẹp sau mỗi lần thay thế. Đối với mỗi bài tập, việc liệt kê mọi hình chữ nhật sẽ mất 

[ 
O(n^2m^2) 
] 

thời gian. Trong trường hợp xấu nhất (n=m=1000), có chính xác (249.500.250.000) hình chữ nhật, do đó công việc brute-force gần như là 

[ 
249.500.250.000 \cdot 2^{1000} 
] 

kiểm tra hình chữ nhật. Ngay cả việc kiểm tra từng hình chữ nhật một lần cũng đã vượt quá giới hạn. 

Quan sát tiếp theo là chúng ta thực sự không cần phải tính toán vẻ đẹp. Mọi hình chữ nhật đẹp trước đây đều không chứa số 0 và bốn giá trị của nó vẫn không thay đổi. Do đó các hình chữ nhật đẹp ban đầu sẽ được tự động bảo tồn. Chúng ta chỉ phải cấm các hình chữ nhật trở nên đẹp sau khi thay thế các góc bằng 0 của chúng. 

Vì mỗi hàng và cột có nhiều nhất một số 0 nên hình chữ nhật chứa nhiều nhất hai số 0. Nếu nó chứa đúng một số 0 thì ba góc còn lại sẽ cố định. Số 0 không được nhận giá trị chung của chúng. Nếu nó chứa hai số 0 thì chúng phải chiếm các góc đối diện và hai góc còn lại cố định. Nếu hai góc cố định đó đều bằng (A) thì hai số 0 không thể cùng nhận (A). Điều tương tự cũng áp dụng cho (B). 

Điều này chỉ đưa ra hai loại hạn chế logic. Một số 0 có thể bị cấm lấy (A) hoặc (B), hoặc hai số 0 có thể bị cấm lấy cùng một lúc (A) hoặc cùng một lúc (B). Cả hai đều là mệnh đề liên quan đến nhiều nhất hai biến Boolean, do đó toàn bộ bài toán trở thành 2-SAT. 

Khó khăn còn lại là tìm ra tất cả các hạn chế một không một cách nhanh chóng. Đối với số 0 tại ((x,y)), giả sử chúng ta muốn biết liệu việc gán nó (A) có tạo ra một hình chữ nhật toàn (A) hay không. Chúng ta cần một hàng khác (r) và một cột khác (c) thỏa mãn 

[ 
a_{x,c}=A,\qquad 
a_{r,c}=A,\qquad 
a_{r,y}=A. 
] 

Đối với mỗi hàng (x), hãy tạo một tập hợp bit chứa tất cả các hàng (r) có (A) ở đâu đó trong cột trong đó hàng (x) cũng có (A). Sau đó giao nó với tập bit của các hàng có (A) trong cột (y). Giao điểm không trống có nghĩa là hình chữ nhật được yêu cầu tồn tại. 

Việc xây dựng tương tự được thực hiện cho (B). Đây là tối ưu hóa bitet được sử dụng trong các giải pháp tiêu chuẩn cho vấn đề này. 

Đối với hình chữ nhật có hai số 0, có nhiều nhất (1000) số 0, do đó, việc kiểm tra trực tiếp tất cả các cặp chỉ tốn chi phí (O(z^2)). Sau đó, chúng tôi giải quyết trường hợp 2-SAT thu được bằng các thành phần được kết nối mạnh mẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^z n^2m^2)) | (O(nm)) | Quá chậm | 
| Bảng liệt kê hình chữ nhật | (O(n^2m^2)) | (O(nm)) | Quá chậm | 
| Tối ưu | (O(nm\lceil n/w\rceil + z^2 + E)) | (O(nm + E)) | Đã chấp nhận | 

Ở đây (z\le\min(n,m)), (w) là kích thước từ máy được sử dụng bởi biểu diễn bitset và (E=O(zn+z^2)) là số cạnh hàm ý. Đối với (n,m\le1000), các thao tác bitset đủ nhỏ để phù hợp thoải mái với cách tiếp cận dự định. 

## Hướng dẫn thuật toán 

1. Đọc ma trận và ghi lại mọi số 0 dưới dạng biến Boolean. Đối với biến (i), hãy`A_i`có nghĩa là số 0 của nó được thay thế bằng (A) và đặt`B_i`có nghĩa là nó được thay thế bởi (B). Vì mỗi số 0 phải nhận chính xác một giá trị,`A_i`Và`B_i`là những chữ bổ sung. 
2. Xây dựng các tập hợp bit cho các vị trí chứa (A) và (B). Đối với mỗi hàng, lưu trữ các cột chứa (A) và riêng các cột chứa (B). Đối với mỗi cột, lưu trữ các hàng chứa (A) và riêng các hàng chứa (B). 
3. Với mỗi hàng (x), hãy xây dựng`coverA[x]`. Các bit được thiết lập của nó chính xác là các hàng (r) tồn tại một số cột (c) có cả (a_{x,c}=A) và (a_{r,c}=A). xây dựng`coverB`theo cùng một cách. 

Điều này biến việc tìm kiếm hình chữ nhật một số không thành một giao điểm bitset thay vì quét mọi cặp hàng và cột có thể có. 
4. Với mọi số 0 tại ((x,y)), kiểm tra xem`colA[y] & coverA[x]`không trống. Nếu đúng thì có một hàng (r) và một cột (c) sao cho ba góc còn lại của hình chữ nhật đều là (A). Do đó, số 0 này không thể được gán cho (A), do đó hãy thêm hàm ý 

[ 
A_i \rightarrow B_i. 
] 

Thực hiện kiểm tra tương tự cho (B). Nếu thành công thì thêm 

[ 
B_i \rightarrow A_i. 
] 
5. Xét mọi cặp số 0 phân biệt (i=(x,y)) và (j=(r,c)). Vì mỗi hàng và cột chứa nhiều nhất một số 0 nên các hàng và cột của chúng sẽ tự động khác nhau. Hình chữ nhật duy nhất chứa cả hai số 0 có chúng ở các góc đối diện. 

Nếu (a_{x,c}=A) và (a_{r,y}=A), việc gán (A) cho cả hai số 0 sẽ tạo ra một hình chữ nhật mới đẹp. Như vậy mệnh đề là 

[ 
\neg(A_i\land A_j), 
] 

điều này trở thành hai hàm ý 

[ 
A_i\rightarrow B_j, 
\qquad 
A_j\rightarrow B_i. 
] 

Nếu hai góc cố định đều là (B) thì thêm giới hạn đối xứng 

[ 
B_i\rightarrow A_j, 
\qquad 
B_j\rightarrow A_i. 
] 
6. Biểu đồ hàm ý bây giờ thể hiện mọi phép gán sẽ tạo ra một hình chữ nhật đẹp mới. Chạy một thuật toán thành phần được kết nối mạnh mẽ trên đó. Không thể thực hiện chính xác phiên bản 2-SAT khi một biến và phần bù của nó thuộc về cùng một thành phần được kết nối mạnh. 
7. Nếu số 0 nào đó có`A_i`Và`B_i`trong cùng một thành phần, in`No`. Nếu không, hãy chọn một giá trị theo thứ tự thành phần, thay thế mọi số 0 và in ma trận kết quả. 

### Tại sao nó hoạt động 

Mọi hình chữ nhật đẹp trước khi thay thế đều không chứa số 0 nên bốn góc của nó không bao giờ thay đổi. Như vậy vẻ đẹp chỉ có thể giảm đi nếu hình chữ nhật cũ thay đổi, điều này không thể xảy ra và nó chỉ có thể tăng lên khi hình chữ nhật chứa một hoặc hai số 0 trở thành đơn sắc. 

Một hình chữ nhật chứa nhiều nhất hai số 0 vì không có hàng hoặc cột nào chứa hai số 0. Với một số 0 thì ba góc còn lại được cố định nên hạn chế duy nhất là số 0 không thể lấy giá trị chung của chúng. Với hai số 0, chúng là các góc đối diện nhau và hai góc còn lại được cố định, do đó, hạn chế duy nhất xảy ra khi các góc cố định đó trùng với (A) hoặc (B), cấm cả hai số 0 lấy giá trị đó. 

Thuật toán tạo ra chính xác những hạn chế này và không có hạn chế nào khác. Mỗi hàm ý trong biểu đồ thể hiện một điều kiện cần thiết để tránh một hình chữ nhật đẹp mới và mọi hình chữ nhật đẹp mới có thể tạo ra một trong những hàm ý này. Do đó, phép gán thỏa mãn của phiên bản 2-SAT tương ứng chính xác với sự thay thế hợp lệ của tất cả các số 0. Bài kiểm tra SCC là tiêu chí đáp ứng tiêu chuẩn cho 2-SAT. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline
sys.setrecursionlimit(1_000_000)

def solve():
    n, m = map(int, input().split())
    k = int(input())
    A, B = map(int, input().split())

    matrix = []
    row_a = [0] * n
    row_b = [0] * n
    col_a = [0] * m
    col_b = [0] * m
    zeros = []

    for i in range(n):
        row = list(map(int, input().split()))
        matrix.append(array('I', row))

        ma = 0
        mb = 0

        for j, value in enumerate(row):
            if value == 0:
                zeros.append((i, j))
            elif value == A:
                ma |= 1 << j
                col_a[j] |= 1 << i
            elif value == B:
                mb |= 1 << j
                col_b[j] |= 1 << i

        row_a[i] = ma
        row_b[i] = mb

    z = len(zeros)

    if z == 0:
        out = ["Yes"]
        out.extend(" ".join(map(str, row)) for row in matrix)
        sys.stdout.write("\n".join(out))
        return

    # cover_a[x] contains every row r that shares an A-column
    # with row x. cover_b is analogous.
    cover_a = [0] * n
    cover_b = [0] * n

    for i in range(n):
        bits = row_a[i]
        cur = 0
        while bits:
            low = bits & -bits
            c = low.bit_length() - 1
            cur |= col_a[c]
            bits -= low
        cover_a[i] = cur

        bits = row_b[i]
        cur = 0
        while bits:
            low = bits & -bits
            c = low.bit_length() - 1
            cur |= col_b[c]
            bits -= low
        cover_b[i] = cur

    nodes = 2 * z

    # Forward-star representation of the implication graph.
    head = [-1] * nodes
    to = array('i')
    nxt = array('i')

    def add_edge(u, v):
        e = len(to)
        to.append(v)
        nxt.append(head[u])
        head[u] = e

    # Literal encoding:
    # 2*i     = zero i is assigned A
    # 2*i + 1 = zero i is assigned B
    #
    # The complement of a literal is literal ^ 1.

    # Restrictions involving exactly one zero.
    for i, (x, y) in enumerate(zeros):
        a_lit = 2 * i
        b_lit = a_lit + 1

        if col_a[y] & cover_a[x]:
            # A_i is forbidden: A_i -> B_i
            add_edge(a_lit, b_lit)

        if col_b[y] & cover_b[x]:
            # B_i is forbidden: B_i -> A_i
            add_edge(b_lit, a_lit)

    # Restrictions involving two zeros.
    for i in range(z):
        x, y = zeros[i]
        ai = 2 * i
        bi = ai + 1

        for j in range(i + 1, z):
            r, c = zeros[j]
            aj = 2 * j
            bj = aj + 1

            if matrix[x][c] == A and matrix[r][y] == A:
                # Not (A_i and A_j).
                add_edge(ai, bj)
                add_edge(aj, bi)

            if matrix[x][c] == B and matrix[r][y] == B:
                # Not (B_i and B_j).
                add_edge(bi, aj)
                add_edge(bj, ai)

    # Tarjan SCC.
    index = [-1] * nodes
    low = [0] * nodes
    on_stack = [False] * nodes
    stack = []
    component = [-1] * nodes
    timer = 0
    comp_id = 0

    def dfs(v):
        nonlocal timer, comp_id

        index[v] = timer
        low[v] = timer
        timer += 1

        stack.append(v)
        on_stack[v] = True

        e = head[v]
        while e != -1:
            w = to[e]

            if index[w] == -1:
                dfs(w)
                if low[w] < low[v]:
                    low[v] = low[w]
            elif on_stack[w] and index[w] < low[v]:
                low[v] = index[w]

            e = nxt[e]

        if low[v] == index[v]:
            while True:
                w = stack.pop()
                on_stack[w] = False
                component[w] = comp_id
                if w == v:
                    break
            comp_id += 1

    for v in range(nodes):
        if index[v] == -1:
            dfs(v)

    # A variable and its complement in the same SCC means
    # that the 2-SAT instance is unsatisfiable.
    for i in range(z):
        if component[2 * i] == component[2 * i + 1]:
            sys.stdout.write("No\n")
            return

    # Tarjan numbers SCCs in reverse topological order of the
    # condensation graph, so the larger component id is chosen.
    for i, (x, y) in enumerate(zeros):
        if component[2 * i] > component[2 * i + 1]:
            matrix[x][y] = A
        else:
            matrix[x][y] = B

    out = ["Yes"]
    out.extend(" ".join(map(str, row)) for row in matrix)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào lưu trữ ma trận trong các mảng số nguyên nhỏ gọn thay vì các danh sách số nguyên Python lồng nhau thông thường. Điều này quan trọng vì danh sách số nguyên Python (1000\times1000) có chi phí hoạt động cao hơn đáng kể so với (4) MB thực tế của các giá trị 32 bit mà ma trận cần.`row_a`Và`row_b`là các tập hợp số nguyên có bit (j) cho biết cột (j) có chứa giá trị tương ứng trong hàng đó hay không.`col_a`Và`col_b`sử dụng ý tưởng tương tự theo hướng khác. Số nguyên Python làm cho các bitset này đặc biệt thuận tiện vì`&`,`|`và dịch chuyển bit hoạt động trên nhiều bit cùng một lúc trong mã gốc. 

các`cover_a`xây dựng đáng được quan tâm. Giả sử hàng (x) chứa (A) trong các cột (c_1,c_2,\ldots). Đối với mỗi cột như vậy,`col_a[c]`chứa mọi hàng có (A) trong cột đó. Lấy liên kết của chúng sẽ cho mỗi hàng có chung ít nhất một cột (A) với hàng (x). Giao điểm này với`col_a[y]`sau đó hỏi liệu một hàng như vậy cũng có (A) tại cột (y), khớp chính xác với ba góc cố định được yêu cầu bởi một hình chữ nhật xung quanh số 0 ((x,y)). 

Biểu đồ hàm ý sử dụng hai nút trên mỗi số không. nút`2*i`đại diện cho việc chọn (A) và nút`2*i+1`đại diện cho việc lựa chọn (B). Giá trị bị cấm trở thành hàm ý một chiều đối với giá trị ngược lại. Lệnh cấm hai số không trở thành cặp hàm ý tiêu chuẩn cho mệnh đề hai nghĩa đen. 

Vòng lặp cặp sử dụng`i + 1`chứ không phải tất cả các cặp có thứ tự. Điều kiện là đối xứng, do đó việc kiểm tra cả hai thứ tự sẽ chỉ lặp lại các mệnh đề giống nhau. Truy cập ma trận`matrix[x][c]`Và`matrix[r][y]`là an toàn vì các số 0 nằm ở các hàng khác nhau và các cột khác nhau. 

Thuật toán của Tarjan tránh lưu trữ biểu đồ ngược thứ hai. Với tối đa khoảng (4) triệu cạnh hàm ý trong trường hợp xấu nhất về mặt lý thuyết, đây là một cách tối ưu hóa bộ nhớ hữu ích trong Python. Độ sâu đệ quy được giới hạn bởi các đỉnh đồ thị (2z\le2000) và giới hạn đệ quy được nâng lên cao hơn giới hạn đó. 

Nhiệm vụ cuối cùng sử dụng thứ tự thành phần SCC. Nếu hai chữ của một biến có các thành phần khác nhau, thì có thể chọn chính xác một biến theo thứ tự tôpô 2-SAT thông thường. Không thể tràn số nguyên vì tất cả các chỉ số biểu đồ và giá trị ma trận tối đa là đa thức trong các chiều đầu vào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào có ba số không: 

[ 
z_0=(1,3),\qquad z_1=(2,1),\qquad z_2=(4,4), 
] 

sử dụng tọa độ một cơ sở. Ở đây (A=3) và (B=5). 

| Bước | Không | Hạn chế số 0 | Hạn chế cặp | Kết quả lựa chọn | 
| --- | --- | --- | --- | --- | 
| 1 | (z_0=(1,3)) | Không có | (z_0,z_2) không thể vừa là (3) | (5) | 
| 2 | (z_1=(2,1)) | Không có | Không hạn chế với các số 0 khác | (5) | 
| 3 | (z_2=(4,4)) | Không có | (z_0,z_2) không thể vừa là (3) | (3) | 
| 4 | Tất cả các biến | SCC nhất quán | Không có biến nào xung đột với phần bù của nó |`Yes`| 

Hình chữ nhật mới có liên quan duy nhất được hình thành bởi số 0 thứ nhất và thứ tư cùng với hai (3) cố định. Do đó, hai số 0 đó không thể cùng trở thành (3). Phép gán (z_0=5,z_1=5,z_2=3) thỏa mãn giới hạn và khớp với cấu trúc hợp lệ của mẫu. 

### Mẫu 2 

Ở đây (A=1) và (B=2), với các số 0 tại ((2,2)) và ((3,3)). 

| Bước | Không | (A) hạn chế | (B) hạn chế | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | ((2,2)) | Bị cấm bởi hình chữ nhật toàn (1) | Được phép | Phải là (2) | 
| 2 | ((3,3)) | Bị cấm bởi các ràng buộc tương ứng | Bị hạn chế bởi một hình chữ nhật khác | Xung đột cưỡng bức | 
| 3 | Cả hai biến | Ý nghĩa lan truyền | (A_i) và (B_i) chạm tới nhau | Cùng SCC | 
| 4 | Công thức | Biến bằng phần bù của nó | Không hài lòng |`No`| 

Số 0 đầu tiên đã nhận được một hạn chế từ một hình chữ nhật có ba góc còn lại là (1). Số 0 thứ hai đóng góp những hàm ý bổ sung và biểu đồ hàm ý thu được sẽ đặt cả hai chữ có thể có của một biến vào cùng một thành phần được kết nối mạnh mẽ. Ví dụ 2-SAT không đạt yêu cầu nên không có sự thay thế nào có thể giữ được vẻ đẹp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm\lceil n/w\rceil + z^2 + E)) | Cấu trúc bitset cộng với tất cả các cặp 0 và cạnh SCC | 
| Không gian | (O(nm + E)) | Ma trận, bitset và biểu đồ hàm ý | 
| Số lượng biến | (z\le\min(n,m)) | Tối đa một số 0 trên mỗi hàng và cột | 
| Số cạnh đồ thị | (E=O(zn+z^2)) | Hạn chế một không và hai không | 

Đối với (n,m\le1000), có tối đa (1000) biến Boolean. Công việc bitset hoạt động trên tối đa (1000) bit tại một thời điểm, trong khi bảng liệt kê cặp chứa tối đa khoảng (500.000) cặp không. Đồ thị SCC chỉ có (2000) đỉnh. Điều này về cơ bản khác với việc liệt kê các hình chữ nhật có thể có (2,5\times10^{11}) trong ma trận (1000\times1000). 

Công thức C++ tiêu chuẩn mô tả phần bitset là (O(n^3/w)) cho các kích thước có kích thước hình vuông, tương tự như ý tưởng song song từ được sử dụng ở đây. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây chạy tương tự`solve()`chức năng, xác nhận`Yes`câu trả lời bằng cách tính toán lại vẻ đẹp cho ma trận nhỏ, kiểm tra`No`trả lời trực tiếp và bao gồm trường hợp ranh giới (1000\times1000) mà không cần thực hiện tính toán đẹp (O(n^2m^2)).```python
import sys
import io
from array import array

# Paste the solve() implementation from the solution above here.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def parse_input(inp: str):
    lines = inp.strip().splitlines()
    n, m = map(int, lines[0].split())
    k = int(lines[1])
    A, B = map(int, lines[2].split())
    mat = [list(map(int, lines[3 + i].split())) for i in range(n)]
    return n, m, A, B, mat

def beauty(mat):
    n = len(mat)
    m = len(mat[0])
    ans = 0

    for r1 in range(n):
        for r2 in range(r1 + 1, n):
            for c1 in range(m):
                x = mat[r1][c1]
                for c2 in range(c1 + 1, m):
                    if (
                        x != 0
                        and x == mat[r1][c2]
                        and x == mat[r2][c1]
                        and x == mat[r2][c2]
                    ):
                        ans += 1

    return ans

def validate_yes(inp, out):
    n, m, A, B, original = parse_input(inp)
    lines = out.strip().splitlines()

    assert lines[0] == "Yes"
    assert len(lines) == n + 1

    result = [list(map(int, lines[i + 1].split())) for i in range(n)]

    assert all(len(row) == m for row in result)

    for i in range(n):
        for j in range(m):
            if original[i][j] == 0:
                assert result[i][j] in (A, B)
            else:
                assert result[i][j] == original[i][j]

    assert beauty(original) == beauty(result)

def validate_no(inp, out):
    assert out.strip() == "No"

# Provided sample 1.
sample1 = """\
4 4
5
3 5
1 1 0 3
0 5 4 5
1 1 4 4
2 5 3 0
"""

out = run(sample1)
validate_yes(sample1, out)

# Provided sample 2.
sample2 = """\
4 4
4
1 2
1 1 3 3
1 0 2 3
1 2 0 3
1 3 1 3
"""

out = run(sample2)
validate_no(sample2, out)

# Custom 1: minimum-size matrix, all equal, no zeros.
case1 = """\
2 2
2
1 2
1 1
1 1
"""

out = run(case1)
validate_yes(case1, out)

# Custom 2: two opposite zeros. They cannot both become A,
# but assigning different values is valid.
case2 = """\
2 2
2
1 2
0 1
1 0
"""

out = run(case2)
validate_yes(case2, out)

# Custom 3: one zero is forbidden from both A and B.
case3 = """\
3 3
2
1 2
1 1 1
1 0 2
1 2 2
"""

out = run(case3)
validate_no(case3, out)

# Custom 4: maximum-size boundary case.
# There are no zeros, so the matrix must simply remain unchanged.
n = 1000
row = "7 " * 999 + "7"
case4 = f"{n} {n}\n1000\n1 2\n" + "\n".join([row] * n) + "\n"

out = run(case4)
lines = out.strip().splitlines()

assert lines[0] == "Yes"
assert len(lines) == n + 1
assert lines[1] == row
assert lines[-1] == row
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 x 2`, tất cả các mục bằng nhau |`Yes`| Kích thước tối thiểu và trường hợp không có số 0 | 
|`2 x 2`với số 0 đối diện |`Yes`| Mệnh đề hai số không | 
|`3 x 3`với một số không |`No`| Một biến duy nhất bị cấm ở cả hai giá trị | 
|`1000 x 1000`, không có số không |`Yes`| Kích thước ma trận tối đa và mức sử dụng bộ nhớ biên | 
| Cung cấp mẫu 1 |`Yes`| Ví dụ thỏa mãn bình thường | 
| Cung cấp mẫu 2 |`No`| SCC mâu thuẫn | 

## Vỏ cạnh 

Trường hợp một không```
2 2
2
1 2
0 1
1 1
```có số 0 tại ((1,1)). Hàng của nó chứa (1) trong cột (2) và cột của nó chứa (1) trong hàng (2). Góc còn lại cũng là (1) nên việc gán số 0 cho (1) sẽ tạo ra một hình chữ nhật mới đẹp. Thuật toán phát hiện`col_a[y] & cover_a[x]`không trống và cộng (A_i\rightarrow B_i). Không có giới hạn tương ứng cho (B), nên người giải SCC chọn (B=2). Đầu ra có thể là```
Yes
2 1
1 1
```Trường hợp hai số không```
2 2
2
1 2
0 1
1 0
```có số không ở các góc đối diện. Nếu cả hai đều trở thành (1) thì cả bốn góc sẽ là (1), do đó thuật toán cộng thêm 

[ 
A_0\mũi tên phải B_1 
] 

và 

[ 
A_1\mũi tên phải B_0. 
] 

Phép gán (A_0=B), (A_1=A) thỏa mãn cả hai hàm ý, đưa ra```
Yes
2 1
1 2
```Số 0 cũng có thể bị cấm đồng thời đối với cả hai giá trị:```
3 3
2
1 2
1 1 1
1 0 2
1 2 2
```Đối với số 0 ở giữa, hình chữ nhật phía trên bên trái có ba (1) cố định, do đó tâm không thể trở thành (1). Hình chữ nhật phía dưới bên phải có ba (2) cố định nên không thể trở thành (2). Do đó, biểu đồ chứa cả hai hàm ý (A_0\rightarrow B_0) và (B_0\rightarrow A_0), đặt hai chữ số trong cùng một SCC. Bộ giải in`No`. 

Nếu không có số 0 thì không có biến Boolean và không có hàm ý. Mỗi hình chữ nhật đều giữ nguyên bốn góc giống nhau nên vẻ đẹp tự động không thay đổi. Thuật toán in ngay lập tức`Yes`và ma trận ban đầu. 

Nếu (A) hoặc (B) không xuất hiện ở bất kỳ đâu trong ma trận ban đầu thì các bit tương ứng sẽ vẫn trống. Số 0 vẫn có thể được gán giá trị đó và không thể có hình chữ nhật mới có các góc cố định sử dụng giá trị vắng mặt đó. Giao điểm bitset đương nhiên không tạo ra hạn chế nào cho giá trị đó. 

Điều kiện một không mỗi hàng và một không mỗi cột cũng rất cần thiết. Nó đảm bảo rằng một hình chữ nhật chứa nhiều nhất hai số 0. Nếu không có thuộc tính đó, một hình chữ nhật có thể chứa ba hoặc bốn số không và hai loại mệnh đề được giải pháp này sử dụng sẽ không còn bao gồm mọi hình chữ nhật đẹp mới có thể có nữa.
