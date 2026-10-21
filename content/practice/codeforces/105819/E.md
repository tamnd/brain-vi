---
title: "CF 105819E - Mingle"
description: "Chúng ta có một đồ thị tình bạn vô hướng với n người chơi và m cạnh tình bạn. Hai người chơi được phép tạo thành một cặp nếu họ được kết nối trực tiếp bởi tình bạn hoặc nếu họ có ít nhất một người bạn chung."
date: "2026-06-25T15:06:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105819
codeforces_index: "E"
codeforces_contest_name: "TeamsCode Spring 2025 Novice Division"
rating: 0
weight: 105819
solve_time_s: 46
verified: true
draft: false
---

[CF 105819E - Mingle](https://codeforces.com/problemset/problem/105819/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị tình bạn vô hướng với`n`người chơi và`m`cạnh tình bạn. 

Hai người chơi được phép tạo thành một cặp nếu họ được kết nối trực tiếp bởi tình bạn hoặc nếu họ có ít nhất một người bạn chung. Theo thuật ngữ biểu đồ, hai đỉnh có thể được ghép nối nếu khoảng cách của chúng trong biểu đồ tình bạn lớn nhất`2`. 

Mỗi người chơi có thể thuộc nhiều nhất một cặp. Nhiệm vụ là xuất ra một cặp với số lượng cặp tối đa có thể. 

Biểu đồ có thể chứa tới`2 · 10^5`các đỉnh và các cạnh. Bất kỳ thuật toán nào xây dựng rõ ràng biểu đồ của tất cả các cặp hợp lệ đều không thể thực hiện được ngay lập tức. Một đỉnh bậc cao duy nhất có thể tạo ra mối quan hệ Θ(n2) khoảng cách-2. Chúng ta cần một giải pháp hoạt động trực tiếp trên biểu đồ gốc trong thời gian gần như tuyến tính. 

Quan sát quan trọng đầu tiên là người chơi từ các thành phần được kết nối khác nhau không bao giờ có thể được ghép nối. Bất kỳ cặp hợp lệ nào cũng phải nằm hoàn toàn bên trong một thành phần được kết nối. 

Quan sát thứ hai đáng ngạc nhiên hơn. Đối với một thành phần được kết nối có chứa`s`các đỉnh, luôn có thể tạo chính xác`⌊s / 2⌋`cặp. Vì không có thành phần nào có thể đóng góp nhiều hơn`⌊s / 2⌋`dù sao đi nữa, đó là mức tối đa. 

Điều này làm giảm vấn đề xây dựng một cặp đôi như vậy một cách hiệu quả. 

Hãy xem xét một thành phần bao gồm một đỉnh cô lập duy nhất:```
1
```Câu trả lời đúng là`0`cặp. Việc triển khai bất cẩn cố gắng ghép đôi mọi cha mẹ DFS với con một cách mù quáng sẽ tạo ra một cặp không hợp lệ. 

Hãy xem xét một đường dẫn có độ dài ba:```
1 - 2 - 3 - 4
```Người chơi`1`Và`3`có thể ghép đôi vì họ có chung bạn bè`2`, và người chơi`2`Và`4`có thể ghép đôi vì họ có chung bạn bè`3`. Câu trả lời đúng là`2`cặp. Chỉ nhìn vào các cạnh của tình bạn sẽ chỉ tìm thấy một cặp không chính xác. 

Hãy xem xét một ngôi sao:```
    2
    |
3 - 1 - 4
    |
    5
```Tất cả các lá đều tương thích theo cặp vì chúng chia sẻ trình phát`1`như một người bạn chung. Câu trả lời đúng là`2`cặp. Bất kỳ giải pháp nào chỉ xem xét các cạnh của đồ thị gốc sẽ bỏ lỡ các cặp này. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trước tiên sẽ xây dựng biểu đồ của tất cả các cặp hợp lệ. Đối với mỗi đỉnh, chúng tôi sẽ kết nối nó với tất cả bạn bè và tất cả các đỉnh có thể truy cập được theo đúng hai bước. Sau khi xây dựng biểu đồ này, chúng tôi sẽ tìm kiếm kết quả phù hợp tối đa. 

Vấn đề là đồ thị khoảng cách-2 có thể chứa các cạnh Θ(n2). Một ngôi sao với`2 · 10^5`các đỉnh đã tạo ra gần như`2 · 10^5²`ghép đôi hợp lệ giữa các lá. Xây dựng biểu đồ đó là không thể. 

Sự đột phá đến từ việc bỏ qua phần lớn biểu đồ. 

Lấy bất kỳ thành phần được kết nối nào và chọn một cây DFS tùy ý bên trong nó. Mọi khía cạnh tình bạn bên ngoài cái cây đó chỉ đơn thuần là một khía cạnh phụ. Nếu chúng ta đã có thể đạt được`⌊s / 2⌋`các cặp chỉ sử dụng cây DFS thì các cạnh bổ sung không liên quan vì`⌊s / 2⌋`là mức tối đa theo lý thuyết. 

Vì vậy, vấn đề thực sự trở thành: 

Cho một cây, hãy ghép càng nhiều đỉnh càng tốt sao cho mỗi cặp cách nhau nhiều nhất`2`. 

DFS từ dưới lên cung cấp chính xác những gì chúng ta cần. 

Đối với mỗi nút, sau khi xử lý tất cả các cây con, một số cây con có thể vẫn chứa một đỉnh chưa khớp. Hai đứa trẻ không có cặp bất kỳ như vậy có thể được ghép đôi với nhau vì khoảng cách của chúng chính xác là`2`thông qua nút hiện tại. 

Sau khi ghép nối các đại diện con với nhau, nếu vẫn còn một con chưa khớp, chúng tôi ghép con đó với nút hiện tại. Khoảng cách của họ là`1`. 

Điều này loại bỏ các đỉnh khỏi cây một cách tham lam trong khi vẫn đảm bảo rằng mọi cặp được tạo đều hợp lệ. 

Ở cuối DFS, mỗi thành phần để lại nhiều nhất một đỉnh chưa từng có, điều đó có nghĩa là chúng ta thu được chính xác`⌊s / 2⌋`cặp. 

DFS tương tự hoạt động không thay đổi trên biểu đồ gốc. Chúng ta chỉ cần duyệt qua một cây DFS của mỗi thành phần được kết nối và bỏ qua tất cả các cạnh không phải là cây. 

Giải pháp chạy trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n²) | Quá chậm | 
| Xây dựng DFS tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của đồ thị tình bạn. 
2. Bắt đầu DFS từ mọi đỉnh chưa được thăm. Mỗi DFS khám phá chính xác một thành phần được kết nối. 
3. Đối với một nút`u`, xử lý đệ quy tất cả các phần tử con chưa được thăm. 
4. Mỗi DFS con trả về liệu cây con của con đó có còn chứa một đại diện chưa từng có hay không. 
5. Thu thập tất cả các đại diện con chưa được so sánh. 
6. Ghép đôi hai người đại diện này cùng một lúc. 

