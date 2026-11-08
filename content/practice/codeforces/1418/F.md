---
title: "CF 1418F - Sản phẩm tương đương"
description: "Cố định giá trị $x1$. Chúng ta cần tìm hai cách biểu diễn khác nhau của cùng một sản phẩm: $$x1 y1 = x2 y2,$$ với $x2 x1$, $y2 < y1$ và sản phẩm nằm bên trong $[l,r]$. Đầu ra được yêu cầu riêng cho mỗi $x1$ từ $1$ đến $n$."
date: "2026-06-11T06:52:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1418
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 95 (Rated for Div. 2)"
rating: 3000
weight: 1418
solve_time_s: 122
verified: true
draft: false
---

[CF 1418F - Sản phẩm tương đương](https://codeforces.com/problemset/problem/1418/F) 

**Đánh giá:** 3000 
**Tags:** cấu trúc dữ liệu, toán, lý thuyết số, hai con trỏ 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cố định một giá trị$x_1$. Chúng ta cần tìm hai cách biểu diễn khác nhau của cùng một sản phẩm:$$x_1 y_1 = x_2 y_2,$$với$x_2 > x_1$,$y_2 < y_1$, và sản phẩm nằm bên trong$[l,r]$. 

Đầu ra được yêu cầu riêng cho mỗi$x_1$từ$1$ĐẾN$n$. Đối với một số giá trị của$x_1$không tồn tại bộ tứ hợp lệ, trong trường hợp đó chúng tôi in$-1$. 

Những ràng buộc là nguyên nhân làm cho vấn đề trở nên khó khăn. Cả hai$n$Và$m$có thể đạt được$2 \cdot 10^5$, vì vậy bất cứ điều gì bậc hai ngay lập tức là không thể. Thậm chí lặp lại trên tất cả các cặp$(x_2,y_2)$cho mọi$x_1$sẽ đòi hỏi hàng chục tỷ hoạt động. 

Hạn chế về phạm vi sản phẩm cũng có thể gây hiểu nhầm. Một cách tiếp cận đơn giản có thể thử lặp lại tất cả các sản phẩm trong$[l,r]$, Nhưng$r$có thể lớn như$nm$, tùy thuộc vào$4 \cdot 10^{10}$. 

Một trường hợp cạnh tinh tế xuất hiện khi phạm vi hợp lệ của$y_1$trống rỗng. 

Ví dụ:```
n = 3
m = 5
l = 20
r = 20
```Vì$x_1 = 3$,$$y_1 = \frac{20}{3}$$không phải là số nguyên và không có giá trị của$y_1$đáp ứng cả phạm vi sản phẩm và$1 \le y_1 \le m$. Bất kỳ việc triển khai nào giả định khoảng không trống sẽ tạo ra các câu trả lời không hợp lệ. 

Một sai lầm dễ dàng khác là quên điều đó$y_2$phải nhỏ hơn rất nhiều so với$y_1$. 

Coi như:```
x1 = 4
y1 = 6
x2 = 8
y2 = 3
```Điều này hợp lệ vì$24 = 24$Và$3 < 6$. 

Nhưng nếu$a=b$trong việc xây dựng tỷ lệ giảm được mô tả sau, sau đó$y_2=y_1$, điều đó bị cấm. Chúng ta phải luôn thực thi$a>b$. 

Cái bẫy thứ ba là tạo ra một sản phẩm tương tự nhưng cho phép$x_2>n$. 

Ví dụ:```
n = 5
x1 = 4
```Tỷ lệ sử dụng$a/b = 3/2$cho$$x_2 = 4 \cdot \frac{3}{2} = 6,$$vi phạm giới hạn về$x_2$. Tỷ số phải thỏa mãn giới hạn$x_2$cũng vậy. 

## Phương pháp tiếp cận 

Giả sử chúng ta sửa$x_1$. 

Các giá trị hợp lệ của$y_1$chính xác là những điều thỏa mãn$$l \le x_1 y_1 \le r,$$cùng với$1 \le y_1 \le m$. Điều này mang lại một khoảng thời gian$$[y_{\text{low}}, y_{\text{high}}].$$Một giải pháp vũ phu sẽ thử mọi cách$y_1$trong khoảng thời gian đó và tìm kiếm một hệ số khác của cùng một sản phẩm. Thậm chí kiểm tra tất cả những gì có thể$x_2$giá trị dẫn đến khoảng$$O(nm)$$hoặc tệ hơn là vượt xa giới hạn. 

Quan sát quan trọng là các sản phẩm bằng nhau thực sự là một tuyên bố về tỷ lệ. 

Cho rằng$$x_1 y_1 = x_2 y_2.$$Viết$$\frac{x_2}{x_1}=\frac{a}{b}$$trong điều kiện thấp nhất. 

Từ$x_2>x_1$, chúng tôi có$a>b$. 

Vì phân số đã giảm đi nên$b$phải chia$x_1$. Cũng,$$\frac{y_1}{y_2}=\frac{a}{b},$$Vì thế$a$phải chia$y_1$. 

Vì vậy, một giải pháp tồn tại nếu chúng ta có thể tìm thấy:$$b \mid x_1,$$và một số$$a>b$$như vậy:$$x_1 \cdot \frac{a}{b} \le n,$$và ít nhất một số bên trong$[y_{\text{low}},y_{\text{high}}]$chia hết cho$a$. 

Bây giờ vấn đề trở thành vấn đề truy vấn số chia. 

Đối với mỗi khoảng thời gian$[y_{\text{low}},y_{\text{high}}]$, chúng ta cần biết giá trị nào của$a$có ít nhất một bội số trong khoảng đó. Khi đó với mọi ước số$b$của$x_1$, chúng tôi muốn hoạt động nhỏ nhất$a$thỏa mãn$$b < a \le \frac{nb}{x_1}.$$Các khoảng thay đổi theo$x_1$và điểm cuối của chúng di chuyển đơn điệu. Điều này làm cho việc quét hai con trỏ có thể thực hiện được. Chúng tôi duy trì tất cả các ước hiện có bội số bên trong khoảng hoạt động. Cây phân đoạn cho phép chúng ta tìm hoạt động đầu tiên$a$trong một phạm vi. 

Điều đó làm giảm toàn bộ vấn đề xuống còn khoảng$O((n+m)\log^2 n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tối ưu |$O((n+m)\log^2 n)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các ước số cho mọi số từ$1$ĐẾN$\max(n,m)$sử dụng vòng lặp kiểu sàng. 
2. Đối với mọi$x$, tính toán trước danh sách ước số của nó. 
3. Đối với mỗi$x$, tính toán$$L_x=\max\!\left(1,\left\lceil\frac{l}{x}\right\rceil\right),$$Và$$R_x=\min\!\left(m,\left\lfloor\frac{r}{x}\right\rfloor\right).$$Đây chính xác là những giá trị hợp lệ của$y_1$. 

1. Quy trình$x$từ$n$xuống$1$. 
2. Duy trì khoảng thời gian hiện tại$[curL,curR]$, ban đầu trống rỗng. 
3. Khi khoảng mục tiêu thay đổi so với khoảng thời gian trước đó, hãy di chuyển hai con trỏ. Bất cứ khi nào một giá trị$y$vào khoảng, cộng tất cả các ước của$y$. Bất cứ khi nào một giá trị$y$rời khỏi khoảng, loại bỏ tất cả các ước của$y$. 
4. Với mọi ước số$a$, giữ một bộ đếm bằng số bội số của$a$hiện có mặt trong khoảng đó. 
5. Một giá trị$a$được kích hoạt nếu bộ đếm của nó dương. Lưu trữ các giá trị hoạt động trong cây phân đoạn. 
6. Đối với hiện tại$x$, lặp qua tất cả các ước$b$của$x$. 
7. Giá trị cho phép lớn nhất của$a$là$$\text{limit}=\left\lfloor \frac{nb}{x}\right\rfloor.$$1. Truy vấn cây phân đoạn để tìm giá trị hoạt động đầu tiên trong$$[b+1,\text{limit}].$$1. Nếu như vậy$a$tồn tại, xây dựng$$y_1=\left\lceil\frac{L_x}{a}\right\rceil a.$$Bởi vì$a$đang hoạt động, bội số này được đảm bảo nằm trong khoảng. 

1. Đặt$$x_2=x_1\frac{a}{b}, \qquad y_2=y_1\frac{b}{a}.$$1. Xuất ra bộ bốn và ngừng tìm kiếm cái này$x_1$. 
2. Nếu không có ước số$b$tạo ra một hợp lệ$a$, đầu ra$-1$. 

### Tại sao nó hoạt động 

Khoảng duy trì chứa chính xác các giá trị hợp lệ của$y_1$cho hiện tại$x_1$. 

một số chia$a$đang hoạt động chính xác khi một số hợp lệ$y_1$chia hết cho$a$. Truy vấn cây phân đoạn tìm thấy một hoạt động$a$thỏa mãn$a>b$Và$x_1a/b \le n$. 

Từ$b\mid x_1$Và$a\mid y_1$,$$x_2=x_1\frac{a}{b}$$Và$$y_2=y_1\frac{b}{a}$$là số nguyên. 

Sự bất bình đẳng$a>b$đảm bảo$$x_2>x_1$$Và$$y_2<y_1.$$Sự bình đẳng$$x_1y_1=x_2y_2$$theo trực tiếp từ việc xây dựng. Mọi bộ tứ được in đều hợp lệ và mọi bộ tứ hợp lệ tương ứng với một số ước$b$và số chia tích cực$a$, do đó việc tìm kiếm không thể bỏ lỡ lời giải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def first_in_range(seg, size, l, r):
    if l > r:
        return -1

    def dfs(node, nl, nr):
        if nr < l or nl > r or seg[node] == 0:
            return -1
        if nl == nr:
            return nl
        mid = (nl + nr) // 2
        left = dfs(node * 2, nl, mid)
        if left != -1:
            return left
        return dfs(node * 2 + 1, mid + 1, nr)

    return dfs(1, 1, size)

def solve():
    n, m = map(int, input().split())
    l, r = map(int, input().split())

    mx = max(n, m)

    divisors = [[] for _ in range(mx + 1)]
    for d in range(1, mx + 1):
        for v in range(d, mx + 1, d):
            divisors[v].append(d)

    size = 1
    while size < m:
        size <<= 1

    seg = [0] * (2 * size)
    cnt = [0] * (m + 1)

    def activate(a):
        p = a + size - 1
        seg[p] = 1
        p //= 2
        while p:
            seg[p] = seg[p * 2] | seg[p * 2 + 1]
            p //= 2

    def deactivate(a):
        p = a + size - 1
        seg[p] = 0
        p //= 2
        while p:
            seg[p] = seg[p * 2] | seg[p * 2 + 1]
            p //= 2

    def add_y(y):
        for d in divisors[y]:
            cnt[d] += 1
            if cnt[d] == 1:
                activate(d)

    def remove_y(y):
        for d in divisors[y]:
            cnt[d] -= 1
            if cnt[d] == 0:
                deactivate(d)

    ans = [None] * (n + 1)

    curL = 1
    curR = 0

    for x in range(n, 0, -1):
        L = (l + x - 1) // x
        R = r // x

        if L < 1:
            L = 1
        if R > m:
            R = m

        if L > R:
            ans[x] = None
            continue

        while curR < R:
            curR += 1
            add_y(curR)

        while curR > R:
            remove_y(curR)
            curR -= 1

        while curL < L:
            remove_y(curL)
            curL += 1

        while curL > L:
            curL -= 1
            add_y(curL)

        found = None

        for b in divisors[x]:
            limit = (n * b) // x
            if limit <= b:
                continue

            a = first_in_range(seg, size, b + 1, min(limit, m))
            if a == -1:
                continue

            y1 = ((L + a - 1) // a) * a
            x2 = x * a // b
            y2 = y1 * b // a

            found = (x, y1, x2, y2)
            break

        ans[x] = found

    out = []
    for x in range(1, n + 1):
        if ans[x] is None:
            out.append("-1")
        else:
            out.append(" ".join(map(str, ans[x])))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Sàng chia là nền tảng của giải pháp. Nó cho phép mọi cập nhật của điểm cuối cửa sổ chỉ chạm vào các ước số của giá trị nhập hoặc rời. 

Hai con trỏ duy trì khoảng thời gian hợp lệ chính xác$y_1$các giá trị hiện tại$x$. Vì chúng tôi xử lý$x$từ$n$xuống$1$, cả hai điểm cuối của khoảng đều chỉ di chuyển về phía trước, vì vậy mọi giá trị của$y$được chèn và xóa nhiều nhất một lần. 

Cây phân đoạn chỉ lưu trữ xem số chia có hoạt động hay không. Truy vấn tìm kiếm giá trị hoạt động đầu tiên trong một phạm vi. Đó chính xác là hoạt động được yêu cầu bởi công thức tỷ lệ. 

Bước tái thiết rất dễ mắc sai lầm. Người được chọn$y_1$phải là bội số của$a$nằm bên trong khoảng hiện tại. Lấy bội số đầu tiên như vậy đảm bảo điều này. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8 20
91 100
```Vì$x_1=6$:$$L=\left\lceil\frac{91}{6}\right\rceil=16, \qquad R=\left\lfloor\frac{100}{6}\right\rfloor=16.$$| x | Khoảng thời gian | b | Đã chọn một | y1 | x2 | y2 | 
| --- | --- | --- | --- | --- | --- | --- | 
| 6 | [16,16] | 3 | 4 | 16 | 8 | 12 | 

Chúng tôi có được:$$6 \cdot 16 = 8 \cdot 12 = 96.$$Điều này thể hiện việc xây dựng tỷ lệ. Khoảng này chỉ chứa một giá trị nhưng vẫn tồn tại một hệ số hợp lệ. 

### Mẫu 2 

đầu vào:```
4 5
1 10
```Vì$x_1=1$:$$L=1,\quad R=5.$$| x | Khoảng thời gian | b | Đã chọn một | y1 | x2 | y2 | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [1,5] | 1 | 2 | 2 | 2 | 1 | 

Vì$x_1=2$: 

| x | Khoảng thời gian | b | Đã chọn một | y1 | x2 | y2 | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | [1,5] | 2 | 3 | 3 | 3 | 2 | 

Dấu vết cho thấy cách thuật toán tìm kiếm thông qua các ước số của$x$và chọn hoạt động đầu tiên$a$thỏa mãn giới hạn trên$x_2$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log^2 n)$| Cập nhật số chia cộng với truy vấn cây phân đoạn | 
| Không gian |$O(n+m)$| Danh sách chia, bộ đếm, cây phân đoạn | 

Tổng số chia có giá trị lên đến$2 \cdot 10^5$là về$N \log N$, có thể dễ dàng quản lý được. Các phép toán trên cây phân đoạn sẽ thêm một hệ số logarit khác. Giải pháp thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # real testing would call solve() and capture stdout
    return out.getvalue()

# Sample outputs are not unique for this problem,
# so exact-string assertions are inappropriate.
# Instead, a checker should verify validity.

# Minimum size
inp = """\
1 1
1 1
"""

# No solution possible because x2 > x1 cannot exist.

# Boundary interval with single product
inp2 = """\
2 2
4 4
"""

# Small dense range
inp3 = """\
4 5
1 10
"""

# Large boundary style test
inp4 = f"""\
200000 200000
1 40000000000
"""

# Interval producing many empty y-ranges
inp5 = """\
5 5
24 25
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=m=1$| Tất cả$-1$| Ví dụ nhỏ nhất có thể | 
| Khoảng thời gian sản phẩm duy nhất | hợp lệ hoặc$-1$mỗi$x$| Xử lý khoảng thời gian chặt chẽ | 
| Mẫu 2 | Tăng gấp bốn lần hợp lệ | Tính đúng đắn cơ bản | 
| Giới hạn tối đa | Kết thúc nhanh chóng | Hiệu suất | 
| Dải cao hẹp | Câu trả lời hỗn hợp | Xử lý khoảng trống | 

## Vỏ cạnh 

Hãy xem xét:```
3 5
20 20
```Vì$x_1=3$,$$L=\left\lceil\frac{20}{3}\right\rceil=7, \qquad R=\left\lfloor\frac{20}{3}\right\rfloor=6.$$Từ$L>R$, khoảng trống. Thuật toán in ngay lập tức$-1$. 

Bây giờ hãy xem xét:```
4 6
24 24
```Vì$x_1=4$, giá trị hợp lệ duy nhất là$y_1=6$. Thuật toán kích hoạt tất cả các ước số có trong khoảng$[6,6]$. Nó có thể chọn$b=2$Và$a=3$, mang lại$$x_2=6,$$vượt quá$n=4$. Sự ràng buộc$$a \le \frac{nb}{x_1}$$từ chối ứng viên này trước khi xây dựng. 

Cuối cùng:```
4 5
1 10
```Nếu thuật toán từng được chọn$a=b$, sau đó$$y_2=y_1.$$Phạm vi truy vấn bắt đầu tại$b+1$, nên điều này không thể xảy ra được. Sự bất bình đẳng nghiêm ngặt được thực thi một cách có hệ thống thay vì kiểm tra sau này.
