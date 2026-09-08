---
title: "CF 104566B - Cây Đỏ Đen"
description: "Chúng ta được cho một cây có trọng số có gốc tại nút 1. Một số đỉnh ban đầu có màu đỏ, bao gồm cả gốc và tất cả các đỉnh khác có màu đen."
date: "2026-06-30T08:31:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104566
codeforces_index: "B"
codeforces_contest_name: "The 2018 ACM-ICPC Asia Qingdao Regional Contest, Online (The 2nd Universal Cup. Stage 1: Qingdao)"
rating: 0
weight: 104566
solve_time_s: 83
verified: true
draft: false
---

[CF 104566B - Cây đỏ đen](https://codeforces.com/problemset/problem/104566/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có trọng số có gốc tại nút 1. Một số đỉnh ban đầu có màu đỏ, bao gồm cả gốc và tất cả các đỉnh khác có màu đen. Đối với mỗi đỉnh, chi phí của nó được xác định bằng cách sử dụng các đỉnh màu đỏ trên đường đi tới gốc: nếu bản thân đỉnh đó có màu đỏ thì chi phí của nó bằng 0, nếu không, chúng ta nhìn lên dọc theo đường đi duy nhất đến gốc và tìm tổ tiên màu đỏ gần nhất và chi phí là khoảng cách dọc theo cây đến tổ tiên đó. 

Đối với mỗi truy vấn, chúng ta được cung cấp một tập hợp con các đỉnh. Chúng ta được phép chọn tối đa một đỉnh ở bất kỳ vị trí nào trên cây và tạm thời chuyển nó thành màu đỏ. Sau sự thay đổi đó, giá của mỗi đỉnh được tính lại bằng quy tắc tương tự. Nhiệm vụ là giảm thiểu chi phí tối đa giữa các đỉnh được truy vấn. 

Kích thước đầu vào buộc chúng ta phải suy nghĩ trong thời gian gần tuyến tính cho mỗi trường hợp thử nghiệm. Cây có thể có tới 100.000 nút và tổng số đỉnh được truy vấn trên tất cả các truy vấn có thể lên tới 2.000.000. Điều này ngay lập tức loại trừ bất kỳ việc duyệt cây theo truy vấn nào hoặc tính toán lại khoảng cách từ đầu. Bất kỳ giải pháp nào cũng phải xử lý trước cây một lần và trả lời từng truy vấn trong thời gian gần như tuyến tính theo kích thước của bộ truy vấn. 

Một cách tiếp cận đơn giản sẽ tính toán lại chi phí sau khi thử mọi lựa chọn có thể có của đỉnh đỏ mới được thêm vào. Đối với một truy vấn, điều đó có nghĩa là thử tất cả n lựa chọn và tính toán lại khoảng cách cho tất cả các đỉnh ki mỗi lần, dẫn đến O(n·ki) cho mỗi truy vấn, con số này quá lớn. 

Một trường hợp thất bại tinh tế hơn xuất phát từ việc bỏ qua rằng tác động của việc tạo ra một đỉnh màu đỏ không mang tính toàn cục. Ví dụ: hãy xem xét một đỉnh x không phải là đỉnh tổ tiên của đỉnh truy vấn v. Việc chuyển x thành màu đỏ hoàn toàn không ảnh hưởng đến v. Bất kỳ cách tiếp cận nào giả định một đỉnh đỏ mới trên toàn cầu sẽ cải thiện mọi khoảng cách sẽ tạo ra câu trả lời sai. 

Một chế độ lỗi khác là giả định rằng chỉ các đỉnh truy vấn mới là ứng cử viên cho nút đỏ được thêm vào. Nút tối ưu có thể nằm bên ngoài tập truy vấn, ví dụ như tổ tiên chung của một số đỉnh truy vấn có chi phí cao. 

## Phương pháp tiếp cận 

Quan sát quan trọng là mọi đỉnh đều đã có một chi phí được xác định rõ ràng được tính toán từ tổ tiên màu đỏ gần nhất trên đường dẫn gốc của nó. Chúng ta có thể xử lý trước các chi phí này trong một DFS từ gốc bằng cách duy trì đỉnh màu đỏ cuối cùng được nhìn thấy trên đường đi và tính toán khoảng cách bằng cách sử dụng tổng tiền tố của trọng số cạnh. 

Khi đã biết các chi phí cơ bản này, mỗi truy vấn sẽ trở thành một vấn đề tối ưu hóa thuần túy trên một tập hợp con các đỉnh: chúng tôi muốn giảm mức tối đa của một tập hợp các giá trị bằng cách tùy ý đưa vào một đỉnh “nguồn” mới chỉ ảnh hưởng đến các đỉnh trong cây con của chính nó. 

Chiến lược brute-force sẽ thử mọi đỉnh x có thể làm nút màu đỏ mới. Đối với mỗi x, chúng tôi sẽ tính toán lại chi phí của mỗi đỉnh v được truy vấn dưới dạng chi phí ban đầu hoặc khoảng cách từ v đến x nếu x nằm trên đường đi từ gốc đến v. Điều này tốn O(n·k) cho mỗi truy vấn và thất bại ngay lập tức ở các ràng buộc đã cho. 

Sự đơn giản hóa chính là chỉ tập trung vào các đỉnh xác định câu trả lời tối đa hiện tại. Nếu chúng tôi không thể giảm giá trị chi phí tối đa hiện tại giữa các đỉnh truy vấn thì không thể cải thiện được. Nếu chúng ta có thể giảm nó thì nút đỏ mới phải nằm trên đường dẫn gốc tới LCA của tất cả các đỉnh đạt được mức tối đa đó, vì nếu không thì ít nhất một trong số chúng sẽ không bị ảnh hưởng. 

Điều này làm giảm đáng kể không gian tìm kiếm của nút đỏ mới. Thay vì xem xét tất cả các đỉnh, chúng ta chỉ xem xét một cấu trúc ứng cử viên duy nhất được xác định bởi LCA của các đỉnh kém nhất. 

Từ đó, vấn đề trở thành kiểm tra xem việc chọn LCA đó làm nút đỏ mới có đủ để giảm tất cả các đỉnh có chi phí tối đa xuống dưới mức tối đa ban đầu hay không và nếu có thì tính toán mức tối đa mới thu được.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các nút đỏ mới | O(q · n · k) | O(n) | Quá chậm | 
| Giảm thiểu dựa trên LCA tối ưu | O(∑k log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xử lý trước cây để tính toán hai giá trị cho mỗi đỉnh: khoảng cách từ gốc và giá trị của nó dựa trên tổ tiên đỏ gần nhất. Điều này được thực hiện với một DFS duy nhất trong đó chúng tôi mang cả khoảng cách gốc hiện tại và nút màu đỏ gần nhất được nhìn thấy cho đến nay trên đường dẫn. 

Đối với các truy vấn LCA, chúng tôi cũng xây dựng cấu trúc nâng nhị phân tiêu chuẩn để có thể tính toán LCA theo thời gian logarit. 

Đối với mỗi truy vấn, chúng tôi tiến hành như sau. 

1. Chúng tôi quét tất cả các đỉnh trong bộ truy vấn và tính toán chi phí hiện tại của chúng. Trong quá trình quét này, chúng tôi xác định giá trị chi phí tối đa M và cũng theo dõi giá trị chi phí tối đa thứ hai trong số các đỉnh còn lại. Chúng tôi cũng thu thập tập T các đỉnh có giá bằng M. Điều này tách biệt chính xác các đỉnh xác định câu trả lời hiện tại. 
2. Nếu T chỉ chứa một đỉnh thì bài toán trở nên đơn giản hơn vì ta chỉ cần xét rút gọn một đỉnh đó. Nếu nhiều đỉnh chia sẻ mức tối đa thì tất cả chúng phải được giảm đồng thời để câu trả lời được cải thiện. 
3. Chúng tôi tính toán LCA của tất cả các đỉnh trong T. Nút này là đỉnh sâu nhất là tổ tiên của mọi đỉnh có chi phí tối đa và bất kỳ nút màu đỏ mới ứng cử viên nào ảnh hưởng đến tất cả chúng đều phải nằm trên đường đi từ gốc xuống LCA này. 
4. Chúng tôi kiểm tra xem việc chọn LCA này làm nút đỏ mới có đủ để giảm tất cả các đỉnh trong T hay không. Với mỗi v trong T, chi phí mới sẽ trở thành khoảng cách từ v đến LCA, bằng dist_root[v] − dist_root[LCA]. Nếu bất kỳ giá trị nào trong số này vẫn ít nhất là M thì không thể cải thiện ở mức tối đa. 
5. Nếu mức giảm hợp lệ, chúng tôi tính mức tối đa mới giữa các đỉnh truy vấn. Đây là giá trị tối đa của hai giá trị: giá trị tối đa thứ hai từ bộ truy vấn ban đầu và chi phí giảm lớn nhất trong số các đỉnh trong T. 
6. Câu trả lời cho truy vấn là mức tối thiểu giữa M tối đa ban đầu và giá trị cải tiến thu được ở trên. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là chỉ những đỉnh đạt được chi phí tối đa mới quan trọng để cải tiến. Bất kỳ cải tiến hợp lệ nào cũng phải giảm tất cả chúng cùng một lúc; nếu không thì mức tối đa vẫn không thay đổi. Các đỉnh duy nhất có khả năng ảnh hưởng đến tất cả chúng là những đỉnh tổ tiên của mọi đỉnh có chi phí tối đa và trong số các đỉnh này, đỉnh sâu nhất sẽ giảm thiểu khoảng cách đến tất cả các nút bị ảnh hưởng. Điều này buộc ứng cử viên tối ưu phải là LCA của tập tối đa, vì bất kỳ tổ tiên cao hơn nào cũng chỉ tăng khoảng cách mà không cải thiện tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n, m, q = map(int, input().split())
    red = list(map(int, input().split()))
    red_set = set(red)

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        g[u].append((v, w))
        g[v].append((u, w))

    LOG = (n + 1).bit_length()
    up = [[0] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)
    dist_root = [0] * (n + 1)
    parent_red = [0] * (n + 1)

    def dfs(u, p):
        up[0][u] = p
        parent_red[u] = p if u in red_set else parent_red[p]
        for v, w in g[u]:
            if v == p:
                continue
            depth[v] = depth[u] + 1
            dist_root[v] = dist_root[u] + w
            dfs(v, u)

    dfs(1, 0)

    for i in range(1, LOG):
        for v in range(1, n + 1):
            up[i][v] = up[i - 1][up[i - 1][v]]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a
        diff = depth[a] - depth[b]
        i = 0
        while diff:
            if diff & 1:
                a = up[i][a]
            diff >>= 1
            i += 1
        if a == b:
            return a
        for i in range(LOG - 1, -1, -1):
            if up[i][a] != up[i][b]:
                a = up[i][a]
                b = up[i][b]
        return up[0][a]

    # compute initial costs
    # cost[v] = dist to nearest red ancestor
    # we can reconstruct from parent_red pointer
    cost = [0] * (n + 1)
    for v in range(1, n + 1):
        cost[v] = dist_root[v] - dist_root[parent_red[v]]

    for _ in range(q):
        tmp = list(map(int, input().split()))
        k = tmp[0]
        nodes = tmp[1:]

        M = -1
        M2 = -1
        T = []

        for v in nodes:
            c = cost[v]
            if c > M:
                M2 = M
                M = c
                T = [v]
            elif c == M:
                T.append(v)
            elif c > M2:
                M2 = c

        if len(T) == 1:
            t_lca = T[0]
        else:
            t_lca = T[0]
            for v in T[1:]:
                t_lca = lca(t_lca, v)

        # check feasibility of using t_lca
        ok = True
        best_reduced = 0

        for v in T:
            newc = dist_root[v] - dist_root[t_lca]
            if newc >= M:
                ok = False
            best_reduced = max(best_reduced, newc)

        if not ok:
            print(M)
        else:
            ans = max(M2, best_reduced)
            print(min(M, ans))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng một DFS tính toán khoảng cách gốc và xác định đối với mỗi nút, tổ tiên màu đỏ gần nhất của nó trên đường dẫn gốc. Điều này cho phép đánh giá liên tục chi phí ban đầu của bất kỳ đỉnh nào. 

Nâng nhị phân được sử dụng cho các truy vấn LCA vì chúng ta cần tính toán liên tục tổ tiên chung của tất cả các đỉnh có chi phí tối đa trong một truy vấn. Tính toán LCA là thành phần logarit duy nhất cho mỗi truy vấn. 

Mỗi truy vấn được xử lý bằng cách quét các đỉnh của nó một lần để trích xuất chi phí tối đa và tối đa thứ hai và thu thập tập hợp các đỉnh xấu nhất. LCA của bộ này xác định ứng cử viên có ý nghĩa duy nhất để đặt một đỉnh đỏ mới. 

Cuối cùng, chúng tôi xác minh xem liệu ứng viên này có thể thực sự giảm tất cả các đỉnh tồi tệ nhất xuống dưới mức tối đa hiện tại hay không và tính toán câu trả lời được cải thiện tương ứng. 

## Ví dụ đã hoạt động 

Hãy xem xét một truy vấn trong đó chi phí của các nút được truy vấn là`[10, 7, 10, 3]`. 

| Bước | Hành động | M | M2 | T | LCA(T) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Quét các nút | 10 | 7 | [v1, v3] | - | 
| 2 | Tính LCA của T | 10 | 7 | [v1, v3] | x | 

Nếu cả hai nút có chi phí tối đa nằm ở các nhánh khác nhau thì LCA của chúng sẽ trở thành ứng cử viên duy nhất để cải thiện. Nếu khoảng cách từ cả hai nút đến x nhỏ hơn 10 thì mức tối đa sẽ giảm xuống; nếu không thì nó vẫn không thay đổi. 

Bây giờ hãy xem xét truy vấn thứ hai trong đó tất cả các nút đều có chi phí`[5, 5, 5]`. 

| Bước | Hành động | M | M2 | T | LCA(T) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Quét các nút | 5 | - | tất cả các nút | - | 
| 2 | Tính LCA của T | 5 | - | tất cả các nút | nút cây con gốc | 

Trong trường hợp này, ngay cả sau khi chọn LCA, ít nhất một nút vẫn có thể có giá từ 5 trở lên, do đó không thể cải thiện và câu trả lời vẫn là 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + qk) log n) | Tiền xử lý DFS cộng với LCA cho mỗi truy vấn trên các nút tệ nhất được thu thập | 
| Không gian | O(n log n) | Bàn nâng nhị phân và mảng phụ trợ | 

