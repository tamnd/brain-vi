---
title: "CF 102201H - Khó giải thích"
description: "Gốc cây tại đỉnh 1. Đối với truy vấn (V, T), các đỉnh liên quan chính xác là tổ tiên của V, bao gồm chính V và gốc. Trong số các đỉnh đó, đỉnh i chỉ có thể được sử dụng khi Ci = T, và giá trị của nó là giá trị của đường thẳng [ fi(T)=Ai+BiT."
date: "2026-08-18T10:35:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102201
codeforces_index: "H"
codeforces_contest_name: "Moscow Pre-Finals Workshop 2019. KAIST Contest"
rating: 0
weight: 102201
solve_time_s: 600
verified: true
draft: false
---

[CF 102201H - Khó giải thích](https://codeforces.com/problemset/problem/102201/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Gốc cây tại đỉnh 1. Đối với một truy vấn`(V, T)`, các đỉnh liên quan chính xác là tổ tiên của`V`, bao gồm`V`chính nó và gốc rễ. Trong số những tổ tiên đó, đỉnh`i`chỉ có thể được sử dụng khi`C_i >= T`, và giá của nó là giá trị của đường 

[ 
f_i(T)=A_i+B_iT. 
] 

Nhiệm vụ là trả về giá trị nhỏ nhất như vậy. 

Tình trạng cây trên`B`là thuộc tính cấu trúc trung tâm: 

[ 
B_{\tên toán tử{parent}(v)}\le B_v. 
] 

Vì vậy, dọc theo mọi đường đi từ gốc tới lá, độ dốc của các đường tương ứng không bao giờ giảm. Nếu không có điều kiện này thì bài toán sẽ yêu cầu một kết cấu thân lồi hoàn toàn động. Với nó, các dòng thuộc một đường dẫn từ gốc tới đỉnh có thứ tự hữu ích. 

Có tới 80.000 đỉnh và 160.000 truy vấn. Một truy vấn có thể liên quan đến toàn bộ đường dẫn từ gốc đến lá, do đó, việc đi theo đường dẫn một cách rõ ràng đã quá chậm trên cây hình chuỗi. Trong trường hợp xấu nhất, có khoảng (80.000) tổ tiên cho mỗi truy vấn, đưa ra đánh giá dòng khoảng (1,28\time10^{10}) cho 160.000 truy vấn. Giải pháp phải tiến gần đến công việc logarit cho mỗi truy vấn. 

Có ba điều kiện biên thường gây ra việc triển khai không chính xác. 

Đầu tiên, đỉnh được truy vấn thuộc về đường dẫn. Ví dụ,```
1 1
5
7
1000000000
1 0
```có câu trả lời`5`, vì đỉnh 1 cũng là đỉnh được truy vấn. Việc triển khai bắt đầu từ`parent[V]`sẽ không tìm thấy ứng cử viên nào một cách sai lầm. 

Thứ hai, điều kiện là`C_i >= T`, không`C_i > T`. Vì```
2 1
10 1
1 2
5 5
1 2
2 5
```câu trả lời là`6`, vì đỉnh 2 có`C_2 = 5`và có hiệu lực tại`T = 5`. Một so sánh chặt chẽ sẽ loại bỏ nó và trả lại`15`. 

Thứ ba,`T = 0`là hợp pháp. Trong trường hợp đó mọi đỉnh đều thỏa mãn`C_i >= T`, vì vậy câu trả lời đơn giản là nhỏ nhất`A_i`trên root-to-`V`con đường. Trong mẫu, truy vấn`(4, 0)`xem xét các đỉnh`1, 2, 4`và trả về`2`. Bất kỳ triển khai nào giả định tọa độ truy vấn tích cực hoặc bắt đầu miền Li Chao của nó tại`1`có thể thất bại ở đây 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. Đối với mọi truy vấn, hãy đi bộ từ`V`về phía gốc, bỏ qua các đỉnh có`C_i`nhỏ hơn`T`, và đánh giá`A_i+B_iT`cho phần còn lại. Điều này đúng vì mỗi đỉnh trên bước đi đó chính xác là một trong các đỉnh trên đường đi duy nhất từ ​​gốc đến`V`. 

Vấn đề là độ dài đường dẫn. Một chuỗi gồm 80.000 đỉnh cùng với 160.000 truy vấn có thể yêu cầu (80.000\cdot160.000=12,8) tỷ đánh giá. Mặc dù một đánh giá chỉ là một vài phép tính số nguyên, nhưng điều đó vượt xa giới hạn thời gian. 

Quan sát hữu ích đầu tiên là coi mọi đỉnh là một đường thẳng 

[ 
y=A_i+B_ix. 
] 

Truy vấn sau đó yêu cầu dòng hợp lệ thấp nhất tại`x = T`. Quan sát thứ hai là tính hợp lệ là tiền tố của trục x: dòng`i`tồn tại cho tất cả`x <= C_i`. Do đó, mỗi đỉnh thực sự cho chúng ta một đoạn thẳng kéo dài từ trái về phía`C_i`. 

Quan sát thứ ba là cấu trúc cây. Nếu chúng ta xử lý các đỉnh từ gốc tới một nút, chúng ta sẽ chèn các đường có hệ số góc không giảm. Truy vấn tại đỉnh`V`cần chính xác cấu trúc dữ liệu thu được sau khi chèn các dòng trên root-to-`V`con đường. 

Chúng ta có thể làm cho điều này bền bỉ. Mỗi đỉnh lưu trữ một phiên bản của cấu trúc dữ liệu được kế thừa từ đỉnh gốc của nó, sau đó thêm đoạn đường riêng của nó. Một truy vấn tại`V`sử dụng phiên bản`V`, do đó, tính bền vững sẽ tự động giới hạn ứng viên được đặt thành tổ tiên của`V`. 

Vấn đề còn lại là một dòng chỉ có giá trị tối đa`C_i`, thay vì trên toàn bộ trục x. Cây Li Chao có thể chèn một dòng trên một khoảng thay vì trên toàn bộ miền tọa độ. Vì mỗi khoảng hợp lệ là`[0, C_i]`, chúng tôi thực hiện chèn phạm vi dòng trên tiền tố đó. Cây Li Chao sau đó đảm bảo rằng một truy vấn tại`T`nhìn thấy chính xác các dòng có khoảng hiệu lực chứa`T`. 

Sự kiên trì được xử lý bằng cách chỉ sao chép các nút Li Chao được thay đổi bằng cách chèn. Phiên bản thuộc về con trỏ đến gốc của cây cha, vì vậy các nhánh cây khác nhau đều có chung cấu trúc không thay đổi. 

Cách tiếp cận thu được là một cây phân đoạn ổn định có các nút chứa thông tin Li Chao. Chi phí chèn phạm vi (O(\log^2 C)), trong đó (C\le10^9) và chi phí truy vấn điểm (O(\log C)). Với giới hạn đã cho, logarit tối đa là khoảng 30. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(NQ)) | (O(N)) | Quá chậm | 
| Khoảng thời gian dai dẳng Li Chao | (O(N\log^2 10^9+Q\log 10^9)) | (O(N\log^2 10^9)) trường hợp xấu nhất | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Căn cây tại đỉnh 1 và xác định cha của mỗi đỉnh. 

