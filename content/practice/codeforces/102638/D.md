---
title: "CF 102638D - Máy tính phân tán"
description: "Hệ thống này là một mạng lưới CPU ba chiều. Một CPU đang hoạt động chỉ có thể gửi thông tin theo hướng tích cực của mỗi trục, nghĩa là CPU có thể di chuyển đến hàng xóm của nó với tọa độ tăng thêm một. CPU bị hỏng không tồn tại trong biểu đồ truyền thông."
date: "2026-08-02T14:46:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102638
codeforces_index: "D"
codeforces_contest_name: "Bredor contest"
rating: 0
weight: 102638
solve_time_s: 96
verified: true
draft: false
---

[CF 102638D - Máy tính phân tán](https://codeforces.com/problemset/problem/102638/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Hệ thống này là một mạng lưới CPU ba chiều. Một CPU đang hoạt động chỉ có thể gửi thông tin theo hướng tích cực của mỗi trục, nghĩa là CPU có thể di chuyển đến hàng xóm của nó với tọa độ tăng thêm một. CPU bị hỏng không tồn tại trong biểu đồ truyền thông. 

Một CPU đang hoạt động được coi là quan trọng khi chỉ loại bỏ CPU đó sẽ phá hủy ít nhất một mối quan hệ giao tiếp hiện có giữa hai CPU khác. Nói cách khác, phải có một số cặp CPU đang hoạt động có đường dẫn tồn tại trước khi loại bỏ, nhưng mọi đường dẫn có thể đều sử dụng CPU đã bị loại bỏ. Nhiệm vụ là đếm xem có bao nhiêu CPU có thuộc tính này. 

Mỗi kích thước của lưới có thể đạt tới 100, do đó tổng số ô có thể lên tới một triệu. Điều này loại trừ việc cố gắng chạy tìm kiếm biểu đồ đầy đủ sau khi loại bỏ mọi CPU. Một lần tìm kiếm đầu tiên trên toàn bộ lưới đã có khoảng một triệu thao tác và việc lặp lại nó cho mỗi ô sẽ là khoảng một nghìn tỷ thao tác. Giải pháp chỉ phải kiểm tra từng CPU một số lần không đổi. 

Các trường hợp nguy hiểm chính xuất phát từ việc CPU có thể trông giống như có một số tuyến đường xung quanh nó, nhưng những tuyến đường đó có thể không tồn tại do hướng lưới hoặc CPU bị hỏng. Ví dụ:```
1 1 3
111
```CPU ở giữa rất quan trọng. Việc loại bỏ nó khiến CPU đầu tiên không thể điều khiển CPU thứ ba. Đầu ra đúng là:```
1
```Một giải pháp bất cẩn chỉ kiểm tra xem một CPU có hai hàng xóm hay không có thể bỏ lỡ điều này vì không có đường vòng hai chiều. 

Một trường hợp khác là:```
1 2 2
11
11
```CPU phía trên bên phải không quan trọng. Có hai cách có thể để đi từ CPU phía dưới bên trái đến CPU phía trên bên phải trước khi xem xét các hướng dẫn, nhưng với các hướng dẫn được phép, câu hỏi có ý nghĩa duy nhất là liệu có tồn tại một tuyến đường đơn điệu khác hay không. Đầu ra đúng là:```
0
```Việc triển khai phải tôn trọng tính chất có hướng của chuyển động thay vì coi lưới là đồ thị vô hướng. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ loại bỏ từng CPU đang hoạt động và chạy tìm kiếm khả năng tiếp cận để tìm xem có cặp giao tiếp nào biến mất hay không. Điều này đúng vì nó mô phỏng chính xác định nghĩa của CPU quan trọng. Tuy nhiên, trong lưới lớn nhất có một triệu CPU. Việc thực hiện tìm kiếm trên một triệu nút cho mỗi nút sẽ mang lại khoảng$10^{12}$công việc vượt xa giới hạn. 

Quan sát quan trọng là mọi đường dẫn vào CPU phải đi qua một trong ba đường dẫn trước có thể có của nó và mọi đường dẫn rời khỏi nó phải đi qua một trong ba đường dẫn có thể có của nó. Nếu CPU là quan trọng thì phải có một số cặp tiền thân và kế tiếp trong đó tất cả các đường dẫn giữa chúng đều đi qua CPU. 

Khoảng cách giữa người tiền nhiệm và người kế nhiệm như vậy là vô cùng nhỏ. Người tiền nhiệm đi sau CPU một bước và người kế nhiệm đi trước nó một bước. Nếu chúng nằm trên cùng một trục thì CPU là ô trung gian duy nhất có thể. Nếu họ sử dụng các trục khác nhau, thì có chính xác một ô ở giữa có thể thay thế, góc của hình chữ nhật nhỏ 2 x 2 x 1. Kiểm tra xem góc đó có tồn tại hay không là đủ. 

Điều này làm giảm toàn bộ vấn đề chỉ còn việc kiểm tra tối đa chín cấu hình cục bộ cho mỗi CPU. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nmk · nmk) | O(nmk) | Quá chậm | 
| Tối ưu | O(nmk) | O(nmk) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Lưu trữ lưới các CPU đang hoạt động và bị hỏng. Đối với mỗi CPU đang hoạt động, chỉ có sáu vị trí lân cận của nó là quan trọng vì giao tiếp có thể vào và ra thông qua các ô liền kề này. 
2. Đối với mỗi CPU đang hoạt động, hãy thu thập chỉ dẫn của các CPU tiền nhiệm và các CPU kế nhiệm đang hoạt động. Người tiền nhiệm có dạng một bước theo hướng âm của trục và người kế tiếp có dạng một bước theo hướng dương. 
3. Hãy thử từng cặp tiền nhiệm và cặp kế nhiệm. Nếu cả hai đều sử dụng cùng một trục thì người tiền nhiệm chỉ có thể tiếp cận người kế nhiệm thông qua CPU hiện tại, do đó CPU rất quan trọng. 
4. Nếu người tiền nhiệm và người kế nhiệm sử dụng các trục khác nhau, hãy tính ô đường vòng duy nhất có thể. Nếu ô đó bị hỏng hoặc nằm ngoài lưới thì CPU hiện tại rất quan trọng. Mặt khác, đường vòng cung cấp một đường đi khác và cặp này không chứng tỏ được mức độ quan trọng. 
5. Đếm CPU nếu ít nhất một cặp tiền thân và cặp kế tiếp chứng minh rằng mọi đường dẫn giữa chúng đều sử dụng CPU. 

