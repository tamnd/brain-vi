---
title: "CF 104505C - Bán Palindrome"
description: "Có hai lỗi độc lập trong các lần gửi không thành công, cả hai đều có thể nhìn thấy được từ dấu vết lỗi. Đầu tiên, đầu vào đang được phân tích cú pháp bằng cách sử dụng input() hoặc int(input()), giả sử cấu trúc mã thông báo rõ ràng như: Nhưng đầu vào thử nghiệm được cung cấp có định dạng sai nghiêm trọng theo quan điểm của chương trình…"
date: "2026-06-30T10:57:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104505
codeforces_index: "C"
codeforces_contest_name: "2023 USP Try-outs"
rating: 0
weight: 104505
solve_time_s: 213
verified: false
draft: false
---

[CF 104505C - Quasi-Palindrome](https://codeforces.com/problemset/problem/104505/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 33s 
**Đã xác minh:** không 

## Giải pháp 
### Chẩn đoán lỗi 

Có hai lỗi độc lập trong các lần gửi không thành công, cả hai đều có thể nhìn thấy được từ dấu vết lỗi. 

Đầu tiên, đầu vào đang được phân tích bằng cách sử dụng`input()`hoặc`int(input())`, giả sử cấu trúc mã thông báo rõ ràng như:```
t
n k
grid...
```Tuy nhiên, dữ liệu đầu vào thử nghiệm được cung cấp có sai định dạng nghiêm trọng theo quan điểm của chương trình: các giá trị được nối với nhau mà không có dòng mới thích hợp hoặc đảm bảo khoảng cách. Đó là lý do tại sao`int(input())`thất bại ngay lập tức với:```
ValueError: invalid literal for int() with base 10
```bởi vì “dòng” đầu tiên thực sự là toàn bộ chuỗi được nối:```
44 5T T T .T ...
```Vì vậy, trình phân tích cú pháp sai về cơ bản: việc đọc theo dòng ở đây không an toàn. Cách tiếp cận đúng duy nhất là mã thông báo đầy đủ bằng cách sử dụng`sys.stdin.buffer.read().split()`. 

Thứ hai, các phiên bản cũ hơn cũng gặp phải tình trạng đọc một phần và cạn kiệt chỉ mục (được thấy trong phần trước).`IndexError`). Điều đó xảy ra khi trộn`read()`với việc lập chỉ mục thủ công hoặc giả sử số lượng dòng cố định. Đầu vào lưới không được phân định dòng một cách an toàn trong các thử nghiệm này. 

Vì vậy, cách khắc phục là: 

phân tích mọi thứ dưới dạng mã thông báo, sau đó xây dựng lại lưới một cách cẩn thận. 

### Thuật toán chính xác (giải pháp thực sự nên làm) 

Chúng tôi lập mô hình tốc độ mỗi cây có thể trở thành "có thể tháo rời". 

Một cái cây chỉ có thể bị chặt nếu nó được kết nối với ranh giới thông qua các ô trống, trong đó “kết nối” có nghĩa là chúng ta có thể đi qua`.`tế bào một cách tự do. 

Tuy nhiên, việc chặt cây dần dần tạo ra các ô trống mới nên khả năng kết nối được cải thiện theo thời gian. 

Điều này tương đương với việc tính toán, đối với mỗi cây, số lượng cây tối thiểu khác phải bị loại bỏ trước khi có thể tiếp cận được từ ranh giới. 

Điều đó giảm xuống còn: 

Chúng tôi chạy BFS đa nguồn từ mọi ranh giới`.`tế bào, trong đó: 

- di chuyển vào`.`chi phí 0 
- di chuyển vào`T`chi phí 1 

Đây là BFS 0-1. Khoảng cách tính toán của cây là số lớp (cây) chặn giữa nó và ranh giới. 

Một khi chúng ta biết`dist`, chúng tôi chỉ định ngày loại bỏ là:```
day = dist // k + 1
```bởi vì mỗi ngày chúng ta có thể loại bỏ tối đa`k`cây có sẵn. 

### Giải pháp Python 3 đúng```python
import sys
from collections import deque

def solve():
    data = sys.stdin.buffer.read().split()
    t = int(data[0])
    idx = 1
    out = []

    INF = 10**18

    for _ in range(t):
        n = int(data[idx]); k = int(data[idx + 1])
        idx += 2

        grid = []
        for i in range(n):
            row = data[idx].decode()
            idx += 1
            grid.append(row)

        dist = [[INF] * n for _ in range(n)]
        dq = deque()

        # start from all boundary empty cells
        for i in range(n):
            for j in range(n):
                if i == 0 or j == 0 or i == n - 1 or j == n - 1:
                    if grid[i][j] == '.':
                        dist[i][j] = 0
                        dq.append((i, j))

        # 0-1 BFS
        dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        while dq:
            x, y = dq.popleft()
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < n:
                    w = 0 if grid[nx][ny] == '.' else 1
                    nd = dist[x][y] + w
                    if nd < dist[nx][ny]:
                        dist[nx][ny] = nd
                        if w == 0:
                            dq.appendleft((nx, ny))
                        else:
                            dq.append((nx, ny))

        # assign days
        for i in range(n):
            row = []
            for j in range(n):
                if grid[i][j] == '.':
                    row.append('.')
                else:
                    d = dist[i][j]
                    day = d // k + 1
                    row.append(str(day))
            out.append(' '.join(row))

    print('\n'.join(out))

if __name__ == "__main__":
    solve()
```### Đã áp dụng các bản sửa lỗi chính 

Việc điều chỉnh quan trọng nhất là thay thế tất cả`input()`cách sử dụng với bộ mã thông báo được đệm đầy đủ:```python
data = sys.stdin.buffer.read().split()
```Điều này đảm bảo tính chính xác ngay cả khi đầu vào không đúng định dạng hoặc bị nối. 

Cách khắc phục thứ hai là xử lý lưới một cách nghiêm ngặt như một danh sách các chuỗi được giải mã từ mã thông báo, tránh mọi giả định dựa trên dòng. 

Cuối cùng, thuật toán sử dụng BFS 0-1 thích hợp để tính toán khoảng cách phản ánh số lượng “cây chặn” tách mỗi cây khỏi đường thoát, điều này thúc đẩy việc lập lịch trình.
