---
title: "CF 150C - Kẻ lừa đảo thông minh"
description: "Chúng tôi có tuyến xe buýt thẳng với tọa độ điểm dừng cố định. Hành khách thường thanh toán toàn bộ khoảng cách giữa các điểm dừng lên máy bay và xuất cảnh. Người soát vé được phép \"giấu\" tối đa một đoạn liên tục của chuyến đi đó khỏi vé."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 150
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 107 (Div. 1)"
rating: 2200
weight: 150
solve_time_s: 131
verified: true
draft: false
---

[CF 150C - Kẻ lừa đảo thông minh](https://codeforces.com/problemset/problem/150/C) 

**Xếp hạng:** 2200 
**Tas:** cấu trúc dữ liệu, toán, xác suất 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có tuyến xe buýt thẳng với tọa độ điểm dừng cố định. Hành khách thường thanh toán toàn bộ khoảng cách giữa các điểm dừng lên máy bay và xuất cảnh. Người soát vé được phép “giấu” tối đa một đoạn liên tục của chuyến đi đó khỏi vé. Nếu một hành khách đi từ điểm dừng`a`dừng lại`b`, người dẫn đường có thể chọn một khoảng nào đó`[C, D]`trong chuyến đi đó và tránh bán vé cho quãng đường từ`C`ĐẾN`D`. 

Số tiền vé tiết kiệm được chia đều cho hành khách và người soát vé. Điều đó có nghĩa là nếu độ dài đoạn ẩn là`x[D] - x[C]`, người soát vé ngay lập tức kiếm được một nửa số tiền đó. 

Rủi ro đến từ việc kiểm tra. Mỗi cạnh giữa các điểm dừng liên tiếp đều có xác suất kiểm tra độc lập. Nếu hành khách không có vé hợp lệ cho đoạn đường bị kiểm tra, người soát vé sẽ bị phạt`c`cho hành khách đó trên đoạn đường đó. 

Nhiệm vụ là tối đa hóa lợi nhuận kỳ vọng trên tất cả các hành khách một cách độc lập. Mỗi hành khách có thể nhận được một kế hoạch gian lận khác nhau. 

Các ràng buộc hoàn toàn định hình giải pháp. Có tới`150000`dừng lại và`300000`hành khách. Giải pháp bậc hai về các điểm dừng là không thể và thậm chí việc xử lý mọi hành khách bằng cách quét tuyến tính trên tuyến đường của họ sẽ tốn khoảng`3 * 10^5 * 1.5 * 10^5`, vượt xa giới hạn. Chúng ta cần thứ gì đó xung quanh`O((n + m) log n)`hoặc`O(n + m)`. 

Phần khó khăn là kỳ vọng trộn lẫn hai đại lượng khác nhau: 

1. Thu lợi ngay lập tức từ việc không bán một phần vé. 
2. Dự kiến ​​tổn thất trong tương lai từ việc kiểm tra. 

Đại lượng thứ hai phụ thuộc vào xác suất của các phân đoạn, do đó việc triển khai bất cẩn thường tính toán sai kỳ vọng. 

Một sai lầm dễ mắc phải là quên rằng việc kiểm tra trên các phân đoạn khác nhau đóng góp tuyến tính vào kỳ vọng. Giả định:```
3 1 10
0 10 20
50 50
1 3
```Nếu chúng ta giấu toàn bộ chuyến đi, chúng ta sẽ tiết kiệm được`20`, do đó người soát vé kiếm được`10`. Mức phạt dự kiến ​​là:`10 * 0.5 + 10 * 0.5 = 10`Lợi nhuận kỳ vọng là`0`, không âm và không được tính bằng các xác suất kết hợp như`1 - (1-0.5)^2`. 

Một trường hợp tinh vi khác là khi chỉ gian lận một phần của lộ trình là tối ưu.```
4 1 100
0 10 20 100
0 100 0
1 4
```Mép giữa đã đảm bảo kiểm tra nên việc giấu kín cả chuyến đi thật là khủng khiếp. Lựa chọn tối ưu là chỉ ẩn đoạn`3 -> 4`, không có rủi ro kiểm tra. Sự tham lam “giấu mọi thứ với khoảng cách tích cực” đã thất bại ở đây. 

Cũng có trường hợp gian lận không bao giờ có lãi.```
2 1 1000
0 10
100
1 2
```Người soát vé sẽ chỉ đạt được`5`, nhưng mức phạt dự kiến ​​là`1000`, vậy câu trả lời hay nhất là`0`. Bất kỳ triển khai nào luôn chọn khoảng ẩn không trống đều cho kết quả sai. 

## Phương pháp tiếp cận 

Bắt đầu với chế độ xem bạo lực. Đối với hành khách đi từ điểm dừng`a`dừng lại`b`, chúng ta có thể thử từng cặp`(C, D)`với`a <= C < D <= b`. Khoảng cách ẩn là:```
x[D] - x[C]
```Người soát vé nhận được một nửa số đó. Mức phạt dự kiến ​​bằng tổng xác suất kiểm tra trên mọi cạnh chưa được che chắn bên trong`[C, D)`nhân với`c`. 

Vì vậy, lợi nhuận dự kiến ​​​​sẽ trở thành:```
(x[D] - x[C]) / 2 - c * sum(probabilities on hidden edges)
```Nếu chúng ta tính toán trước tổng tiền tố của tọa độ và xác suất thì mỗi khoảng ứng cử viên có thể được đánh giá theo`O(1)`. Thật không may là có`O(length^2)`khoảng thời gian cho mỗi hành khách. Trong trường hợp xấu nhất, một hành khách đi khắp nơi`150000`dừng lại, điều này đã mang lại`10^10`khoảng thời gian. 

Lực lượng vũ phu hoạt động vì mọi hành khách đều độc lập. Sự thất bại xuất phát từ việc tính toán lại cùng một biểu thức trong nhiều khoảng thời gian. 

Bây giờ hãy xem xét kỹ công thức lợi nhuận cho khoảng thời gian ẩn`[C, D]`. 

Cho phép:```
w[i] = (x[i+1] - x[i]) / 2 - c * p[i] / 100
```Đây chính là sự đóng góp được mong đợi của ẩn cạnh`i`. 

Nếu chúng ta ẩn đi một số cạnh liên tiếp thì tổng lợi nhuận kỳ vọng chỉ đơn giản là tổng của chúng.`w[i]`. 

Điều đó biến đổi hoàn toàn vấn đề. Đối với một hành khách`(a, b)`, chúng ta chỉ cần tổng mảng con tối đa bên trong các cạnh`[a, b-1]`. 

Nếu mỗi cạnh đóng góp độc lập thì khoảng ẩn tối ưu chính xác là đoạn liền kề có tổng lớn nhất. 

Vì vậy, toàn bộ vấn đề giảm xuống việc trả lời nhiều truy vấn mảng con tối đa trên một mảng tĩnh. 

Đây là một vấn đề cây phân đoạn cổ điển. Đối với mỗi nút chúng tôi lưu trữ: 

1. Tổng số tiền. 
2. Tổng tiền tố tốt nhất. 
3. Tổng hậu tố tốt nhất. 
4. Tổng mảng con tốt nhất. 

Sau đó, mỗi truy vấn hành khách sẽ trở thành`O(log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(mn²) | O(n) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển xác suất kiểm tra sang dạng thập phân. 

Đối với cạnh`i`giữa các điểm dừng`i`Và`i+1`, số tiền phạt dự kiến ​​đóng là:```
c * p[i] / 100
```2. Xây dựng một mảng`w`kích thước`n-1`. 

Mỗi cạnh đóng góp:```
w[i] = (x[i+1] - x[i]) / 2 - c * p[i] / 100
```Đây là lợi nhuận dự kiến ​​thu được bằng cách ẩn chính xác lợi thế đó. 
3. Quan sát rằng việc ẩn nhiều cạnh liên tiếp sẽ cộng thêm phần đóng góp của chúng. 

Nếu chúng ta ẩn các cạnh khỏi`L`ĐẾN`R`, lợi nhuận kỳ vọng là:```
sum(w[L:R+1])
```bởi vì kỳ vọng là tuyến tính. 
4. Đối với mỗi hành khách`(a, b)`, chúng ta cần đoạn liền kề tốt nhất bên trong các cạnh`[a, b-1]`. 

Đó chính xác là truy vấn tổng mảng con tối đa trên:```
w[a-1 : b-1]
```5. Xây dựng cây phân đoạn`w`. 

Mỗi nút lưu trữ:```
total sum
maximum prefix sum
maximum suffix sum
maximum subarray sum
```6. Hợp nhất hai nút con. 

Giả sử con bên trái là`A`và đứa trẻ bên phải là`B`. 

Sau đó:```
total = A.total + B.total
prefix = max(A.prefix, A.total + B.prefix)
suffix = max(B.suffix, B.total + A.suffix)
best = max(A.best, B.best, A.suffix + B.prefix)
```Trường hợp cuối cùng xử lý các mảng con đi qua điểm giữa. 
7. Truy vấn cây phân đoạn cho từng phạm vi hành khách. 

Nút được trả về chứa tổng mảng con tối đa bên trong khoảng đó. 
8. Chỉ thêm lợi nhuận dương vào câu trả lời. 

Nếu tổng mảng con tốt nhất là âm, người soát vé chỉ cần tránh gian lận cho hành khách đó. 

### Tại sao nó hoạt động 

Mỗi cạnh ẩn đóng góp độc lập vào lợi nhuận dự kiến. Sự đóng góp chỉ phụ thuộc vào độ dài của cạnh đó và xác suất kiểm tra chứ không phụ thuộc vào các cạnh lân cận. 

Bởi vì kỳ vọng có tính cộng, lợi nhuận kỳ vọng của việc ẩn một khoảng liên tục bằng tổng đóng góp của cạnh trong khoảng đó. 

Vì vậy, đối với mỗi hành khách, chiến lược tối ưu là chọn quãng đường liền kề với tổng trọng lượng tối đa. Cây phân đoạn tính toán chính xác số lượng đó bằng cách sử dụng bất biến hợp nhất mảng con tối đa tiêu chuẩn. 

Mọi truy vấn đều trả về kế hoạch gian lận tốt nhất có thể cho hành khách đó và hành khách độc lập, vì vậy việc tính tổng các giá trị tối ưu này sẽ mang lại kết quả tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("total", "pref", "suff", "best")

    def __init__(self, total=0.0, pref=0.0, suff=0.0, best=0.0):
        self.total = total
        self.pref = pref
        self.suff = suff
        self.best = best

def merge(a, b):
    res = Node()
    res.total = a.total + b.total
    res.pref = max(a.pref, a.total + b.pref)
    res.suff = max(b.suff, b.total + a.suff)
    res.best = max(a.best, b.best, a.suff + b.pref)
    return res

n, m, c = map(int, input().split())

x = list(map(int, input().split()))
p = list(map(int, input().split()))

w = []
for i in range(n - 1):
    dist = x[i + 1] - x[i]
    val = dist / 2.0 - c * p[i] / 100.0
    w.append(val)

size = 1
while size < n - 1:
    size <<= 1

seg = [Node() for _ in range(2 * size)]

for i in range(n - 1):
    v = w[i]
    seg[size + i] = Node(v, v, v, v)

for i in range(size - 1, 0, -1):
    seg[i] = merge(seg[i << 1], seg[i << 1 | 1])

def query(l, r):
    left_res = None
    right_res = None

    l += size
    r += size

    while l <= r:
        if l & 1:
            if left_res is None:
                left_res = seg[l]
            else:
                left_res = merge(left_res, seg[l])
            l += 1

        if not (r & 1):
            if right_res is None:
                right_res = seg[r]
            else:
                right_res = merge(seg[r], right_res)
            r -= 1

        l >>= 1
        r >>= 1

    if left_res is None:
        return right_res

    if right_res is None:
        return left_res

    return merge(left_res, right_res)

ans = 0.0

for _ in range(m):
    a, b = map(int, input().split())

    res = query(a - 1, b - 2)

    if res.best > 0:
        ans += res.best

print(f"{ans:.9f}")
```Chuyển đổi đầu tiên là bước thực hiện quan trọng. Thay vì suy luận trực tiếp về vé và việc kiểm tra, mã sẽ xây dựng một mảng trọng số trên các cạnh. Mỗi cạnh đều lưu trữ đóng góp dự kiến ​​của nó nếu bị ẩn. 

Cây phân đoạn lưu trữ bốn giá trị cho mỗi nút vì các truy vấn mảng con tối đa cần nhiều hơn là chỉ tổng số. Trường hợp chéo yêu cầu hậu tố tốt nhất từ ​​con bên trái và tiền tố tốt nhất từ ​​con bên phải. 

Một chi tiết tinh tế là lập chỉ mục. Điểm dừng được đánh số từ`1`, nhưng các cạnh được lập chỉ mục tự nhiên từ`0`. 

Một hành khách`(a, b)`có thể ẩn các cạnh:```
[a-1, b-2]
```bởi vì cạnh`i`kết nối dừng lại`i+1`dừng lại`i+2`. 

Một chi tiết quan trọng khác là yếu tố trung tính. Chúng tôi không sử dụng nút 0 giả trong quá trình truy vấn lặp lại vì nó sẽ cho phép các mảng con trống trong quá trình hợp nhất một cách không chính xác. Thay vào đó, việc triển khai sử dụng`None`và khởi tạo một cách lười biếng. 

Cây lá xa hơn`n-1`vẫn có giá trị bằng 0. Chúng không bao giờ được truy vấn vì tất cả các phạm vi đều nằm trong mảng cạnh thực. 

Độ chính xác của dấu phẩy động ở đây là an toàn vì tất cả các phép toán đều là phép cộng và cực đại trên các giá trị được giới hạn bởi khoảng`1e9`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3 10
0 10 100
100 0
1 2
2 3
1 3
```Trọng lượng cạnh: 

| Cạnh | Khoảng cách | Đạt được | Dự Kiến Phạt | Cân nặng | 
| --- | --- | --- | --- | --- | 
| 1-2 | 10 | 5 | 10 | -5 | 
| 2-3 | 90 | 45 | 0 | 45 | 

Truy vấn của hành khách: 

| Hành khách | Phạm vi cạnh | Tổng mảng con tốt nhất | 
| --- | --- | --- | 
| 1 -> 2 | [-5] | -5 | 
| 2 -> 3 | [45] | 45 | 
| 1 -> 3 | [-5, 45] | 45 | 

Tổng số câu trả lời:```
0 + 45 + 45 = 90
```Ví dụ này chứng minh tại sao nên bỏ qua các cạnh âm. Hành khách đầu tiên sẽ an toàn hơn nếu không gian lận. 

### Ví dụ tùy chỉnh 

đầu vào:```
4 2 20
0 10 30 50
0 100 0
1 4
2 4
```Trọng lượng cạnh: 

| Cạnh | Khoảng cách | Đạt được | Dự Kiến Phạt | Cân nặng | 
| --- | --- | --- | --- | --- | 
| 1-2 | 10 | 5 | 0 | 5 | 
| 2-3 | 20 | 10 | 20 | -10 | 
| 3-4 | 20 | 10 | 0 | 10 | 

Xử lý hành khách: 

| Hành khách | Mảng ứng viên | Phân đoạn hay nhất | Lợi nhuận | 
| --- | --- | --- | --- | 
| 1 -> 4 | [5, -10, 10] | [10] | 10 | 
| 2 -> 4 | [-10, 10] | [10] | 10 | 

Câu trả lời cuối cùng:```
20
```Dấu vết này cho thấy khoảng thời gian ẩn tối ưu có thể tránh được hoàn toàn các cạnh giữa nguy hiểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Xây dựng cây phân đoạn cộng với một truy vấn cho mỗi hành khách | 
| Không gian | O(n) | Cây phân đoạn lưu trữ số nút tuyến tính | 

Với`n = 150000`Và`m = 300000`, sự phức tạp này dễ dàng phù hợp trong giới hạn. Mỗi truy vấn chỉ chạm vào`O(log n)`các nút, do đó tổng số thao tác vẫn có thể quản lý được trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    input = input_data.readline

    class Node:
        __slots__ = ("total", "pref", "suff", "best")

        def __init__(self, total=0.0, pref=0.0, suff=0.0, best=0.0):
            self.total = total
            self.pref = pref
            self.suff = suff
            self.best = best

    def merge(a, b):
        res = Node()
        res.total = a.total + b.total
        res.pref = max(a.pref, a.total + b.pref)
        res.suff = max(b.suff, b.total + a.suff)
        res.best = max(a.best, b.best, a.suff + b.pref)
        return res

    n, m, c = map(int, input().split())

    x = list(map(int, input().split()))
    p = list(map(int, input().split()))

    w = []
    for i in range(n - 1):
        w.append((x[i + 1] - x[i]) / 2.0 - c * p[i] / 100.0)

    size = 1
    while size < n - 1:
        size <<= 1

    seg = [Node() for _ in range(2 * size)]

    for i, v in enumerate(w):
        seg[size + i] = Node(v, v, v, v)

    for i in range(size - 1, 0, -1):
        seg[i] = merge(seg[i << 1], seg[i << 1 | 1])

    def query(l, r):
        left_res = None
        right_res = None

        l += size
        r += size

        while l <= r:
            if l & 1:
                left_res = seg[l] if left_res is None else merge(left_res, seg[l])
                l += 1

            if not (r & 1):
                right_res = seg[r] if right_res is None else merge(seg[r], right_res)
                r -= 1

            l >>= 1
            r >>= 1

        if left_res is None:
            return right_res
        if right_res is None:
            return left_res

        return merge(left_res, right_res)

    ans = 0.0

    for _ in range(m):
        a, b = map(int, input().split())
        ans += max(0.0, query(a - 1, b - 2).best)

    output_data.write(f"{ans:.9f}\n")
    return output_data.getvalue()

# provided sample
assert run(
"""3 3 10
0 10 100
100 0
1 2
2 3
1 3
"""
).strip() == "90.000000000", "sample 1"

# minimum size
assert run(
"""2 1 1
0 10
0
1 2
"""
).strip() == "5.000000000", "minimum case"

# always bad to cheat
assert run(
"""2 1 1000
0 10
100
1 2
"""
).strip() == "0.000000000", "negative expectation"

# middle edge should be skipped
assert run(
"""4 1 20
0 10 30 50
0 100 0
1 4
"""
).strip() == "10.000000000", "best subarray in middle"

# off-by-one boundary check
assert run(
"""5 2 10
0 10 20 30 40
0 0 100 0
1 3
3 5
"""
).strip() == "10.000000000", "boundary ranges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn không cần kiểm tra | 5 | Đầu vào hợp lệ tối thiểu | 
| Đảm bảo kiểm tra với mức phạt rất lớn | 0 | Phải cho phép khoảng trống | 
| Trọng số dương-âm-dương | 10 | Logic mảng con tối đa | 
| Truy vấn chạm vào ranh giới tuyến đường | 10 | Độ chính xác từng cái một | 

## Vỏ cạnh 

Hãy xem xét trường hợp mọi phân đoạn ẩn đều mất tiền.```
2 1 1000
0 10
100
1 2
```Cạnh duy nhất góp phần:```
10 / 2 - 1000 = -995
```Cây phân đoạn trả về`-995`, nhưng thuật toán chỉ cộng thêm lợi nhuận dương. Câu trả lời cuối cùng trở thành`0`, phù hợp với chiến lược đúng đắn là không bao giờ gian lận. 

Bây giờ hãy xem xét một tuyến đường trong đó khoảng thời gian tối ưu bỏ qua các cạnh nguy hiểm.```
4 1 100
0 10 20 100
0 100 0
1 4
```Trọng lượng là:```
5, -90, 40
```Phân mảng tối đa là`[40]`, không phải toàn bộ tuyến đường. Cây phân đoạn tính toán chính xác điều này vì mỗi nút lưu trữ thông tin tiền tố, hậu tố và giao nhau. 

Cuối cùng, hãy xem xét cạm bẫy của kỳ vọng.```
3 1 10
0 10 20
50 50
1 3
```Trọng lượng trở thành:```
0, 0
```Mỗi cạnh đóng góp độc lập:```
10 / 2 - 10 * 0.5 = 0
```Tổng lợi nhuận dự kiến ​​là`0`. Thuật toán tính tổng các kỳ vọng theo từng cạnh, do đó, nó không bao giờ kết hợp sai các xác suất theo cấp số nhân.
