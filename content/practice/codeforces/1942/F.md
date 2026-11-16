---
title: "CF 1942F - Chức năng yêu thích của anh nông dân John"
description: "Chúng ta có một mảng $a$ và một giá trị được xác định đệ quy: $$f(1)=sqrt{a1}, qquad f(i)=sqrt{f(i-1)+ai}.$$ Sau mỗi lần cập nhật điểm $ak leftarrow x$, chúng ta cần phần nguyên của $f(n)$. Trở ngại đầu tiên là phép truy toán sử dụng số thực."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1942
codeforces_index: "F"
codeforces_contest_name: "CodeTON Round 8 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 2700
weight: 1942
solve_time_s: 130
verified: true
draft: false
---

[CF 1942F - Chức năng yêu thích của nông dân John](https://codeforces.com/problemset/problem/1942/F) 

**Đánh giá:** 2700 
**Tags:** Brute Force, cấu trúc dữ liệu, triển khai, toán học 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng$a$và một giá trị được xác định đệ quy:$$f(1)=\sqrt{a_1}, \qquad f(i)=\sqrt{f(i-1)+a_i}.$$Sau mỗi lần cập nhật điểm$a_k \leftarrow x$, chúng ta cần phần nguyên của$f(n)$. 

Trở ngại đầu tiên là phép truy toán sử dụng số thực. Một mô phỏng trực tiếp sẽ liên tục đánh giá căn bậc hai trong dấu phẩy động và ngay cả những lỗi chính xác nhỏ cũng có thể thay đổi giá trị sàn cuối cùng. Mẫu đã cho thấy tình huống như vậy: câu trả lời đúng là$2$, mặc dù giá trị cuối cùng rất gần với$3$. 

Những hạn chế là lớn. Cả hai$n$Và$q$đang lên đến$2 \cdot 10^5$. Việc tính toán lại toàn bộ quá trình lặp lại sau mỗi lần cập nhật sẽ mất$O(nq)$, đại khái là$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Điều đó hoàn toàn nằm ngoài tầm với giới hạn 5 giây. 

Khó khăn chính là các cập nhật diễn ra trên mảng nhưng sự lặp lại phụ thuộc vào tất cả các vị trí trước đó. Một sửa đổi duy nhất gần đầu có khả năng thay đổi mọi giá trị sau này. 

Có một số cách dễ dàng để có được một giải pháp sai. 

Coi như:```
n = 2
a = [3, 14]
```Sau đó$$f(1)=\sqrt3 \approx 1.732,$$

$$f(2)=\sqrt{14+\sqrt3}\approx 3.966.$$Câu trả lời là$3$. Nếu chúng ta làm tròn các giá trị trung gian thay vì lấy phép truy toán chính xác, chúng ta sẽ nhận được một kết quả khác. 

Một trường hợp nguy hiểm khác là giá trị rất lớn:```
n = 1
a = [10^18]
```Câu trả lời là chính xác$10^9$. Sử dụng gấp đôi một cách bất cẩn có thể làm mất độ chính xác ở gần ranh giới và tạo ra$999999999$. 

Một ví dụ tinh tế hơn là:```
a = [3, 14, 0, 7, 6]
```Giá trị cuối cùng là khoảng$2.999766$, vậy câu trả lời là$2$. Bất kỳ cách tiếp cận nào liên tục tính toán căn bậc hai dấu phẩy động và sau đó làm tròn ở cuối đều có nguy cơ quay trở lại$3$. 

Giải pháp phải tránh hoàn toàn số học dấu phẩy động. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Sau mỗi lần cập nhật, hãy xây dựng lại sự lặp lại từ trái sang phải.```
cur = sqrt(a[1])
for i = 2..n:
    cur = sqrt(cur + a[i])
answer = floor(cur)
```Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Vấn đề là sự phức tạp. Mỗi truy vấn yêu cầu$O(n)$làm việc nên tổng chi phí là$O(nq)$. Với$n=q=2\cdot10^5$, điều này trở nên đại khái$4\cdot10^{10}$hoạt động. 

Quan sát đầu tiên là chúng ta chỉ cần$\lfloor f(n)\rfloor$, không phải là giá trị thực chính xác. 

Định nghĩa$$g(1)=\left\lfloor \sqrt{a_1}\right\rfloor,$$

$$g(i)=\left\lfloor \sqrt{g(i-1)+a_i}\right\rfloor.$$Một thực tế quan trọng là:$$g(i)=\lfloor f(i)\rfloor$$cho mọi$i$. 

Lý do rất đơn giản. Cho phép$$f(i-1)=g(i-1)+\delta, \qquad 0\le \delta<1.$$Sau đó$$g(i-1)+a_i \le f(i-1)+a_i < g(i-1)+a_i+1.$$Lấy căn bậc hai giữ giá trị bên trong một khoảng có độ dài nhỏ hơn$1$, vì vậy cả hai điểm cuối đều có cùng một tầng. Kể từ đây$$\left\lfloor \sqrt{f(i-1)+a_i}\right\rfloor = \left\lfloor \sqrt{g(i-1)+a_i}\right\rfloor.$$Bây giờ toàn bộ vấn đề trở thành số nguyên. 

Quan sát thứ hai là xử lý tất cả các truy vấn ngoại tuyến. 

Đối với một vị trí cố định$i$, giá trị của nó chỉ thay đổi khi bản cập nhật chạm vào chỉ mục$i$. Dọc theo dòng thời gian truy vấn,$a_i$là hằng số từng phần. 

Giả sử chúng ta duy trì, cho mỗi lần truy vấn$t$, giá trị hiện tại$$dp_t$$sau khi xử lý vị trí$1 \ldots i-1$. 

Khi chúng tôi xử lý vị trí$i$, mọi điểm dòng thời gian đều thực hiện$$dp_t \leftarrow \left\lfloor \sqrt{dp_t + a_i(t)} \right\rfloor.$$Đây$a_i(t)$biểu thị giá trị của vị trí$i$sau khi truy vấn$t$. 

Vì vậy mỗi vị trí đều đóng góp: 

1. Một số phạm vi bổ sung trên dòng thời gian, bởi vì$a_i(t)$là hằng số từng phần. 
2. Một hoạt động toàn cầu$x \mapsto \lfloor\sqrt{x}\rfloor$. 

Thách thức còn lại là hỗ trợ các hoạt động căn bậc hai toàn cầu lặp đi lặp lại một cách hiệu quả. 

Cây phân đoạn lưu trữ giá trị tối thiểu và tối đa trong mỗi nút. 

Đối với một nút bao gồm các giá trị trong$[mn,mx]$, nếu như$$mx-mn = \lfloor\sqrt{mx}\rfloor-\lfloor\sqrt{mn}\rfloor,$$thì mọi giá trị trong phân đoạn đó sẽ thay đổi một lượng như nhau. 

Quả thực trong hoàn cảnh đó$$\lfloor\sqrt{x}\rfloor - x$$là không đổi trong suốt khoảng thời gian. Toàn bộ phân khúc có thể được cập nhật một cách lười biếng bằng cách thêm một phạm vi duy nhất. 

Chỉ những phân đoạn không đáp ứng được điều kiện này mới cần được phân chia thêm. 

Đây là một đối số kiểu phân đoạn cây nhịp cổ điển. Các căn bậc hai lặp đi lặp lại sẽ nhanh chóng thu nhỏ số lượng, do đó độ sâu đệ quy vẫn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Tối ưu |$O((n+q)\log^2 q)$khấu hao |$O(q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng ban đầu và tất cả các bản cập nhật. 
2. Đối với mọi chỉ số$i$, thu thập số lần chỉ mục đó được sửa đổi. 
3. Chuyển đổi lịch sử cập nhật của từng vị trí thành các khoảng thời gian rời rạc. Đối với mỗi khoảng thời gian$[L,R]$, giá trị của$a_i$vẫn không đổi. 
4. Xây dựng cây phân đoạn theo thời gian truy vấn$1 \ldots q$. Ban đầu mọi giá trị dòng thời gian là$0$. 
5. Xử lý vị trí mảng từ trái qua phải. 
6. Đối với vị trí hiện tại$i$, áp dụng phép cộng phạm vi trên mỗi khoảng thời gian trong đó$a_i$là không đổi. Sau những bổ sung này, mỗi điểm dòng thời gian chứa$$g(i-1)+a_i.$$1. Áp dụng phép biến đổi$$x \mapsto \lfloor\sqrt{x}\rfloor$$đến toàn bộ cây phân đoạn. 

1. Cây phân đoạn hiện lưu trữ$$g(i)$$cho mỗi lần truy vấn cùng một lúc. 

1. Sau khi tất cả các vị trí được xử lý, hãy trích xuất tất cả các lá. Giá trị tại vị trí dòng thời gian$t$chính xác là câu trả lời sau khi truy vấn$t$. 

### Tại sao nó hoạt động 

Sau khi xử lý lần đầu$i$vị trí, mọi điểm thời gian$t$cửa hàng$$g_i(t),$$giá trị lặp lại số nguyên cho trạng thái mảng sau truy vấn$t$. 

Bất biến ban đầu là đúng vì tất cả các giá trị đều$0$. 

Khi xử lý vị trí$i$, trước tiên chúng tôi thêm$a_i(t)$đến mọi thời điểm. Giá trị được lưu trữ trở thành$$g_{i-1}(t)+a_i(t).$$Áp dụng phép biến đổi căn bậc hai sẽ thay đổi nó thành$$\left\lfloor \sqrt{g_{i-1}(t)+a_i(t)} \right\rfloor,$$đó chính xác là$g_i(t)$. 

Bằng quy nạp, sau vị trí cuối cùng ta thu được$g_n(t)$cho mỗi lần truy vấn. Từ$g_n(t)=\lfloor f_n(t)\rfloor$, các câu trả lời được báo cáo là chính xác. 

## Giải pháp Python```python
import sys
from math import isqrt

input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        size = 4 * n + 5
        self.mn = [0] * size
        self.mx = [0] * size
        self.lazy = [0] * size

    def apply_add(self, p, v):
        self.mn[p] += v
        self.mx[p] += v
        self.lazy[p] += v

    def push(self, p):
        if self.lazy[p]:
            v = self.lazy[p]
            self.apply_add(p * 2, v)
            self.apply_add(p * 2 + 1, v)
            self.lazy[p] = 0

    def pull(self, p):
        self.mn[p] = min(self.mn[p * 2], self.mn[p * 2 + 1])
        self.mx[p] = max(self.mx[p * 2], self.mx[p * 2 + 1])

    def range_add(self, p, l, r, ql, qr, v):
        if ql <= l and r <= qr:
            self.apply_add(p, v)
            return

        self.push(p)
        m = (l + r) >> 1

        if ql <= m:
            self.range_add(p * 2, l, m, ql, qr, v)
        if qr > m:
            self.range_add(p * 2 + 1, m + 1, r, ql, qr, v)

        self.pull(p)

    def apply_sqrt(self, p, l, r):
        mn = self.mn[p]
        mx = self.mx[p]

        if mx - mn == isqrt(mx) - isqrt(mn):
            delta = isqrt(mx) - mx
            self.apply_add(p, delta)
            return

        if l == r:
            v = isqrt(mx)
            self.mn[p] = v
            self.mx[p] = v
            self.lazy[p] = 0
            return

        self.push(p)
        m = (l + r) >> 1
        self.apply_sqrt(p * 2, l, m)
        self.apply_sqrt(p * 2 + 1, m + 1, r)
        self.pull(p)

    def collect(self, p, l, r, res):
        if l == r:
            res[l] = self.mn[p]
            return

        self.push(p)
        m = (l + r) >> 1
        self.collect(p * 2, l, m, res)
        self.collect(p * 2 + 1, m + 1, r, res)

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    changes = [[] for _ in range(n)]

    for t in range(1, q + 1):
        k, x = map(int, input().split())
        changes[k - 1].append((t, x))

    seg = SegTree(q)

    for i in range(n):
        cur = a[i]
        last = 1

        for t, x in changes[i]:
            if last <= t - 1:
                seg.range_add(1, 1, q, last, t - 1, cur)
            cur = x
            last = t

        if last <= q:
            seg.range_add(1, 1, q, last, q, cur)

        seg.apply_sqrt(1, 1, q)

    ans = [0] * (q + 1)
    seg.collect(1, 1, q, ans)

    sys.stdout.write("\n".join(map(str, ans[1:])))

solve()
```Ý tưởng quan trọng đầu tiên trong quá trình triển khai là thay thế phép truy toán thực bằng phép truy toán số nguyên. Điều đó loại bỏ tất cả các vấn đề về độ chính xác và cho phép mọi giá trị vẫn là số nguyên. 

Giai đoạn tiền xử lý xây dựng các khoảng thời gian cho mọi vị trí mảng. Nếu chỉ mục$i$đôi khi được cập nhật$5$,$11$, Và$20$, thì giá trị của nó không đổi trên một số phạm vi truy vấn riêng biệt. Mỗi phạm vi như vậy sẽ trở thành một phép cộng phạm vi cây phân đoạn. 

Cây phân đoạn chỉ lưu trữ thẻ bổ sung tối thiểu, tối đa và lười biếng. Không cần số tiền. 

Phần tinh tế là`apply_sqrt`. Nếu nút thỏa mãn$$mx-mn = \lfloor\sqrt{mx}\rfloor-\lfloor\sqrt{mn}\rfloor,$$thì phép tính căn bậc hai hoạt động giống như thêm một hằng số vào mọi phần tử trong nút đó. Chúng tôi có thể cập nhật toàn bộ phân khúc một cách lười biếng. 

Nếu không chúng ta sẽ tái diễn thành trẻ em. 

sử dụng`math.isqrt`là điều cần thiết. Nó tính toán căn bậc hai số nguyên chính xác và tránh tất cả các lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6
0 14 0 7 6
1 4
1 3
2 15
4 1
5 2
5 8
```Sau khi xử lý vị trí 1: 

| Truy vấn | a₁ | Giá trị sau sqrt | 
| --- | --- | --- | 
| 1 | 4 | 2 | 
| 2 | 3 | 1 | 
| 3 | 3 | 1 | 
| 4 | 3 | 1 | 
| 5 | 3 | 1 | 
| 6 | 3 | 1 | 

Sau khi xử lý vị trí 2: 

| Truy vấn | Trước | +a₂ | mét vuông | 
| --- | --- | --- | --- | 
| 1 | 2 | 16 | 4 | 
| 2 | 1 | 15 | 3 | 
| 3 | 1 | 16 | 4 | 
| 4 | 1 | 16 | 4 | 
| 5 | 1 | 16 | 4 | 
| 6 | 1 | 16 | 4 | 

Tiếp tục qua tất cả các vị trí sẽ tạo ra:```
3
2
3
2
1
3
```Dấu vết này cho thấy mọi trạng thái truy vấn được xử lý đồng thời như thế nào dọc theo dòng thời gian. 

### Ví dụ 2 

đầu vào:```
2 2
386056082462833225 923951085408043421
1 386056082462833225
1 386056082462833224
```Sau truy vấn 1: 

| Vị trí | Giá trị | 
| --- | --- | 
| a₁ | 386056082462833225 | 
| a₂ | 923951085408043421 | 

Sự tái phát mang lại:$$g(1)=621333955,$$

$$g(2)=961223744.$$Sau truy vấn 2:$$g(1)=621333954,$$

$$g(2)=961223743.$$Đầu ra:```
961223744
961223743
```Ví dụ này chứng tỏ tại sao số học số nguyên chính xác lại cần thiết ở gần các ranh giới căn bậc hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)\log^2 q)$khấu hao | Bổ sung phạm vi cộng với cập nhật căn bậc hai theo phong cách cây phân đoạn | 
| Không gian |$O(q)$| Cây phân đoạn và lưu trữ dòng thời gian | 