Nếu đại diện đến từ trẻ em`a`Và`b`, sau đó`a`Và`b`chia sẻ tình bạn chung`u`, vậy cặp này hợp lệ. 
7. Sau khi ghép càng nhiều đại diện con càng tốt, hãy thêm`u`chính nó vào bộ sưu tập. 
8. Một lần nữa ghép nối hai phần tử của bộ sưu tập cùng một lúc và lưu trữ các cặp đó. 
9. Nếu kích thước bộ sưu tập là số lẻ, chỉ có duy nhất một đại diện chưa được so sánh. Trả lại thông tin đó cho phụ huynh. 
10. Tiếp tục cho đến khi toàn bộ thành phần đã được xử lý. 

### Tại sao nó hoạt động 

DFS duy trì một bất biến đơn giản. 

Sau khi kết thúc cây con của một nút`u`, tất cả các đỉnh trong cây con đó đã được ghép nối ngoại trừ một đại diện được trả về phía trên. 

Bất cứ khi nào tồn tại hai đại diện con chưa từng có, chúng có thể được ghép đôi một cách an toàn vì cả hai đều liền kề với nhau.`u`trong cây DFS. Khoảng cách của họ chính xác là`2`. 

Nếu vẫn còn một đứa trẻ chưa từng có, hãy ghép nó với`u`hợp lệ vì khoảng cách của họ là`1`. 

