---
title: "CF 46F - Bài toán Hercule Poirot"
description: "Chúng ta được cấp một ngôi nhà có một số phòng nhất định nối với nhau bằng cửa và mỗi cửa có một chìa khóa riêng. Có một số cư dân trong nhà, ban đầu mỗi người ở một số phòng với một số chìa khóa. Chúng tôi cũng biết được chức vụ và tài sản chính của mỗi cư dân sau này."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 46
codeforces_index: "F"
codeforces_contest_name: "School Personal Contest #2 (Winter Computer School 2010/11) - Codeforces Beta Round 43 (ACM-ICPC Rules)"
rating: 2300
weight: 46
solve_time_s: 61
verified: true
draft: false
---

[CF 46F - Vấn đề Hercule Poirot](https://codeforces.com/problemset/problem/46/F) 

**Đánh giá:** 2300 
**Tags:** dsu, đồ thị 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một ngôi nhà có một số phòng nhất định nối với nhau bằng cửa và mỗi cửa có một chìa khóa riêng. Có một số cư dân trong nhà, ban đầu mỗi người ở một số phòng với một số chìa khóa. Chúng tôi cũng biết được chức vụ và tài sản chính của mỗi cư dân sau này. Câu hỏi đặt ra là liệu có thể thông qua một chuỗi các hành động pháp lý - mở cửa bằng chìa khóa, di chuyển giữa các phòng bằng cửa mở và trao đổi chìa khóa với những cư dân khác trong cùng phòng - để đạt được cấu hình thứ hai từ cấu hình thứ nhất hay không. Nếu có, chúng tôi in "CÓ"; nếu không thì "KHÔNG". 

Ngôi nhà có thể được mô hình hóa dưới dạng đồ thị: các phòng là các nút, các cửa là các cạnh. Cư dân di chuyển dọc theo các cạnh nếu cạnh được mở khóa bằng một số chìa khóa mà họ giữ hoặc lấy từ người khác. Mỗi cánh cửa có chính xác một chìa khóa ở đâu đó trong số các cư dân. Bất kỳ cánh cửa nào cũng có thể được mở và đóng nhiều lần và cư dân có thể chia sẻ chìa khóa khi ở cùng một phòng. Vấn đề thực sự là hỏi liệu phòng cuối cùng của mỗi cư dân có thể truy cập được từ phòng ban đầu của họ với những ràng buộc chia sẻ khóa này hay không. 

Các hạn chế là nhỏ:$n, m, k \le 1000$. Mô phỏng lực lượng vũ phu trong đó chúng tôi thử mọi hành động có thể có cho mỗi cư dân sẽ quá chậm vì số lượng chuỗi hành động tăng theo cấp số nhân. Tuy nhiên, các ràng buộc cho phép các phương thức truyền tải đồ thị như BFS hoặc DFS trên tối đa 1000 nút và cạnh. Các trường hợp biên bao gồm các tình huống ban đầu một cánh cửa bị cô lập vì không ai có chìa khóa trong cùng một bộ phận được kết nối hoặc tất cả cư dân ở trong một phòng nhưng cuối cùng phải ở trong các phòng bị ngắt kết nối - những cách tiếp cận ngây thơ cho rằng việc di chuyển tự do sẽ thất bại. 

Một ví dụ mà việc thực hiện bất cẩn không thành công là hai phòng thông nhau bằng một cánh cửa, mỗi phòng một người, chìa khóa cửa do người phòng đối diện giữ. Logic ngây thơ "mọi người đều có thể tiếp cận những người khác" sẽ nói không chính xác "CÓ", nhưng câu trả lời đúng là "KHÔNG" vì không ai có thể mở cửa để gặp hoặc trao đổi chìa khóa. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ cố gắng mô phỏng mọi trình tự có thể xảy ra của việc mở cửa, chuyển động của cư dân và trao đổi chìa khóa. Đối với mỗi bước thời gian, chúng tôi sẽ lặp lại từng cư dân và xem xét việc di chuyển qua mọi cánh cửa mà họ có thể mở. Mặc dù điều này sẽ tạo ra câu trả lời đúng, nhưng số lượng các khả năng có thể là vô cùng lớn, dễ dàng vượt quá$2^{1000}$trình tự trong trường hợp xấu nhất, làm cho nó không thực tế. 

Thông tin chi tiết quan trọng là mô hình hóa từng thành phần được kết nối của các phòng có thể được truy cập thông qua các khóa có sẵn cho cư dân trong thành phần đó. Tại bất kỳ thời điểm nào, các cư dân trong cùng một thành phần có thể chia sẻ khóa một cách tự do. Sau khi tất cả các khóa có trong một thành phần đã được truyền qua các phòng có thể truy cập, tất cả các phòng được kết nối bằng các phím đó sẽ tạo thành một "thành phần có thể truy cập" duy nhất. Đây chính xác là vấn đề mà Disjoint Set Union (DSU) hoặc Union-Find tỏa sáng: mỗi cánh cửa là cầu nối giữa các phòng và chìa khóa cần thiết cho một cánh cửa nằm ở đâu đó trong hệ thống. Bằng cách hợp nhất các phòng có thể được mở khóa bằng cách sử dụng bộ khóa hiện tại trong một thành phần, chúng tôi có thể xác định số lượng phòng tối đa mà mỗi nhóm cư dân có thể truy cập. Cuối cùng, nếu phòng mục tiêu của mỗi cư dân nằm trong khu vực có thể tiếp cận được của phòng ban đầu của họ thì cấu hình có thể đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k * 2^m) | O(n + m + k) | Quá chậm | 
| DSU/Tuyên truyền thành phần | O(m + n + k) | O(n + m + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào và lưu trữ biểu đồ các phòng và cửa ra vào. Đối với mỗi cánh cửa, hãy duy trì các điểm cuối của nó và liên kết nó với một khóa duy nhất. 
2. Theo dõi vị trí ban đầu của từng cư dân và chìa khóa họ nắm giữ. Đồng thời theo dõi các vị trí mục tiêu cho từng cư dân. Duy trì ánh xạ từ chìa khóa đến người cư trú ban đầu giữ nó. 
3. Khởi tạo cấu trúc Disjoint Set Union (DSU) để theo dõi các thành phần của phòng có thể truy cập bằng các phím hiện có. Ban đầu, mỗi phòng là thành phần riêng của nó. 
4. Đối với mỗi cư dân, hãy đánh dấu những cánh cửa mà họ hiện có thể mở. Đối với mỗi cánh cửa có thể mở được, hãy hợp nhất hai phòng mà nó kết nối trong DSU. Điều này thể hiện rằng ai đó có chìa khóa có thể di chuyển tự do giữa các phòng đó. 
5. Truyền lặp lại các khóa thông qua các thành phần. Đối với mỗi thành phần, hãy thu thập tất cả các khóa do cư dân bên trong thành phần nắm giữ. Nếu một cánh cửa kết nối hai phòng ở các bộ phận khác nhau nhưng chìa khóa của cánh cửa đó lại nằm ở một trong các bộ phận, hãy hợp nhất các bộ phận đó. Lặp lại điều này cho đến khi không thể hợp nhất mới được. 
6. Sau khi đường truyền ổn định, đối với mỗi cư dân, hãy kiểm tra xem phòng mục tiêu của họ có nằm trong cùng thành phần DSU với phòng ban đầu của họ hay không. Nếu tất cả cư dân đều thỏa mãn điều kiện này, hãy in "CÓ"; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: việc hợp nhất DSU đảm bảo rằng bất kỳ hai phòng nào trong cùng một thành phần đều có thể truy cập được lẫn nhau bằng cách sử dụng chìa khóa có sẵn cho cư dân trong thành phần đó. Việc truyền chìa khóa đảm bảo rằng ngay khi chìa khóa có thể truy cập được trong một số phòng, tất cả các cửa mà nó có thể mở đều được xem xét, mở rộng khu vực có thể tiếp cận. Vì cư dân có thể trao đổi khóa một cách tự do trong các thành phần nên điều này nắm bắt được mọi động thái hợp pháp. Nếu một phòng mục tiêu nằm ngoài thành phần thì không thể truy cập được do các ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root != y_root:
            self.parent[y_root] = x_root

def solve():
    n, m, k = map(int, input().split())
    doors = []
    for _ in range(m):
        u, v = map(int, input().split())
        doors.append((u-1, v-1))
    
    residents_init = {}
    residents_final = {}
    key_owner_init = [None] * m
    
    for i in range(k):
        parts = input().split()
        name = parts[0]
        room = int(parts[1]) - 1
        num_keys = int(parts[2])
        keys = [int(x)-1 for x in parts[3:]]
        residents_init[name] = (room, set(keys))
        for key in keys:
            key_owner_init[key] = name

    for i in range(k):
        parts = input().split()
        name = parts[0]
        room = int(parts[1]) - 1
        residents_final[name] = room

    dsu = DSU(n)
    
    changed = True
    while changed:
        changed = False
        component_keys = [set() for _ in range(n)]
        for name, (room, keys) in residents_init.items():
            root = dsu.find(room)
            component_keys[root].update(keys)
        for idx, (u, v) in enumerate(doors):
            ru, rv = dsu.find(u), dsu.find(v)
            if ru != rv:
                if idx in component_keys[ru] or idx in component_keys[rv]:
                    dsu.union(ru, rv)
                    changed = True
    
    possible = all(dsu.find(residents_init[name][0]) == dsu.find(residents_final[name]) for name in residents_init)
    print("YES" if possible else "NO")

solve()
```Mã duy trì một DSU để theo dõi các phòng có thể tiếp cận, thu thập tất cả chìa khóa trong từng thành phần và liên tục hợp nhất các thành phần khi có thể mở cửa bằng bất kỳ chìa khóa nào trong thành phần. các`changed`cờ đảm bảo việc truyền bá tiếp tục cho đến khi không thể hợp nhất được nữa. Kiểm tra cuối cùng xác minh rằng mỗi cư dân có thể tiếp cận phòng mục tiêu của họ. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
2 1 2
1 2
Dmitry 1 1 1
Natalia 2 0
Natalia 1 1 1
Dmitry 2 0
```| Thường trú | Phòng ban đầu | Phím | Thành phần DSU sau khi nhân giống | Phòng cuối cùng | Có thể truy cập được? | 
| --- | --- | --- | --- | --- | --- | 
| Dmitry | 0 | {0} | 0 và 1 hợp nhất | 1 | Có | 
| Natalia | 1 | {} | 0 và 1 hợp nhất | 0 | Có | 

DSU hợp nhất phòng 0 và phòng 1 vì Dmitry giữ chìa khóa cánh cửa duy nhất. Cả hai cư dân có thể trao đổi chìa khóa và di chuyển đến bất kỳ phòng nào. Đầu ra:`YES`. 

**Ví dụ tùy chỉnh 2:** 

đầu vào:```
3 2 2
1 2
2 3
Alice 0 1 0
Bob 2 1 1
Alice 2 1 0
Bob 0 1 1
```Alice vào phòng 0 bằng chìa khóa 0, Bob vào phòng 2 bằng chìa khóa 1. Chìa khóa 0 mở cửa 0 (1-2), chìa khóa 1 mở cửa 1 (2-3). DSU ban đầu: 

- Hợp nhất phòng 0 và 1 bằng chìa khóa của Alice 
- Hợp nhất phòng 1 và 2 bằng chìa khóa của Bob 

Tất cả các phòng trong một thành phần. Cả hai cư dân đều có thể tiếp cận các phòng mục tiêu. Đầu ra:`YES`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + k) | Mỗi hoạt động liên kết/tìm kiếm DSU gần như không đổi với việc nén đường dẫn. Lặp lại qua các cửa để truyền lan tối đa là O(m * α(n)) trong đó α là nghịch đảo của Ackermann. | 
| Không gian | O(n + m + k) | Mảng cha DSU, bộ khóa cho mỗi thành phần, thông tin thường trú. | 

Cho n, m, k ≤