Lý do điều này hoạt động là vì đường dẫn vào và ra khỏi một CPU có cấu trúc cục bộ. Bất kỳ đường dẫn truyền thông nào sử dụng CPU đều phải đi vào từ một thiết bị tiền nhiệm liền kề và rời khỏi một thiết bị kế tiếp liền kề. Đường vòng duy nhất có thể phải vừa với hình chữ nhật nhỏ nhất nối hai ô đó và hình chữ nhật đó có nhiều nhất một ô ở giữa khác. Kiểm tra ô đó bao gồm mọi đường dẫn thay thế có thể. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    grid = []
    for i in range(n):
        while True:
            line = input().strip()
            if line:
                break
        grid.append([list(line)])
        for _ in range(m - 1):
            grid[i].append(list(input().strip()))

    def inside(x, y, z):
        return 0 <= x < n and 0 <= y < m and 0 <= z < k

    ans = 0

    dirs = [(1, 0, 0), (0, 1, 0), (0, 0, 1)]

    for x in range(n):
        for y in range(m):
            for z in range(k):
                if grid[x][y][z] != '1':
                    continue

                prevs = []
                nexts = []

                for idx, (dx, dy, dz) in enumerate(dirs):
                    px, py, pz = x - dx, y - dy, z - dz
                    nx, ny, nz = x + dx, y + dy, z + dz

                    if inside(px, py, pz) and grid[px][py][pz] == '1':
                        prevs.append(idx)
                    if inside(nx, ny, nz) and grid[nx][ny][nz] == '1':
                        nexts.append(idx)

                critical = False

                for a in prevs:
                    if critical:
                        break
                    for b in nexts:
                        if a == b:
                            critical = True
                            break

                        dx1, dy1, dz1 = dirs[b]
                        dx2, dy2, dz2 = dirs[a]

                        cx = x - dx2 + dx1
                        cy = y - dy2 + dy1
                        cz = z - dz2 + dz1

                        if not inside(cx, cy, cz) or grid[cx][cy][cz] == '0':
                            critical = True
                            break

                if critical:
                    ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp đầu vào bỏ qua các dòng trống ngăn cách các lớp. Lưới được lưu trữ trực tiếp nên mỗi lần tra cứu hàng xóm đều có thời gian không đổi. 

Đối với mỗi ô,`prevs`Và`nexts`chỉ chứa những người hàng xóm đang làm việc có thể tham gia vào đường truyền thông qua ô. Không bao giờ có nhiều hơn ba lần lặp, vì vậy kiểm tra lồng nhau thực hiện tối đa chín lần lặp. 

Việc tính toán đường vòng là phần tinh tế nhất. Giả sử hướng trước là trục x và hướng kế tiếp là trục y. Con đường thay thế duy nhất phải di chuyển theo y trước rồi đến x, để nó đi qua góc đối diện. Công thức tính chính xác góc đó. Không cần phải tìm kiếm vì sự khác biệt tọa độ giữa người tiền nhiệm và người kế nhiệm chỉ là hai bước di chuyển đơn vị. 

# Ví dụ đã hoạt động 

Đối với khối ba x ba x ba đầy đủ, hãy xem xét một CPU ở giữa. 

