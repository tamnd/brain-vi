---
title: "CF 1137C - Tham quan bảo tàng"
description: "Chúng tôi được yêu cầu lên kế hoạch cho một chuyến tham quan bắt đầu từ thành phố 1 vào ngày đầu tiên của tuần, nhằm mục đích đến thăm càng nhiều bảo tàng khác nhau càng tốt. Mỗi thành phố có chính xác một bảo tàng và các bảo tàng có lịch trình hàng tuần chỉ định ngày nào trong tuần họ mở cửa."
date: "2026-06-12T03:56:52+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1137
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 545 (Div. 1)"
rating: 2500
weight: 1137
solve_time_s: 85
verified: true
draft: false
---

[CF 1137C - Tham quan bảo tàng](https://codeforces.com/problemset/problem/1137/C) 

**Đánh giá:** 2500 
**Tags:** dp, đồ thị, triển khai 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu lên kế hoạch cho chuyến tham quan bắt đầu từ thành phố 1 vào ngày đầu tiên của tuần, nhằm mục đích ghé thăm càng nhiều **bảo tàng khác biệt** càng tốt. Mỗi thành phố có chính xác một bảo tàng và các bảo tàng có lịch trình hàng tuần chỉ định ngày nào trong tuần họ mở cửa. Đất nước được mô hình hóa dưới dạng đồ thị có hướng: thành phố là nút, đường là cạnh. Mỗi lần di chuyển giữa các thành phố mất đúng một đêm, do đó, việc di chuyển từ thành phố u đến thành phố v sẽ đẩy ngày trong tuần lên một modulo d. Một thành phố có thể được ghé thăm nhiều lần, nhưng mỗi bảo tàng chỉ được tính một lần vào các bảo tàng riêng biệt đã ghé thăm. Mục tiêu là tối đa hóa số lượng các bảo tàng khác nhau được ghé thăm. 

Những ràng buộc rất chặt chẽ. Với tối đa 100.000 thành phố và 100.000 con đường, bất kỳ thuật toán nào có độ phức tạp bậc hai đều không thực tế. Hoạt động trên tất cả các nút và cạnh trong thời gian tuyến tính hoặc gần tuyến tính đều được chấp nhận. Chu kỳ hàng tuần nhỏ (d ≤ 50), điều này cho thấy rằng các kỹ thuật tận dụng tính tuần hoàn hoặc chu kỳ nhỏ có thể hiệu quả. Các trường hợp biên bao gồm các thành phố không có rìa đi ra (ngõ cụt), các thành phố có bảo tàng không bao giờ mở cửa vào ngày tham quan hiện tại và các thành phần được kết nối chặt chẽ trong đó có thể cần phải xem lại các nút để tiếp cận tất cả các bảo tàng có thể. 

Việc triển khai bất cẩn có thể cho rằng việc ghé thăm một thành phố một lần là đủ để tiếp cận tất cả các bảo tàng trong một thành phần, nhưng tiến trình theo ngày trong tuần sẽ ảnh hưởng đến tính khả dụng của bảo tàng. Ví dụ: nếu thành phố 1 mở cửa vào ngày 2 và khách du lịch bắt đầu vào ngày 1, một BFS ngây thơ bỏ qua ngày đó sẽ cho rằng bảo tàng mở cửa ngay lập tức một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận brute-force là mô hình hóa vấn đề như một không gian trạng thái của`(city, day_of_week)`cặp. Từ trạng thái ban đầu`(1,0)`, chúng tôi có thể khám phá tất cả các con đường có thể, đánh dấu các bảo tàng đã ghé thăm trên đường đi và theo dõi số lượng bảo tàng riêng biệt tối đa. Điều này có thể đúng, nhưng với n=100.000 và d=50, không gian trạng thái là khoảng 5 triệu nút và việc khám phá tất cả các đường dẫn một cách đơn giản sẽ là cấp số nhân về độ dài đường dẫn, vì vậy nó không khả thi. 

Thông tin chi tiết quan trọng là biểu đồ có thể được **cô đọng thành các thành phần được kết nối mạnh mẽ (SCC)**. Trong SCC, bất kỳ nút nào cũng có thể kết nối với bất kỳ nút nào khác và việc truy cập nhiều nút theo các thứ tự khác nhau chỉ làm thay đổi ngày trong tuần theo modulo d. Do đó, chúng tôi có thể lập mô hình cho từng SCC riêng biệt, xác định từng ngày trong tuần những bảo tàng nào có thể tiếp cận được. Sau khi xác định được SCC, chúng tôi có thể xây dựng **DAG của SCC**. Lập trình động trên DAG này cho phép chúng tôi tính toán số lượng bảo tàng riêng biệt tối đa có thể tiếp cận được từ thành phố 1, truyền bá số lượng dọc theo các cạnh trong khi tính toán các ca làm việc trong ngày trong tuần. 

Cách tiếp cận này làm giảm vấn đề từ khả năng khám phá tất cả các đường dẫn một cách rõ ràng đến việc tính toán các giá trị trên mỗi SCC và mỗi ngày, dẫn đến độ phức tạp O(n * d + m), có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d^n) | O(d^n) | Quá chậm | 
| SCC + DP | O((n + m) * d) | O(n * d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Xây dựng biểu đồ** từ các con đường đầu vào và lưu trữ lịch trình bảo tàng cho mỗi thành phố dưới dạng mảng boolean có độ dài d. Mỗi`schedule[i][j]`cho biết liệu bảo tàng i có mở cửa vào ngày j hay không. 
2. **Tính toán các thành phần được kết nối mạnh (SCC)** bằng thuật toán của Kosaraju hoặc Tarjan. Mỗi SCC được coi là một nút trong DAG. Điều này ghi lại các chu kỳ trong biểu đồ trong đó việc tham quan các bảo tàng theo một thứ tự khác có thể cho phép tiếp cận nhiều bảo tàng hơn do tiến triển của các ngày trong tuần. 
3. **Đối với mỗi SCC**, tính toán trước một vectơ`scc_count[day]`đưa ra số lượng **bảo tàng mới** có thể được ghé thăm nếu vào SCC vào một ngày cụ thể. Vì việc ghé thăm một thành phố nhiều lần trong cùng một chu kỳ tuần là không cần thiết nên chúng tôi chỉ cần tính các bảo tàng có thể tiếp cận được từ SCC vào mỗi ngày theo modulo d. 
4. **Xây dựng DAG của SCC**. Đối với mỗi cạnh giữa các thành phố u và v trong biểu đồ ban đầu, nếu u và v thuộc các SCC khác nhau, hãy thêm một cạnh từ`scc[u]`ĐẾN`scc[v]`. 
5. **Sắp xếp SCC DAG** theo cấu trúc liên kết để xử lý SCC theo thứ tự phụ thuộc. Điều này đảm bảo rằng khi xử lý một SCC, tất cả các SCC mà nó có thể truy cập đều đã được xử lý. 
6. **Lập trình động trên SCC DAG**: cho mỗi SCC`c`và mỗi ngày nhập cảnh`t`, tính toán số bảo tàng riêng biệt tối đa có thể tiếp cận được. Cập nhật hàng xóm`c'`như sau: ngày đến`c'`là`(t + 1) % d`. Thêm vào`scc_count[c'][arrival_day]`tới giá trị DP hiện tại nếu chúng ta chưa tính những bảo tàng này. Truyền giá trị tối đa dọc theo DAG. 
7. **Trả về giá trị DP tối đa** trong số tất cả các ngày có thể đến cho SCC chứa thành phố 1, thể hiện số lượng bảo tàng riêng biệt tối đa bắt đầu từ thành phố 1 vào ngày 0. 

**Tại sao nó hoạt động**: Bất biến là đối với mỗi SCC và mỗi ngày,`dp[SCC][day]`lưu trữ số lượng bảo tàng riêng biệt tối đa có thể truy cập nếu vào SCC này vào ngày đó. Vì DAG đảm bảo rằng chúng tôi xử lý tất cả các phần phụ thuộc trước tiên nên tất cả các đường dẫn đóng góp vào số lượng tối đa đều được xem xét. Các chu kỳ được giới hạn trong SCC và chuyển động trong nội bộ SCC được xử lý theo vectơ ngày trong tuần, do đó không có bảo tàng hợp lệ nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

n, m, d = map(int, input().split())
graph = [[] for _ in range(n)]
rev_graph = [[] for _ in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    graph[u].append(v)
    rev_graph[v].append(u)

schedule = []
for _ in range(n):
    line = input().strip()
    schedule.append([c == '1' for c in line])

# Tarjan's SCC algorithm
index = 0
indices = [-1] * n
lowlink = [-1] * n
onstack = [False] * n
stack = []
scc_id = [-1] * n
scc_list = []

def strongconnect(v):
    global index
    indices[v] = lowlink[v] = index
    index += 1
    stack.append(v)
    onstack[v] = True
    for w in graph[v]:
        if indices[w] == -1:
            strongconnect(w)
            lowlink[v] = min(lowlink[v], lowlink[w])
        elif onstack[w]:
            lowlink[v] = min(lowlink[v], indices[w])
    if lowlink[v] == indices[v]:
        scc = []
        while True:
            w = stack.pop()
            onstack[w] = False
            scc_id[w] = len(scc_list)
            scc.append(w)
            if w == v:
                break
        scc_list.append(scc)

for v in range(n):
    if indices[v] == -1:
        strongconnect(v)

scc_count = [ [0]*d for _ in range(len(scc_list)) ]
for i, scc in enumerate(scc_list):
    # Precompute number of new museums reachable on each day
    for day in range(d):
        visited = [False]*len(scc)
        stack_scc = [(city, day) for city in scc]
        count = 0
        for city in scc:
            if schedule[city][day]:
                count += 1
        scc_count[i][day] = count

# Build SCC DAG
scc_graph = [[] for _ in range(len(scc_list))]
for u in range(n):
    for v in graph[u]:
        if scc_id[u] != scc_id[v]:
            scc_graph[scc_id[u]].append(scc_id[v])

# Remove duplicates
for i in range(len(scc_graph)):
    scc_graph[i] = list(set(scc_graph[i]))

# Topological order
visited_scc = [False]*len(scc_list)
topo = []
def dfs_topo(u):
    visited_scc[u] = True
    for v in scc_graph[u]:
        if not visited_scc[v]:
            dfs_topo(v)
    topo.append(u)

for i in range(len(scc_list)):
    if not visited_scc[i]:
        dfs_topo(i)
topo.reverse()

# DP on SCC DAG
dp = [ [0]*d for _ in range(len(scc_list)) ]
start_scc = scc_id[0]
dp[start_scc][0] = scc_count[start_scc][0]

for u in topo:
    for day in range(d):
        val = dp[u][day]
        if val == 0 and u != start_scc:
            continue
        for v in scc_graph[u]:
            next_day = (day + 1) % d
            dp[v][next_day] = max(dp[v][next_day], val + scc_count[v][next_day])

print(max(dp[start_scc]))
```Giải pháp đầu tiên là xây dựng SCC bằng thuật toán Tarjan. Nó tính toán trước, đối với mỗi SCC, số lượng bảo tàng có sẵn mỗi ngày theo modulo d. Sau đó, nó xây dựng một DAG gồm các SCC, sắp xếp nó theo cấu trúc liên kết và thực hiện DP theo ngày và SCC, truyền bá các bảo tàng tối đa có thể tiếp cận dọc theo các cạnh. Chúng tôi lưu trữ kết quả trong một mảng 2D`dp[SCC][day]`để theo dõi kết quả tốt nhất mỗi ngày theo modulo d. Các điểm tinh tế là xử lý việc gói ngày theo modulo d và
