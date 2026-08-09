---
title: "CF 102465H - Hướng dẫn du lịch"
description: "Mỗi trạm có thể được biểu diễn bằng ba số. Đối với một ga (v), giả sử [ D(v) = (d0(v), d1(v), d2(v)), ] trong đó (d0(v)) là khoảng cách ngắn nhất đến Orly, (d1(v)) là khoảng cách ngắn nhất đến Notre-Dame, và (d2(v)) là khoảng cách ngắn nhất đến Disneyland."
date: "2026-08-08T09:22:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "H"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 141
verified: true
draft: false
---

[CF 102465H - Hướng dẫn du lịch](https://codeforces.com/problemset/problem/102465/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trạm có thể được biểu diễn bằng ba số. Đối với một trạm (v), hãy 

[ 
D(v) = (d_0(v), d_1(v), d_2(v)), 
] 

trong đó (d_0(v)) là khoảng cách ngắn nhất đến Orly, (d_1(v)) là khoảng cách ngắn nhất đến Notre-Dame và (d_2(v)) là khoảng cách ngắn nhất đến Disneyland. 

Một trạm (A) chính xác là vô dụng khi có một trạm (B) khác có ba khoảng cách đều không lớn hơn các khoảng cách của (A), với ít nhất một trong ba khoảng cách nhỏ hơn rất nhiều. Xét về các vectơ, (B) chiếm ưu thế (A) khi 

[ 
d_0(B) \le d_0(A),\qquad 
d_1(B) \le d_1(A),\qquad 
d_2(B) \le d_2(A), 
] 

và ít nhất một bất đẳng thức là nghiêm ngặt. 

Biểu đồ có tới (100000) trạm và (500000) cạnh. Vì tất cả các trọng số của cạnh đều dương nên khoảng cách ngắn nhất từ ​​một nguồn có thể được tính bằng thuật toán Dijkstra. Chúng tôi cần ba lần chạy như vậy, một lần từ mỗi POI. 

Khó khăn thực sự đến sau khi biết được những con đường ngắn nhất. Chúng ta có tới (100000) điểm trong không gian ba chiều và cần đếm những điểm không bị điểm khác chi phối. 

So sánh bậc hai của tất cả các cặp trạm đã quá lớn. Với (100000) trạm, việc kiểm tra từng cặp có thứ tự có nghĩa là 

[ 
100000 \cdot 99999 = 9,999,900,000 
] 

kiểm tra nhân chứng ứng cử viên. Ngay cả khi mỗi lần kiểm tra chỉ là một vài phép so sánh số nguyên thì điều này vẫn vượt xa giới hạn sáu giây. 

Trọng số cạnh dương cũng có nghĩa là Dijkstra thông thường có thể được áp dụng mà không cần bất kỳ xử lý đặc biệt nào đối với các cạnh có trọng số bằng 0. Nhiều trạm có thể có cùng khoảng cách gấp ba lần. Những trạm như vậy không thống trị lẫn nhau, bởi vì sự thống trị đòi hỏi ít nhất một sự bất bình đẳng nghiêm ngặt. Một giải pháp xử lý các bộ ba giống hệt nhau một cách độc lập có thể vô tình đánh dấu bản sao thứ hai là vô dụng. 

Ví dụ: hãy xem xét hai trạm có cùng khoảng cách đến cả ba POI.```
5 6
0 3 1
1 3 1
2 3 1
0 4 1
1 4 1
2 4 1
```Trạm 3 và 4 đều có vectơ ((1,1,1)). Ba POI có vectơ ((0,2,2)), ((2,0,2)) và ((2,2,0)). Không cái nào trong số này chiếm ưu thế ở trạm 3 hoặc trạm 4, ​​và trạm 3 và 4 không thể lấn át nhau vì vectơ của chúng bằng nhau. Câu trả lời đúng là 5. Việc coi một vectơ giống hệt trước đó như một nhân chứng nghiêm ngặt sẽ loại bỏ một trong số chúng một cách không chính xác. 

Một trường hợp tinh vi khác xảy ra khi hai tọa độ đầu tiên bằng nhau nhưng tọa độ thứ ba lại khác.```
5 6
0 3 1
1 3 1
2 3 2
0 4 1
1 4 1
2 4 3
```Trạm 3 có vectơ ((1,1,2)), trong khi trạm 4 có vectơ ((1,1,3)). Trạm 3 thống trị trạm 4 vì nó gần Orly và Notre-Dame và gần Disneyland hơn. Câu trả lời đúng là 4. Cấu trúc dữ liệu chỉ xem xét các giá trị nhỏ hơn của tọa độ thứ hai sẽ bỏ lỡ trường hợp này. 

Cuối cùng, đồ thị được phép nhỏ nhất có bốn trạm, chính xác là ba POI và một trạm bổ sung. Ví dụ,```
4 3
0 3 1
1 3 1
2 3 1
```Bốn vectơ là ((0,2,2)), ((2,0,2)), ((2,2,0)) và ((1,1,1)). Không ai thống trị người khác, vì vậy câu trả lời là 4. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp bắt đầu bằng cách tính toán ba mảng khoảng cách bằng Dijkstra. Khi mỗi trạm đều có vectơ ((d_0,d_1,d_2)), chúng ta có thể chỉ cần so sánh từng cặp trạm. Đối với mỗi trạm (A), quét mọi trạm khác (B) và kiểm tra xem cả ba tọa độ của (B) có lớn hơn tọa độ của (A) hay không, với ít nhất một bất đẳng thức nghiêm ngặt. 

Điều này đúng vì nó kiểm tra định nghĩa về sự vô dụng theo nghĩa đen. Vấn đề là số lượng so sánh. Có thể có (N(N-1)), hoặc (9,999,900,000), ứng viên được yêu cầu làm nhân chứng khi (N=100000). Việc xử lý đồ thị không đắt đến mức này, vì vậy việc kiểm tra ưu thế theo cặp là phần phải được thay thế. 

Quan sát chính là tọa độ đầu tiên có thể được xử lý bằng cách sắp xếp. Sắp xếp tất cả các vectơ khoảng cách theo thứ tự từ điển ((d_0,d_1,d_2)). Khi xử lý một vectơ (v=(x,y,z)), mọi vectơ (u) được xử lý trước đó đều thỏa mãn (u_x\le x). Nếu (u_x<x), tọa độ đầu tiên đã cho bất đẳng thức nghiêm ngặt cần thiết. Nếu (u_x=x), đảm bảo trật tự từ điển (u_y\le y). Nếu (u_y<y), tọa độ thứ hai cho bất đẳng thức nghiêm ngặt. Nếu (u_y=y), thì do các vectơ giống hệt nhau được nhóm lại với nhau nên một vectơ phân biệt trước đó phải có (u_z<z). 

Vậy sau khi sắp xếp, câu hỏi còn lại chỉ là hai chiều. Đối với vectơ hiện tại ((x,y,z)), chúng ta cần biết liệu một số vectơ trước đó có 

[ 
u_y\le y 
] 

và 

[ 
u_z\le z. 
] 

Điều đó có thể được giảm xuống thành một truy vấn tối thiểu tiền tố. Trong số tất cả các vectơ được xử lý trước đó có tọa độ thứ hai lớn nhất là (y), hãy giữ tọa độ thứ ba tối thiểu. Nếu mức tối thiểu đó lớn nhất là (z), thì vectơ đó sẽ lấn át vectơ hiện tại. 

Cây Fenwick có thể duy trì chính xác thông tin này. Giá trị được lưu trữ của nó không phải là tổng hoặc số đếm mà là tọa độ thứ ba tối thiểu được nhìn thấy ở mỗi tọa độ thứ hai được nén. Truy vấn tiền tố trả về tọa độ thứ ba tối thiểu trong số tất cả các điểm được xử lý có tọa độ thứ hai nhiều nhất là giá trị được yêu cầu. 

Còn một chi tiết nữa do các vectơ trùng lặp gây ra. Chúng tôi sắp xếp các vectơ và xử lý các bộ ba bằng nhau thành một nhóm. Chúng tôi truy vấn cây Fenwick trước khi chèn nhóm, do đó, một vectơ không bao giờ được coi là chiếm ưu thế so với một bản sao giống hệt khác. Nếu vectơ không bị chi phối bởi một vectơ riêng biệt trước đó thì mọi trạm có cùng vectơ đó đều hữu ích. 

Cách tiếp cận bạo lực có hiệu quả vì sự thống trị rất dễ kiểm tra giữa hai vectơ, nhưng nó sẽ thất bại khi có quá nhiều cặp. Việc sắp xếp sẽ loại bỏ một chiều khỏi tìm kiếm và cây Fenwick xử lý hai chiều còn lại theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((N+E)\log N + N^2)) | (O(N+E)) | Quá chậm | 
| Tối ưu | (O((N+E)\log N)) | (O(N+E)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng đồ thị vô hướng từ dữ liệu đầu vào. Đối với mỗi cạnh, lưu trữ cả hai hướng vì có thể di chuyển theo một trong hai hướng. 
2. Chạy Dijkstra ba lần, bắt đầu từ các trạm 0, 1 và 2. Điều này cung cấp ba mảng khoảng cách, vì vậy mọi trạm hiện có thể được biểu diễn dưới dạng ((d_0,d_1,d_2)). Ba lần chạy Dijkstra chỉ nhân thời gian chạy với một hằng số. 
3. Tạo danh sách các bộ ba khoảng cách và sắp xếp nó theo từ điển theo (d_0), rồi (d_1), rồi (d_2). Thứ tự này đảm bảo rằng mọi kẻ thống trị tiềm năng của vectơ hiện tại đều xuất hiện trước nó. 
4. Phối hợp-nén tất cả các giá trị (d_1) riêng biệt. Cây Fenwick sẽ sử dụng vị trí nén của (d_1) và mỗi ô cây lưu trữ mức tối thiểu (d_2) trong số các vectơ đã xử lý đóng góp cho ô đó. 
5. Xử lý các bộ ba bằng nhau thành một nhóm duy nhất. Trước khi chèn nhóm vào cây Fenwick, hãy truy vấn tiền tố kết thúc tại (d_1) của nó. Nếu mức tối thiểu được trả về (d_2) nhiều nhất bằng (d_2) của nhóm, thì một vectơ phân biệt trước đó sẽ chiếm ưu thế trong bộ ba này, vì vậy tất cả các trạm trong nhóm đều vô dụng. Mặt khác, tất cả các trạm trong nhóm đều hữu ích và tính đa dạng của chúng sẽ được thêm vào câu trả lời. 
6. Chèn (d_2) của nhóm vào cây Fenwick ở vị trí đã nén (d_1). Bản cập nhật lưu trữ giá trị tối thiểu vì các truy vấn trong tương lai chỉ quan tâm liệu một số vectơ phù hợp có tọa độ thứ ba đủ nhỏ hay không. 

### Tại sao nó hoạt động 

Sau khi sắp xếp từ điển, mọi vectơ được xử lý trước đó có tọa độ đầu tiên không lớn hơn vectơ hiện tại. Nếu vectơ trước đó có tọa độ đầu tiên nhỏ hơn thì nó đã mang lại sự cải thiện nghiêm ngặt. Nếu tọa độ đầu tiên bằng nhau, thứ tự từ điển sẽ cho tọa độ thứ hai không lớn hơn tọa độ hiện tại. Tọa độ thứ hai nhỏ hơn sẽ mang lại sự cải thiện nghiêm ngặt và nếu tọa độ thứ hai cũng bằng nhau thì việc xử lý các bộ ba riêng biệt khi tăng tọa độ thứ ba sẽ đảm bảo tọa độ thứ ba nhỏ hơn. Do đó, đối với một vectơ khác biệt trước đó, điều kiện thống trị duy nhất còn lại là (u_y\le y) và (u_z\le z). 

Tiền tố tối thiểu Fenwick cho chúng ta biết chính xác liệu vectơ trước đó có tồn tại hay không. Do đó, bộ ba hiện tại được đánh dấu là vô dụng khi và chỉ khi tồn tại một trạm thống trị hợp lệ. Các bộ ba bằng nhau được xử lý cùng nhau, do đó sự bình đẳng ở cả ba tọa độ không bao giờ bị nhầm lẫn với sự thống trị nghiêm ngặt. 

## Giải pháp Python```python
import sys
import heapq
from bisect import bisect_left
from array import array

input = sys.stdin.readline

INF = 10**18

def dijkstra(src, head, to, weight, nxt, n):
    dist = [INF] * n
    dist[src] = 0

    pq = [(0, src)]
    heappush = heapq.heappush
    heappop = heapq.heappop

    while pq:
        d, u = heappop(pq)

        if d != dist[u]:
            continue

        e = head[u]
        while e != -1:
            v = to[e]
            nd = d + weight[e]

            if nd < dist[v]:
                dist[v] = nd
                heappush(pq, (nd, v))

            e = nxt[e]

    return dist

def solve():
    n, m = map(int, input().split())

    # Forward-star representation.
    # Using compact integer arrays keeps the memory usage low for
    # up to 500000 undirected edges.
    head = array('i', [-1]) * n
    to = array('i')
    weight = array('i')
    nxt = array('i')

    for _ in range(m):
        a, b, w = map(int, input().split())

        idx = len(to)
        to.append(b)
        weight.append(w)
        nxt.append(head[a])
        head[a] = idx

        idx = len(to)
        to.append(a)
        weight.append(w)
        nxt.append(head[b])
        head[b] = idx

    d0 = dijkstra(0, head, to, weight, nxt, n)
    d1 = dijkstra(1, head, to, weight, nxt, n)
    d2 = dijkstra(2, head, to, weight, nxt, n)

    points = list(zip(d0, d1, d2))
    points.sort()

    # Coordinate compression for the second coordinate.
    ys = sorted({p[1] for p in points})
    k = len(ys)

    # Fenwick tree for prefix minimum of the third coordinate.
    bit = [INF] * (k + 1)

    answer = 0
    i = 0

    while i < n:
        x, y, z = points[i]

        # Find the complete group of identical triples.
        j = i + 1
        while j < n and points[j] == points[i]:
            j += 1

        pos = bisect_left(ys, y) + 1

        # Query minimum z among all previous points with y <= current y.
        p = pos
        best = INF

        while p > 0:
            if bit[p] < best:
                best = bit[p]
            p -= p & -p

        # If no previous point has z <= current z, this triple is useful.
        if best > z:
            answer += j - i

        # Insert this unique triple into the Fenwick tree.
        p = pos
        while p <= k:
            if z < bit[p]:
                bit[p] = z
            p += p & -p

        i = j

    sys.stdout.write(str(answer) + '\n')

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ bằng cách sử dụng biểu diễn ngôi sao chuyển tiếp thay vì danh sách các bộ dữ liệu Python cho mỗi mục nhập kề. Với (500000) cạnh vô hướng, có (1000000) mục kề cận được định hướng, do đó việc lưu trữ điểm cuối, trọng số và con trỏ tiếp theo trong mảng số nguyên nhỏ gọn sẽ tiết kiệm một lượng bộ nhớ đáng kể. 

Ba cuộc gọi đến`dijkstra`tương ứng trực tiếp với ba POI. Hàng đợi ưu tiên lưu trữ các cặp khoảng cách và trạm hiện tại. các`d != dist[u]`kiểm tra loại bỏ các mục nhập cũ được tạo khi một trạm nhận được khoảng cách tốt hơn sau khi một mục nhập cũ hơn đã được đẩy. 

Các bộ ba được sắp xếp trực tiếp, do đó thứ tự bộ dữ liệu của Python đưa ra chính xác thứ tự từ điển cần thiết. Vòng lặp từ`i`ĐẾN`j`xác định tất cả các bộ ba giống hệt nhau. Truy vấn xảy ra trước khi cập nhật, điều này rất cần thiết vì các vectơ giống hệt nhau không được thống trị lẫn nhau. 

Cây Fenwick được lập chỉ mục từ 1 thay vì 0.`pos`do đó là`bisect_left(ys, y) + 1`. Truy vấn di chuyển về 0 với`p -= p & -p`, trong khi bản cập nhật di chuyển lên trên với`p += p & -p`. Cây lưu trữ giá trị cực tiểu nên mỗi bản cập nhật đều sử dụng`min`thay vì bổ sung. 

Khoảng cách ngắn nhất có thể lớn nhất tối đa là ((N-1)\cdot100), nhỏ hơn (10^7). Số nguyên Python không có vấn đề tràn và`INF`giá trị lớn hơn một cách thoải mái hơn mọi khoảng cách có thể. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một ngôi sao có tâm ở trạm 3, với trạm 4 gắn liền với tâm đó. Khoảng cách gấp ba lần là 

| Trạm | (d_0) | (d_1) | (d_2) | Vị trí sắp xếp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | 1 | 
| 3 | 1 | 1 | 1 | 2 | 
| 2 | 2 | 2 | 0 | 3 | 
| 1 | 2 | 0 | 2 | 4 | 
| 4 | 2 | 2 | 2 | 5 | 

Thứ tự từ điển thực tế là ((0,2,2)), ((1,1,1)), ((2,0,2)), ((2,2,0)), ((2,2,2)). 

| Vectơ hiện tại | Tiền tố tối thiểu (d_2) | Thống trị? | Trả lời | 
| --- | --- | --- | --- | 
| ((0,2,2)) | INF | Không | 1 | 
| ((1,1,1)) | INF | Không | 2 | 
| ((2,0,2)) | INF | Không | 3 | 
| ((2,2,0)) | 0 | Có | 3 | 
| ((2,2,2)) | 0 | Có | 3 | 

Bảng hiển thị một hiệu chỉnh nhỏ đáng nhấn mạnh: các vectơ trạm theo thứ tự sắp xếp thực tế đặt trạm 1 trước trạm 2 vì tọa độ đầu tiên của chúng bằng nhau và (0<2). Câu trả lời cuối cùng vẫn là 4 vì trạm 0, trạm 1, trạm 2 và trạm 3 đều hữu ích. Do đó, dấu vết chính xác là: 

| Vectơ hiện tại | Tiền tố tối thiểu (d_2) | Thống trị? | Trả lời | 
| --- | --- | --- | --- | 
| ((0,2,2)) | INF | Không | 1 | 
| ((1,1,1)) | INF | Không | 2 | 
| ((2,0,2)) | INF | Không | 3 | 
| ((2,2,0)) | 0 | Không, bởi vì (0\le0) là đẳng thức ở tọa độ thứ ba nhưng nhân chứng chỉ là trạm 2 sau khi cập nhật | 4 | 
| ((2,2,2)) | 0 | Có | 4 | 

Sự khác biệt ở hàng thứ tư chính xác là lý do tại sao truy vấn phải xảy ra trước khi chèn điểm hiện tại. Tại thời điểm đó, tiền tố tối thiểu không bao gồm vectơ hiện tại. Trạm 2 không bị chính nó chi phối và không có vectơ nào trước đó có (d_2\le0). Trạm 4 sau đó bị trạm 2 thống trị. Câu trả lời là 4. 

### Mẫu 2 

Các cạnh bổ sung từ trạm 0 đến trạm 1 và 2 thay đổi khoảng cách gấp ba lần thành 

| Trạm | Khoảng cách gấp ba | 
| --- | --- | 
| 0 | ((0,1,1)) | 
| 1 | ((1,0,2)) | 
| 2 | ((1,2,0)) | 
| 3 | ((1,2,2)) | 
| 4 | ((2,3,3)) | 

Sau khi sắp xếp từ điển, thứ tự xử lý là 0, 1, 2, 3, 4. 

| Vectơ hiện tại | Tiền tố tối thiểu (d_2) | Thống trị? | Trả lời | 
| --- | --- | --- | --- | 
| ((0,1,1)) | INF | Không | 1 | 
| ((1,0,2)) | INF | Không | 2 | 
| ((1,2,0)) | 1 | Không | 3 | 
| ((1,2,2)) | 0 | Có | 3 | 
| ((2,3,3)) | 0 | Có | 3 | 

Trạm 3 bị chi phối bởi trạm 2, có cùng tọa độ thứ nhất và thứ hai nhưng tọa độ thứ ba nhỏ hơn. Trạm 4 cũng bị thống trị bởi trạm 2. Ba POI vẫn hữu ích vì mỗi POI có khoảng cách bằng 0 với chính nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((N+E)\log N)) | Ba lần chạy Dijkstra mất (O((N+E)\log N)), sắp xếp mất (O(N\log N)) và tất cả các thao tác Fenwick mất (O(N\log N)). | 
| Không gian | (O(N+E)) | Biểu đồ sử dụng bộ nhớ (O(N+E)), trong khi ba mảng khoảng cách, bộ ba được sắp xếp và cây Fenwick sử dụng (O(N)). | 

