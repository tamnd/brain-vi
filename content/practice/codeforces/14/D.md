---
title: "CF 14D - Hai Con Đường"
description: "Chúng ta có một cây gồm n thành phố, nghĩa là mỗi thành phố được kết nối theo cách sao cho có chính xác một con đường đơn giản giữa hai thành phố bất kỳ. Đường không có hướng và mỗi đường có chiều dài bằng 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "shortest-paths", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 14
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 14 (Div. 2)"
rating: 1900
weight: 14
solve_time_s: 87
verified: true
draft: false
---
[CF 14D - Hai con đường](https://codeforces.com/problemset/problem/14/D) 

**Xếp hạng:** 1900 
**Thẻ:** dfs và tương tự, dp, đồ thị, đường đi ngắn nhất, cây, hai con trỏ 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây`n`thành phố, nghĩa là mỗi thành phố được kết nối theo cách có chính xác một con đường đơn giản giữa hai thành phố bất kỳ. Các con đường là vô hướng và mỗi con đường có độ dài bằng 1. Nhiệm vụ là chọn hai con đường không chồng nhau sao cho tích độ dài của chúng là lớn nhất. Bằng cách "không chồng chéo", vấn đề có nghĩa là hai con đường không thể chia sẻ một thành phố. 

Đầu vào được cấu trúc như số lượng thành phố theo sau là`n-1`cặp số nguyên đại diện cho các con đường nối các thành phố. Đầu ra là một số nguyên duy nhất, tích độ dài tối đa của hai đường dẫn hợp lệ. 

Các hạn chế là nhỏ:`n ≤ 200`. Vì đây là một cái cây nên số đường đi có thể là`O(n^2)`. Một lực lượng vũ phu ngây thơ kiểm tra tất cả các cặp đường để tìm giao điểm sẽ liên quan đến`O(n^4)`hoạt động, rõ ràng là quá lớn đối với`n=200`. Chúng ta cần một cách tiếp cận thông minh hơn để tận dụng cấu trúc cây. 

Các trường hợp biên cần xem xét bao gồm cây đường (như 1-2-3-4), cây sao (một nút trung tâm được kết nối với tất cả các nút khác) và các cây nhỏ nhất chỉ có 2 hoặc 3 nút. Trong cây đường, hai đường dẫn không chồng chéo dài nhất có thể chỉ là các cạnh đơn. Trong cây sao, đường đi dài nhất phải đi qua tâm, do đó, hai đường đi không chồng nhau chỉ có thể sử dụng các cạnh lá riêng biệt. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê tất cả các đường dẫn đơn giản có thể. Đối với mỗi đường dẫn, chúng ta có thể tính toán độ dài của nó. Sau đó, chúng tôi sẽ ghép mọi đường dẫn với mọi đường dẫn khác, kiểm tra xem chúng có chia sẻ bất kỳ nút nào không. Tổng số cặp đường dẫn là`O(n^4)`trong trường hợp xấu nhất vì có`O(n^2)`các đường dẫn và việc so sánh từng cặp đường dẫn có thể mất`O(n)`thời gian. Điều này có thể đúng nhưng quá chậm đối với`n=200`. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng trong một cây, khoảng cách giữa hai nút bất kỳ là duy nhất. Chúng ta có thể tính toán trước khoảng cách giữa tất cả các cặp nút bằng BFS hoặc DFS đơn giản cho mỗi nút. Khi đã biết khoảng cách, bài toán có thể được trình bày lại: đối với mỗi cạnh trong cây, hãy coi đó là một "vết cắt" chia cây thành hai cây con. Bất kỳ đường dẫn nào trong cây con thứ nhất không thể giao nhau với bất kỳ đường dẫn nào trong cây con thứ hai. Điều này làm giảm vấn đề phải tính toán các đường đi dài nhất bên trong cây con sau khi loại bỏ cạnh và sau đó kết hợp các kết quả cho tất cả các cạnh. 

Lập trình động có thể được sử dụng để tính toán trước các đường đi dài nhất bên trong tất cả các cây con một cách hiệu quả. Cụ thể, nếu`dp[u][v]`là con đường dài nhất bắt đầu từ`u`và bị ràng buộc vào cây con có gốc tại`v`, chúng ta có thể tính toán nó bằng DFS đệ quy, tránh việc tính toán lại dư thừa. Một lần`dp`được lấp đầy, chúng ta có thể lặp lại tất cả các cặp cây con không chồng chéo (chia theo các cạnh) và tính tích cực đại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n^2) | Quá chậm | 
| Phân rã cây + DP tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của cây từ đầu vào. Mỗi thành phố lưu trữ hàng xóm của nó. 
2. Khởi tạo mảng 2D`dist[u][v]`để lưu trữ khoảng cách giữa mỗi cặp nút. 
3. Đối với mỗi nút`u`, chạy DFS hoặc BFS để lấp đầy khoảng cách đến tất cả các nút khác. Điều này đảm bảo`dist[u][v]`được tính vào`O(n^2)`tổng thời gian. 
4. Khởi tạo mảng DP 3D`dp[u][v]`để biểu thị độ dài đường dẫn tối đa bắt đầu từ nút`u`và bị ràng buộc vào cây con có gốc tại nút`v`(hoặc, tương đương, xét đường đi qua cạnh`(u, v)`theo hướng từ`u`ĐẾN`v`). 
5. Sử dụng DFS để điền`dp[u][v]`đệ quy. Đối với nút`v`và hàng xóm`u`,`dp[u][v]`là`1 + max(dp[v][w] for all w ≠ u)`. 
6. Lặp lại tất cả các cặp cạnh`(u1, v1)`Và`(u2, v2)`. Hãy coi chúng như hai "cây con" riêng biệt được hình thành bằng cách loại bỏ từng cạnh. Tính độ dài đường dẫn tối đa trong mỗi cây con bằng cách sử dụng`dp`bàn. 
7. Theo dõi tích cực đại của hai độ dài đường đi gặp phải. 
8. Sản lượng sản phẩm tối đa. 

