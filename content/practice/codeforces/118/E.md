---
title: "CF 118E - Đường Bertown"
description: "Chúng ta được cung cấp một đồ thị vô hướng được kết nối đại diện cho mạng lưới đường bộ của Bertown, trong đó các điểm giao nhau là các nút và đường là các cạnh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 118
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 89 (Div. 2)"
rating: 2000
weight: 118
solve_time_s: 136
verified: false
draft: false
---

[CF 118E - Đường Bertown](https://codeforces.com/problemset/problem/118/E) 

**Xếp hạng:** 2000 
**Thẻ:** dfs và các biểu đồ tương tự 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng được kết nối đại diện cho mạng lưới đường bộ của Bertown, trong đó các điểm giao nhau là các nút và đường là các cạnh. Mỗi con đường hiện cho phép giao thông hai chiều nhưng chúng tôi có nhiệm vụ chỉ định hướng cho mỗi con đường sao cho đồ thị có hướng thu được vẫn được kết nối chặt chẽ. Khả năng kết nối mạnh mẽ có nghĩa là từ bất kỳ ngã ba nào, có thể đến mọi ngã ba khác theo đường một chiều. 

Kích thước đầu vào rất đáng kể: có thể có tới 100.000 nút giao thông và 300.000 con đường. Điều này có nghĩa là bất kỳ giải pháp nào kém hơn O(n + m) có thể là quá chậm, vì n·m có thể đạt tới 10^10, vượt xa giới hạn thời gian 5 giây. Vì vậy, những cách tiếp cận ngây thơ để kiểm tra tất cả các hoán vị của hướng đường là không khả thi. 

Các trường hợp cạnh không rõ ràng phát sinh từ chu kỳ và cầu nối. Ví dụ: nếu đồ thị vô hướng có một cây cầu (một cạnh mà việc loại bỏ nó sẽ ngắt kết nối đồ thị), thì không thể định hướng các cạnh để duy trì khả năng kết nối mạnh mẽ. Một ví dụ đơn giản là một hình tam giác được nối với nhau bằng một cây cầu: việc định hướng cây cầu theo bất kỳ hướng nào sẽ khiến việc di chuyển theo hướng ngược lại là không thể. Một trường hợp cạnh khác xảy ra trong đồ thị hoàn chỉnh: mọi cạnh có thể được định hướng theo kiểu tuần hoàn và khả năng kết nối mạnh mẽ được bảo toàn một cách tự nhiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hướng có thể có của các cạnh, sau đó kiểm tra xem đồ thị có hướng thu được có được kết nối mạnh hay không. Điều này yêu cầu kiểm tra các hướng 2^m, mỗi hướng cần O(n + m) thời gian cho DFS hoặc BFS. Với m lên tới 3×10^5, tốc độ này chậm về mặt thiên văn. 

Quan sát quan trọng là các cây cầu ngăn cản sự kết nối mạnh mẽ. Nếu đồ thị có một cây cầu thì một đầu không thể chạm tới đầu kia nếu cây cầu hướng sai hướng. Vì vậy, bước đầu tiên là xác định xem các cây cầu có tồn tại hay không. Thuật toán của Tarjan có thể phát hiện các cầu nối trong thời gian O(n + m) bằng cách thực hiện DFS và theo dõi thời gian khám phá cũng như các giá trị liên kết thấp. Nếu tìm được cây cầu, câu trả lời ngay lập tức là 0. 

Khi chúng ta biết không có cầu nối, đồ thị vô hướng sẽ được kết nối 2 cạnh. Trong trường hợp này, luôn có thể chỉ định hướng để đồ thị được kết nối chặt chẽ. Một công trình xây dựng dựa trên DFS đơn giản: đối với mỗi cạnh cây gặp phải trong DFS, hãy định hướng nó từ gốc đến con; đối với mọi cạnh sau, hãy định hướng nó từ con này sang cha mẹ khác. Điều này đảm bảo khả năng kết nối mạnh mẽ vì mọi chu trình trong đồ thị vô hướng đều trở thành chu trình có hướng và các chu trình liên kết các thành phần với nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m · (n + m)) | O(n + m) | Quá chậm | 
| Cầu DFS + Tarjan | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của đồ thị vô hướng từ các cạnh đầu vào. Đây là cấu trúc chúng tôi sẽ sử dụng để phát hiện DFS và cầu. 
2. Khởi tạo mảng để theo dõi thời gian khám phá (`tin`), thời gian khám phá có thể truy cập thấp nhất (`low`) và cờ đã truy cập cho mỗi nút. 
3. Thực hiện DFS bắt đầu từ bất kỳ nút nào (ví dụ nút 1) để phát hiện các cầu nối. Đối với mỗi nút, đặt`tin`Và`low`đến bộ đếm thời gian hiện tại, sau đó truy cập đệ quy các hàng xóm chưa được thăm. Cập nhật`low`giá trị theo quy tắc phát hiện cầu Tarjan tiêu chuẩn. Nếu chúng ta tìm thấy bất kỳ cạnh nào ở đó`low[neighbor] > tin[node]`, nó là một cây cầu và ngay lập tức chúng ta trả về 0. 
4. Trong DFS, ghi lại hướng của cạnh. Các cạnh của cây được định hướng từ cha mẹ đến con. Các cạnh sau được định hướng từ nút hiện tại đến nút tổ tiên của nó. Lưu trữ hướng trong từ điển được khóa bằng chỉ số cạnh để giữ nguyên thứ tự đầu vào. 
5. Sau DFS, nếu không tìm thấy cầu nối nào, hãy xuất các hướng cạnh được lưu trữ theo thứ tự chúng được đưa ra trong đầu vào. 

