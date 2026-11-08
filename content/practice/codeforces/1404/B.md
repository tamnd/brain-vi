---
title: "CF 1404B - Thẻ cây"
description: "Chúng ta được cấp một cái cây trong đó hai người chơi bắt đầu ở các đỉnh khác nhau. Alice di chuyển đầu tiên. Ở mỗi lượt, Alice có thể nhảy tới bất kỳ đỉnh nào trong khoảng cách da và Bob có thể nhảy tới bất kỳ đỉnh nào trong khoảng cách db. Khoảng cách là độ dài đường đi ngắn nhất tiêu chuẩn trong cây."
date: "2026-06-14T17:13:28+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "games", "trees"]
categories: ["algorithms"]
codeforces_contest: 1404
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 668 (Div. 1)"
rating: 1900
weight: 1404
solve_time_s: 217
verified: true
draft: false
---

[CF 1404B - Thẻ cây](https://codeforces.com/problemset/problem/1404/B) 

**Xếp hạng:** 1900 
**Tags:** dfs và tương tự, dp, trò chơi, cây cối 
**Thời gian giải:** 3 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cái cây trong đó hai người chơi bắt đầu ở các đỉnh khác nhau. Alice di chuyển đầu tiên. Ở mỗi lượt, Alice có thể nhảy tới bất kỳ đỉnh nào trong khoảng cách`da`và Bob có thể nhảy tới bất kỳ đỉnh nào trong khoảng cách`db`. Khoảng cách là độ dài đường đi ngắn nhất tiêu chuẩn trong cây. 

Mục tiêu của Alice cuối cùng là đến cùng một đỉnh với Bob ở cuối một chuỗi hành động nào đó. Nếu điều đó không bao giờ xảy ra thì Bob coi như đã trốn thoát mãi mãi. 

Khía cạnh quan trọng là sự chuyển động không mang tính cục bộ từng bước một. Mỗi lần di chuyển cho phép nhảy đến bất kỳ vị trí nào trong bán kính theo chỉ số cây, điều này làm cho trò chơi thiên về khả năng tiếp cận trên các vùng lân cận lớn hơn là vùng lân cận. 

Các ràng buộc gợi ý một giải pháp gần tuyến tính cho mỗi trường hợp thử nghiệm, vì tổng số đỉnh trên tất cả các thử nghiệm nhiều nhất là 100000. Bất kỳ giải pháp nào liên tục khám phá cây trong mỗi lần di chuyển hoặc mô phỏng trò chơi đều ngay lập tức quá chậm. 

Một cách giải thích ngây thơ sẽ mô phỏng tất cả các vị trí có thể có sau mỗi lần di chuyển. Điều đó không thành công vì mỗi bước di chuyển sẽ mở rộng thành một quả bóng lớn trên cây và hệ số phân nhánh sẽ trở thành toàn bộ cây trong trường hợp xấu nhất. Ngay cả việc biểu diễn các tập hợp có thể tiếp cận một cách rõ ràng cũng trở thành bậc hai. 

Một dạng sai sót tinh vi hơn sẽ xuất hiện nếu chúng ta chỉ so sánh khoảng cách ban đầu. Ví dụ: nếu ban đầu Alice hơi xa Bob nhưng có khả năng di chuyển lớn hơn nhiều, cô ấy vẫn có thể bắt được anh ấy sau này do cấu trúc của cây. Ngược lại, Bob có thể trốn thoát ngay cả khi đóng ban đầu nếu bán kính di chuyển của anh ta lớn hơn nhiều. 

Thành phần còn thiếu quan trọng trong lý luận ngây thơ là cấu trúc cây giới hạn khoảng cách có thể thu hẹp hoặc tăng nhanh như thế nào và cấu trúc tổng thể, đặc biệt là đường kính, hạn chế các lối thoát. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ theo dõi tất cả các đỉnh mà mỗi người chơi có thể đạt tới sau mỗi lượt. Từ mỗi vị trí hiện tại, chúng tôi sẽ mở rộng tới tất cả các nút trong`da`hoặc`db`sử dụng BFS. Việc lặp đi lặp lại điều này dẫn đến việc khám phá các cây con chồng chéo lớn nhiều lần. Trong trường hợp xấu nhất, mỗi lần di chuyển đều tốn`O(n)`và vì trò chơi có thể kéo dài nhiều nước đi nên điều này sẽ không bị giới hạn trong giới hạn thời gian của vấn đề. 

Điểm mấu chốt là chúng ta không cần phải mô phỏng trò chơi theo thời gian. Kết quả chỉ phụ thuộc vào một tập hợp nhỏ các đại lượng cấu trúc: khoảng cách ban đầu giữa những người chơi và “chiều rộng” tổng thể của cây, được tính bằng đường kính của nó. 

Lý do chia thành ba chế độ. 

Nếu Alice có thể tiếp cận Bob chỉ bằng một nước đi, cô ấy sẽ thắng ngay lập tức. 

Nếu Bob tương đối chậm so với Alice, nghĩa là bước nhảy tối đa của anh ấy không quá hai lần Alice, thì cuối cùng Alice luôn có thể thu hẹp khoảng cách bất kể cấu trúc. Theo trực giác, Alice giảm khoảng cách mỗi giây di chuyển với tốc độ mà Bob không thể bù đắp được. 

Nếu Bob nhanh hơn đáng kể, anh ấy có thể cố gắng chạy về phía xa nhất của cái cây. Trong trường hợp đó, cách duy nhất Alice vẫn thắng là nếu cái cây quá “nhỏ” để Bob có thể duy trì sự tách biệt, nghĩa là đường kính không đủ lớn để hỗ trợ một hành lang thoát hiểm. 

Điều này làm giảm vấn đề trong việc tính toán khoảng cách đường đi ngắn nhất và đường kính cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) trường hợp xấu nhất | O(n) | Quá chậm | 
| Đường kính + Logic khoảng cách | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính khoảng cách giữa Alice và Bob 

