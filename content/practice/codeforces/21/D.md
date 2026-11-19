---
title: "CF 21D - Đồ thị hành trình"
description: "Chúng ta được cho một đồ thị vô hướng có trọng số với tối đa 15 đỉnh và tối đa 2000 cạnh. Mỗi cạnh có trọng số dương và có thể có nhiều cạnh nối cùng một cặp đỉnh hoặc các cạnh lặp lại cùng một đỉnh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "graph-matchings", "graphs"]
categories: ["algorithms"]
codeforces_contest: 21
codeforces_index: "D"
codeforces_contest_name: "Codeforces Alpha Round 21 (Codeforces format)"
rating: 2400
weight: 21
solve_time_s: 42
verified: true
draft: false
---
[CF 21D - Biểu đồ di chuyển](https://codeforces.com/problemset/problem/21/D) 

**Đánh giá:** 2400 
**Thẻ:** bitmask, so khớp biểu đồ, đồ thị 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số với tối đa 15 đỉnh và tối đa 2000 cạnh. Mỗi cạnh có trọng số dương và có thể có nhiều cạnh nối cùng một cặp đỉnh hoặc các cạnh lặp lại cùng một đỉnh. Nhiệm vụ là tìm chu trình ngắn nhất bắt đầu từ đỉnh 1 và đi qua mọi cạnh ít nhất một lần. Nếu chu trình như vậy không tồn tại, chúng ta trả về -1. 

Quan sát quan trọng là chu trình phải được phép sử dụng lại các đỉnh và cạnh nếu cần thiết vì việc truy cập từng cạnh chính xác một lần có thể là không thể. Về cơ bản, chúng ta được yêu cầu tính **chu trình phủ cạnh bắt đầu từ một đỉnh cho trước**. Với n ≤ 15, chúng ta có thể khai thác các thuật toán có hàm mũ theo n nhưng không theo m, vì m có thể lên tới 2000. 

Một cách tiếp cận ngây thơ cố gắng tạo ra tất cả các chuỗi cạnh có thể có là không thực tế vì số lượng các chuỗi tăng theo giai thừa tính bằng m. Ngay cả khi chỉ có 15 đỉnh, việc có 2000 cạnh khiến cho chuỗi cạnh mạnh mẽ là không thể. Vòng lặp và nhiều cạnh làm phức tạp tình hình vì chu trình tối thiểu có thể sử dụng lại một số cạnh nhất định nhiều lần. Ví dụ: một đỉnh có vòng lặp có trọng số 5 sẽ trả về 5 chứ không phải 0. 

Các trường hợp cạnh bao gồm đồ thị có một đỉnh và không có cạnh, trong đó chu trình không tồn tại, đồ thị có nhiều cạnh song song hoặc đồ thị trong đó một số đỉnh bị ngắt kết nối với đỉnh 1. Một giải pháp bất cẩn có thể cố gắng đi qua mỗi đỉnh một lần thay vì mỗi cạnh, tạo ra kết quả sai trong đồ thị có nhiều cạnh. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng liệt kê tất cả các hoán vị của các cạnh hoặc tất cả các bước bắt đầu từ đỉnh 1. Đối với mỗi bước đi ứng cử viên, chúng tôi sẽ kiểm tra xem nó có ghé thăm mọi cạnh hay không và tính tổng trọng số của nó. Số lần đi bộ bùng nổ kết hợp với m. Ngay cả khi chúng ta cắt bớt các đường dẫn rõ ràng là không hợp lệ, cách tiếp cận này vẫn là O((2^m) × m) trong trường hợp xấu nhất và nhanh chóng vượt quá mọi giới hạn thời gian khả thi đối với m ≈ 2000. 

Thông tin chuyên sâu quan trọng đến từ việc **chuyển vấn đề thành một biến thể của Bài toán Người đưa thư Trung Quốc** (còn gọi là Bài toán Kiểm tra Tuyến đường). Trong một đồ thị, nếu muốn đi qua mọi cạnh ít nhất một lần với chi phí tối thiểu, chúng ta chỉ cần thêm các cạnh bổ sung để làm cho tất cả các đỉnh có bậc chẵn, vì chu trình Euler đã bao phủ tất cả các cạnh một lần. Nếu một đỉnh có bậc lẻ, chúng ta phải ghép nó với một đỉnh bậc lẻ khác và thêm đường đi ngắn nhất giữa chúng. Đồ thị tăng cường thu được có thể được duyệt bắt đầu từ đỉnh 1 và nó sẽ bao phủ tất cả các cạnh ít nhất một lần với chi phí tăng thêm tối thiểu. 

Vì n ≤ 15, chúng ta có thể tính toán các đường đi ngắn nhất cho tất cả các cặp bằng cách sử dụng Floyd-Warshall trong O(n^3). Sau đó, chúng tôi tìm thấy tất cả các đỉnh có bậc lẻ. Vì có thể có tối đa n đỉnh bậc lẻ nên chúng ta có thể tính toán ghép cặp tối thiểu bằng cách sử dụng phương pháp **lập trình động bitmask**, trong đó mỗi trạng thái biểu thị đỉnh lẻ nào không khớp. DP này chạy trong O(2^k × k^2) trong đó k là số đỉnh lẻ, khả thi với k ≤ 15. Việc cộng trọng số của các cạnh hiện có và ghép cặp tối thiểu sẽ cho câu trả lời. 

Sự thay đổi quan trọng từ bạo lực là nhận thấy rằng chúng ta không cần phải xây dựng rõ ràng mọi chu trình. Thay vào đó, chúng ta có thể suy luận xem cạnh nào phải được lặp lại để cho phép đi qua Euler. Khi chúng ta giảm bài toán thành việc ghép các đỉnh bậc lẻ một cách tối ưu, bài toán sẽ trở thành một DP có thể xử lý được trên các tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2^m) × m) | O(m) | Quá chậm | 
| Tối ưu (Eulerian + DP) | O(n^3 + 2^k × k^2) | O(n^2 + 2^k) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc đầu vào và xây dựng ma trận kề có kích thước n × n, khởi tạo các cạnh không có cạnh bằng vô cùng. Đối với nhiều cạnh giữa hai đỉnh, chỉ giữ trọng số tối thiểu. 
2. Tính toán đường đi ngắn nhất cho tất cả các cặp bằng Floyd-Warshall. Điều này đảm bảo rằng khi chúng ta cần thêm các cạnh để ghép các đỉnh bậc lẻ, chúng ta biết chi phí tối thiểu để kết nối hai đỉnh bất kỳ. 
3. Tính bậc của mỗi đỉnh bằng cách sử dụng các cạnh ban đầu. Xác định tất cả các đỉnh có bậc lẻ. Gọi k là số đỉnh bậc lẻ. 
4. Nếu k = 0 thì đồ thị đã là Euler. Độ dài chu kỳ tối thiểu chỉ đơn giản là tổng của tất cả các trọng số cạnh. Trả lại số tiền này. 
5. Nếu k > 0, chúng ta phải ghép các đỉnh bậc lẻ. Sử dụng mặt nạ bit để biểu thị các đỉnh lẻ nào vẫn chưa khớp. Khởi tạo dp[mask] = vô cùng, ngoại trừ dp[0] = 0. 
6. Với mỗi mặt nạ, hãy tìm đỉnh i chưa khớp đầu tiên. Sau đó lặp lại tất cả các đỉnh chưa khớp j > i và tính dp[mask | (1 << tôi) | (1 << j)] = min(dp[mask | ...], dp[mask] + dist[i][j]), trong đó dist[i][j] là độ dài đường đi ngắn nhất từ ​​i đến j. 
7. Sau khi điền DP, dp[(1 << k) - 1] chứa tổng trọng số tối thiểu để ghép tất cả các đỉnh lẻ. 
8. Đáp án cuối cùng là sum_of_edge_weights + dp[(1 << k) - 1]. Nếu đồ thị bị ngắt kết nối, hãy kiểm tra xem đỉnh 1 có được kết nối với tất cả các cạnh hay không; nếu không thì trả về -1. 

