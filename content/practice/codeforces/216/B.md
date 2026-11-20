---
title: "CF 216B - Thành lập đội"
description: "Chúng ta có thể mô hình hóa học sinh dưới dạng đồ thị vô hướng. Mỗi học sinh là một đỉnh và mỗi cặp kẻ thù đều tạo ra một cạnh. Chúng tôi muốn chia số học sinh còn lại thành hai đội có quy mô bằng nhau sao cho không có cạnh nào nằm trong một đội."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "implementation"]
categories: ["algorithms"]
codeforces_contest: 216
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 133 (Div. 2)"
rating: 1700
weight: 216
solve_time_s: 87
verified: true
draft: false
---

[CF 216B - Thành lập nhóm](https://codeforces.com/problemset/problem/216/B) 

**Đánh giá:** 1700 
**Tags:** dfs và tương tự, triển khai 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có thể mô hình hóa học sinh dưới dạng đồ thị vô hướng. Mỗi học sinh là một đỉnh và mỗi cặp kẻ thù đều tạo ra một cạnh. Chúng tôi muốn chia số học sinh còn lại thành hai đội có quy mô bằng nhau sao cho không có cạnh nào nằm trong một đội. Về mặt biểu đồ, chúng ta cần một biểu đồ lưỡng cực, bởi vì kẻ thù luôn phải ở hai phía đối diện nhau. 

Nếu một số thành phần được kết nối không phải là lưỡng đảng thì ít nhất một học sinh khỏi thành phần đó phải bị loại bỏ. Sau khi loại bỏ, tổng số học sinh còn lại cũng phải chẵn, vì hai đội phải có số lượng bằng nhau. 

Biểu đồ có tối đa 100 đỉnh và 100 cạnh, do đó, ngay cả các thuật toán đồ thị khá chậm cũng có thể vượt qua. DFS hoặc BFS trên tất cả các đỉnh là không đáng kể trong giới hạn. Phần thú vị hơn là hiểu chính xác khi nào cần loại bỏ. 

Điều kiện mỗi sinh viên có nhiều nhất là 2 bậc đã hạn chế rất nhiều về cấu trúc đồ thị. Mọi thành phần được kết nối đều là: 

- một con đường, 
- một chu kỳ, 
- hoặc một đỉnh cô lập. 

Một con đường luôn có hai bên. Một chu kỳ chẵn cũng có tính chất lưỡng cực. Một chu trình lẻ không phải là chu trình lưỡng cực và việc loại bỏ một đỉnh sẽ biến nó thành một đường đi. 

Một trường hợp khó nhận thấy là học sinh bị cô lập. Coi như:```
3 0
```Tất cả học sinh đều độc lập nên đồ thị có tính chất lưỡng cực. Tuy nhiên, chúng ta không thể chia 3 học sinh thành hai đội bằng nhau. Một học sinh phải ngồi ngoài nên đáp án là`1`. 

Một sai lầm dễ mắc phải khác là quên rằng mỗi chu kỳ lẻ cần phải loại bỏ một lần ngay cả khi tổng số học sinh còn lại sau đó trở thành số chẵn. 

Ví dụ:```
3 3
1 2
2 3
3 1
```Đây là một hình tam giác. Đồ thị không phải là lưỡng cực. Việc loại bỏ một học sinh sẽ để lại một cạnh giữa hai học sinh còn lại, điều này có tác dụng. Câu trả lời đúng là`1`. 

Một trường hợp tinh tế hơn kết hợp cả hai vấn đề:```
6 3
1 2
2 3
3 1
```Có một chu trình lẻ có kích thước 3 và ba đỉnh cô lập. Việc loại bỏ một đỉnh khỏi chu trình sẽ có tổng cộng 5 học sinh, vẫn là số lẻ. Thêm một học sinh nữa cũng phải ngồi ngoài. Câu trả lời đúng là`2`. 

Việc thực hiện bất cẩn chỉ đếm chu kỳ lẻ sẽ in sai`1`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng loại bỏ từng nhóm nhỏ học sinh. Đối với mỗi tập hợp con, chúng tôi sẽ kiểm tra xem biểu đồ còn lại có phải là biểu đồ lưỡng cực hay không và số lượng học sinh còn lại có phải là số chẵn hay không. Vì có`2^n`tập hợp con, điều này trở nên không thể rất nhanh chóng. Ngay cả đối với`n = 100`, không gian tìm kiếm rất lớn về mặt thiên văn. 

Cấu trúc biểu đồ cho một hướng rõ ràng hơn nhiều. Vì mỗi đỉnh có nhiều nhất là 2 nên các thành phần liên thông cực kỳ đơn giản. Cấu trúc có vấn đề duy nhất là một chu kỳ lẻ. 

Một thành phần lưỡng cực được kết nối luôn có thể được tô bằng hai màu, nghĩa là các đỉnh của nó có thể được tách thành hai nhóm không có kẻ thù. Một chu trình lẻ phá vỡ tính chất này vì việc đi vòng quanh chu trình sẽ tạo ra sự mâu thuẫn về màu sắc. 

Vì vậy, vấn đề giảm xuống còn hai quan sát: 

Đầu tiên, mỗi chu kỳ lẻ buộc phải loại bỏ chính xác một lần. 

Thứ hai, sau khi cố định tất cả các chu kỳ lẻ, đồ thị còn lại là đồ thị lưỡng cực. Nếu số học sinh còn lại là số lẻ thì ta loại thêm một học sinh tùy ý để có tổng số chẵn. 

Màu DFS phát hiện các chu kỳ lẻ một cách tự nhiên. Bất cứ khi nào chúng ta cố gắng tô màu các đỉnh liền kề bằng các màu đối lập và phát hiện ra xung đột, thì thành phần được kết nối đó không phải là lưỡng cực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * (n + m)) | O(n + m) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị. 

