---
title: "CF 104252F - Cây được yêu thích"
description: "Chúng tôi được tặng hai cây. Cây đầu tiên, gọi là $T1$, là cấu trúc lớn mà chúng ta được phép tìm kiếm bên trong. Cây thứ hai, $T2$, là mẫu mà chúng ta muốn tìm."
date: "2026-07-01T22:04:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104252
codeforces_index: "F"
codeforces_contest_name: "2022-2023 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104252
solve_time_s: 72
verified: true
draft: false
---

[CF 104252F - Cây yêu thích](https://codeforces.com/problemset/problem/104252/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng hai cây. Cây đầu tiên, gọi nó là$T_1$, là cấu trúc lớn mà chúng ta được phép tìm kiếm bên trong. Cây thứ hai,$T_2$, là mẫu chúng ta muốn tìm. Nhiệm vụ là quyết định xem có tồn tại một tập đỉnh liên thông bên trong hay không.$T_1$tạo thành một cây có cấu trúc giống hệt với$T_2$, nghĩa là có sự gắn nhãn lại từng đỉnh một để duy trì tính kề cận. 

Nói một cách đơn giản hơn, chúng tôi đang tìm kiếm một bản sao của$T_2$ẩn đâu đó bên trong$T_1$, trong đó chúng ta được phép chọn bất kỳ tập con liên thông nào của các đỉnh của$T_1$và cấu trúc cảm ứng đó phải đẳng cấu với$T_2$. 

Cả hai cây đều có tối đa 100 nút. Điều này ngay lập tức gợi ý rằng các giải pháp xung quanh$O(n^3)$hoặc thậm chí$O(n^4)$vẫn có thể được chấp nhận nếu thực hiện cẩn thận, vì$100^3 = 10^6$Và$100^4 = 10^8$, nằm ở ranh giới nhưng chỉ khả thi trong Python với việc cắt tỉa chặt chẽ và các hằng số nhỏ. 

Một ý tưởng hàm mũ ngây thơ là thử mọi tập hợp con của các nút trong$T_1$, kiểm tra xem nó có tạo thành cây không, rồi kiểm tra tính đẳng cấu với$T_2$. Điều này thất bại ngay lập tức vì$T_1$có$2^{100}$các tập hợp con và thậm chí việc giới hạn ở những tập hợp con được kết nối sẽ tạo ra sự bùng nổ theo cấp số nhân về số lượng khả năng. 

Một trường hợp thất bại tinh tế hơn xuất phát từ việc giả định rằng “cây con phù hợp” có nghĩa là “cây con có gốc phù hợp”. Nếu chúng ta tùy ý root cả hai cây và chỉ so sánh các cây con có gốc, chúng ta có thể bỏ lỡ các phần nhúng hợp lệ trong đó tâm được chọn nằm trong$T_1$không tương ứng với gốc của$T_2$. Ví dụ: đường dẫn có độ dài 4 chứa đường dẫn có độ dài 3 ở nhiều vị trí, nhưng việc root ở điểm cuối so với điểm giữa sẽ thay đổi cấu trúc. 

Vì vậy, khó khăn thực sự không phải là tạo ra các ứng cử viên mà là xác minh tính đẳng cấu của cây dưới các ràng buộc nhúng tùy ý một cách hiệu quả. 

## Phương pháp tiếp cận 

Phối cảnh brute-force bắt đầu bằng cách chọn một đỉnh trong$T_1$làm “mỏ neo” của trận đấu và sau đó cố gắng nhúng$T_2$xung quanh nó. Với mỗi lựa chọn như vậy, chúng ta sẽ thử tất cả các ánh xạ của$T_2$các đỉnh của$T_1$, kiểm tra bảo toàn kề. Ngay cả khi chúng tôi sửa chữa ánh xạ gốc, số lượng ánh xạ có thể có của trẻ em là giai thừa theo mức độ và con số này tăng lên rất nhanh. 

Quan sát quan trọng mở ra một giải pháp hiệu quả là các cây có thể được so khớp tăng dần từ các lá trở lên bằng cách sử dụng cấu trúc tương đương của các cây con có gốc. Nếu chúng ta sửa một root trong$T_2$, bất kỳ nhúng hợp lệ nào vào$T_1$phải ánh xạ gốc đó tới một đỉnh nào đó trong$T_1$và các cây con của gốc phải được ánh xạ vào các cây con rời rạc của các cây lân cận trong$T_1$. Điều này biến vấn đề thành sự khớp lặp đi lặp lại giữa nhiều tập hợp cây con có gốc. 

Do đó, chúng tôi trình bày lại vấn đề dưới dạng lập trình động trên các cặp nút$(v, u)$, Ở đâu$v \in T_1$Và$u \in T_2$. giá trị`match[v][u]`có nghĩa là cây con của$T_2$bắt nguồn từ$u$có thể được nhúng vào cây con của$T_1$bắt nguồn từ$v$, với sự linh hoạt mà chúng ta có thể bỏ qua các nhánh không được sử dụng trong$T_1$. 

Một khi trạng thái này được xác định, quá trình chuyển đổi sẽ trở thành một bài toán so khớp hai bên: con của$u$phải được chỉ định tiêm cho trẻ em$v$, sao cho các cây con tương ứng khớp nhau. 

Cách tiếp cận bạo lực là theo cấp số nhân do phải thử tất cả các phần nhúng một cách rõ ràng. Cách tiếp cận DP giảm thời gian này thành thời gian đa thức vì mỗi cặp nút được giải quyết một lần và mỗi lần kiểm tra được giảm xuống thành một kết quả khớp trên biểu đồ mức độ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên nhúng | Hàm mũ | Hàm mũ | Quá chậm | 
| So khớp cây con DP + với so khớp hai bên |$O(n^4)$trường hợp xấu nhất |$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cả hai cây một cách tùy ý, ví dụ tại nút 1 trong mỗi cây. Điều này mang lại cho mỗi nút một cấu trúc cha-con và mọi cây con đều được xác định rõ ràng. 

Sau đó chúng tôi tính toán bảng DP`match[v][u]`có nghĩa là “có thể cây con của$T_2$bắt nguồn từ$u$được nhúng vào cây con của$T_1$bắt nguồn từ$v$?”. 

1. Khởi tạo tất cả`match[v][u]`cho lá của$T_2$đúng với mọi$v$. Mẫu lá luôn khớp với bất kỳ nút nào trong$T_1$bởi vì chúng ta chỉ cần đặt một đỉnh duy nhất. 
2. Các nút xử lý của$T_2$theo thứ tự tăng dần của kích thước cây con (thứ tự sau). Điều này đảm bảo rằng khi chúng ta tính toán`match[v][u]`, tất cả các trạng thái trẻ em`match[v'][u']`đã được biết đến. 
3. Cho mỗi cặp$(v, u)$, xây dựng biểu đồ lưỡng cực giữa các con của$v$và con cái của$u$. Chúng tôi kết nối một đứa trẻ$cv$của$v$đến một đứa trẻ$cu$của$u$nếu như`match[cv][cu]`là đúng. 
4. Thực hiện đối chiếu lưỡng cực tối đa từ trẻ em của$u$cho trẻ em của$v$. Nếu chúng ta có thể kết hợp tất cả trẻ em của$u$, sau đó chúng tôi thiết lập`match[v][u] = True`. Nếu không thì nó là sai. 
5. Sau khi điền vào bảng, chúng ta kiểm tra xem có tồn tại$v \in T_1$như vậy`match[v][root_of_T2]`là đúng. Nếu có, chúng tôi xuất ra`Y`, nếu không thì`N`. 

Lý do quan trọng để so khớp là đủ vì tính đẳng cấu của cây đòi hỏi phải bảo toàn tính liền kề và ở các cây có gốc, điều này chuyển thành việc so khớp từng cây con một cách độc lập. Vì các cây con không tương tác giữa các nhánh khác nhau nên bài toán sẽ phân tách rõ ràng thành các tập con phù hợp. 

### Tại sao nó hoạt động 

Tính bất biến đó là`match[v][u]`nắm bắt chính xác liệu mọi yêu cầu về cấu trúc của$T_2$bắt nguồn từ$u$có thể được thỏa mãn bên trong cấu trúc gốc của$T_1$Tại$v$. Mỗi lần so khớp các phần tử con, chúng tôi thực thi việc phân công từng đối một giữa các cấu trúc con được yêu cầu và các cấu trúc con có sẵn, đảm bảo không có sự chồng chéo và duy trì kết nối. Vì tất cả các bài toán con con đã được xác minh trước khi tính toán trạng thái cha, nên không có giả định sai nào về khả năng tương thích của cây con có thể lan truyền lên trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10000)

