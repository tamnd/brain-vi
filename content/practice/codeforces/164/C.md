---
title: "CF 164C - Lập trình máy"
description: "Chúng tôi có một số công việc, mỗi công việc có thời gian bắt đầu, thời gian và lợi nhuận. Một máy bắt đầu một công việc vẫn được bận trong toàn bộ khoảng thời gian từ s đến s + t - 1. Nhiều nhất k công việc có thể chạy đồng thời vì chúng ta chỉ sở hữu k máy. Mục tiêu không phải là lên lịch cho tất cả các công việc."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 164
codeforces_index: "C"
codeforces_contest_name: "VK Cup 2012 Round 3"
rating: 2400
weight: 164
solve_time_s: 136
verified: true
draft: false
---

[CF 164C - Lập trình máy](https://codeforces.com/problemset/problem/164/C) 

**Đánh giá:** 2400 
**Tags:** luồng, đồ thị 
**Thời gian giải:** 2 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số công việc, mỗi công việc có thời gian bắt đầu, thời gian và lợi nhuận. Một máy bắt đầu một công việc sẽ luôn bận rộn trong suốt khoảng thời gian từ`s`bởi vì`s + t - 1`. Nhiều nhất`k`công việc có thể chạy đồng thời vì chúng tôi chỉ sở hữu`k`máy móc. 

Mục tiêu không phải là lên lịch cho tất cả các công việc. Chúng tôi có thể loại bỏ một số trong số họ. Chúng tôi muốn tập hợp con có tổng lợi nhuận tối đa trong khi không bao giờ vượt quá`k`công việc chồng chéo bất cứ lúc nào. 

Đầu ra là một mảng nhị phân. Chức vụ`i`bằng`1`nếu`i`-công việc thứ được chọn và`0`nếu không thì. 

Các ràng buộc hoàn toàn định hình giải pháp. Có nhiều nhất`1000`công việc đủ nhỏ để xây dựng đồ thị với khoảng`O(n^2)`các cạnh nhưng quá lớn để tìm kiếm theo cấp số nhân. Số lượng máy nhiều nhất là`50`, điều này gợi ý một số loại luồng hoặc DP vượt quá công suất hạn chế. Đạt được tọa độ thời gian`10^9`, vì vậy việc lặp lại mọi thời điểm là không thể. Mọi giải pháp hợp lệ đều phải nén dòng thời gian xuống chỉ còn những điểm sự kiện có ý nghĩa. 

Một chi tiết tinh tế là định nghĩa khoảng. Một công việc chiếm giữ`[s, s+t-1]`xung đột với công việc khác bắt đầu trước đó`s+t`. Ví dụ:```
2 1
1 2 5
3 1 7
```Công việc đầu tiên chiếm nhiều thời gian`1,2`. Thứ hai bắt đầu lúc`3`, do đó chúng không trùng nhau và cả hai đều có thể được chọn. Thực hiện bất cẩn bằng cách sử dụng các khoảng thời gian đóng`[s, s+t]`sẽ từ chối cặp này một cách không chính xác. 

Một sai lầm dễ dàng khác xuất hiện khi nhiều công việc có cùng thời gian bắt đầu hoặc kết thúc.```
3 1
1 2 5
1 1 4
2 1 4
```Công việc đầu tiên chiếm`1,2`. Thứ hai chiếm`1`. Người thứ ba chiếm giữ`2`. Chúng ta không thể thực hiện công việc đầu tiên cùng với công việc nhỏ hơn, nhưng chúng ta có thể thực hiện công việc thứ hai và thứ ba cùng nhau để có tổng lợi nhuận`8`. Bất kỳ việc triển khai nào sắp xếp không chính xác hoặc xử lý các tọa độ bằng nhau không nhất quán có thể vô tình cho phép ba công việc chồng chéo cùng một lúc`1`hoặc`2`. 

Trường hợp nguy hiểm cuối cùng là khi có nhiều công việc chồng chéo nhưng`k > 1`.```
4 2
1 5 10
2 3 8
3 2 7
4 1 6
```Ở đây có thể có tối đa hai công việc chạy đồng thời. Một chiến lược tham lam luôn chọn công việc không chồng chéo có lợi nhuận cao nhất sẽ thất bại vì sự chồng chéo được cho phép đến hết khả năng`k`. Vấn đề không phải là việc lập kế hoạch theo khoảng thời gian có trọng số thông thường. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử mọi tập hợp con công việc, xác minh xem có lúc nào nhiều hơn`k`công việc chồng chéo và giữ lợi nhuận tốt nhất. Tính đúng đắn là hiển nhiên vì mọi câu trả lời có thể đều được kiểm tra. 

Vấn đề là kích thước của không gian tìm kiếm. Với`n = 1000`, số tập con là`2^1000`, điều này là không thể về mặt thiên văn. Thậm chí chỉ kiểm tra`2^40`các tập hợp con sẽ vượt xa giới hạn. 

Một hướng mạnh mẽ hơn là lập trình động theo các khoảng thời gian. Vì`k = 1`, vấn đề trở thành việc lập kế hoạch theo khoảng thời gian có trọng số cổ điển. Chúng tôi sắp xếp công việc theo thời gian hoàn thành và sử dụng tìm kiếm nhị phân để kết nối từng khoảng thời gian với khoảng thời gian tương thích tiếp theo. Điều đó mang lại một`O(n log n)`giải pháp. 

Trở ngại đó chính là`k`có thể vượt quá`1`. Sau khi cho phép nhiều lần chồng lấp, trạng thái không còn chỉ phụ thuộc vào khoảng thời gian được chọn gần nhất. Các máy khác nhau hiện có thể được thực hiện các công việc khác nhau và kết thúc vào các thời điểm khác nhau. DP trực tiếp trên các trạng thái máy sẽ bùng nổ theo kiểu tổ hợp. 

Quan sát quan trọng là hạn chế về cơ bản là về năng lực theo thời gian. Nhiều nhất là vào bất kỳ lúc nào`k`công việc có thể đi qua điểm đó. Đây chính xác là loại mô hình dòng chảy tối đa chi phí tối thiểu về mặt cấu trúc một cách tự nhiên. 

Chúng tôi nén tất cả các tọa độ thời gian riêng biệt. Giữa các thời điểm liên tiếp, chúng ta tạo các cạnh dòng thời gian với dung lượng`k`. Việc gửi một đơn vị dòng biểu thị một máy đang di chuyển về phía trước theo thời gian. Chọn một công việc có nghĩa là chuyển hướng một đơn vị dòng chảy qua một cạnh đặc biệt nhảy từ thời điểm bắt đầu đến thời điểm kết thúc trong khi thu được lợi nhuận. 

Bởi vì lợi nhuận phải được tối đa hóa nên chúng ta chuyển chúng thành chi phí âm và tính toán dòng chi phí tối thiểu. Các cạnh dòng thời gian có chi phí`0`, trong khi mỗi cạnh công việc có chi phí`-c_i`. Đường dẫn luồng có thể không hoạt động theo thời gian hoặc sử dụng các cạnh công việc có lợi bất cứ khi nào có lợi. 

Điều này biến đổi ràng buộc lập kế hoạch thành bảo toàn luồng tiêu chuẩn và năng lực biên. Dung tích`k`trên các cạnh dòng thời gian đảm bảo không nhiều hơn`k`công việc đồng thời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n) | Quá chậm | 
| Luồng chi phí tối thiểu tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi công việc, hãy tính thời điểm kết thúc của nó như sau:`e = s + t`. 

