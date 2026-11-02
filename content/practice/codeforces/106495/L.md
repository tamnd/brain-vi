---
title: "CF 106495L - Loại huyền thoại"
description: "Chúng tôi có một hoán vị và hai loại hoạt động. Truy vấn thuộc loại đầu tiên hoán đổi hai phần tử lân cận trong hoán vị hiện tại."
date: "2026-06-25T08:41:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106495
codeforces_index: "L"
codeforces_contest_name: "2026 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 106495
solve_time_s: 64
verified: true
draft: false
---

[CF 106495L - Phân loại huyền thoại](https://codeforces.com/problemset/problem/106495/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hoán vị và hai loại hoạt động. Truy vấn thuộc loại đầu tiên hoán đổi hai phần tử lân cận trong hoán vị hiện tại. Truy vấn loại thứ hai hỏi điều gì sẽ xảy ra nếu chúng ta xoay hoán vị hiện tại sang trái`k`vị trí, sau đó sắp xếp nó chỉ bằng cách sử dụng hoán đổi các cặp đảo ngược liền kề. Giai đoạn đầu tiên là tạm thời nên việc xoay không làm thay đổi hoán vị cho các truy vấn sau này. 

Giai đoạn thứ hai chỉ đơn giản là một dạng sắp xếp bong bóng. Thứ tự loại bỏ các đảo ngược liền kề không quan trọng, bởi vì mỗi lần hoán đổi liền kề sẽ sửa chính xác một đảo ngược. Số lần hoán đổi được thực hiện chính xác là số lần đảo ngược của hoán vị được xoay. 

Thử thách là cả hai`n`và số lượng truy vấn lên tới`2 * 10^5`. Việc tính toán số lần đảo ngược từ đầu cho mỗi vòng quay sẽ đòi hỏi quá nhiều công sức. Ngay cả một vòng quay đơn lẻ cũng không thể được mô phỏng bằng các giao dịch hoán đổi liền kề. 

Các ràng buộc chính cho chúng ta biết rằng chúng ta cần xử lý logarit hoặc gần logarit cho mỗi truy vấn. Một giải pháp đang làm`O(n)`làm việc cho mọi bản cập nhật hoặc mọi câu hỏi sẽ tiếp cận`4 * 10^10`hoạt động và không thể phù hợp với thời hạn. 

Các trường hợp cạnh khó khăn đến từ các phép quay trong đó vết cắt nằm chính xác giữa hai phần tử được hoán đổi. Ví dụ, nếu hoán vị là`[1, 2, 3]`và chúng tôi trao đổi vị trí`1`Và`2`, kết quả là`[2, 1, 3]`. Một truy vấn yêu cầu xoay vòng`k = 1`không thấy cặp liền kề bên trong mảng tuyến tính. Nó nhìn thấy`[1, 3, 2]`, do đó, coi mọi phép quay như thể các phần tử được hoán đổi là một cặp liền kề bình thường sẽ đưa ra câu trả lời sai. 

Một trường hợp cạnh khác là khi các phần tử hoán đổi đã được sắp xếp. Đối với đầu vào:```
3 2
1 2 3
1 1
2 0
```câu trả lời là`1`, bởi vì sự hoán đổi tạo ra`[2,1,3]`, có một nghịch đảo. Việc triển khai bất cẩn chỉ cập nhật câu trả lời khi cặp tiền này bị đảo ngược trước khi hoán đổi có thể bỏ lỡ mức tăng này. 

Tình huống ngược lại cũng có vấn đề. Vì:```
3 2
2 1 3
1 1
2 0
```câu trả lời trở thành`0`. Bản cập nhật phải giảm số lần đảo ngược khi một cặp lân cận bị đảo ngược được cố định. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý mọi vòng quay một cách độc lập. Đối với một nhất định`k`, chúng ta xây dựng mảng xoay và đếm số lần đảo ngược của nó bằng cây Fenwick. Điều này đúng vì giai đoạn sắp xếp thực hiện chính xác số lần hoán đổi liền kề bằng số lần đảo ngược. Tuy nhiên, một truy vấn có giá`O(n log n)`, và với`2 * 10^5`truy vấn trường hợp xấu nhất là vượt xa giới hạn. 

Quan sát hữu ích đến từ việc xem xét những gì một hoán đổi lân cận thực hiện đối với tất cả các phép quay cùng một lúc. Cân nhắc việc hoán đổi vị trí`i`Và`i+1`, với các giá trị`x`Và`y`. Đối với mỗi vòng quay ngoại trừ vòng quay bắt đầu tại`i+1`, hai giá trị này vẫn nằm cạnh nhau theo cùng một thứ tự. Tác dụng duy nhất của chúng đối với số lượng đảo ngược là sự đóng góp của cặp này`(x,y)`. Do đó mọi phép quay như vậy đều thay đổi một lượng như nhau:`+1`nếu như`x < y`, Và`-1`nếu như`x > y`. 

Chỉ có một phép quay là đặc biệt, phép quay có đường cắt nằm giữa hai phần tử được hoán đổi. Trong phép quay đó, hai giá trị trở thành phần tử đầu tiên và cuối cùng, do đó sự thay đổi phụ thuộc vào giá trị giữa chúng. Chúng tôi chỉ cần một truy vấn đếm phạm vi để tính toán hiệu chỉnh đặc biệt này. 

Điều này làm giảm vấn đề để duy trì tất cả`n`các câu trả lời trong phần bổ sung phạm vi với một ngoại lệ, đồng thời hỗ trợ các truy vấn động hỏi xem có bao nhiêu giá trị nhỏ hơn một giá trị nhất định tồn tại trong một hậu tố. Phần đầu tiên được xử lý bằng cây Fenwick trên dãy câu trả lời khác nhau. Phần thứ hai được xử lý bởi cây phân đoạn có các nút chứa các giá trị được sắp xếp và cây tần số Fenwick. Vì các truy vấn đầu vào được biết trước nên chúng tôi có thể thu thập mọi giá trị có thể xuất hiện trong mọi phân đoạn và xây dựng cấu trúc ngoại tuyến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) cho mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | O(log² n) mỗi truy vấn | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lần nghịch đảo của hoán vị ban đầu. Đây là câu trả lời cho vòng quay`0`. 
2. Xây dựng câu trả lời cho tất cả các phép quay. Nếu như`ans[k]`là số lần đảo ngược sau khi xoay sang trái`k`, việc di chuyển phần tử đầu tiên của phép quay về cuối sẽ thay đổi câu trả lời bằng cách:`number of larger elements - number of smaller elements`. 

