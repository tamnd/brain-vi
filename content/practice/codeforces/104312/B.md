---
title: "CF 104312B - Giờ ăn nhẹ"
description: "Chúng tôi được tặng một cây nhà. Mỗi ngôi nhà ban đầu có một số lượng bạn bè nhất định sống ở đó. Theo thời gian, có hai loại sự kiện xảy ra."
date: "2026-07-01T19:51:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104312
codeforces_index: "B"
codeforces_contest_name: "UTPC Spring 2023 Contest (HS)"
rating: 0
weight: 104312
solve_time_s: 82
verified: false
draft: false
---

[CF 104312B - Thời gian ăn nhẹ](https://codeforces.com/problemset/problem/104312/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây nhà. Mỗi ngôi nhà ban đầu có một số lượng bạn bè nhất định sống ở đó. Theo thời gian, có hai loại sự kiện xảy ra. Umaru thực hiện truy vấn du lịch giữa hai ngôi nhà hoặc cô ấy thực hiện cập nhật nhằm tăng số lượng bạn bè trong một ngôi nhà cụ thể bằng cách nhân nó với một số hệ số. 

Đối với mỗi truy vấn du lịch, Umaru đi dọc theo con đường đơn giản duy nhất giữa hai nút nhất định trong cây. Mỗi ngôi nhà trên con đường đó đều đóng góp số lượng bạn bè hiện tại của mình. Cô ấy muốn mang theo một số đồ ăn nhẹ có thể chia hết cho mọi giá trị trên con đường đó. Nói cách khác, cô ấy cần bội số chung nhỏ nhất của các giá trị trên đường dẫn đó tại thời điểm đó và cô ấy xuất ra nó theo modulo 10^9 + 7. 

Cấu trúc cây đảm bảo có chính xác một đường dẫn giữa hai nút bất kỳ, do đó, mỗi truy vấn giảm xuống việc xử lý một đường dẫn trên một mảng động được xác định trên cây. 

Các ràng buộc nhỏ, với N và Q lên tới 1000. Điều này rất quan trọng vì nó cho phép các giải pháp tính toán lại các giá trị trên các đường dẫn một cách trực tiếp hoặc xây dựng lại thông tin nhiều lần. Bất cứ điều gì liên quan đến N^2 cho mỗi truy vấn vẫn ở mức giới hạn nhưng có thể chấp nhận được, trong khi mọi thứ khối trên tất cả các truy vấn vẫn sẽ vượt qua một cách thoải mái trong Python khi được triển khai chặt chẽ. 

Một khía cạnh tinh tế là các giá trị tăng theo thời gian do cập nhật theo cấp số nhân. Vì chúng tôi đang làm việc với LCM nên việc phân tích hệ số đơn giản cho mỗi truy vấn vẫn có thể hoạt động trong các giới hạn này nhưng phải được cấu trúc cẩn thận để tránh chi phí tính toán lại. 

Một trường hợp biên điển hình phát sinh khi các bản cập nhật ảnh hưởng đến các nút không nằm trên đường dẫn được truy vấn. Ví dụ: nếu chúng ta nhân nút 5 với 10, nhưng sau đó truy vấn một đường dẫn không bao gồm nút 5 thì kết quả sẽ không bị ảnh hưởng. Một trường hợp khác là cập nhật lặp lại trên cùng một nút, có thể tăng giá trị nhanh chóng. Ví dụ: một nút bắt đầu từ 2 được cập nhật theo hệ số 3, 5 và 7 sẽ trở thành 210 và phải được phản ánh đầy đủ trong các tính toán đường dẫn sau này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp xử lý từng truy vấn một cách độc lập. Đối với truy vấn di chuyển giữa u và v, trước tiên chúng tôi tìm đường dẫn duy nhất giữa chúng bằng cách sử dụng cấu trúc lại cha mẹ DFS hoặc BFS. Sau khi có đường dẫn, chúng tôi lặp lại tất cả các nút trên đó và tính bội số chung nhỏ nhất của các giá trị hiện tại của chúng. Để cập nhật, chúng tôi chỉ cần nhân giá trị được lưu trữ tại một nút. 

Điều này đúng vì cấu trúc cây đảm bảo một đường dẫn duy nhất và LCM trên một tập hợp có tính kết hợp và có thể được tính toán tăng dần. Vấn đề là hiệu quả đối với các truy vấn lặp đi lặp lại. 

Độ dài đường dẫn trong trường hợp xấu nhất là O(N) và có thể có các truy vấn O(Q). Mỗi truy vấn tính toán lại LCM trên tối đa các nút O(N), do đó tổng độ phức tạp sẽ trở thành O(NQ), tức là khoảng 10^6 thao tác, đã ổn. Tuy nhiên, nếu được triển khai bằng hệ số đơn giản trong các tính toán LCM, mỗi số có thể gấp tới 10^7 lần số nhân, gây ra hiện tượng bùng nổ hệ số và các phép tính gcd lặp đi lặp lại làm giảm hiệu suất. 

Một quan sát rõ ràng hơn là chúng ta thực sự không cần tính toán lại LCM đầy đủ từ đầu mỗi lần. Vì LCM phụ thuộc vào số mũ nguyên tố nên vấn đề giảm xuống còn việc duy trì hệ số nguyên tố của nó và lấy số mũ tối đa dọc theo đường dẫn đối với mỗi giá trị nút. Mỗi bản cập nhật chỉ ảnh hưởng đến một nút, vì vậy chúng tôi cập nhật hệ số của nút đó theo từng bước. Mỗi truy vấn trở thành một vấn đề "đường dẫn tối đa" đối với số mũ nguyên tố. 

Vì N nhỏ nên chúng ta có thể tính toán trước các đường dẫn thông qua LCA hoặc con trỏ gốc và chỉ cần đi theo đường dẫn cho mỗi truy vấn, duy trì bản đồ các số mũ nguyên tố. Mỗi truy vấn trở thành tuyến tính về độ dài đường dẫn và các bản cập nhật trở thành hệ số O (giá trị nhật ký). 

Do đó, giải pháp về cơ bản là: phân tích các giá trị, duy trì bản đồ số mũ nguyên tố trên mỗi nút và đối với mỗi truy vấn, hãy đi theo đường dẫn và tính số mũ tối đa cho mỗi số nguyên tố.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại LCM theo kiểu Brute Force cho mỗi truy vấn | O(NQ + Q log A) | O(N) | Đã chấp nhận | 
| Tổng hợp đường dẫn nhân tố | O(Q * N * sqrt(A)) | O(N log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây và tiền xử lý các mảng gốc và mảng sâu bằng cách sử dụng DFS từ một gốc tùy ý. Điều này cho phép xây dựng lại đường dẫn giữa hai nút bất kỳ bằng cách nâng cả hai nút cho đến khi đạt được tổ tiên chung thấp nhất của chúng. 
2. Duy trì một mảng`fact[i]`lưu trữ hệ số nguyên tố của giá trị hiện tại tại nút i dưới dạng từ điển hoặc bộ đếm các số nguyên tố thành số mũ. Khởi tạo điều này bằng cách phân tích tất cả a[i]. 
3. Đối với mỗi truy vấn cập nhật có dạng nhân nút w với f, phân tích f và cộng số mũ của nó vào`fact[w]`. Điều này đảm bảo giá trị của nút vẫn được biểu diễn chính xác ở dạng số mũ nguyên tố. 
4. Đối với mỗi truy vấn di chuyển giữa u và v, trước tiên hãy xây dựng lại các nút đường dẫn đầy đủ bằng cách leo lên u và v tới LCA của chúng và nối các đoạn. Điều này cung cấp cho tất cả các nút trên đường dẫn đơn giản theo thứ tự. 
5. Khởi tạo một từ điển trống`res`để lưu trữ số mũ tối đa trên đường dẫn. 
6. Duyệt qua từng nút trên đường dẫn và đối với mỗi số nguyên tố trong hệ số hóa của nó, hãy cập nhật`res[p] = max(res[p], fact[node][p])`. Bước này tổng hợp cấu trúc số mũ cần thiết cho LCM. 
7. Sau khi xử lý tất cả các nút trên đường dẫn, hãy tính kết quả bằng cách nhân`p^res[p] mod MOD`cho mọi số nguyên tố trong`res`. 
8. Xuất giá trị này và tiếp tục truy vấn tiếp theo. 

Lý do chính khiến việc này hiệu quả là việc phân tích hệ số chỉ được thực hiện khi cập nhật và việc truyền tải đường dẫn là tuyến tính trong N. Vì cả N và Q đều nhỏ nên tổng chi phí vẫn nằm trong giới hạn. 

### Tại sao nó hoạt động 

LCM của một tập hợp số được xác định bằng cách lấy, đối với mỗi số nguyên tố, số mũ lớn nhất của số nguyên tố đó trên tất cả các số. Việc biểu thị giá trị của mỗi nút bằng hệ số nguyên tố của nó sẽ bảo toàn tất cả thông tin cần thiết. Các cập nhật chỉ tăng số mũ chứ không bao giờ giảm chúng, do đó việc phân tích thành hệ số vẫn nhất quán theo thời gian. Khi chúng ta đi qua một đường dẫn, việc thu thập số mũ tối đa trên mỗi số nguyên tố sẽ tái tạo lại chính xác LCM của đường dẫn đó. Không có sự tương tác giữa các số nguyên tố, do đó việc xử lý chúng một cách độc lập luôn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

from collections import defaultdict

def factorize(x):
    res = defaultdict(int)
    d = 2
    while d * d <= x:
        while x % d == 0:
            res[d] += 1
            x //= d
        d += 1
    if x > 1:
        res[x] += 1
    return res

def lca(u, v, parent, depth):
    visited = set()
    while u != v:
        if depth[u] > depth[v]:
            visited.add(u)
            u = parent[u]
        else:
            visited.add(v)
            v = parent[v]
    visited.add(u)
    return visited

def build_path(u, v, parent, depth):
    path_u = []
    path_v = []
    a, b = u, v
    while depth[a] > depth[b]:
        path_u.append(a)
        a = parent[a]
    while depth[b] > depth[a]:
        path_v.append(b)
        b = parent[b]
    while a != b:
        path_u.append(a)
        path_v.append(b)
        a = parent[a]
        b = parent[b]
    path_u.append(a)
    return path_u + path_v[::-1]

def dfs(root, g, parent, depth):
    stack = [(root, -1)]
    parent[root] = -1
    depth[root] = 0
    order = []
    while stack:
        u, p = stack.pop()
        for v in g[u]:
            if v == p:
                continue
            parent[v] = u
            depth[v] = depth[u] + 1
            stack.append((v, u))
    return

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    depth = [0] * n
    dfs(0, g, parent, depth)

    fact = [factorize(x) for x in a]

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            u, v = tmp[1] - 1, tmp[2] - 1
            path = build_path(u, v, parent, depth)
            cur = defaultdict(int)
            for node in path:
                for p, e in fact[node].items():
                    if e > cur[p]:
                        cur[p] = e
            ans = 1
            for p, e in cur.items():
                ans = (ans * pow(p, e, MOD)) % MOD
            print(ans)
        else:
            w, f = tmp[1] - 1, tmp[2]
            add = factorize(f)
            for p, e in add.items():
                fact[w][p] += e

if __name__ == "__main__":
    solve()
```DFS thiết lập thông tin gốc và thông tin độ sâu để bất kỳ đường dẫn nào có thể được xây dựng lại theo thời gian tuyến tính. Bảng hệ số hóa`fact`luôn được cập nhật theo các bản cập nhật nhân lên, vì vậy nó luôn là sự thể hiện trung thực của từng giá trị nút. 

các`build_path`hàm xây dựng lại đường dẫn đơn giản bằng cách nâng cả hai điểm cuối cho đến khi chúng gặp nhau, điều này an toàn vì cấu trúc là một cái cây. Điều này tránh mọi nhu cầu xử lý trước LCA ngoài con trỏ gốc. 

Sau đó, mỗi truy vấn sẽ giảm xuống mức quét qua đường dẫn với từ điển tích lũy số mũ nguyên tố tối đa, tương ứng trực tiếp với việc tính toán LCM. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. Truy vấn đầu tiên tính toán LCM dọc theo đường dẫn trong cây ban đầu. Quá trình truyền tải thu thập các giá trị từ tất cả các nút trên đường dẫn đó và hợp nhất các thừa số nguyên tố của chúng, tạo ra 12. 

| Bước | Nút | Hệ số giá trị hiện tại | Số mũ tối đa tổng hợp | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | {} | 
| Thăm 1 | 1 | {1} | {1:1} | 
| Thăm 2 | 2 | {2,3} | {1:1,2:1,3:1} | 
| Thăm 5 | 5 | {2,2} | {1:1,2:2,3:1} | 

Kết quả thu được là 2^2 * 3 = 12, xác nhận tính đúng đắn. 

Sau khi cập nhật, nút 2 sẽ được nhân với 4, tăng số mũ của nó lên 2 lên 2. Điều này thay đổi hệ số của nó từ 6 lên 24. 

| Bước | Nút | Nhân tố hóa | Cập nhật tối đa | 
| --- | --- | --- | --- | 
| Cập nhật nút 2 | - | {2:2,3:1} -> {2:4,3:1} | áp dụng | 

Truy vấn thứ hai hiện bao gồm nút 2 trong đường dẫn, do đó LCM phải phản ánh lũy thừa tăng thêm của 2. Kết quả trở thành 24. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q * N * sqrt(A)) | Mỗi truy vấn có thể đi qua các nút O(N) và chi phí nhân tố hóa chỉ là O(sqrt(A)) khi cập nhật | 
| Không gian | O(N log A) | Mỗi nút lưu trữ hệ số nguyên tố của giá trị hiện tại của nó | 

Các ràng buộc giữ N và Q tối đa là 1000, do đó, ngay cả việc truyền tải toàn bộ cho mỗi truy vấn kết hợp với chi phí phân tích hệ số vẫn duy trì tốt trong giới hạn thời gian. Giải pháp vừa vặn thoải mái dưới cả 2 giây và 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("""6 3
1 6 5 3 4 3
1 2
1 3
1 4
2 5
4 6
1 1 5
2 2 4
1 1 2
""") == """12
24"""

# small chain
assert run("""3 2
2 3 5
1 2
2 3
1 1 3
1 2 3
""") == """30
15"""

# all equal
assert run("""4 1
7 7 7 7
1 2
2 3
3 4
1 1 4
""") == """7"""

# single edge updates
assert run("""2 3
2 2
1 2
2 1 3
1 1 2
1 1 2
""") == """6
6"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dây chuyền nhỏ | 30, 15 | tính đúng đắn của việc hợp nhất đường dẫn | 
| tất cả đều bình đẳng | 7 | ổn định dưới các giá trị thống nhất | 
| cập nhật lặp đi lặp lại | 6, 6 | cập nhật kiên trì | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các cập nhật lặp lại trên một nút. Nếu một nút được nhân lên nhiều lần thì hệ số của nó phải tích lũy chính xác thay vì ghi đè lên các giá trị trước đó. Ví dụ: bắt đầu bằng 2 và áp dụng bội số 3 và 5 sẽ dẫn đến phân tích thành thừa số {2:1,3:1,5:1}. Logic cập nhật trực tiếp thêm số mũ nên những đóng góp trước đó được giữ nguyên. 

Một trường hợp đặc biệt khác là các truy vấn trong đó u bằng v. Đường dẫn chứa một nút duy nhất, vì vậy câu trả lời chỉ đơn giản là giá trị hiện tại của nút đó. Logic xây dựng đường dẫn trả về một danh sách một phần tử trong trường hợp này và tích lũy LCM sẽ giảm chính xác thành hệ số của nút đó. 

Trường hợp cuối cùng là cập nhật trên các nút không bao giờ được truy vấn. Những điều này không ảnh hưởng đến bất kỳ đầu ra nào nhưng vẫn phải được áp dụng chính xác vì các truy vấn trong tương lai có thể bao gồm các nút đó sau trong chuỗi. Việc lưu trữ hệ số hóa đảm bảo các bản cập nhật mang tính toàn cầu và liên tục, không phụ thuộc vào thứ tự truy vấn.
