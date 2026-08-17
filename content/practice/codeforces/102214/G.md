---
title: "CF 102214G - Từ"
description: "Trò chơi có thể được mô hình hóa dưới dạng đồ thị có hướng. Mỗi chữ cái là một đỉnh và mỗi từ là một cạnh có hướng từ chữ cái đầu tiên đến chữ cái cuối cùng của nó."
date: "2026-08-18T00:14:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102214
codeforces_index: "G"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u043e\u0435 \u043b\u0438\u0447\u043d\u043e\u0435 \u043f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0418\u041a\u0418\u0422 \u0421\u0424\u0423 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2015"
rating: 0
weight: 102214
solve_time_s: 97
verified: true
draft: false
---

[CF 102214G - Từ](https://codeforces.com/problemset/problem/102214/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi có thể được mô hình hóa dưới dạng đồ thị có hướng. Mỗi chữ cái là một đỉnh và mỗi từ là một cạnh có hướng từ chữ cái đầu tiên đến chữ cái cuối cùng của nó. Nếu người chơi vừa nói một từ được biểu thị bằng cạnh (u \to v) thì từ tiếp theo phải bắt đầu từ (v), do đó cạnh tiếp theo phải rời khỏi đỉnh (v). 

Nhiệm vụ là thêm càng ít từ mới, do đó càng ít cạnh được định hướng càng tốt để từ mỗi từ hiện có, cuối cùng chúng ta có thể tiếp cận mọi từ khác thông qua các bước di chuyển trò chơi hợp lệ. Dữ liệu đầu vào chứa tối đa (N,M\le 100000), trong đó (N) là kích thước bảng chữ cái và (M) là số từ hiện có. Giới hạn chính thức là 2 giây và 256 MB. Thuật toán bậc hai sẽ yêu cầu khoảng (10^{10}) phép toán ở giới hạn trên, do đó, giải pháp về cơ bản phải là tuyến tính hoặc (O((N+M)\log N)). 

Có một vấn đề khó mô hình hóa: các chữ cái không có từ sẽ không tham gia vào câu trả lời. Ví dụ,```
5 1
1 2
```có câu trả lời`1`. Chúng ta chỉ cần thêm`2 -> 1`. Việc triển khai bất cẩn tạo ra SCC cho cả năm chữ cái và tính ba chữ cái riêng biệt là các thành phần cần kết nối sẽ tạo ra câu trả lời lớn hơn. 

Một trường hợp cạnh khác là đồ thị đã được kết nối mạnh. Ví dụ,```
3 3
1 2
2 3
3 1
```có câu trả lời`0`. Đã có một lộ trình hợp lệ từ mọi từ đến mọi từ khác. Việc triển khai áp dụng một cách mù quáng công thức nguồn/đích thông thường mà không kiểm tra xem liệu chỉ có một SCC có trả về sai hay không`1`. 

Một vòng lặp tự cũng đáng được chú ý. Vì```
1 1
1 1
```câu trả lời là`0`. Từ đơn lẻ chỉ có thể được theo sau bởi một từ khác bắt đầu bằng chữ cái 1 và cùng một chữ cái vừa là đầu vừa là cuối. Vì chỉ có một SCC hoạt động nên không cần từ mới. 

Cuối cùng, một số từ có thể có cùng một cặp chữ cái điểm cuối. Chúng là những từ khác nhau trong trò chơi, nhưng về khả năng tiếp cận, chúng hoạt động giống hệt như các cạnh của biểu đồ. Việc tính toán SCC chỉ cần tồn tại một cạnh giữa hai chữ cái, do đó các cạnh trùng lặp không làm thay đổi cấu trúc thành phần. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử thêm các từ có thể cho đến khi biểu đồ kết quả trở nên được kết nối chặt chẽ. Có thể có (N^2) cặp chữ cái được sắp xếp, do đó đã có (2^{N^2}) tập hợp con các từ có thể có. Đối với mỗi tập hợp con ứng cử viên, chúng tôi sẽ phải xây dựng biểu đồ kết quả và kiểm tra khả năng tiếp cận, yêu cầu (O(N+M+N^2)) hoạt động trong trường hợp xấu nhất. Số lượng phép tính trong trường hợp xấu nhất thu được là (O(2^{N^2}(N+M+N^2))), điều này hoàn toàn không khả thi ngay cả đối với các bảng chữ cái rất nhỏ. Lực lượng vũ phu đúng về mặt khái niệm vì nó kiểm tra chính xác thuộc tính mà trò chơi yêu cầu, nhưng nó bỏ qua cấu trúc của đồ thị có hướng. 

Nhận xét quan trọng là điều kiện của trò chơi tương đương với khả năng kết nối mạnh mẽ của biểu đồ được hình thành bởi các chữ cái thực sự xuất hiện trong từ. Giả sử có một đường dẫn từ chữ cái cuối cùng của từ (A) đến chữ cái đầu tiên của từ (B). Chúng ta có thể chơi (A), đi theo con đường đó bằng cách sử dụng các từ trung gian và cuối cùng chơi (B). Do đó, mọi từ có thể tiếp cận mọi từ khác một cách chính xác khi mọi chữ cái hoạt động có thể tiếp cận mọi chữ cái hoạt động khác. 

Khi vấn đề được diễn đạt theo cách này, các thành phần được kết nối chặt chẽ sẽ tạo ra toàn bộ cấu trúc. Bên trong một SCC, mọi đỉnh đều có thể chạm tới mọi đỉnh khác. Sau khi thu gọn mọi SCC thành một đỉnh, biểu đồ ngưng tụ thu được là DAG. Nếu nó có nhiều hơn một thành phần thì mọi thành phần nguồn đều cần một cạnh vào và mọi thành phần đích đều cần một cạnh đi. Việc thêm các cạnh được định hướng giữa các thành phần có thể khắc phục được cả hai yêu cầu. 

Gọi (S) là số lượng SCC nguồn và (T) số lượng SCC đích. Cần ít nhất (S) từ mới vì một từ được thêm vào chỉ có một điểm bắt đầu và do đó có thể cung cấp một cạnh đến cho nhiều nhất một SCC nguồn. Tương tự, cần ít nhất (T) từ mới vì một từ được thêm vào có thể để lại nhiều nhất một SCC chìm. Do đó ít nhất (\max(S,T)) từ là cần thiết. 

Giới hạn dưới đó cũng có thể đạt được. Chúng ta có thể kết nối các thành phần chìm với các thành phần nguồn theo chu kỳ, sử dụng lại nguồn hoặc phần chìm khi số lượng của chúng khác nhau. Với các cạnh được thêm vào (K=\max(S,T)), mọi nguồn đều nhận được một cạnh đi vào và mọi bồn nhận được một cạnh đi ra, và đồ thị ngưng tụ thu được sẽ được kết nối chặt chẽ. Vì vậy, câu trả lời là chính xác (\max(S,T)), ngoại trừ một biểu đồ chỉ có một SCC hoạt động đã không cần thêm từ nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^{N^2}(N+M+N^2))) | (O(N^2+N+M)) | Quá chậm | 
| ngưng tụ SCC | (O(N+M)) | (O(N+M)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng sử dụng các chữ cái làm đỉnh và các từ làm cạnh. Đồng thời xây dựng biểu đồ đảo ngược. Chúng ta chỉ cần các đỉnh xuất hiện dưới dạng chữ cái đầu tiên hoặc cuối cùng của một số từ, bởi vì các chữ cái riêng biệt không thể ảnh hưởng đến quá trình chuyển đổi giữa các từ. 
2. Tìm tất cả các thành phần liên thông mạnh bằng thuật toán Kosaraju. DFS đầu tiên ghi lại các đỉnh theo thứ tự hoàn thiện và DFS thứ hai trên biểu đồ đảo ngược gán các mã định danh thành phần theo thứ tự hoàn thiện ngược lại. 
3. Nếu tất cả các đỉnh hoạt động thuộc về một SCC, xuất ra`0`. Mỗi từ đã có thể đạt được mọi từ khác. 
4. Với mỗi cạnh gốc (u\to v), hãy so sánh các định danh SCC của (u) và (v). Nếu chúng khác nhau thì đây là cạnh giữa hai đỉnh của DAG ngưng tụ. Tăng mức độ của SCC đích và mức độ bên ngoài của SCC nguồn. 
5. Đếm các SCC có bậc bằng 0 và SCC có bậc ngoài bằng 0. Đây là các thành phần nguồn và phần chìm của biểu đồ ngưng tụ. 
6. Xuất ra số lớn hơn trong hai số đó. Đây là số lượng từ mới tối thiểu cần có vì mọi nguồn đều cần kết nối đến và mọi sink đều cần kết nối đi, trong khi cấu trúc tuần hoàn được mô tả ở trên đạt được cả hai yêu cầu với chính xác số cạnh đó. 

### Tại sao nó hoạt động 

Sau khi thu gọn SCC, mọi cạnh còn lại sẽ đi giữa các thành phần khác nhau và biểu đồ thu được là DAG. Thành phần nguồn không có đường dẫn đến từ thành phần khác, vì vậy ít nhất một từ mới được thêm vào phải nhập vào thành phần đó. Tương tự như vậy, một thành phần chìm không thể tiếp cận thành phần khác nên ít nhất một từ mới được thêm vào phải rời khỏi thành phần đó. Do đó, bất kỳ giải pháp hợp lệ nào cũng cần ít nhất (\max(S,T)) từ mới. 

Ngược lại, lấy SCC đích và SCC nguồn và kết nối chúng theo chu kỳ. Nếu một bên có ít thành phần hơn, hãy sử dụng lại các thành phần từ bên đó khi cần thiết. Sau đó, mọi nguồn sẽ nhận được một cạnh đi vào, mọi điểm chìm sẽ nhận được một cạnh đi ra và việc đi theo các kết nối tuần hoàn cho phép chúng ta di chuyển từ mọi SCC sang mọi SCC khác. Vì mỗi SCC đã được kết nối mạnh mẽ bên trong nên toàn bộ biểu đồ sẽ được kết nối mạnh mẽ. Giới hạn dưới có thể đạt được, vì vậy (\max(S,T)) là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]
    rev = [[] for _ in range(n)]
    edges = []
    active = [False] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        graph[u].append(v)
        rev[v].append(u)
        edges.append((u, v))

        active[u] = True
        active[v] = True

    # First pass of Kosaraju: finishing order.
    visited = [False] * n
    order = []

    for start in range(n):
        if not active[start] or visited[start]:
            continue

        visited[start] = True
        stack = [(start, 0)]

        while stack:
            u, idx = stack[-1]

            if idx < len(graph[u]):
                v = graph[u][idx]
                stack[-1] = (u, idx + 1)

                if not visited[v]:
                    visited[v] = True
                    stack.append((v, 0))
            else:
                order.append(u)
                stack.pop()

    # Second pass on the reversed graph: assign SCCs.
    comp = [-1] * n
    comp_count = 0

    for start in reversed(order):
        if comp[start] != -1:
            continue

        comp[start] = comp_count
        stack = [start]

        while stack:
            u = stack.pop()

            for v in rev[u]:
                if comp[v] == -1:
                    comp[v] = comp_count
                    stack.append(v)

        comp_count += 1

    if comp_count == 1:
        print(0)
        return

    indeg = [0] * comp_count
    outdeg = [0] * comp_count

    for u, v in edges:
        cu = comp[u]
        cv = comp[v]

        if cu != cv:
            outdeg[cu] = 1
            indeg[cv] = 1

    sources = 0
    sinks = 0

    for c in range(comp_count):
        if indeg[c] == 0:
            sources += 1
        if outdeg[c] == 0:
            sinks += 1

    print(max(sources, sinks))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào xây dựng cả biểu đồ gốc và biểu đồ đảo ngược vì Kosaraju cần hai hướng. các`active`mảng ngăn các chữ cái trong bảng chữ cái bị cô lập khỏi bị coi là thành phần có ý nghĩa. 

