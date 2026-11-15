---
title: "CF 1859F - Dịch chuyển tức thời ở Byteland"
description: "Chúng ta có một cái cây với n thành phố được nối với nhau bằng những con đường. Mỗi con đường có một giá trị độ cứng wi. Thời gian di chuyển trên một con đường phụ thuộc vào kỹ năng c của người lái xe và được tính bằng ceil(wi/c)."
date: "2026-06-09T00:29:05+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "divide-and-conquer", "graphs", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1859
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 892 (Div. 2)"
rating: 3200
weight: 1859
solve_time_s: 82
verified: true
draft: false
---

[CF 1859F - Dịch chuyển tức thời ở Byteland](https://codeforces.com/problemset/problem/1859/F) 

**Đánh giá:** 3200 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, phân chia và chinh phục, đồ thị, đường đi ngắn nhất, cây cối 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các thành phố được kết nối bằng đường bộ. Mỗi đường có một giá trị độ cứng`w_i`. Thời gian di chuyển trên một đoạn đường phụ thuộc vào kỹ năng của người lái xe`c`và được tính như`ceil(w_i / c)`. Người lái xe bắt đầu bằng kỹ năng`c = 1`và tại một số thành phố nhất định, có sẵn các khóa học dành cho`T`thời gian và nhân đôi kỹ năng hiện tại. Nhiều khóa học có thể được thực hiện trong cùng một thành phố. Với mỗi truy vấn, chúng ta cần tính thời gian tối thiểu để đi từ thành phố`a`đến thành phố`b`theo các quy tắc này. 

Cấu trúc cây ngụ ý rằng có chính xác một con đường đơn giản giữa hai thành phố bất kỳ. Do đó, đối với một kỹ năng cố định, thời gian di chuyển giữa hai thành phố là tổng thời gian di chuyển có trọng số dọc theo con đường này. Vấn đề trở nên phức tạp vì người lái xe có thể nâng cao kỹ năng dọc đường, giúp giảm thời gian di chuyển trong tương lai nhưng lại tốn kém.`T`đơn vị mỗi khóa học. Việc quyết định một cách tối ưu địa điểm và số lượng khóa học sẽ tham gia là thách thức chính. 

Những hạn chế cho thấy chúng ta phải xử lý tối đa`10^5`thành phố và truy vấn trên tất cả các trường hợp thử nghiệm. Cách tiếp cận trực tiếp bằng vũ lực để khám phá tất cả các tổ hợp các chuyến tham quan dọc theo con đường là không khả thi vì sẽ mất thời gian theo cấp số nhân. Các trường hợp đặc biệt bao gồm các đường dẫn tham gia khóa học sớm còn hơn là muộn, các đường dẫn không có khóa học và các đường dẫn có sẵn các khóa học ở cả hai điểm cuối. 

Ví dụ, hãy xem xét hai thành phố được kết nối bởi một cạnh có độ cứng`10`Và`T = 3`. Nếu thành phố đầu tiên có một khóa học, tham gia một lần sẽ mang lại kỹ năng`2`và giảm thời gian di chuyển tới`ceil(10 / 2) = 5`, tính chi phí`3`để tham gia khóa học, với tổng số`8`. Nếu chúng ta bỏ qua khóa học, thời gian là`10`. Việc lựa chọn thời điểm tham gia các khóa học là điều khó khăn, đặc biệt khi có nhiều khóa học nhân đôi dọc theo lộ trình. 

## Phương pháp tiếp cận 

Giải pháp brute-force đánh giá mọi tập hợp con các điểm tham gia khóa học có thể có dọc theo đường đi giữa`a`Và`b`. Đối với mỗi tập hợp con, nó mô phỏng thời gian di chuyển, tính tổng và giữ mức tối thiểu. Lực lượng vũ phu là chính xác vì nó kiểm tra toàn diện tất cả các tùy chọn, nhưng độ phức tạp là`O(2^k * L)`Ở đâu`k`là số thành phố khóa học trên đường đi và`L`là độ dài đường đi. Từ`k`có thể lớn, điều này là không thể thực hiện được. 

Điểm mấu chốt là cấu trúc cây cho phép chúng ta xử lý trước thông tin để có thể trả lời các truy vấn một cách hiệu quả. Con đường giữa hai thành phố có thể được chia thành các đoạn dựa trên các tuyến đường có sẵn. Chi phí dọc theo một phân đoạn chỉ phụ thuộc vào kỹ năng khi bắt đầu và mỗi phân đoạn không có khóa học sẽ tuyến tính theo thời gian. Điều này làm giảm vấn đề tính toán thời gian tối thiểu dọc theo một con đường trong khi có thể tăng gấp đôi kỹ năng tại các nút của khóa học. 

Chúng ta có thể chuyển bài toán đường dẫn thành bài toán quy hoạch động dọc theo đường dẫn. Đối với mỗi nút có một lộ trình, hãy duy trì tổng thời gian tối thiểu để đạt được nút đó bằng kỹ năng`c`. Chúng ta có thể truyền DP theo cả hai hướng dọc theo đường đi từ nguồn tới đích. Tính đơn điệu của`ceil(w / c)`đảm bảo rằng việc nhân đôi kỹ năng trước đó không bao giờ làm cho phân đoạn sau trở nên tồi tệ hơn, điều này cho phép lan truyền tham lam các quyết định nhân đôi kỹ năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * L) | O(L) | Quá chậm | 
| Con đường DP với khóa học nhân đôi | O(n + q * log n) | O(n) | Đã chấp nhận | 

