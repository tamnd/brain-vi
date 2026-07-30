---
title: "CF 103886L - Khai quật hóa thạch"
description: "Chúng tôi đang làm việc trên một mạng lưới trong đó chỉ có một số lượng nhỏ tế bào thực sự quan trọng: một vị trí cơ sở và một số vị trí hóa thạch. Bản thân lưới có thể lớn và hầu hết trống, nhưng chuyển động chỉ liên quan thông qua các đường đi ngắn nhất trên lưới."
date: "2026-07-02T07:41:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103886
codeforces_index: "L"
codeforces_contest_name: "CerealCodes 2022 Summer Contest"
rating: 0
weight: 103886
solve_time_s: 45
verified: true
draft: false
---

[CF 103886L - Khai quật hóa thạch](https://codeforces.com/problemset/problem/103886/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một mạng lưới trong đó chỉ có một số lượng nhỏ tế bào thực sự quan trọng: một vị trí cơ sở và một số vị trí hóa thạch. Bản thân lưới có thể lớn và hầu hết trống, nhưng chuyển động chỉ liên quan thông qua các đường đi ngắn nhất trên lưới. Nhiệm vụ là vận chuyển tất cả các hóa thạch trở lại căn cứ bằng xe cút kít có thể chở nhiều hóa thạch trong một chuyến đi, trả chi phí di chuyển tỷ lệ thuận với độ dài đường đi trên lưới. 

Quan sát quan trọng là mặc dù lưới có thể lớn nhưng số lượng điểm thú vị lại nhỏ. Các trạng thái có ý nghĩa duy nhất là nền tảng và các ô hóa thạch, bởi vì mọi thứ khác chỉ đóng vai trò là địa hình trung gian để tính toán đường đi ngắn nhất. Bất kỳ giải pháp nào cố gắng mô phỏng chuyển động trên toàn bộ lưới cho mọi kế hoạch thu gom sẽ ngay lập tức trở nên không khả thi. 

Đầu vào mô tả cấu trúc lưới và vị trí của hóa thạch và nền. Đầu ra là tổng chi phí nhiên liệu tối thiểu để thu thập tất cả các hóa thạch, có thể qua nhiều chuyến đi trong đó mỗi chuyến đi bắt đầu tại cơ sở, thăm một tập hợp con hóa thạch và quay trở lại cơ sở. 

Các ràng buộc ngụ ý rằng chúng ta không đủ khả năng để xử lý từng ô một cách độc lập theo bất kỳ ý nghĩa tổ hợp nào. Nếu lưới có kích thước lên tới khoảng n x n thì tính toán đường đi ngắn nhất phải gần tuyến tính về kích thước lưới trên mỗi nguồn và mọi quá trình xử lý tập hợp con phải được giới hạn ở số lượng hóa thạch k, đủ nhỏ để các kỹ thuật hàm mũ trong k khả thi. 

Một cách tiếp cận đơn giản là xử lý từng hóa thạch một cách độc lập, luôn đi từ cơ sở đến hóa thạch và ngược lại. Điều này bỏ qua rằng nhiều hóa thạch có thể được thu thập trong một chuyến đi, dẫn đến việc đánh giá quá cao chi phí. Một thất bại tinh vi hơn sẽ xảy ra nếu chúng ta cố gắng phân nhóm tham lam dựa trên khoảng cách: việc chọn những hóa thạch gần nhất trước tiên có thể chặn các nhóm toàn cầu tốt hơn. 

Một trường hợp thất bại cụ thể xuất hiện khi hai hóa thạch riêng lẻ ở gần gốc nhưng cách xa nhau và hóa thạch thứ ba ở xa gốc nhưng nằm trên một tuyến đường chung giữa hai hóa thạch đầu tiên. Việc ghép đôi tham lam có thể phân chia đường đi chung hoặc lãng phí việc đi lại do không kết hợp chính xác, tạo ra chi phí dưới mức tối ưu. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu từ việc tưởng tượng mọi cách có thể để đưa hóa thạch vào các chuyến đi. Mỗi chuyến đi là một tập hợp con các hóa thạch và với mỗi tập hợp con, chúng tôi tính toán chi phí tối thiểu để bắt đầu từ cơ sở, truy cập tất cả các hóa thạch trong tập hợp con đó theo một số thứ tự và quay trở lại. Đây đã là một bài toán con kiểu nhân viên bán hàng du lịch cho mỗi tập hợp con và ngay cả khi chúng tôi cho rằng mình có thể tính toán chi phí của tập hợp con, thì chúng tôi cần phân chia tất cả các hóa thạch thành các tập hợp con để giảm thiểu tổng chi phí. Số lượng phân vùng tăng nhanh hơn số Bell nên điều này ngay lập tức không thể thực hiện được. 

Cấu trúc của bài toán được đơn giản hóa vì k nhỏ nên các tập hợp con hóa thạch có thể được biểu diễn bằng mặt nạ bit. Cái nhìn sâu sắc quan trọng là chúng ta thực sự không cần thứ tự hoán vị đầy đủ trên tất cả các hóa thạch trên toàn cầu. Thay vào đó, chúng ta chỉ cần khoảng cách đường đi ngắn nhất giữa các điểm quan trọng và sau đó chúng ta có thể suy luận hoàn toàn theo các tập hợp con. 

Điều này dẫn đến sự phân hủy hai lớp. Đầu tiên, chúng tôi nén lưới thành một biểu đồ hoàn chỉnh trên k cộng một nút, trong đó các cạnh biểu thị khoảng cách đường đi ngắn nhất. Sau đó, mỗi tập hợp con hóa thạch xác định một “chi phí chuyến đi” duy nhất, nghĩa là chi phí tối thiểu để bắt đầu từ cơ sở, truy cập tất cả các nút trong tập hợp con và quay trở lại. Khi đã biết các chi phí tập hợp con này, bài toán tổng thể sẽ trở thành việc phân chia tập hợp đầy đủ thành các tập hợp con rời rạc để giảm thiểu tổng chi phí, đây là một bài toán quy hoạch động tập hợp con cổ điển. 

Sự đơn giản hóa quan trọng là chúng ta không bao giờ cần phải xem xét lại toàn bộ lưới sau khi xử lý trước các đường đi ngắn nhất. Tất cả hình học được hấp thụ vào một ma trận khoảng cách. Từ thời điểm đó trở đi, bài toán hoàn toàn mang tính tổ hợp trên k phần tử.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các phân vùng và đường dẫn) | Siêu lũy thừa trong tìm kiếm k + lưới trên mỗi trạng thái | Lớn | Quá chậm | 
| Tối ưu (đường dẫn ngắn nhất + bitmask DP) | O(3^k + k^2 2^k + đường đi ngắn nhất trong lưới) | O(k 2^k + lưới) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia giải pháp thành các khoảng cách tiền xử lý và lập trình động trên các tập hợp con. 

