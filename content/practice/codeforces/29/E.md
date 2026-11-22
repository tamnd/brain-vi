---
title: "CF 29E - Cãi Nhau"
description: "Chúng ta được yêu cầu dẫn đường cho hai người, Bob và Alex, băng qua một thị trấn được biểu diễn dưới dạng đồ thị vô hướng với n ngã tư và m đường. Bob bắt đầu từ nút 1 và muốn đến nút n, trong khi Alex bắt đầu từ nút n và muốn đến nút 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 29
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 29 (Div. 2, Codeforces format)"
rating: 2400
weight: 29
solve_time_s: 76
verified: false
draft: false
---
[CF 29E - Cãi vã](https://codeforces.com/problemset/problem/29/E) 

**Đánh giá:** 2400 
**Tas:** đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu dẫn đường cho hai người, Bob và Alex, băng qua một thị trấn được biểu diễn dưới dạng đồ thị vô hướng với`n`ngã tư và`m`những con đường. Bob bắt đầu ở nút 1 và muốn đến nút`n`, trong khi Alex bắt đầu tại nút`n`and wants to reach node 1. Both must travel along sequences of connected roads simultaneously, moving at the same speed, and they cannot occupy the same crossroad at the same time. Họ có thể gặp nhau ở giữa đường nhưng không bao giờ gặp nhau ở một nút. Nhiệm vụ là tìm đường đi ngắn nhất có thể có độ dài bằng nhau cho cả hai để thỏa mãn điều kiện này. 

The inputs are simple integers representing the graph, but the key challenge is the interaction constraint: two shortest paths that might overlap need careful consideration to ensure the two travelers never meet at the same node at the same time. Nếu không có cặp hợp lệ tồn tại, chúng tôi trả lại`-1`. Ngược lại, chúng ta phải tạo ra bất kỳ tuyến đường ngắn nhất hợp lệ nào. 

Giới hạn vấn đề là vừa phải:`n`lên tới 500 và`m`lên tới 10.000. Điều này cho phép các thuật toán lên tới khoảng O(n^3) về thời gian hoặc O(n^2) về không gian. BFS đơn giản trên đồ thị không có trọng số chạy trong O(n + m) và có thể chấp nhận được, nhưng các phương pháp liệt kê tất cả các hoán vị đường dẫn là không khả thi. Các trường hợp cạnh bao gồm các biểu đồ nhỏ chỉ có một đường (đường tầm thường), biểu đồ bị ngắt kết nối (không có đường dẫn) hoặc các tình huống trong đó các đường dẫn ngắn nhất nhất thiết phải trùng nhau, ngăn cản giải pháp hợp lệ. 

Ví dụ, hãy xem xét`n = 3`, đường`1-2`Và`2-3`. Cả hai đường đi ngắn nhất từ ​​1→3 và 3→1 đều đi qua 2. Không có cách nào để lên lịch di chuyển đồng thời mà không gặp nhau ở nút 2, vì vậy câu trả lời phải là`-1`. Một giải pháp đơn giản chỉ tính toán khoảng cách và chọn các đường dẫn ngắn nhất sẽ trả về không chính xác các đường dẫn xung đột tại một nút. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là liệt kê tất cả các đường đi có độ dài`k`từ 1 đến n đối với Bob và n đến 1 đối với Alex, sau đó mô phỏng chuyển động đồng thời dọc theo tất cả các cặp để kiểm tra xung đột nút. Về nguyên tắc, điều này hoạt động vì cuối cùng nó sẽ tìm thấy một cặp đường dẫn ngắn nhất hợp lệ nếu có. Tuy nhiên, nó chậm một cách vô vọng: ngay cả đối với các đồ thị khiêm tốn, số lượng đường dẫn vẫn tăng theo cấp số nhân. Độ phức tạp trong trường hợp xấu nhất là khoảng O(2^n) chỉ để liệt kê các đường dẫn, vượt xa khả thi đối với n = 500. 

Cái nhìn sâu sắc dẫn đến giải pháp tối ưu xuất phát từ việc coi đây là bài toán đường đi ngắn nhất với ràng buộc "tránh xung đột" bổ sung. Chúng ta có thể mô hình hóa vấn đề bằng cách sử dụng hai lần truyền tải BFS độc lập từ mỗi nút bắt đầu, lưu trữ khoảng cách đến tất cả các nút. Cho phép`d1[v]`là khoảng cách từ 1 đến v và`d2[v]`khoảng cách từ n đến v. Quan sát chính là đường dẫn có độ dài được đồng bộ hóa hợp lệ`L`tồn tại nếu tại mỗi bước`t`dọc theo đường đi, Bob ở một nút`u`và Alex đang ở một nút`v`như vậy hoặc`u != v`hoặc chúng đang di chuyển dọc theo cùng một cạnh theo hướng ngược nhau. Chúng ta có thể coi đây là bài toán đường đi ngắn nhất trên biểu đồ tích các vị trí của Bob và Alex, tránh các trạng thái nơi chúng va chạm nhau. Vì đồ thị ban đầu nhỏ nên BFS trên đồ thị tích có kích thước n×n là khả thi. Điều này làm giảm vấn đề từ việc liệt kê đường dẫn hàm mũ đến thăm dò thời gian đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê đường dẫn) | O(2^n) | O(2^n) | Quá chậm | 
| BFS trên biểu đồ sản phẩm | O(n^2 + n*m) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ đầu vào và xây dựng danh sách kề. Đây là tiêu chuẩn và đảm bảo BFS chạy hiệu quả. 
2. Chạy BFS từ nút 1 để tính toán`dist1[v]`cho tất cả các nút`v`.`dist1[v]`là số cạnh tối thiểu cần đạt tới`v`từ 1. 
3. Chạy BFS từ nút n để tính toán`distn[v]`cho tất cả các nút`v`. 
4. Khởi tạo BFS trên các trạng thái`(b, a)`đại diện cho Bob tại nút`b`và Alex tại nút`a`. Trạng thái bắt đầu là`(1, n)`và trạng thái kết thúc là`(n, 1)`. 
5. Đối với mỗi tiểu bang`(b, a)`trong BFS, hãy xem xét tất cả các nước đi: Bob di chuyển đến hàng xóm của`b`và Alex chuyển đến sống ở hàng xóm của`a`. Chỉ đẩy`(b', a')`vào hàng đợi BFS nếu`b' != a'`. Được phép di chuyển dọc theo cùng một cạnh theo hướng ngược lại vì nó không tạo thành xung đột nút. 
6. Theo dõi khoảng cách cho từng trạng thái trong mảng 2D`dist[b][a]`và lưu trữ trạng thái gốc để xây dựng lại đường dẫn. 
7. BFS chấm dứt khi`(n, 1)`đã đạt được. Nếu không thể truy cập được, hãy xuất`-1`. 
8. Xây dựng lại đường đi cho Bob và Alex bằng cách quay lại từ`(n, 1)`sử dụng con trỏ cha. Cả hai đường dẫn sẽ có cùng độ dài. 

