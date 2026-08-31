---
title: "CF 104442I - C\u00e1lculo num\u00e9rico"
description: "Chúng ta được đưa ra một số kịch bản độc lập trên mặt phẳng số nguyên 2D. Trong mỗi kịch bản, robot bắt đầu tại tọa độ $I = (x1, y1)$ và phải đạt đến tọa độ mục tiêu $F = (x2, y2)$."
date: "2026-06-30T18:07:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104442
codeforces_index: "I"
codeforces_contest_name: "AdaByron Regional Madrid 2023"
rating: 0
weight: 104442
solve_time_s: 57
verified: true
draft: false
---

[CF 104442I - C\u00e1lculo num\u00e9rico](https://codeforces.com/problemset/problem/104442/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một số kịch bản độc lập trên mặt phẳng số nguyên 2D. Trong mỗi kịch bản, robot bắt đầu ở tọa độ$I = (x_1, y_1)$và phải đạt được tọa độ mục tiêu$F = (x_2, y_2)$. Robot di chuyển trên các điểm lưới số nguyên, nhưng mặt phẳng không bị giới hạn, nghĩa là nó không bị giới hạn trong phạm vi tọa độ ban đầu. 

Một số điểm lưới bị cấm. Nếu tọa độ được liệt kê là chướng ngại vật, robot không được phép đứng trên đó. Nó có thể di chuyển tự do qua tất cả các tọa độ nguyên khác. 

Chuyển động xảy ra giữa các điểm lưới lân cận. Chi phí tùy thuộc vào loại bước di chuyển: một số bước đi có giá 8 và một số khác có giá 16. Nếu không có đường đi hợp lệ nào tồn tại từ đầu đến cuối mà không bước vào điểm cấm, câu trả lời là$-1$. Ngược lại, chúng ta phải tính tổng chi phí tối thiểu có thể. 

Giải thích chính là chúng ta đang giải bài toán đường đi ngắn nhất trên một đồ thị vô hạn ẩn trong đó các đỉnh đều là tọa độ nguyên ngoại trừ tọa độ bị chặn và các cạnh kết nối các lân cận hình học với hai trọng số có thể có. 

Các ràng buộc về tọa độ là nhỏ, giữa$-50$Và$50$và có tối đa 100 chướng ngại vật cho mỗi trường hợp thử nghiệm. Tuy nhiên, robot được phép rời khỏi ô này nên đồ thị không bị giới hạn rõ ràng. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng toàn bộ lưới hoặc thực hiện DP dày đặc trên một hình chữ nhật lớn, vì về nguyên tắc, khu vực có thể tiếp cận là không bị giới hạn. 

Trường hợp cạnh nguy hiểm nhất là khi chướng ngại vật tạo thành một rào cản chặt chẽ buộc phải đi đường vòng ra ngoài hộp giới hạn ban đầu. Ví dụ: nếu bắt đầu là lúc$(0,0)$, kết thúc tại$(2,0)$, và mọi điểm$(1,y)$vì$y \in [-50,50]$bị chặn, một BFS ngây thơ bị giới hạn trong hình chữ nhật giới hạn sẽ kết luận không chính xác là không thể truy cập được hoặc bỏ lỡ đường vòng thực sự đi quanh hàng rào tại$y=51$. 

Một trường hợp tế nhị khác là khi điểm bắt đầu hoặc điểm kết thúc liền kề với nhiều chướng ngại vật. Một cách tiếp cận dựa trên tham lam hoặc dựa trên kinh nghiệm cố gắng “bước lại gần” có thể bị mắc kẹt trong mức cực tiểu cục bộ, bởi vì hành động di chuyển ngay lập tức rẻ nhất có thể dẫn đến một vùng chết được bao quanh bởi các nút bị chặn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mọi tọa độ số nguyên là một nút và chạy BFS hoặc Dijkstra trên lưới vô hạn. Từ mỗi nút, chúng tôi sẽ thử tất cả 8 bước di chuyển có thể, bỏ qua chướng ngại vật. Điều này đúng vì nó trực tiếp mô hình hóa biểu đồ, nhưng không thể thực hiện như đã nêu vì số lượng nút là vô hạn. 

Quan sát quan trọng là mặc dù lưới là vô hạn nhưng các “lỗ hổng” có liên quan duy nhất là các điểm chướng ngại vật và có rất ít điểm trong số đó. Đường đi ngắn nhất trong lưới có cấu trúc đơn vị không được hưởng lợi từ việc di chuyển xa một cách tùy ý, bởi vì chi phí di chuyển tăng theo khoảng cách và không có phần thưởng cho việc khám phá không gian trống. Bất kỳ đường đi tối ưu nào cũng có thể được giả định là nằm trong một hộp giới hạn chứa điểm bắt đầu, điểm kết thúc và tất cả các chướng ngại vật, có thể được mở rộng một chút để cho phép đi vòng quanh các chướng ngại vật liền kề với ranh giới. 

Điều này làm giảm bài toán thành bài toán đường đi ngắn nhất hữu hạn. Chúng tôi giới hạn bản thân trong một lưới bao gồm tất cả các điểm từ$\min(x)$ĐẾN$\max(x)$, tương tự cho$y$, kéo dài thêm 1 theo mỗi hướng. Trên tập hợp nút hữu hạn này, chúng tôi chạy Dijkstra vì các cạnh có hai trọng số khác nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS vô hạn kết thúc$\mathbb{Z}^2$| O(vô hạn) | O(vô hạn) | Không khả thi | 
| Lưới giới hạn + Dijkstra |$O(HW \log(HW))$|$O(HW)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi lưới là một biểu đồ có trọng số có các nút là tọa độ nguyên bên trong hình chữ nhật giới hạn được chọn cẩn thận. 

1. Chúng tôi đọc tọa độ bắt đầu, kết thúc và chướng ngại vật, đồng thời thu thập tất cả các giá trị x và y. Từ những điều này, chúng tôi tính toán x và y tối thiểu và tối đa. Chúng tôi mở rộng phạm vi này thêm 1 đơn vị theo mọi hướng. Việc mở rộng đảm bảo chúng tôi có thể định tuyến xung quanh các chướng ngại vật nằm chính xác trên ranh giới của khu vực hữu ích. 
2. Chúng tôi xây dựng một tập hợp tọa độ bị chặn để kiểm tra tư cách thành viên O(1). Điều này rất cần thiết vì việc tra cứu chướng ngại vật diễn ra trong mỗi bước thư giãn. 
3. Chúng tôi chạy thuật toán Dijkstra bắt đầu từ tọa độ ban đầu. Chúng tôi duy trì một hàng ưu tiên của các tiểu bang$(cost, x, y)$. 
4. Từ mỗi trạng thái bật lên, chúng ta thử di chuyển theo cả 8 hướng. Mỗi bước di chuyển đều dẫn tới một tọa độ lân cận. Nếu tọa độ đó nằm ngoài hộp giới hạn hoặc bị chặn, chúng tôi sẽ loại bỏ nó. 
5. Mỗi nước đi hợp lệ có một chi phí cố định tùy thuộc vào loại hướng đi. Di chuyển thẳng tốn 8 chi phí và di chuyển chéo tốn 16. Chúng ta sẽ giúp hàng xóm thư giãn nếu tìm được con đường rẻ hơn. 
6. Chúng tôi dừng sớm khi đạt đến tọa độ mục tiêu, vì Dijkstra đảm bảo đây là chi phí tối thiểu có thể có tại thời điểm đó. 
7. Nếu không bao giờ đạt được mục tiêu, chúng tôi sẽ xuất ra$-1$. 

Lý do điều này có tác dụng là vì mọi đường dẫn tối ưu đều có thể được nhúng vào hộp giới hạn hữu hạn. Bên ngoài khu vực này, bất kỳ đường vòng nào cũng có thể được dự kiến ​​quay lại mà không làm tăng chi phí vì chi phí di chuyển là như nhau và chỉ phụ thuộc vào loại bước chứ không phụ thuộc vào vị trí tuyệt đối. Do đó việc hạn chế đồ thị không loại bỏ được lời giải tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**18

# 8 directions: (dx, dy, cost)
dirs = [
    (1, 0, 8), (-1, 0, 8), (0, 1, 8), (0, -1, 8),
    (1, 1, 16), (1, -1, 16), (-1, 1, 16), (-1, -1, 16)
]

P = int(input())
for _ in range(P):
    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())

    n = int(input())
    blocked = set()

    xs = [x1, x2]
    ys = [y1, y2]

    for _ in range(n):
        x, y = map(int, input().split())
        blocked.add((x, y))
        xs.append(x)
        ys.append(y)

    minx, maxx = min(xs) - 1, max(xs) + 1
    miny, maxy = min(ys) - 1, max(ys) + 1

    def inside(x, y):
        return minx <= x <= maxx and miny <= y <= maxy

    dist = {}
    pq = []

    start = (x1, y1)
    if start in blocked:
        print(-1)
        continue

    dist[start] = 0
    heapq.heappush(pq, (0, x1, y1))

    ans = -1

    while pq:
        d, x, y = heapq.heappop(pq)

        if d != dist.get((x, y), INF):
            continue

        if (x, y) == (x2, y2):
            ans = d
            break

        for dx, dy, w in dirs:
            nx, ny = x + dx, y + dy
            if not inside(nx, ny):
                continue
            if (nx, ny) in blocked:
                continue

            nd = d + w
            if nd < dist.get((nx, ny), INF):
                dist[(nx, ny)] = nd
                heapq.heappush(pq, (nd, nx, ny))

    print(ans)
