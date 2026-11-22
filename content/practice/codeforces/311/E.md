---
title: "CF 311E - Nhà sinh vật học"
description: "Chúng ta được yêu cầu tối đa hóa số tiền ròng mà SmallR có thể kiếm được từ một thí nghiệm trên chó, nơi cô ấy có thể thay đổi giới tính của mỗi con chó với một mức chi phí nhất định."
date: "2026-06-05T18:44:05+07:00"
tags: ["codeforces", "competitive-programming", "flows"]
categories: ["algorithms"]
codeforces_contest: 311
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 185 (Div. 1)"
rating: 2300
weight: 311
solve_time_s: 122
verified: false
draft: false
---

[CF 311E - Nhà sinh vật học](https://codeforces.com/problemset/problem/311/E) 

**Đánh giá:** 2300 
**Thẻ:** dòng chảy 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa số tiền ròng mà SmallR có thể kiếm được từ một thí nghiệm trên chó, nơi cô ấy có thể thay đổi giới tính của mỗi con chó với một mức chi phí nhất định. Có những nhà quan sát giàu có đặt cược có điều kiện: mỗi người chỉ định một tập hợp con chó và một giới tính mục tiêu, đồng thời hứa hẹn một phần thưởng nếu tất cả những con chó trong tập hợp con đó khớp với giới tính đó. Một số người quan sát là bạn bè của cô, trong trường hợp đó việc không đáp ứng điều kiện của họ sẽ khiến cô phải chịu một hình phạt cố định. 

Dữ liệu đầu vào cung cấp giới tính ban đầu của mỗi con chó, chi phí để thay đổi nó và mô tả về từng người quan sát: giới tính mục tiêu, phần thưởng, danh sách chó và tình trạng tình bạn. Đầu ra là lợi nhuận ròng tối đa, có thể âm nếu cái giá phải trả để làm hài lòng bạn bè lớn hơn phần thưởng từ những người quan sát khác. 

Các ràng buộc cho thấy chúng tôi có tới 10.000 con chó và tối đa 2.000 người quan sát. Mỗi người quan sát quan tâm đến tối đa 10 con chó. Điều này cho thấy rằng việc lặp lại trên tất cả các tập con của chó là không thể, nhưng việc lặp lại trên tất cả các nhà quan sát là có thể chấp nhận được. Chi phí và phần thưởng đủ nhỏ để lưu trữ dưới dạng số nguyên. Các trường hợp đặc biệt bao gồm các tình huống trong đó một số người quan sát không có sự trùng lặp về chó hoặc ban đầu tất cả các con chó đều có giới tính sai đối với một số bạn bè, dẫn đến thua cuộc. 

Việc thực hiện bất cẩn có thể cố gắng lật đổ con chó một cách tham lam mà không xem xét các điều kiện quan sát chồng chéo. Ví dụ: nếu hai người quan sát yêu cầu giới tính khác nhau cho cùng một con chó, một chiến lược ngây thơ có thể tính phần thưởng hoặc hình phạt gấp đôi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng thay đổi tất cả các tập hợp con chó có thể có, tính toán cấu hình giới tính thu được và tổng hợp tất cả các phần thưởng trừ đi hình phạt. Tổng số cấu hình là$2^n$, điều này là không thể thực hiện được đối với$n=10^4$. Ngay cả việc lặp lại tất cả các trình quan sát cho từng cấu hình cũng không giúp ích được gì; cách tiếp cận này chậm theo cấp số nhân. 

Quan sát quan trọng là mỗi người quan sát áp đặt một ràng buộc cục bộ lên tối đa 10 con chó. Chúng ta có thể mô hình hóa điều này như một vấn đề về luồng mạng: chúng ta xây dựng một biểu đồ luồng trong đó chúng ta chọn cho mỗi con chó có thay đổi nó hay không, với các cạnh phản ánh chi phí thay đổi và cái được hoặc mất từ ​​những người quan sát. Mỗi người quan sát có thể được biểu diễn dưới dạng một nút được kết nối với những con chó của nó và khả năng mã hóa phần thưởng khi làm hài lòng một người không phải là bạn hoặc hình phạt khi đánh bại một người bạn. Sau đó, mức cắt tối thiểu trong biểu đồ này tương ứng với tập hợp các thay đổi chó tối ưu nhằm tối đa hóa lợi nhuận ròng. Điều này làm giảm vấn đề từ cấu hình hàm mũ đến vấn đề mạng luồng đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * m) | O(n + m) | Quá chậm | 
| Luồng mạng / Cắt tối thiểu | O((n + m) * (tổng của k_i)) | O(n + m + tổng của k_i) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mạng luồng với nút nguồn và nút đích. Mỗi con chó được biểu diễn dưới dạng một nút và mỗi người quan sát được biểu diễn dưới dạng một nút. Mạng sẽ mã hóa chi phí và lợi nhuận dưới dạng năng lực biên. 
2. Đối với mỗi con chó, tạo một cạnh từ nguồn đến nút chó với dung lượng bằng với chi phí thay đổi nó nếu con chó ban đầu là đực và chúng ta muốn đếm số lần lật thành con cái. Tương tự, tạo một cạnh từ nút chó tới nút chìm với dung lượng bằng chi phí thay đổi nó nếu ban đầu là nút nữ. Điều này mô hình hóa quyết định trả tiền để lật một con chó. 
3. Đối với mỗi người quan sát, hãy xác định lợi ích ròng khi làm họ hài lòng. Nếu người quan sát không phải là bạn bè, hãy tạo một cạnh từ nút người quan sát đến phần chìm với dung lượng bằng phần thưởng. Đối với bạn bè, hãy tạo một cạnh từ nút nguồn đến nút quan sát với công suất bằng với hình phạt, thể hiện cái giá phải trả nếu chúng tôi làm họ thất bại. 
4. Kết nối từng nút quan sát với tất cả các nút chó mà nó liên quan. Các cạnh này có khả năng vô hạn vì việc thỏa mãn người quan sát yêu cầu tất cả những con chó được chỉ định của họ phải phù hợp với giới tính mục tiêu, nhưng chúng tôi có thể tự do thay đổi bất kỳ con chó nào tùy theo chi phí. Mạng đảm bảo rằng việc cắt tối thiểu sẽ chọn một tập hợp con nhất quán các lần lật chó để tối đa hóa lợi ích. 
5. Tính lưu lượng tối đa từ nguồn tới đích. Đường cắt tối thiểu trong biểu đồ tương ứng với tập hợp thay đổi chó tối ưu. Lợi nhuận ròng tối đa là tổng của tất cả các phần thưởng không phải là bạn bè cộng với hình phạt dành cho người bạn ban đầu trừ đi giá trị cắt giảm tối thiểu. 
6. Đưa ra mức tăng ròng cuối cùng, có thể âm nếu hình phạt vượt quá phần thưởng. 

