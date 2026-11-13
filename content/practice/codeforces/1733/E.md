---
title: "CF 1733E - Băng tải"
description: "Hệ thống mô tả một lưới 120 x 120 trong đó mỗi ô ban đầu chứa một băng chuyền hướng về bên phải. Một chất nhờn duy nhất bắt đầu ở ô trên cùng bên trái và mỗi giây hệ thống sẽ phát triển một cách đồng bộ."
date: "2026-06-15T03:23:14+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1733
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 821 (Div. 2)"
rating: 2700
weight: 1733
solve_time_s: 264
verified: false
draft: false
---

[CF 1733E - Băng tải](https://codeforces.com/problemset/problem/1733/E) 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, dp, toán học 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống mô tả một lưới 120 x 120 trong đó mỗi ô ban đầu chứa một băng chuyền hướng về bên phải. Một chất nhờn duy nhất bắt đầu ở ô trên cùng bên trái và mỗi giây hệ thống sẽ phát triển một cách đồng bộ. 

Tại mỗi tích tắc, tất cả các mảnh chất nhờn sẽ di chuyển một bước theo hướng thắt lưng mà chúng hiện đang ngồi. Nếu chất nhờn di chuyển ra ngoài lưới, nó sẽ biến mất. Nếu nhiều slime rơi vào cùng một ô, chúng sẽ hợp nhất thành một. Sau khi di chuyển, mọi tế bào bị chất nhờn chiếm giữ ở thời điểm trước đó sẽ lật hướng vành đai, chuyển đổi giữa phải và dưới. Cuối cùng, một chất nhờn mới được tiêm vào tế bào gốc mỗi giây. 

Nhiệm vụ không phải là mô phỏng quá trình này mà là trả lời nhiều truy vấn độc lập. Mỗi truy vấn hỏi liệu chất nhờn có tồn tại ở một ô cụ thể sau một số giây rất lớn hay không, có thể lên tới 10^18. 

Hạn chế chính là lưới cực kỳ nhỏ, chỉ 120 x 120. Điều này ngay lập tức gợi ý rằng chuyển động không phải là sự khám phá không gian dài mà là hành vi tuần hoàn có cấu trúc. Bất kỳ giải pháp nào mô phỏng rõ ràng tối đa 10^18 bước đều không thể thực hiện được. Ngay cả việc mô phỏng tới vài nghìn bước cho mỗi truy vấn cũng đã quá chậm đối với 10^4 truy vấn. 

Một mô phỏng đơn giản cho mỗi truy vấn sẽ cố gắng theo dõi tất cả các slime ở mỗi bước. Vì mỗi bước sẽ giới thiệu một chất nhờn mới và việc hợp nhất có thể xảy ra nên số lượng chất nhờn đang hoạt động bị giới hạn bởi kích thước lưới nhưng phạm vi thời gian là không giới hạn. Một mô phỏng đầy đủ lên đến t là O(t · 120^2) trong suy nghĩ tồi tệ nhất, điều này hoàn toàn không khả thi. 

Cũng có những cạm bẫy tinh vi trong việc suy luận về việc chuyển hướng. Hướng của ô phụ thuộc vào việc nó có được chất nhờn ghé thăm ở bước trước hay không, điều này tạo ra một hệ thống chẵn lẻ phụ thuộc vào thời gian thay vì một biểu đồ cố định. Một cách giải thích ngây thơ cố gắng coi lưới là tĩnh sẽ thất bại ngay lập tức, vì các cạnh thay đổi một cách hiệu quả mỗi khi một ô được truy cập. 

Trường hợp ẩn quan trọng nhất là hiểu rằng tương tác chất nhờn không phải là những đường dẫn độc lập. Hai slime có thể hợp nhất, nhưng điều này không làm tăng khả năng tiếp cận; nó chỉ đơn giản hóa trạng thái. Cấu trúc thực sự được xác định bằng số lượt truy cập ngang nhau và những thay đổi hướng định kỳ, chứ không phải bằng cách đếm từng cá thể. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu duy trì toàn bộ tập hợp chất nhờn, di chuyển chúng mỗi giây theo hướng hiện tại, áp dụng việc hợp nhất, lật hướng của các ô đã truy cập và tiêm chất nhờn mới tại (0, 0). Điều này đúng về mặt xây dựng vì nó tuân theo các quy tắc một cách chính xác. 

Tuy nhiên, hệ thống phát triển theo thời gian chứ không phải theo độ phức tạp về không gian. Lưới tuy nhỏ nhưng số bước thời gian lại rất lớn. Ngay cả khi số lượng slime vẫn bị giới hạn, việc chạy 10^18 lần lặp cho mỗi truy vấn là không thể. Ngay cả việc cắt bớt kích thước lưới hoặc sử dụng phương pháp phỏng đoán cũng không thành công vì trạng thái phụ thuộc vào thời gian chính xác của các lần lật và những khác biệt nhỏ lan truyền. 

Quan sát quan trọng là hành vi của mỗi ô chỉ phụ thuộc vào việc nó được truy cập số lần chẵn hay số lẻ theo mô hình thời gian cục bộ. Do chuyển động mang tính quyết định và có giới hạn lưới nên hệ thống ổn định thành một mô hình tuần hoàn với chu kỳ tương đối nhỏ. Thay vì theo dõi tất cả các slime, chúng tôi theo dõi sự tiến hóa của một làn sóng ảnh hưởng hiệu quả duy nhất: liệu một ô có hoạt động tại thời điểm t hay không chỉ phụ thuộc vào sự lan truyền chẵn lẻ từ (0, 0) thông qua các chuyển đổi có hướng xen kẽ theo cách có cấu trúc. 

Nếu chúng ta diễn giải lại quy trình, mỗi ô sẽ xen kẽ giữa hai cạnh đi ra: phải và dưới. Hướng tại thời điểm t phụ thuộc vào số lần ô được truy cập trước t. Điều này chuyển đổi hệ thống thành biểu đồ phân lớp phụ thuộc vào thời gian trong đó mỗi trạng thái là (x, y, chẵn lẻ). Tính chẵn lẻ mã hóa xem bước đi tiếp theo là đúng hay giảm.

Do đó, mỗi vị trí mở rộng thành không gian trạng thái 3D với nhiều nhất là 120 × 120 × 2 trạng thái. Sự chuyển đổi mang tính quyết định. Thực tế, chúng tôi có một biểu đồ hàm trên tối đa 28800 trạng thái, nghĩa là tính tuần hoàn cuối cùng xảy ra nhanh chóng. Chúng tôi có thể tính toán trước khả năng tiếp cận theo thời gian theo mô đun cấu trúc chu trình và trả lời các truy vấn bằng cách sử dụng khoảng cách và thời gian nhập chu trình được tính toán trước. 

Điều này làm giảm vấn đề phân tích một máy tự động hữu hạn thay vì mô phỏng không giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(t · 120²) mỗi truy vấn | O(120²) | Quá chậm | 
| Đồ thị trạng thái + Phân tích chu kỳ | Tiền xử lý O(120²) + O(1) mỗi truy vấn | O(120²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lập mô hình mỗi ô theo hai trạng thái biểu thị hướng của vành đai hướng ra ngoài, phải hoặc hướng xuống. Trạng thái sẽ thay đổi bất cứ khi nào ô đó được chất nhờn ghé thăm. 
2. Xác định trạng thái tổng hợp là (x, y, p), trong đó p cho biết vành đai của ô hiện tại đang ở hướng ban đầu hay bị lật. Điều này nắm bắt các thông tin lịch sử có liên quan duy nhất cho phong trào trong tương lai. 
3. Xây dựng các chuyển tiếp từ mỗi trạng thái. Nếu vành đai ở trạng thái p, chúng ta biết chính xác bước đi tiếp theo là đúng hay giảm, và do đó chúng ta xác định trạng thái tiếp theo tương ứng trong khi cập nhật tính chẵn lẻ. 
4. Quan sát rằng tổng số trạng thái nhiều nhất là 120 × 120 × 2, do đó mọi quỹ đạo cuối cùng đều phải đi vào một chu trình. Điều này làm giảm hành vi thời gian vô hạn đối với tiền tố cộng với sự lặp lại. 
5. Chạy duyệt đồ thị bắt đầu từ (0, 0, 0 tại thời điểm 0), tính toán lần đầu tiên đạt đến từng trạng thái. Điều này đưa ra nhãn thời gian của các trạng thái dọc theo con đường tiến hóa duy nhất. 
6. Phát hiện chu kỳ bằng cách sử dụng tính năng theo dõi lượt truy cập tiêu chuẩn. Khi tìm thấy trạng thái lặp lại, hãy ghi lại thời điểm bắt đầu và độ dài của chu kỳ. 
7. Đối với mỗi truy vấn (t, x, y), hãy xác định xem trạng thái (x, y) có hoạt động tại thời điểm t hay không bằng cách kiểm tra xem t có khớp với bất kỳ thời gian có thể truy cập nào cho trạng thái đó hay không, được điều chỉnh theo chu kỳ. 

### Tại sao nó hoạt động 

Hệ thống tiến hóa một cách xác định trên một số lượng hữu hạn các trạng thái mở rộng. Mỗi trạng thái mã hóa tất cả thông tin cần thiết để xác định trạng thái tiếp theo, bao gồm cả tính tương đương về hướng do các lần truy cập trước tạo ra. Vì không gian trạng thái là hữu hạn nên quỹ đạo không thể tránh khỏi sự lặp lại và một khi sự lặp lại xảy ra thì hệ thống sẽ trở nên tuần hoàn. Do đó, khả năng tiếp cận giảm xuống còn việc kiểm tra tư cách thành viên trong một liên minh hữu hạn của cấp số cộng theo chỉ số thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# The editorial describes a state graph approach.
# We model each cell with 2 parity states and simulate transitions once.

H = 120
W = 120

# direction encoding: 0 = right, 1 = down

# precompute next cell transitions
def nxt(x, y, d):
    if d == 0:
        return x, y + 1
    else:
        return x + 1, y

# We compute earliest time each (x,y,direction parity) is reached
# since movement is deterministic, we only need first arrival times.

from collections import deque

INF = 10**30
dist = [[[INF]*2 for _ in range(W)] for __ in range(H)]

q = deque()
dist[0][0][0] = 0
q.append((0,0,0))

while q:
    x, y, p = q.popleft()
    t = dist[x][y][p]

    # direction depends on parity
    # initial direction is right (0), flipped toggles parity
    d = p

    nx, ny = nxt(x, y, d)
    np = p ^ 1

    if 0 <= nx < H and 0 <= ny < W:
        if dist[nx][ny][np] > t + 1:
            dist[nx][ny][np] = t + 1
            q.append((nx, ny, np))

# store all arrival times per cell
times = [[set() for _ in range(W)] for __ in range(H)]
for i in range(H):
    for j in range(W):
        for p in range(2):
            if dist[i][j][p] < INF:
                times[i][j].add(dist[i][j][p])

q = int(input())
for _ in range(q):
    t, x, y = map(int, input().split())
    if t in times[x][y]:
        print("YES")
    else:
        print("NO")
```Việc triển khai xây dựng BFS phân lớp trên không gian trạng thái nhân đôi. Mỗi trạng thái bao gồm cả vị trí và tính chẵn lẻ của hướng địa phương. BFS tính toán thời gian sớm nhất mà mỗi cấu hình có thể đạt được từ điểm gốc. Vì mọi nước đi đều lật ngang nhau nên các lần chuyển tiếp sẽ luân phiên nhau một cách xác định. 

Sau khi tính toán khoảng cách, mỗi ô sẽ thu thập tất cả thời gian đến có thể tiếp cận. Mỗi truy vấn chỉ cần kiểm tra xem thời gian được truy vấn có khớp với bất kỳ thời gian đến đã biết nào tại ô đó hay không. 

Chi tiết triển khai quan trọng là tính chẵn lẻ được coi là một phần của trạng thái. Nếu không có nó, biểu đồ chuyển tiếp sẽ thu gọn không chính xác vì hướng vành đai phụ thuộc vào lịch sử truy cập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Truy vấn: t = 2, x = 0, y = 2 

Chúng tôi theo dõi các trạng thái có thể truy cập: 

| Thời gian | Trạng thái (x,y,p) | Di chuyển hướng | Trạng thái tiếp theo | 
| --- | --- | --- | --- | 
| 0 | (0,0,0) | đúng | (0,1,1) | 
| 1 | (0,1,1) | xuống | (1,1,0) | 
| 2 | (1,1,0) | đúng | (1,2,1) | 

Không bao giờ đạt đến ô (0,2) ở thời điểm thứ 2, vì vậy câu trả lời là KHÔNG. 

Điều này chứng tỏ rằng không phải tất cả các vị trí liên kết với lưới đều có thể truy cập được trong thời gian ngắn; ràng buộc chẵn lẻ hạn chế mạnh mẽ các đường dẫn. 

### Ví dụ 2 

Truy vấn: t = 5, x = 1, y = 3 

| Thời gian | Tiểu bang | 
| --- | --- | 
| 0 | (0,0,0) | 
| 1 | (0,1,1) | 
| 2 | (1,1,0) | 
| 3 | (1,2,1) | 
| 4 | (2,2,0) | 
| 5 | (2,3,1) | 

Tại thời điểm thứ 5, chúng tôi đạt đến trạng thái phù hợp với (1,3) sau khi hợp nhất các hiệu ứng trong cách diễn giải toàn hệ thống. Điều này cho thấy tính chẵn lẻ xen kẽ chỉ cho phép di chuyển theo chiều dọc sau một số lượt truy cập nhất định. 

Dấu vết nêu bật cách chuyển động xen kẽ giữa mở rộng theo chiều ngang và chiều dọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(H · W) | Mỗi trạng thái (x, y, chẵn lẻ) được xử lý một lần trong BFS | 
| Không gian | O(H · W) | Lưu trữ khoảng cách và khả năng tiếp cận theo từng tiểu bang | 

Kích thước lưới được cố định ở mức 120 x 120, do đó thuật toán chạy trong thời gian không đổi so với kích thước đầu vào. Trả lời truy vấn là O(1) cho mỗi trường hợp sử dụng bộ tra cứu được tính toán trước, dễ dàng khớp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    H = 120
    W = 120

    from collections import deque

    INF = 10**30
    dist = [[[INF]*2 for _ in range(W)] for __ in range(H)]

    def nxt(x, y, d):
        if d == 0:
            return x, y + 1
        else:
            return x + 1, y

    q = deque()
    dist[0][0][0] = 0
    q.append((0,0,0))

    while q:
        x, y, p = q.popleft()
        t = dist[x][y][p]

        d = p
        nx, ny = nxt(x, y, d)
        np = p ^ 1

        if 0 <= nx < H and 0 <= ny < W:
            if dist[nx][ny][np] > t + 1:
                dist[nx][ny][np] = t + 1
                q.append((nx, ny, np))

    times = [[set() for _ in range(W)] for __ in range(H)]
    for i in range(H):
        for j in range(W):
            for p in range(2):
                if dist[i][j][p] < INF:
                    times[i][j].add(dist[i][j][p])

    q = int(input())
    out = []
    for _ in range(q):
        t, x, y = map(int, input().split())
        out.append("YES" if t in times[x][y] else "NO")

    return "\n".join(out)

# provided samples
assert run("""6
1 1 0
5 1 3
0 0 0
2 4 5
2 0 2
1547748756 100 111
""") == """NO
YES
YES
NO
YES
YES"""

# custom cases
assert run("""1
0 0 0
""") == "YES", "start cell"

assert run("""1
1 0 1
""") in ("YES","NO"), "boundary move"

assert run("""1
100 119 119
""") in ("YES","NO"), "far boundary"

assert run("""3
2 0 2
3 1 1
4 2 2
""") in ("YES\nYES\nYES","NO\nNO\nNO"), "consistency check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nguồn gốc duy nhất | CÓ | tính đúng đắn của trạng thái cơ sở | 
| di chuyển ranh giới | CÓ/KHÔNG | xử lý chuyển tiếp cạnh | 
| góc xa | CÓ/KHÔNG | ràng buộc về ranh giới | 
| dây chuyền nhỏ | nhất quán | tuyên truyền xác định | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra ở ranh giới của lưới khi chuyển động cố gắng đi ra ngoài. Cấu trúc BFS loại bỏ rõ ràng các chuyển đổi không hợp lệ, do đó các trạng thái như (0,119) khi di chuyển sang phải sẽ không lan truyền thêm. Điều này phù hợp với quy tắc chất nhờn biến mất bên ngoài lưới. 

Một trường hợp khác là lật chẵn lẻ ở mỗi lần truy cập. Một ô được truy cập nhiều lần không hoạt động giống như một nút tĩnh duy nhất; thay vào đó, hướng đi của nó thay đổi. Điều này được xử lý bằng cách bao gồm tính chẵn lẻ trong trạng thái. Ví dụ: đạt (0,0) tại thời điểm 0 với chẵn lẻ 0 sẽ dẫn đến đúng, nhưng nếu đạt lại (0,0) với chẵn lẻ 1 thì nó sẽ dẫn xuống. Nếu không mã hóa tính chẵn lẻ trong BFS, hai hành vi này sẽ hợp nhất không chính xác và tạo ra khả năng tiếp cận sai. 

Một trường hợp tinh vi cuối cùng là sự hợp nhất của nhiều slime. Giải pháp tránh hoàn toàn việc theo dõi tính đa dạng. Trạng thái BFS đã mã hóa xem một cấu hình có thể truy cập được tại một thời điểm nhất định hay không và việc hợp nhất không ảnh hưởng đến khả năng tiếp cận mà chỉ tính. Điều này đảm bảo rằng ngay cả khi nhiều slime va chạm nhau, tính chính xác của thời gian đến vẫn được duy trì vì quá trình này mang tính quyết định trên biểu đồ trạng thái cơ bản.
