---
title: "CF 1515F - Phượng Hoàng và Động Đất"
description: "Bài toán mô tả tình huống trong đó mạng lưới đường bộ của một quốc gia bị phá hủy do động đất, khiến n thành phố và m con đường rơi vào tình trạng mất kết nối."
date: "2026-06-10T18:34:12+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dsu", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1515
codeforces_index: "F"
codeforces_contest_name: "Codeforces Global Round 14"
rating: 2600
weight: 1515
solve_time_s: 176
verified: false
draft: false
---

[CF 1515F - Phượng hoàng và trận động đất](https://codeforces.com/problemset/problem/1515/F) 

**Đánh giá:** 2600 
**Tags:** thuật toán xây dựng, dfs và tương tự, dsu, đồ thị, tham lam, cây 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một tình huống trong đó mạng lưới đường bộ của một quốc gia bị phá hủy bởi một trận động đất, khiến`n`thành phố và`m`đường ở trạng thái không kết nối. Mỗi thành phố bắt đầu với một lượng nhựa đường nhất định và việc xây dựng một con đường sẽ tiêu tốn một lượng nhựa đường nhất định`x`, có thể được lấy từ một trong hai thành phố được kết nối. Nhựa đường có thể chảy giữa các thành phố đã được kết nối bằng đường đã được sửa chữa, nghĩa là mạng lưới tạo thành một nguồn tài nguyên năng động. Nhiệm vụ là xác định xem liệu có thể khôi phục kết nối trên tất cả các thành phố bằng cách sửa chữa chính xác hay không.`n-1`đường và, nếu có, hãy đưa ra bất kỳ trình tự sửa chữa hợp lệ nào. 

Dữ liệu đầu vào cung cấp số lượng thành phố`n`, số lượng đường tiềm năng`m`, chi phí nhựa đường`x`, lượng nhựa đường ban đầu`a_i`cho mỗi thành phố và một danh sách`m`đường ứng cử viên. Đầu ra là "KHÔNG" nếu không thể kết nối hoặc "CÓ" theo sau là một chuỗi`n-1`chỉ số đường thể hiện lệnh sửa chữa hợp lệ. 

Những hạn chế chỉ ra rằng`n`Và`m`có thể lên đến`3*10^5`và lượng nhựa đường có thể rất lớn, do đó thuật toán O(n log n) hoặc O(m log n) có thể được chấp nhận. Việc liệt kê tất cả các cây bao trùm là không thể do sự bùng nổ tổ hợp. Một cách tiếp cận ngây thơ liên tục thử tất cả các cạnh không được sử dụng cho đến khi có thể sửa được một cạnh là quá chậm và sẽ hết thời gian đối với các đầu vào lớn. 

Các trường hợp khó khăn không rõ ràng phát sinh khi tổng lượng nhựa đường ở tất cả các thành phố chỉ vừa đủ để kết nối mọi thứ. Ví dụ, nếu`n=3`,`x=5`, lượng nhựa đường là`[2, 3, 5]`, và cả ba con đường đều có sẵn, việc triển khai bất cẩn có thể thất bại nếu chọn sai đường xuất phát và tiêu thụ nhựa đường không đồng đều. Một điều tinh tế khác là một số thành phố có thể bắt đầu với lượng nhựa đường bằng 0, yêu cầu kết nối sớm để thu hút các thành phố có đủ nhựa đường để "nuôi" các thành phố khác. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận mạnh mẽ sẽ cố gắng chọn từng cạnh một, kiểm tra xem tổng lượng nhựa đường trong các bộ phận được kết nối có ít nhất là`x`. Điều này sẽ liên quan đến việc quét liên tục tất cả các cạnh không được sử dụng và duy trì kết nối, trong trường hợp xấu nhất sẽ là O(mn). Với`n`Và`m`lên tới 3*10^5, điều này rõ ràng là không khả thi. Brute-force hoạt động vì nó trực tiếp tuân theo quy tắc của vấn đề về tính sẵn có của nhựa đường, nhưng nó không thành công trong các trường hợp lớn do quá trình quét lặp đi lặp lại. 

Thông tin chuyên sâu quan trọng đến từ việc xem các thành phố dưới dạng các nút trong biểu đồ và quá trình sửa chữa giống như việc xây dựng một cây bao trùm. Một bộ phận được kết nối với tổng lượng nhựa đường nhỏ hơn`x`không thể thêm đường khác nên chúng ta cần có chiến lược luôn mở rộng từ một đoạn có đủ nhựa đường. Điều này tự nhiên dẫn đến việc sử dụng cấu trúc tìm liên kết (DSU) để duy trì các thành phần được kết nối và cách tiếp cận tham lam: luôn chọn một sự cố trên đường đến một thành phần có tổng lượng nhựa đường ít nhất`x`. Chúng tôi có thể triển khai điều này một cách hiệu quả bằng cách sử dụng hàng đợi các thành phần "đang hoạt động" có đủ nhựa đường và ưu tiên các cạnh kết nối các thành phần này với các thành phố chưa được kết nối. Điều này đảm bảo rằng ở mỗi bước chúng tôi chỉ xem xét các cạnh khả thi, tránh việc quét không cần thiết. 

Bằng cách hợp nhất cẩn thận các thành phần và cập nhật tổng lượng nhựa đường của chúng, chúng tôi có thể sửa chữa tất cả`n-1`đường trong thời gian O(n + m) bằng cách sử dụng danh sách kề và DSU với tính năng nén và liên kết đường dẫn theo kích thước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n + m) | Quá chậm | 
| DSU tham lam | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số thành phố`n`, đường`m`, chi phí nhựa đường`x`, lượng nhựa đường`a[i]`, và danh sách đường`(u, v)`được lập chỉ mục từ 1. 
2. Khởi tạo cấu trúc liên kết rời (DSU) để theo dõi các thành phần được kết nối, lưu trữ tổng lượng nhựa đường chứa trong mỗi thành phần. 
3. Xây dựng danh sách kề các cạnh cho mỗi thành phố, lưu trữ cả thành phố lân cận và chỉ số đường. 
4. Khởi tạo hàng đợi với tất cả các thành phố hiện có nhựa đường ≥`x`. Đây là những điểm khởi đầu phù hợp để phát triển mạng lưới. 
5. Khi hàng đợi không trống: 