Mỗi học sinh lưu trữ danh sách kẻ thù có liên quan đến họ. 
2. Duy trì mảng màu được khởi tạo bằng`-1`. 

Một giá trị của`-1`có nghĩa là đỉnh chưa được thăm. Màu sắc`0`Và`1`đại diện cho hai đội tô màu hai bên. 
3. Lặp qua tất cả các đỉnh. 

Bất cứ khi nào tìm thấy một đỉnh chưa được thăm, hãy khởi động DFS từ đỉnh đó. 
4. Trong DFS, gán màu xen kẽ cho các đỉnh lân cận. 

Nếu hàng xóm không được ghé thăm, hãy tô màu ngược lại và tiếp tục DFS. 
5. Nếu chúng ta tìm thấy một cạnh nối hai đỉnh có cùng màu thì thành phần được kết nối đó không phải là một cạnh. 

Bởi vì mọi thành phần đều có bậc tối đa là 2, điều này có nghĩa là thành phần đó chứa một chu trình lẻ. 
6. Đối với mọi thành phần không thuộc lưỡng đảng, hãy tăng câu trả lời lên 1. 

Việc loại bỏ một học sinh khỏi chu trình lẻ luôn phá vỡ chu trình và biến thành phần đó thành biểu đồ hai bên. 
7. Sau khi xử lý tất cả các thành phần, tính xem còn lại bao nhiêu học sinh. 

Nếu như`n - answer`là lẻ, tăng câu trả lời lên 1. 

Các học sinh còn lại phải được chia thành hai đội bằng nhau nên số lượng phải bằng nhau. 
8. In câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Màu DFS mô tả chính xác biểu đồ lưỡng cực. Một đồ thị là lưỡng cực khi và chỉ khi không có cạnh nào nối các đỉnh có cùng màu trong quá trình tô hai màu. 

Vì mọi thành phần liên thông ở đây có nhiều nhất là 2 nên cấu trúc không lưỡng cực duy nhất có thể là một chu trình lẻ. Việc loại bỏ một đỉnh khỏi một chu trình như vậy sẽ phá hủy chu trình và để lại một đường đi lưỡng cực. 

