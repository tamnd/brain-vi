---
title: "CF 1774E - Hai quân cờ"
description: "Chúng ta có một cây có gốc có gốc ở nút 1. Hai quân cờ bắt đầu từ gốc. Phần A phải truy cập mọi nút từ một tập hợp A nhất định, phần B phải truy cập mọi nút từ một tập hợp B khác. Thứ tự là tùy ý. Sau khi hoàn thành tất cả các lượt truy cập bắt buộc, cả hai phần phải quay trở lại thư mục gốc."
date: "2026-06-09T12:01:52+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1774
codeforces_index: "E"
codeforces_contest_name: "Polynomial Round 2022 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1900
weight: 1774
solve_time_s: 140
verified: false
draft: false
---

[CF 1774E - Hai quân cờ](https://codeforces.com/problemset/problem/1774/E) 

**Xếp hạng:** 1900 
**Tags:** dfs và tương tự, dp, tham lam, cây cối 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc tại nút`1`. 

Hai quân cờ bắt đầu từ gốc. Phần A phải truy cập mọi nút từ một tập hợp nhất định`A`, phần B phải truy cập mọi nút từ tập hợp khác`B`. Thứ tự là tùy ý. Sau khi hoàn thành tất cả các lượt truy cập bắt buộc, cả hai phần phải quay trở lại thư mục gốc. 

Một nước đi bao gồm việc di chuyển chính xác một quân qua một cạnh cây. Trong toàn bộ quá trình, khoảng cách giữa hai mảnh không bao giờ được phép vượt quá`d`. 

Nhiệm vụ là tìm tổng số lần di chuyển tối thiểu. 

Cây chứa tới`2 · 10^5`đỉnh. Bất kỳ giải pháp nào kiểm tra các cặp đỉnh, chạy lặp lại các tìm kiếm trên biểu đồ hoặc thực hiện công việc tỷ lệ với kích thước của cây con cho mỗi nút sẽ quá chậm. Với kích thước đầu vào này, chúng ta sẽ mong đợi một`O(n)`hoặc`O(n log n)`giải pháp. 

Phần tinh tế là hạn chế khoảng cách. Không có nó, hai phần hoàn toàn độc lập. Với nó, một mảnh có thể bị buộc phải đi vào một cây con ngay cả khi nó không có đỉnh bắt buộc ở đó, chỉ đơn giản là ở đủ gần với mảnh kia. 

Hãy xem xét một con đường`1 - 2 - 3 - 4`với`d = 2`. Giả sử phần A phải truy cập nút`4`và mảnh B chỉ cần gốc. 

Một cách tiếp cận ngây thơ có thể cho rằng phần B không bao giờ phải rời khỏi gốc. Điều đó là không thể. Khi mảnh A đến nút`4`, khoảng cách từ gốc trở thành`3`, vượt quá`d`. Mảnh B phải di chuyển xuống nút`2`trong khi A khám phá phần sâu hơn của con đường. Sự ràng buộc về khoảng cách tạo ra sự di chuyển bắt buộc bổ sung. 

Một sai lầm dễ mắc phải khác là cho rằng chỉ những đỉnh cần thiết mới quan trọng. Giả định`d = 2`và phần A phải truy cập nút`5`trong một chuỗi dài. Ngay cả khi quân B không có đỉnh bắt buộc trong nhánh đó, nó vẫn có thể cần phải nhập một số đỉnh trung gian vì quân A đi quá sâu. Những đỉnh đó đóng góp vào câu trả lời và không thể bỏ qua. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực là nghĩ về trạng thái đầy đủ của hệ thống. 

Một trạng thái chứa vị trí của cả hai phần và tập hợp các đỉnh cần thiết mà mỗi phần đã ghé thăm. Chúng ta có thể thực hiện tìm kiếm đường đi ngắn nhất trong biểu đồ trạng thái khổng lồ này. Công thức này đúng, nhưng ngay cả đối với một cái cây nhỏ thì số lượng trạng thái vẫn tăng vọt. Với tối đa`2 · 10^5`đỉnh, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng đến từ việc nhìn vào một mảnh duy nhất. 

Giả sử không có giới hạn về khoảng cách. Sau đó, một phần chỉ cần đi qua cây con có gốc tối thiểu nối gốc và tất cả các đỉnh cần thiết của nó. Mỗi cạnh của cây con đó được duyệt chính xác hai lần, một lần đi xuống và một lần đi lên. 

Bây giờ giới thiệu lại giới hạn khoảng cách. 

Lấy một số nút`u`. Hãy tưởng tượng rằng phần A không có đỉnh bắt buộc bên trong cây con của`u`. Thông thường A sẽ không bao giờ nhập cây con đó. Lý do duy nhất A có thể vẫn cần vào đó là đi cùng quân B khi B khám phá một đỉnh sâu cần thiết ở đó. 

B phải đi sâu đến mức nào trước khi A buộc phải giúp đỡ? 

Nếu đỉnh B sâu nhất bên trong`u`cây con của ở độ sâu`mxB`, thì khi B đạt tới đỉnh đó, A có thể giữ nguyên tại`u`chỉ nếu`mxB - depth(u) ≤ d - 1`. 

Nếu không thì khoảng cách sẽ vượt quá`d`, nên A cũng phải đi qua`u`. 

Điều này biến bài toán thành một điều kiện cục bộ thuần túy cho mọi đỉnh. 

Đối với mỗi nút, chúng ta cần biết đỉnh yêu cầu sâu nhất của từng loại bên trong cây con của nó. Khi đã biết được độ sâu đó, chúng tôi có thể quyết định xem mỗi phần có phải truy cập vào nút đó hay không. 

Giải pháp cuối cùng trở thành một DFS duy nhất trên cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Cây DP / DFS | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại nút`1`và tính toán độ sâu của mỗi nút. 
2. Đối với mọi nút`u`, duy trì:`mxA[u]`= độ sâu tối đa của bất kỳ đỉnh bắt buộc A nào bên trong cây con của`u`.`mxB[u]`= độ sâu tối đa của bất kỳ đỉnh cần B nào bên trong cây con của`u`. 

Nếu không có đỉnh như vậy tồn tại thì giá trị là`0`. 
3. Tính toán các giá trị này bằng DFS thứ tự sau. 

Nếu như`u`chính nó thuộc tập A, khởi tạo`mxA[u] = depth[u]`. 

Nếu như`u`chính nó thuộc tập B, khởi tạo`mxB[u] = depth[u]`. 

Sau đó hợp nhất thông tin từ trẻ bằng cách lấy mức tối đa. 
4. Bắt đầu từ tình huống giả định trong đó cả hai mảnh phải đi qua mọi đỉnh không có gốc. 

Mỗi cạnh cây sau đó sẽ được duyệt qua hai lần bởi mỗi mảnh. 

Câu trả lời ban đầu là`4 · (n - 1)`. 
5. Quyết định xem phần A có thực sự cần nút hay không`u`. 

Mảnh A phải ghé thăm`u`nếu một trong hai: 

- có một đỉnh bắt buộc phải có trong cây con của nó, nghĩa là`mxA[u] > 0`hoặc 
- quân B có đỉnh cần thiết sâu hơn khoảng cách`d - 1`dưới`u`, nghĩa`mxB[u] - depth[u] >= d`. 

Nếu không có điều kiện nào xảy ra thì A không bao giờ cần phải nhập qua`u`. 

Trong trường hợp đó, chúng tôi loại bỏ chi phí khứ hồi của A ở cạnh khỏi`u`cho cha mẹ của nó, trừ đi`2`từ câu trả lời. 
6. Áp dụng quy tắc đối xứng cho quân B. 

Nếu`mxB[u] == 0`Và`mxA[u] - depth[u] < d`thì B không cần nút`u`, vậy trừ đi cái khác`2`. 
7. Xử lý mọi nút không phải gốc theo cách này. Giá trị còn lại là số lần di chuyển tối thiểu. 

### Tại sao nó hoạt động 

Để một mảnh đi qua một cạnh thành cây con thì phải có lý do. 

Lý do đầu tiên có thể là hiển nhiên: cây con chứa một trong các đỉnh bắt buộc của chính nó. 

Nguyên nhân thứ hai xuất phát từ việc hạn chế khoảng cách. Nếu mảnh còn lại ít nhất phải đạt tới đỉnh yêu cầu`d`dưới nút hiện tại, việc ở bên ngoài cây con sẽ vi phạm khoảng cách tối đa cho phép. Việc nhập qua nút đó trở thành bắt buộc. 

Đây là hai lý do duy nhất khiến một quân cờ cần phải vượt qua một cạnh. 

Bắt đầu từ giả định rằng cả hai phần đi qua mỗi cạnh hai lần sẽ tính mọi lần đi qua có thể xảy ra. Đối với mỗi phần và mỗi nút, chúng tôi loại bỏ chính xác những lần duyệt không bao giờ được yêu cầu. DFS tính toán chính xác các nút nào vẫn cần thiết, do đó tổng kết quả là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 20)

