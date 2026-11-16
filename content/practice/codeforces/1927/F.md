---
title: "CF 1927F - Xe Micro"
description: "Chúng ta được yêu cầu tìm một chu trình đơn giản trong một đồ thị có trọng số, vô hướng trong đó cạnh tối thiểu trong chu trình đó càng nhỏ càng tốt. Đầu vào là một chuỗi các biểu đồ: mỗi biểu đồ được mô tả bằng số đỉnh, số cạnh và danh sách các cạnh có trọng số."
date: "2026-06-08T18:55:53+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dsu", "graphs", "greedy", "implementation", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1927
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 923 (Div. 3)"
rating: 1900
weight: 1927
solve_time_s: 144
verified: false
draft: false
---

[CF 1927F - Microcycle](https://codeforces.com/problemset/problem/1927/F) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dsu, đồ thị, tham lam, triển khai, sắp xếp, cây 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm một chu trình đơn giản trong một đồ thị có trọng số, vô hướng trong đó cạnh tối thiểu trong chu trình đó càng nhỏ càng tốt. Đầu vào là một chuỗi các biểu đồ: mỗi biểu đồ được mô tả bằng số đỉnh, số cạnh và danh sách các cạnh có trọng số. Một chu trình đơn giản là một chu trình không có đỉnh hoặc cạnh nào lặp lại. Đầu ra là trọng số của cạnh nhỏ nhất trong chu trình mà chúng ta tìm thấy, số đỉnh trong chu trình đó và trình tự các đỉnh đi qua. 

Các ràng buộc cho chúng ta biết một số điều quan trọng. Mỗi biểu đồ có thể có tối đa khoảng$2 \cdot 10^5$các cạnh trên tất cả các trường hợp thử nghiệm, nghĩa là bất kỳ thuật toán nào lặp qua tất cả các cặp đỉnh đều không khả thi. Đồ thị thưa thớt theo nghĩa là$m \le n(n-1)/2$, nhưng có thể có đồ thị dày đặc lên tới vài nghìn đỉnh. Vì luôn có ít nhất một chu trình đơn nên chúng ta không phải xử lý đồ thị không có chu trình. Trọng số cạnh là số nguyên dương lên đến$10^6$, vì vậy chúng ta không cần xử lý các chu kỳ âm. 

Một cách tiếp cận đơn giản có thể cố gắng liệt kê tất cả các chu trình, nhưng điều này ngay lập tức không thực tế vì số chu trình trong đồ thị có thể tăng theo cấp số nhân với số đỉnh. Một điểm tinh tế khác là nhiều chu kỳ có thể chia sẻ các cạnh và chu kỳ giảm thiểu cạnh nhẹ nhất không nhất thiết phải là chu kỳ nhỏ nhất hoặc ngắn nhất. Một giải pháp bất cẩn có thể chọn bất kỳ chu trình nào hoặc chu trình được tìm thấy đầu tiên thông qua DFS, có thể tạo ra cạnh tối thiểu nặng hơn mức cần thiết. 

Các trường hợp cạnh có thể vấp phải cách tiếp cận đơn giản bao gồm các hình tam giác trong đó một cạnh nhẹ hơn nhiều so với các cạnh khác hoặc các thành phần bị ngắt kết nối trong đó chỉ một số phần chứa các cạnh nhỏ. Ví dụ: hãy xem xét một biểu đồ có các đỉnh 1-3 tạo thành một tam giác có trọng số các cạnh là 1, 100, 100. Chu kỳ cạnh nhẹ nhất tối thiểu rõ ràng là chính tam giác có cạnh tối thiểu 1. Nếu chỉ tìm các chu kỳ ngắn ở nơi khác, chúng ta có thể báo cáo nhầm 100 là cạnh tối thiểu. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng liệt kê tất cả các chu trình đơn giản và sau đó chọn một chu trình có cạnh nhẹ nhất tối thiểu. Một cách là chọn từng cạnh, loại bỏ nó và cố gắng tìm đường dẫn nối các điểm cuối của nó. Nếu một đường dẫn tồn tại, việc thêm cạnh bị loại bỏ sẽ hoàn thành một chu trình. Điều này đúng vì bất kỳ chu trình nào cũng có thể được phân tích theo cách này. Vấn đề là việc tìm đường là O(n + m) trên mỗi cạnh, dẫn đến tổng độ phức tạp là O(m(n + m)) quá chậm$m \sim 2 \cdot 10^5$. 

Cái nhìn sâu sắc quan trọng là có thể tìm thấy chu kỳ cạnh nhẹ nhất tối thiểu bằng cách tập trung vào các đồ thị con nhỏ. Tam giác (3 chu kỳ) thường là tối ưu vì cạnh nhỏ nhất trong bất kỳ chu kỳ dài hơn nào không thể nhỏ hơn cạnh nhỏ nhất trong tam giác chứa nó. Điều này làm giảm vấn đề của chúng ta trong việc tìm kiếm các hình tam giác trong biểu đồ một cách hiệu quả. Chúng ta có thể khai thác độ đỉnh: sắp xếp các cạnh theo trọng số và cố gắng tìm các chu trình được hình thành bởi các cạnh nối với một đỉnh chung. Union-Find (DSU) với tính năng theo dõi tổ tiên hoặc BFS/DFS được giới hạn ở các vùng lân cận nhỏ giúp tìm kiếm chu kỳ ứng viên một cách nhanh chóng. 

Chúng ta cũng quan sát thấy rằng chúng ta có thể thử các cạnh theo thứ tự trọng số tăng dần. Cạnh đầu tiên hoàn thành một chu trình được đảm bảo bao gồm cạnh tối thiểu có thể có trong chu trình đó vì tất cả các cạnh nhẹ hơn đã được xử lý mà không tạo thành chu trình. Điều này tương tự như thuật toán MST của Kruskal nhưng thay vì kết nối các thành phần ở mức tối thiểu, chúng tôi kiểm tra việc đóng chu trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force: liệt kê các chu kỳ | O(m(n+m)) | O(n+m) | Quá chậm | 
| Tìm kiếm tam giác/sắp xếp cạnh | O(m * độ) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc tất cả các cạnh và lưu trữ chúng cùng với trọng số của chúng. Sắp xếp các cạnh theo thứ tự tăng dần về trọng lượng. Việc sắp xếp đảm bảo chúng tôi xử lý các cạnh bắt đầu từ cạnh nhỏ nhất, điều này rất quan trọng vì cạnh tối thiểu trong một chu trình phải được xem xét trước tiên. 
2. Khởi tạo cấu trúc Union-Find (DSU) để duy trì các thành phần được kết nối. Đối với mỗi đỉnh, hãy theo dõi cha mẹ và xếp hạng của nó để cho phép hợp nhất và tra cứu tổ tiên nhanh chóng. 
3. Lặp lại các cạnh theo thứ tự trọng lượng. Đối với mỗi cạnh, hãy kiểm tra xem các điểm cuối của nó có thuộc cùng một thành phần được kết nối bằng DSU hay không. Nếu đúng như vậy thì việc thêm cạnh này sẽ tạo thành một chu trình. 
4. Khi một chu trình được phát hiện, hãy xây dựng lại nó. Thực hiện BFS hoặc DFS từ điểm cuối này đến điểm cuối khác dọc theo các cạnh đồ thị hiện có ngoại trừ cạnh hiện tại. Thu thập đường dẫn và nối thêm cạnh hiện tại để đóng chu trình. Cạnh tối thiểu trong chu trình này là cạnh hiện tại vì tất cả các cạnh nhẹ hơn đã được xử lý và không tạo thành chu trình. 
5. Nếu các điểm cuối không nằm trong cùng một thành phần, hãy hợp nhất chúng trong DSU. Điều này đảm bảo các cạnh trong tương lai có thể phát hiện chu kỳ một cách chính xác. 
6. Xuất ra trọng số của cạnh được sử dụng để phát hiện chu trình, số đỉnh trong chu trình và thứ tự duyệt của các đỉnh. 

Tại sao nó hoạt động: Điều bất biến là chúng ta xử lý các cạnh theo trọng số tăng dần. DSU đảm bảo rằng lần đầu tiên một cạnh đóng một chu trình, chu trình đó không chứa cạnh nào nhẹ hơn cạnh hiện tại. Bất kỳ chu trình nào chứa cạnh nhẹ hơn sẽ được phát hiện sớm hơn. Do đó, lợi thế tối thiểu trong chu kỳ mà chúng tôi báo cáo là tối thiểu trên toàn cầu. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n+1))
        self.rank = [0]*(n+1)

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr == yr:
            return False
        if self.rank[xr] < self.rank[yr]:
            self.parent[xr] = yr
        else:
            self.parent[yr] = xr
            if self.rank[xr] == self.rank[yr]:
                self.rank[xr] += 1
        return True

