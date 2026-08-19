---
title: "CF 102214I - Hình ảnh"
description: "Chúng ta có một hình ảnh thang độ xám lớn (I), có pixel là byte được viết dưới dạng giá trị thập lục phân gồm hai chữ số và hình ảnh mẫu nhỏ hơn (T). Mẫu có thể đã được cắt từ hình ảnh lớn nhưng do nén bị mất nên các pixel của nó không cần khớp chính xác."
date: "2026-08-18T11:35:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102214
codeforces_index: "I"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u043e\u0435 \u043b\u0438\u0447\u043d\u043e\u0435 \u043f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0418\u041a\u0418\u0422 \u0421\u0424\u0423 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2015"
rating: 0
weight: 102214
solve_time_s: 217
verified: true
draft: false
---

[CF 102214I - Hình ảnh](https://codeforces.com/problemset/problem/102214/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hình ảnh thang độ xám lớn (I), có pixel là byte được viết dưới dạng giá trị thập lục phân gồm hai chữ số và hình ảnh mẫu nhỏ hơn (T). Mẫu có thể đã được cắt từ hình ảnh lớn nhưng do nén bị mất nên các pixel của nó không cần khớp chính xác. 

Đối với mọi vị trí trên cùng bên trái ((x,y)) có thể phù hợp với mẫu, chúng tôi so sánh mọi pixel mẫu (T(i,j)) với pixel hình ảnh tương ứng (I(x+i,y+j)). Điểm số là tổng bình phương của sự khác biệt, 

[ 
SSD(x,y)=\sum_{i=0}^{M-1}\sum_{j=0}^{N-1} 
(I(x+j,y+i)-T(j,i))^2. 
] 

Đầu ra được yêu cầu là bất kỳ vị trí nào có số điểm tối thiểu. Các tọa độ dựa trên 0, vì vậy (0\le x\le W-N) và (0\le y\le H-M). Các pixel đầu vào có dạng thập lục phân, nhưng sau khi phân tích chúng, chúng là các số nguyên thông thường từ 0 đến 255. 

Hình ảnh lớn có thể chứa tối đa (1024\cdot768=786432) pixel, trong khi bản thân mẫu có thể lớn gần như vậy. Do đó, việc so sánh trực tiếp ở mọi vị trí có thể có thể thực hiện hàng chục tỷ thao tác pixel. Thuật toán bậc hai hoặc bậc bốn trong kích thước hình ảnh không thực tế trong giới hạn 4 giây. Chúng ta cần tính toán đồng thời tất cả các mối tương quan mẫu, đây chính xác là loại hoạt động mà tích chập và FFT hữu ích. 

Có một số trường hợp ranh giới mà việc triển khai trực tiếp có thể xử lý sai. Nếu mẫu có kích thước giống hệt với hình ảnh thì chỉ có một vị trí pháp lý. Ví dụ,```
1 1
7
1 1
7
```có câu trả lời duy nhất có thể`0 0`. Một tìm kiếm vô tình sử dụng`< W-N`thay vì`<= W-N`sẽ không tìm được vị trí nào 

Mẫu một pixel là một trường hợp ranh giới hữu ích khác. Vì```
3 1
10 20 30
1 1
1E
```mẫu chứa thập lục phân`1E`, là số thập phân 30, nên đáp án là`2 0`. Việc xử lý đầu vào dưới dạng thập phân thay vì thập lục phân sẽ âm thầm thay đổi vấn đề. 

Hình ảnh có giá trị bằng nhau có thể có nhiều vị trí tối ưu. Vì```
3 2
07 07 07
07 07 07
2 1
07 07
```mọi vị trí pháp lý đều có SSD bằng 0, vì vậy`0 0`,`1 0`, Và`0 1`tất cả đều đúng. Chương trình không được cho rằng tối ưu là duy nhất. 

Cuối cùng, vị trí dưới cùng bên phải là hợp pháp và phải được kiểm tra. Ví dụ,```
3 3
00 00 00
00 00 00
00 00 2A
1 1
2A
```có điểm tối ưu duy nhất tại`2 2`. Một vòng lặp không chính xác dừng lại ở`W-N-1`hoặc`H-M-1`nhớ nó. 

## Phương pháp tiếp cận 

Phương pháp vũ phu tuân theo định nghĩa trực tiếp. Đối với mọi vị trí hợp pháp trên cùng bên trái, nó truy cập tất cả các pixel mẫu (NM), tính toán sự khác biệt với pixel hình ảnh tương ứng, bình phương nó và thêm nó vào điểm hiện tại. Điều này đúng vì mọi ổ SSD đều được đánh giá chính xác như đã xác định. 

Số vị trí là 

[ 
(W-N+1)(H-M+1), 
] 

vậy tổng công việc là 

[ 
O((W-N+1)(H-M+1)NM). 
] 

Tại (W=1024), (H=768), (N=512) và (M=384), có (513\cdot385=197505) vị trí và mỗi lần quét so sánh (512\cdot384=196608) pixel. Đó là khoảng (3,88\times10^{10}) so sánh pixel. Phương pháp vũ phu đơn giản về mặt toán học nhưng vượt xa giới hạn thời gian cho phép. 

Quan sát hữu ích đến từ việc mở rộng hình vuông: 

[ 
(I-T)^2=I^2-2IT+T^2. 
] 

Đối với một vị trí cố định, điều này mang lại 

\sum I(x+j,y+i)^2 
-2\sum I(x+j,y+i)T(j,i) 
+\tổng T(j,i)^2. 
] 

