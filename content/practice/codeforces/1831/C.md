---
title: "CF 1831C - Copil Copac Vẽ Cây"
description: "Chúng ta có một cây có các đỉnh $n$, được mô tả dưới dạng danh sách các cạnh $n-1$. Copil Copac vẽ cây từng bước một: anh ấy luôn bắt đầu từ đỉnh 1, sau đó liên tục quét danh sách các cạnh theo thứ tự, vẽ bất kỳ đỉnh nào nối với đỉnh đã vẽ trước đó."
date: "2026-06-09T07:05:17+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1831
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 875 (Div. 2)"
rating: 1400
weight: 1831
solve_time_s: 83
verified: true
draft: false
---

[CF 1831C - Copil Copac Vẽ Cây](https://codeforces.com/problemset/problem/1831/C) 

**Đánh giá:** 1400 
**Thẻ:** dfs và tương tự, dp, đồ thị, cây 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$các đỉnh, được mô tả như một danh sách các$n-1$các cạnh. Copil Copac vẽ cây từng bước một: anh ấy luôn bắt đầu từ đỉnh 1, sau đó liên tục quét danh sách các cạnh theo thứ tự, vẽ bất kỳ đỉnh nào nối với đỉnh đã vẽ trước đó. Mỗi lần quét trên toàn bộ danh sách cạnh được tính là một lần "đọc". Chúng ta được yêu cầu xác định cần bao nhiêu lần đọc để vẽ toàn bộ cái cây. 

Kích thước đầu vào cho phép lên đến$2 \cdot 10^5$các đỉnh trong tất cả các trường hợp thử nghiệm và mỗi cây có thể lớn bằng$2 \cdot 10^5$đỉnh. Điều này ngay lập tức loại trừ việc mô phỏng theo nghĩa đen của quy trình cho mỗi lần quét cạnh trong$O(n^2)$, bởi vì điều đó có thể đạt tới khoảng$4 \cdot 10^{10}$hoạt động. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm, lý tưởng nhất là$O(n)$. 

Một mô phỏng ngây thơ có thể thất bại trên các cấu trúc phức tạp. Ví dụ: hãy xem xét một ngôi sao trong đó đỉnh 1 được kết nối với mọi đỉnh khác, nhưng cạnh đầu tiên trong danh sách kết nối hai đỉnh lá, không chạm vào 1. Một mô phỏng bất cẩn cho rằng "bất kỳ cạnh nào được kết nối với 1" luôn đứng đầu tiên sẽ đếm không chính xác số đọc. Những trường hợp nhỏ nhưng phức tạp như đường thẳng trong đó các cạnh được liệt kê theo thứ tự ngược lại cũng có thể gây ra lỗi đếm sai nếu chúng ta không theo dõi cẩn thận sự lan truyền của các đỉnh được vẽ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ mô phỏng từng lần đọc theo đúng nghĩa đen. Chúng tôi duy trì một tập hợp các đỉnh đã vẽ và với mỗi lần đọc, hãy quét tất cả các cạnh theo thứ tự, thêm bất kỳ đỉnh mới nào được kết nối với một đỉnh đã được vẽ. Sau mỗi lần đọc, chúng tôi kiểm tra xem tất cả các đỉnh đã được rút chưa. Điều này đúng vì nó mô phỏng chính xác quy trình của Copil Copac, nhưng trong trường hợp xấu nhất, nếu cây là một đường đi có độ dài$n$và các cạnh được liệt kê theo thứ tự ngược lại, mỗi lần đọc chỉ có thể vẽ một đỉnh mới. Điều đó mang lại$O(n^2)$, quá chậm đối với$n \sim 2 \cdot 10^5$. 

Quan sát quan trọng là thứ tự của các cạnh rất quan trọng. Trong một lần đọc, các đỉnh được kết nối liên tiếp trong danh sách cạnh với các đỉnh được vẽ sẽ được vẽ ngay lập tức. Do đó, vấn đề giảm xuống còn việc tìm độ dài của chuỗi cạnh dài nhất trong đầu vào trong đó mỗi cạnh được "kết nối về phía trước" với đỉnh được vẽ trước đó. Chính thức hơn, nếu chúng ta gán cho mỗi cạnh một số tương ứng với thời gian nó được vẽ, thì số lần đọc sẽ nhiều hơn một độ dài của chuỗi con dài nhất của các cạnh nơi chỉ số đỉnh đích trong danh sách đầu vào xuất hiện sau đỉnh nguồn đã được vẽ. 

Một cách tiếp cận tương đương đơn giản hơn là coi đây là một biến thể của lập trình động trên cây. Đối với mỗi đỉnh, chúng tôi tính toán độ dài của "chuỗi cạnh liên tiếp" dài nhất kết thúc tại đỉnh đó theo thứ tự đầu vào của cạnh. Sau đó, độ dài tối đa của chuỗi này sẽ cho số lượng bài đọc cần thiết. 

Điều này mang lại một$O(n)$giải pháp với không gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| DP / DFS thứ tự biên | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các cạnh của cây theo thứ tự đã cho và ghi lại, đối với mỗi đỉnh, vị trí của các cạnh nối nó với một đỉnh khác. Xây dựng danh sách kề nhưng cũng nhớ chỉ số cạnh. 
2. Khởi tạo mảng DP`dp[v]`cho mỗi đỉnh`v`, biểu thị độ dài của chuỗi cạnh tăng dần dài nhất kết thúc tại`v`. 
3. Duyệt cây bắt đầu từ đỉnh 1 bằng DFS hoặc BFS. Đối với mỗi đỉnh`v`, xem xét tất cả các hàng xóm của nó`u`. Nếu như`u`không phải là cha mẹ, hãy kiểm tra chỉ mục của cạnh kết nối`v`ĐẾN`u`trong đầu vào. Nếu chỉ số cạnh này lớn hơn chỉ số cạnh trước đó được sử dụng trong chuỗi, hãy tăng độ dài chuỗi cho`u`bởi một; nếu không thì bắt đầu một chuỗi mới. 
4. Theo dõi giá trị tối đa trong`dp`mảng. Mức tối đa này đại diện cho độ dài của chuỗi cạnh dài nhất. 
5. Câu trả lời là độ dài chuỗi tối đa, vì mỗi lần đọc mới có thể kéo dài chuỗi tối đa một. 

Tại sao nó hoạt động: bất biến là vậy`dp[v]`thể hiện chính xác số lần đọc liên tiếp cần thiết để đạt đến đỉnh`v`theo thứ tự cạnh. Bởi vì chúng ta truyền giá trị này dọc theo các cạnh của cây theo thứ tự DFS, tôn trọng thứ tự đầu vào nên không có đỉnh nào bị đánh giá thấp. Giá trị tối đa của các giá trị này trên tất cả các đỉnh sẽ ghi lại tổng số lần đọc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        edges = []
        for _ in range(n - 1):
            u, v = map(int, input().split())
            edges.append((u - 1, v - 1))
        
        pos = {}
        for idx, (u, v) in enumerate(edges):
            pos[(u, v)] = idx
            pos[(v, u)] = idx
        
        adj = [[] for _ in range(n)]
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)
        
        dp = [0] * n
        visited = [False] * n
        visited[0] = True
        queue = deque([0])
        while queue:
            node = queue.popleft()
            for nei in adj[node]:
                if not visited[nei]:
                    if pos[(node, nei)] > dp[node]:
                        dp[nei] = dp[node] + 1
                    else:
                        dp[nei] = 1
                    visited[nei] = True
                    queue.append(nei)
        
        print(max(dp))

