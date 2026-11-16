---
title: "CF 19D - Điểm"
description: "Chúng tôi duy trì một tập hợp các điểm động trên mặt phẳng 2D. Ba hoạt động được hỗ trợ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 19
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 19"
rating: 2800
weight: 19
solve_time_s: 229
verified: true
draft: false
---
[CF 19D - Điểm](https://codeforces.com/problemset/problem/19/D) 

**Đánh giá:** 2800 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 3 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một tập hợp các điểm động trên mặt phẳng 2D. Ba hoạt động được hỗ trợ.`add x y`chèn một điểm.`remove x y`xóa một điểm hiện có.`find x y`yêu cầu điểm có tọa độ lớn hơn`(x, y)`ở cả hai chiều, với một quy tắc ràng buộc rất cụ thể. 

Trong số tất cả các điểm thỏa mãn: 

-`px > x`-`py > y`chúng tôi muốn cái nhỏ nhất có thể`px`. Nếu một số điểm có chung điểm đó`px`, ta chọn số nhỏ nhất`py`. 

Vì vậy, truy vấn không yêu cầu điểm hợp lệ nhỏ nhất trên toàn cầu. Đầu tiên nó thu nhỏ tọa độ x, sau đó thu nhỏ tọa độ y. 

Số lượng hoạt động đạt`2 * 10^5`, và tọa độ đi lên`10^9`. Các tọa độ lớn ngay lập tức đề xuất nén tọa độ, vì chúng ta không thể xây dựng các mảng được lập chỉ mục trực tiếp theo các giá trị tọa độ. 

Số lượng hoạt động là hạn chế nghiêm trọng hơn. Quét đơn giản trên tất cả các điểm cho mọi`find`truy vấn sẽ mất thời gian bậc hai trong trường hợp xấu nhất. Với`2 * 10^5`hoạt động, một`O(n)`truy vấn có thể dễ dàng tạo ra xung quanh`4 * 10^10`so sánh, điều này hoàn toàn không thể thực hiện được trong vòng 2 giây. 

Chúng ta cần một cái gì đó gần với thời gian logarit cho mỗi phép tính. 

Phần khó khăn là quy tắc đặt hàng không đối xứng. Chúng ta không chỉ cần bất kỳ điểm nào ở trên và bên phải. Chúng tôi đặc biệt cần tọa độ x hợp lệ ngoài cùng bên trái và chỉ sau khi sửa tọa độ x đó, chúng tôi mới giảm thiểu y. 

Cấu trúc đó chính là chìa khóa của giải pháp. 

Hãy xem xét ví dụ này:```
add 2 100
add 3 1
find 1 50
```Câu trả lời đúng là`(2, 100)`. 

Một cách tiếp cận bất cẩn làm giảm thiểu y trên toàn cầu trong số các điểm hợp lệ sẽ trả về`(3, 1)`, điều này sai vì x có mức độ ưu tiên cao hơn. 

Một trường hợp tế nhị khác là sự bất bình đẳng nghiêm ngặt.```
add 5 5
find 5 4
```Câu trả lời là`-1`. 

Mặc dù`y > 4`, tọa độ x không lớn hơn`5`. 

Tọa độ x trùng lặp cũng có vấn đề.```
add 7 10
add 7 3
find 5 4
```Câu trả lời là`(7, 10)`. 

điểm`(7, 3)`thất bại vì`3`không lớn hơn`4`. Chúng ta không thể chỉ lưu trữ y tối thiểu cho mỗi tọa độ x. Chúng ta cần biết liệu có tồn tại một số y lớn hơn truy vấn hay không. 

Việc xóa động tạo ra một chế độ lỗi phổ biến khác.```
add 4 8
remove 4 8
find 1 1
```Câu trả lời là`-1`. 

Mọi cấu trúc được lưu trong bộ nhớ đệm đều phải xóa nội dung đóng góp một cách chính xác khi điểm biến mất. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Duy trì tất cả các điểm hiện tại trong một tập hợp. Đối với mỗi`find x y`, lặp qua từng điểm và giữ lại ứng viên tốt nhất theo thứ tự bài toán. 

Điều này hoạt động vì định nghĩa truy vấn rõ ràng. Chúng tôi chỉ đơn giản kiểm tra mọi điểm thỏa mãn:```
px > x and py > y
```và giữ từ điển nhỏ nhất`(px, py)`. 

Việc chèn và xóa thật dễ dàng với bộ băm nhưng các truy vấn trở nên tốn kém. Trong trường hợp xấu nhất chúng ta có thể có`2 * 10^5`điểm hoạt động và`2 * 10^5`truy vấn, tạo ra khoảng`4 * 10^10`séc. 

Nút cổ chai đang tìm kiếm tọa độ x hợp lệ nhỏ nhất một cách hiệu quả. 

Quan sát quan trọng là đối với mỗi tọa độ x, chúng ta chỉ quan tâm đến y lớn nhất hiện có tại x đó. 

Giả sử chúng ta đang kiểm tra xem tọa độ x nào đó có thể trả lời một truy vấn hay không`(qx, qy)`. 

Nếu y tối đa tại x đó không lớn hơn`qy`, thì không có nghĩa là x có tác dụng. 

Nếu y tối đa lớn hơn`qy`, thì ít nhất một điểm hợp lệ tồn tại ở đó. 

Điều đó biến vấn đề thành: 

- tìm tọa độ x nhỏ nhất lớn hơn`qx`- có y tối đa lớn hơn`qy`Khi x được tìm thấy, chúng ta chỉ cần y nhỏ nhất tại x vượt quá`qy`. 

Đây chính xác là loại điều kiện mà cây phân đoạn xử lý tốt. 

Chúng tôi nén tất cả tọa độ x xuất hiện trong đầu vào. Đối với mỗi chỉ số x được nén, chúng tôi duy trì: 

- một tập hợp các giá trị y được sắp xếp hiện có tại x đó 
- giá trị y tối đa tại x đó 

Cây phân đoạn lưu trữ các giá trị cực đại này. 

Bây giờ một truy vấn trở thành: 

- tìm kiếm vị trí ngoài cùng bên trái trong cây phân đoạn sau`qx`- có y tối đa được lưu trữ vượt quá`qy`Sau khi định vị tọa độ x đó, tìm kiếm nhị phân bên trong tập y của nó để tìm y nhỏ nhất lớn hơn`qy`. 

Mỗi phép toán trở thành logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) trường hợp xấu nhất | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trước tiên hãy đọc tất cả các thao tác và thu thập mọi tọa độ x xuất hiện. 

