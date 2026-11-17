---
title: "CF 196B - Mê cung vô tận"
description: "Chúng ta được cho một mê cung hữu hạn có kích thước n × m. Một số ô là tường, một số ô mở và một ô chứa vị trí bắt đầu S. Mê cung không chỉ được sử dụng một lần."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 196
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 124 (Div. 1)"
rating: 2000
weight: 196
solve_time_s: 99
verified: true
draft: false
---

[CF 196B - Mê cung vô tận](https://codeforces.com/problemset/problem/196/B) 

**Xếp hạng:** 2000 
**Thẻ:** dfs và các biểu đồ tương tự 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mê cung có kích thước hữu hạn`n × m`. Một số ô là tường, một số ô mở và một ô chứa vị trí bắt đầu`S`. 

Mê cung không chỉ được sử dụng một lần. Thay vào đó, toàn bộ lưới được lặp lại vô hạn theo mọi hướng, giống như xếp mặt phẳng bằng các bản sao của cùng một hình chữ nhật. Di chuyển ra ngoài bản sao này ngay lập tức nhập bản sao khác vào vị trí tương ứng. 

Câu hỏi đặt ra là liệu người chơi có thể di chuyển tùy ý ra xa vị trí xuất phát trong khi chỉ bước lên những ô không có tường hay không. 

Một cách hữu ích để suy nghĩ về mê cung vô tận là: 

Mọi vị trí trong mặt phẳng vô hạn đều tương ứng với: 

1. Một ô bên trong lưới ban đầu. 
2. Chúng ta hiện đang ở bản sao nào của lưới. 

Hai vị trí vô hạn khác nhau có thể ánh xạ tới cùng một`(row, col)`bên trong mê cung ban đầu nếu chúng thuộc về các bản sao khác nhau của ô xếp. 

Các ràng buộc đủ lớn để việc khám phá trực tiếp mê cung vô tận là không thể. Lưới ban đầu có thể chứa tới`1500 × 1500 = 2.25 million`tế bào. Bất kỳ thuật toán nào truy cập lại các ô nhiều lần hoặc lưu trữ thông tin cho vô số vị trí sẽ thất bại ngay lập tức. 

Việc duyệt đồ thị trên lưới ban đầu vẫn khả thi vì mỗi ô ban đầu chỉ xuất hiện một lần ở đó. MỘT`O(nm)`hoặc`O(nm log(nm))`thuật toán phù hợp thoải mái bên trong giới hạn. Bất cứ điều gì bậc hai về số lượng ô thì không. 

Phần khó khăn là việc xem lại cùng một ô ban đầu không phải lúc nào cũng dư thừa. Đạt được điều tương tự`(r, c)`từ các bản sao khác nhau của ô xếp vô hạn chính xác là thứ tạo ra chuyển động vô hạn. 

Hãy xem xét ví dụ này:```
2 2
S.
..
```Câu trả lời đúng là`"Yes"`. 

Một BFS ngây thơ chỉ trên lưới hữu hạn sẽ truy cập từng ô một lần và dừng lại, kết luận sai rằng chuyển động là hữu hạn. Trên thực tế, không có bức tường nào nên chúng ta có thể tiếp tục đi mãi qua các bản sao khác nhau của mê cung. 

Một trường hợp tinh vi khác là khi các chu trình chỉ tồn tại bên trong một vùng giới hạn.```
3 3
###
#S#
#.#
```Câu trả lời đúng là`"No"`. 

Người chơi có thể di chuyển qua lại giữa một vài ô mãi mãi, nhưng không bao giờ có thể trốn sang một bản sao khác của mê cung. Khoảng cách đi bộ vô hạn là không thể mặc dù biểu đồ chứa một chu trình. 

Một sai lầm nguy hiểm nữa là xử lý modulo không chính xác cho tọa độ âm. Trong Python,`%`đã hoạt động đúng cho vấn đề này, nhưng trong nhiều ngôn ngữ`(-1) % m`trở nên tiêu cực. Ví dụ:```
1 2
S.
```Di chuyển sang trái từ cột`0`nên bọc vào cột`1`của bản sao lân cận. Việc xử lý modulo không chính xác sẽ phá vỡ hoàn toàn quá trình chuyển đổi này. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xử lý mê cung vô hạn theo đúng nghĩa đen và chạy BFS hoặc DFS trên tất cả các tọa độ có thể tiếp cận`(x, y)`trong máy bay. 

Điều này hoạt động về mặt khái niệm vì mọi vị trí có thể tiếp cận đều có thể được khám phá thông qua việc truyền tải đồ thị thông thường. Nếu quá trình truyền tải không bao giờ kết thúc, câu trả lời là`"Yes"`. 

Vấn đề là đồ thị là vô hạn. Ngay cả khi chỉ còn một hành lang nhỏ, việc tìm kiếm có thể tiếp tục mãi mãi. Không có giới hạn trên hữu hạn về số lượng trạng thái được khám phá. 

Quan sát quan trọng là mô hình mê cung lặp lại theo chu kỳ. 

Giả sử chúng ta đứng ở tọa độ vô hạn`(x, y)`. Loại tế bào thực tế chỉ phụ thuộc vào:```
(x mod n, y mod m)
```Vì vậy, nhiều vị trí vô hạn tương ứng với cùng một ô bên trong lưới ban đầu. 

Bây giờ đến cái nhìn sâu sắc quan trọng. 

Nếu trong quá trình truyền tải chúng ta đến cùng một ô ban đầu`(r, c)`từ hai tọa độ vô hạn khác nhau thì ta đã khám phá được một chu trình không tầm thường xuyên qua mặt phẳng lát gạch. Từ thời điểm đó trở đi, chúng ta có thể tiếp tục chuyển đổi giữa các bản sao mãi mãi, vì vậy câu trả lời là`"Yes"`. 

Tại sao điều này làm việc? 

Bởi vì sự khác biệt giữa hai tọa độ vô hạn biểu thị một vectơ dịch chuyển giữa các bản sao của mê cung. Việc lặp lại chuỗi chuyển động tương ứng khiến chúng ta trôi đi xa vô tận. 

Điều này cho thấy sự đại diện trạng thái chính xác: 

Đối với mỗi ô lưới ban đầu, hãy lưu trữ tọa độ vô hạn đầu tiên đạt đến nó. 

Trong DFS hoặc BFS: 

- Nếu ô chưa bao giờ được truy cập, hãy ghi lại tọa độ vô hạn của nó. 
- Nếu ô được truy cập trước đó với tọa độ vô hạn khác, chúng tôi đã tìm thấy chuyển động vô hạn. 

Điều này hoàn toàn tránh việc khám phá vô số trạng thái. Mỗi ô ban đầu chỉ được xử lý một lần trong trường hợp bình thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mặt phẳng vô tận | Không giới hạn | Không giới hạn | Không thể | 
| DFS/BFS với ánh xạ tọa độ | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và xác định vị trí ô bắt đầu`S`. 
2. Chạy DFS hoặc BFS bắt đầu từ tọa độ vô hạn tương ứng với`S`. 
3. Với mỗi vị trí vô hạn được khám phá`(x, y)`, tính toán ô lưới gốc tương ứng của nó:```
rx = x % n
ry = y % m
```Điều này cho chúng ta biết chúng ta đang đứng trên ô xếp nào. 

1. Bỏ qua việc di chuyển nếu ô được ánh xạ là một bức tường. 
2. Đối với mỗi ô lưới gốc`(rx, ry)`, lưu trữ tọa độ vô hạn đầu tiên đạt đến nó. 

Ví dụ:```
vis[rx][ry] = (x, y)
```1. Khi đạt tới`(rx, ry)`một lần nữa: 

- Nếu tọa độ vô hạn được lưu trữ giống hệt với`(x, y)`, đây chỉ là một đường đi khác đến cùng một vị trí vô hạn. Bỏ qua nó. 
- Nếu tọa độ được lưu trữ khác với`(x, y)`, thì cùng một ô ban đầu được lấy từ hai bản sao khác nhau của mê cung. In`"Yes"`ngay lập tức. 
2. Tiếp tục di chuyển theo bốn hướng. 
3. Nếu quá trình truyền tải kết thúc mà không phát hiện thấy xung đột như vậy, hãy in`"No"`. 

### Tại sao nó hoạt động 

Bất biến là: 

Đối với mỗi ô lưới gốc, chúng tôi nhớ chính xác một tọa độ vô hạn có thể chạm tới nó. 

Nếu sau đó chúng ta đến cùng một ô ban đầu bằng cách sử dụng tọa độ vô hạn khác, thì sẽ tồn tại một đường dẫn có độ dịch chuyển ròng khác 0. Việc lặp đi lặp lại sự dịch chuyển đó đưa chúng ta đi qua vô số bản sao của mê cung. 

Ngược lại, nếu không tồn tại tọa độ trùng lặp với tọa độ khác nhau như vậy thì mọi trạng thái có thể truy cập đều tương ứng với chính xác một tọa độ vô hạn trên mỗi ô ban đầu. Vì chỉ có`nm`các ô ban đầu, vùng có thể truy cập là hữu hạn. 

Điều đó thiết lập cả hai hướng đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

n, m = map(int, input().split())

grid = []
sx = sy = -1

for i in range(n):
    row = input().strip()
    grid.append(row)

    for j, ch in enumerate(row):
        if ch == 'S':
            sx, sy = i, j

visited = [[None] * m for _ in range(n)]

dirs = [(-1, 0), (1, 0), (0, -1), (0, 1)]

def dfs(x, y):
    rx = x % n
    ry = y % m

    if grid[rx][ry] == '#':
        return False

    if visited[rx][ry] is not None:
        px, py = visited[rx][ry]

        if px != x or py != y:
            return True

        return False

    visited[rx][ry] = (x, y)

    for dx, dy in dirs:
        nx = x + dx
        ny = y + dy

        if dfs(nx, ny):
            return True

    return False

print("Yes" if dfs(sx, sy) else "No")
```Quá trình truyền tải hoạt động trên tọa độ vô hạn`(x, y)`, không chỉ trên các vị trí bên trong lưới ban đầu. Đây là ý tưởng cốt lõi giúp lưu giữ thông tin về bản sao của mê cung mà chúng ta đang truy cập. 

Hoạt động modulo ánh xạ mọi tọa độ vô hạn trở lại ô ban đầu:```
rx = x % n
ry = y % m
```Modulo của Python đã xử lý chính xác các số âm, điều này rất cần thiết khi chuyển động vượt qua ranh giới trên cùng hoặc bên trái. 

các`visited`mảng không lưu trữ booleans. Nó lưu trữ tọa độ vô hạn chính xác lần đầu tiên đạt tới từng ô lưới gốc. Sự khác biệt này là toàn bộ giải pháp. 

Giả định`(2, 3)`bên trong ô lần đầu tiên được tiếp cận từ tọa độ vô hạn`(2, 3)`. Nếu sau này chúng ta tiếp cận ô xếp tương tự từ`(2 + n, 3)`, thì chúng ta biết quá trình truyền tải đã đi vào một bản sao khác của mê cung. 

DFS dừng ngay lập tức khi xung đột như vậy xuất hiện. 

Một chi tiết triển khai rất dễ mắc sai lầm là xem lại chính xác tọa độ vô hạn đó. Điều đó vô hại và không nên kích hoạt`"Yes"`. 

Điều kiện này là kiểm tra quan trọng:```
if px != x or py != y:
    return True
```Giới hạn đệ quy được tăng lên vì thành phần có thể truy cập được có thể chứa hàng triệu ô trong một chuỗi dài. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4
##.#
##S#
#..#
#.##
#..#
```Dấu vết truyền tải: 

| Vị trí vô hạn | Ô được ánh xạ | Được lưu trữ trước | Hành động | 
| --- | --- | --- | --- | 
| (1,2) | (1,2) | Không | Cửa hàng | 
| (2,2) | (2,2) | Không | Cửa hàng | 
| (2,1) | (2,1) | Không | Cửa hàng | 
| (3,1) | (3,1) | Không | Cửa hàng | 
| (4,1) | (4,1) | Không | Cửa hàng | 
| (5,1) | (0,1) | Tường | Dừng lại | 
| (4,2) | (4,2) | Không | Cửa hàng | 
| (5,2) | (0,2) | Không | Cửa hàng | 
| (6,2) | (1,2) | Có, từ (1,2) | Tọa độ khác nhau, trả lời Có | 

Thời điểm quan trọng là tiếp cận ô được ánh xạ`(1,2)`từ hai tọa độ vô hạn khác nhau:```
(1,2)
(6,2)
```Sự khác biệt của họ là`(5,0)`, đúng một lần dịch chuyển ô theo chiều dọc. Điều đó có nghĩa là đường dẫn có thể được lặp lại vô thời hạn. 

### Ví dụ 2 

đầu vào:```
3 3
###
#S#
#.#
```Dấu vết truyền tải: 

| Vị trí vô hạn | Ô được ánh xạ | Được lưu trữ trước | Hành động | 
| --- | --- | --- | --- | 
| (1,1) | (1,1) | Không | Cửa hàng | 
| (2,1) | (2,1) | Không | Cửa hàng | 
| (3,1) | (0,1) | Tường | Dừng lại | 
| (1,1) | (1,1) | Cùng tọa độ | Bỏ qua | 

Việc truyền tải cuối cùng kết thúc mà không tìm thấy cùng một ô xếp từ các tọa độ vô hạn khác nhau. 

Khu vực có thể tiếp cận được giới hạn bên trong một khu vực bị mắc kẹt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô lưới gốc được xử lý tối đa một lần trước khi chấm dứt hoặc từ chối | 
| Không gian | O(nm) | Bảng đã truy cập lưu trữ một cặp tọa độ trên mỗi ô | 

Lưới chứa nhiều nhất`2.25 million`các ô, vì vậy độ phức tạp tuyến tính là cần thiết. Giải pháp chỉ thực hiện công việc liên tục trên mỗi ô có thể tiếp cận và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    n, m = map(int, input().split())

    grid = []
    sx = sy = -1

    for i in range(n):
        row = input().strip()
        grid.append(row)

        for j, ch in enumerate(row):
            if ch == 'S':
                sx, sy = i, j

    visited = [[None] * m for _ in range(n)]

    dirs = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    def dfs(x, y):
        rx = x % n
        ry = y % m

        if grid[rx][ry] == '#':
            return False

        if visited[rx][ry] is not None:
            px, py = visited[rx][ry]

            if px != x or py != y:
                return True

            return False

        visited[rx][ry] = (x, y)

        for dx, dy in dirs:
            if dfs(x + dx, y + dy):
                return True

        return False

    return "Yes\n" if dfs(sx, sy) else "No\n"

# provided sample
assert run(
"""5 4
##.#
##S#
#..#
#.##
#..#
"""
) == "Yes\n", "sample 1"

# minimum grid
assert run(
"""1 1
S
"""
) == "Yes\n", "single open cell repeats forever"

# fully trapped
assert run(
"""3 3
###
#S#
###
"""
) == "No\n", "completely enclosed"

# corridor blocked between copies
assert run(
"""1 3
S##
"""
) == "No\n", "cannot cross tile boundary"

# open strip
assert run(
"""1 2
S.
"""
) == "Yes\n", "infinite horizontal movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1x1`lưới mở |`Yes`| Nhập lại cùng một ô thông qua một bản sao khác | 
| Bắt đầu kèm theo đầy đủ |`No`| Không có lối thoát có thể tiếp cận | 
|`S##`|`No`| Vượt qua ranh giới gạch vẫn có thể là không thể | 
|`S.`|`Yes`| Trường hợp dịch vô hạn đơn giản | 

## Vỏ cạnh 

Hãy xem xét lưới nhỏ nhất có thể:```
1 1
S
```Ô đơn lặp lại vô tận theo mọi hướng. Bắt đầu từ`(0,0)`, di chuyển lên đạt`(-1,0)`, ánh xạ trở lại`(0,0)`. 

Thuật toán đầu tiên lưu trữ:```
visited[0][0] = (0,0)
```Sau đó nó đạt đến:```
(-1,0)
```ánh xạ tới cùng một ô xếp nhưng có tọa độ vô hạn khác nhau. Thuật toán in chính xác`"Yes"`. 

Bây giờ hãy xem xét một cấu hình bị mắc kẹt:```
3 3
###
#S#
###
```DFS chỉ khám phá`(1,1)`. Mỗi ô lân cận là một bức tường sau khi ánh xạ modulo. Không có tọa độ vô hạn thứ hai nào đến được cùng một ô xếp, do đó quá trình truyền tải kết thúc và câu trả lời là`"No"`. 

Cuối cùng, hãy xem xét việc gói qua tọa độ âm:```
1 2
S.
```Di chuyển sang trái từ`(0,0)`cho`(0,-1)`. Python tính toán:```
-1 % 2 == 1
```vì vậy vị trí được ánh xạ trở thành`(0,1)`, chính xác là ô xếp kề trong bản sao bên trái. 

Việc xử lý modulo không chính xác sẽ gây ra sự cố hoặc ánh xạ tới cột không hợp lệ, tạo ra câu trả lời sai.