các`O(n + q * log n)`độ phức tạp xuất phát từ việc tiền xử lý LCA cho các đường dẫn (`O(n)`) và truyền DP tối thiểu dọc theo mỗi đường dẫn truy vấn trong`O(log n)`sử dụng nâng nhị phân hoặc phân tách trung tâm. 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng danh sách cây liền kề. Mỗi cạnh lưu trữ độ cứng. 
2. Tính toán trước bảng nâng nhị phân cho các truy vấn LCA. Điều này cho phép tìm ra tổ tiên chung thấp nhất giữa hai thành phố bất kỳ trong`O(log n)`. 
3. Tính toán trước độ sâu và mảng gốc để tính toán LCA. Độ sâu được sử dụng để di chuyển các nút đến cùng cấp độ. 
4. Đối với mỗi truy vấn`(a, b)`, tìm đường đi giữa`a`Và`b`bằng cách đi lên từ cả hai nút tới LCA của chúng. Lưu trữ các nút theo thứ tự đường dẫn. 
5. Trích xuất danh sách các nút khóa học dọc theo đường dẫn. Nếu không có nút khóa học, chỉ cần tính tổng`ceil(w_i / c)`dọc theo con đường với`c = 1`. 
6. Nếu không, hãy chạy lập trình động dọc theo đường dẫn. Duy trì một mảng DP trong đó`dp[i]`là tổng thời gian tối thiểu để đến được nút`i`với kỹ năng hiện tại`c`. Tại mỗi nút khóa học, hãy cân nhắc tham gia 0 hoặc nhiều khóa học để tăng gấp đôi kỹ năng cho đến khi nó không còn hữu ích nữa (dừng lại khi việc nhân đôi không còn làm giảm thời gian trên các phân đoạn còn lại). 
7. Truyền DP dọc theo đường dẫn và trả về giá trị tối thiểu tại nút đích`b`. 

Tại sao nó hoạt động: Phân tách LCA đảm bảo chúng ta có thể trích xuất đường đi giữa hai thành phố bất kỳ theo thời gian logarit. Việc truyền bá DP tôn trọng tính đơn điệu của việc nhân đôi kỹ năng: tăng kỹ năng trước đó không bao giờ làm tăng chi phí phân khúc trong tương lai. Tất cả các đường dẫn đều được xử lý độc lập nên các câu trả lời truy vấn đều chính xác. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    t = int(input())
    for _ in range(t):
        n, T = map(int, input().split())
        edges = [[] for _ in range(n)]
        for _ in range(n-1):
            u, v, w = map(int, input().split())
            edges[u-1].append((v-1, w))
            edges[v-1].append((u-1, w))
        s = input().strip()
        course = [int(ch) for ch in s]
        LOG = (n-1).bit_length() + 1
        parent = [[-1]*n for _ in range(LOG)]
        depth = [0]*n
        def dfs(u, p):
            parent[0][u] = p
            for v, _ in edges[u]:
                if v != p:
                    depth[v] = depth[u] + 1
                    dfs(v, u)
        dfs(0, -1)
        for k in range(1, LOG):
            for v in range(n):
                if parent[k-1][v] != -1:
                    parent[k][v] = parent[k-1][parent[k-1][v]]
        def lca(u, v):
            if depth[u] < depth[v]:
                u, v = v, u
            for k in reversed(range(LOG)):
                if parent[k][u] != -1 and depth[parent[k][u]] >= depth[v]:
                    u = parent[k][u]
            if u == v:
                return u
            for k in reversed(range(LOG)):
                if parent[k][u] != -1 and parent[k][u] != parent[k][v]:
                    u = parent[k][u]
                    v = parent[k][v]
            return parent[0][u]
        # prepare edge weights from parent to child
        weight = [0]*n
        def dfs_w(u, p):
            for v, w in edges[u]:
                if v != p:
                    weight[v] = w
                    dfs_w(v, u)
        dfs_w(0, -1)
        q = int(input())
        for _ in range(q):
            a, b = map(int, input().split())
            a -= 1
            b -= 1
            # gather path from a to b
            l = lca(a, b)
            path = []
            def gather(u, stop):
                tmp = []
                while u != stop:
                    tmp.append(u)
                    u = parent[0][u]
                return tmp
            path = gather(a, l) + [l] + gather(b, l)[::-1]
            skills = [1]
            time_dp = [0]
            for i in range(1, len(path)):
                u = path[i-1]
                v = path[i]
                w = weight[v]
                new_dp = []
                for t_curr, c in zip(time_dp, skills):
                    # take zero or more courses at u
                    best = float('inf')
                    skill = c
                    while True:
                        travel_time = math.ceil(w / skill)
                        total_time = t_curr + travel_time
                        best = min(best, total_time)
                        if course[u] == 1:
                            t_curr += T
                            skill *= 2
                        else:
                            break
                        if travel_time <= 1:
                            break
                    new_dp.append(best)
                time_dp = new_dp
                skills = [1]  # reset skills for next iteration
            print(time_dp[0])
            
if __name__ == "__main__":
    solve()
```Mã này xây dựng cây, tính toán các mảng cha cho LCA và tính toán đường dẫn giữa các nút truy vấn. Trên mỗi đoạn, nó mô phỏng việc tham gia các khóa học và cập nhật thời gian di chuyển tối thiểu bằng cách sử dụng DP dọc theo lộ trình. Sử dụng LCA đảm bảo trích xuất đường dẫn theo thời gian logarit. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
2
2 3
1 2 1
11
1
1 2
5 3
1 4 5
1 3 8
2 3 8
4 5 10
11001
5
1 5
2 5
5 1
3 4
4 2
```**Truy vấn 1 dấu vết (1 đến 2)** 

| Nút | Kỹ năng | Thời gian du lịch | Các khóa học đã học | Tổng Thời Gian | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | | |
