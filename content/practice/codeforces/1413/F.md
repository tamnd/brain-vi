---
title: "CF 1413F - Đường và Ramen"
description: "Chúng ta được cấp một cái cây với $n$ ngôi làng được nối với nhau bằng những con đường $n-1$, trong đó mỗi con đường là đá hoặc cát. Mỗi ngày, có đúng một con đường thay đổi loại: đá trở thành cát hoặc cát trở thành đá."
date: "2026-06-11T07:24:38+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 1413
codeforces_index: "F"
codeforces_contest_name: "Technocup 2021 - Elimination Round 1"
rating: 2800
weight: 1413
solve_time_s: 107
verified: false
draft: false
---

[CF 1413F - Đường và Ramen](https://codeforces.com/problemset/problem/1413/F) 

**Đánh giá:** 2800 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$làng được kết nối bởi$n-1$đường, trong đó mỗi con đường là đá hoặc cát. Mỗi ngày, có đúng một con đường thay đổi loại: đá trở thành cát hoặc cát trở thành đá. Naruto và Jiraiya sau đó chọn một con đường đơn giản sao cho số lượng đường đá trên đó là chẵn, để mỗi người có thể ăn cùng một số cốc ramen và con đường càng dài càng tốt xét về số lượng đường. Trong mỗi ngày, chúng ta phải xuất ra độ dài tối đa của con đường đó sau khi lật đường. 

Đầu vào đảm bảo cấu trúc là một cây, do đó có một đường dẫn đơn giản duy nhất giữa hai nút bất kỳ. Các ràng buộc rất lớn: lên tới$5 \cdot 10^5$làng và$5 \cdot 10^5$lật hàng ngày. Bất kỳ giải pháp nào với$O(n)$công việc mỗi ngày quá chậm, vì$n \cdot m$sẽ theo thứ tự của$2.5 \cdot 10^{11}$. Điều này loại trừ việc tính toán lại các đường đi dài nhất từ ​​đầu sau mỗi lần lật. Chúng tôi cần một giải pháp cập nhật hiệu quả khi một con đường bị lật. 

Các trường hợp cạnh không rõ ràng bao gồm các cây có dạng chuỗi tuyến tính, hình ngôi sao hoặc có các mẫu đá/cát xen kẽ. Trong một chuỗi gồm tất cả các viên đá có chiều dài chẵn, bất kỳ đường đi nào cũng sẽ tự động thỏa mãn điều kiện chẵn lẻ của ramen. Việc lật một cạnh trung tâm trong chuỗi có độ dài lẻ có thể làm giảm độ dài đường dẫn hợp lệ dài nhất, do đó thuật toán phải theo dõi phân phối chẵn lẻ một cách cẩn thận. 

## Phương pháp tiếp cận 

Phương pháp brute-force cố gắng tính toán đường đi tối đa cho mỗi ngày bằng cách tạo ra tất cả các đường đi đơn giản có thể có và kiểm tra xem đường đi nào có số cạnh đá chẵn. Trên cây, số đường đi đơn là$O(n^2)$. Với mỗi đường đi, việc đếm các cạnh đá cần tới$O(n)$hoạt động. Điều này mang lại độ phức tạp trong trường hợp xấu nhất là$O(n^3)$, điều này là không thể thực hiện được đối với$n$lên đến$5 \cdot 10^5$. Ngay cả việc lưu trữ tất cả các đường dẫn một cách rõ ràng cũng không thể thực hiện được do hạn chế về bộ nhớ. 

Quan sát quan trọng là tính chẵn lẻ của các đường đá dọc theo một đường đi có thể được giảm xuống thành nhãn hai màu của các nút: xem xét mỗi nút để lưu trữ tính chẵn lẻ của các cạnh đá dọc theo đường đi từ một gốc cố định. Một đường đi có số đường đá chẵn khi và chỉ khi các điểm cuối có cùng điểm chẵn lẻ. Điều này cho phép chúng ta phân vùng cây thành hai tập hợp nút, chẵn lẻ 0 và chẵn lẻ 1. Đường dẫn hợp lệ dài nhất sau đó là đường dẫn lớn hơn trong số các đường dẫn dài nhất hoàn toàn nằm trong bộ chẵn lẻ 0 và chẵn lẻ 1. 

Việc lật một con đường sẽ thay đổi tính chẵn lẻ của tất cả các nút ở một trong hai cây con cách nhau bởi cạnh. Bằng cách duy trì độ sâu của các nút trong mỗi tập chẵn lẻ và theo dõi đường kính của các cây con được tạo ra, chúng ta có thể cập nhật độ dài đường dẫn hợp lệ tối đa trong$O(\log n)$mỗi lần lật bằng cách sử dụng cây phân đoạn hoặc cây cắt liên kết. Vấn đề giảm xuống còn vấn đề đường kính cây động với các lần lật chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^2) | Quá chậm | 
| Tối ưu (Chẵn lẻ + Cây động) | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại một nút tùy ý. Đối với mỗi nút, hãy tính tính chẵn lẻ của các cạnh đá dọc theo đường dẫn đến gốc. Giá trị này được lưu dưới dạng 0 cho chẵn lẻ và 1 cho chẵn lẻ lẻ. 
2. Tính đường kính ban đầu của mỗi bộ chẵn lẻ. Đường kính là đường đi dài nhất giữa hai nút bất kỳ trong cùng một chẵn lẻ. Điều này có thể được thực hiện bằng cách sử dụng hai lượt DFS: đầu tiên là tìm nút xa nhất trong tập chẵn lẻ, thứ hai là tìm nút xa nhất tính từ nút đó. 
3. Với mỗi lần lật, hãy xác định cạnh được lật. Xác định cây con nào trong hai cây con được phân tách bằng cạnh đó sẽ có tính chẵn lẻ được chuyển đổi. Lật tính chẵn lẻ của tất cả các nút trong cây con đó. 
4. Cập nhật đường kính cho cả hai bộ chẵn lẻ. Cây con đảo ngược có thể tăng hoặc giảm đường kính của tập chẵn lẻ. Cập nhật hiệu quả có thể được thực hiện bằng cách sử dụng biểu diễn chuyến tham quan Euler kết hợp với cây phân đoạn theo dõi độ sâu tối đa. 
5. Sau mỗi lần lật, đáp án là đường kính lớn nhất của hai bộ chẵn lẻ. Xuất giá trị này. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng mỗi nút biết tính chẵn lẻ hiện tại của nó so với gốc và đường kính của các bộ chẵn lẻ 0 và chẵn lẻ 1 luôn phản ánh các đường dẫn dài nhất có thể được chọn trong đó Naruto và Jiraiya có thể chia đều ramen. Việc lật một cạnh ảnh hưởng chính xác đến tính chẵn lẻ của một cây con và việc cập nhật đường kính tương ứng sẽ đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

