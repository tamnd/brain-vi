---
title: "CF 102253D - Trò chơi chia"
description: "Chúng ta có (k) các cọc giống hệt nhau, mỗi cọc ban đầu chứa cùng một số nguyên rất lớn (n). Các cọc được sắp xếp theo chu kỳ. Vòng (1) vận hành cọc (0), vòng (2) vận hành cọc (1), v.v., quấn quanh cọc (k-1)."
date: "2026-08-17T21:25:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102253
codeforces_index: "D"
codeforces_contest_name: "2017 Chinese Multi-University Training, BeihangU Contest"
rating: 0
weight: 102253
solve_time_s: 193
verified: true
draft: false
---

[CF 102253D - Trò chơi phân chia](https://codeforces.com/problemset/problem/102253/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có (k) các cọc giống hệt nhau, mỗi cọc ban đầu chứa cùng một số nguyên rất lớn (n). Các cọc được sắp xếp theo chu kỳ. Vòng (1) vận hành cọc (0), vòng (2) vận hành cọc (1), v.v., quấn quanh cọc (k-1). 

Một thao tác thay thế giá trị hiện tại (x) trong một đống bằng ước số thích hợp (d) của (x), do đó (1 \le d < x). Trò chơi dừng ngay lập tức khi một số cọc trở thành (1). Với mỗi cọc (i), chúng ta cần số chuỗi thao tác hoàn chỉnh có cọc đầu tiên đạt tới (1), và do đó cọc được thay đổi cuối cùng, là (i). 

Bản thân số nguyên (n) có thể quá lớn để có thể xây dựng được. Thay vào đó, hệ số nguyên tố của nó được đưa ra là 

[ 
n=\prod_{j=1}^{m}p_j^{e_j}. 
] 

Bản thân các số nguyên tố gần như không liên quan đến tổ hợp. Điều quan trọng là vectơ số mũ ((e_1,\ldots,e_m)), bởi vì việc thay thế một số bằng một ước số chỉ đơn giản là giảm từng số mũ nguyên tố một cách độc lập. 

hãy để 

[ 
w=\sum_{j=1}^{m}e_j. 
] 

Mỗi thao tác giảm ít nhất một số mũ xuống ít nhất một, do đó, một cọc đơn có thể được vận hành nhiều nhất (w) lần. Vì (w\le 10^5), một giải pháp có vòng lặp chính là tuyến tính hoặc (O(w\log w)) là thực tế. Thuật toán bậc hai với các phép toán (10^{10}) ở giới hạn trên bị loại trừ hoàn toàn. Thực tế là chỉ có một số trường hợp thử nghiệm có (w\ge 10^4) đặc biệt hữu ích cho việc triển khai dựa trên NTT, vì các phép biến đổi đắt tiền chỉ cần thiết cho các trường hợp thực sự lớn. Giải pháp chính thức sử dụng chính xác mức giảm này và NTT theo mô đun nhất định. 

Có một số trường hợp ranh giới trong đó mô phỏng hời hợt hoặc công thức được lập chỉ mục không chính xác sẽ đưa ra câu trả lời sai. Ví dụ, với```
1 1
2 2
```chúng ta có (n=4) và một đống. Có chính xác hai chuỗi, (4\to1) và (4\to2\to1), vì vậy câu trả lời là`Case #1: 2`. Một mô phỏng chỉ xem xét chuỗi ngắn nhất sẽ bỏ lỡ khả năng thứ hai. 

Với```
1 2
2 1
```chúng ta có hai cọc và (n=2). Cọc (0) có thể ngay lập tức trở thành (1), tạo ra một chuỗi hợp lệ. Nếu cọc (1) được coi là cuối cùng thì cọc (0) trước tiên phải được thay đổi mà không trở thành (1), điều này là không thể. Câu trả lời là`Case #1: 1 0`. Xử lý các búi trĩ một cách đối xứng sẽ đưa ra câu trả lời giống nhau cho cả hai. 

Trường hợp tinh vi thứ hai là khi một số số mũ nguyên tố có thể giảm đi trong cùng một thao tác. Với (n=6=2^1 3^1), một thao tác có thể thay đổi trực tiếp (6) thành (1) hoặc nó chỉ có thể loại bỏ một thừa số nguyên tố và để lại (2) hoặc (3). Một phương pháp đếm các chuỗi cho từng số nguyên tố một cách độc lập rồi nhân chúng đã bỏ lỡ yêu cầu rằng mọi phép toán phải giảm ít nhất một số mũ, trong khi cho phép một số số mũ giảm cùng nhau. 

Cuối cùng, thuật ngữ tương ứng với 0 hoạt động trước đó phải được xử lý chính xác. Chúng ta định nghĩa (f(0)=0), bởi vì một số dương không thể trở thành (1) trong các phép toán bằng 0. Tại ranh giới còn lại, (f(w+1)=0), bởi vì hoạt động (w+1) không thể xảy ra trên một cọc. Hai giá trị biên nhân tạo này làm cho phép tính tổng cuối cùng trở nên rõ ràng và ngăn ngừa các lỗi sai sót một. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng mọi lựa chọn số chia có thể có cho mỗi cọc. Điều này đúng vì mọi phép toán hợp pháp chính xác là sự chuyển đổi từ một ước số sang một ước số thực sự nhỏ hơn. Vấn đề là số lượng chuỗi. Hãy xem xét đầu vào đặc biệt đơn giản (n=2^w). Chuỗi từ (2^w) đến (1) được xác định bởi số mũ trung gian nào (1,2,\ldots,w-1) xuất hiện. Mỗi tập hợp con cho một chuỗi giảm nghiêm ngặt, do đó có chính xác (2^{w-1}) chuỗi cho chỉ một cọc. Với (w=10^5), đó là (2^{99999}) khả năng. Lực lượng vũ phu đã không thể thực hiện được trước khi xem xét nhiều cọc. 

Quan sát quan trọng là quên các giá trị nguyên tố thực tế và mô tả hoàn toàn một cọc bằng vectơ số mũ của nó. Giả sử một cọc được thay đổi đúng (x) lần trước khi đạt đến (1). Gọi (d_{r,j}) là số mũ (e_r) bị loại bỏ trong quá trình thực hiện phép tính (j). Sau đó 

[ 
\sum_{j=1}^{x}d_{r,j}=e_r 
] 

với mọi số nguyên tố (r) và mọi thao tác phải thực sự thay đổi cọc, vì vậy 

[ 
\sum_{r=1}^{m}d_{r,j}>0 
] 

cho mọi hoạt động (j). 

Do đó (f(x)), số cách để một cọc trở thành (1) trong các thao tác (x), chính xác là số ma trận (d). Điều này biến bài toán chuỗi chia thành bài toán thành phần bị hạn chế. Đây là phép biến đổi tổ hợp trung tâm được sử dụng bởi lời giải chính thức. 

Nếu chúng ta tạm thời bỏ qua yêu cầu rằng mọi thao tác phải loại bỏ một cái gì đó thì mỗi số mũ (e_r) có thể được phân bổ giữa các phép toán (x) trong 

[ 
\binom{e_r+x-1}{x-1} 
] 

cách. Nhân trên tất cả các thừa số nguyên tố cho 

[ 
g(x)=\prod_{r=1}^{m}\binom{e_r+x-1}{x-1}. 
] 

Bây giờ một số thao tác (x) có thể không nhận được gì cả. Loại trừ bao gồm các hoạt động trống mang lại 

[ 
f(x)=\sum_{y=0}^{x}(-1)^{x-y}\binom{x}{y}g(y). 
] 

Tính công thức này một cách độc lập với mọi (x) sẽ lại là phương trình bậc hai. Phần hữu ích là hệ số nhị thức phân tách: 

[ 
\binom{x}{y}=\frac{x!}{y!(x-y)!}. 
] 

Do đó 

\sum_{y=0}^{x} 
\frac{(-1)^{x-y}}{(x-y)!} 
\frac{g(y)}{y!}. 
] 

