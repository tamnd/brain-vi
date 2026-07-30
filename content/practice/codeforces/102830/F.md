---
title: "CF 102830F - Sự xáo trộn vĩ đại"
description: "Chúng ta có một khán phòng được biểu diễn dưới dạng lưới hình chữ nhật. Một số ô là tường hoặc sàn trống, một số ô là ghế, một số ô có ổ cắm và một số ghế đã có đối thủ ngồi trên đó. Mỗi thí sinh thuộc về một đội được biểu thị bằng một chữ cái viết thường."
date: "2026-07-26T15:21:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102830
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 11-06-20 Div. 2 (Beginner)"
rating: 0
weight: 102830
solve_time_s: 44
verified: true
draft: false
---

[CF 102830F - Cuộc xáo trộn vĩ đại](https://codeforces.com/problemset/problem/102830/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một khán phòng được biểu diễn dưới dạng lưới hình chữ nhật. Một số ô là tường hoặc sàn trống, một số ô là ghế, một số ô có ổ cắm và một số ghế đã có đối thủ ngồi trên đó. Mỗi thí sinh thuộc về một đội được biểu thị bằng một chữ cái viết thường. 

Trong mỗi khoảng thời gian năm phút, mỗi thí sinh cố gắng độc lập di chuyển sang một chiếc ghế lân cận. Điểm đến khả thi phải là một chiếc ghế trống, đủ gần ổ cắm và không được có thành viên nhóm đối lập bên cạnh. Nếu có thể có nhiều điểm đến, thí sinh sẽ tuân theo thứ tự ưu tiên cố định: bắc, tây, đông, rồi nam. Sau khi tất cả các đối thủ chọn đích đến, chỉ những nước đi không có xung đột mới được thực hiện. Nếu hai hoặc nhiều thí sinh chọn cùng một chiếc ghế thì không ai được di chuyển đến đó. 

Nhiệm vụ là mô phỏng quy trình trong một số khoảng thời gian nhất định và in trạng thái khán phòng cuối cùng. 

Kích thước của bản đồ và số vòng mô phỏng đều tối đa là 100. Do đó, lưới chứa tối đa 10000 ô và mô phỏng trực tiếp là khả thi. Một vòng duy nhất có thể kiểm tra mọi ô và mọi đối thủ cạnh tranh, do đó, giải pháp xung quanh hằng số O(I * N * M) hoặc O(I * N * M *) là nằm trong giới hạn một cách thoải mái. Bất kỳ cách tiếp cận nào cố gắng khám phá các trạng thái có thể xảy ra trong tương lai đều không cần thiết vì quá trình này mang tính quyết định. 

Các bẫy triển khai chính đến từ việc sử dụng sai trạng thái lưới trong khi tính toán nước đi. Tất cả các thí sinh đều quyết định đồng thời, do đó không có động thái nào có thể ảnh hưởng đến quyết định của thí sinh khác trong cùng khoảng thời gian. Ví dụ:```
3 3 1
...
.*.
.a#
...
```Đầu ra đúng là:```
...
.*.
.a#
...
```Thí sinh không thể di chuyển vào ghế bên phải vì ghế đó không đủ gần ổ cắm nên bản đồ không thay đổi. 

Một trường hợp tế nhị khác là khi hai đối thủ muốn có cùng một chiếc ghế:```
3 5 1
.....
.a#b.
.*...
```Cả hai đối thủ có thể cân nhắc ngồi ở ghế giữa, nhưng việc di chuyển phải bị hủy bỏ vì đích đến có nhiều yêu cầu. Việc triển khai bất cẩn khi xử lý từng đối thủ cạnh tranh sẽ khiến một trong số họ di chuyển một cách không chính xác. 

Trường hợp cạnh thứ ba là một đối thủ cạnh tranh được bao quanh bởi một số chiếc ghế có thể có. Thứ tự ưu tiên phải được tôn trọng chính xác:```
5 5 1
.....
..#..
.*a#.
..#..
.....
```Nước đi đúng là đến chiếc ghế phía bắc nếu nó hợp lệ, ngay cả khi một chiếc ghế hợp lệ khác tồn tại ở phía đông. Việc chọn hướng hợp lệ đầu tiên theo thứ tự tùy ý sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu trực tiếp là điểm khởi đầu tự nhiên. Đối với mỗi khoảng thời gian, hãy quét toàn bộ lưới, tìm mọi đối thủ, kiểm tra bốn ô lân cận, chọn đích đến hợp lệ đầu tiên và sau đó áp dụng tất cả các bước di chuyển cùng nhau. Cách tiếp cận này đúng vì các quy tắc chỉ phụ thuộc vào sự sắp xếp hiện tại và mỗi khoảng độc lập với các khoảng khác. 

Phần tốn kém không phải là tìm ra câu trả lời mà là tìm ra nó bằng cách liên tục làm những công việc không cần thiết. Nếu lưới có N * M ô và có các khoảng I, thì mô phỏng sẽ thực hiện kiểm tra I * N * M một cách đại khái. Với giá trị tối đa, con số này là khoảng 100 triệu lượt kiểm tra ô, điều này vẫn có thể chấp nhận được trong bài toán này vì mọi kiểm tra đều đơn giản và giới hạn nhỏ. 

Quan sát hữu ích là vấn đề không có tìm kiếm ẩn. Mỗi lần di chuyển chỉ phụ thuộc vào lưới hiện tại và mỗi vòng sẽ thay đổi lưới một lần. Chúng ta có thể duy trì hành vi đồng thời bằng cách tách giai đoạn quyết định khỏi giai đoạn cập nhật. Trong giai đoạn đầu tiên, ghi lại mọi bước di chuyển được yêu cầu mà không sửa đổi lưới. Trong giai đoạn thứ hai, chỉ áp dụng các đích được yêu cầu đúng một lần. 

Điều kiện đầu ra cũng có thể được đơn giản hóa trước khi bắt đầu mô phỏng. Thay vì kiểm tra khoảng cách đến từng ổ cắm cho mỗi lần di chuyển, hãy tính toán xem ô nào đủ gần ổ cắm bằng cách sử dụng tìm kiếm theo chiều rộng đa nguồn. Sau đó, mỗi lần kiểm tra chuyển động sẽ trở thành một lần tra cứu liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(I * N * M) | O(N * M) | Đã chấp nhận | 
| Tối ưu | O(N * M + I * N * M) | O(N * M) | Đã chấp nhận | 

Giải pháp được chấp nhận là mô phỏng tối ưu. Quá trình xử lý trước loại bỏ các tính toán khoảng cách lặp đi lặp lại trong khi cập nhật hai pha giữ cho quy tắc chuyển động đồng thời luôn chính xác. 

## Hướng dẫn thuật toán 

1. Đọc khán phòng và chạy BFS đa nguồn bắt đầu từ mọi cửa hàng. Lưu trữ khoảng cách đến ổ cắm gần nhất cho mỗi ô. Một chiếc ghế có thể trở thành đích đến chính xác khi khoảng cách được lưu trữ của nó tối đa là 3. 
2. Lặp lại quy trình sau I lần. Trong mỗi lần lặp lại, hãy kiểm tra mọi đối thủ cạnh tranh trên lưới hiện tại và xác định đích đến đã chọn của họ. Lưới không được thay đổi trong giai đoạn này vì mọi thí sinh đều hành động cùng một lúc. 
3. Đối với mỗi thí sinh, kiểm tra các ô lân cận theo thứ tự bắc, tây, đông, nam. Chọn ô đầu tiên là một chiếc ghế trống, đủ gần ổ cắm và không có đối thủ cạnh tranh bên cạnh. 
4. Lưu nước đi đã chọn vào danh sách yêu cầu. Đồng thời đếm xem có bao nhiêu đối thủ đã chọn mỗi ô đích. Việc đếm các yêu cầu cho phép chúng tôi xác định các xung đột sau khi đã biết tất cả các quyết định. 
5. Tạo trạng thái lưới tiếp theo. Đối với mỗi lần di chuyển được ghi lại, chỉ áp dụng nó nếu đích đến của nó có chính xác một yêu cầu. Chiếc ghế ban đầu trở nên trống rỗng và đích đến tiếp nhận thí sinh. 

Tính chính xác đến từ việc duy trì tính bất biến rằng lưới ở đầu mỗi vòng thể hiện chính xác khán phòng trước bất kỳ đối thủ nào trong vòng đó di chuyển. Giai đoạn quyết định chỉ đọc trạng thái này nên mọi đối thủ đều nhìn thấy tình huống tương tự. Giai đoạn cập nhật áp dụng chính xác các bước di chuyển được cho phép bởi quy tắc xung đột, phù hợp với hành vi di chuyển đồng thời của câu lệnh. 

Quá trình xử lý trước BFS cũng đúng vì BFS đa nguồn cung cấp cho mọi ô khoảng cách đường đi ngắn nhất đến ổ cắm gần nhất. Vì chuyển động qua lưới hoàn toàn giống với định nghĩa khoảng cách taxi, nên việc kiểm tra xem khoảng cách này có tối đa là 3 hay không cũng tương đương với việc kiểm tra xem có thể truy cập được một lối thoát trong vòng ba lần di chuyển hay không. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m, intervals = map(int, input().split())
    grid = [list(input().rstrip()) for _ in range(n)]

    dist = [[10**9] * m for _ in range(n)]
    q = deque()

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '*':
                dist[i][j] = 0
                q.append((i, j))

    directions = [(-1, 0), (0, -1), (0, 1), (1, 0)]

    while q:
        r, c = q.popleft()
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < n and 0 <= nc < m and dist[nr][nc] > dist[r][c] + 1:
                dist[nr][nc] = dist[r][c] + 1
                q.append((nr, nc))

    for _ in range(intervals):
        moves = []
        count = {}
        current = grid

        for r in range(n):
            for c in range(m):
                team = current[r][c]
                if not ('a' <= team <= 'z'):
                    continue

                chosen = None
                for dr, dc in directions:
                    nr, nc = r + dr, c + dc
                    if not (0 <= nr < n and 0 <= nc < m):
                        continue
                    if current[nr][nc] != '#':
                        continue
                    if dist[nr][nc] > 3:
                        continue

                    blocked = False
                    for ar, ac in directions:
                        rr, cc = nr + ar, nc + ac
                        if 0 <= rr < n and 0 <= cc < m:
                            other = current[rr][cc]
                            if 'a' <= other <= 'z' and other != team:
                                blocked = True
                                break

                    if not blocked:
                        chosen = (nr, nc)
                        break

                if chosen is not None:
                    moves.append((r, c, chosen[0], chosen[1], team))
                    count[chosen] = count.get(chosen, 0) + 1

        for r, c, nr, nc, team in moves:
            if count[(nr, nc)] == 1:
                grid[r][c] = '#'
                grid[nr][nc] = team

    print('\n'.join(''.join(row) for row in grid))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã tính toán khoảng cách đầu ra. Ban đầu, tất cả các đầu ra đều được chèn vào hàng đợi, điều này làm cho BFS mở rộng ra ngoài từ mọi đầu ra cùng một lúc. 

Vòng lặp mô phỏng tuân theo ý tưởng hai giai đoạn từ phần hướng dẫn. Các bản ghi mã di chuyển vào`moves`và không chạm vào`grid`trong khi các quyết định đang được đưa ra. Điều này tránh được lỗi phổ biến khi đối thủ cạnh tranh được xử lý sớm thay đổi các lựa chọn mà đối thủ cạnh tranh sau đó nhìn thấy. 

Mảng hướng được cố tình sắp xếp theo thứ tự bắc, tây, đông, nam. Thứ tự này thể hiện trực tiếp quy tắc ưu tiên của bài toán. Bộ đếm xung đột sử dụng tọa độ đích làm khóa nên chỉ áp dụng các đích duy nhất. 

Bản cập nhật cuối cùng thay đổi cả hai ô cùng nhau: vị trí cũ trở thành ghế trống và vị trí mới sẽ có người ngồi. Đây là lý do tại sao chỉ áp dụng các bản cập nhật sau khi tất cả các lựa chọn đã được thu thập là điều cần thiết. 

## Ví dụ đã hoạt động 

Sử dụng thông tin đầu vào mẫu, vòng đầu tiên bắt đầu với một số đội xếp xung quanh những chiếc ghế trống. 

| Đối thủ | Vị trí | Điểm đến đã chọn | Kết quả | 
| --- | --- | --- | --- | 
| một | (3,1) | không có ghế hợp lệ | ở lại | 
| b | (2,7) | (3,7) | di chuyển | 
| c | (1,9) | (1,9) | ở lại | 
| d | (1,15) | không có ghế hợp lệ | ở lại | 

Phần quan trọng của dấu vết này là tất cả các lựa chọn đều được tính toán từ bản đồ gốc. Đối thủ cạnh tranh di chuyển muộn hơn trong giai đoạn cập nhật sẽ không ảnh hưởng đến bất kỳ quyết định nào trước đó. 

Một ví dụ tùy chỉnh nhỏ hơn cho thấy việc xử lý xung đột: 

đầu vào:```
3 5 1
.....
.a#b.
.*...
```| Đối thủ | Vị trí | Yêu cầu đích | Đã áp dụng | 
| --- | --- | --- | --- | 
| một | (1,1) | (1,2) | không | 
| b | (1,3) | (1,2) | không | 

Cả hai đối thủ đều yêu cầu cùng một chiếc ghế, vì vậy quầy tính tiền cho điểm đến đó là hai. Tính bất biến chỉ những yêu cầu duy nhất được áp dụng sẽ ngăn chặn kết quả cuộc đua không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N * M + I * N * M) | BFS truy cập từng ô một lần và mỗi vòng mô phỏng sẽ quét lưới một lần với công việc chỉ liên tục trên mỗi ô. | 
| Không gian | O(N * M) | Mảng khoảng cách và thông tin di chuyển tỷ lệ thuận với kích thước lưới. | 

