---
title: "CF 232D - Hàng rào"
description: "Chúng tôi có một loạt các chiều cao tấm ván. Một mảnh hàng rào chỉ đơn giản là một đoạn liền kề. Đối với phân đoạn truy vấn $[l,r]$, chúng ta phải đếm xem có bao nhiêu phân đoạn khác có cùng độ dài khớp với nó. Sự phù hợp có ba yêu cầu. Hai đoạn phải có độ dài bằng nhau. Chúng phải rời rạc."
date: "2026-06-04T09:37:03+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "string-suffix-structures"]
categories: ["algorithms"]
codeforces_contest: 232
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 144 (Div. 1)"
rating: 2900
weight: 232
solve_time_s: 410
verified: false
draft: false
---

[CF 232D - Hàng rào](https://codeforces.com/problemset/problem/232/D) 

**Xếp hạng:** 2900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, cấu trúc hậu tố chuỗi 
**Thời gian giải:** 6 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt các chiều cao tấm ván. Một mảnh hàng rào chỉ đơn giản là một đoạn liền kề. 

Đối với một phân đoạn truy vấn$[l,r]$, chúng ta phải đếm xem có bao nhiêu đoạn khác có cùng độ dài khớp với nó. Sự phù hợp có ba yêu cầu. 

Hai đoạn phải có độ dài bằng nhau. 

Chúng phải rời rạc. 

Nếu chúng ta căn chỉnh chúng theo từng vị trí thì tổng chiều cao tương ứng phải luôn có cùng giá trị. Đối với một phân khúc$[l,r]$và một đoạn khác$[a,b]$, điều này có nghĩa$$h_{l+i}+h_{a+i}=h_l+h_a$$cho mọi phần bù hợp lệ$i$. 

Phản ứng đầu tiên là nhìn trực tiếp vào độ cao, nhưng điều kiện sẽ trở nên rõ ràng hơn nhiều sau khi tính đến sự khác biệt. 

Cho phép$$d_i=h_i-h_{i+1}$$vì$1\le i<n$. 

Giả sử hai đoạn có chiều rộng$k$. Điều kiện phù hợp ngụ ý$$(h_{l+i}-h_{l+i+1}) = -(h_{a+i}-h_{a+i+1})$$cho mọi vị trí. 

Nói cách khác, nếu$$d'_i=-d_i,$$sau đó các phân đoạn khớp sẽ tương ứng chính xác với các chuỗi con bằng nhau giữa mảng khác biệt$d$và mảng sai phân phủ định$d'$. Đây là sự chuyển đổi quan trọng được sử dụng bởi giải pháp chính thức. 

Những hạn chế là những gì buộc phải chuyển đổi này. Cả hai$n$Và$q$đang lên đến$10^5$. Bất kỳ thuật toán nào quét một phân đoạn cho mỗi truy vấn sẽ ngay lập tức trở nên quá chậm. Thậm chí$O(n)$mỗi truy vấn sẽ yêu cầu khoảng$10^{10}$hoạt động. Chúng tôi cần đại khái$O((n+q)\log n)$. 

Có một số trường hợp khó bỏ sót. 

Hãy xem xét một đoạn có chiều dài một.```
h = [5, 8, 1]
query = [2,2]
```Không có sự khác biệt trong một tấm ván duy nhất. Mọi tấm ván đơn khác đều tự động thỏa mãn điều kiện về hình dạng. Hạn chế duy nhất là sự rời rạc. Câu trả lời là$n-1$. Một giải pháp chỉ hoạt động trên các mảng sai phân phải xử lý trường hợp này một cách cẩn thận vì chuỗi con sai phân tương ứng có độ dài bằng 0. 

Hãy xem xét sự chồng chéo hoàn toàn.```
h = [1,2,1]
query = [1,2]
```Bản thân phân đoạn này phù hợp với mẫu riêng của nó, nhưng các phân đoạn phù hợp bắt buộc phải rời rạc. Việc đếm tất cả các lần xuất hiện của mẫu và quên đi giới hạn chồng chéo sẽ tạo ra một câu trả lời quá lớn. 

Hãy xem xét các mẫu lặp đi lặp lại.```
h = [1,2,1,2,1]
query = [1,2]
```Mô hình tương tự xuất hiện nhiều lần. Một số lần xuất hiện trùng lặp với phân đoạn truy vấn và phải được loại trừ, trong khi những lần khác là hợp lệ. Cấu trúc đếm phải hỗ trợ cả việc khớp mẫu và hạn chế vị trí hình học. 

## Phương pháp tiếp cận 

Giải pháp vũ phu rất đơn giản. 

Đối với mỗi phân đoạn truy vấn, hãy liệt kê mọi phân đoạn có cùng độ dài. Kiểm tra xem hai đoạn có rời nhau không. Sau đó so sánh các vị trí tương ứng và xác minh điều kiện khớp. 

Một chi phí so sánh duy nhất$O(k)$, Ở đâu$k$là độ dài đoạn có$O(n)$phân khúc ứng cử viên. Trong trường hợp xấu nhất, chi phí truy vấn$O(n^2)$, và với$10^5$thắc mắc điều này là vô vọng. 

Nhận xét quan trọng là điều kiện so khớp thực sự là điều kiện của các sai phân liên tiếp. 

Đối với các phân đoạn phù hợp,$$h_{l+i}+h_{a+i}=C.$$Trừ phương trình cho offset$i+1$:$$(h_{l+i}-h_{l+i+1}) = -(h_{a+i}-h_{a+i+1}).$$sử dụng$$d_i=h_i-h_{i+1}, \qquad d'_i=-d_i,$$vấn đề trở thành: 

Tìm lần xuất hiện của chuỗi con$$d[l..r-1]$$bên trong mảng$d'$. 

Bây giờ vấn đề trông giống như một vấn đề chuỗi cổ điển trên bảng chữ cái số nguyên. 

Giải pháp chính thức xây dựng một mảng hậu tố trên$$d + [\text{separator}] + d'.$$Đối với phân đoạn truy vấn có chiều rộng$k=r-l+1$, chúng ta cần tất cả các hậu tố trong$d'$của ai đầu tiên$k-1$giá trị chênh lệch bằng nhau$$d[l..r-1].$$Trong một mảng hậu tố, tất cả các hậu tố như vậy tạo thành một khoảng liền kề. Chúng ta có thể tìm thấy khoảng đó bằng cách sử dụng thông tin LCP và tìm kiếm nhị phân. 

Nhiệm vụ còn lại là đếm số lần xuất hiện tương ứng với các phân đoạn hàng rào tách rời khỏi phân đoạn truy vấn. 

Điều này trở thành một vấn đề đếm hình học trên các cấp bậc mảng hậu tố. Mỗi truy vấn hỏi: 

Có bao nhiêu hậu tố trong một khoảng mảng hậu tố có vị trí bắt đầu nằm ngoài phạm vi bị cấm? 

Cấu trúc kiểu cây phân đoạn bền vững hoặc cây wavelet cho phép chúng ta trả lời điều đó trong$O(\log n)$sau khi tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(qn^2)$trường hợp xấu nhất |$O(1)$| Quá chậm | 
| Mảng hậu tố + LCP + Đếm liên tục |$O(n\log n + q\log n)$|$O(n\log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng khác biệt$$d_i=h_i-h_{i+1}.$$Cũng xây dựng$$d'_i=-d_i.$$2. Nối$$T=d+[SEP]+d'.$$Dấu phân cách phải có giá trị nhỏ hơn mọi tọa độ nén để không có kết quả trùng khớp nào vượt qua ranh giới. 
3. Phối hợp-nén tất cả các giá trị và xây dựng một mảng hậu tố trên$T$. 
4. Xây dựng mảng LCP và cấu trúc RMQ sao cho có thể thu được LCP của hai hậu tố bất kỳ trong$O(1)$. 
5. Với mọi vị trí trong mảng sai phân ban đầu$d$, lưu trữ thứ hạng mảng hậu tố của nó. 
6. Đối với một truy vấn$[l,r]$, cho phép$$len=r-l.$$Đây là độ dài của chuỗi con khác biệt. 
7. Nếu$len=0$, phân đoạn truy vấn chứa một bảng. Mọi đoạn ván đơn rời rạc đều khớp. Câu trả lời là$n-1$. 
8. Nếu không hãy tìm hậu tố tương ứng với vị trí$l$TRONG$d$. 
9. Sử dụng tìm kiếm nhị phân trên các vị trí mảng hậu tố và truy vấn LCP, tìm khoảng mảng hậu tố tối đa$[L,R]$chứa hậu tố đó sao cho mỗi hậu tố trong khoảng chia sẻ ít nhất$len$giá trị khác biệt với nó. 

Đây chính xác là sự xuất hiện của mẫu được yêu cầu trong cấu trúc được nối. 
10. Trong số các hậu tố đó chỉ giữ lại những hậu tố thuộc về$d'$một nửa. 
11. Đặt hậu tố như vậy bắt đầu ở vị trí$p$bên trong$d'$. Nó tương ứng với một đoạn hàng rào$$[a,b]$$với$$a=p,\qquad b=p+len.$$12. Đoạn giao nhau với truy vấn iff$$a\le r \quad\text{and}\quad b\ge l.$$Tương tự, các lần xuất hiện hợp lệ thỏa mãn$$b<l \quad\text{or}\quad a>r.$$13. Sử dụng cấu trúc đếm liên tục để đếm các vị trí bên trong khoảng mảng hậu tố có chỉ số bắt đầu thỏa mãn phạm vi cho phép. 
14. Xuất ra số đếm. 

### Tại sao nó hoạt động 

Việc chuyển đổi sang mảng khác biệt là chính xác. Hai đoạn thỏa mãn điều kiện tổng chiều cao ban đầu khi và chỉ khi mọi chênh lệch chiều cao liên tiếp trong một đoạn bằng chênh lệch chiều cao liên tiếp bị phủ định ở đoạn kia. Điều đó chuyển đổi điều kiện hàng rào hình học thành đẳng thức chuỗi con giữa$d$Và$d'$. 

Mảng hậu tố nhóm các tiền tố bằng nhau thành các khoảng liền kề nhau. Điều kiện LCP đảm bảo rằng mọi hậu tố trong khoảng được định vị đều chứa mẫu sai phân bắt buộc và không có hậu tố nào ngoài khoảng đó. 

Cấu trúc đếm chỉ lọc các lần xuất hiện theo yêu cầu về độ rời rạc. Vì mỗi lần xuất hiện còn lại tương ứng với chính xác một đoạn hàng rào phù hợp nên số lượng được báo cáo chính xác là câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SuffixArray:
    def __init__(self, s):
        n = len(s)
        sa = list(range(n))
        rank = s[:]
        tmp = [0] * n

        k = 1
        while True:
            sa.sort(key=lambda x: (
                rank[x],
                rank[x + k] if x + k < n else -1
            ))

            tmp[sa[0]] = 0
            for i in range(1, n):
                a, b = sa[i - 1], sa[i]
                tmp[b] = tmp[a] + (
                    (rank[a], rank[a + k] if a + k < n else -1) <
                    (rank[b], rank[b + k] if b + k < n else -1)
                )

            rank, tmp = tmp, rank

            if rank[sa[-1]] == n - 1:
                break
            k <<= 1

        self.sa = sa
        self.rank = rank

        lcp = [0] * (n - 1)
        h = 0
        for i in range(n):
            r = rank[i]
            if r == 0:
                continue
            j = sa[r - 1]
            while i + h < n and j + h < n and s[i + h] == s[j + h]:
                h += 1
            lcp[r - 1] = h
            if h:
                h -= 1

        self.lcp = lcp

        m = max(1, len(lcp))
        lg = [0] * (m + 1)
        for i in range(2, m + 1):
            lg[i] = lg[i >> 1] + 1

        st = [lcp[:]]
        j = 1
        while (1 << j) <= len(lcp):
            prev = st[j - 1]
            cur = [
                min(prev[i], prev[i + (1 << (j - 1))])
                for i in range(len(lcp) - (1 << j) + 1)
            ]
            st.append(cur)
            j += 1

        self.lg = lg
        self.st = st

    def get_lcp(self, i, j):
        if i == j:
            return 10 ** 9
        ri = self.rank[i]
        rj = self.rank[j]
        if ri > rj:
            ri, rj = rj, ri
        l = rj - ri
        k = self.lg[l]
        return min(
            self.st[k][ri],
            self.st[k][rj - (1 << k)]
        )

class Node:
    __slots__ = ("l", "r", "sum")

    def __init__(self, l=None, r=None, s=0):
        self.l = l
        self.r = r
        self.sum = s

def build(lo, hi):
    node = Node()
    if lo != hi:
        mid = (lo + hi) >> 1
        node.l = build(lo, mid)
        node.r = build(mid + 1, hi)
    return node

def update(prev, lo, hi, pos):
    node = Node(prev.l, prev.r, prev.sum + 1)
    if lo != hi:
        mid = (lo + hi) >> 1
        if pos <= mid:
            node.l = update(prev.l, lo, mid, pos)
        else:
            node.r = update(prev.r, mid + 1, hi, pos)
    return node

def query(rt_r, rt_l, lo, hi, ql, qr):
    if ql > hi or qr < lo or ql > qr:
        return 0
    if ql <= lo and hi <= qr:
        return rt_r.sum - rt_l.sum
    mid = (lo + hi) >> 1
    return (
        query(rt_r.l, rt_l.l, lo, mid, ql, qr) +
        query(rt_r.r, rt_l.r, mid + 1, hi, ql, qr)
    )

def solve():
    n = int(input())
    h = list(map(int, input().split()))

    if n == 1:
        q = int(input())
        print("\n".join("0" for _ in range(q)))
        return

    d = [h[i] - h[i + 1] for i in range(n - 1)]
    nd = [-x for x in d]

    vals = sorted(set(d + nd))
    comp = {v: i + 1 for i, v in enumerate(vals)}

    sep = 0
    T = [comp[x] for x in d] + [sep] + [comp[x] for x in nd]

    sa = SuffixArray(T)

    m = n - 1
    offset = m + 1

    roots = [build(1, m)]
    for pos in sa.sa:
        val = 0
        if offset <= pos < offset + m:
            val = pos - offset + 1

        if val:
            roots.append(update(roots[-1], 1, m, val))
        else:
            roots.append(roots[-1])

    q = int(input())
    ans = []

    for _ in range(q):
        l, r = map(int, input().split())

        if l == r:
            ans.append(str(n - 1))
            continue

        pat_len = r - l
        pos = l - 1

        rk = sa.rank[pos]
        N = len(T)

        lo, hi = 0, rk
        left = rk
        while lo <= hi:
            mid = (lo + hi) // 2
            ok = True
            if mid < rk:
                ok = sa.get_lcp(sa.sa[mid], sa.sa[rk]) >= pat_len
            if ok:
                left = mid
                hi = mid - 1
            else:
                lo = mid + 1

        lo, hi = rk, N - 1
        right = rk
        while lo <= hi:
            mid = (lo + hi) // 2
            ok = True
            if mid > rk:
                ok = sa.get_lcp(sa.sa[mid], sa.sa[rk]) >= pat_len
            if ok:
                right = mid
                lo = mid + 1
            else:
                hi = mid - 1

        total = 0

        max_left_start = l - pat_len
        if max_left_start >= 1:
            total += query(
                roots[right + 1],
                roots[left],
                1,
                m,
                1,
                max_left_start
            )

        min_right_start = r + 1
        if min_right_start <= m:
            total += query(
                roots[right + 1],
                roots[left],
                1,
                m,
                min_right_start,
                m
            )

        ans.append(str(total))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp tuân theo cấu trúc chính xác của phương pháp chính thức. Mảng hậu tố xác định tất cả các lần xuất hiện của mẫu khác biệt được yêu cầu. Cây phân đoạn liên tục lưu trữ các vị trí bắt đầu từ$d'$một nửa được sắp xếp theo thứ hạng mảng hậu tố, cho phép đếm khoảng thời gian theo logarit. Phần tinh tế nhất là chuyển đổi giữa các vị trí hàng rào và các vị trí mảng khác nhau. Một đoạn hàng rào có chiều rộng$k$tương ứng với một chuỗi con khác nhau có độ dài$k-1$, đó là lý do tại sao độ dài truy vấn bên trong cấu trúc hậu tố là`r - l`, không`r - l + 1`. 

## Ví dụ đã hoạt động 

### Mẫu 1```
h = [1,2,2,1,100,99,99,100,100,100]
query = [1,4]
```Mảng khác biệt là:```
d = [-1,0,1,-99,1,0,-1,0,0]
```Truy vấn tương ứng với:```
[-1,0,1]
```Tìm kiếm bên trong$d'$:```
[1,0,-1,99,-1,0,1,0,0]
```đưa ra một lần xuất hiện hợp lệ. 

| Bước | Giá trị | 
| --- | --- | 
| Đoạn truy vấn | [1,4] | 
| Độ dài chênh lệch | 3 | 
| Mẫu | [-1,0,1] | 
| Sự xuất hiện trùng khớp trong d' | bắt đầu = 5 | 
| Rời rạc | vâng | 
| Trả lời | 1 | 

Ví dụ này thể hiện sự rút gọn cốt lõi từ việc so khớp hàng rào sang so khớp chuỗi con. 

### Ví dụ tùy chỉnh```
h = [5,5,5,5]
query = [2,2]
```| Bước | Giá trị | 
| --- | --- | 
| Độ dài đoạn | 1 | 
| Độ dài chuỗi con khác biệt | 0 | 
| Tất cả các ván đơn khác đều phù hợp | vâng | 
| Vị trí rời rạc | 1,3,4 | 
| Trả lời | 3 | 

Điều này thực hiện trường hợp chiều dài một đặc biệt. Không có giá trị chênh lệch nào tồn tại, vì vậy mỗi tấm ván rời nhau đều khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\log n + q\log n)$| xây dựng mảng hậu tố cộng với xử lý truy vấn logarit | 
| Không gian |$O(n\log n)$| cây phân đoạn liên tục và cấu trúc mảng hậu tố | 

Với$n,q \le 10^5$, sự phức tạp này phù hợp thoải mái với các giới hạn. Quá trình tiền xử lý được thực hiện một lần và mỗi truy vấn chỉ yêu cầu một vài tìm kiếm nhị phân và số lượng phạm vi cây liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

assert run(
"""10
1 2 2 1 100 99 99 100 100 100
6
1 4
1 2
3 4
1 5
9 10
10 10
"""
) == "\n".join([
"1",
"2",
"2",
"0",
"2",
"9"
]), "sample 1"

assert run(
"""1
7
1
1 1
"""
) == "0", "minimum size"

assert run(
"""4
5 5 5 5
2
1 1
2 2
"""
) == "\n".join([
"3",
"3"
]), "all equal"

assert run(
"""3
1 2 3
2
1 2
2 3
"""
) == "\n".join([
"0",
"0"
]), "overlapping only"

assert run(
"""5
1 2 1 2 1
1
1 2
"""
) == "1", "repeated pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ván đơn | 0 | Ranh giới tối thiểu | 
| Mọi chiều cao đều bằng nhau | 3, 3 | Xử lý một chiều dài | 
| Tăng chiều cao | 0, 0 | Không có phân đoạn phù hợp | 
| Mô hình xen kẽ | 1 | Nhiều lần xuất hiện và lọc chồng chéo | 

## Vỏ cạnh 

Hãy xem xét một truy vấn có độ dài một.```
n = 4
h = [5,7,9,11]
query = [2,2]
```Thuật toán ngay lập tức vào nhánh đặc biệt`l == r`. Mọi tấm ván khác tạo thành một phân đoạn phù hợp hợp lệ vì không có sự khác biệt bên trong nào để so sánh. Câu trả lời là$3$. 

Hãy xem xét một mô hình chỉ xảy ra ở các vị trí chồng chéo.```
h = [1,2,1]
query = [1,2]
```Mẫu khác biệt bắt buộc xuất hiện một lần, cụ thể là trong chính phân đoạn truy vấn. Khoảng mảng hậu tố chứa lần xuất hiện đó, nhưng giai đoạn đếm liên tục sẽ từ chối nó vì phân đoạn tương ứng giao với truy vấn. Câu trả lời trở thành$0$. 

Hãy xem xét nhiều lần xuất hiện bằng nhau.```
h = [1,2,1,2,1]
query = [1,2]
```Mô hình khác biệt xảy ra nhiều lần. Khoảng mảng hậu tố ghi lại tất cả các lần xuất hiện. Cấu trúc đếm chỉ giữ lại những vị trí bắt đầu nằm hoàn toàn trước hoặc hoàn toàn sau phân đoạn truy vấn. Câu trả lời cuối cùng tính chính xác các kết quả trùng khớp và bỏ qua các kết quả trùng lặp. 

Sự kết hợp giữa tìm kiếm khoảng thời gian hậu tố và lọc vị trí xử lý tất cả các trường hợp này một cách thống nhất.
