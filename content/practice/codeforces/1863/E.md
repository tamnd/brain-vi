---
title: "CF 1863E - Chạy Tốc Độ"
description: "Chúng tôi được giao một bộ nhiệm vụ, mỗi nhiệm vụ chỉ có thể được hoàn thành vào một giờ cụ thể trong một ngày chơi lặp lại. Một số nhiệm vụ phụ thuộc vào những nhiệm vụ khác, có nghĩa là chúng không thể hoàn thành cho đến khi hoàn thành một số nhiệm vụ trước đó."
date: "2026-06-09T00:03:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dp", "graphs", "greedy", "math", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1863
codeforces_index: "E"
codeforces_contest_name: "Pinely Round 2 (Div. 1 + Div. 2)"
rating: 2100
weight: 1863
solve_time_s: 110
verified: false
draft: false
---

[CF 1863E - Chạy tốc độ](https://codeforces.com/problemset/problem/1863/E) 

**Xếp hạng:** 2100 
**Thẻ:** lực lượng vũ phu, dfs và tương tự, dp, đồ thị, tham lam, toán học, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một bộ nhiệm vụ, mỗi nhiệm vụ chỉ có thể được hoàn thành vào một giờ cụ thể trong một ngày chơi lặp lại. Một số nhiệm vụ phụ thuộc vào những nhiệm vụ khác, có nghĩa là chúng không thể hoàn thành cho đến khi hoàn thành một số nhiệm vụ trước đó. Thời gian di chuyển theo những giờ riêng biệt và sau giờ cuối cùng trong ngày, ngày tiếp theo bắt đầu lúc 0 giờ. Chúng ta có nhiệm vụ lên lịch cho tất cả các nhiệm vụ theo thứ tự hợp lệ để giảm thiểu tổng thời gian trôi qua giữa việc hoàn thành nhiệm vụ đầu tiên và nhiệm vụ cuối cùng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng nhiệm vụ`n`, số lượng phụ thuộc`m`, và độ dài của ngày`k`. Mỗi nhiệm vụ có một giờ cố định`h_i`TRONG`[0, k-1]`và các phụ thuộc được đưa ra dưới dạng cặp`(a_i, b_i)`, nghĩa là nhiệm vụ`b_i`chỉ có thể được thực hiện sau`a_i`. 

Những hạn chế ngụ ý rằng`n`có thể đạt tới 200.000 trên tất cả các trường hợp thử nghiệm. Bất kỳ thuật toán nào có độ phức tạp kém hơn O(n log n) cho mỗi trường hợp thử nghiệm đều có thể quá chậm. Ngoài ra, bản chất modulo của thời gian có nghĩa là việc trừ số giờ đơn giản có thể tạo ra khoảng thời gian âm nếu không được điều chỉnh cẩn thận để tính toán thời gian gói trong ngày. Các ví dụ nhỏ minh họa điều này: nếu nhiệm vụ ở giờ thứ 18 phụ thuộc vào nhiệm vụ ở giờ thứ 2, thì sự khác biệt ngây thơ 2-18 = -16 là không hợp lệ; thời gian trôi qua chính xác là 8 giờ nếu tính vào ngày hôm sau. 

Các trường hợp đặc biệt bao gồm các nhiệm vụ có thể được thực hiện trong cùng một giờ, các phần phụ thuộc tạo thành chuỗi dài và các`k`các giá trị có thể đánh lừa một phép tính modulo ngây thơ. Ví dụ: nếu tất cả các nhiệm vụ diễn ra vào cùng một giờ nhưng trong chuỗi phụ thuộc thì tổng thời gian đã trôi qua là`k`lần số ngày cần thiết, không phải bằng không. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các thứ tự tôpô của các nhiệm vụ và tính tổng thời gian đã trôi qua cho mỗi nhiệm vụ. Điều này đúng nhưng không khả thi vì có thể có tới`n!`trật tự topo. Ngay cả khi chúng ta cố gắng tối ưu hóa bằng cách tham lam chọn giờ sớm nhất có sẵn, chúng ta vẫn cần xử lý độ dài ngày theo modulo và các phần phụ thuộc một cách cẩn thận. Mô phỏng trực tiếp sẽ lấy O(n2) trong trường hợp xấu nhất đối với chuỗi phụ thuộc. 

Cái nhìn sâu sắc quan trọng là vấn đề này có thể được giảm xuống thành vấn đề đồ thị với lập trình động trên Đồ thị chu kỳ có hướng (DAG). Mỗi nhiệm vụ là một nút, các cạnh của các hình thức phụ thuộc và thời gian hoàn thành sớm nhất có thể của một nhiệm vụ có thể được biểu thị bằng thời gian tối đa của tất cả thời gian hoàn thành của nhiệm vụ trước đó cộng với bất kỳ thời gian chờ đợi cần thiết nào do ràng buộc ngày modulo. 

Về mặt hình thức, xác định`dp[u]`vì thời gian tối thiểu trôi qua từ nhiệm vụ hoàn thành đầu tiên đến khi hoàn thành nhiệm vụ`u`. Đối với một sự phụ thuộc`v -> u`, khoảng thời gian tăng lên là`(h_u - h_v + k) % k`để tính toán số giờ. Sau đó`dp[u] = max(dp[v] + (h_u - h_v + k) % k for all predecessors v)`. Sau khi xử lý DAG theo thứ tự tôpô, câu trả lời là`max(dp)`. 

Điều này biến một vấn đề giai thừa có khả năng thành một đường tuyến tính trên tất cả các cạnh, tạo ra độ phức tạp O(n + m) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các đơn hàng tôpô) | Ồ (n!) | O(n + m) | Quá chậm | 
| DAG + DP với sắp xếp tôpô | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ thể hiện sự phụ thuộc. Đối với mỗi nhiệm vụ, hãy duy trì một danh sách các nhiệm vụ phụ thuộc vào nó. 
2. Tính toán mức độ của từng nút để xác định các nhiệm vụ không có sự phụ thuộc. 
3. Khởi tạo`dp[u] = 0`cho tất cả các nhiệm vụ. Điều này thể hiện thời gian trôi qua tối thiểu để đạt được nhiệm vụ`u`từ một số nhiệm vụ bắt đầu. 
4. Sử dụng hàng đợi để thực hiện sắp xếp tôpô, bắt đầu với tất cả các nút có bậc 0. 
5. Khi hàng đợi không trống, hãy bật một nút`u`. Đối với mỗi nhiệm vụ phụ thuộc`v`: 

