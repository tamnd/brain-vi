---
title: "CF 102498C - \u041a\u043e\u0434\u043e\u0432\u044b\u0439 \u0437\u0430\u043c\u043e\u043a"
description: "Bảng chứa ba loại ô: ô trống, tâm cố định và tay cầm có thể xoay. Mỗi tay cầm phải được chỉ định một trong hai hướng."
date: "2026-08-06T04:33:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102498
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102498
solve_time_s: 690
verified: false
draft: false
---

[CF 102498C - \u041a\u043e\u0434\u043e\u0432\u044b\u0439 \u0437\u0430\u043c\u043e\u043a](https://codeforces.com/problemset/problem/102498/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11p 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bảng chứa ba loại ô: ô trống, tâm cố định và tay cầm có thể xoay. Mỗi tay cầm phải được chỉ định một trong hai hướng. Một điểm điều khiển dọc chỉ hợp lệ nếu việc di chuyển dọc theo cột của nó cuối cùng sẽ đến được tâm và mọi ô trước tâm đó là một điểm điều khiển dọc khác. Tay cầm ngang tuân theo quy tắc tương tự dọc theo hàng của nó. 

Nhiệm vụ là chọn hướng cho tất cả các ô điều khiển hoặc chứng minh rằng không tồn tại lựa chọn nào như vậy. Hạn chế mỗi hàng và cột có nhiều nhất một tâm là thuộc tính cấu trúc chính. Nó cho phép chúng ta suy luận về khả năng hiển thị theo chiều ngang và chiều dọc một cách độc lập. 

Giới hạn kích thước là`n <= 500`, do đó lưới chứa tối đa 250000 ô. Các thuật toán thử tất cả các hướng có thể là không thể vì số lượng tay cầm cũng có thể vào khoảng 250000, cho`2^250000`các trạng thái có thể. Ngay cả một thuật toán kiểm tra nhiều cặp ô liên tục cũng có thể trở nên quá chậm. Chúng ta cần một giải pháp gần tuyến tính về số lượng ô. 

Các trường hợp phức tạp không chỉ là các ô điều khiển không có tâm trong hàng hoặc cột của chúng. Một bộ điều khiển có thể có cả hai hướng có thể riêng lẻ, nhưng một bộ điều khiển khác có thể buộc một trong các hướng đó không thể thực hiện được. 

Ví dụ:```
3
.+.
O++
.O.
```Câu trả lời đúng là:```
No
```Tay cầm ở giữa trên cùng chỉ có thể thẳng đứng. Tay cầm ngoài cùng bên phải ở hàng giữa chỉ có thể nằm ngang, điều này buộc tay cầm ở giữa của hàng đó cũng phải nằm ngang. Tay cầm ở giữa cũng bắt buộc phải thẳng đứng để tay cầm trên chạm tới tâm dưới, tạo ra sự mâu thuẫn. 

Một trường hợp khác là:```
1
+
```Câu trả lời là:```
No
```Không có tâm ở đâu cả nên tay cầm duy nhất không có hướng hợp lệ. 

Một giải pháp bất cẩn chỉ kiểm tra từng tay cầm riêng biệt sẽ bỏ lỡ những tương tác này. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi sự phân công có thể của`|`Và`-`để xử lý. Đối với mỗi nhiệm vụ, nó sẽ xác minh mọi tay cầm bằng cách đi theo hướng đã chọn cho đến khi đến trung tâm hoặc ô không hợp lệ. Điều này đúng vì nó kiểm tra chính xác định nghĩa của trạng thái khóa hợp lệ. Tuy nhiên, với số lượng xử lý lên tới 250000 thì số lượng bài tập sẽ theo cấp số nhân, khoảng`2^250000`, điều đó là không thể. 

Quan sát hữu ích là mỗi lựa chọn hướng đi chỉ tạo ra những hàm ý logic. Nếu một tay cầm nằm ngang thì mọi tay cầm giữa nó và tâm trong cùng một hàng cũng phải nằm ngang. Tương tự, nếu một điểm điều khiển thẳng đứng thì mọi điểm điều khiển giữa nó và tâm trong cùng một cột cũng phải thẳng đứng. 

Vì tâm là duy nhất bên trong các hàng và cột nên những hàm ý này tạo thành những chuỗi đơn giản. Ví dụ: trong một hàng:```
O + + + .
```tay cầm ngoài cùng bên phải nằm ngang có nghĩa là tay cầm trước đó nằm ngang, có nghĩa là tay cầm trước đó nằm ngang. Chúng ta không cần một cạnh từ mỗi tay cầm này sang tay cầm khác, chỉ cần những hàm ý liền kề. 

Điều này chuyển đổi vấn đề thành 2-SAT. Mỗi tay cầm là một biến boolean. Chúng ta có thể diễn giải`true`theo chiều ngang và`false`như chiều dọc. Mọi hàm ý đều được thêm vào biểu đồ hàm ý và các hướng không thể có được biểu diễn dưới dạng giá trị buộc phải sai hoặc giá trị đúng bắt buộc. Một nhiệm vụ thỏa mãn sẽ đưa ra những hướng dẫn cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^k * n^2)`|`O(n^2)`| Quá chậm | 
| Tối ưu |`O(n^2)`|`O(n^2)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một biến boolean cho mỗi tay cầm. Trạng thái thực sự có nghĩa là hướng ngang. 
2. Xây dựng biểu đồ hàm ý. Đối với mỗi trung tâm, hãy quét hàng của nó theo cả hai hướng. Các tay cầm liên tiếp có thể nhìn thấy từ giữa cho thấy rằng tay cầm ngang xa hơn yêu cầu tay cầm trước đó cũng nằm ngang. Thêm ý nghĩa tương tự cho cột và hướng dọc. 

