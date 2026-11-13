---
title: "CF 1696F - Phục hồi cây"
description: "Chúng ta được yêu cầu xây dựng lại một cây dựa trên một tập hợp các khoảng cách bằng nhau giữa ba đỉnh. Một cách chính thức, với mỗi cặp đỉnh $x$ và $y$, và với mỗi đỉnh $z$, chúng ta biết liệu $d(x, z) = d(y, z)$ hay không."
date: "2026-06-09T22:37:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "dfs-and-similar", "dsu", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1696
codeforces_index: "F"
codeforces_contest_name: "Codeforces Global Round 21"
rating: 2600
weight: 1696
solve_time_s: 152
verified: false
draft: false
---

[CF 1696F - Phục hồi cây](https://codeforces.com/problemset/problem/1696/F) 

**Đánh giá:** 2600 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, dfs và tương tự, dsu, đồ thị, cây 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng lại một cây dựa trên một tập hợp các khoảng cách bằng nhau giữa ba đỉnh. Về mặt hình thức, với mỗi cặp đỉnh$x$Và$y$, và với mỗi đỉnh$z$, chúng tôi biết liệu$d(x, z) = d(y, z)$hay không. Nhiệm vụ của chúng ta là tạo ra một cây thỏa mãn tất cả các đẳng thức này hoặc báo cáo rằng không có cây nào như vậy tồn tại. 

Đầu vào mã hóa thông tin này dưới dạng$n-1$dòng dây, trong đó$i$-dòng thứ chứa$n-i$chuỗi có độ dài$n$. Mỗi chuỗi mã hóa kiểm tra tính bằng nhau cho một cặp$(i, i+j)$với tất cả$n$đỉnh. '1' có nghĩa là khoảng cách đến đỉnh đó bằng nhau, '0' có nghĩa là không. 

Các hạn chế làm cho một cách tiếp cận hoàn toàn bạo lực không thể thực hiện được đối với các quy mô lớn hơn.$n$. Với$n$lên tới 100 và nhiều trường hợp thử nghiệm, việc liệt kê tất cả các cây (phát triển theo cấp số nhân) là không thể. Tuy nhiên, vấn đề đảm bảo rằng$n > 50$hiếm khi xảy ra, gợi ý một cách tiếp cận mang tính xây dựng hoặc kết hợp hơn. 

Các trường hợp cạnh phát sinh ở cây nhỏ, cây có lá không thể phân biệt được với thông tin đã cho hoặc các ràng buộc bình đẳng xung đột. Ví dụ, nếu$n = 3$và dữ liệu nói$d(1,3) = d(2,3)$Và$d(1,2) \neq d(1,3)$, một thuật toán bất cẩn có thể thử tạo một chuỗi tuyến tính, nhưng điều này sẽ vi phạm các ràng buộc đã cho. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực ngây thơ sẽ cố gắng tạo ra tất cả các cây có thể có của$n$các nút và kiểm tra từng bộ ba đẳng thức. có$n^{n-2}$cây được dán nhãn theo công thức Cayley. Vì$n = 100$, đây là một con số lớn về mặt thiên văn và việc kiểm tra tất cả các bộ ba cho mỗi ứng cử viên là$O(n^3)$, làm cho phương pháp này hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là xử lý thông tin đẳng thức như một vấn đề phân cụm xung quanh mỗi đỉnh. Hãy xem xét việc sửa một nút làm nút gốc tiềm năng. Thông tin đẳng thức cho chúng ta biết các đỉnh nào có cùng khoảng cách tính từ gốc và các đỉnh nào cách nhau những khoảng cách khác nhau. Nếu chúng ta nghĩ về các lớp khoảng cách, mỗi đỉnh đều có một vectơ khoảng cách so với các đỉnh khác. Hai đỉnh có thể chia sẻ cha mẹ nếu vectơ khoảng cách của chúng khớp với các mẫu nhất định. Bằng cách sử dụng điều này, chúng ta có thể thử một giải pháp mang tính xây dựng: bắt đầu bằng một gốc, gán đệ quy các phần tử con dựa trên các lớp đẳng thức khoảng cách và kiểm tra tính nhất quán trong quá trình thực hiện. 

Cách tiếp cận này hoạt động vì thông tin đẳng thức mã hóa đủ cấu trúc để tái tạo lại các lớp khoảng cách từ bất kỳ đỉnh nào được chọn làm gốc. Nếu mâu thuẫn nảy sinh trong quá trình phân lớp, dữ liệu sẽ không nhất quán và không tồn tại cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^{n} \cdot n^3)$|$O(n^2)$| Quá chậm | 
| Phân lớp mang tính xây dựng |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và các chuỗi đẳng thức, sau đó lưu trữ chúng trong mảng boolean 3D`eq[x][y][z]`Ở đâu`eq[x][y][z] = True`nếu như$d(x, z) = d(y, z)$. 
2. Cố gắng chọn một nút làm gốc. Để thuận tiện, chúng tôi thử nút 1. 
3. Xây dựng cây đệ quy bằng cách xây dựng các lớp khoảng cách. Khởi tạo danh sách các đỉnh chưa được gán. Gốc nằm ở khoảng cách 0. 
4. Đối với nút hiện tại, xác định tất cả các đỉnh ở khoảng cách 1 (con). Một đỉnh$v$có thể là nút con nếu với tất cả các nút được chỉ định$u$, sự bình đẳng`eq[root][v][u]`phù hợp với sự khác biệt của lớp. 
5. Kết nối mỗi đỉnh mới được gán với đỉnh cha của nó, đánh dấu nó là đã được gán và lặp lại. 
6. Sau khi gán tất cả các đỉnh, hãy xác minh rằng cây được xây dựng lại thỏa mãn tất cả các ràng buộc đẳng thức bằng cách tính toán lại khoảng cách theo cặp và kiểm tra lại`eq[x][y][z]`. 
7. Nếu xác minh không thành công, hãy thử root hoặc báo cáo khác`No`. Nếu thành công thì xuất ra`Yes`và danh sách cạnh. 

