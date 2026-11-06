---
title: "CF 128A - Tượng"
description: "Chúng tôi có một bảng 8 × 8. Maria bắt đầu ở góc dưới bên trái, Anna ở vĩnh viễn ở góc trên bên phải và một số bức tượng chiếm các ô khác. Trò chơi tiến hành theo từng vòng. Maria di chuyển trước, sau đó mỗi bức tượng đồng thời di chuyển một hàng xuống dưới."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 128
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 94 (Div. 1 Only)"
rating: 1500
weight: 128
solve_time_s: 130
verified: true
draft: false
---

[CF 128A - Tượng](https://codeforces.com/problemset/problem/128/A) 

**Đánh giá:** 1500 
**Thẻ:** dfs và tương tự 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bảng 8 × 8. Maria bắt đầu ở góc dưới bên trái, Anna ở vĩnh viễn ở góc trên bên phải và một số bức tượng chiếm các ô khác. 

Trò chơi tiến hành theo từng vòng. Maria di chuyển trước, sau đó mỗi bức tượng đồng thời di chuyển một hàng xuống dưới. Một bức tượng di chuyển bên dưới bảng sẽ biến mất vĩnh viễn. 

Maria có thể di chuyển đến bất kỳ ô nào trong số 8 ô lân cận hoặc giữ nguyên vị trí. Cô ấy không thể di chuyển vào phòng giam hiện đang có một bức tượng. Sau khi cô ấy di chuyển, các bức tượng sẽ dịch chuyển xuống dưới. Nếu bất kỳ bức tượng nào rơi vào phòng giam của Maria, cô ấy sẽ thua ngay lập tức. Nếu Maria đến được phòng giam của Anna, cô ấy sẽ thắng ngay lập tức. 

Bảng cực kỳ nhỏ. Chỉ có 64 ô và các bức tượng di chuyển một cách hoàn toàn xác định. Sau tối đa 8 lần di chuyển các bức tượng, mọi bức tượng đều rơi khỏi bàn cờ, vì vậy giai đoạn nguy hiểm của trò chơi chỉ tồn tại trong thời gian ngắn. Điều đó có nghĩa là chúng tôi có đủ khả năng để tìm kiếm trực tiếp tất cả các trạng thái trò chơi có thể truy cập. 

Khó khăn chính là bảng thay đổi theo thời gian. Vị trí hiện tại an toàn có thể trở nên không an toàn sau khi các bức tượng di chuyển. Việc tìm kiếm bất cẩn chỉ kiểm tra cấu hình bảng hiện tại sẽ chấp nhận các trạng thái không hợp lệ. 

Một trường hợp khó nhận thấy là khi Maria di chuyển vào một ô hiện trống nhưng sẽ ngay lập tức bị chiếm giữ sau khi các bức tượng di chuyển. 

Ví dụ:```
.......A
........
........
........
........
........
.S......
M.......
```Đầu ra đúng:```
LOSE
```Maria không thể di chuyển lên trên một cách an toàn vì bức tượng từ hàng 6 sẽ hạ xuống vị trí của cô sau khi các bức tượng di chuyển. Một BFS ngây thơ chỉ kiểm tra bảng trước khi di chuyển bức tượng sẽ nghĩ sai rằng hành động này là hợp pháp. 

Một trường hợp khó khăn khác xuất hiện khi Maria sống sót đủ lâu để tất cả các bức tượng đều biến mất.```
.......A
SSSSSSSS
........
........
........
........
........
M.......
```Đầu ra đúng:```
WIN
```Maria có thể đơn giản tránh nguy hiểm cho đến khi các bức tượng rơi khỏi bảng. Sau đó, bảng sẽ trống vĩnh viễn và việc đến được với Anna được đảm bảo. Việc triển khai giới hạn độ sâu tìm kiếm quá mạnh một cách giả tạo có thể từ chối trường hợp này một cách không chính xác. 

Sai lầm phổ biến thứ ba là quên rằng Maria có thể ở yên tại chỗ.```
.......A
........
........
........
........
........
S.......
M.......
```Đầu ra đúng:```
WIN
```Di chuyển ngay lập tức rất nguy hiểm, nhưng đợi một lượt sẽ khiến bức tượng di chuyển xuống dưới và biến mất. Các giải pháp chỉ xem xét 8 nước đi có hướng sẽ bỏ lỡ các chiến lược chiến thắng hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng lực lượng vũ phu. Ở mỗi lượt Maria có tối đa 9 lựa chọn, gồm 8 ô lân cận và giữ nguyên. Chúng ta có thể thử đệ quy mọi chuỗi chuyển động có thể có trong khi mô phỏng chuyển động của bức tượng mỗi lượt. 

Điều này hoạt động về mặt khái niệm vì bàn cờ rất nhỏ và diễn biến của trò chơi mang tính quyết định. Vấn đề là số lượng chuỗi di chuyển tăng theo cấp số nhân. Sau 20 lượt thì đã có khoảng$9^{20}$những con đường có thể, hoàn toàn không khả thi. 

Quan sát quan trọng là trạng thái bảng chỉ phụ thuộc vào thời gian. Các bức tượng luôn di chuyển xuống dưới theo cùng một cách bất kể hành động của Maria. Vì bàn cờ chỉ có 8 hàng nên sau tối đa 8 lượt tất cả các bức tượng đều biến mất. 

Điều đó thay đổi hoàn toàn vấn đề. Thay vì khám phá các chuỗi di chuyển có độ dài tùy ý, chúng ta chỉ cần theo dõi: 

1. Vị trí hiện tại của Maria. 
2. Bước thời gian hiện tại. 

Tại mỗi bước thời gian có nhiều nhất 64 vị trí có thể. Tổng số trạng thái có ý nghĩa là rất nhỏ, gần như$64 \times 9$. 

Điều này đương nhiên dẫn đến BFS hoặc DFS qua các trạng thái$(row, col, time)$. Từ một trạng thái, chúng tôi thử tất cả 9 nước đi, kiểm tra xem đích đến có an toàn trước và sau khi di chuyển tượng hay không, sau đó tiếp tục. 

Thông tin chi tiết quan trọng là kiểm tra an toàn kép: 

1. Maria không thể di chuyển vào phòng chứa tượng vào thời điểm hiện tại. 
2. Maria cũng không thể ở trong phòng giam chứa bức tượng sau khi các bức tượng hạ xuống. 

Nếu không có điều kiện thứ hai, cuộc tìm kiếm sẽ chấp nhận những vị trí mà Maria sẽ bị nghiền nát ngay sau khi di chuyển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(9^T) | O(T) | Quá chậm | 
| BFS/DFS tối ưu trên các trạng thái | O(64 × 9 × 9) | O(64 × 9) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bảng 8 × 8 ban đầu. 
2. Lưu trữ tất cả các vị trí tượng từ cấu hình ban đầu. 
3. Xác định hàm`safe(r, c, t)`để kiểm tra xem ô`(r, c)`đôi khi không có tượng`t`. 

Một bức tượng ban đầu ở`(sr, sc)`sẽ có mặt tại`(sr + t, sc)`sau đó`t`di chuyển. Nếu như`sr + t >= 8`, bức tượng đó đã biến mất. 
4. Bắt đầu BFS từ vị trí ban đầu của Maria`(7, 0)`vào thời điểm đó`0`. 
5. Đối với mỗi tiểu bang`(r, c, t)`, trước tiên hãy kiểm tra xem ô hiện tại có an toàn không`t`. 

Nếu bây giờ một bức tượng chiếm vị trí hiện tại thì trạng thái này không hợp lệ và cần được bỏ qua. 
6. Nếu Maria đạt tới`(0, 7)`, trở lại`"WIN"`. 
7. Hãy thử tất cả 9 hành động có thể: 

1. Di chuyển theo một trong 8 hướng. 
2. Giữ nguyên vị trí. 
8. Đối với từng vị trí ứng viên`(nr, nc)`: 

1. Bỏ qua các vị trí bên ngoài bảng. 
2. Bỏ qua các ô bị tượng chiếm giữ vào thời điểm đó`t`. 
3. Bỏ qua các ô sẽ bị chiếm dụng vào thời điểm đó`t + 1`. 

Điều kiện thứ hai ngăn Maria bước lên bức tượng ngay lập tức. Cái thứ ba giúp cô ấy không bị nghiền nát sau khi các bức tượng di chuyển. 
9. Đẩy trạng thái tiếp theo hợp lệ`(nr, nc, min(t + 1, 8))`vào hàng đợi. 

Chúng tôi giới hạn thời gian ở mức 8 vì sau 8 lần di chuyển, tất cả các bức tượng đều biến mất và bàn cờ không bao giờ thay đổi nữa. 
10. Nếu BFS kết thúc mà không liên lạc được với Anna, hãy quay lại`"LOSE"`. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cấu hình bo mạch trong tương lai hoàn toàn được xác định theo thời gian. BFS khám phá mọi vị trí an toàn có thể tiếp cận mà Maria có thể chiếm giữ ở mỗi bước thời gian. 

Quá trình chuyển đổi chỉ được thêm vào nếu ô đích an toàn cả trước và sau khi các bức tượng di chuyển. Điều đó hoàn toàn phù hợp với luật chơi. 

Vì mọi chuỗi nước đi hợp lệ có thể có đều được biểu thị trong biểu đồ trạng thái, BFS sẽ tìm ra đường đi thắng bất cứ khi nào có đường đi đó. Nếu BFS không thể tiếp cận Anna thì không có chiến lược pháp lý nào tồn tại. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

DIRS = [
    (-1, -1), (-1, 0), (-1, 1),
    (0, -1),  (0, 0),  (0, 1),
    (1, -1),  (1, 0),  (1, 1)
]

def solve():
    board = [input().strip() for _ in range(8)]

    statues = []

    for r in range(8):
        for c in range(8):
            if board[r][c] == 'S':
                statues.append((r, c))

    def safe(r, c, t):
        for sr, sc in statues:
            nr = sr + t
            if nr < 8 and nr == r and sc == c:
                return False
        return True

    q = deque()
    q.append((7, 0, 0))

    visited = set()
    visited.add((7, 0, 0))

    while q:
        r, c, t = q.popleft()

        if not safe(r, c, t):
            continue

        if (r, c) == (0, 7):
            print("WIN")
            return

        nt = min(t + 1, 8)

        for dr, dc in DIRS:
            nr = r + dr
            nc = c + dc

            if not (0 <= nr < 8 and 0 <= nc < 8):
                continue

            if not safe(nr, nc, t):
                continue

            if not safe(nr, nc, t + 1):
                continue

            state = (nr, nc, nt)

            if state in visited:
                continue

            visited.add(state)
            q.append(state)

    print("LOSE")

solve()
```Giải pháp bắt đầu bằng việc thu thập các vị trí tượng ban đầu. Vì chuyển động của bức tượng mang tính quyết định nên chúng ta không bao giờ cần phải xây dựng lại bảng một cách rõ ràng cho mỗi bước thời gian. 

các`safe`tính toán xem một ô có trống tại một thời điểm nhất định hay không. Thay vì mô phỏng chuyển động lặp đi lặp lại, nó sẽ xác định vị trí của từng bức tượng trực tiếp từ hàng ban đầu của nó. 

Trạng thái BFS bao gồm thời gian vì cùng một ô bảng có thể an toàn tại một thời điểm và gây chết người sau đó. điều trị`(r, c)`một mình khi truy cập sẽ kết hợp không chính xác các tình huống trò chơi khác nhau. 

Thứ tự kiểm tra an toàn rất quan trọng. Trước tiên, chúng tôi xác minh điểm đến là miễn phí trước khi di chuyển bức tượng, sau đó xác minh rằng nó vẫn miễn phí sau khi các bức tượng hạ xuống. Thiếu bước kiểm tra thứ hai là lỗi phổ biến nhất trong vấn đề này. 

Thời gian được giới hạn ở mức 8 vì tất cả các bức tượng đều biến mất sau đó. Nếu không có giới hạn này, BFS có thể tiếp tục tạo ra các trạng thái tương đương trong tương lai mãi mãi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
.......A
........
........
........
........
........
........
M.......
```Không có bức tượng nào cả. 

| Bước | Vị trí | Thời gian | An toàn ngay bây giờ | An Toàn Sau Khi Di Chuyển | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | (7,0) | 0 | Có | Có | Bắt đầu | 
| 1 | (6,1) | 1 | Có | Có | Di chuyển theo đường chéo | 
| 2 | (5,2) | 2 | Có | Có | Tiếp tục | 
| 3 | (4,3) | 3 | Có | Có | Tiếp tục | 
| 4 | (3,4) | 4 | Có | Có | Tiếp tục | 
| 5 | (2,5) | 5 | Có | Có | Tiếp tục | 
| 6 | (1,6) | 6 | Có | Có | Tiếp tục | 
| 7 | (0,7) | 7 | Có | Có | Tiếp cận Anna | 

Vì không có bức tượng nào tồn tại nên mọi di chuyển đều an toàn. BFS nhanh chóng tiếp cận mục tiêu. 

### Ví dụ 2 

đầu vào:```
.......A
........
........
........
........
........
.S......
M.......
```| Bước | Vị trí | Thời gian | Vị Trí Tượng | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 0 | (7,0) | 0 | (6,1) | Bắt đầu | 
| 1 | (6,1) | 1 | (7,1) | Không hợp lệ | 
| 1 | (7,0) | 1 | (7,1) | hợp lệ | 
| 2 | (6,0) | 2 | Tượng biến mất | hợp lệ | 

Maria không thể ngay lập tức di chuyển theo đường chéo lên trên vì bức tượng sẽ rơi xuống phòng giam của cô ấy sau khi di chuyển. Thuật toán từ chối quá trình chuyển đổi đó bằng cách sử dụng kiểm tra an toàn thứ hai. 

Chờ một lượt sẽ tránh được nguy hiểm hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(64 × 9 × 9) | Tối đa 64 ô, 9 trạng thái thời gian và 9 lần chuyển đổi trên mỗi trạng thái | 
| Không gian | O(64 × 9) | Hàng đợi BFS và các trạng thái đã truy cập | 

Không gian tìm kiếm cực kỳ nhỏ. Ngay cả một BFS đơn giản cũng dễ dàng nằm gọn trong giới hạn 2 giây và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

DIRS = [
    (-1, -1), (-1, 0), (-1, 1),
    (0, -1),  (0, 0),  (0, 1),
    (1, -1),  (1, 0),  (1, 1)
]

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    board = [input().strip() for _ in range(8)]

    statues = []

    for r in range(8):
        for c in range(8):
            if board[r][c] == 'S':
                statues.append((r, c))

    def safe(r, c, t):
        for sr, sc in statues:
            nr = sr + t
            if nr < 8 and nr == r and sc == c:
                return False
        return True

    q = deque([(7, 0, 0)])
    visited = {(7, 0, 0)}

    while q:
        r, c, t = q.popleft()

        if not safe(r, c, t):
            continue

        if (r, c) == (0, 7):
            return "WIN"

        nt = min(t + 1, 8)

        for dr, dc in DIRS:
            nr = r + dr
            nc = c + dc

            if not (0 <= nr < 8 and 0 <= nc < 8):
                continue

            if not safe(nr, nc, t):
                continue

            if not safe(nr, nc, t + 1):
                continue

            state = (nr, nc, nt)

            if state in visited:
                continue

            visited.add(state)
            q.append(state)

    return "LOSE"

# provided sample
assert run(
""".......A
........
........
........
........
........
........
M.......
"""
) == "WIN", "sample 1"

# statue directly above path
assert run(
""".......A
........
........
........
........
........
.S......
M.......
"""
) == "WIN", "must wait before moving"

# full statue wall that eventually disappears
assert run(
""".......A
SSSSSSSS
........
........
........
........
........
M.......
"""
) == "WIN", "survive until statues vanish"

# trapped immediately
assert run(
""".......A
........
........
........
........
........
SS......
MS......
"""
) == "LOSE", "no safe moves"

# empty board diagonal race
assert run(
""".......A
........
........
........
........
........
........
M.......
"""
) == "WIN", "basic movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảng trống | THẮNG | Chuyển động cơ bản hướng tới mục tiêu | 
| Tượng đơn giáng xuống | THẮNG | Chờ đợi tại chỗ là cần thiết | 
| Hàng tượng đầy đủ | THẮNG | Sống sót cho đến khi tất cả các bức tượng biến mất | 
| Bẫy ngay lập tức | THUA | Từ chối đúng các nước đi không an toàn | 
| Bảng trống lặp đi lặp lại | THẮNG | Chuyển động ranh giới và khả năng tiếp cận BFS | 

## Vỏ cạnh 

Hãy xem xét trường hợp Maria di chuyển vào một phòng giam sẽ có người ở ngay sau khi các bức tượng hạ xuống. 

đầu vào:```
.......A
........
........
........
........
........
.S......
M.......
```Tại thời điểm 0 tượng đang ở`(6,1)`. Maria có thể thử chuyển đến`(6,1)`theo đường chéo hướng lên trên nhưng ô đó đã bị chiếm giữ. Thay vào đó cô ấy có thể chuyển đến`(6,0)`. Sau khi tượng di chuyển, tượng sẽ trở thành`(7,1)`, thế là Maria sống sót. 

Thuật toán kiểm tra cả`safe(nr, nc, t)`Và`safe(nr, nc, t + 1)`, ngăn chặn sự chuyển đổi bất hợp pháp. 

Bây giờ hãy xem xét trường hợp tất cả các bức tượng cuối cùng đều biến mất. 

đầu vào:```
.......A
SSSSSSSS
........
........
........
........
........
M.......
```Mỗi lượt, toàn bộ hàng tượng lại dịch chuyển xuống dưới. Sau 7 lần di chuyển các bức tượng sẽ rời khỏi bảng hoàn toàn. Vì thời gian bị giới hạn ở mức 8, BFS tiếp tục khám phá các trạng thái bảng trống an toàn cho đến khi Maria đến gặp Anna. 

Cuối cùng, hãy xem xét trường hợp bắt buộc phải ở yên một chỗ. 

đầu vào:```
.......A
........
........
........
........
........
S.......
M.......
```Nếu Maria di chuyển lên trên ngay lập tức, bức tượng đi xuống sẽ tạo ra những vị trí nguy hiểm. BFS bao gồm`(0,0)`chuyển động trong danh sách phương hướng của nó, cho phép Maria chờ đợi an toàn cho đến khi bức tượng biến mất. Nếu không có hành động ở lại, tìm kiếm sẽ báo cáo không chính xác`"LOSE"`.
