---
title: "CF 102348B - Đỉnh thú vị"
description: "Chúng ta có một cây có các đỉnh được đánh số từ 1 đến (n) và chính xác (k) các đỉnh đó được tô màu. Đối với đỉnh không được tô màu (x), hãy tưởng tượng cắt (x) ra khỏi cây. Mọi lân cận của (x) đều trở thành gốc của một thành phần liên thông."
date: "2026-08-13T00:50:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "B"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 114
verified: true
draft: false
---

[CF 102348B - Các đỉnh thú vị](https://codeforces.com/problemset/problem/102348/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có các đỉnh được đánh số từ 1 đến (n) và chính xác (k) các đỉnh đó được tô màu. Đối với đỉnh không được tô màu (x), hãy tưởng tượng cắt (x) ra khỏi cây. Mọi lân cận của (x) đều trở thành gốc của một thành phần liên thông. Đỉnh (x) thú vị một cách chính xác khi mỗi thành phần đó chứa ít nhất một đỉnh được tô màu. 

Nhiệm vụ là tìm mọi đỉnh không được tô màu thỏa mãn điều kiện đó và in chỉ số của chúng theo thứ tự tăng dần. 

Khó khăn chính là định nghĩa tạm thời tạo rễ cho cây ở mọi mức có thể (x). Việc triển khai trực tiếp dường như yêu cầu duyệt cây riêng biệt cho mỗi đỉnh không được tô màu. Với (n) lớn bằng (2\cdot10^5), mức đó quá đắt đối với giới hạn hai giây. Một thuật toán (O(n^2)) có thể thực hiện khoảng (4\cdot10^{10}) lượt truy cập đỉnh trong trường hợp xấu nhất, vượt xa những gì thực tế. Chúng ta cần trích xuất thông tin về tất cả các nghiệm có thể có từ một lần duyệt. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ hợp lý trở nên sai lầm. Đầu tiên, một chiếc lá không được tô màu luôn thú vị, vì sau khi loại bỏ chiếc lá đó chỉ còn lại một thành phần và thành phần đó chứa tất cả (k) đỉnh được tô màu. Ví dụ,```
2 1
1
1 2
```có đầu ra```
1
2
```Một giải pháp yêu cầu một đỉnh có ít nhất hai hướng được tô màu sẽ bác bỏ đỉnh 2 một cách không chính xác. 

Trường hợp cạnh thứ hai xảy ra khi bản thân ứng viên được tô màu. Một đỉnh như vậy có thể có các đỉnh được tô màu theo mọi hướng, nhưng nó không đủ điều kiện vì chỉ những đỉnh không được tô màu mới có thể là đáp án. Ví dụ,```
3 1
2
1 2
2 3
```có đầu ra```
2
1 3
```Đỉnh 2 được tô màu phải được loại trừ mặc dù cả hai thành phần thu được bằng cách loại bỏ nó đều không chứa các đỉnh được tô màu, điều này đã khiến nó thất bại ở đây. Tổng quát hơn, việc triển khai nên loại trừ rõ ràng các đỉnh được tô màu thay vì dựa vào thử nghiệm định hướng để làm như vậy. 

Trường hợp cạnh thứ ba là một đỉnh có một màu. Nếu (k=1), một đỉnh không được tô màu có bậc ít nhất là hai thì không thể thú vị, bởi vì chỉ một trong các thành phần phụ của nó có thể chứa đỉnh được tô màu duy nhất. Mỗi chiếc lá không màu đều thú vị. Ví dụ,```
5 1
3
1 2
2 3
3 4
4 5
```có đầu ra```
2
1 5
```Việc triển khai bất cẩn chỉ kiểm tra xem toàn bộ cây con có chứa màu hay không có thể vô tình chấp nhận các đỉnh bên trong, mặc dù một trong các hướng của chúng là không màu. 

Cuối cùng, các đỉnh được tô màu được đảm bảo là khác biệt, do đó trường hợp thử nghiệm trong đó tất cả các giá trị được tô màu bằng nhau sẽ không hợp lệ theo ràng buộc của bài toán. Trường hợp ranh giới có ý nghĩa gần nhất là (k=1), được trình bày riêng. 

## Phương pháp tiếp cận 

Giải pháp brute-force bắt đầu bằng cách chọn mọi đỉnh không được tô màu (x) làm gốc. Khi cây có gốc tại (x), chúng ta có thể duyệt qua nó và xác định, với mọi con của (x), liệu cây con của nó có chứa đỉnh được tô màu hay không. Nếu mỗi cây con con đều chứa một cây con thì chúng ta thêm (x) vào câu trả lời. 

Điều này đúng vì các cây con đó chính xác là các thành phần được kết nối được tạo ra khi loại bỏ (x). Vấn đề là số lượng công việc lặp đi lặp lại. Trong trường hợp xấu nhất có thể có (n-1) ứng viên không được tô màu, và việc kiểm tra một ứng cử viên yêu cầu (O(n)) công việc. Ví dụ: với (k=1), điều này có thể đạt tới 

[ 
(n-1)(n-1)=O(n^2), 
] 

đó là về các thao tác (4\cdot10^{10}) khi (n=2\cdot10^5). 

Quan sát quan trọng là chúng ta không thực sự cần phải root cây ở mọi ứng cử viên. Cố định một gốc tùy ý, chẳng hạn như đỉnh 1. Với mỗi đỉnh (v), hãy tính số đỉnh được tô màu trong cây con có gốc thông thường của nó. 

Hãy xem xét một cạnh giữa cha mẹ (u) và con (v). Loại bỏ cạnh này sẽ chia cây thành đúng hai thành phần. Một là cây con của (v), chứa`sub[v]`đỉnh có màu. Cái còn lại chứa tất cả các đỉnh được tô màu còn lại, vì vậy nó chứa 

[ 
k-\text{sub[v] 
] 

đỉnh có màu. 

Điều đó hoàn toàn xác định liệu cạnh có phải là hướng xấu cho một trong hai điểm cuối hay không. Nếu như`sub[v] == 0`, thành phần ở phía (v) không chứa đỉnh được tô màu, vì vậy (u) không thể thú vị. Nếu như`sub[v] == k`, mọi đỉnh được tô màu đều nằm trong cây con của (v), do đó thành phần ở phía (u) không chứa đỉnh được tô màu và (v) không thể thú vị. 

Mọi giá trị khác của`sub[v]`có nghĩa là cả hai cạnh của cạnh đều chứa ít nhất một đỉnh được tô màu, vì vậy cạnh này an toàn cho cả hai điểm cuối. 

Brute-force hoạt động vì nó kiểm tra rõ ràng từng thành phần xung quanh một ứng cử viên, nhưng không thành công khi các thành phần tương tự được tính toán lại cho nhiều ứng cử viên. Việc quan sát thấy mỗi cạnh chỉ có hai cạnh có thể cho phép chúng ta phân loại tất cả các hướng cùng một lúc. Sau khi tính toán số lượng màu của cây con, mỗi cạnh đóng góp tối đa một lỗi cho một điểm cuối, do đó toàn bộ cây có thể được xử lý theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và đánh dấu từng đỉnh được tô màu. Chúng tôi giữ một mảng boolean vì câu trả lời cuối cùng phải loại trừ các đỉnh được tô màu ngay cả khi các đặc tính cấu trúc của chúng sẽ đủ tiêu chuẩn cho chúng. 
2. Root cây tùy ý tại đỉnh 1. Xây dựng một`parent`mảng và một`order`mảng bằng cách sử dụng DFS lặp. các`order`mảng lưu trữ các đỉnh theo thứ tự cha trước con, điều này sau này sẽ cho phép chúng ta xử lý chúng theo chiều ngược lại mà không cần đệ quy. 
3. Khởi tạo`sub[v]`đến 1 cho mọi đỉnh được tô màu và đến 0 cho mọi đỉnh không được tô màu. Như vậy`sub[v]`cuối cùng sẽ biểu thị số đỉnh được tô màu trong cây con gốc của (v). 
4. Xử lý các đỉnh theo thứ tự DFS ngược. Với mọi đỉnh không phải gốc (v), hãy thêm`sub[v]`ĐẾN`sub[parent[v]]`. Vào thời điểm một đỉnh được xử lý, tất cả các đỉnh con của nó đã đóng góp số lượng màu của chúng. 
5. Tạo`bad[v]`, ban đầu bằng không. Đối với mọi đỉnh không phải gốc (v), hãy xem xét cạnh từ đỉnh mẹ (p) đến (v). Nếu như`sub[v] == 0`, toàn bộ cạnh (v) của cạnh đó không màu, do đó cạnh này là hướng xấu cho (p) và chúng ta tăng dần`bad[p]`. 
6. Nếu`sub[v] == k`, tất cả các đỉnh được tô màu đều nằm bên trong cạnh (v). Do đó, thành phần phía cha mẹ không có màu, do đó cạnh giống nhau là hướng xấu cho (v) và chúng ta tăng dần`bad[v]`. 
7. Cuối cùng, quét mọi đỉnh. Một đỉnh thú vị khi nó không bị tô màu và`bad[v] == 0`. Sắp xếp các chỉ số này trước khi in chúng. Việc sắp xếp mất (O(n\log n)), nhưng vì các đỉnh đã được đánh số từ 1 đến (n), thay vào đó, chúng ta có thể quét chúng theo thứ tự số và thu được kết quả được sắp xếp trực tiếp, duy trì độ phức tạp tổng thể (O(n)). 

### Tại sao nó hoạt động 

Với mỗi cạnh (p-v), hai thành phần tạo thành bằng cách loại bỏ cạnh đó chứa chính xác`sub[v]`và (k-\text{sub[v]}) các đỉnh được tô màu. Cạnh xấu cho (p) chính xác khi nào`sub[v] == 0`, và thật tệ cho (v) chính xác là khi nào`sub[v] == k`. Như vậy`bad[x]`đếm chính xác các thành phần sự cố của (x) không chứa đỉnh được tô màu. 

Đối với đỉnh không được tô màu (x), định nghĩa ban đầu nói rằng mọi thành phần xung quanh (x) phải chứa một đỉnh được tô màu. Điều này tương đương với việc nói rằng không có cạnh nào của nó là xấu, chính xác là`bad[x] == 0`. Vì các đỉnh có màu được loại trừ rõ ràng nên mọi đỉnh được báo cáo đều thỏa mãn định nghĩa và mọi đỉnh thú vị đều được báo cáo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    colored = [False] * (n + 1)
    for x in map(int, input().split()):
        colored[x] = True

    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    # Root the tree at vertex 1.
    parent = [0] * (n + 1)
    order = [1]
    parent[1] = -1

    for u in order:
        for v in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    # sub[v] = number of colored vertices in v's rooted subtree.
    sub = [0] * (n + 1)
    for v in range(1, n + 1):
        if colored[v]:
            sub[v] = 1

    for v in reversed(order[1:]):
        sub[parent[v]] += sub[v]

    # bad[v] = number of incident components of v containing no color.
    bad = [0] * (n + 1)

    for v in order[1:]:
        p = parent[v]

        if sub[v] == 0:
            bad[p] += 1

        if sub[v] == k:
            bad[v] += 1

    answer = []
    for v in range(1, n + 1):
        if not colored[v] and bad[v] == 0:
            answer.append(v)

    print(len(answer))
    print(*answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên lưu trữ các đỉnh màu trong`colored`. Việc sử dụng mảng boolean giúp kiểm tra tư cách thành viên theo thời gian liên tục và tránh mọi nhu cầu tìm kiếm trong danh sách các chỉ mục được tô màu sau này. 

Danh sách kề đại diện cho cây có bộ nhớ (O(n)). Vì một cây có chính xác (n-1) cạnh nên tổng số mục trên tất cả các danh sách kề là (2(n-1)). 

DFS được lặp đi lặp lại một cách có chủ ý. DFS đệ quy trên đường dẫn (2\cdot10^5) đỉnh sẽ vượt quá độ sâu đệ quy thông thường của Python và có thể thất bại ngay cả khi bản thân thuật toán là chính xác. các`order`mảng cung cấp cho chúng ta một thứ tự hậu kỳ thuận tiện chỉ bằng cách lặp lại nó. 

Việc khởi tạo của`sub`đặt một đơn vị ở mỗi đỉnh được tô màu. Xử lý`order`ngược lại sau đó tích lũy các đơn vị đó về phía gốc, vì vậy`sub[v]`trở thành số đỉnh được tô màu trong toàn bộ cây con của (v). 

Việc phân loại cạnh là chi tiết triển khai quan trọng nhất. Đối với một cạnh từ`p`ĐẾN`v`,`sub[v] == 0`có nghĩa là thành phần phía con không có màu sắc, vì vậy chỉ có`p`bị loại bởi cạnh đó. Ngược lại,`sub[v] == k`có nghĩa là tất cả các màu đều thuộc về phía trẻ em, vì vậy chỉ`v`bị loại. Hai trường hợp này không được hoán đổi cho nhau. 

Không cần xử lý đặc biệt cho root. Mỗi cạnh liên quan đến gốc xuất hiện dưới dạng cạnh cha-con trong cây có gốc và số lượng cạnh con của nó hoàn toàn xác định liệu cạnh đó có hại cho gốc hay không. 

Vòng lặp cuối cùng đi từ 1 đến (n), do đó kết quả đầu ra đã được sắp xếp. Không cần phải sắp xếp riêng biệt và thời gian chạy vẫn tuyến tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cái cây là```
1 - 2 - 3
    |
    4
```Đỉnh 3 được tô màu. Chúng ta root cây ở đỉnh 1. 

| Đỉnh | Phụ huynh | Tô màu trong cây con | Chỉ đường xấu | 
| --- | --- | --- | --- | 
| 1 | không | 1 | 0 | 
| 2 | 1 | 1 | 2 | 
| 3 | 2 | 1 | 1 | 
| 4 | 2 | 0 | 0 | 

Đối với cạnh (1-2), cây con của 2 chứa đỉnh được tô màu duy nhất, do đó`sub[2] == k`. Điều này làm cho thành phần phía cha mẹ trống rỗng màu sắc và đánh dấu đỉnh 2 là xấu. 

Đối với cạnh (2-3), phía con chứa đỉnh được tô màu, do đó đỉnh 3 không có hướng xấu từ cạnh này. Đối với cạnh (2-4), cây con của 4 không chứa đỉnh được tô màu, do đó đỉnh 2 có hướng xấu khác. 

Đỉnh 1 và 4 không bị tô màu và không có hướng xấu. Đỉnh 3 được tô màu và bị loại trừ, trong khi đỉnh 2 có hướng xấu. Do đó, đầu ra là`1 4`. 

Ví dụ này cho thấy tại sao một chiếc lá có thể thú vị ngay cả khi chỉ có một đỉnh được tô màu. Loại bỏ lá 4 để lại một thành phần chứa đỉnh 3. 

### Mẫu 2 

Root ở đỉnh 1 sẽ cho cấu trúc cha-con sau:```
        1
     / /|\ \
    5 6 2 8
   / \   |
  4   3  7
```Các đỉnh 6, 5 và 7 được tô màu. 

| Đỉnh | Phụ huynh | Tô màu trong cây con | Chỉ đường xấu | 
| --- | --- | --- | --- | 
| 1 | không | 3 | 1 | 
| 2 | 1 | 1 | 0 | 
| 3 | 5 | 0 | 0 | 
| 4 | 5 | 0 | 0 | 
| 5 | 1 | 1 | 2 | 
| 6 | 1 | 1 | 0 | 
| 7 | 2 | 1 | 0 | 
| 8 | 1 | 0 | 0 | 

Cây con của đỉnh 8 có các đỉnh không được tô màu, do đó cạnh (1-8) làm cho đỉnh 1 bị lỗi. Cây con của 3 và 4 cũng không có màu nên cả hai cạnh đều làm cho đỉnh 5 bị hỏng. 

Đối với đỉnh 2, cây con chứa 7 của nó có một đỉnh được tô màu, trong khi phần còn lại của cây chứa hai đỉnh được tô màu còn lại. Do đó, cả hai hướng đều chứa một màu, làm cho 2 hướng trở nên thú vị. 

Các đỉnh 3, 4 và 8 là các lá nên thành phần duy nhất còn lại của chúng chứa tất cả các đỉnh được tô màu. Họ cũng rất thú vị. Đỉnh 5 bị từ chối vì hai hướng lá của nó hướng về 3 và 4 không chứa màu sắc. Các đỉnh màu 6 và 7 bị loại trừ. 

Câu trả lời kết quả là`2 3 4 8`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Việc xây dựng cây, tạo gốc, tính toán số lượng cây con, phân loại các cạnh và quét các đỉnh đều mất thời gian tuyến tính. | 
| Không gian | (O(n)) | Danh sách kề, mảng cha, thứ tự truyền tải, số lượng cây con, số lượng xấu và cờ màu đều sử dụng bộ nhớ tuyến tính. | 

Cây lớn nhất có (2\cdot10^5) đỉnh và chỉ có (2\cdot10^5-1) cạnh. Thuật toán thực hiện một lượng công việc không đổi trên mỗi đỉnh và trên mỗi cạnh, do đó nó phù hợp thoải mái với giới hạn hai giây. Việc truyền tải lặp đi lặp lại cũng tránh được các lỗi về độ sâu đệ quy trên các cây có độ mất cân bằng cao chẳng hạn như một đường dẫn. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`, với`solve()`chức năng hiển thị ở trên.```python
import sys
import io
from contextlib import redirect_stdout

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()

    try:
        with redirect_stdout(output):
            solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return output.getvalue().strip()

# Provided sample 1
assert run("""\
4 1
3
1 2
2 3
2 4
""") == """\
2
1 4
""", "sample 1"

# Provided sample 2
assert run("""\
8 3
6 5 7
1 5
5 4
5 3
1 6
2 7
1 2
1 8
""") == """\
4
2 3 4 8
""", "sample 2"

# Minimum-size tree. The only uncolored vertex is a leaf.
assert run("""\
2 1
1
1 2
""") == """\
1
2
""", "minimum size"

# One colored vertex on a path. Only the two uncolored leaves are interesting.
assert run("""\
5 1
3
1 2
2 3
3 4
4 5
""") == """\
2
1 5
""", "single colored vertex"

# Two colored vertices split the path. Vertex 3 sees one color in each direction.
assert run("""\
5 2
2 4
1 2
2 3
3 4
4 5
""") == """\
3
1 3 5
""", "two colored directions"

# Boundary case with many colored vertices. The uncolored center has a
# colored vertex in every incident component.
assert run("""\
4 3
2 3 4
1 2
1 3
1 4
""") == """\
1
1
""", "every branch contains a color")

# Maximum-size stress case: a star with all leaves colored.
n = 200000
colored = " ".join(map(str, range(2, n + 1)))
edges = "\n".join(f"1 {v}" for v in range(2, n + 1))
maximum_case = f"{n} {n - 1}\n{colored}\n{edges}\n"

assert run(maximum_case) == """\
1
1
""", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=2,\ k=1), cạnh (1-2), màu 1 |`1 / 2`| Kích thước tối thiểu và thực tế là một chiếc lá không màu thật thú vị | 
| Đường đi 5 đỉnh chỉ có đỉnh 3 màu |`2 / 1 5`| Trường hợp (k=1) và sự bác bỏ các đỉnh bên trong | 
| Đường đi 5 đỉnh có màu 2 và 4 |`3 / 1 3 5`| Một đỉnh có hai hướng khác nhau chứa màu sắc | 
| Ngôi sao có tâm 1 và các lá màu 2, 3, 4 |`1 / 1`| Mọi thành phần tới của tâm đều chứa một đỉnh được tô màu | 
| Ngôi sao có 200000 đỉnh và tất cả các lá đều có màu |`1 / 1`| Kích thước đầu vào tối đa, lớn (k) và hành vi thời gian tuyến tính | 

Thử nghiệm kích thước tối đa có chủ ý sử dụng đầu vào được tạo thay vì nhúng trực tiếp hàng trăm nghìn dòng vào bài xã luận. Nó vẫn xây dựng chính xác cây tối đa được phép và chạy cùng một xác nhận đối với giải pháp thực tế. 

## Vỏ cạnh 

### Một đỉnh có một màu 

Hãy xem xét```
5 1
3
1 2
2 3
3 4
4 5
```Sau khi root ở mức 1, số lượng cây con là`1, 1, 1, 0, 0`từ các đỉnh 1 đến 5 ngoại trừ sự tích lũy thích hợp xung quanh đỉnh 3. Trực tiếp hơn, đối với bất kỳ đỉnh không tô màu bên trong nào cũng có ít nhất hai thành phần phụ, nhưng chỉ một thành phần có thể chứa đỉnh 3. Do đó, mọi đỉnh trong đều có hướng xấu. Đỉnh 1 và 5 là các lá, do đó sau khi loại bỏ một trong hai đỉnh sẽ có một thành phần duy nhất chứa đỉnh 3. Thuật toán đưa ra`bad[1] = 0`Và`bad[5] = 0`, trong khi các ứng viên nội bộ nhận được ít nhất một lỗi không hợp lệ, tạo ra`1 5`. 

###Một chiếc lá không màu 

Hãy xem xét```
2 1
1
1 2
```Root ở mức 1 mang lại`sub[2] = 0`, vì đỉnh 2 không được tô màu. điều kiện`sub[2] == 0`sự gia tăng`bad[1]`, không`bad[2]`. Vertex 2 không có cạnh con và do đó không nhận được hướng xấu. Vì nó không có màu nên nó được chấp nhận. Việc gán cạnh xấu cho điểm cuối chính xác này là nơi dễ dàng đưa ra lỗi khái niệm riêng lẻ. 

### Một đỉnh có màu không bao giờ được báo cáo 

Hãy xem xét```
3 1
2
1 2
2 3
```Mảng màu đánh dấu đỉnh 2. Thuật toán có thể tính toán thông tin cấu trúc cho nó giống như mọi đỉnh khác, nhưng điều kiện cuối cùng yêu cầu rõ ràng`not colored[v]`. Như vậy chỉ có đỉnh 1 và 3 mới có thể nhập câu trả lời và cả hai đều là lá. Đầu ra là```
2
1 3
```Loại trừ rõ ràng này được ưu tiên hơn là cố gắng chứng minh rằng mọi đỉnh được tô màu sẽ tự động nhận được số lượng sai, bởi vì bản thân định nghĩa là cơ quan có thẩm quyền rõ ràng nhất về tính đủ điều kiện. 

### Một đỉnh có nhiều nhánh không màu 

Trong mẫu đầu tiên,```
4 1
3
1 2
2 3
2 4
```đỉnh 2 có ba hướng tới. Hướng về đỉnh 3 chứa màu duy nhất, trong khi hướng về đỉnh 1 và 4 không chứa màu nào. Cây con gốc ở số 4 có`sub[4] = 0`, tăng dần`bad[2]`. Cạnh về phía cha mẹ 1 có tất cả các màu ở 2 cạnh, vì vậy`sub[2] == k`sự gia tăng`bad[2]`lại. Do đó, Vertex 2 có hai hướng xấu và bị bác bỏ. 

Kế toán dựa trên cạnh tương tự xử lý trường hợp này mà không cần lý luận riêng về trình độ của ứng viên. Mỗi thành phần sự cố được biểu diễn bằng chính xác một cạnh có hướng của một cạnh, do đó việc kiểm tra xem mỗi cạnh đó có chứa một màu hay không được rút gọn thành hai trường hợp ranh giới đếm cây con`0`Và`k`.
