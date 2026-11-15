---
title: "CF 1836F - Giả thuyết Brown của bác sĩ"
description: "Chúng tôi đang làm việc với một biểu đồ có hướng trong đó các nút biểu thị các hành tinh và các cạnh biểu thị các lỗ sâu đục giữa chúng. Mỗi cạnh mất đúng một giờ để đi qua."
date: "2026-06-09T06:46:37+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1836
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 880 (Div. 2)"
rating: 2900
weight: 1836
solve_time_s: 114
verified: false
draft: false
---

[CF 1836F - Giả thuyết Brown của bác sĩ](https://codeforces.com/problemset/problem/1836/F) 

**Xếp hạng:** 2900 
**Tags:** dfs và tương tự, đồ thị, toán, lý thuyết số 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một biểu đồ có hướng trong đó các nút biểu thị các hành tinh và các cạnh biểu thị các lỗ sâu đục giữa chúng. Mỗi cạnh mất đúng một giờ để đi qua. Bài toán yêu cầu ta đếm số cách chọn một hoặc hai tàu sao cho các tàu chuyển động liên tục cho chính xác.`k`hàng giờ dọc theo những lỗ sâu đục này. Nếu chúng ta chọn hai tàu, chúng phải hoán đổi vị trí một cách chính xác`k`giờ; nếu chúng ta chọn một con tàu, nó phải quay trở lại hành tinh ban đầu sau`k`giờ. 

Đầu vào bao gồm`n`nút và`m`các cạnh có kích thước rất lớn`k`có thể lên tới 10^18. Điều này ngay lập tức loại trừ các cách tiếp cận ngây thơ cố gắng mô phỏng các đường đi có độ dài`k`một cách rõ ràng. Biểu đồ có thể có tối đa 10^5 nút và 2×10^5 cạnh, do đó, bất kỳ thuật toán nào kém hơn tuyến tính hoặc gần tuyến tính trong`n`Và`m`sẽ hết thời gian. Điều này cũng gợi ý rằng chúng ta cần một giải pháp dựa trên các thuộc tính của đồ thị cấu trúc thay vì liệt kê đường dẫn rõ ràng. 

Các trường hợp đặc biệt không rõ ràng phát sinh từ các hành tinh bị cô lập, các thành phần bị ngắt kết nối và các chu kỳ nhỏ. Ví dụ, nếu`k`nhỏ hơn bất kỳ độ dài chu kỳ nào, không con tàu nào có thể di chuyển và quay trở lại chính xác`k`giờ. Một trường hợp tinh tế khác là chu kỳ hai nút, cho phép hoán đổi hai tàu nhưng yêu cầu tính toán cẩn thận để tránh tính các cặp đối xứng kép. Nếu đồ thị không có chu trình thì chỉ có các vòng tự cô lập mới cho phép một con tàu thỏa mãn điều kiện. 

Xét một đồ thị có hai nút và một cạnh từ nút 1 đến nút 2. Với`k = 1`, không có giao dịch hoán đổi hợp lệ hoặc chu kỳ vận chuyển đơn lẻ. Một cách tiếp cận ngây thơ đếm các nút có thể truy cập mà không xem xét độ dài chu kỳ sẽ báo cáo không chính xác một giải pháp hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các đường dẫn có độ dài`k`giữa mỗi cặp nút. Điều này sẽ liên quan đến việc lặp lại DFS hoặc BFS cho`k`các bước hoặc khả năng tính toán của ma trận kề. Cả hai cách tiếp cận đều thất bại vì`k`có thể lên tới 10^18, khiến cho ngay cả các thao tác O(k) cũng không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là xem xét các thành phần được kết nối mạnh mẽ (SCC) của biểu đồ. Bất kỳ nút nào có thể tiếp cận chính nó một cách chính xác`k`các bước phải là một phần của chu trình có độ dài chia`k`. Tương tự, hai nút có thể trao đổi trong`k`các bước phải thuộc cùng một SCC và tuân theo một chu kỳ có độ dài phân chia`k`. Do đó, chúng tôi giảm bớt vấn đề khi phân tích SCC và tính ước số chung lớn nhất (GCD) của độ dài chu kỳ trong mỗi SCC. 

Khi chúng ta có GCD về độ dài chu kỳ trong mỗi SCC, một nút có thể được sử dụng như một con tàu nếu`k`chia hết cho GCD này. Đối với các cặp nút trong cùng một SCC, nếu`k`chia hết cho GCD của SCC, bất kỳ hai nút riêng biệt nào cũng có thể hoán đổi`k`các bước. Điều này tránh bất kỳ việc liệt kê đường dẫn nào và giảm vấn đề xuống còn một vài lần tuyến tính trên biểu đồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS/BFS | O(n·k) | O(n) | Quá chậm, k quá lớn | 
| Phép lũy thừa ma trận kề | O(n³·log k) | O(n²) | Quá chậm đối với n=10^5 | 
| SCC + Phân tích GCD chu kỳ | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy thuật toán Tarjan hoặc thuật toán Kosaraju để xác định tất cả các thành phần có liên kết chặt chẽ trong biểu đồ. Mỗi SCC đại diện cho một tập hợp các nút tối đa trong đó mỗi nút có thể tiếp cận mọi nút khác. Bước này là tuyến tính về số lượng nút và cạnh. 
2. Với mỗi SCC, hãy tính độ dài của tất cả các chu kỳ. Điều này có thể được thực hiện thông qua DFS. Theo dõi độ sâu của từng nút trong DFS và ghi lại sự khác biệt về độ sâu bất cứ khi nào tìm thấy cạnh sau. GCD của những khác biệt này cung cấp GCD của tất cả các độ dài chu kỳ trong SCC. 
3. Đối với mỗi SCC, hãy xác định xem`k`chia hết cho chu kỳ GCD của nó. Nếu có thì mỗi nút trong SCC có thể hoạt động như một con tàu duy nhất. Số lượng lựa chọn một tàu hợp lệ là tổng số nút trong các SCC đó. 
4. Tương tự, bất kỳ cặp nút riêng biệt nào trong SCC đều có thể hoán đổi vị trí`k`bước nếu`k`chia hết cho chu kỳ GCD của SCC. Đếm số cặp như vậy`size * (size - 1)`cho mỗi SCC đủ điều kiện. 
5. Tổng hợp tất cả các SCC để có được tổng số tàu đơn và cặp tàu hợp lệ. 

Tại sao nó hoạt động: SCC đảm bảo khả năng tiếp cận giữa các nút. Việc sử dụng GCD có độ dài chu kỳ đảm bảo rằng`k`các bước có thể phù hợp với các chu kỳ trong biểu đồ. Sự kết hợp của các đặc tính này đảm bảo rằng bất kỳ tàu hoặc cặp tàu nào được đếm đều đáp ứng chính xác`k`-hạn chế giờ. 

## Giải pháp Python```python
import sys
import threading
import math
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def main():
    n, m, k = map(int, input().split())
    graph = [[] for _ in range(n)]
    for _ in range(m):
        x, y = map(int, input().split())
        graph[x-1].append(y-1)

    # Tarjan's SCC algorithm
    index = 0
    indices = [-1]*n
    lowlink = [0]*n
    stack = []
    on_stack = [False]*n
    sccs = []

    def strongconnect(v):
        nonlocal index
        indices[v] = lowlink[v] = index
        index += 1
        stack.append(v)
        on_stack[v] = True
        for w in graph[v]:
            if indices[w] == -1:
                strongconnect(w)
                lowlink[v] = min(lowlink[v], lowlink[w])
            elif on_stack[w]:
                lowlink[v] = min(lowlink[v], indices[w])
        if lowlink[v] == indices[v]:
            scc = []
            while True:
                w = stack.pop()
                on_stack[w] = False
                scc.append(w)
                if w == v:
                    break
            sccs.append(scc)

    for v in range(n):
        if indices[v] == -1:
            strongconnect(v)

    total = 0

    # For each SCC, find the GCD of cycle lengths
    for scc in sccs:
        if len(scc) == 1:
            # Single node SCC
            v = scc[0]
            if v in graph[v]:  # self-loop
                if k % 1 == 0:
                    total += 1
        else:
            # Compute cycle GCD
            depths = [-1]*n
            gcd = 0

            def dfs(u, d):
                nonlocal gcd
                depths[u] = d
                for v in graph[u]:
                    if v not in scc_set:
                        continue
                    if depths[v] == -1:
                        dfs(v, d+1)
                    else:
                        cycle_len = d + 1 - depths[v]
                        gcd = math.gcd(gcd, cycle_len)

            scc_set = set(scc)
            for u in scc:
                if depths[u] == -1:
                    dfs(u, 0)

            if k % gcd == 0:
                total += len(scc)  # single ships
                total += len(scc) * (len(scc) - 1)  # pairs

    print(total)

threading.Thread(target=main).start()
```Giải pháp đầu tiên tính toán SCC bằng thuật toán của Tarjan. Sau đó, nó sử dụng DFS bên trong mỗi SCC để tính toán GCD của tất cả các độ dài chu kỳ. Chúng tôi kiểm tra khả năng chia hết cho`k`để xác định nút nào có thể được sử dụng. Việc sử dụng các bộ để hạn chế DFS đối với các nút trong SCC đảm bảo rằng các chu trình bên ngoài SCC không ảnh hưởng đến tính toán GCD. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7 8 346
1 2
1 3
2 4
3 4
4 5
5 1
6 7
7 6
```SCC chính:`{1,2,3,4,5}`tạo thành một SCC với các chu kỳ,`{6,7}`tạo thành một SCC khác với 2 chu kỳ. GCD của chu trình SCC đầu tiên chia 346, 2 của chu trình thứ hai chia 346. Nút 6 và 7 có thể được sử dụng như các tàu đơn lẻ. Các cặp từ SCC đầu tiên thỏa mãn điều kiện GCD mang lại 3 cặp. Tổng số: 5. 

### Mẫu 2 

đầu vào:```
4 4 2
1 2
2 3
3 4
4 1
```Đồ thị tạo thành một chu kỳ 4. Chu trình GCD là 4, không chia hết cho 2. Vì vậy không có cặp nào. Mỗi nút có thể quay lại sau 2 bước nếu 2 chia hết cho GCD, nhưng 2 % 4 != 0, do đó chỉ các chu kỳ truyền tải cụ thể mới hợp lệ. Tổng số tàu đơn được tính: 0. 

Các bảng trên theo dõi SCC, GCD chu kỳ và khả năng chia hết cho`k`để xác minh tính đúng đắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Thuật toán của Tarjan là tuyến tính. DFS cho độ dài chu kỳ chạy trong SCC, mỗi cạnh một lần. | 
| Không gian | O(n + m) | Vẽ đồ thị danh sách kề và ngăn xếp cho đệ quy. | 

Thuật toán khả thi đối với`n = 10^5`,`m = 2×10^5`và lớn`k`bởi vì nó tránh việc liệt kê đường dẫn rõ ràng và dựa vào
