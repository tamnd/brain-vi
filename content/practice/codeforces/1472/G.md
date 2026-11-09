---
title: "CF 1472G - Di chuyển về Thủ đô"
description: "Chúng ta có một đồ thị có hướng có các đỉnh là các thành phố và gốc là thành phố 1, thủ đô. Với mọi thành phố $v$, hãy xác định $d[v]$ là độ dài đường đi ngắn nhất từ ​​thành phố 1 đến $v$. Những khoảng cách này được cố định và được biết ngầm từ biểu đồ. Polycarp bắt đầu từ một số thành phố $s$."
date: "2026-06-11T00:45:16+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1472
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 693 (Div. 3)"
rating: 2100
weight: 1472
solve_time_s: 689
verified: false
draft: false
---

[CF 1472G - Di chuyển đến Thủ đô](https://codeforces.com/problemset/problem/1472/G) 

**Đánh giá:** 2100 
**Thẻ:** dfs và tương tự, dp, đồ thị, đường dẫn ngắn nhất 
**Thời gian giải:** 11m 29s 
**Đã xác minh:** không 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta có một đồ thị có hướng có các đỉnh là các thành phố và gốc là thành phố 1, thủ đô. Đối với mỗi thành phố$v$, định nghĩa$d[v]$là độ dài đường đi ngắn nhất từ ​​thành phố 1 đến$v$. Những khoảng cách này được cố định và được biết ngầm từ biểu đồ. 

Polycarp bắt đầu từ một thành phố nào đó$s$. Khi đứng ở thành phố$u$, anh ta có thể di chuyển dọc theo bất kỳ cạnh hướng ra ngoài nào$(u,v)$. Hạn chế là trong số tất cả các nước đi có đích đến không có khoảng cách BFS lớn hơn, nghĩa là$d[v] \le d[u]$, anh ta có thể sử dụng tối đa một chiêu thức như vậy trong toàn bộ hành trình. 

Chuyển sang cấp độ BFS lớn hơn, nghĩa là$d[v] > d[u]$, có thể được sử dụng nhiều lần. 

Đối với mỗi thành phố xuất phát$i$, chúng ta phải xác định giá trị khoảng cách BFS tối thiểu có thể đạt được bởi một số hành trình hợp lệ bắt đầu từ$i$. 

Biểu đồ có thể chứa tới$2 \cdot 10^5$các đỉnh và cạnh trong tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào bắt đầu tìm kiếm đồ thị riêng biệt từ mọi đỉnh sẽ yêu cầu khoảng$O(n(n+m))$làm việc trong trường hợp xấu nhất, vượt xa giới hạn. Tổng kích thước đầu vào gợi ý rõ ràng rằng mỗi cạnh chỉ nên được xử lý một số lần không đổi. 

Một số tình huống làm cho vấn đề trở nên phức tạp hơn so với câu hỏi về đường đi ngắn nhất tiêu chuẩn. 

Coi như:```
1 -> 2 -> 3
^         |
|_________|
```Khoảng cách BFS là$0,1,2$. Bắt đầu từ thành phố 3, chúng ta có thể sử dụng nước đi không tăng duy nhất của mình$3 \to 1$, đạt khoảng cách 0 ngay lập tức. Một giải pháp chỉ đi theo các cạnh hướng tới mức BFS lớn hơn sẽ bỏ lỡ điều này. 

Coi như:```
1 -> 2 -> 3 -> 4
     ^         |
     |_________|
```Chu trình cho phép cải tiến sau này thông qua cạnh sau. Nếu chúng ta xử lý các đỉnh theo thứ tự tùy ý, các giá trị quy hoạch động có thể không đầy đủ khi cần thiết. 

Coi như:```
1 -> 2
1 -> 3
2 -> 4
3 -> 4
4 -> 2
```Đỉnh 4 có cạnh sau so với đỉnh 2. Mặc dù đỉnh 4 nằm sâu hơn trong cây BFS nhưng nó vẫn có thể đạt được khoảng cách 1. Một DFS ngây thơ chỉ truyền thông tin đi xuống sẽ tính ra câu trả lời sai. 

# Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tính toán câu trả lời một cách độc lập cho mọi thành phố bắt đầu. Chúng ta có thể mô hình hóa trạng thái như$(u, used)$, Ở đâu`used`cho biết liệu nước đi không tăng duy nhất đã được sử dụng hay chưa. Từ mỗi đỉnh bắt đầu, chúng tôi chạy tìm kiếm biểu đồ trên các trạng thái này và ghi lại khoảng cách tối thiểu gặp phải. 

Điều này có tác dụng vì biểu đồ trạng thái khớp chính xác với các quy tắc của bài toán. Thật không may là có$2n$trạng thái cho mỗi tìm kiếm và chúng tôi sẽ lặp lại tìm kiếm$n$lần. Sự phức tạp trở nên$O(n(n+m))$, đạt tới khoảng$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Quan sát quan trọng đến từ khoảng cách BFS. 

Cho phép$d[v]$là khoảng cách ngắn nhất từ ​​thành phố 1. Bất kỳ cạnh nào$(u,v)$thuộc về một trong hai loại. 

Nếu như$d[v] > d[u]$, cạnh sẽ tiến về phía trước ở các cấp độ BFS. 

Nếu như$d[v] \le d[u]$, cạnh là loại đặc biệt sử dụng ngoại lệ được phép duy nhất. 

Giả sử chúng ta đứng ở đỉnh$u$. Nếu chúng ta sử dụng ngay một cạnh không tăng$(u,v)$, thì chúng ta có thể ngay lập tức đạt được khoảng cách$d[v]$. Thay vào đó, nếu chúng ta chiếm lợi thế về phía trước$(u,v)$, chúng tôi vẫn giữ lại ngoại lệ cho lần sau, vì vậy câu trả lời tốt nhất có thể truy cập được từ$v$cũng có sẵn từ$u$. 

Điều này cho thấy sự lặp lại của lập trình động. 

Cho phép`ans[u]`là khoảng cách BFS tối thiểu có thể đạt được bắt đầu từ$u$. 

Ban đầu, mọi đỉnh ít nhất có thể dừng lại ngay lập tức, do đó`ans[u] = d[u]`. 

Đối với một cạnh$(u,v)$:

Nếu như$d[v] \le d[u]$, sau đó sử dụng cạnh đó làm ngoại lệ ngay lập tức mang lại giá trị ứng cử viên$d[v]$. 

Nếu như$d[v] > d[u]$, sau đó chuyển sang$v$bảo tồn tất cả các lựa chọn trong tương lai, mang lại giá trị cho ứng viên`ans[v]`. 

Sự tái phát trở thành:$$ans[u] = \min \left( d[u], \min_{d[v]\le d[u]} d[v], \min_{d[v]>d[u]} ans[v] \right)$$Bây giờ đồ thị tạo ra bởi các cạnh thuận là không có tính tuần hoàn đối với khoảng cách BFS vì mọi cạnh như vậy đều tăng nghiêm ngặt$d$. Điều đó có nghĩa là chúng ta có thể tính toán các câu trả lời theo thứ tự giảm dần của khoảng cách BFS hoặc tương đương với bản ghi nhớ DFS xử lý các cấp độ sâu hơn trước. 

Điều này làm giảm toàn bộ tính toán xuống còn một BFS và một DP dựa trên DFS trên biểu đồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n(n+m))$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O(n+m)$|$O(n+m)$| Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Chạy BFS từ thành phố 1 và tính mảng khoảng cách ngắn nhất`dist`. 
2. Tạo mảng DP`ans`và khởi tạo tất cả các giá trị thành`-1`. 
3. Xác định`dfs(u)`là khoảng cách BFS tối thiểu có thể đạt được từ đỉnh`u`. 
4. Bắt đầu với`best = dist[u]`vì Polycarp có thể dừng lại ngay lập tức. 
5. Kiểm tra mọi cạnh đi ra`(u,v)`. 
6. Nếu`dist[v] <= dist[u]`, cạnh này có thể đóng vai trò là nước đi đặc biệt duy nhất. Cập nhật:$$best = \min(best, dist[v])$$Ngoại lệ được sử dụng ngay lập tức, vì vậy chỉ có khoảng cách đích mới quan trọng. 

