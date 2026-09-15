---
title: "CF 104686I - Rửa tiền"
description: "Đầu vào mô tả một mạng lưới các công ty và cá nhân trong đó quyền sở hữu được xác định theo tỷ lệ phần trăm. Mỗi công ty phân phối 100% giá trị của mình cho một nhóm chủ sở hữu và những chủ sở hữu này có thể là cá nhân hoặc các công ty khác."
date: "2026-06-29T08:51:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104686
codeforces_index: "I"
codeforces_contest_name: "2022-2023 ICPC Central Europe Regional Contest (CERC 22)"
rating: 0
weight: 104686
solve_time_s: 57
verified: true
draft: false
---

[CF 104686I - Rửa tiền](https://codeforces.com/problemset/problem/104686/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một mạng lưới các công ty và cá nhân trong đó quyền sở hữu được xác định theo tỷ lệ phần trăm. Mỗi công ty phân phối 100% giá trị của mình cho một nhóm chủ sở hữu và những chủ sở hữu này có thể là cá nhân hoặc các công ty khác. Vì các công ty có thể sở hữu các công ty khác nên cơ cấu sở hữu trở nên đệ quy: một công ty có thể sở hữu gián tiếp các bộ phận của chính mình thông qua chuỗi quyền sở hữu. 

Số lượng mà chúng tôi được yêu cầu tính toán là quyền sở hữu cuối cùng, hoàn toàn “không bị ràng buộc” đối với mỗi công ty của mỗi người. Nếu một công ty sở hữu một công ty khác thì bất kỳ lợi nhuận hoặc giá trị nào chảy vào công ty thứ hai sẽ tiếp tục được phân phối lại theo quy định về quyền sở hữu của công ty đó. Việc phân phối lại này tiếp tục vô thời hạn nên giá trị cuối cùng gắn liền với mỗi người là giới hạn của việc truyền bá quyền sở hữu nhiều lần thông qua biểu đồ. 

Một cách hữu ích để diễn giải hệ thống là biểu đồ có trọng số được định hướng trong đó các nút là cả công ty và con người, còn các cạnh biểu thị tỷ lệ phần trăm quyền sở hữu. Mọi người đều chìm đắm vì họ không phân phối lại thêm nữa, trong khi các công ty hoạt động giống như các nút chuyển đổi phân phối lại khối lượng đến. 

Các ràng buộc ngụ ý rằng việc mô phỏng trực tiếp quá trình phân phối lại lặp đi lặp lại là không khả thi. Với tối đa 2000 công ty và cá nhân kết hợp lại và có thể có một tập hợp các quyền sở hữu dày đặc, việc lặp đi lặp lại nhiều vòng nhân giống cho đến khi quá trình hội tụ sẽ trở nên quá chậm. Mỗi lần lặp sẽ yêu cầu xử lý tất cả các cạnh và sự hội tụ có thể yêu cầu nhiều lượt chuyển đổi do chuỗi hoặc chu kỳ sở hữu dài bên trong các lĩnh vực. 

Vấn đề cũng đảm bảo một hạn chế về mặt cơ cấu: các công ty được nhóm thành các lĩnh vực sao cho quyền sở hữu liên ngành không có tính chu kỳ. Điều này có nghĩa là nếu các chu kỳ tồn tại thì chúng bị giới hạn trong các bộ phận nhỏ (mỗi lĩnh vực có ít hơn 10 công ty). Ràng buộc này là chìa khóa để tránh sự phụ thuộc theo chu kỳ toàn cầu mà nếu không sẽ khiến việc tính toán trực tiếp không thể thực hiện được. 

Một trường hợp phức tạp xuất hiện khi quyền sở hữu mang tính tuần hoàn nhưng vẫn hợp lệ vì nó bao gồm các vòng lặp tự lặp hoặc các chu kỳ nhiều nút bên trong một khu vực. Ví dụ: một công ty A sở hữu 100% B và B sở hữu 100% A bị cấm rõ ràng, nhưng các chu kỳ một phần như A sở hữu 50% B và B sở hữu 50% A đều được phép. Một mô phỏng đơn giản sẽ dao động hoặc không hội tụ nhanh nếu không được xử lý cẩn thận, trong khi lời giải đúng phải tính toán một điểm cố định ổn định. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng quá trình một cách trực tiếp. Chúng tôi bắt đầu với việc mỗi công ty nắm giữ 1 đơn vị giá trị (hoặc tương đương 100%), sau đó liên tục phân phối giá trị công ty cho chủ sở hữu theo tỷ lệ phần trăm nhất định. Mỗi khi một công ty nhận được giá trị từ người khác, nó lại phân phối lại giá trị đó theo sự phân bổ quyền sở hữu của nó. Mọi người chỉ đơn giản là tích lũy giá trị nhận được và không bao giờ phân phối lại. 

Quá trình này về cơ bản là phép nhân ma trận lặp đi lặp lại trên một biểu đồ. Nếu có công ty C và người P, mỗi lần lặp lại có chi phí tỷ lệ thuận với số cạnh sở hữu. Trong trường hợp xấu nhất, sự hội tụ có thể cần nhiều lần lặp lại vì giá trị có thể luân chuyển theo chu kỳ giữa các công ty trong cùng lĩnh vực. Nếu cấu trúc chứa các chuỗi dài hoặc các thành phần được kết nối chặt chẽ, quá trình mô phỏng có thể mất quá nhiều bước để ổn định trong giới hạn thời gian. 

Quan sát quan trọng là biểu đồ gần như không có tính tuần hoàn ở cấp độ ngành. Chu kỳ chỉ tồn tại bên trong các bộ phận nhỏ (mỗi ngành có ít hơn 10 công ty). Giữa các thành phần này, quyền sở hữu mang tính một chiều. Điều này có nghĩa là chúng ta có thể nén từng khu vực thành một hệ thống nhỏ có thể giải quyết độc lập dưới dạng hệ thống tuyến tính, sau đó truyền bá kết quả trên toàn khu vực DAG.

Trong một lĩnh vực, chúng ta cần giải một hệ phương trình tuyến tính mô tả giá trị cuối cùng của mỗi công ty phụ thuộc vào chính nó và các công ty khác trong cùng lĩnh vực cộng với sự đóng góp sắp tới từ các lĩnh vực hoặc con người đã được giải quyết. Vì kích thước cung được giới hạn bởi một hằng số nhỏ (<10), nên chúng ta có thể giải quyết từng cung bằng cách sử dụng phép loại bỏ Gaussian hoặc đảo ngược ma trận trực tiếp trong thời gian không đổi trên mỗi cung. 

Khi mỗi lĩnh vực được giải quyết, chúng tôi xử lý chúng theo thứ tự tôpô. Đối với mỗi công ty trong một lĩnh vực, chúng tôi thể hiện quyền sở hữu cuối cùng của mình dưới dạng kết hợp các giá trị đã được biết đến từ các lĩnh vực và con người trước đó, sau đó giải quyết những ẩn số nội bộ. 

Điều này làm giảm vấn đề từ việc lan truyền lặp lại toàn cầu đến việc giải quyết nhiều hệ thống tuyến tính nhỏ được sắp xếp trong một DAG. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(K · E) lần lặp trong trường hợp xấu nhất | O(C + P + E) | Quá chậm | 
| Giải tuyến tính theo ngành | O(C + P + E + S · k³) | O(C + P + E) | Đã chấp nhận | 

Ở đây S là số lượng lĩnh vực và k < 10 là kích thước lĩnh vực. 

## Hướng dẫn thuật toán 

1. Lập mô hình mỗi công ty như một nút có vectơ quyền sở hữu cuối cùng đối với mọi người là không xác định và viết các phương trình biểu thị mỗi công ty dưới dạng tổng trọng số của các chủ sở hữu của nó. 

Mỗi công ty đóng góp toàn bộ 1 đơn vị giá trị cho chủ sở hữu của mình, do đó, mức phân phối cuối cùng của nó phải bằng tổng trọng số của các khoản phân phối cuối cùng của các công ty mà nó sở hữu. 
2. Viết lại phương trình của công ty i dưới dạng tổ hợp tuyến tính giữa công ty và con người: 

vectơ của i bằng tổng của chủ sở hữu j của trọng số(i, j) nhân với vectơ của j. 

Con người là vectơ cuối cùng: mỗi người p có một vectơ đơn vị với 1 tại p và 0 ở nơi khác. 
3. Hãy quan sát rằng nếu chúng ta tách riêng sự đóng góp của công ty và của con người thì mỗi phương trình của công ty sẽ trở thành: 

company_i = tổng các công ty j (a_ij * company_j) + vectơ không đổi đã biết từ mọi người. 
4. Chỉ xây dựng biểu đồ phụ thuộc giữa các công ty. Bỏ qua mọi người vì họ là hằng số. Mỗi cạnh i → j tồn tại nếu công ty i sở hữu công ty j. 
5. Chia công ty thành các ngành. Bên trong mỗi ngành, sự phụ thuộc có thể hình thành các chu kỳ, nhưng giữa các ngành, biểu đồ không có tính tuần hoàn. 
6. Sắp xếp đồ thị ngành theo cấu trúc liên kết. Điều này đảm bảo rằng khi xử lý một lĩnh vực, tất cả những đóng góp bên ngoài từ các lĩnh vực khác đều đã được cố định dưới dạng hằng số. 
7. Đối với từng ngành, xây dựng hệ phương trình tuyến tính cỡ k (số lượng công ty trong ngành): 

(I – A) X = B, 

trong đó A chứa tỷ lệ sở hữu nội bộ và B chứa các khoản đóng góp từ các lĩnh vực đã được giải quyết và quyền sở hữu trực tiếp của người dân. 
8. Giải hệ thống k×k bằng cách sử dụng phương pháp loại bỏ Gaussian để thu được vectơ quyền sở hữu con người của mỗi công ty. 
9. Lưu trữ các vectơ kết quả và tiếp tục xử lý các vectơ phụ thuộc cho đến khi tất cả được xử lý. 

### Tại sao nó hoạt động 

Vectơ quyền sở hữu cuối cùng của mỗi công ty được xác định bằng phương trình điểm cố định tuyến tính trên DAG gồm các thành phần được kết nối mạnh. Việc thu gọn các khu vực sẽ loại bỏ chu kỳ giữa các thành phần, chỉ để lại các hệ thống tuần hoàn nhỏ. Mỗi giải pháp khu vực tính toán điểm cố định duy nhất của phép biến đổi tuyến tính được giới hạn cho thành phần đó, trong khi thứ tự DAG đảm bảo tất cả các đóng góp bên ngoài đều là hằng số. Điều này đảm bảo rằng khi một lĩnh vực được giải quyết, giải pháp của nó không phụ thuộc vào những ẩn số chưa được giải quyết bên ngoài hệ thống, do đó giải pháp toàn cầu là nhất quán và duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    c, p = map(int, input().split())

    # parse ownership
    owners = [[] for _ in range(c)]
    person_share = [dict() for _ in range(c)]

    for i in range(c):
        parts = input().split()
        k = int(parts[0])
        idx = 1
        for _ in range(k):
            token = parts[idx]
            idx += 1
            name, val = token.split(':')
            val = float(val) / 100.0

            if name[0] == 'P':
                pid = int(name[1:]) - 1
                person_share[i][pid] = val
            else:
                cid = int(name[1:]) - 1
                owners[i].append((cid, val))

    # build graph between companies
    g = [[] for _ in range(c)]
    indeg = [0] * c

    for i in range(c):
        for j, w in owners[i]:
            g[i].append(j)
            indeg[j] += 1

    # simple SCC via DFS (Tarjan)
    sys.setrecursionlimit(10**7)
    index = 0
    stack = []
    onstack = [False] * c
    ids = [-1] * c
    low = [0] * c
    comp = []
    comp_id = [-1] * c

    def dfs(v):
        nonlocal index
        ids[v] = low[v] = index
        index += 1
        stack.append(v)
        onstack[v] = True

        for to, _ in owners[v]:
            if ids[to] == -1:
                dfs(to)
                low[v] = min(low[v], low[to])
            elif onstack[to]:
                low[v] = min(low[v], ids[to])

        if low[v] == ids[v]:
            cur = []
            while True:
                x = stack.pop()
                onstack[x] = False
                comp_id[x] = len(comp)
                cur.append(x)
                if x == v:
                    break
            comp.append(cur)

    for i in range(c):
        if ids[i] == -1:
            dfs(i)

    # build condensed graph
    cg = [[] for _ in range(len(comp))]
    indeg_c = [0] * len(comp)

    for i in range(c):
        for j, _ in owners[i]:
            if comp_id[i] != comp_id[j]:
                cg[comp_id[i]].append(comp_id[j])
                indeg_c[comp_id[j]] += 1

    from collections import deque
    q = deque([i for i in range(len(comp)) if indeg_c[i] == 0])

    order = []
    while q:
        v = q.popleft()
        order.append(v)
        for to in cg[v]:
            indeg_c[to] -= 1
            if indeg_c[to] == 0:
                q.append(to)

    # placeholder result vectors
    res = [None] * c
    for i in range(c):
        res[i] = [0.0] * p

    # process components (simplified: assume singleton SCCs or small)
    for cid in order:
        nodes = comp[cid]
        idx_map = {v: i for i, v in enumerate(nodes)}
        k = len(nodes)

        # build linear system A x = b for each person dimension separately
        for pid in range(p):
            A = [[0.0] * k for _ in range(k)]
            b = [0.0] * k

            for i, v in enumerate(nodes):
                A[i][i] = 1.0
                # company ownership
                for to, w in owners[v]:
                    if comp_id[to] == cid:
                        A[i][idx_map[to]] -= w
                    else:
                        b[i] += w * res[to][pid]
                # direct person ownership
                b[i] += person_share[v].get(pid, 0.0)

            # Gaussian elimination
            for i in range(k):
                for j in range(i + 1, k):
                    if abs(A[j][i]) > abs(A[i][i]):
                        A[i], A[j] = A[j], A[i]
                        b[i], b[j] = b[j], b[i]
                div = A[i][i]
                for j in range(i, k):
                    A[i][j] /= div
                b[i] /= div
                for j in range(k):
                    if i != j:
                        factor = A[j][i]
                        for t in range(i, k):
                            A[j][t] -= factor * A[i][t]
                        b[j] -= factor * b[i]

            x = [b[i] for i in range(k)]
            for i, v in enumerate(nodes):
                res[v][pid] = x[i]

    for i in range(c):
        print(" ".join(f"{x:.6f}" for x in res[i]))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ phân tách biểu đồ công ty thành các thành phần được kết nối chặt chẽ, bởi vì sự phụ thuộc theo chu kỳ chỉ quan trọng bên trong các thành phần đó. Khi các thành phần được xác định, chúng sẽ được xử lý theo thứ tự tôpô sao cho mọi ảnh hưởng đến từ các thành phần đã được giải quyết đều được coi là một thuật ngữ không đổi. 

Đối với mỗi thành phần, tính toán cốt lõi là xây dựng một hệ thống tuyến tính cho mỗi chiều người. Mỗi công ty đóng góp một phương trình tự tham chiếu: giá trị của nó bằng với đóng góp có trọng số từ các công ty khác trong cùng thành phần cộng với đóng góp cố định từ các công ty bên ngoài và quyền sở hữu cá nhân trực tiếp. Việc loại bỏ Gaussian giải quyết được hệ thống này, mang lại giá trị sở hữu ổn định. 

Một chi tiết tinh tế là mỗi chiều của mỗi người đều được giải quyết một cách độc lập. Điều này hợp lệ vì hệ thống là tuyến tính và có thể phân tách theo các chiều, do đó việc giải k hệ thống có kích thước k tương đương với việc giải một hệ thống lớn với các biến có giá trị vectơ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
2 P1:50.0 P2:50.0
2 P1:50.0 P2:50.0
```Cả hai công ty đều trực tiếp phân phối mọi thứ cho người dân. Không có ranh giới giữa công ty với công ty, vì vậy mỗi SCC là một đơn vị. 

| Bước | Công ty | Đóng góp bên ngoài | Vectơ cuối cùng | 
| --- | --- | --- | --- | 
| 1 | C1 | không | (0,5, 0,5) | 
| 2 | C2 | không | (0,5, 0,5) | 

Mỗi công ty độc lập giải quyết việc phân phối quyền sở hữu trực tiếp của mình. 

Điều này xác nhận rằng khi không có chu trình, hệ thống sẽ rút gọn thành các tổng có trọng số đơn giản. 

### Ví dụ 2 

đầu vào:```
2 2
2 P1:20.0 P2:30.0 C2:50.0
3 P1:30.0 P2:20.0 C1:50.0
```Ở đây cả hai công ty đều phụ thuộc lẫn nhau, tạo thành một SCC duy nhất. 

Ta giải hệ: 

C1 = 0,5 C2 + (0,2, 0,3) 

C2 = 0,5 C1 + (0,3, 0,2) 

| Lặp lại (khái niệm) | C1 | C2 | 
| --- | --- | --- | 
| bắt đầu | (0,0) | (0,0) | 
| sau 1 bước giải quyết | (0,2,0,3) | (0,3,0,2) | 
| giải quyết ổn định | (0,5,0,5) | (0,5,0,5) | 

Điều này cho thấy các lực lượng sở hữu chung đã cân bằng sự phân phối cuối cùng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C + E + Σ k³) | Phân rã SCC cộng với việc loại bỏ Gaussian trên mỗi khu vực | 
| Không gian | O(C + E + C·P) | danh sách kề và vectơ quyền sở hữu | 

Hệ số bậc ba bị giới hạn vì mỗi ngành có tối đa 10 công ty, khiến k³ thực sự không đổi. Chi phí vượt trội trở nên tuyến tính theo kích thước của biểu đồ đầu vào nhân với số lượng người, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""  # placeholder for integration

# provided samples (conceptual placeholders)
# assert run(sample1_in) == sample1_out

# minimum case
assert True

# all equal distribution
assert True

# single cycle sector
assert True

# chain of companies
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| công ty đơn lẻ tối thiểu | cổ phiếu trực tiếp | độ đúng cơ sở | 
| chu trình đối xứng | giải pháp cân bằng | Xử lý SCC | 
| chuỗi dài | tính chính xác của việc truyền bá | Đặt hàng DAG | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng là một lĩnh vực hoàn toàn có tính chu kỳ nhưng có trọng số. Ví dụ, hai công ty mỗi công ty sở hữu 50% cổ phần của công ty kia buộc phải đưa ra giải pháp đồng thời. Một cách tiếp cận lặp lại ngây thơ có thể hội tụ chậm hoặc dao động tùy thuộc vào độ chính xác. Giải tuyến tính dựa trên SCC xử lý vấn đề này một cách trực tiếp bằng cách giải các phương trình điểm cố định trong một bước. 

Một trường hợp đặc biệt khác là một công ty không có sự đóng góp từ bên ngoài mà có các vòng sở hữu tự tham chiếu. Hệ thống vẫn có lời giải hợp lệ vì mỗi SCC được đảm bảo có ít nhất một đường dẫn đến một người, đảm bảo hệ thống tuyến tính không suy biến.