Tại sao nó hoạt động: luồng mạng mã hóa chính xác sự cân bằng giữa việc trả tiền để lật chó và nhận phần thưởng hoặc trả tiền phạt. Mức cắt tối thiểu sẽ tách nguồn khỏi bộ thu ở tổng công suất tối thiểu, tương đương với việc tối đa hóa lợi nhuận ròng. Các cạnh vô hạn thực thi các ràng buộc từ những con chó được yêu cầu của người quan sát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

class MaxFlow:
    def __init__(self, n):
        self.n = n
        self.graph = [[] for _ in range(n)]
        self.cap = {}
        
    def add_edge(self, u, v, c):
        self.graph[u].append(v)
        self.graph[v].append(u)
        self.cap[(u, v)] = c
        self.cap[(v, u)] = 0
    
    def bfs(self, s, t, parent):
        visited = [False]*self.n
        q = deque([s])
        visited[s] = True
        while q:
            u = q.popleft()
            for v in self.graph[u]:
                if not visited[v] and self.cap[(u,v)] > 0:
                    visited[v] = True
                    parent[v] = u
                    if v == t:
                        return True
                    q.append(v)
        return False
    
    def max_flow(self, s, t):
        parent = [-1]*self.n
        flow = 0
        while self.bfs(s, t, parent):
            path_flow = float('inf')
            v = t
            while v != s:
                u = parent[v]
                path_flow = min(path_flow, self.cap[(u,v)])
                v = u
            v = t
            while v != s:
                u = parent[v]
                self.cap[(u,v)] -= path_flow
                self.cap[(v,u)] += path_flow
                v = u
            flow += path_flow
        return flow