1. Nếu`dist[v] > dist[u]`, quá trình di chuyển sẽ tiếp tục trong các lớp BFS. Cập nhật:$$best = \min(best, dfs(v))$$Ngoại lệ vẫn có sẵn, vì vậy mọi tùy chọn có thể truy cập từ`v`cũng có thể truy cập từ`u`. 

1. Lưu kết quả vào`ans[u]`và trả lại nó. 
2. Chạy`dfs(i)`cho mọi đỉnh. 
3. Xuất ra tất cả các câu trả lời được tính toán. 

### Tại sao nó hoạt động 

Khoảng cách BFS phân chia mọi cạnh thành cạnh tiến hoặc cạnh không tăng. 

Lợi thế không tăng chính xác là kiểu di chuyển có thể tiêu tốn một ngoại lệ duy nhất. Sau khi đạt được lợi thế như vậy, không có động thái nào trong tương lai có thể cải thiện câu trả lời vượt quá khoảng cách BFS tối thiểu đã đạt được ở điểm cuối của nó, do đó phép lặp sử dụng`dist[v]`. 

Cạnh phía trước không tiêu thụ ngoại lệ. Bắt đầu từ`u`và việc đạt được lợi thế như vậy tương đương với việc bắt đầu từ`v`với tất cả các tùy chọn vẫn có sẵn. Kể từ đây`ans[v]`là một ứng cử viên hợp lệ. 

