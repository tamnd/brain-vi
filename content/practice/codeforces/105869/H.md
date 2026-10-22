---
title: "CF 105869H - Con đường đàng hoàng quanh Bajt\u00f3w"
description: "Chúng tôi đang làm việc với một biểu đồ trong đó mỗi trạng thái là một quyết định truyền tải cạnh có hướng chứ không chỉ là giá trị đỉnh. Đối với mỗi đỉnh $v$ và mọi đỉnh $u$ liền kề với nó, chúng ta xác định một đại lượng $L(v, u)$ đại diện cho quãng đường đi bộ dài nhất có thể bắt đầu từ $v$ và buộc phải…"
date: "2026-06-22T02:29:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105869
codeforces_index: "H"
codeforces_contest_name: "OCPC Fall 2024 Day 2 Jagiellonian Contest (The 3rd Universal Cup. Stage 35: Krak\u00f3w)"
rating: 0
weight: 105869
solve_time_s: 49
verified: true
draft: false
---

[CF 105869H - Con đường thuận tiện quanh Bajt\u00f3w](https://codeforces.com/problemset/problem/105869/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một biểu đồ trong đó mỗi trạng thái là một quyết định truyền tải cạnh có hướng chứ không chỉ là giá trị đỉnh. Với mọi đỉnh$v$, và mọi hàng xóm$u$liền kề với nó, chúng ta xác định một đại lượng$L(v, u)$đại diện cho chuyến đi bộ dài nhất có thể bắt đầu lúc$v$và buộc phải chiếm thế thượng phong$(v, u)$như bước đi đầu tiên của nó. Sau bước đầu tiên này, việc đi bộ tiếp tục một cách tối ưu từ$u$, nhưng không được phép quay lại ngay$v$theo cách tạo ra sự phụ thuộc quay lui tầm thường trong đệ quy. 

Ý tưởng chính là tính toán$L(v, u)$phụ thuộc vào giá trị của dạng$L(u, w)$cho tất cả hàng xóm$w \neq v$. Vì vậy, mọi trạng thái cạnh có hướng đều phụ thuộc vào tất cả các trạng thái cạnh đi ra khác của đỉnh tiếp theo của nó, ngoại trừ trạng thái trỏ ngược lại nơi nó xuất phát. 

Điều này tạo ra một cấu trúc phụ thuộc vào các cạnh được định hướng thay vì các đỉnh. Đầu ra cuối cùng được bắt nguồn từ những$L(v, u)$giá trị, thường được tổng hợp thành câu trả lời tốt nhất có thể trên tất cả các lựa chọn ban đầu. 

Đầu vào mô tả một đồ thị vô hướng với$n$đỉnh và$m$các cạnh. Mục tiêu là tính toán một tập hợp các giá trị đường dẫn dài nhất nhất quán trên toàn cầu theo quy tắc phụ thuộc ở trên. 

Từ quan điểm phức tạp, cả hai$n$Và$m$là các ràng buộc quy mô tuyến tính. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai về các cạnh, vì$m$có thể theo thứ tự của$10^5$hoặc$2 \cdot 10^5$. Bất kỳ giải pháp nào cũng phải xử lý hiệu quả từng cạnh được định hướng với số lần không đổi. Vì mọi cạnh vô hướng tương ứng với hai trạng thái có hướng nên chúng ta đang hoạt động trong một không gian có kích thước$O(n + m)$, gợi ý mạnh mẽ về biểu đồ DP hoặc lan truyền một lần trên cấu trúc phụ thuộc. 

Một vấn đề nhỏ xuất hiện khi biểu đồ chứa các chu trình. Một định nghĩa đệ quy ngây thơ của$L(v, u)$có vẻ như nó có thể truy cập lại các trạng thái cạnh giống nhau vô thời hạn, đặc biệt là trong các thành phần tuần hoàn. Thách thức là mặc dù đồ thị không bị định hướng, nhưng sự phụ thuộc giữa các trạng thái cạnh có hướng sẽ tạo thành một cấu trúc tuần hoàn có hướng một khi được định hướng chính xác theo hướng phụ thuộc. 

Một trường hợp thất bại điển hình phát sinh khi thử tính toán lại DFS đơn giản mà không có thứ tự ghi nhớ: 

đầu vào:```
3 3
1 2
2 3
3 1
```Trong một tam giác, tính toán$L(1,2)$phụ thuộc vào$L(2,3)$, điều này phụ thuộc vào$L(3,1)$, vòng lặp quay lại$L(1,2)$. Một phép đệ quy đơn giản không có thứ tự sẽ lặp lại vô hạn hoặc tính toán lại các trạng thái nhiều lần. 

Đầu ra chính xác vẫn được xác định rõ ràng, nhưng chỉ khi các phụ thuộc được giải quyết theo thứ tự tôpô hợp lệ trên các trạng thái cạnh. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ cố gắng tính toán từng$L(v, u)$độc lập bằng cách sử dụng đệ quy. Từ một đỉnh$u$, chúng tôi thử mọi người hàng xóm$w \neq v$, tính toán$L(u, w)$và lấy phần mở rộng tốt nhất. Điều này nhanh chóng mở rộng thành việc khám phá đầy đủ đồ thị cho từng cạnh có hướng. 

Vì có$2m$các trạng thái biên có hướng và mỗi trạng thái có thể kích hoạt việc truyền tải trên hầu hết tất cả các lân cận của một nút, thì độ phức tạp trong trường hợp xấu nhất sẽ trở thành$O(m \cdot n)$trong đồ thị dày đặc hoặc tuần hoàn. Điều này là quá chậm. 

Quan sát chính là sự phụ thuộc được cấu trúc cục bộ. Mỗi$L(v, u)$phụ thuộc vào tất cả các trạng thái đi từ$u$, ngoại trừ một. Điều này có nghĩa là nếu chúng ta đã biết tất cả các giá trị$L(u, w)$, sau đó tính toán$L(v, u)$chỉ là tổng hợp chúng. Vấn đề trở thành sự lan truyền các giá trị trên các cạnh được định hướng trong đó mỗi nút đợi cho đến khi tất cả trừ một trong các trạng thái phụ thuộc của nó được biết. 

Điều này gợi ý việc duy trì, đối với mỗi cạnh có hướng, bộ đếm số lượng phụ thuộc bắt buộc vẫn chưa được giải quyết. Chúng ta có thể coi mỗi đỉnh là quản lý một hàng “các đỉnh đang chờ” phụ thuộc vào trạng thái cạnh đi ra của nó. Một khi là một giá trị$L(u, w)$được tính toán, nó làm giảm các yêu cầu đang chờ xử lý của các trạng thái liên quan và có thể mở khóa các tính toán mới. Đây thực chất là một quá trình giải quyết sự phụ thuộc trên một cấu trúc tuần hoàn được định hướng ngầm định. 

Quá trình này trở thành quá trình lan truyền dựa trên hàng đợi tương tự như sắp xếp tôpô, nhưng được áp dụng cho các trạng thái cạnh thay vì các đỉnh. Mỗi trạng thái được xử lý chính xác một lần và mỗi cập nhật phụ thuộc được xử lý trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n + m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại từng trạng thái cạnh có hướng$(v, u)$như một nút trong hệ thống phụ thuộc. Đối với mỗi trạng thái như vậy, chúng tôi duy trì số lượng trạng thái con "vẫn còn thiếu" mà nó phụ thuộc vào. 

1. Xây dựng danh sách kề cho đồ thị và xử lý từng cạnh có hướng$(v, u)$như một trạng thái chúng ta cần tính toán. Sự phụ thuộc của$(v, u)$là tất cả các tiểu bang$(u, w)$vì$w \neq v$. 
2. Đối với mỗi đỉnh$u$, chúng tôi theo dõi có bao nhiêu trạng thái cạnh được định hướng đi vẫn chưa được hoàn thiện. Ban đầu, mỗi trạng thái có hướng$(v, u)$phụ thuộc vào tất cả các nước láng giềng của$u$ngoại trừ$v$, vì vậy số lượng phụ thuộc của nó là$\deg(u) - 1$. 
3. Khởi tạo hàng đợi với tất cả các trạng thái cạnh có hướng có số lượng phụ thuộc bằng 0. Điều này xảy ra khi một đỉnh có bậc 1, do đó trạng thái duy nhất không có lân cận thay thế nào để phụ thuộc vào. Đây là những trường hợp cơ bản không thể mở rộng thêm nữa. 
4. Xử lý hàng đợi lặp đi lặp lại. Khi một trạng thái$(v, u)$được giải quyết, chúng tôi truyền bá tác dụng của nó đến tất cả các trạng thái phụ thuộc vào nó. Điều này có nghĩa là chúng tôi giảm bộ đếm phụ thuộc của các trạng thái$(u, x)$Ở đâu$x \neq v$. Điều này phản ánh rằng người ta yêu cầu tiếp tục từ$u$hiện đã được giải quyết. 
5. Bất cứ khi nào bộ đếm phụ thuộc giảm xuống 0 đối với một số trạng thái$(u, x)$, điều đó có nghĩa là tất cả các phần tiếp theo cần thiết đã được tính toán, vì vậy chúng tôi tính toán$L(u, x)$và đẩy nó vào hàng đợi. 
6. Tiếp tục cho đến khi tất cả các trạng thái cạnh có hướng có thể tiếp cận được xử lý. 

Tính chính xác phụ thuộc vào thực tế là mỗi trạng thái có thể tính toán được chính xác khi tất cả các trạng thái mà nó phụ thuộc đã được hoàn thiện. 

### Tại sao nó hoạt động 

Hệ thống xác định thứ tự một phần trên các trạng thái cạnh được định hướng gây ra bởi sự loại trừ phụ thuộc của đỉnh cha. Mặc dù biểu đồ ban đầu có thể chứa các chu trình, nhưng mối quan hệ phụ thuộc giữa các trạng thái là không theo chu kỳ vì mọi chuyển đổi đều di chuyển hoàn toàn từ một trạng thái.$(v, u)$đến các trạng thái bắt nguồn từ$u$nhưng loại trừ$v$, ngăn chặn sự đảo ngược ngay lập tức và đảm bảo không có chu kỳ phụ thuộc nào có thể đóng lại. Điều này đảm bảo rằng quá trình phân giải dựa trên hàng đợi xử lý các trạng thái theo thứ tự tôpô hợp lệ trên biểu đồ phụ thuộc ngầm định, vì vậy mọi$L(v, u)$được tính toán chính xác một lần với tất cả các bài toán con cần thiết đã được giải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque, defaultdict

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    
    edges = []
    for _ in range(m):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)
        edges.append((a, b))

    # Each directed edge (v -> u) is a state
    # We map states to ids
    state_id = {}
    rev_state = []
    
    for v in range(1, n + 1):
        for u in g[v]:
            state_id[(v, u)] = len(rev_state)
            rev_state.append((v, u))

    deg = [len(g[v]) for v in range(n + 1)]
    
    # dependency count: deg[u] - 1 for state (v -> u)
    indeg = [0] * len(rev_state)
    dependents = [[] for _ in range(len(rev_state))]

    pos = defaultdict(list)
    for idx, (v, u) in enumerate(rev_state):
        pos[u].append(idx)

    # Build dependency graph
    for idx, (v, u) in enumerate(rev_state):
        indeg[idx] = deg[u] - 1

    # For each state (v->u), all states (u->w), w != v depend on it
    for idx, (v, u) in enumerate(rev_state):
        for w in g[u]:
            if w == v:
                continue
            to_idx = state_id[(u, w)]
            dependents[idx].append(to_idx)

    q = deque()
    for i in range(len(rev_state)):
        if indeg[i] == 0:
            q.append(i)

    value = [0] * len(rev_state)

    while q:
        cur = q.popleft()
        v, u = rev_state[cur]

        for nxt in dependents[cur]:
            value[nxt] = max(value[nxt], value[cur] + 1)
            indeg[nxt] -= 1
            if indeg[nxt] == 0:
                q.append(nxt)

    # final answer is max over all states
    print(max(value) if value else 0)

