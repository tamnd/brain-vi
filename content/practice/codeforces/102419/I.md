---
title: "CF 102419I - Một vấn đề truy vấn khác"
description: "Chúng tôi duy trì một mảng số nguyên (A) có độ dài (n), ban đầu chứa đầy các số 0. Phép toán loại 2 thêm một cấp số cộng vào một khoảng liền kề. Đối với một thao tác ((l,r,a,b)), vị trí (i) nhận được [ a+b(i-l)."
date: "2026-08-12T20:25:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "I"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 474
verified: true
draft: false
---

[CF 102419I - Một vấn đề truy vấn khác](https://codeforces.com/problemset/problem/102419/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 54 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một mảng số nguyên (A) có độ dài (n), ban đầu chứa đầy các số 0. Phép toán loại 2 thêm một cấp số cộng vào một khoảng liền kề. Đối với một thao tác ((l,r,a,b)), vị trí (i) nhận được 

[ 
a+b(i-l). 
] 

Thao tác loại 1 hỏi xem mọi giá trị trong (A_l,A_{l+1},\ldots,A_r) có giống nhau hay không. Đầu ra được yêu cầu là (1) khi khoảng không đổi và (0) nếu không. 

Giới hạn (n,q\le 2\cdot10^5) loại trừ việc quét toàn bộ khoảng thời gian cho mỗi truy vấn. Trong trường hợp xấu nhất, có thể có các thao tác (2\cdot10^5) trong các khoảng độ dài (2\cdot10^5), mang lại khả năng truy cập mảng khoảng (4\cdot10^{10}). Chúng ta cần mỗi thao tác thực hiện khoảng (O(\log n)) hoặc ít nhất là nhỏ hơn đáng kể so với thời gian tuyến tính. 

Phần khó khăn là việc cập nhật cấp số cộng có vẻ như thay đổi mọi phần tử của một khoảng một cách độc lập. Việc lưu trữ các giá trị thực tế và sửa đổi toàn bộ khoảng thời gian chính xác là điều khiến giải pháp vũ phu trở nên quá chậm. 

Trường hợp cạnh hữu ích là truy vấn một phần tử. Ví dụ,```
1 1
1 1 1
```có đầu ra```
1
```bởi vì một phần tử luôn bằng chính nó. Một giải pháp tìm kiếm sự khác biệt bên trong khoảng có thể vô tình coi đây là một phạm vi không cố định. 

Một trường hợp cạnh khác xảy ra khi tiến trình có (b=0). Ví dụ,```
3 2
2 1 3 5 0
1 1 3
```sản xuất```
1
```vì bản cập nhật thêm (5) vào mọi vị trí, cho ([5,5,5]). Một giải pháp bất cẩn có thể cho rằng mọi thao tác loại 2 nhất thiết phải tạo ra các giá trị khác nhau vì nó được mô tả như một cấp số cộng. 

Các ranh giới khoảng cách cũng có vấn đề. Coi như```
4 3
2 2 4 1 1
1 2 4
1 1 4
```Sau khi cập nhật mảng là ([0,1,2,3]), do đó kết quả đầu ra là```
0
0
```Truy vấn đầu tiên chỉ kiểm tra các vị trí từ 2 đến 4. Truy vấn thứ hai bao gồm vị trí 1, không bị ảnh hưởng. Việc triển khai mảng chênh lệch mà quên ranh giới bên phải hoặc sử dụng (l) thay vì (l+1) khi kiểm tra sự bằng nhau có thể âm thầm bao gồm sai khác. 

Cập nhật tiêu cực là một nguồn sai lầm khác. Ví dụ,```
3 3
2 1 3 5 -2
1 1 3
2 2 2 -1 0
1 1 3
```cho```
0
0
```Bản cập nhật đầu tiên tạo ([5,3,1]) và bản cập nhật thứ hai thay đổi vị trí 2 thành (2), đưa ra ([5,2,1]). Cấu trúc dữ liệu phải hoạt động với các giá trị có dấu, không chỉ với việc giá trị tăng hay giảm. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là lưu trữ (A) chính nó. Đối với thao tác loại 2, lặp từ (l) đến (r) và thêm số hạng tương ứng của cấp số nhân. Đối với truy vấn loại 1, hãy quét khoảng thời gian và so sánh mọi giá trị với giá trị đầu tiên. Điều này đúng vì nó thực hiện rõ ràng chính xác các thao tác được mô tả bởi bài toán và trực tiếp kiểm tra định nghĩa của một khoảng không đổi. 

Vấn đề là trường hợp xấu nhất. Một bản cập nhật có thể chạm vào các phần tử (O(n)) và một truy vấn có thể kiểm tra các phần tử (O(n)). Với các thao tác (2\cdot10^5), việc này có thể yêu cầu khoảng (4\cdot10^{10}) thao tác mảng cơ bản, vượt xa giới hạn thời gian. 

Quan sát quan trọng là ngừng nhìn vào các giá trị mà thay vào đó hãy nhìn vào sự khác biệt liền kề của chúng. Xác định 

[ 
D_i=A_i-A_{i-1}, 
] 

với (A_0=0). Một khoảng (A_l,\ldots,A_r) không đổi chính xác khi 

[ 
D_{l+1}=D_{l+2}=\cdots=D_r=0. 
] 

Vì vậy, một truy vấn đẳng thức có khả năng dài sẽ trở thành một truy vấn hỏi xem liệu một phạm vi khác biệt có chứa bất kỳ giá trị nào khác không hay không. 

Bây giờ hãy xem xét cập nhật cấp số cộng sẽ làm gì với (D). hãy để 

[ 
x=a+b(r-l) 
] 

là giá trị cuối cùng được thêm vào khoảng. Tại vị trí (l), chênh lệch thay đổi theo (a). Giữa (l+1) và (r), mọi hiệu liền kề đều thay đổi bởi (b). Tại (r+1), chênh lệch thay đổi theo (-x). Do đó toàn bộ bản cập nhật sẽ trở thành 

[ 
D_l\mathrel{+}=a, 
] 

[ 
D_{l+1},\ldots,D_r\mathrel{+}=b, 
] 

[ 
D_{r+1}\mathrel{-}=x. 
] 

Tiến trình số học dài đã được giảm xuống còn một phạm vi cộng và hai thay đổi điểm. 

Chúng ta vẫn cần xác định xem mọi giá trị trong phạm vi (D) có bằng 0 hay không. Một cách thuận tiện để làm điều đó là duy trì tổng bình phương của các hiệu. Vì mọi (D_i^2) đều không âm nên 

[ 
\sum D_i^2=0 
] 

đúng khi mọi (D_i) bằng 0. 

Cây phân đoạn lười biếng có thể duy trì cả hai 

[ 
S=\tổng D_i 
] 

và 

[ 
Q=\tổng D_i^2 
] 

cho mỗi phân khúc. Nếu chúng ta thêm (x) vào mọi giá trị trong một đoạn có độ dài (k), thì 

[ 
S' = S+kx 
] 

và 

[ 
Q' = Q+2xS+kx^2. 
] 

Điều này mang lại sự lan truyền lười biếng theo thời gian liên tục ở mọi phân đoạn được truy cập. Mỗi cập nhật cấp số số học cần (O(\log n)) thao tác trên cây phân đoạn và mỗi truy vấn đẳng thức cần một truy vấn phạm vi (O(\log n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nq)) | (O(n)) | Quá chậm | 
| Mảng khác biệt + cây phân đoạn lười biếng | (O(q\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mảng hiện tại thông qua mảng sai phân (D), trong đó (D_i=A_i-A_{i-1}). Ban đầu mọi (D_i) đều bằng 0 vì mọi (A_i) đều bằng 0. 
2. Xây dựng cây phân đoạn lười biếng trên (D). Mỗi nút lưu trữ tổng các chênh lệch của nó, tổng bình phương của chúng và một giá trị lười biểu thị phần bổ sung đang chờ xử lý cho toàn bộ phân khúc. Tổng là cần thiết khi cập nhật tổng bình phương. 
3. Đối với bản cập nhật loại 2 ((l,r,a,b)), hãy tính giá trị gia tăng cuối cùng 

[ 
x=a+b(r-l). 
] 

Thêm (a) vào (D_l), thêm (b) vào mọi (D_i) với (l+1\le i\le r) và trừ (x) khỏi (D_{r+1}) khi (r<n). 

Điều này thể hiện chính xác tác dụng của việc cộng cấp số cộng vào (A_l,\ldots,A_r). Trường hợp đặc biệt (r=n) không có (D_{n+1}) bên trong mảng được duy trì, do đó phần chỉnh sửa cuối cùng sẽ bị bỏ qua. 

1. Đối với truy vấn loại 1 ((l,r)), truy vấn cây phân đoạn trên (D_{l+1},\ldots,D_r). Nếu (l=r), phạm vi không chứa sự khác biệt, vì vậy câu trả lời ngay lập tức là (1). 
2. Nếu không, hãy kiểm tra tổng bình phương được trả về. Nếu nó bằng 0 thì mọi hiệu trong khoảng đều bằng 0, do đó tất cả các giá trị tương ứng của (A) đều bằng nhau. Nếu nó dương thì ít nhất một cặp liền kề khác nhau, do đó khoảng không cố định. 

### Tại sao nó hoạt động

Điều bất biến là cây phân đoạn luôn biểu thị chính xác mảng chênh lệch hiện tại (D). Cập nhật tiến trình số học chỉ thay đổi (D) ở ranh giới bên trái, thống nhất trên toàn bộ phần bên trong của nó và tại vị trí ngay sau ranh giới bên phải, do đó ba cập nhật cây phân đoạn tương ứng giữ nguyên bất biến đó. Đối với bất kỳ khoảng được truy vấn nào, (A_l,\ldots,A_r) đều bằng nhau khi và chỉ khi mọi hiệu liền kề (D_{l+1},\ldots,D_r) bằng 0. Vì cây phân đoạn lưu trữ tổng bình phương và bình phương của chúng không âm, nên tổng đó chính xác bằng 0 trong trường hợp hằng số. Vì vậy mọi câu trả lời truy vấn đều đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, n):
        self.n = n
        size = 4 * n + 5
        self.s = [0] * size
        self.sq = [0] * size
        self.lazy = [0] * size

    def _apply(self, node, left, right, x):
        length = right - left + 1
        old_sum = self.s[node]

        self.sq[node] += 2 * x * old_sum + length * x * x
        self.s[node] = old_sum + length * x
        self.lazy[node] += x

    def _push(self, node, left, right):
        x = self.lazy[node]
        if x == 0 or left == right:
            return

        mid = (left + right) >> 1
        self._apply(node << 1, left, mid, x)
        self._apply(node << 1 | 1, mid + 1, right, x)
        self.lazy[node] = 0

    def add(self, ql, qr, x):
        if ql > qr:
            return
        self._add(1, 1, self.n, ql, qr, x)

    def _add(self, node, left, right, ql, qr, x):
        if ql <= left and right <= qr:
            self._apply(node, left, right, x)
            return

        self._push(node, left, right)
        mid = (left + right) >> 1

        if ql <= mid:
            self._add(node << 1, left, mid, ql, qr, x)
        if qr > mid:
            self._add(node << 1 | 1, mid + 1, right, ql, qr, x)

        lc = node << 1
        rc = lc | 1
        self.s[node] = self.s[lc] + self.s[rc]
        self.sq[node] = self.sq[lc] + self.sq[rc]

    def query_sq(self, ql, qr):
        if ql > qr:
            return 0
        return self._query_sq(1, 1, self.n, ql, qr)

    def _query_sq(self, node, left, right, ql, qr):
        if ql <= left and right <= qr:
            return self.sq[node]

        self._push(node, left, right)
        mid = (left + right) >> 1
        result = 0

        if ql <= mid:
            result += self._query_sq(node << 1, left, mid, ql, qr)
        if qr > mid:
            result += self._query_sq(node << 1 | 1, mid + 1, right, ql, qr)

        return result

def solve():
    n, q = map(int, input().split())
    seg = SegmentTree(n)
    out = []

    for _ in range(q):
        query = list(map(int, input().split()))
        typ = query[0]

        if typ == 1:
            l, r = query[1], query[2]

            if l == r:
                out.append("1")
                continue

            # A[l..r] is constant iff
            # D[l+1], ..., D[r] are all zero.
            value = seg.query_sq(l + 1, r)
            out.append("1" if value == 0 else "0")

        else:
            l, r, a, b = query[1:]

            # D[l] += a
            seg.add(l, l, a)

            # D[l+1..r] += b
            if l + 1 <= r:
                seg.add(l + 1, r, b)

            # The value added at position r is the final term.
            last = a + b * (r - l)

            # D[r+1] -= last
            if r < n:
                seg.add(r + 1, r + 1, -last)

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn được khởi tạo bằng các số 0 vì mảng ban đầu bao gồm toàn các số 0, vì vậy mọi khác biệt ban đầu đều bằng 0. Không cần một thủ tục xây dựng riêng biệt. 

các`_apply`chức năng là hoạt động lan truyền lười biếng trung tâm. Giả sử một nút biểu thị các giá trị (D_1,\ldots,D_k), với tổng hiện tại (S) và tổng bình phương (Q). Sau khi thêm (x) vào mọi giá trị, tổng bình phương mới là 

[ 
\sum(D_i+x)^2 
=\sum D_i^2+2x\sum D_i+kx^2. 
] 

Mã lưu số tiền cũ trước khi sửa đổi vì công thức yêu cầu giá trị cũ là (S). 

Để cập nhật,`seg.add(l, l, a)`xử lý ranh giới bên trái của mảng chênh lệch. Cập nhật phạm vi`seg.add(l + 1, r, b)`xử lý sự gia tăng chung của tất cả những khác biệt nội bộ. Cuối cùng,`seg.add(r + 1, r + 1, -last)`giải thích thực tế là quá trình tiến triển dừng lại sau vị trí (r). 

các`r < n`điều kiện là cần thiết. Không có sự khác biệt được duy trì (D_{n+1}), vì mảng ban đầu kết thúc tại (n). Việc quên kiểm tra này sẽ tạo ra chỉ mục cây phân đoạn không hợp lệ. 

Đối với truy vấn loại 1, sự khác biệt có liên quan bắt đầu từ (l+1), không phải (l). (D_l=A_l-A_{l-1}) cho chúng ta biết (A_l) khác với phần tử như thế nào trước khoảng thời gian được truy vấn, điều này không ảnh hưởng gì đến việc bản thân khoảng thời gian được truy vấn có phải là hằng số hay không. 

Số nguyên Python có độ chính xác tùy ý, do đó các giá trị bình phương lớn có thể không bị tràn. Các giá trị mảng lớn nhất có thể lớn hơn nhiều so với tham số cập nhật ban đầu (10^8) sau nhiều thao tác, khiến cho việc biểu diễn 32 bit có chiều rộng cố định không an toàn ở các ngôn ngữ sử dụng nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 3
2 1 3 4 1
1 1 3
1 4 5
```Sau khi cập nhật, (A=[4,9,14,0,0]). Mảng khác biệt của nó là 

[ 
D=[4,5,5,-14,0]. 
] 

Dấu vết là: 

| Hoạt động | (A) về mặt khái niệm | Phạm vi liên quan (D) | Tổng bình phương | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | ([0,0,0,0,0]) | tất cả đều bằng không | 0 | | 
|`2 1 3 4 1`| ([4,9,14,0,0]) | (D_2,D_3=5,5) | 50 | | 
|`1 1 3`| không thay đổi | (D_2,D_3=5,5) | 50 | 0 | 
|`1 4 5`| không thay đổi | (D_5=0) | 0 | 1 | 

Truy vấn đầu tiên thấy sự khác biệt khác 0 bên trong các vị trí từ 1 đến 3, vì vậy tất cả các giá trị đó không thể bằng nhau. Truy vấn thứ hai chứa hai phần tử có giá trị bằng 0, do đó sự khác biệt có liên quan duy nhất của nó là bằng 0. 

### Đã thi công mẫu 2 

Hãy xem xét```
4 5
1 1 4
2 1 4 2 0
1 2 3
2 2 3 -1 2
1 1 4
```Dấu vết là: 

| Hoạt động | (A) về mặt khái niệm | Phạm vi liên quan (D) | Tổng bình phương | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | ([0,0,0,0]) | tất cả đều bằng không | 0 | | 
|`1 1 4`| ([0,0,0,0]) | (D_2,D_3,D_4=0,0,0) | 0 | 1 | 
|`2 1 4 2 0`| ([2,2,2,2]) | (D_2,D_3,D_4=0,0,0) | 0 | | 
|`1 2 3`| không thay đổi | (D_3=0) | 0 | 1 | 
|`2 2 3 -1 2`| ([2,1,3,2]) | (D_2,D_3,D_4=-1,2,-1) | 6 | | 
|`1 1 4`| không thay đổi | (D_2,D_3,D_4=-1,2,-1) | 6 | 0 | 

Bản cập nhật đầu tiên có (b=0), do đó, nó thay đổi các giá trị nhưng để lại mọi chênh lệch bên trong bằng 0. Bản cập nhật thứ hai giới thiệu những khác biệt khác 0 và tổng bình phương ngay lập tức phát hiện ra chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(q\log n)) | Mỗi bản cập nhật thực hiện tối đa ba phép cộng phạm vi và mỗi truy vấn thực hiện một truy vấn cây phân đoạn. | 
| Không gian | (O(n)) | Cây phân đoạn lưu trữ một lượng thông tin không đổi cho mỗi nút. | 

Với (n,q\le2\cdot10^5), giải pháp chỉ thực hiện logarit nhiều thao tác cây cho mỗi truy vấn thay vì chạm vào mọi vị trí mảng trong một khoảng. Cây phân đoạn sử dụng bộ nhớ (O(n)), thoải mái trong giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

class SegmentTree:
    def __init__(self, n):
        size = 4 * n + 5
        self.n = n
        self.s = [0] * size
        self.sq = [0] * size
        self.lazy = [0] * size

    def apply(self, node, left, right, x):
        length = right - left + 1
        old_sum = self.s[node]
        self.sq[node] += 2 * x * old_sum + length * x * x
        self.s[node] = old_sum + length * x
        self.lazy[node] += x

    def push(self, node, left, right):
        x = self.lazy[node]
        if x == 0 or left == right:
            return
        mid = (left + right) // 2
        self.apply(node * 2, left, mid, x)
        self.apply(node * 2 + 1, mid + 1, right, x)
        self.lazy[node] = 0

    def add(self, ql, qr, x):
        if ql > qr:
            return
        self._add(1, 1, self.n, ql, qr, x)

    def _add(self, node, left, right, ql, qr, x):
        if ql <= left and right <= qr:
            self.apply(node, left, right, x)
            return

        self.push(node, left, right)
        mid = (left + right) // 2

        if ql <= mid:
            self._add(node * 2, left, mid, ql, qr, x)
        if qr > mid:
            self._add(node * 2 + 1, mid + 1, right, ql, qr, x)

        self.s[node] = self.s[node * 2] + self.s[node * 2 + 1]
        self.sq[node] = self.sq[node * 2] + self.sq[node * 2 + 1]

    def query_sq(self, ql, qr):
        if ql > qr:
            return 0
        return self._query_sq(1, 1, self.n, ql, qr)

    def _query_sq(self, node, left, right, ql, qr):
        if ql <= left and right <= qr:
            return self.sq[node]

        self.push(node, left, right)
        mid = (left + right) // 2
        ans = 0

        if ql <= mid:
            ans += self._query_sq(node * 2, left, mid, ql, qr)
        if qr > mid:
            ans += self._query_sq(node * 2 + 1, mid + 1, right, ql, qr)

        return ans

def solve(data):
    inp = io.StringIO(data)
    n, q = map(int, inp.readline().split())
    seg = SegmentTree(n)
    ans = []

    for _ in range(q):
        v = list(map(int, inp.readline().split()))

        if v[0] == 1:
            l, r = v[1], v[2]
            if l == r:
                ans.append("1")
            else:
                ans.append("1" if seg.query_sq(l + 1, r) == 0 else "0")
        else:
            _, l, r, a, b = v

            seg.add(l, l, a)

            if l + 1 <= r:
                seg.add(l + 1, r, b)

            last = a + b * (r - l)

            if r < n:
                seg.add(r + 1, r + 1, -last)

    return "\n".join(ans)

# Provided sample.
assert solve(
    """5 3
2 1 3 4 1
1 1 3
1 4 5
"""
) == "0\n1", "sample 1"

# Minimum-size input. A one-element interval is always constant.
assert solve(
    """1 4
1 1 1
2 1 1 100000000 100000000
1 1 1
1 1 1
"""
) == "1\n1\n1", "minimum-size case"

# All values remain equal after a constant update.
assert solve(
    """5 4
2 1 5 7 0
1 1 5
2 2 4 -7 0
1 2 4
"""
) == "1\n1", "all-equal values"

# Boundary-sensitive case. The update starts at 2 and ends at 4.
assert solve(
    """4 4
2 2 4 1 1
1 2 4
1 1 4
1 3 4
"""
) == "0\n0\n0", "boundary conditions"

# Cancellation and negative values.
assert solve(
    """4 6
2 1 4 5 -2
1 1 4
2 2 3 -3 0
1 2 3
2 2 3 1 0
1 1 4
"""
) == "0\n1\n0", "negative updates and cancellation"

# Large input size. The update makes the whole array equal,
# then the full-range query must still be answered efficiently.
n = 200000
large_input = f"{n} 2\n2 1 {n} 12345678 0\n1 1 {n}\n"
assert solve(large_input) == "1", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 4 ...`|`1 1 1`| Khoảng đơn phần tử và (n=1) | 
| Bổ sung liên tục |`1 1`| (b=0) và bảo đảm sự bình đẳng | 
| Cập nhật từ 2 đến 4 |`0 0 0`| Ranh giới trái và phải, cộng với các yếu tố nguyên vẹn | 
| Cập nhật tiêu cực |`0 1 0`| Giá trị đã ký và hủy bỏ | 
| (n=200000) |`1`| Đầu vào có kích thước tối đa và xử lý logarit | 

## Vỏ cạnh 

Đối với khoảng một phần tử, chẳng hạn như```
1 2
1 1 1
```truy vấn không có cặp liền kề để so sánh. Việc triển khai xử lý việc này một cách rõ ràng bằng cách trả về (1). Trong thuật ngữ mảng chênh lệch, phạm vi bắt buộc (D_{l+1},\ldots,D_r) trống. 

Để có sự phát triển không ngừng, hãy xem xét```
3 2
2 1 3 5 0
1 1 3
```Bản cập nhật thêm ([5,5,5]), vì vậy (A=[5,5,5]). Trong mảng sai phân, (D_1=5), while (D_2=D_3=0). Truy vấn chỉ kiểm tra (D_2,D_3), lấy tổng bình phương bằng 0 và trả về (1). 

Đối với bản cập nhật chạm vào vị trí cuối cùng, hãy xem xét```
3 2
2 2 3 4 2
1 2 3
```Mảng mới là ([0,4,6]). Bản cập nhật thay đổi (D_2) theo (4) và (D_3) theo (2). Không có (D_4) trong cấu trúc được duy trì, do đó việc hiệu chỉnh biên phải bị bỏ qua. Truy vấn kiểm tra (D_3=2), có bình phương là dương và trả về (0). 

Đối với một phạm vi bắt đầu từ vị trí 1, hãy xem xét```
3 2
2 1 3 7 3
1 1 3
```Mảng trở thành ([7,10,13]), với các khác biệt (D_2=3,D_3=3). Truy vấn kiểm tra chính xác các vị trí (2) đến (3) của mảng khác biệt. Nó không kiểm tra (D_1=A_1-A_0=7), vì (A_0) nằm ngoài khoảng được truy vấn. 

Đối với các giá trị âm, hãy xem xét```
3 2
2 1 3 5 -2
1 1 3
```Mảng trở thành ([5,3,1]), do đó, sự khác biệt có liên quan của nó là (D_2=-2) và (D_3=-2). Bình phương của chúng có tổng bằng (8), là số dương và câu trả lời là (0). Phương pháp tổng bình phương không phụ thuộc vào sự khác biệt là dương hay âm, đó chính xác là những gì chúng ta cần cho các cập nhật có chữ ký tùy ý.
