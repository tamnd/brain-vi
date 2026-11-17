---
title: "CF 1949C - Kiến tụ tập hàng năm"
description: "Mỗi đỉnh của cây ban đầu chứa đúng một con kiến. Một nước đi sẽ chọn một cạnh $(u,v)$ và ra lệnh cho tất cả các con kiến ​​hiện đang tập trung tại $u$ di chuyển đến $v$. Kiến chỉ tuân theo khi đích đến đã có ít nhất số kiến ​​bằng số kiến ​​ở nguồn."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1949
codeforces_index: "C"
codeforces_contest_name: "European Championship 2024 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1900
weight: 1949
solve_time_s: 288
verified: true
draft: false
---

[CF 1949C - Tập hợp kiến hàng năm](https://codeforces.com/problemset/problem/1949/C) 

**Xếp hạng:** 1900 
**Tags:** dfs và tương tự, dp, tham lam, cây cối 
**Thời gian giải:** 4 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đỉnh của cây ban đầu chứa đúng một con kiến. 

Một nước đi chọn một cạnh$(u,v)$và ra lệnh cho tất cả những con kiến ​​hiện đang tập trung tại$u$di chuyển đến$v$. Kiến chỉ tuân theo khi đích đến đã có ít nhất số kiến ​​bằng số kiến ​​ở nguồn. Nếu việc di chuyển xảy ra, nhóm tại$u$biến mất và kiến ​​của nó được thêm vào$v$. 

Câu hỏi đặt ra là liệu có tồn tại một chuỗi các nước đi hợp lệ nào đó mà cuối cùng tập hợp được tất cả$n$kiến vào một đỉnh duy nhất. 

Đầu vào là một cây có tối đa$200\,000$đỉnh. Một bậc hai hoặc thậm chí$O(n \sqrt n)$giải pháp đã nguy hiểm ở quy mô này. Chúng ta cần thứ gì đó gần với thời gian tuyến tính, có thể có hệ số logarit đến từ việc sắp xếp. 

Phần khó khăn là tính khả thi không chỉ được xác định bởi kích thước cây con. Thứ tự hợp nhất quan trọng. Một nút có thể có khả năng hấp thụ một số thành phần lân cận nếu chúng được xử lý từ nhỏ nhất đến lớn nhất, nhưng sẽ thất bại nếu thành phần lớn phải được hấp thụ quá sớm. 

Một sai lầm phổ biến là chỉ nhìn vào kích thước cuối cùng của các bộ phận. Coi như:```
1 - 2 - 3 - 4
```Nếu chúng ta cố gắng tập hợp mọi thứ ở đỉnh 2 thì thành phần bên phải có kích thước 2. Đỉnh 2 bắt đầu chỉ với một con kiến ​​nên nó không thể hấp thụ thành phần đó ngay lập tức. Chiến lược đúng đắn là trước tiên hấp thụ đỉnh 1, tăng số lượng ở đỉnh 2 lên 2 và chỉ sau đó hấp thụ thành phần cỡ 2. Bất kỳ giải pháp nào bỏ qua thứ tự hợp nhất đều đưa ra câu trả lời sai. 

Một trường hợp tế nhị khác là một ngôi sao:```
    2
    |
3 - 1 - 4
```Câu trả lời là`YES`. Đỉnh 1 có thể hấp thụ từng lá một vì mỗi lá đều có kích thước 1. Một quy tắc ngây thơ như "trung tâm ít nhất phải lớn bằng mọi thành phần lân cận" sẽ bác bỏ trường hợp này một cách sai lầm. 

Đầu vào nhỏ nhất cũng đáng để kiểm tra:```
1
```Câu trả lời là`YES`, bởi vì tất cả kiến ​​đã được tập hợp lại. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi đỉnh làm điểm gặp nhau và mô phỏng rõ ràng tất cả các lệnh hợp nhất có thể có. Đối với một gốc cố định, mọi thành phần lân cận chỉ có thể được hợp nhất sau khi nó đã được tập hợp bên trong, vì vậy chúng ta cần khám phá nhiều thứ tự có thể có. Số lượng các trạng thái tăng theo cấp số nhân và trở nên hoàn toàn không khả thi từ rất lâu trước khi$n=200000$. 

Quan sát quan trọng là khi một thành phần đã được tập hợp thành công vào đỉnh đính kèm của nó, chỉ có một số quan trọng: kích thước của nó. 

Giả sử một đỉnh muốn hấp thụ các thành phần lân cận có kích thước$$s_1,s_2,\dots,s_k.$$Đỉnh bắt đầu bằng một con kiến. Nếu chúng ta xử lý các thành phần theo thứ tự tăng dần thì thứ tự đó luôn là tối ưu. Hấp thụ một thành phần nhỏ hơn sớm hơn chỉ có thể giúp ích. 

Sau khi sắp xếp, chọn kích thước$$s_1 \le s_2 \le \dots \le s_k.$$Việc hợp nhất thành công nếu trước mỗi lần hấp thụ số lượng kiến ​​hiện tại đủ lớn. 

Nếu số tiền hiện tại trước khi xử lý$s_i$là$$1 + \sum_{j<i} s_j,$$thì chúng ta cần$$s_i \le 1 + \sum_{j<i} s_j.$$Điều kiện này đặc trưng hoàn toàn cho dù một đỉnh có thể tập hợp tất cả các thành phần liền kề hay không. 

Bây giờ hãy xem mọi cạnh có hướng$u \to v$. Loại bỏ cạnh. Bên chứa$u$là một thành phần Xác định trạng thái DP có hướng: 

Trạng thái hợp lệ nếu toàn bộ thành phần đó có thể tập hợp thành$u$. 

Hiệu lực của$u \to v$chỉ phụ thuộc vào tính hợp lệ và kích thước của các thành phần lân cận được gắn vào$u$, không kể cạnh chứa$v$. 

Điều này biến vấn đề thành việc DP phải root lại cây. 

Thách thức còn lại là kiểm tra điều kiện kích thước được sắp xếp một cách hiệu quả đối với mọi hàng xóm bị loại trừ có thể. Việc tính toán lại trực tiếp cho mọi cạnh sẽ là bậc hai. Việc tối ưu hóa quan trọng là tính toán trước, đối với mỗi đỉnh, hiệu quả của việc loại bỏ bất kỳ đỉnh nào khỏi danh sách đã sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Reroot tối ưu DP |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán trước kích thước thành phần 

Đối với mỗi cạnh có hướng, chúng ta cần kích thước của thành phần ở cạnh đó sau khi loại bỏ cạnh đó. 

Gốc cây ở đỉnh 1. 

Đối với một đứa trẻ$v$của$u$, kích thước thành phần của$v \to u$đơn giản là`sub[v]`. 

Đối với hướng ngược lại$u \to v$, kích thước là`n - sub[v]`. 

### Tính toán tính khả thi của việc hợp nhất từ các kích thước 

Đối với một chuỗi được sắp xếp$$b_1 \le b_2 \le \dots \le b_m,$$định nghĩa$$d_i = b_i - \sum_{j<i} b_j.$$Điều kiện hợp nhất tương đương với$$\max_i d_i \le 1.$$Điều này xuất phát trực tiếp từ việc yêu cầu số lượng kiến ​​hiện tại phải đủ lớn trước mỗi lần hấp thụ. 

### Loại bỏ một hàng xóm một cách hiệu quả 

Đối với một đỉnh, hãy sắp xếp các kích thước thành phần lân cận. 

Nếu chúng ta loại bỏ phần tử ở vị trí$k$, thì mỗi tổng tiền tố sau đó giảm đi$b_k$. 

Đối với các chỉ số sau$k$,$$d_i' = d_i + b_k.$$Sử dụng tiền tố và hậu tố cực đại của bản gốc$d_i$, chúng ta có thể tính toán trong$O(1)$liệu điều kiện hợp nhất có còn hiệu lực hay không sau khi loại trừ bất kỳ hàng xóm cụ thể nào. 

### DP từ dưới lên 

1. Lấy gốc cây ở đỉnh 1 và tính kích thước cây con. 
2. Đối với mỗi đỉnh, hãy sắp xếp kích thước của tất cả các thành phần sự cố và tính toán trước: 

1. Liệu tất cả các hàng xóm có thể được hợp nhất hay không. 
2. Liệu tất cả các hàng xóm ngoại trừ một hàng xóm cụ thể có thể được hợp nhất hay không. 
3. Xử lý các đỉnh theo thứ tự DFS ngược. 
4. Hãy để`down[u]`biểu thị tính hợp lệ của trạng thái được chỉ đạo$u \to parent(u)$. 
5.`down[u]`là đúng khi: 

1. Mọi trạng thái trẻ em`down[child]`là đúng. 
2. Điều kiện kích thước được thỏa mãn sau khi loại trừ phía cha. 

### DP từ trên xuống 

1. Hãy để`up[u]`biểu thị tính hợp lệ của$parent(u) \to u$. 
2. Đi qua các đỉnh từ gốc trở xuống. 
3. Đối với mỗi đỉnh, hãy đếm xem có bao nhiêu trạng thái được định hướng đến không hợp lệ. 
4. Đỉnh có thể là điểm tập hợp cuối cùng nếu: 

1. Tất cả các trạng thái đến đều hợp lệ. 
2. Điều kiện kích thước sử dụng tất cả các thành phần lân cận là hợp lệ. 
5. Để tính toán`up[child]`, loại trừ trạng thái đến của thành phần con đó khỏi số lượng không hợp lệ và sử dụng kết quả điều kiện-kích thước được tính toán trước để loại trừ thành phần con đó. 

### Tại sao nó hoạt động 

Đối với mọi trạng thái cạnh có hướng, DP thể hiện chính xác câu lệnh: 

"Toàn bộ thành phần ở phía bên này của cạnh có thể tập hợp lại thành điểm cuối." 

Một thành phần chỉ có thể tập hợp thành một đỉnh nếu mọi thành phần lân cận gắn với đỉnh đó trước tiên có thể tập hợp lại thành điểm đính kèm của chính nó. Đó chính xác là những gì trạng thái DP đến mã hóa. 

Sau khi các thành phần lân cận đó có sẵn, chỉ có kích thước của chúng là quan trọng. Điều kiện kích thước được sắp xếp vừa cần vừa đủ để đỉnh có thể hấp thụ tất cả. 

Quá trình từ dưới lên tính toán tất cả các trạng thái hướng tới gốc. Đường chuyền từ trên xuống cung cấp thông tin phía cha mẹ còn thiếu. Sau cả hai lần vượt qua, mọi trạng thái cạnh có hướng đều được đánh giá chính xác. 

Một đỉnh là một điểm gặp nhau hợp lệ một cách chính xác khi mọi thành phần sự cố có thể tập trung về phía nó và nó có thể hấp thụ tất cả chúng. Thuật toán kiểm tra chính xác điều kiện đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    adj = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    if n == 1:
        print("YES")
        return

    parent = [0] * (n + 1)
    parent[1] = -1

    order = [1]
    for u in order:
        for v in adj[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    sub = [1] * (n + 1)
    for u in reversed(order[1:]):
        sub[parent[u]] += sub[u]

    good_all = [False] * (n + 1)
    good_excl = [dict() for _ in range(n + 1)]

    for u in range(1, n + 1):
        items = []

        for v in adj[u]:
            if parent[v] == u:
                sz = sub[v]
            else:
                sz = n - sub[u]
            items.append((sz, v))

        m = len(items)

        if m == 0:
            good_all[u] = True
            continue

        items.sort()

        b = [x for x, _ in items]

        d = [0] * m
        pref = 0
        for i in range(m):
            d[i] = b[i] - pref
            pref += b[i]

        pref_max = [0] * m
        suff_max = [0] * m

        pref_max[0] = d[0]
        for i in range(1, m):
            pref_max[i] = max(pref_max[i - 1], d[i])

        suff_max[m - 1] = d[m - 1]
        for i in range(m - 2, -1, -1):
            suff_max[i] = max(suff_max[i + 1], d[i])

        good_all[u] = pref_max[-1] <= 1

        for i, (_, v) in enumerate(items):
            left = pref_max[i - 1] if i > 0 else 0
            right = suff_max[i + 1] + b[i] if i + 1 < m else 0
            need = max(left, right)
            good_excl[u][v] = (need <= 1)

    down = [True] * (n + 1)

    for u in reversed(order[1:]):
        ok = True

        for v in adj[u]:
            if parent[v] == u and not down[v]:
                ok = False
                break

        down[u] = ok and good_excl[u][parent[u]]

    up = [True] * (n + 1)

    answer = False

    for u in order:
        invalid = 0

        if parent[u] != -1 and not up[u]:
            invalid += 1

        for v in adj[u]:
            if parent[v] == u and not down[v]:
                invalid += 1

        if invalid == 0 and good_all[u]:
            answer = True

        for v in adj[u]:
            if parent[v] == u:
                invalid_excluding_v = invalid - (0 if down[v] else 1)
                up[v] = (invalid_excluding_v == 0 and good_excl[u][v])

    print("YES" if answer else "NO")

solve()
```DFS đầu tiên tính toán các mối quan hệ cha và kích thước cây con. Những kích thước đó xác định kích thước của mọi thành phần cạnh có hướng. 

Giai đoạn tiếp theo xử lý từng đỉnh một cách độc lập. Sau khi sắp xếp các kích thước thành phần lân cận, nó sẽ xây dựng$d_i$mảng và tính toán trước điều kiện hợp nhất cho cả tập hàng xóm đầy đủ và cho mọi hàng xóm bị loại trừ có thể. 

các`down`DP đánh giá các trạng thái hướng về gốc. Một trạng thái chỉ hợp lệ khi tất cả các trạng thái con bắt buộc đều hợp lệ và điều kiện kích thước được giữ nguyên. 

các`up`DP cung cấp thông tin còn thiếu từ phía cha mẹ. Thay vì tính toán lại tính hợp lệ từ đầu cho mọi phần tử con, nó sử dụng các kết quả loại trừ được tính toán trước và số lượng trạng thái đến không hợp lệ. 

Việc triển khai không bao giờ sử dụng đệ quy, điều này tránh được các vấn đề về độ sâu đệ quy trên các cây có$200000$đỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
5 1
3 2
4 6
3 6
7 1
1 3
```Một điểm tập hợp hợp lệ là đỉnh 3. 

| Đỉnh | Kích thước thành phần đến | Kích thước được sắp xếp | Hợp nhất hợp lệ | 
| --- | --- | --- | --- | 
| 3 | 1, 2, 3 | 1, 2, 3 | Có | 
| 1 | 1, 1, 4 | 1, 1, 4 | Không | 
| 6 | 1, 4 | 1, 4 | Không | 

Tại đỉnh 3: 

| Bước | Kiến hiện tại | Thành phần được hấp thụ | 
| --- | --- | --- | 
| Bắt đầu | 1 | - | 
| 1 | 2 | cỡ 1 | 
| 2 | 4 | cỡ 2 | 
| 3 | 7 | cỡ 3 | 

Tất cả các thành phần đều có thể được hấp thụ nên câu trả lời là`YES`. 

Dấu vết này cho thấy tại sao việc xử lý theo thứ tự tăng dần là tối ưu. Mỗi lần hấp thụ thành công sẽ làm tăng khối lượng sẵn có cho những lần hợp nhất sau này. 

### Ví dụ 2 

Hãy xem xét:```
4
1 2
2 3
3 4
```| Đỉnh | Kích thước thành phần đến | Kích thước được sắp xếp | Hợp nhất hợp lệ | 
| --- | --- | --- | --- | 
| 1 | 3 | Không | | 
| 2 | 1, 2 | Có | | 
| 3 | 1, 2 | Có | | 
| 4 | 3 | Không | | 

DP cũng kiểm tra xem mặt cỡ 2 có thể tự tập hợp vào điểm gắn của nó hay không. Không thể, vì bên đó chứa thành phần có kích thước 2 không thể hình thành bắt đầu từ một con kiến. 

Câu trả lời cuối cùng là`NO`. 

Ví dụ này chứng tỏ rằng việc kiểm tra kích thước cục bộ là chưa đủ. Các thành phần lân cận phải có khả năng tập hợp được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi đỉnh sắp xếp kích thước thành phần sự cố của nó một lần | 
| Không gian |$O(n)$| Danh sách kề, kích thước cây con và mảng DP | 

Tổng của tất cả các độ là$2(n-1)$, do đó tổng lượng dữ liệu được xử lý là tuyến tính. Yếu tố siêu tuyến tính duy nhất đến từ việc sắp xếp các kích thước lân cận ở mỗi đỉnh, đưa ra kết quả tổng thể$O(n \log n)$phức tạp, phù hợp thoải mái với các giới hạn cho$n=200000$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    adj = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    if n == 1:
        return "YES\n"

    parent = [0] * (n + 1)
    parent[1] = -1

    order = [1]
    for u in order:
        for v in adj[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    sub = [1] * (n + 1)
    for u in reversed(order[1:]):
        sub[parent[u]] += sub[u]

    good_all = [False] * (n + 1)
    good_excl = [dict() for _ in range(n + 1)]

    for u in range(1, n + 1):
        items = []

        for v in adj[u]:
            if parent[v] == u:
                sz = sub[v]
            else:
                sz = len(parent) - 1 - sub[u]
            items.append((sz, v))

        m = len(items)

        if m == 0:
            good_all[u] = True
            continue

        items.sort()

        b = [x for x, _ in items]

        d = []
        pref = 0
        for s in b:
            d.append(s - pref)
            pref += s

        pm = [0] * m
        sm = [0] * m

        pm[0] = d[0]
        for i in range(1, m):
            pm[i] = max(pm[i - 1], d[i])

        sm[-1] = d[-1]
        for i in range(m - 2, -1, -1):
            sm[i] = max(sm[i + 1], d[i])

        good_all[u] = pm[-1] <= 1

        for i, (_, v) in enumerate(items):
            left = pm[i - 1] if i else 0
            right = sm[i + 1] + b[i] if i + 1 < m else 0
            good_excl[u][v] = max(left, right) <= 1

    down = [True] * (n + 1)

    for u in reversed(order[1:]):
        ok = True
        for v in adj[u]:
            if parent[v] == u and not down[v]:
                ok = False
        down[u] = ok and good_excl[u][parent[u]]

    up = [True] * (n + 1)

    ans = False

    for u in order:
        invalid = 0

        if parent[u] != -1 and not up[u]:
            invalid += 1

        for v in adj[u]:
            if parent[v] == u and not down[v]:
                invalid += 1

        if invalid == 0 and good_all[u]:
            ans = True

        for v in adj[u]:
            if parent[v] == u:
                invalid_ex = invalid - (0 if down[v] else 1)
                up[v] = invalid_ex == 0 and good_excl[u][v]

    return ("YES\n" if ans else "NO\n")

# minimum size
assert run("1\n") == "YES\n"

# star
assert run(
"""4
1 2
1 3
1 4
"""
) == "YES\n"

# chain of four
assert run(
"""4
1 2
2 3
3 4
"""
) == "NO\n"

# balanced binary tree
assert run(
"""7
1 2
1 3
2 4
2 5
3 6
3 7
"""
) == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | CÓ | Trường hợp ranh giới tối thiểu | 
| Ngôi sao 3 lá | CÓ | Sự hấp thụ lặp đi lặp lại của các thành phần cỡ 1 | 
| Đường đi có độ dài 4 | KHÔNG | Các thành phần không thể tập hợp nội bộ | 
| Cây nhị phân cân bằng | CÓ | Nhiều lần hợp nhất thành công ở các cấp độ | 

## Vỏ cạnh 

Xét cây nhỏ nhất:```
1
```Thuật toán không tính toán các trạng thái được định hướng.`good_all[1]`là đúng vì không có thành phần lân cận nào để hấp thụ. Câu trả lời là`YES`. 

Hãy xem xét một ngôi sao:```
4
1 2
1 3
1 4
```Kích thước thành phần lân cận của đỉnh 1 là$[1,1,1]$. Điều kiện được sắp xếp thành công vì mọi kích thước tối đa đều là khối lượng tích lũy hiện tại. Tất cả các trạng thái được định hướng từ lá tới tâm đều hợp lệ, do đó thuật toán đánh dấu đỉnh 1 là điểm tập hợp khả thi. 

Hãy xem xét một con đường:```
4
1 2
2 3
3 4
```Vertex 2 thấy kích thước thành phần$[1,2]$, thỏa mãn cục bộ điều kiện kích thước. Tuy nhiên, mặt cỡ 2 không thể hội tụ vào đỉnh đính kèm của nó. DP được chỉ đạo cho bên đó trở nên không hợp lệ và thuật toán sẽ từ chối cây một cách chính xác. 

Những trường hợp này minh họa tại sao giải pháp cần cả hai thành phần: điều kiện kích thước cục bộ và tính hợp lệ đệ quy của các thành phần lân cận.
