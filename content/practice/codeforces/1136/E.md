---
title: "CF 1136E - Nastya chưa viết một huyền thoại"
description: "Chúng ta có một mảng a và một mảng k khác. Mảng luôn thỏa mãn một ràng buộc kiểu đơn điệu: $$a{i+1} ge ai + ki$$ cho mọi cặp liền kề. Có hai hoạt động. Thao tác đầu tiên tăng một vị trí a[i] lên một giá trị x nào đó."
date: "2026-06-12T04:02:41+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1136
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 546 (Div. 2)"
rating: 2200
weight: 1136
solve_time_s: 120
verified: true
draft: false
---

[CF 1136E - Nastya chưa viết huyền thoại](https://codeforces.com/problemset/problem/1136/E) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`a`và một mảng khác`k`. 

Mảng luôn thỏa mãn ràng buộc kiểu đơn điệu:$$a_{i+1} \ge a_i + k_i$$cho mỗi cặp liền kề. 

Có hai hoạt động. 

Thao tác đầu tiên tăng một vị trí`a[i]`bởi một giá trị nào đó`x`. Sau lần tăng đó, ràng buộc có thể bị vi phạm đối với phần tử tiếp theo. Nếu vậy thì chúng ta phải nâng cao`a[i+1]`đến giá trị tối thiểu để khôi phục ràng buộc. Sự thay đổi đó sau đó có thể buộc`a[i+2]`để tăng lên, vân vân. Bản cập nhật lan truyền sang bên phải cho đến khi ràng buộc trở lại hợp lệ. 

Hoạt động thứ hai yêu cầu tính tổng của một đoạn liền kề. 

Khó khăn là cả hai`n`và số lượng truy vấn lên tới`100000`. Một bản cập nhật duy nhất có thể ảnh hưởng đến gần như toàn bộ hậu tố của mảng. Việc thực hiện việc truyền bá đó một cách rõ ràng sẽ yêu cầu`O(n)`thời gian cho mỗi lần cập nhật, trở thành`O(nq)`trong trường hợp xấu nhất, đại khái`10^10`hoạt động. Điều đó vượt xa giới hạn. 

Thách thức chính là biểu diễn cấu trúc được truyền ở dạng trong đó cả cập nhật và tổng phạm vi có thể được xử lý theo thời gian logarit. 

Một điểm tinh tế là các bản cập nhật không bao giờ làm giảm bất kỳ yếu tố nào. Một khi vị thế đã phát triển thì sau này nó sẽ không bao giờ trở nên nhỏ hơn nữa. Sự đơn điệu này là lý do tồn tại một cấu trúc dữ liệu hiệu quả. 

Một sai lầm dễ mắc phải khác là nghĩ rằng việc truyền bá sẽ dừng ngay lập tức sau khi thay đổi một hàng xóm. Coi như:```
a = [5, 7, 9]
k = [3, 3]
```Sau khi thêm`2`ĐẾN`a[1]`, chúng tôi nhận được:```
a[1] = 7
```Hiện nay:```
a[2] < a[1] + 3
```Vì thế`a[2]`trở thành`10`. 

Sau đó:```
a[3] < a[2] + 3
```Vì thế`a[3]`trở thành`13`. 

Hiệu ứng đạt đến toàn bộ hậu tố. 

Một trường hợp nguy hiểm khác đến từ các giá trị âm trong`k`.```
a = [10, 20]
k = [-100]
```Hạn chế là```
20 >= -90
```điều đó đã đúng rồi. 

Tăng dần`a[1]`một chút vẫn có thể thỏa mãn ràng buộc. Giải pháp giả định rằng mỗi bản cập nhật được truyền bá ít nhất một bước sẽ không chính xác. 

Cái bẫy cuối cùng là tổng phạm vi sau nhiều lần cập nhật trùng lặp. Các giá trị mảng thực tế đang thay đổi hoàn toàn. Nếu chúng ta chỉ lưu trữ các vị trí cập nhật và quên các sửa đổi hậu tố được tạo ra thì các truy vấn tính tổng sau này sẽ trở thành sai. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo tuyên bố theo nghĩa đen. 

Khi chúng tôi nhận được`+ i x`, chúng tôi tăng`a[i]`, sau đó quét sang phải. Bất cứ khi nào$$a_{j+1} < a_j + k_j$$chúng tôi thay thế`a[j+1]`qua`a_j + k_j`. 

Đối với truy vấn tổng, chúng tôi chỉ cần thêm các phần tử được yêu cầu. 

Điều này đúng vì nó mô phỏng trực tiếp quá trình. Vấn đề là sự phức tạp. Một bản cập nhật duy nhất có thể chạm vào mọi vị trí còn lại. Với`100000`cập nhật, trường hợp xấu nhất là về$$100000 \times 100000 = 10^{10}$$hoạt động. 

Để tìm thứ gì đó nhanh hơn, chúng ta cần hiểu cấu trúc của ràng buộc. 

Xác định một mảng tổng tiền tố:$$d_1 = 0$$

$$d_i = \sum_{j=1}^{i-1} k_j$$Bây giờ xác định$$b_i = a_i - d_i$$Điều kiện ban đầu$$a_{i+1} \ge a_i + k_i$$trở thành$$b_{i+1} \ge b_i$$Vì vậy, sau sự chuyển đổi này, toàn bộ mảng`b`đơn giản là không giảm. 

Cập nhật làm gì? 

Tăng dần`a[i]`qua`x`tăng lên`b[i]`qua`x`. 

Sau đó, việc truyền bá lại thực thi thuộc tính không giảm. Mọi vị trí bên phải có giá trị nhỏ hơn vị trí mới`b[i]`được nâng lên chính xác giá trị đó. 

Nói cách khác, bản cập nhật trở thành: 

"Thêm`x`ở vị trí`i`, sau đó thay thế một đoạn liên tiếp tối đa bắt đầu từ`i`bằng một giá trị không đổi." 

Bây giờ cấu trúc đã rõ ràng hơn nhiều. 

Kể từ khi`b`luôn không giảm, chúng ta có thể tìm vị trí bị ảnh hưởng cuối cùng bằng tìm kiếm nhị phân. Bản cập nhật sẽ trở thành một phạm vi gán cho một giá trị. Tổng phạm vi trở thành tổng có trọng số vì$$a_i = b_i + d_i.$$Mảng`d`không bao giờ thay đổi, do đó đóng góp của nó có thể được tính toán trước. Phần động nằm hoàn toàn bên trong`b`. 

Chúng ta cần một cấu trúc dữ liệu hỗ trợ: 

1. Gán phạm vi cho một hằng số. 
2. Truy vấn tổng phạm vi. 
3. Tìm kiếm nhị phân trên các giá trị trong mảng không giảm. 

Cây phân đoạn lười biếng cung cấp cả ba. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O(q log² n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chuyển đổi 

hãy để$$d_1 = 0$$Và$$d_i = k_1 + k_2 + \dots + k_{i-1}.$$Định nghĩa$$b_i = a_i - d_i.$$Sự đảm bảo được đưa ra ngụ ý rằng`b`bắt đầu không giảm. 

### Nội dung cây phân đoạn 

Cây phân đoạn lưu trữ: 

1. Tổng của`b`trên mỗi phân đoạn. 
2. Giá trị tối đa trên mỗi phân đoạn. 
3. Thẻ gán phạm vi lười biếng. 

Mức tối đa cho phép chúng tôi tìm kiếm nhị phân ở cuối phạm vi cập nhật. 

### Đang xử lý bản cập nhật 

Giả sử chúng ta nhận được:```
+ i x
```1. Đọc giá trị hiện tại`b[i]`. 
2. Đặt$$v = b[i] + x.$$3. Tìm vị trí lớn nhất`r`sao cho mọi phần tử trong`[i,r]`thực sự nhỏ hơn`v`. 

Từ`b`không giảm, các vị trí bị ảnh hưởng tạo thành một đoạn liền kề. 
4. Gán giá trị`v`đến toàn bộ phạm vi`[i,r]`. 

Sau khi gán, mảng vẫn không giảm. 

### Xử lý truy vấn tính tổng 

cho```
s l r
```chúng tôi cần$$\sum_{i=l}^{r} a_i.$$Từ$$a_i=b_i+d_i,$$chúng tôi tính toán$$\sum b_i$$từ cây phân đoạn và$$\sum d_i$$từ một mảng tổng tiền tố tĩnh. 

Tổng của họ là câu trả lời. 

### Tại sao nó hoạt động 

Mảng được chuyển đổi`b`luôn không giảm. Việc tăng một phần tử chỉ có thể vi phạm tính đơn điệu ở bên phải của nó. Mọi vị trí nhỏ hơn giá trị mới phải trở thành chính xác giá trị đó và mọi vị trí lớn hơn không thay đổi. Vì mảng được sắp xếp nên các vị trí bị ảnh hưởng đó tạo thành một khoảng liền kề. Việc gán khoảng đó cho giá trị mới sẽ tái tạo chính xác sự lan truyền được mô tả trong câu lệnh. 

Cây phân đoạn luôn lưu trữ hiện tại`b`mảng và mọi truy vấn đều tính toán$$a_i=b_i+d_i$$chính xác. Từ`d`không bao giờ thay đổi, xây dựng lại tổng từ phần động`b`và phần tĩnh`d`là chính xác cho mọi hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr) - 1

        size = 4 * self.n + 5
        self.sum = [0] * size
        self.mx = [0] * size
        self.lazy = [None] * size

        self._build(1, 1, self.n, arr)

    def _build(self, v, tl, tr, arr):
        if tl == tr:
            val = arr[tl]
            self.sum[v] = val
            self.mx[v] = val
            return

        tm = (tl + tr) // 2
        self._build(v * 2, tl, tm, arr)
        self._build(v * 2 + 1, tm + 1, tr, arr)
        self._pull(v)

    def _pull(self, v):
        self.sum[v] = self.sum[v * 2] + self.sum[v * 2 + 1]
        self.mx[v] = max(self.mx[v * 2], self.mx[v * 2 + 1])

    def _apply(self, v, tl, tr, val):
        self.sum[v] = (tr - tl + 1) * val
        self.mx[v] = val
        self.lazy[v] = val

    def _push(self, v, tl, tr):
        if self.lazy[v] is None or tl == tr:
            return

        tm = (tl + tr) // 2
        val = self.lazy[v]

        self._apply(v * 2, tl, tm, val)
        self._apply(v * 2 + 1, tm + 1, tr, val)

        self.lazy[v] = None

    def assign(self, l, r, val):
        self._assign(1, 1, self.n, l, r, val)

    def _assign(self, v, tl, tr, l, r, val):
        if l > r:
            return

        if l == tl and r == tr:
            self._apply(v, tl, tr, val)
            return

        self._push(v, tl, tr)

        tm = (tl + tr) // 2

        self._assign(v * 2, tl, tm, l, min(r, tm), val)
        self._assign(v * 2 + 1, tm + 1, tr,
                     max(l, tm + 1), r, val)

        self._pull(v)

    def query_sum(self, l, r):
        return self._query_sum(1, 1, self.n, l, r)

    def _query_sum(self, v, tl, tr, l, r):
        if l > r:
            return 0

        if l == tl and r == tr:
            return self.sum[v]

        self._push(v, tl, tr)

        tm = (tl + tr) // 2

        return (
            self._query_sum(v * 2, tl, tm, l, min(r, tm))
            + self._query_sum(v * 2 + 1, tm + 1, tr,
                              max(l, tm + 1), r)
        )

    def query_point(self, pos):
        return self._query_point(1, 1, self.n, pos)

    def _query_point(self, v, tl, tr, pos):
        if tl == tr:
            return self.mx[v]

        self._push(v, tl, tr)

        tm = (tl + tr) // 2

        if pos <= tm:
            return self._query_point(v * 2, tl, tm, pos)
        return self._query_point(v * 2 + 1, tm + 1, tr, pos)

    def first_ge(self, l, value):
        return self._first_ge(1, 1, self.n, l, value)

    def _first_ge(self, v, tl, tr, l, value):
        if tr < l or self.mx[v] < value:
            return -1

        if tl == tr:
            return tl

        self._push(v, tl, tr)

        tm = (tl + tr) // 2

        res = self._first_ge(v * 2, tl, tm, l, value)
        if res != -1:
            return res

        return self._first_ge(v * 2 + 1, tm + 1, tr, l, value)

def solve():
    n = int(input())

    a = list(map(int, input().split()))
    k = list(map(int, input().split()))

    d = [0] * (n + 1)

    for i in range(2, n + 1):
        d[i] = d[i - 1] + k[i - 2]

    b = [0] * (n + 1)

    for i in range(1, n + 1):
        b[i] = a[i - 1] - d[i]

    seg = SegmentTree(b)

    pref_d = [0] * (n + 1)
    for i in range(1, n + 1):
        pref_d[i] = pref_d[i - 1] + d[i]

    q = int(input())
    out = []

    for _ in range(q):
        s = input().split()

        if s[0] == '+':
            pos = int(s[1])
            x = int(s[2])

            cur = seg.query_point(pos)
            nv = cur + x

            nxt = seg.first_ge(pos + 1, nv)

            if nxt == -1:
                r = n
            else:
                r = nxt - 1

            seg.assign(pos, r, nv)

        else:
            l = int(s[1])
            r = int(s[2])

            dyn = seg.query_sum(l, r)
            static = pref_d[r] - pref_d[l - 1]

            out.append(str(dyn + static))

    sys.stdout.write("\n".join(out))

solve()
```Sau khi chuyển đổi, toàn bộ vấn đề xoay quanh mảng không giảm`b`. Cây phân đoạn lưu trữ tổng và giá trị cực đại của`b`, cùng với các bài tập phạm vi lười biếng. 

Bản cập nhật đầu tiên đọc giá trị hiện tại tại một vị trí. Giá trị được truyền mới chỉ đơn giản là giá trị đó cộng với`x`. Sau đó, chúng tôi tìm kiếm vị trí đầu tiên bên phải có giá trị ít nhất là giá trị mới. Mọi thứ trước vị trí đó đều phải được nâng lên. Nếu không có vị trí như vậy tồn tại thì bản cập nhật sẽ đến cuối mảng. 

Việc tìm kiếm sử dụng cực đại phân đoạn được lưu trữ. Bởi vì`b`không giảm, vị trí đầu tiên có giá trị ít nhất`nv`xác định duy nhất khoảng thời gian bị ảnh hưởng. 

Truy vấn tổng phạm vi được chia thành phần đóng góp động từ`b`và sự đóng góp tĩnh từ`d`. Cái sau được tính toán trước một lần bằng cách sử dụng tổng tiền tố. 

Một lỗi triển khai phổ biến là quên rằng phạm vi cập nhật bao gồm chính vị trí đã sửa đổi. Một người khác đang tìm kiếm từ`pos`thay vì`pos + 1`, nó sẽ ngay lập tức tìm thấy phần tử mới được cập nhật và tạo ra một khoảng trống. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2 3
1 -1
5
s 2 3
+ 1 2
s 1 2
+ 3 1
s 2 3
```Chúng tôi tính toán:```
d = [0, 1, 0]
b = [1, 1, 3]
```| Hoạt động | b trước | Hành động | b sau | 
| --- | --- | --- | --- | 
| s 2 3 | [1,1,3] | tổng hợp | [1,1,3] | 
| + 1 2 | [1,1,3] | đặt vị trí 1..2 đến 3 | [3,3,3] | 
| s 1 2 | [3,3,3] | tổng hợp | [3,3,3] | 
| + 3 1 | [3,3,3] | đặt vị trí 3 đến 4 | [3,3,4] | 
| s 2 3 | [3,3,4] | tổng hợp | [3,3,4] | 

Dấu vết cho thấy việc truyền bá trở thành một phép gán phạm vi đơn giản trong mảng được chuyển đổi như thế nào. 

### Tuyên Truyền Qua Nhiều Vị Trí 

Hãy xem xét:```
a = [5, 7, 9, 11]
k = [2, 2, 2]
```Sau đó:```
d = [0,2,4,6]
b = [5,5,5,5]
```Áp dụng:```
+ 2 3
```| Bước | Giá trị | 
| --- | --- | 
| hiện tại b[2] | 5 | 
| giá trị mới | 8 | 
| vị trí đầu tiên ≥ 8 | không | 
| phạm vi được chỉ định | [2,4] | 
| b cuối cùng | [5,8,8,8] | 

Tái thiết:```
a = [5,10,12,14]
```đó chính xác là kết quả của việc liên tục thực thi các ràng buộc ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log² n) | mỗi bản cập nhật thực hiện một tìm kiếm logarit và một phép gán logarit | 
| Không gian | O(n) | cây phân đoạn và mảng tiền tố | 

