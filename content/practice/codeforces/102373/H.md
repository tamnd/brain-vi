---
title: "CF 102373H - Thoát khỏi ngôi nhà bỏ hoang"
description: "Lưới là một biểu đồ có các đỉnh đều là các ô không có vách, với các cạnh giữa các ô có chung một cạnh. Những người bạn bắt đầu từ s và cần đạt tới f. Mỗi lần di chuyển theo chiều ngang sẽ thay đổi nhiệt độ đi -1, bất kể di chuyển sang trái hay sang phải."
date: "2026-08-12T23:09:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102373
codeforces_index: "H"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102373
solve_time_s: 456
verified: true
draft: false
---

[CF 102373H - Thoát khỏi ngôi nhà bị bỏ hoang](https://codeforces.com/problemset/problem/102373/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới là một biểu đồ có các đỉnh đều là các ô không có vách, với các cạnh giữa các ô có chung một cạnh. Những người bạn bắt đầu lúc`s`và cần đạt được`f`. 

Mỗi chuyển động ngang làm thay đổi nhiệt độ theo`-1`, bất kể di chuyển sang trái hay phải. Mỗi bước di chuyển theo chiều dọc sẽ thay đổi nó bằng cách`+1`, bất kể chuyển động đi lên hay đi xuống. Vì vậy, để đi dạo có chứa`H`chuyển động ngang và`V`chuyển động thẳng đứng thì nhiệt độ cuối cùng khác với nhiệt độ ban đầu một khoảng 

[ 
V-H. 
] 

Bạn bè có thể xem lại các ô và cạnh tùy ý nhiều lần. Chúng ta cần giá trị nhỏ nhất có thể của 

[ 
|V-H|. 
]

Nếu như`f`không thể truy cập được từ`s`, câu trả lời là`-1`. 

Lưới có thể chứa tối đa (10^6) ô. Điều đó ngay lập tức loại trừ bất kỳ thứ gì lưu trữ trạng thái lớn cho mọi giá trị nhiệt độ hoặc độ dài đường dẫn có thể có. Chúng ta chỉ cần kiểm tra từng ô và từng vùng lân cận cục bộ với số lần không đổi, đưa ra mục tiêu (O(nm)). Việc truyền tải với bộ nhớ tuyến tính cũng phù hợp một cách tự nhiên vì chỉ có (O(nm)) ô có thể truy cập được. 

Có một số trường hợp phím tắt tưởng chừng như hợp lý lại cho ra kết quả sai. 

Hãy xem xét một hành lang ngang duy nhất.```
1 3
s.f
```Đường đi hữu ích duy nhất có hai lần di chuyển theo chiều ngang, do đó nhiệt độ thay đổi theo`-2`và câu trả lời là`2`. Một giải pháp giả định câu trả lời luôn được xác định chỉ bằng tính chẵn lẻ của độ dài đường dẫn sẽ trả về không chính xác`0`, bởi vì thành phần này không có cạnh thẳng đứng để bù cho các chuyển động ngang. 

Tương tự dọc có cùng một vấn đề.```
3 1
s
.
f
```Mọi chuyển động đều theo phương thẳng đứng nên đường đi ngắn nhất sẽ thay đổi nhiệt độ theo`+2`. Câu trả lời đúng là`2`. 

Một lối thoát bị ngắt kết nối cũng phải được xử lý riêng.```
1 3
s#f
```Không có đường đi bộ từ`s`ĐẾN`f`, vậy câu trả lời là`-1`. Thực hiện phép tính trên tọa độ của`s`Và`f`không kiểm tra kết nối sẽ âm thầm tạo ra một giá trị hữu hạn. 

Cuối cùng, có cả cạnh ngang và dọc không có nghĩa là câu trả lời luôn bằng 0. Coi như```
2 2
sf
..
```Có đường đi ngang trực tiếp với sự thay đổi nhiệt độ`-1`. Bởi vì thành phần chứa cả hai loại cạnh, chúng ta có thể thay đổi bất kỳ sự thay đổi nhiệt độ nào của bước đi theo bội số của`2`, nhưng chúng ta không bao giờ có thể thay đổi tính chẵn lẻ của nó. Vì mọi`s`ĐẾN`f`bước đi ở đây có chiều dài lẻ, sự thay đổi nhiệt độ của nó luôn là số lẻ. Câu trả lời đúng là`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ liệt kê số lần đi bộ từ`s`và theo dõi sự thay đổi nhiệt độ tích lũy của chúng. Điều này đúng về mặt khái niệm vì mỗi lần đi bộ có thể tương ứng với một nhiệt độ cuối cùng có thể có. Vấn đề là các lần đi bộ có thể quay lại các ô, vì vậy có vô số lần đi bộ. Ngay cả khi chúng tôi hạn chế tìm kiếm một cách giả tạo ở các bước có độ dài tối đa (L), một đỉnh lưới có thể có tối đa bốn lựa chọn ở mỗi bước, đưa ra (O(4^L)) ứng viên sẽ đi bộ trong trường hợp xấu nhất. Việc tăng (L) không giải quyết được vấn đề một cách rõ ràng vì bước đi tối ưu hợp lệ có thể cố tình chứa các lần lặp lại. 

Cố gắng chỉ liệt kê các đường dẫn đơn giản không phải là sự thay thế hợp lệ. Tuyên bố này rõ ràng cho phép xem lại các ô và những lần lặp lại đó chính xác là những gì cho phép chúng tôi bù đắp những thay đổi về nhiệt độ. 

Quan sát quan trọng xuất phát từ việc xem xét một cạnh được duyệt hai lần. Nếu cạnh nằm ngang, đi ngang qua nó và quay trở lại ngay lập tức, chi phí`-2`. Nếu cạnh thẳng đứng, đi ngang qua nó và ngay lập tức quay trở lại chi phí`+2`. Vì những người bạn có thể chèn một chuyến đi khứ hồi như vậy vào bất cứ đâu dọc theo một chuyến đi bộ hợp lệ, nên sự hiện diện của cạnh ngang cho phép chúng ta giảm tổng số đi`2`bất cứ khi nào chúng ta muốn, trong khi sự hiện diện của cạnh thẳng đứng cho phép chúng ta tăng tổng số lên`2`bất cứ khi nào chúng tôi muốn. 

Giả sử thành phần được kết nối có chứa`s`Và`f`có cả cạnh ngang và cạnh dọc. Bắt đầu với bất kỳ`s`ĐẾN`f`đường đi có nhiệt độ thay đổi là (W). Bằng cách chèn liên tục một hành trình khứ hồi theo chiều ngang, chúng ta có thể thay thế (W) bằng (W-2k) và bằng cách chèn liên tục một hành trình khứ hồi theo chiều dọc, chúng ta có thể thay thế nó bằng (W+2k). Do đó, mọi số nguyên có cùng tính chẵn lẻ với (W) đều có thể đạt được. 

Tính chẵn lẻ của (W) đặc biệt đơn giản. Mỗi lần di chuyển sẽ thay đổi độ dài đường đi một lần và 

[ 
V-H=(V+H)-2H. 
] 

Do đó (V-H) có cùng tính chẵn lẻ với tổng số nước đi. Mỗi con đường từ`s`ĐẾN`f`có cùng độ dài chẵn lẻ, vì việc thay đổi độ chẵn lẻ của tọa độ lưới yêu cầu một lần di chuyển. Do đó, khi cả hai loại cạnh đều tồn tại, câu trả lời là`0`cho độ dài đường dẫn chẵn và`1`cho một đường đi có chiều dài lẻ. 

Có một trường hợp đặc biệt. Nếu thành phần chỉ chứa các cạnh ngang, mọi bước đi có thể chỉ có các chuyển động ngang, do đó sự thay đổi nhiệt độ của nó chính xác là âm của chiều dài của nó. Giá trị tuyệt đối nhỏ nhất sau đó đạt được bằng đường đi ngắn nhất từ`s`ĐẾN`f`. Lý do tương tự cũng được áp dụng nếu thành phần chỉ chứa các cạnh thẳng đứng, ngoại trừ sự thay đổi nhiệt độ là độ dài đường dẫn dương. 

Một BFS duy nhất cung cấp cho chúng tôi mọi thứ chúng tôi cần. Nó tìm khoảng cách ngắn nhất để`f`, xác định liệu`f`có thể truy cập được và trong khi duyệt thành phần, nó sẽ cho chúng ta biết liệu có ít nhất một cạnh ngang và ít nhất một cạnh dọc hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn, (O(4^L)) cho các bước đi có chiều dài (L) | Không giới hạn với độ sâu tìm kiếm | Quá chậm | 
| Tối ưu | (O(nm)) | (O(nm)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và xác định vị trí ô bắt đầu`s`và thoát khỏi ô`f`. Đối xử với mọi nhân vật ngoại trừ`#`như có thể đi qua được. 
2. Chạy BFS từ`s`. Lưu trữ khoảng cách ngắn nhất từ`s`tới mọi ô được truy cập. BFS phù hợp vì mỗi lần di chuyển lưới đều có độ dài đơn vị, do đó, lần đầu tiên đến một ô sẽ có độ dài đường đi ngắn nhất. 
3. Trong khi kiểm tra một ô và từng ô lân cận có thể tiếp cận được, hãy phân loại cạnh tương ứng. Nếu hàng thay đổi, thành phần đó chứa cạnh thẳng đứng. Nếu cột thay đổi, nó chứa cạnh ngang. Chúng ta chỉ cần hai cờ boolean. 
4. Nếu`f`chưa bao giờ được truy cập, xuất ra`-1`. Những người bạn không thể đến được lối ra. 
5. Nếu thành phần chứa cả cạnh ngang và cạnh dọc, hãy xuất`dist[f] % 2`. Độ dài đường đi ngắn nhất xác định tính chẵn lẻ của mọi thay đổi nhiệt độ có thể xảy ra và cả hai dấu hiệu của sự thay đổi theo`2`có sẵn thông qua các chuyến đi khứ hồi. Do đó, chẵn lẻ cho phép loại bỏ chính xác, trong khi chẵn lẻ để lại sự khác biệt tuyệt đối tối thiểu`1`. 
6. Nếu thành phần chỉ có các cạnh ngang, xuất ra`dist[f]`. Mỗi bước đi có thể có sự thay đổi nhiệt độ bằng âm độ dài của nó, vì vậy đường đi ngắn nhất là tối ưu. 
7. Nếu thành phần chỉ có các cạnh thẳng đứng, xuất ra`dist[f]`cũng vậy. Mỗi bước đi có thể có sự thay đổi nhiệt độ bằng với chiều dài của nó, do đó, một lần nữa con đường ngắn nhất là tối ưu. 

### Tại sao nó hoạt động 

Bất biến trung tâm là việc chèn một hành trình khứ hồi cạnh ngang sẽ thay đổi nhiệt độ một cách chính xác`-2`, trong khi chèn một chuyến đi khứ hồi theo cạnh dọc sẽ thay đổi nó một cách chính xác`+2`. Nếu cả hai loại cạnh xảy ra trong thành phần được kết nối, thì mọi thay đổi nhiệt độ của bước đi đều có thể được thay đổi bởi bất kỳ số nguyên chẵn nào. Tính chẵn lẻ không thể thay đổi, vì sự thay đổi nhiệt độ (V-H) có cùng tính chẵn lẻ với số lần di chuyển và mọi`s`ĐẾN`f`walk có cùng tính chẵn lẻ điểm cuối. Do đó, giá trị có thể đạt được gần nhất với 0 chính xác là`0`để có sự chẵn lẻ và`1`cho tính chẵn lẻ lẻ. 

Nếu chỉ tồn tại một loại cạnh thì mỗi bước đi đều có dấu hiệu thay đổi nhiệt độ cố định và độ lớn của nó bằng với chiều dài của nó. Do đó, con đường ngắn nhất sẽ giảm thiểu chênh lệch nhiệt độ tuyệt đối. BFS tính toán chính xác độ dài ngắn nhất đó. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    start = None
    finish = None

    for r in range(n):
        for c in range(m):
            if grid[r][c] == 's':
                start = (r, c)
            elif grid[r][c] == 'f':
                finish = (r, c)

    sr, sc = start
    fr, fc = finish

    dist = [[-1] * m for _ in range(n)]
    dist[sr][sc] = 0

    q = deque([(sr, sc)])

    has_horizontal = False
    has_vertical = False

    directions = ((1, 0), (-1, 0), (0, 1), (0, -1))

    while q:
        r, c = q.popleft()

        for dr, dc in directions:
            nr = r + dr
            nc = c + dc

            if nr < 0 or nr >= n or nc < 0 or nc >= m:
                continue
            if grid[nr][nc] == '#':
                continue

            if dr == 0:
                has_horizontal = True
            else:
                has_vertical = True

            if dist[nr][nc] != -1:
                continue

            dist[nr][nc] = dist[r][c] + 1
            q.append((nr, nc))

    d = dist[fr][fc]

    if d == -1:
        print(-1)
        return

    if has_horizontal and has_vertical:
        print(d & 1)
    else:
        print(d)

if __name__ == "__main__":
    solve()
```Lưới được lưu trữ dưới dạng danh sách các chuỗi, vì vậy việc kiểm tra xem một ô có phải là một bức tường hay không là một thao tác liên tục. Hai ô đặc biệt nằm trong lần quét đầu tiên. 

Mảng khoảng cách BFS sử dụng`-1`cho các ô chưa được thăm và`0`để bắt đầu. Vì mỗi hành động hợp pháp đều tốn một bước,`dist[f]`là độ dài đường đi ngắn nhất bất cứ khi nào có thể đến được lối ra. 

Cờ loại cạnh chỉ được cập nhật cho các hàng xóm hợp lệ, không có tường. Sự kề cận theo chiều ngang được phát hiện bởi`dr == 0`, trong khi một sự kề cận theo chiều dọc có`dr != 0`. Có thể gặp cùng một cạnh từ cả hai điểm cuối, nhưng việc đặt boolean hai lần không có hiệu lực. 

Việc kiểm tra ranh giới phải diễn ra trước khi lập chỉ mục cho lưới. Điều này tránh việc vô tình coi các chỉ số Python phủ định là các ô hợp lệ, đây là một lỗi đặc biệt dễ mắc phải trong lưới BFS. 

Không có vấn đề tràn số nguyên trong Python. Ngay cả khi triển khai C++, khoảng cách ngắn nhất tối đa chỉ là (O(nm)), do đó, loại số nguyên tiêu chuẩn là đủ. 

Quyết định cuối cùng có chủ ý sử dụng khoảng cách BFS thay vì sự thay đổi nhiệt độ thực tế của đường dẫn đó. Khi cả hai loại cạnh đều tồn tại, chỉ có tính chẵn lẻ của nó là quan trọng. Khi thiếu một loại, khoảng cách chính là sự thay đổi tuyệt đối tối thiểu có thể có. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Lưới là```
4 3
..f
..#
s##
...
```Bắt đầu lúc`(2,0)`, BFS có thể đạt tới`(1,0)`, sau đó`(0,0)`, sau đó là lối ra`(0,2)`. Thành phần này chứa cả cạnh dọc và cạnh ngang. 

| Ô hiện tại | Khoảng cách | Tế bào mới | Loại cạnh | Khoảng cách mới | 
| --- | --- | --- | --- | --- | 
|`(2,0)`| 0 |`(1,0)`| Dọc | 1 | 
|`(1,0)`| 1 |`(0,0)`| Dọc | 2 | 
|`(0,0)`| 2 |`(0,1)`| Ngang | 3 | 
|`(0,1)`| 3 |`(0,2)`| Ngang | 4 | 

Khoảng cách ngắn nhất để`f`là`4`, tức là chẵn. Vì thành phần có cả hai loại cạnh nên câu trả lời là`4 % 2 = 0`. 

Bản thân đường dẫn trực tiếp đã có hai chuyển động dọc và hai chuyển động ngang, làm thay đổi nhiệt độ`2 - 2 = 0`. Đối số chẵn lẻ BFS xác nhận rằng có thể hủy bỏ chính xác. 

### Ví dụ tùy chỉnh 2 

Hãy xem xét một hành lang một hàng.```
1 3
s.f
```Không có cạnh dọc. 

| Ô hiện tại | Khoảng cách | Tế bào mới | Loại cạnh | Khoảng cách mới | 
| --- | --- | --- | --- | --- | 
|`(0,0)`| 0 |`(0,1)`| Ngang | 1 | 
|`(0,1)`| 1 |`(0,2)`| Ngang | 2 | 

Lối ra có khoảng cách`2`và thành phần chỉ chứa các cạnh ngang. Mỗi lần đi bộ có thể có sự thay đổi nhiệt độ bằng âm chiều dài của nó. Độ dài ngắn nhất có thể là`2`, vậy câu trả lời là`2`. 

Ví dụ này chứng tỏ tại sao phải kiểm tra sự hiện diện của cả hai loại cạnh thay vì chỉ trả về tính chẵn lẻ của đường đi ngắn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm)) | Mỗi ô được chèn vào BFS tối đa một lần và mỗi ô được truy cập sẽ kiểm tra tối đa bốn ô lân cận. | 
| Không gian | (O(nm)) | Lưới, mảng khoảng cách và hàng đợi BFS sử dụng không gian tuyến tính theo số lượng ô. | 

Với (n,m \le 1000), có nhiều nhất (10^6) ô. Thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi ô và vùng lân cận, do đó, nó chia tỷ lệ tuyến tính với toàn bộ lưới thay vì theo số bước đi có thể có. Mức tiêu thụ bộ nhớ cũng tuyến tính và vẫn phù hợp với lưới điện một triệu ô. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    start = None
    finish = None

    for r in range(n):
        for c in range(m):
            if grid[r][c] == 's':
                start = (r, c)
            elif grid[r][c] == 'f':
                finish = (r, c)

    sr, sc = start
    fr, fc = finish

    dist = [[-1] * m for _ in range(n)]
    dist[sr][sc] = 0

    q = deque([(sr, sc)])

    has_horizontal = False
    has_vertical = False

    for_dr_dc = ((1, 0), (-1, 0), (0, 1), (0, -1))

    while q:
        r, c = q.popleft()

        for dr, dc in for_dr_dc:
            nr = r + dr
            nc = c + dc

            if nr < 0 or nr >= n or nc < 0 or nc >= m:
                continue
            if grid[nr][nc] == '#':
                continue

            if dr == 0:
                has_horizontal = True
            else:
                has_vertical = True

            if dist[nr][nc] != -1:
                continue

            dist[nr][nc] = dist[r][c] + 1
            q.append((nr, nc))

    d = dist[fr][fc]

    if d == -1:
        print(-1)
    elif has_horizontal and has_vertical:
        print(d & 1)
    else:
        print(d)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run(
    """4 3
..f
..#
s##
...
"""
) == "0", "sample 1"

assert run(
    """1 2
sf
"""
) == "1", "minimum horizontal case"

assert run(
    """1 3
s.f
"""
) == "2", "horizontal-only component"

assert run(
    """3 1
s
.
f
"""
) == "2", "vertical-only component"

assert run(
    """1 3
s#f
"""
) == "-1", "unreachable exit"

assert run(
    """2 2
sf
..
"""
) == "1", "both edge types with odd distance"

n = 1000
m = 1000
rows = [['.'] * m for _ in range(n)]
rows[0][0] = 's'
rows[n - 1][m - 1] = 'f'
large_input = f"{n} {m}\n" + "\n".join("".join(row) for row in rows) + "\n"

assert run(large_input) == "0", "maximum-size open grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 x 3`lưới mẫu |`0`| Đã cung cấp mẫu và thành phần chứa cả hai loại cạnh | 
|`1 x 2`,`sf`|`1`| Lưới tối thiểu có thể và một cạnh ngang duy nhất | 
|`1 x 3`,`s.f`|`2`| Thành phần chỉ theo chiều ngang trong đó chỉ riêng tính chẵn lẻ là không đủ | 
|`3 x 1`,`s`,`.`,`f`|`2`| Thành phần chỉ theo chiều dọc | 
|`1 x 3`,`s#f`|`-1`| Kết nối và lối thoát không thể truy cập | 
|`2 x 2`,`sf`,`..`|`1`| Cả hai loại cạnh đều tồn tại, nhưng mọi`s`ĐẾN`f`đi bộ có tính chẵn lẻ lẻ | 
|`1000 x 1000`lưới mở |`0`| Kích thước tối đa, khả năng mở rộng BFS và thậm chí cả tính chẵn lẻ của điểm cuối | 

## Vỏ cạnh 

Đối với trường hợp chỉ nằm ngang```
1 3
s.f
```BFS đến lối ra ở khoảng cách xa`2`. Trong quá trình truyền tải nó thiết lập`has_horizontal`ĐẾN`True`, Nhưng`has_vertical`còn lại`False`. Do đó, thuật toán lấy nhánh một hướng và đưa ra kết quả`2`. Đi qua nhiều lần một cạnh ngang có sẵn chỉ có thể thêm một cạnh ngang khác`-2`trước sự thay đổi nhiệt độ, do đó không có đường vòng nào có thể cải thiện được trên con đường ngắn nhất. 

Đối với trường hợp chỉ theo chiều dọc```
3 1
s
.
f
```BFS lại đạt được khoảng cách`2`, nhưng chỉ lần này thôi`has_vertical`trở thành sự thật. Mỗi lần đi bộ có độ thay đổi nhiệt độ bằng chiều dài của nó nên độ thay đổi tuyệt đối của nó ít nhất là`2`. Đầu ra của thuật toán`2`. 

Đối với một thành phần chứa cả hai loại cạnh,```
2 2
sf
..
```di chuyển ngang đầu tiên đạt đến`f`với khoảng cách`1`. BFS cũng gặp các cạnh dọc ở nơi khác trong thành phần, do đó cả hai cờ đều trở thành đúng. Đầu ra của thuật toán`1 & 1 = 1`. Một chuyến đi khứ hồi theo chiều dọc có thể thêm`+2`và một chuyến đi khứ hồi theo chiều ngang có thể thêm`-2`, nhưng cả hai thao tác đều không thay đổi tính chẵn lẻ lẻ, vì vậy không thể có số 0. 

Đối với trường hợp bị ngắt kết nối,```
1 3
s#f
```BFS chỉ truy cập ô bắt đầu vì ô ở giữa là một bức tường.`dist[f]`còn lại`-1`và thuật toán ngay lập tức đưa ra`-1`. Không tính toán nhiệt độ cho đường dẫn không tồn tại. 

Đối với trường hợp kích thước tối đa, lưới mở hoàn toàn (1000 \times 1000) có cả cạnh ngang và dọc. Nếu như`s`đang ở`(0,0)`Và`f`đang ở`(999,999)`, khoảng cách ngắn nhất là`1998`, tức là chẵn. Do đó, thuật toán trả về`0`. Mặc dù bản thân đường đi ngắn nhất đã dài, BFS chỉ xử lý từng ô trong số tối đa một triệu ô một lần, duy trì giới hạn (O(nm)).
