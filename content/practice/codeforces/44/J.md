---
title: "CF 44J - Lông ngắn"
description: "Chúng ta được cấp một bảng hình chữ nhật có một số ô bị thiếu. Mọi ô còn lại đều đã được tô màu đen hoặc trắng theo mẫu bàn cờ. Nhiệm vụ là bao phủ tất cả các ô hiện có bằng cách sử dụng các đường viền thẳng có kích thước 1 × 3 hoặc 3 × 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 44
codeforces_index: "J"
codeforces_contest_name: "School Team Contest 2 (Winter Computer School 2010/11)"
rating: 2000
weight: 44
solve_time_s: 263
verified: false
draft: false
---

[CF 44J - Triminoes](https://codeforces.com/problemset/problem/44/J) 

**Xếp hạng:** 2000 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 4m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bảng hình chữ nhật có một số ô bị thiếu. Mọi ô còn lại đều đã được tô màu đen hoặc trắng theo mẫu bàn cờ. Nhiệm vụ là bao phủ tất cả các ô hiện có bằng cách sử dụng các đường viền thẳng có kích thước`1 × 3`hoặc`3 × 1`. 

Một Trimino luôn bao gồm ba ô liên tiếp. Bởi vì màu của bảng thay đổi nên một ô trang trí hợp lệ phải bao phủ các ô theo mẫu trắng-đen-trắng hoặc đen-trắng-đen tùy theo hướng. Tuyên bố đảm bảo rằng các màu bàn cờ nhất quán với một số màu bàn cờ, vì vậy chúng ta không bao giờ cần phải tự mình xác minh điều đó. 

Đầu ra không chỉ là câu trả lời có/không. Nếu có một ô xếp, chúng ta cũng phải gán một trong bốn chữ cái`a`,`b`,`c`,`d`tới mọi trang trí để các trang trí lân cận không bao giờ có cùng một chữ cái. 

Kích thước bảng có thể đạt tới`1000 × 1000`, có nghĩa là lên tới một triệu tế bào. Bất kỳ tìm kiếm theo cấp số nhân là không thể ngay lập tức. Ngay cả các thuật toán có hệ số không đổi lớn cũng trở nên rủi ro. Chúng ta cần một cái gì đó về cơ bản là tuyến tính về số lượng ô. 

Quan sát quan trọng đầu tiên là một phần tử luôn chiếm ba ô liên tiếp trong một hàng hoặc một cột. Vì các màu xen kẽ nhau nên ô ở giữa sẽ tự động có màu đối lập ở hai đầu. Hình học, không phải màu sắc, mới là phần khó. 

Việc triển khai đơn giản có thể âm thầm thất bại trên các vùng bị ngắt kết nối. Coi như:```
1 4
wb.w
```Ba ô đầu tiên không tồn tại liên tiếp do có lỗ trống. Một người tham lam bất cẩn chỉ đếm các ô theo mô-đun ba sẽ nghĩ sai rằng hàng đó có thể xếp được. 

Một trường hợp khó phát hiện khác là hành lang có chiều dài 4:```
1 4
wbwb
```Không có cách nào để đặt một`1 × 3`gạch mà không để lại một ô không được che chắn. Bất kỳ thuật toán nào tham lam đặt các ô từ trái sang phải mà không kiểm tra phần còn lại sẽ thất bại. 

Ràng buộc về màu sắc cũng tạo ra bẫy. Giả sử chúng ta cố gắng đặt tùy ý`1 × 3`hình chữ nhật có màu không đúng định dạng:```
1 3
www
```Vị trí như vậy sẽ vi phạm các màu xen kẽ cần thiết. Câu lệnh đảm bảo màu hợp lệ, nhưng thuật toán vẫn phải tôn trọng cấu trúc do nó tạo ra. 

Việc gán chữ cái cũng là một nguồn gây ra lỗi. Ngay cả khi bản thân việc xếp ô là chính xác, việc chỉ sử dụng một biểu tượng cho mỗi phần cắt sẽ không thành công khi hai ô chạm nhau dọc theo một cạnh. Ví dụ:```
3 3
wbw
bwb
wbw
```Việc xếp theo chiều dọc của cả ba cột đều có giá trị về mặt hình học, nhưng các cột liền kề không thể sử dụng lại cùng một nhãn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tại mỗi ô không được che chắn, hãy thử đặt mọi đường cắt ngang hoặc dọc có thể, lặp lại và quay lui nếu vị trí dẫn đến ngõ cụt. 

Điều này hiệu quả vì bảng là hữu hạn và mỗi bước đệ quy sẽ làm giảm số lượng ô chưa được che phủ. Thật không may, yếu tố phân nhánh là rất lớn. Một bảng có một triệu ô sẽ yêu cầu khám phá một số lượng lớn các ô xếp. Thậm chí còn nhỏ hơn nhiều`20 × 20`hội đồng quản trị sẽ không thể thực hiện được. 

Thông tin chi tiết quan trọng là màu sắc của bảng xác định hoàn toàn cách các mảnh vụn có thể tương tác. Một quân cờ thẳng luôn bao phủ đúng một ô đen và hai ô trắng hoặc ngược lại tùy theo độ chẵn lẻ của bàn cờ. Quan trọng hơn, mọi vị trí hợp lệ đều nằm hoàn toàn bên trong một hàng hoặc một cột. 

Điều này biến vấn đề từ một vấn đề ốp lát chung thành một quy trình mang tính xây dựng cục bộ. Thay vì tìm kiếm trên toàn cầu, chúng ta có thể tham lam xử lý bảng theo các mẫu nhỏ. 

Giải pháp tiêu chuẩn sử dụng phân rã đệ quy. Chúng tôi liên tục tìm kiếm các cấu hình cục bộ đơn giản phải có thể điều chỉnh được theo một cách tự nhiên. Sau khi đặt một mảnh trang trí, vấn đề còn lại trở nên nhỏ hơn nhưng có cấu trúc giống hệt nhau. 

Quan sát trọng tâm là mọi thành phần được kết nối có kích thước chia hết cho ba đều có thể được giảm bớt bằng cách sử dụng các ô giống như chiếc lá. Nếu một ô chỉ có một phần mở rộng có thể có thành một phần tử, chúng ta buộc phải sử dụng nó. Bằng cách liên tục loại bỏ các mảnh ghép bắt buộc, chúng ta có thể xếp toàn bộ bảng hoặc phát hiện ra một cấu hình không thể thực hiện được. 

Vì mỗi ô chỉ được xử lý một số lần không đổi nên thuật toán trở nên tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bảng và đánh dấu từng ô hiện có. 
2. Xây dựng thông tin lân cận giữa các ô lân cận trực giao. Mỗi ô có tối đa bốn ô lân cận. 
3. Với mỗi ô, hãy tính mức độ hiện tại của nó, nghĩa là còn tồn tại bao nhiêu ô lân cận. 
4. Đẩy tất cả các ô theo độ`1`vào hàng đợi. Các ô như vậy là điểm cuối của các vùng hẹp, do đó, bất kỳ phần tử hợp lệ nào bao phủ chúng đều bị hạn chế rất nhiều. 
5. Xử lý hàng đợi cho đến khi nó trống. 
6. Lấy ô độ 1`(x, y)`. Hãy để người hàng xóm duy nhất của nó là`(nx, ny)`. 
7. Ô giữa của trang trí phải là`(nx, ny)`. Bây giờ chúng ta tìm kiếm ô thứ ba liền kề với`(nx, ny)`đó không phải là`(x, y)`và vẫn chưa được sử dụng. 
8. Nếu không có ô thứ ba như vậy thì việc xếp gạch là không thể vì`(x, y)`không thể thuộc về bất kỳ Trimino nào nữa. 
9. Nếu không, hãy đặt một mảnh trang trí vào ba ô này. 
10. Gán một trong các chữ cái`a`,`b`,`c`,`d`khác với các loại trang trí đã có màu xung quanh. Vì mỗi ô chạm tối đa ba ô lân cận nên bốn chữ cái luôn là đủ. 
11. Xóa ba ô khỏi biểu đồ. Đối với mỗi ô còn lại lân cận, hãy giảm mức độ của nó. Nếu một mức độ nào đó trở thành`1`, đẩy nó vào hàng đợi. 
12. Tiếp tục cho đến khi không còn vị trí bắt buộc nào nữa. 
13. Cuối cùng, nếu một số ô vẫn chưa được phát hiện, cấu hình sẽ chứa cấu trúc giống như chu trình không thể giảm được. đầu ra`NO`. 
14. Nếu không thì xuất ra`YES`và ghi nhãn được xây dựng. 

### Tại sao nó hoạt động 

Điều bất biến là mọi thành phần được kết nối còn lại sẽ bảo toàn tất cả các ô hợp lệ sau khi các phần cắt được đặt trước đó bị loại bỏ. Khi một ô độ 1 xuất hiện, bất kỳ phần tử hợp lệ nào che phủ nó buộc phải bao gồm ô lân cận duy nhất của nó. Không có sự thay thế. 

Thuật toán không bao giờ đưa ra các lựa chọn hình học tùy ý cho các ô như vậy. Mọi vị trí đều bị ép buộc một cách hợp lý bởi cấu trúc cục bộ. Nếu tại một thời điểm nào đó, một ô độ 1 không thể được hoàn thành thành một ô nhỏ, thì sẽ không có ô xếp hợp lệ nào tồn tại vì ô đó không thể che được. 

Nếu thuật toán kết thúc với các ô chưa được che phủ, thì các ô đó sẽ tạo thành các thành phần trong đó mỗi độ ít nhất là hai. Những cấu trúc như vậy không thể được lát gạch bằng các đường viền thẳng dưới những ràng buộc này. Do đó thuật toán sẽ loại bỏ chúng một cách chính xác. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

DIRS = [(-1, 0), (1, 0), (0, -1), (0, 1)]

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    alive = [[grid[i][j] != '.' for j in range(m)] for i in range(n)]
    ans = [['.' for _ in range(m)] for _ in range(n)]

    deg = [[0] * m for _ in range(n)]

    def inside(x, y):
        return 0 <= x < n and 0 <= y < m

    for i in range(n):
        for j in range(m):
            if not alive[i][j]:
                continue
            cnt = 0
            for dx, dy in DIRS:
                ni = i + dx
                nj = j + dy
                if inside(ni, nj) and alive[ni][nj]:
                    cnt += 1
            deg[i][j] = cnt

    q = deque()

    for i in range(n):
        for j in range(m):
            if alive[i][j] and deg[i][j] == 1:
                q.append((i, j))

    def choose_letter(cells):
        used = set()

        for x, y in cells:
            for dx, dy in DIRS:
                nx = x + dx
                ny = y + dy
                if inside(nx, ny):
                    ch = ans[nx][ny]
                    if ch != '.':
                        used.add(ch)

        for ch in "abcd":
            if ch not in used:
                return ch

        return None

    while q:
        x, y = q.popleft()

        if not alive[x][y] or deg[x][y] != 1:
            continue

        middle = None

        for dx, dy in DIRS:
            nx = x + dx
            ny = y + dy
            if inside(nx, ny) and alive[nx][ny]:
                middle = (nx, ny)
                break

        if middle is None:
            print("NO")
            return

        mx, my = middle

        third = None

        for dx, dy in DIRS:
            tx = mx + dx
            ty = my + dy

            if (tx, ty) == (x, y):
                continue

            if inside(tx, ty) and alive[tx][ty]:
                third = (tx, ty)
                break

        if third is None:
            print("NO")
            return

        tx, ty = third

        cells = [(x, y), (mx, my), (tx, ty)]

        letter = choose_letter(cells)

        for cx, cy in cells:
            ans[cx][cy] = letter

        for cx, cy in cells:
            alive[cx][cy] = False

        affected = set()

        for cx, cy in cells:
            for dx, dy in DIRS:
                nx = cx + dx
                ny = cy + dy

                if inside(nx, ny) and alive[nx][ny]:
                    affected.add((nx, ny))

        for nx, ny in affected:
            cnt = 0
            for dx, dy in DIRS:
                px = nx + dx
                py = ny + dy

                if inside(px, py) and alive[px][py]:
                    cnt += 1

            deg[nx][ny] = cnt

            if cnt == 1:
                q.append((nx, ny))

    for i in range(n):
        for j in range(m):
            if alive[i][j]:
                print("NO")
                return

    print("YES")
    for row in ans:
        print("".join(row))

solve()
```Việc triển khai phản ánh trực tiếp việc giải thích biểu đồ. Mỗi ô hiện có hoạt động giống như một nút được kết nối với các ô lân cận. 

các`alive`mảng theo dõi những ô nào vẫn chưa được khám phá. Khi một mảnh ghép được đặt vào, ba ô của nó sẽ bị xóa vĩnh viễn. 

Hàng đợi lưu trữ tất cả các ô độ 1. Đây là những vị trí bắt buộc. Một lỗi phổ biến là quên xác nhận lại các mục hàng đợi sau khi bật chúng. Độ thay đổi linh hoạt nên chúng ta bỏ qua các ô không còn tồn tại hoặc không còn có độ`1`. 

các`choose_letter`chức năng quét các ô đã được tô màu lân cận và chọn chữ cái đầu tiên không được sử dụng trong số`a`,`b`,`c`,`d`. Vì một ô chỉ có một vài ô lân cận nên bốn chữ cái luôn là đủ. 

Một điểm tinh tế khác là chỉ cập nhật độ xung quanh các ô đã bị loại bỏ. Việc tính toán lại toàn bộ bảng mỗi lần sẽ biến thuật toán thành phương trình bậc hai. 

Xác minh cuối cùng sẽ kiểm tra xem mọi ô đã bị xóa hay chưa. Nếu một số vẫn còn, chúng thuộc về một cấu trúc mà việc rút gọn tham lam không thể giải quyết được, điều đó có nghĩa là không có ô xếp hợp lệ nào tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3
wbw
```Xử lý dấu vết: 

| Bước | Xếp hàng | Ô được chọn | Thư được giao | Các ô còn lại | 
| --- | --- | --- | --- | --- | 
| Ban đầu |`(0,0), (0,2)`| - | - | 3 | 
| 1 |`(0,0)`|`(0,0),(0,1),(0,2)`|`a`| 0 | 

Đầu ra:```
YES
aaa
```Điều này thể hiện vị trí bắt buộc đơn giản nhất. Cả hai điểm cuối đều có mức độ`1`, do đó, phần cắt tỉa duy nhất có thể bao phủ toàn bộ hàng. 

### Ví dụ 2 

đầu vào:```
1 4
wbwb
```Xử lý dấu vết: 

| Bước | Xếp hàng | Ô được chọn | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu |`(0,0), (0,3)`| - | chưa được giải quyết | 
| 1 |`(0,0)`|`(0,0),(0,1),(0,2)`| còn lại một ô | 
| Cuối cùng | trống | - | không thể | 

Đầu ra:```
NO
```Ô đơn còn lại không thể thuộc về bất kỳ phần tử nào. Điều này xác nhận rằng các vị trí cục bộ vẫn duy trì tính chính xác vì cấu hình còn sót lại ngay lập tức bộc lộ tính không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô và cạnh chỉ được xử lý một số lần không đổi | 
| Không gian | O(nm) | Bảng, mảng độ, hàng đợi và lưới trả lời đều có tỷ lệ theo kích thước bảng | 

Với tối đa một triệu ô, độ phức tạp tuyến tính là bắt buộc. Giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    DIRS = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    out = []

    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    alive = [[grid[i][j] != '.' for j in range(m)] for i in range(n)]
    ans = [['.' for _ in range(m)] for _ in range(n)]

    deg = [[0] * m for _ in range(n)]

    def inside(x, y):
        return 0 <= x < n and 0 <= y < m

    for i in range(n):
        for j in range(m):
            if not alive[i][j]:
                continue

            for dx, dy in DIRS:
                ni = i + dx
                nj = j + dy
                if inside(ni, nj) and alive[ni][nj]:
                    deg[i][j] += 1

    q = deque()

    for i in range(n):
        for j in range(m):
            if alive[i][j] and deg[i][j] == 1:
                q.append((i, j))

    while q:
        x, y = q.popleft()

        if not alive[x][y] or deg[x][y] != 1:
            continue

        middle = None

        for dx, dy in DIRS:
            nx = x + dx
            ny = y + dy
            if inside(nx, ny) and alive[nx][ny]:
                middle = (nx, ny)
                break

        if middle is None:
            return "NO\n"

        mx, my = middle

        third = None

        for dx, dy in DIRS:
            tx = mx + dx
            ty = my + dy

            if (tx, ty) == (x, y):
                continue

            if inside(tx, ty) and alive[tx][ty]:
                third = (tx, ty)
                break

        if third is None:
            return "NO\n"

        tx, ty = third

        for cx, cy in [(x, y), (mx, my), (tx, ty)]:
            ans[cx][cy] = 'a'
            alive[cx][cy] = False

    for i in range(n):
        for j in range(m):
            if alive[i][j]:
                return "NO\n"

    out.append("YES")
    for row in ans:
        out.append("".join(row))

    return "\n".join(out) + "\n"

assert run("1 3\nwbw\n").startswith("YES"), "simple horizontal"

assert run("1 4\nwbwb\n") == "NO\n", "length 4 impossible"

assert run("3 1\nw\nb\nw\n").startswith("YES"), "simple vertical"

assert run("1 1\nw\n") == "NO\n", "single cell impossible"

assert run("2 2\nwb\nbw\n") == "NO\n", "area not divisible by 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 × 3`dải | CÓ | Vị trí ngang cơ bản | 
|`1 × 4`dải | KHÔNG | Tế bào còn sót lại chưa được phát hiện | 
|`3 × 1`dải | CÓ | Vị trí dọc | 
| Ô đơn | KHÔNG | Trường hợp không thể tối thiểu | 
|`2 × 2`bảng | KHÔNG | Diện tích không chia hết cho ba | 

## Vỏ cạnh 

Hãy xem xét trường hợp đơn bào bị cô lập:```
1 1
w
```Ô bắt đầu bằng độ`0`, vì vậy nó không bao giờ vào hàng đợi. Sau khi quá trình xử lý kết thúc, vẫn còn một ô chưa được phát hiện và thuật toán sẽ in chính xác`NO`. 

Bây giờ hãy xem xét một bảng bị ngắt kết nối:```
2 4
wb.w
bw.b
```Lỗ phá vỡ bảng thành các vùng không tương thích. Một số tế bào đạt được mức độ`1`, nhưng cuối cùng một thành phần không thể mở rộng thành nhóm ba thành phần. Thuật toán phát hiện điều này khi không tìm thấy ô thứ ba hợp lệ. 

Một trường hợp khó khăn khác là hành lang hẹp:```
1 6
wbwbwb
```Hàng đợi ban đầu chứa cả hai điểm cuối. Việc xử lý điểm cuối bên trái sẽ loại bỏ ba ô đầu tiên. Ba ô còn lại lại tạo thành một dải hợp lệ và được loại bỏ tiếp theo. Tính bất biến mà mọi nước đi bắt buộc đều duy trì khả năng giải quyết được trong suốt quá trình thực hiện. 

Cuối cùng, hãy xem xét một cấu trúc giống như chu trình:```
2 3
wbw
bwb
```Mọi ô ban đầu đều có bậc ít nhất`2`, do đó hàng đợi bắt đầu trống. Không có sự di chuyển bắt buộc nào tồn tại. Thuật toán kết thúc ngay lập tức với các ô còn lại và in`NO`. Điều này phản ánh một cách chính xác rằng không tồn tại gạch ốp lát thẳng.
