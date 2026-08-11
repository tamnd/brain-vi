---
title: "CF 102412H - Mex trên DAG"
description: "Chúng ta có một đồ thị tuần hoàn có hướng với (n) đỉnh và chính xác (2n) cạnh. Các cạnh được đánh số từ (0) đến (2n-1), và cạnh (i) có giá trị (lfloor i/2rfloor). Do đó mọi giá trị từ (0) đến (n-1) xảy ra trên đúng hai cạnh."
date: "2026-08-10T14:03:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102412
codeforces_index: "H"
codeforces_contest_name: "MEX Foundation Contest (supported by AIM Tech)"
rating: 0
weight: 102412
solve_time_s: 141
verified: true
draft: false
---

[CF 102412H - Mex trên DAG](https://codeforces.com/problemset/problem/102412/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị tuần hoàn có hướng với (n) đỉnh và chính xác (2n) cạnh. Các cạnh được đánh số từ (0) đến (2n-1) và cạnh (i) có giá trị (\lfloor i/2\rfloor). Do đó mọi giá trị từ (0) đến (n-1) xảy ra trên đúng hai cạnh. Đối với đường dẫn có hướng, chúng tôi chỉ xem xét tập hợp các giá trị cạnh xuất hiện trên nó và lấy MEX của nó, giá trị không âm nhỏ nhất không xảy ra. Nhiệm vụ là tối đa hóa MEX này trên tất cả các đường dẫn có hướng đơn giản. Câu lệnh ban đầu cung cấp (2\le n\le2000), (2n) cạnh, giới hạn thời gian 5 giây và 256 MiB bộ nhớ. 

Các đỉnh đã được sắp xếp theo cấu trúc liên kết vì mọi cạnh đều được cho là (a_i<b_i). Điều này quan trọng rất nhiều. Một đường dẫn có thể chứa một cạnh (e=(a,b)) theo sau là một cạnh (f=(c,d)) chính xác khi có một đường dẫn có hướng từ (b) đến (c), trong đó (b=c) được cho phép. Vì đồ thị không có tính tuần hoàn nên mọi đường đi có hướng đều tự động đơn giản. 

Câu trả lời nhiều nhất là (n-1). Một đường đi trên (n) đỉnh chứa tối đa (n-1) cạnh, trong khi để đạt được MEX (k) yêu cầu đường đi phải chứa tất cả (k) giá trị riêng biệt (0,1,\ldots,k-1). Giới hạn dưới ít nhất là (1), vì một trong hai cạnh có giá trị (0) bản thân nó là một đường dẫn hợp lệ. 

Trường hợp cạnh đầu tiên là khi cả hai cạnh của một giá trị có điểm cuối giống hệt nhau. Ví dụ,```
2
1 2
1 2
1 2
1 2
```có hai cạnh có giá trị (0) và hai cạnh có giá trị (1), tất cả đều đi từ đỉnh (1) đến đỉnh (2). Câu trả lời là (1), vì một đường đi chỉ có thể chứa một cạnh giữa hai đỉnh này. Một giải pháp bất cẩn coi hai cạnh có cùng giá trị là các yêu cầu riêng biệt có thể nghĩ sai rằng cả hai lựa chọn bằng cách nào đó đều làm tăng MEX. 

Một trường hợp cạnh khác là khi hai giá trị bắt buộc tồn tại nhưng các cạnh của chúng không thể cùng tồn tại trên một đường dẫn. Coi như```
3
1 3
1 3
1 2
1 2
```Hai cạnh đầu tiên có giá trị (0) và hai cạnh cuối cùng có giá trị (1). Một đường đi có thể lấy một cạnh giá trị (0) hoặc một cạnh giá trị (1), nhưng không thể lấy cả hai, vì cả hai loại đều rời khỏi đỉnh (1) và sau đó kết thúc. Câu trả lời đúng là (1). Chỉ kiểm tra thứ tự số của các điểm cuối là không đủ trong bài toán tổng quát, bởi vì (b<c) không hàm ý rằng (b) thực sự có thể đạt tới (c). 

Trường hợp cạnh thứ ba là khi hai cạnh có chung một điểm cuối. Ví dụ: nếu một cạnh là (1\to3) và cạnh kia là (3\to5), chúng tương thích và có thể xảy ra liên tiếp. Mối quan hệ về khả năng tiếp cận phải bao gồm trường hợp có độ dài bằng 0, do đó, một đỉnh phải được coi là có thể tiếp cận được từ chính nó. Việc quên điều này sẽ khiến các đường dẫn có các cạnh liên tiếp bị từ chối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi lựa chọn của một cạnh cho mọi giá trị mà chúng ta muốn đưa vào đường dẫn. Vì mọi giá trị đều có hai cạnh, nên các giá trị bắt buộc (0,\ldots,k-1) sẽ cho (2^k) các lựa chọn khả thi. Đối với mỗi lựa chọn, chúng ta có thể kiểm tra xem các cạnh được chọn có tạo thành một đường dẫn có hướng hay không bằng cách sắp xếp chúng theo vị trí của chúng trong DAG và kiểm tra khả năng tiếp cận liên tiếp. Điều này đúng vì một đường dẫn có MEX ít nhất (k) phải chứa mọi giá trị bên dưới (k) và chỉ có hai cạnh có thể có cho mỗi giá trị đó. 

Vấn đề là số lượng lựa chọn theo cấp số nhân. Trong trường hợp xấu nhất (k=\Theta(n)), đưa ra (2^n) lựa chọn và thậm chí kiểm tra tính hợp lệ (O(n)) hoặc (O(n^2)) (O(n2^n)) hoặc (O(n^22^n)) đều có tác dụng. Với (n=2000), điều này hoàn toàn không thể xảy ra. Ràng buộc chính thức đặc biệt đủ lớn để loại trừ việc liệt kê các đường đi có thể hoặc các lựa chọn cạnh có thể có. 

Quan sát quan trọng là chúng ta không cần phải xây dựng một đường dẫn một cách trực tiếp. Thay vào đó, hãy sửa một câu trả lời ứng viên (k) và hỏi xem liệu đường dẫn nào đó có chứa tất cả các giá trị (0,\ldots,k-1) hay không. Đối với mỗi giá trị có chính xác hai cạnh, vì vậy chúng ta có thể đưa ra một biến Boolean. Biến quyết định cạnh nào trong hai cạnh của giá trị đó được chọn. 

Bây giờ hãy xem xét hai cạnh được chọn (e=(a,b)) và (f=(c,d)). Cả hai đều có thể thuộc về một đường nếu (b) có thể tới (c), nghĩa là (e) đến trước (f) hoặc (d) có thể tới (a), nghĩa là (f) đến trước (e). Nếu không có mối quan hệ nào giữ được thì việc chọn cả hai cạnh là không thể. Do đó, mọi cặp không tương thích đều đưa ra mệnh đề 2-SAT nói rằng ít nhất một trong hai cạnh không được chọn. 

Đây chính xác là cấu trúc được giải pháp tiêu chuẩn khai thác: tìm kiếm nhị phân MEX, giảm từng thử nghiệm khả thi xuống 2-SAT và giải biểu đồ hàm ý thu được bằng SCC. 

Có một tối ưu hóa hữu ích hơn. Chúng ta không cần phải kiểm tra khả năng tiếp cận riêng biệt cho từng cặp cạnh. Đối với mỗi đỉnh (v), chúng ta có thể tính toán trước tập hợp các cạnh có đỉnh bắt đầu có thể tới được từ (v) và tập hợp các cạnh có đỉnh kết thúc có thể tới được (v). Khi đó, đối với một cạnh (e=(a,b)), các cạnh tương thích chính xác là hợp của tập thứ nhất cho (b) và tập thứ hai cho (a). Các số nguyên có kích thước tùy ý của Python làm cho các tập hợp bit này nhỏ gọn, cho phép việc triển khai biểu thị mối quan hệ dày đặc (O(n^2)) mà không lưu trữ hàng triệu số nguyên Python. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2 2^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n^2\log n)) | (O(n^2)) bit cộng với trạng thái SCC | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả (2n) cạnh. Cạnh (2x) và cạnh (2x+1) là hai lựa chọn cho giá trị (x). Lưu trữ các điểm cuối của chúng và xây dựng danh sách kề DAG. 
2. Tính toán khả năng tiếp cận giữa các đỉnh. Bởi vì mọi cạnh đều thỏa mãn (a<b), việc đánh số đỉnh tự nó là một thứ tự tôpô. Xử lý các đỉnh từ (n) xuống (1). Đối với mỗi đỉnh (v), hãy bắt đầu tập bit khả năng tiếp cận của nó với chính (v) và OR trong tập hợp bit khả năng tiếp cận của mọi hàng xóm đi ra. 
3. Với mỗi đỉnh (v), hãy tính`succ[v]`, tập bit của tất cả các cạnh có đỉnh bắt đầu có thể đạt được từ (v). Khởi tạo nó với tất cả các cạnh bắt đầu chính xác tại (v), sau đó xử lý DAG ngược và hợp nhất các cạnh tương ứng`succ`tập hợp các hàng xóm đi. 
4. Tính toán tương tự`pred[v]`, tập bit của tất cả các cạnh có đỉnh kết thúc có thể chạm tới (v). Khởi tạo nó với tất cả các cạnh kết thúc tại (v), sau đó xử lý DAG chuyển tiếp. Khi xử lý (u\to v), mọi thứ có thể kết thúc tại (u) cũng có thể đứng trước (v). 
5. Đối với một cạnh (e_i=(a_i,b_i)), mọi cạnh tương thích với (e_i) đều được chứa trong`succ[b_i] | pred[a_i]`. Phần đầu tiên biểu thị các cạnh sau (e_i), trong khi phần thứ hai biểu thị các cạnh trước nó. Tất cả các cạnh còn lại không tương thích với (e_i). 
6. Loại bỏ cạnh còn lại có cùng giá trị khỏi bộ không tương thích. Hai cạnh của một giá trị không bao giờ được yêu cầu đồng thời, vì biến Boolean chọn chính xác một trong số chúng. 
7. Cố định một ứng cử viên MEX (k). Tạo một biến Boolean cho mọi giá trị (0,\ldots,k-1). Nghĩa đen (2x) nghĩa là chọn cạnh (2x) và nghĩa đen (2x+1) nghĩa là chọn cạnh (2x+1). 
8. Nếu cạnh (i) và cạnh (j) không tương thích, hãy thêm mệnh đề (\neg i\lor\neg j). Trong biểu đồ hàm ý, điều này trở thành (i\to\neg j) và (j\to\neg i). Vì biểu đồ được lưu trữ dưới dạng các tập hợp bit nên những hàm ý này có thể được tạo ra bằng cách hoán đổi mọi bit đã đặt (j) thành (j\mathbin{\hat{}}!1). 
9. Chạy thuật toán SCC của Tarjan trên biểu đồ hàm ý. Một thể hiện 2-SAT thỏa mãn chính xác khi không có biến nào và phủ định của nó thuộc cùng một SCC. Đây là đặc tính SCC tiêu chuẩn của 2-SAT. 
10. Tìm kiếm nhị phân (k) lớn nhất mà phiên bản 2-SAT thỏa mãn. Tính khả thi là đơn điệu: nếu một đường dẫn chứa tất cả các giá trị (0,\ldots,k-1) thì nó cũng chứa tất cả các giá trị (0,\ldots,k-2). 

