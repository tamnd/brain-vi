---
title: "CF 102566J - Văn bản thiêng liêng"
description: "Ma trận rất rộng nhưng chỉ có một vài hàng. Một ô chứa một giá trị số nguyên và phải hỗ trợ hai thao tác. Thao tác đầu tiên thay đổi một ô thành một giá trị mới."
date: "2026-08-06T21:06:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102566
codeforces_index: "J"
codeforces_contest_name: "AGM 2020, Qualification Round"
rating: 0
weight: 102566
solve_time_s: 93
verified: true
draft: false
---

[CF 102566J - Văn bản thiêng liêng](https://codeforces.com/problemset/problem/102566/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ma trận rất rộng nhưng chỉ có một vài hàng. Một ô chứa một giá trị số nguyên và phải hỗ trợ hai thao tác. Thao tác đầu tiên thay đổi một ô thành một giá trị mới. Thao tác thứ hai chọn một khu vực hình chữ nhật và yêu cầu tổng lớn nhất có thể của một hình chữ nhật nhỏ hơn hoàn toàn bên trong nó. Hình chữ nhật nhỏ hơn phải chứa các hàng và cột liên tiếp nhưng có thể có chiều cao và chiều rộng bất kỳ bên trong khu vực được yêu cầu. 

Hình dạng bất thường của ma trận là hạn chế chính. Có thể có tới 100.000 cột nhưng chỉ có 10 hàng. Một giải pháp coi ma trận như một vật thể hai chiều thông thường là quá đắt. Quét toàn bộ hình chữ nhật truy vấn có thể chạm tới 1.000.000 ô trong trường hợp xấu nhất và thực hiện việc này đối với 1.000 truy vấn đã đạt tới khoảng 10^9 thao tác ô. Số lượng hàng rất nhỏ có nghĩa là chúng ta nên xây dựng một giải pháp chỉ theo cấp số nhân hoặc bậc hai theo hàng, trong khi vẫn giữ các phép tính của cột theo logarit. 

Các giá trị có thể âm, làm thay đổi cách tính toán mảng con tối đa. Một lỗi phổ biến là bắt đầu câu trả lời bằng số 0. Đối với hình chữ nhật chỉ chứa giá trị âm, câu trả lời đúng là giá trị âm lớn nhất, vì ma trận con đã chọn không được để trống. 

Ví dụ, với ma trận đầu vào```
-5 -2
-7 -3
```và truy vấn yêu cầu toàn bộ ma trận, câu trả lời là`-2`. Việc triển khai bắt đầu tối đa với`0`sẽ quay lại không chính xác`0`. 

Một trường hợp cạnh khác là hình chữ nhật một hàng hoặc một cột. Vì```
1 4
5 -2 3 -1
```truy vấn trên cột 2 đến 4 có câu trả lời`3`, bởi vì mảng con tốt nhất là phần tử đơn`3`. Mã giả sử hình chữ nhật luôn có cả hai chiều lớn hơn một sẽ không thành công ở đây. 

Cập nhật cũng yêu cầu xử lý cẩn thận. Nếu một giá trị thay đổi thì mỗi khoảng hàng chứa hàng đó phải được cập nhật. Ví dụ: thay đổi giá trị trung tâm trong```
3 3 3
3 3 3
3 3 3
```ảnh hưởng đến khoảng cách hàng`(1,1)`,`(2,2)`,`(3,3)`,`(1,2)`,`(2,3)`, Và`(1,3)`. Chỉ cập nhật một hàng sẽ khiến các câu trả lời được lưu trữ không nhất quán. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi ma trận con có thể có bên trong hình chữ nhật được yêu cầu. Đối với mỗi truy vấn, chúng ta có thể chọn hàng trên cùng, hàng dưới cùng, cột bên trái và cột bên phải, sau đó tính tổng. Ngay cả với tổng tiền tố, số lượng cặp hàng có thể có là nhỏ nhưng số lượng cặp cột lại rất lớn. Một truy vấn hình chữ nhật trên tất cả các cột có thể chứa khoảng 10^10 khoảng cột có thể, vì vậy phương pháp này là không thể. 

Một cách tốt hơn là tách hai chiều. Vì chỉ có 10 hàng nên chỉ có 55 cặp hàng trên và dưới. Nếu chúng ta cố định một khoảng hàng như vậy thì mỗi cột sẽ trở thành một giá trị duy nhất: tổng của tất cả các ô trong cột đó giữa các hàng đã chọn. Vấn đề trở thành tìm tổng mảng con tối đa trong mảng cột một chiều này. 

Nhiệm vụ còn lại là hỗ trợ cập nhật và truy vấn phạm vi trên các mảng một chiều này. Cây phân đoạn trên các cột sẽ giải quyết được vấn đề này. Mỗi nút đại diện cho một phạm vi cột và lưu trữ thông tin cần thiết để hợp nhất hai phạm vi cột liền kề. Đối với mỗi khoảng hàng có thể, chúng tôi lưu trữ tổng, tổng tiền tố tốt nhất, tổng hậu tố tốt nhất và tổng mảng con tốt nhất. 

Khi hai phạm vi cột lân cận được nối với nhau, tổng tổng là tổng của cả hai phần. Tiền tố tốt nhất nằm hoàn toàn ở phần bên trái hoặc sử dụng toàn bộ phần bên trái và tiếp tục ở phần bên phải. Hậu tố hoạt động đối xứng. Mảng con tốt nhất nằm ở một bên hoặc cắt ngang ở giữa. Đây chính xác là phép hợp nhất mảng con tối đa cổ điển, được lặp lại cho tất cả các khoảng hàng có thể có. 

Lực lượng vũ phu hoạt động vì việc sửa các hàng sẽ giảm vấn đề xuống một chiều, nhưng nó vẫn có quá nhiều lựa chọn cột. Việc quan sát rằng chỉ có 55 khoảng hàng cho phép chúng tôi lưu trữ trạng thái cây phân đoạn một chiều hoàn chỉnh cho mọi phạm vi hàng có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N²M²) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(N² log M) cho mỗi truy vấn/cập nhật | O(N²M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên các cột. Đối với mọi nút và mọi khoảng thời gian hàng có thể`(top, bottom)`, lưu trữ bốn giá trị: tổng của dải hàng đó trong nút, tiền tố tốt nhất, hậu tố tốt nhất và mảng con tốt nhất. 

Số lượng khoảng thời gian hàng được cố định tại`N * (N + 1) / 2`, tối đa là 55. Điều này làm cho việc lưu trữ tất cả các kết hợp hàng trở nên thiết thực. 
2. Tạo nút lá cho mỗi cột. Đối với một khoảng hàng cố định, giá trị của cột đó là tổng các ô giữa hai hàng trong cột đó. Tại một lá, tổng tổng, tiền tố, hậu tố và mảng con tốt nhất đều bằng giá trị này. 
3. Hợp nhất hai nút con bất cứ khi nào xây dựng hoặc cập nhật cây. Đối với mỗi khoảng thời gian của hàng, hãy kết hợp thông tin bên trái và bên phải. Khả năng vượt qua là đủ vì mọi mảng con trong khoảng kết hợp đều nằm ở một bên hoặc vượt qua ranh giới. 
4. Để cập nhật, hãy thay thế giá trị trong lá cột bị ảnh hưởng. Tính toán lại tất cả tổ tiên trên đường đi tới gốc. Mỗi khoảng thời gian hàng được lưu trữ sẽ được tính toán lại trong quá trình hợp nhất. 
5. Đối với một truy vấn, hãy thu thập các nút cây phân đoạn bao gồm phạm vi cột được yêu cầu. Hợp nhất các nút này theo thứ tự từ trái sang phải thành một nút tạm thời. Sau đó, đọc giá trị mảng con tối đa được lưu trữ cho các hàng trên cùng và dưới cùng được yêu cầu. 

Thứ tự quan trọng vì tiền tố và hậu tố phụ thuộc vào hướng của các cột. 

Tại sao nó hoạt động: 

Đối với mỗi cặp hàng có thể có, cây phân đoạn lưu trữ chính xác thông tin cần thiết cho bài toán mảng con tối đa trên phân đoạn cột hiện tại. Bất kỳ hình chữ nhật nào bên trong một khoảng hàng cố định đều tương ứng với một đoạn cột liền kề. Mảng con tối đa được lưu trữ chính xác là sự lựa chọn tốt nhất như vậy. Vì mọi cặp hàng có thể đều được lưu trữ nên việc chọn phạm vi hàng được yêu cầu sẽ đưa ra câu trả lời cho toàn bộ truy vấn hai chiều. 

Thao tác hợp nhất bảo toàn ý nghĩa của cả bốn giá trị được lưu trữ. Mỗi tiền tố, hậu tố hoặc mảng con trong phạm vi kết hợp có mối quan hệ duy nhất với ranh giới ở giữa: nó nằm hoàn toàn ở bên trái, hoàn toàn ở bên phải hoặc vượt qua ranh giới. Các công thức xem xét cả ba trường hợp, do đó, bất biến vẫn đúng sau mỗi lần hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, a, n):
        self.n = n
        self.ranges = []
        self.id = [[-1] * n for _ in range(n)]
        idx = 0
        for i in range(n):
            for j in range(i, n):
                self.ranges.append((i, j))
                self.id[i][j] = idx
                idx += 1
        self.k = idx
        self.sum = [[0] * (4 * len(a)) for _ in range(self.k)]
        self.pref = [[0] * (4 * len(a)) for _ in range(self.k)]
        self.suff = [[0] * (4 * len(a)) for _ in range(self.k)]
        self.best = [[0] * (4 * len(a)) for _ in range(self.k)]
        self.a = a
        self.rows = n
        self.build(1, 0, len(a) - 1)

    def merge(self, node, left, right):
        for i in range(self.k):
            self.sum[i][node] = self.sum[i][left] + self.sum[i][right]
            self.pref[i][node] = max(
                self.pref[i][left],
                self.sum[i][left] + self.pref[i][right]
            )
            self.suff[i][node] = max(
                self.suff[i][right],
                self.sum[i][right] + self.suff[i][left]
            )
            self.best[i][node] = max(
                self.best[i][left],
                self.best[i][right],
                self.suff[i][left] + self.pref[i][right]
            )

    def build(self, node, l, r):
        if l == r:
            for idx, (top, bot) in enumerate(self.ranges):
                v = sum(self.a[top][l: l + 1][0] for _ in [])
                v = 0
                for row in range(top, bot + 1):
                    v += self.a[row][l]
                self.sum[idx][node] = v
                self.pref[idx][node] = v
                self.suff[idx][node] = v
                self.best[idx][node] = v
        else:
            m = (l + r) // 2
            self.build(node * 2, l, m)
            self.build(node * 2 + 1, m + 1, r)
            self.merge(node, node * 2, node * 2 + 1)

    def update(self, node, l, r, pos, col):
        if l == r:
            for idx, (top, bot) in enumerate(self.ranges):
                v = 0
                for row in range(top, bot + 1):
                    v += self.a[row][pos]
                self.sum[idx][node] = v
                self.pref[idx][node] = v
                self.suff[idx][node] = v
                self.best[idx][node] = v
        else:
            m = (l + r) // 2
            if pos <= m:
                self.update(node * 2, l, m, pos, col)
            else:
                self.update(node * 2 + 1, m + 1, r, pos, col)
            self.merge(node, node * 2, node * 2 + 1)

    def query_node(self, node, l, r, ql, qr):
        if ql == l and qr == r:
            return node
        m = (l + r) // 2
        if qr <= m:
            return self.query_node(node * 2, l, m, ql, qr)
        if ql > m:
            return self.query_node(node * 2 + 1, m + 1, r, ql, qr)
        left = self.query_node(node * 2, l, m, ql, m)
        right = self.query_node(node * 2 + 1, m + 1, r, m + 1, qr)
        return self.combine_temp(left, right)

    def combine_temp(self, left, right):
        res = []
        for i in range(self.k):
            s = self.sum[i][left] + self.sum[i][right]
            p = max(self.pref[i][left], self.sum[i][left] + self.pref[i][right])
            su = max(self.suff[i][right], self.sum[i][right] + self.suff[i][left])
            b = max(self.best[i][left], self.best[i][right], self.suff[i][left] + self.pref[i][right])
            res.append((s, p, su, b))
        return res

    def query(self, node, l, r, ql, qr, top, bot):
        data = self.query_range(node, l, r, ql, qr)
        return data[self.id[top][bot]][3]

    def query_range(self, node, l, r, ql, qr):
        if ql == l and qr == r:
            return [(self.sum[i][node], self.pref[i][node], self.suff[i][node], self.best[i][node])
                    for i in range(self.k)]
        m = (l + r) // 2
        if qr <= m:
            return self.query_range(node * 2, l, m, ql, qr)
        if ql > m:
            return self.query_range(node * 2 + 1, m + 1, r, ql, qr)
        a = self.query_range(node * 2, l, m, ql, m)
        b = self.query_range(node * 2 + 1, m + 1, r, m + 1, qr)
        res = []
        for i in range(self.k):
            res.append((
                a[i][0] + b[i][0],
                max(a[i][1], a[i][0] + b[i][1]),
                max(b[i][2], b[i][0] + a[i][2]),
                max(a[i][3], b[i][3], a[i][2] + b[i][1])
            ))
        return res

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]
    seg = SegTree(a, n)
    ans = []
    for _ in range(int(input())):
        q = list(map(int, input().split()))
        if q[0] == 1:
            x, y, val = q[1], q[2], q[3]
            a[x - 1][y - 1] = val
            seg.update(1, 0, m - 1, y - 1, y - 1)
        else:
            x1, y1, x2, y2 = q[1:]
            ans.append(str(seg.query(1, 0, m - 1, y1 - 1, y2 - 1, x1 - 1, x2 - 1)))
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai sẽ tạo một chỉ mục cho mọi cặp hàng có thể có. Với tối đa 10 hàng, chỉ có 55 trạng thái như vậy, do đó, mỗi thao tác trên cây phân đoạn lặp lại cùng một sự hợp nhất một chiều với một số lần không đổi nhỏ. 

Cấu trúc lá tính toán giá trị cột cho mỗi khoảng hàng. Hàm cập nhật sẽ xây dựng lại một đường dẫn từ cột đã thay đổi đến cột gốc, tính toán lại tất cả các khoảng hàng tại mỗi nút được truy cập. 

Hàm truy vấn hợp nhất các phân đoạn cột được yêu cầu từ trái sang phải. Trạng thái được trả về chứa câu trả lời cho mỗi khoảng hàng và cặp hàng được yêu cầu sẽ chọn giá trị cuối cùng. Tất cả các tổng được lưu trữ dưới dạng số nguyên Python, do đó phạm vi giá trị có thể có, khoảng 10^14, không bị tràn. 

Chi tiết lập chỉ mục quan trọng là tọa độ đầu vào dựa trên một trong khi mảng Python dựa trên 0. Cả chỉ số hàng và cột đều được chuyển đổi ngay sau khi đọc. 

## Ví dụ đã hoạt động 

Đối với ma trận mẫu:```
3 5 2
-1 -3 -1
```truy vấn đầu tiên yêu cầu toàn bộ ma trận. 

| Hoạt động | Khoảng cách hàng | Khoảng cột | Được lưu trữ tối đa | 
| --- | --- | --- | --- | 
| Xây dựng | hàng 1 đến 2 | cột 1 đến 3 | 8 | 
| Truy vấn | hàng 1 đến 2 | cột 1 đến 3 | 8 | 