sử dụng`s + t`thay vì`s + t - 1`chuyển đổi khoảng thời gian bị chiếm dụng thành phạm vi nửa mở`[s, e)`. Hai công việc tương thích nhau một cách chính xác khi công việc này bắt đầu cùng lúc hoặc sau công việc kia.`e`. 
2. Thu thập tất cả các tọa độ bắt đầu và kết thúc riêng biệt và sắp xếp chúng. 

Các giá trị số thực tế có thể lớn bằng`10^9`, nhưng chỉ có vấn đề về thứ tự tương đối. Nén tọa độ làm giảm kích thước đồ thị tối đa`2n`nút. 
3. Xây dựng đồ thị có hướng theo chỉ số thời gian đã nén. 

Nếu thời gian nén là`T[0], T[1], ..., T[m-1]`, tạo các cạnh:`i -> i+1`với năng lực`k`và chi phí`0`. 

Các cạnh này đại diện cho máy móc di chuyển theo thời gian mà không thực hiện công việc. 
4. Thêm nguồn kết nối tới nút lần đầu có dung lượng`k`và kết nối nút lần cuối với bồn rửa có dung lượng`k`. 

Điều này tạo ra chính xác`k`đơn vị lưu lượng máy có sẵn. 
5. Đối với mỗi công việc, hãy thêm một cạnh từ chỉ số bắt đầu được nén vào chỉ số cuối được nén của nó. 

Cạnh có: 