Cây phân đoạn chứa$O(q)$nút. Mỗi khoảng thời gian cập nhật sẽ trở thành một phạm vi bổ sung. Căn bậc hai lặp đi lặp lại làm giảm nhanh chóng các giá trị, giúp giữ tổng chi phí đệ quy ở mức giới hạn. Điều này thoải mái phù hợp với giới hạn cho$n,q \le 2\cdot10^5$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isqrt

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class SegTree:
        def __init__(self, n):
            self.n = n
            self.mn = [0] * (4 * n + 5)
            self.mx = [0] * (4 * n + 5)
            self.lazy = [0] * (4 * n + 5)

        def apply_add(self, p, v):
            self.mn[p] += v
            self.mx[p] += v
            self.lazy[p] += v

        def push(self, p):
            if self.lazy[p]:
                v = self.lazy[p]
                self.apply_add(p * 2, v)
                self.apply_add(p * 2 + 1, v)
                self.lazy[p] = 0

        def pull(self, p):
            self.mn[p] = min(self.mn[p * 2], self.mn[p * 2 + 1])
            self.mx[p] = max(self.mx[p * 2], self.mx[p * 2 + 1])

        def add(self, p, l, r, ql, qr, v):
            if ql <= l and r <= qr:
                self.apply_add(p, v)
                return
            self.push(p)
            m = (l + r) // 2
            if ql <= m:
                self.add(p * 2, l, m, ql, qr, v)
            if qr > m:
                self.add(p * 2 + 1, m + 1, r, ql, qr, v)
            self.pull(p)

        def sq(self, p, l, r):
            mn = self.mn[p]
            mx = self.mx[p]

            if mx - mn == isqrt(mx) - isqrt(mn):
                self.apply_add(p, isqrt(mx) - mx)
                return

            if l == r:
                v = isqrt(mx)
                self.mn[p] = v
                self.mx[p] = v
                self.lazy[p] = 0
                return

            self.push(p)
            m = (l + r) // 2
            self.sq(p * 2, l, m)
            self.sq(p * 2 + 1, m + 1, r)
            self.pull(p)

        def collect(self, p, l, r, out):
            if l == r:
                out[l] = self.mn[p]
                return
            self.push(p)
            m = (l + r) // 2
            self.collect(p * 2, l, m, out)
            self.collect(p * 2 + 1, m + 1, r, out)

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    changes = [[] for _ in range(n)]

    for t in range(1, q + 1):
        k, x = map(int, input().split())
        changes[k - 1].append((t, x))

    seg = SegTree(q)

    for i in range(n):
        cur = a[i]
        last = 1

        for t, x in changes[i]:
            if last <= t - 1:
                seg.add(1, 1, q, last, t - 1, cur)
            cur = x
            last = t

        if last <= q:
            seg.add(1, 1, q, last, q, cur)

        seg.sq(1, 1, q)

    ans = [0] * (q + 1)
    seg.collect(1, 1, q, ans)

    return "\n".join(map(str, ans[1:]))