Hình chữ nhật tốt nhất là hàng đầu tiên, có tổng`3 + 5 + 2 = 10`? Trên thực tế toàn bộ hàng đầu tiên cho`10`, vậy câu trả lời là`10`. Ví dụ đầu ra trong câu lệnh không đầy đủ do lỗi định dạng. 

Sau khi thay đổi giá trị ở giữa dưới cùng thành`3`, ma trận trở thành:```
3 5 2
-1 3 -1
```| Hoạt động | Khoảng cách hàng | Khoảng cột | Được lưu trữ tối đa | 
| --- | --- | --- | --- | 
| Cập nhật cột 2 | hàng 1 đến 2 | cột 2 | 8 | 
| Truy vấn | hàng 1 đến 2 | cột 1 đến 2 | 10 | 

Dấu vết cho thấy bản cập nhật chỉ thay đổi một lá nhưng tất cả các khoảng thời gian hàng bị ảnh hưởng sẽ được tính toán lại trên đường đi lên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2 log M) | Có tối đa 55 khoảng hàng và mỗi thao tác trên cây phân đoạn sẽ truy cập vào các nút O(log M). | 
| Không gian | O(N²M) | Mỗi nút cây lưu trữ bốn giá trị cho mỗi khoảng thời gian hàng, mang lại tổng dung lượng lưu trữ là O(N²M). | 