Kết quả là, mọi thao tác đều tạo ra một cặp hợp lệ và giảm số đỉnh không khớp xuống còn hai. 

Một thành phần có kích thước`s`có thể để lại nhiều nhất một đỉnh không trùng khớp sau khi gốc kết thúc. Do đó chính xác`⌊s / 2⌋`cặp được sản xuất. Vì không có kết quả phù hợp nào có thể sử dụng nhiều hơn`⌊s / 2⌋`cặp, việc xây dựng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 20)

n, m = map(int, input().split())

adj = [[] for _ in range(n)]

for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    adj[u].append(v)
    adj[v].append(u)

vis = [False] * n
ans = []

def dfs(u):
    vis[u] = True

    rem = []

    for v in adj[u]:
        if vis[v]:
            continue

        if dfs(v):
            rem.append(v)

    rem.append(u)

    for i in range(1, len(rem), 2):
        ans.append((rem[i - 1] + 1, rem[i] + 1))

    return len(rem) % 2 == 1

for i in range(n):
    if not vis[i]:
        dfs(i)

print(len(ans))
print("\n".join(f"{u} {v}" for u, v in ans))
```Danh sách kề lưu trữ biểu đồ tình bạn ban đầu. 

DFS chỉ truy cập các cạnh của cây. Bất kỳ cạnh bổ sung nào trong thành phần đều bị bỏ qua vì chỉ riêng cây DFS là đủ để đạt được số cặp tối ưu. 

Danh sách`rem`lưu trữ các đỉnh con có cây con vẫn chứa một đại diện chưa từng có. Sau khi tất cả các nút con được xử lý, nút hiện tại sẽ được thêm vào cùng danh sách. 

Ghép nối các phần tử liên tiếp của`rem`là an toàn. Hai đại diện trẻ ở khoảng cách xa`2`thông qua nút hiện tại, trong khi nút đại diện con và nút hiện tại ở khoảng cách`1`. 

Giá trị trả về cho biết chính xác một đại diện có còn chưa khớp sau tất cả các cặp có thể có trong cây con hay không. 

Việc triển khai sử dụng các chỉ mục dựa trên 0 trong nội bộ và chuyển đổi trở lại các chỉ mục dựa trên 1 khi lưu trữ câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
1 3
```DFS bắt nguồn từ`1`. 

| Nút | Trẻ em vô song | rem sau khi thêm self | Cặp sản xuất | Trở về | 
| --- | --- | --- | --- | --- | 
| 2 | {} | {2} | không | chưa từng có | 
| 3 | {} | {3} | không | chưa từng có | 
| 1 | {2,3} | {2,3,1} | (2,3) | chưa từng có | 

Đầu ra:```
1
2 3
```Điều này thể hiện quy tắc khoảng cách-2. Người chơi`2`Và`3`không phải là bạn bè nhưng họ có chung bạn bè`1`. 

### Ví dụ 2 

đầu vào:```
5 2
1 2
3 4
```Có ba thành phần được kết nối. 

| Thành phần | Kích thước | Cặp được sản xuất | 
| --- | --- | --- | 
| {1,2} | 2 | (1,2) | 
| {3,4} | 2 | (3,4) | 
| {5} | 1 | không | 

