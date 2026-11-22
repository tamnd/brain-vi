---
title: "CF 280D - Tổng dãy con tối đa k"
description: "Chúng tôi duy trì một mảng thay đổi theo thời gian. Có hai loại hoạt động. Một thao tác cập nhật một vị trí duy nhất. Cái còn lại yêu cầu tổng số tiền tối đa có thể đạt được bằng cách chọn tối đa k phân mảng con rời rạc theo cặp trong một khoảng nhất định [l, r]."
date: "2026-06-05T08:49:07+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "flows", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 280
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 172 (Div. 1)"
rating: 2800
weight: 280
solve_time_s: 147
verified: true
draft: false
---

[CF 280D - Tổng dãy con tối đa k](https://codeforces.com/problemset/problem/280/D) 

**Đánh giá:** 2800 
**Tags:** cấu trúc dữ liệu, luồng, đồ thị, triển khai 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một mảng thay đổi theo thời gian. 

Có hai loại hoạt động. Một thao tác cập nhật một vị trí duy nhất. Người còn lại yêu cầu tổng số tiền tối đa có thể đạt được bằng cách chọn nhiều nhất`k`các mảng con rời rạc theo cặp trong một khoảng nhất định`[l, r]`. 

Cụm từ “nhiều nhất`k`" là rất quan trọng. Chúng tôi không bao giờ bị buộc phải đóng góp âm. Nếu mọi phân mảng có thể có tổng âm, câu trả lời đúng là`0`, thu được bằng cách không chọn phân đoạn nào cả. 

Chiều dài mảng đạt`10^5`, và có tới`10^5`hoạt động. Khoảng thời gian truy vấn có thể bao gồm gần như toàn bộ mảng. Giải pháp quét khoảng thời gian cho mọi truy vấn là không thể. Thậm chí`O(n)`mỗi hoạt động sẽ yêu cầu khoảng`10^10`làm việc trong trường hợp xấu nhất. 

Một quan sát quan trọng thứ hai là`k ≤ 20`. Khoảng cách có thể rất lớn nhưng số đoạn chúng ta được phép chọn lại rất ít. Sự bất đối xứng đó chính là chìa khóa của giải pháp. 

Một số trường hợp đặc biệt làm cho vấn đề trở nên phức tạp hơn so với truy vấn mảng con tối đa tiêu chuẩn. 

Coi như:```
[-5, -2, -7]
k = 3
```Câu trả lời là`0`, không`-2`. Một giải pháp luôn chọn chính xác`k`phân đoạn sẽ thất bại. 

Coi như:```
[9, -8, 9, -1, -1, -1, 9, -8, 9]
k = 2
```Lấy hai khối tích cực tốt nhất riêng lẻ sẽ mang lại`9 + 9 = 18`, nhưng câu trả lời tối ưu là`25`, thu được bằng cách nối qua một số giá trị âm. Lựa chọn địa phương không hoạt động. 

Coi như:```
[10, -100, 10]
k = 1
```Câu trả lời là`10`, không`20`. Khi chỉ cho phép một phân đoạn, hai giá trị dương không thể được lấy độc lập. 

Cập nhật tạo ra một thách thức khác. Việc tính toán trước câu trả lời cho mỗi khoảng là không thể vì một sửa đổi có thể ảnh hưởng đến nhiều khoảng. 

Giải pháp cuối cùng phải hỗ trợ cả tối ưu hóa khoảng thời gian và cập nhật điểm một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là lập trình động trên mỗi khoảng thời gian truy vấn. 

Giả sử một truy vấn hỏi về`[l, r]`. Chúng ta có thể chạy DP cổ điển để chọn nhiều nhất`k`các mảng con rời rạc bên trong khoảng đó. Trong một khoảng thời gian`m`, điều này đòi hỏi khoảng`O(mk)`thời gian. 

DP đúng vì nó theo dõi rõ ràng có bao nhiêu phân đoạn đã được mở và đóng. Thật không may, trong trường hợp xấu nhất`m = 10^5`và có tới`10^4`những truy vấn như vậy. Ngay cả với`k ≤ 20`, điều này trở nên đại khái```
10^4 × 10^5 × 20 = 2 × 10^10
```hoạt động. 

Chúng ta cần một cách để trả lời các truy vấn về khoảng thời gian mà không cần quét khoảng thời gian đó. 

Quan sát quan trọng xuất phát từ sự giảm thiểu nổi tiếng giữa các vấn đề mảng con rời rạc tối đa và luồng chi phí tối thiểu/chi phí tối đa. 

Hãy tưởng tượng mọi phần tử mảng là một cạnh có lợi nhuận bằng giá trị của nó. Việc chọn một mảng con tương ứng với việc gửi một đơn vị luồng qua một khối liên tiếp của các cạnh lợi nhuận đó. Hạn chế bản thân ở mức tối đa`k`mảng con trở thành một hạn chế về dung lượng của`k`. 

Trong một khoảng thời gian cố định, câu trả lời sẽ trở thành chi phí gửi tối đa`k`đơn vị dòng chảy 

Điều này nghe có vẻ tốn kém cho đến khi chúng ta khai thác được hai sự thật. 

Đầu tiên,`k`nhiều nhất là rất nhỏ`20`. 

Thứ hai, đồ thị tương ứng với một khoảng có cấu trúc cực kỳ chặt chẽ. Mỗi đường tăng cường tương ứng chính xác với một mảng con có tổng tối đa trong mạng dư hiện tại. 

Các hoạt động của mạng dư có thể được triển khai bằng cách sử dụng cây phân đoạn hỗ trợ: 

1. Truy vấn tổng mảng con tối đa. 
2. Cập nhật điểm. 
3. Phủ định toàn bộ mảng con đã chọn. 

Sau khi tìm ra mảng con tốt nhất hiện tại, chúng ta tăng thêm một đơn vị luồng. Trong biểu đồ dư, điều này tương đương với việc đảo ngược dấu của mọi phần tử bên trong đoạn đã chọn đó. Sau đó chúng tôi tìm kiếm lại. 

Đây chính xác là cách giải thích đường đi ngắn nhất liên tiếp cổ điển của luồng chi phí tối đa. 

Cây phân đoạn lưu trữ thông tin mảng con tối đa có thể tìm thấy phân đoạn tốt nhất trong`O(log n)`. Việc phủ định toàn bộ phân đoạn được xử lý bằng cách truyền bá lười biếng. Vì nhiều nhất`k ≤ 20`cần tăng cường, mỗi truy vấn chỉ tốn phí`O(k log n)`. 

Cập nhật là các sửa đổi điểm thông thường trong cùng một cây phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP cho mỗi truy vấn | O((r-l+1)k) | O(r-l+1) | Quá chậm | 
| Cây phân đoạn + diễn giải luồng | O(k log n) cho mỗi truy vấn, cập nhật O(log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp được chấp nhận sử dụng biểu diễn cây phân đoạn cổ điển của cấu trúc mảng con tối đa cùng với việc giảm lưu lượng. 

Mỗi nút cây phân đoạn lưu trữ đủ thông tin để trả lời các truy vấn mảng con tối đa và cũng có đủ thông tin để hỗ trợ việc đảo dấu. 

Đối với mỗi phân khúc chúng tôi duy trì: 

- Tổng số tiền. 
- Tổng tiền tố tốt nhất. 
- Tổng hậu tố tốt nhất. 
- Tổng mảng con tốt nhất. 

Chúng tôi cũng duy trì các phiên bản tối thiểu tương ứng vì phủ định hoán đổi cực đại và cực tiểu. 

### 1. Xây dựng cây phân đoạn 

Đối với một lá chứa giá trị`x`, cả hai cấu trúc cực đại và cực tiểu đều được biết ngay lập tức. 

Các nút bên trong được hợp nhất bằng cách sử dụng các công thức mảng con tối đa tiêu chuẩn. 

### 2. Xử lý cập nhật điểm 

Khi nào`a[i]`trở thành`val`, cập nhật lá tương ứng và tính toán lại tất cả tổ tiên. 

Cây phân đoạn vẫn nhất quán sau`O(log n)`công việc. 

### 3. Bắt đầu trả lời truy vấn`(l,r,k)`Giải thích luồng cho biết chúng ta liên tục gửi thêm một đơn vị luồng trong khi đường tăng cường tốt nhất có chi phí dương. 

Ban đầu mạng dư được biểu thị bằng các giá trị mảng hiện tại. 

### 4. Tìm mảng con có tổng lớn nhất bên trong`[l,r]`Sử dụng cây phân đoạn, thu được thông tin mảng con tối đa cho khoảng thời gian. 

Điều này mang lại cả giá trị của phân khúc tốt nhất và điểm cuối của nó. 

### 5. Dừng nếu giá trị không dương 

Vấn đề cho phép chọn ít hơn`k`phân đoạn. 

Khi phân khúc sẵn có tốt nhất có giá trị`≤ 0`, mọi đường tăng thêm còn lại đều không mang lại lợi nhuận nên đáp án không thể tăng. 

### 6. Thêm giá trị phân đoạn vào đáp án 

Điều này tương ứng với việc gửi thêm một đơn vị luồng. 

### 7. Phủ định đoạn đã chọn 

Trong biểu đồ dư, việc sử dụng một đoạn sẽ đưa ra các cạnh ngược với chi phí ngược lại. 

Việc triển khai cây phân đoạn nhận ra hiệu quả tương tự bằng cách nhân mọi phần tử của mảng con đã chọn đó với`-1`. 

Tuyên truyền lười biếng thực hiện thao tác này`O(log n)`. 

### 8. Lặp lại đến`k`lần 

Mỗi lần lặp lại sẽ tìm thấy đường tăng cường tốt nhất tiếp theo trong biểu đồ dư. 

### 9. Khôi phục tất cả các phân đoạn đã sửa đổi 

Truy vấn không được thay đổi vĩnh viễn mảng. 

Mỗi phân đoạn bị phủ định trong quá trình truy vấn sẽ bị phủ định một lần nữa ở cuối, khôi phục trạng thái ban đầu. 

### Tại sao nó hoạt động 

Việc giảm lưu lượng chuyển đổi vấn đề chọn tối đa`k`các mảng con rời rạc thành bài toán luồng chi phí tối đa với việc tăng thêm đơn vị. 

Trong mạng dư, đường tăng chi phí tối đa tương ứng chính xác với mảng con có tổng tối đa hiện tại. Việc tăng thêm một đơn vị dòng sẽ đảo ngược chi phí dọc theo đường dẫn đó, được thể hiện bằng cách phủ nhận phân khúc đã chọn. 

Các lần tăng cường liên tiếp trong thuật toán luồng chi phí tối đa luôn tạo ra luồng tối ưu. Vì mỗi lần tăng thêm được thực hiện chính xác bằng các thao tác trên cây phân đoạn ở trên nên trình tự của các phân đoạn được chọn giống hệt với trình tự được tạo ra bởi thuật toán luồng. 

Việc dừng lại khi lợi nhuận khả dụng tốt nhất trở nên không dương là đúng vì việc tăng thêm không thể làm tăng tổng chi phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10 ** 18

class Node:
    __slots__ = (
        "sum",
        "mx",
        "mxl",
        "mxr",
        "mn",
        "mnl",
        "mnr",
        "mx_pos",
        "mxl_pos",
        "mxr_pos",
        "mn_pos",
        "mnl_pos",
        "mnr_pos",
    )

    def __init__(self):
        self.sum = 0

        self.mx = self.mxl = self.mxr = -INF
        self.mn = self.mnl = self.mnr = INF

        self.mx_pos = (0, 0)
        self.mxl_pos = (0, 0)
        self.mxr_pos = (0, 0)

        self.mn_pos = (0, 0)
        self.mnl_pos = (0, 0)
        self.mnr_pos = (0, 0)

def merge(a, b):
    res = Node()

    res.sum = a.sum + b.sum

    if a.mxl >= a.sum + b.mxl:
        res.mxl = a.mxl
        res.mxl_pos = a.mxl_pos
    else:
        res.mxl = a.sum + b.mxl
        res.mxl_pos = (a.mxl_pos[0], b.mxl_pos[1])

    if b.mxr >= b.sum + a.mxr:
        res.mxr = b.mxr
        res.mxr_pos = b.mxr_pos
    else:
        res.mxr = b.sum + a.mxr
        res.mxr_pos = (a.mxr_pos[0], b.mxr_pos[1])

    res.mx = a.mx
    res.mx_pos = a.mx_pos

    if b.mx > res.mx:
        res.mx = b.mx
        res.mx_pos = b.mx_pos

    cross = a.mxr + b.mxl
    if cross > res.mx:
        res.mx = cross
        res.mx_pos = (a.mxr_pos[0], b.mxl_pos[1])

    if a.mnl <= a.sum + b.mnl:
        res.mnl = a.mnl
        res.mnl_pos = a.mnl_pos
    else:
        res.mnl = a.sum + b.mnl
        res.mnl_pos = (a.mnl_pos[0], b.mnl_pos[1])

    if b.mnr <= b.sum + a.mnr:
        res.mnr = b.mnr
        res.mnr_pos = b.mnr_pos
    else:
        res.mnr = b.sum + a.mnr
        res.mnr_pos = (a.mnr_pos[0], b.mnr_pos[1])

    res.mn = a.mn
    res.mn_pos = a.mn_pos

    if b.mn < res.mn:
        res.mn = b.mn
        res.mn_pos = b.mn_pos

    cross = a.mnr + b.mnl
    if cross < res.mn:
        res.mn = cross
        res.mn_pos = (a.mnr_pos[0], b.mnl_pos[1])

    return res

class SegTree:
    def __init__(self, arr):
        self.n = len(arr) - 1
        self.t = [Node() for _ in range(self.n * 4 + 5)]
        self.lazy = [False] * (self.n * 4 + 5)
        self.build(1, 1, self.n, arr)

    def apply(self, p):
        node = self.t[p]

        node.sum = -node.sum

        node.mx, node.mn = -node.mn, -node.mx
        node.mxl, node.mnl = -node.mnl, -node.mxl
        node.mxr, node.mnr = -node.mnr, -node.mxr

        node.mx_pos, node.mn_pos = node.mn_pos, node.mx_pos
        node.mxl_pos, node.mnl_pos = node.mnl_pos, node.mxl_pos
        node.mxr_pos, node.mnr_pos = node.mnr_pos, node.mxr_pos

        self.lazy[p] ^= True

    def push(self, p):
        if self.lazy[p]:
            self.apply(p << 1)
            self.apply(p << 1 | 1)
            self.lazy[p] = False

    def build(self, p, l, r, arr):
        if l == r:
            x = arr[l]
            node = self.t[p]

            node.sum = x

            node.mx = node.mxl = node.mxr = x
            node.mn = node.mnl = node.mnr = x

            node.mx_pos = node.mxl_pos = node.mxr_pos = (l, l)
            node.mn_pos = node.mnl_pos = node.mnr_pos = (l, l)
            return

        m = (l + r) >> 1
        self.build(p << 1, l, m, arr)
        self.build(p << 1 | 1, m + 1, r, arr)

        self.t[p] = merge(self.t[p << 1], self.t[p << 1 | 1])

    def update_point(self, p, l, r, idx, val):
        if l == r:
            node = self.t[p]

            node.sum = val

            node.mx = node.mxl = node.mxr = val
            node.mn = node.mnl = node.mnr = val

            node.mx_pos = node.mxl_pos = node.mxr_pos = (l, l)
            node.mn_pos = node.mnl_pos = node.mnr_pos = (l, l)
            return

        self.push(p)

        m = (l + r) >> 1
        if idx <= m:
            self.update_point(p << 1, l, m, idx, val)
        else:
            self.update_point(p << 1 | 1, m + 1, r, idx, val)

        self.t[p] = merge(self.t[p << 1], self.t[p << 1 | 1])

    def update_range(self, p, l, r, ql, qr):
        if ql <= l and r <= qr:
            self.apply(p)
            return

        self.push(p)

        m = (l + r) >> 1

        if ql <= m:
            self.update_range(p << 1, l, m, ql, qr)
        if qr > m:
            self.update_range(p << 1 | 1, m + 1, r, ql, qr)

        self.t[p] = merge(self.t[p << 1], self.t[p << 1 | 1])

    def query(self, p, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[p]

        self.push(p)

        m = (l + r) >> 1

        if qr <= m:
            return self.query(p << 1, l, m, ql, qr)
        if ql > m:
            return self.query(p << 1 | 1, m + 1, r, ql, qr)

        left = self.query(p << 1, l, m, ql, qr)
        right = self.query(p << 1 | 1, m + 1, r, ql, qr)

        return merge(left, right)

def solve():
    n = int(input())
    arr = [0] + list(map(int, input().split()))

    seg = SegTree(arr)

    m = int(input())
    ans = []

    for _ in range(m):
        q = list(map(int, input().split()))

        if q[0] == 0:
            _, idx, val = q
            seg.update_point(1, 1, n, idx, val)
        else:
            _, l, r, k = q

            changed = []
            cur = 0

            for _ in range(k):
                node = seg.query(1, 1, n, l, r)

                if node.mx <= 0:
                    break

                cur += node.mx

                x, y = node.mx_pos
                changed.append((x, y))

                seg.update_range(1, 1, n, x, y)

            for x, y in changed:
                seg.update_range(1, 1, n, x, y)

            ans.append(str(cur))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ cả thông tin mảng con tối đa và mảng con tối thiểu. Phiên bản tối thiểu là cần thiết vì việc phủ định một phân đoạn sẽ biến cực đại thành cực tiểu và ngược lại. Nếu không lưu trữ cả hai, việc đảo ngược dấu lười sẽ trở nên không thể. 

các`apply()`chức năng là phần tinh tế nhất. Sự phủ định làm thay đổi dấu của mỗi tổng. Một mảng con tối đa sau khi phủ định tương ứng chính xác với âm của mảng con tối thiểu trước đó. Mã hoán đổi cấu trúc tối đa và tối thiểu trong khi phủ định giá trị của chúng. 

Trong một truy vấn, chúng tôi liên tục trích xuất mảng con tốt nhất, thêm phần đóng góp của nó, phủ định nó và tiếp tục. Mọi khoảng phủ định đều được ghi nhớ. Sau khi kết thúc, tất cả các khoảng đó sẽ bị phủ định một lần nữa để mảng toàn cục trở về trạng thái ban đầu. 

Việc triển khai sử dụng lập chỉ mục dựa trên 1 xuyên suốt vì các điểm cuối khoảng được lưu trữ trực tiếp bên trong các nút cây. Điều này tránh được nhiều lỗi lầm xảy ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 -8 9 -1 -1 -1 9 -8 9
k = 2
```| Lặp lại | Phân khúc hay nhất | Tổng phân đoạn | Chạy câu trả lời | 
| --- | --- | --- | --- | 
| 1 | [1,7] | 16 | 16 | 
| 2 | [9,9] | 9 | 25 | 

Câu trả lời trở thành`25`. 

Ví dụ này cho thấy tại sao việc tham lam nắm giữ những khối tích cực rõ ràng là không đủ. Phân đoạn đầu tiên tối ưu có chủ ý bao gồm một số giá trị âm vì điều đó tạo ra tổng đóng góp lớn hơn. 

### Ví dụ 2 

đầu vào:```
-5 -2 -7
k = 3
```| Lặp lại | Tổng phân khúc tốt nhất | Hành động | 
| --- | --- | --- | 
| 1 | -2 | Dừng lại | 

Câu trả lời là`0`. 

Điều này chứng tỏ “nhiều nhất`k`" điều kiện. Thuật toán dừng ngay khi phân đoạn sẵn có tốt nhất không dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Cập nhật O(log n), truy vấn O(k log n) | Mỗi lần tăng cường yêu cầu một truy vấn khoảng và một phủ định phạm vi | 
| Không gian | O(n) | Cây phân đoạn và mảng lười | 

Kể từ khi`k ≤ 20`, mọi truy vấn chỉ thực hiện một số lượng nhỏ các thao tác trên cây phân đoạn không đổi. Với`n, m ≤ 10^5`, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample
assert run(
"""9
9 -8 9 -1 -1 -1 9 -8 9
3
1 1 9 1
1 1 9 2
1 4 6 3
"""
) == "17\n25\n0", "sample"

# minimum size
assert run(
"""1
5
1
1 1 1 1
"""
) == "5"

# all negative
assert run(
"""3
-1 -2 -3
1
1 1 3 5
"""
) == "0"

# update then query
assert run(
"""3
1 2 3
2
0 2 -10
1 1 3 2
"""
) == "4"

# off-by-one interval
assert run(
"""5
1 2 3 4 5
1
1 5 5 1
"""
) == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố tích cực duy nhất | 5 | Kích thước mảng tối thiểu | 
| Tất cả các giá trị âm | 0 | Chọn phân đoạn bằng 0 | 
| Cập nhật trước truy vấn | 4 | Điểm cập nhật chính xác | 
| Khoảng thời gian`[5,5]`| 5 | Xử lý ranh giới | 
| Mẫu chính thức | 17,25,0 | Chức năng cốt lõi | 

## Vỏ cạnh 

Hãy xem xét:```
3
-5 -2 -7
1
1 1 3 3
```Truy vấn mảng con tối đa trả về`-2`. Vì giá trị này không dương nên vòng lặp tăng thêm sẽ kết thúc ngay lập tức. Không có phân đoạn nào được chọn và đầu ra trở thành`0`. Điều này phù hợp với yêu cầu cho phép chọn phân đoạn bằng 0. 

Coi như:```
3
10 -100 10
1
1 1 3 1
```Mảng con tốt nhất là phần tử thứ nhất hoặc thứ ba, có giá trị`10`. Vì chỉ cho phép một lần tăng thêm nên thuật toán sẽ dừng sau khi chọn một phân đoạn. Nó không bao giờ kết hợp cả hai giá trị dương thông qua khoảng cách âm lớn. 

Coi như:```
5
1 2 3 4 5
2
0 5 -10
1 4 5 1
```Sau khi cập nhật, khoảng thời gian`[4,5]`trở thành`[4,-10]`. Cây phân đoạn tính toán lại tất cả tổ tiên bị ảnh hưởng. Truy vấn tiếp theo trả về`4`, thu được từ phân đoạn một phần tử`[4,4]`. Điều này xác nhận rằng các bản cập nhật được truyền chính xác thông qua cấu trúc mảng con tối đa. 

Coi như:```
5
5 -1 5 -1 5
1
1 1 5 10
```Mặc dù`k=10`, chỉ tồn tại một số lượng hữu hạn các khoản tăng thêm có lợi nhuận. Sau khi hết các đóng góp tích cực, phân đoạn dư tốt nhất trở thành không tích cực và thuật toán dừng sớm. Câu trả lời vẫn là tối ưu vì các phân đoạn bổ sung sẽ không làm tăng tổng số tiền.