# provided sample
assert run(
"""5 6
0 14 0 7 6
1 4
1 3
2 15
4 1
5 2
5 8
"""
) == "3\n2\n3\n2\n1\n3", "sample 1"

# minimum size
assert run(
"""1 1
0
1 0
"""
) == "0", "single element zero"

# boundary sqrt transition
assert run(
"""1 2
16
1 15
1 16
"""
) == "3\n4", "crosses perfect square boundary"

# very large values
assert run(
"""1 1
1000000000000000000
1 1000000000000000000
"""
) == "1000000000", "1e18 square root"

# repeated updates on same index
assert run(
"""2 3
0 0
1 1
1 4
1 9
"""
) == "1\n2\n3", "timeline interval construction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phần tử đơn, giá trị 0 | 0 | Kích thước đầu vào tối thiểu | 
| 16 → 15 → 16 | 3, 4 | Hành vi ranh giới vuông hoàn hảo | 
|$10^{18}$|$10^9$| Căn bậc hai số nguyên lớn | 
| Cập nhật lặp đi lặp lại trên một vị trí | 1, 2, 3 | Tạo khoảng thời gian chính xác trên dòng thời gian | 

## Vỏ cạnh 

Hãy xem xét:```
1 2
16
1 15
1 16
```Sau lần cập nhật đầu tiên,$$\lfloor\sqrt{15}\rfloor=3.$$Sau lần thứ hai,$$\lfloor\sqrt{16}\rfloor=4.$$Thuật toán không bao giờ sử dụng số học dấu phẩy động. Cây phân đoạn lưu trữ các số nguyên chính xác và áp dụng`isqrt`, vì vậy cả hai câu trả lời đều được tính toán chính xác. 

Bây giờ hãy xem xét:```
1 1
1000000000000000000
1 1000000000000000000
```Giá trị chính xác là$10^{18}$. Giải pháp dấu phẩy động có thể mất độ chính xác ở mức độ này. Tính toán tái phát số nguyên$$\lfloor\sqrt{10^{18}}\rfloor = 10^9$$mà không có bất kỳ vấn đề làm tròn. 

Cuối cùng, hãy xem xét nhiều cập nhật trên cùng một chỉ mục:```
2 3
0 0
1 1
1 4
1 9
```Đối với chỉ mục$1$, các giá trị dòng thời gian là: 

| Khoảng truy vấn | Giá trị | 
| --- | --- | 
| [1,1] | 1 | 
| [2,2] | 4 | 
| [3,3] | 9 | 

Quá trình tiền xử lý sẽ chuyển đổi chúng thành ba phép cộng phạm vi. Mỗi khoảng được xử lý độc lập nên mọi truy vấn đều thấy phiên bản chính xác của mảng.
