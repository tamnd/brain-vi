---
title: "CF 1829E - Những Hồ"
description: "Chúng ta có một lưới hai chiều biểu thị địa hình, trong đó mỗi ô chứa một số nguyên không âm cho biết độ sâu của nước tại vị trí đó."
date: "2026-06-09T07:18:08+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1829
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 871 (Div. 4)"
rating: 1100
weight: 1829
solve_time_s: 80
verified: true
draft: false
---

[CF 1829E - The Lakes](https://codeforces.com/problemset/problem/1829/E) 

**Đánh giá:** 1100 
**Thẻ:** dfs và tương tự, dsu, đồ thị, triển khai 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hai chiều biểu thị địa hình, trong đó mỗi ô chứa một số nguyên không âm cho biết độ sâu của nước tại vị trí đó. Một "hồ" được định nghĩa là một vùng liền kề gồm các ô khác 0 được kết nối trực giao, nghĩa là chúng ta có thể di chuyển giữa chúng bằng cách di chuyển lên, xuống, trái hoặc phải mà không cần bước lên ô có độ sâu bằng 0. Thể tích của hồ là tổng độ sâu của các ô cấu thành nó. Nhiệm vụ là tìm thể tích hồ lớn nhất có thể có trong mỗi lưới. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm xác định kích thước của lưới và giá trị độ sâu của từng ô. Kích thước lưới tối đa là 1000 x 1000 và tổng của tất cả các ô trong tất cả các trường hợp thử nghiệm không vượt quá$10^6$. Điều này có nghĩa là bất kỳ thuật toán nào chạm vào từng ô với số lần không đổi sẽ chạy thoải mái trong giới hạn thời gian. Tuy nhiên, các thuật toán cố gắng so sánh mọi cặp ô hoặc thực hiện thao tác toàn cục trên mỗi ô khác 0 (như$O(n^2)$cho mỗi trường hợp thử nghiệm) sẽ quá chậm. 

Các trường hợp cạnh không rõ ràng bao gồm các lưới chỉ chứa các số 0, các lưới trong đó tất cả các ô khác 0 đều bị cô lập và các hồ chạm vào ranh giới lưới. Ví dụ: một hồ đơn ô có độ sâu 5 trong lưới 1x1 sẽ trả về 5. Một DFS ngây thơ quên đánh dấu các ô đã truy cập có thể đếm gấp đôi một hồ, tạo ra thể tích lớn hơn giá trị chính xác. Tương tự, việc quên xử lý ranh giới một cách chính xác có thể dẫn đến lỗi vượt quá giới hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Đối với mỗi ô khác 0, hãy thực hiện DFS hoặc BFS để khám phá vùng khác 0 được kết nối, tính tổng độ sâu để tính thể tích hồ. Theo dõi các ô đã truy cập để tránh tính hai lần. Cuối cùng, trả lại âm lượng tối đa gặp phải. Cách tiếp cận này đúng, nhưng hiệu suất trong trường hợp xấu nhất của nó tỷ lệ thuận với tổng số ô, vì mỗi ô được truy cập một lần và mỗi lần kiểm tra hàng xóm là không đổi. Với các ràng buộc, điều này có thể chấp nhận được, nhưng việc triển khai đơn giản mà không đánh dấu các ô đã truy cập hoặc sử dụng đệ quy một cách cẩn thận có thể dẫn đến tràn ngăn xếp hoặc tính toán dư thừa. 

Cách tiếp cận tối ưu dựa trên cùng một nguyên tắc nhưng sử dụng BFS lặp để tránh các vấn đề về độ sâu đệ quy. Bằng cách xử lý từng ô chính xác một lần và tổng hợp nhanh chóng thành phần được kết nối, chúng tôi đảm bảo rằng chúng tôi truy cập mọi ô nhiều nhất một lần. Thông tin chi tiết quan trọng là mỗi hồ là một thành phần được kết nối độc lập và khối lượng lớn nhất đến từ thành phần có tổng cao nhất. Vì chúng tôi chỉ duyệt qua các ô là một phần của các hồ khác 0 nên chúng tôi bỏ qua các bước kiểm tra không cần thiết một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS/BFS | O(n*m) cho mỗi trường hợp thử nghiệm | O(n*m) | Đã chấp nhận | 
| BFS lặp tối ưu | O(n*m) cho mỗi trường hợp thử nghiệm | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`max_volume`về không. Điều này sẽ theo dõi khối lượng hồ lớn nhất được tìm thấy. 
2. Tạo mảng 2D`visited`có cùng kích thước với lưới để theo dõi những ô nào đã được đưa vào hồ. 
3. Lặp lại từng ô`(i, j)`trong lưới. Nếu như`a[i][j]`bằng 0 hoặc đã truy cập, bỏ qua nó. 
4. Nếu ô khác 0 và không được truy cập, hãy khởi động BFS từ ô đó. Khởi tạo một hàng đợi với`(i, j)`và một biến`current_volume`về không. 
5. Khi hàng đợi không trống, hãy bật một ô`(x, y)`. Đánh dấu nó là đã truy cập và thêm`a[x][y]`ĐẾN`current_volume`. 
6. Kiểm tra tất cả bốn hàng xóm của`(x, y)`. Đối với mỗi người hàng xóm`(nx, ny)`trong giới hạn lưới, nếu`a[nx][ny] > 0`và chưa được truy cập, hãy thêm nó vào hàng đợi. 
7. Sau khi BFS hoàn tất, hãy so sánh`current_volume`với`max_volume`. Nếu nó lớn hơn, hãy cập nhật`max_volume`. 
8. Tiếp tục lặp qua lưới cho đến khi tất cả các ô đã được xử lý. 
9. Trở về`max_volume`cho trường hợp thử nghiệm này. 

Tại sao nó hoạt động: BFS đảm bảo rằng mọi vùng khác 0 được kết nối đều được khám phá đầy đủ một lần, tổng hợp tất cả độ sâu của nó. các`visited`mảng đảm bảo không có ô nào được tính hai lần. Vì tất cả các hồ có thể được khám phá độc lập nên thể tích hồ tối đa được xác định chính xác. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(n)]
        visited = [[False]*m for _ in range(n)]
        max_volume = 0
        
        for i in range(n):
            for j in range(m):
                if grid[i][j] > 0 and not visited[i][j]:
                    q = deque()
                    q.append((i,j))
                    visited[i][j] = True
                    current_volume = 0
                    while q:
                        x, y = q.popleft()
                        current_volume += grid[x][y]
                        for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
                            nx, ny = x + dx, y + dy
                            if 0 <= nx < n and 0 <= ny < m and grid[nx][ny] > 0 and not visited[nx][ny]:
                                visited[nx][ny] = True
                                q.append((nx, ny))
                    max_volume = max(max_volume, current_volume)
        print(max_volume)