Đây là một tích chập thông thường. Xác định 

[ 
A_t=\frac{(-1)^t}{t!}, 
\qquad 
B_y=\frac{g(y)}{y!}. 
] 

Sau đó 

[ 
\frac{f(x)}{x!}=(A*B)_x. 
] 

Mô-đun này đặc biệt thuận tiện: 

[ 
985661441=235\cdot2^{22}+1, 
] 

và (3) là gốc nguyên thủy, do đó lũy thừa của hai tối đa (2^{22}) hỗ trợ NTT. Tích chập cần thiết lớn nhất có độ dài tối đa khoảng (2w), vừa với bên trong (2^{18}) khi (w\le10^5). 

Chúng ta vẫn cần nối (f) trở lại dãy cọc tròn. Giả sử cọc (i), sử dụng chỉ mục dựa trên số 0, là cọc trở thành (1) và giả sử điều này xảy ra trong thao tác thứ (x) của nó. Mỗi cọc trước (i) đã được vận hành (x) lần và vẫn phải chứa nhiều hơn một viên đá. Số lịch sử như vậy là (f(x+1)), bởi vì bất kỳ lịch sử hợp lệ nào của các phép toán (x) kết thúc ở trên (1) đều có chính xác một phép toán tiếp theo có thể xảy ra, cụ thể là thay đổi giá trị còn lại đó thành (1). Cọc cuối cùng có (x) thao tác và đóng góp (f(x)). Mỗi cọc sau (i) chỉ có (x-1) thao tác và cũng đóng góp (f(x)). Như vậy 