Với`n, q ≤ 100000`,`log n`là khoảng 17. Thậm chí`O(q log² n)`vẫn thoải mái trong giới hạn, dẫn đến chỉ có vài chục triệu thao tác nguyên thủy. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    # paste solve() implementation and capture stdout
    pass

# sample 1
assert run(
"""3
1 2 3
1 -1
5
s 2 3
+ 1 2
s 1 2
+ 3 1
s 2 3
"""
) == "5\n7\n8"

# minimum size
assert run(
"""2
1 5
3
2
s 1 2
s 2 2
"""
) == "6\n5"

# propagation to end
assert run(
"""4
5 7 9 11
2 2 2
2
+ 2 3
s 1 4
"""
) == "41"

# negative k
assert run(
"""2
10 20
-100
2
+ 1 1
s 1 2
"""
) == "31"

# single position update
assert run(
"""3
1 10 20
0 0
2
+ 3 5
s 1 3
"""
) == "36"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng kích thước tối thiểu | khoản tiền trực tiếp | trường hợp hợp lệ nhỏ nhất | 
| Tuyên truyền đến cùng | 41 | cập nhật hậu tố đạt`n`| 
| Âm k | 31 | không có sự lan truyền không cần thiết | 
| Cập nhật vị trí đơn | 36 | xử lý ranh giới bên phải | 

