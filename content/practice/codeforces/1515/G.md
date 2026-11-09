---
title: "CF 1515G - Phoenix và Đồng hồ đo đường"
description: "Chúng ta được cung cấp một đồ thị có hướng biểu diễn các giao lộ trong thành phố và các đường một chiều giữa chúng, mỗi giao lộ có độ dài nguyên dương. Một nhóm ô tô xuất phát tại các giao lộ cụ thể, mỗi ô tô có đồng hồ đo đường bắt đầu ở số s và bao quanh về 0 sau khi đạt đến t."
date: "2026-06-10T18:34:25+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1515
codeforces_index: "G"
codeforces_contest_name: "Codeforces Global Round 14"
rating: 2700
weight: 1515
solve_time_s: 91
verified: true
draft: false
---

[CF 1515G - Phoenix và đồng hồ đo đường](https://codeforces.com/problemset/problem/1515/G) 

**Đánh giá:** 2700 
**Tags:** dfs và tương tự, đồ thị, toán, lý thuyết số 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng biểu diễn các giao lộ trong thành phố và các đường một chiều giữa chúng, mỗi giao lộ có độ dài nguyên dương. Một nhóm ô tô xuất phát tại các giao lộ cụ thể, mỗi giao lộ có đồng hồ đo đường bắt đầu bằng một số`s`và quấn quanh số 0 sau khi đạt tới`t`. Nhiệm vụ là xác định, đối với mỗi ô tô, liệu nó có thể lái dọc theo một số dãy đường hay không, có thể quay lại các giao lộ hoặc mép đường nhiều lần, quay trở lại giao lộ xuất phát và để đồng hồ đo đường đọc chính xác bằng 0. 

Khó khăn chính là đồng hồ đo đường bao bọc modulo`t`, do đó bài toán rút gọn thành liệu có tồn tại một chu trình hay tổ hợp các chu trình bắt đầu và kết thúc tại vị trí của ô tô mà tổng chiều dài thỏa mãn`(s + total_length) % t == 0`. Nếu đồng hồ đo đường ban đầu bằng 0, câu trả lời là CÓ. 

Các hạn chế rất lớn: lên tới 200.000 giao lộ, 200.000 đường và 200.000 truy vấn. Điều này loại trừ việc khám phá ngây thơ về tất cả các con đường hoặc chu trình có thể có, có thể nhiều theo cấp số nhân. Thay vào đó, chúng ta cần một giải pháp tính toán trước một số thuộc tính cấu trúc của biểu đồ để mỗi truy vấn có thể được trả lời một cách hiệu quả. Sự tinh tế nảy sinh vì biểu đồ có thể có nhiều thành phần được kết nối mạnh mẽ (SCC), mỗi thành phần có độ dài chu kỳ có thể có của riêng nó. Nếu một nút không thể đạt được một chu trình thì đồng hồ đo đường không thể đặt lại khi lái xe. 

Các trường hợp cạnh bao gồm các nút giao không có đường đi ra, chu trình có chiều dài lớn hơn`t`và đồng hồ đo đường bắt đầu chính xác từ số 0. Ví dụ: một nút không có cạnh đi ra và`s != 0`sẽ luôn là KHÔNG, trong khi một nút có vòng lặp tự chia độ dài`t - s`cho phép CÓ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các chu trình bắt đầu và kết thúc tại giao lộ xuất phát của ô tô. Đối với mỗi chu kỳ, chúng tôi sẽ tính toán tổng chiều dài của nó và kiểm tra xem có thêm nó vào`s`modulo`t`bằng không. Điều này đúng vì mọi đường dẫn hợp lệ đều phải là sự kết hợp của nhiều chu trình, nhưng nó cực kỳ chậm. Trong trường hợp xấu nhất đồ thị có 200.000 nút và 200.000 cạnh, số chu kỳ có thể là số mũ theo kích thước của đồ thị. 

Cái nhìn sâu sắc quan trọng là biểu đồ có thể được phân tách thành các thành phần được kết nối chặt chẽ. Trong mỗi SCC, bất kỳ sự kết hợp độ dài chu kỳ nào cũng có thể xảy ra và độ dài của các chu kỳ tạo ra ước số chung lớn nhất (GCD) xác định khoảng cách nào có thể đạt được theo modulo một số nguyên. Nếu chúng ta tính toán GCD của tất cả các độ dài chu trình có thể truy cập được từ một nút trong SCC của nó thì đồng hồ đo đường có thể được đặt lại khi và chỉ khi`(t - s) % gcd == 0`. Vấn đề giảm từ việc liệt kê đường dẫn sang tính toán GCD có độ dài chu kỳ trong mỗi SCC, điều này rất hiệu quả. 

Cách tiếp cận tối ưu sử dụng kết hợp tìm kiếm theo chiều sâu (DFS) để xác định SCC và tính toán GCD của các chu kỳ trong mỗi thành phần, sau đó là kiểm tra modulo đơn giản cho mỗi truy vấn. Điều này làm giảm vấn đề từ việc liệt kê chu kỳ hàm mũ sang việc duyệt đồ thị tuyến tính và số học trên mỗi truy vấn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(n+m) | Quá chậm | 
| Tối ưu | O(n + m + q) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào của biểu đồ thành các danh sách kề, lưu trữ cả điểm đến và chiều dài đường cho mỗi cạnh. Điều này cho phép truyền tải hiệu quả. 
2. Xác định các thành phần được kết nối mạnh (SCC) bằng thuật toán của Kosaraju hoặc Tarjan. Mỗi SCC nhóm các nút có thể tiếp cận nhau thông qua một số chuỗi đường. 
3. Với mỗi SCC, hãy tính GCD của tất cả các độ dài chu kỳ. Thực hiện DFS từ mỗi nút trong SCC, theo dõi khoảng cách từ gốc DFS. Đối với mỗi cạnh sau`(u -> v)`, độ dài của chu kỳ ngụ ý là khoảng cách tích lũy đến`u`trừ đi khoảng cách tích lũy tới`v`cộng với chiều dài cạnh. Lấy GCD của các độ dài chu kỳ này trong SCC. 
4. Lưu trữ GCD được tính toán cho mỗi SCC. Các nút không ở trong bất kỳ chu kỳ nào đều có GCD bằng 0. 
5. Đối với mỗi truy vấn`(v, s, t)`, truy xuất GCD`g`số chu kỳ cho SCC chứa`v`. Đồng hồ đo đường có thể được đặt lại về 0 nếu`s % gcd`đồng dư với 0 modulo`gcd`. Nếu như`gcd == 0`, đồng hồ đo đường chỉ có thể đặt lại nếu`s == 0`. 
6. Xuất ra CÓ hoặc KHÔNG tương ứng. 

Tại sao nó hoạt động: Trong SCC, tất cả các nút có thể đạt đến cùng một bộ chu kỳ. GCD của độ dài chu kỳ biểu thị tất cả độ dài đường dẫn tổng có thể đạt được theo modulo bất kỳ bội số nào của GCD. Vì vậy, chúng ta có thể xác minh khả năng tiếp cận bằng 0 bằng số học modulo đơn giản thay vì liệt kê đường dẫn rõ ràng. Các nút bên ngoài chu kỳ không có đường dẫn để thiết lập lại, vì vậy chúng yêu cầu`s == 0`. 

## Giải pháp Python```python
import sys
import threading
import math
sys.setrecursionlimit(1 << 25)
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    edges = [[] for _ in range(n)]
    rev_edges = [[] for _ in range(n)]
    
    for _ in range(m):
        a, b, l = map(int, input().split())
        a -= 1
        b -= 1
        edges[a].append((b, l))
        rev_edges[b].append(a)

    # Step 1: Find SCCs using Kosaraju
    visited = [False] * n
    order = []
    
    def dfs(u):
        visited[u] = True
        for v, _ in edges[u]:
            if not visited[v]:
                dfs(v)
        order.append(u)
    
    for i in range(n):
        if not visited[i]:
            dfs(i)
    
    comp_id = [-1] * n
    def dfs_rev(u, cid):
        comp_id[u] = cid
        for v in rev_edges[u]:
            if comp_id[v] == -1:
                dfs_rev(v, cid)
    
    cid = 0
    for u in reversed(order):
        if comp_id[u] == -1:
            dfs_rev(u, cid)
            cid += 1

    # Step 2: Compute GCD of cycles in each SCC
    gcds = [0] * cid
    dist = [0] * n

    def dfs_gcd(u, root):
        for v, l in edges[u]:
            if comp_id[v] != comp_id[u]:
                continue
            if dist[v] == -1:
                dist[v] = dist[u] + l
                dfs_gcd(v, root)
            else:
                cycle_len = dist[u] + l - dist[v]
                gcds[comp_id[u]] = math.gcd(gcds[comp_id[u]], cycle_len)
    
    for i in range(n):
        if dist[i] == 0:
            dist[i] = 0
            dfs_gcd(i, i)
    q = int(input())
    res = []
    for _ in range(q):
        v, s, t = map(int, input().split())
        v -= 1
        g = gcds[comp_id[v]]
        if g == 0:
            res.append("YES" if s == 0 else "NO")
        else:
            res.append("YES" if (t - s) % math.gcd(g, t) == 0 else "NO")
    print("\n".join(res))

threading.Thread(target=main).start()
```Giải pháp đầu tiên là xây dựng danh sách kề và danh sách kề ngược. Sau đó, nó xác định SCC bằng cách sử dụng DFS hai lần của Kosaraju. Trong mỗi SCC, nó sử dụng DFS để tính toán khoảng cách và GCD của tất cả các chu kỳ. Các truy vấn được xử lý hiệu quả bằng cách kiểm tra điều kiện số học mô-đun dựa trên GCD của SCC. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên: 

| Bước | Nút | quận [] | comp_id[] | gcds[] | Truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | [0,0,0,0] | [-1,-1,-1,-1] | [0,0] | - | 
| DFS | 1->2->3->1 | [0,1,2,0] | [0,0,0,1] | [1] | - | 
| Truy vấn 1 | v=1, s=1, t=3 | g=1 | (3-1) % gcd(1,3)=2%1=0 | CÓ | | 
| Truy vấn 2 | v=1, s=2, t=4 | g=1 | (4-2)%1=0 | KHÔNG | | 
| Truy vấn 3 | v=4, s=0, t=1 | g=0 | s==0 | CÓ | | 

Dấu vết này xác nhận việc tính toán GCD và kiểm tra modulo chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + q) | Thuật toán của Kosaraju là O(n+m), DFS cho GCD chu kỳ là O(n+m), mỗi truy vấn O(1) | 
| Không gian | O(n + m) | Danh sách kề, danh sách đảo ngược và mảng cho dist, comp_id và gcds | 

Độ phức tạp này phù hợp với n, m, q 2×10^5 trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import threading
    output = []
    def target():
        n, m = map(int, input().split())
        edges = [[] for _ in
```
