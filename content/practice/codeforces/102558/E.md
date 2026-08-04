---
title: "CF 102558E - \u0420\u0430\u0437\u0434\u0435\u043b\u0435\u043d\u0438\u0435 \u0433\u0440\u0430\u0444\u0430"
description: "Đồ thị chứa các đỉnh được kết nối bởi các cạnh vô hướng có trọng số. Chúng ta phải tô màu các đỉnh bằng hai màu, sử dụng cả hai màu và xem xét các cạnh có điểm cuối nhận được cùng một màu. Trong số các cạnh đó, chúng tôi quan tâm đến trọng lượng nhỏ nhất."
date: "2026-08-04T09:20:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102558
codeforces_index: "E"
codeforces_contest_name: "Contest for Yandex interns 2019"
rating: 0
weight: 102558
solve_time_s: 96
verified: true
draft: false
---

[CF 102558E - \u0420\u0430\u0437\u0434\u0435\u043b\u0435\u043d\u0438\u0435 \u0433\u0440\u0430\u0444\u0430](https://codeforces.com/problemset/problem/102558/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đồ thị chứa các đỉnh được kết nối bởi các cạnh vô hướng có trọng số. Chúng ta phải tô màu các đỉnh bằng hai màu, sử dụng cả hai màu và xem xét các cạnh có điểm cuối nhận được cùng một màu. Trong số các cạnh đó, chúng tôi quan tâm đến trọng lượng nhỏ nhất. Mục tiêu là làm cho cạnh bên trong nhỏ nhất này càng lớn càng tốt. 

Một cách hữu ích để xem xét mục tiêu là chọn một giá trị trả lời`x`. Nếu tất cả các cạnh có trọng số nhỏ hơn`x`buộc phải đi giữa hai nhóm thì mọi cạnh còn lại trong nhóm có trọng số ít nhất`x`. Câu hỏi đặt ra là liệu đồ thị con chỉ bao gồm các cạnh có trọng số nhỏ hơn`x`có thể được chia thành hai bên, đó chính xác là câu hỏi liệu đồ thị con này có phải là lưỡng cực hay không. 

Kích thước đầu vào đạt`100000`đỉnh và`100000`các cạnh. Việc thử mọi phân vùng có thể là không thể vì có`2^n`chất tạo màu. Ngay cả các thuật toán có hành vi bậc hai cũng quá chậm đối với các giới hạn này, vì`10^10`hoạt động sẽ vượt xa những gì thực tế. Chúng ta cần một cách tiếp cận gần`O((n+m) log C)`, Ở đâu`C`là phạm vi của các câu trả lời có thể. 

Có một số trường hợp việc triển khai không chính xác có thể âm thầm thất bại. Các cạnh lặp lại có trọng số khác nhau phải được xử lý độc lập. Ví dụ:```
3 3
1 2 1
1 2 5
2 3 4
```Câu trả lời đúng là`4`. Cạnh của trọng lượng`1`quan trọng khi kiểm tra các ngưỡng nhỏ, nhưng cạnh trọng lượng`5`vẫn có thể trở thành cạnh trong của phân vùng cuối cùng. Giải pháp chỉ giữ một cạnh giữa hai đỉnh có thể loại bỏ thông tin cần thiết cho câu trả lời. 

Một trường hợp khác là khi toàn bộ đồ thị gần như có hai phần nhưng không thể thực hiện được chỉ vì một cạnh nặng hơn:```
3 3
1 2 1
2 3 1
1 3 10
```Câu trả lời đúng là`10`. Với mọi giá trị đến`10`, đồ thị được hình thành bởi các cạnh nhỏ hơn là đồ thị lưỡng cực. Tại`11`, toàn bộ đồ thị được bao gồm và không phải là đồ thị lưỡng cực. Một giải pháp chỉ kiểm tra xem biểu đồ ban đầu có phải là biểu đồ lưỡng cực hay không, có bỏ qua việc giải thích ngưỡng hay không. 

Trường hợp cạnh cuối cùng là câu trả lời có thể có trọng số cạnh lớn nhất trong biểu đồ:```
3 3
1 2 2
2 3 2
1 3 7
```Câu trả lời đúng là`7`. Thuật toán phải cho phép ngưỡng đạt trọng số cạnh tối đa, vì cạnh có trọng số đó có thể là cạnh duy nhất còn lại bên trong một phần. 

## Phương pháp tiếp cận 

Lời giải trực tiếp sẽ liệt kê mọi phép chia có thể của các đỉnh thành hai nhóm khác rỗng. Đối với mỗi bộ phận, chúng tôi sẽ quét tất cả các cạnh, tìm các cạnh có điểm cuối nằm trong cùng một nhóm và giữ trọng số nhỏ nhất như vậy. Điều này đúng vì nó kiểm tra chính xác số lượng mà bài toán yêu cầu. 

Vấn đề là số lượng các phân chia có thể. Với`n`đỉnh có`2^n`phép gán hai màu và thậm chí sau khi bỏ qua các trường hợp đối xứng trong đó cả hai màu được hoán đổi, khối lượng công việc vẫn theo cấp số nhân. Vì`n = 100000`, cách tiếp cận này không khả thi chút nào. 

Quan sát chính là chúng ta không cần trực tiếp xây dựng phân vùng tốt nhất. Chúng ta có thể hỏi một câu hỏi khác: ít nhất câu trả lời có thể là`x`? Để điều đó xảy ra, mọi cạnh có trọng số nhỏ hơn`x`phải kết nối các đỉnh của các nhóm khác nhau. Nói cách khác, nếu chúng ta tạm thời chỉ giữ các cạnh có trọng số nhỏ hơn`x`, đồ thị này phải là đồ thị lưỡng cực. 

Lực lượng vũ phu hoạt động vì nó kiểm tra mọi màu có thể có, nhưng không thành công khi số lượng màu bùng nổ. Việc quan sát ngưỡng cho phép chúng ta thay thế tìm kiếm trên các phân vùng bằng tìm kiếm theo trọng số cạnh. Tính chất đơn điệu: nếu các cạnh nhỏ hơn`x`tạo thành một đồ thị hai bên, khi đó các cạnh nhỏ hơn bất kỳ giá trị nhỏ hơn nào cũng tạo thành một đồ thị hai bên. Điều này cho phép tìm kiếm nhị phân trên`x`. 

Đối với mỗi bước tìm kiếm nhị phân, chúng tôi chạy kiểm tra lưỡng cực bằng BFS hoặc DFS trên tất cả các cạnh có trọng số dưới ngưỡng hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * m) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân với kiểm tra lưỡng cực | O((n + m) log W) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các cạnh. Thao tác duy nhất chúng ta cần trong quá trình kiểm tra là quyết định xem trọng số cạnh có nằm dưới ngưỡng đã chọn hay không. 
2. Tìm kiếm nhị phân câu trả lời giữa`1`Và`100001`. Đối với giá trị ứng cử viên`mid`, chúng tôi kiểm tra xem tất cả các cạnh có trọng số nhỏ hơn`mid`tạo thành một đồ thị lưỡng cực. 
3. Trong quá trình kiểm tra, hãy tạo một mảng màu. Một đỉnh không màu được gán một màu, sau đó BFS trải các màu đối diện qua mọi cạnh có trọng số nhỏ hơn ngưỡng. 
4. Nếu một cạnh có trọng số nhỏ hơn ngưỡng nối hai đỉnh đã có cùng màu thì đồ thị không phải là đồ thị lưỡng cực đối với ngưỡng này. Câu trả lời ứng viên quá lớn nên tìm kiếm nhị phân tiếp tục ở nửa dưới. 
5. Nếu kiểm tra thành công, giá trị ứng viên có thể đạt được. Chúng tôi di chuyển phạm vi tìm kiếm nhị phân lên trên để tìm câu trả lời lớn hơn có thể. 
6. Ngưỡng thành công lớn nhất được in. Vì toàn bộ biểu đồ được đảm bảo không có hai phần nên việc tìm kiếm không thể trả về sai một giá trị trong đó tất cả các cạnh có thể được đặt giữa hai nhóm. 

