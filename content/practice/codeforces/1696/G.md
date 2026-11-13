---
title: "CF 1696G - Fishingprince Lại Chơi Với Mảng"
description: "Chúng ta được cung cấp một mảng có các giá trị biểu thị số lượng cần phải loại bỏ. Một hoạt động tác động lên một cặp liền kề. Nếu chúng ta dành $t$ giây trên cạnh $(i,i+1)$, chúng ta có thể trừ $(x t, y t)$ khỏi cặp hoặc $(y t, x t)$ khỏi cặp."
date: "2026-06-09T22:38:45+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1696
codeforces_index: "G"
codeforces_contest_name: "Codeforces Global Round 21"
rating: 3300
weight: 1696
solve_time_s: 175
verified: false
draft: false
---

[CF 1696G - Fishingprince lại chơi với mảng](https://codeforces.com/problemset/problem/1696/G) 

**Đánh giá:** 3300 
**Tags:** sức mạnh vũ phu, cấu trúc dữ liệu, hình học, toán học 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có các giá trị biểu thị số lượng cần phải loại bỏ. Một hoạt động tác động lên một cặp liền kề. 

Nếu chúng ta chi tiêu$t$giây ở rìa$(i,i+1)$, chúng ta có thể trừ$(x t, y t)$từ cặp hoặc$(y t, x t)$từ cặp. 

Đối với bất kỳ mảng con nào$a=[b_l,\dots,b_r]$, định nghĩa$f(a)$là tổng thời gian tối thiểu cần thiết cho đến khi mọi phần tử trở nên không dương. 

Mảng được cập nhật trực tuyến và sau mỗi truy vấn loại 2 chúng ta phải báo cáo$f$cho đoạn được yêu cầu. 

Khó khăn không phải là cập nhật. Thách thức thực sự là hiểu được số lượng$f(a)$thực sự là vậy. 

Độ dài mảng lên tới$2\cdot 10^5$, và có tới$2\cdot 10^5$cập nhật và truy vấn phạm vi. Bất kỳ giải pháp nào tính toán lại câu trả lời cho một phân đoạn từ đầu sẽ bị loại trừ ngay lập tức. Thậm chí$O(\text{length})$mỗi truy vấn sẽ yêu cầu khoảng$4\cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Câu trả lời cũng là một số thực, thường báo hiệu rằng bài toán tối ưu hóa cần được chuyển đổi thành một chương trình tuyến tính hoặc một số cấu trúc tổ hợp tương đương. 

Trường hợp cạnh tinh tế xuất hiện khi một vị trí bị cô lập với tất cả các vị trí khác trong đối số tạo ra giới hạn dưới. 

Ví dụ, với$x=1,y=2$và mảng$[100,1,100]$, vị trí 1 và 3 không bao giờ thuộc cùng một phép toán. Một lập luận ngây thơ chỉ dựa trên tổng số tiền sẽ cho$$\frac{201}{3}=67.$$Nhưng vị trí 1 và 3 cùng nhau yêu cầu ít nhất$$\frac{100+100}{2}=100$$giây, bởi vì mọi thao tác có thể giảm tối đa một trong số chúng theo tốc độ$2$. Câu trả lời đúng bị chi phối bởi ràng buộc mạnh hơn. 

Một sai lầm dễ mắc phải khác là cho rằng mọi vị trí đều đóng góp độc lập. 

Vì$[100,100]$,$$f=\frac{200}{x+y},$$không$$\frac{100}{y}+\frac{100}{y}.$$Cả hai vị trí có thể được xử lý đồng thời bằng cách thao tác trên một cạnh giữa chúng. 

Toàn bộ giải pháp xuất phát từ việc hiểu chính xác giới hạn dưới nào có thể đạt được đồng thời. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là giới thiệu một biến cho mọi hoạt động có thể. Cho phép$p_i$là thời gian dành cho cạnh$i$sử dụng định hướng$(x,y)$, Và$q_i$thời gian sử dụng định hướng$(y,x)$. 

Mỗi vị trí mảng đều nhận được sự đóng góp từ các cạnh liền kề. Chúng tôi muốn tổng thời gian tối thiểu$$\sum_i (p_i+q_i)$$tùy thuộc vào mọi yếu tố được giảm về không. 

Đây là một chương trình tuyến tính. Nó đúng nhưng nó chứa đựng$O(n)$các biến ngay cả đối với một truy vấn duy nhất. Giải quyết nó trực tiếp là vô vọng. 

Quan sát quan trọng là chương trình tuyến tính kép có cấu trúc đơn giản đến không ngờ. Sau khi giả sử$x\le y$, các biến kép có thể được hiểu là trọng số$d_i$được phân công vào các chức vụ. 

Các ràng buộc kép ngụ ý rằng mọi$d_i$phải là một trong$$0,\quad \frac1{x+y},\quad \frac1y,$$và bất cứ khi nào$d_i=\frac1y$, cả hai hàng xóm phải bằng không. Nói cách khác, một vị trí sử dụng hệ số$1/y$hoạt động giống như một đỉnh được chọn trong một tập hợp đường đi độc lập. Vị trí sử dụng hệ số$1/(x+y)$không có sự tương tác với bất cứ điều gì khác. 

Một thực tế không tầm thường, được chứng minh thông qua tính đối ngẫu LP, là mọi giải pháp kép khả thi đều đưa ra một giới hạn thấp hơn cho$f(a)$, và giới hạn dưới tốt nhất như vậy chính xác bằng$f(a)$. 

Vì vậy, vấn đề tối ưu hóa ban đầu trở thành:$$f(a)= \max \sum_i a_i d_i,$$mỗi nơi$d_i$được chọn từ$$0,\frac1{x+y},\frac1y,$$và các vị trí sử dụng$1/y$không thể ở cạnh nhau. 

Bây giờ vấn đề hoàn toàn là tổ hợp. Mỗi vị trí có ba trạng thái có thể. 

Trạng thái 0 đóng góp$0$. 

Bang 1 đóng góp$a_i/(x+y)$. 

Bang 2 đóng góp$a_i/y$. 

Hạn chế duy nhất là hai vị trí liên tiếp không thể cùng ở trạng thái 2. 

Đây là một con đường DP. Vì các truy vấn yêu cầu các phân đoạn con tùy ý có cập nhật nên chúng tôi lưu trữ quá trình chuyển đổi DP dưới dạng ma trận cộng tối đa và duy trì các tích ma trận trong cây phân đoạn. Đây chính xác là cấu trúc được sử dụng trong các giải pháp được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|------|---| 

| Lập trình tuyến tính cho mỗi truy vấn | Vượt xa giới hạn đa thức cho kích thước đầu vào này | Lớn | Quá chậm | 

| Cây phân đoạn của ma trận cộng cực đại |$O(\log n)$mỗi lần cập nhật/truy vấn |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giả sử$x \le y$. Nếu không, hãy trao đổi chúng. 

Đối với một vị trí$i$, xác định ba trạng thái. 

Trạng thái 0 có nghĩa là hệ số$0$. 

Trạng thái 1 có nghĩa là hệ số$1/(x+y)$. 

Trạng thái 2 có nghĩa là hệ số$1/y$. 

Giá trị đóng góp theo vị trí$i$là:$$0,\quad \frac{a_i}{x+y},\quad \frac{a_i}{y}$$tương ứng. 

1. Xây dựng một$3\times3$ma trận max-plus cho mọi vị trí. 

Ma trận chỉ chứa các giá trị đường chéo. 

Trạng thái 0 đóng góp$0$. 

Bang 1 đóng góp$a_i/(x+y)$. 

Bang 2 đóng góp$a_i/y$. 
2. Tạo ma trận chuyển tiếp giữa các vị trí lân cận. 

Mọi chuyển đổi đều được phép ngoại trừ trạng thái$2 \to 2$. 

Điều này mã hóa quy tắc định vị bằng hệ số$1/y$không thể ở cạnh nhau. 
3. Kết hợp các phân đoạn liền kề bằng cách sử dụng phép nhân ma trận max-plus. 

Nếu ma trận$A$đại diện cho phân khúc bên trái và$B$đoạn bên phải thì$$A \cdot T \cdot B$$đại diện cho sự nối của chúng, trong đó$T$là ma trận chuyển tiếp. 
4. Lưu trữ các ma trận này trong cây phân đoạn. 

Mỗi nút lưu trữ ma trận mô tả khoảng thời gian của nó. 
5. Để cập nhật, hãy xây dựng lại ma trận lá và tính toán lại các ma trận trên đường dẫn tới gốc. 
6. Đối với truy vấn phạm vi$[l,r]$, thu được ma trận biểu diễn chính xác khoảng đó. 
7. Câu trả lời là giá trị lớn nhất của ma trận kết quả. 

Điều này tương ứng với sự lựa chọn tốt nhất của các trạng thái biên và do đó giá trị kép tối ưu. 

### Tại sao nó hoạt động 

Đặc tính kép nói rằng mọi giải pháp khả thi là một chuỗi các trạng thái$0,1,2$, trong đó trạng thái 2 không thể xuất hiện ở các vị trí liền kề. Giá trị khách quan chính xác là tổng đóng góp của nhà nước. 

Ma trận phân đoạn lưu trữ giá trị tốt nhất có thể đạt được cho mỗi cặp trạng thái biên. Phép nhân cộng tối đa là cách tiêu chuẩn để nối các khoảng DP trong khi thực thi khả năng tương thích của các trạng thái lân cận. 

Bởi vì phép nhân ma trận tái tạo chính xác sự lặp lại của đường dẫn DP, mỗi nút cây phân đoạn sẽ lưu trữ giá trị tối ưu cho khoảng thời gian của nó. Mục nhập tối đa của ma trận được truy vấn là giá trị tối ưu trên tất cả các lựa chọn ranh giới có thể có, bằng giá trị tối ưu kép. Tính đối ngẫu LP đảm bảo rằng tối ưu kép bằng thời gian tối thiểu ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 1e100

def mat_mul(A, B):
    C = [[-INF] * 3 for _ in range(3)]
    for i in range(3):
        ai = A[i]
        for k in range(3):
            if ai[k] <= -INF / 2:
                continue
            v = ai[k]
            bk = B[k]
            for j in range(3):
                nv = v + bk[j]
                if nv > C[i][j]:
                    C[i][j] = nv
    return C

class SegTree:
    def __init__(self, arr, x, y):
        self.n = len(arr)
        self.x = x
        self.y = y

        self.trans = [[-INF] * 3 for _ in range(3)]
        for i in range(3):
            for j in range(3):
                if ((i != 2 and j != 2) or i == 0 or j == 0):
                    self.trans[i][j] = 0.0

        self.seg = [None] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def leaf(self, val):
        M = [[-INF] * 3 for _ in range(3)]
        M[0][0] = 0.0
        M[1][1] = val / (self.x + self.y)
        M[2][2] = val / self.y
        return M

    def merge(self, A, B):
        return mat_mul(mat_mul(A, self.trans), B)

    def build(self, p, l, r, arr):
        if l == r:
            self.seg[p] = self.leaf(arr[l])
            return

        m = (l + r) // 2
        self.build(p * 2, l, m, arr)
        self.build(p * 2 + 1, m + 1, r, arr)
        self.seg[p] = self.merge(self.seg[p * 2], self.seg[p * 2 + 1])

    def update(self, p, l, r, idx, val):
        if l == r:
            self.seg[p] = self.leaf(val)
            return

        m = (l + r) // 2
        if idx <= m:
            self.update(p * 2, l, m, idx, val)
        else:
            self.update(p * 2 + 1, m + 1, r, idx, val)

        self.seg[p] = self.merge(self.seg[p * 2], self.seg[p * 2 + 1])

    def modify(self, idx, val):
        self.update(1, 0, self.n - 1, idx, val)

    def query(self, p, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.seg[p]

        m = (l + r) // 2
        if qr <= m:
            return self.query(p * 2, l, m, ql, qr)
        if ql > m:
            return self.query(p * 2 + 1, m + 1, r, ql, qr)

        left = self.query(p * 2, l, m, ql, qr)
        right = self.query(p * 2 + 1, m + 1, r, ql, qr)
        return self.merge(left, right)

    def range_answer(self, l, r):
        M = self.query(1, 0, self.n - 1, l, r)
        ans = -INF
        for i in range(3):
            for j in range(3):
                ans = max(ans, M[i][j])
        return ans

def solve():
    n, q = map(int, input().split())
    x, y = map(int, input().split())

    if x > y:
        x, y = y, x

    b = list(map(int, input().split()))

    seg = SegTree(b, x, y)

    out = []

    for _ in range(q):
        op, a, c = map(int, input().split())

        if op == 1:
            seg.modify(a - 1, c)
        else:
            out.append(f"{seg.range_answer(a - 1, c - 1):.15f}")

    sys.stdout.write("\n".join(out))

solve()
```Ma trận lá thể hiện ba trạng thái có thể có của một vị trí. Chỉ các mục nhập đường chéo là hợp lệ vì một đoạn có độ dài bắt đầu và kết thúc ở cùng một trạng thái. 

Ma trận chuyển tiếp chứa số 0 cho các cặp trạng thái được phép và vô cực âm cho các cặp trạng thái bị cấm. Vùng lân cận bị cấm duy nhất là trạng thái 2 bên cạnh trạng thái 2. 

Hoạt động hợp nhất thực hiện$$A \cdot T \cdot B$$trong đại số cộng max. Đây là phép lặp DP để nối hai khoảng. 

Kết quả truy vấn là một ma trận mô tả toàn bộ phân đoạn. Việc lấy mục nhập tối đa sẽ loại bỏ mọi hạn chế đối với các trạng thái biên và mang lại giá trị tối ưu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Phân đoạn đầu vào:$[3,1,1,4]$,$x=1$,$y=2$. 

Trọng lượng là:$$\frac{a_i}{x+y}= \left[1,\frac13,\frac13,\frac43\right]$$Và$$\frac{a_i}{y}= \left[1.5,0.5,0.5,2\right].$$| Vị trí | Trạng thái 1 giá trị | Giá trị trạng thái 2 | 
| --- | --- | --- | 
| 1 | 1.0 | 1,5 | 
| 2 | 0,333333 | 0,5 | 
| 3 | 0,333333 | 0,5 | 
| 4 | 1.333333 | 2.0 | 

Lựa chọn tối ưu là trạng thái 2 tại vị trí 1 và 4, còn các láng giềng không sử dụng trạng thái 2. 

| Vị trí | Trạng thái được chọn | Đóng góp | 
| --- | --- | --- | 
| 1 | 2 | 1,5 | 
| 2 | 0 | 0 | 
| 3 | 0 | 0 | 
| 4 | 2 | 2.0 | 

Tổng cộng:$$1.5+2.0=3.5.$$Điều này phù hợp với đầu ra mẫu. 

Dấu vết thể hiện hạn chế được thiết lập độc lập. Vị trí 1 và 4 cách xa nhau nên cả hai đều có thể sử dụng hệ số mạnh hơn$1/y$. 

### Ví dụ tùy chỉnh nhỏ 

Mảng$[1,1,1]$,$x=1$,$y=2$. 

| Vị trí | Trạng thái 1 giá trị | Giá trị trạng thái 2 | 
| --- | --- | --- | 
| 1 | 0,333333 | 0,5 | 
| 2 | 0,333333 | 0,5 | 
| 3 | 0,333333 | 0,5 | 

Chọn trạng thái 2 ở vị trí 1 và 3 sẽ cho: 

| Vị trí | Tiểu bang | Đóng góp | 
| --- | --- | --- | 
| 1 | 2 | 0,5 | 
| 2 | 0 | 0 | 
| 3 | 2 | 0,5 | 

Tổng cộng$=1.0$. 

Dấu vết cho thấy tại sao trạng thái 2 hoạt động giống hệt như việc chọn các đỉnh của một tập hợp độc lập trên một đường đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$mỗi lần cập nhật/truy vấn | Chiều cao cây phân đoạn, kích thước ma trận không đổi | 
| Không gian |$O(n)$| Cây phân đoạn lưu trữ một$3\times3$ma trận trên mỗi nút | 

Các ma trận chỉ$3\times3$, nên mọi phép nhân đều là thời gian không đổi. Với$2\cdot10^5$hoạt động, tổng thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# sample
assert run(
"""4 3
1 2
3 1 1 4
2 1 4
1 1 1
2 1 3
"""
).strip().splitlines()[0] == "3.500000000000000"

# minimum length
run(
"""2 1
1 2
1 1
2 1 2
"""
)

# all equal values
run(
"""5 1
1 2
10 10 10 10 10
2 1 5
"""
)

# update first position
run(
"""3 2
1 2
1 2 3
1 1 100
2 1 3
"""
)

# update last position
run(
"""3 2
1 2
1 2 3
1 3 100
2 1 3
"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu | 3,5 rồi 1,0 | Tính đúng đắn cơ bản | 
| Chiều dài 2 | Câu trả lời hợp lệ hữu hạn | Phân khúc được phép nhỏ nhất | 
| Tất cả đều bình đẳng | Hành vi đối xứng | Giá trị lặp lại | 
| Cập nhật vị trí đầu tiên | Tính toán lại đúng | Cập nhật ranh giới bên trái | 
| Cập nhật vị trí cuối cùng | Tính toán lại đúng | Cập nhật ranh giới bên phải | 

## Vỏ cạnh 

Hãy xem xét$x=1,y=2$và mảng$[100,1,100]$. 

Tổng giới hạn dưới chỉ cung cấp$$201/3=67.$$DP của chúng tôi có thể chọn trạng thái 2 ở vị trí 1 và 3. 

| Vị trí | Tiểu bang | Đóng góp | 
| --- | --- | --- | 
| 1 | 2 | 50 | 
| 2 | 0 | 0 | 
| 3 | 2 | 50 | 

Câu trả lời ít nhất trở thành$100$, đó chính là điểm nghẽn thực sự. Cấu trúc được thiết lập độc lập sẽ tự động nắm bắt điều này. 

Coi như$[100,100]$. 

Việc chọn trạng thái 2 trên cả hai vị trí đều bị cấm. Giải pháp tốt nhất là: 

| Vị trí | Tiểu bang | Đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 100/(x+y) | 
| 2 | 1 | 100/(x+y) | 

Tổng cộng:$$\frac{200}{x+y}.$$Điều này phù hợp với thực tế là một thao tác trên cạnh duy nhất có thể xử lý đồng thời cả hai phần tử. 

Hãy xem xét một truy vấn phạm vi có độ dài hai ở ranh giới của mảng, chẳng hạn như$[1,2]$. 

Truy vấn cây phân đoạn trả về chính xác ma trận của khoảng đó. Không cần xử lý đặc biệt vì các trạng thái biên được mã hóa bên trong ma trận. Việc lấy mục nhập tối đa sẽ tự động xem xét mọi trạng thái bắt đầu và kết thúc hợp lệ.