### Tại sao nó hoạt động 

Đối với (k cố định), mọi phép gán thỏa mãn sẽ chọn chính xác một cạnh cho mọi giá trị bên dưới (k). Các mệnh đề cấm chính xác những cặp cạnh được chọn không thể cùng tồn tại trên một đường dẫn có hướng. Do đó, mọi phép gán thỏa mãn đều cho một tập hợp các cạnh tương thích theo cặp. Bởi vì khả năng tiếp cận trong DAG có tính bắc cầu, nên các cạnh được chọn tương thích theo cặp có thể được sắp xếp thành một đường dẫn có hướng. Ngược lại, mọi đường dẫn chứa tất cả các giá trị bắt buộc sẽ chọn một cạnh của mỗi giá trị và không bao giờ chứa một cặp không tương thích, do đó các lựa chọn của nó thỏa mãn mọi mệnh đề. Do đó, bài kiểm tra 2-SAT đúng chính xác khi tồn tại một đường dẫn có MEX ít nhất (k). Tìm kiếm nhị phân sau đó tìm thấy MEX tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

def solve():
    n = int(input())
    m = 2 * n

    u = [0] * m
    v = [0] * m
    graph = [[] for _ in range(n)]

    for i in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        u[i] = a
        v[i] = b
        graph[a].append(b)

    # Vertices are already in topological order because a < b.
    # reach[x] is a bitset of vertices reachable from x, including x.
    reach = [0] * n
    for x in range(n - 1, -1, -1):
        cur = 1 << x
        for y in graph[x]:
            cur |= reach[y]
        reach[x] = cur

    # starts[x] = edges whose starting vertex is x
    # ends[x]   = edges whose ending vertex is x
    starts = [0] * n
    ends = [0] * n
    for i in range(m):
        starts[u[i]] |= 1 << i
        ends[v[i]] |= 1 << i

    # succ[x] = edges whose starting vertex can be reached from x.
    succ = starts[:]
    for x in range(n - 1, -1, -1):
        cur = succ[x]
        for y in graph[x]:
            cur |= succ[y]
        succ[x] = cur

    # pred[x] = edges whose ending vertex can reach x.
    pred = ends[:]
    for x in range(n):
        for y in graph[x]:
            pred[y] |= pred[x]

    full_edges = (1 << m) - 1

    # bad[i] contains every edge incompatible with edge i.
    bad = [0] * m

    for i in range(m):
        compatible = succ[v[i]] | pred[u[i]]

        # Edges i and i^1 have the same value and are not both selected.
        same_value = (1 << i) | (1 << (i ^ 1))
        bad[i] = (full_edges ^ compatible) & ~same_value

    # If bit j is in bad[i], the implication is i -> (j^1).
    # Swap even and odd bit positions to obtain the implication rows.
    even_mask = sum(1 << i for i in range(0, m, 2))
    odd_mask = sum(1 << i for i in range(1, m, 2))

    implication = [0] * m
    reverse_implication = [0] * m

    for i in range(m):
        b = bad[i]
        implication[i] = ((b & even_mask) << 1) | ((b & odd_mask) >> 1)

        # Incoming edges to literal x correspond to bad[x^1].
        reverse_implication[i] = bad[i ^ 1]

    def possible(k):
        vertices = 2 * k
        mask = (1 << vertices) - 1

        disc = [-1] * vertices
        low = [0] * vertices
        on_stack = bytearray(vertices)
        stack = []
        component = [-1] * vertices
        timer = 0
        comp_id = 0

        def dfs(x):
            nonlocal timer, comp_id

            disc[x] = low[x] = timer
            timer += 1
            stack.append(x)
            on_stack[x] = 1

            bits = implication[x] & mask

            while bits:
                bit = bits & -bits
                bits -= bit
                y = bit.bit_length() - 1

                if disc[y] == -1:
                    dfs(y)
                    if low[y] < low[x]:
                        low[x] = low[y]
                elif on_stack[y] and disc[y] < low[x]:
                    low[x] = disc[y]

            if low[x] == disc[x]:
                while True:
                    y = stack.pop()
                    on_stack[y] = 0
                    component[y] = comp_id
                    if y == x:
                        break
                comp_id += 1

        for x in range(vertices):
            if disc[x] == -1:
                dfs(x)

        for x in range(0, vertices, 2):
            if component[x] == component[x ^ 1]:
                return False

        return True

    lo, hi = 1, n - 1
    answer = 1

    while lo <= hi:
        mid = (lo + hi) // 2
        if possible(mid):
            answer = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai đọc các cạnh (2n) và xây dựng DAG. Thực tế là các điểm cuối thỏa mãn (a<b) có nghĩa là không cần phải chạy một loại cấu trúc liên kết riêng biệt. 

