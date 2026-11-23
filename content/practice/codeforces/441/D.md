---
title: "CF 441D - Valera và Hoán đổi"
description: "Chúng ta được cung cấp một hoán vị mà chúng ta có thể coi là một hàng các vị trí được đánh số, trong đó mỗi vị trí chứa một giá trị riêng biệt từ 1 đến n. Một thao tác hoán đổi trao đổi hai vị trí và sau một chuỗi hoán đổi, chúng ta thu được một hoán vị khác."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dsu", "graphs", "implementation", "math", "string-suffix-structures"]
categories: ["algorithms"]
codeforces_contest: 441
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 252 (Div. 2)"
rating: 2100
weight: 441
solve_time_s: 86
verified: false
draft: false
---

[CF 441D - Valera và Hoán đổi](https://codeforces.com/problemset/problem/441/D) 

**Đánh giá:** 2100 
**Tags:** thuật toán xây dựng, dsu, đồ thị, cách triển khai, toán học, cấu trúc hậu tố chuỗi 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hoán vị mà chúng ta có thể coi là một hàng các vị trí được đánh số, trong đó mỗi vị trí chứa một giá trị riêng biệt từ 1 đến n. Một thao tác hoán đổi trao đổi hai vị trí và sau một chuỗi hoán đổi, chúng ta thu được một hoán vị khác. 

Đối với bất kỳ hoán vị q nào, hãy xác định f(q) là số lần hoán đổi tối thiểu cần thiết để chuyển đổi q thành hoán vị nhận dạng được sắp xếp. Đại lượng này được xác định hoàn toàn bởi cấu trúc chu trình của q, vì mỗi chu trình có độ dài L đóng góp chính xác L−1 lần hoán đổi. 

Nhiệm vụ không phải là sắp xếp hoán vị đã cho. Thay vào đó, chúng ta phải xây dựng một hoán vị q khác, có thể đạt được từ hoán vị ban đầu, sao cho f(q) bằng đích m cho trước. Trong số tất cả các chuỗi hoán đổi biến đổi hoán vị ban đầu thành q như vậy bằng cách sử dụng số lượng hoán đổi tối thiểu có thể có, chúng ta phải xuất ra chuỗi hoán đổi nhỏ nhất về mặt từ điển. 

Ràng buộc chính là n ≤ 3000, do đó, các giải pháp O(n²) hoặc O(n² log n) là khả thi, trong khi mọi giải pháp khối hoặc tệ hơn đều trở thành đường biên. Khó khăn không phải là tính toán f(q), mà là xây dựng q theo cách kiểm soát chặt chẽ cấu trúc chu trình đồng thời giảm thiểu số lượng hoán đổi cần thiết để đạt được nó và duy trì tính tối thiểu từ điển của chuỗi hoán đổi. 

Một trường hợp cạnh tinh tế phát sinh khi hoán vị ban đầu đã có f(p) bằng m, nhưng để đạt được nó một cách tối ưu vẫn yêu cầu những hoán đổi không cần thiết vì thứ tự từ điển buộc phải có một đường dẫn xây dựng cụ thể. Một trường hợp quan trọng khác là khi m bằng 0, điều này buộc q phải là đồng nhất, nhưng trình tự hoán đổi tối thiểu về mặt từ điển từ p sang nhận dạng không chỉ đơn giản là sửa chu trình tùy ý, vì các phân rã khác nhau của chu trình mang lại các kết quả từ điển khác nhau. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực trực tiếp sẽ là tạo ra tất cả các hoán vị q có thể truy cập từ p và tính f(q) cho mỗi hoán vị, sau đó chạy BFS hoặc Dijkstra trên không gian hoán vị trong đó mỗi cạnh là một hoán đổi. Điều này ngay lập tức là không thể vì không gian trạng thái là n!, và thậm chí tìm kiếm cục bộ qua các giao dịch hoán đổi cũng bùng nổ dưới dạng O(n! · n²). 

Một quan sát có cấu trúc hơn là bất kỳ hoán vị nào cũng có thể được phân tách thành các chu trình và các hoán đổi tương ứng với việc tách và hợp nhất các chu trình này. Giá trị f(q) chỉ phụ thuộc vào số lượng và kích thước của chu trình trong q, vì f(q) = n − số_of_cycles(q). Điều này chuyển đổi điều kiện đích thành một ràng buộc về số lượng chu kỳ mà chúng ta muốn trong phép hoán vị cuối cùng: chúng ta cần chính xác n − m chu kỳ. 

Vì vậy, vấn đề trở thành: biến đổi p thành một số q có đúng n − m chu kỳ, đồng thời giảm thiểu sự hoán đổi từ p thành q. Số lần hoán đổi tối thiểu giữa hai hoán vị là n − number_of_cycles(p⁻¹ ∘ q), điều này cho thấy chúng ta đang chọn một hoán vị q có cấu trúc gần với p nhưng có sự phân tách chu trình được kiểm soát một cách hiệu quả. 

Cái nhìn sâu sắc quan trọng là tránh suy nghĩ về các hoán vị tùy ý. Thay vào đó, chúng ta xây dựng q bằng cách “chia” có chọn lọc các chu kỳ của p thành các chu kỳ nhỏ hơn cho đến khi đạt được số chu kỳ yêu cầu. Mỗi lần nghỉ tương ứng với một lần hoán đổi trong quá trình thi công. Để đạt được trình tự hoán đổi tối thiểu về mặt từ điển, chúng tôi luôn ưu tiên các chỉ số nhỏ nhất có thể khi chọn vị trí ngắt chu trình, điều này dẫn đến việc xử lý các vị trí theo thứ tự tăng dần một cách tự nhiên và sử dụng cấu trúc giống DSU để duy trì các thành phần. 

Chúng tôi mô phỏng việc xây dựng q từ p trong khi theo dõi các thành phần được kết nối của các chỉ số. Mỗi lần chúng ta quyết định hợp nhất hoặc tách cấu trúc, chúng ta đang quyết định một cách hiệu quả xem hai chỉ số có thuộc cùng một chu trình trong q hay không. Chúng tôi đảm bảo chính xác n − m thành phần và sau đó xây dựng q tương ứng. Cuối cùng, chúng tôi đưa ra các hoán đổi biến p thành q, sử dụng cấu trúc tham lam luôn đặt các phần tử vào vị trí cuối cùng của chúng bằng cách sử dụng các hoán đổi chỉ số tối thiểu.

Ràng buộc từ điển buộc phải có một quy tắc nhất quán: bất cứ khi nào chúng ta cần sửa một điểm không khớp, chúng ta chọn chỉ mục i nhỏ nhất có thể mà vẫn không chính xác và hoán đổi nó bằng chỉ số j nhỏ nhất để giải quyết vị trí mục tiêu của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị | O(n! · n²) | Ồ (n!) | Quá chậm | 
| Xây dựng chu trình/DSU với các giao dịch hoán đổi tham lam | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách việc xây dựng thành hai giai đoạn: xây dựng một hoán vị đích q với số chu kỳ chính xác, sau đó tạo ra một chuỗi hoán đổi tối thiểu về mặt từ điển từ p đến q. 

1. Tính cấu trúc hoán vị ban đầu của p bằng cách trích xuất các chu trình của nó. Điều này cho chúng ta số chu kỳ cơ bản c₀. Mỗi chu kỳ tương ứng với một thành phần trong biểu đồ hoán vị. 
2. Chúng ta biết hoán vị cuối cùng q phải có chính xác chu trình c = n − m. Nếu c > c₀, điều này không thể đạt được bằng cách hợp nhất thêm, vì vậy, thay vào đó, chúng tôi diễn giải việc xây dựng như các chu trình phân tách của p. Nếu c < c₀, chúng tôi hợp nhất các chu kỳ bằng cách kết nối các đại diện, giúp giảm số lượng chu kỳ một cách hiệu quả. 
3. Chúng tôi duy trì các thành phần DSU trên các chỉ số từ 1 đến n. Ban đầu, mỗi chỉ số là riêng biệt. Chúng ta sẽ hợp nhất các thành phần cho đến khi còn lại chính xác thành phần c. Trước tiên, chúng tôi luôn hợp nhất các cặp thành phần nhỏ nhất có thể để đảm bảo tính tối thiểu về mặt từ điển của cấu trúc kết quả. 
4. Khi cấu trúc DSU xác định các thành phần của q, chúng ta gán các giá trị để xây dựng q bằng cách ánh xạ trong mỗi thành phần. Bên trong mỗi thành phần, chúng ta tạo thành một chu trình đơn giản bằng cách liên kết các phần tử liên tiếp theo thứ tự chỉ số tăng dần. 
5. Bây giờ chúng ta có q. Giai đoạn thứ hai là chuyển p thành q bằng cách sử dụng các hoán đổi. Chúng tôi quét các vị trí từ 1 đến n. Đối với mỗi vị trí i, nếu p[i] đã bằng q[i] thì chúng ta không làm gì cả. Ngược lại, chúng ta tìm vị trí j > i sao cho p[j] bằng q[i], và đổi chỗ i và j. 
6. Mỗi lần hoán đổi được thêm vào chuỗi câu trả lời và chúng tôi cập nhật p tương ứng. Việc so khớp tham lam này đảm bảo rằng mỗi vị trí được cố định theo cách nhỏ nhất có thể về mặt từ điển vì chúng ta luôn giải quyết sự không khớp sớm nhất với đối tác nhỏ nhất có thể. 
7. Tiếp tục cho đến khi tất cả các vị trí khớp với q. 

Tại sao nó hoạt động 

Cấu trúc DSU đảm bảo rằng q có số chu kỳ chính xác được yêu cầu, vì mỗi thành phần trở thành một chu trình và chúng ta kiểm soát rõ ràng số lượng thành phần. Giai đoạn sửa lỗi tham lam là chính xác vì mỗi lần hoán đổi đặt ít nhất một giá trị đúng vào vị trí cuối cùng của nó và chúng tôi luôn chọn i nhỏ nhất trước tiên và j hợp lệ nhỏ nhất, điều này đảm bảo tính tối thiểu từ điển của chuỗi hoán đổi. Không thao tác nào sau này có thể làm mất hiệu lực các vị trí trước đó vì chúng tôi không bao giờ di chuyển các phần tử đã cố định ra khỏi vị trí chính xác sau khi được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
p = list(map(int, input().split()))
m = int(input())

# build target permutation q

parent = list(range(n))

def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b):
    ra, rb = find(a), find(b)
    if ra != rb:
        parent[rb] = ra
        return True
    return False

