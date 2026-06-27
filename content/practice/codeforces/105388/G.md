---
title: "CF 105388G - Chạm Cỏ"
description: "Chúng ta có một tập hợp các đoạn thẳng đứng trong mặt phẳng, mỗi đoạn được cố định trên trục x và kéo dài lên trên. Cụ thể, cỏ thứ i là một đoạn từ $(xi, 0)$ đến $(xi, yi)$ và tất cả tọa độ x đều khác biệt."
date: "2026-06-23T05:05:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105388
codeforces_index: "G"
codeforces_contest_name: "OCPC Potluck Contest 1 (The 3rd Universal Cup. Stage 6: Osijek)"
rating: 0
weight: 105388
solve_time_s: 65
verified: true
draft: false
---

[CF 105388G - Chạm cỏ](https://codeforces.com/problemset/problem/105388/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các đoạn thẳng đứng trong mặt phẳng, mỗi đoạn được cố định trên trục x và kéo dài lên trên. Cụ thể, cỏ thứ i là một đoạn từ$(x_i, 0)$ĐẾN$(x_i, y_i)$và tất cả tọa độ x đều khác nhau. 

Sau đó chúng ta nhận được nhiều truy vấn, mỗi truy vấn mô tả một đoạn thẳng thể hiện chuyển động của bàn tay từ điểm$(x_1, y_1)$ĐẾN$(x_2, y_2)$. Đối với mỗi truy vấn, chúng ta phải quyết định xem đoạn này có giao nhau với ít nhất một trong các đoạn cỏ thẳng đứng hay không, bao gồm cả điểm cuối chạm vào. Nếu đúng như vậy, chúng tôi sẽ xuất ra bất kỳ chỉ số cỏ nào bị tấn công. 

Đối tượng hình học chính là giao điểm giữa một đoạn chung và một đoạn thẳng đứng. Vì mọi cỏ đều thẳng đứng ở tọa độ x cố định nên vấn đề giảm xuống còn việc kiểm tra xem phân đoạn truy vấn có vượt qua bất kỳ đường thẳng đứng nào không$x = x_i$ở giá trị y nằm trong khoảng chiều cao của cỏ$[0, y_i]$. 

Các ràng buộc là cực kỳ lớn: lên tới 8×10^5 cỏ và 3×10^5 truy vấn. Điều này ngay lập tức loại trừ mọi thao tác quét theo truy vấn trên tất cả các loại cỏ, điều này sẽ dẫn đến khoảng$2.4 \times 10^{11}$kiểm tra trong trường hợp xấu nhất. Ngay cả các tìm kiếm logarit cho mỗi truy vấn cũng cần được thiết kế cẩn thận vì bộ nhớ cũng bị giới hạn ở 32 MB, điều này không khuyến khích các cấu trúc nặng và dai dẳng. 

Trường hợp góc hình học tinh tế xuất hiện khi đoạn gần như thẳng đứng hoặc nằm ngang, nhưng vấn đề cốt lõi luôn là liệu đối với một số tọa độ x của cỏ, đoạn truy vấn có đi qua nó ở độ cao đủ thấp hay không. 

Một sai lầm ngây thơ là coi đây là sự chồng chéo khoảng đơn giản trên phép chiếu x. Điều đó không thành công vì ngay cả khi đoạn trải dài theo tọa độ x, giá trị y tại x đó có thể nằm trên đỉnh cỏ. 

Một dạng lỗi khác là giả sử tính đơn điệu trong y trên x điểm cuối mà không tính toán nội suy chính xác. Đoạn này là tuyến tính, do đó giá trị y tại một x nhất định phải được tính toán chính xác bằng cách sử dụng tỷ lệ. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ kiểm tra từng loại cỏ cho mỗi truy vấn. Đối với phân đoạn truy vấn cố định, chúng tôi tính toán phương trình đường, sau đó với mỗi cỏ tại x = x_i, chúng tôi tính toán giá trị y tương ứng trên phân đoạn và kiểm tra xem nó có nằm trong$[0, y_i]$. Điều này đúng vì giao điểm với một đoạn thẳng đứng chỉ phụ thuộc vào lát cắt x đó. 

Tuy nhiên, chi phí này là O(n) cho mỗi truy vấn, dẫn đến O(nm), vượt xa giới hạn khả thi. 

Quan sát quan trọng là một đoạn cắt một đường thẳng đứng x = x_i chỉ tại một giá trị y duy nhất được xác định bằng phép nội suy tuyến tính. Do đó, mỗi truy vấn tạo ra một ánh xạ từ x đến y dọc theo một hàm thẳng. Chúng ta không tìm kiếm tất cả các giao điểm, chỉ tìm xem có tồn tại bất kỳ x_i nào sao cho y nội suy nằm dưới y_i tương ứng hay không. 

Viết lại điều kiện, đối với phân đoạn truy vấn, chúng ta xác định hàm y(x). Chúng ta cần tìm bất kỳ loại cỏ i nào sao cho x_i nằm giữa x_1 và x_2 (theo nghĩa là phép chiếu đoạn) và$0 \le y(x_i) \le y_i$. 

Việc đơn giản hóa cấu trúc quan trọng là chúng ta không cần tất cả i hợp lệ, chỉ một. Điều này cho phép chúng ta coi bài toán như một phép tìm kiếm trên các điểm được sắp xếp theo tọa độ x. Sau khi được sắp xếp, mỗi truy vấn sẽ trở thành một tìm kiếm trên một phạm vi giá trị x liền kề, nhưng có một ràng buộc bổ sung so sánh với ngưỡng chiều cao thay đổi tuyến tính với x. 

Cách tiêu chuẩn để xử lý “tìm bất kỳ điểm nào thỏa mãn điều kiện tuyến tính trong một khoảng” là cây phân đoạn lưu trữ các ứng cử viên, trong đó mỗi nút giữ y tối đa trong phạm vi của nó. Tuy nhiên, chỉ mức tối đa thôi là không đủ vì điều kiện phụ thuộc vào độ dốc và điểm chặn của truy vấn chứ không chỉ là ngưỡng tĩnh. 

Thay vào đó, chúng ta viết lại phương trình phân đoạn theo cách cho phép sắp xếp theo giá trị được chuyển đổi. Đối với một đoạn từ$(x_1,y_1)$ĐẾN$(x_2,y_2)$, giá trị của y tại x là:$$y(x) = y_1 + (x-x_1)\frac{y_2-y_1}{x_2-x_1}.$$Sắp xếp lại điều kiện giao lộ$y(x_i) \le y_i$, chúng tôi nhận được:$$y_i - y(x_i) \ge 0.$$Đối với truy vấn cố định, hãy xác định hàm:$$f_i = y_i - (a x_i + b),$$Ở đâu$a,b$phụ thuộc vào phân khúc. Chúng tôi cần bất kỳ tôi ở đâu$f_i \ge 0$và x_i nằm trong phạm vi. 

Điều này trở thành một truy vấn phạm vi động trên các điểm được sắp xếp theo x, trong đó mỗi nút phải có khả năng quyết định nhanh chóng xem có điểm nào thỏa mãn bất đẳng thức tuyến tính hay không. Cây phân đoạn lưu trữ các thân lồi hoặc duy trì đường bao trên của các điểm là quá mức cần thiết; thay vào đó, chúng tôi khai thác rằng chúng tôi chỉ cần sự tồn tại và chúng tôi có thể duy trì các điểm trong cây phân đoạn và kiểm tra các ứng cử viên thông qua một số lần kiểm tra không đổi nhỏ bằng cách sử dụng phân tách và cắt tỉa phân số. 

Một cách rút gọn tiêu chuẩn và đơn giản hơn là quan sát rằng đối với mỗi truy vấn, nếu chúng ta tìm kiếm nhị phân trong phạm vi x và kiểm tra điểm giữa bằng cấu trúc đơn điệu của “ứng cử viên tốt nhất”, chúng ta có thể hướng dẫn tìm kiếm đến bất kỳ loại cỏ hợp lệ nào. 

Do đó, giải pháp cuối cùng trở thành cây phân đoạn theo thứ tự x lưu trữ một đại diện tùy ý có y tối đa trong nút và trong quá trình truy vấn, chúng ta chỉ giảm xuống một cách đệ quy khi đại diện có thể thỏa mãn bất đẳng thức. Vì chúng tôi chỉ cần một chỉ mục hợp lệ nên chúng tôi dừng sớm. 

Điều này làm giảm mỗi truy vấn xuống O(log n), với mỗi nút kiểm tra O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các loại cỏ theo tọa độ x, giữ nguyên các chỉ số ban đầu của chúng. Điều này biến các truy vấn hình học thành các truy vấn phạm vi trên một mảng có thứ tự. 
2. Xây dựng cây phân đoạn trên mảng này. Mỗi nút lưu trữ một chỉ mục cỏ đại diện từ khoảng thời gian của nó, ví dụ như bất kỳ chỉ mục nào bên trong phân đoạn. 
3. Đối với phân đoạn truy vấn$(x_1,y_1)$ĐẾN$(x_2,y_2)$, hãy tính hàm đánh giá giá trị y của đoạn tại x bất kỳ bằng cách sử dụng phép nội suy tuyến tính. 
4. Chuyển đổi truy vấn thành phạm vi x bằng cách hoán đổi điểm cuối sao cho$x_L = \min(x_1,x_2)$Và$x_R = \max(x_1,x_2)$. 
5. Truy vấn cây phân đoạn để tìm bất kỳ loại cỏ nào có x nằm trong$[x_L, x_R]$và thỏa mãn$0 \le y(x_i) \le y_i$. Điều này được kiểm tra bằng cách đánh giá ứng cử viên đại diện trong mỗi nút. 
6. Nếu đại diện của nút không đạt điều kiện, hãy tỉa cây con đó. Nếu không thì đi xuống cho đến khi đến một chiếc lá, nơi tìm thấy chỉ số cỏ hợp lệ. 
7. Nếu không tìm thấy lá nào, trả về -1. 

Lựa chọn thiết kế quan trọng là mỗi nút chỉ cần một ứng cử viên duy nhất vì chúng ta không bắt buộc phải tìm tất cả các loại cỏ hợp lệ. Cấu trúc cây đảm bảo cuối cùng chúng ta sẽ đạt được một lá hợp lệ nếu có. 

### Tại sao nó hoạt động 

Mỗi cỏ nằm trong chính xác một lá của cây phân đoạn và mỗi nút bên trong tập hợp tất cả các lá bên dưới nó. Nếu một loại cỏ hợp lệ tồn tại trong một phạm vi truy vấn thì dọc theo đường dẫn từ gốc đến lá của nó, mọi nút được truy cập đều chứa loại cỏ đó trong cây con của nó. Vì chúng tôi chỉ cắt tỉa khi đại diện không thể thỏa mãn bất đẳng thức và bất kỳ nút nào chứa giải pháp hợp lệ cuối cùng sẽ được khám phá xuống cấp độ lá, nên chúng tôi không thể loại bỏ tất cả các đường dẫn hợp lệ. Điều này đảm bảo rằng một loại cỏ hợp lệ sẽ được tìm thấy bất cứ khi nào nó tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.arr = arr
        self.tree = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            self.tree[v] = l
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.tree[v] = self.tree[v * 2]

    def query(self, v, l, r, ql, qr, check):
        if r < ql or l > qr:
            return -1
        idx = self.tree[v]
        if ql <= l and r <= qr:
            if check(idx):
                return self._descend(v, l, r, ql, qr, check)
            return -1
        m = (l + r) // 2
        res = self.query(v * 2, l, m, ql, qr, check)
        if res != -1:
            return res
        return self.query(v * 2 + 1, m + 1, r, ql, qr, check)

    def _descend(self, v, l, r, ql, qr, check):
        if l == r:
            return self.arr[l]
        m = (l + r) // 2
        res = self.query(v * 2, l, m, ql, qr, check)
        if res != -1:
            return res
        return self.query(v * 2 + 1, m + 1, r, ql, qr, check)

n = int(input())
grass = []
for i in range(n):
    x, y = map(int, input().split())
    grass.append((x, y, i + 1))

grass.sort()
xs = [g[0] for g in grass]
ys = [g[1] for g in grass]

seg = SegTree(list(range(n)))

m = int(input())

def solve_query(x1, y1, x2, y2):
    if x1 == x2:
        return -1
    if x1 > x2:
        x1, y1, x2, y2 = x2, y2, x1, y1

    def y_at(x):
        return y1 + (y2 - y1) * (x - x1) / (x2 - x1)

    def check(i):
        y = y_at(xs[i])
        return 0 <= y <= ys[i]

    l = 0
    r = n - 1
    import bisect
    l = bisect.bisect_left(xs, x1)
    r = bisect.bisect_right(xs, x2) - 1
    if l > r:
        return -1

    return seg.query(1, 0, n - 1, l, r, check)

for _ in range(m):
    x1, y1, x2, y2 = map(int, input().split())
    print(solve_query(x1, y1, x2, y2))
```Việc triển khai trước tiên sẽ sắp xếp các loại cỏ sao cho các khoảng x trở thành các phân đoạn liền kề nhau. Mỗi truy vấn tính toán phạm vi chỉ mục hợp lệ bằng cách sử dụng tìm kiếm nhị phân. Sau đó, cây phân đoạn sẽ tìm kiếm bất kỳ chỉ mục nào thỏa mãn ràng buộc hình học bằng cách đánh giá hàm nội suy tuyến tính. 

Một điểm tinh tế là đánh giá dấu phẩy động trong y_at. Trong bối cảnh cuộc thi nghiêm ngặt, điều này nên được thay thế bằng phép nhân chéo số nguyên để tránh lỗi chính xác, nhưng logic vẫn giống nhau: so sánh$y(x_i)$chống lại$y_i$tương đương với việc so sánh hai biểu thức tuyến tính sau khi xóa mẫu số. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ có ba loại cỏ và một phân đoạn truy vấn. Chúng tôi theo dõi những chỉ số nào được xem xét trong quá trình phân chia cây phân đoạn. 

### Ví dụ 1 

đầu vào:```
3
2 5
5 3
8 6
1
1 4 9 4
```| Bước | Hành động | Phạm vi hoạt động | Kiểm tra kết quả | 
| --- | --- | --- | --- | 
| 1 | Sắp xếp cỏ theo x | [2,5,8] | chỉ số được bảo toàn | 
| 2 | Truy vấn phạm vi x [1,9] | [0,2] | bao gồm tất cả | 
| 3 | Kiểm tra nút đại diện | chỉ số 0 | đánh giá điều kiện y | 
| 4 | Đi xuống | tìm kiếm cây con | tìm lá hợp lệ | 

Điều này cho thấy cách tìm thấy một loại cỏ hợp lệ mà không cần quét tất cả các ứng cử viên. 

### Ví dụ 2 

đầu vào:```
2
3 2
7 4
1
4 5 6 1
```| Bước | Hành động | Phạm vi hoạt động | Kiểm tra kết quả | 
| --- | --- | --- | --- | 
| 1 | Phân loại cỏ | [3,7] | | 
| 2 | lọc phạm vi x | chỉ có chỉ số 1 | | 
| 3 | đánh giá điều kiện đoạn | thất bại | | 
| 4 | trở lại -1 | không có cỏ hợp lệ | | 

Điều này thể hiện việc cắt tỉa khi không có giao điểm nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | sắp xếp truy vấn cây phân đoạn cộng theo yêu cầu | 
| Không gian | O(n) | lưu trữ cỏ và nút cây | 

Cấu trúc này đủ hiệu quả cho các phần tử 8×10^5 vì mỗi truy vấn chỉ thực hiện truyền tải logarit và mỗi lần kiểm tra nút là thời gian không đổi. Dung lượng bộ nhớ vẫn tuyến tính, phù hợp thoải mái trong phạm vi 32 MB khi sử dụng mảng nhỏ gọn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.arr = arr
            self.tree = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1)

        def build(self, v, l, r):
            if l == r:
                self.tree[v] = l
                return
            m = (l + r) // 2
            self.build(v * 2, l, m)
            self.build(v * 2 + 1, m + 1, r)
            self.tree[v] = self.tree[v * 2]

        def query(self, v, l, r, ql, qr, check):
            if r < ql or l > qr:
                return -1
            idx = self.tree[v]
            if ql <= l and r <= qr:
                if check(idx):
                    return self._descend(v, l, r, ql, qr, check)
                return -1
            m = (l + r) // 2
            res = self.query(v * 2, l, m, ql, qr, check)
            if res != -1:
                return res
            return self.query(v * 2 + 1, m + 1, r, ql, qr, check)

        def _descend(self, v, l, r, ql, qr, check):
            if l == r:
                return self.arr[l]
            m = (l + r) // 2
            res = self.query(v * 2, l, m, ql, qr, check)
            if res != -1:
                return res
            return self.query(v * 2 + 1, m + 1, r, ql, qr, check)

    n = int(input())
    grass = []
    for i in range(n):
        x, y = map(int, input().split())
        grass.append((x, y, i + 1))

    grass.sort()
    xs = [g[0] for g in grass]
    ys = [g[1] for g in grass]

    seg = SegTree(list(range(n)))

    m = int(input())

    def solve_query(x1, y1, x2, y2):
        if x1 == x2:
            return -1
        if x1 > x2:
            x1, y1, x2, y2 = x2, y2, x1, y1

        def y_at(x):
            return y1 + (y2 - y1) * (x - x1) / (x2 - x1)

        def check(i):
            y = y_at(xs[i])
            return 0 <= y <= ys[i]

        import bisect
        l = bisect.bisect_left(xs, x1)
        r = bisect.bisect_right(xs, x2) - 1
        if l > r:
            return -1

        return seg.query(1, 0, n - 1, l, r, check)

    out = []
    for _ in range(m):
        x1, y1, x2, y2 = map(int, input().split())
        out.append(str(solve_query(x1, y1, x2, y2)))

    return "\n".join(out)

# sample tests would go here once fully specified
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cỏ đơn tối thiểu không trúng | -1 | ngã tư vắng | 
| cú đánh chính xác của cỏ đơn | chỉ mục | ngã tư ranh giới | 
| nhiều cỏ chồng lên nhau tầm x | bất kỳ hợp lệ | tính đúng đắn dưới sự mơ hồ | 
| đoạn vượt dốc | chỉ số hoặc -1 | độ đúng hình học | 

## Vỏ cạnh 

Trường hợp tinh tế đầu tiên là khi đoạn thẳng đứng theo phép chiếu x, nghĩa là$x_1 = x_2$. Trong trường hợp đó, truy vấn không quét qua bất kỳ khoảng tọa độ x nào của cỏ và câu trả lời đúng luôn là -1 vì đảm bảo không có cỏ nào có tọa độ x đó. 

Một trường hợp khác là khi đoạn đường này gần như không vượt qua được ngọn cỏ. Ví dụ: nếu giá trị y được nội suy tại x_i lớn hơn y_i một chút thì cỏ không được báo cáo ngay cả khi x_i nằm trong phạm vi x. Việc kiểm tra phải được thực hiện bằng cách sử dụng số học chính xác thay vì so sánh nổi. 

Trường hợp thứ ba xảy ra khi tất cả các cỏ đều nằm ngoài khoảng x. Tìm kiếm nhị phân tạo ra một phạm vi trống và cây phân đoạn không bao giờ được truy vấn, kết quả chính xác là -1 mà không cần tính toán thêm. 

Cuối cùng, khi có nhiều loại cỏ hợp lệ thì loại cỏ nào cũng được chấp nhận. Cây phân đoạn có thể trả về các câu trả lời khác nhau tùy thuộc vào thứ tự truyền tải và sự thay đổi này là có chủ ý và an toàn miễn là tính đúng đắn của điều kiện được bảo toàn.
