---
title: "CF 102460A - Câu đố về giờ cao điểm"
description: "Chỉnh sửa Chúng tôi có một bảng 6 x 6 chứa tối đa 10 xe. Mỗi chiếc xe chiếm hai ô liên tiếp, như một chiếc ô tô, hoặc ba ô liên tiếp, như một chiếc xe tải. Một chiếc xe có một hướng cố định, ngang hoặc dọc và chỉ có thể trượt dọc theo hướng đó."
date: "2026-08-12T08:39:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "A"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 106
verified: true
draft: false
---

[CF 102460A - Câu đố về giờ cao điểm](https://codeforces.com/problemset/problem/102460/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
Chỉnh sửa 

#Hiểu vấn đề 

Chúng ta có một bảng 6 x 6 chứa tối đa 10 xe. Mỗi chiếc xe chiếm hai ô liên tiếp, như một chiếc ô tô, hoặc ba ô liên tiếp, như một chiếc xe tải. Một chiếc xe có một hướng cố định, ngang hoặc dọc và chỉ có thể trượt dọc theo hướng đó. Một slide đơn lẻ bằng một ô lưới tốn một bước. 

Xe 1 là xe màu đỏ. Lối ra nằm ngay bên phải hàng 3 nên xe màu đỏ cuối cùng phải chiếm cột 5 và 6 của hàng đó. Khi đến được hai ô đó, phải mất thêm hai bước nữa để di chuyển hoàn toàn ra ngoài bảng. Việc triển khai tham chiếu cho vấn đề này sử dụng chính xác cách giải thích này, trả về số lần di chuyển của bàn cờ cộng với hai khi ô tô màu đỏ đến hai ô cuối cùng. 

Đầu vào chỉ đơn giản là ma trận chiếm chỗ 6 x 6 hiện tại. Số 0 có nghĩa là một ô trống, trong khi giá trị dương xác định phương tiện đang chiếm giữ ô đó. Đầu ra là số lần di chuyển đơn ô tối thiểu cần thiết để đưa chiếc xe màu đỏ hoàn toàn ra ngoài lối ra. Nếu mọi giải pháp cần nhiều hơn 10 bước thì câu trả lời là`-1`. 

Bảng rất nhỏ nhưng không gian tìm kiếm thì không. Với tối đa 10 phương tiện, có thể có tới 20 phương tiện di chuyển một ô thông thường từ một trạng thái, vì mỗi phương tiện có khả năng di chuyển theo một trong hai hướng. Một tìm kiếm đơn giản coi mọi chuỗi di chuyển là khác biệt có thể đạt tới khoảng (20^{10}=10,240,000,000,000) chuỗi ở độ sâu 10. Điều đó vượt xa những gì có thể khám phá trong hai giây. 

Tấm bảng nhỏ cho chúng ta một giới hạn cấu trúc hữu ích. Câu trả lời chỉ cần tối đa là 10 và hai trong số đó sẽ bị buộc phải thực hiện khi xe màu đỏ đến vị trí lối ra. Do đó, chúng ta chỉ cần khám phá các trạng thái có thể đạt được trong tối đa 8 bước di chuyển thông thường trên tàu. Quan trọng hơn, nhiều chuỗi di chuyển khác nhau đạt đến cùng một cấu hình bảng. Khi đã đạt được cấu hình với số lần di chuyển tối thiểu có thể, việc khám phá lại cấu hình đó không thể tạo ra giải pháp tốt hơn. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Nếu ô tô màu đỏ đã có ở cột 5 và 6 thì đáp án là`2`, không`0`, vì chiếc xe vẫn nằm hoàn toàn bên trong bảng.```
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 1 1
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
```Đầu ra đúng là`2`. Việc triển khai coi việc đạt đến cột cuối cùng là mục tiêu đã hoàn thành sẽ trả về 0 không chính xác. 

Lỗi phổ biến thứ hai là chỉ kiểm tra một ô đích khi di chuyển xe. Hãy xem xét một chiếc xe tải chiếm ba ô liên tiếp. Di chuyển nó theo một vị trí đòi hỏi toàn bộ dấu chân ba ô mới phải được tự do, không chỉ ô ngay bên ngoài phía trước của nó. Nếu không, một chiếc xe tải có thể chồng lên một chiếc xe khác một cách bất hợp pháp. 

Cuối cùng, các phương tiện ở ranh giới bảng không thể di chuyển xa hơn ra ngoài bảng. Chỉ có xe màu đỏ mới được phép rời đi và thậm chí chỉ được đi qua lối ra được chỉ định. Đối với thử nghiệm phòng thủ, một bảng hoàn toàn bằng 0 không có xe màu đỏ và nên bị quá trình triển khai từ chối thay vì gây ra lỗi lập chỉ mục.```
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
```Đầu ra phòng thủ chính xác là`-1`. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thực hiện tìm kiếm brute-force theo chiều sâu. Từ bảng hiện tại, liệt kê mọi chuyển động hợp lệ của xe một ô, tiếp tục đệ quy từ mỗi bảng kết quả và dừng lại khi xe màu đỏ có thể rời đi. Điều này đúng vì mọi giải pháp hợp pháp đều là một chuỗi các chuyển động của phương tiện hợp pháp, nên việc liệt kê tất cả các chuỗi như vậy cuối cùng sẽ tìm ra mọi giải pháp. 

Vấn đề là các trạng thái lặp đi lặp lại. Giả sử một chuỗi di chuyển xe A sang trái và xe B sang phải, trong khi một chuỗi khác di chuyển xe B sang phải trước và xe A sang trái thứ hai. Cả hai chuỗi có thể tạo ra cùng một bảng. Tìm kiếm đệ quy đơn giản coi chúng là các nhánh không liên quan và khám phá mọi thứ bên dưới cả hai bản sao. Với tối đa 20 bước di chuyển có thể có trên mỗi trạng thái, một cây lực lượng vũ phu có độ sâu 10 có thể chứa theo thứ tự (20^{10}) hoặc khoảng 10,24 nghìn tỷ chuỗi hành động. 

Quan sát quan trọng là cấu hình câu đố hoàn toàn được xác định bởi vị trí của các phương tiện. Lịch sử được sử dụng để đạt được cấu hình đó không liên quan. Nếu BFS đạt lại cấu hình tương tự, lần truy cập thứ hai không bao giờ có thể tốt hơn vì BFS xử lý các trạng thái theo số lần di chuyển không giảm. Chúng tôi có thể đánh dấu từng cấu hình là đã truy cập và loại bỏ mọi lần xuất hiện sau đó. 

Điều này thay đổi tìm kiếm từ cây chuỗi di chuyển thành tìm kiếm biểu đồ trên các trạng thái bảng duy nhất. BFS chính xác là tìm kiếm đúng vì mỗi cạnh đại diện cho một bước, vì vậy lần đầu tiên gặp trạng thái mục tiêu, chúng tôi đã tìm thấy số bước tối thiểu. 

Có một quan sát hữu ích hơn về lối ra. Chúng ta không cần mô phỏng chiếc xe màu đỏ đang di chuyển ra ngoài bảng. Ngay khi nó chiếm hai ô cuối cùng của hàng 3 thì còn lại đúng hai bước. Vì tổng giới hạn là 10 nên BFS chỉ phải xem xét các bước di chuyển thông thường trong bảng lên đến độ sâu 8. Đây cũng là cách giải pháp được chấp nhận tiêu chuẩn xử lý tình trạng thoát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(20^{10}\cdot V)) | (O(10)) độ sâu đệ quy | Quá chậm | 
| BFS với các trạng thái đã truy cập | (O(S\cdot V\cdot L)) | (O(S\cdot V)) | Đã chấp nhận | 

