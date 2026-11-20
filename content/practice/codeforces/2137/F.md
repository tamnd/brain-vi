---
title: "CF 2137F - Tiền tố bất biến tối đa"
description: "Đối với bất kỳ mảng $x$ nào, chuỗi tiền tố cực đại của nó được xác định hoàn toàn. Tại vị trí $i$, tiền tố tối đa là $$max(x1,x2,dots,xi).$$ Chúng ta được phép xây dựng một mảng $z$ khác có cực đại tiền tố giống hệt với mảng $x$."
date: "2026-06-08T02:32:07+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2137
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1047 (Div. 3)"
rating: 1900
weight: 2137
solve_time_s: 102
verified: true
draft: false
---

[CF 2137F - Tiền tố bất biến tối đa](https://codeforces.com/problemset/problem/2137/F) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, tổ hợp, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với bất kỳ mảng nào$x$, chuỗi tiền tố cực đại của nó được xác định hoàn toàn. Tại vị trí$i$, tiền tố tối đa là$$\max(x_1,x_2,\dots,x_i).$$Chúng tôi được phép xây dựng một mảng khác$z$có cực đại tiền tố giống hệt với cực đại của$x$. Trong số tất cả các mảng như vậy$z$, chúng tôi muốn tối đa hóa số lượng vị trí trong đó$z_i=y_i$. Mức tối đa đó được gọi là$f(x,y)$. 

Nhiệm vụ thực tế lớn hơn nhiều. Chúng tôi được cung cấp hai mảng$a$Và$b$chiều dài$n$. Đối với mỗi mảng con$[l,r]$, chúng tôi hình thành$$x=a_l,a_{l+1},\dots,a_r,$$Và$$y=b_l,b_{l+1},\dots,b_r.$$Chúng ta phải tổng hợp$f(x,y)$trên tất cả các mảng con. 

Thử thách đầu tiên là hiểu những giá trị nào được phép ở một vị trí trong khi vẫn duy trì cực đại tiền tố. Thử thách thứ hai là tránh những điều hiển nhiên$O(n^2)$liệt kê các mảng con. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2\cdot 10^5$. Một thuật toán bậc hai sẽ thực hiện khoảng$4\cdot 10^{10}$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Chúng ta cần một cái gì đó gần gũi$O(n \log n)$. 

Trường hợp cạnh tinh tế xuất hiện khi giá trị bằng giá trị tối đa của tiền tố hiện tại đã xảy ra trước đó. 

Coi như:```
a = [2, 2]
b = [0, 1]
```Đối với vị trí thứ hai chúng ta có thể đặt$z_2=1$, bởi vì phần tử đầu tiên đã giữ tiền tố tối đa bằng$2$. Một giải pháp bất cẩn chỉ tìm kiếm một giá trị lớn hơn trước đó sẽ từ chối trường hợp này một cách không chính xác. 

Một trường hợp quan trọng khác là khi$b_i$lớn hơn$a_i$.```
a = [5, 1]
b = [0, 4]
```Vị trí thứ hai có thể trở thành$4$chỉ vì có một giá trị trước đó$5$giữ cho tiền tố tối đa không thay đổi. Nếu không có giá trị trước đó, việc tăng$z_i$sẽ thay đổi chuỗi tiền tố tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Liệt kê mọi mảng con$[l,r]$. Đối với mảng con đó, hãy xác định số lượng vị trí tối đa có thể khớp. Ngay cả khi bằng cách nào đó chúng ta xử lý một mảng con theo thời gian tuyến tính thì vẫn có sẵn$O(n^2)$mảng con, dẫn đến$O(n^3)$công việc. 

Lý do vũ lực hoạt động về mặt khái niệm là$f(x,y)$chỉ là tổng của các quyết định độc lập ở mỗi vị trí. Cho dù vị trí$i$có thể được làm bằng$y_i$chỉ phụ thuộc vào cấu trúc tiền tố tối đa trước vị trí đó. 

Quan sát này là chìa khóa. 

Giả sử chúng ta kiểm tra một vị trí$i$bên trong một số mảng con. 

Nếu như$a_i=b_i$, thì chúng ta đã khớp rồi mà không thay đổi gì cả. 

Nếu như$a_i\neq b_i$, khi nào chúng ta có thể ép buộc$z_i=b_i$? 

Cho phép$M$là tiền tố tối đa trước vị trí$i$. 

Nếu như$M\ge \max(a_i,b_i)$, thì chúng ta có quyền tự do lựa chọn$z_i=b_i$. Tiền tố tối đa trước đó vẫn chiếm ưu thế, do đó chuỗi tiền tố tối đa không thay đổi. 

Nếu không đạt được giá trị sớm hơn$\max(a_i,b_i)$, sau đó định vị$i$chính nó chịu trách nhiệm tạo ra mức tối đa mới và giá trị của nó bị hạn chế. Trong tình huống đó$z_i=b_i$là không thể. 

Vì vậy, một vị trí đóng góp chính xác khi một trong những điều sau đây được giữ: 

-$a_i=b_i$. 
- Tồn tại phần tử đứng trước có giá trị ít nhất bằng$\max(a_i,b_i)$. 

Bây giờ vấn đề trở thành việc đếm, với mọi chỉ số$i$, có bao nhiêu mảng con thỏa mãn điều kiện này. 

Cho phép$$T_i=\max(a_i,b_i).$$Định nghĩa$p_i$là chỉ mục ngoài cùng bên phải trước đây$i$giá trị của nó ít nhất là$T_i$. 

Nếu không có chỉ mục đó tồn tại,$p_i=0$. 

Đối với bất kỳ mảng con nào chứa$i$, chức vụ$i$đóng góp nếu điểm cuối bên trái nằm ở hoặc trước$p_i$. Sau đó, phần tử đủ điều kiện trước đó vẫn nằm trong mảng con. 

Vì vậy sự đóng góp của vị trí$i$là:

-$i(n-i+1)$nếu như$a_i=b_i$. 
-$p_i(n-i+1)$nếu không thì. 

Nhiệm vụ duy nhất còn lại là tính toán mọi$p_i$một cách hiệu quả. 

Chúng tôi xử lý mảng từ trái sang phải. Đối với mỗi giá trị, chúng tôi lưu trữ vị trí mới nhất nơi nó xuất hiện. Để tìm$p_i$, chúng ta cần vị trí được lưu trữ tối đa trong số tất cả các giá trị ít nhất$T_i$. 

Vì tất cả các giá trị lớn nhất$2n$, cây phân đoạn trên tọa độ giá trị hỗ trợ: 

- cập nhật điểm: vị trí mới nhất của một giá trị, 
- phạm vi truy vấn tối đa trên$[T_i,2n]$. 

Điều này mang lại$O(\log n)$mỗi chỉ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(n\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý mảng từ trái qua phải. 
2. Đối với chức vụ$i$, tính toán$$T_i=\max(a_i,b_i).$$3. Truy vấn cây phân đoạn trên dải giá trị$[T_i,2n]$. 

Kết quả là chỉ mục trước ngoài cùng bên phải có giá trị ít nhất là$T_i$. Gọi nó$p$. 
4. Nếu$a_i=b_i$, thêm vào$$i\cdot(n-i+1)$$để trả lời. 

Mỗi mảng con chứa$i$đóng góp ở vị trí này. 
5. Nếu không thì thêm$$p\cdot(n-i+1).$$Bất kỳ vị trí bắt đầu nào$l\le p$giữ một phần tử đủ điều kiện trước đó bên trong mảng con và mọi vị trí kết thúc$r\ge i$là hợp lệ. 
6. Chèn giá trị hiện tại$a_i$vào cây phân đoạn bằng cách cập nhật tọa độ$a_i$với vị trí$i$. 
7. Tiếp tục cho đến khi tất cả các vị trí được xử lý. 

### Tại sao nó hoạt động 

Đối với một mảng con cố định và một vị trí cố định, điều quan trọng duy nhất là liệu phần tử trước đó bên trong mảng con đó có đạt tới hay không$\max(a_i,b_i)$. 

Nếu phần tử như vậy tồn tại, tiền tố tối đa trước$i$đã đủ lớn rồi. Chúng ta có thể tự do lựa chọn$z_i=b_i$mà không ảnh hưởng đến bất kỳ tiền tố tối đa nào. 

Nếu không có phần tử nào như vậy tồn tại thì vị trí$i$chính nó xác định mức tối đa tiền tố. Thay đổi nó thành$b_i\neq a_i$sẽ thay đổi chuỗi tiền tố tối đa, điều này bị cấm. 

Như vậy mỗi vị trí đều đóng góp độc lập. Số lượng điểm cuối bên trái hợp lệ chính xác là$p_i$và số lượng điểm cuối bên phải hợp lệ là$n-i+1$. Tổng hợp những đóng góp này trên tất cả các vị trí sẽ tính chính xác tổng số được yêu cầu. 

Cây phân đoạn tính toán mọi$p_i$chính xác vì nó luôn lưu trữ sự xuất hiện mới nhất của mọi giá trị giữa các vị trí trước đó một cách nghiêm ngặt.$i$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.seg = [0] * (4 * n)

    def update(self, idx, val, node, l, r):
        if l == r:
            self.seg[node] = max(self.seg[node], val)
            return

        mid = (l + r) >> 1
        if idx <= mid:
            self.update(idx, val, node * 2, l, mid)
        else:
            self.update(idx, val, node * 2 + 1, mid + 1, r)

        self.seg[node] = max(
            self.seg[node * 2],
            self.seg[node * 2 + 1]
        )

    def query(self, ql, qr, node, l, r):
        if ql <= l and r <= qr:
            return self.seg[node]

        mid = (l + r) >> 1
        res = 0

        if ql <= mid:
            res = max(res, self.query(ql, qr, node * 2, l, mid))
        if qr > mid:
            res = max(res, self.query(ql, qr, node * 2 + 1, mid + 1, r))

        return res

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        mx = 2 * n
        seg = SegTree(mx)

        ans = 0

        for i in range(1, n + 1):
            ai = a[i - 1]
            bi = b[i - 1]

            if ai == bi:
                ans += i * (n - i + 1)
            else:
                threshold = max(ai, bi)
                p = seg.query(threshold, mx, 1, 1, mx)
                ans += p * (n - i + 1)

            seg.update(ai, i, 1, 1, mx)

        print(ans)

solve()
```Cây phân đoạn được lập chỉ mục theo giá trị chứ không phải vị trí. Tại giá trị$v$, chúng tôi lưu trữ chỉ mục mới nhất ở đâu$a_j=v$đã xuất hiện cho đến nay. 

Truy vấn kết thúc$[T_i,2n]$trả về vị trí lớn nhất trong số tất cả các giá trị trước đó ít nhất$T_i$. Đó chính xác là$p_i$. 

Một lỗi phổ biến là sử dụng lần xuất hiện đầu tiên thay vì lần xuất hiện mới nhất. Chúng ta cần chỉ mục đủ điều kiện ngoài cùng bên phải vì mọi vị trí bắt đầu cho đến chỉ mục đó đều hoạt động, đưa ra chính xác$p_i$sự lựa chọn. 

Một lỗi dễ mắc khác là quên rằng các giá trị bị giới hạn bởi$2n$, không phải bởi$n$. Cây phân đoạn phải bao gồm đầy đủ phạm vi giá trị. 

Số nguyên Python tự động xử lý kích thước câu trả lời. Tổng số mảng con đóng góp có thể đạt tới khoảng$n^3$, vì vậy số nguyên 32 bit sẽ tràn trong nhiều ngôn ngữ. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
a = [5, 3, 1]
b = [4, 2, 1]
```| tôi | một [tôi] | b[i] | T=max(a,b) | p | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 4 | 5 | 0 | 0 | 
| 2 | 3 | 2 | 3 | 1 | 2 | 
| 3 | 1 | 1 | - | - | 3 | 

Tổng cộng:$$0+2+3=5.$$Điều này phù hợp với câu trả lời mẫu. 

Vị trí thứ hai góp phần vì giá trị$5$trước khi nó duy trì tiền tố tối đa, cho phép chúng ta thay đổi giá trị hiện tại. 

### Ví dụ 2```
a = [2, 2]
b = [1, 1]
```| tôi | một [tôi] | b[i] | T | p | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 0 | 0 | 
| 2 | 2 | 1 | 2 | 1 | 1 | 

Tổng cộng:$$1.$$Vị trí thứ hai chứng minh tại sao "lớn hơn hoặc bằng" là điều kiện đúng. Giá trị trước đó bằng$2$, không lớn hơn nhưng vẫn bảo toàn tiền tố tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\log n)$| Một truy vấn cây phân đoạn và một bản cập nhật cho mỗi vị trí | 
| Không gian |$O(n)$| Phân đoạn cây theo giá trị$1 \ldots 2n$| 

Vì tổng cộng$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2\cdot 10^5$, tổng thời gian chạy là khoảng$2\cdot 10^5 \log(2\cdot 10^5)$, thoải mái phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    sys.stdin = input_data
    sys.stdout = output_data

    class SegTree:
        def __init__(self, n):
            self.n = n
            self.seg = [0] * (4 * n)

        def update(self, idx, val, node, l, r):
            if l == r:
                self.seg[node] = max(self.seg[node], val)
                return
            mid = (l + r) >> 1
            if idx <= mid:
                self.update(idx, val, node * 2, l, mid)
            else:
                self.update(idx, val, node * 2 + 1, mid + 1, r)
            self.seg[node] = max(self.seg[node * 2],
                                 self.seg[node * 2 + 1])

        def query(self, ql, qr, node, l, r):
            if ql <= l and r <= qr:
                return self.seg[node]
            mid = (l + r) >> 1
            res = 0
            if ql <= mid:
                res = max(res, self.query(ql, qr, node * 2, l, mid))
            if qr > mid:
                res = max(res, self.query(ql, qr, node * 2 + 1, mid + 1, r))
            return res

    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        seg = SegTree(2 * n)
        ans = 0

        for i in range(1, n + 1):
            ai = a[i - 1]
            bi = b[i - 1]

            if ai == bi:
                ans += i * (n - i + 1)
            else:
                p = seg.query(max(ai, bi), 2 * n, 1, 1, 2 * n)
                ans += p * (n - i + 1)

            seg.update(ai, i, 1, 1, 2 * n)

        print(ans)

    return output_data.getvalue()

# provided sample
assert run(
"""6
3
5 3 1
4 2 1
5
1 2 3 4 5
1 2 3 4 5
6
7 1 12 10 5 8
9 2 4 3 6 5
1
1
2
6
5 1 2 6 3 4
3 1 6 5 2 4
2
2 2
1 1
"""
) == """5
35
26
0
24
1
"""

# minimum size
assert run(
"""1
1
1
1
"""
) == """1
"""

# minimum size, unequal
assert run(
"""1
1
1
2
"""
) == """0
"""

# equal values earlier should count
assert run(
"""1
2
2 2
1 1
"""
) == """1
"""

# all equal arrays
assert run(
"""1
3
4 4 4
4 4 4
"""
) == """10
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$, giá trị bằng nhau | 1 | Trường hợp hợp lệ nhỏ nhất | 
|$n=1$, các giá trị khác nhau | 0 | Không có phần tử trước đó tồn tại | 
|`[2,2]`Và`[1,1]`| 1 | Bình đẳng với mức tối đa trước đó là đủ | 
| Tất cả các giá trị bằng nhau | 10 | Mọi vị trí đều đóng góp trong mọi mảng con chứa | 

## Vỏ cạnh 

Hãy xem xét:```
a = [2, 2]
b = [1, 1]
```Ở vị trí thứ hai, ngưỡng là$2$. Truy vấn cây phân đoạn tìm sự xuất hiện trước đó tại chỉ mục$1$, Vì thế$p=1$. Thuật toán đếm một mảng con hợp lệ. 

Bên trong mảng con đó, giá trị đầu tiên đã giữ tiền tố tối đa bằng$2$, cho phép giá trị thứ hai trở thành$1$. Đây là lý do tại sao điều kiện phải là "ít nhất" thay vì "hoàn toàn lớn hơn". 

Bây giờ hãy xem xét:```
a = [5, 1]
b = [0, 4]
```Đối với vị trí thứ hai, ngưỡng là$4$. Truy vấn tìm chỉ mục$1$, từ$5 \ge 4$. Như vậy$p=1$, và vị trí đóng góp. 

Giá trị trước đó$5$duy trì tiền tố tối đa trong khi giá trị hiện tại tăng từ$1$ĐẾN$4$. 

Cuối cùng hãy xem xét:```
a = [3, 1]
b = [0, 5]
```Ngưỡng ở vị trí thứ hai là$5$. Không đạt đến giá trị sớm hơn$5$, do đó truy vấn trả về$0$. Thuật toán không đóng góp gì. 

Điều đó đúng. Đặt giá trị thứ hai thành$5$sẽ tạo mức tối đa tiền tố mới, thay đổi chuỗi tối đa tiền tố được yêu cầu.