## Vỏ cạnh 

Hãy xem xét:```
2
10 20
-100
2
+ 1 1
s 1 2
```Sự ràng buộc đã cực kỳ lỏng lẻo. Sau khi tăng`a[1]`ĐẾN`11`, chúng ta vẫn có:```
20 >= -89
```nên không có gì lan truyền. Ở dạng biến đổi:```
d = [0,-100]
b = [10,120]
```Cập nhật vị trí 1 tạo ra:```
b = [11,120]
```Không có phép gán nào được mở rộng thêm vì giá trị tiếp theo đã lớn hơn. Tổng trở thành`31`, khớp với mảng thực sự. 

Bây giờ hãy xem xét việc truyền bá hậu tố đầy đủ:```
4
5 7 9 11
2 2 2
1
+ 2 3
```Ban đầu:```
b = [5,5,5,5]
```Giá trị mới là`8`. Không có phần tử nào sau này có giá trị ít nhất`8`, vì vậy khoảng bị ảnh hưởng là`[2,4]`. Sau khi phân công:```
b = [5,8,8,8]
```Việc tái thiết mang lại:```
a = [5,10,12,14]
```đó chính xác là những gì việc truyền bá lặp đi lặp lại trong câu lệnh ban đầu tạo ra. Bản cập nhật cây phân đoạn phù hợp với hành vi dự định ngay cả khi mọi phần tử còn lại thay đổi.
