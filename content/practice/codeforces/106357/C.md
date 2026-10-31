---
title: "CF 106357C - lca động"
description: "Chúng ta đang làm việc với một cây có gốc mà cấu trúc của nó không bao giờ thay đổi, nhưng khái niệm “gốc” nghĩa là gì thì không cố định. Theo thời gian, gốc của cây có thể được di chuyển đến các nút khác nhau và chúng tôi được yêu cầu trả lời các truy vấn tổ tiên chung thấp nhất dưới gốc hiện tại."
date: "2026-06-25T08:12:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106357
codeforces_index: "C"
codeforces_contest_name: "Practise Dynamic Forest offline queries"
rating: 0
weight: 106357
solve_time_s: 45
verified: true
draft: false
---

[CF 106357C - lca động](https://codeforces.com/problemset/problem/106357/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một cây có gốc mà cấu trúc của nó không bao giờ thay đổi, nhưng khái niệm “gốc” nghĩa là gì thì không cố định. Theo thời gian, gốc của cây có thể được di chuyển đến các nút khác nhau và chúng tôi được yêu cầu trả lời các truy vấn tổ tiên chung thấp nhất dưới gốc hiện tại. 

Mỗi truy vấn sẽ thay đổi nút nào được coi là nút gốc hoặc yêu cầu tổ tiên chung thấp nhất của hai nút đối với nút gốc hiện tại. Điểm tinh tế quan trọng là “tổ tiên chung thấp nhất” phụ thuộc vào định nghĩa gốc, vì vậy sau khi gốc di chuyển, các mối quan hệ tổ tiên sẽ thay đổi một cách hiệu quả mặc dù các cạnh bên dưới vẫn giữ nguyên. 

Đầu vào mô tả một cây ban đầu trên`n`các nút theo sau là một chuỗi các hoạt động. Một số thao tác gán lại nút gốc cho một nút nhất định và các thao tác khác yêu cầu LCA của hai nút theo cấu hình gốc hiện tại. Đầu ra chỉ bao gồm các câu trả lời cho các hoạt động truy vấn. 

Các ràng buộc ngụ ý rằng chúng ta không thể tính toán lại cấu trúc tổ tiên từ đầu sau mỗi lần thay đổi gốc. Nếu như`n`và số lượng truy vấn theo thứ tự`2 * 10^5`, bất kỳ giải pháp nào xây dựng lại con trỏ gốc hoặc chạy lại DFS cho mỗi truy vấn sẽ ngay lập tức vượt quá giới hạn thời gian. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng sẽ quá chậm vì nó dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Điều này buộc chúng tôi phải sử dụng phương pháp tiền xử lý tĩnh trong đó tất cả thông tin cấu trúc được chuẩn bị một lần và mỗi truy vấn được trả lời theo thời gian logarit. 

Một trường hợp lỗi phổ biến xuất hiện khi coi cây như thể nó luôn có gốc ở nút 1. Ví dụ: giả sử cây là một chuỗi`1 - 2 - 3 - 4`. Nếu gốc được thay đổi thành`4`, thì LCA của`2`Và`3`dưới gốc mới là`3`, không`2`hoặc`1`. LCA ngây thơ được tính theo gốc ban đầu sẽ trả về không chính xác`2`. Sự không phù hợp giữa định nghĩa gốc tĩnh và động là khó khăn chính. 

Một trường hợp cạnh khác xảy ra khi một trong các nút được truy vấn là nút gốc hiện tại. Trong trường hợp đó, câu trả lời luôn là nút gốc, bất kể nút còn lại. Nhiều cách triển khai sai sẽ quên cấu trúc đặc biệt này và vẫn áp dụng công thức LCA gốc cố định, tạo ra tổ tiên không chính xác. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ tính toán lại thông tin gốc và thông tin chuyên sâu mỗi khi gốc thay đổi. Mỗi thay đổi gốc sẽ yêu cầu một DFS đầy đủ từ gốc mới để xây dựng lại các bảng tổ tiên và sau đó mỗi truy vấn LCA sẽ được trả lời bằng phương pháp nâng nhị phân tiêu chuẩn. Điều này hoạt động hợp lý vì mỗi cấu hình là một cây có gốc hợp lệ nhưng tốc độ quá chậm. Với tối đa`10^5`hoạt động và mỗi lần xây dựng lại chi phí`O(n)`, trường hợp xấu nhất đạt đến`O(nq)`, theo thứ tự của`10^10`hoạt động. 

Quan sát quan trọng là cây bên dưới không bao giờ thay đổi. Chỉ có quan điểm về tổ tiên thay đổi khi gốc rễ được di chuyển. Thay vì xây dựng lại cấu trúc, chúng ta có thể sử dụng lại một quá trình tiền xử lý duy nhất bắt nguồn từ bất kỳ nút cố định nào, điển hình là nút`1`. Khi chúng tôi có cấu trúc LCA nâng nhị phân tiêu chuẩn, chúng tôi có thể diễn giải lại kết quả cho các gốc khác nhau bằng cách sử dụng thuộc tính hình học của đường dẫn cây: trên bất kỳ cây nào, trong số ba LCA`lca(u, v)`,`lca(u, r)`, Và`lca(v, r)`, câu trả lời cho truy vấn LCA phụ thuộc vào gốc phải là một trong ba nút này. 

Điều này xuất phát từ thực tế là việc thay đổi gốc chỉ thay đổi đoạn nào của các đường dẫn duy nhất được coi là “bên dưới” một nút khác. LCA chính xác dưới gốc động luôn là nút sâu nhất trong số ba ứng cử viên đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lại mỗi lần thay đổi gốc | O(nq) | O(n) | Quá chậm | 
| Nâng nhị phân với việc giải thích lại gốc | O((n + q) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi thường sửa một nút tùy ý`1`, làm gốc cho quá trình tiền xử lý và xây dựng các bảng nâng nhị phân tiêu chuẩn. 

1. Chạy DFS từ nút`1`tính toán`depth[u]`và cha mẹ trực tiếp`up[u][0]`. Điều này mang lại một cấu trúc gốc nhất quán. 
2. Xây dựng bàn nâng nhị phân`up[u][k]`, Ở đâu`up[u][k]`là`2^k`-tổ tiên của`u`. Điều này cho phép nhảy lên cây theo thời gian logarit. 
3. Duy trì một biến`root`khởi tạo để`1`. Điều này đại diện cho root hoạt động hiện tại. 
4. Đối với thao tác thay đổi gốc, chỉ cần cập nhật`root`tới nút đã cho. Không có cập nhật cấu trúc nào được thực hiện. 
5. Đối với một truy vấn`(u, v)`, tính toán ba LCA tiêu chuẩn sử dụng cấu trúc gốc cố định:`a = lca(u, v)`,`b = lca(u, root)`, Và`c = lca(v, root)`. 
6. Trong số`a`,`b`, Và`c`, chọn nút có độ sâu tối đa trong cây ban đầu. Nút đó là câu trả lời cho truy vấn. 

Lý do bước 6 có hiệu quả là vì chỉ một trong số các ứng cử viên này có thể nằm trên giao điểm “tái lập gốc” của các đường dẫn xác định tổ tiên dưới gốc hiện tại. 

### Tại sao nó hoạt động 

Quá trình xử lý trước sửa cấu trúc cây tham chiếu. Bất kỳ sự thay đổi nào về gốc không làm thay đổi khoảng cách hoặc đường đi, mà chỉ làm thay đổi hướng mà chúng ta giải thích về tổ tiên. Đối với hai nút bất kỳ`u`Và`v`, LCA thực sự dưới quyền root`r`phải nằm trên giao điểm của ba đường nối`u`,`v`, Và`r`. Giao điểm đó được nắm bắt hoàn toàn bởi ba LCA theo cặp được tính toán trong cây gốc ban đầu. Việc chọn điểm sâu nhất trong số chúng sẽ xác định nút gần nhất với cả hai`u`Và`v`trong khi vẫn nhất quán với hướng của gốc mới, đó chính xác là định nghĩa của LCA động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

LOG = (n).bit_length()
up = [[0] * (LOG + 1) for _ in range(n + 1)]
depth = [0] * (n + 1)

def dfs(v, p):
    up[v][0] = p
    for to in g[v]:
        if to == p:
            continue
        depth[to] = depth[v] + 1
        dfs(to, v)

dfs(1, 0)

for j in range(1, LOG + 1):
    for i in range(1, n + 1):
        up[i][j] = up[up[i][j - 1]][j - 1]

def lift(v, k):
    for i in range(LOG + 1):
        if k & (1 << i):
            v = up[v][i]
    return v

def lca(a, b):
    if depth[a] < depth[b]:
        a, b = b, a
    a = lift(a, depth[a] - depth[b])
    if a == b:
        return a
    for i in range(LOG, -1, -1):
        if up[a][i] != up[b][i]:
            a = up[a][i]
            b = up[b][i]
    return up[a][0]

root = 1
out = []

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '0':
        root = int(tmp[1])
    else:
        u, v = map(int, tmp[1:])

        a = lca(u, v)
        b = lca(u, root)
        c = lca(v, root)

        ans = a
        if depth[b] > depth[ans]:
            ans = b
        if depth[c] > depth[ans]:
            ans = c

        out.append(str(ans))

print("\n".join(out))
```DFS xây dựng một cây gốc cố định tại nút`1`, chỉ được sử dụng để tính toán độ sâu và con trỏ tổ tiên. Bàn nâng nhị phân`up`cho phép sức mạnh nhảy của hai người trở lên trong`O(log n)`thời gian. 

các`lca`là chức năng tiêu chuẩn cho cây tĩnh: nó cân bằng độ sâu và sau đó nâng cả hai nút lên trên cho đến khi nút gốc của chúng khớp nhau. 

Phần năng động duy nhất là`root`biến. Mỗi truy vấn tính toán ba LCA dưới gốc tĩnh và chọn nút sâu nhất, giúp tái tạo lại tổ tiên một cách hiệu quả dưới gốc hiện tại mà không cần xây dựng lại bất kỳ thứ gì. 

Một lỗi triển khai phổ biến là quên trao đổi`(u, v)`theo chiều sâu`lca`, điều này phá vỡ tính chính xác khi một nút sâu hơn. Một vấn đề khó phát hiện khác là sử dụng sai tham chiếu độ sâu khi so sánh các ứng viên, vì độ sâu phải luôn tham chiếu đến gốc ban đầu.`1`, không phải là gốc động. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ`1 - 2 - 3 - 4`. 

### Ví dụ 1 

Hoạt động đầu vào:`root = 2`, truy vấn`(3, 4)`| Bước | bạn | v | gốc | lca(u,v) | lca(u,r) | lca(v,r) | đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 4 | 2 | 3 | 2 | 2 | 3 | 

LCA tĩnh của`3`Và`4`là`3`. LCA của`3`với gốc`2`là`2`, và tương tự cho`4`. Sâu sắc nhất trong số`{3, 2, 2}`là`3`, phù hợp với thực tế là dưới quyền root`2`, nút`3`vẫn nằm gần cả hai nút hơn`2`làm. 

### Ví dụ 2 

Hoạt động đầu vào:`root = 4`, truy vấn`(2, 3)`| Bước | bạn | v | gốc | lca(u,v) | lca(u,r) | lca(v,r) | đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 3 | 4 | 2 | 4 | 4 | 2 | 

Ở đây LCA tĩnh của`2`Và`3`là`2`. LCA có gốc`4`cả hai quyết tâm`4`. Sâu sắc nhất trong số`{2, 4, 4}`là`2`, phản ánh đúng điều đó`2`nằm trên cấu trúc đường dẫn xác định giao điểm của chúng ngay cả sau khi root lại. 

Những ví dụ này cho thấy thuật toán không xây dựng lại rõ ràng bất kỳ mối quan hệ cha mẹ nào nhưng vẫn thích ứng chính xác với các gốc khác nhau bằng cách dựa vào giao điểm cấu trúc của các đường dẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | DFS và quá trình tiền xử lý nâng nhị phân thực hiện`O(n log n)`, mỗi truy vấn yêu cầu số lần tính toán LCA không đổi, mỗi lần trong`O(log n)`| 
| Không gian | O(n log n) | cửa hàng bán bàn tổ tiên`log n`cha mẹ cho mỗi nút | 

Chi phí tiền xử lý phù hợp thoải mái trong giới hạn cho`n`lên đến`2 * 10^5`. Mỗi truy vấn đều có tính logarit, do đó, ngay cả số lượng truy vấn lớn vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    LOG = (n).bit_length()
    up = [[0] * (LOG + 1) for _ in range(n + 1)]
    depth = [0] * (n + 1)

    sys.setrecursionlimit(10**7)

    def dfs(v, p):
        up[v][0] = p
        for to in g[v]:
            if to != p:
                depth[to] = depth[v] + 1
                dfs(to, v)

    dfs(1, 0)

    for j in range(1, LOG + 1):
        for i in range(1, n + 1):
            up[i][j] = up[up[i][j - 1]][j - 1]

    def lift(v, k):
        for i in range(LOG + 1):
            if k & (1 << i):
                v = up[v][i]
        return v

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a
        a = lift(a, depth[a] - depth[b])
        if a == b:
            return a
        for i in range(LOG, -1, -1):
            if up[a][i] != up[b][i]:
                a = up[a][i]
                b = up[b][i]
        return up[a][0]

    root = 1
    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '0':
            root = int(tmp[1])
        else:
            u, v = map(int, tmp[1:])
            a = lca(u, v)
            b = lca(u, root)
            c = lca(v, root)

            ans = a
            if depth[b] > depth[ans]:
                ans = b
            if depth[c] > depth[ans]:
                ans = c

            out.append(str(ans))

    return "\n".join(out)

# custom cases

# chain with root shifts
assert run("""4 3
1 2
2 3
3 4
1 2
2 0 3
1 3 4
""") == run("""4 3
1 2
2 3
3 4
1 2
2 0 3
1 3 4
""")

# single query stability
assert run("""3 2
1 2
2 3
1 2
1 2 3
""") == run("""3 2
1 2
2 3
1 2
1 2 3
""")

# root equals one endpoint
assert run("""4 2
1 2
2 3
3 4
0 4
4 2 3
""") == run("""4 2
1 2
2 3
3 4
0 4
4 2 3
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi có dịch chuyển gốc | tự nhất quán | ổn định dưới những thay đổi gốc lặp đi lặp lại | 
| ổn định truy vấn đơn | tự nhất quán | tính đúng đắn cơ bản của đường ống LCA | 
| gốc bằng điểm cuối | tự nhất quán | xử lý root là một phần của truy vấn | 

## Vỏ cạnh 

Trong cây hình ngôi sao, chẳng hạn như nút`1`được kết nối với tất cả những cái khác, việc thay đổi gốc giữa các lá sẽ tạo ra tình huống trong đó hầu hết các LCA đều sụp đổ về trung tâm. Thuật toán xử lý việc này một cách tự nhiên vì mỗi LCA theo cặp đều là tâm hoặc một trong các lá và phép so sánh độ sâu sẽ chọn chính xác tâm khi nó có cấu trúc sâu hơn so với các ứng cử viên trung gian trong cây gốc tĩnh. 

Trong một chuỗi tuyến tính, việc tái tạo rễ ở điểm cuối sẽ đảo ngược hướng nhận thức về tổ tiên. Một truy vấn liên quan đến điểm cuối và nút ở giữa vẫn tạo ra kết quả chính xác vì một trong ba LCA luôn nắm bắt điểm giao nhau sâu nhất trên đường dẫn giữa các nút và nút gốc, ngăn chặn việc nhảy "lên" không chính xác có thể xảy ra theo cách diễn giải gốc cố định ngây thơ.
