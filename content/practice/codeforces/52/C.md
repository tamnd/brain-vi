---
title: "CF 52C - RMQ tròn"
description: "Chúng ta có một mảng được sắp xếp thành một vòng tròn. Mọi thao tác đều hoạt động trên một phân đoạn giữa hai chỉ mục, nhưng phân đoạn đó có thể quấn quanh phần cuối của mảng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 52
codeforces_index: "C"
codeforces_contest_name: "Codeforces Testing Round 1"
rating: 2200
weight: 52
solve_time_s: 138
verified: true
draft: false
---
[CF 52C - RMQ thông tư](https://codeforces.com/problemset/problem/52/C) 

**Xếp hạng:** 2200 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng được sắp xếp thành một vòng tròn. Mọi thao tác đều hoạt động trên một phân đoạn giữa hai chỉ mục, nhưng phân đoạn đó có thể quấn quanh phần cuối của mảng. 

Có hai hoạt động. 

Thao tác đầu tiên thêm giá trị cho mọi phần tử trong một phạm vi. 

Thao tác thứ hai yêu cầu giá trị tối thiểu trong một phạm vi. 

Nếu điểm cuối bên trái nhỏ hơn hoặc bằng điểm cuối bên phải thì đoạn đó là khoảng liền kề bình thường. Nếu điểm cuối bên trái lớn hơn thì đoạn đó sẽ bao quanh. Ví dụ: trong một mảng có kích thước 5, phạm vi`[3,1]`có nghĩa là chỉ số`3,4,0,1`. 

Đầu vào cung cấp mảng ban đầu, theo sau là tối đa 200000 thao tác. Đối với mọi thao tác truy vấn, chúng ta phải in giá trị tối thiểu trong đoạn tròn được yêu cầu. 

Các ràng buộc ngay lập tức loại trừ mọi hoạt động xử lý theo thời gian tuyến tính trên mỗi thao tác. Với`n = 200000`Và`m = 200000`, một giải pháp bạo lực có thể chạm vào một cách thô bạo`4 * 10^10`các phần tử trong trường hợp xấu nhất, vượt xa những gì phù hợp trong 2 giây. Thậm chí`O(sqrt n)`mỗi thao tác sẽ không thoải mái trong Python ở quy mô này. Chúng tôi cần thứ gì đó gần với thời gian logarit cho cả cập nhật và truy vấn. 

Các hoạt động cũng được trộn lẫn. Các bản cập nhật sửa đổi phạm vi và các truy vấn yêu cầu phạm vi tối thiểu. Sự kết hợp này chính xác là bối cảnh mà việc lan truyền lười biếng trở nên cần thiết. Cấu trúc chỉ hỗ trợ cập nhật điểm hoặc chỉ truy vấn tĩnh là không đủ. 

Bản chất hình tròn tạo ra một số trường hợp khó xử lý. 

Hãy xem xét đầu vào này:```
5
1 2 3 4 5
1
3 1
```Truy vấn không có nghĩa là một phạm vi trống. Nó có nghĩa là chỉ số`3,4,0,1`, vậy câu trả lời là`1`. Việc thực hiện bất cẩn giả định`l <= r`sẽ bị lỗi hoặc trả về kết quả sai. 

Cập nhật tiêu cực cũng có vấn đề:```
3
5 5 5
2
0 2 -10
0 2
```Sau khi cập nhật, mảng trở thành`[-5,-5,-5]`, vậy câu trả lời là`-5`. Bất kỳ việc triển khai nào sử dụng số nguyên không dấu hoặc khởi tạo không chính xác cho các giá trị tối thiểu sẽ không thành công ở đây. 

Một trường hợp tinh vi khác xuất hiện khi phạm vi bao trùm toàn bộ mảng thông qua việc gói:```
4
10 20 30 40
2
2 1 -5
0 3
```bản cập nhật`[2,1]`ảnh hưởng đến mọi chỉ mục vì phân đoạn được bao bọc là`2,3,0,1`. Mảng cuối cùng trở thành`[5,15,25,35]`, vậy giá trị nhỏ nhất là`5`. Việc chia phạm vi vòng tròn không chính xác có thể để lại khoảng trống. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng mọi thao tác theo đúng nghĩa đen. 

Để cập nhật, hãy lặp qua tất cả các chỉ mục trong phân đoạn và thêm giá trị. Đối với một truy vấn, hãy lặp qua tất cả các chỉ mục trong phân đoạn và tính giá trị tối thiểu. 

Điều này hiệu quả vì các thao tác rất đơn giản và khoảng thời gian vòng tròn có thể được xử lý bằng cách gói các chỉ số theo cách thủ công. Vấn đề là hiệu suất. Trong trường hợp xấu nhất, mọi thao tác có thể chạm tới tất cả`n`các phần tử. Với cả hai`n`Và`m`bằng`200000`, chúng tôi nhận được đại khái:```
200000 * 200000 = 4 * 10^10
```hoạt động. Như vậy là quá chậm gấp trăm lần. 

Quan sát chính là cả hai hoạt động đều dựa trên phạm vi. 

Chúng ta không cần phải cập nhật mọi phần tử ngay lập tức. Nếu toàn bộ phân khúc nhận được`+v`, chúng ta có thể lưu trữ thông tin đó một cách lười biếng và chỉ đẩy nó vào sâu hơn khi cần thiết. Đồng thời, nếu mỗi nút lưu trữ giá trị tối thiểu của phân khúc của nó thì các truy vấn tối thiểu trong phạm vi sẽ trở nên tự nhiên. 

Điều này dẫn trực tiếp đến một cây phân đoạn có khả năng lan truyền lười biếng. 

Khoảng thời gian tròn là sự phức tạp thêm duy nhất. Một đoạn được bọc`[l,r]`với`l > r`có thể được chia thành hai khoảng bình thường:```
[l, n-1]
[0, r]
```Khi chúng tôi giảm mọi phạm vi vòng tròn thành tối đa hai phạm vi thông thường, phần còn lại sẽ trở thành quá trình xử lý cây phân đoạn tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên mảng. 

Mỗi nút lưu trữ giá trị tối thiểu bên trong phân khúc của nó. Gốc lưu trữ tối thiểu của toàn bộ mảng. 
2. Thêm một mảng lười dọc theo cây.`lazy[node]`lưu trữ một phần tăng đang chờ xử lý vẫn cần được truyền cho trẻ em. Điều này cho phép chúng tôi trì hoãn cập nhật thay vì chạm ngay vào mọi phần tử. 
3. Để cập nhật phạm vi, hãy xử lý đệ quy cây phân đoạn. 

Nếu nút hiện tại nằm hoàn toàn trong phạm vi cập nhật, hãy thêm số gia tăng trực tiếp vào`tree[node]`và ghi nó vào`lazy[node]`. 

Chúng tôi không đi xuống sâu hơn vì mọi phần tử trong phân đoạn đó đều thay đổi một lượng như nhau. 
4. Nếu một nút chỉ được che phủ một phần, hãy đẩy giá trị lười đang chờ xử lý của nó xuống trước khi tiếp tục. 

Điều này giữ cho các giá trị con luôn chính xác trước khi đệ quy tiếp tục đi sâu hơn. 
5. Sau khi cập nhật nút con, hãy tính lại nút hiện tại là:```
tree[node] = min(left child, right child)
```Điều này bảo toàn tính bất biến mà mỗi nút lưu trữ mức tối thiểu của phân đoạn của nó. 
6. Đối với truy vấn phạm vi tối thiểu, duyệt đệ quy các nút có liên quan. 

Nếu phân đoạn hiện tại nằm hoàn toàn bên trong phạm vi truy vấn, hãy trả về mức tối thiểu được lưu trữ ngay lập tức. 
7. Nếu phân đoạn chỉ trùng lặp một phần, hãy đẩy các giá trị lười biếng trước và kết hợp kết quả từ cả hai phần tử con bằng cách sử dụng`min`. 
8. Xử lý các khoảng tròn bằng cách chia chúng ra. 

Nếu như`l <= r`, xử lý bình thường 

Nếu như`l > r`, quá trình:```
[l, n-1]
[0, r]
```Đối với các truy vấn, hãy lấy tối thiểu hai phần. 

Để cập nhật, hãy áp dụng mức tăng cho cả hai phần. 

### Tại sao nó hoạt động 

Cây phân đoạn luôn duy trì một bất biến:```
tree[node] = minimum value inside that segment,
including all applied and pending updates.
```Truyền bá lười biếng duy trì tính chính xác vì mọi gia tăng bị trì hoãn đều được ghi lại trước khi bất kỳ truy vấn nào trong tương lai hoặc đệ quy sâu hơn cần thông tin đó. Khi một nút được bao phủ hoàn toàn bởi một bản cập nhật, hãy tăng mức tối thiểu được lưu trữ của nó lên`v`hợp lệ vì mọi phần tử trong phân đoạn đó thay đổi đồng đều theo`v`. 

Việc chia các khoảng bao bọc thành hai khoảng thông thường cũng đúng vì đoạn tròn chính xác là sự kết hợp của hai phạm vi rời rạc đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self.lazy = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, node, l, r, arr):
        if l == r:
            self.tree[node] = arr[l]
            return

        mid = (l + r) // 2

        self.build(node * 2, l, mid, arr)
        self.build(node * 2 + 1, mid + 1, r, arr)

        self.tree[node] = min(
            self.tree[node * 2],
            self.tree[node * 2 + 1]
        )

    def push(self, node):
        if self.lazy[node] != 0:
            val = self.lazy[node]

            left = node * 2
            right = node * 2 + 1

            self.tree[left] += val
            self.tree[right] += val

            self.lazy[left] += val
            self.lazy[right] += val

            self.lazy[node] = 0

    def update(self, node, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.tree[node] += val
            self.lazy[node] += val
            return

        self.push(node)

        mid = (l + r) // 2

        if ql <= mid:
            self.update(node * 2, l, mid, ql, qr, val)

        if qr > mid:
            self.update(node * 2 + 1, mid + 1, r, ql, qr, val)

        self.tree[node] = min(
            self.tree[node * 2],
            self.tree[node * 2 + 1]
        )

    def query(self, node, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.tree[node]

        self.push(node)

        mid = (l + r) // 2
        ans = INF

        if ql <= mid:
            ans = min(ans, self.query(node * 2, l, mid, ql, qr))

        if qr > mid:
            ans = min(ans, self.query(node * 2 + 1, mid + 1, r, ql, qr))

        return ans

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    seg = SegmentTree(arr)

    m = int(input())

    out = []

    for _ in range(m):
        parts = list(map(int, input().split()))

        if len(parts) == 2:
            l, r = parts

            if l <= r:
                ans = seg.query(1, 0, n - 1, l, r)
            else:
                ans = min(
                    seg.query(1, 0, n - 1, l, n - 1),
                    seg.query(1, 0, n - 1, 0, r)
                )

            out.append(str(ans))

        else:
            l, r, v = parts

            if l <= r:
                seg.update(1, 0, n - 1, l, r, v)
            else:
                seg.update(1, 0, n - 1, l, n - 1, v)
                seg.update(1, 0, n - 1, 0, r, v)

    sys.stdout.write("\n".join(out))

solve()
```Cây phân đoạn lưu trữ các giá trị tối thiểu cho các phân đoạn. Giai đoạn xây dựng kết hợp đệ quy các mức tối thiểu của con trở lên. 

các`lazy`mảng là tối ưu hóa quan trọng. Khi một bản cập nhật bao trùm hoàn toàn một phân đoạn, chúng tôi sẽ sửa đổi trực tiếp nút đó và hoãn việc truyền bá. Điều này tránh rơi xuống từng lá. 

các`push`chức năng chuyển các bản cập nhật đang chờ xử lý cho trẻ em trước khi đệ quy sâu hơn. Quên bước này là lỗi phổ biến nhất trong quá trình lan truyền lười biếng. Nếu không thì các truy vấn sẽ đọc các giá trị con cũ. 

Hàm truy vấn khởi tạo câu trả lời với giá trị rất lớn vì các truy vấn tối thiểu cần có phần tử trung tính. sử dụng`0`ở đây sẽ phá vỡ các trường hợp trong đó tất cả các giá trị đều dương. 

Khoảng thời gian tròn được xử lý bên ngoài logic cây phân đoạn. Điều này giữ cho việc triển khai cây luôn rõ ràng và chỉ tập trung vào các khoảng thời gian thông thường. 

Các ranh giới đệ quy được bao gồm ở cả hai đầu. Sự lựa chọn đó phải nhất quán ở mọi nơi, đặc biệt là xung quanh:```
mid = (l + r) // 2
```với các phân đoạn con:```
[l, mid]
[mid + 1, r]
```Việc trộn lẫn các quy ước bao gồm và nửa mở là nguyên nhân phổ biến gây ra các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
4
3 0
3 0 -1
0 1
2 1
```Mảng ban đầu:```
[1, 2, 3, 4]
```| Hoạt động | Phạm vi bị ảnh hưởng | Mảng sau khi hoạt động | Kết quả truy vấn | 
| --- | --- | --- | --- | 
|`3 0`| chỉ số`3,0`|`[1,2,3,4]`|`1`| 
|`3 0 -1`| chỉ số`3,0`|`[0,2,3,3]`| - | 
|`0 1`| chỉ số`0,1`|`[0,2,3,3]`|`0`| 
|`2 1`| chỉ số`2,3,0,1`|`[0,2,3,3]`|`0`| 

Truy vấn đầu tiên thể hiện truy vấn được bao bọc. Bản cập nhật cũng bao bọc và sửa đổi chính xác cả hai đầu của mảng. Truy vấn cuối cùng bao gồm toàn bộ mảng thông qua gói. 

### Ví dụ 2 

đầu vào:```
5
10 20 30 40 50
5
1 3
4 1 -15
0 4
2 2 -100
0 4
```Mảng ban đầu:```
[10,20,30,40,50]
```| Hoạt động | Phạm vi bị ảnh hưởng | Mảng sau khi hoạt động | Kết quả truy vấn | 
| --- | --- | --- | --- | 
|`1 3`| chỉ số`1,2,3`|`[10,20,30,40,50]`|`20`| 
|`4 1 -15`| chỉ số`4,0,1`|`[-5,5,30,40,35]`| - | 
|`0 4`| mảng đầy đủ |`[-5,5,30,40,35]`|`-5`| 
|`2 2 -100`| chỉ mục`2`|`[-5,5,-70,40,35]`| - | 
|`0 4`| mảng đầy đủ |`[-5,5,-70,40,35]`|`-70`| 

Dấu vết này hiển thị cả cập nhật được bao bọc và cập nhật điểm. Cây phân đoạn xử lý cả hai bằng cách sử dụng cùng một cơ chế đệ quy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi cập nhật và truy vấn chạm vào logarit nhiều nút cây phân đoạn | 
| Không gian | O(n) | Cây phân đoạn và mảng lười yêu cầu lưu trữ tuyến tính | 

Với`n`Và`m`cả lên đến`200000`, các phép toán logarit đủ nhanh. Cây phân đoạn có khả năng lan truyền lười biếng sẽ phù hợp thoải mái trong giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    input = input_data.readline

    INF = 10**18

    class SegmentTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.tree = [0] * (4 * self.n)
            self.lazy = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def build(self, node, l, r, arr):
            if l == r:
                self.tree[node] = arr[l]
                return

            mid = (l + r) // 2

            self.build(node * 2, l, mid, arr)
            self.build(node * 2 + 1, mid + 1, r, arr)

            self.tree[node] = min(
                self.tree[node * 2],
                self.tree[node * 2 + 1]
            )

        def push(self, node):
            if self.lazy[node]:
                val = self.lazy[node]

                self.tree[node * 2] += val
                self.tree[node * 2 + 1] += val

                self.lazy[node * 2] += val
                self.lazy[node * 2 + 1] += val

                self.lazy[node] = 0

        def update(self, node, l, r, ql, qr, val):
            if ql <= l and r <= qr:
                self.tree[node] += val
                self.lazy[node] += val
                return

            self.push(node)

            mid = (l + r) // 2

            if ql <= mid:
                self.update(node * 2, l, mid, ql, qr, val)

            if qr > mid:
                self.update(node * 2 + 1, mid + 1, r, ql, qr, val)

            self.tree[node] = min(
                self.tree[node * 2],
                self.tree[node * 2 + 1]
            )

        def query(self, node, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.tree[node]

            self.push(node)

            mid = (l + r) // 2
            ans = INF

            if ql <= mid:
                ans = min(ans, self.query(node * 2, l, mid, ql, qr))

            if qr > mid:
                ans = min(ans, self.query(node * 2 + 1, mid + 1, r, ql, qr))

            return ans

    n = int(input())
    arr = list(map(int, input().split()))

    seg = SegmentTree(arr)

    m = int(input())

    out = []

    for _ in range(m):
        parts = list(map(int, input().split()))

        if len(parts) == 2:
            l, r = parts

            if l <= r:
                ans = seg.query(1, 0, n - 1, l, r)
            else:
                ans = min(
                    seg.query(1, 0, n - 1, l, n - 1),
                    seg.query(1, 0, n - 1, 0, r)
                )

            out.append(str(ans))

        else:
            l, r, v = parts

            if l <= r:
                seg.update(1, 0, n - 1, l, r, v)
            else:
                seg.update(1, 0, n - 1, l, n - 1, v)
                seg.update(1, 0, n - 1, 0, r, v)

    return "\n".join(out)

# provided samples
assert run(
"""4
1 2 3 4
4
3 0
3 0 -1
0 1
2 1
"""
) == "1\n0\n0", "sample 1"

# minimum size
assert run(
"""1
5
3
0 0
0 0 -3
0 0
"""
) == "5\n2", "single element"

# all equal values
assert run(
"""5
7 7 7 7 7
4
0 4
1 3 5
0 4
4 1
"""
) == "7\n7\n7", "all equal values"

# wrap-around update covering all elements
assert run(
"""4
10 20 30 40
2
2 1 -5
0 3
"""
) == "5", "full circular coverage"

# negative updates and queries
assert run(
"""3
5 5 5
3
0 2 -10
0 2
1 1
"""
) == "-5\n-5", "negative values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng một phần tử |`5`, sau đó`2`| Xử lý đúng cây nhỏ nhất có thể | 
| Tất cả các giá trị bằng nhau | Luôn luôn`7`| Cập nhật không vô tình ảnh hưởng đến các phân khúc không liên quan | 
| Cập nhật toàn mảng được gói |`5`| Logic chia tròn | 
| Cập nhật tiêu cực |`-5`| Xử lý tối thiểu hợp lý với số âm | 

## Vỏ cạnh 

Truy vấn được bao bọc không được coi là trống. 

đầu vào:```
5
1 2 3 4 5
1
3 1
```Thuật toán phân chia`[3,1]`vào trong:```
[3,4]
[0,1]
```Truy vấn đầu tiên trả về`4`, lần thứ hai trở lại`1`, và đáp án cuối cùng là:```
min(4,1) = 1
```Điều này phù hợp chính xác với cách giải thích vòng tròn. 

Cập nhật tiêu cực phải lan truyền chính xác thông qua các giá trị lười biếng. 

đầu vào:```
3
5 5 5
2
0 2 -10
0 2
```Bản cập nhật bao trùm hoàn toàn phân khúc gốc, do đó mức tối thiểu gốc ngay lập tức trở thành`-5`, Và`lazy[root]`cửa hàng`-10`. 

Truy vấn sau sẽ đọc giá trị tối thiểu đã chính xác từ gốc mà không cần truy cập vào từng lá. Đầu ra là:```
-5
```Bản cập nhật được gói có thể bao gồm toàn bộ mảng. 

đầu vào:```
4
10 20 30 40
2
2 1 -5
0 3
```Bản cập nhật chia thành:```
[2,3]
[0,1]
```Cùng với nhau, chúng bao gồm mọi chỉ mục chính xác một lần. 

Mảng trở thành:```
[5,15,25,35]
```Truy vấn tối thiểu cuối cùng trả về`5`. 

Trường hợp này xác nhận rằng việc phân tách các phạm vi được bao bọc sẽ duy trì tính chính xác ngay cả khi khoảng thời gian được bao bọc kéo dài toàn bộ mảng.