Chúng ta cần nén tọa độ vì giá trị x có thể lớn bằng`10^9`, nhưng có nhiều nhất`2 * 10^5`tọa độ riêng biệt. 
2. Sắp xếp và sao chép tọa độ x. 

Mỗi tọa độ x ban đầu nhận được một chỉ số nén giữa`0`Và`m - 1`. 
3. Đối với mỗi chỉ mục x được nén, hãy duy trì một vùng chứa các giá trị y đã được sắp xếp hiện đang hoạt động tại x đó. 

Chúng ta cần hai thao tác trên cấu trúc này: 

- chèn/xóa y 
- tìm y nhỏ nhất lớn hơn giá trị truy vấn 
4. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ y tối đa hiện có trong khoảng của nó. 

Nếu tọa độ x không có điểm hoạt động thì giá trị của nó sẽ trở thành`-1`. 
5. Đối với một`add x y`hoạt động: 

- chèn`y`vào cấu trúc đã sắp xếp cho x 
- cập nhật lá cây phân đoạn với y tối đa mới tại x đó 

Cây phân đoạn luôn phản ánh y lớn nhất có sẵn cho mỗi tọa độ x. 
6. Đối với một`remove x y`hoạt động: 

- xóa`y`từ cấu trúc cho x 
- tính toán lại y tối đa tại x đó 
- cập nhật cây phân đoạn 

Nếu không còn điểm nào ở tọa độ x đó thì mức tối đa được lưu trữ sẽ trở thành`-1`. 
7. Đối với một`find x y`hoạt động: 

- xác định chỉ số x được nén đầu tiên lớn hơn`x`- truy vấn cây phân đoạn để tìm chỉ mục ngoài cùng bên trái có y tối đa được lưu trữ vượt quá`y`Việc tìm kiếm hoạt động vì nếu mức tối đa của phân khúc không lớn hơn`y`, thì không có tọa độ x nào trong phân đoạn đó có thể trả lời truy vấn. 
8. Nếu không có tọa độ x như vậy, xuất ra`-1`. 
9. Mặt khác, bên trong vùng chứa y cho tọa độ x đó, tìm kiếm nhị phân cho y nhỏ nhất lớn hơn giá trị truy vấn. 

Điều này đưa ra hành vi ràng buộc cần thiết: 

- nhỏ nhất hợp lệ x đầu tiên 
- y hợp lệ nhỏ nhất trong x đó 

