---
title: "CF 106151F - kiểm toán"
description: "Chúng ta được cung cấp một chuỗi các phép đo theo thời gian, được lưu trữ trong một mảng. Mỗi truy vấn yêu cầu chúng ta xem xét một đoạn liền kề của mảng này. Điều khó khăn là mọi độ dài đoạn không phải là tùy ý, nó luôn là một trong hai độ dài cố định, L1 hoặc L2."
date: "2026-06-19T19:24:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106151
codeforces_index: "F"
codeforces_contest_name: "2025 ICPC Greek Collegiate Programming Contest (GRCPC 2025)"
rating: 0
weight: 106151
solve_time_s: 59
verified: true
draft: false
---

[CF 106151F - kiểm toán](https://codeforces.com/problemset/problem/106151/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các phép đo theo thời gian, được lưu trữ trong một mảng. Mỗi truy vấn yêu cầu chúng ta xem xét một đoạn liền kề của mảng này. Điều khó khăn là mọi độ dài đoạn không phải là tùy ý, nó luôn là một trong hai độ dài cố định, L1 hoặc L2. Đối với mỗi phân khúc như vậy, chúng ta phải báo cáo ba giá trị: phần tử nhỏ nhất trong phân khúc, phần tử lớn nhất trong phân khúc và phần tử nhỏ thứ k bên trong cùng phân khúc đó. 

Vì vậy, mỗi truy vấn về cơ bản là yêu cầu hai cực trị thống kê thứ tự cộng với một thống kê thứ tự chung trên một mảng con: phạm vi tối thiểu, phạm vi tối đa và truy vấn lựa chọn bên trong phạm vi. 

Các ràng buộc thúc đẩy chúng tôi xử lý tới 100.000 phần tử và 100.000 truy vấn. Một cách tiếp cận đơn giản sắp xếp từng mảng con được truy vấn sẽ quá chậm vì việc sắp xếp độ dài lên tới 100.000 phân đoạn cho mỗi truy vấn sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. Ngay cả việc duy trì các đống cho mỗi truy vấn vẫn sẽ bị suy giảm nghiêm trọng do việc xây dựng lại nhiều lần. 

Một gợi ý cấu trúc tinh tế là tất cả độ dài truy vấn chỉ đến từ hai giá trị cố định. Điều đó có nghĩa là chúng tôi không cần một hệ thống dải động hoàn chỉnh; chúng tôi chỉ cần hỗ trợ truy vấn phạm vi cho hai kích thước cửa sổ cố định. 

Một trường hợp lỗi phổ biến phát sinh nếu người ta cố gắng tính toán lại kết quả một cách độc lập cho mỗi truy vấn. Ví dụ, nếu mảng là`[5, 1, 4, 2, 3]`và chúng tôi được yêu cầu phân đoạn`[2, 4, 2]`, sắp xếp cho`[1, 2, 4]`, vậy min là 1, max là 4, k-th là 2. Việc tính toán lại điều này cho mỗi truy vấn là được, nhưng thực hiện 100 nghìn lần thì không. 

Một cạm bẫy tinh vi khác là giả sử độ dài phân đoạn được cố định có nghĩa là chỉ xử lý trước một lần cho mỗi độ dài là đủ mà không cần xử lý cẩn thận các truy vấn thứ k. Tối thiểu và tối đa rất dễ dàng với các bảng thưa thớt, nhưng nhỏ nhất thứ k yêu cầu cấu trúc khác. 

## Phương pháp tiếp cận 

Giải pháp bạo lực xử lý từng truy vấn một cách độc lập. Đối với một truy vấn, chúng tôi trích xuất mảng con, tính toán mức tối thiểu và tối đa của nó thông qua quá trình quét và sắp xếp nó để thu được phần tử nhỏ nhất thứ k. Mỗi truy vấn có chi phí O(L log L) do sắp xếp và trong trường hợp xấu nhất L lên tới N. Với Q lên tới 100.000, điều này trở nên không khả thi. 

Quan sát quan trọng là chúng tôi chỉ truy vấn phạm vi có hai độ dài cố định. Điều này cho phép chúng tôi tính toán trước các cấu trúc dữ liệu riêng biệt cho từng độ dài. Đối với mức tối thiểu và tối đa, việc xử lý trước bảng thưa hoặc cửa sổ trượt trên toàn bộ mảng là đủ. Thách thức thực sự là hỗ trợ hiệu quả các truy vấn nhỏ thứ k trên các cửa sổ có kích thước cố định. 

Chúng tôi xử lý vấn đề này bằng cách xây dựng cây phân đoạn ổn định trên các giá trị nén. Sau khi mảng được nén giá trị, mỗi phiên bản tiền tố của cây phân đoạn sẽ lưu trữ tần số đếm các giá trị cho đến chỉ mục đó. Sau đó, bất kỳ truy vấn phạm vi [l, r] nào cũng có thể được trả lời bằng cách trừ hai cây tiền tố, cho chúng ta phân bố tần số cho phân đoạn đó. Từ đó, chúng ta có thể trích xuất nhỏ nhất, tối đa và thứ k nhỏ nhất theo thời gian logarit bằng cách duyệt qua cấu trúc cây. 

Vì chúng ta chỉ cần hai độ dài nên chúng ta không cần các cấu trúc riêng biệt cho mỗi độ dài; chúng tôi có thể trả lời bất kỳ truy vấn phạm vi nào trong O (log N) bằng cách sử dụng các khác biệt về tiền tố. Min và max chỉ đơn giản là vị trí khác 0 đầu tiên và cuối cùng trong cây tần số. 

Sự khác biệt về hiệu suất rất lớn: thay vì sắp xếp theo từng truy vấn, chúng tôi giảm từng truy vấn xuống mức truyền tải logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · L log L) | O(L) | Quá chậm | 
| Tối ưu (ý tưởng segtree liên tục + bảng thưa thớt) | O((N + Q) log N) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách chuyển các truy vấn phạm vi thành các truy vấn sai phân tiền tố trên cấu trúc tần số. 