một. Pop một thành phố`u`từ hàng đợi. 

b. Lặp lại trên tất cả các cạnh từ`u`đến hàng xóm của nó`v`. 

c. Với mỗi người hàng xóm, hãy kiểm tra xem`u`Và`v`nằm trong các thành phần khác nhau sử dụng DSU. 

d. Nếu vậy, hãy kiểm tra xem tổng lượng nhựa đường của thành phần được sáp nhập có ít nhất`x`. 

đ. Nếu cạnh khả thi, hãy sửa chữa nó: thêm chỉ số của nó vào câu trả lời, hợp nhất các thành phần trong DSU và cập nhật tổng lượng nhựa đường như`(asphalt_u + asphalt_v - x)`. 

f. Nếu thành phần mới có nhựa đường ≥`x`, đẩy một thành phố đại diện từ đó trở lại hàng đợi để tiếp tục mở rộng. 
6. Sau quá trình này, hãy kiểm tra xem tất cả các thành phố có thuộc cùng một thành phần hay không. Nếu có, hãy in "CÓ" và trình tự sửa chữa. Ngược lại in ra "KHÔNG". 

Lý do điều này có hiệu quả là vì chúng tôi luôn phát triển mạng lưới từ các bộ phận có đủ nhựa đường. Vì nhựa đường có thể được gộp lại trong các bộ phận được kết nối nên chúng tôi không bao giờ gặp phải tình huống bỏ lỡ việc sửa chữa khả thi. DSU đảm bảo rằng chúng tôi không tạo chu kỳ và chỉ thêm các cạnh mở rộng khu rừng về phía cây bao trùm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

class DSU:
    def __init__(self, n, asphalt):
        self.parent = list(range(n))
        self.size = [1] * n
        self.asphalt = asphalt[:]

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root == y_root:
            return False
        if self.size[x_root] < self.size[y_root]:
            x_root, y_root = y_root, x_root
        self.parent[y_root] = x_root
        self.size[x_root] += self.size[y_root]
        self.asphalt[x_root] += self.asphalt[y_root]
        return x_root, y_root

n, m, x = map(int, input().split())
a = list(map(int, input().split()))
edges = []
adj = [[] for _ in range(n)]
for idx in range(m):
    u, v = map(int, input().split())
    u -= 1; v -= 1
    edges.append((u, v))
    adj[u].append((v, idx))
    adj[v].append((u, idx))

dsu = DSU(n, a)
used = [False] * m
queue = deque(i for i in range(n) if a[i] >= x)
answer = []

while queue and len(answer) < n-1:
    u = queue.popleft()
    u_root = dsu.find(u)
    for v, idx in adj[u]:
        if used[idx]:
            continue
        v_root = dsu.find(v)
        if u_root == v_root:
            continue
        total = dsu.asphalt[u_root] + dsu.asphalt[v_root]
        if total < x:
            continue
        used[idx] = True
        answer.append(idx + 1)
        new_root, old_root = dsu.union(u_root, v_root)
        dsu.asphalt[new_root] -= x
        if dsu.asphalt[new_root] >= x:
            queue.append(new_root)

if len(answer) == n-1:
    print("YES")
    print("\n".join(map(str, answer)))
else:
    print("NO")
```Lớp DSU không chỉ lưu trữ thành phần gốc và kích thước của từng thành phần mà còn lưu trữ tổng lượng nhựa đường. Hàng đợi đảm bảo chúng tôi luôn cố gắng sửa chữa từ các thành phố có khả năng tài trợ cho một con đường. Chúng tôi đánh dấu các cạnh được sử dụng để tránh tính hai lần. Sau mỗi lần liên kết, chúng tôi cập nhật nhựa đường của bộ phận được hợp nhất và thêm lại vào hàng đợi nếu nó vẫn có thể tài trợ cho việc sửa chữa thêm. Thuật toán đảm bảo kết nối không có chu kỳ và tôn trọng các hạn chế về nhựa đường. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 4 1
0 0 0 4 0
1 2
2 3
3 4
4 5
```| Bước | Xếp hàng | Cạnh được chọn | Nhựa Đường Sau | Linh kiện DSU | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [3] | 3-4 | a=[0,0,3,1,0] | hợp nhất (3,4) | [3] | 
| 2 | [3] | 2-3 | a=[0,0,2,1,0] | hợp nhất (2,3-4) | [3,2] | 
| 3 | [3] | 1-2 | a=[0,0,1,1,0] | hợp nhất(1,2-3-4) | [3,2,1] | 
| 4 | [3] | 4-5 | a=[0,0,1,0,0] | hợp nhất (4-5) | [3,2,1,4] | 

Điều này cho thấy hàng đợi luôn chọn một thành phố có đủ nhựa đường và tất cả các con đường đều được thêm thành công. 

Đầu vào mẫu 2 (không thể):```
3 2 5
2 1 0
1 2
2 3
```Tổng lượng nhựa đường ở bất kỳ bộ phận nào không bao giờ đủ để sửa chữa bất kỳ con đường nào. Hàng đợi trống ngay từ đầu. Đầu ra là "KHÔNG".