\sum_{x=0}^{w} 
f(x+1)^i f(x)^{k-i}. 
]

Đây là (O(wk)), nhỏ vì (k\le10). Công thức tương tự xuất hiện trong đạo hàm chính thức, với chỉ số cọc một cơ sở. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^w)) hoặc tệ hơn trên tất cả các cọc | Hàm mũ | Quá chậm | 
| Tối ưu | (O(wm+w\log w+wk)) | (O(w)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các trường hợp kiểm thử và chỉ lưu trữ số mũ (e_1,\ldots,e_m) và (k). Các giá trị nguyên tố không bao giờ tham gia vào việc đếm vì các phép chia số chia hoàn toàn được xác định bằng cách giảm số mũ. 
2. Tính (w=\sum e_i). Không có cọc nào được phép vận hành nhiều hơn (w) lần, vì mỗi thao tác hợp pháp đều làm giảm tổng số mũ ít nhất một. 
3. Tính toán trước giai thừa và nghịch đảo giai thừa modulo (985661441) cho đến (w) lớn nhất trong số tất cả các trường hợp thử nghiệm. Vì (w<985661441), mọi số nguyên lên đến (w) đều có nghịch đảo môđun. 
4. Xây dựng (g(x)) cho (1\le x\le w), trong đó 

[ 
g(x)=\prod_r\binom{e_r+x-1}{x-1}. 
] 

Đặt (g(0)=0), vì số mũ dương không thể phân bố giữa các phép toán bằng 0. Bắt đầu từ (g(1)=1), mỗi thừa số thỏa mãn 

\binom{e+x-1}{x-1}\frac{e+x}{x}. 
] 

Do đó, mỗi bước từ (g(x)) đến (g(x+1)) chỉ cần (m) phép nhân mô-đun thay vì tính lại các hệ số nhị thức từ đầu. 

1. Xây dựng hai mảng tích chập 

[ 
A_t=\frac{(-1)^t}{t!}, 
\qquad 
B_t=\frac{g(t)}{t!}. 
] 

Hệ số tích chập tại vị trí (x) chính xác là (f(x)/x!). 

1. Nhân hai mảng này với NTT. Kích thước biến đổi được yêu cầu là lũy thừa nhỏ nhất của hai ít nhất (2(w+1)-1). Mô-đun đã cho có lũy thừa đủ lớn bằng 2 inch (p-1), do đó phép biến đổi là số học mô-đun chính xác chứ không phải là FFT dấu phẩy động. 
2. Phục hồi 

[ 
f(x)=(A*B)_x x! 
] 