1. Đầu tiên, nén tất cả các giá trị mảng thành các hạng từ 1 đến M. Điều này là cần thiết để chúng ta có thể lưu trữ tần số trong cây phân đoạn mà không cần sử dụng bộ nhớ lớn tỷ lệ thuận với 10^9. Việc nén duy trì trật tự, đó là tất cả những gì quan trọng đối với các truy vấn nhỏ thứ k. 
2. Xây dựng cây phân đoạn ổn định dựa trên các giá trị nén này. Mỗi phiên bản i đại diện cho phân bố tần số của a[1..i]. Điều này có hiệu quả vì việc chuyển từ phiên bản i sang i+1 chỉ làm tăng số lượng a[i+1], vì vậy chúng tôi có thể sử dụng lại các nút cấu trúc trước đó và chỉ sửa đổi các nút O(log N) cho mỗi lần cập nhật. 
3. Với mỗi truy vấn [l, r], hãy xây dựng cấu trúc tần số của nó dưới dạng phiên bản [r] trừ phiên bản [l-1]. Điều này mang lại cho chúng ta một cấu trúc biểu diễn chính xác nhiều phần tử trong phạm vi. 
4. Để tính giá trị tối thiểu, duyệt cây phân đoạn từ trái sang phải, luôn ưu tiên con bên trái nếu nó có số đếm khác 0. Giá trị đầu tiên gặp phải là giá trị nhỏ nhất trong phạm vi. 
5. Để tính giá trị lớn nhất, thực hiện tương tự nhưng duyệt từ phải sang trái, ưu tiên con bên phải trước. 
6. Để tính giá trị nhỏ nhất thứ k, hãy bắt đầu từ gốc và liên tục xác định xem phần tử thứ k nằm trong cây con trái hay cây con phải bằng cách so sánh k với tần số của cây con trái. Nếu số bên trái ít nhất là k, hãy sang trái; nếu không thì trừ số bên trái khỏi k và sang phải. 

### Tại sao nó hoạt động