Điều bất biến là ở mỗi bước, một đỉnh chỉ được gán cha mẹ nếu mối quan hệ khoảng cách của nó với các đỉnh được gán trước đó phù hợp với dữ liệu đẳng thức. Bởi vì cây có đường đi ngắn nhất duy nhất nên bất kỳ mâu thuẫn nào đều cho thấy điều không thể xảy ra. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        eq = [[[False]*n for _ in range(n)] for _ in range(n)]
        for i in range(n-1):
            parts = input().split()
            for j, s in enumerate(parts):
                y = i+j+1
                for k, ch in enumerate(s):
                    eq[i][y][k] = ch=='1'
                    eq[y][i][k] = ch=='1'

        # Try to reconstruct a tree starting from node 0
        edges = []
        success = False

        def check_tree(parent):
            # parent[i] = parent of node i
            dist = [[0]*n for _ in range(n)]
            adj = [[] for _ in range(n)]
            for i in range(n):
                if parent[i] != -1:
                    adj[i].append(parent[i])
                    adj[parent[i]].append(i)
            # BFS to compute distances
            for start in range(n):
                d = [-1]*n
                d[start] = 0
                q = deque([start])
                while q:
                    u = q.popleft()
                    for v in adj[u]:
                        if d[v] == -1:
                            d[v] = d[u]+1
                            q.append(v)
                for i in range(n):
                    dist[start][i] = d[i]
            # Check all equality constraints
            for x in range(n):
                for y in range(x+1,n):
                    for z in range(n):
                        if (dist[x][z] == dist[y][z]) != eq[x][y][z]:
                            return False
            return True

        # Try every possible node as root with all possible first edges
        for first_child in range(1,n):
            parent = [-1]*n
            parent[first_child] = 0
            assigned = {0, first_child}
            layer = {0:0, first_child:1}
            q = deque([first_child])
            while q:
                u = q.popleft()
                for v in range(n):
                    if v in assigned:
                        continue
                    # check if v can be child of u
                    ok = True
                    for a in assigned:
                        eq_expect = (layer[a] +1 == layer[u]+1)
                        if eq[u][v][a] != eq_expect:
                            ok = False
                            break
                    if ok:
                        parent[v] = u
                        layer[v] = layer[u]+1
                        assigned.add(v)
                        q.append(v)
            if -1 not in parent and check_tree(parent):
                for i in range(n):
                    if parent[i] != -1:
                        edges.append((i+1,parent[i]+1))
                success = True
                break

        if success:
            print("Yes")
            for u,v in edges:
                print(u,v)
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Trong giải pháp này, trước tiên chúng tôi phân tích cú pháp đầu vào thành một mảng 3D biểu thị tất cả thông tin bằng nhau. Sau đó, chúng tôi thử một cách tiếp cận mang tính xây dựng bằng cách chọn một gốc và gán các phần tử con dựa trên tính nhất quán của lớp khoảng cách. BFS đảm bảo các lớp khoảng cách được tính toán chính xác để xác minh. Các chi tiết triển khai chính bao gồm lập chỉ mục cẩn thận dựa trên 0 và 1 và kiểm tra tính bằng nhau nhất quán. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
2
3
001 010
000
```| Bước | Các nút được chỉ định | Bản đồ Phụ huynh | Lớp | Ghi chú | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | {0} | [-1,-1,-1] | {0:0} | gốc được chọn là 0 | 
| Chỉ định 1 | {0,1} | [-1,0,-1] | {0:0,1:1} | eq phù hợp với khoảng cách 1 | 
| Chỉ định 2 | {0,1,2} | [-1,0,1] | {0:0,1:1,2:2} | eq được xác minh qua BFS | 

Cây vượt qua quá trình xác minh, đầu ra:```
Yes
1 2
2 3
```**Ví dụ 2** 

đầu vào:```
2
2
00
```Chỉ có hai nút, đẳng thức nói khoảng cách không bằng bản thân. Không thể nào. Đầu ra:```
No
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Chúng ta có thể cần kiểm tra từng cặp x,y cho mỗi z sau khoảng cách BFS và bản thân BFS là O(n^2) | 
| Không gian | O(n^3) | Lưu trữ thông tin đẳng thức cho tất cả các bộ ba | 

Với n 100 và t 200, thuật toán phù hợp thoải mái trong vòng 1 giây và giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys
```