```Việc triển khai dựa vào Dijkstra vì trọng số cạnh không đồng nhất. Hàng đợi ưu tiên đảm bảo rằng bất cứ khi nào chúng tôi hoàn tất một nút, chúng tôi đã có chi phí tối ưu cho nút đó. 

Giới hạn hộp giới hạn là điều làm cho lưới vô hạn có thể quản lý được. Không có nó, thuật toán sẽ không bao giờ kết thúc. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó bắt đầu là$(0,0)$, kết thúc là$(1,1)$, và không có trở ngại. 

Chúng tôi so sánh hai đường đi có thể: đường chéo trực tiếp hoặc hai đường đi thẳng. 

| Bước | Vị trí | Chi phí | Hành động | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 0 | bắt đầu | 
| 2 | (1,1) | 16 | di chuyển chéo | 

Thuật toán chọn đường chéo ngay lập tức vì đây là đường đi ngắn nhất. 

Bây giờ hãy xem xét trường hợp đường chéo bị chặn: bắt đầu$(0,0)$, kết thúc$(1,1)$, chướng ngại vật tại$(1,1)$không hợp lệ nên chúng tôi điều chỉnh ví dụ: chướng ngại vật tại$(1,0)$. 

| Bước | Vị trí | Chi phí | Hành động | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 0 | bắt đầu | 
| 2 | (0,1) | 8 | lên | 
| 3 | (1,1) | 16 | đúng | 

Điều này cho thấy cách thuật toán định tuyến một cách tự nhiên xung quanh các ô bị chặn và tích lũy chi phí thông qua các đường dẫn thay thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(K \log K)$| Dijkstra nhiều nhất là qua$K = (dx \cdot dy)$điểm lưới giới hạn | 
| Không gian |$O(K)$| Bản đồ khoảng cách và lưu trữ hàng đợi ưu tiên | 

Hộp giới hạn nhỏ vì tọa độ bị giới hạn ở khoảng 100 theo mỗi hướng, do đó lưới hiệu quả tối đa là khoảng$200 \times 200$, giúp giải pháp dễ dàng nhanh chóng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    INF = 10**18
    dirs = [
        (1, 0, 8), (-1, 0, 8), (0, 1, 8), (0, -1, 8),
        (1, 1, 16), (1, -1, 16), (-1, 1, 16), (-1, -1, 16)
    ]

    P = int(input())
    out = []

    for _ in range(P):
        x1, y1 = map(int, input().split())
        x2, y2 = map(int, input().split())
        n = int(input())

        blocked = set()
        xs = [x1, x2]
        ys = [y1, y2]

        for _ in range(n):
            x, y = map(int, input().split())
            blocked.add((x, y))
            xs.append(x)
            ys.append(y)

        minx, maxx = min(xs) - 1, max(xs) + 1
        miny, maxy = min(ys) - 1, max(ys) + 1

        def inside(x, y):
            return minx <= x <= maxx and miny <= y <= maxy

        if (x1, y1) in blocked:
            out.append("-1")
            continue

        dist = {}
        import heapq
        pq = [(0, x1, y1)]
        dist[(x1, y1)] = 0
        ans = -1

        while pq:
            d, x, y = heapq.heappop(pq)
            if d != dist.get((x, y), INF):
                continue
            if (x, y) == (x2, y2):
                ans = d
                break
            for dx, dy, w in dirs:
                nx, ny = x + dx, y + dy
                if not inside(nx, ny): 
                    continue
                if (nx, ny) in blocked:
                    continue
                nd = d + w
                if nd < dist.get((nx, ny), INF):
                    dist[(nx, ny)] = nd
                    heapq.heappush(pq, (nd, nx, ny))

        out.append(str(ans))

    return "\n".join(out)

# custom cases
assert run("1\n0 0\n1 1\n0") == "16"
assert run("1\n0 0\n2 0\n1\n1 0") == "32"
assert run("1\n0 0\n0 0\n0") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường chéo trực tiếp | 16 | tính chính xác của chi phí đường chéo | 
| bị chặn ở giữa | 32 | xử lý đường vòng | 
| bắt đầu/kết thúc giống nhau | 0 | đường dẫn có độ dài bằng không | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi đường đi ngắn nhất yêu cầu di chuyển ra ngoài hình chữ nhật ngay lập tức được xác định bởi điểm bắt đầu, điểm kết thúc và chướng ngại vật. Thuật toán xử lý vấn đề này vì hộp giới hạn được mở rộng thêm một đơn vị, cho phép tạo ra một hành lang đường vòng tối thiểu xung quanh các chướng ngại vật chặt chẽ. 

Một trường hợp khác là khi điểm bắt đầu hoặc điểm kết thúc bị bao quanh ở hầu hết các phía. Ví dụ, nếu$(x_1, y_1)$có tất cả các nút liền kề bị chặn ngoại trừ một đường thoát chéo, Dijkstra vẫn khám phá cạnh hợp lệ còn lại đó vì nó xem xét tất cả 8 hướng một cách thống nhất và không dựa vào sự tiến triển tham lam. 

Trường hợp cạnh cuối cùng là khi bắt đầu bằng kết thúc. Thuật toán khởi tạo chính xác khoảng cách về 0 và ngay lập tức quay trở lại mà không cần vào vòng lặp mở rộng, vì nút được trích xuất đầu tiên đã khớp với mục tiêu.
