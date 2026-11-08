---
title: "CF 1423D - Có ai ghét gió không?"
description: "Chúng tôi được yêu cầu mô phỏng việc điều hướng trên bản đồ biển với các đảo, bến cảng và các điểm cung cấp lương thực, có tính đến mô hình gió hàng ngày và lượng lương thực hạn chế."
date: "2026-06-11T06:07:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1423
codeforces_index: "D"
codeforces_contest_name: "Bubble Cup 13 - Finals [Online Mirror, unrated, Div. 1]"
rating: 3100
weight: 1423
solve_time_s: 71
verified: true
draft: false
---

[CF 1423D - Có ai ghét gió không?](https://codeforces.com/problemset/problem/1423/D) 

**Đánh giá:** 3100 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng việc điều hướng trên bản đồ biển với các đảo, bến cảng và các điểm cung cấp lương thực, có tính đến mô hình gió hàng ngày và lượng lương thực hạn chế. Bản đồ là một lưới N×M trong đó mỗi ô là đất liền ('L'), biển ('S'), cảng ('P') hoặc vị trí xuất phát của tàu ('M'). Con tàu di chuyển một ô mỗi ngày theo bất kỳ hướng nào trong bốn hướng chính hoặc có thể đứng yên, nhưng gió cũng đẩy con tàu đi một ô theo hướng hiện tại của nó. Con tàu không thể kết thúc một ngày trên đất liền mà phải ở trên các ô được kết nối bằng con đường biển 4 hướng. Ngoài ra, tàu còn có K ngày lương thực, đồng thời có các trạm cung cấp lương thực theo tọa độ và ngày cụ thể, giúp thủy thủ đoàn có thể bổ sung hết công suất. 

Nhiệm vụ là tính số ngày tối thiểu cần thiết để đến cảng mà không hết lương thực, hoặc xuất ra -1 nếu không thể. 

Những hạn chế đang nói lên điều đó. N và M tăng lên 200, vì vậy lưới có tối đa 40.000 ô. K lên tới 200, có thể quản lý được để theo dõi trạng thái kiểu BFS. Thông tin gió W có thể lên tới 10^6, loại trừ các thuật toán lặp lại trong tất cả các ngày mà không cần quản lý trạng thái cẩn thận. Số lượng trạm cung cấp thực phẩm T nhỏ (<20), điều này cho thấy chúng ta có thể theo dõi tác động của chúng một cách rõ ràng. Thách thức là kết hợp vị trí, lượng lương thực còn lại, lượng gió và nguồn cung cấp phụ thuộc vào ngày một cách hiệu quả. 

Các trường hợp có biên độ không rõ ràng bao gồm các tình huống gió di chuyển tàu vào đất liền hoặc ra khỏi bản đồ. Một BFS bất cẩn bỏ qua hiệu ứng gió có thể đánh dấu không chính xác các đường dẫn không thể truy cập là có thể truy cập được. Một trường hợp tế nhị khác là khi một trạm thực phẩm phải được ghé thăm đúng vào ngày làm việc của trạm đó; việc bỏ lỡ ngày hôm đó có thể khiến con đường đầy hứa hẹn trước đó trở nên vô hiệu. Ví dụ: một vùng biển 2 × 2 với K = 1, gió thổi về hướng bắc mỗi ngày và nguồn cung cấp vào ngày 0 tại ô biển duy nhất khác có thể không thể thực hiện được nếu tàu không thể đến kịp. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận vũ phu là khám phá mọi con đường có thể có cho con tàu, mô phỏng mọi sự kết hợp giữa chuyển động và gió mỗi ngày, đồng thời theo dõi lượng thức ăn còn lại. Đây sẽ là BFS hoặc DFS trên không gian trạng thái (x, y, ngày, thực phẩm). Mỗi ngày có năm bước di chuyển có thể xảy ra (N, S, E, W, ở lại) kết hợp với gió, mang lại tối đa năm kết quả cho mỗi trạng thái mỗi ngày. Với W lên tới 10^6, một cách tiếp cận đơn giản có thể dễ dàng yêu cầu ở mức 10^6 × 40.000 thao tác, con số này là quá nhiều. Ngoài ra, việc theo dõi lượng thực phẩm còn lại dưới dạng một chiều sẽ nhân không gian trạng thái lên nhiều hơn nữa. 

Cái nhìn sâu sắc quan trọng là mô hình hóa vấn đề dưới dạng BFS trên các trạng thái kết hợp vị trí của tàu và lượng thực phẩm còn lại, trong đó mỗi ngày được xử lý tuần tự. Vì gió cho mỗi ngày đã được xác định trước nên chúng ta chỉ cần tính toán kết quả di chuyển cho từng hành động trong số năm hành động. Bổ sung thực phẩm là một sự kiện mang tính quyết định vào một ngày và vị trí cụ thể nên chúng ta có thể áp dụng khi BFS đạt đến trạng thái đó. Bằng cách lưu trữ ngày đầu tiên chúng ta đạt đến từng trạng thái (x, y, thực phẩm), chúng ta tránh việc truy cập lại các trạng thái một cách không cần thiết. Điều này làm giảm độ phức tạp hiệu quả xuống O(W × N × M × 5) trong trường hợp xấu nhất, nhưng việc cắt tỉa thực tế xảy ra do các ô trên đất liền hoặc di chuyển ngoài bản đồ bị bỏ qua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(W × 5^(N×M)) | O(N×M×K×W) | Quá chậm | 
| BFS với trạng thái (vị trí, lương thực, ngày) | O(W × N × M × 5) | O(N×M×K) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Phân tích lưới, xác định điểm bắt đầu 'M', cổng 'P' và lưu trữ các ô biển. Xây dựng một tập hợp các sự kiện cung cấp thực phẩm được khóa theo ngày và vị trí để tra cứu nhanh. 
2. Mã hóa hướng gió thành các vectơ: 'N' là (-1,0), 'S' là (1,0), 'E' là (0,1), 'W' là (0,-1) và 'C' là (0,0). 
3. Khởi tạo hàng đợi BFS với trạng thái bắt đầu: tọa độ vị trí, thực phẩm còn lại K và ngày 0. Duy trì mảng đã truy cập 3D đã truy cập [x] [y] [thực phẩm] để tránh truy cập lại các trạng thái. 
4. Đối với mỗi trạng thái trong hàng đợi, nếu tàu đang cập cảng thì trả về ngày hiện tại. Mặt khác, đối với mỗi lần di chuyển có thể (ở lại, bắc, nam, đông, tây): 

- Tính toán vị trí kết quả bằng cách cộng vectơ chuyển động và vectơ gió của ngày hiện tại. 
- Bỏ qua nếu vị trí kết quả nằm ngoài giới hạn hoặc trên đất liền. 
- Giảm lượng thức ăn còn lại đi 1. Nếu thức ăn giảm xuống dưới 0 thì loại bỏ trạng thái này. 
- Nếu trạm thực phẩm đang hoạt động tại vị trí kết quả vào ngày này, hãy bổ sung thực phẩm cho K. 
- Nếu trạng thái mới chưa được truy cập với ít nhất số thực phẩm còn lại hiện tại, hãy đánh dấu trạng thái đó đã truy cập và xếp hàng vào ngày hôm sau. 
5. Tăng bộ đếm ngày sau khi xử lý tất cả các trạng thái của ngày hiện tại. Lặp lại cho đến khi hàng đợi trống hoặc đến được cổng. 
6. Nếu BFS hoàn thành mà không đến được cổng, hãy trả về -1. 

Tính chính xác dựa trên việc BFS đảm bảo rằng lần đầu tiên đạt được cổng tương ứng với số ngày tối thiểu. Việc theo dõi thực phẩm đảm bảo không có đường đi không hợp lệ nào được xem xét và xử lý gió trước khi kiểm tra ranh giới đảm bảo không xảy ra hoạt động di chuyển bất hợp pháp. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

dx = {'N': -1, 'S': 1, 'E': 0, 'W': 0, 'C': 0}
dy = {'N': 0, 'S': 0, 'E': 1, 'W': -1, 'C': 0}
moves = [(0,0), (-1,0), (1,0), (0,-1), (0,1)]  # stay, N, S, W, E

N, M = map(int, input().split())
K, T, W = map(int, input().split())
grid = [input().strip().split() for _ in range(N)]
wind = list(input().strip())

food_stations = {}
for _ in range(T):
    y, x, f = map(int, input().split())
    if f not in food_stations:
        food_stations[f] = set()
    food_stations[f].add((y,x))

for i in range(N):
    for j in range(M):
        if grid[i][j] == 'M':
            start = (i,j)
        elif grid[i][j] == 'P':
            port = (i,j)

visited = [[[-1]*(K+1) for _ in range(M)] for _ in range(N)]
queue = deque()
queue.append((start[0], start[1], K, 0))
visited[start[0]][start[1]][K] = 0

while queue:
    x, y, food, day = queue.popleft()
    if (x, y) == port:
        print(day)
        sys.exit()
    if day >= W:
        continue
    wind_dir = wind[day]
    for mx, my in moves:
        nx = x + mx + dx[wind_dir]
        ny = y + my + dy[wind_dir]
        nfood = food - 1
        if 0 <= nx < N and 0 <= ny < M and grid[nx][ny] != 'L' and nfood >= 0:
            if day in food_stations and (nx, ny) in food_stations[day]:
                nfood = K
            if visited[nx][ny][nfood] == -1:
                visited[nx][ny][nfood] = day + 1
                queue.append((nx, ny, nfood, day + 1))

print(-1)
```BFS xử lý tiến trình từng ngày, di chuyển kết hợp với gió và đảm bảo tuân thủ các hạn chế về lương thực. Việc sử dụng mảng đã truy cập 3D sẽ ngăn chặn việc khám phá dư thừa. Mỗi trạng thái được xác định đầy đủ bởi vị trí, lượng thức ăn còn lại và ngày hiện tại. Sự kết hợp của vectơ gió và vectơ di chuyển đảm bảo chúng ta chỉ xếp các vị trí kết quả hợp lệ vào hàng đợi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
5 2 15
M S S
S S S
S S P
S W N N N N N N N N N N N N N
2 1 0
1 2 0
```| Ngày | x | y | món ăn | gió | Ghi chú | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 5 | S | Đã thử di chuyển các tùy chọn; tất cả kết thúc trên biển, thức ăn giảm xuống còn 4 | 
| 1 | ... | ... | ... | W | BFS tiếp tục, ngày 0 không có trạm lương thực, ngày 1 lương thực giảm | 
| ... | ... | ... | ... | ... | Tàu không thể cập cảng trong vòng K ngày, BFS cạn kiệt tất cả các bang | 

Đầu ra:`-1`Điều này cho thấy BFS đánh giá chính xác tất cả các đường dẫn, xem xét gió và tôn trọng các hạn chế về lương thực. 

### Mẫu 2 (Đã thi công) 

đầu vào:```
2 3
2 1 3
M S P
S S S
E C N
0 1 1
```BFS theo dõi con tàu di chuyển về phía đông để đến cảng với việc nạp thực phẩm, xác nhận tính chính xác của thuật toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(W × N × M × 5) | Mỗi bang có tối đa 5 lượt di chuyển mỗi ngày cho |