| CPU hiện tại | Hướng tiền nhiệm | Hướng kế nhiệm | Tế bào thay thế | Kết quả | 
| --- | --- | --- | --- | --- | 
| (2,2,2) | x | x | không | Quan trọng | 
| (2,2,2) | x | y | (2,1,2) | Không đủ để chứng minh quan trọng | 

CPU có các cặp trên cùng một trục, chẳng hạn như CPU ​​trước nó và CPU sau nó dọc theo hướng x. Việc loại bỏ nó sẽ chặn đường truyền thông thẳng đó. Việc lặp lại lý do này sẽ đánh dấu mọi CPU không ở góc là quan trọng. 

Đối với dòng ba CPU:```
1 1 1
```dấu vết là: 

| CPU hiện tại | Người tiền nhiệm | Người kế vị | Quyết định | 
| --- | --- | --- | --- | 
| Đầu tiên | không | giữa | Không quan trọng | 
| Trung | đầu tiên | cuối cùng | Quan trọng | 
| Cuối cùng | giữa | không | Không quan trọng | 

Ví dụ cho thấy tại sao không thể tính được điểm cuối. Một CPU quan trọng cần cả phía nguồn và phía đích bị mất liên lạc. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nmk) | Mỗi CPU kiểm tra số lượng hàng xóm và cặp không đổi. | 
| Không gian | O(nmk) | Bản thân lưới được lưu trữ. | 

Lưới lớn nhất có thể chứa một triệu ô. Thuật toán chỉ thực hiện một lượng công việc cố định nhỏ trên mỗi ô, do đó nó phù hợp với giới hạn đã định. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().splitlines()
    sys.stdin = old

    it = iter(data)
    n, m, k = map(int, next(it).split())
    grid = []
    for _ in range(n):
        while True:
            s = next(it)
            if s:
                break
        grid.append([list(s)])
        for _ in range(m - 1):
            grid[-1].append(list(next(it)))

    def inside(x, y, z):
        return 0 <= x < n and 0 <= y < m and 0 <= z < k

    dirs = [(1,0,0),(0,1,0),(0,0,1)]
    ans = 0

    for x in range(n):
        for y in range(m):
            for z in range(k):
                if grid[x][y][z] != '1':
                    continue
                p = []
                q = []
                for i, (dx,dy,dz) in enumerate(dirs):
                    if inside(x-dx,y-dy,z-dz) and grid[x-dx][y-dy][z-dz]=='1':
                        p.append(i)
                    if inside(x+dx,y+dy,z+dz) and grid[x+dx][y+dy][z+dz]=='1':
                        q.append(i)
                ok = False
                for a in p:
                    for b in q:
                        if a == b:
                            ok = True
                        else:
                            dx1,dy1,dz1 = dirs[b]
                            dx2,dy2,dz2 = dirs[a]
                            cx,cy,cz = x-dx2+dx1, y-dy2+dy1, z-dz2+dz1
                            if not inside(cx,cy,cz) or grid[cx][cy][cz]=='0':
                                ok = True
                        if ok:
                            break
                    if ok:
                        break
                ans += ok
    return str(ans) + "\n"

assert run("""1 1 3
111
""") == "1\n", "line middle"

assert run("""3 3 3
111
111
111
111
111
111
111
111
111
""") == "19\n", "full cube"

assert run("""1 1 10
0101010101
""") == "0\n", "isolated CPUs"

assert run("""1 1 1
1
""") == "0\n", "single CPU"

assert run("""1 2 2
11
11
""") == "0\n", "small square"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba CPU trong một dòng | 1 | Phát hiện tách tế bào giữa | 
| Khối 3D đầy đủ | 19 | Hành vi lưới dày đặc | 
| Các ô làm việc luân phiên | 0 | Không có đường dẫn liên lạc | 
| Một CPU | 0 | Ranh giới kích thước tối thiểu | 
| Hai lớp hai | 0 | Xử lý đường vòng | 

# Vỏ cạnh 

Đối với đường thẳng:```
1 1 3
111
```CPU ở giữa có một CPU tiền nhiệm và một CPU kế tiếp trên cùng một trục. Thuật toán thấy rằng không còn ô nào có thể đi đường vòng và đếm nó. 

Đối với một CPU hoạt động bị cô lập:```
1 1 1
1
```không có người đi trước hay người kế vị. Vì không có mối quan hệ giao tiếp nào có thể bị phá vỡ nên thuật toán không thay đổi câu trả lời. 

Đối với các ô xen kẽ:```
1 1 10
0101010101
```mọi CPU hoạt động đều thiếu cặp lân cận cần thiết. Thuật toán không bao giờ tìm thấy sự kết hợp tiền thân và kế tiếp, vì vậy đầu ra vẫn bằng 0. 

Đối với một khối dày đặc, mọi CPU bên trong đều có ít nhất một cặp tiền thân thẳng hàng. Thuật toán không cần kiểm tra toàn bộ biểu đồ vì đường thẳng cục bộ đã đủ để chứng minh mức độ quan trọng.