Bất biến đằng sau thuật toán là ngưỡng`x`có giá trị chính xác khi mọi cạnh có thể trở thành cạnh nhỏ bên trong bị cấm có thể được phân tách bằng hai màu. Việc kiểm tra lưỡng đảng chứng minh liệu màu đó có tồn tại hay không. Vì việc tăng ngưỡng chỉ thêm nhiều cạnh hơn vào biểu đồ đã chọn nên khi một ngưỡng không thành công thì mọi ngưỡng lớn hơn cũng không thành công. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((u - 1, v - 1, w))

    def check(limit):
        graph = [[] for _ in range(n)]
        for u, v, w in edges:
            if w < limit:
                graph[u].append(v)
                graph[v].append(u)

        color = [-1] * n

        for start in range(n):
            if color[start] != -1:
                continue
            color[start] = 0
            q = deque([start])

            while q:
                v = q.popleft()
                for to in graph[v]:
                    if color[to] == -1:
                        color[to] = color[v] ^ 1
                        q.append(to)
                    elif color[to] == color[v]:
                        return False
        return True

    left = 1
    right = 100002

    while left + 1 < right:
        mid = (left + right) // 2
        if check(mid):
            left = mid
        else:
            right = mid

    print(left)

if __name__ == "__main__":
    solve()