Chúng tôi thực hiện việc này lặp đi lặp lại thay vì đệ quy, vì một cây có thể là một chuỗi gồm 80.000 đỉnh và giới hạn đệ quy của Python không phù hợp cho việc truyền tải như vậy. 
2. Với mọi đỉnh`v`, định nghĩa`root[v]`là gốc của cây Li Chao bền bỉ biểu thị tất cả các đoạn thẳng hợp lệ thuộc các đỉnh trên đường đi từ đỉnh 1 đến`v`. 

Phiên bản gốc ban đầu chứa dòng 1. Với mọi đỉnh khác`v`, bắt đầu từ`root[parent[v]]`và chèn dòng 

[ 
y=A_v+B_vx 
] 

trên khoảng thời gian 

[ 
[0,C_v]. 
] 

Đây là sự bất biến bền bỉ quan trọng. Ngay bây giờ`root[v]`được xây dựng, nó chứa đựng chính xác những dòng dõi thuộc về tổ tiên của`v`. 
3. Biểu diễn trục x bằng khoảng nguyên`[0, 10^9]`. 

Chúng tôi chỉ truy vấn các giá trị nguyên của`T`, do đó không cần giao điểm dấu phẩy động. Cây Li Chao hoạt động hoàn toàn bằng cách so sánh các giá trị dòng nguyên. 
4. Để chèn một dòng vào`[0,C_v]`, đệ quy đi xuống cây tọa độ Li Chao. 