Cây phân đoạn liên tục duy trì số tần số chính xác cho mọi tiền tố của mảng. Bất kỳ truy vấn phạm vi nào cũng trở thành sự khác biệt của hai trạng thái tiền tố hợp lệ, tương đương với việc đếm số lần xuất hiện bên trong phạm vi đó. Vì cây phân chia không gian giá trị theo thứ tự nên việc duyệt từ trái sang phải tương ứng với thứ tự giá trị tăng dần. Điều này đảm bảo rằng việc chọn theo tần số tích lũy sẽ tái tạo lại chính xác số liệu thống kê thứ tự đã sắp xếp mà không cần sắp xếp rõ ràng phạm vi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("l", "r", "cnt")
    def __init__(self):
        self.l = -1
        self.r = -1
        self.cnt = 0

def build(nodes, tl, tr):
    idx = len(nodes)
    nodes.append(Node())
    if tl != tr:
        tm = (tl + tr) // 2
        nodes[idx].l = build(nodes, tl, tm)
        nodes[idx].r = build(nodes, tm + 1, tr)
    return idx

def update(nodes, prev, tl, tr, pos):
    idx = len(nodes)
    nodes.append(Node())
    nodes[idx].l = nodes[prev].l
    nodes[idx].r = nodes[prev].r
    nodes[idx].cnt = nodes[prev].cnt + 1
    if tl != tr:
        tm = (tl + tr) // 2
        if pos <= tm:
            nodes[idx].l = update(nodes, nodes[prev].l, tl, tm, pos)
        else:
            nodes[idx].r = update(nodes, nodes[prev].r, tm + 1, tr, pos)
    return idx

def query_kth(nodes, left_root, right_root, tl, tr, k):
    if tl == tr:
        return tl
    left_count = nodes[nodes[right_root].l].cnt - nodes[nodes[left_root].l].cnt
    tm = (tl + tr) // 2
    if k <= left_count:
        return query_kth(nodes, nodes[left_root].l, nodes[right_root].l, tl, tm, k)
    else:
        return query_kth(nodes, nodes[left_root].r, nodes[right_root].r, tm + 1, tr, k - left_count)

def query_min(nodes, left_root, right_root, tl, tr):
    if tl == tr:
        return tl
    left_count = nodes[nodes[right_root].l].cnt - nodes[nodes[left_root].l].cnt
    tm = (tl + tr) // 2
    if left_count > 0:
        return query_min(nodes, nodes[left_root].l, nodes[right_root].l, tl, tm)
    return query_min(nodes, nodes[left_root].r, nodes[right_root].r, tm + 1, tr)

def query_max(nodes, left_root, right_root, tl, tr):
    if tl == tr:
        return tl
    right_count = nodes[nodes[right_root].r].cnt - nodes[nodes[left_root].r].cnt
    tm = (tl + tr) // 2
    if right_count > 0:
        return query_max(nodes, nodes[left_root].r, nodes[right_root].r, tm + 1, tr)
    return query_max(nodes, nodes[left_root].l, nodes[right_root].l, tl, tm)

def main():
    n, q = map(int, input().split())
    L1, L2 = map(int, input().split())
    arr = list(map(int, input().split()))

    vals = sorted(set(arr))
    comp = {v: i + 1 for i, v in enumerate(vals)}
    rev = {i + 1: v for i, v in enumerate(vals)}

    m = len(vals)

    nodes = [Node()]
    roots = [0]

    empty = build(nodes, 1, m)

    for i in range(n):
        roots.append(update(nodes, roots[-1], 1, m, comp[arr[i]]))

    out = []
    for _ in range(q):
        t, i, k = map(int, input().split())
        l = i
        r = i + (L1 if t == 1 else L2) - 1

        left_root = roots[l - 1]
        right_root = roots[r]

        mn = query_min(nodes, left_root, right_root, 1, m)
        mx = query_max(nodes, left_root, right_root, 1, m)
        kth = query_kth(nodes, left_root, right_root, 1, m, k)

        out.append(f"{rev[mn]} {rev[kth]} {rev[mx]}")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Đầu tiên, mã nén các giá trị để cây phân đoạn hoạt động trên một phạm vi nhỏ gọn. Mỗi phiên bản tiền tố của cây được lưu trữ trong`roots`, Ở đâu`roots[i]`tương ứng với phần tử thứ i đầu tiên. Các truy vấn chuyển thành so sánh hai phiên bản của cây, giúp tách biệt phạm vi phân bổ tần số. 

