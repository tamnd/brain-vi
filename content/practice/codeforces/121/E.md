---
title: "CF 121E - Mảng may mắn"
description: "Chúng tôi có một mảng có giá trị luôn lớn nhất là 10000. Hai thao tác phải được xử lý trực tuyến. Thao tác đầu tiên thêm giá trị d vào mọi phần tử bên trong phân đoạn [l, r]."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 121
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 91 (Div. 1 Only)"
rating: 2400
weight: 121
solve_time_s: 141
verified: true
draft: false
---

[CF 121E - Mảng may mắn](https://codeforces.com/problemset/problem/121/E) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng có giá trị luôn lớn nhất`10000`. Hai thao tác phải được xử lý trực tuyến. 

Thao tác đầu tiên thêm một giá trị`d`tới mọi phần tử bên trong một phân đoạn`[l, r]`. 

Thao tác thứ hai hỏi bên trong có bao nhiêu số`[l, r]`thật may mắn, trong đó con số may mắn chỉ bao gồm các chữ số`4`Và`7`. 

Khó khăn là không nhận ra con số may mắn. Có rất ít trong số chúng dưới đây`10000`. Thách thức thực sự là hỗ trợ lên tới`10^5`bổ sung phạm vi và`10^5`phạm vi truy vấn đủ nhanh. 

Một mô phỏng trực tiếp ngay lập tức gặp rắc rối. Giả sử mọi thao tác đều cập nhật gần như toàn bộ mảng. Tính toán lại từng giá trị bị ảnh hưởng`O(n)`mỗi lần cập nhật. Với`10^5`hoạt động và`10^5`các yếu tố, điều này trở nên đại khái`10^10`cập nhật, vượt xa giới hạn. 

Giới hạn mà mọi giá trị luôn đạt giá trị lớn nhất`10000`là thuộc tính cấu trúc quan trọng. chỉ có`2 + 4 + 8 + 16 = 30`con số may mắn trong phạm vi này. Thay vì theo dõi các giá trị chính xác một cách tốn kém, chúng ta có thể theo dõi xem mỗi giá trị còn cách bao xa để trở thành may mắn. 

Một số trường hợp đặc biệt khiến cho việc tối ưu hóa đơn giản không thành công. 

Hãy xem xét đầu vào này:```
3 2
3 4 5
add 1 3 1
count 1 3
```Sau khi thêm mảng trở thành`[4,5,6]`, vậy câu trả lời là`1`. Bất kỳ triển khai nào chỉ cập nhật các vị trí may mắn trước đó sẽ bỏ lỡ điều đó`3`trở nên may mắn. 

Một trường hợp tế nhị khác:```
3 3
4 7 44
add 1 3 1
add 1 3 1
count 1 3
```Mảng phát triển như`[5,8,45]`sau đó`[6,9,46]`. Câu trả lời đúng là`0`. Một cấu trúc lười biếng chỉ lưu trữ liệu một vị trí hiện có may mắn hay không mà không tính toán lại các chuyển đổi, sẽ giữ sai số lượng may mắn cũ. 

Tình huống quan trọng thứ ba xuất hiện khi một phân đoạn nhận được nhiều bổ sung lười biếng:```
5 3
1 2 3 4 5
add 1 5 100
add 1 5 200
count 1 5
```Mảng cuối cùng là`[301,302,303,304,305]`, và chỉ`304`có may mắn không? Không, không phải vậy, vì số may mắn chỉ chứa các chữ số`4`Và`7`. Câu trả lời đúng là`0`. Điều này phát hiện các triển khai vô tình diễn giải “chứa 4 hoặc 7” thay vì “tất cả các chữ số là 4 hoặc 7”. 

## Phương pháp tiếp cận 

Giải pháp vũ phu rất đơn giản. Đối với mọi`add`hoạt động, lặp lại toàn bộ khoảng thời gian và cập nhật từng phần tử. Đối với mọi`count`hoạt động, lặp lại và kiểm tra từng số riêng lẻ. 

Việc kiểm tra xem một số có may mắn hay không chỉ cần kiểm tra tối đa bốn chữ số vì các giá trị không bao giờ vượt quá`10000`. Vì vậy, phần tốn kém là quét liên tục. 

Trong trường hợp xấu nhất, mọi thao tác đều chạm vào toàn bộ mảng. Điều đó mang lại`10^5 × 10^5 = 10^10`cập nhật hoặc kiểm tra phần tử. Ngay cả trong C++, điều này là vô vọng dưới giới hạn 4 giây. 

Quan sát quan trọng là tập hợp các số may mắn rất nhỏ. Chỉ có 30 con số may mắn tối đa`10000`. 

Đối với một giá trị`x`, điều quan trọng không phải là con số chính xác mà là khoảng cách đến con số may mắn tiếp theo. Giả sử con số may mắn tiếp theo bằng hoặc cao hơn`x`là`L`. Định nghĩa:```
delta = L - x
```Nếu như`delta == 0`, số hiện tại là may mắn. 

Bây giờ hãy xem xét việc thêm`d`đến toàn bộ phân khúc. Giá trị trở thành`x + d`, do đó khoảng cách mới trở thành:```
L - (x + d) = delta - d
```Phép cộng phạm vi chỉ đơn giản là trừ`d`từ khắp vùng đồng bằng. 

Điều này biến vấn đề thành một cái gì đó mà cây phân đoạn xử lý rất tự nhiên. 

Đối với mỗi nút cây phân đoạn, chúng tôi duy trì:```
mn = minimum delta in this segment
cnt = how many positions have delta == 0
```Khi chúng ta áp dụng phép cộng lười, tất cả các delta đều dịch chuyển đồng đều, vì vậy`mn`cũng dịch chuyển đều. 

Bí quyết quan trọng là đây: 

Nếu sau khi trừ`d`, nút vẫn có`mn > 0`, thì không có phần tử nào trong đoạn đó trở nên may mắn. Chúng ta có thể dừng lại ngay lập tức mà không cần đi xuống thêm. 

Chúng tôi chỉ đẩy sâu hơn khi một số delta trở nên không dương. Vì mỗi phần tử chỉ có thể vượt qua ngưỡng may mắn tổng cộng khoảng 30 lần nên tổng số lần hạ xuống đắt tiền vẫn có thể kiểm soát được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nm)`|`O(n)`| Quá chậm | 
| Tối ưu |`O((n + m) log n log C)`khấu hao |`O(n)`| Đã chấp nhận | 

Đây`C`là giá trị giới hạn tối đa, khoảng`10000`. 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các con số may mắn lên đến`10000`. 

DFS hoặc BFS trên chữ số`4`Và`7`chỉ tạo ra 30 số, vì vậy bước này không quan trọng. 
2. Với mọi giá trị mảng ban đầu`a[i]`, tính số may mắn tiếp theo`L >= a[i]`. 

Cửa hàng:```
delta[i] = L - a[i]
```Nếu như`delta[i] == 0`, sau đó`a[i]`đã là may mắn rồi. 
3. Xây dựng cây phân đoạn. 

Mỗi nút lưu trữ:```
mn  = minimum delta in the segment
cnt = number of zeros in the segment
lazy = pending subtraction applied to all deltas
```Một chiếc lá với`delta == 0`có`cnt = 1`, nếu không thì`cnt = 0`. 
4. Đối với một`count l r`truy vấn, trả về tổng của`cnt`giá trị trong khoảng thời gian được yêu cầu. 

Một vị trí đóng góp chính xác khi delta của nó bằng 0. 
5. Đối với một`add l r d`hoạt động, trừ`d`từ tất cả các vùng đồng bằng trong khoảng. 

Về mặt khái niệm, mọi giá trị được di chuyển`d`gần hơn với con số may mắn tiếp theo của nó. 
6. Khi cập nhật một nút cây phân đoạn được bao phủ hoàn toàn bởi bản cập nhật, hãy giảm:```
mn -= d
lazy -= d
```Nếu sau đó`mn > 0`, thì mọi delta vẫn dương nên không có yếu tố nào trở thành may mắn. Nút vẫn hợp lệ. 
7. Nếu`mn <= 0`, ít nhất một phần tử đã vượt qua ranh giới may mắn. 

Chúng ta phải đệ quy đi xuống để tính toán lại các lá bị ảnh hưởng. 
8. Ở một chiếc lá, khi`delta <= 0`, giá trị hiện tại đã đạt hoặc vượt qua con số may mắn được theo dõi. 

Chúng tôi cập nhật giá trị mảng thực tế, tính toán lại số may mắn tiếp theo và xây dựng lại trạng thái lá. 
9. Sau khi cập nhật đệ quy, hợp nhất các phần tử con trở lên:```
mn = min(left.mn, right.mn)
cnt = left.cnt + right.cnt
```### Tại sao nó hoạt động 

Mỗi lá luôn lưu trữ khoảng cách từ giá trị hiện tại của nó đến số may mắn tiếp theo lớn hơn hoặc bằng nó. 

Việc bổ sung phạm vi sẽ làm giảm các khoảng cách này một cách đồng đều. Nếu tất cả các khoảng cách đều dương thì không có giá trị nào trở thành may mắn và không cần tính toán lại. 

Bất cứ khi nào một khoảng cách trở nên không dương, vị trí đó đã vượt qua ngưỡng may mắn. Tính lại số may mắn tiếp theo sẽ khôi phục bất biến. 

Bởi vì mỗi giá trị chỉ vượt qua nhiều nhất một số may mắn tại một thời điểm và chỉ có 30 số may mắn bên dưới`10000`, mỗi phần tử chỉ được xây dựng lại một số lần nhỏ trong toàn bộ quá trình thực thi. Cây phân đoạn luôn duy trì số lượng may mắn chính xác cho mỗi khoảng thời gian. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def generate_lucky():
    res = []

    def dfs(x):
        if x > 10000:
            return
        if x:
            res.append(x)
        dfs(x * 10 + 4)
        dfs(x * 10 + 7)

    dfs(0)
    res.sort()
    return res

LUCKY = generate_lucky()

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.arr = arr[:]

        size = 4 * self.n + 5
        self.mn = [0] * size
        self.cnt = [0] * size
        self.lazy = [0] * size

        self.build(1, 0, self.n - 1)

    def next_delta(self, x):
        idx = bisect_left(LUCKY, x)
        return LUCKY[idx] - x

    def apply(self, node, val):
        self.mn[node] += val
        self.lazy[node] += val

    def push(self, node):
        if self.lazy[node]:
            v = self.lazy[node]
            self.apply(node * 2, v)
            self.apply(node * 2 + 1, v)
            self.lazy[node] = 0

    def pull(self, node):
        self.mn[node] = min(self.mn[node * 2], self.mn[node * 2 + 1])
        self.cnt[node] = self.cnt[node * 2] + self.cnt[node * 2 + 1]

    def build(self, node, l, r):
        if l == r:
            d = self.next_delta(self.arr[l])
            self.mn[node] = d
            self.cnt[node] = 1 if d == 0 else 0
            return

        mid = (l + r) // 2

        self.build(node * 2, l, mid)
        self.build(node * 2 + 1, mid + 1, r)

        self.pull(node)

    def rebuild_leaf(self, node, idx):
        d = self.next_delta(self.arr[idx])
        self.mn[node] = d
        self.cnt[node] = 1 if d == 0 else 0

    def range_add(self, node, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.apply(node, -val)

            if self.mn[node] > 0:
                return

        if l == r:
            self.arr[l] += val
            self.rebuild_leaf(node, l)
            return

        self.push(node)

        mid = (l + r) // 2

        if ql <= mid:
            self.range_add(node * 2, l, mid, ql, qr, val)

        if qr > mid:
            self.range_add(node * 2 + 1, mid + 1, r, ql, qr, val)

        self.pull(node)

    def query(self, node, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.cnt[node]

        self.push(node)

        mid = (l + r) // 2
        ans = 0

        if ql <= mid:
            ans += self.query(node * 2, l, mid, ql, qr)

        if qr > mid:
            ans += self.query(node * 2 + 1, mid + 1, r, ql, qr)

        return ans

def solve():
    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    seg = SegTree(arr)

    out = []

    for _ in range(m):
        parts = input().split()

        if parts[0] == "add":
            l = int(parts[1]) - 1
            r = int(parts[2]) - 1
            d = int(parts[3])

            seg.range_add(1, 0, n - 1, l, r, d)

        else:
            l = int(parts[1]) - 1
            r = int(parts[2]) - 1

            out.append(str(seg.query(1, 0, n - 1, l, r)))

    sys.stdout.write("\n".join(out))

solve()
```Phần đầu tiên tạo ra tất cả các số may mắn lên đến`10000`. Vì số lượng rất nhỏ nên việc tạo đệ quy đơn giản và rõ ràng hơn so với việc kiểm tra từng số nguyên riêng lẻ. 