công suất`1`trị giá`-profit`Lấy cạnh này có nghĩa là một máy thực hiện công việc đó trong khoảng thời gian nó bị chiếm dụng. 
6. Chạy luồng tối đa chi phí tối thiểu từ nguồn đến đích. 

Quy trình tự nhiên quyết định lợi thế công việc nào đủ lợi nhuận để sử dụng. 
7. Khôi phục các công việc đã chọn. 

Mỗi khía cạnh công việc có năng lực trở thành`0`đã được luồng sử dụng đầy đủ nên công việc đó thuộc về lịch trình tối ưu. 

### Tại sao nó hoạt động 

Mỗi đơn vị dòng chảy đại diện cho một cỗ máy đang di chuyển về phía trước theo thời gian. Các cạnh của dòng thời gian duy trì giới hạn tối đa`k`máy đồng thời vì chỉ`k`các đơn vị có thể vượt qua bất kỳ phân đoạn thời gian nào. 

Một cạnh công việc chiếm một máy từ tọa độ đầu đến tọa độ cuối của nó. Vì cạnh đó bỏ qua các cạnh dòng thời gian trung gian nên máy không thể thực hiện đồng thời một công việc chồng chéo khác. 

Bảo toàn dòng chảy đảm bảo mọi máy móc đều tuân theo trình tự công việc theo trình tự thời gian hợp lệ. Tổng chi phí lưu chuyển bằng tổng lợi nhuận âm của các công việc đã chọn. Tối thiểu hóa chi phí tương đương với tối đa hóa lợi nhuận. 

Bởi vì tất cả năng lực và chi phí đều mã hóa chính xác các ràng buộc và mục tiêu lập kế hoạch, mọi luồng khả thi đều tương ứng với một lịch trình hợp lệ với lợi nhuận giống nhau và mọi lịch trình hợp lệ đều tương ứng với một luồng khả thi. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

INF = 10**18

class Edge:
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

    def min_cost_flow(self, s, t, maxf):
        n = self.n
        flow = 0
        cost = 0

        potential = [0] * n

        while flow < maxf:
            dist = [INF] * n
            parent_v = [-1] * n
            parent_e = [-1] * n

            dist[s] = 0

            inq = [False] * n
            q = deque([s])
            inq[s] = True

            while q:
                v = q.popleft()
                inq[v] = False

                for i, e in enumerate(self.g[v]):
                    if e.cap <= 0:
                        continue

                    nd = dist[v] + e.cost + potential[v] - potential[e.to]

                    if nd < dist[e.to]:
                        dist[e.to] = nd
                        parent_v[e.to] = v
                        parent_e[e.to] = i

                        if not inq[e.to]:
                            inq[e.to] = True
                            q.append(e.to)

            if dist[t] == INF:
                break

            for v in range(n):
                if dist[v] < INF:
                    potential[v] += dist[v]

            addf = maxf - flow
            v = t

            while v != s:
                pv = parent_v[v]
                pe = parent_e[v]
                addf = min(addf, self.g[pv][pe].cap)
                v = pv

            v = t

            while v != s:
                pv = parent_v[v]
                pe = parent_e[v]
                e = self.g[pv][pe]

                e.cap -= addf
                self.g[v][e.rev].cap += addf

                cost += addf * e.cost
                v = pv

            flow += addf

        return flow, cost

def solve():
    n, k = map(int, input().split())

    jobs = []
    coords = []

    for i in range(n):
        s, t, c = map(int, input().split())
        e = s + t

        jobs.append((s, e, c, i))

        coords.append(s)
        coords.append(e)

    coords = sorted(set(coords))
    idx = {x: i for i, x in enumerate(coords)}

    m = len(coords)

    SRC = m
    SNK = m + 1

    mcmf = MinCostFlow(m + 2)

    mcmf.add_edge(SRC, 0, k, 0)

    for i in range(m - 1):
        mcmf.add_edge(i, i + 1, k, 0)

    mcmf.add_edge(m - 1, SNK, k, 0)

    job_edges = [None] * n

    for s, e, c, original_idx in jobs:
        u = idx[s]
        v = idx[e]

        edge_index = len(mcmf.g[u])

        mcmf.add_edge(u, v, 1, -c)

        job_edges[original_idx] = (u, edge_index)

    mcmf.min_cost_flow(SRC, SNK, k)

    ans = [0] * n

    for i in range(n):
        u, ei = job_edges[i]
        e = mcmf.g[u][ei]

        if e.cap == 0:
            ans[i] = 1

    print(*ans)

if __name__ == "__main__":
    solve()
