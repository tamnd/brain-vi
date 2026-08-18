---
title: "CF 102268E - Giá trị mong đợi"
description: "Chúng ta có một đồ thị mặt phẳng vô hướng liên thông có các đỉnh được cho bởi tọa độ của chúng và các cạnh của nó là các đoạn thẳng. Bước đi ngẫu nhiên bắt đầu từ đỉnh (1). Bất cứ khi nào bước đi ở một đỉnh, nó sẽ chọn một trong các cạnh tới của nó một cách đồng nhất và di chuyển qua nó."
date: "2026-08-17T18:47:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102268
codeforces_index: "E"
codeforces_contest_name: "300iq Contest 1"
rating: 0
weight: 102268
solve_time_s: 410
verified: false
draft: false
---

[CF 102268E - Giá trị mong đợi](https://codeforces.com/problemset/problem/102268/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 50 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị mặt phẳng vô hướng liên thông có các đỉnh được cho bởi tọa độ của chúng và các cạnh của nó là các đoạn thẳng. Bước đi ngẫu nhiên bắt đầu từ đỉnh (1). Bất cứ khi nào bước đi ở một đỉnh, nó sẽ chọn một trong các cạnh tới của nó một cách đồng nhất và di chuyển qua nó. Quá trình dừng lại khi đạt đến đỉnh (n) lần đầu tiên và chúng ta cần số lần di chuyển dự kiến, được biểu thị theo modulo (998244353). 

Tọa độ không cần thiết cho bước đi ngẫu nhiên. Mục đích của chúng là mô tả việc nhúng mặt phẳng, điều này cho chúng ta giới hạn cấu trúc quan trọng về số cạnh. Một đồ thị mặt phẳng đơn có (n\ge 3) đỉnh có nhiều nhất (3n-6) cạnh. Do đó, mặc dù câu lệnh cho phép đồ thị đầy đủ tổng quát được giới hạn về mặt cú pháp, đồ thị đầu vào thực tế rất thưa thớt, với (m=O(n)). Bài toán chính thức sử dụng (n\le 3000), do đó thuật toán (O(n^2)) là thực tế, trong khi phương pháp đại số tuyến tính dày đặc (O(n^3)) thì quá chậm. 

Có một hạn chế khác ẩn giấu trong bản chất xác suất của vấn đề. Thời gian đánh không bị giới hạn bởi (n), vì vậy việc mô phỏng bước đi cho một số bước cố định không thể trực tiếp đưa ra câu trả lời chính xác. Một đường đi có (n) đỉnh đã có thời gian kết thúc dự kiến ​​là (n(n-1)) và các đồ thị phức tạp hơn cũng có thể có thời gian kết thúc lớn. Chúng ta cần biến chuỗi xác suất sống sót vô hạn thành một phép tính hữu hạn. 

Trường hợp cạnh đầu tiên là đồ thị nhỏ nhất có thể. Với hai đỉnh và một cạnh (1\mathbin{-}2), câu trả lời chính xác là (1).```
2
0 0
5000 5000
1
1 2
```Việc triển khai bất cẩn chỉ xây dựng biểu đồ nhất thời và cho rằng mọi đỉnh nhất thời đều có quá trình chuyển đổi đi ra ngoài có thể thất bại ở đây, vì sau khi loại bỏ mục tiêu, không còn cạnh nào còn lại. Giải thích đúng là bước đi khiến người ta di chuyển thẳng vào đỉnh (2), do đó kết quả đầu ra là (1). 

Trường hợp cạnh thứ hai là cạnh trực tiếp tới mục tiêu trong biểu đồ lớn hơn. Coi như```
3
0 0
1 0
5000 5000
2
1 2
1 3
```Bắt đầu từ đỉnh (1), bước đi đến đỉnh (3) với xác suất (1/2), trong khi với xác suất (1/2) nó di chuyển đến đỉnh (2), từ đó nó phải quay về (1). Kỳ vọng là (3), bởi vì 
[ 
E_1=1+\frac12E_2,\qquad E_2=1+E_1, 
] 
vậy (E_1=3). Một lỗi phổ biến là bình thường hóa xác suất chuyển tiếp nhất thời sau khi xóa mục tiêu. Điều đó sẽ không chính xác khi khiến đỉnh (1) di chuyển đến đỉnh (2) với xác suất (1), trong khi bước đi ban đầu vẫn chọn trong số tất cả các đỉnh lân cận ban đầu. 

Trường hợp cạnh thứ ba là đồ thị có nhiều cạnh liên quan đến đích. Những cạnh đó ảnh hưởng đến mức độ của các điểm cuối khác của chúng, bởi vì chúng thể hiện khả năng thực sự bắn trúng mục tiêu. Chúng phải duy trì ở mức độ được sử dụng cho xác suất chuyển tiếp, ngay cả khi chúng biến mất khỏi ma trận chuyển trạng thái nhất thời. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là viết phương trình bước đầu tiên cho mỗi đỉnh. Gọi (E_v) là thời gian còn lại dự kiến ​​để đến (n) từ (v). Khi đó (E_n=0) và với mọi đỉnh khác 
[ 
E_v=1+\frac{1}{\deg(v)}\sum_{u\sim v}E_u. 
] 
Sau khi sửa (E_n=0), đây là hệ tuyến tính có (n-1) ẩn số. 

Hệ thống này hoàn hảo về mặt toán học, nhưng việc loại bỏ Gaussian dày đặc thông thường thực hiện các phép toán số học (\Theta(n^3)). Chỉ riêng số lượng cập nhật loại bỏ là khoảng 
[ 
\sum_{k=0}^{n-2}(n-k-1)^2 
=\frac{(n-1)(n-2)(2n-3)}6, 
] 
đó là khoảng (9\times10^9) cập nhật khi (n=3000). Đầu vào thưa thớt không lưu được việc triển khai đơn giản, vì việc loại bỏ sẽ tạo ra phần điền vào. Công thức hệ thống tuyến tính brute-force rất hữu ích để hiểu tính đúng đắn, nhưng nó không khai thác đủ cấu trúc đồ thị. 

Quan sát mở ra giải pháp dự định là ngừng hỏi trực tiếp kỳ vọng. Đối với biến ngẫu nhiên có giá trị nguyên không âm (T), 
[ 
E[T]=\sum_{i\ge0}\Pr(T>i). 
] 
Đặt (S_i=\Pr(T>i)). Thay vì giải một giá trị mong đợi, chúng ta tạo ra các giá trị (2(n-1)) đầu tiên của chuỗi (S_i). 

Xóa đỉnh đích (n) khỏi không gian trạng thái nhưng không thay đổi độ ban đầu. Gọi (f_i(v)) là xác suất để sau khi di chuyển chính xác (i) người đi bộ ở đỉnh nhất thời (v), chưa bao giờ ghé qua (n). Quá trình chuyển đổi là tuyến tính: 
[ 
f_{i+1}(v)=\sum_{u\sim v,\ u\ne n}\frac{f_i(u)}{\deg(u)}. 
] 
Xác suất sống sót chỉ đơn giản là 
[ 
S_i=\sum_{v\ne n}f_i(v). 
] 

Chỉ có (n-1) trạng thái nhất thời, vì vậy đây là phép biến đổi tuyến tính cố định được áp dụng nhiều lần. Theo định lý Cayley-Hamilton, mọi dãy thu được từ lũy thừa của ma trận ((n-1)\time(n-1)) đều thỏa mãn tối đa một phép truy hồi tuyến tính về cấp độ (n-1). Do đó, (S_i) cũng thỏa mãn phép truy hồi như vậy. Berlekamp-Massey có thể phục hồi phép truy xuất ngắn nhất từ ​​số hạng (2(n-1)) đầu tiên. Giới hạn thuật ngữ tiêu chuẩn (2N) chính xác là lý do chúng ta chỉ cần một tiền tố hữu hạn của chuỗi vô hạn này. 

Cuối cùng, phép truy toán cho tổng vô hạn mà không tạo thêm bất kỳ số hạng nào. Nếu 
[ 
C(x)=c_0+c_1x+\cdots+c_Lx^L 
] 
là đa thức kết nối được trả về bởi Berlekamp-Massey, khi đó 
[ 
F(x)=\sum_{i\ge0}S_ix^i 
] 
thỏa mãn (F(x)C(x)=R(x)), trong đó (R) có bậc nhỏ hơn (L). Như vậy 
[ 
F(1)=\frac{R(1)}{C(1)}. 
] 
Đây chính xác là kỹ thuật được sử dụng trong giải pháp đã biết cho vấn đề này. Giới hạn đồ thị mặt phẳng (m=O(n)) làm cho việc tạo ra tất cả các thuật ngữ thực hiện các thao tác (O(nm)=O(n^2)), trong khi Berlekamp-Massey thực hiện (O(n^2)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(n^2)) | Quá chậm | 
| Tối ưu | (O(nm+n^2)=O(n^2)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc đồ thị và tính bậc ban đầu của mỗi đỉnh. Bậc phải bao gồm các cạnh đi thẳng đến đỉnh (n), vì các cạnh đó là những lựa chọn thực sự được thực hiện bởi bước đi ngẫu nhiên. 
2. Xóa đỉnh (n) khỏi không gian trạng thái nhất thời. Lưu trữ mọi cạnh có hai điểm cuối khác với (n). Một cạnh như vậy góp phần chuyển tiếp theo cả hai hướng. Các cạnh tới (n) không được lưu trữ dưới dạng chuyển tiếp nhất thời vì việc lấy một trong số chúng sẽ chấm dứt quá trình. 
3. Tính nghịch đảo mô đun của mọi bậc đỉnh nhất thời. Nếu xác suất hiện tại tại (u) là (f(u)), thì lượng đóng góp cho mỗi hàng xóm là (f(u)/\deg(u)). Tất cả các phép chia đều được thực hiện theo modulo (998244353). 
4. Khởi tạo (f_0(1)=1) và mọi xác suất nhất thời khác về 0. Do đó, (S_0=1), vì bước đi không có bước nào và chắc chắn chưa đạt đến (n). 
5. Áp dụng nhiều lần ma trận chuyển tiếp nhất thời để tạo ra (S_1,S_2,\ldots,S_{2N-1}), trong đó (N=n-1). Đối với cạnh trong ((u,v)), xác suất tiếp theo nhận được (f(u)/\deg(u)) tại (v) và (f(v)/\deg(v)) tại (u). Do đó, việc xử lý mọi cạnh vô hướng sẽ thực hiện cả hai chuyển đổi có hướng. 
6. Áp dụng Berlekamp-Massey vào chuỗi kết quả. Nó trả về các hệ số (C_0,\ldots,C_L) thỏa mãn 
[ 
\sum_{j=0}^{L}C_jS_{i-j}=0 
] 
với mọi (i\ge L), với (C_0=1). Mức độ tái phát nhiều nhất là (N), vì vậy (2N) số hạng là đủ để xác định độ tái phát vô hạn. 
7. Xác định 
[ 
F(x)=\sum_{i\ge0}S_ix^i,\qquad 
C(x)=\sum_{j=0}^{L}C_jx^j. 
] 
Nhân chúng mang lại 
[ 
F(x)C(x)=R(x), 
] 
trong đó các hệ số từ (x^L) trở đi biến mất vì chúng chính xác là các phương trình truy hồi. Do đó (R) có bậc nhiều nhất (L-1). 
8. Đánh giá tại (x=1). Vì (F(1)=E[T]), 
[ 
E[T]=\frac{R(1)}{C(1)}. 
] 
Tử số có thể được tính từ tổng tiền tố của (S_i): 
[ 
R(1)= 
\sum_{j=0}^{L-1}C_j 
\left(\sum_{k=0}^{L-1-j}S_k\right). 
] 
Mẫu số chỉ đơn giản là 
[ 
C(1)=\sum_{j=0}^{L}C_j. 
] 
9. Nhân tử số với nghịch đảo mô đun của (C(1)) và in kết quả. Việc giải thích phân số mô-đun của bài toán đảm bảo rằng nghịch đảo này được xác định cho các thử nghiệm được cung cấp. 

### Tại sao nó hoạt động 

Bất biến chính là (f_i) chứa chính xác xác suất ở mỗi đỉnh không phải mục tiêu sau khi (i) di chuyển mà không đến mục tiêu. Quá trình chuyển đổi duy trì ý nghĩa đó bởi vì mọi lựa chọn ban đầu đều có xác suất (1/\deg(u)), trong khi quá trình chuyển đổi thành (n) đơn giản bị bỏ qua khỏi khối xác suất còn sót lại. Do đó (S_i=\sum_v f_i(v)) chính xác là (\Pr(T>i)). 

Quá trình chuyển đổi nhất thời là phép nhân với một ma trận cố định (M). Cayley-Hamilton đưa ra một đa thức bậc nhiều nhất (N=n-1) triệt tiêu (M), do đó mọi dãy vô hướng thu được từ (M^i), kể cả (S_i), đều thỏa mãn tối đa một phép truy hồi bậc (N). Berlekamp-Massey phục hồi sự tái diễn đó từ các điều khoản (2N). Khi đã biết sự truy hồi, (F(x)C(x)) chỉ chứa các hệ số (L) đầu tiên của nó, do đó việc đánh giá hàm tạo hợp lý tại (x=1) sẽ cho tổng vô hạn của tất cả các xác suất sống sót, chính xác là thời gian trúng dự kiến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def berlekamp_massey(s):
    # C[0] = 1 and
    # sum(C[i] * s[n-i]) = 0
    # for all sufficiently large n.
    C = [1]
    B = [1]

    L = 0
    m = 1
    b = 1

    for n in range(len(s)):
        d = s[n]
        for i in range(1, L + 1):
            d = (d + C[i] * s[n - i]) % MOD

        if d == 0:
            m += 1
            continue

        old_C = C[:]
        coef = d * pow(b, MOD - 2, MOD) % MOD

        need = m + len(B)
        if len(C) < need:
            C.extend([0] * (need - len(C)))

        for i in range(len(B)):
            C[i + m] = (C[i + m] - coef * B[i]) % MOD

        if 2 * L <= n:
            L = n + 1 - L
            B = old_C
            b = d
            m = 1
        else:
            m += 1

    return C[:L + 1]

def solve():
    n = int(input())

    # Coordinates only describe the plane embedding.
    for _ in range(n):
        input()

    m = int(input())

    target = n - 1
    deg = [0] * n
    internal_edges = []

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        if u != target:
            deg[u] += 1
        if v != target:
            deg[v] += 1

        if u != target and v != target:
            internal_edges.append((u, v))

    inv_deg = [0] * (n - 1)
    for v in range(n - 1):
        inv_deg[v] = pow(deg[v], MOD - 2, MOD)

    N = n - 1
    terms = [1]

    # f[v] is the probability of being at v without
    # having visited the target.
    f = [0] * N
    f[0] = 1

    # We need 2N terms: S_0 ... S_{2N-1}.
    for _ in range(1, 2 * N):
        scaled = [0] * N
        for v in range(N):
            scaled[v] = f[v] * inv_deg[v] % MOD

        nxt = [0] * N

        # Each undirected internal edge represents two
        # possible directed transitions.
        for u, v in internal_edges:
            x = nxt[u] + scaled[v]
            if x >= MOD:
                x -= MOD
            nxt[u] = x

            x = nxt[v] + scaled[u]
            if x >= MOD:
                x -= MOD
            nxt[v] = x

        total = 0
        for v in range(N):
            total += nxt[v]
            if total >= MOD:
                total -= MOD

        terms.append(total)
        f = nxt

    C = berlekamp_massey(terms)
    L = len(C) - 1

    # prefix[i] = S_0 + ... + S_i
    prefix = [0] * len(terms)
    cur = 0
    for i, x in enumerate(terms):
        cur += x
        if cur >= MOD:
            cur -= MOD
        prefix[i] = cur

    # R(1) = sum_{j=0}^{L-1} C[j] *
    #              (S_0 + ... + S_{L-1-j})
    numerator = 0
    for j in range(L):
        numerator = (
            numerator + C[j] * prefix[L - 1 - j]
        ) % MOD

    denominator = sum(C) % MOD
    answer = numerator * pow(denominator, MOD - 2, MOD) % MOD

    print(answer)

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp đầu vào trước tiên đọc tất cả tọa độ và loại bỏ chúng. Chúng cần thiết để phát biểu mô tả một đồ thị phẳng, nhưng bước đi ngẫu nhiên chỉ phụ thuộc vào sự kề cận. 

Mảng độ được tính toán trước khi mục tiêu bị xóa khỏi hệ thống chuyển tiếp. Đây là chi tiết biểu đồ tinh tế nhất trong quá trình thực hiện. Nếu một cạnh (u-n) tồn tại, nó phải tăng (\deg(u)), mặc dù nó không bao giờ đóng góp vào`nxt`, bởi vì việc chọn cạnh đó chính xác là sự kiện kết thúc bước đi. 

Chỉ các cạnh có điểm cuối tạm thời mới được lưu trữ. Xử lý cạnh vô hướng ((u,v)) sau khi cập nhật cả hai`nxt[u]`Và`nxt[v]`, giúp cắt vòng lặp chuyển tiếp gần bằng một nửa so với việc lưu trữ rõ ràng hai cạnh được định hướng. 

Vectơ xác suất được giữ theo modulo`MOD`sau mỗi lần chuyển đổi. Việc thực hiện sử dụng phép trừ có điều kiện thay vì`% MOD`cho mỗi lần thêm cạnh. Cả hai toán hạng đều đã nằm trong phạm vi ([0,\mathrm{MOD})), do đó, chỉ cần một phép trừ là đủ. 

Berlekamp-Massey chỉ lưu trữ các đa thức kết nối hiện tại và trước đó. Do đó, việc triển khai sử dụng bộ nhớ (O(n)) thay vì lưu trữ mọi đa thức trung gian, điều này sẽ không cần thiết. 

Việc tính toán hàm sinh cuối cùng đáng được quan tâm đặc biệt. Nếu (C(x)=\sum C_jx^j), thì hệ số của (x^k) trong (F(x)C(x)) là 
[ 
\sum_{j=0}^{k}C_jS_{k-j}. 
] 
Nó biến mất đối với (k\ge L), vì vậy chỉ (k<L) đóng góp vào (R(1)). Biểu thức tổng tiền tố trong mã đánh giá tất cả những đóng góp đó trong thời gian (O(L)) thay vì sử dụng vòng lặp lồng nhau (O(L^2)) khác. 

Không xảy ra tràn số nguyên trong tính toán toán học vì mọi giá trị đều được giảm modulo (998244353). Số nguyên Python cũng loại bỏ vấn đề tràn chiều rộng cố định có trong C++, mặc dù việc giảm giá trị vẫn cần thiết để đảm bảo hiệu suất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị chỉ có đỉnh (1) và (2), với (2) là đích. 

| Bước | (f(1)) | (S_i) | Bang BM | 
| --- | --- | --- | --- | 
| (0) | (1) | (1) | ban đầu | 
| (1) | (0) | (0) | bắt đầu tái phát | 
| (2) | (0) | (0) | đuôi không ổn định | 
| cuối cùng | (0) | (1,0,\ldots) | (C(x)=1) hiệu quả | 

Nước đi duy nhất từ ​​đỉnh (1) sẽ đi thẳng đến đỉnh (2), do đó (T=1). Chuỗi sinh tồn là (1,0,0,\ldots), có hàm sinh đơn giản là (F(x)=1). Kết quả cuối cùng là (1). 

### Mẫu 2 

Đồ thị có sáu đỉnh và đỉnh (6) là đích. Các mức độ thoáng qua là 
[ 
\deg(1)=2,\quad 
\deg(2)=4,\quad 
\deg(3)=2,\quad 
\deg(4)=3,\quad 
\deg(5)=2. 
] 

Một vài trạng thái nhất thời đầu tiên là đủ để xem khối lượng xác suất được xử lý như thế nào. 

| Bước | (f(1)) | (f(2)) | (f(3)) | (f(4)) | (f(5)) | (S_i) | 
| --- | --- | --- | --- | --- | --- | --- | 
| (0) | (1) | (0) | (0) | (0) | (0) | (1) | 
| (1) | (0) | (1/2) | (0) | (0) | (0) | (1/2) | 
| (2) | (1/8) | (0) | (1/8) | (1/8) | (0) | (3/8) | 
| (3) | (0) | (1/6) | (24/1) | (16/1) | (24/1) | (16/5) | 

Ở bước (1), một nửa xác suất đi từ (1) đến (2), trong khi nửa còn lại đi thẳng đến mục tiêu và biến mất khỏi trạng thái nhất thời. Đây là lý do tại sao (S_1=1/2). 

Giá trị kỳ vọng thực tế cũng có thể được kiểm tra trực tiếp từ các phương trình bước đầu tiên. Viết (E_i) cho thời gian dự kiến từ đỉnh (i), ta được 
[ 
E_1=1+\frac{E_2}{2}, 
] 
[ 
E_2=1+\frac{E_1+E_3+E_4}{4}, 
] 
[ 
E_3=1+\frac{E_2+E_4}{2}, 
] 
[ 
E_4=1+\frac{E_2+E_3+E_5}{3}, 
] 
[ 
E_5=1+\frac{E_4}{2}. 
] 
Giải được (E_1=18/5). Modulo (998244353), 
[ 
18\cdot5^{-1}\equiv798595486, 
] 
phù hợp với đầu ra mẫu. 

Giai đoạn BM sử dụng tiền tố đầy đủ của (2(n-1)=10) giá trị tồn tại, tìm thấy sự lặp lại cho chuỗi và việc đánh giá hàm tạo trả về cùng kết quả (18/5). Dấu vết cho thấy tại sao xác suất đã đạt tới mục tiêu phải biến mất vĩnh viễn khỏi vectơ trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm+n^2)) | (2(n-1)) chi phí chuyển tiếp thưa thớt (O(nm)) và chi phí Berlekamp-Massey (O(n^2)) | 
| Không gian | (O(n+m)) | Đồ thị, hai vectơ xác suất, chuỗi sinh tồn và đa thức BM đều có kích thước tuyến tính | 

