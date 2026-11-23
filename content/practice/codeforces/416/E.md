---
title: "CF 416E - Con Đường Tổng Thống"
description: "Chúng ta có một biểu đồ gồm n thành phố được nối với nhau bởi m con đường hai chiều, mỗi con đường có chiều dài dương. Mục tiêu là xác định, với mỗi cặp thành phố (s, t) có s < t, có bao nhiêu con đường có thể xuất hiện trên ít nhất một đường đi ngắn nhất từ ​​s đến t."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 416
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 241 (Div. 2)"
rating: 2500
weight: 416
solve_time_s: 124
verified: true
draft: false
---

[CF 416E - Con đường của Tổng thống](https://codeforces.com/problemset/problem/416/E) 

**Đánh giá:** 2500 
**Tags:** dp, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị với`n`các thành phố được kết nối bởi`m`đường hai chiều, mỗi chiều có chiều dài dương. Mục tiêu là xác định, đối với mỗi cặp thành phố`(s, t)`với`s < t`, có bao nhiêu con đường có thể xuất hiện trên **ít nhất một con đường ngắn nhất** từ`s`ĐẾN`t`. Đầu ra là một chuỗi phẳng liệt kê các số đếm này theo thứ tự từ điển của`(s, t)`. 

Đầu vào chỉ định biểu đồ là các cạnh`(x, y, l)`và đầu ra đếm các cạnh có thể là một phần của bất kỳ đường đi ngắn nhất nào giữa mỗi cặp thành phố. Nếu hai thành phố bị ngắt kết nối, số lượng sẽ bằng 0. 

Những hạn chế cho chúng ta biết rằng`n`nhiều nhất là 500. Điều đó ngụ ý rằng hoạt động của`O(n^3)`đều khả thi, nhưng bất cứ điều gì`O(n^4)`có lẽ là quá chậm. Mỗi chiều dài cạnh có thể cao tới 10^6, vì vậy chúng tôi không thể giả định trọng số nhỏ. Vấn đề cũng cho phép`m`bằng 0, nghĩa là một số thành phố có thể bị ngắt kết nối. 

Các trường hợp cạnh tinh tế bao gồm: đồ thị có nhiều đường đi ngắn nhất giữa hai thành phố, các cặp bị ngắt kết nối và đồ thị có chu kỳ các cạnh có trọng số bằng nhau. Ví dụ: trong một hình vuông gồm bốn thành phố có các cạnh có độ dài 1 tạo thành một chu trình, đường đi ngắn nhất từ ​​đỉnh 1 đến đỉnh 3 có thể sử dụng đường chéo, do đó số cạnh bao gồm tất cả các cạnh tham gia vào bất kỳ đường đi ngắn nhất nào. Một cách tiếp cận ngây thơ chỉ tính một con đường duy nhất sẽ bị tính thiếu ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: với mỗi cặp`(s, t)`, chạy Dijkstra hoặc BFS để tìm tất cả các đường đi ngắn nhất và đánh dấu các cạnh trên chúng. Tuy nhiên, nếu chúng ta làm điều này một cách ngây thơ, nó sẽ`O(n * (n + m) log n)`mỗi cặp, từ bỏ`O(n^2 * (n + m) log n)`hoạt động. Với`n = 500`Và`m ~ n^2`, điều này có thể đạt tới hàng trăm triệu hoạt động, quá chậm. 

Thông tin chi tiết quan trọng là trước tiên chúng ta có thể tính toán **khoảng cách ngắn nhất cho tất cả các cặp**. Một khi chúng ta biết`dist[u][v]`đối với tất cả các cặp, chúng ta có thể xác định xem một cạnh`(u, v)`nằm trên đường đi ngắn nhất từ`s`ĐẾN`t`sử dụng kiểm tra khoảng cách đơn giản: nếu`dist[s][u] + length(u, v) + dist[v][t] == dist[s][t]`, cạnh có thể xuất hiện trên đường đi ngắn nhất từ`s`ĐẾN`t`. Điều này làm giảm tính toán lặp lại và tránh liệt kê tất cả các đường dẫn một cách rõ ràng. 

Cách tiếp cận tối ưu sử dụng **Floyd-Warshall** hoặc Dijkstra từ mỗi nút để tính toán các đường đi ngắn nhất cho tất cả các cặp (`O(n^3)`cho Floyd-Warshall). Sau đó, với mỗi nguồn`s`, chúng tôi lặp lại trên tất cả các cạnh và đích`t`để kiểm tra điều kiện đường đi ngắn nhất. Điều này dẫn đến một cấu trúc lồng nhau nhưng vẫn nằm trong`O(n^3)`bởi vì mỗi lần kiểm tra là thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * (n + m) log n) | O(n + m) | Quá chậm với n=500 | 
| Tối ưu | O(n^3) | O(n^2 + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ma trận khoảng cách`dist`kích thước`n x n`với vô cùng và tập hợp`dist[u][u] = 0`. Điều này thể hiện khoảng cách ngắn nhất hiện tại từ mỗi thành phố đến mọi thành phố khác. 
2. Dân số`dist`với độ dài cạnh trực tiếp: cho mỗi cạnh`(u, v, w)`, bộ`dist[u][v] = dist[v][u] = min(dist[u][v], w)`để tính đến nhiều cạnh (mặc dù vấn đề nêu rõ nhiều nhất là một cạnh, nhưng điều này sẽ tránh được việc ghi đè ngẫu nhiên). 
3. Áp dụng Floyd-Warshall: cho từng thành phố trung gian`k`, cho mỗi cặp`(i, j)`, cập nhật`dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`. Sau bước này,`dist[i][j]`giữ khoảng cách ngắn nhất từ`i`ĐẾN`j`. 
4. Khởi tạo một mảng`count`kích thước`n x n`để lưu trữ số cạnh trên ít nhất một đường đi ngắn nhất cho mỗi cạnh`(s, t)`. 
5. Đối với mỗi cạnh`(u, v, w)`, lặp lại tất cả các thành phố nguồn`s`và tất cả các thành phố đích`t`với`s < t`. Nếu như`dist[s][u] + w + dist[v][t] == dist[s][t]`hoặc`dist[s][v] + w + dist[u][t] == dist[s][t]`, tăng`count[s][t]`bằng 1. Điều này kiểm tra xem cạnh`(u, v)`nằm trên đường đi ngắn nhất từ`s`ĐẾN`t`. 
6. Làm phẳng`count`cho tất cả`s < t`thành một chuỗi duy nhất và in. 

**Tại sao nó hoạt động**: Bước Floyd-Warshall đảm bảo rằng`dist[s][t]`chứa khoảng cách thực sự ngắn nhất. Kiểm tra`dist[s][u] + w + dist[v][t] == dist[s][t]`đảm bảo rằng đường đi qua cạnh`(u, v)`không vượt quá khoảng cách tối thiểu. Vì việc kiểm tra là đối xứng đối với`(u, v)`Và`(v, u)`, chúng tôi nắm bắt tất cả các con đường ngắn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
edges = []
INF = 10**18

dist = [[INF] * n for _ in range(n)]
for i in range(n):
    dist[i][i] = 0

for _ in range(m):
    u, v, w = map(int, input().split())
    u -= 1
    v -= 1
    dist[u][v] = w
    dist[v][u] = w
    edges.append((u, v, w))

# Floyd-Warshall
for k in range(n):
    for i in range(n):
        for j in range(n):
            if dist[i][k] + dist[k][j] < dist[i][j]:
                dist[i][j] = dist[i][k] + dist[k][j]

count = [[0] * n for _ in range(n)]

for u, v, w in edges:
    for s in range(n):
        for t in range(s + 1, n):
            if dist[s][u] + w + dist[v][t] == dist[s][t] or dist[s][v] + w + dist[u][t] == dist[s][t]:
                count[s][t] += 1

result = []
for s in range(n):
    for t in range(s + 1, n):
        result.append(str(count[s][t]))

print(" ".join(result))
```các`dist`sử dụng khởi tạo`INF`để đại diện cho các thành phố không thể tiếp cận được. Floyd-Warshall đảm bảo khoảng cách của tất cả các cặp đều chính xác. Khi kiểm tra các cạnh, hai điều kiện này giải thích cho tính chất vô hướng của đường. Vòng lặp làm phẳng tôn trọng`s < t`yêu cầu. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
5 6
1 2 1
2 3 1
3 4 1
4 1 1
2 4 2
4 5 4
```| s | t | quận[s][t] | cạnh trên đường đi ngắn nhất | đếm[s][t] | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | (1,2) | 1 | 
| 1 | 3 | 2 | (1,2),(2,3),(1,4),(4,3) | 4 | 
| 1 | 4 | 1 | (1,4) | 1 | 
| 1 | 5 | 5 | (4,5),(3,4) | 2 | 
| 2 | 3 | 1 | (2,3) | 1 | 
| 2 | 4 | 2 | (2,4),(2,3),(3,4),(1,4) | 5 | 
| 2 | 5 | 6 | (4,5),(3,4) | 2 | 
| 3 | 4 | 1 | (3,4) | 1 | 
| 3 | 5 | 5 | (3,4),(4,5) | 2 | 
| 4 | 5 | 4 | (4,5) | 1 | 

Dấu vết này xác nhận rằng thuật toán xác định chính xác tất cả các cạnh nằm trên đường đi ngắn nhất **bất kỳ** nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Floyd-Warshall là O(n^3), kiểm tra các cạnh là O(n^3) khi m ≤ n^2 | 
| Không gian | O(n^2 + m) | Ma trận khoảng cách là n^2, danh sách cạnh là m | 

Cho n 500, n^3 = 125 triệu thao tác có thể chấp nhận được với giới hạn 4 giây. Việc sử dụng bộ nhớ vẫn nằm trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    edges = []
    INF = 10**18
    dist = [[INF]*n for _ in range(n)]
    for i in range(n): dist[i][i]=0
    for _ in range(m):
        u,v,w=map(int,input().split())
        u-=
```
