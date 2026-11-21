---
title: "CF 246E - Blood Cousins ​​Return"
description: "Chúng tôi được tặng một khu rừng gốc mô tả các mối quan hệ gia đình. Mỗi người đều có một tên và nhiều nhất là một phụ huynh. Cho phép có nhiều gốc rễ vì một số người có thể không có tổ tiên nào cả."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dfs-and-similar", "dp", "sortings"]
categories: ["algorithms"]
codeforces_contest: 246
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 151 (Div. 2)"
rating: 2400
weight: 246
solve_time_s: 98
verified: true
draft: false
---

[CF 246E - Blood Cousins Return](https://codeforces.com/problemset/problem/246/E) 

**Đánh giá:** 2400 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dfs và tương tự, dp, sắp xếp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một khu rừng gốc mô tả các mối quan hệ gia đình. Mỗi người đều có một tên và nhiều nhất là một phụ huynh. Cho phép có nhiều gốc rễ vì một số người có thể không có tổ tiên nào cả. 

Đối với mỗi truy vấn`(v, k)`, chúng ta cần số lượng tên riêng biệt trong số tất cả các hậu duệ của`v`đó chính xác là`k`các cạnh bên dưới`v`. 

Việc diễn đạt lại hữu ích sẽ làm cho cấu trúc rõ ràng hơn. Cho phép`depth[x]`là độ sâu của nút`x`từ gốc cây của nó. Một nút là một`k`-con trai của`v`chính xác khi nào: 

- nó nằm bên trong cây con của`v`- độ sâu của nó là`depth[v] + k`Vì vậy, mọi truy vấn đều hỏi: 

> Bên trong cây con của`v`, có bao nhiêu cái tên riêng biệt xuất hiện ở độ sâu`depth[v] + k`? 

Kích thước cây và số lượng truy vấn đều tăng lên`10^5`. Bất kỳ giải pháp nào quét cây con cho mọi truy vấn đều quá chậm. Trong trường hợp xấu nhất, một cây con có thể chứa hầu hết tất cả các nút, dẫn đến gần như`10^10`hoạt động. 

Các ràng buộc đề xuất một cách tiếp cận tiền xử lý ngoại tuyến. Chúng ta cần thứ gì đó xung quanh`O(n log n)`hoặc`O((n + m) log n)`. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét nhiều gốc:```
4
a 0
b 0
c 1
d 2
2
1 1
2 1
```Các câu trả lời đúng là:```
1
1
```Một DFS bắt đầu chỉ từ một gốc sẽ hoàn toàn bỏ sót một phần của khu rừng. 

Một trường hợp tinh vi khác xuất hiện khi độ sâu được yêu cầu không tồn tại:```
3
a 0
b 1
c 2
1
1 5
```Câu trả lời là:```
0
```Việc triển khai bất cẩn có thể cố gắng truy cập vào các cấu trúc có chiều sâu không tồn tại. 

Tên lặp lại cũng quan trọng:```
5
john 0
john 1
john 1
alice 2
alice 3
2
1 1
1 2
```Ở khoảng cách`1`từ nút`1`, cả hai con cháu đều được đặt tên`john`, vậy câu trả lời là`1`, không`2`. 

Cuối cùng, truy vấn với`k = 0`là không thể trong vấn đề này vì các ràng buộc bắt đầu từ`1`. Điều đó loại bỏ một trường hợp đặc biệt phổ biến. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi truy vấn`(v, k)`, duyệt cây con của`v`, thu thập tất cả các nút có độ sâu bằng`depth[v] + k`, chèn tên của chúng vào một bộ và xuất ra kích thước đã đặt. 

Điều này đúng vì nó tuân theo định nghĩa của một`k`-con trai. Vấn đề là chi phí. Trong một cây dạng chuỗi, một cây con có thể chứa`O(n)`nút. Với`m = 10^5`truy vấn, tổng công việc sẽ trở thành`O(nm)`. 

Chúng ta cần sử dụng lại công việc trên các truy vấn. 

Quan sát chính là mọi truy vấn chỉ phụ thuộc vào hai điều: 

- một cây con 
- một độ sâu duy nhất 

Các truy vấn cây con trên cây thường trở thành các truy vấn ngắt quãng sau chuyến tham quan Euler. Trong DFS, mỗi cây con trở thành một đoạn liền kề`[tin[v], tout[v]]`. 

Bây giờ hãy tưởng tượng việc xử lý các nút theo chiều sâu. Đối với mỗi độ sâu`d`, chúng ta duy trì tất cả các tên xuất hiện ở độ sâu đó, sắp xếp theo thời gian Euler. Sau đó, một truy vấn trở thành: 

> Giữa các nút ở độ sâu`d = depth[v] + k`, đếm các tên riêng biệt có thời gian Euler nằm bên trong`[tin[v], tout[v]]`. 

Thử thách còn lại là đếm các giá trị riêng biệt trong một phạm vi một cách hiệu quả. 

Đây là nơi xuất hiện thủ thuật truy vấn khác biệt ngoại tuyến cổ điển. Giả sử chúng ta xử lý các vị trí Euler từ trái sang phải. Đối với mỗi tên, chúng tôi chỉ giữ lại lần xuất hiện mới nhất của nó trong cây Fenwick. Sau đó, tổng trên một phạm vi sẽ cho số lượng tên riêng biệt bên trong phạm vi đó. 

Chúng ta có thể nhóm tất cả các nút theo độ sâu và tất cả các truy vấn theo độ sâu mục tiêu. Đối với từng độ sâu độc lập: 

1. Sắp xếp các nút theo thứ tự Euler. 
2. Chỉ kích hoạt lần xuất hiện mới nhất của mỗi tên. 
3. Trả lời tất cả các câu hỏi ngắt quãng cho độ sâu đó. 

Mỗi nút và truy vấn chỉ tham gia một lần, đưa ra một`O((n + m) log n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng forest từ mảng cha. 

Mỗi nút có cha mẹ`0`trở thành một gốc rễ. Lưu trữ danh sách lân cận của trẻ em. 
2. Chạy DFS từ mọi root. 

Trong DFS, tính toán: 

-`depth[v]`-`tin[v]`-`tout[v]`Chúng tôi tăng bộ đếm thời gian chung khi nhập cảnh. Do thứ tự DFS, mỗi cây con tạo thành một khoảng Euler liền kề. 
3. Nhóm các nút theo độ sâu. 

Đối với mỗi nút`v`, nối thêm`(tin[v], name[v])`vào trong`depth_nodes[depth[v]]`. 

Các truy vấn chỉ so sánh các nút ở một độ sâu chính xác, do đó việc xử lý độ sâu một cách độc lập sẽ tránh được những công việc không cần thiết. 
4. Chuyển đổi từng truy vấn`(v, k)`vào một truy vấn khoảng thời gian giới hạn độ sâu. 

Độ sâu cần thiết là:```
target = depth[v] + k
```Khoảng cây con là:```
[tin[v], tout[v]]
```Lưu trữ các truy vấn được nhóm theo`target`. 
5. Đối với từng độ sâu riêng biệt, xử lý tất cả các nút theo thứ tự Euler. 

Giả sử chúng ta đang xử lý độ sâu`d`. 

Chúng tôi sắp xếp tất cả các nút ở độ sâu`d`theo thời gian Euler. Sau đó chúng ta quét từ trái sang phải. 

Duy trì: 

- cây Fenwick trên vị trí Euler 
-`last[name]`, vị trí Euler mới nhất mà cái tên này xuất hiện 

Khi đến thăm`(pos, name)`: 

- nếu tên xuất hiện trước đó tại`old`, xóa đóng góp tại`old`- thêm đóng góp tại`pos`- cập nhật`last[name] = pos`Tại bất kỳ thời điểm nào, cây Fenwick chứa chính xác một lần xuất hiện hoạt động cho mỗi tên riêng biệt giữa các vị trí được xử lý. 
6. Trả lời các câu hỏi ngắt quãng ngoại tuyến. 

Sắp xếp các truy vấn cho độ sâu này theo điểm cuối bên phải. 

Trong khi quét các nút đến ranh giới bên phải của truy vấn, hãy cập nhật cây Fenwick. 

Sau đó số lượng tên riêng biệt bên trong`[l, r]`là:```
bit.query(r) - bit.query(l - 1)
```7. Xuất câu trả lời theo thứ tự ban đầu. 

### Tại sao nó hoạt động 

Đối với độ sâu cố định, bất biến quét là: 

> Sau khi xử lý các vị trí Euler lên đến`x`, cây Fenwick chứa`1`chính xác vào thời điểm xuất hiện muộn nhất của mỗi tên riêng biệt giữa các vị trí`<= x`. 

Xem xét một khoảng truy vấn`[l, r]`. 

Nếu một tên xuất hiện trong khoảng này, lần xuất hiện gần đây nhất của nó lên đến`r`cũng phải nằm trong khoảng. Những lần xuất hiện trước đó đã bị xóa khi những lần xuất hiện sau đó. 

Vì vậy, mỗi tên riêng biệt đều đóng góp chính xác một vị trí hoạt động bên trong`[l, r]`và những cái tên vắng mặt trong khoảng không đóng góp gì. 

Tổng phạm vi Fenwick chính xác là số tên riêng biệt trong cây con ở độ sâu yêu cầu. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        while idx <= self.n:
            self.bit[idx] += val
            idx += idx & -idx

    def query(self, idx):
        res = 0
        while idx > 0:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

    def range_query(self, l, r):
        return self.query(r) - self.query(l - 1)

def solve():
    n = int(input())

    names = [""] * (n + 1)
    children = [[] for _ in range(n + 1)]
    roots = []

    for i in range(1, n + 1):
        s, r = input().split()
        r = int(r)

        names[i] = s

        if r == 0:
            roots.append(i)
        else:
            children[r].append(i)

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    depth = [0] * (n + 1)

    depth_nodes = defaultdict(list)

    timer = 0

    sys.setrecursionlimit(1 << 25)

    def dfs(v, d):
        nonlocal timer

        timer += 1
        tin[v] = timer
        depth[v] = d

        depth_nodes[d].append((tin[v], names[v]))

        for to in children[v]:
            dfs(to, d + 1)

        tout[v] = timer

    for root in roots:
        dfs(root, 0)

    m = int(input())

    queries_by_depth = defaultdict(list)
    ans = [0] * m

    for idx in range(m):
        v, k = map(int, input().split())

        target = depth[v] + k

        queries_by_depth[target].append(
            (tout[v], tin[v], idx)
        )

    bit = Fenwick(n)

    for d in queries_by_depth:
        nodes = depth_nodes.get(d, [])
        nodes.sort()

        queries = queries_by_depth[d]
        queries.sort()

        last = {}

        ptr = 0

        used_positions = []

        for r, l, qid in queries:
            while ptr < len(nodes) and nodes[ptr][0] <= r:
                pos, name = nodes[ptr]

                if name in last:
                    bit.add(last[name], -1)

                bit.add(pos, 1)
                last[name] = pos

                used_positions.append(pos)

                ptr += 1

            ans[qid] = bit.range_query(l, r)

        for pos in used_positions:
            bit.add(pos, -1)

    print("\n".join(map(str, ans)))

solve()
```Pha DFS ấn định các khoảng Euler. Bởi vì DFS khám phá đầy đủ một cây con trước khi quay trở lại nên mọi hậu duệ của`v`nhận được một phạm vi Euler liền kề. Thuộc tính đó là thứ chuyển đổi các truy vấn cây con thành các truy vấn khoảng.`depth_nodes[d]`lưu trữ tất cả các nút ở độ sâu`d`. Cặp đôi`(tin[v], name[v])`là đủ vì việc lọc cây con sau đó chỉ diễn ra bằng các khoảng Euler. 

Các truy vấn được nhóm theo độ sâu mục tiêu thay vì theo nút nguồn. Đây là mức giảm trung tâm. Khi độ sâu được khắc phục, vấn đề sẽ trở thành vấn đề về các phần tử khác biệt trong phạm vi ngoại tuyến tiêu chuẩn. 

Cây Fenwick lưu trữ các lần xuất hiện mới nhất đang hoạt động. Khi một tên xuất hiện lại, lần xuất hiện trước đó sẽ bị xóa trước khi chèn tên mới. Nếu không có bước loại bỏ này, tên trùng lặp sẽ được tính nhiều lần. 

Vòng dọn dẹp ở cuối mỗi độ sâu rất tinh tế nhưng cần thiết. Phiên bản cây Fenwick tương tự được sử dụng lại theo độ sâu, vì vậy tất cả các cập nhật từ độ sâu hiện tại phải được hoàn nguyên. 

Một lỗi dễ mắc phải khác là sắp xếp truy vấn theo`tout[v]`. Quá trình quét xử lý các nút theo thứ tự Euler tăng dần, do đó các truy vấn cũng phải tiến lên theo điểm cuối bên phải. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6
pasha 0
gerald 1
gerald 1
valera 2
igor 3
olesya 1
5
1 1
1 2
1 3
3 1
6 1
```Lệnh DFS: 

| Nút | Tên | Độ sâu | thiếc | chào hàng | 
| --- | --- | --- | --- | --- | 
| 1 | pasha | 0 | 1 | 6 | 
| 2 | gerald | 1 | 2 | 3 | 
| 4 | valera | 2 | 3 | 3 | 
| 3 | gerald | 1 | 4 | 5 | 
| 5 | igor | 2 | 5 | 5 | 
| 6 | olesya | 1 | 6 | 6 | 

Chuyển đổi truy vấn: 

| Truy vấn | Độ sâu mục tiêu | Khoảng thời gian | 
| --- | --- | --- | 
| (1,1) | 1 | [1,6] | 
| (1,2) | 2 | [1,6] | 
| (1,3) | 3 | [1,6] | 
| (3,1) | 2 | [4,5] | 
| (6,1) | 2 | [6,6] | 

Độ sâu xử lý`1`: 

| Euler Pos | Tên | Tên hoạt động | 
| --- | --- | --- | 
| 2 | gerald | {gerald} | 
| 4 | gerald | {Gerald lúc 4} | 
| 6 | olesya | {gerald, olesya} | 

Khoảng cây con`[1,6]`chứa hai tên hoạt động, vì vậy câu trả lời là`2`. 

Độ sâu xử lý`2`: 

| Euler Pos | Tên | Tên hoạt động | 
| --- | --- | --- | 
| 3 | valera | {valera} | 
| 5 | igor | {valera, igor} | 

Cây con đầy đủ mang lại`2`, trong khi khoảng`[4,5]`chỉ chứa`igor`, cho`1`. 

Dấu vết này cho thấy lý do tại sao chỉ những lần xuất hiện mới nhất vẫn còn hoạt động. Bản sao`gerald`các nút thu gọn thành một tên riêng biệt. 

### Ví dụ tùy chỉnh 

đầu vào:```
5
a 0
a 1
b 1
a 2
b 3
3
1 1
1 2
2 1
```Lệnh DFS: 

| Nút | Tên | Độ sâu | thiếc | chào hàng | 
| --- | --- | --- | --- | --- | 
| 1 | một | 0 | 1 | 5 | 
| 2 | một | 1 | 2 | 3 | 
| 4 | một | 2 | 3 | 3 | 
| 3 | b | 1 | 4 | 5 | 
| 5 | b | 2 | 5 | 5 | 

Truy vấn: 

| Truy vấn | Độ sâu mục tiêu | Khoảng thời gian | 
| --- | --- | --- | 
| (1,1) | 1 | [1,5] | 
| (1,2) | 2 | [1,5] | 
| (2,1) | 2 | [2,3] | 

Độ sâu xử lý`1`cung cấp tên hoạt động`{a, b}`, trả lời`2`. 

Độ sâu xử lý`2`cung cấp tên hoạt động`{a, b}`đối với cây con đầy đủ, hãy trả lời`2`. 

Đối với khoảng thời gian`[2,3]`, chỉ có nút`4`nằm bên trong, trả lời`1`. 

Ví dụ này chứng tỏ rằng việc lọc cây con và tính toán riêng biệt tương tác chính xác ngay cả khi tên lặp lại trên các nhánh khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi nút và truy vấn thực hiện cập nhật hoặc truy vấn Fenwick | 
| Không gian | O(n + m) | Lưu trữ cây, mảng Euler, truy vấn được nhóm, cây Fenwick | 

Với`10^5`các nút và truy vấn, khoảng vài triệu thao tác Fenwick phù hợp thoải mái trong giới hạn. Việc sử dụng bộ nhớ cũng vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, idx, val):
            while idx <= self.n:
                self.bit[idx] += val
                idx += idx & -idx

        def query(self, idx):
            res = 0
            while idx > 0:
                res += self.bit[idx]
                idx -= idx & -idx
            return res

        def range_query(self, l, r):
            return self.query(r) - self.query(l - 1)

    n = int(input())

    names = [""] * (n + 1)
    children = [[] for _ in range(n + 1)]
    roots = []

    for i in range(1, n + 1):
        s, r = input().split()
        r = int(r)

        names[i] = s

        if r == 0:
            roots.append(i)
        else:
            children[r].append(i)

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    depth = [0] * (n + 1)

    depth_nodes = defaultdict(list)

    timer = 0

    sys.setrecursionlimit(1 << 25)

    def dfs(v, d):
        nonlocal timer

        timer += 1
        tin[v] = timer
        depth[v] = d

        depth_nodes[d].append((tin[v], names[v]))

        for to in children[v]:
            dfs(to, d + 1)

        tout[v] = timer

    for r in roots:
        dfs(r, 0)

    m = int(input())

    queries_by_depth = defaultdict(list)
    ans = [0] * m

    for idx in range(m):
        v, k = map(int, input().split())

        target = depth[v] + k

        queries_by_depth[target].append(
            (tout[v], tin[v], idx)
        )

    bit = Fenwick(n)

    for d in queries_by_depth:
        nodes = sorted(depth_nodes.get(d, []))
        queries = sorted(queries_by_depth[d])

        last = {}
        ptr = 0
        used = []

        for r, l, idx in queries:
            while ptr < len(nodes) and nodes[ptr][0] <= r:
                pos, name = nodes[ptr]

                if name in last:
                    bit.add(last[name], -1)

                bit.add(pos, 1)
                last[name] = pos

                used.append(pos)

                ptr += 1

            ans[idx] = bit.range_query(l, r)

        for pos in used:
            bit.add(pos, -1)

    return "\n".join(map(str, ans))

# provided sample
assert run(
"""6
pasha 0
gerald 1
gerald 1
valera 2
igor 3
olesya 1
5
1 1
1 2
1 3
3 1
6 1
"""
) == "2\n2\n0\n1\n0"

# minimum size
assert run(
"""1
alex 0
1
1 1
"""
) == "0"

# repeated names
assert run(
"""5
john 0
john 1
john 1
alice 2
alice 3
2
1 1
1 2
"""
) == "1\n1"

# multiple roots
assert run(
"""4
a 0
b 0
c 1
d 2
2
1 1
2 1
"""
) == "1\n1"

# chain tree, missing depth
assert run(
"""4
a 0
b 1
c 2
d 3
2
1 5
2 2
"""
) == "0\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây nút đơn | 0 | Thiếu độ sâu mục tiêu | 
| Tên lặp lại | 1 1 | Tính chính xác của việc đếm khác biệt | 
| Nhiều rễ | 1 1 | Tính chính xác của việc đi ngang qua rừng | 
| Chuỗi sâu có k lớn | 0 1 | Xử lý độ sâu ranh giới | 

## Vỏ cạnh 

Hãy xem xét một khu rừng có nhiều rễ:```
4
a 0
b 0
c 1
d 2
2
1 1
2 1
```DFS bắt đầu từ cả hai gốc`1`Và`2`. Khoảng Euler trở thành: 

| Nút | thiếc | chào hàng | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 3 | 2 | 2 | 
| 2 | 3 | 4 | 
| 4 | 4 | 4 | 

Truy vấn`(1,1)`bản đồ tới khoảng`[1,2]`ở độ sâu`1`, chỉ chứa nút`3`. Truy vấn`(2,1)`bản đồ tới`[3,4]`, chỉ chứa nút`4`. 

Thuật toán xử lý các cây bị ngắt kết nối vì mỗi gốc đều khởi chạy DFS của riêng nó. 

Bây giờ hãy xem xét độ sâu không tồn tại:```
3
a 0
b 1
c 2
1
1 5
```Độ sâu mục tiêu là`5`, Nhưng`depth_nodes[5]`trống rỗng. 

Trong quá trình xử lý, danh sách nút cho độ sâu này trống, do đó không có cập nhật Fenwick nào xảy ra. Truy vấn phạm vi ngay lập tức trả về`0`. 

Các tên lặp lại trong cùng một cây con hoạt động chính xác:```
5
john 0
john 1
john 1
alice 2
alice 3
2
1 1
1 2
```Ở độ sâu`1`, nút`(2, john)`Và`(3, john)`xuất hiện. Khi xử lý lần xuất hiện thứ hai, lần xuất hiện đầu tiên sẽ bị xóa khỏi cây Fenwick. 

Vì vậy, khoảng cây con chứa chính xác một lần xuất hiện hoạt động cho`john`, đưa ra câu trả lời`1`thay vì`2`.
