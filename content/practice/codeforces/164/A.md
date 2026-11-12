---
title: "CF 164A - Biến, hoặc ở đó và quay lại"
description: "Chúng ta được cung cấp một biểu đồ có hướng trong đó mỗi nút biểu thị trạng thái chương trình. Mỗi trạng thái thực hiện một trong ba điều đối với một biến: 0 nghĩa là biến đó bị bỏ qua. 1 có nghĩa là biến được gán một giá trị mới. 2 có nghĩa là biến được sử dụng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 164
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2012 Round 3"
rating: 1700
weight: 164
solve_time_s: 111
verified: true
draft: false
---

[CF 164A - Có thể thay đổi, hoặc ở đó và quay lại](https://codeforces.com/problemset/problem/164/A) 

**Đánh giá:** 1700 
**Thẻ:** dfs và các biểu đồ tương tự 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng trong đó mỗi nút biểu thị trạng thái chương trình. Mỗi trạng thái thực hiện một trong ba điều đối với một biến:`0`có nghĩa là biến bị bỏ qua.`1`có nghĩa là biến được gán một giá trị mới.`2`có nghĩa là biến được sử dụng. 

Một trạng thái được gọi là thú vị nếu nó thuộc về ít nhất một đường dẫn có hướng có cấu trúc sau: 

Đường dẫn bắt đầu ở trạng thái gán, kết thúc ở trạng thái sử dụng và sau khi bắt đầu gán, không có phép gán nào khác trên đường dẫn. 

Điều kiện cuối cùng đó là toàn bộ độ khó của bài toán. Khi nhiệm vụ thứ hai xuất hiện, giá trị cũ sẽ bị ghi đè và không còn phù hợp nữa. 

Chúng ta phải xuất ra, đối với mỗi đỉnh, liệu nó có thể nằm trên một đường dẫn hợp lệ nào đó thuộc loại này hay không. 

Biểu đồ có thể chứa tới$10^5$đỉnh và$10^5$các cạnh. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào cố gắng khám phá tất cả các đường dẫn một cách rõ ràng. Ngay cả việc chạy DFS từ mọi nút gán một cách độc lập cũng sẽ trở nên quá tốn kém trong các trường hợp dày đặc. Chúng ta cần một cái gì đó gần với thời gian tuyến tính, đại khái là$O(n + m)$. 

Đồ thị cũng không được đảm bảo là không theo chu kỳ. Chu kỳ quan trọng vì khi chúng ta nhập một khu vực có thể truy cập được từ một nhiệm vụ mà không cần chạm vào một nhiệm vụ khác, mọi nút trong khu vực đó có thể vẫn hợp lệ vô thời hạn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét biểu đồ này:```
1(assign) -> 2(assign) -> 3(use)
```đầu vào:```
3 2
1 1 2
1 2
2 3
```Câu trả lời đúng là:```
0 1 1
```Trạng thái 1 không thú vị. Bất kỳ đường dẫn nào từ 1 đến nút sử dụng đều đi qua trạng thái 2, trạng thái này sẽ ghi đè giá trị trước tiên. Giải pháp về khả năng tiếp cận bất cẩn chỉ kiểm tra "có thể tiếp cận nút sử dụng" sẽ đánh dấu sai trạng thái 1. 

Một trường hợp tinh vi khác là khi bản thân nút gán cũng là nút thú vị duy nhất.```
1(assign) -> 2(ignore)
```đầu vào:```
2 1
1 0
1 2
```Đầu ra đúng:```
0 0
```Không đạt được trạng thái sử dụng nào, vì vậy giá trị được chỉ định không bao giờ quan trọng. 

Chu kỳ cũng cần được chăm sóc:```
1(assign) -> 2(ignore) -> 3(ignore)
                  ^         |
                  |---------|
3 -> 4(use)
```Tất cả bốn nút đều thú vị vì sau khi gán ở mức 1, chúng ta có thể lặp tùy ý nhiều lần trước khi đạt tới 4. Các thuật toán giả định cấu trúc DAG ở đây đều thất bại. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi nút gán, thực hiện duyệt đồ thị dừng lại bất cứ khi nào gặp một nút gán khác. Mỗi nút được truy cập mà cuối cùng có thể dẫn đến nút sử dụng đều thuộc về một đường dẫn hợp lệ. 

Điều này đúng vì quá trình truyền tải mô phỏng chính xác tất cả các đường dẫn có giá trị ban đầu tồn tại. Vấn đề là chi phí. Trong trường hợp xấu nhất có thể có$10^5$các nút gán và mỗi lần truyền tải có thể quét toàn bộ biểu đồ. Sự phức tạp trở nên$O(n(n+m))$, nó quá lớn. 

Quan sát quan trọng là chúng ta không thực sự quan tâm nút gán nào bắt đầu giá trị. Chúng tôi chỉ quan tâm liệu một nút có thể truy cập được từ một số nhiệm vụ mà không vượt qua một nhiệm vụ khác hay không và liệu từ nút đó, chúng tôi vẫn có thể đạt được trạng thái sử dụng trước khi một nhiệm vụ khác ghi đè giá trị hay không. 

Điều đó gợi ý việc truyền bá thông tin trên toàn cầu thay vì khởi động lại tìm kiếm nhiều lần. 

Chúng tôi xác định trạng thái là "còn sống" nếu giá trị hiện tại của biến vẫn hợp lệ ở đó. Từ nút gán, giá trị sẽ trở nên có hiệu lực. Việc duyệt qua các nút bỏ qua hoặc sử dụng sẽ bảo tồn nó. Chuyển sang một nhiệm vụ khác sẽ phá hủy giá trị cũ và bắt đầu một nhiệm vụ mới. 

Điều này biến vấn đề thành một vấn đề về khả năng tiếp cận đồ thị với các rào cản. 

Chúng ta có thể giải quyết nó theo hai giai đoạn. 

Đầu tiên, chúng tôi truyền tiếp từ tất cả các nút phân công, nhưng chúng tôi không bao giờ tiếp tục đi qua nút phân công khác. Mọi nút được truy cập đều có thể truy cập được trong khi vẫn giữ nguyên giá trị hiện tại. 

Thứ hai, trong số các nút có thể truy cập đó, chúng tôi xác định nút nào có thể tiếp cận nút sử dụng mà không cần vượt qua một nhiệm vụ khác. Điều này có thể được thực hiện bằng cách đảo ngược biểu đồ và truyền ngược từ tất cả các nút sử dụng, dừng lại ở ranh giới phân công. 

Một nút được coi là thú vị nếu cả hai điều kiện đều được giữ đồng thời. 

Toàn bộ quá trình chỉ truy cập vào mỗi cạnh một số lần không đổi, tạo ra độ phức tạp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n(n+m))$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O(n+m)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng và đồ thị ngược của nó. 