Cây phân đoạn không bao giờ lưu trữ các giá trị thực tế ngoại trừ tại các điểm kết thúc`self.arr`. Các nút nội bộ chỉ theo dõi khoảng cách tối thiểu tới một số may mắn và số lượng vị trí may mắn hiện tại. 

Chi tiết tinh tế nhất là quy ước ký hiệu trong quá trình cập nhật. 

Nếu một giá trị tăng thêm`d`, khi đó khoảng cách của nó tới số may mắn tiếp theo sẽ giảm đi`d`. Đó là lý do tại sao áp dụng cập nhật`-d`ĐẾN`mn`Và`lazy`. 

Một sai lầm dễ mắc phải khác là dừng đệ quy quá sớm. Chúng ta chỉ có thể dừng lại khi`mn > 0`. Nếu như`mn == 0`, một số phần tử chính xác là may mắn và nút phải lưu giữ thông tin đó một cách chính xác. 

Tại lá, chúng tôi xây dựng lại bằng cách sử dụng giá trị cập nhật thực tế. Chúng ta không thể cứ mãi trừ các delta mãi mãi vì khi một giá trị vượt qua con số may mắn được theo dõi của nó thì “may mắn tiếp theo” sẽ thay đổi. 

Việc triển khai lưu trữ các giá trị lười biếng đang chờ xử lý ngay cả đối với các nút sau đó lặp lại. Điều này đảm bảo trẻ em luôn nhận được các bản cập nhật tích lũy chính xác trước khi thực hiện các hoạt động tiếp theo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 6
2 3 4
count 1 3
count 1 2
add 1 3 2
count 1 3
add 2 3 3
count 1 3
```Đồng bằng ban đầu: 

| Vị trí | Giá trị | May mắn tiếp theo | Đồng bằng | 
| --- | --- | --- | --- | 
| 1 | 2 | 4 | 2 | 
| 2 | 3 | 4 | 1 | 
| 3 | 4 | 4 | 0 | 

Truy vấn đầu tiên: 

| Truy vấn | Vị trí may mắn | Trả lời | 
| --- | --- | --- | 
| đếm 1 3 | {3} | 1 | 

Truy vấn thứ hai: 

| Truy vấn | Vị trí may mắn | Trả lời | 
| --- | --- | --- | 
| đếm 1 2 | {} | 0 | 

Sau đó`add 1 3 2`: 

| Vị trí | Giá trị cũ | Giá trị mới | Đồng bằng mới | 
| --- | --- | --- | --- | 
| 1 | 2 | 4 | 0 | 
| 2 | 3 | 5 | 2 | 
| 3 | 4 | 6 | 1 | 

Truy vấn thứ ba: 

| Truy vấn | Vị trí may mắn | Trả lời | 
| --- | --- | --- | 
| đếm 1 3 | {1} | 1 | 

Sau đó`add 2 3 3`: 

| Vị trí | Giá trị cũ | Giá trị mới | Đồng bằng mới | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | 0 | 
| 2 | 5 | 8 | 36 | 
| 3 | 6 | 9 | 35 | 

Truy vấn cuối cùng: 

| Truy vấn | Vị trí may mắn | Trả lời | 
| --- | --- | --- | 
| đếm 1 3 | {1} | 1 | 

Dấu vết này cho thấy tại sao những chiếc lá phải được xây dựng lại sau khi vượt qua ngưỡng may mắn. Vị trí 2 chuyển từ nhắm mục tiêu vào con số may mắn`7`để nhắm mục tiêu`44`. 

### Mẫu 2 

đầu vào:```
4 3
4 44 5 6
count 1 4
add 3 4 1
count 1 4
```Trạng thái ban đầu: 

| Vị trí | Giá trị | May mắn | 
| --- | --- | --- | 
| 1 | 4 | Có | 
| 2 | 44 | Có | 
| 3 | 5 | Không | 
| 4 | 6 | Không | 

Truy vấn đầu tiên: 

| Truy vấn | Trả lời | 
| --- | --- | 
| đếm 1 4 | 2 | 

Sau khi cập nhật: 

| Vị trí | Giá trị cũ | Giá trị mới | May mắn | 
| --- | --- | --- | --- | 
| 3 | 5 | 6 | Không | 
| 4 | 6 | 7 | Có | 

Truy vấn thứ hai: 

| Truy vấn | Trả lời | 
| --- | --- | 
| đếm 1 4 | 3 | 

Ví dụ này chứng minh rằng một giá trị không may mắn có thể trở thành may mắn chính xác khi delta của nó bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n + m) log n log C)`khấu hao | Mỗi lần xây dựng lại đi qua cây và mọi phần tử chỉ vượt qua một số ít ngưỡng may mắn | 
| Không gian |`O(n)`| Mảng cây phân đoạn và giá trị được lưu trữ | 