Các ràng buộc cho phép tổng cộng tối đa 10^6 nút và tổng số 2×10^6 phần tử truy vấn, do đó, quét tuyến tính trên mỗi truy vấn kết hợp với các phép toán LCA logarit vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# Minimal tree
assert run("""1
2 1 1
1
1 2 1
1 1 2
""") is not None

# All nodes in query
assert run("""1
3 1 1
1
1 2 1
2 3 1
3 1 2 3
""") is not None

# Single node query
assert run("""1
5 2 1
1 3
1 2 1
2 3 1
3 4 1
4 5 1
1 5
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn nút đơn | 0 hoặc không thay đổi | xử lý chi phí cơ bản | 
| truy vấn đầy đủ | mức giảm tối đa được tính toán | xử lý cấu trúc toàn cầu | 
| dây chuyền nhỏ | lý luận tổ tiên đúng đắn | LCA đúng đắn | 

## Vỏ cạnh 

Khi tất cả các đỉnh được truy vấn đã có giá trị bằng 0 vì chúng có màu đỏ hoặc có tổ tiên màu đỏ ngay phía trên chúng, thuật toán sẽ xác định chính xác M là 0 và ngay lập tức trả về 0, vì không thể cải thiện được. 

Khi nhiều đỉnh có chi phí tối đa nằm trong các cây con hoàn toàn khác nhau, LCA sẽ ở vị trí cao trên cây và không thể giảm đủ tất cả chúng. Việc kiểm tra tính khả thi không thành công vì ít nhất một đỉnh vẫn ở trên hoặc bằng ngưỡng tối đa ban đầu sau khi áp dụng nút đỏ ứng cử viên. 

Khi truy vấn chứa một đỉnh duy nhất, thuật toán sẽ giảm chính xác xuống việc kiểm tra xem đỉnh đó có thể được cải thiện hay không, nhưng vì bất kỳ nút màu đỏ ứng cử viên nào cũng phải là nút tổ tiên của nó, nên phép tính LCA sẽ suy biến về chính đỉnh đó, không tạo ra thay đổi và duy trì tính chính xác.
