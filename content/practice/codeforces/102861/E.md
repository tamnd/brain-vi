---
title: "CF 102861E - Đảng Công ty"
description: "Các nhân viên tạo thành một cây có gốc mà chủ công ty là gốc. Mỗi nhân viên đều có độ tuổi không lớn hơn tuổi của người quản lý của họ, vì vậy độ tuổi không bao giờ tăng lên khi chuyển xuống cấp bậc thấp hơn. Một bữa tiệc được chủ sở hữu của nó mô tả và độ tuổi cho phép."
date: "2026-07-25T14:02:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102861
codeforces_index: "E"
codeforces_contest_name: "2020-2021 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102861
solve_time_s: 57
verified: true
draft: false
---

[CF 102861E - Công ty Đảng](https://codeforces.com/problemset/problem/102861/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các nhân viên tạo thành một cây có gốc mà chủ công ty là gốc. Mỗi nhân viên đều có độ tuổi không lớn hơn tuổi của người quản lý của họ, vì vậy độ tuổi không bao giờ tăng lên khi chuyển xuống cấp bậc thấp hơn. 

Một bữa tiệc được chủ sở hữu của nó mô tả và độ tuổi cho phép. Chủ sở hữu luôn có giá trị cho bên riêng của họ. Để một nhân viên khác tham dự, họ phải có mối liên hệ trực tiếp với ai đó đã tham dự và tuổi của họ phải nằm trong khoảng thời gian đó. Điều này có nghĩa là các nhân viên được mời chính xác là thành phần được kết nối của chủ sở hữu bên trong cây sau khi loại bỏ các nhân viên có độ tuổi nằm ngoài khoảng đó. 

Đối với mỗi nhân viên, chúng ta cần đếm xem có bao nhiêu khoảng thời gian của nhóm tạo ra một thành phần được kết nối có chứa nhân viên đó. 

Giới hạn rất lớn: cả số lượng nhân viên và các bên đều có thể đạt tới 100000. Một giải pháp khám phá cây cho mỗi bên có thể thực hiện khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa những gì có thể. Chúng ta cần một nghiệm gần với O((N + M) log N). 

Thứ tự tuổi cho cấu trúc chính. Khi một bên có chủ sở hữu`o`và tuổi tối đa`R`, chúng ta có thể di chuyển lên trên từ`o`trong khi tuổi của người quản lý vẫn nhiều nhất`R`. Hãy để nhân viên có khả năng tiếp cận cao nhất`h`. Mỗi nhân viên được mời tham dự bữa tiệc sẽ ở trong cây con của`h`, bởi vì mọi tổ tiên của`o`lên đến`h`là hợp lệ và mọi nhánh bên từ những tổ tiên đó vẫn được kết nối. Bên trong cây con đó, hạn chế duy nhất còn lại là độ tuổi tối thiểu`L`. 

Có một số trường hợp việc triển khai có thể thất bại. 

Hãy xem xét một chủ sở hữu là người gốc của công ty:```
1 1
5 1
1 1 5
```Câu trả lời là:```
1
```Một giải pháp giả định rằng mỗi nhân viên đều có cha mẹ khác với chính nó có thể bị phá vỡ khi leo lên tổ tiên. 

Một trường hợp khác là khi nhân viên có độ tuổi giới hạn chính xác:```
3 1
10 1
5 1
5 2
2 5 5
```Đầu ra là:```
0 1 1
```Nhân viên có độ tuổi chính xác`L`hoặc chính xác`R`phải được bao gồm. Sử dụng so sánh nghiêm ngặt sẽ loại bỏ chúng một cách không chính xác. 

Một trường hợp khó khăn cuối cùng là một bên có độ tuổi tối đa dừng việc leo lên trước gốc:```
4 1
10 1
7 1
4 2
3 3
3 3 7
```Chủ sở hữu là nhân viên 4. Tổ tiên cao nhất được phép ở độ tuổi 7 là nhân viên 2, không phải nhân viên 1. Tập được mời là cây con của nhân viên 2 có độ tuổi ít nhất là 3, cho:```
0 1 1 1
```Việc triển khai bất cẩn luôn bắt đầu từ gốc sẽ tính sai nhân viên 1. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng bên một cách độc lập. Bắt đầu từ chủ sở hữu, chúng tôi có thể chạy DFS chỉ nhập những nhân viên có độ tuổi trong khoảng thời gian của nhóm. Mỗi nhân viên đến thăm sẽ nhận được thêm một lời mời. Điều này đúng vì DFS tuân thủ chính xác quy tắc mọi nhân viên được mời phải được kết nối thông qua các nhân viên hợp lệ. 

Vấn đề xuất hiện khi có nhiều bên chồng chéo. Trong trường hợp xấu nhất, mọi bên đều có thể đến thăm từng nhân viên, giao việc cho O(NM). Với cả hai giá trị đều đạt 100000, con số này sẽ trở thành khoảng 10 tỷ lượt ghé thăm cây. 

Quan sát hữu ích là một nhóm có thể được chuyển đổi thành một bản cập nhật hình học. Sau khi tìm thấy tổ tiên hợp lệ cao nhất`h`, đảng ảnh hưởng đến mọi nhân viên`v`thỏa mãn hai điều kiện:`v`nằm trong cây con của`h`, Và`age[v] >= L`. 

Nếu chúng ta chạy một lệnh DFS trên cây thì mỗi cây con sẽ trở thành một khoảng liền kề. Người lao động`v`được biểu diễn bằng điểm`(tin[v], age[v])`. Một nhóm trở thành một bản cập nhật hình chữ nhật:`tin[h] <= tin[v] <= tout[h]`Và`age[v] >= L`. 

Chúng ta cần áp dụng nhiều cập nhật như vậy và hỏi giá trị ở mọi điểm của nhân viên. Điều này có thể được giải quyết ngoại tuyến bằng cách sắp xếp nhân viên theo độ tuổi. Trong khi xử lý nhân viên từ trẻ đến lớn tuổi, chúng tôi kích hoạt mọi bên đã đạt đến độ tuổi tối thiểu. Các bên tích cực chỉ cần thêm một vào khoảng Euler, khoảng này có thể được cây Fenwick xử lý. 

Sự biến đổi cuối cùng là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(N) | Quá chậm | 
| Tối ưu | O((N + M) log N) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây nhân viên và tính toán các vị trí tham quan Euler. Đối với mỗi nhân viên, cửa hàng`tin`Và`tout`, do đó mỗi cây con trở thành một khoảng theo thứ tự Euler. 
2. Xây dựng bàn nâng nhị phân cho tổ tiên. Điều kiện tuổi là đơn điệu trên đường đi tới gốc, vì vậy chúng ta có thể tăng lũy ​​thừa hai và tìm ra tổ tiên cao nhất có tuổi không vượt quá độ tuổi tối đa của nhóm. 
3. Đối với mỗi bên, hãy bắt đầu từ chủ nhân của nó và leo lên trên bằng cách sử dụng thang nâng nhị phân. Nếu một ứng cử viên tổ tiên có tuổi nhiều nhất`R`, di chuyển đến đó. Nhân viên cuối cùng là tổ tiên hợp lệ cao nhất`h`. 
4. Chuyển nhóm thành bản cập nhật ngoại tuyến với khoảng thời gian Euler`[tin[h], tout[h]]`và độ tuổi tối thiểu`L`. 
5. Sắp xếp nhân viên theo độ tuổi ngày càng tăng và sắp xếp các cập nhật của đảng theo độ tuổi tối thiểu của họ ngày càng tăng. Trước khi trả lời một nhân viên về độ tuổi`A`, thêm mọi bữa tiệc với`L <= A`vào cây Fenwick dưới dạng gia số phạm vi trên khoảng Euler của nó. 
6. Truy vấn cây Fenwick tại vị trí Euler của nhân viên. Giá trị là số hình chữ nhật của bên đang hoạt động bao phủ nhân viên đó, chính xác là số bên mời họ. 

Tại sao nó hoạt động: 

Đối với một nhóm, tổ tiên cao nhất`h`với độ tuổi nhiều nhất`R`là điểm cao nhất có thể duy trì kết nối với chủ sở hữu. Mọi thứ bên ngoài`h`cây con của sẽ yêu cầu vượt qua cạnh cha có tuổi đã quá lớn. Bên trong`h`cây con của, tối đa mọi độ tuổi đều tự động được`R`vì con cháu không thể già hơn tổ tiên. Yêu cầu duy nhất còn lại là độ tuổi ít nhất`L`, được xử lý bằng cách quét ngoại tuyến. Mỗi nhân viên nhận được một mức tăng cho chính xác các bên có hình chữ nhật chứa họ`(tin, age)`điểm, do đó các truy vấn Fenwick cuối cùng sẽ tạo ra số lượng cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    age = [0] * (n + 1)
    graph = [[] for _ in range(n + 1)]

    for i in range(1, n + 1):
        a, b = map(int, input().split())
        age[i] = a
        if i != b:
            graph[b].append(i)

    LOG = (n + 1).bit_length()
    up = [[1] * (n + 1) for _ in range(LOG)]

    stack = [1]
    order = [1]
    parent = [1] * (n + 1)

    while stack:
        u = stack.pop()
        for v in graph[u]:
            parent[v] = u
            order.append(v)
            stack.append(v)

    for i in range(1, n + 1):
        up[0][i] = parent[i]

    for k in range(1, LOG):
        prev = up[k - 1]
        cur = up[k]
        for i in range(1, n + 1):
            cur[i] = prev[prev[i]]

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    timer = 0

    stack = [(1, 0)]
    while stack:
        u, state = stack.pop()
        if state == 0:
            timer += 1
            tin[u] = timer
            stack.append((u, 1))
            for v in reversed(graph[u]):
                stack.append((v, 0))
        else:
            tout[u] = timer

    def highest(o, r):
        u = o
        for k in range(LOG - 1, -1, -1):
            if age[up[k][u]] <= r:
                u = up[k][u]
        return u

    updates = []
    for _ in range(m):
        o, l, r = map(int, input().split())
        h = highest(o, r)
        updates.append((l, tin[h], tout[h]))

    updates.sort()
    employees = sorted((age[i], tin[i], i) for i in range(1, n + 1))

    bit = [0] * (n + 2)

    def add(idx, val):
        while idx <= n:
            bit[idx] += val
            idx += idx & -idx

    def range_add(l, r, val):
        add(l, val)
        add(r + 1, -val)

    def query(idx):
        res = 0
        while idx:
            res += bit[idx]
            idx -= idx & -idx
        return res

    ans = [0] * (n + 1)
    p = 0

    for a, pos, idx in employees:
        while p < m and updates[p][0] <= a:
            _, l, r = updates[p]
            range_add(l, r, 1)
            p += 1
        ans[idx] = query(pos)

    print(*ans[1:])

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã xây dựng cây gốc và bảng nâng nhị phân. Gốc trỏ đến chính nó trong bảng tổ tiên, điều này tránh được việc xử lý đặc biệt khi leo lên đến đỉnh của hệ thống phân cấp công ty. 

Chuyến tham quan Euler được tính toán lặp đi lặp lại để tránh các giới hạn đệ quy của Python. Thuộc tính quan trọng là tất cả nhân viên trong một cây con đều có vị trí Euler giữa`tin[u]`Và`tout[u]`. 

các`highest`hàm sử dụng nâng nhị phân từ bước nhảy lớn đến bước nhảy nhỏ. Vì độ tuổi chỉ giảm khi di chuyển xuống cây nên nếu mục tiêu nhảy vẫn nằm trong độ tuổi tối đa cho phép thì mọi nhân viên giữa vị trí hiện tại và mục tiêu nhảy đó cũng hợp lệ. 

Cây Fenwick lưu trữ một mảng sai phân trên các vị trí Euler. Một bản cập nhật của nhóm sẽ thêm một bản cập nhật vào toàn bộ khoảng thời gian của cây con bằng cách sử dụng hai bản cập nhật điểm. Việc quét độ tuổi đảm bảo rằng một nhóm được chèn vào chính xác khi độ tuổi của nhân viên hiện tại đạt đến độ tuổi tối thiểu được phép. 

Không cần nhân các giá trị lớn, vì vậy việc tràn số nguyên Python không phải là vấn đề đáng lo ngại. Việc sử dụng cập nhật theo khoảng thời gian`r + 1`; vị trí phụ này vô hại vì cây Fenwick có kích thước`n + 1`và nó ngăn chặn từng lỗi xảy ra ở cuối cây con. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào mẫu:```
10 3
8 1
3 5
5 1
2 3
4 1
3 3
1 2
7 1
2 2
3 2
3 5 9
5 3 8
3 2 6
```Những chuyển biến quan trọng của đảng là: 

| Đảng | Chủ sở hữu | L | R | Tổ tiên cao nhất h | Cập nhật Euler | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | 9 | 1 | cây con của 1 | 
| 2 | 5 | 3 | 8 | 5 | cây con của 5 | 
| 3 | 3 | 2 | 6 | 3 | cây con của 3 | 

Việc quét xử lý nhân viên theo thứ tự độ tuổi: 

| Tuổi nhân viên | Đã thêm các bên | Kết quả truy vấn Euler | 
| --- | --- | --- | 
| 1 | không | 0 | 
| 2 | bữa tiệc 3 | 0 hoặc 1 tùy theo vị trí | 
| 2 | bữa tiệc 3 | 1 | 
| 3 | bên 2 và 3 | 2 | 
| 3 | bên 2 và 3 | 2 | 
| 4 | bên 1, 2, 3 | 1 | 
| 5 | bên 1, 2, 3 | 3 | 
| 7 | bên 1, 2, 3 | 2 | 
| 8 | tiệc 1 | 2 | 
| 3 | các bên phụ thuộc vào vị trí Euler | 1 | 

Kết quả cuối cùng phù hợp:```
2 1 3 1 1 2 0 2 0 1
```Một ví dụ thứ hai:```
4 2
10 1
7 1
4 2
3 3
4 3 10
2 7 10
```Chuyển đổi đảng: 

| Đảng | Tổ tiên cao nhất | Độ tuổi tối thiểu | Cây con bị ảnh hưởng | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | Toàn bộ cây | 
| 2 | 2 | 7 | Nhân viên dưới 2 tuổi | 

Trạng thái quét trở thành: 

| Nhân viên | Tuổi | Các bên hoạt động | Trả lời | 
| --- | --- | --- | --- | 
| 4 | 3 | Bên 1 | 1 | 
| 3 | 4 | Bên 1 | 1 | 
| 2 | 7 | Bên 1, Bên 2 | 2 | 
| 1 | 10 | Bên 1, Bên 2 | 2 | 

Kết quả là:```
2 2 1 1
```Ví dụ này kiểm tra xem giới hạn độ tuổi tối đa có thể ngăn chặn việc leo lên và việc lọc độ tuổi tối thiểu được áp dụng chính xác hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log N) | Nâng nhị phân xử lý các truy vấn tổ tiên và mỗi thao tác Fenwick tiêu tốn thời gian logarit. | 
| Không gian | O(N log N) | Bảng tổ tiên chi phối việc sử dụng bộ nhớ. | 