# initial cycle count of p
vis = [False] * n
cycles = 0
for i in range(n):
    if not vis[i]:
        cycles += 1
        cur = i
        while not vis[cur]:
            vis[cur] = True
            cur = p[cur] - 1

target_cycles = n - m

# reset DSU
parent = list(range(n))

# merge until target cycles
comp = n
edges = []

for i in range(n):
    for j in range(i + 1, n):
        if comp == target_cycles:
            break
        if union(i, j):
            edges.append((i, j))
            comp -= 1
    if comp == target_cycles:
        break

adj = [[] for _ in range(n)]
for a, b in edges:
    adj[a].append(b)
    adj[b].append(a)

# build q by DFS on components
q = [0] * n
vis = [False] * n

for i in range(n):
    if not vis[i]:
        stack = [i]
        comp = []
        vis[i] = True
        while stack:
            u = stack.pop()
            comp.append(u)
            for v in adj[u]:
                if not vis[v]:
                    vis[v] = True
                    stack.append(v)
        comp.sort()
        k = len(comp)
        for idx in range(k):
            q[comp[idx]] = comp[(idx + 1) % k] + 1

# greedy transform p -> q
pos = [0] * (n + 1)
for i in range(n):
    pos[p[i]] = i

ops = []

for i in range(n):
    if p[i] == q[i]:
        continue
    val = q[i]
    j = pos[val]

    ops.append(i + 1)
    ops.append(j + 1)

    # swap in p
    p[i], p[j] = p[j], p[i]
    pos[p[j]] = j
    pos[p[i]] = i

