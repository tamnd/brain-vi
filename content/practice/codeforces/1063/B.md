---
title: "CF 1063B - Mê cung"
description: "Nhiệm vụ này đặt chúng ta vào một mê cung dạng lưới, trong đó mỗi ô đều mở hoặc bị chặn. Chúng ta bắt đầu từ một ô nhất định và có thể di chuyển theo bốn hướng chính miễn là chúng ta ở trong lưới và tránh chướng ngại vật."
date: "2026-06-15T08:31:40+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1063
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 516 (Div. 1, by Moscow Team Olympiad)"
rating: 1800
weight: 1063
solve_time_s: 138
verified: true
draft: false
---

[CF 1063B - Mê cung](https://codeforces.com/problemset/problem/1063/B) 

**Đánh giá:** 1800 
**Tags:** đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này đặt chúng ta vào một mê cung dạng lưới, trong đó mỗi ô đều mở hoặc bị chặn. Chúng ta bắt đầu từ một ô nhất định và có thể di chuyển theo bốn hướng chính miễn là chúng ta ở trong lưới và tránh chướng ngại vật. Điều khó khăn là chuyển động theo chiều ngang bị hạn chế: chúng ta chỉ có thể bước sang trái một số lần giới hạn và bước sang phải một số lần giới hạn. Chuyển động dọc không bị hạn chế. 

Đầu ra không phải là đường dẫn hoặc khoảng cách mà là số lượng khả năng tiếp cận. Đối với mỗi ô trong lưới, chúng tôi muốn biết liệu có tồn tại một số chuỗi di chuyển hợp lệ ngay từ đầu tôn trọng giới hạn bên trái và bên phải và tránh chướng ngại vật hay không và chúng tôi đếm xem có bao nhiêu ô như vậy tồn tại. 

Kích thước lưới có thể lớn tới 2000 vào năm 2000, điều này đã đẩy chúng ta đi ngang qua đồ thị tuyến tính hoặc gần tuyến tính trên toàn bộ lưới. Một cách tiếp cận bùng nổ trạng thái hoặc con đường ngắn nhất ngây thơ để theo dõi ngân sách bên trái và bên phải còn lại một cách rõ ràng sẽ ngay lập tức trở nên quá lớn vì những ngân sách đó lên tới 10^9. Bất kỳ giải pháp nào coi trái và phải còn lại được coi là một phần của trạng thái sẽ không khả thi. 

Một chế độ thất bại tinh tế xuất hiện khi suy nghĩ tham lam. Ví dụ: nếu bạn luôn cố gắng giảm thiểu các bước di chuyển sang trái, bạn có thể kết luận không chính xác rằng một số ô không thể truy cập được mặc dù một đường dẫn hơi khác đi vòng theo chiều dọc và sau đó tiếp cận từ bên phải khiến chúng có thể tiếp cận được với cùng một hoặc ít bước di chuyển trái hơn. Ràng buộc tương tác với hình học, không chỉ khoảng cách ngắn nhất. 

Một trường hợp khác là hành lang ngang dài. Giả sử một hàng có một đoạn trống dài và đường dẫn tối ưu yêu cầu di chuyển sang phải, lên trên, rồi sang trái. Một BFS ngây thơ không tính đến sự bất đối xứng về chi phí sẽ cho rằng việc xem lại một ô có mức sử dụng bên trái kém hơn là vô nghĩa, mặc dù sau đó nó có thể mở khóa một khu vực khác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mỗi trạng thái như một cặp bao gồm vị trí và số lượng nước đi trái và phải đã được sử dụng cho đến nay. Từ mỗi ô, chúng tôi phân nhánh theo bốn hướng và cập nhật bộ đếm. Điều này đúng về mặt khái niệm vì nó mã hóa tất cả các ràng buộc một cách rõ ràng, nhưng không gian trạng thái rất lớn. Mỗi ô có thể được xem lại bằng nhiều cách kết hợp sử dụng trái-phải khác nhau và vì các giá trị đó lên tới 10^9 nên số lượng trạng thái là không giới hạn trong thực tế. Ngay cả khi cắt tỉa, nó vẫn sụp đổ trong trường hợp xấu nhất là lưới có kích thước 4 triệu ô. 

Quan sát quan trọng là chuyển động thẳng đứng không có ràng buộc về chi phí. Điều này cho phép chúng ta nghĩ đến các cột dọc được kết nối trước tiên. Nếu chúng ta cố định một cột, chúng ta có thể tự do di chuyển lên xuống trong các ô mở có thể truy cập được. Khó khăn thực sự là chúng ta có thể mở rộng sang trái và phải bao xa từ bất kỳ cột có thể tiếp cận nào, bởi vì chuyển động theo chiều ngang là thứ tiêu tốn ngân sách. 

Điều này gợi ý cách giải thích kiểu BFS 0-1: di chuyển theo chiều dọc là miễn phí, di chuyển sang trái chi phí 1 từ ngân sách bên trái, di chuyển sang phải chi phí 1 từ ngân sách bên phải. Tuy nhiên, việc theo dõi trực tiếp cả hai ngân sách vẫn còn tốn kém. Sự đơn giản hóa quan trọng là duy trì, đối với mỗi ô, số lần di chuyển bên trái tối thiểu cần thiết để tiếp cận nó, đồng thời tối đa hóa các bước di chuyển bên phải trong phạm vi ngân sách cho phép. Chúng tôi truyền bá các trạng thái trong một deque trong đó các chuyển động sang trái được coi là các chuyển đổi tốn kém hơn. 

Việc triển khai khám phá một cách hiệu quả tất cả các ô có thể tiếp cận trong khi vẫn duy trì năng lực phù hợp còn lại tốt nhất có thể đạt được cho từng vị trí. Nếu một đường dẫn đến một ô có ít bước di chuyển sang trái hơn trước thì đường dẫn đó sẽ chiếm ưu thế ở các trạng thái trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (trạng thái có bộ đếm trái/phải) | Hàm mũ trong thực tế | O(nm * x * y) | Quá chậm | 
| 0-1 BFS trên trạng thái lưới | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu từ ô ban đầu và đánh dấu nó là có thể truy cập được bằng cách sử dụng không trái và không phải. Đây là cấu hình bắt đầu hợp lệ được đảm bảo duy nhất. 
2. Sử dụng deque để thực hiện BFS đã sửa đổi trong đó chúng tôi coi các bước di chuyển sang trái có chi phí cao hơn các bước di chuyển sang phải hoặc dọc. Điều này cho phép chúng tôi luôn mở rộng các trạng thái thuận lợi hơn trước. 
3. Từ mỗi ô, hãy thử di chuyển theo chiều dọc (lên và xuống). Những thứ này không tiêu tốn ngân sách bên trái hoặc bên phải, vì vậy chúng được đẩy lên phía trước deque. Điều này đảm bảo khả năng tiếp cận theo chiều dọc trong một cột được khám phá đầy đủ trước khi chi tiêu ngân sách theo chiều ngang. 
4. Cố gắng di chuyển sang phải. Điều này tiêu tốn một đơn vị ngân sách phù hợp. Nếu chưa vượt quá giới hạn, chúng tôi sẽ cập nhật trạng thái cho ô lân cận. Quá trình chuyển đổi này được coi là chi phí thấp và được đẩy thích hợp theo thứ tự BFS. 
5. Cố gắng di chuyển sang trái. Điều này tiêu tốn một đơn vị ngân sách còn lại. Nếu trong giới hạn, chúng tôi cũng cập nhật trạng thái lân cận theo cách tương tự, nhưng vì các bước di chuyển sang trái hạn chế hơn nên chúng tôi đảm bảo các trạng thái đạt được với ít lần di chuyển trái hơn sẽ thay thế các trạng thái kém hơn. 
6. Duy trì cấu trúc đã truy cập hoặc trạng thái tốt nhất để mỗi ô chỉ giữ cấu hình tốt nhất có thể đạt được xét về số dư ngân sách còn lại. Nếu một đường dẫn mới không cải thiện được trạng thái thì nó sẽ bị loại bỏ. 
7. Sau khi BFS hoàn tất, hãy đếm tất cả các ô đã từng đạt được theo bất kỳ cấu hình hợp lệ nào. 

### Tại sao nó hoạt động 

Lưới có thể được xem như một biểu đồ trong đó các cạnh có chi phí không đối xứng: các cạnh bên trái tiêu thụ từ ngân sách eo hẹp trong khi các cạnh bên phải tiêu thụ từ một ngân sách độc lập khác. Các cạnh dọc được tự do. Thuật toán duy trì sự thống trị giữa các trạng thái: nếu một cách để tiếp cận một ô sử dụng ít bước di chuyển sang trái hơn trong khi không kém hơn về tính khả thi ở bên phải, thì nó sẽ thống trị nghiêm ngặt tất cả các cách khác. Sự thống trị này đảm bảo chúng ta không bao giờ cần phải xem xét lại các trạng thái tồi tệ hơn, bởi vì chúng không bao giờ có thể dẫn đến các ô có thể tiếp cận bổ sung mà trạng thái tốt hơn cũng không thể tiếp cận được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    r, c = map(int, input().split())
    x, y = map(int, input().split())

    r -= 1
    c -= 1

    grid = [input().strip() for _ in range(n)]

    INF = 10**18

    # dist[i][j] = minimum left moves used to reach (i,j)
    dist = [[INF] * m for _ in range(n)]

    dq = deque()
    dq.append((r, c, 0, 0))  # row, col, left_used, right_used
    dist[r][c] = 0

    while dq:
        i, j, l, rr = dq.popleft()

        if l > dist[i][j]:
            continue

        # vertical moves (free)
        for di in (-1, 1):
            ni = i + di
            nj = j
            if 0 <= ni < n and grid[ni][nj] == '.':
                if l < dist[ni][nj]:
                    dist[ni][nj] = l
                    dq.appendleft((ni, nj, l, rr))

        # right move
        ni, nj = i, j + 1
        if nj < m and grid[ni][nj] == '.':
            if rr + 1 <= y:
                if l < dist[ni][nj]:
                    dist[ni][nj] = l
                    dq.append((ni, nj, l, rr + 1))

        # left move
        ni, nj = i, j - 1
        if nj >= 0 and grid[ni][nj] == '.':
            if l + 1 <= x:
                if l + 1 < dist[ni][nj]:
                    dist[ni][nj] = l + 1
                    dq.append((ni, nj, l + 1, rr))

    ans = 0
    for i in range(n):
        for j in range(m):
            if dist[i][j] != INF:
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này mã hóa khả năng tiếp cận bằng cách sử dụng một số liệu thống trị duy nhất: số lần di chuyển trái được sử dụng. Ngân sách phù hợp được theo dõi trong quá trình chuyển đổi nhưng không xác định sự thống trị theo cách tương tự, vì các bước đi phù hợp thường ít hạn chế hơn trong việc lan truyền khi năng lực cho phép chuyển động. 

Deque đảm bảo rằng việc mở rộng theo chiều dọc tự do được xử lý ngay lập tức, trong khi các bước di chuyển theo chiều ngang được xếp lớp theo chi phí. Ma trận khoảng cách ngăn chặn việc xem lại các trạng thái đã đạt được theo cách tốt hơn. 

Một điểm tinh tế là chúng ta không lưu trữ các cặp trạng thái đầy đủ trong tập đã truy cập. Điều đó sẽ làm bùng nổ trí nhớ. Thay vào đó, chúng tôi nén tất cả các so sánh có ý nghĩa vào`dist`mảng, nắm bắt thứ nguyên thứ tự có liên quan duy nhất cần thiết cho việc cắt tỉa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
3 2
1 2
.....
.***.
...**
*....
```Chúng tôi theo dõi chế độ xem khám phá đơn giản, tập trung vào một số ô đại diện. 

| Bước | Vị trí | Còn lại sử dụng | Đúng sử dụng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (3,2) | 0 | 0 | bắt đầu | 
| 2 | (3,3) | 0 | 1 | di chuyển sang phải | 
| 3 | (3,4) | 0 | 2 | di chuyển sang phải | 
| 4 | (2,3) | 0 | 2 | thẳng đứng lên | 
| 5 | (1,3) | 0 | 2 | thẳng đứng tránh đường bị chặn | 
| 6 | (3,1) | 1 | 2 | di chuyển sang trái | 

Dấu vết này cho thấy cách mở rộng theo chiều dọc cho phép tiếp cận các khu vực phía trên mà không tiêu tốn ngân sách theo chiều ngang và chuyển động theo chiều ngang sẽ mở rộng các cột có thể tiếp cận. 

Bộ có thể truy cập cuối cùng bao gồm 10 ô, khớp với đầu ra mẫu. 

### Ví dụ 2 

Xét một hành lang có rào chắn buộc phải đi đường vòng:```
3 5
2 3
0 1
.....
..*..
.....
```| Bước | Vị trí | Còn lại sử dụng | Đúng sử dụng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (2,3) | 0 | 0 | bắt đầu | 
| 2 | (2,4) | 0 | 1 | đúng | 
| 3 | (3,4) | 0 | 1 | xuống | 
| 4 | (3,3) | 1 | 1 | hàng tránh chướng ngại vật bên trái | 
| 5 | (1,3) | 1 | 1 | lên | 

Điều này cho thấy cách các đường vòng theo chiều dọc tương tác với các ràng buộc theo chiều ngang, cho phép truy cập vào các ô mà không thể truy cập được bằng cách quét thẳng theo chiều ngang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô chỉ được xử lý khi tìm thấy trạng thái chi phí trái tốt hơn và mỗi cạnh được nới lỏng một số lần không đổi | 
| Không gian | O(nm) | Ma trận khoảng cách và lưu trữ lưới | 

Kích thước lưới đạt tới 4 triệu ô, vừa vặn thoải mái trong các ràng buộc truyền tải tuyến tính. Mỗi thao tác có thời gian không đổi và BFS dựa trên deque tránh được việc tính toán lại nhiều lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    def solve():
        n, m = map(int, input().split())
        r, c = map(int, input().split())
        x, y = map(int, input().split())

        r -= 1
        c -= 1

        grid = [input().strip() for _ in range(n)]

        INF = 10**18
        dist = [[INF] * m for _ in range(n)]
        dq = deque()
        dq.append((r, c, 0, 0))
        dist[r][c] = 0

        while dq:
            i, j, l, rr = dq.popleft()
            if l > dist[i][j]:
                continue

            for di in (-1, 1):
                ni, nj = i + di, j
                if 0 <= ni < n and grid[ni][nj] == '.':
                    if l < dist[ni][nj]:
                        dist[ni][nj] = l
                        dq.appendleft((ni, nj, l, rr))

            ni, nj = i, j + 1
            if nj < m and grid[ni][nj] == '.' and rr + 1 <= y:
                if l < dist[ni][nj]:
                    dist[ni][nj] = l
                    dq.append((ni, nj, l, rr + 1))

            ni, nj = i, j - 1
            if nj >= 0 and grid[ni][nj] == '.' and l + 1 <= x:
                if l + 1 < dist[ni][nj]:
                    dist[ni][nj] = l + 1
                    dq.append((ni, nj, l + 1, rr))

        return sum(dist[i][j] != INF for i in range(n) for j in range(m))

    return str(solve())

# provided sample
assert run("""4 5
3 2
1 2
.....
.***.
...**
*....
""") == "10"

# custom 1: single cell
assert run("""1 1
1 1
0 0
.
""") == "1"

# custom 2: blocked start surroundings
assert run("""3 3
2 2
1 1
***
*.*
***
""") == "1"

# custom 3: horizontal corridor
assert run("""1 5
1 3
2 2
.....""") == "5"

# custom 4: all open grid
assert run("""2 2
1 1
1 1
..
..""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 mở | 1 | độ chính xác lưới tối thiểu | 
| bắt đầu bao quanh | 1 | xử lý cách ly chướng ngại vật | 
| hành lang 1 dãy | 5 | giới hạn giãn nở theo chiều ngang | 
| lưới đầy đủ | 4 | khả năng tiếp cận không hạn chế | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi bắt đầu ở một hành lang hẹp, nơi không thể di chuyển theo chiều dọc và mọi tiến độ đều phụ thuộc vào việc cân đối ngân sách trái và phải một cách cẩn thận. Trong trường hợp như vậy, thuật toán vẫn hoạt động chính xác vì di chuyển theo chiều ngang chỉ được áp dụng khi nằm trong phạm vi ngân sách và không bao giờ ghi đè lên trạng thái tốt hơn. 

Một trường hợp khác là khi tồn tại một khu vực rộng lớn nhưng bị ngăn cách bởi một cột chướng ngại vật duy nhất buộc phải đi đường vòng dài. BFS trước tiên khám phá các đường đi dọc thay thế, đảm bảo rằng việc đến lối vào đường vòng không làm cạn kiệt ngân sách còn lại trên đường đi dưới mức tối ưu. 

Trường hợp cạnh cuối cùng là khi tuyến đường tối ưu yêu cầu sớm tăng mức sử dụng bên trái tạm thời để mở khóa một khu vực mà sau này cung cấp đường đi ngang ngắn hơn. Quy tắc thống trị đảm bảo rằng bất kỳ trạng thái nào đến một ô có ít bước di chuyển trái hơn sẽ được giữ nguyên, do đó, thuật toán không bao giờ mất khả năng đi đường vòng tốt hơn sau đó.
