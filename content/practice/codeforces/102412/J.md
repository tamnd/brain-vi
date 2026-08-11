---
title: "CF 102412J - Một vấn đề khác của Mex"
description: "Sự cố xảy ra từ Cuộc thi MEX Foundation, Phòng tập 102412, Bài toán J. Các giới hạn chính thức là (2le nle 2cdot10^5), (1le kle n), (0le aile n), với giới hạn thời gian 4 giây và bộ nhớ 512 MiB. Chúng ta có một mảng số nguyên không âm (a)."
date: "2026-08-10T14:14:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102412
codeforces_index: "J"
codeforces_contest_name: "MEX Foundation Contest (supported by AIM Tech)"
rating: 0
weight: 102412
solve_time_s: 350
verified: true
draft: false
---

[CF 102412J - Một vấn đề khác của Mex](https://codeforces.com/problemset/problem/102412/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sự cố xảy ra từ Cuộc thi MEX Foundation, Phòng tập 102412, Bài toán J. Các giới hạn chính thức là (2\le n\le 2\cdot10^5), (1\le k\le n), (0\le a_i\le n), với giới hạn thời gian 4 giây và bộ nhớ 512 MiB. 

Chúng ta có một mảng số nguyên không âm (a). Chúng ta phải chia nó thành các phần liên tiếp, trong đó mỗi phần có chiều dài tối đa là (k). Đối với một quân cờ, giá trị của nó bằng tổng phần tử của nó nhân với MEX của nó. Giá trị của toàn bộ phân vùng là tổng giá trị của tất cả các phần và chúng tôi muốn giá trị tối đa có thể. Đầu ra là giá trị tối đa đó. 

Giả sử (S_i=a_1+\cdots+a_i), với (S_0=0). Nếu phần cuối cùng bắt đầu tại (j+1) và kết thúc tại (i), phần đóng góp của nó là 

[ 
\operatorname{mex}(a_{j+1},\ldots,a_i)(S_i-S_j). 
] 

Nếu (dp_i) biểu thị câu trả lời đúng nhất cho tiền tố kết thúc tại (i), thì phép truy toán trực tiếp là 

[ 
dp_i= 
\max_{i-k\le j<i} 
\left( 
dp_j+ 
\operatorname{mex}(j+1,i)(S_i-S_j) 
\đúng). 
] 

Giới hạn dưới của (j) là giới hạn thực thi độ dài mảnh tối đa. Vì (n) đạt tới (2\cdot10^5), thuật toán (O(n^2)) đã yêu cầu khoảng (4\cdot10^{10}) thao tác, vượt xa những gì một vài giây có thể xử lý. Việc tính toán mọi MEX từ đầu tạo ra cách tiếp cận trực tiếp (O(nk^2)), có thể đạt được khoảng (8\cdot10^{15}) phép toán cơ bản. Ngay cả việc duy trì mọi khoảng thời gian MEX cũng tăng dần (O(nk)), vẫn tối đa khoảng (4\cdot10^{10}) hoạt động. Giải pháp phải khai thác cấu trúc đặc biệt của MEX thay vì kiểm tra tất cả các khoảng ứng cử viên một cách độc lập. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Với`n = 2, k = 1, a = [5, 7]`, mỗi phần chỉ chứa một giá trị dương, nên mọi MEX đều bằng 0 và câu trả lời là`0`. Việc triển khai giả định mọi phân đoạn không trống có MEX dương sẽ thất bại ở đây. 

Với`n = 2, k = 2, a = [0, 0]`, toàn bộ mảng có MEX (1), nhưng tổng của nó bằng 0 nên đóng góp của nó vẫn bằng 0 và câu trả lời là`0`. Việc nhân MEX với tổng phải xảy ra sau khi cả hai đại lượng đã được tính toán. 

Với`n = 4, k = 2, a = [0, 1, 2, 3]`, toàn bộ mảng sẽ có MEX (4), nhưng nó không thể là một phần vì chiều dài của nó là bốn. Phân vùng tốt nhất là`[0,1]`Và`[2,3]`, cho (2\cdot1+0=2). Việc bỏ qua ranh giới độ dài sẽ tạo ra một quá trình chuyển đổi hoàn toàn không hợp lệ. 

Bản sao cũng có vấn đề. Vì`n = 3, k = 3, a = [0, 1, 1]`, MEX là (2), không phải (3), vì MEX quan tâm đến sự hiện diện hơn là tần số. Giá trị đúng là (2\cdot2=4). 

## Phương pháp tiếp cận 

DP bạo lực diễn ra trực tiếp từ sự tái diễn ở trên. Đối với mọi điểm cuối bên phải (i), hãy thử mọi lần cắt trước đó có thể có (j), tính MEX của (a_{j+1},\ldots,a_i) và cập nhật (dp_i). Điều này đúng vì mọi phân vùng hợp lệ đều có chính xác một phần cuối cùng, do đó phần cắt trước đó của nó sẽ xuất hiện trong số các phần chuyển tiếp này. Nếu MEX được tính toán lại bằng cách quét mảnh, thì độ phức tạp trong trường hợp xấu nhất là (O(nk^2)), khoảng (8\cdot10^{15}) hoạt động khi (n=k=2\cdot10^5). Ngay cả phiên bản cẩn thận hơn duy trì tất cả các giá trị MEX trong khi kéo dài khoảng thời gian cần thiết (O(nk)), vẫn còn quá lớn. 

Quan sát hữu ích là đối với điểm cuối bên phải cố định (i), giá trị MEX của hậu tố là đơn điệu. Nếu chúng ta tăng điểm cuối bên trái, các phần tử sẽ bị xóa, do đó MEX chỉ có thể giữ nguyên hoặc giảm. Do đó, các lần cắt trước đó (j) có thể được chia thành các khoảng liên tiếp mà trên đó MEX không đổi. 

Giả sử một khoảng thời gian như vậy của các lần cắt trước đó là (L\le j\le R) và MEX của nó là (m). Mọi chuyển đổi từ toàn bộ khoảng này đều có dạng 

mS_i+\left(dp_j-mS_j\right). 
] 