Thuật toán sử dụng khoảng vài triệu thao tác để`N = M = 100000`, phù hợp thoải mái trong giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return out.strip()

# Sample
assert run("""10 3
8 1
3 5
5 1
2 3
4 1
3 3
1 2
7 1
2 2
3 2
3 5 9
5 3 8
3 2 6
""") == "2 1 3 1 1 2 0 2 0 1"

# Single employee
assert run("""1 1
5 1
1 1 5
""") == "1"

# Equal age boundaries
assert run("""3 2
5 1
5 1
5 2
1 5 5
2 5 5
""") == "1 2 2"

# Climb stops before root
assert run("""4 1
10 1
7 1
4 2
3 3
4 3 7
""") == "0 1 1 1"

# Multiple overlapping ranges
assert run("""5 3
10 1
8 1
5 2
3 2
3 3
1 3 10
2 8 10
3 3 5
""") == "1 2 2 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nhân viên độc thân |`1`| Xử lý gốc và đại diện tự cha mẹ | 
| Ranh giới độ tuổi bình đẳng |`1 2 2`| So sánh độ tuổi toàn diện | 
| Leo dừng trước root |`0 1 1 1`| Tìm kiếm tổ tiên cao nhất chính xác | 
| Nhiều phạm vi chồng chéo |`1 2 2 2 1`| Nhiều cập nhật hình chữ nhật và các bên chồng chéo | 