n = int(input())
edges = []
tree = [[] for _ in range(n)]
for i in range(n-1):
    u, v, t = map(int, input().split())
    u -= 1
    v -= 1
    edges.append((u, v, t))
    tree[u].append((v, t, i))
    tree[v].append((u, t, i))

m = int(input())
flips = [int(input())-1 for _ in range(m)]

# Euler tour for subtree parity flips
in_time = [0]*n
out_time = [0]*n
depth = [0]*n
parity = [0]*n
time = 0

def dfs(u, p, d, par):
    global time
    in_time[u] = time
    time += 1
    depth[u] = d
    parity[u] = par
    for v, t, _ in tree[u]:
        if v == p:
            continue
        dfs(v, u, d+1, par ^ t)
    out_time[u] = time

dfs(0, -1, 0, 0)

# Segment tree to track max depth per parity
size = 1
while size < n:
    size *= 2
seg = [(-1, -1)]*(2*size)

for i in range(n):
    if parity[i] == 0:
        seg[size+i] = (depth[i], -1)
    else:
        seg[size+i] = (-1, depth[i])

for i in range(size-1, 0, -1):
    l, r = seg[2*i], seg[2*i+1]
    seg[i] = (max(l[0], r[0]), max(l[1], r[1]))

def update(pos, old_parity):
    i = size + pos
    if old_parity == 0:
        seg[i] = (-1, depth[pos])
    else:
        seg[i] = (depth[pos], -1)
    i //= 2
    while i >= 1:
        l, r = seg[2*i], seg[2*i+1]
        seg[i] = (max(l[0], r[0]), max(l[1], r[1]))
        i //= 2

def get_max():
    return max(seg[1][0], seg[1][1])

for idx in flips:
    u, v, t = edges[idx]
    if depth[u] < depth[v]:
        u, v = v, u
    old_parity = parity[u]
    def flip_subtree(x):
        parity[x] ^= 1
        for y, _, _
```