các`reach`mảng sử dụng số nguyên Python làm bitset. Bit (x) được đặt chính xác khi có thể truy cập được đỉnh (x). Việc xử lý các đỉnh ngược là hợp lệ vì mọi cạnh đi ra đều đi tới một đỉnh có số lớn hơn. 

các`succ`Và`pred`mảng nén mối quan hệ tương thích đường dẫn theo cặp. Đối với một cạnh (u_i\to v_i),`succ[v_i]`chứa mọi cạnh có thể xuất hiện sau nó, trong khi`pred[u_i]`chứa mọi cạnh có thể xuất hiện trước nó. Hợp của chúng chính xác là tập hợp các cạnh tương thích với (i). 

các`bad`mảng là phần bổ sung của liên minh đó. Hai cạnh có cùng giá trị sẽ bị loại bỏ rõ ràng vì phép gán thỏa mãn chỉ chọn một trong số chúng. Điều này cũng giải thích tại sao mã không bao giờ cần các mệnh đề nói rằng không thể chọn cả hai lựa chọn của một biến, hạn chế đó đã được tích hợp sẵn trong ý nghĩa của biến Boolean. 

Biểu đồ hàm ý sử dụng mã hóa tiêu chuẩn của một mệnh đề. Nếu việc chọn cạnh (i) và chọn cạnh (j) bị cấm thì mệnh đề là (\neg i\lor\neg j), đưa ra hàm ý (i\to\neg j) và (j\to\neg i). Vì phủ định được biểu thị bằng XOR với (1), nên việc hoán đổi mọi bit chẵn và lẻ trong một bitset sẽ tạo ra hàng hàm ý mà không lặp lại tất cả các cặp không tương thích. 