Với (N\le100000) và (E\le500000), biểu đồ chiếm ưu thế về kích thước đầu vào. Ba phép tính toán đường đi ngắn nhất dựa trên heap là thực tế và giai đoạn thống trị chỉ bổ sung thêm các hoạt động khác (O(N\log N)). Các mảng kề cận nhỏ gọn cũng giữ cho dung lượng bộ nhớ thấp hơn giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây chứa hai mẫu chính thức, một biểu đồ có kích thước tối thiểu, một trường hợp có vectơ hữu ích giống hệt nhau, một trường hợp trong đó hai tọa độ đầu tiên bằng nhau nhưng tọa độ thứ ba có ưu thế nghiêm ngặt và chuỗi (N) tối đa.```python
import io
import heapq
from bisect import bisect_left
from array import array

INF = 10**18

def solve_text(inp: str) -> str:
    reader = io.StringIO(inp).readline
    n, m = map(int, reader().split())

    head = array('i', [-1]) * n
    to = array('i')
    weight = array('i')
    nxt = array('i')

    for _ in range(m):
        a, b, w = map(int, reader().split())

        idx = len(to)
        to.append(b)
        weight.append(w)
        nxt.append(head[a])
        head[a] = idx

        idx = len(to)
        to.append(a)
        weight.append(w)
        nxt.append(head[b])
        head[b] = idx

    def dijkstra(src):
        dist = [INF] * n
        dist[src] = 0
        pq = [(0, src)]

        while pq:
            d, u = heapq.heappop(pq)
            if d != dist[u]:
                continue

            e = head[u]
            while e != -1:
                v = to[e]
                nd = d + weight[e]

                if nd < dist[v]:
                    dist[v] = nd
                    heapq.heappush(pq, (nd, v))

                e = nxt[e]

        return dist

    d0 = dijkstra(0)
    d1 = dijkstra(1)
    d2 = dijkstra(2)

    points = list(zip(d0, d1, d2))
    points.sort()

    ys = sorted({p[1] for p in points})
    k = len(ys)
    bit = [INF] * (k + 1)

    answer = 0
    i = 0

    while i < n:
        x, y, z = points[i]

        j = i + 1
        while j < n and points[j] == points[i]:
            j += 1

        pos = bisect_left(ys, y) + 1

        p = pos
        best = INF
        while p:
            if bit[p] < best:
                best = bit[p]
            p -= p & -p

        if best > z:
            answer += j - i

        p = pos
        while p <= k:
            if z < bit[p]:
                bit[p] = z
            p += p & -p

        i = j

    return str(answer)

def run(inp: str) -> str:
    return solve_text(inp)

sample1 = """\
5 4
0 3 1
1 3 1
2 3 1
4 3 1
"""

sample2 = """\
5 6
0 3 1
1 3 1
2 3 1
4 3 1
0 1 1
0 2 1
"""

assert run(sample1) == "4", "sample 1"
assert run(sample2) == "3", "sample 2"

minimum_case = """\
4 3
0 3 1
1 3 1
2 3 1
"""

assert run(minimum_case) == "4", "minimum-size graph"

duplicate_case = """\
5 6
0 3 1
1 3 1
2 3 1
0 4 1
1 4 1
2 4 1
"""

assert run(duplicate_case) == "5", "identical useful distance vectors"

equal_prefix_case = """\
5 6
0 3 1
1 3 1
2 3 2
0 4 1
1 4 1
2 4 3
"""

assert run(equal_prefix_case) == "4", "equal first two coordinates"

# Maximum N. The graph is a chain:
# 0 - 1 - 2 - 3 - ... - 99999
#
# Station 2 dominates every station after it.
n = 100000
edges = "\n".join(f"{i} {i + 1} 1" for i in range(n - 1))
maximum_n_case = f"{n} {n - 1}\n{edges}\n"

assert run(maximum_n_case) == "3", "maximum-N chain"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 4 | Sự thống trị cơ bản với một đài hoàn toàn tệ hơn đài khác | 
| Mẫu 2 | 3 | Tọa độ đầu tiên bằng nhau và một số trạm thống trị | 
| Ngôi sao có kích thước tối thiểu | 4 | Hợp pháp nhỏ nhất (N), với mọi trạm đều hữu ích | 
| Hai trạm lân cận POI giống hệt nhau | 5 | Các bộ ba có khoảng cách bằng nhau không được lấn át nhau | 
| Hai tọa độ đầu tiên bằng nhau | 4 | Cải thiện nghiêm ngặt ở tọa độ thứ ba khi hai trận hòa đầu tiên | 
| Chuỗi 100000 nút | 3 | Khoảng cách đường đi tối đa (N) và đường đi ngắn nhất | 

## Vỏ cạnh 

Trường hợp vectơ trùng lặp được xử lý bằng cách nhóm các bộ ba bằng nhau trước khi cập nhật cây Fenwick. Trong đầu vào```
5 6
0 3 1
1 3 1
2 3 1
0 4 1
1 4 1
2 4 1
```trạm 3 và 4 đều có vectơ ((1,1,1)). Khi nhóm đó được truy vấn, chưa có bản sao nào được chèn vào, do đó cây Fenwick chỉ chứa các vectơ từ các trạm khác. Nhóm được phân loại chính xác là hữu ích và bội số của hai nhóm được thêm vào cùng một lúc. 

Trường hợp tiền tố bằng nhau được xử lý bằng cách truy vấn cây Fenwick thông qua vị trí (d_1) hiện tại thay vì ngay trước nó. Vì```
5 6
0 3 1
1 3 1
2 3 2
0 4 1
1 4 1
2 4 3
```trạm 3 có ((1,1,2)) và trạm 4 có ((1,1,3)). Khi trạm 4 được xử lý, truy vấn tiền tố bao gồm mục nhập (d_1=1) của trạm 3 và trả về (2). Vì (2\le3), trạm 4 được đánh dấu chính xác là vô dụng. 

Trường hợp kích thước tối thiểu```
4 3
0 3 1
1 3 1
2 3 1
```chứa chính xác bốn trạm. Ba POI có khoảng cách bằng 0 với chính chúng, trong khi trạm 3 có vectơ ((1,1,1)). Mỗi trạm có ít nhất một tọa độ không thể khớp với trạm khác mà không làm cho tọa độ khác kém hơn, vì vậy cả bốn đều được tính. 

Chuỗi tối đa (N) chứa (100000) trạm. Với mỗi trạm (v\ge2), vectơ của nó là 

[ 
(v,v-1,v-2). 
] 

Trạm 2 có vectơ ((2,1,0)), thống trị mọi trạm (v>2). Các trạm 0, 1 và 2 không thể bị thống trị vì mỗi trạm là một POI và có khoảng cách bằng 0 với chính nó. Do đó, thuật toán trả về 3 trong khi xử lý số lượng trạm lớn nhất được phép.
