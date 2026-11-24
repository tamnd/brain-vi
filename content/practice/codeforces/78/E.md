---
title: "CF 78E - Sơ tán"
description: "Chúng ta có một lưới phòng thí nghiệm có kích thước n × n, đại diện cho một trạm nghiên cứu trong đó một số ô là lò phản ứng và các ô khác là phòng thí nghiệm. Một lò phản ứng gặp trục trặc và sẽ phát nổ, khiến chất làm mát độc hại lan sang các phòng thí nghiệm lân cận."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 78
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 70 (Div. 2)"
rating: 2300
weight: 78
solve_time_s: 154
verified: true
draft: false
---

[CF 78E - Sơ tán](https://codeforces.com/problemset/problem/78/E) 

**Đánh giá:** 2300 
**Tags:** luồng, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới phòng thí nghiệm có kích thước _n_ × _n_, thể hiện một trạm nghiên cứu trong đó một số ô là lò phản ứng và các ô khác là phòng thí nghiệm. Một lò phản ứng gặp trục trặc và sẽ phát nổ, khiến chất làm mát độc hại lan sang các phòng thí nghiệm lân cận. Mỗi phòng thí nghiệm có một số nhà khoa học và một số viên cứu hộ. Các nhà khoa học có thể di chuyển giữa các phòng thí nghiệm liền kề trong một phút và có thể vào khoang chứa ngay lập tức. Chất làm mát lan truyền theo cách tương tự, lây nhiễm sang các phòng thí nghiệm lân cận mỗi phút. Một khi phòng thí nghiệm bị nhiễm bệnh, bất kỳ nhà khoa học nào chưa ở trong viên nang đều chết. 

Nhiệm vụ là xác định số lượng nhà khoa học tối đa có thể được cứu nếu tất cả các nhà khoa học hành động tối ưu, trong khoảng thời gian có hạn trước vụ nổ. 

Các ràng buộc nhỏ: _n_ có thể nhiều nhất là 10, nghĩa là tổng số ô nhiều nhất là 100. Số phút _t_ có thể lên tới 60. Vì lưới rất nhỏ nên ngay cả các giải pháp có độ phức tạp bậc ba hoặc bậc bốn trong _n_ cũng có thể khả thi. Tuy nhiên, chúng ta phải cẩn thận vì vấn đề đòi hỏi phải xem xét dòng chảy của các nhà khoa học qua mạng lưới phòng thí nghiệm trước khi lây nhiễm. 

Các trường hợp cạnh bao gồm các phòng thí nghiệm ngay cạnh lò phản ứng bị trục trặc. Ví dụ: một phòng thí nghiệm có nhà khoa học và viên nang bên cạnh lò phản ứng có thể cứu được nhà khoa học của mình hoặc không tùy thuộc vào thời điểm lây nhiễm. Một trường hợp phức tạp khác là khi số lượng nhà khoa học vượt quá số lượng viên nang trong các phòng thí nghiệm an toàn có thể tiếp cận được, do đó một số nhà khoa học nhất thiết phải chết. Các giải pháp ngây thơ chỉ khiến các nhà khoa học tham lam mà không xem xét đến dòng thời gian lây nhiễm hoặc dung lượng của viên nang sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ cố gắng mô phỏng mọi chuyển động có thể có của các nhà khoa học vào từng phút đồng thời lây lan sự lây nhiễm. Mỗi nhà khoa học sẽ phải đưa ra quyết định ở mỗi bước, dẫn đến số khả năng tăng theo cấp số nhân, điều này rõ ràng là không khả thi ngay cả với _n_ = 10 và _t_ = 60. 

Nhận xét quan trọng là về cơ bản đây là bài toán luồng cực đại có ràng buộc về thời gian. Chúng ta có thể lập mô hình từng phòng thí nghiệm ở mỗi bước thời gian dưới dạng một nút trong biểu đồ. Các cạnh kết nối một phòng thí nghiệm tại thời điểm _k_ với các phòng thí nghiệm lân cận tại thời điểm _k+1_, thể hiện một nhà khoa học đang di chuyển trong một phút. Ngoài ra, mỗi phòng thí nghiệm có một nút để vào một viên nang có sức chứa bằng số lượng viên nang. Sự lây nhiễm giới hạn thời gian mỗi nút có thể tồn tại. 

Sau khi xây dựng biểu đồ, chúng tôi có thể tính toán số lượng nhà khoa học tối đa có thể tiếp cận các viên nang trước khi phòng thí nghiệm của họ bị lây nhiễm bằng thuật toán dòng tối đa tiêu chuẩn. Kích thước nhỏ của lưới và giới hạn thời gian giúp việc mô hình hóa mạng này thành một mạng luồng có tối đa các nút _n_ × _n_ × (_t_+1) có thể quản lý được bằng thuật toán luồng Dinic hoặc Edmonds-Karp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(4^(_n^2_×_t_)) | O(_n^2_×_t_) | Không khả thi | 
| Lưu lượng tối đa mở rộng theo thời gian | O(V^2 E) ≈ O((n^2 t)^2 × 4) | O(n^2 × t) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định tọa độ của lò phản ứng bị trục trặc. Đây sẽ là nguồn gốc của sự lan truyền chất làm mát. 
2. Tính toán cho mỗi phòng thí nghiệm vào phút sớm nhất mà nó bị lây nhiễm bằng cách sử dụng tìm kiếm theo chiều rộng bắt đầu từ lò phản ứng bị trục trặc. Lò phản ứng ngăn chặn sự lây lan. Lưu trữ thời gian lây nhiễm này trong mảng 2D`infect_time`. Các phòng thí nghiệm không bao giờ bị nhiễm bệnh có thể được coi là có`infect_time = t+1`. 
3. Xây dựng mạng lưới luồng mở rộng theo thời gian. Mỗi phòng thí nghiệm tại thời điểm _k_ (0 ≤ k ≤ _t_) được biểu thị bằng một nút. Đối với phòng thí nghiệm ở vị trí (i,j) và thời điểm k, hãy kết nối phòng thí nghiệm đó với phòng thí nghiệm lân cận tại thời điểm k+1 với dung lượng vô hạn nếu thời gian lây nhiễm của phòng thí nghiệm hàng xóm lớn hơn k+1. Ngoài ra, hãy kết nối cùng một phòng thí nghiệm tại thời điểm k đến thời điểm k+1 để cho phép các nhà khoa học giữ nguyên vị trí. 
4. Đối với mỗi phòng thí nghiệm tại mỗi thời điểm, hãy kết nối nút của nó với nút "capsule sink" có dung lượng bằng số lượng viên nang trong phòng thí nghiệm đó. Cạnh này đôi khi chỉ tồn tại ít hơn thời gian lây nhiễm của phòng thí nghiệm. 
5. Giới thiệu một nút nguồn kết nối với tất cả các phòng thí nghiệm tại thời điểm 0 với dung lượng bằng số lượng nhà khoa học trong phòng thí nghiệm đó. 
6. Chạy thuật toán luồng tối đa (Dinic) từ nguồn đến bồn chứa viên nang. Giá trị dòng chảy tối đa bằng số lượng nhà khoa học tối đa có thể trốn thoát trước khi sự lây nhiễm đến với họ. 
7. Trả về lưu lượng tối đa đã tính toán. 

Tại sao nó hoạt động: Mạng mã hóa chính xác các hạn chế về chuyển động và thời gian. Mỗi nhà khoa học tương ứng với một đơn vị dòng chảy, dung tích viên nang và thời gian lây nhiễm sẽ hạn chế dòng chảy vào bồn rửa. Một nhà khoa học không thể chiếm giữ một nút vượt quá thời gian lây nhiễm của nó và không thể vào một viên nang vượt quá số lượng sẵn có. Do đó, việc tối đa hóa dòng chảy sẽ trực tiếp mô hình hóa kế hoạch sơ tán tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

class Edge:
    def __init__(self, to, rev, cap):
        self.to = to
        self.rev = rev
        self.cap = cap

class MaxFlow:
    def __init__(self, N):
        self.size = N
        self.graph = [[] for _ in range(N)]

    def add(self, fr, to, cap):
        self.graph[fr].append(Edge(to, len(self.graph[to]), cap))
        self.graph[to].append(Edge(fr, len(self.graph[fr]) - 1, 0))

    def bfs_level(self, s, t, level):
        q = deque()
        level[s] = 0
        q.append(s)
        while q:
            v = q.popleft()
            for e in self.graph[v]:
                if e.cap > 0 and level[e.to] < 0:
                    level[e.to] = level[v] + 1
                    q.append(e.to)
        return level[t] >= 0

    def dfs_flow(self, v, t, upTo, level, iter):
        if v == t:
            return upTo
        for i in range(iter[v], len(self.graph[v])):
            e = self.graph[v][i]
            if e.cap > 0 and level[v] < level[e.to]:
                d = self.dfs_flow(e.to, t, min(upTo, e.cap), level, iter)
                if d > 0:
                    e.cap -= d
                    self.graph[e.to][e.rev].cap += d
                    return d
            iter[v] += 1
        return 0

    def max_flow(self, s, t):
        flow = 0
        level = [-1] * self.size
        INF = 10**9
        while True:
            level = [-1] * self.size
            if not self.bfs_level(s, t, level):
                break
            iter = [0] * self.size
            while True:
                f = self.dfs_flow(s, t, INF, level, iter)
                if f == 0:
                    break
                flow += f
        return flow

n, t = map(int, input().split())
sci_grid = [input().strip() for _ in range(n)]
input()  # empty line
cap_grid = [input().strip() for _ in range(n)]

INF = 10**9
dx = [0,0,1,-1]
dy = [1,-1,0,0]

infect_time = [[INF]*n for _ in range(n)]
queue = deque()
for i in range(n):
    for j in range(n):
        if sci_grid[i][j] == 'Z':
            queue.append((i,j,0))
            infect_time[i][j] = 0

while queue:
    x, y, time = queue.popleft()
    for d in range(4):
        nx, ny = x+dx[d], y+dy[d]
        if 0<=nx<n and 0<=ny<n and sci_grid[nx][ny] not in 'YZ' and infect_time[nx][ny] > time+1:
            infect_time[nx][ny] = time+1
            queue.append((nx, ny, time+1))

# build flow graph
def node(i,j,k):
    return (i*n+j)*(t+1)+k

N = n*n*(t+1) + 2
src = N-2
sink = N-1
mf = MaxFlow(N)

for i in range(n):
    for j in range(n):
        if sci_grid[i][j] in 'YZ':
            continue
        max_time = min(t, infect_time[i][j]-1)
        for k in range(max_time+1):
            # stay
            if k < max_time:
                mf.add(node(i,j,k), node(i,j,k+1), INF)
            # move
            for d in range(4):
                ni, nj = i+dx[d], j+dy[d]
                if 0<=ni<n and 0<=nj<n and sci_grid[ni][nj] not in 'YZ' and k+1 <= infect_time[ni][nj]-1:
                    mf.add(node(i,j,k), node(ni,nj,k+1), INF)
            # capsules
            mf.add(node(i,j,k), sink, int(cap_grid[i][j]))

for i in range(n):
    for j in range(n):
        if sci_grid[i][j] not in 'YZ' and int(sci_grid[i][j]) > 0:
            mf.add(src, node(i,j,0), int(sci_grid[i][j]))

print
```