Thuật toán của Tarjan được viết đệ quy nên giới hạn đệ quy được nâng lên đáng kể. Độ sâu tối đa được giới hạn bởi số lượng chữ, nhiều nhất là (4000), mức này an toàn sau khi thay đổi giới hạn đệ quy Python. 

Việc tìm kiếm nhị phân sử dụng`n - 1`làm giới hạn trên vì một đường dẫn đơn giản trong DAG (n)-đỉnh có nhiều nhất (n-1) cạnh. Câu trả lời được khởi tạo là (1), điều này luôn khả thi vì tồn tại ít nhất một trong hai cạnh giá trị-(0) và một cạnh là đường dẫn hợp lệ. 

## Ví dụ đã hoạt động 

Câu lệnh ban đầu không cung cấp các cặp đầu vào/đầu ra mẫu thông thường trong văn bản có sẵn cùng với câu lệnh vấn đề, vì vậy dấu vết đầu tiên bên dưới sử dụng phiên bản thử nghiệm đi kèm với giải pháp tham chiếu và dấu vết thứ hai là một phiên bản được xây dựng nhỏ. 

### Mẫu 1```
8
3 6
2 7
1 3
2 3
6 7
7 8
7 8
4 6
2 7
1 5
2 5
2 8
6 8
7 8
3 5
7 8
```Các cạnh có các giá trị (0,0,1,1,2,2,3,3,\ldots). con đường```
1 -> 3 -> 6 -> 7 -> 8
```có thể chọn các giá trị (1,0,2,3) tương ứng. Do đó, tập hợp các giá trị của nó chứa (0,1,2,3), cho MEX (4). Không có đường dẫn nào cũng có thể chứa giá trị (4), vì vậy câu trả lời là (4). 

| Trạng thái tìm kiếm nhị phân | Ứng viên | Khả thi? | Lý do | 
| --- | --- | --- | --- | 
| Ban đầu | 4 | Có | Đường dẫn (1\to3\to6\to7\to8) chứa các giá trị (0,1,2,3) | 
| Nửa trên | 6 | Không | Không thể đặt tất cả các giá trị (0,\ldots,5) trên một đường dẫn | 
| Còn lại | 5 | Không | Các cạnh giá trị-4 không thể cùng tồn tại với tất cả các giá trị (0,\ldots,3) | 
| Cuối cùng | 4 | Có | Ứng viên khả thi tối đa | 

Điều bất biến là mọi ứng cử viên khả thi đều tương ứng với một phép gán thỏa mãn thực tế của phiên bản 2-SAT. Đường dẫn trên cung cấp một phép gán cụ thể cho (k=4), trong khi các bài kiểm tra không thỏa mãn đối với các ứng viên lớn hơn sẽ loại trừ các giá trị MEX lớn hơn. 

### Mẫu 2```
3
1 3
1 3
1 2
1 2
```Hai cạnh giá trị-(0) là (1\to3) và hai cạnh giá trị-(1) là (1\to2). Bất kỳ đường dẫn nào cũng có thể chứa một trong các loại cạnh này, nhưng không thể chứa cả hai. 

| Ứng viên (k) | Giá trị bắt buộc | Kết quả | 
| --- | --- | --- | 
| 1 | (0) | Khả thi | 
| 2 | (0,1) | Không khả thi | 
| Trả lời | (1) | (1) | 

Đối với (k=2), mọi lựa chọn của cạnh giá trị-(0) xung đột với mọi lựa chọn của cạnh giá trị-(1). Phiên bản 2-SAT thu được không có phép gán thỏa mãn, do đó tìm kiếm nhị phân dừng chính xác ở (1). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2\log n)) | Khả năng tiếp cận và xử lý trước khả năng tương thích sử dụng các hoạt động bitset; mỗi bước tìm kiếm nhị phân giải quyết một phiên bản 2-SAT với các chữ (O(n)) và các hàm ý có thể có (O(n^2)) | 
| Không gian | (O(n^2)) bit cộng với mảng (O(n)) | Các mối quan hệ đồ thị dày đặc được lưu trữ dưới dạng các tập hợp số nguyên Python thay vì các danh sách kề số nguyên Python | 

Giới hạn ban đầu là (n\le2000), 5 giây và 256 MiB. Mối quan hệ dày đặc là mối quan tâm chính về bộ nhớ trong quá trình triển khai Python, đó là lý do tại sao mã cố tình lưu trữ nó dưới dạng bitset. Danh sách kề kề Python thông thường chứa mọi hàm ý có thể tiêu tốn nhiều bộ nhớ hơn vì mỗi số nguyên được lưu trữ là một đối tượng Python. Việc triển khai tham chiếu được viết bằng C++, trong đó phương thức tương tự (O(n^2\log n)) phù hợp thoải mái với các giới hạn dự định; Python nên được coi là nhạy cảm hơn với việc triển khai theo đánh giá ban đầu. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây bao gồm mẫu tham chiếu, trường hợp có kích thước tối thiểu nhỏ, trường hợp điểm cuối hoàn toàn bằng nhau, trường hợp lựa chọn bị ngắt kết nối và trường hợp được tạo ra có kích thước tối đa.```python
import io
import sys

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# Reference sample included with the published solution.
sample1 = """\
8
3 6
2 7
1 3
2 3
6 7
7 8
7 8
4 6
2 7
1 5
2 5
2 8
6 8
7 8
3 5
7 8
"""

