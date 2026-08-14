---
title: "CF 102348I - Đài phát thanh"
description: "Chúng tôi có (p) đài phát thanh. Chọn trạm (i) có nghĩa là ký hợp đồng với trạm đó và điều này chỉ có thể thực hiện được khi công suất tín hiệu đã chọn (f) nằm trong khoảng của nó ([li,ri]). Đối với (f cố định), một trạm nằm ngoài khoảng của nó buộc phải không được chọn."
date: "2026-08-13T01:06:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "I"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 362
verified: true
draft: false
---

[CF 102348I - Đài phát thanh](https://codeforces.com/problemset/problem/102348/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (p) đài phát thanh. Chọn trạm (i) có nghĩa là ký hợp đồng với trạm đó và điều này chỉ có thể thực hiện được khi công suất tín hiệu đã chọn (f) nằm trong khoảng của nó ([l_i,r_i]). Đối với (f cố định), một trạm nằm ngoài khoảng của nó buộc phải không được chọn. 

Mỗi khiếu nại đưa ra một cặp ((x_i,y_i)) và yêu cầu ít nhất một trong hai trạm đó phải được chọn. Mỗi cặp nhiễu ((u_i,v_i)) yêu cầu tối đa một trong hai trạm đó được chọn. Nhiệm vụ là chọn cả (f) và một tập hợp các trạm thỏa mãn tất cả các điều kiện này hoặc báo cáo rằng không có lựa chọn nào như vậy tồn tại. Các ràng buộc chính thức cho phép tất cả bốn tham số chính đạt tới (4\cdot10^5), với giới hạn 7 giây và 256 MB bộ nhớ. 

Chỉ riêng phần chọn trạm thì đây đã là phiên bản 2-SAT. Gọi (S_i) có nghĩa là trạm (i) đã được chọn. Khiếu nại trở thành (S_x\lor S_y), trong khi cặp nhiễu trở thành (\lkhông phải S_u\lor\lkhông phải S_v). Khó khăn là (f) không được biết trước và việc chọn (f) có thể buộc nhiều trạm sai. 

Việc triển khai trực tiếp có thể thử mọi (f), xây dựng phiên bản 2-SAT tương ứng và chạy SCC mỗi lần. Chi phí của một lần kiểm tra (O(p+n+m)), do đó, tất cả (M) lựa chọn đều có chi phí (O(M(p+n+m))). Ở mức giới hạn tối đa, đây là khoảng (4\cdot10^5\cdot1.2\cdot10^6=4.8\cdot10^{11}) hoạt động ở quy mô đầu vào, vượt xa giới hạn thời gian cho phép. 

Có một số trường hợp ranh giới có thể dễ dàng phá vỡ quá trình triển khai. Một trạm có khoảng ([l,r]) có thể sử dụng được ở cả hai điểm cuối, do đó việc thay thế các điều kiện bằng (l<f<r) sẽ âm thầm làm mất đi các câu trả lời hợp lệ. Ví dụ,```
2 3 2 2
1 2
2 3
1 1
1 2
2 2
1 2
2 3
```có câu trả lời hợp lệ (f=2) chỉ với trạm 2 được chọn. Trạm 2 có thể sử dụng được chính xác ở điểm cuối phía trên của nó và việc chọn trạm này sẽ đáp ứng được cả hai khiếu nại. 

Một lỗi khác xảy ra khi các khiếu nại khác nhau chỉ có thể được gửi đến các cơ quan có thẩm quyền khác nhau. Ví dụ,```
2 4 2 2
1 2
3 4
1 1
1 1
2 2
2 2
1 2
3 4
```có câu trả lời`-1`. Tại (f=1) chỉ chọn được trạm 1 và 2 nên khiếu nại thứ 2 không được giải quyết. Tại (f=2), chỉ có trạm 3 và 4 là sử dụng được nên khiếu nại đầu tiên không thể được giải quyết. Việc chỉ kiểm tra xem công thức chọn trạm có thỏa mãn mà không biểu thị (f) hay không sẽ chấp nhận trường hợp này một cách sai lầm. 

Trường hợp tinh tế thứ ba là bản thân công suất tín hiệu phải được biểu thị bằng một số nguyên hợp lệ từ 1 đến (M). Nếu cấu trúc cho phép giá trị nhân tạo 0 hoặc (M+1), nó có thể tạo ra phép gán Boolean không có công suất tín hiệu tương ứng. Việc xây dựng ngưỡng bên dưới ngăn chặn điều đó một cách rõ ràng. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Cố định giá trị (f), đánh dấu mọi trạm có khoảng không chứa (f) là bắt buộc không được chọn, thêm các điều khoản khiếu nại và can thiệp, đồng thời giải quyết trường hợp 2-SAT thu được bằng cách sử dụng các thành phần được kết nối mạnh. Nếu công thức thỏa mãn, việc gán SCC sẽ đưa ra các trạm đã chọn. Việc thử tất cả các lũy thừa (M) là đúng vì mọi câu trả lời có thể đều sử dụng chính xác một trong số chúng. 

Vấn đề là tính toán SCC lặp đi lặp lại. Mặc dù một lần kiểm tra 2-SAT là tuyến tính, nhưng việc nhân nó với (M) sẽ tạo ra các phép tính khoảng (4,8\cdot10^{11}) trong trường hợp xấu nhất. Chữ lớn (M) đặc biệt ở đó để loại trừ cách tiếp cận này. 

Quan sát quan trọng là chúng ta thực sự không cần liệt kê (f). Thay vào đó, hãy biểu thị câu lệnh “(f) ít nhất là (t)” dưới dạng một biến Boolean khác. Gọi nó là (T_t). Chúng ta thêm (T_{t+1}\rightarrow T_t), do đó giá trị đúng của các biến này phải tạo thành tiền tố của giá trị đúng. Chúng tôi cũng buộc (T_1) thành đúng và (T_{M+1}) thành sai. Do đó, mọi phép gán thỏa mãn đều chứa chính xác một điểm cắt và điểm cắt đó có giá trị pháp lý là (f). 

Điều này làm cho các khoảng thời gian của trạm trở thành ý nghĩa 2-SAT thông thường. Nếu trạm (i) được chọn thì (f\ge l_i), do đó (S_i\rightarrow T_{l_i}). Ngoài ra (f\le r_i), tương đương với (f<r_i+1), do đó (S_i\rightarrow\lnot T_{r_i+1}). Cả hai đều là hàm ý thông thường giữa các chữ Boolean. 

Do đó, toàn bộ vấn đề là một trường hợp 2-SAT với các biến Boolean (p+M+1). Việc xây dựng chỉ có mệnh đề (O(n+p+m+M)) và có thể được giải bằng một phép tính SCC. Đây là quan điểm tối ưu hóa tiền tố tương tự thường được sử dụng cho vấn đề này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(M(p+n+m))) | (O(p+n+m)) | Quá chậm | 
| Tối ưu | (O(p+n+m+M)) | (O(p+n+m+M)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tạo biến Boolean (S_i) cho mỗi trạm. Giá trị đúng của nó có nghĩa là trạm (i) được chọn và giá trị sai của nó có nghĩa là trạm đó không được chọn. Biểu diễn mọi biến Boolean bằng hai đỉnh của đồ thị hàm ý, một cho nghĩa đen và một cho phần phủ định của nó. 
2. Chuyển mọi khiếu nại ((x,y)) thành mệnh đề (S_x\lor S_y). Trong biểu đồ hàm ý, điều này cho ta hai cạnh (\lnot S_x\rightarrow S_y) và (\lnot S_y\rightarrow S_x). Những khía cạnh này thể hiện chính xác hai cách mà lời phàn nàn có thể bị buộc phải chấp nhận. 
3. Chuyển từng cặp giao thoa ((u,v)) thành mệnh đề (\lnot S_u\lor\lnot S_v). Các cạnh hàm ý của nó là (S_u\rightarrow\lkhông phải S_v) và (S_v\rightarrow\lkhông phải S_u). Do đó, việc chọn một trong hai điểm cuối sẽ buộc điểm cuối kia không được chọn. 
4. Giới thiệu một biến Boolean (T_t) cho mọi (t) từ 1 đến (M+1), trong đó ý nghĩa mong muốn là (T_t=(f\ge t)). Thêm các mệnh đề (\lnot T_{t+1}\lor T_t) cho mọi (t) từ 1 đến (M). Các mệnh đề này buộc các biến ngưỡng phải đơn điệu. 
5. Buộc (T_1) thành đúng và (T_{M+1}) thành sai. Vì các biến ngưỡng đều đơn điệu nên hiện tại có chính xác một ranh giới giữa giá trị đúng và giá trị sai. Nếu ngưỡng đúng lớn nhất là (f), thì (T_t) đúng chính xác với (t\le f), do đó phép gán Boolean này biểu thị công suất tín hiệu số nguyên (f). 
6. Với mỗi trạm (i), hãy thêm (S_i\rightarrow T_{l_i}). Nếu trạm (i) được chọn, ngưỡng của (l_i) phải đúng, nghĩa là (f\ge l_i). 
7. Với mỗi trạm (i), hãy thêm (S_i\rightarrow\lnot T_{r_i+1}). Vì (T_{r_i+1}) có nghĩa là (f\ge r_i+1), nên phủ định của nó có nghĩa là (f\le r_i). Cùng với hàm ý trước đó, việc chọn trạm (i) sẽ buộc (l_i\le f\le r_i). 
8. Xây dựng biểu đồ hàm ý từ tất cả các mệnh đề này và tính toán các thành phần liên kết chặt chẽ của nó bằng thuật toán Tarjan. Một thể hiện 2-SAT là không thể chính xác khi một số biến và phủ định của nó thuộc cùng một SCC. Chúng tôi kiểm tra điều này cho cả biến trạm và biến ngưỡng. 
9. Nếu công thức phù hợp thì gán từng biến theo thứ tự SCC. Với cách đánh số thành phần của Tarjan, các thành phần được tạo theo thứ tự tôpô ngược, do đó, chữ có số thành phần nhỏ hơn là giá trị thực được chọn. Đối với một trạm, (S_i) đúng khi chữ đúng của nó có số thành phần nhỏ hơn chữ sai của nó. 
10. Quét (T_1,\ldots,T_M) và lấy ngưỡng lớn nhất có chữ thực được chọn. Tính đơn điệu đảm bảo rằng các ngưỡng thực sự này tạo thành một tiền tố, do đó chỉ số lớn nhất này chính xác là chỉ số bắt buộc (f). Xuất ra mọi trạm có biến (S_i) là đúng. 

Điều bất biến đằng sau việc xây dựng là mọi phép gán thỏa mãn các biến ngưỡng đều tương ứng với chính xác một số nguyên hợp pháp (f), trong khi mọi trạm được chọn buộc phải có toàn bộ khoảng chứa số nguyên đó (f). Ngược lại, bất kỳ lựa chọn hợp lệ nào của (f) và các trạm đều có thể được chuyển thành giá trị chân lý cho tất cả (S_i) và (T_t) thỏa mãn mọi mệnh đề được xây dựng. Do đó, phiên bản 2-SAT được xây dựng có thể thỏa mãn chính xác khi bài toán ban đầu có câu trả lời. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    input = sys.stdin.readline

    n, p, M, m = map(int, input().split())

    # Store complaints temporarily. Conflicts can be processed later.
    complaints = array('i')
    for _ in range(n):
        x, y = map(int, input().split())
        complaints.append(x - 1)
        complaints.append(y - 1)

    left = array('i')
    right = array('i')
    for _ in range(p):
        l, r = map(int, input().split())
        left.append(l)
        right.append(r)

    # Variables:
    #   0 .. p-1          : station variables
    #   p .. p+M          : threshold variables T_1 .. T_{M+1}
    variables = p + M + 1
    vertices = variables * 2

    # Store clauses as pairs of literal IDs.
    # Literal 2*v is v=True, literal 2*v+1 is v=False.
    clauses = array('i')

    # Complaint: S_x OR S_y
    for i in range(0, 2 * n, 2):
        x = complaints[i]
        y = complaints[i + 1]
        clauses.append(2 * x)
        clauses.append(2 * y)

    del complaints

    # Station interval:
    # S_i -> T_l
    # S_i -> !T_{r+1}
    for i in range(p):
        station_true = 2 * i

        tl_var = p + left[i] - 1
        tr1_var = p + right[i]

        clauses.append(station_true ^ 1)
        clauses.append(2 * tl_var)

        clauses.append(station_true ^ 1)
        clauses.append(2 * tr1_var + 1)

    del left
    del right

    # Conflict: !S_u OR !S_v
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        clauses.append(2 * u + 1)
        clauses.append(2 * v + 1)

    # T_{t+1} -> T_t
    # Equivalent to !T_{t+1} OR T_t.
    for t in range(1, M + 1):
        cur = p + t - 1
        nxt = cur + 1
        clauses.append(2 * nxt + 1)
        clauses.append(2 * cur)

    # T_1 must be true.
    t1 = p
    clauses.append(2 * t1)

    # T_{M+1} must be false.
    tm1 = p + M
    clauses.append(2 * tm1 + 1)

    # Build adjacency in CSR form.
    # For clause (a OR b):
    #   !a -> b
    #   !b -> a
    degree = array('i', [0]) * vertices

    clen = len(clauses)
    for i in range(0, clen, 2):
        a = clauses[i]
        b = clauses[i + 1]
        degree[a ^ 1] += 1
        degree[b ^ 1] += 1

    start = array('i', [0]) * (vertices + 1)
    total = 0
    for i in range(vertices):
        start[i] = total
        total += degree[i]
    start[vertices] = total

    edge = array('i', [0]) * total
    pos = array('i', start)

    for i in range(0, clen, 2):
        a = clauses[i]
        b = clauses[i + 1]

        u = a ^ 1
        idx = pos[u]
        edge[idx] = b
        pos[u] = idx + 1

        u = b ^ 1
        idx = pos[u]
        edge[idx] = a
        pos[u] = idx + 1

    del clauses
    del degree
    del pos

    # Iterative Tarjan SCC.
    #
    # Recursive Tarjan is unsafe here because the graph can have more
    # than 1.6 million vertices.
    dfn = array('i', [0]) * vertices
    low = array('i', [0]) * vertices
    comp = array('i', [0]) * vertices
    on_stack = bytearray(vertices)

    scc_stack = array('i')
    dfs_vertices = array('i')
    dfs_edges = array('i')

    timer = 0
    component_count = 0

    for root in range(vertices):
        if dfn[root]:
            continue

        timer += 1
        dfn[root] = timer
        low[root] = timer
        on_stack[root] = 1
        scc_stack.append(root)

        dfs_vertices.append(root)
        dfs_edges.append(start[root])

        while dfs_vertices:
            v = dfs_vertices[-1]
            e = dfs_edges[-1]

            if e < start[v + 1]:
                w = edge[e]
                dfs_edges[-1] = e + 1

                if dfn[w] == 0:
                    timer += 1
                    dfn[w] = timer
                    low[w] = timer
                    on_stack[w] = 1
                    scc_stack.append(w)

                    dfs_vertices.append(w)
                    dfs_edges.append(start[w])
                elif on_stack[w]:
                    dw = dfn[w]
                    if dw < low[v]:
                        low[v] = dw
            else:
                dfs_vertices.pop()
                dfs_edges.pop()

                if dfs_vertices:
                    parent = dfs_vertices[-1]
                    lv = low[v]
                    if lv < low[parent]:
                        low[parent] = lv

                if low[v] == dfn[v]:
                    component_count += 1
                    while True:
                        w = scc_stack.pop()
                        on_stack[w] = 0
                        comp[w] = component_count
                        if w == v:
                            break

    del dfn
    del low
    del on_stack
    del scc_stack
    del dfs_vertices
    del dfs_edges

    # Every variable must be different from its negation.
    for v in range(variables):
        if comp[2 * v] == comp[2 * v + 1]:
            print(-1)
            return

    # Tarjan numbers SCCs in reverse topological order.
    # Smaller component number means the literal is chosen.
    selected = []
    for i in range(p):
        if comp[2 * i] < comp[2 * i + 1]:
            selected.append(i + 1)

    # Recover f from the threshold variables.
    f = 1
    for t in range(1, M + 1):
        var = p + t - 1
        if comp[2 * var] < comp[2 * var + 1]:
            f = t

    print(len(selected), f)
    print(*selected)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của việc thực hiện đọc các khiếu nại và khoảng thời gian. Các khiếu nại phải được lưu trữ tạm thời vì dữ liệu khoảng thời gian của trạm đến sau, trong khi các cặp nhiễu đến sau các khoảng thời gian và có thể được chuyển đổi trực tiếp thành các mệnh đề. 

Mỗi biến Boolean chiếm hai ID bằng chữ liên tiếp. ID chẵn đại diện cho chữ đúng và ID lẻ đại diện cho chữ sai, vì vậy phủ định chỉ đơn giản là`literal ^ 1`. Điều này làm cho việc xây dựng hàm ý trở nên nhỏ gọn và tránh việc lưu trữ các đối tượng riêng biệt cho các hằng. 

Mã này tạm thời lưu trữ tất cả các mệnh đề 2-SAT dưới dạng`array('i')`. Một danh sách Python bình thường sẽ tiêu tốn nhiều bộ nhớ hơn vì các phần tử số nguyên của nó là các đối tượng Python. Lý do tương tự thúc đẩy việc sử dụng`array('i')`cho đồ thị và mảng SCC. 

Biểu đồ hàm ý được lưu trữ ở dạng CSR thay vì dưới dạng danh sách các danh sách Python. các`start[v]`Và`start[v+1]`phạm vi chứa chính xác các cạnh đi của đỉnh`v`. Điều này tránh được hàng triệu đối tượng danh sách Python và giữ mọi chỉ mục biểu đồ ở mức bốn byte. 

Tính toán SCC được lặp lại Tarjan. DFS đệ quy có thể đạt tới độ sâu tỷ lệ thuận với số đỉnh đồ thị, ở đây có thể vượt quá 1,6 triệu. Hai ngăn xếp DFS rõ ràng duy trì cùng một trạng thái mà các lệnh gọi đệ quy sẽ được giữ nguyên: đỉnh hiện tại và cạnh đi tiếp theo vẫn cần được kiểm tra. 

Mã hóa khoảng thời gian có chủ ý sử dụng (r_i+1). Điều kiện trên là (f\le r_i), chính xác là (\lnot(f\ge r_i+1)). Vì (r_i) có thể bằng (M), nên cần có ngưỡng bổ sung (T_{M+1}). Nó bị ép buộc sai, do đó trạm có (r_i=M) nhận được giới hạn trên không hạn chế chính xác. 

Không có số nguyên nào có thể tràn trong Python và mỗi chỉ mục biểu đồ tối đa là khoảng 1,6 triệu, điều này cũng vừa vặn thoải mái bên trong các mảng bốn byte được sử dụng khi triển khai. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, hãy xem xét phép gán hợp lệ với (f=3) và trạm 1 và 3 được chọn. Các biến ngưỡng mô tả (f=3) là (T_1=T_2=T_3=\text{true}) và (T_4=\text{false}). 

| Sân khấu | Tiểu bang | 
| --- | --- | 
| Nguồn tín hiệu | (f=3) | 
| (T_1,T_2,T_3,T_4) | đúng, đúng, đúng, sai | 
| Trạm đã chọn | 1, 3 | 
| Khoảng trạm 1 | ([1,4]), hợp lệ | 
| Khoảng trạm 3 | ([3,4]), hợp lệ | 
| Khiếu nại ((1,3)) | hài lòng ở trạm 1 hoặc 3 | 
| Khiếu nại ((2,3)) | hài lòng với trạm 3 | 
| Xung đột ((1,4)) | trạm 4 không được chọn | 
| Xung đột ((3,4)) | trạm 4 không được chọn | 
| Kết quả | hợp lệ | 

Bài tập SCC có thể chọn một bài tập thỏa mãn khác vì bài toán cho phép trả lời tùy ý. Điều quan trọng là các biến ngưỡng đã chọn tạo thành tiền tố và mọi trạm được chọn đều tương thích với ngưỡng kết quả. 

Đối với Mẫu 2, trạm 1 và 2 chỉ có sẵn cho nguồn 1 và 2, trong khi trạm 3 và 4 chỉ có sẵn cho nguồn 3 và 4. 

| Nguồn tín hiệu | Trạm có sẵn | Khiếu nại đầu tiên | Khiếu nại thứ hai | Xung đột | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1, 2 | yêu cầu 1 hoặc 2 | yêu cầu 2 hoặc 4 nên 2 | Không thể chọn cùng lúc 1 và 2 | không thể | 
| 2 | 1, 2 | yêu cầu 1 hoặc 2 | yêu cầu 2 hoặc 4 nên 2 | Không thể chọn cùng lúc 1 và 2 | không thể | 
| 3 | 3, 4 | yêu cầu 1 hoặc 3 nên 3 | yêu cầu 2 hoặc 4 nên 4 | 3 và 4 không thể chọn cùng lúc | không thể | 
| 4 | 3, 4 | yêu cầu 1 hoặc 3 nên 3 | yêu cầu 2 hoặc 4 nên 4 | 3 và 4 không thể chọn cùng lúc | không thể | 

Công thức ngưỡng thể hiện tất cả bốn khả năng trong một phiên bản 2-SAT thay vì xây dựng lại công thức bốn lần. Việc tính toán SCC phát hiện ra sự mâu thuẫn giữa các lựa chọn cần thiết và các mệnh đề can thiệp, do đó chương trình in ra`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+p+m+M)) | Có (O(n+p+m+M)) mệnh đề và đỉnh, và Tarjan kiểm tra mọi đỉnh và cạnh hàm ý một lần. | 
| Không gian | (O(n+p+m+M)) | Các mệnh đề tạm thời, biểu đồ CSR, mảng SCC và các khoảng đầu vào đều tuyến tính ở kích thước đầu vào. | 

Ở giới hạn tối đa có nhiều nhất khoảng (8\cdot10^5) biến Boolean và khoảng (4\cdot10^6) cạnh hàm ý. Việc triển khai sử dụng các mảng số nguyên bốn byte được đóng gói và truyền tải SCC lặp, giúp giữ bộ nhớ về cơ bản ở mức dưới giới hạn 256 MB. Cấu trúc tuyến tính và đường dẫn SCC thay thế công việc ở quy mô brute-force (4,8\cdot10^{11}) bằng vài triệu thao tác biểu đồ. 

## Trường hợp thử nghiệm 

Vì đầu ra không phải là duy nhất nên việc so sánh chuỗi chính xác là không phù hợp cho các thử nghiệm này. Khai thác thử nghiệm bên dưới sẽ kiểm tra xem giải pháp được trả về có hợp lệ về mặt ngữ nghĩa hay không. Thử nghiệm kích thước tối đa chỉ kiểm tra xem người giải có tìm ra giải pháp hay không, vì việc phân tích cú pháp đầy đủ và xác thực độc lập hàng trăm nghìn dòng sẽ khiến bản thân bộ khai thác thử nghiệm trở nên đắt đỏ không cần thiết.```python
# Assume the submitted solution is saved as solution.py.
# Its solve() function reads stdin and writes stdout.

import sys
import io
from solution import solve

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

def validate(inp: str, out: str, possible: bool):
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    p = next(it)
    M = next(it)
    m = next(it)

    complaints = []
    for _ in range(n):
        complaints.append((next(it), next(it)))

    intervals = []
    for _ in range(p):
        intervals.append((next(it), next(it)))

    conflicts = []
    for _ in range(m):
        conflicts.append((next(it), next(it)))

    out = out.strip()

    if not possible:
        assert out == "-1"
        return

    tokens = list(map(int, out.split()))
    assert len(tokens) >= 2

    k, f = tokens[0], tokens[1]
    chosen = tokens[2:]

    assert 1 <= f <= M
    assert k == len(chosen)
    assert len(set(chosen)) == k
    assert all(1 <= x <= p for x in chosen)

    chosen_set = set(chosen)

    for x, y in complaints:
        assert x in chosen_set or y in chosen_set

    for u, v in conflicts:
        assert not (u in chosen_set and v in chosen_set)

    for x in chosen:
        l, r = intervals[x - 1]
        assert l <= f <= r

# Provided sample 1
sample1 = """\
2 4 4 2
1 3
2 3
1 4
1 2
3 4
1 4
1 2
3 4
"""
validate(sample1, run(sample1), True)

# Provided sample 2
sample2 = """\
2 4 4 2
1 3
2 4
1 2
1 2
3 4
3 4
1 2
3 4
"""
validate(sample2, run(sample2), False)

# Minimum feasible size under the distinct-pair condition.
case_min = """\
2 3 2 2
1 2
2 3
1 1
1 2
2 2
1 2
2 3
"""
validate(case_min, run(case_min), True)

# All intervals are equal, so the signal power is unrestricted inside [1, 2].
case_equal = """\
2 4 2 2
1 2
3 4
1 2
1 2
1 2
1 2
1 2
3 4
"""
validate(case_equal, run(case_equal), True)

# The two complaints require disjoint signal ranges.
case_impossible = """\
2 4 2 2
1 2
3 4
1 1
1 1
2 2
2 2
1 2
3 4
"""
validate(case_impossible, run(case_impossible), False)

# Endpoint test: station 2 is usable at both l=1 and r=2,
# and f=2 gives a valid solution using only station 2.
case_endpoint = """\
2 3 2 2
1 2
2 3
1 1
1 2
2 2
1 2
2 3
"""
validate(case_endpoint, run(case_endpoint), True)

# Maximum-size stress test.
# An even cycle is both the complaint graph and the conflict graph.
# Every interval is [1, M], so an alternating selection is valid.
N = 400000
P = 400000
MM = 400000
E = 400000

parts = [f"{N} {P} {MM} {E}\n"]

for i in range(1, N):
    parts.append(f"{i} {i + 1}\n")
parts.append(f"1 {N}\n")

parts.extend(["1 400000\n"] * P)

for i in range(1, N):
    parts.append(f"{i} {i + 1}\n")
parts.append(f"1 {N}\n")

maximum_case = "".join(parts)
assert not run(maximum_case).startswith("-1")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Bất kỳ bài tập hợp lệ nào | Phiên bản thỏa mãn cơ bản với các khoảng thời gian trạm khác nhau | 
| Mẫu 2 |`-1`| Tương tác giữa các khiếu nại, xung đột và dải tần rời rạc | 
|`case_min`| Bất kỳ bài tập hợp lệ nào | Cấu hình hợp lệ thực tế nhỏ nhất theo yêu cầu cặp riêng biệt | 
|`case_equal`| Bất kỳ bài tập hợp lệ nào | Tất cả các khoảng bằng nhau và có nhiều công suất tín hiệu có thể | 
|`case_impossible`|`-1`| Khiếu nại yêu cầu dải tần tách biệt nhau | 
|`case_endpoint`| Bất kỳ phép gán hợp lệ nào, với (f=2) có thể | Bao gồm ranh giới khoảng dưới và trên | 
|`maximum_case`| Bất kỳ bài tập hợp lệ nào | Giá trị tối đa của cả bốn thông số đầu vào chính và áp suất bộ nhớ | 

## Vỏ cạnh 

Trường hợp điểm cuối bao gồm được xử lý theo hàm ý trên (S_i\rightarrow\lnot T_{r_i+1}). TRONG`case_endpoint`, trạm 2 có khoảng ([1,2]). Khi phép gán SCC chọn (T_2=\text{true}) và (T_3=\text{false}), trạm 2 được cho phép vì các bất đẳng thức bắt buộc là (f\ge1) và (f<3), cho (f\le2). Do đó, thuật toán có thể trả về trạm 2 với (f=2). 

Trường hợp tần số rời rạc được xử lý vì các biến ngưỡng mang tính toàn cục thay vì được kiểm tra độc lập. TRONG`case_impossible`, chọn trạm từ lực lượng khiếu nại thứ nhất (f=1), đồng thời chọn trạm từ lực lượng khiếu nại thứ hai (f=2). Các mệnh đề ngưỡng làm cho các yêu cầu này trở thành một phần của cùng một hệ thống Boolean và mâu thuẫn dẫn đến đặt một biến và phủ định của nó vào cùng một SCC. Thuật toán trả về`-1`. 

Ranh giới của dải công suất cho phép được bảo vệ bởi các biến bắt buộc (T_1=\text{true}) và (T_{M+1}=\text{false}). Đối với phép gán giả định tương ứng với (f=0), (T_1) sẽ phải sai, mâu thuẫn với mệnh đề đơn vị đầu tiên. Đối với phép gán tương ứng với (f=M+1), (T_{M+1}) sẽ phải đúng, mâu thuẫn với mệnh đề đơn vị thứ hai. Do đó, mọi phép gán ngưỡng còn tồn tại đều tương ứng với một số (f\in[1,M]). 

Cuối cùng, thuật toán không cho rằng quan hệ nhiễu là bắc cầu. Mỗi cặp được liệt kê đóng góp chính xác một mệnh đề “không được chọn cả hai”. Nếu trạm 1 xung đột với trạm 2 và trạm 2 xung đột với trạm 3 thì việc xây dựng không tạo ra xung đột giữa trạm 1 và 3. Điều này khớp với biểu đồ trong đầu vào và ngăn ngừa lỗi phổ biến trong đó các cặp giao thoa được xử lý không chính xác như các thành phần được kết nối.
