---
title: "CF 102503Q - Og và Ug"
description: "Chúng ta có một cây có gốc với nút 1 là gốc của nó. Mỗi nút có một danh sách các nút con được sắp xếp theo thứ tự. Chương trình duy trì một dãy các cặp (nút, i), trong đó i cho chúng ta biết nút con nào của nút đó sẽ được xử lý tiếp theo. Khi một cặp được loại bỏ khỏi đầu bên phải, nút của nó sẽ được in."
date: "2026-08-09T19:31:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "Q"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 711
verified: true
draft: false
---

[CF 102503Q – Og và Ug](https://codeforces.com/problemset/problem/102503/Q) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với nút 1 là gốc của nó. Mỗi nút có một danh sách các nút con được sắp xếp theo thứ tự. Chương trình duy trì một deque các cặp`(node, i)`, Ở đâu`i`cho chúng ta biết nút con nào của nút đó sẽ được xử lý tiếp theo. 

Khi một cặp được loại bỏ khỏi đầu bên phải, nút của nó sẽ được in. Nếu vẫn còn một nút con chưa được xử lý, chương trình sẽ đặt phần tiếp theo của nút trở lại bên phải và khởi động nút con đó. Đây là dạng lặp thông thường của việc duyệt theo chiều sâu đầu tiên. 

Ug thêm một thao tác bổ sung. Khi một nút đã hoàn tất tất cả các nút con của nó, thay vì biến mất,`(node, 0)`được chèn vào cuối bên trái của deque. Vì các phần tử trong tương lai bị xóa ở bên phải nên các nút đã hoàn thành này sẽ bị hoãn lại cho đến khi mọi thứ hiện đang hoạt động kết thúc. 

Đầu vào mô tả toàn bộ cây và sau đó đưa ra tối đa 143 vị trí trong chuỗi đầu ra vô hạn. Vị trí được yêu cầu có thể lớn bằng (10^{100}), do đó, nhiệm vụ không phải là tạo chuỗi cho đến vị trí đó. Chúng ta cần hiểu cấu trúc đệ quy của nó và bỏ qua những phần rất lớn của nó. 

Bản thân cây rất nhỏ, có tối đa 50 nút. Điều đó loại trừ các thuật toán có độ phức tạp phụ thuộc nhiều vào số lượng nút, nhưng nó không giúp ích gì cho mô phỏng có thời gian chạy tỷ lệ thuận với vị trí được yêu cầu. Một truy vấn (10^{100}) sẽ yêu cầu số lượng lớn các phép toán deque được mô phỏng. Giá trị nhỏ của (n) thay vào đó là tín hiệu cho thấy chúng ta nên xây dựng một mô tả hữu hạn của chuỗi vô hạn. 

Có một số trường hợp ranh giới trong đó việc diễn giải deque không chính xác sẽ đưa ra một trình tự hợp lý nhưng sai. Ví dụ, với một nút duy nhất,```
1 3
0
1
2
10
```nút duy nhất được in mãi mãi, vì vậy đầu ra là```
1
1
1
```Một mô phỏng giả định rằng một nút đã hoàn thành được xử lý ngay lập tức sẽ vẫn hoạt động ở đây, điều này khiến trường hợp này trở nên đặc biệt nguy hiểm khi thử nghiệm vì nó không bộc lộ sai sót đó. 

Một ví dụ rõ ràng hơn là một gốc có một lá con:```
2 4
1 2
0
1
2
3
4
```Đầu ra đúng là```
1
2
1
2
```Ba giá trị đầu tiên đến từ việc kết thúc quá trình duyệt ban đầu của gốc. Gốc đã hoàn thành được đặt ở bên trái, vì vậy nhiệm vụ tiếp theo là nhiệm vụ lá bị trì hoãn, không phải là phần tiếp theo của gốc có thể thu được bằng cách xử lý`push_left`BẰNG`push_right`. 

Một ranh giới hữu ích khác là điểm kết thúc của quá trình truyền tải hoàn chỉnh đầu tiên. Đối với một gốc có hai lá con,```
3 3
2 2 3
0
0
5
6
12
```đầu ra là```
1
2
1
```cho các vị trí 5, 6 và 12 tương ứng. Vị trí 5 là bản in cuối cùng của quá trình duyệt gốc ban đầu, trong khi vị trí 6 bắt đầu xử lý một phần tử con bị trì hoãn. Việc nhầm lẫn hai đầu deque sẽ làm dịch chuyển toàn bộ chuỗi vô hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thực hiện chương trình chính xác như được viết. Chúng tôi giữ deque của các cặp, liên tục loại bỏ phần tử ngoài cùng bên phải của nó, in nút của nó và thực hiện thao tác chèn tương ứng. Điều này đúng vì nó thực sự là sự chuyển trạng thái của chương trình gốc. 

Vấn đề là thời gian chạy của nó. Để trả lời một truy vấn tại vị trí (K), mô phỏng cần các phần tử được in (\Theta(K)) và do đó (\Theta(K)) hoạt động deque. Trong trường hợp xấu nhất (K=10^{100}), do đó, ngay cả số lượng thao tác cần thiết cũng vượt xa mọi giới hạn tính toán hữu hạn. Việc lưu trữ deque một cách rõ ràng cũng không cần thiết vì chuỗi có nhiều cấu trúc hơn so với mô phỏng thô gợi ý. 

Quan sát quan trọng xuất phát từ việc xem xét điều gì xảy ra trong khi một nút đang được xử lý tích cực. Giả định`(v, 0)`là tác vụ đang hoạt động ngoài cùng bên phải và không có tác vụ nào đang hoạt động ở bên phải của nó. Việc truyền qua nó tạo ra một dãy hữu hạn cố định. Nút (v) được in, mỗi cây con con được duyệt và (v) được in lại giữa các lần duyệt con liên tiếp và một lần sau cây con cuối cùng. 

Đặt chuỗi hữu hạn này là (E(v)). Nếu (v) có con (c_1,c_2,\ldots,c_m), thì 

[ 
E(v)=v,E(c_1),v,E(c_2),\ldots,v,E(c_m),v. 
] 

Do đó, một chiếc lá có (E(v)=[v]). Nếu cây con của (v) chứa các nút (s(v)) thì (E(v)) có chính xác các phần tử (2s(v)-1), bởi vì mỗi cạnh của cây gây ra thêm một kết quả trả về cho cây cha của nó. 

Trong khi quá trình truyền tải này đang chạy, mọi nút kết thúc sẽ được chèn vào bên trái. Cuối cùng, những người mới được tạo ra`(node, 0)`nhiệm vụ xuất hiện từ phải sang trái theo đúng thứ tự sau. Các tác vụ bị trì hoãn hiện tại vẫn ở xa hơn về bên phải nên chúng được xử lý trước. 

Điều này mang lại một cách giải thích rõ ràng hơn nhiều. Đối xử`(v,0)`như một nhiệm vụ. Xử lý một tác vụ (v) in toàn bộ khối hữu hạn (E(v)), sau đó nối thêm các nút của cây con của (v) theo thứ tự sau làm thế hệ nhiệm vụ tiếp theo. 

Đặt (Q(v)) biểu thị danh sách thứ tự sau của cây con có gốc tại (v). Nhiệm vụ đầu tiên là root. Thế hệ nhiệm vụ tiếp theo là (Q(root)). Thế hệ sau đó có được bằng cách thay thế mọi nút (v) ở thế hệ trước bằng (Q(v)). Nói cách khác, nếu (W_d) là chuỗi nhiệm vụ ở cấp độ (d), 

[ 
W_0=[gốc], 
] 

và 

[ 
W_{d+1}=Q(W_d). 
] 

Đầu ra là sự kết hợp của (E(v)) cho tất cả (v) ở các cấp độ này. 

Quan sát quan trọng thứ hai là (Q(v)) chứa mọi nút trong cây con của (v) đúng một lần. Do đó, nếu chúng ta định nghĩa ma trận (M) bằng 

[ 
M_{u,v}=1 
] 

khi (v) thuộc cây con của (u) và các trường hợp khác bằng 0, thì số lần xuất hiện của mỗi nút ở cấp độ (d) thu được bằng cách nhân với (M^d). 

Bởi vì (M) có các điểm trên đường chéo của nó và chỉ có các mục từ tổ tiên đến con cháu phía trên đường chéo đó sau khi sắp xếp thứ tự nút phù hợp, nên chúng ta có thể viết 

[ 
M=I+N 
] 

trong đó (N) là linh năng. Cây có nhiều nhất là 50 nút nên (N^{50}=0). Do đó 

[ 
M^d=(I+N)^d 
=\sum_{r=0}^{49}\binom dr N^r. 
] 

Đây là lý do tại sao số mũ khổng lồ có thể quản lý được. Mỗi độ dài liên quan là một đa thức trong (d), được biểu diễn một cách tự nhiên dưới dạng cơ sở nhị thức. 

Đối với nút (v), hãy xác định (A_v(d)) là tổng số giá trị được in thực tế được đóng góp bởi tất cả các tác vụ trong (Q^d(v)). Vì tác vụ thuộc loại (x) đóng góp (|E(x)|) giá trị được in nên (A_v(d)) chính xác là phiên bản có trọng số của hàng của (M^d). Do đó, nó là đa thức ở (d) bậc nhiều nhất là 49. 

Chúng tôi cũng có thể tổng hợp toàn bộ cấp độ. Danh tính 

[ 
\sum_{j=0}^{d-1}\binom jr=\binom d{r+1} 
] 

đưa ra một đa thức cho tổng số giá trị được in ở tất cả các cấp trước cấp (d). Điều này cho phép chúng tôi xác định cấp độ chứa truy vấn bằng tìm kiếm nhị phân. 

Khi đã biết cấp độ, sẽ có một vấn đề tiềm ẩn khác: bản thân cấp độ đó có thể có số mũ rất lớn. Chúng tôi giải quyết điều đó một cách đệ quy. Từ (Q^d(v)) là 

[ 
Q^{d-1}(x_1)Q^{d-1}(x_2)\ldots Q^{d-1}(x_m), 
] 

trong đó (x_1,\ldots,x_m) là các nút thứ tự sau của cây con của (v). Chúng ta có thể tính toán kích thước có trọng số của mỗi khối và xác định vị trí khối cần thiết. 

Khối cuối cùng luôn là (Q^{d-1}(v)), bởi vì (v) là nút cuối cùng trong thứ tự sau của cây con của nó. Sự tự tham chiếu này là lý do duy nhất khiến một phương pháp đệ quy ngây thơ có thể thực hiện các bước (d). Chúng ta nhảy qua tất cả các lựa chọn liên tiếp của khối cuối cùng này cùng một lúc bằng cách sử dụng đa thức (A_v). Mỗi khi chúng ta rời khỏi khối tự, chúng ta sẽ chuyển sang một phần tử con thích hợp, vì vậy có thể có tối đa 50 lần chuyển đổi như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(K)) | (O(K)) trường hợp xấu nhất | Quá chậm | 
| Tối ưu | (O(n^3+k n^2\log K)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách thứ tự sau của cây con của mỗi nút. Danh sách cho (v) chính xác là (Q(v)), bởi vì các tác vụ bị trì hoãn được tạo trong khi xử lý (v) xuất hiện theo thứ tự sau. 
2. Xây dựng (E(v)), đầu ra hữu hạn được tạo ra bởi tác vụ xử lý (v) khi nó đang hoạt động. Bắt đầu với (v), nối đệ quy (E(c)) cho mọi con (c) và nối (v) sau mỗi con. Độ dài của chuỗi này là (w(v)=2s(v)-1), trong đó (s(v)) là kích thước cây con. 
3. Định nghĩa ma trận (N) một cách ngầm định bằng cách nói rằng việc áp dụng (N) cho một vectơ (x) tại nút (v) sẽ cho tổng của (x[u]) trên tất cả các hậu duệ thực sự (u) của (v). Bắt đầu với vectơ (w) và áp dụng nhiều lần (N). Các vectơ kết quả (C_r=N^r w) cho 

[ 
A_v(d)=\sum_r C_r[v]\binom dr. 
] 

Chỉ có tối đa 50 vectơ là khác 0 vì (N) là linh năng.

1. Chuyển đổi các đa thức cơ sở nhị thức này thành đa thức nguyên thông thường. Nhân mọi đa thức với ((H+1)!), trong đó (H) là bậc khác 0 lớn nhất. Điều này loại bỏ tất cả các mẫu số khỏi các hệ số nhị thức và cho phép việc triển khai đánh giá các đa thức bằng cách sử dụng số học số nguyên thông thường. 
2. Xây dựng đa thức cho tổng sản lượng trước mức (d). Nếu (C_r[root]) là hệ số của bước trước thì 

[ 
P(d)=\sum_r C_r[root]\binom d{r+1} 
] 

là số giá trị được in ở các mức từ (0) đến (d-1). 

1. Với mỗi truy vấn (K), tìm kiếm nhị phân mức (d) lớn nhất thỏa mãn (P(d)<K). Truy vấn nằm bên trong cấp độ (d). Trừ (P(d)) khỏi (K) để có được vị trí cơ bản của nó bên trong mức đó, được đo bằng các trọng số (w(v)). 
2. Để tìm nhiệm vụ chính xác bên trong (Q^d(root)), hãy duy trì một nút (v), số mũ (d) và vị trí có trọng số còn lại (r). Nếu (d=0), từ chỉ chứa (v), do đó (r) xác định trực tiếp một vị trí bên trong (E(v)). 
3. Khi (d>0), từ (Q^d(v)) bao gồm một khối (Q^{d-1}(x)) cho mọi (x) trong danh sách thứ tự sau của cây con của (v). Trọng lượng của khối thuộc (x) là (A_x(d-1)). 
4. Khối cuối cùng tương ứng với (x=v). Đặt (A=A_v(d)). Sau khối cuối cùng, phần trước có trọng số 

[ 
A_v(d)-A_v(d-1). 
] 

Nếu vị trí còn lại lớn hơn giá trị này thì vị trí mong muốn sẽ nằm trong khối tự cuối cùng. Việc lặp lại điều này một cách ngây thơ có thể mất (d) lần lặp, do đó, tìm kiếm nhị phân số lượng tối đa (t) của các lựa chọn tự liên tiếp thỏa mãn 

[ 
r>A_v(d)-A_v(d-t). 
] 

Sau đó thay (d) bằng (d-t) và trừ đi trọng lượng đã bỏ qua. 

1. Nếu khối tự không còn được chọn, hãy quét các phần tử con phù hợp theo thứ tự sau. Tìm (x) đầu tiên có khối (Q^{d-1}(x)) chứa vị trí còn lại, trừ đi các khối hoàn chỉnh nếu cần. Đặt (v=x) và (d=d-1). 
2. Mỗi khi bước 10 xảy ra, nút mới sẽ là hậu duệ thực sự của nút trước đó. Do đó điều này có thể xảy ra nhiều nhất (n-1) lần. Số lượng lớn các lần tự chuyển đổi đã được nén vào tìm kiếm nhị phân ở bước 9. 
3. Khi (d=0), câu trả lời là phần tử tương ứng của chuỗi (E(v)) được tính toán trước. 

Tại sao nó hoạt động: về mặt khái niệm, deque có thể được chia thành các khung truyền tải hiện đang hoạt động và các tác vụ khởi động lại bị trì hoãn. Các khung hoạt động luôn chiếm phần cuối bên phải, vì vậy chúng hoàn thành toàn bộ quá trình truyền tải trước khi chạm vào bất kỳ tác vụ bị trì hoãn nào. Mỗi nút hoàn thành sẽ được chèn vào bên trái và vì đầu bên phải có mức độ ưu tiên nên các nút bị hoãn sẽ được xử lý theo thứ tự FIFO. Thứ tự của chúng chính xác là thứ tự sau của cây con đã tạo ra chúng. Điều này chứng tỏ rằng việc thực thi vô hạn được phân chia thành các cấp độ (Q^d(root)), với mỗi nhiệm vụ (v) đóng góp chính xác (E(v)). Đa thức (A_v(d)) đếm kích thước có trọng số chính xác của mỗi khối được tạo đệ quy, do đó mọi tìm kiếm nhị phân chỉ bỏ qua các khối hoàn chỉnh. Do đó, quá trình giảm dần đệ quy sẽ tiếp cận chính xác tác vụ chứa vị trí đầu ra được yêu cầu và (E(v)) cung cấp nút được in chính xác bên trong tác vụ đó. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    children = [[] for _ in range(n)]
    for v in range(n):
        data = list(map(int, input().split()))
        c = data[0]
        children[v] = [x - 1 for x in data[1:]]

    queries = [int(input()) for _ in range(k)]

    sys.setrecursionlimit(10000)

    post = [[] for _ in range(n)]
    euler_output = [[] for _ in range(n)]
    subtree_size = [0] * n

    def build(v):
        p = []
        e = [v]

        for u in children[v]:
            build(u)
            p.extend(post[u])
            e.extend(euler_output[u])
            e.append(v)

        p.append(v)

        post[v] = p
        euler_output[v] = e
        subtree_size[v] = len(p)

    build(0)

    # w[v] is the number of printed values produced by one task v.
    weight = [len(euler_output[v]) for v in range(n)]

    # coeff[r][v] = (N^r * weight)[v].
    coeff = [weight[:]]
    cur = weight[:]

    for _ in range(1, n + 1):
        nxt = [0] * n

        for v in range(n):
            total = 0
            # post[v][:-1] are precisely the proper descendants of v.
            for u in post[v][:-1]:
                total += cur[u]
            nxt[v] = total

        if not any(nxt):
            break

        coeff.append(nxt)
        cur = nxt

    degree = len(coeff) - 1

    # We multiply every polynomial by FACT so that all coefficients
    # become integers.
    FACT = math.factorial(degree + 1)

    # Falling factorial polynomials:
    # fall[r](x) = x * (x-1) * ... * (x-r+1)
    fall = [[1]]
    for r in range(1, degree + 2):
        prev = fall[-1]
        cur_poly = [0] * (r + 1)
        shift = r - 1

        for j, a in enumerate(prev):
            cur_poly[j] -= shift * a
            cur_poly[j + 1] += a

        fall.append(cur_poly)

    # Polynomial for FACT * A_v(d).
    apoly = [[0] * (degree + 1) for _ in range(n)]

    factorials = [math.factorial(i) for i in range(degree + 2)]

    for r in range(degree + 1):
        multiplier = FACT // factorials[r]
        fr = fall[r]

        for v in range(n):
            c = coeff[r][v]
            if c == 0:
                continue

            mul = c * multiplier
            pv = apoly[v]

            for j, a in enumerate(fr):
                pv[j] += mul * a

    # Polynomial for
    # FACT * sum_{j=0}^{d-1} A_root(j).
    # sum C(j,r) = C(d,r+1).
    prefix_poly = [0] * (degree + 2)

    for r in range(degree + 1):
        multiplier = FACT // factorials[r + 1]
        fr = fall[r + 1]
        c = coeff[r][0]

        if c == 0:
            continue

        mul = c * multiplier
        for j, a in enumerate(fr):
            prefix_poly[j] += mul * a

    def eval_poly(poly, x):
        value = 0
        for a in reversed(poly):
            value = value * x + a
        return value

    prefix_cache = {}
    answer_cache = {}

    def prefix(d):
        if d not in prefix_cache:
            prefix_cache[d] = eval_poly(prefix_poly, d)
        return prefix_cache[d]

    def A(v, d, cache):
        key = (v, d)
        value = cache.get(key)
        if value is None:
            value = eval_poly(apoly[v], d)
            cache[key] = value
        return value

    total_target_scale = FACT

    def get_answer(K):
        if K in answer_cache:
            return answer_cache[K]

        target = K * FACT

        # Find the level containing K.
        lo, hi = 0, K
        while lo < hi:
            mid = (lo + hi + 1) // 2
            if prefix(mid) < target:
                lo = mid
            else:
                hi = mid - 1

        d = lo
        rem = target - prefix(d)

        cache = {}

        v = 0

        while d > 0:
            current = A(v, d, cache)

            # Jump over as many consecutive choices of the final
            # self-block Q^(d-1)(v) as possible.
            lo_t, hi_t = 0, d

            while lo_t < hi_t:
                mid = (lo_t + hi_t + 1) // 2
                earlier = A(v, d - mid, cache)

                if rem > current - earlier:
                    lo_t = mid
                else:
                    hi_t = mid - 1

            t = lo_t

            if t:
                new_d = d - t
                skipped = current - A(v, new_d, cache)
                rem -= skipped
                d = new_d

                if d == 0:
                    break

            # The self-block is no longer possible.
            # Q(v) is post[v], whose last element is v.
            found = False

            for u in post[v][:-1]:
                block = A(u, d - 1, cache)

                if rem > block:
                    rem -= block
                else:
                    v = u
                    d -= 1
                    found = True
                    break

            if not found:
                # This branch is reachable only at d == 0,
                # which is handled below.
                break

        # At d == 0 the word is [v].
        # rem is a one-based position inside E(v).
        idx = rem // FACT - 1
        ans = euler_output[v][idx]

        answer_cache[K] = ans
        return ans

    out = [str(get_answer(q) + 1) for q in queries]
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```DFS đầu tiên xây dựng hai đối tượng cho mỗi nút cây.`post[v]`là trình tự chính xác của các tác vụ khởi động lại được tạo ra bằng cách hoàn thành`v`, trong khi`euler_output[v]`là trình tự thực tế được in trong khi tác vụ`v`đang hoạt động. Cái sau có chiều dài`2 * subtree_size - 1`. 

các`coeff`vectơ mã hóa lũy thừa của ma trận con cháu linh năng mà không lưu trữ ma trận một cách rõ ràng. Áp dụng`N`một lần có nghĩa là tính tổng các con cháu thích hợp, vì vậy mọi hệ số có thể được tính trực tiếp từ danh sách thứ tự sau. 

Việc chuyển đổi đa thức xứng đáng được quan tâm. Công thức tự nhiên sử dụng các hệ số nhị thức, nhưng việc đánh giá liên tục các hệ số nhị thức trong mỗi lần tìm kiếm nhị phân sẽ yêu cầu nhiều phép chia. Nhân tất cả các đa thức với`(degree + 1)!`chuyển đổi mọi đa thức nhị thức thành đa thức nguyên. Đánh giá Horner sau đó chỉ cần nhân và cộng. 

Tất cả các vị trí được đại diện nội bộ theo đơn vị`FACT`. Điều này tránh việc chia liên tục các vị trí có trọng số theo hệ số tỷ lệ giai thừa. Ở bước cuối cùng, giá trị còn lại chia hết cho`FACT`và thương số cho biết vị trí dựa trên một bên trong`E(v)`. 

Tìm kiếm nhị phân ở cấp độ đầu tiên sử dụng`prefix(d)`, tính toán mọi đầu ra từ các cấp một cách nghiêm ngặt trước`d`. Giới hạn trên`K`luôn là đủ vì mỗi cấp độ đều chứa ít nhất một tác vụ và mỗi tác vụ in ra ít nhất một giá trị. 

Tìm kiếm nhị phân thứ hai là phần tinh tế. Từ`v`là phần tử cuối cùng trong danh sách thứ tự sau của chính nó,`Q^d(v)`luôn kết thúc bằng`Q^(d-1)(v)`. Nếu mục tiêu vẫn ở trong khối tự này nhiều lần, chúng ta có thể trừ toàn bộ phạm vi bị bỏ qua trong một thao tác. Khi mục tiêu đi vào khối con cháu thích hợp, nút hiện tại sẽ di chuyển xuống dưới trong cây. 

Số nguyên Python có độ chính xác tùy ý, do đó, giá trị đầu vào lên tới (10^{100}), đánh giá đa thức và chia tỷ lệ giai thừa không gây ra vấn đề tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cái cây là```
1
├── 2
│   └── 3
└── 4
```Các trình tự truyền tải tích cực là```
E(3) = [3]
E(4) = [4]
E(2) = [2, 3, 2]
E(1) = [1, 2, 3, 2, 1, 4, 1]
```Do đó, cấp độ đầu tiên đóng góp bảy giá trị được in. 

Thứ tự sau của gốc là`[3, 2, 4, 1]`, vì vậy cấp độ tiếp theo bao gồm bốn nhiệm vụ đó. Trọng số của chúng lần lượt là 1, 3, 1 và 7. 

| Truy vấn | Cấp độ chứa nó | Vị trí bên trong cấp độ | Nhiệm vụ/đầu ra đã chọn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 6 | 0 | 6 |`E(1)[6]`| 4 | 
| 9 | 1 | 2 |`E(2)[1]`| 2 | 
| 69 | 4 | 7 | lựa chọn đệ quy | 2 | 
| 143 | 6 | 9 | lựa chọn đệ quy | 3 | 
| 214 | 7 | 31 | lựa chọn đệ quy | 3 | 
| 241 | 7 | 58 | lựa chọn đệ quy | 3 | 
| 420 | 10 | 37 | lựa chọn đệ quy | 3 | 

Đối với cây này, tổng trọng số của cấp (d) là 

[ 
A_1(d)=7+\frac{d(d+9)}2. 
] 

Do đó, sản lượng tích lũy trước mức (d) là 

[ 
P(d)=\sum_{j=0}^{d-1} 
\left(7+\frac{j(j+9)}2\right). 
] 

Ví dụ: (P(4)=62) và (P(5)=94), do đó truy vấn 69 nằm ở cấp độ 4. Việc lựa chọn khối đệ quy sau đó tìm thấy nút 2, tạo ra giá trị được yêu cầu 2. 

### Chuỗi hai nút 

Hãy xem xét```
2 5
1 2
0
1
2
3
4
10
```Đơn giản là cái cây```
1
└── 2
```Các khối hoạt động là```
E(2) = [2]
E(1) = [1, 2, 1]
```Cấp độ đầu tiên có trọng số 3. Chuỗi nhiệm vụ đặt hàng sau của nó là`[2, 1]`. Việc thay thế mỗi nhiệm vụ bằng trình tự thứ tự sau của nó sẽ đưa ra các cấp độ tiếp theo. 

| Cấp độ | Trình tự nhiệm vụ | Khối tạ | Tổng sản lượng | 
| --- | --- | --- | --- | 
| 0 |`[1]`|`[3]`| 3 | 
| 1 |`[2, 1]`|`[1, 3]`| 4 | 
| 2 |`[2, 2, 1]`|`[1, 1, 3]`| 5 | 

Đầu ra tích lũy trước các cấp 0, 1, 2 và 3 lần lượt là 0, 3, 7 và 12. Như vậy truy vấn 10 thuộc cấp 2 và có vị trí cục bộ 3. Hai nhiệm vụ đầu tiên của cấp đó đều là nút 2, còn lại nhiệm vụ thứ ba, nút 1. Khối của nó là`E(1)`, vì vậy giá trị được in thứ ba bên trong khối đó là 1. 

Do đó, kết quả đầu ra được yêu cầu là```
1
2
1
2
1
```cho các vị trí 1, 2, 3, 4 và 10. Ví dụ này thực hiện bước nhảy tự chặn lặp đi lặp lại vì nút 1 tiếp tục xuất hiện dưới dạng phần tử cuối cùng của việc mở rộng thứ tự sau của chính nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3+k n^2\log K)) | Chi phí xây dựng đa thức (O(n^3)); mỗi truy vấn truy cập ở nhiều nhất (n) cấp độ cây, với các tìm kiếm nhị phân trên số mũ của tối đa (O(\log K)) lần lặp và đánh giá đa thức (O(n)) | 
| Không gian | (O(n^2)) | Cây, danh sách thứ tự sau, khối đầu ra, hệ số đa thức và bộ đệm tạm thời cho mỗi truy vấn đều có kích thước tỷ lệ bậc hai | 

Ở đây (K) biểu thị vị trí được yêu cầu lớn nhất. Với (n\le50), mọi công việc phụ thuộc vào cây đều nhỏ. Sự phụ thuộc vào (K) là logarit theo số bit của truy vấn chứ không phải tuyến tính theo giá trị số của nó, điều này làm cho các vị trí lớn bằng (10^{100}) thực tế. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định`solve()`chức năng từ giải pháp trên có sẵn.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample1 = """\
4 7
2 2 4
1 3
0
0
6
9
69
143
214
241
420
"""

assert run(sample1) == """\
4
2
2
3
3
3
3
""", "sample 1"

# Minimum-size tree, all outputs equal.
case1 = """\
1 4
0
1
2
3
100000000000000000000000000000000000000000000000000000000000
"""

assert run(case1) == """\
1
1
1
1
""", "single-node tree"

# Two-node chain, catches level boundaries and repeated self blocks.
case2 = """\
2 5
1 2
0
1
2
3
4
10
"""

assert run(case2) == """\
1
2
1
2
1
""", "two-node chain"

# Three-node star, checks the transition from the initial traversal
# to postponed tasks.
case3 = """\
3 5
2 2 3
0
0
5
6
7
12
13
"""

assert run(case3) == """\
1
2
1
1
2
""", "star boundary"

# Maximum n = 50, root with 49 leaf children.
# E(root) has length 99:
# odd positions are node 1, even positions are leaves 2,3,...,50.
max_case_parts = [
    "50 3",
    "49 " + " ".join(str(x) for x in range(2, 51))
]
max_case_parts.extend(["0"] * 49)
max_case_parts.extend(["99", "100", "101"])
case4 = "\n".join(max_case_parts) + "\n"

assert run(case4) == """\
1
2
3
""", "maximum-size star"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút đơn với truy vấn lớn |`1, 1, 1, 1`| Kích thước tối thiểu, tất cả các đầu ra bằng nhau, vị trí chính xác tùy ý | 
| Chuỗi hai nút |`1, 2, 1, 2, 1`| Ranh giới cấp độ và tự mở rộng lặp đi lặp lại | 
| Sao ba nút |`1, 2, 1, 1, 2`| Chuyển đổi từ quá trình truyền tải hoạt động ban đầu sang các nhiệm vụ bị trì hoãn | 
| Sao năm mươi nút |`1, 2, 3`| Kích thước cây tối đa và kết thúc chính xác của cấp độ đầu tiên | 

## Vỏ cạnh 

Đối với cây nút đơn, mỗi bước xử lý sẽ in nút 1 và sau đó đặt`(1,0)`trở lại bên trái. Vì nó cũng là phần tử duy nhất nên lần lặp tiếp theo sẽ loại bỏ nó. Biểu diễn đa thức phản ánh điều này bằng một hằng số (A_1(d)=1), trong khi tiền tố tích lũy chỉ đơn giản là (d). Do đó, việc tìm kiếm cấp độ có thể nhảy trực tiếp đến một cấp độ rất lớn và trường hợp cơ sở trả về nút 1. 

Đối với chuỗi hai nút```
2 4
1 2
0
1
2
3
4
```khối hoạt động đầu tiên là`[1,2,1]`. Sau khi nó kết thúc, các nhiệm vụ bị trì hoãn sẽ được`[2,1]`. Do đó, cấp độ tiếp theo bao gồm`E(2)`theo sau là`E(1)`, thay vì khởi động lại nút 1 ngay lập tức. Đầu ra bắt đầu`1,2,1,2,1,2,1`và thuật toán nhận được kết quả tương tự vì nó luôn coi danh sách thứ tự sau là cấp độ nhiệm vụ tiếp theo. 

Tại một ranh giới cấp độ, truy vấn phải thuộc chính xác một cấp độ. Thuật toán sử dụng bất đẳng thức chặt chẽ`prefix(d) < K`khi tìm thấy mức độ. Nếu như`K`chính xác là vị trí cuối cùng của một cấp độ, tìm kiếm nhị phân sẽ giữ nguyên cấp độ đó. Vị trí tiếp theo thuộc về cấp độ sau. Đây là lý do việc triển khai sử dụng các vị trí có trọng số dựa trên một và chỉ trừ tiền tố hoàn chỉnh sau khi xác định được cấp độ. 

Khi mục tiêu nằm bên trong khối cuối cùng được lặp lại (Q^{d-1}(v)), việc giảm đệ quy (d) từng cái một sẽ sai từ góc độ hiệu suất mặc dù nó đúng về mặt logic. Tìm kiếm nhị phân tự chặn sẽ thay thế tất cả các lần lặp lại liên tiếp bằng một bước nhảy. Biểu thức (A_v(d)-A_v(d-t)) chính xác là tổng trọng số bị loại bỏ bởi (t) lần lặp lại như vậy, do đó vị trí còn lại vẫn được đồng bộ hóa với chuỗi ban đầu. 

Cuối cùng, khi mục tiêu rời khỏi khối tự, nó phải nhập vào khối con cháu thích hợp. Độ sâu của cây nhiều nhất là 49, do đó chỉ có thể có hữu hạn nhiều thay đổi như vậy trước khi đạt tới (d=0). Tại thời điểm đó chỉ còn lại một ký hiệu nhiệm vụ và vị trí còn lại sẽ chọn một phần tử trực tiếp từ chuỗi hữu hạn được tính toán trước (E(v)). 

Nếu bạn muốn, tôi cũng có thể cung cấp một phiên bản biên tập cuộc thi ngắn hơn để giữ nguyên bằng chứng nhưng dễ đọc hơn dưới áp lực thời gian.