```Biểu đồ chứa một nút cho mỗi tọa độ thời gian nén. Các cạnh của dòng thời gian kết nối các tọa độ liên tiếp và mang tới`k`máy về phía trước. 

Chi tiết triển khai quan trọng nhất là chuyển đổi khoảng thời gian. Khoảng thời gian ban đầu bao gồm cả hai đầu,`[s, s+t-1]`. Chuyển đổi nó thành`[s, s+t)`tránh các vấn đề riêng lẻ và làm cho việc kiểm tra khả năng tương thích trở nên rõ ràng hơn nhiều. 

Mỗi cạnh công việc lưu trữ dung lượng`1`. Sau khi luồng kết thúc, cạnh được sử dụng có dung lượng còn lại`0`. Đó là cách các công việc đã chọn được xây dựng lại. 

Tính toán đường đi ngắn nhất sử dụng chi phí giảm với tiềm năng. Chi phí có thể âm vì các công việc sinh lợi có trọng số lợi nhuận âm. Tiềm năng đảm bảo chi phí giảm không âm sau mỗi lần nâng cấp. 

Một chi tiết tinh tế khác là chúng tôi gửi chính xác`k`đơn vị lưu lượng, ngay cả khi một số máy không hoạt động. Máy nhàn rỗi chỉ cần đi qua các cạnh dòng thời gian với chi phí bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
2 7 5
1 3 3
4 1 3
```Các khoảng trở thành: 

| Việc làm | Khoảng thời gian | Lợi nhuận | 
| --- | --- | --- | 
| 1 | [2, 9) | 5 | 
| 2 | [1, 4) | 3 | 
| 3 | [4, 5) | 3 | 

Tọa độ nén: 

| Chỉ mục | Thời gian | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 4 | 
| 3 | 5 | 
| 4 | 9 | 

Cạnh công việc: 

| Việc làm | Cạnh | 
| --- | --- | 
| 1 | 1 -> 4 | 
| 2 | 0 -> 2 | 
| 3 | 2 -> 3 | 

Đường đi tối ưu của dòng chảy là: 

| Bước | Cạnh được sử dụng | Tổng lợi nhuận | 
| --- | --- | --- | 
| 1 | Công việc 2 | 3 | 
| 2 | Công việc 3 | 6 | 

Chỉ chọn việc 1 thôi đã có lãi`5`, do đó, quy trình ưu tiên Công việc 2 và 3. 

Dấu vết này cho thấy các công việc tương thích tuần tự được xâu chuỗi một cách tự nhiên như thế nào thông qua các biên của dòng thời gian. 

### Ví dụ 2 

đầu vào:```
4 2
1 5 10
2 3 8
3 2 7
4 1 6
```Khoảng thời gian được chuyển đổi: 

| Việc làm | Khoảng thời gian | Lợi nhuận | 
| --- | --- | --- | 
| 1 | [1, 6) | 10 | 
| 2 | [2, 5) | 8 | 
| 3 | [3, 5) | 7 | 
| 4 | [4, 5) | 6 | 

Ở khoảng thời gian`[4,5)`, bốn công việc trùng nhau nhưng chỉ tồn tại hai máy. 

Luồng chọn: 

| Máy | Việc làm | 
| --- | --- | 
| 1 | Công việc 1 | 
| 2 | Công việc 2 | 

Tổng lợi nhuận trở thành`18`. 

Dấu vết cho thấy năng lực như thế nào`k=2`được thực thi tự động theo năng lực cạnh dòng thời gian. Không quá hai đơn vị dòng chảy có thể đi qua cùng một đoạn thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Biểu đồ có các nút O(n) và các cạnh O(n) và luồng chi phí tối thiểu thực hiện tối đa k lần tăng với các đường đi ngắn nhất | 
| Không gian | O(n^2) | Lưu trữ đồ thị dư | 