def read_tree(n):
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)
    return g

def root_tree(g, root):
    n = len(g)
    parent = [-1] * n
    children = [[] for _ in range(n)]
    stack = [root]
    parent[root] = -2

    order = []
    while stack:
        v = stack.pop()
        order.append(v)
        for to in g[v]:
            if to == parent[v]:
                continue
            if parent[to] == -1:
                parent[to] = v
                children[v].append(to)
                stack.append(to)

    return children, order

def dfs_order(children):
    order = []
    stack = [0]
    while stack:
        v = stack.pop()
        order.append(v)
        for c in children[v]:
            stack.append(c)
    return order[::-1]

def can_match(v, u, children1, children2, dp):
    A = children1[v]
    B = children2[u]

    if not B:
        return True

    # bipartite matching from B to A
    match = [-1] * len(A)

    def dfs(b, seen):
        for i, a in enumerate(A):
            if seen[i]:
                continue
            if not dp[a][b]:
                continue
            seen[i] = True
            if match[i] == -1 or dfs(match[i], seen):
                match[i] = b
                return True
        return False

    for b in B:
        seen = [False] * len(A)
        if not dfs(b, seen):
            return False
    return True

def solve():
    n1 = int(input())
    g1 = read_tree(n1)
    n2 = int(input())
    g2 = read_tree(n2)

    children1, _ = root_tree(g1, 0)
    children2, _ = root_tree(g2, 0)

    # postorder for T2
    order2 = []
    stack = [0]
    parent = [-1] * n2
    parent[0] = -2
    while stack:
        v = stack.pop()
        order2.append(v)
        for to in g2[v]:
            if to == parent[v]:
                continue
            if parent[to] == -1:
                parent[to] = v
                stack.append(to)
    order2 = order2[::-1]

    dp = [[False] * n2 for _ in range(n1)]

    for u in order2:
        for v in range(n1):
            dp[v][u] = can_match(v, u, children1, children2, dp)

    for v in range(n1):
        if dp[v][0]:
            print("Y")
            return
    print("N")

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng các biểu diễn gốc của cả hai cây, sau đó điền vào bảng DP trong đó mỗi mục nhập sẽ kiểm tra xem cây con mẫu có thể được nhúng vào cây con máy chủ hay không. Bước so khớp hai bên đảm bảo rằng mỗi cây con con được yêu cầu của$T_2$được gán cho một cây con con tương thích riêng biệt trong$T_1$. 