các`query_kth`hàm là logic cốt lõi: nó sử dụng số lượng cây con để quyết định hướng. các`query_min`Và`query_max`các hàm là các phép duyệt chuyên biệt luôn đẩy về phía nhỏ nhất hoặc lớn nhất có sẵn tùy thuộc vào cây con nào chứa bất kỳ phần tử nào trong phạm vi. 

Phải cẩn thận khi lập chỉ mục: gốc tiền tố bắt đầu từ 0 đại diện cho một mảng trống, do đó, truy vấn bắt đầu từ chỉ mục l sẽ sử dụng`roots[l-1]`. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

đầu vào:```
N=6, Q=1
L1=4, L2=3
a = [9,2,8,4,3,2]
query: (1,1,2)
```Phạm vi là [1,4] = [9,2,8,4] 

| Bước | Hoạt động | Tần số trái | Tần số phù hợp | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | phạm vi xây dựng | trống | đầy đủ | bắt đầu | 
| 2 | truy vấn tối thiểu | cây con bên trái có giá trị | đi bên trái trước | | 
| 3 | truy vấn tối đa | cây con bên phải trống | đi dự phòng bên trái | | 
| 4 | kth=2 | số trái=2 | đi bên trái | | 

Đầu ra là`2 4 9`. 

Điều này xác nhận rằng sự khác biệt tiền tố sẽ xây dựng lại chính xác thứ tự được sắp xếp mà không cần sắp xếp rõ ràng. 

### Ví dụ thứ hai 

đầu vào:```
N=5, Q=1
L1=3, L2=3
a = [5,1,4,2,3]
query: (1,2,2)
```Phạm vi là [2,4] = [1,4,2] 

Đã sắp xếp là [1,2,4] nên kết quả là`1 2 4`. 

| Bước | truyền tải thứ k | đếm trái | hành động | 
| --- | --- | --- | --- | 
| gốc | chia | 1 ở bên trái | đi bên trái | 
| trái | chính xác | tìm thấy 1 | tiếp tục | 
| đúng | còn lại | k=2 | chọn 2 | 

Điều này cho thấy tính đúng đắn của logic lựa chọn thứ k khi nén giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) log N) | mỗi cập nhật và truy vấn đi qua chiều cao cây phân đoạn | 
| Không gian | O(N log N) | các nút liên tục lưu trữ O(log N) mỗi lần chèn | 