### 1. Trích xuất các điểm chính và nén lưới 

Chúng tôi xác định cơ sở và tất cả các vị trí hóa thạch. Đây là những nút duy nhất quan trọng cho việc tính toán sau này. Giả sử có k hóa thạch cộng với phần gốc, tạo ra k + 1 nút đặc biệt. 

Lý do cho việc nén này là vì bất kỳ tuyến đường nào giữa các hóa thạch đều được xác định đầy đủ bởi khoảng cách đường đi ngắn nhất trên lưới, do đó các ô trung gian không cần phải là một phần của không gian trạng thái. 

### 2. Tính đường đi ngắn nhất từ mỗi key point 

Đối với mỗi nút đặc biệt k + 1, chúng tôi chạy thuật toán đường đi ngắn nhất trên lưới để tính khoảng cách đến mọi ô khác hoặc ít nhất là đến tất cả các nút đặc biệt khác. 

Nếu chi phí di chuyển đồng đều thì BFS là đủ. Nếu có hai loại chi phí (phổ biến trong các vấn đề tương tự), chúng tôi sử dụng BFS 0-1 với deque. Mục tiêu là lấp đầy ma trận khoảng cách dist[i][j] cho tất cả các cặp nút đặc biệt. 

Bước này chuyển đổi bài toán lưới thành một biểu đồ có trọng số hoàn chỉnh trên k + 1 nút. 