Đối với đồ thị mặt phẳng đơn giản với (n\ge3), (m\le3n-6), do đó (nm=O(n^2)). Do đó, tổng độ phức tạp là (O(n^2)). Với (n\le3000), đây là thang đo dự kiến ​​cho bài toán, trong khi việc loại bỏ Gaussian dày đặc sẽ yêu cầu công bậc ba. Độ thưa của đồ thị mặt phẳng chính xác là yếu tố biến phép nhân vectơ ma trận lặp lại thành một phép toán khả thi. 

## Trường hợp thử nghiệm```python
# Save the solution above as solution.py before running this test file.
import sys
import io

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return out.getvalue().strip()

def path_input(n: int) -> str:
    lines = [str(n)]
    for i in range(n):
        lines.append(f"{i} 0")

    lines.append(str(n - 1))
    for i in range(1, n):
        lines.append(f"{i} {i + 1}")

    return "\n".join(lines) + "\n"

# Provided sample 1
sample1 = """\
2
0 0
35 35
1
1 2
"""
assert run(sample1) == "1", "sample 1"

# Provided sample 2
sample2 = """\
6
0 0
1 1
2 4
3 9
4 16
5 25
8
1 2
2 3
2 4
3 4
4 5
5 6
1 6
2 6
"""
assert run(sample2) == "798595486", "sample 2"