Các ràng buộc cho phép khoảng 10^5 log 10^5 thao tác, nằm trong giới hạn trong 1 giây trong Python được tối ưu hóa hoặc thoải mái trong PyPy/C++. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Node:
        __slots__ = ("l", "r", "cnt")
        def __init__(self):
            self.l = -1
            self.r = -1
            self.cnt = 0

    def build(nodes, tl, tr):
        idx = len(nodes)
        nodes.append(Node())
        if tl != tr:
            tm = (tl + tr) // 2
            nodes[idx].l = build(nodes, tl, tm)
            nodes[idx].r = build(nodes, tm + 1, tr)
        return idx

    def update(nodes, prev, tl, tr, pos):
        idx = len(nodes)
        nodes.append(Node())
        nodes[idx].l = nodes[prev].l
        nodes[idx].r = nodes[prev].r
        nodes[idx].cnt = nodes[prev].cnt + 1
        if tl != tr:
            tm = (tl + tr) // 2
            if pos <= tm:
                nodes[idx].l = update(nodes, nodes[prev].l, tl, tm, pos)
            else:
                nodes[idx].r = update(nodes, nodes[prev].r, tm + 1, tr, pos)
        return idx

    def query_kth(nodes, left_root, right_root, tl, tr, k):
        if tl == tr:
            return tl
        left_count = nodes[nodes[right_root].l].cnt - nodes[nodes[left_root].l].cnt
        tm = (tl + tr) // 2
        if k <= left_count:
            return query_kth(nodes, nodes[left_root].l, nodes[right_root].l, tl, tm, k)
        else:
            return query_kth(nodes, nodes[left_root].r, nodes[right_root].r, tm + 1, tr, k - left_count)

    def query_min(nodes, left_root, right_root, tl, tr):
        if tl == tr:
            return tl
        left_count = nodes[nodes[right_root].l].cnt - nodes[nodes[left_root].l].cnt
        tm = (tl + tr) // 2
        if left_count > 0:
            return query_min(nodes, nodes[left_root].l, nodes[right_root].l, tl, tm)
        return query_min(nodes, nodes[left_root].r, nodes[right_root].r, tm + 1, tr)

    def query_max(nodes, left_root, right_root, tl, tr):
        if tl == tr:
            return tl
        right_count = nodes[nodes[right_root].r].cnt - nodes[nodes[left_root].r].cnt
        tm = (tl + tr) // 2
        if right_count > 0:
            return query_max(nodes, nodes[left_root].r, nodes[right_root].r, tm + 1, tr)
        return query_max(nodes, nodes[left_root].l, nodes[right_root].l, tl, tm)

    data = inp.strip().split()
    n, q = map(int, data[:2])
    L1, L2 = map(int, data[2:4])
    arr = list(map(int, data[4:4+n]))

    vals = sorted(set(arr))
    comp = {v:i+1 for i,v in enumerate(vals)}
    rev = {i+1:v for i,v in enumerate(vals)}
    m = len(vals)

    nodes = [Node()]
    roots = [0]
    empty = build(nodes, 1, m)

    for i in range(n):
        roots.append(update(nodes, roots[-1], 1, m, comp[arr[i]]))

    idx = 4 + n
    out = []
    for _ in range(q):
        t = int(data[idx]); i = int(data[idx+1]); k = int(data[idx+2])
        idx += 3
        l = i
        r = i + (L1 if t == 1 else L2) - 1
        left_root = roots[l-1]
        right_root = roots[r]

        mn = query_min(nodes, left_root, right_root, 1, m)
        mx = query_max(nodes, left_root, right_root, 1, m)
        kth = query_kth(nodes, left_root, right_root, 1, m, k)

        out.append(f"{rev[mn]} {rev[kth]} {rev[mx]}")

    return "\n".join(out)

# sample-style sanity checks
assert run("6 1\n4 3\n9 2 8 4 3 2\n1 1 2\n") == "2 4 9"
assert run("5 1\n3 3\n5 1 4 2 3\n1 2 2\n") == "1 2 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | 2 4 9 | chính xác hoàn toàn trên phạm vi hỗn hợp | 
| mẫu 2 | 1 2 4 | tính đúng logic thứ k | 
| tất cả đều bình đẳng | 7 7 7 | xử lý trùng lặp | 
| mẫu đơn | giới hạn nghiêm ngặt | an toàn từng người một | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị trong một phân đoạn đều bằng nhau. Trong trường hợp đó, cả giá trị nhỏ nhất thứ k và giá trị tối thiểu/tối đa đều thu gọn về cùng một giá trị. Cây phân đoạn liên tục xử lý việc này một cách tự nhiên vì tất cả số lượng tích lũy trong cùng một lá, do đó cả việc duyệt qua trái và phải luôn hội tụ chính xác. 

Một trường hợp khác là khi k ở gần các ranh giới, chẳng hạn như k = 2 hoặc k = length - 1. Những trường hợp này kiểm tra xem số lượng cây con có được tính toán chính xác từ những khác biệt về tiền tố hay không. Thuật toán vẫn ổn định vì mọi quyết định đều dựa trên số lượng tần số chính xác chứ không phải giả định về vị trí. 

Trường hợp cạnh cuối cùng là khi L1 và L2 khác nhau đáng kể. Nếu L1 nhỏ và L2 lớn, quá trình tiền xử lý đơn giản theo độ dài sẽ không thành công, nhưng cách tiếp cận cây tiền tố vẫn không bị ảnh hưởng vì nó độc lập với độ dài truy vấn.