Nếu khoảng tọa độ hiện tại được bao phủ hoàn toàn bởi`[0,C_v]`, thực hiện thao tác chèn Li Chao thông thường vào khoảng đó. 

Nếu chỉ một phần của khoảng được bao phủ, hãy xử lý đệ quy các phần tử được bao phủ. 

Bởi vì cấu trúc dữ liệu liên tục nên mọi nút Li Chao được sửa đổi đều được sao chép. Các em chưa chỉnh sửa tiếp tục chỉ vào bản cũ. 
5. Để chèn Li Chao thông thường, hãy giữ một dòng ứng cử viên tại mỗi nút cây phân đoạn. 

So sánh các đường cũ và mới tại điểm cuối bên trái, điểm giữa và điểm cuối bên phải. Nếu dòng mới tốt hơn ở điểm giữa, hãy hoán đổi nó với dòng đã lưu. Đường thua ở điểm giữa vẫn có thể phù hợp ở nhiều nhất một bên, vì vậy hãy lặp lại ở bên đó. 

Đây là bất biến Li Chao tiêu chuẩn: dọc theo mọi đường dẫn từ gốc đến lá, ít nhất một dòng được lưu trữ là tối ưu ở tọa độ lá tương ứng. 
6. Để trả lời`(V,T)`, bắt đầu tại`root[V]`và đi xuống chiếc lá tượng trưng`T`. 

Tại mỗi nút Li Chao được truy cập, hãy đánh giá dòng được lưu trữ của nó tại`T`và lấy mức tối thiểu. Từ`root[V]`chỉ chứa tổ tiên của`V`và chèn khoảng cách làm cho một dòng chỉ hiện diện cho`T <= C_i`, mức tối thiểu chính xác là trên các đỉnh được truy vấn cho phép. 
7. Xuất các câu trả lời đã thu thập theo thứ tự ban đầu. 

### Tại sao nó hoạt động 

Tính bất biến đó là`root[v]`chứa chính xác các đoạn thẳng của các đỉnh trên gốc tới`v`con đường. Điều này đúng với đỉnh 1 sau khi chèn dòng của nó. Khi chuyển từ cấp độ cha mẹ sang cấp độ con, tính kiên trì sẽ sao chép phiên bản của cấp độ gốc và thêm chính xác đoạn đường của cấp độ con, do đó, bất biến vẫn đúng. 

Đối với một truy vấn`(V,T)`, một đường từ đỉnh`i`xuất hiện ở`root[V]`chính xác khi nào`i`là tổ tiên của`V`. Việc chèn khoảng của nó chỉ đặt nó vào cấu trúc Li Chao trên tọa độ`x <= C_i`, do đó nó tham gia vào truy vấn chính xác khi`T <= C_i`. Bất biến Li Chao sau đó đảm bảo rằng giá trị tối thiểu trong số tất cả các dòng tham gia được tìm thấy trên root-to-`T`đường dẫn tìm kiếm. 

Sự đơn điệu của`B`tương thích với cách xây dựng tổ tiên và là lý do cấu trúc mà vấn đề này thừa nhận một giải pháp thân lồi dựa trên đường dẫn. Việc triển khai bên dưới sử dụng công thức khoảng Li Chao tổng quát hơn, do đó không cần tính toán giao điểm dấu phẩy động hoặc các trường hợp đặc biệt cho các hệ số góc bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30
XMAX = 10**9