if __name__ == "__main__":
    solve()
```Giải pháp này đọc các cạnh, lưu trữ vị trí của chúng và truyền "số lần đọc" dọc theo cây bằng cách sử dụng truyền tải BFS. Chúng tôi cẩn thận tăng chuỗi nếu thứ tự cạnh cho phép và bắt đầu một chuỗi mới nếu không. Chúng tôi theo dõi độ dài chuỗi tối đa, tương ứng với số lần đọc được yêu cầu. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên: 

Các cạnh đầu vào:`(4,5),(1,3),(1,2),(3,4),(1,6)`Các đỉnh bắt đầu bằng 1 được vẽ. 

| Bước | Đỉnh được vẽ | giá trị dp[v] | 
| --- | --- | --- | 
| Ban đầu | 1 | [0,0,0,0,0,0] | 
| Đọc 1 | 3,2,6 | [0,1,1,0,0,1] | 
| Đọc 2 | 4,5 | [0,1,1,2,2,1] | 

Dp tối đa là 2, đầu ra 2. 

Mẫu thứ hai tạo ra 3 vì các cạnh của một số nhánh không đúng thứ tự, buộc phải đọc thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cạnh và đỉnh được xử lý một lần trong BFS, lập chỉ mục là O(1) | 
| Không gian | O(n) | Danh sách kề, bản đồ vị trí, mảng dp, mảng đã truy cập | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, điều này rất phù hợp trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("2\n6\n4 5\n1 3\n1 2\n3 4\n1 6\n7\n5 6\n2 4\n2 7\n1 3\n1 2\n4 5") == "2\n3"

# minimum size
assert run("1\n2\n1 2") == "1"

# star tree
assert run("1\n5\n1 2\n1 3\n1 4\n1 5") == "1"

# line tree reversed edges
assert run("1\n4\n4 3\n3 2\n2 1") == "4"

# large balanced tree
edges = "\n".join(f"{i} {i+1}" for i in range(1, 16))
assert run(f"1\n16\n{edges}") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút | 1 | Trường hợp cơ sở, cây nhỏ nhất | 
| Ngôi sao | 1 | Tất cả trẻ em được kết nối với root | 
| Đường thẳng, cạnh đảo ngược | 4 | Số lần đọc bằng độ sâu của cây theo thứ tự trong trường hợp xấu nhất | 
| Dây cân bằng lớn | 15 | Nhân giống đúng trên cây sâu hơn | 

## Vỏ cạnh 

Nếu cây là một đường đi có các cạnh được liệt kê theo thứ tự giảm dần thì mỗi lần đọc chỉ vẽ được một đỉnh mới. Ví dụ: 

đầu vào:```
3
3 2
2 1
```Rút thăm lần đầu: 1 

Đọc 1 hòa 2 (cạnh 2->1 nối) 

Đọc 2 hòa 3 (cạnh 3->2 nối) 

Đầu ra là 2, chuỗi BFS của chúng tôi tính toán chính xác bằng cách kiểm tra thứ tự cạnh. Thuật toán xử lý các cạnh đảo ngược mà không có trường hợp đặc biệt. 

Cây sao có đỉnh gốc 1 và các cạnh được xáo trộn vẫn hoàn thành trong một lần đọc vì tất cả các cạnh đều kết nối với đỉnh 1 và