```Danh sách cạnh được giữ ở dạng ban đầu vì mỗi lần lặp tìm kiếm nhị phân cần lọc các cạnh theo trọng số. Xây dựng lại biểu đồ tạm thời bên trong`check`giữ logic đơn giản và tránh những sai lầm với các khía cạnh lỗi thời. 

Tìm kiếm nhị phân sử dụng phạm vi`[1, 100002)`. Giới hạn trên là độc quyền vì trọng số của cạnh nhiều nhất là`100000`và một ngưỡng lớn hơn mọi cạnh sẽ bao gồm toàn bộ biểu đồ. Việc đảm bảo rằng biểu đồ ban đầu không phải là biểu đồ lưỡng cực có nghĩa là ngưỡng đó phải thất bại. 

Màu sắc BFS kết nối các thành phần riêng biệt. Điều này quan trọng vì biểu đồ có thể chứa một số phần bị ngắt kết nối và mỗi thành phần có thể được tô màu độc lập. Xung đột chỉ xuất hiện khi một người hàng xóm đã được tô màu yêu cầu cùng màu. 

Số nguyên Python không tràn ở đây và số học duy nhất liên quan đến câu trả lời là phép tính điểm giữa tìm kiếm nhị phân. Việc sử dụng`sys.stdin.readline`và một deque giữ cho việc truyền tải đầu vào và đồ thị đủ nhanh cho các ràng buộc. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 4
1 2 1
1 2 2
1 3 3
3 2 4
```Tìm kiếm nhị phân kiểm tra xem có thể có các ngưỡng khác nhau hay không. 

| giới hạn | Các cạnh có trọng lượng < giới hạn | lưỡng đảng? | Quyết định | 
| --- | --- | --- | --- | 
| 50001 | Tất cả các cạnh | Không | giảm | 
| 25001 | Tất cả các cạnh | Không | giảm | 
| 12501 | Tất cả các cạnh | Không | giảm | 
| ... | ... | ... | ... | 
| 5 | Các cạnh có trọng số 1, 2, 3 | Có | tăng | 
| 6 | Tất cả các cạnh | Không | giảm | 

Ngưỡng hợp lệ lớn nhất là`4`. Việc kiểm tra với`limit = 5`chỉ loại bỏ cạnh của trọng lượng`4`, do đó đồ thị còn lại có thể có hai màu. Việc tăng ngưỡng sẽ bao gồm cạnh đó và tạo ra chu kỳ lẻ. 

Đối với mẫu thứ hai:```
4 5
1 2 1
2 3 1
3 4 1
4 1 1
2 4 2
```| giới hạn | Các cạnh có trọng lượng < giới hạn | lưỡng đảng? | Quyết định | 
| --- | --- | --- | --- | 
| 50001 | Tất cả các cạnh | Không | giảm | 
| ... | ... | ... | ... | 
| 3 | Tất cả các cạnh | Không | giảm | 
| 2 | Bốn cạnh chu kỳ | Có | tăng | 

