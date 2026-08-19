---
title: "CF 102268D - Ngày tháng"
description: "Chúng tôi có (t) ngày theo lịch và ngày (x) có thể lưu trữ nhiều nhất (ax) ngày. Mỗi cô gái chỉ được hẹn hò tối đa một lần và cô gái (i) chấp nhận bất kỳ ngày nào từ (li) đến (ri). Hẹn hò với cô ấy góp phần (pi) vào câu trả lời."
date: "2026-08-19T04:18:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102268
codeforces_index: "D"
codeforces_contest_name: "300iq Contest 1"
rating: 0
weight: 102268
solve_time_s: 1010
verified: false
draft: false
---

[CF 102268D - Ngày tháng](https://codeforces.com/problemset/problem/102268/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 16 phút 50 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (t) ngày theo lịch và ngày (x) có thể lưu trữ nhiều nhất (a_x) ngày. Mỗi cô gái chỉ được hẹn hò tối đa một lần và cô gái (i) chấp nhận bất kỳ ngày nào từ (l_i) đến (r_i). Hẹn hò với cô ấy góp phần (p_i) vào câu trả lời. Nhiệm vụ là chọn một tập hợp con các cô gái và ấn định cho mỗi cô gái được chọn một ngày hợp lệ, tôn trọng năng lực sao cho tổng mức độ hài lòng là tối đa. 

Điều kiện cấu trúc bất thường là cả hai điểm cuối đều được sắp xếp: 

[ 
l_1\le l_2\le\cdots\le l_n,\qquad 
r_1\le r_2\le\cdots\le r_n. 
] 

Điều kiện này là chìa khóa để làm cho các ràng buộc khớp có thể nén được. Không có nó, công thức tự nhiên sẽ là một bài toán so khớp có trọng số lớn, quá đắt đối với (n,t\le 300000). 

Các giới hạn dành chỗ cho các phép tính đại khái (O((n+t)\log n)), nhưng không dành cho phép tính bậc hai. Một quy trình kiểm tra mọi cặp nữ đã có (O(n^2)) phép toán, khoảng (9\cdot10^{10}) ở kích thước tối đa. Việc liệt kê tất cả các tập hợp con rõ ràng là không thể, vì có (2^n) trong số chúng. Giải pháp cuối cùng sử dụng tính năng sắp xếp cộng với hai cây phân đoạn lười, cho thời gian (O((n+t)+n\log n)). 

Một số trường hợp cạnh rất dễ xử lý sai. 

Không có dung lượng sẵn có, câu trả lời phải bằng 0:```
1 1
0
1 1 10
```Câu trả lời là`0`. Một giải pháp bất cẩn chỉ kiểm tra xem mỗi khoảng có trống hay không sẽ chọn sai cô gái. 

Một số cô gái có thể có khoảng thời gian giống nhau nhưng năng lực vẫn được chia sẻ. Ví dụ:```
2 1
1
1 1 5
1 1 4
```Câu trả lời là`5`, không`9`. Cả hai cô gái chỉ có thể sử dụng một khe duy nhất có sẵn. 

Kiểm tra từng khoảng thời gian đã chọn một cách độc lập cũng không đủ. Coi như:```
2 2
1 0
1 1 100
1 2 99
```Mỗi cô gái riêng lẻ có một khoảng thời gian chứa ít nhất một đơn vị công suất, nhưng chúng không thể được lên lịch cùng nhau. Cô gái đầu tiên sử dụng ô duy nhất vào ngày (1), trong khi cô gái thứ hai chỉ có thể sử dụng ngày (1) vì ngày (2) không còn sức chứa. Câu trả lời là`100`. Đây chính xác là loại điều kiện tổng thể được định lý Hall nắm bắt. 

Cuối cùng, ranh giới phải được xử lý một cách toàn diện. TRONG```
2 2
0 1
1 2 5
2 2 4
```cả hai cô gái chỉ có thể sử dụng ngày (2), vì vậy chỉ có thể chọn một người và câu trả lời là`5`. Việc vô tình coi một khoảng là nửa mở sẽ làm thay đổi các ràng buộc so khớp. 

Tuyên bố chính thức xác nhận thứ tự điểm cuối và giới hạn (300000) dẫn đến yêu cầu (O(n\log n)). 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê một tập hợp con các cô gái, xác định xem liệu những cô gái đó có thể được chỉ định vào các khung thời gian hẹn hò riêng biệt có sẵn hay không và duy trì mức độ hài lòng tối đa trong số các tập hợp con khả thi. Điều đó đúng vì mọi lựa chọn của các cô gái đều được cân nhắc. Nếu quá trình kiểm tra tính khả thi tự nó chỉ định các khoảng đã chọn cho các vị trí có sẵn thì một tập hợp con có thể đã yêu cầu công việc (O(n+t)), đưa ra (O(2^n(n+t))) trong trường hợp xấu nhất. Ngay cả khi bỏ qua chi phí khả thi, (2^{300000}) tập hợp con cũng không thể được xử lý. 

Ý tưởng tự nhiên tiếp theo là dòng chi phí tối đa. Tạo bản sao năng lực mỗi ngày và kết nối mỗi cô gái với mỗi ngày trong khoảng thời gian của cô ấy. Điều đó mô hình hóa vấn đề một cách chính xác, nhưng biểu đồ có thể chứa các cạnh (\Theta(nt)) và thuật toán luồng hoặc kết hợp chung quá chậm. 

Quan sát hữu ích là các tập hợp khả thi các cô gái tạo thành một matroid. Hãy coi mỗi đơn vị công suất hàng ngày là một khe riêng biệt. Một cô gái có thể được ghép vào bất kỳ vị trí nào tương ứng với một ngày trong khoảng thời gian của cô ấy. Một tập hợp các cô gái sẽ khả thi khi những cô gái này có thể được ghép vào các vị trí khác nhau. Các tập con có thể so khớp như vậy tạo thành một matroid ngang. Định lý tham lam có trọng số cho matroid sau đó nói rằng chúng ta có thể xử lý các cô gái theo thứ tự khoái cảm giảm dần và chấp nhận một cô gái một cách chính xác khi cộng cô ấy giữ cho tập đã chọn khả thi. Đây là bước tối ưu hóa trung tâm. 

Vấn đề còn lại là kiểm tra tính khả thi một cách nhanh chóng. 

Định lý Hall nói rằng một tập hợp đã chọn có thể lập lịch trình chính xác khi mỗi tập hợp các cô gái có ít nhất số ô ngày có sẵn trong tập hợp các ngày được phép của nó bằng số lượng các cô gái được chọn. Bởi vì tất cả các tập hợp được phép đều là các khoảng và cả hai chuỗi điểm cuối đều không giảm nên việc kiểm tra các khối chỉ số nữ liền kề là đủ. Điều này biến điều kiện khớp thành bất đẳng thức đối với tiền tố. 

Đặt (b_i) là (1) nếu cô gái (i) đã được chấp nhận và (0) nếu ngược lại. hãy để 

[ 
A_x=\sum_{j=1}^{x}a_j 
] 

là dung lượng tiền tố và đặt 

[ 
B_x=\sum_{j=1}^{x}b_j 
] 

là số tiền tố của các cô gái được chọn. 

Đối với một khối các cô gái (L,\ldots,R), tất cả các ngày có thể có của họ đều nằm bên trong ([l_L,r_R]). Tình trạng của Hall trở thành 

[ 
B_R-B_{L-1}\le A_{r_R}-A_{l_L-1}. 
] 

Sắp xếp lại mang lại 

[ 
B_R-A_{r_R}\le B_{L-1}-A_{l_L-1}. 
] 

Xác định 

[ 
c_R=B_R-A_{r_R} 
] 

và 

[ 
d_L=B_{L-1}-A_{l_L-1}. 
] 

Toàn bộ tập hợp khả thi được đặc trưng bởi 

[ 
c_R\le d_L\qquad\text{với mọi }L\le R. 
] 

Đây chính xác là mức giảm được sử dụng trong giải pháp đã biết cho vấn đề. 

Giả sử chúng ta đang xem xét cô gái (x). Trước khi chấp nhận cô ấy, (b_x=0). Thêm cô ấy sẽ tăng mọi (B_i) với (i\ge x), do đó mọi (c_i) với (i\ge x) đều tăng thêm (1). Nó cũng chỉ tăng (d_i) khi (i>x), vì (d_i) chứa (B_{i-1}). Do đó, những bất bình đẳng mới duy nhất có thể trở nên khó khăn hơn là những bất bình đẳng có 

[ 
L\le x\le R. 
] 

Đối với những bất đẳng thức đó, (c_R) tăng thêm (1), trong khi (d_L) thì không. Do đó cô gái (x) có thể được chấp nhận chính xác khi 

[ 
\max_{R\ge x}c_R < \min_{L\le x}d_L. 
] 

Sau khi chấp nhận cô ấy, chúng ta thêm (1) vào hậu tố (c_x,\ldots,c_n) và thêm (1) vào hậu tố (d_{x+1},\ldots,d_n). 

Vì vậy, chúng ta cần một cây phân đoạn duy trì các giá trị tối đa của hậu tố với phép cộng phạm vi cho (c) và một cây phân đoạn khác duy trì các giá trị tối thiểu của tiền tố với phép cộng phạm vi cho (d). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n(n+t))) | (O(n+t)) | Quá chậm | 
| Kết hợp / luồng có trọng số | Ít nhất là siêu tuyến tính trong biểu đồ khoảng đầy đủ | Có khả năng (O(nt)) | Quá chậm | 
| Tham lam + hai cây đoạn lười biếng | (O(t+n+n\log n)) | (O(t+n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính dung lượng tiền tố (A_x), trong đó (A_x) là tổng số khe ngày trong ngày (1) đến (x). Điều này cho phép mọi khoảng dung lượng ([u,v]) được đánh giá là (A_v-A_{u-1}) trong thời gian không đổi. 
2. Với mọi cô gái (i), ban đầu không có cô gái nào được chọn nên (B_i=0). cửa hàng 

[ 
c_i=-A_{r_i} 
] 

và 

[ 
d_i=-A_{l_i-1}. 
] 

Đây chính xác là các định nghĩa trước đó với tất cả (b_i=0). 

1. Sắp xếp các cô gái theo mức độ vui vẻ giảm dần. Các tập được chọn độc lập trong một matroid ngang, do đó việc chấp nhận mọi cô gái khả thi theo thứ tự này sẽ tạo ra một tập khả thi có trọng số tối đa. 
2. Hãy xem xét cô gái tiếp theo (x). Truy vấn 

[ 
C=\max_{i\ge x}c_i 
] 

từ cây phân đoạn đầu tiên và 

[ 
D=\min_{i\le x}d_i 
] 

từ cây đoạn thứ hai. 

1. Chấp nhận cô gái (x) nếu (C<D). Bất đẳng thức chặt chẽ là cần thiết vì việc chấp nhận cô ấy sẽ làm tăng mọi (c_i) liên quan lên đúng một. Bất đẳng thức cũ phải có ít nhất một đơn vị độ chùng. 
2. Nếu chấp nhận cô gái (x), hãy thêm (1) vào (c_x,\ldots,c_n). Điều này phản ánh rằng mọi (B_i) với (i\ge x) đều tăng thêm một. 
3. Đồng thời cộng (1) vào (d_{x+1},\ldots,d_n). Giá trị (d_i) chứa (B_{i-1}) nên nó chỉ thay đổi khi (i-1\ge x). Cố tình không có bản cập nhật nào cho (d_x). 
4. Thêm niềm vui của cô gái vào câu trả lời và tiếp tục với cô gái tiếp theo. Nếu cô gái thất bại trong bài kiểm tra, hãy giữ nguyên cả hai cây và đi tiếp. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của thứ tự sắp xếp theo niềm vui, hai cây biểu thị chính xác các giá trị (c_i) và (d_i) cho các cô gái được chấp nhận. Điều kiện cho mọi cặp (L\le R) đã được thỏa mãn. Khi xem xét một cô gái mới, chỉ có sự bất bình đẳng với (L\le x\le R) mới có thể trở nên chặt chẽ hơn. Những bất đẳng thức đó có giá trị sau khi chèn chính xác khi giá trị cũ lớn nhất (c_R) ở bên phải nhỏ hơn giá trị cũ nhỏ nhất (d_L) ở bên trái. Do đó mọi tập hợp được chấp nhận đều khả thi và mọi cô gái bị từ chối sẽ vi phạm điều kiện của Hall nếu được thêm vào. Vì hệ thống khả thi là matroid và các cô gái được xử lý bằng cách giảm trọng lượng nên tập kết quả có tổng khoái cảm tối đa. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

INF = 10**30

class SegmentTree:
    def __init__(self, values, is_max):
        self.n = len(values)
        self.is_max = is_max
        size = 4 * self.n + 5
        self.val = array('q', [0]) * size
        self.tag = array('q', [0]) * size
        self.values = values
        self._build(1, 0, self.n - 1)

    def _merge(self, x, y):
        if self.is_max:
            return x if x > y else y
        return x if x < y else y

    def _build(self, v, l, r):
        if l == r:
            self.val[v] = self.values[l]
            return
        m = (l + r) >> 1
        self._build(v << 1, l, m)
        self._build(v << 1 | 1, m + 1, r)
        self.val[v] = self._merge(
            self.val[v << 1],
            self.val[v << 1 | 1]
        )

    def update_suffix(self, pos, delta=1):
        if pos >= self.n:
            return
        self._update(1, 0, self.n - 1, pos, delta)

    def _update(self, v, l, r, pos, delta):
        if pos <= l:
            self.val[v] += delta
            self.tag[v] += delta
            return

        m = (l + r) >> 1

        if pos <= m:
            self._update(v << 1, l, m, pos, delta)
        self._update(v << 1 | 1, m + 1, r, pos, delta)

        self.val[v] = self._merge(
            self.val[v << 1],
            self.val[v << 1 | 1]
        ) + self.tag[v]

    def query_suffix(self, pos):
        if pos >= self.n:
            return -INF if self.is_max else INF
        return self._query_suffix(1, 0, self.n - 1, pos)

    def _query_suffix(self, v, l, r, pos):
        if pos <= l:
            return self.val[v]

        m = (l + r) >> 1

        if pos <= m:
            left = self._query_suffix(v << 1, l, m, pos)
            right = self.val[v << 1 | 1]
            return self._merge(left, right) + self.tag[v]

        return self._query_suffix(v << 1 | 1, m + 1, r, pos) + self.tag[v]

    def query_prefix(self, pos):
        if pos < 0:
            return -INF if self.is_max else INF
        if pos >= self.n - 1:
            return self.val[1]
        return self._query_prefix(1, 0, self.n - 1, pos)

    def _query_prefix(self, v, l, r, pos):
        if r <= pos:
            return self.val[v]

        m = (l + r) >> 1

        if pos <= m:
            return self._query_prefix(v << 1, l, m, pos) + self.tag[v]

        left = self.val[v << 1]
        right = self._query_prefix(v << 1 | 1, m + 1, r, pos)
        return self._merge(left, right) + self.tag[v]

def solve():
    n, t = map(int, input().split())

    pref = [0] * (t + 1)
    cur = 0
    a = list(map(int, input().split()))

    for i, x in enumerate(a, 1):
        cur += x
        pref[i] = cur

    order = [None] * n
    c = [0] * n
    d = [0] * n

    for i in range(n):
        l, r, p = map(int, input().split())
        order[i] = (p, i)
        c[i] = -pref[r]
        d[i] = -pref[l - 1]

    order.sort(reverse=True)

    tree_c = SegmentTree(c, True)
    tree_d = SegmentTree(d, False)

    del c
    del d
    del pref
    del a

    ans = 0

    for p, x in order:
        right_c = tree_c.query_suffix(x)
        left_d = tree_d.query_prefix(x)

        if right_c < left_d:
            tree_c.update_suffix(x)
            tree_d.update_suffix(x + 1)
            ans += p

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng tiền tố được xây dựng trước tiên vì (c_i) và (d_i) ban đầu của mỗi cô gái chỉ phụ thuộc vào tổng dung lượng ở bên phải hoặc ngay trước điểm cuối bên trái của cô ấy. Khi các giá trị này đã được khởi tạo, (l_i) và (r_i) ban đầu không còn cần thiết nữa. 

các`order`chỉ lưu trữ mảng`(pleasure, index)`. Sắp xếp ngược lại sẽ làm giảm niềm vui, đó chính xác là thứ tự được yêu cầu bởi thuật toán tham lam matroid. Các số nguyên Python có độ chính xác tùy ý, do đó, tổng dung lượng tiền tố và niềm vui không có nguy cơ tràn 32 bit. 

Cây phân đoạn sử dụng kiểu lan truyền lười biếng hơi khác thường.`val[v]`đã bao gồm bản cập nhật lười biếng thuộc về nút (v), trong khi`tag[v]`ghi lại số tiền chưa được đưa vào con cái. Khi giảm xuống thành con, thẻ cha mẹ sẽ được thêm vào giá trị trả về của con. Khi xây dựng lại cha mẹ, giá trị con đã hợp nhất sẽ tăng lên theo thẻ của cha mẹ. Điều này tránh được thao tác đẩy rõ ràng và làm cho các bản cập nhật hậu tố trở nên đặc biệt nhỏ gọn. 

Cây đầu tiên là cấu trúc cộng phạm vi, phạm vi tối đa cho (c). Thứ hai là cấu trúc thêm phạm vi, phạm vi tối thiểu cho (d). Bài kiểm tra ứng viên truy vấn chính xác hai phạm vi xuất hiện trong 

[ 
\max_{R\ge x}c_R < \min_{L\le x}d_L. 
] 

Sau khi được chấp nhận, cập nhật hậu tố trên (c) bắt đầu tại (x), trong khi cập nhật hậu tố trên (d) bắt đầu tại (x+1). Sự khác biệt một chỉ số đó là cần thiết và là lỗi rất có thể xảy ra trong quá trình triển khai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào mẫu thực tế là:```
3 5
0 1 0 1 0
1 2 2
2 4 1
3 5 5
```Dung lượng tiền tố là 

[ 
A=[0,0,1,1,2,2]. 
] 

(c_i=-A_{r_i}) và (d_i=-A_{l_i-1}) ban đầu là: 

[ 
c=[-1,-2,-2], 
\qquad 
d=[0,0,-1]. 
] 

Các cô gái được xem xét theo thứ tự vui vẻ (3,1,2). 

| Cô Gái | Niềm vui | (x) | (\max c[x..]) | (\min d[..x]) | Quyết định | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 5 | 2 | (-2) | (-1) | Chấp nhận | 5 | 
| 1 | 2 | 0 | (-1) | (0) | Chấp nhận | 7 | 
| 2 | 1 | 1 | (0) | (0) | Từ chối | 7 | 

Đối với bé gái (3), (-2<-1), nên có đủ độ chùng để nhét bé vào. Sau khi chấp nhận cô ấy, (c_3) tăng lên một. Cô gái (1) cũng vừa vặn. Khi xét cô gái (2), hai vế bằng nhau, do đó việc chèn cô ấy vào sẽ làm cho một số bất đẳng thức Hall sai đi một đơn vị. Câu trả lời cuối cùng là`7`. Điều này chứng tỏ tại sao việc kiểm tra chấp nhận lại nghiêm ngặt. 

### Ví dụ về ranh giới nặng 

Hãy xem xét:```
2 2
1 0
1 1 100
1 2 99
```Dung lượng tiền tố là (A=[0,1,1]). Ban đầu, 

[ 
c=[-1,-1],\qquad d=[0,0]. 
] 

| Cô Gái | Niềm vui | (x) | (\max c[x..]) | (\min d[..x]) | Quyết định | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 100 | 0 | (-1) | (0) | Chấp nhận | 100 | 
| 2 | 99 | 1 | (0) | (0) | Từ chối | 100 | 

Sau khi chấp nhận cô gái (1), slot dung lượng duy nhất sẽ được sử dụng. Cô gái (2) trùng với năng lực hiệu quả tương tự vì ngày (2) có năng lực bằng 0, do đó ứng viên thứ hai thất bại với tỷ số bằng nhau. Đầu ra là`100`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(t+n+n\log n)) | Tổng tiền tố lấy (O(t)), sắp xếp lấy (O(n\log n)) và mỗi cô gái thực hiện một số lượng không đổi các thao tác trên cây phân đoạn (O(\log n)) | 
| Không gian | (O(t+n)) | Dung lượng tiền tố, ứng viên được sắp xếp và cây hai phân đoạn đều sử dụng bộ nhớ tuyến tính | 

Với (n,t\le300000), số hạng chiếm ưu thế là (O(n\log n)), phù hợp với mục tiêu hai giây. Cây phân đoạn sử dụng mảng 64-bit nhỏ gọn trong quá trình triển khai Python thay vì mảng số nguyên Python thông thường, giúp kiểm soát việc sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

input = sys.stdin.readline

INF = 10**30

class SegmentTree:
    def __init__(self, values, is_max):
        self.n = len(values)
        self.is_max = is_max
        size = 4 * self.n + 5
        self.val = array('q', [0]) * size
        self.tag = array('q', [0]) * size
        self.values = values
        self._build(1, 0, self.n - 1)

    def _merge(self, x, y):
        if self.is_max:
            return x if x > y else y
        return x if x < y else y

    def _build(self, v, l, r):
        if l == r:
            self.val[v] = self.values[l]
            return
        m = (l + r) >> 1
        self._build(v << 1, l, m)
        self._build(v << 1 | 1, m + 1, r)
        self.val[v] = self._merge(
            self.val[v << 1],
            self.val[v << 1 | 1]
        )

    def update_suffix(self, pos):
        if pos >= self.n:
            return
        self._update(1, 0, self.n - 1, pos)

    def _update(self, v, l, r, pos):
        if pos <= l:
            self.val[v] += 1
            self.tag[v] += 1
            return

        m = (l + r) >> 1

        if pos <= m:
            self._update(v << 1, l, m, pos)
        self._update(v << 1 | 1, m + 1, r, pos)

        self.val[v] = self._merge(
            self.val[v << 1],
            self.val[v << 1 | 1]
        ) + self.tag[v]

    def query_suffix(self, pos):
        if pos >= self.n:
            return -INF if self.is_max else INF
        return self._query_suffix(1, 0, self.n - 1, pos)

    def _query_suffix(self, v, l, r, pos):
        if pos <= l:
            return self.val[v]

        m = (l + r) >> 1

        if pos <= m:
            left = self._query_suffix(v << 1, l, m, pos)
            right = self.val[v << 1 | 1]
            return self._merge(left, right) + self.tag[v]

        return self._query_suffix(v << 1 | 1, m + 1, r, pos) + self.tag[v]

    def query_prefix(self, pos):
        if pos < 0:
            return -INF if self.is_max else INF
        if pos >= self.n - 1:
            return self.val[1]
        return self._query_prefix(1, 0, self.n - 1, pos)

    def _query_prefix(self, v, l, r, pos):
        if r <= pos:
            return self.val[v]

        m = (l + r) >> 1

        if pos <= m:
            return self._query_prefix(v << 1, l, m, pos) + self.tag[v]

        left = self.val[v << 1]
        right = self._query_prefix(v << 1 | 1, m + 1, r, pos)
        return self._merge(left, right) + self.tag[v]

def solve_case(inp):
    data = iter(inp.split())
    n = int(next(data))
    t = int(next(data))

    pref = [0] * (t + 1)
    for i in range(1, t + 1):
        pref[i] = pref[i - 1] + int(next(data))

    order = [None] * n
    c = [0] * n
    d = [0] * n

    for i in range(n):
        l = int(next(data))
        r = int(next(data))
        p = int(next(data))
        order[i] = (p, i)
        c[i] = -pref[r]
        d[i] = -pref[l - 1]

    order.sort(reverse=True)

    tc = SegmentTree(c, True)
    td = SegmentTree(d, False)

    ans = 0

    for p, x in order:
        if tc.query_suffix(x) < td.query_prefix(x):
            tc.update_suffix(x)
            td.update_suffix(x + 1)
            ans += p

    return str(ans)

def run(inp: str) -> str:
    return solve_case(inp)

# Provided sample.
assert run("""\
3 5
0 1 0 1 0
1 2 2
2 4 1
3 5 5
""") == "7", "sample 1"

# Minimum-size case with zero capacity.
assert run("""\
1 1
0
1 1 5
""") == "0", "minimum size and zero capacity"

# All girls have the same interval and compete for one slot.
assert run("""\
2 1
1
1 1 5
1 1 4
""") == "5", "shared single capacity"

# Boundary case where day 2 has no capacity.
assert run("""\
2 2
1 0
1 1 100
1 2 99
""") == "100", "Hall constraint across a boundary"

# All values equal, with enough total capacity for every girl.
assert run("""\
4 2
2 2
1 2 10
1 2 10
1 2 10
1 2 10
""") == "40", "all equal values"

# Maximum-size construction.
n = 300000
parts = [f"{n} {n}\n", ("1 " * (n - 1)) + "1\n"]
for i in range(1, n + 1):
    parts.append(f"{i} {i} 1\n")

large_input = "".join(parts)
assert run(large_input) == "300000", "maximum-size instance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0 / 1 1 5`|`0`| Kích thước tối thiểu và công suất bằng không | 
|`2 1 / 1 / 1 1 5 / 1 1 4`|`5`| Nhiều cô gái tranh giành một vị trí | 
|`2 2 / 1 0 / 1 1 100 / 1 2 99`|`100`| Ràng buộc Global Hall và xử lý điểm cuối | 
|`4 2 / 2 2 / four [1,2] intervals`|`40`| Khoảng cách bằng nhau, niềm vui bằng nhau, đủ năng lực | 
| (n=t=300000), công suất đơn vị và khoảng ([i,i]) |`300000`| Kích thước đầu vào tối đa và hành vi bộ nhớ tuyến tính | 

## Vỏ cạnh 

Khi mọi công suất bằng 0 thì mọi (c_i) và (d_i) ban đầu đều bằng 0. Đối với một cô gái ở vị trí (x), bài kiểm tra so sánh hậu tố tối đa bằng 0 với tiền tố tối thiểu bằng 0. Vì (0<0) là sai nên không ai được chấp nhận. Vì```
1 1
0
1 1 10
```thuật toán trả về`0`, đúng như yêu cầu. 

Khi một ngày có nhiều cô gái chia sẻ, người đầu tiên có thể được chấp nhận, nhưng người tiếp theo phải thất bại. Vì```
2 1
1
1 1 5
1 1 4
```ứng viên đầu tiên thỏa mãn (-1<0). Sau khi chấp nhận cô ấy, giá trị (c) liên quan trở thành 0. Ứng viên thứ hai sau đó coi (0<0) là sai và bị từ chối. Kết quả là`5`. 

Ràng buộc Hall toàn cục xuất hiện trong```
2 2
1 0
1 1 100
1 2 99
```Sau khi chấp nhận cô gái đầu tiên, ứng viên thứ hai có (x=2) chỉ mục dựa trên một. Giá trị tối đa bên phải (c) và giá trị tối thiểu bên trái (d) bằng nhau, do đó ứng viên bị loại. Cây phân đoạn đang phát hiện ra rằng cả hai cô gái đều yêu cầu nhiều công suất hơn số ngày có thể cung cấp, mặc dù mỗi cô gái đều có vẻ khả thi. 

Đối với các khoảng thời gian bằng nhau, thuật toán không dựa vào các điểm cuối riêng biệt. Hãy xem xét bốn cô gái với khoảng cách ([1,2]), năng lực (2,2) và niềm vui (10) mỗi cô gái. Mọi ứng cử viên đều vượt qua vì có chính xác bốn vị trí trống. Cập nhật hậu tố sẽ tích lũy chính xác bốn cô gái được chấp nhận và câu trả lời sẽ trở thành`40`. 

Tại ranh giới bên phải, việc chấp nhận cô gái cuối cùng không được cập nhật (d_n), vì (d_n) chứa (B_{n-1}), không chứa (B_n). Đây là lý do tại sao việc triển khai thực hiện`tree_d.update_suffix(x + 1)`còn hơn là`tree_d.update_suffix(x)`. Đối với cô gái ở vị trí cuối cùng, bản cập nhật này bị bỏ qua hoàn toàn. Ranh giới đó là yếu tố làm cho biểu thức tiền tố (B_{L-1}-A_{l_L-1}) thẳng hàng với bất đẳng thức Hall. 

Trường hợp kích thước tối đa có (300000) bé gái và (300000) ngày, với một đơn vị công suất mỗi ngày và bé gái (i) bị giới hạn trong ngày (i). Mỗi cô gái đều có thể sắp xếp lịch trình một cách độc lập, vì vậy tất cả (300000) cô gái đều được chấp nhận và câu trả lời là`300000`. Thuật toán xử lý việc này trong thời gian (O(n\log n)) trong khi vẫn giữ tuyến tính lưu trữ cây phân đoạn.