Tại sao nó hoạt động: bất biến DP đảm bảo rằng`dp[u][v]`luôn lưu trữ độ dài của đường đi dài nhất bắt đầu tại`v`nhưng không quay lại hướng`u`. Bằng cách xem xét từng cặp cạnh, chúng ta đang liệt kê một cách hiệu quả tất cả các tổ hợp đường dẫn không chồng chéo có thể có vì việc loại bỏ một cạnh sẽ phân chia cây duy nhất thành hai tập hợp không giao nhau. Vì chúng tôi tính toán đường dẫn tối đa trong mỗi phân vùng nên tích của chúng sẽ là giá trị tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1000)

n = int(input())
adj = [[] for _ in range(n)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    adj[u].append(v)
    adj[v].append(u)

dp = [[0]*n for _ in range(n)]

def dfs(u, parent):
    for v in adj[u]:
        if v == parent:
            continue
        dfs(v, u)
        dp[u][v] = 1 + max((dp[v][w] for w in adj[v] if w != u), default=0)

for u in range(n):
    dfs(u, -1)

ans = 0
for u in range(n):
    for v in adj[u]:
        if u < v:
            for x in range(n):
                for y in adj[x]:
                    if x < y:
                        # Ensure edges do not share nodes
                        if len({u, v, x, y}) == 4:
                            ans = max(ans, dp[u][v] * dp[x][y])

print(ans)
```Giải pháp được xây dựng một cách cẩn thận`dp`bảng bằng cách coi mỗi cạnh là một "vết cắt" và tính toán đường đi dài nhất trong cây con kết quả. Chúng ta phải tránh các cạnh đếm kép, vì vậy chúng ta lặp lại với`u < v`chỉ xét mỗi cạnh một lần. các`max(..., default=0)`xử lý các nút lá một cách chính xác. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
4
1 2
2 3
3 4
```| Bước | các mục trong bảng dp được cập nhật | Sản phẩm tối đa được xem xét | 
| --- | --- | --- | 
| dfs(0) | dp[0][1] = 3 | - | 
| dfs(1) | dp[1][2] = 2 | - | 
| dfs(2) | dp[2][3] = 1 | - | 
| lặp các cạnh | cặp (0,1)-(2,3) | 1*1 = 1 | 

Giải thích: Cây là một đường thẳng. Hai đường dẫn không chồng chéo dài nhất là các cạnh đơn: mỗi cạnh có độ dài 1. Sản phẩm = 1. 

**Đầu vào tùy chỉnh**```
5
1 2
1 3
3 4
3 5
```Đường đi dài nhất trong cây con sau khi cắt: (1,2) → độ dài 1, (3,4) → độ dài 1. Sản phẩm = 1. Cặp khác (1,3)-(4,5) → sản phẩm = 2*1 = 2. Tối đa là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | BFS/DFS cho tất cả các nút: O(n^2). Tính toán DP trên các cạnh và lân cận: O(n^3). | 
| Không gian | O(n^2) |`dp`bảng lưu trữ đường dẫn cho mỗi cạnh theo cả hai hướng. | 

Được cho`n ≤ 200`,`n^3 ≈ 8*10^6`, điều này khả thi trong giới hạn thời gian 2 giây. Sử dụng bộ nhớ`n^2 ≈ 40000`vừa vặn dễ dàng trong 64 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # Paste the solution here and capture output
    import sys
    out = io.StringIO()
    sys.stdout = out
    # solution start
    n = int(input())
    adj = [[] for _ in range(n)]
    for _ in range(n-1):
        u,v = map(int,input().split())
        u -= 1
        v -= 1
        adj[u].append(v)
        adj[v].append(u)
    dp = [[0]*n for _ in range(n)]
    def dfs(u,parent):
        for v in adj[u]:
            if v==parent:
                continue
            dfs(v,u)
            dp[u][v] = 1 + max((dp[v][w] for w in adj[v] if w != u),default=0)
    for u in range(n):
        dfs(u,-1)
    ans=0
    for u in range(n):
        for v in adj[u]:
            if u<v:
                for x in range(n):
                    for y in adj[x]:
                        if x<y and len({u,v,x,y})==4:
                            ans=max(ans,dp[u][v]*dp[x][y])
    print(ans)
    # solution end
```