DFS đầu tiên là lặp lại chứ không phải đệ quy. Với (N=100000), DFS đệ quy có thể vượt quá giới hạn đệ quy của Python và cũng có thể phát sinh chi phí thông dịch không cần thiết. Ngăn xếp lưu trữ cả đỉnh hiện tại và chỉ mục của cạnh đi tiếp theo, cho phép chúng ta tái tạo thứ tự hoàn thiện DFS đệ quy mà không cần đệ quy. 

DFS thứ hai duyệt qua biểu đồ đảo ngược và gán một mã định danh thành phần cho mọi đỉnh có thể tiếp cận. Vì các đỉnh được xử lý theo thứ tự hoàn thiện giảm dần kể từ lần duyệt đầu tiên nên mỗi lần truyền tải nằm bên trong chính xác một SCC. 

Sau khi biết được SCC, chỉ có các cạnh giao nhau giữa các thành phần khác nhau mới quan trọng. Chúng ta không cần số cạnh chính xác, chỉ cần mỗi thành phần có ít nhất một cạnh vào hoặc cạnh ra, vì vậy`indeg`Và`outdeg`được lưu trữ dưới dạng số nguyên giống như boolean. Một cạnh bên trong một SCC bị bỏ qua vì nó không thể ảnh hưởng đến DAG ngưng tụ. 