cho (0\le x\le w). Nối (f(w+1)=0). Giá trị này bằng 0 vì cọc không thể yêu cầu nhiều hơn (w) thao tác để đạt (1). 

1. Với mỗi cọc (i), hãy tính 

\sum_{x=0}^{w} 
f(x+1)^i f(x)^{k-i}. 
] 

Số mũ (i) tương ứng với các cọc xuất hiện trước cọc cuối cùng theo thứ tự tuần hoàn. Các hệ số (k-i) còn lại tương ứng với cọc cuối cùng và các cọc sau đó. 

1. Xuất ra (k) câu trả lời theo modulo (985661441). Trình tự kết quả nói chung là không đối xứng vì thứ tự vòng được cố định, mặc dù tất cả các cọc đều bắt đầu bằng cùng một giá trị. 

Tại sao nó hoạt động: tính bất biến đằng sau toàn bộ giải pháp là một chuỗi các phép chia số hợp lệ tương đương với một ma trận số mũ giảm có tổng hàng bằng số mũ ban đầu và các cột của nó đều khác 0. Hàm (g) đếm tất cả các ma trận có tổng hàng chính xác và loại trừ bao gồm sẽ loại bỏ chính xác những ma trận chứa cột trống. Do đó (f(x)) đếm chính xác các chuỗi hoạt động-(x) hợp lệ thành (1). Đối với cọc cuối cùng cố định và (x) cố định, lịch trình theo chu kỳ xác định chính xác số lượng thao tác mà mỗi cọc khác đã nhận được và hệ số (f(x+1)) so với (f(x)) tính chính xác các lịch sử chưa đạt đến (1). Tính tổng mọi (x) có thể tính mỗi trò chơi hợp lệ đúng một lần, tại thời điểm cột đầu tiên của nó trở thành (1). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 985661441
ROOT = 3
NAIVE_LIMIT = 256

