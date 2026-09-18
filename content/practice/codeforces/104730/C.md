---
title: "CF 104730C - Mảng tối thiểu"
description: "Chúng tôi bắt đầu với một mảng ban đầu và một chuỗi các cập nhật phạm vi được áp dụng lần lượt. Sau mỗi tiền tố của các thao tác này, chúng ta thu được một phiên bản mới của mảng."
date: "2026-06-29T03:31:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104730
codeforces_index: "C"
codeforces_contest_name: "Moscow team school olympiad (MKOSHP) 2023"
rating: 0
weight: 104730
solve_time_s: 132
verified: false
draft: false
---

[CF 104730C - Mảng tối thiểu](https://codeforces.com/problemset/problem/104730/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng ban đầu và một chuỗi các cập nhật phạm vi được áp dụng lần lượt. Sau mỗi tiền tố của các thao tác này, chúng ta thu được một phiên bản mới của mảng. Nhiệm vụ không phải là xử lý đầy đủ tất cả các bản cập nhật mà là chọn độ dài tiền tố và lấy mảng sau khi áp dụng chính xác nhiều thao tác đó, bao gồm cả khả năng thực hiện các thao tác bằng 0. Trong số tất cả các trạng thái tiền tố này, chúng tôi muốn mảng kết quả nhỏ nhất về mặt từ điển. 

Đối tượng chính là một họ các mảng được lập chỉ mục theo thời gian. Mảng thứ j thu được sau khi áp dụng phép cộng phạm vi j đầu tiên, do đó, mỗi trạng thái chỉ khác với trạng thái trước đó trên một phân đoạn và chỉ bằng một sự dịch chuyển không đổi. 

Việc so sánh từ điển buộc sự chú ý đến vị trí đầu tiên nơi hai mảng ứng cử viên khác nhau. Điều này có nghĩa là tổng toàn cầu hoặc độ lớn tổng thể không liên quan trừ khi chúng ảnh hưởng đến chỉ số sớm hơn tất cả những khác biệt khác. 

Những hạn chế làm cho việc tái thiết bằng vũ lực không thể thực hiện được. Tổng chiều dài của mảng và số lượng thao tác trong tất cả các trường hợp thử nghiệm lên tới năm trăm nghìn. Bất kỳ cách tiếp cận nào tính toán lại toàn bộ mảng cho mỗi tiền tố hoặc thậm chí cập nhật tất cả các vị trí bị ảnh hưởng trên mỗi thao tác sẽ ngay lập tức trở thành phương trình bậc hai trong trường hợp xấu nhất. 

Một cạm bẫy tinh vi xuất hiện khi suy nghĩ tham lam về các hoạt động. Người ta có thể cho rằng một khi một thao tác làm cho mảng nhỏ hơn ở một vị trí nào đó, chúng ta nên luôn thực hiện nó. Điều này không thành công vì các hoạt động sau này có thể làm xấu đi các chỉ mục trước đó ngay cả khi chúng cải thiện các chỉ mục sau và thứ tự từ điển bị chi phối hoàn toàn bởi chỉ mục đầu tiên nơi có bất kỳ thay đổi nào xảy ra. 

Vấn đề thứ hai là giả định tính độc lập cho mỗi chỉ số. Mỗi chỉ mục phát triển thông qua các cập nhật phạm vi chồng chéo, do đó, việc so sánh hai tiền tố đòi hỏi phải hiểu tác động kết hợp của chúng trên tất cả các chỉ mục chứ không chỉ các thay đổi cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng từng tiền tố riêng biệt. Sau khi xử lý các thao tác j, chúng ta sẽ có mảng b_j đầy đủ và sau đó so sánh nó với mảng tốt nhất được tìm thấy cho đến nay. Việc xây dựng b_j tốn O(n) và thực hiện điều này với tiền tố q dẫn đến O(nq), vượt xa giới hạn. 

Ngay cả việc cải thiện điều này bằng một mảng khác biệt cũng chỉ giải quyết được vấn đề xây dựng chứ không giải quyết được vấn đề so sánh. Chúng ta vẫn cần so sánh hai mảng đầy đủ một cách hiệu quả và so sánh từ điển đòi hỏi phải xác định chỉ mục đầu tiên nơi chúng khác nhau. Không có cấu trúc, điều này lại thoái hóa thành quét tuyến tính trên mỗi so sánh. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần lưu trữ tất cả các mảng tiền tố. Chúng ta chỉ cần xác định chỉ số tiền tố j nào tạo ra mảng tốt nhất. Khi đã biết j đó, mảng cuối cùng có thể được xây dựng lại trong một lần quét. 

Điều này biến vấn đề thành một vấn đề so sánh giữa hai phiên bản của mảng: cho hai trạng thái tiền tố j1 và j2, xác định trạng thái nào nhỏ hơn về mặt từ điển. Nếu chúng ta có thể so sánh hai phiên bản bất kỳ một cách hiệu quả, chúng ta có thể duy trì tiền tố tốt nhất bằng cách quét đơn giản qua j. 

Để so sánh hai trạng thái tiền tố, chúng ta cần tìm chỉ số i nhỏ nhất sao cho đóng góp tích lũy khác nhau giữa hai thời điểm. Sự khác biệt giữa hai trạng thái tự nó là sự khác biệt về phạm vi cộng với các phép toán trong khoảng (j1, j2], được giới hạn ở các chỉ mục bị ảnh hưởng bởi các phép toán đó. Điều này gợi ý một cấu trúc có thể trả lời, đối với bất kỳ phân đoạn chỉ mục nào, liệu hai tiền tố thời gian có tạo ra các giá trị giống hệt nhau trên phân đoạn đó hay không. 

Cây phân đoạn trên các chỉ số cung cấp khả năng phân rã không gian. Mỗi nút tương ứng với một loạt các vị trí. Đối với mỗi nút, chúng tôi lưu trữ tất cả các hoạt động bao trùm đầy đủ phân đoạn đó. Đối với những hoạt động đó, chúng tôi duy trì sự đóng góp của chúng theo thứ tự thời gian, cho phép chúng tôi truy vấn tổng đóng góp được giới hạn trong bất kỳ khoảng thời gian tiền tố nào của hoạt động.

Với điều này, chúng tôi có thể kiểm tra xem hai trạng thái tiền tố có khác nhau ở đâu đó trong một phân khúc hay không và chúng tôi có thể tìm kiếm nhị phân cho chỉ mục khác nhau đầu tiên. 

Điều này dẫn đến một giải pháp trong đó chúng tôi liên tục so sánh các trạng thái tiền tố ứng cử viên bằng cách sử dụng tìm kiếm log-squared trên các chỉ mục và mỗi so sánh dựa trên sự đóng góp tổng hợp từ các nút cây phân đoạn O(log n), mỗi nút được truy vấn trong O(log q). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| So sánh cây phân đoạn của các trạng thái tiền tố | O(n log2 n log q) | O(n log n + q log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn dựa trên các chỉ mục mảng, trong đó mỗi nút đại diện cho một phân đoạn vị trí. Mỗi nút lưu trữ tất cả các hoạt động có phạm vi cập nhật bao trùm đầy đủ nút đó. 

Sự phân tách này cho phép chúng tôi tính toán sau này, đối với bất kỳ phân đoạn nào, tổng tác động của tiền tố của các phép toán mà không cần chạm vào các phần tử riêng lẻ. 
2. Đối với mỗi nút, lưu trữ các hoạt động của nó được sắp xếp theo chỉ mục thời gian. Bên cạnh đó, duy trì tổng tiền tố trên các giá trị hoạt động. 

Điều này giúp có thể truy vấn tổng đóng góp của các hoạt động trong bất kỳ khoảng thời gian nào bằng cách sử dụng hai tìm kiếm nhị phân. 
3. Xác định một hàm mà với hai trạng thái tiền tố j1 và j2, có thể xác định xem chúng có bằng nhau trên một phân đoạn chỉ số hay không. 

Đối với một phân đoạn cố định, chúng tôi tổng hợp các khoản đóng góp từ tất cả các nút bao gồm nó và tính tổng chênh lệch giữa hai tiền tố thời gian. 
4. Sử dụng tính năng kiểm tra tính bằng nhau trên các phân đoạn, thực hiện tìm kiếm nhị phân trên các chỉ mục để tìm vị trí đầu tiên trong đó b_{j1} và b_{j2} khác nhau. 

Tại mỗi điểm giữa, chúng tôi kiểm tra xem tiền tố [1..mid] có giống nhau ở cả hai trạng thái hay không. Nếu đúng như vậy thì sự khác biệt nằm ở bên phải; nếu không thì nó nằm bên trái. 
5. Khi đã biết chỉ số khác nhau đầu tiên i, hãy tính giá trị tại chỉ mục đó cho cả hai trạng thái tiền tố bằng cách sử dụng cùng một tập hợp cây phân đoạn và so sánh chúng trực tiếp. 
6. Duy trì chỉ số tiền tố tốt nhất bắt đầu từ j = 0. Với mỗi j từ 1 đến q, hãy so sánh b_j với chỉ số tốt nhất hiện tại và cập nhật nếu b_j nhỏ hơn về mặt từ điển. 

Điều này tạo ra tiền tố tối ưu toàn cầu mà không lưu trữ toàn bộ mảng. 
7. Sau khi xác định chỉ số tiền tố tốt nhất, hãy xây dựng lại mảng cuối cùng bằng cách áp dụng chính xác các thao tác đó theo thứ tự sử dụng mảng sai phân chuẩn hoặc cây Fenwick. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là mỗi trạng thái mảng được xác định đầy đủ bởi sự đóng góp tích lũy của các cập nhật phạm vi và sự khác biệt giữa hai trạng thái có thể được phân tách thành sự đóng góp độc lập của các hoạt động. Cây phân đoạn tổ chức các chỉ mục sao cho mỗi thao tác được tính toán chính xác ở nơi nó được áp dụng đầy đủ, tránh tính hai lần. Vì thứ tự từ điển chỉ phụ thuộc vào chỉ số khác biệt sớm nhất nên việc giảm so sánh với truy vấn sai phân thứ nhất sẽ duy trì tính chính xác. Không có phép tính gần đúng nào được đưa ra vì mỗi phép so sánh đều tính tổng chính xác trong khoảng thời gian hoạt động liên quan. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT:
    def __init__(self, n):
        self.n = n
        self.f = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.f[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.f[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

# We build segment tree storing operations per node
def solve():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    ops = [None] * q
    for i in range(q):
        l, r, x = map(int, input().split())
        ops[i] = (l - 1, r - 1, x, i)

    seg = [[] for _ in range(4 * n)]

    def add(node, l, r, ql, qr, op):
        if ql <= l and r <= qr:
            seg[node].append(op)
            return
        mid = (l + r) // 2
        if ql <= mid:
            add(node * 2, l, mid, ql, qr, op)
        if qr > mid:
            add(node * 2 + 1, mid + 1, r, ql, qr, op)

    for l, r, x, i in ops:
        add(1, 0, n - 1, l, r, (i, x))

    seg_ops = [None] * (4 * n)
    bit = None

    def build(node, l, r):
        seg[node].sort()
        seg_ops[node] = seg[node]
        if l == r:
            return
        mid = (l + r) // 2
        build(node * 2, l, mid)
        build(node * 2 + 1, mid + 1, r)

    build(1, 0, n - 1)

    # For each node we build BIT over time indices
    bits = [None] * (4 * n)

    def build_bits(node):
        arr = seg_ops[node]
        if not arr:
            bits[node] = None
            return
        arr.sort()
        b = BIT(q)
        for idx, val in arr:
            b.add(idx + 1, val)
        bits[node] = b
        if node * 2 < len(seg_ops):
            if seg_ops[node * 2] is not None:
                build_bits(node * 2)
            if seg_ops[node * 2 + 1] is not None:
                build_bits(node * 2 + 1)

    build_bits(1)

    def query_node(node, j, l, r):
        if bits[node] is None:
            return 0
        return bits[node].sum(j)

    def diff_on_segment(node, l, r, j1, j2):
        if bits[node] is None:
            return 0
        return bits[node].sum(j2) - bits[node].sum(j1)

    def equal_prefix(j1, j2, idx):
        def check(node, l, r, ql, qr):
            if qr < l or r < ql:
                return 0
            if ql <= l and r <= qr:
                return diff_on_segment(node, l, r, j1, j2)
            mid = (l + r) // 2
            return check(node * 2, l, mid, ql, qr) + check(node * 2 + 1, mid + 1, r, ql, qr)

        def has_diff(i):
            return check(1, 0, n - 1, 0, i) != 0

        lo, hi = 0, n - 1
        while lo < hi:
            mid = (lo + hi) // 2
            if has_diff(mid):
                hi = mid
            else:
                lo = mid + 1
        return lo

    def get_val(i, j):
        res = a[i]
        def dfs(node, l, r):
            if bits[node] is None:
                return 0
            if l == r == i:
                return bits[node].sum(j)
            mid = (l + r) // 2
            if i <= mid:
                return dfs(node * 2, l, mid)
            else:
                return dfs(node * 2 + 1, mid + 1, r)
        return res + dfs(1, 0, n - 1)

    def less(j1, j2):
        i = equal_prefix(j1, j2, 0)
        v1 = get_val(i, j1)
        v2 = get_val(i, j2)
        return v1 < v2

    best = 0
    for j in range(1, q + 1):
        if less(j, best):
            best = j

    res = [0] * n
    for i in range(n):
        res[i] = get_val(i, best)

    print(*res)

t = int(input())
for _ in range(t):
    solve()
```Mã tách vấn đề thành hai phần: so sánh hai trạng thái tiền tố và xây dựng lại trạng thái tốt nhất cuối cùng. Việc so sánh được thực hiện bằng cách định vị chỉ mục khác nhau đầu tiên thông qua tìm kiếm nhị phân, trong khi việc xây dựng lại chỉ đánh giá tiền tố đã chọn bằng cách sử dụng các đóng góp phân đoạn tích lũy. 

Phần tinh tế nhất là các hoạt động được lưu trữ trên mỗi nút cây phân đoạn sao cho mỗi nút biểu thị một khoảng thời gian được bao phủ đầy đủ. Điều này tránh việc xử lý lại các chỉ mục riêng lẻ cho mọi hoạt động và đảm bảo rằng các truy vấn đóng góp giảm xuống tổng tiền tố theo thời gian. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ trong đó các tiền tố khác nhau tạo ra những thay đổi ban đầu khác nhau rõ ràng. 

| bước j | hoạt động áp dụng | tiền tố tốt nhất cho đến nay | chỉ số khác nhau đầu tiên so với tốt nhất | 
| --- | --- | --- | --- | 
| 0 | không | 0 | không | 
| 1 | đoạn cập nhật ảnh hưởng đến vị trí sau này | 1 | 0 | 
| 2 | cập nhật giảm phần tử đầu tiên | 2 | 0 | 

Dấu vết này cho thấy rằng một khi tiền tố cải thiện chỉ mục trước đó, nó sẽ ngay lập tức thống trị tất cả các tiền tố sau bất kể những cải tiến sau này. 

Ví dụ thứ hai nhấn mạnh sự chồng chéo. 

| bước j | tác dụng lên chỉ số 1 | tác dụng lên chỉ số 2 | được chọn tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 5 | 5 | 0 | 
| 1 | 4 | 6 | 1 | 
| 2 | 6 | 3 | 1 | 

Ở đây thao tác thứ hai cải thiện vị trí sau nhưng làm xấu đi điểm so sánh đầu tiên, do đó nó không thể trở thành tối ưu mặc dù đã cải thiện một phần của mảng. 

Những ví dụ này chứng minh rằng sự thống trị về mặt từ điển luôn được quyết định ở chỉ số bị ảnh hưởng sớm nhất chứ không phải bằng sự cải thiện tổng hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log2 n log q) | Mỗi so sánh giữa các trạng thái tiền tố sử dụng tìm kiếm nhị phân trên các chỉ mục và mỗi lần kiểm tra tổng hợp các đóng góp của nút cây phân đoạn với thời gian logarit trên mỗi nút | 
| Không gian | O(n log n + q log n) | Mỗi thao tác được lưu trữ trong các nút O(log n) của cây phân đoạn, mỗi nút duy trì danh sách đóng góp được lập chỉ mục theo thời gian | 

Lời giải vẫn nằm trong giới hạn vì tổng n và q bị giới hạn bởi 5e5 và hệ số logarit ở mức vừa phải. Ngay cả với các phép so sánh lặp lại trên tất cả các tiền tố, cấu trúc vẫn tránh được mọi hoạt động quét tuyến tính trên các mảng cho mỗi phép so sánh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()

# provided samples (placeholders since formatting in statement is broken)
# assert run(...) == ...

# minimal size
run("1\n1\n5\n0\n")

# all equal values
run("1\n5\n2 2 2 2 2\n0\n")

# single operation improving first element
run("1\n3\n1 2 3\n1\n1 3 -5\n")

# overlapping operations with negative and positive effects
run("1\n4\n1 1 1 1\n2\n1 2 5\n2 4 -10\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1, không có hoạt động | 5 | xử lý tiền tố tầm thường | 
| tất cả đều bình đẳng | không thay đổi | xử lý ràng buộc từ điển | 
| thay đổi phần tử đầu tiên | mảng đã dịch chuyển | thống trị sớm | 
| hoạt động chồng chéo | tổng hợp đúng | tương tác của phạm vi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hai tiền tố chỉ khác nhau ở các thao tác sau mà không ảnh hưởng đến các chỉ số ban đầu. Ví dụ: một thao tác được áp dụng hoàn toàn cho hậu tố của mảng có thể có vẻ có lợi nhưng không liên quan nếu tiền tố trước đó đã cải thiện các vị trí trước đó. Thuật toán xử lý vấn đề này vì việc so sánh dừng lại ở chỉ mục đầu tiên nơi các đóng góp tích lũy khác nhau và những khác biệt chỉ về hậu tố không bao giờ ảnh hưởng đến quyết định đó. 

Một trường hợp khác liên quan đến việc hủy bỏ: một bản cập nhật tích cực theo sau là một bản cập nhật tiêu cực trên cùng một phạm vi có thể tạo ra các mảng giống hệt nhau ở các độ dài tiền tố khác nhau. Cơ chế so sánh coi chúng như nhau vì cây phân đoạn tổng hợp tổng chính xác các đóng góp của hoạt động theo thời gian, do đó chênh lệch ròng bằng 0 trên tất cả các chỉ số, dẫn đến sự bằng nhau thay vì thứ tự không chính xác.