Sau khi cố định tất cả các chu kỳ lẻ, đồ thị trở thành đồ thị lưỡng cực. Bất kỳ biểu đồ lưỡng cực nào cũng có thể được chia thành hai nhóm không có kẻ thù. Vấn đề duy nhất còn lại là tính chẵn lẻ. Nếu tổng số học sinh còn lại là số lẻ thì không thể xếp các đội bằng nhau nên phải loại bỏ thêm một học sinh. 

Bởi vì mỗi chu kỳ lẻ yêu cầu ít nhất một lần loại bỏ và tính chẵn lẻ có thể yêu cầu thêm một lần nữa, thuật toán tạo ra câu trả lời tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

adj = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)

color = [-1] * (n + 1)

def dfs(start):
    stack = [start]
    color[start] = 0

    while stack:
        u = stack.pop()

        for v in adj[u]:
            if color[v] == -1:
                color[v] = color[u] ^ 1
                stack.append(v)
            elif color[v] == color[u]:
                return False

    return True

removed = 0

for i in range(1, n + 1):
    if color[i] == -1:
        if not dfs(i):
            removed += 1

remaining = n - removed

if remaining % 2 == 1:
    removed += 1

print(removed)
```The adjacency list stores the graph compactly and supports efficient traversal. Từ`n`Và`m`nhỏ, DFS đệ quy hoặc lặp lại sẽ hoạt động, nhưng DFS lặp lại hoàn toàn tránh được các mối lo ngại về độ sâu đệ quy. 

các`color`mảng xử lý cả trạng thái truy cập và màu lưỡng cực. Các đỉnh chưa được thăm vẫn ở`-1`. Trong quá trình truyền tải, hàng xóm luôn nhận được màu đối diện bằng XOR với`1`. 

DFS trả về`False`ngay khi nó tìm thấy một cạnh xung đột. Điều đó ngay lập tức cho chúng ta biết thành phần chứa một chu trình lẻ. 

Một chi tiết tinh tế là chúng tôi tính chính xác một lần xóa cho mỗi thành phần không thuộc lưỡng đảng. Vì mỗi đỉnh có nhiều nhất là 2 nên một thành phần liên thông không thể chứa nhiều chu trình lẻ độc lập. Trong biểu đồ chung, phím tắt này sẽ thất bại. 

Việc điều chỉnh chẵn lẻ chỉ xảy ra sau khi tất cả các chu kỳ lẻ được cố định. Việc loại bỏ các đỉnh khỏi chu kỳ lẻ sẽ làm thay đổi tổng số học sinh còn lại, do đó tính chẵn lẻ phải được kiểm tra sau đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 2
2 4
5 3
1 4
```Đồ thị chứa một hình tam giác giữa các đỉnh`1, 2, 4`và một cạnh riêng biệt`3, 5`. 

| Bước | Đỉnh hiện tại | Kết quả DFS | Đã xóa | 
| --- | --- | --- | --- | 
| 1 | 1 | Không lưỡng cực | 1 | 
| 2 | 3 | lưỡng đảng | 1 | 

Sau khi loại bỏ một học sinh khỏi tam giác,`4`học sinh vẫn còn, tức là đã chẵn rồi. 

Câu trả lời cuối cùng:`1`Dấu vết này cho thấy quan sát cốt lõi của vấn đề. Một chu kỳ lẻ yêu cầu chính xác một lần loại bỏ. 

### Ví dụ 2 

đầu vào:```
6 3
1 2
2 3
3 1
```Đỉnh`4, 5, 6`đang bị cô lập. 

| Bước | Đỉnh hiện tại | Kết quả DFS | Đã xóa | 
| --- | --- | --- | --- | 
| 1 | 1 | Không lưỡng cực | 1 | 
| 2 | 4 | lưỡng đảng | 1 | 
| 3 | 5 | lưỡng đảng | 1 | 
| 4 | 6 | lưỡng đảng | 1 | 

Sau khi cố định hình tam giác,`5`học sinh ở lại. 

| Sinh viên còn lại | Thậm chí? | Đã xóa cuối cùng | 
| --- | --- | --- | 
| 5 | Không | 2 | 