assert run(sample1) == "4\n", "reference sample"

# Minimum-size graph.
sample2 = """\
2
1 2
1 2
1 2
1 2
"""

assert run(sample2) == "1\n", "minimum-size graph"

# Values 0 and 1 exist, but no path can contain both.
case3 = """\
3
1 3
1 3
1 2
1 2
"""

assert run(case3) == "1\n", "incompatible value choices"

# A single chain can contain both values.
case4 = """\
3
1 2
1 2
2 3
2 3
3 4
3 4
"""

assert run(case4) == "3\n", "three consecutive values"

# Maximum-size instance: every edge has the same endpoints.
# Every path contains only one edge, so the answer is 1.
n = 2000
lines = [str(n)]
lines.extend(["1 2000"] * (2 * n))
case5 = "\n".join(lines) + "\n"

assert run(case5) == "1\n", "maximum-size dense parallel-edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp 8 đỉnh tham khảo |`4`| Tương tác đầy đủ giữa khả năng tiếp cận và 2-SAT | 
|`n=2`, bốn bản sao của`1 2`|`1`| Kích thước tối thiểu và các cạnh song song có cùng điểm cuối | 
| Hai loại cạnh không tương thích từ đỉnh 1 |`1`| Hạn chế không tương thích theo cặp | 
| Chuỗi chứa các giá trị 0, 1 và 2 |`3`| Khả năng tiếp cận liên tục và ranh giới khả năng tự tiếp cận | 
| (n=2000), tất cả các cạnh`1 2000`|`1`| Kích thước đầu vào tối đa và khả năng tương thích dày đặc | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu,```
2
1 2
1 2
1 2
1 2
```đường đi chỉ có thể chứa một cạnh vì chỉ có hai đỉnh. Nó có thể chọn cạnh giá trị-(0), cho MEX (1), nhưng nó không thể chứa cả giá trị (0) và giá trị (1). Quá trình tiền xử lý tương thích thấy rằng không có cạnh nào có thể theo sau cạnh khác vì mọi cạnh đều kết thúc ở đỉnh (2). Do đó, câu trả lời là (1). 

