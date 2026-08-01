---
title: "CF 102599B - \u041b\u0438\u043f\u0435\u0446\u043a\u043e\u0435 \u043c\u0435\u0442\u0440\u043e"
description: "Chúng tôi được cung cấp một bản đồ tàu điện ngầm với N ga. Mỗi trạm có thể chỉ định tối đa một trạm khác mà nó được kết nối. Nếu p[i] khác -1 thì có một đường hầm vô hướng giữa trạm i và trạm p[i]."
date: "2026-07-31T16:38:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102599
codeforces_index: "B"
codeforces_contest_name: "The fifth Lipetsk collegiate programming contest. Finals. 8-11 form"
rating: 0
weight: 102599
solve_time_s: 122
verified: true
draft: false
---

[CF 102599B - \u041b\u0438\u043f\u0435\u0446\u043a\u043e\u0435 \u043c\u0435\u0442\u0440\u043e](https://codeforces.com/problemset/problem/102599/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bản đồ tàu điện ngầm với`N`trạm. Mỗi trạm có thể chỉ định tối đa một trạm khác mà nó được kết nối. Nếu như`p[i]`không phải`-1`, có một đường hầm vô hướng giữa ga`i`và trạm`p[i]`. Nhiệm vụ là xác định xem có tồn tại tuyến đường ghé thăm mỗi trạm chính xác một lần và chỉ di chuyển qua các đường hầm hay không. Nếu có tuyến đường như vậy thì chúng ta phải xuất thứ tự trạm. 

Đồ thị có cấu trúc đặc biệt. Mỗi trạm đóng góp tối đa một cạnh, do đó tổng số đường hầm nhiều nhất là`N`. Bài toán đường đi Hamilton tổng quát là khó, nhưng một đồ thị có nhiều nhất`N`các cạnh là một cây hoặc một đồ thị bao gồm một cây có một cạnh phụ. Hạn chế này là những gì làm cho một giải pháp tuyến tính có thể thực hiện được. 

Với`N`lên đến`2 * 10^5`, bất kỳ giải pháp nào thử các điểm bắt đầu, hoán vị hoặc quay lui theo cấp số nhân khác nhau đều không thể thực hiện được. Chúng ta cần một thuật toán gần`O(N)`, bởi vì chỉ có vài triệu thao tác phù hợp thoải mái trong thời hạn. 

Những trường hợp nguy hiểm không chỉ là những biểu đồ bị ngắt kết nối. Biểu đồ được kết nối vẫn có thể không thành công vì không thể đi qua cây phân nhánh mà không truy cập lại trạm. Ví dụ:```
4
2 1 1 -1
```Đồ thị là`3-1-2`với một chiếc lá bổ sung`4`gắn liền với`1`. Tuyến đường đi qua tất cả các ga sẽ cần phải vào ga`1`ba lần, đó là điều không thể. Câu trả lời đúng là:```
NO
```Một trường hợp phức tạp khác là một chu trình có nhiều nhánh. Chỉ riêng một chu trình là có hiệu quả, nhưng một khi có quá nhiều nhánh được gắn vào thì sẽ không có đủ đầu tự do trên đường Hamilton. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng tuyến đường theo từng ga, chọn mọi ga tiếp theo có thể. Điều này đúng vì nó liệt kê mọi đường đi Hamilton có thể có, nhưng số khả năng tăng lên theo giai thừa. Ngay cả một đồ thị chỉ có vài chục đỉnh cũng khiến phương pháp này không thể sử dụng được. 

Quan sát hữu ích là biểu đồ cực kỳ thưa thớt. Một đồ thị liên thông có nhiều nhất`N`các cạnh là cây hoặc đồ thị một vòng. Đường đi Hamilton trong cây chỉ có thể thực hiện được khi bản thân cây đó là đường đi đơn. Trong đồ thị một vòng, chu trình mang lại sự linh hoạt nhưng các cây đính kèm cũng phải là những đường đi đơn giản và chỉ có thể có đủ nhánh cho các điểm cuối của tuyến đường. 

Giải pháp là phân loại đồ thị, rút ​​ra chu trình nếu nó tồn tại và sau đó xây dựng các hình dạng duy nhất có thể có của đường đi Hamilton. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N!) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị vô hướng từ các kết nối và cạnh đếm đã cho. Trước tiên, hãy kiểm tra xem tất cả các trạm có thuộc về một thành phần được kết nối hay không, vì một tuyến đường duy nhất không thể đi qua giữa các phần bị ngắt kết nối. 
2. Nếu đồ thị có`N - 1`các cạnh thì đó là một cái cây. Một cây chỉ có đường đi Hamilton khi mỗi đỉnh có nhiều nhất là hai bậc. Trong trường hợp đó, hãy bắt đầu từ một chiếc lá và mỗi lần đi dọc theo cạnh chưa sử dụng duy nhất. 
3. Nếu đồ thị có`N`các cạnh, hãy tìm chu trình bằng cách liên tục loại bỏ các lá. Các đỉnh còn lại sau quá trình này tạo thành chu trình duy nhất. 
4. Đối với mỗi đỉnh chu kỳ, hãy kiểm tra những cây treo trên đỉnh đó. Mỗi phần treo phải là một đường dẫn duy nhất. Lưu trữ đường dẫn đó từ lá của nó tới chu trình. 
5. Nếu không có đường treo thì chính vòng tuần hoàn chính là câu trả lời. 
6. Nếu có chính xác một đường treo, hãy bắt đầu từ lá của nó, đến đỉnh chu kỳ và sau đó tiếp tục đi vòng quanh chu kỳ. 
7. Nếu có hai đường treo thì chúng phải được gắn vào các đỉnh chu trình liền kề. Bắt đầu từ một lá, đi qua nhánh đầu tiên, đi vòng quanh chu trình mà không sử dụng cạnh giữa hai đỉnh chu kỳ đó và kết thúc qua nhánh thứ hai. 
8. Bất kỳ cấu trúc nào khác không thể chứa đường đi Hamilton. 

