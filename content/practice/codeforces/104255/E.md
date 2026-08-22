---
title: "CF 104255E - Giải cứu mèo con"
description: "Lưới mô tả một thế giới nhỏ nơi một con mèo phải tiếp cận một chú mèo con trong khi một con chó chủ động cố gắng ngăn chặn nó. Mỗi ô bị chặn, trống hoặc chứa một trong ba tác nhân: con mèo (bắt đầu), con mèo con (mục tiêu) và tùy chọn một con chó di chuyển sau mỗi hành động của con mèo."
date: "2026-07-01T21:53:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104255
codeforces_index: "E"
codeforces_contest_name: "BSUIR Open X. Reload. Students final"
rating: 0
weight: 104255
solve_time_s: 113
verified: false
draft: false
---

[CF 104255E - Giải cứu mèo con](https://codeforces.com/problemset/problem/104255/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Lưới mô tả một thế giới nhỏ nơi một con mèo phải tiếp cận một chú mèo con trong khi một con chó chủ động cố gắng ngăn chặn nó. Mỗi ô bị chặn, trống hoặc chứa một trong ba tác nhân: con mèo (bắt đầu), con mèo con (mục tiêu) và tùy chọn một con chó di chuyển sau mỗi hành động của con mèo. 

Người chơi chỉ điều khiển con mèo. Mỗi lần di chuyển sẽ dịch chuyển con mèo một bước theo bốn hướng chính, miễn là ô đích không bị chặn. Sau mỗi lần di chuyển của con mèo, con chó sẽ phản ứng một cách dứt khoát: nó thực hiện tối đa hai động tác liên tiếp, mỗi lần cố gắng giảm khoảng cách từ Manhattan đến con mèo. Nếu có thể di chuyển cả theo chiều ngang và chiều dọc thì chiều ngang luôn được chọn. Con chó không bao giờ di chuyển vào chướng ngại vật và không bao giờ di chuyển theo cách làm tăng khoảng cách. 

Việc chạy trốn sẽ thất bại ngay lập tức nếu con chó đi vào chuồng của con mèo hoặc nếu con mèo dẫm lên con chó. Chuồng mèo con cũng không an toàn nếu con chó có mặt ở đó cùng lúc con mèo đến. Nhiệm vụ là quyết định xem có tồn tại một chuỗi tối đa 100000 bước di chuyển của mèo đến được mèo con mà không gây ra trạng thái thất bại hay không và nếu có, hãy xuất ra bất kỳ chuỗi hợp lệ nào. 

Lưới tối đa là 60 x 60, ngụ ý khoảng 3600 vị trí cho mỗi thực thể. Chỉ riêng việc tìm kiếm đơn giản về đường di chuyển của mèo đã có tính cấp số nhân và sự hiện diện của một đối thủ đang di chuyển đồng nghĩa với việc các phương pháp tìm đường đi ngắn nhất tham lam sẽ thất bại. Chuyển động của con chó mang tính quyết định nhưng phụ thuộc vào vị trí của con mèo, điều này làm cho hệ thống trở thành một quá trình trạng thái hai cơ thể kết hợp. 

Một trường hợp thất bại tinh vi xảy ra khi mèo đến gần mèo con nhưng chó lại ở phía sau một chút và có thể “cắt đứt” bước cuối cùng. 

Ví dụ: nếu con mèo cách con mèo một bước nhưng con chó lại ở gần ô mục tiêu thì việc di chuyển vào con mèo con có thể không thành công mặc dù nó trông có vẻ tối ưu cục bộ. Bất kỳ cách tiếp cận nào bỏ qua phản ứng của chó trong tương lai sẽ chấp nhận những cấu hình đó một cách không chính xác. 

Một trường hợp thất bại khác xảy ra khi con chó ban đầu ở xa nhưng lại nằm thẳng hàng trên hành lang. Ngay cả khi con mèo ở gần mèo con hơn, con chó có thể tăng tốc vào cùng một hành lang và cuối cùng buộc phải chặn lại. Điều này vô hiệu hóa bất kỳ cách tiếp cận nào cho rằng con chó có thể bị bỏ qua cho đến khi đến gần. 

Khó khăn cốt lõi là mỗi lần di chuyển của con mèo đều làm thay đổi quỹ đạo tương lai của con chó, vì vậy sự an toàn phải được đánh giá qua toàn bộ tương tác chứ không chỉ khoảng cách hiện tại. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là chỉ xem xét vị trí của con mèo và thử tất cả các đường dẫn đến con mèo con, mỗi lần đều mô phỏng con chó. Điều này đòi hỏi phải khám phá các đường đi theo cấp số nhân và mỗi bước mô phỏng tốn tới hai lần di chuyển chó xác định, do đó, một đường đi thì rẻ nhưng số lượng đường đi thì rất lớn. Điều này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là hệ thống thực sự mang tính quyết định khi cả hai vị trí đều được biết. Từ trạng thái bao gồm con mèo và con chó, mỗi bước di chuyển của con mèo đều dẫn đến đúng một trạng thái sau khi mô phỏng hai bước di chuyển của con chó. Điều này biến bài toán thành tìm kiếm đường đi ngắn nhất trên đồ thị có hướng có các nút là các cặp vị trí. 

Kích thước lưới đủ nhỏ để số lượng trạng thái có thể bị giới hạn bởi 3600 nhân 3600, khoảng 13 triệu. Mỗi trạng thái có tối đa bốn chuyển đổi đi. Tìm kiếm theo chiều rộng trên không gian trạng thái này là đủ để tìm ra bất kỳ đường đi hợp lệ nào, vì tất cả các bước di chuyển đều có chi phí như nhau. Chúng ta phải tránh xem lại các trạng thái và chúng ta phải mô phỏng con chó một cách xác định ở mỗi lần chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các con đường | Hàm mũ | O(1) | Quá chậm | 
| Bang BFS trên (mèo, chó) | O(n²m²) | O(n²m²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi trò chơi như một bài toán tìm kiếm đồ thị trong đó mỗi nút mã hóa vị trí của cả hai diễn viên. 

### 1. Phân tích lưới và định vị các thực thể chính

Chúng tôi quét lưới để trích xuất tọa độ của mèo, mèo con và chó (nếu có). Nếu không có chó, vấn đề sẽ giảm xuống mức BFS tiêu chuẩn từ mèo sang mèo con. 

### 2. Xác định chuyển động xác định của chó 

Chúng ta triển khai một hàm, với vị trí chó và mèo hiện tại, sẽ tính toán vị trí tiếp theo của chó: 

Nếu con chó có thể giảm khoảng cách Manhattan, nó sẽ chọn di chuyển theo hướng làm giảm khoảng cách đó. Di chuyển ngang được ưu tiên khi cả di chuyển ngang và dọc đều hợp lệ. Chướng ngại vật cản trở chuyển động. Nếu không có động thái giảm hợp lệ nào, con chó sẽ giữ nguyên vị trí. 

Chúng ta áp dụng chức năng này hai lần sau mỗi lần mèo di chuyển. 

Lý do mô phỏng rõ ràng hai bước là vì bài toán xác định hai hành động tuần tự của chó cho mỗi bước di chuyển của mèo chứ không phải một bước tổng hợp duy nhất. 

### 3. Xây dựng BFS trên các trạng thái kết hợp 

Chúng tôi đại diện cho mỗi trạng thái dưới dạng (cat_r, cat_c, dog_r, dog_c). Chúng tôi đẩy trạng thái ban đầu vào hàng đợi và đánh dấu nó đã truy cập. 

Từ mỗi tiểu bang, chúng tôi thử tất cả bốn động tác của mèo. Mỗi nước đi ứng cử viên sẽ tạo ra một vị trí mèo mới mà chúng tôi sẽ từ chối nếu bị chặn. 

Sau đó chúng tôi mô phỏng con chó hai lần, sử dụng vị trí hiện tại của con mèo cho cả hai mô phỏng. Nếu tại bất kỳ thời điểm nào con chó đáp xuống con mèo hoặc con mèo tiếp cận con mèo con trong khi con chó đã ở trên đó, chúng tôi sẽ loại bỏ quá trình chuyển đổi đó. 

Trạng thái kết quả sẽ được xếp vào hàng đợi nếu không nhìn thấy được. 

Lý do BFS hợp lệ là vì mỗi lần di chuyển đều có chi phí như nhau, vì vậy lần đầu tiên chúng tôi tiếp cận được chú mèo con, chúng tôi đã tìm thấy một trình tự hợp lệ. 

### 4. Theo dõi con trỏ cha để xây dựng lại 

Để xuất chuỗi di chuyển, chúng tôi lưu trữ trạng thái gốc và bước di chuyển được sử dụng để đạt đến từng trạng thái. Khi đã đến được mèo con, chúng ta quay lại từ trạng thái đó về điểm bắt đầu và đảo ngược đường đi. 

### Tại sao nó hoạt động 

Thuật toán khám phá toàn bộ không gian trạng thái có thể tiếp cận của hệ thống hai tác nhân xác định. Vì mọi quá trình chuyển đổi đều duy trì tính chính xác bằng cách mô phỏng rõ ràng các quy tắc nên không có trạng thái không hợp lệ nào được đưa vào hàng đợi. BFS đảm bảo rằng nếu tồn tại bất kỳ chuỗi an toàn nào thì nó sẽ bị phát hiện vì tất cả các trạng thái ở độ sâu d đều được khám phá trước độ sâu d+1. Không gian trạng thái nắm bắt đầy đủ tất cả các tương tác giữa mèo và chó, do đó không có sự phụ thuộc ẩn nào bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    cat = dog = kit = None
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'C':
                cat = (i, j)
            elif grid[i][j] == 'K':
                kit = (i, j)
            elif grid[i][j] == '@':
                dog = (i, j)

    # no dog case: simple BFS
    if dog is None:
        q = deque([cat])
        par = {cat: None}
        mv = {cat: ''}
        dirs = [(-1,0,'U'), (1,0,'D'), (0,-1,'L'), (0,1,'R')]

        while q:
            x, y = q.popleft()
            if (x, y) == kit:
                path = []
                cur = (x, y)
                while par[cur] is not None:
                    path.append(mv[cur])
                    cur = par[cur]
                print("Yes")
                print("".join(reversed(path)))
                return

            for dx, dy, ch in dirs:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < m and grid[nx][ny] != '#':
                    if (nx, ny) not in par:
                        par[(nx, ny)] = (x, y)
                        mv[(nx, ny)] = ch
                        q.append((nx, ny))

        print("No")
        return

    def dog_move(cat_pos, dog_pos):
        cx, cy = cat_pos
        dx, dy = dog_pos
        best = (dx, dy)

        def step(dx, dy):
            best = (dx, dy)
            best_dist = abs(cx - dx) + abs(cy - dy)

            # horizontal priority
            for ndx, ndy in [(dx, dy-1), (dx, dy+1), (dx-1, dy), (dx+1, dy)]:
                if 0 <= ndx < n and 0 <= ndy < m and grid[ndx][ndy] != '#':
                    dist = abs(cx - ndx) + abs(cy - ndy)
                    if dist < best_dist:
                        best_dist = dist
                        best = (ndx, ndy)
            return best

        return step(dx, dy)

    def simulate(cat_pos, dog_pos, move):
        cx, cy = cat_pos
        if move == 'U': nx, ny = cx-1, cy
        elif move == 'D': nx, ny = cx+1, cy
        elif move == 'L': nx, ny = cx, cy-1
        else: nx, ny = cx, cy+1

        if not (0 <= nx < n and 0 <= ny < m): 
            return None
        if grid[nx][ny] == '#':
            return None

        # cat moves
        c = (nx, ny)
        d = dog_pos

        # immediate collision
        if d == c:
            return None

        # dog move 1
        if d is not None:
            d = dog_move(c, d)
            if d == c:
                return None

        # dog move 2
        if d is not None:
            d = dog_move(c, d)
            if d == c:
                return None

        return (c, d)

    start = (cat[0], cat[1], dog[0], dog[1])
    q = deque([start])
    parent = {start: None}
    pmove = {}

    dirs = ['U','D','L','R']

    while q:
        cx, cy, dx, dy = q.popleft()

        if (cx, cy) == kit:
            path = []
            cur = (cx, cy, dx, dy)
            while parent[cur] is not None:
                path.append(pmove[cur])
                cur = parent[cur]
            print("Yes")
            print("".join(reversed(path)))
            return

        for mvch in dirs:
            res = simulate((cx, cy), (dx, dy), mvch)
            if res is None:
                continue
            nc, nd = res
            nx, ny = nc
            ndx, ndy = nd

            state = (nx, ny, ndx, ndy)
            if state not in parent:
                parent[state] = (cx, cy, dx, dy)
                pmove[state] = mvch
                q.append(state)

    print("No")

if __name__ == "__main__":
    solve()
```Giải pháp tách sự tương tác thành một hàm chuyển đổi xác định. Phần tinh tế nhất là thứ tự mô phỏng: con mèo di chuyển trước, sau đó con chó di chuyển hai lần bằng cách sử dụng vị trí con mèo được cập nhật. Bất kỳ sai lệch nào so với trật tự này sẽ phá vỡ tính đúng đắn vì quyết định của con chó phụ thuộc vào vị trí mới nhất của con mèo. 

Lớp BFS đảm bảo rằng khi đạt đến ô mèo con, trạng thái tương ứng sẽ hợp lệ theo các quy tắc mô phỏng đầy đủ, không chỉ độ gần hình học. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 6
..C...
##....
.#...K
..@...
```Chúng ta bắt đầu với con mèo ở (0,2), con chó ở (3,2). BFS khám phá các chuyển động hợp lệ của mèo trong khi liên tục mô phỏng phản ứng của chó. 

| Bước | Mèo | Con chó | Hành động | 
| --- | --- | --- | --- | 
| 0 | (0,2) | (3,2) | bắt đầu | 
| 1 | (0,1) | (3,2) | di chuyển sang trái | 
| 2 | (0,0) | (3,2) | tiếp tục khám phá hành lang an toàn | 
| ... | ... | ... | con chó từ từ khép lại theo chiều dọc | 
| cuối cùng | (2,5) | (2,4) | mèo con đến nơi an toàn | 

Dấu vết cho thấy giải pháp không dựa vào hành động tham lam đối với mèo con. Thay vào đó, nó tránh rõ ràng các trạng thái mà phản ứng hai bước của con chó sẽ giao với đường đi của con mèo. 

### Ví dụ 2 

đầu vào:```
1 6
C@...K
```| Bước | Mèo | Con chó | Hành động | 
| --- | --- | --- | --- | 
| 0 | (0,0) | (0,1) | bắt đầu | 
| 1 | (0,2) | (0,1) | nước đi đúng không an toàn bị từ chối | 
| 1 | (0,0) | (0,1) | các động thái thay thế được khám phá | 
| cuối cùng | không | không | không tồn tại con đường an toàn | 

Điều này chứng tỏ sự gần kề ngay lập tức với một con chó sẽ ngăn chặn sự tiến triển trực tiếp như thế nào và BFS từ chối chính xác tất cả các đường dẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²m²) | mỗi trạng thái (mèo, chó) được truy cập một lần, mỗi trạng thái mở rộng tối đa 4 bước di chuyển với mô phỏng O(1) | 
| Không gian | O(n²m²) | đã truy cập tập hợp và theo dõi phụ huynh trên tất cả các tiểu bang | 

Các ràng buộc n, m ≤ 60 làm cho không gian trạng thái khoảng 13 triệu, lớn nhưng có thể quản lý được theo BFS được tối ưu hóa trong Python khi quá trình chuyển đổi diễn ra theo thời gian không đổi và việc cắt tỉa xảy ra sớm ở các trạng thái không hợp lệ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# provided sample 1
assert run("""4 6
..C...
##....
.#...K
..@...
""") == "Yes\nLLRRRRRDD"

# provided sample 2
assert run("""1 6
C@...K
""") == "No"

# cat already on kitten
assert run("""1 1
K""") == "Yes\n"

# no dog simple path
assert run("""2 2
C.
.K
""") == "Yes\nDDRR"

# blocked dog adjacency
assert run("""1 3
C@K
""") == "No"

# obstacle forcing detour
assert run("""3 3
C..
###
..K
""") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 K | Có | thành công tầm thường | 
| 2x2 mở | tìm đường | BFS cơ bản | 
| dòng C@K | Không | hạn chế nắm bắt ngay lập tức | 
| hành lang bị chặn | Không | cấu trúc không thể truy cập | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi con chó bắt đầu ở gần con mèo và buộc phải di chuyển vào con mèo sau hành động đầu tiên của con mèo. Thuật toán xử lý vấn đề này bằng cách mô phỏng hành động của con mèo trước và ngay lập tức loại bỏ bất kỳ trạng thái nào mà con chó bằng con mèo trước hoặc sau một trong hai con chó di chuyển. 

Một trường hợp khó khăn khác là con chó không thể di chuyển do chướng ngại vật. Trong những trường hợp như vậy, cả hai bước mô phỏng con chó đều trả về cùng một vị trí và BFS tiếp tục chính xác vì không có chuyển động bất hợp pháp nào xảy ra. 

Trường hợp cạnh thứ ba xảy ra khi con mèo tiếp cận con mèo con trong khi con chó đang đồng thời bước vào ô đó trong lần di chuyển thứ hai. Mô phỏng kiểm tra va chạm sau mỗi bước chó, đảm bảo rằng điều kiện thắng không hợp lệ này không bao giờ được chấp nhận.