Ở đây (V\le 10) là số lượng phương tiện, (L\le 3) là chiều dài phương tiện và (S) là số trạng thái riêng biệt đạt được trong vòng tám lần di chuyển trên tàu. Sự khác biệt quan trọng là (S) tính các cấu hình thay vì các chuỗi chuyển động, do đó các hoán vị lặp lại của cùng các bước chuyển sẽ bị loại bỏ. 

#Hướng dẫn thuật toán 

1. Phân tích bảng 6 x 6 và thu thập các ô của mọi phương tiện. Đối với mỗi phương tiện, hãy xác định hướng, chiều dài và tọa độ của ô đầu tiên. Đầu vào đảm bảo rằng mỗi phương tiện tạo thành một đoạn thẳng, do đó hướng và chiều dài của nó có thể được phục hồi trực tiếp từ các ô bị chiếm giữ. 
2. Chỉ thể hiện một trạng thái bằng vị trí neo của mỗi phương tiện. Mã hóa một mỏ neo`(row, column)`BẰNG`row * 6 + column`. Hướng và độ dài không bao giờ thay đổi nên chúng không cần được lưu trữ ở mọi trạng thái. 
3. Bắt đầu BFS từ bộ vị trí xe ban đầu. Đặt bộ dữ liệu này vào hàng đợi và vào một`visited`bộ. Số lớp BFS biểu thị số lần di chuyển trong bảng thông thường đã được thực hiện. 
4. Trước khi mở rộng trạng thái, hãy kiểm tra xem ô ngoài cùng bên phải của ô tô màu đỏ có phải là cột 5 hay không. Vì ô tô màu đỏ có chiều dài bằng hai nên điều này có nghĩa là nó chiếm cột 4 và 5, đây là hai ô bảng cuối cùng trước khi thoát ra. Hai nước đi thoát còn lại đưa ra câu trả lời`current_depth + 2`. 
5. Dừng mở rộng trạng thái sau độ sâu 8. Nếu lúc đó xe màu đỏ vẫn chưa đến được lối ra, mọi giải pháp đều cần ít nhất 9 lượt vào trong và hai lượt ra, vượt quá 10 bước cho phép. 
6. Xây dựng lại lưới sức chứa 6 x 6 cho trạng thái hiện tại. Điều này cho phép chúng tôi kiểm tra chuyển động mà không cần dựa vào bảng gốc vì các phương tiện khác có thể đã di chuyển kể từ cấu hình ban đầu. 
7. Đối với mỗi phương tiện, hãy thử di chuyển mỏ neo của nó một ô về phía trước và một ô về phía sau dọc theo hướng cố định của nó. Việc di chuyển ứng cử viên chỉ hợp pháp khi mọi ô của dấu chân mới của phương tiện đều nằm bên trong bảng và trống hoặc hiện đang bị chiếm giữ bởi cùng một phương tiện. 
8. Đối với mọi chuyển động hợp pháp, hãy tạo bộ giá trị neo. Nếu nó chưa được truy cập trước đó, hãy thêm nó vào cả hàng đợi và tập hợp đã truy cập. Vì BFS khám phá từng lớp trạng thái nên lần đầu tiên một trạng thái được chèn vào đã có khoảng cách tối thiểu so với cấu hình ban đầu. 
9. Nếu BFS kết thúc mà không đạt được mục tiêu xe đỏ, hãy quay lại`-1`. Mọi trạng thái có thể dẫn đến giải pháp trong vòng 10 bước đều đã được xem xét vì việc tìm kiếm bao gồm mọi cấu hình có thể tiếp cận được thông qua tám bước di chuyển thông thường. 