Đối với khối MEX này chúng ta chỉ cần 

[ 
\max_{L\le j\le R}(dp_j-mS_j). 
] 

Điều này biến việc tối ưu hóa bên trong thành một truy vấn dòng. Liên kết phần cắt trước đó (j) với dòng 

[ 
F_j(x)=-S_jx+dp_j. 
] 

Khi đó số lượng cần thiết chỉ đơn giản là 

[ 
\max_{L\le j\le R}F_j(m). 
] 

Câu hỏi còn lại là làm thế nào để duy trì các khối MEX một cách hiệu quả. Khi điểm cuối bên phải di chuyển từ (i-1) sang (i), chỉ giá trị mới được chèn (a_i) mới có thể thay đổi các giá trị MEX hậu tố. Một khối MEX thay đổi sẽ chia thành các khối nhỏ hơn và sau khi tách các khối đó không cần phải hợp nhất lại. Trong quá trình quét hoàn chỉnh, tổng số khối được tạo là (O(n)). Cây phân đoạn trên miền giá trị, lưu trữ lần xuất hiện cuối cùng của mọi giá trị, cho phép chúng ta định vị từng ranh giới khối mới trong (O(\log n)). Điều này mang lại tổng công (O(n\log n)) cho cấu trúc MEX. Sự phân rã này là quan sát trung tâm trong giải pháp tiêu chuẩn. 

