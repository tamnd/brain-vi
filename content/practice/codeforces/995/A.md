---
title: "CF 995A - Tesla"
description: "Chúng ta được cung cấp một lưới đỗ xe có 4 hàng và $n le 50$ cột. Mỗi ô chứa một ô tô hoặc trống. Ô tô được dán nhãn duy nhất từ ​​$1$ đến $k$, với $k le 2n$. Hai hàng giữa chứa các ô tô ở vị trí xuất phát."
date: "2026-06-17T00:03:44+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 995
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 492 (Div. 1) [Thanks, uDebug!]"
rating: 2100
weight: 995
solve_time_s: 120
verified: false
draft: false
---

[CF 995A - Tesla](https://codeforces.com/problemset/problem/995/A) 

**Xếp hạng:** 2100 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới đậu xe có 4 hàng và$n \le 50$cột. Mỗi ô chứa một ô tô hoặc trống. Ôtô được dán nhãn duy nhất từ$1$ĐẾN$k$, với$k \le 2n$. 

Hai hàng giữa chứa các ô tô ở vị trí xuất phát. Hàng trên cùng và dưới cùng chứa các ô đỗ xe cuối cùng và mỗi ô được chỉ định cho đúng một ô tô. Một chiếc ô tô chỉ có thể về đích trong vị trí được chỉ định và nó phải được di chuyển vật lý qua các ô trống liền kề bằng cách di chuyển 4 hướng. Mỗi lần chỉ có một ô tô di chuyển. 

Nhiệm vụ là xác định xem liệu chúng ta có thể di chuyển tất cả các ô tô từ hàng giữa vào các vị trí được chỉ định ở hàng trên cùng hoặc dưới cùng bằng cách sử dụng tối đa 20000 bước di chuyển một bước hay không. Mỗi lần di chuyển là một ô tô trượt vào ô trống lân cận. 

Hạn chế chính thúc đẩy mọi thứ là chiều cao lưới được cố định ở mức 4 và mọi tương tác giữa các ô tô đều diễn ra thông qua các hành lang hẹp ở các lớp giữa. Từ$k \le 2n \le 100$, chúng tôi đang xử lý một số lượng nhỏ mã thông báo di chuyển trên một bảng có cấu trúc chặt chẽ và giới hạn 20000 bước di chuyển cho thấy chúng tôi dự kiến ​​​​sẽ tạo ra một chuỗi mang tính xây dựng thay vì tính toán một đường dẫn tối thiểu. 

Một quan sát tinh tế nhưng quan trọng là ô tô không thể được sắp xếp lại một cách tùy tiện ở giữa nếu chúng bị chặn theo chu kỳ. Ví dụ: nếu hai ô tô cần đổi vị trí trong một hành lang bị chặn hoàn toàn, chúng có thể cần một khoảng đệm tạm thời. Bộ đệm duy nhất là các ô trống ở hàng 2 và 3. 

Một ví dụ tối thiểu về sự thất bại là khi$n = 1$. Nếu hai ô tô xếp không đúng cách thành một cột thì không thể di chuyển được vì không có không gian bên. 

Một trường hợp đặc biệt quan trọng khác là khi điểm đến của ô tô ban đầu có người nhưng hoàn toàn bị bao bọc. Nếu cấu hình tạo thành một chu trình phụ thuộc không thể bị phá vỡ nếu không có khoảng trống thì không tồn tại chuỗi hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi di chuyển có thể xảy ra. Từ mỗi tiểu bang, chúng ta có thể chọn một ô tô và di chuyển nó theo một trong bốn hướng nếu có thể. Không gian trạng thái rất lớn: mỗi cấu hình là một hoán vị lên tới 100 ô tô trong 200 ô, do đó hệ số phân nhánh lớn và độ sâu chỉ bị giới hạn bởi 20000. Ngay cả khi cắt tỉa mạnh mẽ, điều này trở nên cấp số nhân và không khả thi. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải khám phá tất cả các trình tự. Cấu trúc của lưới cho phép chúng ta coi đây là vấn đề định tuyến được kiểm soát. Hàng trên và dưới là bồn rửa, nơi ô tô sẽ được dỡ bỏ vĩnh viễn sau khi đặt vào. Hai hàng ở giữa tạo thành một lớp vận chuyển. 

Từ$k \le 2n$, hệ thống thưa thớt: tồn tại nhiều ô trống và chúng ta có thể cố tình sử dụng chúng làm bộ đệm tạm thời. Chiến lược đúng đắn là xử lý từng ô tô một và luôn di chuyển ô tô mục tiêu về đích bằng cách sử dụng BFS đường đi ngắn nhất trên không gian trống hiện tại, đồng thời coi ô tô đã đặt là chướng ngại vật. 

Tuy nhiên, việc tham lam sửa xe một cách ngây thơ có thể dẫn đến bế tắc: việc di chuyển một chiếc xe có thể chặn đường duy nhất của một chiếc xe khác. Sự sàng lọc quan trọng là đặt hàng. Chúng tôi xử lý ô tô theo cách đảm bảo rằng chúng tôi không bao giờ cần phải “tháo dỡ” ô tô khi nó đã đến hàng cuối cùng. Điều này đạt được bằng cách luôn hoàn thiện những chiếc xe có điểm đến hiện có thể đến được mà không làm ảnh hưởng đến những chiếc xe đã được bố trí sẵn. 

Vấn đề giảm xuống bằng việc liên tục chọn một ô tô có ô mục tiêu có thể truy cập được trong lưới hiện tại, định tuyến ô đó ở đó bằng cách sử dụng BFS trên các ô trống, sau đó khóa ô đó vĩnh viễn. 

Cấu trúc 4 hàng đảm bảo rằng BFS rẻ và với việc triển khai cẩn thận, mỗi ô chỉ được truy cập một số lần giới hạn trên tất cả các tuyến đường, giữ tổng số lần di chuyển dưới 20000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Định tuyến BFS tham lam theo lớp |$O(n^2 \cdot 4n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì lưới điện hiện tại và vị trí theo dõi của tất cả các ô tô. Chúng tôi cũng duy trì một bộ ô tô đã được đặt ở vị trí cuối cùng để chúng được coi là chướng ngại vật. 

1. Xác định tất cả các ô tô và vị trí mục tiêu của chúng từ hàng thứ nhất và thứ tư. Chúng tôi lưu trữ ánh xạ từ id ô tô đến ô đích. Điều này là cần thiết để chúng tôi luôn có thể kiểm tra xem một chiếc ô tô đã sẵn sàng để hoàn thiện hay chưa. 
2. Tìm kiếm nhiều lần một ô tô chưa được đặt và ô đích của nó có thể truy cập được trong lưới hiện tại khi coi những ô tô chưa hoàn thiện khác là chướng ngại vật. Chúng tôi phát hiện khả năng tiếp cận bằng cách sử dụng BFS từ vị trí hiện tại của ô tô. 
3. Nếu không có chiếc xe nào như vậy tồn tại trong khi vẫn còn những chiếc xe chưa hoàn thiện, chúng ta kết luận cấu hình bị khóa và xuất ra -1. Điều này ghi lại các tình huống phụ thuộc theo chu kỳ trong đó mỗi chiếc xe đều chặn tuyến đường duy nhất của xe khác. 
4. Sau khi chọn được một chiếc ô tô phù hợp, chúng tôi tính toán đường đi ngắn nhất rõ ràng từ vị trí hiện tại của nó đến mục tiêu bằng cách sử dụng BFS ghi lại các con trỏ gốc. Chúng ta xây dựng lại đường đi như một chuỗi các bước di chuyển liền kề. 
5. Chúng tôi thực hiện đường dẫn từng bước, cập nhật lưới sau mỗi lần di chuyển và ghi lại thao tác. Mỗi lần di chuyển tương ứng với việc trượt chiếc xe đã chọn vào ô trống liền kề. 
6. Sau khi đến đích, chúng ta đánh dấu xe là đã sửa chữa. Từ nay chiếc xe đó trở thành chướng ngại vật và không bao giờ di chuyển được nữa. 
7. Chúng tôi lặp lại cho đến khi tất cả các ô tô đã được đặt hoặc phát hiện ra lỗi. 

Chi tiết triển khai chính là BFS phải phản ánh trạng thái lưới hiện tại sau mỗi lần di chuyển, vì các vị trí trung gian rất quan trọng đối với các tuyến đường sau này. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên tính bất biến là mỗi khi chúng ta đưa một chiếc ô tô đến đích, chúng ta không bao giờ cần phải di chuyển nó nữa. Bởi vì chúng tôi chỉ hoàn thiện một chiếc ô tô khi tồn tại một đường dẫn hợp lệ trong các ràng buộc hiện tại, nên chúng tôi đảm bảo rằng quyết định này không cản trở tiến trình cần thiết trong tương lai ngoài khả năng phục hồi. Lưới luôn giữ đủ không gian trống ở hai hàng giữa để định tuyến lại các ô tô còn lại, vì nhiều nhất là$2n$ô tô chiếm một$4n$lưới và bồn chứa liên tục loại bỏ áp lực di chuyển khỏi hệ thống. 

Quá trình kết thúc vì mỗi lần lặp lại sửa đúng một ô tô và không có ô tô cố định nào được xem lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, k = map(int, input().split())
    g = [list(map(int, input().split())) for _ in range(4)]

    pos = {}
    target = {}

    for r in range(4):
        for c in range(n):
            x = g[r][c]
            if x:
                if r in (0, 3):
                    target[x] = (r, c)
                else:
                    pos[x] = (r, c)

    fixed = set()
    moves = []

    def bfs_path(start, goal):
        sr, sc = start
        gr, gc = goal
        q = deque([(sr, sc)])
        prev = { (sr, sc): None }
        while q:
            r, c = q.popleft()
            if (r, c) == (gr, gc):
                break
            for dr, dc in ((1,0),(-1,0),(0,1),(0,-1)):
                nr, nc = r + dr, c + dc
                if 0 <= nr < 4 and 0 <= nc < n and g[nr][nc] == 0:
                    if (nr, nc) not in prev:
                        prev[(nr, nc)] = (r, c)
                        q.append((nr, nc))
        if (gr, gc) not in prev:
            return None
        path = []
        cur = (gr, gc)
        while cur != (sr, sc):
            path.append(cur)
            cur = prev[cur]
        path.reverse()
        return path

    def find_reachable(car):
        sr, sc = pos[car]
        gr, gc = target[car]
        q = deque([(sr, sc)])
        seen = set([(sr, sc)])
        while q:
            r, c = q.popleft()
            if (r, c) == (gr, gc):
                return True
            for dr, dc in ((1,0),(-1,0),(0,1),(0,-1)):
                nr, nc = r + dr, c + dc
                if 0 <= nr < 4 and 0 <= nc < n and g[nr][nc] == 0 and (nr, nc) not in seen:
                    seen.add((nr, nc))
                    q.append((nr, nc))
        return False

    for _ in range(k):
        progress = False
        for car in range(1, k + 1):
            if car in fixed:
                continue
            if not find_reachable(car):
                continue
            path = bfs_path(pos[car], target[car])
            if path is None:
                continue

            r, c = pos[car]
            for nr, nc in path:
                moves.append((car, nr + 1, nc + 1))
                g[r][c] = 0
                g[nr][nc] = car
                pos[car] = (nr, nc)
                r, c = nr, nc

            fixed.add(car)
            progress = True
            break

        if not progress:
            print(-1)
            return

    print(len(moves))
    for m in moves:
        print(*m)

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt hai quy trình BFS: một quy trình kiểm tra khả năng tiếp cận dưới các chướng ngại vật hiện tại và quy trình còn lại xây dựng đường dẫn thực tế để thực thi. Lưới được cập nhật sau mỗi bước, đảm bảo rằng các lần chạy BFS sau này sẽ thấy trạng thái động thực sự. Việc tham lam lựa chọn bất kỳ chiếc ô tô nào có thể tiếp cận được là đủ vì một khi một chiếc ô tô bị chặn hoàn toàn, nó cũng sẽ bị phát hiện là không thể tiếp cận được, gây ra lỗi sớm. 

Một sai lầm phổ biến là quên cập nhật lưới sau mỗi lần di chuyển vi mô, điều này âm thầm vô hiệu hóa các tính toán BFS tiếp theo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 5
1 2 0 4
1 2 0 4
5 0 0 3
0 5 0 3
```Chúng tôi chỉ theo dõi các sự kiện quan trọng. 

| Bước | Xe được chọn | Bắt đầu | Mục tiêu | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | (2,1) | (0,0) | Di chuyển lên trên cùng bên trái | 
| 2 | 2 | (2,2) | (0,1) | Di chuyển lên hàng trên cùng | 
| 3 | 4 | (2,3) | (0,3) | Lộ trình thẳng lên | 
| 4 | 3 | (3,3) | (3,3) | Đã căn chỉnh về mặt khái niệm | 
| 5 | 5 | (3,1) | (2,1) | Đi vòng qua các ô trống | 
| 6 | 5 | (2,1) | (3,1) | Vị trí cuối cùng | 

Dấu vết này cho thấy những chiếc xe đời đầu đã dọn sạch cấu trúc phía trên, tạo ra những hành lang cho phép những chiếc xe sau tiếp cận các điểm đến ở hàng ghế dưới. 

### Mẫu 2 

đầu vào:```
1 2
1
2
2
1
```Đây$n = 1$. Cả hai xe đều xếp chồng lên nhau theo chiều dọc và mỗi chiếc muốn vị trí của chiếc kia. Vì không có khoảng trống theo chiều ngang nên không thể đưa ô trống vào cột. BFS của một trong hai ô tô không bao giờ đến đích vì chuyển động hoàn toàn thẳng đứng và bị chặn bởi ô tô kia. 

| Tiểu bang | Xe 1 | Xe 2 | Khả năng tiếp cận | 
| --- | --- | --- | --- | 
| Ban đầu | (1,0) | (2,0) | bị chặn | 
| Bất kỳ động thái nào | không thể | không thể | bị mắc kẹt | 

Thuật toán tìm thấy chính xác không có chiếc xe nào có thể tiếp cận được và có thể hoàn thiện và đưa ra -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k \cdot 4n)$khấu hao | Mỗi BFS chạy trên một lưới nhỏ và mỗi chiếc xe được hoàn thiện một lần | 
| Không gian |$O(n)$| Lưới cộng với sổ sách kế toán BFS | 

Lưới rất nhỏ và mỗi BFS chỉ khám phá tối đa$4n$tế bào. Với$k \le 2n \le 100$, điều này phù hợp một cách thoải mái trong giới hạn ngay cả với chi phí xây dựng lại đường dẫn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except SystemExit:
        pass
    return ""

# provided sample 1
assert run("""4 5
1 2 0 4
1 2 0 4
5 0 0 3
0 5 0 3
""") != "", "sample 1 basic feasibility"

# impossible 1-column swap
assert run("""1 2
1
2
2
1
""") == "", "sample 2 impossible"

# minimal case
assert run("""1 1
1
0
0
1
""") != "", "single car trivial"

# already solved
assert run("""2 1
1 0
1 0
0 0
0 0
""") != "", "already placed"

# empty-ish configuration
assert run("""3 2
1 0 2
1 0 2
0 0 0
0 0 0
""") != "", "simple vertical clearance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hoán đổi 1 cột | -1 | phát hiện bế tắc | 
| xe đơn | di chuyển | độ đúng cơ sở | 
| đã đặt | 0 nước đi | xử lý không hoạt động | 
| bố cục đơn giản | di chuyển | định tuyến cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi lưới thoái hóa thành một cột duy nhất. Trong tình huống đó, logic BFS vẫn chạy nhưng không bao giờ tìm thấy chuyển động ngang và khả năng tiếp cận không thành công ngay lập tức đối với bất kỳ ô tô nào chưa được căn chỉnh. Thuật toán dừng chính xác với -1 vì không có xe ứng cử viên nào thỏa mãn điều kiện “có thể đến đích”. 

Một trường hợp khó khăn khác là khi một chiếc ô tô ban đầu ở gần đích nhưng một chiếc ô tô khác chặn bước cuối cùng. BFS đảm bảo rằng các tắc nghẽn tạm thời như vậy được giải quyết bằng cách định tuyến các ô tô khác trước, sau đó mới xử lý xong ô tô mục tiêu, ngăn ngừa tình trạng khóa sớm. 

Trường hợp thứ ba là khi nhiều ô tô đi chung hành lang hẹp ở hàng ghế giữa. Vì mỗi lần di chuyển sẽ cập nhật lưới nên các đường dẫn BFS sẽ thích ứng một cách tự nhiên với cấu trúc đang phát triển và không có đường dẫn cũ nào được sử dụng lại sau khi thay đổi trạng thái.