### Tại sao nó hoạt động 

Điều bất biến là mọi trạng thái trong BFS ở độ sâu (d) đều có thể truy cập được từ bảng ban đầu bằng chính xác (d) các bước di chuyển thông thường và không có trạng thái nào ở độ sâu đó có đường đi ngắn hơn. Điều này ban đầu đúng đối với trạng thái bắt đầu ở độ sâu bằng 0 và mỗi lần chuyển đổi sẽ thêm chính xác một lần di chuyển phương tiện hợp pháp. Bởi vì BFS xử lý độ sâu nhỏ hơn trước tiên nên việc loại bỏ trạng thái đã được truy cập không thể loại bỏ giải pháp ngắn hơn. 

Mọi chuyển động một bước hợp pháp đều được tạo cho mọi phương tiện theo cả hai hướng được phép, do đó mọi đường dẫn giải pháp có thể có trên xe đều xuất hiện trong biểu đồ BFS. Xe màu đỏ được coi là giải quyết chính xác khi đến cột 5 và 6, sau đó cố định hai bước thoát còn lại. Do đó, mục tiêu đầu tiên được tìm thấy có tổng số bước tối thiểu và nếu không tìm thấy mục tiêu nào qua độ sâu 8 thì không tồn tại giải pháp nào có tối đa 10 bước. 

#Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

H = W = 6
MAX_INNER_MOVES = 8

def solve(data: str) -> str:
    values = list(map(int, data.split()))
    if len(values) < 36:
        return "-1"

    board = values[:36]

    cells = {}
    for r in range(H):
        for c in range(W):
            v = board[r * W + c]
            if v != 0:
                cells.setdefault(v, []).append((r, c))

    if 1 not in cells:
        return "-1"

    ids = sorted(cells)

    # For each vehicle:
    # (dr, dc, length), where (dr, dc) is its movement direction.
    shape = []
    initial = []
    red_index = -1

    for idx, vid in enumerate(ids):
        pts = cells[vid]
        min_r = min(r for r, c in pts)
        max_r = max(r for r, c in pts)
        min_c = min(c for r, c in pts)
        max_c = max(c for r, c in pts)

        if min_r == max_r:
            dr, dc = 0, 1
            length = max_c - min_c + 1
            anchor = min_r * W + min_c
        else:
            dr, dc = 1, 0
            length = max_r - min_r + 1
            anchor = min_r * W + min_c

        shape.append((dr, dc, length))
        initial.append(anchor)

        if vid == 1:
            red_index = idx

    if red_index == -1:
        return "-1"

    initial = tuple(initial)

    # Build an occupancy grid for one state.
    def build_occupancy(state):
        occ = [0] * 36

        for i, anchor in enumerate(state):
            r = anchor // W
            c = anchor % W
            dr, dc, length = shape[i]

            for k in range(length):
                rr = r + dr * k
                cc = c + dc * k
                occ[rr * W + cc] = i + 1

        return occ

    queue = deque([initial])
    visited = {initial}
    depth = 0

    while queue and depth <= MAX_INNER_MOVES:
        level_size = len(queue)

        for _ in range(level_size):
            state = queue.popleft()

            # Vehicle 1 is the red car. Its rightmost cell must
            # be at column 5 before the final two exit steps.
            red_anchor = state[red_index]
            red_r = red_anchor // W
            red_c = red_anchor % W
            _, red_dc, red_len = shape[red_index]

            if red_dc == 1 and red_c + red_len - 1 == W - 1:
                return str(depth + 2)

            if depth == MAX_INNER_MOVES:
                continue

            occ = build_occupancy(state)

            for i, anchor in enumerate(state):
                r = anchor // W
                c = anchor % W
                dr, dc, length = shape[i]
                vehicle_id = i + 1

                for direction in (-1, 1):
                    nr = r + dr * direction
                    nc = c + dc * direction

                    # Check the complete new footprint.
                    valid = True
                    for k in range(length):
                        rr = nr + dr * k
                        cc = nc + dc * k

                        if not (0 <= rr < H and 0 <= cc < W):
                            valid = False
                            break

                        occupant = occ[rr * W + cc]
                        if occupant != 0 and occupant != vehicle_id:
                            valid = False
                            break

                    if not valid:
                        continue

                    new_state = list(state)
                    new_state[i] = nr * W + nc
                    new_state = tuple(new_state)

                    if new_state not in visited:
                        visited.add(new_state)
                        queue.append(new_state)

        depth += 1

    return "-1"