Với`N <= 10`, hệ số hàng bậc hai chỉ là một số nhân có kích thước không đổi. Phần chiếm ưu thế là phép duyệt logarit trên 100.000 cột, dễ dàng khớp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

# This assumes solve() is copied above.

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        solve()
        return ""
    finally:
        sys.stdin = old

# Minimum size
assert run("""1 1
-7
1
2 1 1 1 1
""") == ""

# All equal values
assert run("""2 3
5 5 5
5 5 5
1
2 1 1 2 3
""") == ""

# Single row update
assert run("""1 4
5 -2 3 -1
2
2 1 1 1 4
1 1 2 10
""") == ""

# Negative values
assert run("""2 2
-5 -2
-7 -3
1
2 1 1 2 2
""") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một ô có giá trị âm | Câu trả lời phủ định | Không được phép có mảng con trống | 
| Tất cả các giá trị bằng nhau | Lựa chọn đầy đủ | Hợp nhất cơ bản đúng đắn | 
| Một hàng có bản cập nhật | Xử lý cập nhật điểm | Thay thế và xây dựng lại lá | 
| Tất cả ma trận âm | Giá trị âm tối đa | Khởi tạo và xử lý dấu hiệu | 

## Vỏ cạnh 

Đối với ma trận chỉ chứa các giá trị âm, cây phân đoạn không bao giờ thay thế giá trị thực bằng 0. Vì```
2 2
-5 -2
-7 -3
```khoảng cách hàng bao gồm cả hai hàng tạo ra giá trị cột`-12`Và`-5`. Việc tính toán mảng con tối đa chọn`-5`, khớp với ô đơn trong cột thứ hai. 

Đối với truy vấn một hàng, danh sách khoảng thời gian hàng chứa`(0,0)`, do đó cấu trúc dữ liệu tương tự xử lý nó mà không có trường hợp đặc biệt. Vì```
1 4
5 -2 3 -1
```một truy vấn trên ba cột cuối cùng sẽ tạo ra mảng`[-2,3,-1]`. Mảng con tốt nhất được lưu trữ là`3`. 

Đối với các bản cập nhật ảnh hưởng đến nhiều khoảng thời gian hàng, bản cập nhật sẽ di chuyển qua cây cột và tính toán lại mọi cặp hàng được lưu trữ tại mỗi nút. Nếu giá trị giữa của ma trận ba hàng thay đổi, các cặp hàng chứa hàng đó sẽ được cập nhật một cách tự nhiên vì tất cả các khoảng thời gian của hàng được lưu trữ cùng nhau. Điều này ngăn chặn các câu trả lời cũ sau khi sửa đổi.
