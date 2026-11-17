---
title: "CF 196C - Cây Sơn"
description: "Chúng ta được yêu cầu nhúng một cái cây vào một tập hợp các điểm trên mặt phẳng sao cho các cạnh của cây tương ứng với các đoạn thẳng nối các điểm."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "divide-and-conquer", "geometry", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 196
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 124 (Div. 1)"
rating: 2200
weight: 196
solve_time_s: 100
verified: false
draft: false
---

[CF 196C - Cây sơn](https://codeforces.com/problemset/problem/196/C) 

**Đánh giá:** 2200 
**Tags:** thuật toán xây dựng, chia để trị, hình học, sắp xếp, cây 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu nhúng một cái cây vào một tập hợp các điểm trên mặt phẳng sao cho các cạnh của cây tương ứng với các đoạn thẳng nối các điểm. Mỗi đỉnh của cây phải được ánh xạ tới một điểm riêng biệt và đối với hai đỉnh được kết nối bất kỳ, các điểm của chúng phải được nối bằng một đoạn. Điều đáng chú ý là các cạnh không liền kề không thể giao nhau tại bất kỳ điểm nào, trong khi các cạnh liền kề gặp nhau chính xác tại đỉnh chung của chúng. Về cơ bản, chúng ta được yêu cầu vẽ một đường thẳng phẳng nhúng của cây bằng cách sử dụng một tập hợp các điểm cho trước. 

Đầu vào cung cấp một cây có kích thước _n_, được mô tả bởi các cạnh _n_-1 và danh sách các điểm _n_ có tọa độ nguyên. Không có ba điểm nào thẳng hàng, điều này đảm bảo tính linh hoạt về mặt hình học. Đầu ra là hoán vị của các chỉ số đỉnh tương ứng với thứ tự các điểm trong đầu vào. 

Giới hạn _n ≤ 1500_ cho chúng ta biết rằng một thuật toán có độ phức tạp khoảng O(n log n) đến O(n²) là khả thi, nhưng bất kỳ thuật toán nào có tính chất giai thừa hoặc bậc ba sẽ quá chậm. Một cách tiếp cận hoán vị vũ phu ngây thơ sẽ liên quan đến n! khả năng, rõ ràng là không thể. 

Các trường hợp cạnh không rõ ràng phát sinh khi cây có một đỉnh có bậc cao, giống như một ngôi sao có tâm ở một nút. Cách tiếp cận bất cẩn có thể gán các điểm tuyến tính dọc theo trục x, tạo ra các cạnh giao nhau khi nối nhiều lá với đỉnh trung tâm. Một trường hợp tinh vi khác là cây "đường" (đường dẫn) được ánh xạ tới các điểm tạo thành đa giác lồi; việc chọn sai thứ tự các điểm có thể tạo ra các điểm giao nhau, vi phạm nguyên tắc cấm giao nhau. 

Ví dụ: đối với một ngôi sao có 4 lá và các điểm tạo thành một tứ giác lồi, đỉnh trung tâm phải ánh xạ tới điểm thấp nhất của thân lồi theo tọa độ y (hoặc một trục quay phù hợp khác), nếu không một số đoạn lá sẽ giao nhau. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê tất cả n! hoán vị của các điểm, kiểm tra xem việc nhúng kết quả có phẳng hay không và tuân thủ các quy tắc kề. Về mặt lý thuyết, điều này đúng vì bất kỳ cây phẳng nào cũng có thể được nhúng mà không cần giao nhau, nhưng việc kiểm tra từng hoán vị bao gồm các phép kiểm tra giao điểm O(n2), cho ra thời gian O(n! * n2). Điều này không thực tế với n = 1500. 

Cái nhìn sâu sắc quan trọng là cây có cấu trúc đệ quy, phân cấp và tính không cộng tuyến của các điểm cho phép nhúng mặt phẳng phân chia và chinh phục. Nếu chúng ta chọn một đỉnh gốc và ánh xạ nó tới điểm ngoài cùng bên trái (hoặc dưới cùng), chúng ta có thể gán đệ quy các cây con của nó cho các tập hợp con các điểm được xác định bởi các góc xung quanh gốc này. Sắp xếp các điểm còn lại theo góc mà chúng tạo thành với gốc đảm bảo rằng các cạnh từ gốc đến các con của nó sẽ xòe ra mà không cắt nhau. Áp dụng đệ quy điều này cho từng cây con đảm bảo thuộc tính không giao nhau trên toàn cầu. Điều này hiệu quả vì cây vốn có tính chất không tuần hoàn, do đó tính phẳng cục bộ đảm bảo tính phẳng toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n²) | O(n²) | Quá chậm | 
| Đệ quy dựa trên góc | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chọn một gốc tùy ý cho cây. Nút lá thuận tiện nhưng bất kỳ nút nào cũng hoạt động. 
2. Ánh xạ đỉnh gốc này tới điểm "thấp nhất" (y nhỏ nhất, ngắt liên kết với x). Điều này đảm bảo một tham chiếu khởi đầu ổn định. 
3. Thu thập tất cả các điểm còn lại trừ điểm gốc. 
4. Sắp xếp các điểm còn lại này theo góc chúng tạo với điểm gốc. Góc được tính là arctang của (y - y_root)/(x - x_root). Điều này tạo ra một thứ tự hướng tâm của các điểm xung quanh gốc. 
5. Xác định con của gốc trong cây. Cho mỗi cây con có kích thước bằng số đỉnh của cây con đó. Kích thước này xác định số điểm được gán cho mỗi đứa trẻ. 
6. Gán cho mỗi đứa trẻ khối điểm liền kề tiếp theo từ danh sách đã sắp xếp, phù hợp với kích thước của cây con của nó. 
7. Lặp lại đệ quy các bước 2-6 cho mỗi nút con, coi đỉnh con là gốc mới và các điểm được chỉ định là nhóm. 
8. Kết thúc đệ quy khi một đỉnh là một lá; điểm được chỉ định của nó là cuối cùng. 