def main():
    data = sys.stdin.read()
    print(solve(data))

if __name__ == "__main__":
    main()
```Phần đầu tiên của quá trình triển khai sẽ xây dựng lại từng phương tiện từ ma trận đầu vào. Đối với mỗi phương tiện,`shape[i]`lưu trữ hướng chuyển động và chiều dài của nó, trong khi`initial[i]`lưu trữ ô trên cùng hoặc ngoài cùng bên trái của nó. Các thuộc tính này không bao giờ thay đổi trong quá trình giải đố, vì vậy trạng thái BFS chỉ cần các điểm neo. 

Bộ trạng thái đặc biệt thuận tiện cho việc băm. Python có thể lưu trữ một bộ dữ liệu trực tiếp trong một tập hợp, do đó không cần hàm băm bảng tùy chỉnh và không có nguy cơ xung đột từ sơ đồ băm số nguyên. Toàn bộ trạng thái động được biểu thị bằng tối đa 10 số nguyên nhỏ.`build_occupancy`chuyển đổi trạng thái vị trí xe nhỏ gọn trở lại thành bảng 36 ô. Việc xây dựng lại rẻ vì chỉ có 10 xe và mỗi xe có tối đa ba ô. 

Đối với mỗi phương tiện, mã sẽ thử cả hai biển báo dọc theo hướng được lưu trữ của nó. Xe ngang chỉ thay đổi cột, trong khi xe thẳng đứng chỉ thay đổi hàng. Điểm neo ứng cử viên được kiểm tra bằng cách lặp lại trên tất cả các ô mà phương tiện chiếm giữ sau khi di chuyển. Việc kiểm tra toàn bộ dấu chân này giúp tránh các lỗi nhỏ khi xe tải và các phương tiện chạm vào ranh giới. 

Bài kiểm tra mục tiêu sử dụng`red_c + red_len - 1 == 5`. Đối với chiếc xe màu đỏ,`red_len`là hai và`red_dc`là một nên đây chính xác là điều kiện xe chiếm cột 4 và 5. Sau đó, mã cộng thêm hai cho các nước đi cưỡng bức thoát ra. 

BFS được xử lý theo cấp độ thay vì lưu trữ khoảng cách dọc theo mọi trạng thái.`depth`là số lần di chuyển trong ván thông thường được biểu thị bằng cấp độ hiện tại. Khi độ sâu đạt đến 8, việc mở rộng thêm sẽ không hữu ích vì ngay cả một chiếc ô tô màu đỏ được giải quyết ngay lập tức cũng sẽ cần thêm hai bước nữa để rời đi. 

Không có vấn đề tràn số nguyên trong Python và tất cả tọa độ bảng đều được kiểm tra rõ ràng trước khi lập chỉ mục cho mảng chiếm chỗ. Trạng thái chỉ được sửa đổi sau khi một chuyển động ứng cử viên hoàn chỉnh đã được xác thực, do đó một chuyển động thất bại không thể làm hỏng cấu hình hiện tại. 

# Ví dụ đã hoạt động 

## Mẫu 1 

Mẫu đầu tiên có 8 xe. Xe màu đỏ xuất phát ở hàng 3 tại cột 2 và 3, trong khi xe 7 chặn đường về phía lối ra. Tìm kiếm có liên quan không tìm thấy cấu hình trong đó chiếc xe màu đỏ đi đến hai ô cuối cùng trong vòng tám bước di chuyển thông thường. 

| Độ sâu BFS | Vị trí chìa khóa xe đỏ | Quan sát liên quan | Kết quả | 
| --- | --- | --- | --- | 
| 0 | cột 2, 3 | Xe 7 chặn lối thoát | Mở rộng | 
| 1 | khác nhau | Xe khác đi được nhưng xe đỏ vẫn không thoát được | Mở rộng | 
| 2 | khác nhau | BFS tiếp tục đi qua các trạng thái duy nhất | Mở rộng | 
| 3 | khác nhau | Không có trạng thái mục tiêu | Mở rộng | 
| 4 | khác nhau | Không có trạng thái mục tiêu | Mở rộng | 
| 5 | khác nhau | Không có trạng thái mục tiêu | Mở rộng | 
| 6 | khác nhau | Không có trạng thái mục tiêu | Mở rộng | 
| 7 | khác nhau | Không có trạng thái mục tiêu | Mở rộng | 
| 8 | khác nhau | Màu đỏ không bao giờ tới cột 5, 6 | Dừng lại | 
| Cuối cùng | chưa đạt | Bất kỳ giải pháp nào cũng cần ít nhất 11 bước |`-1`| 

Phần quan trọng của dấu vết này là giới hạn độ sâu. Đạt đến độ sâu 8 mà không đưa ô tô màu đỏ vào hai ô cuối cùng là đủ để chứng minh rằng không tồn tại nghiệm nào có tổng cộng tối đa 10 bước. 

## Mẫu 2 

Mẫu thứ hai có một giải pháp ngắn. Trình tự hữu ích bắt đầu bằng việc di chuyển xe 6 sang trái một ô. Điều này giải phóng ô phía trên cần thiết để di chuyển xe 7 lên trên. Sau đó, xe màu đỏ được phép rẽ phải hai lần rồi đi qua lối ra thêm hai bước nữa. 

| Bước | Xe đã di chuyển | Cột xe đỏ | Thay đổi trạng thái quan trọng | 
| --- | --- | --- | --- | 
| 0 | không | 3, 4 | Xe 7 khối cột 5 | 
| 1 | 6 trái | 3, 4 | Ô phía trên xe 7 trở nên miễn phí | 
| 2 | 7 lên | 3, 4 | Cột 5 của hàng 3 trở nên miễn phí | 
| 3 | 1 đúng | 4, 5 | Xe đỏ vào 2 ô cuối cùng | 
| 4 | 1 đúng | 5, bên ngoài | Bước thoát đầu tiên | 
| 5 | 1 đúng | bên ngoài | Bước thoát thứ hai | 

Bảng tính chuyển động của xe màu đỏ vào lối ra như những bước đi thông thường sau khi đến vị trí mục tiêu. Thay vào đó, việc triển khai dừng ở bước 2 của BFS, nhận ra rằng vẫn còn hai bước thoát và trả về`2 + 2 = 4`nếu sử dụng các vị trí dựa trên số 0 từ dấu vết này. Trong mẫu thực tế, ô tô màu đỏ bắt đầu ở xa hơn một cột bên trái, do đó, hai ca trong xe cộng với thiết lập bắt buộc sẽ tạo ra câu trả lời tổng cộng là`6`. Đầu ra mẫu chính thức là`6`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(S\cdot V\cdot L)) | Mỗi trạng thái duy nhất xây dựng một lưới chiếm chỗ 36 ô và thử hai bước di chuyển cho mỗi phương tiện, kiểm tra tối đa ba ô cho mỗi ứng viên | 
| Không gian | (O(S\cdot V)) | Hàng đợi và tập hợp đã truy cập lưu trữ tối đa (S) trạng thái duy nhất, mỗi trạng thái chứa tối đa 10 vị trí xe | 

Ở đây (V\le10), (L\le3) và (S) là số lượng cấu hình riêng biệt đạt được trong tám lớp BFS đầu tiên. Kích thước bảng cố định làm cho công việc trên mỗi trạng thái trở nên rất nhỏ. Quan trọng hơn, tập hợp được truy cập sẽ loại bỏ sự trùng lặp to lớn hiện diện trong việc liệt kê chuỗi chuyển động mạnh mẽ. Đây là lý do tại sao BFS lại hữu ích đối với giới hạn đã cho mặc dù vấn đề cơ bản là tìm kiếm trong không gian trạng thái. 

# Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

H = W = 6
MAX_INNER_MOVES = 8

def solve(data: str) -> str:
    values = list(map(int, data.split()))
    if len(values) < 36:
        return "-1"

    board = values[:36]

    cells = {}
    for r in range(H):
        for c in range(W):
            v = board[r * W + c]
            if v != 0:
                cells.setdefault(v, []).append((r, c))

    if 1 not in cells:
        return "-1"

    ids = sorted(cells)
    shape = []
    initial = []
    red_index = -1

    for idx, vid in enumerate(ids):
        pts = cells[vid]
        min_r = min(r for r, c in pts)
        max_r = max(r for r, c in pts)
        min_c = min(c for r, c in pts)
        max_c = max(c for r, c in pts)

        if min_r == max_r:
            dr, dc = 0, 1
            length = max_c - min_c + 1
        else:
            dr, dc = 1, 0
            length = max_r - min_r + 1

        shape.append((dr, dc, length))
        initial.append(min_r * W + min_c)

        if vid == 1:
            red_index = idx

    if red_index == -1:
        return "-1"

    initial = tuple(initial)

    def build_occupancy(state):
        occ = [0] * 36
        for i, anchor in enumerate(state):
            r = anchor // W
            c = anchor % W
            dr, dc, length = shape[i]

            for k in range(length):
                rr = r + dr * k
                cc = c + dc * k
                if 0 <= rr < H and 0 <= cc < W:
                    occ[rr * W + cc] = i + 1
        return occ

    q = deque([initial])
    visited = {initial}
    depth = 0

    while q and depth <= MAX_INNER_MOVES:
        for _ in range(len(q)):
            state = q.popleft()

            red_anchor = state[red_index]
            red_c = red_anchor % W
            _, red_dc, red_len = shape[red_index]

            if red_dc == 1 and red_c + red_len - 1 == W - 1:
                return str(depth + 2)

            if depth == MAX_INNER_MOVES:
                continue

            occ = build_occupancy(state)

            for i, anchor in enumerate(state):
                r = anchor // W
                c = anchor % W
                dr, dc, length = shape[i]

                for direction in (-1, 1):
                    nr = r + dr * direction
                    nc = c + dc * direction

                    valid = True
                    for k in range(length):
                        rr = nr + dr * k
                        cc = nc + dc * k

                        if not (0 <= rr < H and 0 <= cc < W):
                            valid = False
                            break

                        occupant = occ[rr * W + cc]
                        if occupant not in (0, i + 1):
                            valid = False
                            break

                    if not valid:
                        continue

                    nxt = list(state)
                    nxt[i] = nr * W + nc
                    nxt = tuple(nxt)

                    if nxt not in visited:
                        visited.add(nxt)
                        q.append(nxt)

        depth += 1

    return "-1"

def run(inp: str) -> str:
    return solve(inp).strip()

sample1 = """\
2 2 0 0 0 7
3 0 0 5 0 7
3 1 1 5 0 7
3 0 0 5 0 0
4 0 0 0 8 8
4 0 6 6 6 0
"""

sample2 = """\
0 2 0 6 6 0
0 2 0 0 7 0
0 3 1 1 7 0
0 3 4 4 8 0
0 5 5 5 8 0
0 0 0 0 0 0
"""

assert run(sample1) == "-1", "sample 1"
assert run(sample2) == "6", "sample 2"

one_vehicle = """\
0 0 0 0 0 0
0 0 0 0 0 0
1 1 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
"""
assert run(one_vehicle) == "6", "minimum-size board"

already_at_exit = """\
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 1 1
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
"""
assert run(already_at_exit) == "2", "red car already at exit"

all_zero = """\
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
"""
assert run(all_zero) == "-1", "no red vehicle"

ten_vehicles = """\
2 2 3 3 4 4
5 5 6 6 7 7
1 1 0 0 0 0
0 0 0 0 0 0
8 8 9 9 10 10
0 0 0 0 0 0
"""
assert run(ten_vehicles) == "6", "ten vehicles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một ô tô màu đỏ ở cột 1 và 2 |`6`| Số lượng phương tiện tối thiểu và khoảng cách thoát hiểm cơ bản | 
| Xe màu đỏ đã có ở cột 5 và 6 |`2`| Xử lý trạng thái mục tiêu và hai bước thoát bắt buộc | 
| Bảng hoàn toàn trống |`-1`| Xử lý phòng thủ khi vắng xe 1 | 
| Mười phương tiện độc lập |`6`| Số lượng xe tối đa và đại diện tiểu bang | 
| Mẫu 1 |`-1`| Câu đố không thể giải được trong giới hạn mười bước | 
| Mẫu 2 |`6`| Giải pháp ngắn yêu cầu nhiều tương tác giữa xe | 

# Vỏ cạnh 

Trường hợp cạnh thứ nhất là xe màu đỏ đã ở vị trí thoát ra. Đầu vào là```
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 1 1
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
```Điểm neo của chiếc ô tô màu đỏ là cột 4 trong chỉ mục dựa trên số 0 và độ dài của nó là hai, vì vậy ô ngoài cùng bên phải của nó là cột 5. BFS ngay lập tức nhận ra mục tiêu ở độ sâu 0 và quay trở lại`0 + 2 = 2`. Xe không cần di chuyển thông thường nhưng xe vẫn cần hai bước để rời khỏi ván. 

Trường hợp cạnh thứ hai liên quan đến một chiếc xe chạm vào ranh giới. Giả sử một ô tô nằm ngang chiếm cột 5 và 6 của một hàng không có lối ra. Cố gắng di chuyển nó sang phải sẽ tạo ra một điểm neo ở cột 6 và ô thứ hai sẽ nằm ngoài bảng. Ứng viên bị từ chối khi kiểm tra dấu chân nhìn thấy tọa độ bên ngoài`[0, 5]`. Thuật toán không bao giờ coi những phương tiện không có màu đỏ là có khả năng rời khỏi bảng. 

Trường hợp cạnh thứ ba là một chiếc xe tải. Một chiếc xe tải thẳng đứng có neo`(2, 3)`chiếm`(2,3)`,`(3,3)`, Và`(4,3)`. Di chuyển nó xuống dưới tạo ra`(3,3)`,`(4,3)`, Và`(5,3)`, vì vậy cả ba ô đều phải hợp lệ. Việc thực hiện kiểm tra từng cái một. Điều này tránh việc chấp nhận chuyển động trong đó ô dẫn đầu tự do nhưng một phần khác của xe tải sẽ chồng lên chướng ngại vật. 

Trường hợp cạnh thứ tư là đầu vào phòng thủ hoàn toàn bằng không.```
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
```Xe 1 không có mục nào nên người giải trả về`-1`trước khi xây dựng bất kỳ trạng thái BFS nào. Đầu vào này nằm ngoài đặc tả câu đố chính thức vì chiếc xe màu đỏ phải tồn tại, nhưng việc xử lý nó giúp việc triển khai trở nên mạnh mẽ và ngăn ngừa lỗi tra cứu ngẫu nhiên. 

Trường hợp cạnh cuối cùng là các trạng thái lặp lại. Nếu một chiếc xe di chuyển sang trái và sau đó di chuyển sang phải, thì lại đạt được cấu hình ban đầu. Bởi vì trạng thái ban đầu đã được đặt trong`visited`, chuyển động ngược lại không xảy ra lần thứ hai. Tổng quát hơn, bất kỳ hai chuỗi di chuyển khác nhau nào tạo ra cùng một vị trí phương tiện sẽ chuyển sang một trạng thái BFS. Đây là lý do chính khiến việc tìm kiếm vẫn mang tính thực tế thay vì hoạt động giống như cây brute-force có kích thước (20^{10}).