Vì các cạnh phía trước luôn tăng khoảng cách BFS nên các phụ thuộc đệ quy chỉ đi từ khoảng cách nhỏ hơn đến khoảng cách lớn hơn. Không có chu trình nào có thể tồn tại giữa các phụ thuộc này, do đó sự lặp lại được xác định rõ ràng. Mọi hành trình hợp lệ đều bắt đầu bằng cách dừng lại, bằng cách đi theo một cạnh không tăng hoặc bằng cách đi về phía trước. Phép truy toán xem xét cả ba khả năng, do đó giá trị tối thiểu được tạo ra chính xác là câu trả lời tối ưu. 

#Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

t = int(input())

for _ in range(t):
    line = input().strip()
    while line == "":
        line = input().strip()

    n, m = map(int, line.split())

    g = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)

    dist = [-1] * n
    dist[0] = 0

    q = deque([0])

    while q:
        u = q.popleft()
        for v in g[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    ans = [-1] * n

    def dfs(u):
        if ans[u] != -1:
            return ans[u]

        best = dist[u]

        for v in g[u]:
            if dist[v] <= dist[u]:
                best = min(best, dist[v])
            else:
                best = min(best, dfs(v))

        ans[u] = best
        return best

    for i in range(n):
        dfs(i)

    print(*ans)
```Giải pháp bắt đầu bằng BFS từ thủ đô. Điều này tính toán khoảng cách đường đi ngắn nhất mà bài toán yêu cầu và phân loại mọi cạnh thành một trong hai loại được sử dụng bởi phép truy hồi. 

DFS thực hiện trực tiếp mối quan hệ lập trình động. Mảng ghi nhớ`ans`đảm bảo rằng mỗi đỉnh được giải quyết một lần. Khi một cạnh đi tới một đỉnh có khoảng cách BFS lớn hơn, quá trình đệ quy sẽ tiếp tục vì ngoại lệ vẫn chưa được sử dụng. Khi một cạnh đi đến một khoảng cách bằng hoặc nhỏ hơn, ngoại lệ có thể được thực hiện ngay lập tức, do đó chỉ`dist[v]`vấn đề. 

Việc đệ quy là an toàn vì các phần phụ thuộc chỉ tuân theo mức BFS tăng dần một cách nghiêm ngặt. Các cạnh như vậy không thể tạo thành một chu trình. Giới hạn đệ quy rõ ràng tránh tràn ngăn xếp trên chuỗi dài. 

# Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị đầu vào:```
1 -> 2
1 -> 3
2 -> 5
2 -> 4
5 -> 1
3 -> 6
6 -> 2
```Khoảng cách BFS: 

| Đỉnh | Khoảng cách | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 2 | 
| 5 | 2 | 
| 6 | 2 | 

Tính toán DP: 

| Đỉnh | Ứng viên | Trả lời | 
| --- | --- | --- | 
| 4 | dừng lại=2 | 2 | 
| 5 | dừng=2, cạnh 1 cho 0 | 0 | 
| 6 | dừng=2, cạnh 2 cho 1 | 1 | 
| 2 | dừng=1, ans[5]=0, ans[4]=2 | 0 | 
| 3 | dừng=1, ans[6]=1 | 1 | 
| 1 | dừng=0, ans[2]=0, ans[3]=1 | 0 | 

Đầu ra cuối cùng:```
0 0 1 2 0 1
```Ví dụ này cho thấy một đỉnh sâu hơn như 5 có thể ngay lập tức cải thiện câu trả lời của nó thông qua cạnh sau tới chữ hoa. 

### Ví dụ 2 

đầu vào:```
2 2
1 2
2 1
```Khoảng cách BFS: 

| Đỉnh | Khoảng cách | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 

Tính toán DP: 

| Đỉnh | Ứng viên | Trả lời | 
| --- | --- | --- | 
| 2 | dừng=1, cạnh 1 cho 0 | 0 | 
| 1 | dừng=0, ans[2]=0 | 0 | 

Đầu ra:```
0 0
```Điều này chứng tỏ rằng một cạnh không tăng có thể ngay lập tức đưa một đỉnh tới mức khoảng cách của thủ đô. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n+m)$| BFS truy cập mỗi cạnh một lần, DFS-DP xử lý từng cạnh một lần | 
| Không gian |$O(n+m)$| Lưu trữ đồ thị, mảng khoảng cách, mảng DP, ngăn xếp đệ quy | 

Tổng số đỉnh và cạnh của tất cả các ca kiểm thử nhiều nhất là$2 \cdot 10^5$. Độ phức tạp tuyến tính vừa vặn thoải mái trong giới hạn. 

# Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    sys.setrecursionlimit(1 << 20)

    for _ in range(t):
        line = input().strip()
        while line == "":
            line = input().strip()

        n, m = map(int, line.split())

        g = [[] for _ in range(n)]

        for _ in range(m):
            u, v = map(int, input().split())
            g[u - 1].append(v - 1)

        dist = [-1] * n
        dist[0] = 0

        q = deque([0])

        while q:
            u = q.popleft()
            for v in g[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)

        ans = [-1] * n

        def dfs(u):
            if ans[u] != -1:
                return ans[u]

            best = dist[u]

            for v in g[u]:
                if dist[v] <= dist[u]:
                    best = min(best, dist[v])
                else:
                    best = min(best, dfs(v))

            ans[u] = best
            return best

        for i in range(n):
            dfs(i)

        out.append(" ".join(map(str, ans)))

    return "\n".join(out)

assert solve("""3

6 7
1 2
1 3
2 5
2 4
5 1
3 6
6 2

2 2
1 2
2 1

6 8
1 2
1 5
2 6
6 1
2 3
3 4
4 2
5 4
""") == "0 0 1 2 0 1\n0 0\n0 0 2 1 1 0"

assert solve("""1

2 1
1 2
""") == "0 1"

assert solve("""1

3 3
1 2
2 3
3 1
""") == "0 0 0"

assert solve("""1

4 4
1 2
2 3
3 4
4 2
""") == "0 1 1 1"

assert solve("""1

5 4
1 2
2 3
3 4
4 5
""") == "0 1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn 1→2 | 0 1 | Đồ thị không tầm thường nhỏ nhất | 
| Chu kỳ vốn | 0 0 0 | Cạnh sau có thể giảm mọi câu trả lời về 0 | 
| Cạnh sau sâu | 0 1 1 1 | Cải tiến lan truyền đi lên thông qua DP | 
| Chuỗi nguyên chất | 0 1 2 3 4 | Không tồn tại cạnh không tăng hữu ích | 

# Vỏ cạnh 

Hãy xem xét một biểu đồ chỉ bao gồm một chuỗi:```
1 -> 2 -> 3 -> 4
```Khoảng cách BFS là`0 1 2 3`. Mọi cạnh đều chuyển sang mức BFS lớn hơn. Phép truy toán DFS không bao giờ tìm thấy cạnh không tăng, vì vậy mỗi câu trả lời vẫn có khoảng cách riêng. Đầu ra là:```
0 1 2 3
```Xét đồ thị có cạnh sau trực tiếp:```
1 -> 2 -> 3
^         |
|_________|
```Khoảng cách là`0 1 2`. Tại đỉnh 3, cạnh quay về 1 thỏa mãn`dist[1] <= dist[3]`, do đó câu trả lời ngay lập tức trở thành 0. Sự cải thiện đó lan truyền thông qua phép truy hồi, mang lại:```
0 0 0
```Hãy xem xét một biểu đồ chứa một cạnh giữa các đỉnh ở cùng cấp BFS:```
1 -> 2
1 -> 3
2 -> 3
```Khoảng cách là`0 1 1`. Cạnh`2 -> 3`thỏa mãn`dist[3] <= dist[2]`, vì vậy nó được tính là nước đi đặc biệt. Thuật toán xử lý chính xác các cạnh có khoảng cách bằng nhau giống như các cạnh sau, ngăn chặn giả định không chính xác rằng chỉ có khoảng cách nhỏ hơn mới quan trọng. 

Hãy xem xét một biểu đồ trong đó sự cải thiện tốt nhất xảy ra sau một số bước chuyển tiếp:```
1 -> 2 -> 3 -> 4
          |
          v
          1
```Vertex 2 không thể trực tiếp đạt đến mức BFS thấp hơn. Đường đi tối ưu của nó trước tiên đi theo các cạnh tiến tới 3 và sau đó sử dụng bước di chuyển đặc biệt. Sự tái phát nắm bắt được điều này thông qua`ans[3]`, lan truyền trở lại`ans[2]`. Đây chính xác là lý do tại sao DP phải sử dụng`ans[v]`trên các cạnh phía trước thay vì chỉ thông tin cục bộ.