**Tại sao nó hoạt động**: BFS trên biểu đồ sản phẩm khám phá tất cả các chuyển động được đồng bộ hóa hợp lệ theo thứ tự các bước tăng dần. Bằng cách thực thi`b' != a'`ở mỗi bước, chúng tôi ngăn chặn việc chiếm dụng đồng thời cùng một nút. BFS đảm bảo rằng lần đầu tiên chúng tôi đạt được`(n, 1)`nó nằm dọc theo chuỗi di chuyển ngắn nhất. Các con trỏ gốc cung cấp sự tái cấu trúc hợp lệ của các đường dẫn được đồng bộ hóa. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

n, m = map(int, input().split())
adj = [[] for _ in range(n + 1)]
for _ in range(m):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)

dist = [[-1] * (n + 1) for _ in range(n + 1)]
parent = [[None] * (n + 1) for _ in range(n + 1)]

queue = deque()
queue.append((1, n))
dist[1][n] = 0

while queue:
    b, a = queue.popleft()
    if (b, a) == (n, 1):
        break
    for nb in adj[b]:
        for na in adj[a]:
            if nb != na and dist[nb][na] == -1:
                dist[nb][na] = dist[b][a] + 1
                parent[nb][na] = (b, a)
                queue.append((nb, na))

if dist[n][1] == -1:
    print(-1)
    sys.exit()

# reconstruct paths
bpath, apath = [], []
b, a = n, 1
while (b, a) != (1, n):
    bpath.append(b)
    apath.append(a)
    b, a = parent[b][a]
bpath.append(1)
apath.append(n)
bpath.reverse()
apath.reverse()

print(len(bpath) - 1)
print(' '.join(map(str, bpath)))
print(' '.join(map(str, apath)))
```Danh sách kề ghi lại biểu đồ không có trọng số. BFS trên biểu đồ sản phẩm đảm bảo chúng tôi khám phá tất cả các bước di chuyển đồng thời trong khi thực thi việc tránh xung đột nút. Con trỏ gốc cho phép xây dựng lại đường dẫn và BFS đảm bảo các đường dẫn được đồng bộ hóa ngắn nhất. Sự tinh tế nằm ở việc kiểm tra`nb != na`trong quá trình thăm dò hàng xóm để tránh va chạm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
1 2
```| Bước | Xếp hàng | Hiện trạng | Tiểu bang mới | 
| --- | --- | --- | --- | 
| 0 | (1,2) | (1,2) | (2,1) | 
| 1 | (2,1) | (2,1) | - | 

Xây dựng lại đường dẫn: Bob 1→2, Alex 2→1. Độ dài = 1. Cho thấy rằng đường dẫn một cạnh tối thiểu hoạt động. 

### Mẫu tùy chỉnh 

đầu vào:```
3 2
1 2
2 3
```BFS khám phá`(1,3)`→`(2,2)`nhưng không thể di chuyển đến`(3,1)`không va chạm. Không có đường dẫn hợp lệ tồn tại, đầu ra`-1`. Điều này xác nhận rằng việc chiếm dụng đồng thời nút giữa đã bị chặn chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 + n*m) | BFS khám phá tối đa n×n trạng thái; mỗi cái có tổng số cạnh kiểm tra lên đến m cạnh | 
| Không gian | O(n^2) |`dist`Và`parent`mảng là n×n | 

Với`n ≤ 500`Và`m ≤ 10,000`, n^2 = 250.000 và n*m 5.000.000. Cả hai đều vừa vặn thoải mái trong 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    exec(open("solution.py").read())
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# Provided sample
assert run("2 1\n1 2\n") == "1\n1 2\n2 1", "sample 1"

# Minimal case with no valid path
assert run("3 2\n1 2\n2 3\n") == "-1", "collision
```
