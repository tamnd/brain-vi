---
title: "CF 1913E - Bài toán ma trận"
description: "Chúng ta bắt đầu với một ma trận nhị phân. Mỗi ô hiện chứa 0 hoặc 1 và chúng tôi được phép thay đổi bất kỳ ô nào thành một trong hai giá trị. Thay đổi một ô được tính là một thao tác. Ma trận cuối cùng phải đáp ứng hai yêu cầu độc lập."
date: "2026-06-08T20:11:10+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1913
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 160 (Rated for Div. 2)"
rating: 2400
weight: 1913
solve_time_s: 168
verified: true
draft: false
---

[CF 1913E - Sự cố ma trận](https://codeforces.com/problemset/problem/1913/E) 

**Đánh giá:** 2400 
**Tags:** luồng, đồ thị 
**Thời gian giải:** 2 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một ma trận nhị phân. Mỗi ô hiện chứa 0 hoặc 1 và chúng tôi được phép thay đổi bất kỳ ô nào thành một trong hai giá trị. Thay đổi một ô được tính là một thao tác. 

Ma trận cuối cùng phải đáp ứng hai yêu cầu độc lập. Đối với mỗi hàng, số lượng hàng phải khớp với tổng hàng quy định. Đối với mỗi cột, số lượng phải khớp với tổng cột quy định. 

Trong số tất cả các ma trận thỏa mãn các yêu cầu về hàng và cột đó, chúng ta cần ma trận yêu cầu ít thay đổi ô nhất so với ma trận ban đầu. 

Kích thước ma trận tối đa là 50 × 50, do đó có tối đa 2500 ô. Nó đủ nhỏ cho các thuật toán đồ thị trên vài nghìn đỉnh và cạnh, nhưng hoàn toàn loại trừ việc liệt kê các ma trận có thể. Một ma trận nhị phân có 2500 ô có$2^{2500}$các cấu hình có thể. 

Trước khi nghĩ đến việc tối ưu hóa, cần có một điều kiện khả thi cơ bản. Tổng số cái mà các hàng yêu cầu phải bằng tổng số cái mà các cột yêu cầu. Nếu như$$\sum A_i \ne \sum B_j,$$thì không có ma trận nào có thể thỏa mãn đồng thời cả hai tập ràng buộc. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như```
2 2
0 0
0 0
2 0
1 1
```Tổng hàng yêu cầu hai số ở hàng đầu tiên và 0 ở hàng thứ hai. Các tổng cột yêu cầu một số trong mỗi cột. Tổng nhu cầu là 2 theo cả hai hướng, do đó tồn tại một giải pháp. 

Một giải pháp bất cẩn chỉ kiểm tra yêu cầu hàng hoặc chỉ kiểm tra yêu cầu cột có thể từ chối trường hợp này một cách không chính xác. 

Một trường hợp quan trọng khác là khi ma trận ban đầu đã thỏa mãn mọi thứ:```
2 2
1 0
0 1
1 1
1 1
```Câu trả lời là 0. Bất kỳ thuật toán nào cố gắng di chuyển một cách tham lam mà không tính đến các vị trí hiện có đều có thể thực hiện các sửa đổi không cần thiết. 

Trường hợp không thể xảy ra cũng phải được phát hiện ngay:```
2 2
0 0
0 0
2 2
1 1
```Hàng yêu cầu bốn cái trong khi cột chỉ yêu cầu hai cái. Không có ma trận nào có thể thỏa mãn cả hai yêu cầu nên câu trả lời đúng là -1. 

Vấn đề tế nhị nhất là việc giảm thiểu những thay đổi không giống như việc xây dựng bất kỳ ma trận khả thi nào. Các ma trận khả thi khác nhau có thể yêu cầu số lần chỉnh sửa khác nhau. Mục tiêu tối ưu hóa phải được xây dựng trực tiếp vào mô hình. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi ma trận nhị phân có kích thước$n \times m$, kiểm tra xem tổng hàng và tổng cột của nó có khớp với mục tiêu hay không và trong số tất cả các ma trận hợp lệ, hãy tính khoảng cách Hamming nhỏ nhất so với ma trận ban đầu. 

Điều này đúng vì nó kiểm tra rõ ràng mọi ma trận cuối cùng của ứng viên. Thật không may, ngay cả ma trận 10 × 10 cũng đã có$2^{100}$khả năng. Với tối đa 2500 ô, sức mạnh vũ phu là hoàn toàn vô vọng. 

Quan sát chính là các ràng buộc mô tả cấu trúc lưỡng cực. 

Mỗi hàng quyết định những thứ cần thiết của nó sẽ đi đâu. Mỗi cột nhận được một số lượng yêu cầu. Nếu chúng ta nghĩ đến việc đưa một cái vào ô$(i,j)$như chọn một cạnh giữa hàng$i$và cột$j$, thì: 

- Hàng$i$phải tham gia chính xác$A_i$các cạnh được chọn. 
- Cột$j$phải tham gia chính xác$B_j$các cạnh được chọn. 

Đây chính xác là một vấn đề về dòng chảy. 

Bây giờ hãy xem xét mục tiêu. Giả sử ô$(i,j)$hiện có 1. 

Nếu chúng ta giữ nó là 1 thì chi phí là 0. 

Nếu chúng ta thay đổi nó thành 0 thì chi phí là 1. 

Tương tự, nếu một ô hiện chứa 0: 

- Giữ nó 0 tốn 0. 
- Đổi thành 1 tốn 1. 

Thay vì giảm thiểu các sửa đổi một cách trực tiếp, việc tối đa hóa số lượng ô không thay đổi sẽ dễ dàng hơn. 

Đối với ma trận cuối cùng được chọn$x$,$$\text{changes} = nm - \text{unchanged cells}.$$Vì vậy, chúng tôi muốn ma trận khả thi bảo toàn số lượng giá trị ban đầu tối đa. 

Điều này biến vấn đề thành một công thức dòng chi phí tối thiểu. 

Đối với mỗi ô: 

- Chọn giá trị cuối cùng 1 sẽ gửi một đơn vị luồng qua cặp hàng-cột đó. 
- Chi phí cạnh phản ánh liệu lựa chọn này có giữ nguyên hay thay đổi ô ban đầu hay không. 

Sử dụng chi phí$$\text{cost}(1)= \begin{cases} 0 & a_{ij}=1\\ 1 & a_{ij}=0 \end{cases}$$tính chi phí để tạo ra ma trận cuối cùng. 

Tuy nhiên, các ô có kết quả là 0 cũng có thể đóng góp chi phí sửa đổi. Để nắm bắt tổng số lượt chỉnh sửa một cách rõ ràng, chúng tôi thay đổi mục tiêu. 

Giả sử mỗi 1 ban đầu đóng góp một chi phí không đổi ban đầu là 1. Sau đó: 

- Nếu số 1 ban đầu vẫn là 1 thì ta được 1. 
- Nếu số 0 ban đầu trở thành 1 thì chúng tôi trả 1. 

Điều này dẫn đến trọng số cạnh$$w_{ij}= \begin{cases} 1 & a_{ij}=1\\ -1 & a_{ij}=0 \end{cases}$$và chúng tôi tìm kiếm tổng trọng số tối đa hóa ma trận khả thi. 

Sau khi đạt được trọng lượng tối đa$W$,$$\text{answer} = (\#\text{original ones}) - W.$$Tối ưu hóa kết quả là luồng hai bên có chi phí tối đa, tương đương với luồng chi phí tối thiểu sau khi phủ nhận chi phí biên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{nm})$|$O(1)$| Quá chậm | 
| Luồng chi phí tối thiểu tối ưu |$O(V^2E)$hoặc tốt hơn cho kích thước này |$O(E)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận và tính số ma trận ban đầu, ký hiệu là`ones`. 
2. Kiểm tra xem$$\sum A_i = \sum B_j.$$Nếu không thì in`-1`vì không có ma trận nào có thể thỏa mãn cả hai bộ yêu cầu. 
3. Xây dựng mạng lưới dòng chảy. 

Tạo một nguồn`S`, một nút cho mỗi hàng, một nút cho mỗi cột và một bồn rửa`T`. 
4. Thêm các cạnh từ`S`tới mỗi nút hàng có dung lượng`A_i`và chi phí`0`. 

Điều này buộc chính xác`A_i`đơn vị lưu lượng rời khỏi hàng`i`, tương ứng chính xác`A_i`những cái ở hàng đó. 
5. Thêm các cạnh từ mỗi nút cột vào`T`với năng lực`B_j`và chi phí`0`. 

Điều này buộc chính xác`B_j`đơn vị dòng chảy vào cột`j`. 
6. Đối với mỗi ô`(i,j)`, thêm một cạnh từ hàng`i`vào cột`j`với năng lực`1`. 

Chi phí của nó là:$$-1 \quad \text{if } a_{ij}=1$$

$$+1 \quad \text{if } a_{ij}=0$$Chúng tôi phủ nhận các trọng số vì luồng chi phí tối thiểu tiêu chuẩn sẽ giảm thiểu chi phí. 
7. Gửi chính xác$$F=\sum A_i$$đơn vị lưu lượng từ nguồn tới bồn. 

Nếu mạng không thể gửi số tiền này thì không tồn tại ma trận khả thi nào và câu trả lời là`-1`. 
8. Hãy để`cost`là tổng chi phí tối thiểu được tìm thấy. 

Vì chi phí đã bị phủ định nên$$W=-cost.$$9. Đầu ra$$\text{ones}-W.$$### Tại sao nó hoạt động 

Mỗi đơn vị luồng tương ứng với việc chọn chính xác một ô ma trận có giá trị cuối cùng là 1. Dung lượng một trên các cạnh hàng-cột đảm bảo một ô được chọn nhiều nhất một lần. Dung lượng hàng nguồn thực thi tổng hàng và dung lượng cột chìm thực thi tổng cột. 

Đối với số 1 ban đầu, việc chọn ô đó sẽ đóng góp trọng số +1. Đối với số 0 ban đầu, việc chọn ô đó sẽ đóng góp trọng số -1. Tổng trọng lượng bằng$$(\text{original 1s kept}) - (\text{original 0s turned into 1}).$$Nếu ma trận ban đầu chứa`ones`thì những cái đó$$\text{changes} = (\text{original 1s removed}) + (\text{original 0s added}) = \text{ones}-W.$$Do đó, việc tối đa hóa trọng lượng hoàn toàn tương đương với việc giảm thiểu số lượng ô được sửa đổi. Thuật toán luồng chi phí tối thiểu tìm ra luồng khả thi tối ưu, do đó số lượng chỉnh sửa thu được là tối thiểu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

class Edge:
    __slots__ = ("to", "rev", "cap", "cost")

    def __init__(self, to, rev, cap, cost):
        self.to = to
        self.rev = rev
        self.cap = cap
        self.cost = cost

class MinCostFlow:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, fr, to, cap, cost):
        fwd = Edge(to, len(self.g[to]), cap, cost)
        rev = Edge(fr, len(self.g[fr]), 0, -cost)
        self.g[fr].append(fwd)
        self.g[to].append(rev)

    def min_cost_flow(self, s, t, need):
        n = self.n
        pot = [0] * n
        flow = 0
        cost = 0

        INF = 10**18

        while flow < need:
            dist = [INF] * n
            parent_v = [-1] * n
            parent_e = [-1] * n

            dist[s] = 0
            pq = [(0, s)]

            while pq:
                d, v = heapq.heappop(pq)
                if d != dist[v]:
                    continue

                for i, e in enumerate(self.g[v]):
                    if e.cap <= 0:
                        continue

                    nd = d + e.cost + pot[v] - pot[e.to]
                    if nd < dist[e.to]:
                        dist[e.to] = nd
                        parent_v[e.to] = v
                        parent_e[e.to] = i
                        heapq.heappush(pq, (nd, e.to))

            if dist[t] == INF:
                return None

            for v in range(n):
                if dist[v] < INF:
                    pot[v] += dist[v]

            add = need - flow
            v = t

            while v != s:
                pv = parent_v[v]
                pe = parent_e[v]
                add = min(add, self.g[pv][pe].cap)
                v = pv

            v = t
            while v != s:
                pv = parent_v[v]
                pe = parent_e[v]
                e = self.g[pv][pe]

                e.cap -= add
                self.g[v][e.rev].cap += add

                v = pv

            flow += add
            cost += add * pot[t]

        return cost

def solve():
    n, m = map(int, input().split())

    a = [list(map(int, input().split())) for _ in range(n)]
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    if sum(A) != sum(B):
        print(-1)
        return

    original_ones = sum(sum(row) for row in a)

    S = 0
    row_start = 1
    col_start = row_start + n
    T = col_start + m
    N = T + 1

    mcf = MinCostFlow(N)

    for i in range(n):
        mcf.add_edge(S, row_start + i, A[i], 0)

    for j in range(m):
        mcf.add_edge(col_start + j, T, B[j], 0)

    for i in range(n):
        for j in range(m):
            cost = -1 if a[i][j] == 1 else 1
            mcf.add_edge(row_start + i, col_start + j, 1, cost)

    required_flow = sum(A)

    cost = mcf.min_cost_flow(S, T, required_flow)

    if cost is None:
        print(-1)
        return

    max_weight = -cost
    answer = original_ones - max_weight
    print(answer)

if __name__ == "__main__":
    solve()
```Mạng chứa một nút trên mỗi hàng và một nút trên mỗi cột. Một đơn vị luồng qua cạnh hàng-cột có nghĩa là ô tương ứng được gán giá trị 1 trong ma trận cuối cùng. 

Dung lượng hàng và dung lượng cột mã hóa chính xác tổng số hàng và cột được yêu cầu. Vì mỗi cạnh ô có dung lượng 1 nên không ô nào có thể đóng góp nhiều hơn một ô được chọn. 

Phần tinh tế nhất là việc phân bổ chi phí. Một bản gốc nhận được chi phí`-1`, trong khi số 0 ban đầu nhận được chi phí`+1`. Thuật toán luồng chi phí tối thiểu tối thiểu hóa tổng chi phí, tương đương với việc tối đa hóa trọng số được mô tả trong bằng chứng. Sau khi tính toán luồng, công thức cuối cùng sẽ chuyển đổi trọng số tối đa đó thành số lần chỉnh sửa. 

Việc triển khai sử dụng các đường tăng cường ngắn nhất liên tiếp với tiềm năng Johnson. Chi phí biên âm chỉ xuất hiện ở các cạnh ô chuyển tiếp nhưng không có chu kỳ âm. Tiềm năng làm cho tất cả các chi phí giảm được không âm, cho phép sử dụng thuật toán Dijkstra một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
0 0 0
0 0 0
0 0 0
1 1 1
1 1 1
```Ma trận ban đầu chứa số 0. 

Lưu lượng yêu cầu là 3. 

| Ô được chọn | Đóng góp trọng lượng | Tổng trọng lượng | 
| --- | --- | --- | 
| (1,1) | -1 | -1 | 
| (2,2) | -1 | -2 | 
| (3,3) | -1 | -3 | 

Tất cả các ô được chọn đều đến từ số 0 ban đầu. 

| Số lượng | Giá trị | 
| --- | --- | 
| Bản gốc | 0 | 
| Trọng lượng tối đa | -3 | 
| Trả lời | 3 | 

Đầu ra:```
3
```Dấu vết cho thấy rằng mọi yêu cầu đều phải được tạo từ số 0, tạo ra chính xác ba lần chỉnh sửa. 

### Ví dụ 2```
2 2
1 0
0 1
1 1
1 1
```Ma trận đã đáp ứng được mục tiêu. 

| Ô được chọn | Đóng góp trọng lượng | Trọng lượng chạy | 
| --- | --- | --- | 
| (1,1) | +1 | 1 | 
| (2,2) | +1 | 2 | 

| Số lượng | Giá trị | 
| --- | --- | 
| Bản gốc | 2 | 
| Trọng lượng tối đa | 2 | 
| Trả lời | 0 | 

Đầu ra:```
0
```Quy trình chọn chính xác các ô đã là ô, giữ nguyên mọi giá trị ban đầu và không yêu cầu chỉnh sửa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(V^2E)$| Luồng chi phí tối thiểu trên đường tăng ngắn nhất liên tiếp | 
| Không gian |$O(E)$| Lưu trữ đồ thị | 

Đối với vấn đề này,$$V = n + m + 2 \le 102$$Và$$E = nm + n + m \le 2600.$$Các giá trị này rất nhỏ đối với việc triển khai luồng chi phí tối thiểu, do đó giải pháp phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    pass

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# sample 1
assert run(
"""3 3
0 0 0
0 0 0
0 0 0
1 1 1
1 1 1
"""
) == "3\n"

# already valid matrix
assert run(
"""2 2
1 0
0 1
1 1
1 1
"""
) == "0\n"

# impossible totals
assert run(
"""2 2
0 0
0 0
2 2
1 1
"""
) == "-1\n"

# all ones already
assert run(
"""2 2
1 1
1 1
2 2
2 2
"""
) == "0\n"

# minimum dimensions
assert run(
"""2 2
0 0
0 0
1 0
1 0
"""
) == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 3 | Tạo tất cả những cái cần thiết | 
| Ma trận đã hợp lệ | 0 | Không chỉnh sửa không cần thiết | 
| Tổng số không khớp | -1 | Kiểm tra tính khả thi | 
| Tất cả những cái đó rồi | 0 | Bảo tồn cấu trúc hiện có | 
| trường hợp ranh giới 2×2 | 1 | Kích thước hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Xét trường hợp không thể toàn phần:```
2 2
0 0
0 0
2 2
1 1
```Các yêu cầu của hàng có tổng bằng 4, trong khi các yêu cầu của cột có tổng bằng 2. Trước khi bắt đầu tính toán luồng, thuật toán sẽ phát hiện sự không khớp và trả về`-1`. Không có ma trận khả thi nào có thể tồn tại vì mỗi ma trận đều đóng góp đồng thời vào một số hàng và một số cột. 

Hãy xem xét một ma trận đã đáp ứng tất cả các yêu cầu:```
2 2
1 0
0 1
1 1
1 1
```Nhu cầu luồng là 2. Các cạnh rẻ nhất chính xác là hai ô đã chứa các ô đó, mỗi ô có chi phí`-1`. Thuật toán gửi luồng qua các ô đó, thu được tổng chi phí`-2`, tính trọng lượng`2`, và trả về`2 - 2 = 0`. 

Hãy xem xét trường hợp tồn tại nhiều ma trận khả thi:```
2 2
1 0
0 0
1 1
1 1
```Hai ma trận cuối cùng thỏa mãn tổng hàng và cột. Người ta giữ lại ô hiện có và chỉ thay đổi một ô bổ sung. Một cái khác loại bỏ cái hiện có và tạo ra hai cái mới. Mục tiêu luồng thích cấu hình đầu tiên hơn vì việc giữ cấu hình ban đầu sẽ góp phần tạo ra trọng số dương. Câu trả lời kết quả là số lần chỉnh sửa tối thiểu thực sự, không chỉ đơn thuần là bất kỳ số lượng khả thi nào. 

Những ví dụ này minh họa tại sao tính khả thi và tối ưu hóa phải được xử lý cùng nhau, đó chính xác là những gì việc xây dựng luồng chi phí tối thiểu đạt được.
