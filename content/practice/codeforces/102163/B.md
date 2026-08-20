---
title: "CF 102163B - Để tôi ngủ"
description: "Ký túc xá có thể được mô hình hóa như một đồ thị vô hướng. Mỗi phòng là một đỉnh và mỗi cổng là một cạnh. Một cổng thông tin hữu ích cho người giám sát chính xác khi việc loại bỏ cạnh đó sẽ ngắt kết nối thành phần được kết nối của nó, đây là định nghĩa tiêu chuẩn của một cây cầu."
date: "2026-08-19T14:38:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "B"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 461
verified: true
draft: false
---

[CF 102163B - Để tôi ngủ](https://codeforces.com/problemset/problem/102163/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ký túc xá có thể được mô hình hóa như một đồ thị vô hướng. Mỗi phòng là một đỉnh và mỗi cổng là một cạnh. Một cổng thông tin hữu ích cho người giám sát chính xác khi việc loại bỏ cạnh đó sẽ ngắt kết nối thành phần được kết nối của nó, đây là định nghĩa tiêu chuẩn của một cây cầu. 

Hasan có thể thêm chính xác một cổng mới giữa hai phòng. Việc thêm một cạnh có thể phá hủy một số cây cầu hiện có vì cạnh mới có thể tạo ra một chu trình chứa chúng. Mục tiêu là chọn cổng mới sao cho càng ít cầu càng tốt. 

Đầu vào chứa một số biểu đồ độc lập. Đối với mỗi biểu đồ, đầu ra cần thiết là số lượng cầu tối thiểu có thể có sau khi thêm một cạnh. 

Giới hạn cho phép tối đa (10^5) đỉnh và (10^5) cạnh trong một trường hợp thử nghiệm. Trang vấn đề chính thức đưa ra giới hạn thời gian 3 giây và giới hạn bộ nhớ 256 MB. Với kích thước này, thuật toán sẽ xử lý biểu đồ theo thời gian gần như tuyến tính hoặc gần tuyến tính. Việc liệt kê tất cả các cặp đỉnh đã có giá (O(N^2)), tức là khoảng (5\cdot10^9) cặp ở kích thước tối đa, do đó, bất kỳ giải pháp nào kiểm tra từng cổng mới có thể có riêng lẻ đều quá chậm. 

Có một số chi tiết biểu đồ có thể khiến việc triển khai bất cẩn không thành công. Đầu tiên, biểu đồ có thể bị ngắt kết nối. Ví dụ,```
1
4 2
1 2
3 4
```có hai cây cầu, nhưng việc thêm một cạnh giữa các đỉnh từ các thành phần được kết nối khác nhau sẽ không tạo ra một chu trình, vì vậy câu trả lời là`2`. Một giải pháp xử lý toàn bộ cấu trúc cây cầu như một cây có thể kết nối không chính xác hai thành phần và cho rằng cả hai cây cầu đều biến mất. 

Thứ hai, một đồ thị có thể chứa các chu trình. Vì```
1
3 3
1 2
2 3
3 1
```chưa có cây cầu nào nên câu trả lời là`0`. Một giải pháp chỉ đơn giản là đếm các cạnh hoặc giả sử mọi cạnh trong biểu đồ được kết nối là một cây cầu mắc lỗi này. 

Thứ ba, các cạnh lặp lại cần được xử lý chính xác nếu chúng xảy ra. Vì```
1
2 2
1 2
1 2
```không cạnh nào là cầu nối, vì loại bỏ một trong hai vẫn để lại kết nối kia. Câu trả lời là`0`. Thuật toán của Tarjan phải phân biệt hai cạnh vật lý thay vì coi lần xuất hiện thứ hai là cùng một cạnh. 

Tự lặp là một trường hợp vô hại khác nếu đầu vào chứa một vòng lặp. Vì```
1
1 1
1 1
```cạnh không thể ngắt kết nối bất cứ thứ gì, vì vậy câu trả lời là`0`. Việc triển khai bên dưới xử lý nó một cách tự nhiên. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp sẽ thử dùng mọi cặp phòng có thể làm điểm cuối của cổng mới. Đối với mỗi cặp, hãy thêm cạnh một cách khái niệm, chạy thuật toán tìm cầu, đếm các cầu còn lại và giữ mức tối thiểu. Điều này đúng vì mọi lựa chọn hợp pháp của cổng thông tin mới đều được đánh giá rõ ràng. 

Vấn đề là số lượng lựa chọn. Có các cặp (O(N^2)) và việc tìm tất cả các cầu có chi phí (O(N+M)), do đó tổng độ phức tạp là (O(N^2(N+M))). Với (N=M=10^5), đó là thứ tự của các phép toán đồ thị (10^{15}), điều này gần như không khả thi. 

Quan sát hữu ích là việc thêm một cạnh chỉ có thể làm cho các cạnh trên một đường dẫn cụ thể không còn là cầu nối. Giả sử hai phòng đã được kết nối. Có một đường dẫn duy nhất giữa các thành phần được kết nối hai cạnh của chúng sau khi tất cả các cây cầu hiện có được thu gọn lại. Việc thêm cạnh mới sẽ tạo ra một chu trình bao gồm cạnh mới và đường dẫn đó. Mỗi cây cầu trên đường bây giờ là một phần của chu kỳ và không còn là cây cầu nữa. Những cây cầu bên ngoài lối đi vẫn là những cây cầu. 

Điều này gợi ý việc nén mọi vùng tối đa không chứa cầu nối vào một thành phần. Sau quá trình nén này, mỗi cạnh còn lại là một cây cầu và cấu trúc thu được là một khu rừng. Trong mỗi cây của khu rừng này, việc thêm một cạnh giữa hai thành phần sẽ loại bỏ chính xác các cạnh của cây cầu trên đường dẫn duy nhất của chúng. 

Vậy là vấn đề trở nên đơn giản hơn nhiều. Gọi tổng số cầu là (B). Nếu con đường dài nhất trong rừng cầu có chiều dài (D), chúng ta có thể thêm một cổng giữa các phòng thuộc hai điểm cuối của con đường đó và loại bỏ chính xác các cây cầu (D). Câu trả lời là do đó 

[ 
B-D. 
] 

Nếu biểu đồ ban đầu bị ngắt kết nối, cấu trúc nén sẽ là một khu rừng chứ không phải một cây. Cạnh giữa hai cây khác nhau không thể loại bỏ bất kỳ cây cầu cũ nào, vì vậy chúng tôi lấy đường kính tối đa trên tất cả các cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2(N+M))) | (O(N+M)) | Quá chậm | 
| Tối ưu | (O(N+M)) | (O(N+M)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ biểu đồ vô hướng với ID cạnh rõ ràng cho mỗi cổng. Việc sử dụng Edge ID là cần thiết vì hai cổng khác nhau có thể kết nối cùng một cặp phòng. 
2. Chạy thuật toán cầu Tarjan. Đối với mỗi đỉnh, duy trì thời gian khám phá của nó`tin`và thời gian khám phá có thể truy cập thấp nhất`low`. Đối với cạnh cây DFS từ (u) đến (v), cạnh đó là cầu chính xác khi`low[v] > tin[u]`. 

Việc triển khai sử dụng ngăn xếp rõ ràng thay vì DFS đệ quy. Với (10^5) đỉnh, DFS Python đệ quy có thể đạt giới hạn đệ quy hoặc phát sinh chi phí không cần thiết, trong khi phiên bản lặp vẫn giữ nguyên logic Tarjan. 
3. Bỏ qua mọi cây cầu và tìm các thành phần liên thông của đồ thị còn lại. Mỗi thành phần như vậy là một thành phần được kết nối hai cạnh, nghĩa là không một cạnh nào bên trong nó có thể ngắt kết nối thành phần đó. 
4. Cung cấp cho mỗi thành phần được kết nối hai cạnh một ID đỉnh được nén. Mỗi cây cầu ban đầu kết nối hai thành phần nén khác nhau. 
5. Xây dựng một biểu đồ mới chỉ chứa những cây cầu đó. Vì tất cả các cạnh không phải cầu bên trong đều được thu nhỏ lại nên biểu đồ này là một khu rừng. Mỗi cạnh trong đó đại diện chính xác cho một cây cầu ban đầu. 
6. Đếm tất cả các cạnh cầu. Gọi giá trị này là (B). 
7. Với mỗi cây trong rừng cầu, hãy tính đường kính của nó. Bởi vì tất cả các cạnh được nén đều có chiều dài đơn vị, đường kính đơn giản là số cạnh cầu tối đa trên một đường dẫn. 
8. Phương pháp duyệt hai chiều tiêu chuẩn tìm đường kính của mỗi cây. Bắt đầu từ một đỉnh tùy ý và tìm đỉnh xa nhất (a). Bắt đầu từ (a), tìm đỉnh xa nhất (b). Khoảng cách từ (a) đến (b) là đường kính của cây. 
9. Giữ đường kính lớn nhất (D) trong số tất cả các cây. Việc kết nối các phòng từ hai thành phần điểm cuối của đường dẫn này sẽ tạo ra một chu trình chứa chính xác những cây cầu (D) đó, do đó những cây cầu đó sẽ biến mất. 
10. Trở về`total_bridges - maximum_diameter`. 

**Tại sao nó hoạt động.** Sau khi tất cả các cạnh không phải cầu được co lại, mọi cạnh còn lại là một cây cầu và biểu đồ nén là một khu rừng. Việc thêm một cạnh bên trong thành phần được kết nối sẽ tạo ra chính xác một chu trình, bao gồm cạnh mới và đường dẫn duy nhất giữa các điểm cuối của nó trong cây nén. Chính xác là các mép cầu trên con đường đó không còn là cầu nữa, trong khi mọi cây cầu khác vẫn là cầu. Như vậy số lượng cầu bị loại bỏ chính xác là độ dài đường đi. Con đường tốt nhất có thể là đường kính, vì vậy trừ đi đường kính rừng tối đa từ số lượng cầu ban đầu sẽ cho số lượng cầu còn lại tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    answers = []

    for _ in range(t):
        n, m = map(int, input().split())

        adj = [[] for _ in range(n)]
        edges = []

        for eid in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            edges.append((u, v))
            adj[u].append((v, eid))
            adj[v].append((u, eid))

        tin = [-1] * n
        low = [0] * n
        parent = [-1] * n
        parent_edge = [-1] * n
        it = [0] * n
        is_bridge = [False] * m

        timer = 0

        for root in range(n):
            if tin[root] != -1:
                continue

            tin[root] = low[root] = timer
            timer += 1

            stack = [root]

            while stack:
                v = stack[-1]

                if it[v] < len(adj[v]):
                    to, eid = adj[v][it[v]]
                    it[v] += 1

                    if eid == parent_edge[v]:
                        continue

                    if tin[to] == -1:
                        parent[to] = v
                        parent_edge[to] = eid
                        tin[to] = low[to] = timer
                        timer += 1
                        stack.append(to)
                    else:
                        if tin[to] < low[v]:
                            low[v] = tin[to]
                else:
                    stack.pop()
                    p = parent[v]

                    if p != -1:
                        pe = parent_edge[v]

                        if low[v] > tin[p]:
                            is_bridge[pe] = True

                        if low[v] < low[p]:
                            low[p] = low[v]

        comp = [-1] * n
        component_count = 0

        for start in range(n):
            if comp[start] != -1:
                continue

            comp[start] = component_count
            stack = [start]

            while stack:
                v = stack.pop()

                for to, eid in adj[v]:
                    if is_bridge[eid]:
                        continue
                    if comp[to] != -1:
                        continue

                    comp[to] = component_count
                    stack.append(to)

            component_count += 1

        forest = [[] for _ in range(component_count)]
        total_bridges = 0

        for eid, (u, v) in enumerate(edges):
            if not is_bridge[eid]:
                continue

            a = comp[u]
            b = comp[v]

            forest[a].append(b)
            forest[b].append(a)
            total_bridges += 1

        seen = [False] * component_count

        def farthest(start, mark):
            stack = [(start, -1, 0)]
            far_vertex = start
            far_distance = 0

            while stack:
                v, parent_vertex, distance = stack.pop()

                if mark:
                    seen[v] = True

                if distance > far_distance:
                    far_distance = distance
                    far_vertex = v

                for to in forest[v]:
                    if to == parent_vertex:
                        continue
                    stack.append((to, v, distance + 1))

            return far_vertex, far_distance

        maximum_diameter = 0

        for start in range(component_count):
            if seen[start]:
                continue

            endpoint, _ = farthest(start, True)
            _, diameter = farthest(endpoint, False)

            if diameter > maximum_diameter:
                maximum_diameter = diameter

        answers.append(str(total_bridges - maximum_diameter))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Biểu đồ đầu tiên được lưu trữ dưới dạng danh sách kề. Mỗi mục kề kề chứa cả đỉnh lân cận và ID cạnh vật lý, cho phép Tarjan phân biệt chính xác các cạnh song song. 

Tính toán đường truyền đầu tiên`tin`Và`low`. Khi một đứa trẻ DFS`v`kết thúc, điều kiện`low[v] > tin[parent[v]]`xác định cạnh cha là một cây cầu. Bản thân cạnh cha được bỏ qua bằng cách sử dụng ID của nó, thay vì bỏ qua đỉnh cha, đây là chi tiết tinh tế cần thiết cho đa đồ thị. 

Sau khi đã biết các cầu nối, lần truyền tải thứ hai sẽ gán các ID thành phần trong khi từ chối băng qua các cầu nối. Các thành phần này chính xác là các đỉnh của rừng nén. Sau đó, chúng tôi kiểm tra từng cạnh ban đầu một lần và chỉ thêm các cạnh cầu vào`forest`. 

Việc tính toán đường kính sử dụng một ngăn xếp chứa`(vertex, parent, distance)`. Bởi vì`forest`là một khu rừng, việc ghi nhớ cha mẹ là đủ để ngăn chặn việc đi lùi, do đó không cần thêm mảng truy cập mỗi lần truyền tải. Đường duyệt đầu tiên của cây đánh dấu các đỉnh của nó trong`seen`, điều này ngăn cản việc xử lý lại cùng một cây. 

Không có vấn đề tràn số nguyên trong Python. Câu trả lời lớn nhất có thể có nhiều nhất là (10^5), trong khi số nguyên Python vẫn xử lý các kích thước tùy ý. Khoảng cách bắt đầu từ 0, do đó một thành phần nén không có cạnh cầu có đường kính bằng 0 và không đóng góp gì vào số lượng cầu bị loại bỏ. 

## Ví dụ đã hoạt động 

### Trường hợp mẫu 1 

Đồ thị là```
1 -- 2 -- 3
```Cả hai cạnh đều là cầu. Sau khi thu hẹp các vùng không có cầu thì không có gì được thu gọn nên rừng cầu vẫn là một con đường gồm ba thành phần. 

| Giai đoạn | Linh kiện | Cạnh cầu | Tổng số cầu | Đường kính tối đa | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Đồ thị gốc | 3 | 2 | 2 | 2 | 0 | 
| Sau khi chọn điểm cuối 1 và 3 | 3 | 2 | 2 | 2 | 0 | 

Việc thêm một cổng giữa phòng 1 và 3 sẽ tạo ra một chu trình`1-2-3-1`. Cả hai cây cầu ban đầu hiện nằm trên một chiếc xe đạp nên không còn cây cầu nào nữa. Câu trả lời là`2 - 2 = 0`. 

### Trường hợp mẫu 2 

Biểu đồ chứa một hình tam giác ở các phòng 2, 3 và 8. Các phòng 1, 4, 5 và 7 cũng được nối với nhau bằng nhiều đường dẫn, vì vậy tất cả các cạnh đó đều thuộc về một thành phần được kết nối hai cạnh. Cấu trúc cầu còn lại là cây ba lá. 

| Giai đoạn | Linh kiện nén | Cạnh cầu | Tổng số cầu | Đường kính tối đa | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi phát hiện cầu | 4 |`(triangle)-(core)`,`(core)-6`,`(core)-9`| 3 | 2 | 1 | 
| Chọn lá 2 và 6 | 4 | 3 cây cầu gốc | 3 | 2 đã xóa | 1 | 

Ba cây cầu là cổng từ phòng 1 đến phòng 2, cổng từ phòng 4 đến phòng 6 và cổng từ phòng 5 đến phòng 9. Rừng nén là một ngôi sao có tâm là thành phần tuần hoàn lớn và các lá của nó là thành phần tam giác, phòng 6 và phòng 9. 

Con đường dài nhất có hai cây cầu. Nối hai lá tạo thành một vòng chứa hai cây cầu đó, để lại đúng một cây cầu. Do đó đầu ra mẫu là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+M)) | Tarjan, xây dựng thành phần, xây dựng rừng và đường kính, mỗi quá trình xử lý mọi đỉnh và cạnh chỉ với một số lần không đổi | 
| Không gian | (O(N+M)) | Danh sách kề ban đầu, dữ liệu cầu, ID thành phần và nhóm nén đều tuyến tính trong kích thước biểu đồ | 