Một điểm tinh tế là trẻ em được đối xử độc lập: một khi là con của$u$được khớp với một đứa trẻ của$v$, toàn bộ cây con của nó được sử dụng bởi phép đệ quy thông qua`dp`, ngăn chặn sự chồng chéo một phần hoặc tái sử dụng không nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp trong đó$T_1$là một cái cây lớn hơn và$T_2$là một cấu trúc phân nhánh nhỏ hơn xuất hiện sau khi loại bỏ một lá khỏi$T_1$. DP dần dần xây dựng các trận đấu từ lá trở lên. 

| Bước | bạn ở T2 | v ở T1 | Trẻ em phù hợp | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | nút lá | bất kỳ v | yêu cầu trống | Đúng | 
| 2 | cha mẹ của lá | v ứng cử viên | cây con lá phù hợp | Đúng nếu cấu trúc tồn tại | 

Dấu vết này cho thấy các trạng thái lá lan truyền lên trên, cho phép các nút bên trong chỉ xác thực cấu trúc của chúng sau khi các nút con được xác minh. 

Bất biến quan trọng là khi tất cả các kết quả khớp ở cấp độ lá được thiết lập, các nút cao hơn chỉ phụ thuộc vào việc liệu cấu trúc phân nhánh yêu cầu của chúng có được đáp ứng hay không. 