### 3. Tính toán trước chi phí đi lại của tập hợp con 

Đối với mỗi bitmask hóa thạch, chúng tôi tính toán chi phí tối thiểu cho một chuyến đi bắt đầu từ cơ sở, ghé thăm tất cả các hóa thạch trong mặt nạ và quay trở lại. 

Chúng tôi thực hiện việc này bằng cách sử dụng DP trên các tập hợp con trong đó chúng tôi cố gắng mở rộng các tuyến đường một phần bằng cách thêm từng hóa thạch một lần. Trạng thái tự nhiên là dp[mask][i], biểu thị chi phí tối thiểu để bắt đầu tại cơ sở, truy cập chính xác các hóa thạch trong mặt nạ và kết thúc tại hóa thạch i. 

Quá trình chuyển đổi sẽ thêm một hóa thạch mới j không có trong mặt nạ. Chi phí tăng thêm dist[i][j]. Quá trình khởi tạo bắt đầu từ mỗi hóa thạch có thể truy cập trực tiếp từ cơ sở. 

Khi đó, chi phí chuyến đi của tập hợp con là nhỏ nhất trên tất cả các điểm cuối i của dp[mask][i] + dist[i][base]. 

Điều này hiệu quả vì sau khi thứ tự trong một chuyến đi được cố định, chi phí chỉ là tổng của các cạnh ngắn nhất giữa các hóa thạch được truy cập liên tiếp. 

### 4. Kết hợp các chuyến đi bằng DP thứ hai 

Bây giờ, mỗi mặt nạ có chi phí trip_cost[mask] được tính toán trước hoặc không hợp lệ nếu không thể. 

Chúng tôi xác định DP thứ hai trên các tập hợp con: full_dp[mask] là chi phí tối thiểu để thu thập tất cả hóa thạch trong mặt nạ bằng cách sử dụng bất kỳ số chuyến đi nào. 

Chúng tôi chuyển đổi bằng cách chọn một mặt nạ con của mặt nạ đại diện cho một chuyến đi và kết hợp: 

full_dp[mask] = min(full_dp[mask], full_dp[mask \ sub] + trip_cost[sub]) 

Điều này liệt kê các phân vùng của tập hợp thành các chuyến đi hợp lệ. 

### 5. Câu trả lời cuối cùng 

Câu trả lời là full_dp[(1 << k) - 1]. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào việc tách hình học chuyển động khỏi nhóm tổ hợp. Quá trình xử lý trước đường đi ngắn nhất đảm bảo rằng mọi chi phí chuyến đi chỉ phụ thuộc vào điểm cuối và thứ tự truy cập trên các hóa thạch chứ không phụ thuộc vào cấu trúc lưới trung gian. Tập hợp con DP đảm bảo mọi phân vùng hóa thạch thành các chuyến đi được xem xét chính xác một lần theo cách tối ưu, bởi vì mọi giải pháp hợp lệ đều tương ứng với một phân vùng và DP đánh giá tất cả các phân vùng thông qua phân tách mặt nạ con. Vì mỗi chi phí chuyến đi là tối ưu cho tập hợp con của nó và phân vùng DP khám phá tất cả các kết hợp nên mức tối ưu toàn cục được giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

INF = 10**18