Chúng tôi chạy BFS hoặc DFS để tính khoảng cách đường đi ngắn nhất`d(a, b)`trong cây. Điều này trực tiếp xác định liệu Alice có thể bắt Bob chỉ bằng một nước đi hay không. 

Nếu khoảng cách này lớn nhất`da`, Alice ngay lập tức thắng vì cô ấy có thể nhảy thẳng vào vị trí của Bob. 

### 2. Tính đường kính của cây 

Chúng tôi tìm đường kính cây bằng cách sử dụng hai đường BFS. BFS đầu tiên từ bất kỳ nút nào sẽ tìm thấy nút xa nhất`x`, sau đó là BFS thứ hai từ`x`tìm khoảng cách xa nhất`D`. 

Đường kính này thể hiện khoảng cách tối đa có thể có giữa hai nút bất kỳ trong cây và đóng vai trò là giới hạn về khoảng cách Bob có thể liên tục rút lui. 

### 3. Kiểm tra xem Bob có quá nhanh so với Alice không 

Nếu`db <= 2 * da`, Alice luôn thắng. 

Trực giác cho thấy ngay cả khi Bob di chuyển ra xa một cách tối ưu, Alice vẫn có thể giảm khoảng cách hiệu quả qua các lượt rẽ xen kẽ. Bob không thể tăng sự phân tách nhanh hơn Alice có thể vô hiệu hóa nó. 

### 4. Xử lý chế độ fast-Bob 

Nếu`db > 2 * da`, Bob có khả năng duy trì khoảng cách bằng cách luôn di chuyển về phía phần xa nhất của cây. 

Trong trường hợp này, Alice chỉ thắng nếu cây quá nhỏ để có thể thoát ra liên tục, tức là đường kính thỏa mãn`D <= 2 * da`. 

Nếu đường kính lớn hơn mức này, Bob có thể tiếp tục định vị lại về phía điểm cuối ở xa và ngăn chặn việc bắt giữ. 

### 5. Điều kiện kết hợp 

Alice thắng nếu có bất kỳ điều nào sau đây: 

-`d(a, b) <= da`-`db <= 2 * da`-`D <= 2 * da`Ngược lại Bob sẽ thắng. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là khả năng tránh bị bắt của Bob phụ thuộc vào việc duy trì sự phân tách không giảm dọc theo con đường dài nhất trong cây. Nếu cây đủ “rộng” (đường kính lớn) và Bob đủ nhanh hơn Alice, anh ta luôn có thể định vị lại để duy trì khoảng cách lớn hơn bán kính bắt của Alice. 

Nếu Bob không đủ nhanh hơn, lợi thế luân phiên của Alice đảm bảo rằng mọi sự tách biệt cuối cùng sẽ sụp đổ. Điều kiện đường kính trở nên không liên quan trong chế độ đó vì Bob không thể khai thác cấu trúc toàn cục đủ nhanh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def bfs(start, g):
    n = len(g) - 1
    dist = [-1] * (n + 1)
    q = deque([start])
    dist[start] = 0
    far = start
    while q:
        v = q.popleft()
        for to in g[v]:
            if dist[to] == -1:
                dist[to] = dist[v] + 1
                q.append(to)
                if dist[to] > dist[far]:
                    far = to
    return far, dist

def distance(a, b, g):
    _, dist = bfs(a, g)
    return dist[b]

def diameter(g):
    x, _ = bfs(1, g)
    y, dist = bfs(x, g)
    return dist[y]

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, a, b, da, db = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        d = distance(a, b, g)
        D = diameter(g)

        if d <= da:
            out.append("Alice")
        elif db <= 2 * da:
            out.append("Alice")
        elif D <= 2 * da:
            out.append("Alice")
        else:
            out.append("Bob")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tách logic thành ba phép đo độc lập: khoảng cách Alice-Bob, đường kính cây và so sánh tốc độ. Quy trình BFS được sử dụng lại cho cả truy vấn khoảng cách và tính toán đường kính để tránh logic truyền tải trùng lặp. 