FACT = []
IFACT = []

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
        wlen = pow(ROOT, (MOD - 1) // length, MOD)
        if invert:
            wlen = pow(wlen, MOD - 2, MOD)

        half = length >> 1
        for start in range(0, n, length):
            w = 1
            end = start + half
            for i in range(start, end):
                u = a[i]
                v = a[i + half] * w % MOD

                x = u + v
                if x >= MOD:
                    x -= MOD

                y = u - v
                if y < 0:
                    y += MOD

                a[i] = x
                a[i + half] = y
                w = w * wlen % MOD

        length <<= 1

    if invert:
        inv_n = pow(n, MOD - 2, MOD)
        for i in range(n):
            a[i] = a[i] * inv_n % MOD

def convolution(a, b):
    need = len(a) + len(b) - 1

    if min(len(a), len(b)) <= NAIVE_LIMIT:
        res = [0] * need
        for i, x in enumerate(a):
            if x == 0:
                continue
            for j, y in enumerate(b):
                if y:
                    res[i + j] = (res[i + j] + x * y) % MOD
        return res

    size = 1
    while size < need:
        size <<= 1

    a += [0] * (size - len(a))
    b += [0] * (size - len(b))

    ntt(a, False)
    ntt(b, False)

    for i in range(size):
        a[i] = a[i] * b[i] % MOD

    ntt(a, True)
    return a[:need]

def build_f(exps):
    w = sum(exps)

    g = [0] * (w + 1)
    g[1] = 1

    for x in range(1, w):
        inv_x = FACT[x - 1] * IFACT[x] % MOD
        ratio = 1

        for e in exps:
            ratio = ratio * (e + x) % MOD
            ratio = ratio * inv_x % MOD

        g[x + 1] = g[x] * ratio % MOD

    a = [0] * (w + 1)
    b = [0] * (w + 1)

    for x in range(w + 1):
        inv_fact = IFACT[x]
        a[x] = inv_fact if x % 2 == 0 else MOD - inv_fact
        b[x] = g[x] * inv_fact % MOD

    c = convolution(a, b)

    f = [0] * (w + 2)
    for x in range(w + 1):
        f[x] = c[x] * FACT[x] % MOD

    f[w + 1] = 0
    return f

def solve():
    global FACT, IFACT

    cases = []
    max_w = 0

    while True:
        line = input()
        if not line:
            break
        if not line.strip():
            continue

        m, k = map(int, line.split())
        exps = []

        for _ in range(m):
            _, e = map(int, input().split())
            exps.append(e)

        w = sum(exps)
        max_w = max(max_w, w)
        cases.append((exps, k))

    if not cases:
        return

    FACT = [1] * (max_w + 1)
    for i in range(1, max_w + 1):
        FACT[i] = FACT[i - 1] * i % MOD

    IFACT = [1] * (max_w + 1)
    IFACT[max_w] = pow(FACT[max_w], MOD - 2, MOD)
    for i in range(max_w, 0, -1):
        IFACT[i - 1] = IFACT[i] * i % MOD

    out = []

    for case_id, (exps, k) in enumerate(cases, 1):
        w = sum(exps)
        f = build_f(exps)

        ans = [0] * k

        for x in range(w + 1):
            left = f[x + 1]
            right = f[x]

            powers_left = [1] * k
            powers_right = [1] * (k + 1)

            for j in range(1, k):
                powers_left[j] = powers_left[j - 1] * left % MOD

            for j in range(1, k + 1):
                powers_right[j] = powers_right[j - 1] * right % MOD

            for i in range(k):
                ans[i] += powers_left[i] * powers_right[k - i] % MOD
                if ans[i] >= MOD:
                    ans[i] -= MOD

        out.append(
            "Case #{}: {}".format(case_id, " ".join(map(str, ans)))
        )

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào lưu trữ danh sách số mũ trước tiên để các giai thừa chỉ có thể được tính toán trước một lần, sử dụng giá trị tối đa (w) trong tất cả các trường hợp thử nghiệm. Điều này quan trọng vì có thể có khoảng 200 trường hợp và việc liên tục xây dựng lại các mảng giai thừa sẽ tạo thêm công việc không cần thiết. 

Phép truy toán cho (g) tránh được giai thừa có đối số lớn bằng (e_i+x-1). Đối với mỗi số mũ, sự chuyển đổi từ (x) sang (x+1) nhân với ((e_i+x)/x). Nghịch đảo của (x) được lấy là`FACT[x - 1] * IFACT[x]`. Mẫu số phải được áp dụng một lần cho mỗi số mũ, đó là lý do tại sao nghịch đảo xuất hiện bên trong vòng lặp`exps`. 

Hai mảng`a`Và`b`thực hiện tích chập chuẩn hóa trực tiếp.`a[x]`là ((-1)^x/x!), trong khi`b[x]`là (g(x)/x!). Sau khi tích chập, nhân hệ số (x) với`FACT[x]`phục hồi (f(x)). 

NTT sử dụng gốc nguyên thủy (3), hợp lệ cho mô đun này. Mô đun có dạng (235\cdot2^{22}+1), do đó, mọi độ dài biến đổi cần thiết cho bài toán sẽ chia lũy thừa khả dụng của hai. 

Vòng lặp cuối cùng cố tình chạy qua (x=w). Tại (x=w),`f[x + 1]`là số 0 được thêm rõ ràng. Đối với cọc (0), số mũ của nó bằng 0, do đó (0^0) tương ứng được mảng công suất mô-đun hiểu là (1), đây chính xác là những gì công thức yêu cầu. Đối với mọi cọc khác, số hạng tương tự biến mất vì lũy thừa dương của`f[w + 1]`xảy ra. 

Việc triển khai chỉ sử dụng tích chập bậc hai cho các mảng rất nhỏ. Khi đa thức trở nên lớn, NTT sẽ được sử dụng. Điều này không làm thay đổi độ phức tạp tiệm cận và tránh phải trả hệ số không đổi lớn hơn của NTT cho các trường hợp thử nghiệm nhỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp mẫu đầu tiên là```
1 1
2 2
```vì vậy (n=2^2=4), (k=1) và (w=2). 

Đối với một số mũ nguyên tố (e=2), số lượng phân phối không hạn chế là 

[ 
g(1)=1,\qquad g(2)=\binom31=3. 
] 

Bao gồm-loại trừ mang lại 

[ 
f(1)=1, 
] 

và 

[ 
f(2)=g(2)-2g(1)=3-2=1. 
] 

Không thể có ba phép toán khác rỗng khi tổng số mũ chỉ bằng hai, vì vậy (f(3)=0). 

| (x) | (f(x)) | (f(x+1)) | Đóng góp vào cọc 0 | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | (1^0 0^1=0) | 
| 1 | 1 | 1 | (1^0 1^1=1) | 
| 2 | 1 | 0 | (0^0 1^1=1) | 

Hai phần đóng góp tương ứng chính xác với (4\to1) và (4\to2\to1). Kết quả là`Case #1: 2`, phù hợp với đầu ra mẫu. 

### Mẫu 2 

Trường hợp mẫu thứ hai là```
2 1
2 1
3 1
```vì vậy (n=2\cdot3=6), (k=1) và (w=2). 

Có hai hàng số mũ, mỗi hàng chứa một đơn vị. Đối với một thao tác, cả hai đơn vị phải được đặt trong cùng một thao tác, cho (f(1)=1). Đối với hai phép tính, mỗi số mũ phải được gán cho một phép tính khác nhau, đưa ra hai khả năng, do đó (f(2)=2). 

| (x) | (f(x)) | (f(x+1)) | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | (0) | 
| 1 | 1 | 2 | (1) | 
| 2 | 2 | 0 | (2\cdot0^0=2) | 

Hai chuỗi có hai chiều dài là (6\to2\to1) và (6\to3\to1), trong khi (6\to1) tạo ra chuỗi có chiều dài một. Tổng số là (3), tạo ra`Case #2: 3`. 

Dấu vết thứ hai cũng chứng minh tại sao các số mũ nguyên tố khác nhau có thể bị loại bỏ trong cùng một thao tác. Hai đơn vị số mũ không bị buộc phải hoạt động như những chuỗi độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(wm+w\log w+wk)) | (g(x)) lấy (O(wm)), tích chập NTT lấy (O(w\log w)), và tất cả các câu trả lời cọc lấy (O(wk)) | 
| Không gian | (O(w)) | Giai thừa, mảng tích chập và (f) đều có kích thước tuyến tính | 

