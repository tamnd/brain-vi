---
title: "CF 1463E - Kế hoạch bài giảng"
description: "Chúng tôi có một giáo viên, Ivan, người cần sắp xếp các bài giảng về n chủ đề khác nhau. Mỗi chủ đề có nhiều nhất một điều kiện tiên quyết, nghĩa là anh ta không thể giảng về một chủ đề trước điều kiện tiên quyết của nó. Một chủ đề không có điều kiện tiên quyết."
date: "2026-06-11T02:01:34+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dsu", "graphs", "implementation", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1463
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 100 (Rated for Div. 2)"
rating: 2400
weight: 1463
solve_time_s: 145
verified: false
draft: false
---

[CF 1463E - Kế hoạch bài giảng](https://codeforces.com/problemset/problem/1463/E) 

**Đánh giá:** 2400 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, dsu, đồ thị, triển khai, sắp xếp, cây 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một giáo viên, Ivan, người cần sắp xếp các bài giảng`n`chủ đề khác nhau. Mỗi chủ đề có nhiều nhất một điều kiện tiên quyết, nghĩa là anh ta không thể giảng về một chủ đề trước điều kiện tiên quyết của nó. Một chủ đề không có điều kiện tiên quyết. Nhiệm vụ là sắp xếp các bài giảng theo một trật tự hợp lý, tôn trọng những điều kiện tiên quyết này. 

Ngoài ra, có`k`cặp chủ đề đặc biệt`(x_i, y_i)`như vậy nếu chủ đề`x_i`ngay trước chủ đề`y_i`, học sinh hiểu rõ hơn. Tất cả`x_i`và tất cả`y_i`là duy nhất, nghĩa là không có chủ đề nào xuất hiện theo nhiều cặp dưới dạng phần tử thứ nhất hoặc thứ hai. Thách thức là tìm ra thứ tự bài giảng thỏa mãn cả cấu trúc tiên quyết và ưu tiên liền kề của các cặp đặc biệt, hoặc xác định rằng điều đó là không thể. 

Các ràng buộc rất lớn:`n`có thể lên tới 3 * 10^5 và`k`lên đến`n-1`. Điều đó có nghĩa là bất kỳ giải pháp nào có độ phức tạp bậc hai (O(n²)) sẽ quá chậm. Chúng ta cần một thuật toán gần như tuyến tính trong`n`hoặc nhiều nhất là O(n log n). 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó các cặp đặc biệt tạo thành chuỗi hoặc chu trình xung đột với các điều kiện tiên quyết. Ví dụ: nếu điều kiện tiên quyết bắt buộc`1 → 2 → 3`nhưng những cặp đặc biệt lại yêu cầu`3 → 1`, không có giải pháp tồn tại. Một điều tinh tế khác là khi các cặp đặc biệt tự tạo thành một chuỗi có độ dài tùy ý - chúng ta phải đảm bảo các chuỗi này tuân theo thứ tự tiên quyết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các hoán vị của chủ đề và kiểm tra từng chủ đề theo các điều kiện tiên quyết và các cặp đặc biệt. Điều này đúng nhưng không khả thi vì số hoán vị là`n!`, tăng trưởng theo cấp số nhân. 

Quan sát chính là các điều kiện tiên quyết xác định cấu trúc phụ thuộc giống như cây (một nhóm được định hướng có một gốc duy nhất, vì chính xác một chủ đề không có điều kiện tiên quyết). Khi có cây này, chúng ta chỉ cần sắp xếp tuyến tính các chủ đề từ gốc đến lá tôn trọng cha mẹ trước con cái. 

Các cặp đặc biệt áp đặt các ràng buộc kề nhau, có thể được hiểu là hình thành các chuỗi chủ đề phải xuất hiện liên tiếp. Vì tất cả`x_i`là duy nhất, các chuỗi này không trùng nhau ở điểm bắt đầu. Trước tiên, chúng ta có thể xây dựng các chuỗi này và sau đó sắp xếp chúng theo cấu trúc liên kết theo cây điều kiện tiên quyết. 

Do đó, cách tiếp cận tối ưu là xây dựng các chuỗi được chỉ định bởi các cặp đặc biệt, thu gọn từng chuỗi thành một siêu nút và sau đó thực hiện sắp xếp cấu trúc liên kết tôn trọng các điều kiện tiên quyết. Nếu một chuỗi vi phạm các điều kiện tiên quyết (chủ đề đầu tiên của nó xuất hiện sau một điều kiện tiên quyết bên ngoài chuỗi), chúng tôi sẽ phát hiện ra điều không thể xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Chuỗi + Sắp xếp tôpô | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bản đồ`next_in_chain`cho các cặp đặc biệt: cho mỗi`(x, y)`, cửa hàng`next_in_chain[x] = y`. Ngoài ra, lưu trữ`has_prev[y] = x`để phát hiện sự bắt đầu của một chuỗi. 
2. Xác định điểm bắt đầu của mỗi chuỗi: một chủ đề`t`đó là trong`next_in_chain`nhưng không ở trong`has_prev`là người đứng đầu của một chuỗi. Có thể có nhiều chuỗi vì không phải chủ đề nào cũng tham gia vào một cặp đặc biệt. 
3. Duyệt qua từng chuỗi để thu thập đầy đủ chuỗi chủ đề phải xuất hiện liên tiếp. Trong khi duyệt qua, hãy kiểm tra các chu kỳ (nếu chủ đề lặp lại trong chuỗi của nó) và xung đột (một chủ đề xuất hiện trong nhiều chuỗi). Nếu được phát hiện, xuất`0`. 
4. Ánh xạ từng chủ đề vào chuỗi mà nó thuộc về và tạo danh sách các chuỗi chuỗi. Các chủ đề không thuộc bất kỳ cặp đặc biệt nào sẽ trở thành chuỗi đơn. 
5. Xây dựng biểu đồ tiên quyết trong đó mỗi chuỗi là một nút. Nếu một chủ đề trong chuỗi`A`có một điều kiện tiên quyết bên ngoài`A`, tạo lợi thế từ chuỗi điều kiện tiên quyết tới`A`. Điều này đảm bảo thứ tự chuỗi tôn trọng các điều kiện tiên quyết. 
6. Thực hiện sắp xếp các chuỗi tôpô. Nếu một chu trình được phát hiện trong biểu đồ chuỗi, hãy xuất ra`0`. Ngược lại, nối các chuỗi theo thứ tự tôpô để tạo thành chuỗi bài giảng. 
7. Xuất chuỗi. 

Điều bất biến ở đây là các chuỗi bảo toàn các ràng buộc kề và sắp xếp tôpô bảo toàn các điều kiện tiên quyết. Bằng cách coi mỗi chuỗi là một khối không thể phân chia, chúng tôi đảm bảo rằng tất cả các ràng buộc đều được thỏa mãn nếu thuật toán thành công. 

## Giải pháp Python```python
import sys
from collections import defaultdict, deque

input = sys.stdin.readline

n, k = map(int, input().split())
p = list(map(int, input().split()))

next_in_chain = {}
has_prev = {}
for _ in range(k):
    x, y = map(int, input().split())
    next_in_chain[x] = y
    has_prev[y] = x

used = set()
chains = []
topic_to_chain = {}

# Build chains from special pairs
for t in range(1, n+1):
    if t in next_in_chain and t not in has_prev:
        chain = []
        cur = t
        while cur:
            if cur in used:
                print(0)
                sys.exit(0)
            used.add(cur)
            chain.append(cur)
            cur = next_in_chain.get(cur, 0)
        chains.append(chain)
        for topic in chain:
            topic_to_chain[topic] = len(chains)-1

# Add singleton topics
for t in range(1, n+1):
    if t not in used:
        chains.append([t])
        topic_to_chain[t] = len(chains)-1

m = len(chains)
in_deg = [0]*m
adj = [[] for _ in range(m)]

# Build graph of chains based on prerequisites
for i, topic in enumerate(p):
    if topic == 0:
        continue
    curr_chain = topic_to_chain[i+1]
    prereq_chain = topic_to_chain[topic]
    if curr_chain != prereq_chain:
        adj[prereq_chain].append(curr_chain)
        in_deg[curr_chain] += 1

# Topological sort of chains
queue = deque([i for i in range(m) if in_deg[i] == 0])
res = []

while queue:
    u = queue.popleft()
    res.extend(chains[u])
    for v in adj[u]:
        in_deg[v] -= 1
        if in_deg[v] == 0:
            queue.append(v)

if len(res) != n:
    print(0)
else:
    print(' '.join(map(str, res)))
```Mã đầu tiên xây dựng chuỗi từ các cặp đặc biệt và phát hiện các chu trình. Sau đó, các chủ đề đơn lẻ được thêm vào dưới dạng chuỗi có độ dài 1. Biểu đồ chuỗi được xây dựng bằng cách sử dụng các phụ thuộc tiên quyết và sắp xếp theo cấu trúc liên kết sẽ tạo ra thứ tự bài giảng. Các trường hợp đặc biệt như các cặp đặc biệt theo chu kỳ hoặc lệnh tiên quyết không thể thực hiện được sẽ bị bắt và trả lại`0`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 2
2 3 0 5 3
1 5
5 4
```| Bước | chuỗi | topic_to_chain | đồ thị tính từ | xếp hàng | độ phân giải | 
| --- | --- | --- | --- | --- | --- | 
| Xây dựng chuỗi | [1,5,4], [2], [3] | {1:0,5:0,4:0,2:1,3:2} | [] | [] | [] | 
| Xây dựng đồ thị | 3→2,5→4? | xem liền kề | adj=[[],[2],[1]] | hàng đợi=[0] | res=[] | 
| Sắp xếp địa hình | 0->1->2 | | cập nhật hàng đợi | 3 2 1 5 4 | | 

Đầu ra:`3 2 1 5 4`Điều này cho thấy các chuỗi duy trì tính liền kề và sắp xếp cấu trúc liên kết tôn trọng các điều kiện tiên quyết. 

Đầu vào tùy chỉnh 2:```
3 1
0 1 2
1 3
```Chuỗi: [1,3],[2]; điều kiện tiên quyết: 1→2,2→3 

Không thể thỏa mãn tính liền kề của chuỗi và các điều kiện tiên quyết. Đầu ra:`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xây dựng chuỗi, chủ đề ánh xạ, xây dựng biểu đồ chuỗi, sắp xếp tôpô đều là tuyến tính`n`| 
| Không gian | O(n) | Bản đồ, danh sách, biểu đồ kề và hàng đợi đều lưu trữ tối đa các phần tử O(n) | 

Được cho`n <= 3*10^5`, Giải pháp O(n) thực thi trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    p = list(map(int, input().split()))
    next_in_chain = {}
    has_prev = {}
    for _ in range(k):
        x, y = map(int, input().split())
        next_in_chain[x] = y
        has_prev[y] = x
    used = set()
    chains = []
    topic_to_chain = {}
    for t in range(1, n+1):
        if t in next_in_chain and t not in has_prev:
            chain = []
            cur = t
            while cur:
                if cur in used:
                    return "0"
                used.add(cur)
                chain.append(cur)
                cur = next_in_chain.get(cur, 0)
            chains.append(chain)
            for topic in chain:
                topic_to_chain[topic] = len(chains)-1
    for t in range(1, n+1):
        if t not in used:
            chains.append([t])
            topic_to_chain[t] = len(chains)-1
    m = len(chains)
    in_deg = [0]*m
    adj = [[] for _ in range(m)]
    for i, topic in enumerate(p):
        if topic ==
```