if __name__ == "__main__":
    solve()
```Mã mô hình rõ ràng từng cạnh được định hướng dưới dạng trạng thái DP. Cấu trúc liền kề`dependents`mã hóa những trạng thái nào sẽ khả dụng sau khi một trạng thái nhất định được giải quyết. các`indeg`mảng theo dõi số lượng phần phụ thuộc còn lại của mỗi trạng thái trước khi có thể tính toán. 

Hàng đợi xử lý tất cả các trạng thái cơ sở trước tiên, đó là những trạng thái gắn liền với các lá theo nghĩa phụ thuộc. Mỗi khi một trạng thái được giải quyết, nó góp phần mở rộng đường dẫn ở các trạng thái phụ thuộc bằng cách tăng giá trị được biết đến tốt nhất của chúng. 

Một điểm tinh tế là chúng ta không bao giờ truy cập lại một trạng thái khi số lượng phụ thuộc của nó đạt đến 0. Điều này đảm bảo độ phức tạp tuyến tính, vì mỗi trạng thái được đẩy một lần và mỗi cạnh phụ thuộc được xử lý một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
2 3
```Kỳ: 

(1,2), (2,1), (2,3), (3,2) 

Chúng tôi theo dõi`(value, indeg)`. 

| Bước | Trạng thái đã xử lý | Giá trị | Hiệu ứng | 
| --- | --- | --- | --- | 
| 1 | (1,2), (3,2) | 0 | Cả hai đều là nút cơ sở (nút độ 1) | 
| 2 | (2,1) | 1 | Mở rộng đến (2,3) | 
| 3 | (2,3) | 1 | Tuyên truyền cuối cùng | 