Biểu đồ ngược là cần thiết để truyền ngược từ các nút sử dụng. 
2. Bắt đầu BFS hoặc DFS đa nguồn từ mọi nút gán. 

Các nút này đại diện cho những nơi mà giá trị trở nên sống động. 
3. Trong quá trình truyền tải này, di chuyển qua các cạnh đi ra một cách bình thường, nhưng không tiếp tục từ một nút gán khác. 

Nếu chúng ta bắt đầu từ nút gán`s`, khi đó việc tiếp cận nút gán khác có nghĩa là giá trị ban đầu bị ghi đè ở đó. 
4. Đánh dấu mọi nút đã truy cập là`from_assign`. 

Điều này có nghĩa là nút có thể được truy cập trong khi mang một số giá trị biến hợp lệ. 
5. Bắt đầu một quá trình truyền tải đa nguồn khác, lần này là từ mọi nút sử dụng, nhưng trên biểu đồ đảo ngược. 

Di chuyển ngược lại trả lời câu hỏi: "nút này cuối cùng có thể dẫn đến trạng thái sử dụng không?" 
6. Một lần nữa, dừng truyền khi vượt qua các nút gán. 

Khi chúng tôi chuyển qua một nhiệm vụ khác, giá trị hiện tại sẽ thay đổi, do đó các trạng thái trước đó không còn đóng góp vào mức sử dụng cuối cùng nữa. 
7. Đánh dấu mọi nút đã truy cập là`to_use`. 

Điều này có nghĩa là tồn tại một đường dẫn từ nút đến trạng thái sử dụng mà không ghi đè giá trị trước. 
8. Đối với mỗi nút, xuất ra`1`nếu cả hai`from_assign[node]`Và`to_use[node]`là đúng. Nếu không thì xuất ra`0`. 

### Tại sao nó hoạt động 

Một nút thú vị chính xác khi nó nằm trên một đường dẫn bắt đầu bằng một phép gán và kết thúc bằng một cách sử dụng mà không có các phép gán trung gian. 

Việc truyền tải về phía trước xác định tất cả các nút có thể truy cập được từ một số phép gán trước khi giá trị bị ghi đè. 

Quá trình truyền tải ngược xác định tất cả các nút vẫn có thể đạt được mức sử dụng trước khi nhiệm vụ khác xuất hiện. 