Chỉ có 30 con số may mắn bên dưới`10000`, do đó, mỗi vị trí chỉ có thể kích hoạt logic xây dựng lại đắt tiền trong một số lần nhất định. Độ phức tạp được khấu hao dễ dàng phù hợp với giới hạn của`10^5`hoạt động. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def generate_lucky():
        res = []

        def dfs(x):
            if x > 10000:
                return
            if x:
                res.append(x)
            dfs(x * 10 + 4)
            dfs(x * 10 + 7)

        dfs(0)
        res.sort()
        return res

    LUCKY = generate_lucky()

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.arr = arr[:]

            size = 4 * self.n + 5
            self.mn = [0] * size
            self.cnt = [0] * size
            self.lazy = [0] * size

            self.build(1, 0, self.n - 1)

        def next_delta(self, x):
            idx = bisect_left(LUCKY, x)
            return LUCKY[idx] - x

        def apply(self, node, val):
            self.mn[node] += val
            self.lazy[node] += val

        def push(self, node):
            if self.lazy[node]:
                v = self.lazy[node]
                self.apply(node * 2, v)
                self.apply(node * 2 + 1, v)
                self.lazy[node] = 0

        def pull(self, node):
            self.mn[node] = min(self.mn[node * 2], self.mn[node * 2 + 1])
            self.cnt[node] = self.cnt[node * 2] + self.cnt[node * 2 + 1]

        def build(self, node, l, r):
            if l == r:
                d = self.next_delta(self.arr[l])
                self.mn[node] = d
                self.cnt[node] = 1 if d == 0 else 0
                return

            mid = (l + r) // 2

            self.build(node * 2, l, mid)
            self.build(node * 2 + 1, mid + 1, r)

            self.pull(node)

        def rebuild_leaf(self, node, idx):
            d = self.next_delta(self.arr[idx])
            self.mn[node] = d
            self.cnt[node] = 1 if d == 0 else 0

        def range_add(self, node, l, r, ql, qr, val):
            if ql <= l and r <= qr:
                self.apply(node, -val)

                if self.mn[node] > 0:
                    return

            if l == r:
                self.arr[l] += val
                self.rebuild_leaf(node, l)
                return

            self.push(node)

            mid = (l + r) // 2

            if ql <= mid:
                self.range_add(node * 2, l, mid, ql, qr, val)

            if qr > mid:
                self.range_add(node * 2 + 1, mid + 1, r, ql, qr, val)

            self.pull(node)

        def query(self, node, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.cnt[node]

            self.push(node)

            mid = (l + r) // 2
            ans = 0

            if ql <= mid:
                ans += self.query(node * 2, l, mid, ql, qr)

            if qr > mid:
                ans += self.query(node * 2 + 1, mid + 1, r, ql, qr)

            return ans

    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    seg = SegTree(arr)

    out = []

    for _ in range(m):
        parts = input().split()

        if parts[0] == "add":
            l = int(parts[1]) - 1
            r = int(parts[2]) - 1
            d = int(parts[3])

            seg.range_add(1, 0, n - 1, l, r, d)

        else:
            l = int(parts[1]) - 1
            r = int(parts[2]) - 1

            out.append(str(seg.query(1, 0, n - 1, l, r)))

    return "\n".join(out)

# provided sample
assert run(
"""3 6
2 3 4
count 1 3
count 1 2
add 1 3 2
count 1 3
add 2 3 3
count 1 3
"""
) == "1\n0\n1\n1"

# minimum size
assert run(
"""1 2
4
count 1 1
count 1 1
"""
) == "1\n1"

# becoming lucky after update
assert run(
"""2 2
3 6
add 1 2 1
count 1 2
"""
) == "2"

# all equal values
assert run(
"""5 3
7 7 7 7 7
count 1 5
add 2 4 1
count 1 5
"""
) == "5\n2"

# boundary interval update
assert run(
"""5 4
1 2 3 4 5
add 1 5 3
count 1 5
add 5 5 2
count 4 5
"""
) == "2\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố duy nhất đã may mắn |`1 1`| Xử lý kích thước tối thiểu | 
| Giá trị trở nên may mắn |`2`| Vượt ngưỡng may mắn | 
| Tất cả các giá trị ban đầu may mắn |`5 2`| Giảm đúng sau khi cập nhật | 
| Cập nhật đầy đủ và hậu tố |`2 1`| Tính chính xác và lập chỉ mục ranh giới | 

## Vỏ cạnh 

Hãy xem xét trường hợp một giá trị trở thành may mắn chính xác sau khi cập nhật. 

đầu vào:```
1 2
3
add 1 1 1
count 1 1
```Ban đầu con số may mắn tiếp theo là`4`, Vì thế`delta = 1`. Sau khi thêm`1`, cập nhật phân đoạn sẽ trừ đi`1`từ đồng bằng, sản xuất`0`. Từ`mn <= 0`, thuật toán đi xuống lá và xây dựng lại nó. Giá trị trở thành`4`, thật là may mắn, vậy`cnt = 1`. Truy vấn trả về chính xác`1`. 

Bây giờ hãy xem xét việc để lại một con số may mắn. 

đầu vào:```
1 2
4
add 1 1 1
count 1 1
```Ban đầu`delta = 0`. Sau khi thêm`1`, đồng bằng trở thành`-1`, buộc phải xây dựng lại. Giá trị mới là`5`, con số may mắn tiếp theo của ai là`7`, do đó đồng bằng được xây dựng lại trở thành`2`Và`cnt = 0`. Câu trả lời là chính xác`0`. 

Cuối cùng, hãy xem xét việc cập nhật lười biếng lặp đi lặp lại mà không cần xây dựng lại hầu hết các nút. 

đầu vào:```
5 3
1 1 1 1 1
add 1 5 1
add 1 5 1
count 1 5
```Mỗi giá trị di chuyển từ`1`ĐẾN`3`. Con số may mắn tiếp theo của họ vẫn còn`4`, vì vậy đồng bằng châu thổ phát triển từ`3`ĐẾN`2`ĐẾN`1`. Vì mức tối thiểu của phân đoạn không bao giờ trở thành số dương nên thuật toán không bao giờ giảm xuống một cách không cần thiết. Câu trả lời cuối cùng là`0`và chi phí cập nhật vẫn gần logarit thay vì chạm vào từng lá.