Một cạm bẫy phổ biến là tính toán lại khoảng cách nhiều lần cho mỗi trường hợp thử nghiệm. Ở đây mọi thứ được tính toán theo thời gian tuyến tính trên mỗi cây, đảm bảo tổng độ phức tạp nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 2 1 2
1 2
1 3
1 4
```Chúng tôi tính toán khoảng cách`d(3,2)=2`. Bước nhảy của Alice là`da=1`, nên cô ấy không thể liên lạc được với Bob ngay lập tức. 

Đường kính của cây hình ngôi sao này là 2. 

Chúng tôi kiểm tra các điều kiện: 

| Số lượng | Giá trị | 
| --- | --- | 
| d(a,b) | 2 | 
| da | 1 | 
| db | 2 | 
| D | 2 | 

Từ`db <= 2 * da`là`2 <= 2`, Alice thắng ngay lập tức. 

Điều này chứng tỏ chế độ Bob không đủ nhanh để duy trì sự tách biệt ngay cả khi đang ở trên một cây phân nhánh. 

### Ví dụ 2 

đầu vào:```
6 6 1 2 5
1 2
2 3
3 4
4 5
5 6
```Đây là một biểu đồ đường dẫn. Khoảng cách giữa các điểm cuối là 5. 

| Số lượng | Giá trị | 
| --- | --- | 
| d(a,b) | 5 | 
| da | 2 | 
| db | 5 | 
| D | 5 | 

Đây`db > 2 * da`(5 > 4) và cả khoảng cách và đường kính ban đầu đều vượt quá ngưỡng tầm với của Alice. 

Bob luôn có thể rút lui về điểm cuối xa, duy trì sự tách biệt. Điều này xác nhận Bob thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi BFS là tuyến tính theo các nút và chúng tôi chạy một số lượng không đổi cho mỗi lần kiểm tra | 
| Không gian | O(n) | danh sách kề cộng với mảng khoảng cách | 

Tổng số tiền của`n`trên tất cả các thử nghiệm được giới hạn bởi 100000, vì vậy giải pháp chạy thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import deque

    def bfs(start, g):
        n = len(g) - 1
        dist = [-1] * (n + 1)
        q = deque([start])
        dist[start] = 0
        far = start
        while q:
            v = q.popleft()
            for to in g[v]:
                if dist[to] == -1:
                    dist[to] = dist[v] + 1
                    q.append(to)
                    if dist[to] > dist[far]:
                        far = to
        return far, dist

    def distance(a, b, g):
        _, dist = bfs(a, g)
        return dist[b]

    def diameter(g):
        x, _ = bfs(1, g)
        y, dist = bfs(x, g)
        return dist[y]

    t = int(input())
    out = []
    for _ in range(t):
        n, a, b, da, db = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        d = distance(a, b, g)
        D = diameter(g)

        if d <= da or db <= 2 * da or D <= 2 * da:
            out.append("Alice")
        else:
            out.append("Bob")

    return "\n".join(out) + "\n"

# provided samples
assert run("""4
4 3 2 1 2
1 2
1 3
1 4
6 6 1 2 5
1 2
6 5
2 3
3 4
4 5
9 3 9 2 5
1 2
1 6
1 9
1 3
9 5
7 9
4 8
4 3
11 8 11 3 3
1 2
11 9
4 9
6 5
2 10
3 2
5 9
8 3
7 4
7 10
""") == """Alice
Bob
Alice
Alice
"""

# minimum case
assert run("""1
2 1 2 1 1
1 2
""") == "Alice\n"

# line case
assert run("""1
5 1 5 1 1
1 2
2 3
3 4
4 5
""") == "Alice\n"

# fast Bob
assert run("""1
5 1 5 1 10
1 2
2 3
3 4
4 5
""") == "Bob\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây tối thiểu | Alice | chụp liền kề cơ sở | 
| đồ thị đường | Alice | hành vi cạnh đường kính | 
| Bob nhanh | Bob | chế độ thoát hiểm tốc độ cao | 

## Vỏ cạnh 

Trường hợp nguy hiểm xảy ra khi Alice và Bob đã ở gần nhau nhưng Bob có phạm vi di chuyển lớn. Ngay cả khi Bob có thể di chuyển xa, Alice vẫn có thể thắng nếu đường kính đủ nhỏ để ngăn chặn việc né tránh liên tục. điều kiện`D <= 2 * da`nắm bắt điều này bằng cách hạn chế khả năng của Bob trong việc khai thác các lối thoát dài. 

Một trường hợp tinh tế khác là khi Bob nhanh hơn nhưng cây lại rất mất cân bằng. Ngay cả khi đường kính nhỏ, Bob không thể liên tục thiết lập lại khoảng cách. Thuật toán phân loại chính xác những trường hợp như Alice thắng. 

Cuối cùng, ở những cây rất nhỏ như một cạnh, việc so sánh khoảng cách trực tiếp chiếm ưu thế. Kiểm tra khoảng cách BFS đảm bảo tính chính xác mà không cần dựa vào lý luận về đường kính.