Câu trả lời cuối cùng:`2`Ví dụ này chứng tỏ tại sao việc điều chỉnh chẵn lẻ là cần thiết ngay cả sau khi tất cả các chu kỳ lẻ được xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được xử lý một lần trong DFS | 
| Không gian | O(n + m) | Danh sách kề và mảng màu | 

Với tối đa 100 đỉnh và 100 cạnh, giải pháp này hoạt động thoải mái trong giới hạn. Ngay cả việc truyền tải đồ thị chậm hơn nhiều cũng có thể vượt qua, nhưng cách tiếp cận tuyến tính vừa rõ ràng vừa tối ưu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    adj = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    color = [-1] * (n + 1)

    def dfs(start):
        stack = [start]
        color[start] = 0

        while stack:
            u = stack.pop()

            for v in adj[u]:
                if color[v] == -1:
                    color[v] = color[u] ^ 1
                    stack.append(v)
                elif color[v] == color[u]:
                    return False

        return True

    removed = 0

    for i in range(1, n + 1):
        if color[i] == -1:
            if not dfs(i):
                removed += 1

    if (n - removed) % 2 == 1:
        removed += 1

    print(removed)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run(
"""5 4
1 2
2 4
5 3
1 4
"""
) == "1", "sample 1"

# minimum graph, already balanced
assert run(
"""2 0
"""
) == "0", "two isolated students"

# odd cycle
assert run(
"""3 3
1 2
2 3
3 1
"""
) == "1", "triangle requires one removal"

# odd cycle plus parity issue
assert run(
"""6 3
1 2
2 3
3 1
"""
) == "2", "triangle fixed but remaining count still odd"

# even cycle
assert run(
"""4 4
1 2
2 3
3 4
4 1
"""
) == "0", "even cycle is bipartite"

# isolated vertices causing parity problem
assert run(
"""5 0
"""
) == "1", "odd number of isolated students"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0`|`0`| Trường hợp cân bằng hợp lệ nhỏ nhất | 
| Đồ thị tam giác |`1`| Phát hiện chu kỳ lẻ | 
| Tam giác cộng với các nút bị cô lập |`2`| Loại bỏ tính chẵn lẻ bổ sung | 
| Chu kỳ chẵn |`0`| Xử lý lưỡng cực đúng cách | 
|`5 0`|`1`| Tổng số lẻ không có cạnh | 

## Vỏ cạnh 

Hãy xem xét trường hợp hoàn toàn bị ngắt kết nối:```
3 0
```DFS truy cập từng đỉnh một cách độc lập và không bao giờ tìm thấy xung đột màu.`removed = 0`sau khi xử lý đồ thị. Từ`3`số học sinh còn lại, chẵn lẻ là số lẻ nên loại bỏ thêm một học sinh. Thuật toán in`1`. 

Bây giờ hãy xem xét một hình tam giác duy nhất:```
3 3
1 2
2 3
3 1
```Màu DFS`1`với`0`,`2`với`1`, Và`3`với`0`. Khi xử lý cạnh`(3,1)`, cả hai điểm cuối đều có màu`0`, do đó thành phần này không phải là lưỡng cực.`removed`trở thành`1`. Số học sinh còn lại bằng nhau`2`, vốn đã chẵn rồi. Câu trả lời cuối cùng là`1`. 

Cuối cùng, hãy xem xét nhiều thành phần:```
8 5
1 2
2 3
3 1
4 5
6 7
```thành phần`{1,2,3}`là một chu trình lẻ nên phải loại bỏ một học sinh. Linh kiện`{4,5}`Và`{6,7}`là lưỡng cực và có đỉnh`8`bị cô lập. Sau một lần loại bỏ,`7`học sinh vẫn còn, vẫn lẻ loi. Thuật toán loại bỏ thêm một sinh viên và đưa ra kết quả`2`. 

Điều này xác nhận hai nguồn loại bỏ độc lập: ấn định chu kỳ lẻ và ấn định tính chẵn lẻ.