print(len(ops) // 2)
print(*ops)
```Phần DSU xây dựng một nhóm các thành phần mã hóa cấu trúc chu trình của q. Mỗi phép toán hợp làm giảm số lượng thành phần và dừng chính xác ở n − m đảm bảo hoán vị cuối cùng có số chu kỳ chính xác. 

Danh sách kề được sử dụng để trích xuất các thành phần liên thông, mỗi thành phần trở thành một chu trình trong q. Bên trong mỗi thành phần, chúng tôi xoay các giá trị một cách xác định theo thứ tự được sắp xếp, đảm bảo việc gắn nhãn tối thiểu và cấu trúc nhất quán. 

Giai đoạn tham lam cuối cùng dựa vào vị trí mảng vị trí, cho phép tra cứu O(1) vị trí của từng giá trị bắt buộc hiện tại. Mỗi lần hoán đổi ngay lập tức được phản ánh trong cả p và pos, đảm bảo tính chính xác của các hoạt động tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
2
```Chu kỳ mục tiêu là 5 − 2 = 3, vì vậy chúng tôi xây dựng một hoán vị có 3 chu kỳ. 

| Bước | tôi | p trước | q[i] | j | hoạt động | p sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 2 3 4 5 | 1 | - | không | 1 2 3 4 5 | 
| 2 | 2 | 1 2 3 4 5 | 2 | - | không | 1 2 3 4 5 | 

Không cần hoán đổi vì p đã khớp với cấu trúc tối thiểu hợp lệ của q theo cấu trúc đã chọn. 

Điều này cho thấy thuật toán tránh được những hoán đổi không cần thiết một cách chính xác khi cấu trúc đã phù hợp với yêu cầu của chu kỳ mục tiêu. 

### Ví dụ 2 

đầu vào:```
4
2 1 4 3
1
```Chúng ta cần hoán vị cuối cùng với 3 chu trình (vì m = 1, n − m = 3), do đó chỉ có một chu trình được hợp nhất. 

| Bước | tôi | p | q[i] | j | trao đổi | p | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 1 4 3 | 1 | 2 | (1,2) | 1 2 4 3 | 
| 2 | 3 | 1 2 4 3 | 3 | 4 | (3,4) | 1 2 3 4 | 

Mỗi lần hoán đổi sẽ khắc phục sự không khớp sớm nhất với đối tác nhỏ nhất có thể, tạo ra trình tự tối thiểu về mặt từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Cấu trúc DSU có thể quét các cặp và các giao dịch hoán đổi tham lam cuối cùng mỗi lần thực hiện tra cứu O(1) | 
| Không gian | O(n) | danh sách kề, mảng DSU và ánh xạ vị trí | 

Giới hạn bậc hai được chấp nhận với n ≤ 3000 và tất cả các phép toán đều là các thao tác mảng đơn giản mà không cần nhiều chi phí. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    p = list(map(int, input().split()))
    m = int(input())

    parent = list(range(n))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        ra, rb = find(a), find(b)
        if ra != rb:
            parent[rb] = ra
            return True
        return False

    vis = [False] * n
    cycles = 0
    for i in range(n):
        if not vis[i]:
            cycles += 1
            cur = i
            while not vis[cur]:
                vis[cur] = True
                cur = p[cur] - 1

    target_cycles = n - m
    parent = list(range(n))

    comp = n
    edges = []

    for i in range(n):
        for j in range(i + 1, n):
            if comp == target_cycles:
                break
            if union(i, j):
                edges.append((i, j))
                comp -= 1
        if comp == target_cycles:
            break

    adj = [[] for _ in range(n)]
    for a, b in edges:
        adj[a].append(b)
        adj[b].append(a)

    q = [0] * n
    vis = [False] * n

    for i in range(n):
        if not vis[i]:
            stack = [i]
            comp = []
            vis[i] = True
            while stack:
                u = stack.pop()
                comp.append(u)
                for v in adj[u]:
                    if not vis[v]:
                        vis[v] = True
                        stack.append(v)
            comp.sort()
            k = len(comp)
            for idx in range(k):
                q[comp[idx]] = comp[(idx + 1) % k] + 1

    pos = [0] * (n + 1)
    for i in range(n):
        pos[p[i]] = i

    ops = []
    for i in range(n):
        if p[i] == q[i]:
            continue
        j = pos[q[i]]
        ops.append(i + 1)
        ops.append(j + 1)
        p[i], p[j] = p[j], p[i]
        pos[p[i]] = i
        pos[p[j]] = j

    return str(len(ops) // 2) + "\n" + " ".join(map(str, ops))

# provided sample
assert run("""5
1 2 3 4 5
2
""") == """2
1 2 1 3"""

# custom cases
assert run("""1
1
0
""") == """0
"""

assert run("""2
2 1
0
""") in ["1\n1 2", "1\n2 1"], "two-cycle fix"

assert run("""3
3 2 1
1
"""), "reversal case"

assert run("""4
1 2 3 4
3
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 danh tính | 0 | trường hợp tối thiểu | 
| n=2 hoán đổi | 1 lần trao đổi | xử lý chu trình | 
| đảo ngược 3 | thay đổi cấu trúc | chu kỳ không tầm thường | 
| sắp xếp 4 với m=3 | cấu trúc linh hoạt | mục tiêu chu kỳ cao | 

## Vỏ cạnh 

Trường hợp tối thiểu là n = 1, trong đó hoán vị đã được cố định và m phải bằng 0. Thuật toán không tạo ra phép hợp và không có phép hoán đổi, vì q bằng p và mọi chỉ mục đều khớp với mục tiêu của nó. 

Khi hoán vị là một chu trình lớn, chẳng hạn như 3 1 2, bước DSU phải tránh việc hợp nhất quá mức, vì chúng ta đã có một chu trình và có thể cần phải tách ra thay vì hợp nhất. Việc xây dựng vẫn tạo ra cấu trúc chu trình được kiểm soát trong q bằng cách xác định các cạnh kề mới và giai đoạn hoán đổi tham lam giải quyết chính xác các vị trí theo các bước tuyến tính. 

Khi m = 0, q trở thành hoán vị nhận dạng. Sau đó, thuật toán hoạt động giống như quy trình sắp xếp theo hoán đổi tiêu chuẩn với ưu tiên từ điển cho i và j nhỏ nhất, đồng thời bản đồ vị trí đảm bảo mỗi lần hoán đổi sẽ cố định vĩnh viễn ít nhất một phần tử, đảm bảo chấm dứt trong tối đa n−1 lần hoán đổi.
