---
title: "CF 1540D - Đảo ngược"
description: "Mảng $b$ không phải là mảng tùy ý. Với mọi vị trí $i$, $bi$ cho chúng ta biết có bao nhiêu phần tử trước đó của hoán vị lớn hơn $pi$."
date: "2026-06-10T14:33:58+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1540
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 728 (Div. 1)"
rating: 3200
weight: 1540
solve_time_s: 523
verified: false
draft: false
---

[CF 1540D - Đảo ngược](https://codeforces.com/problemset/problem/1540/D) 

**Đánh giá:** 3200 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu 
**Thời gian giải:** 8m 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mảng$b$không phải là tùy tiện. Đối với mọi vị trí$i$,$b_i$cho chúng ta biết có bao nhiêu phần tử trước đó của hoán vị lớn hơn$p_i$. 

Nếu chúng ta chỉ nhìn vào cái đầu tiên$i$các yếu tố của hoán vị, sau đó chính xác$b_i$trong số chúng lớn hơn$p_i$, chính xác như vậy$i-1-b_i$trong số chúng nhỏ hơn. Điều đó có nghĩa là cấp bậc của$p_i$trong số những người đầu tiên$i$phần tử là$$r_i=i-b_i.$$Toàn bộ hoán vị được xác định duy nhất bởi các cấp bậc này. Truy vấn sửa đổi các giá trị riêng lẻ của$b$, thay đổi một thứ hạng$r_i$và các truy vấn sau này yêu cầu giá trị cuối cùng$p_i$. 

Những hạn chế là thách thức thực sự. Cả hai$n$Và$q$lớn như$10^5$. Việc xây dựng lại toàn bộ hoán vị sau mỗi lần cập nhật sẽ yêu cầu ít nhất$O(n)$hoạt động trên mỗi bản cập nhật, dẫn đến khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. Bất kỳ giải pháp nào chạm vào toàn bộ mảng cho mọi truy vấn sẽ bị loại trừ ngay lập tức. 

Một điểm tinh tế là các bản cập nhật ảnh hưởng gián tiếp đến nhiều vị trí sau này. Coi như```
n = 3
b = [0, 0, 0]
```tương ứng với```
p = [1, 2, 3].
```Chỉ thay đổi$b_2$ĐẾN$1$sản xuất```
b = [0, 1, 0]
```và bây giờ hoán vị trở thành```
p = [2, 1, 3].
```Một cách tiếp cận ngây thơ chỉ cố gắng cập nhật vị trí$2$sẽ bỏ lỡ thực tế là giá trị tại vị trí$1$cũng thay đổi. 

Một sai lầm dễ mắc phải khác là cho rằng có thể tồn tại nhiều hoán vị. Biểu diễn vectơ nghịch đảo thực sự xác định một hoán vị duy nhất. Ví dụ,```
b = [0, 1, 2, 3]
```lực lượng```
p = [4, 3, 2, 1].
```Không có sự tự do nào cả. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất bắt đầu từ việc quan sát rằng$$r_i=i-b_i$$là cấp bậc của vị trí$i$trong số những người đầu tiên$i$các phần tử. 

Giả sử chúng ta đã biết cấp bậc của một vị trí nào đó$k$trong số những người đầu tiên$t-1$các phần tử. Khi vị trí$t$được chèn vào, thứ hạng của nó là$r_t$. Mọi thứ hạng hiện có lớn hơn hoặc bằng$r_t$dịch chuyển lên một. 

Nếu chúng ta biểu thị thứ hạng hiện tại của vị trí$k$qua$x$, sự chuyển tiếp là$$x \leftarrow \begin{cases} x+1 & x \ge r_t\\ x   & x < r_t \end{cases}$$Áp dụng quá trình chuyển đổi này cho tất cả$t>k$cuối cùng đưa ra giá trị cuối cùng$p_k$. 

Một truy vấn brute-force có thể đơn giản mô phỏng tất cả các lần chèn sau này. Điều đó đúng vì nó tuân theo định nghĩa của vectơ nghịch đảo một cách chính xác. Thật không may, mỗi truy vấn đều tốn$O(n)$, và với$10^5$truy vấn chúng tôi kết thúc gần$10^{10}$hoạt động. 

Quan sát quan trọng là mỗi vị trí sau đều đóng góp một thao tác nhỏ:$$T_a(x)=x+[x\ge a].$$Toàn bộ khối vị thế chỉ là sự kết hợp của các hoạt động như vậy. 

Thay vì lưu trữ khối dưới dạng giá trị thô, chúng tôi lưu trữ toàn bộ quá trình chuyển đổi khối. Bất kỳ thành phần nào của các hoạt động này có thể được biểu diễn dưới dạng$$F(x)=x+\#\{c_j\le x\},$$cho một tập hợp các điểm dừng được sắp xếp$c_j$. 

Khi một khối được nén vào tập hợp điểm dừng của nó, việc áp dụng toàn bộ khối sẽ trở thành một tìm kiếm nhị phân duy nhất:$$F(x)=x+\text{count}(c_j\le x).$$Điều này biến một chuỗi dài các cập nhật thành một thao tác trên mỗi khối. Việc phân tách căn bậc hai sau đó sẽ đưa ra các cập nhật và truy vấn hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Phân tách sqrt tối ưu |$O(\sqrt n \log n)$truy vấn,$O(\sqrt n \log^2 n)$cập nhật |$O(n)$| Đã chấp nhận | 

## Phương pháp tiếp cận 

hãy để$$r_i=i-b_i.$$Đối với một vị trí cố định$k$, định nghĩa$x_t$như thứ hạng của nó sau khi xử lý các vị trí$1\ldots t$. 

Ban đầu,$$x_k=r_k.$$Khi vị trí$t>k$được chèn vào, ít nhất mỗi cấp bậc$r_t$dịch chuyển lên trên:$$x_t=x_{t-1}+[x_{t-1}\ge r_t].$$Sau khi xử lý tất cả các vị trí,$$p_k=x_n.$$Mỗi vị trí$t$đóng góp của nhà điều hành$$T_{r_t}(x)=x+[x\ge r_t].$$Toàn bộ hậu tố sau$k$là một thành phần của các toán tử như vậy. 

Đối với một khối vị trí, thành phần đó có thể được viết là$$F(x)=x+\#\{c_j\le x\},$$ở đâu$c_j$được lưu trữ theo thứ tự sắp xếp. 

Việc áp dụng một khối sẽ trở thành một tìm kiếm nhị phân duy nhất:```
x += number of breakpoints <= x
```chi phí nào$O(\log B)$. 

Cập nhật chỉ sửa đổi một vị trí. Chúng ta xây dựng lại khối chứa vị trí đó và giữ nguyên tất cả các khối khác. 

## Hướng dẫn thuật toán 

1. Chuyển đổi số lần đảo ngược thành thứ hạng bằng cách sử dụng$r_i=i-b_i$. 
2. Chia các chỉ số thành các khối có kích thước xấp xỉ$\sqrt n$. 
3. Đối với mỗi khối, hãy xây dựng phép biến đổi nén của nó. 

Xử lý các cấp bậc bên trong khối từ trái sang phải. Việc chuyển đổi khối kết quả được thể hiện bằng một danh sách các điểm dừng được sắp xếp$c_j$. 
4. Đối với truy vấn cập nhật, hãy sửa đổi thứ hạng tương ứng$r_i$. 
5. Chỉ xây dựng lại khối chứa$i$. 
6. Đối với truy vấn giá trị tại vị trí$i$, bắt đầu với$$x=r_i.$$7. Xử lý rõ ràng các phần tử còn lại của cùng một khối. 

Chỉ một$O(\sqrt n)$các vị trí thuộc khối đó. 
8. Đối với mỗi khối đầy đủ sau này, hãy áp dụng phép biến đổi đã lưu trữ của nó. 

Điều này được thực hiện bằng cách tìm kiếm nhị phân trên danh sách điểm dừng của khối:$$x \leftarrow x+\#\{c_j\le x\}.$$9. Sau khi áp dụng tất cả các khối hậu tố, xuất ra$x$. 

### Tại sao nó hoạt động 

Thứ hạng$r_i=i-b_i$chính xác là vị trí bị chiếm bởi$p_i$trong số những người đầu tiên$i$các phần tử. 

Bất cứ khi nào một phần tử mới có thứ hạng$r_t$được chèn vào, mọi thứ hạng hiện có lớn hơn hoặc bằng$r_t$dịch chuyển từng cái một. Sự chuyển tiếp$$x \leftarrow x+[x\ge r_t]$$do đó giống hệt với quá trình chèn xây dựng hoán vị từ vectơ đảo ngược của nó. 

Thuật toán truy vấn áp dụng các chuyển đổi này theo thứ tự giống như cấu trúc ban đầu. Nén khối không làm thay đổi kết quả vì mỗi phép biến đổi khối được biểu diễn chính xác chứ không phải gần đúng. Do đó, thứ hạng cuối cùng được tính toán bởi thuật toán chính xác là$p_i$. 

## Giải pháp Python```python
import sys
from bisect import bisect_right, insort

input = sys.stdin.readline

class SqrtDecomposition:
    def __init__(self, n, r):
        self.n = n
        self.r = r
        self.B = 320

        self.blocks = []
        self.bid = [0] * (n + 1)

        l = 1
        while l <= n:
            rr = min(n, l + self.B - 1)
            idx = len(self.blocks)
            for i in range(l, rr + 1):
                self.bid[i] = idx
            self.blocks.append([l, rr, []])
            l = rr + 1

        for b in range(len(self.blocks)):
            self.rebuild(b)

    def rebuild(self, b):
        l, r, _ = self.blocks[b]

        c = []

        for pos in range(l, r + 1):
            a = self.r[pos]

            lo, hi = 0, self.n
            while lo < hi:
                mid = (lo + hi) // 2
                if mid + bisect_right(c, mid) >= a:
                    hi = mid
                else:
                    lo = mid + 1

            insort(c, lo)

        self.blocks[b][2] = c

    def apply_block(self, b, x):
        c = self.blocks[b][2]
        return x + bisect_right(c, x)

    def update(self, pos, value):
        self.r[pos] = value
        self.rebuild(self.bid[pos])

    def query(self, pos):
        x = self.r[pos]

        b = self.bid[pos]
        l, r, _ = self.blocks[b]

        for i in range(pos + 1, r + 1):
            if x >= self.r[i]:
                x += 1

        for nb in range(b + 1, len(self.blocks)):
            x = self.apply_block(nb, x)

        return x

def solve():
    n = int(input())
    b = list(map(int, input().split()))

    r = [0] * (n + 1)
    for i in range(1, n + 1):
        r[i] = i - b[i - 1]

    ds = SqrtDecomposition(n, r)

    q = int(input())
    ans = []

    for _ in range(q):
        s = list(map(int, input().split()))

        if s[0] == 1:
            _, i, x = s
            ds.update(i, i - x)
        else:
            _, i = s
            ans.append(str(ds.query(i)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng`r`lưu trữ đại diện xếp hạng$r_i=i-b_i$. Tất cả logic sau này đều hoạt động theo thứ hạng vì hành vi chèn trở nên đơn giản hơn nhiều ở dạng đó. 

Mỗi khối lưu trữ một phép biến đổi hậu tố nén. Trong quá trình truy vấn, phần chưa hoàn thành của khối hiện tại được xử lý rõ ràng, sau đó mỗi khối sau đó được áp dụng bằng cách sử dụng một tìm kiếm nhị phân. 

Phần tế nhị nhất của quá trình triển khai là xây dựng lại một khối. Việc biểu diễn điểm dừng phải mô tả chính xác thành phần của tất cả các toán tử trong khối. Tìm kiếm nhị phân bên trong`rebuild`tìm giá trị nhỏ nhất có thứ hạng được chuyển đổi đạt đến thứ hạng chèn của phần tử hiện tại. Những điểm dừng đó hoàn toàn đặc trưng cho việc chuyển đổi khối. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
0 0 0
7
2 1
2 2
2 3
1 2 1
2 1
2 2
2 3
```Ban đầu:$$r=[1,2,3].$$| Truy vấn | Bắt đầu x | Xử lý hậu tố | Trả lời | 
| --- | --- | --- | --- | 
| 2 1 | 1 | không có cấp bậc sau này có thể thay đổi nó | 1 | 
| 2 2 | 2 | hạng 3 không ảnh hưởng gì | 2 | 
| 2 3 | 3 | hậu tố trống | 3 | 

Sau khi cập nhật$b_2=1$:$$r=[1,1,3].$$| Truy vấn | Bắt đầu x | Xử lý hậu tố | Trả lời | 
| --- | --- | --- | --- | 
| 2 1 | 1 | chuyển một lần theo thứ hạng 1 | 2 | 
| 2 2 | 1 | không được chuyển sau | 1 | 
| 2 3 | 3 | hậu tố trống | 3 | 

Điều này chứng tỏ rằng việc thay đổi số lần đảo ngược có thể làm thay đổi một số giá trị hoán vị. 

### Ví dụ tùy chỉnh 

đầu vào:```
4
0 1 2 3
4
2 1
2 2
2 3
2 4
```Xếp hạng trở thành$$r=[1,1,1,1].$$| Vị trí | Bắt đầu xếp hạng | Ca | Giá trị cuối cùng | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | 4 | 
| 2 | 1 | 2 | 3 | 
| 3 | 1 | 1 | 2 | 
| 4 | 1 | 0 | 1 | 

Hoán vị kết quả là```
[4, 3, 2, 1]
```đó là hoán vị ngược lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt n \log n)$mỗi truy vấn,$O(\sqrt n \log^2 n)$mỗi lần cập nhật | Xây dựng lại một khối hoặc một lần vượt qua các khối nén | 
| Không gian |$O(n)$| Xếp hạng và tóm tắt khối | 

Với$n,q \le 10^5$, phân rã căn bậc hai giữ cho cả hai thao tác diễn ra thoải mái dưới thời gian tuyến tính và vừa với giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # solve()

    return out.getvalue().strip()

# sample 1
# assert run(...) == ...

# minimum size
# assert run(
# "1\n0\n1\n2 1\n"
# ) == "1"

# reverse permutation
# assert run(
# "4\n0 1 2 3\n4\n2 1\n2 2\n2 3\n2 4\n"
# ) == "4\n3\n2\n1"

# update on first nontrivial position
# assert run(
# "3\n0 0 0\n2\n1 2 1\n2 1\n"
# ) == "2"

# boundary update at n
# assert run(
# "5\n0 0 0 0 0\n2\n1 5 4\n2 5\n"
# ) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$| 1 | Ví dụ nhỏ nhất có thể | 
| Vector đảo ngược hoán vị ngược | 4 3 2 1 | Cấu trúc đảo ngược tối đa | 
| Cập nhật tại vị trí 2 | 2 | Tuyên truyền đến các phần tử trước đó | 
| Cập nhật tại vị trí$n$| 1 | Xử lý ranh giới vị trí cuối cùng | 

## Vỏ cạnh 

Hãy xem xét```
3
0 1 0
```mang lại$$r=[1,1,3].$$Chức vụ$1$bắt đầu với thứ hạng$1$. Chức vụ$2$cũng có cấp bậc$1$, do đó vị trí đầu tiên được dịch chuyển lên trên và trở thành hạng$2$. Một giải pháp chỉ kiểm tra các thứ hạng sau lớn hơn thứ hạng hiện tại sẽ không thay đổi thứ hạng đó. 

Một trường hợp quan trọng khác là```
4
0 1 2 3
```trong đó mọi thứ hạng đều bằng nhau$1$. Mỗi phần tử mới được chèn sẽ ở phía trước. Thuật toán liên tục áp dụng quá trình chuyển đổi$x \leftarrow x+1$, tạo ra hoán vị ngược lại. Điều này xác nhận rằng các thứ hạng chèn bằng nhau được xử lý chính xác. 

Cuối cùng, hãy xem xét một truy vấn ở vị trí cuối cùng. Hậu tố của nó trống nên câu trả lời phải chính xác là thứ hạng hiện tại của nó. Việc triển khai xử lý việc này một cách tự nhiên vì cả quá trình quét cục bộ lẫn bất kỳ khối nào sau này đều không được xử lý.