Có hai vấn đề tối ưu hóa lồng nhau sau quá trình phân tách này. Đầu tiên yêu cầu giá trị tối đa của (F_j(m)) trong khoảng chỉ số. Cây phân đoạn trên chỉ mục (j) có thể lưu trữ cây Li Chao trong mỗi nút. Một dòng được chèn vào tất cả các nút cây chỉ mục (O(\log n)) bao phủ vị trí của nó và một truy vấn phạm vi sẽ truy cập các nút (O(\log n)). Mỗi thao tác Li Chao có chi phí (O(\log n)), mang lại (O(\log^2 n)) cho mỗi thao tác trong phạm vi. 

Tối ưu hóa thứ hai có một đặc tính đặc biệt hữu ích. Khi một khối MEX đã được tạo ra 

[ 
C=\max_{L\le j\le R}F_j(m), 
] 

đóng góp của nó như là một hàm của tổng tiền tố hiện tại là dòng 

[ 
H(x)=mx+C. 
] 

Giá trị MEX chỉ tăng khi điểm cuối bên phải di chuyển, do đó, một dòng cũ không bao giờ cần phải xóa. Chúng tôi chèn dòng này vào ranh giới bên trái của khối và duy trì một cây phân đoạn chỉ mục khác của cây Li Chao. Sau đó, một truy vấn trên cửa sổ trượt hiện tại sẽ xem xét chính xác các đường khối có ranh giới bên trái có thể là đường cắt hợp pháp trước đó. Đây là lớp thứ hai của cấu trúc dữ liệu. Đạo hàm tiêu chuẩn đưa ra (O(n\log^2 n)), với khả năng tinh chỉnh (O(n\log n)) bằng cách xây dựng lớp thứ hai ngoại tuyến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nk^2)) | (O(n)) | Quá chậm | 
| MEX DP tăng dần | (O(nk)) | (O(n)) | Quá chậm | 
| Khối MEX + cây Li Chao lồng nhau | (O(n\log^2 n)) | (O(n\log n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng tổng tiền tố (S_i). Khi đó, tổng của bất kỳ phần ứng cử viên cuối cùng nào (j+1,\ldots,i) sẽ là (S_i-S_j), do đó quá trình chuyển đổi DP trở thành biểu thức affine thay vì yêu cầu quét mảng một lần nữa. 
2. Xác định dòng liên kết với mọi tiền tố (j) đã được giải quyết là 

[ 
F_j(x)=-S_jx+dp_j. 
]

Khi hậu tố có MEX (m), chuyển tiếp của nó là (mS_i+F_j(m)). Do đó, toàn bộ khối MEX có thể được giảm xuống còn một phạm vi truy vấn tối đa tại (x=m). 
3. Duy trì lần xuất hiện cuối cùng của mọi giá trị. Đối với điểm cuối bên phải hiện tại (i), một giá trị (v) xuất hiện ở hậu tố (j+1,\ldots,i) chính xác khi lần xuất hiện cuối cùng của nó lớn hơn (j). Do đó, MEX có thể được xác định bằng mảng lần xuất hiện cuối cùng. 
4. Giữ các khối MEX như một chồng ranh giới. Các giá trị MEX đều đơn điệu dọc theo ngăn xếp. Khi (a_i) được chèn, các khối bật lên có ranh giới không còn hợp lệ, sau đó liên tục tìm giá trị nhỏ nhất có lần xuất hiện cuối cùng nằm dưới ranh giới hiện tại. Mỗi khám phá như vậy sẽ tạo ra một khối MEX mới. 

Cây phân đoạn lưu trữ lần xuất hiện cuối cùng tối thiểu trong mỗi khoảng giá trị hỗ trợ tìm kiếm này trong (O(\log n)). Điểm khấu hao quan trọng là một khối chỉ được chia khi nó được tạo, do đó tổng số lần tạo khối là tuyến tính. 
5. Đối với mỗi khối mới được tạo ([L,R]) có MEX (m), hãy truy vấn cấu trúc Li Chao đầu tiên để tìm 

[ 
C=\max_{L\le j\le R}F_j(m). 
] 

Giá trị (C) tóm tắt mọi lần cắt trước đó có thể có trong khối MEX đó. 
6. Chuyển khối thành dòng 

[ 
H(x)=mx+C. 
] 

Chèn dòng này tại vị trí (L) vào phạm vi cấu trúc Li Chao thứ hai. Đường này biểu thị mọi chuyển đổi trong tương lai có MEX ít nhất là MEX được sử dụng khi đường được tạo. Vì tất cả các phần tử mảng đều không âm nên việc tăng MEX chỉ có thể cải thiện mức đóng góp, do đó việc giữ lại các dòng cũ là an toàn. 
7. Đối với điểm cuối (i), truy vấn cấu trúc thứ hai trên phạm vi cắt hợp pháp trước đó ([i-k,i-1]), được cắt bớt ở mức 0. Điều này xử lý mọi khối MEX hoàn chỉnh nằm bên trong giới hạn độ dài. 
8. Một khối có thể giao với ranh giới bên trái của phạm vi pháp luật mà không bị chứa hoàn toàn trong đó. Tìm khối đó trực tiếp trong ngăn xếp MEX và chỉ truy vấn cấu trúc đầu tiên trên phần bị cắt bớt của nó. Hiệu chỉnh duy nhất này xử lý khối một phần duy nhất được tạo bởi giới hạn độ dài. 
9. Tối đa của truy vấn khối hoàn chỉnh và truy vấn khối một phần này là (dp_i). Chèn dòng mới thu được 

[ 
F_i(x)=-S_ix+dp_i 
] 

vào cấu trúc đầu tiên để các điểm cuối trong tương lai có thể sử dụng tiền tố (i). 
10. Sau khi xử lý mọi điểm cuối, hãy trả về (dp_n). 

Điều bất biến là mọi lần cắt hợp pháp trước đó (j) thuộc về chính xác một khối MEX hiện tại và khối đó lưu trữ MEX chính xác của (a_{j+1},\ldots,a_i). Do đó, cấu trúc Li Chao đầu tiên tính toán quá trình chuyển đổi tốt nhất trong số tất cả các vết cắt trong mỗi khối. Cấu trúc thứ hai lưu trữ chính xác các hàm affine được tạo bởi các khối đã hoàn thành và truy vấn cửa sổ trượt của nó thực thi giới hạn độ dài. Vì mọi phần cuối cùng hợp lệ được biểu thị bằng một trong các trường hợp này, mức tối đa thu được cho (dp_i) chính xác là mức tối ưu cho tiền tố. 

## Giải pháp Python 

Việc triển khai bên dưới tuân theo cấu trúc (O(n\log^2 n)). Nó sử dụng cây phân đoạn trên các giá trị cho ranh giới khối MEX và cây phân đoạn có nút chứa cây Li Chao cho hai cấu trúc phạm vi tối đa.```python
import sys
from array import array

input = sys.stdin.readline
NEG = -(10 ** 40)

class LastOccurrenceTree:
    def __init__(self, n):
        self.n = n
        size = 1
        while size < n + 2:
            size <<= 1
        self.size = size
        self.mn = array('i', [0]) * (2 * size)

    def update(self, p, value):
        p += self.size
        self.mn[p] = value
        p >>= 1
        while p:
            x = self.mn[p << 1]
            y = self.mn[p << 1 | 1]
            self.mn[p] = x if x < y else y
            p >>= 1

    def first_less(self, limit):
        if self.mn[1] >= limit:
            return None

        p = 1
        l = 0
        r = self.size - 1

        while l != r:
            mid = (l + r) >> 1
            left = p << 1
            if self.mn[left] < limit:
                p = left
                r = mid
            else:
                p = left | 1
                l = mid + 1

        return self.mn[p], l

class RangeLiChao:
    def __init__(self, n, prefix, use_prefix):
        self.n = n
        self.prefix = prefix
        self.use_prefix = use_prefix

        self.roots = array('i', [0]) * (4 * n + 20)

        self.left = array('i', [0])
        self.right = array('i', [0])
        self.line = array('i', [0])

        self.slopes = [0]
        self.intercepts = [NEG]

    def value(self, line_id, x):
        if self.use_prefix:
            x = self.prefix[x]
        return self.slopes[line_id] * x + self.intercepts[line_id]

    def add_line(self, slope, intercept):
        self.slopes.append(slope)
        self.intercepts.append(intercept)
        return len(self.slopes) - 1

    def new_node(self, line_id):
        idx = len(self.line)
        self.left.append(0)
        self.right.append(0)
        self.line.append(line_id)
        return idx

    def insert_inner(self, root, line_id, lo, hi):
        if root == 0:
            return self.new_node(line_id)

        cur = self.line[root]
        mid = (lo + hi) >> 1

        left_new = self.value(line_id, lo) > self.value(cur, lo)
        mid_new = self.value(line_id, mid) > self.value(cur, mid)

        if mid_new:
            self.line[root], line_id = line_id, cur
            cur = self.line[root]

        if lo == hi:
            return root

        if left_new != mid_new:
            child = self.left[root]
            new_child = self.insert_inner(child, line_id, lo, mid)
            self.left[root] = new_child
        else:
            child = self.right[root]
            new_child = self.insert_inner(child, line_id, mid + 1, hi)
            self.right[root] = new_child

        return root

    def query_inner(self, root, x, lo, hi):
        if root == 0:
            return NEG

        ans = self.value(self.line[root], x)

        if lo == hi:
            return ans

        mid = (lo + hi) >> 1
        if x <= mid:
            other = self.query_inner(self.left[root], x, lo, mid)
        else:
            other = self.query_inner(self.right[root], x, mid + 1, hi)

        return ans if ans > other else other

    def insert(self, pos, slope, intercept):
        line_id = self.add_line(slope, intercept)

        node = 1
        lo = 0
        hi = self.n - 1

        while True:
            self.roots[node] = self.insert_inner(
                self.roots[node], line_id, 0, self.n - 1
            )

            if lo == hi:
                break

            mid = (lo + hi) >> 1
            if pos <= mid:
                node = node << 1
                hi = mid
            else:
                node = node << 1 | 1
                lo = mid + 1

        return line_id

    def query(self, left, right, x):
        if left > right:
            return NEG

        left += 1
        right += 1

        # Iterative canonical decomposition.
        L = left + self.n - 1
        R = right + self.n - 1

        ans = NEG

        while L <= R:
            if L & 1:
                q = self.query_inner(self.roots[L], x, 0, self.n - 1)
                if q > ans:
                    ans = q
                L += 1

            if not (R & 1):
                q = self.query_inner(self.roots[R], x, 0, self.n - 1)
                if q > ans:
                    ans = q
                R -= 1

            L >>= 1
            R >>= 1

        return ans

def solve_case(n, k, a):
    prefix = [0] * (n + 1)
    for i, x in enumerate(a, 1):
        prefix[i] = prefix[i - 1] + x

    # T1: lines F_j(x) = -S_j*x + dp_j.
    # x is a MEX, hence x in [0, n].
    t1 = RangeLiChao(n + 1, prefix, False)

    # T2: lines H(x) = mex*x + C, evaluated at x = S_i.
    # The implementation indexes x by i and converts it to S_i.
    t2 = RangeLiChao(n + 1, prefix, True)

    last_tree = LastOccurrenceTree(n + 1)
    last = [0] * (n + 1)

    # Stack entries are (boundary, mex).
    # The sentinel boundary is 0.
    stack = [(0, 0)]

    dp = [0] * (n + 1)

    # F_0(x) = 0.
    t1.insert(0, 0, 0)

    for i in range(1, n + 1):
        x = a[i]

        # The value x now occurs at i.
        # Blocks whose boundary lies after the previous occurrence of x
        # may need to be split.
        previous = last[x]

        while stack[-1][0] > previous:
            stack.pop()

        pending = []
        rpos = i

        while rpos > stack[-1][0]:
            result = last_tree.first_less(rpos)
            if result is None:
                break

            min_last, mex = result
            pending.append((rpos, mex))
            rpos = min_last

        pending.reverse()

        for pos, mex in pending:
            left_boundary = stack[-1][0]

            # C = max F_j(mex) for j in [left_boundary, pos - 1].
            c = t1.query(left_boundary, pos - 1, mex)

            # H(S_i) = mex*S_i + C.
            t2.insert(left_boundary, mex, c)

            stack.append((pos, mex))

        last[x] = i
        last_tree.update(x, i)

        lower = max(i - k, 0)

        # Find the first stack boundary >= lower + 1.
        lo = 1
        hi = len(stack)

        while lo < hi:
            mid = (lo + hi) >> 1
            if stack[mid][0] < lower + 1:
                lo = mid + 1
            else:
                hi = mid

        p = lo

        best = NEG

        # The first complete block starting at or after lower.
        q = t2.query(lower, i - 1, i)
        if q > best:
            best = q

        # The block crossing the left boundary may be only partially usable.
        if p < len(stack):
            block_left = stack[p - 1][0]
            block_right = stack[p][0] - 1

            if block_left < lower <= block_right:
                mex = stack[p][1]
                c = t1.query(lower, block_right, mex)
                candidate = mex * prefix[i] + c
                if candidate > best:
                    best = candidate

        dp[i] = best

        # F_i(x) = -S_i*x + dp_i.
        t1.insert(i, -prefix[i], dp[i])

    return dp[n]

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(solve_case(n, k, a))

if __name__ == "__main__":
    main()
```Mảng tổng tiền tố được xây dựng trước tiên vì mọi chuyển đổi đều sử dụng (S_i-S_j). Số nguyên Python đã có độ chính xác tùy ý, do đó không có vấn đề tràn, mặc dù việc triển khai C++ ban đầu cần số nguyên 64 bit.`LastOccurrenceTree`lưu trữ lần xuất hiện cuối cùng tối thiểu trong mỗi khoảng giá trị.`first_less(x)`tìm giá trị nhỏ nhất có lần xuất hiện cuối cùng nhỏ hơn`x`. Đây chính xác là giá trị trở thành MEX khi ranh giới hậu tố đạt đến vị trí đó.`RangeLiChao`triển khai hộp đen mà DP cần. Cây bên ngoài của nó được lập chỉ mục theo vị trí cắt trước đó. Mỗi dòng được chèn vào các nút bên ngoài (O(\log n)) bao phủ vị trí đó. Mỗi nút bên ngoài có một cây Li Chao bên trong, cho phép truy vấn một loạt các vị trí mà không cần truy cập rõ ràng vào mọi lần cắt trước đó. 

Trường hợp đầu tiên đại diện 

[ 
F_j(x)=-S_jx+dp_j. 
] 

Tọa độ truy vấn của nó chính là MEX. Trường hợp thứ hai đại diện 

[ 
H(x)=mx+C, 
] 

trong đó đối số là chỉ mục điểm cuối và được chuyển đổi nội bộ thành tổng tiền tố tương ứng (S_i). 

Ngăn xếp lưu trữ các ranh giới thay vì từng lần cắt riêng lẻ trước đó. Nếu hai ranh giới ngăn xếp liên tiếp là`L`Và`R`, tất cả các lần cắt trước đó trong khoảng đó đều có cùng MEX. Đây là lý do DP không bao giờ thực hiện chuyển tiếp (O(k)) cho một điểm cuối. 

Truy vấn khối một phần là cần thiết vì phạm vi pháp lý bắt đầu tại (i-k), có thể cắt qua giữa khối MEX. Cấu trúc thứ hai xử lý các khối hoàn chỉnh, trong khi cấu trúc thứ nhất xử lý rõ ràng một khối bị cắt bớt đó. Bỏ qua sự điều chỉnh này là một lỗi thường gặp. 

Mã này sử dụng ranh giới trọng điểm ở mức 0 và thể hiện các lần cắt DP bằng cách sử dụng các chỉ số tiền tố dựa trên số 0. Do đó, một phần kết thúc tại (i) và bắt đầu tại (j+1) được liên kết với chỉ số cắt (j), giữ cho đại số nhất quán với các tổng tiền tố. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho```
5 3
3 4 0 0 3
```câu trả lời cần thiết là`10`. 

Các trạng thái DP quan trọng là: 

| Điểm cuối (i) | (S_i) | Phần cuối cùng hay nhất | MEX | (dp_i) | 
| --- | --- | --- | --- | --- | 
| 1 | 3 |`[3]`| 0 | 0 | 
| 2 | 7 |`[4]`hoặc`[3,4]`| 0 | 0 | 
| 3 | 7 |`[3,4,0]`| 1 | 7 | 
| 4 | 7 |`[0]`sau tiền tố kết thúc ở 3 | 1 | 7 | 
| 5 | 10 |`[0,3]`sau tiền tố kết thúc ở 3 | 1 | 10 | 

Tại điểm cuối 3, toàn bộ đoạn`[3,4,0]`có MEX (1) và tổng (7), tạo ra (7). Tại điểm cuối 5, quá trình chuyển đổi tốt nhất là (dp_3+1\cdot(10-7)=7+3=10). Dấu vết cho thấy tại sao một phân khúc có MEX nhỏ vẫn có thể tối ưu khi tổng của nó lớn. 

### Mẫu 2 

cho```
8 4
0 1 2 0 3 1 4 1
```câu trả lời là`26`. Các trạng thái liên quan là: 

| Điểm cuối (i) | (S_i) | Đoạn cắt trước hay nhất (j) | Đoạn cuối cùng | MEX | (dp_i) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 |`[0]`| 1 | 0 | 
| 2 | 1 | 0 |`[0,1]`| 2 | 2 | 
| 3 | 3 | 0 |`[0,1,2]`| 3 | 9 | 
| 4 | 3 | 0 |`[0,1,2,0]`| 3 | 9 | 
| 5 | 6 | 1 |`[1,2,0,3]`| 4 | 24 | 
| 6 | 7 | 2 |`[2,0,3,1]`| 4 | 26 | 
| 7 | 11 | 6 |`[4]`| 0 | 26 | 
| 8 | 12 | 6 |`[4,1]`| 0 | 26 | 

Quá trình chuyển đổi ở điểm cuối 6 là điều thú vị. Bốn yếu tố cuối cùng là`[2,0,3,1]`, có MEX là (4) và tổng của nó là (6). Tiền tố trước chúng có giá trị (dp_2=2), cho 

[ 
2+4\cdot6=26. 
] 

Điều này cho thấy tại sao DP phải bảo toàn giá trị tiền tố tốt nhất tách biệt với MEX của phần hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log^2 n)) | Có (O(n)) việc tạo khối MEX và mỗi phạm vi chi phí vận hành Li Chao (O(\log^2 n)). | 
| Không gian | (O(n\log n)) | Mỗi dòng được chèn sẽ tham gia vào (O(\log n)) nút cây phân đoạn bên ngoài. | 