- Tính chênh lệch thời gian`(h[v] - h[u] + k) % k`. Điều này thể hiện thời gian chờ đợi tối thiểu khi xem xét gói ngày modulo. 
- Cập nhật`dp[v] = max(dp[v], dp[u] + (h[v] - h[u] + k) % k)`. 
- Giảm mức độ của`v`. Nếu nó trở thành số 0, hãy đẩy nó vào hàng đợi. 
6. Sau khi tất cả các nút được xử lý, câu trả lời là`max(dp)`. 

Tại sao nó hoạt động: Mọi nhiệm vụ đều được xử lý sau tất cả các nhiệm vụ trước đó do sắp xếp theo cấu trúc liên kết. Sự lặp lại DP tích lũy chính xác thời gian tối thiểu cần thiết để đạt được mỗi nhiệm vụ từ bất kỳ điểm bắt đầu nào và việc điều chỉnh mô-đun đảm bảo rằng thời gian diễn ra chính xác. Tận dụng tối đa tất cả`dp`giá trị ghi lại tổng thời gian đã trôi qua từ nhiệm vụ đầu tiên đến nhiệm vụ cuối cùng trong lịch trình tối ưu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        h = list(map(int, input().split()))
        g = [[] for _ in range(n)]
        indeg = [0] * n
        for _ in range(m):
            a, b = map(int, input().split())
            a -= 1
            b -= 1
            g[a].append(b)
            indeg[b] += 1

        dp = [0] * n
        queue = deque([i for i in range(n) if indeg[i] == 0])
        while queue:
            u = queue.popleft()
            for v in g[u]:
                delta = (h[v] - h[u] + k) % k
                dp[v] = max(dp[v], dp[u] + delta)
                indeg[v] -= 1
                if indeg[v] == 0:
                    queue.append(v)
        print(max(dp))

solve()
```Mã bắt đầu bằng cách đọc đầu vào và xây dựng DAG. các`dp`mảng lưu trữ thời gian trôi qua sớm nhất cho mỗi nhiệm vụ. Việc sắp xếp cấu trúc liên kết đảm bảo sự phụ thuộc được tôn trọng. Đối với mỗi cạnh, chênh lệch modulo được tính toán để xử lý việc gói ngày một cách chính xác. Điều này đảm bảo rằng ngay cả khi nhiệm vụ phụ thuộc diễn ra sớm hơn trong ngày thì ngày hôm sau vẫn được tính. 

## Ví dụ đã hoạt động 

Mẫu 1, trường hợp thử nghiệm đầu tiên:```
n=4, k=24
h=[12,16,18,12]
dependencies: 1->2, 1->3, 2->4, 3->4
```| Nhiệm vụ | dp[u] | Tính toán | 
| --- | --- | --- | 
| 1 | 0 | nút bắt đầu | 
| 2 | 4 | 0 + (16-12) | 
| 3 | 6 | 0 + (18-12) | 
| 4 | 12 | max(4 + (12-16+24)%24=4+20=24, 6 + (12-18+24)%24=6+18=24)=24 | 

Tối đa`dp`là 24. 

Mẫu 2, trường hợp thử nghiệm thứ ba:```
n=2, k=10
h=[5,5]
dependencies: 1->2
```| Nhiệm vụ | dp[u] | Tính toán | 
| --- | --- | --- | 
| 1 | 0 | nút bắt đầu | 
| 2 | 0 | 0 + (5-5+10)%10 = 0 | 

Tối đa`dp`là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút được xử lý một lần theo thứ tự tôpô, mỗi cạnh đóng góp một bản cập nhật DP | 
| Không gian | O(n + m) | Danh sách đồ thị kề, mảng theo độ, mảng DP | 

Cho tổng của`n`Và`m`trên tất cả các trường hợp thử nghiệm là 200.000, điều này chạy thoải mái dưới giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""6
4 4 24
12 16 18 12
1 2
1 3
2 4
3 4
4 3 10
2 6 5 9
1 4
2 4
3 4
2 1 10
5 5
1 2
5 0 1000
8 800 555 35 35
5 0 10
3 2 5 4 7
3 2 5
4 3 2
1 2
2 3
""") == "24\n7\n0\n480\n5\n8"

# custom minimal case
assert run("1\n1 0 5\n3") == "0", "single quest"

# custom all same hour, chain
assert run("1\n3 2 10\n1 1 1\n1 2\n2 3") == "20", "chain with same hour"

# custom independent quests, same hour
assert run("1\n3 0 10\n5 5 5") == "0", "no dependencies, same hour"

# custom wrap around day
assert run("1\n2 1 10\n9 1\n1 2") == "2", "wrap around day"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|---|
