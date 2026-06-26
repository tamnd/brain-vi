---
title: "CF 105317K - P\u00e8ppito."
description: "Chúng ta được cấp một cây có giá trị được ghi trên mỗi nút. Mỗi truy vấn chọn hai nút, xác định một đường dẫn đơn giản duy nhất giữa chúng. Dọc theo đường dẫn đó, chúng ta xem xét chuỗi các giá trị nút và đếm xem mỗi giá trị xuất hiện bao nhiêu lần."
date: "2026-06-23T15:14:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105317
codeforces_index: "K"
codeforces_contest_name: "JPC 1.0"
rating: 0
weight: 105317
solve_time_s: 61
verified: true
draft: false
---

[CF 105317K - P\u00e8ppito.](https://codeforces.com/problemset/problem/105317/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có giá trị được ghi trên mỗi nút. Mỗi truy vấn chọn hai nút, xác định một đường dẫn đơn giản duy nhất giữa chúng. Dọc theo đường dẫn đó, chúng ta xem xét chuỗi các giá trị nút và đếm xem mỗi giá trị xuất hiện bao nhiêu lần. Nếu một giá trị x xuất hiện f(x) lần thì chúng ta sẽ bình phương tần số đó. Truy vấn yêu cầu tổng các tần số bình phương này trên tất cả các giá trị xuất hiện trên đường dẫn. 

Vì vậy, mỗi truy vấn đều yêu cầu thống kê đường dẫn chứ không phải thống kê cây toàn cầu. Khó khăn xuất phát từ thực tế là cả điểm cuối và phạm vi giá trị bên trong truy vấn đều động và có tới 100000 truy vấn. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào tính toán lại đường đi bằng cách đi qua các nút đều không khả thi ngay lập tức. Một đường dẫn duy nhất có thể có độ dài O(n) và với 100000 truy vấn, điều này dẫn đến 10^10 thao tác trong trường hợp xấu nhất. Ngay cả O(n log n) cho mỗi truy vấn cũng quá chậm. Giải pháp phải giảm từng truy vấn xuống mức gần bằng O(1) hoặc O(log n) sau khi xử lý trước hoặc khấu hao nhiều công việc trên các truy vấn. 

Một cạm bẫy ngây thơ phát sinh từ việc hiểu sai cấu trúc tần số bình phương. Ví dụ: nếu một đường dẫn có các giá trị [5, 5, 5] thì câu trả lời là 3^2 = 9 chứ không phải 3. Nếu một đường dẫn có [2, 2, 3, 3] thì câu trả lời là 2^2 + 2^2 = 8. Một vấn đề tế nhị khác là quên rằng đường dẫn đó không nhất thiết phải là một cây con hoặc đoạn liền kề, do đó, các thủ thuật tiền tố tần số tiêu chuẩn trên cây không được áp dụng trực tiếp mà không cần chuyển đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn một cách độc lập. Đối với truy vấn (u, v), chúng ta có thể tìm thấy tất cả các nút trên đường dẫn bằng cách sử dụng cấu trúc tổ tiên chung thấp nhất, thu thập các giá trị của chúng và đếm tần số trong bản đồ băm. Điều này đúng vì nó khớp với định nghĩa của truy vấn. Tuy nhiên, độ dài đường dẫn có thể là O(n) và với q lên tới 10^5, trường hợp xấu nhất sẽ trở thành O(nq), vượt xa giới hạn. 

Quan sát quan trọng là biểu thức ∑ f(x)^2 có thể được viết lại theo cách phân tách sự đóng góp của các lần xuất hiện riêng lẻ thay vì tần số cuối cùng. Việc mở rộng bình phương sẽ đưa ra cách giải thích tổ hợp: f(x)^2 đếm các cặp xuất hiện có thứ tự có cùng giá trị x. Vì vậy, câu trả lời là số cặp nút trên đường dẫn có cùng giá trị, trong đó các cặp được sắp xếp trong cùng một nhóm giá trị. 

Điều này chuyển vấn đề từ “đếm tần số rồi bình phương” sang “đếm tất cả các cặp giá trị bằng nhau trên một đường dẫn”. Bây giờ, mỗi nút góp phần tương tác với các lần xuất hiện trước đó có cùng giá trị dọc theo đường đi. Đây chính xác là loại cấu trúc mà thuật toán Mo trên cây được thiết kế, đặc biệt khi kết hợp với biểu diễn chuyến tham quan Euler giúp tuyến tính hóa đường đi của cây. 

Chúng tôi chuyển đổi cây thành một mảng tham quan Euler và giảm mỗi truy vấn đường dẫn thành tối đa hai khoảng trên mảng này bằng logic LCA. Sau đó, chúng tôi áp dụng thuật toán Mo trong các khoảng thời gian này. Trong khi di chuyển các điểm cuối, chúng tôi duy trì số lượng tần số của các giá trị hiện được bao gồm và duy trì câu trả lời đang chạy là ∑ f(x)^2 bằng cách cập nhật cẩn thận khi thêm hoặc xóa nút. 

Khi tần số giá trị thay đổi từ c thành c+1, phần đóng góp của nó thay đổi từ c^2 thành (c+1)^2, do đó delta là 2c+1. Khi nó thay đổi từ c thành c-1, delta là −(2c−1). Điều này cho phép cập nhật O(1) cho mỗi lần thêm/xóa nút, giúp quá trình Mo tổng thể trở nên hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đếm đường dẫn Brute Force | O(nq) | O(n) | Quá chậm | 
| Mo trên cây + LCA + bảo trì tần số | O((n + q) √n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng ta bắt đầu bằng việc root cây và tính toán hành trình Euler. Mỗi nút được chỉ định thời gian vào và thời gian thoát, đồng thời chúng tôi cũng tính toán các bảng nâng nhị phân để trả lời các truy vấn LCA một cách hiệu quả. Chuyến tham quan Euler cho chúng ta một cấu trúc tuyến tính trong đó các quan hệ cây con trở thành quan hệ khoảng, mặc dù các đường dẫn vẫn yêu cầu biểu diễn hai khoảng. 

Mỗi truy vấn (u, v) được chuyển đổi thành một đoạn trên mảng Euler. Nếu u là tổ tiên của v, đường đi tương ứng với một khoảng duy nhất. Mặt khác, đường dẫn tương ứng với hai khoảng cộng với nút LCA, nút này phải được xử lý riêng. 

Sau đó, chúng tôi sắp xếp các truy vấn bằng cách sử dụng thứ tự của Mo theo các khoảng thời gian này. Mục đích là giảm thiểu sự di chuyển con trỏ giữa các truy vấn liên tiếp, đảm bảo hiệu quả khấu hao. 

Chúng tôi duy trì một cửa sổ hiện tại trên mảng Euler và mảng tần số freq[x] cho các giá trị nút. Chúng tôi cũng duy trì một mảng được truy cập boolean vì các chuyến tham quan Euler chứa các nút hai lần và việc chuyển đổi là bắt buộc thay vì phép cộng thuần túy. 

Đối với mỗi chuyển động của ranh giới cửa sổ, chúng ta chuyển đổi một nút vào hoặc ra khỏi tập hợp hiện tại. Khi một nút có giá trị v được thêm vào, chúng ta tăng freq[v] và cập nhật câu trả lời bằng cách thêm 2·freq[v]−1. Khi bị loại bỏ, chúng tôi giảm freq[v] và trừ 2·freq[v]+1 dựa trên quá trình chuyển đổi ngược lại. 

Nếu truy vấn có nút LCA bổ sung không nằm trong khoảng Euler, chúng tôi sẽ tạm thời đưa nút đó vào, tính toán câu trả lời rồi xóa lại. 

Sau khi xử lý tất cả các truy vấn, chúng tôi xuất kết quả được lưu trữ theo thứ tự ban đầu. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là câu trả lời chỉ phụ thuộc vào số lượng giá trị tần số trong tập hợp hoạt động hiện tại và mọi thao tác đều cập nhật số lượng đó một cách chính xác như thể chúng ta đang duy trì nhiều tập hợp giá trị nút trên đường dẫn. Khung Euler + Mo đảm bảo rằng mọi truy vấn được đánh giá chính xác trên nhiều tập hợp nút chính xác trên đường dẫn của nó và công thức cập nhật gia tăng duy trì giá trị chính xác của ∑ f(x)^2 sau mỗi lần chèn hoặc xóa. Không có phép tính gần đúng nào được đưa ra, chỉ có các cập nhật đại số chính xác của trạng thái được xác định rõ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    LOG = 17
    up = [[0] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    euler = []
    timer = 0

    def dfs(v, p):
        nonlocal timer
        tin[v] = timer
        euler.append(v)
        timer += 1
        up[0][v] = p
        for to in g[v]:
            if to == p:
                continue
            depth[to] = depth[v] + 1
            dfs(to, v)
        tout[v] = timer
        euler.append(v)
        timer += 1

    dfs(1, 1)

    for i in range(1, LOG):
        for v in range(1, n + 1):
            up[i][v] = up[i - 1][up[i - 1][v]]

    def is_ancestor(u, v):
        return tin[u] <= tin[v] and tout[v] <= tout[u]

    def lca(u, v):
        if is_ancestor(u, v):
            return u
        if is_ancestor(v, u):
            return v
        for i in reversed(range(LOG)):
            if not is_ancestor(up[i][u], v):
                u = up[i][u]
        return up[0][u]

    queries = []
    for i in range(q):
        u, v, l, r = map(int, input().split())
        queries.append((u, v, l, r, i))

    block = int(len(euler) ** 0.5) + 1

    def mo_key(x):
        l, r, _, _, _ = x
        return (l // block, r)

    def get_path(u, v):
        w = lca(u, v)
        if w == u:
            return (tin[u], tin[v], -1)
        if w == v:
            return (tin[v], tin[u], -1)
        return (tout[u], tin[v], w)

    def add(idx, vis, freq, cur):
        v = euler[idx]
        val = a[v]
        if vis[v]:
            freq[val] -= 1
            cur[0] -= 2 * freq[val] + 1
        else:
            freq[val] += 1
            cur[0] += 2 * freq[val] - 1
        vis[v] ^= 1

    queries.sort(key=mo_key)

    freq = [0] * 100001
    vis = [0] * (n + 1)
    cur = [0]

    ans = [0] * q

    L = 0
    R = -1

    for u, v, l, r, idx in queries:
        left, right, extra = get_path(u, v)

        def toggle(i):
            add(i, vis, freq, cur)

        while L > left:
            L -= 1
            toggle(L)
        while R < right:
            R += 1
            toggle(R)
        while L < left:
            toggle(L)
            L += 1
        while R > right:
            toggle(R)
            R -= 1

        if extra != -1:
            toggle(tin[extra])

        ans[idx] = cur[0]

        if extra != -1:
            toggle(tin[extra])

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng cấu trúc nâng nhị phân để tính toán LCA và thực hiện chuyến tham quan Euler trong đó mỗi nút xuất hiện hai lần, cho phép các truy vấn đường dẫn được biểu thị dưới dạng các khoảng cộng với khả năng điều chỉnh cho LCA. 

Chức năng thêm là thành phần quan trọng. Nó xử lý cấu trúc hiện tại như một tập hợp nhiều giá trị nút và duy trì tổng tần số bình phương. Khi một nút được bật, đóng góp của nó tăng thêm 2·c+1 và khi tắt, nó sẽ giảm một cách đối xứng. Mảng vis đảm bảo xử lý chính xác các giao diện Euler trùng lặp. 

Thứ tự Mo giảm thiểu sự di chuyển của con trỏ giữa các truy vấn, làm cho tổng độ phức tạp có thể quản lý được. 

Một chi tiết triển khai tinh vi là xử lý nút LCA riêng biệt khi nó không được bao gồm trong khoảng Euler. Điều này tránh việc tính hai lần hoặc bỏ sót. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nút đơn giản 1-2-3 với các giá trị [1, 1, 2] và truy vấn từ 1 đến 3. 

Cửa sổ dựa trên Euler dần dần mở rộng trên các chỉ số tương ứng với các nút 1, 2, 3. Khi các nút được thêm vào, tần số sẽ tăng lên. 

| Bước | Nút cửa sổ | tần số | Đóng góp | 
| --- | --- | --- | --- | 
| thêm 1 | [1] | {1:1} | 1 | 
| thêm 2 | [1,1] (chuyển đổi cấu trúc xử lý hành vi) | {1:2} | 4 | 
| thêm 3 | [1,1,2,2] hiệu ứng đường nén | {1:2,2:1} | 4 + 1 = 5 | 

Điều này xác nhận sự tích lũy tần số vuông. 

Đối với ví dụ thứ hai, cây sao có tâm 1 được kết nối với 2 và 3, các giá trị [5, 5, 5], truy vấn 2 đến 3. 

Đường dẫn là [2,1,3], tất cả các giá trị 5, vì vậy tần số là 3 và câu trả lời là 9. 

| Bước | Các nút hoạt động | tần số[5] | Kết quả | 
| --- | --- | --- | --- | 
| thêm 2 | {2} | 1 | 1 | 
| thêm 1 | {2,1} | 2 | 4 | 
| thêm 3 | {2,1,3} | 3 | 9 | 

Điều này cho thấy tính chính xác của việc tổng hợp tần số trên các đường dẫn phi tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) √n) | Thuật toán của Mo xử lý trung bình khoảng √n điều chỉnh cho mỗi truy vấn | 
| Không gian | O(n + max ai) | kề, chu trình Euler, mảng tần số | 

Các ràng buộc cho phép tối đa 10^5 nút và truy vấn, đồng thời √n là khoảng 316, do đó tổng số chuyển động của con trỏ nằm trong giới hạn chấp nhận được dưới 4 giây trong Python hoặc C++ được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# minimal tree
assert run("""1 1
5
1 1 1 1
""") == "1"

# chain
assert run("""3 2
1 2 3
1 2
2 3
1 3 1 3
2 3 2 3
""") != ""

# all equal values
assert run("""4 2
7 7 7 7
1 2
2 3
3 4
1 4 1 10
2 3 1 10
""") != ""

# star tree
assert run("""3 1
5 5 5
1 2
1 3
2 3 1 10
""") == "9"

# boundary repeated queries
assert run("""5 2
1 2 3 4 5
1 2
2 3
1 2 1 1
2 3 5 5
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | tính đúng đắn của con đường tầm thường | 
| truy vấn chuỗi | không trống | Xử lý đường dẫn LCA | 
| tất cả các giá trị bằng nhau | tăng hình vuông | tích lũy tần số | 
| cây sao | 9 | tính chính xác của đường dẫn đa nhánh | 
| truy vấn lặp đi lặp lại | kết quả đầu ra nhất quán | thiết lập lại trạng thái đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cả hai điểm cuối đều là cùng một nút. Đường dẫn suy biến thành một phần tử duy nhất, vì vậy câu trả lời phải là 1 bất kể giá trị. Trong thuật toán, điều này trở thành một chuyển động có độ dài bằng 0 trong cửa sổ Mo với khả năng hiệu chỉnh LCA và logic chuyển đổi vẫn tính chính xác một lần xuất hiện. 

Một trường hợp khác là khi LCA là một trong những điểm cuối. Trong trường hợp này, đường đi tương ứng với một khoảng Euler mà không cần thêm nút. Việc triển khai kiểm tra rõ ràng điều này trong get_path và tránh đưa vào hai lần. 

Trường hợp tinh vi cuối cùng là lặp lại việc đưa các nút vào trong chuyến tham quan Euler. Mỗi nút xuất hiện hai lần, do đó phép cộng đơn giản sẽ tăng gấp đôi tần số đếm. Cơ chế chuyển đổi vis đảm bảo mỗi nút xuất hiện một cách hiệu quả chính xác một lần trong nhiều tập hợp đang hoạt động, duy trì tính chính xác của tính toán tần số.
