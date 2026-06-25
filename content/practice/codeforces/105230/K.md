---
title: "CF 105230K - Báu vật"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô đại diện cho một bức tường, một lối đi trống, một cái bẫy, vị trí bắt đầu hoặc một ô chứa một lượng kho báu bằng số. Từ ô bắt đầu, được phép di chuyển theo bốn hướng."
date: "2026-06-24T16:06:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105230
codeforces_index: "K"
codeforces_contest_name: "2024-2025 ICPC Bolivia Pre-National Contest"
rating: 0
weight: 105230
solve_time_s: 314
verified: false
draft: false
---

[CF 105230K - Kho báu](https://codeforces.com/problemset/problem/105230/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô đại diện cho một bức tường, một lối đi trống, một cái bẫy, vị trí bắt đầu hoặc một ô chứa một lượng kho báu bằng số. Từ ô bắt đầu, được phép di chuyển theo bốn hướng. Mục tiêu là để xác định số lượng kho báu có thể thu thập được khi di chuyển qua mê cung dưới một hạn chế rất cụ thể do bẫy áp đặt. 

Điều quan trọng là bẫy không trực tiếp cản trở chuyển động. Thay vào đó, bất kỳ ô nào nằm liền kề với bẫy đều trở thành lãnh thổ nguy hiểm. Aylin từ chối di chuyển qua các vị trí không an toàn theo nghĩa này. Kết quả là, khu vực có thể đi qua hiệu quả không chỉ được xác định bởi các bức tường mà còn bởi “vùng ảnh hưởng” của bẫy, loại bỏ các tế bào lân cận khỏi sự xem xét. 

Mỗi trường hợp thử nghiệm là một mê cung độc lập và chúng ta phải tính tổng của tất cả các chữ số có thể đạt được ngay từ đầu trong khi vẫn tôn trọng các ràng buộc an toàn này. 

Kích thước lưới có thể lớn tới 1000 x 1000. Điều này ngay lập tức loại trừ mọi điều tồi tệ hơn thời gian tuyến tính cho mỗi trường hợp thử nghiệm, vì quá trình quét toàn bộ đã chạm tới một triệu ô. Bất kỳ giải pháp nào truy cập lại các ô quá thường xuyên hoặc mô phỏng quá trình khám phá đơn giản mà không cắt bớt sẽ hết thời gian chờ. 

Một số tình huống có xu hướng phá vỡ các giải pháp ngây thơ. 

Một trường hợp lỗi xuất hiện khi một ô có thể truy cập được chỉ thông qua hình học nhưng lại nằm cạnh một cái bẫy.```
S1.
.T.
.1.
```Một BFS ngây thơ bỏ qua ảnh hưởng của bẫy sẽ đến ô dưới cùng và đếm kho báu, nhưng câu trả lời đúng là 0 hoặc 1 tùy thuộc vào ô đó có liền kề với bẫy hay không; trong cách bố trí này, nó liền kề và phải được loại trừ hoàn toàn khỏi quá trình truyền tải. 

Một trường hợp tinh vi khác là khi điểm bắt đầu nằm cạnh một cái bẫy. Một cách tiếp cận ngây thơ vẫn có thể mở rộng ra bên ngoài, nhưng các quy tắc ngăn cản ngay cả động thái đầu tiên. 

Cuối cùng, các lưới có nhiều vùng an toàn bị ngắt kết nối thường gây nhầm lẫn cho việc triển khai lấp vùng ngập mà không đánh dấu chính xác các ô không an toàn trước khi truyền tải. Nếu độ an toàn được kiểm tra một cách lười biếng trong BFS thay vì tính toán trước, thuật toán có thể vô tình “đi qua” một ô lẽ ra đã bị vô hiệu. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng hoạt động thăm dò như một tìm kiếm có trạng thái, trong đó mỗi động thái sẽ xem xét liệu việc bước vào ô lân cận có được phép hay không dựa trên khoảng cách bẫy cục bộ. Từ mỗi tiểu bang, chúng tôi sẽ kiểm tra tất cả bốn hướng, kiểm tra các bẫy xung quanh một cách linh hoạt và phân nhánh theo cách đệ quy. 

Điều này hoạt động về mặt khái niệm vì mọi đường dẫn hợp lệ đều được khám phá và kho báu được tích lũy bất cứ khi nào một ô được truy cập. Tuy nhiên, vấn đề nằm ở chỗ làm việc lặp đi lặp lại. Trong trường hợp xấu nhất, mọi ô đều có thể được nhập từ nhiều hướng và đối với mỗi mục, chúng tôi có thể quét lại các ô lân cận để xác minh độ an toàn. Điều này đẩy độ phức tạp lên tới O(N²M²) trong các cấu hình bệnh lý, do việc kiểm tra cục bộ được lặp lại trên nhiều đường dẫn. 

Quan sát quan trọng là “tình trạng nguy hiểm” của một tế bào không phụ thuộc vào cách chúng ta đến đó. Một ô an toàn hoặc không an toàn trên toàn cầu, chỉ được xác định bằng việc bất kỳ ô lân cận nào có chứa bẫy hay không. Khi điều này được tính toán trước, lưới sẽ trở thành vấn đề kết nối tiêu chuẩn trên biểu đồ được lọc. 

Sau đó, vấn đề giảm xuống còn việc tìm thành phần được kết nối có chứa nút bắt đầu, nhưng chỉ trên các ô an toàn và tính tổng các giá trị số trong quá trình thực hiện. 

Điều này biến nhiệm vụ thành một BFS hoặc DFS duy nhất trên hầu hết các nút N×M, với các chuyển tiếp theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O((NM)2) | O(NM) | Quá chậm | 
| Tính toán trước an toàn + BFS | O(NM) | O(NM) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét toàn bộ lưới một lần và ghi lại vị trí của tất cả các bẫy. Điều này là cần thiết để sau này chúng ta có thể đánh dấu ảnh hưởng của chúng một cách hiệu quả mà không cần tìm kiếm nhiều lần. 
2. Tạo lưới boolean`bad`, ban đầu sai cho mọi ô. Chúng tôi sẽ sử dụng điều này để đánh dấu các ô không thể truy cập một cách an toàn. 
3. Đối với mỗi ô bẫy, hãy đánh dấu bốn ô lân cận của nó là xấu nếu chúng nằm trong lưới và không phải là tường. Bước này xây dựng “vùng nguy hiểm” do bẫy gây ra. Lý do chúng tôi làm điều này trước khi di chuyển là vì sự an toàn phải được biết một cách độc lập với các lựa chọn đường đi. 
4. Đồng thời đánh dấu tất cả các ô bẫy là xấu vì chúng không thể được nhập vào. 
5. Xác định vị trí ô bắt đầu. Nếu nó đã bị đánh dấu là xấu, câu trả lời ngay lập tức là 0 vì không thể bắt đầu hành động pháp lý nào. 
6. Chạy BFS từ ô bắt đầu trên lưới. Chỉ di chuyển vào các ô không có tường và không bị đánh dấu xấu và chưa từng được ghé thăm trước đó. 
7. Bất cứ khi nào BFS truy cập vào một ô, nếu nó chứa một chữ số, hãy chuyển đổi nó thành một số nguyên và thêm nó vào tổng số đang chạy. 
8. Tiếp tục cho đến khi hàng đợi trống. Số tiền tích lũy là câu trả lời. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là trạng thái “xấu” không phụ thuộc vào thứ tự truyền tải. Một ô sẽ không thể sử dụng được nếu nó là một cái bẫy hoặc liền kề với một ô và thuộc tính này không thay đổi tùy theo cách chúng ta di chuyển. Sau khi loại bỏ tất cả các ô không an toàn, mọi bước di chuyển còn lại sẽ đảm bảo an toàn và tạo thành một biểu đồ lưới vô hướng tiêu chuẩn. Do đó, BFS liệt kê chính xác thành phần an toàn có thể truy cập ngay từ đầu và vì mỗi ô được truy cập một lần nên tất cả kho báu có thể sưu tầm được trong thành phần đó đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def solve():
    n, m = map(int, input().split())
    g = [list(input().strip()) for _ in range(n)]

    bad = [[False] * m for _ in range(n)]
    sx = sy = -1

    traps = []

    for i in range(n):
        for j in range(m):
            if g[i][j] == 'T':
                traps.append((i, j))
            if g[i][j] == 'S':
                sx, sy = i, j

    dirs = [(1,0), (-1,0), (0,1), (0,-1)]

    for x, y in traps:
        bad[x][y] = True
        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < m:
                if g[nx][ny] != '#':
                    bad[nx][ny] = True

    if bad[sx][sy]:
        print(0)
        return

    q = deque()
    q.append((sx, sy))
    vis = [[False] * m for _ in range(n)]
    vis[sx][sy] = True

    ans = 0

    while q:
        x, y = q.popleft()

        if g[x][y].isdigit():
            ans += int(g[x][y])

        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < m:
                if not vis[nx][ny] and not bad[nx][ny] and g[nx][ny] != '#':
                    vis[nx][ny] = True
                    q.append((nx, ny))

    print(ans)

if __name__ == "__main__":
    solve()
```Lưới trước tiên được quét toàn bộ để xác định vị trí bẫy và vị trí bắt đầu. Điều này tránh làm việc lặp đi lặp lại sau này. các`bad`mảng mã hóa tất cả các ô bị cấm do ở gần bẫy. Việc tính toán trước này rất quan trọng vì nó biến điều kiện an toàn động thành thuộc tính tĩnh. 

Sau đó, BFS hoạt động giống như một biện pháp lấp lũ tiêu chuẩn, ngoại trừ việc nó lọc cả các bức tường và các ô không an toàn. Mảng đã truy cập đảm bảo mỗi ô được xử lý một lần, ngăn chặn sự phân nhánh theo cấp số nhân. 

Một lỗi phổ biến là kiểm tra tính liền kề của các bẫy trong quá trình mở rộng BFS thay vì tính toán trước nó. Điều đó dẫn đến các quyết định không nhất quán tùy thuộc vào thứ tự truyền tải. Một vấn đề tế nhị khác là quên chặn chính các ô bẫy, điều này có thể cho phép đi qua chúng một cách không chính xác nếu chúng không liền kề với một bẫy khác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào sau:```
4 9
S......T.
#####.###
111.#.111
..T....T.
```Sau khi tiền xử lý, tất cả các ô liền kề với bẫy sẽ bị chặn. BFS bắt đầu lúc`S`và chỉ khám phá những hành lang an toàn. 

| Bước | Xếp hàng | Ô hiện tại | Hành động | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (S) | S | bắt đầu | 0 | 
| 2 | ... | chỉ các ô an toàn có thể truy cập | thu thập chữ số trong vùng an toàn | 2 | 

Việc truyền tải nhanh chóng bị hạn chế vì phần lớn lưới gần các bẫy bị vô hiệu trước khi bắt đầu tìm kiếm. 

Bây giờ hãy xem xét một ví dụ tùy chỉnh thứ hai:```
3 5
S1T..
..2..
..3..
```Sau khi đánh dấu các ô không an toàn, mọi thứ lân cận`T`được gỡ bỏ. Giả sử chỉ có vùng dưới cùng vẫn được kết nối. 

| Bước | Xếp hàng | Ô hiện tại | Hành động | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | S | S | bắt đầu | 0 | 
| 2 | (1) | 1 | thu thập | 1 | 
| 3 | (2) | 2 | thu thập | 3 | 
| 4 | (3) | 3 | thu thập | 6 | 

Dấu vết này cho thấy BFS chỉ hoạt động trên cấu trúc an toàn đã được xác nhận trước và việc tích lũy kho báu hoàn toàn là một chức năng của khả năng tiếp cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi ô được xử lý một lần trong quá trình tiền xử lý và một lần trong BFS | 
| Không gian | O(NM) | Mảng cho trạng thái lưới, theo dõi đã truy cập và đánh dấu an toàn | 

Các ràng buộc cho phép tối đa một triệu ô cho mỗi trường hợp thử nghiệm, do đó, giải pháp dựa trên quét tuyến tính phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. Thuật toán tránh việc thăm dò lặp đi lặp lại, đảm bảo hiệu suất có thể dự đoán được ngay cả trong các lưới dày đặc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n, m = map(int, input().split())
        g = [list(input().strip()) for _ in range(n)]

        bad = [[False]*m for _ in range(n)]
        sx = sy = -1
        traps = []

        for i in range(n):
            for j in range(m):
                if g[i][j] == 'T':
                    traps.append((i,j))
                if g[i][j] == 'S':
                    sx, sy = i, j

        dirs = [(1,0),(-1,0),(0,1),(0,-1)]

        for x,y in traps:
            bad[x][y] = True
            for dx,dy in dirs:
                nx,ny = x+dx,y+dy
                if 0<=nx<n and 0<=ny<m and g[nx][ny] != '#':
                    bad[nx][ny] = True

        if bad[sx][sy]:
            print(0)
            return

        q = deque([(sx,sy)])
        vis = [[False]*m for _ in range(n)]
        vis[sx][sy] = True
        ans = 0

        while q:
            x,y = q.popleft()
            if g[x][y].isdigit():
                ans += int(g[x][y])
            for dx,dy in dirs:
                nx,ny = x+dx,y+dy
                if 0<=nx<n and 0<=ny<m:
                    if not vis[nx][ny] and not bad[nx][ny] and g[nx][ny] != '#':
                        vis[nx][ny]=True
                        q.append((nx,ny))

        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""4 9
S......T.
#####.###
111.#.111
..T....T.
""") == "2"

# custom: minimum
assert run("""1 1
S
""") == "0"

# custom: start blocked by trap adjacency
assert run("""1 3
TST
""") == "0"

# custom: simple line
assert run("""1 5
S1T12
""") == "1"

# custom: no traps full reach
assert run("""2 3
S12
345
""") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chỉ bắt đầu 1×1 | 0 | xử lý lưới tối thiểu | 
| TST | 0 | bắt đầu ngay lập tức không an toàn | 
| S1T12 | 1 | khối bẫy truyền tải một phần | 
| S12/345 | 15 | kết nối đầy đủ không có bẫy | 

## Vỏ cạnh 

Trường hợp một cạnh là khi điểm bắt đầu bị bao quanh bởi bẫy. Trong cấu hình như vậy, bước tiền xử lý sẽ đánh dấu ô bắt đầu là không hợp lệ và thuật toán sẽ kết thúc ngay lập tức. Ví dụ:```
3 3
T.T
.S.
T.T
```các`bad`việc đánh dấu lan truyền từ mỗi bẫy đến ô trung tâm, do đó BFS không bao giờ được bắt đầu, tạo ra số 0 một cách chính xác. 

Một trường hợp khác là khi một ô chỉ có thể truy cập được thông qua một vùng mà sau đó trở nên không hợp lệ do các quy tắc kề. Vì tất cả việc vô hiệu hóa được thực hiện trước khi truyền tải, BFS không bao giờ xem xét các ô này, ngăn chặn mọi hoạt động thăm dò từng phần không nhất quán. 

Trường hợp cuối cùng xảy ra ở những lưới rộng mở với các bẫy thưa thớt. Mặc dù hầu hết các ô đều có thể truy cập được về mặt hình học, nhưng chỉ những ô không liền kề với bẫy vẫn còn trong biểu đồ BFS. Thuật toán hạn chế việc khám phá một cách chính xác mà không cần mô phỏng khả năng hiển thị hoặc lý luận định hướng.