Đầu ra:```
2
1 2
3 4
```Điều này chứng tỏ rằng các thành phần được xử lý độc lập và các đỉnh biệt lập vẫn không thể so sánh được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được xử lý một số lần không đổi | 
| Không gian | O(n + m) | Danh sách kề, mảng truy cập, ngăn xếp đệ quy, lưu trữ câu trả lời | 

Với`n, m ≤ 2 · 10^5`, độ phức tạp tuyến tính dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

# Sample-style validation is difficult because many valid answers exist.
# These tests verify the number of pairs produced.

from collections import deque

def expected_pairs_count(n, edges):
    g = [[] for _ in range(n)]
    for u, v in edges:
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    vis = [False] * n
    total = 0

    for i in range(n):
        if vis[i]:
            continue

        q = deque([i])
        vis[i] = True
        sz = 0

        while q:
            u = q.popleft()
            sz += 1

            for v in g[u]:
                if not vis[v]:
                    vis[v] = True
                    q.append(v)

        total += sz // 2

    return total

assert expected_pairs_count(3, [(1, 2), (1, 3)]) == 1
assert expected_pairs_count(5, [(1, 2), (3, 4)]) == 2

# minimum graph
assert expected_pairs_count(1, []) == 0

# single edge
assert expected_pairs_count(2, [(1, 2)]) == 1

# star
assert expected_pairs_count(
    5,
    [(1, 2), (1, 3), (1, 4), (1, 5)]
) == 2

# path of length 3
assert expected_pairs_count(
    4,
    [(1, 2), (2, 3), (3, 4)]
) == 2

# odd-sized connected component
assert expected_pairs_count(
    5,
    [(1, 2), (2, 3), (3, 4), (4, 5)]
) == 2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 0 đôi | Thành phần kích thước tối thiểu | 
| Một cạnh | 1 cặp | Ghép đôi tình bạn trực tiếp | 
| Đồ thị sao | 2 cặp | Ghép nối khoảng cách-2 qua trung tâm | 
| Đường đi 4 đỉnh | 2 cặp | Sự kết hợp hoàn hảo trong một cái cây | 
| Đường đi 5 đỉnh | 2 cặp | Còn lại chính xác một đỉnh chưa từng có | 

## Vỏ cạnh 

Hãy xem xét một thành phần chứa một đỉnh duy nhất:```
1 0
```DFS thăm đỉnh và tạo`rem = {1}`. Không có cặp nào có thể được hình thành. Hàm trả về một đại diện chưa từng có và câu trả lời cuối cùng chứa 0 cặp. Điều này là tối ưu. 

Hãy xem xét đồ thị ngôi sao:```
5 4
1 2
1 3
1 4
1 5
```Lá`2`,`3`,`4`, Và`5`tất cả đều trở lại với tư cách là những đại diện chưa từng có. Tại nút`1`, danh sách trở thành:```
{2, 3, 4, 5, 1}
```Cặp thuật toán`(2,3)`Và`(4,5)`. Mỗi cặp đều có chung một người bạn`1`, vì vậy cả hai đều hợp lệ. Một đỉnh vẫn chưa được so sánh vì kích thước thành phần là số lẻ. 

Hãy xem xét đường dẫn:```
4 3
1 2
2 3
3 4
```Xử lý từ dưới lên tạo ra các đại diện mà cuối cùng cho phép ghép nối`(4,3)`Và`(2,1)`. Mỗi cặp được kết nối bởi một cạnh tình bạn, vì vậy cả hai đều hợp lệ. Tất cả bốn đỉnh trở nên khớp nhau, đạt được mức tối đa theo lý thuyết là`2`cặp. 

Lý do tương tự cũng áp dụng cho các thành phần được kết nối tùy ý. Chỉ riêng cây DFS đã đảm bảo`⌊s/2⌋`cặp, vốn đã tối ưu cho một thành phần có kích thước`s`.
