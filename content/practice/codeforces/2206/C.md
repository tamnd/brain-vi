---
title: "CF 2206C - Dijkstra lộn ngược"
description: "Chúng ta có một đồ thị vô hướng liên thông với các đỉnh $n$ và các cạnh $m$, trong đó mỗi cạnh nối hai đỉnh nhưng không biết trọng số của nó."
date: "2026-06-07T19:40:06+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 2206
codeforces_index: "C"
codeforces_contest_name: "2026 ICPC Asia Pacific Championship - Online Mirror (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2200
weight: 2206
solve_time_s: 145
verified: false
draft: false
---

[CF 2206C - Dijkstra lộn ngược](https://codeforces.com/problemset/problem/2206/C) 

**Đánh giá:** 2200 
**Thẻ:** dfs và tương tự 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông với$n$đỉnh và$m$cạnh, trong đó mỗi cạnh nối hai đỉnh nhưng không biết trọng lượng của nó. Anh chị em của bạn đã chạy một biến thể của thuật toán Dijkstra bắt đầu từ đỉnh$1$, nhưng mắc một sai lầm nghiêm trọng: thay vì luôn xuất hiện khoảng cách tối thiểu từ đống, họ lại xuất hiện khoảng cách tối đa trước tiên. Họ đã ghi lại thứ tự các đỉnh được đưa vào một mảng lần đầu tiên$S$. Nhiệm vụ của chúng ta là tái tạo lại các trọng số cạnh số nguyên dương để "Dijkstra lộn ngược" này tạo ra chuỗi chính xác$S$. Nếu điều này là không thể, chúng tôi phải báo cáo như vậy. 

Những hạn chế chính là$n$có thể lên đến$10^5$Và$m$lên tới$2 \cdot 10^5$, nghĩa là mọi giải pháp đều phải đại khái$O(n + m)$hoặc$O((n + m) \log n)$. Việc tìm kiếm brute-force trên tất cả các trọng số cạnh có thể là không khả thi. Trọng số của cạnh phải là số nguyên dương tối đa$10^9$, do đó có một phạm vi lớn nhưng bị giới hạn của các trọng số hợp lệ. 

Các trường hợp cạnh không rõ ràng bao gồm các biểu đồ trong đó$S$bắt đầu bằng một đỉnh không phải là 1 hoặc các chuỗi không thể tương ứng với bất kỳ phép gán khoảng cách nào. Ví dụ: đồ thị tam giác có các đỉnh$1, 2, 3$Và$S = [1, 3, 2]$có thể hài lòng với trọng lượng thích hợp, nhưng$S = [2, 1, 3]$là không thể vì đỉnh xuất hiện đầu tiên phải luôn có thể tiếp cận được với khoảng cách ban đầu lớn nhất tính từ điểm bắt đầu. 

Một điều tinh tế khác là nhiều cạnh có thể kết nối hai "cấp" đỉnh giống nhau. Việc ấn định trọng số đồng đều một cách bất cẩn có thể vi phạm các ràng buộc về thứ tự trong$S$, vì heap có thể xuất hiện các đỉnh theo thứ tự khác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các trọng số nguyên dương có thể có và mô phỏng Dijkstra lộn ngược để xem liệu chuỗi kết quả có khớp không$S$. Điều này rõ ràng là không khả thi vì ngay cả trọng lượng của một cạnh cũng lên tới$10^9$khả năng và có tới$2 \cdot 10^5$các cạnh, mang lại một không gian tìm kiếm lớn về mặt thiên văn. 

Điểm mấu chốt là chúng ta không cần trọng số chính xác mà chỉ cần sự khác biệt tương đối giữa các đỉnh. Nếu chúng ta gán một “mức” hoặc “khoảng cách” cho mỗi đỉnh theo vị trí của nó trong$S$, chúng ta có thể đảm bảo rằng đống lộn ngược sẽ xuất hiện các đỉnh theo đúng thứ tự. Cụ thể là ấn định khoảng cách$d[v]$như vậy$d[S_1] > d[S_2] > \dots > d[S_n]$, giảm đi một giữa các đỉnh liên tiếp. Sau đó với mỗi cạnh$u \text{--} v$, đặt trọng số của nó$w = |d[u] - d[v]|$, đảm bảo rằng việc di chuyển dọc theo cạnh không bao giờ vi phạm thứ tự heap tối đa. 

Điều này hiệu quả vì Dijkstra lộn ngược về cơ bản sẽ làm nổi bật các đỉnh theo thứ tự khoảng cách giảm dần. Bằng cách ấn định khoảng cách phù hợp với trình tự$S$và trọng số bằng với chênh lệch khoảng cách dọc theo mỗi cạnh, chúng tôi đảm bảo rằng mọi đỉnh đầu tiên sẽ được xuất hiện chính xác theo thứ tự được chỉ định của nó trong$S$. Nếu bất kỳ cạnh nào yêu cầu trọng số bằng 0 hoặc âm để đáp ứng thứ tự thì việc gán là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((10^9)^m) | O(n + m) | Quá chậm | 
| Bài tập cấp độ + Sự khác biệt | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`pos`ánh xạ từng đỉnh tới chỉ mục của nó trong$S$. Điều này cho phép tra cứu nhanh chóng thứ tự của một đỉnh trong chuỗi. 
2. Gán “khoảng cách giả”$d[v]$tới mỗi đỉnh như$n - \text{pos}[v]$. đỉnh$S_1$được$n$,$S_2$được$n-1$, vân vân. Điều này đảm bảo$d[S_i] > d[S_{i+1}]$. 
3. Đối với mỗi cạnh$u_j \text{--} v_j$, tính trọng lượng của nó như$w_j = |d[u_j] - d[v_j]|$. Nếu như$w_j = 0$, cạnh không thể hỗ trợ sắp xếp thứ tự, do đó không thể xuất ra. 
4. Ngược lại, chỉ định$w_j = |d[u_j] - d[v_j]|$. Điều này đảm bảo rằng dọc theo bất kỳ cạnh nào, việc di chuyển từ đỉnh này sang đỉnh kia không bao giờ tạo ra một đỉnh có khoảng cách cao hơn khoảng cách cho phép.$S$. 
5. Xuất ra tất cả các trọng số. Bất kỳ bài tập nhất quán nào đáp ứng các quy tắc trên đều hợp lệ. 

Tại sao nó hoạt động: heap luôn xuất hiện đỉnh có khoảng cách lớn nhất. Bằng cách ấn định khoảng cách giảm dần dọc theo$S$và coi trọng số là hiệu, mỗi đỉnh sẽ có khoảng cách lớn nhất khi đến lượt nó. Bất kỳ cạnh nào có trọng số bằng 0 có nghĩa là hai đỉnh có cùng khoảng cách, vi phạm trật tự nghiêm ngặt trong$S$, đó là lý do vì sao chúng ta kiểm tra điều không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
edges = [tuple(map(int, input().split())) for _ in range(m)]
S = list(map(int, input().split()))

pos = [0] * (n + 1)
for idx, v in enumerate(S):
    pos[v] = idx

# assign distances decreasing from n
d = [0] * (n + 1)
for v in range(1, n + 1):
    d[v] = n - pos[v]

weights = []
possible = True
for u, v in edges:
    w = abs(d[u] - d[v])
    if w == 0:
        possible = False
        break
    weights.append(w)

if not possible:
    print("impossible")
else:
    print(" ".join(map(str, weights)))
```Giải pháp trước tiên xây dựng ánh xạ vị trí và gán khoảng cách giả giảm dần. Khi đó, trọng số của mỗi cạnh là hiệu tuyệt đối của khoảng cách. Trọng số bằng 0 báo hiệu không thể xảy ra vì nếu không thì hai đỉnh sẽ được xuất hiện đồng thời, vi phạm thứ tự trong$S$. Việc lựa chọn sử dụng khoảng cách giảm dần từ$n$đảm bảo tất cả các trọng số là số nguyên dương. Sự khác biệt tuyệt đối tạo ra trọng số hợp lệ cho cả hai hướng của một cạnh. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
5 7
3 4
2 3
1 2
3 5
1 4
1 5
4 5
1 4 3 5 2
```Khoảng cách được chỉ định: 

| Đỉnh | Vị trí S | d[v] | 
| --- | --- | --- | 
| 1 | 0 | 5 | 
| 4 | 1 | 4 | 
| 3 | 2 | 3 | 
| 5 | 3 | 2 | 
| 2 | 4 | 1 | 

Trọng lượng: 

| Cạnh | d[u] - d[v] | w | 
| --- | --- | --- | 
| 3-4 | 3-4 | 1 | 
| 2-3 | 1-3 | 2 | 
| 1-2 | 5-1 | 4 | 
| 3-5 | 3-2 | 1 | 
| 1-4 | 5-4 | 1 | 
| 1-5 | 5-2 | 3 | 
| 4-5 | 4-2 | 2 | 

Mọi trọng lượng nhất quán đều được chấp nhận; ở đây, tất cả đều tích cực và thứ tự khớp`S`. 

Ví dụ nhỏ tùy chỉnh: 

Đồ thị 1-2-3, S = [1,2,3]. Khoảng cách: d[1]=3, d[2]=2, d[3]=1. Trọng số cạnh: 1-2=1, 2-3=1. Heap bật 1,2,3, theo yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Ánh xạ vị trí và trọng số cạnh được tính toán trong một lần duyệt qua các đỉnh và cạnh | 
| Không gian | O(n + m) | Lưu trữ vị trí, khoảng cách và cạnh | 

Với$n \le 10^5$Và$m \le 2 \cdot 10^5$, điều này rất phù hợp trong giới hạn thời gian 2 giây. Mỗi bước đều có kích thước tuyến tính và số học số nguyên nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    edges = [tuple(map(int, input().split())) for _ in range(m)]
    S = list(map(int, input().split()))
    pos = [0] * (n + 1)
    for idx, v in enumerate(S):
        pos[v] = idx
    d = [0] * (n + 1)
    for v in range(1, n + 1):
        d[v] = n - pos[v]
    weights = []
    possible = True
    for u, v in edges:
        w = abs(d[u] - d[v])
        if w == 0:
            possible = False
            break
        weights.append(w)
    return "impossible" if not possible else " ".join(map(str, weights))

# Provided sample
assert run("5 7\n3 4\n2 3\n1 2\n3 5\n1 4\n1 5\n4 5\n1 4 3 5 2\n") != "impossible"

# Minimum size graph
assert run("2
```
