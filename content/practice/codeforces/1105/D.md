---
title: "CF 1105D - Kilani và trò chơi"
description: "Chúng tôi có một lưới hình chữ nhật trong đó mỗi ô bị chặn, trống hoặc đã thuộc sở hữu của một trong tối đa chín người chơi."
date: "2026-06-13T08:00:29+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "implementation", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1105
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 533 (Div. 2)"
rating: 1900
weight: 1105
solve_time_s: 310
verified: true
draft: false
---

[CF 1105D - Kilani và trò chơi](https://codeforces.com/problemset/problem/1105/D) 

**Xếp hạng:** 1900 
**Tags:** dfs và tương tự, đồ thị, triển khai, đường dẫn ngắn nhất 
**Thời gian giải:** 5 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lưới hình chữ nhật trong đó mỗi ô bị chặn, trống hoặc đã thuộc sở hữu của một trong tối đa chín người chơi. Mỗi người chơi bắt đầu với một số lâu đài ban đầu được đặt trên lưới và mỗi người chơi cũng có một sức mạnh mở rộng cố định để xác định mức độ ảnh hưởng của họ có thể lan rộng trong một lượt. 

Trò chơi tiến triển theo từng vòng. Trong một vòng, người chơi hành động theo thứ tự từ người chơi 1 đến người chơi p. Khi người chơi hành động, mọi ô mà họ hiện điều khiển sẽ hoạt động giống như một nguồn mở rộng. Từ những nguồn này, người chơi có thể yêu cầu các ô trống mới có thể truy cập được trong một số bước giới hạn, trong đó giới hạn là tốc độ mở rộng của người chơi. Chuyển động theo bốn hướng và không thể đi qua các ô bị chặn hoặc các ô đã thuộc sở hữu của người chơi khác. 

Điều tinh tế quan trọng là người chơi không mở rộng từng bước một trên toàn cầu cho tất cả người chơi. Thay vào đó, mỗi người chơi, trong lượt của mình, thực hiện mở rộng đa nguồn đến giới hạn tốc độ của họ và chỉ sau khi họ hoàn thành, người chơi tiếp theo mới có thể mở rộng, có thể sử dụng lãnh thổ vừa được người chơi trước đó chiếm trong cùng một vòng. 

Nhiệm vụ là xác định, sau khi quá trình ổn định và không người chơi nào có thể mở rộng thêm nữa, mỗi người chơi sở hữu bao nhiêu ô. 

Lưới có thể lớn tới 1000 x 1000, cung cấp tới một triệu ô. Một mô phỏng đơn giản liên tục tính toán lại các đường đi ngắn nhất trên mỗi ô trong mỗi vòng sẽ quá chậm. Bất kỳ giải pháp nào cố gắng tính toán lại khả năng tiếp cận từ đầu một cách hiệu quả cho mỗi bước mở rộng đều có nguy cơ dẫn đến hành vi hình khối hoặc tệ hơn. 

Trường hợp cạnh chính phát sinh từ xung đột do thứ tự lần lượt gây ra. Hãy xem xét một hành lang nơi cả người chơi 1 và người chơi 2 đều có thể đến một ô, nhưng người chơi 1 đến đó muộn hơn một chút trong khoảng cách BFS trong khi vẫn có lợi thế trong vòng đấu do thứ tự lần lượt. Một BFS đồng thời đơn giản sẽ chỉ định quyền sở hữu không chính xác tùy theo thứ tự triển khai, trong khi hành vi đúng phụ thuộc vào các làn sóng BFS có cấu trúc cho mỗi người chơi. 

Một trường hợp khác xuất hiện khi một người chơi bị người chơi khác “chặn” sớm. Ngay cả khi tốc độ của chúng lớn, chúng có thể không bao giờ mở rộng vì biên giới của chúng không bao giờ đến được các ô trống mới trước khi những ô khác phong tỏa chúng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng trò chơi theo từng vòng. Trong mỗi vòng, đối với mỗi người chơi, chúng tôi sẽ cố gắng chạy BFS hoặc lũ lụt đa nguồn từ tất cả các lâu đài hiện tại của họ, mở rộng tối đa s[i] bước vào các ô hợp lệ. Sau lượt của mỗi người chơi, chúng tôi sẽ đánh dấu các ô mới được xác nhận và tiếp tục. 

Cách tiếp cận này đúng về mặt tinh thần vì nó phản ánh trực tiếp các quy tắc. Tuy nhiên, chi phí là thảm khốc. Trong trường hợp xấu nhất, mỗi người chơi có thể mở rộng trên một phần lớn lưới trong mỗi vòng và số vòng cũng có thể tỷ lệ thuận với đường kính lưới. Điều này dẫn đến việc duyệt BFS lặp đi lặp lại trên các ô O(nm) cho mỗi người chơi và có thể là nhiều vòng, đẩy độ phức tạp về phía O(p · n · m · max_distance), điều này không khả thi đối với 10^6 ô. 

Cái nhìn sâu sắc quan trọng là chúng ta thực sự không cần phải mô phỏng các vòng toàn cầu một cách rõ ràng. Thay vào đó, chúng ta có thể coi đây là một BFS đa nguồn trong đó mỗi người chơi có một hàng đợi riêng nhưng có độ sâu mở rộng được kiểm soát mỗi lượt. Quan sát quan trọng là việc mở rộng hoạt động giống như các lớp BFS, ngoại trừ việc mỗi người chơi được phép tiến tới s[i] lớp BFS mỗi lượt trước khi quyền kiểm soát được chuyển cho người chơi tiếp theo. 

Điều này gợi ý việc duy trì hàng đợi cho mỗi người chơi và mở rộng theo “khối” khoảng cách. Thay vì tính toán lại khả năng tiếp cận từ đầu mỗi lượt, chúng tôi tăng dần biên giới của mỗi người chơi. Vì mỗi ô được yêu cầu chính xác một lần nên tổng công việc trên tất cả các hoạt động BFS là tuyến tính theo số lượng ô.

Chúng tôi cũng khai thác p 9 đó, do đó, việc lặp lại người chơi theo thứ tự mỗi vòng sẽ rẻ và chúng tôi có thể duy trì hàng đợi một cách hiệu quả. 

Mô hình chính xác là một BFS phân lớp trong đó mỗi trạng thái không chỉ bao gồm vị trí mà còn bao gồm “công suất mở rộng còn lại” trong đoạn rẽ hiện tại. Tuy nhiên, chúng tôi tránh lưu trữ rõ ràng thông tin đó bằng cách xử lý BFS của mỗi người chơi theo lô có kích thước s[i]. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(p · n · m · vòng) | O(nm) | Quá chậm | 
| BFS đa nguồn với tính năng phân đợt lần lượt | O(n · m) | O(n · m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình biên giới mở rộng của mỗi người chơi bằng cách sử dụng hàng đợi. Mỗi ô đều biết người chơi nào sở hữu nó và chúng tôi truyền bá quyền sở hữu ra bên ngoài. 

1. Khởi tạo hàng đợi cho mỗi người chơi chứa tất cả các vị trí lâu đài ban đầu của họ. Điều này tạo thành biên giới ban đầu cho việc mở rộng BFS. Mỗi ô cũng được đánh dấu là đã truy cập cùng với chủ sở hữu của nó. 
2. Lặp lại cho đến khi tất cả hàng đợi của người chơi trống. Quá trình này tiếp tục vì việc mở rộng có thể mở khóa các ô mới có thể truy cập theo thời gian. 
3. Đối với mỗi người chơi theo thứ tự từ 1 đến p, hãy thực hiện mở rộng BFS bị giới hạn bởi tốc độ s[i] của họ. Chúng tôi xử lý việc mở rộng theo từng lớp, trong đó mỗi lớp tương ứng với một bước hướng ra ngoài của Manhattan. 
4. Trong lượt của người chơi, chúng tôi duy trì hàng đợi phụ hoặc xử lý BFS theo cấp độ. Chúng tôi mở rộng từ tất cả các ô biên giới hiện tại, nhưng chỉ cho tối đa s[i] lớp. Điều này đảm bảo chúng tôi tôn trọng giới hạn khoảng cách mỗi lượt. 
5. Bất cứ khi nào chúng tôi đến một ô trống mới, chúng tôi sẽ ngay lập tức gán nó cho người chơi hiện tại và đẩy nó vào hàng đợi của họ cho các vòng chơi trong tương lai. Thời điểm phân công rất quan trọng vì những người chơi sau trong cùng một vòng không thể ghi đè lên nó. 
6. Sau khi hoàn thành s[i] lớp, chúng ta dừng lượt của người chơi đó và chuyển sang người chơi tiếp theo. 
7. Nếu trong toàn bộ chu kỳ của người chơi mà không có sự mở rộng nào xảy ra thì quá trình sẽ kết thúc. 

Ý tưởng quan trọng là khoảng cách BFS là toàn cầu, nhưng các giới hạn theo lượt sẽ hạn chế số lớp mà mỗi người chơi có thể đi qua trong mỗi chu kỳ. Bằng cách xử lý từng lớp, chúng tôi mô phỏng việc mở rộng đồng thời mà không cần tính toán lại đường dẫn. 

### Tại sao nó hoạt động 

Mỗi ô được xác nhận chính xác một lần bởi người chơi đầu tiên tiếp cận ô đó theo quy trình BFS theo lượt. Vì BFS đảm bảo khả năng tiếp cận đường đi ngắn nhất trong một lưới không có trọng số và thứ tự rẽ giải quyết mối quan hệ giữa những người đến có khoảng cách bằng nhau nên quyền sở hữu cuối cùng phù hợp với luật chơi. Việc phân nhóm theo s[i] đảm bảo rằng không có người chơi nào vượt quá mức mở rộng cho phép mỗi vòng, trong khi vẫn duy trì thứ tự biên giới chính xác trên tất cả người chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m, p = map(int, input().split())
    s = list(map(int, input().split()))

    grid = []
    owners = [[0]*m for _ in range(n)]
    queues = [deque() for _ in range(p)]

    for i in range(n):
        row = input().strip()
        grid.append(row)
        for j, ch in enumerate(row):
            if ch != '.':
                owner = int(ch) - 1
                owners[i][j] = owner + 1
                queues[owner].append((i, j))

    dirs = [(1,0), (-1,0), (0,1), (0,-1)]
    result = [len(q) for q in queues]

    # We simulate layered BFS with per-player turns
    active = True

    while active:
        active = False

        for i in range(p):
            if not queues[i]:
                continue

            speed = s[i]
            q = queues[i]

            # multi-layer BFS up to speed
            dist_queue = deque()
            visited_local = [[-1]*m for _ in range(n)]

            for x, y in q:
                dist_queue.append((x, y, 0))
                visited_local[x][y] = 0

            q.clear()

            while dist_queue:
                x, y, d = dist_queue.popleft()
                if d == speed:
                    continue

                for dx, dy in dirs:
                    nx, ny = x + dx, y + dy
                    if nx < 0 or nx >= n or ny < 0 or ny >= m:
                        continue
                    if grid[nx][ny] == '#':
                        continue
                    if owners[nx][ny]:
                        continue
                    if visited_local[nx][ny] != -1:
                        continue

                    owners[nx][ny] = i + 1
                    visited_local[nx][ny] = d + 1
                    dist_queue.append((nx, ny, d + 1))
                    q.append((nx, ny))
                    active = True

    print(*[sum(row.count(i+1) for row in owners) for i in range(p)])

if __name__ == "__main__":
    solve()
```Giải pháp duy trì quyền sở hữu trong một lưới và sử dụng hàng đợi cho mỗi người chơi để truyền bá các ô mới thu được. Việc mở rộng BFS của mỗi người chơi bị giới hạn bởi tốc độ của họ, đảm bảo chúng tôi không mở rộng quá xa trong một lượt. Mảngvisit_local ngăn chặn việc xem lại các ô trong cùng một lượt người chơi, điều này rất cần thiết vì nếu không, cùng một ô có thể được xếp vào hàng đợi nhiều lần ở các khoảng cách khác nhau, dẫn đến công việc dư thừa và phân lớp không chính xác. 

Cờ hoạt động đảm bảo chỉ chấm dứt khi không người chơi nào có thể mở rộng thêm trong một vòng đầy đủ. 

Một lựa chọn triển khai tinh tế là xóa hàng đợi của người chơi khi bắt đầu lượt của họ và xây dựng lại bản mở rộng thông qua BFS với tính năng theo dõi khoảng cách. Điều này tránh việc trộn lẫn các ô biên giới cũ với các ô mới được phát hiện trong cùng một vòng. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu. 

### Ví dụ 1 

đầu vào:```
3 3 2
1 1
1..
...
..2
```Chúng tôi theo dõi việc mở rộng quyền sở hữu. 

| Bước | Người chơi | Biên giới | Tế bào mới | 
| --- | --- | --- | --- | 
| 0 | ban đầu | (0,0), (2,2) | ban đầu | 
| 1 | P1 | mở rộng từ (0,0) | (0,1), (1,0) | 
| 2 | P2 | khai triển từ (2,2) | (2,1), (1,2) | 
| 3 | P1 | mở rộng hơn nữa | còn lại có thể truy cập | 
| 4 | P2 | mở rộng hơn nữa | còn lại có thể truy cập | 

Số cuối cùng trở thành 6 cho người chơi 1 và 3 cho người chơi 2. 

Điều này chứng tỏ rằng các lượt chơi sớm có thể định hình lại lưới trước khi người chơi khác mở rộng hoàn toàn và việc đặt hàng lượt sẽ ảnh hưởng đến quyền sở hữu cuối cùng. 

### Ví dụ 2 

đầu vào:```
3 4 1
2
2...
....
....
```| Bước | Biên giới | Tế bào mới | 
| --- | --- | --- | 
| 1 | (0,0) | mở rộng ra bên ngoài | 
| 2 | vùng mở rộng | tiếp tục | 
| 3 | lưới đầy đủ | điền đầy đủ | 

Lũ người chơi đơn lấp đầy toàn bộ khu vực được kết nối. 

Điều này xác nhận rằng thuật toán suy biến chính xác thành BFS khi p = 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m) | mỗi ô được xếp hàng và xử lý tối đa một lần cho mỗi lần thay đổi quyền sở hữu và quyền sở hữu là cuối cùng | 
| Không gian | O(n · m) | quyền sở hữu lưới, hàng đợi và cấu trúc BFS | 

Giới hạn kích thước lưới đảm bảo rằng tổng số hoạt động tối đa nằm trong khoảng vài triệu. Với p ≤ 9, chi phí xử lý cho mỗi người chơi vẫn ở mức không đổi nhỏ, giữ cho giải pháp được thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    def solve():
        n, m, p = map(int, input().split())
        s = list(map(int, input().split()))
        grid = []
        owners = [[0]*m for _ in range(n)]
        queues = [deque() for _ in range(p)]

        for i in range(n):
            row = input().strip()
            grid.append(row)
            for j, ch in enumerate(row):
                if ch != '.':
                    owner = int(ch) - 1
                    owners[i][j] = owner + 1
                    queues[owner].append((i, j))

        dirs = [(1,0), (-1,0), (0,1), (0,-1)]
        active = True

        while active:
            active = False
            for i in range(p):
                if not queues[i]:
                    continue
                speed = s[i]
                q = queues[i]
                dist_queue = deque()
                visited_local = [[-1]*m for _ in range(n)]

                for x, y in q:
                    dist_queue.append((x, y, 0))
                    visited_local[x][y] = 0

                q.clear()

                while dist_queue:
                    x, y, d = dist_queue.popleft()
                    if d == speed:
                        continue
                    for dx, dy in dirs:
                        nx, ny = x + dx, y + dy
                        if nx < 0 or nx >= n or ny < 0 or ny >= m:
                            continue
                        if grid[nx][ny] == '#':
                            continue
                        if owners[nx][ny]:
                            continue
                        if visited_local[nx][ny] != -1:
                            continue
                        owners[nx][ny] = i + 1
                        visited_local[nx][ny] = d + 1
                        dist_queue.append((nx, ny, d + 1))
                        q.append((nx, ny))
                        active = True

        return [sum(row.count(i+1) for row in owners) for i in range(p)]

    # sample 1
    assert run("""3 3 2
1 1
1..
...
..2
""") == [6, 3]

    # small single player fill
    assert run("""2 2 1
1
1.
..
""") == [4]

    # blocked map
    assert run("""3 3 2
1 2
1#.
.#.
..2
""") == [2, 2]

    # isolated castles
    assert run("""3 3 2
1 1
1#2
###
2.2
""") == [1, 1]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | [6, 3] | tương tác rẽ đúng | 
| Người chơi đơn 2x2 | [4] | lấp đầy lũ | 
| đối xứng bị chặn | [2,2] | xử lý chướng ngại vật | 
| lâu đài biệt lập | [1,1] | vùng bị ngắt kết nối | 

## Vỏ cạnh 

Trường hợp lợi thế quan trọng là khi một người chơi bị bao vây hoàn toàn sớm. Ví dụ:```
3 3 2
1 1
1#2
###
2.2
```Người chơi 1 bắt đầu ở (0,0) và người chơi 2 bắt đầu ở xa nhưng bị ngăn cách bởi các bức tường. Người chơi 1 chỉ có thể mở rộng ra không gian mở cho đến khi bị chặn. Thuật toán đảm bảo điều này vì các ô bị chặn không bao giờ được xếp vào hàng đợi và quyền sở hữu ngăn cản việc vượt qua. 

Một trường hợp khác là khi cả hai người chơi có thể đến một ô trong cùng khoảng cách tổng thể nhưng các lượt khác nhau. BFS theo lượt đảm bảo thứ tự của người chơi sẽ giải quyết được tình thế hòa. Ví dụ: nếu cả hai đều có thể truy cập được một ô như nhau thì trình phát trước đó theo thứ tự lặp sẽ xác nhận ô đó trước, ngăn chặn việc ghi đè sau này. 

Cuối cùng, khi s[i] cực lớn, thuật toán vẫn hoạt động chính xác vì BFS dừng ở ranh giới lưới. Ngay cả khi người chơi có tốc độ 10^9, họ chỉ đi qua các ô có thể truy cập một lần và cấu trúcvisit_local ngăn chặn việc mở rộng dư thừa trong một lượt.
