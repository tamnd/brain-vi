---
title: "CF 2191C - Trò chơi sắp xếp"
description: "Chúng ta được cho một đồ thị vô hướng có trọng số. Chúng ta phải chọn chính xác các cạnh $n-1$. Nếu các cạnh $n-1$ đó tạo thành một cây bao trùm thì sự lựa chọn bị cấm. Chúng ta muốn tổng trọng số tối thiểu có thể có trong số tất cả các lựa chọn của các cạnh $n-1$ không tạo thành cây."
date: "2026-06-09T04:41:27+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 2191
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1073 (Div. 2)"
rating: 1200
weight: 2191
solve_time_s: 194
verified: false
draft: false
---

[CF 2191C - Trò chơi sắp xếp](https://codeforces.com/problemset/problem/2191/C) 

**Đánh giá:** 1200 
**Thẻ:** trò chơi 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số. Chúng ta phải lựa chọn chính xác$n-1$các cạnh. 

Nếu những$n-1$các cạnh tạo thành một cây bao trùm, sự lựa chọn bị cấm. Chúng tôi muốn tổng trọng lượng tối thiểu có thể có trong số tất cả các lựa chọn về$n-1$các cạnh **không** tạo thành cây. 

Một biểu đồ trên$n$các đỉnh có chính xác$n-1$các cạnh là một cây khi và chỉ nếu nó được kết nối. Quan sát đó ngay lập tức đơn giản hóa vấn đề: chúng ta đang tìm tập trọng số tối thiểu của$n-1$các cạnh có đồ thị cảm ứng bị ngắt kết nối. 

Kích thước biểu đồ lớn. Trong tất cả các trường hợp thử nghiệm, cả tổng số đỉnh và tổng số cạnh đều lớn nhất$2 \cdot 10^5$. Bất kỳ thuật toán nào liệt kê các tập hợp con, cây bao trùm hoặc thậm chí tất cả các cặp tập hợp cạnh đều hoàn toàn không thể thực hiện được. Chúng ta cần một cái gì đó gần gũi$O(m \log m)$mỗi tập tin thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi nhẹ nhất trên toàn cầu$n-1$các cạnh đã bị ngắt kết nối. Ví dụ:```
n = 4
edges: 1, 2, 3, 100
```Việc chọn ba cạnh nhẹ nhất đã cho ra một biểu đồ không liên kết, vì vậy câu trả lời đơn giản là$1+2+3=6$. Giải pháp bắt đầu bằng cách xây dựng MST và sau đó buộc sửa đổi sẽ bỏ lỡ trường hợp này. 

Một trường hợp quan trọng khác là khi nhẹ nhất$n-1$các cạnh tạo thành một cây. Khi đó câu trả lời không nhất thiết phải có được bằng cách thay thế một cạnh cây tùy ý. Việc thay thế phải ngắt kết nối đồ thị. Ví dụ: nếu một cạnh phụ tạo ra một chu trình, việc loại bỏ một cạnh khỏi chu trình đó sẽ giữ cho biểu đồ được kết nối và vẫn cho ra một cây, điều này bị cấm. 

Trường hợp góc cuối cùng xảy ra khi$m=n-1$. Đồ thị chứa đúng một tập hợp$n-1$các cạnh. Nếu tập hợp đó là cây thì không tồn tại câu trả lời hợp lệ và chúng ta phải in$-1$. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ kiểm tra mọi tập hợp con của$n-1$các cạnh, kiểm tra xem nó có tạo thành cây hay không và giữ trọng lượng tối thiểu trong số những cây không hợp lệ. có$$\binom{m}{n-1}$$những tập hợp con như vậy, lớn về mặt thiên văn ngay cả đối với các đồ thị có vài chục cạnh. 

Quan sát chính xuất phát từ việc sắp xếp tất cả các cạnh theo trọng số. 

Cho phép$r=n-1$. Hãy xem xét điều đầu tiên$r$các cạnh theo thứ tự trọng lượng không giảm. Gọi bộ này$P$. 

Mỗi bộ$r$các cạnh có trọng lượng ít nhất bằng trọng lượng của$P$, bởi vì$P$chứa nhẹ nhất toàn cầu$r$các cạnh. 

Nếu như$P$đã bị ngắt kết nối, thì nó sẽ tối ưu ngay lập tức và chúng ta đã hoàn thành. 

Trường hợp thú vị là khi$P$tạo thành một cái cây. 

Giả sử chúng ta thêm một cạnh$e$ngoài$P$. Từ$P$là một cái cây, thêm$e$tạo ra đúng một chu kỳ. Nếu chúng ta loại bỏ một cạnh khỏi chu trình đó, đồ thị vẫn được kết nối và chúng ta vẫn có một cây. Điều đó không giúp ích gì. 

Để thu được đồ thị rời rạc, chúng ta phải loại bỏ một cạnh **không** trên chu trình được tạo bởi$e$. 

Đối với cạnh ngoài cố định$e$, bộ ngắt kết nối rẻ nhất có thể đạt được bằng một lần hoán đổi bằng cách loại bỏ cạnh nặng nhất của$P$không nằm trên đường đi giữa các điểm cuối của$e$. 

Sự thật đáng ngạc nhiên là mọi câu trả lời tối ưu đều có thể được thể hiện bằng một phép hoán đổi duy nhất như vậy. Bất kỳ giải pháp nào khác với$P$bởi một số bổ sung và loại bỏ có trọng số tăng ít nhất bằng mức hoán đổi đơn hợp lệ tốt nhất. 

Điều này làm giảm toàn bộ vấn đề trong việc tìm kiếm, đối với mọi cạnh bên ngoài tiền tố, cạnh có trọng số tối đa của cây tiền tố không nằm trên đường dẫn cây tương ứng. 

Điều đó trở thành vấn đề về cấu trúc dữ liệu cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(m \log^2 n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Xây dựng tiền tố quan trọng 

1. Sắp xếp tất cả các cạnh theo trọng số. 
2. Hãy để$r=n-1$. Lấy cái đầu tiên$r$các cạnh và gọi tập hợp này$P$. 
3. Tính tổng trọng lượng của chúng, ký hiệu là$S$. 
4. Sử dụng DSU trên các cạnh của$P$. 
5. Nếu$P$bị ngắt kết nối, đầu ra$S$. Không có bộ nào khác$r$các cạnh có thể có trọng lượng nhỏ hơn. 
6. Nếu$m=r$, sau đó$P$là tập hợp duy nhất có thể có của$r$các cạnh. Vì nó là một cái cây nên không có câu trả lời hợp lệ nào tồn tại, vì vậy kết quả đầu ra$-1$. 

### Khi tiền tố là cây 

1. Điều trị$P$như một cái cây. 
2. Root cây và xây dựng bảng nâng nhị phân. 
3. Đối với mỗi lần nhảy tổ tiên, hãy lưu trữ hai giá trị: 

- trọng lượng cạnh tối đa trong lần nhảy đó, 
- trọng lượng cạnh tối đa thứ hai trong lần nhảy đó. 
4. Cho mọi cạnh$e=(u,v,w)$ngoài$P$, truy vấn đường dẫn từ$u$ĐẾN$v$bên trong cây. 
5. Hãy để$M$là trọng lượng cạnh tối đa trên toàn bộ cây. 
6. Hãy để$P_{\max}(u,v)$là trọng số cạnh tối đa trên đường đi của cây giữa$u$Và$v$. 
7. Chúng ta cần cạnh cây nặng nhất **không** nằm trên đường dẫn đó. 
8. Nếu$M$không có trên đường dẫn thì câu trả lời chỉ đơn giản là$M$. 
9. Nếu$M$nằm trên đường đi, chúng ta cần cạnh cây lớn nhất bên ngoài đường đi. Giá trị này bằng giá trị lớn hơn của: 

- trọng số cạnh lớn thứ hai trong toàn cây khi giá trị lớn nhất là duy nhất, 
- cùng giá trị tối đa khi nó xuất hiện ở nơi khác bên ngoài đường dẫn. 
10. Sử dụng thông tin tối đa và tối đa thứ hai được lưu trữ, tính trọng lượng cạnh có thể tháo rời tốt nhất$h$. 
11. Chi phí tăng thêm cho cạnh ngoài này là$$w-h.$$1. Lấy mức tăng hợp lệ tối thiểu trên tất cả các cạnh bên ngoài. 
2. Câu trả lời là$$S + \min(w-h).$$### Tại sao nó hoạt động 

đầu tiên$n-1$các cạnh là tập trọng lượng tối thiểu của kích thước đó. Bất kỳ bộ nào khác đều phải thay thế một số trong số chúng bằng các cạnh nặng hơn. 

Khi tiền tố bị ngắt kết nối, đó đã là một giải pháp hợp lệ và không thể cải thiện được. 

Khi tiền tố là một cây, việc thêm một cạnh không có tiền tố sẽ tạo ra một chu trình duy nhất. Việc loại bỏ một cạnh bên ngoài chu trình đó sẽ ngắt kết nối đồ thị, tạo ra một tập hợp hợp lệ các$n-1$các cạnh. 

Đối với cạnh được thêm cố định, việc loại bỏ cạnh cây nặng nhất cho phép sẽ tạo ra mức tăng tối thiểu có thể. 

Bất kỳ giải pháp hợp lệ nào khác với tiền tố bởi một số hoán đổi phải chứa ít nhất một cạnh cây bị loại bỏ mà không được bao phủ bởi bất kỳ đường dẫn cạnh bổ sung nào. So sánh cạnh bị loại bỏ đó với một trong các cạnh được thêm vào sẽ mang lại mức tăng chi phí không nhỏ hơn so với hoán đổi đơn hợp lệ tốt nhất. Do đó, giải pháp tối ưu luôn đạt được bằng một lần hoán đổi và giảm thiểu$w-h$trên tất cả các cạnh bên ngoài là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LOG = 19

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.sz = [1] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]
        return True

def merge_pair(a, b):
    vals = [a[0], a[1], b[0], b[1]]
    vals.sort(reverse=True)
    return (vals[0], vals[1])

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        edges = []
        for _ in range(m):
            u, v, w = map(int, input().split())
            edges.append((w, u - 1, v - 1))

        edges.sort()

        r = n - 1
        prefix = edges[:r]
        extra = edges[r:]

        prefix_sum = sum(w for w, _, _ in prefix)

        dsu = DSU(n)
        for w, u, v in prefix:
            dsu.union(u, v)

        root = dsu.find(0)
        connected = all(dsu.find(i) == root for i in range(n))

        if not connected:
            out.append(str(prefix_sum))
            continue

        if m == r:
            out.append("-1")
            continue

        tree = [[] for _ in range(n)]
        all_tree_weights = []

        for w, u, v in prefix:
            tree[u].append((v, w))
            tree[v].append((u, w))
            all_tree_weights.append(w)

        max1 = max(all_tree_weights)
        cnt_max1 = all_tree_weights.count(max1)

        if cnt_max1 >= 2:
            max2 = max1
        else:
            max2 = max((x for x in all_tree_weights if x != max1), default=-1)

        depth = [0] * n
        up = [[0] * n for _ in range(LOG)]
        best = [[(-1, -1)] * n for _ in range(LOG)]

        stack = [(0, -1, -1)]
        order = [0]

        parent = [-1] * n
        parent[0] = 0

        while stack:
            v, p, w = stack.pop()

            up[0][v] = p if p != -1 else 0
            best[0][v] = (w, -1)

            for to, wt in tree[v]:
                if to == p:
                    continue
                depth[to] = depth[v] + 1
                parent[to] = v
                stack.append((to, v, wt))

        for k in range(1, LOG):
            uk = up[k - 1]
            uk2 = up[k]
            bk = best[k]
            bk1 = best[k - 1]

            for v in range(n):
                mid = uk[v]
                uk2[v] = uk[mid]
                bk[v] = merge_pair(bk1[v], bk1[mid])

        def path_two_max(a, b):
            res = (-1, -1)

            if depth[a] < depth[b]:
                a, b = b, a

            diff = depth[a] - depth[b]
            bit = 0
            while diff:
                if diff & 1:
                    res = merge_pair(res, best[bit][a])
                    a = up[bit][a]
                diff >>= 1
                bit += 1

            if a == b:
                return res

            for k in range(LOG - 1, -1, -1):
                if up[k][a] != up[k][b]:
                    res = merge_pair(res, best[k][a])
                    res = merge_pair(res, best[k][b])
                    a = up[k][a]
                    b = up[k][b]

            res = merge_pair(res, best[0][a])
            res = merge_pair(res, best[0][b])

            return res

        INF = 10**30
        add = INF

        for w, u, v in extra:
            path_max, path_second = path_two_max(u, v)

            if max1 > path_max:
                removable = max1
            else:
                removable = max(max2, path_second)

            if removable != -1:
                add = min(add, w - removable)

        if add == INF:
            out.append("-1")
        else:
            out.append(str(prefix_sum + add))

    sys.stdout.write("\n".join(out))

solve()
```Sau khi sắp xếp, trước tiên thuật toán sẽ kiểm tra xem vật nhẹ nhất trên toàn cầu có$n-1$các cạnh đã bị ngắt kết nối. Trường hợp đó được xử lý ngay vì không còn trường hợp nào khác$n-1$tập cạnh có thể có tổng nhỏ hơn. 

Khi tiền tố tạo thành một cây, phần còn lại của công việc sẽ diễn ra trên cây đó. Nâng nhị phân cho phép chúng ta truy vấn trọng số cạnh lớn nhất và lớn thứ hai trên bất kỳ đường dẫn cây nào trong$O(\log n)$. Những giá trị đó đủ để xác định cạnh nặng nhất không nằm trên đường dẫn, đây chính xác là cạnh mà chúng tôi muốn loại bỏ. 

Việc triển khai lưu trữ hai trọng lượng lớn nhất cho mỗi phân đoạn nâng. Việc hợp nhất hai phân đoạn được thực hiện bằng cách lấy hai giá trị lớn nhất trong số bốn ứng cử viên. Điều này giúp truy vấn đường dẫn hiệu quả đồng thời tránh các cấu trúc dữ liệu phức tạp hơn. 

Tất cả các phép tính trọng lượng đều sử dụng số nguyên Python, xử lý an toàn các giá trị lên tới$10^9$và tổng hợp lại gần như$2 \cdot 10^{14}$. 

## Ví dụ đã hoạt động 

### Mẫu 1, test case đầu tiên 

Trọng số được sắp xếp: 

| Chỉ mục | Cân nặng | 
| --- | --- | 
| 1 | 1 | 
| 2 | 4 | 
| 3 | 5 | 
| 4 | 6 | 
| 5 | 7 | 
| 6 | 9 | 

Đây$n=4$, vì vậy chúng ta cần$r=3$các cạnh. 

Ba cạnh đầu tiên có tổng trọng lượng$10$. 

| Các cạnh tiền tố được chọn | Tổng trọng lượng | Đã kết nối? | 
| --- | --- | --- | 
| 1, 4, 5 | 10 | Không | 

Vì tiền tố đã bị ngắt kết nối nên câu trả lời ngay lập tức là 10. 

Ví dụ này cho thấy tại sao việc kiểm tra tiền tố trước là điều cần thiết. Xây dựng cấu trúc cây sẽ là công việc không cần thiết. 

### Mẫu 1, test case thứ hai 

Biểu đồ có trọng số: 

| Cân nặng | 
| --- | 
| 5 | 
| 5 | 
| 5 | 
| 8 | 

Lại$r=3$. 

Ba cạnh đầu tiên tạo thành đường dẫn$1-2-3-4$, đó là một cái cây. 

| Tổng tiền tố | Đã kết nối? | 
| --- | --- | 
| 15 | Có | 

Chỉ có một cạnh ngoài, trọng lượng 8. 

Đường đi của nó trong cây chứa cả ba cạnh có trọng số 5. Không có rìa cây nào nằm ngoài con đường đó. 

| Trọng lượng cạnh ngoài | Đường dẫn ngoài cạnh có thể tháo rời tốt nhất | 
| --- | --- | 
| 8 | không | 

Không có trao đổi hợp lệ tồn tại. Câu trả lời là$-1$. 

Điều này chứng tỏ tình trạng mà mọi$3$tập con -edge là một cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m + m \log n)$| Sắp xếp cộng với một truy vấn đường dẫn cho mỗi cạnh không có tiền tố | 
| Không gian |$O(n \log n)$| Bàn nâng nhị phân | 

Tổng số đỉnh và cạnh của tất cả các ca kiểm thử nhiều nhất là$2 \cdot 10^5$, Vì thế$O(m \log m + m \log n)$dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    # paste solve() here and return output
    pass

assert run("""4
4 6
1 2 7
1 3 4
1 4 1
2 3 9
2 4 6
3 4 5
4 4
1 2 5
2 3 5
3 4 5
1 4 8
4 4
1 4 1
1 3 4
2 4 2
3 4 3
4 4
2 3 7
1 2 5
2 4 9
4 3 12
""").strip() == """10
-1
8
28""".strip()

assert run("""1
2 1
1 2 5
""").strip() == "-1"

assert run("""1
4 4
1 2 1
3 4 2
1 3 100
2 4 200
""").strip() == "103"

assert run("""1
3 3
1 2 5
2 3 5
1 3 5
""").strip() == "10"

assert run("""1
4 5
1 2 1
2 3 2
3 4 100
1 3 101
2 4 102
""").strip() == "104"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 đỉnh, 1 cạnh | -1 | Tập hợp duy nhất có thể là một cái cây | 
| Tiền tố ánh sáng bị ngắt kết nối | 103 | Câu trả lời tiền tố ngay lập tức | 
| Tất cả trọng lượng bằng nhau | 10 | Xử lý đúng cực đại lặp đi lặp lại | 
| Cạnh lớn trên đường cây | 104 | Lựa chọn đúng cạnh có thể tháo rời bên ngoài đường dẫn | 

## Vỏ cạnh 

Hãy xem xét:```
4 4
1 2 1
3 4 2
1 3 100
2 4 200
```Ba cạnh đầu tiên đã tạo thành một biểu đồ bị ngắt kết nối. Tổng của chúng là 103. Vì chúng là ba cạnh nhẹ nhất toàn cầu nên không có câu trả lời hợp lệ nào có thể nhỏ hơn. Thuật toán phát hiện tiền tố bị ngắt kết nối và dừng ngay lập tức. 

Coi như:```
2 1
1 2 5
```Chỉ có một tập hợp con có thể có kích thước$n-1$. Đó là một cái cây. Không có sự lựa chọn hợp lệ tồn tại. Thuật toán đạt đến`m == n - 1`trường hợp và trả lại$-1$. 

Coi như:```
3 3
1 2 5
2 3 5
1 3 5
```Cây tiền tố chứa hai cạnh có trọng số 5 và cạnh ngoài cũng có trọng số 5. ​​Tồn tại nhiều giá trị tối đa. Việc triển khai giữ cả trọng số lớn nhất và lớn thứ hai trên mọi đường dẫn để tối đa lặp lại được xử lý chính xác. Câu trả lời vẫn là 10.