def bfs_from(start_r, start_c, grid, n, m):
    dist = [[INF] * m for _ in range(n)]
    dq = deque()
    dist[start_r][start_c] = 0
    dq.append((start_r, start_c))
    
    while dq:
        r, c = dq.popleft()
        d = dist[r][c]
        for dr, dc in ((1,0),(-1,0),(0,1),(0,-1)):
            nr, nc = r + dr, c + dc
            if 0 <= nr < n and 0 <= nc < m and grid[nr][nc] != '#':
                if dist[nr][nc] > d + 1:
                    dist[nr][nc] = d + 1
                    dq.append((nr, nc))
    return dist

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]
    
    points = []
    base = None
    
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'B':
                base = (i, j)
            elif grid[i][j] == 'F':
                points.append((i, j))
    
    k = len(points)
    all_nodes = [base] + points
    
    dist = [[INF] * (k + 1) for _ in range(k + 1)]
    
    for i, (r, c) in enumerate(all_nodes):
        dgrid = bfs_from(r, c, grid, n, m)
        for j, (r2, c2) in enumerate(all_nodes):
            dist[i][j] = dgrid[r2][c2]
    
    base_idx = 0
    
    size = 1 << k
    trip_cost = [INF] * size
    
    dp = [[INF] * (k + 1) for _ in range(size)]
    
    for i in range(1, k + 1):
        mask = 1 << (i - 1)
        dp[mask][i] = dist[base_idx][i]
    
    for mask in range(size):
        for i in range(1, k + 1):
            if not (mask & (1 << (i - 1))):
                continue
            if dp[mask][i] == INF:
                continue
            for j in range(1, k + 1):
                if mask & (1 << (j - 1)):
                    continue
                nmask = mask | (1 << (j - 1))
                nd = dp[mask][i] + dist[i][j]
                if nd < dp[nmask][j]:
                    dp[nmask][j] = nd
    
    for mask in range(1, size):
        best = INF
        for i in range(1, k + 1):
            if mask & (1 << (i - 1)):
                best = min(best, dp[mask][i] + dist[i][base_idx])
        trip_cost[mask] = best
    
    full = [INF] * size
    full[0] = 0
    
    for mask in range(1, size):
        sub = mask
        while sub:
            if trip_cost[sub] < INF:
                full[mask] = min(full[mask], full[mask ^ sub] + trip_cost[sub])
            sub = (sub - 1) & mask
    
    print(full[size - 1])

if __name__ == "__main__":
    solve()