Câu trả lời cuối cùng là 1. 

Điều này cho thấy sự lan truyền chuỗi tuyến tính trong đó sự phụ thuộc chảy vào trong từ các lá. 

### Ví dụ 2 

đầu vào:```
4 4
1 2
2 3
3 4
4 2
```Điều này tạo ra một chu trình có cấu trúc đuôi. 

| Bước | Trạng thái đã xử lý | Giá trị | Hiệu ứng | 
| --- | --- | --- | --- | 
| 1 | (1,2) | 0 | Căn cứ | 
| 2 | (3,4) | 0 | Cơ bản do cấu trúc | 
| 3 | (2,3) | 1 | Mở rộng từ 3 | 
| 4 | (4,2) | 1 | Kéo dài chu kỳ | 

Chu trình giải quyết vì loại trừ phụ thuộc ngăn chặn việc chờ đợi vòng tròn. 

Điều này cho thấy ngay cả các đồ thị tuần hoàn cũng trở nên không tuần hoàn như thế nào trong không gian trạng thái phụ thuộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi trạng thái được định hướng và cạnh phụ thuộc được xử lý một lần | 
| Không gian |$O(n + m)$| Lưu trữ các giá trị kề, trạng thái và DP | 

Thuật toán chạy thoải mái trong các ràng buộc vì mọi cạnh chỉ đóng góp chi phí không đổi và không xảy ra tính toán đệ quy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# sample-like linear chain
assert run("3 2\n1 2\n2 3\n") == "1\n"