### Tại sao nó hoạt động 

Bất biến của cây phân đoạn là:```
tree[node] = maximum y among all active points in that interval of x-coordinates
```Trong một truy vấn, nếu tối đa phân đoạn là nhiều nhất`qy`, thì mọi điểm trong đoạn đó đều không thỏa mãn điều kiện`py > qy`. Bỏ qua toàn bộ phân đoạn đó luôn an toàn. 

Việc tìm kiếm luôn đi xuống con bên trái trước tiên. Do đó, lá hợp lệ đầu tiên được tìm thấy sẽ tương ứng với tọa độ x nhỏ nhất thỏa mãn truy vấn. 

Khi tọa độ x được cố định, chọn y nhỏ nhất lớn hơn`qy`từ tọa độ x đó khớp chính xác với quy tắc đặt hàng được yêu cầu. 

Vì các thao tác chèn và xóa luôn làm mới y tối đa tương ứng, nên bất biến vẫn đúng sau mỗi lần cập nhật. 

## Giải pháp Python```python
import sys
from bisect import bisect_right, insort
from collections import defaultdict

input = sys.stdin.readline

class SegmentTree:
    def __init__(self, n):
        self.n = n
        self.tree = [-1] * (4 * n)

    def update(self, node, l, r, idx, val):
        if l == r:
            self.tree[node] = val
            return

        mid = (l + r) // 2

        if idx <= mid:
            self.update(node * 2, l, mid, idx, val)
        else:
            self.update(node * 2 + 1, mid + 1, r, idx, val)

        self.tree[node] = max(
            self.tree[node * 2],
            self.tree[node * 2 + 1]
        )

    def query(self, node, l, r, ql, y):
        if r < ql or self.tree[node] <= y:
            return -1

        if l == r:
            return l

        mid = (l + r) // 2

        left = self.query(node * 2, l, mid, ql, y)
        if left != -1:
            return left

        return self.query(node * 2 + 1, mid + 1, r, ql, y)

def solve():
    n = int(input())

    ops = []
    xs = []

    for _ in range(n):
        t, x, y = input().split()
        x = int(x)
        y = int(y)

        ops.append((t, x, y))
        xs.append(x)

    comp = sorted(set(xs))
    xid = {x: i for i, x in enumerate(comp)}

    m = len(comp)

    ys = defaultdict(list)

    seg = SegmentTree(m)

    out = []

    for t, x, y in ops:
        idx = xid[x]

        if t == "add":
            insort(ys[idx], y)
            seg.update(1, 0, m - 1, idx, ys[idx][-1])

        elif t == "remove":
            pos = bisect_right(ys[idx], y) - 1
            ys[idx].pop(pos)

            new_max = ys[idx][-1] if ys[idx] else -1
            seg.update(1, 0, m - 1, idx, new_max)

        else:
            start = bisect_right(comp, x)

            if start >= m:
                out.append("-1")
                continue

            res = seg.query(1, 0, m - 1, start, y)

            if res == -1:
                out.append("-1")
                continue

            pos = bisect_right(ys[res], y)
            ans_y = ys[res][pos]

            out.append(f"{comp[res]} {ans_y}")

    sys.stdout.write("\n".join(out))

solve()
```Giải pháp bắt đầu bằng việc nén tọa độ vì giá trị x rất thưa thớt và cực kỳ lớn. Mỗi tọa độ x xuất hiện ở bất kỳ đâu trong đầu vào sẽ nhận được một chỉ mục thu gọn. 

Đối với mọi chỉ số x được nén,`ys[idx]`lưu trữ tất cả các giá trị y đang hoạt động theo thứ tự được sắp xếp. Thứ tự sắp xếp quan trọng vì các truy vấn cần y nhỏ nhất lớn hơn ngưỡng. của Python`bisect_right`trực tiếp trao vị trí đó. 

Cây phân đoạn chỉ lưu trữ một số trên tọa độ x, y hoạt động tối đa. Giá trị duy nhất đó đủ để xác định liệu một số điểm tại x đó có thể đáp ứng truy vấn hay không. 

Hàm truy vấn đệ quy là trung tâm của giải pháp. Nó bỏ qua các phân đoạn có y tối đa quá nhỏ và luôn khám phá con bên trái trước tiên. Điều đó đảm bảo lá hợp lệ đầu tiên tương ứng với tọa độ x hợp lệ nhỏ nhất. 

Xóa rất dễ bị sai. Sau khi loại bỏ giá trị y, chúng ta phải tính lại y tối đa tại tọa độ x đó. Nếu thùng chứa trống rỗng, chúng tôi lưu trữ`-1`vì vậy các truy vấn trong tương lai sẽ bỏ qua vị trí đó một cách chính xác. 

Truy vấn sử dụng các bất đẳng thức nghiêm ngặt ở mọi nơi. Đó là lý do tại sao`bisect_right`được sử dụng cho cả tìm kiếm x và y. sử dụng`bisect_left`sẽ cho phép tọa độ bằng nhau không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
add 1 1
add 3 4
find 0 0
remove 1 1
find 0 0
add 1 1
find 0 0
```### Dấu vết 