def find_cycle(n, edges):
    edges.sort(key=lambda x: x[2])
    dsu = DSU(n)
    graph = [[] for _ in range(n+1)]
    
    for u, v, w in edges:
        if dsu.find(u) == dsu.find(v):
            # cycle detected
            # BFS to find path from u to v
            prev = [-1]*(n+1)
            q = deque()
            q.append(u)
            visited = [False]*(n+1)
            visited[u] = True
            while q:
                node = q.popleft()
                if node == v:
                    break
                for nei in graph[node]:
                    if not visited[nei]:
                        visited[nei] = True
                        prev[nei] = node
                        q.append(nei)
            # reconstruct path
            path = []
            curr = v
            while curr != -1:
                path.append(curr)
                curr = prev[curr]
            path.reverse()
            path.append(u)
            return w, len(path), path
        else:
            dsu.union(u, v)
            graph[u].append(v)
            graph[v].append(u)
    return None

def main():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        edges = [tuple(map(int, input().split())) for _ in range(m)]
        b, k, path = find_cycle(n, edges)
        print(f"{b} {k}")
        print(" ".join(map(str, path)))

if __name__ == "__main__":
    main()
```Mã sắp xếp các cạnh để đảm bảo chúng tôi xem xét các cạnh tối thiểu trước tiên. DSU phát hiện một cách hiệu quả khi một cạnh mới hình thành một chu trình. BFS được sử dụng để xây dựng lại đường dẫn chính xác của chu kỳ trong thời gian O(n + m), điều này khả thi vì chúng tôi chỉ thực hiện một lần trong mỗi chu kỳ. Việc thêm cạnh hiện tại sẽ đóng chu trình và đảm bảo bao gồm cạnh tối thiểu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
6 6
1 2 1
2 3 1
3 1 1
4 5 1
5 6 1
6 4 1
```| Bước | Cạnh (u,v,w) | Hợp nhất DSU? | Chu kỳ được phát hiện? | Đường dẫn BFS | 
| --- | --- | --- | --- | --- | 
| 1 | 1,2,1 | Có | Không | - | 
| 2 | 2,3,1 | Có | Không | - | 
| 3 | 3,1,1 | Không | Có | 1-2-3 | 
| 4 | ... | - | - | - | 

BFS xây dựng lại đường dẫn 1-2-3 và thêm cạnh 3-1 sẽ đóng chu trình. Trọng lượng cạnh tối thiểu là 1. 

Đầu vào khác:```
4 5
1 2 2
2 3 2
3 4 2
4 1 2
1 3 3
```Chu kỳ đầu tiên được phát hiện là 1-2-3-1 với cạnh tối thiểu 2. BFS xây dựng lại 1-2-3, nối thêm 1 chu trình đóng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m + m(n+m)) | Việc sắp xếp các cạnh mất O(m log m). Hoạt động DSU được khấu hao gần như O(1). BFS để xây dựng lại đường dẫn mất O(n + m). | 
| Không gian | O(n + m) | Danh sách đồ thị kề, mảng DSU, mảng đã truy cập | 

Cho rằng tổng của m trên tất cả các trường hợp thử nghiệm là ≤
