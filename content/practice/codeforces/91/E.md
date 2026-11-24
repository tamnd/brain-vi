---
title: "CF 91E - Tòa nhà chọc trời Igloo"
description: "Mỗi con hải mã xây dựng một tòa nhà chọc trời có chiều cao thay đổi tuyến tính theo thời gian. Walrus i bắt đầu với chiều cao a[i] và tăng b[i] tầng mỗi phút, vì vậy tại thời điểm t chiều cao của nó bằng: $$hi(t) = ai + bi cdot t$$ Đối với mỗi truy vấn [l, r, t], chúng ta phải tìm một chỉ mục bên trong khoảng đó…"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "geometry"]
categories: ["algorithms"]
codeforces_contest: 91
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 75 (Div. 1 Only)"
rating: 2500
weight: 91
solve_time_s: 164
verified: true
draft: false
---

[CF 91E - Tòa nhà chọc trời Igloo](https://codeforces.com/problemset/problem/91/E) 

**Đánh giá:** 2500 
**Tags:** cấu trúc dữ liệu, hình học 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi con hải mã xây dựng một tòa nhà chọc trời có chiều cao thay đổi tuyến tính theo thời gian. hải mã`i`bắt đầu với chiều cao`a[i]`và lợi ích`b[i]`sàn mỗi phút, vì vậy vào thời điểm đó`t`chiều cao của nó bằng:$$h_i(t) = a_i + b_i \cdot t$$Đối với mỗi truy vấn`[l, r, t]`, chúng ta phải tìm một chỉ mục bên trong khoảng đó có tòa nhà chọc trời cao nhất tại thời điểm đó`t`. 

Giải thích đơn giản là: trong số tất cả các dòng$$y = a_i + b_i t$$giới hạn ở các chỉ số trong`[l, r]`, trả về giá trị lớn nhất tại`t`. 

Các ràng buộc ngay lập tức loại trừ mọi thứ bậc hai. Với`n, q ≤ 10^5`, thậm chí là một`O(n)`quét mỗi truy vấn sẽ yêu cầu khoảng`10^{10}`hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần gũi`O(log^2 n)`hoặc`O(\sqrt n)`mỗi truy vấn. 

Phần nguy hiểm nhất của vấn đề này là sự thay đổi tối đa theo thời gian. Một con hải mã tốt nhất ở kích thước nhỏ`t`sau này có thể trở nên tồi tệ hơn vì một con hải mã khác có độ dốc lớn hơn. 

Hãy xem xét ví dụ này:```
2 2
100 1
1 100
1 2 0
1 2 2
```Vào thời điểm`0`, hải mã`1`chiến thắng nhờ chiều cao`100`. 

Vào thời điểm`2`, hải mã`2`chiến thắng nhờ chiều cao`201`. 

Cấu trúc chỉ lưu trữ mức tối đa hiện tại hoặc so sánh hải mã theo thứ tự cố định sẽ thất bại ngay lập tức. 

Một trường hợp tế nhị khác là cà vạt.```
2 1
5 3
5 3
1 2 10
```Cả hai con hải mã đều có chiều cao như nhau mãi mãi. Bất kỳ câu trả lời là hợp lệ. Việc triển khai bao lồi bất cẩn loại bỏ các dòng bằng nhau không chính xác có thể vô tình xóa tất cả các ứng cử viên hoặc giữ sai một dòng. 

Một cái bẫy nữa là xử lý các dây có độ dốc giống nhau.```
3 1
1 5
10 5
7 5
1 3 100
```Tất cả các độ dốc đều bằng nhau nên con hải mã có điểm chặn lớn nhất luôn chiếm ưu thế so với những con khác. Nếu các độ dốc bằng nhau không được xử lý cẩn thận trong quá trình xây dựng thân tàu, tìm kiếm nhị phân trên các giao lộ sẽ không hợp lệ vì các đường song song không bao giờ giao nhau. 

Quan điểm hình học là quan sát chính. Mỗi con hải mã tương ứng với một dòng và mỗi truy vấn yêu cầu dòng tối đa tại một tọa độ x trên một mảng con gồm các chỉ mục. 

## Phương pháp tiếp cận 

Giải pháp brute-force đánh giá từng con hải mã bên trong`[l, r]`. 

Đối với một truy vấn, chúng tôi tính toán:$$a_i + b_i \cdot t$$cho tất cả`i`trong khoảng thời gian và trả về chỉ mục có giá trị lớn nhất. 

Điều này đúng vì nó trực tiếp thực hiện định nghĩa của vấn đề. Vấn đề là chi phí. Một truy vấn có thể kiểm tra`O(n)`hải mã, do đó tổng độ phức tạp trở thành`O(nq)`. Với cả hai giá trị bằng`10^5`, điều này đạt tới`10^{10}`đánh giá vượt quá giới hạn. 

Quan sát quan trọng là mỗi con hải mã đều xác định một đường thẳng trên mặt phẳng. Một truy vấn yêu cầu đường bao phía trên tại tọa độ`t`. 

Nếu chúng ta có tất cả các đường trên toàn cầu, chúng ta có thể xây dựng một cấu trúc thủ thuật thân lồi. Khó khăn là sự hạn chế về khoảng thời gian`[l, r]`. 

Điều này gợi ý sự kết hợp hai ý tưởng: 

Đầu tiên, sử dụng cây phân đoạn trên các chỉ mục. Mỗi nút lưu trữ tất cả các dòng thuộc phân khúc đó. 

Thứ hai, bên trong mỗi nút, xây dựng một bao lồi cho các truy vấn tối đa. 

Sau đó một khoảng truy vấn`[l, r]`phân hủy thành`O(log n)`các nút cây phân đoạn. Đối với mỗi nút, chúng tôi truy vấn thân của nó tại thời điểm`t`và câu trả lời hay nhất trong số đó là mức tối đa toàn cầu. 

Tại sao điều này hoạt động tốt ở đây? Bởi vì mỗi con hải mã chỉ thuộc về`O(log n)`các nút cây phân đoạn. Tổng số dòng được lưu trữ trở thành`O(n log n)`, có thể quản lý được. 

Bên trong một nút, chúng tôi sắp xếp các đường theo độ dốc và xây dựng phần thân trên. Vì các truy vấn chỉ yêu cầu một tọa độ x duy nhất nên chúng ta có thể tìm kiếm nhị phân trên các điểm giao nhau để tìm đường thẳng tốt nhất trong`O(log n)`thời gian. 

Độ phức tạp cuối cùng trở thành`O(log^2 n)`cho mỗi truy vấn, đủ nhanh để`10^5`hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + q log² n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy biểu diễn mỗi con hải mã dưới dạng một đường thẳng. 

Đối với hải mã`i`, định nghĩa:$$f_i(t) = a_i + b_i t$$Chúng tôi cũng lưu trữ chỉ mục`i`vì các truy vấn phải trả về số hải mã chứ không chỉ chiều cao. 
2. Xây dựng cây phân đoạn trên các chỉ số hải mã. 

Mỗi nút tương ứng với một số khoảng chỉ số. Nút lưu trữ tất cả các dòng có hải mã thuộc khoảng đó. 
3. Đối với mỗi nút, sắp xếp các đường theo độ dốc. 

Nếu hai đường có cùng độ dốc thì chỉ có đường có điểm giao nhau lớn hơn mới quan trọng vì nó lấn át đường kia trong mọi trường hợp.`t`. 
4. Xây dựng bao lồi trên cho nút. 

Trong khi chèn các đường theo thứ tự dốc, hãy loại bỏ các đường không cần thiết ở phía sau. 

Giả sử chúng ta đã có dòng`A`Và`B`, và muốn chèn`C`. Nếu giao điểm của`A`Và`B`xảy ra ở bên phải giao điểm của`B`Và`C`, sau đó`B`không bao giờ là tối ưu và có thể bị loại bỏ. 
5. Lưu trữ thân tàu dưới dạng danh sách các dòng theo thứ tự. 

Vì độ dốc được sắp xếp nên đường tối ưu để tăng`t`cũng chuyển động đều đều dọc theo thân tàu. 
6. Xử lý truy vấn`[l, r, t]`. 

Phân tách khoảng thành`O(log n)`các nút cây phân đoạn. 
7. Truy vấn từng thân nút một cách độc lập. 

Đối với một cố định`t`, tìm kiếm nhị phân trên thân tàu để tìm đường thẳng có giá trị lớn nhất tại`t`. 
8. So sánh tất cả các câu trả lời của ứng viên. 

Đánh giá mọi dòng trả về tại thời điểm`t`và giữ chỉ số hải mã tốt nhất. 

### Tại sao nó hoạt động 

Mỗi dòng hải mã xuất hiện trong mọi nút cây phân đoạn có khoảng chứa chỉ mục của nó. Khi truy vấn`[l, r]`, sự phân rã cây phân đoạn bao gồm chính xác các con hải mã trong khoảng đó và không có gì khác. 

Bên trong một nút, bao lồi lưu trữ chính xác các đường có thể trở thành cực đại trong một giá trị thời gian nào đó. Bất kỳ dòng nào bị loại bỏ đều bị thống trị bởi những dòng khác có thể`t`, vì vậy việc xóa nó không thể thay đổi câu trả lời. 

Tìm kiếm nhị phân hoạt động vì dọc theo thân trên, đường tối ưu thay đổi đơn điệu như`t`tăng lên. Các giao điểm giữa các đường thân tàu liên tiếp được sắp xếp theo thứ tự, đây chính xác là đặc tính được duy trì trong quá trình xây dựng thân tàu. 

Việc kết hợp tất cả các nút được truy vấn đảm bảo chúng tôi xem xét mọi con hải mã trong`[l, r]`, và lấy mức tối đa trong số các ứng cử viên tốt nhất của họ sẽ đưa ra câu trả lời chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

class Line:
    __slots__ = ("m", "b", "idx")

    def __init__(self, m, b, idx):
        self.m = m
        self.b = b
        self.idx = idx

    def value(self, x):
        return self.m * x + self.b

def bad(l1, l2, l3):
    return (l3.b - l1.b) * (l1.m - l2.m) <= (l2.b - l1.b) * (l1.m - l3.m)

class Hull:
    __slots__ = ("lines",)

    def __init__(self, arr):
        arr.sort(key=lambda x: (x.m, x.b))

        filtered = []
        for line in arr:
            if filtered and filtered[-1].m == line.m:
                if filtered[-1].b < line.b:
                    filtered[-1] = line
            else:
                filtered.append(line)

        hull = []
        for line in filtered:
            while len(hull) >= 2 and bad(hull[-2], hull[-1], line):
                hull.pop()
            hull.append(line)

        self.lines = hull

    def query(self, x):
        lines = self.lines

        lo, hi = 0, len(lines) - 1

        while lo < hi:
            mid = (lo + hi) // 2

            if lines[mid].value(x) <= lines[mid + 1].value(x):
                lo = mid + 1
            else:
                hi = mid

        best = lines[lo]
        return best.value(x), best.idx

class SegmentTree:
    def __init__(self, a, b):
        self.n = len(a)
        self.tree = [[] for _ in range(4 * self.n)]

        self.build(1, 0, self.n - 1, a, b)

    def build(self, node, l, r, a, b):
        if l == r:
            self.tree[node] = Hull([Line(b[l], a[l], l + 1)])
            return

        mid = (l + r) // 2

        self.build(node * 2, l, mid, a, b)
        self.build(node * 2 + 1, mid + 1, r, a, b)

        merged = []

        merged.extend(self.tree[node * 2].lines)
        merged.extend(self.tree[node * 2 + 1].lines)

        self.tree[node] = Hull(merged)

    def query(self, node, l, r, ql, qr, t):
        if ql <= l and r <= qr:
            return self.tree[node].query(t)

        mid = (l + r) // 2

        best_val = -INF
        best_idx = -1

        if ql <= mid:
            val, idx = self.query(node * 2, l, mid, ql, qr, t)
            if val > best_val:
                best_val = val
                best_idx = idx

        if qr > mid:
            val, idx = self.query(node * 2 + 1, mid + 1, r, ql, qr, t)
            if val > best_val:
                best_val = val
                best_idx = idx

        return best_val, best_idx

def solve():
    n, q = map(int, input().split())

    a = [0] * n
    b = [0] * n

    for i in range(n):
        a[i], b[i] = map(int, input().split())

    seg = SegmentTree(a, b)

    ans = []

    for _ in range(q):
        l, r, t = map(int, input().split())

        _, idx = seg.query(1, 0, n - 1, l - 1, r - 1, t)

        ans.append(str(idx))

    sys.stdout.write("\n".join(ans))

solve()
```các`Line`lớp lưu trữ độ dốc, điểm chặn và chỉ số hải mã gốc. Biểu thức được viết là`m * x + b`, Ở đâu`m = b[i]`Và`b = a[i]`. 

các`bad`Hàm thực hiện điều kiện loại bỏ bao lồi bằng cách sử dụng phép nhân chéo. Sử dụng số học số nguyên sẽ tránh được các vấn đề về độ chính xác của dấu phẩy động. 

Kết cấu thân tàu được phân loại theo độ dốc. Nếu hai đường có độ dốc giống nhau thì chỉ có đường chặn lớn hơn tồn tại vì đường chặn nhỏ hơn không bao giờ có thể thắng. 

Tìm kiếm nhị phân bên trong`Hull.query`dựa vào thực tế là các giá trị dọc theo thân trên tạo thành một chuỗi không đồng nhất ở mức cố định`x`. So sánh các đường liền kề là đủ để xác định bên nào chứa tối ưu. 

Cây phân đoạn lưu trữ một thân tại mỗi nút. Tòa nhà hợp nhất các đường thân của cả hai phần tử con và xây dựng lại phần thân cho phần tử cha. 

Một chi tiết triển khai tinh tế là lập chỉ mục. Cây hoạt động nội bộ với`0`-chỉ số dựa trên, nhưng câu trả lời phải sử dụng bản gốc`1`-dựa trên số hải mã, vì vậy dòng lưu trữ`l + 1`. 

Một chi tiết quan trọng khác là việc so sánh truy vấn chỉ sử dụng`>`thay vì`>=`. Bài toán cho phép bất kỳ câu trả lời hợp lệ nào trong các mối quan hệ, do đó lựa chọn nào cũng có hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4
4 1
3 5
6 2
3 5
6 5
1 5 2
1 3 5
1 1 0
1 5 0
```Các dòng hải mã là: 

| Hải mã | Công thức | 
| --- | --- | 
| 1 | 4 + t | 
| 2 | 3 + 5t | 
| 3 | 6 + 2t | 
| 4 | 3 + 5t | 
| 5 | 6 + 5t | 

Truy vấn`[1, 5, 2]`. 

| Hải mã | Chiều cao tại t=2 | 
| --- | --- | 
| 1 | 6 | 
| 2 | 13 | 
| 3 | 10 | 
| 4 | 13 | 
| 5 | 16 | 

Câu trả lời là hải mã`5`. 

Truy vấn`[1, 3, 5]`. 

| Hải mã | Chiều cao tại t=5 | 
| --- | --- | 
| 1 | 9 | 
| 2 | 28 | 
| 3 | 16 | 

Câu trả lời là hải mã`2`. 

Ví dụ này cho thấy các độ dốc lớn hơn cuối cùng sẽ chiếm ưu thế như thế nào ngay cả khi độ cao ban đầu của chúng nhỏ hơn. 

### Ví dụ tùy chỉnh```
3 2
100 1
1 100
50 2
1 3 0
1 3 10
```Vào thời điểm`0`: 

| Hải mã | Chiều cao | 
| --- | --- | 
| 1 | 100 | 
| 2 | 1 | 
| 3 | 50 | 

Trả lời:`1`Vào thời điểm`10`: 

| Hải mã | Chiều cao | 
| --- | --- | 
| 1 | 110 | 
| 2 | 1001 | 
| 3 | 70 | 

Trả lời:`2`Dấu vết này chứng tỏ tại sao việc đặt hàng tĩnh là không thể. Những thay đổi tối đa theo thời gian vì độ dốc rất quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log² n) | Mỗi truy vấn chạm vào các nút O(log n) và thực hiện tìm kiếm thân O(log n) | 
| Không gian | O(n log n) | Mỗi dòng xuất hiện trong các nút cây phân đoạn O(log n) | 

Với`10^5`hải mã và truy vấn, khoảng vài triệu thao tác được thực hiện, vừa vặn trong giới hạn của Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

INF = 10**30

class Line:
    __slots__ = ("m", "b", "idx")

    def __init__(self, m, b, idx):
        self.m = m
        self.b = b
        self.idx = idx

    def value(self, x):
        return self.m * x + self.b

def bad(l1, l2, l3):
    return (l3.b - l1.b) * (l1.m - l2.m) <= (l2.b - l1.b) * (l1.m - l3.m)

class Hull:
    def __init__(self, arr):
        arr.sort(key=lambda x: (x.m, x.b))

        filtered = []
        for line in arr:
            if filtered and filtered[-1].m == line.m:
                if filtered[-1].b < line.b:
                    filtered[-1] = line
            else:
                filtered.append(line)

        hull = []
        for line in filtered:
            while len(hull) >= 2 and bad(hull[-2], hull[-1], line):
                hull.pop()
            hull.append(line)

        self.lines = hull

    def query(self, x):
        lines = self.lines

        lo, hi = 0, len(lines) - 1

        while lo < hi:
            mid = (lo + hi) // 2

            if lines[mid].value(x) <= lines[mid + 1].value(x):
                lo = mid + 1
            else:
                hi = mid

        best = lines[lo]
        return best.value(x), best.idx

class SegmentTree:
    def __init__(self, a, b):
        self.n = len(a)
        self.tree = [[] for _ in range(4 * self.n)]

        self.build(1, 0, self.n - 1, a, b)

    def build(self, node, l, r, a, b):
        if l == r:
            self.tree[node] = Hull([Line(b[l], a[l], l + 1)])
            return

        mid = (l + r) // 2

        self.build(node * 2, l, mid, a, b)
        self.build(node * 2 + 1, mid + 1, r, a, b)

        merged = []
        merged.extend(self.tree[node * 2].lines)
        merged.extend(self.tree[node * 2 + 1].lines)

        self.tree[node] = Hull(merged)

    def query(self, node, l, r, ql, qr, t):
        if ql <= l and r <= qr:
            return self.tree[node].query(t)

        mid = (l + r) // 2

        best_val = -INF
        best_idx = -1

        if ql <= mid:
            val, idx = self.query(node * 2, l, mid, ql, qr, t)
            if val > best_val:
                best_val = val
                best_idx = idx

        if qr > mid:
            val, idx = self.query(node * 2 + 1, mid + 1, r, ql, qr, t)
            if val > best_val:
                best_val = val
                best_idx = idx

        return best_val, best_idx

def solve():
    input = sys.stdin.readline

    n, q = map(int, input().split())

    a = []
    b = []

    for _ in range(n):
        x, y = map(int, input().split())
        a.append(x)
        b.append(y)

    seg = SegmentTree(a, b)

    out = []

    for _ in range(q):
        l, r, t = map(int, input().split())

        _, idx = seg.query(1, 0, n - 1, l - 1, r - 1, t)

        out.append(str(idx))

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run(
"""5 4
4 1
3 5
6 2
3 5
6 5
1 5 2
1 3 5
1 1 0
1 5 0
"""
) == "5\n2\n1\n5"

# minimum size
assert run(
"""1 1
7 3
1 1 10
"""
) == "1"

# equal slopes
assert run(
"""3 1
1 5
10 5
7 5
1 3 100
"""
) == "2"

# changing maximum over time
assert run(
"""2 2
100 1
1 100
1 2 0
1 2 10
"""
) == "1\n2"

# single-element range inside larger array
assert run(
"""4 2
1 1
2 2
3 3
4 4
2 2 100
4 4 0
"""
) == "2\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hải mã đơn | Luôn là chính nó | Hạn chế tối thiểu | 
| Độ dốc bằng nhau | Chiến thắng đánh chặn lớn nhất | Xử lý đường song song | 
| Thay đổi tối đa | Câu trả lời khác nhau theo thời gian | Chuyển tiếp thân tàu đúng cách | 
| Khoảng phần tử đơn | Chỉ số chính xác được trả về | Ranh giới cây phân đoạn | 

## Vỏ cạnh 

Hãy xem xét các độ dốc giống hệt nhau một lần nữa:```
3 1
1 5
10 5
7 5
1 3 100
```Tất cả các dòng đều song song. Trong quá trình thi công thân tàu, chỉ có đường có điểm chặn`10`sống sót vì:$$10 + 5t > 7 + 5t$$Và$$10 + 5t > 1 + 5t$$cho mọi`t`. 

Truy vấn trả về chính xác con hải mã`2`. 

Bây giờ hãy xem xét các mối quan hệ:```
2 1
5 3
5 3
1 2 10
```Cả hai dòng đều giống hệt nhau. Thân tàu có thể giữ một trong hai. Vào thời điểm`10`, cả hai đều ước tính`35`, do đó, chỉ mục nào cũng hợp lệ theo câu lệnh. 

Cuối cùng, hãy xem xét một công tắc thống trị:```
2 2
100 1
1 100
1 2 0
1 2 2
```Tại`t = 0`: 

| Hải mã | Chiều cao | 
| --- | --- | 
| 1 | 100 | 
| 2 | 1 | 

Tại`t = 2`: 

| Hải mã | Chiều cao | 
| --- | --- | 
| 1 | 102 | 
| 2 | 201 | 

Bao lồi lưu trữ cả hai đường vì mỗi đường là tối ưu trên một khoảng nào đó của`t`. Tìm kiếm nhị phân trên các quá trình chuyển đổi thân tàu từ hải mã`1`đến hải mã`2`chính xác sau điểm giao nhau của chúng.
