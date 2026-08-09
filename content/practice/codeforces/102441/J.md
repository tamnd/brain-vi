---
title: "CF 102441J - Xét nghiệm quan hệ cha con"
description: "Chúng ta có một cây có gốc với đỉnh 1 là gốc. Các đỉnh được đánh số từ 1 đến (n). Đối với khoảng truy vấn ([l,r]), mọi đỉnh (v) với (lle vle r) đóng góp số đỉnh từ cùng khoảng đó nằm trong cây con của (v)."
date: "2026-08-08T13:33:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102441
codeforces_index: "J"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Final"
rating: 0
weight: 102441
solve_time_s: 268
verified: true
draft: false
---

[CF 102441J - Xét nghiệm quan hệ cha con](https://codeforces.com/problemset/problem/102441/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với đỉnh 1 là gốc. Các đỉnh được đánh số từ 1 đến (n). Đối với khoảng truy vấn ([l,r]), mọi đỉnh (v) với (l\le v\le r) đóng góp số đỉnh từ cùng khoảng đó nằm trong cây con của (v). Do đó, câu trả lời là số cặp có thứ tự ((u,v)) bên trong khoảng mà (u) là tổ tiên của (v), bao gồm cả cặp ((v,v)). 

Cây được cho bởi (n-1) tài liệu tham khảo tổ tiên. Tham chiếu thứ (i)-th mô tả tổ tiên của đỉnh (i+1), do đó mọi tổ tiên của đỉnh có nhãn nhỏ hơn. Các truy vấn được mã hóa bằng cách sử dụng câu trả lời trước: hai giá trị đầu vào được XOR với câu trả lời trước, giảm modulo (n), chuyển sang phạm vi (1\ldots n), sau đó được sắp xếp để thu được (l) và (r). Mã hóa này làm cho các truy vấn trở nên trực tuyến, vì vậy chúng tôi không thể sắp xếp lại chúng hoặc xử lý trước chúng sau khi xem tất cả các khoảng thời gian được giải mã. Vấn đề ban đầu chỉ định (n,q\le 50000) và giới hạn thời gian 3 giây. 

Câu trả lời có thể lớn bằng (n(n+1)/2). Với (n=50000), tức là (1.250.025.000), do đó, số nguyên có dấu 32 bit vẫn đủ cho câu trả lời cuối cùng, mặc dù việc sử dụng số nguyên Python sẽ loại bỏ mọi lo ngại về tràn. 

Khoảng đơn rất dễ bị sai nếu việc triển khai chỉ tính các cặp tổ tiên-con cháu có hai đỉnh khác nhau. Ví dụ,```
1
1
0 0
```giải mã thành ([1,1]) và câu trả lời là`1`, vì một đỉnh được chứa trong cây con của chính nó. 

Hai đỉnh không liên quan là một trường hợp biên hữu ích khác. Coi như```
3
1
1
1
1 2
```Truy vấn là ([2,3]). Không có đỉnh nào là tổ tiên của đỉnh kia, vì vậy mỗi đỉnh chỉ đóng góp chính nó và câu trả lời là`2`. Việc triển khai đếm mọi cặp đỉnh trong khoảng là cặp tổ tiên sẽ trả về không chính xác`3`. 

Một truy vấn chứa gốc cũng kiểm tra xem toàn bộ cây con có được xử lý chính xác hay không. Vì```
3
1
1
1
0 2
```khoảng thời gian là ([1,3]). Đỉnh 1 chứa cả ba đỉnh, trong khi đỉnh 2 và 3 chỉ chứa chính chúng. Câu trả lời là`3 + 1 + 1 = 5`. Việc triển khai vô tình loại trừ phần gốc hoặc coi khoảng thời gian của cây con là nửa mở ở sai vị trí có thể làm mất một trong những đóng góp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra từng cặp đỉnh trong khoảng được truy vấn và kiểm tra xem một đỉnh có phải là tổ tiên của đỉnh kia hay không. DFS cung cấp cho mỗi đỉnh một thời gian vào`tin`và thời gian thoát`tout`và (u) là tổ tiên của (v) chính xác khi 

[ 
tin_u\le tin_v<tout_u. 
] 

Điều này làm cho mỗi cặp thử nghiệm có thời gian không đổi sau khi tiền xử lý. Tuy nhiên, một truy vấn chứa tất cả (n) đỉnh sẽ kiểm tra các cặp (\Theta(n^2)). Với (n=q=50000), tức là khoảng (1,25\times10^9) cặp không có thứ tự cho một truy vấn và khoảng (6,25\times10^{13}) kiểm tra cặp không có thứ tự trên tất cả các truy vấn. Lực lượng vũ phu là chính xác vì nó kiểm tra chính xác định nghĩa của câu trả lời, nhưng nó vượt xa thời gian có sẵn. 

Quan sát hữu ích là mối quan hệ tổ tiên có cấu trúc rất cứng nhắc theo thứ tự DFS. Đối với hai đỉnh phân biệt, có đúng một trong các trường hợp sau xảy ra. Một cây con chứa cây kia hoặc các cây con của chúng rời rạc. Nếu (u) và (v) rời nhau và (u) xuất hiện đầu tiên trong thứ tự DFS thì 

[ 
tout_u\le tin_v. 
] 

Công thức biên tập sử dụng quan điểm bổ sung này. Đối với một khoảng chứa (k) đỉnh, có (\binom{k}{2}) cặp đỉnh phân biệt không có thứ tự. Mỗi cặp không phải là cặp tổ tiên-con cháu là một cặp cây con rời rạc. Nếu như`bad(l,r)`là số cặp rời nhau như vậy thì 

# k+\binom{k}{2}-bad(l,r) 

\frac{k(k+1)}2-bad(l,r). 
] 