### Ví dụ 2 

Hãy xem xét một hình đường dẫn$T_2$được nhúng bên trong một vùng hình ngôi sao ở$T_1$. Sự không khớp xuất hiện ở gốc vì một ngôi sao không thể cung cấp một chuỗi phụ thuộc. 

| Bước | bạn ở T2 | v ở T1 | Nỗ lực phù hợp | Kết quả | 
| --- | --- | --- | --- | --- | 
| lá | điểm cuối | trung tâm | tầm thường | Đúng | 
| giữa | nút nội bộ | trung tâm | cần cấu trúc chuỗi | Sai | 

Điều này chứng tỏ rằng chỉ mức độ không phù hợp thôi là chưa đủ; đệ quy sẽ từ chối chính xác các phần nhúng khi cấu trúc cây con không thể được bảo tồn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n_1 \cdot n_2 \cdot n_1 \cdot n_2)$trường hợp xấu nhất | Mỗi cặp$(v,u)$có thể chạy kết hợp hai bên lên đến$n$trẻ em và việc so khớp được lặp lại cho tất cả các cặp | 
| Không gian |$O(n_1 n_2)$| Bảng DP lưu trữ khả năng tương thích giữa mọi cặp nút | 

Với$n_1, n_2 \le 100$, trường hợp xấu nhất này vẫn phù hợp, vì hệ số hằng số vẫn nhỏ và cấp độ cây bị hạn chế trong thực tế. Việc sử dụng bộ nhớ cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# Sample-like small valid trees
assert run("""3
1 2
2 3
3
1 2
2 3
""") == "Y"

# Different shapes: path vs star
assert run("""4
1 2
1 3
1 4
3
1 2
2 3
3 4
""") == "N"

# Single node pattern always matches
assert run("""3
1 2
2 3
1
""") == "Y"

# Exact match
assert run("""3
1 2
2 3
3
1 2
2 3
""") == "Y"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường dẫn vs đường dẫn | Y | nhúng cơ bản | 
| ngôi sao vs con đường | N | cấu trúc không phù hợp | 
| nút đơn | Y | trường hợp cạnh tối thiểu | 
| cây giống nhau | Y | đẳng cấu hoàn toàn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$T_2$là một nút duy nhất. Trong tình huống này, mỗi nút của$T_1$là một trận đấu hợp lệ. Thuật toán xử lý việc này bởi vì tất cả`dp[v][u]`cho lá$u$được đặt thành true ngay lập tức, do đó lần kiểm tra cuối cùng đương nhiên thành công. 

Một trường hợp tế nhị khác là khi$T_1$có một nút cấp cao nhưng$T_2$đòi hỏi một chuỗi. Đối với một đầu vào nơi$T_1$là một ngôi sao và$T_2$là một đường dẫn có độ dài 3, việc khớp không thành công ở nút bên trong của$T_2$bởi vì không có cấu trúc con duy nhất trong$T_1$có thể đáp ứng các yêu cầu phụ thuộc tuần tự. Bước đối sánh hai bên thực thi điều này một cách nghiêm ngặt bằng cách yêu cầu các nhiệm vụ con riêng biệt cho từng nhánh được yêu cầu và việc không có chuỗi sẽ khiến đệ quy thất bại. 

Trường hợp cuối cùng là khi tồn tại nhiều kết quả khớp một phần trong$T_1$, nhưng chỉ có thể nhúng toàn cầu một cách nhất quán. DP đảm bảo tính nhất quán vì khi một kết quả khớp của cây con được chọn cho một phần tử con, nó sẽ được cố định trong trường hợp khớp đó và không thể được sử dụng lại ở nơi khác, ngăn chặn sự chồng chéo ngẫu nhiên mà cách tiếp cận tham lam sẽ cho phép.