| Bước | Hoạt động | Điểm hoạt động | Phân khúc Maxima | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | thêm 1 1 | {(1,1)} | x=1→1, x=3→-1 | | 
| 2 | thêm 3 4 | {(1,1),(3,4)} | x=1→1, x=3→4 | | 
| 3 | tìm 0 0 | không thay đổi | không thay đổi | 1 1 | 
| 4 | xóa 1 1 | {(3,4)} | x=1→-1, x=3→4 | | 
| 5 | tìm 0 0 | không thay đổi | không thay đổi | 3 4 | 
| 6 | thêm 1 1 | {(1,1),(3,4)} | x=1→1, x=3→4 | | 
| 7 | tìm 0 0 | không thay đổi | không thay đổi | 1 1 | 

Dấu vết cho biết lý do tại sao truy vấn tìm kiếm tọa độ x hợp lệ nhỏ nhất trước tiên. Mặc dù`(3,4)`luôn thỏa mãn điều kiện`(1,1)`được ưu tiên bất cứ khi nào nó tồn tại vì tọa độ x của nó nhỏ hơn. 

### Ví dụ 2 

đầu vào:```
8
add 5 2
add 5 10
add 7 3
find 4 5
find 5 1
remove 5 10
find 4 5
find 6 2
```### Dấu vết 

| Bước | Hoạt động | Điểm hoạt động | Tối đa tại x=5 | Tối đa tại x=7 | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | thêm 5 2 | {(5,2)} | 2 | -1 | | 
| 2 | thêm 5 10 | {(5,2),(5,10)} | 10 | -1 | | 
| 3 | thêm 7 3 | {(5,2),(5,10),(7,3)} | 10 | 3 | | 
| 4 | tìm 4 5 | không thay đổi | 10 | 3 | 5 10 | 
| 5 | tìm 5 1 | không thay đổi | 10 | 3 | 7 3 | 
| 6 | loại bỏ 5 10 | {(5,2),(7,3)} | 2 | 3 | | 
| 7 | tìm 4 5 | không thay đổi | 2 | 3 | -1 | 
| 8 | tìm 6 2 | không thay đổi | 2 | 3 | 7 3 | 