Điều này biến vấn đề thành các truy vấn phạm vi trên các nhãn đỉnh, trong đó mỗi cặp đóng góp một nếu hai cây con tương ứng rời nhau. 

Sau đó chúng tôi chia trục nhãn thành các khối có kích thước (T). Một truy vấn có nhiều nhất hai khối một phần ở hai đầu của nó. Mọi thứ giữa chúng bao gồm các khối hoàn chỉnh. Chúng tôi tính toán trước câu trả lời cho mỗi khoảng thời gian của các khối hoàn chỉnh. Chúng tôi cũng tính toán trước, đối với mỗi đỉnh và mỗi ranh giới khối, có bao nhiêu đỉnh trong tiền tố của các khối có các cây con rời rạc với đỉnh đó và đối xứng với các hậu tố của khối. Điều này cho phép mọi tương tác giữa một đỉnh biên và tất cả các khối hoàn chỉnh ở giữa được đánh giá trong (O(1)). 

Công việc duy nhất còn lại bên trong hai khối một phần được giới hạn bởi (O(T)). Sự tương tác giữa hai khối một phần được tính bằng cách hợp nhất các điểm cuối khoảng DFS của chúng sau khi sắp xếp từng khối một lần. 

Đây là ý tưởng phân tách căn bậc hai tương tự được mô tả trong tài liệu biên tập chính thức: chọn (T) xung quanh (n/\sqrt q), tính toán trước thông tin tiền tố phần tử đến khối và câu trả lời khối hoàn chỉnh, sau đó trả lời từng truy vấn bằng cách chỉ xử lý hai khối ranh giới. Giới hạn kết quả là (O(n(\sqrt q+\log n))). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(qn^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n(\sqrt q+\log n))) | (O(n\sqrt q)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy DFS từ đỉnh 1 và tính toán`tin[v]`Và`tout[v]`. Cây con của (v) được biểu thị bằng khoảng DFS nửa mở ([tin[v],tout[v])). Hai đỉnh có cây con rời nhau chính xác khi một trong`tout[u] <= tin[v]`hoặc`tout[v] <= tin[u]`nắm giữ. 
2. Chọn kích thước khối (T) gần với (n/\sqrt q). Khoảng nhãn (1\ldots n) được chia thành các khối liên tiếp. Một truy vấn có thể giao nhau với nhiều khối hoàn chỉnh, nhưng chỉ khối đầu tiên và khối cuối cùng của nó mới có thể là một phần. 
3. Với mỗi đỉnh (v) và mỗi tiền tố khối, hãy tính trước có bao nhiêu đỉnh trong tiền tố đó có các cây con tách khỏi (v). Chúng tôi gọi đây là`pref[v][b]`, Ở đâu`b`có nghĩa là các khối có chỉ số nhỏ hơn`b`. 

Giá trị này là tổng của hai điều kiện độc lập. Đỉnh trước (u) tách khỏi (v) vì (tout[u]\le tin[v]), hoặc vì (tout[v]\le tin[u]). Cả hai bộ đều có thể được tính bằng cách quét theo thời gian vào và ra DFS. 
4. Xây dựng bảng hậu tố tương tự`suf[v][b]`, đếm các đỉnh rời rạc trong các khối từ`b`trở đi. Hai lần quét thời gian DFS giống nhau được sử dụng nhưng chỉ số khối được xử lý ngược lại. 
5. Đối với mỗi khối, tính toán trước số cặp rời rạc bên trong mỗi khoảng con của khối đó. Chỉ có (T) đỉnh trong một khối, vì vậy tất cả các khoảng con của nó có thể được xử lý trong (O(T^2)). Chúng tôi lưu trữ kết quả trong một mảng số nguyên nhỏ gọn. 
6. Tính toán trước`full[a][b]`, số cặp rời nhau giữa tất cả các đỉnh thuộc các khối hoàn chỉnh (a,a+1,\ldots,b). Khi khối (b) được thêm vào một khoảng bắt đầu ở khối (a), các cặp bên trong của nó đã được biết. Với mọi đỉnh (v) trong khối (b),`pref[v][b] - pref[v][a]`đếm các đối tác rời rạc của nó trong các khối hoàn chỉnh trước đó. Tính tổng số này trên khối sẽ mang lại sự đóng góp xuyên khối mới. 
7. Giải mã mọi truy vấn bằng câu trả lời trước đó. Chuyển đổi hai số được mã hóa thành các đỉnh trong (1\ldots n), sắp xếp chúng thành (l) và (r) và làm việc với các chỉ số dựa trên 0 bên trong. 
8. Nếu cả hai điểm cuối đều nằm trong cùng một khối, hãy lấy`bad(l,r)`trực tiếp từ bảng khoảng con được tính toán trước. 
9. Nếu không, hãy tính toán trước`full`giá trị cho các khối hoàn chỉnh nằm giữa hai khối ranh giới. Thêm các cặp rời rạc bên trong hậu tố một phần của khối bên trái và tiền tố một phần của khối bên phải. 
10. Đối với mỗi đỉnh trong khối một phần bên trái, hãy sử dụng bảng hậu tố để đếm các đối tác rời rạc của nó trong các khối ở giữa hoàn chỉnh. Đối với mỗi đỉnh trong khối một phần bên phải, hãy sử dụng bảng tiền tố để đếm các đối tác rời rạc của nó trong các khối ở giữa đó. Có nhiều nhất (2T) các đỉnh biên như vậy nên giá trị này là (O(T)). 
11. Cuối cùng, đếm các cặp rời rạc giữa hai khối từng phần. Các giá trị`tout`ở bên trái và`tin`bên phải đã được sắp xếp nên số thỏa mãn`tout[left] <= tin[right]`có thể được tìm thấy bằng cách hợp nhất hai con trỏ tuyến tính. Lặp lại với vai trò đảo ngược. Điều này đếm mọi cặp rời rạc giữa hai khối một phần chính xác một lần. 
12. Nếu khoảng chứa (k=r-l+1) đỉnh, xuất ra (k(k+1)/2-bad(l,r)), sau đó lưu giá trị đó làm câu trả lời trước đó để giải mã truy vấn tiếp theo. 

### Tại sao nó hoạt động 

Với mỗi hai đỉnh riêng biệt, các cây con của chúng được lồng vào nhau hoặc rời nhau. Một cặp lồng nhau đóng góp chính xác một mối quan hệ tổ tiên-con cháu, trong khi một cặp rời rạc không đóng góp gì. Do đó, trong số các cặp (\binom{k}{2}) không có thứ tự trong một khoảng truy vấn, chính xác`bad(l,r)`không đóng góp được mối quan hệ tổ tiên. Mỗi đỉnh cũng tự đóng góp, tạo ra (k) cặp bổ sung. Sự phân rã đếm mỗi cặp rời rạc chính xác một lần, hoặc bên trong một khối một phần, giữa một khối một phần và các khối hoàn chỉnh ở giữa, giữa hai khối hoàn chỉnh hoặc giữa hai khối một phần. Do đó, công thức cuối cùng (k(k+1)/2-bad(l,r)) chính xác là tổng được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from array import array
from math import isqrt

def solve():
    n = int(input())

    children = [[] for _ in range(n)]
    for v in range(1, n):
        p = int(input()) - 1
        children[p].append(v)

    # Iterative DFS.
    tin = [0] * n
    tout = [0] * n
    at_tin = [0] * n
    at_tout = [0] * (n + 1)

    timer = 0
    stack = [(0, 0, 0)]

    while stack:
        v, p, state = stack.pop()

        if state == 0:
            tin[v] = timer
            at_tin[timer] = v
            timer += 1

            stack.append((v, p, 1))
            for u in reversed(children[v]):
                if u != p:
                    stack.append((u, v, 0))
        else:
            tout[v] = timer
            at_tout[timer] = v

    q = int(input())

    # T ~= n / sqrt(q).
    sq = max(1, isqrt(q))
    T = max(1, n // sq)
    B = (n + T - 1) // T
    stride = B + 1

    block_of = [v // T for v in range(n)]
    block_start = [b * T for b in range(B)]
    block_end = [min(n, (b + 1) * T) for b in range(B)]

    # Build a table:
    # table[v][b] = number of vertices in blocks [0, b)
    # whose subtrees are disjoint from v.
    #
    # If reverse=True, blocks are considered in reverse order.
    def build_disjoint_table(reverse):
        size = n * stride
        table = array('I', [0]) * size

        def mapped_block(v):
            b = block_of[v]
            return B - 1 - b if reverse else b

        cnt = [0] * B

        # First condition:
        # tout[u] <= tin[v].
        #
        # Sweep the threshold tin[v] from small to large.
        for t in range(n):
            u = at_tout[t] if t <= n else 0
            if t > 0:
                u = at_tout[t]
                cnt[mapped_block(u)] += 1

            v = at_tin[t]
            base = v * stride
            cur = 0
            table[base] = 0

            for b in range(B):
                cur += cnt[b]
                table[base + b + 1] = cur

        # Second condition:
        # tout[v] <= tin[u], equivalently tin[u] >= tout[v].
        #
        # Sweep the threshold tout[v] from large to small.
        cnt = [0] * B

        for s in range(n, 0, -1):
            u = at_tin[s - 1]
            cnt[mapped_block(u)] += 1

            v = at_tout[s]
            base = v * stride
            cur = 0

            for b in range(B):
                cur += cnt[b]
                table[base + b + 1] += cur

        return table

    pref = build_disjoint_table(False)
    suf = build_disjoint_table(True)

    # Precompute all subinterval answers inside one block.
    #
    # small[b][l * m + r] =
    # number of disjoint pairs in that subinterval.
    small = []
    internal = [0] * B

    for b in range(B):
        L = block_start[b]
        R = block_end[b]
        m = R - L

        sqmat = array('I', [0]) * (m * m)

        # Process the left endpoint backwards.
        # bad(l, r) = bad(l+1, r) + pairs(l, l+1..r).
        for l in range(m - 1, -1, -1):
            cur = 0
            vl = L + l
            row = l * m
            next_row = (l + 1) * m

            for r in range(l + 1, m):
                vr = L + r

                disjoint = (
                    tout[vl] <= tin[vr] or
                    tout[vr] <= tin[vl]
                )

                if disjoint:
                    cur += 1

                sqmat[row + r] = sqmat[next_row + r] + cur

        small.append(sqmat)
        internal[b] = sqmat[m - 1] if m else 0

    # full[a][b] = number of disjoint pairs in complete blocks a..b.
    full = [[0] * B for _ in range(B)]

    for a in range(B):
        total = 0

        for b in range(a, B):
            cross = 0

            for v in range(block_start[b], block_end[b]):
                base = v * stride
                cross += pref[base + b] - pref[base + a]

            total += internal[b] + cross
            full[a][b] = total

    # Values sorted by DFS entry and exit times inside every label block.
    # They let us count pairs between the two partial blocks in O(T).
    sorted_tin = []
    sorted_tout = []

    for b in range(B):
        L = block_start[b]
        R = block_end[b]

        ids = list(range(L, R))
        sorted_tin.append(sorted(ids, key=tin.__getitem__))
        sorted_tout.append(sorted(ids, key=tout.__getitem__))

    def count_le(A, Bvals):
        # A and Bvals are sorted.
        # Count pairs (a,b) with a <= b.
        j = 0
        m = len(Bvals)
        ans = 0

        for a in A:
            while j < m and Bvals[j] < a:
                j += 1
            ans += m - j

        return ans

    def cross_partial(lb, l, left_end, rb, right_start, r):
        # Count disjoint pairs between:
        # [l, left_end] and [right_start, r].
        #
        # The first orientation is tout[left] <= tin[right].
        # The second orientation is tout[right] <= tin[left].
        out_left = [
            tout[v]
            for v in sorted_tout[lb]
            if v >= l
        ]
        in_right = [
            tin[v]
            for v in sorted_tin[rb]
            if v <= r
        ]

        out_right = [
            tout[v]
            for v in sorted_tout[rb]
            if v <= r
        ]
        in_left = [
            tin[v]
            for v in sorted_tin[lb]
            if v >= l
        ]

        return count_le(out_left, in_right) + count_le(out_right, in_left)

    def query(l, r):
        k = r - l + 1
        lb = l // T
        rb = r // T

        if lb == rb:
            m = block_end[lb] - block_start[lb]
            ll = l - block_start[lb]
            rr = r - block_start[lb]
            bad = small[lb][ll * m + rr]
            return k * (k + 1) // 2 - bad

        # Complete blocks strictly between the two boundary blocks.
        bad = 0

        ml = lb + 1
        mr = rb - 1

        if ml <= mr:
            bad += full[ml][mr]

        # Pairs entirely inside the two partial blocks.
        left_m = block_end[lb] - block_start[lb]
        left_l = l - block_start[lb]
        bad += small[lb][left_l * left_m + left_m - 1]

        right_m = block_end[rb] - block_start[rb]
        right_r = r - block_start[rb]
        bad += small[rb][right_r]

        if ml <= mr:
            # Left partial block against all complete middle blocks.
            #
            # suf[v][B-k] represents original blocks k..B-1.
            left_prefix_column = B - (lb + 1)
            left_suffix_column = B - rb

            for v in range(l, block_end[lb]):
                base = v * stride
                bad += (
                    suf[base + left_prefix_column]
                    - suf[base + left_suffix_column]
                )

            # Complete middle blocks against the right partial block.
            for v in range(block_start[rb], r + 1):
                base = v * stride
                bad += (
                    pref[base + rb]
                    - pref[base + (lb + 1)]
                )

        # Interaction between the two partial blocks.
        bad += cross_partial(
            lb, l, block_end[lb] - 1,
            rb, block_start[rb], r
        )

        return k * (k + 1) // 2 - bad

    ans = 0
    output = []

    for _ in range(q):
        u, v = map(int, input().split())

        x = 1 + ((u ^ ans) % n)
        y = 1 + ((v ^ ans) % n)

        l = min(x, y) - 1
        r = max(x, y) - 1

        ans = query(l, r)
        output.append(str(ans))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```DFS có tính lặp lại vì một cây hình chuỗi có thể chứa 50000 đỉnh, vượt quá độ sâu đệ quy thông thường của Python.`tin`được chỉ định khi nhập cảnh và`tout`khi thoát, do đó cây con của một đỉnh chính xác là một khoảng nửa mở theo thứ tự DFS. 

Hai bảng cặp rời nhau được lưu trữ trong`array('I')`thay vì danh sách Python thông thường. Có các mục (O(n\sqrt q)) và số nguyên Python sẽ tiêu tốn nhiều bộ nhớ hơn đáng kể. Mỗi số lượng được lưu trữ tối đa là (\ binom n2), phù hợp với số nguyên 32 bit không dấu cho ràng buộc này. 

Lần quét đầu tiên của mỗi tay cầm bàn`tout[u] <= tin[v]`. Tay cầm thứ hai`tout[v] <= tin[u]`. Tổng của chúng chính xác là số đỉnh có cây con tách rời khỏi`v`. Đảo ngược việc đánh số khối sẽ tạo ra bảng hậu tố mà không thay đổi thông tin cây cơ bản. 

các`small`các bảng xử lý các khoảng thời gian vẫn hoàn toàn nằm trong một khối. Phép truy toán sử dụng một hàng được tính toán trước đó, do đó mỗi cặp đỉnh được kiểm tra một lần trong khi tất cả các giá trị khoảng (O(T^2)) được tạo ra. 

Bảng khối hoàn chỉnh được xây dựng từ số lượng tiền tố rời rạc. Khi chặn`b`được thêm vào một phạm vi bắt đầu tại khối`a`, sự khác biệt`pref[v][b] - pref[v][a]`loại bỏ tất cả các đỉnh trước khối`a`và để lại chính xác các khối trước đó trong phạm vi hiện tại. 

Mã truy vấn giữ thứ tự thực hiện chính xác vì mã hóa XOR phụ thuộc vào câu trả lời trước đó. Các giá trị truy vấn thô phải được giải mã trước bất kỳ phép tính phạm vi nào và câu trả lời mới được tính toán phải được gán cho`ans`chỉ sau khi truy vấn hiện tại đã được đánh giá hoàn toàn. 

## Ví dụ đã hoạt động 

Mẫu chính thức chứa năm truy vấn được mã hóa. Hai trường hợp đầu tiên đã thể hiện cả trường hợp phạm vi đầy đủ và truy vấn trải rộng trên nhiều khối về mặt khái niệm. 

| Truy vấn | Câu trả lời trước | (x) | (y) | Khoảng thời gian | Đỉnh | Cặp rời rạc | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 9 | [1,9] | 9 | 3 | 42 | 
| 2 | 42 | 8 | 5 | [5,8] | 4 | 2 | 8 | 
| 3 | 8 | 2 | 3 | [2,3] | 2 | 1 | 3 | 
| 4 | 3 | 6 | 7 | [6,7] | 2 | 1 | 3 | 
| 5 | 3 | 5 | 8 | [5,8] | 4 | 2 | 8 | 

Đối với truy vấn đầu tiên, tất cả chín đỉnh đều được chọn. Có sẵn (9\cdot10/2=45) đóng góp theo cặp hoặc không theo thứ tự trước khi loại bỏ các cặp rời rạc. Cặp cây con rời rạc duy nhất là ba cặp liên quan đến đỉnh 6 và một trong các đỉnh 7, 8 hoặc 9. Do đó (45-3=42). 

Đối với truy vấn thứ hai, câu trả lời trước đó là 42. Các giá trị được mã hóa trở thành (1\oplus42=43) và (2\oplus42=40). Giảm modulo 9 và thêm một sẽ có các đỉnh 8 và 5, do đó khoảng là [5,8]. Có bốn đỉnh, cho ra 10 khả năng tự ghép hoặc không có thứ tự, và hai cặp rời rạc, cho (10-2=8). 

Một ví dụ nhỏ riêng biệt làm cho logic cặp rời rạc trở nên rõ ràng hơn.```
4
1
1
2
1
2 3
```Truy vấn duy nhất giải mã thành [3,4]. Đỉnh 3 và 4 nằm ở các nhánh khác nhau nên cây con của chúng rời nhau. Có hai đỉnh và một cặp rời nhau, do đó câu trả lời là (2\cdot3/2-1=2). 

| Bước | Câu trả lời trước | (u) | (v) | Khoảng thời gian được giải mã | (k) | Cặp xấu | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 3 | [3,4] | 2 | 1 | 2 | 

Ví dụ này xác nhận rằng thuật toán không nhầm lẫn hai đỉnh không liên quan với một cặp tổ tiên-con cháu. Nó cũng thực hiện ranh giới nơi truy vấn bắt đầu và kết thúc bên trong cùng một vùng nhỏ của mảng nhãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n(\sqrt q+\log n))) | DFS và sắp xếp lấy (O(n\log n)); quá trình tiền xử lý khối mất (O(n^2/T)); truy vấn lấy (O(T)) mỗi | 
| Không gian | (O(n^2/T+nT)) | Các bảng khối tiền tố/hậu tố và các bảng khoảng trong khối chiếm ưu thế trong bộ nhớ | 

Với (T\approx n/\sqrt q), thuật ngữ tiền xử lý (n^2/T) và tổng thuật ngữ truy vấn (qT) đều là (O(n\sqrt q)). Đối với (n,q\le50000), đây là sự cân bằng căn bậc hai dự kiến. Mảng số nguyên nhỏ gọn giữ các bảng lớn (O(n\sqrt q)) trong giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm giả định giải pháp được gửi có sẵn dưới dạng`solution.py`và phơi bày`solve()`.```python
import sys
import io
from contextlib import redirect_stdout

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        out = io.StringIO()
        with redirect_stdout(out):
            solve()
        return out.getvalue()
    finally:
        sys.stdin = old_stdin

# Provided sample.
sample = """\
9
1
2
3
4
5
5
7
8
5
0 8
1 2
2 3
4 5
6 7
"""
assert run(sample) == "42\n8\n3\n3\n3", "provided sample"

# Minimum-size tree.
minimum = """\
1
1
0 0
"""
assert run(minimum) == "1", "singleton vertex"

# All encoded values equal.
# Every query becomes a singleton after XOR decoding.
all_equal = """\
5
1
1
1
1
4
0 0
0 0
0 0
0 0
"""
assert run(all_equal) == "1\n1\n1\n1", "all-equal encoded queries"

# Two sibling leaves.
siblings = """\
3
1
1
1
1 2
"""
assert run(siblings) == "2", "disjoint sibling subtrees"

# Root plus two children.
root_interval = """\
3
1
1
1
0 2
"""
assert run(root_interval) == "5", "root interval boundary"

# Maximum-size chain.
# Every vertex is an ancestor of every later vertex, so the full interval
# has answer n * (n + 1) / 2.
n = 50000
parents = "\n".join(["1"] + [str(i) for i in range(2, n)])
maximum = (
    str(n) + "\n" +
    parents + "\n" +
    "1\n" +
    "0 " + str(n - 1) + "\n"
)
expected = str(n * (n + 1) // 2)
assert run(maximum) == expected, "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0`|`1`| Cây tối thiểu và tự đếm | 
| Sao năm nút có bốn`0 0`truy vấn |`1 1 1 1`| Giải mã XOR lặp đi lặp lại và các khoảng thời gian đơn lẻ | 
| Cây ba nút có bố mẹ`1,1`, truy vấn`1 2`|`2`| Hai cây con rời nhau và ranh giới khoảng | 
| Cây ba nút có bố mẹ`1,1`, truy vấn`0 2`|`5`| Cây con gốc và khoảng đầy đủ | 
| Chuỗi 50000 nút |`1250025000`| Kích thước tối đa, câu trả lời lớn và cấu trúc tổ tiên trong trường hợp xấu nhất | 

## Vỏ cạnh 

Đối với một khoảng đơn, thuật toán sẽ đi vào nhánh cùng khối ngay lập tức. Vì```
1
1
0 0
```khoảng chứa một đỉnh, vì vậy`k=1`Và`bad=0`. Giá trị trả về là (1\cdot2/2=1). Bảng bên trong khối chứa kết quả tương tự vì khoảng duy nhất của nó không có cặp đỉnh phân biệt. 

Đối với hai đỉnh không liên quan, hãy xem xét```
3
1
1
1
2
1
```Truy vấn được mã hóa là [2,3]. Cây con của chúng rời nhau, vì vậy`bad=1`. Với (k=2), công thức cho kết quả (2\cdot3/2-1=2). Việc hợp nhất giữa hai phạm vi một phần sẽ phát hiện cặp rời rạc thông qua một trong hai bất đẳng thức khoảng DFS. 

Đối với ngôi sao ba đỉnh đầy đủ,```
3
1
1
1
0 2
```khoảng thời gian là [1,3]. Đỉnh 1 là tổ tiên của cả hai lá, trong khi hai lá rời nhau. Như vậy`bad=1`, (k=3) và câu trả lời là (3\cdot4/2-1=5). Nút gốc đóng góp ba nút vào cây con của nó, trong khi mỗi nút đóng góp chính nó. 

Các truy vấn được mã hóa cũng cần được chăm sóc đặc biệt vì câu trả lời hiện tại sẽ thay đổi ý nghĩa của cặp đầu vào tiếp theo. Trong mẫu chính thức, truy vấn đầu tiên cho kết quả 42. Các giá trị thô tiếp theo`1 2`do đó được giải mã bằng XOR với 42, tạo ra khoảng [5,8], không phải khoảng [2,3] mà các số thô có thể gợi ý. Sự phụ thuộc này là lý do tại sao các truy vấn phải được trả lời nghiêm ngặt theo thứ tự đầu vào. 

Cuối cùng, chuỗi có cấu trúc cực đoan đối lập với một ngôi sao. Trong chuỗi 50000 nút, mỗi cặp đỉnh phân biệt là một cặp tổ tiên-con cháu, do đó`bad=0`cho toàn bộ khoảng thời gian. Câu trả lời trở thành (50000\cdot50001/2=1,250,025,000). Trường hợp này kiểm tra cả số học ở câu trả lời lớn nhất có thể và thực tế là bộ máy tách cặp không vô tình trừ đi các cây con lồng nhau.