(w) lớn nhất là (10^5), trong khi (m,k\le10). Độ dài NTT là lũy thừa tiếp theo của hai số trên (2w+1), nhiều nhất là (2^{18}). Mô-đun hỗ trợ kích thước biến đổi này vì hệ số (2)-adic của nó là (2^{22}). Hạn chế đặc biệt mà chỉ có năm trường hợp có thể có (w\ge10^4) giúp kiểm soát các phép biến đổi lớn đắt tiền. Độ phức tạp thu được phù hợp với giải pháp dự định. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả sử giải pháp trên được lưu dưới dạng`solution.py`. Trình trợ giúp thay thế đầu vào tiêu chuẩn và ghi lại đầu ra tiêu chuẩn, do đó,`solve()`chức năng được thẩm phán sử dụng đã được kiểm tra.```python
# helper: run solution on input string, return output string
import sys
import io
from solution import solve

MOD = 985661441

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return output.getvalue()

# Provided samples
sample = """\
1 1
2 2
2 1
3 1
5 1
1 2
2 3
2 2
2 4
5 4
"""

expected_sample = """\
Case #1: 2
Case #2: 3
Case #3: 6 4
Case #4: 1499980 1281085
"""

assert run(sample) == expected_sample, "provided samples"

# Custom 1: minimum exponent, many piles.
# n = 2, k = 10. Only pile 0 can become 1.
inp = """\
1 10
2 1
"""

expected = "Case #1: 1 " + " ".join(["0"] * 9) + "\n"
assert run(inp) == expected, "prime n with many piles"

# Custom 2: boundary between one and two operations.
# n = 4, k = 2.
# f(1) = 1, f(2) = 1, so answers are [2, 1].
inp = """\
1 2
2 2
"""

assert run(inp) == "Case #1: 2 1\n", "two-pile boundary case"

# Custom 3: equal exponents on distinct primes.
# n = 2^2 * 3^2 = 36, k = 2.
# f(1), f(2), f(3), f(4) = 1, 7, 12, 6.
# Answers are 230 and 163.
inp = """\
2 2
2 2
3 2
"""

assert run(inp) == "Case #1: 230 163\n", "equal exponent case"

# Custom 4: maximum allowed exponent sum.
# n = 2^100000, k = 1.
# For one prime, every chain is a strictly decreasing sequence of
# exponents, so there are 2^99999 chains.
inp = """\
1 1
2 100000
"""

expected = f"Case #1: {pow(2, 99999, MOD)}\n"
assert run(inp) == expected, "maximum-size exponent case"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 10 / 2 1`|`Case #1: 1 0 0 0 0 0 0 0 0 0`| Số mũ tối thiểu, thứ tự tuần hoàn và thực tế là cọc đầu tiên là đặc biệt | 
|`1 2 / 2 2`|`Case #1: 2 1`| Ranh giới (x=0) và (x=w) và hướng chỉ số cọc | 
|`2 2 / 2 2 / 3 2`|`Case #1: 230 163`| Nhiều thừa số nguyên tố có số mũ bằng nhau và số mũ giảm đồng thời | 
|`1 1 / 2 100000`|`Case #1: 2^99999 mod 985661441`| Tối đa (w), giai thừa lớn, NTT và số mũ của chuỗi cơ bản | 

