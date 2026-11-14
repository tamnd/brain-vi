---
title: "CF 1776F - Tách tàu"
description: "Chúng ta được cho một đồ thị vô hướng liên thông. Mỗi cạnh phải được gán cho một công ty. Giả sử công ty c sở hữu tất cả các cạnh được tô màu c. Việc tô màu phải thỏa mãn hai điều kiện. Điều kiện đầu tiên nói rằng không một công ty nào được phép sở hữu một mạng lưới kết nối rộng khắp."
date: "2026-06-09T11:46:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1776
codeforces_index: "F"
codeforces_contest_name: "SWERC 2022-2023 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1700
weight: 1776
solve_time_s: 120
verified: false
draft: false
---

[CF 1776F - Tách tàu](https://codeforces.com/problemset/problem/1776/F) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, đồ thị, tham lam 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông. Mỗi cạnh phải được gán cho một công ty. 

Giả sử công ty`c`sở hữu tất cả các cạnh màu`c`. Việc tô màu phải thỏa mãn hai điều kiện. 

Điều kiện đầu tiên nói rằng không một công ty nào được phép sở hữu một mạng lưới kết nối rộng khắp. Đối với mỗi màu, đồ thị chỉ được tạo bởi các cạnh của màu đó phải bị ngắt kết nối. 

Điều kiện thứ hai mạnh hơn nhiều. Lấy hai công ty bất kỳ. Nếu chúng ta giữ tất cả các cạnh thuộc về một trong hai công ty đó thì đồ thị thu được phải liên thông. 

Chúng tôi được tự do lựa chọn số lượng công ty`k`, miễn là`k ≥ 2`. 

Đồ thị có nhiều nhất là 50 đỉnh và tổng của tất cả`n`trên các trường hợp thử nghiệm tối đa là 5000. Những giới hạn này rất nhỏ. Thách thức không phải là độ phức tạp của thuật toán, mà là tìm ra một cấu trúc luôn hoạt động. 

Sai lầm nguy hiểm nhất là chỉ tập trung vào việc làm cho mỗi màu bị ngắt kết nối. Phần đó rất dễ dàng. Phần khó khăn là đảm bảo rằng mỗi cặp màu tạo thành một biểu đồ được kết nối. 

Hãy xem xét một con đường`1-2-3`. 

Nếu chúng ta tô màu cạnh`(1,2)`với màu 1 và cạnh`(2,3)`với màu 2 thì mỗi màu riêng lẻ sẽ bị ngắt kết nối. Tuy nhiên, nếu chúng tôi giới thiệu màu thứ ba mà không có gì hữu ích, một số cặp màu có thể không kết nối được với biểu đồ. Yêu cầu kết nối theo cặp là toàn cầu và phải phù hợp với mọi cặp màu. 

Một cái bẫy dễ dàng khác xuất hiện trên biểu đồ hoàn chỉnh. 

Giả định`n = 3`và tồn tại cả ba cạnh. Nếu chúng ta tô màu tất cả các cạnh liên quan đến đỉnh 1 bằng màu 1 và cạnh còn lại bằng màu 2 thì chỉ có màu 1 được kết nối, vi phạm điều kiện đầu tiên. 

Biểu đồ hoàn chỉnh là trường hợp duy nhất mà công trình chính cần được điều chỉnh đặc biệt. 

## Phương pháp tiếp cận 

Tư duy vũ phu sẽ cố gắng chỉ định màu sắc và sau đó xác minh các điều kiện. 

Đối với biểu đồ có tối đa 1225 cạnh, ngay cả việc quyết định giữa hai màu cũng đã tạo ra`2^m`khả năng. Kiểm tra kết nối sau mỗi bài tập thật dễ dàng, nhưng không gian tìm kiếm rất lớn. 

Quan sát quan trọng là chúng ta không cần nhiều màu sắc. 

Hãy tưởng tượng chọn một đỉnh`u`. Cung cấp một màu cho mỗi cạnh liên quan đến`u`, và tô màu thứ hai cho mọi cạnh còn lại. 

Nếu chúng ta chỉ sử dụng hai màu thì yêu cầu thứ hai sẽ trở nên tầm thường. Chỉ có một cặp màu duy nhất là`{1,2}`và cùng nhau chúng chứa mọi cạnh của đồ thị ban đầu. Vì đồ thị ban đầu được kết nối nên cặp này được kết nối. 

Bây giờ chúng ta chỉ cần đảm bảo rằng mỗi màu riêng lẻ đã được ngắt kết nối. 

Màu 2 không chạm vào cạnh`u`, vậy đỉnh`u`bị cô lập. Màu 2 sẽ tự động bị ngắt kết nối. 

Màu 1 là ngôi sao có tâm ở`u`. Màu này bị ngắt kết nối bất cứ khi nào tồn tại một số đỉnh không liền kề với`u`, vì đỉnh đó có bậc 0 có màu 1. 

Điều này ngay lập tức giải quyết mọi đồ thị không đầy đủ. Chọn đỉnh có bậc nhỏ hơn`n-1`. 

Trường hợp duy nhất còn lại là một đồ thị hoàn chỉnh. 

Trong một biểu đồ hoàn chỉnh, mọi đỉnh đều liền kề với mọi đỉnh khác, do đó đối số trước đó không thành công. Chúng ta cần thêm một màu nữa. 

Chọn một đỉnh`u`. 

Cho một cạnh tới của`u`màu 1. 

Cho tất cả các cạnh khác liên quan tới`u`màu 2. 

Cho mọi màu cạnh còn lại 3. 

Bây giờ mọi màu đều bị ngắt kết nối: 

Màu 1 chỉ chứa một cạnh. 

Màu 2 là ngôi sao thiếu một chiếc lá. 

Màu 3 không chứa cạnh nào tới`u`. 

Bất kỳ cặp màu nào được kết nối: 

Màu 1 và 2 chứa tất cả các cạnh liên quan đến`u`, tạo thành một ngôi sao bao trùm. 

Màu 1 và 3 kết nối`u`qua cạnh màu 1 duy nhất, trong khi màu 3 chứa đồ thị hoàn chỉnh trên các đỉnh còn lại. 

Màu 2 và 3 đối xứng. 

Điều đó hoàn toàn giải quyết được vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm tô màu Brute Force | O(2^m) hoặc tệ hơn | Hàm mũ | Quá chậm | 
| Màu mang tính xây dựng | O(n + m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đồ thị và tính bậc của mỗi đỉnh. 
2. Kiểm tra xem một số đỉnh`u`có bậc hoàn toàn nhỏ hơn`n-1`. 
3. Nếu có một đỉnh như vậy, hãy sử dụng`k = 2`. 

Gán màu 1 cho mỗi cạnh liên quan tới`u`. 

Gán màu 2 cho mọi cạnh khác. 

Màu 2 bị ngắt kết nối vì`u`không có cạnh màu 2. 

Màu 1 bị ngắt kết nối vì có ít nhất một đỉnh không liền kề với`u`, do đó đỉnh đó không có cạnh màu 1. 
4. Nếu không thì biểu đồ đã hoàn tất. 
5. Sử dụng`k = 3`. 
6. Chọn bất kỳ đỉnh nào, ví dụ đỉnh 1. 
7. Trong số các cạnh liên quan đến đỉnh 1, chọn một cạnh và tô màu nó 1. 
8. Tô màu mọi cạnh còn lại vào đỉnh 1 bằng màu 2. 
9. Tô màu tất cả các cạnh còn lại bằng màu 3. 
10. Xuất màu theo thứ tự đầu vào ban đầu. 

### Tại sao nó hoạt động 

Đối với đồ thị không đầy đủ, đỉnh được chọn`u`có ít nhất một người không phải là hàng xóm. Đồ thị màu-1 chỉ bao gồm các cạnh chạm nhau`u`, do đó người không phải là hàng xóm bị cô lập. Đồ thị màu-2 không chứa cạnh chạm`u`, Vì thế`u`bản thân nó bị cô lập. Cả hai màu đều bị ngắt kết nối. 

Vì chỉ có hai màu nên sự kết hợp của hai màu bất kỳ chỉ đơn giản là biểu đồ gốc được kết nối. 

Đối với một đồ thị đầy đủ, màu 1 chứa một cạnh duy nhất và bị ngắt kết nối. Màu 2 là một phần ngôi sao và bị ngắt kết nối vì một điểm cuối của cạnh màu 1 không nhận được cạnh màu 2. Màu 3 không chứa cạnh nào tới đỉnh 1, do đó đỉnh 1 bị cô lập. 

Để kết nối các cặp màu:`1 ∪ 2`chứa mọi cạnh tới đỉnh 1, tạo thành một ngôi sao bao trùm.`1 ∪ 3`chứa đồ thị hoàn chỉnh trên các đỉnh`{2,3,...,n}`và cũng là một cạnh nối đỉnh 1 với đồ thị hoàn chỉnh đó.`2 ∪ 3`là tương tự. 

Do đó mọi điều kiện cần thiết đều được giữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())

        edges = []
        deg = [0] * (n + 1)

        for _ in range(m):
            u, v = map(int, input().split())
            edges.append((u, v))
            deg[u] += 1
            deg[v] += 1

        u = -1
        for v in range(1, n + 1):
            if deg[v] < n - 1:
                u = v
                break

        if u != -1:
            print(2)

            ans = [2] * m
            for i, (a, b) in enumerate(edges):
                if a == u or b == u:
                    ans[i] = 1

            print(*ans)

        else:
            print(3)

            ans = [3] * m
            first_edge = -1

            for i, (a, b) in enumerate(edges):
                if a == 1 or b == 1:
                    first_edge = i
                    break

            ans[first_edge] = 1

            for i, (a, b) in enumerate(edges):
                if i == first_edge:
                    continue
                if a == 1 or b == 1:
                    ans[i] = 2

            print(*ans)

solve()
```Phần đầu tiên tính toán độ. Đỉnh có bậc nhỏ hơn`n-1`ngay lập tức xác nhận rằng biểu đồ không đầy đủ. 

Trong trường hợp không hoàn chỉnh, mọi cạnh chạm vào đỉnh đã chọn sẽ nhận được màu 1. Tất cả các cạnh khác vẫn giữ nguyên màu 2. Khởi tạo toàn bộ mảng câu trả lời bằng 2 và sau đó ghi đè các cạnh liên quan là cách thực hiện đơn giản nhất. 

Trong trường hợp đồ thị đầy đủ, chúng tôi sử dụng ba màu. Chúng ta xác định vị trí một cạnh liên quan đến đỉnh 1 và dành nó cho màu 1. Mọi cạnh khác chạm vào đỉnh 1 sẽ trở thành màu 2. Các cạnh còn lại giữ màu 3. 

Một điểm tinh tế là chúng ta phải giữ màu sắc theo thứ tự đầu vào ban đầu. Đầu ra đề cập đến`i`-cạnh đầu vào thứ, vì vậy chúng tôi lưu trữ tất cả các cạnh trong danh sách và gán màu theo chỉ số cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị đầu vào:```
5 9
1-2
1-3
1-4
1-5
2-3
2-4
2-5
3-4
3-5
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 4 | 
| 2 | 4 | 
| 3 | 4 | 
| 4 | 2 | 
| 5 | 2 | 

Vì đỉnh 4 có bậc`< n-1 = 4`, chúng tôi sử dụng cấu trúc hai màu. 

| Cạnh | Sự cố đến 4? | Màu sắc | 
| --- | --- | --- | 
| 1-2 | Không | 2 | 
| 1-3 | Không | 2 | 
| 1-4 | Có | 1 | 
| 1-5 | Không | 2 | 
| 2-3 | Không | 2 | 
| 2-4 | Có | 1 | 
| 2-5 | Không | 2 | 
| 3-4 | Có | 1 | 
| 3-5 | Không | 2 | 

Màu 1 bị ngắt kết nối vì đỉnh 5 không liền kề với 4. 

Màu 2 bị ngắt kết nối vì đỉnh 4 không có cạnh màu 2. 

### Ví dụ 2 

đầu vào:```
3 3
1-2
1-3
2-3
```Đây là một biểu đồ hoàn chỉnh. 

| Cạnh | Màu sắc | 
| --- | --- | 
| 1-2 | 1 | 
| 1-3 | 2 | 
| 2-3 | 3 | 

Kiểm tra màu sắc: 

| Màu sắc | Cạnh | 
| --- | --- | 
| 1 | {1-2} | 
| 2 | {1-3} | 
| 3 | {2-3} | 

Mỗi màu một mình bị ngắt kết nối. 

Bất kỳ cặp màu nào tạo thành một biểu đồ được kết nối trên cả ba đỉnh. 

Ví dụ này giải thích tại sao biểu đồ hoàn chỉnh cần có ba màu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một tấm bằng và một tấm tô màu | 
| Không gian | O(m) | Lưu trữ các cạnh và gán màu | 

Với`n ≤ 50`và tổng cộng`n ≤ 5000`, sự phức tạp này thấp hơn nhiều so với giới hạn. Ngay cả đồ thị dày đặc nhất cũng chỉ có 1225 cạnh, do đó giải pháp chạy thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
# These tests validate the construction properties rather than
# a specific coloring, since many valid outputs exist.

from collections import deque

def check(n, edges, k, colors):
    assert 2 <= k

    def connected(allowed):
        g = [[] for _ in range(n)]
        for (u, v), c in zip(edges, colors):
            if c in allowed:
                u -= 1
                v -= 1
                g[u].append(v)
                g[v].append(u)

        q = deque([0])
        vis = [False] * n
        vis[0] = True

        while q:
            x = q.popleft()
            for y in g[x]:
                if not vis[y]:
                    vis[y] = True
                    q.append(y)

        return all(vis)

    for c in range(1, k + 1):
        assert not connected({c})

    for a in range(1, k + 1):
        for b in range(a + 1, k + 1):
            assert connected({a, b})

# sample 1 graph
n = 5
edges = [
    (1,2),(1,3),(1,4),(1,5),
    (2,3),(2,4),(2,5),(3,4),(3,5)
]

# one valid coloring
check(n, edges, 2, [2,2,1,2,2,1,2,1,2])

# sample 2 graph
n = 3
edges = [(1,2),(3,1),(2,3)]
check(n, edges, 3, [1,2,3])

# minimum connected graph
n = 3
edges = [(1,2),(2,3)]
check(n, edges, 2, [1,2])

# complete graph K4
n = 4
edges = [(1,2),(1,3),(1,4),(2,3),(2,4),(3,4)]
check(n, edges, 3, [1,2,2,3,3,3])

# non-complete graph with one missing edge
n = 4
edges = [(1,2),(1,3),(2,3),(2,4),(3,4)]
check(n, edges, 2, [2,2,2,1,1])

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tam giác | Bất kỳ cấu trúc 3 màu hợp lệ nào | Xử lý đồ thị hoàn chỉnh | 
| Đường đi có độ dài 2 | Bất kỳ cấu trúc 2 màu hợp lệ nào | Đồ thị liên thông nhỏ nhất | 
| K4 | Bất kỳ cấu trúc 3 màu hợp lệ nào | Trường hợp đồ thị hoàn chỉnh chung | 
| Một cạnh bị thiếu trong biểu đồ hoàn chỉnh | Bất kỳ cấu trúc 2 màu hợp lệ nào | Bằng cấp`< n-1`chi nhánh | 

## Vỏ cạnh 

Hãy xem xét đồ thị hoàn chỉnh trên ba đỉnh:```
1
3 3
1 2
1 3
2 3
```Mỗi đỉnh đều có bậc`n-1 = 2`, vậy là đồ thị đã hoàn thành. Thuật toán đi vào nhánh ba màu. 

Một cạnh liên quan đến đỉnh 1 có màu 1, cạnh còn lại có màu 2 và cạnh còn lại có màu 3. 

Nếu chúng ta sử dụng sai cấu trúc hai màu ở đây, màu 1 sẽ tạo thành một ngôi sao bao trùm được kết nối và vi phạm các yêu cầu. Nhánh biểu đồ hoàn chỉnh đặc biệt tránh được vấn đề đó. 

Bây giờ hãy xem xét:```
1
4 5
1 2
1 3
2 3
2 4
3 4
```Đỉnh 1 có bậc 2 nhỏ hơn`n-1 = 3`. 

Tất cả các cạnh liên quan đến đỉnh 1 đều có màu 1. Các cạnh còn lại có màu 2. 

Đỉnh 4 không liền kề với đỉnh 1 nên bị cô lập về màu 1. Đỉnh 1 bị cô lập về màu 2. Cả hai màu đều bị ngắt kết nối, trong khi phần kết của chúng là đồ thị liên thông ban đầu. 

Đây chính xác là tài sản mà công trình khai thác.
