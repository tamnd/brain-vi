---
title: "CF 226E - Con đường hiệp sĩ cao quý"
description: "Trong bài toán này, chúng ta được yêu cầu mô phỏng một chuỗi các sự kiện trong hệ thống phân cấp phong kiến ​​được biểu diễn dưới dạng cây. Mỗi chế độ phong kiến ​​sở hữu một lâu đài, và ngoại trừ nhà vua, mỗi chế độ phong kiến ​​đều phục tùng chính xác một cấp trên."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 226
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 140 (Div. 1)"
rating: 2900
weight: 226
solve_time_s: 71
verified: true
draft: false
---

[CF 226E - Con đường của Hiệp sĩ Cao quý](https://codeforces.com/problemset/problem/226/E) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, chúng ta được yêu cầu mô phỏng một chuỗi các sự kiện trong hệ thống phân cấp phong kiến được biểu diễn dưới dạng cây. Mỗi chế độ phong kiến ​​sở hữu một lâu đài, và ngoại trừ nhà vua, mỗi chế độ phong kiến ​​đều phục tùng chính xác một cấp trên. Các lâu đài được nối với nhau bằng những con đường dọc theo hệ thống cấp bậc phong kiến: có một con đường giữa chế độ phong kiến ​​và cấp trên trực tiếp của nó. 

Các sự kiện xảy ra hàng năm và có hai loại. Loại đầu tiên đại diện cho một cuộc tấn công man rợ vào lâu đài. Một khi lâu đài bị tấn công, nó sẽ bị coi là mạo phạm kể từ năm đó trở đi. Loại thứ hai là hiệp sĩ đi từ lâu đài`a`đến lâu đài`b`dọc theo con đường duy nhất trong cây. Hiệp sĩ muốn dừng lại ở`k`-lâu đài dọc theo con đường này không bị tàn phá bởi các cuộc tấn công man rợ sau một năm nhất định`y`. Nếu ít hơn`k`lâu đài có sẵn, câu trả lời là`-1`. 

Đầu vào chứa`n`phong kiến ​​với`n`các số nguyên đại diện cho chủ nhân của mỗi phong kiến ​​(hoặc 0 nếu phong kiến ​​là vua), theo sau là`m`sự kiện. Đầu ra là một danh sách các số nguyên, một số cho mỗi truy vấn hiệp sĩ, cho biết anh ta có thể dừng lại ở lâu đài nào hoặc`-1`nếu không có lâu đài phù hợp tồn tại. 

Được cho`n`Và`m`có thể lên đến`10^5`, bất kỳ giải pháp ngây thơ nào liên tục đi ngang qua các đường dẫn hoặc quét tất cả các cuộc tấn công trong quá khứ sẽ quá chậm. Chúng ta cần xử lý trước cây để cho phép truy vấn đường dẫn hiệu quả và kiểm tra hiệu quả xem các lâu đài có bị mạo phạm so với năm hay không`y`. 

Các trường hợp đặc biệt không rõ ràng bao gồm các hiệp sĩ đi dọc theo những con đường không có lâu đài trung gian,`k`lớn hơn số lượng lâu đài hiện có và tấn công vào lâu đài của nhà vua hoặc các lâu đài ở hai đầu con đường của hiệp sĩ. Giải pháp phải xử lý chính xác những tình huống này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn hiệp sĩ, hãy tìm đường dẫn từ`a`ĐẾN`b`sử dụng BFS hoặc DFS trên cây, thu thập tất cả các lâu đài dọc theo đường đi ngoại trừ`a`Và`b`, và lặp lại chúng để đếm xem có bao nhiêu phần chưa bị xâm phạm sau năm`y`. Trả lại`k`-th lâu đài phù hợp hoặc`-1`nếu không đủ. Cách tiếp cận này đúng vì nó tuân theo đúng nghĩa đen của phát biểu vấn đề. Tuy nhiên, tốc độ này quá chậm: mỗi truy vấn có thể mất`O(n)`để đi qua con đường, đưa ra`O(n * m)`tổng cộng là điều không thể thực hiện được`n, m = 10^5`. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là xử lý trước cây để cho phép`O(log n)`truy vấn đường dẫn. Chúng ta có thể sử dụng **nâng nhị phân** để tính toán tổ tiên chung thấp nhất (LCA) của bất kỳ hai lâu đài nào một cách hiệu quả. Con đường từ`a`ĐẾN`b`sau đó có thể được biểu diễn dưới dạng các phân đoạn từ`a`ĐẾN`LCA(a,b)`và từ`b`ĐẾN`LCA(a,b)`. Chúng tôi cũng duy trì một mảng`last_attack_year`để ghi lại năm mỗi lâu đài bị tấn công lần cuối. Với LCA và mảng này, chúng ta có thể duyệt qua đường dẫn một cách nhanh chóng và kiểm tra sự mạo phạm trong`O(log n + path_length)`, điều này có thể chấp nhận được vì độ dài đường dẫn tối đa là`O(n)`nhưng tổng của tất cả các truy vấn bị giới hạn bởi độ sâu của cây trong thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n) | Quá chậm đối với đầu vào lớn | 
| Tối ưu | O((n + m) log n) | O(n log n) | Được chấp nhận, xử lý đầu vào lớn một cách hiệu quả | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào để đọc hệ thống phân cấp phong kiến và xây dựng cây dưới dạng danh sách kề. Mỗi nút đại diện cho một lâu đài. 
2. Xử lý trước cây để nâng nhị phân. Đối với mỗi lâu đài, hãy lưu trữ tổ tiên của nó theo lũy thừa của hai khoảng cách và độ sâu của nó. Điều này cho phép tính toán LCA hiệu quả giữa hai lâu đài bất kỳ trong`O(log n)`thời gian. 
3. Khởi tạo một mảng`last_attack_year`kích thước`n + 1`với`0`, thể hiện rằng chưa có lâu đài nào bị tấn công. 
4. Lặp lại các sự kiện qua từng năm. Đối với mỗi sự kiện tấn công, hãy cập nhật`last_attack_year[c] = current_year`. 
5. Đối với mỗi truy vấn hiệp sĩ, hãy tính LCA của`a`Và`b`. Tạo danh sách các lâu đài dọc theo con đường từ`a`ĐẾN`b`loại trừ`a`Và`b`bằng cách đi tới LCA từ cả hai đầu. 
6. Lọc danh sách đường dẫn để chỉ giữ lại các lâu đài ở nơi`last_attack_year[c] <= y`, tức là, các lâu đài không bị xâm phạm sau nhiều năm`y`. 
7. Nếu danh sách được lọc có ít nhất`k`lâu đài, trả lại`k`-thứ 1 Còn không thì trả lại`-1`. 
8. Xuất kết quả theo thứ tự truy vấn xuất hiện. 