if __name__ == "__main__":
    solve()
```Giải pháp tách riêng đầu vào đọc, xử lý từng trường hợp thử nghiệm và khám phá BFS. sử dụng`deque`cho BFS đảm bảo bật ra hiệu quả từ phía trước. Đánh dấu các ô là đã truy cập ngay lập tức khi chúng được xếp vào hàng đợi sẽ tránh được việc truy cập nhiều lần và duy trì tính chính xác. Kiểm tra hàng xóm bốn hướng tôn trọng ranh giới lưới để tránh lỗi chỉ mục. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 2 0
3 4 0
0 0 5
```| Di động đã truy cập | Trạng thái xếp hàng | hiện tại_volume | max_volume | 
| --- | --- | --- | --- | 
| (0,0) | [(0,0)] | 0 | 0 | 
| (0,1) | [(0,1)] | 1 | 0 | 
| (1,0) | [(1,0)] | 3 | 0 | 
| (1,1) | [] | 10 | 10 | 
| (2,2) | [(2,2)] | 5 | 10 | 

Giải thích: Trước tiên, BFS khám phá hồ được kết nối gồm các ô (0,0),(0,1),(1,0),(1,1) tổng cộng là 10. Ô bị cô lập (2,2) tạo thành một hồ có thể tích 5. Tối đa chính xác là 10. 

### Mẫu 2 

đầu vào:```
1 1
0
```Dấu vết: Ô bằng 0, bị bỏ qua.`max_volume`vẫn là 0. Xuất ra đúng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) cho mỗi trường hợp thử nghiệm | Mỗi ô được truy cập một lần và mỗi ô lân cận được kiểm tra bốn lần | 
| Không gian | O(n*m) | Lưới và mảng đã truy cập, cộng với hàng đợi lên tới n*m trong trường hợp xấu nhất | 

Với tổng của tất cả các ô trong các trường hợp thử nghiệm không vượt quá$10^6$, giải pháp này chạy thoải mái trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# Provided samples
assert run("5\n3 3\n1 2 0\n3 4 0\n0 0 5\n1 1\n0\n3 3\n0 1 1\n1 0 1\n1 1 1\n5 5\n1 1 1 1 1\n1 0 0 0 1\n1 0 5 0 1\n1 0 0 0 1\n1 1 1 1 1\n5 5\n1 1 1 1 1\n1 0 0 0 1\n1 1 4 0 1\n1 0 0 0 1\n1 1 1 1 1") == "10\n0\n7\n16\n21"

# Custom cases
assert run("1\n1 1\n5") == "5", "single cell lake"
assert run("1\n2 2\n0 0\n0 0") == "0", "all zeros"
assert run("1\n2 2\n1 1\n1 1") == "4", "full lake"
assert run("1\n3 3\n0 1 0\n1 0 1\n0 1 0") == "1", "diagonal not connected"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 với 5 | 5 | Hồ đơn bào | 
| 2x2 tất cả số không | 0 | Trường hợp không hồ | 
| 2x2 tất cả những cái | 4 | Hồ kết nối đầy đủ | 
| 3x3 chéo của những cái | 1 | Chỉ tính các kết nối trực giao, bỏ qua các đường chéo | 

## Vỏ cạnh 

Một lưới chỉ có số 0: BFS không bao giờ