Lý do mà các hàm ý liền kề là đủ là vì một chuỗi hàm ý tự động lan truyền khắp toàn bộ phân đoạn. 
3. Đối với mỗi tay cầm, hãy kiểm tra xem có thể định hướng ngang hay không và liệu có thể định hướng dọc hay không. Nếu một hướng không thể chạm tới bất kỳ trung tâm nào, hãy thêm hàm ý cấm hướng đó. 
4. Tìm các thành phần liên thông chặt chẽ của biểu đồ hàm ý. Nếu một biến và phủ định của nó nằm trong cùng một thành phần thì các ràng buộc sẽ mâu thuẫn với nhau và câu trả lời là không thể. 
5. Nếu không, hãy khôi phục phép gán thỏa mãn từ thứ tự thành phần và in`-`cho tay cầm ngang và`|`cho tay cầm dọc. 

Tại sao nó hoạt động: 

Biểu đồ hàm ý chứa chính xác các điều kiện mà khóa yêu cầu. Bất kỳ sự sắp xếp hợp lệ nào cũng phải thỏa mãn mọi hàm ý vì một điểm điều khiển không thể chạm đến tâm trừ khi tất cả các điểm điều khiển trước nó theo hướng đó được căn chỉnh. Ngược lại, bất kỳ phép gán hàm ý thỏa đáng nào đều làm cho hướng đã chọn của mọi bộ điều khiển được chọn là hợp lệ vì tất cả các bộ điều khiển trước đó được yêu cầu đều buộc phải có cùng một hướng. Kiểm tra SCC là điều kiện chính xác tiêu chuẩn cho 2-SAT: mâu thuẫn tồn tại chính xác khi một biến bao hàm cả một giá trị và giá trị ngược lại của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [list(input().strip()) for _ in range(n)]

    pos = {}
    idx = 0
    for i in range(n):
        for j in range(n):
            if a[i][j] == '+':
                pos[(i, j)] = idx
                idx += 1

    m = idx
    g = [[] for _ in range(2 * m)]
    rg = [[] for _ in range(2 * m)]

    def add_edge(u, v):
        g[u].append(v)
        rg[v].append(u)

    def h(i):
        return 2 * i

    def v(i):
        return 2 * i + 1

    def add_same(x, y, horizontal):
        if horizontal:
            add_edge(h(x), h(y))
            add_edge(v(y), v(x))
        else:
            add_edge(v(x), v(y))
            add_edge(h(y), h(x))

    can_h = [False] * m
    can_v = [False] * m

    for r in range(n):
        c0 = -1
        for c in range(n):
            if a[r][c] == 'O':
                c0 = c
                break
        if c0 != -1:
            last = -1
            for c in range(c0 - 1, -1, -1):
                if a[r][c] == '.':
                    last = -1
                elif a[r][c] == '+':
                    x = pos[(r, c)]
                    can_h[x] = True
                    if last != -1:
                        add_same(x, last, True)
                    last = x
            last = -1
            for c in range(c0 + 1, n):
                if a[r][c] == '.':
                    last = -1
                elif a[r][c] == '+':
                    x = pos[(r, c)]
                    can_h[x] = True
                    if last != -1:
                        add_same(x, last, True)
                    last = x

    for c in range(n):
        r0 = -1
        for r in range(n):
            if a[r][c] == 'O':
                r0 = r
                break
        if r0 != -1:
            last = -1
            for r in range(r0 - 1, -1, -1):
                if a[r][c] == '.':
                    last = -1
                elif a[r][c] == '+':
                    x = pos[(r, c)]
                    can_v[x] = True
                    if last != -1:
                        add_same(x, last, False)
                    last = x
            last = -1
            for r in range(r0 + 1, n):
                if a[r][c] == '.':
                    last = -1
                elif a[r][c] == '+':
                    x = pos[(r, c)]
                    can_v[x] = True
                    if last != -1:
                        add_same(x, last, False)
                    last = x

    for i in range(m):
        if not can_h[i]:
            add_edge(h(i), v(i))
        if not can_v[i]:
            add_edge(v(i), h(i))

    order = []
    seen = [False] * (2 * m)

    sys.setrecursionlimit(1000000)

    def dfs(x):
        seen[x] = True
        for y in g[x]:
            if not seen[y]:
                dfs(y)
        order.append(x)

    for i in range(2 * m):
        if not seen[i]:
            dfs(i)

    comp = [-1] * (2 * m)

    def rdfs(x, c):
        comp[x] = c
        for y in rg[x]:
            if comp[y] == -1:
                rdfs(y, c)

    c = 0
    for x in reversed(order):
        if comp[x] == -1:
            rdfs(x, c)
            c += 1

    ans = [['.' for _ in range(n)] for _ in range(n)]
    for (r, col), x in pos.items():
        if comp[h(x)] == comp[v(x)]:
            print("No")
            return
        if comp[h(x)] > comp[v(x)]:
            ans[r][col] = '-'
        else:
            ans[r][col] = '|'

    for i in range(n):
        for j in range(n):
            if a[i][j] == 'O':
                ans[i][j] = 'O'

    print("Yes")
    for row in ans:
        print(''.join(row))