## Vỏ cạnh 

Đối với trường hợp nguyên tố hai cọc```
1 2
2 1
```chúng ta có (w=1), (f(0)=0), (f(1)=1), và (f(2)=0). Đối với cọc (0), số hạng với (x=1) là 

[ 
f(2)^0f(1)^2=1, 
] 

vì vậy câu trả lời của nó là (1). Đối với cọc (1), mọi số hạng đều chứa lũy thừa dương của (f(2)=0), nên đáp án của nó là (0). Đầu ra của thuật toán`Case #1: 1 0`. Điều này phát hiện ra lỗi lập chỉ mục phổ biến nhất, trong đó thứ tự tuần hoàn vô tình được coi là đối xứng. 

Vì```
1 2
2 2
```chúng ta có (n=4) và (w=2). Chuỗi pháp lý là (4\to1) và (4\to2\to1), vì vậy (f(1)=f(2)=1). Đối với cọc (0), 

[ 
f(1)^2+f(2)^2=2, 
] 

trong khi đối với cọc (1), 

[ 
f(2)f(1)=1. 
] 

Thuật toán tạo ra`Case #1: 2 1`. Ranh giới (f(w+1)=0) loại bỏ tất cả các lịch sử dài hơn không thể có. 

Vì```
2 2
2 1
3 1
```ma trận số mũ cho hai phép tính có hai hàng và hai cột. Mỗi hàng chứa một đơn vị và cả hai cột phải không trống, vì vậy hai hàng phải được gán cho các cột khác nhau. Có đúng hai khả năng, cho (f(2)=2). Các câu trả lời cọc kết quả là`Case #1: 5 2`. Đây là trường hợp cho thấy tại sao một thao tác đơn lẻ có thể rút gọn nhiều số mũ nguyên tố khác nhau cùng một lúc. 

Đối với trường hợp kích thước tối đa```
1 1
2 100000
```chỉ có một số mũ. Chuỗi hợp lệ chỉ đơn giản là một chuỗi giảm dần từ số mũ (100000) đến số mũ (0). Mọi tập hợp con của (99999) số mũ trung gian xác định một chuỗi, vì vậy câu trả lời là 

[ 
2^{99999}\pmod{985661441}. 
] 

Việc triển khai không bao giờ xây dựng (2^{100000}), không bao giờ liệt kê các chuỗi và không bao giờ xây dựng số nguyên ban đầu (n). Nó chỉ xử lý tổng số mũ (w=100000), đây chính xác là thông tin cần thiết cho công thức tổ hợp.
