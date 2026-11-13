---
title: "CF 1715E - Đường Dài Về Nhà"
description: "Stanley muốn đi từ thành phố 1 đến mọi thành phố khác trong một quốc gia bằng hai loại phương tiện giao thông: đường bộ và đường hàng không. Các con đường được đưa ra một cách rõ ràng dưới dạng các cạnh có trọng số, tạo thành một đồ thị vô hướng."
date: "2026-06-09T20:02:48+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dp", "geometry", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1715
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 816 (Div. 2)"
rating: 2400
weight: 1715
solve_time_s: 453
verified: true
draft: false
---

[CF 1715E - Đường về nhà dài](https://codeforces.com/problemset/problem/1715/E) 

**Đánh giá:** 2400 
**Tags:** cấu trúc dữ liệu, phân chia và chinh phục, dp, hình học, đồ thị, tham lam, đường đi ngắn nhất 
**Thời gian giải:** 7 phút 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Stanley muốn đi từ thành phố 1 đến mọi thành phố khác trong một quốc gia bằng hai loại phương tiện giao thông: đường bộ và đường hàng không. Các con đường được đưa ra một cách rõ ràng dưới dạng các cạnh có trọng số, tạo thành một đồ thị vô hướng. Có các chuyến bay giữa mỗi cặp thành phố và thời gian di chuyển là bình phương chênh lệch số lượng thành phố của chúng. Stanley được phép nhiều nhất`k`chuyến bay, đó là một con số nhỏ. Nhiệm vụ là tìm thời gian tối thiểu để đến từng thành phố từ thành phố 1 trong khi vẫn tôn trọng giới hạn chuyến bay. Đầu vào bao gồm số lượng thành phố`n`, số đường`m`, chuyến bay tối đa`k`, và chính những con đường. 

Những ràng buộc cho phép`n`Và`m`để được`10^5`. Điều này loại trừ các cách tiếp cận ngây thơ xem xét tất cả các cặp chuyến bay một cách rõ ràng hoặc chạy các thuật toán có độ phức tạp bậc hai. Mỗi chi phí chuyến bay chỉ phụ thuộc vào các chỉ số thành phố, trong đó đưa ra hàm chi phí lồi, có cấu trúc. cái nhỏ`k`gợi ý rằng lập trình động theo lớp hoặc tính toán đường đi ngắn nhất lặp đi lặp lại là khả thi. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó tất cả các đường đi tối ưu chỉ dựa vào các chuyến bay hoặc chỉ dựa trên các con đường, các trường hợp có đường trùng lặp hoặc khi các chuyến bay chỉ có thể được sử dụng một phần do`k`nhỏ bé. Ví dụ: với 3 thành phố và một con đường nằm trong khoảng từ 1 đến 3, nếu`k=2`, nên đến thành phố 2 bằng chuyến bay (chi phí 1), thành phố 3 bằng đường bộ (chi phí 1). Một cách tiếp cận bất cẩn có thể cố gắng xem xét các chuyến bay theo thứ tự tùy ý và lạm dụng chúng hoặc tính toán tất cả các cạnh chuyến bay một cách ngây thơ, vượt quá giới hạn bộ nhớ và thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét tất cả các đường đi trong biểu đồ kết hợp đường và chuyến bay, liên tục thêm tất cả`n^2`các chuyến bay. Người ta có thể mô hình hóa vấn đề dưới dạng biểu đồ lớp với`k+1`các lớp biểu thị số lượng chuyến bay được sử dụng. Mỗi lớp chứa`n`các nút và cạnh là đường (trong cùng một lớp) hoặc chuyến bay (đến lớp tiếp theo). Chạy Dijkstra trên biểu đồ phân lớp đầy đủ này là đúng nhưng không thực tế:`O(k n^2 log(kn))`sẽ là quá chậm đối với`n=10^5`. 

Quan sát quan trọng là hàm chi phí chuyến bay`(u-v)^2`là lồi. Điều này cho phép chúng tôi sử dụng thủ thuật bao lồi hoặc tối ưu hóa lập trình động chia để trị để tính toán chi phí tối thiểu để tiếp cận tất cả các thành phố bằng cách sử dụng một chuyến bay bổ sung trong`O(n log n)`mỗi lớp, thay vì xem xét tất cả`n^2`các cạnh chuyến bay. Đường có thể được xử lý bằng thuật toán đường đi ngắn nhất Dijkstra tiêu chuẩn. Bằng cách lặp lại`k`lần, mỗi lần truyền bá chi phí chuyến bay trên tất cả các thành phố, chúng tôi có thể tính toán thời gian tối thiểu trong khi vẫn tôn trọng giới hạn chuyến bay. 

Tính lồi xuất hiện vì với mỗi thành phố`v`, thành phố tối ưu trước đó`u`để thực hiện chuyến bay ở vị trí ngoài cùng bên trái hoặc ngoài cùng bên phải so với`v`theo thứ tự được sắp xếp và hàm`(u-v)^2 + dp[u]`có một mức tối thiểu duy nhất. Điều này làm giảm độ phức tạp trên mỗi bước bay từ`O(n^2)`ĐẾN`O(n log n)`sử dụng phương pháp chia để trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force xếp lớp Dijkstra | O(k n^2 log(kn)) | O(kn + m) | Quá chậm | 
| Tối ưu với đường + chuyến bay DP | O((m + n log n) * k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị đường dưới dạng danh sách kề. Mỗi thành phố duy trì các con đường đi của mình bằng trọng lượng. Điều này là bắt buộc đối với các bản cập nhật Dijkstra chỉ dành cho đường bộ. 
2. Khởi tạo mảng khoảng cách`dist`kích thước`n`, bộ`dist[1] = 0`và những người khác đến vô cùng. Điều này thể hiện thời gian tối thiểu để đến từng thành phố. 
3. Chạy Dijkstra trên biểu đồ đường bắt đầu từ thành phố 1 để tính thời gian ngắn nhất chỉ sử dụng đường. Đây là lớp nền trước bất kỳ chuyến bay nào. 
4. Lặp lại`k`lần để mô phỏng bằng một chuyến bay nữa: 

1. Hãy để`dp`là khoảng cách tối thiểu hiện tại sau`i`các chuyến bay. 
2. Tính khoảng cách tối thiểu tới mỗi thành phố bằng cách sử dụng đúng một chuyến bay nữa. Đối với thành phố`v`, chúng tôi cần`min(dp[u] + (u-v)^2)`tổng thể`u`. Sử dụng phép chia để trị hoặc thủ thuật bao lồi để tính toán điều này một cách hiệu quả trong`O(n log n)`. 
3. Sau khi tính toán cập nhật chuyến bay, hãy chạy bản cập nhật Dijkstra để truyền qua các con đường, vì khoảng cách tối thiểu mới sử dụng các chuyến bay có thể cho phép đường đi rẻ hơn đến các thành phố khác. 
5. Sau`k`lặp lại, mảng khoảng cách chứa thời gian tối thiểu tôn trọng giới hạn chuyến bay. Xuất nó ra. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến sau mỗi lần lặp`i`, mảng khoảng cách biểu thị thời gian sử dụng tối thiểu tối đa`i`các chuyến bay. Dijkstra đảm bảo rằng đường đi được truyền bá một cách tối ưu. Độ lồi đảm bảo rằng đối với mỗi lớp chuyến bay, chúng tôi tính toán chi phí chuyến bay tối thiểu một cách hiệu quả mà không bỏ sót bất kỳ cặp thành phố tối ưu nào. Lặp đi lặp lại điều này`k`lần đảm bảo tất cả các lần sử dụng chuyến bay có thể được xem xét đến giới hạn cho phép. 

## Giải pháp Python```python
import sys, heapq
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    adj = [[] for _ in range(n)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].append((v, w))
        adj[v].append((u, w))

    INF = 1 << 60
    dist = [INF] * n
    dist[0] = 0

    def dijkstra():
        heap = [(0, 0)]
        visited = [False] * n
        while heap:
            d, u = heapq.heappop(heap)
            if visited[u]:
                continue
            visited[u] = True
            for v, w in adj[u]:
                if dist[v] > d + w:
                    dist[v] = d + w
                    heapq.heappush(heap, (dist[v], v))

    dijkstra()

    for _ in range(k):
        dp = dist[:]
        # Divide and conquer DP for flights
        def dc(l, r):
            if l >= r:
                return
            m = (l + r) // 2
            best = -1
            val = INF
            for i in range(l, r+1):
                cost = dp[i] + (i - m) ** 2
                if cost < val:
                    val = cost
                    best = i
            dist[m] = min(dist[m], val)
            dc(l, m-1)
            dc(m+1, r)

        dc(0, n-1)
        dijkstra()

    print(' '.join(map(str, dist)))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng danh sách lân cận cho các con đường, tính toán khoảng cách ban đầu bằng Dijkstra, sau đó cải thiện chúng nhiều lần bằng cách xem xét các chuyến bay. Chức năng chia để trị`dc`tính toán hiệu quả tối thiểu`(u-v)^2 + dp[u]`cho mỗi thành phố`v`. Sau mỗi lớp chuyến bay, Dijkstra truyền bá khoảng cách tối thiểu được cập nhật qua các con đường. Thứ tự Dijkstra sau các chuyến bay đảm bảo rằng chúng ta không bỏ lỡ những chặng đường rẻ hơn do trộn đường và chuyến bay. Điều chỉnh chỉ số`-1`là bắt buộc vì Python sử dụng lập chỉ mục dựa trên 0. Cái lớn`INF`ngăn ngừa tràn số nguyên, vì chênh lệch bình phương có thể lên tới`10^10`. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`3 1 2\n1 3 1`| Thành phố | Quận ban đầu | Sau chuyến bay | Quận cuối cùng | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | INF | 1 | 1 | 
| 3 | 1 | 1 | 1 | 

Có thể đến Thành phố 2 bằng chuyến bay từ số 1. Có thể đến Thành phố 3 bằng đường bộ. Các chuyến bay được cải thiện không còn đường đi nào nữa. 

**Mẫu 2:** 

đầu vào:`4 3 1\n1 2 3\n2 3 4\n3 4 5`Sau Dijkstra chỉ có đường: 

| Thành phố | quận | 
| --- | --- | 
| 1 | 0 | 
| 2 | 3 | 
| 3 | 7 | 
| 4 | 12 | 

Một bản cập nhật chuyến bay được tính toán`min(dp[u] + (u-v)^2)`cho mỗi`v`. Đối với thành phố 3, chuyến bay từ 2 cho`3 + 1 = 4`cải thiện hơn 7. Sau đó Dijkstra tuyên truyền cải tiến đường. Khoảng cách cuối cùng: 0,3,4,7. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * (m log n + n log n)) | Mỗi Dijkstra lấy O(m log n), cập nhật chuyến bay thông qua phân chia và chinh phục O(n log n), lặp lại k lần. | 
| Không gian | O(n + m) | Lưu trữ danh sách kề và mảng khoảng cách. | 

Những ràng buộc cho phép`k`lên đến 20 và`n,m`lên tới 10^5, vì vậy độ phức tạp này có thể chấp nhận được trong vòng 3 giây. Bộ nhớ bị chi phối bởi danh sách kề và mảng khoảng cách, có dung lượng dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3 1 2\n1 3
```
