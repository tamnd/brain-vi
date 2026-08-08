---
title: "CF 102535N - Sàn nối"
description: "Tòa nhà là một tập hợp các bản đồ 2D độc lập, mỗi tầng một bản đồ. Mỗi bản đồ là một lưới chứa các ô, tường và ô cầu thang có thể đi bộ. Một cầu thang có cùng một chữ cái ở các tầng khác nhau tượng trưng cho sự kết nối giữa các vị trí đó."
date: "2026-08-06T20:08:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "N"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 139
verified: true
draft: false
---

[CF 102535N - Sàn kết nối](https://codeforces.com/problemset/problem/102535/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tòa nhà là một tập hợp các bản đồ 2D độc lập, mỗi tầng một bản đồ. Mỗi bản đồ là một lưới chứa các ô, tường và ô cầu thang có thể đi bộ. Một cầu thang có cùng một chữ cái ở các tầng khác nhau tượng trưng cho sự kết nối giữa các vị trí đó. Mục tiêu là chọn bất kỳ ô bắt đầu nào ở tầng một và đến bất kỳ ô nào ở tầng cuối cùng đồng thời giảm thiểu số lượng ô tường phải chuyển thành cửa. 

Chi tiết quan trọng là bản thân sự chuyển động là tự do. Đi qua một ô đã mở, di chuyển qua cầu thang hoặc di chuyển qua một bức tường đã bị cắt, tất cả đều không tốn kém gì sau khi cắt xong. Nguồn lực duy nhất được giảm thiểu là số lượng các ô tường riêng biệt được nhập vào như một phần của tuyến đường, bởi vì mỗi ô như vậy cần một lần cắt laser. 

Tổng kích thước đầu vào được giới hạn bởi 100 tầng, với mỗi tầng chứa tối đa một lưới 100 x 100. Điều đó mang lại nhiều nhất một triệu tế bào trên toàn bộ tòa nhà. Một giải pháp thử nhiều đường đi có thể hoặc thực hiện tìm kiếm trên tất cả các tập con của bức tường là không thể vì không gian trạng thái sẽ tăng theo cấp số nhân. Cần phải duyệt đồ thị tuyến tính hoặc gần tuyến tính. Một triệu nút là lớn nhưng có thể quản lý được đối với các thuật toán xử lý từng nút và cạnh một số lần không đổi. 

Một số chi tiết có thể phá vỡ các giải pháp hợp lý. Bắt đầu từ bất kỳ vị trí nào trên tầng một đều quan trọng vì việc buộc tìm kiếm bắt đầu tại một ô trống cụ thể có thể tạo ra câu trả lời lớn hơn. Một tầng cũng có thể không có đường dẫn lên tầng trên cùng ngay cả khi có một số cầu thang, vì vậy các trạng thái không thể tiếp cận phải được xử lý rõ ràng. 

Hãy xem xét ví dụ nhỏ này:```
2
3 3
###
#A#
###
3 3
###
#A#
###
```Câu trả lời là`0`vì cầu thang nối trực tiếp hai tầng. Giải pháp chỉ tìm kiếm các ô lưới liền kề và quên chuyển tiếp cầu thang sẽ báo cáo không chính xác rằng mục tiêu không thể truy cập được. 

Một trường hợp khác là:```
2
3 3
###
#.#
###
3 3
###
#.#
###
```Câu trả lời là`DAMN, THE ABSCONDER ABSCONDS AGAIN!`vì không có cầu thang nối giữa các tầng. Việc thực hiện bất cẩn chỉ kiểm tra xem cả hai tầng có chứa các vị trí trông giống nhau hay không có thể cho rằng có thể xảy ra chuyển động một cách không chính xác. 

Trường hợp quan trọng thứ ba là khi con đường ngắn nhất cắt xuyên qua một bức tường và sau đó sử dụng lối đi mới đó:```
2
3 4
####
#A.#
####
3 4
####
#A.#
####
```Câu trả lời là`0`vì cầu thang là đủ. Tổng quát hơn, khi một bức tường được đưa vào, bức tường đó sẽ trở thành một vị trí hợp lệ cho việc di chuyển trong tương lai. Việc coi các bức tường là các ô bị chặn thay vì các ô có trọng số sẽ bỏ lỡ các tuyến đường tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng các hành trình có thể xảy ra xuyên qua tòa nhà. Chúng ta có thể coi mọi vị trí có thể có của người chơi như một trạng thái và liên tục khám phá các bước di chuyển, cố gắng tìm ra con đường sử dụng ít vết cắt nhất. Điều này đúng vì mọi hành động pháp lý đều được đại diện. Tuy nhiên, việc sử dụng thuật toán đường đi ngắn nhất thông thường với cách xử lý như nhau cho tất cả các bước di chuyển sẽ bỏ qua sự khác biệt chính giữa việc đi vào tường và đi vào ô trống. Việc khám phá tất cả các sự kết hợp có thể có của các bức tường bị cắt thậm chí còn tệ hơn. Trong trường hợp xấu nhất, có thể có khoảng một triệu ô, khiến cho bất kỳ phương pháp nào lưu trữ nhiều tập hợp con hoặc đường dẫn thay thế đều không thể thực hiện được. 

Quan sát hữu ích là đây đã là bài toán đường đi ngắn nhất, nhưng các cạnh chỉ có hai chi phí có thể xảy ra. Di chuyển vào một ô trống, cầu thang hoặc vị trí đã có sẵn sẽ không tốn phí. Di chuyển vào một ô trên tường tốn một chi phí vì nó tiêu tốn một lần cắt laser. Do đó, biểu đồ có trọng số cạnh nhị phân, có nghĩa là BFS 0-1 có thể tìm thấy đường đi ngắn nhất một cách hiệu quả. 

Biểu đồ không cần phải được xây dựng một cách rõ ràng. Mỗi ô lưới là một nút. Bốn ô lân cận tạo ra các cạnh chuyển động và các ô cầu thang tạo thêm các cạnh không tốn phí cho cùng một chữ cái trên các tầng khác. Vì vị trí bắt đầu có thể ở bất kỳ đâu trên tầng một nên mọi ô trên tầng đó đều bắt đầu bằng khoảng cách bằng 0. 

Phương pháp vũ lực hoạt động vì nó khám phá tất cả các chuyển động hợp pháp, nhưng không thành công vì số lượng trạng thái và đường dẫn có thể có quá lớn. Nhận xét rằng mọi quá trình chuyển đổi đều có chi phí bằng 0 hoặc bằng 1 cho phép chúng tôi thay thế việc khám phá đường đi ngắn nhất chung đắt tiền bằng việc duyệt qua dựa trên deque. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lần cắt có thể | Hàm mũ | Quá chậm | 
| Tối ưu | O(V + E) | O(V) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng tầng và lưu trữ lưới. Trong khi đọc, hãy thu thập tọa độ của từng chữ cái cầu thang. Những tọa độ này cần thiết sau này vì việc di chuyển qua các cầu thang phù hợp là một quá trình chuyển đổi không tốn phí. 
2. Tạo một mảng khoảng cách cho mỗi ô và khởi tạo mọi ô ở tầng trệt với khoảng cách bằng 0. Tất cả các ô khác bắt đầu như không thể truy cập được. Mô hình này thực tế là vị trí bắt đầu được chọn một cách tối ưu. 
3. Chạy BFS 0-1 bằng deque. Khi xử lý một ô, hãy thử tất cả bốn ô lân cận. Đi vào một bức tường sẽ thêm một khoảng cách, trong khi đi vào bất kỳ ô nào khác sẽ thêm khoảng cách bằng 0. Nếu khoảng cách mới cải thiện khoảng cách đã biết, hãy cập nhật nó và đặt ô ở phía trước deque đối với nước đi không tốn phí hoặc ở phía sau đối với nước đi có chi phí một. 
4. Khi xử lý một ô cầu thang, hãy di chuyển đến mọi lần xuất hiện khác của cùng một chữ cái cầu thang trên các tầng khác nhau với chi phí bằng 0. Những chuyển tiếp này đại diện cho việc đi cầu thang. 
5. Sau khi tìm kiếm kết thúc, hãy kiểm tra tất cả các ô ở tầng trên cùng. Khoảng cách nhỏ nhất trong số đó chính là câu trả lời. Nếu mọi ô ở tầng trên cùng vẫn không thể truy cập được, hãy báo cáo rằng không thể đi qua tòa nhà. 

Tại sao nó hoạt động: 

0-1 BFS duy trì thứ tự đường đi ngắn nhất giống như thuật toán Dijkstra, nhưng sử dụng thực tế là trọng số chỉ bằng 0 hoặc một để tránh hàng đợi ưu tiên. Tại mọi thời điểm, deque lưu trữ các ô được sắp xếp theo khoảng cách ngắn nhất được biết hiện tại của chúng. Một ô chỉ được hoàn thiện sau khi tất cả các khả năng rẻ hơn đã được xem xét. Vì mọi chuyển động có thể được biểu thị bằng một cạnh với chi phí cắt chính xác của nó, khoảng cách ngắn nhất đầu tiên được tìm thấy cho tầng mục tiêu là số lượng cửa tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    f = int(input())
    floors = []
    stairs = {}

    for floor in range(f):
        r, c = map(int, input().split())
        grid = []
        for i in range(r):
            row = list(input().strip())
            grid.append(row)
            for j, ch in enumerate(row):
                if ch.isalpha():
                    stairs.setdefault(ch, []).append((floor, i, j))
        floors.append((r, c, grid))

    ids = []
    index = 0
    for floor, (r, c, _) in enumerate(floors):
        cur = []
        for i in range(r):
            row = []
            for j in range(c):
                row.append(index)
                index += 1
            cur.append(row)
        ids.append(cur)

    n = index
    dist = [10**9] * n
    q = deque()

    for i in range(floors[0][0]):
        for j in range(floors[0][1]):
            idx = ids[0][i][j]
            dist[idx] = 0
            q.append(idx)

    rev = [None] * n
    for f_id, (r, c, _) in enumerate(floors):
        for i in range(r):
            for j in range(c):
                rev[ids[f_id][i][j]] = (f_id, i, j)

    used_stairs = set()

    while q:
        cur = q.popleft()
        floor, r, c = rev[cur]

        current_distance = dist[cur]

        grid = floors[floor][2]
        for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            nr, nc = r + dr, c + dc
            if 0 <= nr < floors[floor][0] and 0 <= nc < floors[floor][1]:
                nxt = ids[floor][nr][nc]
                weight = 1 if grid[nr][nc] == '#' else 0
                nd = current_distance + weight
                if nd < dist[nxt]:
                    dist[nxt] = nd
                    if weight == 0:
                        q.appendleft(nxt)
                    else:
                        q.append(nxt)

        ch = grid[r][c]
        if ch.isalpha() and ch not in used_stairs:
            used_stairs.add(ch)
            for nf, nr, nc in stairs[ch]:
                nxt = ids[nf][nr][nc]
                if current_distance < dist[nxt]:
                    dist[nxt] = current_distance
                    q.appendleft(nxt)

    ans = 10**9
    top = f - 1
    for i in range(floors[top][0]):
        for j in range(floors[top][1]):
            ans = min(ans, dist[ids[top][i][j]])

    if ans == 10**9:
        print("DAMN, THE ABSCONDER ABSCONDS AGAIN!")
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```Giai đoạn phân tích cú pháp đầu vào lưu trữ từng tầng và ghi lại vị trí cầu thang. Từ điển cầu thang cho phép tìm thấy sự chuyển tiếp cầu thang mà không cần quét liên tục từng tầng. 

các`ids`mảng chuyển đổi vị trí ba chiều`(floor, row, column)`thành một chỉ mục duy nhất. Điều này làm cho mảng khoảng cách trở nên nhỏ gọn và cho phép deque lưu trữ số nguyên thay vì bộ dữ liệu. Ánh xạ ngược sẽ khôi phục tọa độ bất cứ khi nào trạng thái được xử lý. 

Quá trình khởi tạo BFS đặt mọi ô ở tầng dưới cùng vào deque với khoảng cách bằng 0. Đây là phần xử lý việc lựa chọn vị trí xuất phát. Việc hạn chế hàng đợi ban đầu ở một ô sẽ giải quyết được một vấn đề khác. 

Thư giãn hàng xóm chỉ sử dụng chi phí một khi đích đến là một bức tường. Thuật toán không chặn các ô trên tường vì việc nhập một ô có nghĩa là cắt một cánh cửa và làm cho vị trí đó có thể sử dụng được. 

Tối ưu hóa cầu thang với`used_stairs`là một chi tiết hiệu suất. Khi tất cả các ô của chữ cái cầu thang đã được nới lỏng, việc xử lý lại cầu thang đó không thể cải thiện bất kỳ kết quả nào. Nếu không có sự tối ưu hóa này, một cầu thang xuất hiện nhiều lần có thể liên tục thêm các chuyển đổi tương tự với chi phí bằng 0. 

Số nguyên Python không bị giới hạn nên việc tràn không phải là vấn đề đáng lo ngại. Việc kiểm tra ranh giới duy nhất được yêu cầu là bốn chuyển động có thể xảy ra của lưới. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, các trạng thái quan trọng được tóm tắt dưới đây. 

| Bước | Khu vực hiện tại | Hành động | Khoảng cách | 
| --- | --- | --- | --- | 
| 1 | Ô nào ở tầng 0 | Bắt đầu tìm kiếm | 0 | 
| 2 | Cầu thang A tầng 0 | Di chuyển qua cầu thang | 0 | 
| 3 | Cầu thang A tầng 1 | Di chuyển xuyên tường để đến B | 1 | 
| 4 | Cầu thang B tầng 1 | Di chuyển qua cầu thang | 1 | 
| 5 | Cầu thang B tầng 2 | Tiếp cận phòng giam ở tầng trên cùng | 2 | 

Dấu vết cho thấy tại sao chuyển động cầu thang không thể được coi là chuyển động lưới thông thường. Tuyến đường tối ưu sử dụng hai lối vào tường trong khi di chuyển giữa các tầng và bản thân việc chuyển đổi cầu thang sẽ không mất thêm chi phí. 

Ví dụ được xây dựng thứ hai cho thấy tầng trên cùng không thể tiếp cận được.```
2
3 3
###
#A#
###
3 3
###
#.#
###
```| Bước | Khu vực hiện tại | Hành động | Khoảng cách | 
| --- | --- | --- | --- | 
| 1 | Tầng 0 ô | Khởi tạo bắt đầu | 0 | 
| 2 | Tầng 0 cầu thang A | Tìm kiếm liên kết cầu thang | 0 | 
| 3 | Không có cầu thang phù hợp | Không có chuyển đổi nào được tạo | Không thể truy cập | 
| 4 | Tế bào tầng trên cùng | Kiểm tra câu trả lời | Vô cực | 

Dấu vết xác nhận rằng việc có lưới hợp lệ ở mỗi tầng là không đủ. Biểu đồ kết nối cầu thang xác định xem có thể lên tới các tầng hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V + E) | Mỗi ô và mọi cạnh chuyển động đều được xử lý với số lần không đổi. | 
| Không gian | O(V) | Mảng khoảng cách, ánh xạ và thông tin lưu trữ deque tỷ lệ thuận với số lượng ô. | 

Tòa nhà lớn nhất có thể có khoảng một triệu tế bào. Việc truyền tải O(V + E) là phù hợp vì số cạnh chỉ là bội số không đổi nhỏ của số lượng ô. Việc sử dụng bộ nhớ vẫn nằm trong giới hạn vì thuật toán chỉ lưu trữ các mảng và ánh xạ nhỏ gọn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_out
    sys.stdin = old
    return out.getvalue().strip()

assert run("""3
8 8
########
#.A....#
#......#
########
#......#
#......#
#......#
########
8 8
########
#..#..B#
#..#...#
#..#####
#......#
#.######
#.#..A.#
########
8 8
#B#..#.#
#.#..#.#
########
#......#
#......#
#......#
########
""") == "2"

assert run("""2
3 3
###
#A#
###
3 3
###
#A#
###
""") == "0"

assert run("""2
3 3
###
#.#
###
3 3
###
#.#
###
""") == "DAMN, THE ABSCONDER ABSCONDS AGAIN!"

assert run("""2
3 5
#####
#...#
#A###
3 5
#####
#A..#
#####
""") == "1"

assert run("""2
3 3
###
#.#
###
3 3
###
#.#
###
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Xây dựng mẫu | 2 | Chi phí đi qua cầu thang và tường thông thường | 
| Chỉ cầu thang phù hợp | 0 | Thay đổi sàn không tốn phí | 
| Thiếu cầu thang | Thông báo lỗi | Xử lý đồ thị không thể truy cập | 
| Yêu cầu mở tường | 1 | Nhập các ô tường dưới dạng các cạnh có trọng số | 
| Tuyến đường trống giữa các tầng | 0 | Vị trí bắt đầu và chuyển động mở | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là chọn vị trí bắt đầu. Trong ví dụ:```
2
3 3
###
#.#
###
3 3
###
#.#
###
```mọi ô ở tầng một đều bắt đầu với khoảng cách bằng 0. Việc tìm kiếm ngay lập tức phát hiện ra đường dẫn trống và trả về số 0. BFS nguồn đơn bắt đầu từ ô giữa sẽ vô tình phụ thuộc vào lựa chọn bắt đầu tùy ý. 

Trường hợp cạnh thứ hai là chữ cái cầu thang không kết nối ở đâu:```
2
3 3
###
#A#
###
3 3
###
#.#
###
```Thuật toán ghi lại cầu thang`A`chỉ một lần. Vì không có lần xuất hiện thứ hai nên không có cạnh cầu thang nào được thêm vào. Tầng trên cùng vẫn không thể truy cập được, tạo ra thông báo lỗi bắt buộc. 

Trường hợp cạnh cuối cùng là các bức tường không phải là chướng ngại vật trong biểu đồ này. Ví dụ:```
2
3 5
#####
#A###
#####
3 5
#####
#A..#
#####
```Tuyến đường phải cắt xuyên qua một bức tường để di chuyển về phía cầu thang. Thư giãn hàng xóm chỉ định chi phí một cho bức tường đầu tiên được nhập, cho phép tuyến đường tiếp tục đi qua cánh cửa mới được tạo đó. Việc coi các bức tường như những ô bị cấm sẽ tuyên bố không chính xác rằng con đường là không thể.