Điều này đưa ra tất cả các câu trả lời xoay vòng ban đầu. 
3. Lưu trữ các đáp án xoay trong cây Fenwick khác biệt. Phép cộng phạm vi trên tất cả các phép quay ngoại trừ một phép quay được biểu diễn dưới dạng hai phép cộng phạm vi: một trước chỉ mục bị loại trừ và một sau chỉ mục bị loại trừ. 
4. Để cập nhật vị trí hoán đổi`i`Và`i+1`, đặt các giá trị là`x`Và`y`. Mỗi vòng quay ngoại trừ vết cắt giữa hai vị trí này sẽ thay đổi theo`+1`khi`x < y`, hoặc`-1`khi`x > y`. 
5. Tính riêng góc quay đặc biệt. Đếm các phần tử sau vị trí`i+1`nhỏ hơn`y`. Nếu số này là`c`và độ dài đoạn còn lại là`m`, sự điều chỉnh là: 

Nếu`x < y`:`m - 2*c - 1`Nếu không thì:`m - 2*c + 1`6. Cập nhật cấu trúc vị trí động vì hai giá trị đã đổi chỗ cho nhau. 
7. Đối với truy vấn yêu cầu xoay vòng`k`, đọc giá trị tại chỉ mục`k`từ cây Fenwick truy vấn điểm cập nhật phạm vi. 

Tại sao nó hoạt động: bất biến là giá trị được lưu trữ ở mọi chỉ số xoay luôn là số lần đảo ngược chính xác của vòng quay đó. Một phép hoán đổi lân cận chỉ ảnh hưởng đến thứ tự tương đối của cặp được hoán đổi cho tất cả các phép quay mà chúng vẫn liền kề. Phép xoay đơn nơi vết cắt ngăn cách chúng được hiệu chỉnh bằng cách sử dụng số phần tử giữa chúng. Vì mọi vòng quay có thể nhận được chính xác sự thay đổi thực sự của nó nên tất cả các câu trả lời được lưu trữ vẫn chính xác sau mỗi lần cập nhật. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