Thuật ngữ cuối cùng không phụ thuộc vào vị trí vì mẫu không bao giờ thay đổi. Số hạng đầu tiên là tổng các bình phương trên một cửa sổ hình chữ nhật của hình ảnh lớn, vì vậy mọi giá trị như vậy có thể thu được trong thời gian không đổi sau khi xây dựng tổng tiền tố hai chiều của (I^2). 

Phần khó khăn duy nhất là 

[ 
C(x,y)= 
\sum I(x+j,y+i)T(j,i). 
] 

Đây là mối tương quan chéo hai chiều. Nếu chúng ta đảo ngược mẫu theo cả hai chiều, mối tương quan sẽ trở thành tích chập hai chiều thông thường. Định lý tích chập nói rằng tích chập này có thể được tính bằng cách biến đổi cả hai mảng bằng FFT hai chiều, nhân các hệ số tần số tương ứng và chuyển đổi kết quả trở lại. Điều này thay đổi phần tốn kém từ việc quét mọi pixel mẫu ở mọi vị trí thành quét đại khái (O(PQ(\log P+\log Q))), trong đó (P) và (Q) là lũy thừa phù hợp của hai. 

Ngoài ra còn có một tối ưu hóa triển khai hữu ích. Giải pháp FFT đơn giản sẽ thực hiện một biến đổi thuận cho hình ảnh và một biến đổi khác cho mẫu bị đảo ngược, theo sau là một biến đổi nghịch đảo. Vì cả hai đầu vào đều là số thực nên chúng ta có thể gói chúng thành một mảng phức tạp dưới dạng (I+iT'). Từ một biến đổi Fourier, các biến đổi của phần thực và phần ảo có thể được phục hồi bằng cách sử dụng tính đối xứng liên hợp. Điều này chỉ để lại một FFT hai chiều thuận và một FFT hai chiều nghịch đảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((W-N+1)(H-M+1)NM)) | (O(WH)) | Quá chậm | 
| Tối ưu | (O(PQ(\log P+\log Q))) | (O(PQ)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc hình ảnh và mẫu, chuyển đổi từng pixel thập lục phân hai chữ số thành số nguyên từ 0 đến 255. Lưu trữ hình ảnh lớn dưới dạng (I) và mẫu dưới dạng (T). Các kích thước được giữ ở dạng chiều rộng đầu tiên cho đầu ra, trong khi các mảng sử dụng lập chỉ mục theo hàng đầu tiên. 
2. Xây dựng tổng tiền tố hai chiều của (I^2). Đối với bất kỳ cửa sổ hình ảnh hình chữ nhật nào, tổng số pixel hình ảnh bình phương có thể thu được bằng bốn lần truy cập tổng tiền tố. Điều này xử lý thuật ngữ phụ thuộc vào vị trí (\sum I^2) mà không cần truy cập nhiều lần vào tất cả các pixel mẫu. 
3. Tính (S_T=\sum T^2) một lần. Điều này giống nhau đối với mọi vị trí có thể có, vì vậy không có lý do gì để tính toán lại nó. 
4. Chọn lũy thừa của hai (P) và (Q) thỏa mãn (P\ge H+M-1) và (Q\ge W+N-1). Các kích thước này đủ lớn để chứa tích chập tuyến tính hoàn chỉnh, thay vì tích chập tuần hoàn. Phần đệm là cần thiết vì FFT tính toán tích chập theo chu kỳ một cách tự nhiên. 
5. Tạo một mảng phức (P\times Q). Đặt hình ảnh lớn (I) vào phần thật của nó. Đặt mẫu đảo ngược ở cả hai chiều vào phần ảo của nó. Nếu (T') là mẫu đảo ngược, giá trị được lưu trữ về mặt khái niệm là (I+iT'). 
6. Áp dụng FFT hai chiều. Phép biến đổi hai chiều được triển khai dưới dạng FFT một chiều trên mỗi hàng, theo sau là FFT một chiều ở mỗi cột. Sau thao tác này, sử dụng danh tính đối xứng liên hợp để khôi phục các biến đổi miền tần số của (I) và (T'). 
7. Nhân các phép biến đổi đã phục hồi theo điểm. Theo định lý tích chập, phép biến đổi nghịch đảo của tích này là tích chập (I*T'). Vì (T') là mẫu bị đảo ngược theo cả hai chiều nên hệ số tại ((y+M-1,x+N-1)) chính xác là mối tương quan (C(x,y)) mà công thức SSD cần. 
8. Áp dụng FFT hai chiều nghịch đảo để thu được tất cả các giá trị tương quan trong miền không gian. FFT dấu phẩy động gây ra các lỗi số rất nhỏ, do đó, mỗi mối tương quan được làm tròn đến số nguyên gần nhất trước khi được sử dụng trong công thức SSD số nguyên chính xác. 
9. Lặp lại tất cả các vị trí mẫu pháp lý. Đối với mỗi ((x,y)), lấy tổng cửa sổ của (I^2) từ tổng tiền tố, lấy mối tương quan từ kết quả tích chập và tính toán 

[ 
SSD(x,y)=windowSquareSum-2C(x,y)+S_T. 
] 

Giữ vị trí có giá trị nhỏ nhất. 

1. In vị trí tốt nhất như`x y`. Quét các vị trí từ trên xuống dưới và từ trái sang phải là đủ khi một số vị trí có cùng ổ SSD, vì câu lệnh cho phép bất kỳ vị trí tối ưu nào. 

### Tại sao nó hoạt động 

Tổng tiền tố cho giá trị chính xác của số hạng đầu tiên trong công thức SSD mở rộng và (S_T) chính xác là số hạng thứ ba không đổi. Việc đảo ngược mẫu sẽ chuyển đổi mối tương quan chéo còn lại thành tích chập, có hệ số được phục hồi bằng tính toán FFT. Do đó, đối với mọi vị trí hợp pháp, thuật toán sẽ xây dựng lại chính xác ba thuật ngữ xác định SSD của nó, cho đến lỗi FFT dấu phẩy động không đáng kể được loại bỏ bằng cách làm tròn tương quan số nguyên. Vì mọi vị trí hợp pháp đều được kiểm tra và ổ SSD được tái tạo nhỏ nhất được chọn nên vị trí được trả về là tối ưu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def next_pow2(x):
    p = 1
    while p < x:
        p <<= 1
    return p

def make_rev(n):
    rev = [0] * n
    half = n >> 1
    j = 0
    for i in range(1, n):
        bit = half
        while j & bit:
            j ^= bit
            bit >>= 1
        j ^= bit
        rev[i] = j
    return rev

def make_roots(n):
    forward = {}
    inverse = {}

    length = 2
    while length <= n:
        half = length >> 1
        angle = 2.0 * math.pi / length

        wf = []
        wi = []
        for j in range(half):
            a = angle * j
            c = math.cos(a)
            s = math.sin(a)
            wf.append(complex(c, -s))
            wi.append(complex(c, s))

        forward[length] = wf
        inverse[length] = wi
        length <<= 1

    return forward, inverse

def fft1d(a, invert, rev, roots_forward, roots_inverse):
    n = len(a)

    for i in range(n):
        j = rev[i]
        if i < j:
            a[i], a[j] = a[j], a[i]

    length = 2
    roots = roots_inverse if invert else roots_forward

    while length <= n:
        half = length >> 1
        w = roots[length]

        for base in range(0, n, length):
            for j in range(half):
                u = a[base + j]
                v = a[base + j + half] * w[j]
                a[base + j] = u + v
                a[base + j + half] = u - v

        length <<= 1

    if invert:
        inv_n = 1.0 / n
        for i in range(n):
            a[i] *= inv_n

def fft2(mat, invert, rev_p, rev_q, roots_p_f, roots_p_i,
         roots_q_f, roots_q_i):
    p = len(mat)
    q = len(mat[0])

    for r in range(p):
        fft1d(mat[r], invert, rev_q, roots_q_f, roots_q_i)

    col = [0j] * p
    for c in range(q):
        for r in range(p):
            col[r] = mat[r][c]

        fft1d(col, invert, rev_p, roots_p_f, roots_p_i)

        for r in range(p):
            mat[r][c] = col[r]

def build_prefix_square(img):
    h = len(img)
    w = len(img[0])

    pref = [[0] * (w + 1) for _ in range(h + 1)]

    for r in range(h):
        row_sum = 0
        prev = pref[r]
        cur = pref[r + 1]

        for c in range(w):
            v = img[r][c]
            row_sum += v * v
            cur[c + 1] = prev[c + 1] + row_sum

    return pref

def rect_sum(pref, y1, x1, y2, x2):
    return (
        pref[y2][x2]
        - pref[y1][x2]
        - pref[y2][x1]
        + pref[y1][x1]
    )

def solve():
    first = input().split()
    while not first:
        first = input().split()

    W, H = map(int, first)

    image = []
    for _ in range(H):
        row = input().split()
        while not row:
            row = input().split()
        image.append([int(x, 16) for x in row])

    N, M = map(int, input().split())

    template = []
    for _ in range(M):
        row = input().split()
        while not row:
            row = input().split()
        template.append([int(x, 16) for x in row])

    pref = build_prefix_square(image)

    template_square = 0
    for row in template:
        for v in row:
            template_square += v * v

    P = next_pow2(H + M - 1)
    Q = next_pow2(W + N - 1)

    mat = [[0j] * Q for _ in range(P)]

    for r in range(H):
        dst = mat[r]
        src = image[r]
        for c in range(W):
            dst[c] = complex(src[c], 0.0)

    for r in range(M):
        src = template[r]
        dst = mat[M - 1 - r]
        for c in range(N):
            dst[N - 1 - c] += complex(0.0, src[c])

    rev_p = make_rev(P)
    rev_q = make_rev(Q)

    roots_p_f, roots_p_i = make_roots(P)
    roots_q_f, roots_q_i = make_roots(Q)

    fft2(
        mat,
        False,
        rev_p,
        rev_q,
        roots_p_f,
        roots_p_i,
        roots_q_f,
        roots_q_i,
    )

    # Recover FFT(image) * FFT(reversed_template) from
    # one packed transform FFT(image + i * reversed_template).
    #
    # For Z = A + iB:
    # A_k = (Z_k + conj(Z_-k)) / 2
    # B_k = (Z_k - conj(Z_-k)) / (2i)
    #
    # Process conjugate-frequency pairs together so that the
    # original spectrum is never overwritten before it is needed.

    for r in range(P):
        rr = (-r) % P

        for c in range(Q):
            cc = (-c) % Q

            idx = r * Q + c
            ridx = rr * Q + cc

            if idx > ridx:
                continue

            z = mat[r][c]
            zn = mat[rr][cc].conjugate()

            a = (z + zn) * 0.5
            b = (z - zn) * (-0.5j)

            product = a * b

            mat[r][c] = product

            if idx != ridx:
                mat[rr][cc] = product.conjugate()

    fft2(
        mat,
        True,
        rev_p,
        rev_q,
        roots_p_f,
        roots_p_i,
        roots_q_f,
        roots_q_i,
    )

    best_x = 0
    best_y = 0
    best = None

    for y in range(H - M + 1):
        for x in range(W - N + 1):
            window_square = rect_sum(
                pref,
                y,
                x,
                y + M,
                x + N,
            )

            corr = int(round(mat[y + M - 1][x + N - 1].real))

            ssd = window_square - 2 * corr + template_square

            if best is None or ssd < best:
                best = ssd
                best_x = x
                best_y = y

    return f"{best_x} {best_y}"

if __name__ == "__main__":
    print(solve())
```Giai đoạn đầu vào chuyển đổi mọi mã thông báo thập lục phân với`int(token, 16)`. Điều này tốt hơn là xử lý thủ công các chữ số và chữ cái, đồng thời nó cũng chấp nhận chữ hoa hoặc chữ thường thập lục phân. 

Cấu trúc tiền tố lưu trữ thêm một hàng và cột. Một hình chữ nhật có ranh giới nửa mở`[y1, y2) x [x1, x2)`sau đó được phục hồi với bốn lần truy cập. Sử dụng tọa độ nửa mở sẽ tránh được các trường hợp đặc biệt ở hàng và cột đầu tiên. 

Kích thước FFT dựa trên kích thước tích chập đầy đủ, không chỉ dựa trên kích thước hình ảnh gốc. Nếu phần đệm quá nhỏ, FFT sẽ tính toán tích chập tuần hoàn và các giá trị từ một phía của mảng sẽ bao quanh phía bên kia. 

Mẫu được đặt ở tọa độ đảo ngược vì tích chập sử dụng hạt nhân theo hướng ban đầu trong khi tương quan yêu cầu hạt nhân phải đảo ngược. Hệ số tại hàng`y + M - 1`và cột`x + N - 1`do đó tương ứng với mẫu được đặt tại`(x, y)`. 

Phần FFT được đóng gói là phần phức tạp nhất trong quá trình triển khai. Nếu như`Z`là sự biến đổi của`A+iB`, thì phép biến đổi của`A`có thể được phục hồi từ`Z[k]`và liên hợp của`Z[-k]`. Cặp tương tự mang lại sự biến đổi của`B`. Việc xử lý cả hai vị trí tần số cùng nhau sẽ ngăn không cho một giá trị được chuyển đổi bị ghi đè trước khi đối tác liên hợp của nó được đọc. 

Các số nguyên của Python không bị tràn nên biểu thức SSD cuối cùng vẫn an toàn mặc dù giá trị tối đa của nó là khoảng (255^2\cdot786432), lớn hơn (2^{32}). Bản thân FFT sử dụng số phức dấu phẩy động, nhưng mối tương quan mong muốn là số nguyên. Làm tròn hệ số thực cuối cùng sẽ lấy lại số nguyên đó một cách chính xác trong phạm vi giá trị đã cho. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hình ảnh là```
00 FF 12
AA BB 34
```và mẫu là```
FF 11
```Có thể có hai vị trí nằm ngang và chỉ có một vị trí thẳng đứng. 

| x | y | Cửa sổ | Tương quan | Cửa sổ (\tổng I^2) | SSD | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 |`00 FF`| (0\cdot255+255\cdot17) | (0^2+255^2) | 121669 |`(0,0)`| 
| 1 | 0 |`FF 12`| (255\cdot255+18\cdot17) | (255^2+18^2) | 1 |`(1,0)`| 

Tại`(1,0)`, pixel đầu tiên khớp chính xác và pixel thứ hai chỉ khác nhau một, vì vậy SSD là (1). Thuật toán thu được mối tương quan tương tự từ tích chập và chọn`1 0`, phù hợp với mẫu 

### Mẫu 2 

Có sáu vị trí hợp lệ vì hình ảnh là (4\times5) và mẫu là (3\times3). 

| x | y | SSD | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 0 | 82038 |`(0,0)`| 
| 1 | 0 | 72104 |`(1,0)`| 
| 0 | 1 | 85314 |`(1,0)`| 
| 1 | 1 | 88380 |`(1,0)`| 
| 0 | 2 | 83249 |`(1,0)`| 
| 1 | 2 | 105273 |`(1,0)`| 

Điểm nhỏ nhất là ở`(1,0)`. Ví dụ này chứng minh tại sao chỉ tối đa hóa mối tương quan sẽ không phải là sự thay thế hợp lệ cho việc giảm thiểu SSD. Số hạng (\sum I^2) thay đổi giữa các cửa sổ, do đó phải bao gồm cả năng lượng cửa sổ và mối tương quan. 

## Phân tích độ phức tạp 

hãy để 

[ 
P=2^{\lceil\log_2(H+M-1)\rceil} 
] 

và 

[ 
Q=2^{\lceil\log_2(W+N-1)\rceil}. 
] 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(PQ(\log P+\log Q)+WH+NM)) | FFT hai chiều cộng với tổng tiền tố và xử lý đầu vào | 
| Không gian | (O(PQ+WH)) | Tổng tiền tố hình ảnh và mảng FFT phức tạp được đệm | 

Với kích thước tối đa, cả hai kích thước đệm tối đa là 2048, vì vậy (P Q\le 2048^2). Giải pháp dự định phù hợp thoải mái với giới hạn bộ nhớ rộng rãi 1024 MB, trong khi FFT giảm tính toán tương quan từ hàng tỷ phép nhân pixel trực tiếp đến các phép biến đổi miền tần số. Vấn đề được công bố đưa ra giới hạn 4 giây, do đó việc triển khai cần một FFT lặp thay vì đệ quy và được hưởng lợi đáng kể từ việc đóng gói hai đầu vào thực vào một biến đổi phức tạp. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm dưới đây giả định`solve()`chức năng từ giải pháp có sẵn. Trường hợp kích thước tối đa được tạo theo chương trình thay vì nhúng hàng trăm nghìn pixel đầu vào.```python
# helper: run solution on input string, return output string
import sys
import io

# Assume solve() is imported from the submitted solution.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve().strip()
    finally:
        sys.stdin = old_stdin

# Sample 1
sample1 = """\
3 2
00 FF 12
AA BB 34
2 1
FF 11
"""
assert run(sample1) == "1 0", "sample 1"

# Sample 2
sample2 = """\
4 5
89 4E 72 C6
C7 E9 EA 8F
6E B1 FD E4
7C 22 6C D0
93 FB DB E5
3 3
79 C0 51
B9 98 37
BB 64 7F
"""
assert run(sample2) == "1 0", "sample 2"

# Minimum-size input.
minimum = """\
1 1
00
1 1
00
"""
assert run(minimum) == "0 0", "minimum size"

# All positions have the same SSD.
all_equal = """\
3 2
07 07 07
07 07 07
2 1
07 07
"""
assert run(all_equal) == "0 0", "all equal values"

# The unique optimum is the bottom-right position.
bottom_right = """\
3 3
00 00 00
00 00 00
00 00 2A
1 1
2A
"""
assert run(bottom_right) == "2 2", "bottom-right boundary"

# Maximum-size dimensions, all zeros.
# Every position is optimal, and the scan should return 0 0.
W, H = 1024, 768
N, M = 1024, 768

image_rows = "\n".join(
    " ".join(["00"] * W)
    for _ in range(H)
)
template_rows = "\n".join(
    " ".join(["00"] * N)
    for _ in range(M)
)

maximum = (
    f"{W} {H}\n"
    f"{image_rows}\n"
    f"{N} {M}\n"
    f"{template_rows}\n"
)

assert run(maximum) == "0 0", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 00 / 1 1 / 00`|`0 0`| Kích thước tối thiểu và vị trí pháp lý duy nhất | 
| MỘT`3 x 2`hình ảnh chứa đầy`07`, với một`2 x 1`mẫu chứa đầy`07`|`0 0`| Nhiều vị trí tối ưu và không có SSD | 
| MỘT`3 x 3`hình ảnh chỉ có pixel dưới cùng bên phải bằng`2A`, với một`1 x 1`bản mẫu`2A`|`2 2`| Bao gồm ranh giới bên phải và bên dưới | 
|`1024 x 768`hình ảnh hoàn toàn bằng không và mẫu hoàn toàn bằng không có kích thước bằng nhau |`0 0`| Kích thước tối đa, mức sử dụng bộ nhớ và phần đệm lớn | 
| Mẫu 1 |`1 0`| Phân tích thập lục phân và tối ưu duy nhất | 
| Mẫu 2 |`1 0`| Kết hợp hai chiều chung | 

## Vỏ cạnh 

Khi mẫu có cùng kích thước với hình ảnh, FFT vẫn hoạt động nhưng chỉ có một ứng cử viên. Vì```
1 1
7
1 1
7
```tích chập đệm chứa mối tương quan cần thiết tại`(0,0)`, tổng tiền tố là (7^2), tổng bình phương mẫu là (7^2) và SSD trở thành 0. Quá trình quét có đúng một lần lặp và in`0 0`. 

Đối với mẫu một pixel, mối tương quan giảm xuống tích của pixel mẫu và từng pixel hình ảnh. Vì```
3 1
10 20 30
1 1
1E
```giá trị mẫu là thập lục phân`1E`, hoặc 30. Ba giá trị SSD là (400), (100) và (0), do đó thuật toán sẽ in`2 0`. Không cần trường hợp một chiều đặc biệt vì cùng một công thức tích chập xử lý nó. 

Để có một hình ảnh và mẫu hoàn toàn giống nhau, mọi vị trí pháp lý đều có thể có cùng số điểm. Với```
3 2
07 07 07
07 07 07
2 1
07 07
```các thuật ngữ tương quan và cửa sổ vuông giống hệt nhau ở mọi vị trí, khiến SSD bằng 0 ở mọi nơi. Vì quá trình quét bắt đầu lúc`(0,0)`và chỉ thay thế câu trả lời khi tìm thấy số điểm nhỏ hơn, nó sẽ in`0 0`, đó là một tối ưu hợp lệ. 

Đối với ranh giới dưới cùng bên phải,```
3 3
00 00 00
00 00 00
00 00 2A
1 1
2A
```tọa độ pháp lý là`0..2`ở cả hai chiều. Vị trí không có SSD duy nhất là`(2,2)`. Hệ số tích chập được thuật toán sử dụng nằm ở hàng`2 + 1 - 1 = 2`và cột`2 + 1 - 1 = 2`, do đó vị trí biên được đưa vào đúng một lần. 

Đối với đầu vào thập lục phân, các giá trị như`0A`,`FF`, Và`e7`đều phải được chấp nhận. của Python`int(token, 16)`xử lý tất cả chúng, do đó thuật toán không cần logic phân tích cú pháp riêng cho chữ số và chữ cái. 

Đối với kích thước tối đa, cả hình ảnh và mẫu đều có thể là (1024\times768). Tích chập yêu cầu kích thước lên tới (2047\times1535), được làm tròn thành (2048\times2048) cho FFT. Việc triển khai cố tình phân bổ biến đổi đệm ở các kích thước đó thay vì cố gắng chỉ xử lý vùng hình ảnh gốc, vì đệm 0 không đủ sẽ biến tích chập tuyến tính cần thiết thành một chu kỳ và làm hỏng các giá trị tương quan gần các ranh giới.