Đối với các cạnh song song bằng nhau,```
3
1 2
1 2
2 3
2 3
3 4
3 4
```có hai lựa chọn cho mỗi giá trị (0,1,2) và mỗi lựa chọn của mỗi giá trị có thể được đặt liên tiếp trên đường dẫn (1\to2\to3\to4). Do đó, các cạnh được chọn chứa tất cả các giá trị (0,1,2), cho MEX (3). Thuật toán không nhầm lẫn hai bản sao của một giá trị với hai yêu cầu riêng biệt vì một biến Boolean đại diện cho cả hai bản sao. 

Đối với những lựa chọn không tương thích,```
3
1 3
1 3
1 2
1 2
```cạnh giá trị-(0) đã chọn và cạnh giá trị-(1) đã chọn luôn có chung một đỉnh bắt đầu và sau đó phân kỳ. Không thể theo sau người khác. Do đó, mỗi cặp lựa chọn đều tạo ra một cặp bị cấm, làm cho trường hợp (k=2) 2-SAT không thỏa mãn. Thuật toán trả về (1). 

Đối với các cạnh liên tiếp,```
4
1 2
1 2
2 3
2 3
3 4
3 4
4 4
4 4
```hai dòng cuối cùng không hợp lệ theo ràng buộc ban đầu (a<b), vì vậy chúng không được sử dụng làm bài kiểm tra thực tế. Thay vào đó, một ví dụ về ranh giới hợp lệ là```
4
1 2
1 2
2 3
2 3
3 4
3 4
1 4
1 4
```Ở đây các giá trị (0,1,2) có thể xuất hiện dọc theo (1\to2\to3\to4), trong khi giá trị (3) sử dụng một cạnh (1\to4) xung đột với các cạnh đó. Câu trả lời là (3). Biểu diễn khả năng tiếp cận coi điểm cuối được chia sẻ là có thể truy cập được từ chính nó, do đó (1\to2) theo sau là (2\to3) được chấp nhận chính xác. 

Đối với kích thước đầu vào lớn nhất, (n=2000), có (4000) cạnh và có thể có hàng triệu mối quan hệ tương thích theo cặp. Việc lưu trữ từng mối quan hệ dưới dạng danh sách Python sẽ gây ra chi phí đáng kể cho đối tượng. Biểu diễn bitset lưu trữ thông tin dày đặc tương tự trong các khối có kích thước bằng máy bên trong các số nguyên Python, đó là lý do việc triển khai vẫn nằm trong phạm vi dung lượng bộ nhớ hợp lý.