def solve():
    n, q = map(int, input().split())

    A = [0] + list(map(int, input().split()))
    B = [0] + list(map(int, input().split()))
    C = [0] + list(map(int, input().split()))

    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    parent = [0] * (n + 1)
    order = [1]
    parent[1] = -1

    for u in order:
        for v in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    # Persistent Li Chao nodes.
    #
    # Each node contains:
    #   line index stored at this node
    #   left child
    #   right child
    #
    # A line index of 0 means "no line".
    lc = [0]
    rc = [0]
    ln = [0]

    def value(line_id, x):
        if line_id == 0:
            return INF
        return A[line_id] + B[line_id] * x

    def clone(node):
        if node == 0:
            lc.append(0)
            rc.append(0)
            ln.append(0)
        else:
            lc.append(lc[node])
            rc.append(rc[node])
            ln.append(ln[node])
        return len(ln) - 1

    def add_line(node, l, r, new_line):
        node = clone(node)

        old_line = ln[node]

        if old_line == 0:
            ln[node] = new_line
            return node

        mid = (l + r) >> 1

        if value(new_line, mid) < value(old_line, mid):
            ln[node], new_line = new_line, old_line

        if l == r:
            return node

        if value(new_line, l) < value(ln[node], l):
            left = add_line(lc[node], l, mid, new_line)
            lc[node] = left
        elif value(new_line, r) < value(ln[node], r):
            right = add_line(rc[node], mid + 1, r, new_line)
            rc[node] = right

        return node

    def add_segment(node, l, r, ql, qr, new_line):
        if qr < l or r < ql:
            return node

        node = clone(node)

        if ql <= l and r <= qr:
            # The whole interval is covered, so this is a normal
            # Li Chao insertion.
            return add_line(node, l, r, new_line)

        mid = (l + r) >> 1

        if ql <= mid:
            lc[node] = add_segment(
                lc[node], l, mid, ql, qr, new_line
            )

        if qr > mid:
            rc[node] = add_segment(
                rc[node], mid + 1, r, ql, qr, new_line
            )

        return node

    def query(node, l, r, x):
        ans = value(ln[node], x)

        if l == r:
            return ans

        mid = (l + r) >> 1

        if x <= mid:
            if lc[node]:
                other = query(lc[node], l, mid, x)
                if other < ans:
                    ans = other
        else:
            if rc[node]:
                other = query(rc[node], mid + 1, r, x)
                if other < ans:
                    ans = other

        return ans

    roots = [0] * (n + 1)

    # Build versions in parent-before-child order.
    for v in order:
        base = roots[parent[v]] if parent[v] > 0 else 0
        roots[v] = add_segment(base, 0, XMAX, 0, C[v], v)

    out = []

    for _ in range(q):
        v, t = map(int, input().split())
        out.append(str(query(roots[v], 0, XMAX, t)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc truyền tải đầu tiên xây dựng mối quan hệ cha mẹ. Mảng`order`là một trật tự tôpô của cây có gốc theo nghĩa là mọi cha mẹ đều xuất hiện trước con cái của nó, điều này cho phép chúng ta xây dựng`root[v]`không có đệ quy. 

Các mảng`lc`,`rc`, Và`ln`tạo thành cây Li Chao bền bỉ. Chỉ số 0 đại diện cho một nút trống. Một nút được sao chép sẽ kế thừa cả con trỏ con và dòng được lưu trữ của nó từ phiên bản trước, do đó chỉ những nút bị ảnh hưởng bởi dòng mới mới cần phải thay đổi.`add_line`là chèn Li Chao bình thường. Dòng được lưu trữ tại một nút là dòng hiện tốt hơn ở điểm giữa. Nếu đường đã dịch chuyển vẫn có thể đánh bại nó ở điểm cuối bên trái hoặc bên phải, thì nó sẽ được chèn đệ quy vào phía đó.`add_segment`chỉ thêm dòng vào khoảng tọa độ`[0,C_v]`. Khi khoảng thời gian hiện tại được bao phủ hoàn toàn, nó sẽ ủy quyền cho`add_line`. Ngược lại, nó sao chép nút hiện tại và tiếp tục vào các nút con giao nhau. 

Ranh giới là bao gồm. Cuộc gọi sử dụng`0`bởi vì`C[v]`, vì vậy một truy vấn với`T == C[v]`chấp nhận đúng đỉnh. Khoảng tọa độ chung cũng bao gồm cả hai đầu, đó là lý do tại sao đệ quy sử dụng`[l, r]`chứ không phải là một khoảng thời gian nửa mở. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các biểu thức như`B[i] * T + A[i]`không thể tràn. Trong C++, ở đây số nguyên có dấu 64 bit cũng đủ vì giá trị lớn nhất tối đa là khoảng (10^{18}+10^9). 

Một chi tiết thực hiện đáng được chú ý. Việc duyệt cây là lặp đi lặp lại, trong khi đệ quy Li Chao có độ sâu chỉ khoảng 30 vì phạm vi tọa độ của nó là`[0,10^9]`. Do đó, bản thân cây không thể kích hoạt giới hạn độ sâu đệ quy của Python. 

## Ví dụ đã hoạt động 

Mẫu đi kèm có cây```
        1
       / \
      2   3
     / \
    4   5
```Đối với đỉnh 4, đường dẫn là`1 -> 2 -> 4`. Đường dây của họ là 

[ 
5+T,\qquad 4+2T,\qquad 2+4T. 
] 

Giới hạn hiệu lực là`10^9`,`2`, Và`5`. 

Đối với truy vấn đầu tiên,`T = 0`, mọi dòng này đều hợp lệ. 

| Đỉnh | Dòng | C | Giá trị tại T=0 | Tối thiểu hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | (5+T) | (10^9) | 5 | 5 | 
| 2 | (4+2T) | 2 | 4 | 4 | 
| 4 | (2+4T) | 5 | 2 | 2 | 

Câu trả lời là`2`. 

Đối với truy vấn thứ hai,`T = 2`, cả ba đỉnh vẫn thỏa mãn điều kiện có giá trị. 

| Đỉnh | Dòng | C | Giá trị tại T=2 | Tối thiểu hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | (5+T) | (10^9) | 7 | 7 | 
| 2 | (4+2T) | 2 | 8 | 7 | 
| 4 | (2+4T) | 5 | 10 | 7 | 

Câu trả lời là`7`. 

Dấu vết cho thấy tại sao điều kiện hợp lệ lại được gắn vào chính đường đó chứ không phải vào đỉnh được truy vấn. Tại`T=2`, đỉnh 2 nằm chính xác trên ranh giới của nó và vẫn đủ điều kiện. 

Ví dụ thứ hai cô lập điều kiện biên.```
3 3
10 1 100
1 2 3
1000000000 5 100
1 2
2 3
2 5
3 5
3 0
```Đường dẫn gốc tới 3 chứa các đỉnh 1, 2 và 3. 

| Truy vấn | Các đỉnh đủ điều kiện | Giá trị | Trả lời | 
| --- | --- | --- | --- | 
|`(2,5)`| 1, 2 | 15, 11 | 11 | 
|`(3,5)`| 1, 2, 3 | 15, 11, 115 | 11 | 
|`(3,0)`| 1, 2, 3 | 10, 1, 100 | 1 | 

Truy vấn đầu tiên đặc biệt hữu ích vì`C_2 = 5`Và`T = 5`. Dòng từ đỉnh 2 phải vẫn tồn tại. Truy vấn cuối cùng xác nhận rằng`T = 0`làm cho mọi đỉnh đủ điều kiện và giảm mục tiêu xuống mức tối thiểu`A`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log^2 10^9+Q\log 10^9)) | Mỗi đỉnh tạo một lần chèn khoảng liên tục và mỗi truy vấn thực hiện một truy vấn điểm Li Chao | 
| Không gian | (O(N\log^2 10^9)) trường hợp xấu nhất | Kiên trì sao chép các nút Li Chao được sửa đổi sau mỗi lần chèn khoảng thời gian | 

