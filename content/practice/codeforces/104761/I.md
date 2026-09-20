---
title: "CF 104761I - \u0418\u0433\u0440\u0430 \u043d\u0430 \u0434\u0435\u0440\u0435\u0432\u0435"
description: "Chúng ta có một cây có $n$ đỉnh. Trước tiên, một người chơi chọn một đỉnh $u$, sau đó đối thủ chọn một đỉnh khác $v$. Sau đó, một đỉnh $w$ được chọn ngẫu nhiên một cách thống nhất từ ​​tất cả các đỉnh $n$."
date: "2026-06-29T02:27:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104761
codeforces_index: "I"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Regional Contest"
rating: 0
weight: 104761
solve_time_s: 108
verified: false
draft: false
---

[CF 104761I - \u0418\u0433\u0440\u0430 \u043d\u0430 \u0434\u0435\u0440\u0435\u0432\u0435](https://codeforces.com/problemset/problem/104761/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh. Đầu tiên một người chơi chọn một đỉnh$u$, khi đó đối thủ sẽ chọn một đỉnh khác$v$. Sau đó, một đỉnh$w$được chọn ngẫu nhiên thống nhất từ ​​tất cả$n$đỉnh. 

Kết quả được quyết định bằng cách so sánh khoảng cách trong cây: nếu$w$gần hơn với$u$hơn là$v$, người chơi đầu tiên thắng; nếu nó gần hơn$v$, người chơi thứ hai thắng; nếu không thì đó là hòa. 

Người chơi đầu tiên muốn chọn$u$để ngay cả sau khi người chơi thứ hai phản ứng tối ưu với$v$, xác suất để một đỉnh ngẫu nhiên gần với$u$là càng lớn càng tốt. Người chơi thứ hai là đối thủ và sẽ luôn chọn$v$để giảm thiểu xác suất này. Nếu nhiều đỉnh$u$đạt được xác suất đảm bảo tốt nhất như nhau, chúng ta đưa ra chỉ số nhỏ nhất. 

Kích thước cây trên tất cả các trường hợp thử nghiệm có thể lên tới$2 \cdot 10^5$, do đó, mọi giải pháp về cơ bản đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Điều này loại trừ bất kỳ chiến lược nào thử tất cả các cặp$(u, v)$và đánh giá xác suất một cách rõ ràng, vì điều đó sẽ dẫn đến hành vi bậc hai hoặc tệ hơn trên mỗi cây. 

Một khó khăn tinh tế là xác suất phụ thuộc vào hình học tổng thể: thay đổi$v$ảnh hưởng đến khoảng cách đến tất cả các đỉnh theo cách không cục bộ. Một cách tiếp cận ngây thơ tính toán lại khoảng cách từ cả hai$u$Và$v$vì mỗi cặp sẽ liên tục duyệt toàn bộ cây và thất bại ngay lập tức trên các đầu vào lớn. 

Một cạm bẫy tiềm ẩn khác là giả sử người chơi thứ hai chỉ xem xét những người hàng xóm của$u$. Điều đó không đúng, vì một đỉnh xa$v$có thể dịch chuyển phần lớn của cây đến gần hơn$v$hơn là$u$, đặc biệt là dọc theo những con đường dài. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp khắc phục$u$, sau đó thử tất cả$v \neq u$, tính cho mỗi cặp có bao nhiêu đỉnh thỏa mãn$d(u,w) < d(v,w)$, và nhận trường hợp xấu nhất. Việc tính toán giá trị này cho một cặp yêu cầu suy luận về tất cả các đỉnh, do đó, ngay cả với BFS từ cả hai điểm cuối, mỗi lần đánh giá đều tốn chi phí$O(n)$. Điều này dẫn đến$O(n^3)$trên mọi lựa chọn của$u$, vượt xa giới hạn. 

Quan sát cấu trúc quan trọng là đối với một$u$, đối thủ không cần phải cân bằng cẩn thận khoảng cách trên cây. Thay vào đó, việc họ chọn$v$bên trong một vùng cây vốn đã “xa”$u$. Bằng trực giác, một lần$v$được đặt trong một cây con lớn cách xa$u$, hầu hết các đỉnh trong cây con đó trở nên gần hơn với$v$hơn là$u$, và di chuyển$v$đi sâu hơn vào cây con đó chỉ làm tăng thêm hiệu ứng này. 

Điều này biến bài toán thành điều kiện cân bằng cấu trúc trên cây. Chất lượng của một đỉnh$u$bị chi phối bởi mức độ phân chia cây thành các thành phần một cách đồng đều khi bị loại bỏ. Nếu một thành phần lớn, đối thủ có thể chọn$v$bên trong nó và buộc hầu hết các đỉnh phải ưu tiên$v$. Nếu tất cả các thành phần đều nhỏ, bất kể ở đâu$v$được đặt, nó không thể chiếm ưu thế trong phần lớn các đỉnh. Đây chính xác là khái niệm về trọng tâm. 

Vì vậy, vấn đề trở thành việc tìm trọng tâm của cây, với yêu cầu bổ sung là chọn trọng tâm có chỉ số nhỏ nhất nếu có hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực trên tất cả$(u,v)$cặp |$O(n^3)$|$O(n)$| Quá chậm | 
| Trọng tâm của cây |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây tùy ý, cho thuận tiện về đỉnh$1$. Tính toán kích thước cây con bằng DFS. Điều này cung cấp cho mỗi nút kích thước của thành phần “bên dưới” nó trong cây có gốc. 
2. Đối với mỗi đỉnh$u$, hãy tính kích thước của thành phần được kết nối lớn nhất còn lại nếu$u$được gỡ bỏ. Điều này bao gồm mỗi cây con con của$u$, cộng với “phía cha mẹ” có kích thước$n - \text{subtree}(u)$. 
3. Xác định giá trị tối thiểu có thể có của kích thước thành phần tối đa này trên tất cả các đỉnh. Đỉnh đạt được mức tối thiểu này là trọng tâm. 
4. Nếu nhiều đỉnh đạt được mức tối thiểu như nhau, hãy chọn đỉnh có chỉ số nhỏ nhất. 

Lý do điều này có tác dụng là vì chiến lược tối ưu của đối thủ tập trung khối lượng một cách hiệu quả vào một thành phần của cây so với$u$. Kích thước thành phần tồi tệ nhất xác định số lượng cây có thể bị “bắt”$u$, do đó, việc giảm thiểu kích thước thành phần tối đa đó sẽ trực tiếp tối đa hóa phần đỉnh thuận lợi được đảm bảo của người chơi đầu tiên. 

### Tại sao nó hoạt động 

Đối với một cố định$u$, hãy xem xét bất kỳ sự lựa chọn nào của$v$. Các đỉnh ưa thích$v$tạo thành một vùng luôn chứa toàn bộ thành phần của$v$khi cây bị tách ra ở$u$, ngoại trừ các đỉnh gần ranh giới của đường đi giữa$u$Và$v$. Hiệu ứng ranh giới này không thể lớn hơn thực tế là toàn bộ một thành phần bị thiên về$v$. 

Vì vậy, động thái tốt nhất của đối thủ là chọn$v$bên trong thành phần lớn nhất được tạo bằng cách loại bỏ$u$, bởi vì điều đó tối đa hóa số đỉnh có cấu trúc đường đi ngắn nhất phù hợp$v$. Hiệu suất được đảm bảo của$u$do đó được kiểm soát bởi kích thước thành phần lớn nhất của nó và việc tối đa hóa xác suất thắng trong trường hợp tối thiểu tương đương với việc giảm thiểu đại lượng này, xác định trọng tâm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            a, b = map(int, input().split())
            a -= 1
            b -= 1
            g[a].append(b)
            g[b].append(a)

        parent = [-1] * n
        order = []
        stack = [0]
        parent[0] = -2

        while stack:
            v = stack.pop()
            order.append(v)
            for to in g[v]:
                if to == parent[v]:
                    continue
                parent[to] = v
                stack.append(to)

        sz = [1] * n
        for v in reversed(order):
            for to in g[v]:
                if to == parent[v]:
                    continue
                sz[v] += sz[to]

        best = n + 1
        ans = 0

        for v in range(n):
            mx = n - sz[v]
            for to in g[v]:
                if to == parent[v]:
                    continue
                mx = max(mx, sz[to])
            if mx < best or (mx == best and v < ans):
                best = mx
                ans = v

        print(ans + 1)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ xây dựng cây và chạy truyền tải kiểu DFS để thiết lập mối quan hệ cấp trên và tính toán kích thước cây con. Ngăn xếp lặp được sử dụng thay vì đệ quy để tránh các vấn đề về độ sâu đệ quy trên chuỗi dài. 

Sau đó, mỗi đỉnh được đánh giá là một ứng cử viên trọng tâm tiềm năng. giá trị$n - \text{subtree}(v)$đại diện cho kích thước của thành phần trên$v$, trong khi mỗi cây con đóng góp một thành phần tiềm năng nếu$v$được gỡ bỏ. Mức tối đa trong số này được tính toán và giảm thiểu. 

Một lỗi triển khai phổ biến là quên thành phần phía cha, dẫn đến đánh giá thấp kích thước thành phần tối đa thực sự và chọn không chính xác các nút nằm sâu trong một nhánh lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một cái cây nhỏ có hình dạng như một đường thẳng:$1 - 2 - 3 - 4 - 5$. 

| nút | phía cha mẹ | cây con tối đa | thành phần tối đa | 
| --- | --- | --- | --- | 
| 1 | 4 | 0 | 4 | 
| 2 | 3 | 1 | 3 | 
| 3 | 2 | 2 | 2 | 
| 4 | 3 | 1 | 3 | 
| 5 | 4 | 0 | 4 | 

Kích thước thành phần tối đa tối thiểu là$2$, đạt được tại nút$3$. Nút này chia cây một cách đồng đều nhất nên là lựa chọn khởi đầu tốt nhất cho người chơi đầu tiên. 

### Ví dụ 2 

Xét một ngôi sao có tâm tại$1$với lá$2,3,4,5$. 

| nút | phía cha mẹ | cây con tối đa | thành phần tối đa | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 4 | 0 | 4 | 
| 3 | 4 | 0 | 4 | 
| 4 | 4 | 0 | 4 | 
| 5 | 4 | 0 | 4 | 

nút$1$rõ ràng là tối ưu, vì việc loại bỏ nó chỉ tạo ra các thành phần đơn lẻ. Bất kỳ lựa chọn nào khác đều cho phép đối thủ chiếm được gần như toàn bộ cây bằng cách chọn$v=1$. 

Những ví dụ này cho thấy rằng đỉnh tối ưu được xác định hoàn toàn bằng mức độ cân bằng của việc loại bỏ nó, chứ không phải bởi mức độ cục bộ hoặc vị trí theo thứ tự truyền tải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi cạnh được xử lý một số lần không đổi trong quá trình đánh giá DFS và centroid | 
| Không gian |$O(n)$| Danh sách kề, mảng cha, kích thước cây con | 

Tổng của$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, do đó, một giải pháp tuyến tính trên tất cả các thử nghiệm là đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins
    output = io.StringIO()
    sys.stdout = output

    # assume solution is in solve()
    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# sample
assert run("1\n2\n1 2\n") in ["1", "2"]

# star
assert run("1\n5\n1 2\n1 3\n1 4\n1 5\n") == "1"

# line
assert run("1\n4\n1 2\n2 3\n3 4\n") == "2"

# balanced tree
assert run("1\n7\n1 2\n1 3\n2 4\n2 5\n3 6\n3 7\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngôi sao | 1 | centroid tại trung tâm trong cấu trúc rất mất cân bằng | 
| dòng | 2 | xử lý cây hình đường đi đúng cách | 
| cây cân bằng | 1 | sự đúng đắn và tính đúng đắn của trọng tâm | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một chuỗi dài trong đó trọng tâm không phải là duy nhất. Đối với đường đi có độ dài chẵn, cả hai đỉnh trung tâm đều giảm thiểu kích thước thành phần tối đa. Thuật toán xử lý việc này một cách chính xác bằng cách chọn chỉ số nhỏ nhất trong số đó. 

Một trường hợp khác là biểu đồ hình sao trong đó tất cả các lá trông đối xứng ngoại trừ thứ tự chỉ số. Việc tính toán đảm bảo tất cả các lá đều có thành phần tối đa lớn, trong khi tâm có giá trị tối thiểu, do đó tâm luôn được chọn bất kể lập chỉ mục. 

Cuối cùng, các cây lệch với một chuỗi sâu được gắn vào một cây con dày đặc vẫn hoạt động chính xác vì kích thước thành phần phía cha mẹ nắm bắt toàn bộ trọng lượng của nhánh nặng, ngăn việc chọn các nút sâu được chọn không chính xác.