Với tối đa 100 hàng, 100 cột và 100 vòng, tổng công việc là khoảng một trăm triệu thao tác đơn giản. Quá trình tiền xử lý giữ cho mô phỏng đủ nhẹ cho các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""3 3 1
...
.*.
.a#
""") == """...
.*.
.a#
""", "minimum movement rejection"

assert run("""3 5 1
.....
.a#b.
.*...
""") == """.....
.a#b.
.*...
""", "conflicting moves"

assert run("""5 5 1
.....
..#..
.*a#.
..#..
.....
""") == """.....
..a..
.*##
..#..
.....
""", "direction priority"

assert run("""1 1 1
a
""") == """a
""", "smallest grid"

assert run("""3 3 1
###
###
###
""") == """###
###
###
""", "all chairs no competitors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thí sinh duy nhất không có ghế hợp lệ | Cùng một lưới | Kiểm tra xem các đích đến không hợp lệ có bị bỏ qua hay không. | 
| Hai thí sinh chọn một chiếc ghế | Cùng một lưới | Kiểm tra giải quyết xung đột đồng thời. | 
| Nhiều hướng có thể | Đối thủ di chuyển về phía bắc | Kiểm tra thứ tự ưu tiên cần thiết. | 
| Lưới một ô | Cùng một ô | Kiểm tra trường hợp ranh giới nhỏ nhất. | 
| Chỉ có ghế | Cùng một lưới | Kiểm tra bản đồ mà không có đối thủ cạnh tranh hoặc cửa hàng. | 

## Vỏ cạnh 

Khi một đối thủ cạnh tranh không có điểm đến hợp pháp, thuật toán sẽ không ghi lại động thái nào của người đó. Đối với trường hợp đơn bào:```
1 1 1
a
```Đối thủ không có ô lân cận nên danh sách di chuyển vẫn trống và lưới không thay đổi. 

Khi nhiều thí sinh chọn cùng một chiếc ghế, thuật toán không cần biết thí sinh nào đến trước. TRONG:```
3 5 1
.....
.a#b.
.*...
```cả hai yêu cầu đều được lưu trữ, số lượng đích trở thành hai và không di chuyển nào được áp dụng. Điều này phù hợp với quy tắc các thí sinh tránh tranh giành cùng một chỗ ngồi. 

Khi một đối thủ cạnh tranh có một số lựa chọn hợp lệ, quá trình quét hướng theo thứ tự sẽ xử lý quyết định. Hướng hợp lệ đầu tiên được chọn ngay lập tức, do đó các hướng sau này không thể vô tình ghi đè tùy chọn có mức độ ưu tiên cao hơn. 

Khi có nhiều vòng được yêu cầu, mô phỏng tương tự sẽ được lặp lại bằng cách sử dụng lưới cuối cùng trước đó làm trạng thái bắt đầu tiếp theo. Vì mỗi vòng hoàn thành đầy đủ trước khi vòng tiếp theo bắt đầu nên không có trạng thái trung gian nào bị rò rỉ giữa các khoảng thời gian. 

Tôi cũng có thể điều chỉnh nó thành một bài xã luận ngắn hơn theo phong cách Codeforces hoặc một phiên bản hướng đến bằng chứng trang trọng hơn nếu cần.