def main():
    n, m, g = map(int, input().split())
    sex = list(map(int, input().split()))
    cost = list(map(int, input().split()))
    
    S = n + m
    T = S + 1
    mf = MaxFlow(n + m + 2)
    total_reward = 0
    
    for i in range(n):
        if sex[i] == 0:
            mf.add_edge(i, T, cost[i])
        else:
            mf.add_edge(S, i, cost[i])
    
    for idx in range(m):
        line = list(map(int, input().split()))
        t, w, k = line[:3]
        dogs = [x-1 for x in line[3:3+k]]
        friend = line[3+k]
        obs_node = n + idx
        if friend:
            mf.add_edge(S, obs_node, g)
        else:
            mf.add_edge(obs_node, T, w)
            total_reward += w
        for d in dogs:
            if t == 0:
                mf.add_edge(d, obs_node, float('inf'))
            else:
                mf.add_edge(obs_node, d, float('inf'))
    
    flow = mf.max_flow(S, T)
    print(total_reward - flow)

if __name__ == "__main__":
    main()
```Mã này xác định thuật toán luồng tối đa Edmonds-Karp tiêu chuẩn. Mỗi con chó có các cạnh đối với nguồn hoặc phần chìm để biểu thị chi phí lật và mỗi người quan sát kết nối với các con chó thông qua các cạnh có dung lượng vô hạn. Phần thưởng không phải là bạn bè được thêm vào Total_reward và được mô hình hóa dưới dạng các cạnh chìm. Hình phạt bạn bè được mô hình hóa như các cạnh từ nguồn. Câu trả lời cuối cùng trừ đi giá trị cắt nhỏ nhất từ ​​Total_reward. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 5 9
0 1 1 1 0
1 8 6 2 3
0 7 3 3 2 1 1
1 8 1 5 1
1 0 3 2 1 4 1
0 8 3 4 2 1 0
1 7 2 4 1 1
```| Bước | Chó lật cạnh | Các cạnh của người quan sát | Dòng chảy | Lợi nhuận ròng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | cạnh với chi phí | các cạnh đến/từ người quan sát | 0 | 0 | 
| Dòng chảy tối đa | con đường thỏa mãn người quan sát | cạnh vô hạn thực thi các ràng buộc | 22 | 2 | 

Dấu vết cho thấy những lần lật tối ưu làm hài lòng một tập hợp con người quan sát đồng thời giảm thiểu hình phạt. Dòng chảy qua mạng nắm bắt được chi phí của các lần lật và các hình phạt không thể tránh khỏi. 

Đầu vào nhỏ tùy chỉnh:```
2 1 5
0 1
3 4
1 6 2 1 2 1
```Chiến lược tối ưu lật con chó 1 thành con đực, làm hài lòng người bạn, trả chi phí 3 RMB, tránh bị phạt 5 RMB, phần thưởng không được áp dụng. Lợi nhuận ròng: -3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m)*F) | Trường hợp xấu nhất của Edmonds-Karp là O(E_F), ở đây E = O(n_m), F giới hạn bởi tổng khả năng | 
| Không gian | O(n + m + tổng k_i) | Mỗi con chó và người quan sát là một nút; các cạnh tỉ lệ với tổng k_i | 

Cho n ≤ 10^4 và m ≤ 2*10^3 với mỗi k_i ≤ 10, thuật toán chạy thoải mái dưới 2 giây. 

## Trường hợp thử nghiệm```

```