Tại sao nó hoạt động: Các cầu là vật cản duy nhất trong việc định hướng một đồ thị vô hướng liên thông vào một đồ thị có hướng liên thông mạnh. Bằng cách thực hiện DFS và ghi lại các cạnh của cây và mặt sau, chúng tôi định hướng tất cả các cạnh dọc theo chu trình một cách nhất quán. Điều này đảm bảo rằng mọi nút đều có thể tiếp cận mọi nút khác vì các chu kỳ cho phép truyền tải theo cả hai hướng thông qua các đường dẫn khác nhau, đáp ứng khả năng kết nối mạnh mẽ. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**6)
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n)]
    edges = []
    
    for i in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        adj[a].append((b, i))
        adj[b].append((a, i))
        edges.append((a, b))
    
    tin = [-1] * n
    low = [-1] * n
    timer = [0]
    visited = [False] * n
    res = [None] * m
    found_bridge = [False]
    
    def dfs(u, parent):
        visited[u] = True
        tin[u] = low[u] = timer[0]
        timer[0] += 1
        for v, idx in adj[u]:
            if v == parent:
                continue
            if visited[v]:
                low[u] = min(low[u], tin[v])
                if res[idx] is None:
                    res[idx] = (u + 1, v + 1)
            else:
                res[idx] = (u + 1, v + 1)
                dfs(v, u)
                low[u] = min(low[u], low[v])
                if low[v] > tin[u]:
                    found_bridge[0] = True
    
    dfs(0, -1)
    
    if found_bridge[0]:
        print(0)
    else:
        for p, q in res:
            print(p, q)

if __name__ == "__main__":
    main()
```Danh sách kề ghi lại từng cạnh bằng chỉ mục của nó để sau này chúng ta có thể xuất ra theo thứ tự đầu vào.`tin`Và`low`mảng thực hiện phát hiện cầu nối của Tarjan. DFS định hướng các cạnh của cây từ nút cha đến nút con và các cạnh ngược từ nút hiện tại đến nút tổ tiên. Chúng tôi phát hiện ngay những cây cầu để xử lý những trường hợp nan giải. các`res`mảng duy trì ánh xạ định hướng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 8
1 2
2 3
1 3
4 5
4 6
5 6
2 4
3 5
```| Bước | Nút | thiếc | thấp | Định hướng cạnh | Tìm thấy cây cầu | 
| --- | --- | --- | --- | --- | --- | 
| DFS 1 | 1 | 0 | 0 | 1->2 | Sai | 
| DFS 2 | 2 | 1 | 0 | 2->3 | Sai | 
| DFS 3 | 3 | 2 | 0 | 3->1, 3->5 | Sai | 
| DFS 4 | 4 | 3 | 3 | 4->5 | Sai | 
| DFS 5 | 5 | 4 | 3 | 5->6 | Sai | 
| DFS hoàn thành | - | - | - | - | Sai | 

Tất cả các cạnh được định hướng không có cầu nối, duy trì kết nối mạnh mẽ. 

### Ví dụ 2 

đầu vào:```
4 3
1 2
2 3
3 4
```Phát hiện cầu ngay lập tức xác định cạnh 2-3 là cầu nối (`low[3] > tin[2]`). Thuật toán đưa ra 0, biểu thị chính xác không có giải pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | DFS truy cập mỗi nút một lần và kiểm tra tất cả các cạnh. | 
| Không gian | O(n + m) | Danh sách kề, mảng định hướng cạnh và mảng phụ cho DFS. | 

Độ phức tạp này rất phù hợp với các ràng buộc: 105 nút và 3·105 cạnh có thể được xử lý hiệu quả bằng DFS trong vòng chưa đầy một giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("6 8\n1 2\n2 3\n1 3\n4 5\n4 6\n5 6\n2 4\n3 5\n") != "0", "sample 1"

# Minimum-size input, no bridge
assert run("2 1\n1 2\n") == "1 2", "minimum input no bridge"

# Bridge present, no solution
assert run("4 3\n1 2\n2 3\n3 4\n") == "0", "linear bridge"

# Complete graph 3 nodes
assert run("3 3\n1 2\n2 3\n1 3\n") != "0", "triangle, any orientation works"

# Star graph center 1
assert run("4 3\n1 2\n1 3\n1 4\n") == "0", "center is bridge"

# Two separate cycles connected
assert run("6 7\n1 2\n2 3\n3 1\n4 5\n5 6\n6 4\n3 4\n") != "0", "double cycle connected"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 1 2 | đầu vào tối thiểu không có cầu | 
| 4 3 | 0 | cầu cản trở kết nối mạnh mẽ | 
| 3 3 | bất kỳ định hướng nào | chu kỳ cho phép kết nối mạnh mẽ | 
| 4 3 | 0 | trung tâm đồ thị ngôi sao là cây cầu | 
| 6 7 | | |