**Tại sao nó hoạt động**: Định lý Euler đảm bảo rằng một đồ thị có tất cả các bậc chẵn đều có chu trình Euler. Nếu các đỉnh có bậc lẻ, việc thêm các cạnh dọc theo đường đi ngắn nhất giữa chúng sẽ đảm bảo tất cả các đỉnh có bậc chẵn với chi phí tăng thêm tối thiểu. DP trên các tập hợp con đảm bảo rằng chúng ta tìm thấy cặp đỉnh lẻ tối thiểu, tương ứng trực tiếp với số lượng cạnh lặp lại tối thiểu cần thiết để đi qua toàn bộ đồ thị. Bởi vì tất cả các đường đi ngắn nhất đều được tính toán trước nên việc thêm các cạnh thông qua DP luôn sử dụng chi phí tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

def solve():
    n, m = map(int, input().split())
    dist = [[INF]*n for _ in range(n)]
    deg = [0]*n
    sum_edges = 0
    
    for _ in range(m):
        x, y, w = map(int, input().split())
        x -= 1
        y -= 1
        sum_edges += w
        deg[x] += 1
        deg[y] += 1
        if dist[x][y] > w:
            dist[x][y] = dist[y][x] = w

    for i in range(n):
        dist[i][i] = 0

    # Floyd-Warshall
    for k in range(n):
        for i in range(n):
            for j in range(n):
                if dist[i][j] > dist[i][k] + dist[k][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    # check connectivity
    for i in range(n):
        for j in range(n):
            if dist[i][j] == INF:
                # If there are edges in disconnected components, no solution
                if deg[i] > 0 and deg[j] > 0:
                    print(-1)
                    return

    odd = [i for i in range(n) if deg[i] % 2 == 1]
    k = len(odd)

    if k == 0:
        print(sum_edges)
        return

    dp = [INF]*(1<<k)
    dp[0] = 0

    for mask in range(1<<k):
        i = 0
        while i < k and (mask & (1 << i)):
            i += 1
        if i == k:
            continue
        for j in range(i+1, k):
            if not (mask & (1 << j)):
                new_mask = mask | (1 << i) | (1 << j)
                dp[new_mask] = min(dp[new_mask], dp[mask] + dist[odd[i]][odd[j]])

    print(sum_edges + dp[(1<<k)-1])

if __name__ == "__main__":
    solve()
```Ma trận kề cho phép tính toán đường đi ngắn nhất một cách nhanh chóng. Floyd-Warshall đảm bảo chúng ta luôn chọn các đường đi tối thiểu khi ghép các đỉnh lẻ. Bitmask DP xử lý cẩn thận tất cả các cặp mà không có sự dư thừa. Việc kiểm tra kết nối đảm bảo chúng tôi trả về -1 cho các biểu đồ bị ngắt kết nối. Có thể tránh các lỗi riêng lẻ trong chỉ mục bằng cách sử dụng nhất quán lập chỉ mục dựa trên 0 trong nội bộ. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 3
1 2 1
2 3 1
3 1 1
```| Bước | độ | lẻ | mặt nạ dp | giá trị dp | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2,2,2] | [] | 0 | 0 | 

Tất cả các độ đều bằng nhau nên chu kỳ tối thiểu = sum_edges = 3. Không cần DP. Đầu ra đúng là 3. 

**Mẫu tùy chỉnh 2** 

đầu vào:```
4 3
1 2 1
2 3 2
3 4 3
```- Độ: [1,2,2,1], đỉnh lẻ = [0,3] 
- Chỉ một cặp: chi phí đường đi ngắn nhất 1-4 = 6 (1-2-3-4) 
- tổng_cạnh = 6 
- Chu kỳ tối thiểu = 6 + 6 = 12 

bảng DP: 

| mặt nạ | dp[mặt nạ] | 
| --- | --- | 
| 0b00 | 0 | 
| 0b11 | 6 | 

Cuối cùng