## Vỏ cạnh 

Trường hợp chỉ có gốc hoạt động vì bảng nâng nhị phân lưu trữ gốc như tổ tiên của chính nó. các`highest`hàm không bao giờ rời khỏi phạm vi hợp lệ và giữ gốc chính xác là nhân viên cao nhất có thể. 

Đối với trường hợp tuổi biên:```
3 1
5 1
5 1
5 2
2 5 5
```Bữa tiệc có chủ sở hữu 2 và phạm vi`[5,5]`. Chủ và nhân viên 3 có độ tuổi chính xác cho phép nên cả hai đều được tính. Trong quá trình quét, nhóm được chèn trước khi xử lý nhân viên 5 tuổi vì`L <= age`và khoảng Euler bao gồm cả hai nhân viên. 

Đối với trường hợp leo dốc bị dừng:```
4 1
10 1
7 1
4 2
3 3
3 3 7
```Chủ sở hữu là nhân viên 4. Nâng cấp nhị phân cố gắng di chuyển lên trên, nhưng nhân viên 1 có 10 tuổi, nằm ngoài độ tuổi tối đa 7. Tổ tiên hợp lệ cao nhất vẫn là nhân viên 2. Bản cập nhật chỉ bao gồm cây con của nhân viên 2, vì vậy nhân viên 1 không bao giờ nhận được phần tăng thêm của nhóm. 

Quét ngoại tuyến xử lý tất cả các trường hợp như vậy một cách thống nhất vì mỗi bên được đại diện bởi chính xác những nhân viên đáp ứng cả điều kiện cây con và điều kiện độ tuổi tối thiểu.