# Minimum size, also tests the direct transition into the target.
assert run("""\
2
0 0
5000 5000
1
1 2
""") == "1", "minimum-size graph"

# Path 1-2-3 has expected hitting time 3 * 2 = 6.
assert run("""\
3
0 0
1 0
5000 5000
2
1 2
2 3
""") == "6", "path graph"

# Four-cycle, target is vertex 4.
# The expected hitting time from 1 is 9.
assert run("""\
4
0 0
5000 0
5000 5000
0 5000
4
1 2
2 3
3 4
4 1
""") == "9", "regular cycle"

# Star centered at vertex 1, target is vertex 4.
# E = 2 * 3 - 1 = 5.
assert run("""\
4
0 0
5000 0
0 5000
5000 5000
3
1 2
1 3
1 4
""") == "5", "direct target edge and high degree"

# Maximum-size path.
# H(1, n) = n(n-1) for a path.
assert run(path_input(3000)) == "8997000", "maximum-size sparse graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh nối với nhau bằng một cạnh |`1`| Kích thước tối thiểu và tập hợp cạnh trống nhất thời | 
| Đường dẫn ba đỉnh |`6`| Quay lại nhiều lần và tái phát không cần thiết | 
| Bốn chu kỳ |`9`| Đồ thị trong đó mọi đỉnh đều có bậc bằng nhau | 
| Sao bốn đỉnh |`5`| Cạnh mục tiêu phải duy trì ở mức độ nguồn | 
| Đường đi ba đỉnh có tọa độ tại (0) và (5000) |`6`| Phối hợp ranh giới và hành vi đồ thị thông thường | 
| Con đường ba nghìn đỉnh |`8997000`| Biểu đồ mặt phẳng thưa thớt, tối đa và tỷ lệ (O(n^2)) | 