Với (N,M\le10^5), thuật toán chỉ thực hiện một số lượng nhỏ các lần duyệt đồ thị tuyến tính không đổi. Đây là thang đo phù hợp với giới hạn 3 giây và 256 MB chính thức. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
import sys
import io
from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """\
2
3 2
1 2
2 3
9 11
3 2
3 8
2 8
1 2
1 5
1 4
5 9
7 5
4 5
6 4
4 7
"""

assert run(sample) == "0\n1", "provided sample"

assert run("""\
1
1 1
1 1
""") == "0", "single vertex and self-loop"

assert run("""\
1
4 3
1 2
2 3
3 4
""") == "0", "a path can be closed into one cycle"

assert run("""\
1
2 2
1 2
1 2
""") == "0", "parallel edges are not bridges"

assert run("""\
1
4 2
1 2
3 4
""") == "1", "two disconnected components"

n = 100000
m = 100000
maximum_case = "1\n" + f"{n} {m}\n" + ("1 1\n" * m)
assert run(maximum_case) == "0", "maximum size with repeated self-loops"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức |`0\n1`| Các trường hợp chính, chu trình, cầu nối và biểu đồ bị ngắt kết nối | 
|`1 1`có cạnh`1 1`|`0`| Kích thước tối thiểu và xử lý tự vòng lặp | 
| Con đường`1-2-3-4`|`0`| Đường kính có thể tháo rời mọi cây cầu | 
| Hai cạnh song song |`0`| Xử lý Edge-ID chính xác cho các cạnh lặp lại | 
| Hai thành phần một cạnh bị ngắt kết nối |`1`| Đường kính tối đa phải được lấy qua một khu rừng | 
| (N=M=10^5), tất cả các cạnh`1 1`|`0`| Kích thước đầu vào tối đa và điểm cuối giống hệt nhau lặp đi lặp lại | 

