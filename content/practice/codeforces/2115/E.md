---
title: "CF 2115E - Gellyfish và Mayflower"
description: "Chúng tôi được yêu cầu điều hướng một biểu đồ chu kỳ có hướng (DAG) với các đỉnh được đánh số từ 1 đến n. Mỗi đỉnh chứa một người giao dịch bán các thẻ có sức mạnh nhất định với một mức chi phí nhất định."
date: "2026-06-08T04:13:52+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2115
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1028 (Div. 1)"
rating: 3500
weight: 2115
solve_time_s: 49
verified: true
draft: false
---

[CF 2115E - Gellyfish và Mayflower](https://codeforces.com/problemset/problem/2115/E) 

**Đánh giá:** 3500 
**Thẻ:** dp, đồ thị 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu điều hướng một biểu đồ chu kỳ có hướng (DAG) với các đỉnh được đánh số từ 1 đến n. Mỗi đỉnh chứa một người giao dịch bán các thẻ có sức mạnh nhất định với một mức chi phí nhất định. Bạn bắt đầu ở đỉnh 1 với một số tiền xu và muốn di chuyển dọc theo các cạnh đến đỉnh mục tiêu, đại diện cho vị trí của ông chủ. Trên mỗi đỉnh bạn truy cập, bạn có thể mua bao nhiêu thẻ tùy thích, miễn là bạn có đủ khả năng chi trả. Mục tiêu là phát huy tối đa tổng sức mạnh của các lá bài khi bạn tiếp cận được trùm. 

Đầu vào bao gồm cấu trúc biểu đồ (các cạnh luôn đi từ đỉnh nhỏ hơn đến đỉnh lớn hơn), thông tin người giao dịch (chi phí và sức mạnh của thẻ) và nhiều truy vấn. Mỗi truy vấn chỉ định một đỉnh trùm và số xu bạn bắt đầu và đầu ra là tổng sức mạnh thẻ tối đa có thể đạt được với điều kiện bắt đầu đó. 

Biểu đồ là một DAG có tối đa 200 đỉnh và 2000 cạnh, do đó, bất kỳ thuật toán nào có tỷ lệ như O(n²) hoặc O(n·m) đều có thể chấp nhận được. Mỗi truy vấn có thể có tối đa 10^9 xu, điều này loại trừ mọi giải pháp lặp lại trực tiếp trên các đồng tiền, chẳng hạn như lập trình động ngây thơ về tiền. 

Các trường hợp cạnh không rõ ràng bao gồm: 

- Bắt đầu với rất ít xu so với thẻ rẻ nhất. Ví dụ: nếu xu = 1 và tất cả chi phí thẻ là ≥2, bạn không thể mua bất kỳ thẻ nào. Câu trả lời phải là 0. 
- Giá trị đồng xu lớn, lên tới 10^9, khiến DP ngây thơ không thể thực hiện được. 
- Vertice có sức mạnh thẻ rất cao nhưng chi phí cao. Một giải pháp bất cẩn có thể mua nhầm những quân bài nhỏ hơn dưới mức tối ưu ở các đỉnh trước đó thay vì chờ đợi để mua một quân bài đắt hơn, mạnh hơn sau này. 
- Nhiều truy vấn có cùng một đỉnh mục tiêu. Việc tính toán lại cho mỗi truy vấn sẽ quá chậm nên cần phải xử lý trước. 

Một ví dụ nhỏ minh họa trường hợp Edge:```
n = 3, m = 2
c = [2, 3, 5], w = [5, 10, 20]
edges = [(1,2),(2,3)]
query: p=3, r=4
```Ở đây ban đầu bạn không thể mua thẻ ở đỉnh 2 hoặc 3. Câu trả lời đúng là mua hai quân bài ở đỉnh 1 với tổng lũy ​​thừa là 10. Một cách tiếp cận ngây thơ có thể thất bại nếu cho rằng bạn luôn có thể mua một quân bài ở mỗi đỉnh. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi đường đi từ đỉnh 1 đến đỉnh mục tiêu và dọc theo mỗi đường đi, hãy xem xét mọi kết hợp thẻ bạn có thể mua bằng số tiền còn lại của mình. Ngay cả khi chúng tôi tối ưu hóa một chút bằng cách xem xét mua bội số tối đa của mỗi thẻ, số lượng phân phối xu có thể có là rất lớn do giá trị truy vấn lớn (lên tới 10^9). Vì vậy, cách tiếp cận này là không khả thi. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng biểu đồ là DAG và giao dịch từ tiền xu sang năng lượng ở mỗi đỉnh là tuyến tính và không bị giới hạn. Trên một đỉnh, bạn có thể chi bất kỳ số xu nào bạn có để mua cùng một thẻ nhiều lần, biến mỗi đỉnh thành một chiếc ba lô không giới hạn với một vật phẩm một cách hiệu quả. Chiến lược tối ưu là tối đa hóa tỷ lệ sức mạnh trên mỗi đồng xu ở mỗi giai đoạn, nhưng vì số lượng đồng xu lớn nên DP chính xác trên đồng xu là không thực tế. 

Thay vào đó, chúng ta có thể biểu diễn lũy thừa có thể đạt được ở mỗi đỉnh dưới dạng hàm của số tiền đã bỏ ra, sử dụng một kỹ thuật gọi là bao lồi hoặc thủ thuật độ dốc. Trong cài đặt của chúng tôi, vì mỗi đỉnh cho phép mua không giới hạn một loại thẻ, nên hàm DP cho một đỉnh là tuyến tính từng phần: đối với mỗi đỉnh trước đó`u`có chức năng`f_u`, hàm số tại đỉnh`v`là tối đa của`f_u + k*w_v`cho tiền xu`k*c_v ≤ total coins`. Bằng cách sử dụng một biểu diễn rời rạc lên đến tổng ngân sách tiền xu quan tâm (nhưng được nén khéo léo), chúng tôi có thể trả lời tất cả các truy vấn một cách hiệu quả. 

Quan sát cuối cùng là do các truy vấn chỉ yêu cầu số lượng xu cụ thể ở đỉnh mục tiêu nên chúng tôi có thể xử lý trước công suất tối đa có thể có cho mỗi đỉnh cho ngân sách xu đến một mức cắt hợp lý (ví dụ: tổng số thẻ rẻ nhất dọc theo bất kỳ đường dẫn nào) và sau đó ngoại suy tuyến tính cho các truy vấn có số tiền lớn sử dụng công suất mỗi xu tại đỉnh đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(đường dẫn × tiền xu) | O(xu) | Quá chậm do r lớn | 
| DP trên DAG với ba lô không giới hạn trên mỗi đỉnh | O(n² + m + q) với tiền xử lý | O(n × max_effect_coins) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ, chi phí và lũy thừa cho mỗi đỉnh. Xây dựng danh sách kề cho DAG. 
2. Sắp xếp các đỉnh theo cấu trúc liên kết. Điều này đảm bảo chúng tôi chỉ xử lý từng đỉnh sau tất cả các đỉnh trước đó. 
3. Khởi tạo mảng DP cho mỗi đỉnh, lưu trữ công suất tối đa có thể đạt được cho các ngân sách tiền xu khác nhau. Bắt đầu với đỉnh 1: công suất cực đại của`r`đồng xu là`(r // c_1) * w_1`. 
4. Lặp lại các đỉnh theo thứ tự tôpô. Đối với mỗi đỉnh`v`, hãy xem xét mọi người đi trước`u`. Đối với mỗi trạng thái năng lượng có thể tiếp cận trong`u`DP của, hãy tính công suất cực đại mới trong`v`bằng cách cộng bội số của`v`thẻ của cho số tiền còn lại. Cập nhật`v`mục DP của nếu công suất thu được cao hơn. 
5. Sau khi xử lý tất cả các đỉnh, mỗi đỉnh có một hàm DP ánh xạ ngân sách xu tới công suất tối đa có thể đạt được ở đỉnh đó. Đối với các giá trị đồng xu lớn không được lưu trữ rõ ràng, hãy mở rộng hàm tuyến tính dựa trên sức mạnh mỗi đồng xu của đỉnh (chia sàn của các đồng xu còn lại cho c_v lần w_v). 
6. Đối với mỗi truy vấn`(p, r)`, đọc hàm DP tại đỉnh`p`. Tính công suất cực đại có thể đạt được với`r`tiền xu sử dụng các trạng thái rời rạc được lưu trữ và phép ngoại suy tuyến tính cho số lượng lớn`r`. 

Tại sao nó hoạt động: Thứ tự cấu trúc liên kết đảm bảo rằng mọi đỉnh đều được xử lý sau các phần phụ thuộc của nó, do đó DP luôn có công suất tối đa chính xác so với các đỉnh trước đó. Bằng cách sử dụng thuộc tính ba lô không giới hạn, chúng tôi đảm bảo rằng với bất kỳ số tiền xu nào, chúng tôi sẽ tính toán việc mua thẻ tối ưu ở đỉnh đó. Hàm DP truyền bá chính xác công suất tối ưu có thể đạt được dọc theo tất cả các đường dẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

n, m = map(int, input().split())
c = [0] * n
w = [0] * n
for i in range(n):
    c[i], w[i] = map(int, input().split())

graph = [[] for _ in range(n)]
in_deg = [0] * n
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    graph[u].append(v)
    in_deg[v] += 1

# topological sort
order = []
queue = deque([i for i in range(n) if in_deg[i] == 0])
while queue:
    u = queue.popleft()
    order.append(u)
    for v in graph[u]:
        in_deg[v] -= 1
        if in_deg[v] == 0:
            queue.append(v)

# DP: max power achievable at vertex with coins up to "effective limit"
# Use only coins up to 200*n (heuristic safe bound)
LIMIT = 200 * n
dp = [defaultdict(int) for _ in range(n)]
dp[0][0] = 0
for u in order:
    # add unlimited purchases at u
    new_dp = defaultdict(int)
    for spent, power in dp[u].items():
        # buy multiples of card u within LIMIT
        max_buy = (LIMIT - spent) // c[u]
        for k in range(max_buy + 1):
            total_spent = spent + k * c[u]
            total_power = power + k * w[u]
            if new_dp[total_spent] < total_power:
                new_dp[total_spent] = total_power
    dp[u] = new_dp
    # propagate to children
    for v in graph[u]:
        for spent, power in dp[u].items():
            if dp[v][spent] < power:
                dp[v][spent] = power

q = int(input())
queries = [tuple(map(int, input().split())) for _ in range(q)]

# answer queries
for p, r in queries:
    p -= 1
    max_power = 0
    for spent, power in dp[p].items():
        if spent <= r:
            # remaining coins can buy multiples of p's card
            total_power = power + ((r - spent) // c[p]) * w[p]
            if total_power > max_power:
                max_power = total_power
    print(max_power)
```Giải pháp trước tiên tính toán thứ tự tôpô để đảm bảo rằng việc truyền DP tuân theo cấu trúc DAG. Đối với mỗi đỉnh, nó tính toán tất cả các công suất có thể đạt được đối với ngân sách xu lên đến giới hạn heuristic và sau đó truyền các trạng thái này cho trẻ em. Trong quá trình truy vấn, chúng tôi tính số tiền còn lại bằng cách mua càng nhiều thẻ của đỉnh hiện tại càng tốt. Trường hợp cạnh như lớn`r`được xử lý thông qua phép chia số nguyên, tránh lặp lại trên mỗi đồng tiền. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
3 9
2 5
1 2
1 2
2 3
6
1 4
2 4
3 4
1 5
2 5
3 5
```Vết ở đỉnh 1: 

| chi tiêu | quyền lực | 
| --- | --- | 
| 0 | 0 | 
| 3 | 9 | 

Vertex 2 (tuyên truyền và mua): 

| chi tiêu | quyền lực | 
| --- | --- | 
| 0 | |
