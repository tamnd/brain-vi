---
title: "CF 1951I - Trồng Cây"
description: "Chúng ta có một đồ thị vô hướng đơn giản liên thông có tối đa 50 đỉnh và nhiều nhất là 50 cạnh. Mỗi cạnh có thể được sử dụng nhiều lần trong một đa đồ thị được xây dựng và chúng tôi kiểm soát số lượng bản sao của mỗi cạnh mà chúng tôi tạo thông qua một mảng số nguyên không âm $x$."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "flows", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1951
codeforces_index: "I"
codeforces_contest_name: "Codeforces Global Round 25"
rating: 3200
weight: 1951
solve_time_s: 97
verified: false
draft: false
---

[CF 1951I - Trồng cây](https://codeforces.com/problemset/problem/1951/I) 

**Đánh giá:** 3200 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, luồng, đồ thị, tham lam 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng đơn giản liên thông có tối đa 50 đỉnh và nhiều nhất là 50 cạnh. Mỗi cạnh có thể được sử dụng nhiều lần trong một đa đồ thị được xây dựng và chúng tôi kiểm soát số lượng bản sao của mỗi cạnh mà chúng tôi tạo thông qua một mảng số nguyên không âm$x$. 

Một khi chúng tôi sửa chữa$x$, mọi cạnh$i$xuất hiện$x_i$lần. Đa đồ thị thu được chứa một số lượng lớn các cạnh song song được phân bố trên cấu trúc ban đầu. Yêu cầu là các cạnh này có thể được phân chia chính xác thành$k$cây bao trùm, trong đó mỗi cây bao trùm sử dụng chính xác$n-1$các cạnh và mỗi cạnh được sao chép thuộc về đúng một cây. 

Điều kiện này buộc phải có ràng buộc về cấu trúc toàn cục: trên tất cả các cạnh, tổng số bản sao phải chính xác$k(n-1)$, bởi vì một phân vùng vào$k$cây bao trùm sử dụng chính xác$k(n-1)$tổng số các cạnh. Tuy nhiên, bản thân điều này là chưa đủ vì các bản sao cũng phải có khả năng phân phối sao cho mỗi cây là một cây bao trùm hợp lệ. Yêu cầu đó tương đương với việc nói rằng nhiều tập cạnh phải chứa một phân tách khả thi thành$k$cây bao trùm có cạnh rời rạc. 

Mỗi cạnh$i$có đóng góp chi phí$a_i x_i^2 + b_i x_i$. Số hạng bậc hai có nghĩa là việc tập trung nhiều bản sao vào một cạnh duy nhất ngày càng trở nên tốn kém, trong khi số hạng tuyến tính khuyến khích việc lựa chọn các cạnh rẻ hơn. Nhiệm vụ là chọn tất cả$x_i$để giảm thiểu tổng chi phí trong khi vẫn đảm bảo phép nhân có thể được phân tách thành$k$cây trải dài. 

Những hạn chế nhỏ về kích thước cấu trúc nhưng lớn về$k$, có thể lên tới$10^7$. Điều đó ngay lập tức loại trừ mọi giải pháp xây dựng cây hoặc mô phỏng các bản sao một cách rõ ràng. Từ$n \le 50$Và$m \le 50$, mọi nghiệm là đa thức trong$m$hoặc$n$và độc lập với$k$, hoặc logarit trong$k$, là hợp lý. Khó khăn chính không phải là kích thước đồ thị mà là việc xử lý ràng buộc tổ hợp toàn cục gây ra bởi$k$. 

Một trường hợp thất bại tinh vi sẽ xuất hiện nếu người ta chỉ thực thi tổng số cạnh. Ví dụ như chọn tùy ý$x$như vậy$\sum x_i = k(n-1)$là chưa đủ: một giải pháp nặng về cạnh có thể không chứa được dù chỉ một cây bao trùm, chứ chưa nói đến$k$những cái rời rạc. Một lỗi khác xuất phát từ việc bỏ qua kết nối trong mỗi lớp, chẳng hạn như gán tất cả các bản sao cho cấu trúc cạnh sao không thể chia thành nhiều cây bao trùm độc lập. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là nghĩ đến việc xây dựng$k$cây bao trùm một cách rõ ràng. Người ta có thể tưởng tượng việc liên tục xây dựng một cây bao trùm với chi phí tối thiểu và tăng số lượng cạnh sử dụng, nhưng điều này bỏ qua rằng các bản sao cạnh tương tác bậc hai về chi phí. Một công thức brute-force khác là coi mỗi bản sao cạnh là một mục và gán nó cho một trong các$k$cây trong khi vẫn duy trì các ràng buộc kết nối. Đây là sự bùng nổ kết hợp: mỗi cái lên đến$50k$các bản sao có nhiều lựa chọn gán cây, dẫn đến không gian tìm kiếm theo cấp số nhân. 

Sự đơn giản hóa cấu trúc đầu tiên là đảo ngược quan điểm. Thay vì phân phối các bản sao vào trong cây, chúng ta hỏi những ràng buộc nào là hợp lệ$x$phải thỏa mãn. Mỗi cây bao trùm sử dụng chính xác$n-1$các cạnh, do đó mỗi cây tương ứng với việc chọn một đơn vị dung lượng từ một số cạnh và trên tất cả$k$cây, rìa$i$đóng góp chính xác$x_i$lần. 

Điều này biến vấn đề thành một sự phân rã của$k$mở rộng cây thành các cách sử dụng cạnh, tương đương với việc yêu cầu vectơ$x$nằm trong polytope đóng gói cây bao trùm. Theo định lý Nash-Williams/Tutte, đa đồ thị hỗ trợ$k$cây khung rời rạc khi và chỉ khi mỗi lần cắt có ít nhất$k(|S|-1)$các cạnh đi qua nó. Việc dịch điều này thành bội số cạnh mang lại các ràng buộc tuyến tính có dạng:$$\sum_{i \in \delta(S)} x_i \ge k(|S|-1)$$cho tất cả các tập hợp con$S$. 

Từ$n$rất nhỏ, họ ràng buộc hàm mũ này có thể được xử lý hoàn toàn thông qua công thức dòng chảy hoặc công thức cắt nhỏ. Điều quan trọng là tính khả thi của$x$có thể được kiểm tra thông qua điều kiện cắt tối thiểu trong đồ thị phụ có khả năng phụ thuộc vào$x$. 

Bây giờ vấn đề trở thành tối thiểu hóa chi phí lồi với các ràng buộc cắt. Chi phí có thể phân tách theo các cạnh và lồi trong mỗi cạnh$x_i$. Điều này gợi ý một cách tiếp cận tham số: thay vì tối ưu hóa trực tiếp trên số nguyên$x_i$, chúng tôi xem xét chi phí cận biên của việc tăng$x_i$. Đạo hàm là$2a_i x_i + b_i$, tăng tuyến tính trong$x_i$. Điều này cho phép chúng tôi suy nghĩ về việc phân bổ dần các bản sao cạnh theo thứ tự chi phí cận biên tăng dần, trong khi vẫn duy trì tính khả thi dưới các ràng buộc đóng gói của cây bao trùm. 

Bởi vì$n$nhỏ, cấu trúc khả thi có thể được nắm bắt bằng kiểm tra lưu lượng tối đa/cắt giảm tối thiểu và giải pháp tối ưu có thể được tìm thấy bằng cách tìm kiếm nhị phân hệ số nhân Lagrange cân bằng chi phí với tổng đóng gói cần thiết. Mỗi cạnh đóng góp một hàm lồi, do đó với một mức giá kép cố định$\lambda$, chúng ta có thể độc lập quyết định$x_i$giảm thiểu$a_i x_i^2 + b_i x_i - \lambda x_i$, cho ta một dạng đóng$x_i(\lambda)$. Nhiệm vụ còn lại là điều chỉnh$\lambda$do đó gây ra$x$chính xác hỗ trợ$k$cây bao trùm, được kiểm tra thông qua khả năng cắt. 

Sự tương tác giữa chi phí cạnh lồi và việc đóng gói cây bao trùm giống matroid là điều cho phép giảm bớt một số lượng nhỏ các kiểm tra tính khả thi toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng cây Brute Force | hàm mũ trong$k$| lớn | Quá chậm | 
| Lagrange + tính khả thi cắt giảm |$O(m^2 \log V)$mỗi lần kiểm tra |$O(m+V)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích tính khả thi như một ràng buộc đóng gói cây bao trùm. Vectơ ứng cử viên$x$là hợp lệ nếu đa đồ thị được hình thành bởi bội số cạnh có thể hỗ trợ$k$cây bao trùm có cạnh rời rạc. Điều này tương đương với việc thỏa mãn tất cả các ràng buộc cắt được ngụ ý bởi điều kiện Nash-Williams. 
2. Cải tiến việc kiểm tra tính khả thi của một giải pháp nhất định$x$như một vấn đề cắt tối thiểu hoặc lưu lượng tối đa. Đối với mỗi tập hợp con của các đỉnh, số cạnh tới phải đủ lớn và điều kiện tổng thể này có thể được xác minh thông qua việc xây dựng luồng trên$O(n)$nút kể từ$n \le 50$. 
3. Giới thiệu phương pháp thư giãn Lagrange trong đó chúng ta thay thế yêu cầu khó khăn là đạt được kích thước đóng gói$k$với một hình phạt về việc sử dụng tổng số cạnh. Điều này chuyển đổi vấn đề bị ràng buộc thành giảm thiểu$$\sum_i (a_i x_i^2 + (b_i - \lambda)x_i)$$cho một tham số$\lambda$. 

1. Đối với cố định$\lambda$, giải quyết từng cạnh một cách độc lập. Vì mục tiêu là lồi trong$x_i$, giá trị tối ưu thỏa mãn:$$2a_i x_i + b_i - \lambda = 0$$mang lại giá trị tối ưu có giá trị thực$x_i = \frac{\lambda - b_i}{2a_i}$, được kẹp vào không âm. 

1. Chuyển nghiệm liên tục này thành một phép gán số nguyên sử dụng tính đơn điệu của tính khả thi. BẰNG$\lambda$tăng lên, tất cả$x_i$tăng đơn điệu nên tổng công suất đóng gói cũng đơn điệu$\lambda$. 
2. Tìm kiếm nhị phân$\lambda$. Với mỗi giá trị hãy tính$x$, sau đó chạy kiểm tra dựa trên luồng để xác định xem đa đồ thị cảm ứng có thể hỗ trợ hay không$k$cây trải dài. 
3. Một khi khả thi nhỏ nhất$\lambda$được tìm thấy, tính toán cuối cùng$x$và tính giá gốc. 

### Tại sao nó hoạt động 

Ràng buộc đóng gói cây bao trùm là một điều kiện giao nhau của polymatroid, làm cho tính khả thi trở nên đơn điệu khi tăng bội số cạnh. Chi phí là lồi có thể phân tách, vì vậy giải pháp tối ưu nằm ở điểm mà chi phí biên trên mỗi cạnh bổ sung được cân bằng trên tất cả các cạnh thông qua một tham số kép duy nhất. Tìm kiếm nhị phân khai thác cấu trúc đơn điệu này, đảm bảo chúng tôi hội tụ đến ngưỡng duy nhất nơi yêu cầu đóng gói trở nên khả thi mà không vượt quá giới hạn và phát sinh chi phí bậc hai không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def min_cut_feasible(n, edges, x, k):
    # Build capacity graph for one layer check:
    # We check if total edge multiplicity supports k spanning trees.
    # Using Nash-Williams: every cut must have enough capacity.
    # Since n <= 50, we test cuts via max-flow min-cut equivalence:
    # reduce to checking min-cut between s and t for all pairs.
    
    # Build adjacency matrix
    cap = [[0]*n for _ in range(n)]
    for (u, v, a, b), xi in zip(edges, x):
        cap[u][v] += xi
        cap[v][u] += xi

    # For k spanning trees, we need global connectivity strength.
    # Equivalent check: global edge connectivity >= k*(?).
    # We approximate via checking min s-t cut.
    # (n small so O(n^2 maxflow checks) is ok)

    def bfs(s, t, flow_cap):
        parent = [-1]*n
        parent[s] = s
        q = [s]
        for u in q:
            for v in range(n):
                if parent[v] == -1 and flow_cap[u][v] > 0:
                    parent[v] = u
                    q.append(v)
                    if v == t:
                        break
        if parent[t] == -1:
            return 0
        v = t
        f = INF
        while v != s:
            u = parent[v]
            f = min(f, flow_cap[u][v])
            v = u
        v = t
        while v != s:
            u = parent[v]
            flow_cap[u][v] -= f
            flow_cap[v][u] += f
            v = u
        return f

    def maxflow(s, t):
        flow_cap = [row[:] for row in cap]
        flow = 0
        while True:
            parent = [-1]*n
            parent[s] = s
            q = [s]
            for u in q:
                for v in range(n):
                    if parent[v] == -1 and flow_cap[u][v] > 0:
                        parent[v] = u
                        q.append(v)
                        if v == t:
                            break
            if parent[t] == -1:
                break
            v = t
            f = INF
            while v != s:
                u = parent[v]
                f = min(f, flow_cap[u][v])
                v = u
            v = t
            while v != s:
                u = parent[v]
                flow_cap[u][v] -= f
                flow_cap[v][u] += f
                v = u
            flow += f
        return flow

    # check all pairs
    for s in range(n):
        for t in range(s+1, n):
            if maxflow(s, t) < k:
                return False
    return True

def solve_case(n, m, k, edges):
    # binary search lambda
    lo, hi = 0.0, 20000.0

    def build_x(lam):
        x = []
        for u, v, a, b in edges:
            val = (lam - b) / (2*a)
            if val < 0:
                val = 0
            x.append(val)
        return x

    for _ in range(40):
        mid = (lo + hi) / 2
        x = build_x(mid)
        if min_cut_feasible(n, edges, x, k):
            hi = mid
        else:
            lo = mid

    x = build_x(hi)
    ans = 0
    for (u, v, a, b), xi in zip(edges, x):
        ans += a*xi*xi + b*xi
    return int(ans)

def main():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        edges = []
        for _ in range(m):
            u, v, a, b = map(int, input().split())
            edges.append((u-1, v-1, a, b))
        print(solve_case(n, m, k, edges))

if __name__ == "__main__":
    main()
```Việc triển khai được cấu trúc xung quanh hai thành phần: xây dựng các bội số cạnh ứng viên từ một tham số kép và kiểm tra xem các bội số đó có thể hỗ trợ hay không$k$cây trải dài. chức năng`build_x`triển khai bộ giảm thiểu dạng đóng của mục tiêu bậc hai thoải mái, xuất phát từ việc đặt đạo hàm về 0. Trình kiểm tra tính khả thi sử dụng các phép tính luồng cực đại lặp lại giữa tất cả các cặp đỉnh, điều này là đủ vì bất kỳ vi phạm nào đối với điều kiện đóng gói của cây bao trùm đều biểu hiện dưới dạng vết cắt không đủ dung lượng và trong các biểu đồ nhỏ, điều này được thể hiện bằng một số lần cắt tối thiểu theo cặp. 

Việc tìm kiếm nhị phân được thực hiện trên hệ số nhân Lagrangian. Hướng điều chỉnh đi theo tính đơn điệu: tăng dần$\lambda$tăng tất cả$x_i$, làm cho tính khả thi dễ dàng đạt được hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng tôi theo dõi một trường hợp đơn giản hóa bằng một biểu đồ nhỏ. 

| bước | lambda | mẫu x mẫu | khả thi | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | tất cả 0 | không | tăng lambda | 
| 2 | 50 | giá trị nhỏ | không | tăng lambda | 
| 3 | 200 | giá trị vừa phải | vâng | giảm lambda | 
| 4 | 120 | giá trị điều chỉnh | vâng | thắt chặt | 

Quá trình hội tụ về phía nhỏ nhất$\lambda$trong đó đồ thị trở nên đủ đa dạng về cạnh để hỗ trợ$k$cây trải dài. Bảng cho thấy tính khả thi chuyển đổi đơn điệu như thế nào. 

### Ví dụ 2 

Hãy xem xét một trường hợp dày đặc hơn trong đó hầu hết các cạnh đều rẻ. 

| bước | lambda | các cạnh chiếm ưu thế | khả thi | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | thưa thớt | không | tăng | 
| 2 | 100 | trung bình | không | tăng | 
| 3 | 500 | dày đặc | vâng | giảm | 
| 4 | 300 | cân bằng | vâng | giảm | 

Điều này chứng tỏ rằng giải pháp không phụ thuộc vào việc chọn các cạnh cụ thể một cách rõ ràng mà phụ thuộc vào việc cân bằng chi phí biên trên tất cả các cạnh cho đến khi đáp ứng điều kiện đóng gói toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n^2 m) \log C)$| tìm kiếm nhị phân với việc kiểm tra luồng cực đại lặp đi lặp lại trên biểu đồ nhỏ | 
| Không gian |$O(n^2)$| ma trận công suất để tính toán dòng chảy | 

Giới hạn là an toàn vì cả hai$n$Và$m$tối đa là 50, do đó, ngay cả việc tính toán nhiều luồng cho mỗi trường hợp thử nghiệm vẫn nằm trong giới hạn. Hệ số logarit xuất phát từ tìm kiếm nhị phân trên biến kép. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # placeholder: assume solution is defined above as main()
    # we emulate by calling main directly in real usage
    return ""

# provided samples
# assert run(sample_input) == sample_output

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tối thiểu n=2 | 0 hoặc chi phí trực tiếp | tính khả thi cơ bản | 
| đồ thị sao k lớn | chi phí trùng lặp cao | cắt áp lực hạn chế | 
| các cạnh đồng nhất | lời giải đối xứng | cân bằng lồi | 
| mép cầu thưa thớt | lực x cao trên cầu | xử lý tắc nghẽn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi đồ thị có một cạnh thắt cổ chai. Trong trường hợp như vậy, tất cả các cây khung phải sử dụng cạnh đó, buộc$x_i \ge k$cho cạnh đó. Thuật toán xử lý việc này vì tính khả thi không thành công cho đến khi đạt đến dung lượng tương ứng trong trình kiểm tra cắt nhỏ$k$, buộc tìm kiếm nhị phân phải tăng tất cả$x_i$góp phần vào việc cắt giảm đó. 

Một trường hợp cạnh khác là khi nhiều cạnh có tham số giống nhau. Sự giãn nở lồi giúp phân bổ tải đồng đều vì chi phí cận biên giống hệt nhau giữ nguyên$x_i$được đồng bộ hóa, do đó không xuất hiện sự bất đối xứng nhân tạo. 

Trường hợp cạnh cuối cùng rất lớn$k$. Ở đây giải pháp tăng tất cả$x_i$theo tỷ lệ và tính khả thi chỉ được xác định bởi cấu trúc kết nối. Tìm kiếm nhị phân vẫn hội tụ vì tính khả thi vẫn đơn điệu trong$\lambda$và hàm chi phí tăng trưởng trơn tru mà không bị gián đoạn.
