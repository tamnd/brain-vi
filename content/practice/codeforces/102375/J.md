---
title: "CF 102375J - \u041f\u043e\u0440\u0442\u0430\u043b\u044b"
description: "Mê cung là một lưới (N lần M). Ô là không gian trống thông thường, tường kính hoặc tường kiên cố. Chuyển động thông thường chỉ có thể thực hiện được giữa các ô tự do liền kề. Kính chặn chuyển động, nhưng một cú bắn vào cổng có thể xuyên qua nó. Biên giới bên ngoài bao gồm hoàn toàn các bức tường vững chắc."
date: "2026-08-14T13:19:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "J"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 866
verified: false
draft: false
---

[CF 102375J - \u041f\u043e\u0440\u0442\u0430\u043b\u044b](https://codeforces.com/problemset/problem/102375/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14 phút 26 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mê cung là một lưới (N \times M). Ô là không gian trống thông thường, tường kính hoặc tường kiên cố. Chuyển động thông thường chỉ có thể thực hiện được giữa các ô tự do liền kề. Kính chặn chuyển động, nhưng một cú bắn vào cổng có thể xuyên qua nó. Biên giới bên ngoài bao gồm hoàn toàn các bức tường vững chắc. 

Một cổng được gắn vào một bên của một ô có vách cứng. Có hai màu, cam và xanh lam, và mỗi lần chỉ có thể tồn tại một cổng mỗi màu. Chụp thay thế cổng cũ có màu đó. Khi bắn theo một hướng nào đó, tia sáng sẽ tiếp tục cho đến khi chạm tới bức tường vững chắc gần nhất. Tế bào thủy tinh không ngăn được tia. Cánh cổng được đặt ở phía bức tường đối diện với người bắn. 

Một cổng chỉ hữu ích khi cạnh của nó liền kề với một ô trống. Nếu mặt tương ứng là kính, việc ra vào cổng sẽ là không thể hoặc gây tử vong, vì vậy cổng như vậy không thể tham gia vào lộ trình hợp lệ. 

Phần thú vị là hai vị trí cổng đều ổn định. Sau khi sử dụng một cổng, cổng còn lại vẫn giữ nguyên vị trí cũ. Điều này cho phép chúng tôi thay thế một màu bằng một cổng đích mới bằng một lần chụp bổ sung, sau đó nhập màu còn lại và dịch chuyển đến đích mới. 

Đầu ra không yêu cầu con đường ngắn nhất trong chuyển động thông thường. Mục tiêu chính là giảm thiểu số lần bắn. Trong số tất cả các giải pháp có số lần bắn tối thiểu đó, bất kỳ tuyến đường nào có số bước di chuyển tối đa (2NM) đều được chấp nhận. Sự cố cho phép (N,M\le1000), do đó có thể có tối đa (10^6) ô. Một thuật toán có sự phụ thuộc bậc hai hoặc bậc ba vào số lượng ô đã quá lớn, trong khi giải pháp (O(NM)) là mục tiêu tự nhiên. Giới hạn cuộc thi ban đầu là 2 giây và 512 MiB. 

Trường hợp cạnh đầu tiên là khi bắt đầu và thoát trong cùng một thành phần được kết nối thông thường. Ví dụ,```
3 3
WWW
W.W
WWW
2 2
2 2
```Đầu ra đúng là```
0 0
```Không có hành động nào cả. Một giải pháp luôn xây dựng hai cổng trước khi xem xét chuyển động thông thường sẽ sử dụng súng một cách không cần thiết. 

Trường hợp cạnh thứ hai là một ô kính ngay trước một bức tường vững chắc. Coi như```
5 5
WWWWW
W.GWW
WWWWW
W...W
WWWWW
2 2
4 2
```Điểm bắt đầu và kết thúc nằm ở các thành phần khác nhau. Bắn ngay từ đầu sẽ chạm tới bức tường vững chắc sau ô kính, nhưng cổng được đặt cạnh ô kính nên không thể sử dụng an toàn. Bắn theo các hướng khác chỉ chạm tới những bức tường vững chắc ngay xung quanh bộ phận bắt đầu. Đầu ra đúng là`-1 -1`. Việc triển khai bất cẩn coi mọi bức tường nhìn thấy qua kính là đích đến có thể sử dụng được sẽ cho rằng hai thành phần có thể được kết nối một cách không chính xác. 

Trường hợp cạnh thứ ba là một cổng nhắm vào cùng một thành phần thông thường. Trong một nội thất hoàn toàn mở, một cú bắn về phía bức tường ranh giới có thể tạo ra một cánh cổng, nhưng cả hai bên liên quan đến tuyến đường đều đã được kết nối bằng chuyển động thông thường. Một cổng thông tin như vậy không được tính là tiến bộ. Việc coi mọi cú đánh có thể là một cạnh của đồ thị sẽ tạo ra nhiều vòng tự lặp vô ích. 

Trường hợp cạnh thứ tư là một thành phần có thể chứa các ô tự do nhưng không có ô tự do nào tiếp giáp trực tiếp với một bức tường vững chắc. Một thành phần như vậy đôi khi có thể bắn xuyên qua kính và tạo ra một cổng ở một nơi khác, nhưng nó không thể đặt một cổng nguồn có thể sử dụng được cho chính nó. Do đó, nó không thể được sử dụng làm mặt hiện tại của dịch chuyển tức thời. Sự khác biệt này rất quan trọng khi quyết định cạnh nào của đồ thị thực sự có thể đi qua được. 

## Phương pháp tiếp cận 

Mô hình lực lượng vũ phu trực tiếp bắt đầu từ trạng thái vật lý đầy đủ. Trạng thái phải chứa ô trống hiện tại và vị trí của cả hai cổng màu. Có thể có (O(NM)) mặt tường phù hợp cho một cổng, do đó, một cặp cổng đã cung cấp cấu hình (O((NM)^2)). Nhân với vị trí hiện tại sẽ cho ra (O((NM)^3)) trạng thái có thể. Với (NM=10^6), đây là thứ tự của (10^{18}) trạng thái trong trường hợp xấu nhất. Ngay cả việc lưu trữ một byte cho mỗi trạng thái cũng là không thể, vì vậy việc tìm kiếm trong không gian trạng thái đầy đủ không phải là một phương pháp khả thi. 

Quan sát quan trọng là chuyển động thông thường loại bỏ hoàn toàn nhu cầu phân biệt từng ô riêng lẻ khi đếm số lần bắn. Bên trong một thành phần được kết nối của các ô tự do, chúng ta có thể di chuyển đến bất kỳ ô nào khác mà không cần nổ súng. Quá trình chuyển đổi có ý nghĩa duy nhất là dịch chuyển tức thời từ thành phần không gian trống này sang thành phần không gian trống khác. 

Hãy xem xét một chuỗi các ô nằm ngang hoặc dọc không có vách cứng. Giả sử điểm cuối bên phải của nó được theo sau bởi một bức tường vững chắc và ô ngay trước bức tường đó là tự do. Mọi ô tự do trong chuỗi đều có thể bắn về phía tường. Cổng kết quả được đặt trên bức tường liền kề với ô tự do cuối cùng đó. Các ô kính giữa người bắn và bức tường không thành vấn đề. Do đó, mọi thành phần tự do được biểu thị trong chuỗi đó có thể tạo một cổng có điểm cuối có thể sử dụng được thuộc về thành phần chứa ô tự do cuối cùng. 

Điều này đưa ra một đồ thị có hướng có các đỉnh là các thành phần được kết nối của các ô tự do. Có một cạnh (A\to B) khi một ô trong thành phần (A) có thể bắn về phía một bức tường vững chắc và mặt của bức tường đó đối diện với người bắn liền kề với một ô tự do trong thành phần (B). Thành phần (B) sau đó có thể là đích đến cho một cổng thông tin mới được tạo. 

Hướng này rất quan trọng. Nếu (A) có thể thấy một cổng thông tin có thể sử dụng được thuộc về (B), thì từ (A) chúng ta có thể tạo cổng đích đó. Việc chuyển đổi ngược lại có thể không thực hiện được với một lần chụp mới. 

Màu sắc của cổng giải thích tại sao biểu đồ này là đủ. Lần dịch chuyển đầu tiên cần hai lần chụp vì ban đầu cả hai màu đều không có cổng. Đặt một màu ở bất kỳ mặt tường nào có thể sử dụng được trong thành phần hiện tại và màu còn lại ở đích được mô tả bởi cạnh biểu đồ. Sau khi dịch chuyển tức thời, một cổng nằm trong thành phần hiện tại và cổng còn lại nằm trong thành phần mới. Để tiếp cận thành phần khác, chỉ cần một lần bắn. Thay thế cổng vẫn ở phía sau bạn bằng một cổng mới ở điểm đến tiếp theo, sau đó nhập cổng vẫn còn trong thành phần hiện tại của bạn. 

Do đó, tuyến đường sử dụng các cạnh đồ thị (K) yêu cầu chính xác (K+1) số lần chụp khi (K>0). Một tuyến đường không có cạnh nào không cần phải chụp. Do đó, việc giảm thiểu số lần chụp cũng giống như tìm đường đi có hướng ngắn nhất trong biểu đồ thành phần.

Chúng ta không cần phải tìm kiếm rõ ràng tất cả các tia có thể. Một đoạn ngang giữa hai bức tường vững chắc có thể được xử lý bằng hai lần quét, một lần cho mỗi hướng chụp. Một đoạn dọc được xử lý theo cách tương tự. Mỗi ô tự do tham gia vào một số phép toán không đổi, do đó đồ thị hoàn chỉnh có thể được xây dựng theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tìm kiếm toàn bộ không gian trạng thái | (O((NM)^3)) trạng thái | (O((NM)^3)) | Quá chậm | 
| Đồ thị thành phần tối ưu | (O(NM)) | (O(NM)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Dán nhãn cho mọi thành phần được kết nối của`.`các tế bào sử dụng BFS. Hai ô tự do thuộc về cùng một thành phần khi chúng có thể được kết nối bằng chuyển động thông thường mà không đi qua`G`hoặc`W`. 

Trong khi khám phá một thành phần, hãy nhớ một ô trống liền kề với một bức tường vững chắc và hướng của bức tường đó. Một ô như vậy cung cấp cho chúng ta một vị trí hợp lệ cho cổng nguồn bất cứ khi nào thành phần đó cần thực hiện dịch chuyển tức thời. 
2. Xây dựng biểu đồ thành phần có hướng bằng cách quét từng hàng. 

Đối với một hàng cố định, hãy xem khoảng tối đa không chứa`W`. Nếu ô ngay trước bức tường vững chắc bên phải là ô trống thì mọi ô trống trong khoảng đó có thể bắn sang phải và tạo ra một cổng có thể sử dụng được thuộc thành phần của ô tự do cuối cùng đó. Thêm một lợi thế từ thành phần của mỗi game bắn súng vào thành phần mục tiêu đó. 

Lặp lại quá trình quét tương tự từ trái sang phải để thu được các cạnh hướng trái. 
3. Thực hiện hai lần quét tương tự cho mỗi cột. 

Việc quét xuống sẽ xử lý các cổng có bức tường bên dưới người bắn. Việc quét lên trên xử lý các cổng có bức tường phía trên người bắn. 

Một tế bào thủy tinh không bao giờ ngừng quét. Nó chỉ quan trọng liệu ô liền kề với bức tường vững chắc có còn trống hay không. Nếu ô đó là`G`, cổng sẽ có một bức tường kính ở phía có thể sử dụng được và không được thêm vào làm cạnh. 
4. Bỏ qua các cạnh của đồ thị có thành phần nguồn và đích bằng nhau. 

Chuyển động thông thường đã kết nối mọi cặp ô trong một thành phần như vậy, vì vậy một cổng không thể cải thiện số lần bắn. 
5. Chạy BFS trên biểu đồ thành phần được định hướng từ thành phần bắt đầu. 

Mỗi cạnh đồ thị đại diện cho một dịch chuyển tức thời đến một thành phần mới. Tất cả các cạnh đều có chi phí bằng nhau, vì vậy BFS thông thường đưa ra số lần dịch chuyển tối thiểu. Lưu trữ thành phần trước đó cũng như ô và hướng chụp chính xác cho mọi cạnh được phát hiện. Sau này cần có những nhân chứng đó để tái hiện lại các hành động thực tế. 
6. Nếu thành phần thoát không thể truy cập được, hãy in`-1 -1`. 

Biểu đồ thành phần mô tả mọi cách có thể để giới thiệu đích cổng thông tin mới có thể sử dụng được. Nếu không thể tiếp cận thành phần thoát trong biểu đồ này thì không có chuỗi ảnh chụp cổng nào có thể truy cập được. 
7. Nếu phần bắt đầu và kết thúc nằm trong cùng một thành phần, hãy tạo lại một đường dẫn thông thường giữa các ô của chúng và xuất ra nó với số lần chụp bằng 0. 
8. Mặt khác, hãy xây dựng lại trình tự các thành phần được BFS tìm thấy. 

Giả sử cạnh đồ thị đầu tiên sử dụng ô bắn (u), hướng (d) và đến ô (v). Chọn ô liền kề với tường đã nhớ (q) của thành phần bắt đầu làm vị trí cổng đầu tiên. Đi từ đầu đến (q), đặt cổng màu xanh ở đó, đi đến (u), đặt cổng màu cam theo hướng (d), quay lại (q) và di chuyển vào cổng màu xanh. Dịch chuyển tức thời đưa chúng ta đến (v). 

Hai phát bắn đã được sử dụng, đây chính xác là số tiền cần thiết cho lần dịch chuyển đầu tiên. 
9. Đối với mỗi cạnh đồ thị sau này, đã có một cổng trong thành phần hiện tại. Đi bộ từ ô đến hiện tại đến nhân chứng bắn súng (u), bắn màu khác về phía điểm đến tiếp theo, quay lại cổng cũ và vào đó. 

Cổng thông tin mới hiện nằm trong thành phần tiếp theo. Điều này tốn chính xác một lần. 
10. Sau khi đến phần thoát, đi bộ bình thường từ ô đến đến lối ra. 

Đối với mọi thành phần, chúng tôi sử dụng đường dẫn BFS đơn giản bên trong thành phần đó. Mỗi thành phần xảy ra nhiều nhất một lần trên đường đi thành phần ngắn nhất, do đó tổng số lần đi bộ thông thường vẫn nằm trong giới hạn bắt buộc (2NM). 

### Tại sao nó hoạt động 

Điều bất biến là ngay trước mỗi lần chuyển đổi đồ thị, một cổng được đặt trên một bức tường có thể sử dụng được trong thành phần hiện tại. Cổng còn lại không liên quan hoặc đang được thay thế bằng lần chụp tiếp theo. Cạnh biểu đồ được lưu trữ cho chúng ta biết chính xác vị trí cần chụp để cổng mới được đặt trên một bức tường có thể sử dụng được trong thành phần tiếp theo. Sau đó, chúng tôi quay lại cổng hiện có và vượt qua nó để đến thành phần mới. 

Mỗi lần dịch chuyển đầu tiên cần hai lần chụp vì ban đầu cả hai màu đều không có. Mỗi lần dịch chuyển sau đó đến một thành phần mới tiếp cận cần ít nhất một lần bắn mới, vì nếu không thay đổi vị trí cổng, cặp điểm đến sẽ không thể có được thành phần mới. Ngược lại, việc xây dựng sử dụng chính xác một ảnh mới cho mỗi cạnh đồ thị tiếp theo. Do đó, đường đi đồ thị ngắn nhất có cạnh (K) sẽ cho số lần chụp (K+1) tối thiểu có thể. 

Biểu đồ chứa chính xác các chuyển đổi dịch chuyển tức thời hữu ích. Quá trình chuyển đổi có thể được tạo chính xác khi mặt tường đích được nhìn thấy qua các ô tự do hoặc thủy tinh và ô ngay lập tức của nó là tự do. Bốn lần quét liệt kê chính xác những tình huống đó. Do đó BFS trên biểu đồ này tìm thấy số lượng đích đến cổng thông tin mới tối thiểu có thể, đây là mức tối ưu cần thiết. 

## Giải pháp Python```python
import sys
from collections import deque
from array import array

input = sys.stdin.readline

# Direction codes:
# 0 = U, 1 = D, 2 = L, 3 = R
DR = (-1, 1, 0, 0)
DC = (0, 0, -1, 1)
DIR_CHARS = b"UDLR"
OPPOSITE = (1, 0, 3, 2)

def solve():
    n, m = map(int, input().split())
    g = [input().strip() for _ in range(n)]

    sr, sc = map(int, input().split())
    er, ec = map(int, input().split())
    sr -= 1
    sc -= 1
    er -= 1
    ec -= 1

    total = n * m
    start = sr * m + sc
    finish = er * m + ec

    # Component id of every cell, -1 for walls and glass.
    comp = array('i', [-1]) * total

    # One usable portal position for each component.
    portal_cell = array('i')
    portal_dir = bytearray()

    component_count = 0
    q = deque()

    for r in range(1, n - 1):
        row = g[r]
        for c in range(1, m - 1):
            pos = r * m + c
            if row[c] != '.' or comp[pos] != -1:
                continue

            cid = component_count
            component_count += 1
            portal_cell.append(-1)
            portal_dir.append(0)

            comp[pos] = cid
            q.clear()
            q.append(pos)

            while q:
                p = q.popleft()
                pr = p // m
                pc = p - pr * m

                # Find one free cell with a solid wall next to it.
                if portal_cell[cid] == -1:
                    if g[pr - 1][pc] == 'W':
                        portal_cell[cid] = p
                        portal_dir[cid] = 0
                    elif g[pr + 1][pc] == 'W':
                        portal_cell[cid] = p
                        portal_dir[cid] = 1
                    elif g[pr][pc - 1] == 'W':
                        portal_cell[cid] = p
                        portal_dir[cid] = 2
                    elif g[pr][pc + 1] == 'W':
                        portal_cell[cid] = p
                        portal_dir[cid] = 3

                np = p - m
                if g[pr - 1][pc] == '.' and comp[np] == -1:
                    comp[np] = cid
                    q.append(np)

                np = p + m
                if g[pr + 1][pc] == '.' and comp[np] == -1:
                    comp[np] = cid
                    q.append(np)

                np = p - 1
                if g[pr][pc - 1] == '.' and comp[np] == -1:
                    comp[np] = cid
                    q.append(np)

                np = p + 1
                if g[pr][pc + 1] == '.' and comp[np] == -1:
                    comp[np] = cid
                    q.append(np)

    start_comp = comp[start]
    finish_comp = comp[finish]

    # If ordinary movement is already enough, construct that path later.
    # Otherwise build the component graph.
    head = array('i', [-1]) * component_count
    to = array('i')
    nxt = array('i')
    witness = array('i')
    target_cell = array('i')
    edge_dir = bytearray()

    def add_edge(a, b, u, v, d):
        if a == b:
            return
        idx = len(to)
        to.append(b)
        witness.append(u)
        target_cell.append(v)
        edge_dir.append(d)
        nxt.append(head[a])
        head[a] = idx

    # Horizontal edges: shooting right.
    for r in range(1, n - 1):
        target = -1
        base = r * m
        for c in range(m - 1, 0, -1):
            ch = g[r][c]
            if ch == 'W':
                if g[r][c - 1] == '.':
                    target = base + c - 1
                else:
                    target = -1
            elif ch == '.' and target != -1:
                u = base + c
                add_edge(comp[u], comp[target], u, target, 3)

    # Horizontal edges: shooting left.
    for r in range(1, n - 1):
        target = -1
        base = r * m
        for c in range(0, m - 1):
            ch = g[r][c]
            if ch == 'W':
                if g[r][c + 1] == '.':
                    target = base + c + 1
                else:
                    target = -1
            elif ch == '.' and target != -1:
                u = base + c
                add_edge(comp[u], comp[target], u, target, 2)

    # Vertical edges: shooting down.
    for c in range(1, m - 1):
        target = -1
        for r in range(n - 1, 0, -1):
            ch = g[r][c]
            if ch == 'W':
                if g[r - 1][c] == '.':
                    target = (r - 1) * m + c
                else:
                    target = -1
            elif ch == '.':
                if target != -1:
                    u = r * m + c
                    add_edge(comp[u], comp[target], u, target, 1)

    # Vertical edges: shooting up.
    for c in range(1, m - 1):
        target = -1
        for r in range(0, n - 1):
            ch = g[r][c]
            if ch == 'W':
                if g[r + 1][c] == '.':
                    target = (r + 1) * m + c
                else:
                    target = -1
            elif ch == '.' and target != -1:
                u = r * m + c
                add_edge(comp[u], comp[target], u, target, 0)

    # BFS on the component graph.
    parent_comp = array('i', [-1]) * component_count
    parent_edge = array('i', [-1]) * component_count

    parent_comp[start_comp] = start_comp
    cq = deque([start_comp])

    while cq:
        a = cq.popleft()

        if a == finish_comp:
            break

        # A component without a free cell directly adjacent to W
        # cannot serve as the source of a usable portal.
        if portal_cell[a] == -1:
            continue

        e = head[a]
        while e != -1:
            b = to[e]
            if parent_comp[b] == -1:
                parent_comp[b] = a
                parent_edge[b] = e
                cq.append(b)
            e = nxt[e]

    if parent_comp[finish_comp] == -1:
        print("-1 -1")
        return

    # Temporary arrays for paths inside ordinary components.
    cell_parent = array('i', [-1]) * total

    def get_path(a, b, cid):
        """Return direction codes of a shortest ordinary path a -> b."""
        if a == b:
            return []

        bfsq = [a]
        visited = [a]
        cell_parent[a] = a
        qi = 0

        while qi < len(bfsq):
            p = bfsq[qi]
            qi += 1

            if p == b:
                break

            r = p // m
            c = p - r * m

            np = p - m
            if comp[np] == cid and cell_parent[np] == -1:
                cell_parent[np] = p
                bfsq.append(np)
                visited.append(np)

            np = p + m
            if comp[np] == cid and cell_parent[np] == -1:
                cell_parent[np] = p
                bfsq.append(np)
                visited.append(np)

            np = p - 1
            if comp[np] == cid and cell_parent[np] == -1:
                cell_parent[np] = p
                bfsq.append(np)
                visited.append(np)

            np = p + 1
            if comp[np] == cid and cell_parent[np] == -1:
                cell_parent[np] = p
                bfsq.append(np)
                visited.append(np)

        path = []
        cur = b
        while cur != a:
            p = cell_parent[cur]
            delta = cur - p
            if delta == -m:
                path.append(0)
            elif delta == m:
                path.append(1)
            elif delta == -1:
                path.append(2)
            else:
                path.append(3)
            cur = p

        path.reverse()

        for v in visited:
            cell_parent[v] = -1

        return path

    # Reconstruct component path and corresponding graph edges.
    components = []
    edges = []

    cur = finish_comp
    while cur != start_comp:
        components.append(cur)
        e = parent_edge[cur]
        edges.append(e)
        cur = parent_comp[cur]

    components.append(start_comp)
    components.reverse()
    edges.reverse()

    actions = bytearray()
    shots = 0
    steps = 0

    def add_move(d):
        nonlocal steps
        actions.extend((77, DIR_CHARS[d], 10))
        steps += 1

    def add_shot(color, d):
        nonlocal shots
        actions.extend((color, DIR_CHARS[d], 10))
        shots += 1

    if not edges:
        path = get_path(start, finish, start_comp)
        for d in path:
            add_move(d)

        out = bytearray()
        out.extend(f"{shots} {steps}\n".encode())
        out.extend(actions)
        sys.stdout.buffer.write(out)
        return

    # First teleport.
    first_edge = edges[0]
    first_comp = start_comp

    q_cell = portal_cell[first_comp]
    q_dir = portal_dir[first_comp]

    u = witness[first_edge]
    v = target_cell[first_edge]
    d = edge_dir[first_edge]

    # Move to the source portal position.
    path = get_path(start, q_cell, first_comp)
    for x in path:
        add_move(x)

    # Blue is the initial source portal.
    add_shot(ord('B'), q_dir)

    # Move to the shooting position for the destination portal.
    path = get_path(q_cell, u, first_comp)
    for x in path:
        add_move(x)

    # Orange becomes the destination portal.
    add_shot(ord('O'), d)

    # Return to the blue portal.
    for x in reversed(path):
        add_move(OPPOSITE[x])

    # Enter the blue portal and arrive at v.
    add_move(q_dir)

    current_cell = v
    current_portal_dir = d
    current_portal_color = ord('O')

    # Remaining teleports.
    for i in range(1, len(edges)):
        e = edges[i]
        cid = components[i]

        u = witness[e]
        v = target_cell[e]
        d = edge_dir[e]

        # Move from the arrival point to the shooting position.
        path = get_path(current_cell, u, cid)
        for x in path:
            add_move(x)

        # Replace the portal of the opposite color.
        new_color = ord('B') if current_portal_color == ord('O') else ord('O')
        add_shot(new_color, d)

        # Return to the existing portal.
        for x in reversed(path):
            add_move(OPPOSITE[x])

        # Enter the existing portal.
        add_move(current_portal_dir)

        current_cell = v
        current_portal_dir = d
        current_portal_color = new_color

    # Finish by ordinary movement.
    final_cid = finish_comp
    path = get_path(current_cell, finish, final_cid)
    for x in path:
        add_move(x)

    out = bytearray()
    out.extend(f"{shots} {steps}\n".encode())
    out.extend(actions)
    sys.stdout.buffer.write(out)

if __name__ == "__main__":
    solve()
```Chỉ có nhãn giai đoạn đầu`.`tế bào. các`comp`mảng là một mảng số nguyên nhỏ gọn chứ không phải là danh sách Python, điều này quan trọng vì có thể có một triệu ô. Trong cùng một BFS, mã ghi nhớ một ô trống cạnh một bức tường vững chắc. Đó là vị trí mà cổng đầu tiên của chuỗi dịch chuyển có thể được đặt. 

Việc xây dựng biểu đồ có chủ ý tránh lưu trữ bốn giá trị gần nhất cho mỗi ô. Thay vào đó, mỗi hàng và cột được quét hai lần. Trong quá trình quét từ phải sang trái, khi gặp một bức tường vững chắc, mã sẽ ghi nhớ ô trống ngay bên trái của nó. Mỗi ô trống sau đó trước một bức tường vững chắc khác có thể bắn về phía bức tường đó. Các thao tác quét trái, lên và xuống đều đối xứng. 

Điều kiện kiểm tra ô ngay sát vách kiên cố là phần tinh tế. Một ô thủy tinh có thể ở bất kỳ đâu giữa người bắn và bức tường, nhưng ô chạm vào cổng phải trống. Nếu nó là`G`, cổng không thể được sử dụng một cách an toàn nên mục tiêu sẽ bị loại bỏ. 

BFS thành phần lưu trữ cạnh đồ thị chính xác được sử dụng để tiếp cận mọi thành phần. Cạnh đó chứa ô bắn, hướng và ô đích. Do đó, việc tìm kiếm đồ thị không chỉ cho chúng ta biết rằng có một quá trình chuyển đổi tồn tại mà còn cung cấp đủ thông tin hình học để in ra kết quả tương ứng.`O`hoặc`B`hoạt động. 

các`get_path`hàm thực hiện một BFS thông thường được giới hạn ở một thành phần. Nó chỉ được gọi cho các thành phần xuất hiện trên đường dẫn thành phần cuối cùng. Một đường dẫn đơn giản bên trong một thành phần có ít hơn số lượng ô của nó, do đó tổng công việc vẫn là tuyến tính. Mảng trước đó chỉ được đặt lại cho các ô được BFS đó chạm vào thay vì mỗi lần xây dựng lại cấu trúc triệu phần tử. 

Sản lượng được tích lũy trong một`bytearray`. Một giải pháp hợp lệ có thể chứa tới hàng triệu hành động, do đó việc lưu trữ mọi hành động dưới dạng một chuỗi Python riêng biệt sẽ tạo ra chi phí đối tượng không cần thiết. Biểu diễn byte nhỏ gọn và có thể được viết trực tiếp ở cuối. 

Màu sắc của cổng thay đổi sau lần dịch chuyển đầu tiên. Cổng màu xanh lam đầu tiên là nguồn, trong khi màu cam là đích đến đầu tiên. Sau khi đến đích đó, màu cam là cổng hiện có sẵn trong thành phần hiện tại. Lần bắn tiếp theo sẽ tạo ra một điểm đến màu xanh lam, sau đó cổng màu cam sẽ được sử dụng. Mô hình xen kẽ này chính xác là nguyên nhân khiến mỗi lần dịch chuyển sau đó phải tốn một lần. 

Chuyển động thông thường bị ràng buộc sau khi xây dựng. Bên trong mỗi thành phần không phải là thành phần cuối cùng, tuyến đường đi từ ô nhập của nó đến ô chụp và quay lại, có giá tối đa gấp đôi số lượng ô trong thành phần đó. Thành phần cuối cùng chỉ được duyệt một lần. Việc vào mỗi cổng sẽ thêm một hành động chuyển động và tổng số kết quả tối đa là (2NM). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các tế bào tự do chia thành hai thành phần bình thường. Thành phần bắt đầu chứa`(2,3)`Và`(2,4)`, trong khi thành phần thoát chứa các ô trống ở hàng 4. 

Hành lang thẳng đứng xuyên qua cột 3 chứa một ô kính giữa thành phần ban đầu và thành phần bên dưới. Bức tường ranh giới vững chắc bên dưới có một ô trống ngay phía trên nó, vì vậy việc bắn xuống ngay từ đầu có thể tạo ra một cổng thông tin có thể sử dụng được cho thành phần bên dưới. 

| Hành động | Ô hiện tại | Cổng thông tin hữu ích hiện có | Hoạt động | Kết quả | 
| --- | --- | --- | --- | --- | 
|`OD`|`(2,3)`| không | Đặt màu cam bên dưới qua kính | Màu cam thuộc thành phần phía dưới | 
|`BL`|`(2,3)`| màu cam bên dưới | Đặt màu xanh trên bức tường bên trái | Màu xanh là cổng nguồn | 
|`ML`|`(2,3)`| màu xanh trái | Vào cổng màu xanh | Dịch chuyển đến thành phần thấp hơn | 
|`ML`| thành phần thấp hơn | màu cam bên dưới | Di chuyển thông thường | Tiếp cận lối ra | 

Giải pháp cần hai lần bắn, vì đây là lần dịch chuyển đầu tiên. Điểm hình học quan trọng là ô thủy tinh không ngăn cản phát bắn tiếp cận ranh giới vững chắc và ô tự do ngay trước ranh giới làm cho cổng kết quả có thể sử dụng được. 

### Mẫu 2 

Điểm bắt đầu và kết thúc nằm ở các thành phần thông thường khác nhau. Biểu đồ thành phần chứa một đường dẫn sử dụng một dịch chuyển tức thời, vì vậy số lần chụp tối thiểu là hai. 

| Giai đoạn | Thành phần | Hoạt động | Mục đích | 
| --- | --- | --- | --- | 
| 1 | Bắt đầu | Di chuyển thông thường | Đạt được vị trí chụp hữu ích | 
| 2 | Bắt đầu |`OU`| Tạo cổng thông tin đích | 
| 3 | Bắt đầu | Di chuyển thông thường | Tiếp cận cổng nguồn | 
| 4 | Bắt đầu |`BR`| Tạo cổng thông tin nguồn | 
| 5 | Bắt đầu |`M`vào cổng màu xanh | Dịch chuyển tức thời | 
| 6 | Thoát thành phần | Di chuyển thông thường | Đến lối ra | 

Đường dẫn thông thường chính xác có thể khác với đầu ra mẫu. Cờ đam không yêu cầu số bước di chuyển ngắn nhất mà chỉ yêu cầu tối đa (2NM). Biểu đồ BFS chỉ liên quan đến số lần dịch chuyển tức thời, là số lượng xác định số lần bắn tối thiểu. 

Mẫu cũng chứng minh tại sao việc thay thế cổng thông tin lại quan trọng. Sau lần dịch chuyển đầu tiên, một cổng vẫn ở thành phần cũ và cổng còn lại ở thành phần mới. Lần bắn sau có thể thay thế cổng cũ bằng cổng đích, cho phép dịch chuyển tức thời khác chỉ trong một lần bắn bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(NM)) | Thành phần BFS, bốn lần quét lưới, BFS thành phần và xây dựng lại đường dẫn, mỗi quá trình chỉ xử lý một số lượng ô lưới hoặc cạnh đồ thị không đổi | 
| Không gian | (O(NM)) | Id thành phần, cạnh đồ thị, tiền thân BFS và bộ đệm hành động đầu ra đều tuyến tính trong kích thước lưới | 

Có nhiều nhất (10^6) ô. Việc xây dựng biểu đồ chỉ tạo ra một số lượng cạnh ứng cử viên không đổi trên mỗi ô trống và tất cả các tìm kiếm trên biểu đồ đều tuyến tính về số lượng ô và cạnh. nhỏ gọn`array`các cấu trúc giữ mức sử dụng bộ nhớ tỷ lệ thuận với kích thước đầu vào thay vì chi phí đối tượng Python lớn hơn nhiều của biểu diễn không gian trạng thái đầy đủ. 

## Trường hợp thử nghiệm 

Đầu ra không phải là duy nhất, vì vậy việc so sánh toàn bộ chuỗi đầu ra không phải là phép thử hữu ích cho vấn đề này. Thay vào đó, các thử nghiệm bên dưới so sánh các thuộc tính bắt buộc: liệu giải pháp có thể truy cập được hay không, số lần chụp tối thiểu và giới hạn chuyển động. Họ cũng xác minh rằng số lượng hành động được in khớp với tiêu đề.```python
# Save the submitted solution as solution.py.
# The helper imports its solve() function.

import io
import sys

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def header(out: str):
    first = out.splitlines()[0].split()
    return tuple(map(int, first))

def check_valid_header(inp: str, out: str, expected_p: int):
    lines = out.splitlines()
    p, s = map(int, lines[0].split())

    assert p == expected_p
    assert s >= 0

    n, m = map(int, inp.splitlines()[0].split())
    assert s <= 2 * n * m
    assert len(lines) - 1 == p + s

sample1 = """\
5 5
WWWWW
WW..W
WWGWW
W...W
WWWWW
2 3
4 2
"""

sample2 = """\
7 6
WWWWWW
W..W.W
W.W..W
W.W..W
W.WG.W
W...WW
WWWWWW
2 3
2 5
"""

sample3 = """\
5 5
WWWWW
W.G.W
WW.GW
W.G.W
WWWWW
2 2
4 2
"""

assert header(run(sample1))[0] == 2, "sample 1 must use two shots"
check_valid_header(sample1, run(sample1), 2)

assert header(run(sample2))[0] == 2, "sample 2 must use two shots"
check_valid_header(sample2, run(sample2), 2)

assert header(run(sample3))[0] == 4, "sample 3 must use four shots"
check_valid_header(sample3, run(sample3), 4)

# Minimum-size grid, start equals exit.
minimum_case = """\
3 3
WWW
W.W
WWW
2 2
2 2
"""

assert run(minimum_case) == "0 0\n", "same start and exit need no actions"

# Different components with no usable portal transition.
unreachable_case = """\
5 5
WWWWW
W.GWW
WWWWW
W...W
WWWWW
2 2
4 2
"""

assert run(unreachable_case) == "-1 -1\n", "glass directly before a solid wall must not create an edge"

# Boundary-adjacent free cells, but ordinary movement is already sufficient.
boundary_case = """\
5 5
WWWWW
W...W
W.W.W
W...W
WWWWW
2 2
4 4
"""

out = run(boundary_case)
assert header(out)[0] == 0
check_valid_header(boundary_case, out, 0)

# Maximum-size all-open interior. Everything is one ordinary component.
# The minimum number of shots is zero and a shortest Manhattan path has
# 1994 movement steps.
n = 1000
m = 1000
rows = []
rows.append("W" * m)
for _ in range(n - 2):
    rows.append("W" + "." * (m - 2) + "W")
rows.append("W" * m)

maximum_case = (
    f"{n} {m}\n"
    + "\n".join(rows)
    + "\n2 2\n999 999\n"
)

out = run(maximum_case)
p, s = header(out)
assert p == 0
assert s == 1994
check_valid_header(maximum_case, out, 0)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | (P=2), hợp lệ (S\le2NM) | Cổng chuyển tiếp đầu tiên qua kính | 
| Mẫu 2 | (P=2), hợp lệ (S\le2NM) | Chuyển động thông thường không cần thiết xung quanh quá trình chuyển đổi cổng thông tin | 
| Mẫu 3 | (P=4), hợp lệ (S\le2NM) | Một số thay thế cổng một lần liên tiếp | 
|`3 x 3`, bắt đầu bằng thoát |`0 0`| Trường hợp lưới tối thiểu và không bắn | 
| Các thành phần biệt lập với`G`trước`W`|`-1 -1`| Ngăn chặn việc coi một cổng thông tin nguy hiểm là có thể sử dụng được | 
| Mê cung thông thường liền kề ranh giới | (P=0) | Xử lý ranh giới và kết nối thông thường | 
|`1000 x 1000`nội thất mở |`P=0`,`S=1994`| Kích thước lưới tối đa, mức sử dụng bộ nhớ và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Đối với lưới tối thiểu```
3 3
WWW
W.W
WWW
2 2
2 2
```việc ghi nhãn thành phần tạo ra chính xác một thành phần miễn phí chứa phần bắt đầu và thoát. Thành phần BFS không bao giờ cần thiết vì hai ô giống hệt nhau. Việc xây dựng lại đường dẫn trả về một đường dẫn trống, do đó chương trình sẽ in`0 0`. 

Đối với trường hợp kính trước tường đặc```
5 5
WWWWW
W.GWW
WWWWW
W...W
WWWWW
2 2
4 2
```thành phần bắt đầu có một ô trống bên cạnh bức tường vững chắc bên trên và bên dưới, nhưng không cấp quyền truy cập vào thành phần khác. Khi quét về bên phải, bức tường đặc ở cột 4 có một ô kính ở cột 3 ngay trước nó. Do đó, mục tiêu sẽ bị loại bỏ. Các hướng khác tiếp cận các bức tường vững chắc có các cạnh cổng vẫn ở thành phần bắt đầu. Thành phần thoát không bao giờ được thành phần BFS phát hiện, vì vậy câu trả lời là`-1 -1`. 

Đối với lưới có nhiều ô trống trong cùng một thành phần, chẳng hạn như```
5 5
WWWWW
W...W
W.W.W
W...W
WWWWW
2 2
4 4
```ghi nhãn thành phần đặt tất cả các ô tự do vào một thành phần. Bất kỳ cạnh đồ thị nào được tạo ra bởi các bức tường biên nhìn thấy được đều là các cạnh tự nó và bị bỏ qua. Đường dẫn thông thường là đủ nên kết quả là không có bức ảnh nào. Điều này giúp việc xây dựng biểu đồ không gây nhầm lẫn giữa vị trí cổng thông tin có thể có với chuyển đổi cổng thông tin cần thiết. 

Đối với một thành phần được bao quanh bởi kính, thành phần đó vẫn có thể xuất hiện dưới dạng game bắn súng trong quá trình quét tầm nhìn vì một phát bắn có thể xuyên qua kính. Tuy nhiên, nếu thành phần không có ô tự do tiếp giáp trực tiếp với một bức tường vững chắc,`portal_cell[cid]`ở lại`-1`. BFS sau đó từ chối sử dụng thành phần đó làm nguồn dịch chuyển. Điều này phù hợp với quy tắc vật lý: thành phần có thể kích hoạt nhưng nó không có cổng an toàn để người chơi có thể rời đi. 

Đối với trường hợp kích thước tối đa có lưới (1000\times1000) và phần bên trong mở, toàn bộ phần bên trong là một thành phần. Đường đi ngắn nhất thông thường từ`(2,2)`ĐẾN`(999,999)`có (997+997=1994) bước, do đó chương trình trả về 0 lượt quay và 1994 bước chuyển động. Việc xây dựng biểu đồ vẫn chỉ quét một số lần không đổi trên một triệu ô, chứng minh tại sao công thức tuyến tính phù hợp với các ràng buộc.