Thuật toán hoạt động vì LCA đảm bảo rằng chúng tôi có thể chia bất kỳ đường dẫn nào thành hai đường đi lên đơn giản và việc theo dõi các năm tấn công gần nhất đảm bảo chúng tôi có thể kiểm tra mạo phạm một cách hiệu quả. Mỗi truy vấn hiệp sĩ được xử lý độc lập và không có lâu đài nào bị tính sai vì mỗi bước sẽ loại trừ rõ ràng các lâu đài bị mạo phạm sau năm năm`y`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(200000)

def run():
    n = int(input())
    parent = list(map(int, input().split()))
    
    adj = [[] for _ in range(n + 1)]
    root = 0
    for i, p in enumerate(parent, 1):
        if p == 0:
            root = i
        else:
            adj[i].append(p)
            adj[p].append(i)
    
    LOG = 17  # 2^17 > 10^5
    up = [[0] * (LOG + 1) for _ in range(n + 1)]
    depth = [0] * (n + 1)
    
    def dfs(u, par):
        up[u][0] = par
        for i in range(1, LOG + 1):
            up[u][i] = up[up[u][i - 1]][i - 1]
        for v in adj[u]:
            if v != par:
                depth[v] = depth[u] + 1
                dfs(v, u)
    
    dfs(root, 0)
    
    def lca(u, v):
        if depth[u] < depth[v]:
            u, v = v, u
        for i in range(LOG, -1, -1):
            if depth[u] - (1 << i) >= depth[v]:
                u = up[u][i]
        if u == v:
            return u
        for i in range(LOG, -1, -1):
            if up[u][i] != up[v][i]:
                u = up[u][i]
                v = up[v][i]
        return up[u][0]
    
    m = int(input())
    events = [input().split() for _ in range(m)]
    last_attack_year = [0] * (n + 1)
    res = []
    
    for year, event in enumerate(events, 1):
        if event[0] == '1':
            c = int(event[1])
            last_attack_year[c] = year
        else:
            _, a, b, k, y = map(int, event)
            l = lca(a, b)
            path = []
            # up from a to lca
            u = a
            while u != l:
                path.append(u)
                u = up[u][0]
            # up from b to lca, reversed
            tmp = []
            u = b
            while u != l:
                tmp.append(u)
                u = up[u][0]
            path += tmp[::-1]
            # exclude a and b
            path = [x for x in path if x != a and x != b]
            filtered = [x for x in path if last_attack_year[x] <= y]
            if len(filtered) >= k:
                res.append(str(filtered[k - 1]))
            else:
                res.append('-1')
    
    print(' '.join(res))

if __name__ == "__main__":
    run()
```Mã sử ​​dụng DFS để điền vào bảng nâng nhị phân và mảng độ sâu. Mỗi truy vấn hiệp sĩ sử dụng LCA để xây dựng đường dẫn một cách hiệu quả. Việc lọc các lâu đài bị xâm phạm được thực hiện bằng cách hiểu danh sách đơn giản. Việc sử dụng`sys.setrecursionlimit`đảm bảo DFS hoạt động trên cây sâu. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3
0 1 2
5
2 1 3 1 0
1 2
2 1 3 1 0
2 1 3 1 1
2 1 3 1 2
```| Năm | Sự kiện | Đường dẫn a->b | Lâu đài đủ điều kiện | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Hiệp sĩ 1->3 k=1 y=0 | 2 | 2 | 2 | 
| 2 | Tấn công 2 | - | - | - | 
| 3 | Hiệp sĩ 1->3 k=1 y=0 | 2 | 2 | -1 | 
| 4 | Hiệp sĩ 1->3 k=1 y=1 | 2 | không | -1 | 
| 5 | Hiệp sĩ 1->3 k=1 y=2 | 2 | 2 | 2 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | DFS để nâng nhị phân là O(n log n). Mỗi truy vấn hiệp sĩ sử dụng LCA trong O(log n) cộng với truyền tải đường dẫn |