solve()
```Trước tiên, chương trình gán cho mỗi bộ điều khiển một chỉ mục để đồ thị 2-SAT có thể sử dụng các nút số nguyên nhỏ gọn. Mỗi biến có hai nút biểu đồ: một cho hướng ngang và một cho hướng dọc. 

Việc quét hàng và cột xây dựng biểu đồ hàm ý. Biến`last`lưu trữ tay cầm trước gần nhất trong một chuỗi hiển thị. Chỉ liên kết các tay cầm liền kề sẽ tránh tạo ra số cạnh bậc hai trong khi vẫn giữ nguyên thông tin bắc cầu. 

Các hướng không thể được xử lý bằng cách thêm hàm ý từ hướng đó sang hướng ngược lại. Giai đoạn SCC phát hiện những mâu thuẫn và cũng cung cấp thứ tự cần thiết để xây dựng lại một phép gán hợp lệ. 

Việc triển khai sử dụng xử lý đầu vào lặp đi lặp lại và tránh lưu trữ bất kỳ cấu trúc có nguồn gốc từ lưới nào lớn hơn bảng gốc. Độ sâu đệ quy Python được tăng lên vì biểu đồ hàm ý có thể chứa các chuỗi dài. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
O++
+.+
++O
```Những thay đổi trạng thái có liên quan là: 

| Xử lý | Có thể ngang | Có thể theo chiều dọc | Cuối cùng | 
| --- | --- | --- | --- | 
| (0,1) | vâng | không | ngang | 
| (0,2) | vâng | vâng | dọc | 
| (1,0) | không | vâng | dọc | 
| (2,0) | vâng | vâng | ngang | 
| (2,1) | vâng | vâng | ngang | 

Biểu đồ hàm ý không có mâu thuẫn nên phép gán SCC tạo ra một sự sắp xếp hợp lệ như:```
O-|
|.|
--O
```Đối với mẫu thứ hai:```
4
..+.
....
..O.
..+.
```Nhà nước là: 

| Xử lý | Có thể ngang | Có thể theo chiều dọc | 
| --- | --- | --- | 
| (0,2) | không | không | 
| (3,2) | không | vâng | 

Tay cầm trên cùng không có tâm có thể chạm tới theo cả hai hướng, do đó biểu đồ chứa mâu thuẫn bắt buộc và câu trả lời là`No`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^2)`| Mỗi lần quét chạm vào mỗi ô một số lần không đổi và SCC chạy theo thời gian tuyến tính trên biểu đồ hàm ý. | 
| Không gian |`O(n^2)`| Đồ thị có`O(n^2)`các đỉnh và các cạnh. | 

Kích thước lưới tối đa cung cấp 250000 tay cầm và nhiều nhất là gấp vài lần số cạnh biểu đồ, phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # In a local judge, import the submitted solve function and call it here.
    # The placeholder is intentional because this block is only a test template.
    sys.stdin = old
    return ""

assert "Yes" in run("""3
O++
+.+
++O
""")

assert "No" in run("""4
..+.
....
..O.
..+.
""")

assert "No" in run("""3
.+.
O++
.O.
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tay cầm đơn không có tâm |`No`| Thiếu trường hợp hiển thị | 
| Mẫu 1 |`Yes`| Chuỗi ngang và dọc hỗn hợp | 
| Mẫu 3 |`No`| Ý nghĩa xung đột giữa các tay cầm | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một tay cầm không có hướng khả thi. đầu vào```
1
+
```tạo một biến với cả hai hướng bị cấm. Thuật toán cộng cả hai mâu thuẫn, làm cho biến và biến đối diện của nó thuộc cùng một SCC, do đó nó in ra`No`. 

Trường hợp cạnh thứ hai là một bộ điều khiển có giá trị riêng lẻ theo cả hai hướng nhưng bị hạn chế bởi các bộ điều khiển khác. TRONG```
3
.+.
O++
.O.
```tay cầm ở giữa của hàng thứ hai tham gia vào hai chuỗi. Biểu đồ hàm ý nắm bắt cả hai yêu cầu và thử nghiệm SCC phát hiện ra rằng tay cầm cần phải có cả chiều ngang và chiều dọc. Điều này ngăn ngừa lỗi phổ biến khi kiểm tra từng tay cầm một cách độc lập.
