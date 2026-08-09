---
title: "CF 102437A - \u0411\u043b\u044d\u043a \\& \u0423\u0430\u0439\u0442"
description: "Có (n) thành phố được sắp xếp xung quanh một vòng tròn và một thủ đô ở giữa. Những con đường duy nhất có thể là (n) đường vòng giữa các thành phố bên ngoài liên tiếp và (n) nan hoa từ thủ đô đến các thành phố bên ngoài. Một số con đường có thể vắng mặt."
date: "2026-08-09T00:18:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 334
verified: true
draft: false
---

[CF 102437A - \u0411\u043b\u044d\u043a \\& \u0423\u0430\u0439\u0442](https://codeforces.com/problemset/problem/102437/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 34 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (n) thành phố được sắp xếp xung quanh một vòng tròn và một thủ đô ở giữa. Những con đường duy nhất có thể là (n) đường vòng giữa các thành phố bên ngoài liên tiếp và (n) nan hoa từ thủ đô đến các thành phố bên ngoài. Một số con đường có thể vắng mặt. Mọi con đường hiện có đều do Trắng hoặc Đen kiểm soát. 

Chúng ta cần đếm các cây bao trùm của biểu đồ này theo số lượng đường do Người da trắng kiểm soát mà chúng chứa. Nếu một cây có đúng (k) đường trắng thì nó đóng góp vào hệ số (a_k). Vì cây khung trên (n+1) đỉnh luôn có chính xác (n) cạnh nên (k) nằm trong khoảng từ (0) đến (n). 

Cách hữu ích để suy nghĩ về đầu vào là hai mảng tuần hoàn. Chuỗi (s) mô tả các cạnh hình tròn, trong đó (s_i) là cạnh từ thành phố (i) đến thành phố (i+1), với (n+1) được hiểu là thành phố (1). Chuỗi (t) mô tả các nan hoa, trong đó (t_i) là cạnh từ thủ đô đến thành phố (i). Một nhân vật`W`cho cạnh đó trọng số (x), một ký tự`B`mang lại cho nó trọng lượng (1), và`-`mang lại cho nó trọng số (0). 

Khi đó câu trả lời được yêu cầu chỉ đơn giản là dãy hệ số của đa thức sinh cây khung 

[ 
F(x)=\sum_T x^{#W(T)}. 
] 

Ràng buộc (n\le 50000) loại trừ mọi phương trình bậc hai trong (n). Ngay cả một chương trình động (O(n^2)) cũng sẽ yêu cầu khoảng (2,5\cdot 10^9) thao tác cơ bản ở giới hạn trên. Bản thân câu trả lời có hệ số (n+1), do đó, cách tiếp cận (O(n\log^2 n)) hoặc phương pháp thời gian đa thức tương tự là phù hợp. Mô-đun (998244353) đặc biệt thuận tiện vì nó hỗ trợ nhân đa thức NTT hiệu quả. 

Có một số trường hợp ranh giới trong đó một đối số đếm chính xác bề ngoài không thành công. Ví dụ: nếu mọi nan hoa đều vắng mặt```
3
WWW
---
```những thành phố hình tròn tạo thành vòng tròn nhưng thủ đô lại bị cô lập nên không có cây xanh bao quanh và đáp án là`0 0 0 0`. Một phương pháp tính rừng ở chu kỳ bên ngoài mà không thực thi rõ ràng khả năng kết nối với thủ đô sẽ tính sai số lượng rừng. 

Nếu mọi con đường đều có màu đen,```
3
BBB
BBB
```đồ thị là (K_4), do đó có (16) cây bao trùm và tất cả đều không chứa đường Trắng. Câu trả lời là`16 0 0 0`. Đây là một phép kiểm tra hữu ích vì hệ số không đổi phải tồn tại trong mọi thao tác đa thức. 

Nếu mọi con đường đều có màu trắng,```
3
WWW
WWW
```cùng một đồ thị có (16) cây bao trùm, nhưng mỗi cây đều chứa đúng ba cạnh Trắng. Câu trả lời là`0 0 0 16`. Điều này phát hiện ra những lỗi trong đó bậc đa thức được hiểu là số cạnh Đen. 

Ranh giới chu kỳ cũng có vấn đề. Vì```
4
---W
BBBB
```con đường vòng duy nhất là rìa từ thành phố (4) quay lại thành phố (1). Có một cây bao gồm cả bốn nan hoa màu đen và hai cây nữa sử dụng cạnh tròn màu trắng và bỏ qua nan hoa về thành phố (1) hoặc nan hoa về thành phố (4). Như vậy câu trả lời là`1 2 0 0 0`. Nếu coi mảng hình tròn là một đường dẫn sẽ bỏ sót hai cây này. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp có thể liệt kê mọi tập hợp con của các con đường hiện có, kiểm tra xem nó có chứa chính xác (n) cạnh hay không, sau đó kiểm tra xem các cạnh đó có tạo thành cây bao trùm hay không. Có nhiều nhất (2n) đường có thể có, vì vậy việc liệt kê tất cả các tập hợp con đã mang lại khả năng (2^{2n}=4^n). Việc kiểm tra kết nối cho mọi tập hợp con sẽ tốn thêm (O(n)), cho thời gian (O(n4^n)) khi thực hiện đơn giản nhất. Ngay cả khi sử dụng thực tế là một cây có chính xác (n) cạnh cũng chỉ thay đổi điều này thành gần đúng 

[ 
O\left(n\binom{2n}{n}\right), 
] 

vẫn còn theo cấp số nhân. Lực lượng vũ phu là chính xác bởi vì mỗi cây bao trùm chính xác là một tập hợp con cạnh được liệt kê, nhưng nó đã trở nên vô dụng từ rất lâu trước đó (n=50000). 

Cấu trúc của biểu đồ này cho chúng ta mô tả rõ ràng hơn về cây bao trùm. Chỉ nhìn vào các cạnh tròn đã chọn. Chúng tạo thành một tập hợp các đường dẫn hoặc toàn bộ vòng tròn. Nếu thiếu ít nhất một cạnh tròn, các cạnh tròn được chọn sẽ chia các đỉnh bên ngoài thành nhiều thành phần đường dẫn. Mỗi thành phần như vậy phải chứa chính xác một nan hoa được chọn cho thủ đô. Nếu nó không chứa nan hoa, thành phần đó sẽ vẫn bị ngắt kết nối với thủ đô. Nếu nó chứa hai nan hoa trở lên thì hai nan hoa cùng với đường đi giữa các điểm cuối của chúng sẽ tạo ra một chu trình. 

Quan sát này chuyển đổi vấn đề đồ thị thành vấn đề chuỗi cục bộ. Trong khi đi quanh vòng tròn, chúng ta chỉ cần nhớ xem thành phần bên ngoài hiện tại đã nhận được nan hoa duy nhất hay chưa. Đó là một máy tự động hai trạng thái. 

Bản chất tuần hoàn của biểu đồ có nghĩa là thay vì chạy máy tự động này từ trạng thái bắt đầu được chọn tùy ý, chúng ta nhân các ma trận chuyển tiếp (2\times2) của nó và lấy dấu vết của chúng. Dấu vết buộc trạng thái sau cạnh cuối cùng bằng trạng thái trước cạnh đầu tiên, chính xác những gì được yêu cầu khi chuỗi bao quanh. 

Mỗi phần tử ma trận là một đa thức trong (x). Do đó, chúng ta phải nhân (n) ma trận nhỏ có các phần tử là đa thức có bậc tăng dần. Phép nhân đa thức thông thường sẽ lại trở thành phép nhân bậc hai, vì vậy thành phần cuối cùng là tích chập NTT. Cây sản phẩm chia để trị nhân các ma trận ở cấp độ (O(\log n)), trong khi mỗi cấp thực hiện tổng số phép nhân đa thức giới hạn bởi (O(n\log n)). Độ phức tạp hoàn toàn là (O(n\log^2 n)). 

Bản thân tích ma trận có thể được thực hiện bằng bảy phép nhân đa thức bằng công thức Strassen (2\times2) thay vì tám phép nhân thông thường. Đây chỉ là một cách tối ưu hóa triển khai, nhưng nó quan trọng đối với việc triển khai Python vì tích chập đa thức chiếm ưu thế trong thời gian chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n4^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log^2 n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi mọi con đường hiện có là trọng số đa thức. Đường trắng có trọng số (x), đường đen có trọng số (1) và đường vắng có trọng số (0). Khi đó tích các trọng số của tập cạnh đã chọn chính xác là (x^k), trong đó (k) là số cạnh Trắng của nó. 
2. Với mỗi thành phố bên ngoài (i), gọi (q_i) là trọng lượng của các nan hoa và gọi (r_i) là trọng lượng của cạnh hình tròn từ (i) đến (i+1). 

Trong khi xử lý thành phố (i), hãy duy trì trạng thái (0) hoặc (1). Trạng thái (0) có nghĩa là thành phần hình tròn hiện tại chưa chọn một nan hoa, trong khi trạng thái (1) có nghĩa là nó đã có một nan hoa duy nhất. 
3. Đầu tiên xét trường hợp không có cạnh tròn (i). Thành phần hiện tại kết thúc tại thành phố (i), vì vậy nó phải chứa chính xác một nan hoa. Từ trạng thái (0), chúng ta phải chọn nan hoa, đóng góp (q_i) và chuyển sang trạng thái (0) cho thành phần tiếp theo. Từ trạng thái (1), chúng ta không được chọn một người nói khác, đóng góp (1) và lại chuyển sang trạng thái (0). 

Ma trận chuyển tiếp của nó là

[ 
A_i= 
\bắt đầu{pmatrix} 
q_i&0\ 
1&0 
\end{pmatrix}. 
] 
4. Bây giờ hãy xem xét trường hợp cạnh tròn (i) được chọn. Nó đóng góp (r_i). Nếu thành phần hiện tại có trạng thái (0), chúng ta có thể bỏ qua nan hoa và giữ nguyên trạng thái (0) hoặc chọn nan hoa và chuyển sang trạng thái (1). Nếu thành phần đã có trạng thái (1), chúng ta không thể chọn một nan hoa khác, vì vậy nó vẫn ở trạng thái (1). 

Ma trận chuyển tiếp trước khi nhân với trọng số cạnh là 

[ 
B_i= 
\bắt đầu{pmatrix} 
1&q_i\ 
0&1 
\end{pmatrix}. 
] 

Bao gồm cả trọng lượng cạnh tròn cho (r_iB_i). 
5. Chúng ta có thể chọn một trong hai trạng thái của cạnh tròn, do đó ma trận chuyển tiếp hoàn chỉnh là 

\bắt đầu{pmatrix} 
q_i+r_i&q_ir_i\ 
1&r_i 
\end{pmatrix}. 
] 

Mỗi lựa chọn cục bộ hợp lệ được biểu diễn chính xác một lần trong ma trận này. 
6. Nhân tất cả các ma trận theo thứ tự vòng tròn: 

[ 
P=M_1M_2\cdots M_n. 
] 

Việc thực hiện (\operatorname{tr}(P)) buộc máy tự động hoàn thành ở cùng trạng thái mà nó đã bắt đầu. Nếu thiếu ít nhất một cạnh tròn, điều đó sẽ đưa ra chính xác điều kiện là mọi thành phần hình tròn đều chứa một nan hoa. 
7. Có một cấu hình đặc biệt được tính theo dấu vết nhưng không phải là cây bao trùm. Nếu mọi cạnh tròn đều được chọn thì các đỉnh bên ngoài tạo thành một chu trình hoàn chỉnh. Máy tự động có hai trạng thái tuần hoàn có thể có, trạng thái (0) ở mọi nơi và trạng thái (1) ở mọi nơi và cả hai đều tạo ra mức đóng góp bằng 

[ 
\prod_i r_i. 
] 

Không có nan hoa nào được chọn trong cả hai cấu hình, do đó vốn bị ngắt kết nối. Vì vậy chúng ta phải trừ 

[ 
2\prod_i r_i. 
] 

Nếu thiếu cạnh tròn nào thì tích này bằng 0 và không có gì để trừ. 
8. Đa thức thu được có nhiều nhất là (n), vì mọi cây bao trùm đều có chính xác (n) cạnh. Trong quá trình nhân đa thức, chúng ta có thể loại bỏ các hệ số trên bậc (n), vì chúng không bao giờ có thể ảnh hưởng đến câu trả lời cuối cùng. 
9. Nhân các ma trận đa thức (n) theo tuần tự vẫn tạo ra kết quả (O(n^2)). Thay vào đó, hãy xây dựng một cây sản phẩm phân chia để chinh phục cân bằng. Một phân đoạn của ma trận được biểu diễn bằng tích của nó và hai tích của phân đoạn liền kề được nhân lên khi lệnh gọi đệ quy của chúng quay trở lại. 
10. Phép nhân đa thức được thực hiện bởi NTT theo (998244353). Đối với các đa thức rất nhỏ, phép nhân (O(ab)) thông thường nhanh hơn việc xây dựng mảng NTT, do đó việc triển khai sử dụng ngưỡng nhân nhỏ. 
11. Cuối cùng, lấy vết của ma trận hoàn chỉnh, trừ phần đóng góp đặc biệt của toàn bộ cạnh tròn và in các hệ số từ (0) đến (n). 

Bất biến đằng sau cấu trúc là, bất cứ khi nào một cạnh tròn được xử lý, trạng thái ma trận sẽ ghi lại chính xác liệu thành phần hình tròn hiện đang mở có chứa một nan hoa được phép hay không. Việc chuyển đổi qua một cạnh tròn vắng mặt chỉ được phép khi thành phần đó có chính xác một nan hoa, trong khi cạnh tròn được chọn chỉ mở rộng thành phần mà không cho phép một nan hoa thứ hai. Do đó, mọi chuỗi trạng thái tuần hoàn được tính bằng dấu vết tương ứng với một tập hợp các đường tròn có chính xác một nan hoa cho mỗi thành phần. Một tập hợp như vậy có chính xác (n) cạnh và được kết nối với nhau, do đó nó là một cây bao trùm. Các cấu hình dấu vết duy nhất nằm ngoài sự tương ứng này là hai trạng thái không có nan hoa khi mọi cạnh tròn được chọn và chúng bị loại bỏ một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
ROOT = 3
NAIVE_LIMIT = 32

LIMIT = 0
root_cache = {}
inv_root_cache = {}

def cut(p):
    if len(p) > LIMIT + 1:
        p = p[:LIMIT + 1]
    while len(p) > 1 and p[-1] == 0:
        p.pop()
    return p

def padd(a, b):
    n = max(len(a), len(b))
    c = [0] * n
    la = len(a)
    lb = len(b)
    for i in range(n):
        x = a[i] if i < la else 0
        y = b[i] if i < lb else 0
        z = x + y
        if z >= MOD:
            z -= MOD
        c[i] = z
    return cut(c)

def psub(a, b):
    n = max(len(a), len(b))
    c = [0] * n
    la = len(a)
    lb = len(b)
    for i in range(n):
        x = a[i] if i < la else 0
        y = b[i] if i < lb else 0
        z = x - y
        if z < 0:
            z += MOD
        c[i] = z
    return cut(c)

def lincomb(items):
    n = 1
    for p, _ in items:
        if len(p) > n:
            n = len(p)

    c = [0] * n
    for p, sign in items:
        if sign == 1:
            for i, x in enumerate(p):
                c[i] += x
        else:
            for i, x in enumerate(p):
                c[i] -= x

    for i in range(n):
        c[i] %= MOD

    return cut(c)

def ntt(a, invert):
    n = len(a)

    j = 0
    for i in range(1, n):
        bit = n >> 1
        while j & bit:
            j ^= bit
            bit >>= 1
        j ^= bit
        if i < j:
            a[i], a[j] = a[j], a[i]

    length = 2
    while length <= n:
        if invert:
            wlen = inv_root_cache.get(length)
            if wlen is None:
                wlen = pow(ROOT, (MOD - 1) // length, MOD)
                wlen = pow(wlen, MOD - 2, MOD)
                inv_root_cache[length] = wlen
        else:
            wlen = root_cache.get(length)
            if wlen is None:
                wlen = pow(ROOT, (MOD - 1) // length, MOD)
                root_cache[length] = wlen

        half = length >> 1

        for start in range(0, n, length):
            w = 1
            end = start + half
            j = start
            while j < end:
                u = a[j]
                v = a[j + half] * w % MOD

                x = u + v
                if x >= MOD:
                    x -= MOD

                y = u - v
                if y < 0:
                    y += MOD

                a[j] = x
                a[j + half] = y
                w = w * wlen % MOD
                j += 1

        length <<= 1

    if invert:
        inv_n = pow(n, MOD - 2, MOD)
        for i in range(n):
            a[i] = a[i] * inv_n % MOD

def convolution(a, b):
    if not a or not b:
        return [0]

    if a == [0] or b == [0]:
        return [0]

    la = len(a)
    lb = len(b)

    if min(la, lb) <= NAIVE_LIMIT or la * lb <= 4096:
        res = [0] * (min(la + lb - 1, LIMIT + 1))

        for i, x in enumerate(a):
            if x == 0:
                continue
            max_j = min(lb, LIMIT + 1 - i)
            for j in range(max_j):
                res[i + j] = (res[i + j] + x * b[j]) % MOD

        return cut(res)

    need = min(la + lb - 1, LIMIT + 1)

    size = 1
    while size < la + lb - 1:
        size <<= 1

    fa = a + [0] * (size - la)
    fb = b + [0] * (size - lb)

    ntt(fa, False)
    ntt(fb, False)

    for i in range(size):
        fa[i] = fa[i] * fb[i] % MOD

    ntt(fa, True)

    return cut(fa[:need])

def matrix_add(a, b):
    return (
        padd(a[0], b[0]),
        padd(a[1], b[1]),
        padd(a[2], b[2]),
        padd(a[3], b[3]),
    )

def matrix_product(a, b):
    """
    a = [[a0, a1],
         [a2, a3]]

    b = [[b0, b1],
         [b2, b3]]

    Polynomial Strassen multiplication.
    """

    a0, a1, a2, a3 = a
    b0, b1, b2, b3 = b

    p1 = convolution(padd(a0, a3), padd(b0, b3))
    p2 = convolution(padd(a2, a3), b0)
    p3 = convolution(a0, psub(b1, b3))
    p4 = convolution(a3, psub(b2, b0))
    p5 = convolution(padd(a0, a1), b3)
    p6 = convolution(psub(a2, a0), padd(b0, b1))
    p7 = convolution(psub(a1, a3), padd(b2, b3))

    c0 = lincomb([
        (p1, 1),
        (p4, 1),
        (p5, -1),
        (p7, 1),
    ])

    c1 = lincomb([
        (p3, 1),
        (p5, 1),
    ])

    c2 = lincomb([
        (p2, 1),
        (p4, 1),
    ])

    c3 = lincomb([
        (p1, 1),
        (p3, 1),
        (p2, -1),
        (p6, 1),
    ])

    return c0, c1, c2, c3

def make_poly(v):
    if v == 0:
        return [0]
    if v == 1:
        return [1]
    return [0, 1]

def build_product(s, t, left, right):
    if right - left == 1:
        q = make_poly(1 if t[left] == 'B' else 0 if t[left] == '-' else 2)
        r = make_poly(1 if s[left] == 'B' else 0 if s[left] == '-' else 2)

        # The encoding above used 2 for W temporarily.
        # Replace it by the polynomial x.
        if t[left] == 'W':
            q = [0, 1]
        if s[left] == 'W':
            r = [0, 1]

        qr = convolution(q, r)
        qr = cut(qr)

        return (
            padd(q, r),
            qr,
            [1],
            r,
        )

    mid = (left + right) >> 1

    a = build_product(s, t, left, mid)
    b = build_product(s, t, mid, right)

    return matrix_product(a, b)

def solve_case(n, s, t):
    global LIMIT
    LIMIT = n

    # M_i =
    #
    # [ q_i + r_i, q_i r_i ]
    # [     1,       r_i   ]

    product = build_product(s, t, 0, n)

    answer = product[0][:]
    if len(product[3]) > len(answer):
        answer += [0] * (len(product[3]) - len(answer))

    for i, x in enumerate(product[3]):
        answer[i] = (answer[i] + x) % MOD

    # The trace is product[0] + product[3].
    # If every circular edge is present, it contains two
    # invalid zero-spoke cyclic states.
    if '-' not in s:
        white_rim = s.count('W')
        if white_rim >= len(answer):
            answer += [0] * (white_rim + 1 - len(answer))
        answer[white_rim] = (answer[white_rim] - 2) % MOD

    if len(answer) < n + 1:
        answer += [0] * (n + 1 - len(answer))

    answer = answer[:n + 1]

    return ' '.join(map(str, answer))

def main():
    n = int(input())
    s = input().strip()
    t = input().strip()
    print(solve_case(n, s, t))

if __name__ == "__main__":
    main()
```Việc triển khai thể hiện một ma trận đa thức (2\times2) bằng một bộ gồm bốn đa thức theo thứ tự hàng lớn. Tại một chiếc lá,`q`là trọng lượng nói và`r`là trọng số của cạnh tròn, nên ma trận chính xác là 

[ 
\bắt đầu{pmatrix} 
q+r&qr\ 
1&r 
\end{pmatrix}. 
] 

Sự chuyển đổi nhỏ trong`build_product`cố tình giữ các cạnh vắng mặt là đa thức 0, các cạnh Đen là đa thức không đổi (1) và các cạnh Trắng là (x). 

Hàm đệ quy`build_product`giữ nguyên trật tự tuần hoàn ban đầu. Đầu tiên nó tính tích của nửa bên trái và nửa bên phải, sau đó nhân hai ma trận đó. Không có sự giao hoán nào của ma trận được thực hiện, điều này là cần thiết vì bản thân phép nhân ma trận không có tính chất giao hoán. 

Các phép toán đa thức luôn cắt ngắn ở mức độ (n). Điều này là an toàn vì mọi số hạng cây bao trùm mong muốn đều có chính xác (n) cạnh, do đó các hệ số ở bậc cao hơn không bao giờ có thể ảnh hưởng đến câu trả lời được yêu cầu. 

Phép nhân ma trận sử dụng đẳng thức Strassen bảy tích. Đối với đa thức, mỗi phép nhân vô hướng trong đẳng thức đó được thay thế bằng một phép tích chập. Điều này làm giảm tám tích chập NTT xuống còn bảy cho mỗi lần hợp nhất ma trận. 

các`convolution`chức năng chuyển sang phép nhân thông thường cho đầu vào nhỏ. Điều này quan trọng vì NTT có hệ số không đổi tương đối lớn, trong khi đa thức gồm vài chục hệ số sẽ rẻ hơn khi nhân trực tiếp. 

Phép trừ (2\prod r_i) được thực hiện mà không cần nhân đa thức khác. Vì mỗi (r_i) là (0), (1) hoặc (x), tích của chúng bằng 0 nếu không có cạnh tròn nào. Nếu không thì nó chỉ đơn giản là (x^c), trong đó (c) là số cạnh hình tròn màu trắng. 

Số nguyên Python không bị tràn, do đó không có vấn đề tràn riêng biệt. Mọi kết quả số học nhập vào đa thức đều được rút gọn modulo (998244353). Độ dài NTT không bao giờ đạt đến giới hạn mô đun vì biến đổi yêu cầu lớn nhất nằm dưới mức công suất được hỗ trợ của hai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3
---
WBW
```Mọi cạnh tròn đều vắng mặt nên (r_i=0). Các trọng số nan hoa là (q_1=x), (q_2=1) và (q_3=x). 

Các ma trận chuyển tiếp riêng lẻ là 

[ 
M_1= 
\bắt đầu{pmatrix} 
x&0\ 
1&0 
\end{pmatrix}, 
\quad 
M_2= 
\bắt đầu{pmatrix} 
1&0\ 
1&0 
\end{pmatrix}, 
\quad 
M_3= 
\bắt đầu{pmatrix} 
x&0\ 
1&0 
\end{pmatrix}. 
] 

Các trạng thái sản phẩm phát triển như sau. 

| Bước | Sản phẩm ma trận | Dấu vết | 
| --- | --- | --- | 
| Bắt đầu | (M_1) | (x) | 
| Sau thành phố 2 | (M_1M_2) | (x) | 
| Sau thành phố 3 | (M_1M_2M_3) | (x^2) | 

Không có cạnh tròn nên hiệu chỉnh toàn cạnh tròn đặc biệt bằng 0. Đa thức cuối cùng là (x^2), cho```
0 0 1 0
```Điều này chứng tỏ rằng số mũ đa thức đếm trực tiếp các cạnh Trắng. Cây bao trùm duy nhất có thể bao gồm cả ba nan hoa, hai trong số đó là màu Trắng. 

### Mẫu 2 

Đầu vào là```
3
WWW
BBB
```Mỗi cạnh tròn có trọng số (x), trong khi mỗi nan hoa có trọng số (1). Do đó, cả ba ma trận chuyển tiếp đều là 

[ 
M= 
\bắt đầu{pmatrix} 
1+x&x\ 
1&x 
\end{pmatrix}. 
] 

Các sản phẩm nối tiếp nhau là 

| Bước | Ma trận đa thức | 
| --- | --- | 
| (M) | (\begin{pmatrix}1+x&x\1&x\end{pmatrix}) | 
| (M^2) | (\begin{pmatrix}1+3x+x^2&x+2x^2\1+2x&x+x^2\end{pmatrix}) | 
| (M^3) | (\begin{pmatrix}1+4x+5x^2+x^3&x+3x^2+2x^3\1+3x&x+3x^2+x^3\end{pmatrix}) | 

Dấu vết là 

[ 
1+6x+9x^2+2x^3. 
] 

Tất cả các cạnh tròn đều có mặt, do đó hai trạng thái không có nan hoa không hợp lệ đóng góp (2x^3). Loại bỏ chúng mang lại 

[ 
1+6x+9x^2. 
] 

Như vậy câu trả lời là```
1 6 9 0
```Sự điều chỉnh là phần quan trọng của ví dụ này. Nếu không có nó, hệ số của (x^3) sẽ là (2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log^2 n)) | Sản phẩm ma trận chia để trị có các cấp (O(\log n)) và mỗi cấp thực hiện tổng số (O(n\log n)) công việc tích chập NTT | 
| Không gian | (O(n)) | Mỗi sản phẩm đệ quy lưu trữ các hệ số đa thức (O(n)) và bộ đệm NTT lớn nhất cũng là (O(n)) | 

Trường hợp lớn nhất có (50000) thành phố bên ngoài, do đó, thuật toán bậc hai sẽ yêu cầu hàng tỷ thao tác. Cách tiếp cận cây sản phẩm làm giảm công việc đa thức thành độ phức tạp gần tuyến tính ngoài các yếu tố logarit. Mô-đun (998244353) cho phép tất cả các phép tích chập cần thiết được thực hiện chính xác với NTT. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve_case`hoạt động như giải pháp được gửi. Các thử nghiệm kích thước tối đa sử dụng tổng số cây bao trùm đã biết của một bánh xe không có trọng số. Đối với bánh xe có (n) đỉnh ngoài, tổng số này là (L_{2n}-2), trong đó (L_0=2), (L_1=1) và (L_i=L_{i-1}+L_{i-2}).```python
# helper: run solution on input string, return output string
import io
import sys

def run(inp: str) -> str:
    data = inp.strip().splitlines()
    n = int(data[0])
    s = data[1].strip()
    t = data[2].strip()
    return solve_case(n, s, t)

# Provided samples
assert run("""3
---
WBW
""") == "0 0 1 0", "sample 1"

assert run("""3
WWW
BBB
""") == "1 6 9 0", "sample 2"

assert run("""5
BWB-B
WB-W-
""") == "0 2 6 3 0 0", "sample 3"

# Minimum-size graph, all roads Black.
assert run("""3
BBB
BBB
""") == "16 0 0 0", "all black, K4"

# Minimum-size graph, all roads White.
assert run("""3
WWW
WWW
""") == "0 0 0 16", "all white, K4"

# No spokes, so the capital is isolated.
assert run("""3
WWW
---
""") == "0 0 0 0", "isolated capital"

# Only the wrap-around circular edge exists and is White.
assert run("""4
---W
BBBB
""") == "1 2 0 0 0", "wrap-around edge"

# Maximum-size all-Black instance.
n = 50000
s = "B" * n
t = "B" * n

lucas0, lucas1 = 2, 1
for _ in range(2 * n):
    lucas0, lucas1 = lucas1, (lucas0 + lucas1) % MOD

total = (lucas1 - 2) % MOD
expected = " ".join([str(total)] + ["0"] * n)

assert run(f"{n}\n{s}\n{t}\n") == expected, "maximum-size all black"

# Maximum-size all-White instance.
s = "W" * n
t = "W" * n
expected = " ".join(["0"] * n + [str(total)])

assert run(f"{n}\n{s}\n{t}\n") == expected, "maximum-size all white"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / BBB / BBB`|`16 0 0 0`| Kích thước tối thiểu và trọng lượng toàn màu đen | 
|`3 / WWW / WWW`|`0 0 0 16`| Trọng số toàn màu trắng và mức độ cao nhất | 
|`3 / WWW / ---`|`0 0 0 0`| Vốn bị ngắt kết nối | 
|`4 / ---W / BBBB`|`1 2 0 0 0`| Lập chỉ mục ranh giới và cạnh bao quanh hình tròn | 
| (n=50000), tất cả`B`| (L_{100000}-2) ở hệ số 0 | Đầu vào có kích thước tối đa và đường dẫn NTT | 
| (n=50000), tất cả`W`| (L_{100000}-2) ở hệ số (50000) | Mức độ tối đa và ranh giới toàn màu trắng | 

## Vỏ cạnh 

Khi chữ hoa không có cạnh tới, thuật toán sẽ tự động cho kết quả bằng 0. Mọi quá trình chuyển đổi đều có (q_i=0), do đó, một thành phần hình tròn không bao giờ có thể đóng bằng một nan hoa đã chọn. Nếu vẫn còn biểu đồ hình tròn thì dấu vết có thể mô tả các cấu hình hình tròn nhưng điều kiện một nan hoa cho mỗi thành phần bắt buộc sẽ loại bỏ chúng. Vì`3 / WWW / ---`, mọi hệ số đều bằng không. 

Khi mọi con đường đều có màu Đen thì trọng số của mọi con đường là (1). Với (n=3), đồ thị là (K_4) và tích chuyển tiếp tạo ra số lượng cây bao trùm đầy đủ. Việc hiệu chỉnh sẽ trừ đi hai trạng thái toàn cạnh tròn không hợp lệ, để lại (16) ở mức 0. Như vậy câu trả lời là`16 0 0 0`. 

Khi mọi con đường đều có màu Trắng, mỗi cạnh được chọn sẽ đóng góp một hệ số (x). Mỗi cây bao trùm đều có chính xác (n) cạnh, vì vậy tất cả các số hạng hợp lệ đều phải có bậc (n). Với (n=3), đa thức là (16x^3), tạo ra`0 0 0 16`. Đây cũng là một cách kiểm tra hữu ích để đảm bảo việc cắt bớt độ trên (n) không thể loại bỏ câu trả lời hợp lệ. 

Nếu thiếu một cạnh tròn, hiệu chỉnh đặc biệt sẽ biến mất vì (\prod_i r_i=0). Ví dụ, với```
4
---W
BBBB
```cạnh tròn duy nhất là cạnh bao quanh (4\leftrightarrow1). Cây toàn nan đóng góp một số hạng bằng 0. Việc chọn cạnh tròn màu Trắng sẽ tạo ra một đường dẫn giữa các thành phố (4) và (1), do đó phải loại bỏ chính xác một trong hai nan hoa của chúng. Có hai cây như vậy, cả hai đều có một cạnh Trắng. Kết quả là`1 2 0 0 0`. 

Trường hợp tồn tại tất cả các cạnh tròn là trường hợp ranh giới tuần hoàn tinh tế. Dấu vết có hai cấu hình nhân tạo vì máy tự động có thể duy trì vĩnh viễn ở trạng thái (0) hoặc mãi mãi ở trạng thái (1) mà không bao giờ đóng một thành phần nào. Cả hai đều tương ứng với việc chọn mọi cạnh tròn và không có nan hoa. Chúng không phải là cây bao trùm vì vốn bị cô lập nên việc trừ chính xác (2\prod_i r_i) là cần thiết. Phép trừ cũng xử lý chính xác các cạnh hình tròn Đen và Trắng hỗn hợp, vì tích có lũy thừa thích hợp là (x). 

Cuối cùng, các cạnh vắng mặt phải được biểu diễn bằng đa thức 0 thay vì đơn giản là bỏ qua. Sự hiện diện của chúng làm thay đổi cấu trúc kết nối, không chỉ trọng lượng của một cạnh được chọn. Ma trận chuyển tiếp kết hợp điều này bằng cách thực hiện mọi chuyển đổi chọn đường vắng mặt đóng góp bằng 0, trong khi vẫn cho phép máy tự động đóng thành phần hiện tại khi đường đó bị bỏ qua.
