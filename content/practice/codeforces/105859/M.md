---
title: "CF 105859M - Chuyến Đi Đường Núi Rocky"
description: "Bài toán đưa ra một bản đồ núi hình chữ nhật trong đó mỗi ô đều lưu trữ độ cao. Một du khách bắt đầu từ một ô và muốn đến một ô khác. Được phép di chuyển đến bất kỳ ô nào trong số tám ô lân cận, bao gồm cả đường chéo và mỗi lần di chuyển đều tốn một ô."
date: "2026-06-25T14:42:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105859
codeforces_index: "M"
codeforces_contest_name: "Mines HSPC 2025 Open Division"
rating: 0
weight: 105859
solve_time_s: 41
verified: true
draft: false
---

[CF 105859M - Chuyến đi trên đường núi Rocky](https://codeforces.com/problemset/problem/105859/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một bản đồ núi hình chữ nhật trong đó mỗi ô đều lưu trữ độ cao. Một du khách bắt đầu từ một ô và muốn đến một ô khác. Được phép di chuyển đến bất kỳ ô nào trong số tám ô lân cận, bao gồm cả đường chéo và mỗi lần di chuyển đều tốn một ô. Hạn chế bổ sung là hướng thay đổi độ cao phải luân phiên: nếu một bước di chuyển đi lên độ cao cao hơn, bước di chuyển tiếp theo phải đi xuống độ cao thấp hơn và mô hình này vẫn tiếp tục. Nước đi đầu tiên không có nước đi trước đó để so sánh, do đó, có thể tăng hoặc giảm. Nhiệm vụ là tìm số lần di chuyển tối thiểu hoặc quyết định rằng không thể đến đích. 

Kích thước lưới có thể đạt tới 500 x 500, nghĩa là có thể có tới 250.000 ô. Một giải pháp thử mọi tuyến đường có thể là không thể vì số lượng tuyến đường tăng theo cấp số nhân. Ngay cả việc kiểm tra nhiều đường dẫn trên mỗi ô cũng có thể trở nên quá chậm. Chúng ta cần một cái gì đó gần tuyến tính về số lượng ô vì chỉ có một vài trạng thái được liên kết với mỗi vị trí. 

Cái bẫy chính là BFS bình thường không thể tìm thấy đường đi ngắn nhất chỉ trên các ô. Tiếp cận cùng một ô sau khi di chuyển lên trên và tiếp cận ô đó sau khi di chuyển xuống dưới là các tình huống khác nhau vì bước di chuyển được phép tiếp theo là khác nhau. 

Ví dụ, hãy xem xét:```
1 2
3 4
1 1 2 2
```Điểm bắt đầu là ô trên cùng bên trái và mục tiêu là ô dưới cùng bên phải. Một BFS ngây thơ chỉ lưu trữ các vị trí sẽ nhìn thấy một đường đi có độ dài bằng một bằng cách di chuyển theo đường chéo, nhưng độ cao thay đổi từ 1 thành 4 và không có hướng trước đó để thay thế. Nếu bài toán yêu cầu mọi nước đi sau nước đi đầu tiên phải luân phiên, thì nước đi đơn lẻ này là hợp lệ, nhưng sau khi đến đó, trạng thái sẽ khác với trạng thái đến nước đi thông qua một sự giảm bớt. Mảng chỉ được truy cập bằng ô có thể loại bỏ các trạng thái hữu ích một cách không chính xác. 

Một trường hợp khác là:```
1 4
2 3
1 1 2 2
```Nước đi chéo trực tiếp tăng từ 1 lên 3. Nếu sau này chúng ta cần tiếp tục từ ô đó thì nước đi tiếp theo phải giảm. Việc xử lý ô như được truy cập đơn giản sẽ làm mất thông tin này. 

Trường hợp cạnh cuối cùng là khi tất cả các độ cao bằng nhau:```
1 2
1 1
1 1 1 2
```Câu trả lời là`-1`bởi vì không có chuyển động nào có thể tăng hoặc giảm độ cao. Việc triển khai bất cẩn chỉ kiểm tra khả năng tiếp cận bằng hình học sẽ tìm thấy đường dẫn không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thực hiện tìm kiếm đường đi ngắn nhất và tiếp tục thử các đường dẫn cho đến khi đạt được mục tiêu. Vì mỗi chuyển động đều tốn một chi phí nên BFS là điểm khởi đầu tự nhiên. BFS bình thường sẽ đặt các ô vào hàng đợi và đánh dấu chúng đã truy cập khi truy cập lần đầu. Từ mỗi ô, nó sẽ khám phá tất cả tám ô lân cận. 

Điều này đúng đối với các đường đi ngắn nhất trong lưới thông thường vì việc đến một ô muộn hơn không bao giờ tốt hơn việc đến được ô đó sớm hơn. Tuy nhiên, ở đây những bước đi trong tương lai phụ thuộc vào cách chúng tôi đến. Phiên bản Brute Force phải ghi nhớ toàn bộ chuỗi thay đổi độ cao, có nghĩa là số lượng trạng thái có thể tăng lên cùng với số lượng đường đi. Trong trường hợp xấu nhất, nó có thể khám phá một số tuyến đường theo cấp số nhân. 

Quan sát quan trọng là lịch sử duy nhất quan trọng là sự thay đổi độ cao cuối cùng. Chúng tôi không cần toàn bộ con đường. Nếu chúng ta đến một ô bằng cách đi lên cao hơn thì bước tiếp theo phải đi xuống thấp hơn. Nếu chúng ta đến bằng cách đi xuống thấp hơn thì bước đi tiếp theo phải đi lên cao hơn. Điều này biến bài toán thành bài toán đường đi ngắn nhất thông thường trên biểu đồ mở rộng. 

Mỗi ô trở thành hai trạng thái. Một trạng thái có nghĩa là bước đi tiếp theo phải tăng độ cao và trạng thái còn lại có nghĩa là bước đi tiếp theo phải giảm độ cao. BFS trên các trạng thái này hoạt động vì mọi chuyển đổi vẫn có chi phí. 

Tìm kiếm brute-force không thành công vì nó lưu giữ quá nhiều lịch sử. Tính năng nén trạng thái hoạt động vì tất cả các lịch sử kết thúc bằng cùng một ô và cùng hướng tiếp theo được yêu cầu đều hoạt động giống hệt nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo hai trạng thái BFS cho mỗi ô. Một trạng thái biểu thị rằng bước đi tiếp theo phải đi đến độ cao cao hơn và trạng thái còn lại biểu thị rằng bước đi tiếp theo phải đi đến độ cao thấp hơn. Mảng khoảng cách BFS lưu trữ các bước di chuyển tối thiểu cần thiết để đến từng trạng thái. 
2. Bắt đầu BFS từ ô bắt đầu ở cả hai trạng thái với khoảng cách bằng 0. Động thái đầu tiên có thể là tăng hoặc giảm, vì vậy cả hai khả năng đều có sẵn. 
3. Khi xử lý trạng thái yêu cầu di chuyển tăng dần, hãy kiểm tra tất cả tám ô lân cận. Một hàng xóm chỉ hợp lệ nếu độ cao của nó lớn hơn ô hiện tại. Di chuyển đến hàng xóm đó và lưu trữ trạng thái ngược lại vì bước đi tiếp theo phải giảm. 
4. Khi xử lý trạng thái yêu cầu di chuyển giảm dần, hãy thực hiện thao tác đối xứng. Chỉ những người hàng xóm có độ cao nhỏ hơn mới được phép và trạng thái tiếp theo yêu cầu tăng lên. 
5. Lần đầu tiên đạt được ô mục tiêu ở một trong hai trạng thái, khoảng cách đó là câu trả lời. BFS khám phá các trạng thái theo thứ tự khoảng cách tăng dần, do đó không có đường dẫn nào sau đó có thể ngắn hơn. 
6. Nếu cả hai trạng thái mục tiêu vẫn không thể truy cập được, hãy xuất`-1`. 

Lý do điều này có tác dụng là vì mọi tuyến đường hợp lệ có thể có đều có thể được biểu diễn dưới dạng một chuỗi các chuyển đổi hai trạng thái này. Nhà nước lưu trữ chính xác thông tin cần thiết để quyết định những động thái trong tương lai và không có gì hơn thế. BFS tìm thấy chuỗi chuyển tiếp ngắn nhất, đây là tuyến đường hợp lệ ngắn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    h = [list(map(int, input().split())) for _ in range(n)]
    x0, y0, xf, yf = map(int, input().split())
    x0 -= 1
    y0 -= 1
    xf -= 1
    yf -= 1

    from collections import deque

    dist = [[[-1] * 2 for _ in range(m)] for _ in range(n)]
    q = deque()

    dist[x0][y0][0] = 0
    dist[x0][y0][1] = 0
    q.append((x0, y0, 0))
    q.append((x0, y0, 1))

    directions = [
        (-1, -1), (-1, 0), (-1, 1),
        (0, -1),           (0, 1),
        (1, -1),  (1, 0),  (1, 1)
    ]

    while q:
        x, y, need_up = q.popleft()

        if x == xf and y == yf:
            print(dist[x][y][need_up])
            return

        for dx, dy in directions:
            nx = x + dx
            ny = y + dy

            if nx < 0 or nx >= n or ny < 0 or ny >= m:
                continue

            if need_up:
                if h[nx][ny] <= h[x][y]:
                    continue
            else:
                if h[nx][ny] >= h[x][y]:
                    continue

            nxt = 1 - need_up
            if dist[nx][ny][nxt] == -1:
                dist[nx][ny][nxt] = dist[x][y][need_up] + 1
                q.append((nx, ny, nxt))

    print(-1)

if __name__ == "__main__":
    solve()
```Mảng khoảng cách có ba chiều vì chỉ một vị trí không phải là trạng thái hoàn chỉnh. Kích thước cuối cùng thể hiện hướng cần thay đổi độ cao tiếp theo. 

Hàng đợi bắt đầu với cả hai trạng thái ở vị trí bắt đầu. Đây là phần tinh tế của việc thực hiện vì bước đầu tiên không bị hạn chế. Nếu chỉ có một trạng thái được chèn vào, các tuyến đường hợp lệ bắt đầu bằng kiểu di chuyển ngược lại sẽ bị bỏ qua. 

Kiểm tra chuyển đổi sử dụng so sánh nghiêm ngặt. Độ cao bằng nhau không phải là tăng hay giảm, vì vậy chúng không bao giờ được phép. Sau khi thực hiện một nước đi, trạng thái sẽ thay đổi bằng cách sử dụng`1 - need_up`. 

Câu trả lời có thể được trả về ngay lập tức khi mục tiêu bị xóa khỏi hàng đợi. BFS đảm bảo rằng các trạng thái rời khỏi hàng đợi theo thứ tự khoảng cách ngắn nhất. Nếu hàng đợi kết thúc, cả hai trạng thái mục tiêu đều không thể truy cập được. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 5
1 2 3 4 5
6 7 8 9 10
11 12 13 14 15
16 17 18 19 20
1 1 4 5
```Các trạng thái BFS quan trọng dọc theo một tuyến đường ngắn nhất là: 

| Bước | Tế bào | Yêu cầu di chuyển tiếp theo | Khoảng cách | 
| --- | --- | --- | --- | 
| 0 | (1,1) | tăng | 0 | 
| 1 | (2,2) | giảm | 1 | 
| 2 | (1,3) | tăng | 2 | 
| 3 | (2,4) | giảm | 3 | 
| 4 | (1,5) | tăng | 4 | 
| 5 | (2,5) | giảm | 5 | 
| 6 | (3,5) | tăng | 6 | 
| 7 | (4,4) | giảm | 7 | 
| 8 | (4,5) | tăng | 8 | 

Dấu vết này cho thấy tại sao trạng thái định hướng lại quan trọng. Cùng một ô có thể xuất hiện với các khả năng khác nhau trong tương lai tùy thuộc vào sự thay đổi độ cao trước đó. 

Đối với mẫu thứ hai:```
1 4
1 2 3 1
1 1 1 4
```Các trạng thái BFS là: 

| Bước | Tế bào | Yêu cầu di chuyển tiếp theo | Khoảng cách | 
| --- | --- | --- | --- | 
| 0 | (1,1) | tăng | 0 | 
| 0 | (1,1) | giảm | 0 | 
| 1 | (1,2) | giảm | 1 | 
| 2 | (1,3) | tăng | 2 | 

Từ ô thứ ba, khả năng tiếp tục duy nhất sẽ yêu cầu hàng xóm cao hơn, nhưng không tồn tại. Không thể đạt được mục tiêu nên câu trả lời là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi trạng thái trong số hai trạng thái trên mỗi ô được xử lý một lần và mỗi trạng thái sẽ kiểm tra tám trạng thái lân cận. | 
| Không gian | O(nm) | Mảng khoảng cách lưu trữ hai giá trị trên mỗi ô và hàng đợi lưu trữ các trạng thái có thể truy cập. | 

Với tối đa 250.000 ô, số lượng trạng thái BFS nhiều nhất là 500.000. Việc xử lý một số lượng hàng xóm không đổi trên mỗi trạng thái sẽ giữ cho giải pháp nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old_stdin

    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    m = int(next(it))
    h = [[int(next(it)) for _ in range(m)] for _ in range(n)]
    x0 = int(next(it)) - 1
    y0 = int(next(it)) - 1
    xf = int(next(it)) - 1
    yf = int(next(it)) - 1

    from collections import deque

    dist = [[[-1] * 2 for _ in range(m)] for _ in range(n)]
    q = deque([(x0, y0, 0), (x0, y0, 1)])
    dist[x0][y0][0] = dist[x0][y0][1] = 0

    dirs = [(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]

    while q:
        x, y, t = q.popleft()
        if x == xf and y == yf:
            return str(dist[x][y][t]) + "\n"
        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < m:
                if t == 1 and h[nx][ny] <= h[x][y]:
                    continue
                if t == 0 and h[nx][ny] >= h[x][y]:
                    continue
                nt = 1 - t
                if dist[nx][ny][nt] == -1:
                    dist[nx][ny][nt] = dist[x][y][t] + 1
                    q.append((nx, ny, nt))
    return "-1\n"

assert run("""4 5
1 2 3 4 5
6 7 8 9 10
11 12 13 14 15
16 17 18 19 20
1 1 4 5
""") == "8\n"

assert run("""1 4
1 2 3 1
1 1 1 4
1 1 1 2
""") == "-1\n"

assert run("""1 2
1 1
1 1 1 2
""") == "-1\n"

assert run("""2 2
1 2
3 4
1 1 2 2
""") == "1\n"

assert run("""2 3
5 4 3
2 1 0
1 3 2 1
""") == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một hàng không thể di chuyển | -1 | Chiều cao bằng nhau và thiếu chuyển tiếp | 
| Lưới tăng nhỏ | 1 | Chuyển động chéo trực tiếp | 
| Yêu cầu đường dẫn thay thế | 2 | Chuyển trạng thái đúng | 
| Di chuyển độ cao ngược | -1 | Kiểm tra tăng giảm nghiêm ngặt | 
| Có thể tiếp cận mục tiêu sau vài lần di chuyển | Khoảng cách hợp lệ | Truyền tải BFS đầy đủ | 

## Vỏ cạnh 

Đối với trường hợp có chiều cao bằng nhau:```
1 2
1 1
1 1 1 2
```Cả hai trạng thái bắt đầu đều được chèn vào, nhưng mọi ô lân cận đều có cùng độ cao. Các điều kiện chuyển đổi loại bỏ cả hai hướng, do đó hàng đợi trở nên trống và thuật toán in ra`-1`. 

Đối với trường hợp có thể truy cập cùng một ô với các tương lai khác nhau:```
2 2
1 2
3 4
1 1 2 2
```Di chuyển theo đường chéo tiếp cận mục tiêu bằng cách tăng độ cao. BFS lưu trữ trạng thái đó ở trạng thái mà bước tiếp theo sẽ cần phải giảm. Nó không nhầm lẫn điều này với việc đến giả định bằng cách giảm độ cao, điều này sẽ cho phép một sự tiếp tục khác. 

Đối với chuyển động ranh giới:```
1 3
1 2 3
1 1 1 3
```Các ô ở giữa và bên phải nằm trong lưới, nhưng quá trình truyền tải không bao giờ thử các ô lân cận không hợp lệ vì mọi chuyển đổi đều kiểm tra giới hạn hàng và cột trước tiên. Điều này ngăn cản việc truy cập ngoài phạm vi trong khi vẫn khám phá mọi hướng hợp pháp.
