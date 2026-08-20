---
title: "CF 102264C - Thang và Rắn"
description: "Căn phòng là một nửa mặt phẳng được giới hạn bên dưới bởi y = 0 và bên trên bởi y = H. Mỗi chiếc thang là một đoạn thẳng đứng với một số nguyên X, từ độ cao A đến độ cao B. Flynn có thể di chuyển theo chiều ngang ở bất cứ đâu, nhưng chỉ có thể di chuyển theo chiều dọc khi cô ấy ở trên một cái thang."
date: "2026-08-19T03:07:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102264
codeforces_index: "C"
codeforces_contest_name: "2019 Facebook Hacker Cup, Round 1"
rating: 0
weight: 102264
solve_time_s: 542
verified: true
draft: false
---

[CF 102264C - Thang và Rắn](https://codeforces.com/problemset/problem/102264/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 2 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Căn phòng là một nửa mặt phẳng được giới hạn bên dưới bởi`y = 0`trở lên bởi`y = H`. Mỗi bậc thang là một đoạn thẳng đứng với một số nguyên nào đó`X`, từ độ cao`A`chiều cao`B`. Flynn có thể di chuyển theo chiều ngang ở bất cứ đâu, nhưng chỉ có thể di chuyển theo chiều dọc khi cô ấy ở trên thang. 

Những con rắn cũng là những đoạn thẳng đứng nhưng lại là chướng ngại vật. Một con rắn lúc nào đó`x`chặn chuyển động ngang qua đó`x`cho mọi độ cao giữa hai điểm cuối của nó. Điểm cuối của nó được bao gồm trong chướng ngại vật, vì vậy một con rắn có chiều dài bằng 0 có thể chặn một độ cao chính xác. Giá của một con rắn là chiều dài thẳng đứng của nó. 

Nhiệm vụ là chọn những con rắn có tổng chiều dài tối thiểu sao cho không có đường đi liên tục từ`(0,0)`ĐẾN`(0,H)`. Nếu Flynn không thể thực hiện chuyến đi đó mà không có rắn thì câu trả lời là`0`. Nếu không có bộ sưu tập rắn hữu hạn nào có thể ngăn cản cô ấy thì câu trả lời là`-1`. 

Có nhiều nhất là 50 thang, nhưng`H`có thể là 100.000 Điều đó ngay lập tức loại trừ một biểu đồ có một trạng thái cho mọi chiều cao nguyên và mọi bậc thang, vì ngay cả một trường hợp thử nghiệm cũng có thể tạo ra hàng triệu trạng thái và có tới 150 trường hợp thử nghiệm. Quan sát hữu ích là độ cao duy nhất mà cấu trúc sắp xếp của thang thay đổi là điểm cuối của thang. Có nhiều nhất`2N`độ cao như vậy, do đó bài toán liên tục có thể được nén lại chỉ còn`O(N)`các cấp độ liên quan. 

Một trường hợp đặc biệt nguy hiểm là căn phòng nơi Flynn đã bị mắc kẹt. Ví dụ,```
1
2 100
1 0 49
1 50 100
```cho```
Case #1: 0
```Hai cái thang giống nhau`x`phối hợp nhưng không chồng lên nhau nên Flynn không thể di chuyển theo chiều dọc từ điểm này sang điểm kia. Thêm rắn là không cần thiết. Một giải pháp mù quáng cho rằng phải có một con rắn sẽ tạo ra một câu trả lời tích cực. 

Một trường hợp quan trọng khác là khi chuyển tiếp duy nhất có thể sử dụng sàn hoặc trần nhà. Ví dụ,```
1
3 9
33 0 6
66 0 9
99 3 9
```cho```
Case #1: -1
```Thang thứ nhất và thứ hai chồng lên nhau ở sàn, còn thang thứ hai và thứ ba chồng lên nhau ở trần nhà. Con rắn không thể chạm vào một trong hai ranh giới nên không thể chặn quá trình chuyển đổi nào. Một giải pháp xử lý sự chồng chéo có độ dài bằng 0 hoặc điểm cuối dưới dạng cắt giảm chi phí hữu hạn thông thường sẽ trả về không chính xác`0`. 

Trường hợp cạnh thứ ba là một con rắn có chiều dài bằng 0. Ví dụ,```
1
2 5
1 0 2
2 2 5
```có câu trả lời`0`. Các bậc thang gặp nhau ở độ cao`2`, và một con rắn có độ dài bằng 0 được đặt chính xác giữa chúng`y = 2`chặn quá trình chuyển đổi duy nhất. Chi phí bằng 0 vì con rắn chỉ chiếm một điểm. Bất kỳ triển khai nào yêu cầu mọi con rắn hữu ích đều có độ dài dương đều bỏ lỡ trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là rời rạc hóa mọi chiều cao nguyên. Ở mọi độ cao, chúng ta có thể ghi lại những bậc thang nào tồn tại và những chuyển đổi theo chiều ngang nào có thể xảy ra, sau đó tìm kiếm biểu đồ trạng thái kết quả. Vấn đề là ở chỗ đó`H`có thể là 100.000 và mỗi thang trong số 50 thang có thể tương tác với mọi độ cao. Một đồ thị với`O(NH)`các trạng thái có thể đạt tới năm triệu trạng thái trong một phòng, con số này là quá nhiều khi tất cả các trường hợp thử nghiệm đều được xem xét. 

Cách tiếp cận bạo lực là đúng về mặt khái niệm vì chuyển động của Flynn có thể được xem như một biểu đồ về các khả năng theo chiều ngang và chiều dọc. Vấn đề là hầu như tất cả những độ cao đó đều không thể phân biệt được. Giữa hai điểm cuối của bậc thang liên tiếp, không có gì thay đổi. Một con rắn có thể di chuyển liên tục ở đó, nhưng không có sự lựa chọn tổ hợp mới nào trong khoảng đó. 

Quan sát quan trọng là nhìn vấn đề từ phía bên kia. Hãy tưởng tượng bạn đang cố gắng xây dựng một rào cản ngăn Flynn di chuyển từ phía sàn lên phía trần nhà. Những chiếc thang hiện tại là trở ngại cho một rào cản như vậy. Một rào chắn có thể di chuyển theo chiều dọc qua một dải dọc trống và chuyển động đó tiêu tốn chính xác chiều dài con rắn được sử dụng. Tại điểm cuối của thang, rào chắn có thể đi qua điểm cuối và chuyển từ phía bên trái của thang sang phía bên phải mà không mất thêm chi phí. Đây là cách nhìn phẳng-kép của vấn đề. 

Chỉ có điểm cuối của thang mới quan trọng. Giữa hai độ cao điểm cuối liên tiếp, tập hợp các bậc thang không đổi, do đó việc di chuyển theo chiều dọc trong khoảng đó luôn có cùng chi phí cho mỗi đơn vị chiều cao. Do đó, chúng ta có thể tạo một nút cho mỗi cặp bao gồm một khoảng cách dọc giữa các bậc thang`x`tọa độ và chiều cao điểm cuối của thang bên trong. 

Bên trong một khoảng trống, các độ cao điểm cuối liên tiếp được kết nối với một cạnh có chi phí là hiệu của chúng. Băng qua điểm cuối của thang sẽ kết nối hai khoảng trống ngay bên trái và bên phải của thang với cạnh không tốn chi phí. Độ cao`0`Và`H`không thể được sử dụng làm điểm cuối rắn, vì vậy chúng bị loại khỏi biểu đồ rào cản hữu hạn. Nếu một rào chắn phải vượt qua một cái thang ở sàn hoặc trần nhà thì không thể chặn được sự chuyển tiếp đó, đó chính xác là tình huống tạo ra`-1`. 

Trước khi xây dựng biểu đồ này, trước tiên chúng tôi kiểm tra xem Flynn có thể chạm tới trần nhà mà không cần rắn hay không. Coi mỗi bậc thang là một đỉnh và nối hai bậc thang khi các khoảng thẳng đứng của chúng giao nhau. Thang chạm sàn là đỉnh bắt đầu, thang chạm trần là đỉnh đích. Nếu không đến được đích đến, câu trả lời là ngay lập tức`0`. Nếu có thể đến được đích thì vấn đề rào cản tối thiểu là hữu hạn trừ khi mọi rào cản có thể đều bị buộc phải vượt qua ranh giới. 

Đồ thị kết quả chỉ có`O(N^2)`tiểu bang. Thuật toán Dijkstra sau đó tìm ra tổng chiều dài thẳng đứng tối thiểu của một rào chắn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu có chiều cao nguyên | O(NH + N2H) | O(NH) | Quá chậm | 
| Đường dẫn ngắn nhất được nén điểm cuối | O(N2 log N) | O(N2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các bậc thang và sắp xếp chúng theo thứ tự khác nhau`X`tọa độ. Các tọa độ này chia phòng thành các khoảng trống dọc. Khoảng trống là vùng ngay giữa hai bậc thang liên tiếp`x`tọa độ, có thêm một khoảng trống ở bên trái của mỗi bậc thang và một khoảng trống ở bên phải. 
2. Kiểm tra khả năng tiếp cận mà không có rắn. Tạo một biểu đồ chồng chéo khoảng thời gian trên các bậc thang. Hai thang liền kề nhau nếu các khoảng thẳng đứng khép kín của chúng giao nhau. Bắt đầu từ mọi bậc thang với`A = 0`, và xem liệu có cái thang nào đó với`B = H`có thể truy cập được. Điều này ghi lại chính xác những bước di chuyển mà Flynn có thể thực hiện khi không có con rắn nào tồn tại, bởi vì cô ấy có thể di chuyển theo chiều ngang ở một độ cao thông thường và theo chiều dọc dọc theo một cái thang. 
3. Nếu không thể leo lên được thang trần, hãy quay lại`0`. Flynn đã không thể hoàn thành hành trình nên Sneider không cần phải đặt bất cứ thứ gì. 
4. Thu thập mọi điểm cuối của thang một cách nghiêm ngặt trong phòng, cụ thể là mọi`A`Và`B`thỏa mãn`0 < y < H`và sắp xếp các giá trị riêng biệt. Đây là những đỉnh cao duy nhất mà cấu trúc tổ hợp thay đổi. 
5. Tạo một nút biểu đồ cho mỗi cặp`(gap, y)`Ở đâu`y`là một trong những độ cao điểm cuối nội bộ. Nút có nghĩa là rào chắn hiện đang ở trong khoảng trống dọc ở độ cao`y`. 
6. Bên trong mỗi khoảng trống, nối các điểm cuối có độ cao liên tiếp. Nếu hai độ cao là`y1 < y2`, đưa ra chi phí cạnh`y2 - y1`. Việc di chuyển rào cản giữa những độ cao đó đòi hỏi chính xác chiều dài con rắn đó. 
7. Đối với mỗi bậc thang, hãy nhìn vào hai điểm cuối của nó. Tại điểm cuối nội bộ`y`, kết nối nút`(left_gap, y)`với`(right_gap, y)`sử dụng lợi thế chi phí bằng 0. Một rào chắn có thể đi xung quanh điểm cuối của thang mà không tốn chiều dài theo chiều dọc. Nếu điểm cuối nằm trên sàn hoặc trần nhà thì không thêm điểm chuyển tiếp hữu hạn như vậy, vì rắn bị cấm chạm vào những ranh giới đó. 
8. Khởi tạo Dijkstra từ mọi độ cao điểm cuối bên trong nằm trên thang chạm sàn. Cả hai bên của bậc thang đó đều là vị trí bắt đầu hợp lệ khi chúng tồn tại. Di chuyển dọc theo thang không mất gì, vì vậy mọi trạng thái như vậy đều bắt đầu với khoảng cách bằng 0. 
9. Tương tự, mọi điểm cuối bên trong nằm trên thang chạm trần đều là trạng thái đích. Khoảng cách Dijkstra nhỏ nhất giữa các trạng thái này là câu trả lời bắt buộc. 
10. Nếu không có trạng thái đích nào có thể truy cập được trong biểu đồ rào cản, hãy xuất ra`-1`. Điều này có nghĩa là mọi sự ngăn cách có thể sẽ phải sử dụng sàn hoặc trần nhà, những nơi mà rắn không được phép chạm vào. 

### Tại sao nó hoạt động 

Điều bất biến là mọi đường dẫn biểu đồ đều biểu thị một rào cản hợp lệ và mọi rào cản tối thiểu hợp lệ đều có thể được chuyển đổi thành đường dẫn biểu đồ mà không cần tăng độ dài của nó. Phần thẳng đứng của con rắn nằm bên trong một khoảng trống và được biểu diễn bằng các cạnh đồ thị thẳng đứng có tổng trọng lượng bằng chiều dài của nó. Bất cứ khi nào rào cản thay đổi từ bên này sang bên kia của thang, nó phải đi xung quanh một điểm cuối, được biểu thị bằng sự chuyển đổi không tốn chi phí tại điểm cuối đó. Giữa các điểm cuối của thang không có sự thay đổi về cấu trúc nên việc di chuyển điểm rẽ đến điểm cuối của thang không bao giờ làm tăng chiều dài theo chiều dọc cần thiết. Trạng thái ban đầu và cuối cùng tương ứng với các thang mà Flynn có thể sử dụng từ sàn nhà lên trần nhà. Do đó, đường đi ngắn nhất của đồ thị chính xác là tổng chiều dài tối thiểu của con rắn. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def solve_case():
    n, H = map(int, input().split())
    ladders = [tuple(map(int, input().split())) for _ in range(n)]

    # First check whether Flynn can already reach the ceiling.
    start = [i for i, (_, a, _) in enumerate(ladders) if a == 0]
    target = [i for i, (_, _, b) in enumerate(ladders) if b == H]

    reachable = [False] * n
    stack = start[:]
    for i in start:
        reachable[i] = True

    while stack:
        u = stack.pop()
        _, au, bu = ladders[u]

        for v in range(n):
            if reachable[v] or v == u:
                continue

            _, av, bv = ladders[v]

            if max(au, av) <= min(bu, bv):
                reachable[v] = True
                stack.append(v)

    if not any(reachable[i] for i in target):
        return 0

    # Distinct x coordinates define vertical gaps.
    xs = sorted(set(x for x, _, _ in ladders))
    x_id = {x: i for i, x in enumerate(xs)}

    # Each ladder at x=xs[k] has gap k on its left and k+1 on its right.
    gap_count = len(xs) + 1

    # Only internal heights matter for finite snakes.
    ys = sorted({
        y
        for _, a, b in ladders
        for y in (a, b)
        if 0 < y < H
    })

    if not ys:
        # If there is a reachable floor-to-ceiling ladder chain but
        # there are no internal endpoints, every relevant ladder
        # touches a boundary. No finite snake can separate it.
        return -1

    y_id = {y: i for i, y in enumerate(ys)}
    k = len(ys)

    # Node (gap, y-index).
    total_nodes = gap_count * k

    def node(g, yi):
        return g * k + yi

    graph = [[] for _ in range(total_nodes)]

    def add_edge(u, v, w):
        graph[u].append((v, w))
        graph[v].append((u, w))

    # Vertical movement inside every empty gap.
    for g in range(gap_count):
        base = g * k
        for i in range(k - 1):
            w = ys[i + 1] - ys[i]
            add_edge(base + i, base + i + 1, w)

    # Crossing around ladder endpoints.
    for x, a, b in ladders:
        g = x_id[x]

        # Internal bottom endpoint.
        if 0 < a < H:
            yi = y_id[a]
            if g > 0:
                add_edge(node(g, yi), node(g - 1, yi), 0)
            if g + 1 < gap_count:
                add_edge(node(g, yi), node(g + 1, yi), 0)

        # Internal top endpoint.
        if 0 < b < H:
            yi = y_id[b]
            if g > 0:
                add_edge(node(g, yi), node(g - 1, yi), 0)
            if g + 1 < gap_count:
                add_edge(node(g, yi), node(g + 1, yi), 0)

    # We need only finite barrier paths that start on a floor ladder
    # and end on a ceiling ladder. Starting/ending at any internal
    # endpoint on such a ladder costs zero.
    dist = [INF] * total_nodes
    pq = []

    def seed_ladder(x, a, b):
        g = x_id[x]

        for yi, y in enumerate(ys):
            if a <= y <= b:
                if g > 0:
                    u = node(g, yi)
                    if dist[u] != 0:
                        dist[u] = 0
                        heapq.heappush(pq, (0, u))

                if g + 1 < gap_count:
                    u = node(g + 1, yi)
                    if dist[u] != 0:
                        dist[u] = 0
                        heapq.heappush(pq, (0, u))

    for x, a, b in ladders:
        if a == 0:
            seed_ladder(x, a, b)

    target_nodes = set()

    for x, a, b in ladders:
        if b == H:
            g = x_id[x]

            for yi, y in enumerate(ys):
                if a <= y <= b:
                    if g > 0:
                        target_nodes.add(node(g, yi))
                    if g + 1 < gap_count:
                        target_nodes.add(node(g + 1, yi))

    while pq:
        d, u = heapq.heappop(pq)

        if d != dist[u]:
            continue

        if u in target_nodes:
            return d

        for v, w in graph[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    return -1

def main():
    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        ans = solve_case()
        out.append(f"Case #{case_id}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Việc tìm kiếm đồ thị đầu tiên được tách biệt một cách có chủ ý khỏi việc tính toán đường đi ngắn nhất. Nó trả lời một câu hỏi khác: liệu có cần một câu trả lời hữu hạn hay không. Thử nghiệm chồng chéo khoảng thời gian sử dụng các khoảng thời gian khép kín vì Flynn có thể di chuyển theo chiều ngang chính xác tại điểm cuối của thang. 

Biểu đồ thứ hai chỉ sử dụng độ cao điểm cuối bên trong. Đây là nén chính. Có thể có nhiều nhất`2N - 2`độ cao như vậy, vì vậy với nhiều nhất`N + 1`khoảng trống chỉ có`O(N²)`tiểu bang. 

Các cạnh dọc sử dụng sự khác biệt giữa các độ cao liên tiếp. Tổng của chúng chính xác bằng chiều dài của các miếng rắn tương ứng. Chênh lệch bằng 0 không bao giờ xuất hiện vì độ cao của điểm cuối bị trùng lặp. 

Quá trình chuyển đổi không tốn phí được tạo tại các điểm cuối của bậc thang. Các trường hợp ranh giới`y = 0`Và`y = H`được cố tình loại trừ. Một con rắn không thể chạm vào một trong hai ranh giới, vì vậy việc xử lý điểm cuối ranh giới giống như điểm cuối bậc thang thông thường sẽ biến một sự phân tách không thể thành một điểm hữu hạn một cách không chính xác. 

Số nguyên Python không bị giới hạn nên không có vấn đề tràn.`INF`chỉ cần lớn hơn bất kỳ câu trả lời có thể nào, và`10**30`vượt xa tổng chiều dài tối đa có liên quan. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Phòng có 2 thang:```
L0: x = 0, [0, 3]
L1: x = 1, [1, 4]
```Thang đầu tiên chạm tới sàn và thang thứ hai chạm tới trần nhà, vì vậy Flynn có thể di chuyển giữa chúng mà không cần rắn. Chiều cao bên trong duy nhất có liên quan là`1`Và`3`. 

| Tiểu bang | Hành động | Chi phí | Khoảng cách | 
| --- | --- | --- | --- | 
| L0 tại y=3 | Bắt đầu bằng thang sàn | 0 | 0 | 
| Khoảng cách giữa L0 và L1, y=3 | Đi vòng quanh đỉnh L0 | 0 | 0 | 
| Khoảng cách giữa L0 và L1, y=1 | Di chuyển theo chiều dọc | 2 | 2 | 
| L1 tại y=1 | Đi vòng quanh đáy L1 | 0 | 2 | 

Con đường có tổng chi phí`3 - 1 = 2`, vậy câu trả lời là`2`. 

Dấu vết cũng cho thấy tại sao một con rắn dài gấp đôi lại có tác dụng. Nó chiếm đoạn giữa độ cao 1 và 3, chặn mọi đường ngang giữa hai thang. 

### Mẫu 2 

Hai cái thang là```
L0: x = 1, [0, 49]
L1: x = 1, [50, 100]
```Họ có cùng`x`phối hợp nhưng khoảng cách dọc của chúng rời rạc. 

| Thang hiện tại | Thang ứng viên | Giao lộ | Kết quả | 
| --- | --- | --- | --- | 
|`[0,49]`|`[50,100]`| Trống | Không có cạnh | 
|`[0,49]`| chính nó |`[0,49]`| Đã đại diện | 

Tìm kiếm chồng chéo bậc thang không thể tiếp cận thang trần từ thang sàn. Flynn bị mắc kẹt trước khi Sneider đặt bất cứ điều gì, vì vậy câu trả lời là`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2 log N) | Có các trạng thái và cạnh được nén O(N2), tiếp theo là Dijkstra | 
| Không gian | O(N2) | Biểu đồ nén chứa các nút và cạnh O(N²) | 

Với`N <= 50`, biểu đồ nén chỉ chứa vài nghìn trạng thái mỗi phòng. Giá trị của`H`, ngay cả khi nó là 100.000, ảnh hưởng đến trọng số cạnh nhưng không ảnh hưởng đến số lượng trạng thái. Đây là lý do chính khiến giải pháp vẫn đủ nhỏ cho tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import heapq

# The production solve_case/main code should be placed above this test section.
# For a standalone test file, paste the solution implementation before these tests.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Sample 1
assert run("""\
1
2 4
0 0 3
1 1 4
""") == "Case #1: 2\n"

# Sample 2
assert run("""\
1
2 100
1 0 49
1 50 100
""") == "Case #1: 0\n"

# Sample 3
assert run("""\
1
3 9
33 0 6
66 0 9
99 3 9
""") == "Case #1: -1\n"

# Sample 4
assert run("""\
1
7 30
10 0 10
20 0 10
5 8 21
15 7 20
25 9 22
10 20 30
20 20 30
""") == "Case #1: 3\n"

# Minimum-size room. The single ladder reaches the ceiling directly,
# so no finite snake arrangement can stop Flynn.
assert run("""\
1
1 1
0 0 1
""") == "Case #1: -1\n"

# Zero-length snake is sufficient because the ladders meet at one
# internal height.
assert run("""\
1
2 5
1 0 2
2 2 5
""") == "Case #1: 0\n"

# Same x coordinate, but a genuine gap between ladders. Flynn cannot
# switch from one ladder to the other.
assert run("""\
1
2 10
5 0 3
5 7 10
""") == "Case #1: 0\n"

# A ladder chain reaches the ceiling through an internal touching point,
# so a length-zero snake blocks the only transition.
assert run("""\
1
2 6
2 0 3
4 3 6
""") == "Case #1: 0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 0 0 1`|`Case #1: -1`| Thang nối trực tiếp sàn và trần không thể bị chặn | 
|`2 5 / 1 0 2 / 2 2 5`|`Case #1: 0`| Cho phép một con rắn có độ dài bằng 0 ở điểm cuối bên trong | 
|`2 10 / 5 0 3 / 5 7 10`|`Case #1: 0`| Thang cùng tọa độ x không nối qua một khe dọc | 
|`2 6 / 2 0 3 / 4 3 6`|`Case #1: 0`| Việc chuyển đổi qua một độ cao bên trong chính xác có thể không tốn chi phí | 

## Vỏ cạnh 

Đối với căn phòng mà Flynn đã không thể chạm tới trần nhà, biểu đồ chồng lấp khoảng thời gian sẽ phát hiện điều này trước bất kỳ tính toán rào cản nào. Trong đầu vào```
1
2 100
1 0 49
1 50 100
```hai thang không có chiều cao chung. DFS bắt đầu từ bậc thang đầu tiên không bao giờ đạt đến bậc thang thứ hai, do đó thuật toán trả về`0`. 

Đối với sự chồng lấp ranh giới, bản thân sự chồng chéo không đủ để tạo ra một rào cản hữu hạn. TRONG```
1
3 9
33 0 6
66 0 9
99 3 9
```quá trình chuyển đổi đầu tiên có thể xảy ra tại`y = 0`và lần thứ hai tại`y = 9`. Biểu đồ rào cản cố tình không tạo ra các chuyển tiếp điểm cuối hữu hạn ở các độ cao biên đó. Do đó, Dijkstra không thể tạo ra sự phân tách hữu hạn và trả về`-1`. 

Đối với quá trình chuyển đổi một điểm nội bộ, điểm cuối được bao gồm trong biểu đồ và việc vượt qua điểm đó không tốn phí. TRONG```
1
2 5
1 0 2
2 2 5
```cả hai thang đều có thể sử dụng được tại`y = 2`. Một con rắn có chiều dài bằng 0 đặt giữa chúng ở độ cao đó sẽ chặn quá trình chuyển đổi. Thuật toán gieo hạt cả hai bên tại điểm cuối bên trong và đạt khoảng cách bằng 0. 

Đối với một chiếc thang kéo dài toàn bộ căn phòng, không có điểm cuối bên trong nào mà tại đó rào cản hữu hạn có thể bắt đầu hoặc kết thúc. Kiểm tra khả năng tiếp cận cho thấy sàn và trần được kết nối bằng cùng một thang, trong khi biểu đồ rào cản không có đường hữu hạn ngăn cách chúng. Kết quả đúng là`-1`. 

Đối với nhiều thang có cùng`x`, mỗi thang vẫn là một thành phần dọc riêng biệt vì các thang không thể chồng lên nhau. Việc xây dựng không kết nối hai chiếc thang như vậy chỉ vì chúng có chung một`x`điều phối. Chúng chỉ được kết nối thông qua chuyển động ngang hợp lệ ở độ cao chung, chính xác như các quy tắc chuyển động ban đầu yêu cầu.
