---
title: "CF 102392K - Robot bị mắc kẹt"
description: "Chúng ta có một lưới hình chữ nhật ba chiều có kích thước m × n × p. Một ô là mảnh vỡ rắn, không gian trống, ô khởi đầu R của robot hoặc thiết bị dịch chuyển T. Robot chiếm một ô trống và ban đầu được gắn vào một số mảnh vỡ rắn lân cận."
date: "2026-08-10T19:43:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 372
verified: true
draft: false
---

[CF 102392K - Robot bị mắc kẹt](https://codeforces.com/problemset/problem/102392/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật ba chiều có kích thước`m × n × p`. Ô là mảnh vụn rắn, khoảng trống, ô khởi đầu của robot`R`, hoặc máy dịch chuyển`T`. Robot chiếm một ô trống và ban đầu được gắn vào một số mảnh vỡ rắn lân cận. Mục tiêu là để đạt được`T`với ít nước đi nhất trong khi kết thúc một nước đi gắn liền với đống đổ nát. 

Điều bất thường là trọng lực có thể được chọn độc lập trước mỗi lần di chuyển. Có sáu hướng trọng lực có thể có, một hướng dương và một hướng âm cho mỗi trục tọa độ. Đối với hướng đã chọn, ánh sáng mặt trời sẽ đến từ phía đối diện. Một vị trí chỉ có thể sử dụng được nếu không có khối rắn nào nằm giữa nó và mặt trời. Một chuyển động có thể là một chuyển động ngang thông thường dọc theo một bề mặt, một cú nhảy khỏi bề mặt cao hơn, sau đó là một cú ngã hoặc một cú rơi thuần túy sau khi thay đổi hướng trọng lực. Mỗi lần di chuyển đều tốn chính xác một. 

Đầu vào chứa tối đa 500 ô dọc theo mỗi chiều, do đó âm lượng có thể đạt tới`500^3 = 125,000,000`tế bào. Điều đó ngay lập tức loại trừ việc lưu trữ hoặc tìm kiếm biểu đồ có một đỉnh trên mỗi ô lưới. Ngay cả việc truyền tải theo thời gian tuyến tính trên tất cả các ô cũng đã là tỷ lệ của chính đầu vào, trong khi mọi thứ bậc hai trong âm lượng là hoàn toàn không thể. Thuật toán hữu ích phải đọc toàn bộ đầu vào trong`O(mnp)`nhưng sau đó nó cần phải làm việc trên một biểu diễn nhỏ hơn nhiều. 

Các trường hợp cạnh chính xuất phát từ sự khác biệt giữa một ô trống và một ô có vị trí nghỉ hợp lệ. Ví dụ, hãy xem xét sự sắp xếp một chiều sau đây.```
3 1 1
R*T
```Robot được đỡ bởi khối đặc ở giữa, nhưng không có chiều ngang thứ hai để nó có thể di chuyển. Nó không thể đơn giản đi qua tế bào rắn để tiếp cận`T`, vậy câu trả lời là`-1`. Một con đường ngắn nhất ngây thơ qua các ô trống liền kề sẽ coi máy dịch chuyển tức thời là có thể truy cập được một cách không chính xác. 

Một trường hợp tinh tế khác là rơi qua máy dịch chuyển. Coi như:```
2 4 1
R*
T-
--
*-
```Robot có thể chọn trọng lực theo hướng tăng dần`y`. Cột của nó chứa một khối rắn tại`y = 3`, vậy từ`y = 0`nó rơi vào`y = 2`. Người dịch chuyển tức thời tại`y = 1`được thông qua trong mùa thu đó và không kích hoạt. Việc triển khai bất cẩn để kiểm tra từng ô bị ngã sẽ báo cáo thành công không chính xác. Câu trả lời đúng là`-1`. 

Trường hợp cạnh thứ ba là ánh sáng mặt trời. TRONG```
3 3 1
-R-
-*-
-T-
```có mảnh vỡ giữa robot và thiết bị dịch chuyển để có các hướng liên quan và robot không thể sắp xếp một chuỗi di chuyển được chiếu sáng hợp lệ. Câu trả lời là`-1`. Việc coi mọi ô trống lân cận là có thể di chuyển đã bỏ qua thực tế là toàn bộ quá trình di chuyển phải diễn ra trong khi cả hai điểm cuối đều được chiếu sáng. 

Cuối cùng, ranh giới lưới có vấn đề. Một khối đặc trên mặt phẳng tọa độ đầu tiên hoặc cuối cùng có thể được nhìn thấy chỉ từ một phía. Vị trí bên ngoài lưới không bao giờ là vị trí hợp lệ của robot, do đó bề mặt tại tọa độ`0`không thể tạo ra vị trí nghỉ tại tọa độ`-1`, và tương tự ở biên đối diện. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là coi mỗi ô lưới trống là một vị trí robot có thể có và thử tất cả sáu hướng trọng lực từ đó. Đối với mỗi hướng, chúng ta có thể quét qua lưới cho đến khi tìm thấy khối rắn đầu tiên, xác định xem robot có thể di chuyển hay rơi, sau đó chạy BFS. Điều này đúng vì mọi chuyển động vật lý đều có thể được mô phỏng trực tiếp. 

Vấn đề là không gian trạng thái và quá trình quét lặp lại. có thể có`125,000,000`các ô và BFS trên chúng đã yêu cầu khoảng 125 triệu trạng thái. Nếu mỗi trạng thái kiểm tra sáu hướng và mỗi hướng quét tới 500 ô thì trường hợp xấu nhất đạt khoảng`6 · 125,000,000 · 500 = 375,000,000,000`kiểm tra tế bào cơ bản Ngay cả việc lưu trữ toàn bộ mảng đã truy cập cũng lớn một cách không cần thiết. 

Quan sát làm thay đổi vấn đề là, đối với một hướng cố định của ánh sáng mặt trời, chỉ có khối rắn nhìn thấy được đầu tiên trong mỗi đường là quan trọng. Xem xét trọng lực dọc theo tích cực`z`. Đối với mọi`(x,y)`, cho phép`zMin[x,y]`là nhỏ nhất`z`chứa một khối rắn. Mọi khối rắn xa hơn dọc theo đường đó sẽ bị ẩn vĩnh viễn theo hướng này. Robot chỉ có thể hoàn thành một nước đi ngay trước khối nhìn thấy được như vậy, vì vậy nhiều nhất`m · n`có các vị trí liên quan theo hướng này. 

Công trình tương tự hoạt động theo hướng ngược lại bằng cách sử dụng`zMax`, và cho cả hai trục còn lại. Trên tất cả sáu hướng có nhiều nhất`2(np + mp + mn)`các trạng thái bề mặt. Với tất cả các kích thước tối đa là 500, đây là tối đa 1,5 triệu trạng thái, thay vì 125 triệu ô lưới. 

Bộ đệm độ sâu là thông tin hình học hoàn chỉnh mà BFS cần. Đối với lực hấp dẫn dương dọc theo một trục, robot ở tọa độ`q`chỉ có thể di chuyển khi khối rắn đầu tiên ở tọa độ`s`với`q + 1 <= s`. Nếu như`q + 1 < s`, robot đang bị treo và chỉ cần một động tác sẽ thả nó xuống`s - 1`. Nếu như`q + 1 = s`, nó nằm trên bề mặt và có thể di chuyển ngang sang một bề mặt nhìn thấy được khác. Chiều âm là đối xứng, sử dụng`q - 1 >= s`và hạ cánh tại`s + 1`. 

Phương pháp brute-force hoạt động vì nó cố gắng mô phỏng các quy tắc vật lý theo đúng nghĩa đen, nhưng không thành công vì hầu hết các ô không bao giờ có thể trở thành trạng thái neo. Nhận xét rằng chỉ khối hiển thị đầu tiên trên mỗi dòng mới cho phép chúng tôi loại bỏ gần như toàn bộ khối ba chiều trước khi thực hiện BFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(mnp · max(m,n,p))`trong mô phỏng trực tiếp |`O(mnp)`| Quá chậm | 
| Tối ưu |`O(mnp + mnp)`để xây dựng đầu vào/độ sâu và`O(mn + mp + np)`cho BFS |`O(mn + mp + np)`| Đã chấp nhận | 

Bản thân đầu vào chứa`Θ(mnp)`các ký tự, do đó thuật ngữ xử lý đầu vào tuyến tính không thể tránh được một cách tiệm cận. 

## Hướng dẫn thuật toán 

1. Đọc từng hàng một trong lưới và xác định vị trí của robot và thiết bị dịch chuyển. Chúng ta không cần giữ lại lưới ký tự ba chiều ban đầu. Đối với mỗi hàng, hãy ghi lại vị trí nào chứa các khối rắn dưới dạng một bitset nhỏ gọn, vì việc xây dựng bộ đệm độ sâu sau này chỉ cần biết liệu một ô có phải là khối rắn hay không. 
2. Xây dựng`xmin`Và`xmax`cho mọi`(y,z)`đường kẻ.`xmin[y,z]`là chất rắn đầu tiên`x`phối hợp và`xmax[y,z]`là cái cuối cùng Vị trí cố định đầu tiên và cuối cùng của một hàng có thể được tìm thấy trực tiếp bằng cách tìm kiếm chuỗi byte, vì vậy việc này không yêu cầu vòng lặp Python trên tất cả`m`nhân vật. 
3. Đối với mọi cố định`(z,x)`dòng, tính toán`ymin`Và`ymax`. Xử lý các hàng của mỗi lớp từ trên xuống dưới để`ymin`, và từ dưới lên trên cho`ymax`. Một tập hợp các cột chưa được giải quyết cho phép chúng ta chỉ định mỗi cột một lần theo mỗi hướng. 
4. Đối với mọi cố định`(x,y)`dòng, tính toán`zmin`Và`zmax`. Xử lý các lớp từ thấp đến cao cho`zmin`và từ cao xuống thấp cho`zmax`. Một lần nữa, một bitset chưa được giải quyết đảm bảo rằng mọi`(x,y)`vị trí được chỉ định nhiều nhất một lần cho mỗi hướng. 
5. Coi trạng thái BFS như một vị trí neo cùng với hướng của trọng lực được sử dụng cho lần di chuyển trước đó. Trạng thái không cần lưu trữ tọa độ ba chiều đầy đủ. Đối với hướng cố định và tọa độ ngang, bộ đệm độ sâu xác định duy nhất tọa độ neo ngay bên cạnh khối rắn nhìn thấy được. 
6. Khởi tạo BFS bằng cách xem xét tất cả sáu hướng trọng lực có thể có trực tiếp từ tọa độ ban đầu của robot. Điều này hơi khác so với việc chèn robot vào biểu đồ trạng thái bình thường. Robot ban đầu được neo, nhưng nó có thể không ở gần bề mặt nhìn thấy được đối với hướng trọng lực mới được chọn. Nếu nó chỉ đơn thuần là bị treo, nước đi đầu tiên của nó là cú ngã tương ứng. Mọi trạng thái được chèn vào BFS đều đã là điểm cuối của một nước đi hợp lệ. 
7. Khi xử lý trạng thái neo, hãy thử tất cả sáu hướng trọng lực. Đối với hướng đã chọn, hãy tìm khối đặc đầu tiên trên đường thẳng tương ứng. Nếu không có khối như vậy tồn tại, robot sẽ rơi vào không gian nên hướng đó không tạo ra chuyển động. Nếu tọa độ hiện tại không được chiếu sáng thì hướng đó cũng không tạo ra chuyển động nào. 
8. Nếu vị trí hiện tại đã liền kề với khối nhìn thấy được thì robot đang nghỉ ngơi. Nó có thể di chuyển đến một trong bốn vị trí lân cận ở hai trục vuông góc với trọng lực. Đối với hướng trọng lực dương, bề mặt đích ít nhất phải cách xa trục trọng lực bằng bề mặt hiện tại. Đối với hướng âm, nó không được xa hơn dọc theo trục đó. Điểm cuối khi đó là ô ngay trước khối hiển thị của đích. 
9. Nếu robot được chiếu sáng nhưng không ở gần khối nhìn thấy được thì nó đang bị treo. Chuyển động duy nhất có thể thực hiện được dưới lực hấp dẫn này là rơi trực tiếp xuống bề mặt nhìn thấy được. Điểm cuối là ô ngay trước khối theo hướng trọng lực. 
10. Bất cứ khi nào một điểm đến được tạo ra, hãy kiểm tra xem tọa độ của nó có bằng với máy dịch chuyển hay không. Việc kiểm tra chỉ được thực hiện ở điểm cuối cùng của một bước di chuyển, không bao giờ được thực hiện trên các ô trung gian được vượt qua trong một cú ngã. Nếu nó là mới, hãy chèn trạng thái hướng cụ thể của nó vào BFS. 
11. BFS khám phá các trạng thái với số lượng bước di chuyển không giảm vì mỗi lần chuyển đổi thể hiện chính xác một bước di chuyển vật lý. Lần đầu tiên một điểm cuối bằng với thiết bị dịch chuyển tức thời, khoảng cách của nó là câu trả lời tối thiểu có thể xảy ra. 

Tại sao nó hoạt động: đối với mọi hướng trọng lực, bộ đệm độ sâu tương ứng sẽ xác định chính xác khối mảnh vỡ đầu tiên có thể nhìn thấy từ mặt trời dọc theo mỗi đường. Bất kỳ khối nào đằng sau khối đầu tiên đó không bao giờ có thể ảnh hưởng đến một động thái hợp pháp theo hướng đó. Mọi chuyển động hợp pháp của robot được chiếu sáng đều là rơi xuống khối nhìn thấy được đầu tiên này hoặc chuyển động ngang từ bề mặt nghỉ sang bề mặt nhìn thấy khác. Đây chính xác là những chuyển đổi do BFS tạo ra. Ngược lại, mọi chuyển tiếp được tạo ra đều thỏa mãn các điều kiện chiếu sáng, hỗ trợ và chuyển động từ bài toán. Do đó, biểu đồ BFS chứa chính xác tất cả các bước di chuyển hợp pháp giữa các điểm cuối được neo. Vì mỗi cạnh có giá một, BFS trả về số bước di chuyển tối thiểu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    m, n, p = map(int, input().split())

    # For the six directions:
    # +x, -x use planes of size n*p
    # +y, -y use planes of size m*p
    # +z, -z use planes of size m*n
    sx = n * p
    sy = m * p
    sz = m * n

    xmin = array('h', [-1]) * sx
    xmax = array('h', [-1]) * sx
    ymin = array('h', [-1]) * sy
    ymax = array('h', [-1]) * sy
    zmin = array('h', [-1]) * sz
    zmax = array('h', [-1]) * sz

    rx = ry = rz = -1
    tx = ty = tz = -1

    # Translate every '*' to byte 1 and everything else to byte 0.
    trans = bytearray(256)
    trans[ord('*')] = 1
    trans = bytes(trans)

    # Bit i of a row-bitset is stored at bit 8*i.
    # This wastes 7 bits per cell, but makes extraction very simple.
    row_lane_mask = ((1 << (8 * m)) - 1) // 255

    layers = []

    for z in range(p):
        layer = []

        for y in range(n):
            row = input().strip()

            pos = row.find(b'R')
            if pos != -1:
                rx, ry, rz = pos, y, z

            pos = row.find(b'T')
            if pos != -1:
                tx, ty, tz = pos, y, z

            first = row.find(b'*')
            if first != -1:
                last = row.rfind(b'*')
                idx = z * n + y
                xmin[idx] = first
                xmax[idx] = last

            bits = int.from_bytes(row.translate(trans), 'little')
            layer.append(bits)

        layers.append(layer)

        # yMin for this z-layer.
        unseen = row_lane_mask
        base = z * m

        for y, bits in enumerate(layer):
            new = bits & unseen

            while new:
                low = new & -new
                x = (low.bit_length() - 1) >> 3
                ymin[base + x] = y
                unseen ^= low
                new ^= low

        # yMax for this z-layer.
        unseen = row_lane_mask

        for y in range(n - 1, -1, -1):
            new = layer[y] & unseen

            while new:
                low = new & -new
                x = (low.bit_length() - 1) >> 3
                ymax[base + x] = y
                unseen ^= low
                new ^= low

    # zMin and zMax use one lane per (x,y).
    cells = m * n
    global_lane_mask = ((1 << (8 * cells)) - 1) // 255

    # zMin.
    unseen = global_lane_mask

    for z in range(p):
        layer = layers[z]
        for y, bits in enumerate(layer):
            shifted = bits << (8 * y * m)
            new = shifted & unseen

            while new:
                low = new & -new
                cell = (low.bit_length() - 1) >> 3
                zmin[cell] = z
                unseen ^= low
                new ^= low

    # zMax.
    unseen = global_lane_mask

    for z in range(p - 1, -1, -1):
        layer = layers[z]
        for y, bits in enumerate(layer):
            shifted = bits << (8 * y * m)
            new = shifted & unseen

            while new:
                low = new & -new
                cell = (low.bit_length() - 1) >> 3
                zmax[cell] = z
                unseen ^= low
                new ^= low

    # The original 3D grid is no longer needed.
    del layers

    mins = (xmin, ymin, zmin)
    maxs = (xmax, ymax, zmax)
    dims = (m, n, p)
    planes = (sx, sy, sz)

    # Use one fixed stride for the six direction-specific state spaces.
    # Unused entries are harmless and keep state encoding simple.
    stride = max(planes)
    visited = bytearray(6 * stride)

    # Compact BFS queue. Every state id fits in an unsigned 32-bit integer.
    queue = array('I')

    def add_state(d, idx, x, y, z):
        if x == tx and y == ty and z == tz:
            return True

        sid = d * stride + idx
        if not visited[sid]:
            visited[sid] = 1
            queue.append(sid)

        return False

    def expand(x, y, z):
        """
        Generate all one-move destinations from (x,y,z).
        Returns True if the teleporter is reached.
        """
        coords = (x, y, z)

        for d in range(6):
            axis = d >> 1
            sign = 1 if (d & 1) == 0 else -1

            q = coords[axis]

            if axis == 0:
                tidx = y * p + z
            elif axis == 1:
                tidx = x * p + z
            else:
                tidx = y * m + x

            if sign == 1:
                surface = mins[axis][tidx]
                if surface < 0 or q + 1 > surface:
                    continue
            else:
                surface = maxs[axis][tidx]
                if surface < 0 or q - 1 < surface:
                    continue

            # The robot is hanging, so the only possible move is a fall.
            if q + sign != surface:
                q2 = surface - sign

                if q2 < 0 or q2 >= dims[axis]:
                    continue

                if axis == 0:
                    nx, ny, nz = q2, y, z
                elif axis == 1:
                    nx, ny, nz = x, q2, z
                else:
                    nx, ny, nz = x, y, q2

                if add_state(d, tidx, nx, ny, nz):
                    return True

                continue

            # The robot is resting on the visible surface.
            for other in range(3):
                if other == axis:
                    continue

                for delta in (-1, 1):
                    nx, ny, nz = x, y, z

                    if other == 0:
                        nx += delta
                        if nx < 0 or nx >= m:
                            continue
                    elif other == 1:
                        ny += delta
                        if ny < 0 or ny >= n:
                            continue
                    else:
                        nz += delta
                        if nz < 0 or nz >= p:
                            continue

                    if axis == 0:
                        nidx = ny * p + nz
                    elif axis == 1:
                        nidx = nx * p + nz
                    else:
                        nidx = ny * m + nx

                    if sign == 1:
                        ns = mins[axis][nidx]
                        if ns < 0 or ns < q + 1:
                            continue
                        nq = ns - 1
                    else:
                        ns = maxs[axis][nidx]
                        if ns < 0 or ns > q - 1:
                            continue
                        nq = ns + 1

                    if nq < 0 or nq >= dims[axis]:
                        continue

                    if axis == 0:
                        fx, fy, fz = nq, ny, nz
                    elif axis == 1:
                        fx, fy, fz = nx, nq, nz
                    else:
                        fx, fy, fz = nx, ny, nq

                    if add_state(d, nidx, fx, fy, fz):
                        return True

        return False

    # The robot is an anchored starting position, but it has no fixed
    # gravity direction. Generate its first move directly.
    if expand(rx, ry, rz):
        print(1)
        return

    # All states currently in the queue are endpoints of one move.
    distance = 1
    head = 0

    while head < len(queue):
        end = len(queue)

        while head < end:
            sid = queue[head]
            head += 1

            d = sid // stride
            idx = sid - d * stride

            axis = d >> 1
            sign = 1 if (d & 1) == 0 else -1

            if axis == 0:
                y = idx // p
                z = idx - y * p
                surface = xmin[idx] if sign == 1 else xmax[idx]
                x = surface - 1 if sign == 1 else surface + 1
            elif axis == 1:
                x = idx // p
                z = idx - x * p
                surface = ymin[idx] if sign == 1 else ymax[idx]
                y = surface - 1 if sign == 1 else surface + 1
            else:
                y = idx // m
                x = idx - y * m
                surface = zmin[idx] if sign == 1 else zmax[idx]
                z = surface - 1 if sign == 1 else surface + 1

            if expand(x, y, z):
                print(distance + 1)
                return

        distance += 1

    print(-1)

if __name__ == "__main__":
    solve()
```Sáu bộ đệm độ sâu được lưu trữ trong`array('h')`thay vì danh sách Python thông thường. Mọi tọa độ đều nằm giữa`0`Và`499`, do đó, số nguyên 16 bit có dấu là đủ, trong khi giá trị`-1`đại diện cho một dòng không chứa khối rắn. Điều này giữ cho bộ nhớ tỷ lệ thuận với yêu cầu`O(mn + mp + np)`thông tin bề mặt. 

Biểu diễn đầu vào sử dụng một tập bit nhỏ gọn cho mỗi hàng. Một hàng có tối đa 500 ô và việc đưa thông tin cho mỗi ký tự vào bit thấp của byte cho phép Python thực hiện các phần lớn quá trình tiền xử lý đầu vào bên trong các phép toán số nguyên và byte được tối ưu hóa. Bitset đặc biệt hữu ích để tìm vị trí hàng liền đầu tiên và cuối cùng mà không cần quét mọi cột trong Python. 

các`ymin`Và`ymax`việc xây dựng sử dụng mặt nạ cột chưa được giải quyết. Khi một cột gặp ô rắn đầu tiên, cột đó sẽ bị xóa khỏi mặt nạ. Do đó, mặc dù mỗi hàng đầu vào đều được kiểm tra, nhưng mỗi hàng`(x,z)`dòng chỉ gây ra hai nhiệm vụ thành công, một nhiệm vụ cho mỗi hướng. 

Ý tưởng tương tự được sử dụng cho`zmin`Và`zmax`, ngoại trừ các bitset cho một hàng được chuyển sang toàn cục`(x,y)`không gian tọa độ. Mỗi vị trí ô được chỉ định tối đa một lần khi tìm`zmin`và một lần khi tìm thấy`zmax`. 

BFS sử dụng các trạng thái hướng cụ thể. Trạng thái không phải là tọa độ lưới chung. Nó là điểm cuối được neo liên kết với hướng được sử dụng cho bước di chuyển cuối cùng. Đối với một hướng và tọa độ ngang nhất định, bộ đệm độ sâu tương ứng sẽ xác định tọa độ ba chiều thực tế, đó là lý do tại sao chỉ`O(mn + mp + np)`các trạng thái là cần thiết. 

Vị trí ban đầu của robot được xử lý riêng vì robot có thể chọn hướng trọng lực mới trước lần di chuyển đầu tiên. Nếu hướng đó khiến robot bị treo thì động tác đầu tiên là ngã. Sau động thái đó, mọi trạng thái BFS đều là trạng thái bề mặt neo bình thường. 

Không có vấn đề tràn số nguyên trong Python và bộ đệm tọa độ chỉ sử dụng số nguyên có dấu 16 bit vì kích thước đầu vào tối đa là 500. Hàng đợi BFS sử dụng số nguyên 32 bit không dấu vì tồn tại tối đa khoảng 1,5 triệu trạng thái theo hướng cụ thể. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên chính thức là```
2 5 1
R-
*-
*-
*T
**
```Robot bắt đầu lúc`(0,0,0)`và máy dịch chuyển đang ở`(1,3,0)`. Xét trọng lực theo chiều tăng dần`y`. Khối đặc đầu tiên trong cột của robot nằm ở vị trí`y = 1`, vậy robot đang nghỉ ngơi tại`y = 0`. Ở cột lân cận`x = 1`, khối đặc đầu tiên ở`y = 4`, đưa ra một vị trí neo tại`y = 3`. 

| Trạng thái BFS | Trọng lực | Vị trí hiện tại | Điểm đến | Khoảng cách | 
| --- | --- | --- | --- | --- | 
| Ban đầu |`+y`|`(0,0,0)`|`(1,3,0)`| 1 | 

Đích đến chính xác là máy dịch chuyển nên câu trả lời là`1`. Điều này thể hiện hành vi nhảy vọt được mã hóa bởi điều kiện vùng đệm độ sâu. Robot không cần phải đi qua từng ô trống trung gian. 

### Mẫu 2 

Mẫu thứ hai chính thức là```
3 2 1
R-T
***
```Robot bắt đầu lúc`(0,0,0)`và máy dịch chuyển tức thời là`(2,0,0)`. Dưới tác dụng của trọng lực hướng tới tăng dần`y`, mỗi cột có khối đặc đầu tiên tại`y = 1`, do đó robot có thể di chuyển theo chiều ngang dọc theo bề mặt trên. 

| Trạng thái BFS | Vị trí hiện tại | Di chuyển | Điểm đến | Khoảng cách | 
| --- | --- | --- | --- | --- | 
| Ban đầu |`(0,0,0)`|`+x`di chuyển bề mặt |`(1,0,0)`| 1 | 
|`(1,0,0)`|`(1,0,0)`|`+x`di chuyển bề mặt |`(2,0,0)`| 2 | 

Điểm cuối thứ hai là thiết bị dịch chuyển tức thời, vì vậy câu trả lời là`2`. Dấu vết cũng cho thấy tại sao BFS phải giữ hướng liên quan đến trạng thái bề mặt. Cùng một tọa độ có thể là một điểm cuối neo hợp lệ theo một số hướng trọng lực và những khả năng đó có thể dẫn đến những bước di chuyển khác nhau trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(mnp + mn + mp + np)`| Việc đọc và xây dựng thông tin độ sâu là tuyến tính theo kích thước đầu vào; BFS chỉ truy cập các trạng thái bề mặt theo hướng cụ thể | 
| Không gian |`O(mnp)`trong quá trình tiền xử lý đầu vào,`O(mn + mp + np)`sau khi tiền xử lý | Các bit hàng nhỏ gọn được giữ lại tạm thời để xây dựng bộ đệm độ sâu; bản thân BFS chỉ sử dụng sáu bộ đệm độ sâu, trạng thái được truy cập và hàng đợi của nó | 

Thuật toán lý thuyết phù hợp với giải pháp dự định vì kích thước đầu vào không thể tránh khỏi là`O(mnp)`, trong khi không gian tìm kiếm thực tế chỉ có`O(mn + mp + np)`. Vì`m,n,p <= 500`, biểu đồ tìm kiếm có tối đa khoảng 1,5 triệu trạng thái hướng cụ thể. Việc triển khai Python cũng tránh việc lưu trữ lưới ký tự ba chiều ban đầu và sử dụng các mảng số nhỏ gọn để biểu diễn trạng thái cố định. 

## Trường hợp thử nghiệm 

Định dạng PDF của tuyên bố chính thức có thể làm cho mẫu đầu tiên có vẻ bị phẳng. Bố cục mẫu hợp lệ được sử dụng bên dưới là bố cục tương ứng với bố cục thực tế`m × n × p`kích thước.```python
import sys
import io
from array import array

# Paste the solve() implementation above here.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Official samples
assert run("""\
2 5 1
R-
*-
*-
*T
**
""") == "1", "sample 1"

assert run("""\
3 2 1
R-T
***
""") == "2", "sample 2"

assert run("""\
3 3 1
-R-
-*-
-T-
""") == "-1", "sample 3"

assert run("""\
5 4 2
-R---
-****
-****
-****
-----
-----
*T---
----*
""") == "5", "sample 4"

# Minimum possible number of cells that can contain both R and T
# while still giving R a neighboring solid block.
assert run("""\
2 1 1
RT
""") == "-1", "R and T cannot share a supporting configuration"

# Simple one-move boundary case.
assert run("""\
2 2 1
RT
**
""") == "1", "teleporter is reached by one surface move"

# A fall passes through T but does not end there.
assert run("""\
2 4 1
R*
T-
--
*-
""") == "-1", "passing through T during a fall must not count"

# Maximum individual dimension, while keeping the volume practical
# for a regression test. R is supported by the adjacent star.
row = ["-"] * 500
row[0] = "R"
row[1] = "*"
row[499] = "T"
max_dimension_case = "500 1 1\n" + "".join(row) + "\n"
assert run(max_dimension_case) == "-1", "maximum dimension and boundary handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức 1 |`1`| Một lần di chuyển trên bề mặt có thể bao gồm một cú thả dài | 
| Mẫu chính thức 2 |`2`| Chuyển động ngang thông thường và khoảng cách BFS | 
| Mẫu chính thức 3 |`-1`| Sự chiếu sáng có thể khiến mục tiêu ở gần không thể tiếp cận được | 
| Mẫu chính thức 4 |`5`| Sự thay đổi kết hợp của trọng lực và chuyển động ba chiều | 
|`2 1 1`với`RT`|`-1`| Kích thước thoái hóa và thiếu chuyển động ngang | 
|`2 2 1`với`RT / **`|`1`| Bề mặt ranh giới và khả năng dịch chuyển tức thời | 
|`2 4 1`trường hợp thất bại |`-1`| Đi qua máy dịch chuyển khi bị ngã không được tính | 
|`500 1 1`trường hợp thưa thớt |`-1`| Kích thước tọa độ tối đa và xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp rơi qua được xử lý bằng cách chỉ kiểm tra đích sau khi tính toán tọa độ nghỉ cuối cùng. TRONG```
2 4 1
R*
T-
--
*-
```sự tích cực-`y`bộ đệm độ sâu cho`x = 0`có khối rắn đầu tiên tại`y = 3`. Bắt đầu từ`(0,0,0)`, robot được chiếu sáng nhưng bị treo nên điểm cuối được tạo là`(0,2,0)`. Người dịch chuyển tức thời tại`(0,1,0)`không bao giờ được coi là một điểm đến. BFS tiếp tục từ`(0,2,0)`và cuối cùng là báo cáo`-1`. Điều này trực tiếp thực thi quy tắc rằng chỉ đi qua máy dịch chuyển khi bị ngã là không đủ. 

Đối với trường hợp suy biến một chiều```
3 1 1
R*T
```robot đang tựa vào ô đặc ở giữa. Theo hướng trọng lực hữu ích duy nhất, nó không thể di chuyển theo chiều ngang vì cả hai chiều còn lại đều có kích thước bằng một. Do đó BFS không tạo ra điểm cuối tại`T`, và câu trả lời là`-1`. Một giải pháp dựa trên chuyển động của lưới sáu lân cận thông thường sẽ bỏ qua thực tế là ô ở giữa không thể vượt qua một cách không chính xác. 

Đối với trường hợp chiếu sáng```
3 3 1
-R-
-*-
-T-
```bộ đệm độ sâu xác định chính xác ô ở giữa đặc là vật cản đầu tiên cho các đường liên quan. Một hướng bị từ chối bất cứ khi nào tọa độ của robot nằm ngoài khối hiển thị đầu tiên, do đó BFS không bao giờ tạo ra chuyển động qua bóng được chiếu sáng một cách không chính xác. Cuộc tìm kiếm cạn kiệt sáu phương và trở về`-1`. 

Đối với trường hợp ranh giới```
2 2 1
RT
**
```robot bắt đầu lúc`(0,0,0)`. Với lực hấp dẫn ngày càng tăng`y`, mức hỗ trợ có thể nhìn thấy là tại`y = 1`, vậy robot đang nghỉ ngơi tại`y = 0`. Di chuyển dọc theo`x`ĐẾN`(1,0,0)`đạt tới`T`trong một lần di chuyển. Điểm cuối của bộ đệm độ sâu vẫn nằm trong lưới nên kết quả là chính xác`1`. 

Lỗi thường gặp nhất là nhầm lẫn tọa độ khối rắn với tọa độ robot. Nếu khối đặc thứ nhất theo chiều dương ở`s`, robot nằm yên tại`s - 1`, không phải tại`s`. Đối với trọng lực âm, điểm cuối tương ứng là`s + 1`. Việc triển khai tuân theo các công thức này một cách nhất quán khi vừa tạo trạng thái BFS vừa giải mã chúng.
