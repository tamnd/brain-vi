---
title: "CF 444C - DZY Yêu Màu Sắc"
description: "Chúng tôi có một dải băng bao gồm các vị trí 1...n. Ban đầu, vị trí i có màu i nên mỗi vị trí đều bắt đầu bằng một màu riêng biệt. Hai loại hoạt động được thực hiện. Thao tác vẽ sẽ gán một màu x mới cho mọi vị trí trong đoạn [l, r]."
date: "2026-06-07T16:00:27+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 444
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 254 (Div. 1)"
rating: 2400
weight: 444
solve_time_s: 319
verified: false
draft: false
---

[CF 444C - DZY yêu thích màu sắc](https://codeforces.com/problemset/problem/444/C) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 5 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dải băng bao gồm các vị trí`1...n`. Ban đầu, vị trí`i`có màu sắc`i`, vì vậy mọi vị trí đều bắt đầu bằng một màu riêng biệt. 

Hai loại hoạt động được thực hiện. 

Thao tác sơn sẽ gán một màu mới`x`đến mọi vị trí trong một phân khúc`[l, r]`. Khi một vị trí hiện đang được tô màu`y`được sơn lại thành`x`, độ màu tích lũy của nó tăng lên bởi`|x - y|`. 

Một thao tác truy vấn yêu cầu tổng số màu sắc trên một phân đoạn`[l, r]`. 

Thách thức là cả chiều dài dải băng và số lượng thao tác đều có thể đạt tới`10^5`. Một mô phỏng đơn giản về việc sơn lại mọi vị trí trong mỗi bản cập nhật có thể yêu cầu tới`10^10`cập nhật vị trí trong trường hợp xấu nhất, điều này hoàn toàn không khả thi trong giới hạn 2 giây. 

Quan sát đầu tiên là màu sắc thay đổi nhiều lần, nhưng sau mỗi lần sơn, toàn bộ khoảng thời gian sẽ trở thành một màu duy nhất. Ruy băng tự nhiên phân hủy thành các đoạn tối đa có cùng màu hiện tại. Nếu chúng ta có thể duy trì trực tiếp các phân đoạn đó thay vì các vị trí riêng lẻ thì số lượng thay đổi trên mỗi thao tác sẽ nhỏ hơn nhiều. 

Một khía cạnh tinh tế khác là màu sắc sặc sỡ được tích lũy vĩnh viễn. Việc sơn lại một vị trí không ghi đè lên những đóng góp trước đó. Ví dụ:```
n = 1

paint [1,1] color 5
paint [1,1] color 2
query [1,1]
```Câu trả lời là```
|5-1| + |2-5| = 4 + 3 = 7
```Việc triển khai bất cẩn chỉ lưu trữ màu hiện tại sẽ làm mất đi sự đóng góp trước đó. 

Một sai lầm dễ dàng thứ hai xuất hiện khi sơn lại cùng một màu.```
n = 3

paint [1,3] color 10
paint [1,3] color 10
query [1,3]
```Bản cập nhật thứ hai đóng góp bằng 0 vì mọi vị trí đều đã có màu`10`. Câu trả lời phải giữ nguyên sau lần cập nhật thứ hai. 

Nguồn lỗi thứ ba là sự trùng lặp một phần với các phân đoạn màu hiện có.```
n = 5

paint [2,4] color 7
```Ban đầu các phân đoạn màu sắc là```
[1]=1 [2]=2 [3]=3 [4]=4 [5]=5
```Bản cập nhật giao nhau với ba màu hiện có. Sự đóng góp thêm vào sự đầy màu sắc là```
|7-2| + |7-3| + |7-4|
```tương ứng ở các vị trí 2, 3 và 4. Bất kỳ cấu trúc phân đoạn nào trước tiên phải được phân chia ở ranh giới của phạm vi cập nhật, nếu không độ dài và đóng góp của phân đoạn sẽ không chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force lưu trữ màu hiện tại và màu sắc tích lũy của mọi vị trí. 

Để cập nhật`[l,r] -> x`, lặp qua mọi vị trí trong phạm vi, thêm`|current_color - x|`với màu sắc sặc sỡ của nó, sau đó thay thế màu của nó bằng`x`. 

Đối với một truy vấn, hãy tính tổng các giá trị độ màu trong khoảng thời gian được yêu cầu. 

Phương pháp này rõ ràng là đúng vì nó tuân theo định nghĩa trực tiếp. Thật không may, một bản cập nhật có thể chạm vào`O(n)`các vị trí. Với`10^5`cập nhật trên một dải băng dài`10^5`, công việc trong trường hợp xấu nhất đạt đến`10^10`hoạt động. 

Quan sát quan trọng là hoạt động sơn ảnh hưởng đến toàn bộ phân đoạn đơn sắc theo cùng một cách. 

Giả sử vị trí`[L,R]`Hiện tại tất cả đều có màu`c`, và chúng tôi sơn lại chúng bằng màu sắc`x`. 

Mọi vị trí đều nhận được số tiền như nhau:```
|x-c|
```Thay vì cập nhật từng vị trí riêng lẻ, chúng ta có thể thêm giá trị này vào cấu trúc dữ liệu khoảng trên`[L,R]`. 

Điều này gợi ý việc tách vấn đề thành hai phần độc lập. 

Phần đầu tiên theo dõi các màu hiện tại. Để làm được điều đó, chúng tôi duy trì một tập hợp các khoảng đơn sắc tối đa có thứ tự. Việc vẽ lại một phạm vi chỉ yêu cầu kiểm tra các khoảng giao nhau với nó. 

Phần thứ hai theo dõi các bản nhạc đầy màu sắc được tích lũy. Mỗi lần một khoảng thời gian dài`k`và màu sắc`c`được sơn lại thành`x`, mọi vị trí trong khoảng đó đều nhận được một mức tăng cộng`|x-c|`. Đây chỉ đơn giản là một hoạt động thêm phạm vi. Các truy vấn yêu cầu tổng phạm vi. Cây phân đoạn với khả năng lan truyền lười sẽ xử lý cả hai thao tác trong`O(log n)`. 

Tập khoảng thời gian cho chúng ta biết vị trí nào nhận được mức tăng nào. Cây phân đoạn lưu trữ các giá trị màu sắc tích lũy. 

Mỗi hoạt động sơn lại chia ranh giới khoảng thời gian tại`l`Và`r+1`, truy cập tất cả các khoảng đơn sắc bị ảnh hưởng, áp dụng phép cộng phạm vi tương ứng cho cây phân đoạn, xóa các khoảng đó và cuối cùng chèn một khoảng mới được tô màu`x`. 

Đối số cây khoảng thời gian được sắp xếp theo thứ tự cổ điển cho thấy rằng mỗi khoảng thời gian chỉ có thể được tạo và hủy trong một số lần giới hạn. Trong tất cả các hoạt động, tổng số lượt truy cập theo khoảng thời gian vẫn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O((m + K) log n) trong đó K là tổng số lần chia tách/lượt truy cập | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một tập hợp các khoảng có thứ tự đại diện cho các màu hiện tại. 

Ban đầu, mỗi vị trí có một màu duy nhất, vì vậy chúng ta tạo các khoảng:```
[1,1] color 1
[2,2] color 2
...
[n,n] color n
```2. Xây dựng cây phân đoạn lười lưu trữ các giá trị màu sắc. 

Mọi giá trị đều bắt đầu từ số 0. 
3. Trước khi xử lý cập nhật, hãy phân chia cấu trúc khoảng thời gian tại vị trí`l`. 

Nếu một số khoảng vượt qua`l`, thay thế nó bằng hai khoảng để một khoảng bắt đầu chính xác tại`l`. 
4. Tách lại tại vị trí`r+1`. 

Sau bước này, mỗi khoảng thời gian hoàn toàn bên trong`[l,r]`trở thành một khoảng rõ ràng trong tập hợp. 
5. Lặp lại tất cả các khoảng có đầy đủ trong`[l,r]`. 

Giả sử một khoảng là`[L,R]`với màu hiện tại`c`. 

Mọi vị trí trong khoảng đó đều tăng:```
|x-c|
```Áp dụng phép cộng phạm vi cây phân đoạn của`|x-c|`TRÊN`[L,R]`. 
6. Xóa tất cả các khoảng có trong`[l,r]`. 

Màu sắc của chúng không còn phù hợp nữa vì thao tác sơn lại sẽ ghi đè lên chúng. 
7. Chèn một khoảng mới`[l,r]`với màu sắc`x`. 

Khoảng này đại diện cho toàn bộ khu vực được sơn lại. 
8. Đối với thao tác truy vấn, chỉ cần hỏi cây phân đoạn về tổng phạm vi`[l,r]`. 

Cây phân đoạn đã chứa mọi mức tăng màu sắc từng được áp dụng. 

### Tại sao nó hoạt động 

Bộ khoảng thời gian luôn thể hiện màu sắc hiện tại chính xác của dải băng dưới dạng phân vùng thành các khoảng đơn sắc rời rạc. 

Sau khi tách tại`l`Và`r+1`, mỗi khoảng bên trong`[l,r]`có một màu hiện tại duy nhất. Sơn lại một khoảng thời gian như vậy từ màu sắc`c`tô màu`x`tăng chính xác mọi vị trí trong khoảng đó`|x-c|`, đây chính xác là bản cập nhật được áp dụng cho cây phân đoạn. 

Cây phân đoạn lưu trữ tổng số đóng góp sơn lại được tích lũy theo thời gian. Vì mỗi đóng góp sơn lại được thêm một lần vào mỗi vị trí bị ảnh hưởng và không bao giờ bị xóa, các truy vấn tổng phạm vi sẽ trả về chính xác tổng số màu sắc được yêu cầu. 

Do đó, cấu trúc khoảng thời gian duy trì màu sắc hiện tại chính xác, trong khi cây phân đoạn duy trì màu sắc tích lũy chính xác và chúng cùng nhau mô phỏng chính xác mọi hoạt động. 

## Giải pháp Python```python
import sys
from bisect import bisect_left
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.sum = [0] * (4 * n)
        self.lazy = [0] * (4 * n)

    def _push(self, p, l, r):
        if self.lazy[p] == 0:
            return

        mid = (l + r) >> 1
        v = self.lazy[p]

        self.sum[p * 2] += v * (mid - l + 1)
        self.sum[p * 2 + 1] += v * (r - mid)

        self.lazy[p * 2] += v
        self.lazy[p * 2 + 1] += v

        self.lazy[p] = 0

    def _add(self, p, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.sum[p] += val * (r - l + 1)
            self.lazy[p] += val
            return

        self._push(p, l, r)

        mid = (l + r) >> 1

        if ql <= mid:
            self._add(p * 2, l, mid, ql, qr, val)
        if qr > mid:
            self._add(p * 2 + 1, mid + 1, r, ql, qr, val)

        self.sum[p] = self.sum[p * 2] + self.sum[p * 2 + 1]

    def add(self, l, r, val):
        self._add(1, 1, self.n, l, r, val)

    def _query(self, p, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.sum[p]

        self._push(p, l, r)

        mid = (l + r) >> 1
        ans = 0

        if ql <= mid:
            ans += self._query(p * 2, l, mid, ql, qr)
        if qr > mid:
            ans += self._query(p * 2 + 1, mid + 1, r, ql, qr)

        return ans

    def query(self, l, r):
        return self._query(1, 1, self.n, l, r)

def solve():
    n, m = map(int, input().split())

    seg = SegTree(n)

    starts = list(range(1, n + 1))
    color = {i: i for i in range(1, n + 1)}
    right = {i: i for i in range(1, n + 1)}

    def split(pos):
        if pos > n:
            return len(starts)

        idx = bisect_left(starts, pos)

        if idx < len(starts) and starts[idx] == pos:
            return idx

        idx -= 1
        s = starts[idx]
        e = right[s]
        c = color[s]

        starts.insert(idx + 1, pos)

        right[s] = pos - 1
        right[pos] = e

        color[pos] = c

        return idx + 1

    out = []

    for _ in range(m):
        op = list(map(int, input().split()))

        if op[0] == 1:
            _, l, r, x = op

            ir = split(r + 1)
            il = split(l)

            affected = starts[il:ir]

            for s in affected:
                e = right[s]
                c = color[s]
                seg.add(s, e, abs(x - c))

            for s in affected:
                del color[s]
                del right[s]

            starts[il:ir] = [l]
            color[l] = x
            right[l] = r

        else:
            _, l, r = op
            out.append(str(seg.query(l, r)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn chỉ chịu trách nhiệm về các giá trị màu sắc. Mỗi lần sơn lại sẽ chuyển thành thao tác thêm phạm vi và mọi truy vấn sẽ trở thành yêu cầu tổng phạm vi. 

Cấu trúc khoảng lưu trữ màu hiện tại. Mỗi khoảng được xác định bởi điểm cuối bên trái của nó. các`starts`danh sách vẫn được sắp xếp, trong khi từ điển lưu trữ điểm cuối và màu bên phải của khoảng. 

các`split(pos)`thường lệ là hoạt động quan trọng. Nếu một khoảng đi qua`pos`, nó được chia thành hai khoảng có cùng màu. Sau khi phân chia ở cả hai ranh giới cập nhật, mọi khoảng thời gian trong phạm vi sơn lại có thể được xử lý độc lập. 

Một chi tiết triển khai tinh tế là thứ tự của hai phần tách. Chúng tôi chia tay lúc`r + 1`trước khi chia tay tại`l`. Điều này phản ánh việc triển khai ODT tiêu chuẩn và tránh làm mất hiệu lực phạm vi vòng lặp mà chúng tôi xử lý sau này. 

Một chi tiết khác là câu trả lời có thể vượt quá giới hạn 32 bit. Mọi giá trị của cây phân đoạn được lưu trữ dưới dạng số nguyên Python, số nguyên này tự động xử lý các giá trị lớn tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 1 2 4
1 2 3 5
2 1 3
```Trạng thái sau mỗi thao tác: 

| Hoạt động | Khoảng thời gian sau khi hoạt động | Đã thêm đóng góp | 
| --- | --- | --- | 
| Ban đầu | [1,1]:1 [2,2]:2 [3,3]:3 | 0 | 
| Sơn [1,2] -> 4 | [1,2]:4 [3,3]:3 | 3 trên pos1, 2 trên pos2 | 
| Sơn [2,3] -> 5 | [1,1]:4 [2,3]:5 | 1 trên pos2, 2 trên pos3 | 
| Truy vấn [1,3] | không thay đổi | đáp án = 8 | 

Giá trị màu sắc trở thành: 

| Vị trí | Giá trị | 
| --- | --- | 
| 1 | 3 | 
| 2 | 3 | 
| 3 | 2 | 

Tổng:```
3 + 3 + 2 = 8
```Ví dụ này cho thấy cây phân đoạn tích lũy các đóng góp từ nhiều hoạt động sơn lại như thế nào. 

### Ví dụ 2 

đầu vào:```
5 3
1 2 4 7
2 1 5
2 2 4
```Sau khi sơn lại: 

| Vị trí | Màu cũ | Màu mới | Tăng | 
| --- | --- | --- | --- | 
| 2 | 2 | 7 | 5 | 
| 3 | 3 | 7 | 4 | 
| 4 | 4 | 7 | 3 | 

Mảng đầy màu sắc:```
[0, 5, 4, 3, 0]
```Truy vấn: 

| Truy vấn | Kết quả | 
| --- | --- | 
| [1,5] | 12 | 
| [2,4] | 12 | 

Dấu vết này chứng tỏ rằng việc sơn lại một phạm vi góp phần độc lập vào từng khoảng đơn sắc được bao phủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((m + K) log n) | Mỗi thao tác trên cây phân đoạn có chi phí O(log n) và mỗi khoảng được tạo bằng cách phân tách chỉ được xử lý khi được truy cập bởi các bản cập nhật | 
| Không gian | O(n) | Cây phân đoạn cộng với biểu diễn khoảng | 

Cấu trúc khoảng không bao giờ chứa nhiều hơn`O(n + m)`khoảng thời gian trong toàn bộ quá trình thực thi và cây phân đoạn sử dụng bộ nhớ tuyến tính. Với`n, m ≤ 10^5`, điều này phù hợp một cách thoải mái trong giới hạn được sử dụng bởi các giải pháp được chấp nhận cho vấn đề này. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from bisect import bisect_left

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    input = sys.stdin.readline

    class SegTree:
        def __init__(self, n):
            self.n = n
            self.sum = [0] * (4 * n)
            self.lazy = [0] * (4 * n)

        def push(self, p, l, r):
            if not self.lazy[p]:
                return
            mid = (l + r) // 2
            v = self.lazy[p]
            self.sum[p * 2] += v * (mid - l + 1)
            self.sum[p * 2 + 1] += v * (r - mid)
            self.lazy[p * 2] += v
            self.lazy[p * 2 + 1] += v
            self.lazy[p] = 0

        def add(self, p, l, r, ql, qr, v):
            if ql <= l and r <= qr:
                self.sum[p] += v * (r - l + 1)
                self.lazy[p] += v
                return
            self.push(p, l, r)
            mid = (l + r) // 2
            if ql <= mid:
                self.add(p * 2, l, mid, ql, qr, v)
            if qr > mid:
                self.add(p * 2 + 1, mid + 1, r, ql, qr, v)
            self.sum[p] = self.sum[p * 2] + self.sum[p * 2 + 1]

        def query(self, p, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.sum[p]
            self.push(p, l, r)
            mid = (l + r) // 2
            ans = 0
            if ql <= mid:
                ans += self.query(p * 2, l, mid, ql, qr)
            if qr > mid:
                ans += self.query(p * 2 + 1, mid + 1, r, ql, qr)
            return ans

    n, m = map(int, input().split())
    seg = SegTree(n)

    starts = list(range(1, n + 1))
    color = {i: i for i in range(1, n + 1)}
    right = {i: i for i in range(1, n + 1)}

    def split(pos):
        if pos > n:
            return len(starts)
        idx = bisect_left(starts, pos)
        if idx < len(starts) and starts[idx] == pos:
            return idx
        idx -= 1
        s = starts[idx]
        starts.insert(idx + 1, pos)
        right[pos] = right[s]
        color[pos] = color[s]
        right[s] = pos - 1
        return idx + 1

    ans = []

    for _ in range(m):
        q = list(map(int, input().split()))

        if q[0] == 1:
            _, l, r, x = q
            ir = split(r + 1)
            il = split(l)

            cur = starts[il:ir]

            for s in cur:
                seg.add(1, 1, n, s, right[s], abs(x - color[s]))

            for s in cur:
                del color[s]
                del right[s]

            starts[il:ir] = [l]
            color[l] = x
            right[l] = r

        else:
            _, l, r = q
            ans.append(str(seg.query(1, 1, n, l, r)))

    return "\n".join(ans)

# sample
assert run(
"""3 3
1 1 2 4
1 2 3 5
2 1 3
"""
) == "8"

# minimum size
assert run(
"""1 1
2 1 1
"""
) == "0"

# repaint same color
assert run(
"""1 2
1 1 1 1
2 1 1
"""
) == "0"

# full range repaint
assert run(
"""3 2
1 1 3 10
2 1 3
"""
) == "24"

# boundary split test
assert run(
"""5 3
1 2 4 7
2 1 5
2 2 4
"""
) == "12\n12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn phần tử đơn | 0 | Xử lý trạng thái ban đầu | 
| Sơn lại cùng màu | 0 | Không cập nhật đóng góp | 
| Sơn lại toàn diện | 24 | Cập nhật toàn bộ mảng | 
| Sơn lại một phần nội thất | 12, 12 | Chia khoảng đúng | 
| Mẫu chính thức | 8 | Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy cân nhắc việc sơn lại cùng màu.```
1 2
1 1 1 1
2 1 1
```Vị trí 1 ban đầu có màu 1. Sơn lại bằng màu 1 thêm```
|1-1| = 0
```Thuật toán truy cập khoảng, tính toán`abs(x - c)`, nhận được 0 và áp dụng phép cộng phạm vi bằng 0. Truy vấn trả về chính xác`0`. 

Xem xét việc sơn lại nhiều lần ở cùng một vị trí.```
1 3
1 1 1 5
1 1 1 2
2 1 1
```Việc sơn lại lần đầu góp phần`4`, đóng góp thứ hai`3`. Cây phân đoạn lưu trữ cả hai bản cập nhật, vì vậy câu trả lời cuối cùng là`7`. Vì các đóng góp chỉ được thêm vào và không bao giờ bị ghi đè nên màu sắc tích lũy vẫn chính xác. 

Hãy xem xét việc sơn lại bắt đầu và kết thúc trong khoảng thời gian hiện có.```
5 2
1 2 4 7
2 1 5
```Trước khi xử lý cập nhật, thuật toán phân chia tại các vị trí`2`Và`5`. Vùng bị ảnh hưởng trở thành tập hợp các khoảng đơn sắc hoàn chỉnh. Mỗi người nhận được chính xác một phạm vi bổ sung bằng với sự khác biệt về màu sắc của nó so với`7`. Không có vị trí bên ngoài`[2,4]`được chạm vào và truy vấn trả về chính xác`12`. 

Những trường hợp này chính xác là nơi việc chia khoảng và bổ sung phạm vi tích lũy trở nên quan trọng. Thuật toán xử lý chúng một cách tự nhiên thông qua tính bất biến của nó là tập khoảng luôn khớp với màu hiện tại và cây phân đoạn luôn lưu trữ mọi đóng góp sơn lại từng được áp dụng.
