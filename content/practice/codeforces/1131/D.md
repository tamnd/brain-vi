---
title: "CF 1131D - Sự lựa chọn của người sành ăn"
description: "Bài toán yêu cầu chúng ta gán số nguyên dương cho hai bộ món ăn mà ông Apple đã nếm thử trong hai ngày riêng biệt. Mỗi món ăn của ngày đầu tiên có thể được so sánh với mọi món ăn của ngày thứ hai và so sánh là tốt hơn, tệ hơn hoặc bằng nhau."
date: "2026-06-12T04:13:16+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "dsu", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1131
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 541 (Div. 2)"
rating: 2000
weight: 1131
solve_time_s: 88
verified: false
draft: false
---

[CF 1131D - Lựa chọn dành cho người sành ăn](https://codeforces.com/problemset/problem/1131/D) 

**Xếp hạng:** 2000 
**Tags:** dfs và tương tự, dp, dsu, đồ thị, tham lam 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta gán số nguyên dương cho hai bộ món ăn mà ông Apple đã nếm thử trong hai ngày riêng biệt. Mỗi món ăn của ngày đầu tiên có thể được so sánh với mọi món ăn của ngày thứ hai và so sánh là tốt hơn, tệ hơn hoặc bằng nhau. Mục tiêu của chúng tôi là gán số cho các món ăn sao cho thỏa mãn các mối quan hệ này trong khi vẫn giữ được số điểm tối đa càng nhỏ càng tốt. Về cơ bản, đây là bài toán thỏa mãn ràng buộc trong đó các ràng buộc xuất phát từ việc so sánh từng cặp giữa các món ăn. 

Đầu vào là một ma trận có kích thước n x m, trong đó n là số món ăn của ngày đầu tiên và m của ngày thứ hai. Mỗi phần tử là một trong các dấu ">", "<" hoặc "=". Đầu ra phải là "Có" với hai dãy số thỏa mãn tất cả các phép so sánh hoặc "Không" nếu không thể. Bởi vì n và m mỗi cái có thể lên tới 1000, nên cách tiếp cận O(n_m_something) ngây thơ là khả thi nếu nó duy trì ở mức O(n*m) cho mỗi thao tác bên trong, nhưng mọi thứ bậc hai ở cả hai chiều liên tục có thể sẽ quá chậm. 

Trường hợp cạnh khóa phát sinh khi ma trận so sánh mâu thuẫn. Ví dụ: nếu món 1 của ngày đầu tiên tốt hơn món 1 của ngày thứ hai nhưng cũng được đánh dấu bằng với món đó ở hàng khác hoặc nếu bất đẳng thức tạo thành một chu kỳ thì không có phép gán hợp lệ nào. Một trường hợp phức tạp khác xảy ra khi nhiều món ăn bằng nhau; Các bài tập tham lam ngây thơ có thể vô tình vi phạm các ràng buộc "=" nếu chúng ta xử lý "<" và ">" một cách độc lập mà không nhóm các món ăn bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử tất cả các phép gán số nguyên dương có thể có cho cả hai bộ đĩa và kiểm tra từng so sánh. Về nguyên tắc, điều này đúng nhưng không khả thi, vì n và m mỗi cái có thể lên tới 1000, tạo ra một không gian các khả năng quá lớn trong giới hạn thời gian 2 giây. Ngay cả việc tạo ra tất cả các chuỗi có thể có lên tới con số hợp lý lớn nhất cũng sẽ chậm về mặt thiên văn. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận ra rằng vấn đề có thể được mô hình hóa dưới dạng biểu đồ có hướng với nén đẳng thức. Mỗi món ăn tương ứng với một nút. Đối với mọi mối quan hệ ">", chúng ta thêm một cạnh từ nút nhỏ hơn đến nút lớn hơn. Đối với mỗi "<", cạnh sẽ đi theo hướng khác. Các món ăn bằng nhau được hợp nhất thành một nút duy nhất. Khi chúng ta có một đồ thị có hướng biểu thị tất cả các ràng buộc, vấn đề sẽ giảm xuống việc gán số nguyên dương nhỏ nhất cho mỗi nút sao cho tất cả các cạnh đều trỏ từ giá trị thấp hơn đến giá trị cao hơn. Điều này tương đương với việc tính toán đường đi dài nhất trong DAG sau khi nhóm các nút bằng nhau, có thể được thực hiện bằng cách sử dụng sắp xếp cấu trúc liên kết và lập trình động. 

Cách tiếp cận này tận dụng cấu trúc của vấn đề: các ràng buộc có tính bắc cầu và "=" cho phép chúng ta hợp nhất các nút, giúp đơn giản hóa biểu đồ rất nhiều. Nó làm giảm vấn đề gán hàm mũ cho vấn đề truyền tải đồ thị thời gian tuyến tính trên DAG nén. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((max_val)^(n+m)) | O(n+m) | Quá chậm | 
| Đồ thị + Sắp xếp Topo | O(n*m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu bằng cách biểu thị mỗi món ăn dưới dạng một nút và khởi tạo cấu trúc Disjoint Set Union (DSU) để hợp nhất các nút được kết nối bằng "=". Với mỗi dấu "=" trong ma trận tại (i,j), hợp nhất nút i của ngày đầu tiên với nút j của ngày thứ hai, như vậy chúng sẽ nhận được cùng một giá trị. 
2. Sau khi hợp nhất các nút bằng nhau, chúng ta lặp lại ma trận một lần nữa để xây dựng biểu đồ ràng buộc có hướng. Đối với mỗi ">" trong a_{ij}, hãy tạo một cạnh từ nút đại diện cho món j (ngày thứ hai) đến nút i (ngày đầu tiên) vì món ăn của ngày đầu tiên phải có số cao hơn. Đối với "<", tạo một cạnh từ nút i đến nút j. 
3. Kiểm tra các chu kỳ trong biểu đồ. Nếu tồn tại một chu trình thì không thể gán số một cách nhất quán nên chúng ta xuất ra "Không". Việc phát hiện chu trình có thể được thực hiện bằng cách sử dụng tìm kiếm theo chiều sâu với mảng trạng thái đã truy cập hoặc bằng cách thử sắp xếp cấu trúc liên kết. 
4. Nếu đồ thị là không tuần hoàn, hãy tính các phép gán số nguyên tối thiểu bằng cách sử dụng quy hoạch động theo thứ tự tôpô. Giá trị của mỗi nút nhiều hơn giá trị tối đa của các nút trước đó một đơn vị. Khởi tạo các nút không có cạnh nào đến 1. 
5. Ánh xạ các giá trị trở lại các món ăn ban đầu bằng cách sử dụng mảng cha DSU. Mỗi món ăn nhận được giá trị của nút đại diện của nó. 
6. Xuất "Có" theo sau là giá trị được gán cho món ăn của ngày đầu tiên và món ăn của ngày thứ hai. 

Tại sao nó hoạt động: Việc hợp nhất bình đẳng đảm bảo rằng các ràng buộc "=" luôn được thỏa mãn. Đồ thị có hướng thể hiện tất cả các bất bình đẳng nghiêm ngặt và thứ tự tôpô đảm bảo rằng giá trị của mỗi món ăn hoàn toàn lớn hơn bất kỳ món ăn nào mà nó phải lớn hơn, điều này duy trì các ràng buộc "<" và ">". Sự tối thiểu xuất phát từ việc gán 1 cho nguồn và tăng dần dọc theo các cạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

class DSU:
    def __init__(self, n):
        self.par = list(range(n))
    def find(self, x):
        if self.par[x] != x:
            self.par[x] = self.find(self.par[x])
        return self.par[x]
    def union(self, x, y):
        x = self.find(x)
        y = self.find(y)
        if x != y:
            self.par[y] = x

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    total = n + m
    dsu = DSU(total)

    # Merge equal nodes
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '=':
                dsu.union(i, n + j)

    # Build graph of inequalities
    graph = defaultdict(list)
    indeg = [0] * total
    for i in range(n):
        for j in range(m):
            u = dsu.find(i)
            v = dsu.find(n + j)
            if grid[i][j] == '<':
                if u == v:
                    print("No")
                    return
                graph[u].append(v)
                indeg[v] += 1
            elif grid[i][j] == '>':
                if u == v:
                    print("No")
                    return
                graph[v].append(u)
                indeg[u] += 1

    # Topological sort to assign minimal values
    val = [0] * total
    dq = deque([i for i in range(total) if dsu.find(i) == i and indeg[i] == 0])
    while dq:
        u = dq.popleft()
        if val[u] == 0:
            val[u] = 1
        for v in graph[u]:
            if val[v] < val[u] + 1:
                val[v] = val[u] + 1
            indeg[v] -= 1
            if indeg[v] == 0:
                dq.append(v)

    # Check if all nodes got a value
    for i in range(total):
        if val[dsu.find(i)] == 0:
            print("No")
            return

    print("Yes")
    first_day = [val[dsu.find(i)] for i in range(n)]
    second_day = [val[dsu.find(n + j)] for j in range(m)]
    print(' '.join(map(str, first_day)))
    print(' '.join(map(str, second_day)))

if __name__ == "__main__":
    solve()
```DSU hợp nhất các món ăn "=" để đáp ứng các ràng buộc bình đẳng. Các cạnh của đồ thị phản ánh sự bất bình đẳng nghiêm ngặt. Việc sử dụng sắp xếp tôpô đảm bảo chúng ta chỉ định các số nguyên tối thiểu có thể có trong khi vẫn tôn trọng tất cả các ràng buộc "<" và ">". Ánh xạ cuối cùng tới các món ăn ban đầu đảm bảo chúng tôi xuất ra các giá trị cho từng món ăn riêng lẻ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 4
>>>>
>>>>
>>>>
```Dấu vết: 

| Nút | bằng cấp | giá trị được gán | 
| --- | --- | --- | 
| 0,1,2 | 0 | 2 | 
| 3,4,5,6 | 3 | 1 | 

Tất cả các món ăn ngày đầu tiên đều cao hơn món ăn ngày thứ hai. Thuật toán gán 2 cho ngày đầu tiên và 1 cho ngày thứ hai, tối thiểu phù hợp với các ràng buộc. 

Đầu vào mẫu 2 (mâu thuẫn):```
2 2
><
<>
```Sau khi hợp nhất đẳng thức, nút 0 > nút 2, nút 0 < nút 3, nút 1 < nút 2, nút 1 > nút 3. Biểu đồ chứa một chu trình; sắp xếp cấu trúc liên kết không thành công và đầu ra là "Không". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Việc hợp nhất các nút bằng nhau và xây dựng biểu đồ yêu cầu kiểm tra từng phần tử một lần. Sắp xếp cấu trúc liên kết là O(V+E) với V ~ n+m và E ~ n*m. | 
| Không gian | O(n+m) | Lưu trữ DSU, danh sách kề của đồ thị, độ và giá trị cuối cùng. | 

Độ phức tạp này dễ dàng phù hợp với các giới hạn: n*m ≤ 10^6, do đó việc quét tuyến tính của lưới là có thể chấp nhận được. Bộ nhớ cũng dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3 4\n>>>>\n>>>>\n>>>>\n") == "Yes\n2 2 2\n1 1 1 1"
assert run("2 2\n><\n<>\n") == "No"

# Custom cases
assert run("1 1\n=") == "Yes\n1\n1",
```