Ví dụ này chứng tỏ tại sao việc lưu trữ y tối đa cho mỗi tọa độ x là đủ. Trước khi xóa, x=5 vẫn tồn tại vì y tối đa của nó là 10. Sau khi xóa`(5,10)`, tối đa trở thành 2, không còn thỏa mãn`y > 5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi bản cập nhật và truy vấn thực hiện các phép toán tìm kiếm nhị phân và cây phân đoạn logarit | 
| Không gian | O(n) | Nén tọa độ, cây phân đoạn và các điểm được lưu trữ đều có tỷ lệ tuyến tính | 

Với nhiều nhất`2 * 10^5`hoạt động,`O(n log n)`dễ dàng phù hợp trong giới hạn. Cây phân đoạn thực hiện xung quanh`log₂(2 * 10^5) ≈ 18`mức đệ quy cho mỗi thao tác, rất dễ quản lý trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_right, insort
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    input = sys.stdin.readline

    class SegmentTree:
        def __init__(self, n):
            self.n = n
            self.tree = [-1] * (4 * n)

        def update(self, node, l, r, idx, val):
            if l == r:
                self.tree[node] = val
                return

            mid = (l + r) // 2

            if idx <= mid:
                self.update(node * 2, l, mid, idx, val)
            else:
                self.update(node * 2 + 1, mid + 1, r, idx, val)

            self.tree[node] = max(
                self.tree[node * 2],
                self.tree[node * 2 + 1]
            )

        def query(self, node, l, r, ql, y):
            if r < ql or self.tree[node] <= y:
                return -1

            if l == r:
                return l

            mid = (l + r) // 2

            left = self.query(node * 2, l, mid, ql, y)
            if left != -1:
                return left

            return self.query(node * 2 + 1, mid + 1, r, ql, y)

    n = int(input())

    ops = []
    xs = []

    for _ in range(n):
        t, x, y = input().split()
        x = int(x)
        y = int(y)

        ops.append((t, x, y))
        xs.append(x)

    comp = sorted(set(xs))
    xid = {x: i for i, x in enumerate(comp)}

    ys = defaultdict(list)

    seg = SegmentTree(len(comp))

    ans = []

    for t, x, y in ops:
        idx = xid[x]

        if t == "add":
            insort(ys[idx], y)
            seg.update(1, 0, len(comp) - 1, idx, ys[idx][-1])

        elif t == "remove":
            pos = bisect_right(ys[idx], y) - 1
            ys[idx].pop(pos)

            val = ys[idx][-1] if ys[idx] else -1
            seg.update(1, 0, len(comp) - 1, idx, val)

        else:
            start = bisect_right(comp, x)

            if start >= len(comp):
                ans.append("-1")
                continue

            res = seg.query(1, 0, len(comp) - 1, start, y)

            if res == -1:
                ans.append("-1")
                continue

            pos = bisect_right(ys[res], y)
            ans.append(f"{comp[res]} {ys[res][pos]}")

    return "\n".join(ans)

# provided sample
assert run(
"""7
add 1 1
add 3 4
find 0 0
remove 1 1
find 0 0
add 1 1
find 0 0
"""
) == "1 1\n3 4\n1 1"

# minimum case
assert run(
"""1
find 0 0
"""
) == "-1"

# strict inequality check
assert run(
"""3
add 5 5
find 5 4
find 4 5
"""
) == "-1\n-1"

# multiple points with same x
assert run(
"""5
add 7 3
add 7 10
find 5 4
find 5 9
find 5 10
"""
) == "7 10\n7 10\n-1"

# deletion updates maximum correctly
assert run(
"""6
add 2 1
add 2 100
remove 2 100
find 1 50
find 1 0
find 0 0
"""
) == "-1\n2 1\n2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn`find`trên tập trống |`-1`| Xử lý cấu trúc trống | 
| Truy vấn có tọa độ bằng nhau |`-1`| Tính đúng đắn của bất đẳng thức | 
| Nhiều giá trị y ở cùng x | Lựa chọn y đúng đắn | Sửa dây buộc bên trong một x | 
| Xóa y lớn nhất | Đã cập nhật cực đại | Tính chính xác của việc bảo trì cây phân đoạn | 

## Vỏ cạnh 

Hãy xem xét lại sự bất đẳng thức nghiêm ngặt:```
3
add 5 5
find 5 4
find 4 5
```Vì`find 5 4`, việc tìm kiếm bắt đầu ngay sau x=5. Không tồn tại x lớn hơn nên đáp án là`-1`. 

Vì`find 4 5`, x=5 là hợp lệ, nhưng y=5 không hoàn toàn lớn hơn 5. Cây phân đoạn lưu trữ tối đa y=5, không lớn hơn ngưỡng truy vấn, do đó truy vấn sẽ từ chối phân đoạn đó một cách chính xác. 

Bây giờ hãy xem xét các giá trị x trùng lặp:```
4
add 7 3
add 7 10
find 5 4
find 5 9
```Cây phân đoạn lưu trữ tối đa y=10 cho x=7. 

Vì`find 5 4`, truy vấn phát hiện ra rằng x=7 khả thi vì 10 > 4. Bên trong danh sách y được sắp xếp`[3, 10]`, tìm kiếm nhị phân trả về 10 là y nhỏ nhất lớn hơn 4. 

cho`find 5 9`, logic tương tự sẽ trả về 10 một lần nữa. 

Tính chính xác của việc xóa là một điểm tinh tế khác:```
5
add 2 1
add 2 100
remove 2 100
find 1 50
find 1 0
```Ban đầu cây phân đoạn lưu trữ tối đa y=100 tại x=2. 

Sau khi xóa`(2,100)`, giá trị tối đa trở thành 1. Bản cập nhật lan truyền lên trên cây. 

Vì`find 1 50`, cây phân đoạn bây giờ bác bỏ x=2 vì y tối đa của nó không còn lớn hơn 50, do đó câu trả lời trở thành`-1`. 

Vì`find 1 0`, điểm`(2,1)`vẫn đáp ứng truy vấn và được trả về chính xác.
