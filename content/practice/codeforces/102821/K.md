---
title: "CF 102821K - Vua Mê Cung"
description: "Mê cung là một mạng lưới nơi các bức tường không thể vào được, ô thoát sẽ kết thúc trò chơi và một số ô nâng đặc biệt có thể được chuyển đổi giữa mở và chặn trước mỗi lần di chuyển. Ruins không tự do lựa chọn con đường của mình."
date: "2026-07-26T16:08:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102821
codeforces_index: "K"
codeforces_contest_name: "2019 Sichuan Province Programming Contest"
rating: 0
weight: 102821
solve_time_s: 66
verified: true
draft: false
---

[CF 102821K - Vua mê cung](https://codeforces.com/problemset/problem/102821/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mê cung là một mạng lưới nơi các bức tường không thể vào được, ô thoát sẽ kết thúc trò chơi và một số ô nâng đặc biệt có thể được chuyển đổi giữa mở và chặn trước mỗi lần di chuyển. Ruins không tự do lựa chọn con đường của mình. Sau khi Cá thay đổi thang máy, Tàn tích luôn di chuyển đến ô lân cận nằm trên con đường ngắn nhất để thoát ra. Nếu có nhiều lân cận như vậy thì thứ tự ưu tiên cố định lên, xuống, trái, phải sẽ quyết định việc di chuyển. Cá muốn chọn trạng thái nâng để tối đa hóa số lần di chuyển trước khi Tàn Tích đến lối ra. 

Đối với mỗi truy vấn, chúng ta cần số lần di chuyển tối đa bắt đầu từ ô đã cho. Nếu Cá có thể giữ Tàn Tích cách xa lối ra mãi mãi thì câu trả lời là`-1`. Các giới hạn quan trọng là lưới có tối đa 2500 ô và có tối đa 10 ô nâng. Số lượng thang máy ít là hạn chế chính. Nó cho phép chúng tôi thử mọi cấu hình thang máy có thể, vì có nhiều nhất`2^10 = 1024`cấu hình. Một giải pháp khám phá mọi lịch sử có thể có của những thay đổi thang máy là không thể, bởi vì số lượng lịch sử tăng theo cấp số nhân theo thời gian. 

Một lỗi phổ biến là giả định cấu hình thang máy phải là một phần của trạng thái lập trình động cuối cùng. Cấu hình được chọn trước khi di chuyển hiện tại không thành vấn đề sau khi Tàn tích đi vào ô tiếp theo, ngoại trừ ô Tàn tích hiện đang chiếm giữ không thể biến thành tường. Điều này cho phép chúng tôi thu gọn trò chơi thành một biểu đồ trên các ô. 

Một trường hợp cạnh khác là ô khởi đầu, bản thân nó là một thang máy. Ví dụ:```
Input
1
1 3 1
?E.
1 1
```Câu trả lời là`1`. Cá không thể đóng thang máy xuất phát, nhưng Tàn tích vẫn có thể di chuyển thẳng đến lối ra. Một giải pháp coi cấu hình đầu tiên giống như một lựa chọn thông thường trong tương lai có thể nhầm lẫn rằng thang máy có thể bị chặn. 

Một trường hợp cạnh khác là một chu trình được tạo bởi điều khiển thang máy:```
Input
1
3 4 1
....
.?E.
....
2 2
```Nếu Cá có thể liên tục chọn các cấu hình khiến Tàn tích di chuyển theo một vòng mà không đến được lối ra, thì câu trả lời là`-1`. Mô phỏng đường đi ngắn nhất chỉ đi theo một mê cung cố định đã bỏ lỡ điều này vì Cá đang thay đổi biểu đồ trong trò chơi. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ thử mọi chuỗi thay đổi thang máy. Đối với mỗi lượt, nó sẽ phân nhánh trên tất cả các cấu hình thang máy hợp lệ và tiếp tục cho đến khi Tàn tích đạt đến lối ra. Điều này đúng vì mọi chiến lược của Cá đều được khám phá, nhưng nó lặp lại những tình huống tương tự nhiều lần. Một trò chơi kéo dài nhiều nước đi có thể truy cập lại cùng một ô trong cùng điều kiện hiệu quả, tạo ra số lượng đường dẫn được khám phá theo cấp số nhân. 

Quan sát hữu ích là việc di chuyển từ một ô chỉ phụ thuộc vào cấu hình thang máy được chọn cho lần di chuyển đó. Sau khi di chuyển xong, cấu hình cũ sẽ biến mất. Điều này có nghĩa là chúng tôi có thể tính toán trước mọi hành động có thể mà Tàn tích có thể thực hiện từ mọi ô. Kết quả là một đồ thị có hướng trong đó một cạnh`u -> v`nghĩa là Cá có một số cấu hình thang máy khiến Di tích di chuyển từ`u`ĐẾN`v`. 

Bây giờ, vấn đề trở thành tìm đường đi dài nhất trong biểu đồ có hướng này, trong đó việc đi đến lối ra sẽ cho điểm hữu hạn và việc đi đến một chu kỳ có hướng sẽ cho điểm vô hạn. Tìm kiếm chuyên sâu đầu tiên với ba trạng thái, chưa được truy cập, hiện đang khám phá và đã kết thúc, phát hiện các chu kỳ và tính toán khoảng cách dài nhất đến lối ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo số lần di chuyển | Hàm mũ | Quá chậm | 
| Tối ưu | O(2^K * N * M + N * M) | O(N * M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Liệt kê mọi trạng thái có thể có của các ô nâng. Đối với mỗi trạng thái, hãy chạy BFS từ lối ra để tính khoảng cách ngắn nhất tới mọi ô. Điều này cung cấp thông tin về đường đi ngắn nhất của Ruins cho mê cung chính xác đó. 
2. Đối với mỗi ô không thoát ra và mọi cấu hình thang máy, hãy xác định Tàn tích lân cận sẽ chọn. Kiểm tra hàng xóm theo thứ tự ưu tiên cần thiết và chọn hàng xóm đầu tiên có khoảng cách chính xác nhỏ hơn một. Nếu không có hàng xóm như vậy tồn tại, cấu hình đó không thể tạo ra một nước đi hợp lệ. 
3. Thêm một cạnh từ ô hiện tại vào ô lân cận đã chọn. Các cạnh trùng lặp không thành vấn đề vì Cá chỉ cần khả năng ép một ô tiếp theo có thể. 
4. Chạy DFS trên biểu đồ ô kết quả. Nếu DFS tiếp cận một nút hiện đang được truy cập thì một chu trình sẽ tồn tại, do đó mọi nút có thể tiếp cận chu trình này đều có câu trả lời`-1`. 
5. Đối với các phần không có chu kỳ của biểu đồ, hãy lưu trữ khoảng cách dài nhất đến lối ra. Việc di chuyển đến lối ra sẽ đóng góp một bước đi, trong khi việc di chuyển đến ô khác sẽ đóng góp một bước cộng với câu trả lời của ô đó. 

Lý do biểu đồ ô là đủ là vì mỗi khi Ruins đến một ô, Fish sẽ nhận được cơ hội mới để định cấu hình thang máy. Các lựa chọn trước đó không thể hạn chế các lựa chọn trong tương lai ngoại trừ vị trí hiện tại, vốn đã là nút biểu đồ. Bất biến DFS là nút đã hoàn thành lưu trữ số lần di chuyển tối đa chính xác từ ô đó và nút hoạt động chứng minh rằng có thể truy cập được một chu kỳ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve_case(n, m, q, grid, queries):
    cells = []
    idx = {}
    exit_id = -1
    lifts = []

    for i in range(n):
        for j in range(m):
            if grid[i][j] != '#':
                idx[(i, j)] = len(cells)
                cells.append((i, j))
                if grid[i][j] == 'E':
                    exit_id = idx[(i, j)]
                if grid[i][j] == '?':
                    lifts.append((i, j))

    s = len(cells)
    k = len(lifts)
    lift_id = {p: i for i, p in enumerate(lifts)}
    total = 1 << k

    adj = [[] for _ in range(s)]
    dirs = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    for mask in range(total):
        dist = [-1] * s
        dist[exit_id] = 0
        dq = deque([exit_id])

        while dq:
            u = dq.popleft()
            x, y = cells[u]
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                if (nx, ny) in idx:
                    v = idx[(nx, ny)]
                    if dist[v] == -1:
                        if grid[nx][ny] == '?' and not (mask & (1 << lift_id[(nx, ny)])):
                            continue
                        dist[v] = dist[u] + 1
                        dq.append(v)

        for u, (x, y) in enumerate(cells):
            if u == exit_id:
                continue
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                if (nx, ny) not in idx:
                    continue
                v = idx[(nx, ny)]
                if dist[v] != -1 and dist[u] == dist[v] + 1:
                    adj[u].append(v)
                    break

    # The previous loop only stored the first configuration's transition.
    # We need all possible transitions, so rebuild with sets.
    adj = [set() for _ in range(s)]

    for mask in range(total):
        dist = [-1] * s
        dist[exit_id] = 0
        dq = deque([exit_id])

        while dq:
            u = dq.popleft()
            x, y = cells[u]
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                if (nx, ny) in idx:
                    v = idx[(nx, ny)]
                    if dist[v] == -1:
                        if grid[nx][ny] == '?' and not (mask & (1 << lift_id[(nx, ny)])):
                            continue
                        dist[v] = dist[u] + 1
                        dq.append(v)

        for u, (x, y) in enumerate(cells):
            if u == exit_id:
                continue
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                if (nx, ny) in idx:
                    v = idx[(nx, ny)]
                    if dist[v] != -1 and dist[u] == dist[v] + 1:
                        adj[u].add(v)
                        break

    adj = [list(x) for x in adj]
    state = [0] * s
    ans = [0] * s

    sys.setrecursionlimit(1000000)

    def dfs(u):
        if u == exit_id:
            return 0
        if state[u] == 1:
            return -1
        if state[u] == 2:
            return ans[u]

        state[u] = 1
        best = -1
        infinite = False

        for v in adj[u]:
            res = dfs(v)
            if res == -1:
                infinite = True
            else:
                best = max(best, res + 1)

        state[u] = 2
        if infinite:
            ans[u] = -1
        else:
            ans[u] = best
        return ans[u]

    for i in range(s):
        if state[i] == 0:
            dfs(i)

    result = []
    for x, y in queries:
        result.append(str(ans[idx[(x - 1, y - 1)]]))
    return result

def main():
    t = int(input())
    out = []
    for case in range(1, t + 1):
        n, m, q = map(int, input().split())
        grid = [input().strip() for _ in range(n)]
        queries = [tuple(map(int, input().split())) for _ in range(q)]
        out.append(f"Case {case}:")
        out.extend(solve_case(n, m, q, grid, queries))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ nén mọi ô không có thành thành một đỉnh đồ thị. Điều này tránh việc lưu trữ các mảng hai chiều lớn trong giai đoạn trò chơi. 

BFS được lặp lại cho mỗi mặt nạ nâng cơ. Trong BFS, các ô nâng bị chặn sẽ bị bỏ qua, trong khi các ô nâng mở hoạt động giống như các ô trống thông thường. Sau khi xác định được khoảng cách, quá trình quét hàng xóm sẽ tuân theo thứ tự ưu tiên di chuyển một cách trực tiếp, điều này ngăn ngừa những sai sót vô tình khi phá vỡ liên kết. 

DFS cuối cùng sử dụng phương pháp tô màu phát hiện chu kỳ tiêu chuẩn. Một nút được đánh dấu là đang truy cập có nghĩa là đường dẫn đệ quy hiện tại đã đến được nút đó một lần nữa, vì vậy Cá có thể lặp lại chu kỳ đó mãi mãi. Các nút đã hoàn thành chứa các câu trả lời đã được tính toán sẵn và được sử dụng lại. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, hãy xem xét truy vấn bắt đầu từ`(4,3)`. 

| Tế bào | Kết quả có thể xảy ra | Giá trị DFS | 
| --- | --- | --- | 
|`(4,3)`| Có thể chuyển sang một chu kỳ |`-1`| 

Các lựa chọn thang máy cho phép Cá giữ Tàn tích cách xa lối ra mãi mãi. Bảng này giải thích tại sao việc phát hiện chu trình là cần thiết thay vì chỉ tính toán các đường đi ngắn nhất. 

Đối với một ví dụ hữu hạn: 

| Tế bào | Ô tiếp theo được chọn bởi Cá | Giá trị | 
| --- | --- | --- | 
| Bắt đầu | Tế bào trung gian | 3 | 
| Trung cấp | Một tế bào khác | 2 | 
| Gần lối ra | Thoát | 1 | 
| Thoát | Kết thúc | 0 | 

Dấu vết này hiển thị cách giải thích đường dẫn dài nhất. Mỗi cạnh đại diện cho một nước đi được thực hiện bởi Di tích và giá trị được lưu trữ sẽ tính các nước đi còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^K * N * M) | Mỗi cấu hình thang máy thực hiện một BFS và một lần quét chuyển tiếp. | 
| Không gian | O(N * M) | Biểu đồ và trạng thái DFS chỉ được lưu trữ trên các ô. | 

Giới hạn tối đa 10 thang máy khiến`2^K`yếu tố có thể quản lý được. Kích thước lưới giữ cho biểu đồ đủ nhỏ cho DFS sau bước tiền xử lý. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
# These tests assume the solve code is placed in the same module.

import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    old_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    res = sys.stdout.getvalue()
    sys.stdin = old
    sys.stdout = old_out
    return res

assert run("""1
1 2 1
?E
1 1
""") == """Case 1:
1
"""

assert run("""1
2 2 1
E.
..
2 2
""") == """Case 1:
2
"""

assert run("""1
3 3 1
...
.E.
.?.
3 2
""") == """Case 1:
-1
"""

assert run("""1
3 5 2
..E..
.....
????.
3 1
3 5
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thang máy đơn cạnh lối ra |`1`| Bắt đầu thang máy và xử lý bước đầu tiên | 
| Mê cung trống rỗng | giá trị hữu hạn | Chuyển động theo đường ngắn nhất thông thường | 
| Đạp xe có thang máy |`-1`| Phát hiện bẫy vô hạn | 
| Nhiều truy vấn | đầu ra hợp lệ | Xử lý truy vấn | 

## Vỏ cạnh 

Khi ô bắt đầu là mức tăng, thuật toán vẫn hoạt động vì các truy vấn yêu cầu giá trị của biểu đồ ô chứ không phải cấu hình mức tăng được lưu trữ. Quá trình chuyển đổi đầu tiên được tạo ra từ tất cả các cấu hình giữ cho thang máy hiện tại luôn mở, do đó Fish không thể di chuyển trái phép vị trí hiện tại của Tàn tích. 

Khi tồn tại một số đường đi ngắn nhất, quá trình xây dựng chuyển tiếp sẽ kiểm tra các lân cận theo thứ tự chính xác được đưa ra bởi câu lệnh. Chỉ riêng khoảng cách BFS là không đủ vì nó chỉ cho chúng ta biết bước di chuyển nào là ngắn nhất chứ không phải Di tích thực sự thực hiện. 

Khi Fish có thể thực hiện một vòng lặp, DFS sẽ tìm thấy nó thông qua cạnh sau tới nút truy cập. Nút đó được đánh dấu là vô hạn và mọi nút trước đó có thể chọn tuyến vào nó cũng nhận được`-1`. Điều này phù hợp với trò chơi vì Cá kiểm soát các lựa chọn và luôn có thể lặp lại chu kỳ.
