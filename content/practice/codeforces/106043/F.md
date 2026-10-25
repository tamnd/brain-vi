---
title: "CF 106043F - Bài toán đồ thị"
description: "Chúng ta được cho một đồ thị có trọng số vô hướng được kết nối. Bắt đầu từ đỉnh s và kết thúc ở đỉnh t, chúng ta có thể đi theo bất kỳ bước đi nào, không nhất thiết phải là một con đường đơn giản. Các đỉnh và cạnh có thể được xem lại tùy ý nhiều lần."
date: "2026-06-25T12:49:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106043
codeforces_index: "F"
codeforces_contest_name: "Teamscode Summer 2025 Advanced Division"
rating: 0
weight: 106043
solve_time_s: 69
verified: true
draft: false
---

[CF 106043F - Vấn đề về đồ thị](https://codeforces.com/problemset/problem/106043/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có trọng số vô hướng được kết nối. Bắt đầu từ đỉnh`s`và kết thúc ở đỉnh`t`, chúng ta có thể đi theo bất kỳ bước đi nào, không nhất thiết phải là một con đường đơn giản. Các đỉnh và cạnh có thể được xem lại tùy ý nhiều lần. 

Mỗi bước đi có tổng trọng số, thu được bằng cách tính tổng trọng số của tất cả các cạnh đi qua. Chúng tôi chỉ quan tâm đến tổng modulo đó`k`. 

Nhiệm vụ là xác định có bao nhiêu số dư phân biệt modulo`k`có thể xuất hiện trong số tất cả các bước đi có thể từ`s`ĐẾN`t`. 

Biểu đồ có thể chứa tới`10^5`đỉnh và`2·10^5`các cạnh cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng liệt kê các bước đi đều vô vọng, vì ngay cả số lượng đường đi đơn giản cũng có thể là số mũ. Chúng ta cần mô tả đặc điểm cấu trúc của tất cả các tổng đường dẫn có thể đạt được. 

Trường hợp cạnh nguy hiểm nhất là một cái cây. Trong một cây chỉ có một đường đi đơn giản giữa hai đỉnh, nhưng các bước đi vẫn không phải là duy nhất vì chúng ta có thể đi qua một cạnh và ngay lập tức quay lại. 

Ví dụ:```
2 1 4 1 2
1 2 1
```Việc đi bộ trực tiếp có trọng lượng`1`. 

Chúng tôi cũng có thể làm:```
1 -> 2 -> 1 -> 2
```trọng lượng của ai là`3`. 

Modulo dư lượng có thể đạt được`4`là`{1,3}`, vậy câu trả lời là`2`, không`1`. 

Một trường hợp khó phát hiện khác là khi đồ thị chứa các chu trình có tổng trọng số không tương thích với`k`. 

Coi như:```
k = 6
cycle weight = 4
```Việc chèn liên tục chu trình đó sẽ thay đổi phần còn lại bằng cách`4 mod 6`, chỉ tạo ra nhóm con được tạo bởi`gcd(4,6)=2`. Một giải pháp giả định rằng mỗi chu kỳ tạo ra tất cả dư lượng sẽ bị tính quá mức. 

Trường hợp cạnh cuối cùng là`k = 1`. Mỗi số đều bằng nhau`0 mod 1`, vì vậy câu trả lời phải luôn là`1`. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tạo ra các bước đi từ`s`ĐẾN`t`, tính trọng số của chúng theo modulo`k`, và đánh dấu dư lượng có thể tiếp cận. Điều này ngay lập tức gặp rắc rối vì biểu đồ có thể chứa các chu trình. Số lần đi bộ là vô hạn và thậm chí việc hạn chế chúng ta đi bộ ngắn cũng không giúp ích gì vì kích thước biểu đồ đạt tới`10^5`. 

Quan sát quan trọng là tất cả các bước đi khác nhau chỉ bằng cách chèn các bước đi khép kín. Nếu chúng ta biết sự thay đổi trọng lượng nào có thể được tạo ra bằng cách đi bộ kín thì mọi`s -> t`walk chỉ đơn giản là một giá trị cơ bản cố định cộng với một số kết hợp của những thay đổi đó. 

Chọn bất kỳ cây bao trùm nào và để`dist[v]`là khoảng cách của cây từ gốc đến`v`. 

Đối với cạnh không phải cây`(u,v,w)`, so sánh đường đi của cây từ gốc đến`v`với con đường đi từ gốc đến`u`và sau đó sử dụng cạnh này. Sự khác biệt```
dist[u] + w - dist[v]
```đo lường trọng lượng có thể được đưa vào một lần đi bộ bằng cách sử dụng chu trình đó. 

Các cạnh cây đóng góp một loại máy phát điện khác. Ngay cả trong một cái cây thuần túy, chúng ta có thể đi ngang qua một cạnh và ngay lập tức quay trở lại. Điều đó thay đổi chính xác trọng lượng đi bộ`2w`. 

Tất cả những khác biệt có thể có giữa hai`s -> t`walk tạo thành một nhóm con cộng của các số nguyên được tạo bởi:```
2w                           for every edge
dist[u] + w - dist[v]        for every edge
```Cho phép```
G = gcd(all generators above)
```Mỗi trọng lượng đi bộ đều phù hợp với```
base + x·G
```đối với một số nguyên`x`. 

Modulo`k`, nhóm con được tạo bởi`G`chứa chính xác```
k / gcd(k, G)
```dư lượng riêng biệt. 

Giá trị đó chính là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Vô hạn / Hàm mũ | Không giới hạn | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây bao trùm bằng DFS hoặc BFS. 
2. Trong khi duyệt cây, hãy tính`dist[v]`, tổng trọng số của cây từ gốc đến đỉnh`v`. 
3. Khởi tạo`G = 0`. 
4. Với mọi cạnh đồ thị`(u,v,w)`, cập nhật:```
G = gcd(G, 2w)
```Điều này giải thích cho việc đi ngang qua một cạnh và quay trở lại ngay lập tức. 
5. Đối với cùng một cạnh, hãy cập nhật:```
G = gcd(G, abs(dist[u] + w - dist[v]))
```Điều này nắm bắt được sự khác biệt được tạo ra bởi các chu kỳ. 
6. Sau khi tất cả các cạnh được xử lý, hãy tính:```
d = gcd(G, k)
```7. Đầu ra:```
k / d
```### Tại sao nó hoạt động 

Khoảng cách cây cho một hàm tiềm năng. Mỗi lần duyệt cạnh có thể được viết dưới dạng đóng góp co giãn cộng với số hạng sai lệch. 

Khi hoàn thành`s -> t`bước đi được mở rộng theo cách này, các bộ phận thu gọn về một hằng số cố định chỉ phụ thuộc vào`s`Và`t`. Sự tự do duy nhất đến từ sự kết hợp số nguyên của các giá trị sai lệch và`2w`các hoạt động quay lại. 

Tập hợp tất cả sự khác biệt về trọng lượng đi bộ chính xác là nhóm con cộng được tạo ra bởi các giá trị đó. Bất kỳ nhóm con cộng nào của các số nguyên đều được tạo bởi gcd của nó, đó là`G`. 

Modulo`k`, dư lượng có thể tiếp cận được tạo thành nhóm con tuần hoàn được tạo bởi`G mod k`. Một nhóm con tuần hoàn được tạo bởi`G`có kích thước`k / gcd(k,G)`, giving the required count.

 ## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m, k, s, tt = map(int, input().split())

        adj = [[] for _ in range(n)]
        edges = []

        for _ in range(m):
            u, v, w = map(int, input().split())
            u -= 1
            v -= 1

            adj[u].append((v, w))
            adj[v].append((u, w))
            edges.append((u, v, w))

        dist = [0] * n
        vis = [False] * n

        stack = [0]
        vis[0] = True

        while stack:
            u = stack.pop()

            for v, w in adj[u]:
                if not vis[v]:
                    vis[v] = True
                    dist[v] = dist[u] + w
                    stack.append(v)

        g = 0

        for u, v, w in edges:
            g = gcd(g, 2 * w)
            g = gcd(g, abs(dist[u] + w - dist[v]))

        print(k // gcd(k, g))

solve()
```DFS tính toán khoảng cách của cây từ một gốc tùy ý. Lựa chọn gốc không thành vấn đề vì sau này chỉ sử dụng giá trị gcd. 

Bản cập nhật gcd đầu tiên kết hợp khả năng đi qua một cạnh và quay trở lại ngay lập tức. Nếu không có thuật ngữ này, cây sẽ được xử lý không chính xác. 

Bản cập nhật gcd thứ hai ghi lại sự khác biệt về chu kỳ. Các cạnh của cây tự động đóng góp bằng 0 vì đối với một cạnh của cây chúng ta có:```
dist[v] = dist[u] + w
```và biểu thức trở thành số không. 

Tất cả số học phù hợp thoải mái với số nguyên 64 bit. Khoảng cách có thể đạt khoảng:```
10^5 × 10^9 = 10^14
```nằm trong phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4 5 3 1 4
1 4 3
1 2 1
2 4 2
2 3 4
3 4 5
```Một cây DFS có thể cung cấp: 

| Đỉnh | quận | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 5 | 
| 4 | 3 | 

Bây giờ xử lý các cạnh. 

| Cạnh | 2 tuần | dist[u]+w-dist[v] | G hiện tại | 
| --- | --- | --- | --- | 
| 1-4 (3) | 6 | 0 | 6 | 
| 1-2 (1) | 2 | 0 | 2 | 
| 2-4 (2) | 4 | 0 | 2 | 
| 2-3 (4) | 8 | 0 | 2 | 
| 3-4 (5) | 10 | 7 | 1 | 

Cuối cùng: 

| Số lượng | Giá trị | 
| --- | --- | 
| G | 1 | 
| gcd(G,k) | 1 | 
| Trả lời | 3 | 

Điều này khẳng định rằng cả ba dư lượng modulo`3`có thể truy cập được. 

### Ví dụ 2 

đầu vào:```
1
2 1 4 1 2
1 2 1
```Khoảng cách: 

| Đỉnh | quận | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 

Xử lý cạnh: 

| Cạnh | 2 tuần | sự khác biệt | G hiện tại | 
| --- | --- | --- | --- | 
| 1-2 | 2 | 0 | 2 | 

Tính toán cuối cùng: 

| Số lượng | Giá trị | 
| --- | --- | 
| G | 2 | 
| gcd(G,k) | 2 | 
| Trả lời | 2 | 

Dư lượng có thể tiếp cận là`{1,3}`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần duyệt đồ thị và một lần vượt qua tất cả các cạnh | 
| Không gian | O(n + m) | Danh sách kề, khoảng cách, mảng đã truy cập | 

Với`n ≤ 10^5`Và`m ≤ 2·10^5`, một giải pháp tuyến tính dễ dàng phù hợp với giới hạn cuộc thi điển hình. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m, k, s, tt = map(int, input().split())

        adj = [[] for _ in range(n)]
        edges = []

        for _ in range(m):
            u, v, w = map(int, input().split())
            u -= 1
            v -= 1

            adj[u].append((v, w))
            adj[v].append((u, w))
            edges.append((u, v, w))

        dist = [0] * n
        vis = [False] * n

        st = [0]
        vis[0] = True

        while st:
            u = st.pop()

            for v, w in adj[u]:
                if not vis[v]:
                    vis[v] = True
                    dist[v] = dist[u] + w
                    st.append(v)

        g = 0

        for u, v, w in edges:
            g = gcd(g, 2 * w)
            g = gcd(g, abs(dist[u] + w - dist[v]))

        out.append(str(k // gcd(k, g)))

    return "\n".join(out)

# sample
assert run(
"""1
4 5 3 1 4
1 4 3
1 2 1
2 4 2
2 3 4
3 4 5
"""
) == "3"

# single edge
assert run(
"""1
2 1 4 1 2
1 2 1
"""
) == "2"

# k = 1
assert run(
"""1
2 1 1 1 2
1 2 100
"""
) == "1"

# all weights zero
assert run(
"""1
3 2 7 1 3
1 2 0
2 3 0
"""
) == "1"

# cycle producing all residues modulo 5
assert run(
"""1
3 3 5 1 3
1 2 1
2 3 1
1 3 1
"""
) == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn, trọng lượng 1, k=4 | 2 | Quay lại thông qua`2w`| 
| k=1 | 1 | Mô đun nhỏ nhất | 
| Tất cả trọng lượng bằng 0 | 1 | Đồ thị suy biến | 
| Tam giác với gcd 1 | 5 | Bảo hiểm toàn bộ dư lượng | 
| Mẫu chính thức | 3 | Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét cây:```
1
2 1 4 1 2
1 2 1
```Một nghiệm chỉ xét chu trình sẽ kết luận rằng không có tự do vì đồ thị không có chu trình. Điều đó không đúng. đi bộ```
1 -> 2 -> 1 -> 2
```thêm vào`2`đến tổng trọng lượng. Thuật toán của chúng tôi chèn rõ ràng`2w`vào tính toán gcd, tạo ra`G = 2`và câu trả lời đúng`2`. 

Bây giờ hãy xem xét:```
1
2 1 1 1 2
1 2 100
```Mọi số nguyên đều bằng`0 mod 1`. Thuật toán tính toán:```
answer = 1 / gcd(1, G) = 1
```đó là kết quả duy nhất có thể xảy ra. 

Cuối cùng, hãy xem xét một biểu đồ có độ lệch chu kỳ gcd đã`1`. Một lần`G = 1`, chúng ta có:```
gcd(k, G) = 1
```và câu trả lời trở thành`k`. Thuật toán nhận dạng chính xác rằng mọi modulo dư`k`có thể được sản xuất.