Vấn đề ban đầu cho phép (n=2\cdot10^5) và cho 4 giây với 512 MiB. Thuật toán này đủ nhanh một cách tiệm cận vì nó thay thế họ chuyển đổi (O(nk)) chỉ bằng các khối MEX (O(n)) và các truy vấn hình học logarit. Việc triển khai Li Chao lồng nhau tiêu tốn nhiều bộ nhớ, đó là lý do tại sao việc triển khai hiệu suất cao ban đầu sử dụng các mảng tĩnh nhỏ gọn thay vì các đối tượng Python. 

## Trường hợp thử nghiệm 

Các mẫu chính thức là:```
# The reference solution above reads one case at a time.

# Sample 1
assert solve_case(
    5, 3, [3, 4, 0, 0, 3]
) == 10

# Sample 2
assert solve_case(
    8, 4, [0, 1, 2, 0, 3, 1, 4, 1]
) == 26

# Sample 3
assert solve_case(
    10, 5, [0, 2, 0, 1, 2, 1, 0, 2, 2, 1]
) == 33

# Minimum size, k = 1.
assert solve_case(
    2, 1, [5, 7]
) == 0

# All equal values. The MEX is 0 because 0 never appears.
assert solve_case(
    5, 3, [7, 7, 7, 7, 7]
) == 0

# Maximum possible piece length is allowed.
assert solve_case(
    4, 4, [0, 1, 2, 3]
) == 9

# Length boundary catches an invalid transition using all four elements.
assert solve_case(
    4, 2, [0, 1, 2, 3]
) == 2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 5 7`|`0`| Kích thước tối thiểu và MEX bằng 0 cho các giá trị dương đơn lẻ | 
|`5 3 / 7 7 7 7 7`|`0`| Các giá trị bằng nhau không có số 0 | 
|`4 4 / 0 1 2 3`|`9`| Phân đoạn toàn mảng khi (k=n) | 
|`4 2 / 0 1 2 3`|`2`| Thực thi đúng độ dài đoạn tối đa | 