Trường hợp đặc biệt`comp_count == 1`phải được xử lý trước khi đếm nguồn và bồn. Đối với một thành phần đơn lẻ, nó sẽ có cả độ trong và độ ngoài bằng 0 trong biểu diễn ngưng tụ, nhưng câu trả lời đúng là`0`, không`1`. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên. Tổng dung lượng lưu trữ đồ thị là tuyến tính theo số đỉnh và cạnh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu chính thức chứa ba chu kỳ có hướng, với các cạnh bổ sung từ chu kỳ đầu tiên đến hai chu kỳ còn lại. Câu trả lời của nó là`2`.```
9 11
1 2
2 3
3 1
4 5
5 6
6 4
7 8
8 9
9 7
1 4
1 7
```Thông tin phân hủy và ngưng tụ SCC là: 

| SCC | Đỉnh | Bằng cấp | Bằng cấp cao hơn | Vai trò | 
| --- | --- | --- | --- | --- | 
| 0 | 1, 2, 3 | 0 | 1 | Nguồn | 
| 1 | 4, 5, 6 | 1 | 0 | Chìm | 
| 2 | 7, 8, 9 | 1 | 0 | Chìm | 

Có một SCC nguồn và hai SCC đích, do đó thuật toán tính toán`max(1, 2) = 2`. 