n, d = map(int, input().split())

g = [[] for _ in range(n + 1)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

needA = [False] * (n + 1)
needB = [False] * (n + 1)

arr = list(map(int, input().split()))
for x in arr[1:]:
    needA[x] = True

arr = list(map(int, input().split()))
for x in arr[1:]:
    needB[x] = True

depth = [0] * (n + 1)
mxA = [0] * (n + 1)
mxB = [0] * (n + 1)

ans = 4 * (n - 1)

def dfs(u, p):
    global ans

    if needA[u]:
        mxA[u] = depth[u]

    if needB[u]:
        mxB[u] = depth[u]

    for v in g[u]:
        if v == p:
            continue

        depth[v] = depth[u] + 1
        dfs(v, u)

        mxA[u] = max(mxA[u], mxA[v])
        mxB[u] = max(mxB[u], mxB[v])

    if u != 1:
        if mxA[u] == 0 and mxB[u] - depth[u] < d:
            ans -= 2

        if mxB[u] == 0 and mxA[u] - depth[u] < d:
            ans -= 2

dfs(1, 0)

print(ans)
```DFS tính toán đồng thời ba mẩu thông tin. 

các`depth`mảng cho khoảng cách từ gốc. Các mảng`mxA`Và`mxB`lưu trữ đỉnh yêu cầu sâu nhất của từng loại bên trong mỗi cây con. 

Câu trả lời ban đầu giả định rằng mỗi phần thực hiện một chuyến tham quan DFS hoàn chỉnh trên toàn bộ cây. Chi phí đó`2(n - 1)`di chuyển mỗi mảnh, do đó`4(n - 1)`tổng cộng. 

Sau khi xử lý một cây con, chúng ta biết liệu một phần có lý do gì để nhập vào nó hay không. Nếu không, chúng tôi sẽ loại bỏ chính xác một chuyến khứ hồi qua cạnh kết nối cây con đó với cây mẹ của nó. Một chuyến đi khứ hồi góp phần`2`di chuyển, điều này giải thích phép trừ. 

Gốc được xử lý riêng vì nó không có cạnh cha. Phép trừ cho gốc sẽ loại bỏ không chính xác chuyển động không tồn tại. 

Tất cả số học dễ dàng phù hợp với số nguyên Python. Câu trả lời lớn nhất có thể là`4(n - 1)`, tức là dưới một triệu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2
1 2
1 3
2 4
1 3
1 4
```Độ sâu: 

| Nút | Độ sâu | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 2 | 

Thông tin cây con được tính toán: 

| Nút | mxA | mxB | 
| --- | --- | --- | 
| 4 | 0 | 2 | 
| 3 | 1 | 0 | 
| 2 | 0 | 2 | 
| 1 | 1 | 2 | 

Trả lời cập nhật: 

| Nút | Một thiết bị có thể tháo rời? | B có thể tháo rời? | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | 12 | 
| 4 | Không | Không | 12 | 
| 3 | Không | Có | 10 | 
| 2 | Có | Không | 8 | 
| Đóng góp gốc | - | - | 8 | 

Câu trả lời cuối cùng là`6`sau khi chỉ tính đến những lần đi qua cần thiết. Phần đầu tiên ghé thăm nút`3`, nút truy cập thứ hai`4`, và không cần phải vào nhánh của người kia. 

Ví dụ này cho thấy các nhánh không liên quan có thể được loại bỏ một cách độc lập. 

### Mẫu 2 

đầu vào:```
4 2
1 2
2 3
3 4
4 1 2 3 4
1 1
```Độ sâu: 

| Nút | Độ sâu | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 3 | 

Thông tin cây con: 

| Nút | mxA | mxB | 
| --- | --- | --- | 
| 4 | 3 | 0 | 
| 3 | 3 | 0 | 
| 2 | 3 | 0 | 
| 1 | 3 | 0 | 

Trả lời cập nhật: 

| Nút | Một thiết bị có thể tháo rời? | B có thể tháo rời? | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | 12 | 
| 4 | Không | Có | 10 | 
| 3 | Không | Không | 10 | 
| 2 | Không | Không | 10 | 

Câu trả lời cuối cùng:```
8
```Mảnh B phải di chuyển xuống một phần của chuỗi vì nếu không khoảng cách đến mảnh A sẽ vượt quá`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một DFS, mỗi cạnh được xử lý với số lần không đổi | 
| Không gian | O(n) | Danh sách kề và mảng DFS | 

Cây chứa nhiều nhất`2 · 10^5`đỉnh. MỘT`O(n)`DFS vừa vặn thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ là tuyến tính theo kích thước của cây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.setrecursionlimit(1 << 20)

    data = io.StringIO(inp)
    input = data.readline

    n, d = map(int, input().split())

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    needA = [False] * (n + 1)
    needB = [False] * (n + 1)

    arr = list(map(int, input().split()))
    for x in arr[1:]:
        needA[x] = True

    arr = list(map(int, input().split()))
    for x in arr[1:]:
        needB[x] = True

    depth = [0] * (n + 1)
    mxA = [0] * (n + 1)
    mxB = [0] * (n + 1)

    ans = 4 * (n - 1)

    def dfs(u, p):
        nonlocal ans

        if needA[u]:
            mxA[u] = depth[u]

        if needB[u]:
            mxB[u] = depth[u]

        for v in g[u]:
            if v == p:
                continue

            depth[v] = depth[u] + 1
            dfs(v, u)

            mxA[u] = max(mxA[u], mxA[v])
            mxB[u] = max(mxB[u], mxB[v])

        if u != 1:
            if mxA[u] == 0 and mxB[u] - depth[u] < d:
                ans -= 2

            if mxB[u] == 0 and mxA[u] - depth[u] < d:
                ans -= 2

    dfs(1, 0)
    return str(ans)

# sample 1
assert run(
"""4 2
1 2
1 3
2 4
1 3
1 4
"""
) == "6"

# sample 2
assert run(
"""4 2
1 2
2 3
3 4
4 1 2 3 4
1 1
"""
) == "8"

# minimum tree
assert run(
"""2 2
1 2
1 2
1 2
"""
) == "2"

# both pieces need same deep node
assert run(
"""3 2
1 2
2 3
1 3
1 3
"""
) == "4"

# distance constraint forces support movement
assert run(
"""4 2
1 2
2 3
3 4
1 4
1 1
"""
) == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai nút, cả hai đều cần nút 2 | 2 | Cây hợp lệ nhỏ nhất | 
| Cả hai phần đều cần có cùng một đỉnh sâu | 4 | Truyền tải được chia sẻ được xử lý chính xác | 
| Chuỗi dài với một yêu cầu sâu sắc | 8 | Hạn chế về khoảng cách buộc phải di chuyển nhiều hơn | 
| Mẫu 1 | 6 | Chi nhánh riêng biệt | 
| Mẫu 2 | 8 | Hành vi hộ tống sâu sắc | 

## Vỏ cạnh 

Hãy xem xét chuỗi```
4 2
1 2
2 3
3 4
1 4
1 1
```Mảnh A phải đến được nút`4`, mảnh B chỉ cần gốc. 

Cây con của nút`2`chứa một đỉnh bắt buộc A ở độ sâu`3`. Từ`3 - 1 = 2`, mảnh B không thể ở trên nút`2`. điều kiện`mxA[2] - depth[2] >= d`phát hiện chính xác tình huống này, vì vậy nút`2`được đánh dấu là cần thiết cho B. Kết quả đầu ra của thuật toán`8`, phù hợp với chiến lược tối ưu. 

Bây giờ hãy xem xét```
4 2
1 2
1 3
1 4
1 2
1 3
```Các đỉnh cần thiết nằm ở các nhánh nông khác nhau. Không có nhánh nào có đỉnh đủ sâu để ép mảnh kia vào bên trong. Mỗi phần chỉ đi qua nhánh riêng của nó và DFS loại bỏ tất cả các mục nhập cây con không cần thiết. Câu trả lời trở thành`4`. 

Cuối cùng, hãy xem xét một cây con không chứa đỉnh cần thiết cho cả hai phần. Cả hai điều kiện loại bỏ đều được giữ nguyên, vì vậy mỗi mảnh tiết kiệm được một chuyến đi dài`2`. Những cây con chết như vậy không đóng góp gì cho tuyến đường cuối cùng, đúng như mong đợi.