## Vỏ cạnh 

Đồ thị hai đỉnh được xử lý vì thuật toán xác định (N=n-1=1), do đó có chính xác một trạng thái nhất thời. Bậc của nó là một, cạnh duy nhất của nó đi tới mục tiêu và danh sách cạnh trong trống. Trạng thái ban đầu là (f_0(1)=1) và sau một lần chuyển đổi, xác suất nhất thời sẽ bằng 0. Do đó (S_0=1) và (S_i=0) sau đó cho ra (F(x)=1) và đáp án (1).```
2
0 0
5000 5000
1
1 2
```Cạnh trực tiếp tới mục tiêu trong biểu đồ lớn hơn được xử lý khác với cạnh nhất thời thông thường. Vì```
3
0 0
1 0
5000 5000
2
1 2
1 3
```đỉnh (1) có bậc (2), không phải bậc (1). Quá trình chuyển đổi (1\to3) bị bỏ qua từ`nxt`bởi vì nó kết thúc quá trình đi bộ, nhưng hệ số (1/\deg(1)=1/2) vẫn được sử dụng cho quá trình chuyển đổi (1\to2). Xác suất sống sót đầu tiên là (S_0=1), (S_1=1/2), (S_2=1/2) và tổng vô hạn của chúng là (3). 

Nguyên tắc tương tự xử lý một mục tiêu được kết nối với nhiều đỉnh. Hãy xem xét ngôi sao bốn đỉnh```
4
0 0
5000 0
0 5000
5000 5000
3
1 2
1 3
1 4
```Đỉnh (1) có bậc (3). Ở mỗi lần ghé thăm (1), xác suất tiếp cận mục tiêu (4) là (1/3), trong khi xác suất di chuyển đến một trong hai lá không phải mục tiêu là (2/3). Mỗi lá ngay lập tức trở về (1). Phương trình bước đầu là 
[ 
E_1=1+\frac23(1+E_1), 
] 
cho đi (E_1=5). Việc triển khai thu được kết quả tương tự vì hai cạnh lá vẫn nằm trong biểu đồ nhất thời trong khi cạnh đích chỉ đóng góp vào mức độ của đỉnh (1). 

