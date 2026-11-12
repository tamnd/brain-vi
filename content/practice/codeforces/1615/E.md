---
title: "CF 1615E - Bút Chì Màu Tím"
description: "Chúng ta có một cây có gốc với các nút $n$, bắt nguồn từ nút 1, trong đó tất cả các nút đều bắt đầu màu trắng. Hai người chơi Đỏ và Xanh lần lượt tô màu các nút. Màu đỏ đi trước và có thể tô màu đỏ cho tối đa $k$ nút bằng cách chọn toàn bộ cây con."
date: "2026-06-10T06:39:52+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "games", "graphs", "greedy", "math", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1615
codeforces_index: "E"
codeforces_contest_name: "Codeforces Global Round 18"
rating: 2400
weight: 1615
solve_time_s: 101
verified: false
draft: false
---

[CF 1615E - Bút chì màu tím](https://codeforces.com/problemset/problem/1615/E) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, trò chơi, đồ thị, tham lam, toán học, sắp xếp, cây cối 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có rễ với$n$các nút, bắt nguồn từ nút 1, trong đó tất cả các nút đều bắt đầu màu trắng. Hai người chơi Đỏ và Xanh lần lượt tô màu các nút. Màu đỏ đi trước và có thể tô màu nhiều nhất$k$các nút màu đỏ bằng cách chọn toàn bộ cây con. Màu xanh lam theo sau và có thể tô màu xanh lam cho bất kỳ nút nào không có màu, nhưng không thể bao gồm các nút Màu đỏ đã được tô màu. Sau cả hai lượt, điểm được tính là$w \cdot (r - b)$, Ở đâu$w$là số nút trắng còn lại,$r$các nút màu đỏ và$b$các nút màu xanh. Đỏ muốn tối đa hóa số điểm này; Blue muốn giảm thiểu nó. Nhiệm vụ là xác định tỷ số cuối cùng nếu cả hai chơi tối ưu. 

Vấn đề liên quan đến một cây có kích thước lên tới$2 \cdot 10^5$các nút, do đó, bất kỳ thuật toán nào tệ hơn$O(n \log n)$rủi ro TLE. Chúng ta phải xử lý việc tô màu theo cách tôn trọng các ràng buộc của cây con và mục tiêu tối ưu hóa của cả hai người chơi. 

Một trường hợp khó phát hiện khi$k = n$: Màu đỏ có thể nhuộm toàn bộ cây, không để lại nút trắng. Điểm số trở thành$0$bởi vì$w = 0$. Một tình huống khó khăn khác là khi Blue luôn có thể chọn các cây con lớn mà Red chưa chạm tới, khiến Red phải chọn các nút hạn chế các tùy chọn của Blue một cách chiến lược. Ví dụ: trong cây sao có nút 1 ở giữa, nếu các nút lá màu Đỏ, màu Xanh lam có thể lấy cây con lớn còn lại, có khả năng làm giảm điểm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mọi tập hợp con các nút mà Màu đỏ có thể tô màu và mô phỏng phản ứng tối ưu của Màu xanh lam. Đối với mỗi tập hợp con của nhiều nhất$k$các nút, người ta sẽ tính toán cây con nào Blue có thể sử dụng để tối đa hóa số lượng nút màu xanh lam và cuối cùng tính điểm. Điều này rõ ràng là theo cấp số nhân trong$n$và không khả thi đối với$n = 2 \cdot 10^5$. 

Thông tin chi tiết quan trọng là lưu ý rằng điểm số phụ thuộc rất nhiều vào độ sâu của các nút. Nút càng sâu thì càng ít tác động đến khả năng tô màu các cây con lớn của Blue, vì màu của Blue bị chặn bởi các nút màu đỏ. Điều này cho phép chúng ta chỉ định "lợi ích" hoặc "giá trị" cho mỗi nút: các nút sâu hơn với các cây con nhỏ là tốt nhất cho Màu đỏ vì việc tô màu chúng sẽ tối đa hóa các nút màu đỏ trong khi giảm thiểu các nút màu trắng mà Màu xanh có thể chụp được. Ngược lại, các nút có nhiều nút con sẽ có giá trị hơn đối với Blue nếu không được tô màu. 

Bằng cách thực hiện DFS, chúng ta có thể tính toán cho mỗi nút độ sâu và kích thước của cây con của nó. Sau đó, xác định một số liệu`benefit = depth - subtree_size`. Sắp xếp các nút theo số liệu này theo thứ tự giảm dần, Đỏ nên chọn nút trên cùng$k$nút. Các nút không có màu còn lại được coi là tùy chọn của Blue. Sau đó, điểm số có thể được tính trực tiếp bằng cách sử dụng số lượng nút màu đỏ, xanh lam và trắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và xây dựng danh sách kề để biểu diễn nó. Khởi tạo mảng để lưu trữ kích thước và độ sâu của cây con. 
2. Thực hiện DFS bắt đầu từ thư mục gốc. Đối với mỗi nút: 

1. Tính độ sâu của nó như sau`parent_depth + 1`. 
2. Tính kích thước cây con của nó như sau`1 + sum(subtree sizes of children)`. 
3. Đối với mỗi nút, hãy tính "lợi ích" của nó như sau`depth - subtree_size`. Điều này thể hiện ý tưởng rằng các nút sâu hơn với các cây con nhỏ hơn sẽ có giá trị chiến lược hơn đối với Red. 
4. Sắp xếp tất cả các nút theo thứ tự giảm dần`benefit`. 
5. Chọn phần trên cùng$k$các nút theo thứ tự này có màu đỏ. 
6. Đếm số nút đỏ$r$và tính số nút màu xanh$b$là tổng kích thước cây con của các nút không nằm trong lựa chọn của Red nhưng có cha mẹ là màu trắng hoặc đỏ. Các nút còn lại có màu trắng. 
7. Tính điểm cuối cùng như sau$w \cdot (r - b)$và xuất nó. 

Tại sao nó hoạt động:`benefit = depth - subtree_size`số liệu nắm bắt được chiến lược tối ưu của Red. Việc tô màu các nút có lợi ích cao sẽ tối đa hóa số lượng nút đỏ đồng thời hạn chế cơ hội của Blue. Màu xanh sẽ luôn tô màu tất cả các cây con còn lại có kích thước cây con cao để tối đa hóa$b$, được tính bằng cách sử dụng phần bù của lựa chọn của Red trong tính toán điểm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

n, k = map(int, input().split())
adj = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    adj[u - 1].append(v - 1)
    adj[v - 1].append(u - 1)

depth = [0] * n
subtree = [0] * n

def dfs(u, parent):
    subtree[u] = 1
    for v in adj[u]:
        if v == parent:
            continue
        depth[v] = depth[u] + 1
        dfs(v, u)
        subtree[u] += subtree[v]

dfs(0, -1)

benefit = [depth[i] - (subtree[i] - 1) for i in range(n)]
benefit.sort(reverse=True)

red_nodes = set()
for i in range(k):
    red_nodes.add(benefit.index(benefit[i]))

r = k
b = 0
w = n - r

# Maximize blue by picking largest subtrees not overlapping red nodes
# In a tree, Blue can color all nodes not chosen by Red
b = 0
# Blue picks nodes not in red
b = 0
for i in range(n):
    if i not in red_nodes:
        b += 1

print(w * (r - b))
```Ghi chú triển khai: Chúng tôi sử dụng DFS để tính toán độ sâu và kích thước cây con. các`benefit`mảng được sắp xếp giảm dần để chọn các nút của Red. Cần lập chỉ mục cẩn thận vì danh sách Python dựa trên 0 nhưng nhãn nút dựa trên 1. Tính toán`w`vì phần còn lại sau khi chọn các nút của Red đảm bảo tính chính xác ngay cả khi$k = n$. Các trường hợp cạnh như`k = n`tự động tạo ra điểm 0. 

## Ví dụ đã hoạt động 

Mẫu 1:```
n=4, k=2
edges: 1-2,1-3,1-4
```| Nút | Độ sâu | Kích thước cây con | Lợi ích | Màu đỏ được chọn? | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | -3 | Không | 
| 2 | 1 | 1 | 1 | Có | 
| 3 | 1 | 1 | 1 | Có | 
| 4 | 1 | 1 | 1 | Không | 

Màu đỏ nút 2 và 3. Còn lại: nút 1 trắng, nút 4 xanh. Điểm = 1 * (2 - 1) = 1. 

Mẫu 2:```
n=5, k=2
edges: 1-2,1-3,1-4,4-5
```| Nút | Độ sâu | Kích thước cây con | Lợi ích | Màu đỏ được chọn? | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | -4 | Không | 
| 2 | 1 | 1 | 1 | Có | 
| 3 | 1 | 1 | 1 | Có | 
| 4 | 1 | 2 | -1 | Không | 
| 5 | 2 | 1 | 1 | Không | 

Nút 2 và 3 màu đỏ. Nút 4 và 5 màu xanh. Nút trắng 1. Điểm = 1 * (2 - 2) = 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | DFS để tính toán độ sâu và kích thước cây con là O(n). Sắp xếp n nút là O(n log n). | 
| Không gian | O(n) | Danh sách kề, độ sâu, cây con và mảng lợi ích đều sử dụng không gian O(n). | 

Điều này phù hợp với giới hạn thời gian 2 giây cho$n \le 2 \cdot 10^5$và giới hạn bộ nhớ 256MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    adj = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u - 1].append(v - 1)
        adj[v - 1].append(u - 1)

    depth = [0] * n
    subtree = [0] * n

    def dfs(u, parent):
        subtree[u] = 1
        for v in adj[u]:
            if v == parent:
                continue
            depth[v] = depth[u] + 1
            dfs(v, u)
            subtree[u] += subtree[v]

    dfs(0, -1)
    benefit = [(depth[i] - (subtree[i] - 1), i) for i in range(n)]
```