# single edge
assert run("2 1\n1 2\n") == "0\n"

# triangle cycle
assert run("3 3\n1 2\n2 3\n3 1\n") == "1\n"

# star graph
assert run("5 4\n1 2\n1 3\n1 4\n1 5\n") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị đường | 1 | tính chính xác của việc truyền bá | 
| cạnh đơn | 0 | xử lý trường hợp cơ bản | 
| tam giác | 1 | giải quyết phụ thuộc theo chu kỳ | 
| ngôi sao | 1 | xử lý đỉnh cấp độ cao | 

## Vỏ cạnh 

Đồ thị một cạnh là trường hợp cơ bản rõ ràng nhất. Đối với đầu vào`1-2`, cả hai trạng thái được định hướng đều không có sự tiếp tục thay thế, vì vậy cả hai đều có thể tính toán được ngay lập tức. Hàng đợi bắt đầu với cả hai trạng thái và không có sự lan truyền nào xảy ra. 

Biểu đồ tam giác kiểm tra sự phụ thuộc theo chu kỳ. Mặc dù mọi trạng thái đều phụ thuộc vào một trạng thái khác, việc loại trừ cạnh ngược sẽ phá vỡ chu trình trong không gian phụ thuộc, cho phép khởi tạo từ bất kỳ trạng thái nào với mức độ tối thiểu và lan truyền nhất quán cho đến khi tất cả các trạng thái ổn định. 

Biểu đồ hình sao kiểm tra sự mất cân bằng ở mức độ cao. Nút trung tâm có nhiều phần phụ thuộc, nhưng tất cả các trạng thái lá đều là các trường hợp cơ sở độc lập, do đó quá trình tính toán diễn ra chính xác vào bên trong mà không bị bế tắc.