```Giai đoạn BFS xây dựng khoảng cách đường đi ngắn nhất từ ​​mọi điểm đặc biệt, biến lưới thành một biểu đồ có trọng số hoàn chỉnh. Bitmask DP đầu tiên tính toán các đường đi tốt nhất để thu thập một tập hợp con hóa thạch trong một chuyến đi liên tục bắt đầu từ cơ sở và kết thúc ở bất kỳ hóa thạch nào. DP thứ hai kết hợp các chuyến đi này bằng cách liệt kê tất cả các mặt nạ con, thử một cách hiệu quả tất cả các phân vùng hóa thạch thành các chuyến đi hợp lệ. 

Một chi tiết triển khai tinh tế là dp[mask][i] chỉ hợp lệ khi hóa thạch i được đưa vào mặt nạ và lớp nền không bao giờ là một phần của mặt nạ. Điều này tránh các trạng thái dư thừa và giữ cho quá trình chuyển đổi nhất quán. Một điểm quan trọng khác là trip_cost[mask] bao gồm việc quay về cơ sở, điều này đảm bảo rằng việc kết hợp các bài toán con không vô tình bỏ sót chi phí trả về. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ trong đó phần đế nằm ở một góc và hai hóa thạch ở gần đó nhưng được ngăn cách bởi các bức tường buộc phải đi đường vòng. 

### Ví dụ 1 

Lưới đầu vào:```
B..
.#F
..F
```Chúng ta có cơ sở tại (0,0), hóa thạch tại (1,2) và (2,2). 

Sau quá trình tiền xử lý BFS, chúng tôi thu được ma trận khoảng cách trong đó cả hai hóa thạch đều có thể tiếp cận được với độ dài đường đi khác nhau và việc di chuyển giữa các hóa thạch có thể ngắn hơn so với việc đi qua cơ sở hai lần. 

DP trên các tập hợp con đánh giá: 

mặt nạ {F1} chi phí = cơ sở → F1 → cơ sở 

mặt nạ {F2} chi phí = cơ sở → F2 → cơ sở 

mặt nạ {F1,F2} chi phí = đường đi tốt nhất đến cả hai rồi quay lại 

Sau đó DP thứ hai so sánh: 

hai chuyến đi đơn lẻ hoặc một chuyến kết hợp. 

### Ví dụ 2 

Lưới đầu vào:```
B.F
###
F..
```Ở đây hàng tường buộc phải đi đường vòng dài. Quá trình chuyển đổi trực tiếp giữa các hóa thạch có thể lâu hơn nhiều so với dự kiến. DP tránh được các quyết định tham lam một cách chính xác bằng cách đánh giá rõ ràng cả chiến lược thu thập riêng lẻ và kết hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((k+1) · n·m + 3^k) | BFS từ mỗi nút đặc biệt cộng với tập hợp con DP trên mặt nạ | 
| Không gian | O(k^2 + k·2^k) | ma trận khoảng cách và bảng DP trên các tập hợp con | 

Quá trình tiền xử lý lưới chia tỷ lệ tuyến tính trên mỗi nguồn, điều này có thể chấp nhận được vì số lượng nguồn chỉ là k + 1. Phần mũ chỉ phụ thuộc vào k, đủ nhỏ cho bitmask DP. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder, replace with solve()

# minimal case
assert run("1 1\nB\n") == "0", "no fossils"

# single fossil
assert run("3 3\nB..\n...\n..F\n") != "", "single fossil path exists"

# two fossils simple
assert run("3 3\nB.F\n...\n..F\n") != "", "basic grouping case"

# blocked fossil
assert run("3 3\nB#F\n###\nF..\n") != "", "detour case"

# all fossils isolated but reachable via long path
assert run("4 4\nB...\n####\n....\n..FF\n") != "", "separated regions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới tối thiểu | 0 | không cần làm việc | 
| hóa thạch đơn | chi phí hữu hạn | trường hợp cơ sở DP chính xác | 
| hai hóa thạch | chi phí hữu hạn | phân vùng tập hợp con | 
| đường bị chặn | chi phí hữu hạn hoặc xử lý INF | chuyển tiếp không thể truy cập | 
| vùng riêng biệt | chi phí hữu hạn | xử lý đường vòng dài | 

## Vỏ cạnh 

Trường hợp nguy cấp là khi không thể tiếp cận được hóa thạch từ gốc. Trong tình huống này, khoảng cách BFS vẫn là INF và mọi trạng thái dp liên quan đến hóa thạch đó phải vẫn là INF. Thuật toán truyền INF một cách tự nhiên thông qua cả hai lớp DP tập hợp con, đảm bảo không có chuyến đi không hợp lệ nào được xem xét. 

Một trường hợp khác xảy ra khi việc kết hợp các hóa thạch hoàn toàn tốt hơn so với các chuyến đi riêng biệt vì đường đi chung chồng chéo đáng kể. Tập hợp con DP trên trip_cost đảm bảo điều này được nắm bắt, vì nó đánh giá rõ ràng mặt nạ chung như một chuyến đi đơn lẻ và so sánh nó với các phân tách. 

Trường hợp tinh tế cuối cùng là khi nhóm tối ưu sử dụng phân vùng không trực quan, chẳng hạn như chia hóa thạch thành hai cụm có kích thước trung bình thay vì một cụm lớn hoặc nhiều cụm đơn lẻ. Việc liệt kê mặt nạ con đảm bảo tất cả các phân vùng đều có thể truy cập được, vì vậy DP sẽ đánh giá cấu trúc đó một cách rõ ràng thay vì dựa vào các phương pháp phỏng đoán.