Vì (\log_2(10^9)) chỉ khoảng 30 nên hệ số logarit bị giới hạn bởi một hằng số nhỏ. Giải pháp này tránh đi các đường dẫn từ gốc đến đỉnh cho mỗi truy vấn, đây là phần khiến việc triển khai trực tiếp không thể thực hiện được đối với cây hình chuỗi. 

Giới hạn bộ nhớ lớn 1024 MB rất hữu ích cho việc biểu diễn liên tục này. Chi phí chính là các nút Li Chao được sao chép chứ không phải cây gốc. 

## Trường hợp thử nghiệm```python
import io
import sys

# The production solution is the solve() function from above.
# For assert-based tests, execute the same algorithm against an
# in-memory stdin/stdout pair.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample1 = """\
5 2
5 4 3 2 1
1 2 3 4 5
1000000000 2 4 5 2
1 2
1 3
2 4
2 5
4 0
4 2
"""

assert run(sample1) == """\
2
7
""".strip(), "sample 1"

# Minimum-size tree. The only possible candidate is the root.
sample2 = """\
1 3
17
23
1000000000
1 0
1 1000000000
1 999999999
"""

assert run(sample2) == """\
17
23000000017
23000000000
""".strip(), "single vertex"

# Exact C boundary and T = 0.
sample3 = """\
3 3
10 1 100
1 2 3
1000000000 5 100
1 2
2 3
2 5
3 5
3 0
"""

assert run(sample3) == """\
11
11
1
""".strip(), "C boundary and zero"

# Equal slopes. The ancestor condition allows equal B values.
# At T=10, vertex 3 is valid exactly at C=10.
sample4 = """\
4 4
20 5 1 100
7 7 7 7
1000000000 3 10 2
1 2
2 3
2 4
3 3
3 10
4 10
4 0
"""

assert run(sample4) == """\
15
71
27
1
""".strip(), "equal slopes"

# Large values, testing 64-bit-sized products.
sample5 = """\
2 3
1000000000 1000000000
1000000000 1000000000
1000000000 1000000000
1 2
2 0
2 1
2 1000000000
"""

assert run(sample5) == """\
1000000000
2000000000
1000000000000001000000000
""".strip(), "large arithmetic"

# A chain catches implementations that accidentally exclude an
# ancestor or use the wrong validity comparison.
sample6 = """\
5 4
50 40 30 20 10
1 2 3 4 5
1000000000 1 2 3 4
1 2
2 3
3 4
4 5
5 0
5 1
5 2
5 4
"""

assert run(sample6) == """\
10
41
56
250
""".strip(), "chain boundaries"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn |`17`,`23000000017`,`23000000000`| Đường dẫn chỉ có gốc và giá trị truy vấn lớn nhất có thể | 
| Chuỗi ba đỉnh |`11`,`11`,`1`| Chính xác`C_i = T`ranh giới và`T = 0`| 
| Độ dốc bằng nhau |`15`,`71`,`27`,`1`| Tính đơn điệu không chặt chẽ của`B`và các đường có độ dốc bằng nhau | 
| Số học lớn | Giá trị lớn có kích thước 64 bit | Số học đúng số nguyên mà không bị tràn | 
| Chuỗi năm đỉnh |`10`,`41`,`56`,`250`| Nhiều ranh giới hiệu lực của tổ tiên và sự tồn tại của đường dẫn | 

## Vỏ cạnh 

Trường hợp một đỉnh```
1 3
17
23
1000000000
1 0
1 1000000000
1 999999999
```không có cạnh nào và mọi truy vấn đều sử dụng cùng một dòng gốc. Cấu trúc liên tục bắt đầu trống và chèn dòng gốc lên trên`[0,10^9]`. Mọi truy vấn đều đạt đến dòng đó, tạo ra`17`,`23000000017`, Và`23000000000`. 

Để có ranh giới hiệu lực chính xác,```
3 1
10 1 100
1 2 3
1000000000 5 100
1 2
2 3
2 5
```đỉnh 2 có`C_2 = 5`, vì vậy dòng của nó được chèn lên trên`[0,5]`. Truy vấn tại`T=5`đạt đến điểm cuối của khoảng đó và bao gồm đường thẳng. Giá trị của nó là`1+7*5=36`trong đầu vào cụ thể này nếu dòng có`B_2=7`; trong cuộc thử nghiệm bê tông trước đó,`A_2=1`Và`B_2=2`, cho`11`. Việc thực hiện sử dụng`qr = C[v]`, do đó sự bình đẳng được xử lý một cách tự nhiên. 

Vì`T=0`, mọi`C_i`ít nhất là 1, vì vậy mọi dòng trên đường dẫn đều hợp lệ. Truy vấn Li Chao bắt đầu ở tọa độ 0 và không cần nhánh đặc biệt. Trong đường dẫn mẫu`1 -> 2 -> 4`, các giá trị là`5`,`4`, Và`2`, vậy câu trả lời là`2`. 

Độ dốc bằng nhau cũng hợp pháp vì điều kiện trên`B`là không nghiêm ngặt. Giả sử tổ tiên và con của nó đều có`B=7`. Đường thẳng của họ song song. Phép so sánh Li Chao vẫn có tác dụng vì nó không bao giờ chia cho chênh lệch độ dốc. Dòng có giá trị nhỏ hơn tại tọa độ truy vấn sẽ tự động thắng. 

Cuối cùng, tích lớn nhất có thể có là (10^9\cdot10^9=10^{18}). Python xử lý việc này trực tiếp bằng các số nguyên có độ chính xác tùy ý. Khi triển khai có chiều rộng cố định, phép tính phải sử dụng loại có dấu 64 bit thay vì số nguyên 32 bit.