Điều bất biến đằng sau việc xây dựng là mỗi trạm nội bộ của tuyến cuối cùng phải có chính xác hai cạnh sự cố được sử dụng, trong khi hai điểm cuối có thể chỉ có một. Thuật toán loại bỏ mọi cấu trúc mà trạm cần được truy cập từ ba hướng khác nhau. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    if n == 1:
        print("YES")
        print(1)
        return

    g = [[] for _ in range(n)]
    edges = 0

    for i, x in enumerate(p):
        if x != -1:
            x -= 1
            g[i].append(x)
            g[x].append(i)
            edges += 1

    seen = [False] * n
    stack = [0]
    seen[0] = True
    while stack:
        v = stack.pop()
        for u in g[v]:
            if not seen[u]:
                seen[u] = True
                stack.append(u)

    if not all(seen):
        print("NO")
        return

    def tree_path():
        for i in range(n):
            if len(g[i]) <= 1:
                start = i
                break
        ans = []
        prev = -1
        cur = start
        while cur != -1:
            ans.append(cur)
            nxt = -1
            for u in g[cur]:
                if u != prev:
                    nxt = u
                    break
            prev, cur = cur, nxt
        return ans

    if edges == n - 1:
        if max(map(len, g)) > 2:
            print("NO")
        else:
            print("YES")
            print(*[x + 1 for x in tree_path()])
        return

    if edges != n:
        print("NO")
        return

    deg = [len(x) for x in g]
    q = deque(i for i in range(n) if deg[i] == 1)
    removed = [False] * n

    while q:
        v = q.popleft()
        removed[v] = True
        for u in g[v]:
            if not removed[u]:
                deg[u] -= 1
                if deg[u] == 1:
                    q.append(u)

    cycle = [i for i in range(n) if not removed[i]]
    cycle_set = set(cycle)

    order = []
    start = cycle[0]
    prev = -1
    cur = start
    while True:
        order.append(cur)
        nxt = -1
        for u in g[cur]:
            if u != prev and u in cycle_set:
                nxt = u
                break
        prev, cur = cur, nxt
        if cur == start:
            break

    def get_branch(c, nxt):
        res = [c]
        prev = c
        cur = nxt
        while True:
            res.append(cur)
            candidates = [u for u in g[cur] if u != prev and u not in cycle_set]
            if len(candidates) > 1:
                return None
            if not candidates:
                break
            prev, cur = cur, candidates[0]
        return res[::-1]

    branches = {}
    bad = False
    for c in cycle:
        arr = []
        for u in g[c]:
            if u not in cycle_set:
                b = get_branch(c, u)
                if b is None:
                    bad = True
                else:
                    arr.append(b)
        if len(arr) > 1:
            bad = True
        if arr:
            branches[c] = arr[0]

    if bad or len(branches) > 2:
        print("NO")
        return

    def rotate_after(x):
        k = order.index(x)
        return order[k + 1:] + order[:k]

    if not branches:
        ans = order
    elif len(branches) == 1:
        c, b = next(iter(branches.items()))
        ans = b + rotate_after(c)
    else:
        c1, c2 = list(branches.keys())
        if c2 not in g[c1]:
            print("NO")
            return
        i = order.index(c1)
        if order[(i + 1) % len(order)] == c2:
            middle = order[i + 2:] + order[:i + 1]
        else:
            middle = order[i - 1:i - len(order):-1]
        ans = branches[c1] + middle + branches[c2][::-1][1:]

    if len(ans) != n:
        print("NO")
    else:
        print("YES")
        print(*[x + 1 for x in ans])

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tách ba trường hợp cấu trúc: đồ thị rời rạc, cây và đồ thị một vòng. Quá trình loại bỏ lá được sử dụng vì việc xóa liên tục các đỉnh bậc một sẽ loại bỏ mọi cây treo trong chu trình và để lại chính xác các đỉnh chu trình. 

Việc xác nhận chi nhánh là phần tinh tế. Một nhánh chỉ có thể là một chuỗi. Nếu một đỉnh không có chu trình có hai đỉnh con không được sử dụng thì tuyến đường sẽ cần phải tách ra, điều này là không thể đối với một đường đi duy nhất. 

Việc xây dựng không bao giờ xem lại một trạm vì mỗi phần được thêm vào chính xác một lần: đầu tiên là một nhánh có thể, sau đó là một phần của chu trình, sau đó là nhánh thứ hai có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi đỉnh và cạnh được xử lý một số lần không đổi | 
| Không gian | O(N) | Danh sách kề, hàng đợi và mảng trợ giúp lưu trữ biểu đồ | 

Độ phức tạp tuyến tính là cần thiết cho`N = 2 * 10^5`. Thuật toán chỉ thực hiện duyệt đồ thị và kiểm tra cục bộ nên dễ dàng khớp trong giới hạn.
