---
title: "CF 102700L - Ngày cô đơn"
description: "Chúng ta có lưới N × M có các ô sạch hoặc bẩn. Các ô chứa S và E cũng sạch. Vitor có thể di chuyển giữa các ô sạch liền kề trong một bước."
date: "2026-08-08T08:28:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "L"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 466
verified: true
draft: false
---

[CF 102700L - Ngày cô đơn](https://codeforces.com/problemset/problem/102700/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`N × M`lưới có các ô sạch hoặc bẩn. Các tế bào chứa`S`Và`E`cũng sạch sẽ. Vitor có thể di chuyển giữa các ô sạch liền kề trong một bước. Ngoài ra, hai ô sạch trong cùng một hàng có thể được kết nối bằng một đường hầm khi mọi ô nằm giữa chúng trong hàng đó đều bẩn. Quy tắc tương tự được áp dụng theo chiều dọc. 

Đường hầm không phải là một kết nối tầm xa tùy ý. Nếu hai ô sạch được kết nối thành một hàng thì không thể có một ô sạch nào khác giữa chúng. Vì vậy, đối với mỗi ô sạch, có thể có nhiều nhất một điểm cuối đường hầm ở mỗi hướng trong số bốn hướng. Do đó, đồ thị chuyển động có mức độ nhiều nhất là bốn. 

Đầu vào cung cấp cho lưới, với chính xác một`S`và chính xác là một`E`. Đầu ra yêu cầu một chuỗi di chuyển ngắn nhất từ ​​​​`S`ĐẾN`E`. Nếu tồn tại một số chuỗi ngắn nhất thì chuỗi hướng nhỏ nhất về mặt từ điển phải được in. Các ký tự chỉ đạo là`D`,`L`,`R`, Và`U`, vì vậy thứ tự từ điển của chúng là`D < L < R < U`. 

Các ràng buộc đủ lớn để bản thân lưới có thể chứa bốn triệu ô. Một thuật toán thực hiện công việc tỷ lệ thuận với kích thước lưới là hợp lý, nhưng việc quét liên tục toàn bộ một hàng hoặc cột cho từng ô thì không. Đặc biệt, một`O(NM max(N,M))`cách tiếp cận có thể thực hiện đại khái`4 · 2000 · 2000 · 2000 = 32 billion`kiểm tra định hướng trong trường hợp xấu nhất. Thậm chí một`O((NM)^2)`việc xây dựng vượt xa giới hạn hai giây cho phép. Chúng ta cần khám phá tất cả các kết nối đường hầm hữu ích trong thời gian tuyến tính cơ bản. 

Có một số trường hợp đặc biệt trong đó việc triển khai đơn giản có thể âm thầm thất bại. 

Hãy xem xét lưới tối thiểu```
2 2
SX
XE
```Không có đường dẫn rõ ràng và không có đường hầm, bởi vì mọi ô trung gian có thể đều nằm ngoài lưới. Đầu ra đúng là`-1`. Việc triển khai bất cẩn coi hai ô được phân tách theo đường chéo là được kết nối hoặc vô tình cho phép một đường hầm xuyên qua ranh giới, sẽ tìm thấy đường dẫn không chính xác. 

Một đường hầm có thể trải dài qua nhiều ô bẩn và vẫn phải được tính chính xác là một lần di chuyển. Ví dụ,```
2 5
SXXXE
.....
```Hàng đầu tiên chứa`S`, ba ô bẩn, và`E`. Vitor có thể nhảy trực tiếp từ`S`ĐẾN`E`, vậy câu trả lời là```
1
R
```Thay vào đó, việc triển khai chỉ xem xét chuyển động liền kề sẽ trả về một đường dẫn có độ dài bốn. 

Điểm cuối của đường hầm phải sạch sẽ. Ví dụ,```
2 5
SXXXX
XXXXE
```không kết nối`S`Và`E`, vì chúng không ở cùng hàng hoặc cột. Tổng quát hơn, ô bẩn là chướng ngại vật chứ không phải là đỉnh trung gian của đường hầm. Quá trình quét phải ghi lại các ô sạch và kết nối các ô sạch liên tiếp, thay vì coi mọi phân đoạn bẩn tối đa là có thể đi qua được. 

Cuối cùng, chỉ khoảng cách ngắn nhất thôi là chưa đủ. TRONG```
3 3
S..
...
..E
```nhiều đường dẫn có độ dài bằng bốn, nhưng câu trả lời bắt buộc là`DDRR`. Từ`D < L < R < U`, BFS phải được tổ chức sao cho các đường dẫn được xem xét theo thứ tự từ điển giữa các đường dẫn có độ dài bằng nhau. Chỉ tìm ra con đường ngắn nhất là không đủ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xem mọi ô sạch dưới dạng một đỉnh của đồ thị. Một bước di chuyển bình thường liền kề sẽ tạo ra một cạnh và một đường hầm sẽ tạo ra một cạnh khác. Khi biểu đồ đó được xây dựng, BFS thông thường từ`S`đưa ra số bước di chuyển tối thiểu vì mỗi cạnh biểu thị chính xác một bước di chuyển. 

Cách mạnh mẽ nhất để xây dựng biểu đồ là đứng ở mọi ô sạch và nhìn theo cả bốn hướng cho đến khi đến ô sạch tiếp theo. Ô sạch tiếp theo đó chính xác là điểm cuối đường hầm có thể có theo hướng đó. Cách tiếp cận này đúng vì ô sạch đầu tiên gặp phải là điểm cuối duy nhất có thể có: nếu một ô sạch khác xuất hiện trước nó, thì hai ô ở xa hơn sẽ có một ô sạch ở giữa chúng và sẽ không thỏa mãn điều kiện đường hầm. 

Vấn đề là việc quét lặp đi lặp lại. Một ô có thể quét tới`M`vị trí theo chiều ngang và`N`các vị trí theo chiều dọc. Với bốn triệu ô và kích thước của năm 2000, con số này có thể đạt tới hàng chục tỷ lượt kiểm tra. Bản thân biểu đồ rất thưa thớt, nhưng việc khám phá vũ phu không khai thác được sự thưa thớt đó. 

Quan sát quan trọng là có thể tìm thấy các đường hầm lân cận cho toàn bộ hàng hoặc cột chỉ bằng một lần quét tuyến tính. Trong một hàng, giữ nguyên vị trí của ô sạch gần đây nhất. Bất cứ khi nào gặp một ô sạch khác, cả hai đều là các ô sạch liên tiếp trong hàng đó nên chúng được kết nối bằng một đường hầm. Chúng ta có thể ghi lại kết nối theo cả hai hướng ngay lập tức. Việc lặp lại quy trình tương tự cho mỗi cột sẽ tạo ra tất cả các kết nối đường hầm dọc. 

Điều này có tác dụng vì hai ô sạch có một đường hầm chính xác khi không có ô sạch nào ở giữa chúng. Do đó, trong số tất cả các ô sạch trong một hàng, chỉ có các ô sạch liên tiếp mới cần có cạnh. Tuyên bố tương tự áp dụng cho mỗi cột. 

Sau khi bốn lân cận định hướng đó được xây dựng, BFS sẽ giải quyết vấn đề đường đi ngắn nhất. Để có được đường đi ngắn nhất theo từ điển, các cạnh đi ra của mỗi đỉnh được xử lý theo thứ tự`D`,`L`,`R`,`U`. BFS xử lý các đỉnh theo khoảng cách và trong một lớp khoảng cách, nó xử lý chúng theo thứ tự từ điển của các đường dẫn được sử dụng để tiếp cận chúng. Xử lý các cạnh đi theo thứ tự từ điển, sau đó làm cho đường đi được phát hiện đầu tiên tới mọi đỉnh trở thành đường đi nhỏ nhất trong số tất cả các đường đi ngắn nhất đến đỉnh đó. 

Do đó, mối quan hệ giữa hai cách tiếp cận này rất đơn giản. Giải pháp brute-force đã có biểu đồ phù hợp và BFS phù hợp, nhưng lại lãng phí thời gian để khám phá lại các đường hầm lân cận. Quét hàng và quét cột thay thế tất cả những tìm kiếm lặp lại đó bằng bốn lần quét tuyến tính trên lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(NM(N+M))`|`O(NM)`| Quá chậm | 
| Tối ưu |`O(NM)`|`O(NM)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và xác định vị trí các ô`S`Và`E`. Hãy đối xử với cả hai chúng giống hệt như các ô sạch khi xây dựng các kết nối, vì bài toán rõ ràng cho phép Vitor đứng trên chúng. 
2. Phân bổ bốn mảng đại diện cho ô sạch gần nhất theo mỗi hướng. Đối với một ô ở chỉ mục`v`, các mảng lưu trữ các đường hầm lân cận trái, phải, lên và xuống của nó. Một người hàng xóm vắng mặt được đại diện bởi`-1`. 
3. Quét từng hàng từ trái sang phải. Giữ chỉ mục của ô sạch trước đó. Khi ô hiện tại sạch và tồn tại một ô sạch trước đó, hãy kết nối hai ô đó làm hàng xóm bên trái và bên phải. Sau đó biến ô hiện tại thành ô sạch trước đó. 

Hai ô là các ô sạch liên tiếp trong hàng này nên mọi ô ở giữa đều là ô bẩn. Chúng chính xác là một cặp đường hầm hợp lệ. Không có ô nào khác trong hàng có thể là ô lân cận ngay bên phải của ô trước đó. 
4. Quét từng cột từ trên xuống dưới. Áp dụng ý tưởng ô sạch liên tiếp tương tự theo chiều dọc. Khi tìm thấy hai ô sạch liên tiếp, hãy kết nối chúng như các ô lân cận lên và xuống. 
5. Bắt đầu BFS tại`S`. Xử lý các lân cận của mỗi đỉnh theo thứ tự`D`,`L`,`R`,`U`. Đối với mỗi ô sạch chưa được truy cập, hãy ghi lại ô trước đó và hướng được sử dụng để nhập ô đó, sau đó đưa ô đó vào hàng đợi BFS. 

Thứ tự chỉ quan trọng đối với tie-break. BFS đã đảm bảo khoảng cách tối thiểu, trong khi việc xử lý các hướng theo thứ tự từ điển đảm bảo rằng đường đi ngắn nhất đầu tiên đến một ô là đường đi ngắn nhất về mặt từ điển của nó. 
6. Dừng khi đạt đến BFS`E`hoặc cạn kiệt thành phần có thể truy cập. Nếu như`E`chưa bao giờ được ghé thăm, in`-1`. 
7. Nếu`E`đã đạt được, hãy làm theo gợi ý của người tiền nhiệm từ`E`quay lại`S`. Mỗi hướng được lưu trữ mô tả việc di chuyển từ ô trước đó đến ô hiện tại. Đảo ngược các ký tự đã thu thập để có được lộ trình từ`S`ĐẾN`E`. 

Tại sao nó hoạt động: sau khi quét hàng và cột, mọi đường hầm hợp pháp được biểu thị bằng một cạnh. Chính xác hơn, đối với bất kỳ ô sạch nào, ô sạch đầu tiên ở bên trái, phải, lên hoặc xuống chính xác là ô lân cận đường hầm duy nhất có thể có theo hướng đó. Do đó, đồ thị được xây dựng chứa mọi nước đi hợp pháp và không có nước đi không hợp lệ. Do đó, BFS tính toán số lần di chuyển tối thiểu thực sự. Trong mỗi lớp khoảng cách BFS, các đỉnh được đạt đến theo thứ tự từ điển của tiền tố đường dẫn ngắn nhất của chúng vì cha mẹ được xử lý theo thứ tự đó và hướng đi của chúng được xử lý như`D`,`L`,`R`,`U`. Lần đầu tiên`E`được phát hiện, đường đi của nó do đó vừa ngắn nhất vừa nhỏ nhất về mặt từ điển trong số tất cả các đường đi ngắn nhất. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    total = n * m

    left = array('i', [-1]) * total
    right = array('i', [-1]) * total
    up = array('i', [-1]) * total
    down = array('i', [-1]) * total

    start = -1
    target = -1

    for r in range(n):
        base = r * m
        row = grid[r]

        prev = -1
        for c in range(m):
            ch = row[c]
            if ch == 'X':
                continue

            v = base + c

            if ch == 'S':
                start = v
            elif ch == 'E':
                target = v

            if prev != -1:
                left[v] = prev
                right[prev] = v

            prev = v

    for c in range(m):
        prev = -1

        for r in range(n):
            v = r * m + c

            if grid[r][c] == 'X':
                continue

            if prev != -1:
                up[v] = prev
                down[prev] = v

            prev = v

    parent = array('i', [-1]) * total
    move = bytearray(total)

    queue = array('i')
    queue.append(start)
    parent[start] = start

    head = 0

    while head < len(queue):
        v = queue[head]
        head += 1

        if v == target:
            break

        r = v // m
        c = v - r * m

        # Lexicographic order: D < L < R < U.
        if r + 1 < n:
            to = down[v]
            if to != -1 and parent[to] == -1:
                parent[to] = v
                move[to] = ord('D')
                queue.append(to)

        to = left[v]
        if to != -1 and parent[to] == -1:
            parent[to] = v
            move[to] = ord('L')
            queue.append(to)

        to = right[v]
        if to != -1 and parent[to] == -1:
            parent[to] = v
            move[to] = ord('R')
            queue.append(to)

        if r > 0:
            to = up[v]
            if to != -1 and parent[to] == -1:
                parent[to] = v
                move[to] = ord('U')
                queue.append(to)

    if parent[target] == -1:
        print(-1)
        return

    path = bytearray()
    cur = target

    while cur != start:
        path.append(move[cur])
        cur = parent[cur]

    path.reverse()

    print(len(path))
    print(path.decode())

if __name__ == "__main__":
    solve()
```Bốn`array('i')`các đối tượng lưu trữ các chỉ số ô số nguyên bằng cách sử dụng số nguyên có dấu 32 bit. Điều này phù hợp hơn với danh sách Python ở đây vì bốn triệu số nguyên Python sẽ tiêu thụ nhiều bộ nhớ hơn đáng kể so với cách biểu diễn được đóng gói của chúng. 

Quét hàng thực hiện phần nằm ngang của việc xây dựng biểu đồ.`prev`là ô sạch gần đây nhất. Khi một ô sạch mới được tìm thấy, nó sẽ trở thành hàng xóm bên phải của`prev`, trong khi`prev`trở thành hàng xóm bên trái của ô mới. Bởi vì không thể có một ô sạch nào khác giữa chúng, điều này hoàn toàn khớp với định nghĩa đường hầm. 

Việc quét cột thực hiện tương tự theo chiều dọc. Dữ liệu đầu vào được lưu theo từng hàng, do đó chỉ mục của ô`(r, c)`là`r * m + c`. Di chuyển xuống thêm`m`, trong khi di chuyển lên trừ`m`. Việc triển khai sử dụng tính toán trước`up`Và`down`mảng, do đó việc kiểm tra ranh giới chủ yếu mang tính chất phòng thủ. Một người hàng xóm mất tích đã được đại diện bởi`-1`. 

BFS sử dụng`parent[start] = start`làm điểm đánh dấu đã truy cập cho ô bắt đầu. Mỗi ô chưa được thăm khác đều có ô cha`-1`. Điều này tránh việc duy trì một mảng truy cập boolean riêng biệt. 

Hàng đợi cũng là một`array('i')`. các`head`chỉ mục di chuyển về phía trước thay vì liên tục loại bỏ phần tử đầu tiên, điều này tránh được`O(n)`chi phí của`pop(0)`. 

Lệnh hàng xóm là cố tình`D`,`L`,`R`,`U`. Thứ tự ASCII không được dựa vào một cách ngầm định. Mã này tuân theo thứ tự từ điển được yêu cầu một cách rõ ràng. Khi một ô được phát hiện lần đầu tiên, ô trước đó và hướng đi đến của nó sẽ được lưu trữ vĩnh viễn. Đường dẫn sau đến cùng một ô không thể nhỏ hơn về mặt từ điển trong khi có cùng khoảng cách, bởi vì BFS tiếp cận cha mẹ theo thứ tự từ điển và các cạnh của chúng được khám phá theo thứ tự từ điển. 

Không có hiện tượng tràn số nguyên trong quá trình triển khai Python. Chỉ số ô lớn nhất là`N*M-1`, nhiều nhất là`3,999,999`, thoải mái bên trong số nguyên 32 bit có dấu. 

Quá trình tái thiết đi lùi từ`E`ĐẾN`S`. Vì mọi hướng được lưu trữ đều mô tả cạnh thuận được sử dụng trong BFS nên chuỗi được thu thập ban đầu sẽ bị đảo ngược. Đang gọi`reverse()`khôi phục lại tuyến đường thực tế. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Lưới là```
3 3
S..
...
..E
```Không có ô bẩn nên đường hầm không tạo ra bất kỳ bước nhảy dài nào. Đồ thị là lưới bốn lân cận thông thường. 

| Ô hiện tại | Hướng đã thử | Ô tiếp theo | Hành động | 
| --- | --- | --- | --- | 
|`(0,0)`|`D`|`(1,0)`| khám phá | 
|`(0,0)`|`L`| không | bỏ qua | 
|`(0,0)`|`R`|`(0,1)`| khám phá | 
|`(0,0)`|`U`| không | bỏ qua | 
|`(1,0)`|`D`|`(2,0)`| khám phá | 
|`(1,0)`|`L`| không | bỏ qua | 
|`(1,0)`|`R`|`(1,1)`| khám phá | 
|`(1,0)`|`U`|`(0,0)`| đã ghé thăm | 
|`(2,0)`|`D`| không | bỏ qua | 
|`(2,0)`|`L`| không | bỏ qua | 
|`(2,0)`|`R`|`(2,1)`| khám phá | 
|`(2,0)`|`U`|`(1,0)`| đã ghé thăm | 
|`(2,1)`|`D`| không | bỏ qua | 
|`(2,1)`|`L`|`(2,0)`| đã ghé thăm | 
|`(2,1)`|`R`|`(2,2)`| phát hiện`E`| 

Chuỗi tiền thân kết quả là```
S -> D -> D -> R -> R -> E
```vậy câu trả lời là```
4
DDRR
```Có một số đường dẫn khác có độ dài bằng bốn. Thứ tự BFS chọn`DDRR`bởi vì`D`về mặt từ điển nhỏ hơn`R`và sự so sánh tương tự được áp dụng ở mọi điểm phân nhánh. 

### Mẫu 2 

Lưới là```
3 3
SX.
XXX
XXE
```Hàng đầu tiên có hai ô sạch,`S`và ô ở cột hai. Ba ô giữa`S`và ô sạch tiếp theo không tồn tại, do đó ô tiếp theo có thể đến được bằng một bước di chuyển liền kề thông thường. Quan trọng hơn, ô cuối cùng ở hàng đầu tiên và`S`được phân tách bằng chính xác một ô bẩn, do đó quá trình quét ngang kết nối chúng trực tiếp. 

Cột thứ ba chứa một ô sạch ở trên cùng và`E`ở phía dưới, với một ô bẩn ở giữa chúng. Quét dọc kết nối hai ô sạch đó. 

| Ô hiện tại | Hướng | Hàng xóm | Kết quả | 
| --- | --- | --- | --- | 
|`S = (0,0)`|`D`| không | bỏ qua | 
|`S = (0,0)`|`L`| không | bỏ qua | 
|`S = (0,0)`|`R`|`(0,2)`| khám phá | 
|`S = (0,0)`|`U`| không | bỏ qua | 
|`(0,2)`|`D`|`E = (2,2)`| khám phá | 
|`(0,2)`|`L`|`S`| đã ghé thăm | 
|`(0,2)`|`R`| không | bỏ qua | 
|`(0,2)`|`U`| không | bỏ qua | 

Con đường chỉ có hai bước đi:```
RD
```Ví dụ này giải thích tại sao các cạnh của đường hầm phải được thêm vào ngay cả khi điểm cuối của chúng không liền kề trong lưới ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(NM)`| Mỗi ô lưới được kiểm tra một lần trong quá trình quét hàng, một lần trong quá trình quét cột và nhiều nhất một lần trong BFS. | 
| Không gian |`O(NM)`| Bốn mảng lân cận, mảng cha BFS, hàng đợi, mảng di chuyển và lưới đều chia tỷ lệ tuyến tính theo số lượng ô. | 

Vì`N, M <= 2000`, có nhiều nhất là bốn triệu tế bào. Thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi ô, thay vì quét liên tục các hàng và cột dài. Các mảng số nguyên được đóng gói giúp duy trì mức sử dụng bộ nhớ thoải mái dưới giới hạn 512 MB. Giới hạn tiệm cận cũng là thang đo phù hợp cho giới hạn thời gian hai giây. 

## Trường hợp thử nghiệm 

Khai thác kiểm tra sau đây chứa cùng một thuật toán như một hàm có thể gọi được để các xác nhận có thể thực thi độc lập.```python
import sys
import io
from array import array

def solve_input(inp: str) -> str:
    data = inp.splitlines()
    n, m = map(int, data[0].split())
    grid = data[1:1 + n]

    total = n * m

    left = array('i', [-1]) * total
    right = array('i', [-1]) * total
    up = array('i', [-1]) * total
    down = array('i', [-1]) * total

    start = -1
    target = -1

    for r in range(n):
        base = r * m
        prev = -1

        for c in range(m):
            ch = grid[r][c]

            if ch == 'X':
                continue

            v = base + c

            if ch == 'S':
                start = v
            elif ch == 'E':
                target = v

            if prev != -1:
                left[v] = prev
                right[prev] = v

            prev = v

    for c in range(m):
        prev = -1

        for r in range(n):
            if grid[r][c] == 'X':
                continue

            v = r * m + c

            if prev != -1:
                up[v] = prev
                down[prev] = v

            prev = v

    parent = array('i', [-1]) * total
    move = bytearray(total)

    queue = array('i')
    queue.append(start)
    parent[start] = start

    head = 0

    while head < len(queue):
        v = queue[head]
        head += 1

        if v == target:
            break

        r = v // m

        to = down[v]
        if to != -1 and parent[to] == -1:
            parent[to] = v
            move[to] = ord('D')
            queue.append(to)

        to = left[v]
        if to != -1 and parent[to] == -1:
            parent[to] = v
            move[to] = ord('L')
            queue.append(to)

        to = right[v]
        if to != -1 and parent[to] == -1:
            parent[to] = v
            move[to] = ord('R')
            queue.append(to)

        to = up[v]
        if to != -1 and parent[to] == -1:
            parent[to] = v
            move[to] = ord('U')
            queue.append(to)

    if parent[target] == -1:
        return "-1\n"

    path = bytearray()
    cur = target

    while cur != start:
        path.append(move[cur])
        cur = parent[cur]

    path.reverse()

    return f"{len(path)}\n{path.decode()}\n"

# Provided sample 1.
assert solve_input(
    """3 3
S..
...
..E
"""
) == "4\nDDRR\n"

# Provided sample 2.
assert solve_input(
    """3 3
SX.
XXX
XXE
"""
) == "2\nRD\n"

# Provided sample 3.
assert solve_input(
    """2 2
SX
XE
"""
) == "-1\n"

# Custom case 1: a horizontal tunnel spanning three dirty cells.
assert solve_input(
    """2 5
SXXXE
.....
"""
) == "1\nR\n"

# Custom case 2: a vertical tunnel spanning three dirty cells.
assert solve_input(
    """5 2
SX
XX
XX
XX
EX
"""
) == "1\nD\n"

# Custom case 3: shortest path with lexicographic tie.
# The open 3x3 grid has many shortest paths. DDRR is the smallest.
assert solve_input(
    """3 3
S..
...
..E
"""
) == "4\nDDRR\n"

# Custom case 4: maximum-size grid, all cells dirty except S and E.
# There is no row or column containing both endpoints, so E is unreachable.
n = 2000
m = 2000
rows = ["X" * m for _ in range(n)]
rows[0] = "S" + "X" * (m - 1)
rows[-1] = "X" * (m - 1) + "E"

large_input = f"{n} {m}\n" + "\n".join(rows) + "\n"
assert solve_input(large_input) == "-1\n"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 × 5`,`SXXXE`trong một hàng |`1 / R`| Một đường hầm có thể bỏ qua một số ô bẩn tùy ý. | 
|`5 × 2`,`S`bên trên`E`với các ô bẩn giữa |`1 / D`| Xây dựng đường hầm dọc và xử lý ranh giới. | 
| Mở`3 × 3`lưới |`4 / DDRR`| Nhỏ nhất về mặt từ điển trong số một số con đường ngắn nhất. | 
|`2000 × 2000`, chỉ một`S`Và`E`sạch sẽ |`-1`| Kích thước lưới tối đa và đích đến không thể truy cập được mà không vô tình kết nối các đường chéo. | 

## Vỏ cạnh 

các`2 × 2`trường hợp không thể truy cập được```
2 2
SX
XE
```chỉ chứa hai ô sạch,`S`Và`E`, và chúng là đường chéo. Quét hàng không thấy ô sạch thứ hai ở hàng đầu tiên, trong khi quét cột không thấy ô sạch thứ hai trong cột đầu tiên. Điều này cũng đúng đối với điểm đến. Do đó, BFS không có lợi thế đi ra từ`S`, Vì thế`parent[E]`còn lại`-1`và thuật toán in`-1`. 

Đối với một đường hầm nằm ngang dài,```
2 5
SXXXE
.....
```hàng quét bản ghi đầu tiên`S`như ô sạch trước đó. Nó bỏ qua ba`X`tế bào, sau đó gặp`E`và kết nối hai điểm cuối. BFS thấy`E`như`R`hàng xóm của`S`, vì vậy khoảng cách ngắn nhất là một và đầu ra là`1`theo sau là`R`. Các ô bẩn không bao giờ được chèn vào biểu đồ. 

Đối với một đường hầm thẳng đứng dài,```
5 2
SX
XX
XX
XX
EX
```cột đầu tiên chứa`S`, ba ô bẩn, và`E`. Trong quá trình quét cột,`S`trở thành`prev`, các ô bẩn bị bỏ qua, và`E`cuối cùng đã được kết nối với`S`như hàng xóm của nó. BFS đạt được nó trong một`D`di chuyển. Điều này mắc phải sai lầm phổ biến là chỉ coi các ô sạch liền kề là có thể di chuyển được. 

Trường hợp từ điển học```
3 3
S..
...
..E
```có một số tuyến đường ngắn nhất có độ dài bốn. BFS đầu tiên đến ô phía dưới bên trái thông qua`D`, thay vì ô phía trên bên phải thông qua`R`, bởi vì`D`xuất hiện đầu tiên về mặt từ điển. Từ đó nó tiếp tục xử lý`D`trước`R`, sản xuất`DDRR`. Một BFS sử dụng thứ tự lân cận tùy ý vẫn có thể tìm thấy đường đi ngắn nhất, nhưng nó có thể quay trở lại`DRDR`,`DRRD`hoặc một tuyến đường dài bốn hợp lệ khác sẽ vi phạm yêu cầu đầu ra. 

Hộp có kích thước tối đa chứa bốn triệu tế bào nhưng hầu hết tất cả đều bị bẩn. Việc quét hàng và quét vẫn chỉ chạm vào từng ô một số lần không đổi. Chỉ các chuyến thăm BFS`S`bởi vì không có cạnh pháp lý từ nó. Các mảng được đóng gói giữ cho bộ nhớ tỷ lệ thuận với số lượng ô, do đó kích thước trong trường hợp xấu nhất không làm thay đổi cách tiếp cận thuật toán.