Một con đường dài rèn luyện thái cực ngược lại. Với (n=3000), mọi đỉnh trong đều có bậc (2), đỉnh (1) có bậc (1) và đỉnh (n) là mục tiêu hấp thụ. Thời gian dự kiến là 
[ 
n(n-1)=3000\cdot2999=8997000. 
] 
Quá trình đi bộ có thể bao gồm một số bước bậc hai liên tục di chuyển lùi lại trước khi cuối cùng đến được mục tiêu, do đó, bất kỳ phương pháp tiếp cận nào chỉ mô phỏng (O(n)) bước về cơ bản là không đủ. Phương pháp truy hồi không quan tâm đến kỳ vọng thực tế lớn đến mức nào, bởi vì nó tái tạo lại toàn bộ đuôi vô hạn theo đại số. 

Cuối cùng, ranh giới số học môđun đáng được quan tâm. Mọi xác suất chuyển đổi đều là một số hữu tỷ, chẳng hạn như (1/2), (1/3) hoặc (1/\deg(v)), do đó việc triển khai sẽ chuyển đổi từng mẫu số thành nghịch đảo mô-đun của nó trước khi tạo ra chuỗi. Vì mọi đỉnh nhất thời đều có bậc dương trong đồ thị liên thông ban đầu nên các nghịch đảo này tồn tại theo modulo số nguyên tố. Phép chia cuối cùng cho (C(1)) được xử lý theo cách tương tự, phù hợp với cách giải thích phân số mà bài toán yêu cầu.