Nếu cả hai điều kiện đều đúng cho một nút thì chúng ta có thể ghép hai đoạn đường dẫn tương ứng thành một đường dẫn hoàn chỉnh hợp lệ. Ngược lại, nếu một nút thuộc về một đường dẫn hợp lệ thì nó phải đáp ứng cả hai thuộc tính theo định nghĩa. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    f = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    rg = [[] for _ in range(n)]

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        g[a].append(b)
        rg[b].append(a)

    from_assign = [False] * n
    q = deque()

    for i in range(n):
        if f[i] == 1:
            from_assign[i] = True
            q.append(i)

    while q:
        u = q.popleft()

        for v in g[u]:
            if from_assign[v]:
                continue

            from_assign[v] = True

            if f[v] != 1:
                q.append(v)

    to_use = [False] * n
    q = deque()

    for i in range(n):
        if f[i] == 2:
            to_use[i] = True
            q.append(i)

    while q:
        u = q.popleft()

        for v in rg[u]:
            if to_use[v]:
                continue

            to_use[v] = True

            if f[v] != 1:
                q.append(v)

    ans = []

    for i in range(n):
        ans.append('1' if from_assign[i] and to_use[i] else '0')

    print('\n'.join(ans))

solve()
```Cả biểu đồ và biểu đồ ngược đều cần thiết vì hai đường duyệt này trả lời các câu hỏi về khả năng tiếp cận trái ngược nhau. 

BFS đầu tiên bắt đầu đồng thời từ tất cả các nút phân công. Chi tiết tinh tế là các nút gán vẫn được đánh dấu là có thể truy cập được, nhưng việc truyền bá sẽ dừng ở đó trừ khi bản thân nút đó là nguồn ban đầu. Mô hình này ghi đè chính xác. 

BFS thứ hai phản ánh logic tương tự trên biểu đồ đảo ngược. Bắt đầu từ các nút sử dụng và đi lùi sẽ xác định trạng thái nào cuối cùng có thể đóng góp vào việc sử dụng. 

Điều kiện:```
if f[v] != 1:
    q.append(v)
```là đường tới hạn trong cả hai lần duyệt. Chúng tôi vẫn đánh dấu các nút gán là có thể truy cập được, vì bản thân trạng thái gán có thể thú vị nhưng chúng tôi không bao giờ tiếp tục vượt quá chúng bằng cách sử dụng giá trị cũ. 

Câu trả lời cuối cùng là sự giao nhau của hai bộ khả năng tiếp cận. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
1 0 0 2
1 2
2 3
3 4
```Chuyển tiếp truyền tải: 

| Bước | Xếp hàng | Đã truy cập sau bước | 
| --- | --- | --- | 
| Bắt đầu | [1] | {1} | 
| Thăm 1 | [2] | {1,2} | 
| Thăm 2 | [3] | {1,2,3} | 
| Thăm 3 | [4] | {1,2,3,4} | 
| Thăm 4 | [] | {1,2,3,4} | 

Truyền tải ngược: 

| Bước | Xếp hàng | Đã truy cập sau bước | 
| --- | --- | --- | 
| Bắt đầu | [4] | {4} | 
| Thăm 4 | [3] | {3,4} | 
| Thăm 3 | [2] | {2,3,4} | 
| Thăm 2 | [1] | {1,2,3,4} | 
| Thăm 1 | [] | {1,2,3,4} | 

Mỗi nút thuộc về cả hai bộ, vì vậy tất cả các đầu ra đều`1`. 

Ví dụ này cho thấy chuỗi hợp lệ đơn giản nhất. Không có ghi đè nào xảy ra nên giá trị vẫn tồn tại trong toàn bộ đường dẫn. 

### Mẫu 2 

đầu vào:```
3 2
1 0 2
1 3
2 3
```Chuyển tiếp truyền tải: 

| Bước | Xếp hàng | Đã truy cập sau bước | 
| --- | --- | --- | 
| Bắt đầu | [1] | {1} | 
| Thăm 1 | [3] | {1,3} | 
| Thăm 3 | [] | {1,3} | 

Truyền tải ngược: 

| Bước | Xếp hàng | Đã truy cập sau bước | 
| --- | --- | --- | 
| Bắt đầu | [3] | {3} | 
| Thăm 3 | [1,2] | {1,2,3} | 
| Thăm 1 | [2] | {1,2,3} | 
| Thăm 2 | [] | {1,2,3} | 

Giao lộ: 

| Nút | từ_gán | to_use | Trả lời | 
| --- | --- | --- | --- | 
| 1 | Có | Có | 1 | 
| 2 | Không | Có | 0 | 
| 3 | Có | Có | 1 | 

