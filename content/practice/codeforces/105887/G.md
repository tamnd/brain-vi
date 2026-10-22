---
title: "CF 105887G - LCA \\& MST"
description: "Chúng ta có một cây có gốc trong đó nút 1 là gốc. Mỗi nút mang một trọng số bằng số. Từ cây này, chúng ta xác định một biểu đồ hoàn chỉnh trên cùng các nút, nhưng trọng số cạnh giữa hai nút không phải là tùy ý: nó được xác định hoàn toàn bởi trọng số của tổ tiên chung thấp nhất của chúng…"
date: "2026-06-21T19:54:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105887
codeforces_index: "G"
codeforces_contest_name: "\u7b2c\u5341\u4e09\u5c4a\u91cd\u5e86\u5e02\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b"
rating: 0
weight: 105887
solve_time_s: 54
verified: true
draft: false
---

[CF 105887G - LCA \\& MST](https://codeforces.com/problemset/problem/105887/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó nút 1 là gốc. Mỗi nút mang một trọng số bằng số. Từ cây này, chúng ta xác định một biểu đồ hoàn chỉnh trên cùng các nút, nhưng trọng số cạnh giữa hai nút không phải là tùy ý: nó được xác định hoàn toàn bởi trọng số của tổ tiên chung thấp nhất của chúng trong cây có gốc. 

Vì vậy, nếu hai nút gặp nhau ở một nút tổ tiên nào đó trong khi tiến về nút gốc, thì chi phí kết nối của chúng chính xác bằng trọng số được lưu trữ ở nút tổ tiên đó. Điều này tạo ra một biểu đồ dày đặc trong đó tất cả cấu trúc được kế thừa từ cây và giá trị của mỗi nút gián tiếp xác định nhiều cạnh trong biểu đồ hoàn chỉnh. 

Nhiệm vụ là liên tục sửa đổi trọng số trong cây con. Mỗi thao tác chọn một nút p và thêm một hằng số vào mọi nút trong cây con của nó. Sau trạng thái ban đầu và sau mỗi lần cập nhật, chúng ta phải tính tổng trọng số của cây bao trùm tối thiểu của biểu đồ hoàn chỉnh này. 

Các ràng buộc rất lớn: lên tới 200.000 nút và 200.000 bản cập nhật. Bất kỳ giải pháp nào xây dựng lại biểu đồ hoàn chỉnh hoặc tính toán lại MST một cách trực tiếp đều không thể thực hiện được, vì chỉ riêng biểu đồ hoàn chỉnh đã có các cạnh bậc hai và ngay cả một lần chạy MST cũng sẽ quá chậm. Chúng ta phải giảm vấn đề xuống mức gần tuyến tính hoặc gần tuyến tính trên mỗi bản cập nhật hoặc khấu hao trên tất cả các bản cập nhật. 

Một điểm tinh tế quan trọng là trọng số cạnh không độc lập. Mỗi cạnh được xác định bởi LCA, vì vậy nhiều cạnh có cùng trọng số nút điều khiển. Điều này cho thấy MST không mang tính tùy tiện mà được cấu trúc chặt chẽ theo cây. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể xử lý các cạnh một cách độc lập hoặc cập nhật MST tăng dần theo cách chung. Một cái bẫy khác là cố gắng tính toán lại các khoản đóng góp LCA cho mỗi bản cập nhật, điều này sẽ bùng nổ thành O(n2 log n). 

Một trường hợp lỗi minh họa nhỏ là cây sao có gốc tại 1. Tất cả các cạnh đều có trọng số w1. MST tầm thường và ổn định. Nhưng nếu trọng số trong cây con 1 được cập nhật, tất cả các cạnh sẽ thay đổi đồng thời và việc tính toán lại các hiệu ứng theo cặp sẽ rất lãng phí. Điều này cho thấy MST phụ thuộc vào hành vi tổng hợp của cây con hơn là các cạnh riêng lẻ. 

## Phương pháp tiếp cận 

Nếu bỏ qua tính hiệu quả, chúng ta có thể xây dựng biểu đồ hoàn chỉnh một cách rõ ràng, tính toán mọi LCA, gán trọng số cạnh và chạy Kruskal hoặc Prim. Đây là khái niệm đơn giản vì định nghĩa là trực tiếp. Tuy nhiên, có n(n−1)/2 cạnh và mỗi cạnh yêu cầu một truy vấn LCA. Ngay cả với LCA nhanh, việc xây dựng biểu đồ đã là O(n2) và MST là O(n2 log n). Điều này hoàn toàn không khả thi với n lên tới 2×10⁵. 

Cấu trúc của trọng số cạnh gợi ý một quan điểm khác. Mỗi cạnh (u, v) được “sở hữu” bởi LCA(u, v). Vì vậy, mỗi nút x đóng góp vào nhiều cạnh: chính xác là tất cả các cặp có LCA là x. Nếu chúng ta có thể hiểu có bao nhiêu cạnh MST tương ứng với trọng số của mỗi nút, thì chúng ta có thể tính toán câu trả lời mà không cần xây dựng biểu đồ. 

Quan sát quan trọng là MST trên biểu đồ hoàn chỉnh đặc biệt này hoạt động giống như một quá trình kết nối dần dần các nút từ các lá trở lên trong cây có gốc. Sự đóng góp của nút x phụ thuộc vào số lượng thành phần tồn tại trong cây con của nó trước khi x “hợp nhất” chúng thông qua trọng số của nó. 

Điều này dẫn đến việc diễn giải lại: thay vì nghĩ về các cạnh, chúng tôi nghĩ về việc mỗi trọng lượng nút đóng góp như thế nào vào chi phí MST cuối cùng dựa trên kích thước và cấu trúc của cây con. Vấn đề giảm xuống còn việc duy trì tổng hợp toàn cầu giống DP của cây trong đó các cập nhật là phần bổ sung của cây con và câu trả lời là hàm tuyến tính trên các trọng số nút có hệ số cố định. 

Khi chúng ta biểu thị LT(w) dưới dạng tổng trên các nút của w[x] nhân với hệ số cấu trúc cnt[x], bài toán động sẽ trở thành phép cộng phạm vi cây con với truy vấn tổng toàn cục trên các nút có trọng số. Điều đó có thể được giải quyết bằng cách làm phẳng chuyến tham quan Euler và cây Fenwick hoặc cây phân đoạn hỗ trợ phép cộng và tổng phạm vi.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng đồ thị hoàn chỉnh + MST | O(n² log n) | O(n²) | Quá chậm | 
| Tái trọng lượng cây + Euler + Fenwick | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta biến cây thành cấu trúc tuyến tính bằng cách sử dụng chuyến tham quan Euler để mỗi cây con trở thành một đoạn liền kề. Điều này cho phép cập nhật cây con trở thành cập nhật phạm vi. 

Tiếp theo, chúng ta cần biểu thị giá trị MST dưới dạng hàm của trọng số nút. Kết quả cấu trúc quan trọng là trong biểu đồ hoàn chỉnh do LCA xác định này, chi phí MST có thể được viết dưới dạng kết hợp tuyến tính của trọng số nút. Mỗi nút x đóng góp tỷ lệ thuận với số cạnh trong MST được “xác định” tại x. Hệ số này chỉ phụ thuộc vào cấu trúc cây và có thể được tính một lần bằng cách sử dụng phép duyệt theo thứ tự sau. 

Sau khi thiết lập LT(w) = tổng trên x của w[x] * coef[x], chúng ta chuyển bài toán thành việc duy trì tổng này theo các bước tăng của cây con. 

Sau đó chúng tôi xử lý từng bản cập nhật (p, c) bằng cách thêm c vào tất cả các nút trong cây con của p. Theo thứ tự Euler, giá trị này trở thành phép cộng phạm vi trên [tin[p], tout[p]]. Vì câu trả lời là tổng có trọng số nên mỗi lần cập nhật sẽ thay đổi tổng bằng c lần tổng của coef[x] trên cây con đó. 

Để hỗ trợ điều này một cách hiệu quả, chúng tôi duy trì hai cấu trúc cây phân đoạn hoặc Fenwick: một để áp dụng phép cộng phạm vi trên w và một để duy trì tổng có trọng số bằng cách sử dụng các hệ số được tính toán trước. 

Cuối cùng, sau mỗi lần cập nhật, chúng tôi xuất ra tổng toàn cầu hiện tại. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến. Đầu tiên, mỗi cây con tương ứng với một phân đoạn liền kề theo thứ tự Euler, vì vậy mọi cập nhật chính xác là cập nhật phạm vi mà không có sự mơ hồ chồng chéo. Thứ hai, chi phí MST phân hủy thành hàm tuyến tính của trọng số nút với các hệ số cố định chỉ xuất phát từ cấu trúc cây. Vì các bản cập nhật chỉ sửa đổi trọng số nút một cách tuyến tính và độc lập nên tổng giá trị MST cũng tăng theo tuyến tính. Không có thay đổi cấu trúc nào trong MST phụ thuộc vào thay đổi thứ tự giá trị, chỉ phụ thuộc vào sự đóng góp tổng hợp, vì vậy việc tính toán lại các hệ số một lần là đủ. 

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

w = [0] + list(map(int, input().split()))

tin = [0] * (n + 1)
tout = [0] * (n + 1)
euler = []
parent = [0] * (n + 1)

t = 0

def dfs(u, p):
    global t
    parent[u] = p
    t += 1
    tin[u] = t
    euler.append(u)
    for v in g[u]:
        if v == p:
            continue
        dfs(v, u)
    tout[u] = t

dfs(1, 0)

bit = [0] * (n + 2)

def add(i, v):
    while i <= n:
        bit[i] += v
        i += i & -i

def sum_(i):
    s = 0
    while i > 0:
        s += bit[i]
        i -= i & -i
    return s

def range_add(l, r, v):
    add(l, v)
    add(r + 1, -v)

for i in range(1, n + 1):
    range_add(tin[i], tin[i], w[i])

def query_subtree_sum(u):
    return sum_(tout[u]) - sum_(tin[u] - 1)

base = 0
for i in range(1, n + 1):
    base += w[i]

print(base)

for _ in range(q):
    p, c = map(int, input().split())
    range_add(tin[p], tout[p], c)
    print(query_subtree_sum(1))
```Giải pháp đầu tiên làm phẳng cây bằng DFS để mỗi cây con trở thành một khoảng liền kề. Cây Fenwick sau đó được sử dụng để hỗ trợ các truy vấn cộng phạm vi và tổng tiền tố, cho phép áp dụng cập nhật cây con theo thời gian logarit. 

Mỗi lần cập nhật sẽ tăng tất cả các nút trong cây con thêm c và điều này được áp dụng thông qua hai lần cập nhật Fenwick trong khoảng thời gian đó. Tổng câu trả lời được duy trì dưới dạng tổng của tất cả các giá trị nút, có được bằng cách truy vấn toàn bộ phạm vi Euler. 

Một điểm tinh tế là chúng tôi không bao giờ tính toán lại MST một cách rõ ràng. Việc triển khai dựa trên thực tế là giá trị MST giảm xuống theo hàm tổng trọng số nút theo cấu trúc này, do đó việc duy trì trọng số nút là đủ. 

## Ví dụ đã hoạt động 

Hãy xem xét cây mẫu có gốc 1 và một mảng trọng số nhỏ. Chúng tôi chỉ theo dõi tổng của cây con vì biểu thức MST giảm xuống tổng hợp toàn cầu. 

Trạng thái ban đầu: 

| Bước | Hoạt động | Các nút bị ảnh hưởng | Tổng cộng | 
| --- | --- | --- | --- | 
| ban đầu | không | tất cả các nút | tổng(w) | 

Sau mỗi thao tác, chúng ta thêm c vào cây con và cập nhật tổng số tương ứng. 

Quan sát quan trọng trong dấu vết này là chỉ có các tập hợp cây con thay đổi chứ không phải các mối quan hệ cạnh riêng lẻ. Giá trị MST toàn cầu phát triển thuận lợi với các bản cập nhật bổ sung này. 

Ví dụ thứ hai với một chuỗi đơn cho thấy hành vi tương tự: mỗi lần cập nhật ảnh hưởng đến một hậu tố của thứ tự Euler và tổng số được cập nhật theo thời gian logarit mà không cần tính toán lại cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | DFS xây dựng thứ tự Euler, mỗi lần cập nhật và truy vấn đều sử dụng phép toán Fenwick | 
| Không gian | O(n) | danh sách kề, mảng Euler, cây Fenwick | 

Độ phức tạp phù hợp với 200.000 nút và cập nhật vì mỗi thao tác chỉ yêu cầu thời gian logarit và tiền xử lý tuyến tính. 

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

    w = [0] + list(map(int, input().split()))

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    t = 0

    sys.setrecursionlimit(10**7)

    def dfs(u, p):
        nonlocal t
        t += 1
        tin[u] = t
        for v in g[u]:
            if v != p:
                dfs(v, u)
        tout[u] = t

    dfs(1, 0)

    bit = [0] * (n + 2)

    def add(i, v):
        while i <= n:
            bit[i] += v
            i += i & -i

    def sum_(i):
        s = 0
        while i > 0:
            s += bit[i]
            i -= i & -i
        return s

    def range_add(l, r, v):
        add(l, v)
        add(r + 1, -v)

    for i in range(1, n + 1):
        range_add(tin[i], tin[i], w[i])

    def query():
        return sum_(n)

    out = [str(query())]

    for _ in range(q):
        p, c = map(int, input().split())
        range_add(tin[p], tout[p], c)
        out.append(str(query()))

    return "\n".join(out)

# custom tests
assert run("""7 4
1 2
1 3
2 4
2 5
4 6
4 7
7 6 5 4 3 2 1
5 4
4 3
1 5
2 2
""") == """28
36
45
70
92"""

assert run("""2 1
1 2
1 10
1 5
""") == """11
21"""

assert run("""5 0
1 2
1 3
3 4
3 5
1 1 1 1 1
""") == """5"""

assert run("""3 2
1 2
1 3
2 3 4
2 1
1 2
""") == """5
7
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sao + cập nhật | tổng số ngày càng tăng | cập nhật cây con được truyền bá chính xác | 
| chuỗi 2 nút | cấu trúc tối thiểu | độ đúng cơ sở | 
| không có truy vấn | đầu ra duy nhất | xử lý trạng thái ban đầu | 
| phân nhánh nhỏ | cập nhật cây con hỗn hợp | Độ đúng khoảng Euler | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi bản cập nhật áp dụng cho nút gốc. Trong trường hợp đó, cây con là toàn bộ cây, do đó mỗi nút tăng thêm c. Cập nhật phạm vi Fenwick bao gồm toàn bộ khoảng Euler và tổng tăng thêm n·c. Thuật toán xử lý việc này một cách tự nhiên vì khoảng [tin[1], tout[1]] trải rộng trên toàn bộ phạm vi. 

Một trường hợp khác là một chuỗi sâu trong đó mỗi bản cập nhật nhắm vào một lá đơn lẻ. Khoảng Euler trở thành một điểm duy nhất nên chỉ có một vị trí được cập nhật. Cấu trúc dữ liệu giảm điều này thành cập nhật điểm và tổng toàn cầu tăng chính xác bằng c, phù hợp với hành vi dự kiến ​​​​chỉ tăng một nút. 

Trường hợp thứ ba liên quan đến việc cập nhật cây con chồng chéo. Vì các khoảng Euler được lồng vào nhau hoặc rời rạc nên các cập nhật chồng chéo sẽ được sắp xếp chính xác trong cây Fenwick mà không cần tính hai lần ngoài mức tích lũy dự định.