Về mặt khái niệm, hai từ bổ sung có thể kết nối hai thành phần chìm trở lại thành phần nguồn. Khi những kết nối đó tồn tại, cả ba SCC đều nằm trong một cấu trúc được kết nối chặt chẽ. Ví dụ này chứng minh tại sao chỉ đếm các thành phần nguồn là không đủ: cả hai hướng của khả năng tiếp cận đều phải được sửa chữa. 

### Xây dựng ví dụ 2 

Hãy xem xét:```
4 3
1 2
2 3
3 4
```Các SCC đều là các đỉnh đơn. Đồ thị ngưng tụ chỉ đơn giản là một chuỗi. 

| SCC | Đỉnh | Bằng cấp | Bằng cấp cao hơn | Vai trò | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | Nguồn | 
| 1 | 2 | 1 | 1 | Nội bộ | 
| 2 | 3 | 1 | 1 | Nội bộ | 
| 3 | 4 | 1 | 0 | Chìm | 

Có một nguồn và một bồn, vì vậy câu trả lời là`1`. Thêm từ`4 -> 1`đóng chuỗi thành một chu kỳ. 

Dấu vết này cho thấy trường hợp đơn giản nhất trong đó số lượng nguồn và số lượng đích bằng nhau. Nó cũng minh họa tại sao câu trả lời không phải là số lượng SCC trừ đi một. Một cạnh được thêm vào có thể kết nối phần chìm cuối cùng trở lại nguồn ban đầu và sửa chữa toàn bộ chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+M)) | Mỗi đồ thị và cạnh của đồ thị đảo ngược được đi qua một số lần không đổi. | 
| Không gian | (O(N+M)) | Hai danh sách kề, danh sách cạnh, mảng thành phần và ngăn xếp DFS đều là tuyến tính. | 

Giới hạn trên của (100000) chữ cái và (100000) từ làm cho độ phức tạp tuyến tính trở nên phù hợp. Thuật toán thực hiện một vài lần duyệt đồ thị và quét qua các cạnh ban đầu, do đó nó vẫn nằm trong giới hạn 2 giây và 256 MB như dự định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        n, m = map(int, input().split())

        graph = [[] for _ in range(n)]
        rev = [[] for _ in range(n)]
        edges = []
        active = [False] * n

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            graph[u].append(v)
            rev[v].append(u)
            edges.append((u, v))
            active[u] = True
            active[v] = True

        visited = [False] * n
        order = []

        for start in range(n):
            if not active[start] or visited[start]:
                continue

            visited[start] = True
            stack = [(start, 0)]

            while stack:
                u, idx = stack[-1]

                if idx < len(graph[u]):
                    v = graph[u][idx]
                    stack[-1] = (u, idx + 1)

                    if not visited[v]:
                        visited[v] = True
                        stack.append((v, 0))
                else:
                    order.append(u)
                    stack.pop()

        comp = [-1] * n
        comp_count = 0

        for start in reversed(order):
            if comp[start] != -1:
                continue

            comp[start] = comp_count
            stack = [start]

            while stack:
                u = stack.pop()

                for v in rev[u]:
                    if comp[v] == -1:
                        comp[v] = comp_count
                        stack.append(v)

            comp_count += 1

        if comp_count == 1:
            return "0\n"

        indeg = [0] * comp_count
        outdeg = [0] * comp_count

        for u, v in edges:
            cu = comp[u]
            cv = comp[v]

            if cu != cv:
                outdeg[cu] = 1
                indeg[cv] = 1

        sources = sum(x == 0 for x in indeg)
        sinks = sum(x == 0 for x in outdeg)

        return f"{max(sources, sinks)}\n"

    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided sample