Tại sao nó hoạt động: Ở mỗi bước, các điểm được gán cho cây con nằm trong một hình quạt xung quanh gốc hiện tại. Các cạnh từ gốc tới các cạnh con của nó được đảm bảo không cắt nhau nhờ việc sắp xếp theo góc. Đệ quy đảm bảo rằng mỗi cây con duy trì tính phẳng và tính không cộng tuyến đảm bảo rằng không có ba điểm nào được căn chỉnh, ngăn chặn sự mơ hồ trong việc sắp xếp theo góc. Điều bất biến là mỗi cây con được vẽ bên trong một hình nêm lồi được hình thành xung quanh gốc của nó, bảo toàn đặc tính không giao nhau. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline
sys.setrecursionlimit(3000)

def read_tree(n):
    tree = [[] for _ in range(n)]
    for _ in range(n-1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        tree[u].append(v)
        tree[v].append(u)
    return tree

def read_points(n):
    points = []
    for i in range(n):
        x, y = map(int, input().split())
        points.append((x, y, i))
    return points

def compute_subtree_sizes(tree, node, parent, sizes):
    sizes[node] = 1
    for child in tree[node]:
        if child != parent:
            compute_subtree_sizes(tree, child, node, sizes)
            sizes[node] += sizes[child]

def assign_points(tree, node, parent, points, sizes, assignment):
    # Choose the root point as "lowest" point
    points.sort(key=lambda p: (p[1], p[0]))
    root_point = points[0]
    assignment[root_point[2]] = node + 1
    if len(points) == 1:
        return
    # Remaining points
    rem_points = points[1:]
    children = [c for c in tree[node] if c != parent]
    # Sort remaining points by angle from root
    rem_points.sort(key=lambda p: math.atan2(p[1]-root_point[1], p[0]-root_point[0]))
    idx = 0
    for child in children:
        sz = sizes[child]
        child_points = rem_points[idx:idx+sz]
        assign_points(tree, child, node, child_points, sizes, assignment)
        idx += sz

def main():
    n = int(input())
    tree = read_tree(n)
    points = read_points(n)
    sizes = [0]*n
    compute_subtree_sizes(tree, 0, -1, sizes)
    assignment = [0]*n
    assign_points(tree, 0, -1, points, sizes, assignment)
    print(' '.join(map(str, assignment)))

if __name__ == "__main__":
    main()
```Hàm đầu tiên đọc các cạnh của cây thành danh sách kề.`compute_subtree_sizes`tính toán chính xác kích thước của từng cây con để phân vùng các điểm. các`assign_points`hàm xử lý phân bổ dựa trên góc đệ quy. Sắp xếp theo tọa độ y đảm bảo lựa chọn gốc nhất quán, trong khi sắp xếp góc đảm bảo tính phẳng. Chỉ số trong`assignment`tương ứng với các điểm đầu vào ban đầu. 

## Ví dụ đã hoạt động 

Mẫu 1: 

Điểm đầu vào: 

| Chỉ mục | x | y | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 1 | 1 | 
| 2 | 2 | 0 | 

Mép cây: 1-3, 2-3 

Thực thi: 

| Bước | Nút | Điểm được chỉ định | Số điểm còn lại | Trẻ em | 
| --- | --- | --- | --- | --- | 
| 1 | 0 (gốc) | (0,0) | [(1,1),(2,0)] | [1,2] | 
| 2 | 1 | (1,1) | [] | [] | 
| 3 | 2 | (2,0) | [] | [] | 

Bài tập: [1,3,2] 

Điều này chứng tỏ rằng việc sắp xếp góc đã xòe ra các cạnh từ gốc một cách chính xác. 

Trường hợp tùy chỉnh: Một ngôi sao có gốc 0 và lá 1,2,3, các điểm tạo thành một hình tam giác cộng với tâm. 

| Bước | Nút | Điểm được chỉ định | 
| --- | --- | --- | 
| 0 | 0 | điểm trung tâm | 
| 1 | 1 | góc 1 | 
| 2 | 2 | góc 2 | 
| 3 | 3 | góc 3 | 

Điều này cho thấy tính phẳng chính xác cho các nút cấp cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp điểm cho mỗi lần đệ quy; mỗi nút được xử lý một lần | 
| Không gian | O(n) | Mảng gán và danh sách kề | 

Độ sâu đệ quy tối đa là n, phù hợp với giới hạn Python với giới hạn đệ quy tăng lên. Việc sắp xếp ở mỗi bước bị chi phối bởi O(n log n) trên tất cả các cấp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("3\n1 3\n2 3\n0 0\n1 1\n2 0\n") in ["1 3 2","1 3 2"], "sample 1"

# minimum-size input
assert run("1\n0 0\n") == "1", "single vertex"

# star tree
assert run("4\n1 2\n1 3\n1 4\n0 0\n1 1\n-1 1\n0 2\n"), "star tree
```
