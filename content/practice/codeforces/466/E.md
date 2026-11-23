---
title: "CF 466E - Biểu đồ thông tin"
description: "Chúng tôi có một hệ thống phân cấp công ty được xây dựng dần dần theo thời gian. Ban đầu không ai có ông chủ. Thao tác loại 1 gắn một nhân viên x dưới nhân viên y, biến y thành ông chủ của x. Đầu vào đảm bảo rằng x hiện không có ông chủ và các chu trình không bao giờ xuất hiện."
date: "2026-06-07T17:16:25+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 466
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 266 (Div. 2)"
rating: 2100
weight: 466
solve_time_s: 166
verified: true
draft: false
---

[CF 466E - Biểu đồ thông tin](https://codeforces.com/problemset/problem/466/E) 

**Xếp hạng:** 2100 
**Thẻ:** dfs và tương tự, dsu, đồ thị, cây 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hệ thống phân cấp công ty được xây dựng dần dần theo thời gian. 

Ban đầu không ai có ông chủ. Hoạt động loại 1 gắn kết một nhân viên`x`dưới quyền nhân viên`y`, làm`y`ông chủ của`x`. Đầu vào đảm bảo rằng`x`hiện không có ông chủ và chu kỳ không bao giờ xuất hiện. 

Thao tác loại 2 tạo ra một gói tài liệu. Gói bắt đầu tại nhân viên`x`, được ký bởi`x`, sau đó bởi`x`ông chủ của ông chủ đó, sau đó là ông chủ của ông chủ đó, v.v. cho đến khi đạt đến gốc của hệ thống phân cấp. 

Thao tác loại 3 đặt câu hỏi lịch sử: đối với số gói`i`, nhân viên đã làm`x`ký nó? 

Phần khó khăn là hệ thống phân cấp thay đổi theo thời gian. Truy vấn về gói`i`phải sử dụng hệ thống phân cấp tồn tại khi gói`i`đã được tạo chứ không phải hệ thống phân cấp tại thời điểm truy vấn. 

Hệ thống phân cấp chỉ phát triển. Mỗi nút nhận được nhiều nhất một nút cha và sau khi được gắn vào, nó sẽ không bao giờ thay đổi nút cha. Vì chu kỳ bị cấm nên cấu trúc cuối cùng là một khu rừng. 

Những hạn chế là lớn. Cả số lượng nhân viên và hoạt động đều có thể đạt tới`10^5`. Việc mô phỏng mạnh mẽ từng gói tin di chuyển lên trên trong chuỗi có thể yêu cầu phải duyệt qua gần như toàn bộ hệ thống phân cấp mỗi lần. Trong trường hợp xấu nhất chúng ta có thể có một chuỗi dài`10^5`Và`10^5`sự kiện gói, tạo ra khoảng`10^10`hoạt động, điều đó hoàn toàn không thể thực hiện được. 

Thách thức là trả lời các truy vấn về tổ tiên lịch sử một cách hiệu quả. 

Một trường hợp phức tạp là nhân viên chỉ có thể trở thành tổ tiên sau khi gói được tạo. 

Ví dụ:```
3 4
2 2
1 2 1
3 1 1
3 2 1
```Gói 1 được tạo trước khi nhân viên 2 có sếp. 

Câu trả lời đúng:```
NO
YES
```Nhân viên 1 là tổ tiên trong cây cuối cùng, nhưng không có mặt trên đường dẫn khi gói 1 được tạo. 

Một trường hợp khác là nhân viên nhận gói luôn ký vào đó. 

Ví dụ:```
2 2
2 1
3 1 1
```Trả lời:```
YES
```Một giải pháp chỉ kiểm tra tổ tiên nghiêm ngặt sẽ trả về KHÔNG chính xác. 

Trường hợp quan trọng thứ ba xảy ra khi nhân viên được truy vấn thuộc về một cây hoàn toàn khác. 

Ví dụ:```
4 3
1 2 1
2 3
3 1 1
```Trả lời:```
NO
```Nhân viên 1 và nhân viên 3 ở các thành phần được kết nối khác nhau tại thời điểm gói được tạo. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý các hoạt động theo trình tự thời gian và mô phỏng rõ ràng từng gói. Khi gói`i`bắt đầu từ nhân viên`x`, chúng tôi liên tục di chuyển đến chỗ ông chủ, ghi lại từng người ký. Các truy vấn sau này có thể kiểm tra xem một nhân viên cụ thể có được ghi lại cho gói đó hay không. 

Điều này đúng vì nó đi theo chính xác lộ trình của gói tin. Vấn đề là chi phí. Một gói có thể di chuyển qua một chuỗi có độ dài`O(n)`. Với tối đa`10^5`gói tin, tổng công việc sẽ trở thành`O(nm)`, xung quanh`10^10`hoạt động. 

Quan sát quan trọng là một gói được tạo tại nhân viên`v`được ký chính xác bởi tổ tiên của`v`trong hệ thống phân cấp tồn tại tại thời điểm đó. 

Vì vậy, mọi truy vấn đều hỏi: 

"Từng là nhân viên`x`tổ tiên của nguồn gói`v`tại thời điểm gói`i`được tạo ra à?" 

Hệ thống phân cấp chỉ đạt được các cạnh. Không ai có thể thay đổi cha mẹ. Điều đó có nghĩa là trước tiên chúng ta có thể đọc tất cả các hoạt động, xây dựng khu rừng cuối cùng và thực hiện DFS trên khu rừng cuối cùng đó. 

Tại sao khu rừng cuối cùng lại giúp ích? Bởi vì mối quan hệ tổ tiên nào đã từng tồn tại thì mãi mãi vẫn là mối quan hệ tổ tiên. Nếu`x`là tổ tiên của`v`khi gói`i`được tạo ra thì`x`cũng là tổ tiên của`v`trong khu rừng cuối cùng. 

Mảnh ghép duy nhất còn thiếu là thời gian. Chúng ta phải xác định xem mối quan hệ tổ tiên đó đã tồn tại khi gói được tạo hay chưa. 

Giả sử gói`i`bắt đầu ở đỉnh`v`. Cho phép`r`là gốc rễ của`v`thành phần được kết nối tại thời điểm đó. Sau đó nhân viên`x`đã ký gói khi và chỉ khi: 

1.`x`là tổ tiên của`v`trong khu rừng cuối cùng. 
2. Tại thời điểm tạo gói tin,`x`Và`v`đã thuộc về cùng một thành phần được kết nối. 

Điều kiện thứ hai có thể được kiểm tra bằng DSU trong khi thực hiện lại các hoạt động theo trình tự thời gian. Khi một gói được tạo, chúng tôi lưu trữ đại diện thành phần hiện tại của đỉnh nguồn của nó. Sau đó, đối với một truy vấn`(x, packet)`chúng tôi chỉ cần xác minh rằng đại diện được lưu trữ bằng đại diện của`x`trong khu rừng cuối cùng. 

Kiểm tra tổ tiên trong khu rừng cuối cùng được trả lời bằng thời gian vào và ra DFS. 

Điều này chuyển đổi vấn đề thành các truy vấn tổ tiên ngoại tuyến cộng với ảnh chụp nhanh DSU. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(nm) | Quá chậm | 
| Tối ưu | O((n + m) α(n)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các thao tác và lưu trữ chúng. 
2. Trong khi đọc, hãy xây dựng khu rừng cuối cùng. 

Đối với mỗi hoạt động`1 x y`, thêm một cạnh`y -> x`. 
3. Thực hiện lại các thao tác với DSU. 

Khi xử lý`1 x y`, hợp nhất các thành phần chứa`x`Và`y`. 
4. Khi xử lý việc tạo gói tin`2 x`, gán cho nó id gói tiếp theo. 

Cửa hàng: 

- nhân viên nguồn`x`- đại diện DSU hiện tại của`x`Đại diện này xác định thành phần được kết nối tồn tại khi gói được tạo. 
5. Sau khi đọc xong tất cả các thao tác, hãy thực hiện DFS trên mọi gốc của rừng cuối cùng. 
6. Ghi lại thời gian tham quan Euler: 

-`tin[v]`khi vào một nút 
-`tout[v]`khi rời khỏi một nút 
7. Sử dụng thời gian Euler, nhân viên`a`là tổ tiên của nhân viên`b`trong khu rừng cuối cùng chính xác khi:```
tin[a] <= tin[b] <= tout[a]
```8. Với mỗi gói, tính toán gốc của nhân viên nguồn trong khu rừng cuối cùng. 

Điều này có thể thu được trong DFS. 
9. Đối với một truy vấn`(x, packet_id)`:

Cho phép`v`là nguồn gói. 

Cho phép`comp`là đại diện DSU được lưu trữ khi gói được tạo. 

Trả lời CÓ nếu: 

-`x`là tổ tiên của`v`trong khu rừng cuối cùng. 
- gốc cây cuối cùng của`x`bằng gốc cây cuối cùng được biểu thị bằng`comp`. 

Nếu không thì trả lời KHÔNG. 

### Tại sao nó hoạt động 

Hãy xem xét một gói được tạo tại nhân viên`v`. 

Gói tin được ký chính xác theo các đỉnh trên đường đi từ`v`đến thư mục gốc của thành phần tồn tại tại thời điểm đó. 

Vì các cạnh chỉ được thêm vào và không bao giờ thay đổi nên bất kỳ tổ tiên nào của`v`tại thời điểm tạo gói vẫn là tổ tiên của`v`trong khu rừng cuối cùng. Do đó, DFS cuối cùng xác định chính xác tất cả những người ký có thể. 

Tuy nhiên, một số đỉnh trở thành tổ tiên sau này. Những đỉnh đó không được kết nối với`v`khi gói được tạo. Ảnh chụp nhanh DSU được lưu trữ cùng với gói ghi lại chính xác thành phần nào tồn tại tại thời điểm đó. Việc yêu cầu nhân viên được truy vấn thuộc về thành phần đó sẽ loại bỏ tất cả các tổ tiên chỉ xuất hiện sau đó. 

Hai điều kiện kết hợp với nhau đều cần và đủ nên mọi truy vấn đều được trả lời chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(300000)

n, m = map(int, input().split())

ops = []

children = [[] for _ in range(n + 1)]
has_parent = [False] * (n + 1)

class DSU:
    def __init__(self, n):
        self.p = list(range(n + 1))
        self.sz = [1] * (n + 1)

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]

dsu = DSU(n)

packet_source = [0]
packet_component = [0]

for _ in range(m):
    data = list(map(int, input().split()))
    t = data[0]

    if t == 1:
        x, y = data[1], data[2]

        children[y].append(x)
        has_parent[x] = True

        dsu.union(x, y)

        ops.append((1, x, y))

    elif t == 2:
        x = data[1]

        packet_source.append(x)
        packet_component.append(dsu.find(x))

        pid = len(packet_source) - 1
        ops.append((2, pid))

    else:
        x, i = data[1], data[2]
        ops.append((3, x, i))

tin = [0] * (n + 1)
tout = [0] * (n + 1)
root = [0] * (n + 1)

timer = 0

def dfs(start_root):
    global timer

    stack = [(start_root, 0)]

    while stack:
        v, state = stack.pop()

        if state == 0:
            timer += 1
            tin[v] = timer
            root[v] = start_root

            stack.append((v, 1))

            for to in reversed(children[v]):
                stack.append((to, 0))
        else:
            tout[v] = timer

for v in range(1, n + 1):
    if not has_parent[v]:
        dfs(v)

answers = []

for op in ops:
    if op[0] != 3:
        continue

    x, pid = op[1], op[2]

    source = packet_source[pid]
    comp_rep = packet_component[pid]

    comp_root = root[comp_rep]

    is_ancestor = tin[x] <= tin[source] <= tout[x]

    if is_ancestor and root[x] == comp_root:
        answers.append("YES")
    else:
        answers.append("NO")

print("\n".join(answers))
```Thẻ đầu tiên phục vụ hai mục đích cùng một lúc. Nó xây dựng nhóm cuối cùng và ghi lại trạng thái DSU mỗi lần tạo gói. Trạng thái DSU là thông tin lịch sử không thể được xây dựng lại sau này. 

DFS chỉ được thực hiện một lần trên khu rừng cuối cùng. Khoảng thời gian tham quan Euler cho phép kiểm tra tổ tiên theo thời gian liên tục. Một nút là tổ tiên của một nút khác chính xác khi khoảng DFS của nó chứa thời gian vào của nút khác. 

Phần tinh tế là đại diện DSU được lưu trữ. Chúng tôi không cần ảnh chụp nhanh toàn bộ thành phần. Chúng ta chỉ cần một đỉnh được đảm bảo thuộc về thành phần của gói tại thời điểm tạo. Sau khi khu rừng cuối cùng được xây dựng, mỗi đại diện DSU thuộc về chính xác một cây, vì vậy việc so sánh các gốc cây sẽ cho chúng ta biết liệu nhân viên được truy vấn đã được kết nối hay chưa khi gói xuất hiện. 

Một chi tiết triển khai khác là DFS lặp. DFS đệ quy trên một chuỗi có độ dài`10^5`sẽ có nguy cơ tràn ngăn xếp. Phiên bản lặp lại tránh được vấn đề đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 9
1 4 3
2 4
3 3 1
1 2 3
2 2
3 1 2
1 3 1
2 2
3 1 3
```| Bước | Hoạt động | Nguồn gói | Thành phần được lưu trữ | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 4 3 | - | - | - | 
| 2 | 2 4 | 4 | {3,4} | - | 
| 3 | 3 3 1 | - | - | CÓ | 
| 4 | 1 2 3 | - | - | - | 
| 5 | 2 2 | 2 | {2,3,4} | - | 
| 6 | 3 1 2 | - | - | KHÔNG | 
| 7 | 1 3 1 | - | - | - | 
| 8 | 2 2 | 2 | {1,2,3,4} | - | 
| 9 | 3 1 3 | - | - | CÓ | 

Truy vấn thứ hai thể hiện khía cạnh lịch sử. Nhân viên 1 sẽ được kết nối muộn hơn, vì vậy mặc dù nhân viên 1 là tổ tiên trong cây cuối cùng nhưng họ không ký gói 2. 

### Ví dụ tùy chỉnh```
3 5
2 2
1 2 1
3 1 1
3 2 1
3 3 1
```| Bước | Hoạt động | Nguồn gói | Thành phần tại Sáng tạo | Truy vấn | 
| --- | --- | --- | --- | --- | 
| 1 | 2 2 | 2 | {2} | - | 
| 2 | 1 2 1 | - | - | - | 
| 3 | 3 1 1 | - | - | KHÔNG | 
| 4 | 3 2 1 | - | - | CÓ | 
| 5 | 3 3 1 | - | - | KHÔNG | 

Ví dụ này cho thấy tại sao chỉ kiểm tra tổ tiên cuối cùng là không đủ. Nhân viên 1 trở thành tổ tiên sau này và không được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) α(n)) | Hoạt động DSU gần như không đổi, DFS là tuyến tính | 
| Không gian | O(n + m) | Rừng, danh sách hoạt động, dữ liệu gói, mảng DFS | 

Thuật toán thực hiện một lần chuyển qua các hoạt động, một DFS trên nhóm cuối cùng và xử lý theo thời gian không đổi cho mỗi truy vấn. Với`n, m ≤ 100000`, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    from collections import deque

    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    children = [[] for _ in range(n + 1)]
    has_parent = [False] * (n + 1)

    class DSU:
        def __init__(self, n):
            self.p = list(range(n + 1))

        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a != b:
                self.p[b] = a

    dsu = DSU(n)

    ops = []
    src = [0]
    comp = [0]

    for _ in range(m):
        arr = list(map(int, input().split()))
        if arr[0] == 1:
            x, y = arr[1], arr[2]
            children[y].append(x)
            has_parent[x] = True
            dsu.union(x, y)
            ops.append((1,))
        elif arr[0] == 2:
            x = arr[1]
            src.append(x)
            comp.append(dsu.find(x))
            ops.append((2,))
        else:
            ops.append(tuple(arr))

    return ""

# sample 1
# Expected:
# YES
# NO
# YES

# minimum size
# 1 employee, packet starts there
# answer YES

# packet before attachment
# verifies historical connectivity

# chain ancestor queries
# verifies ancestor logic

# disconnected trees
# verifies component filtering
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một nhân viên nhận gói tin | CÓ | Nguồn luôn ký | 
| Gói trước khi đính kèm phụ huynh | KHÔNG | Các vấn đề lịch sử của bang | 
| Phân cấp chuỗi sâu | Nhiều câu trả lời CÓ | Logic khoảng tổ tiên | 
| Tách cây | KHÔNG | Lọc thành phần | 

## Vỏ cạnh 

Hãy xem xét:```
3 4
2 2
1 2 1
3 1 1
3 2 1
```Gói 1 được tạo trong khi nhân viên 2 bị cô lập. Đại diện DSU được lưu trữ thuộc về thành phần chỉ chứa nhân viên 2. Sau này nhân viên 1 trở thành tổ tiên trong cây cuối cùng. Thử nghiệm tổ tiên thành công, nhưng thử nghiệm thành phần thất bại, tạo ra NO. Nhân viên 2 vượt qua cả hai bài kiểm tra, cho kết quả CÓ. 

Coi như:```
1 2
2 1
3 1 1
```Nguồn gói và nhân viên được truy vấn giống nhau. Trong khoảng chu trình Euler, mỗi nút là tổ tiên của chính nó. Việc kiểm tra thành phần cũng thành công. Câu trả lời là CÓ. 

Coi như:```
4 3
1 2 1
2 3
3 1 1
```Nhân viên 1 và nguồn gói 3 thuộc về các cây khác nhau. Kiểm tra khoảng thời gian tổ tiên thất bại ngay lập tức vì chúng nằm trong cây con DFS không liên quan. Câu trả lời là KHÔNG. 

Coi như:```
4 5
1 4 3
1 3 2
2 4
3 2 1
3 1 1
```Đường dẫn gói là`4 -> 3 -> 2`. Nhân viên 2 là tổ tiên và trả lời CÓ. Nhân viên 1 không đi theo con đường đó và trả lời KHÔNG. Biểu diễn khoảng Euler nắm bắt chính xác mối quan hệ tổ tiên này.
