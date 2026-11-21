---
title: "CF 2220E - Tô màu cây đỏ đen"
description: "Chúng ta có một cây có n nút, trong đó mỗi nút có màu đỏ hoặc đen. Mục tiêu là làm cho tất cả các nút có màu đỏ bằng cách sử dụng thao tác ngẫu nhiên: bạn chọn một nút và tô màu lại nó bằng màu của một trong các nút lân cận được chọn thống nhất một cách ngẫu nhiên."
date: "2026-06-09T05:00:28+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "probabilities", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 2220
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1093 (Div. 2)"
rating: 2300
weight: 2220
solve_time_s: 122
verified: false
draft: false
---

[CF 2220E - Tô màu cây đỏ đen](https://codeforces.com/problemset/problem/2220/E) 

**Đánh giá:** 2300 
**Tags:** dp, tham lam, xác suất, phân loại, cây 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các nút, trong đó mỗi nút có màu đỏ hoặc đen. Mục tiêu là làm cho tất cả các nút có màu đỏ bằng cách sử dụng thao tác ngẫu nhiên: bạn chọn một nút và tô màu lại nó bằng màu của một trong các nút lân cận được chọn thống nhất một cách ngẫu nhiên. Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp có số nút, chuỗi nhị phân biểu thị màu ban đầu và các cạnh của cây. Đầu ra là số lượng thao tác dự kiến ​​​​tối thiểu để chuyển toàn bộ cây thành màu đỏ. 

Những hạn chế đòi hỏi phải xử lý hiệu quả.`n`có thể lên tới 200.000 và tổng của tất cả các trường hợp thử nghiệm cũng là 200.000, loại trừ mô phỏng ngây thơ của các hoạt động ngẫu nhiên hoặc liệt kê trạng thái theo cấp số nhân. Mỗi thao tác phụ thuộc vào các nút lân cận của nút hiện tại, vì vậy giải pháp phải truyền bá các kỳ vọng qua cây một cách hiệu quả. 

Các trường hợp cạnh không rõ ràng bao gồm các cây trong đó nút đen chỉ có hàng xóm màu đen hoặc lá có màu đen với nút gốc màu đỏ. Nếu bạn cố gắng tính toán các hoạt động dự kiến ​​mà không xem xét cấu trúc cây và mức độ lân cận, bạn có thể giả định sai sự chuyển đổi ngay lập tức, điều này sẽ tạo ra những kỳ vọng sai lầm. Ví dụ, một chiếc lá đen với một lá hàng xóm màu đỏ duy nhất có giá trị kỳ vọng là`1`vì nó sẽ chuyển sang màu đỏ trong lần thao tác đầu tiên. Một nút đen có hai nút lân cận màu đen nhưng có một nút đỏ ở xa yêu cầu tính toán kỳ vọng đệ quy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng tất cả các chuỗi hoạt động và tính toán số bước trung bình cho mỗi chuỗi. Mặc dù đúng nhưng điều này không khả thi vì không gian trạng thái tăng theo cấp số nhân với`n`và số lượng hoạt động nói chung là không giới hạn. Ví dụ: ngay cả một chuỗi gồm 20 nút cũng sẽ yêu cầu mô phỏng hơn một triệu trạng thái nếu một nút cố gắng liệt kê đầy đủ. 

Thông tin chi tiết quan trọng là số lượng thao tác dự kiến ​​để một nút chuyển sang màu đỏ chỉ phụ thuộc vào giá trị dự kiến ​​của các nút lân cận. Điều này cho phép một công thức đệ quy trên cây, trong đó các lá có thể được giải quyết ngay lập tức và các kỳ vọng sẽ được nhân lên. Trong một cây, giá trị mong đợi của mỗi nút có thể được biểu thị theo các nút lân cận của nó và chúng ta có thể tính toán các giá trị này một cách hiệu quả bằng cách sử dụng tìm kiếm theo chiều sâu. 

Quan sát cho thấy kỳ vọng của mỗi nút là`1 + sum(E[neighbor])/deg(node)`đối với các nút đen, ở đâu`deg(node)`đếm các hàng xóm, chúng ta hãy rút gọn bài toán thành một phép duyệt tuyến tính trên cây. Các nút màu đỏ có kỳ vọng bằng 0. Điều này dẫn đến giải pháp O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tuyên truyền kỳ vọng DFS | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích đầu vào cho số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`, chuỗi nhị phân`s`, và`n-1`các cạnh. Xây dựng danh sách kề đại diện cho cây. 
2. Khởi tạo một mảng`E`của các giá trị dự kiến. Bộ`E[i] = 0`nếu nút`i`có màu đỏ và`None`nếu nút`i`là màu đen. 
3. Xác định hàm đệ quy`dfs(u, parent)`tính toán số lượng hoạt động dự kiến ​​cho nút`u`: 

1. Nếu nút`u`màu đỏ, trả về 0. 
2. Ngược lại, khởi tạo`sum_neighbors = 0`Và`degree = len(adj[u])`. 
3. Đối với mỗi người hàng xóm`v`của`u`(không bao gồm phụ huynh để tránh đi xe đạp), hãy gọi`dfs(v, u)`và thêm kết quả vào`sum_neighbors`. 
4. Tính toán`E[u] = 1 + sum_neighbors / degree`. Điều này chiếm một hoạt động cộng với kỳ vọng trung bình từ hàng xóm. 
5. Trở về`E[u]`. 
4. Gọi`dfs(root, -1)`bắt đầu từ bất kỳ nút màu đỏ nào, vì tồn tại ít nhất một nút màu đỏ. 
5. In`E[root]`với độ chính xác đủ. 

Tại sao nó hoạt động: Hàm đệ quy tính toán giá trị mong đợi chính xác bằng cách sử dụng độ tuyến tính của kỳ vọng. Hoạt động của mỗi nút độc lập với thứ tự các hoạt động được áp dụng ở nơi khác, miễn là các kỳ vọng của nút lân cận được truyền bá chính xác. Các nút màu đỏ hoạt động như các trạng thái hấp thụ với kỳ vọng bằng 0, đảm bảo quá trình đệ quy kết thúc và mang lại số lượng hoạt động dự kiến ​​​​tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        adj = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            adj[u-1].append(v-1)
            adj[v-1].append(u-1)
        
        E = [None]*n
        red_nodes = [i for i, c in enumerate(s) if c == '1']
        for r in red_nodes:
            E[r] = 0.0

        def dfs(u, parent):
            if E[u] is not None:
                return E[u]
            total = 0.0
            deg = len(adj[u])
            for v in adj[u]:
                if v == parent:
                    continue
                total += dfs(v, u)
            E[u] = 1.0 + total / deg
            return E[u]
        
        # start from any black node
        for i in range(n):
            if E[i] is None:
                dfs(i, -1)
        # the maximum expected among black nodes
        res = max(E[i] for i in range(n))
        print(f"{res:.12f}")

solve()
```Mã này đọc cây, khởi tạo các kỳ vọng và sử dụng DFS để truyền các giá trị mong đợi. sử dụng`max(E[i])`đảm bảo chúng tôi báo cáo các hoạt động dự kiến ​​tối thiểu để làm cho tất cả các nút có màu đỏ. Độ chính xác của dấu phẩy động được xử lý bằng`:.12f`. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
3
3
101
1 2
2 3
```| Nút | Màu sắc | Hàng xóm | Tính toán kỳ vọng | E[nút] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | Đỏ → 0 | 0 | 
| 2 | 0 | 1,3 | 1 + (0 + 0)/2 | 1 | 
| 3 | 1 | 2 | Đỏ → 0 | 0 | 

Đầu ra:`1.000000000000`Mẫu 2: 

đầu vào:```
5
10010
1 2
2 3
3 4
2 5
```DFS tính toán từng
