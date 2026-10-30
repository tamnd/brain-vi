---
title: "CF 106270C - Bình Chứa Gas"
description: "Chúng ta được cung cấp một lưới 3D thể hiện không gian dưới lòng đất. Mỗi ô là đá hoặc khí và mỗi ô khí chứa chính xác một đơn vị khí."
date: "2026-06-18T23:03:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106270
codeforces_index: "C"
codeforces_contest_name: "ICPC Asia Dhaka Regional Onsite 2025 \u2014 Replay Contest"
rating: 0
weight: 106270
solve_time_s: 59
verified: true
draft: false
---

[CF 106270C - Bình chứa khí](https://codeforces.com/problemset/problem/106270/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới 3D thể hiện không gian dưới lòng đất. Mỗi ô là đá hoặc khí và mỗi ô khí chứa chính xác một đơn vị khí. Các tế bào khí kết nối ở dạng 3D bằng cách sử dụng tính liền kề của khuôn mặt, do đó, bất kỳ thành phần khí nào được kết nối sẽ tạo thành một “hồ chứa” không thể phân chia: nếu bạn chạm vào một ô của nó, toàn bộ thành phần đó được coi là đã thu thập. 

Chúng ta được phép chọn một cột khoan thẳng đứng duy nhất ở một vị trí bề mặt nào đó$(x, y)$. Mũi khoan đi thẳng xuống mọi độ sâu và bất cứ khi nào nó đi qua bất kỳ ô khí nào, nó sẽ kích hoạt việc thu thập toàn bộ bể chứa chứa ô đó. Nếu gặp cùng một hồ chứa nhiều lần trên cùng một cột thì chỉ được tính một lần. Mục tiêu là chọn một cột sao cho tổng kích thước của tất cả các hồ chứa riêng biệt mà cột đó chạm vào là tối đa. 

Vì vậy, vấn đề không phải là đếm các tế bào khí cục bộ trong một cột. Thay vào đó, một cột hoạt động như một “bộ kích hoạt” cho toàn bộ các thành phần được kết nối ở dạng 3D. 

Kích thước lưới lên đến$50 \times 50 \times 50$, do đó tổng số ô tối đa là 125.000 cho mỗi trường hợp thử nghiệm. Kích thước này cho phép truyền tải tuyến tính hoặc gần tuyến tính trên lưới, nhưng loại trừ mọi thứ bậc hai trên tất cả các ô hoặc khối trên mỗi cột. 

Một sai lầm ngây thơ sẽ là xử lý từng lát cắt độ sâu một cách độc lập và tính tổng các ô khí trên mỗi cột hoặc đếm khí trên mỗi lớp. Điều đó không thành công vì các hồ chứa trải dài trên nhiều cột và độ sâu. 

Ví dụ: hãy xem xét một thành phần lớn được kết nối có hình dạng giống như một đốm màu 3D rỗng chạm vào nhiều$(x,y)$cột. Nếu bạn chọn một cột giao với cột đó, bạn sẽ nhận được toàn bộ thành phần chứ không chỉ các ô trong cột đó. Một chiến lược đếm cục bộ sẽ đánh giá thấp điều này. 

Một vấn đề tế nhị khác là việc đếm trùng lặp theo chiều sâu. Nếu một hồ chứa xuất hiện ở nhiều mức z trong cùng một$(x,y)$, nó vẫn chỉ được tính một lần. Điều này khiến cần phải làm việc với các thành phần được kết nối một cách rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp rất dễ mô tả. Đối với mọi tọa độ bề mặt$(x,y)$, chúng tôi mô phỏng việc khoan xuống cột đó và thu thập tất cả các tế bào khí gặp phải. Mỗi khi chúng tôi chạm vào một ô khí thuộc một bể chứa chưa được tính cho cột đó, chúng tôi sẽ đổ đầy toàn bộ thành phần và cộng thêm kích thước của nó. 

Điều này đúng nhưng không hiệu quả nếu thực hiện một cách ngây thơ. Phần tốn kém là liên tục khám phá các thành phần được kết nối. Trong trường hợp xấu nhất, mỗi ô khí có thể là thành phần của chính nó hoặc thuộc về một thành phần lớn được phát hiện lại nhiều lần trên các cột. Nếu chúng ta thực hiện việc lấp đầy cho mỗi cột một cách độc lập, chúng ta có thể khám phá tới$O(XYZ)$mỗi cột, đưa ra$O((XY)(XYZ))$, nó quá lớn. 

Quan sát quan trọng là các hồ chứa là đối tượng toàn cầu. Khi chúng tôi xác định từng thành phần khí được kết nối và tính toán kích thước của nó, mọi ô sẽ chỉ trở thành một con trỏ tới thành phần đó. Sau đó, vấn đề giảm xuống việc chọn một tập hợp các thành phần giao nhau với một cột nhất định. Thay vì tính toán lại khả năng kết nối trên mỗi cột, chúng tôi tính toán trước tất cả các thành phần một lần, gán cho mỗi ô một id thành phần và lưu trữ kích thước của từng thành phần. 

Sau đó, với mỗi cột$(x,y)$, chúng ta chỉ cần kiểm tra các id thành phần riêng biệt nào xuất hiện dọc theo chiều sâu$z$và tính tổng kích thước của chúng một lần. Điều này biến vấn đề thành một quy trình gồm hai giai đoạn: tiền xử lý toàn cục các thành phần được kết nối, sau đó tổng hợp cục bộ trên mỗi cột. 

Vì mỗi ô thuộc về chính xác một thành phần và chúng tôi chỉ quét từng cột một lần nên tổng công việc sẽ trở thành tuyến tính theo kích thước lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (DFS mỗi cột) |$O((XY)(XYZ))$|$O(XYZ)$| Quá chậm | 
| Ghi nhãn thành phần + tổng hợp cột |$O(XYZ)$|$O(XYZ)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lưới dưới dạng biểu đồ 3D trong đó mỗi ô khí là một nút và các cạnh tồn tại giữa các ô khí liền kề với mặt. 

1. Duyệt từng ô trong lưới. Bất cứ khi nào chúng tôi tìm thấy một tế bào khí chưa được gán cho bất kỳ thành phần nào, chúng tôi sẽ bắt đầu lấp đầy (BFS hoặc DFS) từ nó. Việc lấp đầy này đánh dấu tất cả các tế bào khí có thể tiếp cận là một phần của cùng một thành phần và gán cho chúng một id thành phần. Trong quá trình truyền tải này, chúng tôi cũng đếm xem có bao nhiêu ô thuộc về thành phần này. Số lượng này được lưu trữ dưới dạng kích thước của thành phần. 
2. Sau bước tiền xử lý này, mọi tế bào khí đều biết nó thuộc thành phần nào và mọi thành phần đều biết tổng thể tích khí của nó. Bước này đảm bảo chúng ta đã nén hoàn toàn cấu trúc 3D thành các tập hợp rời rạc. 
3. Đối với mỗi tọa độ bề mặt$(x, y)$, chúng tôi quét theo chiều dọc từ$z = 0$ĐẾN$z = Z-1$. Chúng tôi duy trì một tập hợp boolean hoặc mảng dấu thời gian để đảm bảo rằng chúng tôi không tính cùng một thành phần hai lần cho một cột. 
4. Bất cứ khi nào chúng ta gặp một tế bào khí ở$(x,y,z)$, chúng ta tra cứu id thành phần của nó. Nếu thành phần này chưa được tính cho cột này, chúng tôi sẽ thêm kích thước được tính toán trước của nó vào tổng cột và đánh dấu thành phần đó là đã truy cập cho cột này. 
5. Sau khi xử lý tất cả độ sâu cho một cột, chúng tôi so sánh tổng của nó với mức tối đa toàn cầu và giữ kết quả tốt nhất. 
6. Cuối cùng, xuất giá trị tối đa trên tất cả các cột. 

Lựa chọn thiết kế quan trọng là việc theo dõi “đã truy cập” được đặt lại trên mỗi cột chứ không phải trên toàn cầu vì mỗi cột chọn các thành phần một cách độc lập. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai thuộc tính cấu trúc. Đầu tiên, mỗi tế bào khí thuộc về chính xác một thành phần được kết nối, do đó việc phân nhóm được xác định rõ ràng và rời rạc. Thứ hai, một cột thu thập một thành phần khi và chỉ khi nó chứa ít nhất một ô từ thành phần đó. Do đó, kích thước tổng của các thành phần khớp chính xác với tổng lượng khí thu được, miễn là mỗi thành phần được tính một lần trên mỗi cột. Vì thuật toán thực thi ràng buộc đó bằng cách sử dụng tập hợp được truy cập trên mỗi cột nên không có thành phần nào được tính hai lần và không có thành phần có thể truy cập nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    T = int(input())
    for _ in range(T):
        line = input().strip()
        while line == "":
            line = input().strip()
        X, Y, Z = map(int, line.split())

        grid = [[[None]*Y for _ in range(X)] for _ in range(Z)]

        for z in range(Z):
            blank = input().strip()
            while blank == "":
                blank = input().strip()
            for x in range(X):
                row = input().strip()
                grid[z][x] = list(row)

        comp = [[[-1]*Y for _ in range(X)] for _ in range(Z)]
        comp_size = []

        dirs = [(1,0,0), (-1,0,0), (0,1,0), (0,-1,0), (0,0,1), (0,0,-1)]

        cid = 0

        for z in range(Z):
            for x in range(X):
                for y in range(Y):
                    if grid[z][x][y] == '.' and comp[z][x][y] == -1:
                        q = deque()
                        q.append((z,x,y))
                        comp[z][x][y] = cid
                        cnt = 0

                        while q:
                            cz, cx, cy = q.popleft()
                            cnt += 1
                            for dz, dx, dy in dirs:
                                nz, nx, ny = cz+dz, cx+dx, cy+dy
                                if 0 <= nz < Z and 0 <= nx < X and 0 <= ny < Y:
                                    if grid[nz][nx][ny] == '.' and comp[nz][nx][ny] == -1:
                                        comp[nz][nx][ny] = cid
                                        q.append((nz,nx,ny))

                        comp_size.append(cnt)
                        cid += 1

        best = 0
        seen = [0]*len(comp_size)
        stamp = 1

        for x in range(X):
            for y in range(Y):
                total = 0
                stamp += 1
                local_seen = seen
                for z in range(Z):
                    if grid[z][x][y] == '.':
                        c = comp[z][x][y]
                        if local_seen[c] != stamp:
                            local_seen[c] = stamp
                            total += comp_size[c]
                best = max(best, total)

        print(best)

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên xây dựng các thành phần được kết nối bằng BFS trong lưới 3D đầy đủ. Mỗi tế bào khí được gán một id thành phần chính xác một lần và BFS đảm bảo tất cả khí được kết nối mặt được nhóm lại với nhau. Mảng kích thước lưu trữ mỗi hồ chứa có bao nhiêu ô. 

Giai đoạn thứ hai lặp lại trên tất cả$(x,y)$các vị trí bề mặt. Đối với mỗi cột, chúng tôi quét xuống dưới và kiểm tra id thành phần nào xuất hiện. Thủ thuật dấu thời gian tránh phân bổ một tập hợp mới cho mỗi cột, giúp duy trì hiệu quả về bộ nhớ và thời gian. Mỗi thành phần được thêm tối đa một lần vào mỗi cột. 

Một điểm tinh tế là chúng tôi chỉ kích hoạt việc thu thập thành phần khi nhìn thấy một ô khí, nhưng vì mỗi ô khí đều có id thành phần hợp lệ nên điều này sẽ thu thập đầy đủ tất cả các bể chứa giao nhau với cột. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ nơi tồn tại hai thành phần khí riêng biệt và chỉ có một cột giao nhau với cả hai. 

| Bước | cột (x,y) | quét z | thành phần gặp phải | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | z=0..Z | {A} | 3 | 
| 2 | (1,0) | z=0..Z | {B} | 5 | 
| 3 | (1,1) | z=0..Z | {A,B} | 8 | 

Điều này chứng tỏ rằng thuật toán chỉ hợp nhất các khoản đóng góp một cách chính xác khi một cột giao nhau với nhiều vùng chứa. 

### Ví dụ 2 

Một hồ chứa lớn được kết nối duy nhất trải rộng trên toàn bộ lưới điện. 

| Bước | cột (x,y) | quét z | thành phần gặp phải | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | cột bất kỳ | z=0..Z | {A} | 125 | 

Mỗi cột chạm vào hồ chứa đều có kích thước đầy đủ vì BFS đã hợp nhất tất cả các ô thành một thành phần. Việc lựa chọn cột không còn quan trọng về mặt cấu trúc nữa. 

Điều này xác nhận rằng khả năng kết nối được ghi lại hoàn toàn trong các tập hợp tiền xử lý và logic cột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(XYZ)$| Mỗi ô được truy cập một lần trong BFS và một lần trong quá trình quét cột | 
| Không gian |$O(XYZ)$| Lưu trữ cho lưới, id thành phần và kích thước | 

Các ràng buộc cho phép tối đa 125.000 ô cho mỗi trường hợp thử nghiệm và tối đa 10 trường hợp thử nghiệm, do đó, giải pháp vừa vặn thoải mái trong giới hạn với khả năng truyền tải tuyến tính và kiểm tra thời gian không đổi trên mỗi ô. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        T = int(input())
        for _ in range(T):
            line = input().strip()
            while line == "":
                line = input().strip()
            X, Y, Z = map(int, line.split())

            grid = [[[None]*Y for _ in range(X)] for _ in range(Z)]

            for z in range(Z):
                blank = input().strip()
                while blank == "":
                    blank = input().strip()
                for x in range(X):
                    row = input().strip()
                    grid[z][x] = list(row)

            comp = [[[-1]*Y for _ in range(X)] for _ in range(Z)]
            comp_size = []

            dirs = [(1,0,0), (-1,0,0), (0,1,0), (0,-1,0), (0,0,1), (0,0,-1)]

            cid = 0
            for z in range(Z):
                for x in range(X):
                    for y in range(Y):
                        if grid[z][x][y] == '.' and comp[z][x][y] == -1:
                            q = deque()
                            q.append((z,x,y))
                            comp[z][x][y] = cid
                            cnt = 0
                            while q:
                                cz, cx, cy = q.popleft()
                                cnt += 1
                                for dz, dx, dy in dirs:
                                    nz, nx, ny = cz+dz, cx+dx, cy+dy
                                    if 0 <= nz < Z and 0 <= nx < X and 0 <= ny < Y:
                                        if grid[nz][nx][ny] == '.' and comp[nz][nx][ny] == -1:
                                            comp[nz][nx][ny] = cid
                                            q.append((nz,nx,ny))
                            comp_size.append(cnt)
                            cid += 1

            best = 0
            seen = [0]*len(comp_size)
            stamp = 1

            for x in range(X):
                for y in range(Y):
                    total = 0
                    stamp += 1
                    for z in range(Z):
                        if grid[z][x][y] == '.':
                            c = comp[z][x][y]
                            if seen[c] != stamp:
                                seen[c] = stamp
                                total += comp_size[c]
                    best = max(best, total)

            print(best)

    return solve()

# custom tests

assert run("""1

1 1 1

.
""") == "1", "minimum case"

assert run("""1

2 1 2

.
#
.
#
""") == "2", "vertical split same column"

assert run("""1

2 2 1

..
..
""") == "4", "single layer full gas"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khí 1x1x1 | 1 | thành phần đơn tối thiểu | 
| xen kẽ dọc | 2 | tập hợp cột theo nhiều độ sâu | 
| đầy đủ khí máy bay | 4 | một thành phần khổng lồ xuyên lớp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi các tế bào khí bị cô lập. Trong trường hợp này, mỗi tế bào khí tạo thành thành phần riêng của nó và câu trả lời cho một cột chỉ đơn giản là số lượng tế bào khí trong cột đó. Giai đoạn BFS chỉ định một id thành phần duy nhất cho mỗi ô, do đó việc hợp nhất không xảy ra không chính xác và quá trình quét cột sẽ thêm từng đơn vị chính xác một lần. 

Một trường hợp khác là một thành phần lớn duy nhất bao trùm toàn bộ lưới. Ở đây, mọi tế bào khí đều nhận được cùng một id thành phần. Trong quá trình quét cột, ô khí đầu tiên gặp phải sẽ kích hoạt bao gồm toàn bộ thành phần và tất cả các ô khác sẽ bị bỏ qua do kiểm tra dấu thời gian. Điều này ngăn chặn việc đếm quá mức mặc dù nhiều ô trong cùng một cột thuộc cùng một vùng chứa. 

Trường hợp tinh tế cuối cùng là nhiều thành phần bị ngắt kết nối được xếp chồng lên nhau theo chiều dọc trong cùng một cột. Quá trình quét sẽ tích lũy chúng một cách chính xác vì mỗi id thành phần là riêng biệt và cơ chế truy cập trên mỗi cột đảm bảo mỗi id đóng góp chính xác một lần ngay cả khi nó xuất hiện ở nhiều cấp độ z.