Trạng thái 2 có thể tiếp cận nút sử dụng, nhưng không có nhiệm vụ nào có thể tiếp cận nút đó trước. Đó là lý do tại sao nó bị loại trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n+m)$| Mỗi lần duyệt truy cập vào mọi nút và cạnh nhiều nhất một lần | 
| Không gian |$O(n+m)$| Lưu trữ đồ thị cộng với mảng truy cập | 

Với$10^5$đỉnh và cạnh, độ phức tạp tuyến tính dễ dàng nằm gọn trong các giới hạn. Việc sử dụng bộ nhớ cũng an toàn vì danh sách kề chỉ lưu trữ mỗi cạnh một lần cho mỗi hướng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    n, m = map(int, input().split())
    f = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    rg = [[] for _ in range(n)]

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        g[a].append(b)
        rg[b].append(a)

    from_assign = [False] * n
    q = deque()

    for i in range(n):
        if f[i] == 1:
            from_assign[i] = True
            q.append(i)

    while q:
        u = q.popleft()

        for v in g[u]:
            if from_assign[v]:
                continue

            from_assign[v] = True

            if f[v] != 1:
                q.append(v)

    to_use = [False] * n
    q = deque()

    for i in range(n):
        if f[i] == 2:
            to_use[i] = True
            q.append(i)

    while q:
        u = q.popleft()

        for v in rg[u]:
            if to_use[v]:
                continue

            to_use[v] = True

            if f[v] != 1:
                q.append(v)

    ans = []

    for i in range(n):
        ans.append('1' if from_assign[i] and to_use[i] else '0')

    print('\n'.join(ans))

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run(
"""4 3
1 0 0 2
1 2
2 3
3 4
"""
) == "1\n1\n1\n1\n", "sample 1"

# overwrite blocks old value
assert run(
"""3 2
1 1 2
1 2
2 3
"""
) == "0\n1\n1\n", "overwrite case"

# no usage reachable
assert run(
"""2 1
1 0
1 2
"""
) == "0\n0\n", "no use state"

# cycle without overwrite
assert run(
"""4 4
1 0 0 2
1 2
2 3
3 2
3 4
"""
) == "1\n1\n1\n1\n", "cycle case"

# minimum size graph
assert run(
"""1 0
0
"""
) == "0\n", "minimum case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bài tập tiếp theo là bài tập khác |`0 1 1`| Giá trị cũ phải dừng ở ghi đè | 
| Bài tập không thể sử dụng được |`0 0`| Khả năng tiếp cận thôi là không đủ | 
| Chu kỳ trước khi sử dụng |`1 1 1 1`| Thuật toán xử lý chu trình chính xác | 
| Nút bị cô lập đơn |`0`| Trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Hãy xem xét lại kịch bản ghi đè:```
3 2
1 1 2
1 2
2 3
```Quá trình truyền tải về phía trước bắt đầu từ cả hai nút gán. 

Từ nút 1, quá trình truyền tải đến nút 2 nhưng dừng ở đó vì nút 2 là một phép gán khác. Giá trị cũ không thể tiếp tục nữa. 

Từ nút 2, truyền tải đến nút 3. 

Quá trình truyền ngược từ nút 3 đến nút 2, nhưng không thể tiếp tục qua nút 2 đến nút 1 vì nút 2 ghi đè giá trị. 

Giao lộ trở thành`{2,3}`chỉ, sản xuất:```
0
1
1
```Bây giờ hãy xem xét ví dụ về chu trình:```
4 4
1 0 0 2
1 2
2 3
3 2
3 4
```Quá trình truyền tải về phía trước từ nút 1 đi vào chu kỳ từ 2 đến 3 và đánh dấu cả hai nút đều có thể truy cập được. Vì cả hai nút đều không được gán nên việc truyền bá vẫn tiếp tục bình thường. 

Quá trình truyền ngược từ nút 4 đến nút 3 và sau đó là nút 2 thông qua các cạnh chu kỳ đảo ngược. 

Tất cả các nút đều thỏa mãn cả hai điều kiện, vì vậy mọi nút đều được đánh dấu là thú vị. 

Cuối cùng, hãy xem xét một biểu đồ có phép gán nhưng không có tác dụng:```
3 2
1 0 0
1 2
2 3
```Việc truyền tải về phía trước đánh dấu tất cả các nút có thể truy cập được từ một bài tập. Quá trình truyền ngược bắt đầu từ không có nút nào vì không có trạng thái sử dụng. 

các`to_use`mảng vẫn hoàn toàn sai, vì vậy mọi câu trả lời đều trở thành`0`. 

Điều này nắm bắt chính xác thực tế rằng một giá trị chỉ quan trọng nếu cuối cùng nó được sử dụng.