## Vỏ cạnh 

Đối với một biểu đồ bị ngắt kết nối như```
1
4 2
1 2
3 4
```Tarjan coi cả hai cạnh đều là cầu nối. Vì không có cạnh nào không phải là cầu nên mỗi phòng sẽ trở thành thành phần nén riêng và rừng cầu bao gồm hai cây riêng biệt, mỗi cây chứa một cạnh. Mỗi cây có đường kính bằng một nên đường kính tối đa là một. Tổng số cầu là hai, cho`2 - 1 = 1`. Thuật toán không bao giờ kết nối sai hai cây bị ngắt kết nối vì nó chỉ sử dụng một đường dẫn bên trong một thành phần forest. 

Đối với đồ thị đã chứa chu trình,```
1
3 3
1 2
2 3
3 1
```Tarjan không tìm thấy cây cầu nào. Cả ba đỉnh đều được đặt trong cùng một thành phần nén, do đó rừng cầu có một đỉnh cô lập và đường kính tối đa bằng 0. Tổng số cầu cũng bằng 0, đưa ra câu trả lời đúng`0`. 

Đối với các cạnh song song,```
1
2 2
1 2
1 2
```hai cổng có ID cạnh khác nhau. Trong DFS, khi gặp cạnh vật lý thứ hai, nó được coi là cạnh sau thay vì bị nhầm lẫn với cạnh cha của DFS. Do đó, giá trị liên kết thấp giảm xuống đủ để không có cổng nào thỏa mãn điều kiện cầu nối. Biểu đồ nén chứa một thành phần, số lượng cầu bằng 0 và câu trả lời là`0`. 

Đối với vòng lặp tự,```
1
1 1
1 1
```cạnh bắt đầu và kết thúc ở cùng một đỉnh. Nó không thể ngắt kết nối đồ thị nên Tarjan không đánh dấu nó là một cây cầu. Căn phòng duy nhất trở thành một thành phần bị nén, rừng cầu có đường kính bằng 0 và câu trả lời là`0`. 

Trường hợp rõ ràng nhất là một con đường đơn giản,```
1
4 3
1 2
2 3
3 4
```Cả ba cạnh đều là cầu nên rừng nén là con đường có chiều dài bằng ba. Đường kính đầu tiên đi đến một điểm cuối và đường kính thứ hai đến điểm cuối kia ở khoảng cách ba. Thuật toán tính toán`total_bridges = 3`,`maximum_diameter = 3`, và trả về`0`. Việc thêm một cổng giữa phòng 1 và 4 sẽ tạo ra một chu trình chứa mọi cạnh ban đầu, đây chính xác là hoạt động tối ưu.