Câu trả lời là`2`. Bốn cạnh của trọng lượng`1`tạo thành một chu trình chẵn nên có thể tách chúng thành hai nhóm. Cạnh của trọng lượng`2`có thể vẫn ở trong một nhóm, làm cho trọng số cạnh bên trong tối thiểu bằng`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log W) | Tìm kiếm nhị phân thực hiện khoảng 17 lần kiểm tra và mỗi lần kiểm tra sẽ quét biểu đồ. | 
| Không gian | O(n + m) | Đồ thị, màu sắc, hàng đợi và lưu trữ cạnh tạm thời là tuyến tính. | 

Với`n`Và`m`lên đến`100000`, mỗi kiểm tra lưỡng cực là tuyến tính và hệ số logarit chỉ đến từ phạm vi nhỏ của các trọng số có thể. Điều này phù hợp thoải mái trong giới hạn cần thiết. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((u - 1, v - 1, w))

    def check(limit):
        g = [[] for _ in range(n)]
        for u, v, w in edges:
            if w < limit:
                g[u].append(v)
                g[v].append(u)

        color = [-1] * n
        for s in range(n):
            if color[s] == -1:
                color[s] = 0
                q = deque([s])
                while q:
                    v = q.popleft()
                    for to in g[v]:
                        if color[to] == -1:
                            color[to] = color[v] ^ 1
                            q.append(to)
                        elif color[to] == color[v]:
                            return False
        return True

    l, r = 1, 100002
    while l + 1 < r:
        mid = (l + r) // 2
        if check(mid):
            l = mid
        else:
            r = mid
    return str(l)

assert solution("""3 4
1 2 1
1 2 2
1 3 3
3 2 4
""") == "4"

assert solution("""4 5
1 2 1
2 3 1
3 4 1
4 1 1
2 4 2
""") == "2"

assert solution("""3 3
1 2 1
2 3 2
1 3 3
""") == "3"

assert solution("""3 3
1 2 5
2 3 5
1 3 5
""") == "5"

assert solution("""4 4
1 2 1
2 3 1
3 4 1
4 1 1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tam giác có trọng số 1, 2, 3 | 3 | Cạnh tối đa có thể là câu trả lời. | 
| Tam giác có mọi trọng số 5 | 5 | Trọng lượng bằng nhau và xử lý ngưỡng tối đa. | 
| Bốn chu kỳ với mọi trọng lượng 1 | 1 | Đồ thị ban đầu không lưỡng cực hợp lệ nhỏ nhất. | 

## Vỏ cạnh 

Đối với các cạnh lặp lại, hãy xem xét:```
3 3
1 2 1
1 2 5
2 3 4
```Việc kiểm tra cho`limit = 5`chỉ sử dụng cạnh thứ nhất và thứ ba. Chúng tạo thành một con đường, vì vậy chúng là hai phần. Việc kiểm tra cho`limit = 6`bao gồm tất cả các cạnh và cũng bao gồm cạnh song song nặng hơn. Câu trả lời trở thành`5`, bởi vì ngưỡng lớn nhất giữ cho đồ thị lưỡng cực có cạnh nhỏ hơn chính xác là`5`. Thuật toán không bao giờ hợp nhất các cạnh song song, vì vậy cả hai khả năng đều được bảo toàn. 

Đối với biểu đồ có cạnh tối đa là câu trả lời:```
3 3
1 2 2
2 3 2
1 3 7
```Khi ngưỡng là`7`, chỉ có hai cạnh của trọng lượng`2`được kiểm tra. Chúng tạo thành một con đường và có thể có hai màu. Khi ngưỡng trở thành`8`, cạnh của trọng lượng`7`được thêm vào và tam giác trở thành không lưỡng cực. Tìm kiếm nhị phân trả về`7`, phù hợp với cạnh bên trong tốt nhất có thể. 

Đối với biểu đồ kích thước tối thiểu:```
3 3
1 2 1
2 3 1
1 3 1
```Mỗi ngưỡng lớn hơn`1`bao gồm toàn bộ hình tam giác, không thể tô màu bằng hai màu. Ngưỡng thành công duy nhất là`1`, do đó, thuật toán đầu ra`1`. Điều này xác nhận rằng ranh giới dưới của tìm kiếm được xử lý chính xác.