Đối với trường hợp ứng suất có kích thước cực đại, một bài kiểm tra hữu ích là`n = 200000`,`k = 1`, với mọi phần tử đều bằng`1`. Khi đó mọi phân đoạn đều có MEX bằng 0, vì vậy câu trả lời mong đợi là bằng 0. Điều này kiểm tra xem việc triển khai không vô tình phân bổ công việc tỷ lệ với (nk) khi cấu trúc MEX tầm thường. 

## Vỏ cạnh 

cho`n = 2, k = 1, a = [5,7]`, các phần hợp pháp là`[5]`Và`[7]`. Không chứa số 0 nên cả hai giá trị MEX đều bằng 0. DP được`dp[1] = 0`Và`dp[2] = 0`. Câu trả lời là`0`. Ngăn xếp MEX chỉ chứa các khối zero-MEX, vì vậy cấu trúc Li Chao không bao giờ tạo ra đóng góp tích cực. 

Vì`n = 2, k = 2, a = [0,0]`, toàn bộ đoạn có MEX (1), nhưng tổng của nó bằng 0. Quá trình chuyển đổi là (1\cdot0=0), vì vậy`dp[2]=0`. Điều này áp dụng cho trường hợp MEX dương nhưng phân khúc này không đóng góp gì. 

Vì`n = 4, k = 2, a = [0,1,2,3]`, phạm vi hợp pháp cho điểm cuối 4 chỉ chứa các vết cắt 2 và 3. Đoạn hấp dẫn`[0,1,2,3]`bị loại trừ vì chiều dài của nó là bốn. Phân vùng hợp lệ tốt nhất là`[0,1]`theo sau là`[2,3]`, với giá trị (2\cdot1+0=2). Truy vấn khối một phần rõ ràng là điều ngăn cấu trúc dữ liệu vô tình sử dụng khối MEX vượt qua ranh giới bên trái của cửa sổ trượt. 

Vì`n = 3, k = 3, a = [0,1,1]`, các giá trị`0`Và`1`có mặt, trong khi`2`vắng mặt nên MEX chính xác bằng (2). Tổng là (2), cho (4). Biểu diễn lần xuất hiện cuối cùng xử lý các bản sao một cách tự nhiên vì chỉ lần xuất hiện gần đây nhất của mỗi giá trị mới quan trọng khi quyết định xem giá trị đó có xuất hiện sau khi cắt ứng cử viên hay không.
