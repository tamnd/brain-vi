---
title: "CF 104020K - Xây dựng ki-ốt"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô chứa một nhãn duy nhất từ ​​1 đến $h cdot w$. Hãy coi lưới như một hệ thống điều hướng được chỉ dẫn: từ bất kỳ ô hiện tại nào, khách truy cập không di chuyển ngẫu nhiên hoặc dọc theo những con đường ngắn nhất mà thay vào đó tuân theo một quy tắc xác định rằng…"
date: "2026-07-02T04:42:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104020
codeforces_index: "K"
codeforces_contest_name: "2022 Benelux Algorithm Programming Contest (BAPC 22)"
rating: 0
weight: 104020
solve_time_s: 56
verified: true
draft: false
---

[CF 104020K - Xây dựng ki-ốt](https://codeforces.com/problemset/problem/104020/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô chứa một nhãn duy nhất từ 1 đến$h \cdot w$. Hãy coi lưới như một hệ thống điều hướng được chỉ dẫn: từ bất kỳ ô hiện tại nào, khách truy cập không di chuyển ngẫu nhiên hoặc dọc theo các đường dẫn ngắn nhất mà thay vào đó tuân theo quy tắc xác định phụ thuộc vào ô đích cố định. 

Một khách truy cập bắt đầu tại một ô kiosk đã chọn. Đối với mỗi ô khác được coi là đích đến, khách truy cập liên tục di chuyển từng bước một. Ở mỗi bước, họ xem xét bốn ô liền kề và chọn ô có nhãn gần nhất với nhãn của ô đích. Nếu nhiều hàng xóm gần nhau như nhau, mối ràng buộc sẽ bị phá vỡ bằng cách chọn hàng xóm có nhãn gần với nhãn của ô hiện tại hơn. Quá trình chỉ dừng khi đến ô đích, nếu không nó có thể tiếp tục mãi mãi hoặc bị kẹt trong một vòng lặp. 

Nhiệm vụ là chọn vị trí ki-ốt sao cho mọi ô đích đều có thể truy cập được theo quy tắc này và trong số tất cả các vị trí ki-ốt hợp lệ, hãy giảm thiểu số bước tồi tệ nhất cần thiết để đến bất kỳ đích nào. Nếu không có ki-ốt nào cho phép tiếp cận tất cả các điểm đến thì câu trả lời là không thể. 

Kích thước lưới tối đa là 40 x 40, vì vậy có tối đa 1600 ô. Điều này đủ nhỏ để chúng ta có thể cung cấp các thuật toán bậc hai hoặc thậm chí bậc ba về số lượng ô, nhưng bất kỳ điều gì cố gắng mô phỏng các đường dẫn tùy ý cho tất cả các nguồn và tất cả các đích một cách độc lập theo cách ngây thơ đều có nguy cơ gây ra hàng tỷ chuyển đổi. 

Một trường hợp thất bại tinh vi xuất hiện khi chuyển động mang tính quyết định tạo ra một chu kỳ không bao gồm đích đến. Ví dụ: nếu từ mọi ô trong một vùng, quy tắc “nhãn gần nhất với mục tiêu” tiếp tục nảy giữa một vài ô thì khách truy cập sẽ không bao giờ đến đích ngay cả khi lưới được kết nối theo nghĩa thông thường. Trong trường hợp như vậy, bất kỳ ki-ốt nào có thể tiếp cận khu vực đó cho điểm đến đó đều không hợp lệ. 

Một tình huống khó khăn khác là khả năng tiếp cận phụ thuộc vào điểm đến. Một ô có thể đến ô 10 dưới đích 10, nhưng không đến được ô 20 dưới đích 20. Điều này phá hủy mọi ý tưởng về một biểu đồ tĩnh duy nhất; thay vào đó, mỗi đích tạo ra một biểu đồ có hướng khác nhau trên cùng một lưới. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là sửa một ki-ốt và sau đó, đối với mỗi ô đích, mô phỏng quy trình từng bước. Mỗi bước di chuyển là$O(1)$, nhưng một mô phỏng đơn lẻ có thể lặp vô thời hạn hoặc truy cập nhiều trạng thái trước khi đến đích. Làm điều này cho tất cả các cặp ki-ốt và điểm đến sẽ dẫn đến khoảng$O(n^2 \cdot n)$hành vi trong trường hợp xấu nhất, quá chậm khi$n = 1600$. 

Quan sát quan trọng là đối với một đích đến cố định, quy tắc di chuyển trở nên hoàn toàn mang tính xác định: mỗi ô có chính xác một bước đi tiếp theo. Điều này biến lưới thành một biểu đồ có hướng chức năng cho từng đích riêng biệt. Trong biểu đồ như vậy, khả năng tiếp cận tới đích tương đương với việc liệu đích đến có nằm trong cùng một tập hợp có thể tiếp cận của thành phần chức năng hay không. Chúng ta có thể tính toán tất cả các nút có thể đến đích bằng cách đảo ngược các cạnh và chạy BFS từ đích. 

Khi chúng tôi biết, đối với một đích đến cố định, vị trí xuất phát nào có thể đến đích đó và bao nhiêu bước, chúng tôi có thể lặp lại điều này cho từng đích đến một cách độc lập. Sau đó, việc lựa chọn kiosk trở thành một vấn đề tổng hợp đơn giản: chúng tôi cần một nút có thể tiếp cận tất cả các điểm đến và trong số các nút đó, chúng tôi giảm thiểu khoảng cách tối đa trên tất cả các kết quả BFS dành riêng cho điểm đến. 

Điều này làm giảm vấn đề từ mô phỏng động đến xây dựng$n$đồ thị chức năng và chạy$n$Truyền tải BFS, điều này là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu trên mỗi cặp |$O(n^3)$trường hợp xấu nhất |$O(1)$thêm | Quá chậm | 
| Biểu đồ chức năng BFS cho mỗi điểm đến |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi ô, trước tiên chúng ta chuyển đổi vị trí của nó thành chỉ mục và lưu trữ nhãn của nó. Điều này cho phép truy cập liên tục vào tọa độ và giá trị. 

Sau đó, chúng tôi xử lý từng ô như một đích đến tiềm năng. Đối với một ô đích cố định$t$, chúng ta xây dựng một đồ thị có hướng trong đó mỗi ô$u$có chính xác một cạnh đi được xác định bởi quy tắc chuyển động. 

Sau đó, chúng tôi tính toán danh sách kề ngược của biểu đồ này vì chúng tôi muốn truyền bá khả năng tiếp cận ngược từ đích. 

### Các bước 

1. Đối với mỗi ô đích$t$, tính toán cho từng ô$u$ô tiếp theo của nó$next(u, t)$bằng cách kiểm tra tối đa bốn người hàng xóm và áp dụng quy tắc khoảng cách với tie-break. 
2. Xây dựng biểu đồ ngược nơi chúng tôi lưu trữ tất cả$u$như vậy$next(u, t) = v$. 
3. Chạy BFS bắt đầu từ$t$trong biểu đồ ngược này để tính toán tất cả các nút có thể tiếp cận$t$, cùng với khoảng cách ngắn nhất tới$t$. 
4. Lưu trữ các khoảng cách này vào bảng$dist[t][u]$, trong đó nó thể hiện số bước cần thiết cho$u$để đạt được$t$, hoặc -1 nếu không thể. 
5. Sau khi xử lý tất cả các điểm đến, hãy lặp lại mọi ki-ốt có thể$s$. 
6. Kiểm tra xem$dist[t][s]$được xác định cho tất cả các điểm đến$t$. Nếu không thì bỏ$s$. 
7. Đối với các ki-ốt hợp lệ, hãy tính giá trị lớn nhất của$dist[t][s]$tổng thể$t$. 
8. Chọn ki-ốt giảm thiểu mức tối đa này. 

### Tại sao nó hoạt động 

Đối với mỗi đích đến, quy tắc di chuyển xác định một đồ thị hàm số xác định. Các cạnh đảo ngược chuyển đổi khả năng tiếp cận thành bài toán đường đi ngắn nhất một nguồn trong biểu đồ không có trọng số. BFS tính toán chính xác các bước tối thiểu vì mỗi cạnh tương ứng với chính xác một bước di chuyển. 

Điều kiện hợp lệ toàn cầu cho một ki-ốt chỉ đơn giản là sự giao thoa của tất cả các nhóm có thể truy cập trên mỗi điểm đến. Nếu một ki-ốt không đạt được dù chỉ một điểm đến, điều đó có nghĩa là trong biểu đồ chức năng của điểm đến đó, nó nằm ngoài vùng thu hút của nút mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

h, w = map(int, input().split())
n = h * w

grid = []
pos = [None] * (n + 1)

for i in range(h):
    row = list(map(int, input().split()))
    grid.append(row)
    for j, v in enumerate(row):
        pos[v] = (i, j)

dirs = [(1,0), (-1,0), (0,1), (0,-1)]

def inside(x, y):
    return 0 <= x < h and 0 <= y < w

dist = [[-1] * (n + 1) for _ in range(n + 1)]

for t in range(1, n + 1):
    tx, ty = pos[t]

    nxt = [0] * (n + 1)
    rev = [[] for _ in range(n + 1)]

    for u in range(1, n + 1):
        ux, uy = pos[u]

        best_v = -1
        best_d1 = 10**18
        best_d2 = 10**18

        for dx, dy in dirs:
            nx, ny = ux + dx, uy + dy
            if not inside(nx, ny):
                continue
            v = grid[nx][ny]

            d1 = abs(v - t)
            d2 = abs(v - u)

            if d1 < best_d1 or (d1 == best_d1 and d2 < best_d2):
                best_d1 = d1
                best_d2 = d2
                best_v = v

        nxt[u] = best_v
        rev[best_v].append(u)

    q = deque([t])
    dist[t][t] = 0

    while q:
        v = q.popleft()
        for u in rev[v]:
            if dist[t][u] == -1:
                dist[t][u] = dist[t][v] + 1
                q.append(u)

ans_s = -1
ans_d = 10**18

for s in range(1, n + 1):
    ok = True
    worst = 0
    for t in range(1, n + 1):
        if dist[t][s] == -1:
            ok = False
            break
        worst = max(worst, dist[t][s])

    if ok and worst < ans_d:
        ans_d = worst
        ans_s = s

if ans_s == -1:
    print("impossible")
else:
    print(ans_s, ans_d)
```Việc thực hiện phản ánh trực tiếp thuật toán. Phần quan trọng là việc xây dựng hàm bước tiếp theo cho mỗi điểm đến, hàm này phải áp dụng cả quy tắc khoảng cách đến đích và điểm nối dựa trên khoảng cách với nhãn ô hiện tại. Sau đó, mọi thứ giảm xuống thành một BFS ngược xử lý từng đích một cách độc lập. 

Một cạm bẫy triển khai phổ biến là quên rằng biểu đồ thay đổi đối với mọi đích đến. Mảng con trỏ tiếp theo và vùng kề ngược phải được xây dựng lại bên trong vòng lặp đích. Một vấn đề tế nhị khác là khởi tạo chính xác các khoảng cách riêng biệt cho từng điểm đến; chia sẻ mảng qua các lần lặp sẽ trộn lẫn các trạng thái và làm mất hiệu lực kết quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ trong đó các nhãn đã được sắp xếp theo thứ tự tăng dần từ trái sang phải, từ trên xuống dưới. Đối với mỗi điểm đến, chuyển động luôn hướng tới các nhãn gần hơn về mặt số lượng, tạo ra chuyển động về phía trước hầu như nhất quán. BFS từ mỗi điểm đến sẽ cho thấy nhiều điểm xuất phát có thể tiếp cận tất cả các mục tiêu và ki-ốt có xu hướng nằm gần trung tâm nơi khoảng cách di chuyển tối đa được giảm thiểu. 

Dấu vết cho một điểm đến duy nhất$t$sẽ trông như thế này: 

| Nút | Tiếp cận$t$| Khoảng cách | 
| --- | --- | --- | 
| t | vâng | 0 | 
| hàng xóm của t | vâng | 1 | 
| lớp tiếp theo | vâng | 2 | 

Điều này xác nhận rằng BFS ngược được truyền chính xác ra bên ngoài từ đích. 

### Ví dụ 2 

Trong một lưới không đều hơn, giả sử một chu trình nhỏ hình thành theo quy tắc chuyển động cho một đích cụ thể. Sau đó, chỉ các nút bên trong lưu vực dẫn vào chu kỳ đó mới có thể đến đích. Một ki-ốt được đặt bên ngoài lưu vực đó sẽ không vượt qua được quá trình kiểm tra tính hợp lệ vì BFS từ đích sẽ không bao giờ đến được. 

| Nút | Tiếp cận$t$| Khoảng cách | 
| --- | --- | --- | 
| t | vâng | 0 | 
| nút lưu vực | vâng | hữu hạn | 
| chu kỳ bên ngoài lưu vực | không | - | 

Điều này chứng tỏ tại sao chúng ta phải kiểm tra khả năng tiếp cận riêng biệt cho từng điểm đến thay vì dựa vào kết nối hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Đối với mỗi$n$điểm đến, chúng tôi tính toán một đồ thị chức năng trong$O(n)$và chạy BFS trong$O(n)$| 
| Không gian |$O(n^2)$| Chúng tôi lưu trữ một bảng khoảng cách cho mỗi cặp nguồn đích | 

Kích thước lưới tối đa là 1600 ô, nên đại khái là$2.5 \times 10^6$các mục nhập khoảng cách và có cùng số lần chuyển tiếp, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline()  # placeholder, replace with full solution call

# Since full harness is omitted, these are structural placeholders
# In real use, run() should execute the full solution above

# minimal case (2x2)
# assert run("2 2\n1 2\n3 4\n") == "..."

# single cycle-like arrangement (conceptual)
# assert run("...") == "impossible"

# custom irregular
# assert run("...") == "..."

# boundary test h=w=40 would be constructed similarly
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới có thứ tự 2x2 | ki-ốt hợp lệ | tính đúng đắn cơ bản | 
| bố trí tạo ra chu kỳ | không thể | điểm đến không thể tiếp cận | 
| nhãn tranh giành | kết quả hợp lệ | xử lý các chuyển tiếp không đều | 
| bố cục đơn điệu thống nhất | ki-ốt trung tâm | đối xứng và giảm thiểu khoảng cách | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đích tạo ra một chu trình không bao gồm chính đích đó. Trong tình huống đó, BFS từ đích chỉ đến được các nút trong lưu vực ngược của nó. Bất kỳ ki-ốt nào bên ngoài lưu vực đó sẽ có vẻ hợp lệ không chính xác nếu khả năng tiếp cận không được tính toán lại cho mỗi điểm đến. Thuật toán tránh điều này vì mỗi đích có BFS ngược riêng. 

Một trường hợp khó khăn khác là sự mơ hồ mang tính ràng buộc. Khi hai hàng xóm ở gần đích như nhau, quy tắc phụ sẽ phụ thuộc vào nhãn ô hiện tại chứ không phải đích. Việc trộn lẫn hai sự so sánh này thường là nguyên nhân dẫn đến sự chuyển tiếp không chính xác. Việc triển khai tính toán rõ ràng cả hai khoảng cách ở mỗi bước trước khi chọn ô tiếp theo, đảm bảo hành vi xác định nhất quán với định nghĩa vấn đề.