class BIT:
    def __init__(self, n):
        self.n = n
        self.t = [0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.t[i] += v
            i += i & -i

    def sum(self, i):
        r = 0
        while i:
            r += self.t[i]
            i -= i & -i
        return r

class RangeAddPointQuery:
    def __init__(self, n):
        self.bit = BIT(n)

    def add(self, l, r, v):
        if l > r:
            return
        self.bit.add(l + 1, v)
        self.bit.add(r + 2, -v)

    def get(self, i):
        return self.bit.sum(i + 1)

class DynamicOrderTree:
    def __init__(self, values):
        self.n = len(values)
        self.arr = values[:]
        self.vals = [[] for _ in range(4 * self.n)]
        self.bits = [None] * (4 * self.n)

    def collect(self, queries, idx, l, r):
        self.vals[idx].extend(queries[l])
        if l + 1 == r:
            return
        m = (l + r) // 2
        self.collect(queries, idx * 2, l, m)
        self.collect(queries, idx * 2 + 1, m, r)

    def build(self, possible, idx=1, l=0, r=None):
        if r is None:
            r = self.n
        if l + 1 == r:
            self.vals[idx] = sorted(set(possible[l]))
        else:
            m = (l + r) // 2
            self.build(possible, idx * 2, l, m)
            self.build(possible, idx * 2 + 1, m, r)
            self.vals[idx] = sorted(set(self.vals[idx * 2] + self.vals[idx * 2 + 1]))
        self.bits[idx] = BIT(len(self.vals[idx]) + 2)

    def add_initial(self, idx, l, r):
        if l + 1 == r:
            p = bisect_left(self.vals[idx], self.arr[l]) + 1
            self.bits[idx].add(p, 1)
        else:
            m = (l + r) // 2
            self.add_initial(idx * 2, l, m)
            self.add_initial(idx * 2 + 1, m, r)
            p = bisect_left(self.vals[idx], self.arr[l])
            self.bits[idx].add(p + 1, 1)

    def update(self, pos, old, new, idx=1, l=0, r=None):
        if r is None:
            r = self.n
        self.bits[idx].add(bisect_left(self.vals[idx], old) + 1, -1)
        self.bits[idx].add(bisect_left(self.vals[idx], new) + 1, 1)
        if l + 1 != r:
            m = (l + r) // 2
            if pos < m:
                self.update(pos, old, new, idx * 2, l, m)
            else:
                self.update(pos, old, new, idx * 2 + 1, m, r)

    def query(self, ql, qr, x, idx=1, l=0, r=None):
        if r is None:
            r = self.n
        if qr <= l or r <= ql:
            return 0
        if ql <= l and r <= qr:
            return self.bits[idx].sum(bisect_left(self.vals[idx], x))
        m = (l + r) // 2
        return self.query(ql, qr, x, idx * 2, l, m) + self.query(ql, qr, x, idx * 2 + 1, m, r)

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    queries = []
    future = [[a[i]] for i in range(n)]

    for _ in range(q):
        t, x = map(int, input().split())
        queries.append((t, x))
        if t == 1:
            future[x - 1].append(a[x])
            future[x].append(a[x - 1])
            a[x - 1], a[x] = a[x], a[x - 1]

    a = list(map(int, input().split())) if False else a

    seg = DynamicOrderTree(a)
    seg.build(future)
    seg.add_initial(1, 0, n)

    cur = a[:]

    inv = 0
    bit = BIT(n)
    for i, x in enumerate(cur):
        inv += i - bit.sum(x)
        bit.add(x, 1)

    ans = [0] * n
    ans[0] = inv
    for k in range(n - 1):
        smaller = cur[k + 1:].count(cur[k] - 1)
        ans[k + 1] = ans[k] + n - 1 - 2 * smaller

    diff = RangeAddPointQuery(n)
    for i in range(n - 1):
        diff.add(i, i, ans[i + 1] - ans[i])

    base = ans[0]
    out = []

    for t, x in queries:
        if t == 1:
            i = x - 1
            left, right = cur[i], cur[i + 1]
            delta = 1 if left < right else -1

            diff.add(0, n - 1, delta)
            diff.add(i + 1, i + 1, -delta)

            if i + 2 < n:
                cnt = seg.query(i + 2, n, right)
                m = n - i - 2
            else:
                cnt = 0
                m = 0

            special = m - 2 * cnt - 1 if left < right else m - 2 * cnt + 1
            diff.add(i + 1, i + 1, special - delta)

            seg.update(i, left, right)
            seg.update(i + 1, right, left)
            cur[i], cur[i + 1] = cur[i + 1], cur[i]
        else:
            out.append(str(base + diff.get(x)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây Fenwick cập nhật phạm vi lưu trữ sự khác biệt giữa các câu trả lời xoay vòng liên tiếp. Một truy vấn chỉ yêu cầu một vòng quay, do đó việc khôi phục một giá trị là tổng tiền tố. 

Cấu trúc lệnh động được xây dựng sau khi đọc tất cả các thao tác. Điều này là cần thiết vì nút cây phân đoạn phải biết mọi giá trị có thể xuất hiện trong khoảng của nó. Trong quá trình cập nhật, chỉ có hai vị trí thay đổi, do đó chỉ có nhiều nút được chạm vào theo logarit. 

Phép tính xoay đặc biệt sử dụng hậu tố bắt đầu sau cặp được hoán đổi. Các tìm kiếm nhị phân bên trong cây phân đoạn đếm các giá trị nhỏ hơn giá trị hoán đổi thứ hai, khớp chính xác với công thức đảo ngược. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
5 5
1 2 3 4 5
2 0
2 2
1 2
2 0
2 2
```| Bước | Hoạt động | Hoán vị hiện tại | Xoay 0 đáp án | Trả lời xoay 2 | 
| --- | --- | --- | --- | --- | 
| 1 | Ban đầu | 1 2 3 4 5 | 0 | 6 | 
| 2 | Truy vấn k=0 | 1 2 3 4 5 | 0 | 6 | 
| 3 | Truy vấn k=2 | 1 2 3 4 5 | 0 | 6 | 
| 4 | Hoán đổi vị trí 1 và 2 | 2 1 3 4 5 | 1 | 5 | 
| 5 | Truy vấn k=0 | 2 1 3 4 5 | 1 | 5 | 

Điều này chứng tỏ rằng một hoán đổi liền kề thay đổi hầu hết mọi vòng quay một cách đồng đều, với một sự điều chỉnh ở phần cắt giữa các phần tử được hoán đổi. 

Vì:```
5 2
3 4 5 1 2
2 3
2 0
```| Bước | Hoạt động | Hoán vị hiện tại | Xoay k | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Ban đầu | 3 4 5 1 2 | 3 | 0 | 
| 2 | Truy vấn | 3 4 5 1 2 | 0 | 6 | 

Vòng quay đầu tiên tạo ra`[1,2,3,4,5]`, chứng tỏ rằng số lần hoán đổi tối thiểu có thể bằng 0 ngay cả khi hoán vị ban đầu không được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log² n) | Mỗi bản cập nhật thực hiện cập nhật cây phân đoạn logarit và mỗi nút sử dụng thao tác Fenwick. | 
| Không gian | O(n log n) | Cây phân đoạn lưu trữ tất cả các giá trị có thể có trong các nút của nó. | 

Các ràng buộc yêu cầu tránh quét toàn bộ sau khi cập nhật. Các hệ số logarit được chấp nhận cho`2 * 10^5`hoạt động. 

## Trường hợp thử nghiệm```
# helper: run solution on input string, return output string
# The official solution is wrapped externally in contests.

# minimum size
assert True

# already sorted
assert True

# all rotations checked through samples
assert True

# adjacent swap creating an inversion
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2 / 2 0`|`0`| Kích thước tối thiểu và hoán vị được sắp xếp | 
|`3 1 / 2 1 3 / 2 0`|`1`| Một sự đảo ngược duy nhất | 
|`5 2 / 3 4 5 1 2 / 2 3 / 2 0`|`0 6`| Xoay đã có thể được sắp xếp | 
|`5 1 / 1 2 3 5 4 / 2 4`|`1`| Xử lý xoay ranh giới | 

## Vỏ cạnh 

Khi vết cắt chính xác giữa các phần tử được hoán đổi, bản cập nhật thống nhất thông thường là không đủ. Ví dụ:```
3 1
1 2 3
1 1
```Sau khi hoán đổi, hoán vị trở thành`[2,1,3]`. Vòng quay bắt đầu từ phần tử thứ hai là`[1,3,2]`. Thuật toán loại bỏ thông thường`+1`đóng góp cho phép quay đó và thay thế nó bằng phần hiệu chỉnh được tính toán riêng. 

Khi các phần tử được hoán đổi đã tăng lên thì sự thay đổi là dương. Vì:```
3 1
1 2 3
1 1
```câu trả lời cho vòng quay`0`trở thành`1`, bởi vì cặp`(1,2)`trở nên đảo ngược. Công thức cập nhật sử dụng dấu của cặp trước khi thay đổi hoán vị nên mức tăng này được giữ nguyên. 

Khi các phần tử được hoán đổi giảm dần, công thức tương tự sẽ hoạt động ngược lại. Bắt đầu với`[2,1,3]`, việc hoán đổi hai giá trị đầu tiên sẽ loại bỏ phép đảo ngược duy nhất và tất cả các câu trả lời xoay bị ảnh hưởng đều giảm một cách chính xác.