assert run("""\
9 11
1 2
2 3
3 1
4 5
5 6
6 4
7 8
8 9
9 7
1 4
1 7
""") == "2\n", "provided sample"

# Minimum size, one self-loop
assert run("""\
1 1
1 1
""") == "0\n", "single self-loop"

# One non-trivial word
assert run("""\
2 1
1 2
""") == "1\n", "single directed edge"

# Isolated letters must not count
assert run("""\
5 1
1 2
""") == "1\n", "isolated letters"

# All active vertices already strongly connected
assert run("""\
4 5
1 2
2 3
3 4
4 1
2 4
""") == "0\n", "already strongly connected"

# Maximum-size style case: a chain of 100000 vertices
n = 100000
chain = [f"{i} {i + 1}" for i in range(1, n)]
chain_input = f"{n} {n - 1}\n" + "\n".join(chain) + "\n"
assert run(chain_input) == "1\n", "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`0`| Biểu đồ kích thước tối thiểu và xử lý tự vòng lặp | 
|`2 1 / 1 2`|`1`| Nguồn đơn và bồn rửa đơn | 
|`5 1 / 1 2`|`1`| Các chữ cái riêng biệt phải được bỏ qua | 
|`4 5 / 1 2 / 2 3 / 3 4 / 4 1 / 2 4`|`0`| Biểu đồ đã được kết nối mạnh mẽ | 
| Chuỗi định hướng 100000 đỉnh |`1`| Đầu vào có kích thước tối đa và hiệu suất tuyến tính | 

## Vỏ cạnh 

Đối với các chữ cái bị cô lập, hãy xem xét đầu vào chính xác```
5 1
1 2
```Chỉ có chữ cái 1 và 2 là hoạt động. Kosaraju tạo hai SCC, một SCC chứa mỗi chữ cái hoạt động. Cái đầu tiên có độ bằng 0 và cái thứ hai có độ ngoài bằng 0, vì vậy số lượng nguồn và số lượng chìm đều là một. Thuật toán trả về`1`, bỏ qua đúng các chữ cái 3, 4 và 5. 

Đối với một đồ thị đã được kết nối mạnh mẽ,```
3 3
1 2
2 3
3 1
```cả ba đỉnh đều nhận được cùng một mã định danh thành phần. Thuật toán dừng ngay tại`comp_count == 1`kiểm tra và in`0`. No source or sink counting is needed because the original graph already permits movement between every pair of words.

 Đối với một từ không lặp,```
2 1
1 2
```có hai SCC. Đỉnh 1 là nguồn và đỉnh 2 là chìm. Một từ mới,`2 -> 1`, kết thúc chu trình, vì vậy câu trả lời là`1`. Điều này phát hiện các triển khai vô tình trả về 0 khi chỉ có một cạnh ban đầu. 

Đối với một chuỗi dài một chiều,```
4 3
1 2
2 3
3 4
```mỗi đỉnh là SCC riêng của nó, nhưng chỉ có đỉnh 1 là nguồn và chỉ có đỉnh 4 là chìm. Câu trả lời vẫn là`1`, bởi vì thêm`4 -> 1`làm cho toàn bộ đồ thị được liên kết chặt chẽ. Việc đếm chính các SCC thay vì SCC nguồn và chìm sẽ đánh giá quá cao câu trả lời. 

Đối với mẫu chính thức, ba chu kỳ tạo thành ba SCC. SCC đầu tiên là nguồn, trong khi SCC thứ hai và thứ ba là điểm chìm. Câu trả lời là`2`, phù hợp với đầu ra mẫu. Ví dụ này minh họa trường hợp bất đối xứng trong đó số lượng thành phần đích lớn hơn số lượng thành phần nguồn, đó chính xác là lý do tại sao công thức sử dụng`max(sources, sinks)`.