Với`n <= 1000`, điều này thoải mái phù hợp trong giới hạn. Đồ thị chỉ chứa vài nghìn cạnh và`k <= 50`, do đó số lần tăng thêm là nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from collections import deque

    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    input = input_data.readline

    INF = 10**18

    class Edge:
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

        def min_cost_flow(self, s, t, maxf):
            n = self.n
            flow = 0
            potential = [0] * n

            while flow < maxf:
                dist = [INF] * n
                pv = [-1] * n
                pe = [-1] * n

                dist[s] = 0

                q = deque([s])
                inq = [False] * n
                inq[s] = True

                while q:
                    v = q.popleft()
                    inq[v] = False

                    for i, e in enumerate(self.g[v]):
                        if e.cap <= 0:
                            continue

                        nd = dist[v] + e.cost + potential[v] - potential[e.to]

                        if nd < dist[e.to]:
                            dist[e.to] = nd
                            pv[e.to] = v
                            pe[e.to] = i

                            if not inq[e.to]:
                                inq[e.to] = True
                                q.append(e.to)

                if dist[t] == INF:
                    break

                for i in range(n):
                    if dist[i] < INF:
                        potential[i] += dist[i]

                addf = maxf - flow
                v = t

                while v != s:
                    addf = min(addf, self.g[pv[v]][pe[v]].cap)
                    v = pv[v]

                v = t

                while v != s:
                    e = self.g[pv[v]][pe[v]]
                    e.cap -= addf
                    self.g[v][e.rev].cap += addf
                    v = pv[v]

                flow += addf

    n, k = map(int, input().split())

    jobs = []
    coords = []

    for i in range(n):
        s, t, c = map(int, input().split())
        e = s + t
        jobs.append((s, e, c, i))
        coords.extend([s, e])

    coords = sorted(set(coords))
    idx = {x: i for i, x in enumerate(coords)}

    m = len(coords)

    SRC = m
    SNK = m + 1

    mcf = MinCostFlow(m + 2)

    mcf.add_edge(SRC, 0, k, 0)

    for i in range(m - 1):
        mcf.add_edge(i, i + 1, k, 0)

    mcf.add_edge(m - 1, SNK, k, 0)

    pos = [None] * n

    for s, e, c, i in jobs:
        u = idx[s]
        v = idx[e]

        edge_id = len(mcf.g[u])

        mcf.add_edge(u, v, 1, -c)

        pos[i] = (u, edge_id)

    mcf.min_cost_flow(SRC, SNK, k)

    ans = []

    for i in range(n):
        u, ei = pos[i]
        ans.append("1" if mcf.g[u][ei].cap == 0 else "0")

    return " ".join(ans)

# provided sample
assert run(
"""3 1
2 7 5
1 3 3
4 1 3
"""
) == "0 1 1"

# minimum size
assert run(
"""1 1
1 1 10
"""
) == "1"

# overlapping jobs, only one machine
assert run(
"""2 1
1 5 10
2 3 9
"""
) == "1 0"

# touching intervals should both work
assert run(
"""2 1
1 2 5
3 1 6
"""
) == "1 1"

# two machines allow overlap
assert run(
"""3 2
1 5 10
2 3 9
3 2 8
"""
) == "1 1 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Công việc đơn lẻ |`1`| Ranh giới kích thước tối thiểu | 
| Hai công việc chồng chéo với k=1 |`1 0`| Hạn chế công suất | 
| Khoảng chạm |`1 1`| Xử lý khoảng thời gian nửa mở đúng | 
| Chồng chéo nặng với k=2 |`1 1 0`| Nhiều máy đồng thời | 

## Vỏ cạnh 

Hãy xem xét các công việc có khoảng thời gian chỉ chạm vào điểm cuối.```
2 1
1 2 5
3 1 6
```Các khoảng trở thành`[1,3)`Và`[3,4)`. Các cạnh nén của chúng kết nối liên tiếp mà không chồng lên nhau. Một đơn vị luồng có thể đi qua cả hai cạnh công việc theo trình tự, do đó cả hai công việc đều được chọn. 

Bây giờ hãy xem xét thời gian bắt đầu giống hệt nhau.```
3 1
1 5 10
1 1 4
2 1 4
```Công việc đầu tiên chiếm`[1,6)`. Hai người còn lại chiếm`[1,2)`Và`[2,3)`. Vì chỉ tồn tại một máy nên luồng không thể sử dụng đồng thời cạnh khoảng lớn và chuỗi khoảng nhỏ hơn. Chuỗi nhỏ hơn mang lại lợi nhuận`8`, trong khi công việc lâu dài mang lại lợi nhuận`10`, do đó thuật toán chỉ chọn chính xác công việc đầu tiên. 

Cuối cùng, hãy xem xét nhiều máy.```
4 2
1 5 10
2 3 8
3 2 7
4 1 6
```Cả bốn công việc đều chồng chéo về thời gian`4`, nhưng các cạnh dòng thời gian có khả năng`2`. Nhiều nhất hai đơn vị luồng có thể đi qua phân đoạn đó, do đó, biểu đồ dư về mặt vật lý ngăn chặn việc chọn nhiều hơn hai công việc chồng chéo. Tiến độ kết quả luôn khả thi bằng cách xây dựng.
